---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: Usando o Signalr com aplicativos Web no serviço Azure App | Microsoft Docs
author: bradygaster
description: Este documento descreve como configurar um aplicativo Signalr que é executado no Microsoft Azure. Versões de software usadas no tutorial Visual Studio 2013 ou vis...
ms.author: bradyg
ms.date: 07/01/2015
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: 0e6a18bdbe9cc47e89b5a458753845afb53595f1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558698"
---
# <a name="using-signalr-with-web-apps-in-azure-app-service"></a>Usar o SignalR com aplicativos Web no Serviço de Aplicativo do Azure

por [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Este documento descreve como configurar um aplicativo Signalr que é executado no Microsoft Azure.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) ou Visual Studio 2012
> - .NET 4.5
> - Sinalização versão 2
> - SDK do Azure 2,3 para Visual Studio 2013 ou 2012
>
>
>
> ## <a name="questions-and-comments"></a>Perguntas e comentários
>
> Deixe comentários sobre como você gostou deste tutorial e o que poderíamos melhorar nos comentários na parte inferior da página. Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, você poderá lançá-las no [Fórum do signalr ASP.net](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), no [stackoverflow.com](http://stackoverflow.com/)ou nos [fóruns de Microsoft Azure](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).

## <a name="table-of-contents"></a>Sumário

- [Introdução](#introduction)
- [Implantando um aplicativo Web sinalizador no serviço Azure App](#deploying)
- [Habilitando WebSockets no serviço Azure App](#websocket)
- [Usando o backplane do cache Redis do Azure](#backplane)
- [Próximas etapas](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a>Introdução

O Signalr ASP.NET pode ser usado para trazer um novo nível de interatividade entre servidores e clientes Web ou .NET. Quando hospedados no Azure, os aplicativos Signalr podem aproveitar o ambiente altamente disponível, escalonável e de alto desempenho que é executado na nuvem.

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a>Implantando um aplicativo Web sinalizador no serviço Azure App

O signalr não adiciona complicações específicas à implantação de um aplicativo no Azure em vez de implantar em um servidor local. Um aplicativo que usa o Signalr pode ser hospedado no Azure sem nenhuma alteração na configuração ou outras configurações (no entanto, para suporte a WebSockets, consulte [habilitando WebSockets no serviço Azure app](#websocket) abaixo.) Para este tutorial, você implantará o aplicativo criado no [tutorial introdução](../getting-started/tutorial-getting-started-with-signalr.md) no Azure.

**Pré-requisitos**

- Visual Studio 2013. Se você não tiver o Visual Studio, o Visual Studio 2013 Express para Web estará incluído na instalação do SDK do Azure.
- [SDK do azure 2,3 para Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) ou [sdk do Azure 2,3 para Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).
- Para concluir este tutorial, você precisará de uma assinatura do Azure. Você pode [ativar seus benefícios de assinante do MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)ou [inscrever-se para uma assinatura de avaliação](https://azure.microsoft.com/pricing/free-trial/).

### <a name="deploying-a-signalr-web-app-to-azure"></a>Implantando um aplicativo Web sinalizador no Azure

1. Conclua o [tutorial de introdução](../getting-started/tutorial-getting-started-with-signalr.md)ou baixe o projeto concluído na Galeria de [códigos](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).
2. No Visual Studio, selecione **criar**, **publicar o chat do signalr**.
3. Na caixa de diálogo "publicar Web", selecione "sites do Windows Azure".

    ![Selecionar sites do Azure](using-signalr-with-azure-web-sites/_static/image1.png)
4. Se você não estiver conectado à sua conta Microsoft, clique em **entrar...** na caixa de diálogo "Selecionar site existente" e entre.

    ![Selecionar site existente](using-signalr-with-azure-web-sites/_static/image2.png)    ![Entrar no Azure](using-signalr-with-azure-web-sites/_static/image3.png)
5. Na caixa de diálogo "Selecionar site da Web existente", clique em **novo**.

    ![Novo Site](using-signalr-with-azure-web-sites/_static/image4.png)
6. Na caixa de diálogo "criar site no Windows Azure", insira um nome de aplicativo exclusivo. Selecione a região mais próxima de você na lista suspensa região. Clique em **Criar**.

    ![Criar um site no Azure](using-signalr-with-azure-web-sites/_static/image5.png)
7. Na caixa de diálogo "publicar Web", clique em **publicar**.

    ![Publicar site](using-signalr-with-azure-web-sites/_static/image6.png)
8. Quando o aplicativo concluir a publicação, o aplicativo de chat do Signalr hospedado em aplicativos Web do serviço Azure App será aberto em um navegador.

    ![Abertura de site em um navegador](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a>Habilitando WebSockets em aplicativos Web do Azure App Service

O WebSockets precisa estar explicitamente habilitado em seu aplicativo Web para ser usado em um aplicativo Signalr; caso contrário, outros protocolos serão usados (consulte [transportes e fallbacks](../getting-started/introduction-to-signalr.md#transports) para obter detalhes).

Para usar Websockets em Azure App aplicativos Web do serviço, habilite-os na seção de configuração do aplicativo Web. Para fazer isso, abra seu aplicativo Web no [portal de gerenciamento do Azure](https://manage.windowsazure.com/)e selecione Configurar.

![Guia Configurar](using-signalr-with-azure-web-sites/_static/image8.png)

Na parte superior da página de configuração, verifique se o .NET 4,5 é usado para seu aplicativo Web.

![Configuração do .NET Framework versão 4,5](using-signalr-with-azure-web-sites/_static/image9.png)

Na página configuração, na configuração **WebSockets** , selecione **ativado**.

![Configuração de WebSockets: ativado](using-signalr-with-azure-web-sites/_static/image10.png)

Na parte inferior da página de configuração, selecione **salvar** para salvar as alterações.

![Salvar configurações](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a>Usando o backplane do cache Redis do Azure

Se você usar várias instâncias para seu aplicativo Web, e os usuários dessas instâncias precisarem interagir umas com as outras (de modo que, por exemplo, as mensagens de bate-papo criadas em uma instância possam acessar os usuários conectados a outras instâncias), o [backplane do cache Redis do Azure](../performance/scaleout-with-redis.md) deve ser implementado em seu aplicativo.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Próximas etapas

Para obter mais informações sobre aplicativos Web no serviço Azure App, consulte [visão geral de aplicativos Web](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).
