WebSocket is a communication protocol that enables full-duplex communication channels over a single TCP connection. It allows real-time data exchange between a client (like a web browser) and a server.
###### Components
- Client
- WS server
- Message Broker (optional)
###### Flow
1. Client establishes connection ==(client side)==
2. Sending messages ==(server side)==
3. Handling messages  ==(server side)==
4. Broadcasting messages ==(server side)==
5. Receive messages  ==(client side)==
``` c
+------------+                +------------------+
|  Clients   | <---------->   |WebSocket Server  |
| (User 1,   |  WebSocket     |(Spring Boot App) |
|  User 2,   |   Connection   |                  |
|  User 3)   |                +------------------+
+------------+                       |
          |                          |
          |  Send Message to         |
          |     /app/send            |stompClient.send("/app/send", {}, messageInput.value);
          +--------------------------->|
                                       |
                 +---------------------+
                 | WebSocket Controller|
                 |(Handles Messages)   |
                 +---------------------+
                            |
                            | @SendTo("/topic/public")
                            V
                 +---------------------+
                 |   Message Broker     |
                 | (Routes to /topic/public)|
                 +---------------------+
                            |
                            | Broadcasts to all Clients
                            V
                 +------------+
                 |  Clients   |
                 | (Receive   |
                 |  Messages)  |
                 +------------+

```
##### Implementation in spring boot One Public channel
- create a new Spring Boot project
- Add WebSocket Dependencies
```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-websocket</artifactId>
</dependency>
```
- Configure WebSocket
```java
import org.springframework.context.annotation.Configuration;
import org.springframework.messaging.simp.config.MessageBrokerRegistry;
import org.springframework.web.socket.config.annotation.EnableWebSocketMessageBroker;
import org.springframework.web.socket.config.annotation.StompEndpointRegistry;
import org.springframework.web.socket.config.annotation.WebSocketMessageBrokerConfigurer;

@Configuration
@EnableWebSocketMessageBroker
public class WebSocketConfig implements WebSocketMessageBrokerConfigurer {
    @Override
    public void configureMessageBroker(MessageBrokerRegistry config) {
        // Enable a simple in-memory message broker here boadcast the messages
        config.enableSimpleBroker("/topic");
        // Prefix for messages from clients to server
        config.setApplicationDestinationPrefixes("/app");
    }

    @Override
    public void registerStompEndpoints(StompEndpointRegistry registry) {
        // Register the "/ws" endpoint for WebSocket connections 
        //here client first request for connection using http call and new session created
        registry.addEndpoint("/ws").withSockJS();
    }
}

```

- Controller
```java
import org.springframework.messaging.handler.annotation.MessageMapping;
import org.springframework.messaging.simp.SimpMessagingTemplate;
import org.springframework.stereotype.Controller;

@Controller
public class WebSocketController {
    private final SimpMessagingTemplate messagingTemplate;

    public WebSocketController(SimpMessagingTemplate messagingTemplate) {
        this.messagingTemplate = messagingTemplate;
    }

    @MessageMapping("/send")  // Maps messages to /app/send
    public void sendMessage(String message) {
        // Send the message to all subscribers of /topic/messages
        messagingTemplate.convertAndSend("/topic/messages", message);
    }

    
    @MessageMapping("/send")// Maps messages to /app/send
    @SendTo("/topic/message")  
    public void sendMessage(String message) {
        // Send the message to all subscribers of /topic/messages
      return message;
    }
}

```

- Client 
```html
<!DOCTYPE html>
<html>
<head>
    <title>WebSocket Example</title>
    <script src="https://cdn.jsdelivr.net/npm/sockjs-client/dist/sockjs.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/@stomp/stompjs/lib/stomp.min.js"></script>
</head>
<body>
    <h1>WebSocket Example</h1>
    <input type="text" id="message" placeholder="Type a message" />
    <button onclick="sendMessage()">Send</button>

    <div id="messages"></div>

    <script>
        const socket = new SockJS('/ws');
        const stompClient = Stomp.over(socket);

        stompClient.connect({}, function (frame) {
            console.log('Connected: ' + frame);
            stompClient.subscribe('/topic/messages', function (greeting) {
                // Display received messages
                const messageDiv = document.getElementById('messages');
                messageDiv.innerHTML += '<p>' + greeting.body + '</p>';
            });
        });

        function sendMessage() {
            const messageInput = document.getElementById('message');
            stompClient.send("/app/send", {}, messageInput.value);
            messageInput.value = ''; // Clear input after sending
        }
    </script>
</body>
</html>

```


##### WebSocket Session Life Cycle
###### Client Connection
- A client (e.g., a web browser) initiates a WebSocket connection to a specified endpoint (e.g., `/ws`) through an HTTP handshake.
- If the handshake is successful, the connection is upgraded to WebSocket.
###### Session Creation:
- Spring automatically creates a unique session for each connection.
- Each session is assigned a unique `sessionId`, which is managed by Spring, so you don't need to create sessions manually. If there is n number of user then spring create n number of session which have different Id.
###### Message Sending and Receiving:
- Once connected, the client can send messages to the server using STOMP or directly over WebSocket.
- Each message sent includes the `sessionId`, allowing the server to identify the source session.
###### Handling Messages:
- Use methods annotated with `@MessageMapping` to handle incoming messages.
- Access the `sessionId` in your message handler through `SimpMessageHeaderAccessor`.
###### Disconnection:
- When a client disconnects, Spring automatically cleans up the session.
- You can listen for disconnection events to perform additional cleanup or notifications.




steps
- `npm install sockjs-client stompjs`
- Do not use vite instead use react-create-app works fine
