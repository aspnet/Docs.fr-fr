---
title: Pourquoi migrer WCF vers ASP.NET Core gRPC
author: markrendle
description: Cet article fournit un résumé de la raison pour laquelle ASP.NET Core gRPC est adapté aux développeurs Windows Communication Foundation (WCF) qui souhaitent migrer vers des architectures et des plateformes modernes.
monikerRange: '>= aspnetcore-3.0'
ms.author: wpickett
ms.date: 09/02/2020
no-loc:
- 'appsettings.json'
- 'ASP.NET Core Identity'
- 'cookie'
- 'Cookie'
- 'Blazor'
- 'Blazor Server'
- 'Blazor WebAssembly'
- 'Identity'
- "Let's Encrypt"
- 'Razor'
- 'SignalR'
uid: grpc/wcf
ms.openlocfilehash: 26629b4aa5510f4ef5f53f57b64e45f6c32d4014
ms.sourcegitcommit: ca34c1ac578e7d3daa0febf1810ba5fc74f60bbf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93058686"
---
# <a name="grpc-for-windows-communication-foundation-wcf-developers"></a><span data-ttu-id="25d56-103">gRPC pour les développeurs Windows Communication Foundation (WCF)</span><span class="sxs-lookup"><span data-stu-id="25d56-103">gRPC for Windows Communication Foundation (WCF) developers</span></span>

<span data-ttu-id="25d56-104">Cet article fournit un résumé de la raison pour laquelle ASP.NET Core gRPC est adapté aux développeurs Windows Communication Foundation (WCF) qui souhaitent migrer vers des architectures et des plateformes modernes.</span><span class="sxs-lookup"><span data-stu-id="25d56-104">This article provides a summary of why ASP.NET Core gRPC is a good fit for Windows Communication Foundation (WCF) developers who want to migrate to modern architectures and platforms.</span></span>

## <a name="comparison-to-wcf"></a><span data-ttu-id="25d56-105">Comparaison avec WCF</span><span class="sxs-lookup"><span data-stu-id="25d56-105">Comparison to WCF</span></span>

<span data-ttu-id="25d56-106">Bien que l’implémentation et l’approche soient différentes pour gRPC, l’expérience du développement et de l’utilisation des services avec gRPC doit être intuitive pour les développeurs WCF.</span><span class="sxs-lookup"><span data-stu-id="25d56-106">Although the implementation and approach are different for gRPC, the experience of developing and consuming services with gRPC should be intuitive for WCF developers.</span></span> <span data-ttu-id="25d56-107">WCF et gRPC sont des infrastructures RPC (Remote Procedure Call, appel de procédure distante) ayant les mêmes objectifs :</span><span class="sxs-lookup"><span data-stu-id="25d56-107">WCF and gRPC are RPC (remote procedure call) frameworks with the same goals:</span></span>

* <span data-ttu-id="25d56-108">Rendre possible le code comme si le client et le serveur se trouvent sur la même plate-forme.</span><span class="sxs-lookup"><span data-stu-id="25d56-108">Make it possible to code as though the client and server are on the same platform.</span></span>
* <span data-ttu-id="25d56-109">Fournir une API réseau portable simplifiée.</span><span class="sxs-lookup"><span data-stu-id="25d56-109">Provide a simplified portable networking API.</span></span>

<span data-ttu-id="25d56-110">Les deux plateformes partagent la nécessité de déclarer et d’implémenter une interface, bien que le processus de déclaration de l’interface soit différent.</span><span class="sxs-lookup"><span data-stu-id="25d56-110">Both platforms share the requirement of declaring and implementing an interface, although the process for declaring the interface is different.</span></span> <span data-ttu-id="25d56-111">Les nombreux types d’appels RPC que gRPC prend en charge mappent bien aux liaisons disponibles pour les services WCF.</span><span class="sxs-lookup"><span data-stu-id="25d56-111">The many types of RPC calls that gRPC supports map well to the bindings available to WCF services.</span></span> <span data-ttu-id="25d56-112">Pour plus d’informations et d’exemples, consultez [migrer une solution WCF vers gRPC](/dotnet/architecture/grpc-for-wcf-developers/migrate-wcf-to-grpc).</span><span class="sxs-lookup"><span data-stu-id="25d56-112">For more information and examples, see [Migrate a WCF solution to gRPC](/dotnet/architecture/grpc-for-wcf-developers/migrate-wcf-to-grpc).</span></span>

## <a name="benefits-of-grpc"></a><span data-ttu-id="25d56-113">Avantages de gRPC</span><span class="sxs-lookup"><span data-stu-id="25d56-113">Benefits of gRPC</span></span>

