---
title: Créez des applications Web progressifs avec ASP.NET Core Blazor webassembly
author: guardrex
description: Découvrez comment créer des applications Web progressifs basées sur des Blazor(PWA), des applications Web qui utilisent des fonctionnalités de navigateur modernes pour se comporter comme des applications de bureau.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: blazor/progressive-web-app
ms.openlocfilehash: f1c1ce50f20bf579e67e1d4eb02cc7d9d681e354
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083719"
---
# <a name="build-progressive-web-applications-with-aspnet-core-opno-locblazor-webassembly"></a>Créez des applications Web progressifs avec ASP.NET Core Blazor webassembly

Par [Steve Sanderson](https://github.com/SteveSandersonMS)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

Une application Web progressive (PWA) est une application Web qui utilise des API et des fonctionnalités de navigateur modernes pour se comporter comme une application de bureau. Ces fonctionnalités peuvent inclure :

* Travailler en mode hors connexion et toujours charger instantanément, indépendamment de la vitesse du réseau
* Pouvoir s’exécuter dans sa propre fenêtre d’application, et pas seulement dans une fenêtre de navigateur
* Lancé à partir du menu Démarrer du système d’exploitation hôte (se), de l’ancre ou de l’écran d’accueil
* Réception de notifications push à partir d’un serveur principal, même si l’utilisateur n’utilise pas l’application
* Mise à jour automatique en arrière-plan

Un utilisateur peut tout d’abord découvrir et utiliser l’application dans son navigateur Web comme n’importe quelle autre application à page unique (SPA), puis progresser ultérieurement pour l’installer dans son système d’exploitation et activer les notifications push. C’est la raison pour laquelle nous utilisons le terme *progressif*.

Blazor webassembly est une véritable plate-forme d’application Web côté client basée sur des normes, il peut utiliser n’importe quelle API de navigateur, y compris les API PWA nécessaires pour les fonctionnalités mentionnées ci-dessus. Il peut travailler hors connexion comme n’importe quelle autre technologie Web côté client.

## <a name="pwa-template"></a>Modèle PWA

Lorsque vous créez une application Blazor webassembly, vous avez la possibilité d’ajouter des fonctionnalités de PWA. Dans Visual Studio, l’option est fournie sous la forme d’une case à cocher dans la boîte de dialogue de création du projet :

![image](https://user-images.githubusercontent.com/1101362/76207411-a6b54200-61f5-11ea-9dfc-6acd87a91428.png)

Si vous créez le projet sur la ligne de commande, vous pouvez utiliser l’indicateur `--pwa`. Par exemple,

```dotnetcli
dotnet new blazorwasm --pwa -o MyNewProject
```

Dans les deux cas, vous êtes libre de combiner cela avec l’option « ASP.NET Core hébergée » si vous le souhaitez, mais vous n’avez pas à le faire. Les fonctionnalités de PWA sont indépendantes du modèle d’hébergement.

## <a name="installation-and-app-manifest"></a>Manifeste d’installation et d’application

Quand vous visitez une application créée à l’aide de l’option de modèle PWA, les utilisateurs ont la possibilité d’installer l’application dans le menu Démarrer, l’ancre ou l’écran d’accueil de son système d’exploitation.

La façon dont cette option est présentée dépend du navigateur de l’utilisateur. Par exemple, lorsque vous utilisez des navigateurs basés sur Desktop chrome, tels que Edge ou chrome, un bouton *Ajouter* apparaît dans la barre d’URL :

![image](https://user-images.githubusercontent.com/1101362/76208127-f7796a80-61f6-11ea-8aea-7fba704be787.png)

Sur iOS, les visiteurs peuvent installer le PWA à l’aide du bouton de *partage* de Safari et de l’option *Ajouter à homescreen* . Sur chrome pour Android, les utilisateurs doivent appuyer sur le bouton de *menu* dans l’angle supérieur droit, puis choisir *Ajouter à l’écran d’accueil*.

Une fois installé, l’application s’affiche dans sa propre fenêtre, sans barre d’adresses.

![image](https://user-images.githubusercontent.com/1101362/76208588-e2e9a200-61f7-11ea-85e1-8c3fc849b252.png)

Pour personnaliser le titre, le modèle de couleurs, l’icône ou d’autres détails de la fenêtre, consultez le fichier `manifest.json` dans le répertoire *wwwroot* de votre projet. Le schéma de ce fichier est défini par les normes Web. Pour obtenir une documentation détaillée, consultez https://developer.mozilla.org/docs/Web/Manifest.

## <a name="offline-support"></a>Prise en charge hors connexion

Par défaut, les applications créées à l’aide de l’option de modèle PWA prennent en charge l’exécution hors connexion. Un utilisateur doit d’abord accéder à l’application pendant qu’il est en ligne, puis le navigateur télécharge et met en cache automatiquement toutes les ressources nécessaires pour travailler hors connexion.

> [!IMPORTANT]
> La prise en charge hors connexion est activée uniquement pour les applications *publiées* . Elle n’est pas activée pendant le développement. Cela est dû au fait que cela interfère avec le cycle de développement habituel qui consiste à apporter des modifications et à les tester.

> [!WARNING]
> Si vous envisagez de livrer un PWA en mode hors connexion, vous devez comprendre [plusieurs avertissements importants et avertissements](#caveats-for-offline-pwas) . Celles-ci sont inhérentes aux PWA hors connexion et ne sont pas spécifiques à Blazor. Veillez à lire et à comprendre ces avertissements avant de faire des hypothèses sur le fonctionnement de votre application en mode hors connexion.

Pour voir comment fonctionne la prise en charge hors connexion, [publiez d’abord votre application](https://docs.microsoft.com/aspnet/core/host-and-deploy/blazor/?view=aspnetcore-3.1&tabs=visual-studio#publish-the-app)et hébergez-la sur un serveur prenant en charge le protocole HTTPS. Lorsque vous accédez à l’application, vous devez être en mesure d’ouvrir les outils de développement du navigateur et de vérifier qu’un processus de *travail de service* est inscrit pour votre hôte :

![image](https://user-images.githubusercontent.com/1101362/76210294-bd5e9780-61fb-11ea-9869-65c55c62803d.png)

En outre, si vous rechargez la page, dans l’onglet *réseau* , vous devez voir que toutes les ressources nécessaires pour charger votre page sont récupérées à partir du *Service Worker* ou du *cache de mémoire*:

![image](https://user-images.githubusercontent.com/1101362/76210472-1d553e00-61fc-11ea-84c6-291644df709e.png)

Cela indique que le navigateur n’est pas dépendant de l’accès réseau pour charger votre application. Pour vérifier cela, vous pouvez soit arrêter votre serveur Web, soit demander au navigateur de simuler le mode hors connexion :

![image](https://user-images.githubusercontent.com/1101362/76210556-47a6fb80-61fc-11ea-9d12-20a8f6528744.png)

Désormais, même sans accès à votre serveur Web, vous devez être en mesure de recharger la page et de voir que votre application se charge et s’exécute toujours. De même, même si vous simulez une connexion réseau très lente, votre page sera toujours chargée immédiatement puisqu’elle est chargée indépendamment du réseau.

### <a name="service-worker"></a>Service Worker

La prise en charge hors connexion est obtenue à l’aide d’un service Worker. Il s’agit d’une norme Web, non spécifique à Blazor. Pour obtenir de la documentation sur les Workers de service, consultez https://developer.mozilla.org/docs/Web/API/Service_Worker_API. Pour en savoir plus sur les modèles d’utilisation courants pour les Workers, consultez l’article excellent sur [le cycle de vie des services](https://developers.google.com/web/fundamentals/primers/service-workers/lifecycle).

le modèle PWA de Blazorproduit deux fichiers de travail de service :

* *wwwroot/Service-Worker. js*, qui est utilisé pendant le développement
* *wwwroot/Service-Worker. published. js*, qui est utilisé une fois votre application publiée

Si vous souhaitez partager la logique entre ces deux fichiers, envisagez d’ajouter un troisième fichier JavaScript pour contenir la logique commune et utilisez [`self.importScripts`](https://developer.mozilla.org/docs/Web/API/WorkerGlobalScope/importScripts) pour charger cette logique dans les deux fichiers.

#### <a name="cache-first-fetch-strategy"></a>Stratégie d’extraction du cache en premier

Le service Worker *Service-Worker. published. js* intégré résout les demandes à l’aide d’une stratégie de *cache-première* . Cela signifie qu’il est toujours préférable de retourner le contenu mis en cache s’il est disponible, que l’utilisateur dispose d’un accès réseau ou qu’un contenu plus récent soit disponible sur le serveur.

Cela peut être utile pour deux raisons :

* **Cela garantit la fiabilité.** L’accès réseau n’est pas un état booléen. Un utilisateur n’est pas simplement « en ligne » ou « hors connexion ». En réalité, l’appareil de l’utilisateur peut croire qu’il est en ligne, mais il est possible que le réseau soit si lent qu’il soit difficile à attendre. Il se peut également que le réseau renvoie des résultats non valides pour certaines URL, par exemple lorsqu’il existe un portail WIFI captif qui bloque ou redirige actuellement certaines requêtes. C’est la raison pour laquelle l’API `navigator.onLine` du navigateur n’est pas fiable et ne doit pas dépendre de.
* **Cela garantit l’exactitude.** Lors de la création d’un cache de ressources hors connexion, le service worker utilise le hachage de contenu pour garantir qu’il a extrait un instantané complet et cohérent des ressources en un seul instant. Ce cache est ensuite utilisé comme une unité atomique. À partir de cela, il n’y a aucun point à demander au réseau des ressources plus récentes, puisque les seules versions que vous souhaitez sont celles que vous avez déjà mises en cache. Tout autre risque d’incohérence et d’incompatibilité (par exemple, essayer d’utiliser des versions d’assemblys .NET qui n’ont pas été compilés ensemble).

#### <a name="background-updates"></a>Mises à jour en arrière-plan

En tant que modèle mental, vous pouvez considérer qu’un PWA en mode hors connexion se comporte comme une application mobile qui peut être installée. Il démarre toujours immédiatement, quelle que soit la connectivité réseau, mais la logique d’application installée provient d’un instantané à un moment donné qui peut ne pas être la dernière version.

Le modèle Blazor PWA produit des applications qui essaient automatiquement de se mettre à jour en arrière-plan chaque fois que l’utilisateur accède à une connexion réseau opérationnelle. Ce fonctionnement est le suivant :

* Au cours de la compilation, votre projet génère un *manifeste des ressources du service Worker*. Par défaut, ce nom est appelé *Service-Worker-Assets. js*. Cette liste répertorie toutes les ressources statiques dont votre application a besoin pour fonctionner hors connexion, telles que les assemblys .NET, les fichiers JavaScript, les CSS, etc., y compris leurs hachages de contenu. Cette liste est chargée par votre service Worker afin qu’il sache quelles ressources mettre en cache.
* Chaque fois que l’utilisateur visite votre application, le navigateur redemande *Service-Worker. js* et *Service-Worker-Assets. js* en arrière-plan. Si le serveur retourne le contenu modifié pour l’un de ces fichiers (par rapport à l’octet d’octet avec le Worker de service installé existant), le processus de travail de service tente d’installer une nouvelle version de lui-même.
* Lors de l’installation d’une nouvelle version de lui-même, le processus de travail crée un cache distinct pour les ressources hors connexion et commence à le remplir avec les ressources listées dans *Service-Worker-Assets. js*. Cette logique est implémentée dans la fonction `onInstall` à l’intérieur de *Service-Worker. published. js*.
* Si le processus se termine correctement (par exemple, si toutes les ressources sont chargées sans erreur et que tous les hachages de contenu correspondent), le nouveau service Worker passe à l’État « en attente d’activation ». Dès que l’utilisateur ferme votre application (c’est-à-dire qu’il n’y a pas d’onglets ou de fenêtres restantes qui affichent votre application), le nouveau service Worker devient « actif » et sera utilisé pour les visites ultérieures à votre application. L’ancien service Worker et son cache sont supprimés.
* Si le processus ne se termine pas correctement, la nouvelle instance du Worker service est ignorée. Le processus de mise à jour sera tenté à nouveau à la prochaine visite de l’utilisateur, alors qu’il devrait avoir une meilleure connexion réseau capable d’effectuer les requêtes.

Vous pouvez personnaliser n’importe quel aspect de ce processus en modifiant la logique du service Worker. Aucune des options ci-dessus n’est spécifique à Blazor, mais il s’agit simplement d’une suggestion fournie par l’option de modèle PWA. Pour plus d’informations, consultez [la documentation sur le service Worker](https://developer.mozilla.org/docs/Web/API/Service_Worker_API.) .

#### <a name="how-requests-are-resolved"></a>Résolution des demandes

Comme décrit ci-dessus, le service Worker par défaut utilise une stratégie de *cache en premier* , ce qui signifie qu’il tente de traiter le contenu mis en cache lorsqu’il est disponible. S’il n’existe aucun contenu mis en cache pour une certaine URL, par exemple lors de la demande de données à partir d’une API de serveur principal, le processus de travail de service revient sur une demande réseau normale qui peut uniquement aboutir si le serveur est accessible. Cette logique est implémentée dans `onFetch` dans *Service-Worker. published. js*.

Si vos composants de Blazor s’appuient sur la demande de données à partir des API principales et que vous souhaitez fournir une expérience utilisateur conviviale dans le cas où de telles requêtes échouent en raison d’une indisponibilité du réseau, vous devez implémenter une logique dans vos composants. Par exemple, utilisez `try/catch` autour des demandes `HttpClient`.

#### <a name="support-server-rendered-pages"></a>Prendre en charge les pages rendues par le serveur

Réfléchissez à ce qui se passe quand l’utilisateur accède pour la première fois à une URL telle que `/counter` ou tout autre lien profond dans votre application. Dans ce cas, vous ne souhaitez pas renvoyer le contenu mis en cache en tant que `/counter`, mais vous avez besoin du navigateur pour charger le contenu mis en cache en tant que `/index.html` pour démarrer votre application Blazor webassembly. Ces demandes initiales sont appelées demandes de *navigation* (par opposition aux demandes de sous- *ressources* pour les images/CSS/etc, ou requêtes *Fetch/XHR* pour les données d’API).

Le service Worker par défaut contient une logique de cas spéciale pour les demandes de navigation. Elle les résout en retournant le contenu mis en cache pour `/index.html`, quelle que soit l’URL demandée. Cette logique est implémentée dans la fonction `onFetch` à l’intérieur de *Service-Worker. published. js*.

Si votre application comporte certaines URL qui doivent retourner du code HTML rendu serveur (et ne servent pas `/index.html` du cache), vous devez modifier la logique dans votre service Worker. Par exemple, si toutes les URL contenant des `/Identity/` doivent être gérées en tant que demandes standard en ligne uniquement au serveur, modifiez la logique de `onFetch` de *Service-Worker. published. js* . Recherchez le code suivant :

```javascript
const shouldServeIndexHtml = event.request.mode === 'navigate';
```

Remplacez le code par ce qui suit :

```javascript
const shouldServeIndexHtml = event.request.mode === 'navigate'
    && !event.request.url.includes('/Identity/');
```

Si vous n’effectuez pas cette opération, quelle que soit la connectivité réseau, le service Workers intercepte les demandes de ces URL et les résout à l’aide de `/index.html`.

#### <a name="control-asset-caching"></a>Contrôler la mise en cache des ressources

Si votre projet définit une propriété MSBuild appelée `ServiceWorkerAssetsManifest`, les outils de génération de Blazorgénèrent un manifeste des ressources de travail de service avec le nom spécifié. Le modèle PWA par défaut produit un fichier projet contenant les éléments suivants :

```xml
<ServiceWorkerAssetsManifest>service-worker-assets.js</ServiceWorkerAssetsManifest>
```

Le fichier est placé dans le répertoire de sortie *wwwroot* , de sorte que le navigateur peut récupérer ce fichier en demandant `/service-worker-assets.js`. Pour afficher le contenu, ouvrez *YourProject\bin\Debug\netstandard2.1\wwwroot\service-Worker-Assets.js* dans un éditeur de texte. Toutefois, ne modifiez pas le fichier, car il sera régénéré à chaque Build.

Par défaut, ce manifeste répertorie les éléments suivants :

* Toutes les ressources gérées par Blazor, telles que les assemblys .NET et les fichiers d’exécution webassembly .NET nécessaires pour fonctionner hors connexion
* Toutes les ressources qui seront publiées dans votre répertoire *wwwroot* , telles que des images, des fichiers CSS et des fichiers JavaScript. Cela comprend les ressources Web statiques fournies par les projets externes et les packages NuGet.

Vous pouvez contrôler les ressources qui seront extraites et mises en cache par le service Worker en modifiant la logique dans `onInstall` dans *Service-Worker. published. js*. Par défaut, il récupère et met en cache les fichiers qui correspondent aux extensions de nom de fichier Web typiques, telles que *. html*, *. CSS*, *. js*, *. WASM*, etc., ainsi que les types de fichiers spécifiques à Blazor webassembly ( *. dll*, *. pdb*).

Si vous souhaitez inclure des ressources supplémentaires qui ne sont pas présentes dans votre répertoire *wwwroot* , vous pouvez le faire en définissant des entrées ItemGroup de MSBuild supplémentaires. Par exemple, dans votre fichier projet, ajoutez :

```xml
<ItemGroup>
    <ServiceWorkerAssetsManifestItem
        Include="MyDirectory\AnotherFile.json"
        RelativePath="MyDirectory\AnotherFile.json"
        AssetUrl="files/AnotherFile.json" />
</ItemGroup>
```

Les métadonnées de `AssetUrl` spécifient l’URL relative de base que le navigateur doit utiliser lors de la récupération de la ressource à mettre en cache. Cela peut être indépendant du nom du fichier source d’origine sur le disque.

> [!IMPORTANT]
> L’ajout d’un `ServiceWorkerAssetsManifestItem` n’entraîne pas la publication du fichier dans votre répertoire *wwwroot* . Il vous permet de contrôler votre sortie de publication séparément. Le `ServiceWorkerAssetsManifestItem` entraîne uniquement l’apparition d’une entrée supplémentaire dans le manifeste des ressources du Worker.

## <a name="push-notifications"></a>Notifications Push

Comme tout autre PWA, un Blazor webassembly PWA peut recevoir des notifications push à partir d’un serveur principal. Votre serveur peut les envoyer à tout moment, même lorsque l’utilisateur n’utilise pas activement votre application (par exemple, lorsqu’un autre utilisateur effectue une action qui peut être pertinente).

Le mécanisme d’envoi d’une notification push est entièrement indépendant de Blazor webassembly, car il est implémenté par le serveur principal qui peut utiliser n’importe quelle technologie. Si vous souhaitez envoyer des notifications push à partir d’un serveur de ASP.NET Core, envisagez d' [utiliser une technique similaire à celle de l’atelier de pizzas éblouissantes](https://github.com/dotnet-presentations/blazor-workshop/blob/master/docs/09-progressive-web-app.md#sending-push-notifications).

Le mécanisme de réception et d’affichage d’une notification push sur le client est également indépendant de Blazor webassembly, puisqu’il est implémenté dans le Worker service, qui est un fichier JavaScript. À titre d’exemple, vous pouvez à nouveau voir [l’approche utilisée dans l’atelier de pizzas éblouissant](https://github.com/dotnet-presentations/blazor-workshop/blob/master/docs/09-progressive-web-app.md#displaying-notifications).

## <a name="caveats-for-offline-pwas"></a>Avertissements pour les PWA hors connexion

Toutes les applications ne doivent pas essayer de prendre en charge l’utilisation hors connexion. Elle ajoute une complexité significative, tout en n’étant pas toujours pertinente.

La prise en charge hors connexion est généralement pertinente uniquement :

* Si votre magasin de données principal est local dans le navigateur. Par exemple, lors de la création d’une interface utilisateur pour un appareil [IOT](https://en.wikipedia.org/wiki/Internet_of_things) qui stocke des données dans `localStorage` ou [IndexedDB](https://developer.mozilla.org/docs/Web/API/IndexedDB_API).

* Si vous effectuez un travail important pour extraire et mettre en cache les données d’API principales pertinentes pour chaque utilisateur, ils peuvent naviguer hors connexion. Si vous prenez en charge la modification, vous devrez également créer un système pour le suivi des modifications et leur synchronisation avec le serveur principal.

* Si votre objectif est de garantir le chargement immédiat de l’application indépendamment des conditions du réseau. Vous devez ensuite implémenter une expérience utilisateur appropriée autour des demandes de l’API backend pour afficher la progression des requêtes et se comporter correctement lorsqu’elles échouent en raison d’une indisponibilité du réseau.

En outre, les PWAS en mode hors connexion doivent gérer un éventail de complications supplémentaires. Les développeurs doivent se familiariser avec les avertissements suivants.

### <a name="offline-support-only-when-published"></a>Prise en charge hors connexion uniquement lors de la publication

le modèle PWA de Blazoractive la prise en charge hors connexion uniquement lorsqu’il est publié. En effet, lors du développement, vous souhaitez généralement voir chaque modification reflétée immédiatement dans le navigateur, sans passer par un processus de mise à jour en arrière-plan.

Par conséquent, lors de la création d’une application de compatibilité hors connexion, il n’est pas suffisant de tester votre application en mode de développement. Vous devez tester votre application dans son état publié pour comprendre comment elle doit répondre à des conditions de réseau différentes.

### <a name="update-completion-after-user-navigation-away-from-app"></a>Mettre à jour l’achèvement après la navigation de l’utilisateur hors de l’application

Les mises à jour ne se terminent pas tant que l’utilisateur n’a pas quitté votre application dans tous les onglets. Comme expliqué dans les mises à jour en [arrière-plan](#background-updates), après le déploiement d’une mise à jour de votre application, le navigateur récupère les fichiers de travail de service mis à jour et entame un processus de mise à jour.

Ce que de nombreux développeurs peuvent faire, même lorsque cette mise à jour est terminée, elle n’entre pas en vigueur tant que l’utilisateur n’a **pas** quitté tous les onglets. Il n’est **pas** suffisant d’actualiser l’onglet qui affiche votre application, même s’il s’agit du seul onglet qui affiche votre application. Tant que votre application n’est pas complètement fermée, le nouveau service Worker reste dans un État « en attente d’activation ». **Cela n’est pas spécifique à Blazor, mais plutôt à un comportement de plateforme Web standard.**

Cela ne pose généralement pas de ennuis aux développeurs qui essaient de tester les mises à jour de leur service Worker ou des ressources mises en cache hors connexion. Si vous archivez les outils de développement du navigateur, vous pouvez voir un exemple semblable au suivant :

![image](https://user-images.githubusercontent.com/1101362/76226394-b93f7380-6215-11ea-8572-7d52afee2dd8.png)

Tant que la liste des « clients » (c’est-à-dire les onglets ou les fenêtres affichant votre application) n’est pas vide, le thread de travail continue à attendre. La raison pour laquelle les travailleurs du service effectue cette tâche est de garantir la cohérence, c’est-à-dire que toutes les ressources sont extraites du même cache atomique.

Lorsque vous testez des modifications, il peut s’avérer utile de cliquer sur le lien « skipWaiting », comme indiqué dans la capture d’écran ci-dessus, puis de recharger la page. Si vous le souhaitez, vous pouvez automatiser ce processus pour tous les utilisateurs en codant votre travail de service afin d' [ignorer la phase « en attente » et de l’activer immédiatement lors de la mise à jour](https://developers.google.com/web/fundamentals/primers/service-workers/lifecycle#skip_the_waiting_phase). Toutefois, si vous procédez ainsi, vous vous assurez que les ressources sont toujours extraites de façon cohérente à partir de la même instance de cache.

### <a name="users-may-run-any-historical-version-of-the-app"></a>Les utilisateurs peuvent exécuter n’importe quelle version historique de l’application

Les développeurs Web s’attendent habituellement à ce que les utilisateurs n’exécutent que la dernière version déployée de leur application Web, car cela est normal dans le modèle de distribution Web traditionnel. Toutefois, un PWA en mode hors connexion est plus similaire à une application mobile native, où les utilisateurs n’exécutent pas nécessairement la dernière version.

Comme expliqué dans les [mises à jour en arrière-plan](#background-updates), après le déploiement d’une mise à jour de votre application, **chaque utilisateur existant continue d’utiliser une version antérieure pour au moins une visite** (car la mise à jour se produit en arrière-plan et n’est pas activée tant que l’utilisateur n’a pas quitté l’utilisateur). En outre, la version précédente utilisée n’est pas nécessairement la version précédente que vous avez déployée. il peut s’agir de *n’importe quelle* version historique, selon la date à laquelle l’utilisateur a effectué la dernière mise à jour.

Cela peut être un problème si les composants frontend et backend de votre application requièrent un accord sur le schéma pour les demandes d’API. Vous ne devez pas déployer des modifications de schéma d’API à compatibilité descendante tant que vous ne pouvez pas vous assurer que tous les utilisateurs ont été mis à niveau, ou au moins empêcher les utilisateurs d’utiliser des versions antérieures incompatibles de l’application. C’est comme une application mobile native. Si vous déployez une modification avec rupture dans les API de serveur, l’application cliente est interrompue pour les personnes qui n’ont pas encore été mises à jour.

Si possible, ne déployez pas les modifications importantes apportées à vos API backend. Toutefois, si vous devez le faire, envisagez d’utiliser des [API de travail de service standard, telles que `ServiceWorkerRegistration`](https://developer.mozilla.org/docs/Web/API/ServiceWorkerRegistration) pour déterminer si l’application est à jour et, si ce n’est pas le cas, pour empêcher l’utilisation.

### <a name="interference-with-server-rendered-pages"></a>Interférence avec les pages rendues par le serveur

[Comme décrit ci-dessus](#support-server-rendered-pages), si vous souhaitez contourner le comportement du processus de travail de retour de `/index.html` contenu pour toutes les demandes de navigation, vous devez modifier la logique de votre service Worker.

### <a name="all-service-worker-asset-manifest-contents-are-cached-by-default"></a>Tous les contenus du manifeste de la ressource Service Worker sont mis en cache par défaut

[Comme décrit ci-dessus](#control-asset-caching), le fichier *Service-Worker-Assets. js* est généré pendant la génération et répertorie toutes les ressources que le processus de travail de service doit extraire et mettre en cache.

Étant donné que cette liste inclut par défaut tout ce qui est émis vers *wwwroot* (y compris le contenu fourni par des packages et des projets externes), vous devez veiller à ne pas y placer trop de contenu. Si, par exemple, votre répertoire *wwwroot* contient des millions d’images, le service Worker essaiera de les extraire et de les mettre en cache, en consommant une bande passante excessive et probablement pas correctement.

Vous pouvez implémenter une logique arbitraire pour contrôler le sous-ensemble du contenu du manifeste qui doit être extrait et mis en cache en modifiant la fonction `onInstall` dans *Service-Worker. published. js*.

### <a name="interaction-with-authentication"></a>Interaction avec l’authentification

Il est possible d’utiliser l’option de modèle PWA conjointement avec les options d’authentification. Un PWA compatible hors connexion peut également prendre en charge l’authentification lorsque l’utilisateur dispose d’une connectivité réseau.

Toutefois, lorsqu’un utilisateur n’a pas de connectivité réseau, il ne peut pas s’authentifier ou obtenir des jetons d’accès. Si vous tentez d’accéder à la page « connexion », un message indiquant « erreur réseau » s’affiche par défaut.

Par conséquent, il s’agit de votre travail de conception d’un workflow d’interface utilisateur qui permet à l’utilisateur d’effectuer des opérations utiles en mode hors connexion sans tenter de s’authentifier ou d’obtenir des jetons d’accès, ou au moins échouer dans ces cas. Si ce n’est pas possible dans votre application, il se peut que vous ne souhaitiez pas activer la prise en charge hors connexion.