# ASP.NET Core MVC

ASP.NET Core MVC is a rich framework for building web apps and APIs using the Model-View-Controller design pattern.

## MVC Pattern

The Model-View-Controller (MVC) architectural pattern separates an application into three main groups of components: Models, Views and Controllers. This pattern helps to achieve [separation of concerns](https://docs.microsoft.com/en-us/dotnet/architecture/modern-web-apps-azure/architectural-principles#separation-of-concerns). Using this pattern, user requests are routed to a Controller which is responsible for working with the Model to perform user actions and/or retrieve results of queries. The Controller chooses the View to display to the user, and provides it with ani Model data it requires.

The following diagarams shows the three main components and which ones reference the others:

![MVC pattern](https://docs.microsoft.com/en-us/aspnet/core/mvc/overview/_static/mvc.png?view=aspnetcore-6.0)


