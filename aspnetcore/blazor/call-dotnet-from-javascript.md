---
title: Appeler des méthodes .NET à partir de fonctions JavaScript dans ASP.NET Core Blazor
author: guardrex
description: Découvrez comment appeler des méthodes .NET à partir de fonctions JavaScript dans des Blazor applications.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc, devx-track-js
ms.date: 08/12/2020
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
uid: blazor/call-dotnet-from-javascript
ms.openlocfilehash: 45ddcc9e006df2c5e86a7859efc76882b269a496
ms.sourcegitcommit: 1166b0ff3828418559510c661e8240e5c5717bb7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/12/2021
ms.locfileid: "100280390"
---
# <a name="call-net-methods-from-javascript-functions-in-aspnet-core-blazor"></a><span data-ttu-id="27489-103">Appeler des méthodes .NET à partir de fonctions JavaScript dans ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="27489-103">Call .NET methods from JavaScript functions in ASP.NET Core Blazor</span></span>

<span data-ttu-id="27489-104">Une Blazor application peut appeler des fonctions JavaScript à partir de méthodes .net et de méthodes .net à partir de fonctions JavaScript.</span><span class="sxs-lookup"><span data-stu-id="27489-104">A Blazor app can invoke JavaScript functions from .NET methods and .NET methods from JavaScript functions.</span></span> <span data-ttu-id="27489-105">Ces scénarios portent le nom de *l’interopérabilité de JavaScript* (*js Interop*).</span><span class="sxs-lookup"><span data-stu-id="27489-105">These scenarios are called *JavaScript interoperability* (*JS interop*).</span></span>

<span data-ttu-id="27489-106">Cet article traite de l’appel de méthodes .NET à partir de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="27489-106">This article covers invoking .NET methods from JavaScript.</span></span> <span data-ttu-id="27489-107">Pour plus d’informations sur l’appel de fonctions JavaScript à partir de .NET, consultez <xref:blazor/call-javascript-from-dotnet> .</span><span class="sxs-lookup"><span data-stu-id="27489-107">For information on how to call JavaScript functions from .NET, see <xref:blazor/call-javascript-from-dotnet>.</span></span>

