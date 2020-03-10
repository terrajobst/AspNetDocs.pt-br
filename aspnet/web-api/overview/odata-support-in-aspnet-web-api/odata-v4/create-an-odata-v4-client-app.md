---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: Criar um aplicativo cliente do OData v4C#() | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/26/2014
ms.assetid: 47202362-3808-4add-9a69-c9d1f91d5e4e
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
msc.type: authoredcontent
ms.openlocfilehash: a0016cf2cc7bffe6268664395ccb38e140090310
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556290"
---
# <a name="create-an-odata-v4-client-app-c"></a>Criar um aplicativo cliente do OData v4 (C#)

por [Mike Wasson](https://github.com/MikeWasson)

No tutorial anterior, você criou um serviço OData básico que oferece suporte a operações CRUD. Agora, vamos criar um cliente para o serviço.

Inicie uma nova instância do Visual Studio e crie um novo projeto de aplicativo de console. Na caixa de diálogo **novo projeto** , selecione **modelos** de &gt; **instalados** &gt;  **C# Visual** &gt; **área de trabalho do Windows**e selecione o modelo aplicativo de **console** . Nomeie o projeto &quot;ProductsApp&quot;.

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> Você também pode adicionar o aplicativo de console à mesma solução do Visual Studio que contém o serviço OData.

## <a name="install-the-odata-client-code-generator"></a>Instalar o gerador de código do cliente OData

No menu **Ferramentas**, selecione **Extensões e atualizações**. Selecione **Online** &gt; **Galeria do Visual Studio**. Na caixa de pesquisa, pesquise &quot;gerador de código do cliente OData&quot;. Clique em **baixar** para instalar o VSIX. Talvez seja solicitado que você reinicie o Visual Studio.

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a>Executar o serviço OData localmente

Execute o projeto ProductService do Visual Studio. Por padrão, o Visual Studio inicia um navegador na raiz do aplicativo. Observe o URI; Isso será necessário na próxima etapa. Deixe o aplicativo em execução.

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> Se você colocar os dois projetos na mesma solução, certifique-se de executar o projeto ProductService sem depuração. Na próxima etapa, você precisará manter o serviço em execução enquanto modifica o projeto de aplicativo de console.

## <a name="generate-the-service-proxy"></a>Gerar o proxy de serviço

O proxy de serviço é uma classe .NET que define métodos para acessar o serviço OData. O proxy traduz as chamadas de método em solicitações HTTP. Você criará a classe proxy executando um [modelo T4](https://msdn.microsoft.com/library/bb126445.aspx).

Clique com o botão direito do mouse no projeto. Selecione **adicionar** &gt; **novo item**.

![](create-an-odata-v4-client-app/_static/image5.png)

Na caixa de diálogo **Adicionar novo item** , **selecione C# itens visuais** &gt; **código** &gt; **cliente OData**. Nomeie o modelo &quot;ProductClient.tt&quot;. Clique em **Adicionar** e clique no aviso de segurança.

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

Neste ponto, você receberá um erro, que pode ser ignorado. O Visual Studio executa automaticamente o modelo, mas o modelo precisa de algumas definições de configuração primeiro.

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

Abra o arquivo ProductClient. OData. config. No elemento `Parameter`, Cole o URI do projeto ProductService (etapa anterior). Por exemplo:

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

Execute o modelo novamente. Em Gerenciador de Soluções, clique com o botão direito do mouse no arquivo ProductClient.tt e selecione **executar ferramenta personalizada**.

O modelo cria um arquivo de código chamado ProductClient.cs que define o proxy. Ao desenvolver seu aplicativo, se você alterar o ponto de extremidade OData, execute o modelo novamente para atualizar o proxy.

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a>Usar o proxy de serviço para chamar o serviço OData

Abra o arquivo Program.cs e substitua o código clichê pelo seguinte.

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

Substitua o valor de *ServiceUri* pelo URI de serviço anterior.

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

Quando você executa o aplicativo, ele deve gerar o seguinte:

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]
