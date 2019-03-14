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
# <a name="razor-components-javascript-interop"></a>Interoperabilidade de JavaScript de componentes do Razor

Por [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), e [Luke Latham](https://github.com/guardrex)

Um aplicativo de componentes do Razor pode invocar funções de JavaScript do .NET e .NET métodos do código JavaScript.

## <a name="invoke-javascript-functions-from-net-methods"></a>Invocar funções de JavaScript de métodos do .NET

Há ocasiões em que o código .NET é necessário para chamar uma função de JavaScript. Por exemplo, uma chamada de JavaScript pode expor recursos do navegador ou a funcionalidade de uma biblioteca de JavaScript para o aplicativo.

Para chamar JavaScript do .NET, use o `IJSRuntime` abstração. O `InvokeAsync<T>` método no `IJSRuntime` usa um identificador para a função de JavaScript que você deseja chamar juntamente com qualquer número de argumentos serializável em JSON. O identificador de função é relativo ao escopo global (`window`). Se você deseja chamar `window.someScope.someFunction`, o identificador é `someScope.someFunction`. Não é necessário para registrar a função antes que ele é chamado. O tipo de retorno `T` JSON deve ser serializável.

Para aplicativos do ASP.NET Core Razor componentes do lado do servidor:

* Várias solicitações de usuário são processadas pelo aplicativo do lado do servidor. Não chame `JSRuntime.Current` em um componente para invocar funções de JavaScript.
* Injetar o `IJSRuntime` abstração e use o objeto injetado para emitir chamadas de interoperabilidade de JavaScript.

O exemplo a seguir se baseia [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), um decodificador de baseados em JavaScript experimental. O exemplo demonstra como invocar uma função de JavaScript de um C# método. A função JavaScript aceita uma matriz de bytes de um C# método, decodifica a matriz e retorna o texto para o componente para exibição.

Dentro de `<head>` elemento de *wwwroot/index.html*, fornecer uma função que usa `TextDecoder` para decodificar uma matriz passada:

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

Código de JavaScript, como o código mostrado no exemplo anterior, também pode ser carregado por um arquivo JavaScript com uma referência para o arquivo de script do *wwwroot/index.html* arquivo.

O componente a seguir:

* Invoca o `ConvertArray` usando de função JavaScript `JsRuntime` quando um botão de componente (**converter matriz**) está selecionado.
* Depois que a função JavaScript é chamada, a matriz passada é convertida em uma cadeia de caracteres. A cadeia de caracteres é retornada para o componente para exibição.

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

Para aplicativos do lado do cliente Blazor, o `IJSRuntime` abstração é acessível de `JSRuntime.Current`, que se refere a solicitação do usuário atual. Como há apenas um usuário de um aplicativo de Blazor do lado do cliente, usando `JSRuntime.Current` para invocar um JavaScript função funciona normalmente. Use somente `JSRuntime.Current` em aplicativos do lado do cliente Blazor.

No aplicativo de exemplo de cliente que acompanha este tópico, duas funções de JavaScript estão disponíveis para o aplicativo do lado do cliente que interagem com o DOM para receber entrada do usuário e exibir uma mensagem de boas-vinda:

* `showPrompt` &ndash; Produz um prompt para aceitar a entrada do usuário (o nome do usuário) e retorna o nome para o chamador.
* `displayWelcome` &ndash; Atribui uma mensagem de boas-vinda do chamador para um objeto DOM com um `id` de `welcome`.

*wwwroot/exampleJsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=2-7)]

Coloque o `<script>` marca que faz referência ao arquivo JavaScript na *wwwroot/index.html* arquivo:

[!code-html[](./common/samples/3.x/BlazorSample/wwwroot/index.html?highlight=16)]

Não coloque uma marca de script em um arquivo de componente porque a marca de script não pode ser atualizada dinamicamente.

Interoperabilidade de métodos do .NET com as funções de JavaScript, chamando `InvokeAsync<T>` método no `IJSRuntime`.

O aplicativo de exemplo usa um par de C# métodos, `Prompt` e `Display`, para invocar o `showPrompt` e `displayWelcome` funções JavaScript:

