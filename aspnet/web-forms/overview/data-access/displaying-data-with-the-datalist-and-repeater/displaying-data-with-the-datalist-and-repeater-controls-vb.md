---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-vb
title: Exibindo dados com os controles DataList e Repeater (VB) | Microsoft Docs
author: rick-anderson
description: Nos tutoriais anteriores, usamos o controle GridView para exibir dados. A partir deste tutorial, examinamos a criação de padrões de relatório comuns com...
ms.author: riande
ms.date: 09/13/2006
ms.assetid: 58618954-a9ed-4ca0-8c2d-95a5ffd9c03e
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 4e7aaa1701da67aec61505b64a835ef41031bb13
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74614288"
---
# <a name="displaying-data-with-the-datalist-and-repeater-controls-vb"></a>Exibir dados com os controles DataList e o Repeater (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_29_VB.exe) ou [baixar PDF](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/datatutorial29vb1.pdf)

> Nos tutoriais anteriores, usamos o controle GridView para exibir dados. A partir deste tutorial, examinamos a criação de padrões de relatório comuns com os controles DataList e Repeater, começando com os fundamentos da exibição de dados com esses controles.

## <a name="introduction"></a>Introdução

Em todos os exemplos nos últimos 28 tutoriais, se precisávamos exibir vários registros de uma fonte de dados, transformamos o controle GridView. O GridView renderiza uma linha para cada registro na fonte de dados, exibindo os campos de dados do registro em colunas. Embora o GridView o torne um ajuste para exibir, paginar, classificar, editar e excluir dados, sua aparência é um pouco boxy. Além disso, a marcação responsável pela estrutura GridView s é corrigida, incluindo um `<table>` HTML com uma linha de tabela (`<tr>`) para cada registro e uma célula de tabela (`<td>`) para cada campo.

Para fornecer um grau maior de personalização na aparência e na marcação renderizada ao exibir vários registros, o ASP.NET 2,0 oferece os controles DataList e Repeater (ambos também estão disponíveis na versão 1. x do ASP.NET). Os controles DataList e Repeater renderizam seu conteúdo usando modelos em vez de BoundFields, CheckBoxFields, ButtonFields e assim por diante. Como o GridView, o DataList é renderizado como um `<table>`HTML, mas permite que vários registros de fonte de dados sejam exibidos por linha de tabela. O repetidor, por outro lado, não renderiza nenhuma marcação adicional do que você especifica explicitamente e é um candidato ideal quando você precisa de controle preciso sobre a marcação emitida.

Na próxima dúzia de tutoriais, vamos examinar a criação de padrões de relatório comuns com os controles DataList e Repeater, começando com os fundamentos da exibição de dados com esses modelos de controles. Veremos como formatar esses controles, como alterar o layout dos registros da fonte de dados nos cenários de DataList, mestre/detalhes comuns, maneiras de editar e excluir dados, como paginar os registros e assim por diante.

## <a name="step-1-adding-the-datalist-and-repeater-tutorial-web-pages"></a>Etapa 1: adicionando as páginas da Web do tutorial DataList e Repeater

Antes de iniciar este tutorial, vamos primeiro reservar um momento para adicionar as páginas ASP.NETs que precisaremos para este tutorial e os próximos tutoriais que lidam com a exibição de dados usando o DataList e o Repeater. Comece criando uma nova pasta no projeto chamada `DataListRepeaterBasics`. Em seguida, adicione as cinco páginas do ASP.NET a seguir a essa pasta, fazendo com que todas elas sejam configuradas para usar a página mestra `Site.master`:

- `Default.aspx`
- `Basics.aspx`
- `Formatting.aspx`
- `RepeatColumnAndDirection.aspx`
- `NestedControls.aspx`

![Criar uma pasta DataListRepeaterBasics e adicionar as páginas do tutorial ASP.NET](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image1.png)

**Figura 1**: criar uma pasta `DataListRepeaterBasics` e adicionar as páginas do tutorial ASP.net

Abra a página `Default.aspx` e arraste o `SectionLevelTutorialListing.ascx` controle de usuário da pasta `UserControls` para a superfície de design. Esse controle de usuário, que criamos nas [páginas mestras e](../introduction/master-pages-and-site-navigation-vb.md) no tutorial de navegação do site, enumera o mapa do site e exibe os tutoriais da seção atual em uma lista com marcadores.

