---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb
title: Personalizando a interface de modificação de dados (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, veremos como personalizar a interface de um GridView editável, substituindo os controles TextBox e CheckBox padrão por alternati...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 4830d984-bd2c-4a08-bfe5-2385599f1f7d
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 85ec7bdde6b2bffbbda066b0441bbd36b7072197
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74584887"
---
# <a name="customizing-the-data-modification-interface-vb"></a>Personalizar a interface de modificação de dados (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_20_VB.exe) ou [baixar PDF](customizing-the-data-modification-interface-vb/_static/datatutorial20vb1.pdf)

> Neste tutorial, veremos como personalizar a interface de um GridView editável, substituindo os controles TextBox e CheckBox padrão por controles da Web de entrada alternativos.

## <a name="introduction"></a>Introdução

Os BoundFields e CheckBoxFields usados pelos controles GridView e DetailsView simplificam o processo de modificação de dados devido à sua capacidade de processar interfaces somente leitura, editáveis e insertáveis. Essas interfaces podem ser renderizadas sem a necessidade de adicionar qualquer código ou marcação declarativa adicional. No entanto, as interfaces BoundField e CheckBoxField não têm a personalização geralmente necessária em cenários do mundo real. Para personalizar a interface editável ou insertável em um GridView ou DetailsView, precisamos usar um TemplateField.

No [tutorial anterior](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) , vimos como personalizar as interfaces de modificação de dados adicionando controles da Web de validação. Neste tutorial, veremos como personalizar os controles da Web de coleta de dados reais, substituindo os controles de caixa de texto e caixa de seleção padrão de BoundField e CheckBoxField por controles da Web de entrada alternativos. Em particular, criaremos um GridView editável que permite a atualização do nome, da categoria, do fornecedor e do status descontinuado do produto. Ao editar uma linha específica, os campos de categoria e fornecedor serão renderizados como DropDownLists, contendo o conjunto de categorias e fornecedores disponíveis para sua escolha. Além disso, substituiremos a caixa de seleção padrão de CheckBoxField por um controle RadioButtonList que oferece duas opções: "active" e "Discontinued".

[![a interface de edição do GridView inclui DropDownLists e RadioButtons](customizing-the-data-modification-interface-vb/_static/image2.png)](customizing-the-data-modification-interface-vb/_static/image1.png)

**Figura 1**: a interface de edição do GridView inclui DropDownLists e RadioButtons ([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image3.png))

## <a name="step-1-creating-the-appropriateupdateproductoverload"></a>Etapa 1: criando a sobrecarga de`UpdateProduct`apropriada

Neste tutorial, criaremos um GridView editável que permite a edição do nome, da categoria, do fornecedor e do status descontinuados do produto. Portanto, precisamos de uma sobrecarga de `UpdateProduct` que aceite cinco parâmetros de entrada esses quatro valores de produto mais o `ProductID`. Assim como em nossas sobrecargas anteriores, essa será:

1. Recuperar as informações do produto do banco de dados para o `ProductID`especificado,
2. Atualize os campos `ProductName`, `CategoryID`, `SupplierID`e `Discontinued` e
3. Envie a solicitação de atualização para a DAL por meio do método `Update()` do TableAdapter.

Para resumir, para essa sobrecarga específica, omiti a verificação de regra de negócio que garante que um produto marcado como descontinuado não seja o único produto oferecido por seu fornecedor. Fique à vontade para adicioná-lo se preferir, ou, idealmente, refatorar a lógica para um método separado.

O código a seguir mostra a nova sobrecarga de `UpdateProduct` na classe `ProductsBLL`:

[!code-vb[Main](customizing-the-data-modification-interface-vb/samples/sample1.vb)]

## <a name="step-2-crafting-the-editable-gridview"></a>Etapa 2: criando o GridView editável

Com a sobrecarga de `UpdateProduct` adicionada, estamos prontos para criar o GridView editável. Abra a página `CustomizedUI.aspx` na pasta `EditInsertDelete` e adicione um controle GridView ao designer. Em seguida, crie um novo ObjectDataSource na marca inteligente do GridView. Configure o ObjectDataSource para recuperar informações do produto por meio do método `GetProducts()` da classe `ProductBLL` e para atualizar os dados do produto usando a sobrecarga de `UpdateProduct` que acabamos de criar. Nas guias inserir e excluir, selecione (nenhum) nas listas suspensas.

[![configurar o ObjectDataSource para usar a sobrecarga UpdateProduct recém-criada](customizing-the-data-modification-interface-vb/_static/image5.png)](customizing-the-data-modification-interface-vb/_static/image4.png)

**Figura 2**: configurar o ObjectDataSource para usar a sobrecarga de `UpdateProduct` recém-criada ([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image6.png))

Como vimos durante os tutoriais de modificação de dados, a sintaxe declarativa para o ObjectDataSource criado pelo Visual Studio atribui a propriedade `OldValuesParameterFormatString` a `original_{0}`. Isso, é claro, não funcionará com nossa camada de lógica de negócios, já que nossos métodos não esperam que o valor de `ProductID` original seja passado. Portanto, como fizemos nos tutoriais anteriores, Reserve um tempo para remover essa atribuição de propriedade da sintaxe declarativa ou, em vez disso, definir o valor dessa propriedade como `{0}`.

Após essa alteração, a marcação declarativa do ObjectDataSource deve ser parecida com a seguinte:

[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample2.aspx)]

