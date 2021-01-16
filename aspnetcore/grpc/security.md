---
title: Considérations sur la sécurité dans gRPC pour ASP.NET Core
author: jamesnk
description: En savoir plus sur les considérations de sécurité pour gRPC pour ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 07/07/2019
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
uid: grpc/security
ms.openlocfilehash: 45ac0916a368cf68f4d40e14298a7628446989ee
ms.sourcegitcommit: 063a06b644d3ade3c15ce00e72a758ec1187dd06
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/16/2021
ms.locfileid: "98252810"
---
# <a name="security-considerations-in-grpc-for-aspnet-core"></a><span data-ttu-id="e69b6-103">Considérations sur la sécurité dans gRPC pour ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e69b6-103">Security considerations in gRPC for ASP.NET Core</span></span>

<span data-ttu-id="e69b6-104">Par [James Newton-King](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="e69b6-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

<span data-ttu-id="e69b6-105">Cet article fournit des informations sur la sécurisation de gRPC avec .NET Core.</span><span class="sxs-lookup"><span data-stu-id="e69b6-105">This article provides information on securing gRPC with .NET Core.</span></span>

## <a name="transport-security"></a><span data-ttu-id="e69b6-106">Sécurité du transport</span><span class="sxs-lookup"><span data-stu-id="e69b6-106">Transport security</span></span>

<span data-ttu-id="e69b6-107">les messages gRPC sont envoyés et reçus à l’aide de HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="e69b6-107">gRPC messages are sent and received using HTTP/2.</span></span> <span data-ttu-id="e69b6-108">Nous vous recommandons d’utiliser les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="e69b6-108">We recommend:</span></span>

* <span data-ttu-id="e69b6-109">[TLS (Transport Layer Security)](https://tools.ietf.org/html/rfc5246) permet de sécuriser les messages dans les applications gRPC de production.</span><span class="sxs-lookup"><span data-stu-id="e69b6-109">[Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246) be used to secure messages in production gRPC apps.</span></span>
* <span data-ttu-id="e69b6-110">les services gRPC doivent uniquement écouter les ports sécurisés et y répondre.</span><span class="sxs-lookup"><span data-stu-id="e69b6-110">gRPC services should only listen and respond over secured ports.</span></span>

::: moniker range=">= aspnetcore-5.0"
<span data-ttu-id="e69b6-111">TLS est configuré dans Kestrel.</span><span class="sxs-lookup"><span data-stu-id="e69b6-111">TLS is configured in Kestrel.</span></span> <span data-ttu-id="e69b6-112">Pour plus d’informations sur la configuration des points de terminaison Kestrel, consultez [configuration du point de terminaison Kestrel](xref:fundamentals/servers/kestrel/endpoints).</span><span class="sxs-lookup"><span data-stu-id="e69b6-112">For more information on configuring Kestrel endpoints, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel/endpoints).</span></span>
::: moniker-end

::: moniker range="< aspnetcore-5.0"
<span data-ttu-id="e69b6-113">TLS est configuré dans Kestrel.</span><span class="sxs-lookup"><span data-stu-id="e69b6-113">TLS is configured in Kestrel.</span></span> <span data-ttu-id="e69b6-114">Pour plus d’informations sur la configuration des points de terminaison Kestrel, consultez [configuration du point de terminaison Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="e69b6-114">For more information on configuring Kestrel endpoints, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>
::: moniker-end

## <a name="exceptions"></a><span data-ttu-id="e69b6-115">Exceptions</span><span class="sxs-lookup"><span data-stu-id="e69b6-115">Exceptions</span></span>

<span data-ttu-id="e69b6-116">Les messages d’exception sont généralement considérés comme des données sensibles qui ne doivent pas être révélées à un client.</span><span class="sxs-lookup"><span data-stu-id="e69b6-116">Exception messages are generally considered sensitive data that shouldn't be revealed to a client.</span></span> <span data-ttu-id="e69b6-117">Par défaut, gRPC n’envoie pas les détails d’une exception levée par un service gRPC au client.</span><span class="sxs-lookup"><span data-stu-id="e69b6-117">By default, gRPC doesn't send the details of an exception thrown by a gRPC service to the client.</span></span> <span data-ttu-id="e69b6-118">Au lieu de cela, le client reçoit un message générique indiquant qu’une erreur s’est produite.</span><span class="sxs-lookup"><span data-stu-id="e69b6-118">Instead, the client receives a generic message indicating an error occurred.</span></span> <span data-ttu-id="e69b6-119">La remise de message d’exception au client peut être remplacée (par exemple, dans le développement ou le test) par [EnableDetailedErrors](xref:grpc/configuration#configure-services-options).</span><span class="sxs-lookup"><span data-stu-id="e69b6-119">Exception message delivery to the client can be overridden (for example, in development or test) with [EnableDetailedErrors](xref:grpc/configuration#configure-services-options).</span></span> <span data-ttu-id="e69b6-120">Les messages d’exception ne doivent pas être exposés au client dans les applications de production.</span><span class="sxs-lookup"><span data-stu-id="e69b6-120">Exception messages shouldn't be exposed to the client in production apps.</span></span>

## <a name="message-size-limits"></a><span data-ttu-id="e69b6-121">Limites de taille des messages</span><span class="sxs-lookup"><span data-stu-id="e69b6-121">Message size limits</span></span>

<span data-ttu-id="e69b6-122">Les messages entrants aux clients et services gRPC sont chargés en mémoire.</span><span class="sxs-lookup"><span data-stu-id="e69b6-122">Incoming messages to gRPC clients and services are loaded into memory.</span></span> <span data-ttu-id="e69b6-123">Les limites de taille des messages sont un mécanisme qui permet d’empêcher gRPC de consommer trop de ressources.</span><span class="sxs-lookup"><span data-stu-id="e69b6-123">Message size limits are a mechanism to help prevent gRPC from consuming excessive resources.</span></span>

<span data-ttu-id="e69b6-124">gRPC utilise des limites de taille par message pour gérer les messages entrants et sortants.</span><span class="sxs-lookup"><span data-stu-id="e69b6-124">gRPC uses per-message size limits to manage incoming and outgoing messages.</span></span> <span data-ttu-id="e69b6-125">Par défaut, gRPC limite les messages entrants à 4 Mo.</span><span class="sxs-lookup"><span data-stu-id="e69b6-125">By default, gRPC limits incoming messages to 4 MB.</span></span> <span data-ttu-id="e69b6-126">Il n’existe aucune limite pour les messages sortants.</span><span class="sxs-lookup"><span data-stu-id="e69b6-126">There is no limit on outgoing messages.</span></span>

<span data-ttu-id="e69b6-127">Sur le serveur, les limites de messages gRPC peuvent être configurées pour tous les services d’une application avec `AddGrpc` :</span><span class="sxs-lookup"><span data-stu-id="e69b6-127">On the server, gRPC message limits can be configured for all services in an app with `AddGrpc`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.MaxReceiveMessageSize = 1 * 1024 * 1024; // 1 MB
        options.MaxSendMessageSize = 1 * 1024 * 1024; // 1 MB
    });
}
```

<span data-ttu-id="e69b6-128">Les limites peuvent également être configurées pour un service individuel à l’aide de `AddServiceOptions<TService>` .</span><span class="sxs-lookup"><span data-stu-id="e69b6-128">Limits can also be configured for an individual service using `AddServiceOptions<TService>`.</span></span> <span data-ttu-id="e69b6-129">Pour plus d’informations sur la configuration des limites de taille des messages, consultez [configuration de gRPC](xref:grpc/configuration).</span><span class="sxs-lookup"><span data-stu-id="e69b6-129">For more information on configuring message size limits, see [gRPC configuration](xref:grpc/configuration).</span></span>

## <a name="client-certificate-validation"></a><span data-ttu-id="e69b6-130">Validation du certificat client</span><span class="sxs-lookup"><span data-stu-id="e69b6-130">Client certificate validation</span></span>

<span data-ttu-id="e69b6-131">Les [certificats clients](https://tools.ietf.org/html/rfc5246#section-7.4.4) sont initialement validés lorsque la connexion est établie.</span><span class="sxs-lookup"><span data-stu-id="e69b6-131">[Client certificates](https://tools.ietf.org/html/rfc5246#section-7.4.4) are initially validated when the connection is established.</span></span> <span data-ttu-id="e69b6-132">Par défaut, Kestrel n’effectue pas de validation supplémentaire du certificat client d’une connexion.</span><span class="sxs-lookup"><span data-stu-id="e69b6-132">By default, Kestrel doesn't perform additional validation of a connection's client certificate.</span></span>

<span data-ttu-id="e69b6-133">Nous recommandons que les services gRPC sécurisés par les certificats clients utilisent le package [Microsoft. AspNetCore. Authentication. Certificate](xref:security/authentication/certauth) .</span><span class="sxs-lookup"><span data-stu-id="e69b6-133">We recommend that gRPC services secured by client certificates use the [Microsoft.AspNetCore.Authentication.Certificate](xref:security/authentication/certauth) package.</span></span> <span data-ttu-id="e69b6-134">ASP.NET Core l’authentification de certification effectue une validation supplémentaire sur un certificat client, notamment :</span><span class="sxs-lookup"><span data-stu-id="e69b6-134">ASP.NET Core certification authentication will perform additional validation on a client certificate, including:</span></span>

* <span data-ttu-id="e69b6-135">Le certificat a une utilisation avancée de la clé (EKU) valide</span><span class="sxs-lookup"><span data-stu-id="e69b6-135">Certificate has a valid extended key use (EKU)</span></span>
* <span data-ttu-id="e69b6-136">Est dans sa période de validité</span><span class="sxs-lookup"><span data-stu-id="e69b6-136">Is within its validity period</span></span>
* <span data-ttu-id="e69b6-137">Vérifier la révocation des certificats</span><span class="sxs-lookup"><span data-stu-id="e69b6-137">Check certificate revocation</span></span>
