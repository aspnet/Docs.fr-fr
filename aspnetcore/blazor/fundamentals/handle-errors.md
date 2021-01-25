---
title: Gérer les erreurs dans les Blazor applications ASP.net Core
author: guardrex
description: Découvrez comment ASP.NET Core Blazor comment Blazor gère les exceptions non gérées et comment développer des applications qui détectent et gèrent les erreurs.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/23/2020
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
uid: blazor/fundamentals/handle-errors
ms.openlocfilehash: 5a255c2d3535311cecd6b7219447e80d1ae78877
ms.sourcegitcommit: d4836f9b7c508f51c6c4ee6d0cc719b38c1729c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98758253"
---
# <a name="handle-errors-in-aspnet-core-no-locblazor-apps"></a><span data-ttu-id="d0d6e-103">Gérer les erreurs dans les Blazor applications ASP.net Core</span><span class="sxs-lookup"><span data-stu-id="d0d6e-103">Handle errors in ASP.NET Core Blazor apps</span></span>

<span data-ttu-id="d0d6e-104">Par [Steve Sanderson](https://github.com/SteveSandersonMS)</span><span class="sxs-lookup"><span data-stu-id="d0d6e-104">By [Steve Sanderson](https://github.com/SteveSandersonMS)</span></span>

<span data-ttu-id="d0d6e-105">Cet article explique comment Blazor gère les exceptions non gérées et comment développer des applications qui détectent et gèrent les erreurs.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-105">This article describes how Blazor manages unhandled exceptions and how to develop apps that detect and handle errors.</span></span>

## <a name="detailed-errors-during-development"></a><span data-ttu-id="d0d6e-106">Erreurs détaillées pendant le développement</span><span class="sxs-lookup"><span data-stu-id="d0d6e-106">Detailed errors during development</span></span>

<span data-ttu-id="d0d6e-107">Quand une Blazor application ne fonctionne pas correctement pendant le développement, la réception d’informations d’erreur détaillées de l’application vous aide à résoudre les problèmes et à résoudre le problème.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-107">When a Blazor app isn't functioning properly during development, receiving detailed error information from the app assists in troubleshooting and fixing the issue.</span></span> <span data-ttu-id="d0d6e-108">Lorsqu’une erreur se produit, les Blazor applications affichent une barre dorée en bas de l’écran :</span><span class="sxs-lookup"><span data-stu-id="d0d6e-108">When an error occurs, Blazor apps display a gold bar at the bottom of the screen:</span></span>

* <span data-ttu-id="d0d6e-109">Pendant le développement, la barre dorée vous dirige vers la console du navigateur, où vous pouvez voir l’exception.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-109">During development, the gold bar directs you to the browser console, where you can see the exception.</span></span>
* <span data-ttu-id="d0d6e-110">En production, la barre dorée avertit l’utilisateur qu’une erreur s’est produite et recommande l’actualisation du navigateur.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-110">In production, the gold bar notifies the user that an error has occurred and recommends refreshing the browser.</span></span>

<span data-ttu-id="d0d6e-111">L’interface utilisateur de cette expérience de gestion des erreurs fait partie des Blazor modèles de projet.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-111">The UI for this error handling experience is part of the Blazor project templates.</span></span>

<span data-ttu-id="d0d6e-112">Dans une Blazor WebAssembly application, personnalisez l’expérience dans le `wwwroot/index.html` fichier :</span><span class="sxs-lookup"><span data-stu-id="d0d6e-112">In a Blazor WebAssembly app, customize the experience in the `wwwroot/index.html` file:</span></span>

```html
<div id="blazor-error-ui">
    An unhandled error has occurred.
    <a href="" class="reload">Reload</a>
    <a class="dismiss">🗙</a>
</div>
```

<span data-ttu-id="d0d6e-113">Dans une Blazor Server application, personnalisez l’expérience dans le `Pages/_Host.cshtml` fichier :</span><span class="sxs-lookup"><span data-stu-id="d0d6e-113">In a Blazor Server app, customize the experience in the `Pages/_Host.cshtml` file:</span></span>

```cshtml
<div id="blazor-error-ui">
    <environment include="Staging,Production">
        An error has occurred. This application may no longer respond until reloaded.
    </environment>
    <environment include="Development">
        An unhandled exception has occurred. See browser dev tools for details.
    </environment>
    <a href="" class="reload">Reload</a>
    <a class="dismiss">🗙</a>
</div>
```

<span data-ttu-id="d0d6e-114">L' `blazor-error-ui` élément est masqué par les styles inclus dans les Blazor modèles ( `wwwroot/css/app.css` ou `wwwroot/css/site.css` ), puis indiqué lorsqu’une erreur se produit :</span><span class="sxs-lookup"><span data-stu-id="d0d6e-114">The `blazor-error-ui` element is hidden by the styles included in the Blazor templates (`wwwroot/css/app.css` or `wwwroot/css/site.css`) and then shown when an error occurs:</span></span>

```css
#blazor-error-ui {
    background: lightyellow;
    bottom: 0;
    box-shadow: 0 -1px 2px rgba(0, 0, 0, 0.2);
    display: none;
    left: 0;
    padding: 0.6rem 1.25rem 0.7rem 1.25rem;
    position: fixed;
    width: 100%;
    z-index: 1000;
}

#blazor-error-ui .dismiss {
    cursor: pointer;
    position: absolute;
    right: 0.75rem;
    top: 0.5rem;
}
```

## <a name="no-locblazor-server-detailed-circuit-errors"></a><span data-ttu-id="d0d6e-115">Blazor Server Erreurs de circuit détaillées</span><span class="sxs-lookup"><span data-stu-id="d0d6e-115">Blazor Server detailed circuit errors</span></span>

<span data-ttu-id="d0d6e-116">Les erreurs côté client n’incluent pas la pile des appels et ne fournissent pas de détails sur la cause de l’erreur, mais les journaux du serveur contiennent ces informations.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-116">Client-side errors don't include the callstack and don't provide detail on the cause of the error, but server logs do contain such information.</span></span> <span data-ttu-id="d0d6e-117">À des fins de développement, des informations sur les erreurs de circuit sensible peuvent être mises à la disposition du client en activant des erreurs détaillées.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-117">For development purposes, sensitive circuit error information can be made available to the client by enabling detailed errors.</span></span>

<span data-ttu-id="d0d6e-118">Activez Blazor Server les erreurs détaillées à l’aide des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="d0d6e-118">Enable Blazor Server detailed errors using the following approaches:</span></span>

* <span data-ttu-id="d0d6e-119"><xref:Microsoft.AspNetCore.Components.Server.CircuitOptions.DetailedErrors?displayProperty=nameWithType>.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-119"><xref:Microsoft.AspNetCore.Components.Server.CircuitOptions.DetailedErrors?displayProperty=nameWithType>.</span></span>
* <span data-ttu-id="d0d6e-120">La `DetailedErrors` clé de configuration définie sur `true` , qui peut être définie dans le fichier de paramètres de développement de l’application ( `appsettings.Development.json` ).</span><span class="sxs-lookup"><span data-stu-id="d0d6e-120">The `DetailedErrors` configuration key set to `true`, which can be set in the app's Development settings file (`appsettings.Development.json`).</span></span> <span data-ttu-id="d0d6e-121">La clé peut également être définie à l’aide `ASPNETCORE_DETAILEDERRORS` de la variable d’environnement avec la valeur `true` .</span><span class="sxs-lookup"><span data-stu-id="d0d6e-121">The key can also be set using the `ASPNETCORE_DETAILEDERRORS` environment variable with a value of `true`.</span></span>
* <span data-ttu-id="d0d6e-122">la [ SignalR journalisation côté serveur](xref:signalr/diagnostics#server-side-logging) ( `Microsoft.AspNetCore.SignalR` ) peut être définie sur [Déboguer](xref:Microsoft.Extensions.Logging.LogLevel) ou sur [suivi](xref:Microsoft.Extensions.Logging.LogLevel) pour la SignalR journalisation détaillée.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-122">[SignalR server-side logging](xref:signalr/diagnostics#server-side-logging) (`Microsoft.AspNetCore.SignalR`) can be set to [Debug](xref:Microsoft.Extensions.Logging.LogLevel) or [Trace](xref:Microsoft.Extensions.Logging.LogLevel) for detailed SignalR logging.</span></span>

<span data-ttu-id="d0d6e-123">`appsettings.Development.json`:</span><span class="sxs-lookup"><span data-stu-id="d0d6e-123">`appsettings.Development.json`:</span></span>

```json
{
  "DetailedErrors": true,
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information",
      "Microsoft.AspNetCore.SignalR": "Debug"
    }
  }
}
```

> [!WARNING]
> <span data-ttu-id="d0d6e-124">L’exposition des informations sur les erreurs aux clients sur Internet est un risque de sécurité qui doit toujours être évité.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-124">Exposing error information to clients on the Internet is a security risk that should always be avoided.</span></span>

## <a name="how-a-no-locblazor-server-app-reacts-to-unhandled-exceptions"></a><span data-ttu-id="d0d6e-125">Comment une Blazor Server application réagit aux exceptions non gérées</span><span class="sxs-lookup"><span data-stu-id="d0d6e-125">How a Blazor Server app reacts to unhandled exceptions</span></span>

<span data-ttu-id="d0d6e-126">Blazor Server est une infrastructure avec état.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-126">Blazor Server is a stateful framework.</span></span> <span data-ttu-id="d0d6e-127">Tandis que les utilisateurs interagissent avec une application, ils maintiennent une connexion au serveur appelé « *circuit*».</span><span class="sxs-lookup"><span data-stu-id="d0d6e-127">While users interact with an app, they maintain a connection to the server known as a *circuit*.</span></span> <span data-ttu-id="d0d6e-128">Le circuit contient des instances de composant actives, ainsi que de nombreux autres aspects de l’État, tels que :</span><span class="sxs-lookup"><span data-stu-id="d0d6e-128">The circuit holds active component instances, plus many other aspects of state, such as:</span></span>

* <span data-ttu-id="d0d6e-129">Sortie du rendu le plus récent des composants.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-129">The most recent rendered output of components.</span></span>
* <span data-ttu-id="d0d6e-130">Ensemble actuel de délégués de gestion d’événements qui peuvent être déclenchés par les événements côté client.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-130">The current set of event-handling delegates that could be triggered by client-side events.</span></span>

<span data-ttu-id="d0d6e-131">Si un utilisateur ouvre l’application dans plusieurs onglets de navigateur, il dispose de plusieurs circuits indépendants.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-131">If a user opens the app in multiple browser tabs, they have multiple independent circuits.</span></span>

<span data-ttu-id="d0d6e-132">Blazor traite la plupart des exceptions non gérées comme étant irrécupérables par le circuit dans lequel elles se produisent.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-132">Blazor treats most unhandled exceptions as fatal to the circuit where they occur.</span></span> <span data-ttu-id="d0d6e-133">Si un circuit est arrêté en raison d’une exception non gérée, l’utilisateur ne peut continuer à interagir avec l’application qu’en rechargeant la page pour créer un nouveau circuit.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-133">If a circuit is terminated due to an unhandled exception, the user can only continue to interact with the app by reloading the page to create a new circuit.</span></span> <span data-ttu-id="d0d6e-134">Les circuits en dehors de celui qui est terminé, qui sont des circuits pour d’autres utilisateurs ou d’autres onglets de navigateur, ne sont pas affectés.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-134">Circuits outside of the one that's terminated, which are circuits for other users or other browser tabs, aren't affected.</span></span> <span data-ttu-id="d0d6e-135">Ce scénario est similaire à une application de bureau qui se bloque.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-135">This scenario is similar to a desktop app that crashes.</span></span> <span data-ttu-id="d0d6e-136">L’application bloquée doit être redémarrée, mais les autres applications ne sont pas affectées.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-136">The crashed app must be restarted, but other apps aren't affected.</span></span>

<span data-ttu-id="d0d6e-137">Un circuit se termine lorsqu’une exception non gérée se produit pour les raisons suivantes :</span><span class="sxs-lookup"><span data-stu-id="d0d6e-137">A circuit is terminated when an unhandled exception occurs for the following reasons:</span></span>

* <span data-ttu-id="d0d6e-138">Une exception non gérée rend souvent le circuit dans un État indéfini.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-138">An unhandled exception often leaves the circuit in an undefined state.</span></span>
* <span data-ttu-id="d0d6e-139">L’opération normale de l’application ne peut pas être garantie après une exception non gérée.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-139">The app's normal operation can't be guaranteed after an unhandled exception.</span></span>
* <span data-ttu-id="d0d6e-140">Des failles de sécurité peuvent apparaître dans l’application si le circuit continue.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-140">Security vulnerabilities may appear in the app if the circuit continues.</span></span>

## <a name="manage-unhandled-exceptions-in-developer-code"></a><span data-ttu-id="d0d6e-141">Gérer les exceptions non gérées dans le code du développeur</span><span class="sxs-lookup"><span data-stu-id="d0d6e-141">Manage unhandled exceptions in developer code</span></span>

<span data-ttu-id="d0d6e-142">Pour qu’une application continue après une erreur, l’application doit avoir une logique de gestion des erreurs.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-142">For an app to continue after an error, the app must have error handling logic.</span></span> <span data-ttu-id="d0d6e-143">Les sections suivantes de cet article décrivent les sources potentielles d’exceptions non gérées.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-143">Later sections of this article describe potential sources of unhandled exceptions.</span></span>

<span data-ttu-id="d0d6e-144">En production, ne rendez pas les messages d’exception d’infrastructure ou les traces de pile dans l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-144">In production, don't render framework exception messages or stack traces in the UI.</span></span> <span data-ttu-id="d0d6e-145">Le rendu des messages d’exception ou des traces de pile peut :</span><span class="sxs-lookup"><span data-stu-id="d0d6e-145">Rendering exception messages or stack traces could:</span></span>

* <span data-ttu-id="d0d6e-146">Divulguer des informations sensibles aux utilisateurs finaux.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-146">Disclose sensitive information to end users.</span></span>
* <span data-ttu-id="d0d6e-147">Aidez un utilisateur malveillant à découvrir des faiblesses dans une application qui peuvent compromettre la sécurité de l’application, du serveur ou du réseau.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-147">Help a malicious user discover weaknesses in an app that can compromise the security of the app, server, or network.</span></span>

## <a name="log-errors-with-a-persistent-provider"></a><span data-ttu-id="d0d6e-148">Consigner les erreurs avec un fournisseur persistant</span><span class="sxs-lookup"><span data-stu-id="d0d6e-148">Log errors with a persistent provider</span></span>

<span data-ttu-id="d0d6e-149">Si une exception non gérée se produit, l’exception est consignée dans <xref:Microsoft.Extensions.Logging.ILogger> les instances configurées dans le conteneur de service.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-149">If an unhandled exception occurs, the exception is logged to <xref:Microsoft.Extensions.Logging.ILogger> instances configured in the service container.</span></span> <span data-ttu-id="d0d6e-150">Par défaut, Blazor les applications se connectent à la sortie de la console avec le fournisseur d’informations de journalisation de la console.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-150">By default, Blazor apps log to console output with the Console Logging Provider.</span></span> <span data-ttu-id="d0d6e-151">Envisagez de vous connecter à un emplacement plus permanent avec un fournisseur qui gère la taille du journal et la rotation des journaux.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-151">Consider logging to a more permanent location with a provider that manages log size and log rotation.</span></span> <span data-ttu-id="d0d6e-152">Pour plus d’informations, consultez <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-152">For more information, see <xref:fundamentals/logging/index>.</span></span>

<span data-ttu-id="d0d6e-153">Pendant le développement, Blazor envoie généralement les détails complets des exceptions à la console du navigateur pour faciliter le débogage.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-153">During development, Blazor usually sends the full details of exceptions to the browser's console to aid in debugging.</span></span> <span data-ttu-id="d0d6e-154">En production, les erreurs détaillées dans la console du navigateur sont désactivées par défaut, ce qui signifie que les erreurs ne sont pas envoyées aux clients, mais que les détails complets de l’exception sont toujours consignés côté serveur.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-154">In production, detailed errors in the browser's console are disabled by default, which means that errors aren't sent to clients but the exception's full details are still logged server-side.</span></span> <span data-ttu-id="d0d6e-155">Pour plus d’informations, consultez <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-155">For more information, see <xref:fundamentals/error-handling>.</span></span>

<span data-ttu-id="d0d6e-156">Vous devez choisir les incidents à enregistrer et le niveau de gravité des incidents journalisés.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-156">You must decide which incidents to log and the level of severity of logged incidents.</span></span> <span data-ttu-id="d0d6e-157">Les utilisateurs hostiles peuvent être en mesure de déclencher délibérément des erreurs.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-157">Hostile users might be able to trigger errors deliberately.</span></span> <span data-ttu-id="d0d6e-158">Par exemple, ne consignez pas un incident à partir d’une erreur où un inconnu `ProductId` est fourni dans l’URL d’un composant qui affiche les détails du produit.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-158">For example, don't log an incident from an error where an unknown `ProductId` is supplied in the URL of a component that displays product details.</span></span> <span data-ttu-id="d0d6e-159">Toutes les erreurs ne doivent pas être traitées comme des incidents de gravité élevée pour la journalisation.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-159">Not all errors should be treated as high-severity incidents for logging.</span></span>

<span data-ttu-id="d0d6e-160">Pour plus d’informations, consultez <xref:blazor/fundamentals/logging>.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-160">For more information, see <xref:blazor/fundamentals/logging>.</span></span>

## <a name="places-where-errors-may-occur"></a><span data-ttu-id="d0d6e-161">Emplacements où des erreurs peuvent se produire</span><span class="sxs-lookup"><span data-stu-id="d0d6e-161">Places where errors may occur</span></span>

<span data-ttu-id="d0d6e-162">Le code d’infrastructure et d’application peut déclencher des exceptions non prises en charge dans l’un des emplacements suivants :</span><span class="sxs-lookup"><span data-stu-id="d0d6e-162">Framework and app code may trigger unhandled exceptions in any of the following locations:</span></span>

* [<span data-ttu-id="d0d6e-163">Instanciation du composant</span><span class="sxs-lookup"><span data-stu-id="d0d6e-163">Component instantiation</span></span>](#component-instantiation)
* [<span data-ttu-id="d0d6e-164">Méthodes de cycle de vie</span><span class="sxs-lookup"><span data-stu-id="d0d6e-164">Lifecycle methods</span></span>](#lifecycle-methods)
* [<span data-ttu-id="d0d6e-165">Logique de rendu</span><span class="sxs-lookup"><span data-stu-id="d0d6e-165">Rendering logic</span></span>](#rendering-logic)
* [<span data-ttu-id="d0d6e-166">Gestionnaires d’événements</span><span class="sxs-lookup"><span data-stu-id="d0d6e-166">Event handlers</span></span>](#event-handlers)
* [<span data-ttu-id="d0d6e-167">Suppression de composants</span><span class="sxs-lookup"><span data-stu-id="d0d6e-167">Component disposal</span></span>](#component-disposal)
* [<span data-ttu-id="d0d6e-168">Interopérabilité JavaScript</span><span class="sxs-lookup"><span data-stu-id="d0d6e-168">JavaScript interop</span></span>](#javascript-interop)
* [<span data-ttu-id="d0d6e-169">Blazor Server rerendu</span><span class="sxs-lookup"><span data-stu-id="d0d6e-169">Blazor Server rerendering</span></span>](#blazor-server-prerendering)

<span data-ttu-id="d0d6e-170">Les exceptions non gérées précédentes sont décrites dans les sections suivantes de cet article.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-170">The preceding unhandled exceptions are described in the following sections of this article.</span></span>

### <a name="component-instantiation"></a><span data-ttu-id="d0d6e-171">Instanciation du composant</span><span class="sxs-lookup"><span data-stu-id="d0d6e-171">Component instantiation</span></span>

<span data-ttu-id="d0d6e-172">Lorsque Blazor crée une instance d’un composant :</span><span class="sxs-lookup"><span data-stu-id="d0d6e-172">When Blazor creates an instance of a component:</span></span>

* <span data-ttu-id="d0d6e-173">Le constructeur du composant est appelé.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-173">The component's constructor is invoked.</span></span>
* <span data-ttu-id="d0d6e-174">Les constructeurs de tous les services d’injection de code non Singleton fournis au constructeur du composant via la [`@inject`](xref:mvc/views/razor#inject) directive ou l' [`[Inject]`](xref:blazor/fundamentals/dependency-injection#request-a-service-in-a-component) attribut sont appelés.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-174">The constructors of any non-singleton DI services supplied to the component's constructor via the [`@inject`](xref:mvc/views/razor#inject) directive or the [`[Inject]`](xref:blazor/fundamentals/dependency-injection#request-a-service-in-a-component) attribute are invoked.</span></span>

<span data-ttu-id="d0d6e-175">Un Blazor Server circuit échoue quand un constructeur exécuté ou une méthode setter pour une `[Inject]` propriété lève une exception non gérée.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-175">A Blazor Server circuit fails when any executed constructor or a setter for any `[Inject]` property throws an unhandled exception.</span></span> <span data-ttu-id="d0d6e-176">L’exception est irrécupérable, car l’infrastructure ne peut pas instancier le composant.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-176">The exception is fatal because the framework can't instantiate the component.</span></span> <span data-ttu-id="d0d6e-177">Si la logique du constructeur peut lever des exceptions, l’application doit intercepter les exceptions à l’aide d’une [`try-catch`](/dotnet/csharp/language-reference/keywords/try-catch) instruction avec la gestion des erreurs et la journalisation.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-177">If constructor logic may throw exceptions, the app should trap the exceptions using a [`try-catch`](/dotnet/csharp/language-reference/keywords/try-catch) statement with error handling and logging.</span></span>

### <a name="lifecycle-methods"></a><span data-ttu-id="d0d6e-178">Méthodes de cycle de vie</span><span class="sxs-lookup"><span data-stu-id="d0d6e-178">Lifecycle methods</span></span>

<span data-ttu-id="d0d6e-179">Pendant la durée de vie d’un composant, Blazor appelle les [méthodes de cycle de vie](xref:blazor/components/lifecycle)suivantes :</span><span class="sxs-lookup"><span data-stu-id="d0d6e-179">During the lifetime of a component, Blazor invokes the following [lifecycle methods](xref:blazor/components/lifecycle):</span></span>

* <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized%2A> / <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync%2A>
* <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet%2A> / <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync%2A>
* <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A>
* <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender%2A> / <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync%2A>

<span data-ttu-id="d0d6e-180">Si une méthode de cycle de vie lève une exception, de manière synchrone ou asynchrone, l’exception est irrécupérable pour un Blazor Server circuit.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-180">If any lifecycle method throws an exception, synchronously or asynchronously, the exception is fatal to a Blazor Server circuit.</span></span> <span data-ttu-id="d0d6e-181">Pour les composants qui gèrent les erreurs dans les méthodes de cycle de vie, ajoutez une logique de gestion des erreurs.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-181">For components to deal with errors in lifecycle methods, add error handling logic.</span></span>

<span data-ttu-id="d0d6e-182">Dans l’exemple suivant, où <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync%2A> appelle une méthode pour obtenir un produit :</span><span class="sxs-lookup"><span data-stu-id="d0d6e-182">In the following example where <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync%2A> calls a method to obtain a product:</span></span>

* <span data-ttu-id="d0d6e-183">Une exception levée dans la `ProductRepository.GetProductByIdAsync` méthode est gérée par une [`try-catch`](/dotnet/csharp/language-reference/keywords/try-catch) instruction.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-183">An exception thrown in the `ProductRepository.GetProductByIdAsync` method is handled by a [`try-catch`](/dotnet/csharp/language-reference/keywords/try-catch) statement.</span></span>
* <span data-ttu-id="d0d6e-184">Lorsque le `catch` bloc est exécuté :</span><span class="sxs-lookup"><span data-stu-id="d0d6e-184">When the `catch` block is executed:</span></span>
  * <span data-ttu-id="d0d6e-185">`loadFailed` a la valeur `true` , qui est utilisée pour afficher un message d’erreur à l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-185">`loadFailed` is set to `true`, which is used to display an error message to the user.</span></span>
  * <span data-ttu-id="d0d6e-186">L’erreur est consignée.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-186">The error is logged.</span></span>

[!code-razor[](handle-errors/samples_snapshot/3.x/product-details.razor?highlight=11,27-39)]

### <a name="rendering-logic"></a><span data-ttu-id="d0d6e-187">Logique de rendu</span><span class="sxs-lookup"><span data-stu-id="d0d6e-187">Rendering logic</span></span>

<span data-ttu-id="d0d6e-188">Le balisage déclaratif dans un `.razor` fichier de composant est compilé dans une méthode C# appelée <xref:Microsoft.AspNetCore.Components.ComponentBase.BuildRenderTree%2A> .</span><span class="sxs-lookup"><span data-stu-id="d0d6e-188">The declarative markup in a `.razor` component file is compiled into a C# method called <xref:Microsoft.AspNetCore.Components.ComponentBase.BuildRenderTree%2A>.</span></span> <span data-ttu-id="d0d6e-189">Lorsqu’un composant affiche, <xref:Microsoft.AspNetCore.Components.ComponentBase.BuildRenderTree%2A> exécute et génère une structure de données décrivant les éléments, le texte et les composants enfants du composant rendu.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-189">When a component renders, <xref:Microsoft.AspNetCore.Components.ComponentBase.BuildRenderTree%2A> executes and builds up a data structure describing the elements, text, and child components of the rendered component.</span></span>

<span data-ttu-id="d0d6e-190">La logique de rendu peut lever une exception.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-190">Rendering logic can throw an exception.</span></span> <span data-ttu-id="d0d6e-191">Un exemple de ce scénario se produit lorsque `@someObject.PropertyName` est évalué `@someObject` , mais est `null` .</span><span class="sxs-lookup"><span data-stu-id="d0d6e-191">An example of this scenario occurs when `@someObject.PropertyName` is evaluated but `@someObject` is `null`.</span></span> <span data-ttu-id="d0d6e-192">Une exception non gérée levée par la logique de rendu est irrécupérable pour un Blazor Server circuit.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-192">An unhandled exception thrown by rendering logic is fatal to a Blazor Server circuit.</span></span>

<span data-ttu-id="d0d6e-193">Pour éviter une exception de référence null dans la logique de rendu, recherchez un `null` objet avant d’accéder à ses membres.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-193">To prevent a null reference exception in rendering logic, check for a `null` object before accessing its members.</span></span> <span data-ttu-id="d0d6e-194">Dans l’exemple suivant, les `person.Address` Propriétés ne sont pas accessibles si `person.Address` est `null` :</span><span class="sxs-lookup"><span data-stu-id="d0d6e-194">In the following example, `person.Address` properties aren't accessed if `person.Address` is `null`:</span></span>

[!code-razor[](handle-errors/samples_snapshot/3.x/person-example.razor?highlight=1)]

<span data-ttu-id="d0d6e-195">Le code précédent suppose que `person` ne l’est pas `null` .</span><span class="sxs-lookup"><span data-stu-id="d0d6e-195">The preceding code assumes that `person` isn't `null`.</span></span> <span data-ttu-id="d0d6e-196">Souvent, la structure du code garantit l’existence d’un objet au moment du rendu du composant.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-196">Often, the structure of the code guarantees that an object exists at the time the component is rendered.</span></span> <span data-ttu-id="d0d6e-197">Dans ce cas, il n’est pas nécessaire de rechercher `null` dans la logique de rendu.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-197">In those cases, it isn't necessary to check for `null` in rendering logic.</span></span> <span data-ttu-id="d0d6e-198">Dans l’exemple précédent, `person` peut être garanti qu’il existe, car `person` est créé lorsque le composant est instancié.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-198">In the prior example, `person` might be guaranteed to exist because `person` is created when the component is instantiated.</span></span>

### <a name="event-handlers"></a><span data-ttu-id="d0d6e-199">Gestionnaires d’événements</span><span class="sxs-lookup"><span data-stu-id="d0d6e-199">Event handlers</span></span>

<span data-ttu-id="d0d6e-200">Le code côté client déclenche des appels de code C# lors de la création de gestionnaires d’événements à l’aide de :</span><span class="sxs-lookup"><span data-stu-id="d0d6e-200">Client-side code triggers invocations of C# code when event handlers are created using:</span></span>

* `@onclick`
* `@onchange`
* <span data-ttu-id="d0d6e-201">Autres `@on...` attributs</span><span class="sxs-lookup"><span data-stu-id="d0d6e-201">Other `@on...` attributes</span></span>
* `@bind`

<span data-ttu-id="d0d6e-202">Le code du gestionnaire d’événements peut lever une exception non gérée dans ces scénarios.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-202">Event handler code might throw an unhandled exception in these scenarios.</span></span>

<span data-ttu-id="d0d6e-203">Si un gestionnaire d’événements lève une exception non gérée (par exemple, une requête de base de données échoue), l’exception est irrécupérable pour un Blazor Server circuit.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-203">If an event handler throws an unhandled exception (for example, a database query fails), the exception is fatal to a Blazor Server circuit.</span></span> <span data-ttu-id="d0d6e-204">Si l’application appelle du code qui pourrait échouer pour des raisons externes, interceptez les exceptions à l’aide d’une [`try-catch`](/dotnet/csharp/language-reference/keywords/try-catch) instruction avec gestion des erreurs et journalisation.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-204">If the app calls code that could fail for external reasons, trap exceptions using a [`try-catch`](/dotnet/csharp/language-reference/keywords/try-catch) statement with error handling and logging.</span></span>

<span data-ttu-id="d0d6e-205">Si le code utilisateur n’intercepte pas et ne gère pas l’exception, le Framework journalise l’exception et met fin au circuit.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-205">If user code doesn't trap and handle the exception, the framework logs the exception and terminates the circuit.</span></span>

### <a name="component-disposal"></a><span data-ttu-id="d0d6e-206">Suppression de composants</span><span class="sxs-lookup"><span data-stu-id="d0d6e-206">Component disposal</span></span>

<span data-ttu-id="d0d6e-207">Un composant peut être supprimé de l’interface utilisateur, par exemple, parce que l’utilisateur a accédé à une autre page.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-207">A component may be removed from the UI, for example, because the user has navigated to another page.</span></span> <span data-ttu-id="d0d6e-208">Quand un composant qui implémente <xref:System.IDisposable?displayProperty=fullName> est supprimé de l’interface utilisateur, le Framework appelle la méthode du composant <xref:System.IDisposable.Dispose%2A> .</span><span class="sxs-lookup"><span data-stu-id="d0d6e-208">When a component that implements <xref:System.IDisposable?displayProperty=fullName> is removed from the UI, the framework calls the component's <xref:System.IDisposable.Dispose%2A> method.</span></span>

<span data-ttu-id="d0d6e-209">Si la méthode du composant `Dispose` lève une exception non gérée, l’exception est irrécupérable pour un Blazor Server circuit.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-209">If the component's `Dispose` method throws an unhandled exception, the exception is fatal to a Blazor Server circuit.</span></span> <span data-ttu-id="d0d6e-210">Si la logique de suppression peut lever des exceptions, l’application doit intercepter les exceptions à l’aide d’une [`try-catch`](/dotnet/csharp/language-reference/keywords/try-catch) instruction avec la gestion des erreurs et la journalisation.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-210">If disposal logic may throw exceptions, the app should trap the exceptions using a [`try-catch`](/dotnet/csharp/language-reference/keywords/try-catch) statement with error handling and logging.</span></span>

<span data-ttu-id="d0d6e-211">Pour plus d’informations sur la suppression de composants, consultez <xref:blazor/components/lifecycle#component-disposal-with-idisposable> .</span><span class="sxs-lookup"><span data-stu-id="d0d6e-211">For more information on component disposal, see <xref:blazor/components/lifecycle#component-disposal-with-idisposable>.</span></span>

### <a name="javascript-interop"></a><span data-ttu-id="d0d6e-212">Interopérabilité JavaScript</span><span class="sxs-lookup"><span data-stu-id="d0d6e-212">JavaScript interop</span></span>

<span data-ttu-id="d0d6e-213"><xref:Microsoft.JSInterop.IJSRuntime.InvokeAsync%2A?displayProperty=nameWithType> permet au code .NET d’effectuer des appels asynchrones au runtime JavaScript dans le navigateur de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-213"><xref:Microsoft.JSInterop.IJSRuntime.InvokeAsync%2A?displayProperty=nameWithType> allows .NET code to make asynchronous calls to the JavaScript runtime in the user's browser.</span></span>

<span data-ttu-id="d0d6e-214">Les conditions suivantes s’appliquent à la gestion des erreurs avec <xref:Microsoft.JSInterop.IJSRuntime.InvokeAsync%2A> :</span><span class="sxs-lookup"><span data-stu-id="d0d6e-214">The following conditions apply to error handling with <xref:Microsoft.JSInterop.IJSRuntime.InvokeAsync%2A>:</span></span>

* <span data-ttu-id="d0d6e-215">Si un appel à <xref:Microsoft.JSInterop.IJSRuntime.InvokeAsync%2A> échoue de façon synchrone, une exception .net se produit.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-215">If a call to <xref:Microsoft.JSInterop.IJSRuntime.InvokeAsync%2A> fails synchronously, a .NET exception occurs.</span></span> <span data-ttu-id="d0d6e-216">Un appel à <xref:Microsoft.JSInterop.IJSRuntime.InvokeAsync%2A> peut échouer, par exemple, car les arguments fournis ne peuvent pas être sérialisés.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-216">A call to <xref:Microsoft.JSInterop.IJSRuntime.InvokeAsync%2A> may fail, for example, because the supplied arguments can't be serialized.</span></span> <span data-ttu-id="d0d6e-217">Le code du développeur doit intercepter l’exception.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-217">Developer code must catch the exception.</span></span> <span data-ttu-id="d0d6e-218">Si le code d’application dans une méthode de gestionnaire d’événements ou de cycle de vie de composant ne gère pas une exception, l’exception résultante est irrécupérable pour un Blazor Server circuit.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-218">If app code in an event handler or component lifecycle method doesn't handle an exception, the resulting exception is fatal to a Blazor Server circuit.</span></span>
* <span data-ttu-id="d0d6e-219">Si un appel à <xref:Microsoft.JSInterop.IJSRuntime.InvokeAsync%2A> échoue de manière asynchrone, le .NET <xref:System.Threading.Tasks.Task> échoue.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-219">If a call to <xref:Microsoft.JSInterop.IJSRuntime.InvokeAsync%2A> fails asynchronously, the .NET <xref:System.Threading.Tasks.Task> fails.</span></span> <span data-ttu-id="d0d6e-220">Un appel à <xref:Microsoft.JSInterop.IJSRuntime.InvokeAsync%2A> peut échouer, par exemple, parce que le code côté JavaScript lève une exception ou retourne un `Promise` qui s’est terminé comme `rejected` .</span><span class="sxs-lookup"><span data-stu-id="d0d6e-220">A call to <xref:Microsoft.JSInterop.IJSRuntime.InvokeAsync%2A> may fail, for example, because the JavaScript-side code throws an exception or returns a `Promise` that completed as `rejected`.</span></span> <span data-ttu-id="d0d6e-221">Le code du développeur doit intercepter l’exception.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-221">Developer code must catch the exception.</span></span> <span data-ttu-id="d0d6e-222">Si vous utilisez l' [`await`](/dotnet/csharp/language-reference/keywords/await) opérateur, encapsulez l’appel de méthode dans une [`try-catch`](/dotnet/csharp/language-reference/keywords/try-catch) instruction avec la gestion des erreurs et la journalisation.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-222">If using the [`await`](/dotnet/csharp/language-reference/keywords/await) operator, consider wrapping the method call in a [`try-catch`](/dotnet/csharp/language-reference/keywords/try-catch) statement with error handling and logging.</span></span> <span data-ttu-id="d0d6e-223">Dans le cas contraire, le code défaillant entraîne une exception non gérée qui est irrécupérable pour un Blazor Server circuit.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-223">Otherwise, the failing code results in an unhandled exception that's fatal to a Blazor Server circuit.</span></span>
* <span data-ttu-id="d0d6e-224">Par défaut, les appels à <xref:Microsoft.JSInterop.IJSRuntime.InvokeAsync%2A> doivent se terminer dans un laps de temps donné, sinon l’appel expire. Le délai d’expiration par défaut est d’une minute.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-224">By default, calls to <xref:Microsoft.JSInterop.IJSRuntime.InvokeAsync%2A> must complete within a certain period or else the call times out. The default timeout period is one minute.</span></span> <span data-ttu-id="d0d6e-225">Le délai d’attente protège le code contre toute perte de connectivité réseau ou de code JavaScript qui ne renvoie jamais de message d’achèvement.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-225">The timeout protects the code against a loss in network connectivity or JavaScript code that never sends back a completion message.</span></span> <span data-ttu-id="d0d6e-226">Si l’appel expire, le résultant <xref:System.Threading.Tasks> échoue avec un <xref:System.OperationCanceledException> .</span><span class="sxs-lookup"><span data-stu-id="d0d6e-226">If the call times out, the resulting <xref:System.Threading.Tasks> fails with an <xref:System.OperationCanceledException>.</span></span> <span data-ttu-id="d0d6e-227">Interceptez et traitez l’exception avec la journalisation.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-227">Trap and process the exception with logging.</span></span>

<span data-ttu-id="d0d6e-228">De même, le code JavaScript peut initier des appels à des méthodes .NET indiquées par l' [`[JSInvokable]`](xref:blazor/call-dotnet-from-javascript) attribut.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-228">Similarly, JavaScript code may initiate calls to .NET methods indicated by the [`[JSInvokable]`](xref:blazor/call-dotnet-from-javascript) attribute.</span></span> <span data-ttu-id="d0d6e-229">Si ces méthodes .NET lèvent une exception non gérée :</span><span class="sxs-lookup"><span data-stu-id="d0d6e-229">If these .NET methods throw an unhandled exception:</span></span>

* <span data-ttu-id="d0d6e-230">L’exception n’est pas traitée comme étant irrécupérable pour un Blazor Server circuit.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-230">The exception isn't treated as fatal to a Blazor Server circuit.</span></span>
* <span data-ttu-id="d0d6e-231">Le côté JavaScript `Promise` est rejeté.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-231">The JavaScript-side `Promise` is rejected.</span></span>

<span data-ttu-id="d0d6e-232">Vous avez la possibilité d’utiliser le code de gestion des erreurs côté .NET ou JavaScript de l’appel de méthode.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-232">You have the option of using error handling code on either the .NET side or the JavaScript side of the method call.</span></span>

<span data-ttu-id="d0d6e-233">Pour plus d’informations, consultez les articles suivants :</span><span class="sxs-lookup"><span data-stu-id="d0d6e-233">For more information, see the following articles:</span></span>

* <xref:blazor/call-javascript-from-dotnet>
* <xref:blazor/call-dotnet-from-javascript>

### <a name="no-locblazor-server-prerendering"></a><span data-ttu-id="d0d6e-234">Blazor Server préaffichant</span><span class="sxs-lookup"><span data-stu-id="d0d6e-234">Blazor Server prerendering</span></span>

<span data-ttu-id="d0d6e-235">Blazor les composants peuvent être prérendus à l’aide du [tag Helper du composant](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper) , afin que le balisage HTML rendu soit renvoyé dans le cadre de la requête http initiale de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-235">Blazor components can be prerendered using the [Component Tag Helper](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper) so that their rendered HTML markup is returned as part of the user's initial HTTP request.</span></span> <span data-ttu-id="d0d6e-236">Cela fonctionne de la façon suivante :</span><span class="sxs-lookup"><span data-stu-id="d0d6e-236">This works by:</span></span>

* <span data-ttu-id="d0d6e-237">Création d’un nouveau circuit pour tous les composants prérendus qui font partie de la même page.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-237">Creating a new circuit for all of the prerendered components that are part of the same page.</span></span>
* <span data-ttu-id="d0d6e-238">Génération du code HTML initial.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-238">Generating the initial HTML.</span></span>
* <span data-ttu-id="d0d6e-239">Traitement du circuit comme `disconnected` jusqu’à ce que le navigateur de l’utilisateur établisse une SignalR connexion avec le même serveur.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-239">Treating the circuit as `disconnected` until the user's browser establishes a SignalR connection back to the same server.</span></span> <span data-ttu-id="d0d6e-240">Lorsque la connexion est établie, l’interactivité sur le circuit est reprise et le balisage HTML des composants est mis à jour.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-240">When the connection is established, interactivity on the circuit is resumed and the components' HTML markup is updated.</span></span>

<span data-ttu-id="d0d6e-241">Si un composant lève une exception non gérée pendant le prérendu, par exemple, pendant une méthode de cycle de vie ou dans une logique de rendu :</span><span class="sxs-lookup"><span data-stu-id="d0d6e-241">If any component throws an unhandled exception during prerendering, for example, during a lifecycle method or in rendering logic:</span></span>

* <span data-ttu-id="d0d6e-242">L’exception est irrécupérable pour le circuit.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-242">The exception is fatal to the circuit.</span></span>
* <span data-ttu-id="d0d6e-243">L’exception est levée dans la pile des appels à partir du <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper> tag Helper.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-243">The exception is thrown up the call stack from the <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper> Tag Helper.</span></span> <span data-ttu-id="d0d6e-244">Par conséquent, la requête HTTP entière échoue, sauf si l’exception est explicitement interceptée par le code du développeur.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-244">Therefore, the entire HTTP request fails unless the exception is explicitly caught by developer code.</span></span>

<span data-ttu-id="d0d6e-245">Dans des circonstances normales, lorsque le prérendu échoue, la création et le rendu du composant n’ont pas de sens, car un composant de travail ne peut pas être rendu.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-245">Under normal circumstances when prerendering fails, continuing to build and render the component doesn't make sense because a working component can't be rendered.</span></span>

<span data-ttu-id="d0d6e-246">Pour tolérer les erreurs qui peuvent se produire pendant le prérendu, la logique de gestion des erreurs doit être placée à l’intérieur d’un composant qui peut lever des exceptions.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-246">To tolerate errors that may occur during prerendering, error handling logic must be placed inside a component that may throw exceptions.</span></span> <span data-ttu-id="d0d6e-247">Utilisez des [`try-catch`](/dotnet/csharp/language-reference/keywords/try-catch) instructions avec la gestion des erreurs et la journalisation.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-247">Use [`try-catch`](/dotnet/csharp/language-reference/keywords/try-catch) statements with error handling and logging.</span></span> <span data-ttu-id="d0d6e-248">Au lieu d’encapsuler le <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper> tag Helper dans une [`try-catch`](/dotnet/csharp/language-reference/keywords/try-catch) instruction, placez la logique de gestion des erreurs dans le composant rendu par le <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper> tag Helper.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-248">Instead of wrapping the <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper> Tag Helper in a [`try-catch`](/dotnet/csharp/language-reference/keywords/try-catch) statement, place error handling logic in the component rendered by the <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper> Tag Helper.</span></span>

## <a name="advanced-scenarios"></a><span data-ttu-id="d0d6e-249">Scénarios avancés</span><span class="sxs-lookup"><span data-stu-id="d0d6e-249">Advanced scenarios</span></span>

### <a name="recursive-rendering"></a><span data-ttu-id="d0d6e-250">Rendu récursif</span><span class="sxs-lookup"><span data-stu-id="d0d6e-250">Recursive rendering</span></span>

<span data-ttu-id="d0d6e-251">Les composants peuvent être imbriqués de manière récursive.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-251">Components can be nested recursively.</span></span> <span data-ttu-id="d0d6e-252">Cela est utile pour représenter des structures de données récursives.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-252">This is useful for representing recursive data structures.</span></span> <span data-ttu-id="d0d6e-253">Par exemple, un `TreeNode` composant peut restituer plus de `TreeNode` composants pour chacun des enfants du nœud.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-253">For example, a `TreeNode` component can render more `TreeNode` components for each of the node's children.</span></span>

<span data-ttu-id="d0d6e-254">Lors du rendu de manière récursive, évitez les modèles de codage qui se traduisent par une récurrence infinie :</span><span class="sxs-lookup"><span data-stu-id="d0d6e-254">When rendering recursively, avoid coding patterns that result in infinite recursion:</span></span>

* <span data-ttu-id="d0d6e-255">Ne rendez pas de manière récursive une structure de données qui contient un cycle.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-255">Don't recursively render a data structure that contains a cycle.</span></span> <span data-ttu-id="d0d6e-256">Par exemple, n’affichez pas un nœud d’arbre dont les enfants s’y trouvent.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-256">For example, don't render a tree node whose children includes itself.</span></span>
* <span data-ttu-id="d0d6e-257">Ne créez pas une chaîne de dispositions qui contiennent un cycle.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-257">Don't create a chain of layouts that contain a cycle.</span></span> <span data-ttu-id="d0d6e-258">Par exemple, ne créez pas une disposition dont la disposition est elle-même.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-258">For example, don't create a layout whose layout is itself.</span></span>
* <span data-ttu-id="d0d6e-259">N’autorisez pas un utilisateur final à enfreindre les invariants de récurrence (règles) par le biais d’une entrée de données malveillante ou d’appels Interop JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-259">Don't allow an end user to violate recursion invariants (rules) through malicious data entry or JavaScript interop calls.</span></span>

<span data-ttu-id="d0d6e-260">Boucles infinies pendant le rendu :</span><span class="sxs-lookup"><span data-stu-id="d0d6e-260">Infinite loops during rendering:</span></span>

* <span data-ttu-id="d0d6e-261">Fait en sorte que le processus de rendu continue de façon permanente.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-261">Causes the rendering process to continue forever.</span></span>
* <span data-ttu-id="d0d6e-262">Équivaut à créer une boucle non terminée.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-262">Is equivalent to creating an unterminated loop.</span></span>

<span data-ttu-id="d0d6e-263">Dans ces scénarios, un Blazor Server circuit affecté échoue et le thread tente généralement d’effectuer les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="d0d6e-263">In these scenarios, an affected Blazor Server circuit fails, and the thread usually attempts to:</span></span>

* <span data-ttu-id="d0d6e-264">Consommez le plus de temps processeur autorisé par le système d’exploitation, indéfiniment.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-264">Consume as much CPU time as permitted by the operating system, indefinitely.</span></span>
* <span data-ttu-id="d0d6e-265">Consommez une quantité illimitée de mémoire serveur.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-265">Consume an unlimited amount of server memory.</span></span> <span data-ttu-id="d0d6e-266">La consommation de mémoire illimitée est équivalente au scénario dans lequel une boucle non terminée ajoute des entrées à une collection à chaque itération.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-266">Consuming unlimited memory is equivalent to the scenario where an unterminated loop adds entries to a collection on every iteration.</span></span>

<span data-ttu-id="d0d6e-267">Pour éviter les modèles de récurrence infinis, assurez-vous que le code de rendu récursif contient des conditions d’arrêt appropriées.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-267">To avoid infinite recursion patterns, ensure that recursive rendering code contains suitable stopping conditions.</span></span>

### <a name="custom-render-tree-logic"></a><span data-ttu-id="d0d6e-268">Logique d’arborescence de rendu personnalisé</span><span class="sxs-lookup"><span data-stu-id="d0d6e-268">Custom render tree logic</span></span>

<span data-ttu-id="d0d6e-269">La plupart des Blazor composants sont implémentés en tant que `.razor` fichiers et sont compilés pour produire une logique qui opère sur un <xref:Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder> pour afficher leur sortie.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-269">Most Blazor components are implemented as `.razor` files and are compiled to produce logic that operates on a <xref:Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder> to render their output.</span></span> <span data-ttu-id="d0d6e-270">Un développeur peut implémenter la <xref:Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder> logique manuellement à l’aide du code C# procédural.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-270">A developer may manually implement <xref:Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder> logic using procedural C# code.</span></span> <span data-ttu-id="d0d6e-271">Pour plus d’informations, consultez <xref:blazor/advanced-scenarios#manual-rendertreebuilder-logic>.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-271">For more information, see <xref:blazor/advanced-scenarios#manual-rendertreebuilder-logic>.</span></span>

> [!WARNING]
> <span data-ttu-id="d0d6e-272">L’utilisation de la logique du générateur d’arborescence de rendu manuel est considérée comme un scénario avancé et risqué, non recommandé pour le développement de composants généraux.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-272">Use of manual render tree builder logic is considered an advanced and unsafe scenario, not recommended for general component development.</span></span>

<span data-ttu-id="d0d6e-273">Si le <xref:Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder> code est écrit, le développeur doit garantir l’exactitude du code.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-273">If <xref:Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder> code is written, the developer must guarantee the correctness of the code.</span></span> <span data-ttu-id="d0d6e-274">Par exemple, le développeur doit s’assurer que :</span><span class="sxs-lookup"><span data-stu-id="d0d6e-274">For example, the developer must ensure that:</span></span>

* <span data-ttu-id="d0d6e-275">Les appels à <xref:Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder.OpenElement%2A> et <xref:Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder.CloseElement%2A> sont correctement équilibrés.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-275">Calls to <xref:Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder.OpenElement%2A> and <xref:Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder.CloseElement%2A> are correctly balanced.</span></span>
* <span data-ttu-id="d0d6e-276">Les attributs sont ajoutés uniquement aux emplacements appropriés.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-276">Attributes are only added in the correct places.</span></span>

<span data-ttu-id="d0d6e-277">Une logique incorrecte du générateur d’arborescence de rendu manuel peut entraîner un comportement arbitraire non défini, y compris des pannes, des blocages du serveur et des failles de sécurité.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-277">Incorrect manual render tree builder logic can cause arbitrary undefined behavior, including crashes, server hangs, and security vulnerabilities.</span></span>

<span data-ttu-id="d0d6e-278">Envisagez une logique de générateur d’arborescence de rendu manuel sur le même niveau de complexité et avec le même niveau de *danger* que l’écriture de code assembleur ou d’instructions MSIL à la main.</span><span class="sxs-lookup"><span data-stu-id="d0d6e-278">Consider manual render tree builder logic on the same level of complexity and with the same level of *danger* as writing assembly code or MSIL instructions by hand.</span></span>
