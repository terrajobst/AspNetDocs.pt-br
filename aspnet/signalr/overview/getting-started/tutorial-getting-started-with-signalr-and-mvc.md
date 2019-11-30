---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'Tutorial: chat em tempo real com o Signalr 2 e o MVC 5 | Microsoft Docs'
author: bradygaster
description: Este tutorial mostra como usar o ASP.NET Signalr 2 para criar um aplicativo de chat em tempo real. Você adiciona o Signalr a um aplicativo MVC 5.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 5671e4f0123ca2b0cb5314336cf4411467feac70
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600479"
---
# <a name="tutorial-real-time-chat-with-signalr-2-and-mvc-5"></a>Tutorial: chat em tempo real com o Signalr 2 e o MVC 5

Este tutorial mostra como usar o ASP.NET Signalr 2 para criar um aplicativo de chat em tempo real. Você adiciona o Signalr a um aplicativo MVC 5 e cria um modo de exibição de chat para enviar e exibir mensagens.

Neste tutorial, você:

> [!div class="checklist"]
> * Configurar o projeto
> * Executar o exemplo
> * Examinar o código

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a>{1&gt;{2&gt;Pré-requisitos&lt;2}&lt;1}

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) com a ASP.net e a carga de trabalho de **desenvolvimento da Web** .

## <a name="set-up-the-project"></a>Configurar o projeto

Esta seção mostra como usar o Visual Studio 2017 e o Signalr 2 para criar um aplicativo ASP.NET MVC 5 vazio, adicionar a biblioteca do Signalr e criar o aplicativo de chat.

1. No Visual Studio, crie um C# aplicativo ASP.NET que tenha como destino .NET Framework 4,5, nomeie-o SignalRChat e clique em OK.

    ![Criar Web](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

1. Em **novo aplicativo Web ASP.net-SignalRMvcChat**, selecione **MVC** e, em seguida, selecione **alterar autenticação**.

1. Em **alterar autenticação**, selecione **sem autenticação** e clique em **OK**.

    ![Não selecionar nenhuma autenticação](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)

1. Em **novo ASP.NET Web Application-SignalRMvcChat**, selecione **OK**.

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar** > **novo item**.

1. Em **Adicionar novo item-SignalRChat**, selecione **instalado** >  **C# > ** **Web** > **signalr** e selecione **classe de Hub do signalr (v2)** .

1. Nomeie a classe *ChatHub* e adicione-a ao projeto.

    Esta etapa cria o arquivo de classe *ChatHub.cs* e adiciona um conjunto de arquivos de script e referências de assembly que dão suporte a signalr ao projeto.

1. Substitua o código no novo arquivo da classe *ChatHub.cs* por este código:

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar** > **classe**.

1. Nomeie a nova classe *Startup* e adicione-a ao projeto.

1. Substitua o código no arquivo da classe *Startup.cs* por este código:

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]

1. Em **Gerenciador de soluções**, selecione **controladores** > **HomeController.cs**.

1. Adicione esse método ao *HomeController.cs*.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]

    Esse método retorna o modo de exibição de **chat** que você cria em uma etapa posterior.

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse em **exibições** > **página inicial**e selecione **Adicionar** >  **exibição**.

1. No **modo de exibição adicionar**, nomeie o novo modo de exibição de **chat** e selecione **Adicionar**.

1. Substitua o conteúdo de **chat. cshtml** por este código:

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]

1. Em **Gerenciador de soluções**, expanda **scripts**.

    As bibliotecas de script para jQuery e Signalr são visíveis no projeto.

    > [!IMPORTANT]
    > O Gerenciador de pacotes pode ter instalado uma versão mais recente dos scripts do Signalr.

1. Verifique se as referências de script no bloco de código correspondem às versões dos arquivos de script no projeto.

    Referências de script do bloco de código original:

    ```cshtml
    <!--Script references. -->
    <!--The jQuery library is required and is referenced by default in _Layout.cshtml. -->
    <!--Reference the SignalR library. -->
    <script src="~/Scripts/jquery.signalR-2.1.0.min.js"></script>
    ```

1. Se eles não corresponderem, atualize o arquivo *. cshtml* .

1. Na barra de menus, selecione **arquivo** > **salvar tudo**.

## <a name="run-the-sample"></a>Executar o exemplo

1. Na barra de ferramentas, ative a **depuração de script** e, em seguida, selecione o botão reproduzir para executar o exemplo no modo de depuração.

    ![Inserir nome de usuário](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)

1. Quando o navegador for aberto, insira um nome para sua identidade de chat.

1. Copie a URL do navegador, abra dois outros navegadores e cole as URLs nas barras de endereço.

