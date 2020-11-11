---
title: 'Créer et utiliser des :::no-loc(Razor)::: composants ASP.net Core'
author: guardrex
description: 'Découvrez comment créer et utiliser des :::no-loc(Razor)::: composants, notamment comment lier des données, gérer des événements et gérer des cycles de vie de composant.'
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/09/2020
no-loc:
- ':::no-loc(appsettings.json):::'
- ':::no-loc(ASP.NET Core Identity):::'
- ':::no-loc(cookie):::'
- ':::no-loc(Cookie):::'
- ':::no-loc(Blazor):::'
- ':::no-loc(Blazor Server):::'
- ':::no-loc(Blazor WebAssembly):::'
- ':::no-loc(Identity):::'
- ":::no-loc(Let's Encrypt):::"
- ':::no-loc(Razor):::'
- ':::no-loc(SignalR):::'
uid: blazor/components/index
ms.openlocfilehash: d78076eb29d6d09756e408b388fcf12b4b6460f6
ms.sourcegitcommit: 1be547564381873fe9e84812df8d2088514c622a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/11/2020
ms.locfileid: "94507939"
---
# <a name="create-and-use-aspnet-core-no-locrazor-components"></a><span data-ttu-id="719db-103">Créer et utiliser des :::no-loc(Razor)::: composants ASP.net Core</span><span class="sxs-lookup"><span data-stu-id="719db-103">Create and use ASP.NET Core :::no-loc(Razor)::: components</span></span>

