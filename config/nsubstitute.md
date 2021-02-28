# NSubstitute

https://nsubstitute.github.io/help/getting-started/

## Creating a Substitute
In this section the way of creating a substitute for an _interface_, _`class`_, _multiple interfaces_ or _delegate_ is described.

### Interface
The basic syntax for substitute creation looks the following:
```csharp
var substitute = Substitute.For<ISomeInterface>();
```

### Class
The basic syntax for substitute creation looks the following in case of class creation that has constructor arguments:
```csharp
var someClass = Substitute.For<SomeClassWithCtorArgs>(5, "hello world");
```
> **Warning**: Substituting for classes can have some nasty side-effects!

### Multiple Interfaces
The basic syntax for creating substitute for multiple interfaces looks the following:
```csharp
var subsitute = Substitute.For<ISomeInterface, ISomeOtherInterface>();
```

### Delegate
The basic syntax for substitute creation looks the following:
```csharp
var func = Substitute.For<Func<string>>();

func().Returns("hello");
Assert.AreEqual("hello", func());
```

## Setting a return value
To set a return value for a method call on a substitute, call the method as normal, then follow it with a call to NSubstitute’s **Returns()** extension method.
### Methods
```csharp
var calculator = Substitute.For<ICalculator>();
calculator.Add(1, 2).Returns(3);
```
### Properties
```csharp
calculator.Mode.Returns("DEC");
```
### Examples
```csharp
//Return when first arg is anything and second arg is 5:
calculator.Add(Arg.Any<int>(), 5).Returns(10);
//Return when first arg is 1 and second arg less than 0:
calculator.Add(1, Arg.Is<int>(x => x < 0)).Returns(345);
//Return when both args equal to 0:
calculator.Add(Arg.Is(0), Arg.Is(0)).Returns(99);
//Retrurn for any argument
calculator.Add(1, 2).ReturnsForAnyArgs(100); 
```





