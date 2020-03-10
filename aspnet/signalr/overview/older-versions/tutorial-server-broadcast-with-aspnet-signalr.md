---
uid: signalr/overview/older-versions/tutorial-server-broadcast-with-aspnet-signalr
title: 'Tutorial: transmissão de servidor com o Signalr 1. x do ASP.NET | Microsoft Docs'
author: bradygaster
description: Este tutorial mostra como criar um aplicativo Web que usa o Signalr ASP.NET para fornecer a funcionalidade de difusão do servidor. Difusão do servidor significa que Communic...
ms.author: bradyg
ms.date: 04/10/2013
ms.assetid: ab7b2554-956a-4f6d-b2a0-4ae0c62e8580
msc.legacyurl: /signalr/overview/older-versions/tutorial-server-broadcast-with-aspnet-signalr
msc.type: authoredcontent
ms.openlocfilehash: 68908be34f6b010e512677fe5f5e31bfdefab592
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579404"
---
# <a name="tutorial-server-broadcast-with-aspnet-signalr-1x"></a>Tutorial: transmissão de servidor com o Signalr 1. x ASP.NET

por [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Este tutorial mostra como criar um aplicativo Web que usa o Signalr ASP.NET para fornecer a funcionalidade de difusão do servidor. A difusão do servidor significa que as comunicações enviadas aos clientes são iniciadas pelo servidor. Esse cenário requer uma abordagem de programação diferente dos cenários ponto a ponto, como aplicativos de chat, nos quais as comunicações enviadas aos clientes são iniciadas por um ou mais dos clientes.
> 
> O aplicativo que você criará neste tutorial simula uma cotação de ações, um cenário típico para a funcionalidade de difusão do servidor.
> 
> Comentários sobre o tutorial são bem-vindos. Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, poderá lançá-las no fórum do [signalr ASP.net](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [stackoverflow.com](http://stackoverflow.com).

## <a name="overview"></a>Visão geral

O pacote NuGet [Microsoft. AspNet. signalr. Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) instala um exemplo de aplicativo de cotação de ações simuladas em um projeto do Visual Studio. Na primeira parte deste tutorial, você criará uma versão simplificada desse aplicativo a partir do zero. No restante do tutorial, você instalará o pacote NuGet e examinará os recursos adicionais e o código que ele cria.

O aplicativo de cotação de ações é um representante de um tipo de aplicativo em tempo real no qual você deseja "enviar por push", ou transmitir, periodicamente notificações do servidor para todos os clientes conectados.

O aplicativo que você criará na primeira parte deste tutorial exibirá uma grade com dados de ações.

![Versão inicial do StockTicker](tutorial-server-broadcast-with-aspnet-signalr/_static/image1.png)

Periodicamente, o servidor atualiza de forma aleatória os preços de ações e envia por push as atualizações para todos os clientes conectados. No navegador, os números e símbolos nas colunas de **alteração** e **%** mudam dinamicamente em resposta a notificações do servidor. Se você abrir navegadores adicionais para a mesma URL, todos eles mostrarão os mesmos dados e as mesmas alterações aos dados simultaneamente.

Este tutorial contém as seguintes seções:

- [Pré-requisitos](#prerequisites)
- [Criar o projeto](#createproject)
- [Adicionar os pacotes NuGet do Signalr](#nugetpackages)
- [Configurar o código do servidor](#server)
- [Configurar o código do cliente](#client)
- [Testar o aplicativo](#test)
- [Habilitar o registro em log](#enablelogging)
- [Instalar e examinar o exemplo de StockTicker completo](#fullsample)
- [Próximas etapas](#nextsteps)

> [!NOTE]
> Se você não quiser trabalhar com as etapas de criação do aplicativo, poderá instalar o pacote Signalr. Sample em um novo projeto de **aplicativo Web ASP.net vazio** e ler essas etapas para obter explicações sobre o código. A primeira parte do tutorial aborda um subconjunto do código Signalr. Sample, e a segunda parte explica os principais recursos da funcionalidade adicional no pacote Signalr. Sample.

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Prerequisites

Antes de começar, verifique se você tem o Visual Studio 2012 ou 2010 SP1 instalado no computador. Se você não tiver o Visual Studio, consulte [downloads do ASP.net](https://www.asp.net/downloads) para obter o visual Studio 2012 Express for Web gratuito.

Se você tiver o Visual Studio 2010, verifique se o [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) está instalado.

<a id="createproject"></a>

## <a name="create-the-project"></a>Criar o projeto

1. No menu **Arquivo**, clique em **Novo Projeto**.
2. Na caixa de diálogo **novo projeto** , expanda **C#** em **modelos** e selecione **Web**.
3. Selecione o modelo de **aplicativo Web ASP.net vazio** , nomeie o Project *signalr. StockTicker*e clique em **OK**.

    ![Caixa de diálogo Novo Projeto](tutorial-server-broadcast-with-aspnet-signalr/_static/image2.png)

<a id="nugetpackages"></a>

## <a name="add-the-signalr-nuget-packages"></a>Adicionar os pacotes NuGet do Signalr

### <a name="add-the-signalr-and-jquery-nuget-packages"></a>Adicionar os pacotes do Signalr e do NuGet do JQuery

Você pode adicionar a funcionalidade do Signalr a um projeto instalando um pacote NuGet.

1. Clique em **ferramentas | Gerenciador de pacotes NuGet | Console do Gerenciador de pacotes**.
2. Insira o comando a seguir no Gerenciador de pacotes.

    [!code-powershell[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample1.ps1)]

    O pacote do Signalr instala vários outros pacotes NuGet como dependências. Quando a instalação for concluída, você terá todos os componentes de cliente e servidor necessários para usar o Signalr em um aplicativo ASP.NET.

<a id="server"></a>

## <a name="set-up-the-server-code"></a>Configurar o código de servidor

Nesta seção, você configura o código que é executado no servidor.

### <a name="create-the-stock-class"></a>Criar a classe Stock

Comece criando a classe de modelo de estoque que você usará para armazenar e transmitir informações sobre um estoque.

1. Crie um novo arquivo de classe na pasta do projeto, nomeie-o *stock.cs*e substitua o código do modelo pelo código a seguir:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample2.cs)]

    As duas propriedades que você definirá quando criar ações serão o símbolo (por exemplo, MSFT para Microsoft) e o preço. As outras propriedades dependem de como e quando você define o preço. Na primeira vez que você definir o preço, o valor será propagado para DayOpen. Os tempos subsequentes quando você define o preço, os valores da propriedade Change e PercentChange são calculados com base na diferença entre Price e DayOpen.

### <a name="create-the-stockticker-and-stocktickerhub-classes"></a>Criar as classes StockTicker e StockTickerHub

Você usará a API do Hub do Signalr para lidar com a interação de servidor para cliente. Uma classe StockTickerHub que deriva da classe Hub do Signalr tratará conexões de recebimento e chamadas de método de clientes. Você também precisa manter os dados de estoque e executar um objeto de timer para disparar periodicamente as atualizações de preço, independentemente das conexões de cliente. Você não pode colocar essas funções em uma classe de Hub, pois as instâncias de Hub são transitórias. Uma instância de classe de Hub é criada para cada operação no Hub, como conexões e chamadas do cliente para o servidor. Portanto, o mecanismo que mantém os dados de estoque, atualiza os preços e transmite as atualizações de preço deve ser executado em uma classe separada, que você chamará de StockTicker.

![Transmitindo do StockTicker](tutorial-server-broadcast-with-aspnet-signalr/_static/image4.png)

Você deseja que apenas uma instância da classe StockTicker seja executada no servidor, portanto, você precisará configurar uma referência de cada instância do StockTickerHub para a instância StockTicker singleton. A classe StockTicker precisa ser capaz de difundir para os clientes porque ele tem os dados de estoque e as atualizações de gatilho, mas StockTicker não é uma classe de Hub. Portanto, a classe StockTicker precisa obter uma referência ao objeto de contexto de conexão do Hub Signalr. Em seguida, ele pode usar o objeto de contexto de conexão do Signalr para transmitir para os clientes.

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e clique em **Adicionar novo item**.
2. Se você tiver o Visual Studio 2012 com a [atualização do ASP.net and Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=279941), clique em **Web** em **Visual C#**  e selecione o modelo de item de **classe Hub do signalr** . Caso contrário, selecione o modelo de **classe** .
3. Nomeie a nova classe *StockTickerHub.cs*e clique em **Adicionar**.

    ![Adicionar StockTickerHub.cs](tutorial-server-broadcast-with-aspnet-signalr/_static/image5.png)
4. Substitua o código do modelo pelo seguinte código:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample3.cs)]

    A classe [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) é usada para definir métodos que os clientes podem chamar no servidor. Você está definindo um método: `GetAllStocks()`. Quando um cliente se conecta inicialmente ao servidor, ele chamará esse método para obter uma lista de todas as ações com seus preços atuais. O método pode ser executado de forma síncrona e retornar `IEnumerable<Stock>` porque ele está retornando dados da memória. Se o método tivesse que obter os dados fazendo algo que envolvesse esperar, como uma pesquisa de banco de dado ou uma chamada de serviço Web, você especificaria `Task<IEnumerable<Stock>>` como o valor de retorno para habilitar o processamento assíncrono. Para obter mais informações, consulte [guia da API de hubs do ASP.net signalr-Server-quando executar de forma assíncrona](index.md).

    O atributo HubName especifica como o Hub será referenciado no código JavaScript no cliente. O nome padrão no cliente se você não usar esse atributo é uma versão do nome de classe do camel case, que nesse caso seria stockTickerHub.

    Como você verá posteriormente ao criar a classe StockTicker, uma instância singleton dessa classe é criada em sua propriedade de instância estática. Essa instância singleton do StockTicker permanece na memória, independentemente de quantos clientes se conectam ou se desconectam, e essa instância é o que o método GetAllStocks usa para retornar informações de ações atuais.
5. Crie um novo arquivo de classe na pasta do projeto, nomeie-o *StockTicker.cs*e substitua o código do modelo pelo código a seguir:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample4.cs)]

    Como vários threads executarão a mesma instância do código StockTicker, a classe StockTicker deve ser threadsafe.

    ### <a name="storing-the-singleton-instance-in-a-static-field"></a>Armazenando a instância singleton em um campo estático

    O código inicializa o campo de instância de \_estática que faz o backup da propriedade de instância com uma instância da classe, e essa é a única instância da classe que pode ser criada, pois o construtor é marcado como particular. A [inicialização lenta](https://msdn.microsoft.com/library/dd997286.aspx) é usada para o campo de instância de \_, não por motivos de desempenho, mas para garantir que a criação da instância seja threadsafe.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample5.cs)]

    Cada vez que um cliente se conecta ao servidor, uma nova instância da classe StockTickerHub em execução em um thread separado Obtém a instância singleton StockTicker da propriedade estática StockTicker. Instance, como vimos anteriormente na classe StockTickerHub.

    ### <a name="storing-stock-data-in-a-concurrentdictionary"></a>Armazenando dados de ações em um ConcurrentDictionary

    O construtor inicializa a coleção \_stocks com alguns dados de estoque de exemplo e GetAllStocks retorna as ações. Como vimos anteriormente, essa coleção de ações é, por sua vez, retornada por StockTickerHub. GetAllStocks, que é um método de servidor na classe de Hub que os clientes podem chamar.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample6.cs)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample7.cs)]

    A coleção de ações é definida como um tipo de [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) para segurança de thread. Como alternativa, você pode usar um objeto [Dictionary](https://msdn.microsoft.com/library/xfhwa508.aspx) e bloquear explicitamente o dicionário quando fizer alterações nele.

    Para este aplicativo de exemplo, é possível armazenar dados de aplicativo na memória e perder os dados quando a instância StockTicker for descartada. Em um aplicativo real, você trabalharia com um armazenamento de dados de back-end, como um Database.

    ### <a name="periodically-updating-stock-prices"></a>Atualizando periodicamente os preços de ações

    O Construtor inicia um objeto de temporizador que chama periodicamente os métodos que atualizam os preços de ações em uma base aleatória.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample8.cs)]

    UpdateStockPrices é chamado pelo timer, que passa em NULL no parâmetro State. Antes de atualizar os preços, um bloqueio é obtido no objeto \_updateStockPricesLock. O código verifica se outro thread já está atualizando os preços e, em seguida, chama TryUpdateStockPrice em cada estoque na lista. O método TryUpdateStockPrice decide se o preço da ação deve ser alterado e o quanto alterar. Se o preço da ação for alterado, BroadcastStockPrice será chamado para difundir a alteração de preço de estoque para todos os clientes conectados.

    O sinalizador \_updatingStockPrices é marcado como [volátil](https://msdn.microsoft.com/library/x13ttww7.aspx) para garantir que o acesso a ele seja threadsafe.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample9.cs)]

    Em um aplicativo real, o método TryUpdateStockPrice chamaria um serviço Web para pesquisar o preço; Nesse código, ele usa um gerador de números aleatórios para fazer alterações aleatoriamente.

    ### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a>Obter o contexto do Signalr para que a classe StockTicker possa difundir para os clientes

    Como as alterações de preço são originadas aqui no objeto StockTicker, esse é o objeto que precisa chamar um método updateStockPrice em todos os clientes conectados. Em uma classe de Hub, você tem uma API para chamar métodos de cliente, mas StockTicker não deriva da classe Hub e não tem uma referência a nenhum objeto Hub. Portanto, para difundir para clientes conectados, a classe StockTicker precisa obter a instância de contexto do Signalr para a classe StockTickerHub e usá-la para chamar métodos em clientes.

    O código obtém uma referência ao contexto do Signalr quando ele cria a instância da classe singleton, passa essa referência para o construtor e o Construtor a coloca na propriedade clients.

    Há dois motivos pelos quais você deseja obter o contexto apenas uma vez: obter o contexto é uma operação cara e obtê-lo uma vez garante que a ordem pretendida de mensagens enviadas aos clientes é preservada.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample10.cs)]

    Obter a propriedade clients do contexto e colocá-la na propriedade StockTickerClient permite que você escreva o código para chamar os métodos de cliente que têm a mesma aparência em uma classe de Hub. Por exemplo, para difundir para todos os clientes, você pode gravar clientes. All. updateStockPrice (stock).

    O método updateStockPrice que você está chamando no BroadcastStockPrice ainda não existe; Você o adicionará posteriormente quando escrever o código que é executado no cliente. Você pode fazer referência a updateStockPrice aqui porque clients. All é dinâmico, o que significa que a expressão será avaliada em tempo de execução. Quando essa chamada de método for executada, o Signalr enviará o nome do método e o valor do parâmetro para o cliente e, se o cliente tiver um método chamado updateStockPrice, esse método será chamado e o valor do parâmetro será passado para ele.

    Clients. All significa enviar para todos os clientes. O signalr oferece outras opções para especificar a quais clientes ou grupos de clientes enviar. Para obter mais informações, consulte [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).

### <a name="register-the-signalr-route"></a>Registrar a rota do Signalr

O servidor precisa saber qual URL interceptar e direcionar para o Signalr. Para fazer isso, você adicionará um código ao arquivo *global. asax* .

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e clique em **Adicionar novo item**.
2. Selecione o modelo de item de **classe de aplicativo global** e clique em **Adicionar**.

    ![Adicionar global. asax](tutorial-server-broadcast-with-aspnet-signalr/_static/image6.png)
3. Adicione o código de registro de rota do Signalr ao aplicativo\_método de início:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample11.cs)]

    Por padrão, a URL base para todo o tráfego de sinalização é "/signalr" e "/signalr/hubs" é usado para recuperar um arquivo JavaScript gerado dinamicamente que define proxies para todos os hubs que você tem em seu aplicativo. O método MapHubs inclui sobrecargas que permitem especificar uma URL base diferente e determinadas opções de Signalr em uma instância da classe [HubConfiguration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubconfiguration(v=vs.111).aspx) .
4. Adicione uma instrução using na parte superior do arquivo:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample12.cs)]
5. Salve e feche o arquivo *global. asax* e compile o projeto.

