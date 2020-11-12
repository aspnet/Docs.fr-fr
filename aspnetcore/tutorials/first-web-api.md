---
title: 'Didacticiel : créer une API Web avec ASP.NET Core'
author: rick-anderson
description: Apprendre à créer une API web avec ASP.NET Core.
ms.author: riande
ms.custom: mvc, devx-track-js
ms.date: 08/13/2020
no-loc:
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
- ':::no-loc(Models):::'
uid: tutorials/first-web-api
ms.openlocfilehash: ab06f2d347d7f05e9c72a85e0c6e0fbc67fe48a7
ms.sourcegitcommit: 202144092067ea81be1dbb229329518d781dbdfb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/12/2020
ms.locfileid: "94550710"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="e2912-103">Didacticiel : créer une API Web avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e2912-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="e2912-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT), [Kirk Larkin](https://twitter.com/serpent5)et [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="e2912-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Kirk Larkin](https://twitter.com/serpent5), and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="e2912-105">Ce tutoriel décrit les principes fondamentaux liés à la génération d’une API web avec ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e2912-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

::: moniker range=">= aspnetcore-5.0"

<span data-ttu-id="e2912-106">Dans ce tutoriel, vous allez apprendre à :</span><span class="sxs-lookup"><span data-stu-id="e2912-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e2912-107">Créer un projet d’API web.</span><span class="sxs-lookup"><span data-stu-id="e2912-107">Create a web API project.</span></span>
> * <span data-ttu-id="e2912-108">Ajouter une classe de modèle et un contexte de base de données.</span><span class="sxs-lookup"><span data-stu-id="e2912-108">Add a model class and a database context.</span></span>
> * <span data-ttu-id="e2912-109">Générer automatiquement des modèles pour un contrôleur avec des méthodes CRUD.</span><span class="sxs-lookup"><span data-stu-id="e2912-109">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="e2912-110">Configurer le routage, les chemins d’URL et les valeurs de retour.</span><span class="sxs-lookup"><span data-stu-id="e2912-110">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="e2912-111">Appeler l’API web avec Postman</span><span class="sxs-lookup"><span data-stu-id="e2912-111">Call the web API with Postman.</span></span>

<span data-ttu-id="e2912-112">À la fin, vous disposez d’une API web qui peut gérer des tâches stockées dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="e2912-112">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="e2912-113">Vue d’ensemble</span><span class="sxs-lookup"><span data-stu-id="e2912-113">Overview</span></span>

<span data-ttu-id="e2912-114">Ce didacticiel crée l’API suivante :</span><span class="sxs-lookup"><span data-stu-id="e2912-114">This tutorial creates the following API:</span></span>

|<span data-ttu-id="e2912-115">API</span><span class="sxs-lookup"><span data-stu-id="e2912-115">API</span></span> | <span data-ttu-id="e2912-116">Description</span><span class="sxs-lookup"><span data-stu-id="e2912-116">Description</span></span> | <span data-ttu-id="e2912-117">Corps de la demande</span><span class="sxs-lookup"><span data-stu-id="e2912-117">Request body</span></span> | <span data-ttu-id="e2912-118">Response body</span><span class="sxs-lookup"><span data-stu-id="e2912-118">Response body</span></span> |
|--- | ---- | ---- | ---- |
|`GET /api/TodoItems` | <span data-ttu-id="e2912-119">Obtenir toutes les tâches</span><span class="sxs-lookup"><span data-stu-id="e2912-119">Get all to-do items</span></span> | <span data-ttu-id="e2912-120">Aucun</span><span class="sxs-lookup"><span data-stu-id="e2912-120">None</span></span> | <span data-ttu-id="e2912-121">Tableau de tâches</span><span class="sxs-lookup"><span data-stu-id="e2912-121">Array of to-do items</span></span>|
|`GET /api/TodoItems/{id}` | <span data-ttu-id="e2912-122">Obtenir un élément par ID</span><span class="sxs-lookup"><span data-stu-id="e2912-122">Get an item by ID</span></span> | <span data-ttu-id="e2912-123">Aucun</span><span class="sxs-lookup"><span data-stu-id="e2912-123">None</span></span> | <span data-ttu-id="e2912-124">Tâche</span><span class="sxs-lookup"><span data-stu-id="e2912-124">To-do item</span></span>|
|`POST /api/TodoItems` | <span data-ttu-id="e2912-125">Ajouter un nouvel élément</span><span class="sxs-lookup"><span data-stu-id="e2912-125">Add a new item</span></span> | <span data-ttu-id="e2912-126">Tâche</span><span class="sxs-lookup"><span data-stu-id="e2912-126">To-do item</span></span> | <span data-ttu-id="e2912-127">Tâche</span><span class="sxs-lookup"><span data-stu-id="e2912-127">To-do item</span></span> |
|`PUT /api/TodoItems/{id}` | <span data-ttu-id="e2912-128">Mettre à jour un élément existant &nbsp;</span><span class="sxs-lookup"><span data-stu-id="e2912-128">Update an existing item &nbsp;</span></span> | <span data-ttu-id="e2912-129">Tâche</span><span class="sxs-lookup"><span data-stu-id="e2912-129">To-do item</span></span> | <span data-ttu-id="e2912-130">Aucun</span><span class="sxs-lookup"><span data-stu-id="e2912-130">None</span></span> |
|<span data-ttu-id="e2912-131">`DELETE /api/TodoItems/{id}` &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="e2912-131">`DELETE /api/TodoItems/{id}` &nbsp; &nbsp;</span></span> | <span data-ttu-id="e2912-132">Supprimer un élément &nbsp;&nbsp;</span><span class="sxs-lookup"><span data-stu-id="e2912-132">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="e2912-133">None</span><span class="sxs-lookup"><span data-stu-id="e2912-133">None</span></span> | <span data-ttu-id="e2912-134">None</span><span class="sxs-lookup"><span data-stu-id="e2912-134">None</span></span>|

<span data-ttu-id="e2912-135">Le diagramme suivant illustre la conception de l’application.</span><span class="sxs-lookup"><span data-stu-id="e2912-135">The following diagram shows the design of the app.</span></span>

![Le client est représenté par une zone située à gauche.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="e2912-141">Prérequis</span><span class="sxs-lookup"><span data-stu-id="e2912-141">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e2912-142">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e2912-142">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-5.0.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="e2912-143">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e2912-143">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-5.0.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="e2912-144">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="e2912-144">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-5.0.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="e2912-145">Créer un projet web</span><span class="sxs-lookup"><span data-stu-id="e2912-145">Create a web project</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e2912-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e2912-146">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e2912-147">Dans le menu **fichier** , sélectionnez **nouveau** > **projet**.</span><span class="sxs-lookup"><span data-stu-id="e2912-147">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="e2912-148">Sélectionnez le modèle **Application web ASP.NET Core** et cliquez sur **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="e2912-148">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="e2912-149">Nommez le projet *TodoApi* et cliquez sur **Créer**.</span><span class="sxs-lookup"><span data-stu-id="e2912-149">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="e2912-150">Dans la boîte de dialogue **créer une application Web ASP.net Core** , vérifiez que **.net Core** et **ASP.net Core 5,0** sont sélectionnés.</span><span class="sxs-lookup"><span data-stu-id="e2912-150">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 5.0** are selected.</span></span> <span data-ttu-id="e2912-151">Sélectionnez le modèle **API** et cliquez sur **Créer**.</span><span class="sxs-lookup"><span data-stu-id="e2912-151">Select the **API** template and click **Create**.</span></span>

![Boîte de dialogue de nouveau projet dans VS](first-web-api/_static/5/vs.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="e2912-153">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e2912-153">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e2912-154">Ouvrez le [Terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="e2912-154">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="e2912-155">Définissez les répertoires (`cd`) sur le dossier destiné à contenir le dossier du projet.</span><span class="sxs-lookup"><span data-stu-id="e2912-155">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="e2912-156">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="e2912-156">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoApi
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

* <span data-ttu-id="e2912-157">Quand une boîte de dialogue vous demande si vous souhaitez ajouter les composants nécessaires au projet, sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="e2912-157">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="e2912-158">Les commandes précédentes :</span><span class="sxs-lookup"><span data-stu-id="e2912-158">The preceding commands:</span></span>

  * <span data-ttu-id="e2912-159">Créent un projet API Web et l’ouvrent dans Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e2912-159">Creates a new web API project and opens it in Visual Studio Code.</span></span>
  * <span data-ttu-id="e2912-160">Ajoutent les packages NuGet qui sont requis dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="e2912-160">Adds the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="e2912-161">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="e2912-161">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e2912-162">Sélectionnez **Fichier** > **Nouvelle solution**.</span><span class="sxs-lookup"><span data-stu-id="e2912-162">Select **File** > **New Solution**.</span></span>

  ![macOS - Nouvelle solution](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="e2912-164">Dans Visual Studio pour Mac antérieure à la version 8,6, sélectionnez API de l’application **.net Core**  >  **App**  >  **API**  >  **suivant**.</span><span class="sxs-lookup"><span data-stu-id="e2912-164">In Visual Studio for Mac earlier than version 8.6, select **.NET Core** > **App** > **API** > **Next**.</span></span> <span data-ttu-id="e2912-165">Dans la version 8,6 ou une version ultérieure, sélectionnez application **Web et**  >  **App**  >  **API** application console  >  **suivant**.</span><span class="sxs-lookup"><span data-stu-id="e2912-165">In version 8.6 or later, select **Web and Console** > **App** > **API** > **Next**.</span></span>

  ![sélection du modèle d’API macOS](first-web-api-mac/_static/api_template.png)

* <span data-ttu-id="e2912-167">Dans la boîte de dialogue **configurer la nouvelle API Web ASP.net Core** , sélectionnez la version la plus récente de .net Core 5. x **Target Framework**.</span><span class="sxs-lookup"><span data-stu-id="e2912-167">In the **Configure the new ASP.NET Core Web API** dialog, select the latest .NET Core 5.x **Target Framework**.</span></span> <span data-ttu-id="e2912-168">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="e2912-168">Select **Next**.</span></span>

* <span data-ttu-id="e2912-169">Entrez *TodoApi* comme **Nom du projet** , puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="e2912-169">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![boîte de dialogue de configuration](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

<span data-ttu-id="e2912-171">Ouvrez un terminal de commande dans le dossier de projet, puis exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="e2912-171">Open a command terminal in the project folder and run the following commands:</span></span>

   ```dotnetcli
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   ```

---

### <a name="test-the-project"></a><span data-ttu-id="e2912-172">Tester le projet</span><span class="sxs-lookup"><span data-stu-id="e2912-172">Test the project</span></span>

<span data-ttu-id="e2912-173">Le modèle de projet crée une `WeatherForecast` API avec prise en charge de [Swagger](xref:tutorials/web-api-help-pages-using-swagger).</span><span class="sxs-lookup"><span data-stu-id="e2912-173">The project template creates a `WeatherForecast` API with support for [Swagger](xref:tutorials/web-api-help-pages-using-swagger).</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e2912-174">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e2912-174">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e2912-175">Appuyez sur Ctrl+F5 pour exécuter sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="e2912-175">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="e2912-176">Lancements de Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="e2912-176">Visual Studio launches:</span></span>

* <span data-ttu-id="e2912-177">Serveur Web IIS Express.</span><span class="sxs-lookup"><span data-stu-id="e2912-177">The IIS Express web server.</span></span>
* <span data-ttu-id="e2912-178">Le navigateur par défaut et navigue vers `https://localhost:<port>/https://localhost:5001/swagger/index.html` , où `<port>` est un numéro de port choisi de façon aléatoire.</span><span class="sxs-lookup"><span data-stu-id="e2912-178">The default browser and navigates to `https://localhost:<port>/https://localhost:5001/swagger/index.html`, where `<port>` is a randomly chosen port number.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="e2912-179">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e2912-179">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/trustCertVSC.md)]

<span data-ttu-id="e2912-180">Appuyez sur Ctrl+F5 pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="e2912-180">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="e2912-181">Dans un navigateur, accédez à l’URL suivante : [https://localhost:5001/swagger](https://localhost:5001/swagger)</span><span class="sxs-lookup"><span data-stu-id="e2912-181">In a browser, go to following URL: [https://localhost:5001/swagger](https://localhost:5001/swagger)</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="e2912-182">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="e2912-182">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="e2912-183">Sélectionnez **exécuter**  >  **Démarrer le débogage** pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="e2912-183">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="e2912-184">Visual Studio pour Mac lance un navigateur et accède à `https://localhost:<port>`, où `<port>` est un numéro de port choisi de manière aléatoire.</span><span class="sxs-lookup"><span data-stu-id="e2912-184">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="e2912-185">Une erreur HTTP 404 (introuvable) est retournée.</span><span class="sxs-lookup"><span data-stu-id="e2912-185">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="e2912-186">Ajoutez `/swagger` à l’URL (définissez-la sur `https://localhost:<port>/swagger`).</span><span class="sxs-lookup"><span data-stu-id="e2912-186">Append `/swagger` to the URL (change the URL to `https://localhost:<port>/swagger`).</span></span>

---

<span data-ttu-id="e2912-187">La page Swagger `/swagger/index.html` s’affiche.</span><span class="sxs-lookup"><span data-stu-id="e2912-187">The Swagger page `/swagger/index.html` is displayed.</span></span> <span data-ttu-id="e2912-188">Sélectionnez **faire**  >  **essayer** l'  >  **exécution**.</span><span class="sxs-lookup"><span data-stu-id="e2912-188">Select **GET** > **Try it out** > **Execute**.</span></span> <span data-ttu-id="e2912-189">La page affiche :</span><span class="sxs-lookup"><span data-stu-id="e2912-189">The page displays:</span></span>

* <span data-ttu-id="e2912-190">Commande de [boucle](https://curl.haxx.se/) pour tester l’API WeatherForecast.</span><span class="sxs-lookup"><span data-stu-id="e2912-190">The [Curl](https://curl.haxx.se/) command to test the WeatherForecast API.</span></span>
* <span data-ttu-id="e2912-191">URL pour tester l’API WeatherForecast.</span><span class="sxs-lookup"><span data-stu-id="e2912-191">The URL to test the WeatherForecast API.</span></span>
* <span data-ttu-id="e2912-192">Code, corps et en-têtes de la réponse.</span><span class="sxs-lookup"><span data-stu-id="e2912-192">The response code, body, and headers.</span></span>
* <span data-ttu-id="e2912-193">Zone de liste déroulante avec les types de média et l’exemple de valeur et de schéma.</span><span class="sxs-lookup"><span data-stu-id="e2912-193">A drop down list box with media types and the example value and schema.</span></span>

<!-- Review: Do we care the IE generates several errors. It shows the data, but with  Unrecognized response type; displaying content as text.
-->
<span data-ttu-id="e2912-194">Swagger est utilisé pour générer une documentation utile et des pages d’aide pour les API Web.</span><span class="sxs-lookup"><span data-stu-id="e2912-194">Swagger is used to generate useful documentation and help pages for web APIs.</span></span> <span data-ttu-id="e2912-195">Ce didacticiel se concentre sur la création d’une API Web.</span><span class="sxs-lookup"><span data-stu-id="e2912-195">This tutorial focuses on creating a web API.</span></span> <span data-ttu-id="e2912-196">Pour plus d’informations sur Swagger, consultez <xref:tutorials/web-api-help-pages-using-swagger> .</span><span class="sxs-lookup"><span data-stu-id="e2912-196">For more information on Swagger, see <xref:tutorials/web-api-help-pages-using-swagger>.</span></span>

<span data-ttu-id="e2912-197">Copiez et collez l’URL de la **requête** dans le navigateur :  `https://localhost:<port>/WeatherForecast`</span><span class="sxs-lookup"><span data-stu-id="e2912-197">Copy and past the **Request URL** in the browser:  `https://localhost:<port>/WeatherForecast`</span></span>

<span data-ttu-id="e2912-198">Un code JSON similaire au suivant est retourné :</span><span class="sxs-lookup"><span data-stu-id="e2912-198">JSON similar to the following is returned:</span></span>

```json
[
    {
        "date": "2019-07-16T19:04:05.7257911-06:00",
        "temperatureC": 52,
        "temperatureF": 125,
        "summary": "Mild"
    },
    {
        "date": "2019-07-17T19:04:05.7258461-06:00",
        "temperatureC": 36,
        "temperatureF": 96,
        "summary": "Warm"
    },
    {
        "date": "2019-07-18T19:04:05.7258467-06:00",
        "temperatureC": 39,
        "temperatureF": 102,
        "summary": "Cool"
    },
    {
        "date": "2019-07-19T19:04:05.7258471-06:00",
        "temperatureC": 10,
        "temperatureF": 49,
        "summary": "Bracing"
    },
    {
        "date": "2019-07-20T19:04:05.7258474-06:00",
        "temperatureC": -1,
        "temperatureF": 31,
        "summary": "Chilly"
    }
]
```

### <a name="update-the-launchurl"></a><span data-ttu-id="e2912-199">Mettre à jour launchUrl</span><span class="sxs-lookup"><span data-stu-id="e2912-199">Update the launchUrl</span></span>

<span data-ttu-id="e2912-200">Dans *Properties\launchSettings.js* , mettez à jour `launchUrl` de `"swagger"` vers `"api/TodoItems"` :</span><span class="sxs-lookup"><span data-stu-id="e2912-200">In *Properties\launchSettings.json* , update `launchUrl` from `"swagger"` to `"api/TodoItems"`:</span></span>

```json
"launchUrl": "api/TodoItems",
```

<span data-ttu-id="e2912-201">Comme Swagger a été supprimé, le balisage précédent modifie l’URL qui est lancée vers la méthode d’extraction du contrôleur ajoutée dans les sections suivantes.</span><span class="sxs-lookup"><span data-stu-id="e2912-201">Because Swagger has been removed, the preceding markup changes the URL that is launched to the GET method of the controller added in the following sections.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="e2912-202">Ajouter une classe de modèle</span><span class="sxs-lookup"><span data-stu-id="e2912-202">Add a model class</span></span>

<span data-ttu-id="e2912-203">Un *modèle* est un ensemble de classes qui représentent les données gérées par l’application.</span><span class="sxs-lookup"><span data-stu-id="e2912-203">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="e2912-204">Le modèle pour cette application est une classe `TodoItem` unique.</span><span class="sxs-lookup"><span data-stu-id="e2912-204">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e2912-205">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e2912-205">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e2912-206">Dans l’ **Explorateur de solutions** , cliquez avec le bouton droit sur le projet.</span><span class="sxs-lookup"><span data-stu-id="e2912-206">In **Solution Explorer** , right-click the project.</span></span> <span data-ttu-id="e2912-207">Sélectionnez **Ajouter**  >  **un nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="e2912-207">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="e2912-208">Nommez le dossier *:::no-loc(Models):::* .</span><span class="sxs-lookup"><span data-stu-id="e2912-208">Name the folder *:::no-loc(Models):::*.</span></span>

* <span data-ttu-id="e2912-209">Cliquez avec le bouton droit sur le *:::no-loc(Models):::* dossier et sélectionnez **Ajouter** une  >  **classe**.</span><span class="sxs-lookup"><span data-stu-id="e2912-209">Right-click the *:::no-loc(Models):::* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="e2912-210">Nommez la classe *TodoItem* et sélectionnez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="e2912-210">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="e2912-211">Remplacez le code du modèle par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="e2912-211">Replace the template code with the following:</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="e2912-212">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e2912-212">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e2912-213">Ajoutez un dossier nommé *:::no-loc(Models):::* .</span><span class="sxs-lookup"><span data-stu-id="e2912-213">Add a folder named *:::no-loc(Models):::*.</span></span>

* <span data-ttu-id="e2912-214">Ajoutez une `TodoItem` classe au dossier à l' *:::no-loc(Models):::* aide du code suivant :</span><span class="sxs-lookup"><span data-stu-id="e2912-214">Add a `TodoItem` class to the *:::no-loc(Models):::* folder with the following code:</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="e2912-215">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="e2912-215">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e2912-216">Cliquez avec le bouton droit sur le projet.</span><span class="sxs-lookup"><span data-stu-id="e2912-216">Right-click the project.</span></span> <span data-ttu-id="e2912-217">Sélectionnez **Ajouter**  >  **un nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="e2912-217">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="e2912-218">Nommez le dossier *:::no-loc(Models):::* .</span><span class="sxs-lookup"><span data-stu-id="e2912-218">Name the folder *:::no-loc(Models):::*.</span></span>

  ![nouveau dossier](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="e2912-220">Cliquez avec le bouton droit sur le *:::no-loc(Models):::* dossier, puis sélectionnez **Ajouter** > **un nouveau fichier** > **General** > **classe générale vide**.</span><span class="sxs-lookup"><span data-stu-id="e2912-220">Right-click the *:::no-loc(Models):::* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="e2912-221">Nommez la classe *TodoItem* et cliquez sur **Nouveau**.</span><span class="sxs-lookup"><span data-stu-id="e2912-221">Name the class *TodoItem* , and then click **New**.</span></span>

* <span data-ttu-id="e2912-222">Remplacez le code du modèle par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="e2912-222">Replace the template code with the following:</span></span>

---

  [!code-csharp[](first-web-api/samples/5.x/TodoApi/:::no-loc(Models):::/TodoItem.cs?name=snippet)]

<span data-ttu-id="e2912-223">La propriété `Id` fonctionne comme la clé unique dans une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="e2912-223">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="e2912-224">Les classes de modèle peuvent se trouver n’importe où dans le projet, mais le *:::no-loc(Models):::* dossier est utilisé par Convention.</span><span class="sxs-lookup"><span data-stu-id="e2912-224">Model classes can go anywhere in the project, but the *:::no-loc(Models):::* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="e2912-225">Ajouter un contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="e2912-225">Add a database context</span></span>

<span data-ttu-id="e2912-226">Le *contexte de base de données* est la classe principale qui coordonne les fonctionnalités d’Entity Framework pour un modèle de données.</span><span class="sxs-lookup"><span data-stu-id="e2912-226">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="e2912-227">Cette classe est créée en dérivant de la classe <xref:Microsoft.EntityFrameworkCore.DbContext?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="e2912-227">This class is created by deriving from the <xref:Microsoft.EntityFrameworkCore.DbContext?displayProperty=fullName> class.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e2912-228">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e2912-228">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-nuget-packages"></a><span data-ttu-id="e2912-229">Ajouter des packages NuGet</span><span class="sxs-lookup"><span data-stu-id="e2912-229">Add NuGet packages</span></span>

* <span data-ttu-id="e2912-230">Dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet > Gérer les packages NuGet pour la solution**.</span><span class="sxs-lookup"><span data-stu-id="e2912-230">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
* <span data-ttu-id="e2912-231">Sélectionnez l’onglet **Parcourir** , puis entrez \* \* Microsoft.</span><span class="sxs-lookup"><span data-stu-id="e2912-231">Select the **Browse** tab, and then enter \*\*Microsoft.</span></span>
<span data-ttu-id="e2912-232">**EntityFrameworkCore. SqlServer** dans la zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="e2912-232">**EntityFrameworkCore.SqlServer** in the search box.</span></span>
<!-- https://github.com/dotnet/AspNetCore.Docs/issues/19782 Delete this line at RTM -->
* <span data-ttu-id="e2912-233">Activez la case à cocher inclure la version **préliminaire** afin que la version 5,0 RC soit disponible.</span><span class="sxs-lookup"><span data-stu-id="e2912-233">Select the **Include prerelease** checkbox so the 5.0 RC version is available.</span></span> 
* <span data-ttu-id="e2912-234">Dans le volet gauche, sélectionnez **Microsoft. EntityFrameworkCore. SqlServer** .</span><span class="sxs-lookup"><span data-stu-id="e2912-234">Select **Microsoft.EntityFrameworkCore.SqlServer** in the left pane.</span></span>
* <span data-ttu-id="e2912-235">Cochez la case **Projet** dans le volet droit, puis sélectionnez **Installer**.</span><span class="sxs-lookup"><span data-stu-id="e2912-235">Select the **Project** check box in the right pane and then select **Install**.</span></span>
* <span data-ttu-id="e2912-236">Utilisez les instructions précédentes pour ajouter le package NuGet **Microsoft. EntityFrameworkCore. InMemory** .</span><span class="sxs-lookup"><span data-stu-id="e2912-236">Use the preceding instructions to add the **Microsoft.EntityFrameworkCore.InMemory** NuGet package.</span></span>

<!-- https://github.com/dotnet/AspNetCore.Docs/issues/19782 Update this image at RTM -->
<span data-ttu-id="e2912-237">![Gestionnaire de package NuGet](first-web-api/_static/5/vsNuGet.png)</span><span class="sxs-lookup"><span data-stu-id="e2912-237">![NuGet Package Manager](first-web-api/_static/5/vsNuGet.png)</span></span>

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="e2912-238">Ajouter le contexte de base de données TodoContext</span><span class="sxs-lookup"><span data-stu-id="e2912-238">Add the TodoContext database context</span></span>

* <span data-ttu-id="e2912-239">Cliquez avec le bouton droit sur le *:::no-loc(Models):::* dossier et sélectionnez **Ajouter** une  >  **classe**.</span><span class="sxs-lookup"><span data-stu-id="e2912-239">Right-click the *:::no-loc(Models):::* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="e2912-240">Nommez la classe *TodoContext* et cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="e2912-240">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="e2912-241">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="e2912-241">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="e2912-242">Ajoutez une `TodoContext` classe au *:::no-loc(Models):::* dossier.</span><span class="sxs-lookup"><span data-stu-id="e2912-242">Add a `TodoContext` class to the *:::no-loc(Models):::* folder.</span></span>

---

* <span data-ttu-id="e2912-243">Entrez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e2912-243">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/5.x/TodoApi/:::no-loc(Models):::/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="e2912-244">Inscrire le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="e2912-244">Register the database context</span></span>

<span data-ttu-id="e2912-245">Dans ASP.NET Core, les services tels que le contexte de base de données doivent être inscrits auprès du [conteneur d’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="e2912-245">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="e2912-246">Le conteneur fournit le service aux contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="e2912-246">The container provides the service to controllers.</span></span>

<span data-ttu-id="e2912-247">Mettez à jour *Startup.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e2912-247">Update *Startup.cs* with the following code:</span></span>

[!code-csharp[](first-web-api/samples/5.x/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="e2912-248">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="e2912-248">The preceding code:</span></span>

* <span data-ttu-id="e2912-249">Supprime les appels Swagger.</span><span class="sxs-lookup"><span data-stu-id="e2912-249">Removes the Swagger calls.</span></span>
* <span data-ttu-id="e2912-250">Supprime les déclarations `using` inutilisées.</span><span class="sxs-lookup"><span data-stu-id="e2912-250">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="e2912-251">Ajoute le contexte de base de données au conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="e2912-251">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="e2912-252">Spécifie que le contexte de base de données utilise une base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="e2912-252">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="e2912-253">Générer automatiquement des modèles pour un contrôleur</span><span class="sxs-lookup"><span data-stu-id="e2912-253">Scaffold a controller</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e2912-254">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e2912-254">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e2912-255">Cliquez avec le bouton droit sur le dossier *Contrôleurs*.</span><span class="sxs-lookup"><span data-stu-id="e2912-255">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="e2912-256">Sélectionnez **Ajouter** > **Nouvel élément généré automatiquement**.</span><span class="sxs-lookup"><span data-stu-id="e2912-256">Select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="e2912-257">Sélectionnez **Contrôleur d’API avec actions, utilisant Entity Framework** , puis **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="e2912-257">Select **API Controller with actions, using Entity Framework** , and then select **Add**.</span></span>
* <span data-ttu-id="e2912-258">Dans la boîte de dialogue **Contrôleur d’API avec actions, utilisant Entity Framework**  :</span><span class="sxs-lookup"><span data-stu-id="e2912-258">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>

  * <span data-ttu-id="e2912-259">Sélectionnez **TodoItem (TodoApi. :::no-loc(Models)::: )** dans la **classe de modèle**.</span><span class="sxs-lookup"><span data-stu-id="e2912-259">Select **TodoItem (TodoApi.:::no-loc(Models):::)** in the **Model class**.</span></span>
  * <span data-ttu-id="e2912-260">Sélectionnez **TodoContext (TodoApi. :::no-loc(Models)::: )** dans la **classe de contexte de données**.</span><span class="sxs-lookup"><span data-stu-id="e2912-260">Select **TodoContext (TodoApi.:::no-loc(Models):::)** in the **Data context class**.</span></span>
  * <span data-ttu-id="e2912-261">Sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="e2912-261">Select **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="e2912-262">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="e2912-262">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="e2912-263">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="e2912-263">Run the following commands:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet tool update -g Dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext -outDir Controllers
```

<span data-ttu-id="e2912-264">Les commandes précédentes :</span><span class="sxs-lookup"><span data-stu-id="e2912-264">The preceding commands:</span></span>

* <span data-ttu-id="e2912-265">Ajoutent les packages NuGet nécessaires à la génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="e2912-265">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="e2912-266">Installent le moteur de génération de modèles automatique (`dotnet-aspnet-codegenerator`).</span><span class="sxs-lookup"><span data-stu-id="e2912-266">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="e2912-267">Génèrent automatiquement des modèles pour `TodoItemsController`.</span><span class="sxs-lookup"><span data-stu-id="e2912-267">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="e2912-268">Le code généré :</span><span class="sxs-lookup"><span data-stu-id="e2912-268">The generated code:</span></span>

* <span data-ttu-id="e2912-269">Marque la classe avec l' [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribut.</span><span class="sxs-lookup"><span data-stu-id="e2912-269">Marks the class with the [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute.</span></span> <span data-ttu-id="e2912-270">Cet attribut indique que le contrôleur répond aux requêtes de l’API web.</span><span class="sxs-lookup"><span data-stu-id="e2912-270">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="e2912-271">Pour plus d’informations sur les comportements spécifiques que permet l’attribut, consultez <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="e2912-271">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="e2912-272">Utilise l’injection de dépendances pour injecter le contexte de base de données (`TodoContext`) dans le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="e2912-272">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="e2912-273">Le contexte de base de données est utilisé dans chacune des méthodes la [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="e2912-273">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<span data-ttu-id="e2912-274">Les modèles de ASP.NET Core pour :</span><span class="sxs-lookup"><span data-stu-id="e2912-274">The ASP.NET Core templates for:</span></span>

* <span data-ttu-id="e2912-275">Les contrôleurs avec des vues sont inclus `[action]` dans le modèle de routage.</span><span class="sxs-lookup"><span data-stu-id="e2912-275">Controllers with views include `[action]` in the route template.</span></span>
* <span data-ttu-id="e2912-276">Les contrôleurs d’API n’incluent pas `[action]` dans le modèle de routage.</span><span class="sxs-lookup"><span data-stu-id="e2912-276">API controllers don't include `[action]` in the route template.</span></span>

<span data-ttu-id="e2912-277">Lorsque le `[action]` jeton n’est pas dans le modèle de routage, le nom de l' [action](xref:mvc/controllers/routing#action) est exclu de l’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="e2912-277">When the `[action]` token isn't in the route template, the [action](xref:mvc/controllers/routing#action) name is excluded from the route.</span></span> <span data-ttu-id="e2912-278">Autrement dit, le nom de la méthode associée à l’action n’est pas utilisé dans l’itinéraire correspondant.</span><span class="sxs-lookup"><span data-stu-id="e2912-278">That is, the action's associated method name isn't used in the matching route.</span></span>

## <a name="update-the-posttodoitem-create-method"></a><span data-ttu-id="e2912-279">Mettre à jour la méthode de création de PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="e2912-279">Update the PostTodoItem create method</span></span>

<span data-ttu-id="e2912-280">Remplacez l’instruction return dans `PostTodoItem` pour utiliser l’opérateur [nameof](/dotnet/csharp/language-reference/operators/nameof) :</span><span class="sxs-lookup"><span data-stu-id="e2912-280">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/5.x/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="e2912-281">Le code précédent est une méthode HTTP Après, comme indiqué par l' [`[HttpPost]`](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute) attribut.</span><span class="sxs-lookup"><span data-stu-id="e2912-281">The preceding code is an HTTP POST method, as indicated by the [`[HttpPost]`](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute) attribute.</span></span> <span data-ttu-id="e2912-282">La méthode obtient la valeur de la tâche dans le corps de la requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="e2912-282">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="e2912-283">Pour plus d’informations, consultez [Routage par attributs avec des attributs Http[Verbe]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="e2912-283">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="e2912-284">La méthode <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> :</span><span class="sxs-lookup"><span data-stu-id="e2912-284">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="e2912-285">Retourne un [code d’état HTTP 201](https://developer.mozilla.org/docs/Web/HTTP/Status/201) en cas de réussite.</span><span class="sxs-lookup"><span data-stu-id="e2912-285">Returns an [HTTP 201 status code](https://developer.mozilla.org/docs/Web/HTTP/Status/201) if successful.</span></span> <span data-ttu-id="e2912-286">HTTP 201 est la réponse standard d’une méthode HTTP POST qui crée une ressource sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="e2912-286">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="e2912-287">Ajoute un en-tête [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) à la réponse.</span><span class="sxs-lookup"><span data-stu-id="e2912-287">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="e2912-288">L’en-tête `Location` spécifie l’[URI](https://developer.mozilla.org/docs/Glossary/URI) de l’élément d’action qui vient d’être créé.</span><span class="sxs-lookup"><span data-stu-id="e2912-288">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="e2912-289">Pour plus d’informations, consultez la section [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="e2912-289">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="e2912-290">Fait référence à l’action `GetTodoItem` pour créer l’URI `Location` de l’en-tête.</span><span class="sxs-lookup"><span data-stu-id="e2912-290">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="e2912-291">Le mot clé `nameof` C# est utilisé pour éviter de coder en dur le nom de l’action dans l’appel `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="e2912-291">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="e2912-292">Installer Postman</span><span class="sxs-lookup"><span data-stu-id="e2912-292">Install Postman</span></span>

<span data-ttu-id="e2912-293">Ce tutoriel utilise Postman pour tester l’API web.</span><span class="sxs-lookup"><span data-stu-id="e2912-293">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="e2912-294">Installer le [poste](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="e2912-294">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="e2912-295">Démarrez l’application web.</span><span class="sxs-lookup"><span data-stu-id="e2912-295">Start the web app.</span></span>
* <span data-ttu-id="e2912-296">Démarrez Postman.</span><span class="sxs-lookup"><span data-stu-id="e2912-296">Start Postman.</span></span>
* <span data-ttu-id="e2912-297">Désactivez la **vérification du certificat SSL**.</span><span class="sxs-lookup"><span data-stu-id="e2912-297">Disable **SSL certificate verification**</span></span>
  * <span data-ttu-id="e2912-298">À partir de **Fichier** > **Paramètres** (onglet **Général** ), désactivez **Vérification du certificat SSL**.</span><span class="sxs-lookup"><span data-stu-id="e2912-298">From **File** > **Settings** ( **General** tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="e2912-299">Réactivez la vérification du certificat SSL après avoir testé le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="e2912-299">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="e2912-300">Tester PostTodoItem avec Postman</span><span class="sxs-lookup"><span data-stu-id="e2912-300">Test PostTodoItem with Postman</span></span>

* <span data-ttu-id="e2912-301">Créez une requête.</span><span class="sxs-lookup"><span data-stu-id="e2912-301">Create a new request.</span></span>
* <span data-ttu-id="e2912-302">Affectez `POST` à la méthode HTTP.</span><span class="sxs-lookup"><span data-stu-id="e2912-302">Set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="e2912-303">Définissez l’URI sur `https://localhost:<port>/api/TodoItems` .</span><span class="sxs-lookup"><span data-stu-id="e2912-303">Set the URI to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="e2912-304">Par exemple : `https://localhost:5001/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="e2912-304">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="e2912-305">Sélectionnez l’onglet **Corps** .</span><span class="sxs-lookup"><span data-stu-id="e2912-305">Select the **Body** tab.</span></span>
* <span data-ttu-id="e2912-306">Sélectionnez la case d’option **raw** (données brutes).</span><span class="sxs-lookup"><span data-stu-id="e2912-306">Select the **raw** radio button.</span></span>
* <span data-ttu-id="e2912-307">Définissez le type sur **JSON (application/json)**.</span><span class="sxs-lookup"><span data-stu-id="e2912-307">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="e2912-308">Dans le corps de la demande, entrez la syntaxe JSON d’une tâche :</span><span class="sxs-lookup"><span data-stu-id="e2912-308">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="e2912-309">Sélectionnez **Envoyer**.</span><span class="sxs-lookup"><span data-stu-id="e2912-309">Select **Send**.</span></span>

  ![Postman avec requête de création](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri"></a><span data-ttu-id="e2912-311">Tester l’URI de l’en-tête d’emplacement</span><span class="sxs-lookup"><span data-stu-id="e2912-311">Test the location header URI</span></span>

<span data-ttu-id="e2912-312">L’URI d’en-tête d’emplacement peut être testé dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="e2912-312">The location header URI can be tested in the browser.</span></span> <span data-ttu-id="e2912-313">Copiez et collez l’URI d’en-tête d’emplacement dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="e2912-313">Copy and paste the location header URI into the browser.</span></span>

<span data-ttu-id="e2912-314">Pour effectuer un test dans le poster :</span><span class="sxs-lookup"><span data-stu-id="e2912-314">To test in Postman:</span></span>

* <span data-ttu-id="e2912-315">Sélectionnez l’onglet **Headers** (En-têtes) dans le volet **Response** (Réponse).</span><span class="sxs-lookup"><span data-stu-id="e2912-315">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="e2912-316">Copiez la valeur d’en-tête **Location** (Emplacement) :</span><span class="sxs-lookup"><span data-stu-id="e2912-316">Copy the **Location** header value:</span></span>

  ![Onglet Headers de la console Postman](first-web-api/_static/3/create.png)

* <span data-ttu-id="e2912-318">Affectez `GET` à la méthode HTTP.</span><span class="sxs-lookup"><span data-stu-id="e2912-318">Set the HTTP method to `GET`.</span></span>
* <span data-ttu-id="e2912-319">Définissez l’URI sur `https://localhost:<port>/api/TodoItems/1` .</span><span class="sxs-lookup"><span data-stu-id="e2912-319">Set the URI to `https://localhost:<port>/api/TodoItems/1`.</span></span> <span data-ttu-id="e2912-320">Par exemple : `https://localhost:5001/api/TodoItems/1`.</span><span class="sxs-lookup"><span data-stu-id="e2912-320">For example, `https://localhost:5001/api/TodoItems/1`.</span></span>
* <span data-ttu-id="e2912-321">Sélectionnez **Envoyer**.</span><span class="sxs-lookup"><span data-stu-id="e2912-321">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="e2912-322">Examiner les méthodes GET</span><span class="sxs-lookup"><span data-stu-id="e2912-322">Examine the GET methods</span></span>

<span data-ttu-id="e2912-323">Deux points de terminaison d’extraction sont implémentés :</span><span class="sxs-lookup"><span data-stu-id="e2912-323">Two GET endpoints are implemented:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="e2912-324">Testez l’application en appelant les deux points de terminaison à partir d’un navigateur ou de Postman.</span><span class="sxs-lookup"><span data-stu-id="e2912-324">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="e2912-325">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="e2912-325">For example:</span></span>

* `https://localhost:5001/api/TodoItems`
* `https://localhost:5001/api/TodoItems/1`

<span data-ttu-id="e2912-326">Une réponse semblable à la suivante est produite par l’appel à `GetTodoItems` :</span><span class="sxs-lookup"><span data-stu-id="e2912-326">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="e2912-327">Tester Get avec Postman</span><span class="sxs-lookup"><span data-stu-id="e2912-327">Test Get with Postman</span></span>

* <span data-ttu-id="e2912-328">Créez une requête.</span><span class="sxs-lookup"><span data-stu-id="e2912-328">Create a new request.</span></span>
* <span data-ttu-id="e2912-329">Définissez la méthode HTTP sur **GET**.</span><span class="sxs-lookup"><span data-stu-id="e2912-329">Set the HTTP method to **GET**.</span></span>
* <span data-ttu-id="e2912-330">Définissez l’URI de la demande sur `https://localhost:<port>/api/TodoItems` .</span><span class="sxs-lookup"><span data-stu-id="e2912-330">Set the request URI to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="e2912-331">Par exemple : `https://localhost:5001/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="e2912-331">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="e2912-332">Définissez l’ **affichage à deux volets** dans Postman.</span><span class="sxs-lookup"><span data-stu-id="e2912-332">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="e2912-333">Sélectionnez **Envoyer**.</span><span class="sxs-lookup"><span data-stu-id="e2912-333">Select **Send**.</span></span>

<span data-ttu-id="e2912-334">Cette application utilise une base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="e2912-334">This app uses an in-memory database.</span></span> <span data-ttu-id="e2912-335">Si l’application est arrêtée et démarrée, la requête GET précédente ne retourne aucune donnée.</span><span class="sxs-lookup"><span data-stu-id="e2912-335">If the app is stopped and started, the preceding GET request will not return any data.</span></span> <span data-ttu-id="e2912-336">Si aucune donnée n’est retournée, publiez ([POST](#post)) les données dans l’application.</span><span class="sxs-lookup"><span data-stu-id="e2912-336">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="e2912-337">Routage et chemins d’URL</span><span class="sxs-lookup"><span data-stu-id="e2912-337">Routing and URL paths</span></span>

<span data-ttu-id="e2912-338">L' [`[HttpGet]`](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) attribut désigne une méthode qui répond à une requête http.</span><span class="sxs-lookup"><span data-stu-id="e2912-338">The [`[HttpGet]`](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="e2912-339">Le chemin d’URL pour chaque méthode est construit comme suit :</span><span class="sxs-lookup"><span data-stu-id="e2912-339">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="e2912-340">Partez de la chaîne de modèle dans l’attribut `Route` du contrôleur :</span><span class="sxs-lookup"><span data-stu-id="e2912-340">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/5.x/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="e2912-341">Remplacez `[controller]` par le nom du contrôleur qui, par convention, est le nom de la classe du contrôleur sans le suffixe « Controller ».</span><span class="sxs-lookup"><span data-stu-id="e2912-341">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="e2912-342">Pour cet exemple, le nom de la classe du contrôleur étant **TodoItems** Controller, le nom du contrôleur est « TodoItems ».</span><span class="sxs-lookup"><span data-stu-id="e2912-342">For this sample, the controller class name is **TodoItems** Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="e2912-343">Le [routage](xref:mvc/controllers/routing) d’ASP.NET Core ne respecte pas la casse.</span><span class="sxs-lookup"><span data-stu-id="e2912-343">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="e2912-344">Si l’attribut `[HttpGet]` a un modèle de route (par exemple, `[HttpGet("products")]`), ajoutez-le au chemin.</span><span class="sxs-lookup"><span data-stu-id="e2912-344">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="e2912-345">Cet exemple n’utilise pas de modèle.</span><span class="sxs-lookup"><span data-stu-id="e2912-345">This sample doesn't use a template.</span></span> <span data-ttu-id="e2912-346">Pour plus d’informations, consultez [Routage par attributs avec des attributs Http[Verbe]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="e2912-346">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="e2912-347">Dans la méthode `GetTodoItem` suivante, `"{id}"` est une variable d’espace réservé pour l’identificateur unique de la tâche.</span><span class="sxs-lookup"><span data-stu-id="e2912-347">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="e2912-348">Lorsque `GetTodoItem` est appelé, la valeur de `"{id}"` dans l’URL est fournie à la méthode dans son `id` paramètre.</span><span class="sxs-lookup"><span data-stu-id="e2912-348">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its `id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="e2912-349">Valeurs retournées</span><span class="sxs-lookup"><span data-stu-id="e2912-349">Return values</span></span>

<span data-ttu-id="e2912-350">Le type de retour des `GetTodoItems` `GetTodoItem` méthodes et est [ActionResult \<T> type](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="e2912-350">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="e2912-351">ASP.NET Core sérialise automatiquement l’objet en [JSON](https://www.json.org/) et écrit le JSON dans le corps du message de réponse.</span><span class="sxs-lookup"><span data-stu-id="e2912-351">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="e2912-352">Le code de réponse pour ce type de retour est [200 OK](https://developer.mozilla.org/docs/Web/HTTP/Status/200), en supposant qu’il n’existe aucune exception non gérée.</span><span class="sxs-lookup"><span data-stu-id="e2912-352">The response code for this return type is [200 OK](https://developer.mozilla.org/docs/Web/HTTP/Status/200), assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="e2912-353">Les exceptions non gérées sont converties en erreurs 5xx.</span><span class="sxs-lookup"><span data-stu-id="e2912-353">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="e2912-354">Les types de retour `ActionResult` peuvent représenter une large plage de codes d’état HTTP.</span><span class="sxs-lookup"><span data-stu-id="e2912-354">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="e2912-355">Par exemple, `GetTodoItem` peut retourner deux valeurs d’état différentes :</span><span class="sxs-lookup"><span data-stu-id="e2912-355">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="e2912-356">Si aucun élément ne correspond à l’ID demandé, la méthode retourne un code d’erreur d' [état 404](https://developer.mozilla.org/docs/Web/HTTP/Status/404) <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound%2A> .</span><span class="sxs-lookup"><span data-stu-id="e2912-356">If no item matches the requested ID, the method returns a [404 status](https://developer.mozilla.org/docs/Web/HTTP/Status/404) <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound%2A> error code.</span></span>
* <span data-ttu-id="e2912-357">Sinon, la méthode retourne 200 avec un corps de réponse JSON.</span><span class="sxs-lookup"><span data-stu-id="e2912-357">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="e2912-358">Le retour de `item` entraîne une réponse HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="e2912-358">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="e2912-359">Méthode PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="e2912-359">The PutTodoItem method</span></span>

<span data-ttu-id="e2912-360">Examinez la méthode `PutTodoItem` :</span><span class="sxs-lookup"><span data-stu-id="e2912-360">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/5.x/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="e2912-361">`PutTodoItem` est similaire à `PostTodoItem`, à la différence près qu’il utilise HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="e2912-361">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="e2912-362">La réponse est [204 (aucun contenu)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="e2912-362">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="e2912-363">D’après la spécification HTTP, une requête PUT nécessite que le client envoie toute l’entité mise à jour, et pas seulement les changements.</span><span class="sxs-lookup"><span data-stu-id="e2912-363">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="e2912-364">Pour prendre en charge les mises à jour partielles, utilisez [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="e2912-364">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="e2912-365">Si vous obtenez une erreur en appelant `PutTodoItem`, appelez `GET` pour vérifier que la base de données contient un élément.</span><span class="sxs-lookup"><span data-stu-id="e2912-365">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="e2912-366">Tester la méthode PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="e2912-366">Test the PutTodoItem method</span></span>

<span data-ttu-id="e2912-367">Cet exemple utilise une base de données en mémoire qui doit être initialisée chaque fois que l’application est démarrée.</span><span class="sxs-lookup"><span data-stu-id="e2912-367">This sample uses an in-memory database that must be initialized each time the app is started.</span></span> <span data-ttu-id="e2912-368">La base de données doit contenir un élément avant que vous ne passiez un appel PUT.</span><span class="sxs-lookup"><span data-stu-id="e2912-368">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="e2912-369">Appelez obtenir pour vous assurer qu’il y a un élément dans la base de données avant d’effectuer un appel PUT.</span><span class="sxs-lookup"><span data-stu-id="e2912-369">Call GET to ensure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="e2912-370">Mettez à jour l’élément de tâche dont l’ID est égal à 1 et affectez-lui comme nom `"feed fish"` :</span><span class="sxs-lookup"><span data-stu-id="e2912-370">Update the to-do item that has Id = 1 and set its name to `"feed fish"`:</span></span>

```json
  {
    "Id":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="e2912-371">L’image suivante montre la mise à jour Postman :</span><span class="sxs-lookup"><span data-stu-id="e2912-371">The following image shows the Postman update:</span></span>

![Console Postman montrant la réponse 204 (Pas de contenu)](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="e2912-373">Méthode DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="e2912-373">The DeleteTodoItem method</span></span>

<span data-ttu-id="e2912-374">Examinez la méthode `DeleteTodoItem` :</span><span class="sxs-lookup"><span data-stu-id="e2912-374">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/5.x/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="e2912-375">Tester la méthode DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="e2912-375">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="e2912-376">Utilisez Postman pour supprimer une tâche :</span><span class="sxs-lookup"><span data-stu-id="e2912-376">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="e2912-377">Définissez la méthode sur `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="e2912-377">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="e2912-378">Définissez l’URI de l’objet à supprimer (par exemple `https://localhost:5001/api/TodoItems/1` ).</span><span class="sxs-lookup"><span data-stu-id="e2912-378">Set the URI of the object to delete (for example `https://localhost:5001/api/TodoItems/1`).</span></span>
* <span data-ttu-id="e2912-379">Sélectionnez **Envoyer**.</span><span class="sxs-lookup"><span data-stu-id="e2912-379">Select **Send**.</span></span>

<a name="over-post-v5"></a>

## <a name="prevent-over-posting"></a><span data-ttu-id="e2912-380">Empêcher la post-validation</span><span class="sxs-lookup"><span data-stu-id="e2912-380">Prevent over-posting</span></span>

<span data-ttu-id="e2912-381">Actuellement, l’exemple d’application expose l' `TodoItem` objet entier.</span><span class="sxs-lookup"><span data-stu-id="e2912-381">Currently the sample app exposes the entire `TodoItem` object.</span></span> <span data-ttu-id="e2912-382">En général, les applications de production limitent les données entrées et retournées à l’aide d’un sous-ensemble du modèle.</span><span class="sxs-lookup"><span data-stu-id="e2912-382">Production apps typically limit the data that's input and returned using a subset of the model.</span></span> <span data-ttu-id="e2912-383">Il existe plusieurs raisons à cela, et la sécurité est essentielle.</span><span class="sxs-lookup"><span data-stu-id="e2912-383">There are multiple reasons behind this and security is a major one.</span></span> <span data-ttu-id="e2912-384">Le sous-ensemble d’un modèle est généralement appelé un objet Transfert de données (DTO), un modèle d’entrée ou un modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="e2912-384">The subset of a model is usually referred to as a Data Transfer Object (DTO), input model, or view model.</span></span> <span data-ttu-id="e2912-385">Le **DTO** est utilisé dans cet article.</span><span class="sxs-lookup"><span data-stu-id="e2912-385">**DTO** is used in this article.</span></span>

<span data-ttu-id="e2912-386">Un DTO peut être utilisé pour :</span><span class="sxs-lookup"><span data-stu-id="e2912-386">A DTO may be used to:</span></span>

* <span data-ttu-id="e2912-387">Empêcher la survalidation.</span><span class="sxs-lookup"><span data-stu-id="e2912-387">Prevent over-posting.</span></span>
* <span data-ttu-id="e2912-388">Masquer les propriétés que les clients ne sont pas censés afficher.</span><span class="sxs-lookup"><span data-stu-id="e2912-388">Hide properties that clients are not supposed to view.</span></span>
* <span data-ttu-id="e2912-389">Omettez certaines propriétés afin de réduire la taille de la charge utile.</span><span class="sxs-lookup"><span data-stu-id="e2912-389">Omit some properties in order to reduce payload size.</span></span>
* <span data-ttu-id="e2912-390">Aplatir les graphiques d’objets qui contiennent des objets imbriqués.</span><span class="sxs-lookup"><span data-stu-id="e2912-390">Flatten object graphs that contain nested objects.</span></span> <span data-ttu-id="e2912-391">Les graphiques d’objets aplatis peuvent être plus pratiques pour les clients.</span><span class="sxs-lookup"><span data-stu-id="e2912-391">Flattened object graphs can be more convenient for clients.</span></span>

<span data-ttu-id="e2912-392">Pour illustrer l’approche DTO, mettez à jour la `TodoItem` classe pour inclure un champ secret :</span><span class="sxs-lookup"><span data-stu-id="e2912-392">To demonstrate the DTO approach, update the `TodoItem` class to include a secret field:</span></span>

[!code-csharp[](first-web-api/samples/5.x/TodoApiDTO/:::no-loc(Models):::/TodoItem.cs?name=snippet&highlight=6)]

<span data-ttu-id="e2912-393">Le champ secret doit être masqué à partir de cette application, mais une application administrative peut choisir de l’exposer.</span><span class="sxs-lookup"><span data-stu-id="e2912-393">The secret field needs to be hidden from this app, but an administrative app could choose to expose it.</span></span>

<span data-ttu-id="e2912-394">Vérifiez que vous pouvez poster et recevoir le champ secret.</span><span class="sxs-lookup"><span data-stu-id="e2912-394">Verify you can post and get the secret field.</span></span>

<span data-ttu-id="e2912-395">Créer un modèle DTO :</span><span class="sxs-lookup"><span data-stu-id="e2912-395">Create a DTO model:</span></span>

[!code-csharp[](first-web-api/samples/5.x/TodoApiDTO/:::no-loc(Models):::/TodoItemDTO.cs?name=snippet)]

<span data-ttu-id="e2912-396">Mettez à jour le `TodoItemsController` pour utiliser `TodoItemDTO` :</span><span class="sxs-lookup"><span data-stu-id="e2912-396">Update the `TodoItemsController` to use `TodoItemDTO`:</span></span>

[!code-csharp[](first-web-api/samples/5.x/TodoApiDTO/Controllers/TodoItemsController.cs?name=snippet)]

<span data-ttu-id="e2912-397">Vérifiez que vous ne pouvez pas poster ou recevoir le champ secret.</span><span class="sxs-lookup"><span data-stu-id="e2912-397">Verify you can't post or get the secret field.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="e2912-398">Appelez l’API web avec JavaScript</span><span class="sxs-lookup"><span data-stu-id="e2912-398">Call the web API with JavaScript</span></span>

<span data-ttu-id="e2912-399">Consultez [Didacticiel : appeler une API web ASP.net core avec JavaScript](xref:tutorials/web-api-javascript).</span><span class="sxs-lookup"><span data-stu-id="e2912-399">See [Tutorial: Call an ASP.NET Core web API with JavaScript](xref:tutorials/web-api-javascript).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0 < aspnetcore-5.0"

<span data-ttu-id="e2912-400">Dans ce tutoriel, vous allez apprendre à :</span><span class="sxs-lookup"><span data-stu-id="e2912-400">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e2912-401">Créer un projet d’API web.</span><span class="sxs-lookup"><span data-stu-id="e2912-401">Create a web API project.</span></span>
> * <span data-ttu-id="e2912-402">Ajouter une classe de modèle et un contexte de base de données.</span><span class="sxs-lookup"><span data-stu-id="e2912-402">Add a model class and a database context.</span></span>
> * <span data-ttu-id="e2912-403">Générer automatiquement des modèles pour un contrôleur avec des méthodes CRUD.</span><span class="sxs-lookup"><span data-stu-id="e2912-403">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="e2912-404">Configurer le routage, les chemins d’URL et les valeurs de retour.</span><span class="sxs-lookup"><span data-stu-id="e2912-404">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="e2912-405">Appeler l’API web avec Postman</span><span class="sxs-lookup"><span data-stu-id="e2912-405">Call the web API with Postman.</span></span>

<span data-ttu-id="e2912-406">À la fin, vous disposez d’une API web qui peut gérer des tâches stockées dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="e2912-406">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="e2912-407">Vue d’ensemble</span><span class="sxs-lookup"><span data-stu-id="e2912-407">Overview</span></span>

<span data-ttu-id="e2912-408">Ce didacticiel crée l’API suivante :</span><span class="sxs-lookup"><span data-stu-id="e2912-408">This tutorial creates the following API:</span></span>

|<span data-ttu-id="e2912-409">API</span><span class="sxs-lookup"><span data-stu-id="e2912-409">API</span></span> | <span data-ttu-id="e2912-410">Description</span><span class="sxs-lookup"><span data-stu-id="e2912-410">Description</span></span> | <span data-ttu-id="e2912-411">Corps de la demande</span><span class="sxs-lookup"><span data-stu-id="e2912-411">Request body</span></span> | <span data-ttu-id="e2912-412">Response body</span><span class="sxs-lookup"><span data-stu-id="e2912-412">Response body</span></span> |
|--- | ---- | ---- | ---- |
|`GET /api/TodoItems` | <span data-ttu-id="e2912-413">Obtenir toutes les tâches</span><span class="sxs-lookup"><span data-stu-id="e2912-413">Get all to-do items</span></span> | <span data-ttu-id="e2912-414">Aucun</span><span class="sxs-lookup"><span data-stu-id="e2912-414">None</span></span> | <span data-ttu-id="e2912-415">Tableau de tâches</span><span class="sxs-lookup"><span data-stu-id="e2912-415">Array of to-do items</span></span>|
|`GET /api/TodoItems/{id}` | <span data-ttu-id="e2912-416">Obtenir un élément par ID</span><span class="sxs-lookup"><span data-stu-id="e2912-416">Get an item by ID</span></span> | <span data-ttu-id="e2912-417">Aucun</span><span class="sxs-lookup"><span data-stu-id="e2912-417">None</span></span> | <span data-ttu-id="e2912-418">Tâche</span><span class="sxs-lookup"><span data-stu-id="e2912-418">To-do item</span></span>|
|`POST /api/TodoItems` | <span data-ttu-id="e2912-419">Ajouter un nouvel élément</span><span class="sxs-lookup"><span data-stu-id="e2912-419">Add a new item</span></span> | <span data-ttu-id="e2912-420">Tâche</span><span class="sxs-lookup"><span data-stu-id="e2912-420">To-do item</span></span> | <span data-ttu-id="e2912-421">Tâche</span><span class="sxs-lookup"><span data-stu-id="e2912-421">To-do item</span></span> |
|`PUT /api/TodoItems/{id}` | <span data-ttu-id="e2912-422">Mettre à jour un élément existant &nbsp;</span><span class="sxs-lookup"><span data-stu-id="e2912-422">Update an existing item &nbsp;</span></span> | <span data-ttu-id="e2912-423">Tâche</span><span class="sxs-lookup"><span data-stu-id="e2912-423">To-do item</span></span> | <span data-ttu-id="e2912-424">Aucun</span><span class="sxs-lookup"><span data-stu-id="e2912-424">None</span></span> |
|<span data-ttu-id="e2912-425">`DELETE /api/TodoItems/{id}` &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="e2912-425">`DELETE /api/TodoItems/{id}` &nbsp; &nbsp;</span></span> | <span data-ttu-id="e2912-426">Supprimer un élément &nbsp;&nbsp;</span><span class="sxs-lookup"><span data-stu-id="e2912-426">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="e2912-427">None</span><span class="sxs-lookup"><span data-stu-id="e2912-427">None</span></span> | <span data-ttu-id="e2912-428">None</span><span class="sxs-lookup"><span data-stu-id="e2912-428">None</span></span>|

<span data-ttu-id="e2912-429">Le diagramme suivant illustre la conception de l’application.</span><span class="sxs-lookup"><span data-stu-id="e2912-429">The following diagram shows the design of the app.</span></span>

![Le client est représenté par une zone située à gauche.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="e2912-435">Prérequis</span><span class="sxs-lookup"><span data-stu-id="e2912-435">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e2912-436">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e2912-436">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="e2912-437">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e2912-437">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="e2912-438">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="e2912-438">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="e2912-439">Créer un projet web</span><span class="sxs-lookup"><span data-stu-id="e2912-439">Create a web project</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e2912-440">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e2912-440">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e2912-441">Dans le menu **fichier** , sélectionnez **nouveau** > **projet**.</span><span class="sxs-lookup"><span data-stu-id="e2912-441">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="e2912-442">Sélectionnez le modèle **Application web ASP.NET Core** et cliquez sur **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="e2912-442">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="e2912-443">Nommez le projet *TodoApi* et cliquez sur **Créer**.</span><span class="sxs-lookup"><span data-stu-id="e2912-443">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="e2912-444">Dans la boîte de dialogue **créer une application Web ASP.net Core** , vérifiez que **.net Core** et **ASP.net Core 3,1** sont sélectionnés.</span><span class="sxs-lookup"><span data-stu-id="e2912-444">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.1** are selected.</span></span> <span data-ttu-id="e2912-445">Sélectionnez le modèle **API** et cliquez sur **Créer**.</span><span class="sxs-lookup"><span data-stu-id="e2912-445">Select the **API** template and click **Create**.</span></span>

![Boîte de dialogue de nouveau projet dans VS](first-web-api/_static/vs3.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="e2912-447">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e2912-447">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e2912-448">Ouvrez le [Terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="e2912-448">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="e2912-449">Définissez les répertoires (`cd`) sur le dossier destiné à contenir le dossier du projet.</span><span class="sxs-lookup"><span data-stu-id="e2912-449">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="e2912-450">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="e2912-450">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoApi
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

* <span data-ttu-id="e2912-451">Quand une boîte de dialogue vous demande si vous souhaitez ajouter les composants nécessaires au projet, sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="e2912-451">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="e2912-452">Les commandes précédentes :</span><span class="sxs-lookup"><span data-stu-id="e2912-452">The preceding commands:</span></span>

  * <span data-ttu-id="e2912-453">Créent un projet API Web et l’ouvrent dans Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e2912-453">Creates a new web API project and opens it in Visual Studio Code.</span></span>
  * <span data-ttu-id="e2912-454">Ajoutent les packages NuGet qui sont requis dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="e2912-454">Adds the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="e2912-455">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="e2912-455">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e2912-456">Sélectionnez **Fichier** > **Nouvelle solution**.</span><span class="sxs-lookup"><span data-stu-id="e2912-456">Select **File** > **New Solution**.</span></span>

  ![macOS - Nouvelle solution](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="e2912-458">Dans Visual Studio pour Mac antérieure à la version 8,6, sélectionnez API de l’application **.net Core**  >  **App**  >  **API**  >  **suivant**.</span><span class="sxs-lookup"><span data-stu-id="e2912-458">In Visual Studio for Mac earlier than version 8.6, select **.NET Core** > **App** > **API** > **Next**.</span></span> <span data-ttu-id="e2912-459">Dans la version 8,6 ou une version ultérieure, sélectionnez application **Web et**  >  **App**  >  **API** application console  >  **suivant**.</span><span class="sxs-lookup"><span data-stu-id="e2912-459">In version 8.6 or later, select **Web and Console** > **App** > **API** > **Next**.</span></span>

  ![sélection du modèle d’API macOS](first-web-api-mac/_static/api_template.png)

* <span data-ttu-id="e2912-461">Dans la boîte de dialogue **configurer la nouvelle API Web ASP.net Core** , sélectionnez la version la plus récente de .net Core 3. x **Target Framework**.</span><span class="sxs-lookup"><span data-stu-id="e2912-461">In the **Configure the new ASP.NET Core Web API** dialog, select the latest .NET Core 3.x **Target Framework**.</span></span> <span data-ttu-id="e2912-462">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="e2912-462">Select **Next**.</span></span>

* <span data-ttu-id="e2912-463">Entrez *TodoApi* comme **Nom du projet** , puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="e2912-463">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![boîte de dialogue de configuration](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

<span data-ttu-id="e2912-465">Ouvrez un terminal de commande dans le dossier de projet, puis exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="e2912-465">Open a command terminal in the project folder and run the following commands:</span></span>

   ```dotnetcli
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   ```

---

### <a name="test-the-api"></a><span data-ttu-id="e2912-466">Tester l’API</span><span class="sxs-lookup"><span data-stu-id="e2912-466">Test the API</span></span>

<span data-ttu-id="e2912-467">Le modèle de projet crée une API `WeatherForecast`.</span><span class="sxs-lookup"><span data-stu-id="e2912-467">The project template creates a `WeatherForecast` API.</span></span> <span data-ttu-id="e2912-468">Appelez la méthode `Get` à partir d’un navigateur pour tester l’application.</span><span class="sxs-lookup"><span data-stu-id="e2912-468">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e2912-469">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e2912-469">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e2912-470">Appuyez sur Ctrl+F5 pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="e2912-470">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="e2912-471">Visual Studio lance un navigateur et accède à `https://localhost:<port>/WeatherForecast`, où `<port>` est un numéro de port choisi de manière aléatoire.</span><span class="sxs-lookup"><span data-stu-id="e2912-471">Visual Studio launches a browser and navigates to `https://localhost:<port>/WeatherForecast`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="e2912-472">Si une boîte de dialogue apparaît vous demandant si vous devez approuver le certificat IIS Express, sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="e2912-472">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="e2912-473">Dans la boîte de dialogue **Avertissement de sécurité** qui s’affiche ensuite, sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="e2912-473">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="e2912-474">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e2912-474">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e2912-475">Appuyez sur Ctrl+F5 pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="e2912-475">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="e2912-476">Dans un navigateur, accédez à l’URL suivante : `https://localhost:5001/WeatherForecast`.</span><span class="sxs-lookup"><span data-stu-id="e2912-476">In a browser, go to following URL: `https://localhost:5001/WeatherForecast`.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="e2912-477">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="e2912-477">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="e2912-478">Sélectionnez **exécuter**  >  **Démarrer le débogage** pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="e2912-478">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="e2912-479">Visual Studio pour Mac lance un navigateur et accède à `https://localhost:<port>`, où `<port>` est un numéro de port choisi de manière aléatoire.</span><span class="sxs-lookup"><span data-stu-id="e2912-479">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="e2912-480">Une erreur HTTP 404 (introuvable) est retournée.</span><span class="sxs-lookup"><span data-stu-id="e2912-480">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="e2912-481">Ajoutez `/WeatherForecast` à l’URL (définissez-la sur `https://localhost:<port>/WeatherForecast`).</span><span class="sxs-lookup"><span data-stu-id="e2912-481">Append `/WeatherForecast` to the URL (change the URL to `https://localhost:<port>/WeatherForecast`).</span></span>

---

<span data-ttu-id="e2912-482">Un code JSON similaire au suivant est retourné :</span><span class="sxs-lookup"><span data-stu-id="e2912-482">JSON similar to the following is returned:</span></span>

```json
[
    {
        "date": "2019-07-16T19:04:05.7257911-06:00",
        "temperatureC": 52,
        "temperatureF": 125,
        "summary": "Mild"
    },
    {
        "date": "2019-07-17T19:04:05.7258461-06:00",
        "temperatureC": 36,
        "temperatureF": 96,
        "summary": "Warm"
    },
    {
        "date": "2019-07-18T19:04:05.7258467-06:00",
        "temperatureC": 39,
        "temperatureF": 102,
        "summary": "Cool"
    },
    {
        "date": "2019-07-19T19:04:05.7258471-06:00",
        "temperatureC": 10,
        "temperatureF": 49,
        "summary": "Bracing"
    },
    {
        "date": "2019-07-20T19:04:05.7258474-06:00",
        "temperatureC": -1,
        "temperatureF": 31,
        "summary": "Chilly"
    }
]
```

## <a name="add-a-model-class"></a><span data-ttu-id="e2912-483">Ajouter une classe de modèle</span><span class="sxs-lookup"><span data-stu-id="e2912-483">Add a model class</span></span>

<span data-ttu-id="e2912-484">Un *modèle* est un ensemble de classes qui représentent les données gérées par l’application.</span><span class="sxs-lookup"><span data-stu-id="e2912-484">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="e2912-485">Le modèle pour cette application est une classe `TodoItem` unique.</span><span class="sxs-lookup"><span data-stu-id="e2912-485">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e2912-486">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e2912-486">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e2912-487">Dans l’ **Explorateur de solutions** , cliquez avec le bouton droit sur le projet.</span><span class="sxs-lookup"><span data-stu-id="e2912-487">In **Solution Explorer** , right-click the project.</span></span> <span data-ttu-id="e2912-488">Sélectionnez **Ajouter**  >  **un nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="e2912-488">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="e2912-489">Nommez le dossier *:::no-loc(Models):::* .</span><span class="sxs-lookup"><span data-stu-id="e2912-489">Name the folder *:::no-loc(Models):::*.</span></span>

* <span data-ttu-id="e2912-490">Cliquez avec le bouton droit sur le *:::no-loc(Models):::* dossier et sélectionnez **Ajouter** une  >  **classe**.</span><span class="sxs-lookup"><span data-stu-id="e2912-490">Right-click the *:::no-loc(Models):::* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="e2912-491">Nommez la classe *TodoItem* et sélectionnez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="e2912-491">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="e2912-492">Remplacez le code du modèle par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e2912-492">Replace the template code with the following code:</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="e2912-493">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e2912-493">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e2912-494">Ajoutez un dossier nommé *:::no-loc(Models):::* .</span><span class="sxs-lookup"><span data-stu-id="e2912-494">Add a folder named *:::no-loc(Models):::*.</span></span>

* <span data-ttu-id="e2912-495">Ajoutez une `TodoItem` classe au dossier à l' *:::no-loc(Models):::* aide du code suivant :</span><span class="sxs-lookup"><span data-stu-id="e2912-495">Add a `TodoItem` class to the *:::no-loc(Models):::* folder with the following code:</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="e2912-496">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="e2912-496">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e2912-497">Cliquez avec le bouton droit sur le projet.</span><span class="sxs-lookup"><span data-stu-id="e2912-497">Right-click the project.</span></span> <span data-ttu-id="e2912-498">Sélectionnez **Ajouter**  >  **un nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="e2912-498">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="e2912-499">Nommez le dossier *:::no-loc(Models):::* .</span><span class="sxs-lookup"><span data-stu-id="e2912-499">Name the folder *:::no-loc(Models):::*.</span></span>

  ![nouveau dossier](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="e2912-501">Cliquez avec le bouton droit sur le *:::no-loc(Models):::* dossier, puis sélectionnez **Ajouter** > **un nouveau fichier** > **General** > **classe générale vide**.</span><span class="sxs-lookup"><span data-stu-id="e2912-501">Right-click the *:::no-loc(Models):::* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="e2912-502">Nommez la classe *TodoItem* et cliquez sur **Nouveau**.</span><span class="sxs-lookup"><span data-stu-id="e2912-502">Name the class *TodoItem* , and then click **New**.</span></span>

* <span data-ttu-id="e2912-503">Remplacez le code du modèle par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e2912-503">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/:::no-loc(Models):::/TodoItem.cs?name=snippet)]

<span data-ttu-id="e2912-504">La propriété `Id` fonctionne comme la clé unique dans une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="e2912-504">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="e2912-505">Les classes de modèle peuvent se trouver n’importe où dans le projet, mais le *:::no-loc(Models):::* dossier est utilisé par Convention.</span><span class="sxs-lookup"><span data-stu-id="e2912-505">Model classes can go anywhere in the project, but the *:::no-loc(Models):::* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="e2912-506">Ajouter un contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="e2912-506">Add a database context</span></span>

<span data-ttu-id="e2912-507">Le *contexte de base de données* est la classe principale qui coordonne les fonctionnalités d’Entity Framework pour un modèle de données.</span><span class="sxs-lookup"><span data-stu-id="e2912-507">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="e2912-508">Cette classe est créée en dérivant de la classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="e2912-508">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e2912-509">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e2912-509">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-nuget-packages"></a><span data-ttu-id="e2912-510">Ajouter des packages NuGet</span><span class="sxs-lookup"><span data-stu-id="e2912-510">Add NuGet packages</span></span>

* <span data-ttu-id="e2912-511">Dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet > Gérer les packages NuGet pour la solution**.</span><span class="sxs-lookup"><span data-stu-id="e2912-511">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
* <span data-ttu-id="e2912-512">Sélectionnez l’onglet **Parcourir** , puis entrez **Microsoft.EntityFrameworkCore.SqlServer** dans la zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="e2912-512">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
* <span data-ttu-id="e2912-513">Dans le volet gauche, sélectionnez **Microsoft. EntityFrameworkCore. SqlServer** .</span><span class="sxs-lookup"><span data-stu-id="e2912-513">Select **Microsoft.EntityFrameworkCore.SqlServer** in the left pane.</span></span>
* <span data-ttu-id="e2912-514">Cochez la case **Projet** dans le volet droit, puis sélectionnez **Installer**.</span><span class="sxs-lookup"><span data-stu-id="e2912-514">Select the **Project** check box in the right pane and then select **Install**.</span></span>
* <span data-ttu-id="e2912-515">Utilisez les instructions précédentes pour ajouter le package NuGet **Microsoft. EntityFrameworkCore. InMemory** .</span><span class="sxs-lookup"><span data-stu-id="e2912-515">Use the preceding instructions to add the **Microsoft.EntityFrameworkCore.InMemory** NuGet package.</span></span>

![Gestionnaire de package NuGet](first-web-api/_static/vs3NuGet.png)

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="e2912-517">Ajouter le contexte de base de données TodoContext</span><span class="sxs-lookup"><span data-stu-id="e2912-517">Add the TodoContext database context</span></span>

* <span data-ttu-id="e2912-518">Cliquez avec le bouton droit sur le *:::no-loc(Models):::* dossier et sélectionnez **Ajouter** une  >  **classe**.</span><span class="sxs-lookup"><span data-stu-id="e2912-518">Right-click the *:::no-loc(Models):::* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="e2912-519">Nommez la classe *TodoContext* et cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="e2912-519">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="e2912-520">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="e2912-520">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="e2912-521">Ajoutez une `TodoContext` classe au *:::no-loc(Models):::* dossier.</span><span class="sxs-lookup"><span data-stu-id="e2912-521">Add a `TodoContext` class to the *:::no-loc(Models):::* folder.</span></span>

---

* <span data-ttu-id="e2912-522">Entrez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e2912-522">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/:::no-loc(Models):::/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="e2912-523">Inscrire le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="e2912-523">Register the database context</span></span>

<span data-ttu-id="e2912-524">Dans ASP.NET Core, les services tels que le contexte de base de données doivent être inscrits auprès du [conteneur d’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="e2912-524">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="e2912-525">Le conteneur fournit le service aux contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="e2912-525">The container provides the service to controllers.</span></span>

<span data-ttu-id="e2912-526">Mettez à jour *Startup.cs* avec le code en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="e2912-526">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="e2912-527">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="e2912-527">The preceding code:</span></span>

* <span data-ttu-id="e2912-528">Supprime les déclarations `using` inutilisées.</span><span class="sxs-lookup"><span data-stu-id="e2912-528">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="e2912-529">Ajoute le contexte de base de données au conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="e2912-529">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="e2912-530">Spécifie que le contexte de base de données utilise une base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="e2912-530">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="e2912-531">Générer automatiquement des modèles pour un contrôleur</span><span class="sxs-lookup"><span data-stu-id="e2912-531">Scaffold a controller</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e2912-532">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e2912-532">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e2912-533">Cliquez avec le bouton droit sur le dossier *Contrôleurs*.</span><span class="sxs-lookup"><span data-stu-id="e2912-533">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="e2912-534">Sélectionnez **Ajouter** > **Nouvel élément généré automatiquement**.</span><span class="sxs-lookup"><span data-stu-id="e2912-534">Select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="e2912-535">Sélectionnez **Contrôleur d’API avec actions, utilisant Entity Framework** , puis **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="e2912-535">Select **API Controller with actions, using Entity Framework** , and then select **Add**.</span></span>
* <span data-ttu-id="e2912-536">Dans la boîte de dialogue **Contrôleur d’API avec actions, utilisant Entity Framework**  :</span><span class="sxs-lookup"><span data-stu-id="e2912-536">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>

  * <span data-ttu-id="e2912-537">Sélectionnez **TodoItem (TodoApi. :::no-loc(Models)::: )** dans la **classe de modèle**.</span><span class="sxs-lookup"><span data-stu-id="e2912-537">Select **TodoItem (TodoApi.:::no-loc(Models):::)** in the **Model class**.</span></span>
  * <span data-ttu-id="e2912-538">Sélectionnez **TodoContext (TodoApi. :::no-loc(Models)::: )** dans la **classe de contexte de données**.</span><span class="sxs-lookup"><span data-stu-id="e2912-538">Select **TodoContext (TodoApi.:::no-loc(Models):::)** in the **Data context class**.</span></span>
  * <span data-ttu-id="e2912-539">Sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="e2912-539">Select **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="e2912-540">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="e2912-540">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="e2912-541">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="e2912-541">Run the following commands:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet tool update -g Dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext -outDir Controllers
```

<span data-ttu-id="e2912-542">Les commandes précédentes :</span><span class="sxs-lookup"><span data-stu-id="e2912-542">The preceding commands:</span></span>

* <span data-ttu-id="e2912-543">Ajoutent les packages NuGet nécessaires à la génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="e2912-543">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="e2912-544">Installent le moteur de génération de modèles automatique (`dotnet-aspnet-codegenerator`).</span><span class="sxs-lookup"><span data-stu-id="e2912-544">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="e2912-545">Génèrent automatiquement des modèles pour `TodoItemsController`.</span><span class="sxs-lookup"><span data-stu-id="e2912-545">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="e2912-546">Le code généré :</span><span class="sxs-lookup"><span data-stu-id="e2912-546">The generated code:</span></span>

* <span data-ttu-id="e2912-547">Marque la classe avec l' [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribut.</span><span class="sxs-lookup"><span data-stu-id="e2912-547">Marks the class with the [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute.</span></span> <span data-ttu-id="e2912-548">Cet attribut indique que le contrôleur répond aux requêtes de l’API web.</span><span class="sxs-lookup"><span data-stu-id="e2912-548">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="e2912-549">Pour plus d’informations sur les comportements spécifiques que permet l’attribut, consultez <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="e2912-549">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="e2912-550">Utilise l’injection de dépendances pour injecter le contexte de base de données (`TodoContext`) dans le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="e2912-550">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="e2912-551">Le contexte de base de données est utilisé dans chacune des méthodes la [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="e2912-551">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<span data-ttu-id="e2912-552">Les modèles de ASP.NET Core pour :</span><span class="sxs-lookup"><span data-stu-id="e2912-552">The ASP.NET Core templates for:</span></span>

* <span data-ttu-id="e2912-553">Les contrôleurs avec des vues sont inclus `[action]` dans le modèle de routage.</span><span class="sxs-lookup"><span data-stu-id="e2912-553">Controllers with views include `[action]` in the route template.</span></span>
* <span data-ttu-id="e2912-554">Les contrôleurs d’API n’incluent pas `[action]` dans le modèle de routage.</span><span class="sxs-lookup"><span data-stu-id="e2912-554">API controllers don't include `[action]` in the route template.</span></span>

<span data-ttu-id="e2912-555">Lorsque le `[action]` jeton n’est pas dans le modèle de routage, le nom de l' [action](xref:mvc/controllers/routing#action) est exclu de l’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="e2912-555">When the `[action]` token isn't in the route template, the [action](xref:mvc/controllers/routing#action) name is excluded from the route.</span></span> <span data-ttu-id="e2912-556">Autrement dit, le nom de la méthode associée à l’action n’est pas utilisé dans l’itinéraire correspondant.</span><span class="sxs-lookup"><span data-stu-id="e2912-556">That is, the action's associated method name isn't used in the matching route.</span></span>

## <a name="examine-the-posttodoitem-create-method"></a><span data-ttu-id="e2912-557">Examiner la méthode de création de PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="e2912-557">Examine the PostTodoItem create method</span></span>

<span data-ttu-id="e2912-558">Remplacez l’instruction return dans `PostTodoItem` pour utiliser l’opérateur [nameof](/dotnet/csharp/language-reference/operators/nameof) :</span><span class="sxs-lookup"><span data-stu-id="e2912-558">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="e2912-559">Le code précédent est une méthode HTTP Après, comme indiqué par l' [`[HttpPost]`](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute) attribut.</span><span class="sxs-lookup"><span data-stu-id="e2912-559">The preceding code is an HTTP POST method, as indicated by the [`[HttpPost]`](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute) attribute.</span></span> <span data-ttu-id="e2912-560">La méthode obtient la valeur de la tâche dans le corps de la requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="e2912-560">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="e2912-561">Pour plus d’informations, consultez [Routage par attributs avec des attributs Http[Verbe]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="e2912-561">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="e2912-562">La méthode <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> :</span><span class="sxs-lookup"><span data-stu-id="e2912-562">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="e2912-563">Retourne un code d’état HTTP 201 en cas de réussite.</span><span class="sxs-lookup"><span data-stu-id="e2912-563">Returns an HTTP 201 status code if successful.</span></span> <span data-ttu-id="e2912-564">HTTP 201 est la réponse standard d’une méthode HTTP POST qui crée une ressource sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="e2912-564">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="e2912-565">Ajoute un en-tête [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) à la réponse.</span><span class="sxs-lookup"><span data-stu-id="e2912-565">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="e2912-566">L’en-tête `Location` spécifie l’[URI](https://developer.mozilla.org/docs/Glossary/URI) de l’élément d’action qui vient d’être créé.</span><span class="sxs-lookup"><span data-stu-id="e2912-566">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="e2912-567">Pour plus d’informations, consultez la section [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="e2912-567">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="e2912-568">Fait référence à l’action `GetTodoItem` pour créer l’URI `Location` de l’en-tête.</span><span class="sxs-lookup"><span data-stu-id="e2912-568">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="e2912-569">Le mot clé `nameof` C# est utilisé pour éviter de coder en dur le nom de l’action dans l’appel `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="e2912-569">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="e2912-570">Installer Postman</span><span class="sxs-lookup"><span data-stu-id="e2912-570">Install Postman</span></span>

<span data-ttu-id="e2912-571">Ce tutoriel utilise Postman pour tester l’API web.</span><span class="sxs-lookup"><span data-stu-id="e2912-571">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="e2912-572">Installer le [poste](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="e2912-572">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="e2912-573">Démarrez l’application web.</span><span class="sxs-lookup"><span data-stu-id="e2912-573">Start the web app.</span></span>
* <span data-ttu-id="e2912-574">Démarrez Postman.</span><span class="sxs-lookup"><span data-stu-id="e2912-574">Start Postman.</span></span>
* <span data-ttu-id="e2912-575">Désactivez la **vérification du certificat SSL**.</span><span class="sxs-lookup"><span data-stu-id="e2912-575">Disable **SSL certificate verification**</span></span>
  * <span data-ttu-id="e2912-576">À partir de **Fichier** > **Paramètres** (onglet **Général** ), désactivez **Vérification du certificat SSL**.</span><span class="sxs-lookup"><span data-stu-id="e2912-576">From **File** > **Settings** ( **General** tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="e2912-577">Réactivez la vérification du certificat SSL après avoir testé le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="e2912-577">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="e2912-578">Tester PostTodoItem avec Postman</span><span class="sxs-lookup"><span data-stu-id="e2912-578">Test PostTodoItem with Postman</span></span>

* <span data-ttu-id="e2912-579">Créez une requête.</span><span class="sxs-lookup"><span data-stu-id="e2912-579">Create a new request.</span></span>
* <span data-ttu-id="e2912-580">Affectez `POST` à la méthode HTTP.</span><span class="sxs-lookup"><span data-stu-id="e2912-580">Set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="e2912-581">Définissez l’URI sur `https://localhost:<port>/api/TodoItems` .</span><span class="sxs-lookup"><span data-stu-id="e2912-581">Set the URI to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="e2912-582">Par exemple : `https://localhost:5001/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="e2912-582">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="e2912-583">Sélectionnez l’onglet **Corps** .</span><span class="sxs-lookup"><span data-stu-id="e2912-583">Select the **Body** tab.</span></span>
* <span data-ttu-id="e2912-584">Sélectionnez la case d’option **raw** (données brutes).</span><span class="sxs-lookup"><span data-stu-id="e2912-584">Select the **raw** radio button.</span></span>
* <span data-ttu-id="e2912-585">Définissez le type sur **JSON (application/json)**.</span><span class="sxs-lookup"><span data-stu-id="e2912-585">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="e2912-586">Dans le corps de la demande, entrez la syntaxe JSON d’une tâche :</span><span class="sxs-lookup"><span data-stu-id="e2912-586">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="e2912-587">Sélectionnez **Envoyer**.</span><span class="sxs-lookup"><span data-stu-id="e2912-587">Select **Send**.</span></span>

  ![Postman avec requête de création](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri-with-postman"></a><span data-ttu-id="e2912-589">Tester l’URI d’en-tête d’emplacement avec le poster</span><span class="sxs-lookup"><span data-stu-id="e2912-589">Test the location header URI with Postman</span></span>

* <span data-ttu-id="e2912-590">Sélectionnez l’onglet **Headers** (En-têtes) dans le volet **Response** (Réponse).</span><span class="sxs-lookup"><span data-stu-id="e2912-590">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="e2912-591">Copiez la valeur d’en-tête **Location** (Emplacement) :</span><span class="sxs-lookup"><span data-stu-id="e2912-591">Copy the **Location** header value:</span></span>

  ![Onglet Headers de la console Postman](first-web-api/_static/3/create.png)

* <span data-ttu-id="e2912-593">Affectez `GET` à la méthode HTTP.</span><span class="sxs-lookup"><span data-stu-id="e2912-593">Set the HTTP method to `GET`.</span></span>
* <span data-ttu-id="e2912-594">Définissez l’URI sur `https://localhost:<port>/api/TodoItems/1` .</span><span class="sxs-lookup"><span data-stu-id="e2912-594">Set the URI to `https://localhost:<port>/api/TodoItems/1`.</span></span> <span data-ttu-id="e2912-595">Par exemple : `https://localhost:5001/api/TodoItems/1`.</span><span class="sxs-lookup"><span data-stu-id="e2912-595">For example, `https://localhost:5001/api/TodoItems/1`.</span></span>
* <span data-ttu-id="e2912-596">Sélectionnez **Envoyer**.</span><span class="sxs-lookup"><span data-stu-id="e2912-596">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="e2912-597">Examiner les méthodes GET</span><span class="sxs-lookup"><span data-stu-id="e2912-597">Examine the GET methods</span></span>

<span data-ttu-id="e2912-598">Ces méthodes implémentent deux points de terminaison GET :</span><span class="sxs-lookup"><span data-stu-id="e2912-598">These methods implement two GET endpoints:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="e2912-599">Testez l’application en appelant les deux points de terminaison à partir d’un navigateur ou de Postman.</span><span class="sxs-lookup"><span data-stu-id="e2912-599">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="e2912-600">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="e2912-600">For example:</span></span>

* `https://localhost:5001/api/TodoItems`
* `https://localhost:5001/api/TodoItems/1`

<span data-ttu-id="e2912-601">Une réponse semblable à la suivante est produite par l’appel à `GetTodoItems` :</span><span class="sxs-lookup"><span data-stu-id="e2912-601">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="e2912-602">Tester Get avec Postman</span><span class="sxs-lookup"><span data-stu-id="e2912-602">Test Get with Postman</span></span>

* <span data-ttu-id="e2912-603">Créez une requête.</span><span class="sxs-lookup"><span data-stu-id="e2912-603">Create a new request.</span></span>
* <span data-ttu-id="e2912-604">Définissez la méthode HTTP sur **GET**.</span><span class="sxs-lookup"><span data-stu-id="e2912-604">Set the HTTP method to **GET**.</span></span>
* <span data-ttu-id="e2912-605">Définissez l’URI de la demande sur `https://localhost:<port>/api/TodoItems` .</span><span class="sxs-lookup"><span data-stu-id="e2912-605">Set the request URI to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="e2912-606">Par exemple : `https://localhost:5001/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="e2912-606">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="e2912-607">Définissez l’ **affichage à deux volets** dans Postman.</span><span class="sxs-lookup"><span data-stu-id="e2912-607">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="e2912-608">Sélectionnez **Envoyer**.</span><span class="sxs-lookup"><span data-stu-id="e2912-608">Select **Send**.</span></span>

<span data-ttu-id="e2912-609">Cette application utilise une base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="e2912-609">This app uses an in-memory database.</span></span> <span data-ttu-id="e2912-610">Si l’application est arrêtée et démarrée, la requête GET précédente ne retourne aucune donnée.</span><span class="sxs-lookup"><span data-stu-id="e2912-610">If the app is stopped and started, the preceding GET request will not return any data.</span></span> <span data-ttu-id="e2912-611">Si aucune donnée n’est retournée, publiez ([POST](#post)) les données dans l’application.</span><span class="sxs-lookup"><span data-stu-id="e2912-611">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="e2912-612">Routage et chemins d’URL</span><span class="sxs-lookup"><span data-stu-id="e2912-612">Routing and URL paths</span></span>

<span data-ttu-id="e2912-613">L' [`[HttpGet]`](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) attribut désigne une méthode qui répond à une requête http.</span><span class="sxs-lookup"><span data-stu-id="e2912-613">The [`[HttpGet]`](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="e2912-614">Le chemin d’URL pour chaque méthode est construit comme suit :</span><span class="sxs-lookup"><span data-stu-id="e2912-614">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="e2912-615">Partez de la chaîne de modèle dans l’attribut `Route` du contrôleur :</span><span class="sxs-lookup"><span data-stu-id="e2912-615">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="e2912-616">Remplacez `[controller]` par le nom du contrôleur qui, par convention, est le nom de la classe du contrôleur sans le suffixe « Controller ».</span><span class="sxs-lookup"><span data-stu-id="e2912-616">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="e2912-617">Pour cet exemple, le nom de la classe du contrôleur étant **TodoItems** Controller, le nom du contrôleur est « TodoItems ».</span><span class="sxs-lookup"><span data-stu-id="e2912-617">For this sample, the controller class name is **TodoItems** Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="e2912-618">Le [routage](xref:mvc/controllers/routing) d’ASP.NET Core ne respecte pas la casse.</span><span class="sxs-lookup"><span data-stu-id="e2912-618">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="e2912-619">Si l’attribut `[HttpGet]` a un modèle de route (par exemple, `[HttpGet("products")]`), ajoutez-le au chemin.</span><span class="sxs-lookup"><span data-stu-id="e2912-619">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="e2912-620">Cet exemple n’utilise pas de modèle.</span><span class="sxs-lookup"><span data-stu-id="e2912-620">This sample doesn't use a template.</span></span> <span data-ttu-id="e2912-621">Pour plus d’informations, consultez [Routage par attributs avec des attributs Http[Verbe]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="e2912-621">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="e2912-622">Dans la méthode `GetTodoItem` suivante, `"{id}"` est une variable d’espace réservé pour l’identificateur unique de la tâche.</span><span class="sxs-lookup"><span data-stu-id="e2912-622">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="e2912-623">Lorsque `GetTodoItem` est appelé, la valeur de `"{id}"` dans l’URL est fournie à la méthode dans son `id` paramètre.</span><span class="sxs-lookup"><span data-stu-id="e2912-623">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its `id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="e2912-624">Valeurs retournées</span><span class="sxs-lookup"><span data-stu-id="e2912-624">Return values</span></span> 

<span data-ttu-id="e2912-625">Le type de retour des `GetTodoItems` `GetTodoItem` méthodes et est [ActionResult \<T> type](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="e2912-625">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="e2912-626">ASP.NET Core sérialise automatiquement l’objet en [JSON](https://www.json.org/) et écrit le JSON dans le corps du message de réponse.</span><span class="sxs-lookup"><span data-stu-id="e2912-626">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="e2912-627">Le code de réponse pour ce type de retour est 200, en supposant qu’il n’existe pas d’exception non gérée.</span><span class="sxs-lookup"><span data-stu-id="e2912-627">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="e2912-628">Les exceptions non gérées sont converties en erreurs 5xx.</span><span class="sxs-lookup"><span data-stu-id="e2912-628">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="e2912-629">Les types de retour `ActionResult` peuvent représenter une large plage de codes d’état HTTP.</span><span class="sxs-lookup"><span data-stu-id="e2912-629">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="e2912-630">Par exemple, `GetTodoItem` peut retourner deux valeurs d’état différentes :</span><span class="sxs-lookup"><span data-stu-id="e2912-630">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="e2912-631">Si aucun élément ne correspond à l’ID demandé, la méthode retourne un <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound%2A> code d’erreur 404.</span><span class="sxs-lookup"><span data-stu-id="e2912-631">If no item matches the requested ID, the method returns a 404 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound%2A> error code.</span></span>
* <span data-ttu-id="e2912-632">Sinon, la méthode retourne 200 avec un corps de réponse JSON.</span><span class="sxs-lookup"><span data-stu-id="e2912-632">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="e2912-633">Le retour de `item` entraîne une réponse HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="e2912-633">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="e2912-634">Méthode PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="e2912-634">The PutTodoItem method</span></span>

<span data-ttu-id="e2912-635">Examinez la méthode `PutTodoItem` :</span><span class="sxs-lookup"><span data-stu-id="e2912-635">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="e2912-636">`PutTodoItem` est similaire à `PostTodoItem`, à la différence près qu’il utilise HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="e2912-636">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="e2912-637">La réponse est [204 (aucun contenu)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="e2912-637">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="e2912-638">D’après la spécification HTTP, une requête PUT nécessite que le client envoie toute l’entité mise à jour, et pas seulement les changements.</span><span class="sxs-lookup"><span data-stu-id="e2912-638">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="e2912-639">Pour prendre en charge les mises à jour partielles, utilisez [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="e2912-639">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="e2912-640">Si vous obtenez une erreur en appelant `PutTodoItem`, appelez `GET` pour vérifier que la base de données contient un élément.</span><span class="sxs-lookup"><span data-stu-id="e2912-640">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="e2912-641">Tester la méthode PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="e2912-641">Test the PutTodoItem method</span></span>

<span data-ttu-id="e2912-642">Cet exemple utilise une base de données en mémoire qui doit être initialisée chaque fois que l’application est démarrée.</span><span class="sxs-lookup"><span data-stu-id="e2912-642">This sample uses an in-memory database that must be initialized each time the app is started.</span></span> <span data-ttu-id="e2912-643">La base de données doit contenir un élément avant que vous ne passiez un appel PUT.</span><span class="sxs-lookup"><span data-stu-id="e2912-643">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="e2912-644">Appelez obtenir pour vous assurer qu’il y a un élément dans la base de données avant d’effectuer un appel PUT.</span><span class="sxs-lookup"><span data-stu-id="e2912-644">Call GET to ensure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="e2912-645">Mettez à jour l’élément de tâche dont l’ID est égal à 1 et affectez-lui le nom « flux de poisson » :</span><span class="sxs-lookup"><span data-stu-id="e2912-645">Update the to-do item that has Id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "id":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="e2912-646">L’image suivante montre la mise à jour Postman :</span><span class="sxs-lookup"><span data-stu-id="e2912-646">The following image shows the Postman update:</span></span>

![Console Postman montrant la réponse 204 (Pas de contenu)](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="e2912-648">Méthode DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="e2912-648">The DeleteTodoItem method</span></span>

<span data-ttu-id="e2912-649">Examinez la méthode `DeleteTodoItem` :</span><span class="sxs-lookup"><span data-stu-id="e2912-649">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="e2912-650">Tester la méthode DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="e2912-650">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="e2912-651">Utilisez Postman pour supprimer une tâche :</span><span class="sxs-lookup"><span data-stu-id="e2912-651">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="e2912-652">Définissez la méthode sur `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="e2912-652">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="e2912-653">Définissez l’URI de l’objet à supprimer (par exemple `https://localhost:5001/api/TodoItems/1` ).</span><span class="sxs-lookup"><span data-stu-id="e2912-653">Set the URI of the object to delete (for example `https://localhost:5001/api/TodoItems/1`).</span></span>
* <span data-ttu-id="e2912-654">Sélectionnez **Envoyer**.</span><span class="sxs-lookup"><span data-stu-id="e2912-654">Select **Send**.</span></span>

<a name="over-post"></a>
<a name="over-post-v3"></a>

## <a name="prevent-over-posting"></a><span data-ttu-id="e2912-655">Empêcher la post-validation</span><span class="sxs-lookup"><span data-stu-id="e2912-655">Prevent over-posting</span></span>

<span data-ttu-id="e2912-656">Actuellement, l’exemple d’application expose l' `TodoItem` objet entier.</span><span class="sxs-lookup"><span data-stu-id="e2912-656">Currently the sample app exposes the entire `TodoItem` object.</span></span> <span data-ttu-id="e2912-657">En général, les applications de production limitent les données entrées et retournées à l’aide d’un sous-ensemble du modèle.</span><span class="sxs-lookup"><span data-stu-id="e2912-657">Production apps typically limit the data that's input and returned using a subset of the model.</span></span> <span data-ttu-id="e2912-658">Il existe plusieurs raisons à cela, et la sécurité est essentielle.</span><span class="sxs-lookup"><span data-stu-id="e2912-658">There are multiple reasons behind this and security is a major one.</span></span> <span data-ttu-id="e2912-659">Le sous-ensemble d’un modèle est généralement appelé un objet Transfert de données (DTO), un modèle d’entrée ou un modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="e2912-659">The subset of a model is usually referred to as a Data Transfer Object (DTO), input model, or view model.</span></span> <span data-ttu-id="e2912-660">Le **DTO** est utilisé dans cet article.</span><span class="sxs-lookup"><span data-stu-id="e2912-660">**DTO** is used in this article.</span></span>

<span data-ttu-id="e2912-661">Un DTO peut être utilisé pour :</span><span class="sxs-lookup"><span data-stu-id="e2912-661">A DTO may be used to:</span></span>

* <span data-ttu-id="e2912-662">Empêcher la survalidation.</span><span class="sxs-lookup"><span data-stu-id="e2912-662">Prevent over-posting.</span></span>
* <span data-ttu-id="e2912-663">Masquer les propriétés que les clients ne sont pas censés afficher.</span><span class="sxs-lookup"><span data-stu-id="e2912-663">Hide properties that clients are not supposed to view.</span></span>
* <span data-ttu-id="e2912-664">Omettez certaines propriétés afin de réduire la taille de la charge utile.</span><span class="sxs-lookup"><span data-stu-id="e2912-664">Omit some properties in order to reduce payload size.</span></span>
* <span data-ttu-id="e2912-665">Aplatir les graphiques d’objets qui contiennent des objets imbriqués.</span><span class="sxs-lookup"><span data-stu-id="e2912-665">Flatten object graphs that contain nested objects.</span></span> <span data-ttu-id="e2912-666">Les graphiques d’objets aplatis peuvent être plus pratiques pour les clients.</span><span class="sxs-lookup"><span data-stu-id="e2912-666">Flattened object graphs can be more convenient for clients.</span></span>

<span data-ttu-id="e2912-667">Pour illustrer l’approche DTO, mettez à jour la `TodoItem` classe pour inclure un champ secret :</span><span class="sxs-lookup"><span data-stu-id="e2912-667">To demonstrate the DTO approach, update the `TodoItem` class to include a secret field:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApiDTO/:::no-loc(Models):::/TodoItem.cs?name=snippet&highlight=6)]

<span data-ttu-id="e2912-668">Le champ secret doit être masqué à partir de cette application, mais une application administrative peut choisir de l’exposer.</span><span class="sxs-lookup"><span data-stu-id="e2912-668">The secret field needs to be hidden from this app, but an administrative app could choose to expose it.</span></span>

<span data-ttu-id="e2912-669">Vérifiez que vous pouvez poster et recevoir le champ secret.</span><span class="sxs-lookup"><span data-stu-id="e2912-669">Verify you can post and get the secret field.</span></span>

<span data-ttu-id="e2912-670">Créer un modèle DTO :</span><span class="sxs-lookup"><span data-stu-id="e2912-670">Create a DTO model:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApiDTO/:::no-loc(Models):::/TodoItemDTO.cs?name=snippet)]

<span data-ttu-id="e2912-671">Mettez à jour le `TodoItemsController` pour utiliser `TodoItemDTO` :</span><span class="sxs-lookup"><span data-stu-id="e2912-671">Update the `TodoItemsController` to use `TodoItemDTO`:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApiDTO/Controllers/TodoItemsController.cs?name=snippet)]

<span data-ttu-id="e2912-672">Vérifiez que vous ne pouvez pas poster ou recevoir le champ secret.</span><span class="sxs-lookup"><span data-stu-id="e2912-672">Verify you can't post or get the secret field.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="e2912-673">Appelez l’API web avec JavaScript</span><span class="sxs-lookup"><span data-stu-id="e2912-673">Call the web API with JavaScript</span></span>

<span data-ttu-id="e2912-674">Consultez [Didacticiel : appeler une API web ASP.net core avec JavaScript](xref:tutorials/web-api-javascript).</span><span class="sxs-lookup"><span data-stu-id="e2912-674">See [Tutorial: Call an ASP.NET Core web API with JavaScript](xref:tutorials/web-api-javascript).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e2912-675">Dans ce tutoriel, vous allez apprendre à :</span><span class="sxs-lookup"><span data-stu-id="e2912-675">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e2912-676">Créer un projet d’API web.</span><span class="sxs-lookup"><span data-stu-id="e2912-676">Create a web API project.</span></span>
> * <span data-ttu-id="e2912-677">Ajouter une classe de modèle et un contexte de base de données.</span><span class="sxs-lookup"><span data-stu-id="e2912-677">Add a model class and a database context.</span></span>
> * <span data-ttu-id="e2912-678">Ajouter un contrôleur</span><span class="sxs-lookup"><span data-stu-id="e2912-678">Add a controller.</span></span>
> * <span data-ttu-id="e2912-679">Ajouter les méthodes CRUD</span><span class="sxs-lookup"><span data-stu-id="e2912-679">Add CRUD methods.</span></span>
> * <span data-ttu-id="e2912-680">Configurer le routage et les chemins d’URL</span><span class="sxs-lookup"><span data-stu-id="e2912-680">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="e2912-681">Spécifier des valeurs de retour</span><span class="sxs-lookup"><span data-stu-id="e2912-681">Specify return values.</span></span>
> * <span data-ttu-id="e2912-682">Appeler l’API web avec Postman</span><span class="sxs-lookup"><span data-stu-id="e2912-682">Call the web API with Postman.</span></span>
> * <span data-ttu-id="e2912-683">Appeler l’API web avec JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e2912-683">Call the web API with JavaScript.</span></span>

<span data-ttu-id="e2912-684">À la fin, vous disposez d’une API web qui peut gérer des tâches stockées dans une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="e2912-684">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>

## <a name="overview-21"></a><span data-ttu-id="e2912-685">Vue d’ensemble 2,1</span><span class="sxs-lookup"><span data-stu-id="e2912-685">Overview 2.1</span></span>

<span data-ttu-id="e2912-686">Ce didacticiel crée l’API suivante :</span><span class="sxs-lookup"><span data-stu-id="e2912-686">This tutorial creates the following API:</span></span>

|<span data-ttu-id="e2912-687">API</span><span class="sxs-lookup"><span data-stu-id="e2912-687">API</span></span> | <span data-ttu-id="e2912-688">Description</span><span class="sxs-lookup"><span data-stu-id="e2912-688">Description</span></span> | <span data-ttu-id="e2912-689">Corps de la demande</span><span class="sxs-lookup"><span data-stu-id="e2912-689">Request body</span></span> | <span data-ttu-id="e2912-690">Response body</span><span class="sxs-lookup"><span data-stu-id="e2912-690">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="e2912-691">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="e2912-691">GET /api/TodoItems</span></span> | <span data-ttu-id="e2912-692">Obtenir toutes les tâches</span><span class="sxs-lookup"><span data-stu-id="e2912-692">Get all to-do items</span></span> | <span data-ttu-id="e2912-693">Aucun</span><span class="sxs-lookup"><span data-stu-id="e2912-693">None</span></span> | <span data-ttu-id="e2912-694">Tableau de tâches</span><span class="sxs-lookup"><span data-stu-id="e2912-694">Array of to-do items</span></span>|
|<span data-ttu-id="e2912-695">GET /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="e2912-695">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="e2912-696">Obtenir un élément par ID</span><span class="sxs-lookup"><span data-stu-id="e2912-696">Get an item by ID</span></span> | <span data-ttu-id="e2912-697">Aucun</span><span class="sxs-lookup"><span data-stu-id="e2912-697">None</span></span> | <span data-ttu-id="e2912-698">Tâche</span><span class="sxs-lookup"><span data-stu-id="e2912-698">To-do item</span></span>|
|<span data-ttu-id="e2912-699">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="e2912-699">POST /api/TodoItems</span></span> | <span data-ttu-id="e2912-700">Ajouter un nouvel élément</span><span class="sxs-lookup"><span data-stu-id="e2912-700">Add a new item</span></span> | <span data-ttu-id="e2912-701">Tâche</span><span class="sxs-lookup"><span data-stu-id="e2912-701">To-do item</span></span> | <span data-ttu-id="e2912-702">Tâche</span><span class="sxs-lookup"><span data-stu-id="e2912-702">To-do item</span></span> |
|<span data-ttu-id="e2912-703">PUT /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="e2912-703">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="e2912-704">Mettre à jour un élément existant &nbsp;</span><span class="sxs-lookup"><span data-stu-id="e2912-704">Update an existing item &nbsp;</span></span> | <span data-ttu-id="e2912-705">Tâche</span><span class="sxs-lookup"><span data-stu-id="e2912-705">To-do item</span></span> | <span data-ttu-id="e2912-706">Aucun</span><span class="sxs-lookup"><span data-stu-id="e2912-706">None</span></span> |
|<span data-ttu-id="e2912-707">SUPPRIMER/api/TodoItems/{id} &nbsp;&nbsp;</span><span class="sxs-lookup"><span data-stu-id="e2912-707">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="e2912-708">Supprimer un élément &nbsp;&nbsp;</span><span class="sxs-lookup"><span data-stu-id="e2912-708">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="e2912-709">None</span><span class="sxs-lookup"><span data-stu-id="e2912-709">None</span></span> | <span data-ttu-id="e2912-710">None</span><span class="sxs-lookup"><span data-stu-id="e2912-710">None</span></span>|

<span data-ttu-id="e2912-711">Le diagramme suivant illustre la conception de l’application.</span><span class="sxs-lookup"><span data-stu-id="e2912-711">The following diagram shows the design of the app.</span></span>

![Le client est représenté par une zone située à gauche.](first-web-api/_static/architecture.png)

## <a name="prerequisites-21"></a><span data-ttu-id="e2912-717">Conditions préalables 2,1</span><span class="sxs-lookup"><span data-stu-id="e2912-717">Prerequisites 2.1</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e2912-718">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e2912-718">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="e2912-719">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e2912-719">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="e2912-720">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="e2912-720">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project-21"></a><span data-ttu-id="e2912-721">Créer un projet Web 2,1</span><span class="sxs-lookup"><span data-stu-id="e2912-721">Create a web project 2.1</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e2912-722">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e2912-722">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e2912-723">Dans le menu **fichier** , sélectionnez **nouveau** > **projet**.</span><span class="sxs-lookup"><span data-stu-id="e2912-723">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="e2912-724">Sélectionnez le modèle **Application web ASP.NET Core** et cliquez sur **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="e2912-724">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="e2912-725">Nommez le projet *TodoApi* et cliquez sur **Créer**.</span><span class="sxs-lookup"><span data-stu-id="e2912-725">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="e2912-726">Dans la boîte de dialogue **Créer une application web ASP.NET Core** , vérifiez que **.NET Core** et **ASP.NET Core 2.2** sont sélectionnés.</span><span class="sxs-lookup"><span data-stu-id="e2912-726">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 2.2** are selected.</span></span> <span data-ttu-id="e2912-727">Sélectionnez le modèle **API** et cliquez sur **Créer**.</span><span class="sxs-lookup"><span data-stu-id="e2912-727">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="e2912-728">Ne sélectionnez **pas\*\*\*\*Activer la prise en charge de Docker**.</span><span class="sxs-lookup"><span data-stu-id="e2912-728">**Don't** select **Enable Docker Support**.</span></span>

![Boîte de dialogue de nouveau projet dans VS](first-web-api/_static/vs.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="e2912-730">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e2912-730">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e2912-731">Ouvrez le [Terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="e2912-731">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="e2912-732">Définissez les répertoires (`cd`) sur le dossier destiné à contenir le dossier du projet.</span><span class="sxs-lookup"><span data-stu-id="e2912-732">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="e2912-733">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="e2912-733">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="e2912-734">Ces commandes créent un projet d’API web et ouvrent une nouvelle instance de Visual Studio Code dans le nouveau dossier de projet.</span><span class="sxs-lookup"><span data-stu-id="e2912-734">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="e2912-735">Quand une boîte de dialogue vous demande si vous souhaitez ajouter les composants nécessaires au projet, sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="e2912-735">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="e2912-736">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="e2912-736">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e2912-737">Sélectionnez **Fichier** > **Nouvelle solution**.</span><span class="sxs-lookup"><span data-stu-id="e2912-737">Select **File** > **New Solution**.</span></span>

  ![macOS - Nouvelle solution](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="e2912-739">Dans Visual Studio pour Mac antérieure à la version 8,6, sélectionnez API de l’application **.net Core**  >  **App**  >  **API**  >  **suivant**.</span><span class="sxs-lookup"><span data-stu-id="e2912-739">In Visual Studio for Mac earlier than version 8.6, select **.NET Core** > **App** > **API** > **Next**.</span></span> <span data-ttu-id="e2912-740">Dans la version 8,6 ou une version ultérieure, sélectionnez application **Web et**  >  **App**  >  **API** application console  >  **suivant**.</span><span class="sxs-lookup"><span data-stu-id="e2912-740">In version 8.6 or later, select **Web and Console** > **App** > **API** > **Next**.</span></span>
  
* <span data-ttu-id="e2912-741">Dans la boîte de dialogue **configurer la nouvelle API Web ASP.net Core** , sélectionnez la version la plus récente de .net Core 2. x **Target Framework**.</span><span class="sxs-lookup"><span data-stu-id="e2912-741">In the **Configure the new ASP.NET Core Web API** dialog, select the latest .NET Core 2.x **Target Framework**.</span></span> <span data-ttu-id="e2912-742">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="e2912-742">Select **Next**.</span></span>

* <span data-ttu-id="e2912-743">Entrez *TodoApi* comme **Nom du projet** , puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="e2912-743">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![boîte de dialogue de configuration](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api-21"></a><span data-ttu-id="e2912-745">Tester l’API 2,1</span><span class="sxs-lookup"><span data-stu-id="e2912-745">Test the API 2.1</span></span>

<span data-ttu-id="e2912-746">Le modèle de projet crée une API `values`.</span><span class="sxs-lookup"><span data-stu-id="e2912-746">The project template creates a `values` API.</span></span> <span data-ttu-id="e2912-747">Appelez la méthode `Get` à partir d’un navigateur pour tester l’application.</span><span class="sxs-lookup"><span data-stu-id="e2912-747">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e2912-748">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e2912-748">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e2912-749">Appuyez sur Ctrl+F5 pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="e2912-749">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="e2912-750">Visual Studio lance un navigateur et accède à `https://localhost:<port>/api/values`, où `<port>` est un numéro de port choisi de manière aléatoire.</span><span class="sxs-lookup"><span data-stu-id="e2912-750">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="e2912-751">Si une boîte de dialogue apparaît vous demandant si vous devez approuver le certificat IIS Express, sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="e2912-751">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="e2912-752">Dans la boîte de dialogue **Avertissement de sécurité** qui s’affiche ensuite, sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="e2912-752">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="e2912-753">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e2912-753">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e2912-754">Appuyez sur Ctrl+F5 pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="e2912-754">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="e2912-755">Dans un navigateur, accédez à l’URL suivante : `https://localhost:5001/api/values`.</span><span class="sxs-lookup"><span data-stu-id="e2912-755">In a browser, go to following URL: `https://localhost:5001/api/values`.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="e2912-756">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="e2912-756">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="e2912-757">Sélectionnez **exécuter**  >  **Démarrer le débogage** pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="e2912-757">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="e2912-758">Visual Studio pour Mac lance un navigateur et accède à `https://localhost:<port>`, où `<port>` est un numéro de port choisi de manière aléatoire.</span><span class="sxs-lookup"><span data-stu-id="e2912-758">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="e2912-759">Une erreur HTTP 404 (introuvable) est retournée.</span><span class="sxs-lookup"><span data-stu-id="e2912-759">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="e2912-760">Ajoutez `/api/values` à l’URL (définissez-la sur `https://localhost:<port>/api/values`).</span><span class="sxs-lookup"><span data-stu-id="e2912-760">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="e2912-761">Le code JSON suivant est retourné :</span><span class="sxs-lookup"><span data-stu-id="e2912-761">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class-21"></a><span data-ttu-id="e2912-762">Ajouter une classe de modèle 2,1</span><span class="sxs-lookup"><span data-stu-id="e2912-762">Add a model class 2.1</span></span>

<span data-ttu-id="e2912-763">Un *modèle* est un ensemble de classes qui représentent les données gérées par l’application.</span><span class="sxs-lookup"><span data-stu-id="e2912-763">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="e2912-764">Le modèle pour cette application est une classe `TodoItem` unique.</span><span class="sxs-lookup"><span data-stu-id="e2912-764">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e2912-765">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e2912-765">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e2912-766">Dans l’ **Explorateur de solutions** , cliquez avec le bouton droit sur le projet.</span><span class="sxs-lookup"><span data-stu-id="e2912-766">In **Solution Explorer** , right-click the project.</span></span> <span data-ttu-id="e2912-767">Sélectionnez **Ajouter**  >  **un nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="e2912-767">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="e2912-768">Nommez le dossier *:::no-loc(Models):::* .</span><span class="sxs-lookup"><span data-stu-id="e2912-768">Name the folder *:::no-loc(Models):::*.</span></span>

* <span data-ttu-id="e2912-769">Cliquez avec le bouton droit sur le *:::no-loc(Models):::* dossier et sélectionnez **Ajouter** une  >  **classe**.</span><span class="sxs-lookup"><span data-stu-id="e2912-769">Right-click the *:::no-loc(Models):::* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="e2912-770">Nommez la classe *TodoItem* et sélectionnez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="e2912-770">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="e2912-771">Remplacez le code du modèle par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e2912-771">Replace the template code with the following code:</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="e2912-772">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e2912-772">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e2912-773">Ajoutez un dossier nommé *:::no-loc(Models):::* .</span><span class="sxs-lookup"><span data-stu-id="e2912-773">Add a folder named *:::no-loc(Models):::*.</span></span>

* <span data-ttu-id="e2912-774">Ajoutez une `TodoItem` classe au dossier à l' *:::no-loc(Models):::* aide du code suivant :</span><span class="sxs-lookup"><span data-stu-id="e2912-774">Add a `TodoItem` class to the *:::no-loc(Models):::* folder with the following code:</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="e2912-775">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="e2912-775">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e2912-776">Cliquez avec le bouton droit sur le projet.</span><span class="sxs-lookup"><span data-stu-id="e2912-776">Right-click the project.</span></span> <span data-ttu-id="e2912-777">Sélectionnez **Ajouter**  >  **un nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="e2912-777">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="e2912-778">Nommez le dossier *:::no-loc(Models):::* .</span><span class="sxs-lookup"><span data-stu-id="e2912-778">Name the folder *:::no-loc(Models):::*.</span></span>

  ![nouveau dossier](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="e2912-780">Cliquez avec le bouton droit sur le *:::no-loc(Models):::* dossier, puis sélectionnez **Ajouter** > **un nouveau fichier** > **General** > **classe générale vide**.</span><span class="sxs-lookup"><span data-stu-id="e2912-780">Right-click the *:::no-loc(Models):::* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="e2912-781">Nommez la classe *TodoItem* et cliquez sur **Nouveau**.</span><span class="sxs-lookup"><span data-stu-id="e2912-781">Name the class *TodoItem* , and then click **New**.</span></span>

* <span data-ttu-id="e2912-782">Remplacez le code du modèle par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e2912-782">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/:::no-loc(Models):::/TodoItem.cs)]

<span data-ttu-id="e2912-783">La propriété `Id` fonctionne comme la clé unique dans une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="e2912-783">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="e2912-784">Les classes de modèle peuvent se trouver n’importe où dans le projet, mais le *:::no-loc(Models):::* dossier est utilisé par Convention.</span><span class="sxs-lookup"><span data-stu-id="e2912-784">Model classes can go anywhere in the project, but the *:::no-loc(Models):::* folder is used by convention.</span></span>

## <a name="add-a-database-context-21"></a><span data-ttu-id="e2912-785">Ajouter un contexte de base de données 2,1</span><span class="sxs-lookup"><span data-stu-id="e2912-785">Add a database context 2.1</span></span>

<span data-ttu-id="e2912-786">Le *contexte de base de données* est la classe principale qui coordonne les fonctionnalités d’Entity Framework pour un modèle de données.</span><span class="sxs-lookup"><span data-stu-id="e2912-786">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="e2912-787">Cette classe est créée en dérivant de la classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="e2912-787">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e2912-788">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e2912-788">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e2912-789">Cliquez avec le bouton droit sur le *:::no-loc(Models):::* dossier et sélectionnez **Ajouter** une  >  **classe**.</span><span class="sxs-lookup"><span data-stu-id="e2912-789">Right-click the *:::no-loc(Models):::* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="e2912-790">Nommez la classe *TodoContext* et cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="e2912-790">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="e2912-791">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="e2912-791">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="e2912-792">Ajoutez une `TodoContext` classe au *:::no-loc(Models):::* dossier.</span><span class="sxs-lookup"><span data-stu-id="e2912-792">Add a `TodoContext` class to the *:::no-loc(Models):::* folder.</span></span>

---

* <span data-ttu-id="e2912-793">Remplacez le code du modèle par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e2912-793">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/:::no-loc(Models):::/TodoContext.cs)]

## <a name="register-the-database-context-21"></a><span data-ttu-id="e2912-794">Inscrire le contexte de base de données 2,1</span><span class="sxs-lookup"><span data-stu-id="e2912-794">Register the database context 2.1</span></span>

<span data-ttu-id="e2912-795">Dans ASP.NET Core, les services tels que le contexte de base de données doivent être inscrits auprès du [conteneur d’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="e2912-795">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="e2912-796">Le conteneur fournit le service aux contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="e2912-796">The container provides the service to controllers.</span></span>

<span data-ttu-id="e2912-797">Mettez à jour *Startup.cs* avec le code en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="e2912-797">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="e2912-798">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="e2912-798">The preceding code:</span></span>

* <span data-ttu-id="e2912-799">Supprime les déclarations `using` inutilisées.</span><span class="sxs-lookup"><span data-stu-id="e2912-799">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="e2912-800">Ajoute le contexte de base de données au conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="e2912-800">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="e2912-801">Spécifie que le contexte de base de données utilise une base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="e2912-801">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller-21"></a><span data-ttu-id="e2912-802">Ajouter un contrôleur 2,1</span><span class="sxs-lookup"><span data-stu-id="e2912-802">Add a controller 2.1</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e2912-803">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e2912-803">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e2912-804">Cliquez avec le bouton droit sur le dossier *Contrôleurs*.</span><span class="sxs-lookup"><span data-stu-id="e2912-804">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="e2912-805">Sélectionnez **Ajouter** > **un nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="e2912-805">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="e2912-806">Dans la boîte de dialogue **Ajouter un nouvel élément** , sélectionnez le modèle **Classe de contrôleur d’API**.</span><span class="sxs-lookup"><span data-stu-id="e2912-806">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="e2912-807">Nommez la classe *TodoController* et sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="e2912-807">Name the class *TodoController* , and select **Add**.</span></span>

  ![Boîte de dialogue Ajouter un nouvel élément avec contrôleur dans la zone de recherche et le contrôleur des API web sélectionné](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="e2912-809">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="e2912-809">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="e2912-810">Dans le dossier *Contrôleurs* , créez une classe nommée `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="e2912-810">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="e2912-811">Remplacez le code du modèle par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e2912-811">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="e2912-812">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="e2912-812">The preceding code:</span></span>

* <span data-ttu-id="e2912-813">Définit une classe de contrôleur d’API sans méthodes.</span><span class="sxs-lookup"><span data-stu-id="e2912-813">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="e2912-814">Marque la classe avec l' [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribut.</span><span class="sxs-lookup"><span data-stu-id="e2912-814">Marks the class with the [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute.</span></span> <span data-ttu-id="e2912-815">Cet attribut indique que le contrôleur répond aux requêtes de l’API web.</span><span class="sxs-lookup"><span data-stu-id="e2912-815">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="e2912-816">Pour plus d’informations sur les comportements spécifiques que permet l’attribut, consultez <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="e2912-816">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="e2912-817">Utilise l’injection de dépendances pour injecter le contexte de base de données (`TodoContext`) dans le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="e2912-817">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="e2912-818">Le contexte de base de données est utilisé dans chacune des méthodes la [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="e2912-818">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="e2912-819">Ajoute un élément nommé `Item1` à la base de données si celle-ci est vide.</span><span class="sxs-lookup"><span data-stu-id="e2912-819">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="e2912-820">Ce code se trouvant dans le constructeur, il s’exécute chaque fois qu’une nouvelle requête HTTP existe.</span><span class="sxs-lookup"><span data-stu-id="e2912-820">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="e2912-821">Si vous supprimez tous les éléments, le constructeur recrée `Item1` au prochain appel d’une méthode d’API.</span><span class="sxs-lookup"><span data-stu-id="e2912-821">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="e2912-822">Ainsi, il peut vous sembler à tort que la suppression n’a pas fonctionné.</span><span class="sxs-lookup"><span data-stu-id="e2912-822">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods-21"></a><span data-ttu-id="e2912-823">Ajouter des méthodes de récupération 2,1</span><span class="sxs-lookup"><span data-stu-id="e2912-823">Add Get methods 2.1</span></span>

<span data-ttu-id="e2912-824">Pour fournir une API qui récupère les tâches, ajoutez les méthodes suivantes à la classe `TodoController` :</span><span class="sxs-lookup"><span data-stu-id="e2912-824">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="e2912-825">Ces méthodes implémentent deux points de terminaison GET :</span><span class="sxs-lookup"><span data-stu-id="e2912-825">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="e2912-826">Arrêtez l’application si elle est toujours en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="e2912-826">Stop the app if it's still running.</span></span> <span data-ttu-id="e2912-827">Ensuite, réexécutez-la pour inclure les dernières modifications.</span><span class="sxs-lookup"><span data-stu-id="e2912-827">Then run it again to include the latest changes.</span></span>

<span data-ttu-id="e2912-828">Testez l’application en appelant les deux points de terminaison à partir d’un navigateur.</span><span class="sxs-lookup"><span data-stu-id="e2912-828">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="e2912-829">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="e2912-829">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="e2912-830">La réponse HTTP suivante est générée par l’appel à `GetTodoItems` :</span><span class="sxs-lookup"><span data-stu-id="e2912-830">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths-21"></a><span data-ttu-id="e2912-831">Routage et chemins d’URL 2,1</span><span class="sxs-lookup"><span data-stu-id="e2912-831">Routing and URL paths 2.1</span></span>

<span data-ttu-id="e2912-832">L' [`[HttpGet]`](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) attribut désigne une méthode qui répond à une requête http.</span><span class="sxs-lookup"><span data-stu-id="e2912-832">The [`[HttpGet]`](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="e2912-833">Le chemin d’URL pour chaque méthode est construit comme suit :</span><span class="sxs-lookup"><span data-stu-id="e2912-833">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="e2912-834">Partez de la chaîne de modèle dans l’attribut `Route` du contrôleur :</span><span class="sxs-lookup"><span data-stu-id="e2912-834">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="e2912-835">Remplacez `[controller]` par le nom du contrôleur qui, par convention, est le nom de la classe du contrôleur sans le suffixe « Controller ».</span><span class="sxs-lookup"><span data-stu-id="e2912-835">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="e2912-836">Pour cet exemple, le nom de classe du contrôleur étant **Todo** Controller, le nom du contrôleur est « todo ».</span><span class="sxs-lookup"><span data-stu-id="e2912-836">For this sample, the controller class name is **Todo** Controller, so the controller name is "todo".</span></span> <span data-ttu-id="e2912-837">Le [routage](xref:mvc/controllers/routing) d’ASP.NET Core ne respecte pas la casse.</span><span class="sxs-lookup"><span data-stu-id="e2912-837">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="e2912-838">Si l’attribut `[HttpGet]` a un modèle de route (par exemple, `[HttpGet("products")]`), ajoutez-le au chemin.</span><span class="sxs-lookup"><span data-stu-id="e2912-838">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="e2912-839">Cet exemple n’utilise pas de modèle.</span><span class="sxs-lookup"><span data-stu-id="e2912-839">This sample doesn't use a template.</span></span> <span data-ttu-id="e2912-840">Pour plus d’informations, consultez [Routage par attributs avec des attributs Http[Verbe]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="e2912-840">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="e2912-841">Dans la méthode `GetTodoItem` suivante, `"{id}"` est une variable d’espace réservé pour l’identificateur unique de la tâche.</span><span class="sxs-lookup"><span data-stu-id="e2912-841">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="e2912-842">Quand `GetTodoItem` est appelée, la valeur de `"{id}"` dans l’URL est fournie à la méthode dans son paramètre `id`.</span><span class="sxs-lookup"><span data-stu-id="e2912-842">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values-21"></a><span data-ttu-id="e2912-843">Valeurs de retour 2,1</span><span class="sxs-lookup"><span data-stu-id="e2912-843">Return values 2.1</span></span>

<span data-ttu-id="e2912-844">Le type de retour des `GetTodoItems` `GetTodoItem` méthodes et est [ActionResult \<T> type](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="e2912-844">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="e2912-845">ASP.NET Core sérialise automatiquement l’objet en [JSON](https://www.json.org/) et écrit le JSON dans le corps du message de réponse.</span><span class="sxs-lookup"><span data-stu-id="e2912-845">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="e2912-846">Le code de réponse pour ce type de retour est 200, en supposant qu’il n’existe pas d’exception non gérée.</span><span class="sxs-lookup"><span data-stu-id="e2912-846">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="e2912-847">Les exceptions non gérées sont converties en erreurs 5xx.</span><span class="sxs-lookup"><span data-stu-id="e2912-847">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="e2912-848">Les types de retour `ActionResult` peuvent représenter une large plage de codes d’état HTTP.</span><span class="sxs-lookup"><span data-stu-id="e2912-848">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="e2912-849">Par exemple, `GetTodoItem` peut retourner deux valeurs d’état différentes :</span><span class="sxs-lookup"><span data-stu-id="e2912-849">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="e2912-850">Si aucun élément ne correspond à l’ID demandé, la méthode retourne un <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound%2A> code d’erreur 404.</span><span class="sxs-lookup"><span data-stu-id="e2912-850">If no item matches the requested ID, the method returns a 404 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound%2A> error code.</span></span>
* <span data-ttu-id="e2912-851">Sinon, la méthode retourne 200 avec un corps de réponse JSON.</span><span class="sxs-lookup"><span data-stu-id="e2912-851">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="e2912-852">Le retour de `item` entraîne une réponse HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="e2912-852">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="test-the-gettodoitems-method-21"></a><span data-ttu-id="e2912-853">Tester la méthode GetTodoItems 2,1</span><span class="sxs-lookup"><span data-stu-id="e2912-853">Test the GetTodoItems method 2.1</span></span>

<span data-ttu-id="e2912-854">Ce tutoriel utilise Postman pour tester l’API web.</span><span class="sxs-lookup"><span data-stu-id="e2912-854">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="e2912-855">Installer [Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="e2912-855">Install [Postman](https://www.getpostman.com/downloads/).</span></span>
* <span data-ttu-id="e2912-856">Démarrez l’application web.</span><span class="sxs-lookup"><span data-stu-id="e2912-856">Start the web app.</span></span>
* <span data-ttu-id="e2912-857">Démarrez Postman.</span><span class="sxs-lookup"><span data-stu-id="e2912-857">Start Postman.</span></span>
* <span data-ttu-id="e2912-858">Désactivez la **vérification du certificat SSL**.</span><span class="sxs-lookup"><span data-stu-id="e2912-858">Disable **SSL certificate verification**.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e2912-859">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e2912-859">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e2912-860">À partir de **Fichier** > **Paramètres** (onglet **Général** ), désactivez **Vérification du certificat SSL**.</span><span class="sxs-lookup"><span data-stu-id="e2912-860">From **File** > **Settings** ( **General** tab), disable **SSL certificate verification**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="e2912-861">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="e2912-861">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="e2912-862">Dans **Postman**  >  **Préférences** de publication (onglet **général** ), désactivez la vérification de **certificat SSL**.</span><span class="sxs-lookup"><span data-stu-id="e2912-862">From **Postman** > **Preferences** ( **General** tab), disable **SSL certificate verification**.</span></span> <span data-ttu-id="e2912-863">Vous pouvez également sélectionner la clé et sélectionner **Paramètres** , puis désactiver la vérification du certificat SSL.</span><span class="sxs-lookup"><span data-stu-id="e2912-863">Alternatively, select the wrench and select **Settings** , then disable the SSL certificate verification.</span></span>

---
  
> [!WARNING]
> <span data-ttu-id="e2912-864">Réactivez la vérification du certificat SSL après avoir testé le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="e2912-864">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="e2912-865">Créez une requête.</span><span class="sxs-lookup"><span data-stu-id="e2912-865">Create a new request.</span></span>
  * <span data-ttu-id="e2912-866">Définissez la méthode HTTP sur **GET**.</span><span class="sxs-lookup"><span data-stu-id="e2912-866">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="e2912-867">Définissez l’URI de la demande sur `https://localhost:<port>/api/todo` .</span><span class="sxs-lookup"><span data-stu-id="e2912-867">Set the request URI to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="e2912-868">Par exemple : `https://localhost:5001/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="e2912-868">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="e2912-869">Définissez l’ **affichage à deux volets** dans Postman.</span><span class="sxs-lookup"><span data-stu-id="e2912-869">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="e2912-870">Sélectionnez **Envoyer**.</span><span class="sxs-lookup"><span data-stu-id="e2912-870">Select **Send**.</span></span>

![Postman avec requête Get](first-web-api/_static/2pv.png)

## <a name="add-a-create-method-21"></a><span data-ttu-id="e2912-872">Ajouter une méthode de création 2,1</span><span class="sxs-lookup"><span data-stu-id="e2912-872">Add a Create method 2.1</span></span>

<span data-ttu-id="e2912-873">Ajoutez la méthode `PostTodoItem` suivante à *Controllers/TodoController.cs* :</span><span class="sxs-lookup"><span data-stu-id="e2912-873">Add the following `PostTodoItem` method inside of *Controllers/TodoController.cs* :</span></span> 

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="e2912-874">Le code précédent est une méthode HTTP Après, comme indiqué par l' [`[HttpPost]`](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute) attribut.</span><span class="sxs-lookup"><span data-stu-id="e2912-874">The preceding code is an HTTP POST method, as indicated by the [`[HttpPost]`](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute) attribute.</span></span> <span data-ttu-id="e2912-875">La méthode obtient la valeur de la tâche dans le corps de la requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="e2912-875">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="e2912-876">La méthode `CreatedAtAction` :</span><span class="sxs-lookup"><span data-stu-id="e2912-876">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="e2912-877">Retourne un code d’état HTTP 201 en cas de réussite.</span><span class="sxs-lookup"><span data-stu-id="e2912-877">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="e2912-878">HTTP 201 est la réponse standard d’une méthode HTTP POST qui crée une ressource sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="e2912-878">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="e2912-879">Ajoute un en-tête `Location` à la réponse.</span><span class="sxs-lookup"><span data-stu-id="e2912-879">Adds a `Location` header to the response.</span></span> <span data-ttu-id="e2912-880">L’en-tête `Location` spécifie l’URI de l’élément d’action qui vient d’être créé.</span><span class="sxs-lookup"><span data-stu-id="e2912-880">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="e2912-881">Pour plus d’informations, consultez la section [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="e2912-881">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="e2912-882">Fait référence à l’action `GetTodoItem` pour créer l’URI `Location` de l’en-tête.</span><span class="sxs-lookup"><span data-stu-id="e2912-882">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="e2912-883">Le mot clé `nameof` C# est utilisé pour éviter de coder en dur le nom de l’action dans l’appel `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="e2912-883">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method-21"></a><span data-ttu-id="e2912-884">Tester la méthode PostTodoItem 2,1</span><span class="sxs-lookup"><span data-stu-id="e2912-884">Test the PostTodoItem method 2.1</span></span>

* <span data-ttu-id="e2912-885">Créez le projet.</span><span class="sxs-lookup"><span data-stu-id="e2912-885">Build the project.</span></span>
* <span data-ttu-id="e2912-886">Dans Postman, définissez la méthode HTTP sur `POST`.</span><span class="sxs-lookup"><span data-stu-id="e2912-886">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="e2912-887">Définissez l’URI sur `https://localhost:<port>/api/TodoItem` .</span><span class="sxs-lookup"><span data-stu-id="e2912-887">Set the URI to `https://localhost:<port>/api/TodoItem`.</span></span> <span data-ttu-id="e2912-888">Par exemple : `https://localhost:5001/api/TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="e2912-888">For example, `https://localhost:5001/api/TodoItem`.</span></span>
* <span data-ttu-id="e2912-889">Sélectionnez l’onglet **Corps** .</span><span class="sxs-lookup"><span data-stu-id="e2912-889">Select the **Body** tab.</span></span>
* <span data-ttu-id="e2912-890">Sélectionnez la case d’option **raw** (données brutes).</span><span class="sxs-lookup"><span data-stu-id="e2912-890">Select the **raw** radio button.</span></span>
* <span data-ttu-id="e2912-891">Définissez le type sur **JSON (application/json)**.</span><span class="sxs-lookup"><span data-stu-id="e2912-891">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="e2912-892">Dans le corps de la demande, entrez la syntaxe JSON d’une tâche :</span><span class="sxs-lookup"><span data-stu-id="e2912-892">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="e2912-893">Sélectionnez **Envoyer**.</span><span class="sxs-lookup"><span data-stu-id="e2912-893">Select **Send**.</span></span>

  ![Postman avec requête de création](first-web-api/_static/create.png)

  <span data-ttu-id="e2912-895">Si vous obtenez une erreur 405 Méthode non autorisée, il est probable que le projet n’ait pas été compilé après l’ajout de la méthode `PostTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="e2912-895">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri-21"></a><span data-ttu-id="e2912-896">Tester l’URI d’en-tête d’emplacement 2,1</span><span class="sxs-lookup"><span data-stu-id="e2912-896">Test the location header URI 2.1</span></span>

* <span data-ttu-id="e2912-897">Sélectionnez l’onglet **Headers** (En-têtes) dans le volet **Response** (Réponse).</span><span class="sxs-lookup"><span data-stu-id="e2912-897">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="e2912-898">Copiez la valeur d’en-tête **Location** (Emplacement) :</span><span class="sxs-lookup"><span data-stu-id="e2912-898">Copy the **Location** header value:</span></span>

  ![Onglet Headers de la console Postman](first-web-api/_static/pmc2.png)

* <span data-ttu-id="e2912-900">Définissez la méthode sur GET.</span><span class="sxs-lookup"><span data-stu-id="e2912-900">Set the method to GET.</span></span>
* <span data-ttu-id="e2912-901">Définissez l’URI sur `https://localhost:<port>/api/TodoItems/2` .</span><span class="sxs-lookup"><span data-stu-id="e2912-901">Set the URI to `https://localhost:<port>/api/TodoItems/2`.</span></span> <span data-ttu-id="e2912-902">Par exemple : `https://localhost:5001/api/TodoItems/2`.</span><span class="sxs-lookup"><span data-stu-id="e2912-902">For example, `https://localhost:5001/api/TodoItems/2`.</span></span>
* <span data-ttu-id="e2912-903">Sélectionnez **Envoyer**.</span><span class="sxs-lookup"><span data-stu-id="e2912-903">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method-21"></a><span data-ttu-id="e2912-904">Ajouter une méthode PutTodoItem 2,1</span><span class="sxs-lookup"><span data-stu-id="e2912-904">Add a PutTodoItem method 2.1</span></span>

<span data-ttu-id="e2912-905">Ajoutez la méthode `PutTodoItem` suivante :</span><span class="sxs-lookup"><span data-stu-id="e2912-905">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="e2912-906">`PutTodoItem` est similaire à `PostTodoItem`, à la différence près qu’il utilise HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="e2912-906">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="e2912-907">La réponse est [204 (aucun contenu)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="e2912-907">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="e2912-908">D’après la spécification HTTP, une requête PUT nécessite que le client envoie toute l’entité mise à jour, et pas seulement les changements.</span><span class="sxs-lookup"><span data-stu-id="e2912-908">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="e2912-909">Pour prendre en charge les mises à jour partielles, utilisez [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="e2912-909">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="e2912-910">Si vous obtenez une erreur en appelant `PutTodoItem`, appelez `GET` pour vérifier que la base de données contient un élément.</span><span class="sxs-lookup"><span data-stu-id="e2912-910">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method-21"></a><span data-ttu-id="e2912-911">Tester la méthode PutTodoItem 2,1</span><span class="sxs-lookup"><span data-stu-id="e2912-911">Test the PutTodoItem method 2.1</span></span>

<span data-ttu-id="e2912-912">Cet exemple utilise une base de données en mémoire qui doit être initialisée chaque fois que l’application est démarrée.</span><span class="sxs-lookup"><span data-stu-id="e2912-912">This sample uses an in-memory database that must be initialized each time the app is started.</span></span> <span data-ttu-id="e2912-913">La base de données doit contenir un élément avant que vous ne passiez un appel PUT.</span><span class="sxs-lookup"><span data-stu-id="e2912-913">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="e2912-914">Appelez obtenir pour vous assurer qu’il y a un élément dans la base de données avant d’effectuer un appel PUT.</span><span class="sxs-lookup"><span data-stu-id="e2912-914">Call GET to ensure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="e2912-915">Mettez à jour l’élément de tâche dont l’ID est égal à 1 et affectez-lui le nom « flux de poisson » :</span><span class="sxs-lookup"><span data-stu-id="e2912-915">Update the to-do item that has Id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "id":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="e2912-916">L’image suivante montre la mise à jour Postman :</span><span class="sxs-lookup"><span data-stu-id="e2912-916">The following image shows the Postman update:</span></span>

![Console Postman montrant la réponse 204 (Pas de contenu)](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method-21"></a><span data-ttu-id="e2912-918">Ajouter une méthode DeleteTodoItem 2,1</span><span class="sxs-lookup"><span data-stu-id="e2912-918">Add a DeleteTodoItem method 2.1</span></span>

<span data-ttu-id="e2912-919">Ajoutez la méthode `DeleteTodoItem` suivante :</span><span class="sxs-lookup"><span data-stu-id="e2912-919">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="e2912-920">La réponse `DeleteTodoItem` est [204 (Pas de contenu)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="e2912-920">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method-21"></a><span data-ttu-id="e2912-921">Tester la méthode DeleteTodoItem 2,1</span><span class="sxs-lookup"><span data-stu-id="e2912-921">Test the DeleteTodoItem method 2.1</span></span>

<span data-ttu-id="e2912-922">Utilisez Postman pour supprimer une tâche :</span><span class="sxs-lookup"><span data-stu-id="e2912-922">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="e2912-923">Définissez la méthode sur `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="e2912-923">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="e2912-924">Définissez l’URI de l’objet à supprimer (par exemple, `https://localhost:5001/api/todo/1` ).</span><span class="sxs-lookup"><span data-stu-id="e2912-924">Set the URI of the object to delete (for example, `https://localhost:5001/api/todo/1`).</span></span>
* <span data-ttu-id="e2912-925">Sélectionnez **Envoyer**.</span><span class="sxs-lookup"><span data-stu-id="e2912-925">Select **Send**.</span></span>

<span data-ttu-id="e2912-926">L’exemple d’application vous permet de supprimer tous les éléments.</span><span class="sxs-lookup"><span data-stu-id="e2912-926">The sample app allows you to delete all the items.</span></span> <span data-ttu-id="e2912-927">Toutefois, quand le dernier élément est supprimé, un autre est créé par le constructeur de classe de modèle au prochain appel de l’API.</span><span class="sxs-lookup"><span data-stu-id="e2912-927">However, when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-web-api-with-javascript-21"></a><span data-ttu-id="e2912-928">Appeler l’API Web avec JavaScript 2,1</span><span class="sxs-lookup"><span data-stu-id="e2912-928">Call the web API with JavaScript 2.1</span></span>

<span data-ttu-id="e2912-929">Dans cette section, une page HTML qui utilise JavaScript pour appeler l’API web est ajoutée.</span><span class="sxs-lookup"><span data-stu-id="e2912-929">In this section, an HTML page is added that uses JavaScript to call the web API.</span></span> <span data-ttu-id="e2912-930">jQuery lance la demande.</span><span class="sxs-lookup"><span data-stu-id="e2912-930">jQuery initiates the request.</span></span> <span data-ttu-id="e2912-931">JavaScript met à jour la page avec les détails de la réponse de l’API Web.</span><span class="sxs-lookup"><span data-stu-id="e2912-931">JavaScript updates the page with the details from the web API's response.</span></span>

<span data-ttu-id="e2912-932">Configurez l’application pour [traiter les fichiers statiques](xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles%2A) et [activer le mappage de fichier par défaut](xref:Microsoft.AspNetCore.Builder.DefaultFilesExtensions.UseDefaultFiles%2A) en mettant à jour *Startup.cs* avec le code en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="e2912-932">Configure the app to [serve static files](xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles%2A) and [enable default file mapping](xref:Microsoft.AspNetCore.Builder.DefaultFilesExtensions.UseDefaultFiles%2A) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

<span data-ttu-id="e2912-933">Créez un dossier *wwwroot* dans le répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="e2912-933">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="e2912-934">Ajoutez un fichier HTML nommé *index.html* au répertoire *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="e2912-934">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="e2912-935">Remplacez son contenu par le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="e2912-935">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="e2912-936">Ajoutez un fichier JavaScript nommé *site.js* au répertoire *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="e2912-936">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="e2912-937">Remplacez le contenu par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e2912-937">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="e2912-938">Vous devrez peut-être changer les paramètres de lancement du projet ASP.NET Core pour tester la page HTML localement :</span><span class="sxs-lookup"><span data-stu-id="e2912-938">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="e2912-939">Ouvrez *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="e2912-939">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="e2912-940">Supprimez la `launchUrl` propriété pour forcer l’ouverture de l’application à *index.html* &mdash; fichier par défaut du projet.</span><span class="sxs-lookup"><span data-stu-id="e2912-940">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="e2912-941">Cet exemple appelle toutes les méthodes CRUD de l’API web.</span><span class="sxs-lookup"><span data-stu-id="e2912-941">This sample calls all of the CRUD methods of the web API.</span></span> <span data-ttu-id="e2912-942">Les explications suivantes traitent des appels à l’API.</span><span class="sxs-lookup"><span data-stu-id="e2912-942">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items-21"></a><span data-ttu-id="e2912-943">Obtenir la liste des éléments de tâche 2,1</span><span class="sxs-lookup"><span data-stu-id="e2912-943">Get a list of to-do items 2.1</span></span>

<span data-ttu-id="e2912-944">jQuery envoie une requête HTTP obtenir à l’API Web, qui retourne JSON représentant un tableau d’éléments de tâche.</span><span class="sxs-lookup"><span data-stu-id="e2912-944">jQuery sends an HTTP GET request to the web API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="e2912-945">La fonction de rappel `success` est appelée si la requête réussit.</span><span class="sxs-lookup"><span data-stu-id="e2912-945">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="e2912-946">Dans le rappel, le DOM est mis à jour avec les informations des tâches.</span><span class="sxs-lookup"><span data-stu-id="e2912-946">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item-21"></a><span data-ttu-id="e2912-947">Ajouter un élément de tâche 2,1</span><span class="sxs-lookup"><span data-stu-id="e2912-947">Add a to-do item 2.1</span></span>

<span data-ttu-id="e2912-948">jQuery envoie une requête HTTP POSTÉRIEURe avec l’élément de tâche dans le corps de la demande.</span><span class="sxs-lookup"><span data-stu-id="e2912-948">jQuery sends an HTTP POST request with the to-do item in the request body.</span></span> <span data-ttu-id="e2912-949">Les options `accepts` et `contentType` sont définies avec la valeur `application/json` pour spécifier le type de média qui est reçu et envoyé.</span><span class="sxs-lookup"><span data-stu-id="e2912-949">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="e2912-950">La tâche est convertie au format JSON à l’aide de [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="e2912-950">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="e2912-951">Quand l’API retourne un code d’état de réussite, la fonction `getData` est appelée pour mettre à jour la table HTML.</span><span class="sxs-lookup"><span data-stu-id="e2912-951">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item-21"></a><span data-ttu-id="e2912-952">Mettre à jour un élément de tâche 2,1</span><span class="sxs-lookup"><span data-stu-id="e2912-952">Update a to-do item 2.1</span></span>

<span data-ttu-id="e2912-953">La mise à jour d’une tâche est similaire à l’ajout d’une tâche.</span><span class="sxs-lookup"><span data-stu-id="e2912-953">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="e2912-954">L’identificateur unique de la tâche est ajouté à l’`url` et le `type` est `PUT`.</span><span class="sxs-lookup"><span data-stu-id="e2912-954">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item-21"></a><span data-ttu-id="e2912-955">Supprimer un élément de tâche 2,1</span><span class="sxs-lookup"><span data-stu-id="e2912-955">Delete a to-do item 2.1</span></span>

<span data-ttu-id="e2912-956">Pour supprimer une tâche, vous devez définir le `type` sur l’appel AJAX avec la valeur `DELETE` et spécifier l’identificateur unique de l’élément dans l’URL.</span><span class="sxs-lookup"><span data-stu-id="e2912-956">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

::: moniker-end

<a name="auth"></a>

## <a name="add-authentication-support-to-a-web-api-21"></a><span data-ttu-id="e2912-957">Ajouter la prise en charge de l’authentification à une API Web 2,1</span><span class="sxs-lookup"><span data-stu-id="e2912-957">Add authentication support to a web API 2.1</span></span>

[!INCLUDE[](~/includes/:::no-loc(Identity):::Server4.md)]

## <a name="additional-resources-21"></a><span data-ttu-id="e2912-958">Ressources supplémentaires 2,1</span><span class="sxs-lookup"><span data-stu-id="e2912-958">Additional resources 2.1</span></span>

<span data-ttu-id="e2912-959">[Affichez ou téléchargez l’exemple de code de ce tutoriel](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="e2912-959">[View or download sample code for this tutorial](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="e2912-960">Consultez [Guide pratique pour télécharger](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="e2912-960">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="e2912-961">Pour plus d’informations, consultez les ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="e2912-961">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/intro>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="e2912-962">Version YouTube de ce tutoriel</span><span class="sxs-lookup"><span data-stu-id="e2912-962">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)
