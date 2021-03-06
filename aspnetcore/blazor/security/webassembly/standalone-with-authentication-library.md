---
title: Sécuriser une Blazor WebAssembly application ASP.net Core autonome avec la bibliothèque d’authentification
author: guardrex
description: Découvrez comment sécuriser une Blazor WebAssembly application ASP.net Core autonome avec la bibliothèque d’authentification.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/10/2021
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
uid: blazor/security/webassembly/standalone-with-authentication-library
ms.openlocfilehash: a198606caf55232c221f1d1f1224918d3f87f04c
ms.sourcegitcommit: 1166b0ff3828418559510c661e8240e5c5717bb7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/12/2021
ms.locfileid: "100280885"
---
# <a name="secure-an-aspnet-core-blazor-webassembly-standalone-app-with-the-authentication-library"></a>Sécuriser une Blazor WebAssembly application ASP.net Core autonome avec la bibliothèque d’authentification

*Pour Azure Active Directory (AAD) et Azure Active Directory B2C (AAD B2C), ne suivez pas les instructions de cette rubrique. Consultez les rubriques AAD et AAD B2C dans ce nœud de table des matières.*

Pour créer une [ Blazor WebAssembly application autonome](xref:blazor/hosting-models#blazor-webassembly) qui utilise [`Microsoft.AspNetCore.Components.WebAssembly.Authentication`](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication) la bibliothèque, suivez les instructions de votre choix d’outils. Si vous ajoutez la prise en charge de l’authentification, consultez les sections suivantes de cet article pour obtenir de l’aide sur l’installation et la configuration de l’application.

> [!NOTE]
> Le Identity fournisseur (IP) doit utiliser [OpenID Connect (OIDC)](https://openid.net/connect/). Par exemple, l’adresse IP de Facebook n’est pas un fournisseur conforme à OIDC, de sorte que les instructions de cette rubrique ne fonctionnent pas avec l’adresse IP Facebook. Pour plus d’informations, consultez <xref:blazor/security/webassembly/index#authentication-library>.

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

Pour créer un nouveau Blazor WebAssembly projet avec un mécanisme d’authentification :

1. Après avoir choisi le modèle d' **Blazor WebAssembly application** dans la boîte de dialogue **créer une application Web ASP.net Core** , sélectionnez **modifier** sous **authentification**.

1. Sélectionnez **des comptes d’utilisateur individuels** avec l’option **stocker les comptes d’utilisateur dans l’application** pour utiliser le système de ASP.net Core [Identity](xref:security/authentication/identity) . Cette sélection ajoute la prise en charge de l’authentification et n’entraîne pas le stockage des utilisateurs dans une base de données. Les sections suivantes de cet article fournissent des informations supplémentaires.

# <a name="visual-studio-code--net-core-cli"></a>[Visual Studio Code/.NET Core CLI](#tab/visual-studio-code+netcore-cli)

Créez un Blazor WebAssembly projet avec un mécanisme d’authentification dans un dossier vide. Spécifiez le `Individual` mécanisme d’authentification avec l' `-au|--auth` option permettant d’utiliser le système de ASP.net Core [Identity](xref:security/authentication/identity) . Cette sélection ajoute la prise en charge de l’authentification et n’entraîne pas le stockage des utilisateurs dans une base de données. Les sections suivantes de cet article fournissent des informations supplémentaires.

```dotnetcli
dotnet new blazorwasm -au Individual -o {APP NAME}
```

| Espace réservé  | Exemple        |
| ------------ | -------------- |
| `{APP NAME}` | `BlazorSample` |

L’emplacement de sortie spécifié avec l’option `-o|--output` crée un dossier de projet s’il n’existe pas et devient partie intégrante du nom de l’application.

Pour plus d’informations, consultez la [`dotnet new`](/dotnet/core/tools/dotnet-new) commande dans le guide .net core.

# <a name="visual-studio-for-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

Pour créer un nouveau Blazor WebAssembly projet avec un mécanisme d’authentification :

1. Dans l’étape **configurer votre nouvelle Blazor WebAssembly application** , sélectionnez **authentification individuelle (dans l’application)** dans la liste déroulante **authentification** .

1. L’application est créée pour utiliser ASP.NET Core [Identity](xref:security/authentication/identity) et n’entraîne pas le stockage des utilisateurs dans une base de données. Les sections suivantes de cet article fournissent des informations supplémentaires.

---

## <a name="authentication-package"></a>Package d’authentification

Quand une application est créée pour utiliser des comptes d’utilisateur individuels, l’application reçoit automatiquement une référence de package pour le [`Microsoft.AspNetCore.Components.WebAssembly.Authentication`](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication) package dans le fichier projet de l’application. Le package fournit un ensemble de primitives qui aident l’application à authentifier les utilisateurs et à obtenir des jetons pour appeler des API protégées.

Si vous ajoutez l’authentification à une application, ajoutez manuellement le package au fichier projet de l’application :

```xml
<PackageReference 
  Include="Microsoft.AspNetCore.Components.WebAssembly.Authentication" 
  Version="{VERSION}" />
```

Pour l’espace réservé `{VERSION}` , la dernière version stable du package qui correspond à la version du Framework partagé de l’application est disponible dans l' **historique des versions** du package sur [NuGet.org](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication).

## <a name="authentication-service-support"></a>Prise en charge du service d’authentification

La prise en charge de l’authentification des utilisateurs est inscrite dans le conteneur de service avec la <xref:Microsoft.Extensions.DependencyInjection.WebAssemblyAuthenticationServiceCollectionExtensions.AddOidcAuthentication%2A> méthode d’extension fournie par le [`Microsoft.AspNetCore.Components.WebAssembly.Authentication`](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication) Package. Cette méthode permet de configurer les services requis pour que l’application interagisse avec le Identity fournisseur (IP).

Pour une nouvelle application, fournissez des valeurs pour les `{AUTHORITY}` `{CLIENT ID}` espaces réservés et dans la configuration suivante. Fournissez d’autres valeurs de configuration requises pour l’utilisation de l’adresse IP de l’application. L’exemple est pour Google, qui requiert `PostLogoutRedirectUri` , `RedirectUri` et `ResponseType` . Si vous ajoutez l’authentification à une application, ajoutez manuellement le code et la configuration suivants à l’application avec des valeurs pour les espaces réservés et d’autres valeurs de configuration.

`Program.cs`:

```csharp
builder.Services.AddOidcAuthentication(options =>
{
    builder.Configuration.Bind("Local", options.ProviderOptions);
});
```

La configuration est fournie par le `wwwroot/appsettings.json` fichier :

```json
{
  "Local": {
    "Authority": "{AUTHORITY}",
    "ClientId": "{CLIENT ID}"
  }
}
```

Exemple de OIDC Google OAuth 2,0 :

```json
{
  "Local": {
    "Authority": "https://accounts.google.com/",
    "ClientId": "2.......7-e.....................q.apps.googleusercontent.com",
    "PostLogoutRedirectUri": "https://localhost:5001/authentication/logout-callback",
    "RedirectUri": "https://localhost:5001/authentication/login-callback",
    "ResponseType": "id_token"
  }
}
```

L’URI de redirection ( `https://localhost:5001/authentication/login-callback` ) est enregistré dans la [console des API Google](https://console.developers.google.com/apis/dashboard) dans les **informations d’identification**  >  **`{NAME}`**  >  **URI de redirection autorisées**, où `{NAME}` est le nom du client de l’application dans la liste des applications des **ID client OAuth 2,0** de la console des API Google.

La prise en charge de l’authentification pour les applications autonomes est offerte à l’aide de OpenID Connect (OIDC). La <xref:Microsoft.Extensions.DependencyInjection.WebAssemblyAuthenticationServiceCollectionExtensions.AddOidcAuthentication%2A> méthode accepte un rappel pour configurer les paramètres requis pour authentifier une application à l’aide de OIDC. Les valeurs requises pour la configuration de l’application peuvent être obtenues à partir de l’adresse IP conforme à OIDC. Obtenez les valeurs lors de l’inscription de l’application, qui se produit généralement dans son portail en ligne.

## <a name="access-token-scopes"></a>Étendues de jeton d’accès

Le Blazor WebAssembly modèle configure automatiquement les étendues par défaut pour `openid` et `profile` .

Le Blazor WebAssembly modèle ne configure pas automatiquement l’application pour demander un jeton d’accès pour une API sécurisée. Pour approvisionner un jeton d’accès dans le cadre du processus de connexion, ajoutez l’étendue aux étendues de jetons par défaut de <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.OidcProviderOptions> . Si vous ajoutez l’authentification à une application, ajoutez manuellement le code suivant et configurez l’URI de l’étendue.

`Program.cs`:

```csharp
builder.Services.AddOidcAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultScopes.Add("{SCOPE URI}");
});
```

Pour plus d’informations, consultez les sections suivantes de l’article relatif aux *scénarios supplémentaires* :

* [Demander des jetons d’accès supplémentaires](xref:blazor/security/webassembly/additional-scenarios#request-additional-access-tokens)
* [Attacher des jetons aux demandes sortantes](xref:blazor/security/webassembly/additional-scenarios#attach-tokens-to-outgoing-requests)

## <a name="imports-file"></a>Fichier d’importation

[!INCLUDE[](~/blazor/includes/security/imports-file-standalone.md)]

## <a name="index-page"></a>Page d'index

[!INCLUDE[](~/blazor/includes/security/index-page-authentication.md)]

## <a name="app-component"></a>Composant d’application

[!INCLUDE[](~/blazor/includes/security/app-component.md)]

## <a name="redirecttologin-component"></a>Composant RedirectToLogin

[!INCLUDE[](~/blazor/includes/security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a>Composant LoginDisplay

Le `LoginDisplay` composant ( `Shared/LoginDisplay.razor` ) est rendu dans le `MainLayout` composant ( `Shared/MainLayout.razor` ) et gère les comportements suivants :

* Pour les utilisateurs authentifiés :
  * Affiche le nom d’utilisateur actuel.
  * Offre un bouton permettant de se déconnecter de l’application.
* Pour les utilisateurs anonymes, offre la possibilité de se connecter.

```razor
@using Microsoft.AspNetCore.Components.Authorization
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication
@inject NavigationManager Navigation
@inject SignOutSessionStateManager SignOutManager

<AuthorizeView>
    <Authorized>
        Hello, @context.User.Identity.Name!
        <button class="nav-link btn btn-link" @onclick="BeginSignOut">
            Log out
        </button>
    </Authorized>
    <NotAuthorized>
        <a href="authentication/login">Log in</a>
    </NotAuthorized>
</AuthorizeView>

@code {
    private async Task BeginSignOut(MouseEventArgs args)
    {
        await SignOutManager.SetSignOutState();
        Navigation.NavigateTo("authentication/logout");
    }
}
```

## <a name="authentication-component"></a>Composant d’authentification

[!INCLUDE[](~/blazor/includes/security/authentication-component.md)]

[!INCLUDE[](~/blazor/includes/security/troubleshoot.md)]

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:blazor/security/webassembly/additional-scenarios>
* [Demandes d’API Web non authentifiées ou non autorisées dans une application avec un client par défaut sécurisé](xref:blazor/security/webassembly/additional-scenarios#unauthenticated-or-unauthorized-web-api-requests-in-an-app-with-a-secure-default-client)
* <xref:host-and-deploy/proxy-load-balancer>: Fournit des conseils sur :
  * Utilisation de l’intergiciel d’en-têtes transférés pour conserver les informations de schéma HTTPs entre les serveurs proxy et les réseaux internes.
  * Scénarios et cas d’utilisation supplémentaires, y compris la configuration manuelle du schéma, les modifications du chemin des demandes pour le routage des demandes correct et le transfert du modèle de demande pour les proxys inversés Linux et non-IIS.
