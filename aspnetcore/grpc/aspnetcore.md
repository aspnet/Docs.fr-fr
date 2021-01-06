---
title: Services gRPC avec ASP.NET Core
author: juntaoluo
description: Découvrez les concepts de base liés à l’écriture de services gRPC avec ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 09/03/2019
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
uid: grpc/aspnetcore
ms.openlocfilehash: b120aa4ab6922445f2c53f3b1cb3bd5c159d8a84
ms.sourcegitcommit: 3593c4efa707edeaaceffbfa544f99f41fc62535
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/04/2021
ms.locfileid: "93057828"
---
# <a name="grpc-services-with-aspnet-core"></a><span data-ttu-id="04482-103">Services gRPC avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="04482-103">gRPC services with ASP.NET Core</span></span>

<span data-ttu-id="04482-104">Ce document montre comment prendre en main les services gRPC à l’aide de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="04482-104">This document shows how to get started with gRPC services using ASP.NET Core.</span></span>

[!INCLUDE[](~/includes/gRPCazure.md)]

## <a name="prerequisites"></a><span data-ttu-id="04482-105">Prérequis</span><span class="sxs-lookup"><span data-stu-id="04482-105">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="04482-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="04482-106">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="04482-107">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="04482-107">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="04482-108">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="04482-108">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="04482-109">Bien démarrer avec le service gRPC dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="04482-109">Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="04482-110">[Affichez ou téléchargez un exemple de code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="04482-110">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="04482-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="04482-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="04482-112">Pour obtenir des instructions détaillées sur la création d’un projet gRPC, consultez [prise en main des services gRPC](xref:tutorials/grpc/grpc-start) .</span><span class="sxs-lookup"><span data-stu-id="04482-112">See [Get started with gRPC services](xref:tutorials/grpc/grpc-start) for detailed instructions on how to create a gRPC project.</span></span>

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="04482-113">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="04482-113">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="04482-114">Exécutez `dotnet new grpc -o GrpcGreeter` à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="04482-114">Run `dotnet new grpc -o GrpcGreeter` from the command line.</span></span>

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a><span data-ttu-id="04482-115">Ajouter des services gRPC à une application ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="04482-115">Add gRPC services to an ASP.NET Core app</span></span>

<span data-ttu-id="04482-116">gRPC requiert le package [gRPC. AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) .</span><span class="sxs-lookup"><span data-stu-id="04482-116">gRPC requires the [Grpc.AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) package.</span></span>

### <a name="configure-grpc"></a><span data-ttu-id="04482-117">Configurer gRPC</span><span class="sxs-lookup"><span data-stu-id="04482-117">Configure gRPC</span></span>

<span data-ttu-id="04482-118">Dans *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="04482-118">In *Startup.cs*:</span></span>

