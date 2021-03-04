---
title: Gérer les erreurs dans les Blazor applications ASP.net Core
author: guardrex
description: Découvrez comment ASP.NET Core Blazor comment Blazor gère les exceptions non gérées et comment développer des applications qui détectent et gèrent les erreurs.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/25/2021
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
zone_pivot_groups: blazor-hosting-models
ms.openlocfilehash: 96f4d7fcacf1f8eb03ffe83ba18b353e5588448e
ms.sourcegitcommit: a1db01b4d3bd8c57d7a9c94ce122a6db68002d66
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102109704"
---
# <a name="handle-errors-in-aspnet-core-blazor-apps"></a>Gérer les erreurs dans les Blazor applications ASP.net Core

Cet article explique comment Blazor gère les exceptions non gérées et comment développer des applications qui détectent et gèrent les erreurs.

::: zone pivot="webassembly"

## <a name="detailed-errors-during-development"></a>Erreurs détaillées pendant le développement

Quand une Blazor application ne fonctionne pas correctement pendant le développement, la réception d’informations d’erreur détaillées de l’application vous aide à résoudre les problèmes et à résoudre le problème. Lorsqu’une erreur se produit, Blazor les applications affichent une barre jaune clair au bas de l’écran :

* Pendant le développement, la barre vous dirige vers la console du navigateur, où vous pouvez voir l’exception.
* En production, la barre avertit l’utilisateur qu’une erreur s’est produite et recommande l’actualisation du navigateur.

L’interface utilisateur de cette expérience de gestion des erreurs fait partie des Blazor modèles de projet.

Dans une Blazor WebAssembly application, personnalisez l’expérience dans le `wwwroot/index.html` fichier :

```html
<div id="blazor-error-ui">
    An unhandled error has occurred.
    <a href="" class="reload">Reload</a>
    <a class="dismiss">🗙</a>
</div>
```

L' `blazor-error-ui` élément est normalement masqué en raison de la présence du `display: none` style de la `blazor-error-ui` classe CSS dans la feuille de style de l’application ( `wwwroot/css/app.css` ). Lorsqu’une erreur se produit, l’infrastructure s’applique `display: block` à l’élément.

## <a name="manage-unhandled-exceptions-in-developer-code"></a>Gérer les exceptions non gérées dans le code du développeur

Pour qu’une application continue après une erreur, l’application doit avoir une logique de gestion des erreurs. Les sections suivantes de cet article décrivent les sources potentielles d’exceptions non gérées.

En production, ne rendez pas les messages d’exception d’infrastructure ou les traces de pile dans l’interface utilisateur. Le rendu des messages d’exception ou des traces de pile peut :

* Divulguer des informations sensibles aux utilisateurs finaux.
* Aidez un utilisateur malveillant à découvrir des faiblesses dans une application qui peuvent compromettre la sécurité de l’application, du serveur ou du réseau.

## <a name="global-exception-handling"></a>Gestion globale des exceptions

[!INCLUDE[](~/blazor/includes/handle-errors/global-exception-handling.md)]

## <a name="log-errors-with-a-persistent-provider"></a>Consigner les erreurs avec un fournisseur persistant

Si une exception non gérée se produit, l’exception est consignée dans <xref:Microsoft.Extensions.Logging.ILogger> les instances configurées dans le conteneur de service. Par défaut, Blazor les applications se connectent à la sortie de la console avec le fournisseur d’informations de journalisation de la console. Envisagez de vous connecter à un emplacement plus permanent sur le serveur en envoyant les informations d’erreur à une API Web principale qui utilise un fournisseur de journalisation avec la gestion de la taille du journal et la rotation des journaux. L’application API Web principale peut également utiliser un service de gestion des performances des applications (APM), tel que [Azure application Insights (Azure Monitor) &dagger; ](/azure/azure-monitor/app/app-insights-overview), pour enregistrer les informations d’erreur qu’elle reçoit des clients.

Vous devez choisir les incidents à enregistrer et le niveau de gravité des incidents journalisés. Les utilisateurs hostiles peuvent être en mesure de déclencher délibérément des erreurs. Par exemple, ne consignez pas un incident à partir d’une erreur où un inconnu `ProductId` est fourni dans l’URL d’un composant qui affiche les détails du produit. Toutes les erreurs ne doivent pas être traitées comme des incidents pour la journalisation.

Pour plus d’informations, consultez les articles suivants :

* <xref:blazor/fundamentals/logging>
* <xref:fundamentals/error-handling>&Dagger;
* <xref:web-api/index>

