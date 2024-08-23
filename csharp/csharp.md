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
  * [Dynamic](#dynamic)
  * [throw vs throw ex](#throw-vs-throw-ex)
  * [Garbage collector](#garbage-collector)
- [EF](#ef)
  * [EF vs Dapper](#EF-vs-Dapper) 
  * [How does EF prevents sql injection?](#how-does-ef-prevents-sql-injection)
  * [Types of inheritance in EF](#types-of-inheritance-in-ef)
- [Data types](#data-types)
  * [Linked list](#linked-list)
  * [Stack](#stack)
  * [Array](#array)
- [Patterns](#patterns)
  * [Unit of work](#unit-of-work)
  * [CRQS](#crqs)
  * [Repository pattern](#repository)
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

```C#
public class Calculator
{
    public int Add(int a, int b) => a + b;  
    public int Add(int a, int b, int c) => a + b +c;  
    public double Add(double a, double b) => a + b;  
    public double Add(double a, double b, double c) => a + b + c;  
} 
```

Overriding, on the other hand, is the ability to redefine the implementation of a method in a class that inherits from a parent class.

### [Generics](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/types/generics)

Generic means the general form, not specific. In C#, generic means not specific to a particular data type.

C# allows you to define generic classes, interfaces, abstract classes, fields, methods, static methods, properties, events, delegates, and operators using the type parameter and without the specific data type. A type parameter is a placeholder for a particular type specified when creating an instance of the generic type.

A generic type is declared by specifying a type parameter in an angle brackets after a type name, e.g. TypeName<T> where T is a type parameter.

### [Delegates](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/delegates)

C# delegates are similar to pointers to functions, in C or C++. A delegate is a reference type variable that holds the reference to a method. The reference can be changed at runtime.

Delegates are especially used for implementing events and the call-back methods. All delegates are implicitly derived from the System.Delegate class.

```C#
// Define a delegate type with a signature that matches the method it will point to
delegate void MyDelegate(string message);

class Program
{
    static void Main()
    {
        // Create an instance of the delegate and point it to a method
        MyDelegate delegateInstance = new MyDelegate(PrintMessage);

        // Invoke the delegate, which in turn invokes the method it points to
        delegateInstance("Hello, delegates!");

        // Alternatively, you can use delegate inference for simpler syntax
        MyDelegate delegateInferred = PrintMessage;
        delegateInferred("Delegates with inference!");

        // Multicast delegates: combining multiple methods into one delegate
        MyDelegate multiDelegate = PrintMessage;
        multiDelegate += PrintAnotherMessage;
        multiDelegate("Multicast delegates!");

        // Remove a method from a multicast delegate
        multiDelegate -= PrintMessage;
        multiDelegate("After removing PrintMessage method");

        // Using anonymous methods with delegates
        MyDelegate anonymousDelegate = delegate (string msg)
        {
            Console.WriteLine($"Anonymous method: {msg}");
        };
        anonymousDelegate("Using anonymous methods");

        // Using lambda expressions with delegates
        MyDelegate lambdaDelegate = msg => Console.WriteLine($"Lambda expression: {msg}");
        lambdaDelegate("Using lambda expressions");

        Console.ReadLine();
    }

    static void PrintMessage(string message)
    {
        Console.WriteLine($"Message from PrintMessage method: {message}");
    }

    static void PrintAnotherMessage(string message)
    {
        Console.WriteLine($"Message from PrintAnotherMessage method: {message}");
    }
}
```

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

### Dynamic

Used to declare variables whose types are resolved at runtime rather than compile time. This means that the type of a dynamic variable is determined dynamically based on the operations performed on it during runtime. 

```
dynamic dynamicVar;

dynamicVar = 10;  // Assigning an integer
Console.WriteLine(dynamicVar + 5);  // Output: 15 (dynamicVar is treated as an integer)

dynamicVar = "Hello";  // Assigning a string
Console.WriteLine(dynamicVar.Length);  // Output: 5 (dynamicVar is treated as a string with a Length property)

dynamicVar = new List<int> { 1, 2, 3 };  // Assigning a list
Console.WriteLine(dynamicVar.Count);  // Output: 3 (dynamicVar is treated as a list with a Count property)
```

#### Keys
1. Dynamic Typing: With dynamic, you can work with objects whose types are not known until runtime. This is useful in scenarios where you need to interact with dynamically-typed languages, COM objects, or when dealing with data whose structure is not known at compile time.

2. Late Binding: dynamic allows for late binding, which means that method calls, property accesses, and other operations on dynamic variables are resolved at runtime. This is in contrast to static typing, where such operations are resolved at compile time.

3. Dynamic Operations: You can perform various operations on dynamic variables, such as method calls, property accesses, arithmetic operations, and more, without requiring explicit type casting or conversion.

4. Type Inference: The type of a dynamic variable is inferred based on the operations performed on it. For example, if you assign an integer value to a dynamic variable and then perform arithmetic operations on it, the variable is treated as an integer.

5. Dynamic Language Interoperability: dynamic is often used when working with dynamic languages like JavaScript, Python, or when interacting with dynamic APIs, COM objects, or reflection-based scenarios.

6. No Compile-Time Type Checking: Since dynamic variables are resolved at runtime, there is no compile-time type checking for operations performed on them. This can lead to runtime errors if the operations are not supported by the actual type of the object.

#### Uses
1. Interacting with dynamic languages: When working with dynamic languages like JavaScript through interoperability frameworks such as COM interop or the Dynamic Language Runtime (DLR), using dynamic allows you to work with objects whose structure is not known at compile time.

2. Parsing JSON or XML: When deserializing JSON or XML data into C# objects, using dynamic can simplify the process, especially when dealing with nested or dynamic structures where the schema may vary.

3. Working with dynamic APIs: Some APIs or libraries may return objects whose types are determined at runtime or are not known until runtime. Using dynamic allows you to work with these objects without explicitly specifying their types.

4. Dynamic method invocation: In scenarios where you need to invoke methods on objects whose types are not known until runtime, dynamic can be used to call methods dynamically without compile-time type checking.

5. Duck typing: When implementing duck typing-like behavior in C#, where you want to invoke methods or access properties on objects based on their capabilities rather than their specific types, dynamic can be helpful.

6. Reducing verbosity: In some cases, using dynamic can lead to cleaner and more concise code, especially when dealing with complex object hierarchies or when the exact type information is not necessary.

### Throw vs Throw ex

In C#, the difference between using throw and throw ex inside a catch block is significant in terms of how the exception's stack trace is preserved and presented.
throw:

- Re-throws the current exception without altering the stack trace.
- The original stack trace, which shows where the exception was first thrown, is preserved. This is useful for debugging because you can trace the exact sequence of method calls that led to the exception.

```C#
try
{
    // Some code that might throw an exception
}
catch (Exception ex)
{
    // Handle or log the exception
    throw; // Re-throws the original exception
}
```

throw ex:

- Throws the caught exception again but resets the stack trace.
- The stack trace will now begin from the point where throw ex is called, which can make it harder to trace back to the original source of the exception.

```C#
try
{
    // Some code that might throw an exception
}
catch (Exception ex)
{
    // Handle or log the exception
    throw ex; // Re-throws the exception, resetting the stack trace
}
```

Which One to Use?

- Use throw when you want to maintain the original stack trace, which is almost always the preferred approach because it provides more accurate debugging information.
- Use throw ex only if you have a specific reason to reset the stack trace, which is uncommon and generally discouraged.

Preserving the original stack trace makes it easier to identify where the issue originated, so throw is generally the better practice.

### Garbage collector

Garbage collection (GC) in C# is an automatic memory management system provided by the .NET runtime (CLR - Common Language Runtime). Its main purpose is to manage the allocation and release of memory for your application, ensuring that objects that are no longer in use are properly cleaned up and that memory is efficiently utilized.

#### Key Concepts of Garbage Collection in C#

1. Managed Heap:
 - When an object is created in C#, it is allocated memory on the managed heap. The managed heap is divided into three generations: Generation 0, Generation 1, and Generation 2.

3. Generations:
 - Generation 0: This is where newly created objects are allocated. Since most objects are short-lived, many of them are collected in Generation 0.
 - Generation 1: Objects that survive the first garbage collection (i.e., those that are still referenced) are moved to Generation 1.
 - Generation 2: Objects that survive multiple garbage collections move to Generation 2. These objects are usually long-lived.

4. GC Process:
 - Allocation: When you create a new object, memory is allocated for it in Generation 0.
 - Mark Phase: The garbage collector identifies which objects are still in use by examining the application’s roots (e.g., static fields, local variables, CPU registers). These objects are marked as "live."
 - Sweep Phase: Objects that are not marked as live (i.e., those that are no longer referenced) are considered unreachable and their memory is reclaimed.
 - Compact Phase: The heap is compacted to reduce fragmentation, meaning that the memory occupied by dead objects is reclaimed, and live objects are moved together to make the free space contiguous.

5. GC Modes:
 - Workstation GC: Optimized for client applications with a single CPU.
 - Server GC: Optimized for server applications, making use of multiple threads for GC operations.

6. Finalization and IDisposable:
 - If an object implements a finalizer (destructor), the garbage collector will call it before reclaiming the memory. Finalization is non-deterministic, meaning you cannot predict when the finalizer will run.
 - Implementing the IDisposable interface allows you to manually release unmanaged resources (e.g., file handles, database connections) using the Dispose method. This is usually done in a using statement to ensure resources are cleaned up as soon as they are no longer needed.

7. Generational Hypothesis:
 - The garbage collector operates on the assumption that most objects die young (i.e., they are short-lived). Therefore, it optimizes by focusing more on collecting Generation 0, which usually results in a quick and efficient cleanup.

8. Concurrent and Background GC:
 - Concurrent GC: Runs garbage collection concurrently with the application's execution to minimize pause times.
 - Background GC: A variation of concurrent GC for server applications that minimizes the time the application is paused for garbage collection.

9. Large Object Heap (LOH):
 - Objects that are 85,000 bytes or larger are allocated on the Large Object Heap. The LOH is not compacted as frequently as the other generations to avoid the performance cost of moving large objects.

#### Summary

**Automatic Memory Management**: The garbage collector automatically handles memory allocation and reclamation, freeing developers from manual memory management.
**Generational Collection**: The garbage collector works in generations, focusing more on collecting younger objects, which are more likely to be short-lived.
**Efficient and Optimized**: The GC process is optimized to minimize the impact on application performance, especially with concurrent and background garbage collection modes.
**Finalization and IDisposable**: Developers can influence the GC process by implementing finalizers and the IDisposable interface for proper resource management.

Understanding how garbage collection works can help you write more efficient C# code and avoid common pitfalls like memory leaks and improper resource management.

### Readonly vs constants

### Private function in interface

### Abstract class sealed

### Linq let

## EF

### EF vs Dapper

Entity Framework Core (EF Core) and Dapper are two popular Object-Relational Mappers (ORMs) in C#, but they differ significantly in their approach, features, and performance.
1. Entity Framework Core (EF Core)

    Type: Full-fledged ORM
    Abstraction Level: High
    Features:
        EF Core provides a higher-level abstraction, making it easier to work with databases using LINQ queries and strongly-typed classes.
        It supports complex scenarios like lazy loading, change tracking, and migrations.
        EF Core allows you to model your database directly using code-first or database-first approaches.
        It is more feature-rich and better suited for projects where database schema management and advanced querying capabilities are important.
    Performance:
        EF Core is generally slower than Dapper because it has more overhead due to its higher level of abstraction.
        The performance hit is more noticeable in scenarios with complex queries, large datasets, or where fine-grained control over SQL queries is needed.

2. Dapper

    Type: Micro-ORM
    Abstraction Level: Low
    Features:
        Dapper is a lightweight, performance-oriented micro-ORM that maps database queries to objects with minimal overhead.
        It is essentially an extension to IDbConnection, providing a simple API for running raw SQL queries and mapping the results to C# objects.
        Dapper does not have the advanced features of EF Core, like change tracking, migrations, or LINQ support.
        It allows you to write raw SQL queries, giving you complete control over your database interactions.
    Performance:
        Dapper is generally faster than EF Core because it has very little overhead and is close to the metal (i.e., close to how ADO.NET works).
        It’s often chosen for performance-critical applications or where the developer needs more control over the SQL being executed.

#### Which One is Faster?

Dapper is typically faster than EF Core, especially in scenarios where the application needs to execute complex queries, retrieve large datasets, or requires minimal latency.
However, the difference in performance might be negligible for simple queries or small datasets. If you need the features provided by EF Core, the slight performance trade-off may be worth it.

#### Choosing Between EF Core and Dapper

##### Use EF Core if:
- You need a full-featured ORM with support for complex queries, relationships, and LINQ.
- You want to work with a higher level of abstraction and have the benefits of features like migrations and change tracking.

##### Use Dapper if:
- Performance is a critical factor.
- You prefer writing raw SQL queries and need fine-grained control over the database interactions.
- You don't need the extra features provided by a full ORM like EF Core.

Each has its strengths and can be chosen based on the specific needs of the project. Some developers even use both in the same project, using Dapper for performance-critical sections and EF Core for areas where its features are beneficial.

### How does EF prevents sql injection?

Entity Framework (EF) prevents SQL injection primarily through the use of parameterized queries and the LINQ (Language-Integrated Query) syntax, which abstracts the process of query construction. Here’s how EF achieves this:

### Types of inheritance in EF

Entity Framework (EF) Core supports several types of inheritance strategies that you can use to model inheritance hierarchies in your database. These strategies allow you to map object-oriented inheritance structures to relational database schemas. The three main types of inheritance in EF Core are:

#### 1. **Table-per-Hierarchy (TPH)**
- **Description**: In TPH, a single table is used to store data for all classes in an inheritance hierarchy. A discriminator column is added to the table to distinguish between different types in the hierarchy.
- **Pros**:
  - Simple to implement and requires only one table.
  - Efficient querying since all data is in a single table.
- **Cons**:
  - Can lead to sparse tables if subclasses have many properties that are not shared with the base class.
  - The schema might not be as normalized as in other strategies.
- **Example**:

  ```csharp
  public class Animal
  {
      public int Id { get; set; }
      public string Name { get; set; }
  }

  public class Dog : Animal
  {
      public bool Barks { get; set; }
  }

  public class Cat : Animal
  {
      public bool Meows { get; set; }
  }
  ```

  EF Core will create a single `Animals` table with columns for `Id`, `Name`, `Barks`, `Meows`, and a discriminator column like `Discriminator` that indicates whether the row represents a `Dog`, `Cat`, or another subclass.

#### 2. **Table-per-Type (TPT)**
- **Description**: In TPT, each class in the inheritance hierarchy has its own table. The base class table contains the properties common to all classes, and each derived class has a table containing properties specific to that class. These tables are linked by a primary key/foreign key relationship.
- **Pros**:
  - Schema is more normalized.
  - Allows you to use database constraints that are specific to subclasses.
- **Cons**:
  - Can result in complex queries and joins when retrieving data.
  - May have performance implications due to the need for multiple joins.
- **Example**:

  ```csharp
  public class Animal
  {
      public int Id { get; set; }
      public string Name { get; set; }
  }

  public class Dog : Animal
  {
      public bool Barks { get; set; }
  }

  public class Cat : Animal
  {
      public bool Meows { get; set; }
  }
  ```

  In TPT, EF Core will create separate tables:
  - `Animals` with columns `Id` and `Name`
  - `Dogs` with columns `Id` and `Barks` (where `Id` is a foreign key referencing `Animals`)
  - `Cats` with columns `Id` and `Meows` (where `Id` is a foreign key referencing `Animals`)

#### 3. **Table-per-Concrete-Type (TPC)**
- **Description**: In TPC, each concrete class in the hierarchy has its own table. There is no shared table for the base class. Each table contains all the properties of that class, including inherited properties.
- **Pros**:
  - Simple querying since each class maps to a single table.
  - No need for joins or discriminator columns.
- **Cons**:
  - Data duplication across tables, as the properties of the base class are repeated in each derived class table.
  - Schema is less normalized.
- **Example**:

  ```csharp
  public class Animal
  {
      public int Id { get; set; }
      public string Name { get; set; }
  }

  public class Dog : Animal
  {
      public bool Barks { get; set; }
  }

  public class Cat : Animal
  {
      public bool Meows { get; set; }
  }
  ```

  In TPC, EF Core will create separate tables:
  - `Dogs` with columns `Id`, `Name`, and `Barks`
  - `Cats` with columns `Id`, `Name`, and `Meows`

#### **Choosing the Right Strategy**
- **TPH** is often the default choice because it is straightforward and performs well for many scenarios.
- **TPT** is useful when you want a normalized schema or need to apply different constraints to different subclasses, but it can introduce performance overhead.
- **TPC** is less commonly used due to data duplication but can be useful when you need simple and efficient queries at the cost of storage efficiency.

Each strategy has its trade-offs, and the choice depends on the specific requirements of your application, such as performance needs, database normalization, and the complexity of the inheritance hierarchy.

## Data types

### Linked list

### Stack

### Array

## Patterns

### Unit of work

- **Purpose**: The Unit of Work pattern manages transactions and coordinates changes to multiple repositories to ensure that all operations succeed or fail as a unit. It acts as a wrapper around multiple operations that might involve different repositories or entities, ensuring that they are committed together or rolled back in case of failure.
- **Usage**: In practice, the Unit of Work pattern is used to group operations that should be treated as a single transaction. For instance, in a banking application, transferring money between accounts might involve updating multiple records, and the Unit of Work ensures that these updates either all succeed or none of them do.
- **Benefits**:
  - Ensures atomicity of transactions.
  - Simplifies the management of changes across multiple repositories.
  - Helps reduce the likelihood of data inconsistencies.

### CRQS

- **Purpose**: CQRS is a design pattern that separates the read (query) and write (command) operations into distinct models. This allows for different optimization, scalability, and consistency strategies for reading data and modifying it.
- **Usage**: In CQRS, the "command" model handles operations that change the state of the application, such as creating or updating records, while the "query" model handles operations that retrieve data. This separation is especially beneficial in complex domains or systems with high read/write loads, where different requirements for reading and writing data exist.
- **Benefits**:
  - Enables independent scaling of read and write operations.
  - Allows for more flexibility in handling different data consistency models.
  - Simplifies the handling of complex business logic in commands.

### Repository 

- **Purpose**: The Repository pattern provides an abstraction layer between the application and the data access logic, isolating the database or data source interactions. It encapsulates the logic for retrieving and storing data, allowing the application to work with domain objects without concerning itself with the underlying data access code.
- **Usage**: A repository typically contains methods like `Add`, `Update`, `Remove`, and `Get`, providing a simple interface for working with data entities. This pattern is often used in conjunction with the Unit of Work pattern to manage transactions across multiple repositories.
- **Benefits**:
  - Promotes a clean separation of concerns by isolating the data access logic.
  - Makes the codebase more maintainable and testable.
  - Allows for easy switching of data sources (e.g., from a database to an in-memory collection).

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
