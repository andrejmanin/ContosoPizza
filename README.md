# 🔭 Info
This repository is based on the official [_ASP.NET Core tutorial: Controllers_](https://learn.microsoft.com/en-us/training/modules/build-web-api-aspnet-core/1-introduction) provided by Microsoft.  

The sample project **ContosoPizza** was created by Microsoft as a study project to help people learn how to create _Controllers_, build _RESTful APIs_, and better understand how ASP.NET Core works.  

### 🖋️ Notice
This repository contains a fully completed version of the guide with all code blocks and additional comments.  
Some parts of the official Microsoft documentation are included directly or with slight modifications.  
Other parts are written in my own words, often simplified to make the material easier to understand.  
I also added my own explanations and clarifications throughout the tutorial.  

# 🕵🏼 What is learning here?
There, you can to learn how to write a RESTful APIs, how they shoold look like, how to write controllers and other intresting things.

Also we'll look how to create a _database_.

# ControllerBase
A controller is a public class which contains many also public methods known as _actions_. Usually they located in the _Controllers_ directory. The _actions_ are initialized as HTTP endpoints via routing. So an HTTP ``GET`` request to ``https://localhost:{port}/weatherforecast`` call the ``Get()`` method of the ``WeatherForecastController`` class.

Firstly, pay atention that WeatherForecastController class inherits from the ``ControllerBase`` class. This base class gives standard functionality for handlibg HTTP requests. 

## Atention
Don't create a web API controller from the ``Controller`` class. ``Controller`` comes from the ``ControllerBase`` and adds support for views, so it's for handling webpages, not web API requests.

# API controller class attributes
There are two important attributes which are applied in the ``WeatherForecastController``:

```cs
[ApiController]
[Route("[controller]")]
public class WeatherForecastController : ControllerBase
```

``[ApiController]`` enables _individual behavior_, that make it easier to build web APIs. Some bahaviors include _parameter source inference_, _attribute routing as a requirement_, and _model validation error-handlingenhancements_.

``[Route]`` defines the routing pattern ``[controller]``. The controller's name (case-intensitive, without the _Controller_ suffix) replaces the ``[controller]`` token. Also can be useful thise kind of naming controller: ``[Rote("weatherforecast")]``.

## Give weather results usign the Get() method
Now ``WeatherForecastController`` conteins with a one action method, designed by the ``[HttpGet(Name = "GetWeatherForecast")]`` attribute. This attribute routes HTTP ``GET`` requests to the 
```cs 
public IEnumerable<WeatherForecast> Get()
```
method. 

# Models
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

## Initialize Pizza class
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

# Service
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

# Controller
A _controller_ is a public class with many public methods whose called as actions. By standart, controllers are placed in the _Controllers_ directory. The actions are exposed as HTTP endpoints inside the web API controller. The name of the controller directory _Controllers_ comes from the _model-view-controller_ architecture that the web API uses. And also by convention, controller class names are ends with _Controller_ suffix.

## How to create a controller
Firstly, sellect the _Controllers_ directory and by using UI or terminal create a new file with name _PizzaController.cs_.

Second thing which you need to do, is paste next lines of code and save changes:
```cs
using ContosoPizza.Models;
using ContosoPizza.Services;
using Microsoft.AspNetCore.Mvc;

[ApiController]
[Route("[controller]")]
public class ContosoPizzaController : ControllerBase
{
    public ContosoPizzaController()
    {
    }

    // GET all action
    [HttpGet]
    public ActionResult<List<Pizza>> GetAll() => PizzaService.GetAll();

    // GET by Id action
    [HttpGet("{id}")]
    public ActionResult<Pizza> Get(int id)
    {
        var pizza = PizzaService.Get(id);

        if (pizza is null)
            return NotFound();

        return pizza;
    }

    // POST action


    // PUT action


    // DELETE action
}
```

## Explanation
This class derives from ControllerBase, the base class for working with HTTP requests in ASP.NET Core. It also includes two standart attributes: ``[ApiController]`` and ``[Route("[controller]")]``.

## Attributes
The ``[ApiController]`` attribute indicates that a type and all derived types are used to service HTTP API responses. [ApiControllerAttribute Class](https://learn.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute?view=aspnetcore-9.0)

The ``[Route]`` attribute specifies URL patern for a controller or action. [Create web APIs with ASP.NET Core: Attributes](https://learn.microsoft.com/en-us/aspnet/core/web-api/?view=aspnetcore-9.0#attributes)

## ControllerBase class
A web API based on controllers consist of one or more controllers classes that derive from [__ControllerBase__](https://learn.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controllerbase?view=aspnetcore-9.0). The web API project template provides a starter controller:

```cs
public class ContosoPizzaController : ControllerBase
```

Web API controllers shoold derive from __ControllerBase__ rather from _Controller_. ``Controller`` derives from __ControllerBase__ and support for views, so it's for handiling web pages, not web API requests.

__Pay your atention__: this controller named ``ContosoPizzaController``, it handles requests to ``https://localhost:{port}/contosopizza``.

## Actions
Now you got to the implementing REST actions. How above wrote, in ASP.NET Core there are five different actions:
- GET - receiving data from the web service;
- POST - pushing data to the web service;
- PUT - update data in the web service;
- PACH - update data in the web service with special configuration or requirements;
- DELETE - delete data from the web service.

In code to show which action to use in which case, are special build-in attributes: ``[HttpGet]``, ``[HttpPost]``, ``[HttpPut]``, ``[HttpPach]``, ``[HttpDelete]``. 

# Binding source parameter inference

A binding source attribute defines the location at which an action parameter's value is found. Here are the existing binding attributes:

-----------------------------------------------------------------------------
|   Attribute       |   Binding source                                      |
|   [FromBody]      |   Requests body                                       |
|   [FromForm]      |   Form data in the request body                       |
|   [FromHeader]    |   Request header                                      |
|   [FromQuery]     |   Request query string parameter                      |
|   [FromRoute]     |   Route data from the current request                 |
|   [FromServices]  |   The request service injected as an action parameter |
|   [AsParameters]  |   Method parameters                                   |

If values might contein ``%2f`` (that is ``/``) - don't use ``[FromRoute]``. It won't be unescaped to ``/``. In this case use ``[FromQuery]. 

In the example below, the ``[FromQuery]`` attribute indicates that the ``id`` parameter value is provided in the request URL's query string:

To read more about those attributes see [__Microsoft Binding source parameter inference__](https://learn.microsoft.com/en-us/aspnet/core/web-api/?view=aspnetcore-9.0#binding-source-parameter-inference)

```cs
[HttpGet]
public ActionResult<Pizza> Get([FromQuery] int id) {
    var pizza = PizzaService.Get(id);

    if (pizza is null)
        return NotFound();
    
    return pizza;
}
```

As you can see, your methods in the controller class return ``ActionResult<T>`` type. Each ``ActionResult`` instance use in the preceding action is mapped to the corresponding HTTP status code. In the following table you can see main code results:

-----------------------------------------------------------------------------
| ASP.NET Core action result  |     HTTP status code    |   Description     |
|   ``Ok`` is complied        |            200          | request was correctly processed without issues |
|   ``NotFound``              |            404          | Data which user tried to find was not found or not exist |

# Build and run
Build and start the web API by running command below:

```Bash
dotnet run
```

# Testing controller

## Http file
Firstly, download extantion __REST Client__ so you can start testing in this way.

1. Open __ContosoPizza.http__
2. Add a new __GET__ to call the ContosoPizza endpoint

    ```
    GET {{ContosoPizza_HostAddress}}/ContosoPizza/
    Accept: application/json

    GET {{ContosoPizza_HostAddress}}/WeatherForecast/
    Accept: application/json

    GET {{ContosoPizza_HostAddress}}/ContosoPizza/1
    Accept: applicationo/json
    ```

3. Select the __Send Request__ command above the GET request.
    The command returns a list of all pizzas in JSON:
    ```output
    HTTP/1.1 200 OK
    Connection: close
    Content-Type: application/json; charset=utf-8
    Date: Sat, 06 Sep 2025 14:22:00 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
    {
        "id": 1,
        "name": "Margherita",
        "isBlutenFree": false,
        "price": 7.50
    },
    {
        "id": 2,
        "name": "Funghi",
        "isBlutenFree": true,
        "price": 8.50
    }
    ]
    ```

    If you'll try request with ``id``, you'll see next responce:
    ```output
    HTTP/1.1 200 OK
    Connection: close
    Content-Type: application/json; charset=utf-8
    Date: Sat, 06 Sep 2025 14:24:45 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    {
    "id": 1,
    "name": "Margherita",
    "isBlutenFree": false,
    "price": 7.50
    }
    ```

    And also, if you'll try to put there ``id`` which doesn't exist. You'll se a ``404 Not Found`` error with the following output:

    ```output
    HTTP/1.1 404 Not Found
    Connection: close
    Content-Type: application/problem+json; charset=utf-8
    Date: Sat, 06 Sep 2025 14:26:25 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    {
    "type": "https://tools.ietf.org/html/rfc9110#section-15.5.5",
    "title": "Not Found",
    "status": 404,
    "traceId": "00-a85e6594b1896ee28c3e6dfc43abd48c-ab887d148c3f9737-00"
    }
    ```

## Command Line HTTP Read-Eval-Print Loop (REPL)
1. Open the ``httprepl`` terminal;
2. Connect to your web API by running the following command:
    
    ```.NET CLI
    httprepl https://localhost:{port}
    ```

    Alternatively, run next line at any time while ``HttpRepl`` is running:
    
    ```.NET CLI
    connect https://localhost:{port}
    ```
    
3. To see the available endpoints, run the following command:

    ```.NET CLI
    ls
    ```

    The above command detects all APIs available on the connected endpoint. It will look like this:

    ```output
    https://localhost:{PORT}/> ls
    .                 []
    ContosoPizza      [GET]
    WeatherForecast   [GET]
    ```

4. Go to the ``ContosoPizza`` endpoint by running next command:

    ```.NET CLI
    cd ContosoPizza
    ```

    The preceding command shows an output of available APIs for the ``ContosoPizza`` endpoint:

    ```output
    https://localhost:{PORT}/> cd ContosoPizza
    /ContosoPizza    [GET]
    ```

5. Make a ``GET`` request in ``HttpRepl`` by using the following command:
    
    ```.NET CLI
    get
    ```

    The output will return a list of all pizzas in JSON:

    ```output
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Sat, 06 Sep 2025 14:39:27 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
    {
        "id": 1,
        "name": "Margherita",
        "isBlutenFree": false,
        "price": 7,5
    },
    {
        "id": 2,
        "name": "Funghi",
        "isBlutenFree": true,
        "price": 8,5
    }
    ]
    ```

6. To query for a single pizza, make another ``GET`` request, but pass in an ``id`` parameter:

    ```.NET CLI
    get 1
    ```

    You'll see something like this:

    ```output
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Sat, 06 Sep 2025 14:41:55 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    {
    "id": 1,
    "name": "Margherita",
    "isBlutenFree": false,
    "price": 7,5
    }
    ```

    The API also handless situations where the item doesn't exist. Call the API again, but pass in an invalid pizza ``id``:

    ```.NET CLI
    get 5
    ```

    ```output
    HTTP/1.1 404 Not Found
    Content-Type: application/problem+json; charset=utf-8
    Date: Sat, 06 Sep 2025 14:43:30 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    {
    "type": "https://tools.ietf.org/html/rfc9110#section-15.5.5",
    "title": "Not Found",
    "status": 404,
    "traceId": "00-0115864da252cc4d9c1bbaa53b59cd83-2303a214f54fcf3e-00"
    }
    ```

Return to the ``dotnet`` terminal and shut down the web API by selection CTRL+C on your keyboard.

# 🔗 Links
There are some necessary links which you'll need if you want to read some intresting info about this guide and ASP.NET
- [ASP.NET Core tutorial: Controllers](https://learn.microsoft.com/en-us/training/modules/build-web-api-aspnet-core/1-introduction)
- [OpenAPI](https://learn.microsoft.com/uk-ua/aspnet/core/fundamentals/openapi/aspnetcore-openapi?view=aspnetcore-9.0&tabs=visual-studio%2Cvisual-studio-code)
- [HttpRepl](https://learn.microsoft.com/uk-ua/aspnet/core/web-api/http-repl/?view=aspnetcore-9.0&tabs=windows)
- [Error: unable to find an openapi description](https://learn.microsoft.com/en-us/answers/questions/1665010/i-received-the-error-unable-to-find-an-openapi-des)
- [Create web APIs with ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/web-api/?view=aspnetcore-9.0#attribute-routing-requirement)
