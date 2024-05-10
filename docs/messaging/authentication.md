---
sidebar_position: 5
---

# Authentication

The RTM SDK comes with built-in support for authentication. This ensures that only authenticated users can call into specific remote functions.

Setting up authentication with the RTM Server as easy as defining a single function. Here's an example to authenticate a user with a hard-coded password.

## Enable Authentication on Server

:::note
Consider using Firebase Admin SDK, Auth0, or similar for authentication. The example below does not represent any real-world scenarios.
:::

Provide the `authHandler` handler to the RtmServer options to enable authentication. The function parameters
will contain whatever data sent by the RtmClient when connecting (e.g. a token).

**Note: Any data returned by `authHandler` will be available to all `RtmRemoteClient` objects on RPC functions as `RtmRemoteClient.authentication`**. You can use this to retrieve and store additional user info about the connected clients. This is also useful in a multi-server setup where you need unique IDs of connected clients for sending events.

```typescript title="src/server.ts"
const server = new RtmServer(undefined, {
  port: 70,
  onOpen(port) {
    console.log("Server started on port " + port);
    resolve(server);
  },
  // Provide a callback for authHandler to enable authentication. Any data returned by this
  // function will be available on client.authentication object passed to RPC functions.
  authHandler(...params) {
    console.log(`Authenticating, input: ${JSON.stringify(params)}`);
    return params[0];
  },
  onAuthTimeout(connection) {
    console.log("A connection timed out authentication.");
  },
});
```

## Authenticating from Client Side

The RtmClientOptions allows you to pass authentication data (e.g. token) to the server that is processed by the `authHandler` function. Here's an example of client sending authentication data to the server:

```typescript title="src/client.ts"
const token = getIdToken();
client = new RtmClient("ws://localhost:70/", {
  // Pass authentication data to the server.
  authenticationData: token,

  // Delay between reconnect attempts when connection fails.
  reconnectDelayMs: 5000,
  // Callback for when the client is attempting to reconnect.
  onReconnecting(attempt) {
    console.log(`Trying to reconnect for ${attempt} time...`);
  },
  // Callback for when the client successfully connects.
  async onOpen() {
    console.log("Connected to the RTM server.");
    resolve(client);
  },

  // Callback for when the client has been disconnected.
  onClose() {
    console.log("Connection with the RTM server was closed.");
  },

  // Callback for errors during connection.
  onError() {
    console.log("Error connecting to the RTM server");
    reject();
  },
});
```
