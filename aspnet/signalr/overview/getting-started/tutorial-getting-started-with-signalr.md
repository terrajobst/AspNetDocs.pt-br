---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'Tutorial: chat em tempo real com o Signalr 2 | Microsoft Docs'
author: bradygaster
description: Este tutorial mostra como usar o SignalR para criar um aplicativo de chat em tempo real. Você adiciona o Signalr a um aplicativo Web ASP.NET vazio.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: bc4ef190b6e36812b6fe7ca4e16eb763431e0e82
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600467"
---
# <a name="tutorial-real-time-chat-with-signalr-2"></a>Tutorial: chat em tempo real com o Signalr 2

Este tutorial mostra como usar o Signalr para criar um aplicativo de chat em tempo real. Você adiciona o Signalr a um aplicativo Web ASP.NET vazio e cria uma página HTML para enviar e exibir mensagens.

Neste tutorial, você:

> [!div class="checklist"]
> * Configurar o projeto
> * Executar o exemplo
> * Examinar o código

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a>{1&gt;{2&gt;Pré-requisitos&lt;2}&lt;1}

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) com a ASP.net e a carga de trabalho de **desenvolvimento da Web** .

## <a name="set-up-the-project"></a>Configurar o projeto

Esta seção mostra como usar o Visual Studio 2017 e o Signalr 2 para criar um aplicativo Web ASP.NET vazio, adicionar o Signalr e criar o aplicativo de chat.

1. No Visual Studio, crie um aplicativo Web ASP.NET.

    ![Criar Web](tutorial-getting-started-with-signalr/_static/image2.png)

1. Na janela **novo projeto do ASP.net-SignalRChat** , deixe a seleção **vazia** e selecione **OK**.

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar** > **novo item**.

1. Em **Adicionar novo item-SignalRChat**, selecione **instalado** >  **C# > ** **Web** > **signalr** e selecione **classe de Hub do signalr (v2)** .

1. Nomeie a classe *ChatHub* e adicione-a ao projeto.

    Esta etapa cria o arquivo de classe *ChatHub.cs* e adiciona um conjunto de arquivos de script e referências de assembly que dão suporte a signalr ao projeto.

1. Substitua o código no novo arquivo da classe *ChatHub.cs* por este código:

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar** > **novo item**.

1. Em **Adicionar novo item-SignalRChat,** selecione **instalado** > **Visual C#**  > **Web** e, em seguida, selecione **classe de inicialização OWIN**.

1. Nomeie a classe como *Startup* e adicione-a ao projeto.

1. Substitua o código padrão na classe de *inicialização* por este código:

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar** > **página HTML**.

1. Nomeie o novo *índice* de página e selecione **OK**.

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse na página HTML que você criou e selecione **definir como página inicial**.

1. Substitua o código padrão na página HTML por este código:

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]

1. Em **Gerenciador de soluções**, expanda **scripts**.

    As bibliotecas de script para jQuery e Signalr são visíveis no projeto.

    > [!IMPORTANT]
    > O Gerenciador de pacotes pode ter instalado uma versão mais recente dos scripts do Signalr.

1. Verifique se as referências de script no bloco de código correspondem às versões dos arquivos de script no projeto.

    Referências de script do bloco de código original:

    ```html
    <!--Script references. -->
    <!--Reference the jQuery library. -->
    <script src="Scripts/jquery-3.1.1.min.js" ></script>
    <!--Reference the SignalR library. -->
    <script src="Scripts/jquery.signalR-2.2.1.min.js"></script>
    ```

1. Se eles não corresponderem, atualize o arquivo *. html* .

1. Na barra de menus, selecione **arquivo** > **salvar tudo**.

## <a name="run-the-sample"></a>Executar o exemplo

1. Na barra de ferramentas, ative a **depuração de script** e, em seguida, selecione o botão reproduzir para executar o exemplo no modo de depuração.

    ![Inserir nome de usuário](tutorial-getting-started-with-signalr/_static/image3.png)

1. Quando o navegador for aberto, insira um nome para sua identidade de chat.

1. Copie a URL do navegador, abra dois outros navegadores e cole as URLs nas barras de endereço.

