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


Every microservice interacts with each other via a REST API gateway interface. A system with microservices can leverage the polyglot persistence architecture and other heterogeneous technologies like Java, Python, Ruby, NodeJS, etc.

Polyglot persistence uses multiple database types, like SQL and NoSQL, together in the architecture. We will discuss this in detail in the database lesson.

**Microservices Challenges:**

* Complex system management. We need to set up additional components to manage microservices, such as a node manager like Apache Zookeeper, a distributed tracing service for monitoring the nodes, etc
* Sometimes, strong consistency is hard to guarantee in a distributed environment, especially when trying to achieve a single consistent state across several microservices.

**When to use Microservices architecture**
* A typical social networking application has various components such as messaging, real-time chat, LIVE video streaming, photo uploads, post like and share features, etc.
* This use case fits best for a microservices architecture. Also, the microservices architecture enables a business move fast. A business can separately develop, test, deploy an application feature without affecting the current features. There is not much need for regression testing and so on.


So, by now, we have seen three ways to proceed with the design of our application:

* Picking a monolithic architecture
* Picking a microservice architecture
* Starting with a monolithic architecture and later scaling out into a microservice architecture.

**Why Microservices Architecture**:
* Fault isolation
* Development TEam Automony

**Cons of Microservices**:
* Increased management over due to architechture complexity
* For example, company Segment went from moonolithic -> microservices -> back to monolithic, since system became too complex.
* https://segment.com/blog/goodbye-microservices/
* https://blog.christianposta.com/microservices/istio-as-an-example-of-when-not-to-do-microservices/

## Micro Frontends
**Micro frontends** are distinct loosely coupled components of an application’s user interface developed applying the concept of microservices to the frontend.

Normally, even if system uses microservices on the backend, the frontend is a monolith:

