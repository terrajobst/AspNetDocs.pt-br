---
uid: signalr/overview/getting-started/real-time-web-applications-with-signalr
title: 'Laboratório prático: aplicativos Web em tempo real com Signalr | Microsoft Docs'
author: bradygaster
description: Os aplicativos Web em tempo real apresentam a capacidade de enviar por push o conteúdo do lado do servidor para os clientes conectados conforme eles ocorrem, em tempo real. Para desenvolvedores de ASP.NET, ASP...
ms.author: bradyg
ms.date: 07/16/2014
ms.assetid: ba07958c-42e1-4da0-81db-ba6925ed6db0
msc.legacyurl: /signalr/overview/getting-started/real-time-web-applications-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 9e39fd3f2fc9d4e791002450085215096c222fcd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78537096"
---
# <a name="hands-on-lab-real-time-web-applications-with-signalr"></a>Laboratório prático: aplicativos Web em tempo real com Signalr

por [equipe de acampamentos da Web](https://twitter.com/webcamps)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

[Baixe o Web acampamentos Training Kit, versão de outubro de 2015](https://github.com/Microsoft-Web/WebCampTrainingKit/releases/tag/v2015.10.13b)

> Os aplicativos Web em tempo real apresentam a capacidade de enviar por push o conteúdo do lado do servidor para os clientes conectados conforme eles ocorrem, em tempo real. Para desenvolvedores de ASP.NET, o **signalr ASP.net** é uma biblioteca para adicionar funcionalidade da Web em tempo real a seus aplicativos. Ele tira proveito de vários transportes, selecionando automaticamente o melhor transporte disponível, considerando o melhor transporte disponível do cliente e do servidor. Ele aproveita o **WebSocket**, uma API HTML5 que permite a comunicação bidirecional entre o navegador e o servidor.
> 
> O **signalr** também fornece uma API simples e de alto nível para fazer RPC do servidor para o cliente (chamar funções JavaScript nos navegadores de seus clientes do código .net do lado do servidor) em seu aplicativo ASP.net, bem como adicionar ganchos úteis para o gerenciamento de conexões, como eventos de conexão/desconexão, agrupamento de conexões e autorização.
> 
> O **signalr** é uma abstração de alguns dos transportes que são necessários para realizar o trabalho em tempo real entre o cliente e o servidor. Uma conexão de **signalr** inicia como http e é promovida para uma conexão **WebSocket** , se disponível. **WebSocket** é o transporte ideal para **signalr**, já que ele faz o uso mais eficiente da memória do servidor, tem a menor latência e tem os recursos mais subjacentes (como a comunicação full duplex entre cliente e servidor), mas também tem os requisitos mais rígidos: **WebSocket** requer que o servidor esteja usando o **Windows Server 2012** ou o **Windows 8**, juntamente com **.NET Framework 4,5**. Se esses requisitos não forem atendidos, o **signalr** tentará usar outros transportes para fazer suas conexões (como *sondagem longa do AJAX*).
> 
> A API do **signalr** contém dois modelos para a comunicação entre clientes e servidores: **conexões persistentes** e **hubs**. Uma **conexão** representa um ponto de extremidade simples para enviar mensagens de destinatário único, agrupadas ou de difusão. Um **Hub** é um pipeline de nível mais alto criado com base na API de conexão que permite que o cliente e o servidor chamem métodos entre si diretamente.
> 
> ![Arquitetura do signalr](real-time-web-applications-with-signalr/_static/image1.png)
> 
> Todos os códigos de exemplo e trechos de código estão incluídos no kit de treinamento do acampamentos da Web, versão de outubro de 2015, disponível em [https://github.com/Microsoft-Web/WebCampTrainingKit/releases/tag/v2015.10.13b](https://github.com/Microsoft-Web/WebCampTrainingKit/releases/tag/v2015.10.13b).  Observe que o link do instalador nessa página não funciona mais; em vez disso, use um dos links na seção ativos.

<a id="Overview"></a>
## <a name="overview"></a>Visão geral

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

Neste laboratório prático, você aprenderá a:

- Enviar notificações do servidor para o cliente usando o Signalr.
- Scale Out seu aplicativo Signalr usando **SQL Server**.

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisites

O seguinte é necessário para concluir este laboratório prático:

- [Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/) ou superior

<a id="Setup"></a>
### <a name="setup"></a>Instalação

Para executar os exercícios neste laboratório prático, você precisará configurar seu ambiente primeiro...

1. Abra uma janela do Windows Explorer e navegue até a pasta de **origem** do laboratório.
2. Clique com o botão direito do mouse em **Setup. cmd** e selecione **Executar como administrador** para iniciar o processo de instalação que irá configurar seu ambiente e instalar os trechos de código do Visual Studio para este laboratório.
3. Se a caixa de diálogo controle de conta de usuário for exibida, confirme a ação para continuar.

> [!NOTE]
> Verifique se você verificou todas as dependências deste laboratório antes de executar a instalação.

<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Usando os trechos de código

Em todo o documento do laboratório, você será instruído a inserir blocos de código. Para sua conveniência, a maior parte desse código é fornecida como trechos de Visual Studio Code, que podem ser acessados em Visual Studio 2013 para evitar a necessidade de adicioná-lo manualmente.

> [!NOTE]
> Cada exercício é acompanhado por uma solução inicial localizada na pasta **begin** do exercício que permite que você siga cada exercício independentemente dos outros. Lembre-se de que os trechos de código que são adicionados durante um exercício estão ausentes dessas soluções iniciais e podem não funcionar até que você conclua o exercício. Dentro do código-fonte de um exercício, você também encontrará uma pasta **final** contendo uma solução do Visual Studio com o código que resulta da conclusão das etapas no exercício correspondente. Você pode usar essas soluções como diretrizes se precisar de ajuda adicional ao trabalhar com este laboratório prático.

---

<a id="Exercises"></a>
## <a name="exercises"></a>Exercícios

Este laboratório prático inclui os seguintes exercícios:

1. [Trabalhando com dados em tempo real usando Signalr](#Exercise1)
2. [Dimensionamento usando SQL Server](#Exercise2)

Tempo estimado para concluir este laboratório: **60 minutos**

> [!NOTE]
> Ao iniciar o Visual Studio pela primeira vez, você deve selecionar uma das coleções de configurações predefinidas. Cada coleção predefinida é projetada para corresponder a um estilo de desenvolvimento específico e determina layouts de janela, comportamento do editor, trechos de código IntelliSense e opções da caixa de diálogo. Os procedimentos neste laboratório descrevem as ações necessárias para realizar uma determinada tarefa no Visual Studio ao usar a coleção de **configurações de desenvolvimento geral** . Se você escolher uma coleção de configurações diferentes para seu ambiente de desenvolvimento, poderá haver diferenças nas etapas que você deve levar em conta.

<a id="Exercise1"></a>
### <a name="exercise-1-working-with-real-time-data-using-signalr"></a>Exercício 1: trabalhando com dados em tempo real usando o Signalr

Embora o chat seja geralmente usado como exemplo, você pode fazer muito mais com a funcionalidade da Web em tempo real. Sempre que um usuário atualiza uma página da Web para ver novos dados ou a página implementa a sondagem longa Ajax para recuperar novos dados, você pode usar o Signalr.

O signalr dá suporte à funcionalidade de Push ou de **difusão** **do servidor** ; Ele lida com o gerenciamento de conexão automaticamente. Em conexões HTTP clássicas para comunicação cliente-servidor, a conexão é restabelecida para cada solicitação, mas o Signalr fornece uma conexão persistente entre o cliente e o servidor. No Signalr, o código do servidor chama um código de cliente no navegador usando RPC (chamadas de procedimento remoto), em vez do modelo de solicitação-resposta que conhecemos hoje.

Neste exercício, você configurará o aplicativo de **teste de especialista** para usar o signalr para exibir o painel de estatísticas com as métricas atualizadas sem a necessidade de atualizar a página inteira.

<a id="Ex1Task1"></a>
#### <a name="task-1--exploring-the-geek-quiz-statistics-page"></a>Tarefa 1 – explorando a página de estatísticas do teste de especialista

Nesta tarefa, você passará pelo aplicativo e verificará como a página de estatísticas é mostrada e como você pode melhorar a maneira como as informações são atualizadas.

1. Abra **Visual Studio Express 2013 para Web** e abra a solução **GeekQuiz. sln** localizada na pasta **Source\Ex1-WorkingWithRealTimeData\Begin** .
2. Pressione **F5** para executar a solução. A página de **logon** deve aparecer no navegador.

    ![Executando a solução](real-time-web-applications-with-signalr/_static/image2.png "Executando a solução")

    *Executando a solução*
3. Clique em **registrar** no canto superior direito da página para criar um novo usuário no aplicativo.

    ![Link de registro](real-time-web-applications-with-signalr/_static/image3.png "Link de registro")

    *Link de registro*
4. Na página **registrar** , insira um **nome de usuário** e **senha**e, em seguida, clique em **registrar**.

    ![Registrando um usuário](real-time-web-applications-with-signalr/_static/image4.png "Registrando um usuário")

    *Registrando um usuário*
5. O aplicativo registra a nova conta e o usuário é autenticado e Redirecionado de volta para o home page mostrando a primeira pergunta do teste.
6. Abra a página **estatísticas** em uma nova janela e coloque a página **inicial** e a página **estatísticas** lado a lado.

    ![Janelas lado a lado](real-time-web-applications-with-signalr/_static/image5.png "Janelas lado a lado")

    *Janelas lado a lado*
7. Na **Home** Page, responda à pergunta clicando em uma das opções.

    ![Respondendo a uma pergunta](real-time-web-applications-with-signalr/_static/image6.png "Respondendo a uma pergunta")

    *Respondendo a uma pergunta*
8. Depois de clicar em um dos botões, a resposta deve aparecer.

    ![Pergunta respondida correta](real-time-web-applications-with-signalr/_static/image7.png "Pergunta respondida correta")

    *Pergunta respondida corretamente*
9. Observe que as informações fornecidas na página estatísticas estão desatualizadas. Atualize a página para ver os resultados atualizados.

    ![Página de estatísticas](real-time-web-applications-with-signalr/_static/image8.png "Página de estatísticas")

    *Página de estatísticas*
10. Volte para o Visual Studio e pare a depuração.

<a id="Ex1Task2"></a>
#### <a name="task-2--adding-signalr-to-geek-quiz-to-show-online-charts"></a>Tarefa 2 – adicionando o Signalr ao quiz para pau para mostrar gráficos online

Nesta tarefa, você adicionará o Signalr à solução e enviará atualizações para os clientes automaticamente quando uma nova resposta for enviada ao servidor.

1. No menu **ferramentas** no Visual Studio, selecione **Gerenciador de pacotes NuGet**e clique em **console do Gerenciador de pacotes**.
2. Na janela do **console do Gerenciador de pacotes** , execute o seguinte comando:

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample1.ps1)]

    ![Instalação do pacote do signalr](real-time-web-applications-with-signalr/_static/image9.png "Instalação do pacote do signalr")

    *Instalação do pacote do signalr*

   > [!NOTE]
   > Ao instalar o **signalr** NuGet Packages versão 2.0.2 de um aplicativo totalmente novo MVC 5, você precisará atualizar manualmente os pacotes do **OWIN** para a versão 2.0.1 (ou superior) antes de instalar o signalr. Para fazer isso, você pode executar o seguinte script no **console do Gerenciador de pacotes**:
   > 
   > [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample2.ps1)]
   > 
   > Em uma versão futura do Signalr, as dependências do OWIN serão atualizadas automaticamente.
3. No **Gerenciador de soluções**, expanda a pasta **scripts** e observe que os arquivos *js* do signalr foram adicionados à solução.

    ![Referências de JavaScript do signalr](real-time-web-applications-with-signalr/_static/image10.png "Referências de JavaScript do signalr")

    *Referências de JavaScript do signalr*
4. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto **GeekQuiz** , selecione **Adicionar** | **nova pasta**e nomeie os **hubs**de ti.
5. Clique com o botão direito do mouse na pasta **hubs** e selecione **Adicionar | Novo item**.

    ![Adicionar novo item](real-time-web-applications-with-signalr/_static/image11.png "Adicionar novo item")

    *Adicionar novo item*
6. Na caixa de diálogo **Adicionar novo item** , selecione o **Visual C# | Web | Nó do signalr** no painel esquerdo, selecione **classe de Hub do signalr (v2)** no painel central, nomeie o arquivo **StatisticsHub.cs** e clique em **Adicionar**.

    ![Caixa de diálogo Adicionar novo item](real-time-web-applications-with-signalr/_static/image12.png "Caixa de diálogo Adicionar novo item")

    *Caixa de diálogo Adicionar novo item*
7. Substitua o código na classe **StatisticsHub** pelo código a seguir.

    (Trecho de código- *RealTimeSignalR-EX1-StatisticsHubClass*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample3.cs)]
8. Abra **Startup.cs** e adicione a linha a seguir ao final do método de **configuração** .

    (Trecho de código- *RealTimeSignalR-EX1-MapSignalR*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample4.cs)]
