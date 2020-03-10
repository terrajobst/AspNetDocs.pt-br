---
uid: web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-cs
title: Páginas mestras e ASP.NET AJAXC#() | Microsoft Docs
author: rick-anderson
description: Discute opções para usar o ASP.NET AJAX e páginas mestras. Analisa o uso da classe ScriptManagerProxy; discute como os vários arquivos JS são carregados, depende de...
ms.author: riande
ms.date: 07/11/2008
ms.assetid: 0c55eb66-ba44-4d49-98e8-5c87fd9b1111
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-cs
msc.type: authoredcontent
ms.openlocfilehash: 8cd1d57b4d2aa01654da53ab2b1cc01f71ad8a87
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78629160"
---
# <a name="master-pages-and-aspnet-ajax-c"></a>Páginas mestras e AJAX ASP.NET (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_08_CS.zip) ou [baixar PDF](https://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_08_CS.pdf)

> Discute opções para usar o ASP.NET AJAX e páginas mestras. Analisa o uso da classe ScriptManagerProxy; discute como os vários arquivos JS são carregados dependendo se o ScriptManager é usado na página mestra ou na página de conteúdo.

## <a name="introduction"></a>Introdução

Nos últimos anos, cada vez mais desenvolvedores criaram aplicativos Web habilitados para [Ajax](http://en.wikipedia.org/wiki/Ajax_(programming)). Um site habilitado para AJAX usa uma série de tecnologias da Web relacionadas para oferecer uma experiência de usuário mais responsiva. Criar aplicativos ASP.NET habilitados para AJAX é surpreendentemente fácil graças à estrutura do [ASP.NET AJAX](../../../../ajax/index.md)da Microsoft. O ASP.NET AJAX é integrado ao ASP.NET 3,5 e ao Visual Studio 2008; Ele também está disponível como um download separado para aplicativos ASP.NET 2,0.

Ao criar páginas da Web habilitadas para AJAX com a estrutura do ASP.NET AJAX, você deve adicionar precisamente um [controle do ScriptManager](https://msdn.microsoft.com/library/bb398863.aspx) a cada página que usa a estrutura. Como o nome indica, o ScriptManager gerencia o script do lado do cliente usado em páginas da Web habilitadas para AJAX. No mínimo, o ScriptManager emite HTML que instrui o navegador a baixar os arquivos JavaScript que emitem a biblioteca de cliente do ASP.NET AJAX. Ele também pode ser usado para registrar arquivos JavaScript personalizados, serviços Web habilitados para script e funcionalidade personalizada do serviço de aplicativo.

Se o seu site usar páginas mestras (como deveria), você não precisará necessariamente adicionar um controle ScriptManager a cada página de conteúdo individual; em vez disso, você pode adicionar um controle ScriptManager à página mestra. Este tutorial mostra como adicionar o controle ScriptManager à página mestra. Ele também analisa como usar o controle ScriptManagerProxy para registrar scripts personalizados e serviços de script em uma página de conteúdo específica.

> [!NOTE]
> Este tutorial não explora o design ou a criação de aplicativos Web habilitados para AJAX com a estrutura AJAX do ASP.NET. Para obter mais informações sobre como usar o AJAX, consulte os vídeos e [tutoriais](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md)do [ASP.NET AJAX](../../../videos/aspnet-ajax/index.md) , bem como os recursos listados na seção leitura adicional no final deste tutorial.

## <a name="examining-the-markup-emitted-by-the-scriptmanager-control"></a>Examinando a marcação emitida pelo controle ScriptManager

O controle ScriptManager emite marcação que instrui o navegador a baixar os arquivos JavaScript que emitem a biblioteca de cliente do ASP.NET AJAX. Ele também adiciona um pouco de JavaScript embutido à página que inicializa essa biblioteca. A marcação a seguir mostra o conteúdo que é adicionado à saída renderizada de uma página que inclui um controle ScriptManager:

[!code-html[Main](master-pages-and-asp-net-ajax-cs/samples/sample1.html)]

As marcas de `<script src="url"></script>` instruem o navegador a baixar e executar o arquivo JavaScript na *URL*. O ScriptManager emite três marcas; um faz referência ao arquivo `WebResource.axd`, enquanto os outros dois fazem referência ao arquivo `ScriptResource.axd`. Esses arquivos não existem de fato como arquivos em seu site. Em vez disso, quando uma solicitação de qualquer um desses arquivos chega ao servidor Web, o mecanismo de ASP.NET examina a QueryString e retorna o conteúdo JavaScript apropriado. O script fornecido por esses três arquivos externos JavaScript constitui a biblioteca de cliente do ASP.NET AJAX Framework. As outras marcas de `<script>` emitidas pelo ScriptManager incluem script embutido que inicializa essa biblioteca.

As referências de script externo e o script embutido emitido pelo ScriptManager são essenciais para uma página que usa a estrutura ASP.NET AJAX, mas não é necessária para páginas que não usam a estrutura. Portanto, você pode fazer com que seja ideal apenas adicionar um ScriptManager a essas páginas que usam a estrutura do ASP.NET AJAX. E isso é suficiente, mas se você tiver muitas páginas que usam a estrutura, acabará adicionando o controle ScriptManager a todas as páginas, uma tarefa repetitiva, para dizer o mínimo. Como alternativa, você pode adicionar um ScriptManager à página mestra, que injeta esse script necessário em todas as páginas de conteúdo. Com essa abordagem, você não precisa se lembrar de adicionar um ScriptManager a uma nova página que usa a estrutura do ASP.NET AJAX porque ela já está incluída pela página mestra. A etapa 1 percorre a adição de um ScriptManager à página mestra.

> [!NOTE]
> Se você planeja incluir a funcionalidade do AJAX na interface do usuário da sua página mestra, não tem nenhuma opção no que se deve incluir o ScriptManager na página mestra.

Uma desvantagem de adicionar o ScriptManager à página mestra é que o script acima é emitido em *cada* página, independentemente de sua necessidade. Isso resulta claramente na largura de banda desperdiçada para as páginas que têm o ScriptManager incluído (por meio da página mestra), mas que não usam nenhum recurso da estrutura do ASP.NET AJAX. Mas a quantidade de largura de banda é desperdiçada?

- O conteúdo real emitido pelo ScriptManager (mostrado acima) totaliza um pouco sobre 1 KB.
- Os três arquivos de script externo referenciados pelo elemento `<script>`, no entanto, compõem aproximadamente 450KB de dados descompactados; em um site que usa compactação Gzip, essa largura de banda total pode ser reduzida perto de 100 KB. No entanto, esses arquivos de script são armazenados em cache pelo navegador por um ano, o que significa que eles só precisam ser baixados uma vez e podem ser reutilizados em outras páginas no site.

Na melhor das hipóteses, quando os arquivos de script são armazenados em cache, o custo total é de 1 KB, o que é insignificante. Na pior das hipóteses, no entanto, quando os arquivos de script ainda não foram baixados e o servidor Web não está usando nenhuma forma de compactação, a ocorrência da largura de banda está em volta de 450KB, que pode adicionar em qualquer lugar de um segundo ou dois em uma conexão de banda larga para até um minuto para  usuários em modems de conexão discada. A boa notícia é que, como os arquivos de script externo são armazenados em cache pelo navegador, esse pior cenário ocorre com pouca frequência.

> [!NOTE]
> Se você ainda sentir desagradáveis colocando o controle do ScriptManager na página mestra, considere o formulário da Web (a marcação `<form runat="server">` na página mestra). Cada página ASP.NET que usa o modelo de postback deve incluir precisamente um Web Form. A adição de um formulário da Web adiciona conteúdo adicional: vários campos de formulário ocultos, a `<form>` marca em si e, se necessário, uma função JavaScript para iniciar um postback do script. Essa marcação é desnecessária para páginas que não são postadas. Essa marcação estranha pode ser eliminada removendo o formulário da Web da página mestra e adicionando-o manualmente a cada página de conteúdo que precisa de um. No entanto, os benefícios de fazer com que o formulário da Web na página mestra supere as desvantagens de tê-lo adicionado desnecessariamente a determinadas páginas de conteúdo.

## <a name="step-1-adding-a-scriptmanager-control-to-the-master-page"></a>Etapa 1: adicionar um controle ScriptManager à página mestra

Cada página da Web que usa a estrutura AJAX do ASP.NET deve conter precisamente um controle ScriptManager. Devido a esse requisito, geralmente faz sentido posicionar um único controle ScriptManager na página mestra para que todas as páginas de conteúdo tenham o controle ScriptManager incluído automaticamente. Além disso, o ScriptManager deve vir antes de qualquer um dos controles de servidor ASP.NET AJAX, como os controles UpdatePanel e UpdateProgress. Portanto, é melhor colocar o ScriptManager antes de qualquer controle ContentPlaceHolder dentro do Web Form.

Abra a página `Site.master` mestra e adicione um controle ScriptManager à página no formulário da Web, mas antes do elemento `<div id="topContent">` (consulte a Figura 1). Se você estiver usando o Visual Web Developer 2008 ou o Visual Studio 2008, o controle do ScriptManager estará localizado na caixa de ferramentas na guia extensões AJAX. Se você estiver usando o Visual Studio 2005, precisará primeiro instalar a estrutura do ASP.NET AJAX e adicionar os controles à caixa de ferramentas. Visite o [wiki do ASP.NET AJAX](https://github.com/DevExpress/AjaxControlToolkit/wiki) para obter a estrutura do ASP.NET 2,0.

Depois de adicionar o ScriptManager à página, altere sua `ID` de `ScriptManager1` para `MyManager`.

[![adicionar o ScriptManager à página mestra](master-pages-and-asp-net-ajax-cs/_static/image2.png)](master-pages-and-asp-net-ajax-cs/_static/image1.png)

**Figura 01**: Adicionar o ScriptManager à página mestra ([clique para exibir a imagem em tamanho normal](master-pages-and-asp-net-ajax-cs/_static/image3.png))

## <a name="step-2-using-the-aspnet-ajax-framework-from-a-content-page"></a>Etapa 2: usando a estrutura do ASP.NET AJAX em uma página de conteúdo

Com o controle ScriptManager adicionado à página mestra, agora podemos adicionar a funcionalidade do ASP.NET AJAX Framework a qualquer página de conteúdo. Vamos criar uma nova página ASP.NET que exibe um produto selecionado aleatoriamente do banco de dados Northwind. Usaremos o controle Timer do ASP.NET AJAX Framework para atualizar essa exibição a cada 15 segundos, mostrando um novo produto.

Comece criando uma nova página no diretório raiz chamada `ShowRandomProduct.aspx`. Não se esqueça de associar essa nova página à página mestra de `Site.master`.

[![adicionar uma nova página do ASP.NET ao site](master-pages-and-asp-net-ajax-cs/_static/image5.png)](master-pages-and-asp-net-ajax-cs/_static/image4.png)

**Figura 02**: adicionar uma nova página do ASP.net ao site ([clique para exibir a imagem em tamanho normal](master-pages-and-asp-net-ajax-cs/_static/image6.png))

Lembre-se de que, no tutorial [*especificando o título, as marcas meta e outros cabeçalhos HTML na página mestra*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) , criamos uma classe de página de base personalizada chamada `BasePage` que gerou o título da página se ela não estava definida explicitamente. Vá para a classe code-behind da página `ShowRandomProduct.aspx` e faça com que ela derive de `BasePage` (em vez de `System.Web.UI.Page`).

Por fim, atualize o arquivo de `Web.sitemap` para incluir uma entrada para esta lição. Adicione a seguinte marcação abaixo da `<siteMapNode>` da lição mestre para a interação da página de conteúdo:

[!code-xml[Main](master-pages-and-asp-net-ajax-cs/samples/sample2.xml)]

A adição desse elemento de `<siteMapNode>` é refletida na lista de lições (consulte a Figura 5).

### <a name="displaying-a-randomly-selected-product"></a>Exibindo um produto selecionado aleatoriamente

Retornar para `ShowRandomProduct.aspx`. No designer, arraste um controle UpdatePanel da caixa de ferramentas para o controle de conteúdo `MainContent` e defina sua propriedade `ID` como `ProductPanel`. O UpdatePanel representa uma região na tela que pode ser atualizada de forma assíncrona por meio de um postback de página parcial.

Nossa primeira tarefa é exibir informações sobre um produto selecionado aleatoriamente dentro do UpdatePanel. Comece arrastando um controle DetailsView para o UpdatePanel. Defina a propriedade `ID` do controle DetailsView como `ProductInfo` e desmarque suas propriedades `Height` e `Width`. Expanda a marca inteligente de DetailsView e, na lista suspensa escolher fonte de dados, escolha associar o DetailsView a um novo controle SqlDataSource chamado `RandomProductDataSource`.

[![associar o DetailsView a um novo controle SqlDataSource](master-pages-and-asp-net-ajax-cs/_static/image8.png)](master-pages-and-asp-net-ajax-cs/_static/image7.png)

**Figura 03**: associar o DetailsView a um novo controle SqlDataSource ([clique para exibir a imagem em tamanho normal](master-pages-and-asp-net-ajax-cs/_static/image9.png))

Configure o controle SqlDataSource para se conectar ao banco de dados Northwind por meio do `NorthwindConnectionString` (que criamos na página [*interagindo com a página mestra no conteúdo*](interacting-with-the-content-page-from-the-master-page-cs.md) ). Ao configurar a instrução SELECT, escolha especificar uma instrução SQL personalizada e, em seguida, insira a seguinte consulta:

[!code-sql[Main](master-pages-and-asp-net-ajax-cs/samples/sample3.sql)]

A palavra-chave `TOP 1` na cláusula `SELECT` retorna apenas o primeiro registro retornado pela consulta. A [função`NEWID()`](https://msdn.microsoft.com/library/ms190348.aspx) gera um novo [GUID (valor de identificador global exclusivo)](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) e pode ser usado em uma cláusula `ORDER BY` para retornar os registros da tabela em ordem aleatória.

[![configurar o SqlDataSource para retornar um único registro selecionado aleatoriamente](master-pages-and-asp-net-ajax-cs/_static/image11.png)](master-pages-and-asp-net-ajax-cs/_static/image10.png)

**Figura 04**: configurar o SqlDataSource para retornar um único registro selecionado aleatoriamente ([clique para exibir a imagem em tamanho normal](master-pages-and-asp-net-ajax-cs/_static/image12.png))

Depois de concluir o assistente, o Visual Studio cria um BoundField para as duas colunas retornadas pela consulta acima. Neste ponto, a marcação declarativa de sua página deve ser semelhante ao seguinte:

[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample4.aspx)]

A Figura 5 mostra a página de `ShowRandomProduct.aspx` quando exibida por meio de um navegador. Clique no botão atualizar do navegador para recarregar a página; Você deve ver os valores `ProductName` e `UnitPrice` para um novo registro selecionado aleatoriamente.

[![o nome e o preço de um produto aleatório são exibidos](master-pages-and-asp-net-ajax-cs/_static/image14.png)](master-pages-and-asp-net-ajax-cs/_static/image13.png)

**Figura 05**: o nome e o preço de um produto aleatório são exibidos ([clique para exibir a imagem em tamanho normal](master-pages-and-asp-net-ajax-cs/_static/image15.png))

### <a name="automatically-displaying-a-new-product-every-15-seconds"></a>Exibindo automaticamente um novo produto a cada 15 segundos

A estrutura do ASP.NET AJAX inclui um controle timer que executa um postback em um horário especificado; no postback, o evento de `Tick` do temporizador é gerado. Se o controle de timer for colocado dentro de um UpdatePanel, ele dispara um postback de página parcial, durante o qual podemos reassociar os dados ao DetailsView para exibir um novo produto selecionado aleatoriamente.

Para fazer isso, arraste um temporizador da caixa de ferramentas e solte-o no UpdatePanel. Altere o `ID` do temporizador de `Timer1` para `ProductTimer` e sua propriedade `Interval` de 60000 para 15000. A propriedade `Interval` indica o número de milissegundos entre postbacks; defini-lo como 15000 faz com que o temporizador dispare um postback de página parcial a cada 15 segundos. Neste ponto, a marcação declarativa de seu timer deve ser semelhante ao seguinte:

[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample5.aspx)]

Crie um manipulador de eventos para o evento de `Tick` do temporizador. Nesse manipulador de eventos, precisamos reassociar os dados ao DetailsView chamando o método de `DataBind` de DetailsView. Isso instrui o DetailsView a recuperar novamente os dados de seu controle da fonte de dados, que selecionará e exibirá um novo registro selecionado aleatoriamente (assim como ao recarregar a página clicando no botão atualizar do navegador).

[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample6.cs)]

Isso é tudo! Revisite a página por meio de um navegador. Inicialmente, as informações de um produto aleatório são exibidas. Se você observar o paciente da tela, observará que, após 15 segundos, as informações sobre um novo produto substituem mágica a exibição existente.

Para ver melhor o que está acontecendo aqui, vamos adicionar um controle rótulo ao UpdatePanel que exibe a hora em que a exibição foi atualizada pela última vez. Adicione um controle rótulo da Web dentro do UpdatePanel, defina seu `ID` como `LastUpdateTime`e desmarque sua propriedade `Text`. Em seguida, crie um manipulador de eventos para o evento de `Load` do UpdatePanel e exiba a hora atual no rótulo. (O evento `Load` do UpdatePanel é disparado em todos os postbacks de página completos ou parciais.)

[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample7.cs)]

