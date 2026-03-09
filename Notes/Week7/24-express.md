# 2.4 - Express Exercise

## 🎯 Objectives

1. **Understand** how Express simplifies HTTP server creation in Node.js.
2. **Learn** how to set up an Express application with TypeScript.
3. **Implement** route handling for different HTTP methods (GET, POST, PUT, DELETE).
4. **Use** middleware for request parsing and logging.
5. **Structure** a small Express API using TypeScript best practices.

## 🔨 Setup

1. Navigate to your project folder:

   ```bash
   cd ~/web-ii/exercises/
   ```

2. Go to [the repository for this exercise](https://github.com/JAC-CS-Web-Programming-II-W26/E2.4-Express-Template) and click `Code -> 📋` to copy the URL.

   ```bash
   git clone <paste URL from GitHub>
   ```

3. Clone the Git repo from the CLI `git clone <paste URL from GitHub>` (without the angle brackets) or using a GUI client like [GitHub Desktop](https://desktop.github.com/).

   - You may have to use the `HTTPS` or `SSH` URL to clone depending on your settings. If one doesn’t work, try the other by clicking `Use SSH` or `Use HTTPS` above the 📋, and copy the new URL.

4. Rename the cloned folder to `~/web-ii/exercises/2.4-express/`

5. Start Docker Desktop.

6. In VS Code, hit `CMD/CTRL + SHIFT + P` and search + run `dev container: open folder in container`.

7. In the terminal of VS Code, hit the `+` icon to open a new terminal instance. Run `ls` to make sure you’re in the root directory of the exercise and that you see `package.json`.

8. Run `npm install` to install all our dependencies.

## 🔍 Context

We previously explored the fundamentals of HTTP And router using Node.js's `http` module. Now, we will use **[Express](https://expressjs.com/)**, a minimal framework that simplifies server creation and request handling.

Express Router:

- Helps organize routes into separate files for better maintainability.
- Makes the codebase cleaner and easier to manage.
- Enables modular route handling, which is useful for large applications.

## 🚦 Let’s Go

### Part 1: Setting Up the Express Server

We will separate the server starter from all routing information.

1. Create a new file `server.ts`  in the `src` folder.This starts the server and initializes the express router.This file serves as the entry point of the application, setting up routes, middleware, and starting the server.

   `app.use(express.json())`: This is built-in Middleware provided by Express.It parses incoming JSON requests, allowing the server to handle JSON payloads.

   > [!NOTE]
   >
   > Middleware in Express.js is a function that sits between the request and the response. It processes incoming requests before they reach the final route handler.
   >
   > Middleware functions can:
   >
   > - **Modify the request (`req`) or response (`res`) objects**
   > - **Execute code before passing control to the next middleware**
   > - **End the request-response cycle**
   > - **Call the next middleware function in the stack**

   `app.use("/pokemon", pokemonRouter)`: Mounts the `pokemonRouter` on the `/pokemon` route, meaning all requests starting with `/pokemon` will be handled by the router.

   ```typescript
   import express from "express";
   import pokemonRouter from "./router";
   
   
   const app = express();
   const port = 3000;
   
   // Middleware to parse incoming JSON requests
   app.use(express.json());
   
   /**
    * Home route
    * Responds with a welcome message when the root URL is accessed.
    */
   app.get("/", (req, res) => {
   	res.status(200).json({ message: "Hello from the Pokemon Server!" });
   });
   
   app.use("/pokemon", pokemonRouter);
   /**
    * Starts the Express server and listens on the specified port.
    */
   app.listen(port, () => {
     console.log(`Server running at http://localhost:${port}/`);
   });
   ```

2. Start the server by running:

   ```bash
    npm run server
   ```

3. Test with cURL:

   ```bash
   curl -X GET http://localhost:3000/
   ```

### Part 2: Route Data Structures & Registration

1. In the router.ts define your route definitions for the endpoints and delegates request handling to corresponding controller functions :

   ```ts
   import { Router } from "express";
   import {
     createPokemon,
     deletePokemon,
     getAllPokemon,
     getOnePokemon,
     updatePokemon,
   } from "./controller";
   
   const pokemonRouter: Router = Router();
   
   pokemonRouter.get("/", getAllPokemon);
   
   pokemonRouter.get("/:id", getOnePokemon);
   
   pokemonRouter.post("/", createPokemon);
   
   pokemonRouter.put("/:id", updatePokemon);
   
   pokemonRouter.delete("/:id", deletePokemon);
   
   export default pokemonRouter;
   ```

### Part 2: Updating the Controller to work with express

The http module is replaced with express. Most of the code from the previous exercise remains the same. Pay attention to the express responses.

- You dont need the req.on and req.end as this is automatically handled by express.
- Express automatically handles request body parsing when using express.json().
- Directly accesses req.body instead of manually collecting chunks.

```typescript
  import { Request, Response } from "express";
```
Consider using 
- `req.body`, dont have to convert to string, becuase the middleware does it for you .

- `req.params.id` to extract the id, 

-  `req.query` to extract filters example: 
	
```typescript
	const { type, sortBy, order } = req.query;
```

-  You don't need to use `res.send` nor set the headers. Please make time to read more about the **Request** and **Response** components of Express. For example for getAll
   
```typescript
    res.status(200).json({ message: "All Pokemon", payload: newDatabase });
```


---

### **Testing**

Using cURL, test all your routes to ensure everything is working so far. For example::

```bash
curl -v http://localhost:3000/pokemon

curl -v http://localhost:3000/pokemon/1

curl -v -X POST -H "Content-Type: application/json" -d '{"name": "Bulbasaur", "type": "Grass"}' http://localhost:3000/pokemon

curl -v -X PUT -H "Content-Type: application/json" -d '{"type": "Poison"}' http://localhost:3000/pokemon/2

curl -v -X DELETE http://localhost:3000/pokemon/1

```

## Conclusion

- Express provides a simpler and more scalable way to build HTTP servers compared to the `http` module.

- Middleware like `express.json()` makes request parsing easier.

## 📚 Submission

1. Perform the following cURL operations without the `-v` flag:
   1. `POST` a new `/pokemon`.
   2. `GET` the `/pokemon/2` that was inserted.
   3. `UPDATE` the `/pokemon/2` to have a different name.
   4. `GET` all the `/pokemon` to verify the name was changed.
   5. `DELETE` the `/pokemon/2`.
   6. `GET` all `/pokemon?type=Water` to get only Water Pokemon.
   7. `GET` all `/pokemon?sortBy=name` to get all Pokemon sorted alphabetically by name.

Take a screenshot (or several if they all don’t fit in one) of the output so that I can see **all statements were run successfully**. Submit the screenshot you took to the Moodle dropbox for this exercise.
