---
title: Interoperabilidade de JavaScript de componentes do Razor
author: guardrex
description: Saiba como chamar funções de JavaScript do .NET e .NET métodos do JavaScript em aplicativos Blazor e componentes do Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/javascript-interop
ms.openlocfilehash: 9f822fee8990b03ff15ffa9857a2ddd95328a6ce
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57050363"
---
# <a name="razor-components-javascript-interop"></a><span data-ttu-id="a93f8-103">Interoperabilidade de JavaScript de componentes do Razor</span><span class="sxs-lookup"><span data-stu-id="a93f8-103">Razor Components JavaScript interop</span></span>

<span data-ttu-id="a93f8-104">Por [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a93f8-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="a93f8-105">Um aplicativo de componentes do Razor pode invocar funções de JavaScript do .NET e .NET métodos do código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a93f8-105">A Razor Components app can invoke JavaScript functions from .NET and .NET methods from JavaScript code.</span></span>

## <a name="invoke-javascript-functions-from-net-methods"></a><span data-ttu-id="a93f8-106">Invocar funções de JavaScript de métodos do .NET</span><span class="sxs-lookup"><span data-stu-id="a93f8-106">Invoke JavaScript functions from .NET methods</span></span>

<span data-ttu-id="a93f8-107">Há ocasiões em que o código .NET é necessário para chamar uma função de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a93f8-107">There are times when .NET code is required to call a JavaScript function.</span></span> <span data-ttu-id="a93f8-108">Por exemplo, uma chamada de JavaScript pode expor recursos do navegador ou a funcionalidade de uma biblioteca de JavaScript para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="a93f8-108">For example, a JavaScript call can expose browser capabilities or functionality from a JavaScript library to the app.</span></span>

<span data-ttu-id="a93f8-109">Para chamar JavaScript do .NET, use o `IJSRuntime` abstração.</span><span class="sxs-lookup"><span data-stu-id="a93f8-109">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="a93f8-110">O `InvokeAsync<T>` método no `IJSRuntime` usa um identificador para a função de JavaScript que você deseja chamar juntamente com qualquer número de argumentos serializável em JSON.</span><span class="sxs-lookup"><span data-stu-id="a93f8-110">The `InvokeAsync<T>` method on `IJSRuntime` takes an identifier for the JavaScript function you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="a93f8-111">O identificador de função é relativo ao escopo global (`window`).</span><span class="sxs-lookup"><span data-stu-id="a93f8-111">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="a93f8-112">Se você deseja chamar `window.someScope.someFunction`, o identificador é `someScope.someFunction`.</span><span class="sxs-lookup"><span data-stu-id="a93f8-112">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="a93f8-113">Não é necessário para registrar a função antes que ele é chamado.</span><span class="sxs-lookup"><span data-stu-id="a93f8-113">There's no need to register the function before it's called.</span></span> <span data-ttu-id="a93f8-114">O tipo de retorno `T` JSON deve ser serializável.</span><span class="sxs-lookup"><span data-stu-id="a93f8-114">The return type `T` must also be JSON serializable.</span></span>

<span data-ttu-id="a93f8-115">Para aplicativos do ASP.NET Core Razor componentes do lado do servidor:</span><span class="sxs-lookup"><span data-stu-id="a93f8-115">For server-side ASP.NET Core Razor Components apps:</span></span>

* <span data-ttu-id="a93f8-116">Várias solicitações de usuário são processadas pelo aplicativo do lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="a93f8-116">Multiple user requests are processed by the server-side app.</span></span> <span data-ttu-id="a93f8-117">Não chame `JSRuntime.Current` em um componente para invocar funções de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a93f8-117">Don't call `JSRuntime.Current` in a component to invoke JavaScript functions.</span></span>
* <span data-ttu-id="a93f8-118">Injetar o `IJSRuntime` abstração e use o objeto injetado para emitir chamadas de interoperabilidade de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a93f8-118">Inject the `IJSRuntime` abstraction and use the injected object to issue JavaScript interop calls.</span></span>

<span data-ttu-id="a93f8-119">O exemplo a seguir se baseia [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), um decodificador de baseados em JavaScript experimental.</span><span class="sxs-lookup"><span data-stu-id="a93f8-119">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), an experimental JavaScript-based decoder.</span></span> <span data-ttu-id="a93f8-120">O exemplo demonstra como invocar uma função de JavaScript de um C# método.</span><span class="sxs-lookup"><span data-stu-id="a93f8-120">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="a93f8-121">A função JavaScript aceita uma matriz de bytes de um C# método, decodifica a matriz e retorna o texto para o componente para exibição.</span><span class="sxs-lookup"><span data-stu-id="a93f8-121">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="a93f8-122">Dentro de `<head>` elemento de *wwwroot/index.html*, fornecer uma função que usa `TextDecoder` para decodificar uma matriz passada:</span><span class="sxs-lookup"><span data-stu-id="a93f8-122">Inside the `<head>` element of *wwwroot/index.html*, provide a function that uses `TextDecoder` to decode a passed array:</span></span>

