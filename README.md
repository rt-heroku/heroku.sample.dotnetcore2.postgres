# Softtrends .NET Core 2.x Buildpack for Heroku with Heroku Postgres
## (Sample Code with Heroku Button deploying ASP.Net Core 2.x  + Heroku Postgres)

This uses the .Net Core 2.x Buildpack provided by Softtends and adds full support for Heroku Postgres<br>

We've made some big updates in this release, so it’s **important** that you spend a few minutes to learn what’s new.

You've created a new ASP.NET Core MVC project. [Learn what's new](https://go.microsoft.com/fwlink/?LinkId=518016)

You need to make the following changes in your Program.cs and Startup.cs to deploy on Heroku
<br/>
In **Program.cs**

*   Add UseUrls method and pass args[0] as parameter to start your app. Because Heroku web dyno will start with dynamic port after sucessful deployment. We need to use the same port in code behind also then only your app will start and listen on that port else dotnet runtime will set default port 5000. Thereby we pass port number as parameter with url in Procfile
<br/>
public static void Main(string[] args
{<br/>
            {
            BuildWebHost(args).Run();
        }
                public static IWebHost BuildWebHost(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseStartup<Startup>()
                .Build();
}<br/>

In **Startup.cs**

	public void ConfigureServices(IServiceCollection services)
        {
            // Add framework services.
            //services.AddApplicationInsightsTelemetry(Configuration);
            services.AddMemoryCache(options =>
            {
                options.ExpirationScanFrequency = TimeSpan.FromMinutes(300);
            });
            services.AddSession();

            services.AddMvc();

            //Get Database Connection 
            string _connectionString = Environment.GetEnvironmentVariable("DATABASE_URL");
            _connectionString.Replace("//", "");

            char[] delimiterChars = { '/', ':', '@', '?' };
            string[] strConn = _connectionString.Split(delimiterChars);
            strConn = strConn.Where(x => !string.IsNullOrEmpty(x)).ToArray();

            Config.User = strConn[1];
            Config.Pass = strConn[2];
            Config.Server = strConn[3];
            Config.Database = strConn[5];
            Config.Port = strConn[4];
            Config.ConnectionString = "host=" + Config.Server + ";port=" + Config.Port + ";database=" + Config.Database + ";uid=" + Config.User + ";pwd=" + Config.Pass + ";sslmode=Require;Trust Server Certificate=true;Timeout=1000";

        }
<br/>
<br/>
You can deploy this ASP.Net MVC website on Heroku server by clicking below button
<br/>
<br/>
<a href="https://heroku.com/deploy?template=https://github.com/heroku-softtrends/heroku.sample.dotnetcore2.postgres/tree/master">
  <img src="https://www.herokucdn.com/deploy/button.svg" alt="Deploy">
</a>

## This application consists of:

*   Sample pages using ASP.NET Core 2.x MVC
*   [Gulp](https://go.microsoft.com/fwlink/?LinkId=518007) and [Bower](https://go.microsoft.com/fwlink/?LinkId=518004) for managing client-side libraries
*   Theming using [Bootstrap](https://go.microsoft.com/fwlink/?LinkID=398939)

## How to

*   [Add a Controller and View](https://go.microsoft.com/fwlink/?LinkID=398600)
*   [Add an appsetting in config and access it in app.](https://go.microsoft.com/fwlink/?LinkID=699562)
*   [Manage User Secrets using Secret Manager.](https://go.microsoft.com/fwlink/?LinkId=699315)
*   [Use logging to log a message.](https://go.microsoft.com/fwlink/?LinkId=699316)
*   [Add packages using NuGet.](https://go.microsoft.com/fwlink/?LinkId=699317)
*   [Add client packages using Bower.](https://go.microsoft.com/fwlink/?LinkId=699318)
*   [Target development, staging or production environment.](https://go.microsoft.com/fwlink/?LinkId=699319)

## Overview

*   [Conceptual overview of what is ASP.NET Core](https://go.microsoft.com/fwlink/?LinkId=518008)
*   [Fundamentals of ASP.NET Core such as Startup and middleware.](https://go.microsoft.com/fwlink/?LinkId=699320)
*   [Working with Data](https://go.microsoft.com/fwlink/?LinkId=398602)
*   [Security](https://go.microsoft.com/fwlink/?LinkId=398603)
*   [Client side development](https://go.microsoft.com/fwlink/?LinkID=699321)
*   [Develop on different platforms](https://go.microsoft.com/fwlink/?LinkID=699322)
*   [Read more on the documentation site](https://go.microsoft.com/fwlink/?LinkID=699323)

## Run & Deploy

*   [Run your app](https://go.microsoft.com/fwlink/?LinkID=517851)
*   [Run tools such as EF migrations and more](https://go.microsoft.com/fwlink/?LinkID=517853)
*   [Publish to Microsoft Azure Web Apps](https://go.microsoft.com/fwlink/?LinkID=398609)

We would love to hear your [feedback](https://go.microsoft.com/fwlink/?LinkId=518015)
