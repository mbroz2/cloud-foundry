---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-02"
subcollection: cloud-foundry
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}


# Copy required files to build output folder
{: #copy_files_build_output}

You can use either the project.json tooling or MSBuild tooling to ensure that your application has all of its required files in the build output folder.
{: shortdesc}


## Use project.json tooling
{: #projectjson}

Add the following property to the `buildOptions` section of the `project.json` file.
```
  "copyToOutput": {
    "include": [
      "wwwroot",
      "Areas/**/Views",
      "Views",
      "appsettings.json"
    ]
  }
```
{: codeblock}

In Startup.cs `Startup` method, remove the following line.
```
  .SetBasePath(env.ContentRootPath)
```
{: codeblock}

In Program.cs `Main` method, remove the following line.
```
  .UseContentRoot(Directory.GetCurrentDirectory())
```
{: codeblock}

These changes should allow the .NET CLI to find your application's `Views` as they will now be copied to the build output when the `dotnet run` command executes.  If your application has any other files, such as JSON configuration files, that are required at runtime, also add the files to the `include` section of `copyToOutput` in the `project.json` file for your project.

## Use MSBuild tooling
{: #msbuild}

Add a `<Content>` element to the `<ItemGroup>` element of your `.csproj` file.
```
  <ItemGroup>
    <Content Include="wwwroot/**/*;Areas/**/Views/*;Views/*;appsettings.json">
      <CopyToOutputDirectory>Always</CopyToOutputDirectory>
      <CopyToPublishDirectory>Always</CopyToPublishDirectory>
    </Content>
  </ItemGroup>
```
{: codeblock}

In the `Startup.cs` `Startup` method, remove the following line.
```
  .SetBasePath(env.ContentRootPath)
```
{: codeblock}

In the `Program.cs` `Main` method, remove the following line.
```
  .UseContentRoot(Directory.GetCurrentDirectory())
```
{: codeblock}

These changes allow the .NET CLI to find your application's `Views` as they are copied to the build output when the `dotnet publish` command executes.  If your application has any other files, such as JSON configuration files, that are required at runtime, also add those to the `Include` property of the `Content` element in the .csproj file for your project, separated by semi-colons.