Agora você concluiu a configuração do código do servidor. Na próxima seção, você configurará o cliente.

<a id="client"></a>

## <a name="set-up-the-client-code"></a>Configurar o código do cliente

1. Crie um novo arquivo HTML na pasta do projeto e nomeie-o *StockTicker. html*.
2. Substitua o código do modelo pelo seguinte código:

    [!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample13.html)]

    O HTML cria uma tabela com 5 colunas, uma linha de cabeçalho e uma linha de dados com uma única célula que abrange todas as 5 colunas. A linha de dados exibe "carregando..." e será mostrado momentaneamente apenas quando o aplicativo for iniciado. O código JavaScript removerá essa linha e adicionará suas linhas locais com os dados de estoque recuperados do servidor.

    As marcas de script especificam o arquivo de script jQuery, o arquivo de script do Signalr Core, o arquivo de script de proxies do Signalr e um arquivo de script StockTicker que você criará mais tarde. O arquivo de script de proxies do Signalr, que especifica a URL "/signalr/hubs", é gerado dinamicamente e define métodos de proxy para os métodos na classe Hub, nesse caso, para StockTickerHub. GetAllStocks. Se preferir, você pode gerar esse arquivo JavaScript manualmente usando os [utilitários do signalr](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) e desabilitar a criação dinâmica de arquivos na chamada do método MapHubs.