Com essa alteração concluída, a página inclui a hora em que o produto exibido no momento foi carregado. A Figura 6 mostra a página quando visitada pela primeira vez. A Figura 7 mostra a página 15 segundos depois, depois que o controle de temporizador tiver "com tique" e o UpdatePanel tiver sido atualizado para exibir informações sobre um novo produto.

[![um produto selecionado aleatoriamente é exibido no carregamento da página](master-pages-and-asp-net-ajax-cs/_static/image17.png)](master-pages-and-asp-net-ajax-cs/_static/image16.png)

**Figura 06**: um produto selecionado aleatoriamente é exibido no carregamento da página ([clique para exibir a imagem em tamanho normal](master-pages-and-asp-net-ajax-cs/_static/image18.png))

[![a cada 15 segundos, um novo produto selecionado aleatoriamente será exibido](master-pages-and-asp-net-ajax-cs/_static/image20.png)](master-pages-and-asp-net-ajax-cs/_static/image19.png)

**Figura 07**: a cada 15 segundos, um novo produto selecionado aleatoriamente é exibido ([clique para exibir a imagem em tamanho normal](master-pages-and-asp-net-ajax-cs/_static/image21.png))

## <a name="step-3-using-the-scriptmanagerproxy-control"></a>Etapa 3: usando o controle ScriptManagerProxy

