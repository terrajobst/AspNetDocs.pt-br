---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: Adicionando um controlador | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 194a8a7398e163f0c37164a8724f98b16444984b
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457213"
---
# <a name="adding-a-controller"></a><span data-ttu-id="c2fe9-102">Adicionando um controlador</span><span class="sxs-lookup"><span data-stu-id="c2fe9-102">Adding a Controller</span></span>

<span data-ttu-id="c2fe9-103">por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c2fe9-103">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [Tutorial Note](index.md)]

<span data-ttu-id="c2fe9-104">O MVC significa *Model-View-Controller*.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-104">MVC stands for *model-view-controller*.</span></span> <span data-ttu-id="c2fe9-105">O MVC é um padrão para o desenvolvimento de aplicativos que são bem arquitetados, testados e fáceis de manter.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-105">MVC is a pattern for developing applications that are well architected, testable and easy to maintain.</span></span> <span data-ttu-id="c2fe9-106">Os aplicativos baseados em MVC contêm:</span><span class="sxs-lookup"><span data-stu-id="c2fe9-106">MVC-based applications contain:</span></span>

- <span data-ttu-id="c2fe9-107">**M** Odels: classes que representam os dados do aplicativo e que usam a lógica de validação para impor regras de negócio para esses dados.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-107">**M** odels: Classes that represent the data of the application and that use validation logic to enforce business rules for that data.</span></span>
- <span data-ttu-id="c2fe9-108">**V** iews: Arquivos de modelo que seu aplicativo usa para gerar DINAMICAMENTE respostas HTML.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-108">**V** iews: Template files that your application uses to dynamically generate HTML responses.</span></span>
- <span data-ttu-id="c2fe9-109">**C** Ontrollers: classes que manipulam solicitações de navegador recebidas, recuperam dados de modelo e, em seguida, especificam modelos de exibição que retornam uma resposta ao navegador.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-109">**C** ontrollers: Classes that handle incoming browser requests, retrieve model data, and then specify view templates that return a response to the browser.</span></span>

<span data-ttu-id="c2fe9-110">Abordaremos todos esses conceitos nesta série de tutoriais e mostraremos como usá-los para criar um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-110">We'll be covering all these concepts in this tutorial series and show you how to use them to build an application.</span></span>

<span data-ttu-id="c2fe9-111">Vamos começar criando uma classe de controlador.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-111">Let's begin by creating a controller class.</span></span> <span data-ttu-id="c2fe9-112">Em **Gerenciador de soluções**, clique com o botão direito do mouse na pasta *controladores* e clique em **Adicionar**e em **controlador**.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-112">In **Solution Explorer**, right-click the *Controllers* folder and then click **Add**, then **Controller**.</span></span>

![](adding-a-controller/_static/image1.png)

<span data-ttu-id="c2fe9-113">Na caixa de diálogo **Adicionar Scaffold** , clique em **controlador MVC 5 – vazio**e, em seguida, clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-113">In the **Add Scaffold** dialog box, click **MVC 5 Controller - Empty**, and then click **Add**.</span></span>

![](adding-a-controller/_static/image2.png)  

<span data-ttu-id="c2fe9-114">Nomeie o novo controlador "HelloWorldController" e clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-114">Name your new controller "HelloWorldController" and click **Add**.</span></span>

![Adicionar controlador](adding-a-controller/_static/image3.png)

<span data-ttu-id="c2fe9-116">Observe no **Gerenciador de soluções** que um novo arquivo foi criado chamado *HelloWorldController.cs* e uma nova pasta *Views\HelloWorld*.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-116">Notice in **Solution Explorer** that a new file has been created named *HelloWorldController.cs* and a new folder *Views\HelloWorld*.</span></span> <span data-ttu-id="c2fe9-117">O controlador está aberto no IDE.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-117">The controller is open in the IDE.</span></span>

![](adding-a-controller/_static/image4.png)

<span data-ttu-id="c2fe9-118">Substitua o conteúdo do arquivo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-118">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

