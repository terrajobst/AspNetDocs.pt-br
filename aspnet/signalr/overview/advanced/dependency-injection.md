---
uid: signalr/overview/advanced/dependency-injection
title: Injeção de dependência no Signalr | Microsoft Docs
author: bradygaster
description: Versões de software usadas neste tópico Visual Studio 2013 o .NET 4,5 Signalr versão 2 versões anteriores deste tópico para obter informações sobre versões anteriores do...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: a14121ae-02cf-4024-8af0-9dd0dc810690
msc.legacyurl: /signalr/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 52978b10b6c131ac8eff4535216cc60b43fdf3de
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78537327"
---
# <a name="dependency-injection-in-signalr"></a><span data-ttu-id="43ddb-103">Injeção de dependência no SignalR</span><span class="sxs-lookup"><span data-stu-id="43ddb-103">Dependency Injection in SignalR</span></span>

<span data-ttu-id="43ddb-104">por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="43ddb-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="43ddb-105">Versões de software usadas neste tópico</span><span class="sxs-lookup"><span data-stu-id="43ddb-105">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="43ddb-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="43ddb-106">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="43ddb-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="43ddb-107">.NET 4.5</span></span>
> - <span data-ttu-id="43ddb-108">Sinalização versão 2</span><span class="sxs-lookup"><span data-stu-id="43ddb-108">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="43ddb-109">Versões anteriores deste tópico</span><span class="sxs-lookup"><span data-stu-id="43ddb-109">Previous versions of this topic</span></span>
>
> <span data-ttu-id="43ddb-110">Para obter informações sobre versões anteriores do Signalr, confira [versões mais antigas do signalr](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="43ddb-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="43ddb-111">Perguntas e comentários</span><span class="sxs-lookup"><span data-stu-id="43ddb-111">Questions and comments</span></span>
>
> <span data-ttu-id="43ddb-112">Deixe comentários sobre como você gostou deste tutorial e o que poderíamos melhorar nos comentários na parte inferior da página.</span><span class="sxs-lookup"><span data-stu-id="43ddb-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="43ddb-113">Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, poderá lançá-las no fórum do [signalr ASP.net](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [stackoverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="43ddb-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

<span data-ttu-id="43ddb-114">A injeção de dependência é uma maneira de remover dependências embutidas em código entre objetos, facilitando a substituição de dependências de um objeto, seja para teste (usando objetos fictícios) ou para alterar o comportamento em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="43ddb-114">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="43ddb-115">Este tutorial mostra como executar a injeção de dependência em hubs de sinalização.</span><span class="sxs-lookup"><span data-stu-id="43ddb-115">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="43ddb-116">Ele também mostra como usar contêineres IoC com Signalr.</span><span class="sxs-lookup"><span data-stu-id="43ddb-116">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="43ddb-117">Um contêiner IoC é uma estrutura geral para injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="43ddb-117">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="43ddb-118">O que é injeção de dependência?</span><span class="sxs-lookup"><span data-stu-id="43ddb-118">What is Dependency Injection?</span></span>

<span data-ttu-id="43ddb-119">Ignore esta seção se você já estiver familiarizado com a injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="43ddb-119">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="43ddb-120">*Injeção de dependência* (di) é um padrão em que os objetos não são responsáveis por criar suas próprias dependências.</span><span class="sxs-lookup"><span data-stu-id="43ddb-120">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="43ddb-121">Aqui está um exemplo simples para motivar DI.</span><span class="sxs-lookup"><span data-stu-id="43ddb-121">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="43ddb-122">Suponha que você tenha um objeto que precise registrar mensagens.</span><span class="sxs-lookup"><span data-stu-id="43ddb-122">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="43ddb-123">Você pode definir uma interface de log:</span><span class="sxs-lookup"><span data-stu-id="43ddb-123">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="43ddb-124">Em seu objeto, você pode criar um `ILogger` para registrar mensagens:</span><span class="sxs-lookup"><span data-stu-id="43ddb-124">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="43ddb-125">Isso funciona, mas não é o melhor design.</span><span class="sxs-lookup"><span data-stu-id="43ddb-125">This works, but it's not the best design.</span></span> <span data-ttu-id="43ddb-126">Se você quiser substituir `FileLogger` por outra implementação de `ILogger`, precisará modificar `SomeComponent`.</span><span class="sxs-lookup"><span data-stu-id="43ddb-126">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="43ddb-127">Suponhamos que muitos outros objetos usam `FileLogger`, será necessário alterar todos eles.</span><span class="sxs-lookup"><span data-stu-id="43ddb-127">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="43ddb-128">Ou, se você decidir tornar `FileLogger` um singleton, também precisará fazer alterações em todo o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="43ddb-128">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="43ddb-129">Uma abordagem melhor é "injetar" um `ILogger` no objeto — por exemplo, usando um argumento de construtor:</span><span class="sxs-lookup"><span data-stu-id="43ddb-129">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="43ddb-130">Agora o objeto não é responsável por selecionar qual `ILogger` usar.</span><span class="sxs-lookup"><span data-stu-id="43ddb-130">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="43ddb-131">Você pode alternar `ILogger` implementações sem alterar os objetos que dependem dela.</span><span class="sxs-lookup"><span data-stu-id="43ddb-131">You can switch `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="43ddb-132">Esse padrão é chamado de [injeção de Construtor](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="43ddb-132">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="43ddb-133">Outro padrão é a injeção de setter, em que você define a dependência por meio de um método ou propriedade setter.</span><span class="sxs-lookup"><span data-stu-id="43ddb-133">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="43ddb-134">Injeção de dependência simples no Signalr</span><span class="sxs-lookup"><span data-stu-id="43ddb-134">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="43ddb-135">Considere o aplicativo de chat do tutorial [introdução com o signalr](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="43ddb-135">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="43ddb-136">Esta é a classe de Hub desse aplicativo:</span><span class="sxs-lookup"><span data-stu-id="43ddb-136">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="43ddb-137">Suponha que você deseja armazenar mensagens de chat no servidor antes de enviá-las.</span><span class="sxs-lookup"><span data-stu-id="43ddb-137">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="43ddb-138">Você pode definir uma interface que abstrai essa funcionalidade e usar DI para injetar a interface na classe `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="43ddb-138">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="43ddb-139">O único problema é que um aplicativo Signalr não cria hubs diretamente; O signalr os cria para você.</span><span class="sxs-lookup"><span data-stu-id="43ddb-139">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="43ddb-140">Por padrão, o Signalr espera que uma classe de Hub tenha um construtor sem parâmetros.</span><span class="sxs-lookup"><span data-stu-id="43ddb-140">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="43ddb-141">No entanto, você pode registrar facilmente uma função para criar instâncias de Hub e usar essa função para executar DI.</span><span class="sxs-lookup"><span data-stu-id="43ddb-141">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="43ddb-142">Registre a função chamando **GlobalHost. DependencyResolver. Register**.</span><span class="sxs-lookup"><span data-stu-id="43ddb-142">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="43ddb-143">Agora, o Signalr invocará essa função anônima sempre que precisar criar uma instância de `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="43ddb-143">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="43ddb-144">Contêineres IoC</span><span class="sxs-lookup"><span data-stu-id="43ddb-144">IoC Containers</span></span>

<span data-ttu-id="43ddb-145">O código anterior é adequado para casos simples.</span><span class="sxs-lookup"><span data-stu-id="43ddb-145">The previous code is fine for simple cases.</span></span> <span data-ttu-id="43ddb-146">Mas você ainda teve de escrever isso:</span><span class="sxs-lookup"><span data-stu-id="43ddb-146">But you still had to write this:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

<span data-ttu-id="43ddb-147">Em um aplicativo complexo com muitas dependências, talvez seja necessário escrever muitos desse código de "fiação".</span><span class="sxs-lookup"><span data-stu-id="43ddb-147">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="43ddb-148">Esse código pode ser difícil de manter, especialmente se as dependências estiverem aninhadas.</span><span class="sxs-lookup"><span data-stu-id="43ddb-148">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="43ddb-149">Também é difícil de testar unidade.</span><span class="sxs-lookup"><span data-stu-id="43ddb-149">It is also hard to unit test.</span></span>

<span data-ttu-id="43ddb-150">Uma solução é usar um contêiner IoC.</span><span class="sxs-lookup"><span data-stu-id="43ddb-150">One solution is to use an IoC container.</span></span> <span data-ttu-id="43ddb-151">Um contêiner IoC é um componente de software que é responsável por gerenciar dependências. Você registra os tipos com o contêiner e, em seguida, usa o contêiner para criar objetos.</span><span class="sxs-lookup"><span data-stu-id="43ddb-151">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="43ddb-152">O contêiner ilustra automaticamente as relações de dependência.</span><span class="sxs-lookup"><span data-stu-id="43ddb-152">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="43ddb-153">Muitos contêineres IoC também permitem que você controle coisas como o tempo de vida e o escopo do objeto.</span><span class="sxs-lookup"><span data-stu-id="43ddb-153">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="43ddb-154">"IoC" significa "inversão de controle", que é um padrão geral em que uma estrutura chama o código do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="43ddb-154">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="43ddb-155">Um contêiner IoC constrói seus objetos para você, que "inverte" o fluxo de controle usual.</span><span class="sxs-lookup"><span data-stu-id="43ddb-155">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>

## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="43ddb-156">Usando contêineres IoC no Signalr</span><span class="sxs-lookup"><span data-stu-id="43ddb-156">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="43ddb-157">O aplicativo de chat provavelmente é muito simples de se beneficiar de um contêiner IoC.</span><span class="sxs-lookup"><span data-stu-id="43ddb-157">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="43ddb-158">Em vez disso, vamos dar uma olhada no exemplo de [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) .</span><span class="sxs-lookup"><span data-stu-id="43ddb-158">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="43ddb-159">O exemplo StockTicker define duas classes principais:</span><span class="sxs-lookup"><span data-stu-id="43ddb-159">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="43ddb-160">`StockTickerHub`: a classe Hub, que gerencia as conexões de cliente.</span><span class="sxs-lookup"><span data-stu-id="43ddb-160">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="43ddb-161">`StockTicker`: um singleton que contém preços de estoque e os atualiza periodicamente.</span><span class="sxs-lookup"><span data-stu-id="43ddb-161">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="43ddb-162">`StockTickerHub` mantém uma referência ao singleton `StockTicker`, enquanto `StockTicker` mantém uma referência ao **IHubConnectionContext** para o `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="43ddb-162">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="43ddb-163">Ele usa essa interface para se comunicar com instâncias de `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="43ddb-163">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="43ddb-164">(Para obter mais informações, consulte [transmissão de servidor com signalr ASP.net](../getting-started/tutorial-server-broadcast-with-signalr.md).)</span><span class="sxs-lookup"><span data-stu-id="43ddb-164">(For more information, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)</span></span>

<span data-ttu-id="43ddb-165">Podemos usar um contêiner IoC para Untangle essas dependências um pouco.</span><span class="sxs-lookup"><span data-stu-id="43ddb-165">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="43ddb-166">Primeiro, vamos simplificar as classes `StockTickerHub` e `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="43ddb-166">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="43ddb-167">No código a seguir, eu comentei as partes que não precisamos.</span><span class="sxs-lookup"><span data-stu-id="43ddb-167">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="43ddb-168">Remova o construtor sem parâmetros de `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="43ddb-168">Remove the parameterless constructor from `StockTickerHub`.</span></span> <span data-ttu-id="43ddb-169">Em vez disso, sempre usaremos DI para criar o Hub.</span><span class="sxs-lookup"><span data-stu-id="43ddb-169">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="43ddb-170">Para StockTicker, remova a instância singleton.</span><span class="sxs-lookup"><span data-stu-id="43ddb-170">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="43ddb-171">Posteriormente, usaremos o contêiner IoC para controlar o tempo de vida do StockTicker.</span><span class="sxs-lookup"><span data-stu-id="43ddb-171">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="43ddb-172">Além disso, torne o construtor público.</span><span class="sxs-lookup"><span data-stu-id="43ddb-172">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="43ddb-173">Em seguida, podemos refatorar o código criando uma interface para `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="43ddb-173">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="43ddb-174">Usaremos essa interface para desacoplar a `StockTickerHub` da classe `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="43ddb-174">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="43ddb-175">O Visual Studio torna esse tipo de refatoração fácil.</span><span class="sxs-lookup"><span data-stu-id="43ddb-175">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="43ddb-176">Abra o arquivo StockTicker.cs, clique com o botão direito do mouse na declaração de classe `StockTicker` e selecione **Refactor** ... **Extrair interface**.</span><span class="sxs-lookup"><span data-stu-id="43ddb-176">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="43ddb-177">Na caixa de diálogo **extrair interface** , clique em **selecionar tudo**.</span><span class="sxs-lookup"><span data-stu-id="43ddb-177">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="43ddb-178">Deixe os outros padrões.</span><span class="sxs-lookup"><span data-stu-id="43ddb-178">Leave the other defaults.</span></span> <span data-ttu-id="43ddb-179">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="43ddb-179">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="43ddb-180">O Visual Studio cria uma nova interface chamada `IStockTicker`e também altera `StockTicker` para derivar de `IStockTicker`.</span><span class="sxs-lookup"><span data-stu-id="43ddb-180">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="43ddb-181">Abra o arquivo IStockTicker.cs e altere a interface para **Public**.</span><span class="sxs-lookup"><span data-stu-id="43ddb-181">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="43ddb-182">Na classe `StockTickerHub`, altere as duas instâncias de `StockTicker` para `IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="43ddb-182">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="43ddb-183">A criação de uma interface de `IStockTicker` não é estritamente necessária, mas eu queria mostrar como a DI pode ajudar a reduzir o acoplamento entre componentes em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="43ddb-183">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="43ddb-184">Adicionar a biblioteca Ninject</span><span class="sxs-lookup"><span data-stu-id="43ddb-184">Add the Ninject Library</span></span>

<span data-ttu-id="43ddb-185">Há muitos contêineres IoC de código aberto para .NET.</span><span class="sxs-lookup"><span data-stu-id="43ddb-185">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="43ddb-186">Para este tutorial, usarei [Ninject](http://www.ninject.org/).</span><span class="sxs-lookup"><span data-stu-id="43ddb-186">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="43ddb-187">(Outras bibliotecas populares incluem [Castle Windsor](http://www.castleproject.org/), [Spring.net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity)e [StructureMap](http://docs.structuremap.net).)</span><span class="sxs-lookup"><span data-stu-id="43ddb-187">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="43ddb-188">Use o Gerenciador de pacotes NuGet para instalar a [biblioteca Ninject](https://nuget.org/packages/Ninject/3.0.1.10).</span><span class="sxs-lookup"><span data-stu-id="43ddb-188">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="43ddb-189">No Visual Studio, no menu **ferramentas** , selecione **Gerenciador de pacotes NuGet** > **console do Gerenciador de pacotes**.</span><span class="sxs-lookup"><span data-stu-id="43ddb-189">In Visual Studio, from the **Tools** menu select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="43ddb-190">Na janela Console do Gerenciador de Pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="43ddb-190">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="43ddb-191">Substituir o resolvedor de dependência do Signalr</span><span class="sxs-lookup"><span data-stu-id="43ddb-191">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="43ddb-192">Para usar o Ninject no Signalr, crie uma classe derivada de **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="43ddb-192">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="43ddb-193">Essa classe substitui os métodos **GetService** e **GetServices** de **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="43ddb-193">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="43ddb-194">O signalr chama esses métodos para criar vários objetos em tempo de execução, incluindo instâncias de Hub, bem como vários serviços usados internamente pelo Signalr.</span><span class="sxs-lookup"><span data-stu-id="43ddb-194">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="43ddb-195">O método **GetService** cria uma única instância de um tipo.</span><span class="sxs-lookup"><span data-stu-id="43ddb-195">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="43ddb-196">Substitua esse método para chamar o método **TryGet** do kernel Ninject.</span><span class="sxs-lookup"><span data-stu-id="43ddb-196">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="43ddb-197">Se esse método retornar NULL, volte para o resolvedor padrão.</span><span class="sxs-lookup"><span data-stu-id="43ddb-197">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="43ddb-198">O método **GetServices** cria uma coleção de objetos de um tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="43ddb-198">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="43ddb-199">Substitua esse método para concatenar os resultados de Ninject com os resultados do resolvedor padrão.</span><span class="sxs-lookup"><span data-stu-id="43ddb-199">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="43ddb-200">Configurar associações Ninject</span><span class="sxs-lookup"><span data-stu-id="43ddb-200">Configure Ninject Bindings</span></span>

<span data-ttu-id="43ddb-201">Agora, usaremos Ninject para declarar associações de tipo.</span><span class="sxs-lookup"><span data-stu-id="43ddb-201">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="43ddb-202">Abra a classe Startup.cs do aplicativo (que você criou manualmente de acordo com as instruções do pacote em `readme.txt`ou que foi criado adicionando a autenticação ao seu projeto).</span><span class="sxs-lookup"><span data-stu-id="43ddb-202">Open your application's Startup.cs class (that you either created manually as per the package instructions in `readme.txt`, or that was created by adding authentication to your project).</span></span> <span data-ttu-id="43ddb-203">No método `Startup.Configuration`, crie o contêiner Ninject, que Ninject chama o *kernel*.</span><span class="sxs-lookup"><span data-stu-id="43ddb-203">In the `Startup.Configuration` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="43ddb-204">Crie uma instância do nosso resolvedor de dependências personalizado:</span><span class="sxs-lookup"><span data-stu-id="43ddb-204">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="43ddb-205">Crie uma associação para `IStockTicker` da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="43ddb-205">Create a binding for `IStockTicker` as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample17.cs)]

<span data-ttu-id="43ddb-206">Esse código está dizendo duas coisas.</span><span class="sxs-lookup"><span data-stu-id="43ddb-206">This code is saying two things.</span></span> <span data-ttu-id="43ddb-207">Primeiro, sempre que o aplicativo precisar de uma `IStockTicker`, o kernel deverá criar uma instância do `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="43ddb-207">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="43ddb-208">Em segundo lugar, a classe `StockTicker` deve ser uma criada como um objeto singleton.</span><span class="sxs-lookup"><span data-stu-id="43ddb-208">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="43ddb-209">Ninject criará uma instância do objeto e retornará a mesma instância para cada solicitação.</span><span class="sxs-lookup"><span data-stu-id="43ddb-209">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="43ddb-210">Crie uma associação para **IHubConnectionContext** da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="43ddb-210">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="43ddb-211">Esse código cria uma função anônima que retorna um **IHubConnection**.</span><span class="sxs-lookup"><span data-stu-id="43ddb-211">This code creates an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="43ddb-212">O método **WhenInjectedInto** informa Ninject para usar essa função somente ao criar `IStockTicker` instâncias.</span><span class="sxs-lookup"><span data-stu-id="43ddb-212">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="43ddb-213">O motivo é que o Signalr cria instâncias **IHubConnectionContext** internamente e não queremos substituir como o signalr as cria.</span><span class="sxs-lookup"><span data-stu-id="43ddb-213">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="43ddb-214">Essa função se aplica somente à nossa classe de `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="43ddb-214">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="43ddb-215">Passe o resolvedor de dependência para o método **MapSignalR** adicionando uma configuração de Hub:</span><span class="sxs-lookup"><span data-stu-id="43ddb-215">Pass the dependency resolver into the **MapSignalR** method by adding a hub configuration:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="43ddb-216">Atualize o método Startup. ConfigureSignalR na classe de inicialização do exemplo com o novo parâmetro:</span><span class="sxs-lookup"><span data-stu-id="43ddb-216">Update the Startup.ConfigureSignalR method in the sample's Startup class with the new parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="43ddb-217">Agora, o Signalr usará o resolvedor especificado em **MapSignalR**, em vez do resolvedor padrão.</span><span class="sxs-lookup"><span data-stu-id="43ddb-217">Now SignalR will use the resolver specified in **MapSignalR**, instead of the default resolver.</span></span>

<span data-ttu-id="43ddb-218">Aqui está a listagem de código completa para `Startup.Configuration`.</span><span class="sxs-lookup"><span data-stu-id="43ddb-218">Here is the complete code listing for `Startup.Configuration`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample21.cs)]

<span data-ttu-id="43ddb-219">Para executar o aplicativo StockTicker no Visual Studio, pressione F5.</span><span class="sxs-lookup"><span data-stu-id="43ddb-219">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="43ddb-220">Na janela do navegador, navegue até `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span><span class="sxs-lookup"><span data-stu-id="43ddb-220">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="43ddb-221">O aplicativo tem exatamente a mesma funcionalidade que antes.</span><span class="sxs-lookup"><span data-stu-id="43ddb-221">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="43ddb-222">(Para obter uma descrição, consulte [transmissão de servidor com signalr ASP.net](../getting-started/tutorial-server-broadcast-with-signalr.md).) Não alteramos o comportamento; apenas tornaria o código mais fácil de testar, manter e evoluir.</span><span class="sxs-lookup"><span data-stu-id="43ddb-222">(For a description, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