```html
<script>
  window.ConvertArray = (win1251Array) => {
    var win1251decoder = new TextDecoder('windows-1251');
    var bytes = new Uint8Array(win1251Array);
    var decodedArray = win1251decoder.decode(bytes);
    console.log(decodedArray);
    return decodedArray;
  };
</script>
```

<span data-ttu-id="a93f8-123">Código de JavaScript, como o código mostrado no exemplo anterior, também pode ser carregado por um arquivo JavaScript com uma referência para o arquivo de script do *wwwroot/index.html* arquivo.</span><span class="sxs-lookup"><span data-stu-id="a93f8-123">JavaScript code, such as the code shown in the preceding example, can also be loaded by a JavaScript file with a reference to the script file in the *wwwroot/index.html* file.</span></span>

<span data-ttu-id="a93f8-124">O componente a seguir:</span><span class="sxs-lookup"><span data-stu-id="a93f8-124">The following component:</span></span>

* <span data-ttu-id="a93f8-125">Invoca o `ConvertArray` usando de função JavaScript `JsRuntime` quando um botão de componente (**converter matriz**) está selecionado.</span><span class="sxs-lookup"><span data-stu-id="a93f8-125">Invokes the `ConvertArray` JavaScript function using `JsRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="a93f8-126">Depois que a função JavaScript é chamada, a matriz passada é convertida em uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="a93f8-126">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="a93f8-127">A cadeia de caracteres é retornada para o componente para exibição.</span><span class="sxs-lookup"><span data-stu-id="a93f8-127">The string is returned to the component for display.</span></span>

```cshtml
@page "/"
@using Microsoft.JSInterop;
@inject IJSRuntime JsRuntime;

<h1>Call JavaScript Function Example</h1>

<button type="button" class="btn btn-primary" onclick="@ConvertArray">
    Convert Array
</button>

<p class="mt-2" style="font-size:1.6em">
    <span class="badge badge-success">
        @ConvertedText
    </span>
</p>

