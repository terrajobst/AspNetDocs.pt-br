---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Exemplos de Katana | Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 01/17/2014
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: 1238f7d09492a6856d49dece5de75184ccfa4838
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78584556"
---
# <a name="katana-samples"></a>Exemplos do Katana

pela [Microsoft](https://github.com/microsoft)

## <a name="katana-samples"></a>Exemplos do Katana

**Exemplo de rotas ASP.NET** | [código-fonte](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes)  
Em alguns aplicativos, você desejará conectar componentes do OWIN na tabela de rotas do Asp.Net lado a lado com componentes não OWIN. Este exemplo mostra como usar os métodos de extensão RouteCollection MapOwinPath e MapOwinRoute fornecidos pelo Microsoft. Owin. host. SystemWeb.

**Exemplo de pipelines de ramificação** | [código-fonte](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines)  
Os pipelines de processamento de solicitação OWIN não precisam ser lineares, eles podem ser ramificados para processar solicitações de maneiras diferentes. Este exemplo mostra como construir um pipeline de ramificação baseado em caminhos de solicitação ou outros dados de solicitação, como cabeçalhos. Esses componentes estão disponíveis no pacote NuGet Microsoft. Owin. Mapping.

**Exemplo de servidor personalizado** | [código-fonte](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer)   
Mostra como usar um servidor OWIN personalizado ao hospedar internamente o OWIN.

[Código-fonte](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded) de | de **exemplo inserido**  
Alguns servidores OWIN podem ser executados dentro de seu próprio processo (&quot;&quot;auto-hospedado). Este exemplo mostra como iniciar um aplicativo OWIN usando as ferramentas fornecidas pelo pacote NuGet Microsoft. Owin. Hosting.

[Código-fonte](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld) do | de **exemplo HelloWorld**  
OWIN é uma abstração de API de servidor HTTP que habilita a portabilidade do aplicativo entre vários servidores. Este exemplo demonstra como escrever um aplicativo Olá, Mundo usando alguns **wrappers simples** em relação à abstração OWIN bruta e executá-lo em um servidor web como ASP.net.

**Olá, Mundo | de exemplo OWIN bruto** de [código-fonte](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin)  
Este exemplo demonstra como gravar um aplicativo Olá, Mundo usando a abstração OWIN **bruta** e executá-lo em um servidor web como ASP.net.

[Código-fonte](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR) do | de **exemplo do signalr**  
Mostra como usar o OWIN/Katana por autoatendimento. Para obter mais informações sobre o Signalr de hospedagem interna, consulte [tutorial: auto-host do signalr](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

**Exemplos de arquivos estáticos** | [código-fonte](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample)   
Mostra como dar suporte a solicitações HTTP para arquivos estáticos usando OWIN/katana.

 | de [código-fonte](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi) **da API Web**   
Este exemplo mostra como hospedar OWIN no IIS e adicionar API Web ao pipeline do OWIN.

**Exemplo de soquete da Web** | [código-fonte](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample)   
Mostra como dar suporte a soquetes da Web no OWIN usando a classe [System .net. WebSockets. WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) .
