# Info
This repository is based on the official [_ASP.NET Core tutorial: Controllers_](https://learn.microsoft.com/en-us/training/modules/build-web-api-aspnet-core/1-introduction) provided by Microsoft.  

The sample project **ContosoPizza** was created by Microsoft as a study project to help people learn how to create _Controllers_, build _RESTful APIs_, and better understand how ASP.NET Core works.  

### Notice
This repository contains a fully completed version of the guide with all code blocks and additional comments.  
Some parts of the official Microsoft documentation are included directly or with slight modifications.  
Other parts are written in my own words, often simplified to make the material easier to understand.  
I also added my own explanations and clarifications throughout the tutorial.  

## What is learning here?
There, you can to learn how to write a RESTful APIs, how they shoold look like, how to write controllers and other intresting things.

Also we'll look how to create a _database_.

### Little bit about Controllers
In this project you will see these type of _Controllers_:
- GET - __getting__ data from the web service;
- POST - __uploading__ data to the web service;
- PUT - __update__ data on the web service;
- PACH - __update__ data with _specific_ instructions on the web service;
- DELETE - __delete__ data from the web service;

_In this case we will not use __PACH__ controller_.

## ControllerBase
A controller is a public class which contains many also public methods known as _actions_. Usually they located in the _Controllers_ directory. The _actions_ are initialized as HTTP endpoints via routing. So an HTTP ``GET`` request to ``https://localhost:{port}/weatherforecast`` call the ``Get()`` method of the ``WeatherForecastController`` class.

Firstly, pay atention that WeatherForecastController class inherits from the ``ControllerBase`` class. This base class gives standard functionality for handlibg HTTP requests. 

### Atention
Don't create a web API controller from the ``Controller`` class. ``Controller`` comes from the ``ControllerBase`` and adds support for views, so it's for handling webpages, not web API requests.

## API controller class attributes
There are two important attributes which are applied in the ``WeatherForecastController``:
```cs
[ApiController]
[Route("[controller]")]
public class WeatherForecastController : ControllerBase
```
``[ApiController]`` enables _individual behavior_, that make it easier to build web APIs. Some bahaviors include _parameter source inference_, _attribute routing as a requirement_, and _model validation error-handlingenhancements_.

``[Route]`` defines the routing pattern ``[controller]``. The controller's name (case-intensitive, without the _Controller_ suffix) replaces the ``[controller]`` token. Also can be useful thise kind of naming controller: ``[Rote("weatherforecast")]``.

### Give weather results usign the Get() method
Now ``WeatherForecastController`` conteins with a one action method, designed by the ``[HttpGet(Name = "GetWeatherForecast")]`` attribute. This attribute routes HTTP ``GET`` requests to the 
```cs 
public IEnumerable<WeatherForecast> Get()
```
method. 

## Models
To save somewhere your data, you need to have a data store on which you'll also can perform operations.

You need a ``model`` class to represent a pizzas from your storege. The model will contains properties of a piza. The model is used to pass data in the web API and to save pizza's options in data store.

To create a pizza model you need to write next line in the terminal:
```bash
mkdir Models
```
Next, select _Models_ directory and create a new file with name _Pizza.cs_ from the VS Code UI by clicking on the __new file__ button.
Or write other command in the terminal:

### On Windows:
```bash
New-Item Models/Pizza.cs
```

### On Linux:
```bash
touch Models/Pizza.cs
```

The directory name _Models_ was grabet from the _model-view-controller_ architecture that web API uses.

### Initialize Pizza class
In the _Pizza.cs_ file write next lines of code:
```cs
namespace ContosoPizza.Models;

public class Pizza
{
    public int Id { get; set; }
    public string? Name { get; set; }
    public bool IsBlutenFree { get; set; }
    public decimal Price { get; set; }
}
```

## Service
Next step is to create _Services_ in your project.
Write in the console next line to create a new _directory_:
```bash
mkdir Services
```
Or use VS Core UI by clicking on the __create directory__ button.

Select _Services_ directory and create new file with called _PizzaService.cs_.
Or use preceding command to create a new file from the terminal.

Add the following code to _Services/PizzaService.cs_ and don't forget to save your changes. This code will create in-memory data service.

```cs
using ContosoPizza.Models;

namespace ContosoPizza.Services;

public static class PizzaService
{
    static List<Pizza> Pizzas { get; }
    static int nextId = 3;
    static PizzaService()
    {
        Pizzas = new List<Pizza>
        {
            new Pizza { Id = 1, Name = "Margherita", IsBlutenFree = false, Price = 7.50M },
            new Pizza { Id = 2, Name = "Funghi", IsBlutenFree = true, Price = 8.50M }
        };
    }

    public static List<Pizza> GetAll() => Pizzas;

    public static Pizza? Get(int id) => Pizzas.FirstOrDefault(p => p.Id == id);

    public static void Add(Pizza pizza)
    {
        pizza.Id = nextId++;
        Pizzas.Add(pizza);
    }

    public static void Delete(int id)
    {
        var pizza = Get(id);
        if (pizza is null)
            return;

        Pizzas.Remove(pizza);
    }

    public static void Update(Pizza pizza)
    {
        var index = Pizzas.FindIndex(p => p.Id == pizza.Id);
        if (index == -1)
            return;
            
        Pizzas[index] = pizza;
    }
}
```

You'll have two pizzas by default. This is a __demo__ service, so new data will not save in the data store, if you'll try to _restart_ your program, you see __just those two__ pizzas which created in the PizzaService constructor.

Run next command to bild your project:
```Bash
dotnet build
```

If you'll see errors while building, check the output for troubleshooting information.

# Links
There are some necessary links which you'll need if you want to read some intresting info about this guide and ASP.NET
- [ASP.NET Core tutorial: Controllers](https://learn.microsoft.com/en-us/training/modules/build-web-api-aspnet-core/1-introduction)
- [OpenAPI](https://learn.microsoft.com/uk-ua/aspnet/core/fundamentals/openapi/aspnetcore-openapi?view=aspnetcore-9.0&tabs=visual-studio%2Cvisual-studio-code)
- [HttpRepl](https://learn.microsoft.com/uk-ua/aspnet/core/web-api/http-repl/?view=aspnetcore-9.0&tabs=windows)
- [Error: unable to find an openapi description](https://learn.microsoft.com/en-us/answers/questions/1665010/i-received-the-error-unable-to-find-an-openapi-des)