<span data-ttu-id="25d56-114">gRPC fournit une meilleure infrastructure que les autres approches pour les raisons suivantes.</span><span class="sxs-lookup"><span data-stu-id="25d56-114">gRPC provides a better framework than other approaches for the following reasons.</span></span>

### <a name="performance"></a><span data-ttu-id="25d56-115">Performances</span><span class="sxs-lookup"><span data-stu-id="25d56-115">Performance</span></span>

<span data-ttu-id="25d56-116">gRPC utilise HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="25d56-116">gRPC uses HTTP/2.</span></span> <span data-ttu-id="25d56-117">Contrairement à HTTP/1.1, HTTP/2 :</span><span class="sxs-lookup"><span data-stu-id="25d56-117">In contrast to HTTP/1.1, HTTP/2:</span></span>

* <span data-ttu-id="25d56-118">Est un protocole binaire plus petit et plus rapide.</span><span class="sxs-lookup"><span data-stu-id="25d56-118">Is a smaller, faster binary protocol.</span></span>
* <span data-ttu-id="25d56-119">Est plus efficace pour l’analyse des ordinateurs.</span><span class="sxs-lookup"><span data-stu-id="25d56-119">Is more efficient for computers to parse.</span></span>
* <span data-ttu-id="25d56-120">Prend en charge le multiplexage des requêtes sur une seule connexion.</span><span class="sxs-lookup"><span data-stu-id="25d56-120">Supports multiplexing requests over a single connection.</span></span> <span data-ttu-id="25d56-121">Le multiplexage permet d’envoyer plusieurs demandes sur une connexion sans que les demandes se bloquent mutuellement.</span><span class="sxs-lookup"><span data-stu-id="25d56-121">Multiplexing enables multiple requests to be sent over one connection without requests blocking each other.</span></span> <span data-ttu-id="25d56-122">Dans HTTP/1.1, le blocage est appelé « blocage de la tête de ligne (HOL) ».</span><span class="sxs-lookup"><span data-stu-id="25d56-122">In HTTP/1.1, the blocking is known as "head-of-line (HOL) blocking."</span></span>

<span data-ttu-id="25d56-123">gRPC utilise Protobuf, un format binaire efficace, pour sérialiser les messages.</span><span class="sxs-lookup"><span data-stu-id="25d56-123">gRPC uses Protobuf, an efficient binary format, to serialize messages.</span></span> <span data-ttu-id="25d56-124">Les messages Protobuf sont les suivants :</span><span class="sxs-lookup"><span data-stu-id="25d56-124">Protobuf messages are:</span></span>
* <span data-ttu-id="25d56-125">Rapidité de sérialisation et de désérialisation.</span><span class="sxs-lookup"><span data-stu-id="25d56-125">Fast to serialize and deserialize.</span></span>
* <span data-ttu-id="25d56-126">Utilisez moins de bande passante que les formats textuels.</span><span class="sxs-lookup"><span data-stu-id="25d56-126">Use less bandwidth than text-based formats.</span></span> 

<span data-ttu-id="25d56-127">gRPC est une bonne solution pour les appareils mobiles et les réseaux avec des restrictions de bande passante.</span><span class="sxs-lookup"><span data-stu-id="25d56-127">gRPC is a good solution for mobile devices and networks with bandwidth restrictions.</span></span>

### <a name="interoperability"></a><span data-ttu-id="25d56-128">Interopérabilité</span><span class="sxs-lookup"><span data-stu-id="25d56-128">Interoperability</span></span>

<span data-ttu-id="25d56-129">Il existe des outils et des bibliothèques gRPC pour tous les principaux langages de programmation et plateformes, notamment .NET, Java, Python, Go, C++, Node.js, SWIFT, DART, Ruby et PHP.</span><span class="sxs-lookup"><span data-stu-id="25d56-129">There are gRPC tools and libraries for all major programming languages and platforms, including .NET, Java, Python, Go, C++, Node.js, Swift, Dart, Ruby, and PHP.</span></span> <span data-ttu-id="25d56-130">Grâce au format de câble binaire Protobuf et à la génération de code efficace pour chaque plateforme, les développeurs peuvent créer des applications performantes, multiplateformes.</span><span class="sxs-lookup"><span data-stu-id="25d56-130">Thanks to the Protobuf binary wire format and the efficient code generation for each platform, developers can build cross-platform, performant apps.</span></span>

### <a name="usability-and-productivity"></a><span data-ttu-id="25d56-131">Convivialité et productivité</span><span class="sxs-lookup"><span data-stu-id="25d56-131">Usability and productivity</span></span>

