---
title: Tester l’intergiciel (middleware) ASP.NET Core
author: tratcher
description: Découvrez comment tester ASP.NET Core intergiciel avec TestServer.
ms.author: riande
ms.custom: mvc
ms.date: 5/12/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: test/middleware
ms.openlocfilehash: ea7fc0e889ab32cbaf23257b3e866519af0727aa
ms.sourcegitcommit: 69e1a79a572b0af17d08e81af12c594b7316f2e1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/15/2020
ms.locfileid: "83424535"
---
# <a name="test-aspnet-core-middleware"></a><span data-ttu-id="21bbd-103">Tester l’intergiciel (middleware) ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="21bbd-103">Test ASP.NET Core middleware</span></span>

<span data-ttu-id="21bbd-104">Par [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="21bbd-104">By [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="21bbd-105">L’intergiciel peut être testé en isolation avec <xref:Microsoft.AspNetCore.TestHost.TestServer> .</span><span class="sxs-lookup"><span data-stu-id="21bbd-105">Middleware can be tested in isolation with <xref:Microsoft.AspNetCore.TestHost.TestServer>.</span></span> <span data-ttu-id="21bbd-106">Vous pouvez ainsi :</span><span class="sxs-lookup"><span data-stu-id="21bbd-106">It allows you to:</span></span>

* <span data-ttu-id="21bbd-107">Instanciez un pipeline d’application contenant uniquement les composants que vous devez tester.</span><span class="sxs-lookup"><span data-stu-id="21bbd-107">Instantiate an app pipeline containing only the components that you need to test.</span></span>
* <span data-ttu-id="21bbd-108">Envoyer des demandes personnalisées pour vérifier le comportement de l’intergiciel (middleware).</span><span class="sxs-lookup"><span data-stu-id="21bbd-108">Send custom requests to verify middleware behavior.</span></span>

<span data-ttu-id="21bbd-109">Avantages :</span><span class="sxs-lookup"><span data-stu-id="21bbd-109">Advantages:</span></span>

* <span data-ttu-id="21bbd-110">Les demandes sont envoyées en mémoire au lieu d’être sérialisées sur le réseau.</span><span class="sxs-lookup"><span data-stu-id="21bbd-110">Requests are sent in-memory rather than being serialized over the network.</span></span>
* <span data-ttu-id="21bbd-111">Cela évite des préoccupations supplémentaires, telles que la gestion des ports et les certificats HTTPs.</span><span class="sxs-lookup"><span data-stu-id="21bbd-111">This avoids additional concerns, such as port management and HTTPS certificates.</span></span>
* <span data-ttu-id="21bbd-112">Les exceptions dans l’intergiciel (middleware) peuvent être directement transmises au test appelant.</span><span class="sxs-lookup"><span data-stu-id="21bbd-112">Exceptions in the middleware can flow directly back to the calling test.</span></span>
* <span data-ttu-id="21bbd-113">Il est possible de personnaliser les structures de données du serveur, telles que <xref:Microsoft.AspNetCore.Http.HttpContext> , directement dans le test.</span><span class="sxs-lookup"><span data-stu-id="21bbd-113">It's possible to customize server data structures, such as <xref:Microsoft.AspNetCore.Http.HttpContext>, directly in the test.</span></span>

## <a name="set-up-the-testserver"></a><span data-ttu-id="21bbd-114">Configurer TestServer</span><span class="sxs-lookup"><span data-stu-id="21bbd-114">Set up the TestServer</span></span>

<span data-ttu-id="21bbd-115">Dans le projet de test, créez un test :</span><span class="sxs-lookup"><span data-stu-id="21bbd-115">In the test project, create a test:</span></span>

* <span data-ttu-id="21bbd-116">Créez et démarrez un hôte qui utilise <xref:Microsoft.AspNetCore.TestHost.TestServer> .</span><span class="sxs-lookup"><span data-stu-id="21bbd-116">Build and start a host that uses <xref:Microsoft.AspNetCore.TestHost.TestServer>.</span></span>
* <span data-ttu-id="21bbd-117">Ajoutez les services requis que l’intergiciel utilise.</span><span class="sxs-lookup"><span data-stu-id="21bbd-117">Add any required services that the middleware uses.</span></span>
* <span data-ttu-id="21bbd-118">Configurez le pipeline de traitement pour utiliser l’intergiciel (middleware) pour le test.</span><span class="sxs-lookup"><span data-stu-id="21bbd-118">Configure the processing pipeline to use the middleware for the test.</span></span>

[!code-csharp[](middleware/samples_snapshot/3.x/setup.cs?highlight=4-18)]

## <a name="send-requests-with-httpclient"></a><span data-ttu-id="21bbd-119">Envoyer des demandes avec HttpClient</span><span class="sxs-lookup"><span data-stu-id="21bbd-119">Send requests with HttpClient</span></span>
<span data-ttu-id="21bbd-120">Envoyer une demande à l’aide de <xref:System.Net.Http.HttpClient> :</span><span class="sxs-lookup"><span data-stu-id="21bbd-120">Send a request using <xref:System.Net.Http.HttpClient>:</span></span>

[!code-csharp[](middleware/samples_snapshot/3.x/request.cs?highlight=20)]

<span data-ttu-id="21bbd-121">Déclarez le résultat.</span><span class="sxs-lookup"><span data-stu-id="21bbd-121">Assert the result.</span></span> <span data-ttu-id="21bbd-122">Tout d’abord, faites d’une assertion l’inverse du résultat attendu.</span><span class="sxs-lookup"><span data-stu-id="21bbd-122">First, make an assertion the opposite of the result that you expect.</span></span> <span data-ttu-id="21bbd-123">Une exécution initiale avec une assertion false positive confirme que le test échoue quand l’intergiciel s’exécute correctement.</span><span class="sxs-lookup"><span data-stu-id="21bbd-123">An initial run with a false positive assertion confirms that the test fails when the middleware is performing correctly.</span></span> <span data-ttu-id="21bbd-124">Exécutez le test et vérifiez que le test échoue.</span><span class="sxs-lookup"><span data-stu-id="21bbd-124">Run the test and confirm that the test fails.</span></span>

<span data-ttu-id="21bbd-125">Dans l’exemple suivant, l’intergiciel (middleware) doit retourner un code d’État 404 (*introuvable*) quand le point de terminaison racine est demandé.</span><span class="sxs-lookup"><span data-stu-id="21bbd-125">In the following example, the middleware should return a 404 status code (*Not Found*) when the root endpoint is requested.</span></span> <span data-ttu-id="21bbd-126">Effectuez la première série de tests avec `Assert.NotEqual( ... );` , qui doit échouer :</span><span class="sxs-lookup"><span data-stu-id="21bbd-126">Make the first test run with `Assert.NotEqual( ... );`, which should fail:</span></span>

[!code-csharp[](middleware/samples_snapshot/3.x/false-failure-check.cs?highlight=22)]

<span data-ttu-id="21bbd-127">Modifiez l’assertion pour tester l’intergiciel dans des conditions de fonctionnement normales.</span><span class="sxs-lookup"><span data-stu-id="21bbd-127">Change the assertion to test the middleware under normal operating conditions.</span></span> <span data-ttu-id="21bbd-128">Le test final utilise `Assert.Equal( ... );` .</span><span class="sxs-lookup"><span data-stu-id="21bbd-128">The final test uses `Assert.Equal( ... );`.</span></span> <span data-ttu-id="21bbd-129">Exécutez à nouveau le test pour confirmer qu’il réussit.</span><span class="sxs-lookup"><span data-stu-id="21bbd-129">Run the test again to confirm that it passes.</span></span>

[!code-csharp[](middleware/samples_snapshot/3.x/final-test.cs?highlight=22)]

## <a name="send-requests-with-httpcontext"></a><span data-ttu-id="21bbd-130">Envoyer des demandes avec HttpContext</span><span class="sxs-lookup"><span data-stu-id="21bbd-130">Send requests with HttpContext</span></span>

<span data-ttu-id="21bbd-131">Une application de test peut également envoyer une demande à l’aide de [SendAsync (action \< HttpContext>, CancellationToken)](xref:Microsoft.AspNetCore.TestHost.TestServer.SendAsync%2A).</span><span class="sxs-lookup"><span data-stu-id="21bbd-131">A test app can also send a request using [SendAsync(Action\<HttpContext>, CancellationToken)](xref:Microsoft.AspNetCore.TestHost.TestServer.SendAsync%2A).</span></span> <span data-ttu-id="21bbd-132">Dans l’exemple suivant, plusieurs vérifications sont effectuées lorsque `https://example.com/A/Path/?and=query` est traité par l’intergiciel (middleware) :</span><span class="sxs-lookup"><span data-stu-id="21bbd-132">In the following example, several checks are made when `https://example.com/A/Path/?and=query` is processed by the middleware:</span></span>

```csharp
[Fact]
public async Task TestMiddleware_ExpectedResponse()
{
    using var host = await new HostBuilder()
        .ConfigureWebHost(webBuilder =>
        {
            webBuilder
                .UseTestServer()
                .ConfigureServices(services =>
                {
                    services.AddMyServices();
                })
                .Configure(app =>
                {
                    app.UseMiddleware<MyMiddleware>();
                });
        })
        .StartAsync();

    var server = host.GetTestServer();
    server.BaseAddress = new Uri("https://example.com/A/Path/");

    var context = await server.SendAsync(c =>
    {
        c.Request.Method = HttpMethods.Post;
        c.Request.Path = "/and/file.txt";
        c.Request.QueryString = new QueryString("?and=query");
    });

    Assert.True(context.RequestAborted.CanBeCanceled);
    Assert.Equal(HttpProtocol.Http11, context.Request.Protocol);
    Assert.Equal("POST", context.Request.Method);
    Assert.Equal("https", context.Request.Scheme);
    Assert.Equal("example.com", context.Request.Host.Value);
    Assert.Equal("/A/Path", context.Request.PathBase.Value);
    Assert.Equal("/and/file.txt", context.Request.Path.Value);
    Assert.Equal("?and=query", context.Request.QueryString.Value);
    Assert.NotNull(context.Request.Body);
    Assert.NotNull(context.Request.Headers);
    Assert.NotNull(context.Response.Headers);
    Assert.NotNull(context.Response.Body);
    Assert.Equal(404, context.Response.StatusCode);
    Assert.Null(context.Features.Get<IHttpResponseFeature>().ReasonPhrase);
}
```

<span data-ttu-id="21bbd-133"><xref:Microsoft.AspNetCore.TestHost.TestServer.SendAsync%2A>autorise la configuration directe d’un <xref:Microsoft.AspNetCore.Http.HttpContext> objet plutôt que l’utilisation des <xref:System.Net.Http.HttpClient> abstractions.</span><span class="sxs-lookup"><span data-stu-id="21bbd-133"><xref:Microsoft.AspNetCore.TestHost.TestServer.SendAsync%2A> permits direct configuration of an <xref:Microsoft.AspNetCore.Http.HttpContext> object rather than using the <xref:System.Net.Http.HttpClient> abstractions.</span></span> <span data-ttu-id="21bbd-134">Utilisez <xref:Microsoft.AspNetCore.TestHost.TestServer.SendAsync%2A> pour manipuler des structures uniquement disponibles sur le serveur, telles que [HttpContext. Items](xref:Microsoft.AspNetCore.Http.HttpContext.Items) ou [HttpContext. Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features).</span><span class="sxs-lookup"><span data-stu-id="21bbd-134">Use <xref:Microsoft.AspNetCore.TestHost.TestServer.SendAsync%2A> to manipulate structures only available on the server, such as [HttpContext.Items](xref:Microsoft.AspNetCore.Http.HttpContext.Items) or [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features).</span></span>

<span data-ttu-id="21bbd-135">Comme dans l’exemple précédent qui a testé une réponse *404-introuvable* , vérifiez l’inverse pour chaque `Assert` instruction du test précédent.</span><span class="sxs-lookup"><span data-stu-id="21bbd-135">As with the earlier example that tested for a *404 - Not Found* response, check the opposite for each `Assert` statement in the preceding test.</span></span> <span data-ttu-id="21bbd-136">La vérification confirme que le test échoue correctement lorsque l’intergiciel fonctionne normalement.</span><span class="sxs-lookup"><span data-stu-id="21bbd-136">The check confirms that the test fails correctly when the middleware is operating normally.</span></span> <span data-ttu-id="21bbd-137">Une fois que vous avez confirmé que le test de faux positif fonctionne, définissez les `Assert` instructions finales pour les conditions et les valeurs attendues du test.</span><span class="sxs-lookup"><span data-stu-id="21bbd-137">After you've confirmed that the false positive test works, set the final `Assert` statements for the expected conditions and values of the test.</span></span> <span data-ttu-id="21bbd-138">Exécutez-le à nouveau pour confirmer que le test réussit.</span><span class="sxs-lookup"><span data-stu-id="21bbd-138">Run it again to confirm that the test passes.</span></span>

## <a name="testserver-limitations"></a><span data-ttu-id="21bbd-139">Limitations de TestServer</span><span class="sxs-lookup"><span data-stu-id="21bbd-139">TestServer limitations</span></span>

<span data-ttu-id="21bbd-140">TestServer</span><span class="sxs-lookup"><span data-stu-id="21bbd-140">TestServer:</span></span>

* <span data-ttu-id="21bbd-141">A été créé pour répliquer des comportements de serveur pour tester l’intergiciel (middleware).</span><span class="sxs-lookup"><span data-stu-id="21bbd-141">Was created to replicate server behaviors to test middleware.</span></span>
* <span data-ttu-id="21bbd-142">N’essaie ***pas*** de répliquer tous les <xref:System.Net.Http.HttpClient> comportements.</span><span class="sxs-lookup"><span data-stu-id="21bbd-142">Does ***not*** try to replicate all <xref:System.Net.Http.HttpClient> behaviors.</span></span>
* <span data-ttu-id="21bbd-143">Tente d’accorder au client un accès au plus grand contrôle du serveur que possible et avec autant de visibilité sur ce qui se passe sur le serveur que possible.</span><span class="sxs-lookup"><span data-stu-id="21bbd-143">Attempts to give the client access to as much control over the server as possible, and with as much visibility into what's happening on the server as possible.</span></span> <span data-ttu-id="21bbd-144">Par exemple, il peut lever des exceptions qui ne sont normalement pas levées par afin `HttpClient` de communiquer directement l’état du serveur.</span><span class="sxs-lookup"><span data-stu-id="21bbd-144">For example it may throw exceptions not normally thrown by `HttpClient` in order to directly communicate server state.</span></span>
* <span data-ttu-id="21bbd-145">Ne définit pas certains en-têtes de transport par défaut, car ceux-ci ne sont généralement pas pertinents pour l’intergiciel (middleware).</span><span class="sxs-lookup"><span data-stu-id="21bbd-145">Doesn't set some transport specific headers by default as those are not usually relevant to middleware.</span></span> <span data-ttu-id="21bbd-146">Pour plus d'informations, consultez la section suivante.</span><span class="sxs-lookup"><span data-stu-id="21bbd-146">For more information, see the next section.</span></span>

### <a name="content-length-and-transfer-encoding-headers"></a><span data-ttu-id="21bbd-147">En-têtes Content-Length et Transfer-Encoding</span><span class="sxs-lookup"><span data-stu-id="21bbd-147">Content-Length and Transfer-Encoding headers</span></span>

<span data-ttu-id="21bbd-148">TestServer ne définit ***pas*** les en-têtes de demande ou de réponse de transport tels que [Content-Length](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Length) ou [Transfer-Encoding](https://developer.mozilla.org/docs/Web/HTTP/Headers/Transfer-Encoding).</span><span class="sxs-lookup"><span data-stu-id="21bbd-148">TestServer does ***not*** set transport related request or response headers such as [Content-Length](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Length) or [Transfer-Encoding](https://developer.mozilla.org/docs/Web/HTTP/Headers/Transfer-Encoding).</span></span> <span data-ttu-id="21bbd-149">Les applications doivent éviter en fonction de ces en-têtes, car leur utilisation varie selon le client, le scénario et le protocole.</span><span class="sxs-lookup"><span data-stu-id="21bbd-149">Applications should avoid depending on these headers because their usage varies by client, scenario, and protocol.</span></span> <span data-ttu-id="21bbd-150">Si `Content-Length` et `Transfer-Encoding` sont nécessaires pour tester un scénario spécifique, ils peuvent être spécifiés dans le test lors de la composition de <xref:System.Net.Http.HttpRequestMessage> ou de <xref:Microsoft.AspNetCore.Http.HttpContext> .</span><span class="sxs-lookup"><span data-stu-id="21bbd-150">If `Content-Length` and `Transfer-Encoding` are necessary to test a specific scenario, they can be specified in the test when composing the <xref:System.Net.Http.HttpRequestMessage> or <xref:Microsoft.AspNetCore.Http.HttpContext>.</span></span> <span data-ttu-id="21bbd-151">Pour plus d’informations, consultez les problèmes GitHub suivants :</span><span class="sxs-lookup"><span data-stu-id="21bbd-151">For more information, see the following GitHub issues:</span></span>

* [<span data-ttu-id="21bbd-152">dotnet/aspnetcore # 21677</span><span class="sxs-lookup"><span data-stu-id="21bbd-152">dotnet/aspnetcore#21677</span></span>](https://github.com/dotnet/aspnetcore/issues/21677)
* [<span data-ttu-id="21bbd-153">dotnet/aspnetcore # 18463</span><span class="sxs-lookup"><span data-stu-id="21bbd-153">dotnet/aspnetcore#18463</span></span>](https://github.com/dotnet/aspnetcore/issues/18463)
* [<span data-ttu-id="21bbd-154">dotnet/aspnetcore # 13273</span><span class="sxs-lookup"><span data-stu-id="21bbd-154">dotnet/aspnetcore#13273</span></span>](https://github.com/dotnet/aspnetcore/issues/13273)