9. Abra a página **StatisticsService.cs** dentro da pasta **Serviços** e adicione as seguintes diretivas using.

    (Trecho de código- *RealTimeSignalR-EX1-UsingDirectives*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample5.cs)]
10. Para notificar os clientes conectados de atualizações, primeiro recupere um objeto de **contexto** para a conexão atual. O objeto **Hub** contém métodos para enviar mensagens a um único cliente ou difundir para todos os clientes conectados. Adicione o método a seguir à classe **StatisticsService** para transmitir os dados de estatísticas.

    (Trecho de código- *RealTimeSignalR-EX1-NotifyUpdatesMethod*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample6.cs)]

    > [!NOTE]
    > No código acima, você está usando um nome de método arbitrário para chamar uma função no cliente (ou seja: *updateStatistics*). O nome do método que você especifica é interpretado como um objeto dinâmico, o que significa que não há nenhuma validação de tempo de compilação ou IntelliSense para ele. A expressão é avaliada em tempo de execução. Quando a chamada de método é executada, o Signalr envia o nome do método e os valores de parâmetro para o cliente. Se o cliente tiver um método que corresponda ao nome, esse método será chamado e os valores de parâmetro serão passados para ele. Se nenhum método correspondente for encontrado no cliente, nenhum erro será gerado. Para obter mais informações, consulte o [guia da API de hubs do signalr ASP.net](../guide-to-the-api/hubs-api-guide-server.md).