<span data-ttu-id="c2fe9-119">Os métodos do controlador retornarão uma cadeia de caracteres de HTML como um exemplo.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-119">The controller methods will return a string of HTML as an example.</span></span> <span data-ttu-id="c2fe9-120">O controlador é nomeado `HelloWorldController` e o primeiro método é nomeado `Index`.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-120">The controller is named `HelloWorldController` and the first method is named `Index`.</span></span> <span data-ttu-id="c2fe9-121">Vamos chamá-lo de um navegador.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-121">Let's invoke it from a browser.</span></span> <span data-ttu-id="c2fe9-122">Execute o aplicativo (pressione F5 ou CTRL + F5).</span><span class="sxs-lookup"><span data-stu-id="c2fe9-122">Run the application (press F5 or Ctrl+F5).</span></span> <span data-ttu-id="c2fe9-123">No navegador, acrescente &quot;HelloWorld&quot; ao caminho na barra de endereços.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-123">In the browser, append &quot;HelloWorld&quot; to the path in the address bar.</span></span> <span data-ttu-id="c2fe9-124">(Por exemplo, na ilustração abaixo, é `http://localhost:1234/HelloWorld.`) A página no navegador se parecerá com a captura de tela a seguir.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-124">(For example, in the illustration below, it's `http://localhost:1234/HelloWorld.`) The page in the browser will look like the following screenshot.</span></span> <span data-ttu-id="c2fe9-125">No método acima, o código retornou uma cadeia de caracteres diretamente.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-125">In the method above, the code returned a string directly.</span></span> <span data-ttu-id="c2fe9-126">Você disse ao sistema para retornar apenas um HTML e ele fazia!</span><span class="sxs-lookup"><span data-stu-id="c2fe9-126">You told the system to just return some HTML, and it did!</span></span>

![](adding-a-controller/_static/image5.png)

<span data-ttu-id="c2fe9-127">O ASP.NET MVC invoca classes de controlador diferentes (e métodos de ação diferentes dentro delas) dependendo da URL de entrada.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-127">ASP.NET MVC invokes different controller classes (and different action methods within them) depending on the incoming URL.</span></span> <span data-ttu-id="c2fe9-128">A lógica de roteamento de URL padrão usada pelo ASP.NET MVC usa um formato como este para determinar qual código invocar:</span><span class="sxs-lookup"><span data-stu-id="c2fe9-128">The default URL routing logic used by ASP.NET MVC uses a format like this to determine what code to invoke:</span></span>

`/[Controller]/[ActionName]/[Parameters]`

<span data-ttu-id="c2fe9-129">Você define o formato para roteamento no *aplicativo\_arquivo start/RouteConfig. cs* .</span><span class="sxs-lookup"><span data-stu-id="c2fe9-129">You set the format for routing in the *App\_Start/RouteConfig.cs* file.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

<span data-ttu-id="c2fe9-130">Quando você executa o aplicativo e não fornece nenhum segmento de URL, ele usa como padrão o controlador "Home" e o método de ação "index" especificado na seção padrões do código acima.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-130">When you run the application and don't supply any URL segments, it defaults to the "Home" controller and the "Index" action method specified in the defaults section of the code above.</span></span>

<span data-ttu-id="c2fe9-131">A primeira parte da URL determina a classe do controlador a ser executada.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-131">The first part of the URL determines the controller class to execute.</span></span> <span data-ttu-id="c2fe9-132">Portanto, */HelloWorld* é mapeado para a classe `HelloWorldController`.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-132">So */HelloWorld* maps to the `HelloWorldController` class.</span></span> <span data-ttu-id="c2fe9-133">A segunda parte da URL determina o método de ação na classe a ser executada.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-133">The second part of the URL determines the action method on the class to execute.</span></span> <span data-ttu-id="c2fe9-134">Portanto, */HelloWorld/index* faria com que o método de `Index` da classe `HelloWorldController` fosse executado.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-134">So */HelloWorld/Index* would cause the `Index` method of the `HelloWorldController` class to execute.</span></span> <span data-ttu-id="c2fe9-135">Observe que precisamos apenas navegar até */HelloWorld* e o método `Index` foi usado por padrão.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-135">Notice that we only had to browse to */HelloWorld* and the `Index` method was used by default.</span></span> <span data-ttu-id="c2fe9-136">Isso ocorre porque um método chamado `Index` é o método padrão que será chamado em um controlador se um não for especificado explicitamente.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-136">This is because a method named `Index` is the default method that will be called on a controller if one is not explicitly specified.</span></span> <span data-ttu-id="c2fe9-137">A terceira parte do segmento de URL (`Parameters`) refere-se aos dados de rota.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-137">The third part of the URL segment ( `Parameters`) is for route data.</span></span> <span data-ttu-id="c2fe9-138">Veremos os dados de rota mais adiante neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-138">We'll see route data later on in this tutorial.</span></span>

