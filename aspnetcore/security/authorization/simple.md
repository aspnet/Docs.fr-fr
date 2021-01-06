---
title: Autorisation simple dans ASP.NET Core
author: rick-anderson
description: Découvrez comment utiliser l’attribut Authorize pour restreindre l’accès aux contrôleurs et aux actions de ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
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
uid: security/authorization/simple
ms.openlocfilehash: 1678f1b4af2c65e3b10c66f7ccdbecf19156a834
ms.sourcegitcommit: 3593c4efa707edeaaceffbfa544f99f41fc62535
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/04/2021
ms.locfileid: "97865562"
---
# <a name="simple-authorization-in-aspnet-core"></a><span data-ttu-id="ce80b-103">Autorisation simple dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ce80b-103">Simple authorization in ASP.NET Core</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="ce80b-104">L’autorisation dans ASP.NET Core est contrôlée par <xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute> et ses différents paramètres.</span><span class="sxs-lookup"><span data-stu-id="ce80b-104">Authorization in ASP.NET Core is controlled with <xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute> and its various parameters.</span></span> <span data-ttu-id="ce80b-105">Dans sa forme la plus simple, l’application `[Authorize]` de l’attribut à un contrôleur, une action ou une Razor page limite l’accès à ce composant à n’importe quel utilisateur authentifié.</span><span class="sxs-lookup"><span data-stu-id="ce80b-105">In its simplest form, applying the `[Authorize]` attribute to a controller, action, or Razor Page, limits access to that component to any authenticated user.</span></span>

<span data-ttu-id="ce80b-106">Par exemple, le code suivant limite l’accès à `AccountController` n’importe quel utilisateur authentifié.</span><span class="sxs-lookup"><span data-stu-id="ce80b-106">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

```csharp
[Authorize]
public class AccountController : Controller
{
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

<span data-ttu-id="ce80b-107">Si vous souhaitez appliquer une autorisation à une action plutôt qu’au contrôleur, appliquez l' `AuthorizeAttribute` attribut à l’action elle-même :</span><span class="sxs-lookup"><span data-stu-id="ce80b-107">If you want to apply authorization to an action rather than the controller, apply the `AuthorizeAttribute` attribute to the action itself:</span></span>

```csharp
public class AccountController : Controller
{
   public ActionResult Login()
   {
   }

