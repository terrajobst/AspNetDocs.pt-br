---
uid: signalr/overview/older-versions/dependency-injection
title: Injeção de dependência no Signalr 1. x | Microsoft Docs
author: bradygaster
description: ''
ms.author: bradyg
ms.date: 05/15/2013
ms.assetid: eaa206c4-edb3-487e-8fcb-54a3261fed36
msc.legacyurl: /signalr/overview/older-versions/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: de838ab6b3a299eb1e5ebeb9fa3c583478ce3e56
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536949"
---
# <a name="dependency-injection-in-signalr-1x"></a><span data-ttu-id="06907-102">Injeção de dependência no SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="06907-102">Dependency Injection in SignalR 1.x</span></span>

<span data-ttu-id="06907-103">por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="06907-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="06907-104">A injeção de dependência é uma maneira de remover dependências embutidas em código entre objetos, facilitando a substituição de dependências de um objeto, seja para teste (usando objetos fictícios) ou para alterar o comportamento em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="06907-104">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="06907-105">Este tutorial mostra como executar a injeção de dependência em hubs de sinalização.</span><span class="sxs-lookup"><span data-stu-id="06907-105">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="06907-106">Ele também mostra como usar contêineres IoC com Signalr.</span><span class="sxs-lookup"><span data-stu-id="06907-106">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="06907-107">Um contêiner IoC é uma estrutura geral para injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="06907-107">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="06907-108">O que é injeção de dependência?</span><span class="sxs-lookup"><span data-stu-id="06907-108">What is Dependency Injection?</span></span>

<span data-ttu-id="06907-109">Ignore esta seção se você já estiver familiarizado com a injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="06907-109">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="06907-110">*Injeção de dependência* (di) é um padrão em que os objetos não são responsáveis por criar suas próprias dependências.</span><span class="sxs-lookup"><span data-stu-id="06907-110">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="06907-111">Aqui está um exemplo simples para motivar DI.</span><span class="sxs-lookup"><span data-stu-id="06907-111">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="06907-112">Suponha que você tenha um objeto que precise registrar mensagens.</span><span class="sxs-lookup"><span data-stu-id="06907-112">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="06907-113">Você pode definir uma interface de log:</span><span class="sxs-lookup"><span data-stu-id="06907-113">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="06907-114">Em seu objeto, você pode criar um `ILogger` para registrar mensagens:</span><span class="sxs-lookup"><span data-stu-id="06907-114">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="06907-115">Isso funciona, mas não é o melhor design.</span><span class="sxs-lookup"><span data-stu-id="06907-115">This works, but it's not the best design.</span></span> <span data-ttu-id="06907-116">Se você quiser substituir `FileLogger` por outra implementação de `ILogger`, precisará modificar `SomeComponent`.</span><span class="sxs-lookup"><span data-stu-id="06907-116">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="06907-117">Suponhamos que muitos outros objetos usam `FileLogger`, será necessário alterar todos eles.</span><span class="sxs-lookup"><span data-stu-id="06907-117">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="06907-118">Ou, se você decidir tornar `FileLogger` um singleton, também precisará fazer alterações em todo o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="06907-118">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="06907-119">Uma abordagem melhor é "injetar" um `ILogger` no objeto — por exemplo, usando um argumento de construtor:</span><span class="sxs-lookup"><span data-stu-id="06907-119">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="06907-120">Agora o objeto não é responsável por selecionar qual `ILogger` usar.</span><span class="sxs-lookup"><span data-stu-id="06907-120">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="06907-121">Você pode alternar `ILogger` implementações sem alterar os objetos que dependem dela.</span><span class="sxs-lookup"><span data-stu-id="06907-121">You can switch `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="06907-122">Esse padrão é chamado de [injeção de Construtor](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="06907-122">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="06907-123">Outro padrão é a injeção de setter, em que você define a dependência por meio de um método ou propriedade setter.</span><span class="sxs-lookup"><span data-stu-id="06907-123">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="06907-124">Injeção de dependência simples no Signalr</span><span class="sxs-lookup"><span data-stu-id="06907-124">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="06907-125">Considere o aplicativo de chat do tutorial [introdução com o signalr](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="06907-125">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="06907-126">Esta é a classe de Hub desse aplicativo:</span><span class="sxs-lookup"><span data-stu-id="06907-126">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="06907-127">Suponha que você deseja armazenar mensagens de chat no servidor antes de enviá-las.</span><span class="sxs-lookup"><span data-stu-id="06907-127">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="06907-128">Você pode definir uma interface que abstrai essa funcionalidade e usar DI para injetar a interface na classe `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="06907-128">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="06907-129">O único problema é que um aplicativo Signalr não cria hubs diretamente; O signalr os cria para você.</span><span class="sxs-lookup"><span data-stu-id="06907-129">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="06907-130">Por padrão, o Signalr espera que uma classe de Hub tenha um construtor sem parâmetros.</span><span class="sxs-lookup"><span data-stu-id="06907-130">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="06907-131">No entanto, você pode registrar facilmente uma função para criar instâncias de Hub e usar essa função para executar DI.</span><span class="sxs-lookup"><span data-stu-id="06907-131">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="06907-132">Registre a função chamando **GlobalHost. DependencyResolver. Register**.</span><span class="sxs-lookup"><span data-stu-id="06907-132">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="06907-133">Agora, o Signalr invocará essa função anônima sempre que precisar criar uma instância de `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="06907-133">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="06907-134">Contêineres IoC</span><span class="sxs-lookup"><span data-stu-id="06907-134">IoC Containers</span></span>

