::: moniker range=">= aspnetcore-3.0"

<a name="dc"></a>

<span data-ttu-id="b2129-101">Créez un dossier nommé *Data*.</span><span class="sxs-lookup"><span data-stu-id="b2129-101">Create a *Data* folder.</span></span>

<span data-ttu-id="b2129-102">Ajoutez la classe `MvcMovieContext` suivante au dossier *Data* :</span><span class="sxs-lookup"><span data-stu-id="b2129-102">Add the following `MvcMovieContext` class to the *Data* folder:</span></span>  

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/zDocOnly/MvcMovieContext.cs?name=snippet)]

<span data-ttu-id="b2129-103">Le code précédent crée une propriété `DbSet` pour le jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="b2129-103">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="b2129-104">Dans la terminologie Entity Framework, un jeu d’entités correspond généralement à une table de base de données, et une entité correspond à une ligne dans la table.</span><span class="sxs-lookup"><span data-stu-id="b2129-104">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a><span data-ttu-id="b2129-105">Ajouter une chaîne de connexion de base de données</span><span class="sxs-lookup"><span data-stu-id="b2129-105">Add a database connection string</span></span>

<span data-ttu-id="b2129-106">Ajoutez une chaîne de connexion au *appsettings.jssur* le fichier :</span><span class="sxs-lookup"><span data-stu-id="b2129-106">Add a connection string to the *appsettings.json* file:</span></span>

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/appsettings_SQLite.json?highlight=10-12)]

### <a name="add-nuget-packages-and-ef-tools"></a><span data-ttu-id="b2129-107">Ajouter des packages NuGet des outils EF</span><span class="sxs-lookup"><span data-stu-id="b2129-107">Add NuGet packages and EF tools</span></span>

[!INCLUDE[](~/includes/add-EF-NuGet-SQLite-CLI.md)]

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="b2129-108">Inscrire le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="b2129-108">Register the database context</span></span>

<span data-ttu-id="b2129-109">En tête du fichier *Startup.cs*, ajoutez les instructions `using` suivantes :</span><span class="sxs-lookup"><span data-stu-id="b2129-109">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using MvcMovie.Data;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="b2129-110">Inscrivez le contexte de base de données auprès du conteneur d’[injection de dépendances](xref:fundamentals/dependency-injection) dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b2129-110">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_UseSqlite&highlight=6-7)]

<span data-ttu-id="b2129-111">Générez le projet en tant que vérification des erreurs du compilateur.</span><span class="sxs-lookup"><span data-stu-id="b2129-111">Build the project as a check for compiler errors.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="b2129-112">Ajoutez la classe `MvcMovieContext` suivante au dossier *Models* :</span><span class="sxs-lookup"><span data-stu-id="b2129-112">Add the following `MvcMovieContext` class to the *Models* folder:</span></span>  

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Data/MvcMovieContext.cs)]

<span data-ttu-id="b2129-113">Le code précédent crée une propriété `DbSet` pour le jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="b2129-113">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="b2129-114">Dans la terminologie Entity Framework, un jeu d’entités correspond généralement à une table de base de données, et une entité correspond à une ligne dans la table.</span><span class="sxs-lookup"><span data-stu-id="b2129-114">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a><span data-ttu-id="b2129-115">Ajouter une chaîne de connexion de base de données</span><span class="sxs-lookup"><span data-stu-id="b2129-115">Add a database connection string</span></span>

<span data-ttu-id="b2129-116">Ajoutez une chaîne de connexion au *appsettings.jssur* le fichier :</span><span class="sxs-lookup"><span data-stu-id="b2129-116">Add a connection string to the *appsettings.json* file:</span></span>

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

### <a name="add-required-nuget-packages"></a><span data-ttu-id="b2129-117">Ajouter les packages NuGet exigés</span><span class="sxs-lookup"><span data-stu-id="b2129-117">Add required NuGet packages</span></span>

<span data-ttu-id="b2129-118">Exécutez la commande CLI .NET Core suivante pour ajouter SQLite et CodeGeneration.Design au projet :</span><span class="sxs-lookup"><span data-stu-id="b2129-118">Run the following .NET Core CLI command to add SQLite and CodeGeneration.Design  to the project:</span></span>

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
```

<span data-ttu-id="b2129-119">Le package `Microsoft.VisualStudio.Web.CodeGeneration.Design` est nécessaire à la génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="b2129-119">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="b2129-120">Inscrire le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="b2129-120">Register the database context</span></span>

<span data-ttu-id="b2129-121">En tête du fichier *Startup.cs*, ajoutez les instructions `using` suivantes :</span><span class="sxs-lookup"><span data-stu-id="b2129-121">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using MvcMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="b2129-122">Inscrivez le contexte de base de données auprès du conteneur d’[injection de dépendances](xref:fundamentals/dependency-injection) dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b2129-122">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<span data-ttu-id="b2129-123">Générez le projet en tant que vérification des erreurs.</span><span class="sxs-lookup"><span data-stu-id="b2129-123">Build the project as a check for errors.</span></span>
::: moniker-end