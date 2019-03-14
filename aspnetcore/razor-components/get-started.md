---
title: Introdução ao Razor componentes
author: guardrex
description: Saiba como começar com os componentes do Razor, criando e modificando um projeto de componentes do Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/03/2019
uid: razor-components/get-started
ms.openlocfilehash: a9ada603e5ed4e0e75c4aebc5105c331118666e6
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57036383"
---
# <a name="get-started-with-razor-components"></a>Introdução ao Razor componentes

Por [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Pré-requisitos:

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

Para criar seu primeiro projeto de componentes do Razor no Visual Studio:

1. Selecione **arquivo** > **novo projeto** > **Web** > **aplicativo Web ASP.NET Core**.
1. Certifique-se **.NET Core** e **ASP.NET Core 3.0** são selecionados na parte superior.
1. Escolha o **componentes do Razor** modelo e selecione **Okey**.

   ![Caixa de diálogo Novo aplicativo](https://msdnshared.blob.core.windows.net/media/2019/01/razor-components-template.png)

1. Pressione **F5** para executar o aplicativo.

Parabéns! Você acaba de executar seu primeiro aplicativo de componentes do Razor!

<!--

# [Visual Studio Code](#tab/visual-studio-code)

Prerequisites:

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

To create your first Razor Components project in Visual Studio Code:

1. Execute the following command from a command shell:

   ```console
   dotnet new razorcomponents -o WebApplication1
   ```

1. Open the *WebApplication1* folder in Visual Studio Code.

1. Add a *.vscode* folder.

1. Add a *tasks.json* file to the *.vscode* folder with the following content:

   [!code-json[](get-started/samples_snapshot/3.x/tasks.json)]

1. Add a *launch.json* file to the *.vscode* folder with the following content:

   [!code-json[](get-started/samples_snapshot/3.x/launch.json)]

1. Execute the app using the Visual Studio Code debugger.

1. In a browser, navigate to `https://localhost:5001`.

Congratulations! You just ran your first Razor Components app!

# [Visual Studio for Mac](#tab/visual-studio-mac)

.NET Core 3.0 will be supported with Visual Studio for Mac version 8.0 or later. Visual Studio for Mac version 8.0 Preview isn't available at this time.

Use the [.NET Core CLI version of this topic](xref:razor-components/get-started?tabs=netcore-cli) on macOS.


[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

To create your first project Razor Components project in Visual Studio for Mac:

1. Select **File** > **New Solution** or **New Project**.
1. In the sidebar, select **.NET Core** > **App**.
1. Select **ASP.NET Core Razor Components** and select **Next**.
1. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.
1. In the **Project Name** field, enter `WebApplication1`. Select **Create**.
1. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

Congratulations! You just ran your first Razor Components app!
-->

# <a name="net-core-clitabnetcore-cli"></a>[CLI do .NET Core](#tab/netcore-cli/)

Pré-requisitos:

* [.NET core SDK 3.0 versão prévia](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. Para criar seu primeiro projeto de componentes do Razor de um shell de comando:

   ```console
   dotnet new razorcomponents -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. Em um navegador, navegue até `https://localhost:5001`.

Parabéns! Você acaba de executar seu primeiro aplicativo de componentes do Razor!

---

## <a name="razor-components-project"></a>Projeto de componentes do Razor

A solução criada pelo modelo de componentes do Razor contém dois projetos:

* *WebApplication1.Server* &ndash; o projeto do servidor é um projeto ASP.NET Core configurado para hospedar o aplicativo de componentes do Razor.
* *WebApplication1.App* &ndash; o projeto de interface do usuário da web do lado do cliente que usa componentes do Razor.

A lógica da interface do usuário a *WebApplication1.App* projeto é separado do restante do aplicativo devido a uma limitação técnica no ASP.NET Core 3.0 Preview 2. A extensão de arquivo do Razor (*. cshtml*) usado para componentes de Razor também é usado para exibições de páginas do Razor e MVC. Atualmente, o Razor componentes e as páginas do Razor/MVC tem modelos de compilação diferente, para que os arquivos do Razor de componentes do Razor são mantidos separados. Em uma visualização futura, pretendemos introduzir uma nova extensão de arquivo para componentes do Razor (*.razor*). Componentes, páginas e modos de exibição serão hospedados *no mesmo projeto*.

Quando o aplicativo é executado, várias páginas estão disponíveis nas guias na barra lateral:

* Home
* Contador
* Buscar dados

Na página Contador, selecione o botão **Clique aqui** para incrementar o contador sem uma atualização de página. A incrementação de um contador em uma página da Web normalmente exige JavaScript, mas o Razor Components fornece uma abordagem melhor usando C#.

*WebApplication1.App/Pages/Counter.cshtml*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.cshtml)]

Uma solicitação para `/counter` no navegador, conforme especificado pelo `@page` diretiva na parte superior, faz com que o componente de contador processar seu conteúdo. Componentes processam em uma representação na memória da árvore de renderização que pode ser usado para atualizar a interface do usuário de uma maneira flexível e eficiente.

Cada vez que o **Click me** botão é selecionado:

* O `onclick` evento é disparado.
* O método `IncrementCount` é chamado.
* O `currentCount` é incrementado.
* O componente é renderizado novamente.

O tempo de execução compara o novo conteúdo para o conteúdo anterior e só se aplica o conteúdo alterado para modelo de objeto de documento (DOM).

Adicione um componente para outro componente usando uma sintaxe semelhante ao HTML. Componente são especificados usando atributos ou conteúdo filho. Por exemplo, um componente de contador pode ser adicionado a homepage do aplicativo adicionando um `<Counter />` elemento para o componente do índice.

*WebApplication1.App/Pages/Index.cshtml*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.cshtml?highlight=7)]

Execute o aplicativo. A home page tem seu próprio contador.

Para adicionar um parâmetro para o componente de contador, atualize o componente `@functions` bloco:

* Adicionar uma propriedade para `IncrementAmount` decorada com o `[Parameter]` atributo.
* Altere o método `IncrementCount` para usar o `IncrementAmount` ao aumentar o valor de `currentCount`.

*WebApplication1.App/Pages/Counter.cshtml*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.cshtml?highlight=4,8)]

Especifique um parâmetro `IncrementAmount` no elemento `<Counter>` do componente Home usando um atributo.

*WebApplication1.App/Pages/Index.cshtml*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.cshtml)]

Execute o aplicativo. A home page tem seu próprio contador que incrementa dez cada vez que o **Click me** botão é selecionado.

## <a name="next-steps"></a>Próximas etapas

<xref:tutorials/first-razor-components-app>
