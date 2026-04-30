# 3.5 - Testing

## 🎯 Objectives

- **Explain** the purpose of Playwright and its UI mode in the context of frontend web development.
- **Identify** common frontend issues that can be effectively debugged using Playwright’s UI mode.
- **Execute** Playwright tests in UI mode, observing their visual rendering within a controlled browser environment.
- **Utilize** Playwright’s Inspector tool to examine element properties, CSS styles, and the structure of a web page during debugging.
- **Diagnose** the root causes of frontend rendering errors by visually analysing test failures in Playwright’s UI mode and correlating them with application code.

## 🔨 Setup

1. Navigate to the [template repository](https://github.com/JAC-CS-Web-Programming-II-W26/E3.5-ReactTesting-Template) for this exercise. 
2. Clone the Git repo from the CLI`git clone <paste URL from GitHub>` (without the angle brackets) or using a GUI client like Git Desktop
3. Rename the cloned folder to `~/web-ii/exercises/3.5-testing/`.
4. Start Docker Desktop.
5. Assuming Docker is started, in VS Code, hit `CMD/CTRL + SHIFT + P`, search + run `dev container: open folder in container`, and select the downloaded folder.
6. The dependencies in the client and server and the playwright folder will get installed.
7. Run `ls` to make sure you’re in the root directory of the exercise and you see `client` and `server` folders.
8. cd to `client` to run `npm run dev` to start the react development server.
9. cd to `server` to run `npm run server` to start the server.
10. Split the terminal into another JavaScript debug terminal , 
    1. cd to **`client`** , then run `npm run test:ui` to start the **Playwright UI mode**.

11. Open both the [frontend website](http://localhost:5173/) and the [test runner](http://localhost:3001/) in the browser.

## 🔍 Context

In the last exercise, we used **React** to build dynamic and interactive pages on the frontend. By leveraging React’s component-based architecture, we were able to create reusable UI elements and manage application state efficiently, resulting in a responsive user experience.

Now, we’ll shift gears and focus on **testing our frontend applications** using [Playwright](https://playwright.dev/). Playwright is a powerful automation framework for web browsers. Its [UI mode](https://playwright.dev/docs/test-ui-mode) allows us to visually interact with a running browser instance controlled by Playwright. This is a fantastic tool for debugging frontend issues because we can see the application exactly as a user would, pinpoint rendering problems, and even inspect elements directly within the browser. Imagine being able to watch your application in action, click around, and see how the code translates to the visual elements on the screen - that’s the magic of Playwright’s UI mode, making debugging a breeze!

<div style="position:relative; width:100%; height:0px; padding-bottom:62.500%;">
	<iframe allow="fullscreen;autoplay" allowfullscreen height="100%" src="https://pdmelo.github.io/420-4W6-Web-Programming-II/images/3.3.1-playwright.mp4" width="100%" style="border:none; width:100%; height:100%; position:absolute; left:0px; top:0px; overflow:hidden; border-radius: 5px; ">
	</iframe>
</div>




## 🚦 Let’s Go

A Playwright test suite covering some basic routes is already written. However, some tests are failing due to bugs in the application code. It’s your job to execute the test suite in Playwright’s UI mode (`npm run test:ui`) and focus on *individual* failing tests **one at a time**. Use the visual feedback to identify the root cause of each test failure, and then modify the application code to make the tests pass.

1. **Incorrect Title of Page:**

   - Run the first test in the Playwright UI mode by clicking the green play button next to the test name “Homepage was retrieved successfully”.

   - Take note of where the test fails in Playwright and use the inspector in the actual website (right-click > inspect) to see the actual title of the page.

   - Compare with the expected title in the test assertion.

   - **💡 Is it the \*view\* or the \*controller\* that needs to be fixed?**


 Proceed once the bug is fixed.

2. **Incorrect Nav Link URL:**
   - Running the same test as in the previous step, the test should fail due to the incorrect URL in the navigation link.
   - Inspect the nav element’s `href` attribute and compare it against what the test is asserting.
   - Where do these links get generated?
   - 💡 Where is the error message generated and rendered?

 Proceed once the bug is fixed and the test is passing.

3. **Missing Error Message:**

   - Run the next test titled “Invalid path returned error”.
   - The test should fail due to the absence of an error message.
   - Use the Inspector to confirm the error message is truly absent from the DOM.
   - **💡 Where is the error message generated and rendered?**
    Proceed once the bug is fixed and the test is passing.
   
4. **Pokemon Not Found:**

   - Run the test titled “Pokemon found by ID”.
   - The test should fail due to the absence of the Pokémon’s name and type on the page.
   - Use the Inspector to confirm the absence of the Pokémon’s name and type from the DOM.
   - Where is the Pokémon’s name and type generated and rendered when getting just one Pokémon?
   - **💡 What CSS selectors are used to locate the Pokémon’s name and type?**

 Proceed once the bug is fixed and the test is passing.

5. **Missing Pokémon:**

   - Run the test titled “All Pokemon are displayed.”
   - The test should fail due to the absence of the Pokémon list on the page.
   - Use the Inspector to confirm the absence of the Pokémon list from the DOM.
   - **💡 Where is the Pokémon list generated and rendered when getting all Pokémon?**

   Breakpoints!

   [Set breakpoints in your code](https://pdmelo.github.io/420-4W6-Web-Programming-II/#/Guides/debugging) to pause the execution and inspect the state of your application. This is a great way to understand how your code is working and to identify bugs.

Once all the tests are passing, you’re done! You’ve successfully debugged the application using Playwright’s UI mode. You’ve also gained a deeper understanding of how the frontend and backend work together to create a seamless user experience.

## 📥 Submission

Take a screenshot with all the tests passing in Playwright’s [UI mode](http://localhost:3001/). If you want to get fancy and record a tiny video like the one above in the context section, you can do so using the screen recorder for [Mac](https://support.apple.com/en-us/102618) or [Windows](https://www.microsoft.com/en-us/windows/learning-center/how-to-record-screen-windows-11).

Submit the screenshot/video on Moodle.