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

# Links
There are some necessary links which you'll need if you want to read some intresting info about this guide and ASP.NET
- [ASP.NET Core tutorial: Controllers](https://learn.microsoft.com/en-us/training/modules/build-web-api-aspnet-core/1-introduction)
- [OpenAPI](https://learn.microsoft.com/uk-ua/aspnet/core/fundamentals/openapi/aspnetcore-openapi?view=aspnetcore-9.0&tabs=visual-studio%2Cvisual-studio-code)
- [HttpRepl](https://learn.microsoft.com/uk-ua/aspnet/core/web-api/http-repl/?view=aspnetcore-9.0&tabs=windows)
- [Error: unable to find an openapi description](https://learn.microsoft.com/en-us/answers/questions/1665010/i-received-the-error-unable-to-find-an-openapi-des)

