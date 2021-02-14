---
title: ASP.NET Core Blazor les valeurs et paramètres en cascade
author: guardrex
description: Apprenez à transmettre des données d’un composant ancêtre à des composants descendants.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/02/2021
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
uid: blazor/components/cascading-values-and-parameters
ms.openlocfilehash: 1fb9d75ca1613a7098840efd3ecb86ee90f4064c
ms.sourcegitcommit: 1166b0ff3828418559510c661e8240e5c5717bb7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/12/2021
ms.locfileid: "100280242"
---
# <a name="aspnet-core-blazor-cascading-values-and-parameters"></a><span data-ttu-id="b0253-103">ASP.NET Core Blazor les valeurs et paramètres en cascade</span><span class="sxs-lookup"><span data-stu-id="b0253-103">ASP.NET Core Blazor cascading values and parameters</span></span>

<span data-ttu-id="b0253-104">Les *valeurs et les paramètres en cascade* offrent un moyen convienent de transmettre les données vers le niveau d’une hiérarchie de composants à partir d’un composant ancêtre vers un nombre quelconque de composants decendent.</span><span class="sxs-lookup"><span data-stu-id="b0253-104">*Cascading values and parameters* provide a convienent way to flow data down a component hierarchy from an ancestor component to any number of decendent components.</span></span> <span data-ttu-id="b0253-105">Contrairement aux [paramètres de composant](xref:blazor/components/index#component-parameters), les valeurs et paramètres en cascade ne nécessitent pas d’affectation d’attribut pour chaque composant de descendant dans lequel les données sont consommées.</span><span class="sxs-lookup"><span data-stu-id="b0253-105">Unlike [Component parameters](xref:blazor/components/index#component-parameters), cascading values and parameters don't require an attribute assignment for each descendent component where the data is consumed.</span></span> <span data-ttu-id="b0253-106">Les valeurs et les paramètres en cascade permettent également de coordonner les composants entre eux dans une hiérarchie de composants.</span><span class="sxs-lookup"><span data-stu-id="b0253-106">Cascading values and parameters also allow components to coordinate with each other across a component hierarchy.</span></span>

## <a name="cascadingvalue-component"></a><span data-ttu-id="b0253-107">Composant `CascadingValue`</span><span class="sxs-lookup"><span data-stu-id="b0253-107">`CascadingValue` component</span></span>

<span data-ttu-id="b0253-108">Un composant ancêtre fournit une valeur en cascade à l’aide du Blazor composant de l’infrastructure [`CascadingValue`](xref:Microsoft.AspNetCore.Components.CascadingValue%601) , qui encapsule un sous-arbre d’une hiérarchie de composants et fournit une valeur unique à tous les composants de sa sous-arborescence.</span><span class="sxs-lookup"><span data-stu-id="b0253-108">An ancestor component provides a cascading value using the Blazor framework's [`CascadingValue`](xref:Microsoft.AspNetCore.Components.CascadingValue%601) component, which wraps a subtree of a component hierarchy and supplies a single value to all of the components within its subtree.</span></span>

<span data-ttu-id="b0253-109">L’exemple suivant montre le déroulement des informations de thème dans la hiérarchie des composants d’un composant de disposition pour fournir une classe de style CSS aux boutons des composants enfants.</span><span class="sxs-lookup"><span data-stu-id="b0253-109">The following example demonstrates the flow of theme information down the component hierarchy of a layout component to provide a CSS style class to buttons in child components.</span></span>

<span data-ttu-id="b0253-110">La `ThemeInfo` classe C# suivante est placée dans un dossier nommé `UIThemeClasses` et spécifie les informations de thème.</span><span class="sxs-lookup"><span data-stu-id="b0253-110">The following `ThemeInfo` C# class is placed in a folder named `UIThemeClasses` and specifies the theme information.</span></span>

> [!NOTE]
> <span data-ttu-id="b0253-111">Pour les exemples de cette section, l’espace de noms de l’application est `BlazorSample` .</span><span class="sxs-lookup"><span data-stu-id="b0253-111">For the examples in this section, the app's namespace is `BlazorSample`.</span></span> <span data-ttu-id="b0253-112">Quand vous expérimentez le code dans votre propre exemple d’application, remplacez l’espace de noms de l’application par l’espace de noms de votre application d’exemple.</span><span class="sxs-lookup"><span data-stu-id="b0253-112">When experimenting with the code in your own sample app, change the app's namespace to your sample app's namespace.</span></span>

<span data-ttu-id="b0253-113">`UIThemeClasses/ThemeInfo.cs`:</span><span class="sxs-lookup"><span data-stu-id="b0253-113">`UIThemeClasses/ThemeInfo.cs`:</span></span>

```csharp
namespace BlazorSample.UIThemeClasses
{
    public class ThemeInfo
    {
        public string ButtonClass { get; set; }
    }
}
```

<span data-ttu-id="b0253-114">Le [composant de disposition](xref:blazor/layouts) suivant spécifie les informations de thème ( `ThemeInfo` ) comme valeur en cascade pour tous les composants qui composent le corps de la disposition de la <xref:Microsoft.AspNetCore.Components.LayoutComponentBase.Body> propriété.</span><span class="sxs-lookup"><span data-stu-id="b0253-114">The following [layout component](xref:blazor/layouts) specifies theme information (`ThemeInfo`) as a cascading value for all components that make up the layout body of the <xref:Microsoft.AspNetCore.Components.LayoutComponentBase.Body> property.</span></span> <span data-ttu-id="b0253-115">`ButtonClass` la valeur est affectée à [`btn-success`](https://getbootstrap.com/docs/5.0/components/buttons/) , qui est un style de bouton de démarrage.</span><span class="sxs-lookup"><span data-stu-id="b0253-115">`ButtonClass` is assigned a value of [`btn-success`](https://getbootstrap.com/docs/5.0/components/buttons/), which is a Bootstrap button style.</span></span> <span data-ttu-id="b0253-116">Tout composant descendant dans la hiérarchie des composants peut utiliser la `ButtonClass` propriété à travers la `ThemeInfo` valeur en cascade.</span><span class="sxs-lookup"><span data-stu-id="b0253-116">Any descendent component in the component hierarchy can use the `ButtonClass` property through the `ThemeInfo` cascading value.</span></span>

<span data-ttu-id="b0253-117">`Shared/MainLayout.razor`:</span><span class="sxs-lookup"><span data-stu-id="b0253-117">`Shared/MainLayout.razor`:</span></span>

::: moniker range=">= aspnetcore-5.0"

[!code-razor[](~/blazor/common/samples/5.x/BlazorSample_WebAssembly/Shared/MainLayout.razor?highlight=2,10-14,19)]

::: moniker-end

::: moniker range="< aspnetcore-5.0"

[!code-razor[](~/blazor/common/samples/3.x/BlazorSample_WebAssembly/Shared/MainLayout.razor?highlight=2,9-13,17)]

::: moniker-end

## <a name="cascadingparameter-attribute"></a><span data-ttu-id="b0253-118">Attribut `[CascadingParameter]`</span><span class="sxs-lookup"><span data-stu-id="b0253-118">`[CascadingParameter]` attribute</span></span>

<span data-ttu-id="b0253-119">Pour utiliser des valeurs en cascade, les composants descendants déclarent des paramètres en cascade à l’aide de l' [ `[CascadingParameter]` attribut](xref:Microsoft.AspNetCore.Components.CascadingParameterAttribute).</span><span class="sxs-lookup"><span data-stu-id="b0253-119">To make use of cascading values, descendent components declare cascading parameters using the [`[CascadingParameter]` attribute](xref:Microsoft.AspNetCore.Components.CascadingParameterAttribute).</span></span> <span data-ttu-id="b0253-120">Les valeurs en cascade sont liées aux paramètres **en cascade par type**.</span><span class="sxs-lookup"><span data-stu-id="b0253-120">Cascading values are bound to cascading parameters **by type**.</span></span> <span data-ttu-id="b0253-121">Le fait de disposer en cascade de plusieurs valeurs du même type est abordé dans la section [plusieurs valeurs en cascade](#cascade-multiple-values) plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="b0253-121">Cascading multiple values of the same type is covered in the [Cascade multiple values](#cascade-multiple-values) section later in this article.</span></span>

<span data-ttu-id="b0253-122">Le composant suivant lie la `ThemeInfo` valeur en cascade à un paramètre en cascade, en utilisant éventuellement le même nom `ThemeInfo` .</span><span class="sxs-lookup"><span data-stu-id="b0253-122">The following component binds the `ThemeInfo` cascading value to a cascading parameter, optionally using the same name of `ThemeInfo`.</span></span> <span data-ttu-id="b0253-123">Le paramètre est utilisé pour définir la classe CSS pour le **`Increment Counter (Themed)`** bouton.</span><span class="sxs-lookup"><span data-stu-id="b0253-123">The parameter is used to set the CSS class for the **`Increment Counter (Themed)`** button.</span></span>

<span data-ttu-id="b0253-124">`Pages/ThemedCounter.razor`:</span><span class="sxs-lookup"><span data-stu-id="b0253-124">`Pages/ThemedCounter.razor`:</span></span>

::: moniker range=">= aspnetcore-5.0"

[!code-razor[](~/blazor/common/samples/5.x/BlazorSample_WebAssembly/Pages/ThemedCounter.razor?highlight=2,15-17,23-24)]

::: moniker-end

::: moniker range="< aspnetcore-5.0"

[!code-razor[](~/blazor/common/samples/3.x/BlazorSample_WebAssembly/Pages/ThemedCounter.razor?highlight=2,15-17,23-24)]

::: moniker-end

## <a name="cascade-multiple-values"></a><span data-ttu-id="b0253-125">En cascade plusieurs valeurs</span><span class="sxs-lookup"><span data-stu-id="b0253-125">Cascade multiple values</span></span>

<span data-ttu-id="b0253-126">Pour mettre en cascade plusieurs valeurs du même type dans la même sous-arborescence, fournissez une <xref:Microsoft.AspNetCore.Components.CascadingValue%601.Name%2A> chaîne unique à chaque [`CascadingValue`](xref:Microsoft.AspNetCore.Components.CascadingValue%601) composant et à ses [ `[CascadingParameter]` attributs](xref:Microsoft.AspNetCore.Components.CascadingParameterAttribute)correspondants.</span><span class="sxs-lookup"><span data-stu-id="b0253-126">To cascade multiple values of the same type within the same subtree, provide a unique <xref:Microsoft.AspNetCore.Components.CascadingValue%601.Name%2A> string to each [`CascadingValue`](xref:Microsoft.AspNetCore.Components.CascadingValue%601) component and their corresponding [`[CascadingParameter]` attributes](xref:Microsoft.AspNetCore.Components.CascadingParameterAttribute).</span></span>

<span data-ttu-id="b0253-127">Dans l’exemple suivant, deux [`CascadingValue`](xref:Microsoft.AspNetCore.Components.CascadingValue%601) composants montent en cascade différentes instances de `CascadingType` :</span><span class="sxs-lookup"><span data-stu-id="b0253-127">In the following example, two [`CascadingValue`](xref:Microsoft.AspNetCore.Components.CascadingValue%601) components cascade different instances of `CascadingType`:</span></span>

```razor
<CascadingValue Value="@parentCascadeParameter1" Name="CascadeParam1">
    <CascadingValue Value="@ParentCascadeParameter2" Name="CascadeParam2">
        ...
    </CascadingValue>
</CascadingValue>

@code {
    private CascadingType parentCascadeParameter1;

    [Parameter]
    public CascadingType ParentCascadeParameter2 { get; set; }

    ...
}
```

<span data-ttu-id="b0253-128">Dans un composant descendant, les paramètres en cascade reçoivent leurs valeurs en cascade du composant ancêtre en <xref:Microsoft.AspNetCore.Components.CascadingValue%601.Name%2A> :</span><span class="sxs-lookup"><span data-stu-id="b0253-128">In a descendant component, the cascaded parameters receive their cascaded values from the ancestor component by <xref:Microsoft.AspNetCore.Components.CascadingValue%601.Name%2A>:</span></span>

```razor
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected CascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected CascadingType ChildCascadeParameter2 { get; set; }
}
```

## <a name="pass-data-across-a-component-hierarchy"></a><span data-ttu-id="b0253-129">Transmettre des données dans une hiérarchie de composants</span><span class="sxs-lookup"><span data-stu-id="b0253-129">Pass data across a component hierarchy</span></span>

<span data-ttu-id="b0253-130">Les paramètres en cascade permettent également aux composants de transmettre des données à travers une hiérarchie de composants.</span><span class="sxs-lookup"><span data-stu-id="b0253-130">Cascading parameters also enable components to pass data across a component hierarchy.</span></span> <span data-ttu-id="b0253-131">Prenons l’exemple de jeu d’onglets de l’interface utilisateur suivant, où un composant de jeu d’onglets gère une série d’onglets individuels.</span><span class="sxs-lookup"><span data-stu-id="b0253-131">Consider the following UI tab set example, where a tab set component maintains a series of individual tabs.</span></span>

> [!NOTE]
> <span data-ttu-id="b0253-132">Pour les exemples de cette section, l’espace de noms de l’application est `BlazorSample` .</span><span class="sxs-lookup"><span data-stu-id="b0253-132">For the examples in this section, the app's namespace is `BlazorSample`.</span></span> <span data-ttu-id="b0253-133">Quand vous expérimentez le code dans votre propre exemple d’application, remplacez l’espace de noms par l’espace de noms de votre application d’exemple.</span><span class="sxs-lookup"><span data-stu-id="b0253-133">When experimenting with the code in your own sample app, change the namespace to your sample app's namespace.</span></span>

<span data-ttu-id="b0253-134">Créez une `ITab` interface que les onglets implémentent dans un dossier nommé `UIInterfaces` .</span><span class="sxs-lookup"><span data-stu-id="b0253-134">Create an `ITab` interface that tabs implement in a folder named `UIInterfaces`.</span></span>

<span data-ttu-id="b0253-135">`UIInterfaces/ITab.cs`:</span><span class="sxs-lookup"><span data-stu-id="b0253-135">`UIInterfaces/ITab.cs`:</span></span>

```csharp
using Microsoft.AspNetCore.Components;

namespace BlazorSample.UIInterfaces
{
    public interface ITab
    {
        RenderFragment ChildContent { get; }
    }
}
```

<span data-ttu-id="b0253-136">Le `TabSet` composant suivant gère un ensemble d’onglets.</span><span class="sxs-lookup"><span data-stu-id="b0253-136">The following `TabSet` component maintains a set of tabs.</span></span> <span data-ttu-id="b0253-137">Les composants du jeu d’onglets `Tab` , qui sont créés plus loin dans cette section, fournissent les éléments de liste ( `<li>...</li>` ) de la liste ( `<ul>...</ul>` ).</span><span class="sxs-lookup"><span data-stu-id="b0253-137">The tab set's `Tab` components, which are created later in this section, supply the list items (`<li>...</li>`) for the list (`<ul>...</ul>`).</span></span>

<span data-ttu-id="b0253-138">`Tab`Les composants enfants ne sont pas explicitement passés comme paramètres à `TabSet` .</span><span class="sxs-lookup"><span data-stu-id="b0253-138">Child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="b0253-139">Au lieu de cela, les `Tab` composants enfants font partie du contenu enfant de `TabSet` .</span><span class="sxs-lookup"><span data-stu-id="b0253-139">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="b0253-140">Toutefois, le `TabSet` nécessite toujours une référence `Tab` à chaque composant pour pouvoir afficher les en-têtes et l’onglet actif. Pour activer cette coordination sans nécessiter de code supplémentaire, le `TabSet` composant *peut se présenter comme une valeur en cascade* qui est ensuite récupérée par les `Tab` composants descendants.</span><span class="sxs-lookup"><span data-stu-id="b0253-140">However, the `TabSet` still needs a reference each `Tab` component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="b0253-141">`Shared/TabSet.razor`:</span><span class="sxs-lookup"><span data-stu-id="b0253-141">`Shared/TabSet.razor`:</span></span>

```razor
@using BlazorSample.UIInterfaces

<!-- Display the tab headers -->

<CascadingValue Value=this>
    <ul class="nav nav-tabs">
        @ChildContent
    </ul>
</CascadingValue>

<!-- Display body for only the active tab -->

<div class="nav-tabs-body p-4">
    @ActiveTab?.ChildContent
</div>

@code {
    [Parameter]
    public RenderFragment ChildContent { get; set; }

    public ITab ActiveTab { get; private set; }

    public void AddTab(ITab tab)
    {
        if (ActiveTab == null)
        {
            SetActiveTab(tab);
        }
    }

    public void SetActiveTab(ITab tab)
    {
        if (ActiveTab != tab)
        {
            ActiveTab = tab;
            StateHasChanged();
        }
    }
}
```

<span data-ttu-id="b0253-142">`Tab`Les composants descendants capturent le conteneur `TabSet` sous la forme d’un paramètre en cascade.</span><span class="sxs-lookup"><span data-stu-id="b0253-142">Descendent `Tab` components capture the containing `TabSet` as a cascading parameter.</span></span> <span data-ttu-id="b0253-143">Les `Tab` composants s’ajoutent à la `TabSet` coordonnée et pour définir l’onglet actif.</span><span class="sxs-lookup"><span data-stu-id="b0253-143">The `Tab` components add themselves to the `TabSet` and coordinate to set the active tab.</span></span>

<span data-ttu-id="b0253-144">`Shared/Tab.razor`:</span><span class="sxs-lookup"><span data-stu-id="b0253-144">`Shared/Tab.razor`:</span></span>

```razor
@using BlazorSample.UIInterfaces
@implements ITab

<li>
    <a @onclick="ActivateTab" class="nav-link @TitleCssClass" role="button">
        @Title
    </a>
</li>

@code {
    [CascadingParameter]
    public TabSet ContainerTabSet { get; set; }

    [Parameter]
    public string Title { get; set; }

    [Parameter]
    public RenderFragment ChildContent { get; set; }

    private string TitleCssClass => 
        ContainerTabSet.ActiveTab == this ? "active" : null;

    protected override void OnInitialized()
    {
        ContainerTabSet.AddTab(this);
    }

    private void ActivateTab()
    {
        ContainerTabSet.SetActiveTab(this);
    }
}
```

<span data-ttu-id="b0253-145">Le `ExampleTabSet` composant suivant utilise le `TabSet` composant, qui contient trois `Tab` composants.</span><span class="sxs-lookup"><span data-stu-id="b0253-145">The following `ExampleTabSet` component uses the `TabSet` component, which contains three `Tab` components.</span></span>

<span data-ttu-id="b0253-146">`Pages/ExampleTabSet.razor`:</span><span class="sxs-lookup"><span data-stu-id="b0253-146">`Pages/ExampleTabSet.razor`:</span></span>

```razor
@page "/example-tab-set"

<TabSet>
    <Tab Title="First tab">
        <h4>Greetings from the first tab!</h4>

        <label>
            <input type="checkbox" @bind="showThirdTab" />
            Toggle third tab
        </label>
    </Tab>

    <Tab Title="Second tab">
        <h4>Hello from the second tab!</h4>
    </Tab>

    @if (showThirdTab)
    {
        <Tab Title="Third tab">
            <h4>Welcome to the disappearing third tab!</h4>
            <p>Toggle this tab from the first tab.</p>
        </Tab>
    }
</TabSet>

@code {
    private bool showThirdTab;
}
```
