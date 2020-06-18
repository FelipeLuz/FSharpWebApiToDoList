# F# REST API .NET Core: Using Entity Framework, MVC and PostgreSQL

The objective of this tutorial is to spread the knowledge of the possibilities to use the traditional .NET Core libraries with F very likely the way that they are used in C#, so you can take advantage of your previous knowledge about .NET core libraries and just care about learning the F# syntax.

## Brief Introduction

One of the most popular **ORM **(Object-relational mapping) among the .NET core community is the Entity Framework Core, that distinguishes itself by:

* Being multi-platform;

* EF Core Supports more than one SQL provider: like the **PostgreSQL**, **MySQL**, **SQLite **and the obvious **SQLServer**;

* Having an **in-memory storage system**, that makes it possible to have a development environment without having a relational database set up for testing.

For this project, we are going to use [**.NET core 3.1](https://dotnet.microsoft.com/download/dotnet-core/3.1),** [**Visual Studio Code](https://code.visualstudio.com/)**, [**Entity Framework Core](https://docs.microsoft.com/pt-br/ef/#pivot=entityfmwk)**, [**MVC Core](https://docs.microsoft.com/pt-br/aspnet/core/mvc/)**, and a [**PostgreSQL](https://www.postgresql.org/)** database that will first be stored in memory, and later it will be hosted in [**AWS](https://aws.amazon.com)**. The OS used in this tutorial is [**Ubuntu 18.04](https://ubuntu.com/download/desktop)**, but you can follow it with **Windows** or **Mac OS**, once that all software and extensions needed are multi-platform.

## Let’s start to code

To start our project you need to open the terminal, choose one directory, and type the following commands:

    dotnet new webapi -o FSToDoList -lang f#
    code FSToDoList

The** Visual Studio Code **will open with the **F# Web API .Net Core template**, this template has already a default controller, and you can run it with these **commands** on the terminal **embedded in VS Code**:

    dotnet restore
    dotnet run

![VS Code with its embedded terminal](https://cdn-images-1.medium.com/max/2732/1*7k_Q2SLF1gLBRmFu-NtPRQ.png)*VS Code with its embedded terminal*

If you followed the tutorial so far, you could check your **API** running on [https://localhost:5001/api/values](https://localhost:5001/api/values)

Now we can properly start to code, so first, let’s update project’s dependencies on the file* FSToDoList.fsproj:*

FSToDoLis.fsproj

You must use the command **dotnet restore **in the **embedded VS Code terminal** to download all the project’s dependencies that were added.

Create two new folders: **Data** and **Models.**

Inside of the folder **Models, **you must create a new file and name it as **ToDoItem.fs**. In this file that will be added the **model** for the** EF Core**, bellow is the code that you will need:

ToDoItem.fs

Now, we need the data context for the project, so we will be able to retrieve the information from the database and work with the data collection on the **HTTP requests**. For that, just create a new file named **ToDoContext.fs **inside of the folder **Data**. Below is the code for this data context:

ToDoContext.fs

Create a new file in the folder **Controllers **and name it as **ToDoItemsController.fs; **It is in this file that the HTTP requests are going to be processed.

This demo project will manage the **Get**,** Put**,** Post, **and** Delete** requests. Here is the code for it:

ToDoController.fs

The project is almost complete, just need to update the files **Program.fs** and **Startup.fs **to insert the EF** Core** service.

Program.fs

Startup.fs

Well done! You just need to run the app by using the following command:

    dotnet run

Go to [https://localhost:5001/api/todoitems](https://localhost:5001/api/todoitems), and you will see the items that are hardcoded in the temporary database.

![Response sent by the API](https://cdn-images-1.medium.com/max/2000/1*1hdhkOD5E1lq36aUji4FWg.png)*Response sent by the API*

To test the other functions, you can use an API testing software, e.g., [**curl](https://curl.haxx.se/)** or [**Postman](https://www.getpostman.com/)**.

## Bonus: Add AWS support

You can use an Amazon **RDS** as the SQL provider for this project, to make this possible here are the steps needed:

1. Create an **AWS** account if you don’t have one already.

1. Go to your **Management Console** and search for **RDS.**

1. Create a new **Database** and select **PostgreSQL.**

1. Set up your database by setting up its **template**, **name**, and **credentials**

1. Tip: to avoid a surprise bill, select the **Free tier **template.

![RDS Templates](https://cdn-images-1.medium.com/max/2000/1*vsrer3nBJbEe503E8ozd7Q.png)*RDS Templates*

Now you must update your **appsettings.json **to include the connection string to your** AWS Postgres database**, here’s to code to work over**:**

appsettings.json

Then you need to update the file Startup.cs, first remove the line where the in-memory database is added and then replace it for this:

Startup.fs

Note that the string inside of **GetConnectionString** must be the same as the name set as the identifier at **appsettings.json**.
