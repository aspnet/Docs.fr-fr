---
title: 'Présentation :::no-loc(Razor)::: des pages dans ASP.net Core'
author: Rick-Anderson
description: 'Découvrez comment :::no-loc(Razor)::: les pages de ASP.net Core rendent le codage des scénarios orientés page plus facile et plus productif que l’utilisation de MVC.'
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 02/12/2020
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
uid: razor-pages/index
ms.openlocfilehash: 89e06d00e9312a428c4e164b0dc60527fe12d904
ms.sourcegitcommit: fe5a287fa6b9477b130aa39728f82cdad57611ee
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2020
ms.locfileid: "94430872"
---
# <a name="introduction-to-no-locrazor-pages-in-aspnet-core"></a><span data-ttu-id="6c550-103">Présentation :::no-loc(Razor)::: des pages dans ASP.net Core</span><span class="sxs-lookup"><span data-stu-id="6c550-103">Introduction to :::no-loc(Razor)::: Pages in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="6c550-104">De [Rick Anderson](https://twitter.com/RickAndMSFT) et [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="6c550-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="6c550-105">:::no-loc(Razor)::: Les pages peuvent rendre le codage des scénarios orientés page plus facile et plus productif que l’utilisation de contrôleurs et de vues.</span><span class="sxs-lookup"><span data-stu-id="6c550-105">:::no-loc(Razor)::: Pages can make coding page-focused scenarios easier and more productive than using controllers and views.</span></span>

<span data-ttu-id="6c550-106">Si vous cherchez un didacticiel qui utilise l’approche Model-View-Controller, consultez [Bien démarrer avec ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="6c550-106">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="6c550-107">Ce document fournit une introduction aux :::no-loc(Razor)::: pages.</span><span class="sxs-lookup"><span data-stu-id="6c550-107">This document provides an introduction to :::no-loc(Razor)::: Pages.</span></span> <span data-ttu-id="6c550-108">Il ne s’agit pas d’un didacticiel pas à pas.</span><span class="sxs-lookup"><span data-stu-id="6c550-108">It's not a step by step tutorial.</span></span> <span data-ttu-id="6c550-109">Si vous trouvez certaines des sections trop avancées, consultez [prise en main des :::no-loc(Razor)::: pages](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="6c550-109">If you find some of the sections too advanced, see [Get started with :::no-loc(Razor)::: Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="6c550-110">Pour une vue d’ensemble d’ASP.NET Core, consultez [Introduction à ASP.NET Core](xref:index).</span><span class="sxs-lookup"><span data-stu-id="6c550-110">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6c550-111">Prérequis</span><span class="sxs-lookup"><span data-stu-id="6c550-111">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="6c550-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6c550-112">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="6c550-113">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6c550-113">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="6c550-114">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="6c550-114">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

<a name="rpvs17"></a>

## <a name="create-a-no-locrazor-pages-project"></a><span data-ttu-id="6c550-115">Créer un :::no-loc(Razor)::: projet pages</span><span class="sxs-lookup"><span data-stu-id="6c550-115">Create a :::no-loc(Razor)::: Pages project</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="6c550-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6c550-116">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6c550-117">Pour obtenir des instructions détaillées sur la création d’un projet de pages, consultez [prise en main des :::no-loc(Razor)::: pages](xref:tutorials/razor-pages/razor-pages-start) :::no-loc(Razor)::: .</span><span class="sxs-lookup"><span data-stu-id="6c550-117">See [Get started with :::no-loc(Razor)::: Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a :::no-loc(Razor)::: Pages project.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="6c550-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6c550-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="6c550-119">Exécutez `dotnet new webapp` à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="6c550-119">Run `dotnet new webapp` from the command line.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="6c550-120">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="6c550-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="6c550-121">Pour obtenir des instructions détaillées sur la création d’un projet de pages, consultez [prise en main des :::no-loc(Razor)::: pages](xref:tutorials/razor-pages/razor-pages-start) :::no-loc(Razor)::: .</span><span class="sxs-lookup"><span data-stu-id="6c550-121">See [Get started with :::no-loc(Razor)::: Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a :::no-loc(Razor)::: Pages project.</span></span>

---

## <a name="no-locrazor-pages"></a><span data-ttu-id="6c550-122">:::no-loc(Razor)::: Pages</span><span class="sxs-lookup"><span data-stu-id="6c550-122">:::no-loc(Razor)::: Pages</span></span>

<span data-ttu-id="6c550-123">:::no-loc(Razor)::: Pages est activée dans *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="6c550-123">:::no-loc(Razor)::: Pages is enabled in *Startup.cs* :</span></span>

[!code-csharp[](index/3.0sample/:::no-loc(Razor):::PagesIntro/Startup.cs?name=snippet_Startup&highlight=12,36)]

<span data-ttu-id="6c550-124">Considérez une page de base : <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="6c550-124">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/3.0sample/:::no-loc(Razor):::PagesIntro/Pages/Index.cshtml?highlight=1)]

<span data-ttu-id="6c550-125">Le code précédent ressemble beaucoup à un [ :::no-loc(Razor)::: fichier de vue](xref:tutorials/first-mvc-app/adding-view) utilisé dans une application ASP.net core avec des contrôleurs et des vues.</span><span class="sxs-lookup"><span data-stu-id="6c550-125">The preceding code looks a lot like a [:::no-loc(Razor)::: view file](xref:tutorials/first-mvc-app/adding-view) used in an ASP.NET Core app with controllers and views.</span></span> <span data-ttu-id="6c550-126">Ce qui le rend différent est la [`@page`](xref:mvc/views/razor#page) directive.</span><span class="sxs-lookup"><span data-stu-id="6c550-126">What makes it different is the [`@page`](xref:mvc/views/razor#page) directive.</span></span> <span data-ttu-id="6c550-127">`@page` fait du fichier une action MVC, ce qui signifie qu’il gère les demandes directement, sans passer par un contrôleur.</span><span class="sxs-lookup"><span data-stu-id="6c550-127">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="6c550-128">`@page` doit être la première :::no-loc(Razor)::: directive sur une page.</span><span class="sxs-lookup"><span data-stu-id="6c550-128">`@page` must be the first :::no-loc(Razor)::: directive on a page.</span></span> <span data-ttu-id="6c550-129">`@page` affecte le comportement d’autres [:::no-loc(Razor):::](xref:mvc/views/razor) constructions.</span><span class="sxs-lookup"><span data-stu-id="6c550-129">`@page` affects the behavior of other [:::no-loc(Razor):::](xref:mvc/views/razor) constructs.</span></span> <span data-ttu-id="6c550-130">:::no-loc(Razor)::: Les noms de fichiers de pages ont un suffixe *. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="6c550-130">:::no-loc(Razor)::: Pages file names have a *.cshtml* suffix.</span></span>

<span data-ttu-id="6c550-131">Une page similaire, utilisant une classe `PageModel`, est illustrée dans les deux fichiers suivants.</span><span class="sxs-lookup"><span data-stu-id="6c550-131">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="6c550-132">Le fichier *Pages/Index2.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="6c550-132">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/3.0sample/:::no-loc(Razor):::PagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="6c550-133">Le modèle de page *Pages/Index2.cshtml.cs*  :</span><span class="sxs-lookup"><span data-stu-id="6c550-133">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-csharp[](index/3.0sample/:::no-loc(Razor):::PagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="6c550-134">Par Convention, le `PageModel` fichier de classe a le même nom que le :::no-loc(Razor)::: fichier d’échange avec *. cs* ajouté.</span><span class="sxs-lookup"><span data-stu-id="6c550-134">By convention, the `PageModel` class file has the same name as the :::no-loc(Razor)::: Page file with *.cs* appended.</span></span> <span data-ttu-id="6c550-135">Par exemple, la :::no-loc(Razor)::: page précédente est *pages/index2. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6c550-135">For example, the previous :::no-loc(Razor)::: Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="6c550-136">Le fichier contenant la classe `PageModel` se nomme *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="6c550-136">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="6c550-137">Les associations des chemins d’URL aux pages sont déterminées par l’emplacement de la page dans le système de fichiers.</span><span class="sxs-lookup"><span data-stu-id="6c550-137">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="6c550-138">Le tableau suivant indique un :::no-loc(Razor)::: chemin d’accès à la page et l’URL correspondante :</span><span class="sxs-lookup"><span data-stu-id="6c550-138">The following table shows a :::no-loc(Razor)::: Page path and the matching URL:</span></span>

| <span data-ttu-id="6c550-139">Nom et chemin de fichier</span><span class="sxs-lookup"><span data-stu-id="6c550-139">File name and path</span></span>               | <span data-ttu-id="6c550-140">URL correspondante</span><span class="sxs-lookup"><span data-stu-id="6c550-140">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="6c550-141">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6c550-141">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="6c550-142">`/` ou `/Index`</span><span class="sxs-lookup"><span data-stu-id="6c550-142">`/` or `/Index`</span></span> |
| <span data-ttu-id="6c550-143">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6c550-143">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="6c550-144">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6c550-144">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="6c550-145">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6c550-145">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="6c550-146">`/Store` ou `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="6c550-146">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="6c550-147">Remarques :</span><span class="sxs-lookup"><span data-stu-id="6c550-147">Notes:</span></span>

* <span data-ttu-id="6c550-148">Le runtime recherche les :::no-loc(Razor)::: fichiers de pages dans le dossier *pages* par défaut.</span><span class="sxs-lookup"><span data-stu-id="6c550-148">The runtime looks for :::no-loc(Razor)::: Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="6c550-149">`Index` est la page par défaut quand une URL n’inclut pas de page.</span><span class="sxs-lookup"><span data-stu-id="6c550-149">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="write-a-basic-form"></a><span data-ttu-id="6c550-150">Écrire un formulaire de base</span><span class="sxs-lookup"><span data-stu-id="6c550-150">Write a basic form</span></span>

<span data-ttu-id="6c550-151">:::no-loc(Razor)::: Les pages sont conçues pour rendre les modèles courants utilisés avec les navigateurs Web faciles à implémenter lors de la création d’une application.</span><span class="sxs-lookup"><span data-stu-id="6c550-151">:::no-loc(Razor)::: Pages is designed to make common patterns used with web browsers easy to implement when building an app.</span></span> <span data-ttu-id="6c550-152">La [liaison de modèle](xref:mvc/models/model-binding), les [tag helpers](xref:mvc/views/tag-helpers/intro)et les applications auxiliaires HTML *fonctionnent tout simplement* avec les propriétés définies dans une classe de :::no-loc(Razor)::: page.</span><span class="sxs-lookup"><span data-stu-id="6c550-152">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a :::no-loc(Razor)::: Page class.</span></span> <span data-ttu-id="6c550-153">Considérez une page qui implémente un formulaire « Nous contacter » de base pour le modèle `Contact` :</span><span class="sxs-lookup"><span data-stu-id="6c550-153">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="6c550-154">Pour les exemples de ce document, le `DbContext` est initialisé dans le fichier [Startup.cs](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/3.0sample/:::no-loc(Razor):::PagesContacts/Startup.cs#L23-L24).</span><span class="sxs-lookup"><span data-stu-id="6c550-154">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/3.0sample/:::no-loc(Razor):::PagesContacts/Startup.cs#L23-L24) file.</span></span>

<span data-ttu-id="6c550-155">La base de données en mémoire requiert le `Microsoft.EntityFrameworkCore.InMemory` package NuGet.</span><span class="sxs-lookup"><span data-stu-id="6c550-155">The in memory database requires the `Microsoft.EntityFrameworkCore.InMemory` NuGet package.</span></span>

[!code-csharp[](index/3.0sample/:::no-loc(Razor):::PagesContacts/Startup.cs?name=snippet)]

<span data-ttu-id="6c550-156">Le modèle de données :</span><span class="sxs-lookup"><span data-stu-id="6c550-156">The data model:</span></span>

[!code-csharp[](index/3.0sample/:::no-loc(Razor):::PagesContacts/Models/Customer.cs)]

<span data-ttu-id="6c550-157">Le contexte de la base de données :</span><span class="sxs-lookup"><span data-stu-id="6c550-157">The db context:</span></span>

[!code-csharp[](index/3.0sample/:::no-loc(Razor):::PagesContacts/Data/CustomerDbContext.cs)]

<span data-ttu-id="6c550-158">Le fichier vue *Pages/Create.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="6c550-158">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/:::no-loc(Razor):::PagesContacts/Pages/Customers/Create.cshtml)]

<span data-ttu-id="6c550-159">Le modèle de page *Pages/Create.cshtml.cs*  :</span><span class="sxs-lookup"><span data-stu-id="6c550-159">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-csharp[](index/3.0sample/:::no-loc(Razor):::PagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="6c550-160">Par convention, la classe `PageModel` se nomme `<PageName>Model` et se trouve dans le même espace de noms que la page.</span><span class="sxs-lookup"><span data-stu-id="6c550-160">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="6c550-161">La classe `PageModel` permet de séparer la logique d’une page de sa présentation.</span><span class="sxs-lookup"><span data-stu-id="6c550-161">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="6c550-162">Elle définit des gestionnaires de page pour les demandes envoyées à la page et les données utilisées pour l’afficher.</span><span class="sxs-lookup"><span data-stu-id="6c550-162">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="6c550-163">Cette séparation permet :</span><span class="sxs-lookup"><span data-stu-id="6c550-163">This separation allows:</span></span>

* <span data-ttu-id="6c550-164">Gestion des dépendances de page via l' [injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="6c550-164">Managing of page dependencies through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
* [<span data-ttu-id="6c550-165">Effectuer des tests unitaires</span><span class="sxs-lookup"><span data-stu-id="6c550-165">Unit testing</span></span>](xref:test/razor-pages-tests)

<span data-ttu-id="6c550-166">La page a une  *méthode de gestionnaire*`OnPostAsync`, qui s’exécute sur les requêtes `POST` (quand un utilisateur poste le formulaire).</span><span class="sxs-lookup"><span data-stu-id="6c550-166">The page has an `OnPostAsync` *handler method* , which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="6c550-167">Les méthodes de gestionnaire pour tout verbe HTTP peuvent être ajoutées.</span><span class="sxs-lookup"><span data-stu-id="6c550-167">Handler methods for any HTTP verb can be added.</span></span> <span data-ttu-id="6c550-168">Les gestionnaires les plus courants sont :</span><span class="sxs-lookup"><span data-stu-id="6c550-168">The most common handlers are:</span></span>

* <span data-ttu-id="6c550-169">`OnGet` pour initialiser l’état nécessaire pour la page.</span><span class="sxs-lookup"><span data-stu-id="6c550-169">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="6c550-170">Dans le code précédent, la `OnGet` méthode affiche la page *CreateModel. cshtml* :::no-loc(Razor)::: .</span><span class="sxs-lookup"><span data-stu-id="6c550-170">In the preceding code, the `OnGet` method displays the *CreateModel.cshtml* :::no-loc(Razor)::: Page.</span></span>
* <span data-ttu-id="6c550-171">`OnPost` pour gérer les envois de formulaire.</span><span class="sxs-lookup"><span data-stu-id="6c550-171">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="6c550-172">Le suffixe de nommage `Async` est facultatif, mais souvent utilisé par convention pour les fonctions asynchrones.</span><span class="sxs-lookup"><span data-stu-id="6c550-172">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="6c550-173">Le code précédent est normal pour les :::no-loc(Razor)::: pages.</span><span class="sxs-lookup"><span data-stu-id="6c550-173">The preceding code is typical for :::no-loc(Razor)::: Pages.</span></span>

<span data-ttu-id="6c550-174">Si vous êtes familiarisé avec les applications ASP.NET à l’aide de contrôleurs et de vues :</span><span class="sxs-lookup"><span data-stu-id="6c550-174">If you're familiar with ASP.NET apps using controllers and views:</span></span>

* <span data-ttu-id="6c550-175">Le `OnPostAsync` Code de l’exemple précédent ressemble au code de contrôleur classique.</span><span class="sxs-lookup"><span data-stu-id="6c550-175">The `OnPostAsync` code in the preceding example looks similar to typical controller code.</span></span>
* <span data-ttu-id="6c550-176">La plupart des primitives MVC, telles que la [liaison de modèle](xref:mvc/models/model-binding), la [validation](xref:mvc/models/validation)et les résultats d’action, fonctionnent de la même manière avec les contrôleurs et les :::no-loc(Razor)::: pages.</span><span class="sxs-lookup"><span data-stu-id="6c550-176">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results work the same with Controllers and :::no-loc(Razor)::: Pages.</span></span> 

<span data-ttu-id="6c550-177">La méthode `OnPostAsync` précédente :</span><span class="sxs-lookup"><span data-stu-id="6c550-177">The previous `OnPostAsync` method:</span></span>

[!code-csharp[](index/3.0sample/:::no-loc(Razor):::PagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="6c550-178">Le flux de base de `OnPostAsync` :</span><span class="sxs-lookup"><span data-stu-id="6c550-178">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="6c550-179">Vérifiez s’il y a des erreurs de validation.</span><span class="sxs-lookup"><span data-stu-id="6c550-179">Check for validation errors.</span></span>

* <span data-ttu-id="6c550-180">S’il n’y a aucune erreur, enregistrez les données et redirigez.</span><span class="sxs-lookup"><span data-stu-id="6c550-180">If there are no errors, save the data and redirect.</span></span>
* <span data-ttu-id="6c550-181">S’il y a des erreurs, réaffichez la page avec des messages de validation.</span><span class="sxs-lookup"><span data-stu-id="6c550-181">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="6c550-182">Dans de nombreux cas, les erreurs de validation seraient détectées sur le client et jamais envoyées au serveur.</span><span class="sxs-lookup"><span data-stu-id="6c550-182">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="6c550-183">Le fichier vue *Pages/Create.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="6c550-183">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/:::no-loc(Razor):::PagesContacts/Pages/Customers/Create.cshtml)]

<span data-ttu-id="6c550-184">Le rendu HTML à partir de *pages/Create. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="6c550-184">The rendered HTML from *Pages/Create.cshtml* :</span></span>

[!code-html[](index/3.0sample/:::no-loc(Razor):::PagesContacts/Pages/Customers/Create4.html)]

<span data-ttu-id="6c550-185">Dans le code précédent, la publication du formulaire :</span><span class="sxs-lookup"><span data-stu-id="6c550-185">In the previous code, posting the form:</span></span>

* <span data-ttu-id="6c550-186">Avec des données valides :</span><span class="sxs-lookup"><span data-stu-id="6c550-186">With valid data:</span></span>

  * <span data-ttu-id="6c550-187">La `OnPostAsync` méthode de gestionnaire appelle la <xref:Microsoft.AspNetCore.Mvc.:::no-loc(Razor):::Pages.PageModel.RedirectToPage*> méthode d’assistance.</span><span class="sxs-lookup"><span data-stu-id="6c550-187">The `OnPostAsync` handler method calls the <xref:Microsoft.AspNetCore.Mvc.:::no-loc(Razor):::Pages.PageModel.RedirectToPage*> helper method.</span></span> <span data-ttu-id="6c550-188">`RedirectToPage` retourne une instance de <xref:Microsoft.AspNetCore.Mvc.RedirectToPageResult>.</span><span class="sxs-lookup"><span data-stu-id="6c550-188">`RedirectToPage` returns an instance of <xref:Microsoft.AspNetCore.Mvc.RedirectToPageResult>.</span></span> <span data-ttu-id="6c550-189">`RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="6c550-189">`RedirectToPage`:</span></span>

    * <span data-ttu-id="6c550-190">Est un résultat d’action.</span><span class="sxs-lookup"><span data-stu-id="6c550-190">Is an action result.</span></span>
    * <span data-ttu-id="6c550-191">Est semblable à `RedirectToAction` ou `RedirectToRoute` (utilisé dans les contrôleurs et les vues).</span><span class="sxs-lookup"><span data-stu-id="6c550-191">Is similar to `RedirectToAction` or `RedirectToRoute` (used in controllers and views).</span></span>
    * <span data-ttu-id="6c550-192">Est personnalisé pour les pages.</span><span class="sxs-lookup"><span data-stu-id="6c550-192">Is customized for pages.</span></span> <span data-ttu-id="6c550-193">Dans l’exemple précédent, il redirige vers la page Index racine (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="6c550-193">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="6c550-194">`RedirectToPage` est détaillé dans la section [génération d’URL pour les pages](#url_gen) .</span><span class="sxs-lookup"><span data-stu-id="6c550-194">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

* <span data-ttu-id="6c550-195">Avec les erreurs de validation qui sont transmises au serveur :</span><span class="sxs-lookup"><span data-stu-id="6c550-195">With validation errors that are passed to the server:</span></span>

  * <span data-ttu-id="6c550-196">La `OnPostAsync` méthode de gestionnaire appelle la <xref:Microsoft.AspNetCore.Mvc.:::no-loc(Razor):::Pages.PageBase.Page*> méthode d’assistance.</span><span class="sxs-lookup"><span data-stu-id="6c550-196">The `OnPostAsync` handler method calls the <xref:Microsoft.AspNetCore.Mvc.:::no-loc(Razor):::Pages.PageBase.Page*> helper method.</span></span> <span data-ttu-id="6c550-197">`Page` retourne une instance de <xref:Microsoft.AspNetCore.Mvc.:::no-loc(Razor):::Pages.PageResult>.</span><span class="sxs-lookup"><span data-stu-id="6c550-197">`Page` returns an instance of <xref:Microsoft.AspNetCore.Mvc.:::no-loc(Razor):::Pages.PageResult>.</span></span> <span data-ttu-id="6c550-198">Le retour de `Page` est similaire à la façon dont les actions dans les contrôleurs retournent `View`.</span><span class="sxs-lookup"><span data-stu-id="6c550-198">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="6c550-199">`PageResult` est le type de retour par défaut pour une méthode de gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="6c550-199">`PageResult` is the default return type for a handler method.</span></span> <span data-ttu-id="6c550-200">Une méthode de gestionnaire qui retourne `void` restitue la page.</span><span class="sxs-lookup"><span data-stu-id="6c550-200">A handler method that returns `void` renders the page.</span></span>
  * <span data-ttu-id="6c550-201">Dans l’exemple précédent, la publication du formulaire sans valeur entraîne le renvoi de la valeur false à [ModelState. IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) .</span><span class="sxs-lookup"><span data-stu-id="6c550-201">In the preceding example, posting the form with no value results in [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) returning false.</span></span> <span data-ttu-id="6c550-202">Dans cet exemple, aucune erreur de validation n’est affichée sur le client.</span><span class="sxs-lookup"><span data-stu-id="6c550-202">In this sample, no validation errors are displayed on the client.</span></span> <span data-ttu-id="6c550-203">La gestion des erreurs de validation est traitée plus loin dans ce document.</span><span class="sxs-lookup"><span data-stu-id="6c550-203">Validation error handing is covered later in this document.</span></span>

  [!code-csharp[](index/3.0sample/:::no-loc(Razor):::PagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=3-6)]

* <span data-ttu-id="6c550-204">Avec les erreurs de validation détectées par la validation côté client :</span><span class="sxs-lookup"><span data-stu-id="6c550-204">With validation errors detected by client side validation:</span></span>

  * <span data-ttu-id="6c550-205">Les données ne sont **pas** publiées sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="6c550-205">Data is **not** posted to the server.</span></span>
  * <span data-ttu-id="6c550-206">La validation côté client est expliquée plus loin dans ce document.</span><span class="sxs-lookup"><span data-stu-id="6c550-206">Client-side validation is explained later in this document.</span></span>

<span data-ttu-id="6c550-207">La `Customer` propriété utilise l' [`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) attribut pour s’abonner à la liaison de modèle :</span><span class="sxs-lookup"><span data-stu-id="6c550-207">The `Customer` property uses [`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) attribute to opt in to model binding:</span></span>

[!code-csharp[](index/3.0sample/:::no-loc(Razor):::PagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_PageModel&highlight=15-16)]

<span data-ttu-id="6c550-208">`[BindProperty]` ne doit **pas** être utilisé sur les modèles contenant des propriétés qui ne doivent pas être modifiées par le client.</span><span class="sxs-lookup"><span data-stu-id="6c550-208">`[BindProperty]` should **not** be used on models containing properties that should not be changed by the client.</span></span> <span data-ttu-id="6c550-209">Pour plus d’informations, consultez [survalidation](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="6c550-209">For more information, see [Overposting](xref:data/ef-rp/crud#overposting).</span></span>

<span data-ttu-id="6c550-210">:::no-loc(Razor)::: Les pages, par défaut, lient les propriétés uniquement avec des non- `GET` verbes.</span><span class="sxs-lookup"><span data-stu-id="6c550-210">:::no-loc(Razor)::: Pages, by default, bind properties only with non-`GET` verbs.</span></span> <span data-ttu-id="6c550-211">La liaison aux propriétés évite d’avoir à écrire du code pour convertir des données HTTP en type de modèle.</span><span class="sxs-lookup"><span data-stu-id="6c550-211">Binding to properties removes the need to writing code to convert HTTP data to the model type.</span></span> <span data-ttu-id="6c550-212">Elle réduit la quantité de code en utilisant la même propriété pour afficher les champs de formulaire (`<input asp-for="Customer.Name">`) et accepter l’entrée.</span><span class="sxs-lookup"><span data-stu-id="6c550-212">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name">`) and accept the input.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="6c550-213">Examen du fichier de vue *pages/Create. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="6c550-213">Reviewing the *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/:::no-loc(Razor):::PagesContacts/Pages/Customers/Create.cshtml?highlight=3,9)]

