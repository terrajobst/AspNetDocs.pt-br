---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
title: Noções básicas sobre os recursos de depuração do ASP.NET AJAX | Microsoft Docs
author: scottcate
description: A capacidade de depurar o código é uma habilidade que todos os desenvolvedores devem ter em seu arsenal, independentemente da tecnologia que estão usando. Embora muitos desenvolvedores estejam...
ms.author: riande
ms.date: 03/28/2008
ms.assetid: 7f9380c6-19f7-4c82-a019-916ec6dffc9c
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
msc.type: authoredcontent
ms.openlocfilehash: 08ced380f3551407d757524dbc84b5feeeb5482b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78629503"
---
# <a name="understanding-aspnet-ajax-debugging-capabilities"></a>Noções básicas sobre os recursos de depuração do AJAX ASP.NET

por [Scott Cate](https://github.com/scottcate)

[Baixar PDF](https://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial06_Debugging_MS_Ajax_Applications_cs.pdf)

> A capacidade de depurar o código é uma habilidade que todos os desenvolvedores devem ter em seu arsenal, independentemente da tecnologia que estão usando. Embora muitos desenvolvedores estejam acostumados a usar o Visual Studio .NET ou o Web Developer Express para depurar aplicativos ASP.NET que C# usam o VB.net ou o código, alguns não sabem que também é extremamente útil para depurar o código do lado do cliente, como JavaScript. O mesmo tipo de técnicas usado para depurar aplicativos .NET também pode ser aplicado a aplicativos habilitados para AJAX e mais especificamente ASP.NET aplicativos AJAX.

## <a name="debugging-aspnet-ajax-applications"></a>Depurando aplicativos AJAX ASP.NET

Dan Wahlin

A capacidade de depurar o código é uma habilidade que todos os desenvolvedores devem ter em seu arsenal, independentemente da tecnologia que estão usando. Não é preciso dizer que entender as diferentes opções de depuração disponíveis pode economizar bastante tempo em um projeto e, talvez, até mesmo algumas dores de cabeça. Embora muitos desenvolvedores estejam acostumados a usar o Visual Studio .NET ou o Web Developer Express para depurar aplicativos ASP.NET que C# usam o VB.net ou o código, alguns não sabem que também é extremamente útil para depurar o código do lado do cliente, como JavaScript. O mesmo tipo de técnicas usado para depurar aplicativos .NET também pode ser aplicado a aplicativos habilitados para AJAX e mais especificamente ASP.NET aplicativos AJAX.

Neste artigo, você verá como o Visual Studio 2008 e várias outras ferramentas podem ser usados para depurar aplicativos ASP.NET AJAX para localizar rapidamente bugs e outros problemas. Essa discussão incluirá informações sobre como habilitar o Internet Explorer 6 ou superior para depuração, usando o Visual Studio 2008 e o Gerenciador de scripts para percorrer o código, bem como usar outras ferramentas gratuitas como o auxiliar de desenvolvimento para a Web. Você também aprenderá como depurar aplicativos ASP.NET AJAX no Firefox usando uma extensão chamada Firebug, que permite percorrer o código JavaScript diretamente no navegador sem nenhuma outra ferramenta. Por fim, você será apresentado a classes na biblioteca do ASP.NET AJAX que podem ajudar com várias tarefas de depuração, como instruções de rastreamento e de declaração de código.

Antes de tentar depurar as páginas exibidas no Internet Explorer, há algumas etapas básicas que você precisará executar para habilitá-lo para depuração. Vamos dar uma olhada em alguns requisitos básicos de configuração que precisam ser executados para começar.

## <a name="configuring-internet-explorer-for-debugging"></a>Configurando o Internet Explorer para depuração

A maioria das pessoas não está interessada em ver problemas de JavaScript encontrados em um site exibido com o Internet Explorer. Na verdade, o usuário médio nem saberá o que fazer se eles viram uma mensagem de erro. Como resultado, as opções de depuração são desativadas por padrão no navegador. No entanto, é muito simples ativar a depuração e colocá-la para usar à medida que você desenvolve novos aplicativos AJAX.

Para habilitar a funcionalidade de depuração, vá para ferramentas opções da Internet no menu Internet Explorer e selecione a guia Avançado. Na seção de navegação, verifique se os seguintes itens estão desmarcados:

- Desabilitar depuração de script (Internet Explorer)
- Desabilitar depuração de script (outros)

Embora não seja necessário, se você estiver tentando depurar um aplicativo, provavelmente desejará que todos os erros de JavaScript na página fiquem visíveis e óbvios imediatamente. Você pode forçar a exibição de todos os erros com uma caixa de mensagem marcando a seleção "Exibir notificação sobre cada erro de script". Embora essa seja uma ótima opção para ativar enquanto você estiver desenvolvendo um aplicativo, pode rapidamente ficar irritante se você estiver usando outros sites, já que as chances de encontrar erros de JavaScript são muito boas.

A Figura 1 mostra o que a caixa de diálogo avançada do Internet Explorer deve ter após ter sido configurada corretamente para depuração.

[![configuração do Internet Explorer para depuração.](understanding-asp-net-ajax-debugging-capabilities/_static/image2.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image1.png)

**Figura 1**: Configurando o Internet Explorer para depuração.  ([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-debugging-capabilities/_static/image3.png))

Depois que a depuração tiver sido ativada, você verá um novo item de menu exibido no menu Exibir chamado depurador de script. Ele tem duas opções disponíveis, incluindo abrir e interromper na próxima instrução. Quando abrir estiver selecionado, você será solicitado a depurar a página no Visual Studio 2008 (Observe que o Visual Web Developer Express também pode ser usado para depuração). Se o Visual Studio .NET estiver em execução no momento, você poderá optar por usar essa instância ou criar uma nova instância. Quando a instrução break na próxima for selecionada, você será solicitado a depurar a página quando o código JavaScript for executado. Se o código JavaScript for executado no evento onLoad da página, você poderá atualizar a página para disparar uma sessão de depuração. Se o código JavaScript for executado depois que um botão for clicado, o depurador será executado imediatamente depois que o botão for clicado.

> [!NOTE]
> Se você estiver executando o Windows Vista com o UAC (controle de acesso do usuário) habilitado e tiver o Visual Studio 2008 definido para ser executado como um administrador, o Visual Studio não será anexado ao processo quando você for solicitado a anexá-lo. Para contornar esse problema, inicie primeiro o Visual Studio e use essa instância para depurar.

Embora a próxima seção demonstre como depurar uma página ASP.NET AJAX diretamente do Visual Studio 2008, o uso da opção Internet Explorer Script Debugger é útil quando uma página já está aberta e você gostaria de inspecioná-la mais completamente.

## <a name="debugging-with-visual-studio-2008"></a>Depurando com o Visual Studio 2008

O Visual Studio 2008 fornece a funcionalidade de depuração que os desenvolvedores do mundo todo confiam diariamente para depurar aplicativos .NET. O depurador interno permite que você percorra código, exiba dados de objeto, veja variáveis específicas, monitore a pilha de chamadas mais muito mais. Além de depurar VB.NET ou C# código, o depurador também é útil para depurar aplicativos AJAX ASP.net e permitirá que você percorra o código JavaScript linha por linha. Os detalhes a seguir se concentram em técnicas que podem ser usadas para depurar arquivos de script do lado do cliente em vez de fornecer um descurso sobre o processo geral de depuração de aplicativos usando o Visual Studio 2008.

O processo de depuração de uma página no Visual Studio 2008 pode ser iniciado de várias maneiras diferentes. Primeiro, você pode usar a opção do depurador de script do Internet Explorer mencionada na seção anterior. Isso funciona bem quando uma página já está carregada no navegador e você gostaria de começar a depurá-la. Como alternativa, você pode clicar com o botão direito do mouse em uma página. aspx na Gerenciador de Soluções e selecionar definir como página inicial no menu. Se você estiver acostumado a depurar as páginas do ASP.NET, provavelmente já fez isso antes. Depois que F5 é pressionado, a página pode ser depurada. No entanto, embora geralmente seja possível definir um ponto de interrupção em qualquer lugar C# que você queira no VB.net ou no código, isso nem sempre acontece com o JavaScript, como você verá em seguida.

*Scripts incorporados versus externos*

O depurador do Visual Studio 2008 trata o JavaScript inserido em uma página diferente de arquivos JavaScript externos. Com os arquivos de script externo, você pode abrir o arquivo e definir um ponto de interrupção em qualquer linha escolhida. Os pontos de interrupção podem ser definidos clicando na área da bandeja cinza à esquerda da janela do editor de código. Quando o JavaScript é inserido diretamente em uma página usando a marca `<script>`, definir um ponto de interrupção clicando na área da bandeja cinza não é uma opção. As tentativas de definir um ponto de interrupção em uma linha de script inserido resultarão em um aviso afirmando "este não é um local válido para um ponto de interrupção".

Você pode contornar esse problema movendo o código para um arquivo External. js e fazendo referência a ele usando o atributo src da marca&gt; do script de &lt;:

[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample1.html)]

E se a movimentação do código em um arquivo externo não for uma opção ou exigir mais trabalho do que vale a pena? Embora você não possa definir um ponto de interrupção usando o editor, você pode adicionar a instrução do depurador diretamente no código em que você deseja iniciar a depuração. Você também pode usar a classe Sys. Debug disponível na biblioteca do ASP.NET AJAX para forçar a depuração a ser iniciada. Você aprenderá mais sobre a classe Sys. Debug posteriormente neste artigo.

Um exemplo de como usar a palavra-chave `debugger` é mostrado na Listagem 1. Este exemplo força o depurador a quebrar imediatamente antes que uma chamada para uma função de atualização seja feita.

**Listagem 1. Usando a palavra-chave Debugger para forçar a interrupção do depurador do Visual Studio .NET.**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample2.js)]

