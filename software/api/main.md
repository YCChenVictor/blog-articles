# Title

## Purpose

An API (Application Programming Interface) is a set of rules and protocols that allows different software applications to communicate and interact with each other. It defines the methods, data formats, and rules for exchanging information between applications. With API, developers can access the functionality of one system within another system, without having to know the details of how the underlying system works.

## Concept

### Web APIs

These APIs are designed to enable communication between different web-based applications. They allow developers to access and manipulate resources and data over the internet using standardized protocols such as HTTP (Hypertext Transfer Protocol).

* Purpose: The purpose of using APIs is to enable communication and interaction between different software applications, facilitating interoperability, modularity, and ease of integration. APIs abstract away complexity, standardize communication protocols, and promote scalability and innovation in software development.
* Usage: When you make a request to an API, you're asking for specific information or action. The request typically includes an HTTP method (GET, POST, PUT, DELETE, etc.), a URL (Uniform Resource Locator) specifying the resource or action, and any necessary parameters or data. The API server processes the request and sends back a response, which includes the requested data or information.
  * The concept of CRUD (create, read, update, delete) maps to POST, GET, PATCH/PUT, DELETE in [HTTP]({{site.baseurl}}/internet/2021/04/09/hypertext-transfer-protocol.html).
* Format: APIs often use specific data formats for representing and exchanging information, with the most common formats being JSON and XML. JSON is lightweight, human-readable, and machine-parsable, while XML offers greater flexibility and extensibility despite being more verbose.
  * [JSON API]({{site.baseurl}}/api/2022/08/16/json-api.html) is a specific set of conventions and guidelines for designing APIs that use JSON as the data format.
* Authentication: APIs often require [authentication]({{site.baseurl}}/api/2022/08/17/authentication.html) to ensure that only authorized users or applications can access certain resources or perform specific actions. Common authentication methods include API keys, OAuth, and token-based authentication.

### Library/APIs
  
These are collections of pre-written functions and methods that provide specific functionalities and can be utilized by developers within their applications.

* Usage: Developers typically import the library/API into their projects and utilize its functions or methods according to the provided documentation.
* Format: APIs can be available as standalone libraries, modules, or packages, which developers include in their projects through dependency management systems like npm for JavaScript, pip for Python, or Maven for Java.
* Example: Axios is a popular library API in JavaScript that simplifies making HTTP requests and handling responses, allowing developers to easily communicate with web servers and retrieve data.

### Operating System APIs

Operating systems provide APIs that allow developers to interact with the underlying system resources and services.

* Usage: Developers can utilize operating system APIs to perform various tasks such as file management, process control, memory allocation, and hardware interaction within their applications. APIs are typically accessed through programming languages like C, C++, or through language-specific wrappers or libraries.
* Format: Operating system APIs are typically provided as libraries or sets of header files that developers include in their source code. They consist of functions, data structures, and constants defined according to the operating system's specifications.
* Example: Windows API or POSIX API for Unix-like systems.

### RESTful API

#### Design

Let's say we want to build a task management interface following RESTful API (mostly used by web API). Then the design will be as follow:

```javascript
GET /tasks/new // (Create Show) Show the place to create task
POST /tasks // (Create) Add a new task to the list
GET /tasks // (Read list) Retrieve a list of all tasks
GET /tasks/{id} // (Read one) Retrieve a task with the specified ID
GET /tasks/:id/edit // (Update Show) Show the place to update task
PUT /tasks/{id} // (Update a task) Update a task with the specified ID
PATCH /tasks/{id} // (Update a task partially) Update a task with the specified ID
DELETE /tasks/{id} // (Destroy one) Delete a task with the specified ID
```

The URL structure follows the RESTful pattern of using a noun (in this case, “tasks”) to represent a resource and the HTTP method (GET, POST, PUT, DELETE) to specify the action to be taken on that resource.

#### REST Principles

REST means Representational State Transfer and is an architectural style with the following features:

* Client–Server Architecture
  * Enforces separation of concerns, enhancing portability and scalability.
* Statelessness
  * The current state is independent of the previous state, promoting flexibility in user interactions.
* Cache-ability
  * Utilizes caching to store data locally, reducing the need for repeated server requests.
* Layered System
  * Employs multiple intermediary servers for load balancing, security enhancement, and improved response generation.
* Code on Demand (Optional)
  * Allows optional code execution on the client, improving extensibility.
* Uniform Interface
  * Resource Identification in Requests: Separates resources from their representations.
  * Resource Manipulation through Representations: Users interact with representations (e.g., HTML forms) instead of directly changing server data.
  * Self-Descriptive Messages: Clients can parse resources based on received representations.
  * Hypermedia as the Engine of Application State (HATEOAS): Users can dynamically discover available resources after entering an initial URL.

## What

### node

[node api]({{site.baseurl}}/node/2022/01/26/api.html)

### rails

* POST /events -> /events/create -> events#create
* GET /events/1 -> /events/show/1 -> events#show
* PATCH /events/1 -> /events/update/1 -> events#update
* DELETE /events/1 -> /events/destroy/1 -> events#destroy

HTML now can only use GET and POST; as a result, rails adds params, method=PATCH and method=DELETE in POST for PATCH and DELETE.

refer to [rails API]({{site.baseurl}}/rails/2022/02/05/routes.html)

### curl

refer to [terminal commands]

### Jenkins API

[Jenkins API]({{site.baseurl}}/api/2022/09/12/jenkins-api.html) with node

## references

[表现层状态转换](https://zh.wikipedia.org/wiki/%E8%A1%A8%E7%8E%B0%E5%B1%82%E7%8A%B6%E6%80%81%E8%BD%AC%E6%8D%A2)

https://ihower.tw/rails/restful.html

[REpresentational State Transfer](https://en.wikipedia.org/wiki/Representational_state_transfer)