* <span data-ttu-id="6c550-214">Dans le code précédent, le [tag Helper d’entrée](xref:mvc/views/working-with-forms#the-input-tag-helper) `<input asp-for="Customer.Name" />` lie l’élément HTML `<input>` à l' `Customer.Name` expression de modèle.</span><span class="sxs-lookup"><span data-stu-id="6c550-214">In the preceding code, the [input tag helper](xref:mvc/views/working-with-forms#the-input-tag-helper) `<input asp-for="Customer.Name" />` binds the HTML `<input>` element to the `Customer.Name` model expression.</span></span>
* <span data-ttu-id="6c550-215">[`@addTagHelper`](xref:mvc/views/tag-helpers/intro#addtaghelper-makes-tag-helpers-available) rend les tag helpers disponibles.</span><span class="sxs-lookup"><span data-stu-id="6c550-215">[`@addTagHelper`](xref:mvc/views/tag-helpers/intro#addtaghelper-makes-tag-helpers-available) makes Tag Helpers available.</span></span>

### <a name="the-home-page"></a><span data-ttu-id="6c550-216">La page d’accueil</span><span class="sxs-lookup"><span data-stu-id="6c550-216">The home page</span></span>

<span data-ttu-id="6c550-217">*Index. cshtml* est la page d’hébergement :</span><span class="sxs-lookup"><span data-stu-id="6c550-217">*Index.cshtml* is the home page:</span></span>

[!code-cshtml[](index/3.0sample/:::no-loc(Razor):::PagesContacts/Pages/Customers/Index.cshtml)]

<span data-ttu-id="6c550-218">La classe `PageModel` associée ( *Index.cshtml.cs* ) :</span><span class="sxs-lookup"><span data-stu-id="6c550-218">The associated `PageModel` class ( *Index.cshtml.cs* ):</span></span>

[!code-csharp[](index/3.0sample/:::no-loc(Razor):::PagesContacts/Pages/Customers/Index.cshtml.cs?name=snippet)]

<span data-ttu-id="6c550-219">Le fichier *index. cshtml* contient le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="6c550-219">The *Index.cshtml* file contains the following markup:</span></span>

[!code-cshtml[](index/3.0sample/:::no-loc(Razor):::PagesContacts/Pages/Customers/Index.cshtml?range=21)]

<span data-ttu-id="6c550-220">Le `<a /a>` [tag Helper ancre](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) a utilisé l' `asp-route-{value}` attribut pour générer un lien vers la page de modification.</span><span class="sxs-lookup"><span data-stu-id="6c550-220">The `<a /a>` [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="6c550-221">Le lien contient des données d’itinéraire avec l’ID de contact.</span><span class="sxs-lookup"><span data-stu-id="6c550-221">The link contains route data with the contact ID.</span></span> <span data-ttu-id="6c550-222">Par exemple : `https://localhost:5001/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="6c550-222">For example, `https://localhost:5001/Edit/1`.</span></span> <span data-ttu-id="6c550-223">Les [tag helpers](xref:mvc/views/tag-helpers/intro) permettent au code côté serveur de participer à la création et au rendu des éléments HTML dans les :::no-loc(Razor)::: fichiers.</span><span class="sxs-lookup"><span data-stu-id="6c550-223">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in :::no-loc(Razor)::: files.</span></span>

<span data-ttu-id="6c550-224">Le fichier *index. cshtml* contient un balisage pour créer un bouton Supprimer pour chaque contact client :</span><span class="sxs-lookup"><span data-stu-id="6c550-224">The *Index.cshtml* file contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/3.0sample/:::no-loc(Razor):::PagesContacts/Pages/Customers/Index.cshtml?range=22-23)]

<span data-ttu-id="6c550-225">HTML rendu :</span><span class="sxs-lookup"><span data-stu-id="6c550-225">The rendered HTML:</span></span>

```html
<button type="submit" formaction="/Customers?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="6c550-226">Lorsque le bouton supprimer est rendu en HTML, son [formaction](https://developer.mozilla.org/docs/Web/HTML/Element/button#attr-formaction) comprend des paramètres pour :</span><span class="sxs-lookup"><span data-stu-id="6c550-226">When the delete button is rendered in HTML, its [formaction](https://developer.mozilla.org/docs/Web/HTML/Element/button#attr-formaction) includes parameters for:</span></span>

* <span data-ttu-id="6c550-227">ID du contact client, spécifié par l' `asp-route-id` attribut.</span><span class="sxs-lookup"><span data-stu-id="6c550-227">The customer contact ID, specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="6c550-228">`handler`, Spécifié par l' `asp-page-handler` attribut.</span><span class="sxs-lookup"><span data-stu-id="6c550-228">The `handler`, specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="6c550-229">Quand le bouton est sélectionné, une demande `POST` de forumaire est envoyée au serveur.</span><span class="sxs-lookup"><span data-stu-id="6c550-229">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="6c550-230">Par convention, le nom de la méthode de gestionnaire est sélectionné en fonction de la valeur du paramètre `handler` conformément au schéma `OnPost[handler]Async`.</span><span class="sxs-lookup"><span data-stu-id="6c550-230">By convention, the name of the handler method is selected based on the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="6c550-231">Étant donné que le `handler` est `delete` dans cet exemple, la méthode de gestionnaire `OnPostDeleteAsync` est utilisée pour traiter la demande `POST`.</span><span class="sxs-lookup"><span data-stu-id="6c550-231">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="6c550-232">Si `asp-page-handler` est défini sur une autre valeur, comme `remove`, une méthode de gestionnaire avec le nom `OnPostRemoveAsync` est sélectionnée.</span><span class="sxs-lookup"><span data-stu-id="6c550-232">If the `asp-page-handler` is set to a different value, such as `remove`, a handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-csharp[](index/3.0sample/:::no-loc(Razor):::PagesContacts/Pages/Customers/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="6c550-233">La méthode `OnPostDeleteAsync` :</span><span class="sxs-lookup"><span data-stu-id="6c550-233">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="6c550-234">Obtient le `id` de la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="6c550-234">Gets the `id` from the query string.</span></span>
* <span data-ttu-id="6c550-235">Interroge la base de données pour le contact client avec `FindAsync`.</span><span class="sxs-lookup"><span data-stu-id="6c550-235">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="6c550-236">Si le contact client est trouvé, il est supprimé et la base de données est mise à jour.</span><span class="sxs-lookup"><span data-stu-id="6c550-236">If the customer contact is found, it's removed and the database is updated.</span></span>
* <span data-ttu-id="6c550-237">Appelle <xref:Microsoft.AspNetCore.Mvc.:::no-loc(Razor):::Pages.PageModel.RedirectToPage*> pour rediriger vers la page Index racine (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="6c550-237">Calls <xref:Microsoft.AspNetCore.Mvc.:::no-loc(Razor):::Pages.PageModel.RedirectToPage*> to redirect to the root Index page (`/Index`).</span></span>

### <a name="the-editcshtml-file"></a><span data-ttu-id="6c550-238">Le fichier Edit. cshtml</span><span class="sxs-lookup"><span data-stu-id="6c550-238">The Edit.cshtml file</span></span>

[!code-cshtml[](index/3.0sample/:::no-loc(Razor):::PagesContacts/Pages/Customers/Edit.cshtml?highlight=1)]

<span data-ttu-id="6c550-239">La première ligne contient la directive `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="6c550-239">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="6c550-240">La contrainte de routage `"{id:int}"` indique à la page qu’elle doit accepter les requêtes qui contiennent des données d’itinéraire `int`.</span><span class="sxs-lookup"><span data-stu-id="6c550-240">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="6c550-241">Si une requête à la page ne contient de données d’itinéraire qui peuvent être converties en `int`, le runtime retourne une erreur HTTP 404 (introuvable).</span><span class="sxs-lookup"><span data-stu-id="6c550-241">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="6c550-242">Pour que l’ID soit facultatif, ajoutez `?` à la contrainte d’itinéraire :</span><span class="sxs-lookup"><span data-stu-id="6c550-242">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="6c550-243">Le fichier *Edit.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="6c550-243">The *Edit.cshtml.cs* file:</span></span>

[!code-csharp[](index/3.0sample/:::no-loc(Razor):::PagesContacts/Pages/Customers/Edit.cshtml.cs?name=snippet)]

## <a name="validation"></a><span data-ttu-id="6c550-244">Validation</span><span class="sxs-lookup"><span data-stu-id="6c550-244">Validation</span></span>

<span data-ttu-id="6c550-245">Règles de validation :</span><span class="sxs-lookup"><span data-stu-id="6c550-245">Validation rules:</span></span>

* <span data-ttu-id="6c550-246">Sont spécifiés de façon déclarative dans la classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="6c550-246">Are declaratively specified in the model class.</span></span>
* <span data-ttu-id="6c550-247">Sont appliquées partout dans l’application.</span><span class="sxs-lookup"><span data-stu-id="6c550-247">Are enforced everywhere in the app.</span></span>

<span data-ttu-id="6c550-248">L' <xref:System.ComponentModel.DataAnnotations> espace de noms fournit un jeu d’attributs de validation intégrés qui sont appliqués de façon déclarative à une classe ou une propriété.</span><span class="sxs-lookup"><span data-stu-id="6c550-248">The <xref:System.ComponentModel.DataAnnotations> namespace provides a set of built-in validation attributes that are applied declaratively to a class or property.</span></span> <span data-ttu-id="6c550-249">DataAnnotations contient également des attributs de mise en forme tels [`[DataType]`](xref:System.ComponentModel.DataAnnotations.DataTypeAttribute) que l’aide à la mise en forme et ne fournissent aucune validation.</span><span class="sxs-lookup"><span data-stu-id="6c550-249">DataAnnotations also contains formatting attributes like [`[DataType]`](xref:System.ComponentModel.DataAnnotations.DataTypeAttribute) that help with formatting and don't provide any validation.</span></span>

