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


