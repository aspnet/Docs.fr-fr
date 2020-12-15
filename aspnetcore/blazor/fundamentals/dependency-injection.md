---
title: ASP.NET Core l' Blazor injection de dépendances
author: guardrex
description: Découvrez comment les Blazor applications peuvent injecter des services dans des composants.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/11/2020
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
uid: blazor/fundamentals/dependency-injection
zone_pivot_groups: blazor-hosting-models
ms.openlocfilehash: af6b645fc3c398414c85c78e1cfeb213e538c2a6
ms.sourcegitcommit: 6b87f2e064cea02e65dacd206394b44f5c604282
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/15/2020
ms.locfileid: "97506797"
---
# <a name="aspnet-core-no-locblazor-dependency-injection"></a><span data-ttu-id="85c0e-103">ASP.NET Core l' Blazor injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="85c0e-103">ASP.NET Core Blazor dependency injection</span></span>

<span data-ttu-id="85c0e-104">Par [Rainer Stropek](https://www.timecockpit.com) et [Mike Rousos](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="85c0e-104">By [Rainer Stropek](https://www.timecockpit.com) and [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="85c0e-105">L' [injection de dépendances (di)](xref:fundamentals/dependency-injection) est une technique qui consiste à accéder aux services configurés dans un emplacement central :</span><span class="sxs-lookup"><span data-stu-id="85c0e-105">[Dependency injection (DI)](xref:fundamentals/dependency-injection) is a technique for accessing services configured in a central location:</span></span>

* <span data-ttu-id="85c0e-106">Les services inscrits au Framework peuvent être injectés directement dans des composants d' Blazor applications.</span><span class="sxs-lookup"><span data-stu-id="85c0e-106">Framework-registered services can be injected directly into components of Blazor apps.</span></span>
* <span data-ttu-id="85c0e-107">Blazor les applications définissent et enregistrent des services personnalisés et les rendent disponibles dans toute l’application via l’injection de transactions.</span><span class="sxs-lookup"><span data-stu-id="85c0e-107">Blazor apps define and register custom services and make them available throughout the app via DI.</span></span>

## <a name="default-services"></a><span data-ttu-id="85c0e-108">Services par défaut</span><span class="sxs-lookup"><span data-stu-id="85c0e-108">Default services</span></span>

<span data-ttu-id="85c0e-109">Les services présentés dans le tableau suivant sont couramment utilisés dans les Blazor applications.</span><span class="sxs-lookup"><span data-stu-id="85c0e-109">The services shown the following table are commonly used in Blazor apps.</span></span>

| <span data-ttu-id="85c0e-110">Service</span><span class="sxs-lookup"><span data-stu-id="85c0e-110">Service</span></span> | <span data-ttu-id="85c0e-111">Durée de vie</span><span class="sxs-lookup"><span data-stu-id="85c0e-111">Lifetime</span></span> | <span data-ttu-id="85c0e-112">Description</span><span class="sxs-lookup"><span data-stu-id="85c0e-112">Description</span></span> |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | <span data-ttu-id="85c0e-113">Délimité</span><span class="sxs-lookup"><span data-stu-id="85c0e-113">Scoped</span></span> | <p><span data-ttu-id="85c0e-114">Fournit des méthodes pour envoyer des requêtes HTTP et recevoir des réponses HTTP d’une ressource identifiée par un URI.</span><span class="sxs-lookup"><span data-stu-id="85c0e-114">Provides methods for sending HTTP requests and receiving HTTP responses from a resource identified by a URI.</span></span></p><p><span data-ttu-id="85c0e-115">L’instance de <xref:System.Net.Http.HttpClient> dans une Blazor WebAssembly application utilise le navigateur pour gérer le trafic HTTP en arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="85c0e-115">The instance of <xref:System.Net.Http.HttpClient> in a Blazor WebAssembly app uses the browser for handling the HTTP traffic in the background.</span></span></p><p><span data-ttu-id="85c0e-116">Blazor Server par défaut, les applications n’incluent pas une <xref:System.Net.Http.HttpClient> configuration en tant que service.</span><span class="sxs-lookup"><span data-stu-id="85c0e-116">Blazor Server apps don't include an <xref:System.Net.Http.HttpClient> configured as a service by default.</span></span> <span data-ttu-id="85c0e-117">Fournissez un <xref:System.Net.Http.HttpClient> à une Blazor Server application.</span><span class="sxs-lookup"><span data-stu-id="85c0e-117">Provide an <xref:System.Net.Http.HttpClient> to a Blazor Server app.</span></span></p><p><span data-ttu-id="85c0e-118">Pour plus d’informations, consultez <xref:blazor/call-web-api>.</span><span class="sxs-lookup"><span data-stu-id="85c0e-118">For more information, see <xref:blazor/call-web-api>.</span></span></p><p><span data-ttu-id="85c0e-119">Un <xref:System.Net.Http.HttpClient> est inscrit en tant que service étendu, et non Singleton.</span><span class="sxs-lookup"><span data-stu-id="85c0e-119">An <xref:System.Net.Http.HttpClient> is registered as a scoped service, not singleton.</span></span> <span data-ttu-id="85c0e-120">Pour plus d’informations, consultez la section [durée de vie du service](#service-lifetime) .</span><span class="sxs-lookup"><span data-stu-id="85c0e-120">For more information, see the [Service lifetime](#service-lifetime) section.</span></span></p> |
| <xref:Microsoft.JSInterop.IJSRuntime> | <p><span data-ttu-id="85c0e-121">**Blazor WebAssembly**: Singleton</span><span class="sxs-lookup"><span data-stu-id="85c0e-121">**Blazor WebAssembly**: Singleton</span></span></p><p><span data-ttu-id="85c0e-122">**Blazor Server**: Étendu</span><span class="sxs-lookup"><span data-stu-id="85c0e-122">**Blazor Server**: Scoped</span></span></p> | <span data-ttu-id="85c0e-123">Représente une instance d’un Runtime JavaScript dans laquelle les appels JavaScript sont distribués.</span><span class="sxs-lookup"><span data-stu-id="85c0e-123">Represents an instance of a JavaScript runtime where JavaScript calls are dispatched.</span></span> <span data-ttu-id="85c0e-124">Pour plus d’informations, consultez <xref:blazor/call-javascript-from-dotnet>.</span><span class="sxs-lookup"><span data-stu-id="85c0e-124">For more information, see <xref:blazor/call-javascript-from-dotnet>.</span></span> |
| <xref:Microsoft.AspNetCore.Components.NavigationManager> | <p><span data-ttu-id="85c0e-125">**Blazor WebAssembly**: Singleton</span><span class="sxs-lookup"><span data-stu-id="85c0e-125">**Blazor WebAssembly**: Singleton</span></span></p><p><span data-ttu-id="85c0e-126">**Blazor Server**: Étendu</span><span class="sxs-lookup"><span data-stu-id="85c0e-126">**Blazor Server**: Scoped</span></span></p> | <span data-ttu-id="85c0e-127">Contient des assistances pour l’utilisation des URI et de l’état de navigation.</span><span class="sxs-lookup"><span data-stu-id="85c0e-127">Contains helpers for working with URIs and navigation state.</span></span> <span data-ttu-id="85c0e-128">Pour plus d’informations, consultez [URI et assistance de l’état de navigation](xref:blazor/fundamentals/routing#uri-and-navigation-state-helpers).</span><span class="sxs-lookup"><span data-stu-id="85c0e-128">For more information, see [URI and navigation state helpers](xref:blazor/fundamentals/routing#uri-and-navigation-state-helpers).</span></span> |

<span data-ttu-id="85c0e-129">Un fournisseur de services personnalisé ne fournit pas automatiquement les services par défaut indiqués dans le tableau.</span><span class="sxs-lookup"><span data-stu-id="85c0e-129">A custom service provider doesn't automatically provide the default services listed in the table.</span></span> <span data-ttu-id="85c0e-130">Si vous utilisez un fournisseur de services personnalisé et que vous avez besoin de l’un des services répertoriés dans le tableau, ajoutez les services requis au nouveau fournisseur de services.</span><span class="sxs-lookup"><span data-stu-id="85c0e-130">If you use a custom service provider and require any of the services shown in the table, add the required services to the new service provider.</span></span>

## <a name="add-services-to-an-app"></a><span data-ttu-id="85c0e-131">Ajouter des services à une application</span><span class="sxs-lookup"><span data-stu-id="85c0e-131">Add services to an app</span></span>

::: zone pivot="webassembly"

<span data-ttu-id="85c0e-132">Configurez les services de la collection de services de l’application dans la `Program.Main` méthode de `Program.cs` .</span><span class="sxs-lookup"><span data-stu-id="85c0e-132">Configure services for the app's service collection in the `Program.Main` method of `Program.cs`.</span></span> <span data-ttu-id="85c0e-133">Dans l’exemple suivant, l' `MyDependency` implémentation est inscrite pour `IMyDependency` :</span><span class="sxs-lookup"><span data-stu-id="85c0e-133">In the following example, the `MyDependency` implementation is registered for `IMyDependency`:</span></span>

[!code-csharp[](dependency-injection/samples_snapshot/Program1.cs?highlight=7)]

<span data-ttu-id="85c0e-134">Une fois l’hôte généré, les services sont disponibles à partir de l’étendue de l’injection de comptes racine avant le rendu des composants.</span><span class="sxs-lookup"><span data-stu-id="85c0e-134">After the host is built, services are available from the root DI scope before any components are rendered.</span></span> <span data-ttu-id="85c0e-135">Cela peut être utile pour l’exécution de la logique d’initialisation avant le rendu du contenu :</span><span class="sxs-lookup"><span data-stu-id="85c0e-135">This can be useful for running initialization logic before rendering content:</span></span>

[!code-csharp[](dependency-injection/samples_snapshot/Program2.cs?highlight=7,12-13)]

<span data-ttu-id="85c0e-136">L’hôte fournit une instance de configuration centrale pour l’application.</span><span class="sxs-lookup"><span data-stu-id="85c0e-136">The host provides a central configuration instance for the app.</span></span> <span data-ttu-id="85c0e-137">En s’appuyant sur l’exemple précédent, l’URL du service météo est passée d’une source de configuration par défaut (par exemple, `appsettings.json` ) à `InitializeWeatherAsync` :</span><span class="sxs-lookup"><span data-stu-id="85c0e-137">Building on the preceding example, the weather service's URL is passed from a default configuration source (for example, `appsettings.json`) to `InitializeWeatherAsync`:</span></span>

[!code-csharp[](dependency-injection/samples_snapshot/Program3.cs?highlight=13-14)]

::: zone-end

::: zone pivot="server"

<span data-ttu-id="85c0e-138">Après avoir créé une nouvelle application, examinez la `Startup.ConfigureServices` méthode dans `Startup.cs` :</span><span class="sxs-lookup"><span data-stu-id="85c0e-138">After creating a new app, examine the `Startup.ConfigureServices` method in `Startup.cs`:</span></span>

```csharp
using Microsoft.Extensions.DependencyInjection;

...

public void ConfigureServices(IServiceCollection services)
{
    ...
}
```

<span data-ttu-id="85c0e-139"><xref:Microsoft.Extensions.Hosting.IHostBuilder.ConfigureServices%2A>Une <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection> , qui est une liste d’objets [descripteur de service](xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor) , est passée à la méthode.</span><span class="sxs-lookup"><span data-stu-id="85c0e-139">The <xref:Microsoft.Extensions.Hosting.IHostBuilder.ConfigureServices%2A> method is passed an <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, which is a list of [service descriptor](xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor) objects.</span></span> <span data-ttu-id="85c0e-140">Les services sont ajoutés dans la `ConfigureServices` méthode en fournissant des descripteurs de service à la collection de services.</span><span class="sxs-lookup"><span data-stu-id="85c0e-140">Services are added in the `ConfigureServices` method by providing service descriptors to the service collection.</span></span> <span data-ttu-id="85c0e-141">L’exemple suivant illustre le concept avec l' `IDataAccess` interface et son implémentation concrète `DataAccess` :</span><span class="sxs-lookup"><span data-stu-id="85c0e-141">The following example demonstrates the concept with the `IDataAccess` interface and its concrete implementation `DataAccess`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

::: zone-end

### <a name="service-lifetime"></a><span data-ttu-id="85c0e-142">Durée de vie du service</span><span class="sxs-lookup"><span data-stu-id="85c0e-142">Service lifetime</span></span>

<span data-ttu-id="85c0e-143">Les services peuvent être configurés avec les durées de vie indiquées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="85c0e-143">Services can be configured with the lifetimes shown in the following table.</span></span>

| <span data-ttu-id="85c0e-144">Durée de vie</span><span class="sxs-lookup"><span data-stu-id="85c0e-144">Lifetime</span></span> | <span data-ttu-id="85c0e-145">Description</span><span class="sxs-lookup"><span data-stu-id="85c0e-145">Description</span></span> |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped%2A> | <p><span data-ttu-id="85c0e-146">Blazor WebAssembly les applications n’ont pas actuellement de concept d’étendues DI.</span><span class="sxs-lookup"><span data-stu-id="85c0e-146">Blazor WebAssembly apps don't currently have a concept of DI scopes.</span></span> <span data-ttu-id="85c0e-147">`Scoped`-les services inscrits se comportent comme des `Singleton` services.</span><span class="sxs-lookup"><span data-stu-id="85c0e-147">`Scoped`-registered services behave like `Singleton` services.</span></span></p><p><span data-ttu-id="85c0e-148">Le Blazor Server modèle d’hébergement prend en charge la `Scoped` durée de vie des requêtes http, mais pas entre les messages de connexion/circuit SingalR parmi les composants chargés sur le client.</span><span class="sxs-lookup"><span data-stu-id="85c0e-148">The Blazor Server hosting model supports the `Scoped` lifetime across HTTP requests but not across SingalR connection/circuit messages among components that are loaded on the client.</span></span> <span data-ttu-id="85c0e-149">La Razor partie pages ou MVC de l’application traite normalement les services délimités et recrée les services sur *chaque requête http* lors de la navigation entre les pages ou les vues, ou à partir d’une page ou d’une vue dans un composant.</span><span class="sxs-lookup"><span data-stu-id="85c0e-149">The Razor Pages or MVC portion of the app treats scoped services normally and recreates the services on *each HTTP request* when navigating among pages or views or from a page or view to a component.</span></span> <span data-ttu-id="85c0e-150">Les services délimités ne sont pas reconstruits lors de la navigation entre les composants du client, où la communication avec le serveur a lieu via la SignalR connexion du circuit de l’utilisateur, et non par le biais de requêtes http.</span><span class="sxs-lookup"><span data-stu-id="85c0e-150">Scoped services aren't reconstructed when navigating among components on the client, where the communication to the server takes place over the SignalR connection of the user's circuit, not via HTTP requests.</span></span> <span data-ttu-id="85c0e-151">Dans les scénarios de composants suivants sur le client, les services délimités sont reconstruits car un nouveau circuit est créé pour l’utilisateur :</span><span class="sxs-lookup"><span data-stu-id="85c0e-151">In the following component scenarios on the client, scoped services are reconstructed because a new circuit is created for the user:</span></span></p><ul><li><span data-ttu-id="85c0e-152">L’utilisateur ferme la fenêtre du navigateur.</span><span class="sxs-lookup"><span data-stu-id="85c0e-152">The user closes the browser's window.</span></span> <span data-ttu-id="85c0e-153">L’utilisateur ouvre une nouvelle fenêtre et revient à l’application.</span><span class="sxs-lookup"><span data-stu-id="85c0e-153">The user opens a new window and navigates back to the app.</span></span></li><li><span data-ttu-id="85c0e-154">L’utilisateur ferme le dernier onglet de l’application dans une fenêtre de navigateur.</span><span class="sxs-lookup"><span data-stu-id="85c0e-154">The user closes the last tab of the app in a browser window.</span></span> <span data-ttu-id="85c0e-155">L’utilisateur ouvre un nouvel onglet et revient à l’application.</span><span class="sxs-lookup"><span data-stu-id="85c0e-155">The user opens a new tab and navigates back to the app.</span></span></li><li><span data-ttu-id="85c0e-156">L’utilisateur sélectionne le bouton de rechargement/actualisation du navigateur.</span><span class="sxs-lookup"><span data-stu-id="85c0e-156">The user selects the browser's reload/refresh button.</span></span></li></ul><p><span data-ttu-id="85c0e-157">Pour plus d’informations sur la conservation de l’état utilisateur sur les services étendus dans les Blazor Server applications, consultez <xref:blazor/hosting-models?pivots=server> .</span><span class="sxs-lookup"><span data-stu-id="85c0e-157">For more information on preserving user state across scoped services in Blazor Server apps, see <xref:blazor/hosting-models?pivots=server>.</span></span></p> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton%2A> | <span data-ttu-id="85c0e-158">DI crée une *seule instance* du service.</span><span class="sxs-lookup"><span data-stu-id="85c0e-158">DI creates a *single instance* of the service.</span></span> <span data-ttu-id="85c0e-159">Tous les composants qui requièrent un `Singleton` service reçoivent une instance du même service.</span><span class="sxs-lookup"><span data-stu-id="85c0e-159">All components requiring a `Singleton` service receive an instance of the same service.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient%2A> | <span data-ttu-id="85c0e-160">Chaque fois qu’un composant obtient une instance d’un `Transient` service à partir du conteneur de service, il reçoit une *nouvelle instance* du service.</span><span class="sxs-lookup"><span data-stu-id="85c0e-160">Whenever a component obtains an instance of a `Transient` service from the service container, it receives a *new instance* of the service.</span></span> |

<span data-ttu-id="85c0e-161">Le système DI est basé sur le système DI dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="85c0e-161">The DI system is based on the DI system in ASP.NET Core.</span></span> <span data-ttu-id="85c0e-162">Pour plus d’informations, consultez <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="85c0e-162">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="request-a-service-in-a-component"></a><span data-ttu-id="85c0e-163">Demander un service dans un composant</span><span class="sxs-lookup"><span data-stu-id="85c0e-163">Request a service in a component</span></span>

<span data-ttu-id="85c0e-164">Une fois les services ajoutés à la collection de services, injectez les services dans les composants à l’aide de la directive [ \@ Inject](xref:mvc/views/razor#inject) Razor .</span><span class="sxs-lookup"><span data-stu-id="85c0e-164">After services are added to the service collection, inject the services into the components using the [\@inject](xref:mvc/views/razor#inject) Razor directive.</span></span> <span data-ttu-id="85c0e-165">[`@inject`](xref:mvc/views/razor#inject) a deux paramètres :</span><span class="sxs-lookup"><span data-stu-id="85c0e-165">[`@inject`](xref:mvc/views/razor#inject) has two parameters:</span></span>

* <span data-ttu-id="85c0e-166">Type : type du service à injecter.</span><span class="sxs-lookup"><span data-stu-id="85c0e-166">Type: The type of the service to inject.</span></span>
* <span data-ttu-id="85c0e-167">Propriété : nom de la propriété qui reçoit le service d’application injecté.</span><span class="sxs-lookup"><span data-stu-id="85c0e-167">Property: The name of the property receiving the injected app service.</span></span> <span data-ttu-id="85c0e-168">La propriété ne nécessite pas de création manuelle.</span><span class="sxs-lookup"><span data-stu-id="85c0e-168">The property doesn't require manual creation.</span></span> <span data-ttu-id="85c0e-169">Le compilateur crée la propriété.</span><span class="sxs-lookup"><span data-stu-id="85c0e-169">The compiler creates the property.</span></span>

<span data-ttu-id="85c0e-170">Pour plus d’informations, consultez <xref:mvc/views/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="85c0e-170">For more information, see <xref:mvc/views/dependency-injection>.</span></span>

<span data-ttu-id="85c0e-171">Utilisez plusieurs [`@inject`](xref:mvc/views/razor#inject) instructions pour injecter différents services.</span><span class="sxs-lookup"><span data-stu-id="85c0e-171">Use multiple [`@inject`](xref:mvc/views/razor#inject) statements to inject different services.</span></span>

<span data-ttu-id="85c0e-172">L’exemple suivant montre comment utiliser [`@inject`](xref:mvc/views/razor#inject) .</span><span class="sxs-lookup"><span data-stu-id="85c0e-172">The following example shows how to use [`@inject`](xref:mvc/views/razor#inject).</span></span> <span data-ttu-id="85c0e-173">Le service qui implémente `Services.IDataAccess` est injecté dans la propriété du composant `DataRepository` .</span><span class="sxs-lookup"><span data-stu-id="85c0e-173">The service implementing `Services.IDataAccess` is injected into the component's property `DataRepository`.</span></span> <span data-ttu-id="85c0e-174">Notez la manière dont le code utilise uniquement l' `IDataAccess` abstraction :</span><span class="sxs-lookup"><span data-stu-id="85c0e-174">Note how the code is only using the `IDataAccess` abstraction:</span></span>

[!code-razor[](dependency-injection/samples_snapshot/CustomerList.razor?highlight=2-3,20)]

<span data-ttu-id="85c0e-175">En interne, la propriété générée ( `DataRepository` ) utilise l' [`[Inject]`](xref:Microsoft.AspNetCore.Components.InjectAttribute) attribut.</span><span class="sxs-lookup"><span data-stu-id="85c0e-175">Internally, the generated property (`DataRepository`) uses the [`[Inject]`](xref:Microsoft.AspNetCore.Components.InjectAttribute) attribute.</span></span> <span data-ttu-id="85c0e-176">En règle générale, cet attribut n’est pas utilisé directement.</span><span class="sxs-lookup"><span data-stu-id="85c0e-176">Typically, this attribute isn't used directly.</span></span> <span data-ttu-id="85c0e-177">Si une classe de base est requise pour les composants et les propriétés injectées sont également requises pour la classe de base, ajoutez manuellement l' [`[Inject]`](xref:Microsoft.AspNetCore.Components.InjectAttribute) attribut :</span><span class="sxs-lookup"><span data-stu-id="85c0e-177">If a base class is required for components and injected properties are also required for the base class, manually add the [`[Inject]`](xref:Microsoft.AspNetCore.Components.InjectAttribute) attribute:</span></span>

```csharp
using Microsoft.AspNetCore.Components;

public class ComponentBase : IComponent
{
    [Inject]
    protected IDataAccess DataRepository { get; set; }

    ...
}
```

<span data-ttu-id="85c0e-178">Dans les composants dérivés de la classe de base, la [`@inject`](xref:mvc/views/razor#inject) directive n’est pas obligatoire.</span><span class="sxs-lookup"><span data-stu-id="85c0e-178">In components derived from the base class, the [`@inject`](xref:mvc/views/razor#inject) directive isn't required.</span></span> <span data-ttu-id="85c0e-179">Le <xref:Microsoft.AspNetCore.Components.InjectAttribute> de la classe de base est suffisant :</span><span class="sxs-lookup"><span data-stu-id="85c0e-179">The <xref:Microsoft.AspNetCore.Components.InjectAttribute> of the base class is sufficient:</span></span>

```razor
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="use-di-in-services"></a><span data-ttu-id="85c0e-180">Utiliser DI dans les services</span><span class="sxs-lookup"><span data-stu-id="85c0e-180">Use DI in services</span></span>

<span data-ttu-id="85c0e-181">Les services complexes peuvent nécessiter des services supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="85c0e-181">Complex services might require additional services.</span></span> <span data-ttu-id="85c0e-182">Dans l’exemple suivant, `DataAccess` requiert le <xref:System.Net.Http.HttpClient> service par défaut.</span><span class="sxs-lookup"><span data-stu-id="85c0e-182">In the following example, `DataAccess` requires the <xref:System.Net.Http.HttpClient> default service.</span></span> <span data-ttu-id="85c0e-183">[`@inject`](xref:mvc/views/razor#inject) (ou l' [`[Inject]`](xref:Microsoft.AspNetCore.Components.InjectAttribute) attribut) n’est pas disponible pour une utilisation dans les services.</span><span class="sxs-lookup"><span data-stu-id="85c0e-183">[`@inject`](xref:mvc/views/razor#inject) (or the [`[Inject]`](xref:Microsoft.AspNetCore.Components.InjectAttribute) attribute) isn't available for use in services.</span></span> <span data-ttu-id="85c0e-184">L' *injection de constructeur* doit être utilisée à la place.</span><span class="sxs-lookup"><span data-stu-id="85c0e-184">*Constructor injection* must be used instead.</span></span> <span data-ttu-id="85c0e-185">Les services requis sont ajoutés en ajoutant des paramètres au constructeur du service.</span><span class="sxs-lookup"><span data-stu-id="85c0e-185">Required services are added by adding parameters to the service's constructor.</span></span> <span data-ttu-id="85c0e-186">Lorsque DI crée le service, il reconnaît les services dont il a besoin dans le constructeur et les fournit en conséquence.</span><span class="sxs-lookup"><span data-stu-id="85c0e-186">When DI creates the service, it recognizes the services it requires in the constructor and provides them accordingly.</span></span> <span data-ttu-id="85c0e-187">Dans l’exemple suivant, le constructeur reçoit un <xref:System.Net.Http.HttpClient> via di.</span><span class="sxs-lookup"><span data-stu-id="85c0e-187">In the following example, the constructor receives an <xref:System.Net.Http.HttpClient> via DI.</span></span> <span data-ttu-id="85c0e-188"><xref:System.Net.Http.HttpClient> est un service par défaut.</span><span class="sxs-lookup"><span data-stu-id="85c0e-188"><xref:System.Net.Http.HttpClient> is a default service.</span></span>

```csharp
using System.Net.Http;

public class DataAccess : IDataAccess
{
    public DataAccess(HttpClient http)
    {
        ...
    }
}
```

<span data-ttu-id="85c0e-189">Conditions préalables pour l’injection de constructeur :</span><span class="sxs-lookup"><span data-stu-id="85c0e-189">Prerequisites for constructor injection:</span></span>

* <span data-ttu-id="85c0e-190">Un constructeur doit exister dont les arguments peuvent tous être remplis par DI.</span><span class="sxs-lookup"><span data-stu-id="85c0e-190">One constructor must exist whose arguments can all be fulfilled by DI.</span></span> <span data-ttu-id="85c0e-191">Les paramètres supplémentaires non couverts par DI sont autorisés s’ils spécifient des valeurs par défaut.</span><span class="sxs-lookup"><span data-stu-id="85c0e-191">Additional parameters not covered by DI are allowed if they specify default values.</span></span>
* <span data-ttu-id="85c0e-192">Le constructeur applicable doit être `public` .</span><span class="sxs-lookup"><span data-stu-id="85c0e-192">The applicable constructor must be `public`.</span></span>
* <span data-ttu-id="85c0e-193">Un constructeur applicable doit exister.</span><span class="sxs-lookup"><span data-stu-id="85c0e-193">One applicable constructor must exist.</span></span> <span data-ttu-id="85c0e-194">En cas d’ambiguïté, DI lève une exception.</span><span class="sxs-lookup"><span data-stu-id="85c0e-194">In case of an ambiguity, DI throws an exception.</span></span>

## <a name="utility-base-component-classes-to-manage-a-di-scope"></a><span data-ttu-id="85c0e-195">Classes de composants de base de l’utilitaire pour gérer une étendue DI</span><span class="sxs-lookup"><span data-stu-id="85c0e-195">Utility base component classes to manage a DI scope</span></span>

<span data-ttu-id="85c0e-196">Dans ASP.NET Core applications, les services délimités sont généralement étendus à la requête actuelle.</span><span class="sxs-lookup"><span data-stu-id="85c0e-196">In ASP.NET Core apps, scoped services are typically scoped to the current request.</span></span> <span data-ttu-id="85c0e-197">Une fois la demande terminée, tous les services délimités ou temporaires sont supprimés par le système DI.</span><span class="sxs-lookup"><span data-stu-id="85c0e-197">After the request completes, any scoped or transient services are disposed by the DI system.</span></span> <span data-ttu-id="85c0e-198">Dans Blazor Server les applications, l’étendue de la demande est valable pendant la durée de la connexion cliente, ce qui peut entraîner des services transitoires et de portée bien plus longs que prévu.</span><span class="sxs-lookup"><span data-stu-id="85c0e-198">In Blazor Server apps, the request scope lasts for the duration of the client connection, which can result in transient and scoped services living much longer than expected.</span></span> <span data-ttu-id="85c0e-199">Dans les Blazor WebAssembly applications, les services inscrits avec une durée de vie limitée sont traités comme des singletons, de sorte qu’ils vivent plus longtemps que les services étendus dans les applications de ASP.net Core standard.</span><span class="sxs-lookup"><span data-stu-id="85c0e-199">In Blazor WebAssembly apps, services registered with a scoped lifetime are treated as singletons, so they live longer than scoped services in typical ASP.NET Core apps.</span></span>

> [!NOTE]
> <span data-ttu-id="85c0e-200">Pour détecter les services transitoires distants dans une application, consultez la section détecter les services temporaires à usage [temporaire](#detect-transient-disposables) .</span><span class="sxs-lookup"><span data-stu-id="85c0e-200">To detect disposable transient services in an app, see the [Detect transient disposables](#detect-transient-disposables) section.</span></span>

<span data-ttu-id="85c0e-201">Une approche qui limite la durée de vie d’un service dans Blazor les applications est l’utilisation du <xref:Microsoft.AspNetCore.Components.OwningComponentBase> type.</span><span class="sxs-lookup"><span data-stu-id="85c0e-201">An approach that limits a service lifetime in Blazor apps is use of the <xref:Microsoft.AspNetCore.Components.OwningComponentBase> type.</span></span> <span data-ttu-id="85c0e-202"><xref:Microsoft.AspNetCore.Components.OwningComponentBase> est un type abstrait dérivé de <xref:Microsoft.AspNetCore.Components.ComponentBase> qui crée une étendue di correspondant à la durée de vie du composant.</span><span class="sxs-lookup"><span data-stu-id="85c0e-202"><xref:Microsoft.AspNetCore.Components.OwningComponentBase> is an abstract type derived from <xref:Microsoft.AspNetCore.Components.ComponentBase> that creates a DI scope corresponding to the lifetime of the component.</span></span> <span data-ttu-id="85c0e-203">À l’aide de cette étendue, il est possible d’utiliser l’injection de services avec une durée de vie limitée et de les faire vivre aussi longtemps que le composant.</span><span class="sxs-lookup"><span data-stu-id="85c0e-203">Using this scope, it's possible to use DI services with a scoped lifetime and have them live as long as the component.</span></span> <span data-ttu-id="85c0e-204">Lorsque le composant est détruit, les services du fournisseur de services étendus du composant sont également supprimés.</span><span class="sxs-lookup"><span data-stu-id="85c0e-204">When the component is destroyed, services from the component's scoped service provider are disposed as well.</span></span> <span data-ttu-id="85c0e-205">Cela peut être utile pour les services qui :</span><span class="sxs-lookup"><span data-stu-id="85c0e-205">This can be useful for services that:</span></span>

* <span data-ttu-id="85c0e-206">Doit être réutilisé dans un composant, car la durée de vie temporaire n’est pas appropriée.</span><span class="sxs-lookup"><span data-stu-id="85c0e-206">Should be reused within a component, as the transient lifetime is inappropriate.</span></span>
* <span data-ttu-id="85c0e-207">Ne doit pas être partagé entre les composants, car la durée de vie Singleton n’est pas appropriée.</span><span class="sxs-lookup"><span data-stu-id="85c0e-207">Shouldn't be shared across components, as the singleton lifetime is inappropriate.</span></span>

<span data-ttu-id="85c0e-208">Deux versions du <xref:Microsoft.AspNetCore.Components.OwningComponentBase> type sont disponibles :</span><span class="sxs-lookup"><span data-stu-id="85c0e-208">Two versions of the <xref:Microsoft.AspNetCore.Components.OwningComponentBase> type are available:</span></span>

* <span data-ttu-id="85c0e-209"><xref:Microsoft.AspNetCore.Components.OwningComponentBase> est un enfant abstrait et jetable du <xref:Microsoft.AspNetCore.Components.ComponentBase> type avec une propriété protégée <xref:Microsoft.AspNetCore.Components.OwningComponentBase.ScopedServices> de type <xref:System.IServiceProvider> .</span><span class="sxs-lookup"><span data-stu-id="85c0e-209"><xref:Microsoft.AspNetCore.Components.OwningComponentBase> is an abstract, disposable child of the <xref:Microsoft.AspNetCore.Components.ComponentBase> type with a protected <xref:Microsoft.AspNetCore.Components.OwningComponentBase.ScopedServices> property of type <xref:System.IServiceProvider>.</span></span> <span data-ttu-id="85c0e-210">Ce fournisseur peut être utilisé pour résoudre les services dont la portée est limitée à la durée de vie du composant.</span><span class="sxs-lookup"><span data-stu-id="85c0e-210">This provider can be used to resolve services that are scoped to the lifetime of the component.</span></span>

  <span data-ttu-id="85c0e-211">Les services d’injection de services injectés dans le composant à l’aide de [`@inject`](xref:mvc/views/razor#inject) ou l' [`[Inject]`](xref:Microsoft.AspNetCore.Components.InjectAttribute) attribut ne sont pas créés dans l’étendue du composant.</span><span class="sxs-lookup"><span data-stu-id="85c0e-211">DI services injected into the component using [`@inject`](xref:mvc/views/razor#inject) or the [`[Inject]`](xref:Microsoft.AspNetCore.Components.InjectAttribute) attribute aren't created in the component's scope.</span></span> <span data-ttu-id="85c0e-212">Pour utiliser l’étendue du composant, les services doivent être résolus à l’aide <xref:Microsoft.Extensions.DependencyInjection.ServiceProviderServiceExtensions.GetRequiredService%2A> de ou de <xref:System.IServiceProvider.GetService%2A> .</span><span class="sxs-lookup"><span data-stu-id="85c0e-212">To use the component's scope, services must be resolved using <xref:Microsoft.Extensions.DependencyInjection.ServiceProviderServiceExtensions.GetRequiredService%2A> or <xref:System.IServiceProvider.GetService%2A>.</span></span> <span data-ttu-id="85c0e-213">Les dépendances de tous les services résolus à l’aide du <xref:Microsoft.AspNetCore.Components.OwningComponentBase.ScopedServices> fournisseur sont fournies à partir de cette même étendue.</span><span class="sxs-lookup"><span data-stu-id="85c0e-213">Any services resolved using the <xref:Microsoft.AspNetCore.Components.OwningComponentBase.ScopedServices> provider have their dependencies provided from that same scope.</span></span>

  [!code-razor[](dependency-injection/samples_snapshot/Preferences.razor?highlight=3,20-21)]

* <span data-ttu-id="85c0e-214"><xref:Microsoft.AspNetCore.Components.OwningComponentBase%601> dérive de <xref:Microsoft.AspNetCore.Components.OwningComponentBase> et ajoute une <xref:Microsoft.AspNetCore.Components.OwningComponentBase%601.Service%2A> propriété qui retourne une instance de `T` à partir du fournisseur di étendu.</span><span class="sxs-lookup"><span data-stu-id="85c0e-214"><xref:Microsoft.AspNetCore.Components.OwningComponentBase%601> derives from <xref:Microsoft.AspNetCore.Components.OwningComponentBase> and adds a <xref:Microsoft.AspNetCore.Components.OwningComponentBase%601.Service%2A> property that returns an instance of `T` from the scoped DI provider.</span></span> <span data-ttu-id="85c0e-215">Ce type est un moyen pratique d’accéder aux services délimités sans utiliser une instance de <xref:System.IServiceProvider> lorsqu’un service principal est requis par l’application à partir du conteneur di à l’aide de l’étendue du composant.</span><span class="sxs-lookup"><span data-stu-id="85c0e-215">This type is a convenient way to access scoped services without using an instance of <xref:System.IServiceProvider> when there's one primary service the app requires from the DI container using the component's scope.</span></span> <span data-ttu-id="85c0e-216">La <xref:Microsoft.AspNetCore.Components.OwningComponentBase.ScopedServices> propriété est disponible. par conséquent, l’application peut récupérer des services d’autres types, si nécessaire.</span><span class="sxs-lookup"><span data-stu-id="85c0e-216">The <xref:Microsoft.AspNetCore.Components.OwningComponentBase.ScopedServices> property is available, so the app can get services of other types, if necessary.</span></span>

  [!code-razor[](dependency-injection/samples_snapshot/Users.razor?highlight=3,5,8)]

## <a name="use-of-an-entity-framework-core-ef-core-dbcontext-from-di"></a><span data-ttu-id="85c0e-217">Utilisation d’un DbContext Entity Framework Core (EF Core) à partir de DI</span><span class="sxs-lookup"><span data-stu-id="85c0e-217">Use of an Entity Framework Core (EF Core) DbContext from DI</span></span>

<span data-ttu-id="85c0e-218">Pour plus d’informations, consultez <xref:blazor/blazor-server-ef-core>.</span><span class="sxs-lookup"><span data-stu-id="85c0e-218">For more information, see <xref:blazor/blazor-server-ef-core>.</span></span>

::: moniker range="< aspnetcore-5.0"

## <a name="detect-transient-disposables"></a><span data-ttu-id="85c0e-219">Détecter les supprimables temporaires</span><span class="sxs-lookup"><span data-stu-id="85c0e-219">Detect transient disposables</span></span>

<span data-ttu-id="85c0e-220">Les exemples suivants montrent comment détecter des services transitoires jetables dans une application qui doit utiliser <xref:Microsoft.AspNetCore.Components.OwningComponentBase> .</span><span class="sxs-lookup"><span data-stu-id="85c0e-220">The following examples show how to detect disposable transient services in an app that should use <xref:Microsoft.AspNetCore.Components.OwningComponentBase>.</span></span> <span data-ttu-id="85c0e-221">Pour plus d’informations, consultez la section [classes du composant de base de l’utilitaire pour gérer une étendue de l’injection de](#utility-base-component-classes-to-manage-a-di-scope) données.</span><span class="sxs-lookup"><span data-stu-id="85c0e-221">For more information, see the [Utility base component classes to manage a DI scope](#utility-base-component-classes-to-manage-a-di-scope) section.</span></span>

::: zone pivot="webassembly"

<span data-ttu-id="85c0e-222">`DetectIncorrectUsagesOfTransientDisposables.cs`:</span><span class="sxs-lookup"><span data-stu-id="85c0e-222">`DetectIncorrectUsagesOfTransientDisposables.cs`:</span></span>

[!code-csharp[](dependency-injection/samples_snapshot/3.x/transient-disposables/DetectIncorrectUsagesOfTransientDisposables-wasm.cs)]

<span data-ttu-id="85c0e-223">Le `TransientDisposable` dans l’exemple suivant est détecté ( `Program.cs` ) :</span><span class="sxs-lookup"><span data-stu-id="85c0e-223">The `TransientDisposable` in the following example is detected (`Program.cs`):</span></span>

<!-- moniker range=">= aspnetcore-5.0"

[!code-csharp[](dependency-injection/samples_snapshot/5.x/transient-disposables/DetectIncorrectUsagesOfTransientDisposables-wasm-program.cs?highlight=6,9,17,22-25)]

moniker-end 

moniker range="< aspnetcore-5.0" -->

[!code-csharp[](dependency-injection/samples_snapshot/3.x/transient-disposables/DetectIncorrectUsagesOfTransientDisposables-wasm-program.cs?highlight=6,9,17,22-25)]

<!-- moniker-end -->

::: zone-end

::: zone pivot="server"

<span data-ttu-id="85c0e-224">`DetectIncorrectUsagesOfTransientDisposables.cs`:</span><span class="sxs-lookup"><span data-stu-id="85c0e-224">`DetectIncorrectUsagesOfTransientDisposables.cs`:</span></span>

[!code-csharp[](dependency-injection/samples_snapshot/3.x/transient-disposables/DetectIncorrectUsagesOfTransientDisposables-server.cs)]

<span data-ttu-id="85c0e-225">Ajoutez l’espace de noms pour <xref:Microsoft.Extensions.DependencyInjection?displayProperty=fullName> à `Program.cs` :</span><span class="sxs-lookup"><span data-stu-id="85c0e-225">Add the namespace for <xref:Microsoft.Extensions.DependencyInjection?displayProperty=fullName> to `Program.cs`:</span></span>

```csharp
using Microsoft.Extensions.DependencyInjection;
```

<span data-ttu-id="85c0e-226">Dans `Program.CreateHostBuilder` de `Program.cs` :</span><span class="sxs-lookup"><span data-stu-id="85c0e-226">In `Program.CreateHostBuilder` of `Program.cs`:</span></span>

[!code-csharp[](dependency-injection/samples_snapshot/3.x/transient-disposables/DetectIncorrectUsagesOfTransientDisposables-server-program.cs?highlight=3)]

<span data-ttu-id="85c0e-227">Le `TransientDependency` dans l’exemple suivant est détecté ( `Startup.cs` ) :</span><span class="sxs-lookup"><span data-stu-id="85c0e-227">The `TransientDependency` in the following example is detected (`Startup.cs`):</span></span>

[!code-csharp[](dependency-injection/samples_snapshot/3.x/transient-disposables/DetectIncorrectUsagesOfTransientDisposables-server-startup.cs?highlight=6-8,11-32)]

::: zone-end

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="85c0e-228">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="85c0e-228">Additional resources</span></span>

* <xref:fundamentals/dependency-injection>
* [<span data-ttu-id="85c0e-229">`IDisposable` conseils pour les instances temporaires et partagées</span><span class="sxs-lookup"><span data-stu-id="85c0e-229">`IDisposable` guidance for Transient and shared instances</span></span>](xref:fundamentals/dependency-injection#idisposable-guidance-for-transient-and-shared-instances)
* <xref:mvc/views/dependency-injection>
