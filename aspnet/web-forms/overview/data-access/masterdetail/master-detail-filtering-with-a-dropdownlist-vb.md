---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-vb
title: Filtragem de mestre/detalhes com DropDownList (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, veremos como exibir os registros mestres em um controle DropDownList e os detalhes do item de lista selecionado em um GridView.
ms.author: riande
ms.date: 03/31/2010
ms.assetid: ea44717e-ab2e-46cd-a692-e4a9c0de194c
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 62cd296a3f36e1779666a6b5db15b0ce2488d0e4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78528717"
---
# <a name="masterdetail-filtering-with-a-dropdownlist-vb"></a>Filtragem mestre/detalhes com uma DropDownList (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_7_VB.exe) ou [baixar PDF](master-detail-filtering-with-a-dropdownlist-vb/_static/datatutorial07vb1.pdf)

> Neste tutorial, veremos como exibir os registros mestres em um controle DropDownList e os detalhes do item de lista selecionado em um GridView.

## <a name="introduction"></a>Introdução

Um tipo comum de relatório é o *relatório mestre/detalhado*, no qual o relatório começa mostrando um conjunto de registros "mestre". O usuário pode fazer uma busca detalhada em um dos registros mestres, exibindo assim os "detalhes" do registro mestre. Os relatórios mestre/de detalhes são uma opção ideal para a visualização de relações um-para-muitos, como um relatório que mostra todas as categorias e, em seguida, permite que um usuário selecione uma determinada categoria e exiba seus produtos associados. Além disso, os relatórios mestre/detalhados são úteis para exibir informações detalhadas de tabelas "amplas" (aquelas que têm muitas colunas). Por exemplo, o nível "mestre" de um relatório mestre/detalhado pode mostrar apenas o nome do produto e o preço unitário dos produtos no banco de dados, e fazer drill down em um determinado produto mostraria os campos de produto adicionais (categoria, fornecedor, quantidade por unidade e assim por diante).

Há muitas maneiras com as quais um relatório mestre/detalhado pode ser implementado. Além dos próximos três tutoriais, veremos uma variedade de relatórios mestre/detalhados. Neste tutorial, veremos como exibir os registros mestres em um [controle DropDownList](https://msdn.microsoft.com/library/dtx91y0z.aspx) e os detalhes do item de lista selecionado em um GridView. Em particular, o relatório mestre/detalhes deste tutorial listará informações de categoria e produto.

## <a name="step-1-displaying-the-categories-in-a-dropdownlist"></a>Etapa 1: exibindo as categorias em uma DropDownList

Nosso relatório mestre/detalhado listará as categorias em uma DropDownList, com os produtos do item de lista selecionados exibidos mais abaixo na página em um GridView. A primeira tarefa à frente de nós, então, é ter as categorias exibidas em uma DropDownList. Abra a página `FilterByDropDownList.aspx` na pasta `Filtering`, arraste em uma DropDownList da caixa de ferramentas para o designer da página e defina sua propriedade `ID` como `Categories`. Em seguida, clique no link escolher fonte de dados na marca inteligente da DropDownList. Isso exibirá o assistente de configuração de fonte de dados.

[![especificar a fonte de dados da DropDownList](master-detail-filtering-with-a-dropdownlist-vb/_static/image2.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image1.png)

**Figura 1**: especificar a fonte de dados da DropDownList ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-vb/_static/image3.png))

Escolha Adicionar um novo ObjectDataSource chamado `CategoriesDataSource` que invoca o método de `GetCategories()` da classe `CategoriesBLL`.

[![adicionar um novo ObjectDataSource chamado CategoriesDataSource](master-detail-filtering-with-a-dropdownlist-vb/_static/image5.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image4.png)

**Figura 2**: adicionar um novo ObjectDataSource chamado `CategoriesDataSource` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-vb/_static/image6.png))

[![optar por usar a classe CategoriesBLL](master-detail-filtering-with-a-dropdownlist-vb/_static/image8.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image7.png)

**Figura 3**: escolha usar a classe `CategoriesBLL` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-vb/_static/image9.png))

[![configurar o ObjectDataSource para usar o método GetCategories ()](master-detail-filtering-with-a-dropdownlist-vb/_static/image11.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image10.png)

**Figura 4**: configurar o ObjectDataSource para usar o método `GetCategories()` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-vb/_static/image12.png))

