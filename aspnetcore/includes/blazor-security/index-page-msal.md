<span data-ttu-id="64890-101">La page index page ( `wwwroot/index.html` ) comprend un script qui définit le `AuthenticationService` en JavaScript.</span><span class="sxs-lookup"><span data-stu-id="64890-101">The Index page (`wwwroot/index.html`) page includes a script that defines the `AuthenticationService` in JavaScript.</span></span> <span data-ttu-id="64890-102">`AuthenticationService` gère les détails de bas niveau du protocole OIDC.</span><span class="sxs-lookup"><span data-stu-id="64890-102">`AuthenticationService` handles the low-level details of the OIDC protocol.</span></span> <span data-ttu-id="64890-103">L’application appelle en interne des méthodes définies dans le script pour effectuer les opérations d’authentification.</span><span class="sxs-lookup"><span data-stu-id="64890-103">The app internally calls methods defined in the script to perform the authentication operations.</span></span>

```html
<script src="_content/Microsoft.Authentication.WebAssembly.Msal/
    AuthenticationService.js"></script>
```
