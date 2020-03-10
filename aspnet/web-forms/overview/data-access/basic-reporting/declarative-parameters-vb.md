---
uid: web-forms/overview/data-access/basic-reporting/declarative-parameters-vb
title: Parâmetros declarativos (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, Ilustraremos como usar um parâmetro definido como um valor embutido em código para selecionar os dados a serem exibidos em um controle DetailsView.
ms.author: riande
ms.date: 03/31/2010
ms.assetid: dc1234a3-114f-4c9a-8d25-50ca03cc8e8e
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/declarative-parameters-vb
msc.type: authoredcontent
ms.openlocfilehash: cdc42752fc78d18366af037a81fe4ebe5a1646ef
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78597135"
---
# <a name="declarative-parameters-vb"></a>Parâmetros declarativos (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_5_VB.exe) ou [baixar PDF](declarative-parameters-vb/_static/datatutorial05vb1.pdf)

> Neste tutorial, Ilustraremos como usar um parâmetro definido como um valor embutido em código para selecionar os dados a serem exibidos em um controle DetailsView.

## <a name="introduction"></a>Introdução

No [último tutorial](displaying-data-with-the-objectdatasource-vb.md) , examinamos a exibição de dados com os controles GridView, DetailsView e FormView associados a um controle ObjectDataSource que invocou o método `GetProducts()` da classe `ProductsBLL`. O método `GetProducts()` retorna uma DataTable fortemente tipada preenchida com todos os registros da tabela `Products` do banco de dados Northwind. A classe `ProductsBLL` contém métodos adicionais para retornar apenas subconjuntos dos produtos-`GetProductByProductID(productID)`, `GetProductsByCategoryID(categoryID)`e `GetProductsBySupplierID(supplierID)`. Esses três métodos esperam um parâmetro de entrada que indica como filtrar as informações de produto retornadas.

O ObjectDataSource pode ser usado para invocar métodos que esperam parâmetros de entrada, mas para fazer isso, é necessário especificar de onde vêm os valores para esses parâmetros. Os valores de parâmetro podem ser embutidos em código ou podem vir de uma variedade de fontes dinâmicas, incluindo: valores de QueryString, variáveis de sessão, o valor da propriedade de um controle da Web na página ou outros.

Para este tutorial, vamos começar ilustrando como usar um parâmetro definido para um valor embutido em código. Especificamente, veremos como adicionar um DetailsView à página que exibe informações sobre um produto específico, ou seja, Gumbo Mix do chefe Anton, que tem um `ProductID` de 5. Em seguida, veremos como definir o valor do parâmetro com base em um controle da Web. Em particular, usaremos uma caixa de texto para permitir que o usuário digite um país, depois do qual ele pode clicar em um botão para ver a lista de fornecedores que residem nesse país.

## <a name="using-a-hard-coded-parameter-value"></a>Usando um valor de parâmetro embutido em código

Para o primeiro exemplo, comece adicionando um controle DetailsView à página de `DeclarativeParams.aspx` na pasta `BasicReporting`. Na marca inteligente de DetailsView, selecione &lt;nova fonte de dados&gt; na lista suspensa e escolha Adicionar um ObjectDataSource.

[![adicionar um ObjectDataSource à página](declarative-parameters-vb/_static/image2.png)](declarative-parameters-vb/_static/image1.png)

**Figura 1**: adicionar um ObjectDataSource à página ([clique para exibir a imagem em tamanho normal](declarative-parameters-vb/_static/image3.png))

Isso iniciará automaticamente o assistente para escolher fonte de dados do controle ObjectDataSource. Selecione a classe `ProductsBLL` na primeira tela do assistente.

[![selecionar a classe ProductsBLL](declarative-parameters-vb/_static/image5.png)](declarative-parameters-vb/_static/image4.png)

**Figura 2**: selecione a classe `ProductsBLL` ([clique para exibir a imagem em tamanho normal](declarative-parameters-vb/_static/image6.png))

Como queremos exibir informações sobre um produto específico, queremos usar o método `GetProductByProductID(productID)`.

[![escolher o método GetProductByProductID (productID)](declarative-parameters-vb/_static/image8.png)](declarative-parameters-vb/_static/image7.png)

**Figura 3**: escolha o método de `GetProductByProductID(productID)` ([clique para exibir a imagem em tamanho normal](declarative-parameters-vb/_static/image9.png))

Como o método que selecionamos inclui um parâmetro, há mais uma tela para o assistente, em que é solicitado que você defina o valor a ser usado para o parâmetro. A lista à esquerda mostra todos os parâmetros para o método selecionado. Para `GetProductByProductID(productID)` há apenas um `productID`. À direita, podemos especificar o valor para o parâmetro selecionado. A lista suspensa origem do parâmetro enumera as várias fontes possíveis para o valor do parâmetro. Como queremos especificar um valor embutido em código de 5 para o parâmetro `productID`, deixe a origem do parâmetro como None e insira 5 na caixa de texto DefaultValue.

[![um valor de parâmetro embutido em código de 5 será usado para o parâmetro productID](declarative-parameters-vb/_static/image11.png)](declarative-parameters-vb/_static/image10.png)

**Figura 4**: um valor de parâmetro embutido em código de 5 será usado para o parâmetro `productID` ([clique para exibir a imagem em tamanho normal](declarative-parameters-vb/_static/image12.png))

Depois de concluir o assistente para configurar fonte de dados, a marcação declarativa do controle ObjectDataSource inclui um objeto `Parameter` na coleção `SelectParameters` para cada um dos parâmetros de entrada esperados pelo método definido na propriedade `SelectMethod`. Como o método que estamos usando neste exemplo espera apenas um único parâmetro de entrada, `parameterID`, há apenas uma entrada aqui. A coleção de `SelectParameters` pode conter qualquer classe derivada da classe `Parameter` no namespace `System.Web.UI.WebControls`. Para valores de parâmetros embutidos em código, a classe base `Parameter` é usada, mas para as outras opções de origem de parâmetro, uma classe derivada `Parameter` é usada; Você também pode criar seus próprios [tipos de parâmetro personalizados](http://www.leftslipper.com/ShowFaq.aspx?FaqId=11), se necessário.

[!code-aspx[Main](declarative-parameters-vb/samples/sample1.aspx)]

> [!NOTE]
> Se você estiver acompanhando seu próprio computador, a marcação declarativa que você vê neste ponto pode incluir valores para as propriedades `InsertMethod`, `UpdateMethod`e `DeleteMethod`, bem como `DeleteParameters`. O assistente escolher fonte de dados do ObjectDataSource especifica automaticamente os métodos do `ProductBLL` a serem usados para inserção, atualização e exclusão, portanto, a menos que você os tenha removido explicitamente, eles serão incluídos na marcação acima.

Ao visitar essa página, o controle da Web de dados invocará o método de `Select` do ObjectDataSource, que chamará o método de `GetProductByProductID(productID)` da classe `ProductsBLL` usando o valor embutido em código de 5 para o parâmetro de entrada `productID`. O método retornará um objeto de `ProductDataTable` fortemente tipado que contém uma única linha com informações sobre o Gumbo Mix do chefe Anton (o produto com `ProductID` 5).

[![informações sobre a combinação gumbo do chefe Anton são exibidas](declarative-parameters-vb/_static/image14.png)](declarative-parameters-vb/_static/image13.png)

**Figura 5**: informações sobre a combinação gumbo do chefe Anton são exibidas ([clique para exibir a imagem em tamanho normal](declarative-parameters-vb/_static/image15.png))

## <a name="setting-the-parameter-value-to-the-property-value-of-a-web-control"></a>Definindo o valor do parâmetro para o valor da propriedade de um controle da Web

Os valores de parâmetro de ObjectDataSource também podem ser definidos com base no valor de um controle da Web na página. Para ilustrar isso, vamos ter um GridView que liste todos os fornecedores que estão localizados em um país especificado pelo usuário. Para fazer isso, basta adicionar uma caixa de texto à página na qual o usuário pode inserir um nome de país. Defina a propriedade `ID` do controle TextBox como `CountryName`. Adicione também um controle de botão da Web.

[![adicionar uma caixa de texto à página com ID countryname](declarative-parameters-vb/_static/image17.png)](declarative-parameters-vb/_static/image16.png)

**Figura 6**: adicionar uma caixa de texto à página com `ID` `CountryName` ([clique para exibir a imagem em tamanho normal](declarative-parameters-vb/_static/image18.png))

Em seguida, adicione um GridView à página e, na marca inteligente, escolha Adicionar um novo ObjectDataSource. Como queremos exibir informações do fornecedor, selecione a classe `SuppliersBLL` na primeira tela do assistente. Na segunda tela, escolha o método `GetSuppliersByCountry(country)`.

[![escolher o método de GetSuppliersByCountry (país)](declarative-parameters-vb/_static/image20.png)](declarative-parameters-vb/_static/image19.png)

**Figura 7**: escolha o método de `GetSuppliersByCountry(country)` ([clique para exibir a imagem em tamanho normal](declarative-parameters-vb/_static/image21.png))

Como o método de `GetSuppliersByCountry(country)` tem um parâmetro de entrada, o assistente novamente inclui uma tela final para escolher o valor do parâmetro. Desta vez, defina a origem do parâmetro como Control. Isso preencherá a lista suspensa ControlID com os nomes dos controles na página; Selecione o controle de `CountryName` da lista. Quando a página for visitada pela primeira vez, o `CountryName` caixa de texto ficará em branco, portanto, nenhum resultado será retornado e nada é exibido. Se você quiser exibir alguns resultados por padrão, defina a caixa de texto DefaultValue adequadamente.

[![definir o valor do parâmetro para o valor de controle Paísname](declarative-parameters-vb/_static/image23.png)](declarative-parameters-vb/_static/image22.png)

**Figura 8**: definir o valor do parâmetro para o `CountryName` valor de controle ([clique para exibir a imagem em tamanho normal](declarative-parameters-vb/_static/image24.png))

A marcação declarativa do ObjectDataSource difere ligeiramente do nosso primeiro exemplo, usando um [ControlParameter](https://msdn.microsoft.com/library/system.web.ui.webcontrols.controlparameter.aspx) em vez do objeto de `Parameter` padrão. Um `ControlParameter` tem propriedades adicionais para especificar a `ID` do controle da Web e o valor da propriedade a ser usado para o parâmetro (`PropertyName`). O assistente para configurar fonte de dados foi inteligente o suficiente para determinar que, para uma caixa de texto, provavelmente desejaremos usar a propriedade `Text` para o valor do parâmetro. Se, no entanto, você quiser usar um valor de propriedade diferente do controle da Web, poderá alterar o valor `PropertyName` aqui ou clicando no link "mostrar propriedades avançadas" no assistente.

[!code-aspx[Main](declarative-parameters-vb/samples/sample2.aspx)]

Ao visitar a página pela primeira vez, a caixa de texto `CountryName` está vazia. O método de `Select` do ObjectDataSource ainda é invocado pelo GridView, mas um valor de `Nothing` é passado para o método `GetSuppliersByCountry(country)`. O TableAdapter converte o `Nothing` em um banco de dados `NULL` valor (`DBNull.Value`), mas a consulta usada pelo método `GetSuppliersByCountry(country)` é escrita de modo que ele não retorna valores quando um valor de `NULL` é especificado para o parâmetro `@CategoryID`. Em suma, nenhum fornecedor é retornado.

No entanto, quando o visitante entra em um país e clica no botão Mostrar fornecedores para causar um postback, o método `Select` do ObjectDataSource é reconsultado, passando o valor de `Text` do controle TextBox como o parâmetro `country`.

[![os fornecedores do Canadá são mostrados](declarative-parameters-vb/_static/image26.png)](declarative-parameters-vb/_static/image25.png)

**Figura 9**: são mostrados os fornecedores do Canadá ([clique para exibir a imagem em tamanho normal](declarative-parameters-vb/_static/image27.png))

## <a name="showing-all-suppliers-by-default"></a>Mostrando todos os fornecedores por padrão

Em vez de mostrar nenhum dos fornecedores ao exibir pela primeira vez a página, talvez você queira mostrar *todos os* fornecedores primeiro, permitindo que o usuário desupere a lista inserindo um nome de país na caixa de texto. Quando a caixa de texto está vazia, o método `GetSuppliersByCountry(country)` da classe `SuppliersBLL` é passado `Nothing` para seu parâmetro de entrada *`country`* . Esse valor de `Nothing` é então passado para o método `GetSupplierByCountry(country)` da DAL, em que ele é convertido em um banco de dados `NULL` valor para o parâmetro `@Country` na seguinte consulta:

[!code-sql[Main](declarative-parameters-vb/samples/sample3.sql)]

A expressão `Country = NULL` sempre retorna false, mesmo para registros cuja coluna `Country` tem um valor `NULL`; Portanto, nenhum registro é retornado.

Para retornar *todos os* fornecedores quando a caixa de texto Country estiver vazia, podemos aumentar o método `GetSuppliersByCountry(country)` na BLL para invocar o método `GetSuppliers()` quando seu parâmetro country for `Nothing` e chamar o método `GetSuppliersByCountry(country)` da Dal. Isso terá o efeito de retornar todos os fornecedores quando nenhum país for especificado e o subconjunto apropriado de fornecedores quando o parâmetro Country for incluído.

Altere o método `GetSuppliersByCountry(country)` na classe `SuppliersBLL` para o seguinte:

[!code-vb[Main](declarative-parameters-vb/samples/sample4.vb)]

Com essa alteração, a página `DeclarativeParams.aspx` mostra todos os fornecedores quando visitados pela primeira vez (ou sempre que a caixa de texto `CountryName` está vazia).

[![todos os fornecedores agora são mostrados por padrão](declarative-parameters-vb/_static/image29.png)](declarative-parameters-vb/_static/image28.png)

**Figura 10**: todos os fornecedores agora são mostrados por padrão ([clique para exibir a imagem em tamanho normal](declarative-parameters-vb/_static/image30.png))

## <a name="summary"></a>Resumo

Para usar métodos com parâmetros de entrada, precisamos especificar os valores para os parâmetros na coleção de `SelectParameters` do ObjectDataSource. Tipos diferentes de parâmetros permitem que o valor do parâmetro seja obtido de diferentes fontes. O tipo de parâmetro padrão usa um valor embutido em código, mas tão fácil (e sem uma linha de código) valores de parâmetro podem ser obtidos da QueryString, das variáveis de sessão, dos cookies e de valores inseridos pelo usuário dos controles da Web na página.

Os exemplos que examinamos neste tutorial ilustram como usar valores de parâmetro declarativos. No entanto, pode haver ocasiões em que precisamos usar uma origem de parâmetro que não está disponível, como a data e hora atuais, ou, se nosso site estava usando a associação, a ID de usuário do visitante. Para esses cenários, podemos definir os valores de parâmetro programaticamente antes de ObjectDataSource invocando o método do objeto subjacente. Veremos como fazer isso no [próximo tutorial](programmatically-setting-the-objectdatasource-s-parameter-values-vb.md).

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. O revisor de Lead para este tutorial foi Hilton Giesenow. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](displaying-data-with-the-objectdatasource-vb.md)
> [Próximo](programmatically-setting-the-objectdatasource-s-parameter-values-vb.md)
