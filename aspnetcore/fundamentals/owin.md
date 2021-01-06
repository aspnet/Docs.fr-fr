---
title: OWIN (Open Web Interface for .NET) avec ASP.NET Core
author: ardalis
description: Découvrez comment ASP.NET Core prend en charge OWIN (Open Web Interface pour .NET), ce qui permet aux applications web d’être dissociées des serveurs web.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 12/18/2018
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
uid: fundamentals/owin
ms.openlocfilehash: 6085abc45137223d7a676107cf06dc2cf97a5c19
ms.sourcegitcommit: 3593c4efa707edeaaceffbfa544f99f41fc62535
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/04/2021
ms.locfileid: "93060675"
---
# <a name="open-web-interface-for-net-owin-with-aspnet-core"></a><span data-ttu-id="a0c6b-103">OWIN (Open Web Interface for .NET) avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a0c6b-103">Open Web Interface for .NET (OWIN) with ASP.NET Core</span></span>

<span data-ttu-id="a0c6b-104">Par [Steve Smith](https://ardalis.com/) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a0c6b-104">By [Steve Smith](https://ardalis.com/) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a0c6b-105">ASP.NET Core prend en charge l’interface Open Web pour .NET (OWIN).</span><span class="sxs-lookup"><span data-stu-id="a0c6b-105">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="a0c6b-106">OWIN permet que les applications web soient dissociées des serveurs web.</span><span class="sxs-lookup"><span data-stu-id="a0c6b-106">OWIN allows web apps to be decoupled from web servers.</span></span> <span data-ttu-id="a0c6b-107">Elle définit une façon standard d’utiliser un intergiciel (middleware) dans un pipeline pour gérer les requêtes et les réponses associées.</span><span class="sxs-lookup"><span data-stu-id="a0c6b-107">It defines a standard way for middleware to be used in a pipeline to handle requests and associated responses.</span></span> <span data-ttu-id="a0c6b-108">Les applications ASP.NET Core et l’intergiciel peuvent interagir avec les applications OWIN, les serveurs et l’intergiciel.</span><span class="sxs-lookup"><span data-stu-id="a0c6b-108">ASP.NET Core applications and middleware can interoperate with OWIN-based applications, servers, and middleware.</span></span>

<span data-ttu-id="a0c6b-109">OWIN fournit une couche de dissociation qui permet à deux frameworks ayant des modèles d’objets distincts d’être utilisés ensemble.</span><span class="sxs-lookup"><span data-stu-id="a0c6b-109">OWIN provides a decoupling layer that allows two frameworks with disparate object models to be used together.</span></span> <span data-ttu-id="a0c6b-110">Le package `Microsoft.AspNetCore.Owin` fournit deux implémentations d’adaptateurs :</span><span class="sxs-lookup"><span data-stu-id="a0c6b-110">The `Microsoft.AspNetCore.Owin` package provides two adapter implementations:</span></span>

* <span data-ttu-id="a0c6b-111">ASP.NET Core sur OWIN</span><span class="sxs-lookup"><span data-stu-id="a0c6b-111">ASP.NET Core to OWIN</span></span> 
* <span data-ttu-id="a0c6b-112">OWIN sur ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a0c6b-112">OWIN to ASP.NET Core</span></span>

<span data-ttu-id="a0c6b-113">Cela permet à ASP.NET Core d’être hébergé sur un serveur/hôte compatible OWIN, ou à d’autres composants compatibles OWIN d’être exécutés sur ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a0c6b-113">This allows ASP.NET Core to be hosted on top of an OWIN compatible server/host or for other OWIN compatible components to be run on top of ASP.NET Core.</span></span>

> [!NOTE]
> <span data-ttu-id="a0c6b-114">L’utilisation de ces adaptateurs a un impact sur les performances.</span><span class="sxs-lookup"><span data-stu-id="a0c6b-114">Using these adapters comes with a performance cost.</span></span> <span data-ttu-id="a0c6b-115">Les applications utilisant uniquement des composants ASP.NET Core ne doivent pas utiliser les adaptateurs ou le package `Microsoft.AspNetCore.Owin`.</span><span class="sxs-lookup"><span data-stu-id="a0c6b-115">Apps using only ASP.NET Core components shouldn't use the `Microsoft.AspNetCore.Owin` package or adapters.</span></span>

<span data-ttu-id="a0c6b-116">[Afficher ou télécharger l’exemple de code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a0c6b-116">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="running-owin-middleware-in-the-aspnet-core-pipeline"></a><span data-ttu-id="a0c6b-117">Exécution de l’intergiciel (middleware) OWIN dans le pipeline ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a0c6b-117">Running OWIN middleware in the ASP.NET Core pipeline</span></span>

<span data-ttu-id="a0c6b-118">La prise en charge de l’interface OWIN d’ASP.NET Core est déployée dans le cadre du package `Microsoft.AspNetCore.Owin`.</span><span class="sxs-lookup"><span data-stu-id="a0c6b-118">ASP.NET Core's OWIN support is deployed as part of the `Microsoft.AspNetCore.Owin` package.</span></span> <span data-ttu-id="a0c6b-119">Vous pouvez importer la prise en charge d’OWIN dans votre projet en installant ce package.</span><span class="sxs-lookup"><span data-stu-id="a0c6b-119">You can import OWIN support into your project by installing this package.</span></span>

<span data-ttu-id="a0c6b-120">L’intergiciel OWIN est conforme à la [spécification OWIN](https://owin.org/spec/spec/owin-1.0.0.html), laquelle exige une interface `Func<IDictionary<string, object>, Task>` et la définition de clés spécifiques (comme `owin.ResponseBody`).</span><span class="sxs-lookup"><span data-stu-id="a0c6b-120">OWIN middleware conforms to the [OWIN specification](https://owin.org/spec/spec/owin-1.0.0.html), which requires a `Func<IDictionary<string, object>, Task>` interface, and specific keys be set (such as `owin.ResponseBody`).</span></span> <span data-ttu-id="a0c6b-121">L’intergiciel OWIN simple suivant affiche « Hello World » :</span><span class="sxs-lookup"><span data-stu-id="a0c6b-121">The following simple OWIN middleware displays "Hello World":</span></span>

```csharp
public Task OwinHello(IDictionary<string, object> environment)
{
    string responseText = "Hello World via OWIN";
    byte[] responseBytes = Encoding.UTF8.GetBytes(responseText);

    // OWIN Environment Keys: https://owin.org/spec/spec/owin-1.0.0.html
    var responseStream = (Stream)environment["owin.ResponseBody"];
    var responseHeaders = (IDictionary<string, string[]>)environment["owin.ResponseHeaders"];

    responseHeaders["Content-Length"] = new string[] { responseBytes.Length.ToString(CultureInfo.InvariantCulture) };
    responseHeaders["Content-Type"] = new string[] { "text/plain" };

    return responseStream.WriteAsync(responseBytes, 0, responseBytes.Length);
}
```

<span data-ttu-id="a0c6b-122">L’exemple de signature retourne un `Task` et accepte un `IDictionary<string, object>`, comme l’exige OWIN.</span><span class="sxs-lookup"><span data-stu-id="a0c6b-122">The sample signature returns a `Task` and accepts an `IDictionary<string, object>` as required by OWIN.</span></span>

<span data-ttu-id="a0c6b-123">Le code suivant montre comment ajouter le middleware `OwinHello` (présenté ci-dessus) au pipeline ASP.NET Core avec la méthode d’extension `UseOwin`.</span><span class="sxs-lookup"><span data-stu-id="a0c6b-123">The following code shows how to add the `OwinHello` middleware (shown above) to the ASP.NET Core pipeline with the `UseOwin` extension method.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseOwin(pipeline =>
    {
        pipeline(next => OwinHello);
    });
}
```

<span data-ttu-id="a0c6b-124">Vous pouvez configurer d’autres actions pour qu’elles soient effectuées dans le pipeline OWIN.</span><span class="sxs-lookup"><span data-stu-id="a0c6b-124">You can configure other actions to take place within the OWIN pipeline.</span></span>

> [!NOTE]
> <span data-ttu-id="a0c6b-125">Les en-têtes des réponses doivent être modifiés uniquement avant la première écriture dans le flux de réponse.</span><span class="sxs-lookup"><span data-stu-id="a0c6b-125">Response headers should only be modified prior to the first write to the response stream.</span></span>

> [!NOTE]
> <span data-ttu-id="a0c6b-126">Pour des raisons de performance, il est déconseillé d’effectuer plusieurs appels à `UseOwin`.</span><span class="sxs-lookup"><span data-stu-id="a0c6b-126">Multiple calls to `UseOwin` is discouraged for performance reasons.</span></span> <span data-ttu-id="a0c6b-127">Les composants OWIN fonctionnent mieux s’ils sont regroupés.</span><span class="sxs-lookup"><span data-stu-id="a0c6b-127">OWIN components will operate best if grouped together.</span></span>

```csharp
app.UseOwin(pipeline =>
{
    pipeline(next =>
    {
        return async environment =>
        {
            // Do something before.
            await next(environment);
            // Do something after.
        };
    });
});
```

<a name="hosting-on-owin"></a>

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a><span data-ttu-id="a0c6b-128">Exécuter ASP.NET Core sur un serveur OWIN et utiliser sa prise en charge des WebSockets</span><span class="sxs-lookup"><span data-stu-id="a0c6b-128">Run ASP.NET Core on an OWIN-based server and use its WebSockets support</span></span>

<span data-ttu-id="a0c6b-129">L’accès à des fonctionnalités comme les WebSockets constitue un autre exemple de la façon dont les fonctionnalités de serveurs OWIN peuvent être exploitées par ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a0c6b-129">Another example of how OWIN-based servers' features can be leveraged by ASP.NET Core is access to features like WebSockets.</span></span> <span data-ttu-id="a0c6b-130">Le serveur web .NET OWIN utilisé dans l’exemple précédent prend en charge les WebSockets intégrés, qui peuvent être exploités par une application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a0c6b-130">The .NET OWIN web server used in the previous example has support for Web Sockets built in, which can be leveraged by an ASP.NET Core application.</span></span> <span data-ttu-id="a0c6b-131">L’exemple ci-dessous montre une application web simple qui prend en charge les WebSockets et renvoie tout ce qui est envoyé au serveur via des WebSockets.</span><span class="sxs-lookup"><span data-stu-id="a0c6b-131">The example below shows a simple web app that supports Web Sockets and echoes back everything sent to the server through WebSockets.</span></span>

```csharp
public class Startup
{
    public void Configure(IApplicationBuilder app)
    {
        app.Use(async (context, next) =>
        {
            if (context.WebSockets.IsWebSocketRequest)
            {
                WebSocket webSocket = await context.WebSockets.AcceptWebSocketAsync();
                await EchoWebSocket(webSocket);
            }
            else
            {
                await next();
            }
        });

        app.Run(context =>
        {
            return context.Response.WriteAsync("Hello World");
        });
    }

    private async Task EchoWebSocket(WebSocket webSocket)
    {
        byte[] buffer = new byte[1024];
        WebSocketReceiveResult received = await webSocket.ReceiveAsync(
            new ArraySegment<byte>(buffer), CancellationToken.None);

        while (!webSocket.CloseStatus.HasValue)
        {
            // Echo anything we receive
            await webSocket.SendAsync(new ArraySegment<byte>(buffer, 0, received.Count), 
                received.MessageType, received.EndOfMessage, CancellationToken.None);

            received = await webSocket.ReceiveAsync(new ArraySegment<byte>(buffer), 
                CancellationToken.None);
        }

        await webSocket.CloseAsync(webSocket.CloseStatus.Value, 
            webSocket.CloseStatusDescription, CancellationToken.None);
    }
}
```

## <a name="owin-environment"></a><span data-ttu-id="a0c6b-132">Environnement OWIN</span><span class="sxs-lookup"><span data-stu-id="a0c6b-132">OWIN environment</span></span>

<span data-ttu-id="a0c6b-133">Vous pouvez construire un environnement OWIN avec le `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="a0c6b-133">You can construct an OWIN environment using the `HttpContext`.</span></span>

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a><span data-ttu-id="a0c6b-134">Clés OWIN</span><span class="sxs-lookup"><span data-stu-id="a0c6b-134">OWIN keys</span></span>

<span data-ttu-id="a0c6b-135">OWIN dépend d’un objet `IDictionary<string,object>` pour communiquer des informations tout au long d’un échange Requête HTTP/Réponse.</span><span class="sxs-lookup"><span data-stu-id="a0c6b-135">OWIN depends on an `IDictionary<string,object>` object to communicate information throughout an HTTP Request/Response exchange.</span></span> <span data-ttu-id="a0c6b-136">ASP.NET Core implémente les clés répertoriées ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="a0c6b-136">ASP.NET Core implements the keys listed below.</span></span> <span data-ttu-id="a0c6b-137">Consultez [la spécification principale, les extensions](https://owin.org/#spec) et les [recommandations relatives aux clés OWIN et aux clés communes](https://owin.org/spec/spec/CommonKeys.html).</span><span class="sxs-lookup"><span data-stu-id="a0c6b-137">See the [primary specification, extensions](https://owin.org/#spec), and [OWIN Key Guidelines and Common Keys](https://owin.org/spec/spec/CommonKeys.html).</span></span>

### <a name="request-data-owin-v100"></a><span data-ttu-id="a0c6b-138">Données de requête (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="a0c6b-138">Request data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="a0c6b-139">Clé</span><span class="sxs-lookup"><span data-stu-id="a0c6b-139">Key</span></span>               | <span data-ttu-id="a0c6b-140">Valeur (type)</span><span class="sxs-lookup"><span data-stu-id="a0c6b-140">Value (type)</span></span> | <span data-ttu-id="a0c6b-141">Description</span><span class="sxs-lookup"><span data-stu-id="a0c6b-141">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="a0c6b-142">owin.RequestScheme</span><span class="sxs-lookup"><span data-stu-id="a0c6b-142">owin.RequestScheme</span></span> | `String` |  |
| <span data-ttu-id="a0c6b-143">owin.RequestMethod</span><span class="sxs-lookup"><span data-stu-id="a0c6b-143">owin.RequestMethod</span></span>  | `String` | |    
| <span data-ttu-id="a0c6b-144">owin.RequestPathBase</span><span class="sxs-lookup"><span data-stu-id="a0c6b-144">owin.RequestPathBase</span></span>  | `String` | |    
| <span data-ttu-id="a0c6b-145">owin.RequestPath</span><span class="sxs-lookup"><span data-stu-id="a0c6b-145">owin.RequestPath</span></span> | `String` | |     
| <span data-ttu-id="a0c6b-146">owin.RequestQueryString</span><span class="sxs-lookup"><span data-stu-id="a0c6b-146">owin.RequestQueryString</span></span>  | `String` | |    
| <span data-ttu-id="a0c6b-147">owin.RequestProtocol</span><span class="sxs-lookup"><span data-stu-id="a0c6b-147">owin.RequestProtocol</span></span>  | `String` | |    
| <span data-ttu-id="a0c6b-148">owin.RequestHeaders</span><span class="sxs-lookup"><span data-stu-id="a0c6b-148">owin.RequestHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="a0c6b-149">owin.RequestBody</span><span class="sxs-lookup"><span data-stu-id="a0c6b-149">owin.RequestBody</span></span> | `Stream`  | |

### <a name="request-data-owin-v110"></a><span data-ttu-id="a0c6b-150">Données de requête (OWIN v1.1.0)</span><span class="sxs-lookup"><span data-stu-id="a0c6b-150">Request data (OWIN v1.1.0)</span></span>

| <span data-ttu-id="a0c6b-151">Clé</span><span class="sxs-lookup"><span data-stu-id="a0c6b-151">Key</span></span>               | <span data-ttu-id="a0c6b-152">Valeur (type)</span><span class="sxs-lookup"><span data-stu-id="a0c6b-152">Value (type)</span></span> | <span data-ttu-id="a0c6b-153">Description</span><span class="sxs-lookup"><span data-stu-id="a0c6b-153">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="a0c6b-154">owin.RequestId</span><span class="sxs-lookup"><span data-stu-id="a0c6b-154">owin.RequestId</span></span> | `String` | <span data-ttu-id="a0c6b-155">Facultatif</span><span class="sxs-lookup"><span data-stu-id="a0c6b-155">Optional</span></span> |

### <a name="response-data-owin-v100"></a><span data-ttu-id="a0c6b-156">Données de réponse (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="a0c6b-156">Response data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="a0c6b-157">Clé</span><span class="sxs-lookup"><span data-stu-id="a0c6b-157">Key</span></span>               | <span data-ttu-id="a0c6b-158">Valeur (type)</span><span class="sxs-lookup"><span data-stu-id="a0c6b-158">Value (type)</span></span> | <span data-ttu-id="a0c6b-159">Description</span><span class="sxs-lookup"><span data-stu-id="a0c6b-159">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="a0c6b-160">owin.ResponseStatusCode</span><span class="sxs-lookup"><span data-stu-id="a0c6b-160">owin.ResponseStatusCode</span></span> | `int` | <span data-ttu-id="a0c6b-161">Facultatif</span><span class="sxs-lookup"><span data-stu-id="a0c6b-161">Optional</span></span> |
| <span data-ttu-id="a0c6b-162">owin.ResponseReasonPhrase</span><span class="sxs-lookup"><span data-stu-id="a0c6b-162">owin.ResponseReasonPhrase</span></span> | `String` | <span data-ttu-id="a0c6b-163">Facultatif</span><span class="sxs-lookup"><span data-stu-id="a0c6b-163">Optional</span></span> |
| <span data-ttu-id="a0c6b-164">owin.ResponseHeaders</span><span class="sxs-lookup"><span data-stu-id="a0c6b-164">owin.ResponseHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="a0c6b-165">owin.ResponseBody</span><span class="sxs-lookup"><span data-stu-id="a0c6b-165">owin.ResponseBody</span></span> | `Stream`  | |

### <a name="other-data-owin-v100"></a><span data-ttu-id="a0c6b-166">Autres données (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="a0c6b-166">Other data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="a0c6b-167">Clé</span><span class="sxs-lookup"><span data-stu-id="a0c6b-167">Key</span></span>               | <span data-ttu-id="a0c6b-168">Valeur (type)</span><span class="sxs-lookup"><span data-stu-id="a0c6b-168">Value (type)</span></span> | <span data-ttu-id="a0c6b-169">Description</span><span class="sxs-lookup"><span data-stu-id="a0c6b-169">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="a0c6b-170">owin.CallCancelled</span><span class="sxs-lookup"><span data-stu-id="a0c6b-170">owin.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="a0c6b-171">owin.Version</span><span class="sxs-lookup"><span data-stu-id="a0c6b-171">owin.Version</span></span>  | `String` | |   

### <a name="common-keys"></a><span data-ttu-id="a0c6b-172">Clés communes</span><span class="sxs-lookup"><span data-stu-id="a0c6b-172">Common keys</span></span>

| <span data-ttu-id="a0c6b-173">Clé</span><span class="sxs-lookup"><span data-stu-id="a0c6b-173">Key</span></span>               | <span data-ttu-id="a0c6b-174">Valeur (type)</span><span class="sxs-lookup"><span data-stu-id="a0c6b-174">Value (type)</span></span> | <span data-ttu-id="a0c6b-175">Description</span><span class="sxs-lookup"><span data-stu-id="a0c6b-175">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="a0c6b-176">ssl.ClientCertificate</span><span class="sxs-lookup"><span data-stu-id="a0c6b-176">ssl.ClientCertificate</span></span> | `X509Certificate` |  |
| <span data-ttu-id="a0c6b-177">ssl.LoadClientCertAsync</span><span class="sxs-lookup"><span data-stu-id="a0c6b-177">ssl.LoadClientCertAsync</span></span>  | `Func<Task>` | |    
| <span data-ttu-id="a0c6b-178">server.RemoteIpAddress</span><span class="sxs-lookup"><span data-stu-id="a0c6b-178">server.RemoteIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="a0c6b-179">server.RemotePort</span><span class="sxs-lookup"><span data-stu-id="a0c6b-179">server.RemotePort</span></span> | `String` | |     
| <span data-ttu-id="a0c6b-180">server.LocalIpAddress</span><span class="sxs-lookup"><span data-stu-id="a0c6b-180">server.LocalIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="a0c6b-181">server.LocalPort</span><span class="sxs-lookup"><span data-stu-id="a0c6b-181">server.LocalPort</span></span>  | `String` | |    
| <span data-ttu-id="a0c6b-182">server.IsLocal</span><span class="sxs-lookup"><span data-stu-id="a0c6b-182">server.IsLocal</span></span>  | `bool` | |    
| <span data-ttu-id="a0c6b-183">server.OnSendingHeaders</span><span class="sxs-lookup"><span data-stu-id="a0c6b-183">server.OnSendingHeaders</span></span>  | `Action<Action<object>,object>` | |

### <a name="sendfiles-v030"></a><span data-ttu-id="a0c6b-184">SendFiles v0.3.0</span><span class="sxs-lookup"><span data-stu-id="a0c6b-184">SendFiles v0.3.0</span></span>

| <span data-ttu-id="a0c6b-185">Clé</span><span class="sxs-lookup"><span data-stu-id="a0c6b-185">Key</span></span>               | <span data-ttu-id="a0c6b-186">Valeur (type)</span><span class="sxs-lookup"><span data-stu-id="a0c6b-186">Value (type)</span></span> | <span data-ttu-id="a0c6b-187">Description</span><span class="sxs-lookup"><span data-stu-id="a0c6b-187">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="a0c6b-188">sendfile.SendAsync</span><span class="sxs-lookup"><span data-stu-id="a0c6b-188">sendfile.SendAsync</span></span> | <span data-ttu-id="a0c6b-189">Voir [Signature du délégué](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="a0c6b-189">See [delegate signature](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> | <span data-ttu-id="a0c6b-190">Par requête</span><span class="sxs-lookup"><span data-stu-id="a0c6b-190">Per Request</span></span> |

### <a name="opaque-v030"></a><span data-ttu-id="a0c6b-191">Opaque v0.3.0</span><span class="sxs-lookup"><span data-stu-id="a0c6b-191">Opaque v0.3.0</span></span>

| <span data-ttu-id="a0c6b-192">Clé</span><span class="sxs-lookup"><span data-stu-id="a0c6b-192">Key</span></span>               | <span data-ttu-id="a0c6b-193">Valeur (type)</span><span class="sxs-lookup"><span data-stu-id="a0c6b-193">Value (type)</span></span> | <span data-ttu-id="a0c6b-194">Description</span><span class="sxs-lookup"><span data-stu-id="a0c6b-194">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="a0c6b-195">opaque.Version</span><span class="sxs-lookup"><span data-stu-id="a0c6b-195">opaque.Version</span></span> | `String` |  |
| <span data-ttu-id="a0c6b-196">opaque.Upgrade</span><span class="sxs-lookup"><span data-stu-id="a0c6b-196">opaque.Upgrade</span></span> | `OpaqueUpgrade` | <span data-ttu-id="a0c6b-197">Voir [Signature du délégué](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="a0c6b-197">See [delegate signature](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="a0c6b-198">opaque.Stream</span><span class="sxs-lookup"><span data-stu-id="a0c6b-198">opaque.Stream</span></span> | `Stream` |  |
| <span data-ttu-id="a0c6b-199">opaque.CallCancelled</span><span class="sxs-lookup"><span data-stu-id="a0c6b-199">opaque.CallCancelled</span></span> | `CancellationToken` |  |

### <a name="websocket-v030"></a><span data-ttu-id="a0c6b-200">WebSocket v0.3.0</span><span class="sxs-lookup"><span data-stu-id="a0c6b-200">WebSocket v0.3.0</span></span>

| <span data-ttu-id="a0c6b-201">Clé</span><span class="sxs-lookup"><span data-stu-id="a0c6b-201">Key</span></span>               | <span data-ttu-id="a0c6b-202">Valeur (type)</span><span class="sxs-lookup"><span data-stu-id="a0c6b-202">Value (type)</span></span> | <span data-ttu-id="a0c6b-203">Description</span><span class="sxs-lookup"><span data-stu-id="a0c6b-203">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="a0c6b-204">websocket.Version</span><span class="sxs-lookup"><span data-stu-id="a0c6b-204">websocket.Version</span></span> | `String` |  |
| <span data-ttu-id="a0c6b-205">websocket.Accept</span><span class="sxs-lookup"><span data-stu-id="a0c6b-205">websocket.Accept</span></span> | `WebSocketAccept` | <span data-ttu-id="a0c6b-206">Voir [Signature du délégué](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="a0c6b-206">See [delegate signature](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="a0c6b-207">websocket.AcceptAlt</span><span class="sxs-lookup"><span data-stu-id="a0c6b-207">websocket.AcceptAlt</span></span> |  | <span data-ttu-id="a0c6b-208">Non spécifiée</span><span class="sxs-lookup"><span data-stu-id="a0c6b-208">Non-spec</span></span> |
| <span data-ttu-id="a0c6b-209">websocket.SubProtocol</span><span class="sxs-lookup"><span data-stu-id="a0c6b-209">websocket.SubProtocol</span></span> | `String` | <span data-ttu-id="a0c6b-210">Voir l’étape 5.5 de la [RFC 6455 Section 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2)</span><span class="sxs-lookup"><span data-stu-id="a0c6b-210">See [RFC6455 Section 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) Step 5.5</span></span> |
| <span data-ttu-id="a0c6b-211">websocket.SendAsync</span><span class="sxs-lookup"><span data-stu-id="a0c6b-211">websocket.SendAsync</span></span> | `WebSocketSendAsync` | <span data-ttu-id="a0c6b-212">Voir [Signature du délégué](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="a0c6b-212">See [delegate signature](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="a0c6b-213">websocket.ReceiveAsync</span><span class="sxs-lookup"><span data-stu-id="a0c6b-213">websocket.ReceiveAsync</span></span> | `WebSocketReceiveAsync` | <span data-ttu-id="a0c6b-214">Voir [Signature du délégué](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="a0c6b-214">See [delegate signature](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="a0c6b-215">websocket.CloseAsync</span><span class="sxs-lookup"><span data-stu-id="a0c6b-215">websocket.CloseAsync</span></span> | `WebSocketCloseAsync` | <span data-ttu-id="a0c6b-216">Voir [Signature du délégué](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="a0c6b-216">See [delegate signature](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="a0c6b-217">websocket.CallCancelled</span><span class="sxs-lookup"><span data-stu-id="a0c6b-217">websocket.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="a0c6b-218">websocket.ClientCloseStatus</span><span class="sxs-lookup"><span data-stu-id="a0c6b-218">websocket.ClientCloseStatus</span></span> | `int` | <span data-ttu-id="a0c6b-219">Facultatif</span><span class="sxs-lookup"><span data-stu-id="a0c6b-219">Optional</span></span> |
| <span data-ttu-id="a0c6b-220">websocket.ClientCloseDescription</span><span class="sxs-lookup"><span data-stu-id="a0c6b-220">websocket.ClientCloseDescription</span></span> | `String` | <span data-ttu-id="a0c6b-221">Facultatif</span><span class="sxs-lookup"><span data-stu-id="a0c6b-221">Optional</span></span> |

## <a name="additional-resources"></a><span data-ttu-id="a0c6b-222">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="a0c6b-222">Additional resources</span></span>

* [<span data-ttu-id="a0c6b-223">Middleware</span><span class="sxs-lookup"><span data-stu-id="a0c6b-223">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="a0c6b-224">Serveurs</span><span class="sxs-lookup"><span data-stu-id="a0c6b-224">Servers</span></span>](xref:fundamentals/servers/index)
