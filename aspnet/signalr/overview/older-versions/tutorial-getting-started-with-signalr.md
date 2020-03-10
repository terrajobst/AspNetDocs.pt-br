---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: 'Tutorial: Introdução com Signalr 1. x | Microsoft Docs'
author: bradygaster
description: Use o Signalr ASP.NET para criar um aplicativo de chat em tempo real em uma página HTML.
ms.author: bradyg
ms.date: 02/18/2013
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 87a90b47ae30bee43e0b0c1e078597db54b8e67d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78623539"
---
# <a name="tutorial-getting-started-with-signalr-1x"></a>Tutorial: Introdução com Signalr 1. x

por [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Este tutorial mostra como usar o SignalR para criar um aplicativo de chat em tempo real. Você adicionará o Signalr a um aplicativo Web ASP.NET vazio e criará uma página HTML para enviar e exibir mensagens.

## <a name="overview"></a>Visão geral

Este tutorial apresenta o desenvolvimento do Signalr, mostrando como criar um aplicativo de chat simples baseado em navegador. Você adicionará a biblioteca do Signalr a um aplicativo Web ASP.NET vazio, criará uma classe de Hub para enviar mensagens aos clientes e criará uma página HTML que permite aos usuários enviar e receber mensagens de bate-papo. Para obter um tutorial semelhante que mostra como criar um aplicativo de chat no MVC 4 usando uma exibição do MVC, consulte [introdução com signalr e MVC 4](index.md).

> [!NOTE]
> Este tutorial usa a versão de lançamento (1. x) do Signalr. Para obter detalhes sobre as alterações entre o Signalr 1. x e o 2,0, consulte [Atualizando projetos do signalr 1. x](../releases/upgrading-signalr-1x-projects-to-20.md).

O signalr é uma biblioteca .NET de software livre para a criação de aplicativos Web que exigem interação do usuário ao vivo ou atualizações de dados em tempo real. Os exemplos incluem aplicativos sociais, jogos multiusuário, colaboração de negócios e notícias, clima ou aplicativos de atualização financeira. Eles costumam ser chamados de aplicativos em tempo real.

O signalr simplifica o processo de criação de aplicativos em tempo real. Ele inclui uma biblioteca de servidores ASP.NET e uma biblioteca de cliente JavaScript para facilitar o gerenciamento de conexões cliente-servidor e o envio por push de atualizações de conteúdo aos clientes. Você pode adicionar a biblioteca do Signalr a um aplicativo ASP.NET existente para obter funcionalidades em tempo real.

O tutorial demonstra as seguintes tarefas de desenvolvimento do Signalr:

- Adicionar a biblioteca do Signalr a um aplicativo Web ASP.NET.
- Criar uma classe de Hub para enviar conteúdo por push aos clientes.
- Usar a biblioteca do Signalr jQuery em uma página da Web para enviar mensagens e exibir atualizações do Hub.

A captura de tela a seguir mostra o aplicativo de chat em execução em um navegador. Cada novo usuário pode postar comentários e ver comentários adicionados após o usuário ingressar no chat.

![Instâncias de chat](tutorial-getting-started-with-signalr/_static/image1.png)

As

- [Configurar o projeto](#setup)
- [Executar o exemplo](#run)
- [Examinar o código](#code)
- [Próximas etapas](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Configurar o projeto

Esta seção mostra como criar um aplicativo Web ASP.NET vazio, adicionar o Signalr e criar o aplicativo de chat.

Pré-requisitos:

- Visual Studio 2010 SP1 ou 2012. Se você não tiver o Visual Studio, consulte [downloads do ASP.net](https://www.asp.net/downloads) para obter a ferramenta gratuita de desenvolvimento do Visual Studio 2012 Express.
- [Microsoft ASP.NET and Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=279941). Para o Visual Studio 2012, esse instalador adiciona novos recursos do ASP.NET, incluindo modelos de sinalização ao Visual Studio. Para o Visual Studio 2010 SP1, um instalador não está disponível, mas você pode concluir o tutorial instalando o pacote NuGet do Signalr, conforme descrito nas etapas de instalação.

As etapas a seguir usam o Visual Studio 2012 para criar um aplicativo Web ASP.NET vazio e adicionar a biblioteca do Signalr:

1. No Visual Studio, crie um aplicativo Web ASP.NET vazio.

    ![Criar Web vazia](tutorial-getting-started-with-signalr/_static/image2.png)
2. Abra o **console do Gerenciador de pacotes** selecionando **ferramentas | Gerenciador de pacotes NuGet | Console do Gerenciador de pacotes**. Digite o seguinte comando na janela do console:

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    Esse comando instala a versão mais recente do Signalr 1. x.
3. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto, selecione **Adicionar | Classe**. Nomeie a nova classe **ChatHub**.
4. Em **Gerenciador de soluções** expanda o nó scripts. As bibliotecas de script para jQuery e Signalr são visíveis no projeto.

    ![Referências de biblioteca](tutorial-getting-started-with-signalr/_static/image3.png)
5. Substitua o código na classe **ChatHub** pelo código a seguir.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e clique em **Adicionar | Novo item**. Na caixa de diálogo **Adicionar novo item** , selecione **classe de aplicativo global** e clique em **Adicionar**.

    ![Adicionar global](tutorial-getting-started-with-signalr/_static/image4.png)
7. Adicione as instruções de `using` a seguir após as instruções `using` fornecidas na classe Global.asax.cs.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. Adicione a seguinte linha de código no método `Application_Start` da classe global para registrar a rota padrão para os hubs do Signalr.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e clique em **Adicionar | Novo item**. Na caixa de diálogo **Adicionar novo item** , selecione página HTML e clique em **Adicionar**.
10. No **Gerenciador de soluções**, clique com o botão direito do mouse na página HTML que você acabou de criar e clique em **definir como página inicial**.
11. Substitua o código padrão na página HTML pelo código a seguir.

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. **Salve tudo** para o projeto.

<a id="run"></a>

## <a name="run-the-sample"></a>Executar o exemplo

1. Pressione F5 para executar o projeto no modo de depuração. A página HTML é carregada em uma instância do navegador e solicita um nome de usuário.

    ![Inserir nome de usuário](tutorial-getting-started-with-signalr/_static/image5.png)
2. Insira um nome de usuário.
3. Copie a URL da linha de endereço do navegador e use-a para abrir mais duas instâncias do navegador. Em cada instância do navegador, insira um nome de usuário exclusivo.
4. Em cada instância do navegador, adicione um comentário e clique em **Enviar**. Os comentários devem ser exibidos em todas as instâncias do navegador.

    > [!NOTE]
    > Esse aplicativo de chat simples não mantém o contexto de discussão no servidor. O Hub transmite comentários para todos os usuários atuais. Os usuários que ingressarem no chat mais tarde verão as mensagens adicionadas a partir do momento em que ingressarem.

    A captura de tela a seguir mostra o aplicativo de chat em execução em três instâncias de navegador, todas que são atualizadas quando uma instância envia uma mensagem:

    ![Navegadores de chat](tutorial-getting-started-with-signalr/_static/image6.png)
5. Em **Gerenciador de soluções**, inspecione o nó **documentos de script** para o aplicativo em execução. Há um arquivo de script chamado **hubs** que a biblioteca do signalr gera dinamicamente no tempo de execução. Esse arquivo gerencia a comunicação entre o script do jQuery e o código do lado do servidor.

    ![Script de Hub gerado](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Examinar o código

O aplicativo de chat do Signalr demonstra duas tarefas básicas de desenvolvimento do Signalr: criar um hub como o objeto principal de coordenação no servidor e usar a biblioteca do Signalr jQuery para enviar e receber mensagens.

### <a name="signalr-hubs"></a>Hubs de sinalização

No exemplo de código, a classe **ChatHub** deriva da classe **Microsoft. AspNet. signalr. Hub** . Derivar da classe **Hub** é uma maneira útil de criar um aplicativo signalr. Você pode criar métodos públicos em sua classe Hub e, em seguida, acessar esses métodos chamando-os de scripts jQuery em uma página da Web.

No código do chat, os clientes chamam o método **ChatHub. Send** para enviar uma nova mensagem. O Hub, por sua vez, envia a mensagem a todos os clientes chamando **clientes. All. broadcastMessage**.

O método **Send** demonstra vários conceitos de Hub:

- Declare métodos públicos em um hub para que os clientes possam chamá-los.
- Use a propriedade dinâmica **Microsoft. AspNet. signalr. Hub. clients** para acessar todos os clientes conectados a esse Hub.
- Chame uma função do jQuery no cliente (como a função `broadcastMessage`) para atualizar clientes.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>Signalr e jQuery

A página HTML no exemplo de código mostra como usar a biblioteca do Signalr jQuery para se comunicar com um Hub do Signalr. As tarefas essenciais no código estão declarando um proxy para referenciar o Hub, declarando uma função que o servidor pode chamar para enviar conteúdo por push aos clientes e iniciando uma conexão para envio de mensagens para o Hub.

O código a seguir declara um proxy para um Hub.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> No jQuery, a referência à classe de servidor e seus membros estão no camel case. O exemplo de código faz C# referência à classe **ChatHub** no jQuery como **ChatHub**.

O código a seguir é como você cria uma função de retorno de chamada no script. A classe Hub no servidor chama essa função para enviar por push atualizações de conteúdo para cada cliente. As duas linhas em que o HTML codifica o conteúdo antes de exibi-lo são opcionais e mostram uma maneira simples de evitar a injeção de script.

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

O código a seguir mostra como abrir uma conexão com o Hub. O código inicia a conexão e, em seguida, passa a ela uma função para manipular o evento de clique no botão **Enviar** na página HTML.

> [!NOTE]
> Essa abordagem garante que a conexão seja estabelecida antes que o manipulador de eventos seja executado.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Próximas etapas

Você aprendeu que o Signalr é uma estrutura para a criação de aplicativos Web em tempo real. Você também aprendeu várias tarefas de desenvolvimento do Signalr: como adicionar o Signalr a um aplicativo ASP.NET, como criar uma classe de Hub e como enviar e receber mensagens do Hub.

Você pode tornar o aplicativo de exemplo neste tutorial ou outros aplicativos Signaler disponíveis pela Internet implantando-os em um provedor de hospedagem. A Microsoft oferece hospedagem na Web gratuita para até 10 sites em uma conta de avaliação gratuita do [Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Para obter instruções sobre como implantar o aplicativo Signalr de exemplo, consulte [publicar o signalr introdução amostra como um site do Windows Azure](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx). Para obter informações detalhadas sobre como implantar um projeto Web do Visual Studio em um site do Windows Azure, consulte [implantando um aplicativo ASP.net em um site do Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet). (Observação: o transporte WebSocket não tem suporte no momento para sites do Windows Azure. Quando o transporte WebSocket não está disponível, o Signalr usa os outros transportes disponíveis, conforme descrito na seção transportes do [tópico introdução ao signalr](index.md).)

Para saber mais sobre os conceitos de desenvolvimento do Signalr avançado, visite os seguintes sites para código-fonte do Signalr e recursos:

- [Projeto do signalr](http://signalr.net)
- [GitHub e exemplos do signalr](https://github.com/SignalR/SignalR)
- [Wiki do signalr](https://github.com/SignalR/SignalR/wiki)
