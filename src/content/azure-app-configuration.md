---
layout: post
title: Azure App Configuration
image: img/bird.jpg
author: Thulani S. Chivandikwa
date: 2019-06-13T10:00:00.000Z
tags: [.NET, Azure, App Configuration, Feature Flags]
draft: false
---

I recently did a [post](https://chivandikwa.com/feature-management/) on the Feature Management library created by the the Azure Team, something that can be used outside of Azure to manage and consume feature flag effortlessly. I got curios and started playing with the Azure App Configuration service, as service to centralize management of settings and feature flags. App Configuration is currently in public preview and is free to try out, so do check it out.

### Why should I care

Dynamically change application settings without the need to redeploy or restart an application, point in time replay of setting, native integration with popular framework, encryption at rest and in transit, and more and more...

### I'm sold let's see it in action

For this article I made use of the default Visual Studio MVC web template. You need to add the <code>Microsoft.Extensions.Configuration.AzureAppConfiguration</code> nuget package for the client that we will use to connect to Azure. go ahead to the the Azure console and look for the App Configuration service and create a new one, provide a name and choose the Trial subscription. After deployment is done go to the configuration explorer on this service and create a single setting with the name Test:Settings:Label and value 'Azure App Configuration Test'. Next go to the Access Keys section and take note of the primary readonly key, for now we will only read and this should be sufficient.

![access keys](https://raw.githubusercontent.com/chivandikwa/AzureAppConfigurationRecipes/master/img/access-keys.jpg)
Go back to Visual Studio and in your project enable the secrets manager tool by simply adding this into a csproj file.

```xml
<PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
    <UserSecretsId>[replace-with-guid-here]</UserSecretsId>
</PropertyGroup>
```

Next add your connection string into a user secret by using this command
<code>dotnet user-secrets set ConnectionStrings:AppConfig [replace-with-your-connection-string]</code>

Setup Program as follows

```csharp
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Configuration.AzureAppConfiguration;

namespace AzureAppConfigurationRecipes
{
    public class Program
    {
        public static void Main(string[] args)
        {
            CreateWebHostBuilder(args).Build().Run();
        }

        public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                   .ConfigureAppConfiguration((hostingContext, config) =>
                   {
                       var settings = config.Build();
                       config.AddAzureAppConfiguration(settings["ConnectionStrings:AppConfig"]);
                   })
                .UseStartup<Startup>();
    }
}
```

---

Replace index.cshtml with the following:

```html
@using Microsoft.Extensions.Configuration @{ ViewData["Title"] = "Home Page"; } @inject
IConfiguration Configuration

<div class="text-center">
  <h1>@Configuration["Test:Settings:Label"]</h1>
</div>
```

---

Upon running the application you should see that the value setup in the Azure portal has been successfully pulled out, very easy the client manages it all for us.

### Feature Management

To further make use of the feature management option in Azure App Configuration add the <code>Microsoft.FeatureManagement.AspNetCore</code> package to your project. Go ahead and select the Feature Manager option and create one name Beta and leave it switched off as the default.

![feature-management](https://raw.githubusercontent.com/chivandikwa/AzureAppConfigurationRecipes/master/img/feature-management.jpg)

> Do not be surprised to see a new key appear in the configuration explorer with the key .appconfig.featureflag/Beta and with json content. I am not sure why this has to be visible here or whether it will remain after the preview or not.

Update your Program.cs to the following:

```csharp
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Configuration.AzureAppConfiguration;

namespace AzureAppConfigurationRecipes
{
    public class Program
    {
        public static void Main(string[] args)
        {
            CreateWebHostBuilder(args).Build().Run();
        }

        public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                   .ConfigureAppConfiguration((hostingContext, config) =>
                   {
                       IConfigurationRoot settings = config.Build();
                       config.AddAzureAppConfiguration(options =>
                       {
                           options.Connect(settings["ConnectionStrings:AppConfig"])
                                  .UseFeatureFlags();
                       });
                   })
                   .UseStartup<Startup>();
    }
}
```

---

Add the following to the ConfigureServicesMethod in Startup.cs

```csharp
    services.AddFeatureManagement();
```

---

Introduce a Features enum for easy tracking of features and annotate the Index controller action with the Beta Feature:

```csharp
    public enum Features
    {
        Beta
    }

    [Feature(Features.Beta)]
    public IActionResult Privacy()
    {
        return View();
    }
```

---

Running this application again should give you an HTTP 400 when clicking the privacy button as the feature is currently disabled. While the application is still running, if you go back to the Azure portal and toggle the Beta feature on, wait for about 30 seconds (the default refresh time of the client in your application) then attempt to access the privacy page again this time it should work.

Note how the feature flags auto update but if you change the configuration values they do not auto update. To enable auto update add a watch to the options within the <code>AddAzureAppConfiguration</code> section by adding the method <code>Watch</code>. The first param of this method is the key name and the second is an optional polling interval, default 30 seconds. You can also make use of <code>WatchAndReloadAll</code> to watch for a specific setting change but reload all when it does change.

### Conclusion

This service is definitely very interesting too me and surprising easy to make use of. I am still to play with it somemore to get to grips with more concepts like the point in time snapshots, event handling and going beyond readonly. Hopefully that will be soon and I can report back in another post.

> DON'T forget to cleanup after yourself in Azure and delete any resources you will no longer be using.

Checkout the complete code sample used in this post [on github](https://github.com/chivandikwa/AzureAppConfigurationRecipes).
