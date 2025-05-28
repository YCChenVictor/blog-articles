# Title

#### Design

Let's say we want to build a task management interface following RESTful API (mostly used by web API). Then the design will be as follow:

```javascript
GET /tasks/new – Show form to create a task
POST /tasks – Create a new task
GET /tasks – Get list of tasks
GET /tasks/:id – Get a task by ID
GET /tasks/:id/edit – Show form to edit task
PUT /tasks/:id – Fully update a task
PATCH /tasks/:id – Partially update a task
DELETE /tasks/:id – Delete a task
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
