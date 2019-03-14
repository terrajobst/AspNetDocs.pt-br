---
title: Injeção de dependência de componentes do Razor
author: guardrex
description: Veja como Blazor e Razor componentes de aplicativos podem usar serviços internos mantendo-os injetados em componentes.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/dependency-injection
ms.openlocfilehash: 6ce8fa74f20145f48797d267c20ef2593368b941
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57042423"
---
# <a name="razor-components-dependency-injection"></a><span data-ttu-id="6d1ac-103">Injeção de dependência de componentes do Razor</span><span class="sxs-lookup"><span data-stu-id="6d1ac-103">Razor Components dependency injection</span></span>

<span data-ttu-id="6d1ac-104">Por [Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="6d1ac-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="6d1ac-105">[Injeção de dependência (DI)](/aspnet/core/fundamentals/dependency-injection) é interno.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-105">[Dependency injection (DI)](/aspnet/core/fundamentals/dependency-injection) is built-in.</span></span> <span data-ttu-id="6d1ac-106">Aplicativos podem usar os serviços internos mantendo-os injetados em componentes.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-106">Apps can use built-in services by having them injected into components.</span></span> <span data-ttu-id="6d1ac-107">Aplicativos também podem definir serviços personalizados e disponibilizá-las por meio da DI.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-107">Apps can also define custom services and make them available via DI.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="6d1ac-108">Injeção de dependência</span><span class="sxs-lookup"><span data-stu-id="6d1ac-108">Dependency injection</span></span>

<span data-ttu-id="6d1ac-109">Injeção de dependência é uma técnica para acessar os serviços configurados em um local central.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-109">DI is a technique for accessing services configured in a central location.</span></span> <span data-ttu-id="6d1ac-110">Isso pode ser útil para:</span><span class="sxs-lookup"><span data-stu-id="6d1ac-110">This can be useful to:</span></span>

* <span data-ttu-id="6d1ac-111">Compartilhar uma única instância de uma classe de serviço em vários componentes (conhecido como um *singleton* service).</span><span class="sxs-lookup"><span data-stu-id="6d1ac-111">Share a single instance of a service class across many components (known as a *singleton* service).</span></span>
* <span data-ttu-id="6d1ac-112">Desacoplar componentes das classes concretas de serviço específico e fazer referência apenas abstrações.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-112">Decouple components from particular concrete service classes and only reference abstractions.</span></span> <span data-ttu-id="6d1ac-113">Por exemplo, uma interface `IDataAccess` é implementado por uma classe concreta `DataAccess`.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-113">For example, an interface `IDataAccess` is implemented by a concrete class `DataAccess`.</span></span> <span data-ttu-id="6d1ac-114">Quando um componente usa DI para receber um `IDataAccess` implementação, o componente não está acoplada ao tipo concreto.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-114">When a component uses DI to receive an `IDataAccess` implementation, the component isn't coupled to the concrete type.</span></span> <span data-ttu-id="6d1ac-115">A implementação pode ser trocada, talvez uma implementação fictícia em testes de unidade.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-115">The implementation can be swapped, perhaps to a mock implementation in unit tests.</span></span>

<span data-ttu-id="6d1ac-116">O sistema de DI é responsável por fornecer instâncias dos serviços de componentes.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-116">The DI system is responsible for supplying instances of services to components.</span></span> <span data-ttu-id="6d1ac-117">Injeção de dependência também resolve as dependências recursivamente, de modo que os próprios serviços podem depender mais serviços.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-117">DI also resolves dependencies recursively so that services themselves can depend on further services.</span></span> <span data-ttu-id="6d1ac-118">Injeção de dependência é configurada durante a inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-118">DI is configured during startup of the app.</span></span> <span data-ttu-id="6d1ac-119">Um exemplo é mostrado neste tópico.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-119">An example is shown later in this topic.</span></span>

## <a name="add-services-to-di"></a><span data-ttu-id="6d1ac-120">Adicionar serviços para injeção de dependência</span><span class="sxs-lookup"><span data-stu-id="6d1ac-120">Add services to DI</span></span>

<span data-ttu-id="6d1ac-121">Depois de criar um novo aplicativo, examine o `Startup.ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="6d1ac-121">After creating a new app, examine the `Startup.ConfigureServices` method:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

<span data-ttu-id="6d1ac-122">O `ConfigureServices` método recebe um [IServiceCollection](/dotnet/api/microsoft.extensions.dependencyinjection.iservicecollection), que é uma lista de objetos do descritor de serviço ([ServiceDescriptor](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor)).</span><span class="sxs-lookup"><span data-stu-id="6d1ac-122">The `ConfigureServices` method is passed an [IServiceCollection](/dotnet/api/microsoft.extensions.dependencyinjection.iservicecollection), which is a list of service descriptor objects ([ServiceDescriptor](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor)).</span></span> <span data-ttu-id="6d1ac-123">Serviços são adicionados, fornecendo os descritores de serviço para a coleção de serviço.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-123">Services are added by providing service descriptors to the service collection.</span></span> <span data-ttu-id="6d1ac-124">O exemplo de código a seguir demonstra o conceito:</span><span class="sxs-lookup"><span data-stu-id="6d1ac-124">The following code sample demonstrates the concept:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

<span data-ttu-id="6d1ac-125">Serviços podem ser configurados com os seguintes tempos de vida:</span><span class="sxs-lookup"><span data-stu-id="6d1ac-125">Services can be configured with the following lifetimes:</span></span>

| <span data-ttu-id="6d1ac-126">Método</span><span class="sxs-lookup"><span data-stu-id="6d1ac-126">Method</span></span>      | <span data-ttu-id="6d1ac-127">Descrição</span><span class="sxs-lookup"><span data-stu-id="6d1ac-127">Description</span></span> |
| ----------- | ----------- |
| [<span data-ttu-id="6d1ac-128">Singleton</span><span class="sxs-lookup"><span data-stu-id="6d1ac-128">Singleton</span></span>](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor.singleton#Microsoft_Extensions_DependencyInjection_ServiceDescriptor_Singleton__1_System_Func_System_IServiceProvider___0__) | <span data-ttu-id="6d1ac-129">Injeção de dependência cria uma *única instância* do serviço.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-129">DI creates a *single instance* of the service.</span></span> <span data-ttu-id="6d1ac-130">Todos os componentes que exigem esse serviço recebem uma referência a essa instância.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-130">All components requiring this service receive a reference to this instance.</span></span> |
| [<span data-ttu-id="6d1ac-131">Transitório</span><span class="sxs-lookup"><span data-stu-id="6d1ac-131">Transient</span></span>](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor.transient) | <span data-ttu-id="6d1ac-132">Sempre que um componente exige esse serviço, ele recebe um *nova instância* do serviço.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-132">Whenever a component requires this service, it receives a *new instance* of the service.</span></span> |
| [<span data-ttu-id="6d1ac-133">Com escopo</span><span class="sxs-lookup"><span data-stu-id="6d1ac-133">Scoped</span></span>](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor.scoped) | <span data-ttu-id="6d1ac-134">Blazor do lado do cliente atualmente não tem o conceito de escopos de injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-134">Client-side Blazor doesn't currently have the concept of DI scopes.</span></span> <span data-ttu-id="6d1ac-135">`Scoped` se comporta como `Singleton`.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-135">`Scoped` behaves like `Singleton`.</span></span> <span data-ttu-id="6d1ac-136">No entanto, os componentes do ASP.NET Core Razor suportam o `Scoped` tempo de vida.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-136">However, ASP.NET Core Razor Components support the `Scoped` lifetime.</span></span> <span data-ttu-id="6d1ac-137">Em um componente do Razor, um registro de serviço com escopo está no escopo para a conexão.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-137">In a Razor Component, a scoped service registration is scoped to the connection.</span></span> <span data-ttu-id="6d1ac-138">Por esse motivo, é preferencial para os serviços que devem ser direcionados para o usuário atual usar os serviços com escopo (mesmo se a intenção atual é executar o lado do cliente no navegador).</span><span class="sxs-lookup"><span data-stu-id="6d1ac-138">For this reason, using scoped services is preferred for services that should be scoped to the current user (even if the current intent is to run client-side in the browser).</span></span> |

<span data-ttu-id="6d1ac-139">O sistema de DI é baseado no sistema de DI no ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-139">The DI system is based on the DI system in ASP.NET Core.</span></span> <span data-ttu-id="6d1ac-140">Para obter mais informações, consulte [injeção de dependência no ASP.NET Core](/aspnet/core/fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="6d1ac-140">For more information, see [Dependency injection in ASP.NET Core](/aspnet/core/fundamentals/dependency-injection).</span></span>

## <a name="default-services"></a><span data-ttu-id="6d1ac-141">Serviços padrão</span><span class="sxs-lookup"><span data-stu-id="6d1ac-141">Default services</span></span>

<span data-ttu-id="6d1ac-142">Os serviços padrão são automaticamente adicionados à coleção de serviços de um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-142">Default services are automatically added to the service collection of an app.</span></span> <span data-ttu-id="6d1ac-143">A tabela a seguir mostra alguns dos serviços padrão útil fornecidos.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-143">The following table shows some of the useful default services provided.</span></span>

| <span data-ttu-id="6d1ac-144">Método</span><span class="sxs-lookup"><span data-stu-id="6d1ac-144">Method</span></span>       | <span data-ttu-id="6d1ac-145">Descrição</span><span class="sxs-lookup"><span data-stu-id="6d1ac-145">Description</span></span> |
| ------------ | ----------- |
| [<span data-ttu-id="6d1ac-146">HttpClient</span><span class="sxs-lookup"><span data-stu-id="6d1ac-146">HttpClient</span></span>](/dotnet/api/system.net.http.httpclient) | <span data-ttu-id="6d1ac-147">Fornece métodos para enviar solicitações HTTP e receber respostas HTTP de um recurso identificado por um URI (singleton).</span><span class="sxs-lookup"><span data-stu-id="6d1ac-147">Provides methods for sending HTTP requests and receiving HTTP responses from a resource identified by a URI (singleton).</span></span> <span data-ttu-id="6d1ac-148">Observe que esta instância do `HttpClient` usa o navegador para tratar o tráfego HTTP em segundo plano.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-148">Note that this instance of `HttpClient` uses the browser for handling the HTTP traffic in the background.</span></span> <span data-ttu-id="6d1ac-149">[HttpClient.BaseAddress](/dotnet/api/system.net.http.httpclient.baseaddress) é definida automaticamente como o prefixo URI base do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-149">[HttpClient.BaseAddress](/dotnet/api/system.net.http.httpclient.baseaddress) is automatically set to the base URI prefix of the app.</span></span> <span data-ttu-id="6d1ac-150">`HttpClient` é fornecida apenas para aplicativos do lado do cliente Blazor.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-150">`HttpClient` is only provided to client-side Blazor apps.</span></span> |
| `IJSRuntime` | <span data-ttu-id="6d1ac-151">Representa uma instância de um tempo de execução do JavaScript para o qual chamadas podem ser distribuídas.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-151">Represents an instance of a JavaScript runtime to which calls may be dispatched.</span></span> <span data-ttu-id="6d1ac-152">Para obter mais informações, consulte <xref:razor-components/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-152">For more information, see <xref:razor-components/javascript-interop>.</span></span> |
| `IUriHelper` | <span data-ttu-id="6d1ac-153">Auxiliares para trabalhar com o estado de navegação e URIs (singleton).</span><span class="sxs-lookup"><span data-stu-id="6d1ac-153">Helpers for working with URIs and navigation state (singleton).</span></span> <span data-ttu-id="6d1ac-154">`IUriHelper` é fornecido para ambos os aplicativos do lado do cliente Blazor e componentes do ASP.NET Core Razor.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-154">`IUriHelper` is provided to both client-side Blazor and ASP.NET Core Razor Components apps.</span></span> |

<span data-ttu-id="6d1ac-155">Observe que é possível usar um provedor de serviços personalizados em vez do provedor de serviços padrão que é adicionado pelo modelo padrão.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-155">Note that it is possible to use a custom services provider instead of the default service provider that's added by the default template.</span></span> <span data-ttu-id="6d1ac-156">Um provedor de serviços personalizados automaticamente não fornece os serviços padrão listados na tabela.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-156">A custom service provider doesn't automatically provide the default services listed in the table.</span></span> <span data-ttu-id="6d1ac-157">Esses serviços devem ser adicionados explicitamente para o novo provedor de serviço.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-157">Those services must be added to the new service provider explicitly.</span></span>

## <a name="request-a-service-in-a-component"></a><span data-ttu-id="6d1ac-158">Um serviço em um componente de solicitação</span><span class="sxs-lookup"><span data-stu-id="6d1ac-158">Request a service in a component</span></span>

<span data-ttu-id="6d1ac-159">Depois que os serviços são adicionados à coleção de serviços, pode ser injetados em modelos do Razor dos componentes usando o `@inject` diretiva do Razor.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-159">Once services are added to the service collection, they can be injected into the components' Razor templates using the `@inject` Razor directive.</span></span> <span data-ttu-id="6d1ac-160">`@inject` tem dois parâmetros:</span><span class="sxs-lookup"><span data-stu-id="6d1ac-160">`@inject` has two parameters:</span></span>

* <span data-ttu-id="6d1ac-161">Nome do tipo: O tipo do serviço para injetar.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-161">Type name: The type of the service to inject.</span></span>
* <span data-ttu-id="6d1ac-162">Nome da propriedade: O nome da propriedade que está recebendo o serviço de aplicativo injetado.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-162">Property name: The name of the property receiving the injected app service.</span></span> <span data-ttu-id="6d1ac-163">Observe que a propriedade não exige a criação manual.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-163">Note that the property doesn't require manual creation.</span></span> <span data-ttu-id="6d1ac-164">O compilador cria a propriedade.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-164">The compiler creates the property.</span></span>

<span data-ttu-id="6d1ac-165">Vários `@inject` instruções podem ser usadas para inserir serviços diferentes.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-165">Multiple `@inject` statements can be used to inject different services.</span></span>

<span data-ttu-id="6d1ac-166">O exemplo a seguir mostra como usar `@inject`.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-166">The following example shows how to use `@inject`.</span></span> <span data-ttu-id="6d1ac-167">O serviço que implementa `Services.IDataAccess` é injetado na propriedade do componente `DataRepository`.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-167">The service implementing `Services.IDataAccess` is injected into the component's property `DataRepository`.</span></span> <span data-ttu-id="6d1ac-168">Observe como o código está usando apenas o `IDataAccess` abstração:</span><span class="sxs-lookup"><span data-stu-id="6d1ac-168">Note how the code is only using the `IDataAccess` abstraction:</span></span>

```csharp
@page "/customer-list"
@using Services
@inject IDataAccess DataRepository

<ul>
    @if (Customers != null)
    {
        @foreach (var customer in Customers)
        {
            <li>@customer.FirstName @customer.LastName</li>
        }
    }
</ul>

@functions {
    private IReadOnlyList<Customer> Customers;

    protected override async Task OnInitAsync()
    {
        // The property DataRepository received an implementation
        // of IDataAccess through dependency injection. Use 
        // DataRepository to obtain data from the server.
        Customers = await DataRepository.GetAllCustomersAsync();
    }
}
```

<span data-ttu-id="6d1ac-169">Internamente, a propriedade gerada (`DataRepository`) é decorado com o `InjectAttribute` atributo.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-169">Internally, the generated property (`DataRepository`) is decorated with the `InjectAttribute` attribute.</span></span> <span data-ttu-id="6d1ac-170">Normalmente, esse atributo não é usado diretamente.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-170">Typically, this attribute isn't used directly.</span></span> <span data-ttu-id="6d1ac-171">Se uma classe base é necessária para componentes e injetadas propriedades também são necessárias para a classe base, `InjectAttribute` podem ser adicionadas manualmente:</span><span class="sxs-lookup"><span data-stu-id="6d1ac-171">If a base class is required for components and injected properties are also required for the base class, `InjectAttribute` can be manually added:</span></span>

```csharp
public class ComponentBase : BlazorComponent
{
    // Dependency injection works even if using the
    // InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

<span data-ttu-id="6d1ac-172">Em componentes derivados da classe base, o `@inject` diretiva não é necessária.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-172">In components derived from the base class, the `@inject` directive isn't required.</span></span> <span data-ttu-id="6d1ac-173">O `InjectAttribute` da classe base é suficiente:</span><span class="sxs-lookup"><span data-stu-id="6d1ac-173">The `InjectAttribute` of the base class is sufficient:</span></span>

```csharp
@page "/demo"
@inherits ComponentBase

<h1>...</h1>
...
```

## <a name="dependency-injection-in-services"></a><span data-ttu-id="6d1ac-174">Injeção de dependência nos serviços</span><span class="sxs-lookup"><span data-stu-id="6d1ac-174">Dependency injection in services</span></span>

<span data-ttu-id="6d1ac-175">Serviços complexos podem exigir serviços adicionais.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-175">Complex services might require additional services.</span></span> <span data-ttu-id="6d1ac-176">No exemplo anterior, `DataAccess` pode exigir o `HttpClient` serviço padrão.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-176">In the prior example, `DataAccess` might require the `HttpClient` default service.</span></span> <span data-ttu-id="6d1ac-177">`@inject` ou o `InjectAttribute` não pode ser usado nos serviços.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-177">`@inject` or the `InjectAttribute` can't be used in services.</span></span> <span data-ttu-id="6d1ac-178">*Injeção de construtor* deve ser usado em vez disso.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-178">*Constructor injection* must be used instead.</span></span> <span data-ttu-id="6d1ac-179">Os serviços necessários são adicionados, adicionando parâmetros ao construtor do serviço.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-179">Required services are added by adding parameters to the service's constructor.</span></span> <span data-ttu-id="6d1ac-180">Quando a injeção de dependência cria o serviço, ele reconhece os serviços que ele requer no construtor e fornece-os adequadamente.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-180">When dependency injection creates the service, it recognizes the services it requires in the constructor and provides them accordingly.</span></span>

<span data-ttu-id="6d1ac-181">O exemplo de código a seguir demonstra o conceito:</span><span class="sxs-lookup"><span data-stu-id="6d1ac-181">The following code sample demonstrates the concept:</span></span>

```csharp
public class DataAccess : IDataAccess
{
    // The constructor receives an HttpClient via dependency
    // injection. HttpClient is a default service.
    public DataAccess(HttpClient client)
    {
        ...
    }
    ...
}
```

<span data-ttu-id="6d1ac-182">Observe os seguintes pré-requisitos para injeção de construtor:</span><span class="sxs-lookup"><span data-stu-id="6d1ac-182">Note the following prerequisites for constructor injection:</span></span>

* <span data-ttu-id="6d1ac-183">Deve haver um construtor cujos argumentos podem ser atendidos pela injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-183">There must be one constructor whose arguments can all be fulfilled by dependency injection.</span></span> <span data-ttu-id="6d1ac-184">Observe que os parâmetros adicionais não cobertos pela DI sejam permitidos se valores padrão são especificados para elas.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-184">Note that additional parameters not covered by DI are allowed if default values are specified for them.</span></span>
* <span data-ttu-id="6d1ac-185">O construtor aplicável deve ser *público*.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-185">The applicable constructor must be *public*.</span></span>
* <span data-ttu-id="6d1ac-186">Deve haver apenas um construtor aplicável.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-186">There must only be one applicable constructor.</span></span> <span data-ttu-id="6d1ac-187">No caso de uma ambiguidade, injeção de dependência gera uma exceção.</span><span class="sxs-lookup"><span data-stu-id="6d1ac-187">In case of an ambiguity, DI throws an exception.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6d1ac-188">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="6d1ac-188">Additional resources</span></span>

* [<span data-ttu-id="6d1ac-189">Injeção de dependência no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6d1ac-189">Dependency injection in ASP.NET Core</span></span>](/aspnet/core/fundamentals/dependency-injection)