@functions {
    // Quote (c)2005 Universal Pictures: Serenity
    // https://www.uphe.com/movies/serenity
    // David Krumholtz on IMDB: https://www.imdb.com/name/nm0472710/

    private MarkupString ConvertedText =
        new MarkupString("Select the <b>Convert Array</b> button.");

    private uint[] QuoteArray = new uint[]
        {
            60, 101, 109, 62, 67, 97, 110, 39, 116, 32, 115, 116, 111, 112, 32,
            116, 104, 101, 32, 115, 105, 103, 110, 97, 108, 44, 32, 77, 97,
            108, 46, 60, 47, 101, 109, 62, 32, 45, 32, 77, 114, 46, 32, 85, 110,
            105, 118, 101, 114, 115, 101, 10, 10,
        };

    async void ConvertArray()
    {
        var text =
            await JsRuntime.InvokeAsync<string>("ConvertArray", QuoteArray);

        ConvertedText = new MarkupString(text);

        StateHasChanged();
    }
}
```

<span data-ttu-id="a93f8-128">Para aplicativos do lado do cliente Blazor, o `IJSRuntime` abstração é acessível de `JSRuntime.Current`, que se refere a solicitação do usuário atual.</span><span class="sxs-lookup"><span data-stu-id="a93f8-128">For client-side Blazor apps, the `IJSRuntime` abstraction is accessible from `JSRuntime.Current`, which refers to the current user's request.</span></span> <span data-ttu-id="a93f8-129">Como há apenas um usuário de um aplicativo de Blazor do lado do cliente, usando `JSRuntime.Current` para invocar um JavaScript função funciona normalmente.</span><span class="sxs-lookup"><span data-stu-id="a93f8-129">Because there's only a single user of a client-side Blazor app, using `JSRuntime.Current` to invoke a JavaScript function works normally.</span></span> <span data-ttu-id="a93f8-130">Use somente `JSRuntime.Current` em aplicativos do lado do cliente Blazor.</span><span class="sxs-lookup"><span data-stu-id="a93f8-130">Only use `JSRuntime.Current` in client-side Blazor apps.</span></span>

<span data-ttu-id="a93f8-131">No aplicativo de exemplo de cliente que acompanha este tópico, duas funções de JavaScript estão disponíveis para o aplicativo do lado do cliente que interagem com o DOM para receber entrada do usuário e exibir uma mensagem de boas-vinda:</span><span class="sxs-lookup"><span data-stu-id="a93f8-131">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the client-side app that interact with the DOM to receive user input and display a welcome message:</span></span>

* <span data-ttu-id="a93f8-132">`showPrompt` &ndash; Produz um prompt para aceitar a entrada do usuário (o nome do usuário) e retorna o nome para o chamador.</span><span class="sxs-lookup"><span data-stu-id="a93f8-132">`showPrompt` &ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* <span data-ttu-id="a93f8-133">`displayWelcome` &ndash; Atribui uma mensagem de boas-vinda do chamador para um objeto DOM com um `id` de `welcome`.</span><span class="sxs-lookup"><span data-stu-id="a93f8-133">`displayWelcome` &ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="a93f8-134">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="a93f8-134">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="a93f8-135">Coloque o `<script>` marca que faz referência ao arquivo JavaScript na *wwwroot/index.html* arquivo:</span><span class="sxs-lookup"><span data-stu-id="a93f8-135">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file:</span></span>

[!code-html[](./common/samples/3.x/BlazorSample/wwwroot/index.html?highlight=16)]

<span data-ttu-id="a93f8-136">Não coloque uma marca de script em um arquivo de componente porque a marca de script não pode ser atualizada dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="a93f8-136">Don't place a script tag in a component file because the script tag can't be updated dynamically.</span></span>

<span data-ttu-id="a93f8-137">Interoperabilidade de métodos do .NET com as funções de JavaScript, chamando `InvokeAsync<T>` método no `IJSRuntime`.</span><span class="sxs-lookup"><span data-stu-id="a93f8-137">.NET methods interop with the JavaScript functions by calling `InvokeAsync<T>` method on `IJSRuntime`.</span></span>

<span data-ttu-id="a93f8-138">O aplicativo de exemplo usa um par de C# métodos, `Prompt` e `Display`, para invocar o `showPrompt` e `displayWelcome` funções JavaScript:</span><span class="sxs-lookup"><span data-stu-id="a93f8-138">The sample app uses a pair of C# methods, `Prompt` and `Display`, to invoke the `showPrompt` and `displayWelcome` JavaScript functions:</span></span>

<span data-ttu-id="a93f8-139">*JsInteropClasses/ExampleJsInterop.cs*:</span><span class="sxs-lookup"><span data-stu-id="a93f8-139">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=6-8,14-16)]

<span data-ttu-id="a93f8-140">O `IJSRuntime` abstração é assíncrona para permitir cenários de servidor.</span><span class="sxs-lookup"><span data-stu-id="a93f8-140">The `IJSRuntime` abstraction is asynchronous to allow for server-side scenarios.</span></span> <span data-ttu-id="a93f8-141">Se o aplicativo é executado no cliente, e você deseja invocar uma função JavaScript de forma síncrona, baixá-los para `IJSInProcessRuntime` e chamar `Invoke<T>` em vez disso.</span><span class="sxs-lookup"><span data-stu-id="a93f8-141">If the app runs client-side and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="a93f8-142">É recomendável que melhor uso de interoperabilidade de bibliotecas JavaScript async APIs para garantir que as bibliotecas estão disponíveis em todos os cenários, cliente ou servidor.</span><span class="sxs-lookup"><span data-stu-id="a93f8-142">We recommend that most JavaScript interop libraries use the async APIs to ensure the libraries are available in all scenarios, client-side or server-side.</span></span>

<span data-ttu-id="a93f8-143">O aplicativo de exemplo inclui um componente para demonstrar a interoperabilidade JS.</span><span class="sxs-lookup"><span data-stu-id="a93f8-143">The sample app includes a component to demonstrate JS interop.</span></span> <span data-ttu-id="a93f8-144">O componente:</span><span class="sxs-lookup"><span data-stu-id="a93f8-144">The component:</span></span>

* <span data-ttu-id="a93f8-145">Recebe entrada do usuário por meio de um prompt de JS.</span><span class="sxs-lookup"><span data-stu-id="a93f8-145">Receives user input via a JS prompt.</span></span>
* <span data-ttu-id="a93f8-146">Retorna o texto para o componente para processamento.</span><span class="sxs-lookup"><span data-stu-id="a93f8-146">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="a93f8-147">Chama uma segunda função JS que interage com o DOM para exibir uma mensagem de boas-vinda.</span><span class="sxs-lookup"><span data-stu-id="a93f8-147">Calls a second JS function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="a93f8-148">*Pages/JSInterop.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a93f8-148">*Pages/JSInterop.cshtml*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=1&end=21&highlight=2-3,9-11,13,16-20)]

1. <span data-ttu-id="a93f8-149">Quando `TriggerJsPrompt` é executado, selecionando o componente **gatilho JavaScript Prompt** botão, o `ExampleJsInterop.Prompt` método em C# código é chamado.</span><span class="sxs-lookup"><span data-stu-id="a93f8-149">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the `ExampleJsInterop.Prompt` method in C# code is called.</span></span>
1. <span data-ttu-id="a93f8-150">O `Prompt` método executa o JavaScript `showPrompt` função fornecida na *wwwroot/exampleJsInterop.js* arquivo.</span><span class="sxs-lookup"><span data-stu-id="a93f8-150">The `Prompt` method executes the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file.</span></span>
1. <span data-ttu-id="a93f8-151">O `showPrompt` função aceita entrada do usuário (o nome do usuário), que é codificada em HTML e retornado para o `Prompt` método e, por fim, de volta para o componente.</span><span class="sxs-lookup"><span data-stu-id="a93f8-151">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the `Prompt` method and ultimately back to the component.</span></span> <span data-ttu-id="a93f8-152">O componente armazena o nome do usuário em uma variável local, `name`.</span><span class="sxs-lookup"><span data-stu-id="a93f8-152">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="a93f8-153">A cadeia de caracteres armazenados em `name` incorporado a uma mensagem de boas-vinda, que é passada para um segundo C# método `ExampleJsInterop.Display`.</span><span class="sxs-lookup"><span data-stu-id="a93f8-153">The string stored in `name` is incorporated into a welcome message, which is passed to a second C# method, `ExampleJsInterop.Display`.</span></span>
1. <span data-ttu-id="a93f8-154">`Display` chama uma função de JavaScript, `displayWelcome`, que processa a mensagem de boas-vinda em uma marca de título.</span><span class="sxs-lookup"><span data-stu-id="a93f8-154">`Display` calls a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="capture-references-to-elements"></a><span data-ttu-id="a93f8-155">Referências a elementos de captura</span><span class="sxs-lookup"><span data-stu-id="a93f8-155">Capture references to elements</span></span>

<span data-ttu-id="a93f8-156">Alguns [interoperabilidade de JavaScript](xref:razor-components/javascript-interop) cenários exigem que as referências a elementos HTML.</span><span class="sxs-lookup"><span data-stu-id="a93f8-156">Some [JavaScript interop](xref:razor-components/javascript-interop) scenarios require references to HTML elements.</span></span> <span data-ttu-id="a93f8-157">Por exemplo, uma biblioteca de interface do usuário pode exigir uma referência de elemento para a inicialização, ou talvez você precise chamar APIs do tipo de comando em um elemento, como `focus` ou `play`.</span><span class="sxs-lookup"><span data-stu-id="a93f8-157">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="a93f8-158">Você pode capturar as referências a elementos HTML em um componente, adicionando um `ref` de atributos para o elemento HTML e, em seguida, definindo um campo do tipo `ElementRef` cujo nome corresponde ao valor do `ref` atributo.</span><span class="sxs-lookup"><span data-stu-id="a93f8-158">You can capture references to HTML elements in a component by adding a `ref` attribute to the HTML element and then defining a field of type `ElementRef` whose name matches the value of the `ref` attribute.</span></span>

<span data-ttu-id="a93f8-159">O exemplo a seguir mostra como capturar uma referência para o elemento de entrada de nome de usuário:</span><span class="sxs-lookup"><span data-stu-id="a93f8-159">The following example shows capturing a reference to the username input element:</span></span>

```csharp
<input ref="username" ... />

