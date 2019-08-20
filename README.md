# webapp101

This is a very basic web app that can be deployed to Azure.  It doesn't do anything sophisticated and uses NLog to generate basic statements when the app is restarted.  It will spit out the web app slot and instance the application is running on.

## Initial Development

This uses dotnet to create a basic application

```bash
dotnet new webapp -o webapp101 
```
From here, the following was added to the project (.csproj file) definition.

```.net
  <ItemGroup>
    <PackageReference Include="NLog.Web.AspNetCore" Version="4.8.0" />
    <PackageReference Include="NLog" Version="4.5.11" />
    <Content Update="nlog.config" CopyToOutputDirectory="PreserveNewest" />
    <PackageReference Include="NLog.Extensions.Logging" Version="1.4.0" />
  </ItemGroup>
```

These changes were added to Program.cs
```.net
using Microsoft.Extensions.Logging;
using NLog.Web;

namespace webapp101
{
     public class Program
     {
          public static void Main(string[] args)
          {
               var logger = NLog.Web.NLogBuilder.ConfigureNLog("nlog.config").GetCurrentClassLogger();
               try
               {
                    logger.Debug("init main");
               }
          }
     }
}
```

Additionally, changes were made in Startup.cs that admittedly don't use NLog but do log info that finds its way into nlog log files.

## Deployment

Once you have a web app deployed to Azure, configure a git remote site to point to the SCM host.  Once the code is to your liking just perform a git push.

```bash
git remote add azure https://webapp101.scm.azurewebsites.net:443/webapp101.git
git push azure
```

## Usage

You'll no doubt notice this web app is just whatever you get with the default dotnet new command.  If you log into Kudu and head to d:\home\site\wwwroot you'll find output from nlog.

## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

Please make sure to update tests as appropriate.