<span data-ttu-id="25d56-132">gRPC est une solution RPC complète.</span><span class="sxs-lookup"><span data-stu-id="25d56-132">gRPC is a comprehensive RPC solution.</span></span> <span data-ttu-id="25d56-133">Il fonctionne de manière cohérente sur plusieurs langages et plateformes.</span><span class="sxs-lookup"><span data-stu-id="25d56-133">It works consistently across multiple languages and platforms.</span></span> <span data-ttu-id="25d56-134">Il fournit également d’excellents outils, avec une grande partie du code réutilisable généré automatiquement.</span><span class="sxs-lookup"><span data-stu-id="25d56-134">It also provides excellent tooling, with much of the boilerplate code automatically generated.</span></span> <span data-ttu-id="25d56-135">Comme WCF, gRPC génère automatiquement des messages et un client fortement typé.</span><span class="sxs-lookup"><span data-stu-id="25d56-135">Like WCF, gRPC automatically generates messages and a strongly typed client.</span></span> <span data-ttu-id="25d56-136">Le temps du développeur est libéré pour se concentrer sur la logique métier.</span><span class="sxs-lookup"><span data-stu-id="25d56-136">Developer time is freed up to focus on business logic.</span></span>

### <a name="streaming"></a><span data-ttu-id="25d56-137">Diffusion en continu</span><span class="sxs-lookup"><span data-stu-id="25d56-137">Streaming</span></span>

<span data-ttu-id="25d56-138">gRPC dispose d’une diffusion en continu bidirectionnelle complète qui offre des fonctionnalités similaires à celles des services duplex complets de WCF.</span><span class="sxs-lookup"><span data-stu-id="25d56-138">gRPC has full bidirectional streaming, which provides similar functionality to WCF's full duplex services.</span></span> <span data-ttu-id="25d56-139">la diffusion en continu gRPC peut fonctionner sur des connexions Internet régulières, des équilibrages de charge et des maillages de service.</span><span class="sxs-lookup"><span data-stu-id="25d56-139">gRPC streaming can operate over regular internet connections, load balancers, and service meshes.</span></span>

### <a name="deadlines-timeouts-and-cancellation"></a><span data-ttu-id="25d56-140">Échéances, délais d’attente et annulation</span><span class="sxs-lookup"><span data-stu-id="25d56-140">Deadlines, timeouts, and cancellation</span></span>

<span data-ttu-id="25d56-141">gRPC permet aux clients de spécifier la durée maximale pendant laquelle un RPC se termine.</span><span class="sxs-lookup"><span data-stu-id="25d56-141">gRPC allows clients to specify a maximum time for an RPC to finish.</span></span> <span data-ttu-id="25d56-142">Si l’échéance spécifiée est dépassée, le serveur peut annuler l’opération indépendamment du client.</span><span class="sxs-lookup"><span data-stu-id="25d56-142">If the specified deadline is exceeded, the server can cancel the operation independently of the client.</span></span> <span data-ttu-id="25d56-143">Les échéances et les annulations peuvent être propagées par le biais des appels gRPC suivants pour aider à appliquer les limites d’utilisation des ressources.</span><span class="sxs-lookup"><span data-stu-id="25d56-143">Deadlines and cancellations can be propagated through subsequent gRPC calls to help enforce resource usage limits.</span></span> <span data-ttu-id="25d56-144">Les clients peuvent arrêter des opérations lorsqu’une échéance est dépassée, ou plus tôt si nécessaire.</span><span class="sxs-lookup"><span data-stu-id="25d56-144">Clients can stop operations when a deadline is exceeded, or earlier if necessary.</span></span> <span data-ttu-id="25d56-145">Par exemple, les clients peuvent arrêter des opérations en raison d’une interaction avec l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="25d56-145">For example, clients can stop operations because of a user interaction.</span></span>

### <a name="security"></a><span data-ttu-id="25d56-146">Sécurité</span><span class="sxs-lookup"><span data-stu-id="25d56-146">Security</span></span>

<span data-ttu-id="25d56-147">gRPC peut utiliser TLS et HTTP/2 pour fournir une connexion chiffrée de bout en bout entre le client et le serveur.</span><span class="sxs-lookup"><span data-stu-id="25d56-147">gRPC can use TLS and HTTP/2 to provide an end-to-end encrypted connection between the client and the server.</span></span> <span data-ttu-id="25d56-148">La prise en charge de l’authentification par certificat client augmente davantage la sécurité et l’approbation entre le client et le serveur.</span><span class="sxs-lookup"><span data-stu-id="25d56-148">Support for client certificate authentication further increases security and trust between client and server.</span></span>

