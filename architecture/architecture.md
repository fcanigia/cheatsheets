# Architecture

## Traditional web apps vs SPAs (angular, reactjs, vue)
There are two general approaches to building web applications today: traditional web applications 
that perform most of the application logic on the server, and single-page applications (SPAs) that 
perform most of the user interface logic in a web browser, communicating with the web server 
primarily using web APIs. A hybrid approach is also possible, the simplest being host one or more rich 
SPA-like subapplications within a larger traditional web application.

Use traditional web applications when:
• Your application’s client-side requirements are simple or even read-only.
• Your application needs to function in browsers without JavaScript support.
• Your application is public-facing and benefits from search engine discovery and referrals.

Use a SPA when:
• Your application must expose a rich user interface with many features.
• Your team is familiar with JavaScript, TypeScript, or Blazor WebAssembly development.
• Your application must already expose an API for other (internal or public) clients.

## Common design principles

- separation of concers
- encapsulation
- dependency inversion
- explicit dependencies
- single responsibility
- DRY
- persistence ignorance
- bounded contexts

## Common web application architectures

### Monolithic

A monolithic application is one that is entirely self-contained, in terms of its behavior. It may interact 
with other services or data stores in the course of performing its operations, but the core of its 
behavior runs within its own process and the entire application is typically deployed as a single unit. If 
such an application needs to scale horizontally, typically the entire application is duplicated across 
multiple servers or virtual machines

### Traditional N-Layer architecture

These layers are frequently abbreviated as UI, BLL (Business Logic Layer), and DAL (Data Access Layer). 
Using this architecture, users make requests through the UI layer, which interacts only with the BLL. 
The BLL, in turn, can call the DAL for data access requests. The UI layer shouldn’t make any requests to 
the DAL directly, nor should it interact with persistence directly through other means. Likewise, the BLL 
should only interact with persistence by going through the DAL. In this way, each layer has its own 
well-known responsibility.

## Clean architecture (or hexagonal / ports and adapters / onion)

Clean architecture puts the business logic and application model at the center of the application. 
Instead of having business logic depend on data access or other infrastructure concerns, this 
dependency is inverted: infrastructure and implementation details depend on the Application Core. 
This functionality is achieved by defining abstractions, or interfaces, in the Application Core, which are 
then implemented by types defined in the Infrastructure layer. A common way of visualizing this 
architecture is to use a series of concentric circles, similar to an onion.

### Organizing code in Clean Architecture
In a Clean Architecture solution, each project has clear responsibilities. As such, certain types belong 
in each project and you’ll frequently find folders corresponding to these types in the appropriate 
project.

#### Application Core
The Application Core holds the business model, which includes entities, services, and interfaces. These 
interfaces include abstractions for operations that will be performed using Infrastructure, such as data 
access, file system access, network calls, etc. Sometimes services or interfaces defined at this layer will 
need to work with non-entity types that have no dependencies on UI or Infrastructure. These can be 
defined as simple Data Transfer Objects (DTOs).

##### Application Core types
• Entities (business model classes that are persisted)
• Aggregates (groups of entities)
• Interfaces
• Domain Services
• Specifications
• Custom Exceptions and Guard Clauses
• Domain Events and Handlers

#### Infrastructure
The Infrastructure project typically includes data access implementations. In a typical ASP.NET Core 
web application, these implementations include the Entity Framework (EF) DbContext, any EF Core 
Migration objects that have been defined, and data access implementation classes. The most common 
way to abstract data access implementation code is through the use of the Repository design pattern.
In addition to data access implementations, the Infrastructure project should contain implementations 
of services that must interact with infrastructure concerns. These services should implement interfaces 
defined in the Application Core, and so Infrastructure should have a reference to the Application Core 
project.

##### Infrastructure types
• EF Core types (DbContext, Migration)
• Data access implementation types (Repositories)
• Infrastructure-specific services (for example, FileLogger or SmtpNotifier)

#### UI Layer
The user interface layer in an ASP.NET Core MVC application is the entry point for the application. This 
project should reference the Application Core project, and its types should interact with infrastructure 
strictly through interfaces defined in Application Core. No direct instantiation of or static calls to the 
Infrastructure layer types should be allowed in the UI layer.

##### UI Layer types
• Controllers
• Custom Filters
• Custom Middleware
• Views
• ViewModels
• Startup
The Startup class or Program.cs file is responsible for configuring the application, and for wiring up 
implementation types to interfaces. The place where this logic is performed is known as the app’s 
composition root, and is what allows dependency injection to work properly at run time.

## Slice architecture

## Microservices
The concept of dividing server side features into many small servers that are responsible for only one or a few specific features.

Following our example, before we only had a single server responsible for all features (a monolithic architecture). After implementing microservices we'll have a server responsible for authentication, another responsible for payments, another for streaming, and the last one for recommendations.

### Backend for frontend

One problem that comes up when implementing microservices is that the communication with front-end apps gets more complex. Now we have many servers responsible for different things, which means front-end apps would need to keep track of that info to know who to make requests to.

Normally this problem gets solved by implementing an intermediary layer between the front-end apps and the microservices. This layer will receive all the front-end requests, redirect them to the corresponding microservice, receive the microservice response, and then redirect the response to the corresponding front-end app.

The benefit of the BFF pattern is that we get the benefits of the microservices architecture, without over complicating the communication with front-end apps.

## Web-Queue-Worker

The core components of this architecture are a web front end that serves 
client requests, and a worker that performs resource-intensive tasks, long-running workflows, or 
batch jobs. The web front end communicates with the worker through a message queue.