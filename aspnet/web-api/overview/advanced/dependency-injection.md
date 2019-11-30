---
uid: web-api/overview/advanced/dependency-injection
title: Injeção de dependência no ASP.NET Web API 2-ASP.NET 4. x
author: MikeWasson
description: Este tutorial mostra como injetar dependências em seu controlador de ASP.NET Web API para o ASP.NET 4. x.
ms.author: riande
ms.date: 01/20/2014
ms.custom: seoapril2019
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: f9c212af92168ac02644625b9aa8ec1bef329cab
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600419"
---
# <a name="dependency-injection-in-aspnet-web-api-2"></a><span data-ttu-id="f4ee9-103">Injeção de dependência no ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="f4ee9-103">Dependency Injection in ASP.NET Web API 2</span></span>

<span data-ttu-id="f4ee9-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f4ee9-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="f4ee9-105">Baixar projeto concluído</span><span class="sxs-lookup"><span data-stu-id="f4ee9-105">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> <span data-ttu-id="f4ee9-106">Este tutorial mostra como injetar dependências em seu controlador de ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-106">This tutorial shows how to inject dependencies into your ASP.NET Web API controller.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f4ee9-107">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="f4ee9-107">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="f4ee9-108">API Web 2</span><span class="sxs-lookup"><span data-stu-id="f4ee9-108">Web API 2</span></span>
> - [<span data-ttu-id="f4ee9-109">Bloco de aplicativo do Unity</span><span class="sxs-lookup"><span data-stu-id="f4ee9-109">Unity Application Block</span></span>](https://www.nuget.org/packages/Unity/)
> - <span data-ttu-id="f4ee9-110">Entity Framework 6 (a versão 5 também funciona)</span><span class="sxs-lookup"><span data-stu-id="f4ee9-110">Entity Framework 6 (version 5 also works)</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="f4ee9-111">O que é injeção de dependência?</span><span class="sxs-lookup"><span data-stu-id="f4ee9-111">What is Dependency Injection?</span></span>

<span data-ttu-id="f4ee9-112">Uma *dependência* é qualquer objeto exigido por outro objeto.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-112">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="f4ee9-113">Por exemplo, é comum definir um [repositório](http://martinfowler.com/eaaCatalog/repository.html) que lide com o acesso a dados.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-113">For example, it's common to define a [repository](http://martinfowler.com/eaaCatalog/repository.html) that handles data access.</span></span> <span data-ttu-id="f4ee9-114">Vamos ilustrar com um exemplo.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-114">Let's illustrate with an example.</span></span> <span data-ttu-id="f4ee9-115">Primeiro, vamos definir um modelo de domínio:</span><span class="sxs-lookup"><span data-stu-id="f4ee9-115">First, we'll define a domain model:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="f4ee9-116">Aqui está uma classe de repositório simples que armazena itens em um banco de dados, usando Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-116">Here is a simple repository class that stores items in a database, using Entity Framework.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="f4ee9-117">Agora, vamos definir um controlador de API da Web que dá suporte a solicitações GET para `Product` entidades.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-117">Now let's define a Web API controller that supports GET requests for `Product` entities.</span></span> <span data-ttu-id="f4ee9-118">(Estou deixando o POST e outros métodos para simplificar.) Esta é uma primeira tentativa:</span><span class="sxs-lookup"><span data-stu-id="f4ee9-118">(I'm leaving out POST and other methods for simplicity.) Here is a first attempt:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="f4ee9-119">Observe que a classe Controller depende de `ProductRepository`e estamos permitindo que o controlador crie a instância de `ProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-119">Notice that the controller class depends on `ProductRepository`, and we are letting the controller create the `ProductRepository` instance.</span></span> <span data-ttu-id="f4ee9-120">No entanto, é uma boa ideia codificar a dependência dessa forma, por vários motivos.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-120">However, it's a bad idea to hard code the dependency in this way, for several reasons.</span></span>

- <span data-ttu-id="f4ee9-121">Se você quiser substituir `ProductRepository` por uma implementação diferente, também precisará modificar a classe do controlador.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-121">If you want to replace `ProductRepository` with a different implementation, you also need to modify the controller class.</span></span>
- <span data-ttu-id="f4ee9-122">Se o `ProductRepository` tiver dependências, você deverá configurá-las dentro do controlador.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-122">If the `ProductRepository` has dependencies, you must configure these inside the controller.</span></span> <span data-ttu-id="f4ee9-123">Para um projeto grande com vários controladores, seu código de configuração se torna disperso em seu projeto.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-123">For a large project with multiple controllers, your configuration code becomes scattered across your project.</span></span>
- <span data-ttu-id="f4ee9-124">É difícil fazer o teste de unidade, pois o controlador é embutido em código para consultar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-124">It is hard to unit test, because the controller is hard-coded to query the database.</span></span> <span data-ttu-id="f4ee9-125">Para um teste de unidade, você deve usar um repositório de simulação ou de stub, o que não é possível com o design atual.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-125">For a unit test, you should use a mock or stub repository, which is not possible with the current design.</span></span>

<span data-ttu-id="f4ee9-126">Podemos resolver esses problemas *injetando* o repositório no controlador.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-126">We can address these problems by *injecting* the repository into the controller.</span></span> <span data-ttu-id="f4ee9-127">Primeiro, refatore a classe `ProductRepository` em uma interface:</span><span class="sxs-lookup"><span data-stu-id="f4ee9-127">First, refactor the `ProductRepository` class into an interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="f4ee9-128">Em seguida, forneça o `IProductRepository` como um parâmetro de construtor:</span><span class="sxs-lookup"><span data-stu-id="f4ee9-128">Then provide the `IProductRepository` as a constructor parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="f4ee9-129">Este exemplo usa [injeção de Construtor](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="f4ee9-129">This example uses [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="f4ee9-130">Você também pode usar a *injeção de setter*, em que você define a dependência por meio de um método ou propriedade setter.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-130">You can also use *setter injection*, where you set the dependency through a setter method or property.</span></span>

<span data-ttu-id="f4ee9-131">Mas agora há um problema, porque seu aplicativo não cria o controlador diretamente.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-131">But now there is a problem, because your application doesn't create the controller directly.</span></span> <span data-ttu-id="f4ee9-132">A API da Web cria o controlador ao rotear a solicitação, e a API da Web não sabe nada sobre `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-132">Web API creates the controller when it routes the request, and Web API doesn't know anything about `IProductRepository`.</span></span> <span data-ttu-id="f4ee9-133">É aí que entra o resolvedor de dependências da API Web.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-133">This is where the Web API dependency resolver comes in.</span></span>

## <a name="the-web-api-dependency-resolver"></a><span data-ttu-id="f4ee9-134">O resolvedor de dependência da API Web</span><span class="sxs-lookup"><span data-stu-id="f4ee9-134">The Web API Dependency Resolver</span></span>

<span data-ttu-id="f4ee9-135">A API Web define a interface **IDependencyResolver** para resolver dependências.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-135">Web API defines the **IDependencyResolver** interface for resolving dependencies.</span></span> <span data-ttu-id="f4ee9-136">Aqui está a definição da interface:</span><span class="sxs-lookup"><span data-stu-id="f4ee9-136">Here is the definition of the interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="f4ee9-137">A interface **IDependencyScope** tem dois métodos:</span><span class="sxs-lookup"><span data-stu-id="f4ee9-137">The **IDependencyScope** interface has two methods:</span></span>

- <span data-ttu-id="f4ee9-138">**GetService** cria uma instância de um tipo.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-138">**GetService** creates one instance of a type.</span></span>
- <span data-ttu-id="f4ee9-139">**GetServices** cria uma coleção de objetos de um tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-139">**GetServices** creates a collection of objects of a specified type.</span></span>

<span data-ttu-id="f4ee9-140">O método **IDependencyResolver** herda **IDependencyScope** e adiciona o método **BeginScope** .</span><span class="sxs-lookup"><span data-stu-id="f4ee9-140">The **IDependencyResolver** method inherits **IDependencyScope** and adds the **BeginScope** method.</span></span> <span data-ttu-id="f4ee9-141">Falarei sobre escopos mais adiante neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-141">I'll talk about scopes later in this tutorial.</span></span>

<span data-ttu-id="f4ee9-142">Quando a API da Web cria uma instância de controlador, ela primeiro chama **IDependencyResolver. GetService**, passando o tipo de controlador.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-142">When Web API creates a controller instance, it first calls **IDependencyResolver.GetService**, passing in the controller type.</span></span> <span data-ttu-id="f4ee9-143">Você pode usar esse gancho de extensibilidade para criar o controlador, resolvendo quaisquer dependências.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-143">You can use this extensibility hook to create the controller, resolving any dependencies.</span></span> <span data-ttu-id="f4ee9-144">Se **GetService** retornar NULL, a API da Web procurará um construtor sem parâmetros na classe do controlador.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-144">If **GetService** returns null, Web API looks for a parameterless constructor on the controller class.</span></span>

## <a name="dependency-resolution-with-the-unity-container"></a><span data-ttu-id="f4ee9-145">Resolução de dependência com o contêiner do Unity</span><span class="sxs-lookup"><span data-stu-id="f4ee9-145">Dependency Resolution with the Unity Container</span></span>

<span data-ttu-id="f4ee9-146">Embora você possa escrever uma implementação de **IDependencyResolver** completa do zero, a interface é realmente projetada para atuar como ponte entre a API da Web e os contêineres IOC existentes.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-146">Although you could write a complete **IDependencyResolver** implementation from scratch, the interface is really designed to act as bridge between Web API and existing IoC containers.</span></span>

<span data-ttu-id="f4ee9-147">Um contêiner IoC é um componente de software que é responsável por gerenciar dependências.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-147">An IoC container is a software component that is responsible for managing dependencies.</span></span> <span data-ttu-id="f4ee9-148">Você registra os tipos com o contêiner e, em seguida, usa o contêiner para criar objetos.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-148">You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="f4ee9-149">O contêiner ilustra automaticamente as relações de dependência.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-149">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="f4ee9-150">Muitos contêineres IoC também permitem que você controle coisas como o tempo de vida e o escopo do objeto.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-150">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="f4ee9-151">"IoC" significa "inversão de controle", que é um padrão geral em que uma estrutura chama o código do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-151">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="f4ee9-152">Um contêiner IoC constrói seus objetos para você, que "inverte" o fluxo de controle usual.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-152">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>

<span data-ttu-id="f4ee9-153">Para este tutorial, usaremos o [Unity](https://msdn.microsoft.com/library/ff647202.aspx) da Microsoft patterns &amp; Practices.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-153">For this tutorial, we'll use [Unity](https://msdn.microsoft.com/library/ff647202.aspx) from Microsoft Patterns &amp; Practices.</span></span> <span data-ttu-id="f4ee9-154">(Outras bibliotecas populares incluem [Castle Windsor](http://www.castleproject.org/), [Spring.net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/)e [StructureMap](http://structuremap.github.io/documentation/).) Você pode usar o Gerenciador de pacotes NuGet para instalar o Unity.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-154">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), and [StructureMap](http://structuremap.github.io/documentation/).) You can use NuGet Package Manager to install Unity.</span></span> <span data-ttu-id="f4ee9-155">No menu **ferramentas** no Visual Studio, selecione **Gerenciador de pacotes NuGet**e, em seguida, selecione **console do Gerenciador de pacotes**.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-155">From the **Tools** menu in Visual Studio, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="f4ee9-156">Na janela do console do Gerenciador de pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="f4ee9-156">In the Package Manager Console window, type the following command:</span></span>

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

<span data-ttu-id="f4ee9-157">Aqui está uma implementação de **IDependencyResolver** que encapsula um contêiner do Unity.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-157">Here is an implementation of **IDependencyResolver** that wraps a Unity container.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="f4ee9-158">Se o método **GetService** não puder resolver um tipo, ele deverá retornar **NULL**.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-158">If the **GetService** method cannot resolve a type, it should return **null**.</span></span> <span data-ttu-id="f4ee9-159">Se o método **GetServices** não puder resolver um tipo, ele deverá retornar um objeto de coleção Vazio.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-159">If the **GetServices** method cannot resolve a type, it should return an empty collection object.</span></span> <span data-ttu-id="f4ee9-160">Não gerar exceções para tipos desconhecidos.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-160">Don't throw exceptions for unknown types.</span></span>

## <a name="configuring-the-dependency-resolver"></a><span data-ttu-id="f4ee9-161">Configurando o resolvedor de dependência</span><span class="sxs-lookup"><span data-stu-id="f4ee9-161">Configuring the Dependency Resolver</span></span>

<span data-ttu-id="f4ee9-162">Defina o resolvedor de dependência na propriedade **DependencyResolver** do objeto **HttpConfiguration** global.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-162">Set the dependency resolver on the **DependencyResolver** property of the global **HttpConfiguration** object.</span></span>

<span data-ttu-id="f4ee9-163">O código a seguir registra a interface `IProductRepository` com o Unity e, em seguida, cria uma `UnityResolver`.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-163">The following code registers the `IProductRepository` interface with Unity and then creates a `UnityResolver`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a><span data-ttu-id="f4ee9-164">Tempo de vida do controlador e do escopo de dependência</span><span class="sxs-lookup"><span data-stu-id="f4ee9-164">Dependency Scope and Controller Lifetime</span></span>

<span data-ttu-id="f4ee9-165">Os controladores são criados por solicitação.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-165">Controllers are created per request.</span></span> <span data-ttu-id="f4ee9-166">Para gerenciar tempos de vida de objeto, **IDependencyResolver** usa o conceito de um *escopo*.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-166">To manage object lifetimes, **IDependencyResolver** uses the concept of a *scope*.</span></span>

<span data-ttu-id="f4ee9-167">O resolvedor de dependência anexado ao objeto **HttpConfiguration** tem escopo global.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-167">The dependency resolver attached to the **HttpConfiguration** object has global scope.</span></span> <span data-ttu-id="f4ee9-168">Quando a API da Web cria um controlador, ele chama **BeginScope**.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-168">When Web API creates a controller, it calls **BeginScope**.</span></span> <span data-ttu-id="f4ee9-169">Esse método retorna um **IDependencyScope** que representa um escopo filho.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-169">This method returns an **IDependencyScope** that represents a child scope.</span></span>

<span data-ttu-id="f4ee9-170">A API Web então chama **GetService** no escopo filho para criar o controlador.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-170">Web API then calls **GetService** on the child scope to create the controller.</span></span> <span data-ttu-id="f4ee9-171">Quando a solicitação for concluída, as chamadas da API Web serão **descartadas** no escopo filho.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-171">When request is complete, Web API calls **Dispose** on the child scope.</span></span> <span data-ttu-id="f4ee9-172">Use o método **Dispose** para descartar as dependências do controlador.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-172">Use the **Dispose** method to dispose of the controller's dependencies.</span></span>

<span data-ttu-id="f4ee9-173">A maneira como você implementa o **BeginScope** depende do contêiner IoC.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-173">How you implement **BeginScope** depends on the IoC container.</span></span> <span data-ttu-id="f4ee9-174">Para Unity, o escopo corresponde a um contêiner filho:</span><span class="sxs-lookup"><span data-stu-id="f4ee9-174">For Unity, scope corresponds to a child container:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

<span data-ttu-id="f4ee9-175">A maioria dos contêineres IoC tem equivalentes semelhantes.</span><span class="sxs-lookup"><span data-stu-id="f4ee9-175">Most IoC containers have similar equivalents.</span></span>
