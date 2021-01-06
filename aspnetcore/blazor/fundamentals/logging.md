---
title: BlazorJournalisation ASP.net Core
author: guardrex
description: En savoir plus sur la journalisation dans Blazor les applications, y compris la configuration du niveau de journalisation et comment écrire des messages de journal à partir de Razor composants.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/16/2020
no-loc:
- appsettings.json
- ASP.NET Core Identity
- cookie
- Cookie
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/fundamentals/logging
zone_pivot_groups: blazor-hosting-models
ms.openlocfilehash: 10c96bd2d0cc64f3bd035e7079b0996eb5768595
ms.sourcegitcommit: 3593c4efa707edeaaceffbfa544f99f41fc62535
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/04/2021
ms.locfileid: "97666831"
---
# <a name="aspnet-core-no-locblazor-logging"></a><span data-ttu-id="7c786-103">BlazorJournalisation ASP.net Core</span><span class="sxs-lookup"><span data-stu-id="7c786-103">ASP.NET Core Blazor logging</span></span>

::: zone pivot="webassembly"

<span data-ttu-id="7c786-104">Configurez la journalisation personnalisée dans Blazor WebAssembly les applications avec la <xref:Microsoft.AspNetCore.Components.WebAssembly.Hosting.WebAssemblyHostBuilder.Logging?displayProperty=nameWithType> propriété.</span><span class="sxs-lookup"><span data-stu-id="7c786-104">Configure custom logging in Blazor WebAssembly apps with the <xref:Microsoft.AspNetCore.Components.WebAssembly.Hosting.WebAssemblyHostBuilder.Logging?displayProperty=nameWithType> property.</span></span>

<span data-ttu-id="7c786-105">Ajoutez l’espace de noms pour <xref:Microsoft.AspNetCore.Components.WebAssembly.Hosting?displayProperty=fullName> à `Program.cs` :</span><span class="sxs-lookup"><span data-stu-id="7c786-105">Add the namespace for <xref:Microsoft.AspNetCore.Components.WebAssembly.Hosting?displayProperty=fullName> to `Program.cs`:</span></span>

```csharp
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;
```

<span data-ttu-id="7c786-106">Dans `Program.Main` de `Program.cs` , définissez le niveau de journalisation minimale avec <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.SetMinimumLevel%2A?displayProperty=nameWithType> et ajoutez le fournisseur de journalisation personnalisé :</span><span class="sxs-lookup"><span data-stu-id="7c786-106">In `Program.Main` of `Program.cs`, set the minimum logging level with <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.SetMinimumLevel%2A?displayProperty=nameWithType> and add the custom logging provider:</span></span>

```csharp
var builder = WebAssemblyHostBuilder.CreateDefault(args);
...
builder.Logging.SetMinimumLevel(LogLevel.Debug);
builder.Logging.AddProvider(new CustomLoggingProvider());
```

<span data-ttu-id="7c786-107">La `Logging` propriété est de type <xref:Microsoft.Extensions.Logging.ILoggingBuilder> , donc toutes les méthodes d’extension disponibles sur <xref:Microsoft.Extensions.Logging.ILoggingBuilder> sont également disponibles sur `Logging` .</span><span class="sxs-lookup"><span data-stu-id="7c786-107">The `Logging` property is of type <xref:Microsoft.Extensions.Logging.ILoggingBuilder>, so all of the extension methods available on <xref:Microsoft.Extensions.Logging.ILoggingBuilder> are also available on `Logging`.</span></span>

<span data-ttu-id="7c786-108">La configuration de la journalisation peut être chargée à partir des fichiers de paramètres d’application.</span><span class="sxs-lookup"><span data-stu-id="7c786-108">Logging configuration can be loaded from app settings files.</span></span> <span data-ttu-id="7c786-109">Pour plus d’informations, consultez <xref:blazor/fundamentals/configuration#logging-configuration>.</span><span class="sxs-lookup"><span data-stu-id="7c786-109">For more information, see <xref:blazor/fundamentals/configuration#logging-configuration>.</span></span>

## <a name="no-locsignalr-net-client-logging"></a><span data-ttu-id="7c786-110">SignalR Journalisation du client .NET</span><span class="sxs-lookup"><span data-stu-id="7c786-110">SignalR .NET client logging</span></span>

<span data-ttu-id="7c786-111">Injectez un <xref:Microsoft.Extensions.Logging.ILoggerProvider> pour ajouter une `WebAssemblyConsoleLogger` aux fournisseurs de journalisation passés à <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder> .</span><span class="sxs-lookup"><span data-stu-id="7c786-111">Inject an <xref:Microsoft.Extensions.Logging.ILoggerProvider> to add a `WebAssemblyConsoleLogger` to the logging providers passed to <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder>.</span></span> <span data-ttu-id="7c786-112">Contrairement à un traditionnel <xref:Microsoft.Extensions.Logging.Console.ConsoleLogger> , `WebAssemblyConsoleLogger` est un wrapper autour des API de journalisation spécifiques aux navigateurs (par exemple, `console.log` ).</span><span class="sxs-lookup"><span data-stu-id="7c786-112">Unlike a traditional <xref:Microsoft.Extensions.Logging.Console.ConsoleLogger>, `WebAssemblyConsoleLogger` is a wrapper around browser-specific logging APIs (for example, `console.log`).</span></span> <span data-ttu-id="7c786-113">L’utilisation de `WebAssemblyConsoleLogger` rend la journalisation possible dans mono dans un contexte de navigateur.</span><span class="sxs-lookup"><span data-stu-id="7c786-113">Use of `WebAssemblyConsoleLogger` makes logging possible within Mono inside a browser context.</span></span>