1. Em cada navegador, insira um nome exclusivo.

1. Agora, adicione um comentário e selecione **Enviar**. Repita isso nos outros navegadores. Os comentários aparecem em tempo real.

    > [!NOTE]
    > Esse aplicativo de chat simples não mantém o contexto de discussão no servidor. O Hub transmite comentários para todos os usuários atuais. Os usuários que ingressarem no chat mais tarde verão as mensagens adicionadas a partir do momento em que ingressarem.

    Veja como o aplicativo de chat é executado em três navegadores diferentes. Quando Tom, Anand e Susan enviam mensagens, todos os navegadores são atualizados em tempo real:

    ![Todos os três navegadores exibem o mesmo histórico de chat](tutorial-getting-started-with-signalr/_static/image4.png)

1. Em **Gerenciador de soluções**, inspecione o nó **documentos de script** para o aplicativo em execução. Há um arquivo de script chamado *hubs* que a biblioteca do signalr gera no tempo de execução. Esse arquivo gerencia a comunicação entre o script do jQuery e o código do lado do servidor.

    ![script de hubs gerados automaticamente no nó documentos de script](tutorial-getting-started-with-signalr/_static/image5.png)

## <a name="examine-the-code"></a>Examinar o código

O aplicativo SignalRChat demonstra duas tarefas básicas de desenvolvimento do Signalr. Ele mostra como criar um Hub. O servidor usa esse Hub como o objeto de coordenação principal. O Hub usa a biblioteca do Signalr jQuery para enviar e receber mensagens.

### <a name="signalr-hubs-in-the-chathubcs"></a>Hubs de sinalização no ChatHub.cs

No exemplo de código acima, a classe `ChatHub` deriva da classe `Microsoft.AspNet.SignalR.Hub`. Derivar da classe `Hub` é uma maneira útil de criar um aplicativo Signalr. Você pode criar métodos públicos em sua classe Hub e, em seguida, usar esses métodos chamando-os de scripts em uma página da Web.

No código do chat, os clientes chamam o método `ChatHub.Send` para enviar uma nova mensagem. O Hub, em seguida, envia a mensagem a todos os clientes chamando `Clients.All.broadcastMessage`.

O método `Send` demonstra vários conceitos de Hub:

* Declare métodos públicos em um hub para que os clientes possam chamá-los.

* Use a propriedade dinâmica `Microsoft.AspNet.SignalR.Hub.Clients` para se comunicar com todos os clientes conectados a esse Hub.

* Chame uma função no cliente (como a função `broadcastMessage`) para atualizar clientes.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery-in-the-indexhtml"></a>Signalr e jQuery no index. html

A página *index. html* no exemplo de código mostra como usar a biblioteca do signalr jQuery para se comunicar com um hub signalr. O código executa muitas tarefas importantes. Ele declara um proxy para referenciar o Hub, declara uma função que o servidor pode chamar para enviar conteúdo por push aos clientes e inicia uma conexão para envio de mensagens ao Hub.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> Em JavaScript, a referência à classe de servidor e seus membros deve ser camelCase. O exemplo de código faz C# referência à classe *ChatHub* em JavaScript como `chatHub`.

Nesse bloco de código, você cria uma função de retorno de chamada no script.

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

A classe Hub no servidor chama essa função para enviar por push atualizações de conteúdo para cada cliente. As duas linhas que codificam o conteúdo em HTML antes de exibi-las são opcionais e mostram uma boa maneira de evitar a injeção de script.

Esse código abre uma conexão com o Hub.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

> [!NOTE]
> Essa abordagem garante que o código estabeleça uma conexão antes que o manipulador de eventos seja executado.

O código inicia a conexão e, em seguida, passa a ela uma função para manipular o evento de clique no botão **Enviar** na página HTML.

## <a name="get-the-code"></a>Obter o código

[Baixar projeto concluído](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

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

Avance para o próximo artigo para aprender a usar o Signalr e o MVC 5.
> [!div class="nextstepaction"]
> [Signalr 2 e MVC 5](tutorial-getting-started-with-signalr-and-mvc.md)