<span data-ttu-id="719db-104">Par [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27)et [Tobias Bartsch](https://www.aveo-solutions.com/)</span><span class="sxs-lookup"><span data-stu-id="719db-104">By [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), and [Tobias Bartsch](https://www.aveo-solutions.com/)</span></span>

<span data-ttu-id="719db-105">[Afficher ou télécharger l’exemple de code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="719db-105">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="719db-106">:::no-loc(Blazor)::: les applications sont générées à l’aide de *composants*.</span><span class="sxs-lookup"><span data-stu-id="719db-106">:::no-loc(Blazor)::: apps are built using *components*.</span></span> <span data-ttu-id="719db-107">Un composant est un bloc d’interface utilisateur (IU) autonome, tel qu’une page, une boîte de dialogue ou un formulaire.</span><span class="sxs-lookup"><span data-stu-id="719db-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="719db-108">Un composant comprend le balisage HTML et la logique de traitement requise pour injecter des données ou répondre à des événements d’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="719db-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="719db-109">Les composants sont flexibles et légers.</span><span class="sxs-lookup"><span data-stu-id="719db-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="719db-110">Elles peuvent être imbriquées, réutilisées et partagées entre les projets.</span><span class="sxs-lookup"><span data-stu-id="719db-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="719db-111">Classes de composant</span><span class="sxs-lookup"><span data-stu-id="719db-111">Component classes</span></span>

<span data-ttu-id="719db-112">Les composants sont implémentés dans des [:::no-loc(Razor):::](xref:mvc/views/razor) fichiers composants ( `.razor` ) à l’aide d’une combinaison de balisage C# et html.</span><span class="sxs-lookup"><span data-stu-id="719db-112">Components are implemented in [:::no-loc(Razor):::](xref:mvc/views/razor) component files (`.razor`) using a combination of C# and HTML markup.</span></span> <span data-ttu-id="719db-113">Un composant dans :::no-loc(Blazor)::: est officiellement désigné sous le terme de *:::no-loc(Razor)::: composant*.</span><span class="sxs-lookup"><span data-stu-id="719db-113">A component in :::no-loc(Blazor)::: is formally referred to as a *:::no-loc(Razor)::: component*.</span></span>

### <a name="no-locrazor-syntax"></a><span data-ttu-id="719db-114">Syntaxe de :::no-loc(Razor):::</span><span class="sxs-lookup"><span data-stu-id="719db-114">:::no-loc(Razor)::: syntax</span></span>

<span data-ttu-id="719db-115">:::no-loc(Razor)::: les composants dans les :::no-loc(Blazor)::: applications utilisent largement la :::no-loc(Razor)::: syntaxe.</span><span class="sxs-lookup"><span data-stu-id="719db-115">:::no-loc(Razor)::: components in :::no-loc(Blazor)::: apps extensively use :::no-loc(Razor)::: syntax.</span></span> <span data-ttu-id="719db-116">Si vous n’êtes pas familiarisé avec le :::no-loc(Razor)::: langage de balisage, nous vous recommandons de lire <xref:mvc/views/razor> avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="719db-116">If you aren't familiar with the :::no-loc(Razor)::: markup language, we recommend reading <xref:mvc/views/razor> before proceeding.</span></span>

<span data-ttu-id="719db-117">Lorsque vous accédez au contenu sur la :::no-loc(Razor)::: syntaxe, portez une attention particulière aux sections suivantes :</span><span class="sxs-lookup"><span data-stu-id="719db-117">When accessing the content on :::no-loc(Razor)::: syntax, pay special attention to the following sections:</span></span>

* <span data-ttu-id="719db-118">[Directives](xref:mvc/views/razor#directives): `@` -préfixe des mots clés réservés qui modifient généralement la manière dont le balisage du composant est analysé ou fonction.</span><span class="sxs-lookup"><span data-stu-id="719db-118">[Directives](xref:mvc/views/razor#directives): `@`-prefixed reserved keywords that typically change the way component markup is parsed or function.</span></span>
* <span data-ttu-id="719db-119">[Attributs de directive](xref:mvc/views/razor#directive-attributes): `@` Mots clés réservés préfixés qui modifient généralement le mode d’analyse ou de fonction des éléments de composant.</span><span class="sxs-lookup"><span data-stu-id="719db-119">[Directive attributes](xref:mvc/views/razor#directive-attributes): `@`-prefixed reserved keywords that typically change the way component elements are parsed or function.</span></span>

### <a name="names"></a><span data-ttu-id="719db-120">Noms</span><span class="sxs-lookup"><span data-stu-id="719db-120">Names</span></span>

<span data-ttu-id="719db-121">Le nom d’un composant doit commencer par un caractère majuscule.</span><span class="sxs-lookup"><span data-stu-id="719db-121">A component's name must start with an uppercase character.</span></span> <span data-ttu-id="719db-122">Par exemple, `MyCoolComponent.razor` est valide et `myCoolComponent.razor` n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="719db-122">For example, `MyCoolComponent.razor` is valid, and `myCoolComponent.razor` is invalid.</span></span>

### <a name="routing"></a><span data-ttu-id="719db-123">Routage</span><span class="sxs-lookup"><span data-stu-id="719db-123">Routing</span></span>

<span data-ttu-id="719db-124">Le routage dans :::no-loc(Blazor)::: est effectué en fournissant un modèle de routage à chaque composant accessible dans l’application.</span><span class="sxs-lookup"><span data-stu-id="719db-124">Routing in :::no-loc(Blazor)::: is achieved by providing a route template to each accessible component in the app.</span></span> <span data-ttu-id="719db-125">Lorsqu’un :::no-loc(Razor)::: fichier avec une [`@page`][9] directive est compilé, la classe générée reçoit un <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> qui spécifie le modèle de routage.</span><span class="sxs-lookup"><span data-stu-id="719db-125">When a :::no-loc(Razor)::: file with an [`@page`][9] directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="719db-126">Lors de l’exécution, le routeur recherche les classes de composant avec un <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> et rend le composant qui a un modèle de routage correspondant à l’URL demandée.</span><span class="sxs-lookup"><span data-stu-id="719db-126">At runtime, the router looks for component classes with a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> and renders whichever component has a route template that matches the requested URL.</span></span> <span data-ttu-id="719db-127">Pour plus d'informations, consultez <xref:blazor/fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="719db-127">For more information, see <xref:blazor/fundamentals/routing>.</span></span>

```razor
@page "/ParentComponent"

...
```

### <a name="markup"></a><span data-ttu-id="719db-128">balisage</span><span class="sxs-lookup"><span data-stu-id="719db-128">Markup</span></span>

<span data-ttu-id="719db-129">L’interface utilisateur d’un composant est définie à l’aide de HTML.</span><span class="sxs-lookup"><span data-stu-id="719db-129">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="719db-130">La logique de rendu dynamique (par exemple, les boucles, les instructions conditionnelles, les expressions) est ajoutée à l’aide d’une syntaxe C# incorporée appelée *:::no-loc(Razor):::* .</span><span class="sxs-lookup"><span data-stu-id="719db-130">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called *:::no-loc(Razor):::*.</span></span> <span data-ttu-id="719db-131">Quand une application est compilée, le balisage HTML et la logique de rendu C# sont convertis en une classe de composant.</span><span class="sxs-lookup"><span data-stu-id="719db-131">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="719db-132">Le nom de la classe générée correspond au nom du fichier.</span><span class="sxs-lookup"><span data-stu-id="719db-132">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="719db-133">Les membres de la classe Component sont définis dans un [`@code`][1] bloc.</span><span class="sxs-lookup"><span data-stu-id="719db-133">Members of the component class are defined in an [`@code`][1] block.</span></span> <span data-ttu-id="719db-134">Dans le [`@code`][1] bloc, l’état du composant (propriétés, champs) est spécifié avec des méthodes pour la gestion des événements ou pour la définition d’une autre logique de composant.</span><span class="sxs-lookup"><span data-stu-id="719db-134">In the [`@code`][1] block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="719db-135">Plus d’un [`@code`][1] bloc est autorisé.</span><span class="sxs-lookup"><span data-stu-id="719db-135">More than one [`@code`][1] block is permissible.</span></span>

<span data-ttu-id="719db-136">Les membres de composant peuvent être utilisés dans le cadre de la logique de rendu du composant à l’aide d’expressions C# qui commencent par `@` .</span><span class="sxs-lookup"><span data-stu-id="719db-136">Component members can be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="719db-137">Par exemple, un champ C# est rendu en préfixant `@` le nom du champ.</span><span class="sxs-lookup"><span data-stu-id="719db-137">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="719db-138">L’exemple suivant évalue et affiche :</span><span class="sxs-lookup"><span data-stu-id="719db-138">The following example evaluates and renders:</span></span>

* <span data-ttu-id="719db-139">`headingFontStyle` à la valeur de propriété CSS pour `font-style` .</span><span class="sxs-lookup"><span data-stu-id="719db-139">`headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="719db-140">`headingText` au contenu de l' `<h1>` élément.</span><span class="sxs-lookup"><span data-stu-id="719db-140">`headingText` to the content of the `<h1>` element.</span></span>

```razor
<h1 style="font-style:@headingFontStyle">@headingText</h1>

@code {
    private string headingFontStyle = "italic";
    private string headingText = "Put on your new :::no-loc(Blazor):::!";
}
```

<span data-ttu-id="719db-141">Une fois le composant restitué initialement, le composant régénère son arborescence de rendu en réponse aux événements.</span><span class="sxs-lookup"><span data-stu-id="719db-141">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> <span data-ttu-id="719db-142">:::no-loc(Blazor)::: compare ensuite la nouvelle arborescence de rendu à la précédente et applique toutes les modifications apportées au Document Object Model du navigateur (DOM).</span><span class="sxs-lookup"><span data-stu-id="719db-142">:::no-loc(Blazor)::: then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="719db-143">Les composants sont des classes C# ordinaires qui peuvent être placées n’importe où dans un projet.</span><span class="sxs-lookup"><span data-stu-id="719db-143">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="719db-144">Les composants qui produisent des pages Web résident généralement dans le `Pages` dossier.</span><span class="sxs-lookup"><span data-stu-id="719db-144">Components that produce webpages usually reside in the `Pages` folder.</span></span> <span data-ttu-id="719db-145">Les composants qui ne sont pas des pages sont souvent placés dans le `Shared` dossier ou dans un dossier personnalisé ajouté au projet.</span><span class="sxs-lookup"><span data-stu-id="719db-145">Non-page components are frequently placed in the `Shared` folder or a custom folder added to the project.</span></span>

### <a name="namespaces"></a><span data-ttu-id="719db-146">Espaces de noms</span><span class="sxs-lookup"><span data-stu-id="719db-146">Namespaces</span></span>

<span data-ttu-id="719db-147">En règle générale, l’espace de noms d’un composant est dérivé de l’espace de noms racine de l’application et de l’emplacement (dossier) du composant dans l’application.</span><span class="sxs-lookup"><span data-stu-id="719db-147">Typically, a component's namespace is derived from the app's root namespace and the component's location (folder) within the app.</span></span> <span data-ttu-id="719db-148">Si l’espace de noms racine de l’application est `:::no-loc(Blazor):::Sample` et que le `Counter` composant se trouve dans le `Pages` dossier :</span><span class="sxs-lookup"><span data-stu-id="719db-148">If the app's root namespace is `:::no-loc(Blazor):::Sample` and the `Counter` component resides in the `Pages` folder:</span></span>

* <span data-ttu-id="719db-149">L' `Counter` espace de noms du composant est `:::no-loc(Blazor):::Sample.Pages` .</span><span class="sxs-lookup"><span data-stu-id="719db-149">The `Counter` component's namespace is `:::no-loc(Blazor):::Sample.Pages`.</span></span>
* <span data-ttu-id="719db-150">Le nom de type qualifié complet du composant est `:::no-loc(Blazor):::Sample.Pages.Counter` .</span><span class="sxs-lookup"><span data-stu-id="719db-150">The fully qualified type name of the component is `:::no-loc(Blazor):::Sample.Pages.Counter`.</span></span>

<span data-ttu-id="719db-151">Pour les dossiers personnalisés qui contiennent des composants, ajoutez une [`@using`][2] directive au composant parent ou au fichier de l’application `_Imports.razor` .</span><span class="sxs-lookup"><span data-stu-id="719db-151">For custom folders that hold components, add a [`@using`][2] directive to the parent component or to the app's `_Imports.razor` file.</span></span> <span data-ttu-id="719db-152">L’exemple suivant rend les composants du `Components` dossier disponibles :</span><span class="sxs-lookup"><span data-stu-id="719db-152">The following example makes components in the `Components` folder available:</span></span>

```razor
@using :::no-loc(Blazor):::Sample.Components
```

<span data-ttu-id="719db-153">Les composants peuvent également être référencés à l’aide de leurs noms complets, ce qui ne requiert pas la [`@using`][2] directive :</span><span class="sxs-lookup"><span data-stu-id="719db-153">Components can also be referenced using their fully qualified names, which doesn't require the [`@using`][2] directive:</span></span>

```razor
<:::no-loc(Blazor):::Sample.Components.MyComponent />
```

<span data-ttu-id="719db-154">L’espace de noms d’un composant créé avec :::no-loc(Razor)::: est basé sur (par ordre de priorité) :</span><span class="sxs-lookup"><span data-stu-id="719db-154">The namespace of a component authored with :::no-loc(Razor)::: is based on (in priority order):</span></span>

* <span data-ttu-id="719db-155">[`@namespace`][8] désignation dans le :::no-loc(Razor)::: balisage de fichier ( `.razor` ) `@namespace :::no-loc(Blazor):::Sample.MyNamespace` .</span><span class="sxs-lookup"><span data-stu-id="719db-155">[`@namespace`][8] designation in :::no-loc(Razor)::: file (`.razor`) markup (`@namespace :::no-loc(Blazor):::Sample.MyNamespace`).</span></span>
* <span data-ttu-id="719db-156">Du projet `RootNamespace` dans le fichier projet ( `<RootNamespace>:::no-loc(Blazor):::Sample</RootNamespace>` ).</span><span class="sxs-lookup"><span data-stu-id="719db-156">The project's `RootNamespace` in the project file (`<RootNamespace>:::no-loc(Blazor):::Sample</RootNamespace>`).</span></span>
* <span data-ttu-id="719db-157">Nom du projet, pris à partir du nom de fichier du fichier projet ( `.csproj` ) et chemin d’accès de la racine du projet au composant.</span><span class="sxs-lookup"><span data-stu-id="719db-157">The project name, taken from the project file's file name (`.csproj`), and the path from the project root to the component.</span></span> <span data-ttu-id="719db-158">Par exemple, le Framework résout `{PROJECT ROOT}/Pages/Index.razor` ( `:::no-loc(Blazor):::Sample.csproj` ) l’espace de noms `:::no-loc(Blazor):::Sample.Pages` .</span><span class="sxs-lookup"><span data-stu-id="719db-158">For example, the framework resolves `{PROJECT ROOT}/Pages/Index.razor` (`:::no-loc(Blazor):::Sample.csproj`) to the namespace `:::no-loc(Blazor):::Sample.Pages`.</span></span> <span data-ttu-id="719db-159">Les composants suivent les règles de liaison de noms C#.</span><span class="sxs-lookup"><span data-stu-id="719db-159">Components follow C# name binding rules.</span></span> <span data-ttu-id="719db-160">Pour le `Index` composant dans cet exemple, les composants de l’étendue sont tous les composants :</span><span class="sxs-lookup"><span data-stu-id="719db-160">For the `Index` component in this example, the components in scope are all of the components:</span></span>
  * <span data-ttu-id="719db-161">Dans le même dossier, `Pages` .</span><span class="sxs-lookup"><span data-stu-id="719db-161">In the same folder, `Pages`.</span></span>
  * <span data-ttu-id="719db-162">Composants de la racine du projet qui ne spécifient pas explicitement un espace de noms différent.</span><span class="sxs-lookup"><span data-stu-id="719db-162">The components in the project's root that don't explicitly specify a different namespace.</span></span>

> [!NOTE]
> <span data-ttu-id="719db-163">La `global::` qualification n’est pas prise en charge.</span><span class="sxs-lookup"><span data-stu-id="719db-163">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="719db-164">L’importation de composants avec des instructions avec alias [`using`](/dotnet/csharp/language-reference/keywords/using-statement) (par exemple, `@using Foo = Bar` ) n’est pas prise en charge.</span><span class="sxs-lookup"><span data-stu-id="719db-164">Importing components with aliased [`using`](/dotnet/csharp/language-reference/keywords/using-statement) statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="719db-165">Les noms partiellement qualifiés ne sont pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="719db-165">Partially qualified names aren't supported.</span></span> <span data-ttu-id="719db-166">Par exemple, `@using :::no-loc(Blazor):::Sample` l’ajout et la référencement du `NavMenu` composant ( `NavMenu.razor` ) avec `<Shared.NavMenu></Shared.NavMenu>` ne sont pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="719db-166">For example, adding `@using :::no-loc(Blazor):::Sample` and referencing the `NavMenu` component (`NavMenu.razor`) with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

### <a name="partial-class-support"></a><span data-ttu-id="719db-167">Prise en charge des classes partielles</span><span class="sxs-lookup"><span data-stu-id="719db-167">Partial class support</span></span>

<span data-ttu-id="719db-168">:::no-loc(Razor)::: les composants sont générés en tant que classes partielles.</span><span class="sxs-lookup"><span data-stu-id="719db-168">:::no-loc(Razor)::: components are generated as partial classes.</span></span> <span data-ttu-id="719db-169">:::no-loc(Razor)::: les composants sont créés à l’aide de l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="719db-169">:::no-loc(Razor)::: components are authored using either of the following approaches:</span></span>

* <span data-ttu-id="719db-170">Le code C# est défini dans un [`@code`][1] bloc avec le balisage HTML et :::no-loc(Razor)::: le code dans un fichier unique.</span><span class="sxs-lookup"><span data-stu-id="719db-170">C# code is defined in an [`@code`][1] block with HTML markup and :::no-loc(Razor)::: code in a single file.</span></span> <span data-ttu-id="719db-171">:::no-loc(Blazor)::: les modèles définissent leurs :::no-loc(Razor)::: composants à l’aide de cette approche.</span><span class="sxs-lookup"><span data-stu-id="719db-171">:::no-loc(Blazor)::: templates define their :::no-loc(Razor)::: components using this approach.</span></span>
* <span data-ttu-id="719db-172">Le code C# est placé dans un fichier code-behind défini en tant que classe partielle.</span><span class="sxs-lookup"><span data-stu-id="719db-172">C# code is placed in a code-behind file defined as a partial class.</span></span>

<span data-ttu-id="719db-173">L’exemple suivant montre le composant par défaut `Counter` avec un [`@code`][1] bloc dans une application générée à partir d’un :::no-loc(Blazor)::: modèle.</span><span class="sxs-lookup"><span data-stu-id="719db-173">The following example shows the default `Counter` component with an [`@code`][1] block in an app generated from a :::no-loc(Blazor)::: template.</span></span> <span data-ttu-id="719db-174">Le balisage HTML, le :::no-loc(Razor)::: code et le code C# se trouvent dans le même fichier :</span><span class="sxs-lookup"><span data-stu-id="719db-174">HTML markup, :::no-loc(Razor)::: code, and C# code are in the same file:</span></span>

<span data-ttu-id="719db-175">`Pages/Counter.razor`:</span><span class="sxs-lookup"><span data-stu-id="719db-175">`Pages/Counter.razor`:</span></span>

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

@code {
    private int currentCount = 0;

    void IncrementCount()
    {
        currentCount++;
    }
}
```

<span data-ttu-id="719db-176">Le `Counter` composant peut également être créé à l’aide d’un fichier code-behind avec une classe partielle :</span><span class="sxs-lookup"><span data-stu-id="719db-176">The `Counter` component can also be created using a code-behind file with a partial class:</span></span>

<span data-ttu-id="719db-177">`Pages/Counter.razor`:</span><span class="sxs-lookup"><span data-stu-id="719db-177">`Pages/Counter.razor`:</span></span>

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>
```

<span data-ttu-id="719db-178">`Counter.razor.cs`:</span><span class="sxs-lookup"><span data-stu-id="719db-178">`Counter.razor.cs`:</span></span>

```csharp
namespace :::no-loc(Blazor):::Sample.Pages
{
    public partial class Counter
    {
        private int currentCount = 0;

        void IncrementCount()
        {
            currentCount++;
        }
    }
}
```

<span data-ttu-id="719db-179">Ajoutez tous les espaces de noms requis au fichier de classe partielle, si nécessaire.</span><span class="sxs-lookup"><span data-stu-id="719db-179">Add any required namespaces to the partial class file as needed.</span></span> <span data-ttu-id="719db-180">Les espaces de noms standard utilisés par les :::no-loc(Razor)::: composants sont les suivants :</span><span class="sxs-lookup"><span data-stu-id="719db-180">Typical namespaces used by :::no-loc(Razor)::: components include:</span></span>

```csharp
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Components;
using Microsoft.AspNetCore.Components.Authorization;
using Microsoft.AspNetCore.Components.Forms;
using Microsoft.AspNetCore.Components.Routing;
using Microsoft.AspNetCore.Components.Web;
```

> [!IMPORTANT]
> <span data-ttu-id="719db-181">[`@using`][2] les directives du `_Imports.razor` fichier ne sont appliquées qu’aux fichiers :::no-loc(Razor)::: ( `.razor` ), et non aux fichiers C# ( `.cs` ).</span><span class="sxs-lookup"><span data-stu-id="719db-181">[`@using`][2] directives in the `_Imports.razor` file are only applied to :::no-loc(Razor)::: files (`.razor`), not C# files (`.cs`).</span></span>

### <a name="specify-a-base-class"></a><span data-ttu-id="719db-182">Spécifier une classe de base</span><span class="sxs-lookup"><span data-stu-id="719db-182">Specify a base class</span></span>

<span data-ttu-id="719db-183">La [`@inherits`][6] directive peut être utilisée pour spécifier une classe de base pour un composant.</span><span class="sxs-lookup"><span data-stu-id="719db-183">The [`@inherits`][6] directive can be used to specify a base class for a component.</span></span> <span data-ttu-id="719db-184">L’exemple suivant montre comment un composant peut hériter d’une classe de base, `:::no-loc(Blazor):::RocksBase` , pour fournir les propriétés et les méthodes du composant.</span><span class="sxs-lookup"><span data-stu-id="719db-184">The following example shows how a component can inherit a base class, `:::no-loc(Blazor):::RocksBase`, to provide the component's properties and methods.</span></span> <span data-ttu-id="719db-185">La classe de base doit dériver de <xref:Microsoft.AspNetCore.Components.ComponentBase> .</span><span class="sxs-lookup"><span data-stu-id="719db-185">The base class should derive from <xref:Microsoft.AspNetCore.Components.ComponentBase>.</span></span>

<span data-ttu-id="719db-186">`Pages/:::no-loc(Blazor):::Rocks.razor`:</span><span class="sxs-lookup"><span data-stu-id="719db-186">`Pages/:::no-loc(Blazor):::Rocks.razor`:</span></span>

```razor
@page "/:::no-loc(Blazor):::Rocks"
@inherits :::no-loc(Blazor):::RocksBase

<h1>@:::no-loc(Blazor):::RocksText</h1>
```

<span data-ttu-id="719db-187">`:::no-loc(Blazor):::RocksBase.cs`:</span><span class="sxs-lookup"><span data-stu-id="719db-187">`:::no-loc(Blazor):::RocksBase.cs`:</span></span>

```csharp
using Microsoft.AspNetCore.Components;

namespace :::no-loc(Blazor):::Sample
{
    public class :::no-loc(Blazor):::RocksBase : ComponentBase
    {
        public string :::no-loc(Blazor):::RocksText { get; set; } = 
            ":::no-loc(Blazor)::: rocks the browser!";
    }
}
```

### <a name="use-components"></a><span data-ttu-id="719db-188">Utiliser des composants</span><span class="sxs-lookup"><span data-stu-id="719db-188">Use components</span></span>

<span data-ttu-id="719db-189">Les composants peuvent inclure d’autres composants en les déclarant à l’aide de la syntaxe d’élément HTML.</span><span class="sxs-lookup"><span data-stu-id="719db-189">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="719db-190">Le balisage pour l’utilisation d’un composant ressemble à une balise HTML où le nom de la balise est le type du composant.</span><span class="sxs-lookup"><span data-stu-id="719db-190">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="719db-191">Le balisage suivant dans `Pages/Index.razor` rend une `HeadingComponent` instance de :</span><span class="sxs-lookup"><span data-stu-id="719db-191">The following markup in `Pages/Index.razor` renders a `HeadingComponent` instance:</span></span>

```razor
<HeadingComponent />
```

<span data-ttu-id="719db-192">`Components/HeadingComponent.razor`:</span><span class="sxs-lookup"><span data-stu-id="719db-192">`Components/HeadingComponent.razor`:</span></span>

[!code-razor[](index/samples_snapshot/HeadingComponent.razor)]

<span data-ttu-id="719db-193">Si un composant contient un élément HTML avec une première lettre majuscule qui ne correspond pas à un nom de composant, un avertissement est émis pour indiquer que l’élément a un nom inattendu.</span><span class="sxs-lookup"><span data-stu-id="719db-193">If a component contains an HTML element with an uppercase first letter that doesn't match a component name, a warning is emitted indicating that the element has an unexpected name.</span></span> <span data-ttu-id="719db-194">L’ajout d’une [`@using`][2] directive pour l’espace de noms du composant rend le composant disponible, ce qui résout l’avertissement.</span><span class="sxs-lookup"><span data-stu-id="719db-194">Adding an [`@using`][2] directive for the component's namespace makes the component available, which resolves the warning.</span></span>

## <a name="parameters"></a><span data-ttu-id="719db-195">Paramètres</span><span class="sxs-lookup"><span data-stu-id="719db-195">Parameters</span></span>

### <a name="route-parameters"></a><span data-ttu-id="719db-196">Paramètres d’itinéraire</span><span class="sxs-lookup"><span data-stu-id="719db-196">Route parameters</span></span>

<span data-ttu-id="719db-197">Les composants peuvent recevoir des paramètres de routage du modèle de routage fourni dans la [`@page`][9] directive.</span><span class="sxs-lookup"><span data-stu-id="719db-197">Components can receive route parameters from the route template provided in the [`@page`][9] directive.</span></span> <span data-ttu-id="719db-198">Le routeur utilise des paramètres de routage pour remplir les paramètres de composant correspondants.</span><span class="sxs-lookup"><span data-stu-id="719db-198">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="719db-199">`Pages/RouteParameter.razor`:</span><span class="sxs-lookup"><span data-stu-id="719db-199">`Pages/RouteParameter.razor`:</span></span>

[!code-razor[](index/samples_snapshot/RouteParameter.razor?highlight=2,7-8)]

<span data-ttu-id="719db-200">Les paramètres facultatifs ne sont pas pris en charge [`@page`][9] . deux directives sont donc appliquées dans l’exemple précédent.</span><span class="sxs-lookup"><span data-stu-id="719db-200">Optional parameters aren't supported, so two [`@page`][9] directives are applied in the preceding example.</span></span> <span data-ttu-id="719db-201">La première permet de naviguer jusqu’au composant sans paramètre.</span><span class="sxs-lookup"><span data-stu-id="719db-201">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="719db-202">La deuxième [`@page`][9] directive reçoit le `{text}` paramètre d’itinéraire et assigne la valeur à la `Text` propriété.</span><span class="sxs-lookup"><span data-stu-id="719db-202">The second [`@page`][9] directive receives the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

<span data-ttu-id="719db-203">Pour plus d’informations sur les paramètres d’itinéraire Catch-All ( `{*pageRoute}` ), qui capturent les chemins d’accès dans plusieurs limites de dossiers, consultez <xref:blazor/fundamentals/routing#catch-all-route-parameters> .</span><span class="sxs-lookup"><span data-stu-id="719db-203">For information on catch-all route parameters (`{*pageRoute}`), which capture paths across multiple folder boundaries, see <xref:blazor/fundamentals/routing#catch-all-route-parameters>.</span></span>

### <a name="component-parameters"></a><span data-ttu-id="719db-204">Paramètres de composant</span><span class="sxs-lookup"><span data-stu-id="719db-204">Component parameters</span></span>

<span data-ttu-id="719db-205">Les composants peuvent avoir des *paramètres de composant* , qui sont définis à l’aide de propriétés publiques sur la classe de composant avec l' [`[Parameter]`](xref:Microsoft.AspNetCore.Components.ParameterAttribute) attribut.</span><span class="sxs-lookup"><span data-stu-id="719db-205">Components can have *component parameters* , which are defined using public properties on the component class with the [`[Parameter]`](xref:Microsoft.AspNetCore.Components.ParameterAttribute) attribute.</span></span> <span data-ttu-id="719db-206">Utilisez des attributs pour spécifier des arguments pour un composant dans le balisage.</span><span class="sxs-lookup"><span data-stu-id="719db-206">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="719db-207">`Components/ChildComponent.razor`:</span><span class="sxs-lookup"><span data-stu-id="719db-207">`Components/ChildComponent.razor`:</span></span>

[!code-razor[](../common/samples/5.x/:::no-loc(Blazor):::WebAssemblySample/Components/ChildComponent.razor?highlight=2,11-12)]

<span data-ttu-id="719db-208">Dans l’exemple suivant tiré de l’exemple d’application, le `ParentComponent` définit la valeur de la `Title` propriété de `ChildComponent` .</span><span class="sxs-lookup"><span data-stu-id="719db-208">In the following example from the sample app, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="719db-209">`Pages/ParentComponent.razor`:</span><span class="sxs-lookup"><span data-stu-id="719db-209">`Pages/ParentComponent.razor`:</span></span>

[!code-razor[](index/samples_snapshot/ParentComponent.razor?highlight=5-6)]

> [!WARNING]
> <span data-ttu-id="719db-210">Ne créez pas de composants qui écrivent dans leurs propres *paramètres de composant* , utilisez un champ privé à la place.</span><span class="sxs-lookup"><span data-stu-id="719db-210">Don't create components that write to their own *component parameters* , use a private field instead.</span></span> <span data-ttu-id="719db-211">Pour plus d’informations, consultez la section [paramètres remplacés](#overwritten-parameters) .</span><span class="sxs-lookup"><span data-stu-id="719db-211">For more information, see the [Overwritten parameters](#overwritten-parameters) section.</span></span>

## <a name="child-content"></a><span data-ttu-id="719db-212">Contenu enfant</span><span class="sxs-lookup"><span data-stu-id="719db-212">Child content</span></span>

<span data-ttu-id="719db-213">Les composants peuvent définir le contenu d’un autre composant.</span><span class="sxs-lookup"><span data-stu-id="719db-213">Components can set the content of another component.</span></span> <span data-ttu-id="719db-214">Le composant d’affectation fournit le contenu entre les balises qui spécifient le composant récepteur.</span><span class="sxs-lookup"><span data-stu-id="719db-214">The assigning component provides the content between the tags that specify the receiving component.</span></span>

<span data-ttu-id="719db-215">Dans l’exemple suivant, `ChildComponent` a une `ChildContent` propriété qui représente un <xref:Microsoft.AspNetCore.Components.RenderFragment> , qui représente un segment de l’interface utilisateur à restituer.</span><span class="sxs-lookup"><span data-stu-id="719db-215">In the following example, the `ChildComponent` has a `ChildContent` property that represents a <xref:Microsoft.AspNetCore.Components.RenderFragment>, which represents a segment of UI to render.</span></span> <span data-ttu-id="719db-216">La valeur de `ChildContent` est positionnée dans le balisage du composant où le contenu doit être rendu.</span><span class="sxs-lookup"><span data-stu-id="719db-216">The value of `ChildContent` is positioned in the component's markup where the content should be rendered.</span></span> <span data-ttu-id="719db-217">La valeur de `ChildContent` est reçue du composant parent et rendue à l’intérieur du panneau de démarrage `panel-body` .</span><span class="sxs-lookup"><span data-stu-id="719db-217">The value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="719db-218">`Components/ChildComponent.razor`:</span><span class="sxs-lookup"><span data-stu-id="719db-218">`Components/ChildComponent.razor`:</span></span>

[!code-razor[](../common/samples/5.x/:::no-loc(Blazor):::WebAssemblySample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="719db-219">La propriété qui reçoit le <xref:Microsoft.AspNetCore.Components.RenderFragment> contenu doit être nommée `ChildContent` par Convention.</span><span class="sxs-lookup"><span data-stu-id="719db-219">The property receiving the <xref:Microsoft.AspNetCore.Components.RenderFragment> content must be named `ChildContent` by convention.</span></span>

<span data-ttu-id="719db-220">`ParentComponent`Dans l’exemple d’application, vous pouvez fournir du contenu pour le rendu de `ChildComponent` en plaçant le contenu à l’intérieur des `<ChildComponent>` balises.</span><span class="sxs-lookup"><span data-stu-id="719db-220">The `ParentComponent` in the sample app can provide content for rendering the `ChildComponent` by placing the content inside the `<ChildComponent>` tags.</span></span>

<span data-ttu-id="719db-221">`Pages/ParentComponent.razor`:</span><span class="sxs-lookup"><span data-stu-id="719db-221">`Pages/ParentComponent.razor`:</span></span>

[!code-razor[](index/samples_snapshot/ParentComponent.razor?highlight=7-8)]

<span data-ttu-id="719db-222">En raison de la façon dont le :::no-loc(Blazor)::: contenu enfant est rendu, les composants de rendu à l’intérieur d’une `for` boucle requièrent une variable d’index local si la variable de boucle d’incrémentation est utilisée dans le contenu du composant enfant :</span><span class="sxs-lookup"><span data-stu-id="719db-222">Due to the way that :::no-loc(Blazor)::: renders child content, rendering components inside a `for` loop requires a local index variable if the incrementing loop variable is used in the child component's content:</span></span>
>
> ```razor
> @for (int c = 0; c < 10; c++)
> {
>     var current = c;
>     <ChildComponent Param1="@c">
>         Child Content: Count: @current
>     </ChildComponent>
> }
> ```
>
> <span data-ttu-id="719db-223">Vous pouvez également utiliser une `foreach` boucle avec <xref:System.Linq.Enumerable.Range%2A?displayProperty=nameWithType> :</span><span class="sxs-lookup"><span data-stu-id="719db-223">Alternatively, use a `foreach` loop with <xref:System.Linq.Enumerable.Range%2A?displayProperty=nameWithType>:</span></span>
>
> ```razor
> @foreach(var c in Enumerable.Range(0,10))
> {
>     <ChildComponent Param1="@c">
>         Child Content: Count: @c
>     </ChildComponent>
> }
> ```

## <a name="attribute-splatting-and-arbitrary-parameters"></a><span data-ttu-id="719db-224">Réprojection d’attribut et paramètres arbitraires</span><span class="sxs-lookup"><span data-stu-id="719db-224">Attribute splatting and arbitrary parameters</span></span>

<span data-ttu-id="719db-225">Les composants peuvent capturer et restituer des attributs supplémentaires en plus des paramètres déclarés du composant.</span><span class="sxs-lookup"><span data-stu-id="719db-225">Components can capture and render additional attributes in addition to the component's declared parameters.</span></span> <span data-ttu-id="719db-226">Des attributs supplémentaires peuvent être capturés dans un dictionnaire *, puis* réintégrés à un élément lorsque le composant est rendu à l’aide de la [`@attributes`][3] :::no-loc(Razor)::: directive.</span><span class="sxs-lookup"><span data-stu-id="719db-226">Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the [`@attributes`][3] :::no-loc(Razor)::: directive.</span></span> <span data-ttu-id="719db-227">Ce scénario est utile lors de la définition d’un composant qui produit un élément de balisage qui prend en charge diverses personnalisations.</span><span class="sxs-lookup"><span data-stu-id="719db-227">This scenario is useful when defining a component that produces a markup element that supports a variety of customizations.</span></span> <span data-ttu-id="719db-228">Par exemple, il peut être fastidieux de définir des attributs séparément pour un `<input>` qui prend en charge de nombreux paramètres.</span><span class="sxs-lookup"><span data-stu-id="719db-228">For example, it can be tedious to define attributes separately for an `<input>` that supports many parameters.</span></span>

<span data-ttu-id="719db-229">Dans l’exemple suivant, le premier `<input>` élément ( `id="useIndividualParams"` ) utilise des paramètres de composant individuels, tandis que le deuxième `<input>` élément ( `id="useAttributesDict"` ) utilise la projection d’attributs :</span><span class="sxs-lookup"><span data-stu-id="719db-229">In the following example, the first `<input>` element (`id="useIndividualParams"`) uses individual component parameters, while the second `<input>` element (`id="useAttributesDict"`) uses attribute splatting:</span></span>

```razor
<input id="useIndividualParams"
       maxlength="@maxlength"
       placeholder="@placeholder"
       required="@required"
       size="@size" />

<input id="useAttributesDict"
       @attributes="InputAttributes" />

@code {
    public string maxlength = "10";
    public string placeholder = "Input placeholder text";
    public string required = "required";
    public string size = "50";

    public Dictionary<string, object> InputAttributes { get; set; } =
        new Dictionary<string, object>()
        {
            { "maxlength", "10" },
            { "placeholder", "Input placeholder text" },
            { "required", "required" },
            { "size", "50" }
        };
}
```

<span data-ttu-id="719db-230">Le type du paramètre doit implémenter `IEnumerable<KeyValuePair<string, object>>` ou `IReadOnlyDictionary<string, object>` avec des clés de chaîne.</span><span class="sxs-lookup"><span data-stu-id="719db-230">The type of the parameter must implement `IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` with string keys.</span></span>

<span data-ttu-id="719db-231">Les éléments rendus `<input>` à l’aide des deux approches sont identiques :</span><span class="sxs-lookup"><span data-stu-id="719db-231">The rendered `<input>` elements using both approaches is identical:</span></span>

```html
<input id="useIndividualParams"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">

<input id="useAttributesDict"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">
```

<span data-ttu-id="719db-232">Pour accepter des attributs arbitraires, définissez un paramètre de composant à l’aide de l' [`[Parameter]`](xref:Microsoft.AspNetCore.Components.ParameterAttribute) attribut avec la <xref:Microsoft.AspNetCore.Components.ParameterAttribute.CaptureUnmatchedValues> propriété définie sur `true` :</span><span class="sxs-lookup"><span data-stu-id="719db-232">To accept arbitrary attributes, define a component parameter using the [`[Parameter]`](xref:Microsoft.AspNetCore.Components.ParameterAttribute) attribute with the <xref:Microsoft.AspNetCore.Components.ParameterAttribute.CaptureUnmatchedValues> property set to `true`:</span></span>

```razor
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

<span data-ttu-id="719db-233">La <xref:Microsoft.AspNetCore.Components.ParameterAttribute.CaptureUnmatchedValues> propriété sur [`[Parameter]`](xref:Microsoft.AspNetCore.Components.ParameterAttribute) permet au paramètre de correspondre à tous les attributs qui ne correspondent à aucun autre paramètre.</span><span class="sxs-lookup"><span data-stu-id="719db-233">The <xref:Microsoft.AspNetCore.Components.ParameterAttribute.CaptureUnmatchedValues> property on [`[Parameter]`](xref:Microsoft.AspNetCore.Components.ParameterAttribute) allows the parameter to match all attributes that don't match any other parameter.</span></span> <span data-ttu-id="719db-234">Un composant ne peut définir qu’un seul paramètre avec <xref:Microsoft.AspNetCore.Components.ParameterAttribute.CaptureUnmatchedValues> .</span><span class="sxs-lookup"><span data-stu-id="719db-234">A component can only define a single parameter with <xref:Microsoft.AspNetCore.Components.ParameterAttribute.CaptureUnmatchedValues>.</span></span> <span data-ttu-id="719db-235">Le type de propriété utilisé avec <xref:Microsoft.AspNetCore.Components.ParameterAttribute.CaptureUnmatchedValues> doit pouvoir être assigné à partir de `Dictionary<string, object>` avec des clés de chaîne.</span><span class="sxs-lookup"><span data-stu-id="719db-235">The property type used with <xref:Microsoft.AspNetCore.Components.ParameterAttribute.CaptureUnmatchedValues> must be assignable from `Dictionary<string, object>` with string keys.</span></span> <span data-ttu-id="719db-236">`IEnumerable<KeyValuePair<string, object>>` ou `IReadOnlyDictionary<string, object>` sont également des options dans ce scénario.</span><span class="sxs-lookup"><span data-stu-id="719db-236">`IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` are also options in this scenario.</span></span>

<span data-ttu-id="719db-237">La position [`@attributes`][3] relative à la position des attributs d’élément est importante.</span><span class="sxs-lookup"><span data-stu-id="719db-237">The position of [`@attributes`][3] relative to the position of element attributes is important.</span></span> <span data-ttu-id="719db-238">Quand sont représentées [`@attributes`][3] sur l’élément, les attributs sont traités de droite à gauche (dernier à premier).</span><span class="sxs-lookup"><span data-stu-id="719db-238">When [`@attributes`][3] are splatted on the element, the attributes are processed from right to left (last to first).</span></span> <span data-ttu-id="719db-239">Prenons l’exemple suivant d’un composant qui consomme un `Child` composant :</span><span class="sxs-lookup"><span data-stu-id="719db-239">Consider the following example of a component that consumes a `Child` component:</span></span>

<span data-ttu-id="719db-240">`ParentComponent.razor`:</span><span class="sxs-lookup"><span data-stu-id="719db-240">`ParentComponent.razor`:</span></span>

```razor
<ChildComponent extra="10" />
```

<span data-ttu-id="719db-241">`ChildComponent.razor`:</span><span class="sxs-lookup"><span data-stu-id="719db-241">`ChildComponent.razor`:</span></span>

```razor
<div @attributes="AdditionalAttributes" extra="5" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="719db-242">L' `Child` attribut du composant `extra` est défini à droite de [`@attributes`][3] .</span><span class="sxs-lookup"><span data-stu-id="719db-242">The `Child` component's `extra` attribute is set to the right of [`@attributes`][3].</span></span> <span data-ttu-id="719db-243">Le `Parent` rendu du composant `<div>` contient `extra="5"` lorsqu’il est transmis via l’attribut supplémentaire, car les attributs sont traités de droite à gauche (dernier à premier) :</span><span class="sxs-lookup"><span data-stu-id="719db-243">The `Parent` component's rendered `<div>` contains `extra="5"` when passed through the additional attribute because the attributes are processed right to left (last to first):</span></span>

```html
<div extra="5" />
```

<span data-ttu-id="719db-244">Dans l’exemple suivant, l’ordre de `extra` et [`@attributes`][3] est inversé dans le d' `Child` un composant `<div>` :</span><span class="sxs-lookup"><span data-stu-id="719db-244">In the following example, the order of `extra` and [`@attributes`][3] is reversed in the `Child` component's `<div>`:</span></span>

<span data-ttu-id="719db-245">`ParentComponent.razor`:</span><span class="sxs-lookup"><span data-stu-id="719db-245">`ParentComponent.razor`:</span></span>

```razor
<ChildComponent extra="10" />
```

<span data-ttu-id="719db-246">`ChildComponent.razor`:</span><span class="sxs-lookup"><span data-stu-id="719db-246">`ChildComponent.razor`:</span></span>

```razor
<div extra="5" @attributes="AdditionalAttributes" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="719db-247">Le rendu `<div>` dans le `Parent` composant contient `extra="10"` lorsqu’il est passé par le biais de l’attribut supplémentaire :</span><span class="sxs-lookup"><span data-stu-id="719db-247">The rendered `<div>` in the `Parent` component contains `extra="10"` when passed through the additional attribute:</span></span>

```html
<div extra="10" />
```

## <a name="capture-references-to-components"></a><span data-ttu-id="719db-248">Capturer des références à des composants</span><span class="sxs-lookup"><span data-stu-id="719db-248">Capture references to components</span></span>

<span data-ttu-id="719db-249">Les références de composant offrent un moyen de référencer une instance de composant afin que vous puissiez émettre des commandes vers cette instance, telles que `Show` ou `Reset` .</span><span class="sxs-lookup"><span data-stu-id="719db-249">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="719db-250">Pour capturer une référence de composant :</span><span class="sxs-lookup"><span data-stu-id="719db-250">To capture a component reference:</span></span>

* <span data-ttu-id="719db-251">Ajoutez un [`@ref`][4] attribut au composant enfant.</span><span class="sxs-lookup"><span data-stu-id="719db-251">Add an [`@ref`][4] attribute to the child component.</span></span>
* <span data-ttu-id="719db-252">Définissez un champ avec le même type que le composant enfant.</span><span class="sxs-lookup"><span data-stu-id="719db-252">Define a field with the same type as the child component.</span></span>

```razor
<CustomLoginDialog @ref="loginDialog" ... />

@code {
    private CustomLoginDialog loginDialog;

    private void OnSomething()
    {
        loginDialog.Show();
    }
}
```

<span data-ttu-id="719db-253">Lors du rendu du composant, le `loginDialog` champ est rempli avec l' `MyLoginDialog` instance du composant enfant.</span><span class="sxs-lookup"><span data-stu-id="719db-253">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="719db-254">Vous pouvez ensuite appeler des méthodes .NET sur l’instance du composant.</span><span class="sxs-lookup"><span data-stu-id="719db-254">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="719db-255">La `loginDialog` variable est remplie uniquement après le rendu du composant et sa sortie comprend l' `MyLoginDialog` élément.</span><span class="sxs-lookup"><span data-stu-id="719db-255">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="719db-256">Tant que le composant n’est pas rendu, il n’y a rien à référencer.</span><span class="sxs-lookup"><span data-stu-id="719db-256">Until the component is rendered, there's nothing to reference.</span></span>
>
> <span data-ttu-id="719db-257">Pour manipuler des références de composants après la fin du rendu du composant, utilisez les [ `OnAfterRenderAsync` `OnAfterRender` méthodes ou](xref:blazor/components/lifecycle#after-component-render).</span><span class="sxs-lookup"><span data-stu-id="719db-257">To manipulate components references after the component has finished rendering, use the [`OnAfterRenderAsync` or `OnAfterRender` methods](xref:blazor/components/lifecycle#after-component-render).</span></span>
>
> <span data-ttu-id="719db-258">Pour utiliser une variable de référence avec un gestionnaire d’événements, utilisez une expression lambda ou assignez le délégué de gestionnaire d’événements dans les [ `OnAfterRenderAsync` `OnAfterRender` méthodes ou](xref:blazor/components/lifecycle#after-component-render).</span><span class="sxs-lookup"><span data-stu-id="719db-258">To use a reference variable with an event handler, use a lambda expression or assign the event handler delegate in the [`OnAfterRenderAsync` or `OnAfterRender` methods](xref:blazor/components/lifecycle#after-component-render).</span></span> <span data-ttu-id="719db-259">Cela garantit que la variable de référence est assignée avant que le gestionnaire d’événements soit affecté.</span><span class="sxs-lookup"><span data-stu-id="719db-259">This ensures that the reference variable is assigned before the event handler is assigned.</span></span>
>
> ```razor
> <button type="button" 
>     @onclick="@(() => loginDialog.DoSomething())">Do Something</button>
>
> <MyLoginDialog @ref="loginDialog" ... />
>
> @code {
>     private MyLoginDialog loginDialog;
> }
> ```

<span data-ttu-id="719db-260">Pour référencer des composants dans une boucle, consultez [capturer des références à plusieurs composants enfants similaires (dotnet/aspnetcore #13358)](https://github.com/dotnet/aspnetcore/issues/13358).</span><span class="sxs-lookup"><span data-stu-id="719db-260">To reference components in a loop, see [Capture references to multiple similar child-components (dotnet/aspnetcore #13358)](https://github.com/dotnet/aspnetcore/issues/13358).</span></span>

<span data-ttu-id="719db-261">Bien que la capture de références de composant utilise une syntaxe similaire pour [capturer des références d’élément](xref:blazor/call-javascript-from-dotnet#capture-references-to-elements), il ne s’agit pas d’une fonctionnalité JavaScript Interop.</span><span class="sxs-lookup"><span data-stu-id="719db-261">While capturing component references use a similar syntax to [capturing element references](xref:blazor/call-javascript-from-dotnet#capture-references-to-elements), it isn't a JavaScript interop feature.</span></span> <span data-ttu-id="719db-262">Les références de composant ne sont pas passées au code JavaScript.</span><span class="sxs-lookup"><span data-stu-id="719db-262">Component references aren't passed to JavaScript code.</span></span> <span data-ttu-id="719db-263">Les références de composants sont uniquement utilisées dans le code .NET.</span><span class="sxs-lookup"><span data-stu-id="719db-263">Component references are only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="719db-264">N’utilisez **pas** de références de composant pour muter l’état des composants enfants.</span><span class="sxs-lookup"><span data-stu-id="719db-264">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="719db-265">Utilisez plutôt des paramètres déclaratifs normaux pour passer des données aux composants enfants.</span><span class="sxs-lookup"><span data-stu-id="719db-265">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="719db-266">L’utilisation de paramètres déclaratifs normaux entraîne le rerendu automatique des composants enfants.</span><span class="sxs-lookup"><span data-stu-id="719db-266">Use of normal declarative parameters result in child components that rerender at the correct times automatically.</span></span>

## <a name="synchronization-context"></a><span data-ttu-id="719db-267">Contexte de synchronisation</span><span class="sxs-lookup"><span data-stu-id="719db-267">Synchronization context</span></span>

<span data-ttu-id="719db-268">:::no-loc(Blazor)::: utilise un contexte de synchronisation ( <xref:System.Threading.SynchronizationContext> ) pour appliquer un seul thread logique d’exécution.</span><span class="sxs-lookup"><span data-stu-id="719db-268">:::no-loc(Blazor)::: uses a synchronization context (<xref:System.Threading.SynchronizationContext>) to enforce a single logical thread of execution.</span></span> <span data-ttu-id="719db-269">Les méthodes de [cycle de vie](xref:blazor/components/lifecycle) d’un composant et les rappels d’événements déclenchés par :::no-loc(Blazor)::: sont exécutés sur le contexte de synchronisation.</span><span class="sxs-lookup"><span data-stu-id="719db-269">A component's [lifecycle methods](xref:blazor/components/lifecycle) and any event callbacks that are raised by :::no-loc(Blazor)::: are executed on the synchronization context.</span></span>

<span data-ttu-id="719db-270">:::no-loc(Blazor Server):::le contexte de synchronisation de tente d’émuler un environnement à thread unique afin qu’il corresponde étroitement au modèle webassembly dans le navigateur, qui est monothread.</span><span class="sxs-lookup"><span data-stu-id="719db-270">:::no-loc(Blazor Server):::'s synchronization context attempts to emulate a single-threaded environment so that it closely matches the WebAssembly model in the browser, which is single threaded.</span></span> <span data-ttu-id="719db-271">À un moment donné, le travail est effectué sur un seul thread, ce qui donne l’impression d’un seul thread logique.</span><span class="sxs-lookup"><span data-stu-id="719db-271">At any given point in time, work is performed on exactly one thread, giving the impression of a single logical thread.</span></span> <span data-ttu-id="719db-272">Deux opérations ne sont pas exécutées simultanément.</span><span class="sxs-lookup"><span data-stu-id="719db-272">No two operations execute concurrently.</span></span>

### <a name="avoid-thread-blocking-calls"></a><span data-ttu-id="719db-273">Éviter les appels de blocage de thread</span><span class="sxs-lookup"><span data-stu-id="719db-273">Avoid thread-blocking calls</span></span>

<span data-ttu-id="719db-274">En règle générale, n’appelez pas les méthodes suivantes.</span><span class="sxs-lookup"><span data-stu-id="719db-274">Generally, don't call the following methods.</span></span> <span data-ttu-id="719db-275">Les méthodes suivantes bloquent le thread et empêchent donc l’application de reprendre le travail jusqu’à ce que le sous-jacent <xref:System.Threading.Tasks.Task> soit terminé :</span><span class="sxs-lookup"><span data-stu-id="719db-275">The following methods block the thread and thus block the app from resuming work until the underlying <xref:System.Threading.Tasks.Task> is complete:</span></span>

* <xref:System.Threading.Tasks.Task%601.Result%2A>
* <xref:System.Threading.Tasks.Task.Wait%2A>
* <xref:System.Threading.Tasks.Task.WaitAny%2A>
* <xref:System.Threading.Tasks.Task.WaitAll%2A>
* <xref:System.Threading.Thread.Sleep%2A>
* <xref:System.Runtime.CompilerServices.TaskAwaiter.GetResult%2A>

### <a name="invoke-component-methods-externally-to-update-state"></a><span data-ttu-id="719db-276">Appeler des méthodes de composant en externe pour mettre à jour l’État</span><span class="sxs-lookup"><span data-stu-id="719db-276">Invoke component methods externally to update state</span></span>

<span data-ttu-id="719db-277">Dans le cas où un composant doit être mis à jour en fonction d’un événement externe, tel qu’un minuteur ou d’autres notifications, utilisez la `InvokeAsync` méthode, qui est distribuée au :::no-loc(Blazor)::: contexte de synchronisation de.</span><span class="sxs-lookup"><span data-stu-id="719db-277">In the event a component must be updated based on an external event, such as a timer or other notifications, use the `InvokeAsync` method, which dispatches to :::no-loc(Blazor):::'s synchronization context.</span></span> <span data-ttu-id="719db-278">Par exemple, considérez un *service de notification* qui peut notifier n’importe quel composant d’écoute de l’État mis à jour :</span><span class="sxs-lookup"><span data-stu-id="719db-278">For example, consider a *notifier service* that can notify any listening component of the updated state:</span></span>

```csharp
public class NotifierService
{
    // Can be called from anywhere
    public async Task Update(string key, int value)
    {
        if (Notify != null)
        {
            await Notify.Invoke(key, value);
        }
    }

    public event Func<string, int, Task> Notify;
}
```

<span data-ttu-id="719db-279">Inscrire `NotifierService` :</span><span class="sxs-lookup"><span data-stu-id="719db-279">Register the `NotifierService`:</span></span>

* <span data-ttu-id="719db-280">Dans :::no-loc(Blazor WebAssembly)::: , inscrivez le service comme Singleton dans `Program.Main` :</span><span class="sxs-lookup"><span data-stu-id="719db-280">In :::no-loc(Blazor WebAssembly):::, register the service as singleton in `Program.Main`:</span></span>

  ```csharp
  builder.Services.AddSingleton<NotifierService>();
  ```

* <span data-ttu-id="719db-281">Dans :::no-loc(Blazor Server)::: , inscrivez le service comme étant étendu dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="719db-281">In :::no-loc(Blazor Server):::, register the service as scoped in `Startup.ConfigureServices`:</span></span>

  ```csharp
  services.AddScoped<NotifierService>();
  ```

<span data-ttu-id="719db-282">Utilisez `NotifierService` pour mettre à jour un composant :</span><span class="sxs-lookup"><span data-stu-id="719db-282">Use the `NotifierService` to update a component:</span></span>

```razor
@page "/"
@inject NotifierService Notifier
@implements IDisposable

<p>Last update: @lastNotification.key = @lastNotification.value</p>

@code {
    private (string key, int value) lastNotification;

    protected override void OnInitialized()
    {
        Notifier.Notify += OnNotify;
    }

    public async Task OnNotify(string key, int value)
    {
        await InvokeAsync(() =>
        {
            lastNotification = (key, value);
            StateHasChanged();
        });
    }

    public void Dispose()
    {
        Notifier.Notify -= OnNotify;
    }
}
```

<span data-ttu-id="719db-283">Dans l’exemple précédent, `NotifierService` appelle la méthode du composant `OnNotify` en dehors du :::no-loc(Blazor)::: contexte de synchronisation de.</span><span class="sxs-lookup"><span data-stu-id="719db-283">In the preceding example, `NotifierService` invokes the component's `OnNotify` method outside of :::no-loc(Blazor):::'s synchronization context.</span></span> <span data-ttu-id="719db-284">`InvokeAsync` est utilisé pour basculer vers le contexte correct et pour la mise en file d’attente d’un rendu.</span><span class="sxs-lookup"><span data-stu-id="719db-284">`InvokeAsync` is used to switch to the correct context and queue a render.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="719db-285">Utiliser \@ la clé pour contrôler la conservation des éléments et des composants</span><span class="sxs-lookup"><span data-stu-id="719db-285">Use \@key to control the preservation of elements and components</span></span>

<span data-ttu-id="719db-286">Lors du rendu d’une liste d’éléments ou de composants, et si les éléments ou les composants changent par la suite, l' :::no-loc(Blazor)::: algorithme de différenciation de doit décider quels éléments ou composants précédents peuvent être conservés et comment les objets de modèle doivent être mappés à eux.</span><span class="sxs-lookup"><span data-stu-id="719db-286">When rendering a list of elements or components and the elements or components subsequently change, :::no-loc(Blazor):::'s diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="719db-287">Normalement, ce processus est automatique et peut être ignoré, mais dans certains cas, il peut être utile de contrôler le processus.</span><span class="sxs-lookup"><span data-stu-id="719db-287">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="719db-288">Prenons l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="719db-288">Consider the following example:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor Details="@person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="719db-289">Le contenu de la `People` collection peut changer avec des entrées insérées, supprimées ou réorganisées.</span><span class="sxs-lookup"><span data-stu-id="719db-289">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="719db-290">Lors du rerendu du composant, le `<DetailsEditor>` composant peut changer pour recevoir des `Details` valeurs de paramètre différentes.</span><span class="sxs-lookup"><span data-stu-id="719db-290">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="719db-291">Cela peut entraîner un rerendu plus complexe que prévu.</span><span class="sxs-lookup"><span data-stu-id="719db-291">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="719db-292">Dans certains cas, le rerendu peut entraîner des différences de comportement visibles, telles que le focus de l’élément perdu.</span><span class="sxs-lookup"><span data-stu-id="719db-292">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="719db-293">Le processus de mappage peut être contrôlé à l’aide de l' [`@key`][5] attribut directive.</span><span class="sxs-lookup"><span data-stu-id="719db-293">The mapping process can be controlled with the [`@key`][5] directive attribute.</span></span> <span data-ttu-id="719db-294">[`@key`][5] force l’algorithme de comparaison à garantir la préservation des éléments ou des composants en fonction de la valeur de la clé :</span><span class="sxs-lookup"><span data-stu-id="719db-294">[`@key`][5] causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor @key="person" Details="@person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="719db-295">Lorsque la `People` collection est modifiée, l’algorithme de comparaison conserve l’association entre les `<DetailsEditor>` instances et les `person` instances :</span><span class="sxs-lookup"><span data-stu-id="719db-295">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="719db-296">Si un `Person` est supprimé de la `People` liste, seule l' `<DetailsEditor>` instance correspondante est supprimée de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="719db-296">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="719db-297">Les autres instances restent inchangées.</span><span class="sxs-lookup"><span data-stu-id="719db-297">Other instances are left unchanged.</span></span>
* <span data-ttu-id="719db-298">Si un `Person` est inséré à une position dans la liste, une nouvelle `<DetailsEditor>` instance est insérée à cette position correspondante.</span><span class="sxs-lookup"><span data-stu-id="719db-298">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="719db-299">Les autres instances restent inchangées.</span><span class="sxs-lookup"><span data-stu-id="719db-299">Other instances are left unchanged.</span></span>
* <span data-ttu-id="719db-300">Si `Person` les entrées sont réordonnées, les `<DetailsEditor>` instances correspondantes sont conservées et réordonnées dans l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="719db-300">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="719db-301">Dans certains scénarios, l’utilisation de [`@key`][5] réduit la complexité du rerendu et évite les problèmes potentiels liés aux parties avec état de la modification du modèle DOM, telles que la position du focus.</span><span class="sxs-lookup"><span data-stu-id="719db-301">In some scenarios, use of [`@key`][5] minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="719db-302">Les clés sont locales pour chaque élément ou composant conteneur.</span><span class="sxs-lookup"><span data-stu-id="719db-302">Keys are local to each container element or component.</span></span> <span data-ttu-id="719db-303">Les clés ne sont pas comparées globalement dans le document.</span><span class="sxs-lookup"><span data-stu-id="719db-303">Keys aren't compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="719db-304">Quand utiliser la \@ clé</span><span class="sxs-lookup"><span data-stu-id="719db-304">When to use \@key</span></span>

<span data-ttu-id="719db-305">En règle générale, il est judicieux d’utiliser [`@key`][5] chaque fois qu’une liste est affichée (par exemple, dans un bloc [foreach](/dotnet/csharp/language-reference/keywords/foreach-in) ) et qu’une valeur appropriée existe pour définir [`@key`][5] .</span><span class="sxs-lookup"><span data-stu-id="719db-305">Typically, it makes sense to use [`@key`][5] whenever a list is rendered (for example, in a [foreach](/dotnet/csharp/language-reference/keywords/foreach-in) block) and a suitable value exists to define the [`@key`][5].</span></span>

<span data-ttu-id="719db-306">Vous pouvez également utiliser [`@key`][5] pour empêcher :::no-loc(Blazor)::: de conserver un élément ou une sous-arborescence de composants lorsqu’un objet change :</span><span class="sxs-lookup"><span data-stu-id="719db-306">You can also use [`@key`][5] to prevent :::no-loc(Blazor)::: from preserving an element or component subtree when an object changes:</span></span>

```razor
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

<span data-ttu-id="719db-307">En cas `@currentPerson` de modification, la [`@key`][5] directive :::no-loc(Blazor)::: d’attribut force à ignorer la totalité `<div>` et ses descendants, et à reconstruire la sous-arborescence de l’interface utilisateur avec de nouveaux éléments et composants.</span><span class="sxs-lookup"><span data-stu-id="719db-307">If `@currentPerson` changes, the [`@key`][5] attribute directive forces :::no-loc(Blazor)::: to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="719db-308">Cela peut être utile si vous devez garantir qu’aucun État d’interface utilisateur n’est préservé lorsque des `@currentPerson` modifications sont apportées.</span><span class="sxs-lookup"><span data-stu-id="719db-308">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="719db-309">Quand ne pas utiliser la \@ clé</span><span class="sxs-lookup"><span data-stu-id="719db-309">When not to use \@key</span></span>

<span data-ttu-id="719db-310">Il y a un coût en matière de performances lors de la comparaison avec [`@key`][5] .</span><span class="sxs-lookup"><span data-stu-id="719db-310">There's a performance cost when diffing with [`@key`][5].</span></span> <span data-ttu-id="719db-311">Le coût des performances n’est pas important, mais spécifiez uniquement [`@key`][5] si les règles de conservation des éléments ou des composants bénéficient de l’application.</span><span class="sxs-lookup"><span data-stu-id="719db-311">The performance cost isn't large, but only specify [`@key`][5] if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="719db-312">Même si [`@key`][5] n’est pas utilisé, :::no-loc(Blazor)::: conserve autant que possible les instances d’élément et de composant enfants.</span><span class="sxs-lookup"><span data-stu-id="719db-312">Even if [`@key`][5] isn't used, :::no-loc(Blazor)::: preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="719db-313">Le seul avantage de l’utilisation de [`@key`][5] est de contrôler la *façon dont* les instances de modèle sont mappées aux instances de composant conservées, au lieu de l’algorithme de comparaison qui sélectionne le mappage.</span><span class="sxs-lookup"><span data-stu-id="719db-313">The only advantage to using [`@key`][5] is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="719db-314">Valeurs à utiliser pour la \@ clé</span><span class="sxs-lookup"><span data-stu-id="719db-314">What values to use for \@key</span></span>

<span data-ttu-id="719db-315">En règle générale, il est logique de fournir l’un des types de valeur suivants pour [`@key`][5] :</span><span class="sxs-lookup"><span data-stu-id="719db-315">Generally, it makes sense to supply one of the following kinds of value for [`@key`][5]:</span></span>

* <span data-ttu-id="719db-316">Instances d’objet de modèle (par exemple, une `Person` instance comme dans l’exemple précédent).</span><span class="sxs-lookup"><span data-stu-id="719db-316">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="719db-317">Cela garantit la préservation en fonction de l’égalité des références d’objet.</span><span class="sxs-lookup"><span data-stu-id="719db-317">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="719db-318">Identificateurs uniques (par exemple, les valeurs de clé primaire de type `int` , `string` ou `Guid` ).</span><span class="sxs-lookup"><span data-stu-id="719db-318">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="719db-319">Vérifiez que les valeurs utilisées pour [`@key`][5] ne sont pas en conflit.</span><span class="sxs-lookup"><span data-stu-id="719db-319">Ensure that values used for [`@key`][5] don't clash.</span></span> <span data-ttu-id="719db-320">Si les valeurs en conflit sont détectées dans le même élément parent, :::no-loc(Blazor)::: lève une exception, car il ne peut pas mapper de manière déterministe les anciens éléments ou composants aux nouveaux éléments ou composants.</span><span class="sxs-lookup"><span data-stu-id="719db-320">If clashing values are detected within the same parent element, :::no-loc(Blazor)::: throws an exception because it can't deterministically map old elements or components to new elements or components.</span></span> <span data-ttu-id="719db-321">Utilisez uniquement des valeurs distinctes, telles que des instances d’objets ou des valeurs de clé primaire.</span><span class="sxs-lookup"><span data-stu-id="719db-321">Only use distinct values, such as object instances or primary key values.</span></span>

## <a name="overwritten-parameters"></a><span data-ttu-id="719db-322">Paramètres remplacés</span><span class="sxs-lookup"><span data-stu-id="719db-322">Overwritten parameters</span></span>

<span data-ttu-id="719db-323">L' :::no-loc(Blazor)::: infrastructure impose généralement une attribution de paramètres parent-enfant sécurisée :</span><span class="sxs-lookup"><span data-stu-id="719db-323">The :::no-loc(Blazor)::: framework generally imposes safe parent-to-child parameter assignment:</span></span>

* <span data-ttu-id="719db-324">Les paramètres ne sont pas remplacés de manière inattendue.</span><span class="sxs-lookup"><span data-stu-id="719db-324">Parameters aren't overwritten unexpectedly.</span></span>
* <span data-ttu-id="719db-325">Les effets secondaires sont réduits.</span><span class="sxs-lookup"><span data-stu-id="719db-325">Side-effects are minimized.</span></span> <span data-ttu-id="719db-326">Par exemple, les rendus supplémentaires sont évités, car ils peuvent créer des boucles de rendu infinies.</span><span class="sxs-lookup"><span data-stu-id="719db-326">For example, additional renders are avoided because they may create infinite rendering loops.</span></span>

<span data-ttu-id="719db-327">Un composant enfant reçoit de nouvelles valeurs de paramètre qui, éventuellement, remplacent les valeurs existantes lors du rerendu du composant parent.</span><span class="sxs-lookup"><span data-stu-id="719db-327">A child component receives new parameter values that possibly overwrite existing values when the parent component rerenders.</span></span> <span data-ttu-id="719db-328">Accidentially remplacement des valeurs de paramètre dans un composant enfant se produit souvent lors du développement du composant avec un ou plusieurs paramètres liés aux données et le développeur écrit directement dans un paramètre de l’enfant :</span><span class="sxs-lookup"><span data-stu-id="719db-328">Accidentially overwriting parameter values in a child component often occurs when developing the component with one or more data-bound parameters and the developer writes directly to a parameter in the child:</span></span>

* <span data-ttu-id="719db-329">Le composant enfant est restitué avec une ou plusieurs valeurs de paramètre à partir du composant parent.</span><span class="sxs-lookup"><span data-stu-id="719db-329">The child component is rendered with one or more parameter values from the parent component.</span></span>
* <span data-ttu-id="719db-330">L’enfant écrit directement dans la valeur d’un paramètre.</span><span class="sxs-lookup"><span data-stu-id="719db-330">The child writes directly to the value of a parameter.</span></span>
* <span data-ttu-id="719db-331">Le composant parent effectue un rerendu et remplace la valeur du paramètre de l’enfant.</span><span class="sxs-lookup"><span data-stu-id="719db-331">The parent component rerenders and overwrites the value of the child's parameter.</span></span>

<span data-ttu-id="719db-332">Le potentiel de remplacement des valeurs de paramètre s’étend également dans les accesseurs set de propriété du composant enfant.</span><span class="sxs-lookup"><span data-stu-id="719db-332">The potential for overwriting paramater values extends into the child component's property setters, too.</span></span>

<span data-ttu-id="719db-333">**Nos conseils généraux ne permettent pas de créer des composants qui écrivent directement dans leurs propres paramètres.**</span><span class="sxs-lookup"><span data-stu-id="719db-333">**Our general guidance is not to create components that directly write to their own parameters.**</span></span>

<span data-ttu-id="719db-334">Prenons l’exemple du composant défaillant suivant `Expander` :</span><span class="sxs-lookup"><span data-stu-id="719db-334">Consider the following faulty `Expander` component that:</span></span>

* <span data-ttu-id="719db-335">Restitue le contenu enfant.</span><span class="sxs-lookup"><span data-stu-id="719db-335">Renders child content.</span></span>
* <span data-ttu-id="719db-336">Active ou désactive l’indication du contenu enfant avec un paramètre de composant ( `Expanded` ).</span><span class="sxs-lookup"><span data-stu-id="719db-336">Toggles showing child content with a component parameter (`Expanded`).</span></span>
* <span data-ttu-id="719db-337">Le composant écrit directement dans le `Expanded` paramètre, qui illustre le problème avec les paramètres remplacés et doit être évité.</span><span class="sxs-lookup"><span data-stu-id="719db-337">The component writes directly to the `Expanded` parameter, which demonstrates the problem with overwritten parameters and should be avoided.</span></span>

```razor
<div @onclick="@Toggle" class="card bg-light mb-3" style="width:30rem">
    <div class="card-body">
        <h2 class="card-title">Toggle (<code>Expanded</code> = @Expanded)</h2>

        @if (Expanded)
        {
            <p class="card-text">@ChildContent</p>
        }
    </div>
</div>

@code {
    [Parameter]
    public bool Expanded { get; set; }

    [Parameter]
    public RenderFragment ChildContent { get; set; }

    private void Toggle()
    {
        Expanded = !Expanded;
    }
}
```

<span data-ttu-id="719db-338">Le `Expander` composant est ajouté à un composant parent qui peut appeler <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A> :</span><span class="sxs-lookup"><span data-stu-id="719db-338">The `Expander` component is added to a parent component that may call <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A>:</span></span>

```razor
@page "/expander"

<Expander Expanded="true">
    Expander 1 content
</Expander>

<Expander Expanded="true" />

<button @onclick="StateHasChanged">
    Call StateHasChanged
</button>
```

<span data-ttu-id="719db-339">Au départ, les `Expander` composants se comportent indépendamment lorsque leurs `Expanded` propriétés sont basculées.</span><span class="sxs-lookup"><span data-stu-id="719db-339">Initially, the `Expander` components behave independently when their `Expanded` properties are toggled.</span></span> <span data-ttu-id="719db-340">Les composants enfants maintiennent leurs États comme prévu.</span><span class="sxs-lookup"><span data-stu-id="719db-340">The child components maintain their states as expected.</span></span> <span data-ttu-id="719db-341">Lorsque <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A> est appelé dans le parent, le `Expanded` paramètre du premier composant enfant est réinitialisé à sa valeur initiale ( `true` ).</span><span class="sxs-lookup"><span data-stu-id="719db-341">When <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A> is called in the parent, the `Expanded` parameter of the first child component is reset back to its initial value (`true`).</span></span> <span data-ttu-id="719db-342">La `Expander` valeur du deuxième composant `Expanded` n’est pas réinitialisée, car aucun contenu enfant n’est restitué dans le deuxième composant.</span><span class="sxs-lookup"><span data-stu-id="719db-342">The second `Expander` component's `Expanded` value isn't reset because no child content is rendered in the second component.</span></span>

<span data-ttu-id="719db-343">Pour maintenir l’État dans le scénario précédent, utilisez un *champ privé* dans le `Expander` composant pour maintenir son état bascule.</span><span class="sxs-lookup"><span data-stu-id="719db-343">To maintain state in the preceding scenario, use a *private field* in the `Expander` component to maintain its toggled state.</span></span>

<span data-ttu-id="719db-344">Le composant révisé suivant `Expander` :</span><span class="sxs-lookup"><span data-stu-id="719db-344">The following revised `Expander` component:</span></span>

* <span data-ttu-id="719db-345">Accepte la `Expanded` valeur de paramètre du composant à partir du parent.</span><span class="sxs-lookup"><span data-stu-id="719db-345">Accepts the `Expanded` component parameter value from the parent.</span></span>
* <span data-ttu-id="719db-346">Affecte la valeur du paramètre de composant à un *champ privé* ( `expanded` ) dans l' [événement OnInitialized](xref:blazor/components/lifecycle#component-initialization-methods).</span><span class="sxs-lookup"><span data-stu-id="719db-346">Assigns the component parameter value to a *private field* (`expanded`) in the [OnInitialized event](xref:blazor/components/lifecycle#component-initialization-methods).</span></span>
* <span data-ttu-id="719db-347">Utilise le champ privé pour conserver son état bascule interne, qui montre comment éviter d’écrire directement dans un paramètre.</span><span class="sxs-lookup"><span data-stu-id="719db-347">Uses the private field to maintain its internal toggle state, which demonstrates how to avoid writing directly to a parameter.</span></span>

```razor
<div @onclick="@Toggle" class="card bg-light mb-3" style="width:30rem">
    <div class="card-body">
        <h2 class="card-title">Toggle (<code>expanded</code> = @expanded)</h2>

        @if (expanded)
        {
            <p class="card-text">@ChildContent</p>
        }
    </div>
</div>

@code {
    private bool expanded;

    [Parameter]
    public bool Expanded { get; set; }

    [Parameter]
    public RenderFragment ChildContent { get; set; }

    protected override void OnInitialized()
    {
        expanded = Expanded;
    }

    private void Toggle()
    {
        expanded = !expanded;
    }
}
```

<span data-ttu-id="719db-348">Pour plus d’informations, consultez [ :::no-loc(Blazor)::: erreur de liaison bidirectionnelle (dotnet/aspnetcore #24599)](https://github.com/dotnet/aspnetcore/issues/24599).</span><span class="sxs-lookup"><span data-stu-id="719db-348">For additional information, see [:::no-loc(Blazor)::: Two Way Binding Error (dotnet/aspnetcore #24599)](https://github.com/dotnet/aspnetcore/issues/24599).</span></span> 

## <a name="apply-an-attribute"></a><span data-ttu-id="719db-349">Appliquer un attribut</span><span class="sxs-lookup"><span data-stu-id="719db-349">Apply an attribute</span></span>

<span data-ttu-id="719db-350">Les attributs peuvent être appliqués aux :::no-loc(Razor)::: composants avec la [`@attribute`][7] directive.</span><span class="sxs-lookup"><span data-stu-id="719db-350">Attributes can be applied to :::no-loc(Razor)::: components with the [`@attribute`][7] directive.</span></span> <span data-ttu-id="719db-351">L’exemple suivant applique l' [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribut à la classe de composant :</span><span class="sxs-lookup"><span data-stu-id="719db-351">The following example applies the [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute to the component class:</span></span>

```razor
@page "/"
@attribute [Authorize]
```

## <a name="conditional-html-element-attributes"></a><span data-ttu-id="719db-352">Attributs d’éléments HTML conditionnels</span><span class="sxs-lookup"><span data-stu-id="719db-352">Conditional HTML element attributes</span></span>

<span data-ttu-id="719db-353">Les attributs des éléments HTML sont rendus de manière conditionnelle en fonction de la valeur .NET.</span><span class="sxs-lookup"><span data-stu-id="719db-353">HTML element attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="719db-354">Si la valeur est `false` ou `null` , l’attribut n’est pas rendu.</span><span class="sxs-lookup"><span data-stu-id="719db-354">If the value is `false` or `null`, the attribute isn't rendered.</span></span> <span data-ttu-id="719db-355">Si la valeur est `true` , l’attribut est rendu réduit.</span><span class="sxs-lookup"><span data-stu-id="719db-355">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="719db-356">Dans l’exemple suivant, `IsCompleted` détermine si `checked` est rendu dans le balisage de l’élément :</span><span class="sxs-lookup"><span data-stu-id="719db-356">In the following example, `IsCompleted` determines if `checked` is rendered in the element's markup:</span></span>

```razor
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

<span data-ttu-id="719db-357">Si `IsCompleted` est `true` , la case à cocher s’affiche comme suit :</span><span class="sxs-lookup"><span data-stu-id="719db-357">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="719db-358">Si `IsCompleted` est `false` , la case à cocher s’affiche comme suit :</span><span class="sxs-lookup"><span data-stu-id="719db-358">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="719db-359">Pour plus d'informations, consultez <xref:mvc/views/razor>.</span><span class="sxs-lookup"><span data-stu-id="719db-359">For more information, see <xref:mvc/views/razor>.</span></span>

> [!WARNING]
> <span data-ttu-id="719db-360">Certains attributs HTML, tels que [`aria-pressed`](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons) , ne fonctionnent pas correctement quand le type .net est un `bool` .</span><span class="sxs-lookup"><span data-stu-id="719db-360">Some HTML attributes, such as [`aria-pressed`](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), don't function properly when the .NET type is a `bool`.</span></span> <span data-ttu-id="719db-361">Dans ce cas, utilisez un `string` type au lieu d’un `bool` .</span><span class="sxs-lookup"><span data-stu-id="719db-361">In those cases, use a `string` type instead of a `bool`.</span></span>

## <a name="raw-html"></a><span data-ttu-id="719db-362">HTML brut</span><span class="sxs-lookup"><span data-stu-id="719db-362">Raw HTML</span></span>

<span data-ttu-id="719db-363">Les chaînes sont normalement rendues à l’aide de nœuds de texte DOM, ce qui signifie que tout balisage qu’elles peuvent contenir est ignorée et traitée comme du texte littéral.</span><span class="sxs-lookup"><span data-stu-id="719db-363">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="719db-364">Pour afficher le code HTML brut, encapsulez le contenu HTML dans une `MarkupString` valeur.</span><span class="sxs-lookup"><span data-stu-id="719db-364">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="719db-365">La valeur est analysée au format HTML ou SVG et est insérée dans le DOM.</span><span class="sxs-lookup"><span data-stu-id="719db-365">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="719db-366">Le rendu de code HTML brut construit à partir de n’importe quelle source non approuvée constitue un risque pour la **sécurité** et doit être évité !</span><span class="sxs-lookup"><span data-stu-id="719db-366">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="719db-367">L’exemple suivant illustre l’utilisation du `MarkupString` type pour ajouter un bloc de contenu HTML statique à la sortie rendue d’un composant :</span><span class="sxs-lookup"><span data-stu-id="719db-367">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@code {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="no-locrazor-templates"></a><span data-ttu-id="719db-368">:::no-loc(Razor)::: ceux</span><span class="sxs-lookup"><span data-stu-id="719db-368">:::no-loc(Razor)::: templates</span></span>

<span data-ttu-id="719db-369">Les fragments de rendu peuvent être définis à l’aide de la :::no-loc(Razor)::: syntaxe de modèle.</span><span class="sxs-lookup"><span data-stu-id="719db-369">Render fragments can be defined using :::no-loc(Razor)::: template syntax.</span></span> <span data-ttu-id="719db-370">:::no-loc(Razor)::: les modèles sont un moyen de définir un extrait de code d’interface utilisateur et de supposer le format suivant :</span><span class="sxs-lookup"><span data-stu-id="719db-370">:::no-loc(Razor)::: templates are a way to define a UI snippet and assume the following format:</span></span>

```razor
@<{HTML tag}>...</{HTML tag}>
```

<span data-ttu-id="719db-371">L’exemple suivant montre comment spécifier <xref:Microsoft.AspNetCore.Components.RenderFragment> des valeurs et <xref:Microsoft.AspNetCore.Components.RenderFragment%601> et restituer des modèles directement dans un composant.</span><span class="sxs-lookup"><span data-stu-id="719db-371">The following example illustrates how to specify <xref:Microsoft.AspNetCore.Components.RenderFragment> and <xref:Microsoft.AspNetCore.Components.RenderFragment%601> values and render templates directly in a component.</span></span> <span data-ttu-id="719db-372">Les fragments de rendu peuvent également être passés comme arguments à des [composants basés](xref:blazor/components/templated-components)sur un modèle.</span><span class="sxs-lookup"><span data-stu-id="719db-372">Render fragments can also be passed as arguments to [templated components](xref:blazor/components/templated-components).</span></span>

```razor
@timeTemplate

@petTemplate(new Pet { Name = "Rex" })

@code {
    private RenderFragment timeTemplate = @<p>The time is @DateTime.Now.</p>;
    private RenderFragment<Pet> petTemplate = (pet) => @<p>Pet: @pet.Name</p>;

    private class Pet
    {
        public string Name { get; set; }
    }
}
```

<span data-ttu-id="719db-373">Sortie rendue du code précédent :</span><span class="sxs-lookup"><span data-stu-id="719db-373">Rendered output of the preceding code:</span></span>

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Pet: Rex</p>
```

## <a name="static-assets"></a><span data-ttu-id="719db-374">Les ressources statiques</span><span class="sxs-lookup"><span data-stu-id="719db-374">Static assets</span></span>

<span data-ttu-id="719db-375">:::no-loc(Blazor):::respecte la Convention de ASP.NET Core les applications qui placent des ressources statiques dans le [ `web root (wwwroot)` dossier](xref:fundamentals/index#web-root)du projet.</span><span class="sxs-lookup"><span data-stu-id="719db-375">:::no-loc(Blazor)::: follows the convention of ASP.NET Core apps placing static assets under the project's [`web root (wwwroot)` folder](xref:fundamentals/index#web-root).</span></span>

<span data-ttu-id="719db-376">Utilisez un chemin d’accès relatif à `/` la base () pour faire référence à la racine Web d’une ressource statique.</span><span class="sxs-lookup"><span data-stu-id="719db-376">Use a base-relative path (`/`) to refer to the web root for a static asset.</span></span> <span data-ttu-id="719db-377">Dans l’exemple suivant, `logo.png` se trouve physiquement dans le `{PROJECT ROOT}/wwwroot/images` dossier :</span><span class="sxs-lookup"><span data-stu-id="719db-377">In the following example, `logo.png` is physically located in the `{PROJECT ROOT}/wwwroot/images` folder:</span></span>

```razor
<img alt="Company logo" src="/images/logo.png" />
```

<span data-ttu-id="719db-378">:::no-loc(Razor)::: les composants ne prennent **pas** en charge la notation tilde-slash ( `~/` ).</span><span class="sxs-lookup"><span data-stu-id="719db-378">:::no-loc(Razor)::: components do **not** support tilde-slash notation (`~/`).</span></span>

<span data-ttu-id="719db-379">Pour plus d’informations sur la définition du chemin d’accès de base d’une application, consultez <xref:blazor/host-and-deploy/index#app-base-path> .</span><span class="sxs-lookup"><span data-stu-id="719db-379">For information on setting an app's base path, see <xref:blazor/host-and-deploy/index#app-base-path>.</span></span>

## <a name="tag-helpers-arent-supported-in-components"></a><span data-ttu-id="719db-380">Les balises tag Helper ne sont pas prises en charge dans les composants</span><span class="sxs-lookup"><span data-stu-id="719db-380">Tag Helpers aren't supported in components</span></span>

<span data-ttu-id="719db-381">[`Tag Helpers`](xref:mvc/views/tag-helpers/intro) ne sont pas pris en charge dans les :::no-loc(Razor)::: composants ( `.razor` fichiers).</span><span class="sxs-lookup"><span data-stu-id="719db-381">[`Tag Helpers`](xref:mvc/views/tag-helpers/intro) aren't supported in :::no-loc(Razor)::: components (`.razor` files).</span></span> <span data-ttu-id="719db-382">Pour fournir des fonctionnalités semblables à tag Helper dans :::no-loc(Blazor)::: , créez un composant avec les mêmes fonctionnalités que le tag Helper et utilisez le composant à la place.</span><span class="sxs-lookup"><span data-stu-id="719db-382">To provide Tag Helper-like functionality in :::no-loc(Blazor):::, create a component with the same functionality as the Tag Helper and use the component instead.</span></span>

## <a name="scalable-vector-graphics-svg-images"></a><span data-ttu-id="719db-383">Images SVG (Scalable Vector Graphics)</span><span class="sxs-lookup"><span data-stu-id="719db-383">Scalable Vector Graphics (SVG) images</span></span>

<span data-ttu-id="719db-384">Étant donné que :::no-loc(Blazor)::: le rendu des images HTML, prises en charge par le navigateur, y compris les images SVG (Scalable Vector Graphics) ( `.svg` ), est pris en charge via la `<img>` balise :</span><span class="sxs-lookup"><span data-stu-id="719db-384">Since :::no-loc(Blazor)::: renders HTML, browser-supported images, including Scalable Vector Graphics (SVG) images (`.svg`), are supported via the `<img>` tag:</span></span>

```html
<img alt="Example image" src="some-image.svg" />
```

<span data-ttu-id="719db-385">De même, les images SVG sont prises en charge dans les règles CSS d’un fichier de feuille de style ( `.css` ) :</span><span class="sxs-lookup"><span data-stu-id="719db-385">Similarly, SVG images are supported in the CSS rules of a stylesheet file (`.css`):</span></span>

```css
.my-element {
    background-image: url("some-image.svg");
}
```

<span data-ttu-id="719db-386">Toutefois, le balisage SVG en ligne n’est pas pris en charge dans tous les scénarios.</span><span class="sxs-lookup"><span data-stu-id="719db-386">However, inline SVG markup isn't supported in all scenarios.</span></span> <span data-ttu-id="719db-387">Si vous placez une `<svg>` balise directement dans un fichier de composant ( `.razor` ), le rendu d’image de base est pris en charge, mais de nombreux scénarios avancés ne sont pas encore pris en charge.</span><span class="sxs-lookup"><span data-stu-id="719db-387">If you place an `<svg>` tag directly into a component file (`.razor`), basic image rendering is supported but many advanced scenarios aren't yet supported.</span></span> <span data-ttu-id="719db-388">Par exemple, les `<use>` balises ne sont pas actuellement respectées et [`@bind`][10] ne peuvent pas être utilisées avec certaines balises SVG.</span><span class="sxs-lookup"><span data-stu-id="719db-388">For example, `<use>` tags aren't currently respected, and [`@bind`][10] can't be used with some SVG tags.</span></span> <span data-ttu-id="719db-389">Pour plus d’informations, consultez [prise en charge SVG dans :::no-loc(Blazor)::: (dotnet/aspnetcore #18271)](https://github.com/dotnet/aspnetcore/issues/18271).</span><span class="sxs-lookup"><span data-stu-id="719db-389">For more information, see [SVG support in :::no-loc(Blazor)::: (dotnet/aspnetcore #18271)](https://github.com/dotnet/aspnetcore/issues/18271).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="719db-390">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="719db-390">Additional resources</span></span>

* <span data-ttu-id="719db-391"><xref:blazor/security/server/threat-mitigation>: Fournit des conseils sur la création d' :::no-loc(Blazor Server)::: applications qui doivent être en concurrence avec l’épuisement des ressources.</span><span class="sxs-lookup"><span data-stu-id="719db-391"><xref:blazor/security/server/threat-mitigation>: Includes guidance on building :::no-loc(Blazor Server)::: apps that must contend with resource exhaustion.</span></span>

<!--Reference links in article-->
[1]: <xref:mvc/views/razor#code>
[2]: <xref:mvc/views/razor#using>
[3]: <xref:mvc/views/razor#attributes>
[4]: <xref:mvc/views/razor#ref>
[5]: <xref:mvc/views/razor#key>
[6]: <xref:mvc/views/razor#inherits>
[7]: <xref:mvc/views/razor#attribute>
[8]: <xref:mvc/views/razor#namespace>
[9]: <xref:mvc/views/razor#page>
[10]: <xref:mvc/views/razor#bind>
