![logo](images/c-sharp-logo.png)

## Where to practice coding?
- www.leetcode.com
- www.exercism.com
- www.codewars.com
- www.hackerrank.com

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

### Single/First/FirstOrDefault

### Struct vs class

### Heap and stack

### Action vs function

### Authentication methods

### Property binding

### Value type vs reference type

### Deffered vs immmediate execution

### Hosted services

### Dependency injection

#### Lifecycle

- AddTransient
- AddScoped
- AddSingleton

### Middleware

### Map

### Async/await

### Threads

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
