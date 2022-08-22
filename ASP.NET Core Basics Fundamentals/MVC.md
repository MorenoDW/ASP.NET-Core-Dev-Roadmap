# ASP.NET Core MVC

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
