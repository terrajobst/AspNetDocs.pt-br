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
# <a name="get-started-with-razor-components"></a><span data-ttu-id="bb32f-103">Introdução ao Razor componentes</span><span class="sxs-lookup"><span data-stu-id="bb32f-103">Get started with Razor Components</span></span>

<span data-ttu-id="bb32f-104">Por [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="bb32f-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bb32f-105">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bb32f-105">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="bb32f-106">Pré-requisitos:</span><span class="sxs-lookup"><span data-stu-id="bb32f-106">Prerequisites:</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

<span data-ttu-id="bb32f-107">Para criar seu primeiro projeto de componentes do Razor no Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="bb32f-107">To create your first Razor Components project in Visual Studio:</span></span>

1. <span data-ttu-id="bb32f-108">Selecione **arquivo** > **novo projeto** > **Web** > **aplicativo Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="bb32f-108">Select **File** > **New Project** > **Web** > **ASP.NET Core Web Application**.</span></span>
1. <span data-ttu-id="bb32f-109">Certifique-se **.NET Core** e **ASP.NET Core 3.0** são selecionados na parte superior.</span><span class="sxs-lookup"><span data-stu-id="bb32f-109">Make sure **.NET Core** and **ASP.NET Core 3.0** are selected at the top.</span></span>
1. <span data-ttu-id="bb32f-110">Escolha o **componentes do Razor** modelo e selecione **Okey**.</span><span class="sxs-lookup"><span data-stu-id="bb32f-110">Choose the **Razor Components** template and select **OK**.</span></span>

   ![Caixa de diálogo Novo aplicativo](https://msdnshared.blob.core.windows.net/media/2019/01/razor-components-template.png)

1. <span data-ttu-id="bb32f-112">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="bb32f-112">Press **F5** to run the app.</span></span>

<span data-ttu-id="bb32f-113">Parabéns!</span><span class="sxs-lookup"><span data-stu-id="bb32f-113">Congratulations!</span></span> <span data-ttu-id="bb32f-114">Você acaba de executar seu primeiro aplicativo de componentes do Razor!</span><span class="sxs-lookup"><span data-stu-id="bb32f-114">You just ran your first Razor Components app!</span></span>

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

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="bb32f-115">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="bb32f-115">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="bb32f-116">Pré-requisitos:</span><span class="sxs-lookup"><span data-stu-id="bb32f-116">Prerequisites:</span></span>

* [<span data-ttu-id="bb32f-117">.NET core SDK 3.0 versão prévia</span><span class="sxs-lookup"><span data-stu-id="bb32f-117">.NET Core SDK 3.0 Preview</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. <span data-ttu-id="bb32f-118">Para criar seu primeiro projeto de componentes do Razor de um shell de comando:</span><span class="sxs-lookup"><span data-stu-id="bb32f-118">To create your first Razor Components project from a command shell:</span></span>

   ```console
   dotnet new razorcomponents -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. <span data-ttu-id="bb32f-119">Em um navegador, navegue até `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="bb32f-119">In a browser, navigate to `https://localhost:5001`.</span></span>

<span data-ttu-id="bb32f-120">Parabéns!</span><span class="sxs-lookup"><span data-stu-id="bb32f-120">Congratulations!</span></span> <span data-ttu-id="bb32f-121">Você acaba de executar seu primeiro aplicativo de componentes do Razor!</span><span class="sxs-lookup"><span data-stu-id="bb32f-121">You just ran your first Razor Components app!</span></span>

---

## <a name="razor-components-project"></a><span data-ttu-id="bb32f-122">Projeto de componentes do Razor</span><span class="sxs-lookup"><span data-stu-id="bb32f-122">Razor Components project</span></span>

<span data-ttu-id="bb32f-123">A solução criada pelo modelo de componentes do Razor contém dois projetos:</span><span class="sxs-lookup"><span data-stu-id="bb32f-123">The solution created by the Razor Components template contains two projects:</span></span>

* <span data-ttu-id="bb32f-124">*WebApplication1.Server* &ndash; o projeto do servidor é um projeto ASP.NET Core configurado para hospedar o aplicativo de componentes do Razor.</span><span class="sxs-lookup"><span data-stu-id="bb32f-124">*WebApplication1.Server* &ndash; The server project is an ASP.NET Core project set up to host the Razor Components app.</span></span>
* <span data-ttu-id="bb32f-125">*WebApplication1.App* &ndash; o projeto de interface do usuário da web do lado do cliente que usa componentes do Razor.</span><span class="sxs-lookup"><span data-stu-id="bb32f-125">*WebApplication1.App* &ndash; The client-side web UI project that uses Razor Components.</span></span>

<span data-ttu-id="bb32f-126">A lógica da interface do usuário a *WebApplication1.App* projeto é separado do restante do aplicativo devido a uma limitação técnica no ASP.NET Core 3.0 Preview 2.</span><span class="sxs-lookup"><span data-stu-id="bb32f-126">The UI logic in the *WebApplication1.App* project is separated from the rest of the app due to a technical limitation in ASP.NET Core 3.0 Preview 2.</span></span> <span data-ttu-id="bb32f-127">A extensão de arquivo do Razor (*. cshtml*) usado para componentes de Razor também é usado para exibições de páginas do Razor e MVC.</span><span class="sxs-lookup"><span data-stu-id="bb32f-127">The Razor file extension (*.cshtml*) used for Razor Components is also used for Razor Pages and MVC views.</span></span> <span data-ttu-id="bb32f-128">Atualmente, o Razor componentes e as páginas do Razor/MVC tem modelos de compilação diferente, para que os arquivos do Razor de componentes do Razor são mantidos separados.</span><span class="sxs-lookup"><span data-stu-id="bb32f-128">Currently, Razor Components and Razor Pages/MVC have different compilation models, so the Razor Components Razor files are kept separate.</span></span> <span data-ttu-id="bb32f-129">Em uma visualização futura, pretendemos introduzir uma nova extensão de arquivo para componentes do Razor (*.razor*).</span><span class="sxs-lookup"><span data-stu-id="bb32f-129">In a future preview, we plan to introduce a new file extension for Razor Components (*.razor*).</span></span> <span data-ttu-id="bb32f-130">Componentes, páginas e modos de exibição serão hospedados *no mesmo projeto*.</span><span class="sxs-lookup"><span data-stu-id="bb32f-130">Components, pages, and views will be hosted *in the same project*.</span></span>

<span data-ttu-id="bb32f-131">Quando o aplicativo é executado, várias páginas estão disponíveis nas guias na barra lateral:</span><span class="sxs-lookup"><span data-stu-id="bb32f-131">When the app is run, multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="bb32f-132">Home</span><span class="sxs-lookup"><span data-stu-id="bb32f-132">Home</span></span>
* <span data-ttu-id="bb32f-133">Contador</span><span class="sxs-lookup"><span data-stu-id="bb32f-133">Counter</span></span>
* <span data-ttu-id="bb32f-134">Buscar dados</span><span class="sxs-lookup"><span data-stu-id="bb32f-134">Fetch data</span></span>

<span data-ttu-id="bb32f-135">Na página Contador, selecione o botão **Clique aqui** para incrementar o contador sem uma atualização de página.</span><span class="sxs-lookup"><span data-stu-id="bb32f-135">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="bb32f-136">A incrementação de um contador em uma página da Web normalmente exige JavaScript, mas o Razor Components fornece uma abordagem melhor usando C#.</span><span class="sxs-lookup"><span data-stu-id="bb32f-136">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor Components provides a better approach using C#.</span></span>

<span data-ttu-id="bb32f-137">*WebApplication1.App/Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="bb32f-137">*WebApplication1.App/Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.cshtml)]

<span data-ttu-id="bb32f-138">Uma solicitação para `/counter` no navegador, conforme especificado pelo `@page` diretiva na parte superior, faz com que o componente de contador processar seu conteúdo.</span><span class="sxs-lookup"><span data-stu-id="bb32f-138">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="bb32f-139">Componentes processam em uma representação na memória da árvore de renderização que pode ser usado para atualizar a interface do usuário de uma maneira flexível e eficiente.</span><span class="sxs-lookup"><span data-stu-id="bb32f-139">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="bb32f-140">Cada vez que o **Click me** botão é selecionado:</span><span class="sxs-lookup"><span data-stu-id="bb32f-140">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="bb32f-141">O `onclick` evento é disparado.</span><span class="sxs-lookup"><span data-stu-id="bb32f-141">The `onclick` event is fired.</span></span>
* <span data-ttu-id="bb32f-142">O método `IncrementCount` é chamado.</span><span class="sxs-lookup"><span data-stu-id="bb32f-142">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="bb32f-143">O `currentCount` é incrementado.</span><span class="sxs-lookup"><span data-stu-id="bb32f-143">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="bb32f-144">O componente é renderizado novamente.</span><span class="sxs-lookup"><span data-stu-id="bb32f-144">The component is rendered again.</span></span>

<span data-ttu-id="bb32f-145">O tempo de execução compara o novo conteúdo para o conteúdo anterior e só se aplica o conteúdo alterado para modelo de objeto de documento (DOM).</span><span class="sxs-lookup"><span data-stu-id="bb32f-145">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="bb32f-146">Adicione um componente para outro componente usando uma sintaxe semelhante ao HTML.</span><span class="sxs-lookup"><span data-stu-id="bb32f-146">Add a component to another component using an HTML-like syntax.</span></span> <span data-ttu-id="bb32f-147">Componente são especificados usando atributos ou conteúdo filho.</span><span class="sxs-lookup"><span data-stu-id="bb32f-147">Component parameters are specified using attributes or child content.</span></span> <span data-ttu-id="bb32f-148">Por exemplo, um componente de contador pode ser adicionado a homepage do aplicativo adicionando um `<Counter />` elemento para o componente do índice.</span><span class="sxs-lookup"><span data-stu-id="bb32f-148">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="bb32f-149">*WebApplication1.App/Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="bb32f-149">*WebApplication1.App/Pages/Index.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.cshtml?highlight=7)]