3. > [!IMPORTANT]
   > Verifique se as referências de arquivo JavaScript em *StockTicker. html* estão corretas. Ou seja, certifique-se de que a versão do jQuery na sua marca de script (1.8.2 no exemplo) seja a mesma que a versão do jQuery na pasta de *scripts* do projeto e certifique-se de que a versão do signalr na marca do script seja igual à versão do signalr na pasta de *scripts* do projeto. Altere os nomes de arquivo nas marcas de script, se necessário.
4. No **Gerenciador de soluções**, clique com o botão direito do mouse em *StockTicker. html*e clique em **definir como página inicial**.
5. Crie um novo arquivo JavaScript na pasta do projeto e nomeie-o *StockTicker. js*.
6. Substitua o código do modelo pelo seguinte código:

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample14.js)]

    $. conexão refere-se aos proxies do Signalr. O código obtém uma referência ao proxy para a classe StockTickerHub e a coloca na variável de marca. O nome do proxy é o nome que foi definido pelo atributo [HubName]:

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample15.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample16.cs)]

    Depois que todas as variáveis e funções são definidas, a última linha de código no arquivo Inicializa a conexão do Signalr chamando a função de início do Signalr. A função Start é executada de forma assíncrona e retorna um [objeto do jQuery adiado](http://api.jquery.com/category/deferred-object/), o que significa que você pode chamar a função Done para especificar a função a ser chamada quando a operação assíncrona for concluída.

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample17.js)]

    A função init chama a função getAllStocks no servidor e usa as informações que o servidor retorna para atualizar a tabela de estoque. Observe que, por padrão, você precisa usar o camel case no cliente, embora o nome do método seja Pascal-case no servidor. A regra de camel-case só se aplica a métodos, não a objetos. Por exemplo, você se refere a ações. Símbolo e estoque. Preço, não estoque. símbolo ou estoque. preço.

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample18.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample19.cs)]

    Se você quisesse usar a capitalização de Pascal no cliente ou se quisesse usar um nome de método completamente diferente, poderia decorar o método de Hub com o atributo HubMethodName da mesma forma que você decorau a própria classe de Hub com o atributo HubName.

    No método Init, o HTML para uma linha de tabela é criado para cada objeto de estoque recebido do servidor chamando formatStock para formatar as propriedades do objeto Stock e, em seguida, chamando suplantar (que é definido na parte superior de *StockTicker. js*) para substituir os espaços reservados na variável rowgroup pelos valores de Propriedade do objeto stock. O HTML resultante é então anexado à tabela stock.

    Chame init passando-o como uma função de retorno de chamada que é executada após a conclusão da função de início assíncrono. Se você chamou init como uma instrução JavaScript separada depois de chamar Start, a função falhará porque ela seria executada imediatamente sem esperar que a função start termine de estabelecer a conexão. Nesse caso, a função init tentaria chamar a função getAllStocks antes que a conexão do servidor seja estabelecida.

    Quando o servidor altera o preço de uma ação, ele chama o updateStockPrice em clientes conectados. A função é adicionada à propriedade Client do proxy stockTicker para disponibilizá-la a chamadas do servidor.

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample20.js)]

    A função updateStockPrice formata um objeto de estoque recebido do servidor para uma linha de tabela da mesma maneira que na função init. No entanto, em vez de acrescentar a linha à tabela, ela localiza a linha atual do estoque na tabela e substitui essa linha pela nova.