> [!NOTE]
> <span data-ttu-id="7c786-114">`WebAssemblyConsoleLogger` est [interne](/dotnet/csharp/language-reference/keywords/internal) et n’est pas disponible pour une utilisation directe dans le code du développeur.</span><span class="sxs-lookup"><span data-stu-id="7c786-114">`WebAssemblyConsoleLogger` is [internal](/dotnet/csharp/language-reference/keywords/internal) and not available for direct use in developer code.</span></span>

<span data-ttu-id="7c786-115">Ajoutez l’espace de noms pour <xref:Microsoft.Extensions.Logging?displayProperty=fullName> et injectez <xref:Microsoft.Extensions.Logging.ILoggerProvider> dans le composant :</span><span class="sxs-lookup"><span data-stu-id="7c786-115">Add the namespace for <xref:Microsoft.Extensions.Logging?displayProperty=fullName> and inject an <xref:Microsoft.Extensions.Logging.ILoggerProvider> into the component:</span></span>

```csharp
@using Microsoft.Extensions.Logging
@inject ILoggerProvider LoggerProvider
```

<span data-ttu-id="7c786-116">Dans la [ `OnInitializedAsync` méthode](xref:blazor/components/lifecycle#component-initialization-methods)du composant, utilisez <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilderExtensions.ConfigureLogging%2A?displayProperty=nameWithType> :</span><span class="sxs-lookup"><span data-stu-id="7c786-116">In the component's [`OnInitializedAsync` method](xref:blazor/components/lifecycle#component-initialization-methods), use <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilderExtensions.ConfigureLogging%2A?displayProperty=nameWithType>:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl(NavigationManager.ToAbsoluteUri("/chathub"))
    .ConfigureLogging(logging => logging.AddProvider(LoggerProvider))
    .Build();
```

::: zone-end

::: zone pivot="server"

<span data-ttu-id="7c786-117">Pour obtenir des instructions générales sur la journalisation des ASP.NET Core qui concernent Blazor Server , consultez <xref:fundamentals/logging/index> .</span><span class="sxs-lookup"><span data-stu-id="7c786-117">For general ASP.NET Core logging guidance that pertains to Blazor Server, see <xref:fundamentals/logging/index>.</span></span>

::: zone-end

## <a name="log-in-no-locrazor-components"></a><span data-ttu-id="7c786-118">Connecter les Razor composants</span><span class="sxs-lookup"><span data-stu-id="7c786-118">Log in Razor components</span></span>

<span data-ttu-id="7c786-119">Les journaux respectent la configuration de démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="7c786-119">Loggers respect app startup configuration.</span></span>

<span data-ttu-id="7c786-120">La `using` directive pour <xref:Microsoft.Extensions.Logging> est requise pour prendre en charge les saisies semi-automatiques [IntelliSense](/visualstudio/ide/using-intellisense) pour les API, telles que <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogWarning%2A> et <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError%2A> .</span><span class="sxs-lookup"><span data-stu-id="7c786-120">The `using` directive for <xref:Microsoft.Extensions.Logging> is required to support [IntelliSense](/visualstudio/ide/using-intellisense) completions for APIs, such as <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogWarning%2A> and <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError%2A>.</span></span>

<span data-ttu-id="7c786-121">L’exemple suivant illustre la journalisation avec un <xref:Microsoft.Extensions.Logging.ILogger> dans des composants.</span><span class="sxs-lookup"><span data-stu-id="7c786-121">The following example demonstrates logging with an <xref:Microsoft.Extensions.Logging.ILogger> in components.</span></span>

<span data-ttu-id="7c786-122">`Pages/Counter.razor`:</span><span class="sxs-lookup"><span data-stu-id="7c786-122">`Pages/Counter.razor`:</span></span>

[!code-razor[](logging/samples_snapshot/Counter1.razor?highlight=3,16)]

<span data-ttu-id="7c786-123">L’exemple suivant illustre la journalisation avec un <xref:Microsoft.Extensions.Logging.ILoggerFactory> dans des composants.</span><span class="sxs-lookup"><span data-stu-id="7c786-123">The following example demonstrates logging with an <xref:Microsoft.Extensions.Logging.ILoggerFactory> in components.</span></span>

<span data-ttu-id="7c786-124">`Pages/Counter.razor`:</span><span class="sxs-lookup"><span data-stu-id="7c786-124">`Pages/Counter.razor`:</span></span>

[!code-razor[](logging/samples_snapshot/Counter2.razor?highlight=3,16-17)]

## <a name="additional-resources"></a><span data-ttu-id="7c786-125">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="7c786-125">Additional resources</span></span>

* <xref:fundamentals/logging/index>