## <a name="grpc-as-a-migration-path-for-wcf-to-net-core-and-net-5"></a><span data-ttu-id="25d56-149">gRPC comme chemin de migration pour WCF vers .NET Core et .NET 5</span><span class="sxs-lookup"><span data-stu-id="25d56-149">gRPC as a migration path for WCF to .NET Core and .NET 5</span></span>

<span data-ttu-id="25d56-150">.NET Core et .NET 5 marquent une évolution de la façon dont Microsoft fournit des solutions de communication à distance pour les développeurs qui souhaitent fournir des services sur toute une gamme de plateformes.</span><span class="sxs-lookup"><span data-stu-id="25d56-150">.NET Core and .NET 5 marks a shift in the way that Microsoft delivers remote communication solutions to developers who want to deliver services across a range of platforms.</span></span> <span data-ttu-id="25d56-151">.NET Core et .NET 5 prennent en charge l' [appel des services WCF](/dotnet/core/additional-tools/wcf-web-service-reference-guide), mais n’offrent pas de prise en charge côté serveur pour l’hébergement de WCF.</span><span class="sxs-lookup"><span data-stu-id="25d56-151">.NET Core and .NET 5 support [calling WCF services](/dotnet/core/additional-tools/wcf-web-service-reference-guide), but won't offer server-side support for hosting WCF.</span></span>

<span data-ttu-id="25d56-152">Il existe deux chemins d’accès recommandés pour la modernisation des applications WCF :</span><span class="sxs-lookup"><span data-stu-id="25d56-152">There are two recommended paths for modernizing WCF apps:</span></span>

* <span data-ttu-id="25d56-153">gRPC repose sur les technologies modernes et est apparu comme le choix le plus populaire dans la communauté des développeurs pour les applications RPC.</span><span class="sxs-lookup"><span data-stu-id="25d56-153">gRPC is built on modern technologies and has emerged as the most popular choice across the developer community for RPC apps.</span></span> <span data-ttu-id="25d56-154">À compter de .NET Core 3,0, les plateformes .NET modernes offrent une excellente prise en charge de gRPC.</span><span class="sxs-lookup"><span data-stu-id="25d56-154">Starting with .NET Core 3.0, modern .NET platforms have excellent support for gRPC.</span></span> <span data-ttu-id="25d56-155">La migration des services WCF pour utiliser gRPC permet de fournir les fonctionnalités RPC, les performances et l’interopérabilité nécessaires dans les applications modernes.</span><span class="sxs-lookup"><span data-stu-id="25d56-155">Migrating WCF services to use gRPC helps provide the RPC features, performance, an interoperability needed in modern apps.</span></span>

* <span data-ttu-id="25d56-156">[CoreWCF](https://github.com/CoreWCF/CoreWCF) est un effort de la Communauté visant à prendre en charge l’hébergement des services WCF sur .net Core et .net 5.</span><span class="sxs-lookup"><span data-stu-id="25d56-156">[CoreWCF](https://github.com/CoreWCF/CoreWCF) is a community effort to bring support for hosting WCF services to .NET Core and .NET 5.</span></span> <span data-ttu-id="25d56-157">Une version préliminaire est disponible et le projet est prêt pour la production.</span><span class="sxs-lookup"><span data-stu-id="25d56-157">A preview release is available and the project is working towards being production ready.</span></span> <span data-ttu-id="25d56-158">CoreWCF prend uniquement en charge un sous-ensemble des fonctionnalités de WCF et .NET Framework les applications qui migrent pour l’utiliser nécessitent des modifications de code et des tests pour réussir.</span><span class="sxs-lookup"><span data-stu-id="25d56-158">CoreWCF only supports a subset of WCF's features, and .NET Framework apps that migrate to use it will need code changes and testing to be successful.</span></span> <span data-ttu-id="25d56-159">CoreWCF est un bon choix si une application doit assurer la compatibilité avec les clients existants qui appellent les services WCF.</span><span class="sxs-lookup"><span data-stu-id="25d56-159">CoreWCF is a good choice if an app has to maintain compatibility with existing clients that call WCF services.</span></span>

## <a name="get-started"></a><span data-ttu-id="25d56-160">Bien démarrer</span><span class="sxs-lookup"><span data-stu-id="25d56-160">Get started</span></span>

<span data-ttu-id="25d56-161">Pour obtenir des instructions détaillées sur la création de services gRPC dans ASP.NET Core pour les développeurs WCF, consultez [ASP.net Core gRPC pour les développeurs WCF](/dotnet/architecture/grpc-for-wcf-developers) .</span><span class="sxs-lookup"><span data-stu-id="25d56-161">For detailed guidance on building gRPC services in ASP.NET Core for WCF developers, see [ASP.NET Core gRPC for WCF Developers](/dotnet/architecture/grpc-for-wcf-developers)</span></span>