Além de incluir o script necessário para a biblioteca de cliente do ASP.NET AJAX Framework, o ScriptManager também pode registrar arquivos JavaScript personalizados, referências a serviços Web habilitados por script e serviços de autenticação, autorização e perfil personalizados. Normalmente, essas personalizações são específicas de uma determinada página. No entanto, se os arquivos de script personalizados, as referências de serviço Web ou os serviços de autenticação, autorização ou perfil forem referenciados no ScriptManager na página mestra, eles serão incluídos em *todas as* páginas do site.

Para adicionar personalizações relacionadas ao ScriptManager em uma base página a página, use o controle ScriptManagerProxy. Você pode adicionar um ScriptManagerProxy a uma página de conteúdo e registrar o arquivo JavaScript personalizado, a referência de serviço Web ou o serviço de autenticação, autorização ou perfil do ScriptManagerProxy; Isso tem o efeito de registrar esses serviços para a página de conteúdo específica.

> [!NOTE]
> Uma página ASP.NET só pode ter mais de um controle ScriptManager presente. Portanto, você não poderá adicionar um controle ScriptManager a uma página de conteúdo se o controle do ScriptManager já estiver definido na página mestra. A única finalidade do ScriptManagerProxy é fornecer uma maneira para os desenvolvedores definirem o ScriptManager na página mestra, mas ainda têm a capacidade de adicionar personalizações do ScriptManager por página.

