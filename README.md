# Api Gateway

## what is the problem without api gateway
- suppose there is 3-4 services niw these services will run on diff diff port while making an api call in frontend client may write diff diff port to fecth diff diff api which is very   inefficient, so there comes api gateway in picture
- In api gateway we make a common port on which api gateway server will be running now this gateway will direct the api to there respective services

## Architecture
```json
Client
   ↓
API Gateway (PORT 8080)
   ↓
User Service (3001)
Order Service (3002)
Payment Service (3003)
```

# Example of api gateway 

## Api gateway service
```javascript
const express = require("express");
const { createProxyMiddleware } = require("http-proxy-middleware");

const app = express();
const PORT = 8080;

/**
 * USER SERVICE
 * Client → /users → forwarded to user-service
 */
app.use(
  "/users",
  createProxyMiddleware({
    target: "http://localhost:3001",
    changeOrigin: true,
  })
);

/**
 * ORDER SERVICE
 */
app.use(
  "/orders",
  createProxyMiddleware({
    target: "http://localhost:3002",
    changeOrigin: true,
  })
);

/**
 * PAYMENT SERVICE
 */
app.use(
  "/payments",
  createProxyMiddleware({
    target: "http://localhost:3003",
    changeOrigin: true,
  })
);

app.listen(PORT, () => {
  console.log(`API Gateway running on port ${PORT}`);
});
```

##  Example Backend Services

### 👤 User Service (3001)
```javascript 
const express = require("express");
const app = express();

app.get("/users/:id", (req, res) => {
  res.json({ id: req.params.id, name: "Abhishek" });
});

app.listen(3001, () => {
  console.log("User Service running on 3001");
});
```
### 📦 Order Service (3002)
```javascript 
const express = require("express");
const app = express();

app.get("/orders/:id", (req, res) => {
  res.json({ orderId: req.params.id, item: "Laptop" });
});

app.listen(3002, () => {
  console.log("Order Service running on 3002");
});
```
### 💳 Payment Service (3003)
```javascript 
const express = require("express");
const app = express();

app.get("/payments/:id", (req, res) => {
  res.json({ paymentId: req.params.id, status: "SUCCESS" });
});

app.listen(3003, () => {
  console.log("Payment Service running on 3003");
});
```

## How Actually it works
 - There are 3 route in gateway one for user services one for order services and one for payment service
```javascript

app.use(
  "/users",
  createProxyMiddleware({
    target: "http://localhost:3001",
    changeOrigin: true,
  })
);

what this peace of code does

For every request that starts with /users, the API Gateway forwards that request to the User Service running on port 3001.

changeOrigin: true

Gateway changes Host header before forwarding:
Host: localhost:8080 changes to localhost:3001
example all these 

localhost:8080:users
localhost:8080:users/1
localhost:8080:users/123/profile
localhost:8080:users?active=true

***all these users start with users/ so these request header will be changed to localhost:3001***

#### Similarly for all the services









```
