# Routing

## What is a Router?

- A  **router** is responsible for mapping incoming HTTP requests (like GET /home  or POST /login) to the appropriate controller actions.
- It acts as a traffic director, deciding which controller should handle a given request.


- **The  router interprets the HTTP request (URL, method, etc.) and forwards it to     the correct controller.**



**Is HTTP the Router?**

- No, HTTP is just a protocol that enables communication between clients (like     web browsers) and servers.

## Example : express.js-Node.js
```ts
const app = express();

// Router handling
app.get('/home', (req, res) => {
    res.send('Welcome Home!');
});

app.post('/login', (req, res) => {

    res.send('Logging in...');
});

app.listen(3000, () => console.log('Server running on port 3000'));

```

- Here, Express.js acts as the router.

- HTTP requests (GET /home, POST /login) are routed to appropriate handlers.