Para ver o controle ScriptManagerProxy em ação, vamos aumentar o UpdatePanel em `ShowRandomProduct.aspx` para incluir um botão que usa o script do lado do cliente para pausar ou retomar o controle de timer. O controle timer tem três métodos do lado do cliente que podemos usar para obter essa funcionalidade desejada:

- `_startTimer()`-inicia o controle de timer
- `_raiseTick()`-faz com que o controle do temporizador seja "tick" e, portanto, faça o postback e disparando seu evento `Tick` no servidor
- `_stopTimer()`-interrompe o controle de timer

Vamos criar um arquivo JavaScript com uma variável chamada `timerEnabled` e uma função chamada `ToggleTimer`. A variável `timerEnabled` indica se o controle de timer está habilitado ou desabilitado no momento; o padrão é true. A função `ToggleTimer` aceita dois parâmetros de entrada: uma referência ao botão pausar/retomar e o valor de `id` do lado do cliente do controle de timer. Essa função alterna o valor de `timerEnabled`, obtém uma referência para o controle timer, inicia ou interrompe o temporizador (dependendo do valor de `timerEnabled`) e atualiza o texto de exibição do botão para "Pause" ou "resume". Essa função será chamada sempre que o botão pausar/retomar for clicado.

Comece criando uma nova pasta no site denominada `Scripts`. Em seguida, adicione um novo arquivo à pasta scripts chamado `TimerScript.js` do tipo arquivo JScript.

