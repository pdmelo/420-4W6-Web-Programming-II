# 4 - Auth

- 💯 **Worth**: to a portion of your Final Project%

- 📅 **Due**: With your Final project, 2026

## 🎯 Objectives

- **Implement** a registration system using emails and passwords for user account creation.
- **Utilize** sessions and cookies to manage user login and logout flows.
- **Construct** user interface elements (login and registration forms) that interact with authentication logic.
- **Protect** specific app routes by requiring user authentication.
- **Enhance** the app by adding user profile management features or an admin role with elevated permissions.

## 🔨 Setup

1. [Click here](https://github.com/JAC-CS-Web-Programming-II-W26/assignment-4-auth) to get the template.
2. Make sure Docker Desktop is open.
3. Start the development container in VS Code by using the `Dev Containers: Open Folder in Container...` command from the Command Palette (Ctrl or ⌘ + SHIFT + P) and select the cloned directory.
4. Verify that the database was set up properly by running:
   1. `psql -d TodoDB` to connect to the database server.
   2. `\dt` to see the list of tables. There should be `users, ` `todos` , and `subtodos`.
5. Start the server `npm run server` in a debug terminal so you’re able to use breakpoints in your code.
6. Start the client `npm run dev` in a debug terminal so you’re able to use breakpoints in your code.'
7. Keep both the terminals open.

> [!note]
>
> If the database does not get set up properly when you first build your container, you can run `psql -f .devcontainer/init.sql` (making sure you’re inside the container, i.e. the bottom left of VS Code should say “Dev Container”) to manually run the initialization script.

## 🔍 Context

To complete this assignment, you should be familiar with the following concepts and theories:

- **[Cookies](https://pdmelo.github.io/420-4W6-Web-Programming-II/#/Notes/Week11/cookies)**: How to set and read cookies in a web application.
- **[Sessions](https://pdmelo.github.io/420-4W6-Web-Programming-II/#/Notes/Week11/43-sessions)**: How to manage sessions in a web application.

> [!note]
>
> If you’re not comfortable with any of the above, **please send me a message on Teams** and I can help you! It is your responsibility to ensure you have learned all concepts we’ve seen so far so that you can do the assignment effectively.

In this assignment, you will be implementing user authentication in the Todo application. This will involve creating a `User` model class, a `UserController` class, and an `AuthController` class. The `User` model will represent a user in the system, the `UserController` will handle user-related HTTP requests, and the `AuthController` will handle authentication-related HTTP requests such as logging in and logging out of the system.

> [!important]
>
> Starter Code
>
> There is a minimal solution from last assignment to get you started. Many of the features and error handling are not implemented. It is advised to copy over as much code as you can from your previous assignment to save time. The minimal solution is there for those who perhaps didn’t complete the last assignment or are having trouble with the concepts.

### Auth Classes

In the 4.X exercises, we learned about cookies/sessions and how to manage them using relatively simple mechanisms. In this assignment, like in previous assignments, the mechanisms are more sophisticated and abstracted away from you. You will be using the following classes to manage cookies and sessions:

1. `Cookie.ts`: Represents a cookie in the system. It has a `name`, `value`, and `options` object that can be used to set the cookie’s `path`, `expires`, `maxAge`, etc.

2. `Session.ts`: Represents a session in the system. It has a `sessionId` and a `data` object that can be used to store session data.

3. `SessionManager.ts`: Manages sessions in the system. It has a `sessions` object that maps session IDs to session objects. It has methods to create a new session, get a session by ID, and delete expired sessions.

4. `Request.ts`: Has been modified to include a `cookies` array and a `session` object. The `cookies` array is populated with cookies from the request headers, and the `session` object is populated with any session data from previous requests.

   `Request.ts`

```diff
  constructor(req: IncomingMessage) {
    this.req = req;
+   this.cookies = this.getCookies();
+   this.session = this.getSession();
  }
```

- `getCookies()`: Parses the `Cookie` header from the request and returns an array of `Cookie` objects.
- `findCookie()`: Finds a cookie in the `cookies` array by name.
- `getSession()`: Finds the session ID in the `Cookie` header and returns the session object from the `SessionManager` if one exists. If one does not exist, a new session is created.

5. `Response.ts`: Has been modified to include a `cookies` array. The `cookies` array is used to store cookies that will be sent in the response headers.

   `Response.ts`

```diff
  constructor (
    public req: Request,
    public res: ServerResponse,
+    public cookies: Cookie[] = [],
  ) {}

+   public setCookie(cookie: Cookie) { ... }
```

- `setCookie()`: Sets a cookie in the response. Every time this method is called, the `Set-Cookie` header is updated with the new cookie (and all cookies that were added before it, if any), and the new cookie is added to the `cookies` array.

## 🚦 Let’s Go

> [!note]
>
> **Read the Comments**
>
> I have left detailed comments in all the classes. Please read them and then re-read them to get a solid understanding about what is expected from you. The comments are part of the assignment instructions to help you know what to do. It’s up to you to (most importantly for this assignment) understand the relationship between the controller, request, response, and authentication classes.

### Part 1: User Model (20%)

**Goal**: Implement the `User` model class which will represent a user in the system.

1. Create
   - The `User` class should have a `create` method that will insert a new user into the database.

   - The database table for users has already been created for you. You can find the schema in `init.sql`.

   - If the email already exists, throw a `DuplicateEmailError`.

> [!caution]
>
> **Models**
>
> At this point, you should be comfortable with making models to represent entities of your application, so I won’t be providing a lot of guidance on this. If you’re unsure about how to do this, please refer back to the previous assignments where we created models for the Todo and SubTodo entities.

2. Login
   - The `User` model should have a `login` method that will check if the email and password match a row in the users database table.

   - If the email and password match, the method should return a new `User` object. Similar to reading a single Todo.

   - If the email and password do not match, we throw an `InvalidCredentialsError`.

3. Testing
   - Find the tests for the `User` model in `user.model.test.ts`.

   - Implement the logic required to pass the tests for the `create` and `login` methods.

   - The commented out tests are for the extra feature in part 5. You can ignore them for now.

### Part 2: Registration (20%)

**Goal**: Implement the `UserController` and `AuthController` classes which will handle user-related HTTP requests.

#### Controller

1. Just like creating a Todo, the `UserController` should have a `createUser` method that will handle the POST request to `/users`.
2. Upon form submission, this controller method should validate that no fields are blank/missing, that the passwords match, and that there isn’t already a user with the given email. If there are any errors, redirect back to the registration form with an error message.

#### View

1. In the client folder you have `Register.tsx`component which had the for for user to register.
2. The form should have fields for `email`, `password`, and `confirmPassword`.
3. The form should have a submit button that will send a POST request to `/users`.
4. Have an area to display any error messages that are passed in the query params.

User registration

<div style="position:relative; width:100%; height:0px; padding-bottom:62.500%;">
	<iframe allow="fullscreen;autoplay" allowfullscreen height="100%" src="https://pdmelo.github.io/420-4W6-Web-Programming-II/Assignments/images/A4-register.mp4" width="100%" style="border:none; width:100%; height:100%; position:absolute; left:0px; top:0px; overflow:hidden; border-radius: 5px; ">
	</iframe>
</div>

User already exist

<div style="position:relative; width:100%; height:0px; padding-bottom:62.500%;">
	<iframe allow="fullscreen;autoplay" allowfullscreen height="100%" src="https://pdmelo.github.io/420-4W6-Web-Programming-II/Assignments/images/A4-register-duplicate.mp4" width="100%" style="border:none; width:100%; height:100%; position:absolute; left:0px; top:0px; overflow:hidden; border-radius: 5px; ">
	</iframe>
</div>

### Part 3: Login/Logout (20%)

**Goal**: Implement the `AuthController` class which will handle authentication-related HTTP requests.

#### Controller

1. The `AuthController` should have a `login` method that will log the user in when making a POST request to `/login` by calling the `login` method on the `User` model.

2. If the login is successful, the server should **set a session parameter** for the user and redirect them to the `/todos` page (the redirection is done is react on successful login), along with a **session cookie** to remember that the user is logged in.

> [!important]
>
> **E4.3 - Sessions**
>
> It’s crucial that you understand how sessions work to do this assignment. If you did not complete the 4.X exercises, please go back and do them now. Refer to 4.3 in particular to understand how to set a session parameter and a session cookie, and re-read the context section above that explain the 3 auth classes you need to understand and use.

3. If the login is unsuccessful, send a respond of `BadRequest` with an error message `"Invalid credentials."` using the same query param technique outlined in the tip above.

4. The `AuthController` should have a `logout` method that will log the user out of the system when making a POST request to `/logout`.

5. This should clear the session.

#### View

1. The `react application` should have a `login` component that will render a login form view when making a POST request to `/login`.
2. The form should have fields for `email` and `password`.
3. The form should have a submit button that will send a POST request to `/login`.
4. Upon submission, the validate that no fields are blank/missing, and that the email and password match a user in the database. If there are any errors, display error message to the user.
5. The form should also have a checkbox for “Remember Me”. If this checkbox is checked when the form is submitted, the server should set a cookie to remember the user’s email. When the user logs out and visits the login page again, the email field should be pre-filled with the value of this cookie.**(OPTIONAL)**

<div style="position:relative; width:100%; height:0px; padding-bottom:62.500%;">
	<iframe allow="fullscreen;autoplay" allowfullscreen height="100%" src="https://pdmelo.github.io/420-4W6-Web-Programming-II/Assignments/images/A4-login.mp4" width="100%" style="border:none; width:100%; height:100%; position:absolute; left:0px; top:0px; overflow:hidden; border-radius: 5px; ">
	</iframe>
</div>

### Part 4: Todo Authentication (40%)

**Goal**: Update `TodoController.ts` to ensure that only authenticated users can access the Todo-related routes.

1. You’ll need to update the Todos table/model to accept a `user_id`/`userId` parameter, respectively, for the `todos.model.test.ts` to pass.

2. In each Todo controller method, you should check if the user is authenticated before proceeding. If the user is not authenticated, redirect them to the login page. Now that we have sessions, we can check if the user is authenticated by checking if the session has a `userId` parameter. If it does, the user is authenticated. If it doesn’t, they are not authenticated.

3. Since a Todo can only be created by an authenticated user, you should also update the `createTodo` method in the `TodoController` to set the `userId` parameter of the Todo to the `userId` parameter of the session. Also, ensure that a Todo can only be updated/deleted by the user who created it. To achieve this, notice the `todos` database table now has a `userId` column to signify that each todo belongs to a user, and the `TodoProps` interface has been updated to reflect this.

<div style="position:relative; width:100%; height:0px; padding-bottom:62.500%;">
	<iframe allow="fullscreen;autoplay" allowfullscreen height="100%" src="https://pdmelo.github.io/420-4W6-Web-Programming-II/Assignments/images/A4-authentication.mp4" width="100%" style="border:none; width:100%; height:100%; position:absolute; left:0px; top:0px; overflow:hidden; border-radius: 5px; ">
	</iframe>
</div>

#### View

1. Ensure on the client side, that no endpoint bypass the login.
1. Look at all the components and the router in the App.tsx , you want only authenticated and authorised users to have access to them.

## 💡 Tests & Tips

> [!note]
>
> Tests are a Guide
>
> Passing all the tests is **not an indicator for obtaining 100%**. Granted, when you do pass all the tests it’s definitely a good sign. However, the tests are provided as an aide for you to guide your development.

- The tests will give you an idea about what I’ll be looking for, but I haven’t written tests for every single case. You should be testing your code manually as well. Here are some non-exhaustive examples of things you should be testing:
  - **400 Bad Request**: If a user tries to create an entity with a blank field, for example, you should return a 400 Bad Request status code and an error message.
  - **404 Not Found**: If a user tries to perform an action on an entity that doesn’t exist, you should return a 404 Not Found status code.
  - **401 Unauthorized**: If an unauthenticated user tries to perform an action on an entity that they need to be authenticated for, you should return a 401 Unauthorized status code.
  - **403 Forbidden**: If an authenticated user tries to perform an action on an entity that they didn’t create, you should return a 403 Forbidden status code.

- Ensure the database is being affected how you think it should be by pausing on a breakpoint in your code and running a select statement on the database using `psql`.

**In fact, I advise having the following terminals open:**

1. `psql`
2. `npm run dev`
3. `npm run server`
4. `npm run test` (debug terminal)

- Follow the steps outlined above and run the tests according to which feature you’re currently working on. Remember to **not run all the tests**, but instead, run the tests for the feature you’re currently working on. For non-Playwright tests, stick a `test.only` in the test you’re working on to run only that test.
  - `npm run test -- user.model` to run only the tests for the `User` model.

  - `npm run test -- todo.model` to run only the tests for the `Todo` model.

  - `npm run test -- user.http` to run only the tests for the `User` controller.

  - `npm run test -- todo.http` to run only the tests for the `Todo` controller.

- Does the test time out before you’re done debugging? Increase the timeout time inside `playwright.config.ts`.I don't write the browser test.
- Does the debugger take you through weird code that you didn’t write? Make better use of the “Continue” button. If there are 2 breakpoints you want to hit, for example one in the test and on in the controller, you can set them both and then click “Continue” to hit the second one instead of trying to step over every single line of code.

## 📥 Submission

To submit your assignment, follow these steps:

1. Commit your changes to the local repository, for example:

   Terminal window

   ```cmd
   git add .

   git commit -m "Completed Authentication implementation."
   ```

2. Push your commits to the remote repository:

   Terminal window

   ```cmd
   git push
   ```

3. Submit your assignment on Gradescope.
   1. Go to [gradescope.ca](http://gradescope.ca/) (**not .com!**), log in, and click the link for this assignment.
   2. Select the correct repository and branch from the dropdown menus.
