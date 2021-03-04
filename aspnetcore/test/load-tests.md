---
title: ASP.NET Core le test de charge/stress
author: Jeremy-Meng
description: Découvrez plusieurs outils et approches notables pour le test de charge et les tests de stress ASP.NET Core les applications.
ms.author: riande
ms.custom: mvc
ms.date: 4/05/2019
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
uid: test/loadtests
ms.openlocfilehash: 5a860fe398dff2e12671b6854a80bbab3120499b
ms.sourcegitcommit: a1db01b4d3bd8c57d7a9c94ce122a6db68002d66
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102109479"
---
# <a name="aspnet-core-loadstress-testing"></a><span data-ttu-id="9535e-103">ASP.NET Core le test de charge/stress</span><span class="sxs-lookup"><span data-stu-id="9535e-103">ASP.NET Core load/stress testing</span></span>

<span data-ttu-id="9535e-104">Les tests de charge et les tests de stress sont importants pour garantir qu’une application Web est performante et évolutive.</span><span class="sxs-lookup"><span data-stu-id="9535e-104">Load testing and stress testing are important to ensure a web app is performant and scalable.</span></span> <span data-ttu-id="9535e-105">Leurs objectifs sont différents même s’ils partagent souvent des tests similaires.</span><span class="sxs-lookup"><span data-stu-id="9535e-105">Their goals are different even though they often share similar tests.</span></span>

<span data-ttu-id="9535e-106">**Tests de charge**: Testez si l’application peut gérer une charge d’utilisateurs spécifiée pour un certain scénario tout en répondant toujours à l’objectif de réponse.</span><span class="sxs-lookup"><span data-stu-id="9535e-106">**Load tests**: Test whether the app can handle a specified load of users for a certain scenario while still satisfying the response goal.</span></span> <span data-ttu-id="9535e-107">L’application est exécutée dans des conditions normales.</span><span class="sxs-lookup"><span data-stu-id="9535e-107">The app is run under normal conditions.</span></span>

<span data-ttu-id="9535e-108">**Tests de stress**: stabilité de l’application de test lors de l’exécution dans des conditions extrêmes, souvent pendant une longue période de temps.</span><span class="sxs-lookup"><span data-stu-id="9535e-108">**Stress tests**: Test app stability when running under extreme conditions, often for a long period of time.</span></span> <span data-ttu-id="9535e-109">Les tests mettent en place une charge utilisateur élevée, des pics ou une augmentation progressive de la charge, sur l’application, ou ils limitent les ressources informatiques de l’application.</span><span class="sxs-lookup"><span data-stu-id="9535e-109">The tests place high user load, either spikes or gradually increasing load, on the app, or they limit the app's computing resources.</span></span>

<span data-ttu-id="9535e-110">Les tests de contrainte déterminent si une application en contrainte peut récupérer après une défaillance et retourner normalement au comportement attendu.</span><span class="sxs-lookup"><span data-stu-id="9535e-110">Stress tests determine if an app under stress can recover from failure and gracefully return to expected behavior.</span></span> <span data-ttu-id="9535e-111">En cas de stress, l’application n’est pas exécutée dans des conditions normales.</span><span class="sxs-lookup"><span data-stu-id="9535e-111">Under stress, the app isn't run under normal conditions.</span></span>

<span data-ttu-id="9535e-112">Visual Studio 2019 a annoncé des plans pour [déprécier le test de charge](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).</span><span class="sxs-lookup"><span data-stu-id="9535e-112">Visual Studio 2019 announced plans to [deprecate the load testing](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).</span></span> <span data-ttu-id="9535e-113">Le service de test de charge basé sur le Cloud Azure DevOps correspondant a été fermé.</span><span class="sxs-lookup"><span data-stu-id="9535e-113">The corresponding Azure DevOps cloud-based load testing service has been closed.</span></span>

## <a name="third-party-tools"></a><span data-ttu-id="9535e-114">Outils tiers</span><span class="sxs-lookup"><span data-stu-id="9535e-114">Third-party tools</span></span>

<span data-ttu-id="9535e-115">La liste suivante contient des outils de performances Web tiers avec différents ensembles de fonctionnalités :</span><span class="sxs-lookup"><span data-stu-id="9535e-115">The following list contains third-party web performance tools with various feature sets:</span></span>

* [<span data-ttu-id="9535e-116">Apache JMeter</span><span class="sxs-lookup"><span data-stu-id="9535e-116">Apache JMeter</span></span>](https://jmeter.apache.org/)
* [<span data-ttu-id="9535e-117">ApacheBench (AB)</span><span class="sxs-lookup"><span data-stu-id="9535e-117">ApacheBench (ab)</span></span>](https://httpd.apache.org/docs/2.4/programs/ab.html)
* [<span data-ttu-id="9535e-118">Gatling</span><span class="sxs-lookup"><span data-stu-id="9535e-118">Gatling</span></span>](https://gatling.io/)
* [<span data-ttu-id="9535e-119">K6</span><span class="sxs-lookup"><span data-stu-id="9535e-119">k6</span></span>](https://k6.io)
* [<span data-ttu-id="9535e-120">Caroubes</span><span class="sxs-lookup"><span data-stu-id="9535e-120">Locust</span></span>](https://locust.io/)
* [<span data-ttu-id="9535e-121">Surtension de l’ouest du vent</span><span class="sxs-lookup"><span data-stu-id="9535e-121">West Wind WebSurge</span></span>](https://websurge.west-wind.com/)
* [<span data-ttu-id="9535e-122">Netlingue</span><span class="sxs-lookup"><span data-stu-id="9535e-122">Netling</span></span>](https://github.com/hallatore/Netling)
* [<span data-ttu-id="9535e-123">Vegeta</span><span class="sxs-lookup"><span data-stu-id="9535e-123">Vegeta</span></span>](https://github.com/tsenart/vegeta)
* [<span data-ttu-id="9535e-124">NBomber</span><span class="sxs-lookup"><span data-stu-id="9535e-124">NBomber</span></span>](https://github.com/PragmaticFlow/NBomber)