Depois que a instrução do depurador for atingida, você será solicitado a depurar a página usando o Visual Studio .NET e poderá começar a percorrer o código. Ao fazer isso, você pode encontrar um problema com o acesso aos arquivos de script da biblioteca do ASP.NET AJAX usados na página, portanto, vamos dar uma olhada no uso do Visual Studio. Gerenciador de script da NET.

## <a name="using-visual-studio-net-windows-to-debug"></a>Usando o Windows do Visual Studio .NET para depurar

Depois que uma sessão de depuração é iniciada e você começa a percorrer o código usando a chave F11 padrão, você pode encontrar a caixa de diálogo de erro mostrada em consulte a Figura 2, a menos que todos os arquivos de script usados na página estejam abertos e disponíveis para depuração.

[![caixa de diálogo de erro mostrada quando não há código-fonte disponível para depuração.](understanding-asp-net-ajax-debugging-capabilities/_static/image5.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image4.png)

**Figura 2**: caixa de diálogo de erro mostrada quando não há código-fonte disponível para depuração.  ([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-debugging-capabilities/_static/image6.png))

Essa caixa de diálogo é mostrada porque o Visual Studio .NET não tem certeza de como chegar ao código-fonte de alguns dos scripts referenciados pela página. Embora isso possa ser bastante frustrante a princípio, há uma correção simples. Depois de iniciar uma sessão de depuração e atingir um ponto de interrupção, vá para a janela depurar Gerenciador de script do Windows no menu do Visual Studio 2008 ou use a tecla Ctrl + Alt + N.

> [!NOTE]
> Se você não conseguir ver o menu do Gerenciador de script listado, vá para **ferramentas** > **Personalizar** > **comandos** no menu do Visual Studio .net. Localize a entrada de **depuração** na seção categorias e clique nela para mostrar todas as entradas de menu disponíveis. Na lista comandos, role para baixo até Gerenciador de script e arraste-o para cima até o menu Depurar Windows mencionado anteriormente. Isso fará com que a entrada de menu do script Explorer seja disponibilizada sempre que você executar o Visual Studio .NET.

O Gerenciador de script pode ser usado para exibir todos os scripts usados em uma página e abri-los no editor de código. Quando o Gerenciador de scripts estiver aberto, clique duas vezes na página. aspx que está sendo depurada para abri-la na janela do editor de códigos. Execute a mesma ação para todos os outros scripts mostrados no Gerenciador de scripts. Depois que todos os scripts estiverem abertos na janela de código, você poderá pressionar F11 (e usar as outras teclas de tecla de depuração) para percorrer o código. A Figura 3 mostra um exemplo do Gerenciador de scripts. Ele lista o arquivo atual que está sendo depurado (demo. aspx), bem como dois scripts personalizados e dois scripts injetados dinamicamente na página pelo ScriptManager do ASP.NET AJAX.

[![o Gerenciador de script fornece acesso fácil aos scripts usados em uma página.](understanding-asp-net-ajax-debugging-capabilities/_static/image8.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image7.png)

**Figura 3**. O Gerenciador de script fornece acesso fácil aos scripts usados em uma página.  ([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-debugging-capabilities/_static/image9.png))

Várias outras janelas também podem ser usadas para fornecer informações úteis conforme você percorre o código em uma página. Por exemplo, você pode usar a janela locais para ver os valores de variáveis diferentes usadas na página, a janela imediata para avaliar variáveis ou condições específicas e exibir a saída. Você também pode usar a janela saída para exibir instruções de rastreamento gravadas usando a função sys. Debug. Trace (que será abordada posteriormente neste artigo) ou a função Debug. writeln do Internet Explorer.

Ao percorrer o código usando o depurador, você pode passar o mouse sobre variáveis no código para exibir o valor atribuído. No entanto, o depurador de scripts ocasionalmente não mostrará nada à medida que você passar o mouse sobre uma determinada variável JavaScript. Para ver o valor, realce a instrução ou a variável que você está tentando ver na janela do editor de código e, em seguida, passe o mouse sobre ela. Embora essa técnica não funcione em todas as situações, muitas vezes você poderá ver o valor sem precisar examinar uma janela de depuração diferente, como a janela locais.

Um tutorial em vídeo que demonstra alguns dos recursos discutidos aqui pode ser exibido em [http://www.xmlforasp.net](http://www.xmlforasp.net).

## <a name="debugging-with-web-development-helper"></a>Depuração com o auxiliar de desenvolvimento da Web

Embora o Visual Studio 2008 (e o Visual Web Developer Express 2008) sejam ferramentas de depuração com capacidade, há opções adicionais que podem ser usadas também, que são mais leves. Uma das ferramentas mais recentes a serem lançadas é o auxiliar de desenvolvimento para a Web. O Nikhil Kothari da Microsoft (um dos principais arquitetos de ASP.NET AJAX na Microsoft) escreveu essa excelente ferramenta que pode executar muitas tarefas diferentes de depuração simples para exibir mensagens de solicitação e resposta HTTP. O auxiliar de desenvolvimento da Web pode ser baixado em [http://projects.nikhilk.net/Projects/WebDevHelper.aspx](http://projects.nikhilk.net/Projects/WebDevHelper.aspx).

O auxiliar de desenvolvimento da Web pode ser usado diretamente no Internet Explorer, o que torna conveniente usar. Ele é iniciado com a seleção de ferramentas auxiliar de desenvolvimento para a Web no menu do Internet Explorer. Isso abrirá a ferramenta na parte inferior do navegador, o que é bom, pois você não precisa sair do navegador para executar várias tarefas, como o log de mensagens de solicitação e resposta HTTP. A Figura 4 mostra a aparência do auxiliar de desenvolvimento da Web em ação.

[Auxiliar de desenvolvimento para a Web ![](understanding-asp-net-ajax-debugging-capabilities/_static/image11.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image10.png)

**Figura 4**: auxiliar de desenvolvimento da Web ([clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-debugging-capabilities/_static/image12.png))

O auxiliar de desenvolvimento da Web não é uma ferramenta que você usará para percorrer o código linha por linha como com o Visual Studio 2008. No entanto, ele pode ser usado para exibir a saída de rastreamento, avaliar facilmente variáveis em um script ou explorar dados dentro de um objeto JSON. Também é muito útil para exibir dados que são passados para e de uma página ASP.NET AJAX e um servidor.

Quando o auxiliar de desenvolvimento da Web estiver aberto no Internet Explorer, a depuração de script deverá ser habilitada selecionando script Habilitar depuração de script no menu do auxiliar de desenvolvimento da Web, como mostrado anteriormente na Figura 4. Isso permite que a ferramenta intercepte erros que ocorrem à medida que uma página é executada. Ele também permite acesso fácil a mensagens de rastreamento que são enviadas na página. Para exibir informações de rastreamento ou executar comandos de script para testar diferentes funções dentro de uma página, selecione script show script console no menu auxiliar de desenvolvimento da Web. Isso fornece acesso a uma janela de comando e uma janela imediata simples.

*Exibindo mensagens de rastreamento e dados de objeto JSON*

A janela imediata pode ser usada para executar comandos de script ou até mesmo carregar ou salvar scripts que são usados para testar funções diferentes em uma página. A janela de comando exibe mensagens de rastreamento ou de depuração gravadas pela página que está sendo exibida. A listagem 2 mostra como gravar uma mensagem de rastreamento usando a função Debug. writeln do Internet Explorer.

**Listagem 2. Gravando uma mensagem de rastreamento no lado do cliente usando a classe Debug.**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample3.js)]

Se a propriedade LastName contiver um valor de Doe, o auxiliar de desenvolvimento da Web exibirá a mensagem "nome da pessoa: Doe" na janela de comando do console de script (supondo que a depuração tenha sido habilitada). O auxiliar de desenvolvimento da Web também adiciona um objeto debugService de nível superior em páginas que podem ser usadas para gravar informações de rastreamento ou exibir o conteúdo de objetos JSON. A listagem 3 mostra um exemplo de como usar a função de rastreamento da classe debugService.

**Listagem 3. Usando a classe debugService do auxiliar de desenvolvimento da Web para gravar uma mensagem de rastreamento.**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample4.js)]

Um bom recurso da classe debugService é que ela funcionará mesmo se a depuração não estiver habilitada no Internet Explorer, facilitando sempre o acesso aos dados de rastreamento quando o auxiliar de desenvolvimento da Web estiver em execução. Quando a ferramenta não estiver sendo usada para depurar uma página, as instruções de rastreamento serão ignoradas, pois a chamada para Window. debugService retornará false.

A classe debugService também permite que os dados do objeto JSON sejam exibidos usando a janela Inspetor do auxiliar de desenvolvimento da Web. A listagem 4 cria um objeto JSON simples que contém dados de pessoa. Depois que o objeto é criado, uma chamada é feita à função de inspeção da classe debugService para permitir que o objeto JSON seja inspecionado visualmente.

**Listagem 4. Usando a função debugService. Inspecione para exibir dados de objeto JSON.**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample5.js)]

