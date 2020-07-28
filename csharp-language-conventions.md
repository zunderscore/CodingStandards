# C#/.NET Conventions

## Built-In Types

Built-in types are types that are built into and available in both .NET and the C# language. Examples include: `bool`, `byte`, `float`, `int`, `object`, `string`, etc.

When instantiating variables for any built-in types, always use the C# language form of the class name, NOT the framework form.

_Example:_

```csharp
var message = new string("Default message");
```

However, when accessing a static member of a built-in type, always use the framework form of the class name, NOT the language form.

_Example:_

```csharp
message = String.Join(",", listOfItems);
```

## Constructor Overloading

When overloading constructors for a type, less-specific constructors should typically call more specific constructors using the `this` keyword.

_Example:_

```csharp
public Animal() : this(String.Empty) { ... }
public Animal(string name) { ... }
```

## Language Conventions vs. Framework Conventions

Just like with types, there are instances where C# and the .NET framework accomplish the same goal but with different syntax. In those instances, we always want to defer to the language convention.

### Nullable Types

Use the C# `?` operand instead of the `Nullable<T>` type. Also, null checking should be done with the equality `==` or inequality `!=` operators instead of using a nullable type’s `HasValue` property.

_Example:_

```csharp
DateTime? lastLoginDate;
if (lastLoginDate != null) { ... }
```

## Dynamic Types

Because C# is a strongly-typed language, we should always use strong, explicit types. Dynamic types should NEVER be used unless absolutely necessary. These create a performance bottleneck and can also create errors that are not caught until runtime.

## Fully Qualified Type Names

Use `using` statements wherever possible to avoid fully qualified type names. Likewise, unused `using` statements should be removed from file headers.

_Example:_

```csharp
using System.Data;
```

## Object/Collection Initialization

Wherever possible, use object or collection initialization over setting properties/adding items after initializing.

_Examples:_

```csharp
var myDog = new Dog()
{
    Name = "Rusty",
    Breed = "Dachshund"
};
var subjects = new List<string>()
{
    "Reading",
    "Writing",
    "Math"
};
```

## `IDisposable` Objects
Every instance object that implements the `IDisposable` interface must be wrapped inside of a `using` block. This ensures that the object’s Dispose method is always called, and memory allocated for the object is freed.

_Example:_

```csharp
using (var conn = new SQLConnection(connString)) { ... }
```

## `this` and `base` Keywords

Do not use the `this` or `base` keywords unless necessary (e.g. overloading constructors). Using these keywords can make code less readable and harder to maintain.

## Enumerators

Wherever possible, use enumerators instead of string or numeric values. Always explicitly declare enumerator numeric values to ensure consistency when storing/retrieving data.

_Example:_

```csharp
public enum Status
{
    NotStarted = 0;
    Started = 1;
    InProgress = 2;
    Completed = 3;
}
```

## Empty Strings

Always use `String.Empty` instead of `""`.

_Example:_

```csharp
var message = String.Empty;
```

## Extension Methods

When given the option to use an extension method or a static method for a type, use the extension method.

_Examples:_

```csharp
return emailAddress.Contains("@");
var theAnswer = listOfNumbers.Find(42);
```

## String Comparisons

When comparing strings, unless otherwise necessary, always use a form of the `String.Equals` method with the `InvariantCultureIgnoreCase` comparer.

Example:

```csharp
string submittedEmail = FormValues["Email"];
return submittedEmail.Equals(userEmail, StringComparison.InvariantCultureIgnoreCase);
```

## Conditional Expressions

Wherever possible, always prefer the conditional `?:` over an `if` statement, especially for variable assignments or return statements.

_Examples:_

```csharp
var serverPort = isDefaultPortSelected ? 80 : customPortNumber;
return isConnectionAvailable ? "Success" : "Failure";
```

## Attributes

When creating attributes, always add the `Attribute` suffix to the class name and inherit the base `Attribute` class. To make code easier to read, any attributes applied to a member should be set on individual lines. You may omit the `Attribute` suffix when applying attributes.

_Example:_

```csharp
public class SensitiveDataAttribute : Attribute { ... }
...
[Serializable]
[SensitiveData]
public class UserGroups { ... }
```

## Lambda Expressions vs. Anonymous Methods

In some cases, we may have the opportunity to use either lambda expressions (using the `=>` operator) or anonymous methods to perform actions. In these cases, use lambda expressions wherever possible and avoid anonymous methods. This makes code cleaner and easier to read.

_Example:_

```csharp
return users.FirstOrDefault(user => user.ID == currentUserID);
public string ErrorMessage
{
    get => _errorMessage ?? DEFAULT_ERROR_MESSAGE;
    set => _errorMessage = value;
}
```

## Asynchronous Code
Whenever possible, create and use asynchronous methods for I/O (e.g. data) access and CPU-intensive work, using the `async` and `await` keywords and returning `Task` objects. Asynchronous methods should be named as normal, but with the addition of the word `Async` at the end.

_Example:_

```csharp
public async Task<List<User>> GetUsersAsync()
{
    await dbContext.Users.ToListAsync();
}
```

## LINQ

LINQ is a powerful way to retrieve, manipulate, and transform data and objects. In most cases, when using LINQ, use extension methods rather than the query language. This makes code more consistent and easier to read/maintain.

_Example:_

```csharp
var currentUser = await dbContext.Users.Include(user => user.Groups).FirstAsync(user => user.ID == currentUserID);
```

## Fluent APIs

When using Fluent APIs like LINQ, it’s always best to separate each chained method call onto its own individual line to increase readability. Therefore, the above LINQ example would look like this:

```csharp
var currentUser = await dbContext.Users
    .Include(user => user.Groups)
    .FirstAsync(user => user.ID == currentUserID);
```