@functions {
    ElementRef username;
}
```

> [!NOTE]
> <span data-ttu-id="a93f8-160">Fazer **não** usar referências de elemento capturado como uma maneira de preencher o DOM.</span><span class="sxs-lookup"><span data-stu-id="a93f8-160">Do **not** use captured element references as a way of populating the DOM.</span></span> <span data-ttu-id="a93f8-161">Isso pode interferir com o modelo de renderização declarativa.</span><span class="sxs-lookup"><span data-stu-id="a93f8-161">Doing so may interfere with the declarative rendering model.</span></span>

<span data-ttu-id="a93f8-162">No que diz respeito código .NET, um `ElementRef` é um identificador opaco.</span><span class="sxs-lookup"><span data-stu-id="a93f8-162">As far as .NET code is concerned, an `ElementRef` is an opaque handle.</span></span> <span data-ttu-id="a93f8-163">O *apenas* coisa que você pode fazer com ele é passado para o código JavaScript por meio da interoperabilidade do JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a93f8-163">The *only* thing you can do with it is pass it through to JavaScript code via JavaScript interop.</span></span> <span data-ttu-id="a93f8-164">Quando você fizer isso, o código do JavaScript recebe um `HTMLElement` instância, que pode ser usado com APIs de DOM normal.</span><span class="sxs-lookup"><span data-stu-id="a93f8-164">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="a93f8-165">Por exemplo, o código a seguir define um método de extensão do .NET que permite definir o foco em um elemento:</span><span class="sxs-lookup"><span data-stu-id="a93f8-165">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="a93f8-166">*mylib.js*:</span><span class="sxs-lookup"><span data-stu-id="a93f8-166">*mylib.js*:</span></span>

```javascript
window.myLib = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="a93f8-167">*ElementRefExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="a93f8-167">*ElementRefExtensions.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Blazor;
using Microsoft.JSInterop;
using System.Threading.Tasks;