Observe que a propriedade `OldValuesParameterFormatString` foi removida e que há um `Parameter` na coleção de `UpdateParameters` para cada um dos parâmetros de entrada esperados por nossa sobrecarga de `UpdateProduct`.

Enquanto o ObjectDataSource está configurado para atualizar apenas um subconjunto de valores de produto, o GridView atualmente mostra *todos* os campos Product. Reserve um tempo para editar o GridView para que:

- Ele inclui apenas o `ProductName`, `SupplierName`, o `CategoryName` BoundFields e a `Discontinued` CheckBoxField
- Os campos `CategoryName` e `SupplierName` a serem exibidos antes (à esquerda) do `Discontinued` CheckBoxField
- A propriedade `CategoryName` e `SupplierName` BoundFields ' `HeaderText` está definida como "category" e "Supplier", respectivamente
- O suporte à edição está habilitado (marque a caixa de seleção Habilitar edição na marca inteligente do GridView)

Após essas alterações, o designer será semelhante à figura 3, com a sintaxe declarativa do GridView mostrada abaixo.

[![remover os campos desnecessários do GridView](customizing-the-data-modification-interface-vb/_static/image8.png)](customizing-the-data-modification-interface-vb/_static/image7.png)

**Figura 3**: remover os campos desnecessários do GridView ([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image9.png))

[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample3.aspx)]

Neste ponto, o comportamento somente leitura do GridView é concluído. Ao exibir os dados, cada produto é renderizado como uma linha no GridView, mostrando o nome do produto, a categoria, o fornecedor e o status descontinuado.

[![a interface somente leitura do GridView estiver concluída](customizing-the-data-modification-interface-vb/_static/image11.png)](customizing-the-data-modification-interface-vb/_static/image10.png)

**Figura 4**: a interface somente leitura do GridView é concluída ([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image12.png))