*JsInteropClasses/ExampleJsInterop.cs*:

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=6-8,14-16)]

O `IJSRuntime` abstração é assíncrona para permitir cenários de servidor. Se o aplicativo é executado no cliente, e você deseja invocar uma função JavaScript de forma síncrona, baixá-los para `IJSInProcessRuntime` e chamar `Invoke<T>` em vez disso. É recomendável que melhor uso de interoperabilidade de bibliotecas JavaScript async APIs para garantir que as bibliotecas estão disponíveis em todos os cenários, cliente ou servidor.

O aplicativo de exemplo inclui um componente para demonstrar a interoperabilidade JS. O componente:

* Recebe entrada do usuário por meio de um prompt de JS.
* Retorna o texto para o componente para processamento.
* Chama uma segunda função JS que interage com o DOM para exibir uma mensagem de boas-vinda.

*Pages/JSInterop.cshtml*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=1&end=21&highlight=2-3,9-11,13,16-20)]

1. Quando `TriggerJsPrompt` é executado, selecionando o componente **gatilho JavaScript Prompt** botão, o `ExampleJsInterop.Prompt` método em C# código é chamado.
1. O `Prompt` método executa o JavaScript `showPrompt` função fornecida na *wwwroot/exampleJsInterop.js* arquivo.
1. O `showPrompt` função aceita entrada do usuário (o nome do usuário), que é codificada em HTML e retornado para o `Prompt` método e, por fim, de volta para o componente. O componente armazena o nome do usuário em uma variável local, `name`.
1. A cadeia de caracteres armazenados em `name` incorporado a uma mensagem de boas-vinda, que é passada para um segundo C# método `ExampleJsInterop.Display`.
1. `Display` chama uma função de JavaScript, `displayWelcome`, que processa a mensagem de boas-vinda em uma marca de título.

## <a name="capture-references-to-elements"></a>Referências a elementos de captura

Alguns [interoperabilidade de JavaScript](xref:razor-components/javascript-interop) cenários exigem que as referências a elementos HTML. Por exemplo, uma biblioteca de interface do usuário pode exigir uma referência de elemento para a inicialização, ou talvez você precise chamar APIs do tipo de comando em um elemento, como `focus` ou `play`.

Você pode capturar as referências a elementos HTML em um componente, adicionando um `ref` de atributos para o elemento HTML e, em seguida, definindo um campo do tipo `ElementRef` cujo nome corresponde ao valor do `ref` atributo.

O exemplo a seguir mostra como capturar uma referência para o elemento de entrada de nome de usuário:

```csharp
<input ref="username" ... />

@functions {
    ElementRef username;
}
```

> [!NOTE]
> Fazer **não** usar referências de elemento capturado como uma maneira de preencher o DOM. Isso pode interferir com o modelo de renderização declarativa.

No que diz respeito código .NET, um `ElementRef` é um identificador opaco. O *apenas* coisa que você pode fazer com ele é passado para o código JavaScript por meio da interoperabilidade do JavaScript. Quando você fizer isso, o código do JavaScript recebe um `HTMLElement` instância, que pode ser usado com APIs de DOM normal.

Por exemplo, o código a seguir define um método de extensão do .NET que permite definir o foco em um elemento:

*mylib.js*:

```javascript
window.myLib = {
  focusElement : function (element) {
    element.focus();
  }
}
```

*ElementRefExtensions.cs*:

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

