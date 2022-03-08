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

#### Client-sdie vs Server-side Rendering

The server-side rendering approach is perfect for delivering static content, such as WordPress blogs. It’s also good for SEO because the crawlers can easily read the generated content.

However, modern websites are highly dependent on AJAX. On such websites, content for a particular module or a page section has to be fetched and rendered on the fly. In this use case, the server-side rendering doesn’t help much.
* we can leverage a hybrid approach to get the best of both worlds. We can use server-side rendering for the static content of our website and client-side rendering for dynamic content.

## Scalability

### What is Scalability and Latency?

**Latency** is measured as the time difference between the action that a user takes on the website and the system’s response in reaction to that action. The action can be an event like clicking a button, scrolling down a web page, etc.

To cut down the network latency, businesses use a CDN (Content Delivery Network) to deploy their servers across the globe as close to the end-user as possible. These close to the user locations are also known as **Edge locations.**

If you intend to run the code in a distributed environment, it needs to be **stateless.** There should be no state in the code. What do I mean by this?

Also, whatever data static variables hold, it’s not application-wide. For this reason, distributed memory like Redis, Memcache, etc., are used to maintain a consistent state application-wide. 

Microservices Architecture

![image](https://user-images.githubusercontent.com/13190696/157093854-dc8eb241-3099-4989-a939-6c40ddb23d38.png)

### Primary Bottlenecks

* **Database**
* **Application Design**: For example, not using asynchronous processes. if a user uploads a document on the portal, tasks such as sending a confirmation email to the user, sending a notification to all subscribers/listeners to the upload event should be done asynchronously.Tasks like these should be forwarded to a messaging server or a task queue for asynchronous processing as opposed to being processed sequentially, making the user wait.
* **Not using cacheing wisely**: If the system has a lot of static data, caching can bring down the deployment costs significantly.
* **Inefficient configuration and setup of load balancers**
* **Having business logic in databse**
* **Not picking the correct databse**: Need transactions and strong consistency? Pick a relational database. If you can do without strong consistency rather than need horizontal scalability, pick a NoSQL database.
* **Code-level**

## High Availability

### Fault Tolerance
* An elementary example of a system like this is social networking platforms. In the case of backend node failures, a few services of the app, such as image upload, post likes, etc., may stop working. However, the application as a whole will still be up. This approach technically is also known as fail soft.

There are many upsides of splitting a big monolith into several microservices:
* Easy management and maintenance
* Ease of development
* Ease of adding new features to a service without affecting other services
* Scalability and high availability of the system

Every microservice takes the onus of running different features of an application such as photo upload, comment system, instant messaging, groups, marketplace, etc. In this case, even if a few services go down, the other services of the application are still up.

### Redundancy

This instance setup approach is also known as the **Active-passive HA mode**. An initial set of nodes are active, and a set of redundant nodes are passive, on standby. Active nodes get replaced by passive nodes in case of failures.

Systems should be well monitored in real-time to detect any bottlenecks or single point of failures. Automation enables the instances to self-recover without any human intervention. It gives the instances the power of self-healing.

Also, the systems become intelligent enough to add or remove instances on the fly as per the requirements. **Kubernetes** is one good example of this.

Since the most common cause of failures is human error, automation helps cut down failures considerably.

### High AVailability Clustering

A high availability cluster, also known as the fail-over cluster, contains a set of nodes running in conjunction with each other that ensures the high availability of the service. The nodes in the cluster are connected by a private network called the **heartbeat network** that continuously monitors the health and the status of each node in the cluster.

A single state across all the nodes in a cluster is achieved with the help of a shared distributed memory and a distributed coordination service like the Zookeeper.

## Load Balancing

They can also be set up at the application component level to efficiently manage traffic directed towards any application component, be it the backend application server, database component, message queue, or any other. This is done to uniformly spread the request load across the machines in the cluster powering that particular application component.

To ensure that the user request is always routed to the machine that is up and running, load balancers regularly perform health checks on the machines in the cluster.

### DNS

Four key components, or a group of servers, make up the DNS infrastructure. These are:
* DNS Recursive nameserver aka DNS Resolver
* Root nameserver
* Top-Level Domain nameserver
* Authoritative nameserver

![image](https://user-images.githubusercontent.com/13190696/157281142-d0569859-7787-4038-a3d3-ada61fcc4341.png)

DNS Process:
* User sends hostname to DNS Resolver (Recursive Nameserver)
* DNS Resolver routes to Root Nameserver
* Root Nameserver returns address of Toplevel Domain Server (.com server) to the DNS Resolver
* DNS REsolver then sends request to Toplevel Domain Server, which returns address for Authoritative Nameserver for that hostname
* Finally, Request gets routed to authoritative server, which returns IP Address

![image](https://user-images.githubusercontent.com/13190696/157282481-04096f48-ce77-4d89-ab0d-dd03a8098cfb.png)

### DNS Load Balancing
Occurs at the Authoritative Server. Instead of returning one IP address, it returns a list of IP address to multiple servers for the website. With every request, the authoritative server changes the order of the IP addresses in the list in a round-robin fashion.

Also, when the client hits an IP, it may not necessarily hit an application server. Instead, it may hit another load balancer implemented at the data center level that manages the clusters of application servers.

For instance, it does not take into account the current load on the servers, the content they hold, their request processing time, their in-service status, and so on.

Also, since these IP addresses are cached by the client’s machine and the DNS Resolver, there is always a possibility of a request being routed to a machine that is out of service.

DNS load balancing, despite its limitations, is preferred by companies because it is an easy and less expensive way of setting up load balancing on their services.

### Load Balancing Methods

There are primarily three modes of load balancing:
* DNS Load Balancing
* Hardware-based Load Balancing
* Software-based Load Balancing

Hardware-based ones are generally more proformant, but have less flexibility, harder to work with, for work to integrate. Software load-balancers are usually preferred.

HAProxy is one example of a software load balancer widely used by the big guns in the industry, including GitHub, Reddit, Instagram, AWS, Tumblr, StackOverflow, to scale their systems.

### Algorithms/Traffic routing approaches

* **Round robin and weighted round robin**: Can give servers with more computing power a higher weight
* **Least connections:** One approach is to simply route to server with least open connections. Another is to also consider CPU utilization/request processing time of chosen machine. THis is because the server with the least connections could be doing more intensive work. The first approach can be good for gaming, where there are generally long open connections
* **Random**
* **Hash:** In this approach, the source IP where the request is coming from and the request URL are hashed to route the traffic to the backend servers. Hasing the source IP ensures that requests from a given source IP are routed to the same server. THis is good becasue server already has source data in local memory. Hashing a URL ensures that the request with that URL always hits a certain cache that already has data on it. This is to ensure that there is no cache miss.

## Monolith and Microservices

A monolithic application is a self-contained, tightly coupled software application. This is unlike the microservices architecture, where every distinct feature of an application may have one or more dedicated microservices powering it.

Monolithic application is easier to build at first. But it has significant costs associated with converting to distributed microservices application. Linkedin had to do this.

### Microservices

![image](https://user-images.githubusercontent.com/13190696/157292467-083bc509-2173-4717-b32f-53c3aaad3ddd.png)








