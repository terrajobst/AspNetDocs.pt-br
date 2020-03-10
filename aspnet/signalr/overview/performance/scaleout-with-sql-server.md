---
uid: signalr/overview/performance/scaleout-with-sql-server
title: Dimensionamento do signalr com SQL Server | Microsoft Docs
author: bradygaster
description: Versões de software usadas neste tópico Visual Studio 2013 o .NET 4,5 Signalr versão 2 versões anteriores deste tópico para obter informações sobre versões anteriores do...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 98358b6e-9139-4239-ba3a-2d7dd74dd664
msc.legacyurl: /signalr/overview/performance/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 709a9ebf8f3396842bee0d87e621c00ae1418ec1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579180"
---
# <a name="signalr-scaleout-with-sql-server"></a>Expansão do SignalR com o SQL Server

por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a>Versões de software usadas neste tópico
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - Sinalização versão 2
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

Neste tutorial, você usará SQL Server para distribuir mensagens em um aplicativo Signalr que é implantado em duas instâncias do IIS separadas. Você também pode executar este tutorial em um único computador de teste, mas para obter o efeito completo, você precisa implantar o aplicativo Signalr em dois ou mais servidores. Você também deve instalar o SQL Server em um dos servidores ou em um servidor dedicado separado. Outra opção é executar o tutorial usando VMs no Azure.

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a>Prerequisites

Microsoft SQL Server 2005 ou posterior. O backplane dá suporte a edições de desktop e de servidor do SQL Server. Ele não dá suporte à edição SQL Server Compact ou ao banco de dados SQL do Azure. (Se seu aplicativo estiver hospedado no Azure, considere o backplane do barramento de serviço.)

## <a name="overview"></a>Visão geral

Antes de chegarmos ao tutorial detalhado, aqui está uma visão geral rápida do que você fará.

1. Crie um novo banco de dados vazio. O backplane criará as tabelas necessárias neste banco de dados.
2. Adicione esses pacotes NuGet ao seu aplicativo:

    - [Microsoft. AspNet. Signalr](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft. AspNet. Signalr. SqlServer](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. Crie um aplicativo Signalr.
4. Adicione o seguinte código a Startup.cs para configurar o backplane:

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

   Esse código configura o backplane com os valores padrão para [TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) e [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx). Para obter informações sobre como alterar esses valores, consulte [desempenho do signalr: métricas de scale](signalr-performance.md#scaleout_metrics)out.

## <a name="configure-the-database"></a>Configurar o banco de dados

Decida se o aplicativo usará a autenticação do Windows ou SQL Server autenticação para acessar o banco de dados. Independentemente, verifique se o usuário do banco de dados tem permissões para fazer logon, criar esquemas e criar tabelas.

Crie um novo banco de dados para o backplane usar. Você pode dar qualquer nome ao banco de dados. Você não precisa criar nenhuma tabela no banco de dados; o backplane criará as tabelas necessárias.

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a>Habilitar Service Broker

É recomendável habilitar Service Broker para o banco de dados do backplane. O Service Broker fornece suporte nativo para mensagens e enfileiramento no SQL Server, o que permite que o backplane Receba atualizações com mais eficiência. (No entanto, o backplane também funciona sem Service Broker.)

Para verificar se Service Broker está habilitado, consulte a coluna **is\_Broker\_Enabled** na exibição do catálogo **Sys. databases** .

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

Para habilitar Service Broker, use a seguinte consulta SQL:

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> Se essa consulta aparecer para deadlock, verifique se não há aplicativos conectados ao BD.

Se você tiver habilitado o rastreamento, os rastreamentos também mostrarão se Service Broker está habilitado.

## <a name="create-a-signalr-application"></a>Criar um aplicativo Signalr

Crie um aplicativo Signalr seguindo um destes tutoriais:

- [Introdução com Signalr 2,0](../getting-started/tutorial-getting-started-with-signalr.md)
- [Introdução com Signalr 2,0 e MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

Em seguida, modificaremos o aplicativo de chat para dar suporte ao scale out com SQL Server. Primeiro, adicione o pacote NuGet do Signalr. SqlServer ao seu projeto. No Visual Studio, no menu **ferramentas** , selecione **Gerenciador de pacotes NuGet**e, em seguida, selecione **console do Gerenciador de pacotes**. Na janela Console do Gerenciador de Pacotes, digite o seguinte comando:

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

Em seguida, abra o arquivo Startup.cs. Adicione o seguinte código ao método **Configure** :

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a>Implantar e executar o aplicativo

Prepare suas instâncias do Windows Server para implantar o aplicativo Signalr.

Adicione a função IIS. Inclua recursos de "desenvolvimento de aplicativos", incluindo o protocolo WebSocket.

![](scaleout-with-sql-server/_static/image4.png)

Inclua também o serviço de gerenciamento (listado em "ferramentas de gerenciamento").

![](scaleout-with-sql-server/_static/image5.png)

**Instale o Implantação da Web 3,0.** Quando você executar o Gerenciador do IIS, ele solicitará que você instale a plataforma Web da Microsoft ou você poderá [baixar o instalador](https://go.microsoft.com/fwlink/?LinkId=255386). No instalador da plataforma, procure Implantação da Web e instale o Implantação da Web 3,0

![](scaleout-with-sql-server/_static/image6.png)

Verifique se o serviço de gerenciamento da Web está em execução. Caso contrário, inicie o serviço. (Se você não vir o serviço de gerenciamento da Web na lista de serviços do Windows, certifique-se de que você instalou o serviço de gerenciamento quando adicionou a função do IIS.)

Por fim, abra a porta 8172 para TCP. Essa é a porta que a ferramenta de Implantação da Web usa.

Agora você está pronto para implantar o projeto do Visual Studio do computador de desenvolvimento no servidor. Em Gerenciador de Soluções, clique com o botão direito do mouse na solução e clique em **publicar**.

Para obter uma documentação mais detalhada sobre a implantação da Web, consulte [mapa de conteúdo de implantação da Web para Visual Studio e ASP.net](../../../whitepapers/aspnet-web-deployment-content-map.md).

Se você implantar o aplicativo em dois servidores, poderá abrir cada instância em uma janela separada do navegador e ver que cada uma recebe mensagens do Signalr da outra. (Naturalmente, em um ambiente de produção, os dois servidores ficam atrás de um balanceador de carga.)

![](scaleout-with-sql-server/_static/image7.png)

Depois de executar o aplicativo, você pode ver que o Signalr criou automaticamente as tabelas no banco de dados:

![](scaleout-with-sql-server/_static/image8.png)

O signalr gerencia as tabelas. Desde que seu aplicativo seja implantado, não exclua linhas, modifique a tabela e assim por diante.
