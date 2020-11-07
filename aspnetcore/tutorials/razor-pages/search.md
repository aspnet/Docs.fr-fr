---
title: Partie 6, ajouter une recherche
author: rick-anderson
description: 'Partie 6 de la série de didacticiels sur les :::no-loc(Razor)::: pages.'
ms.author: riande
ms.date: 12/05/2019
no-loc:
- ':::no-loc(Index):::'
- ':::no-loc(Create):::'
- ':::no-loc(Delete):::'
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
uid: tutorials/razor-pages/search
ms.openlocfilehash: 00c1be2704d92c7d4f868e6eaa346bd8e9901dbf
ms.sourcegitcommit: 342588e10ae0054a6d6dc0fd11dae481006be099
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/07/2020
ms.locfileid: "94360837"
---
# <a name="part-6-add-search-to-aspnet-core-no-locrazor-pages"></a><span data-ttu-id="8c3ac-103">Partie 6, ajouter une recherche aux :::no-loc(Razor)::: Pages ASP.net Core</span><span class="sxs-lookup"><span data-stu-id="8c3ac-103">Part 6, add search to ASP.NET Core :::no-loc(Razor)::: Pages</span></span>

<span data-ttu-id="8c3ac-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8c3ac-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-5.0"

<span data-ttu-id="8c3ac-105">[Affichez ou téléchargez un exemple de code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/:::no-loc(Razor):::PagesMovie50) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="8c3ac-105">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/:::no-loc(Razor):::PagesMovie50) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-5.0 >= aspnetcore-3.0"

<span data-ttu-id="8c3ac-106">[Affichez ou téléchargez un exemple de code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/:::no-loc(Razor):::PagesMovie30) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="8c3ac-106">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/:::no-loc(Razor):::PagesMovie30) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="8c3ac-107">Dans les sections suivantes, la recherche de films par *genre* ou par *nom* est ajoutée.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-107">In the following sections, searching movies by *genre* or *name* is added.</span></span>

<span data-ttu-id="8c3ac-108">Ajoutez l’instruction et les propriétés en surbrillance suivantes à *pages/movies/ :::no-loc(Index)::: . cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="8c3ac-108">Add the following highlighted using statement and properties to *Pages/Movies/:::no-loc(Index):::.cshtml.cs* :</span></span>

[!code-csharp[](razor-pages-start/sample/:::no-loc(Razor):::PagesMovie30/Pages/Movies/:::no-loc(Index):::.cshtml.cs?name=snippet_newProps&highlight=3,23,24,25,26,27)]

<span data-ttu-id="8c3ac-109">Dans le code précédent :</span><span class="sxs-lookup"><span data-stu-id="8c3ac-109">In the previous code:</span></span>

* <span data-ttu-id="8c3ac-110">`SearchString`: Contient le texte que les utilisateurs entrent dans la zone de texte Rechercher.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-110">`SearchString`: Contains the text users enter in the search text box.</span></span> <span data-ttu-id="8c3ac-111">`SearchString` a l' [`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) attribut.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-111">`SearchString` has the [`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) attribute.</span></span> <span data-ttu-id="8c3ac-112">`[BindProperty]` lie les valeurs de formulaire et les chaînes de requête avec le même nom que la propriété.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-112">`[BindProperty]` binds form values and query strings with the same name as the property.</span></span> <span data-ttu-id="8c3ac-113">`[BindProperty(SupportsGet = true)]` est requis pour la liaison sur les requêtes HTTP d’extraction.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-113">`[BindProperty(SupportsGet = true)]` is required for binding on HTTP GET requests.</span></span>
* <span data-ttu-id="8c3ac-114">`Genres`: Contient la liste des genres.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-114">`Genres`: Contains the list of genres.</span></span> <span data-ttu-id="8c3ac-115">`Genres` permet à l’utilisateur de sélectionner un genre dans la liste.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-115">`Genres` allows the user to select a genre from the list.</span></span> <span data-ttu-id="8c3ac-116">`SelectList` nécessite `using Microsoft.AspNetCore.Mvc.Rendering;`</span><span class="sxs-lookup"><span data-stu-id="8c3ac-116">`SelectList` requires `using Microsoft.AspNetCore.Mvc.Rendering;`</span></span>
* <span data-ttu-id="8c3ac-117">`MovieGenre`: Contient le genre spécifique que l’utilisateur sélectionne.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-117">`MovieGenre`: Contains the specific genre the user selects.</span></span> <span data-ttu-id="8c3ac-118">Par exemple, « Ouest ».</span><span class="sxs-lookup"><span data-stu-id="8c3ac-118">For example, "Western".</span></span>
* <span data-ttu-id="8c3ac-119">`Genres` et `MovieGenre` sont utilisés plus loin dans ce tutoriel.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-119">`Genres` and `MovieGenre` are used later in this tutorial.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="8c3ac-120">Mettez à jour la :::no-loc(Index)::: méthode de la page `OnGetAsync` avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="8c3ac-120">Update the :::no-loc(Index)::: page's `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/:::no-loc(Razor):::PagesMovie30/Pages/Movies/:::no-loc(Index):::.cshtml.cs?name=snippet_1stSearch)]

