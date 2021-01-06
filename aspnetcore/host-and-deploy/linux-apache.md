---
title: Héberger ASP.NET Core sur Linux avec Apache
author: rick-anderson
description: Découvrez comment configurer Apache comme serveur proxy inverse sur CentOS pour rediriger le trafic HTTP vers une application web ASP.NET Core s’exécutant sur Kestrel.
monikerRange: '>= aspnetcore-2.1'
ms.author: shboyer
ms.custom: mvc
ms.date: 04/10/2020
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
uid: host-and-deploy/linux-apache
ms.openlocfilehash: 0bae3f888a1b7a3c2860b85754779189c636d86f
ms.sourcegitcommit: 3593c4efa707edeaaceffbfa544f99f41fc62535
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/04/2021
ms.locfileid: "93057698"
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="52322-103">Héberger ASP.NET Core sur Linux avec Apache</span><span class="sxs-lookup"><span data-stu-id="52322-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="52322-104">Par [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="52322-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="52322-105">À l’aide de ce guide, découvrez comment configurer [Apache](https://httpd.apache.org/) comme serveur proxy inverse sur [CentOS 7](https://www.centos.org/) pour rediriger le trafic HTTP vers une application web ASP.NET Core s’exécutant sur un serveur [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="52322-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="52322-106">[L’extension mod_proxy](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html) et les modules associés créent le proxy inverse du serveur.</span><span class="sxs-lookup"><span data-stu-id="52322-106">The [mod_proxy extension](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="52322-107">Prérequis</span><span class="sxs-lookup"><span data-stu-id="52322-107">Prerequisites</span></span>

* <span data-ttu-id="52322-108">Serveur exécutant CentOS 7 avec un compte d’utilisateur standard disposant du privilège sudo.</span><span class="sxs-lookup"><span data-stu-id="52322-108">Server running CentOS 7 with a standard user account with sudo privilege.</span></span>
* <span data-ttu-id="52322-109">Installez le runtime .NET Core sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="52322-109">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="52322-110">Visitez la [page Télécharger .net Core](https://dotnet.microsoft.com/download/dotnet-core).</span><span class="sxs-lookup"><span data-stu-id="52322-110">Visit the [Download .NET Core page](https://dotnet.microsoft.com/download/dotnet-core).</span></span>
   1. <span data-ttu-id="52322-111">Sélectionnez la dernière version non préliminaire de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="52322-111">Select the latest non-preview .NET Core version.</span></span>
   1. <span data-ttu-id="52322-112">Téléchargez le dernier Runtime non Preview dans le tableau sous **exécuter des applications-Runtime**.</span><span class="sxs-lookup"><span data-stu-id="52322-112">Download the latest non-preview runtime in the table under **Run apps - Runtime**.</span></span>
   1. <span data-ttu-id="52322-113">Sélectionnez le lien **des instructions du gestionnaire de package** Linux et suivez les instructions CentOS.</span><span class="sxs-lookup"><span data-stu-id="52322-113">Select the Linux **Package manager instructions** link and follow the CentOS instructions.</span></span>
* <span data-ttu-id="52322-114">Une application ASP.NET Core existante.</span><span class="sxs-lookup"><span data-stu-id="52322-114">An existing ASP.NET Core app.</span></span>

<span data-ttu-id="52322-115">À tout moment après la mise à niveau de l’infrastructure partagée, redémarrez le ASP.NET Core les applications hébergées par le serveur.</span><span class="sxs-lookup"><span data-stu-id="52322-115">At any point in the future after upgrading the shared framework, restart the ASP.NET Core apps hosted by the server.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="52322-116">Publier et copier sur l’application</span><span class="sxs-lookup"><span data-stu-id="52322-116">Publish and copy over the app</span></span>

<span data-ttu-id="52322-117">Configurez l’application pour un [déploiement dépendant du framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="52322-117">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="52322-118">Si l’application est exécutée localement et n’est pas configurée pour établir des connexions sécurisées (HTTPS), adoptez l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="52322-118">If the app is run locally and isn't configured to make secure connections (HTTPS), adopt either of the following approaches:</span></span>

* <span data-ttu-id="52322-119">Configurez l’application pour gérer les connexions locales sécurisées.</span><span class="sxs-lookup"><span data-stu-id="52322-119">Configure the app to handle secure local connections.</span></span> <span data-ttu-id="52322-120">Pour plus d’informations, consultez la section [Configuration HTTPS](#https-configuration).</span><span class="sxs-lookup"><span data-stu-id="52322-120">For more information, see the [HTTPS configuration](#https-configuration) section.</span></span>
* <span data-ttu-id="52322-121">Supprimez `https://localhost:5001` (le cas échéant) de la propriété `applicationUrl` dans le fichier *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="52322-121">Remove `https://localhost:5001` (if present) from the `applicationUrl` property in the *Properties/launchSettings.json* file.</span></span>

<span data-ttu-id="52322-122">Exécutez [dotnet publish](/dotnet/core/tools/dotnet-publish) à partir de l’environnement de développement pour empaqueter une application dans un répertoire (par exemple, *bin/Release/&lt;moniker_framework_target&gt;/publish*) exécutable sur le serveur :</span><span class="sxs-lookup"><span data-stu-id="52322-122">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```dotnetcli
dotnet publish --configuration Release
```

<span data-ttu-id="52322-123">L’application peut également être publiée en tant que [déploiement autonome](/dotnet/core/deploying/#self-contained-deployments-scd) si vous préférez ne pas gérer le runtime .NET Core sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="52322-123">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="52322-124">Copiez l’application ASP.NET Core sur le serveur à l’aide d’un outil qui s’intègre au flux de travail de l’organisation (par exemple, SCP ou SFTP).</span><span class="sxs-lookup"><span data-stu-id="52322-124">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="52322-125">Il est courant de placer les applications web sous le répertoire *var* (par exemple, *var/www/helloapp*).</span><span class="sxs-lookup"><span data-stu-id="52322-125">It's common to locate web apps under the *var* directory (for example, *var/www/helloapp*).</span></span>

> [!NOTE]
> <span data-ttu-id="52322-126">Dans un scénario de déploiement en production, un workflow d’intégration continue effectue le travail de publication de l’application et de copie des composants sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="52322-126">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

## <a name="configure-a-proxy-server"></a><span data-ttu-id="52322-127">Configurer un serveur proxy</span><span class="sxs-lookup"><span data-stu-id="52322-127">Configure a proxy server</span></span>

<span data-ttu-id="52322-128">Un proxy inverse est une configuration courante pour traiter les applications web dynamiques.</span><span class="sxs-lookup"><span data-stu-id="52322-128">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="52322-129">Le proxy inverse met fin à la requête HTTP et la transfère à l’application ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="52322-129">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="52322-130">Un serveur proxy est un serveur qui transfère les requêtes des clients à un autre serveur, au lieu de traiter celles-ci lui-même.</span><span class="sxs-lookup"><span data-stu-id="52322-130">A proxy server is one which forwards client requests to another server instead of fulfilling requests itself.</span></span> <span data-ttu-id="52322-131">Un proxy inverse transfère à une destination fixe, en général pour le compte de clients arbitraires.</span><span class="sxs-lookup"><span data-stu-id="52322-131">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="52322-132">Dans ce guide, Apache est configuré comme proxy inverse s’exécutant sur le même serveur où Kestrel traite l’application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="52322-132">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

<span data-ttu-id="52322-133">Étant donné que les demandes sont transférées par le proxy inverse, utilisez l' [intergiciel d’en-têtes transmis](xref:host-and-deploy/proxy-load-balancer) à partir du package [Microsoft. AspNetCore. HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) .</span><span class="sxs-lookup"><span data-stu-id="52322-133">Because requests are forwarded by reverse proxy, use the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="52322-134">Le middleware met à jour le `Request.Scheme`, à l’aide de l’en-tête `X-Forwarded-Proto`, afin que les URI de redirection et d’autres stratégies de sécurité fonctionnent correctement.</span><span class="sxs-lookup"><span data-stu-id="52322-134">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="52322-135">Tout composant qui dépend du schéma, tel que l’authentification, la génération de lien, les redirections et la géolocalisation, doit être placé après l’appel du middleware des en-têtes transférés.</span><span class="sxs-lookup"><span data-stu-id="52322-135">Any component that depends on the scheme, such as authentication, link generation, redirects, and geolocation, must be placed after invoking the Forwarded Headers Middleware.</span></span>

[!INCLUDE[](~/includes/ForwardedHeaders.md)]

<span data-ttu-id="52322-136">Appelez la <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> méthode en haut de `Startup.Configure` avant d’appeler un autre intergiciel.</span><span class="sxs-lookup"><span data-stu-id="52322-136">Invoke the <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> method at the top of `Startup.Configure` before calling other middleware.</span></span> <span data-ttu-id="52322-137">Configurez le middleware pour transférer les en-têtes `X-Forwarded-For` et `X-Forwarded-Proto` :</span><span class="sxs-lookup"><span data-stu-id="52322-137">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
// using Microsoft.AspNetCore.HttpOverrides;

app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

<span data-ttu-id="52322-138">Si aucune option <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> n’est spécifiée au middleware, les en-têtes par défaut à transférer sont `None`.</span><span class="sxs-lookup"><span data-stu-id="52322-138">If no <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="52322-139">Les serveurs proxy exécutés sur les adresses de bouclage (127.0.0.0/8, [ :: 1]), notamment l’adresse localhost standard (127.0.0.1), sont approuvés par défaut.</span><span class="sxs-lookup"><span data-stu-id="52322-139">Proxies running on loopback addresses (127.0.0.0/8, [::1]), including the standard localhost address (127.0.0.1), are trusted by default.</span></span> <span data-ttu-id="52322-140">Si d’autres proxys ou réseaux approuvés au sein de l’organisation gèrent les requêtes entre Internet et le serveur web, ajoutez-les à la liste des <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> ou des <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> avec <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>.</span><span class="sxs-lookup"><span data-stu-id="52322-140">If other trusted proxies or networks within the organization handle requests between the Internet and the web server, add them to the list of <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> or <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> with <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>.</span></span> <span data-ttu-id="52322-141">L’exemple suivant ajoute un serveur proxy approuvé avec l’adresse IP 10.0.0.100 au middleware des en-têtes transférés `KnownProxies` dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="52322-141">The following example adds a trusted proxy server at IP address 10.0.0.100 to the Forwarded Headers Middleware `KnownProxies` in `Startup.ConfigureServices`:</span></span>

```csharp
// using System.Net;

services.Configure<ForwardedHeadersOptions>(options =>
{
    options.KnownProxies.Add(IPAddress.Parse("10.0.0.100"));
});
```

<span data-ttu-id="52322-142">Pour plus d’informations, consultez <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="52322-142">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

### <a name="install-apache"></a><span data-ttu-id="52322-143">Installer Apache</span><span class="sxs-lookup"><span data-stu-id="52322-143">Install Apache</span></span>

<span data-ttu-id="52322-144">Mettez à jour les packages CentOS vers leur dernière version stable :</span><span class="sxs-lookup"><span data-stu-id="52322-144">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="52322-145">Installez le serveur web Apache sur CentOS avec une seule commande `yum` :</span><span class="sxs-lookup"><span data-stu-id="52322-145">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="52322-146">Exemple de sortie après exécution de la commande :</span><span class="sxs-lookup"><span data-stu-id="52322-146">Sample output after running the command:</span></span>

```bash
Downloading packages:
httpd-2.4.6-40.el7.centos.4.x86_64.rpm               | 2.7 MB  00:00:01
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
Installing : httpd-2.4.6-40.el7.centos.4.x86_64      1/1 
Verifying  : httpd-2.4.6-40.el7.centos.4.x86_64      1/1 

Installed:
httpd.x86_64 0:2.4.6-40.el7.centos.4

Complete!
```

> [!NOTE]
> <span data-ttu-id="52322-147">Dans cet exemple, la sortie correspond à httpd.86_64, car la version de CentOS 7 est en 64 bits.</span><span class="sxs-lookup"><span data-stu-id="52322-147">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="52322-148">Pour vérifier où Apache est installé, exécutez `whereis httpd` à partir d’une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="52322-148">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span>

### <a name="configure-apache"></a><span data-ttu-id="52322-149">Configurer Apache</span><span class="sxs-lookup"><span data-stu-id="52322-149">Configure Apache</span></span>

<span data-ttu-id="52322-150">Les fichiers de configuration pour Apache se trouvent dans le répertoire `/etc/httpd/conf.d/`.</span><span class="sxs-lookup"><span data-stu-id="52322-150">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="52322-151">Les fichiers avec l’extension *.conf* sont traités par ordre alphabétique en plus des fichiers de configuration des modules dans `/etc/httpd/conf.modules.d/`, qui contient tous les fichiers de configuration nécessaires au chargement des modules.</span><span class="sxs-lookup"><span data-stu-id="52322-151">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="52322-152">Créez un fichier de configuration nommé *helloapp.conf*, pour l’application :</span><span class="sxs-lookup"><span data-stu-id="52322-152">Create a configuration file, named *helloapp.conf*, for the app:</span></span>

```
<VirtualHost *:*>
    RequestHeader set "X-Forwarded-Proto" expr=%{REQUEST_SCHEME}
</VirtualHost>

<VirtualHost *:80>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ServerName www.example.com
    ServerAlias *.example.com
    ErrorLog ${APACHE_LOG_DIR}helloapp-error.log
    CustomLog ${APACHE_LOG_DIR}helloapp-access.log common
</VirtualHost>
```

<span data-ttu-id="52322-153">Le bloc `VirtualHost` peut apparaître plusieurs fois, dans un ou plusieurs fichiers sur un serveur.</span><span class="sxs-lookup"><span data-stu-id="52322-153">The `VirtualHost` block can appear multiple times, in one or more files on a server.</span></span> <span data-ttu-id="52322-154">Dans le fichier de configuration précédent, Apache accepte le trafic public sur le port 80.</span><span class="sxs-lookup"><span data-stu-id="52322-154">In the preceding configuration file, Apache accepts public traffic on port 80.</span></span> <span data-ttu-id="52322-155">Le domaine `www.example.com` est pris en charge, et l’alias `*.example.com` aboutit au même site web.</span><span class="sxs-lookup"><span data-stu-id="52322-155">The domain `www.example.com` is being served, and the `*.example.com` alias resolves to the same website.</span></span> <span data-ttu-id="52322-156">Pour plus d’informations, consultez [Name-based virtual host support](https://httpd.apache.org/docs/current/vhosts/name-based.html) (Prise en charge des hôtes virtuels par nom).</span><span class="sxs-lookup"><span data-stu-id="52322-156">See [Name-based virtual host support](https://httpd.apache.org/docs/current/vhosts/name-based.html) for more information.</span></span> <span data-ttu-id="52322-157">Les requêtes sont transmises par proxy à la racine vers le port 5000 du serveur sur 127.0.0.1.</span><span class="sxs-lookup"><span data-stu-id="52322-157">Requests are proxied at the root to port 5000 of the server at 127.0.0.1.</span></span> <span data-ttu-id="52322-158">Pour la communication bidirectionnelle, `ProxyPass` et `ProxyPassReverse` sont requis.</span><span class="sxs-lookup"><span data-stu-id="52322-158">For bi-directional communication, `ProxyPass` and `ProxyPassReverse` are required.</span></span> <span data-ttu-id="52322-159">Pour changer le port/l’adresse IP Kestrel, consultez [Kestrel : configuration du point de terminaison](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="52322-159">To change Kestrel's IP/port, see [Kestrel: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!WARNING]
> <span data-ttu-id="52322-160">La spécification d’une [directive ServerName](https://httpd.apache.org/docs/current/mod/core.html#servername) incorrecte dans le bloc **VirtualHost** expose votre application à des failles de sécurité.</span><span class="sxs-lookup"><span data-stu-id="52322-160">Failure to specify a proper [ServerName directive](https://httpd.apache.org/docs/current/mod/core.html#servername) in the **VirtualHost** block exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="52322-161">Une liaison générique de sous-domaine (par exemple, `*.example.com`) ne présente pas ce risque de sécurité si vous contrôlez le domaine parent en entier (par opposition à `*.com`, qui est vulnérable).</span><span class="sxs-lookup"><span data-stu-id="52322-161">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="52322-162">Consultez la [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="52322-162">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="52322-163">Vous pouvez configurer la journalisation par `VirtualHost` à l’aide des directives `ErrorLog` et `CustomLog`.</span><span class="sxs-lookup"><span data-stu-id="52322-163">Logging can be configured per `VirtualHost` using `ErrorLog` and `CustomLog` directives.</span></span> <span data-ttu-id="52322-164">`ErrorLog` est l’emplacement où le serveur journalise les erreurs, tandis que `CustomLog` définit le nom de fichier et le format du fichier journal.</span><span class="sxs-lookup"><span data-stu-id="52322-164">`ErrorLog` is the location where the server logs errors, and `CustomLog` sets the filename and format of log file.</span></span> <span data-ttu-id="52322-165">Dans ce cas, c’est l’endroit où les informations des requêtes sont journalisées.</span><span class="sxs-lookup"><span data-stu-id="52322-165">In this case, this is where request information is logged.</span></span> <span data-ttu-id="52322-166">Il y a une ligne pour chaque requête.</span><span class="sxs-lookup"><span data-stu-id="52322-166">There's one line for each request.</span></span>

<span data-ttu-id="52322-167">Enregistrez le fichier et testez la configuration.</span><span class="sxs-lookup"><span data-stu-id="52322-167">Save the file and test the configuration.</span></span> <span data-ttu-id="52322-168">Si tout réussit, la réponse doit être `Syntax [OK]`.</span><span class="sxs-lookup"><span data-stu-id="52322-168">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="52322-169">Redémarrez Apache :</span><span class="sxs-lookup"><span data-stu-id="52322-169">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitor-the-app"></a><span data-ttu-id="52322-170">Surveiller l’application</span><span class="sxs-lookup"><span data-stu-id="52322-170">Monitor the app</span></span>

<span data-ttu-id="52322-171">Apache est désormais configuré pour transférer les requêtes faites à `http://localhost:80` vers l’application ASP.NET Core s’exécutant sur Kestrel à l’adresse `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="52322-171">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="52322-172">Cependant, Apache n’est pas configuré pour gérer le processus Kestrel.</span><span class="sxs-lookup"><span data-stu-id="52322-172">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="52322-173">Utilisez *systemd* pour créer un fichier de service afin de démarrer et surveiller l’application web sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="52322-173">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="52322-174">*systemd* est un système d’initialisation qui fournit de nombreuses et puissantes fonctionnalités pour le démarrage, l’arrêt et la gestion des processus.</span><span class="sxs-lookup"><span data-stu-id="52322-174">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span>

### <a name="create-the-service-file"></a><span data-ttu-id="52322-175">Créer le fichier de service</span><span class="sxs-lookup"><span data-stu-id="52322-175">Create the service file</span></span>

<span data-ttu-id="52322-176">Créez le fichier de définition de service :</span><span class="sxs-lookup"><span data-stu-id="52322-176">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-helloapp.service
```

<span data-ttu-id="52322-177">Exemple de fichier de service pour l’application :</span><span class="sxs-lookup"><span data-stu-id="52322-177">An example service file for the app:</span></span>

```
[Unit]
Description=Example .NET Web API App running on CentOS 7

[Service]
WorkingDirectory=/var/www/helloapp
ExecStart=/usr/local/bin/dotnet /var/www/helloapp/helloapp.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
KillSignal=SIGINT
SyslogIdentifier=dotnet-example
User=apache
Environment=ASPNETCORE_ENVIRONMENT=Production 

[Install]
WantedBy=multi-user.target
```

<span data-ttu-id="52322-178">Dans l’exemple précédent, l’utilisateur qui gère le service est spécifié par l' `User` option.</span><span class="sxs-lookup"><span data-stu-id="52322-178">In the preceding example, the user that manages the service is specified by the `User` option.</span></span> <span data-ttu-id="52322-179">L’utilisateur ( `apache` ) doit exister et avoir la propriété correcte des fichiers de l’application.</span><span class="sxs-lookup"><span data-stu-id="52322-179">The user (`apache`) must exist and have proper ownership of the app's files.</span></span>

<span data-ttu-id="52322-180">Utilisez `TimeoutStopSec` pour configurer la durée d’attente de l’arrêt de l’application après la réception du signal d’interruption initial.</span><span class="sxs-lookup"><span data-stu-id="52322-180">Use `TimeoutStopSec` to configure the duration of time to wait for the app to shut down after it receives the initial interrupt signal.</span></span> <span data-ttu-id="52322-181">Si l’application ne s’arrête pas pendant cette période, le signal SIGKILL est émis pour mettre fin à l’application.</span><span class="sxs-lookup"><span data-stu-id="52322-181">If the app doesn't shut down in this period, SIGKILL is issued to terminate the app.</span></span> <span data-ttu-id="52322-182">Indiquez la valeur en secondes sans unité (par exemple, `150`), une valeur d’intervalle de temps (par exemple, `2min 30s`) ou `infinity` pour désactiver le délai d’attente.</span><span class="sxs-lookup"><span data-stu-id="52322-182">Provide the value as unitless seconds (for example, `150`), a time span value (for example, `2min 30s`), or `infinity` to disable the timeout.</span></span> <span data-ttu-id="52322-183">`TimeoutStopSec` prend la valeur par défaut de `DefaultTimeoutStopSec` dans le fichier de configuration du gestionnaire (*systemd-system.conf*, *system.conf.d*, *systemd-user.conf*, *user.conf.d*).</span><span class="sxs-lookup"><span data-stu-id="52322-183">`TimeoutStopSec` defaults to the value of `DefaultTimeoutStopSec` in the manager configuration file (*systemd-system.conf*, *system.conf.d*, *systemd-user.conf*, *user.conf.d*).</span></span> <span data-ttu-id="52322-184">Le délai d’expiration par défaut pour la plupart des distributions est de 90 secondes.</span><span class="sxs-lookup"><span data-stu-id="52322-184">The default timeout for most distributions is 90 seconds.</span></span>

```
# The default value is 90 seconds for most distributions.
TimeoutStopSec=90
```

<span data-ttu-id="52322-185">Certaines valeurs (par exemple, les chaînes de connexion SQL) doivent être placées dans une séquence d’échappement afin que les fournisseurs de configuration puissent lire les variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="52322-185">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="52322-186">Utilisez la commande suivante pour générer une valeur correctement placée dans une séquence d’échappement en vue de son utilisation dans le fichier de configuration :</span><span class="sxs-lookup"><span data-stu-id="52322-186">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>

```console
systemd-escape "<value-to-escape>"
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="52322-187">Les séparateurs `:` (signe deux-points) ne sont pas pris en charge dans les noms de variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="52322-187">Colon (`:`) separators aren't supported in environment variable names.</span></span> <span data-ttu-id="52322-188">Utilisez un double trait de soulignement (`__`) à la place d’un signe deux-points.</span><span class="sxs-lookup"><span data-stu-id="52322-188">Use a double underscore (`__`) in place of a colon.</span></span> <span data-ttu-id="52322-189">Le [fournisseur de configuration de variables d’environnement](xref:fundamentals/configuration/index#environment-variables-configuration-provider) convertit les doubles traits de soulignement en signes deux-points quand les variables d’environnement sont lues dans la configuration.</span><span class="sxs-lookup"><span data-stu-id="52322-189">The [Environment Variables configuration provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider) converts double-underscores into colons when environment variables are read into configuration.</span></span> <span data-ttu-id="52322-190">Dans l’exemple suivant, la clé de chaîne de connexion `ConnectionStrings:DefaultConnection` est définie dans le fichier de définition de service en tant que `ConnectionStrings__DefaultConnection` :</span><span class="sxs-lookup"><span data-stu-id="52322-190">In the following example, the connection string key `ConnectionStrings:DefaultConnection` is set into the service definition file as `ConnectionStrings__DefaultConnection`:</span></span>

::: moniker-end
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="52322-191">Les séparateurs `:` (signe deux-points) ne sont pas pris en charge dans les noms de variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="52322-191">Colon (`:`) separators aren't supported in environment variable names.</span></span> <span data-ttu-id="52322-192">Utilisez un double trait de soulignement (`__`) à la place d’un signe deux-points.</span><span class="sxs-lookup"><span data-stu-id="52322-192">Use a double underscore (`__`) in place of a colon.</span></span> <span data-ttu-id="52322-193">Le [fournisseur de configuration de variables d’environnement](xref:fundamentals/configuration/index#environment-variables) convertit les doubles traits de soulignement en signes deux-points quand les variables d’environnement sont lues dans la configuration.</span><span class="sxs-lookup"><span data-stu-id="52322-193">The [Environment Variables configuration provider](xref:fundamentals/configuration/index#environment-variables) converts double-underscores into colons when environment variables are read into configuration.</span></span> <span data-ttu-id="52322-194">Dans l’exemple suivant, la clé de chaîne de connexion `ConnectionStrings:DefaultConnection` est définie dans le fichier de définition de service en tant que `ConnectionStrings__DefaultConnection` :</span><span class="sxs-lookup"><span data-stu-id="52322-194">In the following example, the connection string key `ConnectionStrings:DefaultConnection` is set into the service definition file as `ConnectionStrings__DefaultConnection`:</span></span>

::: moniker-end

```
Environment=ConnectionStrings__DefaultConnection={Connection String}
```

<span data-ttu-id="52322-195">Enregistrez le fichier et activez le service :</span><span class="sxs-lookup"><span data-stu-id="52322-195">Save the file and enable the service:</span></span>

```bash
sudo systemctl enable kestrel-helloapp.service
```

<span data-ttu-id="52322-196">Démarrez le service et vérifiez qu’il s’exécute :</span><span class="sxs-lookup"><span data-stu-id="52322-196">Start the service and verify that it's running:</span></span>

```bash
sudo systemctl start kestrel-helloapp.service
sudo systemctl status kestrel-helloapp.service

◝ kestrel-helloapp.service - Example .NET Web API App running on CentOS 7
    Loaded: loaded (/etc/systemd/system/kestrel-helloapp.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-helloapp.service
            └─9021 /usr/local/bin/dotnet /var/www/helloapp/helloapp.dll
```

<span data-ttu-id="52322-197">Le proxy inverse étant configuré et Kestrel étant géré via *systemd*, l’application web est maintenant entièrement configurée et accessible à partir d’un navigateur sur la machine locale à l’adresse `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="52322-197">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="52322-198">Comme l’indiquent les en-têtes de réponse, l’en-tête **Server** montre que l’application ASP.NET Core est traitée par Kestrel :</span><span class="sxs-lookup"><span data-stu-id="52322-198">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="view-logs"></a><span data-ttu-id="52322-199">Afficher les journaux d’activité</span><span class="sxs-lookup"><span data-stu-id="52322-199">View logs</span></span>

<span data-ttu-id="52322-200">Puisque l’application web utilisant Kestrel est gérée à l’aide de *systemd*, les processus et les événements sont enregistrés dans un journal centralisé.</span><span class="sxs-lookup"><span data-stu-id="52322-200">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="52322-201">Cependant, ce journal comprend les entrées de tous les services et processus gérés par *systemd*.</span><span class="sxs-lookup"><span data-stu-id="52322-201">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="52322-202">Pour afficher les éléments propres à `kestrel-helloapp.service`, utilisez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="52322-202">To view the `kestrel-helloapp.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service
```

<span data-ttu-id="52322-203">Pour le filtrage du temps, spécifiez les options appropriées dans la commande.</span><span class="sxs-lookup"><span data-stu-id="52322-203">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="52322-204">Par exemple, utilisez `--since today` pour filtrer en fonction du jour actuel ou `--until 1 hour ago` pour voir les entrées de l’heure précédente.</span><span class="sxs-lookup"><span data-stu-id="52322-204">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="52322-205">Pour plus d’informations, consultez la [page man pour journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span><span class="sxs-lookup"><span data-stu-id="52322-205">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="data-protection"></a><span data-ttu-id="52322-206">Protection de données</span><span class="sxs-lookup"><span data-stu-id="52322-206">Data protection</span></span>

<span data-ttu-id="52322-207">La [pile de protection des données ASP.net Core](xref:security/data-protection/introduction) est utilisée par plusieurs ASP.net Core [intergiciels](xref:fundamentals/middleware/index), y compris l’intergiciel (middleware) d’authentification (par exemple, cookie intergiciel) et les protections CSRF (cross-site request falsification).</span><span class="sxs-lookup"><span data-stu-id="52322-207">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including authentication middleware (for example, cookie middleware) and cross-site request forgery (CSRF) protections.</span></span> <span data-ttu-id="52322-208">Même si les API de protection des données ne sont pas appelées par le code de l’utilisateur, la protection des données doit être configurée pour créer un [magasin de clés](xref:security/data-protection/implementation/key-management) de chiffrement persistantes.</span><span class="sxs-lookup"><span data-stu-id="52322-208">Even if Data Protection APIs aren't called by user code, data protection should be configured to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="52322-209">Si la protection des données n’est pas configurée, les clés sont conservées en mémoire et ignorées au redémarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="52322-209">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="52322-210">Si le Key Ring est stocké en mémoire, au redémarrage de l’application :</span><span class="sxs-lookup"><span data-stu-id="52322-210">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="52322-211">Tous les cookie jetons d’authentification de base sont invalidés.</span><span class="sxs-lookup"><span data-stu-id="52322-211">All cookie-based authentication tokens are invalidated.</span></span>
* <span data-ttu-id="52322-212">Les utilisateurs doivent se reconnecter pour envoyer leur prochaine demande.</span><span class="sxs-lookup"><span data-stu-id="52322-212">Users are required to sign in again on their next request.</span></span>
* <span data-ttu-id="52322-213">toutes les données protégées par le Key Ring ne peuvent plus être déchiffrées.</span><span class="sxs-lookup"><span data-stu-id="52322-213">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="52322-214">Cela peut inclure des [jetons CSRF](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) et des ASP.NET Core les de la [MVC cookie ](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="52322-214">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="52322-215">Pour configurer la protection des données de façon à conserver et chiffrer le porte-clés (Key Ring), consultez :</span><span class="sxs-lookup"><span data-stu-id="52322-215">To configure data protection to persist and encrypt the key ring, see:</span></span>

* <xref:security/data-protection/implementation/key-storage-providers>
* <xref:security/data-protection/implementation/key-encryption-at-rest>

## <a name="secure-the-app"></a><span data-ttu-id="52322-216">Sécuriser l’application</span><span class="sxs-lookup"><span data-stu-id="52322-216">Secure the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="52322-217">Configurer le pare-feu</span><span class="sxs-lookup"><span data-stu-id="52322-217">Configure firewall</span></span>

<span data-ttu-id="52322-218">*Firewalld* est un démon dynamique pour gérer le pare-feu avec prise en charge des zones réseau.</span><span class="sxs-lookup"><span data-stu-id="52322-218">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="52322-219">Les ports et le filtrage des paquets peuvent toujours être gérés par iptables.</span><span class="sxs-lookup"><span data-stu-id="52322-219">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="52322-220">*Firewalld* doit être installé par défaut.</span><span class="sxs-lookup"><span data-stu-id="52322-220">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="52322-221">Vous pouvez utiliser `yum` pour installer le package ou vérifier qu’il est installé.</span><span class="sxs-lookup"><span data-stu-id="52322-221">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="52322-222">Utilisez `firewalld` pour ouvrir seulement les ports nécessaires à l’application.</span><span class="sxs-lookup"><span data-stu-id="52322-222">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="52322-223">Dans ce cas, les ports 80 et 443 sont utilisés.</span><span class="sxs-lookup"><span data-stu-id="52322-223">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="52322-224">Les commandes suivantes définissent l’ouverture des ports 80 et 443 de façon permanente :</span><span class="sxs-lookup"><span data-stu-id="52322-224">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="52322-225">Rechargez les paramètres du pare-feu.</span><span class="sxs-lookup"><span data-stu-id="52322-225">Reload the firewall settings.</span></span> <span data-ttu-id="52322-226">Vérifiez les services et les ports disponibles dans la zone par défaut.</span><span class="sxs-lookup"><span data-stu-id="52322-226">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="52322-227">Vous voyez les options disponibles en examinant le résultat de `firewall-cmd -h`.</span><span class="sxs-lookup"><span data-stu-id="52322-227">Options are available by inspecting `firewall-cmd -h`.</span></span>

```bash
sudo firewall-cmd --reload
sudo firewall-cmd --list-all
```

```bash
public (default, active)
interfaces: eth0
sources: 
services: dhcpv6-client
ports: 443/tcp 80/tcp
masquerade: no
forward-ports: 
icmp-blocks: 
rich rules: 
```

### <a name="https-configuration"></a><span data-ttu-id="52322-228">Configuration HTTPS</span><span class="sxs-lookup"><span data-stu-id="52322-228">HTTPS configuration</span></span>

<span data-ttu-id="52322-229">**Configurer l’application pour les connexions locales sécurisées (HTTPS)**</span><span class="sxs-lookup"><span data-stu-id="52322-229">**Configure the app for secure (HTTPS) local connections**</span></span>

<span data-ttu-id="52322-230">La commande [dotnet run](/dotnet/core/tools/dotnet-run) utilise le fichier *Properties/launchSettings.json* de l’application, qui configure l’application pour l’écoute sur les URL fournies par la propriété `applicationUrl` (par exemple `https://localhost:5001;http://localhost:5000`).</span><span class="sxs-lookup"><span data-stu-id="52322-230">The [dotnet run](/dotnet/core/tools/dotnet-run) command uses the app's *Properties/launchSettings.json* file, which configures the app to listen on the URLs provided by the `applicationUrl` property (for example, `https://localhost:5001;http://localhost:5000`).</span></span>

<span data-ttu-id="52322-231">Configurez l’application pour utiliser un certificat en développement pour la commande `dotnet run` ou l’environnement de développement (F5 ou Ctrl+F5 dans Visual Studio Code) en adoptant l’une de ces approches :</span><span class="sxs-lookup"><span data-stu-id="52322-231">Configure the app to use a certificate in development for the `dotnet run` command or development environment (F5 or Ctrl+F5 in Visual Studio Code) using one of the following approaches:</span></span>

* <span data-ttu-id="52322-232">[Remplacer le certificat par défaut dans la configuration](xref:fundamentals/servers/kestrel#configuration) (*recommandé*)</span><span class="sxs-lookup"><span data-stu-id="52322-232">[Replace the default certificate from configuration](xref:fundamentals/servers/kestrel#configuration) (*Recommended*)</span></span>
* [<span data-ttu-id="52322-233">KestrelServerOptions.ConfigureHttpsDefaults</span><span class="sxs-lookup"><span data-stu-id="52322-233">KestrelServerOptions.ConfigureHttpsDefaults</span></span>](xref:fundamentals/servers/kestrel#configurehttpsdefaultsactionhttpsconnectionadapteroptions)

<span data-ttu-id="52322-234">**Configurer le proxy inverse pour les connexions clientes sécurisées (HTTPS)**</span><span class="sxs-lookup"><span data-stu-id="52322-234">**Configure the reverse proxy for secure (HTTPS) client connections**</span></span>

<span data-ttu-id="52322-235">Pour configurer Apache pour HTTPS, vous utilisez le module *mod_ssl*.</span><span class="sxs-lookup"><span data-stu-id="52322-235">To configure Apache for HTTPS, the *mod_ssl* module is used.</span></span> <span data-ttu-id="52322-236">Le module *mod_ssl* a été installé parallèlement au module *httpd*.</span><span class="sxs-lookup"><span data-stu-id="52322-236">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="52322-237">S’il n’a pas été installé, utilisez `yum` pour l’ajouter à la configuration.</span><span class="sxs-lookup"><span data-stu-id="52322-237">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```

<span data-ttu-id="52322-238">Pour mettre en œuvre HTTPS, installez le module `mod_rewrite` afin de permettre la réécriture d’URL :</span><span class="sxs-lookup"><span data-stu-id="52322-238">To enforce HTTPS, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="52322-239">Modifiez le fichier *helloapp.conf* pour permettre la réécriture d’URL et sécuriser les communications sur le port 443 :</span><span class="sxs-lookup"><span data-stu-id="52322-239">Modify the *helloapp.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

```
<VirtualHost *:*>
    RequestHeader set "X-Forwarded-Proto" expr=%{REQUEST_SCHEME}
</VirtualHost>

<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/$1 [R,L]
</VirtualHost>

<VirtualHost *:443>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ErrorLog /var/log/httpd/helloapp-error.log
    CustomLog /var/log/httpd/helloapp-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

> [!NOTE]
> <span data-ttu-id="52322-240">Cet exemple utilise un certificat généré localement.</span><span class="sxs-lookup"><span data-stu-id="52322-240">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="52322-241">**SSLCertificateFile** doit être le fichier de certificat principal pour le nom de domaine.</span><span class="sxs-lookup"><span data-stu-id="52322-241">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="52322-242">**SSLCertificateKeyFile** doit être le fichier de clé généré quand la demande de signature de certificat est créée.</span><span class="sxs-lookup"><span data-stu-id="52322-242">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="52322-243">**SSLCertificateChainFile** doit être le fichier de certificat intermédiaire (le cas échéant) qui a été fourni par l’autorité de certification.</span><span class="sxs-lookup"><span data-stu-id="52322-243">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="52322-244">Enregistrez le fichier et testez la configuration :</span><span class="sxs-lookup"><span data-stu-id="52322-244">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="52322-245">Redémarrez Apache :</span><span class="sxs-lookup"><span data-stu-id="52322-245">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="52322-246">Autres suggestions pour Apache</span><span class="sxs-lookup"><span data-stu-id="52322-246">Additional Apache suggestions</span></span>

### <a name="restart-apps-with-shared-framework-updates"></a><span data-ttu-id="52322-247">Redémarrer des applications avec des mises à jour de Framework partagé</span><span class="sxs-lookup"><span data-stu-id="52322-247">Restart apps with shared framework updates</span></span>

<span data-ttu-id="52322-248">Après la mise à niveau de l’infrastructure partagée sur le serveur, redémarrez le ASP.NET Core les applications hébergées par le serveur.</span><span class="sxs-lookup"><span data-stu-id="52322-248">After upgrading the shared framework on the server, restart the ASP.NET Core apps hosted by the server.</span></span>

### <a name="additional-headers"></a><span data-ttu-id="52322-249">En-têtes supplémentaires</span><span class="sxs-lookup"><span data-stu-id="52322-249">Additional headers</span></span>

<span data-ttu-id="52322-250">Afin d’assurer une protection contre les attaques malveillantes, vous devez modifier ou ajouter quelques en-têtes.</span><span class="sxs-lookup"><span data-stu-id="52322-250">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="52322-251">Vérifiez que le module `mod_headers` est installé :</span><span class="sxs-lookup"><span data-stu-id="52322-251">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="52322-252">Sécuriser Apache contre les attaques par détournement de clic</span><span class="sxs-lookup"><span data-stu-id="52322-252">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="52322-253">Le [détournement de clic](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), également appelé *UI redress attack*, est une attaque malveillante qui amène le visiteur d’un site web à cliquer sur un lien ou un bouton sur une page différente de celle qu’il est en train de visiter.</span><span class="sxs-lookup"><span data-stu-id="52322-253">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="52322-254">Utilisez `X-FRAME-OPTIONS` pour sécuriser le site.</span><span class="sxs-lookup"><span data-stu-id="52322-254">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="52322-255">Pour atténuer les attaques par détournement de clic :</span><span class="sxs-lookup"><span data-stu-id="52322-255">To mitigate clickjacking attacks:</span></span>

1. <span data-ttu-id="52322-256">Modifiez le fichier *httpd.conf* :</span><span class="sxs-lookup"><span data-stu-id="52322-256">Edit the *httpd.conf* file:</span></span>

   ```bash
   sudo nano /etc/httpd/conf/httpd.conf
   ```

   <span data-ttu-id="52322-257">Ajoutez la ligne `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span><span class="sxs-lookup"><span data-stu-id="52322-257">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span>
1. <span data-ttu-id="52322-258">Enregistrez le fichier .</span><span class="sxs-lookup"><span data-stu-id="52322-258">Save the file.</span></span>
1. <span data-ttu-id="52322-259">Redémarrez Apache.</span><span class="sxs-lookup"><span data-stu-id="52322-259">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="52322-260">Détection de type MIME</span><span class="sxs-lookup"><span data-stu-id="52322-260">MIME-type sniffing</span></span>

<span data-ttu-id="52322-261">L’en-tête `X-Content-Type-Options` empêche Internet Explorer d’effectuer une *détection de données MIME* (détermination du `Content-Type` d’un fichier à partir de son contenu).</span><span class="sxs-lookup"><span data-stu-id="52322-261">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determining a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="52322-262">Si le serveur définit l’en-tête `Content-Type` sur `text/html` et que l’option `nosniff` est définie, Internet Explorer restitue le contenu en tant que `text/html`, quel que soit le contenu du fichier.</span><span class="sxs-lookup"><span data-stu-id="52322-262">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="52322-263">Modifiez le fichier *httpd.conf* :</span><span class="sxs-lookup"><span data-stu-id="52322-263">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="52322-264">Ajoutez la ligne `Header set X-Content-Type-Options "nosniff"`.</span><span class="sxs-lookup"><span data-stu-id="52322-264">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="52322-265">Enregistrez le fichier .</span><span class="sxs-lookup"><span data-stu-id="52322-265">Save the file.</span></span> <span data-ttu-id="52322-266">Redémarrez Apache.</span><span class="sxs-lookup"><span data-stu-id="52322-266">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="52322-267">Équilibrage de la charge.</span><span class="sxs-lookup"><span data-stu-id="52322-267">Load Balancing</span></span>

<span data-ttu-id="52322-268">Cet exemple montre comment installer et configurer Apache sur CentOS 7 et Kestrel sur la même machine de l’instance.</span><span class="sxs-lookup"><span data-stu-id="52322-268">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="52322-269">Afin de multiplier les instances en cas de défaillance, l’utilisation de *mod_proxy_balancer* et la modification du **VirtualHost** permettent de gérer plusieurs instances des applications web derrière le serveur proxy Apache.</span><span class="sxs-lookup"><span data-stu-id="52322-269">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing multiple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="52322-270">Dans le fichier de configuration illustré ci-dessous, une instance supplémentaire de `helloapp` est configurée pour s’exécuter sur le port 5001.</span><span class="sxs-lookup"><span data-stu-id="52322-270">In the configuration file shown below, an additional instance of the `helloapp` is set up to run on port 5001.</span></span> <span data-ttu-id="52322-271">La section *Proxy* est définie avec une configuration d’équilibreur qui comprend deux membres pour équilibrer la charge selon la méthode *byrequests*.</span><span class="sxs-lookup"><span data-stu-id="52322-271">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

```
<VirtualHost *:*>
    RequestHeader set "X-Forwarded-Proto" expr=%{REQUEST_SCHEME}
</VirtualHost>

<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/$1 [R,L]
</VirtualHost>

<VirtualHost *:443>
    ProxyPass / balancer://mycluster/ 

    ProxyPassReverse / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5001/

    <Proxy balancer://mycluster>
        BalancerMember http://127.0.0.1:5000
        BalancerMember http://127.0.0.1:5001 
        ProxySet lbmethod=byrequests
    </Proxy>

    <Location />
        SetHandler balancer
    </Location>
    ErrorLog /var/log/httpd/helloapp-error.log
    CustomLog /var/log/httpd/helloapp-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

### <a name="rate-limits"></a><span data-ttu-id="52322-272">Limites du débit</span><span class="sxs-lookup"><span data-stu-id="52322-272">Rate Limits</span></span>

<span data-ttu-id="52322-273">Vous pouvez utiliser *mod_ratelimit*, qui est inclus dans le module *httpd*, pour limiter la bande passante des clients :</span><span class="sxs-lookup"><span data-stu-id="52322-273">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```

<span data-ttu-id="52322-274">L’exemple de fichier limite la bande passante à 600 Ko/s sous l’emplacement racine :</span><span class="sxs-lookup"><span data-stu-id="52322-274">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```

### <a name="long-request-header-fields"></a><span data-ttu-id="52322-275">Longs champs d'en-tête de demande</span><span class="sxs-lookup"><span data-stu-id="52322-275">Long request header fields</span></span>

<span data-ttu-id="52322-276">Les paramètres par défaut du serveur proxy limitent généralement les champs d’en-tête de demande à 8 190 octets.</span><span class="sxs-lookup"><span data-stu-id="52322-276">Proxy server default settings typically limit request header fields to 8,190 bytes.</span></span> <span data-ttu-id="52322-277">Une application peut nécessiter des champs plus longs que la valeur par défaut (par exemple, les applications qui utilisent [Azure Active Directory](https://azure.microsoft.com/services/active-directory/)).</span><span class="sxs-lookup"><span data-stu-id="52322-277">An app may require fields longer than the default (for example, apps that use [Azure Active Directory](https://azure.microsoft.com/services/active-directory/)).</span></span> <span data-ttu-id="52322-278">Si des champs plus longs sont requis, la directive [LimitRequestFieldSize](https://httpd.apache.org/docs/2.4/mod/core.html#LimitRequestFieldSize) du serveur proxy doit être ajustée.</span><span class="sxs-lookup"><span data-stu-id="52322-278">If longer fields are required, the proxy server's [LimitRequestFieldSize](https://httpd.apache.org/docs/2.4/mod/core.html#LimitRequestFieldSize) directive requires adjustment.</span></span> <span data-ttu-id="52322-279">La valeur à appliquer dépend du scénario.</span><span class="sxs-lookup"><span data-stu-id="52322-279">The value to apply depends on the scenario.</span></span> <span data-ttu-id="52322-280">Pour plus d'informations, voir la documentation du serveur.</span><span class="sxs-lookup"><span data-stu-id="52322-280">For more information, see your server's documentation.</span></span>

> [!WARNING]
> <span data-ttu-id="52322-281">N’augmentez pas la valeur par défaut de `LimitRequestFieldSize` à moins que ce ne soit nécessaire.</span><span class="sxs-lookup"><span data-stu-id="52322-281">Don't increase the default value of `LimitRequestFieldSize` unless necessary.</span></span> <span data-ttu-id="52322-282">Son augmentation augmente le risque de dépassement de mémoire tampon (dépassement de capacité) et d’attaques par déni de service (DoS) par des utilisateurs malveillants.</span><span class="sxs-lookup"><span data-stu-id="52322-282">Increasing the value increases the risk of buffer overrun (overflow) and Denial of Service (DoS) attacks by malicious users.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="52322-283">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="52322-283">Additional resources</span></span>

* [<span data-ttu-id="52322-284">Prérequis pour .NET Core sur Linux</span><span class="sxs-lookup"><span data-stu-id="52322-284">Prerequisites for .NET Core on Linux</span></span>](/dotnet/core/linux-prerequisites)
* <xref:test/troubleshoot>
* <xref:host-and-deploy/proxy-load-balancer>