[![adicionar o controle de usuário SectionLevelTutorialListing. ascx a default. aspx](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image3.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image2.png)

**Figura 2**: Adicionar o controle de usuário `SectionLevelTutorialListing.ascx` ao `Default.aspx` ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image4.png))

Para que a lista com marcadores exiba os tutoriais DataList e Repeater que vamos criar, precisamos adicioná-los ao mapa do site. Abra o arquivo `Web.sitemap` e adicione a seguinte marcação após a marcação adicionar botões personalizados marca do nó do mapa do site:

[!code-xml[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample1.xml)]

![Atualizar o mapa do site para incluir as novas páginas do ASP.NET](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image5.png)

**Figura 3**: atualizar o mapa do site para incluir as novas páginas do ASP.net

## <a name="step-2-displaying-product-information-with-the-datalist"></a>Etapa 2: exibindo informações do produto com o DataList

Semelhante ao FormView, a saída processada por s do controle DataList depende de modelos em vez de BoundFields, CheckBoxFields e assim por diante. Ao contrário de FormView, o DataList é projetado para exibir um conjunto de registros em vez de um solitários. Vamos começar este tutorial com uma visão da vinculação de informações do produto a um DataList. Comece abrindo a página de `Basics.aspx` na pasta `DataListRepeaterBasics`. Em seguida, arraste um DataList da caixa de ferramentas para o designer. Como a Figura 4 ilustra, antes de especificar os modelos de DataList, o designer o exibe como uma caixa cinza.

[![arrastar a DataList da caixa de ferramentas para o designer](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image7.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image6.png)

**Figura 4**: arraste o DataList da caixa de ferramentas para o designer ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image8.png))

Na marca inteligente DataList s, adicione um novo ObjectDataSource e configure-o para usar o método de `GetProducts` da classe `ProductsBLL`. Como recriamos um DataList somente leitura neste tutorial, defina a lista suspensa como (nenhum) nas guias inserir, atualizar e excluir do assistente.

[![optar por criar um novo ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image10.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image9.png)

**Figura 5**: optar por criar um novo ObjectDataSource ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image11.png))

[![configurar o ObjectDataSource para usar a classe ProductsBLL](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image13.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image12.png)

**Figura 6**: configurar o ObjectDataSource para usar a classe `ProductsBLL` ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image14.png))

[![recuperar informações sobre todos os produtos usando o método GetProducts](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image16.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image15.png)

**Figura 7**: recuperar informações sobre todos os produtos usando o método `GetProducts` ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image17.png))

Depois de configurar o ObjectDataSource e associá-lo com o DataList por meio de sua marca inteligente, o Visual Studio criará automaticamente um `ItemTemplate` no DataList que exibe o nome e o valor de cada campo de dados retornado pela fonte de dados (consulte a marcação abaixo). Essa aparência padrão `ItemTemplate` s é idêntica à dos modelos criados automaticamente ao associar uma fonte de dados ao FormView por meio do designer.

[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample2.aspx)]

> [!NOTE]
> Lembre-se de que, ao associar uma fonte de dados a um controle FormView por meio da marca inteligente FormView s, o Visual Studio criou um `ItemTemplate`, `InsertItemTemplate`e `EditItemTemplate`. No entanto, com o DataList, apenas um `ItemTemplate` é criado. Isso ocorre porque o DataList não tem o mesmo suporte interno de edição e inserção oferecido pelo FormView. O DataList contém eventos relacionados a editar e excluir, e a edição e exclusão de suporte pode ser adicionada com um pouco de código, mas não há suporte pronto para uso simples como o FormView. Veremos como incluir a edição e a exclusão do suporte com o DataList em um tutorial futuro.

Vamos reservar um momento para melhorar a aparência deste modelo. Em vez de exibir todos os campos de dados, vamos exibir apenas o nome, o fornecedor, a categoria, a quantidade por unidade e o preço unitário do produto. Além disso, vamos exibir o nome em um cabeçalho `<h4>` e definir os campos restantes usando um `<table>` abaixo do título.

Para fazer essas alterações, você pode usar os recursos de edição de modelo no designer a partir da marca inteligente DataList s, clique no link editar modelos ou você pode modificar o modelo manualmente por meio da sintaxe declarativa de s da página. Se você usar a opção Editar modelos no designer, sua marcação resultante pode não corresponder exatamente à marcação a seguir, mas quando visualizado por meio de um navegador deve ser muito semelhante à captura de tela mostrada na Figura 8.

[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample3.aspx)]