Agora você pode se concentrar entradas em qualquer um de seus componentes:

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
> O `username` variável é populada somente depois que o componente processa e sua saída inclui o `<input>` elemento. Se você tentar passar um não populada `ElementRef` para o código JavaScript, o código JavaScript recebe `null`. Para manipular as referências de elemento após o componente de renderização (para definir o foco inicial em um elemento) use o `OnAfterRenderAsync` ou `OnAfterRender` [métodos de ciclo de vida do componente](xref:razor-components/components#lifecycle-methods).

## <a name="invoke-net-methods-from-javascript-functions"></a>Invocar métodos do .NET de funções do JavaScript

### <a name="static-net-method-call"></a>Chamada de método estática do .NET

Para invocar um método static .NET do JavaScript, use o `DotNet.invokeMethod` ou `DotNet.invokeMethodAsync` funções. Passe o identificador do método estático que você deseja chamar, o nome do assembly que contém a função e nenhum argumento. Novamente, a versão assíncrona é preferida para dar suporte a cenários do lado do servidor. Para ser invocável do JavaScript, o método do .NET deve ser público, estático e decorado com `[JSInvokable]`. Por padrão, o identificador do método é o nome do método, mas você pode especificar um identificador diferente usando o `JSInvokableAttribute` construtor. Chamar métodos genéricos abertos no momento, não é suportado.

O aplicativo de exemplo inclui um C# método para retornar uma matriz de `int`s. O método é decorado com o `JSInvokable` atributo.

*Pages/JsInterop.cshtml*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=47&end=58&highlight=7-11)]

JavaScript servido ao cliente invoca o C# método do .NET.

*wwwroot/exampleJsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=8-12)]

Quando o **método estático de gatilho .NET ReturnArrayAsync** botão é selecionado, examine a saída do console nas ferramentas de desenvolvedor do navegador da web:

```console
Array(4) [ 1, 2, 3, 4 ]
```

O quarto valor de matriz é enviada por push para a matriz (`data.push(4);`) retornado por `ReturnArrayAsync`.

### <a name="instance-method-call"></a>Chamada de método de instância

Você também pode chamar métodos de instância de .NET do JavaScript. Para invocar um método de instância de .NET do JavaScript, primeiro passar a instância do .NET para JavaScript encapsulando-o em um `DotNetObjectRef` instância. A instância do .NET é passada por referência para o JavaScript, e você pode invocar métodos de instância do .NET na instância usando o `invokeMethod` ou `invokeMethodAsync` funções. A instância do .NET também pode ser passada como um argumento ao chamar outros métodos .NET do JavaScript.

> [!NOTE]
> O aplicativo de exemplo registra mensagens para o console do lado do cliente. Para os exemplos a seguir demonstrados pelo aplicativo de exemplo, examine a saída do console do navegador nas ferramentas de desenvolvedor do navegador.

Quando o **método de instância de gatilho .NET HelloHelper.SayHello** botão for selecionado, `ExampleJsInterop.CallHelloHelperSayHello` é chamado e passa um nome, `Blazor`, para o método.

*Pages/JsInterop.cshtml*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=60&end=69&highlight=8)]

`CallHelloHelperSayHello` invoca a função JavaScript `sayHello` com uma nova instância da `HelloHelper`.

*JsInteropClasses/ExampleJsInterop.cs*:

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=19-25)]

*wwwroot/exampleJsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=15-17)]

O nome é passado para `HelloHelper`do construtor, que define o `HelloHelper.Name` propriedade. Quando a função JavaScript `sayHello` é executado, `HelloHelper.SayHello` retorna o `Hello, {Name}!` mensagem, que é gravada no console pela função JavaScript.

*JsInteropClasses/HelloHelper.cs*:

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

Console de saída nas ferramentas de desenvolvedor do navegador da web:

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-razor-component-class-library"></a>Compartilhar código de interoperabilidade em uma biblioteca de classes de componente do Razor

Código de interoperabilidade de JavaScript pode ser incluído em uma biblioteca de classes de componente do Razor (`dotnet new blazorlib`), que permite que você compartilhar o código em um pacote do NuGet.

A biblioteca de classes Razor componente lida com a incorporação de recursos de JavaScript no assembly compilado. Os arquivos JavaScript são colocados na *wwwroot* pasta e as ferramentas se encarrega de incorporação de recursos quando a biblioteca é compilada.

O pacote NuGet construído é referenciado no arquivo de projeto do aplicativo, assim como qualquer pacote NuGet normal é referenciado. Depois que o aplicativo tiver sido restaurado, o código do aplicativo pode chamar em JavaScript como se fosse C#.
