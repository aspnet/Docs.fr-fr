---
title: 'Prise en main de ASP.NET Core SignalR'
author: bradygaster
description: 'Dans ce didacticiel, vous allez créer une application de conversation qui utilise ASP.NET Core SignalR .'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/21/2019
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
uid: tutorials/signalr
ms.openlocfilehash: 59c296f3388e71254badb02fa3ae4279005c359c
ms.sourcegitcommit: ca34c1ac578e7d3daa0febf1810ba5fc74f60bbf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93056879"
---
# <a name="tutorial-get-started-with-aspnet-core-no-locsignalr"></a><span data-ttu-id="13308-103">Didacticiel : prise en main de ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="13308-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="13308-104">Ce didacticiel enseigne les bases de la création d’une application en temps réel à l’aide de SignalR .</span><span class="sxs-lookup"><span data-stu-id="13308-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="13308-105">Vous allez apprendre à effectuer les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="13308-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="13308-106">Créez un projet web.</span><span class="sxs-lookup"><span data-stu-id="13308-106">Create a web project.</span></span>
> * <span data-ttu-id="13308-107">Ajoutez la SignalR bibliothèque cliente.</span><span class="sxs-lookup"><span data-stu-id="13308-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="13308-108">Créez un SignalR concentrateur.</span><span class="sxs-lookup"><span data-stu-id="13308-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="13308-109">Configurez le projet à utiliser SignalR .</span><span class="sxs-lookup"><span data-stu-id="13308-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="13308-110">Ajouter du code qui envoie des messages de n’importe quel client vers tous les clients connectés.</span><span class="sxs-lookup"><span data-stu-id="13308-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="13308-111">À la fin, vous disposerez d’une application de conversation opérationnelle :</span><span class="sxs-lookup"><span data-stu-id="13308-111">At the end, you'll have a working chat app:</span></span>

![::: No-Loc (Signalr) ::: Sample App](signalr/_static/3.x/signalr-get-started-finished.png)

## <a name="prerequisites"></a><span data-ttu-id="13308-113">Prérequis</span><span class="sxs-lookup"><span data-stu-id="13308-113">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="13308-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="13308-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="13308-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="13308-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="13308-116">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="13308-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-web-app-project"></a><span data-ttu-id="13308-117">Créer un projet d’application web</span><span class="sxs-lookup"><span data-stu-id="13308-117">Create a web app project</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="13308-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="13308-118">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="13308-119">Dans le menu, sélectionnez **Fichier > Nouveau projet** .</span><span class="sxs-lookup"><span data-stu-id="13308-119">From the menu, select **File > New Project** .</span></span>

* <span data-ttu-id="13308-120">Dans la boîte de dialogue **Créer un projet** , sélectionnez **Application web ASP.NET Core** , puis **Suivant** .</span><span class="sxs-lookup"><span data-stu-id="13308-120">In the **Create a new project** dialog, select **ASP.NET Core Web Application** , and then select **Next** .</span></span>

* <span data-ttu-id="13308-121">Dans la boîte de dialogue **configurer votre nouveau projet** , nommez la *SignalR conversation* de projet, puis sélectionnez **créer** .</span><span class="sxs-lookup"><span data-stu-id="13308-121">In the **Configure your new project** dialog, name the project *SignalRChat* , and then select **Create** .</span></span>

* <span data-ttu-id="13308-122">Dans la boîte de dialogue **créer une application web ASP.net Core** , sélectionnez **.net Core** et **ASP.net Core 3,1** .</span><span class="sxs-lookup"><span data-stu-id="13308-122">In the **Create a new ASP.NET Core web Application** dialog, select **.NET Core** and **ASP.NET Core 3.1** .</span></span> 

* <span data-ttu-id="13308-123">Sélectionnez **application Web** pour créer un projet qui utilise des Razor pages, puis sélectionnez **créer** .</span><span class="sxs-lookup"><span data-stu-id="13308-123">Select **Web Application** to create a project that uses Razor Pages, and then select **Create** .</span></span>

  ![Boîte de dialogue Nouveau projet dans Visual Studio](signalr/_static/3.x/signalr-new-project-dialog.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="13308-125">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="13308-125">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="13308-126">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal) dans le dossier dans lequel le nouveau dossier de projet va être créé.</span><span class="sxs-lookup"><span data-stu-id="13308-126">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="13308-127">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="13308-127">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="13308-128">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="13308-128">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="13308-129">Dans le menu, sélectionnez **Fichier > Nouvelle solution** .</span><span class="sxs-lookup"><span data-stu-id="13308-129">From the menu, select **File > New Solution** .</span></span>