> [!NOTE]
> O exemplo acima usa rótulos de controles da Web cuja propriedade `Text` é atribuída ao valor da sintaxe DataBinding. Como alternativa, poderíamos omitir os rótulos completamente, digitando apenas a sintaxe DataBinding. Ou seja, em vez de usar `<asp:Label ID="CategoryNameLabel" runat="server" Text='<%# Eval("CategoryName") %>' />` poderíamos ter usado a sintaxe declarativa `<%# Eval("CategoryName") %>`.

No entanto, deixar no rótulo controles da Web oferece duas vantagens. Primeiro, ele fornece um meio mais fácil para formatar os dados com base nos dados, como veremos no próximo tutorial. Em segundo lugar, a opção Editar modelos no designer não exibe a sintaxe declarativa DataBinding que aparece fora de algum controle da Web. Em vez disso, a interface editar modelos foi projetada para facilitar o trabalho com marcação estática e controles da Web e pressupõe que qualquer DataBinding será feita por meio da caixa de diálogo Editar DataBindings, que pode ser acessada por marcas inteligentes de controles da Web.

Portanto, ao trabalhar com o DataList, que fornece a opção de editar os modelos por meio do designer, prefiro usar controles de rótulo da Web para que o conteúdo seja acessível por meio da interface editar modelos. Como veremos em breve, o repetidor exige que o conteúdo do modelo seja editado a partir da exibição da fonte. Consequentemente, ao criar os modelos do repetidor, geralmente omitirei os controles da Web do rótulo, a menos que eu saiba que precisarei Formatar a aparência do texto associado a dados com base na lógica programática.

[![a saída de cada produto é processada usando ItemTemplate s de DataList](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image19.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image18.png)

**Figura 8**: cada saída do produto é renderizada usando o `ItemTemplate` s de DataList ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image20.png))

## <a name="step-3-improving-the-appearance-of-the-datalist"></a>Etapa 3: aprimorando a aparência do DataList

Como o GridView, o DataList oferece várias propriedades relacionadas a estilo, como `Font`, `ForeColor`, `BackColor`, `CssClass`, `ItemStyle`, `AlternatingItemStyle`, `SelectedItemStyle`e assim por diante. Ao trabalhar com os controles GridView e DetailsView, criamos arquivos de capa no tema `DataWebControls` que definiu previamente as propriedades `CssClass` para esses dois controles e a propriedade `CssClass` para várias das suas subpropriedades (`RowStyle`, `HeaderStyle`e assim por diante). Deixe que os s façam o mesmo para o DataList.

Conforme discutido no tutorial [exibindo dados com o ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md) , um arquivo de capa especifica as propriedades padrão relacionadas à aparência para um controle da Web; um tema é uma coleção de arquivos de capa, CSS, de imagem e JavaScript que definem uma aparência específica para um site. No tutorial *exibindo dados com o ObjectDataSource* , criamos um `DataWebControls` tema (que é implementado como uma pasta dentro da pasta `App_Themes`) que tem, atualmente, dois arquivos de capa-`GridView.skin` e `DetailsView.skin`. Vamos adicionar um terceiro arquivo de capa para especificar as configurações de estilo predefinidas para DataList.

Para adicionar um arquivo de capa, clique com o botão direito do mouse na pasta `App_Themes/DataWebControls`, escolha Adicionar um novo item e selecione a opção arquivo de capa na lista. Dê o nome `DataList.skin` para o arquivo.

[![criar um novo arquivo de capa chamado DataList. Skin](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image22.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image21.png)

**Figura 9**: criar um novo arquivo de capa chamado `DataList.skin` ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image23.png))

Use a seguinte marcação para o arquivo de `DataList.skin`:

[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample4.aspx)]

Essas configurações atribuem as mesmas classes CSS às propriedades DataList apropriadas como usadas com os controles GridView e DetailsView. As classes CSS usadas aqui `DataWebControlStyle`, `AlternatingRowStyle`, `RowStyle`e assim por diante são definidas no arquivo `Styles.css` e foram adicionadas aos tutoriais anteriores.

Com a adição desse arquivo de capa, a aparência de s do DataList é atualizada no designer (talvez seja necessário atualizar a exibição do designer para ver os efeitos do novo arquivo de capa; no menu Exibir, escolha Atualizar). Como mostra a Figura 10, cada produto alternado tem uma cor de fundo rosa clara.

