# C# Naming Conventions

## General

All named members should have a short but descriptive name. Do NOT use abbreviations, except in commonly known instances. Do NOT use single letter names EXCEPT as counters in loop mechanisms (e.g. `for` loops). Do NOT use underscores as whole names. UI objects should have the type name appended to the end of the descriptive name. Any kind of identifier should end with the letters `ID`.

_Examples:_

```csharp
machineNumber; SQLConnection; StudentID; STANDARD_GREETING; SaveButton; UsernameTextBox;
```

## Namespaces

Namespaces should always be in **PascalCase**.

_Example:_

```csharp
namespace Microsoft.VisualStudio.AwesomeStuff;
```

## Classes

Classes should always be in **PascalCase**, regardless of access level.

_Examples:_

```csharp
public class Animal { ... }
internal class Dog { ... }
private class Cat { ... }
```

## Interfaces

Interfaces should always be in **PascalCase**, starting with the letter `I`.

_Example:_

```csharp
interface IAnimal { ... }
```

## Methods

Methods should always be in **PascalCase**, regardless of access level.

_Examples:_

```csharp
public int GetNumberOfItems(IEnumerable collectionOfItems) { ... }
private void DoSomeWork() { ... }
```

## Parameters

Parameters in constructors or methods should always be in **camelCase**.

_Example:_

```csharp
public void DoSomeWork(string nameOfJob, int timesToPerform, bool isSecretWork) { ... }
```

## Properties

Properties should always be in **PascalCase**, regardless of access level.

_Examples:_

```csharp
public int ItemCount { get; set; };
private string InternalMessage { get; set; }
```

## Private Members

Private members of a class (e.g. property-backing variables) should always be in **camelCase**, starting with an underscore.

_Example:_

```csharp
private string _errorMessage;
```

## Constants

Constants should always be in **ALL CAPS** with underscores between each word of the name.

_Example:_

```csharp
public const string DEFAULT_MESSAGE = "I'm sorry, Dave. I'm afraid I can't do that.";
```

## Local Variables

Local variables should always be in **camelCase**. The var keyword will be used wherever possible for declaring local variables. Exceptions to this are numeric types, where the type should always be declared explicitly.

_Example:_

```csharp
var thisAnimal = new Animal(); float costPerUnit = 0.00; int numberOfToesPerFoot = 5;
```

## Enumerators

Enumerations declared using the enum keyword should always be in **PascalCase**.

_Example:_

```csharp
public enum UserType { ... }
```

## Booleans

Boolean member names should usually begin with the item- and verb tense-appropriate form of **Is** to quickly indicate that the item returns a Boolean value. In some instances, this naming convention does not make sense and a more concise and appropriate name should be chosen.

_Examples:_

```csharp
bool HasOperationCompleted(SqlConnection conn) { ... }
var _isLocalUser = true;
var containsItem = someEnumerable.Contains(expression);
```
