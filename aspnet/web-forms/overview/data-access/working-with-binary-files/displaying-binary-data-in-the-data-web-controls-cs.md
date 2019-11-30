---
uid: web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-cs
title: Exibindo dados binários nos controles da WebC#de dados () | Microsoft Docs
author: rick-anderson
description: Neste tutorial, examinaremos as opções para apresentar dados binários em uma página da Web, incluindo a exibição de um arquivo de imagem e o provisionamento de um link de ' download ' f...
ms.author: riande
ms.date: 03/27/2007
ms.assetid: 5cbeb9f8-5f92-4ba8-87ae-0b4d460ae6d4
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: f38de7adcd77b3dc2622759646168cf533b8308f
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74642460"
---
# <a name="displaying-binary-data-in-the-data-web-controls-c"></a>Exibir dados binários nos controles de dados da Web (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_55_CS.exe) ou [baixar PDF](displaying-binary-data-in-the-data-web-controls-cs/_static/datatutorial55cs1.pdf)

> Neste tutorial, examinaremos as opções para apresentar dados binários em uma página da Web, incluindo a exibição de um arquivo de imagem e o provisionamento de um link de "download" para um arquivo PDF.

## <a name="introduction"></a>Introdução

No tutorial anterior, exploramos as duas técnicas para associar dados binários a um modelo de dados subjacente de aplicativo e usavam o controle FileUpload para carregar arquivos de um navegador para o sistema de arquivos do servidor Web. Ainda vemos como associar os dados binários carregados ao modelo de dados. Ou seja, depois que um arquivo for carregado e salvo no sistema de arquivos, um caminho para o arquivo deverá ser armazenado no registro de banco de dados apropriado. Se os dados estiverem sendo armazenados diretamente no banco de dados, não será necessário salvá-los no sistema de arquivos, mas eles deverão ser injetados no banco de dado.

Antes de observarmos a Associação dos dados com o modelo de dados, no entanto, vamos examinar primeiro como fornecer os dados binários para o usuário final. A apresentação de dados de texto é simples o suficiente, mas como os dados binários devem ser apresentados? Isso depende, é claro, sobre o tipo de dados binários. Para imagens, provavelmente desejamos exibir a imagem; para PDFs, documentos do Microsoft Word, arquivos ZIP e outros tipos de dados binários, o fornecimento de um link de download é provavelmente mais apropriado.

Neste tutorial, veremos como apresentar os dados binários junto com seus dados de texto associados usando controles da Web de dados como GridView e DetailsView. No próximo tutorial, vamos transformar nossa atenção para associar um arquivo carregado ao banco de dados.

## <a name="step-1-providingbrochurepathvalues"></a>Etapa 1: fornecendo valores de`BrochurePath`