> [!NOTE]
> Conforme discutido em [uma visão geral de inserção, atualização e exclusão do tutorial de dados](an-overview-of-inserting-updating-and-deleting-data-cs.md), é extremamente importante que o estado de exibição de GridView s seja habilitado (o comportamento padrão). Se você definir a Propriedade GridView s `EnableViewState` como `false`, correrá o risco de ter usuários simultâneos excluindo ou editando os registros de forma involuntária. Consulte [AVISO: problema de simultaneidade com ASP.NET 2,0 GridViews/DetailsView/formviews que dão suporte à edição e/ou exclusão e cujo estado de exibição está desabilitado](http://scottonwriting.net/sowblog/posts/10054.aspx) para obter mais informações.

## <a name="step-3-using-a-dropdownlist-for-the-category-and-supplier-editing-interfaces"></a>Etapa 3: usando uma DropDownList para as interfaces de edição de categoria e fornecedor

Lembre-se de que o objeto `ProductsRow` contém as propriedades `CategoryID`, `CategoryName`, `SupplierID`e `SupplierName`, que fornecem os valores reais de ID de chave estrangeira na tabela `Products` banco de dados e os valores `Name` correspondentes nas tabelas `Categories` e `Suppliers`. Os `CategoryID` e `SupplierID` do `ProductRow`podem ser lidos e gravados, enquanto as propriedades `CategoryName` e `SupplierName` são marcadas como somente leitura.

Devido ao status somente leitura das propriedades `CategoryName` e `SupplierName`, os BoundFields correspondentes tiveram sua propriedade `ReadOnly` definida como `True`, impedindo que esses valores sejam modificados quando uma linha for editada. Embora possamos definir a propriedade `ReadOnly` como `False`, renderizando o `CategoryName` e `SupplierName` os BoundFields como caixas de textdurante a edição, tal abordagem resultará em uma exceção quando o usuário tentar atualizar o produto, já que não há sobrecarga de `UpdateProduct` que retenha as entradas `CategoryName` e `SupplierName`. Na verdade, não queremos criar tal sobrecarga por dois motivos:

- A tabela `Products` não tem `SupplierName` ou `CategoryName` campos, mas `SupplierID` e `CategoryID`. Portanto, queremos que nosso método transmita esses valores de ID específicos, não seus valores de tabelas de pesquisa.
- Exigir que o usuário digite o nome do fornecedor ou da categoria é menor que o ideal, pois exige que o usuário conheça as categorias e os fornecedores disponíveis e suas grafias corretas.

Os campos de fornecedor e categoria devem exibir os nomes de categoria e fornecedores no modo somente leitura (como faz agora) e uma lista suspensa de opções aplicáveis ao serem editadas. Usando uma lista suspensa, o usuário final pode ver rapidamente quais categorias e fornecedores estão disponíveis para escolher entre e pode fazer sua seleção com mais facilidade.

Para fornecer esse comportamento, precisamos converter o `SupplierName` e `CategoryName` BoundFields em TemplateFields, cuja `ItemTemplate` emite os valores `SupplierName` e `CategoryName` e cujos `EditItemTemplate` usa um controle DropDownList para listar as categorias e os fornecedores disponíveis.

## <a name="adding-thecategoriesandsuppliersdropdownlists"></a>Adicionando os DropDownLists`Categories`e`Suppliers`

Comece convertendo o `SupplierName` e `CategoryName` BoundFields em TemplateFields por: clicando no link Editar colunas da marca inteligente do GridView; selecionando o BoundField na lista na parte inferior esquerda; e clicando no link "converter este campo em um TemplateField". O processo de conversão criará um TemplateField com um `ItemTemplate` e um `EditItemTemplate`, conforme mostrado na sintaxe declarativa abaixo:

[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample4.aspx)]

Como o BoundField foi marcado como somente leitura, tanto o `ItemTemplate` quanto o `EditItemTemplate` contêm um controle de rótulo da Web cuja propriedade `Text` está associada ao campo de dados aplicável (`CategoryName`, na sintaxe acima). Precisamos modificar o `EditItemTemplate`, substituindo o controle da Web Label por um controle DropDownList.

Como vimos nos tutoriais anteriores, o modelo pode ser editado por meio do designer ou diretamente da sintaxe declarativa. Para editá-lo por meio do designer, clique no link editar modelos na marca inteligente do GridView e escolha trabalhar com o `EditItemTemplate`do campo de categoria. Remova o controle da Web Label e substitua-o por um controle DropDownList, definindo a propriedade ID da DropDownList como `Categories`.

