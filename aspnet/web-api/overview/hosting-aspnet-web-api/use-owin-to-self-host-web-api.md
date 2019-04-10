---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: Usar o OWIN para auto-hospedar a API Web ASP.NET - ASP.NET 4.x
author: rick-anderson
description: Tutorial com código que mostra como hospedar a API Web ASP.NET em um aplicativo de console.
ms.author: riande
ms.date: 07/09/2013
ms.custom: seoapril2019
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: a67db0bd061846af2db3599e0843ed7c6a22db1e
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59386509"
---
# <a name="use-owin-to-self-host-aspnet-web-api"></a>Usar o OWIN para auto-hospedar a API Web ASP.NET 


> Este tutorial mostra como hospedar a API Web ASP.NET em um aplicativo de console, usando o OWIN para auto-hospedar a estrutura da API Web.
>
> [Open Web Interface para .NET](http://owin.org) (OWIN) define uma abstração entre servidores de web do .NET e aplicativos da web. OWIN separa o aplicativo web do servidor, o que torna o OWIN ideal para auto-hospedagem em um aplicativo web em seu próprio processo, fora do IIS.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
>
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) 
> - Web API 5.2.7


> [!NOTE]
> Você pode encontrar o código-fonte completo para este tutorial em [github.com/aspnet/samples](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/OwinSelfhostSample).


## <a name="create-a-console-application"></a>Criar um aplicativo de console

Sobre o **arquivo** menu, **New**, em seguida, selecione **projeto**. Partir **instalados**, em **Visual C#** , selecione **área de trabalho do Windows** e, em seguida, selecione **aplicativo de Console (.Net Framework)**. Nomeie o projeto "OwinSelfhostSample" e selecione **Okey**.

[![](use-owin-to-self-host-web-api/_static/image7.png)](use-owin-to-self-host-web-api/_static/image7.png)

## <a name="add-the-web-api-and-owin-packages"></a>Adicione os pacotes de API da Web e OWIN

Dos **ferramentas** menu, selecione **Gerenciador de pacotes NuGet**, em seguida, selecione **Package Manager Console**. Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

Isso instalará o pacote de selfhost WebAPI OWIN e todos os pacotes necessários do OWIN.

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a>Configurar API da Web para hospedar internamente

No Gerenciador de soluções, clique com botão direito no projeto e selecione **Add** / **classe** para adicionar uma nova classe. Nomeie a classe `Startup`.

![](use-owin-to-self-host-web-api/_static/image5.png)

Substitua todo o código clichê nesse arquivo com o seguinte:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a>Adicionar um controlador de API da Web

Em seguida, adicione uma classe de controlador de API da Web. No Gerenciador de soluções, clique com botão direito no projeto e selecione **Add** / **classe** para adicionar uma nova classe. Nomeie a classe `ValuesController`.

Substitua todo o código clichê nesse arquivo com o seguinte:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-with-httpclient"></a>Iniciar o Host do OWIN e fazer uma solicitação por HttpClient

Substitua todo o código clichê no arquivo Program.cs pelo seguinte:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="run-the-application"></a>Executar o aplicativo

Para executar o aplicativo, pressione F5 no Visual Studio. A saída deve se parecer com o seguinte:

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a>Recursos adicionais

[Uma visão geral do projeto Katana](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[Hospedar a API da Web ASP.NET em uma função de trabalho do Azure](host-aspnet-web-api-in-an-azure-worker-role.md)
