# 1 - Models

- 💯 **Worth**: 7%
- 📅 **Due**: February 16, 2026 @ 02:00 am
- 🚫 **Penalty**: Late submissions lose 10% per day to a maximum of 3 days. Nothing is accepted after 3 days and a grade of 0% will be given.

## 🎯 Objectives

- **Understand** the concept of the Model in MVC architecture by implementing CRUD operations.
- **Use** TypeScript to define and manipulate data structures for a Todo application.
- **Examine** the provided Todo class skeleton and determine how to implement the required methods effectively.
- **Test** the implemented methods using the provided Jest test suite to ensure correctness.
- **Develop** a fully functional model layer for a Todo application that interacts with a database.

## 🔨 Setup

1. [Click here](https://classroom.github.com/a/VnbgAC1m) to join the Git classroom.
2. Clone (do not download as a zip) the starter repository from GitHub.
3. Make sure Docker Desktop is open.
4. Start the development container in VS Code by using the `Dev Containers: Open Folder in Container...` command from the Command Palette (CTRL/CMD+SHIFT+P) and select the cloned directory.
5. Run the following command in the terminal to install all necessary dependencies: `npm install`.
6. Verify that the database was set up properly by running:
   1. `psql` to connect to the database server.
   2. `\c TodoDB` to connect to the database.  
   3. `\dt` to see the list of tables. There should be one called `todos`.

> [!Note]
>
> If the database does not get set up properly when you first build your container, you can run `psql -f .devcontainer/init.sql` (making sure you’re inside the container, i.e. the bottom left of VSC should say “Dev Container”) to manually run the initialization script.



## 🔍 Context

To complete this assignment, you should be familiar with the following concepts and theories:

- **[MVC Architecture](Notes/Week2/mvc.md)**: Understanding the role of the Model in the MVC (Model-View-Controller) pattern.
- **[TypeScript](Notes/Week2/14-typescript.md)**: Knowledge of basic TypeScript syntax and type system.
- **[Database Interaction](Notes/Week2/12-docker.md)**: How to interact with a database using SQL.
- **[CRUD Operations](Notes/Week2/13-postgresql.md)**: How to create, read, update, and delete data in a persistent storage system.
- **[Testing with Jest](Guides/testing)**: Familiarity with Jest and how to run tests for TypeScript applications.

>[!CAUTION]
>
>If you’re not comfortable with any of the above, **please send me a message on Teams** and I can help you! It is your responsibility to ensure you have learned all concepts we’ve seen so far so that you can do the assignment effectively.



## 🚦 Let’s Go

Your task is to create a todo application that keeps track of a user’s task list. You will accomplish this by implementing the CRUD methods in the provided Todo.ts class skeleton.

## Todos (85%)
>[!TIP]
>**Read the Comments**
>I have left detailed comments in Todo.ts and Todo.test.ts. Please read them and then re-read them to get a solid understanding about what is expected from you.
>


Todo.ts

```ts
function create(sql: postgres.Sql<any>, props: TodoProps): Promise<Todo> {}
function read(sql: postgres.Sql<any>, id: number): Promise<Todo | null> {}
function update(updateProps: Partial<TodoProps>): Promise<void> {}
function delete(): Promise<void> {}
function markComplete(): Promise<void>{}
```



![CRUD](../images/CRUD.png)

While implementing the methods, refer to the Jest test suite (./tests/Todo.test.ts) to guide your development process. Make sure all tests pass before considering the assignment complete.

> [!WARNING] 
>
> **Tests are a Guide**
>
> Passing all the tests is not an indicator for obtaining 100%. Granted, when you do pass all the tests it’s definitely a good sign. However, the tests are provided as an aide for you to guide your development.

## SubTodos (15%)
For a challenge, you can implement a subtodo feature. A subtodo is a smaller task that is part of a larger todo item. For example, if you have a todo item to “Plan a party”, you might have subtodos like “Send invitations”, “Buy decorations”, and “Plan the menu”.

- You will need to create a new SubTodo class and infer the method signatures from the test suite.

- A SubTodo is a stripped down version of a regular Todo. A SubTodo should not have a description, editedAt, or dueAt props.

- Each SubTodo should be associated with a Todo item. This means you will also need to modify your TodoProps interface to include an array of SubTodo items.

- Remember to update your database schema to include a new table for the subtodos.

- There are subtodo tests in the test suite that you should uncomment.

## 📥 Submission
To submit your assignment, follow these steps:

1. Commit your changes to the local repository:

   ```cmd
    git add .
    git commit -m "Completed Todo model implementation."
    
   ```




2. Push your commits to the remote repository:

   ```cmd
   git push
   ```

3. Submit your assignment on Gradescope.

	1. Accept the invitation to Gradescope in your [JAC email](https://outlook.com/) using your `studentid@johnabbottcollege.net` address.
	2. Go to Gradescope and click the link for this [assignment](https://www.gradescope.ca/courses/35557/assignments/180550) 
	3. Select the correct repository and branch from the dropdown menus.
	4. Click *Upload*.

