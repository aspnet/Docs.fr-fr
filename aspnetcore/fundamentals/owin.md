---
title: OWIN (Open Web Interface for .NET) avec ASP.NET Core
author: ardalis
description: Découvrez comment ASP.NET Core prend en charge OWIN (Open Web Interface pour .NET), ce qui permet aux applications web d’être dissociées des serveurs web.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 2/8/2021
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
ms.openlocfilehash: e476f3f62a250d960c809e5062b3bd8ca3a1940a
ms.sourcegitcommit: 54fe1ae5e7d068e27376d562183ef9ddc7afc432
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/10/2021
ms.locfileid: "102587230"
---
# <a name="open-web-interface-for-net-owin-with-aspnet-core"></a><span data-ttu-id="84fed-103">OWIN (Open Web Interface for .NET) avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="84fed-103">Open Web Interface for .NET (OWIN) with ASP.NET Core</span></span>

<span data-ttu-id="84fed-104">Par [Steve Smith](https://ardalis.com/) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="84fed-104">By [Steve Smith](https://ardalis.com/) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="84fed-105">ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="84fed-105">ASP.NET Core:</span></span>

* <span data-ttu-id="84fed-106">Prend en charge l’interface Open Web pour .NET (OWIN).</span><span class="sxs-lookup"><span data-stu-id="84fed-106">Supports the Open Web Interface for .NET (OWIN).</span></span>
* <span data-ttu-id="84fed-107">Dispose de remplacements compatibles avec .NET Core pour les `Microsoft.Owin.*` bibliothèques ([Katana](/aspnet/aspnet/overview/owin-and-katana/)).</span><span class="sxs-lookup"><span data-stu-id="84fed-107">Has .NET Core compatible replacements for the `Microsoft.Owin.*` ([Katana](/aspnet/aspnet/overview/owin-and-katana/)) libraries.</span></span>

<span data-ttu-id="84fed-108">OWIN permet que les applications web soient dissociées des serveurs web.</span><span class="sxs-lookup"><span data-stu-id="84fed-108">OWIN allows web apps to be decoupled from web servers.</span></span> <span data-ttu-id="84fed-109">Elle définit une façon standard d’utiliser un intergiciel (middleware) dans un pipeline pour gérer les requêtes et les réponses associées.</span><span class="sxs-lookup"><span data-stu-id="84fed-109">It defines a standard way for middleware to be used in a pipeline to handle requests and associated responses.</span></span> <span data-ttu-id="84fed-110">Les applications ASP.NET Core et l’intergiciel peuvent interagir avec les applications OWIN, les serveurs et l’intergiciel.</span><span class="sxs-lookup"><span data-stu-id="84fed-110">ASP.NET Core applications and middleware can interoperate with OWIN-based applications, servers, and middleware.</span></span>

<span data-ttu-id="84fed-111">OWIN fournit une couche de dissociation qui permet à deux frameworks ayant des modèles d’objets distincts d’être utilisés ensemble.</span><span class="sxs-lookup"><span data-stu-id="84fed-111">OWIN provides a decoupling layer that allows two frameworks with disparate object models to be used together.</span></span> <span data-ttu-id="84fed-112">Le package `Microsoft.AspNetCore.Owin` fournit deux implémentations d’adaptateurs :</span><span class="sxs-lookup"><span data-stu-id="84fed-112">The `Microsoft.AspNetCore.Owin` package provides two adapter implementations:</span></span>

* <span data-ttu-id="84fed-113">ASP.NET Core sur OWIN</span><span class="sxs-lookup"><span data-stu-id="84fed-113">ASP.NET Core to OWIN</span></span> 
* <span data-ttu-id="84fed-114">OWIN sur ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="84fed-114">OWIN to ASP.NET Core</span></span>

<span data-ttu-id="84fed-115">Cela permet à ASP.NET Core d’être hébergé sur un serveur/hôte compatible OWIN, ou à d’autres composants compatibles OWIN d’être exécutés sur ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="84fed-115">This allows ASP.NET Core to be hosted on top of an OWIN compatible server/host or for other OWIN compatible components to be run on top of ASP.NET Core.</span></span>

> [!NOTE]
> <span data-ttu-id="84fed-116">L’utilisation de ces adaptateurs a un impact sur les performances.</span><span class="sxs-lookup"><span data-stu-id="84fed-116">Using these adapters comes with a performance cost.</span></span> <span data-ttu-id="84fed-117">Les applications utilisant uniquement des composants ASP.NET Core ne doivent pas utiliser les adaptateurs ou le package `Microsoft.AspNetCore.Owin`.</span><span class="sxs-lookup"><span data-stu-id="84fed-117">Apps using only ASP.NET Core components shouldn't use the `Microsoft.AspNetCore.Owin` package or adapters.</span></span>

<span data-ttu-id="84fed-118">[Afficher ou télécharger l’exemple de code](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/fundamentals/owin/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="84fed-118">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/fundamentals/owin/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="running-owin-middleware-in-the-aspnet-core-pipeline"></a><span data-ttu-id="84fed-119">Exécution de l’intergiciel (middleware) OWIN dans le pipeline ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="84fed-119">Running OWIN middleware in the ASP.NET Core pipeline</span></span>

<span data-ttu-id="84fed-120">La prise en charge de l’interface OWIN d’ASP.NET Core est déployée dans le cadre du package `Microsoft.AspNetCore.Owin`.</span><span class="sxs-lookup"><span data-stu-id="84fed-120">ASP.NET Core's OWIN support is deployed as part of the `Microsoft.AspNetCore.Owin` package.</span></span> <span data-ttu-id="84fed-121">Vous pouvez importer la prise en charge d’OWIN dans votre projet en installant ce package.</span><span class="sxs-lookup"><span data-stu-id="84fed-121">You can import OWIN support into your project by installing this package.</span></span>

<span data-ttu-id="84fed-122">L’intergiciel OWIN est conforme à la [spécification OWIN](https://owin.org/spec/spec/owin-1.0.0.html), laquelle exige une interface `Func<IDictionary<string, object>, Task>` et la définition de clés spécifiques (comme `owin.ResponseBody`).</span><span class="sxs-lookup"><span data-stu-id="84fed-122">OWIN middleware conforms to the [OWIN specification](https://owin.org/spec/spec/owin-1.0.0.html), which requires a `Func<IDictionary<string, object>, Task>` interface, and specific keys be set (such as `owin.ResponseBody`).</span></span> <span data-ttu-id="84fed-123">L’intergiciel OWIN simple suivant affiche « Hello World » :</span><span class="sxs-lookup"><span data-stu-id="84fed-123">The following simple OWIN middleware displays "Hello World":</span></span>

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

<span data-ttu-id="84fed-124">L’exemple de signature retourne un `Task` et accepte un `IDictionary<string, object>`, comme l’exige OWIN.</span><span class="sxs-lookup"><span data-stu-id="84fed-124">The sample signature returns a `Task` and accepts an `IDictionary<string, object>` as required by OWIN.</span></span>

<span data-ttu-id="84fed-125">Le code suivant montre comment ajouter le middleware `OwinHello` (présenté ci-dessus) au pipeline ASP.NET Core avec la méthode d’extension `UseOwin`.</span><span class="sxs-lookup"><span data-stu-id="84fed-125">The following code shows how to add the `OwinHello` middleware (shown above) to the ASP.NET Core pipeline with the `UseOwin` extension method.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseOwin(pipeline =>
    {
        pipeline(next => OwinHello);
    });
}
```

<span data-ttu-id="84fed-126">Vous pouvez configurer d’autres actions pour qu’elles soient effectuées dans le pipeline OWIN.</span><span class="sxs-lookup"><span data-stu-id="84fed-126">You can configure other actions to take place within the OWIN pipeline.</span></span>

> [!NOTE]
> <span data-ttu-id="84fed-127">Les en-têtes des réponses doivent être modifiés uniquement avant la première écriture dans le flux de réponse.</span><span class="sxs-lookup"><span data-stu-id="84fed-127">Response headers should only be modified prior to the first write to the response stream.</span></span>

> [!NOTE]
> <span data-ttu-id="84fed-128">Pour des raisons de performance, il est déconseillé d’effectuer plusieurs appels à `UseOwin`.</span><span class="sxs-lookup"><span data-stu-id="84fed-128">Multiple calls to `UseOwin` is discouraged for performance reasons.</span></span> <span data-ttu-id="84fed-129">Les composants OWIN fonctionnent mieux s’ils sont regroupés.</span><span class="sxs-lookup"><span data-stu-id="84fed-129">OWIN components will operate best if grouped together.</span></span>

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

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a><span data-ttu-id="84fed-130">Exécuter ASP.NET Core sur un serveur OWIN et utiliser sa prise en charge des WebSockets</span><span class="sxs-lookup"><span data-stu-id="84fed-130">Run ASP.NET Core on an OWIN-based server and use its WebSockets support</span></span>

<span data-ttu-id="84fed-131">L’accès à des fonctionnalités comme les WebSockets constitue un autre exemple de la façon dont les fonctionnalités de serveurs OWIN peuvent être exploitées par ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="84fed-131">Another example of how OWIN-based servers' features can be leveraged by ASP.NET Core is access to features like WebSockets.</span></span> <span data-ttu-id="84fed-132">Le serveur web .NET OWIN utilisé dans l’exemple précédent prend en charge les WebSockets intégrés, qui peuvent être exploités par une application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="84fed-132">The .NET OWIN web server used in the previous example has support for Web Sockets built in, which can be leveraged by an ASP.NET Core application.</span></span> <span data-ttu-id="84fed-133">L’exemple ci-dessous montre une application web simple qui prend en charge les WebSockets et renvoie tout ce qui est envoyé au serveur via des WebSockets.</span><span class="sxs-lookup"><span data-stu-id="84fed-133">The example below shows a simple web app that supports Web Sockets and echoes back everything sent to the server through WebSockets.</span></span>

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

## <a name="owin-environment"></a><span data-ttu-id="84fed-134">Environnement OWIN</span><span class="sxs-lookup"><span data-stu-id="84fed-134">OWIN environment</span></span>

<span data-ttu-id="84fed-135">Vous pouvez construire un environnement OWIN avec le `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="84fed-135">You can construct an OWIN environment using the `HttpContext`.</span></span>

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a><span data-ttu-id="84fed-136">Clés OWIN</span><span class="sxs-lookup"><span data-stu-id="84fed-136">OWIN keys</span></span>

<span data-ttu-id="84fed-137">OWIN dépend d’un objet `IDictionary<string,object>` pour communiquer des informations tout au long d’un échange Requête HTTP/Réponse.</span><span class="sxs-lookup"><span data-stu-id="84fed-137">OWIN depends on an `IDictionary<string,object>` object to communicate information throughout an HTTP Request/Response exchange.</span></span> <span data-ttu-id="84fed-138">ASP.NET Core implémente les clés répertoriées ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="84fed-138">ASP.NET Core implements the keys listed below.</span></span> <span data-ttu-id="84fed-139">Consultez [la spécification principale, les extensions](https://owin.org/#spec) et les [recommandations relatives aux clés OWIN et aux clés communes](https://owin.org/spec/spec/CommonKeys.html).</span><span class="sxs-lookup"><span data-stu-id="84fed-139">See the [primary specification, extensions](https://owin.org/#spec), and [OWIN Key Guidelines and Common Keys](https://owin.org/spec/spec/CommonKeys.html).</span></span>

### <a name="request-data-owin-v100"></a><span data-ttu-id="84fed-140">Données de requête (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="84fed-140">Request data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="84fed-141">Clé :</span><span class="sxs-lookup"><span data-stu-id="84fed-141">Key</span></span>               | <span data-ttu-id="84fed-142">Valeur (type)</span><span class="sxs-lookup"><span data-stu-id="84fed-142">Value (type)</span></span> | <span data-ttu-id="84fed-143">Description</span><span class="sxs-lookup"><span data-stu-id="84fed-143">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="84fed-144">owin.RequestScheme</span><span class="sxs-lookup"><span data-stu-id="84fed-144">owin.RequestScheme</span></span> | `String` |  |
| <span data-ttu-id="84fed-145">owin.RequestMethod</span><span class="sxs-lookup"><span data-stu-id="84fed-145">owin.RequestMethod</span></span>  | `String` | |    
| <span data-ttu-id="84fed-146">owin.RequestPathBase</span><span class="sxs-lookup"><span data-stu-id="84fed-146">owin.RequestPathBase</span></span>  | `String` | |    
| <span data-ttu-id="84fed-147">owin.RequestPath</span><span class="sxs-lookup"><span data-stu-id="84fed-147">owin.RequestPath</span></span> | `String` | |     
| <span data-ttu-id="84fed-148">owin.RequestQueryString</span><span class="sxs-lookup"><span data-stu-id="84fed-148">owin.RequestQueryString</span></span>  | `String` | |    
| <span data-ttu-id="84fed-149">owin.RequestProtocol</span><span class="sxs-lookup"><span data-stu-id="84fed-149">owin.RequestProtocol</span></span>  | `String` | |    
| <span data-ttu-id="84fed-150">owin.RequestHeaders</span><span class="sxs-lookup"><span data-stu-id="84fed-150">owin.RequestHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="84fed-151">owin.RequestBody</span><span class="sxs-lookup"><span data-stu-id="84fed-151">owin.RequestBody</span></span> | `Stream`  | |

### <a name="request-data-owin-v110"></a><span data-ttu-id="84fed-152">Données de requête (OWIN v1.1.0)</span><span class="sxs-lookup"><span data-stu-id="84fed-152">Request data (OWIN v1.1.0)</span></span>

| <span data-ttu-id="84fed-153">Clé :</span><span class="sxs-lookup"><span data-stu-id="84fed-153">Key</span></span>               | <span data-ttu-id="84fed-154">Valeur (type)</span><span class="sxs-lookup"><span data-stu-id="84fed-154">Value (type)</span></span> | <span data-ttu-id="84fed-155">Description</span><span class="sxs-lookup"><span data-stu-id="84fed-155">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="84fed-156">owin.RequestId</span><span class="sxs-lookup"><span data-stu-id="84fed-156">owin.RequestId</span></span> | `String` | <span data-ttu-id="84fed-157">Facultatif</span><span class="sxs-lookup"><span data-stu-id="84fed-157">Optional</span></span> |

### <a name="response-data-owin-v100"></a><span data-ttu-id="84fed-158">Données de réponse (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="84fed-158">Response data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="84fed-159">Clé :</span><span class="sxs-lookup"><span data-stu-id="84fed-159">Key</span></span>               | <span data-ttu-id="84fed-160">Valeur (type)</span><span class="sxs-lookup"><span data-stu-id="84fed-160">Value (type)</span></span> | <span data-ttu-id="84fed-161">Description</span><span class="sxs-lookup"><span data-stu-id="84fed-161">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="84fed-162">owin.ResponseStatusCode</span><span class="sxs-lookup"><span data-stu-id="84fed-162">owin.ResponseStatusCode</span></span> | `int` | <span data-ttu-id="84fed-163">Facultatif</span><span class="sxs-lookup"><span data-stu-id="84fed-163">Optional</span></span> |
| <span data-ttu-id="84fed-164">owin.ResponseReasonPhrase</span><span class="sxs-lookup"><span data-stu-id="84fed-164">owin.ResponseReasonPhrase</span></span> | `String` | <span data-ttu-id="84fed-165">Facultatif</span><span class="sxs-lookup"><span data-stu-id="84fed-165">Optional</span></span> |
| <span data-ttu-id="84fed-166">owin.ResponseHeaders</span><span class="sxs-lookup"><span data-stu-id="84fed-166">owin.ResponseHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="84fed-167">owin.ResponseBody</span><span class="sxs-lookup"><span data-stu-id="84fed-167">owin.ResponseBody</span></span> | `Stream`  | |

### <a name="other-data-owin-v100"></a><span data-ttu-id="84fed-168">Autres données (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="84fed-168">Other data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="84fed-169">Clé :</span><span class="sxs-lookup"><span data-stu-id="84fed-169">Key</span></span>               | <span data-ttu-id="84fed-170">Valeur (type)</span><span class="sxs-lookup"><span data-stu-id="84fed-170">Value (type)</span></span> | <span data-ttu-id="84fed-171">Description</span><span class="sxs-lookup"><span data-stu-id="84fed-171">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="84fed-172">owin.CallCancelled</span><span class="sxs-lookup"><span data-stu-id="84fed-172">owin.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="84fed-173">owin.Version</span><span class="sxs-lookup"><span data-stu-id="84fed-173">owin.Version</span></span>  | `String` | |   

### <a name="common-keys"></a><span data-ttu-id="84fed-174">Clés communes</span><span class="sxs-lookup"><span data-stu-id="84fed-174">Common keys</span></span>

| <span data-ttu-id="84fed-175">Clé :</span><span class="sxs-lookup"><span data-stu-id="84fed-175">Key</span></span>               | <span data-ttu-id="84fed-176">Valeur (type)</span><span class="sxs-lookup"><span data-stu-id="84fed-176">Value (type)</span></span> | <span data-ttu-id="84fed-177">Description</span><span class="sxs-lookup"><span data-stu-id="84fed-177">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="84fed-178">ssl.ClientCertificate</span><span class="sxs-lookup"><span data-stu-id="84fed-178">ssl.ClientCertificate</span></span> | `X509Certificate` |  |
| <span data-ttu-id="84fed-179">ssl.LoadClientCertAsync</span><span class="sxs-lookup"><span data-stu-id="84fed-179">ssl.LoadClientCertAsync</span></span>  | `Func<Task>` | |    
| <span data-ttu-id="84fed-180">server.RemoteIpAddress</span><span class="sxs-lookup"><span data-stu-id="84fed-180">server.RemoteIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="84fed-181">server.RemotePort</span><span class="sxs-lookup"><span data-stu-id="84fed-181">server.RemotePort</span></span> | `String` | |     
| <span data-ttu-id="84fed-182">server.LocalIpAddress</span><span class="sxs-lookup"><span data-stu-id="84fed-182">server.LocalIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="84fed-183">server.LocalPort</span><span class="sxs-lookup"><span data-stu-id="84fed-183">server.LocalPort</span></span>  | `String` | |    
| <span data-ttu-id="84fed-184">server.IsLocal</span><span class="sxs-lookup"><span data-stu-id="84fed-184">server.IsLocal</span></span>  | `bool` | |    
| <span data-ttu-id="84fed-185">server.OnSendingHeaders</span><span class="sxs-lookup"><span data-stu-id="84fed-185">server.OnSendingHeaders</span></span>  | `Action<Action<object>,object>` | |

### <a name="sendfiles-v030"></a><span data-ttu-id="84fed-186">SendFiles v0.3.0</span><span class="sxs-lookup"><span data-stu-id="84fed-186">SendFiles v0.3.0</span></span>

| <span data-ttu-id="84fed-187">Clé :</span><span class="sxs-lookup"><span data-stu-id="84fed-187">Key</span></span>               | <span data-ttu-id="84fed-188">Valeur (type)</span><span class="sxs-lookup"><span data-stu-id="84fed-188">Value (type)</span></span> | <span data-ttu-id="84fed-189">Description</span><span class="sxs-lookup"><span data-stu-id="84fed-189">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="84fed-190">sendfile.SendAsync</span><span class="sxs-lookup"><span data-stu-id="84fed-190">sendfile.SendAsync</span></span> | <span data-ttu-id="84fed-191">Voir [Signature du délégué](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="84fed-191">See [delegate signature](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> | <span data-ttu-id="84fed-192">Par requête</span><span class="sxs-lookup"><span data-stu-id="84fed-192">Per Request</span></span> |

### <a name="opaque-v030"></a><span data-ttu-id="84fed-193">Opaque v0.3.0</span><span class="sxs-lookup"><span data-stu-id="84fed-193">Opaque v0.3.0</span></span>

| <span data-ttu-id="84fed-194">Clé :</span><span class="sxs-lookup"><span data-stu-id="84fed-194">Key</span></span>               | <span data-ttu-id="84fed-195">Valeur (type)</span><span class="sxs-lookup"><span data-stu-id="84fed-195">Value (type)</span></span> | <span data-ttu-id="84fed-196">Description</span><span class="sxs-lookup"><span data-stu-id="84fed-196">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="84fed-197">opaque.Version</span><span class="sxs-lookup"><span data-stu-id="84fed-197">opaque.Version</span></span> | `String` |  |
| <span data-ttu-id="84fed-198">opaque.Upgrade</span><span class="sxs-lookup"><span data-stu-id="84fed-198">opaque.Upgrade</span></span> | `OpaqueUpgrade` | <span data-ttu-id="84fed-199">Voir [Signature du délégué](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="84fed-199">See [delegate signature](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="84fed-200">opaque.Stream</span><span class="sxs-lookup"><span data-stu-id="84fed-200">opaque.Stream</span></span> | `Stream` |  |
| <span data-ttu-id="84fed-201">opaque.CallCancelled</span><span class="sxs-lookup"><span data-stu-id="84fed-201">opaque.CallCancelled</span></span> | `CancellationToken` |  |

### <a name="websocket-v030"></a><span data-ttu-id="84fed-202">WebSocket v0.3.0</span><span class="sxs-lookup"><span data-stu-id="84fed-202">WebSocket v0.3.0</span></span>

| <span data-ttu-id="84fed-203">Clé :</span><span class="sxs-lookup"><span data-stu-id="84fed-203">Key</span></span>               | <span data-ttu-id="84fed-204">Valeur (type)</span><span class="sxs-lookup"><span data-stu-id="84fed-204">Value (type)</span></span> | <span data-ttu-id="84fed-205">Description</span><span class="sxs-lookup"><span data-stu-id="84fed-205">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="84fed-206">websocket.Version</span><span class="sxs-lookup"><span data-stu-id="84fed-206">websocket.Version</span></span> | `String` |  |
| <span data-ttu-id="84fed-207">websocket.Accept</span><span class="sxs-lookup"><span data-stu-id="84fed-207">websocket.Accept</span></span> | `WebSocketAccept` | <span data-ttu-id="84fed-208">Voir [Signature du délégué](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="84fed-208">See [delegate signature](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="84fed-209">websocket.AcceptAlt</span><span class="sxs-lookup"><span data-stu-id="84fed-209">websocket.AcceptAlt</span></span> |  | <span data-ttu-id="84fed-210">Non spécifiée</span><span class="sxs-lookup"><span data-stu-id="84fed-210">Non-spec</span></span> |
| <span data-ttu-id="84fed-211">websocket.SubProtocol</span><span class="sxs-lookup"><span data-stu-id="84fed-211">websocket.SubProtocol</span></span> | `String` | <span data-ttu-id="84fed-212">Voir l’étape 5.5 de la [RFC 6455 Section 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2)</span><span class="sxs-lookup"><span data-stu-id="84fed-212">See [RFC6455 Section 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) Step 5.5</span></span> |
| <span data-ttu-id="84fed-213">websocket.SendAsync</span><span class="sxs-lookup"><span data-stu-id="84fed-213">websocket.SendAsync</span></span> | `WebSocketSendAsync` | <span data-ttu-id="84fed-214">Voir [Signature du délégué](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="84fed-214">See [delegate signature](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="84fed-215">websocket.ReceiveAsync</span><span class="sxs-lookup"><span data-stu-id="84fed-215">websocket.ReceiveAsync</span></span> | `WebSocketReceiveAsync` | <span data-ttu-id="84fed-216">Voir [Signature du délégué](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="84fed-216">See [delegate signature](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="84fed-217">websocket.CloseAsync</span><span class="sxs-lookup"><span data-stu-id="84fed-217">websocket.CloseAsync</span></span> | `WebSocketCloseAsync` | <span data-ttu-id="84fed-218">Voir [Signature du délégué](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="84fed-218">See [delegate signature](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="84fed-219">websocket.CallCancelled</span><span class="sxs-lookup"><span data-stu-id="84fed-219">websocket.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="84fed-220">websocket.ClientCloseStatus</span><span class="sxs-lookup"><span data-stu-id="84fed-220">websocket.ClientCloseStatus</span></span> | `int` | <span data-ttu-id="84fed-221">Facultatif</span><span class="sxs-lookup"><span data-stu-id="84fed-221">Optional</span></span> |
| <span data-ttu-id="84fed-222">websocket.ClientCloseDescription</span><span class="sxs-lookup"><span data-stu-id="84fed-222">websocket.ClientCloseDescription</span></span> | `String` | <span data-ttu-id="84fed-223">Facultatif</span><span class="sxs-lookup"><span data-stu-id="84fed-223">Optional</span></span> |

## <a name="additional-resources"></a><span data-ttu-id="84fed-224">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="84fed-224">Additional resources</span></span>

* [<span data-ttu-id="84fed-225">Middleware</span><span class="sxs-lookup"><span data-stu-id="84fed-225">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="84fed-226">Serveurs</span><span class="sxs-lookup"><span data-stu-id="84fed-226">Servers</span></span>](xref:fundamentals/servers/index)