* <span data-ttu-id="04482-119">gRPC est activé avec la `AddGrpc` méthode.</span><span class="sxs-lookup"><span data-stu-id="04482-119">gRPC is enabled with the `AddGrpc` method.</span></span>
* <span data-ttu-id="04482-120">Chaque service gRPC est ajouté au pipeline de routage via la `MapGrpcService` méthode.</span><span class="sxs-lookup"><span data-stu-id="04482-120">Each gRPC service is added to the routing pipeline through the `MapGrpcService` method.</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7,24)]
[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

<span data-ttu-id="04482-121">ASP.NET Core intergiciels et les fonctionnalités partagent le pipeline de routage, une application peut donc être configurée pour servir des gestionnaires de demandes supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="04482-121">ASP.NET Core middlewares and features share the routing pipeline, therefore an app can be configured to serve additional request handlers.</span></span> <span data-ttu-id="04482-122">Les gestionnaires de demandes supplémentaires, tels que les contrôleurs MVC, fonctionnent en parallèle avec les services gRPC configurés.</span><span class="sxs-lookup"><span data-stu-id="04482-122">The additional request handlers, such as MVC controllers, work in parallel with the configured gRPC services.</span></span>

### <a name="configure-kestrel"></a><span data-ttu-id="04482-123">Configurer Kestrel</span><span class="sxs-lookup"><span data-stu-id="04482-123">Configure Kestrel</span></span>

<span data-ttu-id="04482-124">Points de terminaison Kestrel gRPC :</span><span class="sxs-lookup"><span data-stu-id="04482-124">Kestrel gRPC endpoints:</span></span>

* <span data-ttu-id="04482-125">Nécessite HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="04482-125">Require HTTP/2.</span></span>
* <span data-ttu-id="04482-126">Doit être sécurisé avec [TLS (Transport Layer Security)](https://tools.ietf.org/html/rfc5246).</span><span class="sxs-lookup"><span data-stu-id="04482-126">Should be secured with [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span></span>

#### <a name="http2"></a><span data-ttu-id="04482-127">HTTP/2</span><span class="sxs-lookup"><span data-stu-id="04482-127">HTTP/2</span></span>

<span data-ttu-id="04482-128">gRPC requiert HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="04482-128">gRPC requires HTTP/2.</span></span> <span data-ttu-id="04482-129">gRPC pour ASP.NET Core valide [HttpRequest. Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) est `HTTP/2` .</span><span class="sxs-lookup"><span data-stu-id="04482-129">gRPC for ASP.NET Core validates [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) is `HTTP/2`.</span></span>

<span data-ttu-id="04482-130">Kestrel [prend en charge http/2](xref:fundamentals/servers/kestrel#http2-support) sur la plupart des systèmes d’exploitation modernes.</span><span class="sxs-lookup"><span data-stu-id="04482-130">Kestrel [supports HTTP/2](xref:fundamentals/servers/kestrel#http2-support) on most modern operating systems.</span></span> <span data-ttu-id="04482-131">Les points de terminaison Kestrel sont configurés pour prendre en charge les connexions HTTP/1.1 et HTTP/2 par défaut.</span><span class="sxs-lookup"><span data-stu-id="04482-131">Kestrel endpoints are configured to support HTTP/1.1 and HTTP/2 connections by default.</span></span>

#### <a name="tls"></a><span data-ttu-id="04482-132">TLS</span><span class="sxs-lookup"><span data-stu-id="04482-132">TLS</span></span>

<span data-ttu-id="04482-133">Les points de terminaison Kestrel utilisés pour gRPC doivent être sécurisés avec TLS.</span><span class="sxs-lookup"><span data-stu-id="04482-133">Kestrel endpoints used for gRPC should be secured with TLS.</span></span> <span data-ttu-id="04482-134">En cours de développement, un point de terminaison sécurisé avec TLS est automatiquement créé `https://localhost:5001` lorsque le certificat de développement ASP.net Core est présent.</span><span class="sxs-lookup"><span data-stu-id="04482-134">In development, an endpoint secured with TLS is automatically created at `https://localhost:5001` when the ASP.NET Core development certificate is present.</span></span> <span data-ttu-id="04482-135">Aucune configuration n'est requise.</span><span class="sxs-lookup"><span data-stu-id="04482-135">No configuration is required.</span></span> <span data-ttu-id="04482-136">Un `https` préfixe vérifie que le point de terminaison Kestrel utilise TLS.</span><span class="sxs-lookup"><span data-stu-id="04482-136">An `https` prefix verifies the Kestrel endpoint is using TLS.</span></span>

<span data-ttu-id="04482-137">En production, TLS doit être configuré de manière explicite.</span><span class="sxs-lookup"><span data-stu-id="04482-137">In production, TLS must be explicitly configured.</span></span> <span data-ttu-id="04482-138">Dans l' *appsettings.json* exemple suivant, un point de terminaison http/2 sécurisé avec TLS est fourni :</span><span class="sxs-lookup"><span data-stu-id="04482-138">In the following *appsettings.json* example, an HTTP/2 endpoint secured with TLS is provided:</span></span>

[!code-json[](~/grpc/aspnetcore/sample/appsettings.json?highlight=4)]

<span data-ttu-id="04482-139">Vous pouvez également configurer les points de terminaison Kestrel dans *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="04482-139">Alternatively, Kestrel endpoints can be configured in *Program.cs*:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/Program.cs?highlight=7&name=snippet)]

#### <a name="protocol-negotiation"></a><span data-ttu-id="04482-140">Négociation de protocole</span><span class="sxs-lookup"><span data-stu-id="04482-140">Protocol negotiation</span></span>

<span data-ttu-id="04482-141">TLS est utilisé pour davantage que la sécurisation de la communication.</span><span class="sxs-lookup"><span data-stu-id="04482-141">TLS is used for more than securing communication.</span></span> <span data-ttu-id="04482-142">La négociation de [négociation de protocole de couche d’application (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) TLS est utilisée pour négocier le protocole de connexion entre le client et le serveur lorsqu’un point de terminaison prend en charge plusieurs protocoles.</span><span class="sxs-lookup"><span data-stu-id="04482-142">The TLS [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) handshake is used to negotiate the connection protocol between the client and the server when an endpoint supports multiple protocols.</span></span> <span data-ttu-id="04482-143">Cette négociation détermine si la connexion utilise HTTP/1.1 ou HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="04482-143">This negotiation determines whether the connection uses HTTP/1.1 or HTTP/2.</span></span>

<span data-ttu-id="04482-144">Si un point de terminaison HTTP/2 est configuré sans TLS, le [ListenOptions. Protocols](xref:fundamentals/servers/kestrel#listenoptionsprotocols) du point de terminaison doit avoir la valeur `HttpProtocols.Http2` .</span><span class="sxs-lookup"><span data-stu-id="04482-144">If an HTTP/2 endpoint is configured without TLS, the endpoint's [ListenOptions.Protocols](xref:fundamentals/servers/kestrel#listenoptionsprotocols) must be set to `HttpProtocols.Http2`.</span></span> <span data-ttu-id="04482-145">Un point de terminaison avec plusieurs protocoles (par exemple, `HttpProtocols.Http1AndHttp2` ) ne peut pas être utilisé sans TLS, car il n’y a aucune négociation.</span><span class="sxs-lookup"><span data-stu-id="04482-145">An endpoint with multiple protocols (for example, `HttpProtocols.Http1AndHttp2`) can't be used without TLS because there is no negotiation.</span></span> <span data-ttu-id="04482-146">Toutes les connexions au point de terminaison non sécurisé par défaut pour les appels HTTP/1.1 et gRPC échouent.</span><span class="sxs-lookup"><span data-stu-id="04482-146">All connections to the unsecured endpoint default to HTTP/1.1, and gRPC calls fail.</span></span>

<span data-ttu-id="04482-147">Pour plus d’informations sur l’activation de HTTP/2 et de TLS avec Kestrel, consultez [configuration du point de terminaison Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="04482-147">For more information on enabling HTTP/2 and TLS with Kestrel, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!NOTE]
> <span data-ttu-id="04482-148">MacOS ne prend pas en charge ASP.NET Core gRPC avec TLS.</span><span class="sxs-lookup"><span data-stu-id="04482-148">macOS doesn't support ASP.NET Core gRPC with TLS.</span></span> <span data-ttu-id="04482-149">Une configuration supplémentaire est nécessaire pour exécuter correctement les services gRPC sur MacOS.</span><span class="sxs-lookup"><span data-stu-id="04482-149">Additional configuration is required to successfully run gRPC services on macOS.</span></span> <span data-ttu-id="04482-150">Pour plus d’informations, consultez [Impossible de démarrer l’application ASP.NET Core gRPC sur MacOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span><span class="sxs-lookup"><span data-stu-id="04482-150">For more information, see [Unable to start ASP.NET Core gRPC app on macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span></span>

## <a name="integration-with-aspnet-core-apis"></a><span data-ttu-id="04482-151">Intégration avec les API ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="04482-151">Integration with ASP.NET Core APIs</span></span>

<span data-ttu-id="04482-152">les services gRPC ont un accès complet aux fonctionnalités de ASP.NET Core telles que l' [injection de dépendances](xref:fundamentals/dependency-injection) (di) et la [journalisation](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="04482-152">gRPC services have full access to the ASP.NET Core features such as [Dependency Injection](xref:fundamentals/dependency-injection) (DI) and [Logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="04482-153">Par exemple, l’implémentation de service peut résoudre un service d’enregistreur d’événements à partir du conteneur DI via le constructeur :</span><span class="sxs-lookup"><span data-stu-id="04482-153">For example, the service implementation can resolve a logger service from the DI container via the constructor:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

<span data-ttu-id="04482-154">Par défaut, l’implémentation du service gRPC peut résoudre d’autres services DI avec n’importe quelle durée de vie (Singleton, étendue ou transitoire).</span><span class="sxs-lookup"><span data-stu-id="04482-154">By default, the gRPC service implementation can resolve other DI services with any lifetime (Singleton, Scoped, or Transient).</span></span>

### <a name="resolve-httpcontext-in-grpc-methods"></a><span data-ttu-id="04482-155">Résoudre HttpContext dans les méthodes gRPC</span><span class="sxs-lookup"><span data-stu-id="04482-155">Resolve HttpContext in gRPC methods</span></span>

<span data-ttu-id="04482-156">L’API gRPC permet d’accéder à certaines données de message HTTP/2, telles que la méthode, l’hôte, l’en-tête et les codes de fin.</span><span class="sxs-lookup"><span data-stu-id="04482-156">The gRPC API provides access to some HTTP/2 message data, such as the method, host, header, and trailers.</span></span> <span data-ttu-id="04482-157">L’accès s’effectue par le biais de l' `ServerCallContext` argument passé à chaque méthode gRPC :</span><span class="sxs-lookup"><span data-stu-id="04482-157">Access is through the `ServerCallContext` argument passed to each gRPC method:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService.cs?highlight=3-4&name=snippet)]

<span data-ttu-id="04482-158">`ServerCallContext` ne fournit pas un accès complet à `HttpContext` dans toutes les api ASP.net.</span><span class="sxs-lookup"><span data-stu-id="04482-158">`ServerCallContext` does not provide full access to `HttpContext` in all ASP.NET APIs.</span></span> <span data-ttu-id="04482-159">La `GetHttpContext` méthode d’extension fournit un accès complet au `HttpContext` représentant le message http/2 sous-jacent dans les API ASP.net :</span><span class="sxs-lookup"><span data-stu-id="04482-159">The `GetHttpContext` extension method provides full access to the `HttpContext` representing the underlying HTTP/2 message in ASP.NET APIs:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService2.cs?highlight=6-7&name=snippet)]


## <a name="additional-resources"></a><span data-ttu-id="04482-160">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="04482-160">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:fundamentals/servers/kestrel>
