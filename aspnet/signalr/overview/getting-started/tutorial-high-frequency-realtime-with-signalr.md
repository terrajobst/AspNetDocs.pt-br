---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: 'Tutorial: criar um aplicativo em tempo real de alta frequência com o Signalr 2 | Microsoft Docs'
author: bradygaster
description: Este tutorial mostra como criar um aplicativo Web que usa o Signalr ASP.NET para fornecer funcionalidade de mensagens de alta frequência.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 2503e90735d6cfa445ee08c9e43f8443aa106096
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558621"
---
# <a name="tutorial-create-high-frequency-real-time-app-with-signalr-2"></a>Tutorial: criar um aplicativo em tempo real de alta frequência com o Signalr 2

Este tutorial mostra como criar um aplicativo Web que usa o Signalr ASP.NET 2 para fornecer funcionalidade de mensagens de alta frequência. Nesse caso, "mensagens de alta frequência" significa que o servidor envia atualizações a uma taxa fixa. Você envia até 10 mensagens por um segundo.

O aplicativo que você cria exibe uma forma que os usuários podem arrastar. O servidor atualiza a posição da forma em todos os navegadores conectados para corresponder à posição da forma arrastada usando atualizações cronometradas.

Os conceitos apresentados neste tutorial têm aplicativos em jogos em tempo real e em outros aplicativos de simulação.

Neste tutorial, você:

> [!div class="checklist"]
> * Configurar o projeto
> * Criar o aplicativo base
> * Mapear para o Hub quando o aplicativo for iniciado
> * Adicionar o cliente
> * Executar o aplicativo
> * Adicionar o loop do cliente
> * Adicionar o loop do servidor
> * Adicionar animação suave

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a>Prerequisites

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) com a carga de trabalho **ASP.NET e desenvolvimento para a Web**.

## <a name="set-up-the-project"></a>Configurar o projeto

Nesta seção, você criará o projeto no Visual Studio 2017.

Esta seção mostra como usar o Visual Studio 2017 para criar um aplicativo Web ASP.NET vazio e adicionar as bibliotecas Signalr e jQuery. UI.

1. No Visual Studio, crie um aplicativo Web ASP.NET.

    ![Criar Web](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

1. Na janela **novo aplicativo Web ASP.net-MoveShapeDemo** , deixe a seleção **vazia** e selecione **OK**.

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar** > **novo item**.

1. Em **Adicionar novo item-MoveShapeDemo**, selecione **instalado** >  **C# > ** **Web** > **signalr** e selecione **classe de Hub do signalr (v2)** .

1. Nomeie a classe *MoveShapeHub* e adicione-a ao projeto.

    Esta etapa cria o arquivo de classe *MoveShapeHub.cs* . Simultaneamente, ele adiciona um conjunto de arquivos de script e referências de assembly que dão suporte ao Signalr para o projeto.

1. Selecione **Ferramentas** > **Gerenciador de Pacotes NuGet** > **Console do Gerenciador de Pacotes**.

1. No **console do Gerenciador de pacotes**, execute este comando:

    ```console
    Install-Package jQuery.UI.Combined
    ```

    O comando instala a biblioteca da interface do usuário do jQuery. Você o usa para animar a forma.

1. Em **Gerenciador de soluções**, expanda o nó scripts.

    ![Referências da biblioteca de scripts](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

    As bibliotecas de script para jQuery, jQueryUI e Signalr são visíveis no projeto.

## <a name="create-the-base-application"></a>Criar o aplicativo base

Nesta seção, você criará um aplicativo de navegador. O aplicativo envia o local da forma para o servidor durante cada evento de movimentação do mouse. O servidor transmite essas informações para todos os outros clientes conectados em tempo real. Você aprenderá mais sobre esse aplicativo em seções posteriores.

1. Abra o arquivo *MoveShapeHub.cs* .

1. Substitua o código no arquivo *MoveShapeHub.cs* por este código:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

1. Salve o arquivo.

A classe `MoveShapeHub` é uma implementação de um hub Signalr. Como no tutorial de [introdução com signalr](tutorial-getting-started-with-signalr.md) , o Hub tem um método que os clientes chamam diretamente. Nesse caso, o cliente envia um objeto com as novas coordenadas X e Y da forma para o servidor. Essas coordenadas são transmitidas para todos os outros clientes conectados. O signalr serializa automaticamente esse objeto usando JSON.

O aplicativo envia o objeto de `ShapeModel` para o cliente. Ele tem membros para armazenar a posição da forma. A versão do objeto no servidor também tem um membro para controlar quais dados do cliente estão sendo armazenados. Esse objeto impede que o servidor envie dados de um cliente de volta para ele mesmo. Esse membro usa o atributo `JsonIgnore` para impedir que o aplicativo faça a serialização dos dados e o envie de volta para o cliente.

## <a name="map-to-the-hub-when-app-starts"></a>Mapear para o Hub quando o aplicativo for iniciado

Em seguida, configure o mapeamento para o Hub quando o aplicativo for iniciado. No Signalr 2, a adição de uma classe de inicialização OWIN cria o mapeamento.

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar** > **novo item**.

1. Em **Adicionar novo item-MoveShapeDemo,** selecione **instalado** > **Visual C#**  > **Web** e, em seguida, selecione **classe de inicialização OWIN**.

1. Nomeie a classe como *Startup* e selecione **OK**.

1. Substitua o código padrão no arquivo *Startup.cs* por este código:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

A classe de inicialização OWIN chama `MapSignalR` quando o aplicativo executa o método `Configuration`. O aplicativo adiciona a classe ao processo de inicialização do OWIN usando o atributo `OwinStartup` assembly.

## <a name="add-the-client"></a>Adicionar o cliente

Adicione a página HTML do cliente.

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar** > **página HTML**.

1. Nomeie a página **como padrão** e selecione **OK**.

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse em *Default. html* e selecione **definir como página inicial**.

1. Substitua o código padrão no arquivo *Default. html* por este código:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html?highlight=14-16)]