<span data-ttu-id="6c550-250">Prenons le `Customer` modèle :</span><span class="sxs-lookup"><span data-stu-id="6c550-250">Consider the `Customer` model:</span></span>

[!code-csharp[](index/3.0sample/:::no-loc(Razor):::PagesContacts/Models/Customer.cs)]

<span data-ttu-id="6c550-251">À l’aide du fichier de vue *Create. cshtml* suivant :</span><span class="sxs-lookup"><span data-stu-id="6c550-251">Using the following *Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/:::no-loc(Razor):::PagesContacts/Pages/Customers/Create3.cshtml?highlight=3,8-9,15-99)]

<span data-ttu-id="6c550-252">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="6c550-252">The preceding code:</span></span>

* <span data-ttu-id="6c550-253">Comprend des scripts de validation jQuery et jQuery.</span><span class="sxs-lookup"><span data-stu-id="6c550-253">Includes jQuery and jQuery validation scripts.</span></span>
* <span data-ttu-id="6c550-254">Utilise le `<div />` et les `<span />` [aide pour les balises](xref:mvc/views/tag-helpers/intro) pour activer :</span><span class="sxs-lookup"><span data-stu-id="6c550-254">Uses the `<div />` and `<span />` [Tag Helpers](xref:mvc/views/tag-helpers/intro) to enable:</span></span>

  * <span data-ttu-id="6c550-255">Validation côté client.</span><span class="sxs-lookup"><span data-stu-id="6c550-255">Client-side validation.</span></span>
  * <span data-ttu-id="6c550-256">Rendu des erreurs de validation.</span><span class="sxs-lookup"><span data-stu-id="6c550-256">Validation error rendering.</span></span>

* <span data-ttu-id="6c550-257">Génère le code HTML suivant :</span><span class="sxs-lookup"><span data-stu-id="6c550-257">Generates the following HTML:</span></span>

  [!code-html[](index/3.0sample/:::no-loc(Razor):::PagesContacts/Pages/Customers/Create5.html)]

<span data-ttu-id="6c550-258">La publication de la valeur créer un formulaire sans nom affiche le message d’erreur « le champ nom est obligatoire. »</span><span class="sxs-lookup"><span data-stu-id="6c550-258">Posting the Create form without a name value displays the error message "The Name field is required."</span></span> <span data-ttu-id="6c550-259">sur le formulaire.</span><span class="sxs-lookup"><span data-stu-id="6c550-259">on the form.</span></span> <span data-ttu-id="6c550-260">Si JavaScript est activé sur le client, le navigateur affiche l’erreur sans la publier sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="6c550-260">If JavaScript is enabled on the client, the browser displays the error without posting to the server.</span></span>

<span data-ttu-id="6c550-261">L' `[StringLength(10)]` attribut génère `data-val-length-max="10"` sur le rendu HTML.</span><span class="sxs-lookup"><span data-stu-id="6c550-261">The `[StringLength(10)]` attribute generates `data-val-length-max="10"` on the rendered HTML.</span></span> <span data-ttu-id="6c550-262">`data-val-length-max` empêche les navigateurs d’entrer une valeur supérieure à la longueur maximale spécifiée.</span><span class="sxs-lookup"><span data-stu-id="6c550-262">`data-val-length-max` prevents browsers from entering more than the maximum length specified.</span></span> <span data-ttu-id="6c550-263">Si un outil tel que [Fiddler](https://www.telerik.com/fiddler) est utilisé pour modifier et relire la publication :</span><span class="sxs-lookup"><span data-stu-id="6c550-263">If a tool such as [Fiddler](https://www.telerik.com/fiddler) is used to edit and replay the post:</span></span>

* <span data-ttu-id="6c550-264">Avec le nom plus long que 10.</span><span class="sxs-lookup"><span data-stu-id="6c550-264">With the name longer than 10.</span></span>
* <span data-ttu-id="6c550-265">Le message d’erreur « le nom du champ doit être une chaîne d’une longueur maximale de 10 ».</span><span class="sxs-lookup"><span data-stu-id="6c550-265">The error message "The field Name must be a string with a maximum length of 10."</span></span> <span data-ttu-id="6c550-266">» est renvoyé.</span><span class="sxs-lookup"><span data-stu-id="6c550-266">is returned.</span></span>

<span data-ttu-id="6c550-267">Prenons le `Movie` modèle suivant :</span><span class="sxs-lookup"><span data-stu-id="6c550-267">Consider the following `Movie` model:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/:::no-loc(Razor):::PagesMovie30/Models/MovieDateRatingDA.cs?name=snippet1)]

<span data-ttu-id="6c550-268">Les attributs de validation spécifient le comportement à appliquer sur les propriétés de modèle auxquelles ils sont appliqués :</span><span class="sxs-lookup"><span data-stu-id="6c550-268">The validation attributes specify behavior to enforce on the model properties they're applied to:</span></span>

* <span data-ttu-id="6c550-269">Les `Required` `MinimumLength` attributs et indiquent qu’une propriété doit avoir une valeur, mais rien n’empêche un utilisateur d’entrer un espace blanc pour satisfaire cette validation.</span><span class="sxs-lookup"><span data-stu-id="6c550-269">The `Required` and `MinimumLength` attributes indicate that a property must have a value, but nothing prevents a user from entering white space to satisfy this validation.</span></span>
* <span data-ttu-id="6c550-270">L’attribut `RegularExpression` sert à limiter les caractères pouvant être entrés.</span><span class="sxs-lookup"><span data-stu-id="6c550-270">The `RegularExpression` attribute is used to limit what characters can be input.</span></span> <span data-ttu-id="6c550-271">Dans le code précédent, « Genre » :</span><span class="sxs-lookup"><span data-stu-id="6c550-271">In the preceding code, "Genre":</span></span>

  * <span data-ttu-id="6c550-272">Doit utiliser seulement des lettres.</span><span class="sxs-lookup"><span data-stu-id="6c550-272">Must only use letters.</span></span>
  * <span data-ttu-id="6c550-273">La première lettre doit être une majuscule.</span><span class="sxs-lookup"><span data-stu-id="6c550-273">The first letter is required to be uppercase.</span></span> <span data-ttu-id="6c550-274">Les espaces, les chiffres et les caractères spéciaux ne sont pas autorisés.</span><span class="sxs-lookup"><span data-stu-id="6c550-274">White space, numbers, and special characters are not allowed.</span></span>

* <span data-ttu-id="6c550-275">L’expression `RegularExpression` « Rating » :</span><span class="sxs-lookup"><span data-stu-id="6c550-275">The `RegularExpression` "Rating":</span></span>

  * <span data-ttu-id="6c550-276">Nécessite que le premier caractère soit une lettre majuscule.</span><span class="sxs-lookup"><span data-stu-id="6c550-276">Requires that the first character be an uppercase letter.</span></span>
  * <span data-ttu-id="6c550-277">Autorise les caractères spéciaux et les nombres dans les espaces suivants.</span><span class="sxs-lookup"><span data-stu-id="6c550-277">Allows special characters and numbers in subsequent spaces.</span></span> <span data-ttu-id="6c550-278">« PG-13 » est valide pour une évaluation, mais échoue pour un « Genre ».</span><span class="sxs-lookup"><span data-stu-id="6c550-278">"PG-13" is valid for a rating, but fails for a "Genre".</span></span>

* <span data-ttu-id="6c550-279">L’attribut `Range` limite une valeur à une plage spécifiée.</span><span class="sxs-lookup"><span data-stu-id="6c550-279">The `Range` attribute constrains a value to within a specified range.</span></span>
* <span data-ttu-id="6c550-280">L' `StringLength` attribut définit la longueur maximale d’une propriété de type chaîne et, éventuellement, sa longueur minimale.</span><span class="sxs-lookup"><span data-stu-id="6c550-280">The `StringLength` attribute sets the maximum length of a string property, and optionally its minimum length.</span></span>
* <span data-ttu-id="6c550-281">Les types valeur (tels que `decimal`, `int`, `float` et `DateTime`) sont obligatoires par nature et n’ont pas besoin de l’attribut `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="6c550-281">Value types (such as `decimal`, `int`, `float`, `DateTime`) are inherently required and don't need the `[Required]` attribute.</span></span>

<span data-ttu-id="6c550-282">La page créer du modèle affiche des `Movie` Erreurs avec des valeurs non valides :</span><span class="sxs-lookup"><span data-stu-id="6c550-282">The Create page for the `Movie` model shows displays errors with invalid values:</span></span>

![Formulaire de vue Movie avec plusieurs erreurs de validation jQuery côté client](~/tutorials/razor-pages/validation/_static/val.png)

<span data-ttu-id="6c550-284">Pour plus d’informations, consultez :</span><span class="sxs-lookup"><span data-stu-id="6c550-284">For more information, see:</span></span>

* [<span data-ttu-id="6c550-285">Ajouter la validation à l’application vidéo</span><span class="sxs-lookup"><span data-stu-id="6c550-285">Add validation to the Movie app</span></span>](xref:tutorials/razor-pages/validation)
* <span data-ttu-id="6c550-286">[Validation de modèle dans ASP.net Core](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="6c550-286">[Model validation in ASP.NET Core](xref:mvc/models/validation).</span></span>

## <a name="handle-head-requests-with-an-onget-handler-fallback"></a><span data-ttu-id="6c550-287">Gérer les requêtes HEAD avec un gestionnaire OnGet de secours</span><span class="sxs-lookup"><span data-stu-id="6c550-287">Handle HEAD requests with an OnGet handler fallback</span></span>

<span data-ttu-id="6c550-288">`HEAD` les requêtes permettent de récupérer les en-têtes pour une ressource spécifique.</span><span class="sxs-lookup"><span data-stu-id="6c550-288">`HEAD` requests allow retrieving the headers for a specific resource.</span></span> <span data-ttu-id="6c550-289">Contrairement aux requêtes `GET`, les requêtes `HEAD` ne retournent pas un corps de réponse.</span><span class="sxs-lookup"><span data-stu-id="6c550-289">Unlike `GET` requests, `HEAD` requests don't return a response body.</span></span>

<span data-ttu-id="6c550-290">En règle générale, un gestionnaire `OnHead` est créé et appelé pour les requêtes `HEAD` :</span><span class="sxs-lookup"><span data-stu-id="6c550-290">Ordinarily, an `OnHead` handler is created and called for `HEAD` requests:</span></span>

[!code-csharp[](index/3.0sample/:::no-loc(Razor):::PagesContacts/Pages/Privacy.cshtml.cs?name=snippet)]

<span data-ttu-id="6c550-291">:::no-loc(Razor)::: Les pages reviennent à appeler le `OnGet` Gestionnaire si aucun `OnHead` gestionnaire n’est défini.</span><span class="sxs-lookup"><span data-stu-id="6c550-291">:::no-loc(Razor)::: Pages falls back to calling the `OnGet` handler if no `OnHead` handler is defined.</span></span>

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-no-locrazor-pages"></a><span data-ttu-id="6c550-292">XSRF/CSRF et :::no-loc(Razor)::: pages</span><span class="sxs-lookup"><span data-stu-id="6c550-292">XSRF/CSRF and :::no-loc(Razor)::: Pages</span></span>

<span data-ttu-id="6c550-293">:::no-loc(Razor)::: Les pages sont protégées par la validation anti- [contrefaçon](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="6c550-293">:::no-loc(Razor)::: Pages are protected by [Antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="6c550-294">Le [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injecte des jetons anti-contrefaçon dans des éléments de formulaire HTML.</span><span class="sxs-lookup"><span data-stu-id="6c550-294">The [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects antiforgery tokens into HTML form elements.</span></span>

<a name="layout"></a>

## <a name="using-layouts-partials-templates-and-tag-helpers-with-no-locrazor-pages"></a><span data-ttu-id="6c550-295">Utilisation de dispositions, de partiels, de modèles et d’aide pour les balises avec des :::no-loc(Razor)::: pages</span><span class="sxs-lookup"><span data-stu-id="6c550-295">Using Layouts, partials, templates, and Tag Helpers with :::no-loc(Razor)::: Pages</span></span>

<span data-ttu-id="6c550-296">Les pages fonctionnent avec toutes les fonctionnalités du :::no-loc(Razor)::: moteur d’affichage.</span><span class="sxs-lookup"><span data-stu-id="6c550-296">Pages work with all the capabilities of the :::no-loc(Razor)::: view engine.</span></span> <span data-ttu-id="6c550-297">Les mises en page, les partiels, les modèles, les tag helpers, *_ViewStart. cshtml* et *_ViewImports. cshtml* fonctionnent de la même façon que pour les :::no-loc(Razor)::: vues conventionnelles.</span><span class="sxs-lookup"><span data-stu-id="6c550-297">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml* , and *_ViewImports.cshtml* work in the same way they do for conventional :::no-loc(Razor)::: views.</span></span>

<span data-ttu-id="6c550-298">Nous allons nettoyer un peu cette page en tirant parti de certaines de ces fonctionnalités.</span><span class="sxs-lookup"><span data-stu-id="6c550-298">Let's declutter this page by taking advantage of some of those capabilities.</span></span>

<span data-ttu-id="6c550-299">Ajoutez une [page de disposition](xref:mvc/views/layout) à *Pages/Shared/_Layout.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="6c550-299">Add a [layout page](xref:mvc/views/layout) to *Pages/Shared/_Layout.cshtml* :</span></span>

[!code-cshtml[](index/3.0sample/:::no-loc(Razor):::PagesContacts/Pages/Shared/_Layout2.cshtml?hightlight=12)]

<span data-ttu-id="6c550-300">La [disposition](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="6c550-300">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="6c550-301">Contrôle la disposition de chaque page (à moins que la page ne refuse la disposition).</span><span class="sxs-lookup"><span data-stu-id="6c550-301">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="6c550-302">Importe des structures HTML telles que JavaScript et les feuilles de style.</span><span class="sxs-lookup"><span data-stu-id="6c550-302">Imports HTML structures such as JavaScript and stylesheets.</span></span>
* <span data-ttu-id="6c550-303">Le contenu de la :::no-loc(Razor)::: page est rendu où `@RenderBody()` est appelé.</span><span class="sxs-lookup"><span data-stu-id="6c550-303">The contents of the :::no-loc(Razor)::: page are rendered where `@RenderBody()` is called.</span></span>

<span data-ttu-id="6c550-304">Pour plus d’informations, consultez [page disposition](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="6c550-304">For more information, see [layout page](xref:mvc/views/layout).</span></span>

<span data-ttu-id="6c550-305">La propriété [Layout](xref:mvc/views/layout#specifying-a-layout) est définie dans *Pages/_ViewStart.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="6c550-305">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml* :</span></span>

[!code-cshtml[](index/sample/:::no-loc(Razor):::PagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="6c550-306">La disposition est dans le dossier *Pages/Shared*.</span><span class="sxs-lookup"><span data-stu-id="6c550-306">The layout is in the *Pages/Shared* folder.</span></span> <span data-ttu-id="6c550-307">Les pages recherchent d’autres vues (dispositions, modèles, partiels) hiérarchiquement, en commençant dans le même dossier que la page active.</span><span class="sxs-lookup"><span data-stu-id="6c550-307">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="6c550-308">Une mise en page dans le dossier *pages/Shared* peut être utilisée à partir de n’importe quelle :::no-loc(Razor)::: page sous le dossier *pages* .</span><span class="sxs-lookup"><span data-stu-id="6c550-308">A layout in the *Pages/Shared* folder can be used from any :::no-loc(Razor)::: page under the *Pages* folder.</span></span>

<span data-ttu-id="6c550-309">Le fichier de disposition doit être placé dans le dossier *Pages/Shared*.</span><span class="sxs-lookup"><span data-stu-id="6c550-309">The layout file should go in the *Pages/Shared* folder.</span></span>

<span data-ttu-id="6c550-310">Nous vous recommandons de **ne pas** placer le fichier de disposition dans le dossier *Views/Shared*.</span><span class="sxs-lookup"><span data-stu-id="6c550-310">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="6c550-311">*Views/Shared* est un modèle de vues MVC.</span><span class="sxs-lookup"><span data-stu-id="6c550-311">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="6c550-312">:::no-loc(Razor)::: Les pages sont conçues pour s’appuyer sur la hiérarchie des dossiers, et non sur les conventions de chemin.</span><span class="sxs-lookup"><span data-stu-id="6c550-312">:::no-loc(Razor)::: Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="6c550-313">L’affichage de la recherche à partir d’une :::no-loc(Razor)::: page comprend le dossier *pages* .</span><span class="sxs-lookup"><span data-stu-id="6c550-313">View search from a :::no-loc(Razor)::: Page includes the *Pages* folder.</span></span> <span data-ttu-id="6c550-314">Les mises en page, les modèles et les partiels utilisés avec les contrôleurs MVC et les :::no-loc(Razor)::: vues conventionnelles *fonctionnent simplement*.</span><span class="sxs-lookup"><span data-stu-id="6c550-314">The layouts, templates, and partials used with MVC controllers and conventional :::no-loc(Razor)::: views *just work*.</span></span>

<span data-ttu-id="6c550-315">Ajoutez un fichier *Pages/_ViewImports.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="6c550-315">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/:::no-loc(Razor):::PagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="6c550-316">`@namespace` est expliqué plus loin dans le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="6c550-316">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="6c550-317">La directive `@addTagHelper` permet de bénéficier des [Tag Helpers intégrés](xref:mvc/views/tag-helpers/builtin-th/Index) dans toutes les pages du dossier *Pages*.</span><span class="sxs-lookup"><span data-stu-id="6c550-317">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="6c550-318">La `@namespace` directive est définie sur une page :</span><span class="sxs-lookup"><span data-stu-id="6c550-318">The `@namespace` directive set on a page:</span></span>

[!code-cshtml[](index/sample/:::no-loc(Razor):::PagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="6c550-319">La `@namespace` directive définit l’espace de noms de la page.</span><span class="sxs-lookup"><span data-stu-id="6c550-319">The `@namespace` directive sets the namespace for the page.</span></span> <span data-ttu-id="6c550-320">La directive `@model` n’a pas besoin d’inclure l’espace de noms.</span><span class="sxs-lookup"><span data-stu-id="6c550-320">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="6c550-321">Quand la directive `@namespace` est contenue dans *_ViewImports.cshtml* , l’espace de noms spécifié fournit le préfixe de l’espace de noms généré dans la Page qui importe la directive `@namespace`.</span><span class="sxs-lookup"><span data-stu-id="6c550-321">When the `@namespace` directive is contained in *_ViewImports.cshtml* , the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="6c550-322">Le reste de l’espace de noms généré (la partie suffixe) est le chemin relatif séparé par un point entre le dossier contenant *_ViewImports.cshtml* et le dossier contenant la page.</span><span class="sxs-lookup"><span data-stu-id="6c550-322">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="6c550-323">Par exemple, la classe `PageModel`*Pages/Customers/Edit.cshtml.cs* définit explicitement l’espace de noms :</span><span class="sxs-lookup"><span data-stu-id="6c550-323">For example, the `PageModel` class *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-csharp[](index/sample/:::no-loc(Razor):::PagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="6c550-324">Le fichier *Pages/_ViewImports.cshtml* définit l’espace de noms suivant :</span><span class="sxs-lookup"><span data-stu-id="6c550-324">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/:::no-loc(Razor):::PagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="6c550-325">L’espace de noms généré pour la page *pages/Customers/Edit. cshtml* :::no-loc(Razor)::: est le même que la `PageModel` classe.</span><span class="sxs-lookup"><span data-stu-id="6c550-325">The generated namespace for the *Pages/Customers/Edit.cshtml* :::no-loc(Razor)::: Page is the same as the `PageModel` class.</span></span>

<span data-ttu-id="6c550-326">`@namespace`*fonctionne également avec les :::no-loc(Razor)::: vues conventionnelles.*</span><span class="sxs-lookup"><span data-stu-id="6c550-326">`@namespace` *also works with conventional :::no-loc(Razor)::: views.*</span></span>

<span data-ttu-id="6c550-327">Examinez le fichier de vue *pages/Create. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="6c550-327">Consider the *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/:::no-loc(Razor):::PagesContacts/Pages/Customers/Create3.cshtml?highlight=2-3)]

<span data-ttu-id="6c550-328">Le fichier de vue *pages/Create. cshtml* mis à jour avec *_ViewImports. cshtml* et le fichier de disposition précédent :</span><span class="sxs-lookup"><span data-stu-id="6c550-328">The updated *Pages/Create.cshtml* view file with *_ViewImports.cshtml* and the preceding layout file:</span></span>

[!code-cshtml[](index/3.0sample/:::no-loc(Razor):::PagesContacts/Pages/Customers/Create4.cshtml?highlight=2)]

<span data-ttu-id="6c550-329">Dans le code précédent, le *_ViewImports. cshtml* a importé l’espace de noms et les tag helpers.</span><span class="sxs-lookup"><span data-stu-id="6c550-329">In the preceding code, the *_ViewImports.cshtml* imported the namespace and Tag Helpers.</span></span> <span data-ttu-id="6c550-330">Le fichier de disposition a importé les fichiers JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6c550-330">The layout file imported the JavaScript files.</span></span>

<span data-ttu-id="6c550-331">Le [ :::no-loc(Razor)::: projet de démarrage pages](#rpvs17) contient les *pages/_ValidationScriptsPartial. cshtml* , qui raccorde la validation côté client.</span><span class="sxs-lookup"><span data-stu-id="6c550-331">The [:::no-loc(Razor)::: Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml* , which hooks up client-side validation.</span></span>

<span data-ttu-id="6c550-332">Pour plus d'informations sur les affichages partiels, consultez <xref:mvc/views/partial>.</span><span class="sxs-lookup"><span data-stu-id="6c550-332">For more information on partial views, see <xref:mvc/views/partial>.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="6c550-333">Génération d’URL pour les pages</span><span class="sxs-lookup"><span data-stu-id="6c550-333">URL generation for Pages</span></span>

<span data-ttu-id="6c550-334">La page `Create`, illustrée précédemment, utilise `RedirectToPage` :</span><span class="sxs-lookup"><span data-stu-id="6c550-334">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-csharp[](index/3.0sample/:::no-loc(Razor):::PagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_PageModel&highlight=28)]

<span data-ttu-id="6c550-335">L’application a la structure de fichiers/dossiers suivante :</span><span class="sxs-lookup"><span data-stu-id="6c550-335">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="6c550-336">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="6c550-336">*/Pages*</span></span>

  * <span data-ttu-id="6c550-337">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6c550-337">*Index.cshtml*</span></span>
  * <span data-ttu-id="6c550-338">*Privacy. cshtml*</span><span class="sxs-lookup"><span data-stu-id="6c550-338">*Privacy.cshtml*</span></span>
  * <span data-ttu-id="6c550-339">*/Customers*</span><span class="sxs-lookup"><span data-stu-id="6c550-339">*/Customers*</span></span>

    * <span data-ttu-id="6c550-340">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6c550-340">*Create.cshtml*</span></span>
    * <span data-ttu-id="6c550-341">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6c550-341">*Edit.cshtml*</span></span>
    * <span data-ttu-id="6c550-342">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6c550-342">*Index.cshtml*</span></span>

<span data-ttu-id="6c550-343">Les pages *pages/Customers/Create. cshtml* et *pages/Customers/Edit. cshtml* redirigent vers *pages/Customers/index. cshtml* après réussite.</span><span class="sxs-lookup"><span data-stu-id="6c550-343">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Customers/Index.cshtml* after success.</span></span> <span data-ttu-id="6c550-344">La chaîne `./Index` est un nom de page relatif utilisé pour accéder à la page précédente.</span><span class="sxs-lookup"><span data-stu-id="6c550-344">The string `./Index` is a relative page name used to access the preceding page.</span></span> <span data-ttu-id="6c550-345">Elle est utilisée pour générer des URL dans la page *pages/Customers/index. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="6c550-345">It is used to generate URLs to the *Pages/Customers/Index.cshtml* page.</span></span> <span data-ttu-id="6c550-346">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="6c550-346">For example:</span></span>

* `Url.Page("./Index", ...)`
* `<a asp-page="./Index">Customers Index Page</a>`
* `RedirectToPage("./Index")`

<span data-ttu-id="6c550-347">Le nom de page absolu `/Index` est utilisé pour générer des URL dans la page *pages/index. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="6c550-347">The absolute page name `/Index` is used to generate URLs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="6c550-348">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="6c550-348">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">Home Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="6c550-349">Le nom de la page est le chemin de la page à partir du dossier racine */Pages* avec un `/` devant (par exemple, `/Index`).</span><span class="sxs-lookup"><span data-stu-id="6c550-349">The page name is the path to the page from the root */Pages* folder including a leading `/` (for example, `/Index`).</span></span> <span data-ttu-id="6c550-350">Les exemples de génération d’URL précédents offrent des options améliorées et des fonctionnalités fonctionnelles par rapport au codage en dur d’une URL.</span><span class="sxs-lookup"><span data-stu-id="6c550-350">The preceding URL generation samples offer enhanced options and functional capabilities over hard-coding a URL.</span></span> <span data-ttu-id="6c550-351">La génération d’URL utilise le [routage](xref:mvc/controllers/routing) et peut générer et encoder des paramètres en fonction de la façon dont l’itinéraire est défini dans le chemin de destination.</span><span class="sxs-lookup"><span data-stu-id="6c550-351">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="6c550-352">La génération d’URL pour les pages prend en charge les noms relatifs.</span><span class="sxs-lookup"><span data-stu-id="6c550-352">URL generation for pages supports relative names.</span></span> <span data-ttu-id="6c550-353">Le tableau suivant indique quelle page d’index est sélectionnée à l’aide `RedirectToPage` de différents paramètres dans *pages/Customers/Create. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6c550-353">The following table shows which Index page is selected using different `RedirectToPage` parameters in *Pages/Customers/Create.cshtml*.</span></span>

| <span data-ttu-id="6c550-354">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="6c550-354">RedirectToPage(x)</span></span>| <span data-ttu-id="6c550-355">Page</span><span class="sxs-lookup"><span data-stu-id="6c550-355">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="6c550-356">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="6c550-356">RedirectToPage("/Index")</span></span> | <span data-ttu-id="6c550-357">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="6c550-357">*Pages/Index*</span></span> |
| <span data-ttu-id="6c550-358">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="6c550-358">RedirectToPage("./Index");</span></span> | <span data-ttu-id="6c550-359">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="6c550-359">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="6c550-360">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="6c550-360">RedirectToPage("../Index")</span></span> | <span data-ttu-id="6c550-361">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="6c550-361">*Pages/Index*</span></span> |
| <span data-ttu-id="6c550-362">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="6c550-362">RedirectToPage("Index")</span></span>  | <span data-ttu-id="6c550-363">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="6c550-363">*Pages/Customers/Index*</span></span> |

<!-- Test via ~/razor-pages/index/3.0sample/:::no-loc(Razor):::PagesContacts/Pages/Customers/Details.cshtml.cs -->

<span data-ttu-id="6c550-364">`RedirectToPage("Index")`, `RedirectToPage("./Index")` et `RedirectToPage("../Index")` sont des *noms relatifs*.</span><span class="sxs-lookup"><span data-stu-id="6c550-364">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")` are *relative names*.</span></span> <span data-ttu-id="6c550-365">Le paramètre `RedirectToPage` est *combiné* avec le chemin de la page active pour calculer le nom de la page de destination.</span><span class="sxs-lookup"><span data-stu-id="6c550-365">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>

