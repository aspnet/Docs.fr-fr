---
title: Intégration et déploiement continus-DevOps avec ASP.NET Core et Azure
author: CamSoper
description: Intégration et déploiement continus dans DevOps avec ASP.NET Core et Azure
ms.author: scaddie
ms.date: 10/24/2018
ms.custom: devx-track-csharp, mvc, seodec18
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
uid: azure/devops/cicd
ms.openlocfilehash: eddd7034bf1860fb35cf00eefb7a11a408869700
ms.sourcegitcommit: ca34c1ac578e7d3daa0febf1810ba5fc74f60bbf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93052641"
---
# <a name="continuous-integration-and-deployment"></a><span data-ttu-id="895a7-103">Intégration et déploiement continus</span><span class="sxs-lookup"><span data-stu-id="895a7-103">Continuous integration and deployment</span></span>

<span data-ttu-id="895a7-104">Dans le chapitre précédent, vous avez créé un référentiel Git local pour l’application de lecture de flux simple.</span><span class="sxs-lookup"><span data-stu-id="895a7-104">In the previous chapter, you created a local Git repository for the Simple Feed Reader app.</span></span> <span data-ttu-id="895a7-105">Dans ce chapitre, vous allez publier ce code dans un dépôt GitHub et construire un pipeline Azure DevOps Services à l’aide de Azure Pipelines.</span><span class="sxs-lookup"><span data-stu-id="895a7-105">In this chapter, you'll publish that code to a GitHub repository and construct an Azure DevOps Services pipeline using Azure Pipelines.</span></span> <span data-ttu-id="895a7-106">Le pipeline permet des builds et des déploiements continus de l’application.</span><span class="sxs-lookup"><span data-stu-id="895a7-106">The pipeline enables continuous builds and deployments of the app.</span></span> <span data-ttu-id="895a7-107">Toute validation dans le référentiel GitHub déclenche une build et un déploiement sur l’emplacement intermédiaire de l’application Web Azure.</span><span class="sxs-lookup"><span data-stu-id="895a7-107">Any commit to the GitHub repository triggers a build and a deployment to the Azure Web App's staging slot.</span></span>

<span data-ttu-id="895a7-108">Dans cette section, vous allez effectuer les tâches suivantes :</span><span class="sxs-lookup"><span data-stu-id="895a7-108">In this section, you'll complete the following tasks:</span></span>

* <span data-ttu-id="895a7-109">Publier le code de l’application sur GitHub</span><span class="sxs-lookup"><span data-stu-id="895a7-109">Publish the app's code to GitHub</span></span>
* <span data-ttu-id="895a7-110">Déconnecter le déploiement Git local</span><span class="sxs-lookup"><span data-stu-id="895a7-110">Disconnect local Git deployment</span></span>
* <span data-ttu-id="895a7-111">Créer une organisation Azure DevOps</span><span class="sxs-lookup"><span data-stu-id="895a7-111">Create an Azure DevOps organization</span></span>
* <span data-ttu-id="895a7-112">Créer un projet d’équipe dans Azure DevOps Services</span><span class="sxs-lookup"><span data-stu-id="895a7-112">Create a team project in Azure DevOps Services</span></span>
* <span data-ttu-id="895a7-113">Créer une définition de build</span><span class="sxs-lookup"><span data-stu-id="895a7-113">Create a build definition</span></span>
* <span data-ttu-id="895a7-114">Créer un pipeline de mise en production</span><span class="sxs-lookup"><span data-stu-id="895a7-114">Create a release pipeline</span></span>
* <span data-ttu-id="895a7-115">Valider les modifications apportées à GitHub et les déployer automatiquement dans Azure</span><span class="sxs-lookup"><span data-stu-id="895a7-115">Commit changes to GitHub and automatically deploy to Azure</span></span>
* <span data-ttu-id="895a7-116">Examiner le pipeline Azure Pipelines</span><span class="sxs-lookup"><span data-stu-id="895a7-116">Examine the Azure Pipelines pipeline</span></span>

## <a name="publish-the-apps-code-to-github"></a><span data-ttu-id="895a7-117">Publier le code de l’application sur GitHub</span><span class="sxs-lookup"><span data-stu-id="895a7-117">Publish the app's code to GitHub</span></span>

1. <span data-ttu-id="895a7-118">Ouvrez une fenêtre de navigateur et accédez à `https://github.com` .</span><span class="sxs-lookup"><span data-stu-id="895a7-118">Open a browser window, and navigate to `https://github.com`.</span></span>
1. <span data-ttu-id="895a7-119">Cliquez sur la **+** liste déroulante dans l’en-tête, puis sélectionnez **nouveau référentiel** :</span><span class="sxs-lookup"><span data-stu-id="895a7-119">Click the **+** drop-down in the header, and select **New repository** :</span></span>

    ![GitHub nouvelle option de référentiel](media/cicd/github-new-repo.png)

1. <span data-ttu-id="895a7-121">Sélectionnez votre compte dans la liste déroulante **propriétaire** , puis entrez *simple-Feed-Reader* dans la zone de texte **nom du dépôt** .</span><span class="sxs-lookup"><span data-stu-id="895a7-121">Select your account in the **Owner** drop-down, and enter *simple-feed-reader* in the **Repository name** textbox.</span></span>
1. <span data-ttu-id="895a7-122">Cliquez sur le bouton **créer un référentiel** .</span><span class="sxs-lookup"><span data-stu-id="895a7-122">Click the **Create repository** button.</span></span>
1. <span data-ttu-id="895a7-123">Ouvrez l’interface de commande de votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="895a7-123">Open your local machine's command shell.</span></span> <span data-ttu-id="895a7-124">Accédez au répertoire dans lequel est stocké le référentiel git du *lecteur de flux simple* .</span><span class="sxs-lookup"><span data-stu-id="895a7-124">Navigate to the directory in which the *simple-feed-reader* Git repository is stored.</span></span>
1. <span data-ttu-id="895a7-125">Renommez l' *origine* existante à distance en *amont* .</span><span class="sxs-lookup"><span data-stu-id="895a7-125">Rename the existing *origin* remote to *upstream* .</span></span> <span data-ttu-id="895a7-126">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="895a7-126">Execute the following command:</span></span>

    ```console
    git remote rename origin upstream
    ```

1. <span data-ttu-id="895a7-127">Ajoutez une nouvelle *origine* à distance pointant vers votre copie du référentiel sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="895a7-127">Add a new *origin* remote pointing to your copy of the repository on GitHub.</span></span> <span data-ttu-id="895a7-128">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="895a7-128">Execute the following command:</span></span>

    ```console
    git remote add origin https://github.com/<GitHub_username>/simple-feed-reader/
    ```

1. <span data-ttu-id="895a7-129">Publiez votre référentiel Git local dans le référentiel GitHub nouvellement créé.</span><span class="sxs-lookup"><span data-stu-id="895a7-129">Publish your local Git repository to the newly created GitHub repository.</span></span> <span data-ttu-id="895a7-130">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="895a7-130">Execute the following command:</span></span>

    ```console
    git push -u origin master
    ```