[![remover o caixa emails e adicionar uma DropDownList ao EditItemTemplate](customizing-the-data-modification-interface-vb/_static/image14.png)](customizing-the-data-modification-interface-vb/_static/image13.png)

**Figura 5**: remover o caixa emails e adicionar um DropDownList à `EditItemTemplate` ([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image15.png))

Em seguida, precisamos preencher a DropDownList com as categorias disponíveis. Clique no link escolher fonte de dados na marca inteligente da DropDownList e opte por criar um novo ObjectDataSource chamado `CategoriesDataSource`.

[![criar um novo controle ObjectDataSource chamado CategoriesDataSource](customizing-the-data-modification-interface-vb/_static/image17.png)](customizing-the-data-modification-interface-vb/_static/image16.png)

**Figura 6**: criar um novo controle ObjectDataSource chamado `CategoriesDataSource` ([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image18.png))

Para que esse ObjectDataSource retorne todas as categorias, associe-o ao método `GetCategories()` da classe `CategoriesBLL`.

[![associar o ObjectDataSource ao método GetCategories () do CategoriesBLL](customizing-the-data-modification-interface-vb/_static/image20.png)](customizing-the-data-modification-interface-vb/_static/image19.png)

**Figura 7**: associar o ObjectDataSource ao método `GetCategories()` do `CategoriesBLL`([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image21.png))

Por fim, defina as configurações da DropDownList de modo que o campo `CategoryName` seja exibido em cada `ListItem` DropDownList com o campo `CategoryID` usado como o valor.

[![ter o campo CategoryName exibido e o CategoryID usado como o valor](customizing-the-data-modification-interface-vb/_static/image23.png)](customizing-the-data-modification-interface-vb/_static/image22.png)

**Figura 8**: ter o campo `CategoryName` exibido e o `CategoryID` usado como o valor ([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image24.png))

Depois de fazer essas alterações, a marcação declarativa para o `EditItemTemplate` no `CategoryName` TemplateField incluirá um DropDownList e um ObjectDataSource:

[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample5.aspx)]

> [!NOTE]
> O DropDownList no `EditItemTemplate` deve ter seu estado de exibição habilitado. Em breve, adicionaremos a sintaxe de DataBinding à sintaxe declarativa da DropDownList e os comandos de vinculação de dados como `Eval()` e `Bind()` só podem aparecer em controles cujo estado de exibição esteja habilitado.

Repita essas etapas para adicionar uma DropDownList chamada `Suppliers` à `EditItemTemplate`do `SupplierName` TemplateField. Isso envolverá a adição de uma DropDownList ao `EditItemTemplate` e a criação de outro ObjectDataSource. No entanto, o ObjectDataSource da `Suppliers` DropDownList deve ser configurado para invocar o método de `GetSuppliers()` da classe `SuppliersBLL`. Além disso, configure o `Suppliers` DropDownList para exibir o campo `CompanyName` e use o campo `SupplierID` como o valor para seus `ListItem` s.

Depois de adicionar os DropDownLists às duas `EditItemTemplate` s, carregue a página em um navegador e clique no botão Editar para o produto Cajun de época do chefe Anton. Como mostra a Figura 9, as colunas categoria e fornecedor do produto são renderizadas como listas suspensas contendo as categorias e os fornecedores disponíveis dos quais escolher. No entanto, observe que os *primeiros* itens em ambas as listas suspensas são selecionados por padrão (bebidas para a categoria e exóticas liquids como o fornecedor), mesmo que a época de Cajun da chefe Anton seja uma condiment fornecida por novos Orleans de Cajun.

[![o primeiro item nas listas suspensas é selecionado por padrão](customizing-the-data-modification-interface-vb/_static/image26.png)](customizing-the-data-modification-interface-vb/_static/image25.png)

**Figura 9**: o primeiro item nas listas suspensas é selecionado por padrão ([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image27.png))

