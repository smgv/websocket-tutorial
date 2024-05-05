## What is WebSockets ?

1. WebSockets is a communication protocol that provides full-duplex communication channels over a single, long-lived TCP connection.
2. It's designed to be implemented in web browsers and web servers, enabling real-time data transfer between a client (such as a web browser) and a server.
3. Unlike traditional HTTP connections, which are stateless and involve initiating a new connection for each request-response cycle, WebSockets maintain a persistent connection, allowing for bi-directional, low-latency communication.

## How WebSockets Work ?

### 1. Handshake:

The WebSocket connection starts with a handshake between the client and the server. This handshake is similar to an HTTP handshake but includes specific headers indicating the desire to upgrade the connection to the WebSocket protocol.

### 2. Upgrade Request:

The client sends an upgrade request to the server, indicating that it wants to establish a WebSocket connection. This request includes the Upgrade header with the value "websocket", among other headers.

### 3. Upgrade Response:

If the server supports WebSockets and agrees to upgrade the connection, it responds with a 101 status code (Switching Protocols), indicating that the connection has been successfully upgraded to the WebSocket protocol. This response also includes additional headers to acknowledge the upgrade.

### 4. Persistent Connection:

Once the handshake is complete, the connection between the client and the server becomes persistent. This means that both parties can send data to each other at any time without needing to establish a new connection for each message.

### 5. Bi-directional Communication:

With the WebSocket connection established, both the client and the server can send data to each other asynchronously, in full-duplex mode. This means that data can flow in both directions simultaneously, unlike traditional HTTP connections, where the client sends a request and then waits for a response from the server.

### 6. Data Framing:

WebSocket messages are framed, meaning they are divided into smaller units for transmission. Each frame includes a header that specifies the message type and size, followed by the message payload. This framing allows for efficient data transmission over the WebSocket connection.

## Advantages of WebSockets:

### 1. Real-time Communication:

WebSockets enable real-time communication between clients and servers, allowing for instant updates and notifications without the need for constant polling.

### 2. Low Latency:

Because WebSocket connections are persistent and full-duplex, they can significantly reduce latency compared to techniques like polling or long-polling, where the client repeatedly requests updates from the server.

### 3. Efficient Resource Usage:

WebSockets require fewer server resources compared to techniques like polling, as they eliminate the need for frequent connection establishment and teardown.

### 4. Cross-Domain Support:

WebSockets support cross-domain communication, allowing clients and servers hosted on different domains to establish WebSocket connections.

### 5. Standardized Protocol:

The WebSocket protocol is standardized by the IETF, ensuring interoperability between different WebSocket implementations and providing a reliable foundation for real-time web applications.

## Projects:

### 1. [Basics](Basics)

Created a simple websocket server using nodejs 'ws' package.

1. clone the project
2. cd Basics && npm i
3. cd server && node index.js
4. open the index.html file on browser

In your IDE terminal you will see 'New client connected' message and than you will receive the message 'Client has sent us: Hey, How are you?'. after receiving the message serve will send the message to the client as Message Event object you can destructure the object and get the data from that.

#### 1. Created a Single Websocket Server which is running on port 8002.

```js
# server code
const WebSocket = require("ws");

const wss = new WebSocket.Server({ port: 8082 });
```

### 2. Listening to multiple clients.

1. 'connection' event listens to multiple clients calling the Websocket Server
2. 'message' event is called when client send some data to server.
3. 'send' event is called inside 'message' event listener if you want to send some data as response to client on receiving the data.
4. 'close' event is called when client close the tab or get out of the session.

```js
# server code
wss.on("connection", (ws) => {
  console.log("New client connected!");

  ws.on("message", (data) => {
    console.log(`Client has sent us: ${data}`);

    ws.send(data.toString().toUpperCase());
  });

  ws.on("close", () => {
    console.log("Client has disconnected!");
  });
});
```

### 3. Client Code

1. we create the connection by calling websocket server "ws://localhost:8082" if you are using the production server the url will start with 'wss' instead of 'ws'.
2. we add event listener for 'open' to check if we are connected or not. once we are connected we are calling websocket client send method to send the data to the client end.
3. we have added the 'message' event listener to check the data that we are getting from the server end.
4. data that we receive from server end is of 'Message Event Object'.

```js
 <script>
      const ws = new WebSocket("ws://localhost:8082");

      ws.addEventListener("open", () => {
        console.log("We are connected");

        ws.send("Hey, How are you?");
      });

      ws.addEventListener("message", (data) => {
        console.log(data);
      });
</script>
```
