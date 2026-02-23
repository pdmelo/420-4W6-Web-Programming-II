# 2.3 - Jest Testing

## 🎯 Objectives

1. **Understand** how to test asynchronous functions using Jest.
2. **Implement** tests for functions that return promises and use async/await.
3. **Use** Jest matchers such as `toBe`, `toEqual` and `rejects` to handle async assertions.

## 🔨 Setup

1. Using the terminal, navigate to your `~/web-ii/exercises/` folder.
2. Go to [the repository for this exercise](https://github.com/JAC-CS-Web-Programming-II-W26/E2.3-Jest-Template) and click `Code -> 📋` to copy the URL.
3. Clone the Git repo from the CLI `git clone <paste URL from GitHub>` (without the angle brackets) or using a GUI client like [GitHub Desktop](https://desktop.github.com/).

   - You may have to use the `HTTPS` or `SSH` URL to clone depending on your settings. If one doesn’t work, try the other by clicking `Use SSH` or `Use HTTPS` above the 📋, and copy the new URL.

4. Rename the cloned folder to `~/web-ii/exercises/2.3-jesttesting/`.

5. Start Docker Desktop.

6. In VS Code

   1. open `package.json` observe the dependencies added for the test and the scripts sections

   2. open the typescript compiler options `tsconfig.json` observe the types included.

7. In VS Code, hit `CMD/CTRL + SHIFT + P` and search + run `dev container: open folder in container`.

8. In the terminal of VS Code, hit the `+` icon to open a new terminal instance. Run `ls` to make sure you’re in the root directory of the exercise and that you see `package.json`.

9. Run `npm install` to install all our dependencies.

## 🔍 Context

Jest allows testing asynchronous code using promises, async/await, and callbacks. We will explore different ways to handle asynchronous operations and ensure correct test execution.

## 🚦 Let’s Go

### Part 1: Testing Asynchronous Functions

#### 1. Create Jest Tests

Lets create jest test for the `app.ts` application in the `src` folder

Create a new file in the `tests` folder `tests/app.test.ts`

- This **naming convention** is important. The name of the test file matches the name of the file you are testing with the insertion of '.test' before '.ts'

**import the method form the application that we need to test**
copy the following code to create a teste suite using describe and set the stage for testing using beforeEach:

```ts
import { fetchPokemon, createPokemon} from "../src/app";
//dont forget to import database from the right source

describe("Pokemon CRUD Operations", () => {
///* Make sure the database is empty before each test.  This runs before each test.  See https://jestjs.io/docs/api */

 beforeEach(() => {
    // Reset the database before each test to ensure tests are isolated
    database.length = 0;
  });

});//eof test suite
```

- **describe**: Groups related tests together to organize them into meaningful sections, such as "Pokemon Operations", "createPokemon function", etc.,
- **beforeEach**: Ensures that before each test, the database is reset (database.length = 0), so each test starts with a clean state.

now add each test with the following approximate Syntax within the same test suite.:

```ts
test(label-for-the-test, async () => {
const { name, type } = createPokemonData();
<call createPokemon function>
<get results from database file>
<use ‘expect’ to check the results>
});
```

Consult https://jestjs.io/docs/expect to see methods you can use to check results, such as expect(result).toBe(value);

Example :unit test for a successful call to addPokemon below.

```ts
// Write your imports here

describe("Pokemon CRUD Operations", () => {
///* Make sure the database is empty before each test.  This runs before each test.  See https://jestjs.io/docs/api */

 beforeEach(() => {
    // Reset the database before each test to ensure tests are isolated
    database.length = 0;
  });
 test("Pokemon was created", async () => {
		const initialLength = database.length;
		const newLength = await createPokemon({
			name: "Pikachu",
			type: "Electric",
		});
		expect(newLength).toBe(1);

		expect(database).toContainEqual({ name: "Pikachu", type: "Electric" });
	});//eof test1

});//eof test suite

```

Run the test:

```bash
npm test
```

To run an individual test, you can use the `--testNamePattern` option with `npm test`. For example, to run a test with the name “Pokemon was created.”, you can use the following command:

```
npm test --testNamePattern = "Pokemon was created"
```

here is an example to test to fetch Pokemons

```ts
test("Pokemon retrieve Pokemon after adding", async () => {
  const pokemon = { name: "Pikachu", type: "Electric" };
  await createPokemon(pokemon);
  const result = await fetchPokemon();
  expect(result).toEqual([{ name: "Pikachu", type: "Electric" }]);
});
```

Run the test:

```
npm test
```

### Part 2: Testing a Function that Rejects

#### 1. An example of a rejected promise

```ts
export async function fetchPokemonWithError(): Promise<never> { {
  return Promise.reject(new Error("Failed to fetch Pokemon"));
}
```

#### 2. Test for the rejection case

```ts
import { fetchPokemonWithError } from "../src/app";

test("fetchPokemonWithError throws an error", async () => {
  await expect(fetchPokemonWithError()).rejects.toThrow("Failed to fetch Pokemon");
});
```

Run the test:

```bash
npm test
```

## 📥 Submission

1. Write the following Jest tests and ensure all 8 pass:
   1. `createPokemon` adds a new Pokémon.
   2. `fetchPokemon` logs the database.
   3. `addPokemon` logs the database.
   4. `updatePokemon` with a test for throw error if pokemon is not found
   5. `deletePokemon` with a test for throw error if pokemon is not found
   6. `fetchPokemonWithError` properly throws an error.
2. Take a screenshot of your Jest output showing the successful test results.
3. Submit the screenshot to the Moodle dropbox for this exercise.
