# 🛤️3.1 JSX Exercise

## 🎯 Objectives

1. **Understand** the basics of jsx syntax in a React application using React .

2. **Create** Single pages and display content.

3. **Implement** components using Classes and Functions.

## 🔨 Setup

1. Using the terminal, navigate to your `~/web-ii/exercises/` folder.
2. Go to [the repository for this exercise](https://github.com/JAC-CS-Web-Programming-II-W26/E3.0-React-Template) and click `Code -> 📋` to copy the URL.
3. Clone the Git repo from the CLI `git clone <paste URL from GitHub>` (without the angle brackets) or using a GUI client like [GitHub Desktop](https://desktop.github.com/).
   - You may have to use the `HTTPS` or `SSH` URL to clone depending on your settings. If one doesn’t work, try the other by clicking `Use SSH` or `Use HTTPS` above the 📋, and copy the new URL.
4. Rename the cloned folder to `~/web-ii/exercises/3-react/`.
5. Start Docker Desktop.
6. Make time to go through the folder structure.
7. In VS Code, hit `CMD/CTRL + SHIFT + P` and search + run `dev container: open folder in container`.
8. In the terminal of VS Code, hit the `+` icon to open a new terminal instance. Run `ls` to make sure you’re in the root directory of the exercise and you see `client` and `server` folders.
9. cd to `client` to run `npm run dev` to start the react server.

## Part 1: Understanding the TSX Syntax

The `App.jsx` is the entry point to the React application.

### **1. Declaring a Variable and Using TSX**

1. Open `client/src/App.tsx`and and declare a variable `name` with a value and replace the h1 html tag, see example below:

```tsx
import React from "react";

function App = {
  const name = "Pokemons!";
  return (
    <div>
      <h1>Meet my {name}</h1>
    </div>
  );
};

export default App;
```

2. You can also embed styles in the html, lets make the name italics and add colour to it.

   ```tsx
   <h1>
     Meet the <i style={{ color: "SteelBlue" }}>{name}</i>
   </h1>
   ```

   > [!NOTE]
   > The **double curly brackets** on the style:
   >
   > - {color:"SteelBlue"} is a javascript object
   > - TSX requires an extra set of `{}` to embed JavaScript expressions. hence the {{color:"SteelBlue"}}

3. Adding other html tags like a button

   ```jsx
   <button className="outline" onClick={() => alert("Hi there")}>
     Click Me
   </button>
   ```

__Key Points:__

- JSX allows embedding JavaScript expressions within curly braces `{}`.
- It’s used to render HTML-like elements inside the JavaScript code.

## Part 2: Creating Reusable Components

React components can be defined in multiple ways. Let's explore two common methods.

### **Method 1 : Class Components**

A React component can be created using a class. The class name should start with an uppercase letter and extend `React.Component`

```jsx
class Welcome extends React.Component {
  constructor(props) {
    super(props);
  }
  render() {
    return <h1>Meet my {this.props.name}</h1>;
  }
}
```

Now, replace the `<h1>` tag in the `App()` function with the Component class, delete the previous `name`declaration:

```tsx
<Welcome name="Pokemons" />
```

### **Method 2. Functional Components**

Functional components are simpler and more commonly used in modern React:

```jsx
function Greeting(props) {
  return <h1>Hello {props.name}</h1>;
}
```

** Key Points:**

- The `Greeting` component accepts a `name` prop and dynamically displays the greeting message.
- Props allow components to receive dynamic values.

Update your `App` function to include both components:

```jsx
return (
  <div className="container">
    <article>
     
        <Welcome name="Pokemons" />
        <Greeting name="Pikachu" />
        <p>
          Each with <b>unique abilities </b> and personalities.
          <br />
          Together, we embark on <b>countless adventures</b> and face every
          challenge that comes our way.
        </p>
     
      <button onClick={() => alert("Hi there")}>Click Me</button>
    </article>
  </div>
);
```

**Key Points:**

- The `Greeting` component accepts a `name` prop and dynamically displays the greeting message.
- Props allow components to receive dynamic values.
- The `<hgroup>` tag is **used to surround a heading and one or more `<p> `elements**.
- `name="Pokemons"` is passed as a prop, and you access Props via `props` object using `props.name`

## Part 3 Sending multiple props.

```tsx
<Greeting name="Alice" color="blue" />
```

In the `Greeting` function component access the props as follows:

```tsx
type GreetingProps = {
  name: string;
  color: string;
};
function Greeting(props: GreetingProps) {
  return (
    <h1 style={{ color: props.color }}>
      Hello <i>{props.name}</i>{" "}
    </h1>
  );
}
```

## Part 4: Working with Lists and JSX

### 1. **Without using a component.**

Modify `App.tsx` to render a list of Pokemons

Declare an array `myPokemons` with some values. Display the `myPokemons` array in your App component using the .map() function to render the list directly inside JSX.

```jsx
<ul>
  {myPokemons.map((pokemon, index) => (
    <li key={index}>{pokemon}</li>
  ))}
</ul>
```

**Key Points:**

- The `map` function is used to iterate over the array of names.

You could render a `Greeting` component for each name.

```jsx
<ul>
			  {myPokemons.map((pokemon, index) => (
					<li key={index}><Welcome {pokemon}/></li>
				))}
			</ul>
```

> [!Note]
> The curly brackets {} in JSX are used to embed JavaScript expressions inside JSX.JSX allows you to mix HTML-like syntax with JavaScript, but JavaScript expressions must be wrapped in {} inside JSX.

### 2. **Using a Component.** [TODO]

Declare a component function called `PokemonList`, call it in the `App` function

```jsx
<PokemonList pokemons={myPokemons} />
```

## Part 5 Separating Components into Files.

1. Create a `components` folder in the `src` folder . Add a new file `Greeting.tsx`

```tsx
export default function Greeting(props) {
  return (
    <h1>
      Hello <i>{props.name}</i>
    </h1>
  );
}
```

2. Import it in `App.jsx`:

```jsx
import Greeting from "./components/Greeting";
```

3. Use the component in the `App()` function as before.

## Part 6 Composing Complex Components.

We can compose [multiple components](https://zhenyong.github.io/react/docs/multiple-components.html) into a single component.

- Create a new `HeaderBody` component in a separate file.

  ```jsx
  export default function HeaderBody() {
  	return (
  		<>
  			<p>
  				My friend has <b>powers</b>
  			</p>
  			<br />
  		</>
  	);
  }
  ```

  

- Create a new `MainHeader` component in a separate file.

```jsx
import Greeting from "./Greeting";
import HeaderBody from "./HeaderBody";

function MainHeader() {
	const name = "Pikachu";
	const color = "SteelBlue";
	return (
		<>
			<Greeting name={name} color={color} />
			<HeaderBody />
		</>
	);
}

export default MainHeader;

```

Call the `MainHeader` component in the `App.jsx`. The return should look like this. Make sure to import the appropriate components.

```jsx
return (
		<div className="container">
			<article>
					<div>
						<MainHeader />
					</div>

				<PokemonList pokemons={myPokemons} />

				<button onClick={() => alert("Hi there")}>Click Me</button>
			</article>
		</div>
	);
```





## 📥 Submission

Take screenshots of:

- The app displaying the name and greeting.
- The app displaying dynamic greetings for different names.
- The list of Pokemons rendered from an array using a functional component.
- Snippet of your coding using complex components.

Submit all screenshots on Moodle.