<span data-ttu-id="c2fe9-139">Navegue até `http://localhost:xxxx/HelloWorld/Welcome`.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-139">Browse to `http://localhost:xxxx/HelloWorld/Welcome`.</span></span> <span data-ttu-id="c2fe9-140">O método `Welcome` é executado e retorna a cadeia de caracteres &quot;este é o método de ação de boas-vindas...&quot;.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-140">The `Welcome` method runs and returns the string &quot;This is the Welcome action method...&quot;.</span></span> <span data-ttu-id="c2fe9-141">O mapeamento do MVC padrão é `/[Controller]/[ActionName]/[Parameters]`.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-141">The default MVC mapping is `/[Controller]/[ActionName]/[Parameters]`.</span></span> <span data-ttu-id="c2fe9-142">Para essa URL, o controlador é `HelloWorld` e `Welcome` é o método de ação.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-142">For this URL, the controller is `HelloWorld` and `Welcome` is the action method.</span></span> <span data-ttu-id="c2fe9-143">Você ainda não usou a parte `[Parameters]` da URL.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-143">You haven't used the `[Parameters]` part of the URL yet.</span></span>

![](adding-a-controller/_static/image6.png)

<span data-ttu-id="c2fe9-144">Vamos modificar o exemplo ligeiramente para que você possa passar algumas informações de parâmetro da URL para o controlador (por exemplo, */HelloWorld/Welcome? Name = Scott&amp;numtimes = 4*).</span><span class="sxs-lookup"><span data-stu-id="c2fe9-144">Let's modify the example slightly so that you can pass some parameter information from the URL to the controller (for example, */HelloWorld/Welcome?name=Scott&amp;numtimes=4*).</span></span> <span data-ttu-id="c2fe9-145">Altere o método `Welcome` para incluir dois parâmetros, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-145">Change your `Welcome` method to include two parameters as shown below.</span></span> <span data-ttu-id="c2fe9-146">Observe que o código usa o C# recurso opcional-Parameter para indicar que o parâmetro `numTimes` deve usar como padrão 1 se nenhum valor for passado para esse parâmetro.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-146">Note that the code uses the C# optional-parameter feature to indicate that the `numTimes` parameter should default to 1 if no value is passed for that parameter.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> <span data-ttu-id="c2fe9-147">Observação de segurança: o código acima usa [HttpUtility. HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) para proteger o aplicativo contra entradas mal-intencionadas (ou seja, JavaScript).</span><span class="sxs-lookup"><span data-stu-id="c2fe9-147">Security Note: The code above uses [HttpUtility.HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) to protect the application from malicious input (namely JavaScript).</span></span> <span data-ttu-id="c2fe9-148">Para obter mais informações [, consulte Como: proteger contra explorações de script em um aplicativo Web aplicando codificação HTML a cadeias de caracteres](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="c2fe9-148">For more information see [How to: Protect Against Script Exploits in a Web Application by Applying HTML Encoding to Strings](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).</span></span>

 <span data-ttu-id="c2fe9-149">Execute o aplicativo e navegue até a URL de exemplo (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`).</span><span class="sxs-lookup"><span data-stu-id="c2fe9-149">Run your application and browse to the example URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`).</span></span> <span data-ttu-id="c2fe9-150">Você pode tentar valores diferentes para `name` e `numtimes` na URL.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-150">You can try different values for `name` and `numtimes` in the URL.</span></span> <span data-ttu-id="c2fe9-151">O [sistema de associação de modelo MVC do ASP.net](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) mapeia automaticamente os parâmetros nomeados da cadeia de caracteres de consulta na barra de endereços para parâmetros em seu método.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-151">The [ASP.NET MVC model binding system](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatically maps the named parameters from the query string in the address bar to parameters in your method.</span></span>

![](adding-a-controller/_static/image7.png)

<span data-ttu-id="c2fe9-152">No exemplo acima, o segmento de URL (`Parameters`) não é usado, os parâmetros `name` e `numTimes` são passados como [cadeias de caracteres de consulta](http://en.wikipedia.org/wiki/Query_string).</span><span class="sxs-lookup"><span data-stu-id="c2fe9-152">In the sample above, the URL segment ( `Parameters`) is not used, the `name` and `numTimes` parameters are passed as [query strings](http://en.wikipedia.org/wiki/Query_string).</span></span> <span data-ttu-id="c2fe9-153">O ?</span><span class="sxs-lookup"><span data-stu-id="c2fe9-153">The ?</span></span> <span data-ttu-id="c2fe9-154">(ponto de interrogação) na URL acima é um separador e as cadeias de consulta seguem.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-154">(question mark) in the above URL is a separator, and the query strings follow.</span></span> <span data-ttu-id="c2fe9-155">O caractere &amp; separa as cadeias de consulta.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-155">The &amp; character separates query strings.</span></span>

<span data-ttu-id="c2fe9-156">Substitua o método Welcome pelo seguinte código:</span><span class="sxs-lookup"><span data-stu-id="c2fe9-156">Replace the Welcome method with the following code:</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

<span data-ttu-id="c2fe9-157">Execute o aplicativo e insira a seguinte URL: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`</span><span class="sxs-lookup"><span data-stu-id="c2fe9-157">Run the application and enter the following URL: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`</span></span>

![](adding-a-controller/_static/image8.png)

<span data-ttu-id="c2fe9-158">Desta vez, o terceiro segmento de URL correspondeu ao parâmetro de rota `ID.` o `Welcome` método de ação contém um parâmetro (`ID`) que correspondeu à especificação de URL no método `RegisterRoutes`.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-158">This time the third URL segment matched the route parameter `ID.` The `Welcome` action method contains a parameter (`ID`) that matched the URL specification in the `RegisterRoutes` method.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

<span data-ttu-id="c2fe9-159">Em aplicativos MVC ASP.NET, é mais comum passar parâmetros como dados de rota (como fizemos com a ID acima) do que passá-los como cadeias de consulta.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-159">In ASP.NET MVC applications, it's more typical to pass in parameters as route data (like we did with ID above) than passing them as query strings.</span></span> <span data-ttu-id="c2fe9-160">Você também pode adicionar uma rota para passar o `name` e `numtimes` em parâmetros como dados de rota na URL.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-160">You could also add a route to pass both the `name` and `numtimes` in parameters as route data in the URL.</span></span> <span data-ttu-id="c2fe9-161">No arquivo de *\_do aplicativo Start\RouteConfig.cs* , adicione a rota "Olá":</span><span class="sxs-lookup"><span data-stu-id="c2fe9-161">In the *App\_Start\RouteConfig.cs* file, add the "Hello" route:</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

<span data-ttu-id="c2fe9-162">Execute o aplicativo e navegue até `/localhost:XXX/HelloWorld/Welcome/Scott/3`.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-162">Run the application and browse to `/localhost:XXX/HelloWorld/Welcome/Scott/3`.</span></span>

![](adding-a-controller/_static/image9.png)

<span data-ttu-id="c2fe9-163">Para muitos aplicativos MVC, a rota padrão funciona bem.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-163">For many MVC applications, the default route works fine.</span></span> <span data-ttu-id="c2fe9-164">Você aprenderá mais adiante neste tutorial para passar dados usando o associador de modelo e não precisará modificar a rota padrão para isso.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-164">You'll learn later in this tutorial to pass data using the model binder, and you won't have to modify the default route for that.</span></span>

<span data-ttu-id="c2fe9-165">Nestes exemplos, o controlador está fazendo a &quot;VC&quot; parte do MVC, ou seja, o modo de exibição e o controlador funcionam.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-165">In these examples the controller has been doing the &quot;VC&quot; portion of MVC — that is, the view and controller work.</span></span> <span data-ttu-id="c2fe9-166">O controlador retorna o HTML diretamente.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-166">The controller is returning HTML directly.</span></span> <span data-ttu-id="c2fe9-167">Normalmente, você não quer que os controladores retornem HTML diretamente, já que isso se torna muito complicado para o código.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-167">Ordinarily you don't want controllers returning HTML directly, since that becomes very cumbersome to code.</span></span> <span data-ttu-id="c2fe9-168">Em vez disso, normalmente usaremos um arquivo de modelo de exibição separado para ajudar a gerar a resposta HTML.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-168">Instead we'll typically use a separate view template file to help generate the HTML response.</span></span> <span data-ttu-id="c2fe9-169">Vejamos em seguida como podemos fazer isso.</span><span class="sxs-lookup"><span data-stu-id="c2fe9-169">Let's look next at how we can do this.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c2fe9-170">[Anterior](getting-started.md)
> [Próximo](adding-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="c2fe9-170">[Previous](getting-started.md)
[Next](adding-a-view.md)</span></span>
