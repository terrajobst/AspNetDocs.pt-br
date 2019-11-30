---
uid: web-forms/overview/data-access/working-with-batched-data/batch-inserting-cs
title: Inserção em lote (C#) | Microsoft Docs
author: rick-anderson
description: Saiba como inserir vários registros de banco de dados em uma única operação. Na camada de interface do usuário, estendemos o GridView para permitir que o usuário insira vários n...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: cf025e08-48fc-4385-b176-8610aa7b5565
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-inserting-cs
msc.type: authoredcontent
ms.openlocfilehash: 5dc4d0b6ac9bf3aa2baa54fe9f5d4149494e47d2
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74584517"
---
# <a name="batch-inserting-c"></a>Inserção em lote (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_66_CS.zip) ou [baixar PDF](batch-inserting-cs/_static/datatutorial66cs1.pdf)

> Saiba como inserir vários registros de banco de dados em uma única operação. Na camada de interface do usuário, estendemos o GridView para permitir que o usuário insira vários novos registros. Na camada de acesso a dados, Encapsulamos as várias operações de inserção em uma transação para garantir que todas as inserções tenham sucesso ou que todas as inserções sejam revertidas.

## <a name="introduction"></a>Introdução

No tutorial de [atualização do lote](batch-updating-cs.md) , examinamos a personalização do controle GridView para apresentar uma interface em que vários registros eram editáveis. O usuário que está visitando a página pode fazer uma série de alterações e, em seguida, com um único clique de botão, executar uma atualização em lotes. Para situações em que os usuários normalmente atualizam muitos registros em uma só vez, essa interface pode economizar inúmeros cliques e opções de contexto de teclado para mouse quando comparadas aos recursos de edição por linha padrão que foram explorados primeiro na [visão geral do tutorial inserir, atualizar e excluir dados](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) .

Esse conceito também pode ser aplicado ao adicionar registros. Imagine que, na Northwind Traders, geralmente recebemos remessas de fornecedores que contêm vários produtos para uma determinada categoria. Por exemplo, podemos receber uma remessa de seis produtos de chá e café diferentes de Tokyo Traders. Se um usuário inserir os seis produtos um por vez por meio de um controle DetailsView, ele terá que escolher muitos dos mesmos valores repetidamente: eles precisarão escolher a mesma categoria (bebidas), o mesmo fornecedor (Tokyo Traders), o mesmo valor descontinuado ( False) e as mesmas unidades no valor de ordem (0). Essa entrada repetida de dados não é apenas demorada, mas está sujeita a erros.

Com um pouco de trabalho, podemos criar uma interface de inserção em lote que permite ao usuário escolher o fornecedor e a categoria uma vez, inserir uma série de nomes de produtos e preços unitários e, em seguida, clicar em um botão para adicionar os novos produtos ao banco de dados (consulte a Figura 1). À medida que cada produto é adicionado, seus campos de dados `ProductName` e `UnitPrice` são atribuídos aos valores inseridos nas caixas de caixa de entrada, enquanto seus valores `CategoryID` e `SupplierID` são atribuídos aos valores de DropDownLists na parte superior do formulário. Os valores `Discontinued` e `UnitsOnOrder` são definidos como valores embutidos em código de `false` e 0, respectivamente.

[![a interface de inserção em lote](batch-inserting-cs/_static/image2.png)](batch-inserting-cs/_static/image1.png)

**Figura 1**: a interface de inserção em lote ([clique para exibir a imagem em tamanho normal](batch-inserting-cs/_static/image3.png))

Neste tutorial, criaremos uma página que implementa a interface de inserção em lote mostrada na Figura 1. Assim como nos dois tutoriais anteriores, encapsularemos as inserções dentro do escopo de uma transação para garantir a atomicidade. Vamos começar!

## <a name="step-1-creating-the-display-interface"></a>Etapa 1: criando a interface de vídeo

Este tutorial consistirá em uma única página dividida em duas regiões: uma região de exibição e uma região de inserção. A interface de vídeo, que criaremos nesta etapa, mostra os produtos em um GridView e inclui um botão intitulado processo de remessa de produto. Quando esse botão é clicado, a interface de vídeo é substituída pela interface de inserção, que é mostrada na Figura 1. A interface de exibição retorna depois que os botões Adicionar produtos de remessa ou cancelar são clicados. Vamos criar a interface de inserção na etapa 2.

Ao criar uma página que tem duas interfaces, apenas uma delas é visível de cada vez, cada interface normalmente é colocada dentro de um [controle Web Panel](http://www.w3schools.com/aspnet/control_panel.asp), que serve como um contêiner para outros controles. Portanto, nossa página terá dois controles de painel um para cada interface.

Comece abrindo a página `BatchInsert.aspx` na pasta `BatchData` e arraste um painel da caixa de ferramentas para o designer (consulte a Figura 2). Defina a propriedade `ID` do painel como `DisplayInterface`. Ao adicionar o painel ao designer, suas propriedades `Height` e `Width` são definidas como 50px e 125px, respectivamente. Desmarque esses valores de propriedade da janela Propriedades.

[![arrastar um painel da caixa de ferramentas para o designer](batch-inserting-cs/_static/image5.png)](batch-inserting-cs/_static/image4.png)

**Figura 2**: arraste um painel da caixa de ferramentas para o designer ([clique para exibir a imagem em tamanho normal](batch-inserting-cs/_static/image6.png))

Em seguida, arraste um botão e controle GridView para o painel. Defina o botão s `ID` Propriedade como `ProcessShipment` e sua propriedade `Text` para processar a remessa do produto. Defina a Propriedade GridView s `ID` como `ProductsGrid` e, em sua marca inteligente, associe-a a um novo ObjectDataSource chamado `ProductsDataSource`. Configure o ObjectDataSource para efetuar pull de seus dados do método de `GetProducts` da classe `ProductsBLL`. Como esse GridView é usado somente para exibir dados, defina as listas suspensas nas guias UPDATE, INSERT e DELETE como (None). Clique em concluir para concluir o assistente para configurar fonte de dados.

[![exibir os dados retornados do método GetProducts da classe ProductsBLL](batch-inserting-cs/_static/image8.png)](batch-inserting-cs/_static/image7.png)

**Figura 3**: exibir os dados retornados do método de `GetProducts` da classe `ProductsBLL` ([clique para exibir a imagem em tamanho normal](batch-inserting-cs/_static/image9.png))

[![definir as listas suspensas nas guias atualizar, inserir e excluir para (nenhum)](batch-inserting-cs/_static/image11.png)](batch-inserting-cs/_static/image10.png)

**Figura 4**: definir as listas suspensas nas guias atualizar, inserir e excluir para (nenhum) ([clique para exibir a imagem em tamanho normal](batch-inserting-cs/_static/image12.png))

Depois de concluir o assistente ObjectDataSource, o Visual Studio adicionará BoundFields e um CheckBoxField para os campos de dados do produto. Remova todos os campos `ProductName`, `CategoryName`, `SupplierName`, `UnitPrice`e `Discontinued`. Sinta-se à vontade para fazer personalizações estética. Decidi formatar o campo `UnitPrice` como um valor de moeda, reordenar os campos e renomear vários dos campos `HeaderText` valores. Também configure o GridView para incluir o suporte de paginação e classificação marcando as caixas de seleção habilitar paginação e Habilitar classificação na marca inteligente s do GridView.

Depois de adicionar os controles Panel, Button, GridView e ObjectDataSource e personalizar os campos de GridView, a marcação declarativa de s de página deve ser semelhante ao seguinte:

[!code-aspx[Main](batch-inserting-cs/samples/sample1.aspx)]

Observe que a marcação do botão e do GridView aparece dentro das marcas de `<asp:Panel>` de abertura e fechamento. Como esses controles estão dentro do painel de `DisplayInterface`, podemos ocultá-los simplesmente definindo a propriedade s `Visible` do painel como `false`. A etapa 3 examina a alteração programática da propriedade `Visible` do painel em resposta a um clique de botão para mostrar uma interface enquanto oculta a outra.

Reserve um tempo para exibir nosso progresso por meio de um navegador. Como mostra a Figura 5, você verá um botão processar remessa de produto acima de um GridView que lista os produtos dez de cada vez.

[![o GridView lista os produtos e oferece recursos de classificação e paginação](batch-inserting-cs/_static/image14.png)](batch-inserting-cs/_static/image13.png)

**Figura 5**: o GridView lista os produtos e oferece recursos de classificação e paginação ([clique para exibir a imagem em tamanho normal](batch-inserting-cs/_static/image15.png))

## <a name="step-2-creating-the-inserting-interface"></a>Etapa 2: criando a interface de inserção

Com a interface de exibição concluída, estamos prontos para criar a interface de inserção. Para este tutorial, vamos criar uma interface de inserção que solicita um único fornecedor e valor de categoria e, em seguida, permite que o usuário insira até cinco nomes de produtos e valores de preço unitários. Com essa interface, o usuário pode adicionar um a cinco novos produtos que compartilham a mesma categoria e o mesmo fornecedor, mas têm preços e nomes de produtos exclusivos.

Comece arrastando um painel da caixa de ferramentas para o designer, colocando-o abaixo do painel de `DisplayInterface` existente. Defina a propriedade `ID` desse painel recém-adicionado para `InsertingInterface` e defina sua propriedade `Visible` como `false`. Vamos adicionar o código que define a propriedade `Visible` do painel de `InsertingInterface` como `true` na etapa 3. Além disso, desmarque as `Height` s de painel e `Width` valores de propriedade.

Em seguida, precisamos criar a interface de inserção que foi mostrada de volta na Figura 1. Essa interface pode ser criada por meio de uma variedade de técnicas HTML, mas usaremos uma tabela razoavelmente simples: quatro colunas de sete linhas.

> [!NOTE]
> Ao inserir a marcação para elementos de `<table>` HTML, prefiro usar a exibição de código-fonte. Embora o Visual Studio tenha ferramentas para adicionar elementos de `<table>` por meio do designer, o designer parece estar muito disposto a injetar as configurações de `style` não solicitadas na marcação. Depois de criar a marcação de `<table>`, eu geralmente retorno ao designer para adicionar os controles da Web e definir suas propriedades. Ao criar tabelas com colunas predeterminadas e linhas, prefiro usar HTML estático em vez do [controle da Web de tabela](https://msdn.microsoft.com/library/system.web.ui.webcontrols.table.aspx) , pois os controles da Web colocados em um controle da Web de tabela só podem ser acessados usando o padrão de `FindControl("controlID")`. No entanto, eu uso controles de tabela da Web para tabelas de tamanho dinâmico (aquelas cujas linhas ou colunas se baseiam em algum banco de dados ou em critérios especificados pelo usuário), já que o controle da Web da tabela pode ser construído programaticamente.

Insira a seguinte marcação dentro das marcas de `<asp:Panel>` do painel de `InsertingInterface`:

[!code-html[Main](batch-inserting-cs/samples/sample2.html)]

Essa marcação de `<table>` não inclui nenhum controle da Web ainda; vamos adicioná-las momentaneamente. Observe que cada elemento de `<tr>` contém uma configuração de classe CSS específica: `BatchInsertHeaderRow` para a linha de cabeçalho onde o fornecedor e a categoria DropDownLists vão; `BatchInsertFooterRow` para a linha de rodapé onde os botões Adicionar produtos de remessa e cancelamento vão; e alternar `BatchInsertRow` e `BatchInsertAlternatingRow` valores para as linhas que conterão os controles de caixa de texto produto e preço unitário. Eu criei classes CSS correspondentes no arquivo `Styles.css` para dar à interface de inserção uma aparência semelhante aos controles GridView e DetailsView que usamos em todos esses tutoriais. Essas classes CSS são mostradas abaixo.

[!code-css[Main](batch-inserting-cs/samples/sample3.css)]

Com essa marcação inserida, retorne ao modo de exibição de Design. Essa `<table>` deve ser mostrada como uma tabela de quatro colunas de sete linhas no designer, como ilustra a Figura 6.

[![a interface de inserção é composta de uma tabela de quatro colunas, de sete linhas](batch-inserting-cs/_static/image17.png)](batch-inserting-cs/_static/image16.png)

**Figura 6**: a interface de inserção é composta de uma tabela de quatro colunas de sete linhas ([clique para exibir a imagem em tamanho normal](batch-inserting-cs/_static/image18.png))

Agora, estamos prontos para adicionar os controles da Web à interface de inserção. Arraste dois DropDownLists da caixa de ferramentas para as células apropriadas na tabela um para o fornecedor e outro para a categoria.

Defina a propriedade do fornecedor DropDownList s `ID` como `Suppliers` e a associe a uma nova ObjectDataSource denominada `SuppliersDataSource`. Configure o novo ObjectDataSource para recuperar seus dados do método `SuppliersBLL` `GetSuppliers` classe e defina a lista suspensa da guia atualizar como (nenhum). Clique em Concluir para concluir o assistente.

[![configurar o ObjectDataSource para usar o método SuppliersBLL da classe s getsuppliers](batch-inserting-cs/_static/image20.png)](batch-inserting-cs/_static/image19.png)

**Figura 7**: configurar o ObjectDataSource para usar o método de `GetSuppliers` da classe `SuppliersBLL` ([clique para exibir a imagem em tamanho normal](batch-inserting-cs/_static/image21.png))

Faça com que o `Suppliers` DropDownList exiba o `CompanyName` campo de dados e use o campo de dados `SupplierID` como seus valores de `ListItem` s.

[![exibir o campo de dados CompanyName e usar CódigoDoFornecedor como o valor](batch-inserting-cs/_static/image23.png)](batch-inserting-cs/_static/image22.png)

**Figura 8**: exibir o campo de dados `CompanyName` e usar `SupplierID` como o valor ([clique para exibir a imagem em tamanho normal](batch-inserting-cs/_static/image24.png))

Nomeie a segunda `Categories` DropDownList e associe-a a uma nova ObjectDataSource chamada `CategoriesDataSource`. Configurar o `CategoriesDataSource` ObjectDataSource para usar o método de `GetCategories` da classe `CategoriesBLL`; Defina as listas suspensas nas guias atualizar e excluir como (nenhum) e clique em concluir para concluir o assistente. Por fim, faça com que a DropDownList exiba o campo de dados `CategoryName` e use o `CategoryID` como o valor.

Depois que essas duas DropDownLists tiverem sido adicionadas e vinculadas às ObjectDataSource configuradas adequadamente, sua tela deverá ser semelhante à figura 9.

[![a linha de cabeçalho agora contém os DropDownLists de fornecedores e categorias](batch-inserting-cs/_static/image26.png)](batch-inserting-cs/_static/image25.png)

**Figura 9**: a linha de cabeçalho agora contém as `Suppliers` e `Categories` DropDownLists ([clique para exibir a imagem em tamanho normal](batch-inserting-cs/_static/image27.png))

Agora, precisamos criar as caixas de entrada para coletar o nome e o preço de cada novo produto. Arraste um controle TextBox da caixa de ferramentas para o designer para cada um dos cinco linhas de preço e nome do produto. Defina as propriedades `ID` das caixas de Textpara `ProductName1`, `UnitPrice1`, `ProductName2`, `UnitPrice2`, `ProductName3`, `UnitPrice3`e assim por diante.

Adicione um CompareValidator depois de cada uma das caixas de Textde preço unitário, definindo a propriedade `ControlToValidate` como o `ID`apropriado. Além disso, defina a propriedade `Operator` como `GreaterThanEqual`, `ValueToCompare` como 0 e `Type` para `Currency`. Essas configurações instruem o CompareValidator a garantir que o preço, se inserido, seja um valor de moeda válido maior ou igual a zero. Defina a propriedade `Text` como \*e `ErrorMessage` como o preço deve ser maior ou igual a zero. Além disso, omita os símbolos de moeda.

> [!NOTE]
> A interface de inserção não inclui nenhum controle de RequiredFieldValidator, mesmo que o campo `ProductName` na tabela de banco de dados `Products` não permita valores de `NULL`. Isso ocorre porque queremos permitir que o usuário insira até cinco produtos. Por exemplo, se o usuário fosse fornecer o nome do produto e o preço unitário das três primeiras linhas, deixando as duas últimas linhas em branco, nós d apenas adicionaremos três novos produtos ao sistema. Como `ProductName` é necessário, no entanto, precisaremos verificar programaticamente para garantir que, se um preço unitário for inserido, um valor de nome de produto correspondente será fornecido. Resolveremos essa verificação na etapa 4.

Ao validar a entrada de s do usuário, o CompareValidator relatará dados inválidos se o valor contiver um símbolo de moeda. Adicione um $ na frente de cada uma das caixas de pagamento de preço unitário para servir como uma indicação visual que instrui o usuário a omitir o símbolo de moeda ao inserir o preço.

Por fim, adicione um controle ValidationSummary dentro do painel de `InsertingInterface`, configurações sua propriedade `ShowMessageBox` como `true` e sua propriedade `ShowSummary` como `false`. Com essas configurações, se o usuário inserir um valor de preço unitário inválido, um asterisco será exibido ao lado dos controles de caixa de texto incorretos e o ValidationSummary exibirá uma MessageBox do lado do cliente que mostra a mensagem de erro especificada anteriormente.

Neste ponto, a tela deve ser semelhante à figura 10.

[![a interface de inserção agora inclui caixas de Textpara os nomes de produtos e preços](batch-inserting-cs/_static/image29.png)](batch-inserting-cs/_static/image28.png)

**Figura 10**: a interface de inserção agora inclui caixas de Textpara os nomes dos produtos e preços ([clique para exibir a imagem em tamanho normal](batch-inserting-cs/_static/image30.png))

Em seguida, precisamos adicionar os botões Adicionar produtos da remessa e cancelar à linha do rodapé. Arraste dois controles de botão da caixa de ferramentas para o rodapé da interface de inserção, definindo os botões `ID` Propriedades para `AddProducts` e `CancelButton` e `Text` Propriedades para adicionar produtos da remessa e cancelar, respectivamente. Além disso, defina o controle de `CancelButton` s `CausesValidation` Propriedade como `false`.

Por fim, precisamos adicionar um controle rótulo da Web que exibirá mensagens de status para as duas interfaces. Por exemplo, quando um usuário adiciona com êxito uma nova remessa de produtos, desejamos retornar à interface de vídeo e exibir uma mensagem de confirmação. No entanto, se o usuário fornecer um preço para um novo produto, mas sair do nome do produto, precisaremos exibir uma mensagem de aviso, uma vez que o campo `ProductName` é necessário. Como precisamos que essa mensagem seja exibida para ambas as interfaces, coloque-a na parte superior da página fora dos painéis.

Arraste um controle de rótulo da Web da caixa de ferramentas para a parte superior da página no designer. Defina a propriedade `ID` como `StatusLabel`, desmarque a propriedade `Text` e defina as propriedades `Visible` e `EnableViewState` como `false`. Como vimos nos tutoriais anteriores, definir a propriedade `EnableViewState` como `false` nos permite alterar programaticamente os valores de Propriedade do rótulo e fazer com que eles revertam automaticamente para seus padrões no postback subsequente. Isso simplifica o código para mostrar uma mensagem de status em resposta a alguma ação do usuário que desaparece no postback subsequente. Por fim, defina o controle de `StatusLabel` s `CssClass` Propriedade como aviso, que é o nome de uma classe CSS definida em `Styles.css` que exibe o texto em uma fonte grande, em itálico, negrito e vermelho.

A Figura 11 mostra o designer do Visual Studio após o rótulo ter sido adicionado e configurado.

[![Coloque o controle StatusLabel acima dos dois controles de painel](batch-inserting-cs/_static/image32.png)](batch-inserting-cs/_static/image31.png)

**Figura 11**: Coloque o controle de `StatusLabel` acima dos dois controles de painel ([clique para exibir a imagem em tamanho normal](batch-inserting-cs/_static/image33.png))

## <a name="step-3-switching-between-the-display-and-inserting-interfaces"></a>Etapa 3: alternando entre as interfaces de exibição e de inserção

Neste ponto, concluímos a marcação para nossas interfaces de exibição e inserção, mas continuamos com duas tarefas:

- Alternando entre as interfaces de exibição e de inserção
- Adicionando os produtos na remessa ao banco de dados

Atualmente, a interface de exibição está visível, mas a interface de inserção está oculta. Isso ocorre porque a propriedade `Visible` do painel de `DisplayInterface` é definida como `true` (o valor padrão), enquanto a propriedade `Visible` do painel `InsertingInterface` é definida como `false`. Para alternar entre as duas interfaces, precisamos apenas alternar cada controle `Visible` valor da propriedade.

Queremos mover da interface de exibição para a interface de inserção quando o botão processar remessa do produto for clicado. Portanto, crie um manipulador de eventos para esse botão `Click` evento que contenha o seguinte código:

[!code-csharp[Main](batch-inserting-cs/samples/sample4.cs)]

Esse código simplesmente oculta o painel de `DisplayInterface` e mostra o painel de `InsertingInterface`.

Em seguida, crie manipuladores de eventos para os controles Adicionar produtos de remessa e cancelar botão na interface de inserção. Quando um desses botões é clicado, precisamos voltar para a interface de vídeo. Crie `Click` manipuladores de eventos para ambos os controles de botão para que eles chamem `ReturnToDisplayInterface`, um método que adicionaremos momentaneamente. Além de ocultar o painel de `InsertingInterface` e mostrar o painel de `DisplayInterface`, o método `ReturnToDisplayInterface` precisa retornar os controles da Web para o estado anterior à edição. Isso envolve definir as propriedades de `SelectedIndex` de DropDownLists como 0 e limpar as propriedades `Text` dos controles TextBox.

> [!NOTE]
> Considere o que pode acontecer se não retornarmos os controles para seu estado de pré-edição antes de retornar à interface de vídeo. Um usuário pode clicar no botão processar remessa de produto, inserir os produtos da remessa e, em seguida, clicar em Adicionar produtos da remessa. Isso adicionaria os produtos e retornará o usuário para a interface de exibição. Neste ponto, o usuário pode querer adicionar outra remessa. Ao clicar no botão processar remessa de produto, eles retornariam para a interface de inserção, mas as seleções de DropDownList e os valores de TextBox ainda seriam populados com seus valores anteriores.

[!code-csharp[Main](batch-inserting-cs/samples/sample5.cs)]

Ambos os manipuladores de eventos `Click` simplesmente chamam o método `ReturnToDisplayInterface`, embora Vamos retornar ao manipulador de eventos Add Products from `Click` de remessa na etapa 4 e adicionar código para salvar os produtos. `ReturnToDisplayInterface` começa retornando o `Suppliers` e `Categories` DropDownLists para suas primeiras opções. As duas constantes `firstControlID` e `lastControlID` marcam os valores de índice de controle inicial e final usados na nomeação das caixas de texto nome do produto e preço unitário na interface de inserção e são usados nos limites do loop `for` que define as propriedades `Text` dos controles TextBox de volta para uma cadeia de caracteres vazia. Por fim, os painéis `Visible` propriedades são redefinidos para que a interface de inserção esteja oculta e a interface de exibição mostrada.

Reserve um tempo para testar esta página em um navegador. Ao visitar a página pela primeira vez, você deverá ver a interface de exibição como foi mostrada na Figura 5. Clique no botão processar remessa de produto. A página será postback e você verá agora a interface de inserção, conforme mostrado na Figura 12. Clicar nos botões Adicionar produtos da remessa ou cancelar retornará à interface de vídeo.

> [!NOTE]
> Ao exibir a interface de inserção, Reserve um tempo para testar o CompareValidators nas caixas de caixa de entrada de preço unitário. Você deverá ver um aviso MessageBox do lado do cliente ao clicar no botão Adicionar produtos da remessa com valores de moeda ou preços inválidos com um valor menor que zero.

[![a interface de inserção é exibida depois de clicar no botão processar remessa de produto](batch-inserting-cs/_static/image35.png)](batch-inserting-cs/_static/image34.png)

**Figura 12**: a interface de inserção é exibida depois de clicar no botão processar remessa de produto ([clique para exibir a imagem em tamanho normal](batch-inserting-cs/_static/image36.png))

## <a name="step-4-adding-the-products"></a>Etapa 4: adicionando os produtos

Tudo o que resta para este tutorial é salvar os produtos no banco de dados no botão Adicionar produtos da remessa s `Click` manipulador de eventos. Isso pode ser feito criando um `ProductsDataTable` e adicionando uma instância de `ProductsRow` para cada um dos nomes de produtos fornecidos. Depois que essas `ProductsRow` s foram adicionadas, faremos uma chamada para o método `ProductsBLL` classe s `UpdateWithTransaction` passando no `ProductsDataTable`. Lembre-se de que o método `UpdateWithTransaction`, que foi criado de volta nas [modificações do banco de dados de disposição em um](wrapping-database-modifications-within-a-transaction-cs.md) tutorial de transação, passa o `ProductsDataTable` para o método `UpdateWithTransaction` s `ProductsTableAdapter`. A partir daí, uma transação ADO.NET é iniciada e o TableAdapter emite uma instrução `INSERT` ao banco de dados para cada `ProductsRow` adicionado na DataTable. Supondo que todos os produtos sejam adicionados sem erros, a transação é confirmada, caso contrário, ela é revertida.

O código do botão Adicionar produtos da remessa s `Click` manipulador de eventos também precisa executar um pouco de verificação de erro. Como não há nenhum RequiredFieldValidator usado na interface de inserção, um usuário poderia inserir um preço para um produto ao omitir seu nome. Como o nome do produto é necessário, se essa condição for desdobrada, precisaremos alertar o usuário e não prosseguir com as inserções. O código completo do manipulador de eventos `Click` segue:

[!code-csharp[Main](batch-inserting-cs/samples/sample6.cs)]

O manipulador de eventos começa garantindo que a propriedade `Page.IsValid` retorna um valor de `true`. Se ele retornar `false`, isso significa que um ou mais dos CompareValidators estão relatando dados inválidos; Nesse caso, não queremos tentar inserir os produtos inseridos, ou terminaremos com uma exceção ao tentar atribuir o valor de preço unitário inserido pelo usuário à propriedade `ProductsRow` s `UnitPrice`.

Em seguida, uma nova instância de `ProductsDataTable` é criada (`products`). Um loop de `for` é usado para iterar pelas caixas de textnome do produto e preço unitário e as propriedades de `Text` são lidas nas variáveis locais `productName` e `unitPrice`. Se o usuário tiver inserido um valor para o preço unitário, mas não para o nome do produto correspondente, o `StatusLabel` exibirá a mensagem se você fornecer um preço unitário, também deverá incluir o nome do produto e o manipulador de eventos será encerrado.

Se um nome de produto tiver sido fornecido, uma nova instância de `ProductsRow` será criada usando o método `ProductsDataTable` s `NewProductsRow`. Essa nova propriedade `ProductsRow` `ProductName` da instância é definida como a caixa de texto nome do produto atual, enquanto as propriedades `SupplierID` e `CategoryID` são atribuídas às propriedades `SelectedValue` de DropDownLists no cabeçalho inserindo interface s. Se o usuário inserir um valor para o preço do produto, ele será atribuído à propriedade `ProductsRow` s `UnitPrice` da instância; caso contrário, a propriedade será deixada sem atribuição, o que resultará em um valor `NULL` para `UnitPrice` no banco de dados. Por fim, as propriedades `Discontinued` e `UnitsOnOrder` são atribuídas aos valores embutidos `false` e 0, respectivamente.

Depois que as propriedades tiverem sido atribuídas à instância de `ProductsRow`, elas serão adicionadas à `ProductsDataTable`.

Na conclusão do loop de `for`, verificamos se algum produto foi adicionado. O usuário pode, afinal, ter clicado em Adicionar produtos da remessa antes de inserir os nomes de produtos ou preços. Se houver pelo menos um produto na `ProductsDataTable`, o método de `UpdateWithTransaction` da classe `ProductsBLL` será chamado. Em seguida, os dados são reassociados ao `ProductsGrid` GridView para que os produtos recém-adicionados apareçam na interface de vídeo. O `StatusLabel` é atualizado para exibir uma mensagem de confirmação e o `ReturnToDisplayInterface` é invocado, ocultando a interface de inserção e mostrando a interface de vídeo.

Se nenhum produto tiver sido inserido, a interface de inserção permanecerá exibida, mas a mensagem nenhum produto foi adicionado. Insira os nomes de produtos e os preços unitários nas caixas de caixa de exibição.

A Figura 13, 14 e 15 mostra as interfaces de inserção e exibição em ação. Na Figura 13, o usuário inseriu um valor de preço unitário sem um nome de produto correspondente. A Figura 14 mostra a interface de vídeo depois que três novos produtos foram adicionados com êxito, enquanto a Figura 15 mostra dois dos produtos recém-adicionados no GridView (o terceiro está na página anterior).

[![um nome de produto é necessário ao inserir um preço unitário](batch-inserting-cs/_static/image38.png)](batch-inserting-cs/_static/image37.png)

**Figura 13**: é necessário um nome de produto ao inserir um preço unitário ([clique para exibir a imagem em tamanho normal](batch-inserting-cs/_static/image39.png))

[![três novas veggies foram adicionadas para o fornecedor Mayumi s](batch-inserting-cs/_static/image41.png)](batch-inserting-cs/_static/image40.png)

**Figura 14**: três novas veggies foram adicionadas para o fornecedor Mayumi s ([clique para exibir a imagem em tamanho normal](batch-inserting-cs/_static/image42.png))

[![os novos produtos podem ser encontrados na última página do GridView](batch-inserting-cs/_static/image44.png)](batch-inserting-cs/_static/image43.png)

**Figura 15**: os novos produtos podem ser encontrados na última página do GridView ([clique para exibir a imagem em tamanho normal](batch-inserting-cs/_static/image45.png))

> [!NOTE]
> A lógica de inserção de lote usada neste tutorial encapsula as inserções dentro do escopo da transação. Para verificar isso, introduza propositadamente um erro no nível do banco de dados. Por exemplo, em vez de atribuir a nova propriedade de `CategoryID` da instância `ProductsRow` s ao valor selecionado no `Categories` DropDownList, atribua-a a um valor como `i * 5`. Aqui `i` é o indexador de loop e tem valores variando de 1 a 5. Portanto, ao adicionar dois ou mais produtos no lote, o primeiro produto terá um valor de `CategoryID` válido (5), mas os produtos subsequentes terão `CategoryID` valores que não correspondem aos valores `CategoryID` na tabela `Categories`. O efeito líquido é que, enquanto o primeiro `INSERT` terá êxito, os próximos falharão com uma violação de restrição de chave estrangeira. Como a inserção do lote é atômica, a primeira `INSERT` será revertida, retornando o banco de dados para seu estado antes do início do processo de inserção em lote.

## <a name="summary"></a>Resumo

Além dos dois tutoriais anteriores, criamos interfaces que permitem a atualização, a exclusão e a inserção de lotes de dados, todos os quais usaram o suporte de transação que adicionamos à camada de acesso a dados nas [modificações de quebra de banco de base em um tutorial de transação](wrapping-database-modifications-within-a-transaction-cs.md) . Para determinados cenários, essas interfaces do usuário de processamento em lotes melhoram muito a eficiência do usuário final, reduzindo o número de cliques, postbacks e opções de contexto de teclado para mouse, mantendo também a integridade dos dados subjacentes.

Este tutorial conclui nossa visão do trabalho com dados em lote. O próximo conjunto de tutoriais explora uma variedade de cenários de camada de acesso a dados avançados, incluindo o uso de procedimentos armazenados nos métodos TableAdapter s, a definição de configurações de nível de comando e conexão na DAL, criptografia de cadeias de conexão e muito mais!

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores potenciais para este tutorial foram Hilton Giesenow e S Ren Jacob Lauritsen. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](batch-deleting-cs.md)
> [Próximo](wrapping-database-modifications-within-a-transaction-vb.md)