<a id="test"></a>

## <a name="test-the-application"></a>Testar o aplicativo

1. Pressione F5 para executar o aplicativo em modo de depuração.

    A tabela Stock inicialmente exibe o "carregando..." , depois de um pequeno atraso, os dados iniciais de ações são exibidos e, em seguida, os preços de ações começam a ser alterados.

    ![Carregando](tutorial-server-broadcast-with-aspnet-signalr/_static/image7.png)

    ![Tabela de estoque inicial](tutorial-server-broadcast-with-aspnet-signalr/_static/image8.png)

    ![Tabela de ações recebendo alterações do servidor](tutorial-server-broadcast-with-aspnet-signalr/_static/image9.png)
2. Copie a URL da barra de endereços do navegador e cole-a em uma ou mais novas janela (s) do navegador.

    A exibição de estoque inicial é a mesma do primeiro navegador e as alterações acontecem simultaneamente.
3. Feche todos os navegadores e abra um novo navegador e vá para a mesma URL.

    O objeto singleton StockTicker continuou a ser executado no servidor, portanto, a exibição da tabela Stock mostra que as ações continuaram a ser alteradas. (Você não vê a tabela inicial com valores de alteração zero.)
4. Feche o navegador.

<a id="enablelogging"></a>

## <a name="enable-logging"></a>Habilitar o registro em log