<span data-ttu-id="6c550-366">La liaison de nom relatif est utile lors de la création de sites avec une structure complexe.</span><span class="sxs-lookup"><span data-stu-id="6c550-366">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="6c550-367">Lorsque des noms relatifs sont utilisés pour établir une liaison entre les pages d’un dossier :</span><span class="sxs-lookup"><span data-stu-id="6c550-367">When relative names are used to link between pages in a folder:</span></span>

* <span data-ttu-id="6c550-368">Le changement de nom d’un dossier n’interrompt pas les liens relatifs.</span><span class="sxs-lookup"><span data-stu-id="6c550-368">Renaming a folder doesn't break the relative links.</span></span>
* <span data-ttu-id="6c550-369">Les liens ne sont pas rompus, car ils n’incluent pas le nom du dossier.</span><span class="sxs-lookup"><span data-stu-id="6c550-369">Links are not broken because they don't include the folder name.</span></span>

<span data-ttu-id="6c550-370">Pour rediriger vers une page située dans une autre [Zone](xref:mvc/controllers/areas), spécifiez la zone :</span><span class="sxs-lookup"><span data-stu-id="6c550-370">To redirect to a page in a different [Area](xref:mvc/controllers/areas), specify the area:</span></span>

```csharp
RedirectToPage("/Index", new { area = "Services" });
```

<span data-ttu-id="6c550-371">Pour plus d’informations, consultez <xref:mvc/controllers/areas> et <xref:razor-pages/razor-pages-conventions>.</span><span class="sxs-lookup"><span data-stu-id="6c550-371">For more information, see <xref:mvc/controllers/areas> and <xref:razor-pages/razor-pages-conventions>.</span></span>

## <a name="viewdata-attribute"></a><span data-ttu-id="6c550-372">Attribut ViewData</span><span class="sxs-lookup"><span data-stu-id="6c550-372">ViewData attribute</span></span>

<span data-ttu-id="6c550-373">Les données peuvent être passées à une page avec <xref:Microsoft.AspNetCore.Mvc.ViewDataAttribute> .</span><span class="sxs-lookup"><span data-stu-id="6c550-373">Data can be passed to a page with <xref:Microsoft.AspNetCore.Mvc.ViewDataAttribute>.</span></span> <span data-ttu-id="6c550-374">Les valeurs des propriétés avec l' `[ViewData]` attribut sont stockées et chargées à partir de <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ViewDataDictionary> .</span><span class="sxs-lookup"><span data-stu-id="6c550-374">Properties with the `[ViewData]` attribute have their values stored and loaded from the <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ViewDataDictionary>.</span></span>

<span data-ttu-id="6c550-375">Dans l’exemple suivant, le `AboutModel` applique l' `[ViewData]` attribut à la `Title` propriété :</span><span class="sxs-lookup"><span data-stu-id="6c550-375">In the following example, the `AboutModel` applies the `[ViewData]` attribute to the `Title` property:</span></span>

```csharp
public class AboutModel : PageModel
{
    [ViewData]
    public string Title { get; } = "About";

    public void OnGet()
    {
    }
}
```

<span data-ttu-id="6c550-376">Dans la page À propos de, accédez à la propriété `Title` en tant que propriété de modèle :</span><span class="sxs-lookup"><span data-stu-id="6c550-376">In the About page, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="6c550-377">Dans la disposition, le titre est lu à partir du dictionnaire ViewData :</span><span class="sxs-lookup"><span data-stu-id="6c550-377">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

## <a name="tempdata"></a><span data-ttu-id="6c550-378">TempData</span><span class="sxs-lookup"><span data-stu-id="6c550-378">TempData</span></span>

