# Bogus Package  
Link: *https://github.com/bchavez/Bogus*  
> Bogus is a simple and sane fake data generator for .NET languages like C#, F# and VB.NET. Bogus will help you load databases, UI and apps with fake data for your testing needs.  

### Install
```  
Install-Package Bogus
```

## Examples  

```csharp

class Order
{
    int OrderId { get; set; }
    string Item { get; set; }
    int Quantity { get; set; }
    int? LotNumber { get; set; }
}

//Set the randomizer seed if you wish to generate repeatable data sets.
Randomizer.Seed = new Random(8675309);
var fruit = new[] { "apple", "banana", "orange", "strawberry", "kiwi" };
var orderIds = 0;

var testOrders = new Faker<Order>()
    //Ensure all properties have rules. By default, StrictMode is false
    //Set a global policy by using Faker.DefaultStrictMode
    .StrictMode(true)
    //OrderId is deterministic
    .RuleFor(o => o.OrderId, f => orderIds++)
    //Pick some fruit from a basket
    .RuleFor(o => o.Item, f => f.PickRandom(fruit))
    //A random quantity from 1 to 10
    .RuleFor(o => o.Quantity, f => f.Random.Number(1, 10))
    //A nullable int? with 80% probability of being null.
    //The .OrNull extension is in the Bogus.Extensions namespace.
    .RuleFor(o => o.LotNumber, f => f.Random.Int(0, 100).OrNull(f, .8f));

```