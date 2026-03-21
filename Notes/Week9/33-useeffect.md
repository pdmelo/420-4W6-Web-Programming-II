# 🔄3.3 `useEffect` Hook Exercise

## 🎯 Objectives

1. **Understand** what the `useEffect` Hook is and why it is used in React.
2. **Use** the `useEffect` Hook to perform side effects in a functional component.
3. **Modify** a React component to fetch and display data dynamically.

## 🔨 Setup

1. We will continue using the same [template](https://github.com/JAC-CS-Web-Programming-II-W26/E3.0-React-Template)  as the last exercise. 
2. Rename the cloned folder to ` ~/web-ii/exercises/3.3-react/` or `~/web-ii/exercises/3.3-useffect/`
3. Start Docker Desktop.
4. All our dependencies are already installed.
5. In VS Code, hit `CMD/CTRL + SHIFT + P` and search + run `dev container: open folder in container`.
6. In the terminal of VS Code, hit the `+` icon to open a new terminal instance.
7. Navigate to the client folder.
8. Start the React server: `npm run dev`.

## ⚡Part 1: Understanding `useEffect` in React

The `useEffect` **Hook** allows you to perform **side effects** in functional components.

### What is a Side Effect?

A **side effect** refers to anything that affects something **outside the component**:

- Fetching data from an API
- Updating the DOM manually
- Setting up subscriptions (e.g. event listeners)
- Updating local storage
- Changing the document title

### Why are Side Effects Important?

- React components should be **pure functions**,they should return the same output for the same input.
- Some operations (like fetching data) **must happen outside the rendering process**— that’s why we use **side effects**.

- `useEffect` is runs **after the component renders**.
  - `After React updates the UI → run this code`

- - It can optionally **cleanup** effects when the component unmounts.

### **How `useEffect` works?**

Use `useEffect` when you need to run code **after the component renders**:

- Runs **after the first render**

  Runs **after every update (by default)**

- Can optionally:

  - Run only once
  - Run when specific state changes
  - Clean up after itself


### **When Would You Use `useEffect`?**

- **Fetching data from an API** when a component mounts.

- **Setting up event listeners** (e.g.`window.addEventListener`).

- **Performing cleanup tasks** (e.g.  clearing timers) when a component unmounts.

  

## 📝Part 2: Syntax

### **🔹`useEffect` Basic Syntax**

```tsx
useEffect(() => {
  // Code to run
});
```

👉 The function inside `useEffect` runs after  **every render**

---

#### 🔹 1. No Dependency Array  (Runs on Every Render)
**Syntax**:

```tsx
useEffect(() => {
  console.log("Component rendered!");
});
```
🚀**Demo**: - Logs on every render

```tsx
import { useEffect, useState } from "react";

function App() {
  const [count, setCount] = useState<number>(0);

  useEffect(() => {
    console.log("Component rendered!");
  });

  return (
    <div>
      <h1>Counter: {count}</h1>
      <button onClick={() => setCount(count + 1)}>Increase</button>
    </div>
  );
}

export default App;
```

✅**Key Points:**

- Watch the Console on the developer tools for ***“Component Mounted”\*** , every time you click on Increase, it will print the “Component Mounted”
- Clicking the button triggers `useEffect` again.

👉**Runs on:**
   - Initial mount
   - Every state/prop update

> [!Caution]
> Avoid infinite loops! If the effect updates state, it will cause a re-render and repeat infinitely.
>
> ⚠️ Infinite Loop Example
>
> ```tsx
> import { useEffect, useState } from "react";
> 
> function App() {
>   const [count, setCount] = useState<number>(0);
> 
>   useEffect(() => {
>     setCount(count + 1); // ❌ updating state
>   }); // no dependency array
> 
>   return <h1>{count}</h1>;
> }
> 
> export default App;
> ```
>
> 💥 **What Happens Step-by-Step**
>
> 1. Component renders → `useEffect` runs
> 2. `setCount(count + 1)` updates state
> 3. State change → triggers re-render
> 4. Re-render → `useEffect` runs again
> 5. Repeat forever… 🔁
>
> 👉 Result: **Infinite loop**

---

#### 🔹 2. Empty Dependency Array (Runs Only Once)

**Syntax**:

```tsx
useEffect(() => {
  // Code to run
}, []);
```

🚀**Demo**: Logs **only once**

```tsx
import { useEffect, useState } from "react";

function App() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    // ✅ This runs only once, when the component mounts
    console.log("Component mounted");
  }, []); // empty dependency array ensures it does NOT run on updates

  return (
    <div className="container">
      <h1>Counter: {count}</h1>
      <button onClick={() => setCount(count + 1)}>Increase</button>
    </div>
  );
}

export default App;
```

✅**Key Points:**

- Watch the Console on the developer tools for ***“Component Mounted”\*** , there is no update on the Console every time you click on Increase
- The empty dependency array `[]` ensures the effect doesn’t run on updates.
- Subsequent updates (like clicking the button) **do not trigger this effect**.
- `useEffect(() => { console.log("Component mounted"); })` runs **only once when the component mounts**.
- Perfect for **fetching data** or **adding event listeners**.

👉**Runs only once** when the component **mounts** 
>[!Note]
>💡 React's **Strict Mode** in development may run this effect **twice on mount**, >but this only happens in development.

---

#### 🔹 3. With Dependencies (Runs on  State/Prop Change)

**Syntax**:

```tsx
useEffect(() => {
    // Perform appropriate update
}, [stateVar1, stateVar2]);
```

👉**Runs only when `StateVar1` or `StateVar2`changes**.

🚀**Demo:** - Watching a state change

```tsx
import { useEffect, useState } from "react";

function App() {
  const [count, setCount] = useState<number>(0);

  useEffect(() => {
    console.log(`Count changed: ${count}`);
  }, [count]);

  return (
    <div>
      <h2>Count: {count}</h2>
      <button onClick={() => setCount(count + 1)}>Increase</button>
    </div>
  );
}
```

👉**Runs** when `count` changes

- Great for reacting to state updates.

- Useful for watching **specific state or props**.

- More useful than just logging

- Useful for watching **specific state or props**.

✅**Key Points**

- `useEffect` runs **whenever** `count` **changes**.

- Can trigger logic (alerts, API calls, etc.)

  

## 🔄Part 3: Fetching Data with `useEffect`

**Step 1**: **Create a `json`  data** in file `pokemonList.json` in the `public` folder. Add some data for example a list of Pokemons with id , name and type.

```json
[
  { "id": 1, "name": "Charmander" },
  { "id": 2, "name": "Bulbasaur" },
  { "id": 3, "name": "Squirtle" }
]
```



**Step 2**: **Create** FetchData Component `FetchData.tsx`.

```tsx
import { useState, useEffect } from "react";
import PokemonList from "./PokemonList";

type Pokemon = {
  id: number;
  name: string;
};

function FetchData() {
  const [pokemon, setPokemon] = useState<Pokemon[]>([]);

  useEffect(() => {
    async function fetchPokemon() {
      const response = await fetch("pokemonList.json");
      const data = await response.json();
      setPokemon(data);
    }

    fetchPokemon();
  }, []);

  return (
    <div>
      <h4>List of Pokemon</h4>
      <PokemonList pokemons={pokemon} />
    </div>
  );
}

export default FetchData;
```

**Step 3:* **Update** `PokemonList.tsx` to now map an object instead of an array.

```tsx
type Props = {
  pokemons: { id: number; name: string }[];
};

function PokemonList({ pokemons }: Props) {
  return (
    <ul>
      {pokemons.map((pokemon) => (
        <li key={pokemon.id}>{pokemon.name}</li>
      ))}
    </ul>
  );
}

export default PokemonList;
```

**Step4:** **Modify** `App.tsx` to fetch Pokemon data from `Fetchdata`.

```tsx
import FetchData from "./FetchData";

function App() {
  return (
    <div>
      <h1>My Pokemon List</h1>
      <FetchData />
    </div>
  );
}
```



✅**Key Points:**

- `fetch("pokemonList.json")` retrieves JSON data from a file..
- `useEffect` rensures fetching **runs only once**.
- The `pokemon` state  updates dynamically render the list.



## 🧹Part 4: Cleanup on Unmount 

1. Create a new component `Clock.tsx`.

```tsx
import { useState, useEffect } from "react";

function Clock() {
  const [time, setTime] = useState(new Date().toLocaleTimeString());

  useEffect(() => {
    const interval = setInterval(() => {
      setTime(new Date().toLocaleTimeString());
    }, 1000);

    return () => clearInterval(interval); // Cleanup to avoid memory leaks
  }, []);

  return (
    <div>
      <p>Current Time: {time}</p>
    </div>
  );
}

export default Clock;
```

✅**Key Points:**

1. **Side Effect (`useEffect`)**:

   - When the component mounts, `useEffect` sets up a repeating action using `setInterval`.
   - `setInterval` runs a function every **1 second**, updating `time` with the new current time.
   - The `useEffect` dependency array (`[]`) ensures this effect runs **only once** when the component mounts.

2. **Cleanup** `clearInterval`

   - This is the **cleanup function**, which React calls when the component **unmounts**.
   - `clearInterval(interval)` stops the interval from running to prevent memory leaks or errors.
   - Without this cleanup, if the component is removed from the UI, the interval would keep running, leading to unwanted side effects

> [!Note]  
> `clearInterval` is part of the `setInterval/clearInterval` pair in JavaScript.
> `setInterval` is used to repeatedly execute a function after a specified delay (in milliseconds).
> `clearInterval` is used to stop an interval that was started with `setInterval`.


## 🔀🧹Part 5: Conditional Rendering + Cleanup Demo 

Modify the `App.tsx` to include the clock component, But first declare a Boolean state, `showClock`. The Clock will be displayed on onclick of a button. We will use conditional rendering here


```tsx
import { useState } from "react";
import Clock from "./Clock";

function App() {
  const [showClock, setShowClock] = useState<boolean>(true);

  return (
    <div>
      <button onClick={() => setShowClock(!showClock)}>
        Toggle Clock
      </button>

      {showClock && <Clock />}
    </div>
  );
}
```

> [!Note]  
> **Conditional Rendering in React :**
> Conditional rendering shows/hides components.
>
> In the given App function, the component Clock is displayed based on the state variable `showClock`.
>
> - If `showClock` is true, <Clock/> is rendered.
> - If `showClock` is false, React doesn't render anything.

Once you have the clock functioning on the app. Remove the cleanup in the `useEffect`

```tsx
return () => clearInterval(interval); // remove this line.
```

---

⚠️**What Happens Without Cleanup?**

- What Happens When We Click the Button?

- Initially, the `Clock` is mounted and starts the `setInterval`, updating the time every second.

- When you click the `Toggle Clock` button:

  - The `Clock` component unmounts (disappears from the UI).

  - But the interval is still running in the background because we didn't use `clearInterval`!

- If you click the button again and re-mount the component:
  - A new interval is created.

Now two intervals are running at the same time, updating state twice per second.

If you toggle the component multiple times, intervals keep stacking up, making the clock update multiple times per second.

**In Short**- 

- Timer keeps running in background
- Multiple timers stack
- Clock speeds up
- ❌ Memory leak



## 🎯Summary

| `useEffect(() => {})`                      | Runs on every render        |
| ------------------------------------------ | --------------------------- |
| `useEffect(() => {}, [])`                  | Runs **only once** on mount |
| `useEffect(() => {}, [count])`             | Runs when `count` changes   |
| `useEffect(() => { return () => {} }, [])` | Runs cleanup on unmount     |



## 📥 Submission

Take screenshots of:

- The console log showing "Component mounted" when count increases(Without dependencies).
- Pokemon data displayed after fetching.

Submit all screenshots on Moodle.