O signalr tem uma função de log interna que você pode habilitar no cliente para auxiliar na solução de problemas. Nesta seção, você habilitará o registro em log e verá exemplos que mostram como os logs informam quais dos seguintes métodos de transporte o Signalr está usando:

- [WebSockets](http://en.wikipedia.org/wiki/WebSocket), com suporte do IIS 8 e navegadores atuais.
- [Eventos enviados pelo servidor](http://en.wikipedia.org/wiki/Server-sent_events), com suporte de navegadores diferentes do Internet Explorer.
- [Quadro contínuo](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), com suporte do Internet Explorer.
- [Sondagem longa do AJAX](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), com suporte de todos os navegadores.

Para qualquer conexão fornecida, o Signalr escolhe o melhor método de transporte que o servidor e o cliente oferecem suporte.

1. Abra *StockTicker. js* e adicione uma linha de código para habilitar o registro em log imediatamente antes do código que inicializa a conexão no final do arquivo:

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample21.js)]
2. Pressione F5 para executar o projeto.
3. Abra a janela de ferramentas de desenvolvedor do navegador e selecione o console do para ver os logs. Talvez seja necessário atualizar a página para ver os logs do Signalr negociando o método de transporte para uma nova conexão.

    Se você estiver executando o Internet Explorer 10 no Windows 8 (IIS 8), o método de transporte será WebSockets.

    ![Console do IE 10 IIS 8](tutorial-server-broadcast-with-aspnet-signalr/_static/image10.png)

    Se você estiver executando o Internet Explorer 10 no Windows 7 (IIS 7,5), o método de transporte será iframe.

    ![Console do IE 10, IIS 7,5](tutorial-server-broadcast-with-aspnet-signalr/_static/image11.png)

    No Firefox, instale o suplemento do Firebug para obter uma janela de console. Se você estiver executando o Firefox 19 no Windows 8 (IIS 8), o método de transporte será WebSockets.

    ![WebSockets do Firefox 19 IIS 8](tutorial-server-broadcast-with-aspnet-signalr/_static/image12.png)

    Se você estiver executando o Firefox 19 no Windows 7 (IIS 7,5), o método de transporte será um evento enviado pelo servidor.

    ![Console do Firefox 19 IIS 7,5](tutorial-server-broadcast-with-aspnet-signalr/_static/image13.png)

