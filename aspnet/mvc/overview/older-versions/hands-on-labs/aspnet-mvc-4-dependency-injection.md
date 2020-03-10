---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: Injeção de dependência do MVC 4 do ASP.NET | Microsoft Docs
author: rick-anderson
description: 'Observação: este laboratório prático pressupõe que você tenha conhecimento básico dos filtros ASP.NET MVC e ASP.NET MVC 4. Se você não usou os filtros do ASP.NET MVC 4 antes, nós...'
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 15c9d4dcb9e2c6b9f6adf54d65d15737b32cca3b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78560609"
---
# <a name="aspnet-mvc-4-dependency-injection"></a><span data-ttu-id="bafca-104">Injeção de dependência do ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="bafca-104">ASP.NET MVC 4 Dependency Injection</span></span>

<span data-ttu-id="bafca-105">por [equipe de acampamentos da Web](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="bafca-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="bafca-106">Baixe o kit de treinamento do Web acampamentos</span><span class="sxs-lookup"><span data-stu-id="bafca-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="bafca-107">Este laboratório prático pressupõe que você tenha conhecimento básico dos filtros **ASP.NET MVC** e **ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="bafca-107">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC** and **ASP.NET MVC 4 filters**.</span></span> <span data-ttu-id="bafca-108">Se você não usou os **filtros do ASP.NET MVC 4** antes, recomendamos que você vá para o laboratório prático de **filtros de ação personalizada do ASP.NET MVC** .</span><span class="sxs-lookup"><span data-stu-id="bafca-108">If you have not used **ASP.NET MVC 4 filters** before, we recommend you to go over **ASP.NET MVC Custom Action Filters** Hands-on Lab.</span></span>

> [!NOTE]
> <span data-ttu-id="bafca-109">Todos os códigos de exemplo e trechos de código estão incluídos no Web acampamentos Training Kit, disponível em [Microsoft-Web/WebCampTrainingKit releases](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="bafca-109">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="bafca-110">O projeto específico deste laboratório está disponível na [injeção de dependência ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="bafca-110">The project specific to this lab is available at [ASP.NET MVC 4 Dependency Injection](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).</span></span>

<span data-ttu-id="bafca-111">No paradigma de **programação orientada a objeto** , os objetos trabalham juntos em um modelo de colaboração onde há colaboradores e consumidores.</span><span class="sxs-lookup"><span data-stu-id="bafca-111">In **Object Oriented Programming** paradigm, objects work together in a collaboration model where there are contributors and consumers.</span></span> <span data-ttu-id="bafca-112">Naturalmente, esse modelo de comunicação gera dependências entre objetos e componentes, tornando-se difícil de gerenciar quando a complexidade aumenta.</span><span class="sxs-lookup"><span data-stu-id="bafca-112">Naturally, this communication model generates dependencies between objects and components, becoming difficult to manage when complexity increases.</span></span>

<span data-ttu-id="bafca-113">![Dependências de classe e complexidade do modelo](aspnet-mvc-4-dependency-injection/_static/image1.png "Dependências de classe e complexidade do modelo")</span><span class="sxs-lookup"><span data-stu-id="bafca-113">![Class dependencies and model complexity](aspnet-mvc-4-dependency-injection/_static/image1.png "Class dependencies and model complexity")</span></span>

<span data-ttu-id="bafca-114">*Dependências de classe e complexidade do modelo*</span><span class="sxs-lookup"><span data-stu-id="bafca-114">*Class dependencies and model complexity*</span></span>

<span data-ttu-id="bafca-115">Você provavelmente já ouviu falar do **padrão de fábrica** e da separação entre a interface e a implementação usando serviços, em que os objetos de cliente são frequentemente responsáveis pelo local do serviço.</span><span class="sxs-lookup"><span data-stu-id="bafca-115">You have probably heard about the **Factory Pattern** and the separation between the interface and the implementation using services, where the client objects are often responsible for service location.</span></span>

<span data-ttu-id="bafca-116">O padrão de injeção de dependência é uma implementação específica de inversão de controle.</span><span class="sxs-lookup"><span data-stu-id="bafca-116">The Dependency Injection pattern is a particular implementation of Inversion of Control.</span></span> <span data-ttu-id="bafca-117">A **inversão de controle (IOC)** significa que os objetos não criam outros objetos nos quais eles dependem para realizar seu trabalho.</span><span class="sxs-lookup"><span data-stu-id="bafca-117">**Inversion of Control (IoC)** means that objects do not create other objects on which they rely to do their work.</span></span> <span data-ttu-id="bafca-118">Em vez disso, eles obtêm os objetos de que precisam de uma fonte externa (por exemplo, um arquivo de configuração XML).</span><span class="sxs-lookup"><span data-stu-id="bafca-118">Instead, they get the objects that they need from an outside source (for example, an xml configuration file).</span></span>

<span data-ttu-id="bafca-119">**Injeção de dependência (di)** significa que isso é feito sem a intervenção do objeto, geralmente por um componente de estrutura que passa parâmetros de construtor e define propriedades.</span><span class="sxs-lookup"><span data-stu-id="bafca-119">**Dependency Injection (DI)** means that this is done without the object intervention, usually by a framework component that passes constructor parameters and set properties.</span></span>

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a><span data-ttu-id="bafca-120">O padrão de design de injeção de dependência (DI)</span><span class="sxs-lookup"><span data-stu-id="bafca-120">The Dependency Injection (DI) Design Pattern</span></span>

<span data-ttu-id="bafca-121">Em um alto nível, o objetivo da injeção de dependência é que uma classe de cliente (por exemplo, *o jogador*) precisa de algo que satisfaça uma interface (por exemplo, *IClub*).</span><span class="sxs-lookup"><span data-stu-id="bafca-121">At a high level, the goal of Dependency Injection is that a client class (e.g. *the golfer*) needs something that satisfies an interface (e.g. *IClub*).</span></span> <span data-ttu-id="bafca-122">Não importa qual é o tipo concreto (por exemplo *, WoodClub, IronClub, WedgeClub* ou *PutterClub*), quer que outra pessoa manipule isso (por exemplo, um bom *Caddy*).</span><span class="sxs-lookup"><span data-stu-id="bafca-122">It doesn't care what the concrete type is (e.g. *WoodClub, IronClub, WedgeClub* or *PutterClub*), it wants someone else to handle that (e.g. a good *caddy*).</span></span> <span data-ttu-id="bafca-123">O resolvedor de dependência no MVC ASP.NET pode permitir que você registre sua lógica de dependência em outro lugar (por exemplo, um contêiner ou um recipiente *de paus*).</span><span class="sxs-lookup"><span data-stu-id="bafca-123">The Dependency Resolver in ASP.NET MVC can allow you to register your dependency logic somewhere else (e.g. a container or a *bag of clubs*).</span></span>

<span data-ttu-id="bafca-124">![Diagrama de injeção de dependência](aspnet-mvc-4-dependency-injection/_static/image2.png "Ilustração de injeção de dependência")</span><span class="sxs-lookup"><span data-stu-id="bafca-124">![Dependency Injection diagram](aspnet-mvc-4-dependency-injection/_static/image2.png "Dependency Injection illustration")</span></span>

<span data-ttu-id="bafca-125">*Injeção de dependência-analogia de golfe*</span><span class="sxs-lookup"><span data-stu-id="bafca-125">*Dependency Injection - Golf analogy*</span></span>

<span data-ttu-id="bafca-126">As vantagens de usar o padrão de injeção de dependência e a inversão de controle são as seguintes:</span><span class="sxs-lookup"><span data-stu-id="bafca-126">The advantages of using Dependency Injection pattern and Inversion of Control are the following:</span></span>

- <span data-ttu-id="bafca-127">Reduz o acoplamento de classe</span><span class="sxs-lookup"><span data-stu-id="bafca-127">Reduces class coupling</span></span>
- <span data-ttu-id="bafca-128">Aumenta a reutilização de código</span><span class="sxs-lookup"><span data-stu-id="bafca-128">Increases code reusing</span></span>
- <span data-ttu-id="bafca-129">Melhora a manutenção do código</span><span class="sxs-lookup"><span data-stu-id="bafca-129">Improves code maintainability</span></span>
- <span data-ttu-id="bafca-130">Melhora o teste de aplicativos</span><span class="sxs-lookup"><span data-stu-id="bafca-130">Improves application testing</span></span>

> [!NOTE]
> <span data-ttu-id="bafca-131">A injeção de dependência às vezes é comparada com o padrão de design de fábrica abstrato, mas há uma pequena diferença entre ambas as abordagens.</span><span class="sxs-lookup"><span data-stu-id="bafca-131">Dependency Injection is sometimes compared with Abstract Factory Design Pattern, but there is a slight difference between both approaches.</span></span> <span data-ttu-id="bafca-132">DI tem uma estrutura que está trabalhando para resolver dependências chamando as fábricas e os serviços registrados.</span><span class="sxs-lookup"><span data-stu-id="bafca-132">DI has a Framework working behind to solve dependencies by calling the factories and the registered services.</span></span>

<span data-ttu-id="bafca-133">Agora que você entende o padrão de injeção de dependência, aprenderá em todo este laboratório como aplicá-lo no ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="bafca-133">Now that you understand the Dependency Injection Pattern, you will learn throughout this lab how to apply it in ASP.NET MVC 4.</span></span> <span data-ttu-id="bafca-134">Você começará a usar a injeção de dependência nos **controladores** para incluir um serviço de acesso ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="bafca-134">You will start using Dependency Injection in the **Controllers** to include a database access service.</span></span> <span data-ttu-id="bafca-135">Em seguida, você aplicará injeção de dependência às **exibições** para consumir um serviço e mostrar informações.</span><span class="sxs-lookup"><span data-stu-id="bafca-135">Next, you will apply Dependency Injection to the **Views** to consume a service and show information.</span></span> <span data-ttu-id="bafca-136">Por fim, você estenderá os filtros DI para ASP.NET MVC 4, injetando um filtro de ação personalizado na solução.</span><span class="sxs-lookup"><span data-stu-id="bafca-136">Finally, you will extend the DI to ASP.NET MVC 4 Filters, injecting a custom action filter in the solution.</span></span>

<span data-ttu-id="bafca-137">Neste laboratório prático, você aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="bafca-137">In this Hands-on Lab, you will learn how to:</span></span>

- <span data-ttu-id="bafca-138">Integrar o ASP.NET MVC 4 ao Unity para injeção de dependência usando pacotes NuGet</span><span class="sxs-lookup"><span data-stu-id="bafca-138">Integrate ASP.NET MVC 4 with Unity for Dependency Injection using NuGet Packages</span></span>
- <span data-ttu-id="bafca-139">Usar injeção de dependência dentro de um controlador MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="bafca-139">Use Dependency Injection inside an ASP.NET MVC Controller</span></span>
- <span data-ttu-id="bafca-140">Usar injeção de dependência dentro de uma exibição do ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="bafca-140">Use Dependency Injection inside an ASP.NET MVC View</span></span>
- <span data-ttu-id="bafca-141">Usar injeção de dependência dentro de um filtro de ação do ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="bafca-141">Use Dependency Injection inside an ASP.NET MVC Action Filter</span></span>

> [!NOTE]
> <span data-ttu-id="bafca-142">Este laboratório está usando o pacote NuGet do Unity. Mvc3 para resolução de dependência, mas é possível adaptar qualquer estrutura de injeção de dependência para trabalhar com o ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="bafca-142">This Lab is using Unity.Mvc3 NuGet Package for dependency resolution, but it is possible to adapt any Dependency Injection Framework to work with ASP.NET MVC 4.</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="bafca-143">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="bafca-143">Prerequisites</span></span>

<span data-ttu-id="bafca-144">Você deve ter os seguintes itens para concluir este laboratório:</span><span class="sxs-lookup"><span data-stu-id="bafca-144">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="bafca-145">[Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou superior (leia [o apêndice a](#AppendixA) para obter instruções sobre como instalá-lo).</span><span class="sxs-lookup"><span data-stu-id="bafca-145">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="bafca-146">Instalação</span><span class="sxs-lookup"><span data-stu-id="bafca-146">Setup</span></span>

<span data-ttu-id="bafca-147">**Instalando trechos de código**</span><span class="sxs-lookup"><span data-stu-id="bafca-147">**Installing Code Snippets**</span></span>

<span data-ttu-id="bafca-148">Para sua conveniência, grande parte do código que você gerenciará ao longo deste laboratório está disponível como trechos de código do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bafca-148">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="bafca-149">Para instalar os trechos de código, execute o arquivo **.\Source\Setup\CodeSnippets.VSI** .</span><span class="sxs-lookup"><span data-stu-id="bafca-149">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="bafca-150">Se você não estiver familiarizado com os trechos de Visual Studio Code e quiser saber como usá-los, consulte o apêndice deste documento &quot;[Apêndice B: usando trechos de código](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="bafca-150">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="bafca-151">Exercícios</span><span class="sxs-lookup"><span data-stu-id="bafca-151">Exercises</span></span>

<span data-ttu-id="bafca-152">Este laboratório prático é composto pelos seguintes exercícios:</span><span class="sxs-lookup"><span data-stu-id="bafca-152">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="bafca-153">Exercício 1: injetando um controlador</span><span class="sxs-lookup"><span data-stu-id="bafca-153">Exercise 1: Injecting a Controller</span></span>](#Exercise1)
2. [<span data-ttu-id="bafca-154">Exercício 2: injetando uma exibição</span><span class="sxs-lookup"><span data-stu-id="bafca-154">Exercise 2: Injecting a View</span></span>](#Exercise2)
3. [<span data-ttu-id="bafca-155">Exercício 3: injetando filtros</span><span class="sxs-lookup"><span data-stu-id="bafca-155">Exercise 3: Injecting Filters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="bafca-156">Cada exercício é acompanhado por uma pasta **final** que contém a solução resultante que você deve obter depois de concluir os exercícios.</span><span class="sxs-lookup"><span data-stu-id="bafca-156">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="bafca-157">Você pode usar essa solução como um guia se precisar de ajuda adicional para trabalhar com os exercícios.</span><span class="sxs-lookup"><span data-stu-id="bafca-157">You can use this solution as a guide if you need additional help working through the exercises.</span></span>

<span data-ttu-id="bafca-158">Tempo estimado para concluir este laboratório: **30 minutos**.</span><span class="sxs-lookup"><span data-stu-id="bafca-158">Estimated time to complete this lab: **30 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a><span data-ttu-id="bafca-159">Exercício 1: injetando um controlador</span><span class="sxs-lookup"><span data-stu-id="bafca-159">Exercise 1: Injecting a Controller</span></span>

<span data-ttu-id="bafca-160">Neste exercício, você aprenderá a usar a injeção de dependência em controladores MVC do ASP.NET integrando o Unity usando um pacote NuGet.</span><span class="sxs-lookup"><span data-stu-id="bafca-160">In this exercise, you will learn how to use Dependency Injection in ASP.NET MVC Controllers by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="bafca-161">Por esse motivo, você incluirá serviços em seus controladores MvcMusicStore para separar a lógica do acesso a dados.</span><span class="sxs-lookup"><span data-stu-id="bafca-161">For that reason, you will include services into your MvcMusicStore controllers to separate the logic from the data access.</span></span> <span data-ttu-id="bafca-162">Os serviços criarão uma nova dependência no construtor do controlador, que será resolvida usando injeção de dependência com a ajuda do **Unity**.</span><span class="sxs-lookup"><span data-stu-id="bafca-162">The services will create a new dependency in the controller constructor, which will be resolved using Dependency Injection with the help of **Unity**.</span></span>

<span data-ttu-id="bafca-163">Essa abordagem lhe mostrará como gerar aplicativos menos acoplados, que são mais flexíveis e fáceis de manter e testar.</span><span class="sxs-lookup"><span data-stu-id="bafca-163">This approach will show you how to generate less coupled applications, which are more flexible and easier to maintain and test.</span></span> <span data-ttu-id="bafca-164">Você também aprenderá a integrar o ASP.NET MVC ao Unity.</span><span class="sxs-lookup"><span data-stu-id="bafca-164">You will also learn how to integrate ASP.NET MVC with Unity.</span></span>

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a><span data-ttu-id="bafca-165">Sobre o serviço Storemanager</span><span class="sxs-lookup"><span data-stu-id="bafca-165">About StoreManager Service</span></span>

<span data-ttu-id="bafca-166">A loja de música MVC fornecida na solução inicial agora inclui um serviço que gerencia os dados do controlador de armazenamento denominado **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="bafca-166">The MVC Music Store provided in the begin solution now includes a service that manages the Store Controller data named **StoreService**.</span></span> <span data-ttu-id="bafca-167">Abaixo, você encontrará a implementação do serviço de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="bafca-167">Below you will find the Store Service implementation.</span></span> <span data-ttu-id="bafca-168">Observe que todos os métodos retornam entidades de modelo.</span><span class="sxs-lookup"><span data-stu-id="bafca-168">Note that all the methods return Model entities.</span></span>

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

<span data-ttu-id="bafca-169">O **StoreController** da solução inicial agora consome **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="bafca-169">**StoreController** from the begin solution now consumes **StoreService**.</span></span> <span data-ttu-id="bafca-170">Todas as referências de dados foram removidas do **StoreController**e agora é possível modificar o provedor de acesso a dados atual sem alterar nenhum método que consuma **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="bafca-170">All the data references were removed from **StoreController**, and now possible to modify the current data access provider without changing any method that consumes **StoreService**.</span></span>

<span data-ttu-id="bafca-171">Você verá abaixo que a implementação de **StoreController** tem uma dependência com **StoreService** dentro do construtor de classe.</span><span class="sxs-lookup"><span data-stu-id="bafca-171">You will find below that the **StoreController** implementation has a dependency with **StoreService** inside the class constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="bafca-172">A dependência introduzida neste exercício está relacionada à **inversão de controle** (IOC).</span><span class="sxs-lookup"><span data-stu-id="bafca-172">The dependency introduced in this exercise is related to **Inversion of Control** (IoC).</span></span>
> 
> <span data-ttu-id="bafca-173">O construtor da classe **StoreController** recebe um parâmetro de tipo **IStoreService** , que é essencial para executar chamadas de serviço de dentro da classe.</span><span class="sxs-lookup"><span data-stu-id="bafca-173">The **StoreController** class constructor receives an **IStoreService** type parameter, which is essential to perform service calls from inside the class.</span></span> <span data-ttu-id="bafca-174">No entanto, o **StoreController** não implementa o construtor padrão (sem parâmetros) que qualquer controlador deve ter para trabalhar com o MVC do ASP.net.</span><span class="sxs-lookup"><span data-stu-id="bafca-174">However, **StoreController** does not implement the default constructor (with no parameters) that any controller must have to work with ASP.NET MVC.</span></span>
> 
> <span data-ttu-id="bafca-175">Para resolver a dependência, o controlador deve ser criado por uma fábrica abstrata (uma classe que retorna qualquer objeto do tipo especificado).</span><span class="sxs-lookup"><span data-stu-id="bafca-175">To resolve the dependency, the controller has to be created by an abstract factory (a class that returns any object of the specified type).</span></span>

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="bafca-176">Você receberá um erro quando a classe tentar criar o StoreController sem enviar o objeto de serviço, já que não há nenhum construtor com parâmetros declarado.</span><span class="sxs-lookup"><span data-stu-id="bafca-176">You will get an error when the class tries to create the StoreController without sending the service object, as there is no parameterless constructor declared.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a><span data-ttu-id="bafca-177">Tarefa 1-executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="bafca-177">Task 1 - Running the Application</span></span>

<span data-ttu-id="bafca-178">Nesta tarefa, você executará o aplicativo Begin, que inclui o serviço no controlador da loja que separa o acesso a dados da lógica do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="bafca-178">In this task, you will run the Begin application, which includes the service into the Store Controller that separates the data access from the application logic.</span></span>

<span data-ttu-id="bafca-179">Ao executar o aplicativo, você receberá uma exceção, pois o serviço do controlador não será passado como um parâmetro por padrão:</span><span class="sxs-lookup"><span data-stu-id="bafca-179">When running the application, you will receive an exception, as the controller service is not passed as a parameter by default:</span></span>

1. <span data-ttu-id="bafca-180">Abra a solução **inicial** localizada em **Source\Ex01-Injecting Controller\Begin**.</span><span class="sxs-lookup"><span data-stu-id="bafca-180">Open the **Begin** solution located in **Source\Ex01-Injecting Controller\Begin**.</span></span>

   1. <span data-ttu-id="bafca-181">Será necessário baixar alguns pacotes NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="bafca-181">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="bafca-182">Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="bafca-182">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="bafca-183">Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="bafca-183">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="bafca-184">Por fim, Compile a solução clicando em **build** | **Compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="bafca-184">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="bafca-185">Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="bafca-185">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="bafca-186">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="bafca-186">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="bafca-187">É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.</span><span class="sxs-lookup"><span data-stu-id="bafca-187">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="bafca-188">Pressione **Ctrl + F5** para executar o aplicativo sem depuração.</span><span class="sxs-lookup"><span data-stu-id="bafca-188">Press **Ctrl + F5** to run the application without debugging.</span></span> <span data-ttu-id="bafca-189">Você receberá a mensagem de erro &quot;**nenhum construtor sem parâmetros definido para este objeto**&quot;:</span><span class="sxs-lookup"><span data-stu-id="bafca-189">You will get the error message &quot;**No parameterless constructor defined for this object**&quot;:</span></span>

    <span data-ttu-id="bafca-190">![Erro ao executar o aplicativo de início do ASP.NET MVC](aspnet-mvc-4-dependency-injection/_static/image3.png "Erro ao executar o aplicativo de início do ASP.NET MVC")</span><span class="sxs-lookup"><span data-stu-id="bafca-190">![Error while running ASP.NET MVC Begin Application](aspnet-mvc-4-dependency-injection/_static/image3.png "Error while running ASP.NET MVC Begin Application")</span></span>

    <span data-ttu-id="bafca-191">*Erro ao executar o aplicativo de início do ASP.NET MVC*</span><span class="sxs-lookup"><span data-stu-id="bafca-191">*Error while running ASP.NET MVC Begin Application*</span></span>
3. <span data-ttu-id="bafca-192">Feche o navegador.</span><span class="sxs-lookup"><span data-stu-id="bafca-192">Close the browser.</span></span>

<span data-ttu-id="bafca-193">Nas etapas a seguir, você trabalhará na solução de loja de música para injetar a dependência que esse controlador precisa.</span><span class="sxs-lookup"><span data-stu-id="bafca-193">In the following steps you will work on the Music Store Solution to inject the dependency this controller needs.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a><span data-ttu-id="bafca-194">Tarefa 2-incluindo o Unity na solução MvcMusicStore</span><span class="sxs-lookup"><span data-stu-id="bafca-194">Task 2 - Including Unity into MvcMusicStore Solution</span></span>

<span data-ttu-id="bafca-195">Nesta tarefa, você incluirá o pacote NuGet do **Unity. Mvc3** para a solução.</span><span class="sxs-lookup"><span data-stu-id="bafca-195">In this task, you will include **Unity.Mvc3** NuGet Package to the solution.</span></span>

> [!NOTE]
> <span data-ttu-id="bafca-196">O pacote Unity. Mvc3 foi projetado para o ASP.NET MVC 3, mas é totalmente compatível com o ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="bafca-196">Unity.Mvc3 package was designed for ASP.NET MVC 3, but it is fully compatible with ASP.NET MVC 4.</span></span>
> 
> <span data-ttu-id="bafca-197">O Unity é um contêiner de injeção de dependência simples e extensível com suporte opcional para a interceptação de instância e tipo.</span><span class="sxs-lookup"><span data-stu-id="bafca-197">Unity is a lightweight, extensible dependency injection container with optional support for instance and type interception.</span></span> <span data-ttu-id="bafca-198">É um contêiner de finalidade geral para uso em qualquer tipo de aplicativo .NET.</span><span class="sxs-lookup"><span data-stu-id="bafca-198">It is a general-purpose container for use in any type of .NET application.</span></span> <span data-ttu-id="bafca-199">Ele fornece todos os recursos comuns encontrados em mecanismos de injeção de dependência, incluindo: criação de objeto, abstração de requisitos, especificando dependências em tempo de execução e flexibilidade, adiando a configuração do componente para o contêiner.</span><span class="sxs-lookup"><span data-stu-id="bafca-199">It provides all the common features found in dependency injection mechanisms including: object creation, abstraction of requirements by specifying dependencies at runtime and flexibility, by deferring the component configuration to the container.</span></span>

1. <span data-ttu-id="bafca-200">Instale o pacote NuGet do **Unity. Mvc3** no projeto **MvcMusicStore** .</span><span class="sxs-lookup"><span data-stu-id="bafca-200">Install **Unity.Mvc3** NuGet Package in the **MvcMusicStore** project.</span></span> <span data-ttu-id="bafca-201">Para fazer isso, abra o **console do Gerenciador de pacotes** do **modo de exibição** | **outras janelas**.</span><span class="sxs-lookup"><span data-stu-id="bafca-201">To do this, open the **Package Manager Console** from **View** | **Other Windows**.</span></span>
2. <span data-ttu-id="bafca-202">Execute o comando a seguir.</span><span class="sxs-lookup"><span data-stu-id="bafca-202">Run the following command.</span></span>

    <span data-ttu-id="bafca-203">PMC</span><span class="sxs-lookup"><span data-stu-id="bafca-203">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    <span data-ttu-id="bafca-204">![Instalando o pacote NuGet do Unity. Mvc3](aspnet-mvc-4-dependency-injection/_static/image4.png "Instalando o pacote NuGet do Unity. Mvc3")</span><span class="sxs-lookup"><span data-stu-id="bafca-204">![Installing Unity.Mvc3 NuGet Package](aspnet-mvc-4-dependency-injection/_static/image4.png "Installing Unity.Mvc3 NuGet Package")</span></span>

    <span data-ttu-id="bafca-205">*Instalando o pacote NuGet do Unity. Mvc3*</span><span class="sxs-lookup"><span data-stu-id="bafca-205">*Installing Unity.Mvc3 NuGet Package*</span></span>
3. <span data-ttu-id="bafca-206">Depois que o pacote **Unity. Mvc3** for instalado, explore os arquivos e as pastas que ele adiciona automaticamente para simplificar a configuração do Unity.</span><span class="sxs-lookup"><span data-stu-id="bafca-206">Once the **Unity.Mvc3** package is installed, explore the files and folders it automatically adds in order to simplify Unity configuration.</span></span>

    <span data-ttu-id="bafca-207">![Pacote do Unity. Mvc3 instalado](aspnet-mvc-4-dependency-injection/_static/image5.png "Pacote do Unity. Mvc3 instalado")</span><span class="sxs-lookup"><span data-stu-id="bafca-207">![Unity.Mvc3 package installed](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 package installed")</span></span>

    <span data-ttu-id="bafca-208">*Pacote do Unity. Mvc3 instalado*</span><span class="sxs-lookup"><span data-stu-id="bafca-208">*Unity.Mvc3 package installed*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-application_start"></a><span data-ttu-id="bafca-209">Tarefa 3-registrando o Unity no aplicativo Global.asax.cs\_iniciar</span><span class="sxs-lookup"><span data-stu-id="bafca-209">Task 3 - Registering Unity in Global.asax.cs Application\_Start</span></span>

<span data-ttu-id="bafca-210">Nesta tarefa, você atualizará o método **Start do aplicativo\_** localizado em **global.asax.cs** para chamar o inicializador do bootstrapper do Unity e, em seguida, atualizará o arquivo bootstrapper registrando o serviço e o controlador que será usado para injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="bafca-210">In this task, you will update the **Application\_Start** method located in **Global.asax.cs** to call the Unity Bootstrapper initializer and then, update the Bootstrapper file registering the Service and Controller you will use for Dependency Injection.</span></span>

1. <span data-ttu-id="bafca-211">Agora, você conectará o bootstrapper, que é o arquivo que inicializa o contêiner do Unity e o resolvedor de dependência.</span><span class="sxs-lookup"><span data-stu-id="bafca-211">Now, you will hook up the Bootstrapper which is the file that initializes the Unity container and Dependency Resolver.</span></span> <span data-ttu-id="bafca-212">Para fazer isso, abra **global.asax.cs** e adicione o seguinte código realçado dentro do método de **início do aplicativo\_** .</span><span class="sxs-lookup"><span data-stu-id="bafca-212">To do this, open **Global.asax.cs** and add the following highlighted code within the **Application\_Start** method.</span></span>

    <span data-ttu-id="bafca-213">(Trecho de código – *laboratório de injeção de dependência ASP.net-Ex01-Initialize Unity*)</span><span class="sxs-lookup"><span data-stu-id="bafca-213">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Initialize Unity*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. <span data-ttu-id="bafca-214">Abra o arquivo **bootstrapper.cs** .</span><span class="sxs-lookup"><span data-stu-id="bafca-214">Open **Bootstrapper.cs** file.</span></span>
3. <span data-ttu-id="bafca-215">Inclua os seguintes namespaces: **MvcMusicStore. Services** e **MusicStore. Controllers**.</span><span class="sxs-lookup"><span data-stu-id="bafca-215">Include the following namespaces: **MvcMusicStore.Services** and **MusicStore.Controllers**.</span></span>

    <span data-ttu-id="bafca-216">(Trecho de código- *laboratório de injeção de dependência ASP.net-Ex01-bootstrapper Adicionando namespaces*)</span><span class="sxs-lookup"><span data-stu-id="bafca-216">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. <span data-ttu-id="bafca-217">Substitua o conteúdo do método **BuildUnityContainer** pelo código a seguir que registra o controlador de armazenamento e o serviço de repositório.</span><span class="sxs-lookup"><span data-stu-id="bafca-217">Replace **BuildUnityContainer** method's content with the following code that registers Store Controller and Store Service.</span></span>

    <span data-ttu-id="bafca-218">(Trecho de código – *laboratório de injeção de dependência ASP.net-Ex01-registrar controlador de armazenamento e serviço*)</span><span class="sxs-lookup"><span data-stu-id="bafca-218">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Register Store Controller and Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="bafca-219">Tarefa 4-executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="bafca-219">Task 4 - Running the Application</span></span>

<span data-ttu-id="bafca-220">Nesta tarefa, você executará o aplicativo para verificar se ele agora pode ser carregado depois de incluir o Unity.</span><span class="sxs-lookup"><span data-stu-id="bafca-220">In this task, you will run the application to verify that it can now be loaded after including Unity.</span></span>

1. <span data-ttu-id="bafca-221">Pressione **F5** para executar o aplicativo. agora, o aplicativo deve ser carregado sem mostrar nenhuma mensagem de erro.</span><span class="sxs-lookup"><span data-stu-id="bafca-221">Press **F5** to run the application, the application should now load without showing any error message.</span></span>

    <span data-ttu-id="bafca-222">![Executando o aplicativo com injeção de dependência](aspnet-mvc-4-dependency-injection/_static/image6.png "Executando o aplicativo com injeção de dependência")</span><span class="sxs-lookup"><span data-stu-id="bafca-222">![Running Application with Dependency Injection](aspnet-mvc-4-dependency-injection/_static/image6.png "Running Application with Dependency Injection")</span></span>

    <span data-ttu-id="bafca-223">*Executando o aplicativo com injeção de dependência*</span><span class="sxs-lookup"><span data-stu-id="bafca-223">*Running Application with Dependency Injection*</span></span>
2. <span data-ttu-id="bafca-224">Navegue até **/Store**.</span><span class="sxs-lookup"><span data-stu-id="bafca-224">Browse to **/Store**.</span></span> <span data-ttu-id="bafca-225">Isso invocará **StoreController**, que agora é criado usando o **Unity**.</span><span class="sxs-lookup"><span data-stu-id="bafca-225">This will invoke **StoreController**, which is now created using **Unity**.</span></span>

    <span data-ttu-id="bafca-226">![Loja de música MVC](aspnet-mvc-4-dependency-injection/_static/image7.png "Loja de música MVC")</span><span class="sxs-lookup"><span data-stu-id="bafca-226">![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")</span></span>

    <span data-ttu-id="bafca-227">*Loja de música MVC*</span><span class="sxs-lookup"><span data-stu-id="bafca-227">*MVC Music Store*</span></span>
3. <span data-ttu-id="bafca-228">Feche o navegador.</span><span class="sxs-lookup"><span data-stu-id="bafca-228">Close the browser.</span></span>

<span data-ttu-id="bafca-229">Nos exercícios a seguir, você aprenderá a estender o escopo de injeção de dependência para usá-lo dentro de exibições e filtros de ação do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="bafca-229">In the following exercises you will learn how to extend the Dependency Injection scope to use it inside ASP.NET MVC Views and Action Filters.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a><span data-ttu-id="bafca-230">Exercício 2: injetando uma exibição</span><span class="sxs-lookup"><span data-stu-id="bafca-230">Exercise 2: Injecting a View</span></span>

<span data-ttu-id="bafca-231">Neste exercício, você aprenderá a usar a injeção de dependência em uma exibição com os novos recursos do ASP.NET MVC 4 para integração com o Unity.</span><span class="sxs-lookup"><span data-stu-id="bafca-231">In this exercise, you will learn how to use Dependency Injection in a view with the new features of ASP.NET MVC 4 for Unity integration.</span></span> <span data-ttu-id="bafca-232">Para fazer isso, você chamará um serviço personalizado dentro da exibição de navegação da loja, que mostrará uma mensagem e uma imagem abaixo.</span><span class="sxs-lookup"><span data-stu-id="bafca-232">In order to do that, you will call a custom service inside the Store Browse View, which will show a message and an image below.</span></span>

<span data-ttu-id="bafca-233">Em seguida, você integrará o projeto ao Unity e criará um resolvedor de dependência personalizado para injetar as dependências.</span><span class="sxs-lookup"><span data-stu-id="bafca-233">Then, you will integrate the project with Unity and create a custom dependency resolver to inject the dependencies.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a><span data-ttu-id="bafca-234">Tarefa 1-criando uma exibição que consome um serviço</span><span class="sxs-lookup"><span data-stu-id="bafca-234">Task 1 - Creating a View that Consumes a Service</span></span>

<span data-ttu-id="bafca-235">Nesta tarefa, você criará uma exibição que executa uma chamada de serviço para gerar uma nova dependência.</span><span class="sxs-lookup"><span data-stu-id="bafca-235">In this task, you will create a view that performs a service call to generate a new dependency.</span></span> <span data-ttu-id="bafca-236">O serviço consiste em um serviço de mensagens simples incluído nesta solução.</span><span class="sxs-lookup"><span data-stu-id="bafca-236">The service consists in a simple messaging service included in this solution.</span></span>

1. <span data-ttu-id="bafca-237">Abra a solução **inicial** localizada na pasta **Source\Ex02-Injecting View\Begin**</span><span class="sxs-lookup"><span data-stu-id="bafca-237">Open the **Begin** solution located in the **Source\Ex02-Injecting View\Begin** folder.</span></span> <span data-ttu-id="bafca-238">Caso contrário, você pode continuar usando a solução **final** obtida concluindo o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="bafca-238">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="bafca-239">Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="bafca-239">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="bafca-240">Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="bafca-240">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="bafca-241">Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="bafca-241">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="bafca-242">Por fim, Compile a solução clicando em **build** | **Compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="bafca-242">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="bafca-243">Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="bafca-243">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="bafca-244">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="bafca-244">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="bafca-245">É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.</span><span class="sxs-lookup"><span data-stu-id="bafca-245">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="bafca-246">Para obter mais informações, consulte este artigo: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="bafca-246">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="bafca-247">Inclua as classes **MessageService.cs** e **IMessageService.cs** localizadas na pasta **\Assets de origem** em **/Services**.</span><span class="sxs-lookup"><span data-stu-id="bafca-247">Include the **MessageService.cs** and the **IMessageService.cs** classes located in the **Source \Assets** folder in **/Services**.</span></span> <span data-ttu-id="bafca-248">Para fazer isso, clique com o botão direito do mouse em **Serviços** pasta e selecione **Adicionar item existente**.</span><span class="sxs-lookup"><span data-stu-id="bafca-248">To do this, right-click **Services** folder and select **Add Existing Item**.</span></span> <span data-ttu-id="bafca-249">Navegue até o local dos arquivos e inclua-os.</span><span class="sxs-lookup"><span data-stu-id="bafca-249">Browse to the files' location and include them.</span></span>

    <span data-ttu-id="bafca-250">![Adicionando serviço de mensagens e interface de serviço](aspnet-mvc-4-dependency-injection/_static/image8.png "Adicionando serviço de mensagens e interface de serviço")</span><span class="sxs-lookup"><span data-stu-id="bafca-250">![Adding Message Service and Service Interface](aspnet-mvc-4-dependency-injection/_static/image8.png "Adding Message Service and Service Interface")</span></span>

    <span data-ttu-id="bafca-251">*Adicionando serviço de mensagens e interface de serviço*</span><span class="sxs-lookup"><span data-stu-id="bafca-251">*Adding Message Service and Service Interface*</span></span>

    > [!NOTE]
    > <span data-ttu-id="bafca-252">A interface **IMessageService** define duas propriedades implementadas pela classe **MessageService** .</span><span class="sxs-lookup"><span data-stu-id="bafca-252">The **IMessageService** interface defines two properties implemented by the **MessageService** class.</span></span> <span data-ttu-id="bafca-253">Essas propriedades-**Message** e **ImageUrl**-armazenam a mensagem e a URL da imagem a ser exibida.</span><span class="sxs-lookup"><span data-stu-id="bafca-253">These properties -**Message** and **ImageUrl**- store the message and the URL of the image to be displayed.</span></span>
3. <span data-ttu-id="bafca-254">Crie a pasta **/pages** na pasta raiz do projeto e, em seguida, adicione a classe existente **MyBasePage.cs** de **Source\Assets**.</span><span class="sxs-lookup"><span data-stu-id="bafca-254">Create the folder **/Pages** in the project's root folder, and then add the existing class **MyBasePage.cs** from **Source\Assets**.</span></span> <span data-ttu-id="bafca-255">A página de base da qual você herdará tem a seguinte estrutura.</span><span class="sxs-lookup"><span data-stu-id="bafca-255">The base page you will inherit from has the following structure.</span></span>

    <span data-ttu-id="bafca-256">![Pasta de páginas](aspnet-mvc-4-dependency-injection/_static/image9.png "Pasta Páginas")</span><span class="sxs-lookup"><span data-stu-id="bafca-256">![Pages folder](aspnet-mvc-4-dependency-injection/_static/image9.png "Pages folder")</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. <span data-ttu-id="bafca-257">Abra o modo de exibição **Browse. cshtml** na pasta **/views/Store** e torne-o herdado de **MyBasePage.cs**.</span><span class="sxs-lookup"><span data-stu-id="bafca-257">Open **Browse.cshtml** view from **/Views/Store** folder, and make it inherit from **MyBasePage.cs**.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. <span data-ttu-id="bafca-258">No modo de exibição de **navegação** , adicione uma chamada para **MessageService** para exibir uma imagem e uma mensagem recuperada pelo serviço.</span><span class="sxs-lookup"><span data-stu-id="bafca-258">In the **Browse** view, add a call to **MessageService** to display an image and a message retrieved by the service.</span></span>
   <span data-ttu-id="bafca-259">(C#)</span><span class="sxs-lookup"><span data-stu-id="bafca-259">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a><span data-ttu-id="bafca-260">Tarefa 2-incluindo um resolvedor de dependência personalizado e um ativador de página de exibição personalizada</span><span class="sxs-lookup"><span data-stu-id="bafca-260">Task 2 - Including a Custom Dependency Resolver and a Custom View Page Activator</span></span>

<span data-ttu-id="bafca-261">Na tarefa anterior, você inseriu uma nova dependência dentro de uma exibição para executar uma chamada de serviço dentro dela.</span><span class="sxs-lookup"><span data-stu-id="bafca-261">In the previous task, you injected a new dependency inside a view to perform a service call inside it.</span></span> <span data-ttu-id="bafca-262">Agora, você resolverá essa dependência implementando as interfaces de injeção de dependência ASP.NET MVC **IViewPageActivator** e **IDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="bafca-262">Now, you will resolve that dependency by implementing the ASP.NET MVC Dependency Injection interfaces **IViewPageActivator** and **IDependencyResolver**.</span></span> <span data-ttu-id="bafca-263">Você incluirá na solução uma implementação de **IDependencyResolver** que tratará da recuperação do serviço usando o Unity.</span><span class="sxs-lookup"><span data-stu-id="bafca-263">You will include in the solution an implementation of **IDependencyResolver** that will deal with the service retrieval by using Unity.</span></span> <span data-ttu-id="bafca-264">Em seguida, você incluirá outra implementação personalizada da interface **IViewPageActivator** , que resolverá a criação das exibições.</span><span class="sxs-lookup"><span data-stu-id="bafca-264">Then, you will include another custom implementation of **IViewPageActivator** interface that will solve the creation of the views.</span></span>

> [!NOTE]
> <span data-ttu-id="bafca-265">Desde o ASP.NET MVC 3, a implementação para injeção de dependência simplificou as interfaces para registrar serviços.</span><span class="sxs-lookup"><span data-stu-id="bafca-265">Since ASP.NET MVC 3, the implementation for Dependency Injection had simplified the interfaces to register services.</span></span> <span data-ttu-id="bafca-266">**IDependencyResolver** e **IViewPageActivator** fazem parte dos recursos do ASP.NET MVC 3 para injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="bafca-266">**IDependencyResolver** and **IViewPageActivator** are part of ASP.NET MVC 3 features for Dependency Injection.</span></span>
> 
> <span data-ttu-id="bafca-267">**-** A interface IDependencyResolver substitui a IMvcServiceLocator anterior.</span><span class="sxs-lookup"><span data-stu-id="bafca-267">**- IDependencyResolver** interface replaces the previous IMvcServiceLocator.</span></span> <span data-ttu-id="bafca-268">Os implementadores de IDependencyResolver devem retornar uma instância do serviço ou de uma coleção de serviços.</span><span class="sxs-lookup"><span data-stu-id="bafca-268">Implementers of IDependencyResolver must return an instance of the service or a service collection.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> <span data-ttu-id="bafca-269">**-A interface IViewPageActivator** fornece um controle mais refinado sobre como as páginas de exibição são instanciadas por meio da injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="bafca-269">**- IViewPageActivator** interface provides more fine-grained control over how view pages are instantiated via dependency injection.</span></span> <span data-ttu-id="bafca-270">As classes que implementam a interface **IViewPageActivator** podem criar instâncias de exibição usando informações de contexto.</span><span class="sxs-lookup"><span data-stu-id="bafca-270">The classes that implement **IViewPageActivator** interface can create view instances using context information.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]

1. <span data-ttu-id="bafca-271">Crie a pasta/**factories** na pasta raiz do projeto.</span><span class="sxs-lookup"><span data-stu-id="bafca-271">Create the /**Factories** folder in the project's root folder.</span></span>
2. <span data-ttu-id="bafca-272">Inclua **CustomViewPageActivator.cs** para sua solução da pasta **/sources/assets/** para **fábricas** .</span><span class="sxs-lookup"><span data-stu-id="bafca-272">Include **CustomViewPageActivator.cs** to your solution from **/Sources/Assets/** to **Factories** folder.</span></span> <span data-ttu-id="bafca-273">Para fazer isso, clique com o botão direito do mouse na pasta **/factories** , selecione **Add | Item existente** e, em seguida, selecione **CustomViewPageActivator.cs**.</span><span class="sxs-lookup"><span data-stu-id="bafca-273">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **CustomViewPageActivator.cs**.</span></span> <span data-ttu-id="bafca-274">Essa classe implementa a interface **IViewPageActivator** para manter o contêiner do Unity.</span><span class="sxs-lookup"><span data-stu-id="bafca-274">This class implements the **IViewPageActivator** interface to hold the Unity Container.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > <span data-ttu-id="bafca-275">**CustomViewPageActivator** é responsável por gerenciar a criação de uma exibição usando um contêiner do Unity.</span><span class="sxs-lookup"><span data-stu-id="bafca-275">**CustomViewPageActivator** is responsible for managing the creation of a view by using a Unity container.</span></span>
3. <span data-ttu-id="bafca-276">Inclua o arquivo **UnityDependencyResolver.cs** de **/sources/assets** para a pasta **/factories** .</span><span class="sxs-lookup"><span data-stu-id="bafca-276">Include **UnityDependencyResolver.cs** file from **/Sources/Assets** to **/Factories** folder.</span></span> <span data-ttu-id="bafca-277">Para fazer isso, clique com o botão direito do mouse na pasta **/factories** , selecione **Add | Item existente** e, em seguida, selecione arquivo **UnityDependencyResolver.cs** .</span><span class="sxs-lookup"><span data-stu-id="bafca-277">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **UnityDependencyResolver.cs** file.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > <span data-ttu-id="bafca-278">A classe **UnityDependencyResolver** é uma DependencyResolver personalizada para o Unity.</span><span class="sxs-lookup"><span data-stu-id="bafca-278">**UnityDependencyResolver** class is a custom DependencyResolver for Unity.</span></span> <span data-ttu-id="bafca-279">Quando um serviço não pode ser encontrado dentro do contêiner do Unity, o resolvedor de base é invocated.</span><span class="sxs-lookup"><span data-stu-id="bafca-279">When a service cannot be found inside the Unity container, the base resolver is invocated.</span></span>

<span data-ttu-id="bafca-280">Na tarefa a seguir, ambas as implementações serão registradas para permitir que o modelo saiba o local dos serviços e as exibições.</span><span class="sxs-lookup"><span data-stu-id="bafca-280">In the following task both implementations will be registered to let the model know the location of the services and the views.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a><span data-ttu-id="bafca-281">Tarefa 3-registrando para injeção de dependência no contêiner do Unity</span><span class="sxs-lookup"><span data-stu-id="bafca-281">Task 3 - Registering for Dependency Injection within Unity container</span></span>

<span data-ttu-id="bafca-282">Nesta tarefa, você colocará todas as coisas anteriores em conjunto para tornar o trabalho de injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="bafca-282">In this task, you will put all the previous things together to make Dependency Injection work.</span></span>

<span data-ttu-id="bafca-283">Até agora, sua solução tem os seguintes elementos:</span><span class="sxs-lookup"><span data-stu-id="bafca-283">Up to now your solution has the following elements:</span></span>

- <span data-ttu-id="bafca-284">Um modo de exibição de **navegação** que herda de **MyBaseClass** e consome **MessageService**.</span><span class="sxs-lookup"><span data-stu-id="bafca-284">A **Browse** View that inherits from **MyBaseClass** and consumes **MessageService**.</span></span>
- <span data-ttu-id="bafca-285">Uma classe intermediária-**MyBaseClass**-que tem injeção de dependência declarada para a interface de serviço.</span><span class="sxs-lookup"><span data-stu-id="bafca-285">An intermediate class -**MyBaseClass**- that has dependency injection declared for the service interface.</span></span>
- <span data-ttu-id="bafca-286">Um Service- **MessageService** -e sua interface **IMessageService**.</span><span class="sxs-lookup"><span data-stu-id="bafca-286">A service - **MessageService** - and its interface **IMessageService**.</span></span>
- <span data-ttu-id="bafca-287">Um resolvedor de dependência personalizado para o Unity- **UnityDependencyResolver** -que lida com a recuperação de serviço.</span><span class="sxs-lookup"><span data-stu-id="bafca-287">A custom dependency resolver for Unity - **UnityDependencyResolver** - that deals with service retrieval.</span></span>
- <span data-ttu-id="bafca-288">Uma exibição Page Activator- **CustomViewPageActivator** -que cria a página.</span><span class="sxs-lookup"><span data-stu-id="bafca-288">A View Page activator - **CustomViewPageActivator** - that creates the page.</span></span>

<span data-ttu-id="bafca-289">Para injetar o modo de exibição de **procura** , agora você registrará o resolvedor de dependência personalizado no contêiner do Unity.</span><span class="sxs-lookup"><span data-stu-id="bafca-289">To inject **Browse** View, you will now register the custom dependency resolver in the Unity container.</span></span>

1. <span data-ttu-id="bafca-290">Abra o arquivo **bootstrapper.cs** .</span><span class="sxs-lookup"><span data-stu-id="bafca-290">Open **Bootstrapper.cs** file.</span></span>
2. <span data-ttu-id="bafca-291">Registre uma instância do **MessageService** no contêiner do Unity para inicializar o serviço:</span><span class="sxs-lookup"><span data-stu-id="bafca-291">Register an instance of **MessageService** into the Unity container to initialize the service:</span></span>

    <span data-ttu-id="bafca-292">(Trecho de código – *laboratório de injeção de dependência ASP.net-Ex02-serviço de mensagens de registro*)</span><span class="sxs-lookup"><span data-stu-id="bafca-292">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register Message Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. <span data-ttu-id="bafca-293">Adicione uma referência ao namespace **MvcMusicStore. fábricas** .</span><span class="sxs-lookup"><span data-stu-id="bafca-293">Add a reference to **MvcMusicStore.Factories** namespace.</span></span>

    <span data-ttu-id="bafca-294">(Trecho de código – *laboratório de injeção de dependência ASP.net-Ex02-fábricas namespace*)</span><span class="sxs-lookup"><span data-stu-id="bafca-294">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Factories Namespace*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. <span data-ttu-id="bafca-295">Registre o **CustomViewPageActivator** como um ativador de página de exibição no contêiner do Unity:</span><span class="sxs-lookup"><span data-stu-id="bafca-295">Register **CustomViewPageActivator** as a View Page activator into the Unity container:</span></span>

    <span data-ttu-id="bafca-296">(Trecho de código – *laboratório de injeção de dependência ASP.net-Ex02-Register CustomViewPageActivator*)</span><span class="sxs-lookup"><span data-stu-id="bafca-296">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register CustomViewPageActivator*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. <span data-ttu-id="bafca-297">Substitua o resolvedor de dependência padrão ASP.NET MVC 4 por uma instância de **UnityDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="bafca-297">Replace ASP.NET MVC 4 default dependency resolver with an instance of **UnityDependencyResolver**.</span></span> <span data-ttu-id="bafca-298">Para fazer isso, substitua o conteúdo do método **Initialize** pelo código a seguir:</span><span class="sxs-lookup"><span data-stu-id="bafca-298">To do this, replace **Initialize** method content with the following code:</span></span>

    <span data-ttu-id="bafca-299">(Trecho de código – *laboratório de injeção de dependência ASP.net-Ex02 – resolvedor de dependência de atualização*)</span><span class="sxs-lookup"><span data-stu-id="bafca-299">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Update Dependency Resolver*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > <span data-ttu-id="bafca-300">O ASP.NET MVC fornece uma classe de resolvedor de dependências padrão.</span><span class="sxs-lookup"><span data-stu-id="bafca-300">ASP.NET MVC provides a default dependency resolver class.</span></span> <span data-ttu-id="bafca-301">Para trabalhar com resolvedores de dependência personalizados como aquele que criamos para o Unity, esse resolvedor precisa ser substituído.</span><span class="sxs-lookup"><span data-stu-id="bafca-301">To work with custom dependency resolvers as the one we have created for unity, this resolver has to be replaced.</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="bafca-302">Tarefa 4-executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="bafca-302">Task 4 - Running the Application</span></span>

<span data-ttu-id="bafca-303">Nesta tarefa, você executará o aplicativo para verificar se o navegador da loja consome o serviço e mostra a imagem e a mensagem recuperada:</span><span class="sxs-lookup"><span data-stu-id="bafca-303">In this task, you will run the application to verify that the Store Browser consumes the service and shows the image and the message retrieved:</span></span>

1. <span data-ttu-id="bafca-304">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="bafca-304">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="bafca-305">Clique em **rock** dentro do menu gêneros e veja como o **MessageService** foi injetado na exibição e carregou a mensagem de boas-vindas e a imagem.</span><span class="sxs-lookup"><span data-stu-id="bafca-305">Click **Rock** within the Genres Menu and see how the **MessageService** was injected to the view and loaded the welcome message and the image.</span></span> <span data-ttu-id="bafca-306">Neste exemplo, estamos entrando para &quot;&quot;de **rock** :</span><span class="sxs-lookup"><span data-stu-id="bafca-306">In this example, we are entering to &quot;**Rock**&quot;:</span></span>

    <span data-ttu-id="bafca-307">![Loja de música MVC-exibir injeção](aspnet-mvc-4-dependency-injection/_static/image10.png "Loja de música MVC-exibir injeção")</span><span class="sxs-lookup"><span data-stu-id="bafca-307">![MVC Music Store - View Injection](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - View Injection")</span></span>

    <span data-ttu-id="bafca-308">*Loja de música MVC-exibir injeção*</span><span class="sxs-lookup"><span data-stu-id="bafca-308">*MVC Music Store - View Injection*</span></span>
3. <span data-ttu-id="bafca-309">Feche o navegador.</span><span class="sxs-lookup"><span data-stu-id="bafca-309">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a><span data-ttu-id="bafca-310">Exercício 3: injetando filtros de ação</span><span class="sxs-lookup"><span data-stu-id="bafca-310">Exercise 3: Injecting Action Filters</span></span>

<span data-ttu-id="bafca-311">Nos **filtros de ação personalizada** do laboratório prático anterior, você trabalhou com a personalização e a injeção de filtros.</span><span class="sxs-lookup"><span data-stu-id="bafca-311">In the previous Hands-On lab **Custom Action Filters** you have worked with filters customization and injection.</span></span> <span data-ttu-id="bafca-312">Neste exercício, você aprenderá a injetar filtros com injeção de dependência usando o contêiner do Unity.</span><span class="sxs-lookup"><span data-stu-id="bafca-312">In this exercise, you will learn how to inject filters with Dependency Injection by using the Unity container.</span></span> <span data-ttu-id="bafca-313">Para fazer isso, você adicionará à solução de loja de música um filtro de ação personalizado que rastreará a atividade do site.</span><span class="sxs-lookup"><span data-stu-id="bafca-313">To do that, you will add to the Music Store solution a custom action filter that will trace the activity of the site.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a><span data-ttu-id="bafca-314">Tarefa 1-incluindo o filtro de controle na solução</span><span class="sxs-lookup"><span data-stu-id="bafca-314">Task 1 - Including the Tracking Filter in the Solution</span></span>

<span data-ttu-id="bafca-315">Nesta tarefa, você incluirá na loja de músicas um filtro de ação personalizada para rastrear eventos.</span><span class="sxs-lookup"><span data-stu-id="bafca-315">In this task, you will include in the Music Store a custom action filter to trace events.</span></span> <span data-ttu-id="bafca-316">Como os conceitos de filtro de ação personalizada já são tratados no laboratório anterior &quot;filtros de ação personalizados&quot;, você incluirá apenas a classe de filtro da pasta ativos deste laboratório e, em seguida, criará um provedor de filtro para o Unity:</span><span class="sxs-lookup"><span data-stu-id="bafca-316">As custom action filter concepts are already treated in the previous Lab &quot;Custom Action Filters&quot;, you will just include the filter class from the Assets folder of this lab, and then create a Filter Provider for Unity:</span></span>

1. <span data-ttu-id="bafca-317">Abra a solução **inicial** localizada na pasta **Filter\Begin da ação de injeção de Source\Ex03** .</span><span class="sxs-lookup"><span data-stu-id="bafca-317">Open the **Begin** solution located in the **Source\Ex03 - Injecting Action Filter\Begin** folder.</span></span> <span data-ttu-id="bafca-318">Caso contrário, você pode continuar usando a solução **final** obtida concluindo o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="bafca-318">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="bafca-319">Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="bafca-319">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="bafca-320">Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="bafca-320">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="bafca-321">Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="bafca-321">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="bafca-322">Por fim, Compile a solução clicando em **build** | **Compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="bafca-322">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="bafca-323">Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="bafca-323">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="bafca-324">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="bafca-324">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="bafca-325">É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.</span><span class="sxs-lookup"><span data-stu-id="bafca-325">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="bafca-326">Para obter mais informações, consulte este artigo: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="bafca-326">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="bafca-327">Inclua o arquivo **TraceActionFilter.cs** de **/sources/assets** para a pasta **/Filters** .</span><span class="sxs-lookup"><span data-stu-id="bafca-327">Include **TraceActionFilter.cs** file from **/Sources/Assets** to **/Filters** folder.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > <span data-ttu-id="bafca-328">Esse filtro de ação personalizada executa o rastreamento ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bafca-328">This custom action filter performs ASP.NET tracing.</span></span> <span data-ttu-id="bafca-329">Você pode verificar &quot;filtros de ação locais e dinâmicos do ASP.NET MVC 4&quot; laboratório para obter mais referências.</span><span class="sxs-lookup"><span data-stu-id="bafca-329">You can check &quot;ASP.NET MVC 4 local and Dynamic Action Filters&quot; Lab for more reference.</span></span>
3. <span data-ttu-id="bafca-330">Adicione a classe vazia **FilterProvider.cs** ao projeto na pasta **/Filters.**</span><span class="sxs-lookup"><span data-stu-id="bafca-330">Add the empty class **FilterProvider.cs** to the project in the folder **/Filters.**</span></span>
4. <span data-ttu-id="bafca-331">Adicione os namespaces **System. Web. Mvc** e **Microsoft. Practices. Unity** no **FilterProvider.cs**.</span><span class="sxs-lookup"><span data-stu-id="bafca-331">Add the **System.Web.Mvc** and **Microsoft.Practices.Unity** namespaces in **FilterProvider.cs**.</span></span>

    <span data-ttu-id="bafca-332">(Trecho de código – *laboratório de injeção de dependência ASP.net-Ex03-provedor de filtros Adicionando namespaces*)</span><span class="sxs-lookup"><span data-stu-id="bafca-332">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. <span data-ttu-id="bafca-333">Faça a classe herdar da interface **IFilterProvider** .</span><span class="sxs-lookup"><span data-stu-id="bafca-333">Make the class inherit from **IFilterProvider** Interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. <span data-ttu-id="bafca-334">Adicione uma propriedade **IUnityContainer** na classe **FilterProvider** e, em seguida, crie um construtor de classe para atribuir o contêiner.</span><span class="sxs-lookup"><span data-stu-id="bafca-334">Add a **IUnityContainer** property in the **FilterProvider** class, and then create a class constructor to assign the container.</span></span>

    <span data-ttu-id="bafca-335">(Trecho de código- *ASP.net de injeção de dependência do Ex03-Construtor de provedor de filtro*)</span><span class="sxs-lookup"><span data-stu-id="bafca-335">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Constructor*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="bafca-336">O construtor da classe do provedor de filtro não está criando um **novo** objeto dentro de.</span><span class="sxs-lookup"><span data-stu-id="bafca-336">The filter provider class constructor is not creating a **new** object inside.</span></span> <span data-ttu-id="bafca-337">O contêiner é passado como um parâmetro e a dependência é resolvida pelo Unity.</span><span class="sxs-lookup"><span data-stu-id="bafca-337">The container is passed as a parameter, and the dependency is solved by Unity.</span></span>
7. <span data-ttu-id="bafca-338">Na classe **FilterProvider** , implemente o método **getfiltras** da interface **IFilterProvider** .</span><span class="sxs-lookup"><span data-stu-id="bafca-338">In the **FilterProvider** class, implement the method **GetFilters** from **IFilterProvider** interface.</span></span>

    <span data-ttu-id="bafca-339">(Trecho de código – *laboratório de injeção de dependência ASP.net-Ex03-filtro Getfiltros*)</span><span class="sxs-lookup"><span data-stu-id="bafca-339">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider GetFilters*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a><span data-ttu-id="bafca-340">Tarefa 2-registrando e habilitando o filtro</span><span class="sxs-lookup"><span data-stu-id="bafca-340">Task 2 - Registering and Enabling the Filter</span></span>

<span data-ttu-id="bafca-341">Nesta tarefa, você habilitará o rastreamento de site.</span><span class="sxs-lookup"><span data-stu-id="bafca-341">In this task, you will enable site tracking.</span></span> <span data-ttu-id="bafca-342">Para fazer isso, você registrará o filtro no método **bootstrapper.cs BuildUnityContainer** para iniciar o rastreamento:</span><span class="sxs-lookup"><span data-stu-id="bafca-342">To do that, you will register the filter in **Bootstrapper.cs BuildUnityContainer** method to start tracing:</span></span>

1. <span data-ttu-id="bafca-343">Abra o **Web. config** localizado na raiz do projeto e habilite o rastreamento de rastreamento no grupo System. Web.</span><span class="sxs-lookup"><span data-stu-id="bafca-343">Open **Web.config** located in the project root and enable trace tracking at System.Web group.</span></span>

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. <span data-ttu-id="bafca-344">Abra **bootstrapper.cs** na raiz do projeto.</span><span class="sxs-lookup"><span data-stu-id="bafca-344">Open **Bootstrapper.cs** at project root.</span></span>
3. <span data-ttu-id="bafca-345">Adicione uma referência ao namespace **MvcMusicStore. Filters** .</span><span class="sxs-lookup"><span data-stu-id="bafca-345">Add a reference to the **MvcMusicStore.Filters** namespace.</span></span>

    <span data-ttu-id="bafca-346">(Trecho de código- *laboratório de injeção de dependência ASP.net-Ex03-bootstrapper Adicionando namespaces*)</span><span class="sxs-lookup"><span data-stu-id="bafca-346">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. <span data-ttu-id="bafca-347">Selecione o método **BuildUnityContainer** e registre o filtro no contêiner do Unity.</span><span class="sxs-lookup"><span data-stu-id="bafca-347">Select the **BuildUnityContainer** method and register the filter in the Unity Container.</span></span> <span data-ttu-id="bafca-348">Você precisará registrar o provedor de filtro, bem como o filtro de ação.</span><span class="sxs-lookup"><span data-stu-id="bafca-348">You will have to register the filter provider as well as the action filter.</span></span>

    <span data-ttu-id="bafca-349">(Trecho de código – *laboratório de injeção de dependência ASP.net-Ex03-registrar FilterProvider e ActionFilter*)</span><span class="sxs-lookup"><span data-stu-id="bafca-349">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Register FilterProvider and ActionFilter*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="bafca-350">Tarefa 3-executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="bafca-350">Task 3 - Running the Application</span></span>

<span data-ttu-id="bafca-351">Nesta tarefa, você executará o aplicativo e testará se o filtro de ação personalizada está rastreando a atividade:</span><span class="sxs-lookup"><span data-stu-id="bafca-351">In this task, you will run the application and test that the custom action filter is tracing the activity:</span></span>

1. <span data-ttu-id="bafca-352">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="bafca-352">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="bafca-353">Clique em **rock** dentro do menu gêneros.</span><span class="sxs-lookup"><span data-stu-id="bafca-353">Click **Rock** within the Genres Menu.</span></span> <span data-ttu-id="bafca-354">Você pode navegar para mais gêneros se desejar.</span><span class="sxs-lookup"><span data-stu-id="bafca-354">You can browse to more genres if you want to.</span></span>

    <span data-ttu-id="bafca-355">![Loja de Música](aspnet-mvc-4-dependency-injection/_static/image11.png "Loja de Música")</span><span class="sxs-lookup"><span data-stu-id="bafca-355">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span></span>

    <span data-ttu-id="bafca-356">*Loja de Música*</span><span class="sxs-lookup"><span data-stu-id="bafca-356">*Music Store*</span></span>
3. <span data-ttu-id="bafca-357">Navegue até **/trace.axd** para ver a página de rastreamento do aplicativo e clique em **Exibir detalhes**.</span><span class="sxs-lookup"><span data-stu-id="bafca-357">Browse to **/Trace.axd** to see the Application Trace page, and then click **View Details**.</span></span>

    <span data-ttu-id="bafca-358">![Log de rastreamento do aplicativo](aspnet-mvc-4-dependency-injection/_static/image12.png "Log de rastreamento do aplicativo")</span><span class="sxs-lookup"><span data-stu-id="bafca-358">![Application Trace Log](aspnet-mvc-4-dependency-injection/_static/image12.png "Application Trace Log")</span></span>

    <span data-ttu-id="bafca-359">*Log de rastreamento do aplicativo*</span><span class="sxs-lookup"><span data-stu-id="bafca-359">*Application Trace Log*</span></span>

    <span data-ttu-id="bafca-360">![Rastreamento de aplicativo-detalhes da solicitação](aspnet-mvc-4-dependency-injection/_static/image13.png "Rastreamento de aplicativo-detalhes da solicitação")</span><span class="sxs-lookup"><span data-stu-id="bafca-360">![Application Trace - Request Details](aspnet-mvc-4-dependency-injection/_static/image13.png "Application Trace - Request Details")</span></span>

    <span data-ttu-id="bafca-361">*Rastreamento de aplicativo-detalhes da solicitação*</span><span class="sxs-lookup"><span data-stu-id="bafca-361">*Application Trace - Request Details*</span></span>
4. <span data-ttu-id="bafca-362">Feche o navegador.</span><span class="sxs-lookup"><span data-stu-id="bafca-362">Close the browser.</span></span>

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="bafca-363">Resumo</span><span class="sxs-lookup"><span data-stu-id="bafca-363">Summary</span></span>

<span data-ttu-id="bafca-364">Ao concluir este laboratório prático, você aprendeu a usar a injeção de dependência no ASP.NET MVC 4 integrando o Unity usando um pacote NuGet.</span><span class="sxs-lookup"><span data-stu-id="bafca-364">By completing this Hands-On Lab you have learned how to use Dependency Injection in ASP.NET MVC 4 by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="bafca-365">Para conseguir isso, você usou a injeção de dependência dentro de controladores, exibições e filtros de ação.</span><span class="sxs-lookup"><span data-stu-id="bafca-365">To achieve that, you have used Dependency Injection inside controllers, views and action filters.</span></span>

<span data-ttu-id="bafca-366">Os conceitos a seguir foram abordados:</span><span class="sxs-lookup"><span data-stu-id="bafca-366">The following concepts were covered:</span></span>

- <span data-ttu-id="bafca-367">Recursos de injeção de dependência do MVC 4 do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="bafca-367">ASP.NET MVC 4 Dependency Injection features</span></span>
- <span data-ttu-id="bafca-368">Integração do Unity usando o pacote NuGet do Unity. Mvc3</span><span class="sxs-lookup"><span data-stu-id="bafca-368">Unity integration using Unity.Mvc3 NuGet Package</span></span>
- <span data-ttu-id="bafca-369">Injeção de dependência em controladores</span><span class="sxs-lookup"><span data-stu-id="bafca-369">Dependency Injection in Controllers</span></span>
- <span data-ttu-id="bafca-370">Injeção de dependência em exibições</span><span class="sxs-lookup"><span data-stu-id="bafca-370">Dependency Injection in Views</span></span>
- <span data-ttu-id="bafca-371">Injeção de dependência de filtros de ação</span><span class="sxs-lookup"><span data-stu-id="bafca-371">Dependency injection of Action Filters</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="bafca-372">Apêndice A: Instalando o Visual Studio Express 2012 para Web</span><span class="sxs-lookup"><span data-stu-id="bafca-372">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="bafca-373">Você pode instalar o **Microsoft Visual Studio Express 2012 para Web** ou outra versão &quot;Express&quot; usando o **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="bafca-373">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="bafca-374">As instruções a seguir orientam você pelas etapas necessárias para instalar o *Visual Studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="bafca-374">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="bafca-375">Ir para [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="bafca-375">Go to [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="bafca-376">Como alternativa, se você já tiver instalado o Web Platform Installer, poderá abri-lo e pesquisar o produto &quot;<em>Visual Studio Express 2012 para Web com o SDK do Windows Azure</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="bafca-376">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="bafca-377">Clique em **instalar agora**.</span><span class="sxs-lookup"><span data-stu-id="bafca-377">Click on **Install Now**.</span></span> <span data-ttu-id="bafca-378">Se você não tiver **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.</span><span class="sxs-lookup"><span data-stu-id="bafca-378">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="bafca-379">Quando **Web Platform Installer** estiver aberto, clique em **instalar** para iniciar a instalação.</span><span class="sxs-lookup"><span data-stu-id="bafca-379">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="bafca-380">![Instalar Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Instalar Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="bafca-380">![Install Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="bafca-381">*Instalar Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="bafca-381">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="bafca-382">Leia todos os termos e licenças de produtos e clique em **aceito** para continuar.</span><span class="sxs-lookup"><span data-stu-id="bafca-382">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Aceitando os termos de licença](aspnet-mvc-4-dependency-injection/_static/image15.png)

    <span data-ttu-id="bafca-384">*Aceitando os termos de licença*</span><span class="sxs-lookup"><span data-stu-id="bafca-384">*Accepting the license terms*</span></span>
5. <span data-ttu-id="bafca-385">Aguarde até que o processo de download e instalação seja concluído.</span><span class="sxs-lookup"><span data-stu-id="bafca-385">Wait until the downloading and installation process completes.</span></span>

    ![Progresso da instalação](aspnet-mvc-4-dependency-injection/_static/image16.png)

    <span data-ttu-id="bafca-387">*Progresso da instalação*</span><span class="sxs-lookup"><span data-stu-id="bafca-387">*Installation progress*</span></span>
6. <span data-ttu-id="bafca-388">Quando a instalação for concluída, clique em **concluir**.</span><span class="sxs-lookup"><span data-stu-id="bafca-388">When the installation completes, click **Finish**.</span></span>

    ![Instalação concluída](aspnet-mvc-4-dependency-injection/_static/image17.png)

    <span data-ttu-id="bafca-390">*Instalação concluída*</span><span class="sxs-lookup"><span data-stu-id="bafca-390">*Installation completed*</span></span>
7. <span data-ttu-id="bafca-391">Clique em **sair** para fechar Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="bafca-391">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="bafca-392">Para abrir o Visual Studio Express para Web, vá para a tela **Iniciar** e comece a escrever &quot;**vs Express**&quot;e, em seguida, clique no bloco **vs Express para Web** .</span><span class="sxs-lookup"><span data-stu-id="bafca-392">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Bloco VS Express para Web](aspnet-mvc-4-dependency-injection/_static/image18.png)

    <span data-ttu-id="bafca-394">*Bloco VS Express para Web*</span><span class="sxs-lookup"><span data-stu-id="bafca-394">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="bafca-395">Apêndice B: usando trechos de código</span><span class="sxs-lookup"><span data-stu-id="bafca-395">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="bafca-396">Com trechos de código, você tem todo o código de que precisa ao seu alcance.</span><span class="sxs-lookup"><span data-stu-id="bafca-396">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="bafca-397">O documento de laboratório informará exatamente quando você pode usá-los, conforme mostrado na figura a seguir.</span><span class="sxs-lookup"><span data-stu-id="bafca-397">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="bafca-398">![Usando trechos de código do Visual Studio para inserir código em seu projeto](aspnet-mvc-4-dependency-injection/_static/image19.png "Usando trechos de código do Visual Studio para inserir código em seu projeto")</span><span class="sxs-lookup"><span data-stu-id="bafca-398">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-dependency-injection/_static/image19.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="bafca-399">*Usando trechos de código do Visual Studio para inserir código em seu projeto*</span><span class="sxs-lookup"><span data-stu-id="bafca-399">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="bafca-400">***Para adicionar um trecho de código usando o tecladoC# (somente)***</span><span class="sxs-lookup"><span data-stu-id="bafca-400">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="bafca-401">Coloque o cursor onde você deseja inserir o código.</span><span class="sxs-lookup"><span data-stu-id="bafca-401">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="bafca-402">Comece digitando o nome do trecho de código (sem espaços ou hifens).</span><span class="sxs-lookup"><span data-stu-id="bafca-402">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="bafca-403">Observe como o IntelliSense exibe nomes de trechos de código correspondentes.</span><span class="sxs-lookup"><span data-stu-id="bafca-403">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="bafca-404">Selecione o trecho correto (ou continue digitando até que o nome do trecho inteiro seja selecionado).</span><span class="sxs-lookup"><span data-stu-id="bafca-404">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="bafca-405">Pressione a tecla TAB duas vezes para inserir o trecho de código no local do cursor.</span><span class="sxs-lookup"><span data-stu-id="bafca-405">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="bafca-406">![Comece a digitar o nome do trecho](aspnet-mvc-4-dependency-injection/_static/image20.png "Comece a digitar o nome do trecho")</span><span class="sxs-lookup"><span data-stu-id="bafca-406">![Start typing the snippet name](aspnet-mvc-4-dependency-injection/_static/image20.png "Start typing the snippet name")</span></span>

<span data-ttu-id="bafca-407">*Comece a digitar o nome do trecho*</span><span class="sxs-lookup"><span data-stu-id="bafca-407">*Start typing the snippet name*</span></span>

<span data-ttu-id="bafca-408">![Pressione Tab para selecionar o trecho realçado](aspnet-mvc-4-dependency-injection/_static/image21.png "Pressione Tab para selecionar o trecho realçado")</span><span class="sxs-lookup"><span data-stu-id="bafca-408">![Press Tab to select the highlighted snippet](aspnet-mvc-4-dependency-injection/_static/image21.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="bafca-409">*Pressione Tab para selecionar o trecho realçado*</span><span class="sxs-lookup"><span data-stu-id="bafca-409">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="bafca-410">![Pressione Tab novamente e o trecho será expandido](aspnet-mvc-4-dependency-injection/_static/image22.png "Pressione Tab novamente e o trecho será expandido")</span><span class="sxs-lookup"><span data-stu-id="bafca-410">![Press Tab again and the snippet will expand](aspnet-mvc-4-dependency-injection/_static/image22.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="bafca-411">*Pressione Tab novamente e o trecho será expandido*</span><span class="sxs-lookup"><span data-stu-id="bafca-411">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="bafca-412">***Para adicionar um trecho de código usando o mouseC#(, Visual Basic e XML)*** uma.</span><span class="sxs-lookup"><span data-stu-id="bafca-412">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="bafca-413">Clique com o botão direito do mouse no local em que você deseja inserir o trecho de código.</span><span class="sxs-lookup"><span data-stu-id="bafca-413">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="bafca-414">Selecione **Inserir trecho** seguido por **meus trechos de código**.</span><span class="sxs-lookup"><span data-stu-id="bafca-414">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="bafca-415">Selecione o trecho relevante na lista clicando nele.</span><span class="sxs-lookup"><span data-stu-id="bafca-415">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="bafca-416">![Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho](aspnet-mvc-4-dependency-injection/_static/image23.png "Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho")</span><span class="sxs-lookup"><span data-stu-id="bafca-416">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-dependency-injection/_static/image23.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="bafca-417">*Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho*</span><span class="sxs-lookup"><span data-stu-id="bafca-417">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="bafca-418">![Selecione o trecho relevante na lista clicando nele](aspnet-mvc-4-dependency-injection/_static/image24.png "Selecione o trecho relevante na lista clicando nele")</span><span class="sxs-lookup"><span data-stu-id="bafca-418">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-dependency-injection/_static/image24.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="bafca-419">*Selecione o trecho relevante na lista clicando nele*</span><span class="sxs-lookup"><span data-stu-id="bafca-419">*Pick the relevant snippet from the list, by clicking on it*</span></span>
