+++
title = "How to Create REST API with ASP.NET Core and Entity Framework + MySQL"
[taxonomies]
tags = [ "coding", "c#", "tutorial", "mysql", "database" ]
+++

ASP.NET Core has gained traction throughout the years, and it has become #1 most loved web framework according to the [GitHub developer survey 2020](https://insights.stackoverflow.com/survey/2020#technology-most-loved-dreaded-and-wanted-web-frameworks). One of the things you can make with ASP.NET Core is a **REST API**, along with other things like MVC web app, server rendered view with Razor template--similar to Java's JSP, Thymeleaf, or PHP's Blade template engine--and others.

In this guide, we are going to create an company management system in the form of REST API with ASP.NET Core, Entity Framework, and MySQL database (provided in XAMPP). We'll be using the **Dotnet CLI** because of its availability in both Linux and Windows platform, in contrast to Visual Studio IDE which is only available in Windows.

## Prerequisites

1. [Dotnet CLI](https://docs.microsoft.com/en-us/dotnet/core/install/) with .NET 5.0 SDK

Install the dotnet CLI for the specific platform that you use (Windows/Linux/MacOS) provided with the link above.

Verify that the Dotnet CLI is already installed by entering this command on Windows (CMD) or Terminal (Linux):

```cmd
dotnet
```

2. [Dotnet EF CLI Tool](https://docs.microsoft.com/en-us/ef/core/cli/dotnet)

Install the `dotnet ef` command with the cli tool which has been installed. Typically looks like this

```sh
dotnet tool install --global dotnet-ef
```

Note that you might want to add the installed `dotnet-ef` to your system's **PATH**. In Linux, typically the path is `~/.dotnet/tools`.

3. [Visual Studio Code with C# Plugin](https://code.visualstudio.com/)

Install Visual Studio Code and search for the C# plugin in the extensions marketplace.

<p align="center">
  <img src="/asset-0004-cs-vscode.png" />
</p>

4. [XAMPP for MySQL Database](https://www.apachefriends.org/download.html)

Install XAMPP for the specific platform that you use (Windows/Linux/MacOS) provided with the link above.

In Linux, to start XAMPP you need to run this command if you provide default installation path:

```sh
sudo /opt/lampp/lampp start
```

In Windows, you can start the **XAMPP control panel** and activate **MySQL** and **Apache**.

Once XAMPP has finished starting, to enter the **phpmyadmin** database management system open up your web browser and go to:

```
http://localhost/phpmyadmin
```

You can try creating databases there.

Or if you want to access MySQL through shell, do this command:

- Windows

```cmd
cd c:\xampp\mysql\bin
mysql.exe -u root --password
```

- Linux

```
sudo /opt/lampp/bin/mysql -u root
```

## Creating the project

To create a new ASP.NET Core project, you can follow the instructions below which creates new project with the `webapi` template. To create a project with another template, visit [here](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-new#arguments) to show the available templates for `dotnet new`.

```sh
dotnet new webapi -o companymanagement
cd companymanagement
```

## Installing Dependencies

An empty ASP.NET Core `webapi` project does not include the **Entity Framework Core** and **MySQL driver** by default, but we need it in order to complete this guide. To install it, run these commands:
[Source](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core-example.html)

```sh
dotnet add package MySql.EntityFrameworkCore --version 5.0.0+m8.0.23
dotnet add package Microsoft.EntityFrameworkCore.Tools
```

## Running the Project

To run the project, enter the command below in terminal/cmd. But this will do nothing because we haven't added anything yet in our project.  

```sh
dotnet run
```

However you can view the example provided by the `webapi` template, in `https://localhost:5001/swagger`. Your browser will throw a warning because of unsafe HTTPS connection, but just ignore the warning and just proceed, it's safe.



## Creating Models

An Entity Framework **model** is a representation of a database table.

You only need to define C# classes and add and some attributes in order for Entity Framework to automatically map these classes to MySQL tables, so you don't have to write SQL by hand.

Create a file called `Model.cs` and add the code below into there.

```cs
// Model.cs

using System.ComponentModel.DataAnnotations.Schema;

namespace companymanagement
{
    public class Company : BaseModel
    {
        public string Name { get; set; }
    }

    public class Employee : BaseModel
    {
        public string Name { get; set; }
        public string CompanyID { get; set; }

    }

    public class BaseModel
    {
        [DatabaseGenerated(DatabaseGeneratedOption.Identity)]
        public long ID { get; set; }
    }
}
```

In order for Entity Framework to be able to map the classes to a database, you need to add a context class which extends `DbContext`. Here you also must provide the details for connecting to your MySQL database. For XAMPP, put this detail in `UseMySQL` function:

```
server=localhost;database=companymanagement;user=root;password=
```

Put this

- **inside** `namespace companymanagement`
- and **below** `public class BaseModel`
- and also add `using Microsoft.EntityFrameworkCore`

```cs
// Model.cs

using System.ComponentModel.DataAnnotations.Schema;

using Microsoft.EntityFrameworkCore; // +++ Add this line

namespace companymanagement
{
    // public class BaseModel
    ...

    // Context
    public class AppContext : DbContext
    {
        public DbSet<Company> Company { get; set; }
        public DbSet<Employee> Employee { get; set; }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseMySQL("server=localhost;database=companymanagement;user=root;password=");
        }
    }
}
```

## Migrating models

In order for Entity Framework to actually map your model into your database, you'll have to migrate it with a CLI command.  
Add the migration script in `Program.cs` in your program startup file:

```cs
// Program.cs

using Microsoft.EntityFrameworkCore; // +++ Add this line

...

public static void Main(string[] args)
{
    // Migrate at the start of the program
    using (var context = new AppContext())
    {
        context.Database.Migrate();
    }

    CreateHostBuilder(args).Build().Run();
}
...
```

Then in project root, run:

```sh
# you can change 'mymigration' to anything you like
dotnet ef migrations add mymigration
```

Ensure the table `companymanagement` exists in your database, and then finally run:

```sh
dotnet run
```

The REST server will start. The tables and fields should already be in the database.

Each time your model changes and you want to refresh your tables, you'll have to run:

```sh
dotnet ef migrations add <migration_name>
dotnet run
```

## Creating Controllers

**WIP**

```cs
// Controllers/CompanyController.cs

using Microsoft.AspNetCore.Mvc;

namespace companymanagement.Controllers
{
    [ApiController]
    [Route("[controller]")]
    public class CompanyController : ControllerBase
    {
        private readonly AppContext _context;

        public CompanyController()
        {
            _context = new AppContext();
        }

        [HttpGet("/api/Companies")]
        public dynamic All()
        {
            return _context.Company;
        }
    }
}
```

## Disable HTTPS Redirection

**WIP**

```cs
// Startup.cs

public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...

    // app.UseHttpsRedirection();

    ...
}
```