1. Em **Gerenciador de soluções**, expanda **scripts**.

    As bibliotecas de script para jQuery e Signalr são visíveis no projeto.

    > [!IMPORTANT]
    > O Gerenciador de pacotes instala uma versão mais recente dos scripts do Signalr.

1. Atualize as referências de script no bloco de código para corresponder às versões dos arquivos de script no projeto.

Esse código HTML e JavaScript cria um `div` vermelho chamado `shape`. Ele habilita o comportamento de arrastar da forma usando a biblioteca jQuery e usa o evento `drag` para enviar a posição da forma para o servidor.

## <a name="run-the-app"></a>Executar o aplicativo

Você pode executar o aplicativo para se'e-lo funcionar. Quando você arrasta a forma em uma janela do navegador, a forma também se move em outros navegadores.

1. Na barra de ferramentas, ative a **depuração de script** e, em seguida, selecione o botão reproduzir para executar o aplicativo no modo de depuração.

    ![Captura de tela do usuário que está ligando o modo de depuração e selecionando reproduzir.](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)

    Uma janela do navegador é aberta com a forma vermelha no canto superior direito.

1. Copie a URL da página.

1. Abra outro navegador e cole a URL na barra de endereços.

1. Arraste a forma em uma das janelas do navegador. A forma na outra janela do navegador é a seguinte.

Embora o aplicativo funcione usando esse método, ele não é um modelo de programação recomendado. Não há nenhum limite máximo para o número de mensagens enviadas. Como resultado, os clientes e o servidor ficarão sobrecarregados com mensagens e degradações de desempenho. Além disso, o aplicativo exibe uma animação não conjunta no cliente. Essa animação jerky acontece porque a forma se move instantaneamente por cada método. É melhor se a forma se mover suavemente para cada novo local. Em seguida, você aprenderá a corrigir esses problemas.

## <a name="add-the-client-loop"></a>Adicionar o loop do cliente

O envio do local da forma em cada evento de movimentação do mouse cria uma quantidade desnecessária de tráfego de rede. O aplicativo precisa limitar as mensagens do cliente.

Use a função JavaScript `setInterval` para configurar um loop que envia novas informações de posição ao servidor em uma taxa fixa. Esse loop é uma representação básica de um "loop de jogo". É uma função chamada repetidamente que orienta toda a funcionalidade de um jogo.

1. Substitua o código do cliente no arquivo *Default. html* por este código:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html?highlight=14-16)]

    > [!IMPORTANT]
    > Você precisa substituir as referências de script novamente. Eles devem corresponder às versões dos scripts no projeto.

    Esse novo código adiciona a função `updateServerModel`. Ele é chamado em uma frequência fixa. A função envia os dados de posição para o servidor sempre que o sinalizador de `moved` indica que há novos dados de posição a serem enviados.

1. Selecione o botão reproduzir para iniciar o aplicativo

1. Copie a URL da página.

1. Abra outro navegador e cole a URL na barra de endereços.