* <span data-ttu-id="13308-130">Sélectionnez **.NET Core > Application > Application web** (ne sélectionnez pas **Application web (modèle-vue-contrôleur)** ), puis **Suivant** .</span><span class="sxs-lookup"><span data-stu-id="13308-130">Select **.NET Core > App > Web Application** (Don't select **Web Application (Model-View-Controller)** ), and then select **Next** .</span></span>

* <span data-ttu-id="13308-131">Assurez-vous que la version cible du .NET **Framework** est définie sur **.net Core 3,1** , puis sélectionnez **suivant** .</span><span class="sxs-lookup"><span data-stu-id="13308-131">Make sure the **Target Framework** is set to **.NET Core 3.1** , and then select **Next** .</span></span>

* <span data-ttu-id="13308-132">Nommez la *SignalR conversation* de projet, puis sélectionnez **créer** .</span><span class="sxs-lookup"><span data-stu-id="13308-132">Name the project *SignalRChat* , and then select **Create** .</span></span>

---

## <a name="add-the-no-locsignalr-client-library"></a><span data-ttu-id="13308-133">Ajouter la SignalR bibliothèque cliente</span><span class="sxs-lookup"><span data-stu-id="13308-133">Add the SignalR client library</span></span>

<span data-ttu-id="13308-134">La SignalR bibliothèque serveur est incluse dans l’infrastructure partagée ASP.NET Core 3,1.</span><span class="sxs-lookup"><span data-stu-id="13308-134">The SignalR server library is included in the ASP.NET Core 3.1 shared framework.</span></span> <span data-ttu-id="13308-135">La bibliothèque cliente JavaScript n’est pas incluse automatiquement dans le projet.</span><span class="sxs-lookup"><span data-stu-id="13308-135">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="13308-136">Pour ce tutoriel, vous utilisez le Gestionnaire de bibliothèque (LibMan) pour obtenir la bibliothèque cliente à partir de *unpkg* .</span><span class="sxs-lookup"><span data-stu-id="13308-136">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg* .</span></span> <span data-ttu-id="13308-137">unpkg est un réseau de distribution de contenu (CDN) qui peut fournir tout ce qui se trouve dans NPM, le gestionnaire de package Node.js.</span><span class="sxs-lookup"><span data-stu-id="13308-137">unpkg is a content delivery network (CDN) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="13308-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="13308-138">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="13308-139">Dans **l’Explorateur de solutions** , cliquez avec le bouton droit sur le projet, puis sélectionnez **Ajouter** > **Bibliothèque côté client** .</span><span class="sxs-lookup"><span data-stu-id="13308-139">In **Solution Explorer** , right-click the project, and select **Add** > **Client-Side Library** .</span></span>

* <span data-ttu-id="13308-140">Dans la boîte de dialogue **Ajouter une bibliothèque côté Client** , pour **Fournisseur** sélectionnez **unpkg** .</span><span class="sxs-lookup"><span data-stu-id="13308-140">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg** .</span></span>

* <span data-ttu-id="13308-141">Pour **Bibliothèque** , entrez `@microsoft/signalr@latest`.</span><span class="sxs-lookup"><span data-stu-id="13308-141">For **Library** , enter `@microsoft/signalr@latest`.</span></span>

* <span data-ttu-id="13308-142">Sélectionnez **Choisir des fichiers spécifiques** , développez le dossier *dist/browser* , puis sélectionnez *signalr.js* et *signalr.min.js* .</span><span class="sxs-lookup"><span data-stu-id="13308-142">Select **Choose specific files** , expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js* .</span></span>

* <span data-ttu-id="13308-143">Définissez **emplacement cible** sur *wwwroot/js/signalr/* , puis sélectionnez **installer** .</span><span class="sxs-lookup"><span data-stu-id="13308-143">Set **Target Location** to *wwwroot/js/signalr/* , and select **Install** .</span></span>

  ![Boîte de dialogue Ajouter une bibliothèque côté client - sélectionner la bibliothèque](signalr/_static/3.x/find-signalr-client-libs-select-files.png)

  <span data-ttu-id="13308-145">LibMan crée un dossier *wwwroot/js/signalr* et y copie les fichiers sélectionnés.</span><span class="sxs-lookup"><span data-stu-id="13308-145">LibMan creates a *wwwroot/js/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="13308-146">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="13308-146">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="13308-147">Dans le terminal intégré, exécutez la commande suivante pour installer LibMan.</span><span class="sxs-lookup"><span data-stu-id="13308-147">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```dotnetcli
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="13308-148">Exécutez la commande suivante pour récupérer la SignalR bibliothèque cliente à l’aide de LibMan.</span><span class="sxs-lookup"><span data-stu-id="13308-148">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="13308-149">Vous devrez peut-être attendre quelques secondes avant que la sortie ne s’affiche.</span><span class="sxs-lookup"><span data-stu-id="13308-149">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @microsoft/signalr@latest -p unpkg -d wwwroot/js/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="13308-150">Les paramètres spécifient les options suivantes :</span><span class="sxs-lookup"><span data-stu-id="13308-150">The parameters specify the following options:</span></span>
  * <span data-ttu-id="13308-151">Utilisez le fournisseur unpkg.</span><span class="sxs-lookup"><span data-stu-id="13308-151">Use the unpkg provider.</span></span>
  * <span data-ttu-id="13308-152">Copiez les fichiers vers la destination *wwwroot/js/signalr* .</span><span class="sxs-lookup"><span data-stu-id="13308-152">Copy files to the *wwwroot/js/signalr* destination.</span></span>
  * <span data-ttu-id="13308-153">Copiez uniquement les fichiers spécifiés.</span><span class="sxs-lookup"><span data-stu-id="13308-153">Copy only the specified files.</span></span>

  <span data-ttu-id="13308-154">La sortie ressemble à l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="13308-154">The output looks like the following example:</span></span>

  ```console
  wwwroot/js/signalr/dist/browser/signalr.js written to disk
  wwwroot/js/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@microsoft/signalr@latest" to "wwwroot/js/signalr"
  ```

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="13308-155">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="13308-155">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="13308-156">Dans le **Terminal** , exécutez la commande suivante pour installer LibMan.</span><span class="sxs-lookup"><span data-stu-id="13308-156">In the **Terminal** , run the following command to install LibMan.</span></span>

  ```dotnetcli
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="13308-157">Accédez au dossier du projet (celui qui contient le fichier *SignalR chat. csproj* ).</span><span class="sxs-lookup"><span data-stu-id="13308-157">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="13308-158">Exécutez la commande suivante pour récupérer la SignalR bibliothèque cliente à l’aide de LibMan.</span><span class="sxs-lookup"><span data-stu-id="13308-158">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @microsoft/signalr@latest -p unpkg -d wwwroot/js/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="13308-159">Les paramètres spécifient les options suivantes :</span><span class="sxs-lookup"><span data-stu-id="13308-159">The parameters specify the following options:</span></span>
  * <span data-ttu-id="13308-160">Utilisez le fournisseur unpkg.</span><span class="sxs-lookup"><span data-stu-id="13308-160">Use the unpkg provider.</span></span>
  * <span data-ttu-id="13308-161">Copiez les fichiers vers la destination *wwwroot/js/signalr* .</span><span class="sxs-lookup"><span data-stu-id="13308-161">Copy files to the *wwwroot/js/signalr* destination.</span></span>
  * <span data-ttu-id="13308-162">Copiez uniquement les fichiers spécifiés.</span><span class="sxs-lookup"><span data-stu-id="13308-162">Copy only the specified files.</span></span>

  <span data-ttu-id="13308-163">La sortie ressemble à l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="13308-163">The output looks like the following example:</span></span>

  ```console
  wwwroot/js/signalr/dist/browser/signalr.js written to disk
  wwwroot/js/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@microsoft/signalr@latest" to "wwwroot/js/signalr"
  ```

---

## <a name="create-a-no-locsignalr-hub"></a><span data-ttu-id="13308-164">Créer un SignalR Hub</span><span class="sxs-lookup"><span data-stu-id="13308-164">Create a SignalR hub</span></span>

<span data-ttu-id="13308-165">Un *hub* est une classe servant de pipeline global qui gère les communications client-serveur.</span><span class="sxs-lookup"><span data-stu-id="13308-165">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="13308-166">Dans le SignalR dossier de projet de conversation, créez un dossier *hubs* .</span><span class="sxs-lookup"><span data-stu-id="13308-166">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="13308-167">Dans le dossier *Hubs* , créez un fichier *ChatHub.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="13308-167">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[ChatHub](signalr/sample-snapshot/3.x/ChatHub.cs)]

  <span data-ttu-id="13308-168">La classe `ChatHub` hérite de la classe SignalR `Hub`.</span><span class="sxs-lookup"><span data-stu-id="13308-168">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="13308-169">La classe `Hub` gère les connexions, les groupes et la messagerie.</span><span class="sxs-lookup"><span data-stu-id="13308-169">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="13308-170">La méthode `SendMessage` peut être appelée par un client connecté afin d’envoyer un message à tous les clients.</span><span class="sxs-lookup"><span data-stu-id="13308-170">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="13308-171">Le code client JavaScript qui appelle la méthode est indiqué plus loin dans le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="13308-171">JavaScript client code that calls the method is shown later in the tutorial.</span></span> <span data-ttu-id="13308-172">SignalR le code est asynchrone pour fournir une évolutivité maximale.</span><span class="sxs-lookup"><span data-stu-id="13308-172">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-no-locsignalr"></a><span data-ttu-id="13308-173">Configurer SignalR</span><span class="sxs-lookup"><span data-stu-id="13308-173">Configure SignalR</span></span>

<span data-ttu-id="13308-174">Le SignalR serveur doit être configuré pour transmettre les SignalR demandes à SignalR .</span><span class="sxs-lookup"><span data-stu-id="13308-174">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="13308-175">Ajoutez le code en surbrillance suivant au fichier *Startup.cs* .</span><span class="sxs-lookup"><span data-stu-id="13308-175">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/3.x/Startup.cs?highlight=11,28,55)]

  <span data-ttu-id="13308-176">Ces modifications sont ajoutées SignalR aux systèmes de routage et d’injection de dépendances ASP.net core.</span><span class="sxs-lookup"><span data-stu-id="13308-176">These changes add SignalR to the ASP.NET Core dependency injection and routing systems.</span></span>

## <a name="add-no-locsignalr-client-code"></a><span data-ttu-id="13308-177">Ajouter du SignalR code client</span><span class="sxs-lookup"><span data-stu-id="13308-177">Add SignalR client code</span></span>

* <span data-ttu-id="13308-178">Remplacez le contenu de *Pages\Index.cshtml* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="13308-178">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample-snapshot/3.x/Index.cshtml)]

  <span data-ttu-id="13308-179">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="13308-179">The preceding code:</span></span>

  * <span data-ttu-id="13308-180">Crée des zones de texte pour le nom et le texte du message, ainsi qu’un bouton Envoyer.</span><span class="sxs-lookup"><span data-stu-id="13308-180">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="13308-181">Crée une liste avec `id="messagesList"` pour afficher les messages reçus du SignalR Hub.</span><span class="sxs-lookup"><span data-stu-id="13308-181">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="13308-182">Comprend des références de script à SignalR et le code d’application *chat.js* que vous créez à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="13308-182">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="13308-183">Dans le dossier *wwwroot/js* , créez un fichier *chat.js* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="13308-183">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[chat](signalr/sample-snapshot/3.x/chat.js)]

  <span data-ttu-id="13308-184">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="13308-184">The preceding code:</span></span>

  * <span data-ttu-id="13308-185">Crée et lance une connexion.</span><span class="sxs-lookup"><span data-stu-id="13308-185">Creates and starts a connection.</span></span>
  * <span data-ttu-id="13308-186">Ajoute au bouton Envoyer un gestionnaire qui envoie des messages au hub.</span><span class="sxs-lookup"><span data-stu-id="13308-186">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="13308-187">Ajoute à l’objet de connexion un gestionnaire qui reçoit des messages à partir du hub et les ajoute à la liste.</span><span class="sxs-lookup"><span data-stu-id="13308-187">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="13308-188">Exécuter l’application</span><span class="sxs-lookup"><span data-stu-id="13308-188">Run the app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="13308-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="13308-189">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="13308-190">Appuyez sur **Ctrl+F5** pour exécuter l’application sans débogage.</span><span class="sxs-lookup"><span data-stu-id="13308-190">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="13308-191">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="13308-191">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="13308-192">Dans le terminal intégré, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="13308-192">In the integrated terminal, run the following command:</span></span>

  ```dotnetcli
  dotnet watch run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="13308-193">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="13308-193">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="13308-194">Dans le menu, sélectionnez **Exécuter > Démarrer sans débogage** .</span><span class="sxs-lookup"><span data-stu-id="13308-194">From the menu, select **Run > Start Without Debugging** .</span></span>

---

* <span data-ttu-id="13308-195">Copiez l’URL à partir de la barre d’adresse, ouvrez un autre onglet ou instance du navigateur, puis collez l’URL dans la barre d’adresse.</span><span class="sxs-lookup"><span data-stu-id="13308-195">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="13308-196">Choisissez un des navigateurs, entrez un nom et un message, puis sélectionnez le bouton **Envoyer le message** .</span><span class="sxs-lookup"><span data-stu-id="13308-196">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="13308-197">Le nom et le message sont affichés instantanément dans les deux pages.</span><span class="sxs-lookup"><span data-stu-id="13308-197">The name and message are displayed on both pages instantly.</span></span>

  ![::: No-Loc (Signalr) ::: Sample App](signalr/_static/3.x/signalr-get-started-finished.png)

> [!TIP]
> * <span data-ttu-id="13308-199">Si l’application ne fonctionne pas, ouvrez vos outils de développement (F12) de navigateur et accédez à la console.</span><span class="sxs-lookup"><span data-stu-id="13308-199">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="13308-200">Vous pouvez observer des erreurs liées à votre code HTML et JavaScript.</span><span class="sxs-lookup"><span data-stu-id="13308-200">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="13308-201">Par exemple, supposez que vous placez *signalr.js* dans un dossier autre que celui stipulé.</span><span class="sxs-lookup"><span data-stu-id="13308-201">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="13308-202">Dans ce cas, la référence à ce fichier ne fonctionnera pas et vous verrez une erreur 404 dans la console.</span><span class="sxs-lookup"><span data-stu-id="13308-202">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
>   <span data-ttu-id="13308-203">![Erreur de fichier SignalR.js introuvable](signalr/_static/3.x/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="13308-203">![signalr.js not found error](signalr/_static/3.x/f12-console.png)</span></span>
> * <span data-ttu-id="13308-204">Si vous recevez l’erreur ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY dans Chrome, exécutez les commandes suivantes pour mettre à jour votre certificat de développement :</span><span class="sxs-lookup"><span data-stu-id="13308-204">If you get the error ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY in Chrome, run these commands to update your development certificate:</span></span>
>
>   ```dotnetcli
>   dotnet dev-certs https --clean
>   dotnet dev-certs https --trust
>   ```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="13308-205">Ce didacticiel enseigne les bases de la création d’une application en temps réel à l’aide de SignalR .</span><span class="sxs-lookup"><span data-stu-id="13308-205">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="13308-206">Vous allez apprendre à effectuer les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="13308-206">You learn how to:</span></span> 

> [!div class="checklist"]  
> * <span data-ttu-id="13308-207">Créez un projet web.</span><span class="sxs-lookup"><span data-stu-id="13308-207">Create a web project.</span></span>   
> * <span data-ttu-id="13308-208">Ajoutez la SignalR bibliothèque cliente.</span><span class="sxs-lookup"><span data-stu-id="13308-208">Add the SignalR client library.</span></span>   
> * <span data-ttu-id="13308-209">Créez un SignalR concentrateur.</span><span class="sxs-lookup"><span data-stu-id="13308-209">Create a SignalR hub.</span></span> 
> * <span data-ttu-id="13308-210">Configurez le projet à utiliser SignalR .</span><span class="sxs-lookup"><span data-stu-id="13308-210">Configure the project to use SignalR.</span></span> 
> * <span data-ttu-id="13308-211">Ajouter du code qui envoie des messages de n’importe quel client vers tous les clients connectés.</span><span class="sxs-lookup"><span data-stu-id="13308-211">Add code that sends messages from any client to all connected clients.</span></span>  
<span data-ttu-id="13308-212">À la fin, vous disposerez d’une application de conversation active :::: ![ No-Loc (signalr) ::: Sample App](signalr/_static/2.x/signalr-get-started-finished.png)</span><span class="sxs-lookup"><span data-stu-id="13308-212">At the end, you'll have a working chat app: ![SignalR sample app](signalr/_static/2.x/signalr-get-started-finished.png)</span></span>   

## <a name="prerequisites"></a><span data-ttu-id="13308-213">Prérequis</span><span class="sxs-lookup"><span data-stu-id="13308-213">Prerequisites</span></span>    

# <a name="visual-studio"></a>[<span data-ttu-id="13308-214">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="13308-214">Visual Studio</span></span>](#tab/visual-studio)   

[!INCLUDE[](~/includes/net-core-prereqs-vs2017-2.2.md)] 

# <a name="visual-studio-code"></a>[<span data-ttu-id="13308-215">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="13308-215">Visual Studio Code</span></span>](#tab/visual-studio-code) 

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]    

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="13308-216">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="13308-216">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)   

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]    

--- 

## <a name="create-a-web-project"></a><span data-ttu-id="13308-217">Créer un projet web</span><span class="sxs-lookup"><span data-stu-id="13308-217">Create a web project</span></span> 

# <a name="visual-studio"></a>[<span data-ttu-id="13308-218">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="13308-218">Visual Studio</span></span>](#tab/visual-studio/)  

* <span data-ttu-id="13308-219">Dans le menu, sélectionnez **Fichier > Nouveau projet** .</span><span class="sxs-lookup"><span data-stu-id="13308-219">From the menu, select **File > New Project** .</span></span> 

* <span data-ttu-id="13308-220">Dans la boîte de dialogue **Nouveau projet** , sélectionnez **Installé > Visual C# > Web > Application web ASP.NET Core** .</span><span class="sxs-lookup"><span data-stu-id="13308-220">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application** .</span></span> <span data-ttu-id="13308-221">Nommez la *SignalR conversation* de projet.</span><span class="sxs-lookup"><span data-stu-id="13308-221">Name the project *SignalRChat* .</span></span>   

  ![Boîte de dialogue Nouveau projet dans Visual Studio](signalr/_static/2.x/signalr-new-project-dialog.png)    

* <span data-ttu-id="13308-223">Sélectionnez **application Web** pour créer un projet qui utilise des Razor pages.</span><span class="sxs-lookup"><span data-stu-id="13308-223">Select **Web Application** to create a project that uses Razor Pages.</span></span>   

* <span data-ttu-id="13308-224">Sélectionnez **.NET Core** comme framework cible, sélectionnez **ASP.NET Core 2.2** , puis cliquez sur **OK** .</span><span class="sxs-lookup"><span data-stu-id="13308-224">Select a target framework of **.NET Core** , select **ASP.NET Core 2.2** , and click **OK** .</span></span>    

  ![Boîte de dialogue Nouveau projet dans Visual Studio](signalr/_static/2.x/signalr-new-project-choose-type.png)   

# <a name="visual-studio-code"></a>[<span data-ttu-id="13308-226">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="13308-226">Visual Studio Code</span></span>](#tab/visual-studio-code/)    

* <span data-ttu-id="13308-227">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal) dans le dossier dans lequel le nouveau dossier de projet va être créé.</span><span class="sxs-lookup"><span data-stu-id="13308-227">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>  

* <span data-ttu-id="13308-228">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="13308-228">Run the following commands:</span></span>   

   ```dotnetcli 
   dotnet new webapp -o SignalRChat   
   code -r SignalRChat    
   ```  

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="13308-229">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="13308-229">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)   

* <span data-ttu-id="13308-230">Dans le menu, sélectionnez **Fichier > Nouvelle solution** .</span><span class="sxs-lookup"><span data-stu-id="13308-230">From the menu, select **File > New Solution** .</span></span>    

* <span data-ttu-id="13308-231">Sélectionnez **.NET Core > Application > Application web ASP.NET Core** (ne sélectionnez pas **Application web ASP.NET Core (MVC)** ).</span><span class="sxs-lookup"><span data-stu-id="13308-231">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)** ).</span></span>  

* <span data-ttu-id="13308-232">Sélectionnez **Suivant** .</span><span class="sxs-lookup"><span data-stu-id="13308-232">Select **Next** .</span></span>  

* <span data-ttu-id="13308-233">Nommez la *SignalR conversation* de projet, puis sélectionnez **créer** .</span><span class="sxs-lookup"><span data-stu-id="13308-233">Name the project *SignalRChat* , and then select **Create** .</span></span> 

--- 

## <a name="add-the-no-locsignalr-client-library"></a><span data-ttu-id="13308-234">Ajouter la SignalR bibliothèque cliente</span><span class="sxs-lookup"><span data-stu-id="13308-234">Add the SignalR client library</span></span> 

<span data-ttu-id="13308-235">La SignalR bibliothèque serveur est incluse dans le `Microsoft.AspNetCore.App` Package.</span><span class="sxs-lookup"><span data-stu-id="13308-235">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="13308-236">La bibliothèque cliente JavaScript n’est pas incluse automatiquement dans le projet.</span><span class="sxs-lookup"><span data-stu-id="13308-236">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="13308-237">Pour ce tutoriel, vous utilisez le Gestionnaire de bibliothèque (LibMan) pour obtenir la bibliothèque cliente à partir de *unpkg* .</span><span class="sxs-lookup"><span data-stu-id="13308-237">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg* .</span></span> <span data-ttu-id="13308-238">unpkg est un réseau de distribution de contenu (CDN) qui peut fournir tout ce qui se trouve dans NPM, le gestionnaire de package Node.js.</span><span class="sxs-lookup"><span data-stu-id="13308-238">unpkg is a content delivery network (CDN) that can deliver anything found in npm, the Node.js package manager.</span></span>   

# <a name="visual-studio"></a>[<span data-ttu-id="13308-239">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="13308-239">Visual Studio</span></span>](#tab/visual-studio/)  

* <span data-ttu-id="13308-240">Dans **l’Explorateur de solutions** , cliquez avec le bouton droit sur le projet, puis sélectionnez **Ajouter** > **Bibliothèque côté client** .</span><span class="sxs-lookup"><span data-stu-id="13308-240">In **Solution Explorer** , right-click the project, and select **Add** > **Client-Side Library** .</span></span>  

* <span data-ttu-id="13308-241">Dans la boîte de dialogue **Ajouter une bibliothèque côté Client** , pour **Fournisseur** sélectionnez **unpkg** .</span><span class="sxs-lookup"><span data-stu-id="13308-241">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg** .</span></span> 

* <span data-ttu-id="13308-242">Pour **Bibliothèque** , entrez `@microsoft/signalr@3`, puis sélectionnez la version la plus récente qui n’est pas en préversion.</span><span class="sxs-lookup"><span data-stu-id="13308-242">For **Library** , enter `@microsoft/signalr@3`, and select the latest version that isn't preview.</span></span>  

  ![Boîte de dialogue Ajouter une bibliothèque côté client - sélectionner la bibliothèque](signalr/_static/2.x/libman1.png)   

* <span data-ttu-id="13308-244">Sélectionnez **Choisir des fichiers spécifiques** , développez le dossier *dist/browser* , puis sélectionnez *signalr.js* et *signalr.min.js* .</span><span class="sxs-lookup"><span data-stu-id="13308-244">Select **Choose specific files** , expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js* .</span></span> 

* <span data-ttu-id="13308-245">Définissez **Emplacement cible** sur *wwwroot/lib/signalr/* , puis sélectionnez **Installer** .</span><span class="sxs-lookup"><span data-stu-id="13308-245">Set **Target Location** to *wwwroot/lib/signalr/* , and select **Install** .</span></span>    

  ![Boîte de dialogue Ajouter une bibliothèque côté client - sélectionner les fichiers et la destination](signalr/_static/2.x/libman2.png) 

  <span data-ttu-id="13308-247">LibMan crée un dossier *wwwroot/lib/signalr* et y copie les fichiers sélectionnés.</span><span class="sxs-lookup"><span data-stu-id="13308-247">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>    

# <a name="visual-studio-code"></a>[<span data-ttu-id="13308-248">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="13308-248">Visual Studio Code</span></span>](#tab/visual-studio-code/)    

* <span data-ttu-id="13308-249">Dans le terminal intégré, exécutez la commande suivante pour installer LibMan.</span><span class="sxs-lookup"><span data-stu-id="13308-249">In the integrated terminal, run the following command to install LibMan.</span></span>  

  ```dotnetcli  
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli   
  ```   

* <span data-ttu-id="13308-250">Exécutez la commande suivante pour récupérer la SignalR bibliothèque cliente à l’aide de LibMan.</span><span class="sxs-lookup"><span data-stu-id="13308-250">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="13308-251">Vous devrez peut-être attendre quelques secondes avant que la sortie ne s’affiche.</span><span class="sxs-lookup"><span data-stu-id="13308-251">You might have to wait a few seconds before seeing output.</span></span> 

  ```console    
  libman install @microsoft/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js 
  ```   

  <span data-ttu-id="13308-252">Les paramètres spécifient les options suivantes :</span><span class="sxs-lookup"><span data-stu-id="13308-252">The parameters specify the following options:</span></span> 
  * <span data-ttu-id="13308-253">Utilisez le fournisseur unpkg.</span><span class="sxs-lookup"><span data-stu-id="13308-253">Use the unpkg provider.</span></span> 
  * <span data-ttu-id="13308-254">Copiez les fichiers vers la destination *wwwroot/lib/signalr* .</span><span class="sxs-lookup"><span data-stu-id="13308-254">Copy files to the *wwwroot/lib/signalr* destination.</span></span>    
  * <span data-ttu-id="13308-255">Copiez uniquement les fichiers spécifiés.</span><span class="sxs-lookup"><span data-stu-id="13308-255">Copy only the specified files.</span></span>  

  <span data-ttu-id="13308-256">La sortie ressemble à l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="13308-256">The output looks like the following example:</span></span>  

  ```console    
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk   
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk   
  Installed library "@microsoft/signalr@3.0.1" to "wwwroot/lib/signalr" 
  ```   

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="13308-257">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="13308-257">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)   

* <span data-ttu-id="13308-258">Dans le **Terminal** , exécutez la commande suivante pour installer LibMan.</span><span class="sxs-lookup"><span data-stu-id="13308-258">In the **Terminal** , run the following command to install LibMan.</span></span> 

  ```dotnetcli  
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli   
  ```   

* <span data-ttu-id="13308-259">Accédez au dossier du projet (celui qui contient le fichier *SignalR chat. csproj* ).</span><span class="sxs-lookup"><span data-stu-id="13308-259">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>   

* <span data-ttu-id="13308-260">Exécutez la commande suivante pour récupérer la SignalR bibliothèque cliente à l’aide de LibMan.</span><span class="sxs-lookup"><span data-stu-id="13308-260">Run the following command to get the SignalR client library by using LibMan.</span></span>    

  ```console    
  libman install @microsoft/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js 
  ```   

  <span data-ttu-id="13308-261">Les paramètres spécifient les options suivantes :</span><span class="sxs-lookup"><span data-stu-id="13308-261">The parameters specify the following options:</span></span> 
  * <span data-ttu-id="13308-262">Utilisez le fournisseur unpkg.</span><span class="sxs-lookup"><span data-stu-id="13308-262">Use the unpkg provider.</span></span> 
  * <span data-ttu-id="13308-263">Copiez les fichiers vers la destination *wwwroot/lib/signalr* .</span><span class="sxs-lookup"><span data-stu-id="13308-263">Copy files to the *wwwroot/lib/signalr* destination.</span></span>    
  * <span data-ttu-id="13308-264">Copiez uniquement les fichiers spécifiés.</span><span class="sxs-lookup"><span data-stu-id="13308-264">Copy only the specified files.</span></span>  

  <span data-ttu-id="13308-265">La sortie ressemble à l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="13308-265">The output looks like the following example:</span></span>  

  ```console    
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk   
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk   
  Installed library "@microsoft/signalr@3.x.x" to "wwwroot/lib/signalr" 
  ```   

--- 

## <a name="create-a-no-locsignalr-hub"></a><span data-ttu-id="13308-266">Créer un SignalR Hub</span><span class="sxs-lookup"><span data-stu-id="13308-266">Create a SignalR hub</span></span>   

<span data-ttu-id="13308-267">Un *hub* est une classe servant de pipeline global qui gère les communications client-serveur.</span><span class="sxs-lookup"><span data-stu-id="13308-267">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>   

* <span data-ttu-id="13308-268">Dans le SignalR dossier de projet de conversation, créez un dossier *hubs* .</span><span class="sxs-lookup"><span data-stu-id="13308-268">In the SignalRChat project folder, create a *Hubs* folder.</span></span>  

* <span data-ttu-id="13308-269">Dans le dossier *Hubs* , créez un fichier *ChatHub.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="13308-269">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span> 

  [!code-csharp[Startup](signalr/sample-snapshot/2.x/ChatHub.cs)]   

  <span data-ttu-id="13308-270">La classe `ChatHub` hérite de la classe SignalR `Hub`.</span><span class="sxs-lookup"><span data-stu-id="13308-270">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="13308-271">La classe `Hub` gère les connexions, les groupes et la messagerie.</span><span class="sxs-lookup"><span data-stu-id="13308-271">The `Hub` class manages connections, groups, and messaging.</span></span>  

  <span data-ttu-id="13308-272">La méthode `SendMessage` peut être appelée par un client connecté afin d’envoyer un message à tous les clients.</span><span class="sxs-lookup"><span data-stu-id="13308-272">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="13308-273">Le code client JavaScript qui appelle la méthode est indiqué plus loin dans le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="13308-273">JavaScript client code that calls the method is shown later in the tutorial.</span></span> <span data-ttu-id="13308-274">SignalR le code est asynchrone pour fournir une évolutivité maximale.</span><span class="sxs-lookup"><span data-stu-id="13308-274">SignalR code is asynchronous to provide maximum scalability.</span></span>    

## <a name="configure-no-locsignalr"></a><span data-ttu-id="13308-275">Configurer SignalR</span><span class="sxs-lookup"><span data-stu-id="13308-275">Configure SignalR</span></span>  

<span data-ttu-id="13308-276">Le SignalR serveur doit être configuré pour transmettre les SignalR demandes à SignalR .</span><span class="sxs-lookup"><span data-stu-id="13308-276">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>    

* <span data-ttu-id="13308-277">Ajoutez le code en surbrillance suivant au fichier *Startup.cs* .</span><span class="sxs-lookup"><span data-stu-id="13308-277">Add the following highlighted code to the *Startup.cs* file.</span></span>  

  [!code-csharp[Startup](signalr/sample-snapshot/2.x/Startup.cs?highlight=7,33,52-55)]  

  <span data-ttu-id="13308-278">Ces modifications sont ajoutées SignalR au système d’injection de dépendances ASP.net Core et au pipeline de l’intergiciel (middleware).</span><span class="sxs-lookup"><span data-stu-id="13308-278">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>  

## <a name="add-no-locsignalr-client-code"></a><span data-ttu-id="13308-279">Ajouter du SignalR code client</span><span class="sxs-lookup"><span data-stu-id="13308-279">Add SignalR client code</span></span>    

* <span data-ttu-id="13308-280">Remplacez le contenu de *Pages\Index.cshtml* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="13308-280">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>  

  [!code-cshtml[Index](signalr/sample-snapshot/2.x/Index.cshtml)]   

  <span data-ttu-id="13308-281">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="13308-281">The preceding code:</span></span>   

  * <span data-ttu-id="13308-282">Crée des zones de texte pour le nom et le texte du message, ainsi qu’un bouton Envoyer.</span><span class="sxs-lookup"><span data-stu-id="13308-282">Creates text boxes for name and message text, and a submit button.</span></span>  
  * <span data-ttu-id="13308-283">Crée une liste avec `id="messagesList"` pour afficher les messages reçus du SignalR Hub.</span><span class="sxs-lookup"><span data-stu-id="13308-283">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>   
  * <span data-ttu-id="13308-284">Comprend des références de script à SignalR et le code d’application *chat.js* que vous créez à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="13308-284">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>    

* <span data-ttu-id="13308-285">Dans le dossier *wwwroot/js* , créez un fichier *chat.js* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="13308-285">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>  

  [!code-javascript[Index](signalr/sample-snapshot/2.x/chat.js)]    

  <span data-ttu-id="13308-286">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="13308-286">The preceding code:</span></span>   

  * <span data-ttu-id="13308-287">Crée et lance une connexion.</span><span class="sxs-lookup"><span data-stu-id="13308-287">Creates and starts a connection.</span></span>    
  * <span data-ttu-id="13308-288">Ajoute au bouton Envoyer un gestionnaire qui envoie des messages au hub.</span><span class="sxs-lookup"><span data-stu-id="13308-288">Adds to the submit button a handler that sends messages to the hub.</span></span> 
  * <span data-ttu-id="13308-289">Ajoute à l’objet de connexion un gestionnaire qui reçoit des messages à partir du hub et les ajoute à la liste.</span><span class="sxs-lookup"><span data-stu-id="13308-289">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>  

## <a name="run-the-app"></a><span data-ttu-id="13308-290">Exécuter l’application</span><span class="sxs-lookup"><span data-stu-id="13308-290">Run the app</span></span>  

# <a name="visual-studio"></a>[<span data-ttu-id="13308-291">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="13308-291">Visual Studio</span></span>](#tab/visual-studio)   

* <span data-ttu-id="13308-292">Appuyez sur **Ctrl+F5** pour exécuter l’application sans débogage.</span><span class="sxs-lookup"><span data-stu-id="13308-292">Press **CTRL+F5** to run the app without debugging.</span></span>   

# <a name="visual-studio-code"></a>[<span data-ttu-id="13308-293">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="13308-293">Visual Studio Code</span></span>](#tab/visual-studio-code) 

* <span data-ttu-id="13308-294">Dans le terminal intégré, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="13308-294">In the integrated terminal, run the following command:</span></span>    

  ```dotnetcli
  dotnet run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="13308-295">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="13308-295">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="13308-296">Dans le menu, sélectionnez **Exécuter > Démarrer sans débogage** .</span><span class="sxs-lookup"><span data-stu-id="13308-296">From the menu, select **Run > Start Without Debugging** .</span></span>

---

* <span data-ttu-id="13308-297">Copiez l’URL à partir de la barre d’adresse, ouvrez un autre onglet ou instance du navigateur, puis collez l’URL dans la barre d’adresse.</span><span class="sxs-lookup"><span data-stu-id="13308-297">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="13308-298">Choisissez un des navigateurs, entrez un nom et un message, puis sélectionnez le bouton **Envoyer le message** .</span><span class="sxs-lookup"><span data-stu-id="13308-298">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>  

  <span data-ttu-id="13308-299">Le nom et le message sont affichés instantanément dans les deux pages.</span><span class="sxs-lookup"><span data-stu-id="13308-299">The name and message are displayed on both pages instantly.</span></span>   

  ![::: No-Loc (Signalr) ::: Sample App](signalr/_static/2.x/signalr-get-started-finished.png) 

> [!TIP]    
> <span data-ttu-id="13308-301">Si l’application ne fonctionne pas, ouvrez vos outils de développement (F12) de navigateur et accédez à la console.</span><span class="sxs-lookup"><span data-stu-id="13308-301">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="13308-302">Vous pouvez observer des erreurs liées à votre code HTML et JavaScript.</span><span class="sxs-lookup"><span data-stu-id="13308-302">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="13308-303">Par exemple, supposez que vous placez *signalr.js* dans un dossier autre que celui stipulé.</span><span class="sxs-lookup"><span data-stu-id="13308-303">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="13308-304">Dans ce cas, la référence à ce fichier ne fonctionnera pas et vous verrez une erreur 404 dans la console.</span><span class="sxs-lookup"><span data-stu-id="13308-304">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>   
> <span data-ttu-id="13308-305">![Erreur de fichier SignalR.js introuvable](signalr/_static/2.x/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="13308-305">![signalr.js not found error](signalr/_static/2.x/f12-console.png)</span></span>    
## <a name="additional-resources"></a><span data-ttu-id="13308-306">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="13308-306">Additional resources</span></span> 
* [<span data-ttu-id="13308-307">Version YouTube de ce didacticiel</span><span class="sxs-lookup"><span data-stu-id="13308-307">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=iKlVmu-r0JQ)   

::: moniker-end