   [Authorize]
   public ActionResult Logout()
   {
   }
}
```

<span data-ttu-id="ce80b-108">Désormais, seuls les utilisateurs authentifiés peuvent accéder à la `Logout` fonction.</span><span class="sxs-lookup"><span data-stu-id="ce80b-108">Now only authenticated users can access the `Logout` function.</span></span>

<span data-ttu-id="ce80b-109">Vous pouvez également utiliser l' `AllowAnonymous` attribut pour autoriser l’accès par des utilisateurs non authentifiés à des actions individuelles.</span><span class="sxs-lookup"><span data-stu-id="ce80b-109">You can also use the `AllowAnonymous` attribute to allow access by non-authenticated users to individual actions.</span></span> <span data-ttu-id="ce80b-110">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="ce80b-110">For example:</span></span>

```csharp
[Authorize]
public class AccountController : Controller
{
    [AllowAnonymous]
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

<span data-ttu-id="ce80b-111">Cela autorise uniquement les utilisateurs authentifiés à, à l' `AccountController` exception de l' `Login` action, qui est accessible par tout le monde, indépendamment de leur état authentifié ou non authentifié/anonyme.</span><span class="sxs-lookup"><span data-stu-id="ce80b-111">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

> [!WARNING]
> <span data-ttu-id="ce80b-112">`[AllowAnonymous]` ignore toutes les instructions d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="ce80b-112">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="ce80b-113">Si vous combinez `[AllowAnonymous]` et n’importe quel `[Authorize]` attribut, les `[Authorize]` attributs sont ignorés.</span><span class="sxs-lookup"><span data-stu-id="ce80b-113">If you combine `[AllowAnonymous]` and any `[Authorize]` attribute, the `[Authorize]` attributes are ignored.</span></span> <span data-ttu-id="ce80b-114">Par exemple, si vous appliquez `[AllowAnonymous]` au niveau du contrôleur, tous les `[Authorize]` attributs du même contrôleur (ou de toute action qu’il contient) sont ignorés.</span><span class="sxs-lookup"><span data-stu-id="ce80b-114">For example if you apply `[AllowAnonymous]` at the controller level, any `[Authorize]` attributes on the same controller (or on any action within it) is ignored.</span></span>

[!INCLUDE[](~/includes/requireAuth.md)]

<a name="aarp"></a>

## <a name="authorize-attribute-and-no-locrazor-pages"></a><span data-ttu-id="ce80b-115">Autoriser l’attribut et les Razor pages</span><span class="sxs-lookup"><span data-stu-id="ce80b-115">Authorize attribute and Razor Pages</span></span>

<span data-ttu-id="ce80b-116">Le <xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute> ne peut **pas** être appliqué aux Razor gestionnaires de pages.</span><span class="sxs-lookup"><span data-stu-id="ce80b-116">The <xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute> can \***not** _ be applied to Razor Page handlers.</span></span> <span data-ttu-id="ce80b-117">Par exemple, `[Authorize]` ne peut pas être appliqué à `OnGet` , `OnPost` ou à un autre gestionnaire de page.</span><span class="sxs-lookup"><span data-stu-id="ce80b-117">For example, `[Authorize]` can't be applied to `OnGet`, `OnPost`, or any other page handler.</span></span> <span data-ttu-id="ce80b-118">Envisagez d’utiliser un contrôleur ASP.NET Core MVC pour les pages avec des exigences d’autorisation différentes pour différents gestionnaires.</span><span class="sxs-lookup"><span data-stu-id="ce80b-118">Consider using an ASP.NET Core MVC controller for pages with different authorization requirements for different handlers.</span></span>

<span data-ttu-id="ce80b-119">Les deux approches suivantes peuvent être utilisées pour appliquer l’autorisation aux Razor méthodes du gestionnaire de page :</span><span class="sxs-lookup"><span data-stu-id="ce80b-119">The following two approaches can be used to apply authorization to Razor Page handler methods:</span></span>

<span data-ttu-id="ce80b-120">_ Utiliser des pages distinctes pour les gestionnaires de pages nécessitant une autorisation différente.</span><span class="sxs-lookup"><span data-stu-id="ce80b-120">_ Use separate pages for page handlers requiring different authorization.</span></span> <span data-ttu-id="ce80b-121">Déplacez le contenu partagé dans une ou plusieurs [vues partielles](xref:mvc/views/partial).</span><span class="sxs-lookup"><span data-stu-id="ce80b-121">Move shared content into one or more [partial views](xref:mvc/views/partial).</span></span> <span data-ttu-id="ce80b-122">Dans la mesure du possible, il s’agit de l’approche recommandée.</span><span class="sxs-lookup"><span data-stu-id="ce80b-122">When possible, this is the recommended approach.</span></span>
* <span data-ttu-id="ce80b-123">Pour le contenu qui doit partager une page commune, écrivez un filtre qui effectue une autorisation dans le cadre de [IAsyncPageFilter. OnPageHandlerSelectionAsync](xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter.OnPageHandlerSelectionAsync%2A).</span><span class="sxs-lookup"><span data-stu-id="ce80b-123">For content that must share a common page, write a filter that performs authorization as part of [IAsyncPageFilter.OnPageHandlerSelectionAsync](xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter.OnPageHandlerSelectionAsync%2A).</span></span> <span data-ttu-id="ce80b-124">Le projet GitHub [PageHandlerAuth](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/simple/samples/3.1/PageHandlerAuth) illustre cette approche :</span><span class="sxs-lookup"><span data-stu-id="ce80b-124">The [PageHandlerAuth](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/simple/samples/3.1/PageHandlerAuth) GitHub project demonstrates this approach:</span></span>
  * <span data-ttu-id="ce80b-125">[AuthorizeIndexPageHandlerFilter](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/security/authorization/simple/samples/3.1/PageHandlerAuth/AuthorizeIndexPageHandlerFilter.cs) implémente le filtre d’autorisation :[!code-csharp[](~/security/authorization/simple/samples/3.1/PageHandlerAuth/Pages/Index.cshtml.cs?name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="ce80b-125">The [AuthorizeIndexPageHandlerFilter](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/security/authorization/simple/samples/3.1/PageHandlerAuth/AuthorizeIndexPageHandlerFilter.cs) implements the authorization filter: [!code-csharp[](~/security/authorization/simple/samples/3.1/PageHandlerAuth/Pages/Index.cshtml.cs?name=snippet)]</span></span>

  * <span data-ttu-id="ce80b-126">L’attribut [[AuthorizePageHandler]](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/simple/samples/3.1/PageHandlerAuth/Pages/Index.cshtml.cs#L16) est appliqué au `OnGet` Gestionnaire de page : [!code-csharp[](~/security/authorization/simple/samples/3.1/PageHandlerAuth/AuthorizeIndexPageHandlerFilter.cs?name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="ce80b-126">The [[AuthorizePageHandler]](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/simple/samples/3.1/PageHandlerAuth/Pages/Index.cshtml.cs#L16) attribute is applied to the `OnGet` page handler: [!code-csharp[](~/security/authorization/simple/samples/3.1/PageHandlerAuth/AuthorizeIndexPageHandlerFilter.cs?name=snippet)]</span></span>

> [!WARNING]
> <span data-ttu-id="ce80b-127">L’exemple d’approche [PageHandlerAuth](https://github.com/pranavkm/PageHandlerAuth) ne fait **pas** de \* : _ compose avec les attributs d’autorisation appliqués à la page, au modèle de page ou globalement.</span><span class="sxs-lookup"><span data-stu-id="ce80b-127">The [PageHandlerAuth](https://github.com/pranavkm/PageHandlerAuth) sample approach does \***not** _: _ Compose with authorization attributes applied to the page, page model, or globally.</span></span> <span data-ttu-id="ce80b-128">La composition des attributs d’autorisation entraîne l’exécution de plusieurs fois `AuthorizeAttribute` pour une ou plusieurs `AuthorizeFilter` instances sur la page.</span><span class="sxs-lookup"><span data-stu-id="ce80b-128">Composing authorization attributes results in authentication and authorization executing multiple times when you have one more `AuthorizeAttribute` or `AuthorizeFilter` instances also applied to the page.</span></span>
> * <span data-ttu-id="ce80b-129">Travaillez conjointement avec le reste du système d’authentification et d’autorisation de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ce80b-129">Work in conjunction with the rest of ASP.NET Core authentication and authorization system.</span></span> <span data-ttu-id="ce80b-130">Vous devez vérifier que l’utilisation de cette approche fonctionne correctement pour votre application.</span><span class="sxs-lookup"><span data-stu-id="ce80b-130">You must verify using this approach works correctly for your application.</span></span>

<span data-ttu-id="ce80b-131">Il n’est pas prévu de prendre en charge le `AuthorizeAttribute` sur les Razor gestionnaires de page.</span><span class="sxs-lookup"><span data-stu-id="ce80b-131">There are no plans to support the `AuthorizeAttribute` on Razor Page handlers.</span></span> 