<span data-ttu-id="06907-135">O código anterior é adequado para casos simples.</span><span class="sxs-lookup"><span data-stu-id="06907-135">The previous code is fine for simple cases.</span></span> <span data-ttu-id="06907-136">Mas você ainda teve de escrever isso:</span><span class="sxs-lookup"><span data-stu-id="06907-136">But you still had to write this:</span></span>

[!code-css[Main](dependency-injection/samples/sample8.css)]

<span data-ttu-id="06907-137">Em um aplicativo complexo com muitas dependências, talvez seja necessário escrever muitos desse código de "fiação".</span><span class="sxs-lookup"><span data-stu-id="06907-137">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="06907-138">Esse código pode ser difícil de manter, especialmente se as dependências estiverem aninhadas.</span><span class="sxs-lookup"><span data-stu-id="06907-138">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="06907-139">Também é difícil de testar unidade.</span><span class="sxs-lookup"><span data-stu-id="06907-139">It is also hard to unit test.</span></span>

<span data-ttu-id="06907-140">Uma solução é usar um contêiner IoC.</span><span class="sxs-lookup"><span data-stu-id="06907-140">One solution is to use an IoC container.</span></span> <span data-ttu-id="06907-141">Um contêiner IoC é um componente de software que é responsável por gerenciar dependências. Você registra os tipos com o contêiner e, em seguida, usa o contêiner para criar objetos.</span><span class="sxs-lookup"><span data-stu-id="06907-141">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="06907-142">O contêiner ilustra automaticamente as relações de dependência.</span><span class="sxs-lookup"><span data-stu-id="06907-142">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="06907-143">Muitos contêineres IoC também permitem que você controle coisas como o tempo de vida e o escopo do objeto.</span><span class="sxs-lookup"><span data-stu-id="06907-143">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="06907-144">"IoC" significa "inversão de controle", que é um padrão geral em que uma estrutura chama o código do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="06907-144">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="06907-145">Um contêiner IoC constrói seus objetos para você, que "inverte" o fluxo de controle usual.</span><span class="sxs-lookup"><span data-stu-id="06907-145">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>

## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="06907-146">Usando contêineres IoC no Signalr</span><span class="sxs-lookup"><span data-stu-id="06907-146">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="06907-147">O aplicativo de chat provavelmente é muito simples de se beneficiar de um contêiner IoC.</span><span class="sxs-lookup"><span data-stu-id="06907-147">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="06907-148">Em vez disso, vamos dar uma olhada no exemplo de [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) .</span><span class="sxs-lookup"><span data-stu-id="06907-148">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="06907-149">O exemplo StockTicker define duas classes principais:</span><span class="sxs-lookup"><span data-stu-id="06907-149">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="06907-150">`StockTickerHub`: a classe Hub, que gerencia as conexões de cliente.</span><span class="sxs-lookup"><span data-stu-id="06907-150">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="06907-151">`StockTicker`: um singleton que contém preços de estoque e os atualiza periodicamente.</span><span class="sxs-lookup"><span data-stu-id="06907-151">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="06907-152">`StockTickerHub` mantém uma referência ao singleton `StockTicker`, enquanto `StockTicker` mantém uma referência ao **IHubConnectionContext** para o `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="06907-152">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="06907-153">Ele usa essa interface para se comunicar com instâncias de `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="06907-153">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="06907-154">(Para obter mais informações, consulte [transmissão de servidor com signalr ASP.net](index.md).)</span><span class="sxs-lookup"><span data-stu-id="06907-154">(For more information, see [Server Broadcast with ASP.NET SignalR](index.md).)</span></span>