1. Arraste a forma em uma das janelas do navegador. A forma na outra janela do navegador é a seguinte.

Como o aplicativo limita o número de mensagens que são enviadas ao servidor, a animação não aparecerá como um Smooth.

## <a name="add-the-server-loop"></a>Adicionar o loop do servidor

No aplicativo atual, as mensagens enviadas do servidor para o cliente saem com a frequência em que são recebidas. Esse tráfego de rede apresenta um problema semelhante ao que vemos no cliente.

O aplicativo pode enviar mensagens com mais frequência do que são necessárias. A conexão pode ser inundada como resultado. Esta seção descreve como atualizar o servidor para adicionar um temporizador que limita a taxa de mensagens de saída.

1. Substitua o conteúdo de `MoveShapeHub.cs` com este código:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

1. Selecione o botão reproduzir para iniciar o aplicativo.

1. Copie a URL da página.

1. Abra outro navegador e cole a URL na barra de endereços.

1. Arraste a forma em uma das janelas do navegador.

Esse código expande o cliente para adicionar a classe de `Broadcaster`. A nova classe limita as mensagens de saída usando a classe `Timer` do .NET Framework.

É bom saber que o próprio Hub é transitório. Ele é criado toda vez que é necessário. Portanto, o aplicativo cria o `Broadcaster` como um singleton. Ele usa a inicialização lenta para adiar a criação do `Broadcaster`até que seja necessário. Isso garante que o aplicativo crie a primeira instância de Hub completamente antes de iniciar o temporizador.

A chamada para a função de `UpdateShape` dos clientes é então movida do método `UpdateModel` do Hub. Ele não é mais chamado imediatamente sempre que o aplicativo recebe mensagens de entrada. Em vez disso, o aplicativo envia as mensagens para os clientes a uma taxa de 25 chamadas por segundo. O processo é gerenciado pelo temporizador `_broadcastLoop` de dentro da classe `Broadcaster`.

Por fim, em vez de chamar o método de cliente diretamente do Hub, a classe `Broadcaster` precisa obter uma referência para o Hub de `_hubContext` operacional no momento. Ele obtém a referência com a `GlobalHost`.

## <a name="add-smooth-animation"></a>Adicionar animação suave

O aplicativo está quase terminando, mas poderíamos fazer mais um aperfeiçoamento. O aplicativo move a forma no cliente em resposta às mensagens do servidor. Em vez de definir a posição da forma como o novo local fornecido pelo servidor, use a função `animate` da biblioteca de interface do usuário do JQuery. Ele pode mover a forma suavemente entre sua posição atual e nova.

1. Atualize o método de `updateShape` do cliente no arquivo *Default. html* para se parecer com o código realçado:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

1. Selecione o botão reproduzir para iniciar o aplicativo.

1. Copie a URL da página.

1. Abra outro navegador e cole a URL na barra de endereços.

1. Arraste a forma em uma das janelas do navegador.

A movimentação da forma na outra janela aparece menos jerky. O aplicativo interpola seu movimento ao longo do tempo em vez de ser definido uma vez por mensagem de entrada.

Esse código move a forma do local antigo para o novo. O servidor fornece a posição da forma no decorrer do intervalo de animação. Nesse caso, são 100 milissegundos. O aplicativo limpa qualquer animação anterior em execução na forma antes que a nova animação seja iniciada.

## <a name="get-the-code"></a>Obter o código

[Baixar projeto concluído](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)

## <a name="additional-resources"></a>Recursos adicionais

O paradigma de comunicação sobre o qual você aprendeu é útil para desenvolver jogos online e outras simulações, como [o jogo do emissor criado com o signalr](https://shootr.azurewebsites.net/).

Para obter mais informações sobre o Signalr, consulte os seguintes recursos:

* [Projeto do signalr](http://signalr.net)

* [GitHub e exemplos do signalr](https://github.com/SignalR/SignalR)

* [Wiki do signalr](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você:

> [!div class="checklist"]
> * Configurar o projeto
> * Criou o aplicativo base
> * Mapeado para o Hub quando o aplicativo é iniciado
> * Adicionado o cliente
> * Executou o aplicativo
> * Adicionado o loop do cliente
> * Adicionado o loop do servidor
> * Animação suave adicionada

Avance para o próximo artigo para saber como criar um aplicativo Web que usa o Signalr ASP.NET 2 para fornecer a funcionalidade de difusão do servidor.
> [!div class="nextstepaction"]
> [Sinalização 2 e difusão do servidor](tutorial-server-broadcast-with-signalr.md)