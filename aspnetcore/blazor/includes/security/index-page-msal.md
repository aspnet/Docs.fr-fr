---
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
ms.openlocfilehash: ce0a993093812c9a477d85d363827988ae9bd536
ms.sourcegitcommit: a49c47d5a573379effee5c6b6e36f5c302aa756b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/16/2021
ms.locfileid: "100552453"
---
La page index page ( `wwwroot/index.html` ) comprend un script qui définit le `AuthenticationService` en JavaScript. `AuthenticationService` gère les détails de bas niveau du protocole OIDC. L’application appelle en interne des méthodes définies dans le script pour effectuer les opérations d’authentification.

```html
<script src="_content/Microsoft.Authentication.WebAssembly.Msal/
    AuthenticationService.js"></script>
```