<span data-ttu-id="06907-155">Podemos usar um contêiner IoC para Untangle essas dependências um pouco.</span><span class="sxs-lookup"><span data-stu-id="06907-155">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="06907-156">Primeiro, vamos simplificar as classes `StockTickerHub` e `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="06907-156">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="06907-157">No código a seguir, eu comentei as partes que não precisamos.</span><span class="sxs-lookup"><span data-stu-id="06907-157">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="06907-158">Remova o construtor sem parâmetros de `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="06907-158">Remove the parameterless constructor from `StockTicker`.</span></span> <span data-ttu-id="06907-159">Em vez disso, sempre usaremos DI para criar o Hub.</span><span class="sxs-lookup"><span data-stu-id="06907-159">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="06907-160">Para StockTicker, remova a instância singleton.</span><span class="sxs-lookup"><span data-stu-id="06907-160">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="06907-161">Posteriormente, usaremos o contêiner IoC para controlar o tempo de vida do StockTicker.</span><span class="sxs-lookup"><span data-stu-id="06907-161">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="06907-162">Além disso, torne o construtor público.</span><span class="sxs-lookup"><span data-stu-id="06907-162">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="06907-163">Em seguida, podemos refatorar o código criando uma interface para `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="06907-163">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="06907-164">Usaremos essa interface para desacoplar a `StockTickerHub` da classe `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="06907-164">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="06907-165">O Visual Studio torna esse tipo de refatoração fácil.</span><span class="sxs-lookup"><span data-stu-id="06907-165">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="06907-166">Abra o arquivo StockTicker.cs, clique com o botão direito do mouse na declaração de classe `StockTicker` e selecione **Refactor** ... **Extrair interface**.</span><span class="sxs-lookup"><span data-stu-id="06907-166">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="06907-167">Na caixa de diálogo **extrair interface** , clique em **selecionar tudo**.</span><span class="sxs-lookup"><span data-stu-id="06907-167">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="06907-168">Deixe os outros padrões.</span><span class="sxs-lookup"><span data-stu-id="06907-168">Leave the other defaults.</span></span> <span data-ttu-id="06907-169">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="06907-169">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="06907-170">O Visual Studio cria uma nova interface chamada `IStockTicker`e também altera `StockTicker` para derivar de `IStockTicker`.</span><span class="sxs-lookup"><span data-stu-id="06907-170">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="06907-171">Abra o arquivo IStockTicker.cs e altere a interface para **Public**.</span><span class="sxs-lookup"><span data-stu-id="06907-171">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="06907-172">Na classe `StockTickerHub`, altere as duas instâncias de `StockTicker` para `IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="06907-172">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="06907-173">A criação de uma interface de `IStockTicker` não é estritamente necessária, mas eu queria mostrar como a DI pode ajudar a reduzir o acoplamento entre componentes em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="06907-173">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="06907-174">Adicionar a biblioteca Ninject</span><span class="sxs-lookup"><span data-stu-id="06907-174">Add the Ninject Library</span></span>

<span data-ttu-id="06907-175">Há muitos contêineres IoC de código aberto para .NET.</span><span class="sxs-lookup"><span data-stu-id="06907-175">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="06907-176">Para este tutorial, usarei [Ninject](http://www.ninject.org/).</span><span class="sxs-lookup"><span data-stu-id="06907-176">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="06907-177">(Outras bibliotecas populares incluem [Castle Windsor](http://www.castleproject.org/), [Spring.net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity)e [StructureMap](http://docs.structuremap.net).)</span><span class="sxs-lookup"><span data-stu-id="06907-177">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="06907-178">Use o Gerenciador de pacotes NuGet para instalar a [biblioteca Ninject](https://nuget.org/packages/Ninject/3.0.1.10).</span><span class="sxs-lookup"><span data-stu-id="06907-178">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="06907-179">No Visual Studio, no menu **ferramentas** , selecione **Gerenciador de pacotes NuGet** > **console do Gerenciador de pacotes**.</span><span class="sxs-lookup"><span data-stu-id="06907-179">In Visual Studio, from the **Tools** menu select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="06907-180">Na janela Console do Gerenciador de Pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="06907-180">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="06907-181">Substituir o resolvedor de dependência do Signalr</span><span class="sxs-lookup"><span data-stu-id="06907-181">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="06907-182">Para usar o Ninject no Signalr, crie uma classe derivada de **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="06907-182">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="06907-183">Essa classe substitui os métodos **GetService** e **GetServices** de **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="06907-183">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="06907-184">O signalr chama esses métodos para criar vários objetos em tempo de execução, incluindo instâncias de Hub, bem como vários serviços usados internamente pelo Signalr.</span><span class="sxs-lookup"><span data-stu-id="06907-184">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="06907-185">O método **GetService** cria uma única instância de um tipo.</span><span class="sxs-lookup"><span data-stu-id="06907-185">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="06907-186">Substitua esse método para chamar o método **TryGet** do kernel Ninject.</span><span class="sxs-lookup"><span data-stu-id="06907-186">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="06907-187">Se esse método retornar NULL, volte para o resolvedor padrão.</span><span class="sxs-lookup"><span data-stu-id="06907-187">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="06907-188">O método **GetServices** cria uma coleção de objetos de um tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="06907-188">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="06907-189">Substitua esse método para concatenar os resultados de Ninject com os resultados do resolvedor padrão.</span><span class="sxs-lookup"><span data-stu-id="06907-189">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="06907-190">Configurar associações Ninject</span><span class="sxs-lookup"><span data-stu-id="06907-190">Configure Ninject Bindings</span></span>

<span data-ttu-id="06907-191">Agora, usaremos Ninject para declarar associações de tipo.</span><span class="sxs-lookup"><span data-stu-id="06907-191">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="06907-192">Abra o arquivo RegisterHubs.cs.</span><span class="sxs-lookup"><span data-stu-id="06907-192">Open the file RegisterHubs.cs.</span></span> <span data-ttu-id="06907-193">No método `RegisterHubs.Start`, crie o contêiner Ninject, que Ninject chama o *kernel*.</span><span class="sxs-lookup"><span data-stu-id="06907-193">In the `RegisterHubs.Start` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="06907-194">Crie uma instância do nosso resolvedor de dependências personalizado:</span><span class="sxs-lookup"><span data-stu-id="06907-194">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="06907-195">Crie uma associação para `IStockTicker` da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="06907-195">Create a binding for `IStockTicker` as follows:</span></span>

[!code-html[Main](dependency-injection/samples/sample17.html)]

<span data-ttu-id="06907-196">Esse código está dizendo duas coisas.</span><span class="sxs-lookup"><span data-stu-id="06907-196">This code is saying two things.</span></span> <span data-ttu-id="06907-197">Primeiro, sempre que o aplicativo precisar de uma `IStockTicker`, o kernel deverá criar uma instância do `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="06907-197">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="06907-198">Em segundo lugar, a classe `StockTicker` deve ser uma criada como um objeto singleton.</span><span class="sxs-lookup"><span data-stu-id="06907-198">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="06907-199">Ninject criará uma instância do objeto e retornará a mesma instância para cada solicitação.</span><span class="sxs-lookup"><span data-stu-id="06907-199">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="06907-200">Crie uma associação para **IHubConnectionContext** da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="06907-200">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="06907-201">Esse código cria uma função anônima que retorna um **IHubConnection**.</span><span class="sxs-lookup"><span data-stu-id="06907-201">This code creates an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="06907-202">O método **WhenInjectedInto** informa Ninject para usar essa função somente ao criar `IStockTicker` instâncias.</span><span class="sxs-lookup"><span data-stu-id="06907-202">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="06907-203">O motivo é que o Signalr cria instâncias **IHubConnectionContext** internamente e não queremos substituir como o signalr as cria.</span><span class="sxs-lookup"><span data-stu-id="06907-203">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="06907-204">Essa função se aplica somente à nossa classe de `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="06907-204">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="06907-205">Passe o resolvedor de dependência para o método **MapHubs** :</span><span class="sxs-lookup"><span data-stu-id="06907-205">Pass the dependency resolver into the **MapHubs** method:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="06907-206">Agora, o Signalr usará o resolvedor especificado em **MapHubs**, em vez do resolvedor padrão.</span><span class="sxs-lookup"><span data-stu-id="06907-206">Now SignalR will use the resolver specified in **MapHubs**, instead of the default resolver.</span></span>

<span data-ttu-id="06907-207">Aqui está a listagem de código completa para `RegisterHubs.Start`.</span><span class="sxs-lookup"><span data-stu-id="06907-207">Here is the complete code listing for `RegisterHubs.Start`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="06907-208">Para executar o aplicativo StockTicker no Visual Studio, pressione F5.</span><span class="sxs-lookup"><span data-stu-id="06907-208">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="06907-209">Na janela do navegador, navegue até `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span><span class="sxs-lookup"><span data-stu-id="06907-209">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="06907-210">O aplicativo tem exatamente a mesma funcionalidade que antes.</span><span class="sxs-lookup"><span data-stu-id="06907-210">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="06907-211">(Para obter uma descrição, consulte [transmissão de servidor com signalr ASP.net](index.md).) Não alteramos o comportamento; apenas tornaria o código mais fácil de testar, manter e evoluir.</span><span class="sxs-lookup"><span data-stu-id="06907-211">(For a description, see [Server Broadcast with ASP.NET SignalR](index.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
