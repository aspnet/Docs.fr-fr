---
title: Didacticiel, partie 5, appliquer des migrations à l’exemple Contoso University
description: Partie 5 de la série de didacticiels sur Contoso University. Utilisez la fonctionnalité migrations EF Core pour gérer les modifications de modèle de données dans une application ASP.NET Core MVC.
author: rick-anderson
ms.author: riande
ms.custom: contperf-fy21q2
ms.date: 11/13/2020
ms.topic: tutorial
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
uid: data/ef-mvc/migrations
ms.openlocfilehash: aebbc3f29b0356c7993abd83869ab21d3613bf61
ms.sourcegitcommit: 54fe1ae5e7d068e27376d562183ef9ddc7afc432
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/10/2021
ms.locfileid: "102589346"
---
# <a name="tutorial-part-5-apply-migrations-to-the-contoso-university-sample"></a><span data-ttu-id="81cc3-104">Didacticiel : partie 5, appliquer des migrations à l’exemple Contoso University</span><span class="sxs-lookup"><span data-stu-id="81cc3-104">Tutorial: Part 5, apply migrations to the Contoso University sample</span></span>

<span data-ttu-id="81cc3-105">Dans ce didacticiel, vous utilisez la fonctionnalité Migrations d’EF Core pour gérer les modifications du modèle de données.</span><span class="sxs-lookup"><span data-stu-id="81cc3-105">In this tutorial, you start using the EF Core migrations feature for managing data model changes.</span></span> <span data-ttu-id="81cc3-106">Dans les didacticiels suivants, vous allez ajouter d’autres migrations au fil de la modification du modèle de données.</span><span class="sxs-lookup"><span data-stu-id="81cc3-106">In later tutorials, you'll add more migrations as you change the data model.</span></span>

<span data-ttu-id="81cc3-107">Dans ce tutoriel, vous allez :</span><span class="sxs-lookup"><span data-stu-id="81cc3-107">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="81cc3-108">En savoir plus sur les migrations</span><span class="sxs-lookup"><span data-stu-id="81cc3-108">Learn about migrations</span></span>
> * <span data-ttu-id="81cc3-109">Créer une migration initiale</span><span class="sxs-lookup"><span data-stu-id="81cc3-109">Create an initial migration</span></span>
> * <span data-ttu-id="81cc3-110">Examiner les méthodes Up et Down</span><span class="sxs-lookup"><span data-stu-id="81cc3-110">Examine Up and Down methods</span></span>
> * <span data-ttu-id="81cc3-111">En savoir plus sur la capture instantanée du modèle de données</span><span class="sxs-lookup"><span data-stu-id="81cc3-111">Learn about the data model snapshot</span></span>
> * <span data-ttu-id="81cc3-112">Appliquer la migration</span><span class="sxs-lookup"><span data-stu-id="81cc3-112">Apply the migration</span></span>

## <a name="prerequisites"></a><span data-ttu-id="81cc3-113">Prérequis</span><span class="sxs-lookup"><span data-stu-id="81cc3-113">Prerequisites</span></span>

* [<span data-ttu-id="81cc3-114">Tri, filtrage et pagination</span><span class="sxs-lookup"><span data-stu-id="81cc3-114">Sorting, filtering, and paging</span></span>](sort-filter-page.md)

## <a name="about-migrations"></a><span data-ttu-id="81cc3-115">À propos des migrations</span><span class="sxs-lookup"><span data-stu-id="81cc3-115">About migrations</span></span>

<span data-ttu-id="81cc3-116">Quand vous développez une nouvelle application, votre modèle de données change fréquemment et, chaque fois que le modèle change, il n’est plus en synchronisation avec la base de données.</span><span class="sxs-lookup"><span data-stu-id="81cc3-116">When you develop a new application, your data model changes frequently, and each time the model changes, it gets out of sync with the database.</span></span> <span data-ttu-id="81cc3-117">Vous avez démarré ce didacticiel en configurant Entity Framework pour créer la base de données si elle n’existait pas.</span><span class="sxs-lookup"><span data-stu-id="81cc3-117">You started these tutorials by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="81cc3-118">Ensuite, chaque fois que vous modifiez le modèle de données, en ajoutant, supprimant ou changeant des classes d’entité ou votre classe DbContext, vous pouvez supprimer la base de données : EF en crée alors une nouvelle qui correspond au modèle et l’alimente avec des données de test.</span><span class="sxs-lookup"><span data-stu-id="81cc3-118">Then each time you change the data model -- add, remove, or change entity classes or change your DbContext class -- you can delete the database and EF creates a new one that matches the model, and seeds it with test data.</span></span>