11. Abra a página **TriviaController.cs** dentro da pasta **Controllers** e adicione as seguintes diretivas using.

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample7.cs)]
12. Adicione o seguinte código realçado ao método **post** Action.

    (Trecho de código- *RealTimeSignalR-EX1-NotifyUpdatesCall*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample8.cs)]
13. Abra a página **Statistics. cshtml** dentro das **exibições | Pasta base** . Localize a seção **scripts** e adicione as seguintes referências de script no início da seção.

    (Trecho de código- *RealTimeSignalR-EX1-SignalRScriptReferences*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample9.cshtml)]

    > [!NOTE]
    > Quando você adiciona o Signalr e outras bibliotecas de scripts ao seu projeto do Visual Studio, o Gerenciador de pacotes pode instalar uma versão do arquivo de script do Signalr mais recente do que a versão mostrada neste tópico. Certifique-se de que a referência de script em seu código corresponda à versão da biblioteca de scripts instalada em seu projeto.
14. Adicione o seguinte código realçado para conectar o cliente ao Hub do Signalr e atualizar os dados de estatísticas quando uma nova mensagem for recebida do Hub.

    (Trecho de código- *RealTimeSignalR-EX1-SignalRClientCode*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample10.cshtml)]

    Nesse código, você está criando um proxy de Hub e registrando um manipulador de eventos para escutar mensagens enviadas pelo servidor. Nesse caso, você escuta mensagens enviadas por meio do método *updateStatistics* .

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>Tarefa 3 – executando a solução

