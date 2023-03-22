![logo](images/c-sharp-logo.png)

## Where to practice coding?
- www.leetcode.com
- www.exercism.com
- www.codewars.com
- www.hackerrank.com

## Table of Content

- [Common](#common)
  * [String vs string builder](#string-vs-string-builder)
  * [Access Modifiers](#access-modifiers)
  * [Override vs overload](#override-vs-overload)
  * [Generics](#generics)
  * [Delegates](#delegates)
  * [Action vs function](#action-vs-function)
  * [Events](#events)
  * [Anon data type vs regular data type](#anon-data-type-vs-regular-data-type)
  * [Why .net Core?](#why-net-core)
    + [Characteristics](#characteristics)
  * [IQueryable vs IEnumerable](#iqueryable-vs-ienumerable)
  * [Single/First/FirstOrDefault](#single-first-firstordefault)
    + [Semantical Difference](#semantical-difference)
    + [Performance Difference](#performance-difference)
    + [Conclusion](#conclusion)
  * [Struct vs class](#struct-vs-class)
  * [Heap and stack](#heap-and-stack)
  * [Authentication methods](#authentication-methods)
  * [Property binding](#property-binding)
  * [Value type vs reference type](#value-type-vs-reference-type)
  * [Deffered vs immmediate execution](#deffered-vs-immmediate-execution)
  * [Hosted services](#hosted-services)
  * [Dependency injection](#dependency-injection)
    + [Lifecycle](#lifecycle)
      - [AddTransient](#addtransient)
      - [AddScoped](#addscoped)
      - [AddSingleton](#addsingleton)
  * [Middleware](#middleware)
  * [Middleware: Map, MapWhen, Use, Run](#middleware)
  * [Async/await](#async-await)
  * [Threads](#threads)
  * [Readonly vs constants](#readonly-vs-constants)
  * [Private function in interface](#private-function-in-interface)
  * [Abstract class sealed](#abstract-class-sealed)
  * [Linq let](#linq-let)
- [EF](#ef)
  * [How does EF prevents sql injection?](#how-does-ef-prevents-sql-injection)
  * [Types of inheritance in EF](#types-of-inheritance-in-ef)
- [Data types](#data-types)
  * [Linked list](#linked-list)
  * [Stack](#stack)
  * [Array](#array)
- [Patterns](#patterns)
  * [Unit of work](#unit-of-work)
  * [CRQS](#crqs)
  * [Repository pattern](#repository-pattern)
- [API](#api)
  * [Verboses](#verboses)
    + [POST](#post)
    + [GET](#get)
    + [PUT](#put)
    + [PATCH](#patch)
    + [DELETE](#delete)

## Common

### String vs string builder
The string is an immutable type in C#, which means it can’t be changed once it’s been created. StringBuilder, on the other hand, is mutable, which means that if an operation is performed on the string object, it won’t construct a new instance in memory every time, unlike string.

### [Access Modifiers](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/access-modifiers)
- *public*: The type or member can be accessed by any other code in the same assembly or another assembly that references it. The accessibility level of public members of a type is controlled by the accessibility level of the type itself.
- *private*: The type or member can be accessed only by code in the same class or struct.
- *protected*: The type or member can be accessed only by code in the same class, or in a class that is derived from that class.
- *internal*: The type or member can be accessed by any code in the same assembly, but not from another assembly. In other words, internal types or members can be accessed from code that is part of the same compilation.
- *protected internal*: The type or member can be accessed by any code in the assembly in which it's declared, or from within a derived class in another assembly.
- *private protected*: The type or member can be accessed by types derived from the class that are declared within its containing assembly.

| Caller's location                      | public | protected internal | protected | internal | private protected | private |
|----------------------------------------|:------:|:------------------:|:---------:|:--------:|:-----------------:|:-------:|
| Within the class                       |    ✔️️   |          ✔️         |     ✔️     |     ✔️    |         ✔️         |    ✔️    |
| Derived class (same assembly)          |    ✔️   |          ✔️         |     ✔️     |     ✔️    |         ✔️         |    ❌    |
| Non-derived class (same assembly)      |    ✔️   |          ✔️         |     ❌     |     ✔️    |         ❌         |    ❌    |
| Derived class (different assembly)     |    ✔️   |          ✔️         |     ✔️     |     ❌    |         ❌         |    ❌    |
| Non-derived class (different assembly) |    ✔️   |          ❌         |     ❌     |     ❌    |         ❌         |    ❌    |

### Override vs overload

Overloading is the ability to have multiple methods within the same class with the same name, but with different parameters.

public class Calculator
{
    public int Add(int a, int b) => a + b;
    public int Add(int a, int b, int c) => a + b +c;
    public double Add(double a, double b) => a + b;
    public double Add(double a, double b, double c) => a + b + c;
} 

Overriding, on the other hand, is the ability to redefine the implementation of a method in a class that inherits from a parent class.

### [Generics](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/types/generics)

Generic means the general form, not specific. In C#, generic means not specific to a particular data type.

C# allows you to define generic classes, interfaces, abstract classes, fields, methods, static methods, properties, events, delegates, and operators using the type parameter and without the specific data type. A type parameter is a placeholder for a particular type specified when creating an instance of the generic type.

A generic type is declared by specifying a type parameter in an angle brackets after a type name, e.g. TypeName<T> where T is a type parameter.

### [Delegates](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/delegates)

C# delegates are similar to pointers to functions, in C or C++. A delegate is a reference type variable that holds the reference to a method. The reference can be changed at runtime.

Delegates are especially used for implementing events and the call-back methods. All delegates are implicitly derived from the System.Delegate class.

### [Events](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/events/)
An event is a notification sent by an object to signal the occurrence of an action. Events in .NET follow the observer design pattern.

The class who raises events is called Publisher, and the class who receives the notification is called Subscriber. There can be multiple subscribers of a single event. Typically, a publisher raises an event when some action occurred. The subscribers, who are interested in getting a notification when an action occurred, should register with an event and handle it.

In C#, an event is an encapsulated delegate. It is dependent on the delegate. The delegate defines the signature for the event handler method of the subscriber class.

### [Anon data type vs regular data type](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/types/anonymous-types)
An anonymous type is a type (class) without any name that can contain public read-only properties only. It cannot contain other members, such as fields, methods, events, etc.
    
> var student = new { Id = 1, FirstName = "James", LastName = "Bond" };    

### Diff between a.equals(b) and a == b
The equality operator == checks whether two operands are equal or not, and the Object.Equals() method checks whether the two object instances are equal or not.

Internally, == is implemented as the operator overloading method, so the result depends on how that method is overloaded. In the same way, Object.Equals() method is a virtual method and the result depends on the implementation. For example, the == operator and .Equals() compare the values of the two built-in value types variables. if both values are equal then returns true; otherwise returns false.    

### Why .net Core?
There are  some limitations with the .NET Framework. For example, it only runs on the Windows platform. Also, you need to use different .NET APIs for different Windows devices such as Windows Desktop, Windows Store, Windows Phone, and Web applications. In addition to this, the .NET Framework is a machine-wide framework. Any changes made to it affect all applications taking a dependency on it.
    
Today, it's common to have an application that runs across devices; a backend on the web server, admin front-end on windows desktop, web, and mobile apps for consumers. So, there is a need for a single framework that works everywhere. So, considering this, Microsoft created .NET Core. The main objective of .NET Core is to make .NET Framework open-source, cross-platform compatible that can be used in a wide variety of verticals, from the data center to touch-based devices.
    
#### Characteristics
- Open-source framework
- Cross platfrom
- Consistent across architectures
- Wide range of applications
- Supports multiple languages
- Modular architecture
- CLI tools
- Flexible deployment
- Containerizable

### IQueryable vs IEnumerable

We use IEnumerable and IQueryable to manipulate the data that is retrieved from database. IQueryable inherits from IEnumerable, so IQueryable does contain all the IEnumerable features. The major difference between IQueryable and IEnumerable is that IQueryable executes query with filters whereas IEnumerable executes the query first and then it filters the data based on conditions.

**Find more detailed differentiation below :**

**IEnumerable**

- IEnumerable exists in the System.Collections namespace
- IEnumerable execute a select query on the server side, load data in-memory on a client-side and then filter data
- IEnumerable is suitable for querying data from in-memory collections like List, Array
- IEnumerable is beneficial for LINQ to Object and LINQ to XML queries

**IQueryable**

- IQueryable exists in the System.Linq namespace
- IQueryable executes a 'select query' on server-side with all filters
- IQueryable is suitable for querying data from out-memory (like remote database, service) collections
- IQueryable is beneficial for LINQ to SQL queries

So IEnumerable is generally used for dealing with in-memory collection, whereas, IQueryable is generally used to manipulate collections.    
    
### Single/First/FirstOrDefault

#### Semantical Difference:

- FirstOrDefault returns a first item of potentially multiple (or default if none exists).
- SingleOrDefault assumes that there is a single item and returns it (or default if none exists). Multiple items are a violation of contract, an exception is thrown.

#### Performance Difference

- FirstOrDefault is usually faster, it iterates until it finds the element and only has to iterate the whole enumerable when it doesn't find it. In many cases, there is a high probability to find an item.
- SingleOrDefault needs to check if there is only one element and therefore always iterates the whole enumerable. To be precise, it iterates until it finds a second element and throws an exception. But in most cases, there is no second element.

#### Conclusion

- Use FirstOrDefault if you don't care how many items there are or when you can't afford checking uniqueness (e.g. in a very large collection). When you check uniqueness on adding the items to the collection, it might be too expensive to check it again when searching for those items.
- Use SingleOrDefault if you don't have to care about performance too much and want to make sure that the assumption of a single item is clear to the reader and checked at runtime.

In practice, you use First / FirstOrDefault often even in cases when you assume a single item, to improve performance. You should still remember that Single / SingleOrDefault can improve readability (because it states the assumption of a single item) and stability (because it checks it) and use it appropriately.   
    
### [Struct vs class](https://learn.microsoft.com/en-us/dotnet/standard/design-guidelines/choosing-between-class-and-struct)
Structs are light versions of classes. Structs are value types and can be used to create objects that behave like built-in types.

Structs share many features with classes but with the following limitations as compared to classes.

- Struct cannot have a default constructor (a constructor without parameters) or a destructor.
- Structs are value types and are copied on assignment.
- Structs are value types while classes are reference types.
- Structs can be instantiated without using a new operator.
- A struct cannot inherit from another struct or class, and it cannot be the base of a class. All structs inherit directly from System.ValueType, which inherits from System.Object.
- Struct cannot be a base class. So, Struct types cannot abstract and are always implicitly sealed.
- Abstract and sealed modifiers are not allowed and struct member cannot be protected or protected internals.
- Function members in a struct cannot be abstract or virtual, and the override modifier is allowed only to the override methods inherited from System.ValueType.
- Struct does not allow the instance field declarations to include variable initializers. But, static fields of a struct are allowed to include variable initializers.
- A struct can implement interfaces.
- A struct can be used as a nullable type and can be assigned a null value.
    
|  S.N |  Struct                                                                                                                                                                                     |  Classes                                                                                                                               |
|------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
|  1   |  Structs are value types, allocated either on the stack or inline in containing types.                                                                                                      | Classes are reference types, allocated on the heap and garbage-collected.                                                              |
|  2   |  Allocations and de-allocations of value types are in general cheaper than allocations and de-allocations of reference types.                                                               |  Assignments of large reference types are cheaper than assignments of large value types.                                               |
|  3   | In structs, each variable contains its own copy of the data (except in the case of the ref and out parameter variables), and an operation on one variable does not affect another variable. |  In classes, two variables can contain the reference of the same object and any operation on one variable can affect another variable. |
    
In this way, struct should be used only when you are sure that,
- It logically represents a single value, like primitive types (int, double, etc.).
- It is immutable.
- It should not be boxed and un-boxed frequently.
In all other cases, you should define your types as classes.    
    
### Heap and stack
https://www.c-sharpcorner.com/article/C-Sharp-heaping-vs-stacking-in-net-part-i/#:~:text=The%20Stack%20is%20more%20or,on%20top%20of%20the%20next.
    
### [Action vs function](https://medium.com/@serhat21zor/c-action-vs-func-62fe917da43f#:~:text=Actions%20can%20only%20take%20input,value%20in%20the%20other%20words.&text=The%20following%20example%20and%20screenshot%20show%20an%20action%20definition.)
Actions can only take input parameters, while Funcs can take input and output parameters. It is the biggest difference between them.  

An action can not return a value, while a func should return a value in the other words.  

### Authentication methods (ASP.Net)
 
ASP.NET default authentication Providers

1. Form Authentication: With cookies or passing user details in the query string.
2. Passport Authentication: Using MS Passport service
3. Windows Authentication
4. Custom authentication Provider  
 4.1 Multipass/SSO  
 4.2 JWT  
 4.3 SAML  
 
[Link1](https://medium.com/fabrit-global/authentication-methods-in-c-3ef83c9df043) - [Link2](https://www.loginradius.com/blog/engineering/alternate-authentication-asp/)

### Property binding

.Net MVVM binding.
 
[Link 1](https://learn.microsoft.com/en-us/archive/msdn-magazine/2016/july/data-binding-a-better-way-to-implement-data-binding-in-net) 
 
### Value type vs reference type

A Value Type stores its contents in memory allocated on the stack. When you created a Value Type, a single space in memory is allocated to store the value and that variable directly holds a value. If you assign it to another variable, the value is copied directly and both variables work independently. Predefined datatypes, structures, enums are also value types, and work in the same way. Value types can be created at compile time and Stored in stack memory, because of this, Garbage collector can't access the stack.
 
Reference Types are used by a reference which holds a reference (address) to the object but not the object itself. Because reference types represent the address of the variable rather than the data itself, assigning a reference variable to another doesn't copy the data. Instead it creates a second copy of the reference, which refers to the same location of the heap as the original value. Reference Type variables are stored in a different area of memory called the heap. This means that when a reference type variable is no longer used, it can be marked for garbage collection. Examples of reference types are Classes, Objects, Arrays, Indexers, Interfaces etc.

[Link 1](http://net-informations.com/faq/general/valuetype-referencetype.htm) 
 
### Deffered vs immmediate execution - Linq
 
```C#
  var query = (from e in empTable where e.Age > 45 select new { e.Name });
 
  foreach (var item in query)
  {
   DoSomething()
  }
```

The query is actually executed when the query variable is iterated over, not when the query variable is created. This is called *deferred execution*.
 
 ```C#
  var query = (from e in empTable where e.Age > 45 select new { e.Name }).Count();
```
 
The basic difference between a Deferred execution vs Immediate execution is that Deferred execution of queries produce a sequence of values, whereas Immediate execution of queries return a singleton value and is executed immediately. Examples are using Count(), Average(), Max() etc.
 
 To force immediate execution of a query that does not produce a singleton value, you can call the ToList(), ToDictionary() or the ToArray() method on a query or query variable. These are called conversion operators which allow you to make a copy/snapshot of the result and access is as many times you want, without the need to re-execute the query.

[Link 1](https://www.linkedin.com/pulse/immediate-vs-deferred-execution-linq-pawan-verma/)

### Hosted services

In ASP.NET Core, background tasks can be implemented as hosted services. A hosted service is a class with background task logic that implements the IHostedService interface. 
 
[Link 1](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/host/hosted-services?view=aspnetcore-7.0&tabs=visual-studio)
 
### [Dependency injection](https://learn.microsoft.com/en-us/dotnet/core/extensions/dependency-injection)

#### Lifecycle

##### AddTransient
    
Transient lifetime services are created each time they're requested from the service container. This lifetime works best for lightweight, stateless services. 
Register transient services with AddTransient. 
In apps that process requests, transient services are disposed at the end of the request.
    
##### AddScoped
    
For web applications, a scoped lifetime indicates that services are created once per client request (connection). Register scoped services with AddScoped. In apps that process requests, scoped services are disposed at the end of the request. When using Entity Framework Core, the AddDbContext extension method registers DbContext types with a scoped lifetime by default. By default, in the development environment, resolving a service from another service with a longer lifetime throws an exception. For more information, see Scope validation.
    
#####  AddSingleton
Singleton lifetime services are created either:

- The first time they're requested.
- By the developer, when providing an implementation instance directly to the container. This approach is rarely needed.    
    
Every subsequent request of the service implementation from the dependency injection container uses the same instance. If the app requires singleton behavior, allow the service container to manage the service's lifetime. Don't implement the singleton design pattern and provide code to dispose of the singleton. Services should never be disposed by code that resolved the service from the container. If a type or factory is registered as a singleton, the container disposes the singleton automatically.

Register singleton services with AddSingleton. Singleton services must be thread safe and are often used in stateless services.

In apps that process requests, singleton services are disposed when the ServiceProvider is disposed on application shutdown. Because memory is not released until the app is shut down, consider memory use with a singleton service.
    
    
### [Middleware](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/middleware/?view=aspnetcore-7.0)
Middleware is software that's assembled into an app pipeline to handle requests and responses. Each component:

- Chooses whether to pass the request to the next component in the pipeline.  
- Can perform work before and after the next component in the pipeline.  
    
Request delegates are used to build the request pipeline. The request delegates handle each HTTP request.

Request delegates are configured using Run, Map, and Use extension methods. An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class. These reusable classes and in-line anonymous methods are middleware, also called middleware components. Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the pipeline. When a middleware short-circuits, it's called a terminal middleware because it prevents further middleware from processing the request.    

![middleware](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/middleware/index/_static/request-delegate-pipeline.png)

### [Middleware: Map, MapWhen, Use, Run](https://www.codeproject.com/Tips/1069790/Understand-Run-Use-Map-and-MapWhen-to-Hook-Middl-2)
*Map*: Map extensions are used as convention for branching the pipeline. We can hook delegate to Map extension to push it to HTTP pipeline. Map simply accepts a path and a function that configures a separate middleware pipeline. In this example, we will hook one middleware/delegate to HTTP pipeline using Map extension.  

*Use*: In case of Use extension, there is a chance to pass next invoker, so that HTTP request will be transferred to the next middleware after execution of current Use if there next invoker is present.
    
*Run*: The nature of Run extension is to short circuit the HTTP pipeline immediately. It is a shorthand way of adding middleware to the pipeline that does not call any other middleware which is next to it and immediately return HTTP response. So, it’s recommended to use Run extension to hook middleware at last in HTTP pipeline.

### Async/await

### Threads

A thread is defined as the execution path of a program. Each thread defines a unique flow of control. If your application involves complicated and time consuming operations, then it is often helpful to set different execution paths or threads, with each thread performing a particular job.

Threads are lightweight processes. One common example of use of thread is implementation of concurrent programming by modern operating systems. Use of threads saves wastage of CPU cycle and increase efficiency of an application.

So far we wrote the programs where a single thread runs as a single process which is the running instance of the application. However, this way the application can perform one job at a time. To make it execute more than one task at a time, it could be divided into smaller threads.

#### Thread Life Cycle
The life cycle of a thread starts when an object of the System.Threading.Thread class is created and ends when the thread is terminated or completes execution.

Following are the various states in the life cycle of a thread:

- The Unstarted State − It is the situation when the instance of the thread is created but the Start method is not called.  
- The Ready State − It is the situation when the thread is ready to run and waiting CPU cycle.  
- The Not Runnable State − A thread is not executable, when
   - Sleep method has been called  
   - Wait method has been called  
   - Blocked by I/O operations  
- The Dead State − It is the situation when the thread completes execution or is aborted. 
 
[Link 1](https://www.tutorialspoint.com/csharp/csharp_multithreading.htm) - [Link 2](https://learn.microsoft.com/en-us/dotnet/api/system.threading.thread?view=net-7.0) - [Link 3](https://www.c-sharpcorner.com/article/Threads-in-CSharp/)  

### Readonly vs constants

### Private function in interface

### Abstract class sealed

### Linq let

## EF

### How does EF prevents sql injection?

### Types of inheritance in EF

## Data types

### Linked list

### Stack

### Array

## Patterns

### Unit of work
### CRQS
### Repository pattern

## API

### Verboses
| HTTP Verb | CRUD           | Entire Collection (e.g. /customers)                                                                  | Specific Item (e.g. /customers/{id})                                       |
|-----------|----------------|------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------|
| POST      | Create         | 201 (Created), 'Location' header with link to /customers/{id} containing new ID.                     | 404 (Not Found), 409 (Conflict) if resource already exists..               |
| GET       | Read           | 200 (OK), list of customers. Use pagination, sorting and filtering to navigate big lists.            | 200 (OK), single customer. 404 (Not Found), if ID not found or invalid.    |
| PUT       | Update/Replace | 405 (Method Not Allowed), unless you want to update/replace every resource in the entire collection. | 200 (OK) or 204 (No Content). 404 (Not Found), if ID not found or invalid. |
| PATCH     | Update/Modify  | 405 (Method Not Allowed), unless you want to modify the collection itself.                           | 200 (OK) or 204 (No Content). 404 (Not Found), if ID not found or invalid. |
| DELETE    | Delete         | 405 (Method Not Allowed), unless you want to delete the whole collection—not often desirable.        | 200 (OK). 404 (Not Found), if ID not found or invalid.                     |

#### POST
The POST verb is most-often utilized to **create** new resources. In particular, it's used to create subordinate resources. That is, subordinate to some other (e.g. parent) resource. In other words, when creating a new resource, POST to the parent and the service takes care of associating the new resource with the parent, assigning an ID (new resource URI), etc.

On successful creation, return HTTP status 201, returning a Location header with a link to the newly-created resource with the 201 HTTP status.

POST is neither safe nor idempotent. It is therefore recommended for non-idempotent resource requests. Making two identical POST requests will most-likely result in two resources containing the same information.

##### Examples:

POST http://www.example.com/customers  
POST http://www.example.com/customers/12345/orders  
    
#### GET
The HTTP GET method is used to **read** (or retrieve) a representation of a resource. In the “happy” (or non-error) path, GET returns a representation in XML or JSON and an HTTP response code of 200 (OK). In an error case, it most often returns a 404 (NOT FOUND) or 400 (BAD REQUEST).

According to the design of the HTTP specification, GET (along with HEAD) requests are used only to read data and not change it. Therefore, when used this way, they are considered safe. That is, they can be called without risk of data modification or corruption—calling it once has the same effect as calling it 10 times, or none at all. Additionally, GET (and HEAD) is idempotent, which means that making multiple identical requests ends up having the same result as a single request.

Do not expose unsafe operations via GET—it should never modify any resources on the server.

##### Examples:

GET http://www.example.com/customers/12345
GET http://www.example.com/customers/12345/orders
GET http://www.example.com/buckets/sample
    
#### PUT
PUT is most-often utilized for **update** capabilities, PUT-ing to a known resource URI with the request body containing the newly-updated representation of the original resource.

However, PUT can also be used to create a resource in the case where the resource ID is chosen by the client instead of by the server. In other words, if the PUT is to a URI that contains the value of a non-existent resource ID. Again, the request body contains a resource representation. Many feel this is convoluted and confusing. Consequently, this method of creation should be used sparingly, if at all.

Alternatively, use POST to create new resources and provide the client-defined ID in the body representation—presumably to a URI that doesn't include the ID of the resource (see POST below).

On successful update, return 200 (or 204 if not returning any content in the body) from a PUT. If using PUT for create, return HTTP status 201 on successful creation. A body in the response is optional—providing one consumes more bandwidth. It is not necessary to return a link via a Location header in the creation case since the client already set the resource ID.

PUT is not a safe operation, in that it modifies (or creates) state on the server, but it is idempotent. In other words, if you create or update a resource using PUT and then make that same call again, the resource is still there and still has the same state as it did with the first call.

If, for instance, calling PUT on a resource increments a counter within the resource, the call is no longer idempotent. Sometimes that happens and it may be enough to document that the call is not idempotent. However, it's recommended to keep PUT requests idempotent. It is strongly recommended to use POST for non-idempotent requests.

##### Examples:

PUT http://www.example.com/customers/12345  
PUT http://www.example.com/customers/12345/orders/98765  
PUT http://www.example.com/buckets/secret_stuff  

#### PATCH
PATCH is used for **modify** capabilities. The PATCH request only needs to contain the changes to the resource, not the complete resource.

This resembles PUT, but the body contains a set of instructions describing how a resource currently residing on the server should be modified to produce a new version. This means that the PATCH body should not just be a modified part of the resource, but in some kind of patch language like JSON Patch or XML Patch.

PATCH is neither safe nor idempotent. However, a PATCH request can be issued in such a way as to be idempotent, which also helps prevent bad outcomes from collisions between two PATCH requests on the same resource in a similar time frame. Collisions from multiple PATCH requests may be more dangerous than PUT collisions because some patch formats need to operate from a known base-point or else they will corrupt the resource. Clients using this kind of patch application should use a conditional request such that the request will fail if the resource has been updated since the client last accessed the resource. For example, the client can use a strong ETag in an If-Match header on the PATCH request.

##### Examples:

PATCH http://www.example.com/customers/12345  
PATCH http://www.example.com/customers/12345/orders/98765  
PATCH http://www.example.com/buckets/secret_stuff  

#### DELETE
DELETE is pretty easy to understand. It is used to **delete** a resource identified by a URI.

On successful deletion, return HTTP status 200 (OK) along with a response body, perhaps the representation of the deleted item (often demands too much bandwidth), or a wrapped response (see Return Values below). Either that or return HTTP status 204 (NO CONTENT) with no response body. In other words, a 204 status with no body, or the JSEND-style response and HTTP status 200 are the recommended responses.

HTTP-spec-wise, DELETE operations are idempotent. If you DELETE a resource, it's removed. Repeatedly calling DELETE on that resource ends up the same: the resource is gone. If calling DELETE say, decrements a counter (within the resource), the DELETE call is no longer idempotent. As mentioned previously, usage statistics and measurements may be updated while still considering the service idempotent as long as no resource data is changed. Using POST for non-idempotent resource requests is recommended.

There is a caveat about DELETE idempotence, however. Calling DELETE on a resource a second time will often return a 404 (NOT FOUND) since it was already removed and therefore is no longer findable. This, by some opinions, makes DELETE operations no longer idempotent, however, the end-state of the resource is the same. Returning a 404 is acceptable and communicates accurately the status of the call.

##### Examples:

DELETE http://www.example.com/customers/12345  
DELETE http://www.example.com/customers/12345/orders  
DELETE http://www.example.com/bucket/sample  
