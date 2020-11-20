---
title: Appliquer le protocole HTTPs en ASP.NET Core
author: rick-anderson
description: Découvrez comment exiger le protocole HTTPs/TLS dans une application Web ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/06/2019
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
uid: security/enforcing-ssl
ms.openlocfilehash: 209d055f6205eceb9efb5434427c303345791809
ms.sourcegitcommit: 8363e44f630fcc6433ccd2a85f7aa9567cd274ed
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2020
ms.locfileid: "94981958"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="d071f-103">Appliquer le protocole HTTPs en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d071f-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="d071f-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d071f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d071f-105">Ce document montre comment :</span><span class="sxs-lookup"><span data-stu-id="d071f-105">This document shows how to:</span></span>

* <span data-ttu-id="d071f-106">Exiger le protocole HTTPs pour toutes les demandes.</span><span class="sxs-lookup"><span data-stu-id="d071f-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="d071f-107">Redirige toutes les requêtes HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d071f-107">Redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="d071f-108">Aucune API ne peut empêcher un client d’envoyer des données sensibles à la première demande.</span><span class="sxs-lookup"><span data-stu-id="d071f-108">No API can prevent a client from sending sensitive data on the first request.</span></span>

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> ## <a name="api-projects"></a><span data-ttu-id="d071f-109">Projets d’API</span><span class="sxs-lookup"><span data-stu-id="d071f-109">API projects</span></span>
>
> <span data-ttu-id="d071f-110">N' **not** utilisez pas [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) sur les API Web qui reçoivent des informations sensibles.</span><span class="sxs-lookup"><span data-stu-id="d071f-110">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="d071f-111">`RequireHttpsAttribute` utilise des codes d’état HTTP pour rediriger les navigateurs de HTTP vers HTTPs.</span><span class="sxs-lookup"><span data-stu-id="d071f-111">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="d071f-112">Les clients d’API peuvent ne pas comprendre ou respecter les redirections du protocole HTTP vers HTTPs.</span><span class="sxs-lookup"><span data-stu-id="d071f-112">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="d071f-113">Ces clients peuvent envoyer des informations via HTTP.</span><span class="sxs-lookup"><span data-stu-id="d071f-113">Such clients may send information over HTTP.</span></span> <span data-ttu-id="d071f-114">Les API Web doivent être les suivantes :</span><span class="sxs-lookup"><span data-stu-id="d071f-114">Web APIs should either:</span></span>
>
> * <span data-ttu-id="d071f-115">Ne pas écouter sur HTTP.</span><span class="sxs-lookup"><span data-stu-id="d071f-115">Not listen on HTTP.</span></span>
> * <span data-ttu-id="d071f-116">Fermez la connexion avec le code d’état 400 (requête incorrecte) et ne traitez pas la requête.</span><span class="sxs-lookup"><span data-stu-id="d071f-116">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>
>
> ## <a name="hsts-and-api-projects"></a><span data-ttu-id="d071f-117">Projets HSTS et API</span><span class="sxs-lookup"><span data-stu-id="d071f-117">HSTS and API projects</span></span>
>
> <span data-ttu-id="d071f-118">Les projets d’API par défaut n’incluent pas [HSTS](#hsts) , car HSTS est généralement une instruction de navigateur uniquement.</span><span class="sxs-lookup"><span data-stu-id="d071f-118">The default API projects don't include [HSTS](#hsts) because HSTS is generally a browser only instruction.</span></span> <span data-ttu-id="d071f-119">Les autres appelants, tels que les applications de bureau ou de téléphone, ne respectent **pas** l’instruction.</span><span class="sxs-lookup"><span data-stu-id="d071f-119">Other callers, such as phone or desktop apps, do **not** obey the instruction.</span></span> <span data-ttu-id="d071f-120">Même dans les navigateurs, un seul appel authentifié à une API sur HTTP présente des risques sur les réseaux non sécurisés.</span><span class="sxs-lookup"><span data-stu-id="d071f-120">Even within browsers, a single authenticated call to an API over HTTP has risks on insecure networks.</span></span> <span data-ttu-id="d071f-121">L’approche sécurisée consiste à configurer des projets d’API pour écouter et répondre uniquement au protocole HTTPs.</span><span class="sxs-lookup"><span data-stu-id="d071f-121">The secure approach is to configure API projects to only listen to and respond over HTTPS.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

> [!WARNING]
> ## <a name="api-projects"></a><span data-ttu-id="d071f-122">Projets d’API</span><span class="sxs-lookup"><span data-stu-id="d071f-122">API projects</span></span>
>
> <span data-ttu-id="d071f-123">N' **not** utilisez pas [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) sur les API Web qui reçoivent des informations sensibles.</span><span class="sxs-lookup"><span data-stu-id="d071f-123">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="d071f-124">`RequireHttpsAttribute` utilise des codes d’état HTTP pour rediriger les navigateurs de HTTP vers HTTPs.</span><span class="sxs-lookup"><span data-stu-id="d071f-124">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="d071f-125">Les clients d’API peuvent ne pas comprendre ou respecter les redirections du protocole HTTP vers HTTPs.</span><span class="sxs-lookup"><span data-stu-id="d071f-125">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="d071f-126">Ces clients peuvent envoyer des informations via HTTP.</span><span class="sxs-lookup"><span data-stu-id="d071f-126">Such clients may send information over HTTP.</span></span> <span data-ttu-id="d071f-127">Les API Web doivent être les suivantes :</span><span class="sxs-lookup"><span data-stu-id="d071f-127">Web APIs should either:</span></span>
>
> * <span data-ttu-id="d071f-128">Ne pas écouter sur HTTP.</span><span class="sxs-lookup"><span data-stu-id="d071f-128">Not listen on HTTP.</span></span>
> * <span data-ttu-id="d071f-129">Fermez la connexion avec le code d’état 400 (requête incorrecte) et ne traitez pas la requête.</span><span class="sxs-lookup"><span data-stu-id="d071f-129">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

::: moniker-end

## <a name="require-https"></a><span data-ttu-id="d071f-130">Exiger HTTPS</span><span class="sxs-lookup"><span data-stu-id="d071f-130">Require HTTPS</span></span>

<span data-ttu-id="d071f-131">Nous vous recommandons d’utiliser les applications Web de production ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="d071f-131">We recommend that production ASP.NET Core web apps use:</span></span>

* <span data-ttu-id="d071f-132">Intergiciel (middleware) de redirection HTTPs ( <xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*> ) pour rediriger les requêtes http vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d071f-132">HTTPS Redirection Middleware (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) to redirect HTTP requests to HTTPS.</span></span>
* <span data-ttu-id="d071f-133">Intergiciel (middleware) HSTS ([UseHsts](#http-strict-transport-security-protocol-hsts)) pour envoyer des en-têtes HSTS (protocole de sécurité de transport strict) aux clients.</span><span class="sxs-lookup"><span data-stu-id="d071f-133">HSTS Middleware ([UseHsts](#http-strict-transport-security-protocol-hsts)) to send HTTP Strict Transport Security Protocol (HSTS) headers to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="d071f-134">Les applications déployées dans une configuration de proxy inverse autorisent le proxy à gérer la sécurité de la connexion (HTTPs).</span><span class="sxs-lookup"><span data-stu-id="d071f-134">Apps deployed in a reverse proxy configuration allow the proxy to handle connection security (HTTPS).</span></span> <span data-ttu-id="d071f-135">Si le proxy gère également la redirection HTTPs, il n’est pas nécessaire d’utiliser l’intergiciel (middleware) de redirection HTTPs.</span><span class="sxs-lookup"><span data-stu-id="d071f-135">If the proxy also handles HTTPS redirection, there's no need to use HTTPS Redirection Middleware.</span></span> <span data-ttu-id="d071f-136">Si le serveur proxy gère également l’écriture d’en-têtes HSTS (par exemple, la [prise en charge native de HSTS dans IIS 10,0 (1709) ou version ultérieure](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)), l’application ne nécessite pas d’intergiciel HSTS.</span><span class="sxs-lookup"><span data-stu-id="d071f-136">If the proxy server also handles writing HSTS headers (for example, [native HSTS support in IIS 10.0 (1709) or later](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)), HSTS Middleware isn't required by the app.</span></span> <span data-ttu-id="d071f-137">Pour plus d’informations, consultez [opt-out de https/HSTS lors de la création du projet](#opt-out-of-httpshsts-on-project-creation).</span><span class="sxs-lookup"><span data-stu-id="d071f-137">For more information, see [Opt-out of HTTPS/HSTS on project creation](#opt-out-of-httpshsts-on-project-creation).</span></span>

### <a name="usehttpsredirection"></a><span data-ttu-id="d071f-138">UseHttpsRedirection</span><span class="sxs-lookup"><span data-stu-id="d071f-138">UseHttpsRedirection</span></span>

<span data-ttu-id="d071f-139">Le code suivant appelle `UseHttpsRedirection` dans la `Startup` classe :</span><span class="sxs-lookup"><span data-stu-id="d071f-139">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet1&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet1&highlight=13)]

::: moniker-end

<span data-ttu-id="d071f-140">Code mis en surbrillance précédent :</span><span class="sxs-lookup"><span data-stu-id="d071f-140">The preceding highlighted code:</span></span>

* <span data-ttu-id="d071f-141">Utilise la valeur par défaut [HttpsRedirectionOptions. RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span><span class="sxs-lookup"><span data-stu-id="d071f-141">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span></span>
* <span data-ttu-id="d071f-142">Utilise la valeur par défaut [HttpsRedirectionOptions. HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null), sauf si elle est remplacée par la `ASPNETCORE_HTTPS_PORT` variable d’environnement ou [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span><span class="sxs-lookup"><span data-stu-id="d071f-142">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) unless overridden by the `ASPNETCORE_HTTPS_PORT` environment variable or [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span></span>

<span data-ttu-id="d071f-143">Nous vous recommandons d’utiliser des redirections temporaires plutôt que des redirections permanentes.</span><span class="sxs-lookup"><span data-stu-id="d071f-143">We recommend using temporary redirects rather than permanent redirects.</span></span> <span data-ttu-id="d071f-144">La mise en cache des liens peut provoquer un comportement instable dans les environnements de développement.</span><span class="sxs-lookup"><span data-stu-id="d071f-144">Link caching can cause unstable behavior in development environments.</span></span> <span data-ttu-id="d071f-145">Si vous préférez envoyer un code d’état de redirection permanent lorsque l’application se trouve dans un environnement de non-développement, consultez la section [configurer des redirections permanentes en production](#configure-permanent-redirects-in-production) .</span><span class="sxs-lookup"><span data-stu-id="d071f-145">If you prefer to send a permanent redirect status code when the app is in a non-Development environment, see the [Configure permanent redirects in production](#configure-permanent-redirects-in-production) section.</span></span> <span data-ttu-id="d071f-146">Nous vous recommandons d’utiliser [HSTS](#http-strict-transport-security-protocol-hsts) pour signaler aux clients que seules les demandes de ressources sécurisées doivent être envoyées à l’application (uniquement en production).</span><span class="sxs-lookup"><span data-stu-id="d071f-146">We recommend using [HSTS](#http-strict-transport-security-protocol-hsts) to signal to clients that only secure resource requests should be sent to the app (only in production).</span></span>

### <a name="port-configuration"></a><span data-ttu-id="d071f-147">Configuration du port</span><span class="sxs-lookup"><span data-stu-id="d071f-147">Port configuration</span></span>

<span data-ttu-id="d071f-148">Un port doit être disponible pour que l’intergiciel redirige une requête non sécurisée vers HTTPs.</span><span class="sxs-lookup"><span data-stu-id="d071f-148">A port must be available for the middleware to redirect an insecure request to HTTPS.</span></span> <span data-ttu-id="d071f-149">Si aucun port n’est disponible :</span><span class="sxs-lookup"><span data-stu-id="d071f-149">If no port is available:</span></span>

* <span data-ttu-id="d071f-150">La redirection vers HTTPs ne se produit pas.</span><span class="sxs-lookup"><span data-stu-id="d071f-150">Redirection to HTTPS doesn't occur.</span></span>
* <span data-ttu-id="d071f-151">L’intergiciel enregistre l’avertissement « échec de la détermination du port HTTPS pour la redirection ».</span><span class="sxs-lookup"><span data-stu-id="d071f-151">The middleware logs the warning "Failed to determine the https port for redirect."</span></span>

<span data-ttu-id="d071f-152">Spécifiez le port HTTPs à l’aide de l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="d071f-152">Specify the HTTPS port using any of the following approaches:</span></span>

* <span data-ttu-id="d071f-153">Définissez [HttpsRedirectionOptions. HttpsPort](#options).</span><span class="sxs-lookup"><span data-stu-id="d071f-153">Set [HttpsRedirectionOptions.HttpsPort](#options).</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="d071f-154">Définissez le `https_port` [paramètre d’ordinateur hôte](../fundamentals/host/generic-host.md?view=aspnetcore-3.0#https_port):</span><span class="sxs-lookup"><span data-stu-id="d071f-154">Set the `https_port` [host setting](../fundamentals/host/generic-host.md?view=aspnetcore-3.0#https_port):</span></span>

  * <span data-ttu-id="d071f-155">Dans la configuration de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="d071f-155">In host configuration.</span></span>
  * <span data-ttu-id="d071f-156">En définissant la `ASPNETCORE_HTTPS_PORT` variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="d071f-156">By setting the `ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
  * <span data-ttu-id="d071f-157">En ajoutant une entrée de niveau supérieur dans *appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="d071f-157">By adding a top-level entry in *appsettings.json*:</span></span>

    [!code-json[](enforcing-ssl/sample-snapshot/3.x/appsettings.json?highlight=2)]

* <span data-ttu-id="d071f-158">Indiquez un port avec le schéma sécurisé à l’aide de la [variable d’environnement ASPNETCORE_URLS](../fundamentals/host/generic-host.md?view=aspnetcore-3.0#urls).</span><span class="sxs-lookup"><span data-stu-id="d071f-158">Indicate a port with the secure scheme using the [ASPNETCORE_URLS environment variable](../fundamentals/host/generic-host.md?view=aspnetcore-3.0#urls).</span></span> <span data-ttu-id="d071f-159">La variable d’environnement configure le serveur.</span><span class="sxs-lookup"><span data-stu-id="d071f-159">The environment variable configures the server.</span></span> <span data-ttu-id="d071f-160">L’intergiciel Découvre indirectement le port HTTPs via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> .</span><span class="sxs-lookup"><span data-stu-id="d071f-160">The middleware indirectly discovers the HTTPS port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span> <span data-ttu-id="d071f-161">Cette approche ne fonctionne pas dans les déploiements de proxy inverse.</span><span class="sxs-lookup"><span data-stu-id="d071f-161">This approach doesn't work in reverse proxy deployments.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

* <span data-ttu-id="d071f-162">Définissez le `https_port` [paramètre d’ordinateur hôte](xref:fundamentals/host/web-host#https-port):</span><span class="sxs-lookup"><span data-stu-id="d071f-162">Set the `https_port` [host setting](xref:fundamentals/host/web-host#https-port):</span></span>

  * <span data-ttu-id="d071f-163">Dans la configuration de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="d071f-163">In host configuration.</span></span>
  * <span data-ttu-id="d071f-164">En définissant la `ASPNETCORE_HTTPS_PORT` variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="d071f-164">By setting the `ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
  * <span data-ttu-id="d071f-165">En ajoutant une entrée de niveau supérieur dans *appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="d071f-165">By adding a top-level entry in *appsettings.json*:</span></span>

    [!code-json[](enforcing-ssl/sample-snapshot/2.x/appsettings.json?highlight=2)]

* <span data-ttu-id="d071f-166">Indiquez un port avec le schéma sécurisé à l’aide de la [variable d’environnement ASPNETCORE_URLS](xref:fundamentals/host/web-host#server-urls).</span><span class="sxs-lookup"><span data-stu-id="d071f-166">Indicate a port with the secure scheme using the [ASPNETCORE_URLS environment variable](xref:fundamentals/host/web-host#server-urls).</span></span> <span data-ttu-id="d071f-167">La variable d’environnement configure le serveur.</span><span class="sxs-lookup"><span data-stu-id="d071f-167">The environment variable configures the server.</span></span> <span data-ttu-id="d071f-168">L’intergiciel Découvre indirectement le port HTTPs via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> .</span><span class="sxs-lookup"><span data-stu-id="d071f-168">The middleware indirectly discovers the HTTPS port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span> <span data-ttu-id="d071f-169">Cette approche ne fonctionne pas dans les déploiements de proxy inverse.</span><span class="sxs-lookup"><span data-stu-id="d071f-169">This approach doesn't work in reverse proxy deployments.</span></span>

::: moniker-end

* <span data-ttu-id="d071f-170">Dans développement, définissez une URL HTTPs dans *launchsettings.js*.</span><span class="sxs-lookup"><span data-stu-id="d071f-170">In development, set an HTTPS URL in *launchsettings.json*.</span></span> <span data-ttu-id="d071f-171">Activez le protocole HTTPs lorsque IIS Express est utilisé.</span><span class="sxs-lookup"><span data-stu-id="d071f-171">Enable HTTPS when IIS Express is used.</span></span>

* <span data-ttu-id="d071f-172">Configurez un point de terminaison d’URL HTTPs pour un déploiement de périphérie public d’un serveur [Kestrel](xref:fundamentals/servers/kestrel) ou d’un serveur [HTTP.sys](xref:fundamentals/servers/httpsys) .</span><span class="sxs-lookup"><span data-stu-id="d071f-172">Configure an HTTPS URL endpoint for a public-facing edge deployment of [Kestrel](xref:fundamentals/servers/kestrel) server or [HTTP.sys](xref:fundamentals/servers/httpsys) server.</span></span> <span data-ttu-id="d071f-173">**Un seul port HTTPS** est utilisé par l’application.</span><span class="sxs-lookup"><span data-stu-id="d071f-173">Only **one HTTPS port** is used by the app.</span></span> <span data-ttu-id="d071f-174">L’intergiciel Découvre le port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> .</span><span class="sxs-lookup"><span data-stu-id="d071f-174">The middleware discovers the port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span>

> [!NOTE]
> <span data-ttu-id="d071f-175">Lorsqu’une application est exécutée dans une configuration de proxy inverse, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> n’est pas disponible.</span><span class="sxs-lookup"><span data-stu-id="d071f-175">When an app is run in a reverse proxy configuration, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> isn't available.</span></span> <span data-ttu-id="d071f-176">Définissez le port à l’aide de l’une des autres approches décrites dans cette section.</span><span class="sxs-lookup"><span data-stu-id="d071f-176">Set the port using one of the other approaches described in this section.</span></span>

### <a name="edge-deployments"></a><span data-ttu-id="d071f-177">Déploiements de périphérie</span><span class="sxs-lookup"><span data-stu-id="d071f-177">Edge deployments</span></span> 

<span data-ttu-id="d071f-178">Quand Kestrel ou HTTP.sys est utilisé en tant que serveur Edge public, Kestrel ou HTTP.sys doit être configuré pour écouter les deux :</span><span class="sxs-lookup"><span data-stu-id="d071f-178">When Kestrel or HTTP.sys is used as a public-facing edge server, Kestrel or HTTP.sys must be configured to listen on both:</span></span>

* <span data-ttu-id="d071f-179">Le port sécurisé où le client est redirigé (en général, 443 en production et 5001 en développement).</span><span class="sxs-lookup"><span data-stu-id="d071f-179">The secure port where the client is redirected (typically, 443 in production and 5001 in development).</span></span>
* <span data-ttu-id="d071f-180">Le port non sécurisé (en général, 80 en production et 5000 en développement).</span><span class="sxs-lookup"><span data-stu-id="d071f-180">The insecure port (typically, 80 in production and 5000 in development).</span></span>

<span data-ttu-id="d071f-181">Le port non sécurisé doit être accessible par le client afin que l’application puisse recevoir une demande non sécurisée et rediriger le client vers le port sécurisé.</span><span class="sxs-lookup"><span data-stu-id="d071f-181">The insecure port must be accessible by the client in order for the app to receive an insecure request and redirect the client to the secure port.</span></span>

<span data-ttu-id="d071f-182">Pour plus d’informations, consultez [configuration du point de terminaison Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) ou <xref:fundamentals/servers/httpsys> .</span><span class="sxs-lookup"><span data-stu-id="d071f-182">For more information, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) or <xref:fundamentals/servers/httpsys>.</span></span>

### <a name="deployment-scenarios"></a><span data-ttu-id="d071f-183">Scénarios de déploiement</span><span class="sxs-lookup"><span data-stu-id="d071f-183">Deployment scenarios</span></span>

<span data-ttu-id="d071f-184">Tout pare-feu entre le client et le serveur doit également disposer de ports de communication ouverts pour le trafic.</span><span class="sxs-lookup"><span data-stu-id="d071f-184">Any firewall between the client and server must also have communication ports open for traffic.</span></span>

<span data-ttu-id="d071f-185">Si les demandes sont transférées dans une configuration de proxy inverse, utilisez l' [intergiciel d’en-tête transféré](xref:host-and-deploy/proxy-load-balancer) avant d’appeler l’intergiciel (middleware) de REdirection HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d071f-185">If requests are forwarded in a reverse proxy configuration, use [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) before calling HTTPS Redirection Middleware.</span></span> <span data-ttu-id="d071f-186">L’intergiciel d’en-têtes transférés met à jour le `Request.Scheme` , à l’aide de l' `X-Forwarded-Proto` en-tête.</span><span class="sxs-lookup"><span data-stu-id="d071f-186">Forwarded Headers Middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header.</span></span> <span data-ttu-id="d071f-187">L’intergiciel permet de rediriger les URI et d’autres stratégies de sécurité pour fonctionner correctement.</span><span class="sxs-lookup"><span data-stu-id="d071f-187">The middleware permits redirect URIs and other security policies to work correctly.</span></span> <span data-ttu-id="d071f-188">Lorsque l’intergiciel d’en-têtes transférés n’est pas utilisé, l’application principale peut ne pas recevoir le schéma correct et se terminer dans une boucle de redirection.</span><span class="sxs-lookup"><span data-stu-id="d071f-188">When Forwarded Headers Middleware isn't used, the backend app might not receive the correct scheme and end up in a redirect loop.</span></span> <span data-ttu-id="d071f-189">Un message d’erreur de l’utilisateur final commun est qu’un trop grand nombre de redirections se sont produites.</span><span class="sxs-lookup"><span data-stu-id="d071f-189">A common end user error message is that too many redirects have occurred.</span></span>

<span data-ttu-id="d071f-190">Lors du déploiement sur Azure App Service, suivez les instructions du [Didacticiel : lier un certificat SSL personnalisé existant à Azure Web Apps](/azure/app-service/app-service-web-tutorial-custom-ssl).</span><span class="sxs-lookup"><span data-stu-id="d071f-190">When deploying to Azure App Service, follow the guidance in [Tutorial: Bind an existing custom SSL certificate to Azure Web Apps](/azure/app-service/app-service-web-tutorial-custom-ssl).</span></span>

### <a name="options"></a><span data-ttu-id="d071f-191">Options</span><span class="sxs-lookup"><span data-stu-id="d071f-191">Options</span></span>

<span data-ttu-id="d071f-192">Le code en surbrillance suivant appelle [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) pour configurer les options de l’intergiciel :</span><span class="sxs-lookup"><span data-stu-id="d071f-192">The following highlighted code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>


::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet2&highlight=14-18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet2&highlight=14-18)]

::: moniker-end


<span data-ttu-id="d071f-193">`AddHttpsRedirection`L’appel de est nécessaire uniquement pour modifier les valeurs de `HttpsPort` ou `RedirectStatusCode` .</span><span class="sxs-lookup"><span data-stu-id="d071f-193">Calling `AddHttpsRedirection` is only necessary to change the values of `HttpsPort` or `RedirectStatusCode`.</span></span>

<span data-ttu-id="d071f-194">Code mis en surbrillance précédent :</span><span class="sxs-lookup"><span data-stu-id="d071f-194">The preceding highlighted code:</span></span>

* <span data-ttu-id="d071f-195">Définit [HttpsRedirectionOptions. RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) sur <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect> , qui est la valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="d071f-195">Sets [HttpsRedirectionOptions.RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) to <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>, which is the default value.</span></span> <span data-ttu-id="d071f-196">Utilisez les champs de la <xref:Microsoft.AspNetCore.Http.StatusCodes> classe pour les assignations à `RedirectStatusCode` .</span><span class="sxs-lookup"><span data-stu-id="d071f-196">Use the fields of the <xref:Microsoft.AspNetCore.Http.StatusCodes> class for assignments to `RedirectStatusCode`.</span></span>
* <span data-ttu-id="d071f-197">Définit le port HTTPs sur 5001.</span><span class="sxs-lookup"><span data-stu-id="d071f-197">Sets the HTTPS port to 5001.</span></span>

#### <a name="configure-permanent-redirects-in-production"></a><span data-ttu-id="d071f-198">Configurer des redirections permanentes en production</span><span class="sxs-lookup"><span data-stu-id="d071f-198">Configure permanent redirects in production</span></span>

<span data-ttu-id="d071f-199">Par défaut, l’intergiciel envoie un [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) avec toutes les redirections.</span><span class="sxs-lookup"><span data-stu-id="d071f-199">The middleware defaults to sending a [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) with all redirects.</span></span> <span data-ttu-id="d071f-200">Si vous préférez envoyer un code d’état de redirection permanent lorsque l’application se trouve dans un environnement de non-développement, encapsulez la configuration des options d’intergiciel dans une vérification conditionnelle pour un environnement de non-développement.</span><span class="sxs-lookup"><span data-stu-id="d071f-200">If you prefer to send a permanent redirect status code when the app is in a non-Development environment, wrap the middleware options configuration in a conditional check for a non-Development environment.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d071f-201">Lors de la configuration des services dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="d071f-201">When configuring services in *Startup.cs*:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // IWebHostEnvironment (stored in _env) is injected into the Startup class.
    if (!_env.IsDevelopment())
    {
        services.AddHttpsRedirection(options =>
        {
            options.RedirectStatusCode = StatusCodes.Status308PermanentRedirect;
            options.HttpsPort = 443;
        });
    }
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="d071f-202">Lors de la configuration des services dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="d071f-202">When configuring services in *Startup.cs*:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // IHostingEnvironment (stored in _env) is injected into the Startup class.
    if (!_env.IsDevelopment())
    {
        services.AddHttpsRedirection(options =>
        {
            options.RedirectStatusCode = StatusCodes.Status308PermanentRedirect;
            options.HttpsPort = 443;
        });
    }
}
```

::: moniker-end


## <a name="https-redirection-middleware-alternative-approach"></a><span data-ttu-id="d071f-203">Approche alternative de l’intergiciel (middleware) de redirection HTTPs</span><span class="sxs-lookup"><span data-stu-id="d071f-203">HTTPS Redirection Middleware alternative approach</span></span>

<span data-ttu-id="d071f-204">Une alternative à l’utilisation de l’intergiciel () de redirection HTTPs ( `UseHttpsRedirection` ) consiste à utiliser l’intergiciel () de réécriture d’URL `AddRedirectToHttps` .</span><span class="sxs-lookup"><span data-stu-id="d071f-204">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="d071f-205">`AddRedirectToHttps` peut également définir le code et le port d’état lors de l’exécution de la redirection.</span><span class="sxs-lookup"><span data-stu-id="d071f-205">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="d071f-206">Pour plus d’informations, consultez intergiciel (middleware) de [réécriture d’URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="d071f-206">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="d071f-207">Lorsque vous redirigez vers le protocole HTTPs sans avoir besoin de règles de redirection supplémentaires, nous vous recommandons d’utiliser l’intergiciel () de redirection HTTPs ( `UseHttpsRedirection` ) décrit dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="d071f-207">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

<a name="hsts"></a>

## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="d071f-208">Protocole HTTP strict transport Security (HSTS)</span><span class="sxs-lookup"><span data-stu-id="d071f-208">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="d071f-209">Par [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [http strict transport Security (HSTS)](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Strict_Transport_Security_Cheat_Sheet.html) est une amélioration de sécurité qui est spécifiée par une application Web via l’utilisation d’un en-tête de réponse.</span><span class="sxs-lookup"><span data-stu-id="d071f-209">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Strict_Transport_Security_Cheat_Sheet.html) is an opt-in security enhancement that's specified by a web app through the use of a response header.</span></span> <span data-ttu-id="d071f-210">Quand un [navigateur prenant en charge HSTS](https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Protection_Cheat_Sheet.html#browser-support) reçoit cet en-tête :</span><span class="sxs-lookup"><span data-stu-id="d071f-210">When a [browser that supports HSTS](https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Protection_Cheat_Sheet.html#browser-support) receives this header:</span></span>

* <span data-ttu-id="d071f-211">Le navigateur stocke la configuration pour le domaine qui empêche l’envoi de toute communication sur HTTP.</span><span class="sxs-lookup"><span data-stu-id="d071f-211">The browser stores configuration for the domain that prevents sending any communication over HTTP.</span></span> <span data-ttu-id="d071f-212">Le navigateur force toutes les communications sur HTTPs.</span><span class="sxs-lookup"><span data-stu-id="d071f-212">The browser forces all communication over HTTPS.</span></span>
* <span data-ttu-id="d071f-213">Le navigateur empêche l’utilisateur d’utiliser des certificats non approuvés ou non valides.</span><span class="sxs-lookup"><span data-stu-id="d071f-213">The browser prevents the user from using untrusted or invalid certificates.</span></span> <span data-ttu-id="d071f-214">Le navigateur désactive les invites qui permettent à un utilisateur d’approuver temporairement ce type de certificat.</span><span class="sxs-lookup"><span data-stu-id="d071f-214">The browser disables prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="d071f-215">Étant donné que HSTS est appliqué par le client, il présente certaines limitations :</span><span class="sxs-lookup"><span data-stu-id="d071f-215">Because HSTS is enforced by the client, it has some limitations:</span></span>

* <span data-ttu-id="d071f-216">Le client doit prendre en charge HSTS.</span><span class="sxs-lookup"><span data-stu-id="d071f-216">The client must support HSTS.</span></span>
* <span data-ttu-id="d071f-217">HSTS nécessite au moins une demande HTTPs réussie pour établir la stratégie HSTS.</span><span class="sxs-lookup"><span data-stu-id="d071f-217">HSTS requires at least one successful HTTPS request to establish the HSTS policy.</span></span>
* <span data-ttu-id="d071f-218">L’application doit vérifier chaque requête HTTP et rediriger ou rejeter la requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="d071f-218">The application must check every HTTP request and redirect or reject the HTTP request.</span></span>

<span data-ttu-id="d071f-219">ASP.NET Core 2,1 et versions ultérieures implémentent HSTS avec la `UseHsts` méthode d’extension.</span><span class="sxs-lookup"><span data-stu-id="d071f-219">ASP.NET Core 2.1 and later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="d071f-220">Le code suivant appelle `UseHsts` lorsque l’application n’est pas en [mode de développement](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="d071f-220">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet1&highlight=11)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet1&highlight=10)]

::: moniker-end

<span data-ttu-id="d071f-221">`UseHsts` n’est pas recommandé dans le développement, car les paramètres HSTS sont très facilement mis en cache par les navigateurs.</span><span class="sxs-lookup"><span data-stu-id="d071f-221">`UseHsts` isn't recommended in development because the HSTS settings are highly cacheable by browsers.</span></span> <span data-ttu-id="d071f-222">Par défaut, `UseHsts` exclut l’adresse de bouclage locale.</span><span class="sxs-lookup"><span data-stu-id="d071f-222">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="d071f-223">Pour les environnements de production qui implémentent le protocole HTTPs pour la première fois, définissez [HstsOptions. MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) initiale sur une petite valeur à l’aide de l’une des <xref:System.TimeSpan> méthodes.</span><span class="sxs-lookup"><span data-stu-id="d071f-223">For production environments that are implementing HTTPS for the first time, set the initial [HstsOptions.MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) to a small value using one of the <xref:System.TimeSpan> methods.</span></span> <span data-ttu-id="d071f-224">Définissez la valeur des heures sur un seul jour si vous devez rétablir l’infrastructure HTTPs sur HTTP.</span><span class="sxs-lookup"><span data-stu-id="d071f-224">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="d071f-225">Une fois que vous êtes certain de la durabilité de la configuration HTTPs, augmentez la valeur de HSTS `max-age` ; une valeur couramment utilisée est d’un an.</span><span class="sxs-lookup"><span data-stu-id="d071f-225">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS `max-age` value; a commonly used value is one year.</span></span>

<span data-ttu-id="d071f-226">Le code suivant :</span><span class="sxs-lookup"><span data-stu-id="d071f-226">The following code:</span></span>


::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet2&highlight=5-12)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet2&highlight=5-12)]

::: moniker-end


* <span data-ttu-id="d071f-227">Définit le paramètre de préchargement de l' `Strict-Transport-Security` en-tête.</span><span class="sxs-lookup"><span data-stu-id="d071f-227">Sets the preload parameter of the `Strict-Transport-Security` header.</span></span> <span data-ttu-id="d071f-228">Le préchargement ne fait pas partie de la [spécification RFC HSTS](https://tools.ietf.org/html/rfc6797), mais est pris en charge par les navigateurs Web pour précharger des sites HSTS sur une nouvelle installation.</span><span class="sxs-lookup"><span data-stu-id="d071f-228">Preload isn't part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="d071f-229">Pour plus d’informations, consultez [https://hstspreload.org/](https://hstspreload.org/).</span><span class="sxs-lookup"><span data-stu-id="d071f-229">For more information, see [https://hstspreload.org/](https://hstspreload.org/).</span></span>
* <span data-ttu-id="d071f-230">Active [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), qui applique la stratégie HSTS pour héberger des sous-domaines.</span><span class="sxs-lookup"><span data-stu-id="d071f-230">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span>
* <span data-ttu-id="d071f-231">Définit explicitement le `max-age` paramètre de l' `Strict-Transport-Security` en-tête à 60 jours.</span><span class="sxs-lookup"><span data-stu-id="d071f-231">Explicitly sets the `max-age` parameter of the `Strict-Transport-Security` header to 60 days.</span></span> <span data-ttu-id="d071f-232">S’il n’est pas défini, la valeur par défaut est 30 jours.</span><span class="sxs-lookup"><span data-stu-id="d071f-232">If not set, defaults to 30 days.</span></span> <span data-ttu-id="d071f-233">Pour plus d’informations, consultez la [directive max-age](https://tools.ietf.org/html/rfc6797#section-6.1.1).</span><span class="sxs-lookup"><span data-stu-id="d071f-233">For more information, see the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1).</span></span>
* <span data-ttu-id="d071f-234">Ajoute `example.com` à la liste des hôtes à exclure.</span><span class="sxs-lookup"><span data-stu-id="d071f-234">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="d071f-235">`UseHsts` exclut les hôtes de bouclage suivants :</span><span class="sxs-lookup"><span data-stu-id="d071f-235">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="d071f-236">`localhost` : Adresse de bouclage IPv4.</span><span class="sxs-lookup"><span data-stu-id="d071f-236">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="d071f-237">`127.0.0.1` : Adresse de bouclage IPv4.</span><span class="sxs-lookup"><span data-stu-id="d071f-237">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="d071f-238">`[::1]` : Adresse de bouclage IPv6.</span><span class="sxs-lookup"><span data-stu-id="d071f-238">`[::1]` : The IPv6 loopback address.</span></span>

## <a name="opt-out-of-httpshsts-on-project-creation"></a><span data-ttu-id="d071f-239">Désactiver HTTPs/HSTS lors de la création du projet</span><span class="sxs-lookup"><span data-stu-id="d071f-239">Opt-out of HTTPS/HSTS on project creation</span></span>

<span data-ttu-id="d071f-240">Dans certains scénarios de service backend où la sécurité de connexion est gérée au niveau du périmètre public du réseau, la configuration de la sécurité des connexions sur chaque nœud n’est pas obligatoire.</span><span class="sxs-lookup"><span data-stu-id="d071f-240">In some backend service scenarios where connection security is handled at the public-facing edge of the network, configuring connection security at each node isn't required.</span></span> <span data-ttu-id="d071f-241">Les applications Web générées à partir des modèles dans Visual Studio ou à partir de la commande [dotnet New](/dotnet/core/tools/dotnet-new) activent la [redirection https](#require-https) et [HSTS](#http-strict-transport-security-protocol-hsts).</span><span class="sxs-lookup"><span data-stu-id="d071f-241">Web apps that are generated from the templates in Visual Studio or from the [dotnet new](/dotnet/core/tools/dotnet-new) command enable [HTTPS redirection](#require-https) and [HSTS](#http-strict-transport-security-protocol-hsts).</span></span> <span data-ttu-id="d071f-242">Pour les déploiements qui n’ont pas besoin de ces scénarios, vous pouvez désactiver HTTPs/HSTS quand l’application est créée à partir du modèle.</span><span class="sxs-lookup"><span data-stu-id="d071f-242">For deployments that don't require these scenarios, you can opt-out of HTTPS/HSTS when the app is created from the template.</span></span>

<span data-ttu-id="d071f-243">Pour désactiver HTTPs/HSTS :</span><span class="sxs-lookup"><span data-stu-id="d071f-243">To opt-out of HTTPS/HSTS:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="d071f-244">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d071f-244">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="d071f-245">Désactivez la case à cocher **configurer pour HTTPS** .</span><span class="sxs-lookup"><span data-stu-id="d071f-245">Uncheck the **Configure for HTTPS** check box.</span></span>

::: moniker range=">= aspnetcore-3.0"

![Nouvelle boîte de dialogue d’application Web ASP.NET Core avec la case à cocher configurer pour HTTPs désélectionnée.](enforcing-ssl/_static/out-vs2019.png)

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

![Nouvelle boîte de dialogue d’application Web ASP.NET Core avec la case à cocher configurer pour HTTPs désélectionnée.](enforcing-ssl/_static/out.png)

::: moniker-end


# <a name="net-core-cli"></a>[<span data-ttu-id="d071f-248">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="d071f-248">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="d071f-249">Utilisez l'option `--no-https`.</span><span class="sxs-lookup"><span data-stu-id="d071f-249">Use the `--no-https` option.</span></span> <span data-ttu-id="d071f-250">Par exemple</span><span class="sxs-lookup"><span data-stu-id="d071f-250">For example</span></span>

```dotnetcli
dotnet new webapp --no-https
```

---

<a name="trust"></a>

## <a name="trust-the-aspnet-core-https-development-certificate-on-windows-and-macos"></a><span data-ttu-id="d071f-251">Approuver le certificat de développement HTTPs ASP.NET Core sur Windows et macOS</span><span class="sxs-lookup"><span data-stu-id="d071f-251">Trust the ASP.NET Core HTTPS development certificate on Windows and macOS</span></span>

<span data-ttu-id="d071f-252">Le kit SDK .NET Core comprend un certificat de développement HTTPs.</span><span class="sxs-lookup"><span data-stu-id="d071f-252">The .NET Core SDK includes an HTTPS development certificate.</span></span> <span data-ttu-id="d071f-253">Le certificat est installé dans le cadre de la première exécution.</span><span class="sxs-lookup"><span data-stu-id="d071f-253">The certificate is installed as part of the first-run experience.</span></span> <span data-ttu-id="d071f-254">Par exemple, `dotnet --info` produit une variation de la sortie suivante :</span><span class="sxs-lookup"><span data-stu-id="d071f-254">For example, `dotnet --info` produces a variation of the following output:</span></span>

```
ASP.NET Core
------------
Successfully installed the ASP.NET Core HTTPS Development Certificate.
To trust the certificate run 'dotnet dev-certs https --trust' (Windows and macOS only).
For establishing trust on other platforms refer to the platform specific documentation.
For more information on configuring HTTPS see https://go.microsoft.com/fwlink/?linkid=848054.
```

<span data-ttu-id="d071f-255">L’installation du kit SDK .NET Core installe le certificat de développement ASP.NET Core HTTPS dans le magasin de certificats de l’utilisateur local.</span><span class="sxs-lookup"><span data-stu-id="d071f-255">Installing the .NET Core SDK installs the ASP.NET Core HTTPS development certificate to the local user certificate store.</span></span> <span data-ttu-id="d071f-256">Le certificat a été installé, mais il n’est pas approuvé.</span><span class="sxs-lookup"><span data-stu-id="d071f-256">The certificate has been installed, but it's not trusted.</span></span> <span data-ttu-id="d071f-257">Pour approuver le certificat, effectuez l’étape unique pour exécuter l' `dev-certs` outil dotnet :</span><span class="sxs-lookup"><span data-stu-id="d071f-257">To trust the certificate, perform the one-time step to run the dotnet `dev-certs` tool:</span></span>

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="d071f-258">La commande suivante fournit de l’aide sur l’outil `dev-certs` :</span><span class="sxs-lookup"><span data-stu-id="d071f-258">The following command provides help on the `dev-certs` tool:</span></span>

```dotnetcli
dotnet dev-certs https --help
```

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a><span data-ttu-id="d071f-259">Comment configurer un certificat de développeur pour l’arrimeur</span><span class="sxs-lookup"><span data-stu-id="d071f-259">How to set up a developer certificate for Docker</span></span>

<span data-ttu-id="d071f-260">Consultez [ce problème GitHub](https://github.com/dotnet/AspNetCore.Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="d071f-260">See [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/6199).</span></span>

<a name="ssl-linux"></a>

## <a name="trust-https-certificate-on-linux"></a><span data-ttu-id="d071f-261">Approuver le certificat HTTPs sur Linux</span><span class="sxs-lookup"><span data-stu-id="d071f-261">Trust HTTPS certificate on Linux</span></span>

<!-- Instructions to be updated by engineering team after 5.0 RTM. -->

<span data-ttu-id="d071f-262">Pour obtenir des instructions sur Linux, reportez-vous à la documentation relative à la distribution.</span><span class="sxs-lookup"><span data-stu-id="d071f-262">For instructions on Linux, refer to the distribution documentation.</span></span>

<a name="wsl"></a>

## <a name="trust-https-certificate-from-windows-subsystem-for-linux"></a><span data-ttu-id="d071f-263">Approuver le certificat HTTPs à partir du sous-système Windows pour Linux</span><span class="sxs-lookup"><span data-stu-id="d071f-263">Trust HTTPS certificate from Windows Subsystem for Linux</span></span>

<span data-ttu-id="d071f-264">Le sous-système Windows pour Linux (WSL) génère un certificat auto-signé HTTPs. Pour configurer le magasin de certificats Windows de façon à ce qu’il approuve le certificat WSL :</span><span class="sxs-lookup"><span data-stu-id="d071f-264">The Windows Subsystem for Linux (WSL) generates an HTTPS self-signed cert. To configure the Windows certificate store to trust the WSL certificate:</span></span>

* <span data-ttu-id="d071f-265">Exécutez la commande suivante pour exporter le certificat généré par WSL :</span><span class="sxs-lookup"><span data-stu-id="d071f-265">Run the following command to export the WSL-generated certificate:</span></span>

  ```
  dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p <cryptic-password>
  ```
* <span data-ttu-id="d071f-266">Dans une fenêtre WSL, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="d071f-266">In a WSL window, run the following command:</span></span>

  ```
    ASPNETCORE_Kestrel__Certificates__Default__Password="<cryptic-password>" 
    ASPNETCORE_Kestrel__Certificates__Default__Path=/mnt/c/Users/user-name/.aspnet/https/aspnetapp.pfx
    dotnet watch run
  ```

  <span data-ttu-id="d071f-267">La commande précédente définit les variables d’environnement pour que Linux utilise le certificat de confiance Windows.</span><span class="sxs-lookup"><span data-stu-id="d071f-267">The preceding command sets the environment variables so Linux uses the Windows trusted certificate.</span></span>

## <a name="troubleshoot-certificate-problems"></a><span data-ttu-id="d071f-268">Résoudre les problèmes de certificat</span><span class="sxs-lookup"><span data-stu-id="d071f-268">Troubleshoot certificate problems</span></span>

<span data-ttu-id="d071f-269">Cette section fournit de l’aide lorsque le certificat de développement HTTPs ASP.NET Core a été [installé et approuvé](#trust), mais que vous avez encore des avertissements de navigateur indiquant que le certificat n’est pas approuvé.</span><span class="sxs-lookup"><span data-stu-id="d071f-269">This section provides help when the ASP.NET Core HTTPS development certificate has been [installed and trusted](#trust), but you still have browser warnings that the certificate is not trusted.</span></span> <span data-ttu-id="d071f-270">Le certificat de développement HTTPs ASP.NET Core est utilisé par [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="d071f-270">The ASP.NET Core HTTPS development certificate is used by [Kestrel](xref:fundamentals/servers/kestrel).</span></span>

### <a name="all-platforms---certificate-not-trusted"></a><span data-ttu-id="d071f-271">Toutes les plateformes-certificat non approuvé</span><span class="sxs-lookup"><span data-stu-id="d071f-271">All platforms - certificate not trusted</span></span>

<span data-ttu-id="d071f-272">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="d071f-272">Run the following commands:</span></span>

```dotnetcli
dotnet dev-certs https --clean
dotnet dev-certs https --trust
```

<span data-ttu-id="d071f-273">Fermez toutes les instances de navigateur ouvertes.</span><span class="sxs-lookup"><span data-stu-id="d071f-273">Close any browser instances open.</span></span> <span data-ttu-id="d071f-274">Ouvrez une nouvelle fenêtre de navigateur pour l’application.</span><span class="sxs-lookup"><span data-stu-id="d071f-274">Open a new browser window to app.</span></span> <span data-ttu-id="d071f-275">L’approbation de certificat est mise en cache par les navigateurs.</span><span class="sxs-lookup"><span data-stu-id="d071f-275">Certificate trust is cached by browsers.</span></span>

<span data-ttu-id="d071f-276">Les commandes précédentes résolvent la plupart des problèmes d’approbation du navigateur.</span><span class="sxs-lookup"><span data-stu-id="d071f-276">The preceding commands solve most browser trust issues.</span></span> <span data-ttu-id="d071f-277">Si le navigateur n’approuve toujours pas le certificat, suivez les suggestions spécifiques à la plateforme qui suivent.</span><span class="sxs-lookup"><span data-stu-id="d071f-277">If the browser is still not trusting the certificate, follow the platform-specific suggestions that follow.</span></span>

### <a name="docker---certificate-not-trusted"></a><span data-ttu-id="d071f-278">Dockr-certificat non approuvé</span><span class="sxs-lookup"><span data-stu-id="d071f-278">Docker - certificate not trusted</span></span>

* <span data-ttu-id="d071f-279">Supprimez le dossier *C:\Users \{ User} \AppData\Roaming\ASP.NET\Https* .</span><span class="sxs-lookup"><span data-stu-id="d071f-279">Delete the *C:\Users\{USER}\AppData\Roaming\ASP.NET\Https* folder.</span></span>
* <span data-ttu-id="d071f-280">Nettoyez la solution.</span><span class="sxs-lookup"><span data-stu-id="d071f-280">Clean the solution.</span></span> <span data-ttu-id="d071f-281">Supprimez les dossiers *bin* et *obj*.</span><span class="sxs-lookup"><span data-stu-id="d071f-281">Delete the *bin* and *obj* folders.</span></span>
* <span data-ttu-id="d071f-282">Redémarrez l’outil de développement.</span><span class="sxs-lookup"><span data-stu-id="d071f-282">Restart the development tool.</span></span> <span data-ttu-id="d071f-283">Par exemple, Visual Studio, Visual Studio Code ou Visual Studio pour Mac.</span><span class="sxs-lookup"><span data-stu-id="d071f-283">For example, Visual Studio, Visual Studio Code, or Visual Studio for Mac.</span></span>

### <a name="windows---certificate-not-trusted"></a><span data-ttu-id="d071f-284">Windows-certificat non approuvé</span><span class="sxs-lookup"><span data-stu-id="d071f-284">Windows - certificate not trusted</span></span>

* <span data-ttu-id="d071f-285">Vérifiez les certificats dans le magasin de certificats.</span><span class="sxs-lookup"><span data-stu-id="d071f-285">Check the certificates in the certificate store.</span></span> <span data-ttu-id="d071f-286">Il doit y avoir un `localhost` certificat avec le `ASP.NET Core HTTPS development certificate` nom convivial à la fois sous `Current User > Personal > Certificates` et `Current User > Trusted root certification authorities > Certificates`</span><span class="sxs-lookup"><span data-stu-id="d071f-286">There should be a `localhost` certificate with the `ASP.NET Core HTTPS development certificate` friendly name both under `Current User > Personal > Certificates` and `Current User > Trusted root certification authorities > Certificates`</span></span>
* <span data-ttu-id="d071f-287">Supprimez tous les certificats détectés des autorités de certification racines personnelles et de confiance.</span><span class="sxs-lookup"><span data-stu-id="d071f-287">Remove all the found certificates from both Personal and Trusted root certification authorities.</span></span> <span data-ttu-id="d071f-288">Ne supprimez **pas** le certificat IIS Express localhost.</span><span class="sxs-lookup"><span data-stu-id="d071f-288">Do **not** remove the IIS Express localhost certificate.</span></span>
* <span data-ttu-id="d071f-289">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="d071f-289">Run the following commands:</span></span>

```dotnetcli
dotnet dev-certs https --clean
dotnet dev-certs https --trust
```

<span data-ttu-id="d071f-290">Fermez toutes les instances de navigateur ouvertes.</span><span class="sxs-lookup"><span data-stu-id="d071f-290">Close any browser instances open.</span></span> <span data-ttu-id="d071f-291">Ouvrez une nouvelle fenêtre de navigateur pour l’application.</span><span class="sxs-lookup"><span data-stu-id="d071f-291">Open a new browser window to app.</span></span>

### <a name="os-x---certificate-not-trusted"></a><span data-ttu-id="d071f-292">OS X-certificat non approuvé</span><span class="sxs-lookup"><span data-stu-id="d071f-292">OS X - certificate not trusted</span></span>

* <span data-ttu-id="d071f-293">Ouvrez le trousseau d’accès.</span><span class="sxs-lookup"><span data-stu-id="d071f-293">Open KeyChain Access.</span></span>
* <span data-ttu-id="d071f-294">Sélectionnez le trousseau système.</span><span class="sxs-lookup"><span data-stu-id="d071f-294">Select the System keychain.</span></span>
* <span data-ttu-id="d071f-295">Vérifiez la présence d’un certificat localhost.</span><span class="sxs-lookup"><span data-stu-id="d071f-295">Check for the presence of a localhost certificate.</span></span>
* <span data-ttu-id="d071f-296">Vérifiez qu’il contient un `+` symbole sur l’icône pour indiquer qu’il est approuvé pour tous les utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="d071f-296">Check that it contains a `+` symbol on the icon to indicate it's trusted for all users.</span></span>
* <span data-ttu-id="d071f-297">Supprimez le certificat du trousseau système.</span><span class="sxs-lookup"><span data-stu-id="d071f-297">Remove the certificate from the system keychain.</span></span>
* <span data-ttu-id="d071f-298">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="d071f-298">Run the following commands:</span></span>

```dotnetcli
dotnet dev-certs https --clean
dotnet dev-certs https --trust
```

<span data-ttu-id="d071f-299">Fermez toutes les instances de navigateur ouvertes.</span><span class="sxs-lookup"><span data-stu-id="d071f-299">Close any browser instances open.</span></span> <span data-ttu-id="d071f-300">Ouvrez une nouvelle fenêtre de navigateur pour l’application.</span><span class="sxs-lookup"><span data-stu-id="d071f-300">Open a new browser window to app.</span></span>

<span data-ttu-id="d071f-301">Consultez [erreur HTTPS à l’aide de IIS Express (dotnet/AspNetCore #16892)](https://github.com/dotnet/AspNetCore/issues/16892) pour résoudre les problèmes liés aux certificats dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d071f-301">See [HTTPS Error using IIS Express (dotnet/AspNetCore #16892)](https://github.com/dotnet/AspNetCore/issues/16892) for troubleshooting certificate issues with Visual Studio.</span></span>

### <a name="iis-express-ssl-certificate-used-with-visual-studio"></a><span data-ttu-id="d071f-302">IIS Express certificat SSL utilisé avec Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d071f-302">IIS Express SSL certificate used with Visual Studio</span></span>

<span data-ttu-id="d071f-303">Pour résoudre les problèmes liés au certificat de IIS Express, sélectionnez **réparer** dans le programme d’installation de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d071f-303">To fix problems with the IIS Express certificate, select **Repair** from the Visual Studio installer.</span></span> <span data-ttu-id="d071f-304">Pour plus d’informations, consultez [ce problème GitHub](https://github.com/dotnet/aspnetcore/issues/16892).</span><span class="sxs-lookup"><span data-stu-id="d071f-304">For more information, see [this GitHub issue](https://github.com/dotnet/aspnetcore/issues/16892).</span></span>

<a name="trust-ff"></a>

### <a name="firefox-sec_error_inadequate_key_usage-certificate-error"></a><span data-ttu-id="d071f-305">Erreur de certificat de SEC_ERROR_INADEQUATE_KEY_USAGE Firefox</span><span class="sxs-lookup"><span data-stu-id="d071f-305">Firefox SEC_ERROR_INADEQUATE_KEY_USAGE certificate error</span></span>

<span data-ttu-id="d071f-306">Le navigateur Firefox utilise son propre magasin de certificats. par conséquent, il n’approuve pas les certificats de développeur [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ou [Kestrel](xref:fundamentals/servers/kestrel) .</span><span class="sxs-lookup"><span data-stu-id="d071f-306">The Firefox browser uses it's own certificate store, and therefore doesn't trust the [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) or [Kestrel](xref:fundamentals/servers/kestrel) developer certificates.</span></span>

<span data-ttu-id="d071f-307">Pour utiliser Firefox avec IIS Express ou Kestrel, définissez  `security.enterprise_roots.enabled` = `true`</span><span class="sxs-lookup"><span data-stu-id="d071f-307">To use Firefox with IIS Express or Kestrel, set  `security.enterprise_roots.enabled` = `true`</span></span>

1. <span data-ttu-id="d071f-308">Entrez `about:config` dans le navigateur Firefox.</span><span class="sxs-lookup"><span data-stu-id="d071f-308">Enter `about:config` in the FireFox browser.</span></span>
1. <span data-ttu-id="d071f-309">Sélectionnez **accepter le risque et continuer** si vous acceptez le risque.</span><span class="sxs-lookup"><span data-stu-id="d071f-309">Select **Accept the Risk and Continue** if you accept the risk.</span></span>
1. <span data-ttu-id="d071f-310">Sélectionner **Afficher tout**</span><span class="sxs-lookup"><span data-stu-id="d071f-310">Select **Show All**</span></span>
1. <span data-ttu-id="d071f-311">Définie `security.enterprise_roots.enabled` = `true`</span><span class="sxs-lookup"><span data-stu-id="d071f-311">Set `security.enterprise_roots.enabled` = `true`</span></span>
1. <span data-ttu-id="d071f-312">Quitter et redémarrer Firefox</span><span class="sxs-lookup"><span data-stu-id="d071f-312">Exit and restart Firefox</span></span>

<span data-ttu-id="d071f-313">Pour plus d’informations, consultez [Configuration des autorités de certification (ca) dans Firefox](https://support.mozilla.org/kb/setting-certificate-authorities-firefox).</span><span class="sxs-lookup"><span data-stu-id="d071f-313">For more information, see [Setting Up Certificate Authorities (CAs) in Firefox](https://support.mozilla.org/kb/setting-certificate-authorities-firefox).</span></span>

## <a name="additional-information"></a><span data-ttu-id="d071f-314">Informations supplémentaires</span><span class="sxs-lookup"><span data-stu-id="d071f-314">Additional information</span></span>

* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="d071f-315">ASP.NET Core d’hôte sur Linux avec Apache : configuration HTTPs</span><span class="sxs-lookup"><span data-stu-id="d071f-315">Host ASP.NET Core on Linux with Apache: HTTPS configuration</span></span>](xref:host-and-deploy/linux-apache#https-configuration)
* [<span data-ttu-id="d071f-316">ASP.NET Core d’hôte sur Linux avec Nginx : configuration HTTPs</span><span class="sxs-lookup"><span data-stu-id="d071f-316">Host ASP.NET Core on Linux with Nginx: HTTPS configuration</span></span>](xref:host-and-deploy/linux-nginx#https-configuration)
* [<span data-ttu-id="d071f-317">Configuration du protocole SSL sur IIS</span><span class="sxs-lookup"><span data-stu-id="d071f-317">How to Set Up SSL on IIS</span></span>](/iis/manage/configuring-security/how-to-set-up-ssl-on-iis)
* [<span data-ttu-id="d071f-318">Prise en charge du navigateur OWASP HSTS</span><span class="sxs-lookup"><span data-stu-id="d071f-318">OWASP HSTS browser support</span></span>](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