<a id="fullsample"></a>

## <a name="install-and-review-the-full-stockticker-sample"></a>Instalar e examinar o exemplo de StockTicker completo

O aplicativo StockTicker que é instalado pelo pacote do NuGet [Microsoft. AspNet. signaler. Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) inclui mais recursos do que a versão simplificada que você acabou de criar do zero. Nesta seção do tutorial, você instala o pacote NuGet e examina os novos recursos e o código que os implementa.

### <a name="install-the-signalrsample-nuget-package"></a>Instalar o pacote NuGet. Sample do Signalr

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e clique em **gerenciar pacotes NuGet**.
2. Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **online**, *digite signalr. Sample* na caixa **Pesquisar online** e clique em **instalar** no pacote **signalr. Sample** .

    ![Instalar um pacote. Sample do Signalr](tutorial-server-broadcast-with-aspnet-signalr/_static/image14.png)
3. No arquivo *global. asax* , comente o RouteTable. routes. MapHubs (); linha que você adicionou anteriormente no método Start do\_do aplicativo.

    O código em *global. asax* não é mais necessário porque o pacote signalr. Sample registra a rota do signalr no *aplicativo\_Start/RegisterHubs. cs* file:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample22.cs)]

    A classe webactivator que é referenciada pelo atributo assembly está incluída no pacote NuGet WebActivatorEx, que é instalado como uma dependência do pacote Signalr. Sample.
