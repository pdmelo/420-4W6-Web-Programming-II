# React

**MVC (Model-View-Controller)** is a design pattern for organizing code.<br>
**Model:** Manages data and business logic.<br>
**View:** Responsible for presenting data to the user.<br>
**Controller:** Handles user inputs and updates the Model & View.

## Understanding the View in MVC
- The **View** is the UI layer that displays data to the user.
- Separates presentation code from logic code
- It gets updates from the **Model** and re-renders accordingly.
- Common implementation: Server-rendered templates (e.g., EJS, Handlebars).
- **Limitations:**
    - Full-page reloads required for updates.
    - Less interactive and slower for dynamic content.

As web applications became more complex, traditional Views became inefficient. The rise of Single-Page Applications (SPAs) helped create more dynamic experiences.<br>

**Key requirements:**

   - Efficient updates without full-page reloads.
   - Better user experience with smooth interactions.
   - Maintainable and reusable UI components.

[**Web Frameworks**](https://en.wikipedia.org/wiki/Web_framework)

- Web frameworks provide a standard way to build and deploy web applications
- They often automate common activities to simplify coding
- They often promote code re-use
- They often provide many additional library functions to allow developers to quickly incorporate a range of capabilities without coding unnecessarily from scratch.
- There are 2 types of frameworks:
  - Server-side / back-end
    - Ruby on Rails, ASP.NET Core, Spring MVC, Django
    - Express (with Node.js)
  - Client-side / front-end
    - Angular, Vue
    - React

## ⚛️ Meet React - Modern solution for the View

- A JavaScript library for building **interactive UIs**. .
- Developed by **Facebook (Meta)**.
- Instead of server-rendered templates, React uses components to manage Views dynamically.
- Efficient with **Virtual DOM**.
- Used for **single-page applications (SPA)**.
- React is [declarative](https://medium.com/@myung.kim287/declarative-vs-imperative-251ce99c6c44) writing style ie It focuses on the result that will be displayed rather than on the steps for how to display it.

- Why Use React?
	- **Reusability**: Build UI components once and use them everywhere. 
	- **Performance**: Efficient updates with Virtual DOM. 
	- **Community Support**: Large ecosystem & many tools. 
	- **Declarative UI**: Focus on **what** to render, not **how**.
	- **Strong Ecosystem**: Works with frameworks like Next.js.

**React vs Traditional Views**

| Feature       | Traditional View      | React View                         |
| ------------- | --------------------- | ---------------------------------- |
| Rendering     | Server-side templates | Client-side rendering              |
| Performance   | Page reloads          | Efficient updates with Virtual DOM |
| Interactivity | Limited               | High (components, event handling)  |
| Reusability   | Difficult             | Component-based                    |

**Example: traditional template vs a React component**

```html
<!-- Traditional Server-side Template (EJS) -->
<h1>Hello, <%= user.name %>!</h1>

// React component
function Greeting({ name }) {
  return <h1>Hello, {name}!</h1>;
}
```
**Explanation:**
- Traditional templates rely on server-side rendering and require full page reloads.

- React components render dynamically on the client without reloading the entire page.

- Using JavaScript and a special markup language called **JSX** (JavaScript XML), you can specify the HTML you want to generate.

  - This is called a React element. Think of it as a description of an HTML element, not the HTML itself.

  - The specifics can vary depending on data, conditional logic, loops, etc.

  - The element (typically) describes the entire UI and is made up of many other elements

- Then, that React element gets "**rendered**" in an HTML `<div>` container.
    - The React element is rendered in the DOM element.
    - Essentially think of this as a "cut-and-paste" into the `<div>` container.

- This rendered element is **immutable** – you can't change its children or elements.

    - An element is like a single frame in a movie – it represents the UI/web page at a certain point in time.

    - To change the web page, you create a new React element and render that.
- But, **React only changes the parts of the UI that have** **actually changed****.**


>[!NOTE]
>React front-end uses the endpoints we've defined in our node back-end to send and receive data from the server. In "traditional" MVC, the controller directly determines what view to display in the next step. In our approach, the controller can only communicate the HTTP responses.
>
>- So, for example, sending a 500 error may trigger an error view in React, but React actually decides to show that, not the controller.


## ⚙️ React Core Concepts
- **[Virtual DOM](#the-virtual-dom):** Efficient rendering
- **[Components](#components):** Reusable UI building blocks.
- **[TSX](#tsx):** combines TypeScript and JSX( JavaScript +html), letting you write HTML-like UI elements with full type safety..
- **[State & Props](#props-and-state):** Manage dynamic data..
- **[Hooks](#hooks):** Modern way to handle state and side effects.

### The Virtual DOM

- What is the DOM?
  - A representation of the webpage.
  
- Instead of updating the real DOM directly, React:
  1. Creates a virtual copy of the DOM.
  
  2. Compares the new and old versions (diffing algorithm).
  
  3. Updates only the necessary parts or the updated parts of the real DOM.
  
  4. For every (real) DOM object, there is a corresponding "Virtual DOM object".
	   - [A virtual DOM](https://www.codecademy.com/article/react-virtual-dom) object is like a lightweight copy of a DOM object
     
  5. A virtual DOM object has the same properties as a real DOM object, but it lacks the real thing’s power to directly change what’s on the screen.
  
  6. Manipulating the DOM is slow. 
    - Re-rendering or re-painting of the UI is what makes it slow
  
  7. Manipulating the virtual DOM is much faster, because nothing gets drawn onscreen.
    - Think of manipulating the virtual DOM as editing a blueprint, as opposed to moving rooms in an actual house.
  8. Here’s what happens when you try to update the DOM in React (i.e., when you [render a React](https://dev.to/teo_garcia/understanding-rendering-in-react-i5i) element):
     - The entire virtual DOM gets updated.
     - The virtual DOM gets compared to what it looked like before you updated it. React figures out which objects have changed.
     - The changed objects, and the changed objects only, get updated on the real DOM.
     - Changes on the real DOM cause the screen to change. 
     
------

### Components

- The **building blocks** of React apps.Like functions returning HTML elements
- Encapsulated, reusable pieces of UI.
- The purpose of a Component is to generate a particular React element or portion of an element
- A component generally gets an input, and changes behavior based on it. 
- This behavior change generally manifests as a change in the UI of some part of the page. 
- Two types:
  - **Functional Components** (modern, recommended)
  - **Class Components** (older, still supported)

**Example Functional Component:**

```jsx
const Welcome = () => {
  return <h2>Hello, React!</h2>;
};
```
**Example Class Component:**
```jsx
import { Component } from "react";

class GreetingClass extends Component {
  render() {
    return <h2>Hello from Class Component!</h2>;
  }
}
```
Both components simply return an `<h2>` element with a greeting message.

------

### TSX

- A **syntax extension**  lets you write UI elements in TypeScript using an HTML-like syntax, making it easier to build interfaces while keeping the benefits of TypeScript.

- **Not a template language** — TSX is **transpiled by Babel or TypeScript** into standard JavaScript/React calls (`React.createElement`).

  **Type-safe** — TSX allows you to define **types for props and state**, so your editor can catch errors before the app runs.

- - React doesn’t require TSX, but using it makes the UI **easier to read, maintain, and understand**.

  > In short: **TSX = JSX + TypeScript types**, which means HTML-like code in TypeScript with type safety.

#### How TSX Works

Every TSX element is just shorthand for a JavaScript call:
```typescript
React.createElement(component, props, ...children)
```

Anything you can do in TSX can also be done with plain JavaScript—but TSX is **cleaner and easier to read**.

**Some key differences from HTML:**

- Attribute names use **camelCase**:

  - `className` instead of `class`
  - `htmlFor` instead of `for`

- `style` uses a **JavaScript object** instead of a CSS string:

  ```typescript
  <div style={{ backgroundColor: "blue", fontSize: "16px" }} />
  ```

- Use `dangerouslySetInnerHTML` instead of `innerHTML`.

**Examples**:

TSX with typed props:

```tsx
type AppProps = {
  name: string;
};

function Greeting({ name }: AppProps) {
  return <h1>Hello, {name}!</h1>;
}

export default Greeting;

```

**Traditional JavaScript (without JSX):**

```js
const Greeting = () => {
  return <h2>Hello!</h2>;
};

export default Greeting;

```

TSX is shorter, more readable, and type-safe .

#### **Comments in TSX**

```tsx
<div>
  {/* Single-line comment */}
  Hello, {name}!
</div>

<div>
  {/*  Multi-line comment
    works too. */}
  Hello, {name}! 
</div>
```

✅ **Key Points **

- `.tsx` files = **TypeScript + JSX** (type-safe).
- `.jsx` files = **JavaScript + JSX** (no types).
- TSX makes UI code **easy to read, maintain, and type-safe**.
- It’s just a **shortcut for `React.createElement`** with extra TypeScript benefits.

------

### Props And State

#### 🎒 Props (Properties)

- Used to **pass data** to components.
- **Read-only** (immutable).
- Allow data to be passed from parent to child components.

**Example (Without Destructuring):**

```tsx
type GreetingProps = {
  name: string;
};

const Greeting = (props: GreetingProps) => {
  return <h1>Hello, {props.name}!</h1>;
};

export default Greeting;
```

**Example (With Destructuring):**

```tsx
type GreetingProps = {
  name: string;
};

const Greeting = ({ name }: GreetingProps) => {
  return <h1>Hello, {name}!</h1>;
};

export default Greeting;
```

**Usage:**

```tsx
import Greeting from "./Greeting";

function App() {
  return (
    <div>
      <Greeting name="Alice" />
      <Greeting name="Bob" />
    </div>
  );
}

export default App;
```



**Modifying Props**

- **❌ Trying to Modify Props (Incorrect)**

```tsx
type GreetingProps = {
  name: string;
};

const Greeting = ({ name }: GreetingProps) => {
  // name = "Bob"; // ❌ This will NOT work! Props are read-only.
  return <h1>Hello, {name}!</h1>;
};

export default Greeting;
```

👉 **Trying to modify `name` inside the component will cause a TypeScript error..**



- **Using State for Mutable Data**
  - If you need to change the data, use **state** instead of modifying props:

```tsx
import { useState } from "react";

type GreetingProps = {
  initialName: string;
};

const Greeting = ({ initialName }: GreetingProps) => {
  //  Using state for mutable data
  const [name, setName] = useState<string>(initialName);

  return (
    <div>
      <h1>Hello, {name}!</h1>
      <button onClick={() => setName("Bob")}>Change Name</button>
    </div>
  );
};

export default Greeting;
    
```
👉 Here, initialName is a prop (immutable), but we store it in useState to make it mutable.
Now clicking the button updates the name without modifying the original prop.

**Usage**

```tsx
<Greeting initialName="Alice" />;
// Initially displays: "Hello, Alice!"
// Clicking the button changes it to "Hello, Bob!"
```

------

#### 🔥 State (Component Memory)

- State is like a variable, but React **remembers** its value across re-renders.

- Stores **dynamic data** in components.ie Data is mutable

- Uses the `useState` hook (functional components).

**Example:**

```tsx
import { useState } from 'react';

const Counter = () => {
  //const [count, setCount] = useState(0); //TypeScript infers count is a number from useState(0)
  const [count, setCount] = useState<number>(0);  
    
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increase</button>
    </div>
  );
};
export default Counter;
```
----



### Developer Tools

You can add the [React Developer Tool](https://react.dev/learn/react-developer-tools) extension on your browser. It adds React debugging tools to the Chrome Developer Tools

## Resources

- [What is React](https://www.w3schools.com/whatis/whatis_react.asp)
- [Quick Start](https://react.dev/learn)
- [W3school- React-Components](https://www.w3schools.com/react/react_components.asp)
- [Component](https://react.dev/learn/your-first-component)
- [Adding react to existing project](https://react.dev/learn/add-react-to-an-existing-project)
- [Most Popular Technology for webframe](https://survey.stackoverflow.co/2022/#most-popular-technologies-webframe-prof)