<span data-ttu-id="bb32f-150">Execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="bb32f-150">Run the app.</span></span> <span data-ttu-id="bb32f-151">A home page tem seu próprio contador.</span><span class="sxs-lookup"><span data-stu-id="bb32f-151">The homepage has its own counter.</span></span>

<span data-ttu-id="bb32f-152">Para adicionar um parâmetro para o componente de contador, atualize o componente `@functions` bloco:</span><span class="sxs-lookup"><span data-stu-id="bb32f-152">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="bb32f-153">Adicionar uma propriedade para `IncrementAmount` decorada com o `[Parameter]` atributo.</span><span class="sxs-lookup"><span data-stu-id="bb32f-153">Add a property for `IncrementAmount` decorated with the `[Parameter]` attribute.</span></span>
* <span data-ttu-id="bb32f-154">Altere o método `IncrementCount` para usar o `IncrementAmount` ao aumentar o valor de `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="bb32f-154">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="bb32f-155">*WebApplication1.App/Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="bb32f-155">*WebApplication1.App/Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.cshtml?highlight=4,8)]

<span data-ttu-id="bb32f-156">Especifique um parâmetro `IncrementAmount` no elemento `<Counter>` do componente Home usando um atributo.</span><span class="sxs-lookup"><span data-stu-id="bb32f-156">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="bb32f-157">*WebApplication1.App/Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="bb32f-157">*WebApplication1.App/Pages/Index.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.cshtml)]

<span data-ttu-id="bb32f-158">Execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="bb32f-158">Run the app.</span></span> <span data-ttu-id="bb32f-159">A home page tem seu próprio contador que incrementa dez cada vez que o **Click me** botão é selecionado.</span><span class="sxs-lookup"><span data-stu-id="bb32f-159">The homepage has its own counter that increments by ten each time the **Click me** button is selected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bb32f-160">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="bb32f-160">Next steps</span></span>

<xref:tutorials/first-razor-components-app>
