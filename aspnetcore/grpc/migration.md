---
title: Migration des services gRPC de C Core vers ASP.NET Core
author: juntaoluo
description: Découvrez comment déplacer une application gRPC basée sur un noyau C existante pour qu’elle s’exécute sur ASP.NET Core pile.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 09/25/2019
no-loc:
- 'appsettings.json'
- 'ASP.NET Core Identity'
- 'cookie'
- 'Cookie'
- 'Blazor'
- 'Blazor Server'
- 'Blazor WebAssembly'
- 'Identity'
- "Let's Encrypt"
- 'Razor'
- 'SignalR'
uid: grpc/migration
ms.openlocfilehash: 1a230e470fa666b2aa6761b4d5dabd09264d2aae
ms.sourcegitcommit: ca34c1ac578e7d3daa0febf1810ba5fc74f60bbf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93059830"
---
# <a name="migrating-grpc-services-from-c-core-to-aspnet-core"></a><span data-ttu-id="d0042-103">Migration des services gRPC de C Core vers ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d0042-103">Migrating gRPC services from C-core to ASP.NET Core</span></span>

<span data-ttu-id="d0042-104">Par [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="d0042-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="d0042-105">En raison de l’implémentation de la pile sous-jacente, certaines fonctionnalités ne fonctionnent pas de la même façon entre les applications [gRPC basées sur le langage C](https://grpc.io/blog/grpc-stacks) et les applications basées sur les ASP.net core.</span><span class="sxs-lookup"><span data-stu-id="d0042-105">Due to the implementation of the underlying stack, not all features work in the same way between [C-core-based gRPC](https://grpc.io/blog/grpc-stacks) apps and ASP.NET Core-based apps.</span></span> <span data-ttu-id="d0042-106">Ce document met en évidence les principales différences en matière de migration entre les deux piles.</span><span class="sxs-lookup"><span data-stu-id="d0042-106">This document highlights the key differences for migrating between the two stacks.</span></span>

## <a name="grpc-service-implementation-lifetime"></a><span data-ttu-id="d0042-107">durée de vie de l’implémentation du service gRPC</span><span class="sxs-lookup"><span data-stu-id="d0042-107">gRPC service implementation lifetime</span></span>

<span data-ttu-id="d0042-108">Dans la pile ASP.NET Core, les services gRPC, par défaut, sont créés avec une [durée de vie limitée](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="d0042-108">In the ASP.NET Core stack, gRPC services, by default, are created with a [scoped lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="d0042-109">En revanche, gRPC C-Core est lié par défaut à un service avec une [durée de vie Singleton](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="d0042-109">In contrast, gRPC C-core by default binds to a service with a [singleton lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

<span data-ttu-id="d0042-110">Une durée de vie limitée permet à l’implémentation de service de résoudre d’autres services avec des durées de vie délimitées.</span><span class="sxs-lookup"><span data-stu-id="d0042-110">A scoped lifetime allows the service implementation to resolve other services with scoped lifetimes.</span></span> <span data-ttu-id="d0042-111">Par exemple, une durée de vie délimitée peut également être résolue `DbContext` à partir du conteneur di via l’injection de constructeur.</span><span class="sxs-lookup"><span data-stu-id="d0042-111">For example, a scoped lifetime can also resolve `DbContext` from the DI container through constructor injection.</span></span> <span data-ttu-id="d0042-112">Utilisation de la durée de vie limitée :</span><span class="sxs-lookup"><span data-stu-id="d0042-112">Using scoped lifetime:</span></span>

* <span data-ttu-id="d0042-113">Une nouvelle instance de l’implémentation de service est construite pour chaque requête.</span><span class="sxs-lookup"><span data-stu-id="d0042-113">A new instance of the service implementation is constructed for each request.</span></span>
* <span data-ttu-id="d0042-114">Il n’est pas possible de partager l’état entre les demandes via des membres d’instance sur le type d’implémentation.</span><span class="sxs-lookup"><span data-stu-id="d0042-114">It isn't possible to share state between requests via instance members on the implementation type.</span></span>
* <span data-ttu-id="d0042-115">L’objectif est de stocker les États partagés dans un service Singleton dans le conteneur DI.</span><span class="sxs-lookup"><span data-stu-id="d0042-115">The expectation is to store shared states in a singleton service in the DI container.</span></span> <span data-ttu-id="d0042-116">Les États partagés stockés sont résolus dans le constructeur de l’implémentation du service gRPC.</span><span class="sxs-lookup"><span data-stu-id="d0042-116">The stored shared states are resolved in the constructor of the gRPC service implementation.</span></span>

<span data-ttu-id="d0042-117">Pour plus d’informations sur les durées de vie des services, consultez <xref:fundamentals/dependency-injection#service-lifetimes> .</span><span class="sxs-lookup"><span data-stu-id="d0042-117">For more information on service lifetimes, see <xref:fundamentals/dependency-injection#service-lifetimes>.</span></span>

### <a name="add-a-singleton-service"></a><span data-ttu-id="d0042-118">Ajouter un service Singleton</span><span class="sxs-lookup"><span data-stu-id="d0042-118">Add a singleton service</span></span>

<span data-ttu-id="d0042-119">Pour faciliter la transition d’une implémentation gRPC C-Core à ASP.NET Core, il est possible de modifier la durée de vie du service de l’implémentation de service de l’étendue à Singleton.</span><span class="sxs-lookup"><span data-stu-id="d0042-119">To facilitate the transition from a gRPC C-core implementation to ASP.NET Core, it's possible to change the service lifetime of the service implementation from scoped to singleton.</span></span> <span data-ttu-id="d0042-120">Cela implique l’ajout d’une instance de l’implémentation de service au conteneur DI :</span><span class="sxs-lookup"><span data-stu-id="d0042-120">This involves adding an instance of the service implementation to the DI container:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton(new GreeterService());
}
```

<span data-ttu-id="d0042-121">Toutefois, une implémentation de service avec une durée de vie Singleton n’est plus en mesure de résoudre les services délimités via l’injection de constructeur.</span><span class="sxs-lookup"><span data-stu-id="d0042-121">However, a service implementation with a singleton lifetime is no longer able to resolve scoped services through constructor injection.</span></span>

## <a name="configure-grpc-services-options"></a><span data-ttu-id="d0042-122">Configurer les options des services gRPC</span><span class="sxs-lookup"><span data-stu-id="d0042-122">Configure gRPC services options</span></span>

<span data-ttu-id="d0042-123">Dans les applications basées sur C-Core, les paramètres tels que `grpc.max_receive_message_length` et `grpc.max_send_message_length` sont configurés avec `ChannelOption` lors de [la construction de l’instance de serveur](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).</span><span class="sxs-lookup"><span data-stu-id="d0042-123">In C-core-based apps, settings such as `grpc.max_receive_message_length` and `grpc.max_send_message_length` are configured with `ChannelOption` when [constructing the Server instance](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).</span></span>

<span data-ttu-id="d0042-124">Dans ASP.NET Core, gRPC fournit la configuration par le biais du `GrpcServiceOptions` type.</span><span class="sxs-lookup"><span data-stu-id="d0042-124">In ASP.NET Core, gRPC provides configuration through the `GrpcServiceOptions` type.</span></span> <span data-ttu-id="d0042-125">Par exemple, la taille maximale des messages entrants peut être configurée par le biais du service gRPC `AddGrpc` .</span><span class="sxs-lookup"><span data-stu-id="d0042-125">For example, a gRPC service's the maximum incoming message size can be configured via `AddGrpc`.</span></span> <span data-ttu-id="d0042-126">L’exemple suivant modifie la valeur par défaut `MaxReceiveMessageSize` de 4 Mo à 16 Mo :</span><span class="sxs-lookup"><span data-stu-id="d0042-126">The following example changes the default `MaxReceiveMessageSize` of 4 MB to 16 MB:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.MaxReceiveMessageSize = 16 * 1024 * 1024; // 16 MB
    });
}
```

<span data-ttu-id="d0042-127">Pour plus d’informations sur la configuration, consultez <xref:grpc/configuration> .</span><span class="sxs-lookup"><span data-stu-id="d0042-127">For more information on configuration, see <xref:grpc/configuration>.</span></span>

## <a name="logging"></a><span data-ttu-id="d0042-128">Journalisation</span><span class="sxs-lookup"><span data-stu-id="d0042-128">Logging</span></span>

<span data-ttu-id="d0042-129">Les applications basées sur le langage C s’appuient sur le `GrpcEnvironment` pour [configurer l’enregistreur](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) d’événements à des fins de débogage.</span><span class="sxs-lookup"><span data-stu-id="d0042-129">C-core-based apps rely on the `GrpcEnvironment` to [configure the logger](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) for debugging purposes.</span></span> <span data-ttu-id="d0042-130">La pile ASP.NET Core fournit cette fonctionnalité par le biais de l' [API de journalisation](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="d0042-130">The ASP.NET Core stack provides this functionality through the [Logging API](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="d0042-131">Par exemple, un enregistreur d’événements peut être ajouté au service gRPC via l’injection de constructeur :</span><span class="sxs-lookup"><span data-stu-id="d0042-131">For example, a logger can be added to the gRPC service via constructor injection:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

## <a name="https"></a><span data-ttu-id="d0042-132">HTTPS</span><span class="sxs-lookup"><span data-stu-id="d0042-132">HTTPS</span></span>

<span data-ttu-id="d0042-133">Les applications basées sur le langage C configurent le protocole HTTPs via la [propriété Server. ports](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports).</span><span class="sxs-lookup"><span data-stu-id="d0042-133">C-core-based apps configure HTTPS through the [Server.Ports property](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports).</span></span> <span data-ttu-id="d0042-134">Un concept similaire est utilisé pour configurer des serveurs dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d0042-134">A similar concept is used to configure servers in ASP.NET Core.</span></span> <span data-ttu-id="d0042-135">Par exemple, Kestrel utilise la [configuration de point de terminaison](xref:fundamentals/servers/kestrel#endpoint-configuration) pour cette fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="d0042-135">For example, Kestrel uses [endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) for this functionality.</span></span>

## <a name="grpc-interceptors-vs-middleware"></a><span data-ttu-id="d0042-136">intercepteurs gRPC vs intergiciel</span><span class="sxs-lookup"><span data-stu-id="d0042-136">gRPC Interceptors vs Middleware</span></span>

<span data-ttu-id="d0042-137">ASP.NET Core [intergiciel](xref:fundamentals/middleware/index) offre des fonctionnalités similaires par rapport aux intercepteurs des applications gRPC basées sur le langage C.</span><span class="sxs-lookup"><span data-stu-id="d0042-137">ASP.NET Core [middleware](xref:fundamentals/middleware/index) offers similar functionalities compared to interceptors in C-core-based gRPC apps.</span></span> <span data-ttu-id="d0042-138">ASP.NET Core l’intergiciel et les intercepteurs sont similaires d’un plan conceptuel.</span><span class="sxs-lookup"><span data-stu-id="d0042-138">ASP.NET Core middleware and interceptors are conceptually similar.</span></span> <span data-ttu-id="d0042-139">Les deux :</span><span class="sxs-lookup"><span data-stu-id="d0042-139">Both:</span></span>

* <span data-ttu-id="d0042-140">Sont utilisés pour construire un pipeline qui gère une demande gRPC.</span><span class="sxs-lookup"><span data-stu-id="d0042-140">Are used to construct a pipeline that handles a gRPC request.</span></span>
* <span data-ttu-id="d0042-141">Autorisez l’exécution du travail avant ou après le composant suivant dans le pipeline.</span><span class="sxs-lookup"><span data-stu-id="d0042-141">Allow work to be performed before or after the next component in the pipeline.</span></span>
* <span data-ttu-id="d0042-142">Fournir un accès à `HttpContext` :</span><span class="sxs-lookup"><span data-stu-id="d0042-142">Provide access to `HttpContext`:</span></span>
  * <span data-ttu-id="d0042-143">Dans l’intergiciel (middleware), `HttpContext` est un paramètre.</span><span class="sxs-lookup"><span data-stu-id="d0042-143">In middleware the `HttpContext` is a parameter.</span></span>
  * <span data-ttu-id="d0042-144">Dans les intercepteurs, le `HttpContext` est accessible à l’aide du `ServerCallContext` paramètre avec la `ServerCallContext.GetHttpContext` méthode d’extension.</span><span class="sxs-lookup"><span data-stu-id="d0042-144">In interceptors the `HttpContext` can be accessed using the `ServerCallContext` parameter with the `ServerCallContext.GetHttpContext` extension method.</span></span> <span data-ttu-id="d0042-145">Notez que cette fonctionnalité est spécifique aux intercepteurs s’exécutant dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d0042-145">Note that this feature is specific to interceptors running in ASP.NET Core.</span></span>

<span data-ttu-id="d0042-146">différences entre l’intercepteur gRPC et l’intergiciel (middleware) ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="d0042-146">gRPC Interceptor differences from ASP.NET Core Middleware:</span></span>

* <span data-ttu-id="d0042-147">Intercepteurs</span><span class="sxs-lookup"><span data-stu-id="d0042-147">Interceptors:</span></span>
  * <span data-ttu-id="d0042-148">Opérer sur la couche d’abstraction gRPC à l’aide de [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).</span><span class="sxs-lookup"><span data-stu-id="d0042-148">Operate on the gRPC layer of abstraction using the [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).</span></span>
  * <span data-ttu-id="d0042-149">Fournir un accès à :</span><span class="sxs-lookup"><span data-stu-id="d0042-149">Provide access to:</span></span>
    * <span data-ttu-id="d0042-150">Message désérialisé envoyé à un appel.</span><span class="sxs-lookup"><span data-stu-id="d0042-150">The deserialized message sent to a call.</span></span>
    * <span data-ttu-id="d0042-151">Message retourné par l’appel avant qu’il ne soit sérialisé.</span><span class="sxs-lookup"><span data-stu-id="d0042-151">The message being returned from the call before it is serialized.</span></span>
  * <span data-ttu-id="d0042-152">Peut intercepter et gérer les exceptions levées à partir des services gRPC.</span><span class="sxs-lookup"><span data-stu-id="d0042-152">Can catch and handle exceptions thrown from gRPC services.</span></span>
* <span data-ttu-id="d0042-153">Intergiciel</span><span class="sxs-lookup"><span data-stu-id="d0042-153">Middleware:</span></span>
  * <span data-ttu-id="d0042-154">S’exécute avant les intercepteurs gRPC.</span><span class="sxs-lookup"><span data-stu-id="d0042-154">Runs before gRPC interceptors.</span></span>
  * <span data-ttu-id="d0042-155">Opère sur les messages HTTP/2 sous-jacents.</span><span class="sxs-lookup"><span data-stu-id="d0042-155">Operates on the underlying HTTP/2 messages.</span></span>
  * <span data-ttu-id="d0042-156">Peut uniquement accéder aux octets des flux de requête et de réponse.</span><span class="sxs-lookup"><span data-stu-id="d0042-156">Can only access bytes from the request and response streams.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d0042-157">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="d0042-157">Additional resources</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:tutorials/grpc/grpc-start>