A coluna `Picture` na tabela `Categories` já contém dados binários para as várias imagens de categoria. Especificamente, a coluna `Picture` de cada registro contém o conteúdo binário de uma imagem de bitmap granular, de baixa qualidade e de 16 cores. Cada imagem de categoria tem 172 pixels de largura e 120 pixels de altura e consome aproximadamente 11 KB. Além disso, o conteúdo binário na coluna `Picture` inclui um cabeçalho [OLE](http://en.wikipedia.org/wiki/Object_Linking_and_Embedding) de 78 bytes que deve ser eliminado antes da exibição da imagem. Essas informações de cabeçalho estão presentes porque o banco de dados Northwind tem suas raízes no Microsoft Access. No Access, os dados binários são armazenados usando o tipo de dados objeto OLE, que se encontra nesse cabeçalho. Por enquanto, veremos como remover os cabeçalhos dessas imagens de baixa qualidade para exibir a imagem. Em um tutorial futuro, criaremos uma interface para atualizar uma coluna de `Picture` de categoria e substituirei essas imagens de bitmap que usam cabeçalhos OLE por imagens JPG equivalentes sem os cabeçalhos OLE desnecessários.

No tutorial anterior, vimos como usar o controle FileUpload. Portanto, você pode adicionar arquivos de folheto ao sistema de arquivos do servidor Web. Isso, no entanto, não atualiza a coluna `BrochurePath` na tabela `Categories`. No próximo tutorial, veremos como fazer isso, mas, por enquanto, precisamos fornecer valores manualmente para essa coluna.

Neste download do tutorial, você encontrará sete arquivos de folheto em PDF na pasta `~/Brochures`, um para cada uma das categorias, exceto frutos do mar. Eu omiti propositadamente a adição de um folheto de frutos do mar para ilustrar como lidar com cenários em que nem todos os registros têm dados binários associados. Para atualizar a tabela de `Categories` com esses valores, clique com o botão direito do mouse no nó `Categories` de Gerenciador de Servidores e escolha Mostrar dados da tabela. Em seguida, insira os caminhos virtuais para os arquivos de folheto para cada categoria que tenha um folheto, como mostra a Figura 1. Como não há um folheto para a categoria de frutos do mar, deixe seu valor de `BrochurePath` coluna s como `NULL`.

[![Insira manualmente os valores para a coluna BrochurePath da tabela de categorias](displaying-binary-data-in-the-data-web-controls-cs/_static/image1.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image1.png)

**Figura 1**: Insira manualmente os valores para a coluna `Categories` s `BrochurePath` ([clique para exibir a imagem em tamanho normal](displaying-binary-data-in-the-data-web-controls-cs/_static/image2.png))

## <a name="step-2-providing-a-download-link-for-the-brochures-in-a-gridview"></a>Etapa 2: fornecer um link de download para os folhetos em um GridView

Com os valores de `BrochurePath` fornecidos para a tabela `Categories`, estamos prontos para criar um GridView que liste cada categoria junto com um link para baixar o folheto da categoria. Na etapa 4, estenderemos esse GridView para exibir também a imagem da categoria s.

Comece arrastando um GridView da caixa de ferramentas para o designer da página `DisplayOrDownloadData.aspx` na pasta `BinaryData`. Defina as `ID` s GridView como `Categories` e por meio da marca inteligente GridView s, escolha associá-la a uma nova fonte de dados. Especificamente, associe-o a um ObjectDataSource chamado `CategoriesDataSource` que recupera dados usando o método `GetCategories()` do objeto `CategoriesBLL`.

[![criar um novo ObjectDataSource chamado CategoriesDataSource](displaying-binary-data-in-the-data-web-controls-cs/_static/image2.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image3.png)

**Figura 2**: criar um novo ObjectDataSource chamado `CategoriesDataSource` ([clique para exibir a imagem em tamanho normal](displaying-binary-data-in-the-data-web-controls-cs/_static/image4.png))

[![configurar o ObjectDataSource para usar a classe CategoriesBLL](displaying-binary-data-in-the-data-web-controls-cs/_static/image3.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image5.png)

**Figura 3**: configurar o ObjectDataSource para usar a classe `CategoriesBLL` ([clique para exibir a imagem em tamanho normal](displaying-binary-data-in-the-data-web-controls-cs/_static/image6.png))

[![recuperar a lista de categorias usando o método GetCategories ()](displaying-binary-data-in-the-data-web-controls-cs/_static/image4.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image7.png)

**Figura 4**: recuperar a lista de categorias usando o método `GetCategories()` ([clique para exibir a imagem em tamanho normal](displaying-binary-data-in-the-data-web-controls-cs/_static/image8.png))

Depois de concluir o assistente para configurar fonte de dados, o Visual Studio adicionará automaticamente um BoundField ao `Categories` GridView para os `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`e `BrochurePath` `DataColumn` s. Vá em frente e remova o `NumberOfProducts` BoundField, uma vez que a consulta `GetCategories()` método s não recupera essas informações. Além disso, remova o `CategoryID` BoundField e renomeie o `CategoryName` e `BrochurePath` BoundFields `HeaderText` Propriedades para categoria e folheto, respectivamente. Depois de fazer essas alterações, a marcação declarativa de GridView e ObjectDataSource s deve ser parecida com a seguinte:

[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample1.aspx)]

Exiba esta página por meio de um navegador (veja a Figura 5). Cada uma das oito categorias é listada. As sete categorias com valores `BrochurePath` têm o valor de `BrochurePath` exibido no respectivo BoundField. Frutos do mar, que tem um valor `NULL` para sua `BrochurePath`, exibe uma célula vazia.

[![cada nome, descrição e valor de BrochurePath da categoria está listado](displaying-binary-data-in-the-data-web-controls-cs/_static/image5.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image9.png)

**Figura 5**: cada nome de categoria, descrição e `BrochurePath` valor é listado ([clique para exibir a imagem em tamanho normal](displaying-binary-data-in-the-data-web-controls-cs/_static/image10.png))

Em vez de exibir o texto da coluna `BrochurePath`, queremos criar um link para o folheto. Para fazer isso, remova o `BrochurePath` BoundField e substitua-o por um HyperLinkField. Defina a propriedade New HyperLinkField s `HeaderText` como panfleto, sua propriedade `Text` para exibir o folheto e sua propriedade `DataNavigateUrlFields` como `BrochurePath`.

![Adicionar um HyperLinkField para BrochurePath](displaying-binary-data-in-the-data-web-controls-cs/_static/image6.gif)

**Figura 6**: adicionar um HyperLinkField para `BrochurePath`

Isso adicionará uma coluna de links ao GridView, como mostra a Figura 7. Clicar em um link exibir folheto exibirá o PDF diretamente no navegador ou solicitará que o usuário Baixe o arquivo, dependendo se um leitor de PDF está instalado e as configurações do navegador.

[![um folheto de categoria s pode ser exibido clicando no link exibir folheto](displaying-binary-data-in-the-data-web-controls-cs/_static/image7.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image11.png)

**Figura 7**: um folheto de categoria s pode ser exibido clicando no link exibir folheto ([clique para exibir a imagem em tamanho normal](displaying-binary-data-in-the-data-web-controls-cs/_static/image12.png))

[![o folheto da categoria s brochura é exibido](displaying-binary-data-in-the-data-web-controls-cs/_static/image8.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image13.png)

**Figura 8**: a categoria PDF do folheto de categorias é exibida ([clique para exibir a imagem em tamanho normal](displaying-binary-data-in-the-data-web-controls-cs/_static/image14.png))

## <a name="hiding-the-view-brochure-text-for-categories-without-a-brochure"></a>Ocultando o texto de folheto de exibição para categorias sem um folheto

Como mostra a Figura 7, o `BrochurePath` HyperLinkField exibe seu valor de propriedade `Text` (Visualizar folheto) para todos os registros, independentemente de um valor não`NULL` para `BrochurePath`. É claro que, se `BrochurePath` for `NULL`, o link será exibido somente como texto, como é o caso com a categoria de frutos do mar (consulte novamente a Figura 7). Em vez de exibir o folheto de exibição de texto, pode ser interessante ter essas categorias sem um valor `BrochurePath` exibir algum texto alternativo, como nenhum folheto disponível.

Para fornecer esse comportamento, precisamos usar um TemplateField cujo conteúdo é gerado por meio de uma chamada para um método Page que emite a saída apropriada com base no valor `BrochurePath`. Primeiro exploramos essa técnica de formatação no tutorial [Usando TemplateFields no controle GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) .

Transforme o HyperLinkField em um TemplateField selecionando o `BrochurePath` HyperLinkField e clicando no link converter este campo em um TemplateField na caixa de diálogo Editar colunas.

![Converter o HyperLinkField em um TemplateField](displaying-binary-data-in-the-data-web-controls-cs/_static/image9.gif)

**Figura 9**: converter o HyperLinkField em um TemplateField

Isso criará um TemplateField com um `ItemTemplate` que contém um controle Web de hiperlink cuja propriedade `NavigateUrl` está associada ao valor `BrochurePath`. Substitua essa marcação por uma chamada para o método `GenerateBrochureLink`, passando o valor de `BrochurePath`:

[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample2.aspx)]

Em seguida, crie um método de `protected` na classe code-behind da página ASP.NET chamada `GenerateBrochureLink` que retorna um `string` e aceita um `object` como um parâmetro de entrada.

[!code-csharp[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample3.cs)]

Esse método determina se o valor de `object` passado é um banco de dados `NULL` e, em caso afirmativo, retorna uma mensagem indicando que a categoria não tem um folheto. Caso contrário, se houver um valor `BrochurePath`, ele será exibido em um hiperlink. Observe que, se o valor de `BrochurePath` estiver presente, ele será passado para o [método`ResolveUrl(url)`](https://msdn.microsoft.com/library/system.web.ui.control.resolveurl.aspx). Esse método resolve a *URL*passada, substituindo o `~` caractere pelo caminho virtual apropriado. Por exemplo, se o aplicativo tiver raiz em `/Tutorial55`, `ResolveUrl("~/Brochures/Meats.pdf")` retornará `/Tutorial55/Brochures/Meat.pdf`.

A Figura 10 mostra a página após essas alterações terem sido aplicadas. Observe que a categoria de frutos do mar `BrochurePath` campo agora exibe o texto sem folheto disponível.

[![o texto nenhum folheto disponível é exibido para essas categorias sem um folheto](displaying-binary-data-in-the-data-web-controls-cs/_static/image10.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image15.png)

**Figura 10**: o texto nenhum folheto disponível é exibido para essas categorias sem um folheto ([clique para exibir a imagem em tamanho normal](displaying-binary-data-in-the-data-web-controls-cs/_static/image16.png))

## <a name="step-3-adding-a-web-page-to-display-a-category-s-picture"></a>Etapa 3: adicionar uma página da Web para exibir uma imagem de categoria s

Quando um usuário visita uma página ASP.NET, ele recebe o HTML página s do ASP.NET. O HTML recebido é apenas texto e não contém dados binários. Quaisquer dados binários adicionais, como imagens, arquivos de som, aplicativos do Macromedia Flash, vídeos incorporados do Windows Media Player e assim por diante, existem como recursos separados no servidor Web. O HTML contém referências a esses arquivos, mas não inclui o conteúdo real dos arquivos.

Por exemplo, em HTML, o elemento `<img>` é usado para fazer referência a uma imagem, com o atributo `src` apontando para o arquivo de imagem da seguinte forma:

[!code-html[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample4.html)]

Quando um navegador recebe esse HTML, ele faz outra solicitação ao servidor Web para recuperar o conteúdo binário do arquivo de imagem, que é exibido no navegador. O mesmo conceito se aplica a qualquer dado binário. Na etapa 2, o folheto não foi enviado para o navegador como parte da marcação HTML da página. Em vez disso, o HTML renderizado forneceu hiperlinks que, quando clicados, fizeram com que o navegador solicitasse o documento PDF diretamente.

Para exibir ou permitir que os usuários baixem dados binários que residem dentro do banco de dado, precisamos criar uma página da Web separada que retorne os dados. Para o nosso aplicativo, há apenas um campo de dados binários armazenado diretamente no banco do dados da categoria s Picture. Portanto, precisamos de uma página que, quando chamada, retorna os dados da imagem para uma determinada categoria.

Adicione uma nova página do ASP.NET à pasta `BinaryData` chamada `DisplayCategoryPicture.aspx`. Ao fazer isso, deixe a caixa de seleção selecionar página mestra desmarcada. Esta página espera um valor `CategoryID` na QueryString e retorna os dados binários dessa categoria `Picture` coluna. Como essa página retorna dados binários e nada mais, ela não precisa de nenhuma marcação na seção HTML. Portanto, clique na guia origem no canto inferior esquerdo e remova todas as marcas de s da página, exceto a diretiva de `<%@ Page %>`. Ou seja, `DisplayCategoryPicture.aspx` s marcação declarativa deve consistir em uma única linha:

[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample5.aspx)]

Se você vir o atributo `MasterPageFile` na diretiva `<%@ Page %>`, remova-o.

Na classe s code-behind da página, adicione o seguinte código ao manipulador de eventos `Page_Load`:

[!code-csharp[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample6.cs)]

Esse código começa lendo o valor de `CategoryID` QueryString em uma variável chamada `categoryID`. Em seguida, os dados da imagem são recuperados por meio de uma chamada para o método de `GetCategoryWithBinaryDataByCategoryID(categoryID)` da classe `CategoriesBLL`. Esses dados são retornados ao cliente usando o método `Response.BinaryWrite(data)`, mas antes que isso seja chamado, o cabeçalho OLE do valor da coluna `Picture` deve ser removido. Isso é feito com a criação de uma matriz de `byte` chamada `strippedImageData` que terá exatamente 78 caracteres abaixo do que está na coluna `Picture`. O [método`Array.Copy`](https://msdn.microsoft.com/library/z50k9bft.aspx) é usado para copiar os dados de `category.Picture` começando na posição 78 para `strippedImageData`.

A propriedade `Response.ContentType` especifica o [tipo MIME](http://en.wikipedia.org/wiki/MIME) do conteúdo que está sendo retornado para que o navegador saiba como renderizá-lo. Como a coluna de `Picture` da tabela `Categories` é uma imagem de bitmap, o tipo MIME de bitmap é usado aqui (Image/BMP). Se você omitir o tipo MIME, a maioria dos navegadores ainda exibirá a imagem corretamente, pois eles podem inferir o tipo com base no conteúdo dos dados binários do arquivo de imagem. No entanto, é prudente incluir o tipo MIME quando possível. Consulte o [site da Internet Assigned Numbers s](http://www.iana.org/) para obter uma lista completa de [tipos de mídia MIME](http://www.iana.org/assignments/media-types/).

Com essa página criada, uma imagem de s de categoria específica pode ser exibida visitando `DisplayCategoryPicture.aspx?CategoryID=categoryID`. A Figura 11 mostra a imagem dos s categoria de bebidas, que pode ser exibida em `DisplayCategoryPicture.aspx?CategoryID=1`.

[![a imagem da categoria de bebidas é exibida](displaying-binary-data-in-the-data-web-controls-cs/_static/image11.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image17.png)

**Figura 11**: a figura s da categoria bebidas é exibida ([clique para exibir a imagem em tamanho normal](displaying-binary-data-in-the-data-web-controls-cs/_static/image18.png))

Se, ao visitar `DisplayCategoryPicture.aspx?CategoryID=categoryID`, você receberá uma exceção que lê não é possível converter o objeto do tipo ' System. DBNull ' no tipo ' System. Byte [] ', há duas coisas que podem estar causando isso. Primeiro, a coluna de `Picture` da tabela `Categories` permite valores de `NULL`. No entanto, a página de `DisplayCategoryPicture.aspx` pressupõe que há um valor não`NULL` presente. A propriedade `Picture` da `CategoriesDataTable` não poderá ser acessada diretamente se tiver um valor `NULL`. Se você quiser permitir `NULL` valores para a coluna `Picture`, você d deseja incluir a seguinte condição:

[!code-csharp[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample7.cs)]

O código acima pressupõe que há um arquivo de imagem chamado `NoPictureAvailable.gif` na pasta `Images` que você deseja exibir para essas categorias sem uma imagem.

Essa exceção também pode ser causada se a instrução `CategoriesTableAdapter` s `GetCategoryWithBinaryDataByCategoryID` método s `SELECT` foi revertida para a lista de colunas da consulta principal, o que pode acontecer se você estiver usando instruções SQL ad hoc e você salvar novamente o assistente para a consulta principal do TableAdapter s. Verifique se `GetCategoryWithBinaryDataByCategoryID` instrução `SELECT` do método ainda inclui a coluna `Picture`.

> [!NOTE]
> Toda vez que o `DisplayCategoryPicture.aspx` é visitado, o banco de dados é acessado e a categoria especificada do s Picture data é retornada. No entanto, se a categoria s Picture não foi alterada desde que o usuário o exibiu pela última vez, isso é um esforço desperdiçado. Felizmente, o HTTP permite *gets condicionais*. Com um GET condicional, o cliente que faz a solicitação HTTP envia ao longo de um [cabeçalho http`If-Modified-Since`](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html) que fornece a data e a hora em que o cliente recuperou por último esse recurso do servidor Web. Se o conteúdo não foi alterado desde essa data especificada, o servidor Web pode responder com um [código de status não modificado (304)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) e abrem mão enviar de volta o conteúdo de recurso s solicitado. Resumindo, essa técnica alivia o servidor Web de ter que enviar o conteúdo de um recurso se ele não tiver sido modificado desde a última vez que o cliente o acessou.

No entanto, para implementar esse comportamento, é necessário adicionar uma coluna `PictureLastModified` à tabela `Categories` para capturar quando a coluna de `Picture` foi atualizada pela última vez, bem como o código para verificar o cabeçalho de `If-Modified-Since`. Para obter mais informações sobre o cabeçalho de `If-Modified-Since` e o fluxo de trabalho de obtenção condicional, consulte [http condicional get para hackers de RSS](http://fishbowl.pastiche.org/2002/10/21/http_conditional_get_for_rss_hackers) e [uma análise mais profunda sobre como executar solicitações HTTP em uma página ASP.net](http://aspnet.4guysfromrolla.com/articles/122204-1.aspx).

## <a name="step-4-displaying-the-category-pictures-in-a-gridview"></a>Etapa 4: exibindo as imagens de categoria em um GridView

Agora que temos uma página da Web para exibir uma imagem de uma categoria em particular, podemos exibi-la usando o [controle de imagem da Web](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/image.aspx) ou um elemento HTML `<img>` apontando para `DisplayCategoryPicture.aspx?CategoryID=categoryID`. As imagens cuja URL é determinada pelos dados do banco podem ser exibidas em GridView ou DetailsView usando o ImageField. O ImageField contém as propriedades `DataImageUrlField` e `DataImageUrlFormatString` que funcionam como as propriedades `DataNavigateUrlFields` s HyperLinkField e `DataNavigateUrlFormatString`.

Vamos aumentar o `Categories` GridView em `DisplayOrDownloadData.aspx` adicionando um ImageField para mostrar cada imagem da categoria. Basta adicionar o ImageField e definir suas propriedades `DataImageUrlField` e `DataImageUrlFormatString` como `CategoryID` e `DisplayCategoryPicture.aspx?CategoryID={0}`, respectivamente. Isso criará uma coluna GridView que renderiza um elemento `<img>` cujo atributo `src` faz referência a `DisplayCategoryPicture.aspx?CategoryID={0}`, onde {0} é substituído pelo valor de `CategoryID` da linha GridView.

![Adicionar um ImageField ao GridView](displaying-binary-data-in-the-data-web-controls-cs/_static/image12.gif)

**Figura 12**: adicionar um ImageField ao GridView

Depois de adicionar o ImageField, a sintaxe declarativa s de GridView deve ser semelhante a acalme a seguir:

[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample8.aspx)]

Reserve um tempo para exibir esta página por meio de um navegador. Observe como cada registro agora inclui uma imagem para a categoria.

[![a figura s da categoria é exibida para cada linha](displaying-binary-data-in-the-data-web-controls-cs/_static/image13.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image19.png)

**Figura 13**: a figura s da categoria é exibida para cada linha ([clique para exibir a imagem em tamanho normal](displaying-binary-data-in-the-data-web-controls-cs/_static/image20.png))

## <a name="summary"></a>Resumo

Neste tutorial, examinamos como apresentar dados binários. A forma como os dados são apresentados depende do tipo de dados. Para os arquivos de folheto em PDF, oferecemos ao usuário um link exibir folheto que, quando clicado, levou o usuário diretamente para o arquivo PDF. Para a categoria s Picture, primeiro criamos uma página para recuperar e retornar os dados binários do banco de dado e, em seguida, usamos essa página para exibir cada categoria de imagem em um GridView.

Agora que vimos como exibir dados binários, estamos prontos para examinar como executar inserções, atualizações e exclusões em relação ao banco de dados com o dado binário. No próximo tutorial, veremos como associar um arquivo carregado com seu registro de banco de dados correspondente. No tutorial depois disso, veremos como atualizar dados binários existentes, bem como como excluir os dados binários quando seu registro associado é removido.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores potenciais para este tutorial foram Teresa Murphy e Dave Gardner. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](uploading-files-cs.md)
> [Próximo](including-a-file-upload-option-when-adding-a-new-record-cs.md)