1. Em cada navegador, insira um nome exclusivo.

1. Agora, adicione um comentário e selecione **Enviar**. Repita isso nos outros navegadores. Os comentários aparecem em tempo real.

    > [!NOTE]
    > Esse aplicativo de chat simples não mantém o contexto de discussão no servidor. O Hub transmite comentários para todos os usuários atuais. Os usuários que ingressarem no chat mais tarde verão as mensagens adicionadas a partir do momento em que ingressarem.

    Veja como o aplicativo de chat é executado em três navegadores diferentes. Quando Tom, Anand e Susan enviam mensagens, todos os navegadores são atualizados em tempo real:

    ![Todos os três navegadores exibem o mesmo histórico de chat](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

1. Em **Gerenciador de soluções**, inspecione o nó **documentos de script** para o aplicativo em execução. Há um arquivo de script chamado *hubs* que a biblioteca do signalr gera no tempo de execução. Esse arquivo gerencia a comunicação entre o script do jQuery e o código do lado do servidor.

    ![script de hubs gerados automaticamente no nó documentos de script](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)

## <a name="examine-the-code"></a>Examinar o código

O aplicativo de chat do Signalr demonstra duas tarefas básicas de desenvolvimento do Signalr. Ele mostra como criar um Hub. O servidor usa esse Hub como o objeto de coordenação principal. O Hub usa a biblioteca do Signalr jQuery para enviar e receber mensagens.

### <a name="signalr-hubs-in-the-chathubcs"></a>Hubs de sinalização no ChatHub.cs

No exemplo de código, a classe `ChatHub` deriva da classe `Microsoft.AspNet.SignalR.Hub`. Derivar da classe `Hub` é uma maneira útil de criar um aplicativo Signalr. Você pode criar métodos públicos em sua classe Hub e, em seguida, acessar esses métodos chamando-os de scripts em uma página da Web.

No código do chat, os clientes chamam o método `ChatHub.Send` para enviar uma nova mensagem. O Hub, por sua vez, envia a mensagem a todos os clientes chamando `Clients.All.addNewMessageToPage`.

O método `Send` demonstra vários conceitos de Hub:

* Declare métodos públicos em um hub para que os clientes possam chamá-los.

* Use a propriedade dinâmica `Microsoft.AspNet.SignalR.Hub.Clients` para se comunicar com todos os clientes conectados a esse Hub.

* Chame uma função no cliente (como a função `addNewMessageToPage`) para atualizar clientes.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery-chatcshtml"></a>Signalr e jQuery chat. cshtml

O arquivo de exibição *chat. cshtml* no exemplo de código mostra como usar a biblioteca do signalr jQuery para se comunicar com um Hub do signalr.  O código executa muitas tarefas importantes. Ele cria uma referência ao proxy gerado automaticamente para o Hub, declara uma função que o servidor pode chamar para enviar conteúdo por push aos clientes e inicia uma conexão para envio de mensagens para o Hub.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> No JavaScript, a referência à classe de servidor e seus membros estão em camelCase. O exemplo de código faz C# referência à classe `ChatHub` em JavaScript como `chatHub`.

Nesse bloco de código, você cria uma função de retorno de chamada no script.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

A classe Hub no servidor chama essa função para enviar por push atualizações de conteúdo para cada cliente. A chamada opcional para a função `htmlEncode` mostra uma maneira de codificar o conteúdo da mensagem em HTML antes de exibi-lo na página. É uma maneira de evitar a injeção de script.

Esse código abre uma conexão com o Hub.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

> [!NOTE]
> Essa abordagem garante que você estabeleça uma conexão antes que o manipulador de eventos seja executado.

O código inicia a conexão e, em seguida, passa a ela uma função para manipular o evento de clique no botão **Enviar** na página de bate-papo.

## <a name="get-the-code"></a>Obter o código

[Baixar projeto concluído](https://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

## <a name="additional-resources"></a>Recursos adicionais

Para obter mais informações sobre o Signalr, consulte os seguintes recursos:

* [Projeto do signalr](http://signalr.net)

* [GitHub e exemplos do signalr](https://github.com/SignalR/SignalR)

* [Wiki do signalr](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>{1&gt;{2&gt;Próximas etapas&lt;2}&lt;1}

Neste tutorial, você:

> [!div class="checklist"]
> * Configurar o projeto
> * O exemplo foi executado
> * Examinou o código

Avance para o próximo artigo para saber como criar um aplicativo Web que usa o Signalr ASP.NET 2 para fornecer funcionalidade de mensagens de alta frequência.
> [!div class="nextstepaction"]
> [Aplicativo Web com mensagens de alta frequência](tutorial-high-frequency-realtime-with-signalr.md)