Chamar a função GetPerson () na página ou por meio da janela Immediate fará com que a janela de diálogo Inspetor de objetos apareça como mostrado na Figura 5. As propriedades dentro do objeto podem ser alteradas dinamicamente, realçando-as, alterando o valor mostrado na caixa de texto valor e, em seguida, clicando no link atualizar. O uso do Inspetor de objetos torna simples a exibição de dados de objetos JSON e o experimento com a aplicação de valores diferentes às propriedades.

*Erros de depuração*

Além de permitir que os dados de rastreamento e os objetos JSON sejam exibidos, o auxiliar de desenvolvimento da Web também pode auxiliar na depuração de erros em uma página. Se for encontrado um erro, você será solicitado a continuar na próxima linha de código ou depurar o script (consulte a Figura 6). A janela da caixa de diálogo de erro de script mostra a pilha de chamadas completa, bem como os números de linha, para que você possa identificar facilmente onde os problemas estão dentro de um script.

[![usando a janela Inspetor de objetos para exibir um objeto JSON.](understanding-asp-net-ajax-debugging-capabilities/_static/image14.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image13.png)

**Figura 5**: usando a janela Inspetor de objetos para exibir um objeto JSON.  ([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-debugging-capabilities/_static/image15.png))

A seleção da opção de depuração permite executar instruções de script diretamente na janela imediata do auxiliar de desenvolvimento da Web para exibir o valor das variáveis, gravar objetos JSON, além de mais. Se a mesma ação que disparou o erro for executada novamente e o Visual Studio 2008 estiver disponível no computador, você será solicitado a iniciar uma sessão de depuração para que possa percorrer o código linha por linha, conforme discutido na seção anterior.

[Caixa de diálogo de erro de script do auxiliar de desenvolvimento Web ![](understanding-asp-net-ajax-debugging-capabilities/_static/image17.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image16.png)

**Figura 6**: caixa de diálogo de erro de script do auxiliar de desenvolvimento da Web ([clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-debugging-capabilities/_static/image18.png))

*Inspecionando mensagens de solicitação e resposta*

Durante a depuração de páginas do ASP.NET AJAX, geralmente é útil ver mensagens de solicitação e resposta enviadas entre uma página e um servidor. A exibição do conteúdo em mensagens permite que você veja se os dados adequados estão sendo passados, bem como o tamanho das mensagens. O auxiliar de desenvolvimento para a Web fornece um excelente recurso de agente de mensagens HTTP que facilita a exibição de dados como texto bruto ou em um formato mais legível.

Para exibir mensagens de solicitação e resposta do ASP.NET AJAX, o agente HTTP deve ser habilitado selecionando HTTP habilitar log HTTP no menu do auxiliar de desenvolvimento da Web. Uma vez habilitado, todas as mensagens enviadas da página atual podem ser exibidas no Visualizador de log HTTP, que pode ser acessado selecionando HTTP Mostrar logs HTTP.

Embora a exibição do texto bruto enviado em cada mensagem de solicitação/resposta seja certamente útil (e uma opção no auxiliar de desenvolvimento da Web), geralmente é mais fácil exibir dados de mensagem em um formato mais gráfico. Depois que o log HTTP tiver sido habilitado e as mensagens tiverem sido registradas, os dados da mensagem poderão ser exibidos clicando duas vezes na mensagem no Visualizador de log HTTP. Isso permite que você exiba todos os cabeçalhos associados a uma mensagem, bem como o conteúdo real da mensagem. A Figura 7 mostra um exemplo de mensagem de solicitação e mensagem de resposta exibida na janela do Visualizador de log HTTP.

[![usando o Visualizador de log HTTP para exibir dados de mensagem de solicitação e resposta.](understanding-asp-net-ajax-debugging-capabilities/_static/image20.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image19.png)

**Figura 7**: usando o Visualizador de log http para exibir dados de mensagem de solicitação e resposta.  ([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-debugging-capabilities/_static/image21.png))

O Visualizador de log HTTP analisa automaticamente objetos JSON e os exibe usando um modo de exibição de árvore, tornando rápido e fácil exibir os dados de Propriedade do objeto. Quando um UpdatePanel está sendo usado em uma página do ASP.NET AJAX, o visualizador divide cada parte da mensagem em partes individuais, conforme mostrado na Figura 8. Esse é um ótimo recurso que torna muito mais fácil ver e entender o que está na mensagem em comparação com a exibição dos dados brutos da mensagem.

[![uma mensagem de resposta UpdatePanel exibida usando o Visualizador de log HTTP.](understanding-asp-net-ajax-debugging-capabilities/_static/image23.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image22.png)

**Figura 8**: uma mensagem de resposta UpdatePanel exibida usando o Visualizador de log http.  ([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-debugging-capabilities/_static/image24.png))

Há várias outras ferramentas que podem ser usadas para exibir mensagens de solicitação e resposta, além do auxiliar de desenvolvimento da Web. Outra boa opção é o Fiddler, que está disponível gratuitamente em [http://www.fiddlertool.com](http://www.fiddlertool.com). Embora o Fiddler não seja discutido aqui, também é uma boa opção quando você precisa inspecionar detalhadamente os cabeçalhos e os dados da mensagem.

## <a name="debugging-with-firefox-and-firebug"></a>Depuração com Firefox e Firebug

Embora o Internet Explorer ainda seja o navegador mais amplamente usado, outros navegadores, como o Firefox, se tornaram bastante populares e estão sendo usados cada vez mais. Como resultado, você desejará exibir e depurar suas páginas do ASP.NET AJAX no Firefox, bem como no Internet Explorer, para garantir que seus aplicativos funcionem corretamente. Embora o Firefox não possa se associar diretamente ao Visual Studio 2008 para depuração, ele tem uma extensão chamada Firebug que pode ser usada para depurar páginas. O Firebug pode ser baixado gratuitamente acessando [http://www.getfirebug.com](http://www.getfirebug.com).

O Firebug fornece um ambiente de depuração repleto de recursos que pode ser usado para percorrer o código linha por linha, acessar todos os scripts usados em uma página, exibir estruturas DOM, exibir estilos CSS e até mesmo controlar eventos que ocorrem em uma página. Uma vez instalado, o Firebug pode ser acessado selecionando Ferramentas Firebug abrir o Firebug no menu Firefox. Como o auxiliar de desenvolvimento para a Web, o Firebug é usado diretamente no navegador, embora também possa ser usado como um aplicativo autônomo.

Depois que o Firebug estiver em execução, os pontos de interrupção poderão ser definidos em qualquer linha de um arquivo JavaScript, independentemente de o script ser inserido em uma página ou não. Para definir um ponto de interrupção, primeiro carregue a página apropriada que você gostaria de depurar no Firefox. Depois que a página for carregada, selecione o script a ser depurado na lista suspensa scripts do Firebug. Todos os scripts usados pela página serão mostrados. Um ponto de interrupção é definido clicando na área da bandeja cinza do Firebug na linha em que o ponto de interrupção deve ir, como você faria no Visual Studio 2008.

Depois que um ponto de interrupção tiver sido definido no Firebug, você poderá executar a ação necessária para executar o script que precisa ser depurado, como clicar em um botão ou atualizar o navegador para disparar o evento onLoad. A execução será interrompida automaticamente na linha que contém o ponto de interrupção. A Figura 9 mostra um exemplo de um ponto de interrupção que foi disparado no Firebug.

[![tratamento de pontos de interrupção no Firebug.](understanding-asp-net-ajax-debugging-capabilities/_static/image26.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image25.png)

**Figura 9**: tratamento de pontos de interrupção no Firebug.  ([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-debugging-capabilities/_static/image27.png))

Depois que um ponto de interrupção é atingido, você pode entrar, percorrer ou sair do código usando os botões de seta. Conforme você percorre o código, as variáveis de script são exibidas na parte direita do depurador, permitindo que você veja os valores e faça uma busca detalhada nos objetos. O Firebug também inclui uma lista suspensa pilha de chamadas para exibir as etapas de execução do script que levaram à linha atual sendo depurada.

O Firebug também inclui uma janela de console que pode ser usada para testar diferentes instruções de script, avaliar variáveis e exibir a saída de rastreamento. Ele é acessado clicando na guia Console na parte superior da janela do Firebug. A página que está sendo depurada também pode ser "inspecionada" para ver sua estrutura e conteúdo DOM clicando na guia inspecionar. À medida que você passa o mouse sobre os diferentes elementos DOM mostrados na janela Inspetor, a parte apropriada da página será realçada, facilitando a visualização de onde o elemento é usado na página. Os valores de atributo associados a um determinado elemento podem ser alterados "dinâmicos" para experimentar a aplicação de diferentes larguras, estilos, etc. a um elemento. Esse é um recurso interessante que evita que você precise alternar constantemente entre o editor de código-fonte e o navegador Firefox para exibir como as alterações simples afetam uma página.

A Figura 10 mostra um exemplo de como usar o Inspetor DOM para localizar uma caixa de texto chamada txtCountry na página. O Inspetor do Firebug também pode ser usado para exibir estilos CSS usados em uma página, bem como eventos que ocorrem como rastreamento de movimentos do mouse, cliques de botão e mais.

[![usando o Inspetor DOM do Firebug.](understanding-asp-net-ajax-debugging-capabilities/_static/image29.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image28.png)

**Figura 10**: usando o Inspetor dom do Firebug.  ([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-debugging-capabilities/_static/image30.png))

O Firebug fornece uma maneira leve de depurar rapidamente uma página diretamente no Firefox, bem como uma excelente ferramenta para inspecionar elementos diferentes dentro da página.

## <a name="debugging-support-in-aspnet-ajax"></a>Suporte à depuração no AJAX ASP.NET

A biblioteca do ASP.NET AJAX inclui muitas classes diferentes que podem ser usadas para simplificar o processo de adição de recursos AJAX em uma página da Web. Você pode usar essas classes para localizar elementos em uma página e manipulá-los, adicionar novos controles, chamar serviços Web e até mesmo manipular eventos. A biblioteca do ASP.NET AJAX também contém classes que podem ser usadas para aprimorar o processo de depuração de páginas. Nesta seção, você será apresentado à classe Sys. Debug e verá como ela pode ser usada em aplicativos.

*Usando a classe Sys. Debug*

A classe Sys. debug (uma classe JavaScript localizada no namespace sys) pode ser usada para executar várias funções diferentes, incluindo a criação de saída de rastreamento, a execução de declarações de código e a força do código a falhar para que ele possa ser depurado. Ele é usado extensivamente nos arquivos de depuração da biblioteca do ASP.NET AJAX (instalado em C:\Arquivos de Programas\microsoft ASP. NET\ASP.NET 2,0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0 por padrão) para executar testes condicionais ( chamadas de asserções) que garantem que os parâmetros sejam passados adequadamente para funções, que os objetos contenham os dados esperados e escrevam instruções de rastreamento.

A classe Sys. Debug expõe várias funções diferentes que podem ser usadas para lidar com rastreamento, asserções de código ou falhas, conforme mostrado na tabela 1.

**Tabela 1. Funções de classe Sys. Debug.**

| **Nome da Função** | **Descrição** |
| --- | --- |
| assert(condition, message, displayCaller) | Declara que o parâmetro Condition é true. Se a condição que está sendo testada for falsa, uma caixa de mensagem será usada para exibir o valor do parâmetro de mensagem. Se o parâmetro displayCaller for true, o método também exibirá informações sobre o chamador. |
| clearTrace() | Apaga a saída de instruções das operações de rastreamento. |
| falha (mensagem) | Faz com que o programa pare a execução e quebre no depurador. O parâmetro Message pode ser usado para fornecer um motivo para a falha. |
| rastreamento (mensagem) | Grava o parâmetro de mensagem na saída de rastreamento. |
| traceDump (objeto, nome) | Gera os dados de um objeto em um formato legível. O parâmetro Name pode ser usado para fornecer um rótulo para o despejo de rastreamento. Todos os subobjetos dentro do objeto que está sendo despejado serão gravados por padrão. |

O rastreamento no lado do cliente pode ser usado quase da mesma forma que a funcionalidade de rastreamento disponível no ASP.NET. Ele permite que mensagens diferentes sejam facilmente vistas sem interromper o fluxo do aplicativo. A listagem 5 mostra um exemplo de como usar a função sys. Debug. Trace para gravar no log de rastreamento. Essa função simplesmente usa a mensagem que deve ser gravada como um parâmetro.

**Listagem 5. Usando a função sys. Debug. Trace.**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample6.js)]

Se você executar o código mostrado na listagem 5, não verá nenhuma saída de rastreamento na página. A única maneira de vê-lo é usar uma janela de console disponível no Visual Studio .NET, no auxiliar de desenvolvimento da Web ou no Firebug. Se você quiser ver a saída de rastreamento na página, precisará adicionar uma marca TextArea e atribuir a ela uma ID de TraceConsole, conforme mostrado a seguir:

[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample7.html)]

Qualquer instrução sys. Debug. Trace na página será gravada na TextArea TraceConsole.

Nos casos em que você deseja ver os dados contidos em um objeto JSON, você pode usar a função traceDump da classe Sys. Debug. Essa função usa dois parâmetros, incluindo o objeto que deve ser despejado no console de rastreamento e um nome que pode ser usado para identificar o objeto na saída de rastreamento. A listagem 6 mostra um exemplo de como usar a função traceDump.

**Listagem 6. Usando a função sys. Debug. traceDump.**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample8.js)]

A Figura 11 mostra a saída da chamada da função sys. Debug. traceDump. Observe que, além de escrever os dados do objeto Person, ele também grava os dados do subobjeto address.

Além do rastreamento, a classe Sys. debug também pode ser usada para executar declarações de código. As asserções são usadas para testar se as condições específicas são atendidas enquanto um aplicativo está em execução. A versão de depuração dos scripts da biblioteca do ASP.NET AJAX contém várias instruções Assert para testar uma variedade de condições.

A listagem 7 mostra um exemplo de como usar a função sys. Debug. Assert para testar uma condição. O código testa se o objeto de endereço é nulo antes de atualizar um objeto Person.

[![saída da função sys. Debug. traceDump.](understanding-asp-net-ajax-debugging-capabilities/_static/image32.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image31.png)

**Figura 11**: saída da função sys. Debug. traceDump.  ([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-debugging-capabilities/_static/image33.png))

**Listagem 7. Usando a função Debug. Assert.**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample9.js)]

Três parâmetros são passados, incluindo a condição a ser avaliada, a mensagem a ser exibida se a declaração retornar false e se as informações sobre o chamador devem ou não ser exibidas. Nos casos em que uma asserção falhar, a mensagem será exibida, bem como informações do chamador se o terceiro parâmetro for verdadeiro. A Figura 12 mostra um exemplo da caixa de diálogo de falha que aparece se a declaração mostrada na Listagem 7 falhar.

A função final a ser coberta é sys. Debug. Fail. Quando você deseja forçar o código a falhar em uma linha específica em um script, você pode adicionar uma chamada sys. Debug. Fail em vez da instrução do depurador normalmente usada em aplicativos JavaScript. A função sys. Debug. Fail aceita um único parâmetro de cadeia de caracteres que representa o motivo da falha, conforme mostrado a seguir:

[!code-css[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample10.css)]

[![uma mensagem de falha sys. Debug. Assert.](understanding-asp-net-ajax-debugging-capabilities/_static/image35.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image34.png)

**Figura 12**: uma mensagem de falha sys. Debug. Assert.  ([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-debugging-capabilities/_static/image36.png))

Quando uma instrução sys. Debug. Fail for encontrada durante a execução de um script, o valor do parâmetro Message será exibido no console de um aplicativo de depuração, como o Visual Studio 2008, e será solicitado que você depure o aplicativo. Um caso em que isso pode ser bastante útil é quando você não pode definir um ponto de interrupção com o Visual Studio 2008 em um script embutido, mas quer que o código pare em uma linha específica para que você possa inspecionar o valor de variáveis.

*Noções básicas sobre a propriedade ScriptMode do controle ScriptManager*

A biblioteca do ASP.NET AJAX inclui versões de script de depuração e versão instaladas em C:\Program Files\Microsoft ASP. NET\ASP.NET 2,0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0 por padrão. Os scripts de depuração são formatados, são fáceis de ler e têm várias chamadas sys. Debug. Assert espalhadas ao longo deles, enquanto os scripts de versão têm espaços em branco eliminados e usam a classe Sys. Debug com moderação para minimizar seu tamanho geral.

O controle ScriptManager adicionado às páginas do ASP.NET AJAX lê o atributo debug do elemento Compilation em Web. config para determinar quais versões dos scripts de biblioteca carregar. No entanto, você pode controlar se os scripts de depuração ou versão são carregados (scripts de biblioteca ou seus próprios scripts personalizados) alterando a propriedade ScriptMode. O ScriptMode aceita uma enumeração de ScriptMode cujos membros incluem auto, Debug, release e Inherit.

O ScriptMode assume como padrão um valor de auto, o que significa que o ScriptManager verificará o atributo de depuração em Web. config. Quando Debug for false, o ScriptManager carregará a versão de lançamento dos scripts da biblioteca do ASP.NET AJAX. Quando Debug for true, a versão de depuração dos scripts será carregada. Alterar a propriedade ScriptMode para liberar ou depurar forçará o ScriptManager a carregar os scripts apropriados, independentemente do valor que o atributo debug tem em Web. config. A listagem 8 mostra um exemplo de como usar o controle ScriptManager para carregar scripts de depuração da biblioteca do ASP.NET AJAX.

**Listagem 8. Carregando scripts de depuração usando o ScriptManager**.

[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample11.aspx)]

Você também pode carregar diferentes versões (depuração ou versão) de seus próprios scripts personalizados usando a propriedade scripts do ScriptManager junto com o componente ScriptReference, conforme mostrado na listagem 9.

**Listagem 9. Carregando scripts personalizados usando o ScriptManager.**

[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample12.aspx)]

> [!NOTE]
> Se você estiver carregando scripts personalizados usando o componente ScriptReference, deverá notificar o ScriptManager quando o carregamento do script for concluído adicionando o seguinte código na parte inferior do script:

[!code-csharp[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample13.cs)]

O código mostrado na listagem 9 informa ao ScriptManager para procurar uma versão de depuração do script Person, de modo que ele procurará automaticamente por Person. Debug. js em vez de Person. js. Se o arquivo Person. Debug. js não for encontrado, um erro será gerado.

Nos casos em que você deseja que uma versão de depuração ou de lançamento de um script personalizado seja carregada com base no valor da propriedade ScriptMode definida no controle ScriptManager, você pode definir a propriedade ScriptMode do controle ScriptReference para herdar. Isso fará com que a versão apropriada do script personalizado seja carregada com base na propriedade ScriptMode do ScriptManager, conforme mostrado na listagem 10. Como a propriedade ScriptMode do controle ScriptManager está definida como Debug, o script Person. Debug. js será carregado e usado na página.

**Listagem 10. Herdar o ScriptMode do ScriptManager para scripts personalizados.**

[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample14.aspx)]

