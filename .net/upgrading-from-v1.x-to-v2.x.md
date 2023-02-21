# Upgrading from v1.x to v2.x

In **v2.x**, we've changed the **Nuget** package name from **Thundra.Agent.Lambda** to **Thundra.Agent** as well as the top **namespace** as we've started to support **.NET Core 2.1** applications as well.&#x20;

If you are using the Thundra agent's handler already, make sure to adjust it accordingly.&#x20;

### Installing the Agent

With **v2.x**, the Thundra agent will be installed with the following command.

```bash
dotnet add package Thundra.Agent --version $LATEST_VERSION
```

![Latest Version](https://img.shields.io/nuget/v/Thundra.Agent)

With **v1.x**, this was the following.

```bash
dotnet add package Thundra.Agent.Lambda --version $LATEST_VERSION
```

![Latest Version](https://img.shields.io/nuget/v/Thundra.Agent.Lambda)

### Addressing the Handler

Since we've changed the top namespace from **Thundra.Agent.Lambda** to **Thundra.Agent** in **v2.x**, if you are using the Thundra agent's handler already, you would need to change that as well.

```csharp
// Old Handler. For v1.x.
Thundra.Agent.Lambda::Thundra.Agent.Lambda.Core.ThundraProxy::Handle

// New Handler. For v2.x.
Thundra.Agent::Thundra.Agent.Lambda.Core.ThundraProxy::Handle
```