<span data-ttu-id="6c550-379">ASP.NET Core expose <xref:Microsoft.AspNetCore.Mvc.Controller.TempData> .</span><span class="sxs-lookup"><span data-stu-id="6c550-379">ASP.NET Core exposes the <xref:Microsoft.AspNetCore.Mvc.Controller.TempData>.</span></span> <span data-ttu-id="6c550-380">Cette propriété stocke les données jusqu’à ce qu’elles soient lues.</span><span class="sxs-lookup"><span data-stu-id="6c550-380">This property stores data until it's read.</span></span> <span data-ttu-id="6c550-381">Vous pouvez utiliser les méthodes <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Keep*> et <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Peek*> pour examiner les données sans suppression.</span><span class="sxs-lookup"><span data-stu-id="6c550-381">The <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Keep*> and <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Peek*> methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="6c550-382">`TempData` est utile pour la redirection, quand des données sont nécessaires pour plusieurs requêtes.</span><span class="sxs-lookup"><span data-stu-id="6c550-382">`TempData` is useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="6c550-383">Le code suivant définit la valeur de `Message` à l’aide de `TempData` :</span><span class="sxs-lookup"><span data-stu-id="6c550-383">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-csharp[](index/sample/:::no-loc(Razor):::PagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="6c550-384">Le balisage suivant dans le fichier *Pages/Customers/Index.cshtml* affiche la valeur de `Message` à l’aide de `TempData`.</span><span class="sxs-lookup"><span data-stu-id="6c550-384">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="6c550-385">Le modèle de page *Pages/Customers/Index.cshtml.cs* applique l’attribut `[TempData]` à la propriété `Message`.</span><span class="sxs-lookup"><span data-stu-id="6c550-385">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```csharp
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="6c550-386">Pour plus d’informations, consultez [TempData](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="6c550-386">For more information, see [TempData](xref:fundamentals/app-state#tempdata).</span></span>

<a name="mhpp"></a>

## <a name="multiple-handlers-per-page"></a><span data-ttu-id="6c550-387">Plusieurs gestionnaires par page</span><span class="sxs-lookup"><span data-stu-id="6c550-387">Multiple handlers per page</span></span>

<span data-ttu-id="6c550-388">La page suivante génère un balisage pour deux gestionnaires en utilisant le Tag Helper `asp-page-handler` :</span><span class="sxs-lookup"><span data-stu-id="6c550-388">The following page generates markup for two handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/:::no-loc(Razor):::PagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<span data-ttu-id="6c550-389">Le formulaire dans l’exemple précédent contient deux boutons d’envoi, chacun utilisant `FormActionTagHelper` pour envoyer à une URL différente.</span><span class="sxs-lookup"><span data-stu-id="6c550-389">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="6c550-390">L’attribut `asp-page-handler` est un complément de `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="6c550-390">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="6c550-391">`asp-page-handler` génère des URL qui envoient à chacune des méthodes de gestionnaire définies par une page.</span><span class="sxs-lookup"><span data-stu-id="6c550-391">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="6c550-392">`asp-page` n’est pas spécifié, car l’exemple établit une liaison à la page active.</span><span class="sxs-lookup"><span data-stu-id="6c550-392">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="6c550-393">Le modèle de page :</span><span class="sxs-lookup"><span data-stu-id="6c550-393">The page model:</span></span>

[!code-csharp[](index/sample/:::no-loc(Razor):::PagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="6c550-394">Le code précédent utilise des *méthodes de gestionnaire nommées*.</span><span class="sxs-lookup"><span data-stu-id="6c550-394">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="6c550-395">Pour créer des méthodes de gestionnaire nommées, il faut prendre le texte dans le nom après `On<HTTP Verb>` et avant `Async` (le cas échéant).</span><span class="sxs-lookup"><span data-stu-id="6c550-395">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="6c550-396">Dans l’exemple précédent, les méthodes de page sont OnPost **JoinList** Async et OnPost **JoinListUC** Async.</span><span class="sxs-lookup"><span data-stu-id="6c550-396">In the preceding example, the page methods are OnPost **JoinList** Async and OnPost **JoinListUC** Async.</span></span> <span data-ttu-id="6c550-397">Avec *OnPost* et *Async* supprimés, les noms des gestionnaires sont `JoinList` et `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="6c550-397">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/:::no-loc(Razor):::PagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="6c550-398">Avec le code précédent, le chemin d’URL qui envoie à `OnPostJoinListAsync` est `https://localhost:5001/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="6c550-398">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="6c550-399">Le chemin d’URL qui envoie à `OnPostJoinListUCAsync` est `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="6c550-399">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="custom-routes"></a><span data-ttu-id="6c550-400">Routes personnalisées</span><span class="sxs-lookup"><span data-stu-id="6c550-400">Custom routes</span></span>

<span data-ttu-id="6c550-401">Utilisez la directive `@page` pour :</span><span class="sxs-lookup"><span data-stu-id="6c550-401">Use the `@page` directive to:</span></span>

* <span data-ttu-id="6c550-402">Spécifier une route personnalisée vers une page.</span><span class="sxs-lookup"><span data-stu-id="6c550-402">Specify a custom route to a page.</span></span> <span data-ttu-id="6c550-403">Par exemple, la route vers la page À propos peut être définie sur `/Some/Other/Path` avec `@page "/Some/Other/Path"`.</span><span class="sxs-lookup"><span data-stu-id="6c550-403">For example, the route to the About page can be set to `/Some/Other/Path` with `@page "/Some/Other/Path"`.</span></span>
* <span data-ttu-id="6c550-404">Ajouter des segments à la route par défaut d’une page.</span><span class="sxs-lookup"><span data-stu-id="6c550-404">Append segments to a page's default route.</span></span> <span data-ttu-id="6c550-405">Par exemple, un segment « item » peut être ajouté à la route par défaut d’une page avec `@page "item"`.</span><span class="sxs-lookup"><span data-stu-id="6c550-405">For example, an "item" segment can be added to a page's default route with `@page "item"`.</span></span>
* <span data-ttu-id="6c550-406">Ajouter des paramètres à la route par défaut d’une page.</span><span class="sxs-lookup"><span data-stu-id="6c550-406">Append parameters to a page's default route.</span></span> <span data-ttu-id="6c550-407">Par exemple, un paramètre d’ID, `id`, peut être nécessaire pour une page avec `@page "{id}"`.</span><span class="sxs-lookup"><span data-stu-id="6c550-407">For example, an ID parameter, `id`, can be required for a page with `@page "{id}"`.</span></span>

<span data-ttu-id="6c550-408">Un chemin relatif racine désigné par un tilde (`~`) au début du chemin est pris en charge.</span><span class="sxs-lookup"><span data-stu-id="6c550-408">A root-relative path designated by a tilde (`~`) at the beginning of the path is supported.</span></span> <span data-ttu-id="6c550-409">Par exemple, `@page "~/Some/Other/Path"` est identique à `@page "/Some/Other/Path"`.</span><span class="sxs-lookup"><span data-stu-id="6c550-409">For example, `@page "~/Some/Other/Path"` is the same as `@page "/Some/Other/Path"`.</span></span>

<span data-ttu-id="6c550-410">Si vous n’aimez pas la chaîne `?handler=JoinList` de requête dans l’URL, modifiez l’itinéraire pour placer le nom du gestionnaire dans la partie chemin d’accès de l’URL.</span><span class="sxs-lookup"><span data-stu-id="6c550-410">If you don't like the query string `?handler=JoinList` in the URL, change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="6c550-411">L’itinéraire peut être personnalisé en ajoutant un modèle de routage entre guillemets doubles après la `@page` directive.</span><span class="sxs-lookup"><span data-stu-id="6c550-411">The route can be customized by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/:::no-loc(Razor):::PagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="6c550-412">Avec le code précédent, le chemin d’URL qui envoie à `OnPostJoinListAsync` est `https://localhost:5001/Customers/CreateFATH/JoinList`.</span><span class="sxs-lookup"><span data-stu-id="6c550-412">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH/JoinList`.</span></span> <span data-ttu-id="6c550-413">Le chemin d’URL qui envoie à `OnPostJoinListUCAsync` est `https://localhost:5001/Customers/CreateFATH/JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="6c550-413">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH/JoinListUC`.</span></span>

<span data-ttu-id="6c550-414">Le `?` suivant `handler` signifie que le paramètre d’itinéraire est facultatif.</span><span class="sxs-lookup"><span data-stu-id="6c550-414">The `?` following `handler` means the route parameter is optional.</span></span>

## <a name="advanced-configuration-and-settings"></a><span data-ttu-id="6c550-415">Configuration et paramètres avancés</span><span class="sxs-lookup"><span data-stu-id="6c550-415">Advanced configuration and settings</span></span>

<span data-ttu-id="6c550-416">La configuration et les paramètres des sections suivantes ne sont pas requis par la plupart des applications.</span><span class="sxs-lookup"><span data-stu-id="6c550-416">The configuration and settings in following sections is not required by most apps.</span></span>

<span data-ttu-id="6c550-417">Pour configurer des options avancées, utilisez la surcharge qui est configurée <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.Add:::no-loc(Razor):::Pages%2A> <xref:Microsoft.AspNetCore.Mvc.:::no-loc(Razor):::Pages.:::no-loc(Razor):::PagesOptions> :</span><span class="sxs-lookup"><span data-stu-id="6c550-417">To configure advanced options, use the <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.Add:::no-loc(Razor):::Pages%2A> overload that configures <xref:Microsoft.AspNetCore.Mvc.:::no-loc(Razor):::Pages.:::no-loc(Razor):::PagesOptions>:</span></span>

[!code-csharp[](index/3.0sample/:::no-loc(Razor):::PagesContacts/StartupRPoptions.cs?name=snippet)]

<span data-ttu-id="6c550-418">Utilisez <xref:Microsoft.AspNetCore.Mvc.:::no-loc(Razor):::Pages.:::no-loc(Razor):::PagesOptions> pour définir le répertoire racine pour les pages ou ajouter des conventions de modèle d’application pour les pages.</span><span class="sxs-lookup"><span data-stu-id="6c550-418">Use the <xref:Microsoft.AspNetCore.Mvc.:::no-loc(Razor):::Pages.:::no-loc(Razor):::PagesOptions> to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="6c550-419">Pour plus d’informations sur les conventions, consultez [ :::no-loc(Razor)::: conventions d’autorisation des pages](xref:security/authorization/razor-pages-authorization).</span><span class="sxs-lookup"><span data-stu-id="6c550-419">For more information on conventions, see [:::no-loc(Razor)::: Pages authorization conventions](xref:security/authorization/razor-pages-authorization).</span></span>

<span data-ttu-id="6c550-420">Pour précompiler des vues, consultez [ :::no-loc(Razor)::: afficher la compilation](xref:mvc/views/view-compilation).</span><span class="sxs-lookup"><span data-stu-id="6c550-420">To precompile views, see [:::no-loc(Razor)::: view compilation](xref:mvc/views/view-compilation).</span></span>

### <a name="specify-that-no-locrazor-pages-are-at-the-content-root"></a><span data-ttu-id="6c550-421">Spécifier que :::no-loc(Razor)::: les pages se trouvent à la racine du contenu</span><span class="sxs-lookup"><span data-stu-id="6c550-421">Specify that :::no-loc(Razor)::: Pages are at the content root</span></span>

<span data-ttu-id="6c550-422">Par défaut, :::no-loc(Razor)::: les pages sont enracinées dans le répertoire */pages*</span><span class="sxs-lookup"><span data-stu-id="6c550-422">By default, :::no-loc(Razor)::: Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="6c550-423">Ajouter <xref:Microsoft.Extensions.DependencyInjection.Mvc:::no-loc(Razor):::PagesMvcBuilderExtensions.With:::no-loc(Razor):::PagesAtContentRoot*> pour spécifier que vos :::no-loc(Razor)::: pages se trouvent à la [racine du contenu](xref:fundamentals/index#content-root) ( <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath> ) de l’application :</span><span class="sxs-lookup"><span data-stu-id="6c550-423">Add <xref:Microsoft.Extensions.DependencyInjection.Mvc:::no-loc(Razor):::PagesMvcBuilderExtensions.With:::no-loc(Razor):::PagesAtContentRoot*> to specify that your :::no-loc(Razor)::: Pages are at the [content root](xref:fundamentals/index#content-root) (<xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>) of the app:</span></span>

[!code-csharp[](index/3.0sample/:::no-loc(Razor):::PagesContacts/StartupWith:::no-loc(Razor):::PagesAtContentRoot.cs?name=snippet)]

### <a name="specify-that-no-locrazor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="6c550-424">Spécifier que :::no-loc(Razor)::: les pages se trouvent dans un répertoire racine personnalisé</span><span class="sxs-lookup"><span data-stu-id="6c550-424">Specify that :::no-loc(Razor)::: Pages are at a custom root directory</span></span>

<span data-ttu-id="6c550-425">Ajouter <xref:Microsoft.Extensions.DependencyInjection.Mvc:::no-loc(Razor):::PagesMvcCoreBuilderExtensions.With:::no-loc(Razor):::PagesRoot*> pour spécifier que :::no-loc(Razor)::: les pages se trouvent dans un répertoire racine personnalisé dans l’application (fournissez un chemin d’accès relatif) :</span><span class="sxs-lookup"><span data-stu-id="6c550-425">Add <xref:Microsoft.Extensions.DependencyInjection.Mvc:::no-loc(Razor):::PagesMvcCoreBuilderExtensions.With:::no-loc(Razor):::PagesRoot*> to specify that :::no-loc(Razor)::: Pages are at a custom root directory in the app (provide a relative path):</span></span>

[!code-csharp[](index/3.0sample/:::no-loc(Razor):::PagesContacts/StartupWith:::no-loc(Razor):::PagesRoot.cs?name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="6c550-426">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="6c550-426">Additional resources</span></span>

* <span data-ttu-id="6c550-427">Consultez [prise en main des :::no-loc(Razor)::: pages](xref:tutorials/razor-pages/razor-pages-start), qui s’appuie sur cette introduction.</span><span class="sxs-lookup"><span data-stu-id="6c550-427">See [Get started with :::no-loc(Razor)::: Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>
* [<span data-ttu-id="6c550-428">Autoriser l’attribut et les :::no-loc(Razor)::: pages</span><span class="sxs-lookup"><span data-stu-id="6c550-428">Authorize attribute and :::no-loc(Razor)::: Pages</span></span>](xref:security/authorization/simple#aarp)
* [<span data-ttu-id="6c550-429">Télécharger ou afficher l’exemple de code</span><span class="sxs-lookup"><span data-stu-id="6c550-429">Download or view sample code</span></span>](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/3.0sample)
* <xref:index>
* <xref:mvc/views/razor>
* <xref:mvc/controllers/areas>
* <xref:tutorials/razor-pages/razor-pages-start>
* <xref:security/authorization/razor-pages-authorization>
* <xref:razor-pages/razor-pages-conventions>
* <xref:test/razor-pages-tests>
* <xref:mvc/views/partial>
* <xref:blazor/components/prerendering-and-integration>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="6c550-430">De [Rick Anderson](https://twitter.com/RickAndMSFT) et [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="6c550-430">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="6c550-431">:::no-loc(Razor)::: Pages est un nouvel aspect de ASP.NET Core MVC qui rend le codage des scénarios orientés page plus facile et plus productif.</span><span class="sxs-lookup"><span data-stu-id="6c550-431">:::no-loc(Razor)::: Pages is a new aspect of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="6c550-432">Si vous cherchez un didacticiel qui utilise l’approche Model-View-Controller, consultez [Bien démarrer avec ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="6c550-432">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="6c550-433">Ce document fournit une introduction aux :::no-loc(Razor)::: pages.</span><span class="sxs-lookup"><span data-stu-id="6c550-433">This document provides an introduction to :::no-loc(Razor)::: Pages.</span></span> <span data-ttu-id="6c550-434">Il ne s’agit pas d’un didacticiel pas à pas.</span><span class="sxs-lookup"><span data-stu-id="6c550-434">It's not a step by step tutorial.</span></span> <span data-ttu-id="6c550-435">Si vous trouvez certaines des sections trop avancées, consultez [prise en main des :::no-loc(Razor)::: pages](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="6c550-435">If you find some of the sections too advanced, see [Get started with :::no-loc(Razor)::: Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="6c550-436">Pour une vue d’ensemble d’ASP.NET Core, consultez [Introduction à ASP.NET Core](xref:index).</span><span class="sxs-lookup"><span data-stu-id="6c550-436">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6c550-437">Prérequis</span><span class="sxs-lookup"><span data-stu-id="6c550-437">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="6c550-438">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6c550-438">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="6c550-439">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6c550-439">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="6c550-440">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="6c550-440">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

<a name="rpvs17"></a>

## <a name="create-a-no-locrazor-pages-project"></a><span data-ttu-id="6c550-441">Créer un :::no-loc(Razor)::: projet pages</span><span class="sxs-lookup"><span data-stu-id="6c550-441">Create a :::no-loc(Razor)::: Pages project</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="6c550-442">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6c550-442">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6c550-443">Pour obtenir des instructions détaillées sur la création d’un projet de pages, consultez [prise en main des :::no-loc(Razor)::: pages](xref:tutorials/razor-pages/razor-pages-start) :::no-loc(Razor)::: .</span><span class="sxs-lookup"><span data-stu-id="6c550-443">See [Get started with :::no-loc(Razor)::: Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a :::no-loc(Razor)::: Pages project.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="6c550-444">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="6c550-444">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="6c550-445">Exécutez `dotnet new webapp` à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="6c550-445">Run `dotnet new webapp` from the command line.</span></span>

<span data-ttu-id="6c550-446">Ouvrez le fichier *.csproj* généré à partir de Visual Studio pour Mac.</span><span class="sxs-lookup"><span data-stu-id="6c550-446">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="6c550-447">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6c550-447">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="6c550-448">Exécutez `dotnet new webapp` à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="6c550-448">Run `dotnet new webapp` from the command line.</span></span>

---

## <a name="no-locrazor-pages"></a><span data-ttu-id="6c550-449">:::no-loc(Razor)::: Pages</span><span class="sxs-lookup"><span data-stu-id="6c550-449">:::no-loc(Razor)::: Pages</span></span>

<span data-ttu-id="6c550-450">:::no-loc(Razor)::: Pages est activée dans *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="6c550-450">:::no-loc(Razor)::: Pages is enabled in *Startup.cs* :</span></span>

[!code-csharp[](index/sample/:::no-loc(Razor):::PagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="6c550-451">Considérez une page de base : <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="6c550-451">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/sample/:::no-loc(Razor):::PagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="6c550-452">Le code précédent ressemble beaucoup à un [ :::no-loc(Razor)::: fichier de vue](xref:tutorials/first-mvc-app/adding-view) utilisé dans une application ASP.net core avec des contrôleurs et des vues.</span><span class="sxs-lookup"><span data-stu-id="6c550-452">The preceding code looks a lot like a [:::no-loc(Razor)::: view file](xref:tutorials/first-mvc-app/adding-view) used in an ASP.NET Core app with controllers and views.</span></span> <span data-ttu-id="6c550-453">Ce qui le rend différent est la directive `@page`.</span><span class="sxs-lookup"><span data-stu-id="6c550-453">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="6c550-454">`@page` fait du fichier une action MVC, ce qui signifie qu’il gère les demandes directement, sans passer par un contrôleur.</span><span class="sxs-lookup"><span data-stu-id="6c550-454">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="6c550-455">`@page` doit être la première :::no-loc(Razor)::: directive sur une page.</span><span class="sxs-lookup"><span data-stu-id="6c550-455">`@page` must be the first :::no-loc(Razor)::: directive on a page.</span></span> <span data-ttu-id="6c550-456">`@page` affecte le comportement d’autres :::no-loc(Razor)::: constructions.</span><span class="sxs-lookup"><span data-stu-id="6c550-456">`@page` affects the behavior of other :::no-loc(Razor)::: constructs.</span></span>

<span data-ttu-id="6c550-457">Une page similaire, utilisant une classe `PageModel`, est illustrée dans les deux fichiers suivants.</span><span class="sxs-lookup"><span data-stu-id="6c550-457">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="6c550-458">Le fichier *Pages/Index2.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="6c550-458">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/sample/:::no-loc(Razor):::PagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="6c550-459">Le modèle de page *Pages/Index2.cshtml.cs*  :</span><span class="sxs-lookup"><span data-stu-id="6c550-459">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-csharp[](index/sample/:::no-loc(Razor):::PagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="6c550-460">Par Convention, le `PageModel` fichier de classe a le même nom que le :::no-loc(Razor)::: fichier d’échange avec *. cs* ajouté.</span><span class="sxs-lookup"><span data-stu-id="6c550-460">By convention, the `PageModel` class file has the same name as the :::no-loc(Razor)::: Page file with *.cs* appended.</span></span> <span data-ttu-id="6c550-461">Par exemple, la :::no-loc(Razor)::: page précédente est *pages/index2. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6c550-461">For example, the previous :::no-loc(Razor)::: Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="6c550-462">Le fichier contenant la classe `PageModel` se nomme *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="6c550-462">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="6c550-463">Les associations des chemins d’URL aux pages sont déterminées par l’emplacement de la page dans le système de fichiers.</span><span class="sxs-lookup"><span data-stu-id="6c550-463">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="6c550-464">Le tableau suivant indique un :::no-loc(Razor)::: chemin d’accès à la page et l’URL correspondante :</span><span class="sxs-lookup"><span data-stu-id="6c550-464">The following table shows a :::no-loc(Razor)::: Page path and the matching URL:</span></span>

| <span data-ttu-id="6c550-465">Nom et chemin de fichier</span><span class="sxs-lookup"><span data-stu-id="6c550-465">File name and path</span></span>               | <span data-ttu-id="6c550-466">URL correspondante</span><span class="sxs-lookup"><span data-stu-id="6c550-466">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="6c550-467">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6c550-467">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="6c550-468">`/` ou `/Index`</span><span class="sxs-lookup"><span data-stu-id="6c550-468">`/` or `/Index`</span></span> |
| <span data-ttu-id="6c550-469">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6c550-469">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="6c550-470">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6c550-470">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="6c550-471">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6c550-471">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="6c550-472">`/Store` ou `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="6c550-472">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="6c550-473">Remarques :</span><span class="sxs-lookup"><span data-stu-id="6c550-473">Notes:</span></span>

* <span data-ttu-id="6c550-474">Le runtime recherche les :::no-loc(Razor)::: fichiers de pages dans le dossier *pages* par défaut.</span><span class="sxs-lookup"><span data-stu-id="6c550-474">The runtime looks for :::no-loc(Razor)::: Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="6c550-475">`Index` est la page par défaut quand une URL n’inclut pas de page.</span><span class="sxs-lookup"><span data-stu-id="6c550-475">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="write-a-basic-form"></a><span data-ttu-id="6c550-476">Écrire un formulaire de base</span><span class="sxs-lookup"><span data-stu-id="6c550-476">Write a basic form</span></span>

<span data-ttu-id="6c550-477">:::no-loc(Razor)::: Les pages sont conçues pour rendre les modèles courants utilisés avec les navigateurs Web faciles à implémenter lors de la création d’une application.</span><span class="sxs-lookup"><span data-stu-id="6c550-477">:::no-loc(Razor)::: Pages is designed to make common patterns used with web browsers easy to implement when building an app.</span></span> <span data-ttu-id="6c550-478">La [liaison de modèle](xref:mvc/models/model-binding), les [tag helpers](xref:mvc/views/tag-helpers/intro)et les applications auxiliaires HTML *fonctionnent tout simplement* avec les propriétés définies dans une classe de :::no-loc(Razor)::: page.</span><span class="sxs-lookup"><span data-stu-id="6c550-478">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a :::no-loc(Razor)::: Page class.</span></span> <span data-ttu-id="6c550-479">Considérez une page qui implémente un formulaire « Nous contacter » de base pour le modèle `Contact` :</span><span class="sxs-lookup"><span data-stu-id="6c550-479">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="6c550-480">Pour les exemples de ce document, le `DbContext` est initialisé dans le fichier [Startup.cs](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/sample/:::no-loc(Razor):::PagesContacts/Startup.cs#L15-L16).</span><span class="sxs-lookup"><span data-stu-id="6c550-480">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/sample/:::no-loc(Razor):::PagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-csharp[](index/sample/:::no-loc(Razor):::PagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="6c550-481">Le modèle de données :</span><span class="sxs-lookup"><span data-stu-id="6c550-481">The data model:</span></span>

[!code-csharp[](index/sample/:::no-loc(Razor):::PagesContacts/Data/Customer.cs)]

<span data-ttu-id="6c550-482">Le contexte de la base de données :</span><span class="sxs-lookup"><span data-stu-id="6c550-482">The db context:</span></span>

[!code-csharp[](index/sample/:::no-loc(Razor):::PagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="6c550-483">Le fichier vue *Pages/Create.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="6c550-483">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/:::no-loc(Razor):::PagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="6c550-484">Le modèle de page *Pages/Create.cshtml.cs*  :</span><span class="sxs-lookup"><span data-stu-id="6c550-484">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-csharp[](index/sample/:::no-loc(Razor):::PagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="6c550-485">Par convention, la classe `PageModel` se nomme `<PageName>Model` et se trouve dans le même espace de noms que la page.</span><span class="sxs-lookup"><span data-stu-id="6c550-485">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="6c550-486">La classe `PageModel` permet de séparer la logique d’une page de sa présentation.</span><span class="sxs-lookup"><span data-stu-id="6c550-486">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="6c550-487">Elle définit des gestionnaires de page pour les demandes envoyées à la page et les données utilisées pour l’afficher.</span><span class="sxs-lookup"><span data-stu-id="6c550-487">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="6c550-488">Cette séparation permet :</span><span class="sxs-lookup"><span data-stu-id="6c550-488">This separation allows:</span></span>

* <span data-ttu-id="6c550-489">Gestion des dépendances de page via l' [injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="6c550-489">Managing of page dependencies through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="6c550-490">[Test unitaire](xref:test/razor-pages-tests) des pages.</span><span class="sxs-lookup"><span data-stu-id="6c550-490">[Unit testing](xref:test/razor-pages-tests) the pages.</span></span>

<span data-ttu-id="6c550-491">La page a une  *méthode de gestionnaire*`OnPostAsync`, qui s’exécute sur les requêtes `POST` (quand un utilisateur poste le formulaire).</span><span class="sxs-lookup"><span data-stu-id="6c550-491">The page has an `OnPostAsync` *handler method* , which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="6c550-492">Vous pouvez ajouter des méthodes de gestionnaire pour n’importe quel verbe HTTP.</span><span class="sxs-lookup"><span data-stu-id="6c550-492">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="6c550-493">Les gestionnaires les plus courants sont :</span><span class="sxs-lookup"><span data-stu-id="6c550-493">The most common handlers are:</span></span>

* <span data-ttu-id="6c550-494">`OnGet` pour initialiser l’état nécessaire pour la page.</span><span class="sxs-lookup"><span data-stu-id="6c550-494">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="6c550-495">Exemple [OnGet](#OnGet).</span><span class="sxs-lookup"><span data-stu-id="6c550-495">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="6c550-496">`OnPost` pour gérer les envois de formulaire.</span><span class="sxs-lookup"><span data-stu-id="6c550-496">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="6c550-497">Le suffixe de nommage `Async` est facultatif, mais souvent utilisé par convention pour les fonctions asynchrones.</span><span class="sxs-lookup"><span data-stu-id="6c550-497">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="6c550-498">Le code précédent est normal pour les :::no-loc(Razor)::: pages.</span><span class="sxs-lookup"><span data-stu-id="6c550-498">The preceding code is typical for :::no-loc(Razor)::: Pages.</span></span>

<span data-ttu-id="6c550-499">Si vous êtes familiarisé avec les applications ASP.NET à l’aide de contrôleurs et de vues :</span><span class="sxs-lookup"><span data-stu-id="6c550-499">If you're familiar with ASP.NET apps using controllers and views:</span></span>

* <span data-ttu-id="6c550-500">Le `OnPostAsync` Code de l’exemple précédent ressemble au code de contrôleur classique.</span><span class="sxs-lookup"><span data-stu-id="6c550-500">The `OnPostAsync` code in the preceding example looks similar to typical controller code.</span></span>
* <span data-ttu-id="6c550-501">La plupart des primitives MVC, telles que la [liaison de modèle](xref:mvc/models/model-binding), la [validation](xref:mvc/models/validation), la [validation](xref:mvc/models/validation)et les résultats d’action, sont partagées.</span><span class="sxs-lookup"><span data-stu-id="6c550-501">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), [Validation](xref:mvc/models/validation),  and action results are shared.</span></span>

<span data-ttu-id="6c550-502">La méthode `OnPostAsync` précédente :</span><span class="sxs-lookup"><span data-stu-id="6c550-502">The previous `OnPostAsync` method:</span></span>

[!code-csharp[](index/sample/:::no-loc(Razor):::PagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="6c550-503">Le flux de base de `OnPostAsync` :</span><span class="sxs-lookup"><span data-stu-id="6c550-503">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="6c550-504">Vérifiez s’il y a des erreurs de validation.</span><span class="sxs-lookup"><span data-stu-id="6c550-504">Check for validation errors.</span></span>

* <span data-ttu-id="6c550-505">S’il n’y a aucune erreur, enregistrez les données et redirigez.</span><span class="sxs-lookup"><span data-stu-id="6c550-505">If there are no errors, save the data and redirect.</span></span>
* <span data-ttu-id="6c550-506">S’il y a des erreurs, réaffichez la page avec des messages de validation.</span><span class="sxs-lookup"><span data-stu-id="6c550-506">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="6c550-507">La validation côté client est identique à celle des applications ASP.NET Core MVC traditionnelles.</span><span class="sxs-lookup"><span data-stu-id="6c550-507">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="6c550-508">Dans de nombreux cas, les erreurs de validation seraient détectées sur le client et jamais envoyées au serveur.</span><span class="sxs-lookup"><span data-stu-id="6c550-508">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="6c550-509">Quand les données sont entrées correctement, la méthode de gestionnaire `OnPostAsync` appelle la méthode d’assistance `RedirectToPage` pour retourner une instance de `RedirectToPageResult`.</span><span class="sxs-lookup"><span data-stu-id="6c550-509">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="6c550-510">`RedirectToPage` est un nouveau résultat d’action, semblable à `RedirectToAction` ou `RedirectToRoute`, mais personnalisé pour les pages.</span><span class="sxs-lookup"><span data-stu-id="6c550-510">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="6c550-511">Dans l’exemple précédent, il redirige vers la page Index racine (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="6c550-511">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="6c550-512">`RedirectToPage` est détaillé dans la section [génération d’URL pour les pages](#url_gen) .</span><span class="sxs-lookup"><span data-stu-id="6c550-512">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="6c550-513">Quand le formulaire envoyé comporte des erreurs de validation (qui sont passées au serveur), la méthode de gestionnaire `OnPostAsync` appelle la méthode d’assistance `Page`.</span><span class="sxs-lookup"><span data-stu-id="6c550-513">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="6c550-514">`Page` retourne une instance de `PageResult`.</span><span class="sxs-lookup"><span data-stu-id="6c550-514">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="6c550-515">Le retour de `Page` est similaire à la façon dont les actions dans les contrôleurs retournent `View`.</span><span class="sxs-lookup"><span data-stu-id="6c550-515">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="6c550-516">`PageResult` est le type de retour par défaut pour une méthode de gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="6c550-516">`PageResult` is the default return type for a handler method.</span></span> <span data-ttu-id="6c550-517">Une méthode de gestionnaire qui retourne `void` restitue la page.</span><span class="sxs-lookup"><span data-stu-id="6c550-517">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="6c550-518">La propriété `Customer` utilise l’attribut `[BindProperty]` pour accepter la liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="6c550-518">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-csharp[](index/sample/:::no-loc(Razor):::PagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="6c550-519">:::no-loc(Razor)::: Les pages, par défaut, lient les propriétés uniquement avec des non- `GET` verbes.</span><span class="sxs-lookup"><span data-stu-id="6c550-519">:::no-loc(Razor)::: Pages, by default, bind properties only with non-`GET` verbs.</span></span> <span data-ttu-id="6c550-520">La liaison à des propriétés peut réduire la quantité de code à écrire.</span><span class="sxs-lookup"><span data-stu-id="6c550-520">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="6c550-521">Elle réduit la quantité de code en utilisant la même propriété pour afficher les champs de formulaire (`<input asp-for="Customer.Name">`) et accepter l’entrée.</span><span class="sxs-lookup"><span data-stu-id="6c550-521">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name">`) and accept the input.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="6c550-522">La page d’accueil ( *Index.cshtml* ) :</span><span class="sxs-lookup"><span data-stu-id="6c550-522">The home page ( *Index.cshtml* ):</span></span>

[!code-cshtml[](index/sample/:::no-loc(Razor):::PagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="6c550-523">La classe `PageModel` associée ( *Index.cshtml.cs* ) :</span><span class="sxs-lookup"><span data-stu-id="6c550-523">The associated `PageModel` class ( *Index.cshtml.cs* ):</span></span>

[!code-csharp[](index/sample/:::no-loc(Razor):::PagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="6c550-524">Le fichier *Index.cshtml* contient le balisage suivant pour créer un lien d’édition pour chaque contact :</span><span class="sxs-lookup"><span data-stu-id="6c550-524">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[](index/sample/:::no-loc(Razor):::PagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="6c550-525">Le `<a asp-page="./Edit" asp-route-id="@contact.Id">Edit</a>` [tag Helper ancre](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) a utilisé l' `asp-route-{value}` attribut pour générer un lien vers la page de modification.</span><span class="sxs-lookup"><span data-stu-id="6c550-525">The `<a asp-page="./Edit" asp-route-id="@contact.Id">Edit</a>` [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="6c550-526">Le lien contient des données d’itinéraire avec l’ID de contact.</span><span class="sxs-lookup"><span data-stu-id="6c550-526">The link contains route data with the contact ID.</span></span> <span data-ttu-id="6c550-527">Par exemple : `https://localhost:5001/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="6c550-527">For example, `https://localhost:5001/Edit/1`.</span></span> <span data-ttu-id="6c550-528">Les [tag helpers](xref:mvc/views/tag-helpers/intro) permettent au code côté serveur de participer à la création et au rendu des éléments HTML dans les :::no-loc(Razor)::: fichiers.</span><span class="sxs-lookup"><span data-stu-id="6c550-528">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in :::no-loc(Razor)::: files.</span></span> <span data-ttu-id="6c550-529">Les Tag Helpers sont activés par `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span><span class="sxs-lookup"><span data-stu-id="6c550-529">Tag Helpers are enabled by `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span></span>

<span data-ttu-id="6c550-530">Le fichier *Pages/Edit.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="6c550-530">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[](index/sample/:::no-loc(Razor):::PagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="6c550-531">La première ligne contient la directive `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="6c550-531">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="6c550-532">La contrainte de routage `"{id:int}"` indique à la page qu’elle doit accepter les requêtes qui contiennent des données d’itinéraire `int`.</span><span class="sxs-lookup"><span data-stu-id="6c550-532">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="6c550-533">Si une requête à la page ne contient de données d’itinéraire qui peuvent être converties en `int`, le runtime retourne une erreur HTTP 404 (introuvable).</span><span class="sxs-lookup"><span data-stu-id="6c550-533">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="6c550-534">Pour que l’ID soit facultatif, ajoutez `?` à la contrainte d’itinéraire :</span><span class="sxs-lookup"><span data-stu-id="6c550-534">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="6c550-535">Le fichier *Pages/Edit.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="6c550-535">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-csharp[](index/sample/:::no-loc(Razor):::PagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="6c550-536">Le fichier *Index.cshtml* contient également le balisage pour créer un bouton Supprimer pour chaque contact client :</span><span class="sxs-lookup"><span data-stu-id="6c550-536">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/sample/:::no-loc(Razor):::PagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="6c550-537">Lorsque le bouton Supprimer est rendu en HTML, son `formaction` comprend des paramètres pour :</span><span class="sxs-lookup"><span data-stu-id="6c550-537">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="6c550-538">L’ID du contact client spécifié par l’attribut `asp-route-id`.</span><span class="sxs-lookup"><span data-stu-id="6c550-538">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="6c550-539">Le `handler` spécifié par l’attribut `asp-page-handler`.</span><span class="sxs-lookup"><span data-stu-id="6c550-539">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="6c550-540">Voici un exemple de bouton Supprimer rendu avec un ID de contact client de `1`:</span><span class="sxs-lookup"><span data-stu-id="6c550-540">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="6c550-541">Quand le bouton est sélectionné, une demande `POST` de forumaire est envoyée au serveur.</span><span class="sxs-lookup"><span data-stu-id="6c550-541">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="6c550-542">Par convention, le nom de la méthode de gestionnaire est sélectionné en fonction de la valeur du paramètre `handler` conformément au schéma `OnPost[handler]Async`.</span><span class="sxs-lookup"><span data-stu-id="6c550-542">By convention, the name of the handler method is selected based on the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="6c550-543">Étant donné que le `handler` est `delete` dans cet exemple, la méthode de gestionnaire `OnPostDeleteAsync` est utilisée pour traiter la demande `POST`.</span><span class="sxs-lookup"><span data-stu-id="6c550-543">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="6c550-544">Si `asp-page-handler` est défini sur une autre valeur, comme `remove`, une méthode de gestionnaire avec le nom `OnPostRemoveAsync` est sélectionnée.</span><span class="sxs-lookup"><span data-stu-id="6c550-544">If the `asp-page-handler` is set to a different value, such as `remove`, a handler method with the name `OnPostRemoveAsync` is selected.</span></span> <span data-ttu-id="6c550-545">Le code suivant illustre le `OnPostDeleteAsync` Gestionnaire :</span><span class="sxs-lookup"><span data-stu-id="6c550-545">The following code shows the `OnPostDeleteAsync` handler:</span></span>

[!code-csharp[](index/sample/:::no-loc(Razor):::PagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="6c550-546">La méthode `OnPostDeleteAsync` :</span><span class="sxs-lookup"><span data-stu-id="6c550-546">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="6c550-547">Accepte l’`id` de la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="6c550-547">Accepts the `id` from the query string.</span></span> <span data-ttu-id="6c550-548">Si la directive de la page *index. cshtml* contenait la contrainte `"{id:int?}"` de routage, `id` provient des données d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="6c550-548">If the *Index.cshtml* page directive contained routing constraint `"{id:int?}"`, `id` would come from route data.</span></span> <span data-ttu-id="6c550-549">Les données d’itinéraire pour `id` sont spécifiées dans l’URI, par exemple `https://localhost:5001/Customers/2` .</span><span class="sxs-lookup"><span data-stu-id="6c550-549">The route data for `id` is specified in the URI such as `https://localhost:5001/Customers/2`.</span></span>
* <span data-ttu-id="6c550-550">Interroge la base de données pour le contact client avec `FindAsync`.</span><span class="sxs-lookup"><span data-stu-id="6c550-550">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="6c550-551">Si le contact client est trouvé, il est supprimé de la liste des contacts client.</span><span class="sxs-lookup"><span data-stu-id="6c550-551">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="6c550-552">La base de données est mise à jour.</span><span class="sxs-lookup"><span data-stu-id="6c550-552">The database is updated.</span></span>
* <span data-ttu-id="6c550-553">Appelle `RedirectToPage` pour rediriger vers la page Index racine (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="6c550-553">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

## <a name="mark-page-properties-as-required"></a><span data-ttu-id="6c550-554">Marquer les propriétés de page comme Required</span><span class="sxs-lookup"><span data-stu-id="6c550-554">Mark page properties as required</span></span>

<span data-ttu-id="6c550-555">Les propriétés d’un `PageModel` peuvent être marquées avec l’attribut [Required](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) :</span><span class="sxs-lookup"><span data-stu-id="6c550-555">Properties on a `PageModel` can be marked with the [Required](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) attribute:</span></span>

[!code-csharp[](index/sample/Create.cshtml.cs?highlight=3,15-16)]

<span data-ttu-id="6c550-556">Pour plus d’informations, consultez [Validation de modèle](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="6c550-556">For more information, see [Model validation](xref:mvc/models/validation).</span></span>

## <a name="handle-head-requests-with-an-onget-handler-fallback"></a><span data-ttu-id="6c550-557">Gérer les requêtes HEAD avec un gestionnaire OnGet de secours</span><span class="sxs-lookup"><span data-stu-id="6c550-557">Handle HEAD requests with an OnGet handler fallback</span></span>

<span data-ttu-id="6c550-558">Les requêtes `HEAD` vous permettent de récupérer les en-têtes pour une ressource spécifique.</span><span class="sxs-lookup"><span data-stu-id="6c550-558">`HEAD` requests allow you to retrieve the headers for a specific resource.</span></span> <span data-ttu-id="6c550-559">Contrairement aux requêtes `GET`, les requêtes `HEAD` ne retournent pas un corps de réponse.</span><span class="sxs-lookup"><span data-stu-id="6c550-559">Unlike `GET` requests, `HEAD` requests don't return a response body.</span></span>

<span data-ttu-id="6c550-560">En règle générale, un gestionnaire `OnHead` est créé et appelé pour les requêtes `HEAD` :</span><span class="sxs-lookup"><span data-stu-id="6c550-560">Ordinarily, an `OnHead` handler is created and called for `HEAD` requests:</span></span> 

```csharp
public void OnHead()
{
    HttpContext.Response.Headers.Add("HandledBy", "Handled by OnHead!");
}
```

<span data-ttu-id="6c550-561">Dans ASP.NET Core 2,1 ou version ultérieure, :::no-loc(Razor)::: les pages reviennent à appeler le `OnGet` Gestionnaire si aucun `OnHead` gestionnaire n’est défini.</span><span class="sxs-lookup"><span data-stu-id="6c550-561">In ASP.NET Core 2.1 or later, :::no-loc(Razor)::: Pages falls back to calling the `OnGet` handler if no `OnHead` handler is defined.</span></span> <span data-ttu-id="6c550-562">Ce comportement est activé par l’appel à [SetCompatibilityVersion](xref:mvc/compatibility-version) dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="6c550-562">This behavior is enabled by the call to [SetCompatibilityVersion](xref:mvc/compatibility-version) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
```

<span data-ttu-id="6c550-563">Les modèles par défaut génèrent l'appel `SetCompatibilityVersion` dans ASP.NET Core 2.1 et 2.2.</span><span class="sxs-lookup"><span data-stu-id="6c550-563">The default templates generate the `SetCompatibilityVersion` call in ASP.NET Core 2.1 and 2.2.</span></span> <span data-ttu-id="6c550-564">`SetCompatibilityVersion` affecte efficacement :::no-loc(Razor)::: à l’option pages la `AllowMappingHeadRequestsToGetHandler` valeur `true` .</span><span class="sxs-lookup"><span data-stu-id="6c550-564">`SetCompatibilityVersion` effectively sets the :::no-loc(Razor)::: Pages option `AllowMappingHeadRequestsToGetHandler` to `true`.</span></span>

<span data-ttu-id="6c550-565">Au lieu d’adhérer à tous les comportements avec `SetCompatibilityVersion`, vous pouvez adhérer explicitement à des comportements *spécifiques*.</span><span class="sxs-lookup"><span data-stu-id="6c550-565">Rather than opting in to all behaviors with `SetCompatibilityVersion`, you can explicitly opt in to *specific* behaviors.</span></span> <span data-ttu-id="6c550-566">Le code suivant adhère à l’autorisation de mappage des requêtes `HEAD` au gestionnaire `OnGet` :</span><span class="sxs-lookup"><span data-stu-id="6c550-566">The following code opts in to allowing `HEAD` requests to be mapped to the `OnGet` handler:</span></span>

```csharp
services.AddMvc()
    .Add:::no-loc(Razor):::PagesOptions(options =>
    {
        options.AllowMappingHeadRequestsToGetHandler = true;
    });
```

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-no-locrazor-pages"></a><span data-ttu-id="6c550-567">XSRF/CSRF et :::no-loc(Razor)::: pages</span><span class="sxs-lookup"><span data-stu-id="6c550-567">XSRF/CSRF and :::no-loc(Razor)::: Pages</span></span>

<span data-ttu-id="6c550-568">Vous n’avez aucun code à écrire pour la [validation anti-contrefaçon](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="6c550-568">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="6c550-569">La génération et la validation des jetons anti-contrefaçon sont automatiquement incluses dans les :::no-loc(Razor)::: pages.</span><span class="sxs-lookup"><span data-stu-id="6c550-569">Antiforgery token generation and validation are automatically included in :::no-loc(Razor)::: Pages.</span></span>

<a name="layout"></a>

## <a name="using-layouts-partials-templates-and-tag-helpers-with-no-locrazor-pages"></a><span data-ttu-id="6c550-570">Utilisation de dispositions, de partiels, de modèles et d’aide pour les balises avec des :::no-loc(Razor)::: pages</span><span class="sxs-lookup"><span data-stu-id="6c550-570">Using Layouts, partials, templates, and Tag Helpers with :::no-loc(Razor)::: Pages</span></span>

<span data-ttu-id="6c550-571">Les pages fonctionnent avec toutes les fonctionnalités du :::no-loc(Razor)::: moteur d’affichage.</span><span class="sxs-lookup"><span data-stu-id="6c550-571">Pages work with all the capabilities of the :::no-loc(Razor)::: view engine.</span></span> <span data-ttu-id="6c550-572">Les mises en page, les partiels, les modèles, les tag helpers, *_ViewStart. cshtml* , *_ViewImports. cshtml* fonctionnent de la même façon que pour les :::no-loc(Razor)::: vues conventionnelles.</span><span class="sxs-lookup"><span data-stu-id="6c550-572">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml* , *_ViewImports.cshtml* work in the same way they do for conventional :::no-loc(Razor)::: views.</span></span>

<span data-ttu-id="6c550-573">Nous allons nettoyer un peu cette page en tirant parti de certaines de ces fonctionnalités.</span><span class="sxs-lookup"><span data-stu-id="6c550-573">Let's declutter this page by taking advantage of some of those capabilities.</span></span>

<span data-ttu-id="6c550-574">Ajoutez une [page de disposition](xref:mvc/views/layout) à *Pages/Shared/_Layout.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="6c550-574">Add a [layout page](xref:mvc/views/layout) to *Pages/Shared/_Layout.cshtml* :</span></span>

[!code-cshtml[](index/sample/:::no-loc(Razor):::PagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="6c550-575">La [disposition](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="6c550-575">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="6c550-576">Contrôle la disposition de chaque page (à moins que la page ne refuse la disposition).</span><span class="sxs-lookup"><span data-stu-id="6c550-576">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="6c550-577">Importe des structures HTML telles que JavaScript et les feuilles de style.</span><span class="sxs-lookup"><span data-stu-id="6c550-577">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="6c550-578">Pour plus d’informations, consultez [Page de disposition](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="6c550-578">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="6c550-579">La propriété [Layout](xref:mvc/views/layout#specifying-a-layout) est définie dans *Pages/_ViewStart.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="6c550-579">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml* :</span></span>

[!code-cshtml[](index/sample/:::no-loc(Razor):::PagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="6c550-580">La disposition est dans le dossier *Pages/Shared*.</span><span class="sxs-lookup"><span data-stu-id="6c550-580">The layout is in the *Pages/Shared* folder.</span></span> <span data-ttu-id="6c550-581">Les pages recherchent d’autres vues (dispositions, modèles, partiels) hiérarchiquement, en commençant dans le même dossier que la page active.</span><span class="sxs-lookup"><span data-stu-id="6c550-581">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="6c550-582">Une mise en page dans le dossier *pages/Shared* peut être utilisée à partir de n’importe quelle :::no-loc(Razor)::: page sous le dossier *pages* .</span><span class="sxs-lookup"><span data-stu-id="6c550-582">A layout in the *Pages/Shared* folder can be used from any :::no-loc(Razor)::: page under the *Pages* folder.</span></span>

<span data-ttu-id="6c550-583">Le fichier de disposition doit être placé dans le dossier *Pages/Shared*.</span><span class="sxs-lookup"><span data-stu-id="6c550-583">The layout file should go in the *Pages/Shared* folder.</span></span>

<span data-ttu-id="6c550-584">Nous vous recommandons de **ne pas** placer le fichier de disposition dans le dossier *Views/Shared*.</span><span class="sxs-lookup"><span data-stu-id="6c550-584">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="6c550-585">*Views/Shared* est un modèle de vues MVC.</span><span class="sxs-lookup"><span data-stu-id="6c550-585">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="6c550-586">:::no-loc(Razor)::: Les pages sont conçues pour s’appuyer sur la hiérarchie des dossiers, et non sur les conventions de chemin.</span><span class="sxs-lookup"><span data-stu-id="6c550-586">:::no-loc(Razor)::: Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="6c550-587">L’affichage de la recherche à partir d’une :::no-loc(Razor)::: page comprend le dossier *pages* .</span><span class="sxs-lookup"><span data-stu-id="6c550-587">View search from a :::no-loc(Razor)::: Page includes the *Pages* folder.</span></span> <span data-ttu-id="6c550-588">Les mises en page, les modèles et les partiels que vous utilisez avec les contrôleurs MVC et les :::no-loc(Razor)::: vues conventionnelles *fonctionnent simplement*.</span><span class="sxs-lookup"><span data-stu-id="6c550-588">The layouts, templates, and partials you're using with MVC controllers and conventional :::no-loc(Razor)::: views *just work*.</span></span>

<span data-ttu-id="6c550-589">Ajoutez un fichier *Pages/_ViewImports.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="6c550-589">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/:::no-loc(Razor):::PagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="6c550-590">`@namespace` est expliqué plus loin dans le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="6c550-590">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="6c550-591">La directive `@addTagHelper` permet de bénéficier des [Tag Helpers intégrés](xref:mvc/views/tag-helpers/builtin-th/Index) dans toutes les pages du dossier *Pages*.</span><span class="sxs-lookup"><span data-stu-id="6c550-591">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="6c550-592">Quand la directive `@namespace` est utilisée explicitement sur une page :</span><span class="sxs-lookup"><span data-stu-id="6c550-592">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[](index/sample/:::no-loc(Razor):::PagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="6c550-593">La directive définit l’espace de noms pour la page.</span><span class="sxs-lookup"><span data-stu-id="6c550-593">The directive sets the namespace for the page.</span></span> <span data-ttu-id="6c550-594">La directive `@model` n’a pas besoin d’inclure l’espace de noms.</span><span class="sxs-lookup"><span data-stu-id="6c550-594">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="6c550-595">Quand la directive `@namespace` est contenue dans *_ViewImports.cshtml* , l’espace de noms spécifié fournit le préfixe de l’espace de noms généré dans la Page qui importe la directive `@namespace`.</span><span class="sxs-lookup"><span data-stu-id="6c550-595">When the `@namespace` directive is contained in *_ViewImports.cshtml* , the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="6c550-596">Le reste de l’espace de noms généré (la partie suffixe) est le chemin relatif séparé par un point entre le dossier contenant *_ViewImports.cshtml* et le dossier contenant la page.</span><span class="sxs-lookup"><span data-stu-id="6c550-596">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="6c550-597">Par exemple, la classe `PageModel`*Pages/Customers/Edit.cshtml.cs* définit explicitement l’espace de noms :</span><span class="sxs-lookup"><span data-stu-id="6c550-597">For example, the `PageModel` class *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-csharp[](index/sample/:::no-loc(Razor):::PagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="6c550-598">Le fichier *Pages/_ViewImports.cshtml* définit l’espace de noms suivant :</span><span class="sxs-lookup"><span data-stu-id="6c550-598">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/:::no-loc(Razor):::PagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="6c550-599">L’espace de noms généré pour la page *pages/Customers/Edit. cshtml* :::no-loc(Razor)::: est le même que la `PageModel` classe.</span><span class="sxs-lookup"><span data-stu-id="6c550-599">The generated namespace for the *Pages/Customers/Edit.cshtml* :::no-loc(Razor)::: Page is the same as the `PageModel` class.</span></span>

<span data-ttu-id="6c550-600">`@namespace`*fonctionne également avec les :::no-loc(Razor)::: vues conventionnelles.*</span><span class="sxs-lookup"><span data-stu-id="6c550-600">`@namespace` *also works with conventional :::no-loc(Razor)::: views.*</span></span>

<span data-ttu-id="6c550-601">Le fichier de vue *Pages/Create.cshtml* d’origine :</span><span class="sxs-lookup"><span data-stu-id="6c550-601">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/:::no-loc(Razor):::PagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="6c550-602">Le fichier vue *Pages/Create.cshtml* mis à jour :</span><span class="sxs-lookup"><span data-stu-id="6c550-602">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/:::no-loc(Razor):::PagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="6c550-603">Le [ :::no-loc(Razor)::: projet de démarrage pages](#rpvs17) contient les *pages/_ValidationScriptsPartial. cshtml* , qui raccorde la validation côté client.</span><span class="sxs-lookup"><span data-stu-id="6c550-603">The [:::no-loc(Razor)::: Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml* , which hooks up client-side validation.</span></span>

<span data-ttu-id="6c550-604">Pour plus d'informations sur les affichages partiels, consultez <xref:mvc/views/partial>.</span><span class="sxs-lookup"><span data-stu-id="6c550-604">For more information on partial views, see <xref:mvc/views/partial>.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="6c550-605">Génération d’URL pour les pages</span><span class="sxs-lookup"><span data-stu-id="6c550-605">URL generation for Pages</span></span>

<span data-ttu-id="6c550-606">La page `Create`, illustrée précédemment, utilise `RedirectToPage` :</span><span class="sxs-lookup"><span data-stu-id="6c550-606">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-csharp[](index/sample/:::no-loc(Razor):::PagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="6c550-607">L’application a la structure de fichiers/dossiers suivante :</span><span class="sxs-lookup"><span data-stu-id="6c550-607">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="6c550-608">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="6c550-608">*/Pages*</span></span>

  * <span data-ttu-id="6c550-609">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6c550-609">*Index.cshtml*</span></span>
  * <span data-ttu-id="6c550-610">*/Customers*</span><span class="sxs-lookup"><span data-stu-id="6c550-610">*/Customers*</span></span>

    * <span data-ttu-id="6c550-611">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6c550-611">*Create.cshtml*</span></span>
    * <span data-ttu-id="6c550-612">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6c550-612">*Edit.cshtml*</span></span>
    * <span data-ttu-id="6c550-613">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6c550-613">*Index.cshtml*</span></span>

<span data-ttu-id="6c550-614">Une fois l’opération réussie, les pages *Pages/Customers/Create.cshtml* et *Pages/Customers/Edit.cshtml* redirigent vers *Pages/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6c550-614">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="6c550-615">La chaîne `/Index` fait partie de l’URI pour accéder à la page précédente.</span><span class="sxs-lookup"><span data-stu-id="6c550-615">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="6c550-616">La chaîne `/Index` peut être utilisée pour générer l’URI de la page *Pages/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6c550-616">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="6c550-617">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="6c550-617">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="6c550-618">Le nom de la page est le chemin de la page à partir du dossier racine */Pages* avec un `/` devant (par exemple, `/Index`).</span><span class="sxs-lookup"><span data-stu-id="6c550-618">The page name is the path to the page from the root */Pages* folder including a leading `/` (for example, `/Index`).</span></span> <span data-ttu-id="6c550-619">Les exemples de génération d’URL précédents offrent des options améliorées et des capacités fonctionnelles sur le codage en dur d’une URL.</span><span class="sxs-lookup"><span data-stu-id="6c550-619">The preceding URL generation samples offer enhanced options and functional capabilities over hardcoding a URL.</span></span> <span data-ttu-id="6c550-620">La génération d’URL utilise le [routage](xref:mvc/controllers/routing) et peut générer et encoder des paramètres en fonction de la façon dont l’itinéraire est défini dans le chemin de destination.</span><span class="sxs-lookup"><span data-stu-id="6c550-620">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="6c550-621">La génération d’URL pour les pages prend en charge les noms relatifs.</span><span class="sxs-lookup"><span data-stu-id="6c550-621">URL generation for pages supports relative names.</span></span> <span data-ttu-id="6c550-622">Le tableau suivant montre la page Index sélectionnée avec différents paramètres `RedirectToPage` à partir de *Pages/Customers/Create.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="6c550-622">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml* :</span></span>

| <span data-ttu-id="6c550-623">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="6c550-623">RedirectToPage(x)</span></span>| <span data-ttu-id="6c550-624">Page</span><span class="sxs-lookup"><span data-stu-id="6c550-624">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="6c550-625">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="6c550-625">RedirectToPage("/Index")</span></span> | <span data-ttu-id="6c550-626">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="6c550-626">*Pages/Index*</span></span> |
| <span data-ttu-id="6c550-627">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="6c550-627">RedirectToPage("./Index");</span></span> | <span data-ttu-id="6c550-628">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="6c550-628">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="6c550-629">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="6c550-629">RedirectToPage("../Index")</span></span> | <span data-ttu-id="6c550-630">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="6c550-630">*Pages/Index*</span></span> |
| <span data-ttu-id="6c550-631">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="6c550-631">RedirectToPage("Index")</span></span>  | <span data-ttu-id="6c550-632">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="6c550-632">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="6c550-633">`RedirectToPage("Index")`, `RedirectToPage("./Index")` et `RedirectToPage("../Index")`  sont des *noms relatifs*.</span><span class="sxs-lookup"><span data-stu-id="6c550-633">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span></span> <span data-ttu-id="6c550-634">Le paramètre `RedirectToPage` est *combiné* avec le chemin de la page active pour calculer le nom de la page de destination.</span><span class="sxs-lookup"><span data-stu-id="6c550-634">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

<span data-ttu-id="6c550-635">La liaison de nom relatif est utile lors de la création de sites avec une structure complexe.</span><span class="sxs-lookup"><span data-stu-id="6c550-635">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="6c550-636">Si vous utilisez des noms relatifs pour établir une liaison entre les pages d’un dossier, vous pouvez renommer ce dossier.</span><span class="sxs-lookup"><span data-stu-id="6c550-636">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="6c550-637">Tous les liens fonctionneront encore (car ils n’incluent pas le nom du dossier).</span><span class="sxs-lookup"><span data-stu-id="6c550-637">All the links still work (because they didn't include the folder name).</span></span>

<span data-ttu-id="6c550-638">Pour rediriger vers une page située dans une autre [Zone](xref:mvc/controllers/areas), spécifiez la zone :</span><span class="sxs-lookup"><span data-stu-id="6c550-638">To redirect to a page in a different [Area](xref:mvc/controllers/areas), specify the area:</span></span>

```csharp
RedirectToPage("/Index", new { area = "Services" });
```

<span data-ttu-id="6c550-639">Pour plus d'informations, consultez <xref:mvc/controllers/areas>.</span><span class="sxs-lookup"><span data-stu-id="6c550-639">For more information, see <xref:mvc/controllers/areas>.</span></span>

## <a name="viewdata-attribute"></a><span data-ttu-id="6c550-640">Attribut ViewData</span><span class="sxs-lookup"><span data-stu-id="6c550-640">ViewData attribute</span></span>

<span data-ttu-id="6c550-641">Les données peuvent être passées à une page avec [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span><span class="sxs-lookup"><span data-stu-id="6c550-641">Data can be passed to a page with [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span></span> <span data-ttu-id="6c550-642">Les propriétés sur les contrôleurs ou :::no-loc(Razor)::: les modèles de page avec l' `[ViewData]` attribut ont leurs valeurs stockées et chargées à partir du [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span><span class="sxs-lookup"><span data-stu-id="6c550-642">Properties on controllers or :::no-loc(Razor)::: Page models with the `[ViewData]` attribute have their values stored and loaded from the [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span></span>

<span data-ttu-id="6c550-643">Dans l’exemple suivant, le `AboutModel` contient une `Title` propriété marquée avec `[ViewData]` .</span><span class="sxs-lookup"><span data-stu-id="6c550-643">In the following example, the `AboutModel` contains a `Title` property marked with `[ViewData]`.</span></span> <span data-ttu-id="6c550-644">La propriété `Title` a pour valeur le titre de la page À propos de :</span><span class="sxs-lookup"><span data-stu-id="6c550-644">The `Title` property is set to the title of the About page:</span></span>

```csharp
public class AboutModel : PageModel
{
    [ViewData]
    public string Title { get; } = "About";

    public void OnGet()
    {
    }
}
```

<span data-ttu-id="6c550-645">Dans la page À propos de, accédez à la propriété `Title` en tant que propriété de modèle :</span><span class="sxs-lookup"><span data-stu-id="6c550-645">In the About page, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="6c550-646">Dans la disposition, le titre est lu à partir du dictionnaire ViewData :</span><span class="sxs-lookup"><span data-stu-id="6c550-646">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

## <a name="tempdata"></a><span data-ttu-id="6c550-647">TempData</span><span class="sxs-lookup"><span data-stu-id="6c550-647">TempData</span></span>

<span data-ttu-id="6c550-648">ASP.NET Core expose la propriété [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) sur un [contrôleur](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="6c550-648">ASP.NET Core exposes the [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="6c550-649">Cette propriété stocke les données jusqu’à ce qu’elles soient lues.</span><span class="sxs-lookup"><span data-stu-id="6c550-649">This property stores data until it's read.</span></span> <span data-ttu-id="6c550-650">Vous pouvez utiliser les méthodes `Keep` et `Peek` pour examiner les données sans suppression.</span><span class="sxs-lookup"><span data-stu-id="6c550-650">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="6c550-651">`TempData` est utile pour la redirection, quand des données sont nécessaires pour plusieurs requêtes.</span><span class="sxs-lookup"><span data-stu-id="6c550-651">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="6c550-652">Le code suivant définit la valeur de `Message` à l’aide de `TempData` :</span><span class="sxs-lookup"><span data-stu-id="6c550-652">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-csharp[](index/sample/:::no-loc(Razor):::PagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="6c550-653">Le balisage suivant dans le fichier *Pages/Customers/Index.cshtml* affiche la valeur de `Message` à l’aide de `TempData`.</span><span class="sxs-lookup"><span data-stu-id="6c550-653">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="6c550-654">Le modèle de page *Pages/Customers/Index.cshtml.cs* applique l’attribut `[TempData]` à la propriété `Message`.</span><span class="sxs-lookup"><span data-stu-id="6c550-654">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```csharp
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="6c550-655">Pour plus d’informations, consultez [TempData](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="6c550-655">For more information, see [TempData](xref:fundamentals/app-state#tempdata) .</span></span>

<a name="mhpp"></a>

## <a name="multiple-handlers-per-page"></a><span data-ttu-id="6c550-656">Plusieurs gestionnaires par page</span><span class="sxs-lookup"><span data-stu-id="6c550-656">Multiple handlers per page</span></span>

<span data-ttu-id="6c550-657">La page suivante génère un balisage pour deux gestionnaires en utilisant le Tag Helper `asp-page-handler` :</span><span class="sxs-lookup"><span data-stu-id="6c550-657">The following page generates markup for two handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/:::no-loc(Razor):::PagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a :::no-loc(Razor)::: page, even when there's no `asp-` attribute   -->

<span data-ttu-id="6c550-658">Le formulaire dans l’exemple précédent contient deux boutons d’envoi, chacun utilisant `FormActionTagHelper` pour envoyer à une URL différente.</span><span class="sxs-lookup"><span data-stu-id="6c550-658">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="6c550-659">L’attribut `asp-page-handler` est un complément de `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="6c550-659">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="6c550-660">`asp-page-handler` génère des URL qui envoient à chacune des méthodes de gestionnaire définies par une page.</span><span class="sxs-lookup"><span data-stu-id="6c550-660">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="6c550-661">`asp-page` n’est pas spécifié, car l’exemple établit une liaison à la page active.</span><span class="sxs-lookup"><span data-stu-id="6c550-661">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="6c550-662">Le modèle de page :</span><span class="sxs-lookup"><span data-stu-id="6c550-662">The page model:</span></span>

[!code-csharp[](index/sample/:::no-loc(Razor):::PagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="6c550-663">Le code précédent utilise des *méthodes de gestionnaire nommées*.</span><span class="sxs-lookup"><span data-stu-id="6c550-663">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="6c550-664">Pour créer des méthodes de gestionnaire nommées, il faut prendre le texte dans le nom après `On<HTTP Verb>` et avant `Async` (le cas échéant).</span><span class="sxs-lookup"><span data-stu-id="6c550-664">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="6c550-665">Dans l’exemple précédent, les méthodes de page sont OnPost **JoinList** Async et OnPost **JoinListUC** Async.</span><span class="sxs-lookup"><span data-stu-id="6c550-665">In the preceding example, the page methods are OnPost **JoinList** Async and OnPost **JoinListUC** Async.</span></span> <span data-ttu-id="6c550-666">Avec *OnPost* et *Async* supprimés, les noms des gestionnaires sont `JoinList` et `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="6c550-666">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/:::no-loc(Razor):::PagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="6c550-667">Avec le code précédent, le chemin d’URL qui envoie à `OnPostJoinListAsync` est `https://localhost:5001/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="6c550-667">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="6c550-668">Le chemin d’URL qui envoie à `OnPostJoinListUCAsync` est `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="6c550-668">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="custom-routes"></a><span data-ttu-id="6c550-669">Routes personnalisées</span><span class="sxs-lookup"><span data-stu-id="6c550-669">Custom routes</span></span>

<span data-ttu-id="6c550-670">Utilisez la directive `@page` pour :</span><span class="sxs-lookup"><span data-stu-id="6c550-670">Use the `@page` directive to:</span></span>

* <span data-ttu-id="6c550-671">Spécifier une route personnalisée vers une page.</span><span class="sxs-lookup"><span data-stu-id="6c550-671">Specify a custom route to a page.</span></span> <span data-ttu-id="6c550-672">Par exemple, la route vers la page À propos peut être définie sur `/Some/Other/Path` avec `@page "/Some/Other/Path"`.</span><span class="sxs-lookup"><span data-stu-id="6c550-672">For example, the route to the About page can be set to `/Some/Other/Path` with `@page "/Some/Other/Path"`.</span></span>
* <span data-ttu-id="6c550-673">Ajouter des segments à la route par défaut d’une page.</span><span class="sxs-lookup"><span data-stu-id="6c550-673">Append segments to a page's default route.</span></span> <span data-ttu-id="6c550-674">Par exemple, un segment « item » peut être ajouté à la route par défaut d’une page avec `@page "item"`.</span><span class="sxs-lookup"><span data-stu-id="6c550-674">For example, an "item" segment can be added to a page's default route with `@page "item"`.</span></span>
* <span data-ttu-id="6c550-675">Ajouter des paramètres à la route par défaut d’une page.</span><span class="sxs-lookup"><span data-stu-id="6c550-675">Append parameters to a page's default route.</span></span> <span data-ttu-id="6c550-676">Par exemple, un paramètre d’ID, `id`, peut être nécessaire pour une page avec `@page "{id}"`.</span><span class="sxs-lookup"><span data-stu-id="6c550-676">For example, an ID parameter, `id`, can be required for a page with `@page "{id}"`.</span></span>

<span data-ttu-id="6c550-677">Un chemin relatif racine désigné par un tilde (`~`) au début du chemin est pris en charge.</span><span class="sxs-lookup"><span data-stu-id="6c550-677">A root-relative path designated by a tilde (`~`) at the beginning of the path is supported.</span></span> <span data-ttu-id="6c550-678">Par exemple, `@page "~/Some/Other/Path"` est identique à `@page "/Some/Other/Path"`.</span><span class="sxs-lookup"><span data-stu-id="6c550-678">For example, `@page "~/Some/Other/Path"` is the same as `@page "/Some/Other/Path"`.</span></span>

<span data-ttu-id="6c550-679">Si vous n’aimez pas la chaîne `?handler=JoinList` de requête dans l’URL, modifiez l’itinéraire pour placer le nom du gestionnaire dans la partie chemin d’accès de l’URL.</span><span class="sxs-lookup"><span data-stu-id="6c550-679">If you don't like the query string `?handler=JoinList` in the URL, change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="6c550-680">L’itinéraire peut être personnalisé en ajoutant un modèle de routage entre guillemets doubles après la `@page` directive.</span><span class="sxs-lookup"><span data-stu-id="6c550-680">The route can be customized by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/:::no-loc(Razor):::PagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="6c550-681">Avec le code précédent, le chemin d’URL qui envoie à `OnPostJoinListAsync` est `https://localhost:5001/Customers/CreateFATH/JoinList`.</span><span class="sxs-lookup"><span data-stu-id="6c550-681">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH/JoinList`.</span></span> <span data-ttu-id="6c550-682">Le chemin d’URL qui envoie à `OnPostJoinListUCAsync` est `https://localhost:5001/Customers/CreateFATH/JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="6c550-682">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH/JoinListUC`.</span></span>

<span data-ttu-id="6c550-683">Le `?` suivant `handler` signifie que le paramètre d’itinéraire est facultatif.</span><span class="sxs-lookup"><span data-stu-id="6c550-683">The `?` following `handler` means the route parameter is optional.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="6c550-684">Configuration et paramètres</span><span class="sxs-lookup"><span data-stu-id="6c550-684">Configuration and settings</span></span>

<span data-ttu-id="6c550-685">Pour configurer les options avancées, utilisez la méthode d’extension `Add:::no-loc(Razor):::PagesOptions` sur le générateur MVC :</span><span class="sxs-lookup"><span data-stu-id="6c550-685">To configure advanced options, use the extension method `Add:::no-loc(Razor):::PagesOptions` on the MVC builder:</span></span>

[!code-csharp[](index/sample/:::no-loc(Razor):::PagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="6c550-686">Actuellement, vous pouvez utiliser `:::no-loc(Razor):::PagesOptions` pour définir le répertoire racine pour les pages, ou ajouter des conventions de modèle d’application pour les pages.</span><span class="sxs-lookup"><span data-stu-id="6c550-686">Currently you can use the `:::no-loc(Razor):::PagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="6c550-687">Nous permettrons à l’avenir une plus grande extensibilité en ce sens.</span><span class="sxs-lookup"><span data-stu-id="6c550-687">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="6c550-688">Pour précompiler des vues, consultez [ :::no-loc(Razor)::: afficher la compilation](xref:mvc/views/view-compilation) .</span><span class="sxs-lookup"><span data-stu-id="6c550-688">To precompile views, see [:::no-loc(Razor)::: view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="6c550-689">[Téléchargez ou affichez des exemples de code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="6c550-689">[Download or view sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/sample).</span></span>

<span data-ttu-id="6c550-690">Consultez [prise en main des :::no-loc(Razor)::: pages](xref:tutorials/razor-pages/razor-pages-start), qui s’appuie sur cette introduction.</span><span class="sxs-lookup"><span data-stu-id="6c550-690">See [Get started with :::no-loc(Razor)::: Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-no-locrazor-pages-are-at-the-content-root"></a><span data-ttu-id="6c550-691">Spécifier que :::no-loc(Razor)::: les pages se trouvent à la racine du contenu</span><span class="sxs-lookup"><span data-stu-id="6c550-691">Specify that :::no-loc(Razor)::: Pages are at the content root</span></span>

<span data-ttu-id="6c550-692">Par défaut, :::no-loc(Razor)::: les pages sont enracinées dans le répertoire */pages*</span><span class="sxs-lookup"><span data-stu-id="6c550-692">By default, :::no-loc(Razor)::: Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="6c550-693">Ajoutez [avec :::no-loc(Razor)::: PagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) à [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) pour spécifier que vos :::no-loc(Razor)::: pages se trouvent à la [racine du contenu](xref:fundamentals/index#content-root) ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) de l’application :</span><span class="sxs-lookup"><span data-stu-id="6c550-693">Add [With:::no-loc(Razor):::PagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your :::no-loc(Razor)::: Pages are at the [content root](xref:fundamentals/index#content-root) ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .Add:::no-loc(Razor):::PagesOptions(options =>
    {
        ...
    })
    .With:::no-loc(Razor):::PagesAtContentRoot();
```

### <a name="specify-that-no-locrazor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="6c550-694">Spécifier que :::no-loc(Razor)::: les pages se trouvent dans un répertoire racine personnalisé</span><span class="sxs-lookup"><span data-stu-id="6c550-694">Specify that :::no-loc(Razor)::: Pages are at a custom root directory</span></span>

<span data-ttu-id="6c550-695">Ajoutez [avec :::no-loc(Razor)::: PagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) à [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) pour spécifier que vos :::no-loc(Razor)::: pages se trouvent dans un répertoire racine personnalisé dans l’application (fournissez un chemin d’accès relatif) :</span><span class="sxs-lookup"><span data-stu-id="6c550-695">Add [With:::no-loc(Razor):::PagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your :::no-loc(Razor)::: Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .Add:::no-loc(Razor):::PagesOptions(options =>
    {
        ...
    })
    .With:::no-loc(Razor):::PagesRoot("/path/to/razor/pages");
```

## <a name="additional-resources"></a><span data-ttu-id="6c550-696">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="6c550-696">Additional resources</span></span>

* [<span data-ttu-id="6c550-697">Autoriser l’attribut et les :::no-loc(Razor)::: pages</span><span class="sxs-lookup"><span data-stu-id="6c550-697">Authorize attribute and :::no-loc(Razor)::: Pages</span></span>](xref:security/authorization/simple#aarp)
* <xref:index>
* <xref:mvc/views/razor>
* <xref:mvc/controllers/areas>
* <xref:tutorials/razor-pages/razor-pages-start>
* <xref:security/authorization/razor-pages-authorization>
* <xref:razor-pages/razor-pages-conventions>
* <xref:test/razor-pages-tests>
* <xref:mvc/views/partial>

::: moniker-end
