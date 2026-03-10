# 2.5 - MongoDB Exercise

## 🎯 Objectives

1. **Understand** the fundamental concepts of NoSQL databases and MongoDB's document model.
2. **Explain** the role of MongoDB in modern web development and how it integrates with Node.js applications.
3. **Implement** a basic Node.js server that uses MongoDB to store and retrieve data.
4. **Set up** MongoDB on the docker..
5. **Perform** basic CRUD (Create, Read, Update, Delete) operations with MongoDB from a Node.js app.

## 🔨 Setup

1. Open your terminal and navigate to your `~/web-ii/exercises/` directory.
1. Go to the [repository for this exercise](https://github.com/JAC-CS-Web-Programming-II-W26/E2.5-MongoDB-Template) and click `Code -> 📋` to copy the URL:

```bash
git clone <paste URL here>
```

3. Clone the Git repo from the CLI `git clone <paste URL from GitHub>` (without the angle brackets) or using a GUI client like [GitHub Desktop](https://desktop.github.com/).
   - You may have to use the `HTTPS` or `SSH` URL to clone depending on your settings. If one doesn’t work, try the other by clicking `Use SSH` or `Use HTTPS` above the 📋, and copy the new URL.
4. Rename the cloned folder to `~/web-ii/exercises/2.5-mongodb/`.

5. Start Docker Desktop.

6. Open VS Code and install necessary dependencies by running:

```bash
npm install
```

## 🔍 Context

In E1.3, we had the Docker to spin up two containers. One for running our application code, and one for running our database server. We used the PSQL client to connect to the database and execute SQL commands.

In this exercise, we’re going to do the same thing, but with MongoDB databse.  The first thing to do is open `model.ts` and notice at the top is a module we’re importing: [mongodb](https://github.com/porsager/postgres). This library is going to allow us to connect to the MongoDB database we’re running in our Docker container.

For review, check out the [MongoDB Notes](../Notes/Week7/mongodb) before starting this exercise. We will be using the Express router from the previous exercise to route the endpoints.

## 🚦 Let’s Go

### Part 1: Creating a Database and Collection

1. In the `model.ts`, initialize the MongoDB database.

```typescript
export async function initDB() {
	try {
		const url = `mongodb://${process.env.MONGO_USER}:${process.env.MONGO_PWD}@${process.env.MONGO_HOST}/`;

		 // Only create a client if it doesn't exist
        if (!client) {
            client = new MongoClient(url);
            await client.connect();
            console.log("Connected to MongoDB");
        }

		const db = client.db(DATABASE_NAME);

	} catch (err) {
		if (err instanceof MongoError) {
			console.error("MongoDB connection failed:", err.message);
		} else {
			console.error("Unexpected error:", err);
		}
        throw err;
	}
}
```

2. Now we will create a collection for the Pokemons if it doesn't exist.In the `try` section , after the db is initialized. Add the following code.

```typescript
       // Get the list of existing collections in the database
		const collections = await db.listCollections().toArray();
		const collectionNames = collections.map((col) => col.name);

		// Create "pokemon" collection if it does not exist
		if (!collectionNames.includes("pokemon")) {
			console.log('Creating "pokemon" collection...');
			await db.createCollection("pokemon");
		}

		// Reference to the "pokemon" collection
		pokemonCollection = db.collection("pokemon");   
```

3. Now that we have a collection , we will add the initial document if doesn't already exist. This step can be omitted or data could be added directly using curl.

```typescript
         // Check if the collection is empty and insert initial data if needed
   		const userCount = await pokemonCollection.countDocuments();
   
   		if (userCount === 0) {
   			console.log("Inserting initial pokemons...");
   			await pokemonCollection.insertMany(database);
   		}
```
4. Update the getAll function to return the document.

```typescript
export async function getAll(): Promise<Pokemon[]> {
	if (!pokemonCollection) {
		throw new Error("Database not initialized. Call initDB() first.");
	}
	return await pokemonCollection.find().toArray();
}
```
-----
### Part 2: Updating the Controller to access the Mongodb database.   

1. Import the function  `getAll` from `model.ts` to access data from the database.

```typescript
   import {getAll} from "./model";
```
2. Now update the `getAllPokemon` function in the `controller.ts` file.

>[!NOTE] **Cursor**
>In Node, when you make certain calls to the MongoDb that can return multiple pieces of information, you don't get back the actual data. 
>Instead, you get back a special object called a [Cursor](https://www.mongodb.com/docs/drivers/node/current/fundamentals/crud/read-operations/cursor/).
>
>The purpose of a cursor is to avoid sending a very large amount of data all at once, and instead allow the app to iterate over it as appropriate.
>***Recall: NoSQL databases can be enormous.***
>In this class, we will convert the cursor to an array (The real world uses a different approach. ).  
>This will force all the data to be loaded into a JavaScript array.
>If **cursor** is the result of a `collection.find()` call, then `cursor.toArray()` will give you a JavaScript array of all the results.
>**Important:** This toArray() operation is asynchronous since it involves getting data from the online database.


```typescript
const pokemon = await getAll();
```
3. Send appropriate response back to server 

```typescript
			
		res.status(200).json({
			success: "Pokemon Collection Found",
			pokemon,
			});

```



4. Then add a try-catch block to capture an internal Server error

```typescript
  try {
		const pokemon = await getAll(); // Get collection

		res.status(200).json({
			success: true,
			pokemon,
		});
	} catch (error) {
		res.status(500).json({
			success: false,
			message: "Internal server error",
		});
	}
```
----
### Part 3: Updating the Server engine with express router.

1. We will update the `servet.ts` to initialize the Mongodb database by importing the `initdb` function from `model.ts`

```typescript
   import { initDB } from "./model";
   ...
   
   async function startServer() {
	 try {
		await initDB(); // Wait for MongoDB connection
		console.log(" Database initialized");
		app.listen(port, () => {
			//The server (app.listen) does not start until the DB connection is successful.
			console.log(`Server running at http://localhost:${port}/`);
		});
	 } catch (err) {
		console.error(" Failed to start server:", err);
		process.exit(1); // Exit if DB fails
	 }
    }

   startServer();
```
   - Start the server by running `npm run server`
   - Try it!  On a separate Terminal window

```bash
    curl -v http://localhost:3000/pokemon
```

----
### Part 4: Expanding our Pokemon API
1. **GET One Pokemon**
 **model.ts**
    Update `getOne` function in `model.ts`. Pay attention to the Promise return type. Look up the findOne [method](https://www.mongodb.com/docs/manual/reference/method/) to understand the query. The id which is returned as a string from the request is converted to the MongoDB object. `ObjectId` is function in MongoDB that is already imported in the `model.ts` file.

  ```typescript
  export async function getOne(id: string): Promise<Pokemon | null> {
      //check first of pokemonCollection exist , through appropriate error
  	
  	return await pokemonCollection?.findOne({ _id: new ObjectId(id) });
  }
  ```


**controller.ts**

Update the `getOnePokemon` function to call the `getOne` function from the `model.ts`. Return appropriate server responses. Use Try - catch block to have clear server responses.

```typescript
const pokemonId = req.params.id; // Extract ID from request parameters

const foundPokemon = await getOne(pokemonId); 
```
>[!NOTE]
>Since MongoDB serializes the ID, you will need to run "Get All Pokemon" to retrieve the ID required for "Get One," "Update," and "Delete" operations.


2. **POST a New Pokemon**
 **model.ts**
    Update `addOne` function in `model.ts` Look up the insertOne [method](https://www.mongodb.com/docs/manual/reference/method/) to understand the query
```typescript
await pokemonCollection?.insertOne(pokemon);
```
**controller.ts**

Update the `createPokemon` function to call the `addOne` function from the `model.ts`. Return appropriate server responses. Use Try - catch block to have clear server responses.

```typescript
addOne(newPokemon);
```

3. **Update and delete.**

   Complete update and delete a pokemon on your own
>[!TIP]
>You will need findOneAndUpdate Collection method to update.
>You will need deleteOne Collection method to delete.

-----

### Part 5: cURL Tests.
Open a separate terminal window. We’ll use this one to make cURL requests and see the server responses.

Using cURL, to  test all your routes to ensure everything is working so far. For example

```bash
curl -v http://localhost:3000/pokemon

curl -v http://localhost:3000/pokemon/id_from_mongodb

curl -v -X POST -H "Content-Type: application/json" -d '{"name": "Bulbasaur", "type": "Grass"}' http://localhost:3000/pokemon

curl -v -X PUT -H "Content-Type: application/json" -d '{"type": "Poison"}' http://localhost:3000/pokemon/id_from_mongodb

curl -v -X DELETE http://localhost:3000/pokemon/id_from_mongodb
```
----

### Part 6: Unit Tests (Optional)

- Write a unit test for your addOne function in a file called model.test.ts

- You'll need to use appropriate functions in beforeEach and afterEach to manage your database state connection

- Add error handling, logging and documentation as appropriate



## 📚 Submission

1. Perform the following cURL operations without the `-v` flag:

   1. `POST` a new `/pokemon`.
   2. `GET` the `/pokemon/:_id` that was inserted.
   3. `UPDATE` the `/pokemon/:_id` to have a different name.
   4. `GET` all the `/pokemon` to verify the name was changed.
   5. `DELETE` the `/pokemon/:_id`
   <br>Would be good to do the next two as well (Optional for submission)
   7. `GET` all `/pokemon?type=Water` to get only Water Pokemon.
   8. `GET` all `/pokemon?sortBy=name` to get all Pokemon sorted alphabetically by name.

Take a screenshot (or several if they all don’t fit in one) of the output so that I can see **all statements were run successfully**. Submit the screenshot you took to the Moodle dropbox for this exercise.