[![adicionar um novo arquivo JavaScript à pasta scripts](master-pages-and-asp-net-ajax-cs/_static/image23.png)](master-pages-and-asp-net-ajax-cs/_static/image22.png)

**Figura 08**: adicionar um novo arquivo JavaScript à pasta `Scripts` ([clique para exibir a imagem em tamanho normal](master-pages-and-asp-net-ajax-cs/_static/image24.png))

[![um novo arquivo JavaScript foi adicionado ao site](master-pages-and-asp-net-ajax-cs/_static/image26.png)](master-pages-and-asp-net-ajax-cs/_static/image25.png)

**Figura 09**: um novo arquivo JavaScript foi adicionado ao site ([clique para exibir a imagem em tamanho normal](master-pages-and-asp-net-ajax-cs/_static/image27.png))

Em seguida, adicione o seguinte cript ao arquivo TimerScript. js:

[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample8.cs)]

Agora, precisamos registrar esse arquivo JavaScript personalizado no `ShowRandomProduct.aspx`. Retornar para `ShowRandomProduct.aspx` e adicionar um controle ScriptManagerProxy à página; Defina seu `ID` como `MyManagerProxy`. Para registrar um arquivo JavaScript personalizado, selecione o controle ScriptManagerProxy no designer e, em seguida, vá para a janela Propriedades. Uma das propriedades é intitulada scripts. A seleção dessa propriedade exibe o editor de coleção ScriptReference mostrado na Figura 10. Clique no botão Adicionar para incluir uma nova referência de script e, em seguida, digite o caminho para o arquivo de script na propriedade Path: `~/Scripts/TimerScript.js`.