namespace MyLib
{
    public static class MyLibElementRefExtensions
    {
        public static Task Focus(this ElementRef elementRef)
        {
            return JSRuntime.Current.InvokeAsync<object>("myLib.focusElement", elementRef);
        }
    }
}
```

<span data-ttu-id="a93f8-168">Agora você pode se concentrar entradas em qualquer um de seus componentes:</span><span class="sxs-lookup"><span data-stu-id="a93f8-168">Now you can focus inputs in any of your components:</span></span>

```cshtml
@using MyLib

<input ref="username" />
<button onclick="@SetFocus">Set focus</button>

@functions {
    ElementRef username;

    void SetFocus()
    {
        username.Focus();
    }
}
```

> [!IMPORTANT]
> <span data-ttu-id="a93f8-169">O `username` variável é populada somente depois que o componente processa e sua saída inclui o `<input>` elemento.</span><span class="sxs-lookup"><span data-stu-id="a93f8-169">The `username` variable is only populated after the component renders and its output includes the `<input>` element.</span></span> <span data-ttu-id="a93f8-170">Se você tentar passar um não populada `ElementRef` para o código JavaScript, o código JavaScript recebe `null`.</span><span class="sxs-lookup"><span data-stu-id="a93f8-170">If you try to pass an unpopulated `ElementRef` to JavaScript code, the JavaScript code receives `null`.</span></span> <span data-ttu-id="a93f8-171">Para manipular as referências de elemento após o componente de renderização (para definir o foco inicial em um elemento) use o `OnAfterRenderAsync` ou `OnAfterRender` [métodos de ciclo de vida do componente](xref:razor-components/components#lifecycle-methods).</span><span class="sxs-lookup"><span data-stu-id="a93f8-171">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the `OnAfterRenderAsync` or `OnAfterRender` [component lifecycle methods](xref:razor-components/components#lifecycle-methods).</span></span>

## <a name="invoke-net-methods-from-javascript-functions"></a><span data-ttu-id="a93f8-172">Invocar métodos do .NET de funções do JavaScript</span><span class="sxs-lookup"><span data-stu-id="a93f8-172">Invoke .NET methods from JavaScript functions</span></span>

### <a name="static-net-method-call"></a><span data-ttu-id="a93f8-173">Chamada de método estática do .NET</span><span class="sxs-lookup"><span data-stu-id="a93f8-173">Static .NET method call</span></span>

<span data-ttu-id="a93f8-174">Para invocar um método static .NET do JavaScript, use o `DotNet.invokeMethod` ou `DotNet.invokeMethodAsync` funções.</span><span class="sxs-lookup"><span data-stu-id="a93f8-174">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="a93f8-175">Passe o identificador do método estático que você deseja chamar, o nome do assembly que contém a função e nenhum argumento.</span><span class="sxs-lookup"><span data-stu-id="a93f8-175">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="a93f8-176">Novamente, a versão assíncrona é preferida para dar suporte a cenários do lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="a93f8-176">Again, the async version is preferred to support server-side scenarios.</span></span> <span data-ttu-id="a93f8-177">Para ser invocável do JavaScript, o método do .NET deve ser público, estático e decorado com `[JSInvokable]`.</span><span class="sxs-lookup"><span data-stu-id="a93f8-177">To be invokable from JavaScript, the .NET method must be public, static, and decorated with `[JSInvokable]`.</span></span> <span data-ttu-id="a93f8-178">Por padrão, o identificador do método é o nome do método, mas você pode especificar um identificador diferente usando o `JSInvokableAttribute` construtor.</span><span class="sxs-lookup"><span data-stu-id="a93f8-178">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor.</span></span> <span data-ttu-id="a93f8-179">Chamar métodos genéricos abertos no momento, não é suportado.</span><span class="sxs-lookup"><span data-stu-id="a93f8-179">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="a93f8-180">O aplicativo de exemplo inclui um C# método para retornar uma matriz de `int`s.</span><span class="sxs-lookup"><span data-stu-id="a93f8-180">The sample app includes a C# method to return an array of `int`s.</span></span> <span data-ttu-id="a93f8-181">O método é decorado com o `JSInvokable` atributo.</span><span class="sxs-lookup"><span data-stu-id="a93f8-181">The method is decorated with the `JSInvokable` attribute.</span></span>

<span data-ttu-id="a93f8-182">*Pages/JsInterop.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a93f8-182">*Pages/JsInterop.cshtml*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=47&end=58&highlight=7-11)]

<span data-ttu-id="a93f8-183">JavaScript servido ao cliente invoca o C# método do .NET.</span><span class="sxs-lookup"><span data-stu-id="a93f8-183">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="a93f8-184">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="a93f8-184">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=8-12)]

<span data-ttu-id="a93f8-185">Quando o **método estático de gatilho .NET ReturnArrayAsync** botão é selecionado, examine a saída do console nas ferramentas de desenvolvedor do navegador da web:</span><span class="sxs-lookup"><span data-stu-id="a93f8-185">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="a93f8-186">O quarto valor de matriz é enviada por push para a matriz (`data.push(4);`) retornado por `ReturnArrayAsync`.</span><span class="sxs-lookup"><span data-stu-id="a93f8-186">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

### <a name="instance-method-call"></a><span data-ttu-id="a93f8-187">Chamada de método de instância</span><span class="sxs-lookup"><span data-stu-id="a93f8-187">Instance method call</span></span>

<span data-ttu-id="a93f8-188">Você também pode chamar métodos de instância de .NET do JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a93f8-188">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="a93f8-189">Para invocar um método de instância de .NET do JavaScript, primeiro passar a instância do .NET para JavaScript encapsulando-o em um `DotNetObjectRef` instância.</span><span class="sxs-lookup"><span data-stu-id="a93f8-189">To invoke a .NET instance method from JavaScript, first pass the .NET instance to JavaScript by wrapping it in a `DotNetObjectRef` instance.</span></span> <span data-ttu-id="a93f8-190">A instância do .NET é passada por referência para o JavaScript, e você pode invocar métodos de instância do .NET na instância usando o `invokeMethod` ou `invokeMethodAsync` funções.</span><span class="sxs-lookup"><span data-stu-id="a93f8-190">The .NET instance is passed by reference to JavaScript, and you can invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="a93f8-191">A instância do .NET também pode ser passada como um argumento ao chamar outros métodos .NET do JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a93f8-191">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="a93f8-192">O aplicativo de exemplo registra mensagens para o console do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="a93f8-192">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="a93f8-193">Para os exemplos a seguir demonstrados pelo aplicativo de exemplo, examine a saída do console do navegador nas ferramentas de desenvolvedor do navegador.</span><span class="sxs-lookup"><span data-stu-id="a93f8-193">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="a93f8-194">Quando o **método de instância de gatilho .NET HelloHelper.SayHello** botão for selecionado, `ExampleJsInterop.CallHelloHelperSayHello` é chamado e passa um nome, `Blazor`, para o método.</span><span class="sxs-lookup"><span data-stu-id="a93f8-194">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="a93f8-195">*Pages/JsInterop.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a93f8-195">*Pages/JsInterop.cshtml*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=60&end=69&highlight=8)]

<span data-ttu-id="a93f8-196">`CallHelloHelperSayHello` invoca a função JavaScript `sayHello` com uma nova instância da `HelloHelper`.</span><span class="sxs-lookup"><span data-stu-id="a93f8-196">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="a93f8-197">*JsInteropClasses/ExampleJsInterop.cs*:</span><span class="sxs-lookup"><span data-stu-id="a93f8-197">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=19-25)]

<span data-ttu-id="a93f8-198">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="a93f8-198">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=15-17)]

<span data-ttu-id="a93f8-199">O nome é passado para `HelloHelper`do construtor, que define o `HelloHelper.Name` propriedade.</span><span class="sxs-lookup"><span data-stu-id="a93f8-199">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="a93f8-200">Quando a função JavaScript `sayHello` é executado, `HelloHelper.SayHello` retorna o `Hello, {Name}!` mensagem, que é gravada no console pela função JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a93f8-200">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="a93f8-201">*JsInteropClasses/HelloHelper.cs*:</span><span class="sxs-lookup"><span data-stu-id="a93f8-201">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="a93f8-202">Console de saída nas ferramentas de desenvolvedor do navegador da web:</span><span class="sxs-lookup"><span data-stu-id="a93f8-202">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-razor-component-class-library"></a><span data-ttu-id="a93f8-203">Compartilhar código de interoperabilidade em uma biblioteca de classes de componente do Razor</span><span class="sxs-lookup"><span data-stu-id="a93f8-203">Share interop code in a Razor Component class library</span></span>

<span data-ttu-id="a93f8-204">Código de interoperabilidade de JavaScript pode ser incluído em uma biblioteca de classes de componente do Razor (`dotnet new blazorlib`), que permite que você compartilhar o código em um pacote do NuGet.</span><span class="sxs-lookup"><span data-stu-id="a93f8-204">JavaScript interop code can be included in a Razor Component class library (`dotnet new blazorlib`), which allows you to share the code in a NuGet package.</span></span>

<span data-ttu-id="a93f8-205">A biblioteca de classes Razor componente lida com a incorporação de recursos de JavaScript no assembly compilado.</span><span class="sxs-lookup"><span data-stu-id="a93f8-205">The Razor Component class library handles embedding JavaScript resources in the built assembly.</span></span> <span data-ttu-id="a93f8-206">Os arquivos JavaScript são colocados na *wwwroot* pasta e as ferramentas se encarrega de incorporação de recursos quando a biblioteca é compilada.</span><span class="sxs-lookup"><span data-stu-id="a93f8-206">The JavaScript files are placed in the *wwwroot* folder, and the tooling takes care of embedding the resources when the library is built.</span></span>

<span data-ttu-id="a93f8-207">O pacote NuGet construído é referenciado no arquivo de projeto do aplicativo, assim como qualquer pacote NuGet normal é referenciado.</span><span class="sxs-lookup"><span data-stu-id="a93f8-207">The built NuGet package is referenced in the project file of the app just as any normal NuGet package is referenced.</span></span> <span data-ttu-id="a93f8-208">Depois que o aplicativo tiver sido restaurado, o código do aplicativo pode chamar em JavaScript como se fosse C#.</span><span class="sxs-lookup"><span data-stu-id="a93f8-208">After the app has been restored, app code can call into JavaScript as if it were C#.</span></span>