Usando a propriedade ScriptMode adequadamente, você pode depurar aplicativos com mais facilidade e simplificar o processo geral. Os scripts de versão da biblioteca do ASP.NET AJAX são difíceis de percorrer e ler, uma vez que a formatação do código foi removida enquanto os scripts de depuração são formatados especificamente para fins de depuração.

## <a name="conclusion"></a>Conclusão

A tecnologia ASP.NET AJAX da Microsoft fornece uma base sólida para a criação de aplicativos habilitados para AJAX que podem aprimorar a experiência geral do usuário final. No entanto, assim como acontece com qualquer tecnologia de programação, bugs e outros problemas de aplicativos certamente ocorrerão. Conhecer as diferentes opções de depuração disponíveis pode economizar muito tempo e resultar em um produto mais estável.

Neste artigo, você foi apresentado a várias técnicas diferentes para a depuração de páginas do ASP.NET AJAX, incluindo o Internet Explorer com o Visual Studio 2008, o auxiliar de desenvolvimento da Web e o Firebug. Essas ferramentas podem simplificar o processo de depuração geral, já que você pode acessar dados variáveis, percorrer as instruções linha de código por linha e exibir rastreamento. Além das diferentes ferramentas de depuração discutidas, você também viu como a classe Sys. Debug da biblioteca do ASP.NET AJAX pode ser usada em um aplicativo e como a classe ScriptManager pode ser usada para carregar versões de depuração ou de lançamento de scripts.

