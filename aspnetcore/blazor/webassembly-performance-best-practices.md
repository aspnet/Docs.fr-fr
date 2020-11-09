---
title: Blazor WebAssemblyMeilleures pratiques en matière de performances de ASP.net Core
author: pranavkm
description: 'Conseils pour améliorer les performances dans les Blazor WebAssembly applications ASP.net Core et éviter les problèmes de performances courants.'
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc, devx-track-js
ms.date: 10/09/2020
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
uid: blazor/webassembly-performance-best-practices
ms.openlocfilehash: 423745d734d8da2b8f3f974f9b4dd1a0265d4877
ms.sourcegitcommit: ca34c1ac578e7d3daa0febf1810ba5fc74f60bbf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93054734"
---
# <a name="aspnet-core-no-locblazor-webassembly-performance-best-practices"></a><span data-ttu-id="f2b67-103">Blazor WebAssemblyMeilleures pratiques en matière de performances de ASP.net Core</span><span class="sxs-lookup"><span data-stu-id="f2b67-103">ASP.NET Core Blazor WebAssembly performance best practices</span></span>

<span data-ttu-id="f2b67-104">Par [Pranav Krishnamoorthy](https://github.com/pranavkm) et [Steve Sanderson](https://github.com/SteveSandersonMS)</span><span class="sxs-lookup"><span data-stu-id="f2b67-104">By [Pranav Krishnamoorthy](https://github.com/pranavkm) and [Steve Sanderson](https://github.com/SteveSandersonMS)</span></span>

<span data-ttu-id="f2b67-105">Blazor WebAssembly est conçu et optimisé pour assurer des performances élevées dans les scénarios d’interface utilisateur d’application les plus réalistes.</span><span class="sxs-lookup"><span data-stu-id="f2b67-105">Blazor WebAssembly is carefully designed and optimized to enable high performance in most realistic application UI scenarios.</span></span> <span data-ttu-id="f2b67-106">Toutefois, la production des meilleurs résultats dépend des développeurs qui utilisent les modèles et les fonctionnalités appropriés.</span><span class="sxs-lookup"><span data-stu-id="f2b67-106">However, producing the best results depends on developers using the right patterns and features.</span></span> <span data-ttu-id="f2b67-107">Tenez compte des aspects suivants :</span><span class="sxs-lookup"><span data-stu-id="f2b67-107">Consider the following aspects:</span></span>

* <span data-ttu-id="f2b67-108">**Débit du runtime** : le code .net s’exécute sur un interpréteur au sein du runtime webassembly, le débit de l’UC est donc limité.</span><span class="sxs-lookup"><span data-stu-id="f2b67-108">**Runtime throughput** : The .NET code runs on an interpreter within the WebAssembly runtime, so CPU throughput is limited.</span></span> <span data-ttu-id="f2b67-109">Dans les scénarios exigeants, l’application tire parti de l’optimisation de la [vitesse de rendu](#optimize-rendering-speed).</span><span class="sxs-lookup"><span data-stu-id="f2b67-109">In demanding scenarios, the app benefits from [optimizing rendering speed](#optimize-rendering-speed).</span></span>
* <span data-ttu-id="f2b67-110">**Heure de démarrage** : l’application transfère un Runtime .net au navigateur. il est donc important d’utiliser des fonctionnalités qui [réduisent la taille du téléchargement de l’application](#minimize-app-download-size).</span><span class="sxs-lookup"><span data-stu-id="f2b67-110">**Startup time** : The app transfers a .NET runtime to the browser, so it's important to use features that [minimize the application download size](#minimize-app-download-size).</span></span>

## <a name="optimize-rendering-speed"></a><span data-ttu-id="f2b67-111">Optimiser la vitesse de rendu</span><span class="sxs-lookup"><span data-stu-id="f2b67-111">Optimize rendering speed</span></span>

<span data-ttu-id="f2b67-112">Les sections suivantes fournissent des recommandations pour réduire la charge de travail de rendu et améliorer la réactivité de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="f2b67-112">The following sections provide recommendations to minimize rendering workload and improve UI responsiveness.</span></span> <span data-ttu-id="f2b67-113">Le fait de suivre ce Conseil pourrait facilement améliorer la vitesse de rendu de l’interface utilisateur à *dix fois ou plus* .</span><span class="sxs-lookup"><span data-stu-id="f2b67-113">Following this advice could easily make a *ten-fold or higher improvement* in UI rendering speeds.</span></span>

### <a name="avoid-unnecessary-rendering-of-component-subtrees"></a><span data-ttu-id="f2b67-114">Éviter le rendu inutile des sous-arborescences de composants</span><span class="sxs-lookup"><span data-stu-id="f2b67-114">Avoid unnecessary rendering of component subtrees</span></span>

<span data-ttu-id="f2b67-115">Au moment de l’exécution, les composants existent sous la forme d’une hiérarchie.</span><span class="sxs-lookup"><span data-stu-id="f2b67-115">At runtime, components exist as a hierarchy.</span></span> <span data-ttu-id="f2b67-116">Un composant racine a des composants enfants.</span><span class="sxs-lookup"><span data-stu-id="f2b67-116">A root component has child components.</span></span> <span data-ttu-id="f2b67-117">À son tour, les enfants de la racine ont leurs propres composants enfants, et ainsi de suite.</span><span class="sxs-lookup"><span data-stu-id="f2b67-117">In turn, the root's children have their own child components, and so on.</span></span> <span data-ttu-id="f2b67-118">Lorsqu’un événement se produit, tel qu’un utilisateur qui sélectionne un bouton, c’est la méthode Blazor qui détermine les composants à restituer :</span><span class="sxs-lookup"><span data-stu-id="f2b67-118">When an event occurs, such as a user selecting a button, this is how Blazor decides which components to rerender:</span></span>

 1. <span data-ttu-id="f2b67-119">L’événement lui-même est distribué à n’importe quel composant qui rend le gestionnaire de l’événement.</span><span class="sxs-lookup"><span data-stu-id="f2b67-119">The event itself is dispatched to whichever component rendered the event's handler.</span></span> <span data-ttu-id="f2b67-120">Après l’exécution du gestionnaire d’événements, ce composant est rerendu.</span><span class="sxs-lookup"><span data-stu-id="f2b67-120">After executing the event handler, that component is rerendered.</span></span>
 1. <span data-ttu-id="f2b67-121">Chaque fois qu’un composant est rerendu, il fournit une nouvelle copie des valeurs de paramètre à chacun de ses composants enfants.</span><span class="sxs-lookup"><span data-stu-id="f2b67-121">Whenever any component is rerendered, it supplies a new copy of the parameter values to each of its child components.</span></span>
 1. <span data-ttu-id="f2b67-122">Lors de la réception d’un nouvel ensemble de valeurs de paramètres, chaque composant choisit s’il faut effectuer un nouveau rendu.</span><span class="sxs-lookup"><span data-stu-id="f2b67-122">When receiving a new set of parameter values, each component chooses whether to rerender.</span></span> <span data-ttu-id="f2b67-123">Par défaut, les composants sont rerendus si les valeurs des paramètres ont pu être modifiées (par exemple, s’il s’agit d’objets mutables).</span><span class="sxs-lookup"><span data-stu-id="f2b67-123">By default, components rerender if the parameter values may have changed (for example, if they are mutable objects).</span></span>

<span data-ttu-id="f2b67-124">Les deux dernières étapes de cette séquence continuent de manière récursive la hiérarchie des composants.</span><span class="sxs-lookup"><span data-stu-id="f2b67-124">The last two steps of this sequence continue recursively down the component hierarchy.</span></span> <span data-ttu-id="f2b67-125">Dans de nombreux cas, l’ensemble de la sous-arborescence est rerendu.</span><span class="sxs-lookup"><span data-stu-id="f2b67-125">In many cases, the entire subtree is rerendered.</span></span> <span data-ttu-id="f2b67-126">Cela signifie que les événements ciblant des composants de haut niveau peuvent entraîner des processus de rerendu coûteux, car tout ce qui se trouve au-dessous de ce point doit être rerendu.</span><span class="sxs-lookup"><span data-stu-id="f2b67-126">This means that events targeting high-level components can cause expensive rerendering processes because everything below that point must be rerendered.</span></span>

<span data-ttu-id="f2b67-127">Si vous souhaitez interrompre ce processus et empêcher la récursivité du rendu dans une sous-arborescence particulière, vous pouvez soit :</span><span class="sxs-lookup"><span data-stu-id="f2b67-127">If you want to interrupt this process and prevent rendering recursion into a particular subtree, then you can either:</span></span>

 * <span data-ttu-id="f2b67-128">Assurez-vous que tous les paramètres d’un certain composant sont de types immuables primitifs (par exemple,,,, `string` `int` `bool` `DateTime` et autres).</span><span class="sxs-lookup"><span data-stu-id="f2b67-128">Ensure that all parameters to a certain component are of primitive immutable types (for example, `string`, `int`, `bool`, `DateTime`, and others).</span></span> <span data-ttu-id="f2b67-129">La logique intégrée pour la détection des modifications ignore automatiquement le rerendu si aucune de ces valeurs de paramètre n’a changé.</span><span class="sxs-lookup"><span data-stu-id="f2b67-129">The built-in logic for detecting changes automatically skips rerendering if none of these parameter values have changed.</span></span> <span data-ttu-id="f2b67-130">Si vous affichez un composant enfant avec `<Customer CustomerId="@item.CustomerId" />` , où `CustomerId` est une `int` valeur, il n’est pas rerendu, sauf en cas de `item.CustomerId` modification.</span><span class="sxs-lookup"><span data-stu-id="f2b67-130">If you render a child component with `<Customer CustomerId="@item.CustomerId" />`, where `CustomerId` is an `int` value, then it isn't rerendered except when `item.CustomerId` changes.</span></span>
 * <span data-ttu-id="f2b67-131">Si vous avez besoin d’accepter des valeurs de paramètre non primitives, telles que des types de modèles personnalisés, des rappels d’événements ou des <xref:Microsoft.AspNetCore.Components.RenderFragment> valeurs, vous pouvez substituer <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> pour contrôler la décision relative à l’affichage, qui est décrit dans l' [utilisation `ShouldRender` de](#use-of-shouldrender) la section.</span><span class="sxs-lookup"><span data-stu-id="f2b67-131">If you need to accept nonprimitive parameter values, such as custom model types, event callbacks, or <xref:Microsoft.AspNetCore.Components.RenderFragment> values, then you can override <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> to control the decision about whether to render, which is described in the [Use of `ShouldRender`](#use-of-shouldrender) section.</span></span>

<span data-ttu-id="f2b67-132">En ignorant le rerendu des sous-arborescences entières, vous pouvez supprimer la grande majorité du coût de rendu lorsqu’un événement se produit.</span><span class="sxs-lookup"><span data-stu-id="f2b67-132">By skipping rerendering of whole subtrees, you may be able to remove the vast majority of the rendering cost when an event occurs.</span></span>

<span data-ttu-id="f2b67-133">Vous souhaiterez peut-être prendre en compte les composants enfants spécifiquement pour pouvoir ignorer le rerendu de cette partie de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="f2b67-133">You may wish to factor out child components specifically so that you can skip rerendering that part of the UI.</span></span> <span data-ttu-id="f2b67-134">Il s’agit d’un moyen valide de réduire le coût de rendu d’un composant parent.</span><span class="sxs-lookup"><span data-stu-id="f2b67-134">This is a valid way to reduce the rendering cost of a parent component.</span></span>

#### <a name="use-of-shouldrender"></a><span data-ttu-id="f2b67-135">Utilisation de `ShouldRender`</span><span class="sxs-lookup"><span data-stu-id="f2b67-135">Use of `ShouldRender`</span></span>

<span data-ttu-id="f2b67-136">Si vous créez un composant d’interface utilisateur qui n’est jamais modifié après le rendu initial (indépendamment des valeurs de paramètre), configurez <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> pour retourner `false` :</span><span class="sxs-lookup"><span data-stu-id="f2b67-136">If authoring a UI-only component that never changes after the initial render (regardless of any parameter values), configure <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> to return `false`:</span></span>

```razor
@code {
    protected override bool ShouldRender() => false;
}
```

<span data-ttu-id="f2b67-137">Si le composant nécessite uniquement un rerendu lorsque ses valeurs de paramètres changent de manière particulière, vous pouvez utiliser des champs privés pour suivre les informations nécessaires pour détecter les modifications.</span><span class="sxs-lookup"><span data-stu-id="f2b67-137">If the component only requires rerendering when its parameter values mutate in particular ways, then you can use private fields to track the necessary information to detect changes.</span></span> <span data-ttu-id="f2b67-138">Dans l’exemple suivant, `shouldRender` est basé sur la vérification de tout type de modification ou de mutation qui doit demander un rerendu.</span><span class="sxs-lookup"><span data-stu-id="f2b67-138">In the following example, `shouldRender` is based on checking for any kind of change or mutation that should prompt a rerender.</span></span> <span data-ttu-id="f2b67-139">`prevOutboundFlightId` et `prevInboundFlightId` suivent les informations relatives à la prochaine mise à jour potentielle :</span><span class="sxs-lookup"><span data-stu-id="f2b67-139">`prevOutboundFlightId` and `prevInboundFlightId` track information for the next potential update:</span></span>

```razor
@code {
    [Parameter]
    public FlightInfo OutboundFlight { get; set; }
    
    [Parameter]
    public FlightInfo InboundFlight { get; set; }

    private int prevOutboundFlightId;
    private int prevInboundFlightId;
    private bool shouldRender;

    protected override void OnParametersSet()
    {
        shouldRender = OutboundFlight.FlightId != prevOutboundFlightId
            || InboundFlight.FlightId != prevInboundFlightId;

        prevOutboundFlightId = OutboundFlight.FlightId;
        prevInboundFlightId = InboundFlight.FlightId;
    }

    protected override void ShouldRender() => shouldRender;

    // Note that 
}
```

<span data-ttu-id="f2b67-140">Dans le code précédent, un gestionnaire d’événements peut également `shouldRender` avoir la valeur pour `true` que le composant soit rerendu après l’événement.</span><span class="sxs-lookup"><span data-stu-id="f2b67-140">In the preceding code, an event handler may also set `shouldRender` to `true` so that the component is rerendered after the event.</span></span>

<span data-ttu-id="f2b67-141">Pour la plupart des composants, ce niveau de contrôle manuel n’est pas nécessaire.</span><span class="sxs-lookup"><span data-stu-id="f2b67-141">For most components, this level of manual control isn't necessary.</span></span> <span data-ttu-id="f2b67-142">Vous devez uniquement vous préoccuper du fait que vous ignorez le rendu des sous-arborescences si ces sous-arborescences sont particulièrement coûteuses à afficher et sont à l’origine du décalage de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="f2b67-142">You should only be concerned about skipping rendering subtrees if those subtrees are particularly expensive to render and are causing UI lag.</span></span>

<span data-ttu-id="f2b67-143">Pour plus d'informations, consultez <xref:blazor/components/lifecycle>.</span><span class="sxs-lookup"><span data-stu-id="f2b67-143">For more information, see <xref:blazor/components/lifecycle>.</span></span>

::: moniker range=">= aspnetcore-5.0"

### <a name="virtualization"></a><span data-ttu-id="f2b67-144">Virtualisation</span><span class="sxs-lookup"><span data-stu-id="f2b67-144">Virtualization</span></span>

<span data-ttu-id="f2b67-145">Lors du rendu de grandes quantités d’IU au sein d’une boucle, par exemple une liste ou une grille avec des milliers d’entrées, la quantité importante d’opérations de rendu peut entraîner un décalage de l’interface utilisateur et donc une expérience utilisateur médiocre.</span><span class="sxs-lookup"><span data-stu-id="f2b67-145">When rendering large amounts of UI within a loop, for example a list or grid with thousands of entries, the sheer quantity of rendering operations can lead to a lag in UI rendering and thus a poor user experience.</span></span> <span data-ttu-id="f2b67-146">Étant donné que l’utilisateur ne peut voir qu’un petit nombre d’éléments à la fois sans faire défiler la liste, il semblerait gaspiller de perdre du temps à afficher des éléments qui ne sont pas actuellement visibles.</span><span class="sxs-lookup"><span data-stu-id="f2b67-146">Given that the user can only see a small number of elements at once without scrolling, it seems wasteful to spend so much time rendering elements that aren't currently visible.</span></span>

<span data-ttu-id="f2b67-147">Pour résoudre ce Blazor problème, fournit un [ `<Virtualize>` composant](xref:blazor/components/virtualization) intégré qui crée les comportements d’apparence et de défilement d’une liste arbitrairement grande, mais qui affiche en fait uniquement les éléments de liste qui se trouvent dans la fenêtre d’affichage de défilement en cours.</span><span class="sxs-lookup"><span data-stu-id="f2b67-147">To address this, Blazor provides a built-in [`<Virtualize>` component](xref:blazor/components/virtualization) that creates the appearance and scroll behaviors of an arbitrarily-large list but actually only renders the list items that are within the current scroll viewport.</span></span> <span data-ttu-id="f2b67-148">Par exemple, cela signifie que l’application peut avoir une liste avec 100 000 entrées, mais uniquement le coût de rendu de 20 éléments visibles à un moment donné.</span><span class="sxs-lookup"><span data-stu-id="f2b67-148">For example, this means that the app can have a list with 100,000 entries but only pay the rendering cost of 20 items that are visible at any one time.</span></span> <span data-ttu-id="f2b67-149">L’utilisation du `<Virtualize>` composant peut mettre à l’échelle les performances de l’interface utilisateur par ordre de grandeur.</span><span class="sxs-lookup"><span data-stu-id="f2b67-149">Use of the `<Virtualize>` component can scale up UI performance by orders of magnitude.</span></span>

<span data-ttu-id="f2b67-150">`<Virtualize>` peut être utilisé dans les cas suivants :</span><span class="sxs-lookup"><span data-stu-id="f2b67-150">`<Virtualize>` can be used when:</span></span>

 * <span data-ttu-id="f2b67-151">Rendu d’un ensemble d’éléments de données dans une boucle.</span><span class="sxs-lookup"><span data-stu-id="f2b67-151">Rendering a set of data items in a loop.</span></span>
 * <span data-ttu-id="f2b67-152">La plupart des éléments ne sont pas visibles en raison du défilement.</span><span class="sxs-lookup"><span data-stu-id="f2b67-152">Most of the items aren't visible due to scrolling.</span></span>
 * <span data-ttu-id="f2b67-153">Les éléments rendus ont exactement la même taille.</span><span class="sxs-lookup"><span data-stu-id="f2b67-153">The rendered items are exactly the same size.</span></span> <span data-ttu-id="f2b67-154">Lorsque l’utilisateur fait défiler jusqu’à un point arbitraire, le composant peut calculer les éléments visibles à afficher.</span><span class="sxs-lookup"><span data-stu-id="f2b67-154">When the user scrolls to an arbitrary point, the component can calculate the visible items to show.</span></span>

<span data-ttu-id="f2b67-155">L’exemple suivant illustre une liste non virtualisée :</span><span class="sxs-lookup"><span data-stu-id="f2b67-155">The following shows an example of a non-virtualized list:</span></span>

```razor
<div class="all-flights" style="height:500px;overflow-y:scroll">
    @foreach (var flight in allFlights)
    {
        <FlightSummary @key="flight.FlightId" Flight="@flight" />
    }
</div>
```

<span data-ttu-id="f2b67-156">Si la `allFlights` collection contient 10 000 éléments, elle instancie et restitue les `<FlightSummary>` instances de composant 10 000.</span><span class="sxs-lookup"><span data-stu-id="f2b67-156">If the `allFlights` collection holds 10,000 items, it instantiates and renders 10,000 `<FlightSummary>` component instances.</span></span> <span data-ttu-id="f2b67-157">En comparaison, le code suivant montre un exemple de liste virtualisée :</span><span class="sxs-lookup"><span data-stu-id="f2b67-157">In comparison, the following shows an example of a virtualized list:</span></span>

```razor
<div class="all-flights" style="height:500px;overflow-y:scroll">
    <Virtualize Items="@allFlights" Context="flight">
        <FlightSummary @key="flight.FlightId" Flight="@flight" />
    </Virtualize>
</div>
```

<span data-ttu-id="f2b67-158">Même si l’interface utilisateur résultante est similaire à un utilisateur, en arrière-plan, le composant instancie et affiche uniquement autant d' `<FlightSummary>` instances que nécessaire pour remplir la région défilante.</span><span class="sxs-lookup"><span data-stu-id="f2b67-158">Even though the resulting UI looks the same to a user, behind the scenes the component only instantiates and renders as many `<FlightSummary>` instances as are required to fill the scrollable region.</span></span> <span data-ttu-id="f2b67-159">L’ensemble des `<FlightSummary>` instances affichées est recalculé et restitué lorsque l’utilisateur fait défiler.</span><span class="sxs-lookup"><span data-stu-id="f2b67-159">The set of `<FlightSummary>` instances displayed is recalculated and rendered as the user scrolls.</span></span>

<span data-ttu-id="f2b67-160">`<Virtualize>` offre également d’autres avantages.</span><span class="sxs-lookup"><span data-stu-id="f2b67-160">`<Virtualize>` has other benefits, too.</span></span> <span data-ttu-id="f2b67-161">Par exemple, lorsqu’un composant demande des données à partir d’une API externe, `<Virtualize>` permet au composant d’extraire uniquement le segment des enregistrements qui correspondent à la région visible actuelle, au lieu de télécharger toutes les données de la collection.</span><span class="sxs-lookup"><span data-stu-id="f2b67-161">For example when a component requests data from an external API, `<Virtualize>` permits the component to only fetch the slice of records that correspond to the current visible region, instead of downloading all the data from the collection.</span></span>

<span data-ttu-id="f2b67-162">Pour plus d'informations, consultez <xref:blazor/components/virtualization>.</span><span class="sxs-lookup"><span data-stu-id="f2b67-162">For more information, see <xref:blazor/components/virtualization>.</span></span>

::: moniker-end

### <a name="create-lightweight-optimized-components"></a><span data-ttu-id="f2b67-163">Créer des composants légers et optimisés</span><span class="sxs-lookup"><span data-stu-id="f2b67-163">Create lightweight, optimized components</span></span>

<span data-ttu-id="f2b67-164">La plupart des Blazor composants ne nécessitent pas d’effort d’optimisation agressif.</span><span class="sxs-lookup"><span data-stu-id="f2b67-164">Most Blazor components don't require aggressive optimization efforts.</span></span> <span data-ttu-id="f2b67-165">Cela est dû au fait que la plupart des composants ne se répètent souvent pas dans l’interface utilisateur et ne sont pas restitués à une fréquence élevée.</span><span class="sxs-lookup"><span data-stu-id="f2b67-165">This is because most components don't often repeat in the UI and don't rerender at high frequency.</span></span> <span data-ttu-id="f2b67-166">Par exemple, les `@page` composants et les composants qui représentent des éléments d’interface utilisateur de haut niveau, tels que les boîtes de dialogue ou les formulaires, s’affichent le plus souvent un seul à la fois et sont uniquement rerendus en réponse à un mouvement utilisateur.</span><span class="sxs-lookup"><span data-stu-id="f2b67-166">For example, `@page` components and components representing high-level UI pieces such as dialogs or forms, most likely appear only one at a time and only rerender in response to a user gesture.</span></span> <span data-ttu-id="f2b67-167">Ces composants ne créent pas une charge de travail de rendu élevée. vous pouvez utiliser librement toute combinaison de fonctionnalités d’infrastructure que vous souhaitez sans vous soucier des performances de rendu.</span><span class="sxs-lookup"><span data-stu-id="f2b67-167">These components don't create a high rendering workload, so you can freely use any combination of framework features you want without worrying much about rendering performance.</span></span>

<span data-ttu-id="f2b67-168">Toutefois, il existe également des scénarios courants dans lesquels vous créez des composants qui doivent être répétés à l’échelle.</span><span class="sxs-lookup"><span data-stu-id="f2b67-168">However, there are also common scenarios where you build components that need to be repeated at scale.</span></span> <span data-ttu-id="f2b67-169">Exemple :</span><span class="sxs-lookup"><span data-stu-id="f2b67-169">For example:</span></span>

 * <span data-ttu-id="f2b67-170">Les grands formulaires imbriqués peuvent avoir des centaines d’entrées individuelles, d’étiquettes et d’autres éléments.</span><span class="sxs-lookup"><span data-stu-id="f2b67-170">Large nested forms may have hundreds of individual inputs, labels, and other elements.</span></span>
 * <span data-ttu-id="f2b67-171">Les grilles peuvent avoir des milliers de cellules.</span><span class="sxs-lookup"><span data-stu-id="f2b67-171">Grids may have thousands of cells.</span></span>
 * <span data-ttu-id="f2b67-172">Les nuages de points peuvent comporter des millions de points de données.</span><span class="sxs-lookup"><span data-stu-id="f2b67-172">Scatter plots may have millions of data points.</span></span>

<span data-ttu-id="f2b67-173">En cas de modélisation de chaque unité comme des instances de composant distinctes, il y aura autant d’entre elles que leurs performances de rendu deviennent critiques.</span><span class="sxs-lookup"><span data-stu-id="f2b67-173">If modelling each unit as separate component instances, there will be so many of them that their rendering performance does become critical.</span></span> <span data-ttu-id="f2b67-174">Cette section fournit des conseils sur la façon de rendre ces composants légers afin que l’interface utilisateur reste rapide et réactive.</span><span class="sxs-lookup"><span data-stu-id="f2b67-174">This section provides advice on making such components lightweight so that the UI remains fast and responsive.</span></span>

#### <a name="avoid-thousands-of-component-instances"></a><span data-ttu-id="f2b67-175">Éviter des milliers d’instances de composant</span><span class="sxs-lookup"><span data-stu-id="f2b67-175">Avoid thousands of component instances</span></span>

<span data-ttu-id="f2b67-176">Chaque composant est un îlot distinct qui peut être rendu indépendamment de ses parents et de ses enfants.</span><span class="sxs-lookup"><span data-stu-id="f2b67-176">Each component is a separate island that can render independently of its parents and children.</span></span> <span data-ttu-id="f2b67-177">En choisissant comment fractionner l’interface utilisateur en une hiérarchie de composants, vous prenez le contrôle de la granularité du rendu de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="f2b67-177">By choosing how to split up the UI into a hierarchy of components, you are taking control over the granularity of UI rendering.</span></span> <span data-ttu-id="f2b67-178">Il peut s’agir d’une valeur correcte ou d’une mauvaise performance.</span><span class="sxs-lookup"><span data-stu-id="f2b67-178">This can be either good or bad for performance.</span></span>

 * <span data-ttu-id="f2b67-179">En fractionnant l’interface utilisateur en plusieurs composants, vous pouvez avoir des parties plus petites du rendu de l’interface utilisateur lorsque des événements se produisent.</span><span class="sxs-lookup"><span data-stu-id="f2b67-179">By splitting the UI into more components, you can have smaller portions of the UI rerender when events occur.</span></span> <span data-ttu-id="f2b67-180">Par exemple, lorsqu’un utilisateur clique sur un bouton dans une ligne de table, vous pouvez être en mesure d’avoir uniquement ce rerendu de ligne unique à la place de la page ou de la table entière.</span><span class="sxs-lookup"><span data-stu-id="f2b67-180">For example when a user clicks a button in a table row, you may be able to have only that single row rerender instead of the whole page or table.</span></span>
 * <span data-ttu-id="f2b67-181">Toutefois, chaque composant supplémentaire implique des surcharges mémoire et processeur supplémentaires pour gérer son cycle de vie d’État et de rendu indépendant.</span><span class="sxs-lookup"><span data-stu-id="f2b67-181">However, each extra component involves some extra memory and CPU overhead to deal with its independent state and rendering lifecycle.</span></span>

<span data-ttu-id="f2b67-182">Lors du réglage des performances de Blazor WebAssembly sur .net 5, nous avons mesuré une surcharge de rendu d’environ 0,06 ms par instance de composant.</span><span class="sxs-lookup"><span data-stu-id="f2b67-182">When tuning the performance of Blazor WebAssembly on .NET 5, we measured a rendering overhead of around 0.06 ms per component instance.</span></span> <span data-ttu-id="f2b67-183">Elle est basée sur un composant simple qui accepte trois paramètres s’exécutant sur un ordinateur portable standard.</span><span class="sxs-lookup"><span data-stu-id="f2b67-183">This is based on a simple component that accepts three parameters running on a typical laptop.</span></span> <span data-ttu-id="f2b67-184">En interne, la surcharge est principalement due à la récupération de l’État par composant à partir de dictionnaires et au passage et à la réception de paramètres.</span><span class="sxs-lookup"><span data-stu-id="f2b67-184">Internally, the overhead is largely due to retrieving per-component state from dictionaries and passing and receiving parameters.</span></span> <span data-ttu-id="f2b67-185">Par multiplication, vous pouvez constater que l’ajout d’instances de composants supplémentaires 2 000 ajoute 0,12 secondes à l’heure de rendu et que l’interface utilisateur a commencé à être trop lente pour les utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="f2b67-185">By multiplication, you can see that adding 2,000 extra component instances would add 0.12 seconds to the rendering time and the UI would begin feeling slow to users.</span></span>

<span data-ttu-id="f2b67-186">Il est possible de rendre les composants plus légers afin de pouvoir en avoir davantage, mais la technique la plus puissante n’est souvent pas d’avoir autant de composants.</span><span class="sxs-lookup"><span data-stu-id="f2b67-186">It's possible to make components more lightweight so that you can have more of them, but often the more powerful technique is not to have so many components.</span></span> <span data-ttu-id="f2b67-187">Les sections suivantes décrivent deux approches.</span><span class="sxs-lookup"><span data-stu-id="f2b67-187">The following sections describe two approaches.</span></span>

##### <a name="inline-child-components-into-their-parents"></a><span data-ttu-id="f2b67-188">Composants enfants Inline dans leurs parents</span><span class="sxs-lookup"><span data-stu-id="f2b67-188">Inline child components into their parents</span></span>

<span data-ttu-id="f2b67-189">Prenons le composant suivant qui restitue une séquence de composants enfants :</span><span class="sxs-lookup"><span data-stu-id="f2b67-189">Consider the following component that renders a sequence of child components:</span></span>

```razor
<div class="chat">
    @foreach (var message in messages)
    {
        <ChatMessageDisplay Message="@message" />
    }
</div>
```

<span data-ttu-id="f2b67-190">Pour l’exemple de code précédent, le `<ChatMessageDisplay>` composant est défini dans un fichier `ChatMessageDisplay.razor` contenant :</span><span class="sxs-lookup"><span data-stu-id="f2b67-190">For the preceding example code, the `<ChatMessageDisplay>` component is defined in a file `ChatMessageDisplay.razor` containing:</span></span>

```razor
<div class="chat-message">
    <span class="author">@Message.Author</span>
    <span class="text">@Message.Text</span>
</div>

@code {
    [Parameter]
    public ChatMessage Message { get; set; }
}
```

<span data-ttu-id="f2b67-191">L’exemple précédent fonctionne correctement et fonctionne bien tant que des milliers de messages ne sont pas affichés à la fois.</span><span class="sxs-lookup"><span data-stu-id="f2b67-191">The preceding example works fine and performs well as long as thousands of messages aren't shown at once.</span></span> <span data-ttu-id="f2b67-192">Pour afficher des milliers de messages à la fois, envisagez de *ne pas* factoriser le `ChatMessageDisplay` composant séparé.</span><span class="sxs-lookup"><span data-stu-id="f2b67-192">To show thousands of messages at once, consider *not* factoring out the separate `ChatMessageDisplay` component.</span></span> <span data-ttu-id="f2b67-193">Au lieu de cela, incorporez le rendu directement dans le parent :</span><span class="sxs-lookup"><span data-stu-id="f2b67-193">Instead, inline the rendering directly into the parent:</span></span>

```razor
<div class="chat">
    @foreach (var message in messages)
    {
        <div class="chat-message">
            <span class="author">@message.Author</span>
            <span class="text">@message.Text</span>
        </div>
    }
</div>
```

<span data-ttu-id="f2b67-194">Cela permet d’éviter la surcharge par composant du rendu d’un grand nombre de composants enfants au prix de ne pas être en mesure de les restituer séparément.</span><span class="sxs-lookup"><span data-stu-id="f2b67-194">This avoids the per-component overhead of rendering so many child components at the cost of not being able to rerender each of them independently.</span></span>

##### <a name="define-reusable-renderfragments-in-code"></a><span data-ttu-id="f2b67-195">Définir réutilisable `RenderFragments` dans le code</span><span class="sxs-lookup"><span data-stu-id="f2b67-195">Define reusable `RenderFragments` in code</span></span>

<span data-ttu-id="f2b67-196">Vous pouvez prendre en compte les composants enfants comme un moyen de réutiliser la logique de rendu.</span><span class="sxs-lookup"><span data-stu-id="f2b67-196">You may be factoring out child components purely as a way of reusing rendering logic.</span></span> <span data-ttu-id="f2b67-197">Si c’est le cas, il est toujours possible de réutiliser la logique de rendu sans déclarer les composants réels.</span><span class="sxs-lookup"><span data-stu-id="f2b67-197">If that's the case, it's still possible to reuse rendering logic without declaring actual components.</span></span> <span data-ttu-id="f2b67-198">Dans le bloc d’un composant `@code` , vous pouvez définir un <xref:Microsoft.AspNetCore.Components.RenderFragment> qui émet l’interface utilisateur et peut être appelé à partir de n’importe quel emplacement :</span><span class="sxs-lookup"><span data-stu-id="f2b67-198">In any component's `@code` block, you can define a <xref:Microsoft.AspNetCore.Components.RenderFragment> that emits UI and can be called from anywhere:</span></span>

```razor
<h1>Hello, world!</h1>

@RenderWelcomeInfo

@code {
    RenderFragment RenderWelcomeInfo = __builder =>
    {
        <div>
            <p>Welcome to your new app!</p>

            <SurveyPrompt Title="How is Blazor working for you?" />
        </div>
    };
}
```

<span data-ttu-id="f2b67-199">Comme demonstated dans l’exemple précédent, les composants peuvent émettre un balisage à partir du code dans leur `@code` bloc et en dehors de celui-ci.</span><span class="sxs-lookup"><span data-stu-id="f2b67-199">As demonstated in the preceding example, components can emit markup from code within their `@code` block and outside it.</span></span> <span data-ttu-id="f2b67-200">Cette approche définit un <xref:Microsoft.AspNetCore.Components.RenderFragment> délégué que vous pouvez afficher à l’intérieur de la sortie de rendu normale du composant, éventuellement à plusieurs endroits.</span><span class="sxs-lookup"><span data-stu-id="f2b67-200">This approach defines a <xref:Microsoft.AspNetCore.Components.RenderFragment> delegate that you can render inside the component's normal render output, optionally in multiple places.</span></span> <span data-ttu-id="f2b67-201">Il est nécessaire que le délégué accepte un paramètre appelé `__builder` de type <xref:Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder> afin que le Razor compilateur puisse produire des instructions de rendu pour celui-ci.</span><span class="sxs-lookup"><span data-stu-id="f2b67-201">It's necessary for the delegate to accept a parameter called `__builder` of type <xref:Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder> so that the Razor compiler can produce rendering instructions for it.</span></span>

<span data-ttu-id="f2b67-202">Si vous souhaitez rendre ce réutilisable sur plusieurs composants, pensez à le déclarer en tant que `public static` membre :</span><span class="sxs-lookup"><span data-stu-id="f2b67-202">If you want to make this reusable across multiple components, consider declaring it as a `public static` member:</span></span>

```razor
public static RenderFragment SayHello = __builder =>
{
    <h1>Hello!</h1>
};
```

<span data-ttu-id="f2b67-203">Cela peut maintenant être appelé à partir d’un composant non lié.</span><span class="sxs-lookup"><span data-stu-id="f2b67-203">This could now be invoked from an unrelated component.</span></span> <span data-ttu-id="f2b67-204">Cette technique est utile pour générer des bibliothèques d’extraits de code réutilisables qui s’affichent sans surcharge par composant.</span><span class="sxs-lookup"><span data-stu-id="f2b67-204">This technique is useful for building libraries of reusable markup snippets that render without any per-component overhead.</span></span>

<span data-ttu-id="f2b67-205"><xref:Microsoft.AspNetCore.Components.RenderFragment> les délégués peuvent également accepter des paramètres.</span><span class="sxs-lookup"><span data-stu-id="f2b67-205"><xref:Microsoft.AspNetCore.Components.RenderFragment> delegates can also accept parameters.</span></span> <span data-ttu-id="f2b67-206">Pour créer l’équivalent du `ChatMessageDisplay` composant à partir de l’exemple précédent :</span><span class="sxs-lookup"><span data-stu-id="f2b67-206">To create the equivalent of the `ChatMessageDisplay` component from the earlier example:</span></span>

```razor
<div class="chat">
    @foreach (var message in messages)
    {
        @DisplayChatMessage(message)
    }
</div>

@code {
    RenderFragment<ChatMessage> DisplayChatMessage = message => __builder =>
    {
        <div class="chat-message">
            <span class="author">@message.Author</span>
            <span class="text">@message.Text</span>
        </div>
    };
}
```

<span data-ttu-id="f2b67-207">Cette approche offre l’avantage de réutiliser la logique de rendu sans surcharge par composant.</span><span class="sxs-lookup"><span data-stu-id="f2b67-207">This approach provides the benefit of reusing rendering logic without per-component overhead.</span></span> <span data-ttu-id="f2b67-208">Toutefois, il n’a pas l’avantage de pouvoir actualiser sa sous-arborescence de l’interface utilisateur de manière indépendante, pas plus qu’il ne peut ignorer le rendu de cette sous-arborescence de l’interface utilisateur lors du rendu de son parent, car il n’y a pas de limite de composant.</span><span class="sxs-lookup"><span data-stu-id="f2b67-208">However, it doesn't have the benefit of being able to refresh its subtree of the UI independently, nor does it have the ability to skip rendering that subtree of the UI when its parent renders, since there's no component boundary.</span></span>

#### <a name="dont-receive-too-many-parameters"></a><span data-ttu-id="f2b67-209">Ne pas recevoir un trop grand nombre de paramètres</span><span class="sxs-lookup"><span data-stu-id="f2b67-209">Don't receive too many parameters</span></span>

<span data-ttu-id="f2b67-210">Si un composant se répète très souvent, par exemple des centaines ou des milliers de fois, gardez à l’esprit que la surcharge liée à la transmission et à la réception de chaque paramètre est générée.</span><span class="sxs-lookup"><span data-stu-id="f2b67-210">If a component repeats extremely often, for example hundreds or thousands of times, then bear in mind that the overhead of passing and receiving each parameter builds up.</span></span>

<span data-ttu-id="f2b67-211">Il est rare qu’un trop grand nombre de paramètres restreint gravement les performances, mais cela peut être un facteur.</span><span class="sxs-lookup"><span data-stu-id="f2b67-211">It's rare that too many parameters severely restricts performance, but it can be a factor.</span></span> <span data-ttu-id="f2b67-212">Pour un `<TableCell>` composant qui restitue 1 000 fois dans une grille, chaque paramètre supplémentaire qui lui est transmis peut ajouter environ 15 ms au coût total de rendu.</span><span class="sxs-lookup"><span data-stu-id="f2b67-212">For a `<TableCell>` component that renders 1,000 times within a grid, each extra parameter passed to it could add around 15 ms to the total rendering cost.</span></span> <span data-ttu-id="f2b67-213">Si chaque cellule a accepté 10 paramètres, le passage de paramètres prend environ 150 MS par rendu de composant et, par conséquent, peut-être 150 000 MS (150 secondes) et, par conséquent, une interface utilisateur laggy.</span><span class="sxs-lookup"><span data-stu-id="f2b67-213">If each cell accepted 10 parameters, parameter passing takes around 150 ms per component render and  thus perhaps 150,000 ms (150 seconds) and on its own cause a laggy UI.</span></span>

<span data-ttu-id="f2b67-214">Pour réduire cette charge, vous pouvez regrouper plusieurs paramètres via des classes personnalisées.</span><span class="sxs-lookup"><span data-stu-id="f2b67-214">To reduce this load, you could bundle together multiple parameters via custom classes.</span></span> <span data-ttu-id="f2b67-215">Par exemple, un `<TableCell>` composant peut accepter les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="f2b67-215">For example, a `<TableCell>` component might accept:</span></span>

```razor
@typeparam TItem

...

@code {
    [Parameter]
    public TItem Data { get; set; }
    
    [Parameter]
    public GridOptions Options { get; set; }
}
```

<span data-ttu-id="f2b67-216">Dans l’exemple précédent, `Data` est différent pour chaque cellule, mais `Options` est commun à tous.</span><span class="sxs-lookup"><span data-stu-id="f2b67-216">In the preceding example, `Data` is different for every cell, but `Options` is common across all of them.</span></span> <span data-ttu-id="f2b67-217">Bien entendu, il peut s’agir d’une amélioration ne permettant pas d’avoir un composant et d’inverser `<TableCell>` sa logique dans le composant parent.</span><span class="sxs-lookup"><span data-stu-id="f2b67-217">Of course, it might be an improvement not to have a `<TableCell>` component and instead inline its logic into the parent component.</span></span>

#### <a name="ensure-cascading-parameters-are-fixed"></a><span data-ttu-id="f2b67-218">S’assurer que les paramètres en cascade sont fixes</span><span class="sxs-lookup"><span data-stu-id="f2b67-218">Ensure cascading parameters are fixed</span></span>

<span data-ttu-id="f2b67-219">Le `<CascadingValue>` composant a un paramètre facultatif appelé `IsFixed` .</span><span class="sxs-lookup"><span data-stu-id="f2b67-219">The `<CascadingValue>` component has an optional parameter called `IsFixed`.</span></span>

 * <span data-ttu-id="f2b67-220">Si la `IsFixed` valeur est `false` (valeur par défaut), chaque destinataire de la valeur en cascade configure un abonnement pour recevoir des notifications de modification.</span><span class="sxs-lookup"><span data-stu-id="f2b67-220">If the `IsFixed` value is `false` (the default), then every recipient of the cascaded value sets up a subscription to receive change notifications.</span></span> <span data-ttu-id="f2b67-221">Dans ce cas, chacune `[CascadingParameter]` d’elles est **beaucoup plus coûteuse** qu’une normale `[Parameter]` en raison du suivi des abonnements.</span><span class="sxs-lookup"><span data-stu-id="f2b67-221">In this case, each each `[CascadingParameter]` is **substantially more expensive** than a regular `[Parameter]` due to the subscription tracking.</span></span>
 * <span data-ttu-id="f2b67-222">Si la `IsFixed` valeur est `true` (par exemple, `<CascadingValue Value="@someValue" IsFixed="true">` ), destinataires reçoit la valeur initiale, mais ne configure *pas* d’abonnement pour recevoir des mises à jour.</span><span class="sxs-lookup"><span data-stu-id="f2b67-222">If the `IsFixed` value is `true` (for example, `<CascadingValue Value="@someValue" IsFixed="true">`), then receipients receive the initial value but do *not* set up any subscription to receive updates.</span></span> <span data-ttu-id="f2b67-223">Dans ce cas, chacune `[CascadingParameter]` est légère et **n’est pas plus coûteuse** qu’une normale `[Parameter]` .</span><span class="sxs-lookup"><span data-stu-id="f2b67-223">In this case, each `[CascadingParameter]` is lightweight and **no more expensive** than a regular `[Parameter]`.</span></span>

<span data-ttu-id="f2b67-224">Par conséquent, dans la mesure du possible, vous devez utiliser `IsFixed="true"` des valeurs en cascade.</span><span class="sxs-lookup"><span data-stu-id="f2b67-224">So wherever possible, you should use `IsFixed="true"` on cascaded values.</span></span> <span data-ttu-id="f2b67-225">Vous pouvez effectuer cette opération chaque fois que la valeur fournie ne change pas au fil du temps.</span><span class="sxs-lookup"><span data-stu-id="f2b67-225">You can do this whenever the value being supplied doesn't change over time.</span></span> <span data-ttu-id="f2b67-226">Dans le modèle commun où un composant passe `this` comme une valeur en cascade, vous devez utiliser `IsFixed="true"` :</span><span class="sxs-lookup"><span data-stu-id="f2b67-226">In the common pattern where a component passes `this` as a cascaded value, you should use `IsFixed="true"`:</span></span>

```razor
<CascadingValue Value="this" IsFixed="true">
    <SomeOtherComponents>
</CascadingValue>
```

<span data-ttu-id="f2b67-227">Cela fait une énorme différence s’il y a un grand nombre d’autres composants qui reçoivent la valeur en cascade.</span><span class="sxs-lookup"><span data-stu-id="f2b67-227">This makes a huge difference if there are a large number of other components that receive the cascaded value.</span></span> <span data-ttu-id="f2b67-228">Pour plus d'informations, consultez <xref:blazor/components/cascading-values-and-parameters>.</span><span class="sxs-lookup"><span data-stu-id="f2b67-228">For more information, see <xref:blazor/components/cascading-values-and-parameters>.</span></span>

#### <a name="avoid-attribute-splatting-with-captureunmatchedvalues"></a><span data-ttu-id="f2b67-229">Éviter la projection d’attributs avec `CaptureUnmatchedValues`</span><span class="sxs-lookup"><span data-stu-id="f2b67-229">Avoid attribute splatting with `CaptureUnmatchedValues`</span></span>

<span data-ttu-id="f2b67-230">Les composants peuvent choisir de recevoir des valeurs de paramètre « sans correspondance » à l’aide de l' <xref:Microsoft.AspNetCore.Components.ParameterAttribute.CaptureUnmatchedValues> indicateur :</span><span class="sxs-lookup"><span data-stu-id="f2b67-230">Components can elect to receive "unmatched" parameter values using the <xref:Microsoft.AspNetCore.Components.ParameterAttribute.CaptureUnmatchedValues> flag:</span></span>

```razor
<div @attributes="OtherAttributes">...</div>

@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public IDictionary<string, object> OtherAttributes { get; set; }
}
```

<span data-ttu-id="f2b67-231">Cette approche permet de passer des attributs supplémentaires arbitraires à l’élément.</span><span class="sxs-lookup"><span data-stu-id="f2b67-231">This approach allows passing through arbitrary additional attributes to the element.</span></span> <span data-ttu-id="f2b67-232">Toutefois, il est également très onéreux, car le convertisseur doit :</span><span class="sxs-lookup"><span data-stu-id="f2b67-232">However, it is also quite expensive because the renderer must:</span></span>

* <span data-ttu-id="f2b67-233">Mettre en correspondance tous les paramètres fournis par rapport au jeu de paramètres connus pour générer un dictionnaire.</span><span class="sxs-lookup"><span data-stu-id="f2b67-233">Match all of the supplied parameters against the set of known parameters to build a dictionary.</span></span>
* <span data-ttu-id="f2b67-234">Effectuer le suivi de la façon dont plusieurs copies du même attribut sont remplacées.</span><span class="sxs-lookup"><span data-stu-id="f2b67-234">Keep track of how multiple copies of the same attribute overwrite each other.</span></span>

<span data-ttu-id="f2b67-235">N’hésitez pas à utiliser <xref:Microsoft.AspNetCore.Components.ParameterAttribute.CaptureUnmatchedValues> des composants non critiques pour les performances, tels que ceux qui ne sont pas fréquemment répétés.</span><span class="sxs-lookup"><span data-stu-id="f2b67-235">Feel free to use <xref:Microsoft.AspNetCore.Components.ParameterAttribute.CaptureUnmatchedValues> on non-performance-critical components, such as ones that are not repeated frequently.</span></span> <span data-ttu-id="f2b67-236">Toutefois, pour les composants qui restituent à l’échelle, tels que les éléments d’une grande liste ou les cellules d’une grille, essayez d’éviter la projection d’attributs.</span><span class="sxs-lookup"><span data-stu-id="f2b67-236">However for components that render at scale, such as each items in a large list or cells in a grid, try to avoid attribute splatting.</span></span>

<span data-ttu-id="f2b67-237">Pour plus d'informations, consultez <xref:blazor/components/index#attribute-splatting-and-arbitrary-parameters>.</span><span class="sxs-lookup"><span data-stu-id="f2b67-237">For more information, see <xref:blazor/components/index#attribute-splatting-and-arbitrary-parameters>.</span></span>

#### <a name="implement-setparametersasync-manually"></a><span data-ttu-id="f2b67-238">Implémenter `SetParametersAsync` manuellement</span><span class="sxs-lookup"><span data-stu-id="f2b67-238">Implement `SetParametersAsync` manually</span></span>

<span data-ttu-id="f2b67-239">L’un des principaux aspects de la surcharge de rendu par composant consiste à écrire des valeurs de paramètre entrantes dans les `[Parameter]` Propriétés.</span><span class="sxs-lookup"><span data-stu-id="f2b67-239">One of the main aspects of the per-component rendering overhead is writing incoming parameter values to the `[Parameter]` properties.</span></span> <span data-ttu-id="f2b67-240">Pour ce faire, le convertisseur doit utiliser la réflexion.</span><span class="sxs-lookup"><span data-stu-id="f2b67-240">The renderer has to use reflection to do this.</span></span> <span data-ttu-id="f2b67-241">Même s’il est un peu optimisé, l’absence de prise en charge de JIT sur le runtime webassembly impose des limites.</span><span class="sxs-lookup"><span data-stu-id="f2b67-241">Even though this is somewhat optimized, the absence of JIT support on the WebAssembly runtime imposes limits.</span></span>

<span data-ttu-id="f2b67-242">Dans certains cas extrêmes, vous souhaiterez peut-être éviter la réflexion et implémenter manuellement votre propre logique de paramétrage de paramètres.</span><span class="sxs-lookup"><span data-stu-id="f2b67-242">In some extreme cases, you may wish to avoid the reflection and implement your own parameter setting logic manually.</span></span> <span data-ttu-id="f2b67-243">Cela peut s’appliquer dans les cas suivants :</span><span class="sxs-lookup"><span data-stu-id="f2b67-243">This may be applicable when:</span></span>

 * <span data-ttu-id="f2b67-244">Vous disposez d’un composant qui effectue un rendu extrêmement fréquent (par exemple, il y a des centaines voire des milliers de copies dans l’interface utilisateur).</span><span class="sxs-lookup"><span data-stu-id="f2b67-244">You have a component that renders extremely often (for example, there are hundreds or thousands of copies of it in the UI).</span></span>
 * <span data-ttu-id="f2b67-245">Il accepte un grand nombre de paramètres.</span><span class="sxs-lookup"><span data-stu-id="f2b67-245">It accepts many parameters.</span></span>
 * <span data-ttu-id="f2b67-246">Vous constatez que la surcharge liée à la réception de paramètres a un impact observable sur la réactivité de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="f2b67-246">You find that the overhead of receiving parameters has an observable impact on UI responsiveness.</span></span>

<span data-ttu-id="f2b67-247">Dans ce cas, vous pouvez remplacer la méthode virtuelle du composant <xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync%2A> et implémenter votre propre logique propre au composant.</span><span class="sxs-lookup"><span data-stu-id="f2b67-247">In these cases, you can override the component's virtual <xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync%2A> method and implement your own component-specific logic.</span></span> <span data-ttu-id="f2b67-248">L’exemple suivant évite délibérément les recherches dans les dictionnaires :</span><span class="sxs-lookup"><span data-stu-id="f2b67-248">The following example deliberately avoids any dictionary lookups:</span></span>

```razor
@code {
    [Parameter]
    public int MessageId { get; set; }

    [Parameter]
    public string Text { get; set; }

    [Parameter]
    public EventCallback<string> TextChanged { get; set; }

    [Parameter]
    public Theme CurrentTheme { get; set; }

    public override Task SetParametersAsync(ParameterView parameters)
    {
        foreach (var parameter in parameters)
        {
            switch (parameter.Name)
            {
                case nameof(MessageId):
                    MessageId = (int)parameter.Value;
                    break;
                case nameof(Text):
                    Text = (string)parameter.Value;
                    break;
                case nameof(TextChanged):
                    TextChanged = (EventCallback<string>)parameter.Value;
                    break;
                case nameof(CurrentTheme):
                    CurrentTheme = (Theme)parameter.Value;
                    break;
                default:
                    throw new ArgumentException($"Unknown parameter: {parameter.Name}");
            }
        }

        return base.SetParametersAsync(ParameterView.Empty);
    }
}
```

<span data-ttu-id="f2b67-249">Dans le code précédent, le retour de la classe de base <xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync%2A> exécute les méthodes de cycle de vie normales sans assigner à nouveau les paramètres.</span><span class="sxs-lookup"><span data-stu-id="f2b67-249">In the preceding code, returning the base class <xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync%2A> runs the normal lifecycle methods without assigning parameters again.</span></span>

<span data-ttu-id="f2b67-250">Comme vous pouvez le voir dans le code précédent, la substitution <xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync%2A> et la fourniture d’une logique personnalisée sont compliquées et fastidieuses, donc nous ne recommandons pas cette approche en général.</span><span class="sxs-lookup"><span data-stu-id="f2b67-250">As you can see in the preceding code, overriding <xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync%2A> and supplying custom logic is complicated and laborious, so we don't recommend this approach in general.</span></span> <span data-ttu-id="f2b67-251">Dans les cas extrêmes, il peut améliorer les performances de rendu de 20-25%, mais cette approche ne doit être prise en compte que dans les scénarios répertoriés plus haut.</span><span class="sxs-lookup"><span data-stu-id="f2b67-251">In extreme cases, it can improve rendering performance by 20-25%, but you should only consider this approach in the scenarios listed earlier.</span></span>

### <a name="dont-trigger-events-too-rapidly"></a><span data-ttu-id="f2b67-252">Ne pas déclencher d’événements trop rapidement</span><span class="sxs-lookup"><span data-stu-id="f2b67-252">Don't trigger events too rapidly</span></span>

<span data-ttu-id="f2b67-253">Certains événements de navigateur se déclenchent très souvent, par exemple `onmousemove` et `onscroll` , ce qui peut déclencher des dizaines ou des centaines de fois par seconde.</span><span class="sxs-lookup"><span data-stu-id="f2b67-253">Some browser events fire extremely frequently, for example `onmousemove` and `onscroll`, which can fire tens or hundreds of times per second.</span></span> <span data-ttu-id="f2b67-254">Dans la plupart des cas, vous n’avez pas besoin d’effectuer des mises à jour de l’interface utilisateur fréquemment.</span><span class="sxs-lookup"><span data-stu-id="f2b67-254">In most cases, you don't need to perform UI updates this frequently.</span></span> <span data-ttu-id="f2b67-255">Si vous essayez de le faire, vous risquez de nuire à la réactivité de l’interface utilisateur ou de consommer un temps processeur excessif.</span><span class="sxs-lookup"><span data-stu-id="f2b67-255">If you try to do so, you may harm UI responsiveness or consume excessive CPU time.</span></span>

<span data-ttu-id="f2b67-256">Au lieu d’utiliser `@onmousemove` des événements natifs ou `@onscroll` , vous préférerez peut-être utiliser l’interopérabilité js pour inscrire un rappel qui se déclenche moins fréquemment.</span><span class="sxs-lookup"><span data-stu-id="f2b67-256">Rather than using native `@onmousemove` or `@onscroll` events, you may prefer to use JS interop to register a callback that fires less frequently.</span></span> <span data-ttu-id="f2b67-257">Par exemple, le composant suivant ( `MyComponent.razor` ) affiche la position de la souris, mais uniquement les mises à jour une fois toutes les 500 ms :</span><span class="sxs-lookup"><span data-stu-id="f2b67-257">For example, the following component (`MyComponent.razor`) displays the position of the mouse but only updates at most once every 500 ms:</span></span>

```razor
@inject IJSRuntime JS
@implements IDisposable

<h1>@message</h1>

<div @ref="myMouseMoveElement" style="border:1px dashed red;height:200px;">
    Move mouse here
</div>

@code {
    ElementReference myMouseMoveElement;
    DotNetObjectReference<MyComponent> selfReference;
    private string message = "Move the mouse in the box";

    [JSInvokable]
    public void HandleMouseMove(int x, int y)
    {
        message = $"Mouse move at {x}, {y}";
        StateHasChanged();
    }

    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender)
        {
            selfReference = DotNetObjectReference.Create(this);
            var minInterval = 500; // Only notify every 500 ms
            await JS.InvokeVoidAsync("onThrottledMouseMove", 
                myMouseMoveElement, selfReference, minInterval);
        }
    }

    public void Dispose() => selfReference?.Dispose();
}
```

<span data-ttu-id="f2b67-258">Le code JavaScript correspondant, qui peut être placé dans la `index.html` page ou chargé en tant que module ES6, inscrit l’écouteur d’événements DOM réel.</span><span class="sxs-lookup"><span data-stu-id="f2b67-258">The corresponding JavaScript code, which can be placed in the `index.html` page or loaded as an ES6 module, registers the actual DOM event listener.</span></span> <span data-ttu-id="f2b67-259">Dans cet exemple, l’écouteur d’événements utilise la [ `throttle` fonction de Lodash](https://lodash.com/docs/4.17.15#throttle) pour limiter le taux d’appels :</span><span class="sxs-lookup"><span data-stu-id="f2b67-259">In this example, the event listener uses [Lodash's `throttle` function](https://lodash.com/docs/4.17.15#throttle) to limit the rate of invocations:</span></span>

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.17.20/lodash.min.js"></script>
<script>
  function onThrottledMouseMove(elem, component, interval) {
    elem.addEventListener('mousemove', _.throttle(e => {
      component.invokeMethodAsync('HandleMouseMove', e.offsetX, e.offsetY);
    }, interval));
  }
</script>
```

<span data-ttu-id="f2b67-260">Cette technique peut être encore plus importante pour Blazor Server , puisque chaque appel d’événement implique la transmission d’un message sur le réseau.</span><span class="sxs-lookup"><span data-stu-id="f2b67-260">This technique can be even more important for Blazor Server, since each event invocation involves delivering a message over the network.</span></span> <span data-ttu-id="f2b67-261">Il est utile pour Blazor WebAssembly , car il peut réduire sensiblement la quantité de travail de rendu.</span><span class="sxs-lookup"><span data-stu-id="f2b67-261">It's valuable for Blazor WebAssembly because it can greatly reduce the amount of rendering work.</span></span>

## <a name="optimize-javascript-interop-speed"></a><span data-ttu-id="f2b67-262">Optimiser la vitesse de l’interopérabilité JavaScript</span><span class="sxs-lookup"><span data-stu-id="f2b67-262">Optimize JavaScript interop speed</span></span>

<span data-ttu-id="f2b67-263">Les appels entre .NET et JavaScript impliquent une surcharge supplémentaire, car :</span><span class="sxs-lookup"><span data-stu-id="f2b67-263">Calls between .NET and JavaScript involve some additional overhead because:</span></span>

 * <span data-ttu-id="f2b67-264">Par défaut, les appels sont asynchrones.</span><span class="sxs-lookup"><span data-stu-id="f2b67-264">By default, calls are asynchronous.</span></span>
 * <span data-ttu-id="f2b67-265">Par défaut, les paramètres et les valeurs de retour sont sérialisés au format JSON.</span><span class="sxs-lookup"><span data-stu-id="f2b67-265">By default, parameters and return values are JSON-serialized.</span></span> <span data-ttu-id="f2b67-266">Cela permet de fournir un mécanisme de conversion facile à comprendre entre les types .NET et JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f2b67-266">This is to provide an easy-to-understand conversion mechanism between .NET and JavaScript types.</span></span>

<span data-ttu-id="f2b67-267">En outre Blazor Server , ces appels sont transmis sur le réseau.</span><span class="sxs-lookup"><span data-stu-id="f2b67-267">Additionally on Blazor Server, these calls are passed across the network.</span></span>

### <a name="avoid-excessively-fine-grained-calls"></a><span data-ttu-id="f2b67-268">Évitez les appels extrêmement précis</span><span class="sxs-lookup"><span data-stu-id="f2b67-268">Avoid excessively fine-grained calls</span></span>

<span data-ttu-id="f2b67-269">Étant donné que chaque appel implique une surcharge, il peut être utile de réduire le nombre d’appels.</span><span class="sxs-lookup"><span data-stu-id="f2b67-269">Since each call involves some overhead, it can be valuable to reduce the number of calls.</span></span> <span data-ttu-id="f2b67-270">Prenons le code suivant, qui stocke une collection d’éléments dans le magasin du navigateur `localStorage` :</span><span class="sxs-lookup"><span data-stu-id="f2b67-270">Consider the following code, which stores a collection of items in the browser's `localStorage` store:</span></span>

```csharp
private async Task StoreAllInLocalStorage(IEnumerable<TodoItem> items)
{
    foreach (var item in items)
    {
        await JS.InvokeVoidAsync("localStorage.setItem", item.Id, 
            JsonSerializer.Serialize(item));
    }
}
```

<span data-ttu-id="f2b67-271">L’exemple précédent effectue un appel d’interopérabilité JS distinct pour chaque élément.</span><span class="sxs-lookup"><span data-stu-id="f2b67-271">The preceding example makes a separate JS interop call for each item.</span></span> <span data-ttu-id="f2b67-272">Au lieu de cela, l’approche suivante réduit l’interopérabilité de JS à un appel unique :</span><span class="sxs-lookup"><span data-stu-id="f2b67-272">Instead, the following approach reduces the JS interop to a single call:</span></span>

```csharp
private async Task StoreAllInLocalStorage(IEnumerable<TodoItem> items)
{
    await JS.InvokeVoidAsync("storeAllInLocalStorage", items);
}
```

<span data-ttu-id="f2b67-273">La fonction JavaScript correspondante est définie comme suit :</span><span class="sxs-lookup"><span data-stu-id="f2b67-273">The corresponding JavaScript function defined as follows:</span></span>

```javascript
function storeAllInLocalStorage(items) {
  items.forEach(item => {
    localStorage.setItem(item.id, JSON.stringify(item));
  });
}
```

<span data-ttu-id="f2b67-274">Pour Blazor WebAssembly , cela est généralement important uniquement si vous effectuez un grand nombre d’appels d’interopérabilité js.</span><span class="sxs-lookup"><span data-stu-id="f2b67-274">For Blazor WebAssembly, this usually only matters if you're making a large number of JS interop calls.</span></span>

### <a name="consider-making-synchronous-calls"></a><span data-ttu-id="f2b67-275">Envisagez d’effectuer des appels synchrones</span><span class="sxs-lookup"><span data-stu-id="f2b67-275">Consider making synchronous calls</span></span>

<span data-ttu-id="f2b67-276">Les appels Interop JavaScript sont asynchrones par défaut, que le code appelé soit synchrone ou asynchrone.</span><span class="sxs-lookup"><span data-stu-id="f2b67-276">JavaScript interop calls are asynchronous by default, regardless of whether the code being called is synchronous or asynchronous.</span></span> <span data-ttu-id="f2b67-277">Cela permet de garantir la compatibilité des composants avec Blazor WebAssembly et Blazor Server .</span><span class="sxs-lookup"><span data-stu-id="f2b67-277">This is to ensure components are compatible with both Blazor WebAssembly and Blazor Server.</span></span> <span data-ttu-id="f2b67-278">Sur Blazor Server , tous les appels de l’interopérabilité JavaScript doivent être asynchrones, car ils sont envoyés via une connexion réseau.</span><span class="sxs-lookup"><span data-stu-id="f2b67-278">On Blazor Server, all JavaScript interop calls must be asynchronous because they are sent over a network connection.</span></span>

<span data-ttu-id="f2b67-279">Si vous savez que votre application n’est jamais exécutée sur Blazor WebAssembly , vous pouvez choisir d’effectuer des appels d’interopérabilité JavaScript synchrones.</span><span class="sxs-lookup"><span data-stu-id="f2b67-279">If you know for certain that your app only ever runs on Blazor WebAssembly, you can choose to make synchronous JavaScript interop calls.</span></span> <span data-ttu-id="f2b67-280">Cela a une surcharge légèrement inférieure à la création d’appels asynchrones et peut entraîner moins de cycles de rendu, car il n’y a pas d’état intermédiaire en attendant les résultats.</span><span class="sxs-lookup"><span data-stu-id="f2b67-280">This has slightly less overhead than making asynchronous calls and can result in fewer render cycles because there is no intermediate state while awaiting results.</span></span>

<span data-ttu-id="f2b67-281">Pour effectuer un appel synchrone de .NET à JavaScript, effectuez un cast <xref:Microsoft.JSInterop.IJSRuntime> en <xref:Microsoft.JSInterop.IJSInProcessRuntime> :</span><span class="sxs-lookup"><span data-stu-id="f2b67-281">To make a synchronous call from .NET to JavaScript, cast <xref:Microsoft.JSInterop.IJSRuntime> to <xref:Microsoft.JSInterop.IJSInProcessRuntime>:</span></span>

```razor
@inject IJSRuntime JS

...

@code {
    protected override void HandleSomeEvent()
    {
        var jsInProcess = (IJSInProcessRuntime)JS;
        var value = jsInProcess.Invoke<string>("javascriptFunctionIdentifier");
    }
}
```

::: moniker range=">= aspnetcore-5.0"

<span data-ttu-id="f2b67-282">Lorsque vous utilisez `IJSObjectReference` , vous pouvez effectuer un appel synchrone en effectuant un cast en `IJSInProcessObjectReference` .</span><span class="sxs-lookup"><span data-stu-id="f2b67-282">When working with `IJSObjectReference`, you can make a synchronous call by casting to `IJSInProcessObjectReference`.</span></span>

::: moniker-end

<span data-ttu-id="f2b67-283">Pour effectuer un appel synchrone de JavaScript à .NET, utilisez à la `DotNet.invokeMethod` place de `DotNet.invokeMethodAsync` .</span><span class="sxs-lookup"><span data-stu-id="f2b67-283">To make a synchronous call from JavaScript to .NET, use `DotNet.invokeMethod` instead of `DotNet.invokeMethodAsync`.</span></span>

<span data-ttu-id="f2b67-284">Les appels synchrones fonctionnent dans les cas suivants :</span><span class="sxs-lookup"><span data-stu-id="f2b67-284">Synchronous calls work if:</span></span>

* <span data-ttu-id="f2b67-285">L’application est en cours d’exécution sur Blazor WebAssembly , et non Blazor Server .</span><span class="sxs-lookup"><span data-stu-id="f2b67-285">The app is running on Blazor WebAssembly, not Blazor Server.</span></span>
* <span data-ttu-id="f2b67-286">La fonction appelée retourne une valeur de façon synchrone (il ne s’agit pas d’une `async` méthode et ne retourne pas de .NET <xref:System.Threading.Tasks.Task> ou JavaScript `Promise` ).</span><span class="sxs-lookup"><span data-stu-id="f2b67-286">The called function returns a value synchronously (it isn't an `async` method and doesn't return a .NET <xref:System.Threading.Tasks.Task> or JavaScript `Promise`).</span></span>

<span data-ttu-id="f2b67-287">Pour plus d'informations, consultez <xref:blazor/call-javascript-from-dotnet>.</span><span class="sxs-lookup"><span data-stu-id="f2b67-287">For more information, see <xref:blazor/call-javascript-from-dotnet>.</span></span>

::: moniker range=">= aspnetcore-5.0"
 
### <a name="consider-making-unmarshalled-calls"></a><span data-ttu-id="f2b67-288">Envisagez d’effectuer des appels démarshalés</span><span class="sxs-lookup"><span data-stu-id="f2b67-288">Consider making unmarshalled calls</span></span>

<span data-ttu-id="f2b67-289">Lors de son exécution sur Blazor WebAssembly , il est possible d’effectuer des appels démarshalés de .net à JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f2b67-289">When running on Blazor WebAssembly, it's possible to make unmarshalled calls from .NET to JavaScript.</span></span> <span data-ttu-id="f2b67-290">Il s’agit d’appels synchrones qui n’effectuent pas de sérialisation JSON des arguments ou des valeurs de retour.</span><span class="sxs-lookup"><span data-stu-id="f2b67-290">These are synchronous calls that don't perform JSON serialization of arguments or return values.</span></span> <span data-ttu-id="f2b67-291">Tous les aspects de la gestion de la mémoire et des traductions entre les représentations .NET et JavaScript sont laissés au développeur.</span><span class="sxs-lookup"><span data-stu-id="f2b67-291">All aspects of memory management and translations between .NET and JavaScript representations are left up to the developer.</span></span>

> [!WARNING]
> <span data-ttu-id="f2b67-292">Bien que l’utilisation `IJSUnmarshalledRuntime` de ait la surcharge la plus faible des approches de l’interopérabilité de js, les API JavaScript requises pour interagir avec ces API ne sont actuellement pas documentées et sujettes à des modifications importantes dans les versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="f2b67-292">While using `IJSUnmarshalledRuntime` has the least overhead of the JS interop approaches, the JavaScript APIs required to interact with these APIs are currently undocumented and subject to breaking changes in future releases.</span></span>

```javascript
function jsInteropCall() {
    return BINDING.js_to_mono_obj("Hello world");
}
```

```razor
@inject IJSRuntime JS

@code {
    protected override void OnInitialized()
    {
        var unmarshalledJs = (IJSUnmarshalledRuntime)JS;
        var value = unmarshalledJs.InvokeUnmarshalled<string>("jsInteropCall");
    }
}
```

::: moniker-end

## <a name="minimize-app-download-size"></a><span data-ttu-id="f2b67-293">Réduire la taille du téléchargement de l’application</span><span class="sxs-lookup"><span data-stu-id="f2b67-293">Minimize app download size</span></span>

::: moniker range=">= aspnetcore-5.0"

### <a name="intermediate-language-il-trimming"></a><span data-ttu-id="f2b67-294">Découpage en langage intermédiaire (IL)</span><span class="sxs-lookup"><span data-stu-id="f2b67-294">Intermediate Language (IL) trimming</span></span>

<span data-ttu-id="f2b67-295">Le [découpage des assemblys inutilisés d’une Blazor WebAssembly application](xref:blazor/host-and-deploy/configure-trimmer) réduit la taille de l’application en supprimant le code inutilisé dans les fichiers binaires de l’application.</span><span class="sxs-lookup"><span data-stu-id="f2b67-295">[Trimming unused assemblies from a Blazor WebAssembly app](xref:blazor/host-and-deploy/configure-trimmer) reduces the app's size by removing unused code in the app's binaries.</span></span> <span data-ttu-id="f2b67-296">Par défaut, le massicot est exécuté lors de la publication d’une application.</span><span class="sxs-lookup"><span data-stu-id="f2b67-296">By default, the Trimmer is executed when publishing an application.</span></span> <span data-ttu-id="f2b67-297">Pour tirer parti de la suppression, publiez l’application pour le déploiement à l’aide de la [`dotnet publish`](/dotnet/core/tools/dotnet-publish) commande avec l’option [-c |--configuration](/dotnet/core/tools/dotnet-publish#options) définie sur `Release` :</span><span class="sxs-lookup"><span data-stu-id="f2b67-297">To benefit from trimming, publish the app for deployment using the [`dotnet publish`](/dotnet/core/tools/dotnet-publish) command with the [-c|--configuration](/dotnet/core/tools/dotnet-publish#options) option set to `Release`:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-5.0"

### <a name="intermediate-language-il-linking"></a><span data-ttu-id="f2b67-298">Liaison en langage intermédiaire (IL)</span><span class="sxs-lookup"><span data-stu-id="f2b67-298">Intermediate Language (IL) linking</span></span>

<span data-ttu-id="f2b67-299">La [liaison d’une Blazor WebAssembly application](xref:blazor/host-and-deploy/configure-linker) réduit la taille de l’application en découpant le code inutilisé dans les fichiers binaires de l’application.</span><span class="sxs-lookup"><span data-stu-id="f2b67-299">[Linking a Blazor WebAssembly app](xref:blazor/host-and-deploy/configure-linker) reduces the app's size by trimming unused code in the app's binaries.</span></span> <span data-ttu-id="f2b67-300">Par défaut, l’éditeur de liens de langage intermédiaire (IL) est activé uniquement lors de la génération de la `Release` Configuration.</span><span class="sxs-lookup"><span data-stu-id="f2b67-300">By default, the Intermediate Language (IL) Linker is only enabled when building in `Release` configuration.</span></span> <span data-ttu-id="f2b67-301">Pour tirer parti de ce, publiez l’application pour le déploiement à l’aide de la [`dotnet publish`](/dotnet/core/tools/dotnet-publish) commande avec l’option [-c |--configuration](/dotnet/core/tools/dotnet-publish#options) définie sur `Release` :</span><span class="sxs-lookup"><span data-stu-id="f2b67-301">To benefit from this, publish the app for deployment using the [`dotnet publish`](/dotnet/core/tools/dotnet-publish) command with the [-c|--configuration](/dotnet/core/tools/dotnet-publish#options) option set to `Release`:</span></span>

::: moniker-end

```dotnetcli
dotnet publish -c Release
```

### <a name="use-systemtextjson"></a><span data-ttu-id="f2b67-302">Utiliser System.Text.Jssur</span><span class="sxs-lookup"><span data-stu-id="f2b67-302">Use System.Text.Json</span></span>

<span data-ttu-id="f2b67-303">Blazorl’implémentation de l’interopérabilité JS de est basée sur <xref:System.Text.Json> , qui est une bibliothèque de SÉRIALISATION JSON haute performance avec une faible allocation de mémoire.</span><span class="sxs-lookup"><span data-stu-id="f2b67-303">Blazor's JS interop implementation relies on <xref:System.Text.Json>, which is a high-performance JSON serialization library with low memory allocation.</span></span> <span data-ttu-id="f2b67-304">L’utilisation <xref:System.Text.Json> de n’entraîne pas de taille de charge utile d’application supplémentaire par rapport à l’ajout d’une ou de plusieurs autres bibliothèques JSON.</span><span class="sxs-lookup"><span data-stu-id="f2b67-304">Using <xref:System.Text.Json> doesn't result in additional app payload size over adding one or more alternate JSON libraries.</span></span>

<span data-ttu-id="f2b67-305">Pour obtenir des conseils sur la migration, consultez [Comment migrer de `Newtonsoft.Json` vers `System.Text.Json` ](/dotnet/standard/serialization/system-text-json-migrate-from-newtonsoft-how-to).</span><span class="sxs-lookup"><span data-stu-id="f2b67-305">For migration guidance, see [How to migrate from `Newtonsoft.Json` to `System.Text.Json`](/dotnet/standard/serialization/system-text-json-migrate-from-newtonsoft-how-to).</span></span>

### <a name="lazy-load-assemblies"></a><span data-ttu-id="f2b67-306">Charger des assemblys en différé</span><span class="sxs-lookup"><span data-stu-id="f2b67-306">Lazy load assemblies</span></span>

<span data-ttu-id="f2b67-307">Chargez les assemblys au moment de l’exécution lorsque les assemblys sont requis par un itinéraire.</span><span class="sxs-lookup"><span data-stu-id="f2b67-307">Load assemblies at runtime when the assemblies are required by a route.</span></span> <span data-ttu-id="f2b67-308">Pour plus d'informations, consultez <xref:blazor/webassembly-lazy-load-assemblies>.</span><span class="sxs-lookup"><span data-stu-id="f2b67-308">For more information, see <xref:blazor/webassembly-lazy-load-assemblies>.</span></span>

### <a name="compression"></a><span data-ttu-id="f2b67-309">Compression</span><span class="sxs-lookup"><span data-stu-id="f2b67-309">Compression</span></span>

<span data-ttu-id="f2b67-310">Quand une Blazor WebAssembly application est publiée, la sortie est compressée de manière statique lors de la publication afin de réduire la taille de l’application et de supprimer la surcharge liée à la compression du Runtime.</span><span class="sxs-lookup"><span data-stu-id="f2b67-310">When a Blazor WebAssembly app is published, the output is statically compressed during publish to reduce the app's size and remove the overhead for runtime compression.</span></span> <span data-ttu-id="f2b67-311">Blazor s’appuie sur le serveur pour effectuer des negotation de contenu et traiter des fichiers compressés statiquement.</span><span class="sxs-lookup"><span data-stu-id="f2b67-311">Blazor relies on the server to perform content negotation and serve statically-compressed files.</span></span>

<span data-ttu-id="f2b67-312">Une fois qu’une application a été déployée, vérifiez que l’application dessert des fichiers compressés.</span><span class="sxs-lookup"><span data-stu-id="f2b67-312">After an app is deployed, verify that the app serves compressed files.</span></span> <span data-ttu-id="f2b67-313">Examinez l’onglet réseau dans le Outils de développement d’un navigateur et vérifiez que les fichiers sont pris en charge avec `Content-Encoding: br` ou `Content-Encoding: gz` .</span><span class="sxs-lookup"><span data-stu-id="f2b67-313">Inspect the Network tab in a browser's Developer Tools and verify that the files are served with `Content-Encoding: br` or `Content-Encoding: gz`.</span></span> <span data-ttu-id="f2b67-314">Si l’hôte ne dessert pas de fichiers compressés, suivez les instructions de la procédure <xref:blazor/host-and-deploy/webassembly#compression> .</span><span class="sxs-lookup"><span data-stu-id="f2b67-314">If the host isn't serving compressed files, follow the instructions in <xref:blazor/host-and-deploy/webassembly#compression>.</span></span>

### <a name="disable-unused-features"></a><span data-ttu-id="f2b67-315">Désactiver les fonctionnalités inutilisées</span><span class="sxs-lookup"><span data-stu-id="f2b67-315">Disable unused features</span></span>

<span data-ttu-id="f2b67-316">Blazor WebAssemblyle runtime de comprend les fonctionnalités .NET suivantes qui peuvent être désactivées si l’application n’en a pas besoin pour une plus petite taille de charge utile :</span><span class="sxs-lookup"><span data-stu-id="f2b67-316">Blazor WebAssembly's runtime includes the following .NET features that can be disabled if the app doesn't require them for a smaller payload size:</span></span>

* <span data-ttu-id="f2b67-317">Un fichier de données est inclus pour corriger les informations de fuseau horaire.</span><span class="sxs-lookup"><span data-stu-id="f2b67-317">A data file is included to make timezone information correct.</span></span> <span data-ttu-id="f2b67-318">Si l’application n’a pas besoin de cette fonctionnalité, envisagez de la désactiver en définissant la `BlazorEnableTimeZoneSupport` propriété MSBuild dans le fichier projet de l’application sur `false` :</span><span class="sxs-lookup"><span data-stu-id="f2b67-318">If the app doesn't require this feature, consider disabling it by setting the `BlazorEnableTimeZoneSupport` MSBuild property in the app's project file to `false`:</span></span>

  ```xml
  <PropertyGroup>
    <BlazorEnableTimeZoneSupport>false</BlazorEnableTimeZoneSupport>
  </PropertyGroup>
  ```

::: moniker range=">= aspnetcore-5.0"

* <span data-ttu-id="f2b67-319">Par défaut, Blazor WebAssembly transporte les ressources de globalisation requises pour afficher des valeurs, telles que les dates et les devises, dans la culture de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="f2b67-319">By default, Blazor WebAssembly carries globalization resources required to display values, such as dates and currency, in the user's culture.</span></span> <span data-ttu-id="f2b67-320">Si l’application ne nécessite pas de localisation, vous pouvez [configurer l’application pour qu’elle prenne en charge la culture dite indifférente](xref:blazor/globalization-localization), qui est basée sur la `en-US` culture :</span><span class="sxs-lookup"><span data-stu-id="f2b67-320">If the app doesn't require localization, you may [configure the app to support the invariant culture](xref:blazor/globalization-localization), which is based on the `en-US` culture:</span></span>

  ```xml
  <PropertyGroup>
    <InvariantGlobalization>true</InvariantGlobalization>
  </PropertyGroup>
  ```

::: moniker-end

::: moniker range="< aspnetcore-5.0"

* <span data-ttu-id="f2b67-321">Les informations de classement sont incluses pour que les API telles que <xref:System.StringComparison.InvariantCultureIgnoreCase?displayProperty=nameWithType> fonctionnent correctement.</span><span class="sxs-lookup"><span data-stu-id="f2b67-321">Collation information is included to make APIs such as <xref:System.StringComparison.InvariantCultureIgnoreCase?displayProperty=nameWithType> work correctly.</span></span> <span data-ttu-id="f2b67-322">Si vous êtes certain que l’application n’a pas besoin des données de classement, envisagez de la désactiver en définissant la `BlazorWebAssemblyPreserveCollationData` propriété MSBuild dans le fichier projet de l’application sur `false` :</span><span class="sxs-lookup"><span data-stu-id="f2b67-322">If you're certain that the app doesn't require the collation data, consider disabling it by setting the `BlazorWebAssemblyPreserveCollationData` MSBuild property in the app's project file to `false`:</span></span>

  ```xml
  <PropertyGroup>
    <BlazorWebAssemblyPreserveCollationData>false</BlazorWebAssemblyPreserveCollationData>
  </PropertyGroup>
  ```

::: moniker-end
