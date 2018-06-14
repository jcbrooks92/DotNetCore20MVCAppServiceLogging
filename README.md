# DotNetCore20MVCAppServiceLogging

Welcome to the DotNet (.Net) Core 2.0 MVC Logging with App Services.

Since there isn't a full project example that I could I find I decided to create one. This should just be used as a reference to get started. Thanks to the StackOverflow user Lars for his reference : [Stackoverflowpost](https://stackoverflow.com/questions/49111671/where-does-the-asp-net-core-logging-api-as-default-store-logs)

Startup.cs should look similar to this, note the important parts are surrounded by ** **:
```
     public void Configure(IApplicationBuilder app, IHostingEnvironment env, **ILoggerFactory loggerFactory**)
        {
            if (env.IsDevelopment())
            {
                app.UseBrowserLink();
                app.UseDeveloperExceptionPage();
            }
            else
            {
                app.UseExceptionHandler("/Error");
            }

            loggerFactory.AddDebug();
            **loggerFactory.AddAzureWebAppDiagnostics();**

            app.UseStaticFiles();

            app.UseMvc();
        }
```

The appsettings.json should reflect the log level you are wanting to use:
```
{
  "Logging": {
    "IncludeScopes": false,
    "LogLevel": {
      "Default": "Information"
    }
  }
}
```

Finally your Controller (I used Home Controller similar to the StackOverflow post) should look similar to this:
```
 public class HomeController : Controller
    {
        private readonly ILogger<HomeController> _logger;

        public HomeController(ILogger<HomeController> logger)
        {
            _logger = logger;
        }

        public IActionResult Index()
        {
            _logger.LogInformation("TEST INDEX LOGGER");

            return View();
        }

        public IActionResult About()
        {
            _logger.LogInformation("TEST ABOUT LOGGER");
            ViewData["Message"] = "Your application description page.";

            return View();
        }

        public IActionResult Contact()
        {
            _logger.LogInformation("TEST CONTACT LOGGER");
            ViewData["Message"] = "Your contact page.";

            return View();
        }

        public IActionResult Error()
        {
            return View(new ErrorViewModel { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
        }
    }
}

```

Tags : .net core 2.0 app service logging ILogger ILoggerFactory Diagnostic Logging MVC
