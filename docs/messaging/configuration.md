---
sidebar_position: 2
---

# Usage

To enable real-time communication from clients to your backend application, you need to set up the RTM Server.

Here's a simple example of how to create an open RTM server.

## Basic RTM Server

```typescript title="src/server.ts"
function hello(client: RtmRemoteClient, ...data: any[]) {
  // This function is called from the remote client, and any defined value that this function will return
  // is sent back to the client.
  console.log("Hello, World!");
  return "hello world!";
}

function setupServer(): Promise<RtmServer> {
  return new Promise(async (resolve) => {
    const server = new RtmServer(undefined, {
      port: 70,
      onOpen(port) {
        console.log("Server started on port " + port);
        resolve(server);
      },
    });

    // Register RPC functions
    server.Register(hello);

    await server.Start();
  });
}

setupServer();
```

## Client-Side

The following code example demonstrates how you can utilize `@zexcore/rtm-client` library to connect and communicate with the server.

```typescript title="src/client.ts"
async function functionCall(client: RtmClient){
  // Use CallWait<T> to call a remote-defined function and return its result, casted to the specified type.
  const response = await client.CallWait<string>("hello"); // Make sure the function name is exact.
  console.log(response); // Prints 'hello world!'
}

function setupClient(): Promise<RtmClient> {
  return new Promise(async (resolve) => {
    client = new RtmClient("ws://localhost:70/", {
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
  }
}

setupClient().then(client=>functionCall(client));
```

### Fire & Forget Functions

To call remote functions without waiting for their resulting data, you can use `RtmClient.Call` function. This function simply invokes the registered function on the server without returning anything. Useful in cases where you only need to send data without any response.

```
await client.Call("hello"); // Will print `Hello, World!` on the server.
```