![image](https://user-images.githubusercontent.com/13190696/157506628-d0efb4f7-3e1e-496e-84a1-9ddcc442e188.png)

With the micro frontends approach, we split our application into vertical slices, where a single slice goes end to end right from the user interface to the database. And every slice is owned by a dedicated team.

![image](https://user-images.githubusercontent.com/13190696/157506705-1c446a6a-60ad-4968-99f8-5de7583ce92f.png)

Micro Frontends are popular with e-commerce websites

**Example - Online Game Store:**

Our online gaming store will have several different UI components. A few of the key components would be:
* The Search Component
* Add to cart and checkout component
* The payment component
* The game category component

The smaller components that integrate into other pages/components of the application are known as fragments.
![image](https://user-images.githubusercontent.com/13190696/157508984-338f8495-a26d-495a-a5fc-2b404ef2bc72.png)

**Benefits of Micro FrontEnds**
* Easier coordination between the frontend and the backend devs
* Can use different technologies for different components. If we want to use Vue for a component, we don't have to rewrite everything.
* Best fit for medium to large websites

![image](https://user-images.githubusercontent.com/13190696/157509555-7445d7a8-2457-4469-b569-8850a817446b.png)

https://engineering.atspotify.com/2014/03/spotify-engineering-culture-part-1/


### Integrating MIcro Frontends

Once we have respective micro frontends ready for our online game store, we need to integrate them together to have a functional website. There are two ways we can do this:
1. By integrating micro frontends on the client
2. By integrating micro frontends on the server

Similar to client-side vs server-side rendering

#### Client-side

**Links**
* Each micro frontend has a unique link.
* Ex, when in checkout page, you are on checkout frontend. When you clink link to payment frontend, the url changes and you are using payment frontend

**Within same page**
* Also, following this basic approach, the micro frontends that need to be integrated within a specific page can be done using Iframes.

* Using Links and Iframes is not recommended. Bad for SEO. They can't be bookmarked. Stability and performance issues.

A recommended way to integrate components on the client-side is by leveraging a technology called the Web Components and frameworks such as Single SPA.
Single SPA is a JavaScript framework for frontend microservices that enables developers to build their frontend while leveraging different JavaScript frameworks.

#### Server-side
When the UI components are integrated on the server, on the user request, the complete pre-built page of the website is delivered to the client from the server as opposed to sending individual micro frontends to the client and having them clubbed there.

ome of the popular frameworks that facilitate server-side integration of micro frontends are:
* Project Mosaic
* Open Components
* Podium

Zolando Company uses Project Mosaic: https://www.youtube.com/watch?v=m32EdvitXy4

## Database

A database is a component in application architecture required to persist data. Data can be of many forms: structured, unstructured, semi-structured, and user state data.

### Types of Data

* **Structured data** is the type of data that conforms to a certain structure, typically stored in a database in a normalized fashion. Structured data is typically managed by a query language such as SQL (Structured query language).
* **Unstructured Data**: Unstructured data has no definite structure. It is the heterogeneous type of data consisting of text, image files, videos, multimedia files, pdfs, blob objects, word documents, machine-generated data, etc. We cannot directly interact with unstructured data. The initial data collected is pretty raw. We have to make it flow through a data preparation stage that segregates it based on business logic and then analytics algorithms are run to extract meaningful information.

![image](https://user-images.githubusercontent.com/13190696/157513314-4f6bd274-6190-4967-8194-351a71134a0c.png)

* **Semi-structured Data**: Semi-structured data is a mix of structured and unstructured data. This data is often stored in data transport formats such as XML, JSON and handled as per the business requirements.
* **User-state Data**: User state data is the data containing the information of activity the user performs on the website.

### Relational Database

A **relational database** persists data containing relationships: one to one, one to many, many to many, many to one, etc. It is the most widely used type of database in web development.

Relational databases have a relational data model, data is organized in tables having rows and columns and SQL is the primary data query language used to interact with relational databases.

MySQL is an example of a relational database.

**Example:** Imagine you buy five different books from an online bookstore. When you create an account at the bookstore, the system will assign you a customer id say C1. Now, C1 will be linked to five different books B1, B2, B3, B4, and B5. This is a **one-to-many relationship**. In the simplest of forms, one database table will contain the details of all the customers and another table will contain all the products in the inventory.One row in the customer table will correspond to multiple rows in the product inventory table. Upon pulling the user object with the id C1 from the database, we can easily find what books C1 purchased via the relationship model.

Besides the relationships, relational databases also ensure saving data in a **normalized fashion**. In very simple terms, normalized data means an entity occurs in only one place/table in its simplest and atomic form and is not spread throughout the database.

Besides normalization and consistency, relational databases also ensure **ACID transactions.** ACID stands for atomicity, consistency, isolation and durability.

Fabebook uses relational database: https://www.scaleyourapp.com/what-database-does-facebook-use-a-1000-feet-deep-dive/

**Populat Relational Databases**
* MySQL, an open-source relationship database written in C and C++, has been around since 1995
* PostgreSQL, an open-source RDBMS written in C.
* Microsoft SQL Server, a proprietary RDBMS written by Microsoft in C and C++.
* MariaDB, Amazon Aurora, Google Cloud SQL, etc.

### NoSQL Databases

As the name implies, NoSQL databases have no SQL; they are more like JSON-based databases built for Web 2.0.

Some of the popular NoSQL databases used in the industry are MongoDB, Redis, Neo4J, Cassandra, Memcache, etc.

* NoSQL databases are built for high-frequency read writes, typically required in social applications like micro-blogging, real-time sports apps, online massive multiplayer games, and so on.
* Are **Scalable**. Can add new server nodes on the fly. Relational databases require sharding and replication.
* Run intelligently on clusters (minimal human intervention).
* The data with NoSQL databases is more eventually consistent as opposed to being strongly consistent.

#### NoSQL Pros

Learning Curve Not so steep
* With relational, we need to be very focused on designing the schema to minimize joins and aboid issues.
* NoSQL has not strictly enforced schemas
* No/limited relationships

#### NoSQL Cons
* Since data is not normalized, there is risk of inconsistent data
* No support for ACID transactions. Claims often have limitations and stipulations

### Time Series DB

Some of the popular time-series databases used in the industry are **Influx DB, Timescale DB, Prometheus, etc.**

Examples:
* IBM uses Influx DB to run analytics for real-time cognitive fraud detection.
* Spiio uses Influx DB to remotely monitor vertical lining green walls and plant installations.

### Wide-Column Databases

Some of the popular wide-column databases are **Cassandra, HBase, Google BigTable, ScyllaDB, etc.**

* Netflix uses Cassandra in the analytics infrastructure.
* Adobe and other big guns use HBase for processing large amounts of data.

## Caching

Strategies:

* Cache Aside: Most common. Good for read heavy. Data has TTL
* Read-though: Similar to above, except data alyways stays consistent with database. The cache framework takes onus of maintaining consistency.
* Write-through: Every write goes through the cache before updating the database. Adds latency to write operations. Good for needing strict data consistency between cache and DB.
* Write-back: Data is directly written to the cache instead of the database, and the cache, after some delay, as per the business logic, writes data to the database.

## Message Queues

Message queues facilitate asynchronous behavior.  Asynchronous behavior allows the modules to communicate in the background without hindering their primary tasks.

* **Publish-subscribe:** There are different types of exchanges available in message queues, some of which are: direct, topic, headers, and fanout. The fanout exchange will fit best for implementing a pub-sub pattern. It will push the messages to the queue and the consumers will receive the message broadcast. The relationship between the exchange and the queue is known as binding.
* **Point-to-Point:** In a point-to-point model, a message from the producer is consumed by only one consumer. There are two popular protocols when working with message queues: AMQP Advanced Message Queue Protocol and STOMP Simple or Streaming Text Oriented Message Protocol. Every messaging technology, RabbitMQ, ActiveMQ, Apache Kafka, will have its own implementations of these protocols.

* https://www.scaleyourapp.com/linkedin-real-time-architecture-how-does-linkedin-identify-its-users-online/
* https://engineering.fb.com/2015/12/03/ios/under-the-hood-broadcasting-live-video-to-millions/





