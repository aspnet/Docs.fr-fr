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
ms.openlocfilehash: b2519eaab7a83d66e60be05b1c2deeb8094b016f
ms.sourcegitcommit: a49c47d5a573379effee5c6b6e36f5c302aa756b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/16/2021
ms.locfileid: "100552949"
---
<span data-ttu-id="964ed-101">La page index page ( `wwwroot/index.html` ) comprend un script qui définit le `AuthenticationService` en JavaScript.</span><span class="sxs-lookup"><span data-stu-id="964ed-101">The Index page (`wwwroot/index.html`) page includes a script that defines the `AuthenticationService` in JavaScript.</span></span> <span data-ttu-id="964ed-102">`AuthenticationService` gère les détails de bas niveau du protocole OIDC.</span><span class="sxs-lookup"><span data-stu-id="964ed-102">`AuthenticationService` handles the low-level details of the OIDC protocol.</span></span> <span data-ttu-id="964ed-103">L’application appelle en interne des méthodes définies dans le script pour effectuer les opérations d’authentification.</span><span class="sxs-lookup"><span data-stu-id="964ed-103">The app internally calls methods defined in the script to perform the authentication operations.</span></span>

```html
<script src="_content/Microsoft.AspNetCore.Components.WebAssembly.Authentication/
    AuthenticationService.js"></script>
```
