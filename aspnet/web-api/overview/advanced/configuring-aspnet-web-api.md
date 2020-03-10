---
uid: web-api/overview/advanced/configuring-aspnet-web-api
title: Configurando ASP.NET Web API 2-ASP.NET 4. x
author: MikeWasson
description: 'Configurar ASP.NET Web API 2 para ASP.NET 4. x: definir configurações, ASP.NET 4. x hospedagem, OWIN hospedagem interna, serviços globais e configuração de pré-controlador.'
ms.author: riande
ms.date: 03/31/2014
ms.custom: seoapril2019
ms.assetid: 9e10a700-8d91-4d2e-a31e-b8b569fe867c
msc.legacyurl: /web-api/overview/advanced/configuring-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 4f76728fa5e4602e35e1b7cb2d41b2245093cad8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557718"
---
# <a name="configuring-aspnet-web-api-2"></a>Configurando o ASP.NET Web API 2

por [Mike Wasson](https://github.com/MikeWasson)

Este tópico descreve como configurar ASP.NET Web API.

- [Definições de configuração](#settings)
- [Configurando a API Web com hospedagem ASP.NET](#webhost)
- [Configurando a API Web com hospedagem interna OWIN](#selfhost)
- [Serviços de API Web globais](#services)
- [Configuração por controlador](#percontrollerconfig)

<a id="settings"></a>
## <a name="configuration-settings"></a>Definições de configuração

As definições de configuração da API Web são definidas na classe [HttpConfiguration](https://msdn.microsoft.com/library/system.web.http.httpconfiguration.aspx) .

| Membro | DESCRIÇÃO |
| --- | --- |
| **DependencyResolver** | Habilita a injeção de dependência para controladores. Consulte [usando o resolvedor de dependência da API Web](dependency-injection.md). |
| **Filtros** | Filtros de ação. |
| **Formatadores** | [Formatadores de tipo de mídia](../formats-and-model-binding/media-formatters.md). |
| **IncludeErrorDetailPolicy** | Especifica se o servidor deve incluir detalhes do erro, como mensagens de exceção e rastreamentos de pilha, em mensagens de resposta HTTP. Consulte [IncludeErrorDetailPolicy](https://msdn.microsoft.com/library/system.web.http.includeerrordetailpolicy(v=vs.108)). |
| **Inicializador** | Uma função que executa a inicialização final do **HttpConfiguration**. |
| **MessageHandlers** | [Manipuladores de mensagens http](http-message-handlers.md). |
| **ParameterBindingRules** | Uma coleção de regras para parâmetros de associação em ações do controlador. |
| **Propriedades** | Um recipiente de propriedades genérico. |
| **Rotas** | A coleção de rotas. Consulte [Roteamento em ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md). |
| **Serviços** | A coleção de serviços. Consulte [Serviços](#services). |

## <a name="prerequisites"></a>Prerequisites

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional ou Enterprise Edition.

<a id="webhost"></a>
## <a name="configuring-web-api-with-aspnet-hosting"></a>Configurando a API Web com hospedagem ASP.NET

Em um aplicativo ASP.NET, configure a API da Web chamando [GlobalConfiguration. Configure](https://msdn.microsoft.com/library/system.web.http.globalconfiguration.configure.aspx) no método **Start do aplicativo\_** . O método **Configure** usa um delegado com um único parâmetro do tipo **HttpConfiguration**. Execute toda a sua configuração dentro do delegado.

Veja um exemplo de como usar um delegado anônimo:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample1.cs)]

No Visual Studio 2017, o modelo de projeto "aplicativo Web ASP.NET" configura automaticamente o código de configuração, se você selecionar "API Web" na caixa de diálogo **novo projeto ASP.net** .

[![](configuring-aspnet-web-api/_static/image2.png)](configuring-aspnet-web-api/_static/image1.png)

O modelo de projeto cria um arquivo chamado WebApiConfig.cs dentro do aplicativo\_pasta inicial. Esse arquivo de código define o delegado em que você deve colocar o código de configuração da API Web.

![](configuring-aspnet-web-api/_static/image3.png)

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample2.cs?highlight=12)]

O modelo de projeto também adiciona o código que chama o delegado do **aplicativo\_iniciar**.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample3.cs?highlight=5)]

<a id="selfhost"></a>
## <a name="configuring-web-api-with-owin-self-hosting"></a>Configurando a API Web com hospedagem interna OWIN

Se você estiver hospedando internamente com o OWIN, crie uma nova instância do **HttpConfiguration** . Execute qualquer configuração nessa instância e, em seguida, passe a instância para o método de extensão **Owin. UseWebApi** .

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample4.cs)]

O tutorial [usar OWIN para auto-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md) mostra as etapas completas.

<a id="services"></a>
## <a name="global-web-api-services"></a>Serviços de API Web globais

A coleção **HttpConfiguration. Services** contém um conjunto de serviços globais que a API da Web usa para executar várias tarefas, como a seleção de controlador e a negociação de conteúdo.

> [!NOTE]
> A coleção de **Serviços** não é um mecanismo de finalidade geral para descoberta de serviço ou injeção de dependência. Ele armazena apenas os tipos de serviço que são conhecidos pela estrutura da API Web.

A coleção de **Serviços** é inicializada com um conjunto padrão de serviços e você pode fornecer suas próprias implementações personalizadas. Alguns serviços dão suporte a várias instâncias, enquanto outros podem ter apenas uma instância. (No entanto, você também pode fornecer serviços no nível do controlador; consulte [configuração por controlador](#percontrollerconfig).

Serviços de instância única

| Serviço | DESCRIÇÃO |
| --- | --- |
| **IActionValueBinder** | Obtém uma associação para um parâmetro. |
| **IApiExplorer** | Obtém descrições das APIs expostas pelo aplicativo. Consulte [criando uma página de ajuda para uma API da Web](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IAssembliesResolver** | Obtém uma lista dos assemblies para o aplicativo. Consulte [seleção de roteamento e ação](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IBodyModelValidator** | Valida um modelo que é lido a partir do corpo da solicitação por um formatador de tipo de mídia. |
| **IContentNegotiator** | Realiza a negociação de conteúdo. |
| **IDocumentationProvider** | Fornece documentação para APIs. O padrão é **nulo**. Consulte [criando uma página de ajuda para uma API da Web](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IHostBufferPolicySelector** | Indica se o host deve armazenar em buffer os corpos de entidade de mensagem HTTP. |
| **IHttpActionInvoker** | Invoca uma ação do controlador. Consulte [seleção de roteamento e ação](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpActionSelector** | Seleciona uma ação do controlador. Consulte [seleção de roteamento e ação](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerActivator** | Ativa um controlador. Consulte [seleção de roteamento e ação](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerSelector** | Seleciona um controlador. Consulte [seleção de roteamento e ação](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerTypeResolver** | Fornece uma lista dos tipos de controlador da API Web no aplicativo. Consulte [seleção de roteamento e ação](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **ITraceManager** | Inicializa a estrutura de rastreamento. Consulte [rastreamento em ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **ITraceWriter** | Fornece um gravador de rastreamento. O padrão é um gravador de rastreamento "não op". Consulte [rastreamento em ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **IModelValidatorCache** | Fornece um cache de validadores de modelo. |

Serviços de várias instâncias

|                 Serviço                 |                                                                                                              DESCRIÇÃO                                                                                                               |
|-----------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <strong>IFilterprovider</strong>     |                                                                                           Retorna uma lista de filtros para uma ação do controlador.                                                                                           |
|  <strong>ModelBinderProvider</strong>   |                                                                                                Retorna um associador de modelo para um determinado tipo.                                                                                                |
| <strong>ModelMetadataProvider</strong>  |                                                                                                     Fornece metadados para um modelo.                                                                                                     |
| <strong>ModelValidatorProvider</strong> |                                                                                                   Fornece um validador para um modelo.                                                                                                    |
|  <strong>ValueProviderFactory</strong>  | Cria um provedor de valor. Para obter mais informações, consulte a postagem no blog de Mike Stall [como criar um provedor de valor personalizado no WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx) |

Para adicionar uma implementação personalizada a um serviço de várias instâncias, chame **Adicionar** ou **Inserir** na coleção de **Serviços** :

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample5.cs)]

Para substituir um serviço de instância única por uma implementação personalizada, chame **replace** na coleção de **Serviços** :

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample6.cs)]

<a id="percontrollerconfig"></a>
## <a name="per-controller-configuration"></a>Configuração por controlador

Você pode substituir as seguintes configurações por controlador:

- Formatadores de tipo de mídia
- Regras de associação de parâmetro
- Serviços

Para fazer isso, defina um atributo personalizado que implemente a interface **IControllerConfiguration** . Em seguida, aplique o atributo ao controlador.

O exemplo a seguir substitui os formatadores de tipo de mídia padrão por um formatador personalizado.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample7.cs)]

O método **IControllerConfiguration. Initialize** usa dois parâmetros:

- Um objeto **HttpControllerSettings**
- Um objeto **HttpControllerDescriptor**

O **HttpControllerDescriptor** contém uma descrição do controlador, que você pode examinar para fins informativos (digamos, para distinguir entre dois controladores).

Use o objeto **HttpControllerSettings** para configurar o controlador. Esse objeto contém o subconjunto de parâmetros de configuração que você pode substituir por controlador. As configurações que você não alterar padrão para o objeto **HttpConfiguration** global.