[![criar um novo arquivo de capa chamado DataList. Skin](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image25.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image24.png)

**Figura 10**: criar um novo arquivo de capa chamado `DataList.skin` ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image26.png))

## <a name="step-4-exploring-the-datalist-s-other-templates"></a>Etapa 4: explorando os outros modelos DataList

Além do `ItemTemplate`, o DataList dá suporte a seis outros modelos opcionais:

- `HeaderTemplate` se fornecido, adiciona uma linha de cabeçalho à saída e é usada para renderizar essa linha
- `AlternatingItemTemplate` usado para renderizar itens alternados
- `SelectedItemTemplate` usado para processar o item selecionado; o item selecionado é o item cujo índice corresponde à Propriedade DataList s [`SelectedIndex`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.selectedindex.aspx)
- `EditItemTemplate` usado para renderizar o item que está sendo editado
- `SeparatorTemplate` se fornecido, adiciona um separador entre cada item e é usado para renderizar esse separador
- `FooterTemplate`-se fornecida, adiciona uma linha de rodapé à saída e é usada para renderizar esta linha

Ao especificar o `HeaderTemplate` ou `FooterTemplate`, o DataList adiciona uma linha de cabeçalho ou rodapé adicional à saída renderizada. Assim como nas linhas de cabeçalho e rodapé de GridView, o cabeçalho e o rodapé em um DataList não estão associados aos dados. Portanto, qualquer sintaxe de DataBinding no `HeaderTemplate` ou `FooterTemplate` que tenta acessar dados associados retornará uma cadeia de caracteres em branco.

> [!NOTE]
> Como vimos na exibição de [informações resumidas no tutorial de rodapé do GridView](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md) , enquanto as linhas do cabeçalho e do rodapé Don t dão suporte à sintaxe DataBinding, informações específicas de dados podem ser injetadas diretamente nessas linhas do manipulador de eventos GridView s `RowDataBound`. Essa técnica pode ser usada para calcular os totais de execução ou outras informações dos dados associados ao controle, bem como atribuir essas informações ao rodapé. Esse mesmo conceito pode ser aplicado aos controles DataList e Repeater; a única diferença é que, para o DataList e o Repeater, crie um manipulador de eventos para o evento `ItemDataBound` (em vez de para o evento `RowDataBound`).

Para nosso exemplo, vamos ter o título informações do produto exibidas na parte superior dos resultados de DataList em um cabeçalho `<h3>`. Para fazer isso, adicione um `HeaderTemplate` com a marcação apropriada. No designer, isso pode ser feito clicando no link editar modelos na marca inteligente s de DataList, escolhendo o modelo de cabeçalho na lista suspensa e digitando o texto depois de escolher a opção título 3 na lista suspensa estilo (consulte a Figura 11).

[![adicionar um HeaderTemplate com as informações de produto de texto](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image28.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image27.png)

**Figura 11**: adicionar um `HeaderTemplate` com as informações do produto de texto ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image29.png))

Como alternativa, isso pode ser adicionado declarativamente digitando a seguinte marcação dentro das marcas de `<asp:DataList>`:

[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample5.html)]

Para adicionar um pouco de espaço entre cada listagem de produtos, vamos adicionar uma `SeparatorTemplate` que inclui uma linha entre cada seção. A marca de regra horizontal (`<hr>`), adiciona tal divisor. Crie o `SeparatorTemplate` para que ele tenha a seguinte marcação:

[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample6.html)]

> [!NOTE]
> Assim como o `HeaderTemplate` e `FooterTemplates`, o `SeparatorTemplate` não está associado a nenhum registro da fonte de dados e, portanto, não pode acessar diretamente os registros de fonte de dados associados ao DataList.

Depois de fazer essa adição, ao exibir a página por meio de um navegador, ela deverá ser semelhante à figura 12. Observe a linha de cabeçalho e a linha entre cada listagem de produtos.

[![o DataList inclui uma linha de cabeçalho e uma regra horizontal entre cada listagem de produtos](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image31.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image30.png)

**Figura 12**: o DataList inclui uma linha de cabeçalho e uma regra horizontal entre cada listagem de produto ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image32.png))

## <a name="step-5-rendering-specific-markup-with-the-repeater-control"></a>Etapa 5: renderizando marcação específica com o controle Repeater

