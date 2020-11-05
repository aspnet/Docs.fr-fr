---
title: ':::no-loc(Razor)::: Pages avec Entity Framework Core dans ASP.NET Core-didacticiel 1 sur 8'
author: rick-anderson
description: 'Montre comment créer une :::no-loc(Razor)::: application pages à l’aide de Entity Framework Core'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 9/26/2020
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
uid: data/ef-rp/intro
ms.openlocfilehash: 74f65b916c2d5b7de61ec29f4259a51584ee5989
ms.sourcegitcommit: 33f631a4427b9a422755601ac9119953db0b4a3e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2020
ms.locfileid: "93365416"
---
# <a name="no-locrazor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a><span data-ttu-id="a3fde-103">:::no-loc(Razor)::: Pages avec Entity Framework Core dans ASP.NET Core-didacticiel 1 sur 8</span><span class="sxs-lookup"><span data-stu-id="a3fde-103">:::no-loc(Razor)::: Pages with Entity Framework Core in ASP.NET Core - Tutorial 1 of 8</span></span>

<span data-ttu-id="a3fde-104">Par [Tom Dykstra](https://github.com/tdykstra) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a3fde-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-5.0"

<span data-ttu-id="a3fde-105">Il s’agit de la première d’une série de didacticiels qui montrent comment utiliser Entity Framework (EF) Core dans une application [ASP.net Core :::no-loc(Razor)::: pages](xref:razor-pages/index) .</span><span class="sxs-lookup"><span data-stu-id="a3fde-105">This is the first in a series of tutorials that show how to use Entity Framework (EF) Core in an [ASP.NET Core :::no-loc(Razor)::: Pages](xref:razor-pages/index) app.</span></span> <span data-ttu-id="a3fde-106">Dans ces tutoriels, un site web est créé pour une université fictive nommée Contoso.</span><span class="sxs-lookup"><span data-stu-id="a3fde-106">The tutorials build a web site for a fictional Contoso University.</span></span> <span data-ttu-id="a3fde-107">Le site comprend des fonctionnalités comme l’admission des étudiants, la création de cours et les affectations des formateurs.</span><span class="sxs-lookup"><span data-stu-id="a3fde-107">The site includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="a3fde-108">Le didacticiel utilise l’approche code First.</span><span class="sxs-lookup"><span data-stu-id="a3fde-108">The tutorial uses the code first approach.</span></span> <span data-ttu-id="a3fde-109">Pour plus d’informations sur ce didacticiel avec la première approche basée sur la base de données, consultez [ce problème GitHub](https://github.com/dotnet/AspNetCore.Docs/issues/16897).</span><span class="sxs-lookup"><span data-stu-id="a3fde-109">For information on following this tutorial using the database first approach, see [this Github issue](https://github.com/dotnet/AspNetCore.Docs/issues/16897).</span></span>

[<span data-ttu-id="a3fde-110">Télécharger ou afficher l’application terminée.</span><span class="sxs-lookup"><span data-stu-id="a3fde-110">Download or view the completed app.</span></span>](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="a3fde-111">[Instructions de téléchargement](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="a3fde-111">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a3fde-112">Prérequis</span><span class="sxs-lookup"><span data-stu-id="a3fde-112">Prerequisites</span></span>

* <span data-ttu-id="a3fde-113">Si vous :::no-loc(Razor)::: débutez avec des pages, consultez la série de didacticiels [prise en main des :::no-loc(Razor)::: pages](xref:tutorials/razor-pages/razor-pages-start) avant de commencer celle-ci.</span><span class="sxs-lookup"><span data-stu-id="a3fde-113">If you're new to :::no-loc(Razor)::: Pages, go through the [Get started with :::no-loc(Razor)::: Pages](xref:tutorials/razor-pages/razor-pages-start) tutorial series before starting this one.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a3fde-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a3fde-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[VS prereqs](~/includes/net-core-prereqs-vs-5.0.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="a3fde-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a3fde-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[VS Code prereqs](~/includes/net-core-prereqs-vsc-5.0.md)]

---

## <a name="database-engines"></a><span data-ttu-id="a3fde-116">Moteurs de base de données</span><span class="sxs-lookup"><span data-stu-id="a3fde-116">Database engines</span></span>

<span data-ttu-id="a3fde-117">Les instructions Visual Studio utilisent la [Base de données locale SQL Server](/sql/database-engine/configure-windows/sql-server-2016-express-localdb), version de SQL Server Express qui s’exécute uniquement sur Windows.</span><span class="sxs-lookup"><span data-stu-id="a3fde-117">The Visual Studio instructions use [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb), a version of SQL Server Express that runs only on Windows.</span></span>

<span data-ttu-id="a3fde-118">Les instructions Visual Studio Code utilisent [SQLite](https://www.sqlite.org/), moteur de base de données multiplateforme.</span><span class="sxs-lookup"><span data-stu-id="a3fde-118">The Visual Studio Code instructions use [SQLite](https://www.sqlite.org/), a cross-platform database engine.</span></span>

<span data-ttu-id="a3fde-119">Si vous choisissez d’utiliser SQLite, téléchargez et installez un outil tiers pour la gestion et l’affichage d’une base de données SQLite, comme [Browser for SQLite](https://sqlitebrowser.org/).</span><span class="sxs-lookup"><span data-stu-id="a3fde-119">If you choose to use SQLite, download and install a third-party tool for managing and viewing a SQLite database, such as [DB Browser for SQLite](https://sqlitebrowser.org/).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="a3fde-120">Résolution des problèmes</span><span class="sxs-lookup"><span data-stu-id="a3fde-120">Troubleshooting</span></span>

<span data-ttu-id="a3fde-121">Si vous rencontrez un problème que vous ne pouvez pas résoudre, comparez votre code au [projet terminé](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="a3fde-121">If you run into a problem you can't resolve, compare your code to the [completed project](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span> <span data-ttu-id="a3fde-122">Un bon moyen d’obtenir de l’aide est de poster une question sur StackOverflow.com en utilisant le [mot-clé ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) ou le [mot-clé EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="a3fde-122">A good way to get help is by posting a question to StackOverflow.com, using the [ASP.NET Core tag](https://stackoverflow.com/questions/tagged/asp.net-core) or the [EF Core tag](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

## <a name="the-sample-app"></a><span data-ttu-id="a3fde-123">Exemple d’application</span><span class="sxs-lookup"><span data-stu-id="a3fde-123">The sample app</span></span>

<span data-ttu-id="a3fde-124">L’application générée dans ces didacticiels est le site web de base d’une université.</span><span class="sxs-lookup"><span data-stu-id="a3fde-124">The app built in these tutorials is a basic university web site.</span></span> <span data-ttu-id="a3fde-125">Les utilisateurs peuvent afficher et mettre à jour les informations relatives aux étudiants, aux cours et aux formateurs.</span><span class="sxs-lookup"><span data-stu-id="a3fde-125">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="a3fde-126">Voici quelques-uns des écrans créés dans le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="a3fde-126">Here are a few of the screens created in the tutorial.</span></span>

![Page d’index des étudiants](intro/_static/students-index30.png)

![Page de modification des étudiants](intro/_static/student-edit30.png)

<span data-ttu-id="a3fde-129">Le style de l’interface utilisateur de ce site repose sur les modèles de projet intégrés.</span><span class="sxs-lookup"><span data-stu-id="a3fde-129">The UI style of this site is based on the built-in project templates.</span></span> <span data-ttu-id="a3fde-130">Ce didacticiel porte sur l’utilisation de EF Core avec ASP.NET Core, et non sur la personnalisation de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="a3fde-130">The tutorial's focus is on how to use EF Core with ASP.NET Core, not how to customize the UI.</span></span>

<!-- 
Follow the link at the top of the page to get the source code for the completed project. The *cu50* folder has the code for the ASP.NET Core 5.0 version of the tutorial. Files that reflect the state of the code for tutorials 1-7 can be found in the *cu50snapshots* folder.

# [Visual Studio](#tab/visual-studio)

To run the app after downloading the completed project:

* Build the project.
* In Package Manager Console (PMC) run the following command:

  ```powershell
  Update-Database
  ```

* Run the project to seed the database.

# [Visual Studio Code](#tab/visual-studio-code)

To run the app after downloading the completed project:

* In *Program.cs*, remove the comments from `// webBuilder.UseStartup<StartupSQLite>();`  so `StartupSQLite` is used.
* Copy the contents of *appSettingsSQLite.json* into *appSettings.json*.
* Delete the *Migrations* folder, and rename *MigrationsSQL* to *Migrations*.
* Do a global search for `#if SQLiteVersion` and remove `#if SQLiteVersion` and the associated `#endif` statement.
* Build the project.
* At a command prompt in the project folder, run the following commands:

  ```dotnetcli
  dotnet tool install --global dotnet-ef -v 5.0.0-*
  dotnet ef database update
  ```

* In your SQLite tool, run the following SQL statement:

  ```sql
  UPDATE Department SET RowVersion = randomblob(8)
  ```

* Run the project to seed the database.

---

-->

## <a name="create-the-web-app-project"></a><span data-ttu-id="a3fde-131">Créer le projet d’application web</span><span class="sxs-lookup"><span data-stu-id="a3fde-131">Create the web app project</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a3fde-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a3fde-132">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a3fde-133">Démarrez Visual Studio et sélectionnez **Créer un projet**.</span><span class="sxs-lookup"><span data-stu-id="a3fde-133">Start Visual Studio and select **Create a new project**.</span></span>
* <span data-ttu-id="a3fde-134">Sélectionnez **ASP.net Core application Web** > **suivant**.</span><span class="sxs-lookup"><span data-stu-id="a3fde-134">Select **ASP.NET Core Web Application** > **NEXT**.</span></span>
* <span data-ttu-id="a3fde-135">Nommez le projet *ContosoUniversity*.</span><span class="sxs-lookup"><span data-stu-id="a3fde-135">Name the project *ContosoUniversity*.</span></span> <span data-ttu-id="a3fde-136">Il est important d’utiliser ce nom exact, en respectant l’utilisation des majuscules, de sorte que les espaces de noms correspondent au moment où le code est copié et collé.</span><span class="sxs-lookup"><span data-stu-id="a3fde-136">It's important to use this exact name including capitalization, so the namespaces match when code is copied and pasted.</span></span>
* <span data-ttu-id="a3fde-137">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="a3fde-137">Select **Create**.</span></span>
* <span data-ttu-id="a3fde-138">Sélectionnez **.net Core** et **ASP.net Core 5,0** dans les listes déroulantes, puis sélectionnez **application Web**.</span><span class="sxs-lookup"><span data-stu-id="a3fde-138">Select **.NET Core** and **ASP.NET Core 5.0** in the dropdowns, and then select **Web Application**.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="a3fde-139">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a3fde-139">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a3fde-140">Dans un terminal, accédez au dossier dans lequel le dossier de projet doit être créé.</span><span class="sxs-lookup"><span data-stu-id="a3fde-140">In a terminal, navigate to the folder in which the project folder should be created.</span></span>
* <span data-ttu-id="a3fde-141">Exécutez les commandes suivantes pour créer un :::no-loc(Razor)::: projet de pages et `cd` dans le dossier du nouveau projet :</span><span class="sxs-lookup"><span data-stu-id="a3fde-141">Run the following commands to create a :::no-loc(Razor)::: Pages project and `cd` into the new project folder:</span></span>

  ```dotnetcli
  dotnet new webapp -o ContosoUniversity
  cd ContosoUniversity  
  ```

---

## <a name="set-up-the-site-style"></a><span data-ttu-id="a3fde-142">Configurer le style du site</span><span class="sxs-lookup"><span data-stu-id="a3fde-142">Set up the site style</span></span>

<span data-ttu-id="a3fde-143">Copiez et collez le code suivant dans le fichier *pages/Shared/_Layout. cshtml* : [!code-cshtml[Main](intro/samples/cu50/Pages/Shared/_Layout.cshtml?highlight=6,14,21-35,49)]</span><span class="sxs-lookup"><span data-stu-id="a3fde-143">Copy and paste the following code into the *Pages/Shared/_Layout.cshtml* file: [!code-cshtml[Main](intro/samples/cu50/Pages/Shared/_Layout.cshtml?highlight=6,14,21-35,49)]</span></span>

<span data-ttu-id="a3fde-144">Le fichier de disposition définit l’en-tête, le pied de page et le menu du site.</span><span class="sxs-lookup"><span data-stu-id="a3fde-144">The layout file sets the site header, footer, and menu.</span></span> <span data-ttu-id="a3fde-145">Le code précédent apporte les modifications suivantes :</span><span class="sxs-lookup"><span data-stu-id="a3fde-145">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="a3fde-146">Chaque occurrence de « ContosoUniversity » à « Contoso University ».</span><span class="sxs-lookup"><span data-stu-id="a3fde-146">Each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="a3fde-147">Il y a trois occurrences.</span><span class="sxs-lookup"><span data-stu-id="a3fde-147">There are three occurrences.</span></span>
* <span data-ttu-id="a3fde-148">Les entrées du menu d' **hébergement** et de **confidentialité** sont supprimées.</span><span class="sxs-lookup"><span data-stu-id="a3fde-148">The **Home** and **Privacy** menu entries are deleted.</span></span>
* <span data-ttu-id="a3fde-149">Des entrées sont ajoutées pour **à propos** de, **étudiants** , **cours** , **instructeurs** et **services**.</span><span class="sxs-lookup"><span data-stu-id="a3fde-149">Entries are added for **About** , **Students** , **Courses** , **Instructors** , and **Departments**.</span></span>

<span data-ttu-id="a3fde-150">Dans *pages/index. cshtml* , remplacez le contenu du fichier par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a3fde-150">In *Pages/Index.cshtml* , replace the contents of the file with the following code:</span></span>

[!code-cshtml[Main](intro/samples/cu50/Pages/Index.cshtml)]

<span data-ttu-id="a3fde-151">Le code précédent remplace le texte relatif à ASP.NET Core par le texte de cette application.</span><span class="sxs-lookup"><span data-stu-id="a3fde-151">The preceding code replaces the text about ASP.NET Core with text about this app.</span></span>

<span data-ttu-id="a3fde-152">Exécutez l’application pour vérifier que la page d’accueil (« Home ») s’affiche.</span><span class="sxs-lookup"><span data-stu-id="a3fde-152">Run the app to verify that the home page appears.</span></span>

## <a name="the-data-model"></a><span data-ttu-id="a3fde-153">Modèle de données</span><span class="sxs-lookup"><span data-stu-id="a3fde-153">The data model</span></span>

<span data-ttu-id="a3fde-154">Les sections suivantes créent un modèle de données :</span><span class="sxs-lookup"><span data-stu-id="a3fde-154">The following sections create a data model:</span></span>

![Diagramme du modèle de données Course-Enrollment-Student](intro/_static/data-model-diagram.png)

<span data-ttu-id="a3fde-156">Un étudiant peut s’inscrire à un nombre quelconque de cours et un cours peut avoir un nombre quelconque d’élèves inscrits.</span><span class="sxs-lookup"><span data-stu-id="a3fde-156">A student can enroll in any number of courses, and a course can have any number of students enrolled in it.</span></span>

## <a name="the-student-entity"></a><span data-ttu-id="a3fde-157">L’entité Student</span><span class="sxs-lookup"><span data-stu-id="a3fde-157">The Student entity</span></span>

![Diagramme de l’entité Student](intro/_static/student-entity.png)

* <span data-ttu-id="a3fde-159">Créez un dossier *Models* dans le dossier de projet.</span><span class="sxs-lookup"><span data-stu-id="a3fde-159">Create a *Models* folder in the project folder.</span></span> 

* <span data-ttu-id="a3fde-160">Créez *Models/Student.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a3fde-160">Create *Models/Student.cs* with the following code:</span></span>

  [!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Student.cs)]

<span data-ttu-id="a3fde-161">La propriété `ID` devient la colonne de clé primaire de la table de base de données qui correspond à cette classe.</span><span class="sxs-lookup"><span data-stu-id="a3fde-161">The `ID` property becomes the primary key column of the database table that corresponds to this class.</span></span> <span data-ttu-id="a3fde-162">Par défaut, EF Core interprète une propriété nommée `ID` ou `classnameID` comme clé primaire.</span><span class="sxs-lookup"><span data-stu-id="a3fde-162">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span> <span data-ttu-id="a3fde-163">L’autre nom reconnu automatiquement de la clé primaire de classe `Student` est `StudentID`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-163">So the alternative automatically recognized name for the `Student` class primary key is `StudentID`.</span></span> <span data-ttu-id="a3fde-164">Pour plus d’informations, consultez [EF Core-Keys](/ef/core/modeling/keys?tabs=data-annotations).</span><span class="sxs-lookup"><span data-stu-id="a3fde-164">For more information, see [EF Core - Keys](/ef/core/modeling/keys?tabs=data-annotations).</span></span>

<span data-ttu-id="a3fde-165">La `Enrollments` propriété est une [propriété de navigation](/ef/core/modeling/relationships).</span><span class="sxs-lookup"><span data-stu-id="a3fde-165">The `Enrollments` property is a [navigation property](/ef/core/modeling/relationships).</span></span> <span data-ttu-id="a3fde-166">Les propriétés de navigation contiennent d’autres entités qui sont associées à cette entité.</span><span class="sxs-lookup"><span data-stu-id="a3fde-166">Navigation properties hold other entities that are related to this entity.</span></span> <span data-ttu-id="a3fde-167">Dans ce cas, la propriété `Enrollments` d’une entité `Student` contient toutes les entités `Enrollment` associées à cet étudiant.</span><span class="sxs-lookup"><span data-stu-id="a3fde-167">In this case, the `Enrollments` property of a `Student` entity holds all of the `Enrollment` entities that are related to that Student.</span></span> <span data-ttu-id="a3fde-168">Par exemple, si une ligne Student dans la base de données est associée à deux lignes Enrollment, la propriété de navigation `Enrollments` contient ces deux entités Enrollment.</span><span class="sxs-lookup"><span data-stu-id="a3fde-168">For example, if a Student row in the database has two related Enrollment rows, the `Enrollments` navigation property contains those two Enrollment entities.</span></span> 

<span data-ttu-id="a3fde-169">Dans la base de données, une ligne Enrollment est associée à une ligne Student si sa colonne StudentID contient la valeur d’ID de l’étudiant.</span><span class="sxs-lookup"><span data-stu-id="a3fde-169">In the database, an Enrollment row is related to a Student row if its StudentID column contains the student's ID value.</span></span> <span data-ttu-id="a3fde-170">Par exemple, supposez qu’une ligne Student présente un ID égal à 1.</span><span class="sxs-lookup"><span data-stu-id="a3fde-170">For example, suppose a Student row has ID=1.</span></span> <span data-ttu-id="a3fde-171">Les lignes Enrollment associées auront un StudentID égal à 1.</span><span class="sxs-lookup"><span data-stu-id="a3fde-171">Related Enrollment rows will have StudentID = 1.</span></span> <span data-ttu-id="a3fde-172">StudentID est une *clé étrangère* dans la table Enrollment.</span><span class="sxs-lookup"><span data-stu-id="a3fde-172">StudentID is a *foreign key* in the Enrollment table.</span></span> 

<span data-ttu-id="a3fde-173">La propriété `Enrollments` est définie en tant que `ICollection<Enrollment>`, car plusieurs entités Enrollment associées peuvent exister.</span><span class="sxs-lookup"><span data-stu-id="a3fde-173">The `Enrollments` property is defined as `ICollection<Enrollment>` because there may be multiple related Enrollment entities.</span></span> <span data-ttu-id="a3fde-174">Vous pouvez utiliser d’autres types de collection, comme `List<Enrollment>` ou `HashSet<Enrollment>`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-174">You can use other collection types, such as `List<Enrollment>` or `HashSet<Enrollment>`.</span></span> <span data-ttu-id="a3fde-175">Quand vous utilisez `ICollection<Enrollment>`, EF Core crée une collection `HashSet<Enrollment>` par défaut.</span><span class="sxs-lookup"><span data-stu-id="a3fde-175">When `ICollection<Enrollment>` is used, EF Core creates a `HashSet<Enrollment>` collection by default.</span></span>

## <a name="the-enrollment-entity"></a><span data-ttu-id="a3fde-176">L’entité Enrollment</span><span class="sxs-lookup"><span data-stu-id="a3fde-176">The Enrollment entity</span></span>

![Diagramme de l’entité Enrollment](intro/_static/enrollment-entity.png)

<span data-ttu-id="a3fde-178">Créez *Models/Enrollment.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a3fde-178">Create *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Enrollment.cs)]

<span data-ttu-id="a3fde-179">La propriété `EnrollmentID` est la clé primaire ; cette entité utilise le modèle `classnameID` à la place de `ID` par lui-même.</span><span class="sxs-lookup"><span data-stu-id="a3fde-179">The `EnrollmentID` property is the primary key; this entity uses the `classnameID` pattern instead of `ID` by itself.</span></span> <span data-ttu-id="a3fde-180">Pour un modèle de données de production, choisissez un modèle et utilisez-le systématiquement.</span><span class="sxs-lookup"><span data-stu-id="a3fde-180">For a production data model, choose one pattern and use it consistently.</span></span> <span data-ttu-id="a3fde-181">Ce tutoriel utilise les deux pour montrer qu’ils fonctionnent tous les deux.</span><span class="sxs-lookup"><span data-stu-id="a3fde-181">This tutorial uses both just to illustrate that both work.</span></span> <span data-ttu-id="a3fde-182">L’utilisation de `ID` sans `classname` facilite l’implémentation de certaines modifications du modèle de données.</span><span class="sxs-lookup"><span data-stu-id="a3fde-182">Using `ID` without `classname` makes it easier to implement some kinds of data model changes.</span></span>

<span data-ttu-id="a3fde-183">La propriété `Grade` est un `enum`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-183">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="a3fde-184">La présence du point d’interrogation après la déclaration de type `Grade` indique que la propriété `Grade`[accepte les valeurs Null](/dotnet/csharp/programming-guide/nullable-types/).</span><span class="sxs-lookup"><span data-stu-id="a3fde-184">The question mark after the `Grade` type declaration indicates that the `Grade` property is [nullable](/dotnet/csharp/programming-guide/nullable-types/).</span></span> <span data-ttu-id="a3fde-185">Une note (Grade) de valeur Null est différente d’une note zéro : la valeur Null signifie que la note n’est pas connue ou qu’elle n’a pas encore été attribuée.</span><span class="sxs-lookup"><span data-stu-id="a3fde-185">A grade that's null is different from a zero grade&mdash;null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="a3fde-186">La propriété `StudentID` est une clé étrangère, et la propriété de navigation correspondante est `Student`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-186">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="a3fde-187">Une entité `Enrollment` est associée à une entité `Student`. Par conséquent, la propriété contient une seule entité `Student`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-187">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span>

<span data-ttu-id="a3fde-188">La propriété `CourseID` est une clé étrangère, et la propriété de navigation correspondante est `Course`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-188">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="a3fde-189">Une entité `Enrollment` est associée à une entité `Course`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-189">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="a3fde-190">EF Core interprète une propriété en tant que clé étrangère si elle se nomme `<navigation property name><primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-190">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="a3fde-191">Par exemple, `StudentID` est la clé étrangère pour la propriété de navigation `Student`, car la clé primaire de l’entité `Student` est `ID`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-191">For example,`StudentID` is the foreign key for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="a3fde-192">Les propriétés de clé étrangère peuvent également se nommer `<primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-192">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="a3fde-193">Par exemple, `CourseID` puisque la clé primaire de l’entité `Course` est `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-193">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

## <a name="the-course-entity"></a><span data-ttu-id="a3fde-194">L’entité Course</span><span class="sxs-lookup"><span data-stu-id="a3fde-194">The Course entity</span></span>

![Diagramme de l’entité Course](intro/_static/course-entity.png)

<span data-ttu-id="a3fde-196">Créez *Models/Course.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a3fde-196">Create *Models/Course.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Course.cs)]

<span data-ttu-id="a3fde-197">La propriété `Enrollments` est une propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="a3fde-197">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="a3fde-198">Une entité `Course` peut être associée à un nombre quelconque d’entités `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-198">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="a3fde-199">L’attribut `DatabaseGenerated` permet à l’application de spécifier la clé primaire, plutôt que de la faire générer par la base de données.</span><span class="sxs-lookup"><span data-stu-id="a3fde-199">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the database generate it.</span></span>

<span data-ttu-id="a3fde-200">Générez le projet pour vérifier qu’il n’y a pas d’erreurs du compilateur.</span><span class="sxs-lookup"><span data-stu-id="a3fde-200">Build the project to validate that there are no compiler errors.</span></span>

## <a name="scaffold-student-pages"></a><span data-ttu-id="a3fde-201">Générer automatiquement des modèles de pages Student</span><span class="sxs-lookup"><span data-stu-id="a3fde-201">Scaffold Student pages</span></span>

<span data-ttu-id="a3fde-202">Dans cette section, vous allez utiliser l’outil de génération de modèles automatique ASP.NET Core pour générer :</span><span class="sxs-lookup"><span data-stu-id="a3fde-202">In this section, you use the ASP.NET Core scaffolding tool to generate:</span></span>

* <span data-ttu-id="a3fde-203">Classe EF Core `DbContext` .</span><span class="sxs-lookup"><span data-stu-id="a3fde-203">An EF Core `DbContext` class.</span></span> <span data-ttu-id="a3fde-204">Le contexte est la classe principale qui coordonne les fonctionnalités d’Entity Framework pour un modèle de données déterminé.</span><span class="sxs-lookup"><span data-stu-id="a3fde-204">The context is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="a3fde-205">Il dérive de la classe <xref:Microsoft.EntityFrameworkCore.DbContext?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="a3fde-205">It derives from the <xref:Microsoft.EntityFrameworkCore.DbContext?displayProperty=fullName> class.</span></span>
* <span data-ttu-id="a3fde-206">:::no-loc(Razor)::: les pages qui gèrent les opérations de création, lecture, mise à jour et suppression (CRUD) pour l' `Student` entité.</span><span class="sxs-lookup"><span data-stu-id="a3fde-206">:::no-loc(Razor)::: pages that handle Create, Read, Update, and Delete (CRUD) operations for the `Student` entity.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a3fde-207">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a3fde-207">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a3fde-208">Créez un dossier *Pages/Students*.</span><span class="sxs-lookup"><span data-stu-id="a3fde-208">Create a *Pages/Students* folder.</span></span>
* <span data-ttu-id="a3fde-209">Dans l’ **Explorateur de solutions** , cliquez avec le bouton droit sur le dossier *Pages/Students* , puis sélectionnez **Ajouter** > **Nouvel élément généré automatiquement**.</span><span class="sxs-lookup"><span data-stu-id="a3fde-209">In **Solution Explorer** , right-click the *Pages/Students* folder and select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="a3fde-210">Dans la boîte de dialogue **Ajouter un nouvel élément de structure** :</span><span class="sxs-lookup"><span data-stu-id="a3fde-210">In the **Add New Scaffold Item** dialog:</span></span>
  * <span data-ttu-id="a3fde-211">Dans l’onglet de gauche, sélectionnez **installé > :::no-loc(Razor)::: pages > communes**</span><span class="sxs-lookup"><span data-stu-id="a3fde-211">In the left tab, select **Installed > Common > :::no-loc(Razor)::: Pages**</span></span>
  * <span data-ttu-id="a3fde-212">Sélectionnez **:::no-loc(Razor)::: pages à l’aide de Entity Framework (CRUD)** > **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="a3fde-212">Select **:::no-loc(Razor)::: Pages using Entity Framework (CRUD)** > **ADD**.</span></span>
* <span data-ttu-id="a3fde-213">Dans la boîte de dialogue **Ajouter des :::no-loc(Razor)::: pages à l’aide de Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="a3fde-213">In the **Add :::no-loc(Razor)::: Pages using Entity Framework (CRUD)** dialog:</span></span>
  * <span data-ttu-id="a3fde-214">Dans la liste déroulante **Classe de modèle** , sélectionnez **Student (ContosoUniversity.Models)**.</span><span class="sxs-lookup"><span data-stu-id="a3fde-214">In the **Model class** drop-down, select **Student (ContosoUniversity.Models)**.</span></span>
  * <span data-ttu-id="a3fde-215">Dans la ligne **Classe du contexte de données** , sélectionnez le signe **+** (plus).</span><span class="sxs-lookup"><span data-stu-id="a3fde-215">In the **Data context class** row, select the **+** (plus) sign.</span></span>
    * <span data-ttu-id="a3fde-216">Modifiez le nom du contexte de données pour qu’il se termine par `SchoolContext` plutôt que `ContosoUniversityContext` .</span><span class="sxs-lookup"><span data-stu-id="a3fde-216">Change the data context name to end in `SchoolContext` rather than `ContosoUniversityContext`.</span></span> <span data-ttu-id="a3fde-217">Nom du contexte mis à jour : `ContosoUniversity.Data.SchoolContext`</span><span class="sxs-lookup"><span data-stu-id="a3fde-217">The updated context name: `ContosoUniversity.Data.SchoolContext`</span></span>
   * <span data-ttu-id="a3fde-218">Sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="a3fde-218">Select **Add**.</span></span>

<span data-ttu-id="a3fde-219">Les packages suivants sont automatiquement installés :</span><span class="sxs-lookup"><span data-stu-id="a3fde-219">The following packages are automatically installed:</span></span>

* `Microsoft.EntityFrameworkCore.SqlServer`
* `Microsoft.EntityFrameworkCore.Tools`
* `Microsoft.VisualStudio.Web.CodeGeneration.Design`

# <a name="visual-studio-code"></a>[<span data-ttu-id="a3fde-220">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a3fde-220">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a3fde-221">Exécutez les commandes CLI .NET Core suivantes pour installer les packages NuGet nécessaires :</span><span class="sxs-lookup"><span data-stu-id="a3fde-221">Run the following .NET Core CLI commands to install required NuGet packages:</span></span>

  ```dotnetcli
  dotnet add package Microsoft.EntityFrameworkCore.SQLite -v 5.0.0-*
  dotnet add package Microsoft.EntityFrameworkCore.SqlServer -v 5.0.0-*
  dotnet add package Microsoft.EntityFrameworkCore.Design -v 5.0.0-*
  dotnet add package Microsoft.EntityFrameworkCore.Tools -v 5.0.0-*
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v 5.0.0-*
  dotnet add package Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore -v 5.0.0-*  
  ```

   <span data-ttu-id="a3fde-222">Le package Microsoft.VisualStudio.Web.CodeGeneration.Design est requis pour la génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="a3fde-222">The Microsoft.VisualStudio.Web.CodeGeneration.Design package is required for scaffolding.</span></span> <span data-ttu-id="a3fde-223">Bien que l’application ne soit pas appelée à utiliser SQL Server, l’outil de génération de modèles automatique a besoin du package SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a3fde-223">Although the app won't use SQL Server, the scaffolding tool needs the SQL Server package.</span></span>

* <span data-ttu-id="a3fde-224">Créez un dossier *Pages/Students*.</span><span class="sxs-lookup"><span data-stu-id="a3fde-224">Create a *Pages/Students* folder.</span></span>

* <span data-ttu-id="a3fde-225">Exécutez la commande suivante pour installer l’[outil de génération de modèles automatique aspnet-codegenerator](xref:fundamentals/tools/dotnet-aspnet-codegenerator).</span><span class="sxs-lookup"><span data-stu-id="a3fde-225">Run the following command to install the [aspnet-codegenerator scaffolding tool](xref:fundamentals/tools/dotnet-aspnet-codegenerator).</span></span>

  ```dotnetcli
  dotnet tool uninstall --global dotnet-aspnet-codegenerator
  dotnet tool install --global dotnet-aspnet-codegenerator --version 5.0.0-*  
  ```

* <span data-ttu-id="a3fde-226">Exécutez la commande suivante pour générer automatiquement des modèles de pages Student.</span><span class="sxs-lookup"><span data-stu-id="a3fde-226">Run the following command to scaffold Student pages.</span></span>

  <span data-ttu-id="a3fde-227">**Sur Windows**</span><span class="sxs-lookup"><span data-stu-id="a3fde-227">**On Windows**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Data.SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries -sqlite  
  ```

  <span data-ttu-id="a3fde-228">**Sur macOS ou Linux**</span><span class="sxs-lookup"><span data-stu-id="a3fde-228">**On macOS or Linux**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Data.SchoolContext -udl -outDir Pages/Students --referenceScriptLibraries -sqlite  
  ```

---

<span data-ttu-id="a3fde-229">Si l’étape précédente échoue, générez le projet et recommencez l’étape de génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="a3fde-229">If the preceding step fails, build the project and retry the scaffold step.</span></span>

<span data-ttu-id="a3fde-230">Le processus de génération de modèles automatique :</span><span class="sxs-lookup"><span data-stu-id="a3fde-230">The scaffolding process:</span></span>

* <span data-ttu-id="a3fde-231">Crée :::no-loc(Razor)::: des pages dans le dossier *pages/élèves* :</span><span class="sxs-lookup"><span data-stu-id="a3fde-231">Creates :::no-loc(Razor)::: pages in the *Pages/Students* folder:</span></span>
  * <span data-ttu-id="a3fde-232">*Create.cshtml* et *Create.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="a3fde-232">*Create.cshtml* and *Create.cshtml.cs*</span></span>
  * <span data-ttu-id="a3fde-233">*Delete.cshtml* et *Delete.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="a3fde-233">*Delete.cshtml* and *Delete.cshtml.cs*</span></span>
  * <span data-ttu-id="a3fde-234">*Details.cshtml* et *Details.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="a3fde-234">*Details.cshtml* and *Details.cshtml.cs*</span></span>
  * <span data-ttu-id="a3fde-235">*Edit.cshtml* et *Edit.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="a3fde-235">*Edit.cshtml* and *Edit.cshtml.cs*</span></span>
  * <span data-ttu-id="a3fde-236">*Index.cshtml* et *Index.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="a3fde-236">*Index.cshtml* and *Index.cshtml.cs*</span></span>
* <span data-ttu-id="a3fde-237">Crée *Data/SchoolContext. cs*.</span><span class="sxs-lookup"><span data-stu-id="a3fde-237">Creates *Data/SchoolContext.cs*.</span></span>
* <span data-ttu-id="a3fde-238">Ajoute le contexte à l’injection de dépendances dans *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="a3fde-238">Adds the context to dependency injection in *Startup.cs*.</span></span>
* <span data-ttu-id="a3fde-239">Ajoute une chaîne de connexion de base de données à *:::no-loc(appsettings.json):::* .</span><span class="sxs-lookup"><span data-stu-id="a3fde-239">Adds a database connection string to *:::no-loc(appsettings.json):::*.</span></span>

## <a name="database-connection-string"></a><span data-ttu-id="a3fde-240">Chaîne de connexion de base de données</span><span class="sxs-lookup"><span data-stu-id="a3fde-240">Database connection string</span></span>

<span data-ttu-id="a3fde-241">L’outil de génération de modèles automatique génère une chaîne de connexion dans le *:::no-loc(appsettings.json):::* fichier.</span><span class="sxs-lookup"><span data-stu-id="a3fde-241">The scaffolding tool generates a connection string in the *:::no-loc(appsettings.json):::* file.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a3fde-242">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a3fde-242">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a3fde-243">La chaîne de connexion spécifie [SQL Server](/sql/database-engine/configure-windows/sql-server-2016-express-localdb)base de données locale :</span><span class="sxs-lookup"><span data-stu-id="a3fde-243">The connection string specifies [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb):</span></span>

[!code-json[Main](intro/samples/cu50/:::no-loc(appsettings.json):::?highlight=11)]

<span data-ttu-id="a3fde-244">LocalDB est une version allégée du moteur de base de données SQL Server Express. Elle est destinée au développement d’applications, et non à une utilisation en production.</span><span class="sxs-lookup"><span data-stu-id="a3fde-244">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="a3fde-245">Par défaut, la Base de données locale crée des fichiers *.mdf* dans le répertoire `C:/Users/<user>`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-245">By default, LocalDB creates *.mdf* files in the `C:/Users/<user>` directory.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="a3fde-246">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a3fde-246">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a3fde-247">Raccourcissez la chaîne de connexion SQLite à *cu. db* :</span><span class="sxs-lookup"><span data-stu-id="a3fde-247">Shorten the SQLite connection string to *CU.db* :</span></span>

[!code-json[Main](intro/samples/cu50/appsettingsSQLite.json?highlight=11)]

---

## <a name="update-the-database-context-class"></a><span data-ttu-id="a3fde-248">Mettre à jour la classe du contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="a3fde-248">Update the database context class</span></span>

<span data-ttu-id="a3fde-249">La classe principale qui coordonne les fonctionnalités d’EF Core pour un modèle de données déterminé est la classe du contexte de base de données.</span><span class="sxs-lookup"><span data-stu-id="a3fde-249">The main class that coordinates EF Core functionality for a given data model is the database context class.</span></span> <span data-ttu-id="a3fde-250">Le contexte est dérivé de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="a3fde-250">The context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="a3fde-251">Il spécifie les entités qui sont incluses dans le modèle de données.</span><span class="sxs-lookup"><span data-stu-id="a3fde-251">The context specifies which entities are included in the data model.</span></span> <span data-ttu-id="a3fde-252">Dans ce projet, la classe est nommée `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-252">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="a3fde-253">Mettez à jour *Data/SchoolContext.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a3fde-253">Update *Data/SchoolContext.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Data/SchoolContext.cs?highlight=13-22)]

<span data-ttu-id="a3fde-254">Le code précédent passe du singulier `DbSet<Student> Student` au pluriel `DbSet<Student> Students` .</span><span class="sxs-lookup"><span data-stu-id="a3fde-254">The preceding code changes from the singular `DbSet<Student> Student` to the  plural `DbSet<Student> Students`.</span></span> <span data-ttu-id="a3fde-255">Pour que le :::no-loc(Razor)::: Code des pages corresponde au nouveau `DBSet` nom, effectuez une modification globale à partir de : `_context.Student.`</span><span class="sxs-lookup"><span data-stu-id="a3fde-255">To make the :::no-loc(Razor)::: Pages code match the new `DBSet` name, make a global change from: `_context.Student.`</span></span>
<span data-ttu-id="a3fde-256">À: `_context.Students.`</span><span class="sxs-lookup"><span data-stu-id="a3fde-256">to: `_context.Students.`</span></span>

<span data-ttu-id="a3fde-257">Il y a 8 occurrences.</span><span class="sxs-lookup"><span data-stu-id="a3fde-257">There are 8 occurrences.</span></span>

<span data-ttu-id="a3fde-258">Un jeu d’entités contenant plusieurs entités, de nombreux développeurs préfèrent que les `DBSet` noms de propriété soient au pluriel.</span><span class="sxs-lookup"><span data-stu-id="a3fde-258">Because an entity set contains multiple entities, many developers prefer the `DBSet` property names should be plural.</span></span>

<span data-ttu-id="a3fde-259">Code mis en surbrillance :</span><span class="sxs-lookup"><span data-stu-id="a3fde-259">The highlighted code:</span></span>

* <span data-ttu-id="a3fde-260">Crée une [propriété \<TEntity> DbSet](/dotnet/api/microsoft.entityframeworkcore.dbset-1) pour chaque jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="a3fde-260">Creates a [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for each entity set.</span></span> <span data-ttu-id="a3fde-261">Dans la terminologie d’EF Core :</span><span class="sxs-lookup"><span data-stu-id="a3fde-261">In EF Core terminology:</span></span>
  * <span data-ttu-id="a3fde-262">Un jeu d’entités correspond généralement à une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="a3fde-262">An entity set typically corresponds to a database table.</span></span>
  * <span data-ttu-id="a3fde-263">Une entité correspond à une ligne dans la table.</span><span class="sxs-lookup"><span data-stu-id="a3fde-263">An entity corresponds to a row in the table.</span></span>
* <span data-ttu-id="a3fde-264">Appelle <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating%2A>.</span><span class="sxs-lookup"><span data-stu-id="a3fde-264">Calls <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating%2A>.</span></span> <span data-ttu-id="a3fde-265">`OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="a3fde-265">`OnModelCreating`:</span></span>
  * <span data-ttu-id="a3fde-266">Est appelé quand `SchoolContext` a été initialisé, mais avant que le modèle ait été verrouillé et utilisé pour initialiser le contexte.</span><span class="sxs-lookup"><span data-stu-id="a3fde-266">Is called when `SchoolContext` has been initialized, but before the model has been locked down and used to initialize the context.</span></span>
  * <span data-ttu-id="a3fde-267">Est requis, car ultérieurement dans le didacticiel, l' `Student` entité aura des références aux autres entités.</span><span class="sxs-lookup"><span data-stu-id="a3fde-267">Is required because later in the tutorial The `Student` entity will have references to the other entities.</span></span>
  <!-- Review, OnModelCreating needs review -->

<span data-ttu-id="a3fde-268">Générez le projet pour vérifier qu’il n’y a pas d’erreurs du compilateur.</span><span class="sxs-lookup"><span data-stu-id="a3fde-268">Build the project to verify there are no compiler errors.</span></span>

## <a name="startupcs"></a><span data-ttu-id="a3fde-269">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="a3fde-269">Startup.cs</span></span>

<span data-ttu-id="a3fde-270">ASP.NET Core comprend [l’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a3fde-270">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="a3fde-271">Les services tels que le `SchoolContext` sont inscrits avec l’injection de dépendances au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="a3fde-271">Services such as the `SchoolContext` are registered with dependency injection during app startup.</span></span> <span data-ttu-id="a3fde-272">Ces services sont fournis par les composants qui requièrent ces services, tels que les :::no-loc(Razor)::: pages, via des paramètres de constructeur.</span><span class="sxs-lookup"><span data-stu-id="a3fde-272">Components that require these services, such as :::no-loc(Razor)::: Pages, are provided these services via constructor parameters.</span></span> <span data-ttu-id="a3fde-273">Le code de constructeur qui obtient une instance de contexte de base de données est indiqué plus loin dans le tutoriel.</span><span class="sxs-lookup"><span data-stu-id="a3fde-273">The constructor code that gets a database context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="a3fde-274">L’outil de génération de modèles automatique a inscrit automatiquement la classe du contexte dans le conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="a3fde-274">The scaffolding tool automatically registered the context class with the dependency injection container.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a3fde-275">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a3fde-275">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a3fde-276">Les lignes en surbrillance suivantes ont été ajoutées par le générateur de modèles :</span><span class="sxs-lookup"><span data-stu-id="a3fde-276">The following highlighted lines were added by the scaffolder:</span></span>

[!code-csharp[Main](intro/samples/cu30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="a3fde-277">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a3fde-277">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a3fde-278">Vérifiez le code ajouté par les appels de l’échafaudage `UseSqlite` .</span><span class="sxs-lookup"><span data-stu-id="a3fde-278">Verify the code added by the scaffolder calls `UseSqlite`.</span></span>

[!code-csharp[Main](intro/samples/cu30/StartupSQLite.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="a3fde-279">Pour plus d’informations sur l’utilisation d’une base de données de production [, consultez utiliser SQLite pour le développement, SQL Server pour la production](xref:tutorials/razor-pages/model#use-sqlite-for-development-sql-server-for-production) .</span><span class="sxs-lookup"><span data-stu-id="a3fde-279">See [Use SQLite for development, SQL Server for production](xref:tutorials/razor-pages/model#use-sqlite-for-development-sql-server-for-production) for information on using a production database.</span></span>

---

<span data-ttu-id="a3fde-280">Le nom de la chaîne de connexion est transmis au contexte en appelant une méthode sur un objet [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="a3fde-280">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="a3fde-281">Pour le développement local, le [système de configuration ASP.net Core](xref:fundamentals/configuration/index) lit la chaîne de connexion à partir du *:::no-loc(appsettings.json):::* fichier.</span><span class="sxs-lookup"><span data-stu-id="a3fde-281">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *:::no-loc(appsettings.json):::* file.</span></span>

### <a name="add-the-database-exception-filter"></a><span data-ttu-id="a3fde-282">Ajouter le filtre d’exception de base de données</span><span class="sxs-lookup"><span data-stu-id="a3fde-282">Add the database exception filter</span></span>

<span data-ttu-id="a3fde-283">Ajoutez <xref:Microsoft.Extensions.DependencyInjection.DatabaseDeveloperPageExceptionFilterServiceExtensions.AddDatabaseDeveloperPageExceptionFilter%2A> à `ConfigureServices` , comme indiqué dans le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a3fde-283">Add <xref:Microsoft.Extensions.DependencyInjection.DatabaseDeveloperPageExceptionFilterServiceExtensions.AddDatabaseDeveloperPageExceptionFilter%2A> to `ConfigureServices` as shown in the following code:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a3fde-284">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a3fde-284">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[Main](intro/samples/cu50/Startup.cs?name=snippet_ConfigureServices&highlight=8)]

<span data-ttu-id="a3fde-285">Ajoutez le package NuGet [Microsoft. AspNetCore. Diagnostics. EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore) .</span><span class="sxs-lookup"><span data-stu-id="a3fde-285">Add the [Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore) NuGet package.</span></span>

<span data-ttu-id="a3fde-286">Dans le PMC, entrez ce qui suit pour ajouter le package NuGet :</span><span class="sxs-lookup"><span data-stu-id="a3fde-286">In the PMC, enter the following to add the NuGet package:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore -Version 5.0.0-rc.2.20475.17
```

# <a name="visual-studio-code"></a>[<span data-ttu-id="a3fde-287">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a3fde-287">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!code-csharp[Main](intro/samples/cu50/StartupSQLite.cs?name=snippet_ConfigureServices&highlight=8)]

---

<span data-ttu-id="a3fde-288">Le `Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore` package NuGet fournit ASP.net Core intergiciel pour Entity Framework Core pages d’erreurs.</span><span class="sxs-lookup"><span data-stu-id="a3fde-288">The `Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore` NuGet package provides ASP.NET Core middleware for Entity Framework Core error pages.</span></span> <span data-ttu-id="a3fde-289">Cet intergiciel (middleware) permet de détecter et de diagnostiquer les erreurs avec Entity Framework Core migrations.</span><span class="sxs-lookup"><span data-stu-id="a3fde-289">This middleware helps to detect and diagnose errors with Entity Framework Core migrations.</span></span>

<span data-ttu-id="a3fde-290">Le `AddDatabaseDeveloperPageExceptionFilter` fournit des informations d’erreur utiles dans l' [environnement de développement](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="a3fde-290">The `AddDatabaseDeveloperPageExceptionFilter` provides helpful error information in the [development environment](xref:fundamentals/environments).</span></span>

## <a name="create-the-database"></a><span data-ttu-id="a3fde-291">Création de la base de données</span><span class="sxs-lookup"><span data-stu-id="a3fde-291">Create the database</span></span>

<span data-ttu-id="a3fde-292">Mettez à jour *Program.cs* pour créer la base de données si elle n’existe pas :</span><span class="sxs-lookup"><span data-stu-id="a3fde-292">Update *Program.cs* to create the database if it doesn't exist:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Program.cs?highlight=1-2,14-18,21-38)]

<span data-ttu-id="a3fde-293">La méthode [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated) n’effectue aucune action s’il existe une base de données pour le contexte.</span><span class="sxs-lookup"><span data-stu-id="a3fde-293">The [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated) method takes no action if a database for the context exists.</span></span> <span data-ttu-id="a3fde-294">S’il n’existe pas de base de données, elle crée la base de données et le schéma.</span><span class="sxs-lookup"><span data-stu-id="a3fde-294">If no database exists, it creates the database and schema.</span></span> <span data-ttu-id="a3fde-295">`EnsureCreated` active le workflow suivant pour gérer les modifications du modèle de données :</span><span class="sxs-lookup"><span data-stu-id="a3fde-295">`EnsureCreated` enables the following workflow for handling data model changes:</span></span>

* <span data-ttu-id="a3fde-296">Supprimez la base de données.</span><span class="sxs-lookup"><span data-stu-id="a3fde-296">Delete the database.</span></span> <span data-ttu-id="a3fde-297">Toutes les données existantes sont perdues.</span><span class="sxs-lookup"><span data-stu-id="a3fde-297">Any existing data is lost.</span></span>
* <span data-ttu-id="a3fde-298">Modifiez le modèle de données.</span><span class="sxs-lookup"><span data-stu-id="a3fde-298">Change the data model.</span></span> <span data-ttu-id="a3fde-299">Par exemple, ajoutez un champ `EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-299">For example, add an `EmailAddress` field.</span></span>
* <span data-ttu-id="a3fde-300">Exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="a3fde-300">Run the app.</span></span>
* <span data-ttu-id="a3fde-301">`EnsureCreated` crée une base de données avec le nouveau schéma.</span><span class="sxs-lookup"><span data-stu-id="a3fde-301">`EnsureCreated` creates a database with the new schema.</span></span>

<span data-ttu-id="a3fde-302">Ce workflow fonctionne bien à un stade précoce du développement, quand le schéma évolue rapidement, aussi longtemps que vous n’avez pas besoin de conserver les données.</span><span class="sxs-lookup"><span data-stu-id="a3fde-302">This workflow works well early in development when the schema is rapidly evolving, as long as you don't need to preserve data.</span></span> <span data-ttu-id="a3fde-303">La situation est différente quand les données qui ont été entrées dans la base de données doivent être conservées.</span><span class="sxs-lookup"><span data-stu-id="a3fde-303">The situation is different when data that has been entered into the database needs to be preserved.</span></span> <span data-ttu-id="a3fde-304">Dans ce cas, procédez à des migrations.</span><span class="sxs-lookup"><span data-stu-id="a3fde-304">When that is the case, use migrations.</span></span>

<span data-ttu-id="a3fde-305">Plus tard dans cette série de tutoriels, vous supprimerez la base de données créée par `EnsureCreated` et procéderez à des migrations.</span><span class="sxs-lookup"><span data-stu-id="a3fde-305">Later in the tutorial series, you delete the database that was created by `EnsureCreated` and use migrations instead.</span></span> <span data-ttu-id="a3fde-306">Une base de données créée par `EnsureCreated` ne peut pas être mise à jour via des migrations.</span><span class="sxs-lookup"><span data-stu-id="a3fde-306">A database that is created by `EnsureCreated` can't be updated by using migrations.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="a3fde-307">Tester l’application</span><span class="sxs-lookup"><span data-stu-id="a3fde-307">Test the app</span></span>

* <span data-ttu-id="a3fde-308">Exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="a3fde-308">Run the app.</span></span>
* <span data-ttu-id="a3fde-309">Sélectionnez le lien **Students** , puis **Créer nouveau**.</span><span class="sxs-lookup"><span data-stu-id="a3fde-309">Select the **Students** link and then **Create New**.</span></span>
* <span data-ttu-id="a3fde-310">Testez les liens Edit, Details et Delete.</span><span class="sxs-lookup"><span data-stu-id="a3fde-310">Test the Edit, Details, and Delete links.</span></span>

## <a name="seed-the-database"></a><span data-ttu-id="a3fde-311">Amorcer la base de données</span><span class="sxs-lookup"><span data-stu-id="a3fde-311">Seed the database</span></span>

<span data-ttu-id="a3fde-312">La méthode `EnsureCreated` crée une base de données vide.</span><span class="sxs-lookup"><span data-stu-id="a3fde-312">The `EnsureCreated` method creates an empty database.</span></span> <span data-ttu-id="a3fde-313">Cette section ajoute du code qui remplit la base de données avec des données de test.</span><span class="sxs-lookup"><span data-stu-id="a3fde-313">This section adds code that populates the database with test data.</span></span>

<span data-ttu-id="a3fde-314">Créez *Data/DbInitializer.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a3fde-314">Create *Data/DbInitializer.cs* with the following code:</span></span>
<!-- next update, keep this file in the project and surround with #if -->
  [!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Data/DbInitializer.cs)]

  <span data-ttu-id="a3fde-315">Le code vérifie si des étudiants figurent dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="a3fde-315">The code checks if there are any students in the database.</span></span> <span data-ttu-id="a3fde-316">S’il n’y a pas d’étudiants, il ajoute des données de test à la base de données.</span><span class="sxs-lookup"><span data-stu-id="a3fde-316">If there are no students, it adds test data to the database.</span></span> <span data-ttu-id="a3fde-317">Il crée les données de test dans des tableaux et non dans des collections `List<T>` afin d’optimiser les performances.</span><span class="sxs-lookup"><span data-stu-id="a3fde-317">It creates the test data in arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="a3fde-318">Dans *Program.cs* , remplacez l’appel `EnsureCreated` par un appel `DbInitializer.Initialize` :</span><span class="sxs-lookup"><span data-stu-id="a3fde-318">In *Program.cs* , replace the `EnsureCreated` call with a `DbInitializer.Initialize` call:</span></span>

  ```csharp
  // context.Database.EnsureCreated();
  DbInitializer.Initialize(context);
  ```

# <a name="visual-studio"></a>[<span data-ttu-id="a3fde-319">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a3fde-319">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a3fde-320">Arrêtez l’application si elle est en cours d’exécution et exécutez la commande suivante dans la **Console du gestionnaire de package**  :</span><span class="sxs-lookup"><span data-stu-id="a3fde-320">Stop the app if it's running, and run the following command in the **Package Manager Console** (PMC):</span></span>

```powershell
Drop-Database -Confirm
```

<span data-ttu-id="a3fde-321">Répondre avec `Y` pour supprimer la base de données.</span><span class="sxs-lookup"><span data-stu-id="a3fde-321">Respond with `Y` to delete the database.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="a3fde-322">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a3fde-322">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a3fde-323">Arrêtez l’application si elle est en cours d’exécution, puis supprimez le fichier *CU.db*.</span><span class="sxs-lookup"><span data-stu-id="a3fde-323">Stop the app if it's running, and delete the *CU.db* file.</span></span>

---

* <span data-ttu-id="a3fde-324">Redémarrez l’application.</span><span class="sxs-lookup"><span data-stu-id="a3fde-324">Restart the app.</span></span>
* <span data-ttu-id="a3fde-325">Sélectionnez la page Students pour examiner les données amorcées.</span><span class="sxs-lookup"><span data-stu-id="a3fde-325">Select the Students page to see the seeded data.</span></span>

## <a name="view-the-database"></a><span data-ttu-id="a3fde-326">Afficher la base de données</span><span class="sxs-lookup"><span data-stu-id="a3fde-326">View the database</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a3fde-327">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a3fde-327">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a3fde-328">Ouvrez **l’Explorateur d’objets SQL Server** (SSOX) à partir du menu **Affichage** de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a3fde-328">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
* <span data-ttu-id="a3fde-329">Dans SSOX, sélectionnez **(localdb)\MSSQLLocalDB > Databases > SchoolContext-{GUID}**.</span><span class="sxs-lookup"><span data-stu-id="a3fde-329">In SSOX, select **(localdb)\MSSQLLocalDB > Databases > SchoolContext-{GUID}**.</span></span> <span data-ttu-id="a3fde-330">Le nom de la base de données est généré à partir du nom de contexte indiqué précédemment, ainsi que d’un tiret et d’un GUID.</span><span class="sxs-lookup"><span data-stu-id="a3fde-330">The database name is generated from the context name you provided earlier plus a dash and a GUID.</span></span>
* <span data-ttu-id="a3fde-331">Développez le nœud **Tables**.</span><span class="sxs-lookup"><span data-stu-id="a3fde-331">Expand the **Tables** node.</span></span>
* <span data-ttu-id="a3fde-332">Cliquez avec le bouton droit sur la table **Student** et cliquez sur **Afficher les données** pour voir les colonnes créées et les lignes insérées dans la table.</span><span class="sxs-lookup"><span data-stu-id="a3fde-332">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>
* <span data-ttu-id="a3fde-333">Cliquez avec le bouton droit sur la table **Student** et cliquez sur **Afficher le code** pour voir comment le modèle `Student` est mappé au schéma de la table `Student`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-333">Right-click the **Student** table and click **View Code** to see how the `Student` model maps to the `Student` table schema.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="a3fde-334">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a3fde-334">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a3fde-335">Utilisez votre outil SQLite pour examiner le schéma de base de données et les données amorcées.</span><span class="sxs-lookup"><span data-stu-id="a3fde-335">Use your SQLite tool to view the database schema and seeded data.</span></span> <span data-ttu-id="a3fde-336">Le fichier de base de données est nommé *CU.db* et se trouve dans le dossier du projet.</span><span class="sxs-lookup"><span data-stu-id="a3fde-336">The database file is named *CU.db* and is located in the project folder.</span></span>

---

## <a name="asynchronous-code"></a><span data-ttu-id="a3fde-337">Code asynchrone</span><span class="sxs-lookup"><span data-stu-id="a3fde-337">Asynchronous code</span></span>

<span data-ttu-id="a3fde-338">La programmation asynchrone est le mode par défaut pour ASP.NET Core et EF Core.</span><span class="sxs-lookup"><span data-stu-id="a3fde-338">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="a3fde-339">Un serveur web a un nombre limité de threads disponibles et, dans les situations de forte charge, tous les threads disponibles peuvent être utilisés.</span><span class="sxs-lookup"><span data-stu-id="a3fde-339">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="a3fde-340">Quand cela se produit, le serveur ne peut pas traiter de nouvelle requête tant que les threads ne sont pas libérés.</span><span class="sxs-lookup"><span data-stu-id="a3fde-340">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="a3fde-341">Avec le code synchrone, de nombreux threads peuvent être associés alors qu’ils ne fonctionnent pas car ils sont en attente de la fin des e/s.</span><span class="sxs-lookup"><span data-stu-id="a3fde-341">With synchronous code, many threads may be tied up while they aren't doing work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="a3fde-342">Avec le code asynchrone, quand un processus attend que des E/S se terminent, son thread est libéré afin d’être utilisé par le serveur pour traiter d’autres demandes.</span><span class="sxs-lookup"><span data-stu-id="a3fde-342">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="a3fde-343">Il permet ainsi d’utiliser les ressources serveur plus efficacement, et le serveur peut gérer plus de trafic sans retard.</span><span class="sxs-lookup"><span data-stu-id="a3fde-343">As a result, asynchronous code enables server resources to be used more efficiently, and the server can handle more traffic without delays.</span></span>

<span data-ttu-id="a3fde-344">Le code asynchrone introduit néanmoins une petite surcharge au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="a3fde-344">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="a3fde-345">Dans les situations de faible trafic, le gain de performances est négligeable, tandis qu’en cas de trafic élevé l’amélioration potentielle des performances est importante.</span><span class="sxs-lookup"><span data-stu-id="a3fde-345">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="a3fde-346">Dans le code suivant, le mot clé [async](/dotnet/csharp/language-reference/keywords/async), la valeur renvoyée `Task<T>`, le mot clé `await` et la méthode `ToListAsync` déclenchent l’exécution asynchrone du code.</span><span class="sxs-lookup"><span data-stu-id="a3fde-346">In the following code, the [async](/dotnet/csharp/language-reference/keywords/async) keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

```csharp
public async Task OnGetAsync()
{
    Students = await _context.Students.ToListAsync();
}
```

* <span data-ttu-id="a3fde-347">Le mot clé `async` fait en sorte que le compilateur :</span><span class="sxs-lookup"><span data-stu-id="a3fde-347">The `async` keyword tells the compiler to:</span></span>
  * <span data-ttu-id="a3fde-348">Génère des rappels pour les parties du corps de méthode.</span><span class="sxs-lookup"><span data-stu-id="a3fde-348">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="a3fde-349">Crée l’objet [Task](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType) qui est retourné.</span><span class="sxs-lookup"><span data-stu-id="a3fde-349">Create the [Task](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType) object that's returned.</span></span>
* <span data-ttu-id="a3fde-350">Le type de retour `Task<T>` représente le travail en cours.</span><span class="sxs-lookup"><span data-stu-id="a3fde-350">The `Task<T>` return type represents ongoing work.</span></span>
* <span data-ttu-id="a3fde-351">Le mot clé `await` indique au compilateur de fractionner la méthode en deux parties.</span><span class="sxs-lookup"><span data-stu-id="a3fde-351">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="a3fde-352">La première partie se termine par l’opération qui est démarrée de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="a3fde-352">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="a3fde-353">La seconde partie est placée dans une méthode de rappel qui est appelée quand l’opération se termine.</span><span class="sxs-lookup"><span data-stu-id="a3fde-353">The second part is put into a callback method that's called when the operation completes.</span></span>
* <span data-ttu-id="a3fde-354">`ToListAsync` est la version asynchrone de la méthode d’extension `ToList`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-354">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="a3fde-355">Voici quelques éléments à connaître lors de l’écriture de code asynchrone qui utilise EF Core :</span><span class="sxs-lookup"><span data-stu-id="a3fde-355">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="a3fde-356">Seules les instructions qui provoquent l’envoi de requêtes ou de commandes vers la base de données sont exécutées de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="a3fde-356">Only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="a3fde-357">Cela inclut `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync` et `SaveChangesAsync`,</span><span class="sxs-lookup"><span data-stu-id="a3fde-357">That includes `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="a3fde-358">mais pas les instructions qui ne font que changer un `IQueryable`, telles que `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-358">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>
* <span data-ttu-id="a3fde-359">Un contexte EF Core n’est pas thread-safe : n’essayez pas d’effectuer plusieurs opérations en parallèle.</span><span class="sxs-lookup"><span data-stu-id="a3fde-359">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span>
* <span data-ttu-id="a3fde-360">Pour tirer parti des avantages de performances du code asynchrone, vérifiez que les packages de bibliothèque (par exemple pour la pagination) utilisent le mode asynchrone s’ils appellent des méthodes EF Core qui envoient des requêtes à la base de données.</span><span class="sxs-lookup"><span data-stu-id="a3fde-360">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the database.</span></span>

<span data-ttu-id="a3fde-361">Pour plus d’informations sur la programmation asynchrone dans .NET, consultez [Vue d’ensemble d’Async](/dotnet/standard/async) et [Programmation asynchrone avec async et await](/dotnet/csharp/programming-guide/concepts/async/).</span><span class="sxs-lookup"><span data-stu-id="a3fde-361">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/standard/async) and [Asynchronous programming with async and await](/dotnet/csharp/programming-guide/concepts/async/).</span></span>

<!-- Review: See https://github.com/dotnet/AspNetCore.Docs/issues/14528 -->
## <a name="performance-considerations"></a><span data-ttu-id="a3fde-362">Considérations relatives aux performances</span><span class="sxs-lookup"><span data-stu-id="a3fde-362">Performance considerations</span></span>

<span data-ttu-id="a3fde-363">En général, une page Web ne doit pas charger un nombre arbitraire de lignes.</span><span class="sxs-lookup"><span data-stu-id="a3fde-363">In general, a web page shouldn't be loading an arbitrary number of rows.</span></span> <span data-ttu-id="a3fde-364">Une requête doit utiliser la pagination ou une approche de limitation.</span><span class="sxs-lookup"><span data-stu-id="a3fde-364">A query should use paging or a limiting approach.</span></span> <span data-ttu-id="a3fde-365">Par exemple, la requête ci-dessus peut utiliser `Take` pour limiter les lignes retournées :</span><span class="sxs-lookup"><span data-stu-id="a3fde-365">For example, the preceding query could use `Take` to limit the rows returned:</span></span>

[!code-csharp[Main](intro/samples/cu50snapshots/Index.cshtml.cs?name=snippet)]

<span data-ttu-id="a3fde-366">L’énumération d’une table volumineuse dans une vue peut retourner une réponse HTTP 200 partiellement construite si une exception de base de données se produit de façon partielle via l’énumération.</span><span class="sxs-lookup"><span data-stu-id="a3fde-366">Enumerating a large table in a view could return a partially constructed HTTP 200 response if a database exception occurs part way through the enumeration.</span></span>

<span data-ttu-id="a3fde-367"><xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxModelBindingCollectionSize> la valeur par défaut est 1024.</span><span class="sxs-lookup"><span data-stu-id="a3fde-367"><xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxModelBindingCollectionSize> defaults to 1024.</span></span> <span data-ttu-id="a3fde-368">Les jeux de codes suivants `MaxModelBindingCollectionSize` :</span><span class="sxs-lookup"><span data-stu-id="a3fde-368">The following code sets `MaxModelBindingCollectionSize`:</span></span>

[!code-csharp[Main](intro/samples/cu50/StartupMaxMBsize.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="a3fde-369">La pagination est traitée plus loin dans le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="a3fde-369">Paging is covered later in the tutorial.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a3fde-370">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="a3fde-370">Next steps</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a3fde-371">Didacticiel suivant</span><span class="sxs-lookup"><span data-stu-id="a3fde-371">Next tutorial</span></span>](xref:data/ef-rp/crud)

::: moniker-end

::: moniker range=">= aspnetcore-3.0 < aspnetcore-5.0"

<span data-ttu-id="a3fde-372">Il s’agit de la première d’une série de didacticiels qui montrent comment utiliser Entity Framework (EF) Core dans une application [ASP.net Core :::no-loc(Razor)::: pages](xref:razor-pages/index) .</span><span class="sxs-lookup"><span data-stu-id="a3fde-372">This is the first in a series of tutorials that show how to use Entity Framework (EF) Core in an [ASP.NET Core :::no-loc(Razor)::: Pages](xref:razor-pages/index) app.</span></span> <span data-ttu-id="a3fde-373">Dans ces tutoriels, un site web est créé pour une université fictive nommée Contoso.</span><span class="sxs-lookup"><span data-stu-id="a3fde-373">The tutorials build a web site for a fictional Contoso University.</span></span> <span data-ttu-id="a3fde-374">Le site comprend des fonctionnalités comme l’admission des étudiants, la création de cours et les affectations des formateurs.</span><span class="sxs-lookup"><span data-stu-id="a3fde-374">The site includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="a3fde-375">Le didacticiel utilise l’approche code First.</span><span class="sxs-lookup"><span data-stu-id="a3fde-375">The tutorial uses the code first approach.</span></span> <span data-ttu-id="a3fde-376">Pour plus d’informations sur ce didacticiel avec la première approche basée sur la base de données, consultez [ce problème GitHub](https://github.com/dotnet/AspNetCore.Docs/issues/16897).</span><span class="sxs-lookup"><span data-stu-id="a3fde-376">For information on following this tutorial using the database first approach, see [this Github issue](https://github.com/dotnet/AspNetCore.Docs/issues/16897).</span></span>

[<span data-ttu-id="a3fde-377">Télécharger ou afficher l’application terminée.</span><span class="sxs-lookup"><span data-stu-id="a3fde-377">Download or view the completed app.</span></span>](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="a3fde-378">[Instructions de téléchargement](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="a3fde-378">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a3fde-379">Prérequis</span><span class="sxs-lookup"><span data-stu-id="a3fde-379">Prerequisites</span></span>

* <span data-ttu-id="a3fde-380">Si vous :::no-loc(Razor)::: débutez avec des pages, consultez la série de didacticiels [prise en main des :::no-loc(Razor)::: pages](xref:tutorials/razor-pages/razor-pages-start) avant de commencer celle-ci.</span><span class="sxs-lookup"><span data-stu-id="a3fde-380">If you're new to :::no-loc(Razor)::: Pages, go through the [Get started with :::no-loc(Razor)::: Pages](xref:tutorials/razor-pages/razor-pages-start) tutorial series before starting this one.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a3fde-381">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a3fde-381">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[VS prereqs](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="a3fde-382">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a3fde-382">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[VS Code prereqs](~/includes/net-core-prereqs-vsc-3.0.md)]

---

## <a name="database-engines"></a><span data-ttu-id="a3fde-383">Moteurs de base de données</span><span class="sxs-lookup"><span data-stu-id="a3fde-383">Database engines</span></span>

<span data-ttu-id="a3fde-384">Les instructions Visual Studio utilisent la [Base de données locale SQL Server](/sql/database-engine/configure-windows/sql-server-2016-express-localdb), version de SQL Server Express qui s’exécute uniquement sur Windows.</span><span class="sxs-lookup"><span data-stu-id="a3fde-384">The Visual Studio instructions use [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb), a version of SQL Server Express that runs only on Windows.</span></span>

<span data-ttu-id="a3fde-385">Les instructions Visual Studio Code utilisent [SQLite](https://www.sqlite.org/), moteur de base de données multiplateforme.</span><span class="sxs-lookup"><span data-stu-id="a3fde-385">The Visual Studio Code instructions use [SQLite](https://www.sqlite.org/), a cross-platform database engine.</span></span>

<span data-ttu-id="a3fde-386">Si vous choisissez d’utiliser SQLite, téléchargez et installez un outil tiers pour la gestion et l’affichage d’une base de données SQLite, comme [Browser for SQLite](https://sqlitebrowser.org/).</span><span class="sxs-lookup"><span data-stu-id="a3fde-386">If you choose to use SQLite, download and install a third-party tool for managing and viewing a SQLite database, such as [DB Browser for SQLite](https://sqlitebrowser.org/).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="a3fde-387">Résolution des problèmes</span><span class="sxs-lookup"><span data-stu-id="a3fde-387">Troubleshooting</span></span>

<span data-ttu-id="a3fde-388">Si vous rencontrez un problème que vous ne pouvez pas résoudre, comparez votre code au [projet terminé](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="a3fde-388">If you run into a problem you can't resolve, compare your code to the [completed project](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span> <span data-ttu-id="a3fde-389">Un bon moyen d’obtenir de l’aide est de poster une question sur StackOverflow.com en utilisant le [mot-clé ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) ou le [mot-clé EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="a3fde-389">A good way to get help is by posting a question to StackOverflow.com, using the [ASP.NET Core tag](https://stackoverflow.com/questions/tagged/asp.net-core) or the [EF Core tag](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

## <a name="the-sample-app"></a><span data-ttu-id="a3fde-390">Exemple d’application</span><span class="sxs-lookup"><span data-stu-id="a3fde-390">The sample app</span></span>

<span data-ttu-id="a3fde-391">L’application générée dans ces didacticiels est le site web de base d’une université.</span><span class="sxs-lookup"><span data-stu-id="a3fde-391">The app built in these tutorials is a basic university web site.</span></span> <span data-ttu-id="a3fde-392">Les utilisateurs peuvent afficher et mettre à jour les informations relatives aux étudiants, aux cours et aux formateurs.</span><span class="sxs-lookup"><span data-stu-id="a3fde-392">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="a3fde-393">Voici quelques-uns des écrans créés dans le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="a3fde-393">Here are a few of the screens created in the tutorial.</span></span>

![Page d’index des étudiants](intro/_static/students-index30.png)

![Page de modification des étudiants](intro/_static/student-edit30.png)

<span data-ttu-id="a3fde-396">Le style de l’interface utilisateur de ce site repose sur les modèles de projet intégrés.</span><span class="sxs-lookup"><span data-stu-id="a3fde-396">The UI style of this site is based on the built-in project templates.</span></span> <span data-ttu-id="a3fde-397">Le tutoriel traite essentiellement de l’utilisation d’EF Core, et non de la façon de personnaliser l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="a3fde-397">The tutorial's focus is on how to use EF Core, not how to customize the UI.</span></span>

<span data-ttu-id="a3fde-398">Suivez le lien en haut de la page pour obtenir le code source du projet terminé.</span><span class="sxs-lookup"><span data-stu-id="a3fde-398">Follow the link at the top of the page to get the source code for the completed project.</span></span> <span data-ttu-id="a3fde-399">Le dossier *cu30* contient le code de la version 3.0 d’ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a3fde-399">The *cu30* folder has the code for the ASP.NET Core 3.0 version of the tutorial.</span></span> <span data-ttu-id="a3fde-400">Les fichiers qui reflètent l’état du code pour les tutoriels 1-7 se trouvent dans le dossier *cu30snapshots*.</span><span class="sxs-lookup"><span data-stu-id="a3fde-400">Files that reflect the state of the code for tutorials 1-7 can be found in the *cu30snapshots* folder.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a3fde-401">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a3fde-401">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a3fde-402">Pour exécuter l’application après avoir téléchargé le projet terminé :</span><span class="sxs-lookup"><span data-stu-id="a3fde-402">To run the app after downloading the completed project:</span></span>

* <span data-ttu-id="a3fde-403">Créez le projet.</span><span class="sxs-lookup"><span data-stu-id="a3fde-403">Build the project.</span></span>
* <span data-ttu-id="a3fde-404">Dans la console du Gestionnaire de package (PMC), exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="a3fde-404">In Package Manager Console (PMC) run the following command:</span></span>

  ```powershell
  Update-Database
  ```

* <span data-ttu-id="a3fde-405">Exécutez le projet pour amorcer la base de données.</span><span class="sxs-lookup"><span data-stu-id="a3fde-405">Run the project to seed the database.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="a3fde-406">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a3fde-406">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a3fde-407">Pour exécuter l’application après avoir téléchargé le projet terminé :</span><span class="sxs-lookup"><span data-stu-id="a3fde-407">To run the app after downloading the completed project:</span></span>

* <span data-ttu-id="a3fde-408">Supprimez *ContosoUniversity.csproj* , puis renommez *ContosoUniversitySQLite.csproj* en *ContosoUniversity.csproj*.</span><span class="sxs-lookup"><span data-stu-id="a3fde-408">Delete *ContosoUniversity.csproj* , and rename *ContosoUniversitySQLite.csproj* to *ContosoUniversity.csproj*.</span></span>
* <span data-ttu-id="a3fde-409">Dans *Program.cs* , commentez-le `#define Startup` pour qu’il `StartupSQLite` soit utilisé.</span><span class="sxs-lookup"><span data-stu-id="a3fde-409">In *Program.cs* , comment out `#define Startup` so `StartupSQLite` is used.</span></span>
* <span data-ttu-id="a3fde-410">Supprimez *appSettings.json* et renommez *appSettingsSQLite.json* en *appSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a3fde-410">Delete *appSettings.json* , and rename *appSettingsSQLite.json* to *appSettings.json*.</span></span>
* <span data-ttu-id="a3fde-411">Supprimez le dossier *Migrations* et renommez *MigrationsSQL* en *Migrations*.</span><span class="sxs-lookup"><span data-stu-id="a3fde-411">Delete the *Migrations* folder, and rename *MigrationsSQL* to *Migrations*.</span></span>
* <span data-ttu-id="a3fde-412">Effectuez une recherche globale `#if SQLiteVersion` et supprimez `#if SQLiteVersion` et l' `#endif` instruction associée.</span><span class="sxs-lookup"><span data-stu-id="a3fde-412">Do a global search for `#if SQLiteVersion` and remove `#if SQLiteVersion` and the associated `#endif` statement.</span></span>
* <span data-ttu-id="a3fde-413">Créez le projet.</span><span class="sxs-lookup"><span data-stu-id="a3fde-413">Build the project.</span></span>
* <span data-ttu-id="a3fde-414">Dans une invite de commandes, exécutez la commande suivante dans le dossier du projet :</span><span class="sxs-lookup"><span data-stu-id="a3fde-414">At a command prompt in the project folder, run the following commands:</span></span>

  ```dotnetcli
  dotnet tool uninstall --global dotnet-ef
  dotnet tool install --global dotnet-ef --version 5.0.0-*
  dotnet ef database update
  ```

* <span data-ttu-id="a3fde-415">Dans votre outil SQLite, exécutez l’instruction SQL suivante :</span><span class="sxs-lookup"><span data-stu-id="a3fde-415">In your SQLite tool, run the following SQL statement:</span></span>

  ```sql
  UPDATE Department SET RowVersion = randomblob(8)
  ```
  
* <span data-ttu-id="a3fde-416">Exécutez le projet pour amorcer la base de données.</span><span class="sxs-lookup"><span data-stu-id="a3fde-416">Run the project to seed the database.</span></span>

---

## <a name="create-the-web-app-project"></a><span data-ttu-id="a3fde-417">Créer le projet d’application web</span><span class="sxs-lookup"><span data-stu-id="a3fde-417">Create the web app project</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a3fde-418">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a3fde-418">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a3fde-419">Dans Visual Studio, dans le menu **Fichier** , sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="a3fde-419">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="a3fde-420">Sélectionnez **Application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="a3fde-420">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="a3fde-421">Nommez le projet *ContosoUniversity*.</span><span class="sxs-lookup"><span data-stu-id="a3fde-421">Name the project *ContosoUniversity*.</span></span> <span data-ttu-id="a3fde-422">Il est important d’utiliser ce nom exact, en respectant l’utilisation des majuscules, de sorte que les espaces de noms correspondent au moment où le code est copié et collé.</span><span class="sxs-lookup"><span data-stu-id="a3fde-422">It's important to use this exact name including capitalization, so the namespaces match when code is copied and pasted.</span></span>
* <span data-ttu-id="a3fde-423">Sélectionnez **.NET Core** et **ASP.NET Core 3.0** dans les listes déroulantes, puis **Application web**.</span><span class="sxs-lookup"><span data-stu-id="a3fde-423">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdowns, and then select **Web Application**.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="a3fde-424">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a3fde-424">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a3fde-425">Dans un terminal, accédez au dossier dans lequel le dossier de projet doit être créé.</span><span class="sxs-lookup"><span data-stu-id="a3fde-425">In a terminal, navigate to the folder in which the project folder should be created.</span></span>

* <span data-ttu-id="a3fde-426">Exécutez les commandes suivantes pour créer un :::no-loc(Razor)::: projet de pages et `cd` dans le dossier du nouveau projet :</span><span class="sxs-lookup"><span data-stu-id="a3fde-426">Run the following commands to create a :::no-loc(Razor)::: Pages project and `cd` into the new project folder:</span></span>

  ```dotnetcli
  dotnet new webapp -o ContosoUniversity
  cd ContosoUniversity
  ```

---

## <a name="set-up-the-site-style"></a><span data-ttu-id="a3fde-427">Configurer le style du site</span><span class="sxs-lookup"><span data-stu-id="a3fde-427">Set up the site style</span></span>

<span data-ttu-id="a3fde-428">Configurez l’en-tête, le pied de page et le menu du site en mettant à jour *Pages/Shared/_Layout.cshtml*  :</span><span class="sxs-lookup"><span data-stu-id="a3fde-428">Set up the site header, footer, and menu by updating *Pages/Shared/_Layout.cshtml* :</span></span>

* <span data-ttu-id="a3fde-429">Remplacez chaque occurrence de « ContosoUniversity » par « Contoso University ».</span><span class="sxs-lookup"><span data-stu-id="a3fde-429">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="a3fde-430">Il y a trois occurrences.</span><span class="sxs-lookup"><span data-stu-id="a3fde-430">There are three occurrences.</span></span>

* <span data-ttu-id="a3fde-431">Supprimez les entrées de menu **Home** et **Privacy** et ajoutez les entrées **About** , **Students** , **Courses** , **Instructors** et **Departments**.</span><span class="sxs-lookup"><span data-stu-id="a3fde-431">Delete the **Home** and **Privacy** menu entries, and add entries for **About** , **Students** , **Courses** , **Instructors** , and **Departments**.</span></span>

<span data-ttu-id="a3fde-432">Les modifications sont mises en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="a3fde-432">The changes are highlighted.</span></span>

[!code-cshtml[Main](intro/samples/cu30/Pages/Shared/_Layout.cshtml?highlight=6,14,21-35,49)]

<span data-ttu-id="a3fde-433">Dans *Pages/Index.cshtml* , remplacez le contenu du fichier par le code suivant de façon à remplacer le texte relatif à ASP.NET Core par le texte se rapportant à cette application :</span><span class="sxs-lookup"><span data-stu-id="a3fde-433">In *Pages/Index.cshtml* , replace the contents of the file with the following code to replace the text about ASP.NET Core with text about this app:</span></span>

[!code-cshtml[Main](intro/samples/cu30/Pages/Index.cshtml)]

<span data-ttu-id="a3fde-434">Exécutez l’application pour vérifier que la page d’accueil (« Home ») s’affiche.</span><span class="sxs-lookup"><span data-stu-id="a3fde-434">Run the app to verify that the home page appears.</span></span>

## <a name="the-data-model"></a><span data-ttu-id="a3fde-435">Modèle de données</span><span class="sxs-lookup"><span data-stu-id="a3fde-435">The data model</span></span>

<span data-ttu-id="a3fde-436">Les sections suivantes créent un modèle de données :</span><span class="sxs-lookup"><span data-stu-id="a3fde-436">The following sections create a data model:</span></span>

![Diagramme du modèle de données Course-Enrollment-Student](intro/_static/data-model-diagram.png)

<span data-ttu-id="a3fde-438">Un étudiant peut s’inscrire à un nombre quelconque de cours et un cours peut avoir un nombre quelconque d’élèves inscrits.</span><span class="sxs-lookup"><span data-stu-id="a3fde-438">A student can enroll in any number of courses, and a course can have any number of students enrolled in it.</span></span>

## <a name="the-student-entity"></a><span data-ttu-id="a3fde-439">L’entité Student</span><span class="sxs-lookup"><span data-stu-id="a3fde-439">The Student entity</span></span>

![Diagramme de l’entité Student](intro/_static/student-entity.png)

* <span data-ttu-id="a3fde-441">Créez un dossier *Models* dans le dossier de projet.</span><span class="sxs-lookup"><span data-stu-id="a3fde-441">Create a *Models* folder in the project folder.</span></span>
* <span data-ttu-id="a3fde-442">Créez *Models/Student.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a3fde-442">Create *Models/Student.cs* with the following code:</span></span>

  [!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Student.cs)]

<span data-ttu-id="a3fde-443">La propriété `ID` devient la colonne de clé primaire de la table de base de données qui correspond à cette classe.</span><span class="sxs-lookup"><span data-stu-id="a3fde-443">The `ID` property becomes the primary key column of the database table that corresponds to this class.</span></span> <span data-ttu-id="a3fde-444">Par défaut, EF Core interprète une propriété nommée `ID` ou `classnameID` comme clé primaire.</span><span class="sxs-lookup"><span data-stu-id="a3fde-444">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span> <span data-ttu-id="a3fde-445">L’autre nom reconnu automatiquement de la clé primaire de classe `Student` est `StudentID`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-445">So the alternative automatically recognized name for the `Student` class primary key is `StudentID`.</span></span> <span data-ttu-id="a3fde-446">Pour plus d’informations, consultez [EF Core-Keys](/ef/core/modeling/keys?tabs=data-annotations).</span><span class="sxs-lookup"><span data-stu-id="a3fde-446">For more information, see [EF Core - Keys](/ef/core/modeling/keys?tabs=data-annotations).</span></span>

<span data-ttu-id="a3fde-447">La `Enrollments` propriété est une [propriété de navigation](/ef/core/modeling/relationships).</span><span class="sxs-lookup"><span data-stu-id="a3fde-447">The `Enrollments` property is a [navigation property](/ef/core/modeling/relationships).</span></span> <span data-ttu-id="a3fde-448">Les propriétés de navigation contiennent d’autres entités qui sont associées à cette entité.</span><span class="sxs-lookup"><span data-stu-id="a3fde-448">Navigation properties hold other entities that are related to this entity.</span></span> <span data-ttu-id="a3fde-449">Dans ce cas, la propriété `Enrollments` d’une entité `Student` contient toutes les entités `Enrollment` associées à cet étudiant.</span><span class="sxs-lookup"><span data-stu-id="a3fde-449">In this case, the `Enrollments` property of a `Student` entity holds all of the `Enrollment` entities that are related to that Student.</span></span> <span data-ttu-id="a3fde-450">Par exemple, si une ligne Student dans la base de données est associée à deux lignes Enrollment, la propriété de navigation `Enrollments` contient ces deux entités Enrollment.</span><span class="sxs-lookup"><span data-stu-id="a3fde-450">For example, if a Student row in the database has two related Enrollment rows, the `Enrollments` navigation property contains those two Enrollment entities.</span></span> 

<span data-ttu-id="a3fde-451">Dans la base de données, une ligne Enrollment est associée à une ligne Student si sa colonne StudentID contient la valeur d’ID de l’étudiant.</span><span class="sxs-lookup"><span data-stu-id="a3fde-451">In the database, an Enrollment row is related to a Student row if its StudentID column contains the student's ID value.</span></span> <span data-ttu-id="a3fde-452">Par exemple, supposez qu’une ligne Student présente un ID égal à 1.</span><span class="sxs-lookup"><span data-stu-id="a3fde-452">For example, suppose a Student row has ID=1.</span></span> <span data-ttu-id="a3fde-453">Les lignes Enrollment associées auront un StudentID égal à 1.</span><span class="sxs-lookup"><span data-stu-id="a3fde-453">Related Enrollment rows will have StudentID = 1.</span></span> <span data-ttu-id="a3fde-454">StudentID est une *clé étrangère* dans la table Enrollment.</span><span class="sxs-lookup"><span data-stu-id="a3fde-454">StudentID is a *foreign key* in the Enrollment table.</span></span> 

<span data-ttu-id="a3fde-455">La propriété `Enrollments` est définie en tant que `ICollection<Enrollment>`, car plusieurs entités Enrollment associées peuvent exister.</span><span class="sxs-lookup"><span data-stu-id="a3fde-455">The `Enrollments` property is defined as `ICollection<Enrollment>` because there may be multiple related Enrollment entities.</span></span> <span data-ttu-id="a3fde-456">Vous pouvez utiliser d’autres types de collection, comme `List<Enrollment>` ou `HashSet<Enrollment>`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-456">You can use other collection types, such as `List<Enrollment>` or `HashSet<Enrollment>`.</span></span> <span data-ttu-id="a3fde-457">Quand vous utilisez `ICollection<Enrollment>`, EF Core crée une collection `HashSet<Enrollment>` par défaut.</span><span class="sxs-lookup"><span data-stu-id="a3fde-457">When `ICollection<Enrollment>` is used, EF Core creates a `HashSet<Enrollment>` collection by default.</span></span>

## <a name="the-enrollment-entity"></a><span data-ttu-id="a3fde-458">L’entité Enrollment</span><span class="sxs-lookup"><span data-stu-id="a3fde-458">The Enrollment entity</span></span>

![Diagramme de l’entité Enrollment](intro/_static/enrollment-entity.png)

<span data-ttu-id="a3fde-460">Créez *Models/Enrollment.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a3fde-460">Create *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Enrollment.cs)]

<span data-ttu-id="a3fde-461">La propriété `EnrollmentID` est la clé primaire ; cette entité utilise le modèle `classnameID` à la place de `ID` par lui-même.</span><span class="sxs-lookup"><span data-stu-id="a3fde-461">The `EnrollmentID` property is the primary key; this entity uses the `classnameID` pattern instead of `ID` by itself.</span></span> <span data-ttu-id="a3fde-462">Pour un modèle de données de production, choisissez un modèle et utilisez-le systématiquement.</span><span class="sxs-lookup"><span data-stu-id="a3fde-462">For a production data model, choose one pattern and use it consistently.</span></span> <span data-ttu-id="a3fde-463">Ce tutoriel utilise les deux pour montrer qu’ils fonctionnent tous les deux.</span><span class="sxs-lookup"><span data-stu-id="a3fde-463">This tutorial uses both just to illustrate that both work.</span></span> <span data-ttu-id="a3fde-464">L’utilisation de `ID` sans `classname` facilite l’implémentation de certaines modifications du modèle de données.</span><span class="sxs-lookup"><span data-stu-id="a3fde-464">Using `ID` without `classname` makes it easier to implement some kinds of data model changes.</span></span>

<span data-ttu-id="a3fde-465">La propriété `Grade` est un `enum`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-465">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="a3fde-466">La présence du point d’interrogation après la déclaration de type `Grade` indique que la propriété `Grade`[accepte les valeurs Null](/dotnet/csharp/programming-guide/nullable-types/).</span><span class="sxs-lookup"><span data-stu-id="a3fde-466">The question mark after the `Grade` type declaration indicates that the `Grade` property is [nullable](/dotnet/csharp/programming-guide/nullable-types/).</span></span> <span data-ttu-id="a3fde-467">Une note (Grade) de valeur Null est différente d’une note zéro : la valeur Null signifie que la note n’est pas connue ou qu’elle n’a pas encore été attribuée.</span><span class="sxs-lookup"><span data-stu-id="a3fde-467">A grade that's null is different from a zero grade&mdash;null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="a3fde-468">La propriété `StudentID` est une clé étrangère, et la propriété de navigation correspondante est `Student`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-468">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="a3fde-469">Une entité `Enrollment` est associée à une entité `Student`. Par conséquent, la propriété contient une seule entité `Student`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-469">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span>

<span data-ttu-id="a3fde-470">La propriété `CourseID` est une clé étrangère, et la propriété de navigation correspondante est `Course`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-470">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="a3fde-471">Une entité `Enrollment` est associée à une entité `Course`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-471">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="a3fde-472">EF Core interprète une propriété en tant que clé étrangère si elle se nomme `<navigation property name><primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-472">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="a3fde-473">Par exemple, `StudentID` est la clé étrangère pour la propriété de navigation `Student`, car la clé primaire de l’entité `Student` est `ID`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-473">For example,`StudentID` is the foreign key for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="a3fde-474">Les propriétés de clé étrangère peuvent également se nommer `<primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-474">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="a3fde-475">Par exemple, `CourseID` puisque la clé primaire de l’entité `Course` est `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-475">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

## <a name="the-course-entity"></a><span data-ttu-id="a3fde-476">L’entité Course</span><span class="sxs-lookup"><span data-stu-id="a3fde-476">The Course entity</span></span>

![Diagramme de l’entité Course](intro/_static/course-entity.png)

<span data-ttu-id="a3fde-478">Créez *Models/Course.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a3fde-478">Create *Models/Course.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Course.cs)]

<span data-ttu-id="a3fde-479">La propriété `Enrollments` est une propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="a3fde-479">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="a3fde-480">Une entité `Course` peut être associée à un nombre quelconque d’entités `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-480">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="a3fde-481">L’attribut `DatabaseGenerated` permet à l’application de spécifier la clé primaire, plutôt que de la faire générer par la base de données.</span><span class="sxs-lookup"><span data-stu-id="a3fde-481">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the database generate it.</span></span>

<span data-ttu-id="a3fde-482">Générez le projet pour vérifier qu’il n’y a pas d’erreurs du compilateur.</span><span class="sxs-lookup"><span data-stu-id="a3fde-482">Build the project to validate that there are no compiler errors.</span></span>

## <a name="scaffold-student-pages"></a><span data-ttu-id="a3fde-483">Générer automatiquement des modèles de pages Student</span><span class="sxs-lookup"><span data-stu-id="a3fde-483">Scaffold Student pages</span></span>

<span data-ttu-id="a3fde-484">Dans cette section, vous allez utiliser l’outil de génération de modèles automatique ASP.NET Core pour générer :</span><span class="sxs-lookup"><span data-stu-id="a3fde-484">In this section, you use the ASP.NET Core scaffolding tool to generate:</span></span>

* <span data-ttu-id="a3fde-485">Une classe de *contexte* EF Core.</span><span class="sxs-lookup"><span data-stu-id="a3fde-485">An EF Core *context* class.</span></span> <span data-ttu-id="a3fde-486">Le contexte est la classe principale qui coordonne les fonctionnalités d’Entity Framework pour un modèle de données déterminé.</span><span class="sxs-lookup"><span data-stu-id="a3fde-486">The context is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="a3fde-487">Il dérive de la classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-487">It derives from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>
* <span data-ttu-id="a3fde-488">:::no-loc(Razor)::: les pages qui gèrent les opérations de création, lecture, mise à jour et suppression (CRUD) pour l' `Student` entité.</span><span class="sxs-lookup"><span data-stu-id="a3fde-488">:::no-loc(Razor)::: pages that handle Create, Read, Update, and Delete (CRUD) operations for the `Student` entity.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a3fde-489">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a3fde-489">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a3fde-490">Créez un dossier *Students* dans le dossier *Pages*.</span><span class="sxs-lookup"><span data-stu-id="a3fde-490">Create a *Students* folder in the *Pages* folder.</span></span>
* <span data-ttu-id="a3fde-491">Dans l’ **Explorateur de solutions** , cliquez avec le bouton droit sur le dossier *Pages/Students* , puis sélectionnez **Ajouter** > **Nouvel élément généré automatiquement**.</span><span class="sxs-lookup"><span data-stu-id="a3fde-491">In **Solution Explorer** , right-click the *Pages/Students* folder and select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="a3fde-492">Dans la boîte de dialogue **Ajouter une structure** , sélectionnez **:::no-loc(Razor)::: pages à l’aide de Entity Framework (CRUD)** > **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="a3fde-492">In the **Add Scaffold** dialog, select **:::no-loc(Razor)::: Pages using Entity Framework (CRUD)** > **ADD**.</span></span>
* <span data-ttu-id="a3fde-493">Dans la boîte de dialogue **Ajouter des :::no-loc(Razor)::: pages à l’aide de Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="a3fde-493">In the **Add :::no-loc(Razor)::: Pages using Entity Framework (CRUD)** dialog:</span></span>
  * <span data-ttu-id="a3fde-494">Dans la liste déroulante **Classe de modèle** , sélectionnez **Student (ContosoUniversity.Models)**.</span><span class="sxs-lookup"><span data-stu-id="a3fde-494">In the **Model class** drop-down, select **Student (ContosoUniversity.Models)**.</span></span>
  * <span data-ttu-id="a3fde-495">Dans la ligne **Classe du contexte de données** , sélectionnez le signe **+** (plus).</span><span class="sxs-lookup"><span data-stu-id="a3fde-495">In the **Data context class** row, select the **+** (plus) sign.</span></span>
  * <span data-ttu-id="a3fde-496">Remplacez le nom du contexte de données *ContosoUniversity.Models.ContosoUniversityContext* par *ContosoUniversity.Data.SchoolContext*.</span><span class="sxs-lookup"><span data-stu-id="a3fde-496">Change the data context name from *ContosoUniversity.Models.ContosoUniversityContext* to *ContosoUniversity.Data.SchoolContext*.</span></span>
  * <span data-ttu-id="a3fde-497">Sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="a3fde-497">Select **Add**.</span></span>

<span data-ttu-id="a3fde-498">Les packages suivants sont automatiquement installés :</span><span class="sxs-lookup"><span data-stu-id="a3fde-498">The following packages are automatically installed:</span></span>

* `Microsoft.VisualStudio.Web.CodeGeneration.Design`
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Microsoft.Extensions.Logging.Debug`
* `Microsoft.EntityFrameworkCore.Tools`

# <a name="visual-studio-code"></a>[<span data-ttu-id="a3fde-499">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a3fde-499">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a3fde-500">Exécutez les commandes CLI .NET Core suivantes pour installer les packages NuGet nécessaires :</span><span class="sxs-lookup"><span data-stu-id="a3fde-500">Run the following .NET Core CLI commands to install required NuGet packages:</span></span>
<!-- TO DO  After testing, Replace with
[!INCLUDE[](~/includes/includes/add-EF-NuGet-SQLite-CLI.md)]
remove dotnet tool install --global  below
 -->
  ```dotnetcli
  dotnet add package Microsoft.EntityFrameworkCore.SQLite
  dotnet add package Microsoft.EntityFrameworkCore.SqlServer
  dotnet add package Microsoft.EntityFrameworkCore.Design
  dotnet add package Microsoft.EntityFrameworkCore.Tools
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
  dotnet add package Microsoft.Extensions.Logging.Debug
  ```

  <span data-ttu-id="a3fde-501">Le package Microsoft.VisualStudio.Web.CodeGeneration.Design est requis pour la génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="a3fde-501">The Microsoft.VisualStudio.Web.CodeGeneration.Design package is required for scaffolding.</span></span> <span data-ttu-id="a3fde-502">Bien que l’application ne soit pas appelée à utiliser SQL Server, l’outil de génération de modèles automatique a besoin du package SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a3fde-502">Although the app won't use SQL Server, the scaffolding tool needs the SQL Server package.</span></span>

* <span data-ttu-id="a3fde-503">Créez un dossier *Pages/Students*.</span><span class="sxs-lookup"><span data-stu-id="a3fde-503">Create a *Pages/Students* folder.</span></span>

* <span data-ttu-id="a3fde-504">Exécutez la commande suivante pour installer l’[outil de génération de modèles automatique aspnet-codegenerator](xref:fundamentals/tools/dotnet-aspnet-codegenerator).</span><span class="sxs-lookup"><span data-stu-id="a3fde-504">Run the following command to install the [aspnet-codegenerator scaffolding tool](xref:fundamentals/tools/dotnet-aspnet-codegenerator).</span></span>

  ```dotnetcli
  dotnet tool install --global dotnet-aspnet-codegenerator
  ```

* <span data-ttu-id="a3fde-505">Exécutez la commande suivante pour générer automatiquement des modèles de pages Student.</span><span class="sxs-lookup"><span data-stu-id="a3fde-505">Run the following command to scaffold Student pages.</span></span>

  <span data-ttu-id="a3fde-506">**Sur Windows**</span><span class="sxs-lookup"><span data-stu-id="a3fde-506">**On Windows**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Data.SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
  ```

  <span data-ttu-id="a3fde-507">**Sur macOS ou Linux**</span><span class="sxs-lookup"><span data-stu-id="a3fde-507">**On macOS or Linux**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Data.SchoolContext -udl -outDir Pages/Students --referenceScriptLibraries
  ```

---

<span data-ttu-id="a3fde-508">Si vous rencontrez un problème à l’étape précédente, générez le projet et recommencez l’étape de génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="a3fde-508">If you have a problem with the preceding step, build the project and retry the scaffold step.</span></span>

<span data-ttu-id="a3fde-509">Le processus de génération de modèles automatique :</span><span class="sxs-lookup"><span data-stu-id="a3fde-509">The scaffolding process:</span></span>

* <span data-ttu-id="a3fde-510">Crée :::no-loc(Razor)::: des pages dans le dossier *pages/élèves* :</span><span class="sxs-lookup"><span data-stu-id="a3fde-510">Creates :::no-loc(Razor)::: pages in the *Pages/Students* folder:</span></span>
  * <span data-ttu-id="a3fde-511">*Create.cshtml* et *Create.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="a3fde-511">*Create.cshtml* and *Create.cshtml.cs*</span></span>
  * <span data-ttu-id="a3fde-512">*Delete.cshtml* et *Delete.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="a3fde-512">*Delete.cshtml* and *Delete.cshtml.cs*</span></span>
  * <span data-ttu-id="a3fde-513">*Details.cshtml* et *Details.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="a3fde-513">*Details.cshtml* and *Details.cshtml.cs*</span></span>
  * <span data-ttu-id="a3fde-514">*Edit.cshtml* et *Edit.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="a3fde-514">*Edit.cshtml* and *Edit.cshtml.cs*</span></span>
  * <span data-ttu-id="a3fde-515">*Index.cshtml* et *Index.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="a3fde-515">*Index.cshtml* and *Index.cshtml.cs*</span></span>
* <span data-ttu-id="a3fde-516">Crée *Data/SchoolContext. cs*.</span><span class="sxs-lookup"><span data-stu-id="a3fde-516">Creates *Data/SchoolContext.cs*.</span></span>
* <span data-ttu-id="a3fde-517">Ajoute le contexte à l’injection de dépendances dans *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="a3fde-517">Adds the context to dependency injection in *Startup.cs*.</span></span>
* <span data-ttu-id="a3fde-518">Ajoute une chaîne de connexion de base de données à *:::no-loc(appsettings.json):::* .</span><span class="sxs-lookup"><span data-stu-id="a3fde-518">Adds a database connection string to *:::no-loc(appsettings.json):::*.</span></span>

## <a name="database-connection-string"></a><span data-ttu-id="a3fde-519">Chaîne de connexion de base de données</span><span class="sxs-lookup"><span data-stu-id="a3fde-519">Database connection string</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a3fde-520">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a3fde-520">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a3fde-521">Le *:::no-loc(appsettings.json):::* fichier spécifie la chaîne de connexion [SQL Server](/sql/database-engine/configure-windows/sql-server-2016-express-localdb)la base de données locale.</span><span class="sxs-lookup"><span data-stu-id="a3fde-521">The *:::no-loc(appsettings.json):::* file specifies the connection string [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span>

[!code-json[Main](intro/samples/cu30/:::no-loc(appsettings.json):::?highlight=11)]

<span data-ttu-id="a3fde-522">LocalDB est une version allégée du moteur de base de données SQL Server Express. Elle est destinée au développement d’applications, et non à une utilisation en production.</span><span class="sxs-lookup"><span data-stu-id="a3fde-522">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="a3fde-523">Par défaut, la Base de données locale crée des fichiers *.mdf* dans le répertoire `C:/Users/<user>`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-523">By default, LocalDB creates *.mdf* files in the `C:/Users/<user>` directory.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="a3fde-524">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a3fde-524">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a3fde-525">Modifiez la chaîne de connexion de sorte qu’elle pointe vers un fichier de base de données SQLite nommé *CU.db*  :</span><span class="sxs-lookup"><span data-stu-id="a3fde-525">Change the connection string to point to a SQLite database file named *CU.db* :</span></span>

[!code-json[Main](intro/samples/cu30/appsettingsSQLite.json?highlight=11)]

---

## <a name="update-the-database-context-class"></a><span data-ttu-id="a3fde-526">Mettre à jour la classe du contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="a3fde-526">Update the database context class</span></span>

<span data-ttu-id="a3fde-527">La classe principale qui coordonne les fonctionnalités d’EF Core pour un modèle de données déterminé est la classe du contexte de base de données.</span><span class="sxs-lookup"><span data-stu-id="a3fde-527">The main class that coordinates EF Core functionality for a given data model is the database context class.</span></span> <span data-ttu-id="a3fde-528">Le contexte est dérivé de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="a3fde-528">The context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="a3fde-529">Il spécifie les entités qui sont incluses dans le modèle de données.</span><span class="sxs-lookup"><span data-stu-id="a3fde-529">The context specifies which entities are included in the data model.</span></span> <span data-ttu-id="a3fde-530">Dans ce projet, la classe est nommée `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-530">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="a3fde-531">Mettez à jour *Data/SchoolContext.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a3fde-531">Update *Data/SchoolContext.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Data/SchoolContext.cs?highlight=13-22)]

<span data-ttu-id="a3fde-532">Le code en surbrillance crée une propriété [DbSet \<TEntity> ](/dotnet/api/microsoft.entityframeworkcore.dbset-1) pour chaque jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="a3fde-532">The highlighted code creates a [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for each entity set.</span></span> <span data-ttu-id="a3fde-533">Dans la terminologie d’EF Core :</span><span class="sxs-lookup"><span data-stu-id="a3fde-533">In EF Core terminology:</span></span>

* <span data-ttu-id="a3fde-534">Un jeu d’entités correspond généralement à une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="a3fde-534">An entity set typically corresponds to a database table.</span></span>
* <span data-ttu-id="a3fde-535">Une entité correspond à une ligne dans la table.</span><span class="sxs-lookup"><span data-stu-id="a3fde-535">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="a3fde-536">Comme un jeu d’entités contient plusieurs entités, les propriétés DBSet doivent être des noms au pluriel.</span><span class="sxs-lookup"><span data-stu-id="a3fde-536">Since an entity set contains multiple entities, the DBSet properties should be plural names.</span></span> <span data-ttu-id="a3fde-537">Comme l’outil de génération de modèles automatique a créé un DBSet `Student`, cette étape le remplace par le pluriel `Students`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-537">Since the scaffolding tool created a`Student` DBSet, this step changes it to plural `Students`.</span></span> 

<span data-ttu-id="a3fde-538">Pour que le :::no-loc(Razor)::: Code des pages corresponde au nouveau nom de DBSet, effectuez une modification globale sur l’ensemble du projet de `_context.Student` à `_context.Students` .</span><span class="sxs-lookup"><span data-stu-id="a3fde-538">To make the :::no-loc(Razor)::: Pages code match the new DBSet name, make a global change across the whole project of `_context.Student` to `_context.Students`.</span></span>  <span data-ttu-id="a3fde-539">Il y a 8 occurrences.</span><span class="sxs-lookup"><span data-stu-id="a3fde-539">There are 8 occurrences.</span></span>

<span data-ttu-id="a3fde-540">Générez le projet pour vérifier qu’il n’y a pas d’erreurs du compilateur.</span><span class="sxs-lookup"><span data-stu-id="a3fde-540">Build the project to verify there are no compiler errors.</span></span>

## <a name="startupcs"></a><span data-ttu-id="a3fde-541">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="a3fde-541">Startup.cs</span></span>

<span data-ttu-id="a3fde-542">ASP.NET Core comprend [l’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a3fde-542">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="a3fde-543">Certains services (comme le contexte de base de données EF Core) sont inscrits avec l’injection de dépendances au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="a3fde-543">Services (such as the EF Core database context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="a3fde-544">Ces services sont fournis par les composants qui requièrent ces services (tels que les :::no-loc(Razor)::: pages) par le biais de paramètres de constructeur.</span><span class="sxs-lookup"><span data-stu-id="a3fde-544">Components that require these services (such as :::no-loc(Razor)::: Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="a3fde-545">Le code de constructeur qui obtient une instance de contexte de base de données est indiqué plus loin dans le tutoriel.</span><span class="sxs-lookup"><span data-stu-id="a3fde-545">The constructor code that gets a database context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="a3fde-546">L’outil de génération de modèles automatique a inscrit automatiquement la classe du contexte dans le conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="a3fde-546">The scaffolding tool automatically registered the context class with the dependency injection container.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a3fde-547">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a3fde-547">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a3fde-548">Dans `ConfigureServices`, les lignes en surbrillance ont été ajoutées par l’outil de génération de modèles automatique :</span><span class="sxs-lookup"><span data-stu-id="a3fde-548">In `ConfigureServices`, the highlighted lines were added by the scaffolder:</span></span>

  [!code-csharp[Main](intro/samples/cu30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="a3fde-549">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a3fde-549">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a3fde-550">Dans `ConfigureServices`, vérifiez que le code ajouté par l’outil de génération de modèles automatique appelle `UseSqlite`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-550">In `ConfigureServices`, make sure the code added by the scaffolder calls `UseSqlite`.</span></span>

  [!code-csharp[Main](intro/samples/cu30/StartupSQLite.cs?name=snippet_ConfigureServices&highlight=5-6)]

---

<span data-ttu-id="a3fde-551">Le nom de la chaîne de connexion est transmis au contexte en appelant une méthode sur un objet [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="a3fde-551">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="a3fde-552">Pour le développement local, le [système de configuration ASP.net Core](xref:fundamentals/configuration/index) lit la chaîne de connexion à partir du *:::no-loc(appsettings.json):::* fichier.</span><span class="sxs-lookup"><span data-stu-id="a3fde-552">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *:::no-loc(appsettings.json):::* file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="a3fde-553">Création de la base de données</span><span class="sxs-lookup"><span data-stu-id="a3fde-553">Create the database</span></span>

<span data-ttu-id="a3fde-554">Mettez à jour *Program.cs* pour créer la base de données si elle n’existe pas :</span><span class="sxs-lookup"><span data-stu-id="a3fde-554">Update *Program.cs* to create the database if it doesn't exist:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Program.cs?highlight=1-2,14-18,21-38)]

<span data-ttu-id="a3fde-555">La méthode [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated) n’effectue aucune action s’il existe une base de données pour le contexte.</span><span class="sxs-lookup"><span data-stu-id="a3fde-555">The [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated) method takes no action if a database for the context exists.</span></span> <span data-ttu-id="a3fde-556">S’il n’existe pas de base de données, elle crée la base de données et le schéma.</span><span class="sxs-lookup"><span data-stu-id="a3fde-556">If no database exists, it creates the database and schema.</span></span> <span data-ttu-id="a3fde-557">`EnsureCreated` active le workflow suivant pour gérer les modifications du modèle de données :</span><span class="sxs-lookup"><span data-stu-id="a3fde-557">`EnsureCreated` enables the following workflow for handling data model changes:</span></span>

* <span data-ttu-id="a3fde-558">Supprimez la base de données.</span><span class="sxs-lookup"><span data-stu-id="a3fde-558">Delete the database.</span></span> <span data-ttu-id="a3fde-559">Toutes les données existantes sont perdues.</span><span class="sxs-lookup"><span data-stu-id="a3fde-559">Any existing data is lost.</span></span>
* <span data-ttu-id="a3fde-560">Modifiez le modèle de données.</span><span class="sxs-lookup"><span data-stu-id="a3fde-560">Change the data model.</span></span> <span data-ttu-id="a3fde-561">Par exemple, ajoutez un champ `EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-561">For example, add an `EmailAddress` field.</span></span>
* <span data-ttu-id="a3fde-562">Exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="a3fde-562">Run the app.</span></span>
* <span data-ttu-id="a3fde-563">`EnsureCreated` crée une base de données avec le nouveau schéma.</span><span class="sxs-lookup"><span data-stu-id="a3fde-563">`EnsureCreated` creates a database with the new schema.</span></span>

<span data-ttu-id="a3fde-564">Ce workflow fonctionne bien à un stade précoce du développement, quand le schéma évolue rapidement, aussi longtemps que vous n’avez pas besoin de conserver les données.</span><span class="sxs-lookup"><span data-stu-id="a3fde-564">This workflow works well early in development when the schema is rapidly evolving, as long as you don't need to preserve data.</span></span> <span data-ttu-id="a3fde-565">La situation est différente quand les données qui ont été entrées dans la base de données doivent être conservées.</span><span class="sxs-lookup"><span data-stu-id="a3fde-565">The situation is different when data that has been entered into the database needs to be preserved.</span></span> <span data-ttu-id="a3fde-566">Dans ce cas, procédez à des migrations.</span><span class="sxs-lookup"><span data-stu-id="a3fde-566">When that is the case, use migrations.</span></span>

<span data-ttu-id="a3fde-567">Plus tard dans cette série de tutoriels, vous supprimerez la base de données créée par `EnsureCreated` et procéderez à des migrations.</span><span class="sxs-lookup"><span data-stu-id="a3fde-567">Later in the tutorial series, you delete the database that was created by `EnsureCreated` and use migrations instead.</span></span> <span data-ttu-id="a3fde-568">Une base de données créée par `EnsureCreated` ne peut pas être mise à jour via des migrations.</span><span class="sxs-lookup"><span data-stu-id="a3fde-568">A database that is created by `EnsureCreated` can't be updated by using migrations.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="a3fde-569">Tester l’application</span><span class="sxs-lookup"><span data-stu-id="a3fde-569">Test the app</span></span>

* <span data-ttu-id="a3fde-570">Exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="a3fde-570">Run the app.</span></span>
* <span data-ttu-id="a3fde-571">Sélectionnez le lien **Students** , puis **Créer nouveau**.</span><span class="sxs-lookup"><span data-stu-id="a3fde-571">Select the **Students** link and then **Create New**.</span></span>
* <span data-ttu-id="a3fde-572">Testez les liens Edit, Details et Delete.</span><span class="sxs-lookup"><span data-stu-id="a3fde-572">Test the Edit, Details, and Delete links.</span></span>

## <a name="seed-the-database"></a><span data-ttu-id="a3fde-573">Amorcer la base de données</span><span class="sxs-lookup"><span data-stu-id="a3fde-573">Seed the database</span></span>

<span data-ttu-id="a3fde-574">La méthode `EnsureCreated` crée une base de données vide.</span><span class="sxs-lookup"><span data-stu-id="a3fde-574">The `EnsureCreated` method creates an empty database.</span></span> <span data-ttu-id="a3fde-575">Cette section ajoute du code qui remplit la base de données avec des données de test.</span><span class="sxs-lookup"><span data-stu-id="a3fde-575">This section adds code that populates the database with test data.</span></span>

<span data-ttu-id="a3fde-576">Créez *Data/DbInitializer.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a3fde-576">Create *Data/DbInitializer.cs* with the following code:</span></span>
<!-- next update, keep this file in the project and surround with #if -->
  [!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Data/DbInitializer.cs)]

  <span data-ttu-id="a3fde-577">Le code vérifie si des étudiants figurent dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="a3fde-577">The code checks if there are any students in the database.</span></span> <span data-ttu-id="a3fde-578">S’il n’y a pas d’étudiants, il ajoute des données de test à la base de données.</span><span class="sxs-lookup"><span data-stu-id="a3fde-578">If there are no students, it adds test data to the database.</span></span> <span data-ttu-id="a3fde-579">Il crée les données de test dans des tableaux et non dans des collections `List<T>` afin d’optimiser les performances.</span><span class="sxs-lookup"><span data-stu-id="a3fde-579">It creates the test data in arrays rather than `List<T>` collections to optimize performance.</span></span>

* <span data-ttu-id="a3fde-580">Dans *Program.cs* , remplacez l’appel `EnsureCreated` par un appel `DbInitializer.Initialize` :</span><span class="sxs-lookup"><span data-stu-id="a3fde-580">In *Program.cs* , replace the `EnsureCreated` call with a `DbInitializer.Initialize` call:</span></span>

  ```csharp
  // context.Database.EnsureCreated();
  DbInitializer.Initialize(context);
  ```

# <a name="visual-studio"></a>[<span data-ttu-id="a3fde-581">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a3fde-581">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a3fde-582">Arrêtez l’application si elle est en cours d’exécution et exécutez la commande suivante dans la **Console du gestionnaire de package**  :</span><span class="sxs-lookup"><span data-stu-id="a3fde-582">Stop the app if it's running, and run the following command in the **Package Manager Console** (PMC):</span></span>

```powershell
Drop-Database
```

# <a name="visual-studio-code"></a>[<span data-ttu-id="a3fde-583">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a3fde-583">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a3fde-584">Arrêtez l’application si elle est en cours d’exécution, puis supprimez le fichier *CU.db*.</span><span class="sxs-lookup"><span data-stu-id="a3fde-584">Stop the app if it's running, and delete the *CU.db* file.</span></span>

---

* <span data-ttu-id="a3fde-585">Redémarrez l’application.</span><span class="sxs-lookup"><span data-stu-id="a3fde-585">Restart the app.</span></span>

* <span data-ttu-id="a3fde-586">Sélectionnez la page Students pour examiner les données amorcées.</span><span class="sxs-lookup"><span data-stu-id="a3fde-586">Select the Students page to see the seeded data.</span></span>

## <a name="view-the-database"></a><span data-ttu-id="a3fde-587">Afficher la base de données</span><span class="sxs-lookup"><span data-stu-id="a3fde-587">View the database</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a3fde-588">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a3fde-588">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a3fde-589">Ouvrez **l’Explorateur d’objets SQL Server** (SSOX) à partir du menu **Affichage** de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a3fde-589">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
* <span data-ttu-id="a3fde-590">Dans SSOX, sélectionnez **(localdb)\MSSQLLocalDB > Databases > SchoolContext-{GUID}**.</span><span class="sxs-lookup"><span data-stu-id="a3fde-590">In SSOX, select **(localdb)\MSSQLLocalDB > Databases > SchoolContext-{GUID}**.</span></span> <span data-ttu-id="a3fde-591">Le nom de la base de données est généré à partir du nom de contexte indiqué précédemment, ainsi que d’un tiret et d’un GUID.</span><span class="sxs-lookup"><span data-stu-id="a3fde-591">The database name is generated from the context name you provided earlier plus a dash and a GUID.</span></span>
* <span data-ttu-id="a3fde-592">Développez le nœud **Tables**.</span><span class="sxs-lookup"><span data-stu-id="a3fde-592">Expand the **Tables** node.</span></span>
* <span data-ttu-id="a3fde-593">Cliquez avec le bouton droit sur la table **Student** et cliquez sur **Afficher les données** pour voir les colonnes créées et les lignes insérées dans la table.</span><span class="sxs-lookup"><span data-stu-id="a3fde-593">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>
* <span data-ttu-id="a3fde-594">Cliquez avec le bouton droit sur la table **Student** et cliquez sur **Afficher le code** pour voir comment le modèle `Student` est mappé au schéma de la table `Student`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-594">Right-click the **Student** table and click **View Code** to see how the `Student` model maps to the `Student` table schema.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="a3fde-595">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a3fde-595">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a3fde-596">Utilisez votre outil SQLite pour examiner le schéma de base de données et les données amorcées.</span><span class="sxs-lookup"><span data-stu-id="a3fde-596">Use your SQLite tool to view the database schema and seeded data.</span></span> <span data-ttu-id="a3fde-597">Le fichier de base de données est nommé *CU.db* et se trouve dans le dossier du projet.</span><span class="sxs-lookup"><span data-stu-id="a3fde-597">The database file is named *CU.db* and is located in the project folder.</span></span>

---

## <a name="asynchronous-code"></a><span data-ttu-id="a3fde-598">Code asynchrone</span><span class="sxs-lookup"><span data-stu-id="a3fde-598">Asynchronous code</span></span>

<span data-ttu-id="a3fde-599">La programmation asynchrone est le mode par défaut pour ASP.NET Core et EF Core.</span><span class="sxs-lookup"><span data-stu-id="a3fde-599">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="a3fde-600">Un serveur web a un nombre limité de threads disponibles et, dans les situations de forte charge, tous les threads disponibles peuvent être utilisés.</span><span class="sxs-lookup"><span data-stu-id="a3fde-600">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="a3fde-601">Quand cela se produit, le serveur ne peut pas traiter de nouvelle requête tant que les threads ne sont pas libérés.</span><span class="sxs-lookup"><span data-stu-id="a3fde-601">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="a3fde-602">Avec le code synchrone, plusieurs threads peuvent être bloqués alors qu’ils n’effectuent en fait aucun travail, car ils attendent que des E/S se terminent.</span><span class="sxs-lookup"><span data-stu-id="a3fde-602">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="a3fde-603">Avec le code asynchrone, quand un processus attend que des E/S se terminent, son thread est libéré afin d’être utilisé par le serveur pour traiter d’autres demandes.</span><span class="sxs-lookup"><span data-stu-id="a3fde-603">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="a3fde-604">Il permet ainsi d’utiliser les ressources serveur plus efficacement, et le serveur peut gérer plus de trafic sans retard.</span><span class="sxs-lookup"><span data-stu-id="a3fde-604">As a result, asynchronous code enables server resources to be used more efficiently, and the server can handle more traffic without delays.</span></span>

<span data-ttu-id="a3fde-605">Le code asynchrone introduit néanmoins une petite surcharge au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="a3fde-605">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="a3fde-606">Dans les situations de faible trafic, le gain de performances est négligeable, tandis qu’en cas de trafic élevé l’amélioration potentielle des performances est importante.</span><span class="sxs-lookup"><span data-stu-id="a3fde-606">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="a3fde-607">Dans le code suivant, le mot clé [async](/dotnet/csharp/language-reference/keywords/async), la valeur renvoyée `Task<T>`, le mot clé `await` et la méthode `ToListAsync` déclenchent l’exécution asynchrone du code.</span><span class="sxs-lookup"><span data-stu-id="a3fde-607">In the following code, the [async](/dotnet/csharp/language-reference/keywords/async) keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

```csharp
public async Task OnGetAsync()
{
    Students = await _context.Students.ToListAsync();
}
```

* <span data-ttu-id="a3fde-608">Le mot clé `async` fait en sorte que le compilateur :</span><span class="sxs-lookup"><span data-stu-id="a3fde-608">The `async` keyword tells the compiler to:</span></span>
  * <span data-ttu-id="a3fde-609">Génère des rappels pour les parties du corps de méthode.</span><span class="sxs-lookup"><span data-stu-id="a3fde-609">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="a3fde-610">Crée l’objet [Task](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType) qui est retourné.</span><span class="sxs-lookup"><span data-stu-id="a3fde-610">Create the [Task](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType) object that's returned.</span></span>
* <span data-ttu-id="a3fde-611">Le type de retour `Task<T>` représente le travail en cours.</span><span class="sxs-lookup"><span data-stu-id="a3fde-611">The `Task<T>` return type represents ongoing work.</span></span>
* <span data-ttu-id="a3fde-612">Le mot clé `await` indique au compilateur de fractionner la méthode en deux parties.</span><span class="sxs-lookup"><span data-stu-id="a3fde-612">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="a3fde-613">La première partie se termine par l’opération qui est démarrée de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="a3fde-613">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="a3fde-614">La seconde partie est placée dans une méthode de rappel qui est appelée quand l’opération se termine.</span><span class="sxs-lookup"><span data-stu-id="a3fde-614">The second part is put into a callback method that's called when the operation completes.</span></span>
* <span data-ttu-id="a3fde-615">`ToListAsync` est la version asynchrone de la méthode d’extension `ToList`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-615">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="a3fde-616">Voici quelques éléments à connaître lors de l’écriture de code asynchrone qui utilise EF Core :</span><span class="sxs-lookup"><span data-stu-id="a3fde-616">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="a3fde-617">Seules les instructions qui provoquent l’envoi de requêtes ou de commandes vers la base de données sont exécutées de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="a3fde-617">Only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="a3fde-618">Cela inclut `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync` et `SaveChangesAsync`,</span><span class="sxs-lookup"><span data-stu-id="a3fde-618">That includes `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="a3fde-619">mais pas les instructions qui ne font que changer un `IQueryable`, telles que `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-619">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>
* <span data-ttu-id="a3fde-620">Un contexte EF Core n’est pas thread-safe : n’essayez pas d’effectuer plusieurs opérations en parallèle.</span><span class="sxs-lookup"><span data-stu-id="a3fde-620">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span>
* <span data-ttu-id="a3fde-621">Pour tirer parti des avantages de performances du code asynchrone, vérifiez que les packages de bibliothèque (par exemple pour la pagination) utilisent le mode asynchrone s’ils appellent des méthodes EF Core qui envoient des requêtes à la base de données.</span><span class="sxs-lookup"><span data-stu-id="a3fde-621">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the database.</span></span>

<span data-ttu-id="a3fde-622">Pour plus d’informations sur la programmation asynchrone dans .NET, consultez [Vue d’ensemble d’Async](/dotnet/standard/async) et [Programmation asynchrone avec async et await](/dotnet/csharp/programming-guide/concepts/async/).</span><span class="sxs-lookup"><span data-stu-id="a3fde-622">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/standard/async) and [Asynchronous programming with async and await](/dotnet/csharp/programming-guide/concepts/async/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a3fde-623">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="a3fde-623">Next steps</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a3fde-624">Didacticiel suivant</span><span class="sxs-lookup"><span data-stu-id="a3fde-624">Next tutorial</span></span>](xref:data/ef-rp/crud)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a3fde-625">L’exemple d’application Web Contoso University montre comment créer une :::no-loc(Razor)::: application ASP.net Core pages à l’aide d’Entity Framework (EF) Core.</span><span class="sxs-lookup"><span data-stu-id="a3fde-625">The Contoso University sample web app demonstrates how to create an ASP.NET Core :::no-loc(Razor)::: Pages app using Entity Framework (EF) Core.</span></span>

<span data-ttu-id="a3fde-626">L’exemple d’application est un site web pour une université Contoso fictive.</span><span class="sxs-lookup"><span data-stu-id="a3fde-626">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="a3fde-627">Il comprend des fonctionnalités telles que l’admission des étudiants, la création des cours et les affectations des formateurs.</span><span class="sxs-lookup"><span data-stu-id="a3fde-627">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="a3fde-628">Cette page est la première d’une série de didacticiels qui expliquent comment générer l’exemple d’application Contoso University.</span><span class="sxs-lookup"><span data-stu-id="a3fde-628">This page is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

[<span data-ttu-id="a3fde-629">Télécharger ou afficher l’application terminée.</span><span class="sxs-lookup"><span data-stu-id="a3fde-629">Download or view the completed app.</span></span>](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="a3fde-630">[Instructions de téléchargement](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="a3fde-630">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a3fde-631">Prérequis</span><span class="sxs-lookup"><span data-stu-id="a3fde-631">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a3fde-632">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a3fde-632">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="a3fde-633">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a3fde-633">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [](~/includes/2.1-SDK.md)]

---

<span data-ttu-id="a3fde-634">Vous êtes familiarisé avec les [ :::no-loc(Razor)::: pages](xref:razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="a3fde-634">Familiarity with [:::no-loc(Razor)::: Pages](xref:razor-pages/index).</span></span> <span data-ttu-id="a3fde-635">Les nouveaux programmeurs doivent effectuer la [prise en main des :::no-loc(Razor)::: pages avant de](xref:tutorials/razor-pages/razor-pages-start) commencer cette série.</span><span class="sxs-lookup"><span data-stu-id="a3fde-635">New programmers should complete [Get started with :::no-loc(Razor)::: Pages](xref:tutorials/razor-pages/razor-pages-start) before starting this series.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="a3fde-636">Résolution des problèmes</span><span class="sxs-lookup"><span data-stu-id="a3fde-636">Troubleshooting</span></span>

<span data-ttu-id="a3fde-637">Si vous rencontrez un problème que vous ne pouvez pas résoudre, vous pouvez généralement trouver la solution en comparant votre code au [projet terminé](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="a3fde-637">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed project](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span> <span data-ttu-id="a3fde-638">Un bon moyen d’obtenir de l’aide est de publier une question sur [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) pour [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) ou [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="a3fde-638">A good way to get help is by posting a question to [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

## <a name="the-contoso-university-web-app"></a><span data-ttu-id="a3fde-639">L’application web Contoso University</span><span class="sxs-lookup"><span data-stu-id="a3fde-639">The Contoso University web app</span></span>

<span data-ttu-id="a3fde-640">L’application générée dans ces didacticiels est le site web de base d’une université.</span><span class="sxs-lookup"><span data-stu-id="a3fde-640">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="a3fde-641">Les utilisateurs peuvent afficher et mettre à jour les informations relatives aux étudiants, aux cours et aux formateurs.</span><span class="sxs-lookup"><span data-stu-id="a3fde-641">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="a3fde-642">Voici quelques-uns des écrans créés dans le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="a3fde-642">Here are a few of the screens created in the tutorial.</span></span>

![Page d’index des étudiants](intro/_static/students-index.png)

![Page de modification des étudiants](intro/_static/student-edit.png)

<span data-ttu-id="a3fde-645">Le style de l’interface utilisateur de ce site est proche de ce qui est généré par les modèles prédéfinis.</span><span class="sxs-lookup"><span data-stu-id="a3fde-645">The UI style of this site is close to what's generated by the built-in templates.</span></span> <span data-ttu-id="a3fde-646">Ce didacticiel se concentre sur EF Core avec :::no-loc(Razor)::: les pages, et non sur l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="a3fde-646">The tutorial focus is on EF Core with :::no-loc(Razor)::: Pages, not the UI.</span></span>

## <a name="create-the-contosouniversity-no-locrazor-pages-web-app"></a><span data-ttu-id="a3fde-647">Créer l' :::no-loc(Razor)::: application Web ContosoUniversity pages</span><span class="sxs-lookup"><span data-stu-id="a3fde-647">Create the ContosoUniversity :::no-loc(Razor)::: Pages web app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a3fde-648">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a3fde-648">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a3fde-649">Dans Visual Studio, dans le menu **Fichier** , sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="a3fde-649">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="a3fde-650">Créez une application web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a3fde-650">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="a3fde-651">Nommez le projet **ContosoUniversity**.</span><span class="sxs-lookup"><span data-stu-id="a3fde-651">Name the project **ContosoUniversity**.</span></span> <span data-ttu-id="a3fde-652">Il est important de nommer le projet *ContosoUniversity* afin que les espaces de noms correspondent quand le code est copié/collé.</span><span class="sxs-lookup"><span data-stu-id="a3fde-652">It's important to name the project *ContosoUniversity* so the namespaces match when code is copy/pasted.</span></span>
* <span data-ttu-id="a3fde-653">Sélectionnez **ASP.NET Core 2.1** dans la liste déroulante, puis sélectionnez **Application web**.</span><span class="sxs-lookup"><span data-stu-id="a3fde-653">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

<span data-ttu-id="a3fde-654">Pour obtenir des images des étapes précédentes, consultez [créer une :::no-loc(Razor)::: application Web](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-pages-web-app).</span><span class="sxs-lookup"><span data-stu-id="a3fde-654">For images of the preceding steps, see [Create a :::no-loc(Razor)::: web app](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-pages-web-app).</span></span>
<span data-ttu-id="a3fde-655">Exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="a3fde-655">Run the app.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="a3fde-656">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a3fde-656">Visual Studio Code</span></span>](#tab/visual-studio-code)

```dotnetcli
dotnet new webapp -o ContosoUniversity
cd ContosoUniversity
dotnet run
```

---

## <a name="set-up-the-site-style"></a><span data-ttu-id="a3fde-657">Configurer le style du site</span><span class="sxs-lookup"><span data-stu-id="a3fde-657">Set up the site style</span></span>

<span data-ttu-id="a3fde-658">Quelques changements permettent de définir le menu, la disposition et la page d’accueil du site.</span><span class="sxs-lookup"><span data-stu-id="a3fde-658">A few changes set up the site menu, layout, and home page.</span></span> <span data-ttu-id="a3fde-659">Mettez à jour *Pages/Shared/_Layout.cshtml* avec les changements suivants :</span><span class="sxs-lookup"><span data-stu-id="a3fde-659">Update *Pages/Shared/_Layout.cshtml* with the following changes:</span></span>

* <span data-ttu-id="a3fde-660">Remplacez chaque occurrence de « ContosoUniversity » par « Contoso University ».</span><span class="sxs-lookup"><span data-stu-id="a3fde-660">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="a3fde-661">Il y a trois occurrences.</span><span class="sxs-lookup"><span data-stu-id="a3fde-661">There are three occurrences.</span></span>

* <span data-ttu-id="a3fde-662">Ajoutez des entrées de menu pour **Students** , **Courses** , **Instructors** et **Departments** , et supprimez l’entrée de menu **Contact**.</span><span class="sxs-lookup"><span data-stu-id="a3fde-662">Add menu entries for **Students** , **Courses** , **Instructors** , and **Departments** , and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="a3fde-663">Les modifications sont mises en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="a3fde-663">The changes are highlighted.</span></span> <span data-ttu-id="a3fde-664">(Tout le balisage n’est *pas* affiché.)</span><span class="sxs-lookup"><span data-stu-id="a3fde-664">(All the markup is *not* displayed.)</span></span>

[!code-cshtml[](intro/samples/cu21/Pages/Shared/_Layout.cshtml?highlight=6,29,35-38,50&name=snippet)]

<span data-ttu-id="a3fde-665">Dans *Pages/Index.cshtml* , remplacez le contenu du fichier par le code suivant afin de remplacer le texte sur ASP.NET et MVC par le texte concernant cette application :</span><span class="sxs-lookup"><span data-stu-id="a3fde-665">In *Pages/Index.cshtml* , replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this app:</span></span>

[!code-cshtml[](intro/samples/cu21/Pages/Index.cshtml)]

## <a name="create-the-data-model"></a><span data-ttu-id="a3fde-666">Créer le modèle de données</span><span class="sxs-lookup"><span data-stu-id="a3fde-666">Create the data model</span></span>

<span data-ttu-id="a3fde-667">Créez des classes d’entités pour l’application Contoso University.</span><span class="sxs-lookup"><span data-stu-id="a3fde-667">Create entity classes for the Contoso University app.</span></span> <span data-ttu-id="a3fde-668">Commencez par les trois entités suivantes :</span><span class="sxs-lookup"><span data-stu-id="a3fde-668">Start with the following three entities:</span></span>

![Diagramme du modèle de données Course-Enrollment-Student](intro/_static/data-model-diagram.png)

<span data-ttu-id="a3fde-670">Il existe une relation un-à-plusieurs entre les entités `Student` et `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-670">There's a one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="a3fde-671">Il existe une relation un-à-plusieurs entre les entités `Course` et `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-671">There's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="a3fde-672">Un étudiant peut s’inscrire à autant de cours qu’il le souhaite.</span><span class="sxs-lookup"><span data-stu-id="a3fde-672">A student can enroll in any number of courses.</span></span> <span data-ttu-id="a3fde-673">Un cours peut avoir une quantité illimitée d’élèves inscrits.</span><span class="sxs-lookup"><span data-stu-id="a3fde-673">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="a3fde-674">Dans les sections suivantes, une classe pour chacune de ces entités est créée.</span><span class="sxs-lookup"><span data-stu-id="a3fde-674">In the following sections, a class for each one of these entities is created.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="a3fde-675">L’entité Student</span><span class="sxs-lookup"><span data-stu-id="a3fde-675">The Student entity</span></span>

![Diagramme de l’entité Student](intro/_static/student-entity.png)

<span data-ttu-id="a3fde-677">Créez un dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="a3fde-677">Create a *Models* folder.</span></span> <span data-ttu-id="a3fde-678">Dans le dossier *Models* , créez un fichier de classe nommé *Student.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a3fde-678">In the *Models* folder, create a class file named *Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="a3fde-679">La propriété `ID` devient la colonne de clé primaire de la table de base de données qui correspond à cette classe.</span><span class="sxs-lookup"><span data-stu-id="a3fde-679">The `ID` property becomes the primary key column of the database (DB) table that corresponds to this class.</span></span> <span data-ttu-id="a3fde-680">Par défaut, EF Core interprète une propriété nommée `ID` ou `classnameID` comme clé primaire.</span><span class="sxs-lookup"><span data-stu-id="a3fde-680">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span> <span data-ttu-id="a3fde-681">Dans `classnameID`, `classname` est le nom de la classe.</span><span class="sxs-lookup"><span data-stu-id="a3fde-681">In `classnameID`, `classname` is the name of the class.</span></span> <span data-ttu-id="a3fde-682">L’autre clé primaire reconnue automatiquement est `StudentID` dans l’exemple précédent.</span><span class="sxs-lookup"><span data-stu-id="a3fde-682">The alternative automatically recognized primary key is `StudentID` in the preceding example.</span></span>

<span data-ttu-id="a3fde-683">La `Enrollments` propriété est une [propriété de navigation](/ef/core/modeling/relationships).</span><span class="sxs-lookup"><span data-stu-id="a3fde-683">The `Enrollments` property is a [navigation property](/ef/core/modeling/relationships).</span></span> <span data-ttu-id="a3fde-684">Les propriétés de navigation établissent des liaisons à d’autres entités qui sont associées à cette entité.</span><span class="sxs-lookup"><span data-stu-id="a3fde-684">Navigation properties link to other entities that are related to this entity.</span></span> <span data-ttu-id="a3fde-685">Ici, la propriété `Enrollments` d’un `Student entity` contient toutes les entités `Enrollment` associées à ce `Student`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-685">In this case, the `Enrollments` property of a `Student entity` holds all of the `Enrollment` entities that are related to that `Student`.</span></span> <span data-ttu-id="a3fde-686">Par exemple, si une ligne Student dans la base de données a deux lignes Enrollment associées, la propriété de navigation `Enrollments` contient ces deux entités `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-686">For example, if a Student row in the DB has two related Enrollment rows, the `Enrollments` navigation property contains those two `Enrollment` entities.</span></span> <span data-ttu-id="a3fde-687">Une ligne `Enrollment` associée est une ligne qui contient la valeur de clé primaire de cette étudiant dans la colonne `StudentID`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-687">A related `Enrollment` row is a row that contains that student's primary key value in the `StudentID` column.</span></span> <span data-ttu-id="a3fde-688">Par exemple, supposez que l’étudiant avec ID=1 a deux lignes dans la table `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-688">For example, suppose the student with ID=1 has two rows in the `Enrollment` table.</span></span> <span data-ttu-id="a3fde-689">La table `Enrollment` a deux lignes avec `StudentID` = 1.</span><span class="sxs-lookup"><span data-stu-id="a3fde-689">The `Enrollment` table has two rows with `StudentID` = 1.</span></span> <span data-ttu-id="a3fde-690">`StudentID` est une clé étrangère dans la table `Enrollment` qui spécifie l’étudiant dans la table `Student`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-690">`StudentID` is a foreign key in the `Enrollment` table that specifies the student in the `Student` table.</span></span>

<span data-ttu-id="a3fde-691">Si une propriété de navigation peut contenir plusieurs entités, la propriété de navigation doit être un type de liste, tel que `ICollection<T>`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-691">If a navigation property can hold multiple entities, the navigation property must be a list type, such as `ICollection<T>`.</span></span> <span data-ttu-id="a3fde-692">Vous pouvez spécifier `ICollection<T>`, ou un type tel que `List<T>` ou `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-692">`ICollection<T>` can be specified, or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="a3fde-693">Quand vous utilisez `ICollection<T>`, EF Core crée une collection `HashSet<T>` par défaut.</span><span class="sxs-lookup"><span data-stu-id="a3fde-693">When `ICollection<T>` is used, EF Core creates a `HashSet<T>` collection by default.</span></span> <span data-ttu-id="a3fde-694">Les propriétés de navigation qui contiennent plusieurs entités proviennent de relations plusieurs-à-plusieurs et un-à-plusieurs.</span><span class="sxs-lookup"><span data-stu-id="a3fde-694">Navigation properties that hold multiple entities come from many-to-many and one-to-many relationships.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="a3fde-695">L’entité Enrollment</span><span class="sxs-lookup"><span data-stu-id="a3fde-695">The Enrollment entity</span></span>

![Diagramme de l’entité Enrollment](intro/_static/enrollment-entity.png)

<span data-ttu-id="a3fde-697">Dans le dossier *Models* , créez un fichier *Enrollment.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a3fde-697">In the *Models* folder, create *Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="a3fde-698">La propriété `EnrollmentID` est la clé primaire.</span><span class="sxs-lookup"><span data-stu-id="a3fde-698">The `EnrollmentID` property is the primary key.</span></span> <span data-ttu-id="a3fde-699">Cette entité utilise le modèle `classnameID` au lieu de `ID` comme l’entité `Student`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-699">This entity uses the `classnameID` pattern instead of `ID` like the `Student` entity.</span></span> <span data-ttu-id="a3fde-700">En général, les développeurs choisissent un modèle et l’utilisent dans tout le modèle de données.</span><span class="sxs-lookup"><span data-stu-id="a3fde-700">Typically developers choose one pattern and use it throughout the data model.</span></span> <span data-ttu-id="a3fde-701">Dans un prochain didacticiel, nous verrons qu’utiliser ID sans classname simplifie l’implémentation de l’héritage dans le modèle de données.</span><span class="sxs-lookup"><span data-stu-id="a3fde-701">In a later tutorial, using ID without classname is shown to make it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="a3fde-702">La propriété `Grade` est un `enum`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-702">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="a3fde-703">Le point d’interrogation après la déclaration de type `Grade` indique que la propriété `Grade` est nullable.</span><span class="sxs-lookup"><span data-stu-id="a3fde-703">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="a3fde-704">Une note (Grade) qui a la valeur Null est différente d’une note égale à zéro : la valeur Null signifie qu’une note n’est pas connue ou n’a pas encore été affectée.</span><span class="sxs-lookup"><span data-stu-id="a3fde-704">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="a3fde-705">La propriété `StudentID` est une clé étrangère, et la propriété de navigation correspondante est `Student`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-705">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="a3fde-706">Une entité `Enrollment` est associée à une entité `Student`. Par conséquent, la propriété contient une seule entité `Student`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-706">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span> <span data-ttu-id="a3fde-707">L’entité `Student` diffère de la propriété de navigation `Student.Enrollments`, qui contient plusieurs entités `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-707">The `Student` entity differs from the `Student.Enrollments` navigation property, which contains multiple `Enrollment` entities.</span></span>

<span data-ttu-id="a3fde-708">La propriété `CourseID` est une clé étrangère, et la propriété de navigation correspondante est `Course`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-708">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="a3fde-709">Une entité `Enrollment` est associée à une entité `Course`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-709">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="a3fde-710">EF Core interprète une propriété en tant que clé étrangère si elle se nomme `<navigation property name><primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-710">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="a3fde-711">Par exemple, `StudentID` pour la propriété de navigation `Student`, puisque la clé primaire de l’entité `Student` est `ID`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-711">For example,`StudentID` for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="a3fde-712">Les propriétés de clé étrangère peuvent également se nommer `<primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-712">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="a3fde-713">Par exemple, `CourseID` puisque la clé primaire de l’entité `Course` est `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-713">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="a3fde-714">L’entité Course</span><span class="sxs-lookup"><span data-stu-id="a3fde-714">The Course entity</span></span>

![Diagramme de l’entité Course](intro/_static/course-entity.png)

<span data-ttu-id="a3fde-716">Dans le dossier *Models* , créez un fichier *Course.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a3fde-716">In the *Models* folder, create *Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="a3fde-717">La propriété `Enrollments` est une propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="a3fde-717">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="a3fde-718">Une entité `Course` peut être associée à un nombre quelconque d’entités `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-718">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="a3fde-719">L’attribut `DatabaseGenerated` permet à l’application de spécifier la clé primaire, plutôt que de la faire générer par la base de données.</span><span class="sxs-lookup"><span data-stu-id="a3fde-719">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the DB generate it.</span></span>

## <a name="scaffold-the-student-model"></a><span data-ttu-id="a3fde-720">Générer automatiquement le modèle d’étudiant</span><span class="sxs-lookup"><span data-stu-id="a3fde-720">Scaffold the student model</span></span>

<span data-ttu-id="a3fde-721">Dans cette section, le modèle d’étudiant est généré automatiquement.</span><span class="sxs-lookup"><span data-stu-id="a3fde-721">In this section, the student model is scaffolded.</span></span> <span data-ttu-id="a3fde-722">Autrement dit, l’outil de génération de modèles automatique génère des pages pour les opérations de création (Create), de lecture (Read), de mise à jour (Update) et de suppression (Delete) (CRUD) pour le modèle d’étudiant.</span><span class="sxs-lookup"><span data-stu-id="a3fde-722">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the student model.</span></span>

* <span data-ttu-id="a3fde-723">Créez le projet.</span><span class="sxs-lookup"><span data-stu-id="a3fde-723">Build the project.</span></span>
* <span data-ttu-id="a3fde-724">Créez le dossier *Pages/Students*.</span><span class="sxs-lookup"><span data-stu-id="a3fde-724">Create the *Pages/Students* folder.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a3fde-725">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a3fde-725">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a3fde-726">Dans **l’Explorateur de solutions** , cliquez avec le bouton droit sur le dossier *Pages/Students* > **Ajouter** > **Nouvel élément généré automatiquement**.</span><span class="sxs-lookup"><span data-stu-id="a3fde-726">In **Solution Explorer** , right click on the *Pages/Students* folder > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="a3fde-727">Dans la boîte de dialogue **Ajouter une structure** , sélectionnez **:::no-loc(Razor)::: pages à l’aide de Entity Framework (CRUD)** > **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="a3fde-727">In the **Add Scaffold** dialog, select **:::no-loc(Razor)::: Pages using Entity Framework (CRUD)** > **ADD**.</span></span>

<span data-ttu-id="a3fde-728">Complétez la boîte de dialogue **Ajouter des :::no-loc(Razor)::: pages à l’aide de Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="a3fde-728">Complete the **Add :::no-loc(Razor)::: Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="a3fde-729">Dans la liste déroulante **Classe de modèle** , sélectionnez **Student (ContosoUniversity.Models)**.</span><span class="sxs-lookup"><span data-stu-id="a3fde-729">In the **Model class** drop-down, select **Student (ContosoUniversity.Models)**.</span></span>
* <span data-ttu-id="a3fde-730">Dans la ligne **Classe de contexte de données** , sélectionnez le signe **+** (plus) et remplacez le nom généré par **ContosoUniversity.Models.SchoolContext**.</span><span class="sxs-lookup"><span data-stu-id="a3fde-730">In the **Data context class** row, select the **+** (plus) sign and change the generated name to **ContosoUniversity.Models.SchoolContext**.</span></span>
* <span data-ttu-id="a3fde-731">Dans la liste déroulante **Classe de contexte de données** , sélectionnez **ContosoUniversity.Models.SchoolContext**</span><span class="sxs-lookup"><span data-stu-id="a3fde-731">In the **Data context class** drop-down,  select **ContosoUniversity.Models.SchoolContext**</span></span>
* <span data-ttu-id="a3fde-732">Sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="a3fde-732">Select **Add**.</span></span>

![Boîte de dialogue CRUD](intro/_static/s1.png)

<span data-ttu-id="a3fde-734">Consultez [Générer automatiquement le modèle de film](xref:tutorials/razor-pages/model#scaffold-the-movie-model) si vous rencontrez un problème à l’étape précédente.</span><span class="sxs-lookup"><span data-stu-id="a3fde-734">See [Scaffold the movie model](xref:tutorials/razor-pages/model#scaffold-the-movie-model) if you have a problem with the preceding step.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="a3fde-735">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a3fde-735">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a3fde-736">Exécutez les commandes suivantes pour générer automatiquement le modèle d’étudiant.</span><span class="sxs-lookup"><span data-stu-id="a3fde-736">Run the following commands to scaffold the student model.</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Models.SchoolContext -udl -outDir Pages/Students --referenceScriptLibraries
```

---

<span data-ttu-id="a3fde-737">Le processus de génération de modèles automatique a créé et changé les fichiers suivants :</span><span class="sxs-lookup"><span data-stu-id="a3fde-737">The scaffold process created and changed the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="a3fde-738">Fichiers créés</span><span class="sxs-lookup"><span data-stu-id="a3fde-738">Files created</span></span>

* <span data-ttu-id="a3fde-739">*Pages/Students* Create, Delete, Details, Edit, Index.</span><span class="sxs-lookup"><span data-stu-id="a3fde-739">*Pages/Students* Create, Delete, Details, Edit, Index.</span></span>
* <span data-ttu-id="a3fde-740">*Data/SchoolContext.cs*</span><span class="sxs-lookup"><span data-stu-id="a3fde-740">*Data/SchoolContext.cs*</span></span>

### <a name="file-updates"></a><span data-ttu-id="a3fde-741">Mises à jour du fichier</span><span class="sxs-lookup"><span data-stu-id="a3fde-741">File updates</span></span>

* <span data-ttu-id="a3fde-742">*Startup.cs*  : Les changements apportés à ce fichier sont détaillés dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="a3fde-742">*Startup.cs* : Changes to this file are detailed in the next section.</span></span>
* <span data-ttu-id="a3fde-743">*:::no-loc(appsettings.json):::* : La chaîne de connexion utilisée pour se connecter à une base de données locale est ajoutée.</span><span class="sxs-lookup"><span data-stu-id="a3fde-743">*:::no-loc(appsettings.json):::* : The connection string used to connect to a local database is added.</span></span>

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="a3fde-744">Examiner le contexte inscrit avec l’injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="a3fde-744">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="a3fde-745">ASP.NET Core comprend [l’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a3fde-745">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="a3fde-746">Des services (tels que le contexte de base de données EF Core) sont inscrits avec l’injection de dépendances au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="a3fde-746">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="a3fde-747">Ces services sont fournis par les composants qui requièrent ces services (tels que les :::no-loc(Razor)::: pages) par le biais de paramètres de constructeur.</span><span class="sxs-lookup"><span data-stu-id="a3fde-747">Components that require these services (such as :::no-loc(Razor)::: Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="a3fde-748">Le code du constructeur qui obtient une instance de contexte de base de données est indiqué plus loin dans le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="a3fde-748">The constructor code that gets a db context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="a3fde-749">L’outil de génération de modèles automatique a créé automatiquement un contexte de base de données et l’a inscrit dans le conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="a3fde-749">The scaffolding tool automatically created a DB Context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="a3fde-750">Examinez la méthode `ConfigureServices` dans *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="a3fde-750">Examine the `ConfigureServices` method in *Startup.cs*.</span></span> <span data-ttu-id="a3fde-751">La ligne en surbrillance a été ajoutée par l’outil de génération de modèles automatique :</span><span class="sxs-lookup"><span data-stu-id="a3fde-751">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](intro/samples/cu21/Startup.cs?name=snippet_SchoolContext&highlight=13-14)]

<span data-ttu-id="a3fde-752">Le nom de la chaîne de connexion est transmis au contexte en appelant une méthode sur un objet [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="a3fde-752">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="a3fde-753">Pour le développement local, le [système de configuration ASP.net Core](xref:fundamentals/configuration/index) lit la chaîne de connexion à partir du *:::no-loc(appsettings.json):::* fichier.</span><span class="sxs-lookup"><span data-stu-id="a3fde-753">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *:::no-loc(appsettings.json):::* file.</span></span>

## <a name="update-main"></a><span data-ttu-id="a3fde-754">Mettre à jour la méthode Main</span><span class="sxs-lookup"><span data-stu-id="a3fde-754">Update main</span></span>

<span data-ttu-id="a3fde-755">Dans *Program.cs* , modifiez la méthode `Main` pour effectuer les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="a3fde-755">In *Program.cs* , modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="a3fde-756">Obtenir une instance de contexte de base de données à partir du conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="a3fde-756">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="a3fde-757">Appelez [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).</span><span class="sxs-lookup"><span data-stu-id="a3fde-757">Call the  [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).</span></span>
* <span data-ttu-id="a3fde-758">Supprimez le contexte une fois la méthode `EnsureCreated` exécutée.</span><span class="sxs-lookup"><span data-stu-id="a3fde-758">Dispose the context when the `EnsureCreated` method completes.</span></span>

<span data-ttu-id="a3fde-759">Le code suivant montre le fichier *Program.cs* mis à jour.</span><span class="sxs-lookup"><span data-stu-id="a3fde-759">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet)]

<span data-ttu-id="a3fde-760">`EnsureCreated` garantit que la base de données existe pour le contexte.</span><span class="sxs-lookup"><span data-stu-id="a3fde-760">`EnsureCreated` ensures that the database for the context exists.</span></span> <span data-ttu-id="a3fde-761">Si elle existe, aucune action n’est effectuée.</span><span class="sxs-lookup"><span data-stu-id="a3fde-761">If it exists, no action is taken.</span></span> <span data-ttu-id="a3fde-762">Si elle n’existe pas, la base de données et tous ses schéma sont créés.</span><span class="sxs-lookup"><span data-stu-id="a3fde-762">If it does not exist, then the database and all its schema are created.</span></span> <span data-ttu-id="a3fde-763">`EnsureCreated` n’utilise pas de migrations pour créer la base de données.</span><span class="sxs-lookup"><span data-stu-id="a3fde-763">`EnsureCreated` does not use migrations to create the database.</span></span> <span data-ttu-id="a3fde-764">Une base de données créée avec `EnsureCreated` ne peut pas être mise à jour à l’aide de migrations par la suite.</span><span class="sxs-lookup"><span data-stu-id="a3fde-764">A database that is created with `EnsureCreated` cannot be later updated using migrations.</span></span>

<span data-ttu-id="a3fde-765">`EnsureCreated` est appelée au démarrage de l’application, ce qui active le flux de travail suivant :</span><span class="sxs-lookup"><span data-stu-id="a3fde-765">`EnsureCreated` is called on app start, which allows the following work flow:</span></span>

* <span data-ttu-id="a3fde-766">Supprimez la base de données.</span><span class="sxs-lookup"><span data-stu-id="a3fde-766">Delete the DB.</span></span>
* <span data-ttu-id="a3fde-767">Modification du schéma de base de données (par exemple, ajout d’un champ `EmailAddress`).</span><span class="sxs-lookup"><span data-stu-id="a3fde-767">Change the DB schema (for example, add an `EmailAddress` field).</span></span>
* <span data-ttu-id="a3fde-768">Exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="a3fde-768">Run the app.</span></span>
* <span data-ttu-id="a3fde-769">`EnsureCreated` crée une base de données avec la colonne `EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-769">`EnsureCreated` creates a DB with the`EmailAddress` column.</span></span>

<span data-ttu-id="a3fde-770">`EnsureCreated` est pratique au début du développement quand le schéma évolue rapidement.</span><span class="sxs-lookup"><span data-stu-id="a3fde-770">`EnsureCreated` is convenient early in development when the schema is rapidly evolving.</span></span> <span data-ttu-id="a3fde-771">Plus loin dans le tutoriel, la base de données est supprimée et les migrations sont utilisées.</span><span class="sxs-lookup"><span data-stu-id="a3fde-771">Later in the tutorial the DB is deleted and migrations are used.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="a3fde-772">Tester l’application</span><span class="sxs-lookup"><span data-stu-id="a3fde-772">Test the app</span></span>

<span data-ttu-id="a3fde-773">Exécutez l’application et acceptez la :::no-loc(cookie)::: stratégie.</span><span class="sxs-lookup"><span data-stu-id="a3fde-773">Run the app and accept the :::no-loc(cookie)::: policy.</span></span> <span data-ttu-id="a3fde-774">Cette application ne conserve pas les informations personnelles.</span><span class="sxs-lookup"><span data-stu-id="a3fde-774">This app doesn't keep personal information.</span></span> <span data-ttu-id="a3fde-775">Pour en savoir plus sur la :::no-loc(cookie)::: stratégie au niveau de la [prise en charge de l’union européenne règlement général sur la protection des données (RGPD)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="a3fde-775">You can read about the :::no-loc(cookie)::: policy at [EU General Data Protection Regulation (GDPR) support](xref:security/gdpr).</span></span>

* <span data-ttu-id="a3fde-776">Sélectionnez le lien **Students** , puis **Créer nouveau**.</span><span class="sxs-lookup"><span data-stu-id="a3fde-776">Select the **Students** link and then **Create New**.</span></span>
* <span data-ttu-id="a3fde-777">Testez les liens Edit, Details et Delete.</span><span class="sxs-lookup"><span data-stu-id="a3fde-777">Test the Edit, Details, and Delete links.</span></span>

## <a name="examine-the-schoolcontext-db-context"></a><span data-ttu-id="a3fde-778">Examiner le contexte de base de données SchoolContext</span><span class="sxs-lookup"><span data-stu-id="a3fde-778">Examine the SchoolContext DB context</span></span>

<span data-ttu-id="a3fde-779">La classe principale qui coordonne les fonctionnalités d’EF Core pour un modèle de données spécifié est la classe de contexte de base de données.</span><span class="sxs-lookup"><span data-stu-id="a3fde-779">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="a3fde-780">Le contexte de données est dérivé de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="a3fde-780">The data context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="a3fde-781">Il spécifie les entités qui sont incluses dans le modèle de données.</span><span class="sxs-lookup"><span data-stu-id="a3fde-781">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="a3fde-782">Dans ce projet, la classe est nommée `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-782">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="a3fde-783">Mettez à jour *SchoolContext.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a3fde-783">Update *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_Intro&highlight=12-14)]

<span data-ttu-id="a3fde-784">Le code en surbrillance crée une propriété [DbSet \<TEntity> ](/dotnet/api/microsoft.entityframeworkcore.dbset-1) pour chaque jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="a3fde-784">The highlighted code creates a [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for each entity set.</span></span> <span data-ttu-id="a3fde-785">Dans la terminologie d’EF Core :</span><span class="sxs-lookup"><span data-stu-id="a3fde-785">In EF Core terminology:</span></span>

* <span data-ttu-id="a3fde-786">Un jeu d’entités correspond généralement à une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="a3fde-786">An entity set typically corresponds to a DB table.</span></span>
* <span data-ttu-id="a3fde-787">Une entité correspond à une ligne dans la table.</span><span class="sxs-lookup"><span data-stu-id="a3fde-787">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="a3fde-788">`DbSet<Enrollment>` et `DbSet<Course>` peuvent être omis.</span><span class="sxs-lookup"><span data-stu-id="a3fde-788">`DbSet<Enrollment>` and `DbSet<Course>` could be omitted.</span></span> <span data-ttu-id="a3fde-789">EF Core les inclut implicitement, car l’entité `Student` référence l’entité `Enrollment`, et l’entité `Enrollment` référence l’entité `Course`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-789">EF Core includes them implicitly because the `Student` entity references the `Enrollment` entity, and the `Enrollment` entity references the `Course` entity.</span></span> <span data-ttu-id="a3fde-790">Pour ce didacticiel, conservez `DbSet<Enrollment>` et `DbSet<Course>` dans le `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-790">For this tutorial, keep `DbSet<Enrollment>` and `DbSet<Course>` in the `SchoolContext`.</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="a3fde-791">Base de données locale SQL Server Express</span><span class="sxs-lookup"><span data-stu-id="a3fde-791">SQL Server Express LocalDB</span></span>

<span data-ttu-id="a3fde-792">La chaîne de connexion spécifie [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span><span class="sxs-lookup"><span data-stu-id="a3fde-792">The connection string specifies [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="a3fde-793">LocalDB est une version allégée du moteur de base de données SQL Server Express. Elle est destinée au développement d’applications, et non à une utilisation en production.</span><span class="sxs-lookup"><span data-stu-id="a3fde-793">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="a3fde-794">LocalDB démarre à la demande et s’exécute en mode utilisateur, ce qui n’implique aucune configuration complexe.</span><span class="sxs-lookup"><span data-stu-id="a3fde-794">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="a3fde-795">Par défaut, LocalDB crée des fichiers de base de données *.mdf* dans le répertoire `C:/Users/<user>`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-795">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-db-with-test-data"></a><span data-ttu-id="a3fde-796">Ajouter du code pour initialiser la base de données avec des données de test</span><span class="sxs-lookup"><span data-stu-id="a3fde-796">Add code to initialize the DB with test data</span></span>

<span data-ttu-id="a3fde-797">EF Core crée une base de données vide.</span><span class="sxs-lookup"><span data-stu-id="a3fde-797">EF Core creates an empty DB.</span></span> <span data-ttu-id="a3fde-798">Dans cette section, une méthode `Initialize` est écrite pour la remplir avec des données de test.</span><span class="sxs-lookup"><span data-stu-id="a3fde-798">In this section, an `Initialize` method is written to populate it with test data.</span></span>

<span data-ttu-id="a3fde-799">Dans le dossier *Data* , créez un fichier de classe nommé *DbInitializer.cs* et ajoutez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a3fde-799">In the *Data* folder, create a new class file named *DbInitializer.cs* and add the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="a3fde-800">Remarque : le code précédent utilise `Models` pour l’espace de noms ( `namespace ContosoUniversity.Models` ) au lieu de `Data` .</span><span class="sxs-lookup"><span data-stu-id="a3fde-800">Note: The preceding code uses `Models` for the namespace (`namespace ContosoUniversity.Models`) rather than `Data`.</span></span> <span data-ttu-id="a3fde-801">`Models` est cohérent avec le code généré par l’outil de génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="a3fde-801">`Models` is consistent with the scaffolder-generated code.</span></span> <span data-ttu-id="a3fde-802">Pour plus d’informations, consultez [ce problème de génération de modèles automatique GitHub](https://github.com/aspnet/Scaffolding/issues/822).</span><span class="sxs-lookup"><span data-stu-id="a3fde-802">For more information, see [this GitHub scaffolding issue](https://github.com/aspnet/Scaffolding/issues/822).</span></span>

<span data-ttu-id="a3fde-803">Le code vérifie s’il existe des étudiants dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="a3fde-803">The code checks if there are any students in the DB.</span></span> <span data-ttu-id="a3fde-804">S’il n’y en a pas, la base de données est initialisée avec des données de test.</span><span class="sxs-lookup"><span data-stu-id="a3fde-804">If there are no students in the DB, the DB is initialized with test data.</span></span> <span data-ttu-id="a3fde-805">Il charge les données de test dans des tableaux plutôt que des collections `List<T>` afin d’optimiser les performances.</span><span class="sxs-lookup"><span data-stu-id="a3fde-805">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="a3fde-806">La méthode `EnsureCreated` crée automatiquement la base de données pour le contexte de base de données.</span><span class="sxs-lookup"><span data-stu-id="a3fde-806">The `EnsureCreated` method automatically creates the DB for the DB context.</span></span> <span data-ttu-id="a3fde-807">Si la base de données existe, `EnsureCreated` retourne sans modifier la base de données.</span><span class="sxs-lookup"><span data-stu-id="a3fde-807">If the DB exists, `EnsureCreated` returns without modifying the DB.</span></span>

<span data-ttu-id="a3fde-808">Dans *Program.cs* , modifiez la méthode `Main` pour appeler `Initialize` :</span><span class="sxs-lookup"><span data-stu-id="a3fde-808">In *Program.cs* , modify the `Main` method to call `Initialize`:</span></span>

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet2&highlight=14-15)]

# <a name="visual-studio"></a>[<span data-ttu-id="a3fde-809">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a3fde-809">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a3fde-810">Arrêtez l’application si elle est en cours d’exécution et exécutez la commande suivante dans la **Console du gestionnaire de package**  :</span><span class="sxs-lookup"><span data-stu-id="a3fde-810">Stop the app if it's running, and run the following command in the **Package Manager Console** (PMC):</span></span>

```powershell
Drop-Database
```

# <a name="visual-studio-code"></a>[<span data-ttu-id="a3fde-811">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a3fde-811">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a3fde-812">Arrêtez l’application si elle est en cours d’exécution, puis supprimez le fichier *CU.db*.</span><span class="sxs-lookup"><span data-stu-id="a3fde-812">Stop the app if it's running, and delete the *CU.db* file.</span></span>

---

## <a name="view-the-db"></a><span data-ttu-id="a3fde-813">Afficher la base de données</span><span class="sxs-lookup"><span data-stu-id="a3fde-813">View the DB</span></span>

<span data-ttu-id="a3fde-814">Le nom de la base de données est généré à partir du nom de contexte indiqué précédemment, ainsi que d’un tiret et d’un GUID.</span><span class="sxs-lookup"><span data-stu-id="a3fde-814">The database name is generated from the context name you provided earlier plus a dash and a GUID.</span></span> <span data-ttu-id="a3fde-815">Il sera donc « SchoolContext-{GUID} ».</span><span class="sxs-lookup"><span data-stu-id="a3fde-815">Thus, the database name will be "SchoolContext-{GUID}".</span></span> <span data-ttu-id="a3fde-816">Le GUID est différent pour chaque utilisateur.</span><span class="sxs-lookup"><span data-stu-id="a3fde-816">The GUID will be different for each user.</span></span>
<span data-ttu-id="a3fde-817">Ouvrez **l’Explorateur d’objets SQL Server** (SSOX) à partir du menu **Affichage** de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a3fde-817">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
<span data-ttu-id="a3fde-818">Dans SSOX, cliquez sur **(localdb)\MSSQLLocalDB > Databases > SchoolContext-{GUID}**.</span><span class="sxs-lookup"><span data-stu-id="a3fde-818">In SSOX, click **(localdb)\MSSQLLocalDB > Databases > SchoolContext-{GUID}**.</span></span>

<span data-ttu-id="a3fde-819">Développez le nœud **Tables**.</span><span class="sxs-lookup"><span data-stu-id="a3fde-819">Expand the **Tables** node.</span></span>

<span data-ttu-id="a3fde-820">Cliquez avec le bouton droit sur la table **Student** et cliquez sur **Afficher les données** pour voir les colonnes créées et les lignes insérées dans la table.</span><span class="sxs-lookup"><span data-stu-id="a3fde-820">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="a3fde-821">Code asynchrone</span><span class="sxs-lookup"><span data-stu-id="a3fde-821">Asynchronous code</span></span>

<span data-ttu-id="a3fde-822">La programmation asynchrone est le mode par défaut pour ASP.NET Core et EF Core.</span><span class="sxs-lookup"><span data-stu-id="a3fde-822">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="a3fde-823">Un serveur web a un nombre limité de threads disponibles et, dans les situations de forte charge, tous les threads disponibles peuvent être utilisés.</span><span class="sxs-lookup"><span data-stu-id="a3fde-823">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="a3fde-824">Quand cela se produit, le serveur ne peut pas traiter de nouvelle requête tant que les threads ne sont pas libérés.</span><span class="sxs-lookup"><span data-stu-id="a3fde-824">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="a3fde-825">Avec le code synchrone, plusieurs threads peuvent être bloqués alors qu’ils n’effectuent en fait aucun travail, car ils attendent que des E/S se terminent.</span><span class="sxs-lookup"><span data-stu-id="a3fde-825">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="a3fde-826">Avec le code asynchrone, quand un processus attend que des E/S se terminent, son thread est libéré afin d’être utilisé par le serveur pour traiter d’autres demandes.</span><span class="sxs-lookup"><span data-stu-id="a3fde-826">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="a3fde-827">Il permet ainsi d’utiliser les ressources serveur plus efficacement, et le serveur peut gérer plus de trafic sans retard.</span><span class="sxs-lookup"><span data-stu-id="a3fde-827">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="a3fde-828">Le code asynchrone introduit néanmoins une petite surcharge au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="a3fde-828">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="a3fde-829">Dans les situations de faible trafic, le gain de performances est négligeable, tandis qu’en cas de trafic élevé l’amélioration potentielle des performances est importante.</span><span class="sxs-lookup"><span data-stu-id="a3fde-829">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="a3fde-830">Dans le code suivant, le mot clé [async](/dotnet/csharp/language-reference/keywords/async), la valeur renvoyée `Task<T>`, le mot clé `await` et la méthode `ToListAsync` déclenchent l’exécution asynchrone du code.</span><span class="sxs-lookup"><span data-stu-id="a3fde-830">In the following code, the [async](/dotnet/csharp/language-reference/keywords/async) keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="a3fde-831">Le mot clé `async` fait en sorte que le compilateur :</span><span class="sxs-lookup"><span data-stu-id="a3fde-831">The `async` keyword tells the compiler to:</span></span>
  * <span data-ttu-id="a3fde-832">Génère des rappels pour les parties du corps de méthode.</span><span class="sxs-lookup"><span data-stu-id="a3fde-832">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="a3fde-833">Crée automatiquement l’objet [Task](/dotnet/api/system.threading.tasks.task) qui est retourné.</span><span class="sxs-lookup"><span data-stu-id="a3fde-833">Automatically create the [Task](/dotnet/api/system.threading.tasks.task) object that's returned.</span></span> <span data-ttu-id="a3fde-834">Pour plus d’informations, consultez [Type de retour Task](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span><span class="sxs-lookup"><span data-stu-id="a3fde-834">For more information, see [Task Return Type](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span></span>

* <span data-ttu-id="a3fde-835">Le type de retour implicite `Task` représente le travail en cours.</span><span class="sxs-lookup"><span data-stu-id="a3fde-835">The implicit return type `Task` represents ongoing work.</span></span>
* <span data-ttu-id="a3fde-836">Le mot clé `await` indique au compilateur de fractionner la méthode en deux parties.</span><span class="sxs-lookup"><span data-stu-id="a3fde-836">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="a3fde-837">La première partie se termine par l’opération qui est démarrée de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="a3fde-837">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="a3fde-838">La seconde partie est placée dans une méthode de rappel qui est appelée quand l’opération se termine.</span><span class="sxs-lookup"><span data-stu-id="a3fde-838">The second part is put into a callback method that's called when the operation completes.</span></span>
* <span data-ttu-id="a3fde-839">`ToListAsync` est la version asynchrone de la méthode d’extension `ToList`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-839">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="a3fde-840">Voici quelques éléments à connaître lors de l’écriture de code asynchrone qui utilise EF Core :</span><span class="sxs-lookup"><span data-stu-id="a3fde-840">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="a3fde-841">Seules les instructions qui provoquent l’envoi de requêtes ou de commandes vers la base de données sont exécutées de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="a3fde-841">Only statements that cause queries or commands to be sent to the DB are executed asynchronously.</span></span> <span data-ttu-id="a3fde-842">Cela comprend `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync` et `SaveChangesAsync`,</span><span class="sxs-lookup"><span data-stu-id="a3fde-842">That includes, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="a3fde-843">mais pas les instructions qui ne font que changer un `IQueryable`, telles que `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="a3fde-843">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>
* <span data-ttu-id="a3fde-844">Un contexte EF Core n’est pas thread-safe : n’essayez pas d’effectuer plusieurs opérations en parallèle.</span><span class="sxs-lookup"><span data-stu-id="a3fde-844">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span>
* <span data-ttu-id="a3fde-845">Pour tirer parti des avantages de performances du code asynchrone, vérifiez que les packages de bibliothèque (par exemple pour la pagination) utilisent le mode asynchrone s’ils appellent des méthodes EF Core qui envoient des requêtes à la base de données.</span><span class="sxs-lookup"><span data-stu-id="a3fde-845">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the DB.</span></span>

<span data-ttu-id="a3fde-846">Pour plus d’informations sur la programmation asynchrone dans .NET, consultez [Vue d’ensemble d’Async](/dotnet/standard/async) et [Programmation asynchrone avec async et await](/dotnet/csharp/programming-guide/concepts/async/).</span><span class="sxs-lookup"><span data-stu-id="a3fde-846">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/standard/async) and [Asynchronous programming with async and await](/dotnet/csharp/programming-guide/concepts/async/).</span></span>

<span data-ttu-id="a3fde-847">Dans le didacticiel suivant, nous allons examiner les opérations CRUD de base (créer, lire, mettre à jour, supprimer).</span><span class="sxs-lookup"><span data-stu-id="a3fde-847">In the next tutorial, basic CRUD (create, read, update, delete) operations are examined.</span></span>



## <a name="additional-resources"></a><span data-ttu-id="a3fde-848">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="a3fde-848">Additional resources</span></span>

* [<span data-ttu-id="a3fde-849">Version YouTube de ce tutoriel</span><span class="sxs-lookup"><span data-stu-id="a3fde-849">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=P7iTtQnkrNs)

> [!div class="step-by-step"]
> [<span data-ttu-id="a3fde-850">Next</span><span class="sxs-lookup"><span data-stu-id="a3fde-850">Next</span></span>](xref:data/ef-rp/crud)

::: moniker-end