## <a name="bio"></a>Biografia

Dan Wahlin (Microsoft Most Valuable Professional para ASP.NET e XML Web Services) é um instrutor de desenvolvimento .NET e consultor de arquitetura em www.interfacett.com (treinamento técnico de interface[)](http://www.interfacett.com). Dan fundou o site do XML for ASP.NET Developers ([www.XMLforASP.net](http://www.XMLforASP.NET)), está na central do palestrante da INETA e fala em várias conferências. Dan My-autod Professional Windows DNA (Wrox), ASP.NET: Tips, tutoriais e código (SAMS), ASP.NET 1,1 insider Solutions, Professional ASP.NET 2,0 AJAX (Wrox), ASP.NET 2,0 de hacks MVP e XML autod for ASP.NET Developers (SAMS). Quando ele não está escrevendo código, artigos ou livros, Dan gosta de escrever e gravar música e jogar golfe e basquete com sua esposa e crianças.

Scott Cate tem trabalhado com tecnologias Web da Microsoft desde 1997 e é presidente da myKB.com ([www.myKB.com](http://www.myKB.com)), onde é especialista em escrever aplicativos baseados em ASP.net voltados para as soluções de software da base de dados de conhecimento. Scott pode ser contatado por email em [scott.cate@myKB.com](mailto:scott.cate@myKB.com) ou em seu blog em [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Anterior](understanding-asp-net-ajax-web-services.md)