Se você fizer uma exibição/origem do seu navegador ao visitar o exemplo DataList da figura 12, verá que o DataList emite um `<table>` HTML que contém uma linha de tabela (`<tr>`) com uma única célula de tabela (`<td>`) para cada item associado ao DataList. Essa saída, na verdade, é idêntica à que seria emitida de um GridView com um único TemplateField. Como veremos em um tutorial futuro, o DataList permite mais personalização da saída, permitindo exibir vários registros de fonte de dados por linha de tabela.

Mas se você não quiser emitir um `<table>`HTML, mas? Para obter controle total e completo sobre a marcação gerada por um controle da Web de dados, devemos usar o controle Repeater. Como o DataList, o repetidor é construído com base nos modelos. O repetidor, no entanto, oferece apenas os cinco modelos a seguir:

- `HeaderTemplate`, se fornecido, adiciona a marcação especificada antes dos itens
- `ItemTemplate` usado para processar itens
- `AlternatingItemTemplate` se fornecido, usado para renderizar itens alternados
- `SeparatorTemplate`, se fornecido, adiciona a marcação especificada entre cada item
- `FooterTemplate`-se fornecido, adiciona a marcação especificada após os itens

No ASP.NET 1. x, o controle Repeater era comumente usado para exibir uma lista com marcadores cujos dados vieram de alguma fonte de dados. Nesse caso, o `HeaderTemplate` e `FooterTemplates` contêm as marcas de abertura e fechamento `<ul>`, respectivamente, enquanto a `ItemTemplate` conteria elementos `<li>` com a sintaxe DataBinding. Essa abordagem ainda pode ser usada no ASP.NET 2,0 como vimos em dois exemplos nas [páginas mestras e](../introduction/master-pages-and-site-navigation-vb.md) no tutorial de navegação do site:

- Na página `Site.master` mestra, um repetidor foi usado para exibir uma lista com marcadores do conteúdo do mapa do site de nível superior (relatórios básicos, filtragem de relatórios, formatação personalizada e assim por diante); outro, repetidor aninhado foi usado para exibir as seções filhas das seções de nível superior
- No `SectionLevelTutorialListing.ascx`, um repetidor foi usado para exibir uma lista com marcadores das seções filhas da seção do mapa do site atual

> [!NOTE]
> ASP.NET 2,0 apresenta o novo [controle BulletedList](https://msdn.microsoft.com/library/ms228101.aspx), que pode ser associado a um controle da fonte de dados para exibir uma lista com marcadores simples. Com o controle BulletedList, não precisamos especificar nenhum HTML relacionado à lista; em vez disso, simplesmente indicamos o campo de dados a ser exibido como o texto de cada item de lista.

O Repeater serve como um controle da Web capturar todos os dados. Se não houver um controle existente que gere a marcação necessária, o controle Repeater poderá ser usado. Para ilustrar o uso do Repeater, vamos ter a lista de categorias exibida acima das informações do produto DataList criado na etapa 2. Em particular, vamos ter as categorias exibidas em um HTML de linha única `<table>` com cada categoria exibida como uma coluna na tabela.

Para fazer isso, comece arrastando um controle Repeater da caixa de ferramentas para o designer, acima das informações do produto DataList. Assim como com o DataList, o repetidor inicialmente é exibido como uma caixa cinza até que seus modelos tenham sido definidos.

[![adicionar um repetidor ao designer](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image34.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image33.png)

**Figura 13**: adicionar um repetidor ao designer ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image35.png))

Há apenas uma opção na marca inteligente repetida s: escolha fonte de dados. Opte por criar um novo ObjectDataSource e configure-o para usar o método de `GetCategories` da classe `CategoriesBLL`.

[![criar um novo ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image37.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image36.png)

**Figura 14**: criar um novo ObjectDataSource ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image38.png))

[![configurar o ObjectDataSource para usar a classe CategoriesBLL](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image40.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image39.png)

**Figura 15**: configurar o ObjectDataSource para usar a classe `CategoriesBLL` ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image41.png))

[![recuperar informações sobre todas as categorias usando o método GetCategories](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image43.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image42.png)

**Figura 16**: recuperar informações sobre todas as categorias usando o método `GetCategories` ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image44.png))

Ao contrário do DataList, o Visual Studio não cria automaticamente um ItemTemplate para o repetidor depois de associá-lo a uma fonte de dados. Além disso, os modelos do Repeater s não podem ser configurados por meio do designer e devem ser especificados de forma declarativa.

Para exibir as categorias como uma `<table>` de linha única com uma coluna para cada categoria, precisamos que o repetidor emita uma marcação semelhante à seguinte:

[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample7.html)]

