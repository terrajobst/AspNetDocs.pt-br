---
uid: signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
title: Expansão do signalr com o barramento de serviço do Azure (Signalr 1. x) | Microsoft Docs
author: bradygaster
description: ''
ms.author: bradyg
ms.date: 05/01/2013
ms.assetid: 501db899-e68c-49ff-81b2-1dc561bfe908
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: e64f84db00b571c01ea52f48d1ac1af46698d391
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558411"
---
# <a name="signalr-scaleout-with-azure-service-bus-signalr-1x"></a>Expansão do SignalR com o Barramento de Serviço do Azure (SignalR 1.x)

por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

Neste tutorial, você implantará um aplicativo Signalr em uma função Web do Windows Azure, usando o backplane do barramento de serviço para distribuir mensagens para cada instância de função.

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

Pré-requisitos:

- Uma conta do Windows Azure.
- O [SDK do Windows Azure](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).
- Visual Studio 2012.

O backplane do barramento de serviço também é compatível com o [barramento de serviço para Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), versão 1,1. No entanto, ele não é compatível com a versão 1,0 do barramento de serviço para Windows Server.

## <a name="pricing"></a>Preços

O backplane do barramento de serviço usa tópicos para enviar mensagens. Para obter as informações de preços mais recentes, consulte [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/). No momento da redação deste artigo, você pode enviar 1 milhão mensagens por mês para menos de $1. O backplane envia uma mensagem do barramento de serviço para cada invocação de um método de Hub do Signalr. Há também algumas mensagens de controle para conexões, desconexões, junção ou saída de grupos e assim por diante. Na maioria dos aplicativos, a maior parte do tráfego de mensagens será invocações de método de Hub.

## <a name="overview"></a>Visão geral

Antes de chegarmos ao tutorial detalhado, aqui está uma visão geral rápida do que você fará.

1. Use o portal do Azure do Windows para criar um novo namespace do barramento de serviço.
2. Adicione esses pacotes NuGet ao seu aplicativo: 

    - [Microsoft. AspNet. Signalr](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft. AspNet. Signalr. ServiceBus](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. Crie um aplicativo Signalr.
4. Adicione o seguinte código ao global. asax para configurar o backplane: 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

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

Na caixa de diálogo **novo serviço de nuvem do Windows Azure** , selecione função Web do ASP.NET MVC 4. Clique no botão de seta para a direita ( **&gt;** ) para adicionar a função à sua solução.

Passe o mouse sobre a nova função, portanto, o ícone de lápis está visível. Clique nesse ícone para renomear a função. Nomeie a função "SignalRChat" e clique em **OK**.

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

No assistente **novo projeto do ASP.NET MVC 4** , selecione **aplicativo de Internet**. Clique em **OK**. O assistente de projeto cria dois projetos:

- ChatService: este projeto é o aplicativo do Windows Azure. Ele define as funções do Azure e outras opções de configuração.
- SignalRChat: este projeto é seu projeto do ASP.NET MVC 4.

## <a name="create-the-signalr-chat-application"></a>Criar o aplicativo de chat do Signalr

Para criar o aplicativo de chat, siga as etapas no tutorial [introdução com signalr e MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).

Use o NuGet para instalar as bibliotecas necessárias. No menu **ferramentas** , selecione **Gerenciador de pacotes NuGet**e, em seguida, selecione **console do Gerenciador de pacotes**. Na janela do **console do Gerenciador de pacotes** , digite os seguintes comandos:

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

Use a opção `-ProjectName` para instalar os pacotes no projeto MVC do ASP.NET, em vez de no projeto do Windows Azure.

## <a name="configure-the-backplane"></a>Configurar o backplane

No arquivo global. asax do seu aplicativo, adicione o seguinte código:

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

Agora você precisa obter a cadeia de conexão do barramento de serviço. No portal do Azure, selecione o namespace do barramento de serviço que você criou e clique no ícone de chave de acesso.

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

Copie a cadeia de conexão para a área de transferência e cole-a na variável *ConnectionString* .

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a>Implantar no Azure

Em Gerenciador de Soluções, expanda a pasta **funções** dentro do projeto ChatService.

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

Clique com o botão direito do mouse na função SignalRChat e selecione **Propriedades**. Selecione a guia **configuração** . Em **instâncias** , selecione 2. Você também pode definir o tamanho da VM para **extra pequena**.

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

Salve as alterações.

Em Gerenciador de Soluções, clique com o botão direito do mouse no projeto ChatService. Selecione **Publicar**.

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

Se esta for a primeira vez que você publica no Windows Azure, você deve baixar suas credenciais. No assistente de **publicação** , clique em "entrar para baixar credenciais". Isso solicitará que você entre no portal do Azure do Windows e baixe um arquivo de configurações de publicação.

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

Clique em **importar** e selecione o arquivo de configurações de publicação que você baixou.

Clique em **Próximo**. Na caixa de diálogo **configurações de publicação** , em **serviço de nuvem**, selecione o serviço de nuvem que você criou anteriormente.

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

Clique em **Publicar**. Pode levar alguns minutos para implantar o aplicativo e iniciar as VMs.

Agora, quando você executa o aplicativo de chat, as instâncias de função se comunicam por meio do barramento de serviço do Azure, usando um tópico do barramento de serviço. Um tópico é uma fila de mensagens que permite vários assinantes.

O backplane cria automaticamente o tópico e as assinaturas. Para ver a atividade de assinaturas e mensagens, abra o portal do Azure, selecione o namespace do barramento de serviço e clique em "tópicos".

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

Reserve alguns minutos para que a atividade de mensagem seja exibida no painel.

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

O signalr gerencia o tempo de vida do tópico. Desde que seu aplicativo seja implantado, não tente excluir manualmente os tópicos ou alterar as configurações no tópico.
