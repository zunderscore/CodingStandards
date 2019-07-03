# Misc Standards/Conventions

## DRY/Code Reuse

Always follow the **DRY (Don’t Repeat Yourself)** methodology: Any logical operation that will be or potentially will be used more than once should be refactored into its own method. This reduces repeated code making it less bug-prone, more maintainable, and easier to read.

_Example:_

```csharp
public void SaveButton_Click()
{
    SaveDocument();
}

public void AutoSaveTimer_Tick()
{
    SaveDocument();
}

public void SaveDocument()
{
    // Work only gets done here
}
```

## Event Handlers

Event handlers should always contain as little code as possible and should never do work directly in the event handler method. They should always call another method to perform the actual work. This is especially important with UI events. This is consistent with the policy of DRY/code reuse.

_Example:_

```csharp
public void SaveButton_Click(EventArgs e)
{
    SaveDocument();
}
```

## Exception Handling

### General

Most code blocks inside of methods should be wrapped in `try`-`catch`-`finally` statements, especially those that perform any kinds of data access or depend on connecting to other services. When catching an exception, ALWAYS handle the exception in the most appropriate way, whether that’s providing alternate logic, logging the exception, or rethrowing the exception. Empty `catch` blocks are unacceptable. Just like with other blocks of logic, `try`-`catch`-`finally` constructs should be as short and concise as possible, attempting to perform only a single action.

### Use of `finally` Keyword

Not all operations will require a `finally` block, but they should exist in the case that any final operations need to be performed regardless of whether an exception is thrown or not (e.g. manually closing database connections).

### Naming Convention

Exceptions should be named in a similar manner to local variables, but as short as possible, using abbreviations where applicable. They should always end with the letters `ex`. Generic `Exception` types should just be named `ex`.

_Examples:_

```csharp
ArgumentOutOfRangeException aoorex;
InvalidOperationException ioex;
FileNotFoundException fnfex;
Exception ex;
```

### Catch Order

Exceptions should be caught and handled in specific-to-generic order. In other words, more specific exception types should be caught first, and the generic `Exception` type should be the last type caught.

_Example:_

```csharp
try
{
    DoSomeWork();
}
catch (ArgumentOutOfRangeException aoorex)
{
    throw;
}
catch (ArgumentException aex)
{
    throw;
}
catch (Exception ex)
{
    throw;
}
```

### Rethrowing Exceptions

When needing to rethrow an exception, ALWAYS use a single simple `throw` statement. Do NOT create a new `Exception` object unless absolutely necessary. If you must create and throw a new `Exception` object, use as specific of an `Exception` type as possible (e.g. `InvalidOperationException`, `ArgumentException`, etc.), and set the new object’s `InnerException` property the original exception. Do NOT throw the exception by name to avoid the stack trace and other important information being lost.

_Example:_

```csharp
try
{
    DoSomeWork();
}
catch (InvalidOperationException ioex)
{
    LogError();
    throw;
}
catch (Exception ex)
{
    throw;
}
```

## Tester-Doer Pattern

When writing any logic, always adhere to the **Tester-Doer Pattern**. This pattern dictates that you test that an object, property, variable, etc. meets specific requirements before you do anything to it. In many of these instances, the conditional `?:` and null coalescing `??` operators can be helpful. This will reduce unnecessary bugs and exceptions, especially the dreaded `NullReferenceException`, leading to better application performance and reliability.

_Examples:_

```csharp
if (!String.IsNullOrWhitespace(emailAddress)) { ... }

var serverPort = isDefaultPortSelected ? 80 : customPortNumber;

ConnectToServer(serverName ?? DEFAULT_SERVER_NAME);
```

## Try-Parse Pattern

In some cases, parsing strings to other data types may be necessary. In these cases, always use the **Try-Parse Pattern** to determine if a string can correctly be parsed as the desired type. Using this along with the **Tester-Doer Pattern** can reduce exceptions/bugs and increase performance/reliability even more.

_Example:_

```csharp
if (Int32.TryParse(Fields[“PortNumber”], out int portNumber)) { ... }
```

## Retry Policy

Any operations, especially those that retrieve/save data, should be wrapped in a mechanism to retry the operation if it fails (e.g. in the event of a database deadlock). The minimum number of tries should be 3 unless otherwise specified. Each retry should be accompanied by a small delay (250ms by default) between retries to ensure any conditions preventing the operation have cleared.
