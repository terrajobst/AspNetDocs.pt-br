---
uid: signalr/overview/performance/scaleout-with-windows-azure-service-bus
title: Expansão do signalr com o barramento de serviço do Azure | Microsoft Docs
author: bradygaster
description: As versões de software usadas neste tópico Visual Studio 2013 o .NET 4,5 Signaler versão 2 versões anteriores deste tópico para a versão do Signalr 1. x deste tópico,...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: ce1305f9-30fd-49e3-bf38-d0a78dfb06c3
msc.legacyurl: /signalr/overview/performance/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: 73ed95c5027f57c7e390069dcb36b18a3714973f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579173"
---
# <a name="signalr-scaleout-with-azure-service-bus"></a>Expansão do SignalR com o Barramento de Serviço do Azure

por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

Neste tutorial, você implantará um aplicativo Signalr em uma função Web do Windows Azure, usando o backplane do barramento de serviço para distribuir mensagens para cada instância de função. (Você também pode usar o backplane do barramento de serviço com [aplicativos Web no serviço Azure app](https://docs.microsoft.com/azure/app-service-web/).)

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

Pré-requisitos:

- Uma conta do Windows Azure.
- O [SDK do Windows Azure](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).
- Visual Studio 2012 ou 2013.

O backplane do barramento de serviço também é compatível com o [barramento de serviço para Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), versão 1,1. No entanto, ele não é compatível com a versão 1,0 do barramento de serviço para Windows Server.

## <a name="pricing"></a>Preços

O backplane do barramento de serviço usa tópicos para enviar mensagens. Para obter as informações de preços mais recentes, consulte [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/). No momento da redação deste artigo, você pode enviar 1 milhão mensagens por mês para menos de $1. O backplane envia uma mensagem do barramento de serviço para cada invocação de um método de Hub do Signalr. Há também algumas mensagens de controle para conexões, desconexões, junção ou saída de grupos e assim por diante. Na maioria dos aplicativos, a maior parte do tráfego de mensagens será invocações de método de Hub.

## <a name="overview"></a>Visão geral

Antes de chegarmos ao tutorial detalhado, aqui está uma visão geral rápida do que você fará.

1. Use o portal do Azure do Windows para criar um novo namespace do barramento de serviço.
2. Adicione esses pacotes NuGet ao seu aplicativo: 

    - [Microsoft. AspNet. Signalr](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft. AspNet. signalr. ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) ou [Microsoft. AspNet. Signalr. ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)
3. Crie um aplicativo Signalr.
4. Adicione o seguinte código a Startup.cs para configurar o backplane: 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

Esse código configura o backplane com os valores padrão para [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) e [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx). Para obter informações sobre como alterar esses valores, consulte [desempenho do signalr: métricas de scale](signalr-performance.md#scaleout_metrics)out.

Para cada aplicativo, escolha um valor diferente para "Nomedoseuaplicativo". Não use o mesmo valor em vários aplicativos.

## <a name="create-the-azure-services"></a>Criar os serviços do Azure

Crie um serviço de nuvem, conforme descrito em [como criar e implantar um serviço de nuvem](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy). Siga as etapas na seção "como criar um serviço de nuvem usando a criação rápida". Para este tutorial, você não precisa carregar um certificado.

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

Crie um novo namespace do barramento de serviço, conforme descrito em [como usar os tópicos/assinaturas do barramento de serviço](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions). Siga as etapas na seção "criar um namespace de serviço".

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> Certifique-se de selecionar a mesma região para o serviço de nuvem e o namespace do barramento de serviço.

## <a name="create-the-visual-studio-project"></a>Criar o projeto do Visual Studio

Inicie o Visual Studio. No menu **Arquivo**, clique em **Novo Projeto**.

Na caixa de diálogo **novo projeto** , expanda **Visual C#** . Em **modelos instalados**, selecione **nuvem** e, em seguida, selecione **serviço de nuvem do Windows Azure**. Mantenha o .NET Framework padrão 4,5. Nomeie o aplicativo ChatService e clique em **OK**.

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

Na caixa de diálogo **novo serviço de nuvem do Windows Azure** , selecione função Web ASP.net. Clique no botão de seta para a direita ( **&gt;** ) para adicionar a função à sua solução.

Passe o mouse sobre a nova função, portanto, o ícone de lápis está visível. Clique nesse ícone para renomear a função. Nomeie a função "SignalRChat" e clique em **OK**.

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

Na caixa de diálogo **novo projeto ASP.net** , selecione **MVC**e clique em OK.

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

O assistente de projeto cria dois projetos:

- ChatService: este projeto é o aplicativo do Windows Azure. Ele define as funções do Azure e outras opções de configuração.
- SignalRChat: este projeto é seu projeto do ASP.NET MVC 5.

## <a name="create-the-signalr-chat-application"></a>Criar o aplicativo de chat do Signalr

Para criar o aplicativo de chat, siga as etapas no tutorial [introdução com signalr e MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).

Use o NuGet para instalar as bibliotecas necessárias. No menu **ferramentas** , selecione **Gerenciador de pacotes NuGet**e, em seguida, selecione **console do Gerenciador de pacotes**. Na janela do **console do Gerenciador de pacotes** , digite os seguintes comandos:

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

Use a opção `-ProjectName` para instalar os pacotes no projeto MVC do ASP.NET, em vez de no projeto do Windows Azure.

## <a name="configure-the-backplane"></a>Configurar o backplane

No arquivo Startup.cs do seu aplicativo, adicione o seguinte código:

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

Agora você precisa obter a cadeia de conexão do barramento de serviço. No portal do Azure, selecione o namespace do barramento de serviço que você criou e clique no ícone de chave de acesso.

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

Copie a cadeia de conexão para a área de transferência e cole-a na variável *ConnectionString* .

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a>Implantar no Azure

Em Gerenciador de Soluções, expanda a pasta **funções** dentro do projeto ChatService.

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

Clique com o botão direito do mouse na função SignalRChat e selecione **Propriedades**. Selecione a guia **configuração** . Em **instâncias** , selecione 2. Você também pode definir o tamanho da VM para **extra pequena**.

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

Salve as alterações.

Em Gerenciador de Soluções, clique com o botão direito do mouse no projeto ChatService. Selecione **Publicar**.

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

Se esta for a primeira vez que você publica no Windows Azure, você deve baixar suas credenciais. No assistente de **publicação** , clique em "entrar para baixar credenciais". Isso solicitará que você entre no portal do Azure do Windows e baixe um arquivo de configurações de publicação.

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

Clique em **importar** e selecione o arquivo de configurações de publicação que você baixou.

Clique em **Próximo**. Na caixa de diálogo **configurações de publicação** , em **serviço de nuvem**, selecione o serviço de nuvem que você criou anteriormente.

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

Clique em **Publicar**. Pode levar alguns minutos para implantar o aplicativo e iniciar as VMs.

Agora, quando você executa o aplicativo de chat, as instâncias de função se comunicam por meio do barramento de serviço do Azure, usando um tópico do barramento de serviço. Um tópico é uma fila de mensagens que permite vários assinantes.

O backplane cria automaticamente o tópico e as assinaturas. Para ver a atividade de assinaturas e mensagens, abra o portal do Azure, selecione o namespace do barramento de serviço e clique em "tópicos".

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

Reserve alguns minutos para que a atividade de mensagem seja exibida no painel.

![](scaleout-with-windows-azure-service-bus/_static/image15.png)

O signalr gerencia o tempo de vida do tópico. Desde que seu aplicativo seja implantado, não tente excluir manualmente os tópicos ou alterar as configurações no tópico.

## <a name="troubleshooting"></a>solução de problemas

**System. InvalidOperationException "o único IsolationLevel com suporte é ' IsolationLevel. Serializable '."**

Esse erro poderá ocorrer se o nível de transação de uma operação for definido como algo diferente de `Serializable`. Verifique se nenhuma operação está sendo executada com outros níveis de transação.