Nesta tarefa, você executará a solução para verificar se o modo de exibição de estatísticas é atualizado automaticamente usando Signalr depois de responder a uma nova pergunta.

1. Pressione **F5** para executar a solução.

    > [!NOTE]
    > Se ainda não estiver conectado ao aplicativo, faça logon com o usuário que você criou na tarefa 1.
2. Abra a página **estatísticas** em uma nova janela e coloque a página página **inicial** e **estatísticas** lado a lado, como você fez na tarefa 1.
3. Na **Home** Page, responda à pergunta clicando em uma das opções.

    ![Respondendo a outra pergunta](real-time-web-applications-with-signalr/_static/image13.png "Respondendo a outra pergunta")

    *Respondendo a outra pergunta*
4. Depois de clicar em um dos botões, a resposta deve aparecer. Observe que as informações de estatísticas na página são atualizadas automaticamente depois de responder à pergunta com as informações atualizadas sem a necessidade de atualizar a página inteira.

    ![Página de estatísticas atualizada após a resposta](real-time-web-applications-with-signalr/_static/image14.png "Página de estatísticas atualizada após a resposta")

    *Página de estatísticas atualizada após a resposta*

<a id="Exercise2"></a>
### <a name="exercise-2-scaling-out-using-sql-server"></a>Exercício 2: escalar horizontalmente usando SQL Server