Depois de configurar o ObjectDataSource, ainda precisamos especificar qual campo de fonte de dados deve ser exibido em DropDownList e qual deles deve ser associado como o valor para o item de lista. Tenha o campo `CategoryName` como a exibição e `CategoryID` como o valor para cada item de lista.

[![fazer com que a DropDownList exiba o campo CategoryName e use CategoryID como o valor](master-detail-filtering-with-a-dropdownlist-vb/_static/image14.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image13.png)

**Figura 5**: fazer com que a DropDownList exiba o campo `CategoryName` e use `CategoryID` como o valor ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-vb/_static/image15.png))

Neste ponto, temos um controle DropDownList que é preenchido com os registros da tabela `Categories` (todos são realizados em cerca de seis segundos). A Figura 6 mostra nosso progresso até o momento, quando visualizado por meio de um navegador.

[![um menu suspenso lista as categorias atuais](master-detail-filtering-with-a-dropdownlist-vb/_static/image17.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image16.png)

**Figura 6**: um menu suspenso lista as categorias atuais ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-vb/_static/image18.png))

## <a name="step-2-adding-the-products-gridview"></a>Etapa 2: adicionando os produtos GridView

A última etapa em nosso relatório mestre/detalhado é listar os produtos associados à categoria selecionada. Para fazer isso, adicione um GridView à página e crie um novo ObjectDataSource chamado `productsDataSource`. Fazer com que o controle de `productsDataSource` faça a remoção dos dados do método de `GetProductsByCategoryID(categoryID)` da classe `ProductsBLL`.

[![selecionar o método GetProductsByCategoryID (categoryID)](master-detail-filtering-with-a-dropdownlist-vb/_static/image20.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image19.png)

**Figura 7**: selecione o método de `GetProductsByCategoryID(categoryID)` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-vb/_static/image21.png))

Depois de escolher esse método, o assistente ObjectDataSource nos solicitará o valor do parâmetro *`categoryID`* do método. Para usar o valor do item `categories` DropDownList selecionado, defina a origem do parâmetro como Control e a ControlID como `Categories`.

[![definir o parâmetro categoryID como o valor das categorias DropDownList](master-detail-filtering-with-a-dropdownlist-vb/_static/image23.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image22.png)

**Figura 8**: definir o parâmetro *`categoryID`* para o valor da `Categories` DropDownList ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-vb/_static/image24.png))

Reserve um tempo para conferir nosso progresso em um navegador. Ao visitar a página pela primeira vez, os produtos que pertencem à categoria selecionada (bebidas) são exibidos (como mostra a Figura 9), mas a alteração da DropDownList não atualiza os dados. Isso ocorre porque um postback deve ocorrer para que o GridView seja atualizado. Para fazer isso, temos duas opções (nenhuma delas requer escrever qualquer código):

- **Defina a**[propriedade AutoPostBack](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listcontrol.autopostback%28VS.80%29.aspx)de categorias DropDownList**como true.** (Você pode fazer isso verificando a opção Enable AutoPostBack na marca inteligente da DropDownList.) Isso irá disparar um postback sempre que o item selecionado da DropDownList for alterado pelo usuário. Portanto, quando o usuário seleciona uma nova categoria da DropDownList, um postback será acontecerdo e o GridView será atualizado com os produtos para a categoria selecionada recentemente. (Essa é a abordagem que usei neste tutorial.)
- **Adicione um controle Web de botão ao lado de DropDownList.** Defina sua propriedade `Text` como atualizar ou algo semelhante. Com essa abordagem, o usuário precisará selecionar uma nova categoria e clicar no botão. Clicar no botão causará um postback e atualizará o GridView para listar os produtos da categoria selecionada.

As figuras 9 e 10 ilustram o relatório mestre/detalhado em ação.

[![ao visitar a página pela primeira vez, os produtos de bebidas são exibidos](master-detail-filtering-with-a-dropdownlist-vb/_static/image26.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image25.png)

**Figura 9**: ao visitar a página pela primeira vez, os produtos de bebidas são exibidos ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-vb/_static/image27.png))

[![selecionar um novo produto (produzir) gera automaticamente um PostBack, atualizando o GridView](master-detail-filtering-with-a-dropdownlist-vb/_static/image29.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image28.png)

**Figura 10**: selecionar um novo produto (produzir) gera automaticamente um postback, atualizando o GridView ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-vb/_static/image30.png))

## <a name="adding-a----choose-a-category----list-item"></a>Adicionando um item de lista "--escolher uma categoria--"

