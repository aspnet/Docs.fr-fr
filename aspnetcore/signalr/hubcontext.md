---
title: SignalR HubContext
author: bradygaster
description: Découvrez comment utiliser le service ASP.NET Core SignalR HubContext pour envoyer des notifications à des clients en dehors d’un concentrateur.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
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
- IHubContext
uid: signalr/hubcontext
ms.openlocfilehash: 2b2939a7692a195c6dc1b8421433a723310b4bd6
ms.sourcegitcommit: 54fe1ae5e7d068e27376d562183ef9ddc7afc432
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/10/2021
ms.locfileid: "102589203"
---
# <a name="send-messages-from-outside-a-hub"></a>Envoyer des messages provenant de l’extérieur d’un concentrateur

Par [Mikael Mengistu](https://twitter.com/MikaelM_12)

Le SignalR Hub est l’abstraction principale pour l’envoi de messages aux clients connectés au SignalR serveur. Il est également possible d’envoyer des messages à partir d’autres emplacements dans votre application à l’aide du `IHubContext` service. Cet article explique comment accéder à un SignalR `IHubContext` pour envoyer des notifications à des clients en dehors d’un concentrateur.

[Afficher ou télécharger l’exemple de code](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/signalr/hubcontext/sample/) [(procédure de téléchargement)](xref:index#how-to-download-a-sample)

## <a name="get-an-instance-of-ihubcontext"></a>Obtient une instance de IHubContext

Dans ASP.NET Core SignalR , vous pouvez accéder à une instance de `IHubContext` via l’injection de dépendances. Vous pouvez injecter une instance de `IHubContext` dans un contrôleur, un intergiciel ou un autre service d’injection de contrôle. Utilisez l’instance pour envoyer des messages aux clients.

> [!NOTE]
> Cela diffère de ASP.NET 4. x SignalR qui utilisait GlobalHost pour fournir l’accès à `IHubContext` . ASP.NET Core a une infrastructure d’injection de dépendances qui supprime la nécessité de ce Singleton global.

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a>Injecter une instance de IHubContext dans un contrôleur

Vous pouvez injecter une instance de `IHubContext` dans un contrôleur en l’ajoutant à votre constructeur :

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

Désormais, avec l’accès à une instance de `IHubContext` , vous pouvez appeler les méthodes de concentrateur comme si vous étiez dans le Hub lui-même.

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a>Obtient une instance de IHubContext dans l’intergiciel (middleware)

Accédez à `IHubContext` dans le pipeline de l’intergiciel (middleware) comme suit :

```csharp
app.Use(async (context, next) =>
{
    var hubContext = context.RequestServices
                            .GetRequiredService<IHubContext<ChatHub>>();
    //...
    
    if (next != null)
    {
        await next.Invoke();
    }
});
```

> [!NOTE]
> Lorsque les méthodes de concentrateur sont appelées à partir de l’extérieur de la `Hub` classe, aucun appelant n’est associé à l’appel. Par conséquent, il n’existe aucun accès `ConnectionId` aux `Caller` Propriétés, et `Others` .

### <a name="get-an-instance-of-ihubcontext-from-ihost"></a>Obtient une instance de IHubContext à partir de IHost

L’accès `IHubContext` à un à partir de l’hôte Web est utile pour l’intégration à des zones en dehors de ASP.net Core, par exemple, à l’aide d’infrastructures d’injection de dépendances tierces :

```csharp
    public class Program
    {
        public static void Main(string[] args)
        {
            var host = CreateHostBuilder(args).Build();
            var hubContext = host.Services.GetService(typeof(IHubContext<ChatHub>));
            host.Run();
        }

        public static IHostBuilder CreateHostBuilder(string[] args) =>
            Host.CreateDefaultBuilder(args)
                .ConfigureWebHostDefaults(webBuilder => {
                    webBuilder.UseStartup<Startup>();
                });
    }
```

### <a name="inject-a-strongly-typed-hubcontext"></a>Injecter un HubContext fortement typé

Pour injecter un HubContext fortement typé, vérifiez que votre Hub hérite de `Hub<T>` . Injectez-le à l’aide de l' `IHubContext<THub, T>` interface plutôt que `IHubContext<THub>` .

```csharp
public class ChatController : Controller
{
    public IHubContext<ChatHub, IChatClient> _strongChatHubContext { get; }

    public ChatController(IHubContext<ChatHub, IChatClient> chatHubContext)
    {
        _strongChatHubContext = chatHubContext;
    }

    public async Task SendMessage(string user, string message)
    {
        await _strongChatHubContext.Clients.All.ReceiveMessage(user, message);
    }
}
```

Pour plus d’informations, consultez [hubs fortement typés](xref:signalr/hubs#strongly-typed-hubs) .

## <a name="related-resources"></a>Ressources associées

* [Prise en main](xref:tutorials/signalr)
* [Concentrateurs](xref:signalr/hubs)
* [Publier sur Azure](xref:signalr/publish-to-azure-web-app)