Ao dimensionar um aplicativo Web, geralmente você pode escolher entre *expandir* e *escalar* horizontalmente as opções. *Escalar verticalmente* significa usar um servidor maior, com mais recursos (CPU, RAM, etc.) enquanto *expande* significa adicionar mais servidores para lidar com a carga. O problema com o último é que os clientes podem ser roteados para servidores diferentes. Um cliente que está conectado a um servidor não receberá mensagens enviadas de outro servidor.

Você pode resolver esses problemas usando um componente chamado *backplane*para encaminhar mensagens entre servidores. Com um backplane habilitado, cada instância do aplicativo envia mensagens para o backplane e o backplane as encaminha para as outras instâncias do aplicativo.

Atualmente, há três tipos de backplanes para o Signalr:

- **Barramento de serviço do Windows Azure**. O barramento de serviço é uma infraestrutura de mensagens que permite que os componentes enviem mensagens menos rígidas.
- **SQL Server**. O backplane SQL Server grava mensagens em tabelas SQL. O backplane usa Service Broker para mensagens eficientes. No entanto, ele também funcionará se o Service Broker não estiver habilitado.
- **Redis**. Redis é um repositório de chave-valor na memória. O Redis dá suporte a um padrão de publicação/assinatura ("pub/sub") para enviar mensagens.

Cada mensagem é enviada por meio de um barramento de mensagem. Um barramento de mensagem implementa a interface [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) , que fornece uma abstração de publicação/assinatura. As backplanes funcionam substituindo o padrão **IMessageBus** por um barramento projetado para esse backplane.

Cada instância de servidor conecta-se ao backplane através do barramento. Quando uma mensagem é enviada, ela vai para o backplane e o backplane a envia para todos os servidores. Quando um servidor recebe uma mensagem do backplane, ele armazena a mensagem em seu cache local. Em seguida, o servidor entrega mensagens a clientes de seu cache local.

