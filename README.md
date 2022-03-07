# SystemDesignNotes

## Different Tiers
Think of a tier as both logical and physical separation of components in an application or a service. This separation is at a component level, not the code level.

Note: Don’t confuse tiers with the layers of the application. Some prefer to use them interchangeably. However, in the industry, application layers typically mean the user interface, business, service and the data access layers.

* In a **single-tier** application, the user interface, backend business logic, and the database reside in the same machine. Examples: MS Office, PC Games, image editing software like Gimp, Photoshop, etc.


* A **two-tier** application involves a client and a server. The client contains the user interface with the business logic in one machine. Meanwhile, the backend server includes the database running on a different machine. The database server is hosted by the business and has control over it.

* In a **three-tier** application, the user interface, business logic, and the database all reside on different machines and, thus, have different tiers. They are physically separated.

* An **n-tier** application is an application that has more than three components (user interface, backend server, database) involved in its architecture. n-tier applications are known as distributed systems.

### Benefits of n-tier systems
* **Single Responsibility Principle**: Each component has a signle job that does it well. Has a lot of flexibility and makes management easy. A team can develop their single component without affecting the rest of the system.
* **Seperation of Concerns**: Similar to above. Keeping components seperate makes them reusable. Having loosely coupled components is the way to go. This approach enables us to scale our service easily when things grow beyond a certain scale in the future.

## Web Architechture

Typical Web Application

![image](https://user-images.githubusercontent.com/13190696/157071322-deee1dc1-946c-405f-9af9-6e38411f0c96.png)

* The primary task of a **web server** is to receive the requests from the client and provide the response after executing the business logic based on the request parameters received from the client.

**REST API Benefits:** With the REST implementation, developers can have different implementations for different clients, leveraging different technologies with separate codebases. Different clients accessing a common REST API could be a mobile browser, a desktop browser, a tablet or an API testing tool. Introducing new types of clients or modifying the client code does not affect the functionality of the backend service. This means the clients and the backend service are decoupled.

### HTTP PULL/PUSH

For every response, there has to be a request first. The client sends the request and the server responds with the data. This is the default mode of HTTP communication, called the **HTTP PULL** mechanism. The client pulls the data from the server whenever required. It keeps doing this over and over to fetch the latest data. What if there is no updated data available on the server every time the client sends a request?

To tackle this, we have the **HTTP PUSH-based** mechanism. In this mechanism, the client sends the request for certain information to the server just once. After the first request, the server keeps pushing the new updates to the client whenever they are available.
* This is also known as a **callback**
* A common example of this is notifications

There are multiple technologies involved in the HTTP PUSH-based mechanism, such as:
* Ajax Long polling
* Web Sockets
* HTML5 Event Source
* Message Queues
* Streaming over HTTP

#### HTTP Pull - AJAX
**AJAX** stands for Asynchronous JavaScript and XML. AJAX Polling is HTTP Pull based, since it repeatedly sends requests for latest data. The name says it all. AJAX is used for adding asynchronous behavior to the web page.
* instead of requesting the data manually every time with the click of a button, AJAX enables us to fetch the updated data from the server by automatically sending the requests over and over at stipulated intervals.
* This dynamic technique of requesting information from the server at regular intervals is known as **polling.**

#### HTTP Push

A **persistent connection** is a network connection between the client and the server that remains open for future requests and responses, as opposed to being closed after a single communication. This facilitates HTTP PUSH-based communication between the client and the server.

![image](https://user-images.githubusercontent.com/13190696/157076870-20af71af-fb44-4ca1-b542-a552caa313b4.png)

The connection between the client and the server stays open with the help of **Heartbeat Interceptors**. These are just blank request responses between the client and the server to prevent the browser from killing the connection.

Persistent connections consume a lot of resources compared to the HTTP PULL behavior. However, there are use cases where establishing a persistent connection is vital to an application’s feature. But critical for some applications. For instance, a browser-based multiplayer game has a pretty large amount of request-response activity within a limited time than a regular web application.

A **Web Socket** connection is preferred when we need a persistent bi-directional low latency data flow from the client to the server and back.
* Typical use-cases of web sockets are messaging, chat applications, real-time social streams, browser-based massive multiplayer games, etc. These are apps with quite a significant number of read writes compared to a regular web app.
* Have bi-directional data? Go ahead with web sockets. One more thing, web sockets don’t work over HTTP. The mechanism runs over TCP. Also, the server and the client should both support web sockets. Else it won’t work.

**Long polling** lies somewhere between AJAX and Web Sockets. In this technique, instead of immediately returning the empty response, the server holds the response until it finds an update to be sent to the client.
* Long polling can be used in simple asynchronous data fetch use cases when you do not want to poll the server every now and then.

The **Server-Sent Events (SSE)** implementation takes a different approach. Instead of the client polling for data, the server automatically pushes the data to the client whenever the updates are available. The incoming messages from the server are treated as events.
* SSE is ideal for scenarios like a real-time Twitter feed, displaying stock quotes on the UI, real-time notifications, etc.
* https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events


**Streaming over HTTP** is ideal for cases where we need to stream extensive data over HTTP by breaking it into smaller chunks. This is made possible with HTML5 and a JavaScript Stream API.
* The technique is primarily used for streaming multimedia content, like large images, videos, etc., over HTTP. Empowered by this technique, we can watch a partially downloaded video as it downloads by playing the downloaded chunks on the client.
* https://developer.mozilla.org/en-US/docs/Web/API/Streams_API/Concepts


















