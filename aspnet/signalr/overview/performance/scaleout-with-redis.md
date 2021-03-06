---
uid: signalr/overview/performance/scaleout-with-redis
title: Expansão do signalr com Redis | Microsoft Docs
author: bradygaster
description: Versões de software usadas neste tópico Visual Studio 2013 o .NET 4,5 Signalr versão 2 versões anteriores deste tópico para obter informações sobre versões anteriores do...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 6ecd08c1-e364-4cd7-ad4c-806521911585
msc.legacyurl: /signalr/overview/performance/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 58a7affa1769523955adc76455a1c33be6f49751
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579222"
---
# <a name="signalr-scaleout-with-redis"></a>Expansão do SignalR com Redis

por [Mike Wasson](https://github.com/MikeWasson)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a>Versões de software usadas neste tópico
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - Sinalização versão 2,4
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Versões anteriores deste tópico
>
> Para obter informações sobre versões anteriores do Signalr, confira [versões mais antigas do signalr](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Perguntas e comentários
>
> Deixe comentários sobre como você gostou deste tutorial e o que poderíamos melhorar nos comentários na parte inferior da página. Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, poderá lançá-las no fórum do [signalr ASP.net](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [stackoverflow.com](http://stackoverflow.com/).

Neste tutorial, você usará o [Redis](http://redis.io/) para distribuir mensagens em um aplicativo signalr que é implantado em duas instâncias do IIS separadas.

Redis é um repositório de chave-valor na memória. Ele também dá suporte a um sistema de mensagens com um modelo de publicação/assinatura. O backplane Redis do Signalr usa o recurso pub/sub para encaminhar mensagens para outros servidores.

![](scaleout-with-redis/_static/image1.png)

Para este tutorial, você usará três servidores:

- Dois servidores executando o Windows, que serão usados para implantar um aplicativo Signalr.
- Um servidor que executa o Linux, que será usado para executar o Redis. Para as capturas de tela deste tutorial, usei o Ubuntu 12, 4 TLS.

Se você não tiver três servidores físicos para usar, poderá criar VMs no Hyper-V. Outra opção é criar VMs no Azure.

Embora este tutorial use a implementação oficial do Redis, também há uma [porta do Windows de Redis](https://github.com/MSOpenTech/redis) do MSOpenTech. A instalação e a configuração são diferentes, mas, caso contrário, as etapas são as mesmas.

> [!NOTE]
>
> O dimensionamento do signalr com Redis não dá suporte a clusters Redis.

## <a name="overview"></a>Visão geral

Antes de chegarmos ao tutorial detalhado, aqui está uma visão geral rápida do que você fará.

1. Instale o Redis e inicie o servidor Redis.
2. Adicione esses pacotes NuGet ao seu aplicativo:

    - [Microsoft. AspNet. Signalr](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft. AspNet. Signalr. StackExchangeRedis](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.StackExchangeRedis)
    
3. Crie um aplicativo Signalr.
4. Adicione o seguinte código a Startup.cs para configurar o backplane:

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a>Ubuntu no Hyper-V

Usando o Windows Hyper-V, você pode criar facilmente uma VM do Ubuntu no Windows Server.

Baixe o ISO do Ubuntu de [http://www.ubuntu.com](http://www.ubuntu.com/).

No Hyper-V, adicione uma nova VM. Na etapa **conectar disco rígido virtual** , selecione **criar um disco rígido virtual**.

![](scaleout-with-redis/_static/image2.png)

Na etapa **Opções de instalação** , selecione **arquivo de imagem (. ISO)** , clique em **procurar**e navegue até o ISO de instalação do Ubuntu.

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a>Instalar o Redis

Siga as etapas em [http://redis.io/download](http://redis.io/download) para baixar e compilar o Redis.

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

Isso cria os binários Redis no diretório `src`.

Por padrão, o Redis não requer uma senha. Para definir uma senha, edite o arquivo `redis.conf`, localizado no diretório raiz do código-fonte. (Faça uma cópia de backup do arquivo antes de editá-lo!) Adicione a seguinte diretiva a `redis.conf`:

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

Agora, inicie o servidor Redis:

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

Abra a porta 6379, que é a porta padrão que o Redis escuta. (Você pode alterar o número da porta no arquivo de configuração.)

## <a name="create-the-signalr-application"></a>Criar o aplicativo Signalr

Crie um aplicativo Signalr seguindo um destes tutoriais:

- [Introdução com Signalr 2,0](../getting-started/tutorial-getting-started-with-signalr.md)
- [Introdução com Signalr 2,0 e MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

Em seguida, modificaremos o aplicativo de chat para dar suporte ao ScaleOut com Redis. Primeiro, adicione o pacote NuGet `Microsoft.AspNet.SignalR.StackExchangeRedis` ao seu projeto. No Visual Studio, no menu **ferramentas** , selecione **Gerenciador de pacotes NuGet**e, em seguida, selecione **console do Gerenciador de pacotes**. Na janela Console do Gerenciador de Pacotes, digite o seguinte comando:

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

Em seguida, abra o arquivo Startup.cs. Adicione o seguinte código ao método de **configuração** :

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- "servidor" é o nome do servidor que está executando o Redis.
- *Port* é o número da porta
- "password" é a senha que você definiu no arquivo Redis. conf.
- "AppName" é qualquer cadeia de caracteres. O signalr cria um Redis pub/sub Channel com esse nome.

Por exemplo:

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a>Implantar e executar o aplicativo

Prepare suas instâncias do Windows Server para implantar o aplicativo Signalr.

Adicione a função IIS. Inclua recursos de "desenvolvimento de aplicativos", incluindo o protocolo WebSocket.

![](scaleout-with-redis/_static/image5.png)

Inclua também o serviço de gerenciamento (listado em "ferramentas de gerenciamento").

![](scaleout-with-redis/_static/image6.png)

**Instale o Implantação da Web 3,0.** Quando você executar o Gerenciador do IIS, ele solicitará que você instale a plataforma Web da Microsoft ou você poderá [baixar o instalador](https://go.microsoft.com/fwlink/?LinkId=255386). No instalador da plataforma, procure Implantação da Web e instale o Implantação da Web 3,0

![](scaleout-with-redis/_static/image7.png)

Verifique se o serviço de gerenciamento da Web está em execução. Caso contrário, inicie o serviço. (Se você não vir o serviço de gerenciamento da Web na lista de serviços do Windows, certifique-se de que você instalou o serviço de gerenciamento quando adicionou a função do IIS.)

Por padrão, o serviço de gerenciamento da Web escuta na porta TCP 8172. No firewall do Windows, crie uma nova regra de entrada para permitir o tráfego TCP na porta 8172. Para obter mais informações, consulte [Configurando regras de firewall](https://technet.microsoft.com/library/dd448559(WS.10).aspx). (Se você estiver hospedando as VMs no Azure, poderá fazer isso diretamente na portal do Azure. Consulte [como configurar pontos de extremidade para uma máquina virtual](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)

Agora você está pronto para implantar o projeto do Visual Studio do computador de desenvolvimento no servidor. Em Gerenciador de Soluções, clique com o botão direito do mouse na solução e clique em **publicar**.

Para obter uma documentação mais detalhada sobre a implantação da Web, consulte [mapa de conteúdo de implantação da Web para Visual Studio e ASP.net](../../../whitepapers/aspnet-web-deployment-content-map.md).

Se você implantar o aplicativo em dois servidores, poderá abrir cada instância em uma janela separada do navegador e ver que cada uma recebe mensagens do Signalr da outra. (Naturalmente, em um ambiente de produção, os dois servidores ficam atrás de um balanceador de carga.)

![](scaleout-with-redis/_static/image8.png)

Se você estiver curioso para ver as mensagens que são enviadas para o Redis, você pode usar o cliente **Redis-CLI** , que é instalado com o Redis.

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