<span data-ttu-id="81cc3-119">Cette méthode pour conserver la base de données en synchronisation avec le modèle de données fonctionne bien jusqu’au déploiement de l’application en production.</span><span class="sxs-lookup"><span data-stu-id="81cc3-119">This method of keeping the database in sync with the data model works well until you deploy the application to production.</span></span> <span data-ttu-id="81cc3-120">Quand l’application s’exécute en production, elle stocke généralement les données que vous voulez conserver, et vous ne voulez pas tout perdre chaque fois que vous apportez une modification, comme ajouter une nouvelle colonne.</span><span class="sxs-lookup"><span data-stu-id="81cc3-120">When the application is running in production it's usually storing data that you want to keep, and you don't want to lose everything each time you make a change such as adding a new column.</span></span> <span data-ttu-id="81cc3-121">La fonctionnalité Migrations d’EF Core résout ce problème en permettant à EF de mettre à jour le schéma de base de données au lieu de créer une nouvelle base de données.</span><span class="sxs-lookup"><span data-stu-id="81cc3-121">The EF Core Migrations feature solves this problem by enabling EF to update the database schema instead of creating  a new database.</span></span>

<span data-ttu-id="81cc3-122">Pour travailler avec des migrations, vous pouvez utiliser la **console du gestionnaire de package** (PMC) ou l’interface CLI.</span><span class="sxs-lookup"><span data-stu-id="81cc3-122">To work with migrations, you can use the **Package Manager Console** (PMC) or the CLI.</span></span>  <span data-ttu-id="81cc3-123">Ces didacticiels montrent comment utiliser des commandes CLI.</span><span class="sxs-lookup"><span data-stu-id="81cc3-123">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="81cc3-124">Vous trouverez des informations sur la console du Gestionnaire de package [à la fin de ce didacticiel](#pmc).</span><span class="sxs-lookup"><span data-stu-id="81cc3-124">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

## <a name="drop-the-database"></a><span data-ttu-id="81cc3-125">Supprimer la base de données</span><span class="sxs-lookup"><span data-stu-id="81cc3-125">Drop the database</span></span>

<span data-ttu-id="81cc3-126">Installez EF Core Tools en tant qu' [outil Global](/ef/core/miscellaneous/cli/dotnet) et supprimez la base de données :</span><span class="sxs-lookup"><span data-stu-id="81cc3-126">Install EF Core tools as a [global tool](/ef/core/miscellaneous/cli/dotnet) and delete the database:</span></span>

 ```dotnetcli
 dotnet tool install --global dotnet-ef
 dotnet ef database drop
 ```

<span data-ttu-id="81cc3-127">La section suivante explique comment exécuter des commandes CLI.</span><span class="sxs-lookup"><span data-stu-id="81cc3-127">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="81cc3-128">Créer une migration initiale</span><span class="sxs-lookup"><span data-stu-id="81cc3-128">Create an initial migration</span></span>

<span data-ttu-id="81cc3-129">Enregistrez vos modifications et générez le projet.</span><span class="sxs-lookup"><span data-stu-id="81cc3-129">Save your changes and build the project.</span></span> <span data-ttu-id="81cc3-130">Ouvrez ensuite une fenêtre Commande et accédez au dossier du projet.</span><span class="sxs-lookup"><span data-stu-id="81cc3-130">Then open a command window and navigate to the project folder.</span></span> <span data-ttu-id="81cc3-131">Voici un moyen rapide pour le faire :</span><span class="sxs-lookup"><span data-stu-id="81cc3-131">Here's a quick way to do that:</span></span>

* <span data-ttu-id="81cc3-132">Dans **l’Explorateur de solutions**, cliquez sur le projet et choisissez **Ouvrir le dossier dans l’Explorateur de fichiers** dans le menu contextuel.</span><span class="sxs-lookup"><span data-stu-id="81cc3-132">In **Solution Explorer**, right-click the project and choose **Open Folder in File Explorer** from the context menu.</span></span>

  ![Élément de menu Ouvrir dans l’Explorateur de fichiers](migrations/_static/open-in-file-explorer.png)

* <span data-ttu-id="81cc3-134">Entrez « cmd » dans la barre d’adresses et appuyez sur Entrée.</span><span class="sxs-lookup"><span data-stu-id="81cc3-134">Enter "cmd" in the address bar and press Enter.</span></span>

  ![Ouvrir une fenêtre Commande](migrations/_static/open-command-window.png)

<span data-ttu-id="81cc3-136">Entrez la commande suivante dans la fenêtre Commande :</span><span class="sxs-lookup"><span data-stu-id="81cc3-136">Enter the following command in the command window:</span></span>

```dotnetcli
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="81cc3-137">Dans les commandes précédentes, une sortie similaire à ce qui suit s’affiche :</span><span class="sxs-lookup"><span data-stu-id="81cc3-137">In the preceding commands, output similar to the following is displayed:</span></span>

```console
info: Microsoft.EntityFrameworkCore.Infrastructure[10403]
      Entity Framework Core initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="81cc3-138">Si vous voyez un message d’erreur «*Impossible d’accéder au fichier... ContosoUniversity.dll parce qu’il est utilisé par un autre processus.*», recherchez l’icône de IIS Express dans la barre d’état système Windows, cliquez dessus avec le bouton droit, puis cliquez sur **ContosoUniversity > arrêter le site**.</span><span class="sxs-lookup"><span data-stu-id="81cc3-138">If you see an error message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*", find the IIS Express icon in the Windows System Tray, and right-click it, then click **ContosoUniversity > Stop Site**.</span></span>

## <a name="examine-up-and-down-methods"></a><span data-ttu-id="81cc3-139">Examiner les méthodes Up et Down</span><span class="sxs-lookup"><span data-stu-id="81cc3-139">Examine Up and Down methods</span></span>

<span data-ttu-id="81cc3-140">Quand vous avez exécuté la commande `migrations add`, EF a généré le code qui crée la base de données à partir de zéro.</span><span class="sxs-lookup"><span data-stu-id="81cc3-140">When you executed the `migrations add` command, EF generated the code that will create the database from scratch.</span></span> <span data-ttu-id="81cc3-141">Ce code se trouve dans le dossier *migrations* , dans le fichier nommé *\<timestamp> _InitialCreate. cs*.</span><span class="sxs-lookup"><span data-stu-id="81cc3-141">This code is in the *Migrations* folder, in the file named *\<timestamp>_InitialCreate.cs*.</span></span> <span data-ttu-id="81cc3-142">La méthode `Up` de la classe `InitialCreate` crée les tables de base de données qui correspondent aux jeux d’entités du modèle de données, et la méthode `Down` les supprime, comme indiqué dans l’exemple suivant.</span><span class="sxs-lookup"><span data-stu-id="81cc3-142">The `Up` method of the `InitialCreate` class creates the database tables that correspond to the data model entity sets, and the `Down` method deletes them, as shown in the following example.</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

<span data-ttu-id="81cc3-143">La fonctionnalité Migrations appelle la méthode `Up` pour implémenter les modifications du modèle de données pour une migration.</span><span class="sxs-lookup"><span data-stu-id="81cc3-143">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="81cc3-144">Quand vous entrez une commande pour annuler la mise à jour, Migrations appelle la méthode `Down`.</span><span class="sxs-lookup"><span data-stu-id="81cc3-144">When you enter a command to roll back the update, Migrations calls the `Down` method.</span></span>

<span data-ttu-id="81cc3-145">Ce code est celui de la migration initiale qui a été créé quand vous avez entré la commande `migrations add InitialCreate`.</span><span class="sxs-lookup"><span data-stu-id="81cc3-145">This code is for the initial migration that was created when you entered the `migrations add InitialCreate` command.</span></span> <span data-ttu-id="81cc3-146">Le paramètre de nom de la migration (« InitialCreate » dans l’exemple) est utilisé comme nom de fichier ; vous pouvez le choisir librement.</span><span class="sxs-lookup"><span data-stu-id="81cc3-146">The migration name parameter ("InitialCreate" in the example) is used for the file name and can be whatever you want.</span></span> <span data-ttu-id="81cc3-147">Nous vous conseillons néanmoins de choisir un mot ou une expression qui résume ce qui est effectué dans la migration.</span><span class="sxs-lookup"><span data-stu-id="81cc3-147">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="81cc3-148">Par exemple, vous pouvez nommer une migration ultérieure « AjouterTableDépartement ».</span><span class="sxs-lookup"><span data-stu-id="81cc3-148">For example, you might name a later migration "AddDepartmentTable".</span></span>

<span data-ttu-id="81cc3-149">Si vous avez créé la migration initiale alors que la base de données existait déjà, le code de création de la base de données est généré, mais il n’est pas nécessaire de l’exécuter, car la base de données correspond déjà au modèle de données.</span><span class="sxs-lookup"><span data-stu-id="81cc3-149">If you created the initial migration when the database already exists, the database creation code is generated but it doesn't have to run because the database already matches the data model.</span></span> <span data-ttu-id="81cc3-150">Quand vous déployez l’application sur un autre environnement où la base de données n’existe pas encore, ce code est exécuté pour créer votre base de données : il est donc judicieux de le tester au préalable.</span><span class="sxs-lookup"><span data-stu-id="81cc3-150">When you deploy the app to another environment where the database doesn't exist yet, this code will run to create your database, so it's a good idea to test it first.</span></span> <span data-ttu-id="81cc3-151">C’est la raison pour laquelle vous avez précédemment changé le nom de la base de données dans la chaîne de connexion : les migrations doivent pouvoir créer une base de données à partir de zéro.</span><span class="sxs-lookup"><span data-stu-id="81cc3-151">That's why you changed the name of the database in the connection string earlier -- so that migrations can create a new one from scratch.</span></span>

## <a name="the-data-model-snapshot"></a><span data-ttu-id="81cc3-152">Capture instantanée du modèle de données</span><span class="sxs-lookup"><span data-stu-id="81cc3-152">The data model snapshot</span></span>

<span data-ttu-id="81cc3-153">Migrations crée une *capture instantanée* du schéma de base de données actuel dans *Migrations/SchoolContextModelSnapshot.cs*.</span><span class="sxs-lookup"><span data-stu-id="81cc3-153">Migrations creates a *snapshot* of the current database schema in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="81cc3-154">Quand vous ajoutez une migration, EF détermine ce qui a changé en comparant le modèle de données au fichier de capture instantanée.</span><span class="sxs-lookup"><span data-stu-id="81cc3-154">When you add a migration, EF determines what changed by comparing the data model to the snapshot file.</span></span>

<span data-ttu-id="81cc3-155">Utilisez la commande [dotnet EF migrations Remove](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) pour supprimer une migration.</span><span class="sxs-lookup"><span data-stu-id="81cc3-155">Use the [dotnet ef migrations remove](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command to remove a migration.</span></span> <span data-ttu-id="81cc3-156">`dotnet ef migrations remove` supprime la migration et garantit que la capture instantanée est correctement réinitialisée.</span><span class="sxs-lookup"><span data-stu-id="81cc3-156">`dotnet ef migrations remove` deletes the migration and ensures the snapshot is correctly reset.</span></span> <span data-ttu-id="81cc3-157">En cas `dotnet ef migrations remove` d’échec, utilisez `dotnet ef migrations remove -v` pour obtenir plus d’informations sur l’échec.</span><span class="sxs-lookup"><span data-stu-id="81cc3-157">If `dotnet ef migrations remove` fails, use `dotnet ef migrations remove -v` to get more information on the failure.</span></span>

<span data-ttu-id="81cc3-158">Pour plus d’informations sur l’utilisation du fichier de capture instantanée, consultez [Migrations EF Core dans les environnements d’équipe](/ef/core/managing-schemas/migrations/teams).</span><span class="sxs-lookup"><span data-stu-id="81cc3-158">See [EF Core Migrations in Team Environments](/ef/core/managing-schemas/migrations/teams) for more information about how the snapshot file is used.</span></span>

## <a name="apply-the-migration"></a><span data-ttu-id="81cc3-159">Appliquer la migration</span><span class="sxs-lookup"><span data-stu-id="81cc3-159">Apply the migration</span></span>

<span data-ttu-id="81cc3-160">Dans la fenêtre Commande, entrez la commande suivante pour créer la base de données et ses tables.</span><span class="sxs-lookup"><span data-stu-id="81cc3-160">In the command window, enter the following command to create the database and tables in it.</span></span>

```dotnetcli
dotnet ef database update
```

<span data-ttu-id="81cc3-161">La sortie de la commande est similaire à la commande `migrations add`, à ceci près que vous voyez des journaux pour les commandes SQL qui configurent la base de données.</span><span class="sxs-lookup"><span data-stu-id="81cc3-161">The output from the command is similar to the `migrations add` command, except that you see logs for the SQL commands that set up the database.</span></span> <span data-ttu-id="81cc3-162">La plupart des journaux sont omis dans l’exemple de sortie suivant.</span><span class="sxs-lookup"><span data-stu-id="81cc3-162">Most of the logs are omitted in the following sample output.</span></span> <span data-ttu-id="81cc3-163">Si vous préférez ne pas voir ce niveau de détail dans les messages des journaux, vous pouvez changer le niveau de journalisation dans le fichier *appsettings.Development.json*.</span><span class="sxs-lookup"><span data-stu-id="81cc3-163">If you prefer not to see this level of detail in log messages, you can change the log level in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="81cc3-164">Pour plus d’informations, consultez <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="81cc3-164">For more information, see <xref:fundamentals/logging/index>.</span></span>

```text
info: Microsoft.EntityFrameworkCore.Infrastructure[10403]
      Entity Framework Core initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
info: Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (274ms) [Parameters=[], CommandType='Text', CommandTimeout='60']
      CREATE DATABASE [ContosoUniversity2];
info: Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (60ms) [Parameters=[], CommandType='Text', CommandTimeout='60']
      IF SERVERPROPERTY('EngineEdition') <> 5
      BEGIN
          ALTER DATABASE [ContosoUniversity2] SET READ_COMMITTED_SNAPSHOT ON;
      END;
info: Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (15ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE [__EFMigrationsHistory] (
          [MigrationId] nvarchar(150) NOT NULL,
          [ProductVersion] nvarchar(32) NOT NULL,
          CONSTRAINT [PK___EFMigrationsHistory] PRIMARY KEY ([MigrationId])
      );

<logs omitted for brevity>

info: Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (3ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      INSERT INTO [__EFMigrationsHistory] ([MigrationId], [ProductVersion])
      VALUES (N'20190327172701_InitialCreate', N'5.0-rtm');
Done.
```

<span data-ttu-id="81cc3-165">Utilisez **l’Explorateur d’objets SQL Server** pour inspecter la base de données, comme vous l’avez fait dans le premier didacticiel.</span><span class="sxs-lookup"><span data-stu-id="81cc3-165">Use **SQL Server Object Explorer** to inspect the database as you did in the first tutorial.</span></span>  <span data-ttu-id="81cc3-166">Vous pouvez noter l’ajout d’une table \_\_EFMigrationsHistory, qui fait le suivi des migrations qui ont été appliquées à la base de données.</span><span class="sxs-lookup"><span data-stu-id="81cc3-166">You'll notice the addition of an \_\_EFMigrationsHistory table that keeps track of which migrations have been applied to the database.</span></span> <span data-ttu-id="81cc3-167">Visualisez les données de cette table : vous y voyez une ligne pour la première migration.</span><span class="sxs-lookup"><span data-stu-id="81cc3-167">View the data in that table and you'll see one row for the first migration.</span></span> <span data-ttu-id="81cc3-168">(Le dernier journal dans l’exemple de sortie CLI précédent montre l’instruction INSERT qui crée cette ligne.)</span><span class="sxs-lookup"><span data-stu-id="81cc3-168">(The last log in the preceding CLI output example shows the INSERT statement that creates this row.)</span></span>

<span data-ttu-id="81cc3-169">Exécutez l’application pour vérifier que tout fonctionne toujours comme avant.</span><span class="sxs-lookup"><span data-stu-id="81cc3-169">Run the application to verify that everything still works the same as before.</span></span>

![Page d’index des étudiants](migrations/_static/students-index.png)

<a id="pmc"></a>

## <a name="compare-cli-and-pmc"></a><span data-ttu-id="81cc3-171">Comparer l’interface CLI et PMC</span><span class="sxs-lookup"><span data-stu-id="81cc3-171">Compare CLI and PMC</span></span>

<span data-ttu-id="81cc3-172">Les outils EF pour la gestion des migrations sont disponibles à partir de commandes CLI .NET Core ou d’applets de commande PowerShell dans la fenêtre **Console du Gestionnaire de package** de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="81cc3-172">The EF tooling for managing migrations is available from .NET Core CLI commands or from PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="81cc3-173">Ce didacticiel montre comment utiliser l’interface CLI, mais vous pouvez utiliser la console du Gestionnaire de package si vous préférez.</span><span class="sxs-lookup"><span data-stu-id="81cc3-173">This tutorial shows how to use the CLI, but you can use the PMC if you prefer.</span></span>

<span data-ttu-id="81cc3-174">Les commandes EF pour la console du Gestionnaire de package se trouvent dans le package [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools).</span><span class="sxs-lookup"><span data-stu-id="81cc3-174">The EF commands for the PMC commands are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="81cc3-175">Ce package étant inclus dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), vous n’avez pas besoin d’ajouter une référence de package si votre application en comporte une pour `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="81cc3-175">This package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), so you don't need to add a package reference if your app has a package reference for `Microsoft.AspNetCore.App`.</span></span>

<span data-ttu-id="81cc3-176">**Important :** Il ne s’agit pas du même package que celui que vous installez pour l’interface CLI en modifiant le fichier *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="81cc3-176">**Important:** This isn't the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="81cc3-177">Le nom de celui-ci se termine par `Tools`, contrairement au nom du package CLI qui se termine par `Tools.DotNet`.</span><span class="sxs-lookup"><span data-stu-id="81cc3-177">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="81cc3-178">Pour plus d’informations sur les commandes CLI, consultez [Interface CLI .NET Core](/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="81cc3-178">For more information about the CLI commands, see [.NET Core CLI](/ef/core/miscellaneous/cli/dotnet).</span></span>

<span data-ttu-id="81cc3-179">Pour plus d’informations sur les commandes de la console du Gestionnaire de package, consultez [Console du Gestionnaire de package (Visual Studio)](/ef/core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="81cc3-179">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="get-the-code"></a><span data-ttu-id="81cc3-180">Obtenir le code</span><span class="sxs-lookup"><span data-stu-id="81cc3-180">Get the code</span></span>

[<span data-ttu-id="81cc3-181">Télécharger ou afficher l’application complète.</span><span class="sxs-lookup"><span data-stu-id="81cc3-181">Download or view the completed application.</span></span>](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/data/ef-mvc/intro/samples)

## <a name="next-step"></a><span data-ttu-id="81cc3-182">Étape suivante</span><span class="sxs-lookup"><span data-stu-id="81cc3-182">Next step</span></span>

<span data-ttu-id="81cc3-183">Passez au tutoriel suivant pour aborder des sujets plus avancés sur le développement du modèle de données.</span><span class="sxs-lookup"><span data-stu-id="81cc3-183">Advance to the next tutorial to begin looking at more advanced topics about expanding the data model.</span></span> <span data-ttu-id="81cc3-184">Au cour de ce processus, vous allez créer et appliquer d’autres migrations.</span><span class="sxs-lookup"><span data-stu-id="81cc3-184">Along the way you'll create and apply additional migrations.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="81cc3-185">Créer et appliquer d’autres migrations</span><span class="sxs-lookup"><span data-stu-id="81cc3-185">Create and apply additional migrations</span></span>](complex-data-model.md)
