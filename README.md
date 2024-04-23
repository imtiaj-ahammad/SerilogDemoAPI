## SerilogDemoAPI




1. Create a folder for the project and get into the directory
    ```
    mkdir SerilogDemoAPI
    cd SerilogDemoAPI
    ```
2. Create a solution named SerilogDemoAPI
    ```
    dotnet new  sln -n SerilogDemoAPI
    ```
3. Create a project into the folder
    ```
    dotnet new webapi -f net6.0 -n SerilogDemoAPI
    ```
4. Add the project into the solution
    ```
    dotnet sln add SerilogDemoAPI/SerilogDemoAPI.csproj
    ```
5. Open the solution with visual studio code
    ```
    code .
    ```
6. Add a gitignore file into the solution
    ```
    dotnet new gitignore
    ```
7. Add a README.md file into the solution
    ```
    touch README.md
    ```
8. Install the following packages
    ```
    dotnet add package Serilog.AspNetCore
    dotnet add package Serilog
    dotnet add package Serilog.Sinks.Console
    dotnet add package Serilog.Sinks.File
    ```
#### Serilog - start
#### Log the information to a text file using the serilog file sink-start
9. Go to program file and inject serilog
    ```
    var logger = new LoggerConfiguration()
    .ReadFrom.Configuration(builder.Configuration)
    .Enrich.FromLogContext()
    .CreateLogger();
    builder.Logging.ClearProviders();
    builder.Logging.AddSerilog(logger);
    ```
10. Now lets configure settings in appSettings.json
    ```
      "Serilog": {
    "Using": [ "Serilog.Sinks.File" ],
    "MinimumLevel": {
      "Default": "Information"
    },
    "WriteTo": [
      {
        "Name": "File",
        "Args": {
          "path": "../logs/webapi-.log",
          "rollingInterval": "Day",
          "outputTemplate": "{Timestamp:yyyy-MM-dd HH:mm:ss.fff zzz} {CorrelationId} {Level:u3} {Username} {Message:lj}{Exception}{NewLine}"
        }
      }
    ]
  }
    ```
11. Let's go to the WeatherForecastController and create some logs
    ```
    _logger.LogInformation("Seri Log is Working");
    ```
#### Log the information to a text file using the serilog file sink-end
#### Log the information to console- start
12. Lets add the config for logging in console
  ``` {
        "Name": "Console",
        "Args": {
          "outputTemplate": "{Timestamp:yyyy-MM-dd HH:mm:ss} [{Level:u3}] {Message}{NewLine}{Exception}"
        }
      }
  ```
#### Log the information to console- end
#### Log the information to db- start
13. Add the following package
  ```
  dotnet add package Serilog.Sinks.MSSqlServer
  ```
14. Add Configuration for writing in  db
  ```
      {
        "Name": "MSSqlServer",
        "Args": {
          "connectionString": "Server=DESKTOP-TM16N21; Database=MovieRentalDb; Trusted_Connection=True; TrustServerCertificate=true",
          "tableName": "Logs",
          "autoCreateSqlTable": true
        }
      }  
  ```
15. Try running the editor in administration mode
#### Log the information to db- end
#### Serilog - end


#### References:
- https://www.c-sharpcorner.com/article/how-to-implement-serilog-in-asp-net-core-web-api/
- https://www.c-sharpcorner.com/article/how-to-implementation-serilog-in-asp-net-core-5-0-application-with-database/
- https://medium.com/@oevrensel/logging-in-asp-net-core-17efca14d953