Para obter mais informações sobre como funciona o backplane do Signalr, leia este [artigo](../performance/scaleout-in-signalr.md).

> [!NOTE]
> Há alguns cenários em que um backplane pode se tornar um afunilamento. Aqui estão alguns cenários típicos do Signalr:
> 
> - Difusão de servidor (por exemplo, cotação de ações): os planos de [retransmissão](tutorial-server-broadcast-with-signalr.md) funcionam bem para esse cenário, pois o servidor controla a taxa na qual as mensagens são enviadas.
> - [Cliente para cliente](tutorial-getting-started-with-signalr.md) (por exemplo, chat): nesse cenário, o backplane poderá ser um afunilamento se o número de mensagens for dimensionado com o número de clientes; ou seja, se a taxa de mensagens aumentar proporcionalmente à medida que mais clientes ingressarem.
> - [Tempo real de alta frequência](tutorial-high-frequency-realtime-with-signalr.md) (por exemplo, jogos em tempo real): um backplane não é recomendado para esse cenário.

Neste exercício, você usará **SQL Server** para distribuir mensagens pelo aplicativo de **teste de especialista** . Você executará essas tarefas em um único computador de teste para saber como configurar a configuração, mas para obter o efeito completo, você precisará implantar o aplicativo Signalr em dois ou mais servidores. Você também deve instalar o SQL Server em um dos servidores ou em um servidor dedicado separado.

![Scale Out usando SQL Server diagrama](real-time-web-applications-with-signalr/_static/image15.png)

<a id="Ex2Task1"></a>
#### <a name="task-1---understanding-the-scenario"></a>Tarefa 1-noções básicas sobre o cenário

Nesta tarefa, você executará 2 instâncias do **teste de especialista** que simulam várias instâncias do IIS em seu computador local. Nesse cenário, ao responder perguntas de Trívia em um aplicativo, a atualização não será notificada na página de estatísticas da segunda instância. Essa simulação é semelhante a um ambiente em que seu aplicativo é implantado em várias instâncias e usando um balanceador de carga para se comunicar com eles.

1. Abra a solução **begin. sln** localizada na pasta **Source/EX2-ScalingOutWithSQLServer/Begin** . Depois de carregado, você observará na **Gerenciador de servidores** que a solução tem dois projetos com estruturas idênticas, mas com nomes diferentes. Isso simulará a execução de duas instâncias do mesmo aplicativo em seu computador local.

    ![Comece a solução simulando 2 instâncias de teste de especialista](real-time-web-applications-with-signalr/_static/image16.png "Comece a solução simulando 2 instâncias de teste de especialista")

    *Comece a solução simulando 2 instâncias de teste de especialista*
2. Abra a página de propriedades da solução clicando com o botão direito do mouse no nó da solução e selecionando **Propriedades**. Em **projeto de inicialização**, selecione **vários projetos de inicialização** e altere o valor da **ação** para ambos os projetos para *Iniciar*.

    ![Iniciando vários projetos](real-time-web-applications-with-signalr/_static/image17.png "Iniciando vários projetos")

    *Iniciando vários projetos*
3. Pressione **F5** para executar a solução. O aplicativo iniciará duas instâncias de **teste de especialista** em diferentes portas, simulando várias instâncias do mesmo aplicativo. Fixe um dos navegadores à esquerda e o outro à direita da tela. Faça logon com suas credenciais ou registre um novo usuário. Depois de conectado, mantenha a página Trívia à esquerda e vá para a página **estatísticas** no navegador à direita.

    ![Teste de pau lado a lado](real-time-web-applications-with-signalr/_static/image18.png)

    *Teste de pau lado a lado*

    ![Teste de especialista em portas diferentes](real-time-web-applications-with-signalr/_static/image19.png)

    *Teste de especialista em portas diferentes*
