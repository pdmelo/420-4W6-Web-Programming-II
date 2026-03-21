# 🧭React Router Exercise

## 🎯 Objectives

1. **Understand** what React Router is and why it's used in React applications..
2. **Use** React Router to navigate between pages without full page reloads.
3. **Use** dynamic routes and URL parameters to display different content based on the URL.
4. **Navigate** programmatically using React Router hooks.

## Why Use React Router?

React Router allows us to build **single-page applications (SPAs)** that :

- Navigate between pages without full page reloads.

- Use dynamic routing based on URL parameters(e.g. `/profile/john`)

- Implement nested routes for better structure.

- Redirect users programmatically..

## 🔨 Setup

1. Using the terminal, navigate to your `~/web-ii/exercises/` folder.
2. We will continue using the same [template](https://github.com/JAC-CS-Web-Programming-II-W26/E3.0-React-Template)  as the last exercise. 
3. Rename the cloned folder to ` ~/web-ii/exercises/3.4-react/
4. Start Docker Desktop.
5. All our dependencies are already installed.
6. In VS Code, hit `CMD/CTRL + SHIFT + P` and search + run `dev container: open folder in container`.
7. In the terminal of VS Code, hit the `+` icon to open a new terminal instance.
10. cd to `client` to run `npm run dev` to start the react server.

## 🔍 Context

**React Router** is a standard **routing library ** for React applications. It allows navigation between different views or components in a **single-page application (SPA)** without requiring a full page reload.



## 🧱Part 1: Setting Up The App

1. In the components folder, create two components `Home.tsx` and `About.tsx` with a some data.

   ```tsx
   export default function Home() {
   	return <h1> Welcome to by Pomemon World</h1>;
   }
   ```

   

   ```tsx
   export default function About() {
   	return <h2> Its all about pokemon trainers and pokemon wars</h2>;
   }
   ```

   

2. Add this style in the index.css. (Use the colors that please your eyes...and mine !)

   ```css
   .nav {
     background-color: rgb(163, 169, 228);
     color: whitesmoke;
     display: flex;
     justify-content: space-between;
     padding: 0 1rem;
   }
   .nav a {
     color: whitesmoke;
   }
   ```



## 🧪Part 2: Routing the OLD way(for comparision).

1. **Create** a new component `NavBarOld.tsx` and the following code.

```tsx
function NavBarOld() {
  return (
    <nav className="nav">
      <ul>
        <li>
          <a href="/">Home</a>
        </li>
        <li>
          <a href="/about">About</a>
        </li>
      </ul>
    </nav>
  );
}
export default NavBarOld;
```

2. **Update** the `App.tsx` (**Old Way**)to include the following code, with `NavBarOld` component.

```tsx
//...necessary imports
import { JSX } from "react";

function App() {
  let component: JSX.Element | null = null;

  //log this to understand the what it returns
  console.log(window.location);

  switch (window.location.pathname) {
    case "/home":
      component = <Home />;
      break;
    case "/about":
      component = <About />;
      break;
          default:
      component = <Home />; // fallback (optional but recommended)
  }
  return (
    <>
      <article>
        <NavBarOld />
        {component}
      </article>
    </>
  );
}
export default App;
```



## 🚀Part 3: React Router

`BrowserRouter` is a component from `react-router-dom` that enables **client-side routing** in a React. It manages navigation without causing a full-page reload.

1. **Modify** `App.tsx` to wrap the app with **BrowserRouter** and define basic routes.Pay attention to the imports..

```tsx
import { BrowserRouter as Router, Route, Routes } from "react-router-dom";
import Home from "./Home";
import About from "./About";

function App() {
  return (
    <Router>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </Router>
  );
}

export default App;
```



#### Navigating Between Pages

2. **Create** a new component `Navbar` , Use the `Link` component instead of `< a >` tags. Pay attention to the imports,It requires the `Link` react library from `react-router-dom`..
   - Unlike a traditional `<a href="...">`, `Link` **does not refresh the page**. Instead, it updates the URL and renders the corresponding route using React Router.

```tsx
import { Link } from 'react-router-dom';

export default function Navbar() {
  return (
    <nav>
      <Link to="/">Home</Link>
      <Link to="/about">About</Link>
    </nav>
  );
}
```

3. Update the `App.tsx` to replace`NavBarOld` with `Navbar` it should works as smoothly.

   ```tsx
   <Router>
     <Navbar />
     <Routes>
       <Route path="/" element={<Home />} />
       <Route path="/about" element={<About />} />
     </Routes>
   </Router>
   ```



## 🖥️ Part 4: New item in the Navigation bar(TODO) 
Create new link in the navigation bar that will link to a component that will display all the pokemons from pokemon.json in the public folder.



## 🔗 Part 5: URL Parameters (Dynamic Routes) 

Create a component `DisplaySingle.tsx` to show dynamic content based on URL parameters. Define dynamic routes in the `App.tsx` using `:` before a parameter name:

```tsx
//DisplaySingle.tsx
import { useParams } from "react-router-dom";

type Params = {
  username: string;
};

export default function DsiplaySingle() {
  const { username } = useParams<Params>();
  return <h1>Profile of {username}</h1>;
}
```

**Add route** in the `App.tsx`:

```tsx
<Route path="/profile/:username" element={<Profile />} />
```

**Add a Link** in your `Navbar` to navigate to a profile:`

```tsx
<Link to="/profile/Pikachu">Pikachu's Profile</Link>
```

### 💡 Why we use `:` for dynamic routes

In React Router, a URL parameter lets you capture **dynamic values** from the URL.

- The `:` tells React Router that this part of the path is a **variable**, not a fixed string.
- For example, `/profile/:username` means the URL could be `/profile/Pikachu`, `/profile/Bulbasaur`, etc.
- You can then access the value (`username`) inside your component using `useParams()`.



## 🔁 Part 6: Programmatic Navigation

Use `useNavigate` to programmatically navigate.`useNavigate` is a hook from **React Router v6** that allows you to programmatically navigate between pages

In the `Home.tsx`

```tsx
import { useNavigate } from "react-router-dom";

export default function Home() {
  const navigate = useNavigate();
  return <div>
      <h1>Home Page</h1>
      <button onClick={() => navigate("/about")}>
        Go to About
      </button>
    </div>
}
```

## Summary

- React Router enables SPA navigation without full page reloads.
- Routes are defined using `Routes` and `Route`.

  - `Router` wraps your app

    `Routes` contains all routes

    `Route` defines a path → component

- Navigation is handled using `Link` and `useNavigate`.

  - `Link` replaces `<a>` (no page reload!)
  - `useNavigate()` → navigate with code

- URL parameters allow dynamic routing.

  - `useParams()` → read URL values


React Router is an essential tool for structuring and navigating React applications effectively.
