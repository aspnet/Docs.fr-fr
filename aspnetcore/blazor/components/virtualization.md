---
title: BlazorVirtualisation des composants ASP.net Core
author: guardrex
description: Découvrez comment utiliser la virtualisation de composants dans des Blazor applications ASP.net core.
monikerRange: '>= aspnetcore-5.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/02/2020
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
uid: blazor/components/virtualization
ms.openlocfilehash: 706564bb8607d0bb25c092c31a72e5790c825ee4
ms.sourcegitcommit: 8b0e9a72c1599ce21830c843558a661ba908ce32
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98024676"
---
# <a name="aspnet-core-no-locblazor-component-virtualization"></a><span data-ttu-id="8ca12-103">BlazorVirtualisation des composants ASP.net Core</span><span class="sxs-lookup"><span data-stu-id="8ca12-103">ASP.NET Core Blazor component virtualization</span></span>

<span data-ttu-id="8ca12-104">Par [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="8ca12-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="8ca12-105">Améliorez les performances perçues du rendu des composants à l’aide de la Blazor prise en charge intégrée de la virtualisation du Framework.</span><span class="sxs-lookup"><span data-stu-id="8ca12-105">Improve the perceived performance of component rendering using the Blazor framework's built-in virtualization support.</span></span> <span data-ttu-id="8ca12-106">La virtualisation est une technique qui permet de limiter le rendu de l’interface utilisateur uniquement aux parties qui sont actuellement visibles.</span><span class="sxs-lookup"><span data-stu-id="8ca12-106">Virtualization is a technique for limiting UI rendering to just the parts that are currently visible.</span></span> <span data-ttu-id="8ca12-107">Par exemple, la virtualisation est utile lorsque l’application doit afficher une longue liste d’éléments et que seul un sous-ensemble d’éléments doit être visible à un moment donné.</span><span class="sxs-lookup"><span data-stu-id="8ca12-107">For example, virtualization is helpful when the app must render a long list of items and only a subset of items is required to be visible at any given time.</span></span> <span data-ttu-id="8ca12-108">Blazorfournit le [ `Virtualize` composant](xref:Microsoft.AspNetCore.Components.Web.Virtualization.Virtualize%601) qui peut être utilisé pour ajouter la virtualisation aux composants d’une application.</span><span class="sxs-lookup"><span data-stu-id="8ca12-108">Blazor provides the [`Virtualize` component](xref:Microsoft.AspNetCore.Components.Web.Virtualization.Virtualize%601) that can be used to add virtualization to an app's components.</span></span>

<span data-ttu-id="8ca12-109">Le `Virtualize` composant peut être utilisé dans les cas suivants :</span><span class="sxs-lookup"><span data-stu-id="8ca12-109">The `Virtualize` component can be used when:</span></span>

* <span data-ttu-id="8ca12-110">Rendu d’un ensemble d’éléments de données dans une boucle.</span><span class="sxs-lookup"><span data-stu-id="8ca12-110">Rendering a set of data items in a loop.</span></span>
* <span data-ttu-id="8ca12-111">La plupart des éléments ne sont pas visibles en raison du défilement.</span><span class="sxs-lookup"><span data-stu-id="8ca12-111">Most of the items aren't visible due to scrolling.</span></span>
* <span data-ttu-id="8ca12-112">Les éléments rendus ont exactement la même taille.</span><span class="sxs-lookup"><span data-stu-id="8ca12-112">The rendered items are exactly the same size.</span></span> <span data-ttu-id="8ca12-113">Lorsque l’utilisateur fait défiler jusqu’à un point arbitraire, le composant peut calculer les éléments visibles à afficher.</span><span class="sxs-lookup"><span data-stu-id="8ca12-113">When the user scrolls to an arbitrary point, the component can calculate the visible items to show.</span></span>

<span data-ttu-id="8ca12-114">Sans virtualisation, une liste standard peut utiliser une boucle C# [`foreach`](/dotnet/csharp/language-reference/keywords/foreach-in) pour afficher chaque élément de la liste :</span><span class="sxs-lookup"><span data-stu-id="8ca12-114">Without virtualization, a typical list might use a C# [`foreach`](/dotnet/csharp/language-reference/keywords/foreach-in) loop to render each item in the list:</span></span>

```razor
<div class="all-flights" style="height:500px;overflow-y:scroll">
    @foreach (var flight in allFlights)
    {
        <FlightSummary @key="flight.FlightId" Flight="@flight" />
    }
</div>
```

<span data-ttu-id="8ca12-115">Si la liste contient des milliers d’éléments, le rendu de la liste peut prendre beaucoup de temps.</span><span class="sxs-lookup"><span data-stu-id="8ca12-115">If the list contains thousands of items, then rendering the list may take a long time.</span></span> <span data-ttu-id="8ca12-116">L’utilisateur peut rencontrer un décalage de l’interface utilisateur notable.</span><span class="sxs-lookup"><span data-stu-id="8ca12-116">The user may experience a noticeable UI lag.</span></span>

<span data-ttu-id="8ca12-117">Au lieu de restituer tous les éléments de la liste en même temps, remplacez la [`foreach`](/dotnet/csharp/language-reference/keywords/foreach-in) boucle par le `Virtualize` composant et spécifiez une source d’élément fixe avec <xref:Microsoft.AspNetCore.Components.Web.Virtualization.Virtualize%601.Items%2A?displayProperty=nameWithType> .</span><span class="sxs-lookup"><span data-stu-id="8ca12-117">Instead of rendering each item in the list all at one time, replace the [`foreach`](/dotnet/csharp/language-reference/keywords/foreach-in) loop with the `Virtualize` component and specify a fixed item source with <xref:Microsoft.AspNetCore.Components.Web.Virtualization.Virtualize%601.Items%2A?displayProperty=nameWithType>.</span></span> <span data-ttu-id="8ca12-118">Seuls les éléments qui sont actuellement visibles sont rendus :</span><span class="sxs-lookup"><span data-stu-id="8ca12-118">Only the items that are currently visible are rendered:</span></span>

```razor
<div class="all-flights" style="height:500px;overflow-y:scroll">
    <Virtualize Items="@allFlights" Context="flight">
        <FlightSummary @key="flight.FlightId" Details="@flight.Summary" />
    </Virtualize>
</div>
```

<span data-ttu-id="8ca12-119">Si vous ne spécifiez pas de contexte pour le composant avec `Context` , utilisez la `context` valeur ( `context.{PROPERTY}` / `@context.{PROPERTY}` ) dans le modèle de contenu de l’élément :</span><span class="sxs-lookup"><span data-stu-id="8ca12-119">If not specifying a context to the component with `Context`, use the `context` value (`context.{PROPERTY}`/`@context.{PROPERTY}`) in the item content template:</span></span>

```razor
<div class="all-flights" style="height:500px;overflow-y:scroll">
    <Virtualize Items="@allFlights">
        <FlightSummary @key="context.FlightId" Details="@context.Summary" />
    </Virtualize>
</div>
```

> [!NOTE]
> <span data-ttu-id="8ca12-120">Le processus de mappage des objets de modèle aux éléments et aux composants peut être contrôlé à l’aide de l' `@key` attribut de directive [] [XREF : MVC/views/Razor # clé].</span><span class="sxs-lookup"><span data-stu-id="8ca12-120">The mapping process of model objects to elements and components can be controlled with the [`@key`][xref:mvc/views/razor#key] directive attribute.</span></span> <span data-ttu-id="8ca12-121">`@key` force l’algorithme de comparaison à garantir la préservation des éléments ou des composants en fonction de la valeur de la clé.</span><span class="sxs-lookup"><span data-stu-id="8ca12-121">`@key` causes the diffing algorithm to guarantee preservation of elements or components based on the key's value.</span></span>
>
> <span data-ttu-id="8ca12-122">Pour plus d’informations, consultez les articles suivants :</span><span class="sxs-lookup"><span data-stu-id="8ca12-122">For more information, see the following articles:</span></span>
>
> <xref:blazor/components/index#use-key-to-control-the-preservation-of-elements-and-components>
> <xref:mvc/views/razor#key>

<span data-ttu-id="8ca12-123">Le `Virtualize` composant :</span><span class="sxs-lookup"><span data-stu-id="8ca12-123">The `Virtualize` component:</span></span>

* <span data-ttu-id="8ca12-124">Calcule le nombre d’éléments à afficher en fonction de la hauteur du conteneur et de la taille des éléments rendus.</span><span class="sxs-lookup"><span data-stu-id="8ca12-124">Calculates how many items to render based on the height of the container and the size of the rendered items.</span></span>
* <span data-ttu-id="8ca12-125">Recalcule et restitue les éléments à mesure que l’utilisateur fait défiler.</span><span class="sxs-lookup"><span data-stu-id="8ca12-125">Recalculates and rerenders items as the user scrolls.</span></span>
* <span data-ttu-id="8ca12-126">Extrait uniquement la tranche d’enregistrements d’une API externe qui correspond à la région visible actuelle, au lieu de télécharger toutes les données de la collection.</span><span class="sxs-lookup"><span data-stu-id="8ca12-126">Only fetches the slice of records from an external API that correspond to the current visible region, instead of downloading all of the data from the collection.</span></span>

<span data-ttu-id="8ca12-127">Le contenu de l’élément pour le `Virtualize` composant peut inclure les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="8ca12-127">The item content for the `Virtualize` component can include:</span></span>

* <span data-ttu-id="8ca12-128">Code HTML et Razor code brut, comme le montre l’exemple précédent.</span><span class="sxs-lookup"><span data-stu-id="8ca12-128">Plain HTML and Razor code, as the preceding example shows.</span></span>
* <span data-ttu-id="8ca12-129">Un ou plusieurs Razor composants.</span><span class="sxs-lookup"><span data-stu-id="8ca12-129">One or more Razor components.</span></span>
* <span data-ttu-id="8ca12-130">Combinaison de composants HTML/ Razor et Razor .</span><span class="sxs-lookup"><span data-stu-id="8ca12-130">A mix of HTML/Razor and Razor components.</span></span>

## <a name="item-provider-delegate"></a><span data-ttu-id="8ca12-131">Délégué du fournisseur d’éléments</span><span class="sxs-lookup"><span data-stu-id="8ca12-131">Item provider delegate</span></span>

<span data-ttu-id="8ca12-132">Si vous ne souhaitez pas charger tous les éléments dans la mémoire, vous pouvez spécifier une méthode déléguée du fournisseur items pour le paramètre du composant <xref:Microsoft.AspNetCore.Components.Web.Virtualization.Virtualize%601.ItemsProvider%2A?displayProperty=nameWithType> qui récupère de manière asynchrone les éléments demandés à la demande :</span><span class="sxs-lookup"><span data-stu-id="8ca12-132">If you don't want to load all of the items into memory, you can specify an items provider delegate method to the component's <xref:Microsoft.AspNetCore.Components.Web.Virtualization.Virtualize%601.ItemsProvider%2A?displayProperty=nameWithType> parameter that asynchronously retrieves the requested items on demand:</span></span>

```razor
<Virtualize Context="employee" ItemsProvider="@LoadEmployees">
    <p>
        @employee.FirstName @employee.LastName has the 
        job title of @employee.JobTitle.
    </p>
</Virtualize>
```

<span data-ttu-id="8ca12-133">Le fournisseur items reçoit un <xref:Microsoft.AspNetCore.Components.Web.Virtualization.ItemsProviderRequest> , qui spécifie le nombre d’éléments requis commençant à un index de début spécifique.</span><span class="sxs-lookup"><span data-stu-id="8ca12-133">The items provider receives an <xref:Microsoft.AspNetCore.Components.Web.Virtualization.ItemsProviderRequest>, which specifies the required number of items starting at a specific start index.</span></span> <span data-ttu-id="8ca12-134">Le fournisseur d’éléments récupère ensuite les éléments demandés à partir d’une base de données ou d’un autre service et les retourne sous la forme d’un, ainsi que du <xref:Microsoft.AspNetCore.Components.Web.Virtualization.ItemsProviderResult%601> nombre total d’éléments.</span><span class="sxs-lookup"><span data-stu-id="8ca12-134">The items provider then retrieves the requested items from a database or other service and returns them as an <xref:Microsoft.AspNetCore.Components.Web.Virtualization.ItemsProviderResult%601> along with a count of the total items.</span></span> <span data-ttu-id="8ca12-135">Le fournisseur d’éléments peut choisir de récupérer les éléments avec chaque demande ou de les mettre en cache afin qu’ils soient facilement disponibles.</span><span class="sxs-lookup"><span data-stu-id="8ca12-135">The items provider can choose to retrieve the items with each request or cache them so that they're readily available.</span></span>

<span data-ttu-id="8ca12-136">Un `Virtualize` composant ne peut accepter qu' **une seule source d’élément** à partir de ses paramètres. par conséquent, n’essayez pas d’utiliser simultanément un fournisseur d’éléments et d’affecter une collection à `Items` .</span><span class="sxs-lookup"><span data-stu-id="8ca12-136">A `Virtualize` component can only accept **one item source** from its parameters, so don't attempt to simultaneously use an items provider and assign a collection to `Items`.</span></span> <span data-ttu-id="8ca12-137">Si les deux sont assignés, une <xref:System.InvalidOperationException> exception est levée lorsque les paramètres du composant sont définis au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="8ca12-137">If both are assigned, an <xref:System.InvalidOperationException> is thrown when the component's parameters are set at runtime.</span></span>

<span data-ttu-id="8ca12-138">L’exemple suivant charge des employés à partir d’un `EmployeeService` :</span><span class="sxs-lookup"><span data-stu-id="8ca12-138">The following example loads employees from an `EmployeeService`:</span></span>

```csharp
private async ValueTask<ItemsProviderResult<Employee>> LoadEmployees(
    ItemsProviderRequest request)
{
    var numEmployees = Math.Min(request.Count, totalEmployees - request.StartIndex);
    var employees = await EmployeesService.GetEmployeesAsync(request.StartIndex, 
        numEmployees, request.CancellationToken);

    return new ItemsProviderResult<Employee>(employees, totalEmployees);
}
```

<span data-ttu-id="8ca12-139"><xref:Microsoft.AspNetCore.Components.Web.Virtualization.Virtualize%601.RefreshDataAsync%2A?displayProperty=nameWithType> indique au composant de redemander des données à partir de son <xref:Microsoft.AspNetCore.Components.Web.Virtualization.Virtualize%601.ItemsProvider%2A> .</span><span class="sxs-lookup"><span data-stu-id="8ca12-139"><xref:Microsoft.AspNetCore.Components.Web.Virtualization.Virtualize%601.RefreshDataAsync%2A?displayProperty=nameWithType> instructs the component to rerequest data from its <xref:Microsoft.AspNetCore.Components.Web.Virtualization.Virtualize%601.ItemsProvider%2A>.</span></span> <span data-ttu-id="8ca12-140">Cela est utile lorsque des données externes sont modifiées.</span><span class="sxs-lookup"><span data-stu-id="8ca12-140">This is useful when external data changes.</span></span> <span data-ttu-id="8ca12-141">Il n’est pas nécessaire d’appeler cette valeur lors de l’utilisation de <xref:Microsoft.AspNetCore.Components.Web.Virtualization.Virtualize%601.Items%2A> .</span><span class="sxs-lookup"><span data-stu-id="8ca12-141">There's no need to call this when using <xref:Microsoft.AspNetCore.Components.Web.Virtualization.Virtualize%601.Items%2A>.</span></span>

## <a name="placeholder"></a><span data-ttu-id="8ca12-142">Espace réservé</span><span class="sxs-lookup"><span data-stu-id="8ca12-142">Placeholder</span></span>

<span data-ttu-id="8ca12-143">Étant donné que la demande d’éléments à partir d’une source de données distante peut prendre un certain temps, vous avez la possibilité d’afficher un espace réservé avec le contenu de l’élément :</span><span class="sxs-lookup"><span data-stu-id="8ca12-143">Because requesting items from a remote data source might take some time, you have the option to render a placeholder  with item content:</span></span>

* <span data-ttu-id="8ca12-144">Utilisez un <xref:Microsoft.AspNetCore.Components.Web.Virtualization.Virtualize%601.Placeholder%2A> ( `<Placeholder>...</Placeholder>` ) pour afficher le contenu jusqu’à ce que les données de l’élément soient disponibles.</span><span class="sxs-lookup"><span data-stu-id="8ca12-144">Use a <xref:Microsoft.AspNetCore.Components.Web.Virtualization.Virtualize%601.Placeholder%2A> (`<Placeholder>...</Placeholder>`) to display content until the item data is available.</span></span>
* <span data-ttu-id="8ca12-145">Utilisez <xref:Microsoft.AspNetCore.Components.Web.Virtualization.Virtualize%601.ItemContent%2A?displayProperty=nameWithType> pour définir le modèle d’élément pour la liste.</span><span class="sxs-lookup"><span data-stu-id="8ca12-145">Use <xref:Microsoft.AspNetCore.Components.Web.Virtualization.Virtualize%601.ItemContent%2A?displayProperty=nameWithType> to set the item template for the list.</span></span>

```razor
<Virtualize Context="employee" ItemsProvider="@LoadEmployees">
    <ItemContent>
        <p>
            @employee.FirstName @employee.LastName has the 
            job title of @employee.JobTitle.
        </p>
    </ItemContent>
    <Placeholder>
        <p>
            Loading&hellip;
        </p>
    </Placeholder>
</Virtualize>
```

## <a name="item-size"></a><span data-ttu-id="8ca12-146">Taille de l’élément</span><span class="sxs-lookup"><span data-stu-id="8ca12-146">Item size</span></span>

<span data-ttu-id="8ca12-147">La taille de chaque élément en pixels peut être définie avec <xref:Microsoft.AspNetCore.Components.Web.Virtualization.Virtualize%601.ItemSize%2A?displayProperty=nameWithType> (valeur par défaut : 50px) :</span><span class="sxs-lookup"><span data-stu-id="8ca12-147">The size of each item in pixels can be set with <xref:Microsoft.AspNetCore.Components.Web.Virtualization.Virtualize%601.ItemSize%2A?displayProperty=nameWithType> (default: 50px):</span></span>

```razor
<Virtualize Context="employee" Items="@employees" ItemSize="25">
    ...
</Virtualize>
```

## <a name="overscan-count"></a><span data-ttu-id="8ca12-148">Nombre de suranalyses</span><span class="sxs-lookup"><span data-stu-id="8ca12-148">Overscan count</span></span>

<span data-ttu-id="8ca12-149"><xref:Microsoft.AspNetCore.Components.Web.Virtualization.Virtualize%601.OverscanCount%2A?displayProperty=nameWithType> détermine le nombre d’éléments supplémentaires qui sont rendus avant et après la zone visible.</span><span class="sxs-lookup"><span data-stu-id="8ca12-149"><xref:Microsoft.AspNetCore.Components.Web.Virtualization.Virtualize%601.OverscanCount%2A?displayProperty=nameWithType> determines how many additional items are rendered before and after the visible region.</span></span> <span data-ttu-id="8ca12-150">Ce paramètre permet de réduire la fréquence de rendu lors du défilement.</span><span class="sxs-lookup"><span data-stu-id="8ca12-150">This setting helps to reduce the frequency of rendering during scrolling.</span></span> <span data-ttu-id="8ca12-151">Toutefois, des valeurs plus élevées entraînent le rendu d’un plus grand nombre d’éléments dans la page (valeur par défaut : 3) :</span><span class="sxs-lookup"><span data-stu-id="8ca12-151">However, higher values result in more elements rendered in the page (default: 3):</span></span>

```razor
<Virtualize Context="employee" Items="@employees" OverscanCount="4">
    ...
</Virtualize>
```

## <a name="state-changes"></a><span data-ttu-id="8ca12-152">Modifications d'état</span><span class="sxs-lookup"><span data-stu-id="8ca12-152">State changes</span></span>

<span data-ttu-id="8ca12-153">Lorsque vous apportez des modifications aux éléments restitués par le `Virtualize` composant, appelez <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A> pour forcer la réévaluation et le rerendu du composant.</span><span class="sxs-lookup"><span data-stu-id="8ca12-153">When making changes to items rendered by the `Virtualize` component, call <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A> to force re-evaluation and rerendering of the component.</span></span>