4. Comece a responder a perguntas no navegador esquerdo e você observará que a página de **estatísticas** no navegador direito não está sendo atualizada. Isso ocorre porque o **signalr** usa um cache local para distribuir mensagens entre seus clientes e esse cenário está simulando várias instâncias, portanto, o cache não é compartilhado entre elas. Você pode verificar se o **signalr** está funcionando testando as mesmas etapas, mas usando um único aplicativo. Nas tarefas a seguir, você configurará um backplane para replicar as mensagens entre instâncias.
5. Volte para o Visual Studio e pare a depuração.

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-sql-server-backplane"></a>Tarefa 2 – Criando o SQL Server backplane

Nesta tarefa, você criará um banco de dados que servirá como um backplane para o aplicativo de **teste de especialista** . Você usará **pesquisador de objetos do SQL Server** para procurar o servidor e inicializar o banco de dados. Além disso, você habilitará o **Service Broker**.

1. No **Visual Studio**, abra o **modo de exibição** de menu e selecione **pesquisador de objetos do SQL Server**.
2. Conecte-se à instância do LocalDB clicando com o botão direito do mouse no nó **SQL Server** e selecionando **Adicionar SQL Server...** opção.

    ![Adicionando uma instância de SQL Server](real-time-web-applications-with-signalr/_static/image20.png "Adicionando uma instância de SQL Server")

    *Adicionando uma instância de SQL Server ao Pesquisador de Objetos do SQL Server*
3. Defina o **nome do servidor** como *(LocalDB) \v11.0* e deixe a **autenticação do Windows** como seu modo de autenticação. Clique em **Conectar** para continuar.

    ![Conectando ao LocalDB](real-time-web-applications-with-signalr/_static/image21.png "Conectando ao LocalDB")

    *Conectando ao LocalDB*
4. Agora que você está conectado à instância do LocalDB, será necessário criar um banco de dados que representará o SQL Server backplane para o Signalr. Para fazer isso, clique com o botão direito do mouse no nó **bancos** de dados e selecione **Add New Database**.

    ![Adicionando um novo banco de dados](real-time-web-applications-with-signalr/_static/image22.png "Adicionando um novo banco de dados")

    *Adicionando um novo banco de dados*
5. Defina o nome do banco de dados como *signalr* e clique em **OK** para criá-lo.

    ![Criando o banco de dados Signalr](real-time-web-applications-with-signalr/_static/image23.png "Criando o banco de dados Signalr")

    *Criando o banco de dados Signalr*

    > [!NOTE]
    > Você pode escolher qualquer nome para o banco de dados.
6. Para receber atualizações com mais eficiência do backplane, é recomendável habilitar Service Broker para o banco de dados. O Service Broker fornece suporte nativo para mensagens e enfileiramento no SQL Server. O backplane também funciona sem Service Broker. Abra uma nova consulta clicando com o botão direito do mouse no banco de dados e selecionando **nova consulta**.

    ![Abrindo uma nova consulta](real-time-web-applications-with-signalr/_static/image24.png "Abrindo uma nova consulta")

    *Abrindo uma nova consulta*
7. Para verificar se Service Broker está habilitado, consulte a coluna **is\_Broker\_Enabled** na exibição do catálogo **Sys. databases** . Execute o script a seguir na janela de consulta aberta recentemente.

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample11.sql)]

    ![Consultando o status de Service Broker](real-time-web-applications-with-signalr/_static/image25.png "Consultando o status de Service Broker")

    *Consultando o status de Service Broker*
8. Se o valor da coluna **is\_broker\_habilitado** em seu banco de dados for &quot;0&quot;, use o comando a seguir para habilitá-lo. Substitua **&lt;seu&gt;de banco de dados** pelo nome que você definiu ao criar o banco de dados (por exemplo: signalr).

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample12.sql)]

    ![Habilitando Service Broker](real-time-web-applications-with-signalr/_static/image26.png "Habilitando o Service Broker")

    *Habilitando Service Broker*

    > [!NOTE]
    > Se essa consulta aparecer para deadlock, verifique se não há aplicativos conectados ao BD.