&dagger;Les fonctionnalités [application Insights](/azure/azure-monitor/app/app-insights-overview) natives pour la prise en charge des Blazor WebAssembly applications et de la Blazor prise en charge native de [Google Analytics](https://analytics.google.com/analytics/web/) peuvent devenir disponibles dans les versions ultérieures de ces technologies. Pour plus d’informations, consultez [prise en charge d’application Insights dans Blazor WASM client (Microsoft/ApplicationInsights-dotnet #2143)](https://github.com/microsoft/ApplicationInsights-dotnet/issues/2143) et [Web Analytics et diagnostics (y compris des liens vers les implémentations de la Communauté) (dotnet/aspnetcore #5461)](https://github.com/dotnet/aspnetcore/issues/5461). En attendant, une application côté client Blazor WebAssembly peut utiliser le kit de [développement logiciel (SDK) JavaScript application Insights](/azure/azure-monitor/app/javascript) avec l' [interopérabilité de JS](xref:blazor/call-javascript-from-dotnet) pour consigner les erreurs directement sur application Insights à partir d’une application côté client.

&Dagger;S’applique aux applications ASP.NET Core côté serveur qui sont des applications principales d’API Web pour les Blazor applications. Les applications côté client interceptent et envoient des informations sur les erreurs à une API Web, qui enregistre les informations d’erreur dans un fournisseur de journalisation persistant.

## <a name="places-where-errors-may-occur"></a>Emplacements où des erreurs peuvent se produire

Le code d’infrastructure et d’application peut déclencher des exceptions non prises en charge dans l’un des emplacements suivants, décrits plus en détail dans les sections suivantes de cet article :

* [Instanciation du composant](#component-instantiation-webassembly)
* [Méthodes de cycle de vie](#lifecycle-methods-webassembly)
* [Logique de rendu](#rendering-logic-webassembly)
* [Gestionnaires d’événements](#event-handlers-webassembly)
* [Suppression de composants](#component-disposal-webassembly)
* [Interopérabilité JavaScript](#javascript-interop-webassembly)

<h3 id="component-instantiation-webassembly">Instanciation du composant</h3>

Lorsque Blazor crée une instance d’un composant :

* Le constructeur du composant est appelé.
* Les constructeurs de tous les services d’injection de code non Singleton fournis au constructeur du composant via la [`@inject`](xref:mvc/views/razor#inject) directive ou l' [ `[Inject]` attribut](xref:blazor/fundamentals/dependency-injection#request-a-service-in-a-component) sont appelés.

Une erreur dans un constructeur exécuté ou un accesseur Set pour une `[Inject]` propriété entraîne une exception non gérée et arrête l’infrastructure de l’instanciation du composant. Si la logique du constructeur peut lever des exceptions, l’application doit intercepter les exceptions à l’aide d’une [`try-catch`](/dotnet/csharp/language-reference/keywords/try-catch) instruction avec la gestion des erreurs et la journalisation.

<h3 id="lifecycle-methods-webassembly">Méthodes de cycle de vie</h3>

Pendant la durée de vie d’un composant, Blazor appelle des [méthodes de cycle de vie](xref:blazor/components/lifecycle#lifecycle-methods). Pour les composants qui gèrent les erreurs dans les méthodes de cycle de vie, ajoutez une logique de gestion des erreurs.

Dans l’exemple suivant, où <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync%2A> appelle une méthode pour obtenir un produit :

* Une exception levée dans la `ProductRepository.GetProductByIdAsync` méthode est gérée par une [`try-catch`](/dotnet/csharp/language-reference/keywords/try-catch) instruction.
* Lorsque le `catch` bloc est exécuté :
  * `loadFailed` a la valeur `true` , qui est utilisée pour afficher un message d’erreur à l’utilisateur.
  * L’erreur est consignée.

::: moniker range=">= aspnetcore-5.0"

[!code-razor[](~/blazor/common/samples/5.x/BlazorSample_WebAssembly/Pages/handle-errors/ProductDetails.razor?name=snippet&highlight=11,27-39)]

::: moniker-end

::: moniker range="< aspnetcore-5.0"

[!code-razor[](~/blazor/common/samples/3.x/BlazorSample_WebAssembly/Pages/handle-errors/ProductDetails.razor?name=snippet&highlight=11,27-39)]

::: moniker-end

<h3 id="rendering-logic-webassembly">Logique de rendu</h3>

Le balisage déclaratif dans un Razor fichier de composant ( `.razor` ) est compilé dans une méthode C# appelée <xref:Microsoft.AspNetCore.Components.ComponentBase.BuildRenderTree%2A> . Lorsqu’un composant affiche, <xref:Microsoft.AspNetCore.Components.ComponentBase.BuildRenderTree%2A> exécute et génère une structure de données décrivant les éléments, le texte et les composants enfants du composant rendu.

La logique de rendu peut lever une exception. Un exemple de ce scénario se produit lorsque `@someObject.PropertyName` est évalué `@someObject` , mais est `null` .

Pour empêcher une <xref:System.NullReferenceException> dans la logique de rendu, recherchez un `null` objet avant d’accéder à ses membres. Dans l’exemple suivant, les `person.Address` Propriétés ne sont pas accessibles si `person.Address` est `null` :

::: moniker range=">= aspnetcore-5.0"

[!code-razor[](~/blazor/common/samples/5.x/BlazorSample_WebAssembly/Pages/handle-errors/PersonExample.razor?name=snippet&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-5.0"

[!code-razor[](~/blazor/common/samples/3.x/BlazorSample_WebAssembly/Pages/handle-errors/PersonExample.razor?name=snippet&highlight=1)]

::: moniker-end

Le code précédent suppose que `person` ne l’est pas `null` . Souvent, la structure du code garantit l’existence d’un objet au moment du rendu du composant. Dans ce cas, il n’est pas nécessaire de rechercher `null` dans la logique de rendu. Dans l’exemple précédent, `person` peut être garanti qu’il existe, car `person` est créé lors de l’instanciation du composant, comme le montre l’exemple suivant :

```razor
@code {
    private Person person = new Person();

    ...
}
```

<h3 id="event-handlers-webassembly">Gestionnaires d’événements</h3>

Le code côté client déclenche des appels de code C# lors de la création de gestionnaires d’événements à l’aide de :

* `@onclick`
* `@onchange`
* Autres `@on...` attributs
* `@bind`

Le code du gestionnaire d’événements peut lever une exception non gérée dans ces scénarios.

Si l’application appelle du code qui pourrait échouer pour des raisons externes, interceptez les exceptions à l’aide d’une [`try-catch`](/dotnet/csharp/language-reference/keywords/try-catch) instruction avec gestion des erreurs et journalisation.

Si le code utilisateur n’intercepte pas et ne gère pas l’exception, le Framework journalise l’exception.

<h3 id="component-disposal-webassembly">Suppression de composants</h3>

Un composant peut être supprimé de l’interface utilisateur, par exemple, parce que l’utilisateur a accédé à une autre page. Quand un composant qui implémente <xref:System.IDisposable?displayProperty=fullName> est supprimé de l’interface utilisateur, le Framework appelle la méthode du composant <xref:System.IDisposable.Dispose%2A> .

Si la logique de suppression peut lever des exceptions, l’application doit intercepter les exceptions à l’aide d’une [`try-catch`](/dotnet/csharp/language-reference/keywords/try-catch) instruction avec la gestion des erreurs et la journalisation.

Pour plus d’informations sur la suppression de composants, consultez <xref:blazor/components/lifecycle#component-disposal-with-idisposable> .

<h3 id="javascript-interop-webassembly">Interopérabilité JavaScript</h3>

<xref:Microsoft.JSInterop.IJSRuntime.InvokeAsync%2A?displayProperty=nameWithType> permet au code .NET d’effectuer des appels asynchrones au runtime JavaScript dans le navigateur de l’utilisateur.

Les conditions suivantes s’appliquent à la gestion des erreurs avec <xref:Microsoft.JSInterop.IJSRuntime.InvokeAsync%2A> :

* Si un appel à <xref:Microsoft.JSInterop.IJSRuntime.InvokeAsync%2A> échoue de façon synchrone, une exception .net se produit. Un appel à <xref:Microsoft.JSInterop.IJSRuntime.InvokeAsync%2A> peut échouer, par exemple, car les arguments fournis ne peuvent pas être sérialisés. Le code du développeur doit intercepter l’exception.
* Si un appel à <xref:Microsoft.JSInterop.IJSRuntime.InvokeAsync%2A> échoue de manière asynchrone, le .NET <xref:System.Threading.Tasks.Task> échoue. Un appel à <xref:Microsoft.JSInterop.IJSRuntime.InvokeAsync%2A> peut échouer, par exemple, parce que le code côté JavaScript lève une exception ou retourne un `Promise` qui s’est terminé comme `rejected` . Le code du développeur doit intercepter l’exception. Si vous utilisez l' [`await`](/dotnet/csharp/language-reference/keywords/await) opérateur, encapsulez l’appel de méthode dans une [`try-catch`](/dotnet/csharp/language-reference/keywords/try-catch) instruction avec la gestion des erreurs et la journalisation.
* Par défaut, les appels à <xref:Microsoft.JSInterop.IJSRuntime.InvokeAsync%2A> doivent se terminer dans un laps de temps donné, sinon l’appel expire. Le délai d’expiration par défaut est d’une minute. Le délai d’attente protège le code contre toute perte de connectivité réseau ou de code JavaScript qui ne renvoie jamais de message d’achèvement. Si l’appel expire, le résultant <xref:System.Threading.Tasks> échoue avec un <xref:System.OperationCanceledException> . Interceptez et traitez l’exception avec la journalisation.

De même, le code JavaScript peut initier des appels à des méthodes .NET indiquées par l' [ `[JSInvokable]` attribut](xref:blazor/call-dotnet-from-javascript). Si ces méthodes .NET lèvent une exception non gérée, le côté JavaScript `Promise` est rejeté.

Vous avez la possibilité d’utiliser le code de gestion des erreurs côté .NET ou JavaScript de l’appel de méthode.

Pour plus d’informations, consultez les articles suivants :

* <xref:blazor/call-javascript-from-dotnet>
* <xref:blazor/call-dotnet-from-javascript>

## <a name="advanced-scenarios"></a>Scénarios avancés

### <a name="recursive-rendering"></a>Rendu récursif

Les composants peuvent être imbriqués de manière récursive. Cela est utile pour représenter des structures de données récursives. Par exemple, un `TreeNode` composant peut restituer plus de `TreeNode` composants pour chacun des enfants du nœud.

Lors du rendu de manière récursive, évitez les modèles de codage qui se traduisent par une récurrence infinie :

* Ne rendez pas de manière récursive une structure de données qui contient un cycle. Par exemple, n’affichez pas un nœud d’arbre dont les enfants s’y trouvent.
* Ne créez pas une chaîne de dispositions qui contiennent un cycle. Par exemple, ne créez pas une disposition dont la disposition est elle-même.
* N’autorisez pas un utilisateur final à enfreindre les invariants de récurrence (règles) par le biais d’une entrée de données malveillante ou d’appels Interop JavaScript.

Boucles infinies pendant le rendu :

* Fait en sorte que le processus de rendu continue de façon permanente.
* Équivaut à créer une boucle non terminée.

Dans ces scénarios, le thread tente généralement d’effectuer les opérations suivantes :

* Consommez le plus de temps processeur autorisé par le système d’exploitation, indéfiniment.
* Consommez une quantité illimitée de mémoire client. La consommation de mémoire illimitée est équivalente au scénario dans lequel une boucle non terminée ajoute des entrées à une collection à chaque itération.

Pour éviter les modèles de récurrence infinis, assurez-vous que le code de rendu récursif contient des conditions d’arrêt appropriées.

### <a name="custom-render-tree-logic"></a>Logique d’arborescence de rendu personnalisé

La plupart des Blazor composants sont implémentés en tant que Razor fichiers de composants ( `.razor` ) et sont compilés par l’infrastructure pour produire une logique qui opère sur un <xref:Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder> pour afficher leur sortie. Toutefois, un développeur peut implémenter la <xref:Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder> logique manuellement à l’aide du code C# procédural. Pour plus d’informations, consultez <xref:blazor/advanced-scenarios#manual-rendertreebuilder-logic>.

> [!WARNING]
> L’utilisation de la logique du générateur d’arborescence de rendu manuel est considérée comme un scénario avancé et risqué, non recommandé pour le développement de composants généraux.

Si le <xref:Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder> code est écrit, le développeur doit garantir l’exactitude du code. Par exemple, le développeur doit s’assurer que :

* Les appels à <xref:Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder.OpenElement%2A> et <xref:Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder.CloseElement%2A> sont correctement équilibrés.
* Les attributs sont ajoutés uniquement aux emplacements appropriés.

Une logique de générateur d’arborescence de rendu manuel incorrecte peut entraîner un comportement arbitraire indéfini, y compris des incidents, des blocages d’application et des failles de sécurité.

Considérez la logique du générateur d’arborescence de rendu manuel sur le même niveau de complexité et avec le même niveau de *danger* que l’écriture de code assembleur ou d’instructions [MSIL (Microsoft Intermediate Language)](/dotnet/standard/managed-code) manuellement.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:blazor/fundamentals/logging>
* <xref:fundamentals/error-handling>&dagger;
* <xref:web-api/index>

&dagger;S’applique au serveur principal ASP.NET Core applications API Web que Blazor WebAssembly les applications côté client utilisent pour la journalisation.

::: zone-end

::: zone pivot="server"

## <a name="detailed-errors-during-development"></a>Erreurs détaillées pendant le développement

Quand une Blazor application ne fonctionne pas correctement pendant le développement, la réception d’informations d’erreur détaillées de l’application vous aide à résoudre les problèmes et à résoudre le problème. Lorsqu’une erreur se produit, Blazor les applications affichent une barre jaune clair au bas de l’écran :

* Pendant le développement, la barre vous dirige vers la console du navigateur, où vous pouvez voir l’exception.
* En production, la barre avertit l’utilisateur qu’une erreur s’est produite et recommande l’actualisation du navigateur.

L’interface utilisateur de cette expérience de gestion des erreurs fait partie des Blazor modèles de projet.

Dans une Blazor Server application, personnalisez l’expérience dans le `Pages/_Host.cshtml` fichier :

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

L' `blazor-error-ui` élément est normalement masqué en raison de la présence du `display: none` style de la `blazor-error-ui` classe CSS dans la feuille de style du site ( `wwwroot/css/site.css` ). Lorsqu’une erreur se produit, l’infrastructure s’applique `display: block` à l’élément.

## <a name="blazor-server-detailed-circuit-errors"></a>Blazor Server Erreurs de circuit détaillées

Les erreurs côté client n’incluent pas la pile des appels et ne fournissent pas de détails sur la cause de l’erreur, mais les journaux du serveur contiennent ces informations. À des fins de développement, des informations sur les erreurs de circuit sensible peuvent être mises à la disposition du client en activant des erreurs détaillées.

Définissez <xref:Microsoft.AspNetCore.Components.Server.CircuitOptions.DetailedErrors?displayProperty=nameWithType> sur `true`. Pour plus d'informations et pour obtenir un exemple, consultez <xref:blazor/fundamentals/signalr#circuit-handler-options>.

Une alternative à la définition de <xref:Microsoft.AspNetCore.Components.Server.CircuitOptions.DetailedErrors?displayProperty=nameWithType> est de définir la `DetailedErrors` clé de configuration sur `true` dans le fichier de paramètres de l’environnement de développement de l’application ( `appsettings.Development.json` ).  En outre, définissez la [ SignalR journalisation côté serveur](xref:signalr/diagnostics#server-side-logging) ( `Microsoft.AspNetCore.SignalR` ) à [Déboguer](xref:Microsoft.Extensions.Logging.LogLevel) ou à [suivre](xref:Microsoft.Extensions.Logging.LogLevel) pour la SignalR journalisation détaillée.

`appsettings.Development.json`:

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

La <xref:Microsoft.AspNetCore.Components.Server.CircuitOptions.DetailedErrors> clé de configuration peut également être définie pour `true` utiliser la `ASPNETCORE_DETAILEDERRORS` variable d’environnement avec la valeur sur les serveurs d’environnement de `true` développement/intermédiaire ou sur votre système local.

> [!WARNING]
> Évitez toujours d’exposer les informations d’erreur aux clients sur Internet, ce qui constitue un risque pour la sécurité.

## <a name="how-a-blazor-server-app-reacts-to-unhandled-exceptions"></a>Comment une Blazor Server application réagit aux exceptions non gérées

Blazor Server est une infrastructure avec état. Tandis que les utilisateurs interagissent avec une application, ils maintiennent une connexion au serveur appelé « *circuit*». Le circuit contient des instances de composant actives, ainsi que de nombreux autres aspects de l’État, tels que :

* Sortie du rendu le plus récent des composants.
* Ensemble actuel de délégués de gestion d’événements qui peuvent être déclenchés par les événements côté client.

Si un utilisateur ouvre l’application dans plusieurs onglets de navigateur, il crée plusieurs circuits indépendants.

Blazor traite la plupart des exceptions non gérées comme étant irrécupérables par le circuit dans lequel elles se produisent. Si un circuit est arrêté en raison d’une exception non gérée, l’utilisateur ne peut continuer à interagir avec l’application qu’en rechargeant la page pour créer un nouveau circuit. Les circuits en dehors de celui qui est terminé, qui sont des circuits pour d’autres utilisateurs ou d’autres onglets de navigateur, ne sont pas affectés. Ce scénario est similaire à une application de bureau qui se bloque. L’application bloquée doit être redémarrée, mais les autres applications ne sont pas affectées.

Le Framework met fin à un circuit lorsqu’une exception non gérée se produit pour les raisons suivantes :

* Une exception non gérée rend souvent le circuit dans un État indéfini.
* L’opération normale de l’application ne peut pas être garantie après une exception non gérée.
* Des failles de sécurité peuvent apparaître dans l’application si le circuit continue dans un État indéfini.

## <a name="manage-unhandled-exceptions-in-developer-code"></a>Gérer les exceptions non gérées dans le code du développeur

Pour qu’une application continue après une erreur, l’application doit avoir une logique de gestion des erreurs. Les sections suivantes de cet article décrivent les sources potentielles d’exceptions non gérées.

En production, ne rendez pas les messages d’exception d’infrastructure ou les traces de pile dans l’interface utilisateur. Le rendu des messages d’exception ou des traces de pile peut :

* Divulguer des informations sensibles aux utilisateurs finaux.
* Aidez un utilisateur malveillant à découvrir des faiblesses dans une application qui peuvent compromettre la sécurité de l’application, du serveur ou du réseau.

## <a name="global-exception-handling"></a>Gestion globale des exceptions

[!INCLUDE[](~/blazor/includes/handle-errors/global-exception-handling.md)]

Étant donné que les approches de cette section gèrent les erreurs avec une [`try-catch`](/dotnet/csharp/language-reference/keywords/try-catch) instruction, la SignalR connexion entre le client et le serveur n’est pas interrompue lorsqu’une erreur se produit et que le circuit reste actif. Toute exception non gérée est irrémédiable pour un circuit. Pour plus d’informations, consultez la section précédente sur [la manière dont une Blazor Server application réagit aux exceptions non gérées](#how-a-blazor-server-app-reacts-to-unhandled-exceptions).

## <a name="log-errors-with-a-persistent-provider"></a>Consigner les erreurs avec un fournisseur persistant

Si une exception non gérée se produit, l’exception est consignée dans <xref:Microsoft.Extensions.Logging.ILogger> les instances configurées dans le conteneur de service. Par défaut, Blazor les applications se connectent à la sortie de la console avec le fournisseur d’informations de journalisation de la console. Envisagez de vous connecter à un emplacement plus permanent sur le serveur avec un fournisseur qui gère la taille du journal et la rotation des journaux. L’application peut également utiliser un service de gestion des performances des applications (APM), tel que [Azure application Insights (Azure Monitor)](/azure/azure-monitor/app/app-insights-overview).

Pendant le développement, une Blazor Server application envoie généralement les détails complets des exceptions à la console du navigateur pour faciliter le débogage. En production, les erreurs détaillées ne sont pas envoyées aux clients, mais les détails complets d’une exception sont enregistrés sur le serveur.

Vous devez choisir les incidents à enregistrer et le niveau de gravité des incidents journalisés. Les utilisateurs hostiles peuvent être en mesure de déclencher délibérément des erreurs. Par exemple, ne consignez pas un incident à partir d’une erreur où un inconnu `ProductId` est fourni dans l’URL d’un composant qui affiche les détails du produit. Toutes les erreurs ne doivent pas être traitées comme des incidents pour la journalisation.

Pour plus d’informations, consultez les articles suivants :

* <xref:blazor/fundamentals/logging>
* <xref:fundamentals/error-handling>&dagger;

&dagger;S’applique aux applications ASP.NET Core côté serveur qui sont des applications principales d’API Web pour les Blazor applications.

## <a name="places-where-errors-may-occur"></a>Emplacements où des erreurs peuvent se produire

Le code d’infrastructure et d’application peut déclencher des exceptions non prises en charge dans l’un des emplacements suivants, décrits plus en détail dans les sections suivantes de cet article :

* [Instanciation du composant](#component-instantiation-server)
* [Méthodes de cycle de vie](#lifecycle-methods-server)
* [Logique de rendu](#rendering-logic-server)
* [Gestionnaires d’événements](#event-handlers-server)
* [Suppression de composants](#component-disposal-server)
* [Interopérabilité JavaScript](#javascript-interop-server)
* [Blazor Server rerendu](#blazor-server-prerendering-server)

<h3 id="component-instantiation-server">Instanciation du composant</h3>

Lorsque Blazor crée une instance d’un composant :

* Le constructeur du composant est appelé.
* Les constructeurs de tous les services d’injection de code non Singleton fournis au constructeur du composant via la [`@inject`](xref:mvc/views/razor#inject) directive ou l' [ `[Inject]` attribut](xref:blazor/fundamentals/dependency-injection#request-a-service-in-a-component) sont appelés.

Un Blazor Server circuit échoue quand un constructeur exécuté ou une méthode setter pour une `[Inject]` propriété lève une exception non gérée. L’exception est irrécupérable, car l’infrastructure ne peut pas instancier le composant. Si la logique du constructeur peut lever des exceptions, l’application doit intercepter les exceptions à l’aide d’une [`try-catch`](/dotnet/csharp/language-reference/keywords/try-catch) instruction avec la gestion des erreurs et la journalisation.

<h3 id="lifecycle-methods-server">Méthodes de cycle de vie</h3>

Pendant la durée de vie d’un composant, Blazor appelle des [méthodes de cycle de vie](xref:blazor/components/lifecycle#lifecycle-methods). Si une méthode de cycle de vie lève une exception, de manière synchrone ou asynchrone, l’exception est irrécupérable pour un Blazor Server circuit. Pour les composants qui gèrent les erreurs dans les méthodes de cycle de vie, ajoutez une logique de gestion des erreurs.

Dans l’exemple suivant, où <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync%2A> appelle une méthode pour obtenir un produit :

* Une exception levée dans la `ProductRepository.GetProductByIdAsync` méthode est gérée par une [`try-catch`](/dotnet/csharp/language-reference/keywords/try-catch) instruction.
* Lorsque le `catch` bloc est exécuté :
  * `loadFailed` a la valeur `true` , qui est utilisée pour afficher un message d’erreur à l’utilisateur.
  * L’erreur est consignée.

::: moniker range=">= aspnetcore-5.0"

[!code-razor[](~/blazor/common/samples/5.x/BlazorSample_Server/Pages/handle-errors/ProductDetails.razor?name=snippet&highlight=11,27-39)]

::: moniker-end

::: moniker range="< aspnetcore-5.0"

[!code-razor[](~/blazor/common/samples/3.x/BlazorSample_Server/Pages/handle-errors/ProductDetails.razor?name=snippet&highlight=11,27-39)]

::: moniker-end

<h3 id="rendering-logic-server">Logique de rendu</h3>

Le balisage déclaratif dans un Razor fichier de composant ( `.razor` ) est compilé dans une méthode C# appelée <xref:Microsoft.AspNetCore.Components.ComponentBase.BuildRenderTree%2A> . Lorsqu’un composant affiche, <xref:Microsoft.AspNetCore.Components.ComponentBase.BuildRenderTree%2A> exécute et génère une structure de données décrivant les éléments, le texte et les composants enfants du composant rendu.

La logique de rendu peut lever une exception. Un exemple de ce scénario se produit lorsque `@someObject.PropertyName` est évalué `@someObject` , mais est `null` . Une exception non gérée levée par la logique de rendu est irrécupérable pour un Blazor Server circuit.

Pour empêcher une <xref:System.NullReferenceException> dans la logique de rendu, recherchez un `null` objet avant d’accéder à ses membres. Dans l’exemple suivant, les `person.Address` Propriétés ne sont pas accessibles si `person.Address` est `null` :

::: moniker range=">= aspnetcore-5.0"

[!code-razor[](~/blazor/common/samples/5.x/BlazorSample_Server/Pages/handle-errors/PersonExample.razor?name=snippet&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-5.0"

[!code-razor[](~/blazor/common/samples/3.x/BlazorSample_Server/Pages/handle-errors/PersonExample.razor?name=snippet&highlight=1)]

::: moniker-end

Le code précédent suppose que `person` ne l’est pas `null` . Souvent, la structure du code garantit l’existence d’un objet au moment du rendu du composant. Dans ce cas, il n’est pas nécessaire de rechercher `null` dans la logique de rendu. Dans l’exemple précédent, `person` peut être garanti qu’il existe, car `person` est créé lors de l’instanciation du composant, comme le montre l’exemple suivant :

```razor
@code {
    private Person person = new Person();

    ...
}
```

<h3 id="event-handlers-server">Gestionnaires d’événements</h3>

Le code côté client déclenche des appels de code C# lors de la création de gestionnaires d’événements à l’aide de :

* `@onclick`
* `@onchange`
* Autres `@on...` attributs
* `@bind`

Le code du gestionnaire d’événements peut lever une exception non gérée dans ces scénarios.

Si un gestionnaire d’événements lève une exception non gérée (par exemple, une requête de base de données échoue), l’exception est irrécupérable pour un Blazor Server circuit. Si l’application appelle du code qui pourrait échouer pour des raisons externes, interceptez les exceptions à l’aide d’une [`try-catch`](/dotnet/csharp/language-reference/keywords/try-catch) instruction avec gestion des erreurs et journalisation.

Si le code utilisateur n’intercepte pas et ne gère pas l’exception, le Framework journalise l’exception et met fin au circuit.

<h3 id="component-disposal-server">Suppression de composants</h3>

Un composant peut être supprimé de l’interface utilisateur, par exemple, parce que l’utilisateur a accédé à une autre page. Quand un composant qui implémente <xref:System.IDisposable?displayProperty=fullName> est supprimé de l’interface utilisateur, le Framework appelle la méthode du composant <xref:System.IDisposable.Dispose%2A> .

Si la méthode du composant `Dispose` lève une exception non gérée, l’exception est irrécupérable pour un Blazor Server circuit. Si la logique de suppression peut lever des exceptions, l’application doit intercepter les exceptions à l’aide d’une [`try-catch`](/dotnet/csharp/language-reference/keywords/try-catch) instruction avec la gestion des erreurs et la journalisation.

Pour plus d’informations sur la suppression de composants, consultez <xref:blazor/components/lifecycle#component-disposal-with-idisposable> .

<h3 id="javascript-interop-server">Interopérabilité JavaScript</h3>

<xref:Microsoft.JSInterop.IJSRuntime.InvokeAsync%2A?displayProperty=nameWithType> permet au code .NET d’effectuer des appels asynchrones au runtime JavaScript dans le navigateur de l’utilisateur.

Les conditions suivantes s’appliquent à la gestion des erreurs avec <xref:Microsoft.JSInterop.IJSRuntime.InvokeAsync%2A> :

* Si un appel à <xref:Microsoft.JSInterop.IJSRuntime.InvokeAsync%2A> échoue de façon synchrone, une exception .net se produit. Un appel à <xref:Microsoft.JSInterop.IJSRuntime.InvokeAsync%2A> peut échouer, par exemple, car les arguments fournis ne peuvent pas être sérialisés. Le code du développeur doit intercepter l’exception. Si le code d’application dans une méthode de gestionnaire d’événements ou de cycle de vie de composant ne gère pas une exception, l’exception résultante est irrécupérable pour un Blazor Server circuit.
* Si un appel à <xref:Microsoft.JSInterop.IJSRuntime.InvokeAsync%2A> échoue de manière asynchrone, le .NET <xref:System.Threading.Tasks.Task> échoue. Un appel à <xref:Microsoft.JSInterop.IJSRuntime.InvokeAsync%2A> peut échouer, par exemple, parce que le code côté JavaScript lève une exception ou retourne un `Promise` qui s’est terminé comme `rejected` . Le code du développeur doit intercepter l’exception. Si vous utilisez l' [`await`](/dotnet/csharp/language-reference/keywords/await) opérateur, encapsulez l’appel de méthode dans une [`try-catch`](/dotnet/csharp/language-reference/keywords/try-catch) instruction avec la gestion des erreurs et la journalisation. Dans le cas contraire, le code défaillant entraîne une exception non gérée qui est irrécupérable pour un Blazor Server circuit.
* Par défaut, les appels à <xref:Microsoft.JSInterop.IJSRuntime.InvokeAsync%2A> doivent se terminer dans un laps de temps donné, sinon l’appel expire. Le délai d’expiration par défaut est d’une minute. Le délai d’attente protège le code contre toute perte de connectivité réseau ou de code JavaScript qui ne renvoie jamais de message d’achèvement. Si l’appel expire, le résultant <xref:System.Threading.Tasks> échoue avec un <xref:System.OperationCanceledException> . Interceptez et traitez l’exception avec la journalisation.

De même, le code JavaScript peut initier des appels à des méthodes .NET indiquées par l' [ `[JSInvokable]` attribut](xref:blazor/call-dotnet-from-javascript). Si ces méthodes .NET lèvent une exception non gérée :

* L’exception n’est pas traitée comme étant irrécupérable pour un Blazor Server circuit.
* Le côté JavaScript `Promise` est rejeté.

Vous avez la possibilité d’utiliser le code de gestion des erreurs côté .NET ou JavaScript de l’appel de méthode.

Pour plus d’informations, consultez les articles suivants :

* <xref:blazor/call-javascript-from-dotnet>
* <xref:blazor/call-dotnet-from-javascript>

<h3 id="blazor-server-prerendering-server">Blazor Server préaffichant</h3>

Blazor les composants peuvent être prérendus à l’aide du [tag Helper du composant](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper) , afin que le balisage HTML rendu soit renvoyé dans le cadre de la requête http initiale de l’utilisateur. Cela fonctionne de la façon suivante :

* Création d’un nouveau circuit pour tous les composants prérendus qui font partie de la même page.
* Génération du code HTML initial.
* Traitement du circuit comme `disconnected` jusqu’à ce que le navigateur de l’utilisateur établisse une SignalR connexion avec le même serveur. Lorsque la connexion est établie, l’interactivité sur le circuit est reprise et le balisage HTML des composants est mis à jour.

Si un composant lève une exception non gérée pendant le prérendu, par exemple, pendant une méthode de cycle de vie ou dans une logique de rendu :

* L’exception est irrécupérable pour le circuit.
* L’exception est levée dans la pile des appels à partir du <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper> tag Helper. Par conséquent, la requête HTTP entière échoue, sauf si l’exception est explicitement interceptée par le code du développeur.

Dans des circonstances normales, lorsque le prérendu échoue, la création et le rendu du composant n’ont pas de sens, car un composant de travail ne peut pas être rendu.

Pour tolérer les erreurs qui peuvent se produire pendant le prérendu, la logique de gestion des erreurs doit être placée à l’intérieur d’un composant qui peut lever des exceptions. Utilisez des [`try-catch`](/dotnet/csharp/language-reference/keywords/try-catch) instructions avec la gestion des erreurs et la journalisation. Au lieu d’encapsuler le <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper> tag Helper dans une [`try-catch`](/dotnet/csharp/language-reference/keywords/try-catch) instruction, placez la logique de gestion des erreurs dans le composant rendu par le <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper> tag Helper.

## <a name="advanced-scenarios"></a>Scénarios avancés

### <a name="recursive-rendering"></a>Rendu récursif

Les composants peuvent être imbriqués de manière récursive. Cela est utile pour représenter des structures de données récursives. Par exemple, un `TreeNode` composant peut restituer plus de `TreeNode` composants pour chacun des enfants du nœud.

Lors du rendu de manière récursive, évitez les modèles de codage qui se traduisent par une récurrence infinie :

* Ne rendez pas de manière récursive une structure de données qui contient un cycle. Par exemple, n’affichez pas un nœud d’arbre dont les enfants s’y trouvent.
* Ne créez pas une chaîne de dispositions qui contiennent un cycle. Par exemple, ne créez pas une disposition dont la disposition est elle-même.
* N’autorisez pas un utilisateur final à enfreindre les invariants de récurrence (règles) par le biais d’une entrée de données malveillante ou d’appels Interop JavaScript.

Boucles infinies pendant le rendu :

* Fait en sorte que le processus de rendu continue de façon permanente.
* Équivaut à créer une boucle non terminée.

Dans ces scénarios, un Blazor Server circuit affecté échoue et le thread tente généralement d’effectuer les opérations suivantes :

* Consommez le plus de temps processeur autorisé par le système d’exploitation, indéfiniment.
* Consommez une quantité illimitée de mémoire serveur. La consommation de mémoire illimitée est équivalente au scénario dans lequel une boucle non terminée ajoute des entrées à une collection à chaque itération.

Pour éviter les modèles de récurrence infinis, assurez-vous que le code de rendu récursif contient des conditions d’arrêt appropriées.

### <a name="custom-render-tree-logic"></a>Logique d’arborescence de rendu personnalisé

La plupart des Blazor composants sont implémentés en tant que Razor fichiers de composants ( `.razor` ) et sont compilés par l’infrastructure pour produire une logique qui opère sur un <xref:Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder> pour afficher leur sortie. Toutefois, un développeur peut implémenter la <xref:Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder> logique manuellement à l’aide du code C# procédural. Pour plus d’informations, consultez <xref:blazor/advanced-scenarios#manual-rendertreebuilder-logic>.

> [!WARNING]
> L’utilisation de la logique du générateur d’arborescence de rendu manuel est considérée comme un scénario avancé et risqué, non recommandé pour le développement de composants généraux.

Si le <xref:Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder> code est écrit, le développeur doit garantir l’exactitude du code. Par exemple, le développeur doit s’assurer que :

* Les appels à <xref:Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder.OpenElement%2A> et <xref:Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder.CloseElement%2A> sont correctement équilibrés.
* Les attributs sont ajoutés uniquement aux emplacements appropriés.

Une logique incorrecte du générateur d’arborescence de rendu manuel peut entraîner un comportement arbitraire non défini, y compris des pannes, des blocages du serveur et des failles de sécurité.

Considérez la logique du générateur d’arborescence de rendu manuel sur le même niveau de complexité et avec le même niveau de *danger* que l’écriture de code assembleur ou d’instructions [MSIL (Microsoft Intermediate Language)](/dotnet/standard/managed-code) manuellement.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:blazor/fundamentals/logging>
* <xref:fundamentals/error-handling>&dagger;

&dagger;S’applique aux applications ASP.NET Core côté serveur qui sont des applications principales d’API Web pour les Blazor applications.

::: zone-end