Ao visitar pela primeira vez o `FilterByDropDownList.aspx` página, o primeiro item de lista da DropDownList de categorias (bebidas) é selecionado por padrão, mostrando os produtos de bebidas no GridView. Em vez de mostrar os produtos da primeira categoria, poderemos, em vez disso, ter um item DropDownList selecionado que diz algo como, "--escolher uma categoria--".

Para adicionar um novo item de lista à DropDownList, vá para a janela Propriedades e clique nas reticências na propriedade `Items`. Adicione um novo item de lista com o `Text` "--escolha uma categoria--" e o `Value` `-1`.

[![adicionar uma--escolha uma categoria--item de lista](master-detail-filtering-with-a-dropdownlist-vb/_static/image32.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image31.png)

**Figura 11**: adicionar um – escolha uma categoria – item de lista ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-vb/_static/image33.png))

Como alternativa, você pode adicionar o item de lista adicionando a seguinte marcação ao DropDownList:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-vb/samples/sample1.aspx)]

Além disso, precisamos definir a `AppendDataBoundItems` do controle DropDownList como true porque, quando as categorias estiverem vinculadas a DropDownList do ObjectDataSource, eles substituirão quaisquer itens de lista adicionados manualmente se `AppendDataBoundItems` não for verdadeiro.

![Defina a propriedade AppendDataBoundItems como true](master-detail-filtering-with-a-dropdownlist-vb/_static/image34.png)

**Figura 12**: definir a propriedade `AppendDataBoundItems` como true

Após essas alterações, ao visitar a página a opção "--escolher uma categoria--" é selecionada e nenhum produto é exibido.

[![na página inicial carregar nenhum produto é exibido](master-detail-filtering-with-a-dropdownlist-vb/_static/image36.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image35.png)

**Figura 13**: na página inicial, não é possível carregar nenhum produto ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-vb/_static/image37.png))

O motivo pelo qual nenhum produto é exibido quando o item de lista "--escolher uma categoria--" é selecionado porque seu valor é `-1` e não há produtos no banco de dados com uma `CategoryID` de `-1`. Se esse for o comportamento desejado, você terminará neste ponto! No entanto, se você quiser exibir *todas* as categorias quando o item de lista "--escolher uma categoria--" for selecionado, retorne para a classe `ProductsBLL` e personalize o método `GetProductsByCategoryID(categoryID)` para que ele invoque o método `GetProducts()` se o parâmetro de *`categoryID`* passado for menor que zero:

[!code-vb[Main](master-detail-filtering-with-a-dropdownlist-vb/samples/sample2.vb)]

A técnica usada aqui é semelhante à abordagem que usamos para exibir todos os fornecedores de volta no tutorial de [parâmetros declarativos](../basic-reporting/declarative-parameters-cs.md) , embora, para este exemplo, estamos usando um valor de `-1` para indicar que todos os registros devem ser recuperados em oposição a `Nothing`. Isso ocorre porque o parâmetro *`categoryID`* do método `GetProductsByCategoryID(categoryID)` espera como um valor inteiro passado, enquanto no tutorial de parâmetros declarativos que estávamos passando em um parâmetro de entrada de cadeia de caracteres.

A Figura 14 mostra uma captura de tela de `FilterByDropDownList.aspx` quando a opção "--escolher uma categoria--" está selecionada. Aqui, todos os produtos são exibidos por padrão e o usuário pode restringir a exibição escolhendo uma categoria específica.

[![todos os produtos agora estão listados por padrão](master-detail-filtering-with-a-dropdownlist-vb/_static/image39.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image38.png)

**Figura 14**: todos os produtos agora são listados por padrão ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-vb/_static/image40.png))

## <a name="summary"></a>Resumo

Ao exibir dados relacionados hierarquicamente, ele geralmente ajuda a apresentar os dados usando relatórios mestre/detalhados, dos quais o usuário pode começar a usar os dados da parte superior da hierarquia e fazer uma busca detalhada nos detalhes. Neste tutorial, examinamos a criação de um relatório mestre/detalhado simples que mostra os produtos de uma categoria selecionada. Isso foi feito usando uma DropDownList para a lista de categorias e um GridView para os produtos que pertencem à categoria selecionada.

No [próximo tutorial](master-detail-filtering-with-two-dropdownlists-vb.md) , vamos pegar a interface DropDownList um passo adiante, usando dois DropDownLists.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)
> [Próximo](master-detail-filtering-with-two-dropdownlists-vb.md)
