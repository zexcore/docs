---
sidebar_position: 3
---

# Events

The RTM SDK provides support for sending and receiving events. For example, when a user sends a message to another user on your platform, you may want to send the event to the recipient user from your server without them having to explicitly request for the messages. This event can also include any data that can be serialized as JSON.

Following examples demonstrate how to send an event to all the connected clients from the server side, and receive them on the clients.

**Note: The example below uses `Emit` function that works only on a single-server setup. If you have multiple server instances, use `Publish` to send the events to clients over Redis**

## Sending Events

You can use `RtmServer.Emit` to send events to clients connected to a single server, or `RtmClient.Publish` to send event to a specific user across multiple server instances. The use case depends on the kind of the application you are building.

### Via Single Server Configuration (without Redis)

```typescript title="src/server.ts"
// ... Configure and Start server as 'server'
server.Emit("alert", (client) => true, "Hello from server!");

// You can also filter the clients based on their connection information, such as authentication data.
// See authentication page for details.
server.Emit(
  "alert",
  (client) => client.authentication.email === "someone@example.com",
  "Hello, client!"
);
```

### Via Multi-Server Configuration (with Redis)

Multi-server events are only supported on an Authentication-enabled RTM Server. With authentication, it is important for your clients to have unique user identifiers, which are used by the underlying SDK to deliver the messages. See Authentication page for details.

```typescript title="src/server.ts"
// ... Configure and Start server as 'server'
server.Publish("uuid", "alert", "Hello from server!");
```

## Receiving / Subscribing to Events

The `@zexcore/rtm-client` library also allows you to **Subscribe** to events sent from the server. The code below demonstrates how to subscribe to a specific event and read its data.

```typescript title="src/client.ts"
// ... Configure and Connect with server as 'client'
client.Subscribe("eventName", (data: any[]) => {
  // Callback that is executed when the event is received.
  console.log(data[0]); // Will print `hello from server!`
});
```