Além disso, se você clicar em atualizar, descobrirá que os valores de `CategoryID` e `SupplierID` do produto estão definidos como `NULL`. Ambos os comportamentos indesejados são causados porque os DropDownLists no `EditItemTemplate` s não estão associados a nenhum campo de dados dos dados do produto subjacentes.

## <a name="binding-the-dropdownlists-to-thecategoryidandsupplieriddata-fields"></a>Ligando os DropDownLists aos campos de dados`CategoryID`e`SupplierID`

Para fazer com que as listas suspensas de categoria e fornecedor do produto editado sejam definidas com os valores apropriados e que tenham esses valores enviados de volta ao método de `UpdateProduct` da BLL ao clicar em atualizar, precisamos associar as propriedades de `SelectedValue` de DropDownLists aos `CategoryID` e `SupplierID` campos de dados usando DataBinding bidirecional. Para fazer isso com a `Categories` DropDownList, você pode adicionar `SelectedValue='<%# Bind("CategoryID") %>'` diretamente à sintaxe declarativa.

Como alternativa, você pode definir os DataBindings da DropDownList editando o modelo por meio do designer e clicando no link editar DataBindings da marca inteligente da DropDownList. Em seguida, indique que a propriedade `SelectedValue` deve ser associada ao campo `CategoryID` usando a ligação de dados bidirecional (consulte a Figura 10). Repita o processo declarativo ou designer para associar o `SupplierID` campo de dados ao `Suppliers` DropDownList.

[![associar a CategoryID à propriedade SelectedValue da DropDownList usando a associação de dados bidirecional](customizing-the-data-modification-interface-vb/_static/image29.png)](customizing-the-data-modification-interface-vb/_static/image28.png)

**Figura 10**: associar o `CategoryID` à propriedade `SelectedValue` da DropDownList usando a associação de dados bidirecional ([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image30.png))

Depois que as associações forem aplicadas às propriedades de `SelectedValue` dos dois DropDownLists, as colunas de categoria e fornecedor do produto editado usarão como padrão os valores do produto atual. Ao clicar em atualizar, os valores de `CategoryID` e `SupplierID` do item de lista suspensa selecionado serão passados para o método `UpdateProduct`. A Figura 11 mostra o tutorial depois que as instruções DataBinding foram adicionadas; Observe como os itens de lista suspensa selecionados para a época Cajun do chefe Anton são corretamente condiment e novos Orleans Cajun.

[![os valores de categoria e fornecedor atuais do produto editado são selecionados por padrão](customizing-the-data-modification-interface-vb/_static/image32.png)](customizing-the-data-modification-interface-vb/_static/image31.png)

**Figura 11**: os valores atuais de categoria e fornecedor do produto editado são selecionados por padrão ([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image33.png))

## <a name="handlingnullvalues"></a>Manipulando valores de`NULL`

As colunas `CategoryID` e `SupplierID` na tabela `Products` podem ser `NULL`, mas os DropDownLists na `EditItemTemplate` s não incluem um item de lista para representar um valor de `NULL`. Isso tem duas consequências:

- O usuário não pode usar nossa interface para alterar a categoria ou o fornecedor de um produto de um valor não`NULL` para um `NULL` um
- Se um produto tiver um `NULL` `CategoryID` ou `SupplierID`, clicar no botão Editar resultará em uma exceção. Isso ocorre porque o valor de `NULL` retornado por `CategoryID` (ou `SupplierID`) na instrução `Bind()` não é mapeado para um valor em DropDownList (DropDownList gera uma exceção quando sua propriedade `SelectedValue` é definida como um valor que *não* está em sua coleção de itens de lista).

