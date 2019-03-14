---
ms.openlocfilehash: b6f39dd0dedb38961eb021cde355f7ec83283195
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57024863"
---
# <a name="aspnet-core-web-api-controller-sample"></a><span data-ttu-id="1c945-101">Exemplo de Controlador da API Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1c945-101">ASP.NET Core Web API Controller Sample</span></span>

<span data-ttu-id="1c945-102">Este aplicativo de exemplo consiste nos seguintes projetos:</span><span class="sxs-lookup"><span data-stu-id="1c945-102">This sample app consists of the following projects:</span></span>

- <span data-ttu-id="1c945-103">\**WebApiSample.Api.22*: Um projeto do ASP.NET Core 2.2 direcionado 2.2 do .NET Core.</span><span class="sxs-lookup"><span data-stu-id="1c945-103">\**WebApiSample.Api.22*: An ASP.NET Core 2.2 project targeting .NET Core 2.2.</span></span>
- <span data-ttu-id="1c945-104">**WebApiSample.Api.21**: Um projeto do ASP.NET Core 2.1 direcionando o .NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="1c945-104">**WebApiSample.Api.21**: An ASP.NET Core 2.1 project targeting .NET Core 2.1.</span></span>
- <span data-ttu-id="1c945-105">**WebApiSample.Api.Pre21**: Um projeto do ASP.NET Core 2.0 direcionado ao .NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="1c945-105">**WebApiSample.Api.Pre21**: An ASP.NET Core 2.0 project targeting .NET Core 2.0.</span></span>
- <span data-ttu-id="1c945-106">**WebApiSample.DataAccess**: Uma biblioteca de classes .NET Standard 2.0 que funciona como uma camada de acesso de dados para os projetos de API Web 2.</span><span class="sxs-lookup"><span data-stu-id="1c945-106">**WebApiSample.DataAccess**: A .NET Standard 2.0 class library serving as a data access tier for the 2 Web API projects.</span></span>

<span data-ttu-id="1c945-107">Este exemplo mostra variações da criação do controlador da API Web:</span><span class="sxs-lookup"><span data-stu-id="1c945-107">This sample illustrates variations of Web API controller creation:</span></span>

- [<span data-ttu-id="1c945-108">Derivar a classe do ControllerBase</span><span class="sxs-lookup"><span data-stu-id="1c945-108">Derive class from ControllerBase</span></span>](https://docs.microsoft.com/aspnet/core/web-api#derive-class-from-controllerbase)
- [<span data-ttu-id="1c945-109">Anotar classe com o ApiControllerAttribute</span><span class="sxs-lookup"><span data-stu-id="1c945-109">Annotate class with ApiControllerAttribute</span></span>](https://docs.microsoft.com/aspnet/core/web-api#annotate-class-with-apicontrollerattribute)