4. Em **Gerenciador de soluções**, expanda a pasta *signalr. Sample* que foi criada pela instalação do pacote signalr. Sample.
5. Na pasta *signalr. Sample* , clique com o botão direito do mouse em *StockTicker. html*e clique em **definir como página inicial**.

    > [!NOTE]
    > A instalação do pacote NuGet do Signalr. Sample pode alterar a versão do jQuery que você tem em sua pasta *scripts* . O novo arquivo *StockTicker. html* que o pacote instala na pasta *signalr. Sample* estará em sincronia com a versão do jQuery instalada pelo pacote, mas se você quiser executar o arquivo *StockTicker. html* original novamente, talvez seja necessário atualizar a referência do jQuery na marca do script primeiro.

### <a name="run-the-application"></a>Executar o aplicativo

1. Pressione F5 para executar o aplicativo.

    Além da grade que você viu anteriormente, o aplicativo de cotação de ações completo mostra uma janela de rolagem horizontal que exibe os mesmos dados de ações. Quando você executa o aplicativo pela primeira vez, o "mercado" é "fechado" e você vê uma grade estática e uma janela de marcação que não está rolando.

    ![Início da tela StockTicker](tutorial-server-broadcast-with-aspnet-signalr/_static/image15.png)

    Quando você clica em **abrir mercado**, a caixa de Cotações de **estoque ao vivo** começa a rolar horizontalmente, e o servidor começa a transmitir periodicamente as alterações de preço de ações aleatoriamente. Cada vez que o preço de uma ação é alterado, a grade da **tabela de estoque ao** vivo e a caixa de Cotações de **estoque ao vivo** são atualizadas. Quando a alteração de preço de um estoque é positiva, o estoque é mostrado com um plano de fundo verde e, quando a alteração é negativa, o estoque é mostrado com um plano de fundo vermelho.

    ![Aplicativo StockTicker, mercado aberto](tutorial-server-broadcast-with-aspnet-signalr/_static/image16.png)

    O botão **fechar mercado** interrompe as alterações e interrompe a rolagem do marcador e o botão **Redefinir** redefine todos os dados de ações para o estado inicial antes do início das alterações de preço. Se você abrir mais janelas do navegador e ir para a mesma URL, verá os mesmos dados atualizados dinamicamente ao mesmo tempo em cada navegador. Quando você clica em um dos botões, todos os navegadores respondem da mesma maneira ao mesmo tempo.

