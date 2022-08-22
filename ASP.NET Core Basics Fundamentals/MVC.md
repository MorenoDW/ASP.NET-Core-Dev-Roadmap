# Overview ASP.NET Core MVC

ASP.NET Core MVC is a rich framework for building web apps and APIs using the Model-View-Controller design pattern.

## MVC Pattern

The Model-View-Controller (MVC) architectural pattern separates an application into three main groups of components: Models, Views and Controllers. This pattern helps to achieve [separation of concerns](https://docs.microsoft.com/en-us/dotnet/architecture/modern-web-apps-azure/architectural-principles#separation-of-concerns). Using this pattern, user requests are routed to a Controller which is responsible for working with the Model to perform user actions and/or retrieve results of queries. The Controller chooses the View to display to the user, and provides it with ani Model data it requires.

The following diagarams shows the three main components and which ones reference the others:

![MVC pattern](https://docs.microsoft.com/en-us/aspnet/core/mvc/overview/_static/mvc.png?view=aspnetcore-6.0)

This delineation of responsabilities helps you scale the application in terms of complexity because it's easier to code, debug and test something (model, view or controller) that has a single job. It's more difficult to update, test and debug code that has dependecies spread across two or more of these areas. For example, user interface logic tends to change more frequently that business logic. If presentation code and business logic are combined in a single object, and object containing business logic must be modified every time the user interface is changed. This often introduces errors and requires the retesting of business logic after minimal user interface change.

#### Note
Both the view and the controller depend on the model. However, the model depends on neither the view nor the controller. This is one of the key benefits of the separation. This separation allows the model to be built and tested independent of the visual presentation.

## Model Responsibilities

The Model in an MVC application represents the state of the applciation and any business logic or operations that should be performed by it. Business logic should be encapsulated in the model, along with any implementation logic for the persisting of the state of the application. Strongly-typed views tipically use ViewModel types designed to contain the data to display on that view. The controller creates and populates these ViewModel instances from the model.

## View Responsibilities

Views are responsible for presenting content through the user interface. They use the [Razor view engine](https://docs.microsoft.com/en-us/aspnet/core/mvc/overview?view=aspnetcore-6.0#razor-view-engine) to embed .NET code in HTML markup. There should be minimal logic within views, and any logic in them should relate to presenting content. If you find the need to perform a great deal of logic in views files in order to display data from a complex model, consider using a [View Component](https://docs.microsoft.com/en-us/aspnet/core/mvc/views/view-components?view=aspnetcore-6.0), ViewModel or view template to simplify the view.

## Controller Responsibilities

Controllers are the component that handle user interaction, wotk with the model and ultimately select a view to render. In am MVC application, the view only displays information; the controller handles and responds to user input and interaction. In the MVC pattern, the controller is the initial entry point, and is responsible for selecting which model types to work with and which view to render (hence its name - it controls how the app responds to a given request).

#### Note 
Controllers shouldn't be overly complicated by too many responsibilities. To keep controller logic from becoming overly complex, push business logic out of the controller and into the domain model.
#### Tip
If you find that your controller actions frequently perform the same kinds of actions, move these common actions into filters.

# ASP.NET Core MVC

The ASP.NET Core MVC framework is a lightweight, open source, highly testable presentation framework optimized for use with ASP.NET Core.

ASP.NET Core MVC provides a patterns-based way to build dynamic websites that enables a clean separation of concerns. It gives you full control over markup, supports TDD-friendly development and uses the latest web standards.

## Routing

ASP.NET Core MVC is built on top of [ASP:NET Core's routing](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/routing?view=aspnetcore-6.0), a powerful URL-mapping component that lets you build applications that have comprehensible and searchable URLs. This enables you to define your application's URL naming patterns that work well for search engine optimization (SEO) and for link generation, without regard for how the files on your web server are organized. You can define your routes using a convenient route template syntax that supports route value constraints, defaults and optional values.

Convention-based routing enables you to globally define the URL formats that your application accepts and how each of those formats maps to a specific action method on a given controller. When an incoming request is received, the routing engine parses the URL and matches it to one of the defined URL formats, and then calls the associated controller's action method.
```cs
routes.MapRoute(name: "Default", template: "{controller=Home}/{action=Index}/{id?}");
```
*Attribute routing* enables you to specify routing information by decorating your controllers and actions with attributes that define your application's routes. This means that your route definitions are placed next to the controller and action with which they're associated.
```cs
[Route("api/[controller]")]
public class ProductsController : Controller
{
    [HttpGet("{id}")]
    public IActionResult GetProduct(int id)
    {
      ...
    }
}
```

# Model binding

ASP.NET Core MVC [model binding](https://docs.microsoft.com/en-us/aspnet/core/mvc/models/model-binding?view=aspnetcore-6.0) converts client request data (form values, route data, query string parameters, HTTP headers) into objects that the controller can handle. As a result, your controller logic doesn't have to do the work of figuring out the incoming request data; it simply has the data as parameters to its action methods.
```cs
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null) { ... }
```
# Model validation

ASP.NET Core MVC supports [validation](https://docs.microsoft.com/en-us/aspnet/core/mvc/models/validation?view=aspnetcore-6.0) by decorating your model object with data annotaion validation attributes. The validation attributes are checked on the client side before values are posted to the server, as well as on the server before the controller action is called.
```cs
using System.ComponentModel.DataAnnotations;
public class LoginViewModel
{
    [Required]
    [EmailAddress]
    public string Email { get; set; }

    [Required]
    [DataType(DataType.Password)]
    public string Password { get; set; }

    [Display(Name = "Remember me?")]
    public bool RememberMe { get; set; }
}
```
A controller action:
```cs
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null)
{
    if (ModelState.IsValid)
    {
      // work with the model
    }
    // At this point, something failed, redisplay form
    return View(model);
}
```
The framework handles validating request data both on the client and on the server. Validation logic specified on model types is added to the rendered views as unobstusive annotations and is enforced in the browser with [JQuery Validation](https://jqueryvalidation.org/).

## Dependency injection

ASP.NET Core has built-in support for [dependency injection (DI)](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/dependency-injection?view=aspnetcore-6.0). In ASP.NET Core MVC, [controller](https://docs.microsoft.com/en-us/aspnet/core/mvc/controllers/dependency-injection?view=aspnetcore-6.0) can request needed services through their constructors, allowing them to follow the [Explicit Dependecies Principles](https://docs.microsoft.com/en-us/dotnet/architecture/modern-web-apps-azure/architectural-principles#explicit-dependencies).

Your app can also use [dependency injection in view files](https://docs.microsoft.com/en-us/aspnet/core/mvc/views/dependency-injection?view=aspnetcore-6.0), using the `@inject` directive:

```cshtml
@inject SomeService ServiceName

<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ServiceName.GetTitle</title>
</head>
<body>
    <h1>@ServiceName.GetTitle</h1>
</body>
</html>
```

## Filters

[Filters](https://docs.microsoft.com/en-us/aspnet/core/mvc/controllers/filters?view=aspnetcore-6.0) help developers encapsulate cross-cutting concerns, like exeption handling or authorization. Filters enable running custom pre- and post-processing logic for action methods, and can be configured to run at certain points within the execution pipeline for a given ewquest. Filters can be applied to controllers or actions as attributes (or can be run globally). Several filters (such as `Autorize`) are included in the framework. `[Authorize]` is the attribute that is used to create MVC authorization filters.
```cs
[Authorize]
public class AccountController : Controller
```
