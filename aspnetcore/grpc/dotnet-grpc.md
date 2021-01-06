---
title: Gérer les références Protobuf avec dotnet-GRPC
author: juntaoluo
description: En savoir plus sur l’ajout, la mise à jour, la suppression et la liste de références Protobuf avec l’outil Global dotnet-GRPC.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 10/17/2019
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
uid: grpc/dotnet-grpc
ms.openlocfilehash: f34e1543d9695e138a85db3b79e013cf5fb6d138
ms.sourcegitcommit: 3593c4efa707edeaaceffbfa544f99f41fc62535
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/04/2021
ms.locfileid: "93059908"
---
# <a name="manage-protobuf-references-with-dotnet-grpc"></a><span data-ttu-id="1ca01-103">Gérer les références Protobuf avec dotnet-GRPC</span><span class="sxs-lookup"><span data-stu-id="1ca01-103">Manage Protobuf references with dotnet-grpc</span></span>

<span data-ttu-id="1ca01-104">Par [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="1ca01-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="1ca01-105">`dotnet-grpc` est un outil Global .NET Core pour la gestion des références [Protobuf (*. proto*)](xref:grpc/basics#proto-file) dans un projet .net gRPC.</span><span class="sxs-lookup"><span data-stu-id="1ca01-105">`dotnet-grpc` is a .NET Core Global Tool for managing [Protobuf (*.proto*)](xref:grpc/basics#proto-file) references within a .NET gRPC project.</span></span> <span data-ttu-id="1ca01-106">L’outil peut être utilisé pour ajouter, actualiser, supprimer et répertorier des références Protobuf.</span><span class="sxs-lookup"><span data-stu-id="1ca01-106">The tool can be used to add, refresh, remove, and list Protobuf references.</span></span>

## <a name="installation"></a><span data-ttu-id="1ca01-107">Installation</span><span class="sxs-lookup"><span data-stu-id="1ca01-107">Installation</span></span>

<span data-ttu-id="1ca01-108">Pour installer l' `dotnet-grpc` [outil Global .net Core](/dotnet/core/tools/global-tools), exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="1ca01-108">To install the `dotnet-grpc` [.NET Core Global Tool](/dotnet/core/tools/global-tools), run the following command:</span></span>

```dotnetcli
dotnet tool install -g dotnet-grpc
```

## <a name="add-references"></a><span data-ttu-id="1ca01-109">Ajouter des références</span><span class="sxs-lookup"><span data-stu-id="1ca01-109">Add references</span></span>

<span data-ttu-id="1ca01-110">`dotnet-grpc` peut être utilisé pour ajouter des références Protobuf en tant qu' `<Protobuf />` éléments au fichier *. csproj* :</span><span class="sxs-lookup"><span data-stu-id="1ca01-110">`dotnet-grpc` can be used to add Protobuf references as `<Protobuf />` items to the *.csproj* file:</span></span>

```xml
<Protobuf Include="Protos\greet.proto" GrpcServices="Server" />
```

<span data-ttu-id="1ca01-111">Les références Protobuf sont utilisées pour générer les ressources du client et/ou du serveur C#.</span><span class="sxs-lookup"><span data-stu-id="1ca01-111">The Protobuf references are used to generate the C# client and/or server assets.</span></span> <span data-ttu-id="1ca01-112">L' `dotnet-grpc` outil peut :</span><span class="sxs-lookup"><span data-stu-id="1ca01-112">The `dotnet-grpc` tool can:</span></span>

* <span data-ttu-id="1ca01-113">Créez une référence Protobuf à partir de fichiers locaux sur le disque.</span><span class="sxs-lookup"><span data-stu-id="1ca01-113">Create a Protobuf reference from local files on disk.</span></span>
* <span data-ttu-id="1ca01-114">Crée une référence Protobuf à partir d’un fichier distant spécifié par une URL.</span><span class="sxs-lookup"><span data-stu-id="1ca01-114">Create a Protobuf reference from a remote file specified by a URL.</span></span>
* <span data-ttu-id="1ca01-115">Vérifiez que les dépendances de package gRPC correctes sont ajoutées au projet.</span><span class="sxs-lookup"><span data-stu-id="1ca01-115">Ensure the correct gRPC package dependencies are added to the project.</span></span>

<span data-ttu-id="1ca01-116">Par exemple, le `Grpc.AspNetCore` package est ajouté à une application Web.</span><span class="sxs-lookup"><span data-stu-id="1ca01-116">For example, the `Grpc.AspNetCore` package is added to a web app.</span></span> <span data-ttu-id="1ca01-117">`Grpc.AspNetCore` contient les bibliothèques clientes et de serveur gRPC et la prise en charge des outils.</span><span class="sxs-lookup"><span data-stu-id="1ca01-117">`Grpc.AspNetCore` contains gRPC server and client libraries and tooling support.</span></span> <span data-ttu-id="1ca01-118">Les `Grpc.Net.Client` `Grpc.Tools` packages, et `Google.Protobuf` , qui contiennent uniquement les bibliothèques clientes gRPC et la prise en charge des outils, sont également ajoutés à une application console.</span><span class="sxs-lookup"><span data-stu-id="1ca01-118">Alternatively, the `Grpc.Net.Client`, `Grpc.Tools` and `Google.Protobuf` packages, which contain only the gRPC client libraries and tooling support, are added to a Console app.</span></span>

### <a name="add-file"></a><span data-ttu-id="1ca01-119">Ajouter un fichier</span><span class="sxs-lookup"><span data-stu-id="1ca01-119">Add file</span></span>

<span data-ttu-id="1ca01-120">La `add-file` commande est utilisée pour ajouter des fichiers locaux sur le disque en tant que références Protobuf.</span><span class="sxs-lookup"><span data-stu-id="1ca01-120">The `add-file` command is used to add local files on disk as Protobuf references.</span></span> <span data-ttu-id="1ca01-121">Chemins d’accès aux fichiers fournis :</span><span class="sxs-lookup"><span data-stu-id="1ca01-121">The file paths provided:</span></span>

* <span data-ttu-id="1ca01-122">Peut être relatif au répertoire actif ou aux chemins d’accès absolus.</span><span class="sxs-lookup"><span data-stu-id="1ca01-122">Can be relative to the current directory or absolute paths.</span></span>
* <span data-ttu-id="1ca01-123">Peut contenir des caractères génériques pour les [globbing](https://wikipedia.org/wiki/Glob_(programming))de fichiers basés sur des modèles.</span><span class="sxs-lookup"><span data-stu-id="1ca01-123">May contain wild cards for pattern-based file [globbing](https://wikipedia.org/wiki/Glob_(programming)).</span></span>

<span data-ttu-id="1ca01-124">Si des fichiers se trouvent en dehors du répertoire du projet, un `Link` élément est ajouté pour afficher le fichier sous le dossier `Protos` dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1ca01-124">If any files are outside the project directory, a `Link` element is added to display the file under the folder `Protos` in Visual Studio.</span></span>

### <a name="usage"></a><span data-ttu-id="1ca01-125">Utilisation</span><span class="sxs-lookup"><span data-stu-id="1ca01-125">Usage</span></span>

```dotnetcli
dotnet grpc add-file [options] <files>...
```

#### <a name="arguments"></a><span data-ttu-id="1ca01-126">Arguments</span><span class="sxs-lookup"><span data-stu-id="1ca01-126">Arguments</span></span>

| <span data-ttu-id="1ca01-127">Argument</span><span class="sxs-lookup"><span data-stu-id="1ca01-127">Argument</span></span> | <span data-ttu-id="1ca01-128">Description</span><span class="sxs-lookup"><span data-stu-id="1ca01-128">Description</span></span> |
|-|-|
| <span data-ttu-id="1ca01-129">files</span><span class="sxs-lookup"><span data-stu-id="1ca01-129">files</span></span> | <span data-ttu-id="1ca01-130">Références du fichier protobuf.</span><span class="sxs-lookup"><span data-stu-id="1ca01-130">The protobuf file references.</span></span> <span data-ttu-id="1ca01-131">Il peut s’agir d’un chemin d’accès à glob pour les fichiers protobuf locaux.</span><span class="sxs-lookup"><span data-stu-id="1ca01-131">These can be a path to glob for local protobuf files.</span></span> |

#### <a name="options"></a><span data-ttu-id="1ca01-132">Options</span><span class="sxs-lookup"><span data-stu-id="1ca01-132">Options</span></span>

| <span data-ttu-id="1ca01-133">Option Short</span><span class="sxs-lookup"><span data-stu-id="1ca01-133">Short option</span></span> | <span data-ttu-id="1ca01-134">Option longue</span><span class="sxs-lookup"><span data-stu-id="1ca01-134">Long option</span></span> | <span data-ttu-id="1ca01-135">Description</span><span class="sxs-lookup"><span data-stu-id="1ca01-135">Description</span></span> |
|-|-|-|
| <span data-ttu-id="1ca01-136">-p</span><span class="sxs-lookup"><span data-stu-id="1ca01-136">-p</span></span> | <span data-ttu-id="1ca01-137">--projet</span><span class="sxs-lookup"><span data-stu-id="1ca01-137">--project</span></span> | <span data-ttu-id="1ca01-138">Chemin d’accès au fichier projet sur lequel effectuer l’opération.</span><span class="sxs-lookup"><span data-stu-id="1ca01-138">The path to the project file to operate on.</span></span> <span data-ttu-id="1ca01-139">Si aucun fichier n’est spécifié, la commande effectue une recherche dans le répertoire actif.</span><span class="sxs-lookup"><span data-stu-id="1ca01-139">If a file is not specified, the command searches the current directory for one.</span></span>
| <span data-ttu-id="1ca01-140">-S</span><span class="sxs-lookup"><span data-stu-id="1ca01-140">-s</span></span> | <span data-ttu-id="1ca01-141">--services</span><span class="sxs-lookup"><span data-stu-id="1ca01-141">--services</span></span> | <span data-ttu-id="1ca01-142">Type des services gRPC qui doivent être générés.</span><span class="sxs-lookup"><span data-stu-id="1ca01-142">The type of gRPC services that should be generated.</span></span> <span data-ttu-id="1ca01-143">Si `Default` est spécifié, `Both` est utilisé pour les projets Web et `Client` est utilisé pour les projets non Web.</span><span class="sxs-lookup"><span data-stu-id="1ca01-143">If `Default` is specified, `Both` is used for Web projects and `Client` is used for non-Web projects.</span></span> <span data-ttu-id="1ca01-144">Les valeurs acceptées sont `Both` , `Client` ,, `Default` `None` , `Server` .</span><span class="sxs-lookup"><span data-stu-id="1ca01-144">Accepted values are `Both`, `Client`, `Default`, `None`, `Server`.</span></span>
| <span data-ttu-id="1ca01-145">-i</span><span class="sxs-lookup"><span data-stu-id="1ca01-145">-i</span></span> | <span data-ttu-id="1ca01-146">--Import-dirs</span><span class="sxs-lookup"><span data-stu-id="1ca01-146">--additional-import-dirs</span></span> | <span data-ttu-id="1ca01-147">Répertoires supplémentaires à utiliser lors de la résolution des importations des fichiers protobuf.</span><span class="sxs-lookup"><span data-stu-id="1ca01-147">Additional directories to be used when resolving imports for the protobuf files.</span></span> <span data-ttu-id="1ca01-148">Il s’agit d’une liste de chemins séparés par des points-virgules.</span><span class="sxs-lookup"><span data-stu-id="1ca01-148">This is a semicolon separated list of paths.</span></span>
| | <span data-ttu-id="1ca01-149">--accès</span><span class="sxs-lookup"><span data-stu-id="1ca01-149">--access</span></span> | <span data-ttu-id="1ca01-150">Modificateur d’accès à utiliser pour les classes C# générées.</span><span class="sxs-lookup"><span data-stu-id="1ca01-150">The access modifier to use for the generated C# classes.</span></span> <span data-ttu-id="1ca01-151">La valeur par défaut est `Public`.</span><span class="sxs-lookup"><span data-stu-id="1ca01-151">The default value is `Public`.</span></span> <span data-ttu-id="1ca01-152">Les valeurs acceptées sont `Internal` et `Public`.</span><span class="sxs-lookup"><span data-stu-id="1ca01-152">Accepted values are `Internal` and `Public`.</span></span>

### <a name="add-url"></a><span data-ttu-id="1ca01-153">Ajouter une URL</span><span class="sxs-lookup"><span data-stu-id="1ca01-153">Add URL</span></span>

<span data-ttu-id="1ca01-154">La `add-url` commande permet d’ajouter un fichier distant spécifié par une URL source en tant que référence Protobuf.</span><span class="sxs-lookup"><span data-stu-id="1ca01-154">The `add-url` command is used to add a remote file specified by an source URL as Protobuf reference.</span></span> <span data-ttu-id="1ca01-155">Un chemin d’accès de fichier doit être fourni pour spécifier l’emplacement de téléchargement du fichier distant.</span><span class="sxs-lookup"><span data-stu-id="1ca01-155">A file path must be provided to specify where to download the remote file.</span></span> <span data-ttu-id="1ca01-156">Le chemin d’accès du fichier peut être relatif au répertoire actif ou à un chemin d’accès absolu.</span><span class="sxs-lookup"><span data-stu-id="1ca01-156">The file path can be relative to the current directory or an absolute path.</span></span> <span data-ttu-id="1ca01-157">Si le chemin d’accès au fichier se trouve en dehors du répertoire du projet, un `Link` élément est ajouté pour afficher le fichier sous le dossier virtuel `Protos` dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1ca01-157">If the file path is outside the project directory, a `Link` element is added to display the file under the virtual folder `Protos` in Visual Studio.</span></span>

### <a name="usage"></a><span data-ttu-id="1ca01-158">Utilisation</span><span class="sxs-lookup"><span data-stu-id="1ca01-158">Usage</span></span>

```dotnetcli
dotnet-grpc add-url [options] <url>
```

#### <a name="arguments"></a><span data-ttu-id="1ca01-159">Arguments</span><span class="sxs-lookup"><span data-stu-id="1ca01-159">Arguments</span></span>

| <span data-ttu-id="1ca01-160">Argument</span><span class="sxs-lookup"><span data-stu-id="1ca01-160">Argument</span></span> | <span data-ttu-id="1ca01-161">Description</span><span class="sxs-lookup"><span data-stu-id="1ca01-161">Description</span></span> |
|-|-|
| <span data-ttu-id="1ca01-162">url</span><span class="sxs-lookup"><span data-stu-id="1ca01-162">url</span></span> | <span data-ttu-id="1ca01-163">URL d’un fichier protobuf distant.</span><span class="sxs-lookup"><span data-stu-id="1ca01-163">The URL to a remote protobuf file.</span></span> |

#### <a name="options"></a><span data-ttu-id="1ca01-164">Options</span><span class="sxs-lookup"><span data-stu-id="1ca01-164">Options</span></span>

| <span data-ttu-id="1ca01-165">Option Short</span><span class="sxs-lookup"><span data-stu-id="1ca01-165">Short option</span></span> | <span data-ttu-id="1ca01-166">Option longue</span><span class="sxs-lookup"><span data-stu-id="1ca01-166">Long option</span></span> | <span data-ttu-id="1ca01-167">Description</span><span class="sxs-lookup"><span data-stu-id="1ca01-167">Description</span></span> |
|-|-|-|
| <span data-ttu-id="1ca01-168">-o</span><span class="sxs-lookup"><span data-stu-id="1ca01-168">-o</span></span> | <span data-ttu-id="1ca01-169">--output</span><span class="sxs-lookup"><span data-stu-id="1ca01-169">--output</span></span> | <span data-ttu-id="1ca01-170">Spécifie le chemin de téléchargement du fichier protobuf distant.</span><span class="sxs-lookup"><span data-stu-id="1ca01-170">Specifies the download path for the remote protobuf file.</span></span> <span data-ttu-id="1ca01-171">C'est une option obligatoire.</span><span class="sxs-lookup"><span data-stu-id="1ca01-171">This is a required option.</span></span>
| <span data-ttu-id="1ca01-172">-p</span><span class="sxs-lookup"><span data-stu-id="1ca01-172">-p</span></span> | <span data-ttu-id="1ca01-173">--projet</span><span class="sxs-lookup"><span data-stu-id="1ca01-173">--project</span></span> | <span data-ttu-id="1ca01-174">Chemin d’accès au fichier projet sur lequel effectuer l’opération.</span><span class="sxs-lookup"><span data-stu-id="1ca01-174">The path to the project file to operate on.</span></span> <span data-ttu-id="1ca01-175">Si aucun fichier n’est spécifié, la commande effectue une recherche dans le répertoire actif.</span><span class="sxs-lookup"><span data-stu-id="1ca01-175">If a file is not specified, the command searches the current directory for one.</span></span>
| <span data-ttu-id="1ca01-176">-S</span><span class="sxs-lookup"><span data-stu-id="1ca01-176">-s</span></span> | <span data-ttu-id="1ca01-177">--services</span><span class="sxs-lookup"><span data-stu-id="1ca01-177">--services</span></span> | <span data-ttu-id="1ca01-178">Type des services gRPC qui doivent être générés.</span><span class="sxs-lookup"><span data-stu-id="1ca01-178">The type of gRPC services that should be generated.</span></span> <span data-ttu-id="1ca01-179">Si `Default` est spécifié, `Both` est utilisé pour les projets Web et `Client` est utilisé pour les projets non Web.</span><span class="sxs-lookup"><span data-stu-id="1ca01-179">If `Default` is specified, `Both` is used for Web projects and `Client` is used for non-Web projects.</span></span> <span data-ttu-id="1ca01-180">Les valeurs acceptées sont `Both` , `Client` ,, `Default` `None` , `Server` .</span><span class="sxs-lookup"><span data-stu-id="1ca01-180">Accepted values are `Both`, `Client`, `Default`, `None`, `Server`.</span></span>
| <span data-ttu-id="1ca01-181">-i</span><span class="sxs-lookup"><span data-stu-id="1ca01-181">-i</span></span> | <span data-ttu-id="1ca01-182">--Import-dirs</span><span class="sxs-lookup"><span data-stu-id="1ca01-182">--additional-import-dirs</span></span> | <span data-ttu-id="1ca01-183">Répertoires supplémentaires à utiliser lors de la résolution des importations des fichiers protobuf.</span><span class="sxs-lookup"><span data-stu-id="1ca01-183">Additional directories to be used when resolving imports for the protobuf files.</span></span> <span data-ttu-id="1ca01-184">Il s’agit d’une liste de chemins séparés par des points-virgules.</span><span class="sxs-lookup"><span data-stu-id="1ca01-184">This is a semicolon separated list of paths.</span></span>
| | <span data-ttu-id="1ca01-185">--accès</span><span class="sxs-lookup"><span data-stu-id="1ca01-185">--access</span></span> | <span data-ttu-id="1ca01-186">Modificateur d’accès à utiliser pour les classes C# générées.</span><span class="sxs-lookup"><span data-stu-id="1ca01-186">The access modifier to use for the generated C# classes.</span></span> <span data-ttu-id="1ca01-187">La valeur par défaut est `Public`.</span><span class="sxs-lookup"><span data-stu-id="1ca01-187">Default value is `Public`.</span></span> <span data-ttu-id="1ca01-188">Les valeurs acceptées sont `Internal` et `Public`.</span><span class="sxs-lookup"><span data-stu-id="1ca01-188">Accepted values are `Internal` and `Public`.</span></span>

## <a name="remove"></a><span data-ttu-id="1ca01-189">Supprimer</span><span class="sxs-lookup"><span data-stu-id="1ca01-189">Remove</span></span>

<span data-ttu-id="1ca01-190">La `remove` commande permet de supprimer les références Protobuf du fichier *. csproj* .</span><span class="sxs-lookup"><span data-stu-id="1ca01-190">The `remove` command is used to remove Protobuf references from the *.csproj* file.</span></span> <span data-ttu-id="1ca01-191">La commande accepte les arguments de chemin d’accès et les URL source comme arguments.</span><span class="sxs-lookup"><span data-stu-id="1ca01-191">The command accepts path arguments and source URLs as arguments.</span></span> <span data-ttu-id="1ca01-192">L’outil :</span><span class="sxs-lookup"><span data-stu-id="1ca01-192">The tool:</span></span>

* <span data-ttu-id="1ca01-193">Supprime uniquement la référence Protobuf.</span><span class="sxs-lookup"><span data-stu-id="1ca01-193">Only removes the Protobuf reference.</span></span>
* <span data-ttu-id="1ca01-194">Ne supprime pas le fichier *. proto* , même s’il a été téléchargé à l’origine à partir d’une URL distante.</span><span class="sxs-lookup"><span data-stu-id="1ca01-194">Does not delete the *.proto* file, even if it was originally downloaded from a remote URL.</span></span>

### <a name="usage"></a><span data-ttu-id="1ca01-195">Utilisation</span><span class="sxs-lookup"><span data-stu-id="1ca01-195">Usage</span></span>

```dotnetcli
dotnet-grpc remove [options] <references>...
```

### <a name="arguments"></a><span data-ttu-id="1ca01-196">Arguments</span><span class="sxs-lookup"><span data-stu-id="1ca01-196">Arguments</span></span>

| <span data-ttu-id="1ca01-197">Argument</span><span class="sxs-lookup"><span data-stu-id="1ca01-197">Argument</span></span> | <span data-ttu-id="1ca01-198">Description</span><span class="sxs-lookup"><span data-stu-id="1ca01-198">Description</span></span> |
|-|-|
| <span data-ttu-id="1ca01-199">references</span><span class="sxs-lookup"><span data-stu-id="1ca01-199">references</span></span> | <span data-ttu-id="1ca01-200">URL ou chemins de fichier des références protobuf à supprimer.</span><span class="sxs-lookup"><span data-stu-id="1ca01-200">The URLs or file paths of the protobuf references to remove.</span></span> |

### <a name="options"></a><span data-ttu-id="1ca01-201">Options</span><span class="sxs-lookup"><span data-stu-id="1ca01-201">Options</span></span>

| <span data-ttu-id="1ca01-202">Option Short</span><span class="sxs-lookup"><span data-stu-id="1ca01-202">Short option</span></span> | <span data-ttu-id="1ca01-203">Option longue</span><span class="sxs-lookup"><span data-stu-id="1ca01-203">Long option</span></span> | <span data-ttu-id="1ca01-204">Description</span><span class="sxs-lookup"><span data-stu-id="1ca01-204">Description</span></span> |
|-|-|-|
| <span data-ttu-id="1ca01-205">-p</span><span class="sxs-lookup"><span data-stu-id="1ca01-205">-p</span></span> | <span data-ttu-id="1ca01-206">--projet</span><span class="sxs-lookup"><span data-stu-id="1ca01-206">--project</span></span> | <span data-ttu-id="1ca01-207">Chemin d’accès au fichier projet sur lequel effectuer l’opération.</span><span class="sxs-lookup"><span data-stu-id="1ca01-207">The path to the project file to operate on.</span></span> <span data-ttu-id="1ca01-208">Si aucun fichier n’est spécifié, la commande effectue une recherche dans le répertoire actif.</span><span class="sxs-lookup"><span data-stu-id="1ca01-208">If a file is not specified, the command searches the current directory for one.</span></span>

## <a name="refresh"></a><span data-ttu-id="1ca01-209">Actualiser</span><span class="sxs-lookup"><span data-stu-id="1ca01-209">Refresh</span></span>

<span data-ttu-id="1ca01-210">La `refresh` commande est utilisée pour mettre à jour une référence distante avec le contenu le plus récent à partir de l’URL source.</span><span class="sxs-lookup"><span data-stu-id="1ca01-210">The `refresh` command is used to update a remote reference with the latest content from the source URL.</span></span> <span data-ttu-id="1ca01-211">Le chemin d’accès au fichier de téléchargement et l’URL source peuvent être utilisés pour spécifier la référence à mettre à jour.</span><span class="sxs-lookup"><span data-stu-id="1ca01-211">Both the download file path and the source URL can be used to specify the reference to be updated.</span></span> <span data-ttu-id="1ca01-212">Remarque :</span><span class="sxs-lookup"><span data-stu-id="1ca01-212">Note:</span></span>

* <span data-ttu-id="1ca01-213">Les hachages du contenu du fichier sont comparés pour déterminer si le fichier local doit être mis à jour.</span><span class="sxs-lookup"><span data-stu-id="1ca01-213">The hashes of the file contents are compared to determine whether the local file should be updated.</span></span>
* <span data-ttu-id="1ca01-214">Aucune information d’horodatage n’est comparée.</span><span class="sxs-lookup"><span data-stu-id="1ca01-214">No timestamp information is compared.</span></span>

<span data-ttu-id="1ca01-215">L’outil remplace toujours le fichier local par le fichier distant si une mise à jour est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="1ca01-215">The tool always replaces the local file with the remote file if an update is needed.</span></span>

### <a name="usage"></a><span data-ttu-id="1ca01-216">Utilisation</span><span class="sxs-lookup"><span data-stu-id="1ca01-216">Usage</span></span>

```dotnetcli
dotnet-grpc refresh [options] [<references>...]
```

### <a name="arguments"></a><span data-ttu-id="1ca01-217">Arguments</span><span class="sxs-lookup"><span data-stu-id="1ca01-217">Arguments</span></span>

| <span data-ttu-id="1ca01-218">Argument</span><span class="sxs-lookup"><span data-stu-id="1ca01-218">Argument</span></span> | <span data-ttu-id="1ca01-219">Description</span><span class="sxs-lookup"><span data-stu-id="1ca01-219">Description</span></span> |
|-|-|
| <span data-ttu-id="1ca01-220">references</span><span class="sxs-lookup"><span data-stu-id="1ca01-220">references</span></span> | <span data-ttu-id="1ca01-221">URL ou chemins d’accès aux références protobuf distantes qui doivent être mises à jour.</span><span class="sxs-lookup"><span data-stu-id="1ca01-221">The URLs or file paths to remote protobuf references that should be updated.</span></span> <span data-ttu-id="1ca01-222">Laissez cet argument vide pour actualiser toutes les références distantes.</span><span class="sxs-lookup"><span data-stu-id="1ca01-222">Leave this argument empty to refresh all remote references.</span></span> |

### <a name="options"></a><span data-ttu-id="1ca01-223">Options</span><span class="sxs-lookup"><span data-stu-id="1ca01-223">Options</span></span>

| <span data-ttu-id="1ca01-224">Option Short</span><span class="sxs-lookup"><span data-stu-id="1ca01-224">Short option</span></span> | <span data-ttu-id="1ca01-225">Option longue</span><span class="sxs-lookup"><span data-stu-id="1ca01-225">Long option</span></span> | <span data-ttu-id="1ca01-226">Description</span><span class="sxs-lookup"><span data-stu-id="1ca01-226">Description</span></span> |
|-|-|-|
| <span data-ttu-id="1ca01-227">-p</span><span class="sxs-lookup"><span data-stu-id="1ca01-227">-p</span></span> | <span data-ttu-id="1ca01-228">--projet</span><span class="sxs-lookup"><span data-stu-id="1ca01-228">--project</span></span> | <span data-ttu-id="1ca01-229">Chemin d’accès au fichier projet sur lequel effectuer l’opération.</span><span class="sxs-lookup"><span data-stu-id="1ca01-229">The path to the project file to operate on.</span></span> <span data-ttu-id="1ca01-230">Si aucun fichier n’est spécifié, la commande effectue une recherche dans le répertoire actif.</span><span class="sxs-lookup"><span data-stu-id="1ca01-230">If a file is not specified, the command searches the current directory for one.</span></span>
| | <span data-ttu-id="1ca01-231">--à sec-exécution</span><span class="sxs-lookup"><span data-stu-id="1ca01-231">--dry-run</span></span> | <span data-ttu-id="1ca01-232">Génère une liste des fichiers qui seraient mis à jour sans télécharger de nouveau contenu.</span><span class="sxs-lookup"><span data-stu-id="1ca01-232">Outputs a list of files that would be updated without downloading any new content.</span></span>

## <a name="list"></a><span data-ttu-id="1ca01-233">Liste</span><span class="sxs-lookup"><span data-stu-id="1ca01-233">List</span></span>

<span data-ttu-id="1ca01-234">La `list` commande est utilisée pour afficher toutes les références Protobuf dans le fichier projet.</span><span class="sxs-lookup"><span data-stu-id="1ca01-234">The `list` command is used to display all the Protobuf references in the project file.</span></span> <span data-ttu-id="1ca01-235">Si toutes les valeurs d’une colonne sont des valeurs par défaut, la colonne peut être omise.</span><span class="sxs-lookup"><span data-stu-id="1ca01-235">If all values of a column are default values, the column may be omitted.</span></span>

### <a name="usage"></a><span data-ttu-id="1ca01-236">Usage</span><span class="sxs-lookup"><span data-stu-id="1ca01-236">Usage</span></span>

```dotnetcli
dotnet-grpc list [options]
```

### <a name="options"></a><span data-ttu-id="1ca01-237">Options</span><span class="sxs-lookup"><span data-stu-id="1ca01-237">Options</span></span>

| <span data-ttu-id="1ca01-238">Option Short</span><span class="sxs-lookup"><span data-stu-id="1ca01-238">Short option</span></span> | <span data-ttu-id="1ca01-239">Option longue</span><span class="sxs-lookup"><span data-stu-id="1ca01-239">Long option</span></span> | <span data-ttu-id="1ca01-240">Description</span><span class="sxs-lookup"><span data-stu-id="1ca01-240">Description</span></span> |
|-|-|-|
| <span data-ttu-id="1ca01-241">-p</span><span class="sxs-lookup"><span data-stu-id="1ca01-241">-p</span></span> | <span data-ttu-id="1ca01-242">--projet</span><span class="sxs-lookup"><span data-stu-id="1ca01-242">--project</span></span> | <span data-ttu-id="1ca01-243">Chemin d’accès au fichier projet sur lequel effectuer l’opération.</span><span class="sxs-lookup"><span data-stu-id="1ca01-243">The path to the project file to operate on.</span></span> <span data-ttu-id="1ca01-244">Si aucun fichier n’est spécifié, la commande effectue une recherche dans le répertoire actif.</span><span class="sxs-lookup"><span data-stu-id="1ca01-244">If a file is not specified, the command searches the current directory for one.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1ca01-245">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="1ca01-245">Additional resources</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
