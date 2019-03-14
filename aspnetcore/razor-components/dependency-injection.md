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
# <a name="razor-components-dependency-injection"></a>Injeção de dependência de componentes do Razor

Por [Rainer Stropek](https://www.timecockpit.com)

[Injeção de dependência (DI)](/aspnet/core/fundamentals/dependency-injection) é interno. Aplicativos podem usar os serviços internos mantendo-os injetados em componentes. Aplicativos também podem definir serviços personalizados e disponibilizá-las por meio da DI.

## <a name="dependency-injection"></a>Injeção de dependência

Injeção de dependência é uma técnica para acessar os serviços configurados em um local central. Isso pode ser útil para:

* Compartilhar uma única instância de uma classe de serviço em vários componentes (conhecido como um *singleton* service).
* Desacoplar componentes das classes concretas de serviço específico e fazer referência apenas abstrações. Por exemplo, uma interface `IDataAccess` é implementado por uma classe concreta `DataAccess`. Quando um componente usa DI para receber um `IDataAccess` implementação, o componente não está acoplada ao tipo concreto. A implementação pode ser trocada, talvez uma implementação fictícia em testes de unidade.

O sistema de DI é responsável por fornecer instâncias dos serviços de componentes. Injeção de dependência também resolve as dependências recursivamente, de modo que os próprios serviços podem depender mais serviços. Injeção de dependência é configurada durante a inicialização do aplicativo. Um exemplo é mostrado neste tópico.

## <a name="add-services-to-di"></a>Adicionar serviços para injeção de dependência

Depois de criar um novo aplicativo, examine o `Startup.ConfigureServices` método:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

O `ConfigureServices` método recebe um [IServiceCollection](/dotnet/api/microsoft.extensions.dependencyinjection.iservicecollection), que é uma lista de objetos do descritor de serviço ([ServiceDescriptor](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor)). Serviços são adicionados, fornecendo os descritores de serviço para a coleção de serviço. O exemplo de código a seguir demonstra o conceito:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

Serviços podem ser configurados com os seguintes tempos de vida:

| Método      | Descrição |
| ----------- | ----------- |
| [Singleton](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor.singleton#Microsoft_Extensions_DependencyInjection_ServiceDescriptor_Singleton__1_System_Func_System_IServiceProvider___0__) | Injeção de dependência cria uma *única instância* do serviço. Todos os componentes que exigem esse serviço recebem uma referência a essa instância. |
| [Transitório](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor.transient) | Sempre que um componente exige esse serviço, ele recebe um *nova instância* do serviço. |
| [Com escopo](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor.scoped) | Blazor do lado do cliente atualmente não tem o conceito de escopos de injeção de dependência. `Scoped` se comporta como `Singleton`. No entanto, os componentes do ASP.NET Core Razor suportam o `Scoped` tempo de vida. Em um componente do Razor, um registro de serviço com escopo está no escopo para a conexão. Por esse motivo, é preferencial para os serviços que devem ser direcionados para o usuário atual usar os serviços com escopo (mesmo se a intenção atual é executar o lado do cliente no navegador). |

O sistema de DI é baseado no sistema de DI no ASP.NET Core. Para obter mais informações, consulte [injeção de dependência no ASP.NET Core](/aspnet/core/fundamentals/dependency-injection).

## <a name="default-services"></a>Serviços padrão

Os serviços padrão são automaticamente adicionados à coleção de serviços de um aplicativo. A tabela a seguir mostra alguns dos serviços padrão útil fornecidos.

| Método       | Descrição |
| ------------ | ----------- |
| [HttpClient](/dotnet/api/system.net.http.httpclient) | Fornece métodos para enviar solicitações HTTP e receber respostas HTTP de um recurso identificado por um URI (singleton). Observe que esta instância do `HttpClient` usa o navegador para tratar o tráfego HTTP em segundo plano. [HttpClient.BaseAddress](/dotnet/api/system.net.http.httpclient.baseaddress) é definida automaticamente como o prefixo URI base do aplicativo. `HttpClient` é fornecida apenas para aplicativos do lado do cliente Blazor. |
| `IJSRuntime` | Representa uma instância de um tempo de execução do JavaScript para o qual chamadas podem ser distribuídas. Para obter mais informações, consulte <xref:razor-components/javascript-interop>. |
| `IUriHelper` | Auxiliares para trabalhar com o estado de navegação e URIs (singleton). `IUriHelper` é fornecido para ambos os aplicativos do lado do cliente Blazor e componentes do ASP.NET Core Razor. |

Observe que é possível usar um provedor de serviços personalizados em vez do provedor de serviços padrão que é adicionado pelo modelo padrão. Um provedor de serviços personalizados automaticamente não fornece os serviços padrão listados na tabela. Esses serviços devem ser adicionados explicitamente para o novo provedor de serviço.

## <a name="request-a-service-in-a-component"></a>Um serviço em um componente de solicitação

Depois que os serviços são adicionados à coleção de serviços, pode ser injetados em modelos do Razor dos componentes usando o `@inject` diretiva do Razor. `@inject` tem dois parâmetros:

* Nome do tipo: O tipo do serviço para injetar.
* Nome da propriedade: O nome da propriedade que está recebendo o serviço de aplicativo injetado. Observe que a propriedade não exige a criação manual. O compilador cria a propriedade.

Vários `@inject` instruções podem ser usadas para inserir serviços diferentes.

O exemplo a seguir mostra como usar `@inject`. O serviço que implementa `Services.IDataAccess` é injetado na propriedade do componente `DataRepository`. Observe como o código está usando apenas o `IDataAccess` abstração:

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

Internamente, a propriedade gerada (`DataRepository`) é decorado com o `InjectAttribute` atributo. Normalmente, esse atributo não é usado diretamente. Se uma classe base é necessária para componentes e injetadas propriedades também são necessárias para a classe base, `InjectAttribute` podem ser adicionadas manualmente:

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

Em componentes derivados da classe base, o `@inject` diretiva não é necessária. O `InjectAttribute` da classe base é suficiente:

```csharp
@page "/demo"
@inherits ComponentBase

<h1>...</h1>
...
```

## <a name="dependency-injection-in-services"></a>Injeção de dependência nos serviços

Serviços complexos podem exigir serviços adicionais. No exemplo anterior, `DataAccess` pode exigir o `HttpClient` serviço padrão. `@inject` ou o `InjectAttribute` não pode ser usado nos serviços. *Injeção de construtor* deve ser usado em vez disso. Os serviços necessários são adicionados, adicionando parâmetros ao construtor do serviço. Quando a injeção de dependência cria o serviço, ele reconhece os serviços que ele requer no construtor e fornece-os adequadamente.

O exemplo de código a seguir demonstra o conceito:

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

Observe os seguintes pré-requisitos para injeção de construtor:

* Deve haver um construtor cujos argumentos podem ser atendidos pela injeção de dependência. Observe que os parâmetros adicionais não cobertos pela DI sejam permitidos se valores padrão são especificados para elas.
* O construtor aplicável deve ser *público*.
* Deve haver apenas um construtor aplicável. No caso de uma ambiguidade, injeção de dependência gera uma exceção.

## <a name="additional-resources"></a>Recursos adicionais

* [Injeção de dependência no ASP.NET Core](/aspnet/core/fundamentals/dependency-injection)