### <a name="live-stock-ticker-display"></a>Exibição de Cotações de estoque ao vivo

A exibição de **Cotações de estoque ao vivo** é uma lista não ordenada em um elemento div que é formatada em uma única linha por estilos CSS. O marcador é inicializado e atualizado da mesma maneira que a tabela: substituindo espaços reservados em uma cadeia de caracteres de modelo &lt;li&gt; e adicionando dinamicamente os elementos de&gt; de &lt;li ao elemento &lt;UL&gt;. A rolagem é executada usando a função de animação jQuery para variar a margem esquerda da lista não ordenada dentro da div.

O HTML da bolsa de ações:

[!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample23.html)]

O CSS de Cotações de ações:

[!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample24.html)]

O código do jQuery que faz a rolagem:

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample25.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a>Métodos adicionais no servidor que o cliente pode chamar

A classe StockTickerHub define quatro métodos adicionais que o cliente pode chamar:

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample26.cs)]

OpenMarket, CloseMarket e Reset são chamados em resposta aos botões na parte superior da página. Eles demonstram o padrão de um cliente que dispara uma alteração no estado que é propagada imediatamente para todos os clientes. Cada um desses métodos chama um método na classe StockTicker que afeta a alteração de estado do mercado e, em seguida, transmite o novo estado.

Na classe StockTicker, o estado do mercado é mantido por uma propriedade Marketstate que retorna um valor de enumeração Marketstate:

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample27.cs)]

Cada um dos métodos que alteram o estado do mercado faz isso dentro de um bloco de bloqueio porque a classe StockTicker deve ser threadsafe:

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample28.cs)]

Para garantir que esse código seja threadsafe, o campo marketstate \_que faz backup da propriedade Marketstate é marcado como volátil,

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample29.cs)]

Os métodos BroadcastMarketStateChange e BroadcastMarketReset são semelhantes ao método BroadcastStockPrice que você já viu, exceto que eles chamam métodos diferentes definidos no cliente:

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample30.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a>Funções adicionais no cliente que o servidor pode chamar

A função updateStockPrice agora lida com a exibição da grade e da marca e usa jQuery. Color para piscar as cores vermelhas e verdes.

Novas funções no *signalr. StockTicker. js* habilitam e desabilitam os botões com base no estado do mercado, e eles param ou iniciam a rolagem horizontal da janela de marcação. Como várias funções estão sendo adicionadas ao tickr. Client, a [função de extensão jQuery](http://api.jquery.com/jQuery.extend/) é usada para adicioná-las.

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample31.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a>Configuração adicional do cliente depois de estabelecer a conexão

Depois que o cliente estabelece a conexão, ele tem algum trabalho adicional a fazer: Descubra se o mercado está aberto ou fechado para chamar a função marketOpened ou marketClosed apropriada e anexe as chamadas de método de servidor aos botões.

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample32.js)]

Os métodos de servidor não são conectados aos botões até que a conexão seja estabelecida, para que o código não possa tentar chamar os métodos de servidor antes que eles estejam disponíveis.

<a id="nextsteps"></a>

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu a programar um aplicativo Signalr que transmite mensagens do servidor para todos os clientes conectados, periodicamente e em resposta a notificações de qualquer cliente. O padrão de usar uma instância singleton multi-threaded para manter o estado do servidor também pode ser usado em cenários de jogos online de vários jogadores. Para obter um exemplo, consulte [o jogo de partida que se baseia no signalr](https://github.com/NTaylorMullen/ShootR).

Para obter tutoriais que mostrem cenários de comunicação ponto a ponto, confira [introdução com sinalizador](index.md) e [atualização em tempo real com o signalr](index.md).

Para saber mais sobre os conceitos avançados de desenvolvimento do Signalr, visite os seguintes sites para código-fonte e recursos do Signalr:

- [ASP.NET SignalR](https://asp.net/signalr/)
- [Projeto do signalr](http://signalr.net/)
- [GitHub e exemplos do signalr](https://github.com/SignalR/SignalR)
- [Wiki do signalr](https://github.com/SignalR/SignalR/wiki)