Para dar suporte a `NULL` `CategoryID` e valores de `SupplierID`, precisamos adicionar outro `ListItem` a cada DropDownList para representar o valor de `NULL`. Na [filtragem mestre/de detalhes com um tutorial DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) , vimos como adicionar um `ListItem` adicional a uma DropDownList de ligação de ligações, que envolvia a definição da propriedade `AppendDataBoundItems` da DropDownList como `True` e a adição manual da `ListItem`adicional. No entanto, no tutorial anterior, adicionamos um `ListItem` com uma `Value` de `-1`. No entanto, a lógica de DataBinding em ASP.NET converterá automaticamente uma cadeia de caracteres em branco em um valor `NULL` e vice-versa. Portanto, para este tutorial, queremos que a `Value` do `ListItem`seja uma cadeia de caracteres vazia.

Comece definindo a propriedade de `AppendDataBoundItems` de DropDownLists como `True`. Em seguida, adicione o `NULL` `ListItem` adicionando o seguinte elemento `<asp:ListItem>` a cada DropDownList para que a marcação declarativa se pareça com:

[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample6.aspx)]

Optei por usar "(None)" como o valor de texto para esse `ListItem`, mas você pode alterá-lo para também uma cadeia de caracteres em branco, se desejar.

> [!NOTE]
> Como vimos na filtragem de *mestre/detalhes com um tutorial DropDownList* , `ListItem` s pode ser adicionado a uma DropDownList por meio do Designer clicando na propriedade `Items` da dropdownlist no janela Propriedades (que exibirá o `ListItem` editor de coleção). No entanto, certifique-se de adicionar o `ListItem` de `NULL` para este tutorial por meio da sintaxe declarativa. Se você usar o `ListItem` editor de coleção, a sintaxe declarativa gerada omitirá a configuração de `Value` totalmente quando uma cadeia de caracteres em branco for atribuída, criando marcação declarativa como: `<asp:ListItem>(None)</asp:ListItem>`. Embora isso possa parecer inofensivo, o valor ausente faz com que o DropDownList use o valor da propriedade `Text` em seu lugar. Isso significa que, se essa `NULL` `ListItem` for selecionada, o valor "(None)" será tentado para ser atribuído ao `CategoryID`, o que resultará em uma exceção. Ao definir explicitamente `Value=""`, um valor de `NULL` será atribuído a `CategoryID` quando a `ListItem` `NULL` for selecionada.

Repita essas etapas para a DropDownList de fornecedores.

Com essa `ListItem`adicional, a interface de edição agora pode atribuir `NULL` valores aos campos `CategoryID` e `SupplierID` de um produto, como mostra a Figura 12.

[![escolher (nenhum) para atribuir um valor nulo para a categoria ou fornecedor de um produto](customizing-the-data-modification-interface-vb/_static/image35.png)](customizing-the-data-modification-interface-vb/_static/image34.png)

**Figura 12**: escolha (nenhum) para atribuir um valor de `NULL` para a categoria ou o fornecedor de um produto ([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image36.png))

## <a name="step-4-using-radiobuttons-for-the-discontinued-status"></a>Etapa 4: usando os botões de opção para o status descontinuado

Atualmente, o campo de dados de `Discontinued` produtos é expresso usando um CheckBoxField, que renderiza uma caixa de seleção desabilitada para as linhas somente leitura e uma caixa de seleção habilitada para a linha que está sendo editada. Embora essa interface do usuário seja geralmente adequada, podemos personalizá-la se necessário usando um TemplateField. Para este tutorial, vamos alterar o CheckBoxField em um TemplateField que usa um controle RadioButtonList com duas opções "active" e "Discontinued", da qual o usuário pode especificar o valor de `Discontinued` do produto.

Comece convertendo o `Discontinued` CheckBoxField em um TemplateField, que criará um TemplateField com um `ItemTemplate` e `EditItemTemplate`. Ambos os modelos incluem uma caixa de seleção com sua propriedade `Checked` associada ao campo de dados `Discontinued`, a única diferença entre os dois sendo que a propriedade `Enabled` da caixa de `ItemTemplate`é definida como `False`.

