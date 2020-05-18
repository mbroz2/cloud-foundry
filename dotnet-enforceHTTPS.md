---

copyright:
  years: 2015, 2018
lastupdated: "2017-10-25"
subcollection: cloud-foundry
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}

# Enforce HTTPS on all pages in your application
{: #enforce_https}

To enforce HTTPS instead of HTTP on all pages in your application when it runs in {{site.data.keyword.Bluemix}}, you need to make the following changes to your application.

Add the following `using` statements in the `Startup` class:

```
  using Microsoft.AspNetCore.Mvc;
  using Microsoft.AspNetCore.Rewrite;
```

Add the following to the `ConfigureServices` method in the `Startup` class:

```
  // if app is running in {{site.data.keyword.Bluemix_notm}}
  if (!string.IsNullOrEmpty(Environment.GetEnvironmentVariable("BLUEMIX_REGION")))
  {
    services.Configure<MvcOptions>(options =>
    {
      options.Filters.Add(new RequireHttpsAttribute());
    });
  }
```

Add the following to your `Configure` method in the `Startup` class:

```
  // if app is running in {{site.data.keyword.Bluemix_notm}}
  if (!string.IsNullOrEmpty(Environment.GetEnvironmentVariable("BLUEMIX_REGION")))
  {
    app.Use(async (context, next) =>
      {
        if (string.Equals(context.Request.Headers["X-Forwarded-Proto"], "https", StringComparison.OrdinalIgnoreCase))
        {
          context.Request.Scheme = "https";
        }

        await next();
      });
    var options = new RewriteOptions()
                .AddRedirectToHttps();

    app.UseRewriter(options);
  }
```
