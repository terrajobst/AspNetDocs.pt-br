---
uid: signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
title: Tempo real de alta frequência com Signalr 1. x | Microsoft Docs
author: bradygaster
description: Este tutorial mostra como criar um aplicativo Web que usa o Signalr ASP.NET para fornecer funcionalidade de mensagens de alta frequência. Mensagens de alta frequência no...
ms.author: bradyg
ms.date: 04/16/2013
ms.assetid: ad2a5da5-2e79-40ea-bc84-028d327f5982
msc.legacyurl: /signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: e37e63a410952ec170cbbaadeeb54eae7e82b3b5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78623497"
---
# <a name="high-frequency-realtime-with-signalr-1x"></a>Tempo real de alta frequência com SignalR 1.x

por [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Este tutorial mostra como criar um aplicativo Web que usa o Signalr ASP.NET para fornecer funcionalidade de mensagens de alta frequência. O sistema de mensagens de alta frequência nesse caso significa que as atualizações são enviadas a uma taxa fixa; no caso desse aplicativo, até 10 mensagens por segundo.
> 
> O aplicativo que você criará neste tutorial exibe uma forma que os usuários podem arrastar. A posição da forma em todos os outros navegadores conectados será atualizada para corresponder à posição da forma arrastada usando atualizações cronometradas.
> 
> Os conceitos apresentados neste tutorial têm aplicativos em jogos em tempo real e em outros aplicativos de simulação.
> 
> Comentários sobre o tutorial são bem-vindos. Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, poderá lançá-las no fórum do [signalr ASP.net](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [stackoverflow.com](http://stackoverflow.com).

## <a name="overview"></a>Visão geral

Este tutorial demonstra como criar um aplicativo que compartilha o estado de um objeto com outros navegadores em tempo real. O aplicativo que criaremos é chamado de MoveShape. A página MoveShape exibirá um elemento HTML div que o usuário pode arrastar; Quando o usuário arrastar a div, sua nova posição será enviada para o servidor, o que dirá para todos os outros clientes conectados atualizarem a posição da forma para correspondência.

![A janela do aplicativo](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

O aplicativo criado neste tutorial é baseado em uma demonstração do Damian Edwards. Um vídeo que contém esta demonstração pode ser visto [aqui](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR).

O tutorial começará demonstrando como enviar mensagens de sinalização de cada evento que é acionado à medida que a forma é arrastada. Cada cliente conectado atualizará a posição da versão local da forma a cada vez que uma mensagem for recebida.

Embora o aplicativo funcione usando esse método, esse não é um modelo de programação recomendado, já que não haveria nenhum limite superior para o número de mensagens sendo enviadas, portanto, os clientes e o servidor poderiam ficar sobrecarregados com mensagens e o desempenho diminuiria . A animação exibida no cliente também seria desativada, pois a forma seria movida instantaneamente por cada método, em vez de mover suavemente para cada novo local. As seções posteriores do tutorial demonstrarão como criar uma função de temporizador que restringe a taxa máxima em que as mensagens são enviadas pelo cliente ou pelo servidor e como mover a forma suavemente entre locais. A versão final do aplicativo criado neste tutorial pode ser baixada na [Galeria de códigos](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6).

Este tutorial contém as seguintes seções:

- [Pré-requisitos](#prerequisites)
- [Criar o projeto](#createtheproject)
- [Adicionar os pacotes do NuGet do ASP.NET e do JQuery. UI](#nugetpackages)
- [Criar o aplicativo base](#baseapp)
- [Adicionar o loop do cliente](#clientloop)
- [Adicionar o loop do servidor](#serverloop)
- [Adicionar animação suave no cliente](#animation)
- [Etapas adicionais](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Prerequisites

Este tutorial requer o Visual Studio 2012 ou o Visual Studio 2010. Se o Visual Studio 2010 for usado, o projeto usará .NET Framework 4 em vez de .NET Framework 4,5.

Se você estiver usando o Visual Studio 2012, é recomendável instalar a [atualização ASP.NET and Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650). Essa atualização contém novos recursos, como aprimoramentos na publicação, nova funcionalidade e novos modelos.

Se você tiver o Visual Studio 2010, verifique se o [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) está instalado.

<a id="createtheproject"></a>

## <a name="create-the-project"></a>Criar o projeto

Nesta seção, vamos criar o projeto no Visual Studio.

1. No menu **Arquivo**, clique em **Novo Projeto**.
2. Na caixa de diálogo **novo projeto** , expanda **C#** em **modelos** e selecione **Web**.
3. Selecione o modelo de **aplicativo Web ASP.net vazio** , nomeie o projeto *MoveShapeDemo*e clique em **OK**.

    ![Criando o novo projeto](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

<a id="nugetpackages"></a>

## <a name="add-the-signalr-and-jqueryui-nuget-packages"></a>Adicionar os pacotes de NuGet do Signalr e JQuery. UI

Você pode adicionar a funcionalidade do Signalr a um projeto instalando um pacote NuGet. Este tutorial também usará o pacote JQuery. UI para permitir que a forma seja arrastada e animada.

1. Clique em **ferramentas | Gerenciador de pacotes NuGet | Console do Gerenciador de pacotes**.
2. Insira o comando a seguir no Gerenciador de pacotes.

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.ps1)]

    O pacote do Signalr instala vários outros pacotes NuGet como dependências. Quando a instalação for concluída, você terá todos os componentes de cliente e servidor necessários para usar o Signalr em um aplicativo ASP.NET.
3. Insira o comando a seguir no console do Gerenciador de pacotes para instalar os pacotes JQuery e JQuery. UI.

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.ps1)]

<a id="baseapp"></a>

## <a name="create-the-base-application"></a>Criar o aplicativo base

Nesta seção, criaremos um aplicativo de navegador que envia o local da forma para o servidor durante cada evento de movimentação do mouse. Em seguida, o servidor transmite essas informações para todos os outros clientes conectados conforme ele é recebido. Expandiremos esse aplicativo em seções posteriores.

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar**, **classe..** .. Nomeie a classe **MoveShapeHub** e clique em **Adicionar**.
2. Substitua o código na nova classe **MoveShapeHub** pelo código a seguir.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.cs)]

    A classe de `MoveShapeHub` acima é uma implementação de um hub Signalr. Como no tutorial de [introdução com signalr](index.md) , o Hub tem um método que os clientes chamarão diretamente. Nesse caso, o cliente enviará um objeto que contém as novas coordenadas X e Y da forma para o servidor, que, em seguida, será transmitido para todos os outros clientes conectados. O signalr serializará automaticamente esse objeto usando JSON.

    O objeto que será enviado ao cliente (`ShapeModel`) contém membros para armazenar a posição da forma. A versão do objeto no servidor também contém um membro para controlar quais dados do cliente estão sendo armazenados, para que um determinado cliente não possa enviar seus próprios dados. Esse membro usa o atributo `JsonIgnore` para impedir que ele seja serializado e enviado ao cliente.
3. Em seguida, configuraremos o Hub quando o aplicativo for iniciado. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e clique em **Adicionar | Classe de aplicativo global**. Aceite o nome padrão *global* e clique em **OK**.

    ![Adicionar classe de aplicativo global](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
4. Adicione a seguinte instrução `using` após as instruções **using** fornecidas na classe global.asax.cs.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.cs)]
5. Adicione a seguinte linha de código no método `Application_Start` da classe global para registrar a rota padrão para o Signalr.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    O arquivo global. asax deve ser semelhante ao seguinte:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.cs)]
6. Em seguida, adicionaremos o cliente. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e clique em **Adicionar | Novo item**. Na caixa de diálogo **Adicionar novo item** , selecione **página HTML**. Dê à página um nome apropriado (como **Default. html**) e clique em **Adicionar**.
7. No **Gerenciador de soluções**, clique com o botão direito do mouse na página que você acabou de criar e clique em **definir como página inicial**.
8. Substitua o código padrão na página HTML pelo trecho de código a seguir.

    > [!NOTE]
    > Verifique se as referências de script abaixo correspondem aos pacotes adicionados ao seu projeto na pasta scripts. No Visual Studio 2010, a versão do JQuery e o Signalr adicionados ao projeto podem não corresponder aos números de versão abaixo.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample7.html)]

    O código HTML e JavaScript acima cria uma div vermelha chamada Shape, habilita o comportamento de arrastar da forma usando a biblioteca jQuery e usa o evento `drag` da forma para enviar a posição da forma para o servidor.
9. Inicie o aplicativo pressionando F5. Copie a URL da página e cole-a em uma segunda janela do navegador. Arraste a forma em uma das janelas do navegador; a forma na outra janela do navegador deve ser movida.

    ![A janela do aplicativo](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a>Adicionar o loop do cliente

Como o envio do local da forma em cada evento de movimentação do mouse criará uma quantidade desnecessária de tráfego de rede, as mensagens do cliente precisarão ser limitadas. Usaremos a função JavaScript `setInterval` para configurar um loop que envia novas informações de posição ao servidor em uma taxa fixa. Esse loop é uma representação muito básica de um "loop de jogo", uma função chamada repetidamente que conduz toda a funcionalidade de um jogo ou de outra simulação.

1. Atualize o código do cliente na página HTML para corresponder ao trecho de código a seguir.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample8.html)]

    A atualização acima adiciona a função `updateServerModel`, que é chamada em uma frequência fixa. Essa função envia os dados de posição para o servidor sempre que o sinalizador de `moved` indica que há novos dados de posição a serem enviados.
2. Inicie o aplicativo pressionando F5. Copie a URL da página e cole-a em uma segunda janela do navegador. Arraste a forma em uma das janelas do navegador; a forma na outra janela do navegador deve ser movida. Como o número de mensagens que são enviadas ao servidor será limitado, a animação não aparecerá tão suave quanto na seção anterior.

    ![A janela do aplicativo](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a>Adicionar o loop do servidor

No aplicativo atual, as mensagens enviadas do servidor para o cliente saem com a frequência em que são recebidas. Isso apresenta um problema semelhante como foi visto no cliente; as mensagens podem ser enviadas com mais frequência do que são necessárias e a conexão pode ser inundada como resultado. Esta seção descreve como atualizar o servidor para implementar um temporizador que limita a taxa de mensagens de saída.

1. Substitua o conteúdo de `MoveShapeHub.cs` pelo trecho de código a seguir.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample9.cs)]

    O código acima expande o cliente para adicionar a classe `Broadcaster`, que limita as mensagens de saída usando a classe `Timer` do .NET Framework.

    Como o próprio Hub é transitório (ele é criado toda vez que for necessário), o `Broadcaster` será criado como um singleton. A inicialização lenta (introduzida no .NET 4) é usada para adiar sua criação até que seja necessária, garantindo que a primeira instância do Hub seja completamente criada antes que o temporizador seja iniciado.

    A chamada para a função de `UpdateShape` dos clientes é então movida do método `UpdateModel` do Hub, para que ele não seja mais chamado imediatamente sempre que as mensagens recebidas forem recebidas. Em vez disso, as mensagens para os clientes serão enviadas a uma taxa de 25 chamadas por segundo, gerenciadas pelo temporizador `_broadcastLoop` de dentro da classe `Broadcaster`.

    Por fim, em vez de chamar o método de cliente diretamente do Hub, a classe `Broadcaster` precisa obter uma referência ao Hub operacional (`_hubContext`) no momento usando o `GlobalHost`.