[![adicionar uma referência de script ao controle ScriptManagerProxy](master-pages-and-asp-net-ajax-cs/_static/image29.png)](master-pages-and-asp-net-ajax-cs/_static/image28.png)

**Figura 10**: adicionar uma referência de script ao controle ScriptManagerProxy ([clique para exibir a imagem em tamanho normal](master-pages-and-asp-net-ajax-cs/_static/image30.png))

Depois de adicionar a referência de script, a marcação declarativa do controle ScriptManagerProxy é atualizada para incluir uma coleção de `<Scripts>` com uma única entrada de `ScriptReference`, como ilustra o seguinte trecho de marcação:

[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample9.aspx)]

A entrada `ScriptReference` instrui o ScriptManagerProxy a incluir uma referência ao arquivo JavaScript em sua marcação renderizada. Ou seja, ao registrar o script personalizado no ScriptManagerProxy, a saída processada da página de `ShowRandomProduct.aspx` agora inclui outra marca de `<script src="url"></script>`: `<script src="Scripts/TimerScript.js" type="text/javascript"></script>`.

Agora, podemos chamar a função `ToggleTimer` definida em `TimerScript.js` do script de cliente na página `ShowRandomProduct.aspx`. Adicione o seguinte HTML dentro do UpdatePanel:

[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample10.aspx)]

Isso exibe um botão com o texto "Pause". Sempre que ele é clicado, a função JavaScript `ToggleTimer` é chamada, passando uma referência para o botão e o valor de ID do controle timer (`ProductTimer`). Observe a sintaxe para obter o valor `id` do controle timer. `<%=ProductTimer.ClientID%>` emite o valor da propriedade `ClientID` do controle de timer `ProductTimer`. No tutorial [*nome da ID de controle em páginas de conteúdo*](control-id-naming-in-content-pages-cs.md) , discutimos as diferenças entre o valor de `ID` do lado do servidor e o valor de `id` do lado do cliente resultante e como `ClientID` retorna a `id`do lado do cliente.

A Figura 11 mostra essa página quando visitada pela primeira vez por meio de um navegador. O temporizador está em execução no momento e atualiza as informações do produto exibidas a cada 15 segundos. A Figura 12 mostra a tela depois que o botão Pausar é clicado. Clicar no botão Pausar interrompe o timer e atualiza o texto do botão para "retomar". As informações do produto serão atualizadas (e continuarão a ser atualizadas a cada 15 segundos) quando o usuário clicar em continuar.

[![clique no botão pausar para parar o controle de timer](master-pages-and-asp-net-ajax-cs/_static/image32.png)](master-pages-and-asp-net-ajax-cs/_static/image31.png)

**Figura 11**: clique no botão pausar para parar o controle de temporizador ([clique para exibir a imagem em tamanho normal](master-pages-and-asp-net-ajax-cs/_static/image33.png))

[![clique no botão retomar para reiniciar o temporizador](master-pages-and-asp-net-ajax-cs/_static/image35.png)](master-pages-and-asp-net-ajax-cs/_static/image34.png)

**Figura 12**: clique no botão retomar para reiniciar o temporizador ([clique para exibir a imagem em tamanho normal](master-pages-and-asp-net-ajax-cs/_static/image36.png))

## <a name="summary"></a>Resumo

Ao criar aplicativos Web habilitados para AJAX usando o ASP.NET AJAX Framework, é imperativo que cada página da Web habilitada para AJAX inclua um controle ScriptManager. Para facilitar esse processo, podemos adicionar um ScriptManager à página mestra em vez de ter que se lembrar de adicionar um ScriptManager a cada página de conteúdo e a cada. A etapa 1 mostrou como adicionar o ScriptManager à página mestra enquanto a etapa 2 procurou a implementação da funcionalidade do AJAX em uma página de conteúdo.

Se você precisar adicionar scripts personalizados, referências a serviços Web habilitados para script ou serviços de autenticação, autorização ou perfil personalizados a uma página de conteúdo específica, adicione um controle ScriptManagerProxy à página de conteúdo e, em seguida, configure o personalizações lá. A etapa 3 examinou como usar o ScriptManagerProxy para registrar um arquivo JavaScript personalizado em uma página de conteúdo específica.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Estrutura AJAX do ASP.NET](../../../../ajax/index.md)
- [Tutoriais do ASP.NET AJAX](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md)
- [Vídeos do ASP.NET AJAX](../../../videos/aspnet-ajax/index.md)
- [Criando a interface do usuário interativa com o Microsoft ASP.NET AJAX](http://aspnet.4guysfromrolla.com/articles/101007-1.aspx)
- [Usando NEWID para classificar registros aleatoriamente](http://www.sqlteam.com/article/using-newid-to-randomly-sort-records)
- [Usando o controle timer](http://aspnet.4guysfromrolla.com/articles/061808-1.aspx)

### <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de vários livros sobre ASP/ASP. net e fundador da 4GuysFromRolla.com, tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 3,5 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott pode ser contatado em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog em [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](interacting-with-the-content-page-from-the-master-page-cs.md)
> [Próximo](specifying-the-master-page-programmatically-cs.md)