<span data-ttu-id="27489-108">[Afficher ou télécharger l’exemple de code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="27489-108">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

> [!NOTE]
> <span data-ttu-id="27489-109">Ajoutez des fichiers JS ( `<script>` balises) avant la `</body>` balise de fermeture dans le fichier `wwwroot/index.html` ( Blazor WebAssembly ) ou le `Pages/_Host.cshtml` fichier ( Blazor Server ).</span><span class="sxs-lookup"><span data-stu-id="27489-109">Add JS files (`<script>` tags) before the closing `</body>` tag in the `wwwroot/index.html` file (Blazor WebAssembly) or `Pages/_Host.cshtml` file (Blazor Server).</span></span> <span data-ttu-id="27489-110">Assurez-vous que les fichiers JS avec les méthodes d’interopérabilité JS sont inclus avant les Blazor fichiers Framework js.</span><span class="sxs-lookup"><span data-stu-id="27489-110">Ensure that JS files with JS interop methods are included before Blazor framework JS files.</span></span>

## <a name="static-net-method-call"></a><span data-ttu-id="27489-111">Appel de méthode .NET statique</span><span class="sxs-lookup"><span data-stu-id="27489-111">Static .NET method call</span></span>

<span data-ttu-id="27489-112">Pour appeler une méthode .NET statique à partir de JavaScript, utilisez les `DotNet.invokeMethod` `DotNet.invokeMethodAsync` fonctions ou.</span><span class="sxs-lookup"><span data-stu-id="27489-112">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="27489-113">Transmettez l’identificateur de la méthode statique que vous souhaitez appeler, le nom de l’assembly contenant la fonction et les arguments éventuels.</span><span class="sxs-lookup"><span data-stu-id="27489-113">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="27489-114">La version asynchrone est recommandée pour prendre en charge les Blazor Server scénarios.</span><span class="sxs-lookup"><span data-stu-id="27489-114">The asynchronous version is preferred to support Blazor Server scenarios.</span></span> <span data-ttu-id="27489-115">La méthode .NET doit être publique, statique et avoir l' [ `[JSInvokable]` attribut](xref:Microsoft.JSInterop.JSInvokableAttribute).</span><span class="sxs-lookup"><span data-stu-id="27489-115">The .NET method must be public, static, and have the [`[JSInvokable]` attribute](xref:Microsoft.JSInterop.JSInvokableAttribute).</span></span> <span data-ttu-id="27489-116">L’appel de méthodes génériques ouvertes n’est pas pris en charge actuellement.</span><span class="sxs-lookup"><span data-stu-id="27489-116">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="27489-117">L’exemple d’application comprend une méthode C# pour retourner un `int` tableau.</span><span class="sxs-lookup"><span data-stu-id="27489-117">The sample app includes a C# method to return an `int` array.</span></span> <span data-ttu-id="27489-118">L' [ `[JSInvokable]` attribut](xref:Microsoft.JSInterop.JSInvokableAttribute) est appliqué à la méthode.</span><span class="sxs-lookup"><span data-stu-id="27489-118">The [`[JSInvokable]` attribute](xref:Microsoft.JSInterop.JSInvokableAttribute) is applied to the method.</span></span>

<span data-ttu-id="27489-119">`Pages/JsInterop.razor`:</span><span class="sxs-lookup"><span data-stu-id="27489-119">`Pages/JsInterop.razor`:</span></span>

```razor
<button type="button" class="btn btn-primary"
        onclick="exampleJsFunctions.returnArrayAsyncJs()">
    Trigger .NET static method ReturnArrayAsync
</button>

@code {
    [JSInvokable]
    public static Task<int[]> ReturnArrayAsync()
    {
        return Task.FromResult(new int[] { 1, 2, 3 });
    }
}
```

<span data-ttu-id="27489-120">JavaScript traité au client appelle la méthode .NET C#.</span><span class="sxs-lookup"><span data-stu-id="27489-120">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="27489-121">`wwwroot/exampleJsInterop.js`:</span><span class="sxs-lookup"><span data-stu-id="27489-121">`wwwroot/exampleJsInterop.js`:</span></span>

[!code-javascript[](./common/samples/5.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=8-14)]

<span data-ttu-id="27489-122">Lorsque le **`Trigger .NET static method ReturnArrayAsync`** bouton est sélectionné, examinez la sortie de la console dans les outils de développement Web du navigateur.</span><span class="sxs-lookup"><span data-stu-id="27489-122">When the **`Trigger .NET static method ReturnArrayAsync`** button is selected, examine the console output in the browser's web developer tools.</span></span>

<span data-ttu-id="27489-123">La sortie de la console est la suivante :</span><span class="sxs-lookup"><span data-stu-id="27489-123">The console output is:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="27489-124">La quatrième valeur de tableau fait l’objet d’un push dans le tableau ( `data.push(4);` ) retourné par `ReturnArrayAsync` .</span><span class="sxs-lookup"><span data-stu-id="27489-124">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

<span data-ttu-id="27489-125">Par défaut, l’identificateur de méthode est le nom de la méthode, mais vous pouvez spécifier un identificateur différent à l’aide du constructeur d' [ `[JSInvokable]` attribut](xref:Microsoft.JSInterop.JSInvokableAttribute) :</span><span class="sxs-lookup"><span data-stu-id="27489-125">By default, the method identifier is the method name, but you can specify a different identifier using the [`[JSInvokable]` attribute](xref:Microsoft.JSInterop.JSInvokableAttribute) constructor:</span></span>

```csharp
@code {
    [JSInvokable("DifferentMethodName")]
    public static Task<int[]> ReturnArrayAsync()
    {
        return Task.FromResult(new int[] { 1, 2, 3 });
    }
}
```

<span data-ttu-id="27489-126">Dans le fichier JavaScript côté client :</span><span class="sxs-lookup"><span data-stu-id="27489-126">In the client-side JavaScript file:</span></span>

```javascript
returnArrayAsyncJs: function () {
  DotNet.invokeMethodAsync('{APP ASSEMBLY}', 'DifferentMethodName')
    .then(data => {
      data.push(4);
      console.log(data);
    });
}
```

<span data-ttu-id="27489-127">L’espace réservé `{APP ASSEMBLY}` est le nom de l’assembly d’application de l’application (par exemple, `BlazorSample` ).</span><span class="sxs-lookup"><span data-stu-id="27489-127">The placeholder `{APP ASSEMBLY}` is the app's app assembly name (for example, `BlazorSample`).</span></span>

## <a name="instance-method-call"></a><span data-ttu-id="27489-128">Appel de méthode d’instance</span><span class="sxs-lookup"><span data-stu-id="27489-128">Instance method call</span></span>

<span data-ttu-id="27489-129">Vous pouvez également appeler des méthodes d’instance .NET à partir de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="27489-129">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="27489-130">Pour appeler une méthode d’instance .NET à partir de JavaScript :</span><span class="sxs-lookup"><span data-stu-id="27489-130">To invoke a .NET instance method from JavaScript:</span></span>

* <span data-ttu-id="27489-131">Passer l’instance .NET par référence à JavaScript :</span><span class="sxs-lookup"><span data-stu-id="27489-131">Pass the .NET instance by reference to JavaScript:</span></span>
  * <span data-ttu-id="27489-132">Effectuez un appel statique à <xref:Microsoft.JSInterop.DotNetObjectReference.Create%2A?displayProperty=nameWithType> .</span><span class="sxs-lookup"><span data-stu-id="27489-132">Make a static call to <xref:Microsoft.JSInterop.DotNetObjectReference.Create%2A?displayProperty=nameWithType>.</span></span>
  * <span data-ttu-id="27489-133">Encapsulez l’instance dans une <xref:Microsoft.JSInterop.DotNetObjectReference> instance et appelez <xref:Microsoft.JSInterop.DotNetObjectReference.Create%2A> sur l' <xref:Microsoft.JSInterop.DotNetObjectReference> instance.</span><span class="sxs-lookup"><span data-stu-id="27489-133">Wrap the instance in a <xref:Microsoft.JSInterop.DotNetObjectReference> instance and call <xref:Microsoft.JSInterop.DotNetObjectReference.Create%2A> on the <xref:Microsoft.JSInterop.DotNetObjectReference> instance.</span></span> <span data-ttu-id="27489-134">Supprimez des <xref:Microsoft.JSInterop.DotNetObjectReference> objets (un exemple apparaît plus loin dans cette section).</span><span class="sxs-lookup"><span data-stu-id="27489-134">Dispose of <xref:Microsoft.JSInterop.DotNetObjectReference> objects (an example appears later in this section).</span></span>
* <span data-ttu-id="27489-135">Appeler des méthodes d’instance .NET sur l’instance à l’aide des `invokeMethod` `invokeMethodAsync` fonctions ou.</span><span class="sxs-lookup"><span data-stu-id="27489-135">Invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="27489-136">L’instance .NET peut également être passée comme argument lors de l’appel d’autres méthodes .NET à partir de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="27489-136">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="27489-137">L’exemple d’application enregistre les messages dans la console côté client.</span><span class="sxs-lookup"><span data-stu-id="27489-137">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="27489-138">Pour les exemples suivants présentés dans l’exemple d’application, examinez la sortie de console du navigateur dans les outils de développement du navigateur.</span><span class="sxs-lookup"><span data-stu-id="27489-138">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="27489-139">Lorsque le **`Trigger .NET instance method HelloHelper.SayHello`** bouton est sélectionné, `ExampleJsInterop.CallHelloHelperSayHello` est appelé et passe un nom, `Blazor` , à la méthode.</span><span class="sxs-lookup"><span data-stu-id="27489-139">When the **`Trigger .NET instance method HelloHelper.SayHello`** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="27489-140">`Pages/JsInterop.razor`:</span><span class="sxs-lookup"><span data-stu-id="27489-140">`Pages/JsInterop.razor`:</span></span>

```razor
<button type="button" class="btn btn-primary" @onclick="TriggerNetInstanceMethod">
    Trigger .NET instance method HelloHelper.SayHello
</button>

@code {
    public async Task TriggerNetInstanceMethod()
    {
        var exampleJsInterop = new ExampleJsInterop(JS);
        await exampleJsInterop.CallHelloHelperSayHello("Blazor");
    }
}
```

<span data-ttu-id="27489-141">`CallHelloHelperSayHello` appelle la fonction JavaScript `sayHello` avec une nouvelle instance de `HelloHelper` .</span><span class="sxs-lookup"><span data-stu-id="27489-141">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="27489-142">`JsInteropClasses/ExampleJsInterop.cs`:</span><span class="sxs-lookup"><span data-stu-id="27489-142">`JsInteropClasses/ExampleJsInterop.cs`:</span></span>

[!code-csharp[](./common/samples/5.x/BlazorWebAssemblySample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=11-18)]

<span data-ttu-id="27489-143">`wwwroot/exampleJsInterop.js`:</span><span class="sxs-lookup"><span data-stu-id="27489-143">`wwwroot/exampleJsInterop.js`:</span></span>

[!code-javascript[](./common/samples/5.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=15-18)]

<span data-ttu-id="27489-144">Le nom est passé au `HelloHelper` constructeur de, qui définit la `HelloHelper.Name` propriété.</span><span class="sxs-lookup"><span data-stu-id="27489-144">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="27489-145">Lorsque la fonction JavaScript `sayHello` est exécutée, `HelloHelper.SayHello` retourne le `Hello, {Name}!` message, qui est écrit dans la console par la fonction JavaScript.</span><span class="sxs-lookup"><span data-stu-id="27489-145">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="27489-146">`JsInteropClasses/HelloHelper.cs`:</span><span class="sxs-lookup"><span data-stu-id="27489-146">`JsInteropClasses/HelloHelper.cs`:</span></span>

[!code-csharp[](./common/samples/5.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="27489-147">Sortie de la console dans les outils de développement Web du navigateur :</span><span class="sxs-lookup"><span data-stu-id="27489-147">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

<span data-ttu-id="27489-148">Pour éviter une fuite de mémoire et autoriser garbage collection sur un composant qui crée un <xref:Microsoft.JSInterop.DotNetObjectReference> , adoptez l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="27489-148">To avoid a memory leak and allow garbage collection on a component that creates a <xref:Microsoft.JSInterop.DotNetObjectReference>, adopt one of the following approaches:</span></span>

* <span data-ttu-id="27489-149">Supprimez l’objet dans la classe qui a créé l' <xref:Microsoft.JSInterop.DotNetObjectReference> instance :</span><span class="sxs-lookup"><span data-stu-id="27489-149">Dispose of the object in the class that created the <xref:Microsoft.JSInterop.DotNetObjectReference> instance:</span></span>

  ```csharp
  public class ExampleJsInterop : IDisposable
  {
      private readonly IJSRuntime js;
      private DotNetObjectReference<HelloHelper> objRef;

      public ExampleJsInterop(IJSRuntime js)
      {
          this.js = js;
      }

      public ValueTask<string> CallHelloHelperSayHello(string name)
      {
          objRef = DotNetObjectReference.Create(new HelloHelper(name));

          return js.InvokeAsync<string>(
              "exampleJsFunctions.sayHello",
              objRef);
      }

      public void Dispose()
      {
          objRef?.Dispose();
      }
  }
  ```

  <span data-ttu-id="27489-150">Le modèle précédent présenté dans la `ExampleJsInterop` classe peut également être implémenté dans un composant :</span><span class="sxs-lookup"><span data-stu-id="27489-150">The preceding pattern shown in the `ExampleJsInterop` class can also be implemented in a component:</span></span>

  ```razor
  @page "/JSInteropComponent"
  @using {APP ASSEMBLY}.JsInteropClasses
  @implements IDisposable
  @inject IJSRuntime JS

  <h1>JavaScript Interop</h1>

  <button type="button" class="btn btn-primary" @onclick="TriggerNetInstanceMethod">
      Trigger .NET instance method HelloHelper.SayHello
  </button>

  @code {
      private DotNetObjectReference<HelloHelper> objRef;

      public async Task TriggerNetInstanceMethod()
      {
          objRef = DotNetObjectReference.Create(new HelloHelper("Blazor"));

          await JS.InvokeAsync<string>(
              "exampleJsFunctions.sayHello",
              objRef);
      }

      public void Dispose()
      {
          objRef?.Dispose();
      }
  }
  ```
  
  <span data-ttu-id="27489-151">L’espace réservé `{APP ASSEMBLY}` est le nom de l’assembly d’application de l’application (par exemple, `BlazorSample` ).</span><span class="sxs-lookup"><span data-stu-id="27489-151">The placeholder `{APP ASSEMBLY}` is the app's app assembly name (for example, `BlazorSample`).</span></span>

* <span data-ttu-id="27489-152">Lorsque le composant ou la classe ne supprime pas le <xref:Microsoft.JSInterop.DotNetObjectReference> , supprimez l’objet sur le client en appelant `.dispose()` :</span><span class="sxs-lookup"><span data-stu-id="27489-152">When the component or class doesn't dispose of the <xref:Microsoft.JSInterop.DotNetObjectReference>, dispose of the object on the client by calling `.dispose()`:</span></span>

  ```javascript
  window.myFunction = (dotnetHelper) => {
    dotnetHelper.invokeMethodAsync('{APP ASSEMBLY}', 'MyMethod');
    dotnetHelper.dispose();
  }
  ```

## <a name="component-instance-method-call"></a><span data-ttu-id="27489-153">Appel de méthode d’instance de composant</span><span class="sxs-lookup"><span data-stu-id="27489-153">Component instance method call</span></span>

<span data-ttu-id="27489-154">Pour appeler les méthodes .NET d’un composant :</span><span class="sxs-lookup"><span data-stu-id="27489-154">To invoke a component's .NET methods:</span></span>

* <span data-ttu-id="27489-155">Utilisez la `invokeMethod` `invokeMethodAsync` fonction ou pour effectuer un appel de méthode statique au composant.</span><span class="sxs-lookup"><span data-stu-id="27489-155">Use the `invokeMethod` or `invokeMethodAsync` function to make a static method call to the component.</span></span>
* <span data-ttu-id="27489-156">La méthode statique du composant encapsule l’appel à sa méthode d’instance en tant que appelé <xref:System.Action> .</span><span class="sxs-lookup"><span data-stu-id="27489-156">The component's static method wraps the call to its instance method as an invoked <xref:System.Action>.</span></span>

> [!NOTE]
> <span data-ttu-id="27489-157">Pour les Blazor Server applications, où plusieurs utilisateurs peuvent utiliser le même composant simultanément, utilisez une classe d’assistance pour appeler des méthodes d’instance.</span><span class="sxs-lookup"><span data-stu-id="27489-157">For Blazor Server apps, where several users might be concurrently using the same component, use a helper class to invoke instance methods.</span></span>
>
> <span data-ttu-id="27489-158">Pour plus d’informations, consultez la section de la [classe d’assistance de la méthode d’instance de composant](#component-instance-method-helper-class) .</span><span class="sxs-lookup"><span data-stu-id="27489-158">For more information, see the [Component instance method helper class](#component-instance-method-helper-class) section.</span></span>

<span data-ttu-id="27489-159">Dans le code JavaScript côté client :</span><span class="sxs-lookup"><span data-stu-id="27489-159">In the client-side JavaScript:</span></span>

```javascript
function updateMessageCallerJS() {
  DotNet.invokeMethodAsync('{APP ASSEMBLY}', 'UpdateMessageCaller');
}
```

<span data-ttu-id="27489-160">L’espace réservé `{APP ASSEMBLY}` est le nom de l’assembly d’application de l’application (par exemple, `BlazorSample` ).</span><span class="sxs-lookup"><span data-stu-id="27489-160">The placeholder `{APP ASSEMBLY}` is the app's app assembly name (for example, `BlazorSample`).</span></span>

<span data-ttu-id="27489-161">`Pages/JSInteropComponent.razor`:</span><span class="sxs-lookup"><span data-stu-id="27489-161">`Pages/JSInteropComponent.razor`:</span></span>

```razor
@page "/JSInteropComponent"

<p>
    Message: @message
</p>

<p>
    <button onclick="updateMessageCallerJS()">Call JS Method</button>
</p>

@code {
    private static Action action;
    private string message = "Select the button.";

    protected override void OnInitialized()
    {
        action = UpdateMessage;
    }

    private void UpdateMessage()
    {
        message = "UpdateMessage Called!";
        StateHasChanged();
    }

    [JSInvokable]
    public static void UpdateMessageCaller()
    {
        action.Invoke();
    }
}
```

<span data-ttu-id="27489-162">Pour passer des arguments à la méthode d’instance :</span><span class="sxs-lookup"><span data-stu-id="27489-162">To pass arguments to the instance method:</span></span>

* <span data-ttu-id="27489-163">Ajoutez des paramètres à l’appel de méthode JS.</span><span class="sxs-lookup"><span data-stu-id="27489-163">Add parameters to the JS method invocation.</span></span> <span data-ttu-id="27489-164">Dans l’exemple suivant, un nom est passé à la méthode.</span><span class="sxs-lookup"><span data-stu-id="27489-164">In the following example, a name is passed to the method.</span></span> <span data-ttu-id="27489-165">Des paramètres supplémentaires peuvent être ajoutés à la liste en fonction des besoins.</span><span class="sxs-lookup"><span data-stu-id="27489-165">Additional parameters can be added to the list as needed.</span></span>

  ```javascript
  function updateMessageCallerJS(name) {
    DotNet.invokeMethodAsync('{APP ASSEMBLY}', 'UpdateMessageCaller', name);
  }
  ```
  
  <span data-ttu-id="27489-166">L’espace réservé `{APP ASSEMBLY}` est le nom de l’assembly d’application de l’application (par exemple, `BlazorSample` ).</span><span class="sxs-lookup"><span data-stu-id="27489-166">The placeholder `{APP ASSEMBLY}` is the app's app assembly name (for example, `BlazorSample`).</span></span>

* <span data-ttu-id="27489-167">Fournissez les types corrects au <xref:System.Action> pour les paramètres.</span><span class="sxs-lookup"><span data-stu-id="27489-167">Provide the correct types to the <xref:System.Action> for the parameters.</span></span> <span data-ttu-id="27489-168">Fournissez la liste de paramètres aux méthodes C#.</span><span class="sxs-lookup"><span data-stu-id="27489-168">Provide the parameter list to the C# methods.</span></span> <span data-ttu-id="27489-169">Appelez <xref:System.Action> ( `UpdateMessage` ) avec les paramètres ( `action.Invoke(name)` ).</span><span class="sxs-lookup"><span data-stu-id="27489-169">Invoke the <xref:System.Action> (`UpdateMessage`) with the parameters (`action.Invoke(name)`).</span></span>

  <span data-ttu-id="27489-170">`Pages/JSInteropComponent.razor`:</span><span class="sxs-lookup"><span data-stu-id="27489-170">`Pages/JSInteropComponent.razor`:</span></span>

  ```razor
  @page "/JSInteropComponent"

  <p>
      Message: @message
  </p>

  <p>
      <button onclick="updateMessageCallerJS('Sarah Jane')">
          Call JS Method
      </button>
  </p>

  @code {
      private static Action<string> action;
      private string message = "Select the button.";

      protected override void OnInitialized()
      {
          action = UpdateMessage;
      }

      private void UpdateMessage(string name)
      {
          message = $"{name}, UpdateMessage Called!";
          StateHasChanged();
      }

      [JSInvokable]
      public static void UpdateMessageCaller(string name)
      {
          action.Invoke(name);
      }
  }
  ```

  <span data-ttu-id="27489-171">Sortie `message` lorsque le bouton **Call js Method** est sélectionné :</span><span class="sxs-lookup"><span data-stu-id="27489-171">Output `message` when the **Call JS Method** button is selected:</span></span>

  ```
  Sarah Jane, UpdateMessage Called!
  ```

## <a name="component-instance-method-helper-class"></a><span data-ttu-id="27489-172">Classe d’assistance de méthode d’instance de composant</span><span class="sxs-lookup"><span data-stu-id="27489-172">Component instance method helper class</span></span>

<span data-ttu-id="27489-173">La classe d’assistance est utilisée pour appeler une méthode d’instance en tant que <xref:System.Action> .</span><span class="sxs-lookup"><span data-stu-id="27489-173">The helper class is used to invoke an instance method as an <xref:System.Action>.</span></span> <span data-ttu-id="27489-174">Les classes d’assistance sont utiles dans les cas suivants :</span><span class="sxs-lookup"><span data-stu-id="27489-174">Helper classes are useful when:</span></span>

* <span data-ttu-id="27489-175">Plusieurs composants du même type sont rendus sur la même page.</span><span class="sxs-lookup"><span data-stu-id="27489-175">Several components of the same type are rendered on the same page.</span></span>
* <span data-ttu-id="27489-176">Une Blazor Server application est utilisée, où plusieurs utilisateurs peuvent utiliser un composant simultanément.</span><span class="sxs-lookup"><span data-stu-id="27489-176">A Blazor Server app is used, where multiple users might be using a component concurrently.</span></span>

<span data-ttu-id="27489-177">Dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="27489-177">In the following example:</span></span>

* <span data-ttu-id="27489-178">Le `JSInteropExample` composant contient plusieurs `ListItem` composants.</span><span class="sxs-lookup"><span data-stu-id="27489-178">The `JSInteropExample` component contains several `ListItem` components.</span></span>
* <span data-ttu-id="27489-179">Chaque `ListItem` composant se compose d’un message et d’un bouton.</span><span class="sxs-lookup"><span data-stu-id="27489-179">Each `ListItem` component is composed of a message and a button.</span></span>
* <span data-ttu-id="27489-180">Quand un `ListItem` bouton de composant est sélectionné, `ListItem` la `UpdateMessage` méthode modifie le texte de l’élément de liste et masque le bouton.</span><span class="sxs-lookup"><span data-stu-id="27489-180">When a `ListItem` component button is selected, that `ListItem`'s `UpdateMessage` method changes the list item text and hides the button.</span></span>

<span data-ttu-id="27489-181">`MessageUpdateInvokeHelper.cs`:</span><span class="sxs-lookup"><span data-stu-id="27489-181">`MessageUpdateInvokeHelper.cs`:</span></span>

```csharp
using System;
using Microsoft.JSInterop;

public class MessageUpdateInvokeHelper
{
    private Action action;

    public MessageUpdateInvokeHelper(Action action)
    {
        this.action = action;
    }

    [JSInvokable("{APP ASSEMBLY}")]
    public void UpdateMessageCaller()
    {
        action.Invoke();
    }
}
```

<span data-ttu-id="27489-182">L’espace réservé `{APP ASSEMBLY}` est le nom de l’assembly d’application de l’application (par exemple, `BlazorSample` ).</span><span class="sxs-lookup"><span data-stu-id="27489-182">The placeholder `{APP ASSEMBLY}` is the app's app assembly name (for example, `BlazorSample`).</span></span>

<span data-ttu-id="27489-183">Dans le code JavaScript côté client :</span><span class="sxs-lookup"><span data-stu-id="27489-183">In the client-side JavaScript:</span></span>

```javascript
window.updateMessageCallerJS = (dotnetHelper) => {
    dotnetHelper.invokeMethodAsync('{APP ASSEMBLY}', 'UpdateMessageCaller');
    dotnetHelper.dispose();
}
```

<span data-ttu-id="27489-184">L’espace réservé `{APP ASSEMBLY}` est le nom de l’assembly d’application de l’application (par exemple, `BlazorSample` ).</span><span class="sxs-lookup"><span data-stu-id="27489-184">The placeholder `{APP ASSEMBLY}` is the app's app assembly name (for example, `BlazorSample`).</span></span>

<span data-ttu-id="27489-185">`Shared/ListItem.razor`:</span><span class="sxs-lookup"><span data-stu-id="27489-185">`Shared/ListItem.razor`:</span></span>

```razor
@inject IJSRuntime JS

<li>
    @message
    <button @onclick="InteropCall" style="display:@display">InteropCall</button>
</li>

@code {
    private string message = "Select one of these list item buttons.";
    private string display = "inline-block";
    private MessageUpdateInvokeHelper messageUpdateInvokeHelper;

    protected override void OnInitialized()
    {
        messageUpdateInvokeHelper = new MessageUpdateInvokeHelper(UpdateMessage);
    }

    protected async Task InteropCall()
    {
        await JS.InvokeVoidAsync("updateMessageCallerJS",
            DotNetObjectReference.Create(messageUpdateInvokeHelper));
    }

    private void UpdateMessage()
    {
        message = "UpdateMessage Called!";
        display = "none";
        StateHasChanged();
    }
}
```

<span data-ttu-id="27489-186">`Pages/JSInteropExample.razor`:</span><span class="sxs-lookup"><span data-stu-id="27489-186">`Pages/JSInteropExample.razor`:</span></span>

```razor
@page "/JSInteropExample"

<h1>List of components</h1>

<ul>
    <ListItem />
    <ListItem />
    <ListItem />
    <ListItem />
</ul>
```

[!INCLUDE[](~/blazor/includes/share-interop-code.md)]

## <a name="avoid-circular-object-references"></a><span data-ttu-id="27489-187">Éviter les références d’objets circulaires</span><span class="sxs-lookup"><span data-stu-id="27489-187">Avoid circular object references</span></span>

<span data-ttu-id="27489-188">Les objets qui contiennent des références circulaires ne peuvent pas être sérialisés sur le client pour :</span><span class="sxs-lookup"><span data-stu-id="27489-188">Objects that contain circular references can't be serialized on the client for either:</span></span>

* <span data-ttu-id="27489-189">Appels de méthode .NET.</span><span class="sxs-lookup"><span data-stu-id="27489-189">.NET method calls.</span></span>
* <span data-ttu-id="27489-190">Appels de méthode JavaScript à partir de C# lorsque le type de retour a des références circulaires.</span><span class="sxs-lookup"><span data-stu-id="27489-190">JavaScript method calls from C# when the return type has circular references.</span></span>

<span data-ttu-id="27489-191">Pour plus d’informations, consultez les problèmes suivants :</span><span class="sxs-lookup"><span data-stu-id="27489-191">For more information, see the following issues:</span></span>

* [<span data-ttu-id="27489-192">Les références circulaires ne sont pas prises en charge, prennent deux (dotnet/aspnetcore #20525)</span><span class="sxs-lookup"><span data-stu-id="27489-192">Circular references are not supported, take two (dotnet/aspnetcore #20525)</span></span>](https://github.com/dotnet/aspnetcore/issues/20525)
* [<span data-ttu-id="27489-193">Proposition : ajouter un mécanisme pour gérer les références circulaires lors de la sérialisation (dotnet/Runtime #30820)</span><span class="sxs-lookup"><span data-stu-id="27489-193">Proposal: Add mechanism to handle circular references when serializing (dotnet/runtime #30820)</span></span>](https://github.com/dotnet/runtime/issues/30820)

## <a name="size-limits-on-js-interop-calls"></a><span data-ttu-id="27489-194">Limites de taille sur les appels d’interopérabilité JS</span><span class="sxs-lookup"><span data-stu-id="27489-194">Size limits on JS interop calls</span></span>

<span data-ttu-id="27489-195">Dans Blazor WebAssembly , l’infrastructure n’impose pas de limite sur la taille des entrées et sorties d’interopérabilité js.</span><span class="sxs-lookup"><span data-stu-id="27489-195">In Blazor WebAssembly, the framework doesn't impose a limit on the size of JS interop inputs and outputs.</span></span>

<span data-ttu-id="27489-196">Dans Blazor Server , les appels d’interopérabilité js ont une taille limitée par la SignalR taille maximale autorisée pour les méthodes de concentrateur, qui est appliquée par <xref:Microsoft.AspNetCore.SignalR.HubOptions.MaximumReceiveMessageSize?displayProperty=nameWithType> (valeur par défaut : 32 Ko).</span><span class="sxs-lookup"><span data-stu-id="27489-196">In Blazor Server, JS interop calls are limited in size by the maximum incoming SignalR message size permitted for hub methods, which is enforced by <xref:Microsoft.AspNetCore.SignalR.HubOptions.MaximumReceiveMessageSize?displayProperty=nameWithType> (default: 32 KB).</span></span> <span data-ttu-id="27489-197">JS en SignalR messages .net plus grands que ne <xref:Microsoft.AspNetCore.SignalR.HubOptions.MaximumReceiveMessageSize> génèrent une erreur.</span><span class="sxs-lookup"><span data-stu-id="27489-197">JS to .NET SignalR messages larger than <xref:Microsoft.AspNetCore.SignalR.HubOptions.MaximumReceiveMessageSize> throw an error.</span></span> <span data-ttu-id="27489-198">L’infrastructure n’impose pas de limite sur la taille d’un SignalR message du concentrateur à un client.</span><span class="sxs-lookup"><span data-stu-id="27489-198">The framework doesn't impose a limit on the size of a SignalR message from the hub to a client.</span></span>

<span data-ttu-id="27489-199">Lorsque SignalR la journalisation n’est pas définie sur [Debug](xref:Microsoft.Extensions.Logging.LogLevel) ou [trace](xref:Microsoft.Extensions.Logging.LogLevel), une erreur de taille de message s’affiche uniquement dans la console outils de développement du navigateur :</span><span class="sxs-lookup"><span data-stu-id="27489-199">When SignalR logging isn't set to [Debug](xref:Microsoft.Extensions.Logging.LogLevel) or [Trace](xref:Microsoft.Extensions.Logging.LogLevel), a message size error only appears in the browser's developer tools console:</span></span>

> <span data-ttu-id="27489-200">Erreur : connexion déconnectée avec l’erreur’erreur : le serveur a retourné une erreur lors de la fermeture : la connexion a été fermée avec une erreur. '.</span><span class="sxs-lookup"><span data-stu-id="27489-200">Error: Connection disconnected with error 'Error: Server returned an error on close: Connection closed with an error.'.</span></span>

<span data-ttu-id="27489-201">Lorsque la [ SignalR journalisation côté serveur](xref:signalr/diagnostics#server-side-logging) est définie sur [Debug](xref:Microsoft.Extensions.Logging.LogLevel) ou [trace](xref:Microsoft.Extensions.Logging.LogLevel), la journalisation côté serveur couvre un <xref:System.IO.InvalidDataException> pour une erreur de taille de message.</span><span class="sxs-lookup"><span data-stu-id="27489-201">When [SignalR server-side logging](xref:signalr/diagnostics#server-side-logging) is set to [Debug](xref:Microsoft.Extensions.Logging.LogLevel) or [Trace](xref:Microsoft.Extensions.Logging.LogLevel), server-side logging surfaces an <xref:System.IO.InvalidDataException> for a message size error.</span></span>

<span data-ttu-id="27489-202">`appsettings.Development.json`:</span><span class="sxs-lookup"><span data-stu-id="27489-202">`appsettings.Development.json`:</span></span>

```json
{
  "DetailedErrors": true,
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information",
      "Microsoft.AspNetCore.SignalR": "Debug"
    }
  }
}
```

> <span data-ttu-id="27489-203">System. IO. InvalidDataException : la taille de message maximale de 32768B a été dépassée.</span><span class="sxs-lookup"><span data-stu-id="27489-203">System.IO.InvalidDataException: The maximum message size of 32768B was exceeded.</span></span> <span data-ttu-id="27489-204">La taille du message peut être configurée dans AddHubOptions.</span><span class="sxs-lookup"><span data-stu-id="27489-204">The message size can be configured in AddHubOptions.</span></span>

<span data-ttu-id="27489-205">Augmentez la limite en définissant <xref:Microsoft.AspNetCore.SignalR.HubOptions.MaximumReceiveMessageSize> dans `Startup.ConfigureServices` .</span><span class="sxs-lookup"><span data-stu-id="27489-205">Increase the limit by setting <xref:Microsoft.AspNetCore.SignalR.HubOptions.MaximumReceiveMessageSize> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="27489-206">L’exemple suivant définit la taille maximale des messages de réception sur 64 Ko (64 \* 1024) :</span><span class="sxs-lookup"><span data-stu-id="27489-206">The following example sets the maximum receive message size to 64 KB (64 \* 1024):</span></span>

```csharp
services.AddServerSideBlazor()
   .AddHubOptions(options => options.MaximumReceiveMessageSize = 64 * 1024);
```

<span data-ttu-id="27489-207">Si vous augmentez la SignalR limite de taille des messages entrants, vous avez besoin de davantage de ressources serveur et cela expose le serveur à des risques accrus d’un utilisateur malveillant.</span><span class="sxs-lookup"><span data-stu-id="27489-207">Increasing the SignalR incoming message size limit comes at the cost of requiring more server resources, and it exposes the server to increased risks from a malicious user.</span></span> <span data-ttu-id="27489-208">En outre, la lecture d’une grande quantité de contenu dans la mémoire en tant que chaînes ou tableaux d’octets peut également entraîner des allocations qui fonctionnent mal avec le garbage collector, ce qui entraîne des pénalités en matière de performances.</span><span class="sxs-lookup"><span data-stu-id="27489-208">Additionally, reading a large amount of content in to memory as strings or byte arrays can also result in allocations that work poorly with the garbage collector, resulting in additional performance penalties.</span></span>

<span data-ttu-id="27489-209">L’une des options de lecture des charges utiles volumineuses consiste à envoyer le contenu en plus petits segments et à traiter la charge utile en tant que <xref:System.IO.Stream> .</span><span class="sxs-lookup"><span data-stu-id="27489-209">One option for reading large payloads is to send the content in smaller chunks and process the payload as a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="27489-210">Cela peut être utilisé lors de la lecture de charges utiles JSON volumineuses ou si les données sont disponibles en tant qu’octets bruts dans JavaScript.</span><span class="sxs-lookup"><span data-stu-id="27489-210">This can be used when reading large JSON payloads or if data is available in JavaScript as raw bytes.</span></span> <span data-ttu-id="27489-211">Pour obtenir un exemple qui montre comment envoyer des charges utiles binaires volumineuses dans Blazor Server qui utilise des techniques similaires au `InputFile` composant, consultez l' [exemple d’application de soumission binaire](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/BinarySubmit).</span><span class="sxs-lookup"><span data-stu-id="27489-211">For an example that demonstrates sending large binary payloads in Blazor Server that uses techniques similar to the `InputFile` component, see the [Binary Submit sample app](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/BinarySubmit).</span></span>

<span data-ttu-id="27489-212">Tenez compte des conseils suivants lors du développement de code qui transfère une grande quantité de données entre JavaScript et Blazor :</span><span class="sxs-lookup"><span data-stu-id="27489-212">Consider the following guidance when developing code that transfers a large amount of data between JavaScript and Blazor:</span></span>

* <span data-ttu-id="27489-213">Découpez les données en éléments plus petits et envoyez les segments de données de façon séquentielle jusqu’à ce que toutes les données soient reçues par le serveur.</span><span class="sxs-lookup"><span data-stu-id="27489-213">Slice the data into smaller pieces, and send the data segments sequentially until all of the data is received by the server.</span></span>
* <span data-ttu-id="27489-214">N’allouez pas d’objets volumineux dans du code JavaScript et C#.</span><span class="sxs-lookup"><span data-stu-id="27489-214">Don't allocate large objects in JavaScript and C# code.</span></span>
* <span data-ttu-id="27489-215">Ne bloquez pas le thread d’interface utilisateur principal pendant de longues périodes lors de l’envoi ou de la réception de données.</span><span class="sxs-lookup"><span data-stu-id="27489-215">Don't block the main UI thread for long periods when sending or receiving data.</span></span>
* <span data-ttu-id="27489-216">Libérez la mémoire consommée lorsque le processus est terminé ou annulé.</span><span class="sxs-lookup"><span data-stu-id="27489-216">Free any memory consumed when the process is completed or cancelled.</span></span>
* <span data-ttu-id="27489-217">Appliquer les exigences supplémentaires suivantes pour des raisons de sécurité :</span><span class="sxs-lookup"><span data-stu-id="27489-217">Enforce the following additional requirements for security purposes:</span></span>
  * <span data-ttu-id="27489-218">Déclarez la taille maximale du fichier ou des données qui peut être passée.</span><span class="sxs-lookup"><span data-stu-id="27489-218">Declare the maximum file or data size that can be passed.</span></span>
  * <span data-ttu-id="27489-219">Déclarez le taux de téléchargement minimal du client au serveur.</span><span class="sxs-lookup"><span data-stu-id="27489-219">Declare the minimum upload rate from the client to the server.</span></span>
* <span data-ttu-id="27489-220">Une fois les données reçues par le serveur, les données peuvent être :</span><span class="sxs-lookup"><span data-stu-id="27489-220">After the data is received by the server, the data can be:</span></span>
  * <span data-ttu-id="27489-221">Stocké temporairement dans une mémoire tampon jusqu’à ce que tous les segments soient collectés.</span><span class="sxs-lookup"><span data-stu-id="27489-221">Temporarily stored in a memory buffer until all of the segments are collected.</span></span>
  * <span data-ttu-id="27489-222">Consommé immédiatement.</span><span class="sxs-lookup"><span data-stu-id="27489-222">Consumed immediately.</span></span> <span data-ttu-id="27489-223">Par exemple, les données peuvent être stockées immédiatement dans une base de données ou écrites sur le disque à mesure que chaque segment est reçu.</span><span class="sxs-lookup"><span data-stu-id="27489-223">For example, the data can be stored immediately in a database or written to disk as each segment is received.</span></span>

## <a name="js-modules"></a><span data-ttu-id="27489-224">Modules JS</span><span class="sxs-lookup"><span data-stu-id="27489-224">JS modules</span></span>

<span data-ttu-id="27489-225">Pour l’isolation de JS, l’interopérabilité de JS fonctionne avec la prise en charge par défaut du navigateur pour les [modules ECMAScript (ESM)](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Modules) ([spécification ECMAScript](https://tc39.es/ecma262/#sec-modules)).</span><span class="sxs-lookup"><span data-stu-id="27489-225">For JS isolation, JS interop works with the browser's default support for [EcmaScript modules (ESM)](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Modules) ([ECMAScript specification](https://tc39.es/ecma262/#sec-modules)).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="27489-226">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="27489-226">Additional resources</span></span>

* <xref:blazor/call-javascript-from-dotnet>
* [<span data-ttu-id="27489-227">`InteropComponent.razor` exemple (référentiel GitHub dotnet/AspNetCore, branche de version 3,1)</span><span class="sxs-lookup"><span data-stu-id="27489-227">`InteropComponent.razor` example (dotnet/AspNetCore GitHub repository, 3.1 release branch)</span></span>](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