1. <span data-ttu-id="895a7-131">Ouvrez une fenêtre de navigateur et accédez à `https://github.com/<GitHub_username>/simple-feed-reader/` .</span><span class="sxs-lookup"><span data-stu-id="895a7-131">Open a browser window, and navigate to `https://github.com/<GitHub_username>/simple-feed-reader/`.</span></span> <span data-ttu-id="895a7-132">Vérifiez que votre code apparaît dans le référentiel GitHub.</span><span class="sxs-lookup"><span data-stu-id="895a7-132">Validate that your code appears in the GitHub repository.</span></span>

## <a name="disconnect-local-git-deployment"></a><span data-ttu-id="895a7-133">Déconnecter le déploiement Git local</span><span class="sxs-lookup"><span data-stu-id="895a7-133">Disconnect local Git deployment</span></span>

<span data-ttu-id="895a7-134">Supprimez le déploiement Git local en suivant les étapes ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="895a7-134">Remove the local Git deployment with the following steps.</span></span> <span data-ttu-id="895a7-135">Azure Pipelines (un service Azure DevOps) remplace et augmente cette fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="895a7-135">Azure Pipelines (an Azure DevOps service) both replaces and augments that functionality.</span></span>

1. <span data-ttu-id="895a7-136">Ouvrez le [portail Azure](https://portal.azure.com/), puis accédez à l’application Web *intermédiaire (myWebApp \<unique_number\> /staging)* .</span><span class="sxs-lookup"><span data-stu-id="895a7-136">Open the [Azure portal](https://portal.azure.com/), and navigate to the *staging (mywebapp\<unique_number\>/staging)* Web App.</span></span> <span data-ttu-id="895a7-137">L’application Web peut être rapidement localisée en entrant *intermédiaire* dans la zone de recherche du portail :</span><span class="sxs-lookup"><span data-stu-id="895a7-137">The Web App can be quickly located by entering *staging* in the portal's search box:</span></span>

    ![terme de recherche de l’application Web intermédiaire](media/cicd/portal-search-box.png)

1. <span data-ttu-id="895a7-139">Cliquez sur **Centre de déploiement** .</span><span class="sxs-lookup"><span data-stu-id="895a7-139">Click **Deployment Center** .</span></span> <span data-ttu-id="895a7-140">Un nouveau panneau s’affiche.</span><span class="sxs-lookup"><span data-stu-id="895a7-140">A new panel appears.</span></span> <span data-ttu-id="895a7-141">Cliquez sur **déconnecter** pour supprimer la configuration du contrôle de code source git locale qui a été ajoutée dans le chapitre précédent.</span><span class="sxs-lookup"><span data-stu-id="895a7-141">Click **Disconnect** to remove the local Git source control configuration that was added in the previous chapter.</span></span> <span data-ttu-id="895a7-142">Confirmez l’opération de suppression en cliquant sur le bouton **Oui** .</span><span class="sxs-lookup"><span data-stu-id="895a7-142">Confirm the removal operation by clicking the **Yes** button.</span></span>
1. <span data-ttu-id="895a7-143">Accédez au *<mywebapp unique_number>* App service.</span><span class="sxs-lookup"><span data-stu-id="895a7-143">Navigate to the *mywebapp<unique_number>* App Service.</span></span> <span data-ttu-id="895a7-144">En guise de rappel, la zone de recherche du portail peut être utilisée pour localiser rapidement le App Service.</span><span class="sxs-lookup"><span data-stu-id="895a7-144">As a reminder, the portal's search box can be used to quickly locate the App Service.</span></span>
1. <span data-ttu-id="895a7-145">Cliquez sur **Centre de déploiement** .</span><span class="sxs-lookup"><span data-stu-id="895a7-145">Click **Deployment Center** .</span></span> <span data-ttu-id="895a7-146">Un nouveau panneau s’affiche.</span><span class="sxs-lookup"><span data-stu-id="895a7-146">A new panel appears.</span></span> <span data-ttu-id="895a7-147">Cliquez sur **déconnecter** pour supprimer la configuration du contrôle de code source git locale qui a été ajoutée dans le chapitre précédent.</span><span class="sxs-lookup"><span data-stu-id="895a7-147">Click **Disconnect** to remove the local Git source control configuration that was added in the previous chapter.</span></span> <span data-ttu-id="895a7-148">Confirmez l’opération de suppression en cliquant sur le bouton **Oui** .</span><span class="sxs-lookup"><span data-stu-id="895a7-148">Confirm the removal operation by clicking the **Yes** button.</span></span>

## <a name="create-an-azure-devops-organization"></a><span data-ttu-id="895a7-149">Créer une organisation Azure DevOps</span><span class="sxs-lookup"><span data-stu-id="895a7-149">Create an Azure DevOps organization</span></span>

1. <span data-ttu-id="895a7-150">Ouvrez un navigateur et accédez à la page de création de l' [organisation Azure DevOps](https://go.microsoft.com/fwlink/?LinkId=307137).</span><span class="sxs-lookup"><span data-stu-id="895a7-150">Open a browser, and navigate to the [Azure DevOps organization creation page](https://go.microsoft.com/fwlink/?LinkId=307137).</span></span>
1. <span data-ttu-id="895a7-151">Tapez un nom unique dans la zone de texte **choisir un nom mémorable** pour former l’URL d’accès à votre organisation Azure DevOps.</span><span class="sxs-lookup"><span data-stu-id="895a7-151">Type a unique name into the **Pick a memorable name** textbox to form the URL for accessing your Azure DevOps organization.</span></span>
1. <span data-ttu-id="895a7-152">Sélectionnez la case d’option **git** , car le code est hébergé dans un référentiel github.</span><span class="sxs-lookup"><span data-stu-id="895a7-152">Select the **Git** radio button, since the code is hosted in a GitHub repository.</span></span>
1. <span data-ttu-id="895a7-153">Cliquez sur le bouton **Continuer** .</span><span class="sxs-lookup"><span data-stu-id="895a7-153">Click the **Continue** button.</span></span> <span data-ttu-id="895a7-154">Après une brève attente, un compte et un projet d’équipe, nommés *MyFirstProject* , sont créés.</span><span class="sxs-lookup"><span data-stu-id="895a7-154">After a short wait, an account and a team project, named *MyFirstProject* , are created.</span></span>

    ![Page de création de l’organisation Azure DevOps](media/cicd/vsts-account-creation.png)

1. <span data-ttu-id="895a7-156">Ouvrez le message électronique de confirmation indiquant que l’organisation et le projet Azure DevOps sont prêts à être utilisés.</span><span class="sxs-lookup"><span data-stu-id="895a7-156">Open the confirmation email indicating that the Azure DevOps organization and project are ready for use.</span></span> <span data-ttu-id="895a7-157">Cliquez sur le bouton **Démarrer votre projet** :</span><span class="sxs-lookup"><span data-stu-id="895a7-157">Click the **Start your project** button:</span></span>

    ![Bouton Démarrer votre projet](media/cicd/vsts-start-project.png)

1. <span data-ttu-id="895a7-159">Un navigateur s’ouvre sur *\<account_name\> . VisualStudio.com* .</span><span class="sxs-lookup"><span data-stu-id="895a7-159">A browser opens to *\<account_name\>.visualstudio.com* .</span></span> <span data-ttu-id="895a7-160">Cliquez sur le lien *MyFirstProject* pour commencer à configurer le pipeline DevOps du projet.</span><span class="sxs-lookup"><span data-stu-id="895a7-160">Click the *MyFirstProject* link to begin configuring the project's DevOps pipeline.</span></span>

## <a name="configure-the-azure-pipelines-pipeline"></a><span data-ttu-id="895a7-161">Configurer le pipeline de Azure Pipelines</span><span class="sxs-lookup"><span data-stu-id="895a7-161">Configure the Azure Pipelines pipeline</span></span>

<span data-ttu-id="895a7-162">Il existe trois étapes distinctes à effectuer.</span><span class="sxs-lookup"><span data-stu-id="895a7-162">There are three distinct steps to complete.</span></span> <span data-ttu-id="895a7-163">La réalisation des étapes décrites dans les trois sections suivantes aboutit à un pipeline DevOps opérationnel.</span><span class="sxs-lookup"><span data-stu-id="895a7-163">Completing the steps in the following three sections results in an operational DevOps pipeline.</span></span>

### <a name="grant-azure-devops-access-to-the-github-repository"></a><span data-ttu-id="895a7-164">Accorder à Azure DevOps l’accès au référentiel GitHub</span><span class="sxs-lookup"><span data-stu-id="895a7-164">Grant Azure DevOps access to the GitHub repository</span></span>

1. <span data-ttu-id="895a7-165">Développez le **Code de build ou à partir d’un accordéon de référentiel externe** .</span><span class="sxs-lookup"><span data-stu-id="895a7-165">Expand the **or build code from an external repository** accordion.</span></span> <span data-ttu-id="895a7-166">Cliquez sur le bouton **configurer la build** :</span><span class="sxs-lookup"><span data-stu-id="895a7-166">Click the **Setup Build** button:</span></span>

    ![Bouton Configurer la build](media/cicd/vsts-setup-build.png)

1. <span data-ttu-id="895a7-168">Sélectionnez l’option **GitHub** dans la section **Sélectionner une source** :</span><span class="sxs-lookup"><span data-stu-id="895a7-168">Select the **GitHub** option from the **Select a source** section:</span></span>

    ![Sélectionner une source-GitHub](media/cicd/vsts-select-source.png)

1. <span data-ttu-id="895a7-170">L’autorisation est requise avant qu’Azure DevOps puisse accéder à votre référentiel GitHub.</span><span class="sxs-lookup"><span data-stu-id="895a7-170">Authorization is required before Azure DevOps can access your GitHub repository.</span></span> <span data-ttu-id="895a7-171">Entrez *<GitHub_username> connexion GitHub* dans la zone de texte nom de la **connexion** .</span><span class="sxs-lookup"><span data-stu-id="895a7-171">Enter *<GitHub_username> GitHub connection* in the **Connection name** textbox.</span></span> <span data-ttu-id="895a7-172">Exemple :</span><span class="sxs-lookup"><span data-stu-id="895a7-172">For example:</span></span>

    ![Nom de la connexion GitHub](media/cicd/vsts-repo-authz.png)

1. <span data-ttu-id="895a7-174">Si l’authentification à deux facteurs est activée sur votre compte GitHub, un jeton d’accès personnel est requis.</span><span class="sxs-lookup"><span data-stu-id="895a7-174">If two-factor authentication is enabled on your GitHub account, a personal access token is required.</span></span> <span data-ttu-id="895a7-175">Dans ce cas, cliquez sur le lien **autoriser avec un jeton d’accès personnel GitHub** .</span><span class="sxs-lookup"><span data-stu-id="895a7-175">In that case, click the **Authorize with a GitHub personal access token** link.</span></span> <span data-ttu-id="895a7-176">Pour obtenir de l’aide, consultez les [instructions officielles de création d’un jeton d’accès personnel GitHub](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) .</span><span class="sxs-lookup"><span data-stu-id="895a7-176">See the [official GitHub personal access token creation instructions](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) for help.</span></span> <span data-ttu-id="895a7-177">Seule l’étendue *référentiel* des autorisations est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="895a7-177">Only the *repo* scope of permissions is needed.</span></span> <span data-ttu-id="895a7-178">Dans le cas contraire, cliquez sur le bouton **autoriser à l’aide d’OAuth** .</span><span class="sxs-lookup"><span data-stu-id="895a7-178">Otherwise, click the **Authorize using OAuth** button.</span></span>
1. <span data-ttu-id="895a7-179">Lorsque vous y êtes invité, connectez-vous à votre compte GitHub.</span><span class="sxs-lookup"><span data-stu-id="895a7-179">When prompted, sign in to your GitHub account.</span></span> <span data-ttu-id="895a7-180">Sélectionnez ensuite autoriser pour accorder l’accès à votre organisation Azure DevOps.</span><span class="sxs-lookup"><span data-stu-id="895a7-180">Then select Authorize to grant access to your Azure DevOps organization.</span></span> <span data-ttu-id="895a7-181">En cas de réussite, un nouveau point de terminaison de service est créé.</span><span class="sxs-lookup"><span data-stu-id="895a7-181">If successful, a new service endpoint is created.</span></span>
1. <span data-ttu-id="895a7-182">Cliquez sur le bouton de sélection en regard du bouton **référentiel** .</span><span class="sxs-lookup"><span data-stu-id="895a7-182">Click the ellipsis button next to the **Repository** button.</span></span> <span data-ttu-id="895a7-183">Sélectionnez le *<GitHub_username>référentiel/simple-Feed-Reader* dans la liste.</span><span class="sxs-lookup"><span data-stu-id="895a7-183">Select the *<GitHub_username>/simple-feed-reader* repository from the list.</span></span> <span data-ttu-id="895a7-184">Cliquez sur le bouton **Sélectionner** .</span><span class="sxs-lookup"><span data-stu-id="895a7-184">Click the **Select** button.</span></span>
1. <span data-ttu-id="895a7-185">Sélectionnez la branche *principale* à partir de la branche par défaut pour la liste déroulante **manuelle et planifiée des builds** .</span><span class="sxs-lookup"><span data-stu-id="895a7-185">Select the *master* branch from the **Default branch for manual and scheduled builds** drop-down.</span></span> <span data-ttu-id="895a7-186">Cliquez sur le bouton **Continuer** .</span><span class="sxs-lookup"><span data-stu-id="895a7-186">Click the **Continue** button.</span></span> <span data-ttu-id="895a7-187">La page sélection du modèle s’affiche.</span><span class="sxs-lookup"><span data-stu-id="895a7-187">The template selection page appears.</span></span>

### <a name="create-the-build-definition"></a><span data-ttu-id="895a7-188">Créer la définition de build</span><span class="sxs-lookup"><span data-stu-id="895a7-188">Create the build definition</span></span>

1. <span data-ttu-id="895a7-189">Dans la page sélection du modèle, entrez *ASP.net Core* dans la zone de recherche :</span><span class="sxs-lookup"><span data-stu-id="895a7-189">From the template selection page, enter *ASP.NET Core* in the search box:</span></span>

    ![ASP.NET Core la recherche sur la page de modèle](media/cicd/vsts-template-selection.png)

1. <span data-ttu-id="895a7-191">Les résultats de la recherche du modèle s’affichent.</span><span class="sxs-lookup"><span data-stu-id="895a7-191">The template search results appear.</span></span> <span data-ttu-id="895a7-192">Pointez sur le modèle **ASP.net Core** , puis cliquez sur le bouton **appliquer** .</span><span class="sxs-lookup"><span data-stu-id="895a7-192">Hover over the **ASP.NET Core** template, and click the **Apply** button.</span></span>
1. <span data-ttu-id="895a7-193">L’onglet **tâches** de la définition de build s’affiche.</span><span class="sxs-lookup"><span data-stu-id="895a7-193">The **Tasks** tab of the build definition appears.</span></span> <span data-ttu-id="895a7-194">Cliquez sur l’onglet **déclencheurs** .</span><span class="sxs-lookup"><span data-stu-id="895a7-194">Click the **Triggers** tab.</span></span>
1. <span data-ttu-id="895a7-195">Cochez la case **activer l’intégration continue** .</span><span class="sxs-lookup"><span data-stu-id="895a7-195">Check the **Enable continuous integration** box.</span></span> <span data-ttu-id="895a7-196">Dans la section **filtres de branche** , vérifiez que la liste déroulante **type** est définie sur *include* .</span><span class="sxs-lookup"><span data-stu-id="895a7-196">Under the **Branch filters** section, confirm that the **Type** drop-down is set to *Include* .</span></span> <span data-ttu-id="895a7-197">Définissez la liste déroulante **spécification de branche** sur *Master* .</span><span class="sxs-lookup"><span data-stu-id="895a7-197">Set the **Branch specification** drop-down to *master* .</span></span>

    ![Activer les paramètres d’intégration continue](media/cicd/vsts-enable-ci.png)

    <span data-ttu-id="895a7-199">Ces paramètres provoquent le déclenchement d’une génération lorsqu’une modification est envoyée à la branche *maître* du référentiel github.</span><span class="sxs-lookup"><span data-stu-id="895a7-199">These settings cause a build to trigger when any change is pushed to the *master* branch of the GitHub repository.</span></span> <span data-ttu-id="895a7-200">L’intégration continue est testée dans la section [valider les modifications apportées à GitHub et déployer automatiquement sur Azure](#commit-changes-to-github-and-automatically-deploy-to-azure) .</span><span class="sxs-lookup"><span data-stu-id="895a7-200">Continuous integration is tested in the [Commit changes to GitHub and automatically deploy to Azure](#commit-changes-to-github-and-automatically-deploy-to-azure) section.</span></span>

1. <span data-ttu-id="895a7-201">Cliquez sur le bouton **enregistrer la file d’attente &** , puis sélectionnez l’option **Enregistrer** :</span><span class="sxs-lookup"><span data-stu-id="895a7-201">Click the **Save & queue** button, and select the **Save** option:</span></span>

    ![Bouton Enregistrer](media/cicd/vsts-save-build.png)

1. <span data-ttu-id="895a7-203">La boîte de dialogue modale suivante s’affiche :</span><span class="sxs-lookup"><span data-stu-id="895a7-203">The following modal dialog appears:</span></span>

    ![Boîte de dialogue Enregistrer la définition de build-modal](media/cicd/vsts-save-modal.png)

    <span data-ttu-id="895a7-205">Utilisez le dossier par défaut de *\\* , puis cliquez sur le bouton **Enregistrer** .</span><span class="sxs-lookup"><span data-stu-id="895a7-205">Use the default folder of *\\* , and click the **Save** button.</span></span>

### <a name="create-the-release-pipeline"></a><span data-ttu-id="895a7-206">Créer le pipeline de mise en production</span><span class="sxs-lookup"><span data-stu-id="895a7-206">Create the release pipeline</span></span>

1. <span data-ttu-id="895a7-207">Cliquez sur l’onglet **mises** en production de votre projet d’équipe.</span><span class="sxs-lookup"><span data-stu-id="895a7-207">Click the **Releases** tab of your team project.</span></span> <span data-ttu-id="895a7-208">Cliquez sur le bouton **nouveau pipeline** .</span><span class="sxs-lookup"><span data-stu-id="895a7-208">Click the **New pipeline** button.</span></span>

    ![Onglet versions-bouton nouvelle définition](media/cicd/vsts-new-release-definition.png)

    <span data-ttu-id="895a7-210">Le volet sélection du modèle s’affiche.</span><span class="sxs-lookup"><span data-stu-id="895a7-210">The template selection pane appears.</span></span>

1. <span data-ttu-id="895a7-211">Dans la page sélection du modèle, entrez *app service* dans la zone de recherche :</span><span class="sxs-lookup"><span data-stu-id="895a7-211">From the template selection page, enter *App Service* in the search box:</span></span>

    ![Zone de recherche du modèle de pipeline de mise en version](media/cicd/vsts-release-template-search.png)

1. <span data-ttu-id="895a7-213">Les résultats de la recherche du modèle s’affichent.</span><span class="sxs-lookup"><span data-stu-id="895a7-213">The template search results appear.</span></span> <span data-ttu-id="895a7-214">Pointez sur le modèle **Azure App service déploiement avec emplacement** , puis cliquez sur le bouton **appliquer** .</span><span class="sxs-lookup"><span data-stu-id="895a7-214">Hover over the **Azure App Service Deployment with Slot** template, and click the **Apply** button.</span></span> <span data-ttu-id="895a7-215">L’onglet **pipeline** du pipeline de mise en sortie s’affiche.</span><span class="sxs-lookup"><span data-stu-id="895a7-215">The **Pipeline** tab of the release pipeline appears.</span></span>

    ![Onglet pipeline du pipeline de mise en version](media/cicd/vsts-release-definition-pipeline.png)

1. <span data-ttu-id="895a7-217">Cliquez sur le bouton **Ajouter** dans la zone **artefacts** .</span><span class="sxs-lookup"><span data-stu-id="895a7-217">Click the **Add** button in the **Artifacts** box.</span></span> <span data-ttu-id="895a7-218">Le panneau **Ajouter un artefact** s’affiche :</span><span class="sxs-lookup"><span data-stu-id="895a7-218">The **Add artifact** panel appears:</span></span>

    ![Pipeline de mise en version-ajouter un panneau d’artefact](media/cicd/vsts-release-add-artifact.png)

1. <span data-ttu-id="895a7-220">Sélectionnez la vignette **Build** dans la section **type de source** .</span><span class="sxs-lookup"><span data-stu-id="895a7-220">Select the **Build** tile from the **Source type** section.</span></span> <span data-ttu-id="895a7-221">Ce type autorise la liaison du pipeline de mise en version à la définition de Build.</span><span class="sxs-lookup"><span data-stu-id="895a7-221">This type allows for the linking of the release pipeline to the build definition.</span></span>
1. <span data-ttu-id="895a7-222">Sélectionnez *MyFirstProject* dans la liste déroulante **projet** .</span><span class="sxs-lookup"><span data-stu-id="895a7-222">Select *MyFirstProject* from the **Project** drop-down.</span></span>
1. <span data-ttu-id="895a7-223">Sélectionnez le nom de la définition de build, *MyFirstProject-ASP.net Core-ci* , dans la liste déroulante **source (définition de Build)** .</span><span class="sxs-lookup"><span data-stu-id="895a7-223">Select the build definition name, *MyFirstProject-ASP.NET Core-CI* , from the **Source (Build definition)** drop-down.</span></span>
1. <span data-ttu-id="895a7-224">Sélectionnez *dernier* dans la liste déroulante **version par défaut** .</span><span class="sxs-lookup"><span data-stu-id="895a7-224">Select *Latest* from the **Default version** drop-down.</span></span> <span data-ttu-id="895a7-225">Cette option génère les artefacts produits par la dernière exécution de la définition de Build.</span><span class="sxs-lookup"><span data-stu-id="895a7-225">This option builds the artifacts produced by the latest run of the build definition.</span></span>
1. <span data-ttu-id="895a7-226">Remplacez le texte de la zone de texte **alias source** par *Drop* .</span><span class="sxs-lookup"><span data-stu-id="895a7-226">Replace the text in the **Source alias** textbox with *Drop* .</span></span>
1. <span data-ttu-id="895a7-227">Cliquez sur le bouton **Add** .</span><span class="sxs-lookup"><span data-stu-id="895a7-227">Click the **Add** button.</span></span> <span data-ttu-id="895a7-228">La section **artefacts** est mise à jour pour afficher les modifications.</span><span class="sxs-lookup"><span data-stu-id="895a7-228">The **Artifacts** section updates to display the changes.</span></span>
1. <span data-ttu-id="895a7-229">Cliquez sur l’icône représentant un éclair pour activer les déploiements continus :</span><span class="sxs-lookup"><span data-stu-id="895a7-229">Click the lightning bolt icon to enable continuous deployments:</span></span>

    ![Artefacts du pipeline de mise en sortie-icône éclair](media/cicd/vsts-artifacts-lightning-bolt.png)

    <span data-ttu-id="895a7-231">Lorsque cette option est activée, un déploiement se produit chaque fois qu’une nouvelle build est disponible.</span><span class="sxs-lookup"><span data-stu-id="895a7-231">With this option enabled, a deployment occurs each time a new build is available.</span></span>
1. <span data-ttu-id="895a7-232">Un panneau **déclencheur de déploiement continu** s’affiche à droite.</span><span class="sxs-lookup"><span data-stu-id="895a7-232">A **Continuous deployment trigger** panel appears to the right.</span></span> <span data-ttu-id="895a7-233">Cliquez sur le bouton bascule pour activer la fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="895a7-233">Click the toggle button to enable the feature.</span></span> <span data-ttu-id="895a7-234">Il n’est pas nécessaire d’activer le **déclencheur de requête de tirage** .</span><span class="sxs-lookup"><span data-stu-id="895a7-234">It isn't necessary to enable the **Pull request trigger** .</span></span>
1. <span data-ttu-id="895a7-235">Cliquez sur la liste déroulante **Ajouter** dans la section **créer des filtres de branche** .</span><span class="sxs-lookup"><span data-stu-id="895a7-235">Click the **Add** drop-down in the **Build branch filters** section.</span></span> <span data-ttu-id="895a7-236">Choisissez l’option **de branche par défaut de la définition de build** .</span><span class="sxs-lookup"><span data-stu-id="895a7-236">Choose the **Build Definition's default branch** option.</span></span> <span data-ttu-id="895a7-237">Ce filtre entraîne le déclenchement de la mise en sortie uniquement pour une build à partir de la branche *maître* du référentiel github.</span><span class="sxs-lookup"><span data-stu-id="895a7-237">This filter causes the release to trigger only for a build from the GitHub repository's *master* branch.</span></span>
1. <span data-ttu-id="895a7-238">Cliquez sur le bouton **Enregistrer** .</span><span class="sxs-lookup"><span data-stu-id="895a7-238">Click the **Save** button.</span></span> <span data-ttu-id="895a7-239">Cliquez sur le bouton **OK** dans la boîte de dialogue d' **enregistrement** modal.</span><span class="sxs-lookup"><span data-stu-id="895a7-239">Click the **OK** button in the resulting **Save** modal dialog.</span></span>
1. <span data-ttu-id="895a7-240">Cliquez sur la zone **environnement 1** .</span><span class="sxs-lookup"><span data-stu-id="895a7-240">Click the **Environment 1** box.</span></span> <span data-ttu-id="895a7-241">Un panneau d' **environnement** s’affiche à droite.</span><span class="sxs-lookup"><span data-stu-id="895a7-241">An **Environment** panel appears to the right.</span></span> <span data-ttu-id="895a7-242">Modifiez le texte de l' *environnement 1* dans la zone de texte nom de l' **environnement** en *production* .</span><span class="sxs-lookup"><span data-stu-id="895a7-242">Change the *Environment 1* text in the **Environment name** textbox to *Production* .</span></span>

   ![Zone de texte Release pipeline-nom de l’environnement](media/cicd/vsts-environment-name-textbox.png)

1. <span data-ttu-id="895a7-244">Cliquez sur le lien **1 phase, 2 tâches** dans la zone **production** :</span><span class="sxs-lookup"><span data-stu-id="895a7-244">Click the **1 phase, 2 tasks** link in the **Production** box:</span></span>

    ![Pipeline de mise en production-link.png de l’environnement de production](media/cicd/vsts-production-link.png)

    <span data-ttu-id="895a7-246">L’onglet **tâches** de l’environnement s’affiche.</span><span class="sxs-lookup"><span data-stu-id="895a7-246">The **Tasks** tab of the environment appears.</span></span>
1. <span data-ttu-id="895a7-247">Cliquez sur la tâche **déployer Azure App service à l’emplacement** .</span><span class="sxs-lookup"><span data-stu-id="895a7-247">Click the **Deploy Azure App Service to Slot** task.</span></span> <span data-ttu-id="895a7-248">Ses paramètres s’affichent dans un panneau à droite.</span><span class="sxs-lookup"><span data-stu-id="895a7-248">Its settings appear in a panel to the right.</span></span>
1. <span data-ttu-id="895a7-249">Sélectionnez l’abonnement Azure associé à la App Service dans la liste déroulante **abonnement Azure** .</span><span class="sxs-lookup"><span data-stu-id="895a7-249">Select the Azure subscription associated with the App Service from the **Azure subscription** drop-down.</span></span> <span data-ttu-id="895a7-250">Une fois sélectionné, cliquez sur le bouton **autoriser** .</span><span class="sxs-lookup"><span data-stu-id="895a7-250">Once selected, click the **Authorize** button.</span></span>
1. <span data-ttu-id="895a7-251">Sélectionnez *application Web* dans la liste déroulante **type d’application** .</span><span class="sxs-lookup"><span data-stu-id="895a7-251">Select *Web App* from the **App type** drop-down.</span></span>
1. <span data-ttu-id="895a7-252">Sélectionnez *myWebApp/<unique_number/>* dans la liste déroulante nom de l' **app service** .</span><span class="sxs-lookup"><span data-stu-id="895a7-252">Select *mywebapp/<unique_number/>* from the **App service name** drop-down.</span></span>
1. <span data-ttu-id="895a7-253">Sélectionnez *AzureTutorial* dans la liste déroulante **groupe de ressources** .</span><span class="sxs-lookup"><span data-stu-id="895a7-253">Select *AzureTutorial* from the **Resource group** drop-down.</span></span>
1. <span data-ttu-id="895a7-254">Sélectionnez *intermédiaire* dans la liste déroulante **emplacement** .</span><span class="sxs-lookup"><span data-stu-id="895a7-254">Select *staging* from the **Slot** drop-down.</span></span>
1. <span data-ttu-id="895a7-255">Cliquez sur le bouton **Enregistrer** .</span><span class="sxs-lookup"><span data-stu-id="895a7-255">Click the **Save** button.</span></span>
1. <span data-ttu-id="895a7-256">Pointez sur le nom du pipeline de version par défaut.</span><span class="sxs-lookup"><span data-stu-id="895a7-256">Hover over the default release pipeline name.</span></span> <span data-ttu-id="895a7-257">Cliquez sur l’icône de crayon pour la modifier.</span><span class="sxs-lookup"><span data-stu-id="895a7-257">Click the pencil icon to edit it.</span></span> <span data-ttu-id="895a7-258">Utilisez *MyFirstProject-ASP.net Core-CD* comme nom.</span><span class="sxs-lookup"><span data-stu-id="895a7-258">Use *MyFirstProject-ASP.NET Core-CD* as the name.</span></span>

    ![Nom du pipeline de version](media/cicd/vsts-release-definition-name.png)

1. <span data-ttu-id="895a7-260">Cliquez sur le bouton **Enregistrer** .</span><span class="sxs-lookup"><span data-stu-id="895a7-260">Click the **Save** button.</span></span>

## <a name="commit-changes-to-github-and-automatically-deploy-to-azure"></a><span data-ttu-id="895a7-261">Valider les modifications apportées à GitHub et les déployer automatiquement dans Azure</span><span class="sxs-lookup"><span data-stu-id="895a7-261">Commit changes to GitHub and automatically deploy to Azure</span></span>

1. <span data-ttu-id="895a7-262">Ouvrez *SimpleFeedReader. sln* dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="895a7-262">Open *SimpleFeedReader.sln* in Visual Studio.</span></span>
1. <span data-ttu-id="895a7-263">Dans Explorateur de solutions, ouvrez *Pages\Index.cshtml* .</span><span class="sxs-lookup"><span data-stu-id="895a7-263">In Solution Explorer, open *Pages\Index.cshtml* .</span></span> <span data-ttu-id="895a7-264">Remplacez `<h2>Simple Feed Reader - V3</h2>` par `<h2>Simple Feed Reader - V4</h2>`.</span><span class="sxs-lookup"><span data-stu-id="895a7-264">Change `<h2>Simple Feed Reader - V3</h2>` to `<h2>Simple Feed Reader - V4</h2>`.</span></span>
1. <span data-ttu-id="895a7-265">Appuyez sur **CTRL** + **MAJ** + **B** pour générer l’application.</span><span class="sxs-lookup"><span data-stu-id="895a7-265">Press **Ctrl**+**Shift**+**B** to build the app.</span></span>
1. <span data-ttu-id="895a7-266">Validez le fichier dans le référentiel GitHub.</span><span class="sxs-lookup"><span data-stu-id="895a7-266">Commit the file to the GitHub repository.</span></span> <span data-ttu-id="895a7-267">Utilisez la page **modifications** dans l’onglet *Team Explorer* de Visual Studio, ou exécutez la commande suivante à l’aide de l’interface de commande de l’ordinateur local :</span><span class="sxs-lookup"><span data-stu-id="895a7-267">Use either the **Changes** page in Visual Studio's *Team Explorer* tab, or execute the following using the local machine's command shell:</span></span>

    ```console
    git commit -a -m "upgraded to V4"
    ```

1. <span data-ttu-id="895a7-268">Transmettent la modification dans la branche *principale* à l' *origine* distante de votre référentiel GitHub :</span><span class="sxs-lookup"><span data-stu-id="895a7-268">Push the change in the *master* branch to the *origin* remote of your GitHub repository:</span></span>

    ```console
    git push origin master
    ```

    <span data-ttu-id="895a7-269">La validation s’affiche dans la branche *principale* du dépôt github :</span><span class="sxs-lookup"><span data-stu-id="895a7-269">The commit appears in the GitHub repository's *master* branch:</span></span>

    ![Validation GitHub dans la branche maître](media/cicd/github-commit.png)

    <span data-ttu-id="895a7-271">La build est déclenchée, car l’intégration continue est activée dans l’onglet **déclencheurs** de la définition de build :</span><span class="sxs-lookup"><span data-stu-id="895a7-271">The build is triggered, since continuous integration is enabled in the build definition's **Triggers** tab:</span></span>

    ![activer l’intégration continue](media/cicd/enable-ci.png)

1. <span data-ttu-id="895a7-273">Accédez à l’onglet en **file d’attente** de la page **Azure pipelines**  >  **Builds** dans Azure DevOps services.</span><span class="sxs-lookup"><span data-stu-id="895a7-273">Navigate to the **Queued** tab of the **Azure Pipelines** > **Builds** page in Azure DevOps Services.</span></span> <span data-ttu-id="895a7-274">La build en file d’attente affiche la branche et la validation qui ont déclenché la build :</span><span class="sxs-lookup"><span data-stu-id="895a7-274">The queued build shows the branch and commit that triggered the build:</span></span>

    ![Build mise en file d’attente](media/cicd/build-queued.png)

1. <span data-ttu-id="895a7-276">Une fois la génération réussie, un déploiement sur Azure se produit.</span><span class="sxs-lookup"><span data-stu-id="895a7-276">Once the build succeeds, a deployment to Azure occurs.</span></span> <span data-ttu-id="895a7-277">Accédez à l’application dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="895a7-277">Navigate to the app in the browser.</span></span> <span data-ttu-id="895a7-278">Notez que le texte « v4 » s’affiche dans l’en-tête :</span><span class="sxs-lookup"><span data-stu-id="895a7-278">Notice that the "V4" text appears in the heading:</span></span>

    ![application mise à jour](media/cicd/updated-app-v4.png)

## <a name="examine-the-azure-pipelines-pipeline"></a><span data-ttu-id="895a7-280">Examiner le pipeline Azure Pipelines</span><span class="sxs-lookup"><span data-stu-id="895a7-280">Examine the Azure Pipelines pipeline</span></span>

### <a name="build-definition"></a><span data-ttu-id="895a7-281">Définition de build</span><span class="sxs-lookup"><span data-stu-id="895a7-281">Build definition</span></span>

<span data-ttu-id="895a7-282">Une définition de Build a été créée avec le nom *MyFirstProject-ASP.net Core-ci* .</span><span class="sxs-lookup"><span data-stu-id="895a7-282">A build definition was created with the name *MyFirstProject-ASP.NET Core-CI* .</span></span> <span data-ttu-id="895a7-283">Une fois l’opération terminée, la génération produit un fichier *. zip* incluant les ressources à publier.</span><span class="sxs-lookup"><span data-stu-id="895a7-283">Upon completion, the build produces a *.zip* file including the assets to be published.</span></span> <span data-ttu-id="895a7-284">Le pipeline de mise en version déploie ces ressources dans Azure.</span><span class="sxs-lookup"><span data-stu-id="895a7-284">The release pipeline deploys those assets to Azure.</span></span>

<span data-ttu-id="895a7-285">L’onglet **tâches** de la définition de build répertorie les étapes individuelles en cours d’utilisation.</span><span class="sxs-lookup"><span data-stu-id="895a7-285">The build definition's **Tasks** tab lists the individual steps being used.</span></span> <span data-ttu-id="895a7-286">Il existe cinq tâches de génération.</span><span class="sxs-lookup"><span data-stu-id="895a7-286">There are five build tasks.</span></span>

![tâches de définition de build](media/cicd/build-definition-tasks.png)

1. <span data-ttu-id="895a7-288">**Restaurer** &mdash; Exécute la `dotnet restore` commande pour restaurer les packages NuGet de l’application.</span><span class="sxs-lookup"><span data-stu-id="895a7-288">**Restore** &mdash; Executes the `dotnet restore` command to restore the app's NuGet packages.</span></span> <span data-ttu-id="895a7-289">Le flux de package par défaut utilisé est nuget.org.</span><span class="sxs-lookup"><span data-stu-id="895a7-289">The default package feed used is nuget.org.</span></span>
1. <span data-ttu-id="895a7-290">**Créer** &mdash; Exécute la `dotnet build --configuration release` commande pour compiler le code de l’application.</span><span class="sxs-lookup"><span data-stu-id="895a7-290">**Build** &mdash; Executes the `dotnet build --configuration release` command to compile the app's code.</span></span> <span data-ttu-id="895a7-291">Cette `--configuration` option est utilisée pour produire une version optimisée du code, qui convient au déploiement dans un environnement de production.</span><span class="sxs-lookup"><span data-stu-id="895a7-291">This `--configuration` option is used to produce an optimized version of the code, which is suitable for deployment to a production environment.</span></span> <span data-ttu-id="895a7-292">Modifiez la variable *BuildConfiguration* sous l’onglet **variables** de la définition de build si, par exemple, une configuration Debug est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="895a7-292">Modify the *BuildConfiguration* variable on the build definition's **Variables** tab if, for example, a debug configuration is needed.</span></span>
1. <span data-ttu-id="895a7-293">**Test** &mdash; Exécute la `dotnet test --configuration release --logger trx --results-directory <local_path_on_build_agent>` commande pour exécuter les tests unitaires de l’application.</span><span class="sxs-lookup"><span data-stu-id="895a7-293">**Test** &mdash; Executes the `dotnet test --configuration release --logger trx --results-directory <local_path_on_build_agent>` command to run the app's unit tests.</span></span> <span data-ttu-id="895a7-294">Les tests unitaires sont exécutés dans n’importe quel projet C# correspondant au `**/*Tests/*.csproj` modèle glob.</span><span class="sxs-lookup"><span data-stu-id="895a7-294">Unit tests are executed within any C# project matching the `**/*Tests/*.csproj` glob pattern.</span></span> <span data-ttu-id="895a7-295">Les résultats des tests sont enregistrés dans un fichier *. trx* à l’emplacement spécifié par l' `--results-directory` option.</span><span class="sxs-lookup"><span data-stu-id="895a7-295">Test results are saved in a *.trx* file at the location specified by the `--results-directory` option.</span></span> <span data-ttu-id="895a7-296">Si des tests échouent, la génération échoue et n’est pas déployée.</span><span class="sxs-lookup"><span data-stu-id="895a7-296">If any tests fail, the build fails and isn't deployed.</span></span>

    > [!NOTE]
    > <span data-ttu-id="895a7-297">Pour vérifier le fonctionnement des tests unitaires, modifiez *SimpleFeedReader. Tests\Services\NewsServiceTests.cs* afin de rompre intentionnellement l’un des tests.</span><span class="sxs-lookup"><span data-stu-id="895a7-297">To verify the unit tests work, modify *SimpleFeedReader.Tests\Services\NewsServiceTests.cs* to purposefully break one of the tests.</span></span> <span data-ttu-id="895a7-298">Par exemple, remplacez `Assert.True(result.Count > 0);` par `Assert.False(result.Count > 0);` dans la `Returns_News_Stories_Given_Valid_Uri` méthode.</span><span class="sxs-lookup"><span data-stu-id="895a7-298">For example, change `Assert.True(result.Count > 0);` to `Assert.False(result.Count > 0);` in the `Returns_News_Stories_Given_Valid_Uri` method.</span></span> <span data-ttu-id="895a7-299">Valider et transmettre la modification à GitHub.</span><span class="sxs-lookup"><span data-stu-id="895a7-299">Commit and push the change to GitHub.</span></span> <span data-ttu-id="895a7-300">La génération est déclenchée et échoue.</span><span class="sxs-lookup"><span data-stu-id="895a7-300">The build is triggered and fails.</span></span> <span data-ttu-id="895a7-301">L’état du pipeline de build passe à **échec** .</span><span class="sxs-lookup"><span data-stu-id="895a7-301">The build pipeline status changes to **failed** .</span></span> <span data-ttu-id="895a7-302">Rétablissez la modification, la validation et la transmission de type push.</span><span class="sxs-lookup"><span data-stu-id="895a7-302">Revert the change, commit, and push again.</span></span> <span data-ttu-id="895a7-303">La génération a échoué.</span><span class="sxs-lookup"><span data-stu-id="895a7-303">The build succeeds.</span></span>

1. <span data-ttu-id="895a7-304">**Publication** &mdash; Exécute la `dotnet publish --configuration release --output <local_path_on_build_agent>` commande pour générer un fichier *. zip* avec les artefacts à déployer.</span><span class="sxs-lookup"><span data-stu-id="895a7-304">**Publish** &mdash; Executes the `dotnet publish --configuration release --output <local_path_on_build_agent>` command to produce a *.zip* file with the artifacts to be deployed.</span></span> <span data-ttu-id="895a7-305">L' `--output` option spécifie l’emplacement de publication du fichier *. zip* .</span><span class="sxs-lookup"><span data-stu-id="895a7-305">The `--output` option specifies the publish location of the *.zip* file.</span></span> <span data-ttu-id="895a7-306">Cet emplacement est spécifié en passant une [variable prédéfinie](/azure/devops/pipelines/build/variables) nommée `$(build.artifactstagingdirectory)` .</span><span class="sxs-lookup"><span data-stu-id="895a7-306">That location is specified by passing a [predefined variable](/azure/devops/pipelines/build/variables) named `$(build.artifactstagingdirectory)`.</span></span> <span data-ttu-id="895a7-307">Cette variable se développe en un chemin d’accès local, tel que *c:\Agent \_ work\1\a* , sur l’agent de Build.</span><span class="sxs-lookup"><span data-stu-id="895a7-307">That variable expands to a local path, such as *c:\agent\_work\1\a* , on the build agent.</span></span>
1. <span data-ttu-id="895a7-308">**Publier l’artefact** &mdash; Publie le fichier *. zip* produit par la tâche de **publication** .</span><span class="sxs-lookup"><span data-stu-id="895a7-308">**Publish Artifact** &mdash; Publishes the *.zip* file produced by the **Publish** task.</span></span> <span data-ttu-id="895a7-309">La tâche accepte l’emplacement du fichier *. zip* en tant que paramètre, qui est la variable prédéfinie `$(build.artifactstagingdirectory)` .</span><span class="sxs-lookup"><span data-stu-id="895a7-309">The task accepts the *.zip* file location as a parameter, which is the predefined variable `$(build.artifactstagingdirectory)`.</span></span> <span data-ttu-id="895a7-310">Le fichier *. zip* est publié sous la forme d’un dossier nommé *Drop* .</span><span class="sxs-lookup"><span data-stu-id="895a7-310">The *.zip* file is published as a folder named *drop* .</span></span>

<span data-ttu-id="895a7-311">Cliquez sur le lien **Résumé** de la définition de build pour afficher un historique des builds avec la définition :</span><span class="sxs-lookup"><span data-stu-id="895a7-311">Click the build definition's **Summary** link to view a history of builds with the definition:</span></span>

![Capture d’écran montrant l’historique de définition de build](media/cicd/build-definition-summary.png)

<span data-ttu-id="895a7-313">Sur la page résultante, cliquez sur le lien correspondant au numéro de build unique :</span><span class="sxs-lookup"><span data-stu-id="895a7-313">On the resulting page, click the link corresponding to the unique build number:</span></span>

![Capture d’écran montrant la page de résumé de la définition de build](media/cicd/build-definition-completed.png)

<span data-ttu-id="895a7-315">Un résumé de cette build spécifique s’affiche.</span><span class="sxs-lookup"><span data-stu-id="895a7-315">A summary of this specific build is displayed.</span></span> <span data-ttu-id="895a7-316">Cliquez sur l’onglet **artefacts** , et notez que le dossier de *dépôt* produit par la build est listé :</span><span class="sxs-lookup"><span data-stu-id="895a7-316">Click the **Artifacts** tab, and notice the *drop* folder produced by the build is listed:</span></span>

![Capture d’écran montrant les artefacts de définition de build-dossier cible](media/cicd/build-definition-artifacts.png)

<span data-ttu-id="895a7-318">Utilisez les liens **Télécharger** et **Explorer** pour inspecter les artefacts publiés.</span><span class="sxs-lookup"><span data-stu-id="895a7-318">Use the **Download** and **Explore** links to inspect the published artifacts.</span></span>

### <a name="release-pipeline"></a><span data-ttu-id="895a7-319">Pipeline de mise en production</span><span class="sxs-lookup"><span data-stu-id="895a7-319">Release pipeline</span></span>

<span data-ttu-id="895a7-320">Un pipeline de version a été créé avec le nom *MyFirstProject-ASP.net Core-CD* :</span><span class="sxs-lookup"><span data-stu-id="895a7-320">A release pipeline was created with the name *MyFirstProject-ASP.NET Core-CD* :</span></span>

![Capture d’écran montrant la présentation du pipeline de version](media/cicd/release-definition-overview.png)

<span data-ttu-id="895a7-322">Les deux principaux composants du pipeline de mise en version sont les **artefacts** et les **environnements** .</span><span class="sxs-lookup"><span data-stu-id="895a7-322">The two major components of the release pipeline are the **Artifacts** and the **Environments** .</span></span> <span data-ttu-id="895a7-323">Le fait de cliquer sur la zone dans la section **artefacts** affiche le panneau suivant :</span><span class="sxs-lookup"><span data-stu-id="895a7-323">Clicking the box in the **Artifacts** section reveals the following panel:</span></span>

![Capture d’écran montrant les artefacts du pipeline de mise en version](media/cicd/release-definition-artifacts.png)

<span data-ttu-id="895a7-325">La valeur **source (définition de Build)** représente la définition de build à laquelle ce pipeline de mise en version est lié.</span><span class="sxs-lookup"><span data-stu-id="895a7-325">The **Source (Build definition)** value represents the build definition to which this release pipeline is linked.</span></span> <span data-ttu-id="895a7-326">Le fichier *. zip* produit par une exécution réussie de la définition de build est fourni à l’environnement de *production* pour le déploiement sur Azure.</span><span class="sxs-lookup"><span data-stu-id="895a7-326">The *.zip* file produced by a successful run of the build definition is provided to the *Production* environment for deployment to Azure.</span></span> <span data-ttu-id="895a7-327">Cliquez sur le lien *1 phase, 2 tâches* dans la zone environnement de production pour afficher les tâches de pipeline de mise en *production* :</span><span class="sxs-lookup"><span data-stu-id="895a7-327">Click the *1 phase, 2 tasks* link in the *Production* environment box to view the release pipeline tasks:</span></span>

![Capture d’écran montrant les tâches de pipeline de version](media/cicd/release-definition-tasks.png)

<span data-ttu-id="895a7-329">Le pipeline de mise en version est constitué de deux tâches : *déployer des Azure App service sur l’emplacement* et gérer l' *échange d’emplacement Azure App service* .</span><span class="sxs-lookup"><span data-stu-id="895a7-329">The release pipeline consists of two tasks: *Deploy Azure App Service to Slot* and *Manage Azure App Service - Slot Swap* .</span></span> <span data-ttu-id="895a7-330">Le fait de cliquer sur la première tâche révèle la configuration de tâche suivante :</span><span class="sxs-lookup"><span data-stu-id="895a7-330">Clicking the first task reveals the following task configuration:</span></span>

![Capture d’écran montrant la tâche de déploiement du pipeline de version](media/cicd/release-definition-task1.png)

<span data-ttu-id="895a7-332">L’abonnement Azure, le type de service, le nom de l’application Web, le groupe de ressources et l’emplacement de déploiement sont définis dans la tâche de déploiement.</span><span class="sxs-lookup"><span data-stu-id="895a7-332">The Azure subscription, service type, web app name, resource group, and deployment slot are defined in the deployment task.</span></span> <span data-ttu-id="895a7-333">La zone de texte **package ou dossier** contient le chemin d’accès du fichier *. zip* à extraire et à déployer dans l’emplacement *intermédiaire* de l’application Web *myWebApp \<unique_number\>* .</span><span class="sxs-lookup"><span data-stu-id="895a7-333">The **Package or folder** textbox holds the *.zip* file path to be extracted and deployed to the *staging* slot of the *mywebapp\<unique_number\>* web app.</span></span>

<span data-ttu-id="895a7-334">Le fait de cliquer sur la tâche d’échange d’emplacement révèle la configuration de tâche suivante :</span><span class="sxs-lookup"><span data-stu-id="895a7-334">Clicking the slot swap task reveals the following task configuration:</span></span>

![Capture d’écran montrant la tâche d’échange d’emplacement de pipeline de version](media/cicd/release-definition-task2.png)

<span data-ttu-id="895a7-336">Les détails de l’abonnement, du groupe de ressources, du type de service, du nom de l’application Web et de l’emplacement de déploiement sont fournis.</span><span class="sxs-lookup"><span data-stu-id="895a7-336">The subscription, resource group, service type, web app name, and deployment slot details are provided.</span></span> <span data-ttu-id="895a7-337">La case à cocher **swap with production** est activée.</span><span class="sxs-lookup"><span data-stu-id="895a7-337">The **Swap with Production** check box is checked.</span></span> <span data-ttu-id="895a7-338">Par conséquent, les bits déployés sur l’emplacement *intermédiaire* sont échangés dans l’environnement de production.</span><span class="sxs-lookup"><span data-stu-id="895a7-338">Consequently, the bits deployed to the *staging* slot are swapped into the production environment.</span></span>

## <a name="additional-reading"></a><span data-ttu-id="895a7-339">Documentation supplémentaire</span><span class="sxs-lookup"><span data-stu-id="895a7-339">Additional reading</span></span>

* [<span data-ttu-id="895a7-340">Créer son premier pipeline avec Azure Pipelines</span><span class="sxs-lookup"><span data-stu-id="895a7-340">Create your first pipeline with Azure Pipelines</span></span>](/azure/devops/pipelines/get-started-yaml)
* [<span data-ttu-id="895a7-341">Build et projet .NET Core</span><span class="sxs-lookup"><span data-stu-id="895a7-341">Build and .NET Core project</span></span>](/azure/devops/pipelines/languages/dotnet-core)
* [<span data-ttu-id="895a7-342">Déployer une application Web avec Azure Pipelines</span><span class="sxs-lookup"><span data-stu-id="895a7-342">Deploy a web app with Azure Pipelines</span></span>](/azure/devops/pipelines/targets/webapp)
