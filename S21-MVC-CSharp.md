---
title: Model View Controller in C# and Razor - Walkthrough
---
[Model View Controller](https://en.wikipedia.org/wiki/Model-view-controller) (MVC) is a [Design Pattern](https://en.wikipedia.org/wiki/Software_design_pattern) for developing applications with a Web or GUI user interface.

* **Model**: Defines the data to be displayed and manipulated.
* **View**: Renders the data from the model onto the screen - whether through a GUI or through a web interface.
* **Controller**: Takes user input and connects the appropriate model to a corresponding view.

This walkthrough will use [Visual Studio IDE](https://visualstudio.microsoft.com/) to produce a simple, web-based MVC application.

## The Application Shell

Launch Visual Studio and create a new project. Select `ASP.NET Core Web App (Model-View-Controller)` for the project type.

* Set `Authentication` to `None`
* Unselect `Configure for HTTPS` since this will be run locally.
* Click `Create`

In the solution tree, open `Views/Home/Index.cshtml`. This is the default view of the application. It is written in [Razor](https://docs.microsoft.com/en-us/aspnet/core/mvc/views/razor) using HTML mixed with C# code. Much like PHP, the C# code executes on the server side.

Just under the `<h1>` heading add the following:

```c#
<p>The time is @DateTime.Now</p>
```

Click the run button (green triangle with IIS Express next to it). The application will run on a local HTTP server and your browser will be directed to it.

## Create a Model

The model we will build is for a car.

In the solution tree, right-click `Models`. Select `Add > Class`. name the class "Car.cs" and click `Add`.

Fill out the car class as follows:

```
    public class Car
    {
        public string Make { get; set; }
        public string Model { get; set; }
        public int Year { get; set; }

        public override string ToString()
        {
            return $"(make='{Make}' model='{Model}' year='{Year}')";
        }
    }
```

This class represents one record. When storing in a database, C# will use _Object Relational Mapping_ to transfer the properties (Make, Model and Year) between the database table and the program.

You can build and run, but nothing new will happen because we haven't changed anything in the controller or the view.

## Update the Controller

In the solution tree, open `Controllers/HomeController.cs`. and then find the method called `Index()` This is the existing controller that handles references to the root of the server's URL.

> How is that mapping accomplished? Take a look at `Startup.cs`. In there, it has a call to `MapControllerRoute`. The pattern is lists "Home" for the default controller and "Index" for the default action. Since no other controller or action were specified in the URL path, the controller to be selected is the class named `HomeController` and the method to be called on that class is `Index()`.

Change the `Index()` method to read as follows:

```c#
public IActionResult Index()
{
    ViewData["car"] = new Car()
    {
        Make = "Mini",
        Model = "Cooper",
        Year = 2004
    };
    return View();
}
```

This creates a new instance of the `Car` **Model** and fills it in with information. Then stores it in the `ViewData` collection for access from the **View**.

In a real application, you would retrieve the data from a database or some other source rather than hard-coding it as we did here.

## Update the View

The last line of the controller says `return View()`. This function creates the default view for the controller which, in this case, is the view listed under `Views/Home/Index.cshtml` in the solution tree. You can use other methods to create a different view when appropriate as it's the controller's job to determine which view to display.

So, go back to `Index.cshtml` (the same file you modified to display the time and date). At the bottom, or wherever you like, add the following code.

```c#
<p>My car is @ViewData["car"]</p>
```

We just used **Razor** to insert the data into our default HTML page.

## Razor Basics

Links:
* [Razor Syntax](https://docs.microsoft.com/en-us/aspnet/core/mvc/views/razor)
* [Razor Tutorial](https://docs.microsoft.com/en-us/aspnet/core/tutorials/razor-pages)

Razor lets you embed C# code within HTML, much in the same way that PHP lets you embed PHP script within HTML. In the **MVC** pattern, Razor is used to build **Views**.

### Razor Expressions

In **Razor**, you introduce C# code with the `@` character. If the `@` is followed by an expression then the expression will be interpreted and output directly. If the expression starts with a symbol (such as `@ViewData`) then Razor can find the end of the expression without parentheses. Other times, you may have to use parentheses to indicate the beginning and end of the expression.

For example, `@(2 + 2)` will output `4`.

As they are output, expressions are converted to a string and then HTML encoded to prevent cross-site scripting attacks.

To output raw HTML from C#, use `Html.Raw()`. For example, `@Html.Raw("<strong>Hello!</strong>")`.

> Warning: Using `Html.Raw()` on unsanitized user input risks a cross-site scripting attack.

### Razor Code Blocks

A Razor code block starts with `@{` and ends with `}`. Within a code block you can set variables, have branches and loops, call functions, perform calculations, and so forth.

For example:

```c#
@{
    Car car = (Car)ViewData["car"];
}
```

You can use `@if`, `@elseif`, `@else`, `@for`, and `@foreach` to create conditionals and loops with HTML content.

For example:

```c#
@if (tempInF < 60)
{
    <strong>Bring a coat.</strong>
}
```

### Razor sample

Let's use this knowledge to make our car output prettier. Add this to your page.

```c#
<table>
    @{
        Car car = (Car)ViewData["car"];
    }
    @foreach (var prop in car.GetType().GetProperties())
    {
        <tr><th>@prop.Name:</th><td>@prop.GetValue(car)</td></tr>
    }
</table>
```