Como o texto de `<td>Category X</td>` é a parte que se repete, isso aparecerá no ItemTemplate s do repetidor. A marcação que aparece antes de `<table><tr>`-será colocada na `HeaderTemplate` enquanto a marcação final-`</tr></table>`-será colocada no `FooterTemplate`. Para inserir essas configurações de modelo, vá para a parte declarativa da página ASP.NET clicando no botão fonte no canto inferior esquerdo e digite a seguinte sintaxe:

[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample8.aspx)]

O Repeater emite a marcação precisa conforme especificado por seus modelos, nada mais, nada menos. A figura 17 mostra a saída do repetidor de s quando exibida por meio de um navegador.

[![uma tabela de &lt;HTML de linha única&gt; lista cada categoria em uma coluna separada](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image46.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image45.png)

**Figura 17**: um HTML de linha única `<table>` lista cada categoria em uma coluna separada ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image47.png))

## <a name="step-6-improving-the-appearance-of-the-repeater"></a>Etapa 6: melhorando a aparência do repetidor

Como o Repeater emite precisamente a marcação especificada por seus modelos, ela deve ser tão surpresa que não há nenhuma propriedade relacionada ao estilo para o repetidor. Para alterar a aparência do conteúdo gerado pelo repetidor, devemos adicionar manualmente o conteúdo HTML ou CSS necessário diretamente aos modelos s do repetidor.

Para nosso exemplo, vamos ter as colunas de categoria cores de plano de fundo alternativas, como com as linhas alternadas no DataList. Para fazer isso, precisamos atribuir o `RowStyle` classe CSS a cada item de repetidor e a classe CSS `AlternatingRowStyle` a cada item Repetidor alternado por meio dos modelos `ItemTemplate` e `AlternatingItemTemplate`, desta forma:

[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample9.aspx)]

Deixe que o s também adicione uma linha de cabeçalho à saída com as categorias de produto texto. Como não sabemos quantas colunas nossas `<table>` resultantes serão compostas, a maneira mais simples de gerar uma linha de cabeçalho com garantia de abranger todas as colunas é usar *duas* `<table>` s. A primeira `<table>` conterá duas linhas a linha de cabeçalho e uma linha que conterá o segundo, uma única linha `<table>` que tenha uma coluna para cada categoria no sistema. Ou seja, desejamos emitir a seguinte marcação:

[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample10.html)]

O `HeaderTemplate` e `FooterTemplate` a seguir resultam na marcação desejada:

[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample11.aspx)]

A Figura 18 mostra o repetidor após essas alterações terem sido feitas.

[![as colunas de categoria alternam na cor do plano de fundo e incluem uma linha de cabeçalho](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image49.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image48.png)

**Figura 18**: as colunas de categoria alternam na cor da tela de fundo e incluem uma linha de cabeçalho ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image50.png))

## <a name="summary"></a>Resumo

Embora o controle GridView facilite a exibição, edição, exclusão, classificação e a página por meio de dados, a aparência é muito boxy e semelhante à de grade. Para obter mais controle sobre a aparência, precisamos voltar para os controles DataList ou Repeater. Ambos os controles exibem um conjunto de registros usando modelos em vez de BoundFields, CheckBoxFields e assim por diante.

O DataList é renderizado como um `<table>` HTML que, por padrão, exibe cada registro da fonte de dados em uma única linha de tabela, assim como um GridView com um único TemplateField. Como veremos em um tutorial futuro, no entanto, o DataList permite que vários registros sejam exibidos por linha de tabela. O repetidor, por outro lado, emite estritamente a marcação especificada em seus modelos; Ele não adiciona nenhuma marcação adicional e, portanto, é usado normalmente para exibir dados em elementos HTML diferentes de um `<table>` (como em uma lista com marcadores).

Embora o DataList e o Repeater ofereçam mais flexibilidade em sua saída renderizada, eles não têm muitos dos recursos internos encontrados no GridView. Como vamos examinar nos próximos tutoriais, alguns desses recursos podem ser conectados novamente sem muito esforço, mas lembre-se de que usar o DataList ou o repetidor no lugar do GridView limita os recursos que você pode usar sem a necessidade de implementar esses recursos corrigi.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores potenciais para este tutorial foram Yaakov Ellis, Liz Shulok, Randy Schmidt e Stacy Park. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](nested-data-web-controls-cs.md)
> [Próximo](formatting-the-datalist-and-repeater-based-upon-data-vb.md)