<span data-ttu-id="8c3ac-121">La première ligne de la méthode `OnGetAsync` crée une requête [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) pour sélectionner les films :</span><span class="sxs-lookup"><span data-stu-id="8c3ac-121">The first line of the `OnGetAsync` method creates a [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) query to select the movies:</span></span>

```csharp
// using System.Linq;
var movies = from m in _context.Movie
             select m;
```

<span data-ttu-id="8c3ac-122">La requête est *seulement* définie à ce stade, elle n’a **pas** été exécutée sur la base de données.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-122">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="8c3ac-123">Si la propriété `SearchString` n’est pas nulle ou vide, la requête sur les films est modifiée de façon à filtrer sur la chaîne de recherche :</span><span class="sxs-lookup"><span data-stu-id="8c3ac-123">If the `SearchString` property is not null or empty, the movies query is modified to filter on the search string:</span></span>

[!code-csharp[](razor-pages-start/sample/:::no-loc(Razor):::PagesMovie30/Pages/Movies/:::no-loc(Index):::.cshtml.cs?name=snippet_SearchNull)]

<span data-ttu-id="8c3ac-124">Le code `s => s.Title.Contains()` est une [expression lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="8c3ac-124">The `s => s.Title.Contains()` code is a [Lambda Expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="8c3ac-125">Les expressions lambda sont utilisées dans les requêtes [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) basées sur une méthode en tant qu’arguments pour les méthodes d’opérateur de requête standard, telles que la méthode [Where](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) ou `Contains` .</span><span class="sxs-lookup"><span data-stu-id="8c3ac-125">Lambdas are used in method-based [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) queries as arguments to standard query operator methods such as the [Where](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) method or `Contains`.</span></span> <span data-ttu-id="8c3ac-126">Les requêtes LINQ ne sont pas exécutées quand elles sont définies ou quand elles sont modifiées en appelant une méthode, telle que `Where` , `Contains` ou `OrderBy` .</span><span class="sxs-lookup"><span data-stu-id="8c3ac-126">LINQ queries are not executed when they're defined or when they're modified by calling a method, such as `Where`, `Contains`, or `OrderBy`.</span></span> <span data-ttu-id="8c3ac-127">Au lieu de cela, l’exécution de la requête est différée.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-127">Rather, query execution is deferred.</span></span> <span data-ttu-id="8c3ac-128">L’évaluation d’une expression est retardée jusqu’à ce que sa valeur réalisée soit itérée ou que la `ToListAsync` méthode soit appelée.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-128">The evaluation of an expression is delayed until its realized value is iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="8c3ac-129">Pour plus d’informations, consultez [Exécution de requête](/dotnet/framework/data/adonet/ef/language-reference/query-execution).</span><span class="sxs-lookup"><span data-stu-id="8c3ac-129">See [Query Execution](/dotnet/framework/data/adonet/ef/language-reference/query-execution) for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="8c3ac-130">La méthode [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) est exécutée sur la base de données, et non pas dans le code C#.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-130">The [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the C# code.</span></span> <span data-ttu-id="8c3ac-131">Le respect de la casse pour la requête dépend de la base de données et du classement.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-131">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="8c3ac-132">Sur SQL Server, `Contains` est mappée à [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), qui ne respecte pas la casse.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-132">On SQL Server, `Contains` maps to [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="8c3ac-133">Dans SQLite, avec le classement par défaut, elle respecte la casse.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-133">In SQLite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="8c3ac-134">Accédez à la page films et ajoutez une chaîne de requête telle que `?searchString=Ghost` à l’URL.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-134">Navigate to the Movies page and append a query string such as `?searchString=Ghost` to the URL.</span></span> <span data-ttu-id="8c3ac-135">Par exemple, `https://localhost:5001/Movies?searchString=Ghost`.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-135">For example, `https://localhost:5001/Movies?searchString=Ghost`.</span></span> <span data-ttu-id="8c3ac-136">Les films filtrés sont affichés.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-136">The filtered movies are displayed.</span></span>

![::: No-Loc (index) ::: View](search/_static/ghost.png)

<span data-ttu-id="8c3ac-138">Si le modèle de routage suivant est ajouté à la :::no-loc(Index)::: page, la chaîne de recherche peut être transmise en tant que segment d’URL.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-138">If the following route template is added to the :::no-loc(Index)::: page, the search string can be passed as a URL segment.</span></span> <span data-ttu-id="8c3ac-139">Par exemple, `https://localhost:5001/Movies/Ghost`.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-139">For example, `https://localhost:5001/Movies/Ghost`.</span></span>

```cshtml
@page "{searchString?}"
```

<span data-ttu-id="8c3ac-140">La contrainte d’itinéraire précédente permet de rechercher le titre comme données d’itinéraire (un segment d’URL) et non comme valeur de chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-140">The preceding route constraint allows searching the title as route data (a URL segment) instead of as a query string value.</span></span>  <span data-ttu-id="8c3ac-141">Le `?` dans `"{searchString?}"` signifie qu’il s’agit d’un paramètre d’itinéraire facultatif.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-141">The `?` in `"{searchString?}"` means this is an optional route parameter.</span></span>

![::: No-Loc (index) ::: View avec le mot fantôme ajouté à l’URL et une liste de films retournée de deux films, Ghostbusters et Ghostbusters 2](search/_static/g2.png)

<span data-ttu-id="8c3ac-143">Le runtime ASP.NET Core utilise la [liaison de modèle](xref:mvc/models/model-binding) pour définir la valeur de la propriété `SearchString` à partir de la chaîne de requête (`?searchString=Ghost`) ou des données de la route (`https://localhost:5001/Movies/Ghost`).</span><span class="sxs-lookup"><span data-stu-id="8c3ac-143">The ASP.NET Core runtime uses [model binding](xref:mvc/models/model-binding) to set the value of the `SearchString` property from the query string (`?searchString=Ghost`) or route data (`https://localhost:5001/Movies/Ghost`).</span></span> <span data-ttu-id="8c3ac-144">La liaison de modèle est *_sans_* respect de la casse.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-144">Model binding is \* *_not_* _ case sensitive.</span></span>

<span data-ttu-id="8c3ac-145">Toutefois, les utilisateurs ne sont pas censés modifier l’URL pour rechercher un film.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-145">However, users cannot be expected to modify the URL to search for a movie.</span></span> <span data-ttu-id="8c3ac-146">Dans cette étape, une interface utilisateur est ajoutée pour filtrer les films.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-146">In this step, UI is added to filter movies.</span></span> <span data-ttu-id="8c3ac-147">Si vous avez ajouté la contrainte d’itinéraire `"{searchString?}"`, supprimez-la.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-147">If you added the route constraint `"{searchString?}"`, remove it.</span></span>

<span data-ttu-id="8c3ac-148">Ouvrez le fichier _Pages/movies/ :::no-loc(Index)::: . cshtml \* et ajoutez le balisage mis en surbrillance dans le code suivant :</span><span class="sxs-lookup"><span data-stu-id="8c3ac-148">Open the _Pages/Movies/:::no-loc(Index):::.cshtml\* file, and add the markup highlighted in the following code:</span></span>

[!code-cshtml[](razor-pages-start/sample/:::no-loc(Razor):::PagesMovie30/SnapShots/:::no-loc(Index):::2.cshtml?highlight=14-19&range=1-22)]

<span data-ttu-id="8c3ac-149">La balise HTML `<form>` utilise les [Tag Helpers](xref:mvc/views/tag-helpers/intro) suivants :</span><span class="sxs-lookup"><span data-stu-id="8c3ac-149">The HTML `<form>` tag uses the following [Tag Helpers](xref:mvc/views/tag-helpers/intro):</span></span>

* <span data-ttu-id="8c3ac-150">[Tag Helper Form](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="8c3ac-150">[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="8c3ac-151">Lorsque le formulaire est envoyé, la chaîne de filtrage est envoyée à la page *pages/ :::no-loc(Index)::: movies/* page via la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-151">When the form is submitted, the filter string is sent to the *Pages/Movies/:::no-loc(Index):::* page via query string.</span></span>
* [<span data-ttu-id="8c3ac-152">Tag Helper Input</span><span class="sxs-lookup"><span data-stu-id="8c3ac-152">Input Tag Helper</span></span>](xref:mvc/views/working-with-forms#the-input-tag-helper)

<span data-ttu-id="8c3ac-153">Enregistrez les modifications apportées, puis testez le filtre.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-153">Save the changes and test the filter.</span></span>

![::: No-Loc (index) ::: View avec le mot fantôme tapé dans la zone de texte filtre de titre](search/_static/filter.png)

## <a name="search-by-genre"></a><span data-ttu-id="8c3ac-155">Rechercher par genre</span><span class="sxs-lookup"><span data-stu-id="8c3ac-155">Search by genre</span></span>

<span data-ttu-id="8c3ac-156">Mettez à jour la :::no-loc(Index)::: méthode de la page `OnGetAsync` avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="8c3ac-156">Update the :::no-loc(Index)::: page's `OnGetAsync` method with the following code:</span></span>

   [!code-csharp[](razor-pages-start/sample/:::no-loc(Razor):::PagesMovie30/Pages/Movies/:::no-loc(Index):::.cshtml.cs?name=snippet_SearchGenre)]

<span data-ttu-id="8c3ac-157">Le code suivant est une requête LINQ qui récupère tous les genres dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-157">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[](razor-pages-start/sample/:::no-loc(Razor):::PagesMovie30/Pages/Movies/:::no-loc(Index):::.cshtml.cs?name=snippet_LINQ)]

<span data-ttu-id="8c3ac-158">La liste `SelectList` de genres est créée en projetant des différents genres.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-158">The `SelectList` of genres is created by projecting the distinct genres.</span></span>

[!code-csharp[](razor-pages-start/sample/:::no-loc(Razor):::PagesMovie30/Pages/Movies/:::no-loc(Index):::.cshtml.cs?name=snippet_SelectList)]

### <a name="add-search-by-genre-to-the-no-locrazor-page"></a><span data-ttu-id="8c3ac-159">Ajouter la recherche par genre à la :::no-loc(Razor)::: page</span><span class="sxs-lookup"><span data-stu-id="8c3ac-159">Add search by genre to the :::no-loc(Razor)::: Page</span></span>

1. <span data-ttu-id="8c3ac-160">Mettez à jour le *:::no-loc(Index)::: . cshtml* [ `<form>` élément] ( https://developer.mozilla.org/docs/Web/HTML/Element/form) mis en surbrillance dans le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="8c3ac-160">Update the *:::no-loc(Index):::.cshtml* [`<form>` element] (https://developer.mozilla.org/docs/Web/HTML/Element/form) as highlighted in the following markup:</span></span>

   [!code-cshtml[](razor-pages-start/sample/:::no-loc(Razor):::PagesMovie30/SnapShots/:::no-loc(Index):::FormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

1. <span data-ttu-id="8c3ac-161">Testez l’application en effectuant une recherche par genre, par titre de film et selon ces deux critères.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-161">Test the app by searching by genre, by movie title, and by both.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8c3ac-162">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="8c3ac-162">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8c3ac-163">[Précédent : mettre à jour les pages](xref:tutorials/razor-pages/da1) 
>  [Suivant : ajouter un nouveau champ](xref:tutorials/razor-pages/new-field)</span><span class="sxs-lookup"><span data-stu-id="8c3ac-163">[Previous: Update the pages](xref:tutorials/razor-pages/da1)
[Next: Add a new field](xref:tutorials/razor-pages/new-field)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="8c3ac-164">[Affichez ou téléchargez un exemple de code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="8c3ac-164">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="8c3ac-165">Dans les sections suivantes, la recherche de films par *genre* ou par *nom* est ajoutée.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-165">In the following sections, searching movies by *genre* or *name* is added.</span></span>

<span data-ttu-id="8c3ac-166">Ajoutez les propriétés en surbrillance suivantes à *pages/movies/ :::no-loc(Index)::: . cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="8c3ac-166">Add the following highlighted properties to *Pages/Movies/:::no-loc(Index):::.cshtml.cs* :</span></span>

[!code-csharp[](razor-pages-start/sample/:::no-loc(Razor):::PagesMovie22/Pages/Movies/:::no-loc(Index):::.cshtml.cs?name=snippet_newProps&highlight=11-999)]

* <span data-ttu-id="8c3ac-167">`SearchString`: Contient le texte que les utilisateurs entrent dans la zone de texte Rechercher.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-167">`SearchString`: Contains the text users enter in the search text box.</span></span> <span data-ttu-id="8c3ac-168">`SearchString` a l' [`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) attribut.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-168">`SearchString` has the [`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) attribute.</span></span> <span data-ttu-id="8c3ac-169">`[BindProperty]` lie les valeurs de formulaire et les chaînes de requête avec le même nom que la propriété.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-169">`[BindProperty]` binds form values and query strings with the same name as the property.</span></span> <span data-ttu-id="8c3ac-170">`[BindProperty(SupportsGet = true)]` est requis pour la liaison sur les requêtes HTTP d’extraction.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-170">`[BindProperty(SupportsGet = true)]` is required for binding on HTTP GET requests.</span></span>
* <span data-ttu-id="8c3ac-171">`Genres`: Contient la liste des genres.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-171">`Genres`: Contains the list of genres.</span></span> <span data-ttu-id="8c3ac-172">`Genres` permet à l’utilisateur de sélectionner un genre dans la liste.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-172">`Genres` allows the user to select a genre from the list.</span></span> <span data-ttu-id="8c3ac-173">`SelectList` nécessite `using Microsoft.AspNetCore.Mvc.Rendering;`</span><span class="sxs-lookup"><span data-stu-id="8c3ac-173">`SelectList` requires `using Microsoft.AspNetCore.Mvc.Rendering;`</span></span>
* <span data-ttu-id="8c3ac-174">`MovieGenre`: Contient le genre spécifique que l’utilisateur sélectionne.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-174">`MovieGenre`: Contains the specific genre the user selects.</span></span> <span data-ttu-id="8c3ac-175">Par exemple, « Ouest ».</span><span class="sxs-lookup"><span data-stu-id="8c3ac-175">For example, "Western".</span></span>
* <span data-ttu-id="8c3ac-176">`Genres` et `MovieGenre` sont utilisés plus loin dans ce tutoriel.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-176">`Genres` and `MovieGenre` are used later in this tutorial.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="8c3ac-177">Mettez à jour la :::no-loc(Index)::: méthode de la page `OnGetAsync` avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="8c3ac-177">Update the :::no-loc(Index)::: page's `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/:::no-loc(Razor):::PagesMovie22/Pages/Movies/:::no-loc(Index):::.cshtml.cs?name=snippet_1stSearch)]

<span data-ttu-id="8c3ac-178">La première ligne de la méthode `OnGetAsync` crée une requête [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) pour sélectionner les films :</span><span class="sxs-lookup"><span data-stu-id="8c3ac-178">The first line of the `OnGetAsync` method creates a [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) query to select the movies:</span></span>

```csharp
// using System.Linq;
var movies = from m in _context.Movie
             select m;
```

<span data-ttu-id="8c3ac-179">La requête est *seulement* définie à ce stade, elle n’a **pas** été exécutée sur la base de données.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-179">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="8c3ac-180">Si la propriété `SearchString` n’est pas nulle ou vide, la requête sur les films est modifiée de façon à filtrer sur la chaîne de recherche :</span><span class="sxs-lookup"><span data-stu-id="8c3ac-180">If the `SearchString` property is not null or empty, the movies query is modified to filter on the search string:</span></span>

[!code-csharp[](razor-pages-start/sample/:::no-loc(Razor):::PagesMovie22/Pages/Movies/:::no-loc(Index):::.cshtml.cs?name=snippet_SearchNull)]

<span data-ttu-id="8c3ac-181">Le code `s => s.Title.Contains()` est une [expression lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="8c3ac-181">The `s => s.Title.Contains()` code is a [Lambda Expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="8c3ac-182">Les expressions lambda sont utilisées dans les requêtes [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) basées sur une méthode en tant qu’arguments pour les méthodes d’opérateur de requête standard, telles que la méthode [Where](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) ou `Contains` .</span><span class="sxs-lookup"><span data-stu-id="8c3ac-182">Lambdas are used in method-based [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) queries as arguments to standard query operator methods such as the [Where](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) method or `Contains`.</span></span> <span data-ttu-id="8c3ac-183">Les requêtes LINQ ne sont pas exécutées quand elles sont définies ou quand elles sont modifiées en appelant une méthode, telle que `Where` , `Contains`  ou `OrderBy` .</span><span class="sxs-lookup"><span data-stu-id="8c3ac-183">LINQ queries are not executed when they're defined or when they're modified by calling a method, such as `Where`, `Contains`  or `OrderBy`.</span></span> <span data-ttu-id="8c3ac-184">Au lieu de cela, l’exécution de la requête est différée.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-184">Rather, query execution is deferred.</span></span> <span data-ttu-id="8c3ac-185">L’évaluation d’une expression est retardée jusqu’à ce que sa valeur réalisée soit itérée ou que la `ToListAsync` méthode soit appelée.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-185">The evaluation of an expression is delayed until its realized value is iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="8c3ac-186">Pour plus d’informations, consultez [Exécution de requête](/dotnet/framework/data/adonet/ef/language-reference/query-execution).</span><span class="sxs-lookup"><span data-stu-id="8c3ac-186">See [Query Execution](/dotnet/framework/data/adonet/ef/language-reference/query-execution) for more information.</span></span>

<span data-ttu-id="8c3ac-187">**Remarque :** La méthode [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) est exécutée sur la base de données, et non pas dans le code C#.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-187">**Note:** The [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the C# code.</span></span> <span data-ttu-id="8c3ac-188">Le respect de la casse pour la requête dépend de la base de données et du classement.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-188">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="8c3ac-189">Sur SQL Server, `Contains` est mappée à [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), qui ne respecte pas la casse.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-189">On SQL Server, `Contains` maps to [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="8c3ac-190">Dans SQLite, avec le classement par défaut, elle respecte la casse.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-190">In SQLite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="8c3ac-191">Accédez à la page films et ajoutez une chaîne de requête telle que `?searchString=Ghost` à l’URL.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-191">Navigate to the Movies page and append a query string such as `?searchString=Ghost` to the URL.</span></span> <span data-ttu-id="8c3ac-192">Par exemple, `https://localhost:5001/Movies?searchString=Ghost`.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-192">For example, `https://localhost:5001/Movies?searchString=Ghost`.</span></span> <span data-ttu-id="8c3ac-193">Les films filtrés sont affichés.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-193">The filtered movies are displayed.</span></span>

![::: No-Loc (index) ::: View](search/_static/ghost.png)

<span data-ttu-id="8c3ac-195">Si le modèle de routage suivant est ajouté à la :::no-loc(Index)::: page, la chaîne de recherche peut être transmise en tant que segment d’URL.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-195">If the following route template is added to the :::no-loc(Index)::: page, the search string can be passed as a URL segment.</span></span> <span data-ttu-id="8c3ac-196">Par exemple, `https://localhost:5001/Movies/Ghost`.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-196">For example, `https://localhost:5001/Movies/Ghost`.</span></span>

```cshtml
@page "{searchString?}"
```

<span data-ttu-id="8c3ac-197">La contrainte d’itinéraire précédente permet de rechercher le titre comme données d’itinéraire (un segment d’URL) et non comme valeur de chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-197">The preceding route constraint allows searching the title as route data (a URL segment) instead of as a query string value.</span></span>  <span data-ttu-id="8c3ac-198">Le `?` dans `"{searchString?}"` signifie qu’il s’agit d’un paramètre d’itinéraire facultatif.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-198">The `?` in `"{searchString?}"` means this is an optional route parameter.</span></span>

![::: No-Loc (index) ::: View avec le mot fantôme ajouté à l’URL et une liste de films retournée de deux films, Ghostbusters et Ghostbusters 2](search/_static/g2.png)

<span data-ttu-id="8c3ac-200">Le runtime ASP.NET Core utilise la [liaison de modèle](xref:mvc/models/model-binding) pour définir la valeur de la propriété `SearchString` à partir de la chaîne de requête (`?searchString=Ghost`) ou des données de la route (`https://localhost:5001/Movies/Ghost`).</span><span class="sxs-lookup"><span data-stu-id="8c3ac-200">The ASP.NET Core runtime uses [model binding](xref:mvc/models/model-binding) to set the value of the `SearchString` property from the query string (`?searchString=Ghost`) or route data (`https://localhost:5001/Movies/Ghost`).</span></span> <span data-ttu-id="8c3ac-201">La liaison de modèle est *_sans_* respect de la casse.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-201">Model binding is \* *_not_* _ case sensitive.</span></span>

<span data-ttu-id="8c3ac-202">Toutefois, les utilisateurs ne sont pas censés modifier l’URL pour rechercher un film.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-202">However, users can't be expected to modify the URL to search for a movie.</span></span> <span data-ttu-id="8c3ac-203">Dans cette étape, une interface utilisateur est ajoutée pour filtrer les films.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-203">In this step, UI is added to filter movies.</span></span> <span data-ttu-id="8c3ac-204">Si vous avez ajouté la contrainte d’itinéraire `"{searchString?}"`, supprimez-la.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-204">If you added the route constraint `"{searchString?}"`, remove it.</span></span>

<span data-ttu-id="8c3ac-205">Ouvrez le fichier _Pages/movies/ :::no-loc(Index)::: . cshtml \* et ajoutez le `<form>` balisage mis en surbrillance dans le code suivant :</span><span class="sxs-lookup"><span data-stu-id="8c3ac-205">Open the _Pages/Movies/:::no-loc(Index):::.cshtml\* file, and add the `<form>` markup highlighted in the following code:</span></span>

[!code-cshtml[](razor-pages-start/sample/:::no-loc(Razor):::PagesMovie22/Pages/Movies/:::no-loc(Index):::2.cshtml?highlight=14-19&range=1-22)]

<span data-ttu-id="8c3ac-206">La balise HTML `<form>` utilise les [Tag Helpers](xref:mvc/views/tag-helpers/intro) suivants :</span><span class="sxs-lookup"><span data-stu-id="8c3ac-206">The HTML `<form>` tag uses the following [Tag Helpers](xref:mvc/views/tag-helpers/intro):</span></span>

* <span data-ttu-id="8c3ac-207">[Tag Helper Form](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="8c3ac-207">[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="8c3ac-208">Lorsque le formulaire est envoyé, la chaîne de filtrage est envoyée à la page *pages/ :::no-loc(Index)::: movies/* page via la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-208">When the form is submitted, the filter string is sent to the *Pages/Movies/:::no-loc(Index):::* page via query string.</span></span>
* [<span data-ttu-id="8c3ac-209">Tag Helper Input</span><span class="sxs-lookup"><span data-stu-id="8c3ac-209">Input Tag Helper</span></span>](xref:mvc/views/working-with-forms#the-input-tag-helper)

<span data-ttu-id="8c3ac-210">Enregistrez les modifications apportées, puis testez le filtre.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-210">Save the changes and test the filter.</span></span>

![::: No-Loc (index) ::: View avec le mot fantôme tapé dans la zone de texte filtre de titre](search/_static/filter.png)

## <a name="search-by-genre"></a><span data-ttu-id="8c3ac-212">Rechercher par genre</span><span class="sxs-lookup"><span data-stu-id="8c3ac-212">Search by genre</span></span>

<span data-ttu-id="8c3ac-213">Mettez à jour la méthode `OnGetAsync` avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="8c3ac-213">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/:::no-loc(Razor):::PagesMovie22/Pages/Movies/:::no-loc(Index):::.cshtml.cs?name=snippet_SearchGenre)]

<span data-ttu-id="8c3ac-214">Le code suivant est une requête LINQ qui récupère tous les genres dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-214">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[](razor-pages-start/sample/:::no-loc(Razor):::PagesMovie22/Pages/Movies/:::no-loc(Index):::.cshtml.cs?name=snippet_LINQ)]

<span data-ttu-id="8c3ac-215">La liste `SelectList` de genres est créée en projetant des différents genres.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-215">The `SelectList` of genres is created by projecting the distinct genres.</span></span>

[!code-csharp[](razor-pages-start/sample/:::no-loc(Razor):::PagesMovie22/Pages/Movies/:::no-loc(Index):::.cshtml.cs?name=snippet_SelectList)]

### <a name="add-search-by-genre-to-the-no-locrazor-page"></a><span data-ttu-id="8c3ac-216">Ajouter la recherche par genre à la :::no-loc(Razor)::: page</span><span class="sxs-lookup"><span data-stu-id="8c3ac-216">Add search by genre to the :::no-loc(Razor)::: Page</span></span>

<span data-ttu-id="8c3ac-217">Mettez à jour le *:::no-loc(Index)::: . cshtml* [ `<form>` élément] ( https://developer.mozilla.org/docs/Web/HTML/Element/form) mis en surbrillance dans le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="8c3ac-217">Update the *:::no-loc(Index):::.cshtml* [`<form>` element] (https://developer.mozilla.org/docs/Web/HTML/Element/form) as highlighted in the following markup:</span></span>

[!code-cshtml[](razor-pages-start/sample/:::no-loc(Razor):::PagesMovie22/Pages/Movies/:::no-loc(Index):::FormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

<span data-ttu-id="8c3ac-218">Testez l’application en effectuant une recherche par genre, par titre de film et selon ces deux critères.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-218">Test the app by searching by genre, by movie title, and by both.</span></span>
<span data-ttu-id="8c3ac-219">Le code précédent utilise le [tag Helper Select](xref:mvc/views/working-with-forms#the-select-tag-helper) et option tag Helper.</span><span class="sxs-lookup"><span data-stu-id="8c3ac-219">The preceding code uses the [Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper) and Option Tag Helper.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8c3ac-220">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="8c3ac-220">Additional resources</span></span>

* [<span data-ttu-id="8c3ac-221">Version YouTube de ce tutoriel</span><span class="sxs-lookup"><span data-stu-id="8c3ac-221">YouTube version of this tutorial</span></span>](https://youtu.be/4B6pHtdyo08)

> [!div class="step-by-step"]
> <span data-ttu-id="8c3ac-222">[Précédent : mettre à jour les pages](xref:tutorials/razor-pages/da1) 
>  [Suivant : ajouter un nouveau champ](xref:tutorials/razor-pages/new-field)</span><span class="sxs-lookup"><span data-stu-id="8c3ac-222">[Previous: Update the pages](xref:tutorials/razor-pages/da1)
[Next: Add a new field](xref:tutorials/razor-pages/new-field)</span></span>

::: moniker-end
