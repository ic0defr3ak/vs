# OneOf
Link: *https://github.com/mcintyre321/OneOf*   

>This library provides F# style ~~discriminated~~ unions for C#, using a custom type OneOf<T0, ... Tn>. An instance of this type holds a single value, which is one of the types in its generic argument list.  


### Install
```
Install-Package OneOf
```

## Examples  

### Method Return Value
```csharp
public OneOf<User, InvalidName, NameTaken> CreateUser(string username)
{
    if (!IsValid(username)) return new InvalidName();
    var user = _repo.FindByUsername(username);
    if(user != null) return new NameTaken();
    var user = new User(username);
    _repo.Save(user);
    return user;
}

[HttpPost]
public IActionResult Register(string username)
{
    OneOf<User, InvalidName, NameTaken> createUserResult = CreateUser(username);
    return createUserResult.Match(
        user => new RedirectResult("/dashboard"),
        invalidName => {
            ModelState.AddModelError(nameof(username), $"Sorry, that is not a valid username.");
            return View("Register");
        },
        nameTaken => {
            ModelState.AddModelError(nameof(username), "Sorry, that name is already in use.");
            return View("Register");
        }
    );
}
```

### Method Parameter Value
```csharp
public void SetBackground(OneOf<string, ColorName, Color> backgroundColor) {  }
```

## Matching  
```csharp
OneOf<string, ColorName, Color> backgroundColor = ...;
Color c = backgroundColor.Match(
    str => CssHelper.GetColorFromString(str),
    name => new Color(name),
    col => col
);
_window.BackgroundColor = c;

OneOf<string, DateTime> dateValue = ...;
dateValue.Switch(
    str => AddEntry(DateTime.Parse(str), foo),
    int => AddEntry(int, foo)
);
```