<a id="Ex2Task3"></a>
#### <a name="task-3--configuring-the-signalr-application"></a>Tarefa 3 – Configurando o aplicativo Signalr

Nesta tarefa, você configurará o **teste de especialista** para se conectar ao backplane de SQL Server. Primeiro, você adicionará o pacote NuGet do **signalr. SqlServer** e definirá a cadeia de conexão para o banco de dados do backplane.

1. Abra o **console do Gerenciador de pacotes** em **ferramentas** > **Gerenciador de pacotes NuGet**. Verifique se o projeto **GeekQuiz** está selecionado na lista suspensa **projeto padrão** . Digite o seguinte comando para instalar o pacote NuGet **Microsoft. AspNet. signalr. SqlServer** .

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample13.ps1)]
2. Repita a etapa anterior, mas desta vez para o projeto **GeekQuiz2**.
3. Para configurar o SQL Server backplane, abra o arquivo **Startup.cs** do projeto **GeekQuiz** e adicione o código a seguir ao método **Configure** . Substitua **&lt;seu&gt;de banco de dados** pelo nome do banco de dados usado ao criar o SQL Server backplane. Repita esta etapa para o projeto **GeekQuiz2** .

    (Trecho de código- *RealTimeSignalR-EX2-StartupConfiguration*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample14.cs)]
4. Agora que ambos os projetos estão configurados para usar o SQL Server backplane, pressione **F5** para executá-los simultaneamente.
5. Novamente, o **Visual Studio** iniciará duas instâncias de **teste de especialista** em diferentes portas. Fixe um dos navegadores à esquerda e o outro à direita da tela e faça logon com suas credenciais. Mantenha a página Trívia à esquerda e vá para a página de **estatísticas** no navegador certo.
6. Comece a responder a perguntas no navegador esquerdo. Desta vez, a página **estatísticas** é atualizada graças ao backplane. Alterne entre aplicativos (as**estatísticas** estão agora à esquerda e **Trívia** está à direita) e repita o teste para validar que ele está funcionando para ambas as instâncias. O backplane serve como um *cache compartilhado* de mensagens para cada servidor conectado, e cada servidor armazenará as mensagens em seu próprio cache local para distribuir aos clientes conectados.
7. Volte para o Visual Studio e pare a depuração.
8. O componente de backplane SQL Server gera automaticamente as tabelas necessárias no banco de dados especificado. No painel de **pesquisador de objetos do SQL Server** , abra o banco de dados criado para o backplane (por exemplo: signalr) e expanda suas tabelas. Você deve ver as seguintes tabelas:

    ![Tabelas geradas do backplane](real-time-web-applications-with-signalr/_static/image27.png)

    *Tabelas geradas do backplane*
9. Clique com o botão direito do mouse na tabela **signalr. messages\_0** e selecione **exibir dados**.

    ![Exibir tabela de mensagens do backplane do Signalr](real-time-web-applications-with-signalr/_static/image28.png)

    *Exibir tabela de mensagens do backplane do Signalr*
10. Você pode ver as diferentes mensagens enviadas ao **Hub** ao responder às perguntas trívias. O backplane distribui essas mensagens para qualquer instância conectada.

    ![Tabela de mensagens do backplane](real-time-web-applications-with-signalr/_static/image29.png)

    *Tabela de mensagens do backplane*

---

<a id="Summary"></a>
## <a name="summary"></a>Resumo

Neste laboratório prático, você aprendeu como adicionar o **signalr** ao seu aplicativo e enviar notificações do servidor para seus clientes conectados usando **hubs**. Além disso, você aprendeu como escalar horizontalmente seu aplicativo usando um componente do *backplane* quando seu aplicativo é implantado em várias instâncias do IIS.
