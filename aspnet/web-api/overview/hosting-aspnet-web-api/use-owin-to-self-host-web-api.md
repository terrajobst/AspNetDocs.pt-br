---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: Usar OWIN para auto-host ASP.NET Web API-ASP.NET 4. x
author: rick-anderson
description: Tutorial com código mostrando como hospedar ASP.NET Web API em um aplicativo de console.
ms.author: riande
ms.date: 07/09/2013
ms.custom: seoapril2019
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: 872b931391a63ef82b96e5b264c070c0b5e9605d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556535"
---
# <a name="use-owin-to-self-host-aspnet-web-api"></a>Usar o OWIN para hospedar automaticamente ASP.NET Web API 

> Este tutorial mostra como hospedar ASP.NET Web API em um aplicativo de console, usando OWIN para hospedar internamente a estrutura da API Web.
>
> O [Open Web interface for .net](http://owin.org) (OWIN) define uma abstração entre servidores Web e aplicativos Web do .net. OWIN dissocia o aplicativo Web do servidor, o que torna o OWIN ideal para hospedar internamente um aplicativo Web em seu próprio processo, fora do IIS.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
>
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) 
> - 5\.2.7 da API Web

> [!NOTE]
> Você pode encontrar o código-fonte completo deste tutorial em [github.com/ASPNET/Samples](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/OwinSelfhostSample).

## <a name="create-a-console-application"></a>Criar um aplicativo de console

No menu **arquivo** , **novo**, selecione **projeto**. Do **instalado**, em **Visual C#** , selecione **área de trabalho do Windows** e, em seguida, selecione **aplicativo de console (.NET Framework)** . Nomeie o projeto "OwinSelfhostSample" e selecione **OK**.

[![](use-owin-to-self-host-web-api/_static/image7.png)](use-owin-to-self-host-web-api/_static/image7.png)

## <a name="add-the-web-api-and-owin-packages"></a>Adicionar a API Web e os pacotes OWIN

No menu **ferramentas** , selecione **Gerenciador de pacotes NuGet**e, em seguida, selecione **console do Gerenciador de pacotes**. Na janela Console do Gerenciador de Pacotes, digite o seguinte comando:

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

Isso instalará o pacote WebAPI OWIN selfhost e todos os pacotes OWIN necessários.

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a>Configurar API Web para hospedagem interna

Em Gerenciador de Soluções, clique com o botão direito do mouse no projeto e selecione **adicionar** / **classe** para adicionar uma nova classe. Nome da classe `Startup`.

![](use-owin-to-self-host-web-api/_static/image5.png)

Substitua todo o código clichê deste arquivo pelo seguinte:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a>Adicionar um controlador de API Web

Em seguida, adicione uma classe de controlador da API Web. Em Gerenciador de Soluções, clique com o botão direito do mouse no projeto e selecione **adicionar** / **classe** para adicionar uma nova classe. Nome da classe `ValuesController`.

Substitua todo o código clichê deste arquivo pelo seguinte:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-with-httpclient"></a>Iniciar o host OWIN e fazer uma solicitação com HttpClient

Substitua todo o código clichê no arquivo Program.cs pelo seguinte:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="run-the-application"></a>Executar o aplicativo

Para executar o aplicativo, pressione F5 no Visual Studio. A saída deve ser semelhante ao seguinte:

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a>Recursos adicionais

[Uma visão geral do projeto Katana](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[ASP.NET Web API de host em uma função de trabalho do Azure](host-aspnet-web-api-in-an-azure-worker-role.md)