Substitua a caixa de seleção no `ItemTemplate` e `EditItemTemplate` por um controle RadioButtonList, definindo as propriedades de `ID` RadioButtonLists como `DiscontinuedChoice`. Em seguida, indique que as RadioButtonLists devem conter dois botões de opção, um rotulado "ativo" com um valor de "falso" e um rotulado "descontinuado" com um valor de "verdadeiro". Para fazer isso, você pode inserir os elementos de `<asp:ListItem>` diretamente por meio da sintaxe declarativa ou usar o editor de coleção `ListItem` do designer. A Figura 13 mostra o editor de coleção `ListItem` após a especificação das duas opções de botão de opção.

[![adicionar opções ativas e descontinuadas à RadioButtonList](customizing-the-data-modification-interface-vb/_static/image38.png)](customizing-the-data-modification-interface-vb/_static/image37.png)

**Figura 13**: adicionar opções ativas e descontinuadas à RadioButtonList ([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image39.png))

Como a RadioButtonList no `ItemTemplate` não deve ser editável, defina sua propriedade `Enabled` como `False`, deixando a propriedade `Enabled` como `True` (o padrão) para o RadioButtonList na `EditItemTemplate`. Isso fará com que os botões de opção na linha não editada sejam somente leitura, mas permitirão que o usuário altere os valores de RadioButton para a linha editada.

Ainda precisamos atribuir as propriedades de `SelectedValue` de controles RadioButtonList para que o botão de opção apropriado seja selecionado com base no campo de dados `Discontinued` do produto. Assim como com os DropDownLists examinados anteriormente neste tutorial, essa sintaxe de DataBinding pode ser adicionada diretamente à marcação declarativa ou por meio do link editar DataBindings nas marcas inteligentes de RadioButtonLists.

Depois de adicionar os dois RadioButtonLists e configurá-los, a marcação declarativa do `Discontinued` TemplateField deve ser semelhante a:

[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample7.aspx)]

Com essas alterações, a coluna `Discontinued` foi transformada de uma lista de caixas de seleção em uma lista de pares de botão de opção (consulte a Figura 14). Ao editar um produto, o botão de opção apropriado é selecionado e o status descontinuado do produto pode ser atualizado selecionando o outro botão de opção e clicando em atualizar.

[![as caixas de seleção descontinuadas foram substituídas por pares de botão de opção](customizing-the-data-modification-interface-vb/_static/image41.png)](customizing-the-data-modification-interface-vb/_static/image40.png)

**Figura 14**: as caixas de seleção descontinuadas foram substituídas por pares de botão de opção ([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image42.png))

> [!NOTE]
> Como a coluna `Discontinued` no banco de dados `Products` não pode ter valores `NULL`, não precisamos nos preocupar em capturar informações de `NULL` na interface. No entanto, se `Discontinued` coluna puder conter `NULL` valores, gostaríamos de adicionar um terceiro botão de opção à lista com seu `Value` definido como uma cadeia de caracteres vazia (`Value=""`), assim como a categoria e os DropDownLists do fornecedor.

## <a name="summary"></a>Resumo

Embora o BoundField e o CheckBoxField processem automaticamente as interfaces somente leitura, edição e inserção, eles não têm a capacidade de personalização. No entanto, com frequência, precisaremos personalizar a interface de edição ou inserção, talvez adicionando controles de validação (como vimos no tutorial anterior) ou Personalizando a interface do usuário da coleta de dados (como vimos neste tutorial). A personalização da interface com um TemplateField pode ser resumida nas seguintes etapas:

1. Adicionar um TemplateField ou converter um BoundField ou CheckBoxField existente em um TemplateField
2. Aumente a interface conforme necessário
3. Associar os campos de dados apropriados aos controles da Web adicionados recentemente usando a associação de dados bidirecional

Além de usar os controles da Web ASP.NET internos, você também pode personalizar os modelos de um TemplateField com controles de servidor personalizados e compilados e controles de usuário.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)
> [Próximo](implementing-optimistic-concurrency-vb.md)
