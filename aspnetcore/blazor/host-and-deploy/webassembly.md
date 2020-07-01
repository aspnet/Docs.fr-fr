---
title: Héberger et déployer des ASP.NET CoreBlazor WebAssembly
author: guardrex
description: Découvrez comment héberger et déployer une Blazor application à l’aide de ASP.net Core, de réseaux de distribution de contenu (CDN), de serveurs de fichiers et de pages github.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/07/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/host-and-deploy/webassembly
ms.openlocfilehash: 2b100ba029c08e0ce68d208df761f22a712fbbfd
ms.sourcegitcommit: 99c784a873b62fbd97a73c5c07f4fe7a7f5db638
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85503511"
---
# <a name="host-and-deploy-aspnet-core-blazor-webassembly"></a>Héberger et déployer des ASP.NET CoreBlazor WebAssembly

Par [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), [Daniel Roth](https://github.com/danroth27), [Ben Adams](https://twitter.com/ben_a_adams)et [safia Abdalla](https://safia.rocks)

Avec le [ Blazor WebAssembly modèle d’hébergement](xref:blazor/hosting-models#blazor-webassembly):

* L' Blazor application, ses dépendances et le Runtime .net sont téléchargés dans le navigateur en parallèle.
* L’application est exécutée directement sur le thread d’interface utilisateur du navigateur.

Les stratégies de déploiement suivantes sont prises en charge :

* L' Blazor application est traitée par une application ASP.net core. Cette stratégie est abordée dans la section [Déploiement hébergé avec ASP.NET Core](#hosted-deployment-with-aspnet-core).
* L' Blazor application est placée sur un service ou un serveur Web d’hébergement statique, où .net n’est pas utilisé pour traiter l' Blazor application. Cette stratégie est traitée dans la section [Déploiement autonome](#standalone-deployment) , qui comprend des informations sur l’hébergement d’une Blazor WebAssembly application en tant que sous-application IIS.

## <a name="compression"></a>Compression

Quand une Blazor WebAssembly application est publiée, la sortie est compressée de manière statique lors de la publication afin de réduire la taille de l’application et de supprimer la surcharge liée à la compression du Runtime. Les algorithmes de compression suivants sont utilisés :

* [Brotli](https://tools.ietf.org/html/rfc7932) (niveau le plus élevé)
* [Gzip](https://tools.ietf.org/html/rfc1952)

Blazors’appuie sur l’hôte pour servir les fichiers compressés appropriés. Lors de l’utilisation d’un projet hébergé ASP.NET Core, le projet hôte peut effectuer la négociation de contenu et traiter les fichiers compressés statiquement. Lors de l’hébergement d’une Blazor WebAssembly application autonome, un travail supplémentaire peut être nécessaire pour s’assurer que les fichiers compressés statiquement sont pris en charge :

* Pour `web.config` la configuration de la compression IIS, consultez la section [IIS : Brotli et compression gzip](#brotli-and-gzip-compression) . 
* Lors de l’hébergement sur des solutions d’hébergement statiques qui ne prennent pas en charge la négociation de contenu de fichier compressée statiquement, telles que les pages GitHub, envisagez de configurer l’application pour extraire et décoder les fichiers compressés Brotli :

  * Référencez le décodeur Brotli à partir du [référentiel GitHub Google/Brotli](https://github.com/google/brotli/) dans l’application.
  * Mettez à jour l’application pour utiliser le décodeur. Remplacez le balisage `<body>` dans la balise de fermeture par `wwwroot/index.html` ce qui suit :
  
    ```html
    <script src="brotli.decode.min.js"></script>
    <script src="_framework/blazor.webassembly.js" autostart="false"></script>
    <script>
    Blazor.start({
      loadBootResource: function (type, name, defaultUri, integrity) {
        if (type !== 'dotnetjs' && location.hostname !== 'localhost') {
          return (async function () {
            const response = await fetch(defaultUri + '.br', { cache: 'no-cache' });
            if (!response.ok) {
              throw new Error(response.statusText);
            }
            const originalResponseBuffer = await response.arrayBuffer();
            const originalResponseArray = new Int8Array(originalResponseBuffer);
            const decompressedResponseArray = BrotliDecode(originalResponseArray);
            const contentType = type === 
              'dotnetwasm' ? 'application/wasm' : 'application/octet-stream';
            return new Response(decompressedResponseArray, 
              { headers: { 'content-type': contentType } });
          })();
        }
      }
    });
    </script>
    ```
 
Pour désactiver la compression, ajoutez la `BlazorEnableCompression` propriété MSBuild au fichier projet de l’application et définissez la valeur sur `false` :

```xml
<PropertyGroup>
  <BlazorEnableCompression>false</BlazorEnableCompression>
</PropertyGroup>
```

## <a name="rewrite-urls-for-correct-routing"></a>Réécriture d’URL pour un routage correct

Le routage des requêtes pour les composants de page dans une Blazor WebAssembly application n’est pas aussi simple que les demandes de routage dans une Blazor Server application hébergée. Prenons l’exemple d’une Blazor WebAssembly application avec deux composants :

* `Main.razor`: Charge à la racine de l’application et contient un lien vers le `About` composant ( `href="About"` ).
* `About.razor`: `About` composant.

Quand le document par défaut de l’application est demandé à l’aide de la barre d’adresses du navigateur (par exemple, `https://www.contoso.com/`) :

1. Le navigateur effectue une requête.
1. La page par défaut est retournée, qui est généralement `index.html` .
1. `index.html`amorce l’application.
1. Blazorle routeur est chargé et le Razor `Main` composant est rendu.

Dans la page principale, le fait de sélectionner le lien vers le `About` composant fonctionne sur le client car le Blazor routeur empêche le navigateur d’effectuer une requête sur Internet pour `www.contoso.com` `About` et sert le `About` composant rendu lui-même. Toutes les demandes de points de terminaison internes *au sein de l' Blazor WebAssembly application* fonctionnent de la même façon : les demandes ne déclenchent pas de requêtes basées sur un navigateur vers des ressources hébergées sur le serveur sur Internet. Le routeur gère les requêtes en interne.

Si une requête pour `www.contoso.com/About` est effectuée à l’aide de la barre d’adresses du navigateur, elle échoue. Comme cette ressource n’existe pas sur l’hôte Internet de l’application, une réponse *404 – Non trouvé* est retournée.

Étant donné que les navigateurs effectuent des demandes aux hôtes basés sur Internet pour les pages côté client, les serveurs Web et les services d’hébergement doivent réécrire toutes les demandes de ressources qui ne sont pas physiquement sur le serveur sur la `index.html` page. Lorsque `index.html` est retourné, le routeur de l’application Blazor prend le relais et répond avec la ressource correcte.

Lors du déploiement sur un serveur IIS, vous pouvez utiliser le module de réécriture d’URL avec le fichier publié de l’application `web.config` . Pour plus d’informations, consultez la section [IIS](#iis) .

## <a name="hosted-deployment-with-aspnet-core"></a>Déploiement hébergé avec ASP.NET Core

Un *déploiement hébergé* sert l' Blazor WebAssembly application aux navigateurs à partir d’une [application ASP.net Core](xref:index) qui s’exécute sur un serveur Web.

L’application cliente Blazor WebAssembly est publiée dans le `/bin/Release/{TARGET FRAMEWORK}/publish/wwwroot` dossier de l’application serveur, ainsi que les autres ressources Web statiques de l’application serveur. Les deux applications sont déployées ensemble. Un serveur web capable d’héberger une application ASP.NET Core est nécessaire. Pour un déploiement hébergé, Visual Studio comprend le modèle de projet d' ** Blazor WebAssembly application** ( `blazorwasm` modèle lors de l’utilisation de la [`dotnet new`](/dotnet/core/tools/dotnet-new) commande) avec l' **`Hosted`** option sélectionnée (lors de l' `-ho|--hosted` utilisation de la `dotnet new` commande).

Pour plus d’informations sur l’hébergement et le déploiement d’applications ASP.NET Core, consultez <xref:host-and-deploy/index>.

Pour plus d’informations concernant le déploiement sur Azure App Service, consultez <xref:tutorials/publish-to-azure-webapp-using-vs>.

## <a name="standalone-deployment"></a>Déploiement autonome

Un *Déploiement autonome* sert l' Blazor WebAssembly application sous la forme d’un ensemble de fichiers statiques demandés directement par les clients. Tout serveur de fichiers statique peut traiter l' Blazor application.

Les ressources de déploiement autonomes sont publiées dans le `/bin/Release/{TARGET FRAMEWORK}/publish/wwwroot` dossier.

### <a name="azure-app-service"></a>Azure App Service

Blazor WebAssemblyles applications peuvent être déployées sur Azure App services sur Windows, qui héberge l’application sur [IIS](#iis).

Le déploiement d’une Blazor WebAssembly application autonome sur Azure App service pour Linux n’est pas pris en charge actuellement. Une image de serveur Linux pour héberger l’application n’est pas disponible pour l’instant. Le travail est en cours pour activer ce scénario.

### <a name="iis"></a>IIS

IIS est un serveur de fichiers statiques et puissant pour les Blazor applications. Pour configurer IIS pour héberger Blazor , consultez [créer un site Web statique sur IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).

Les ressources publiées sont créées dans le `/bin/Release/{TARGET FRAMEWORK}/publish` dossier. Hébergez le contenu du `publish` dossier sur le serveur Web ou le service d’hébergement.

#### <a name="webconfig"></a>web.config

Lorsqu’un Blazor projet est publié, un `web.config` fichier est créé avec la configuration IIS suivante :

* Les types MIME sont définis pour les extensions de fichiers suivantes :
  * `.dll`: `application/octet-stream`
  * `.json`: `application/json`
  * `.wasm`: `application/wasm`
  * `.woff`: `application/font-woff`
  * `.woff2`: `application/font-woff`
* La compression HTTP est activée pour les types MIME suivants :
  * `application/octet-stream`
  * `application/wasm`
* Des règles du module de réécriture d’URL sont établies :
  * Servez-vous du sous-répertoire dans lequel résident les ressources statiques de l’application ( `wwwroot/{PATH REQUESTED}` ).
  * Créez le routage de secours SPA afin que les demandes pour les ressources non-fichier soient redirigées vers le document par défaut de l’application dans son dossier ressources statiques ( `wwwroot/index.html` ).
  
#### <a name="use-a-custom-webconfig"></a>Utiliser un web.config personnalisé

Pour utiliser un `web.config` fichier personnalisé, placez le `web.config` fichier personnalisé à la racine du dossier du projet et publiez le projet.

#### <a name="install-the-url-rewrite-module"></a>Installer le module de réécriture d’URL

Le [module de réécriture d’URL](https://www.iis.net/downloads/microsoft/url-rewrite) est nécessaire pour réécrire les URL. Il n’est pas installé par défaut, et ne peut pas l’être en tant que fonctionnalité de service de rôle Serveur Web (IIS). Vous devez le télécharger à partir du site web IIS. Utilisez Web Platform Installer pour installer le module :

1. Localement, accédez à la [page des téléchargements du Module de réécriture d’URL](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads). Pour la version anglaise, sélectionnez **WebPI** pour télécharger le programme d’installation WebPI. Pour les autres langues, sélectionnez l’architecture appropriée pour le serveur (x86/x64) afin de télécharger le programme d’installation.
1. Copiez le programme d’installation sur le serveur. Exécutez le programme d’installation. Sélectionnez le bouton **Installer** et acceptez les termes du contrat de licence. Il n’est pas nécessaire de redémarrer le serveur après l’installation.

#### <a name="configure-the-website"></a>Configurer le site web

Affectez le dossier de l’application comme **chemin physique** du site web. Le dossier contient les éléments suivants :

* `web.config`Fichier utilisé par IIS pour configurer le site Web, y compris les règles de redirection et les types de contenu de fichier requis.
* Le dossier de ressources statiques de l’application

#### <a name="host-as-an-iis-sub-app"></a>Héberger en tant que sous-application IIS

Si une application autonome est hébergée en tant que sous-application IIS, effectuez l’une des opérations suivantes :

* Désactivez le gestionnaire de module ASP.NET Core hérité.

  Supprimez le gestionnaire dans le Blazor fichier publié de l’application `web.config` en ajoutant une `<handlers>` section au fichier :

  ```xml
  <handlers>
    <remove name="aspNetCore" />
  </handlers>
  ```

* Désactivez l’héritage de la section de l’application racine (parent) `<system.webServer>` à l’aide d’un `<location>` élément dont la `inheritInChildApplications` valeur est `false` :

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <configuration>
    <location path="." inheritInChildApplications="false">
      <system.webServer>
        <handlers>
          <add name="aspNetCore" ... />
        </handlers>
        <aspNetCore ... />
      </system.webServer>
    </location>
  </configuration>
  ```

La suppression du gestionnaire ou la désactivation de l’héritage est effectuée en plus de [la configuration du chemin d’accès de base de l’application](xref:blazor/host-and-deploy/index#app-base-path). Définissez le chemin d’accès de base de l’application dans le fichier de l’application `index.html` sur l’alias IIS utilisé lors de la configuration de la sous-application dans IIS.

#### <a name="brotli-and-gzip-compression"></a>Compression Brotli et gzip

IIS peut être configuré via `web.config` pour servir des ressources compressées Brotli ou gzip Blazor . Pour obtenir un exemple de configuration, consultez [`web.config`](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/blazor/host-and-deploy/webassembly/_samples/web.config?raw=true) .

#### <a name="troubleshooting"></a>Dépannage

Si vous recevez un message *500 – Erreur interne du serveur* et que le Gestionnaire IIS lève des erreurs quand vous tentez d’accéder à la configuration du site web, vérifiez que le module de réécriture d’URL est installé. Lorsque le module n’est pas installé, le `web.config` fichier ne peut pas être analysé par IIS. Cela empêche le gestionnaire des services Internet de charger la configuration du site Web et le site Web à partir des Blazor fichiers statiques de service.

Pour plus d’informations sur le dépannage des déploiements sur IIS, consultez <xref:test/troubleshoot-azure-iis>.

### <a name="azure-storage"></a>Azure Storage

L’hébergement de fichiers statiques [Azure Storage](/azure/storage/) permet l’hébergement d’applications sans serveur Blazor . Les noms de domaine personnalisé, le réseau de distribution de contenu Azure (CDN) et HTTPS sont pris en charge.

Lorsque le service blob est activé pour l’hébergement de site Web statique sur un compte de stockage :

* Définissez le **nom du document d’index** sur `index.html`.
* Définissez le **chemin d’accès au document d’erreur** sur `index.html`. Razorles composants et autres points de terminaison non-fichier ne résident pas sur des chemins d’accès physiques dans le contenu statique stocké par le service BLOB. Lorsqu’une demande pour l’une de ces ressources est reçue que le Blazor routeur doit gérer, l’erreur *404-introuvable* générée par le service BLOB achemine la requête vers le **chemin du document d’erreur**. L' `index.html` objet blob est retourné et le Blazor routeur charge et traite le chemin d’accès.

Si les fichiers ne sont pas chargés au moment de l’exécution en raison de types MIME inappropriés dans les `Content-Type` en-têtes des fichiers, effectuez l’une des actions suivantes :

* Configurez vos outils pour définir les types MIME corrects ( `Content-Type` en-têtes) lors du déploiement des fichiers.
* Modifiez les types MIME ( `Content-Type` en-têtes) des fichiers après le déploiement de l’application.

  En Explorateur Stockage (Portail Azure) pour chaque fichier :
  
  1. Cliquez avec le bouton droit sur le fichier et sélectionnez **Propriétés**.
  1. Définissez le **ContentType** et sélectionnez le bouton **Enregistrer** .

Pour plus d’informations, consultez [Hébergement de sites web statiques dans le service Stockage Azure](/azure/storage/blobs/storage-blob-static-website).

### <a name="nginx"></a>Nginx

Le `nginx.conf` fichier suivant est simplifié pour montrer comment configurer Nginx pour envoyer le `index.html` fichier lorsqu’il ne peut pas trouver de fichier correspondant sur le disque.

```
events { }
http {
    server {
        listen 80;

        location / {
            root /usr/share/nginx/html;
            try_files $uri $uri/ /index.html =404;
        }
    }
}
```

Pour plus d’informations sur la configuration du serveur web Nginx de production, consultez [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/) (Création de fichiers de configuration NGINX et NGINX Plus).

### <a name="nginx-in-docker"></a>Nginx dans Docker

Pour héberger Blazor dans l’ancrage à l’aide de nginx, configurez le fichier dockerfile pour utiliser l’image Nginx alpine. Mettez à jour le fichier dockerfile pour copier le `nginx.config` fichier dans le conteneur.

Ajoutez une ligne au fichier Dockerfile, comme indiqué dans l’exemple suivant :

```dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

### <a name="apache"></a>Apache

Pour déployer une Blazor WebAssembly application sur CentOS 7 ou version ultérieure :

1. Créez le fichier de configuration Apache. L’exemple suivant est un fichier de configuration simplifié ( `blazorapp.config` ) :

   ```
   <VirtualHost *:80>
       ServerName www.example.com
       ServerAlias *.example.com

       DocumentRoot "/var/www/blazorapp"
       ErrorDocument 404 /index.html

       AddType application/wasm .wasm
       AddType application/octet-stream .dll
   
       <Directory "/var/www/blazorapp">
           Options -Indexes
           AllowOverride None
       </Directory>

       <IfModule mod_deflate.c>
           AddOutputFilterByType DEFLATE text/css
           AddOutputFilterByType DEFLATE application/javascript
           AddOutputFilterByType DEFLATE text/html
           AddOutputFilterByType DEFLATE application/octet-stream
           AddOutputFilterByType DEFLATE application/wasm
           <IfModule mod_setenvif.c>
           BrowserMatch ^Mozilla/4 gzip-only-text/html
           BrowserMatch ^Mozilla/4.0[678] no-gzip
           BrowserMatch bMSIE !no-gzip !gzip-only-text/html
       </IfModule>
       </IfModule>

       ErrorLog /var/log/httpd/blazorapp-error.log
       CustomLog /var/log/httpd/blazorapp-access.log common
   </VirtualHost>
   ```

1. Placez le fichier de configuration Apache dans le `/etc/httpd/conf.d/` répertoire, qui est le répertoire de configuration Apache par défaut dans CentOS 7.

1. Placez les fichiers de l’application dans le `/var/www/blazorapp` répertoire (l’emplacement spécifié `DocumentRoot` dans le fichier de configuration).

1. Redémarrez le service Apache.

Pour plus d’informations, consultez [`mod_mime`](https://httpd.apache.org/docs/2.4/mod/mod_mime.html) et [`mod_deflate`](https://httpd.apache.org/docs/current/mod/mod_deflate.html) .

### <a name="github-pages"></a>GitHub Pages

Pour gérer la réécriture d’URL, ajoutez un `404.html` fichier avec un script qui gère la redirection de la requête vers la `index.html` page. Pour obtenir un exemple d’implémentation fournie par la communauté, consultez [Single Page Apps for GitHub Pages](https://spa-github-pages.rafrex.com/) (Applications à une seule page pour GitHub Pages) ([rafrex/spa-github-pages sur GitHub](https://github.com/rafrex/spa-github-pages#readme)). Vous pouvez obtenir un exemple d’utilisation de cette approche à l’adresse [blazor-demo/blazor-demo.github.io sur GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([site actif](https://blazor-demo.github.io/)).

Lorsque vous utilisez un site de projet plutôt qu’un site d’organisation, ajoutez ou mettez à jour la `<base>` balise dans `index.html` . Définissez la valeur d’attribut `href` sur le nom du dépôt GitHub avec une barre oblique (par exemple, `my-repository/`.

## <a name="host-configuration-values"></a>Valeurs de configuration de l’hôte

les [ Blazor WebAssembly applications](xref:blazor/hosting-models#blazor-webassembly) peuvent accepter les valeurs de configuration d’hôte suivantes comme arguments de ligne de commande au moment de l’exécution dans l’environnement de développement.

### <a name="content-root"></a>Racine de contenu

L' `--contentroot` argument définit le chemin d’accès absolu au répertoire qui contient les fichiers de contenu de l’application ([racine du contenu](xref:fundamentals/index#content-root)). Dans les exemples suivants, `/content-root-path` est le chemin racine du contenu de l’application.

* Passez l’argument lors de l’exécution de l’application localement à une invite de commandes. À partir du répertoire de l’application, exécutez :

  ```dotnetcli
  dotnet run --contentroot=/content-root-path
  ```

* Ajoutez une entrée au fichier de l’application `launchSettings.json` dans le profil **IIS Express** . Ce paramètre est utilisé en cas d’exécution de l’application avec le débogueur Visual Studio et dans une invite de commandes avec `dotnet run`.

  ```json
  "commandLineArgs": "--contentroot=/content-root-path"
  ```

* Dans Visual Studio, spécifiez l’argument dans **Propriétés**  >  **Déboguer**les arguments de l'  >  **application**. La définition de l’argument dans la page de propriétés de Visual Studio ajoute l’argument au `launchSettings.json` fichier.

  ```console
  --contentroot=/content-root-path
  ```

### <a name="path-base"></a>Base du chemin

L' `--pathbase` argument définit le chemin d’accès de base d’application pour une application exécutée localement avec un chemin d’URL relatif non racine (la `<base>` balise `href` est définie sur un chemin d’accès autre que `/` pour la mise en lots et la production). Dans les exemples suivants, `/relative-URL-path` est la base du chemin de l’application. Pour plus d’informations, consultez chemin de la base de l' [application](xref:blazor/host-and-deploy/index#app-base-path).

> [!IMPORTANT]
> Contrairement au chemin fourni au `href` de la balise `<base>`, n’incluez pas de barre oblique (`/`) quand vous passez la valeur d’argument `--pathbase`. Si vous spécifiez `<base href="/CoolApp/">` (inclut une barre oblique) comme chemin de base de l’application dans la balise `<base>`, passez `--pathbase=/CoolApp` (aucune barre oblique de fin) comme valeur d’argument de ligne de commande.

* Passez l’argument lors de l’exécution de l’application localement à une invite de commandes. À partir du répertoire de l’application, exécutez :

  ```dotnetcli
  dotnet run --pathbase=/relative-URL-path
  ```

* Ajoutez une entrée au fichier de l’application `launchSettings.json` dans le profil **IIS Express** . Ce paramètre est utilisé en cas d’exécution de l’application avec le débogueur Visual Studio et dans une invite de commandes avec `dotnet run`.

  ```json
  "commandLineArgs": "--pathbase=/relative-URL-path"
  ```

* Dans Visual Studio, spécifiez l’argument dans **Propriétés**  >  **Déboguer**les arguments de l'  >  **application**. La définition de l’argument dans la page de propriétés de Visual Studio ajoute l’argument au `launchSettings.json` fichier.

  ```console
  --pathbase=/relative-URL-path
  ```

### <a name="urls"></a>URLs

L’argument `--urls` définit les adresses IP ou les adresses d’hôtes avec les ports et protocoles sur lesquels il faut écouter les demandes.

* Passez l’argument lors de l’exécution de l’application localement à une invite de commandes. À partir du répertoire de l’application, exécutez :

  ```dotnetcli
  dotnet run --urls=http://127.0.0.1:0
  ```

* Ajoutez une entrée au fichier de l’application `launchSettings.json` dans le profil **IIS Express** . Ce paramètre est utilisé en cas d’exécution de l’application avec le débogueur Visual Studio et dans une invite de commandes avec `dotnet run`.

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* Dans Visual Studio, spécifiez l’argument dans **Propriétés**  >  **Déboguer**les arguments de l'  >  **application**. La définition de l’argument dans la page de propriétés de Visual Studio ajoute l’argument au `launchSettings.json` fichier.

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="configure-the-linker"></a>Configurer l'éditeur de liens

Blazoreffectue une liaison IL (Intermediate Language) sur chaque version de mise en production pour supprimer l’IL inutile des assemblys de sortie. Pour plus d’informations, consultez <xref:blazor/host-and-deploy/configure-linker>.

## <a name="custom-boot-resource-loading"></a>Chargement des ressources de démarrage personnalisé

Une Blazor WebAssembly application peut être initialisée avec la `loadBootResource` fonction pour remplacer le mécanisme de chargement des ressources de démarrage intégré. Utilisez `loadBootResource` pour les scénarios suivants :

* Autorisez les utilisateurs à charger des ressources statiques, telles que des données de fuseau horaire ou `dotnet.wasm` à partir d’un CDN.
* Chargez les assemblys compressés à l’aide d’une requête HTTP et décompressez-les sur le client pour les hôtes qui ne prennent pas en charge l’extraction du contenu compressé à partir du serveur.
* Alias des ressources à un autre nom en redirigeant chaque `fetch` requête vers un nouveau nom.

`loadBootResource`les paramètres s’affichent dans le tableau suivant.

| Paramètre    | Description |
| ------------ | ----------- |
| `type`       | Type de la ressource. Types permissables : `assembly` , `pdb` , `dotnetjs` , `dotnetwasm` ,`timezonedata` |
| `name`       | Nom de la ressource. |
| `defaultUri` | URI relatif ou absolu de la ressource. |
| `integrity`  | Chaîne d’intégrité représentant le contenu attendu dans la réponse. |

`loadBootResource`retourne l’un des éléments suivants pour remplacer le processus de chargement :

* Chaîne d’URI. Dans l’exemple suivant ( `wwwroot/index.html` ), les fichiers suivants sont pris en charge à partir d’un CDN à l’adresse `https://my-awesome-cdn.com/` :

  * `dotnet.*.js`
  * `dotnet.wasm`
  * Données de fuseau horaire

  ```html
  ...

  <script src="_framework/blazor.webassembly.js" autostart="false"></script>
  <script>
    Blazor.start({
      loadBootResource: function (type, name, defaultUri, integrity) {
        console.log(`Loading: '${type}', '${name}', '${defaultUri}', '${integrity}'`);
        switch (type) {
          case 'dotnetjs':
          case 'dotnetwasm':
          case 'timezonedata':
            return `https://my-awesome-cdn.com/blazorwebassembly/3.2.0/${name}`;
        }
      }
    });
  </script>
  ```

* `Promise<Response>`. Transmettez le `integrity` paramètre dans un en-tête pour conserver le comportement de contrôle d’intégrité par défaut.

  L’exemple suivant ( `wwwroot/index.html` ) ajoute un en-tête HTTP personnalisé aux demandes sortantes et passe `integrity` le paramètre à l' `fetch` appel :
  
  ```html
  <script src="_framework/blazor.webassembly.js" autostart="false"></script>
  <script>
    Blazor.start({
      loadBootResource: function (type, name, defaultUri, integrity) {
        return fetch(defaultUri, { 
          cache: 'no-cache',
          integrity: integrity,
          headers: { 'MyCustomHeader': 'My custom value' }
        });
      }
    });
  </script>
  ```

* `null`/`undefined`, ce qui entraîne le comportement de chargement par défaut.

Les sources externes doivent retourner les en-têtes CORS requis pour les navigateurs afin d’autoriser le chargement des ressources Cross-Origin. Les CDN fournissent généralement les en-têtes requis par défaut.

Il vous suffit de spécifier des types pour les comportements personnalisés. Les types non spécifiés à `loadBootResource` sont chargés par le Framework en fonction de leurs comportements de chargement par défaut.

## <a name="change-the-filename-extension-of-dll-files"></a>Modifier l’extension de nom de fichier des fichiers DLL

Si vous avez besoin de modifier les extensions de nom de fichier des fichiers publiés de l’application `.dll` , suivez les instructions de cette section.

Après avoir publié l’application, utilisez un script d’interpréteur de commandes ou un pipeline de build DevOps pour renommer les `.dll` fichiers afin d’utiliser une extension de fichier différente. Ciblez les `.dll` fichiers dans le `wwwroot` Répertoire de la sortie publiée de l’application (par exemple, `{CONTENT ROOT}/bin/Release/netstandard2.1/publish/wwwroot` ).

Dans les exemples suivants, les `.dll` fichiers sont renommés pour utiliser l' `.bin` extension de fichier.

Sur Windows :

```powershell
dir .\_framework\_bin | rename-item -NewName { $_.name -replace ".dll\b",".bin" }
((Get-Content .\_framework\blazor.boot.json -Raw) -replace '.dll"','.bin"') | Set-Content .\_framework\blazor.boot.json
```

Si des ressources de service Worker sont également utilisées, ajoutez la commande suivante :

```powershell
((Get-Content .\service-worker-assets.js -Raw) -replace '.dll"','.bin"') | Set-Content .\service-worker-assets.js
```

Sur Linux ou macOS :

```console
for f in _framework/_bin/*; do mv "$f" "`echo $f | sed -e 's/\.dll\b/.bin/g'`"; done
sed -i 's/\.dll"/.bin"/g' _framework/blazor.boot.json
```

Si des ressources de service Worker sont également utilisées, ajoutez la commande suivante :

```console
sed -i 's/\.dll"/.bin"/g' service-worker-assets.js
```
   
Pour utiliser une extension de fichier différente de `.bin` , remplacez `.bin` dans les commandes précédentes.

Pour résoudre les fichiers et compressés `blazor.boot.json.gz` `blazor.boot.json.br` , adoptez l’une des approches suivantes :

* Supprimez les `blazor.boot.json.gz` fichiers et compressés `blazor.boot.json.br` . La compression est désactivée avec cette approche.
* Recompressez le fichier mis à jour `blazor.boot.json` .

Les instructions précédentes s’appliquent également lorsque les ressources de service Worker sont en cours d’utilisation. Supprimez ou recompressez `wwwroot/service-worker-assets.js.br` et `wwwroot/service-worker-assets.js.gz` . Dans le cas contraire, les vérifications de l’intégrité des fichiers échouent dans le navigateur.

L’exemple Windows suivant utilise un script PowerShell placé à la racine du projet.

`ChangeDLLExtensions.ps1:`:

```powershell
param([string]$filepath,[string]$tfm)
dir $filepath\bin\Release\$tfm\wwwroot\_framework\_bin | rename-item -NewName { $_.name -replace ".dll\b",".bin" }
((Get-Content $filepath\bin\Release\$tfm\wwwroot\_framework\blazor.boot.json -Raw) -replace '.dll"','.bin"') | Set-Content $filepath\bin\Release\$tfm\wwwroot\_framework\blazor.boot.json
Remove-Item $filepath\bin\Release\$tfm\wwwroot\_framework\blazor.boot.json.gz
```

Si des ressources de service Worker sont également utilisées, ajoutez la commande suivante :

```powershell
((Get-Content $filepath\bin\Release\$tfm\wwwroot\service-worker-assets.js -Raw) -replace '.dll"','.bin"') | Set-Content $filepath\bin\Release\$tfm\wwwroot\service-worker-assets.js
```

Dans le fichier projet, le script est exécuté après la publication de l’application :

```xml
<Target Name="ChangeDLLFileExtensions" AfterTargets="Publish" Condition="'$(Configuration)'=='Release'">
  <Exec Command="powershell.exe -command &quot;&amp; { .\ChangeDLLExtensions.ps1 '$(SolutionDir)' '$(TargetFramework)'}&quot;" />
</Target>
```

Pour nous faire part de vos commentaires, visitez [aspnetcore/issues #5477](https://github.com/dotnet/aspnetcore/issues/5477).
 