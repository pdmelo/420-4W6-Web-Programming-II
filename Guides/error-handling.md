# ⚠️ Error Handling - Server Side (Node.js)

## What is Server-Side Error Handling?

Server-side error handling is how your backend:

- Detects problems
- Responds safely
- Prevents crashes
- Communicates clearly with the client

📌 **Key Point:**
 The goal is **not to avoid errors**, it’s to **survive them**.

------

## Why Error Handling Matters

Without proper error handling:

- The server can crash
- Requests can hang forever
- Clients receive confusing responses
- Bugs are harder to debug

In **raw Node.js (`http` module)**:

- Uncaught errors can **stop the entire server**

------

## 🆚 Types of Errors

### 1️⃣ Client-Side Errors (4xx)

- The client did something wrong
- Examples:
  - Invalid URL
  - Missing fields in the request body
  - Wrong data type
  - Invalid JSON
- Common status codes:
  - `400` → Bad Request
  - `404` → Not Found

### 2️⃣ Server-Side Errors (5xx)

- The server failed even though the request was valid
- Examples:
  - Crashed while parsing data
  - Logic errors
  - Database failure
- Common status code:
  - `500` → Internal Server Error

------

## ⚙️ Core Error-Handling Practices (Node.js)

### 1️⃣ Always Validate Input

```
if (!newPokemon.name || !newPokemon.type) {
  res.statusCode = 400;
}
```

**Why:**

- Never trust incoming data
- Prevents bad data from entering your system

------

### 2️⃣ Protect Risky Operations with try/catch

```
try {
  JSON.parse(body);
} catch {
  res.statusCode = 400;
}
```

**Why:**

- `JSON.parse()` throws errors
- Without `try/catch`, your server **crashes**

------

### 3️⃣ Validate URL Parameters

```
if (isNaN(pokemonId)) {
  res.statusCode = 400;
}
```

**Why:**

- URLs are strings
- Prevents invalid IDs like `/pokemon/abc`

------

### 4️⃣ Always Send a Response

**Bad:**

```
// no res.end()
```

**Good:**

```
res.end(JSON.stringify({ message: "Error occurred" }));
```

**Why:**

- Requests should never hang
- Clients expect closure

------

### 5️⃣ Use Correct HTTP Status Codes

**Don’t do this:**

```
res.statusCode = 200;
res.end({ message: "Pokemon not found" });
```

**Do this:**

```
res.statusCode = 404;
res.end(JSON.stringify({ message: "Pokemon not found" }));
```

**Why:**

- Status codes communicate meaning
- Clients rely on them for logic

------

## 🧠 Questions to ask every request

Ask these questions for **every request**:

1. Is the request valid?
2. Does the resource exist?
3. Did something break on the server?
4. Did I send a response?

------

## ✅ Key Takeaway

Good error handling makes your server:

- Predictable
- Safe
- Professional

…even when things go wrong.