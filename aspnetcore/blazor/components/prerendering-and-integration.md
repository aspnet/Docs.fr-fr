---
title: 'Prérendu et intégration des :::no-loc(Razor)::: composants ASP.net Core'
author: guardrex
description: 'Découvrez :::no-loc(Razor)::: les scénarios d’intégration de composants pour :::no-loc(Blazor)::: les applications, y compris le prérendu des :::no-loc(Razor)::: composants sur le serveur.'
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/29/2020
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
uid: blazor/components/prerendering-and-integration
zone_pivot_groups: blazor-hosting-models
ms.openlocfilehash: affca6c9b585b91787f94a13144d07bedfefdd37
ms.sourcegitcommit: fe5a287fa6b9477b130aa39728f82cdad57611ee
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2020
ms.locfileid: "94431704"
---
# <a name="prerender-and-integrate-aspnet-core-no-locrazor-components"></a><span data-ttu-id="8f915-103">Prérendu et intégration des :::no-loc(Razor)::: composants ASP.net Core</span><span class="sxs-lookup"><span data-stu-id="8f915-103">Prerender and integrate ASP.NET Core :::no-loc(Razor)::: components</span></span>

<span data-ttu-id="8f915-104">Par [Luke Latham](https://github.com/guardrex) et [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="8f915-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

::: zone pivot="webassembly"

::: moniker range=">= aspnetcore-5.0"

<span data-ttu-id="8f915-105">:::no-loc(Razor)::: les composants peuvent être intégrés dans des :::no-loc(Razor)::: pages et des applications MVC dans une solution hébergée :::no-loc(Blazor WebAssembly)::: .</span><span class="sxs-lookup"><span data-stu-id="8f915-105">:::no-loc(Razor)::: components can be integrated into :::no-loc(Razor)::: Pages and MVC apps in a hosted :::no-loc(Blazor WebAssembly)::: solution.</span></span> <span data-ttu-id="8f915-106">Lorsque la page ou la vue est restituée, les composants peuvent être prérendus en même temps.</span><span class="sxs-lookup"><span data-stu-id="8f915-106">When the page or view is rendered, components can be prerendered at the same time.</span></span>

## <a name="configuration"></a><span data-ttu-id="8f915-107">Configuration</span><span class="sxs-lookup"><span data-stu-id="8f915-107">Configuration</span></span>

<span data-ttu-id="8f915-108">Pour configurer le prérendu d’une :::no-loc(Blazor WebAssembly)::: application :</span><span class="sxs-lookup"><span data-stu-id="8f915-108">To set up prerendering for a :::no-loc(Blazor WebAssembly)::: app:</span></span>

1. <span data-ttu-id="8f915-109">Hébergez l' :::no-loc(Blazor WebAssembly)::: application dans une application ASP.net core.</span><span class="sxs-lookup"><span data-stu-id="8f915-109">Host the :::no-loc(Blazor WebAssembly)::: app in an ASP.NET Core app.</span></span> <span data-ttu-id="8f915-110">Une :::no-loc(Blazor WebAssembly)::: application autonome peut être ajoutée à une solution ASP.net Core, ou vous pouvez utiliser une application hébergée :::no-loc(Blazor WebAssembly)::: créée à partir du :::no-loc(Blazor)::: modèle de projet hébergé.</span><span class="sxs-lookup"><span data-stu-id="8f915-110">A standalone :::no-loc(Blazor WebAssembly)::: app can be added to an ASP.NET Core solution, or you can use a hosted :::no-loc(Blazor WebAssembly)::: app created from the :::no-loc(Blazor)::: Hosted project template.</span></span>

1. <span data-ttu-id="8f915-111">Supprimez le fichier statique par défaut `wwwroot/index.html` du :::no-loc(Blazor WebAssembly)::: projet client.</span><span class="sxs-lookup"><span data-stu-id="8f915-111">Remove the default static `wwwroot/index.html` file from the :::no-loc(Blazor WebAssembly)::: client project.</span></span>

1. <span data-ttu-id="8f915-112">Supprimez la ligne suivante dans `Program.Main` le projet client :</span><span class="sxs-lookup"><span data-stu-id="8f915-112">Delete the following line in `Program.Main` in the client project:</span></span>

   ```csharp
   builder.RootComponents.Add<App>("#app");
   ```

1. <span data-ttu-id="8f915-113">Ajoutez un `Pages/_Host.cshtml` fichier au projet serveur.</span><span class="sxs-lookup"><span data-stu-id="8f915-113">Add a `Pages/_Host.cshtml` file to the server project.</span></span> <span data-ttu-id="8f915-114">Vous pouvez obtenir un `_Host.cshtml` fichier à partir d’une application créée à partir du :::no-loc(Blazor Server)::: modèle à l’aide `dotnet new blazorserver -o :::no-loc(Blazor):::Server` de la commande dans un interpréteur de commandes.</span><span class="sxs-lookup"><span data-stu-id="8f915-114">You can obtain a `_Host.cshtml` file from an app created from the :::no-loc(Blazor Server)::: template with the `dotnet new blazorserver -o :::no-loc(Blazor):::Server` command in a command shell.</span></span> <span data-ttu-id="8f915-115">Après avoir placé le `Pages/_Host.cshtml` fichier dans l’application serveur de la :::no-loc(Blazor WebAssembly)::: solution hébergée, apportez les modifications suivantes au fichier :</span><span class="sxs-lookup"><span data-stu-id="8f915-115">After placing the `Pages/_Host.cshtml` file into the server app of the hosted :::no-loc(Blazor WebAssembly)::: solution, make the following changes to the file:</span></span>

   * <span data-ttu-id="8f915-116">Définissez l’espace de noms sur le dossier de l’application serveur `Pages` (par exemple, `@namespace :::no-loc(Blazor):::Hosted.Server.Pages` ).</span><span class="sxs-lookup"><span data-stu-id="8f915-116">Set the namespace to the server app's `Pages` folder (for example, `@namespace :::no-loc(Blazor):::Hosted.Server.Pages`).</span></span>
   * <span data-ttu-id="8f915-117">Définissez une [`@using`](xref:mvc/views/razor#using) directive pour le projet client (par exemple, `@using :::no-loc(Blazor):::Hosted.Client` ).</span><span class="sxs-lookup"><span data-stu-id="8f915-117">Set an [`@using`](xref:mvc/views/razor#using) directive for the client project (for example, `@using :::no-loc(Blazor):::Hosted.Client`).</span></span>
   * <span data-ttu-id="8f915-118">Mettez à jour les liens de feuille de style pour pointer vers la feuille de style de l’application webassembly.</span><span class="sxs-lookup"><span data-stu-id="8f915-118">Update the stylesheet links to point to the WebAssembly app's stylesheet.</span></span> <span data-ttu-id="8f915-119">Dans l’exemple suivant, l’espace de noms de l’application cliente est `:::no-loc(Blazor):::Hosted.Client` :</span><span class="sxs-lookup"><span data-stu-id="8f915-119">In the following example, the client app's namespace is `:::no-loc(Blazor):::Hosted.Client`:</span></span>

     ```cshtml
     <link href="css/app.css" rel="stylesheet" />
     <link href=":::no-loc(Blazor):::Hosted.Client.styles.css" rel="stylesheet" />
     ```

   * <span data-ttu-id="8f915-120">Mettez à jour le `render-mode` du [tag Helper du composant](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper) pour prérestituer le `App` composant racine :</span><span class="sxs-lookup"><span data-stu-id="8f915-120">Update the `render-mode` of the [Component Tag Helper](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper) to prerender the root `App` component:</span></span>

     ```cshtml
     <component type="typeof(App)" render-mode="WebAssemblyPrerendered" />
     ```

   * <span data-ttu-id="8f915-121">Mettez à jour la :::no-loc(Blazor)::: source du script pour utiliser le :::no-loc(Blazor WebAssembly)::: script côté client :</span><span class="sxs-lookup"><span data-stu-id="8f915-121">Update the :::no-loc(Blazor)::: script source to use the client-side :::no-loc(Blazor WebAssembly)::: script:</span></span>

     ```cshtml
     <script src="_framework/blazor.webassembly.js"></script>
     ```

1. <span data-ttu-id="8f915-122">Dans `Startup.Configure` ( `Startup.cs` ) du projet serveur :</span><span class="sxs-lookup"><span data-stu-id="8f915-122">In `Startup.Configure` (`Startup.cs`) of the server project:</span></span>

   * <span data-ttu-id="8f915-123">Appelez `UseDeveloperExceptionPage` sur le générateur d’applications dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="8f915-123">Call `UseDeveloperExceptionPage` on the app builder in the Development environment.</span></span>
   * <span data-ttu-id="8f915-124">Appelez `Use:::no-loc(Blazor):::FrameworkFiles` sur le générateur d’applications.</span><span class="sxs-lookup"><span data-stu-id="8f915-124">Call `Use:::no-loc(Blazor):::FrameworkFiles` on the app builder.</span></span>
   * <span data-ttu-id="8f915-125">Modifiez le secours de la `index.html` page ( `endpoints.MapFallbackToFile("index.html");` ) à la `_Host.cshtml` page.</span><span class="sxs-lookup"><span data-stu-id="8f915-125">Change the fallback from the `index.html` page (`endpoints.MapFallbackToFile("index.html");`) to the `_Host.cshtml` page.</span></span>

   ```csharp
   public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
   {
       if (env.IsDevelopment())
       {
           app.UseDeveloperExceptionPage();
           app.UseWebAssemblyDebugging();
       }
       else
       {
           app.UseExceptionHandler("/Error");
           app.UseHsts();
       }

       app.UseHttpsRedirection();
       app.Use:::no-loc(Blazor):::FrameworkFiles();
       app.UseStaticFiles();

       app.UseRouting();

       app.UseEndpoints(endpoints =>
       {
           endpoints.Map:::no-loc(Razor):::Pages();
           endpoints.MapControllers();
           endpoints.MapFallbackToPage("/_Host");
       });
   }
   ```

## <a name="render-components-in-a-page-or-view-with-the-component-tag-helper"></a><span data-ttu-id="8f915-126">Rendre des composants dans une page ou une vue à l’aide du tag Helper Component</span><span class="sxs-lookup"><span data-stu-id="8f915-126">Render components in a page or view with the Component Tag Helper</span></span>

<span data-ttu-id="8f915-127">Le tag Helper Component prend en charge deux modes de rendu pour le rendu d’un composant à partir d’une :::no-loc(Blazor WebAssembly)::: application dans une page ou une vue :</span><span class="sxs-lookup"><span data-stu-id="8f915-127">The Component Tag Helper supports two render modes for rendering a component from a :::no-loc(Blazor WebAssembly)::: app in a page or view:</span></span>

* <span data-ttu-id="8f915-128">`WebAssembly`: Restitue un marqueur pour une :::no-loc(Blazor WebAssembly)::: application à utiliser pour inclure un composant interactif lorsqu’il est chargé dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="8f915-128">`WebAssembly`: Renders a marker for a :::no-loc(Blazor WebAssembly)::: app for use to include an interactive component when loaded in the browser.</span></span> <span data-ttu-id="8f915-129">Le composant n’est pas prérendu.</span><span class="sxs-lookup"><span data-stu-id="8f915-129">The component isn't prerendered.</span></span> <span data-ttu-id="8f915-130">Cette option facilite le rendu de différents :::no-loc(Blazor WebAssembly)::: composants sur des pages différentes.</span><span class="sxs-lookup"><span data-stu-id="8f915-130">This option makes it easier to render different :::no-loc(Blazor WebAssembly)::: components on different pages.</span></span>
* <span data-ttu-id="8f915-131">`WebAssemblyPrerendered`: Prérestitue le composant en HTML statique et comprend un marqueur pour une :::no-loc(Blazor WebAssembly)::: application pour une utilisation ultérieure afin de rendre le composant interactif lorsqu’il est chargé dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="8f915-131">`WebAssemblyPrerendered`: Prerenders the component into static HTML and includes a marker for a :::no-loc(Blazor WebAssembly)::: app for later use to make the component interactive when loaded in the browser.</span></span>

<span data-ttu-id="8f915-132">Dans l' :::no-loc(Razor)::: exemple de pages suivantes, le `Counter` composant est rendu dans une page.</span><span class="sxs-lookup"><span data-stu-id="8f915-132">In the following :::no-loc(Razor)::: Pages example, the `Counter` component is rendered in a page.</span></span> <span data-ttu-id="8f915-133">Pour rendre le composant interactif, le :::no-loc(Blazor WebAssembly)::: script est inclus dans la section de [rendu](xref:mvc/views/layout#sections)de la page.</span><span class="sxs-lookup"><span data-stu-id="8f915-133">To make the component interactive, the :::no-loc(Blazor WebAssembly)::: script is included in the page's [render section](xref:mvc/views/layout#sections).</span></span> <span data-ttu-id="8f915-134">Pour éviter d’utiliser l’espace de noms complet du `Counter` composant avec le tag Helper Component ( `{APP ASSEMBLY}.Pages.Counter` ), ajoutez une [`@using`](xref:mvc/views/razor#using) directive pour l’espace de noms de l’application cliente `Pages` .</span><span class="sxs-lookup"><span data-stu-id="8f915-134">To avoid using the full namespace for the `Counter` component with the Component Tag Helper (`{APP ASSEMBLY}.Pages.Counter`), add an [`@using`](xref:mvc/views/razor#using) directive for the client app's `Pages` namespace.</span></span> <span data-ttu-id="8f915-135">Dans l’exemple suivant, l’espace de noms de l’application cliente est `:::no-loc(Blazor):::Hosted.Client` :</span><span class="sxs-lookup"><span data-stu-id="8f915-135">In the following example, the client app's namespace is `:::no-loc(Blazor):::Hosted.Client`:</span></span>

```cshtml
...
@using :::no-loc(Blazor):::Hosted.Client.Pages

<h1>@ViewData["Title"]</h1>

<component type="typeof(Counter)" render-mode="WebAssemblyPrerendered" />

@section Scripts {
    <script src="_framework/blazor.webassembly.js"></script>
}
```

<span data-ttu-id="8f915-136"><xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> Configure si le composant :</span><span class="sxs-lookup"><span data-stu-id="8f915-136"><xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> configures whether the component:</span></span>

* <span data-ttu-id="8f915-137">Est prérendu dans la page.</span><span class="sxs-lookup"><span data-stu-id="8f915-137">Is prerendered into the page.</span></span>
* <span data-ttu-id="8f915-138">Est rendu en HTML statique sur la page ou s’il contient les informations nécessaires pour démarrer une :::no-loc(Blazor)::: application à partir de l’agent utilisateur.</span><span class="sxs-lookup"><span data-stu-id="8f915-138">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a :::no-loc(Blazor)::: app from the user agent.</span></span>

<span data-ttu-id="8f915-139">Pour plus d’informations sur le tag Helper Component, y compris le passage de paramètres et de la <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> configuration, consultez <xref:mvc/views/tag-helpers/builtin-th/component-tag-helper> .</span><span class="sxs-lookup"><span data-stu-id="8f915-139">For more information on the Component Tag Helper, including passing parameters and <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> configuration, see <xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>.</span></span>

<span data-ttu-id="8f915-140">L’exemple précédent exige que la disposition de l’application serveur ( `_Layout.cshtml` ) inclue une [section Render](xref:mvc/views/layout#sections) ( <xref:Microsoft.AspNetCore.Mvc.:::no-loc(Razor):::.:::no-loc(Razor):::Page.RenderSection%2A> ) pour le script à l’intérieur de la `</body>` balise de fermeture :</span><span class="sxs-lookup"><span data-stu-id="8f915-140">The preceding example requires that the server app's layout (`_Layout.cshtml`) include a [render section](xref:mvc/views/layout#sections) (<xref:Microsoft.AspNetCore.Mvc.:::no-loc(Razor):::.:::no-loc(Razor):::Page.RenderSection%2A>) for the script inside the closing `</body>` tag:</span></span>

```cshtml
    ...

    @RenderSection("Scripts", required: false)
</body>
```

<span data-ttu-id="8f915-141">Le `_Layout.cshtml` fichier se trouve dans le `Pages/Shared` dossier d’une :::no-loc(Razor)::: application de pages ou `Views/Shared` d’un dossier dans une application MVC.</span><span class="sxs-lookup"><span data-stu-id="8f915-141">The `_Layout.cshtml` file is located in the `Pages/Shared` folder in a :::no-loc(Razor)::: Pages app or `Views/Shared` folder in an MVC app.</span></span>

<span data-ttu-id="8f915-142">Si l’application doit également styliser les composants avec les styles de l' :::no-loc(Blazor WebAssembly)::: application, incluez les styles de l’application dans le `_Layout.cshtml` fichier.</span><span class="sxs-lookup"><span data-stu-id="8f915-142">If the app should also style components with the styles in the :::no-loc(Blazor WebAssembly)::: app, include the app's styles in the `_Layout.cshtml` file.</span></span> <span data-ttu-id="8f915-143">Dans l’exemple suivant, l’espace de noms de l’application cliente est `:::no-loc(Blazor):::Hosted.Client` :</span><span class="sxs-lookup"><span data-stu-id="8f915-143">In the following example, the client app's namespace is `:::no-loc(Blazor):::Hosted.Client`:</span></span>

```cshtml
<head>
    ...

    <link href="css/app.css" rel="stylesheet" />
    <link href=":::no-loc(Blazor):::Hosted.Client.styles.css" rel="stylesheet" />
</head>
```

## <a name="render-components-in-a-page-or-view-with-a-css-selector"></a><span data-ttu-id="8f915-144">Afficher des composants dans une page ou une vue à l’aide d’un sélecteur CSS</span><span class="sxs-lookup"><span data-stu-id="8f915-144">Render components in a page or view with a CSS selector</span></span>

<span data-ttu-id="8f915-145">Ajoutez des composants racine au projet *client* dans `Program.Main` ( `Program.cs` ).</span><span class="sxs-lookup"><span data-stu-id="8f915-145">Add root components to the *Client* project in `Program.Main` (`Program.cs`).</span></span> <span data-ttu-id="8f915-146">Dans l’exemple suivant, le `Counter` composant est déclaré en tant que composant racine avec un sélecteur CSS qui sélectionne l’élément avec le `id` qui correspond `my-counter` .</span><span class="sxs-lookup"><span data-stu-id="8f915-146">In the following example, the `Counter` component is declared as a root component with a CSS selector that selects the element with the `id` that matches `my-counter`.</span></span> <span data-ttu-id="8f915-147">Dans l’exemple suivant, l’espace de noms de l’application cliente est `:::no-loc(Blazor):::Hosted.Client` :</span><span class="sxs-lookup"><span data-stu-id="8f915-147">In the following example, the client app's namespace is `:::no-loc(Blazor):::Hosted.Client`:</span></span>

```csharp
using :::no-loc(Blazor):::Hosted.Client.Pages;

...

builder.RootComponents.Add<Counter>("#my-counter");
```

<span data-ttu-id="8f915-148">Dans l' :::no-loc(Razor)::: exemple de pages suivantes, le `Counter` composant est rendu dans une page.</span><span class="sxs-lookup"><span data-stu-id="8f915-148">In the following :::no-loc(Razor)::: Pages example, the `Counter` component is rendered in a page.</span></span> <span data-ttu-id="8f915-149">Pour rendre le composant interactif, le :::no-loc(Blazor WebAssembly)::: script est inclus dans la [section Render](xref:mvc/views/layout#sections)de la page :</span><span class="sxs-lookup"><span data-stu-id="8f915-149">To make the component interactive, the :::no-loc(Blazor WebAssembly)::: script is included in the page's [render section](xref:mvc/views/layout#sections):</span></span>

```cshtml
...

<h1>@ViewData["Title"]</h1>

<div id="my-counter">Loading...</div>

@section Scripts {
    <script src="_framework/blazor.webassembly.js"></script>
}
```

<span data-ttu-id="8f915-150">L’exemple précédent exige que la disposition de l’application serveur ( `_Layout.cshtml` ) inclue une [section Render](xref:mvc/views/layout#sections) ( <xref:Microsoft.AspNetCore.Mvc.:::no-loc(Razor):::.:::no-loc(Razor):::Page.RenderSection%2A> ) pour le script à l’intérieur de la `</body>` balise de fermeture :</span><span class="sxs-lookup"><span data-stu-id="8f915-150">The preceding example requires that the server app's layout (`_Layout.cshtml`) include a [render section](xref:mvc/views/layout#sections) (<xref:Microsoft.AspNetCore.Mvc.:::no-loc(Razor):::.:::no-loc(Razor):::Page.RenderSection%2A>) for the script inside the closing `</body>` tag:</span></span>

```cshtml
    ...

    @RenderSection("Scripts", required: false)
</body>
```

<span data-ttu-id="8f915-151">Le `_Layout.cshtml` fichier se trouve dans le `Pages/Shared` dossier d’une :::no-loc(Razor)::: application de pages ou `Views/Shared` d’un dossier dans une application MVC.</span><span class="sxs-lookup"><span data-stu-id="8f915-151">The `_Layout.cshtml` file is located in the `Pages/Shared` folder in a :::no-loc(Razor)::: Pages app or `Views/Shared` folder in an MVC app.</span></span>

<span data-ttu-id="8f915-152">Si l’application doit également styliser les composants avec les styles de l' :::no-loc(Blazor WebAssembly)::: application, incluez les styles de l’application dans le `_Layout.cshtml` fichier.</span><span class="sxs-lookup"><span data-stu-id="8f915-152">If the app should also style components with the styles in the :::no-loc(Blazor WebAssembly)::: app, include the app's styles in the `_Layout.cshtml` file.</span></span> <span data-ttu-id="8f915-153">Dans l’exemple suivant, l’espace de noms de l’application cliente est `:::no-loc(Blazor):::Hosted.Client` :</span><span class="sxs-lookup"><span data-stu-id="8f915-153">In the following example, the client app's namespace is `:::no-loc(Blazor):::Hosted.Client`:</span></span>

```cshtml
<head>
    ...

    <link href="css/app.css" rel="stylesheet" />
    <link href=":::no-loc(Blazor):::Hosted.Client.styles.css" rel="stylesheet" />
</head>
```

::: moniker-end

::: moniker range="< aspnetcore-5.0"

<span data-ttu-id="8f915-154">L’intégration de composants dans des :::no-loc(Razor)::: :::no-loc(Razor)::: pages et des applications MVC dans une solution hébergée :::no-loc(Blazor WebAssembly)::: est prise en charge dans ASP.net core dans .net 5 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="8f915-154">Integrating :::no-loc(Razor)::: components into :::no-loc(Razor)::: Pages and MVC apps in a hosted :::no-loc(Blazor WebAssembly)::: solution is supported in ASP.NET Core in .NET 5 or later.</span></span> <span data-ttu-id="8f915-155">Sélectionnez une version .NET 5 ou ultérieure de cet article.</span><span class="sxs-lookup"><span data-stu-id="8f915-155">Select a .NET 5 or later version of this article.</span></span>

::: moniker-end

::: zone-end

::: zone pivot="server"

<span data-ttu-id="8f915-156">:::no-loc(Razor)::: les composants peuvent être intégrés dans des :::no-loc(Razor)::: pages et des applications MVC dans une :::no-loc(Blazor Server)::: application.</span><span class="sxs-lookup"><span data-stu-id="8f915-156">:::no-loc(Razor)::: components can be integrated into :::no-loc(Razor)::: Pages and MVC apps in a :::no-loc(Blazor Server)::: app.</span></span> <span data-ttu-id="8f915-157">Lorsque la page ou la vue est restituée, les composants peuvent être prérendus en même temps.</span><span class="sxs-lookup"><span data-stu-id="8f915-157">When the page or view is rendered, components can be prerendered at the same time.</span></span>

<span data-ttu-id="8f915-158">Après avoir [configuré l’application](#configuration), suivez les instructions des sections suivantes en fonction des exigences de l’application :</span><span class="sxs-lookup"><span data-stu-id="8f915-158">After [configuring the app](#configuration), use the guidance in the following sections depending on the app's requirements:</span></span>

* <span data-ttu-id="8f915-159">Composants routables : pour les composants qui sont directement routables à partir des demandes de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="8f915-159">Routable components: For components that are directly routable from user requests.</span></span> <span data-ttu-id="8f915-160">Suivez ces instructions lorsque les visiteurs doivent être en mesure de faire une requête HTTP dans leur navigateur pour un composant avec une [`@page`](xref:mvc/views/razor#page) directive.</span><span class="sxs-lookup"><span data-stu-id="8f915-160">Follow this guidance when visitors should be able to make an HTTP request in their browser for a component with an [`@page`](xref:mvc/views/razor#page) directive.</span></span>
  * [<span data-ttu-id="8f915-161">Utiliser des composants routables dans une :::no-loc(Razor)::: application pages</span><span class="sxs-lookup"><span data-stu-id="8f915-161">Use routable components in a :::no-loc(Razor)::: Pages app</span></span>](#use-routable-components-in-a-razor-pages-app)
  * [<span data-ttu-id="8f915-162">Utiliser des composants routables dans une application MVC</span><span class="sxs-lookup"><span data-stu-id="8f915-162">Use routable components in an MVC app</span></span>](#use-routable-components-in-an-mvc-app)
* <span data-ttu-id="8f915-163">[Rendez les composants à partir d’une page ou d’une vue](#render-components-from-a-page-or-view): pour les composants qui ne sont pas directement routables à partir des demandes de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="8f915-163">[Render components from a page or view](#render-components-from-a-page-or-view): For components that aren't directly routable from user requests.</span></span> <span data-ttu-id="8f915-164">Suivez ces instructions lorsque l’application incorpore des composants dans des pages et des vues existantes avec le [tag Helper Component](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="8f915-164">Follow this guidance when the app embeds components into existing pages and views with the [Component Tag Helper](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper).</span></span>

## <a name="configuration"></a><span data-ttu-id="8f915-165">Configuration</span><span class="sxs-lookup"><span data-stu-id="8f915-165">Configuration</span></span>

<span data-ttu-id="8f915-166">Une :::no-loc(Razor)::: application de pages ou MVC existante peut intégrer :::no-loc(Razor)::: des composants dans des pages et des vues :</span><span class="sxs-lookup"><span data-stu-id="8f915-166">An existing :::no-loc(Razor)::: Pages or MVC app can integrate :::no-loc(Razor)::: components into pages and views:</span></span>

1. <span data-ttu-id="8f915-167">Dans le fichier de disposition de l’application ( `_Layout.cshtml` ) :</span><span class="sxs-lookup"><span data-stu-id="8f915-167">In the app's layout file (`_Layout.cshtml`):</span></span>

   * <span data-ttu-id="8f915-168">Ajoutez la `<base>` balise suivante à l' `<head>` élément :</span><span class="sxs-lookup"><span data-stu-id="8f915-168">Add the following `<base>` tag to the `<head>` element:</span></span>

     ```html
     <base href="~/" />
     ```

     <span data-ttu-id="8f915-169">La `href` valeur (le *chemin d’accès de base* de l’application) dans l’exemple précédent suppose que l’application se trouve dans le chemin d’URL racine ( `/` ).</span><span class="sxs-lookup"><span data-stu-id="8f915-169">The `href` value (the *app base path* ) in the preceding example assumes that the app resides at the root URL path (`/`).</span></span> <span data-ttu-id="8f915-170">Si l’application est une sous-application, suivez les instructions de la section *chemin d’accès de base* de l’application de l' <xref:blazor/host-and-deploy/index#app-base-path> article.</span><span class="sxs-lookup"><span data-stu-id="8f915-170">If the app is a sub-application, follow the guidance in the *App base path* section of the <xref:blazor/host-and-deploy/index#app-base-path> article.</span></span>

     <span data-ttu-id="8f915-171">Le `_Layout.cshtml` fichier se trouve dans le `Pages/Shared` dossier d’une :::no-loc(Razor)::: application de pages ou `Views/Shared` d’un dossier dans une application MVC.</span><span class="sxs-lookup"><span data-stu-id="8f915-171">The `_Layout.cshtml` file is located in the `Pages/Shared` folder in a :::no-loc(Razor)::: Pages app or `Views/Shared` folder in an MVC app.</span></span>

   * <span data-ttu-id="8f915-172">Ajoutez une `<script>` balise pour le `blazor.server.js` script immédiatement avant la `Scripts` section Render :</span><span class="sxs-lookup"><span data-stu-id="8f915-172">Add a `<script>` tag for the `blazor.server.js` script immediately before the `Scripts` render section:</span></span>

     ```html
         ...
         <script src="_framework/blazor.server.js"></script>

         @await RenderSectionAsync("Scripts", required: false)
     </body>
     ```

     <span data-ttu-id="8f915-173">L’infrastructure ajoute le `blazor.server.js` script à l’application.</span><span class="sxs-lookup"><span data-stu-id="8f915-173">The framework adds the `blazor.server.js` script to the app.</span></span> <span data-ttu-id="8f915-174">Il n’est pas nécessaire d’ajouter manuellement le script à l’application.</span><span class="sxs-lookup"><span data-stu-id="8f915-174">There's no need to manually add the script to the app.</span></span>

1. <span data-ttu-id="8f915-175">Ajoutez un `_Imports.razor` fichier au dossier racine du projet avec le contenu suivant (modifiez le dernier espace de noms,, en lui attribuant l' `MyAppNamespace` espace de noms de l’application) :</span><span class="sxs-lookup"><span data-stu-id="8f915-175">Add an `_Imports.razor` file to the root folder of the project with the following content (change the last namespace, `MyAppNamespace`, to the namespace of the app):</span></span>

   ```razor
   @using System.Net.Http
   @using Microsoft.AspNetCore.Authorization
   @using Microsoft.AspNetCore.Components.Authorization
   @using Microsoft.AspNetCore.Components.Forms
   @using Microsoft.AspNetCore.Components.Routing
   @using Microsoft.AspNetCore.Components.Web
   @using Microsoft.JSInterop
   @using MyAppNamespace
   ```

1. <span data-ttu-id="8f915-176">Dans `Startup.ConfigureServices` , inscrivez le :::no-loc(Blazor Server)::: service :</span><span class="sxs-lookup"><span data-stu-id="8f915-176">In `Startup.ConfigureServices`, register the :::no-loc(Blazor Server)::: service:</span></span>

   ```csharp
   services.AddServerSide:::no-loc(Blazor):::();
   ```

1. <span data-ttu-id="8f915-177">Dans `Startup.Configure` , ajoutez le :::no-loc(Blazor)::: point de terminaison Hub à `app.UseEndpoints` :</span><span class="sxs-lookup"><span data-stu-id="8f915-177">In `Startup.Configure`, add the :::no-loc(Blazor)::: Hub endpoint to `app.UseEndpoints`:</span></span>

   ```csharp
   endpoints.Map:::no-loc(Blazor):::Hub();
   ```

1. <span data-ttu-id="8f915-178">Intégrer des composants dans n’importe quelle page ou vue.</span><span class="sxs-lookup"><span data-stu-id="8f915-178">Integrate components into any page or view.</span></span> <span data-ttu-id="8f915-179">Pour plus d’informations, consultez la section [rendre les composants à partir d’une page ou d’une vue](#render-components-from-a-page-or-view) .</span><span class="sxs-lookup"><span data-stu-id="8f915-179">For more information, see the [Render components from a page or view](#render-components-from-a-page-or-view) section.</span></span>

## <a name="use-routable-components-in-a-no-locrazor-pages-app"></a><span data-ttu-id="8f915-180">Utiliser des composants routables dans une :::no-loc(Razor)::: application pages</span><span class="sxs-lookup"><span data-stu-id="8f915-180">Use routable components in a :::no-loc(Razor)::: Pages app</span></span>

<span data-ttu-id="8f915-181">*Cette section concerne l’ajout de composants qui sont directement routables à partir des demandes des utilisateurs.*</span><span class="sxs-lookup"><span data-stu-id="8f915-181">*This section pertains to adding components that are directly routable from user requests.*</span></span>

<span data-ttu-id="8f915-182">Pour prendre en charge les composants routables :::no-loc(Razor)::: dans les :::no-loc(Razor)::: applications pages :</span><span class="sxs-lookup"><span data-stu-id="8f915-182">To support routable :::no-loc(Razor)::: components in :::no-loc(Razor)::: Pages apps:</span></span>

1. <span data-ttu-id="8f915-183">Suivez les instructions de la section [configuration](#configuration) .</span><span class="sxs-lookup"><span data-stu-id="8f915-183">Follow the guidance in the [Configuration](#configuration) section.</span></span>

1. <span data-ttu-id="8f915-184">Ajoutez un `App.razor` fichier à la racine du projet avec le contenu suivant :</span><span class="sxs-lookup"><span data-stu-id="8f915-184">Add an `App.razor` file to the project root with the following content:</span></span>

   ```razor
   @using Microsoft.AspNetCore.Components.Routing

   <Router AppAssembly="@typeof(Program).Assembly">
       <Found Context="routeData">
           <RouteView RouteData="routeData" />
       </Found>
       <NotFound>
           <h1>Page not found</h1>
           <p>Sorry, but there's nothing here!</p>
       </NotFound>
   </Router>
   ```

1. <span data-ttu-id="8f915-185">Ajoutez un `_Host.cshtml` fichier au `Pages` dossier avec le contenu suivant :</span><span class="sxs-lookup"><span data-stu-id="8f915-185">Add a `_Host.cshtml` file to the `Pages` folder with the following content:</span></span>

   ```cshtml
   @page "/blazor"
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="8f915-186">Les composants utilisent le `_Layout.cshtml` fichier partagé pour leur disposition.</span><span class="sxs-lookup"><span data-stu-id="8f915-186">Components use the shared `_Layout.cshtml` file for their layout.</span></span>

   <span data-ttu-id="8f915-187"><xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> Configure si le `App` composant :</span><span class="sxs-lookup"><span data-stu-id="8f915-187"><xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> configures whether the `App` component:</span></span>

   * <span data-ttu-id="8f915-188">Est prérendu dans la page.</span><span class="sxs-lookup"><span data-stu-id="8f915-188">Is prerendered into the page.</span></span>
   * <span data-ttu-id="8f915-189">Est rendu en HTML statique sur la page ou s’il contient les informations nécessaires pour démarrer une :::no-loc(Blazor)::: application à partir de l’agent utilisateur.</span><span class="sxs-lookup"><span data-stu-id="8f915-189">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a :::no-loc(Blazor)::: app from the user agent.</span></span>

   <span data-ttu-id="8f915-190">Pour plus d’informations sur le tag Helper Component, y compris le passage de paramètres et de la <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> configuration, consultez <xref:mvc/views/tag-helpers/builtin-th/component-tag-helper> .</span><span class="sxs-lookup"><span data-stu-id="8f915-190">For more information on the Component Tag Helper, including passing parameters and <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> configuration, see <xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>.</span></span>

1. <span data-ttu-id="8f915-191">Ajoutez un itinéraire de priorité basse pour la `_Host.cshtml` page à la configuration de point de terminaison dans `Startup.Configure` :</span><span class="sxs-lookup"><span data-stu-id="8f915-191">Add a low-priority route for the `_Host.cshtml` page to endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToPage("/_Host");
   });
   ```

1. <span data-ttu-id="8f915-192">Ajoutez des composants routables à l’application.</span><span class="sxs-lookup"><span data-stu-id="8f915-192">Add routable components to the app.</span></span> <span data-ttu-id="8f915-193">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="8f915-193">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

<span data-ttu-id="8f915-194">Pour plus d’informations sur les espaces de noms, consultez la section [espaces de noms de composants](#component-namespaces) .</span><span class="sxs-lookup"><span data-stu-id="8f915-194">For more information on namespaces, see the [Component namespaces](#component-namespaces) section.</span></span>

## <a name="use-routable-components-in-an-mvc-app"></a><span data-ttu-id="8f915-195">Utiliser des composants routables dans une application MVC</span><span class="sxs-lookup"><span data-stu-id="8f915-195">Use routable components in an MVC app</span></span>

<span data-ttu-id="8f915-196">*Cette section concerne l’ajout de composants qui sont directement routables à partir des demandes des utilisateurs.*</span><span class="sxs-lookup"><span data-stu-id="8f915-196">*This section pertains to adding components that are directly routable from user requests.*</span></span>

<span data-ttu-id="8f915-197">Pour prendre en charge les composants routables :::no-loc(Razor)::: dans les applications MVC :</span><span class="sxs-lookup"><span data-stu-id="8f915-197">To support routable :::no-loc(Razor)::: components in MVC apps:</span></span>

1. <span data-ttu-id="8f915-198">Suivez les instructions de la section [configuration](#configuration) .</span><span class="sxs-lookup"><span data-stu-id="8f915-198">Follow the guidance in the [Configuration](#configuration) section.</span></span>

1. <span data-ttu-id="8f915-199">Ajoutez un `App.razor` fichier à la racine du projet avec le contenu suivant :</span><span class="sxs-lookup"><span data-stu-id="8f915-199">Add an `App.razor` file to the root of the project with the following content:</span></span>

   ```razor
   @using Microsoft.AspNetCore.Components.Routing

   <Router AppAssembly="@typeof(Program).Assembly">
       <Found Context="routeData">
           <RouteView RouteData="routeData" />
       </Found>
       <NotFound>
           <h1>Page not found</h1>
           <p>Sorry, but there's nothing here!</p>
       </NotFound>
   </Router>
   ```

1. <span data-ttu-id="8f915-200">Ajoutez un `_Host.cshtml` fichier au `Views/Home` dossier avec le contenu suivant :</span><span class="sxs-lookup"><span data-stu-id="8f915-200">Add a `_Host.cshtml` file to the `Views/Home` folder with the following content:</span></span>

   ```cshtml
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="8f915-201">Les composants utilisent le `_Layout.cshtml` fichier partagé pour leur disposition.</span><span class="sxs-lookup"><span data-stu-id="8f915-201">Components use the shared `_Layout.cshtml` file for their layout.</span></span>

   <span data-ttu-id="8f915-202"><xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> Configure si le `App` composant :</span><span class="sxs-lookup"><span data-stu-id="8f915-202"><xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> configures whether the `App` component:</span></span>

   * <span data-ttu-id="8f915-203">Est prérendu dans la page.</span><span class="sxs-lookup"><span data-stu-id="8f915-203">Is prerendered into the page.</span></span>
   * <span data-ttu-id="8f915-204">Est rendu en HTML statique sur la page ou s’il contient les informations nécessaires pour démarrer une :::no-loc(Blazor)::: application à partir de l’agent utilisateur.</span><span class="sxs-lookup"><span data-stu-id="8f915-204">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a :::no-loc(Blazor)::: app from the user agent.</span></span>

   <span data-ttu-id="8f915-205">Pour plus d’informations sur le tag Helper Component, y compris le passage de paramètres et de la <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> configuration, consultez <xref:mvc/views/tag-helpers/builtin-th/component-tag-helper> .</span><span class="sxs-lookup"><span data-stu-id="8f915-205">For more information on the Component Tag Helper, including passing parameters and <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> configuration, see <xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>.</span></span>

1. <span data-ttu-id="8f915-206">Ajoutez une action au contrôleur d’hébergement :</span><span class="sxs-lookup"><span data-stu-id="8f915-206">Add an action to the Home controller:</span></span>

   ```csharp
   public IActionResult :::no-loc(Blazor):::()
   {
      return View("_Host");
   }
   ```

1. <span data-ttu-id="8f915-207">Ajoutez un itinéraire de faible priorité pour l’action de contrôleur qui retourne la `_Host.cshtml` vue à la configuration du point de terminaison dans `Startup.Configure` :</span><span class="sxs-lookup"><span data-stu-id="8f915-207">Add a low-priority route for the controller action that returns the `_Host.cshtml` view to the endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToController(":::no-loc(Blazor):::", "Home");
   });
   ```

1. <span data-ttu-id="8f915-208">Créez un `Pages` dossier et ajoutez des composants routables à l’application.</span><span class="sxs-lookup"><span data-stu-id="8f915-208">Create a `Pages` folder and add routable components to the app.</span></span> <span data-ttu-id="8f915-209">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="8f915-209">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

<span data-ttu-id="8f915-210">Pour plus d’informations sur les espaces de noms, consultez la section [espaces de noms de composants](#component-namespaces) .</span><span class="sxs-lookup"><span data-stu-id="8f915-210">For more information on namespaces, see the [Component namespaces](#component-namespaces) section.</span></span>

## <a name="render-components-from-a-page-or-view"></a><span data-ttu-id="8f915-211">Rendre les composants à partir d’une page ou d’une vue</span><span class="sxs-lookup"><span data-stu-id="8f915-211">Render components from a page or view</span></span>

<span data-ttu-id="8f915-212">*Cette section se rapporte à l’ajout de composants à des pages ou à des vues, où les composants ne sont pas directement routés à partir des demandes de l’utilisateur.*</span><span class="sxs-lookup"><span data-stu-id="8f915-212">*This section pertains to adding components to pages or views, where the components aren't directly routable from user requests.*</span></span>

<span data-ttu-id="8f915-213">Pour afficher un composant à partir d’une page ou d’une vue, utilisez le [tag Helper Component](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="8f915-213">To render a component from a page or view, use the [Component Tag Helper](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper).</span></span>

### <a name="render-stateful-interactive-components"></a><span data-ttu-id="8f915-214">Rendu des composants interactifs avec état</span><span class="sxs-lookup"><span data-stu-id="8f915-214">Render stateful interactive components</span></span>

<span data-ttu-id="8f915-215">Les composants interactifs avec état peuvent être ajoutés à une :::no-loc(Razor)::: page ou à une vue.</span><span class="sxs-lookup"><span data-stu-id="8f915-215">Stateful interactive components can be added to a :::no-loc(Razor)::: page or view.</span></span>

<span data-ttu-id="8f915-216">Lors du rendu de la page ou de la vue :</span><span class="sxs-lookup"><span data-stu-id="8f915-216">When the page or view renders:</span></span>

* <span data-ttu-id="8f915-217">Le composant est prérendu avec la page ou la vue.</span><span class="sxs-lookup"><span data-stu-id="8f915-217">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="8f915-218">L’état initial du composant utilisé pour le prérendu est perdu.</span><span class="sxs-lookup"><span data-stu-id="8f915-218">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="8f915-219">Un nouvel état de composant est créé lorsque la :::no-loc(SignalR)::: connexion est établie.</span><span class="sxs-lookup"><span data-stu-id="8f915-219">New component state is created when the :::no-loc(SignalR)::: connection is established.</span></span>

<span data-ttu-id="8f915-220">La :::no-loc(Razor)::: page suivante affiche un `Counter` composant :</span><span class="sxs-lookup"><span data-stu-id="8f915-220">The following :::no-loc(Razor)::: page renders a `Counter` component:</span></span>

```cshtml
<h1>My :::no-loc(Razor)::: Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@functions {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

<span data-ttu-id="8f915-221">Pour plus d'informations, consultez <xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>.</span><span class="sxs-lookup"><span data-stu-id="8f915-221">For more information, see <xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>.</span></span>

### <a name="render-noninteractive-components"></a><span data-ttu-id="8f915-222">Rendre les composants non interactifs</span><span class="sxs-lookup"><span data-stu-id="8f915-222">Render noninteractive components</span></span>

<span data-ttu-id="8f915-223">Dans la :::no-loc(Razor)::: page suivante, le `Counter` composant est rendu statiquement avec une valeur initiale spécifiée à l’aide d’un formulaire.</span><span class="sxs-lookup"><span data-stu-id="8f915-223">In the following :::no-loc(Razor)::: page, the `Counter` component is statically rendered with an initial value that's specified using a form.</span></span> <span data-ttu-id="8f915-224">Étant donné que le composant est rendu statiquement, le composant n’est pas interactif :</span><span class="sxs-lookup"><span data-stu-id="8f915-224">Since the component is statically rendered, the component isn't interactive:</span></span>

```cshtml
<h1>My :::no-loc(Razor)::: Page</h1>

<form>
    <input type="number" asp-for="InitialValue" />
    <button type="submit">Set initial value</button>
</form>

<component type="typeof(Counter)" render-mode="Static" 
    param-InitialValue="InitialValue" />

@functions {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

<span data-ttu-id="8f915-225">Pour plus d'informations, consultez <xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>.</span><span class="sxs-lookup"><span data-stu-id="8f915-225">For more information, see <xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>.</span></span>

## <a name="component-namespaces"></a><span data-ttu-id="8f915-226">Espaces de noms de composants</span><span class="sxs-lookup"><span data-stu-id="8f915-226">Component namespaces</span></span>

<span data-ttu-id="8f915-227">Lorsque vous utilisez un dossier personnalisé pour stocker les composants de l’application, ajoutez l’espace de noms représentant le dossier à la page/la vue ou au `_ViewImports.cshtml` fichier.</span><span class="sxs-lookup"><span data-stu-id="8f915-227">When using a custom folder to hold the app's components, add the namespace representing the folder to either the page/view or to the `_ViewImports.cshtml` file.</span></span> <span data-ttu-id="8f915-228">Dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="8f915-228">In the following example:</span></span>

* <span data-ttu-id="8f915-229">Accédez `MyAppNamespace` à l’espace de noms de l’application.</span><span class="sxs-lookup"><span data-stu-id="8f915-229">Change `MyAppNamespace` to the app's namespace.</span></span>
* <span data-ttu-id="8f915-230">Si un dossier nommé `Components` n’est pas utilisé pour contenir les composants, accédez `Components` au dossier dans lequel se trouvent les composants.</span><span class="sxs-lookup"><span data-stu-id="8f915-230">If a folder named `Components` isn't used to hold the components, change `Components` to the folder where the components reside.</span></span>

```cshtml
@using MyAppNamespace.Components
```

<span data-ttu-id="8f915-231">Le `_ViewImports.cshtml` fichier se trouve dans le `Pages` dossier d’une :::no-loc(Razor)::: application pages ou dans le `Views` dossier d’une application MVC.</span><span class="sxs-lookup"><span data-stu-id="8f915-231">The `_ViewImports.cshtml` file is located in the `Pages` folder of a :::no-loc(Razor)::: Pages app or the `Views` folder of an MVC app.</span></span>

<span data-ttu-id="8f915-232">Pour plus d'informations, consultez <xref:blazor/components/index#namespaces>.</span><span class="sxs-lookup"><span data-stu-id="8f915-232">For more information, see <xref:blazor/components/index#namespaces>.</span></span>

::: zone-end