2. Inicie o aplicativo pressionando F5. Copie a URL da página e cole-a em uma segunda janela do navegador. Arraste a forma em uma das janelas do navegador; a forma na outra janela do navegador deve ser movida. Não haverá uma diferença visível no navegador a partir da seção anterior, mas o número de mensagens enviadas para o cliente será limitado.

    ![A janela do aplicativo](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a>Adicionar animação suave no cliente

O aplicativo está quase concluído, mas poderíamos fazer mais um aperfeiçoamento, no movimento da forma no cliente, à medida que ele é movido em resposta às mensagens do servidor. Em vez de definir a posição da forma como o novo local fornecido pelo servidor, usaremos a função `animate` da biblioteca da interface do usuário do JQuery para mover a forma suavemente entre sua posição atual e nova.

1. Atualize o método de `updateShape` do cliente para se parecer com o código realçado abaixo:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample10.html?highlight=35-42)]

    O código acima move a forma do local antigo para o novo dado pelo servidor no decorrer do intervalo de animação (nesse caso, 100 milissegundos). Qualquer animação anterior em execução na forma é desmarcada antes que a nova animação seja iniciada.
2. Inicie o aplicativo pressionando F5. Copie a URL da página e cole-a em uma segunda janela do navegador. Arraste a forma em uma das janelas do navegador; a forma na outra janela do navegador deve ser movida. O movimento da forma na outra janela deve aparecer menos jerky à medida que seu movimento é interpolado ao longo do tempo em vez de ser definido uma vez por mensagem de entrada.

    ![A janela do aplicativo](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a>Etapas adicionais

Neste tutorial, você aprendeu a programar um aplicativo Signalr que envia mensagens de alta frequência entre clientes e servidores. Esse paradigma de comunicação é útil para o desenvolvimento de jogos online e outras simulações, como [o jogo do lançante criado com o signalr](http://shootr.signalr.net).

O aplicativo completo criado neste tutorial pode ser baixado da [Galeria de códigos](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6).

Para saber mais sobre os conceitos de desenvolvimento do Signalr, visite os seguintes sites para código-fonte do Signalr e recursos:

- [Projeto do signalr](http://signalr.net)
- [GitHub e exemplos do signalr](https://github.com/SignalR/SignalR)
- [Wiki do signalr](https://github.com/SignalR/SignalR/wiki)
