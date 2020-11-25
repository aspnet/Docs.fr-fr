<span data-ttu-id="ef92e-101">Le `App` composant ( `App.razor` ) est similaire au `App` composant trouvé dans les applications serveur éblouissantes :</span><span class="sxs-lookup"><span data-stu-id="ef92e-101">The `App` component (`App.razor`) is similar to the `App` component found in Blazor Server apps:</span></span>

* <span data-ttu-id="ef92e-102">Le <xref:Microsoft.AspNetCore.Components.Authorization.CascadingAuthenticationState> composant gère l’exposition de au <xref:Microsoft.AspNetCore.Components.Authorization.AuthenticationState> reste de l’application.</span><span class="sxs-lookup"><span data-stu-id="ef92e-102">The <xref:Microsoft.AspNetCore.Components.Authorization.CascadingAuthenticationState> component manages exposing the <xref:Microsoft.AspNetCore.Components.Authorization.AuthenticationState> to the rest of the app.</span></span>
* <span data-ttu-id="ef92e-103">Le <xref:Microsoft.AspNetCore.Components.Authorization.AuthorizeRouteView> composant permet de s’assurer que l’utilisateur actuel est autorisé à accéder à une page donnée ou à effectuer un rendu du `RedirectToLogin` composant.</span><span class="sxs-lookup"><span data-stu-id="ef92e-103">The <xref:Microsoft.AspNetCore.Components.Authorization.AuthorizeRouteView> component makes sure that the current user is authorized to access a given page or otherwise renders the `RedirectToLogin` component.</span></span>
* <span data-ttu-id="ef92e-104">Le `RedirectToLogin` composant gère la redirection des utilisateurs non autorisés vers la page de connexion.</span><span class="sxs-lookup"><span data-stu-id="ef92e-104">The `RedirectToLogin` component manages redirecting unauthorized users to the login page.</span></span>

```razor
<CascadingAuthenticationState>
    <Router AppAssembly="@typeof(Program).Assembly">
        <Found Context="routeData">
            <AuthorizeRouteView RouteData="@routeData" 
                DefaultLayout="@typeof(MainLayout)">
                <NotAuthorized>
                    @if (!context.User.Identity.IsAuthenticated)
                    {
                        <RedirectToLogin />
                    }
                    else
                    {
                        <p>
                            You are not authorized to access 
                            this resource.
                        </p>
                    }
                </NotAuthorized>
            </AuthorizeRouteView>
        </Found>
        <NotFound>
            <LayoutView Layout="@typeof(MainLayout)">
                <p>Sorry, there's nothing at this address.</p>
            </LayoutView>
        </NotFound>
    </Router>
</CascadingAuthenticationState>
```
