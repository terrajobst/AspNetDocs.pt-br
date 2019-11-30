---
uid: web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-cs
title: Atualizando e excluindo dados binários existentes (C#) | Microsoft Docs
author: rick-anderson
description: Nos tutoriais anteriores, vimos como o controle GridView torna simples a edição e a exclusão de dados de texto. Neste tutorial, vemos como o controle GridView também faz...
ms.author: riande
ms.date: 03/27/2007
ms.assetid: 35798f21-1606-434b-83f8-30166906ef49
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 3e37381ee48fcda8e0e10374aa7a6ae53c3cc77c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74587413"
---
# <a name="updating-and-deleting-existing-binary-data-c"></a>Atualizar e excluir dados binários existentes (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_57_CS.exe) ou [baixar PDF](updating-and-deleting-existing-binary-data-cs/_static/datatutorial57cs1.pdf)

> Nos tutoriais anteriores, vimos como o controle GridView torna simples a edição e a exclusão de dados de texto. Neste tutorial, vemos como o controle GridView também possibilita a edição e a exclusão de dados binários, independentemente de os dados binários serem salvos ou armazenados no sistema de arquivos.

## <a name="introduction"></a>Introdução

Nos últimos três tutoriais, adicionamos um pouco de funcionalidade para trabalhar com dados binários. Começamos adicionando uma coluna `BrochurePath` à tabela `Categories` e atualizei a arquitetura de acordo. Também adicionamos métodos de camada de acesso a dados e de camadas de lógica de negócios para trabalhar com as categorias de `Picture` coluna de tabela existente, que contém o conteúdo binário s de um arquivo de imagem. Criamos páginas da Web para apresentar os dados binários em um GridView um link de download para o folheto, com a categoria s Picture mostrada em um elemento `<img>` e adicionamos um DetailsView para permitir que os usuários adicionem uma nova categoria e carreguem seus dados de folheto e imagem.

Tudo o que resta para ser implementado é a capacidade de editar e excluir categorias existentes, que serão realizadas neste tutorial usando os recursos internos de edição e exclusão do GridView. Ao editar uma categoria, o usuário poderá, opcionalmente, carregar uma nova imagem ou fazer com que a categoria continue a usar a existente. Para o folheto, eles podem optar por usar o folheto existente, carregar um novo folheto ou indicar que a categoria não tem mais um folheto associado a ele. Vamos começar!

## <a name="step-1-updating-the-data-access-layer"></a>Etapa 1: atualizando a camada de acesso a dados

A DAL tem métodos de `Insert`, `Update`e `Delete` gerados automaticamente, mas esses métodos foram gerados com base na consulta principal de `CategoriesTableAdapter` s, que não inclui a coluna de `Picture`. Portanto, os métodos `Insert` e `Update` não incluem parâmetros para especificar os dados binários da categoria s Picture. Como fizemos no [tutorial anterior](including-a-file-upload-option-when-adding-a-new-record-cs.md), precisamos criar um novo método TableAdapter para atualizar a tabela de `Categories` ao especificar dados binários.

Abra o DataSet tipado e, no designer, clique com o botão direito do mouse no cabeçalho `CategoriesTableAdapter` s e escolha Adicionar consulta no menu de contexto para iniciar o assistente de configuração de consulta do TableAdapter. Esse assistente é iniciado ao nos perguntar como a consulta do TableAdapter deve acessar o banco de dados. Escolha usar instruções SQL e clique em Avançar. A próxima etapa solicita o tipo de consulta a ser gerada. Como recriamos uma consulta para adicionar um novo registro à tabela `Categories`, escolha Atualizar e clique em Avançar.

[![selecionar a opção de atualização](updating-and-deleting-existing-binary-data-cs/_static/image1.gif)](updating-and-deleting-existing-binary-data-cs/_static/image1.png)

**Figura 1**: selecione a opção de atualização ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image2.png))

Agora, precisamos especificar a instrução `UPDATE` SQL. O assistente sugere automaticamente uma instrução `UPDATE` correspondente à consulta principal do TableAdapter s (uma que atualiza os valores `CategoryName`, `Description`e `BrochurePath`). Altere a instrução para que a coluna `Picture` seja incluída junto com um parâmetro `@Picture`, desta forma:

[!code-sql[Main](updating-and-deleting-existing-binary-data-cs/samples/sample1.sql)]

A tela final do assistente nos pede para nomear o novo método TableAdapter. Insira `UpdateWithPicture` e clique em concluir.

[![nomear o novo método TableAdapter UpdateWithPicture](updating-and-deleting-existing-binary-data-cs/_static/image2.gif)](updating-and-deleting-existing-binary-data-cs/_static/image3.png)

**Figura 2**: nomeie o novo método TableAdapter `UpdateWithPicture` ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image4.png))

## <a name="step-2-adding-the-business-logic-layer-methods"></a>Etapa 2: adicionando os métodos de camada de lógica de negócios

Além de atualizar a DAL, precisamos atualizar a BLL para incluir métodos de atualização e exclusão de uma categoria. Estes são os métodos que serão invocados da camada de apresentação.

Para excluir uma categoria, podemos usar o método `Delete` `CategoriesTableAdapter` s gerado automaticamente. Adicione o seguinte método à classe `CategoriesBLL`:

[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample2.cs)]

Para este tutorial, vamos criar dois métodos para atualizar uma categoria-um que espera os dados da imagem binária e invoca o método `UpdateWithPicture` que acabamos de adicionar ao `CategoriesTableAdapter` e outro que aceita apenas os valores `CategoryName`, `Description`e `BrochurePath` e usa a instrução `CategoriesTableAdapter` gerada automaticamente pela classe `Update`. A lógica por trás do uso de dois métodos é que, em algumas circunstâncias, um usuário pode querer atualizar a categoria s Picture junto com seus outros campos, caso em que o usuário precisará carregar a nova imagem. Os dados binários da imagem carregada podem ser usados na instrução `UPDATE`. Em outros casos, o usuário só pode estar interessado em atualizar, digamos, o nome e a descrição. Mas se a instrução `UPDATE` espera os dados binários para a coluna de `Picture` também, então, d também precisamos fornecer essas informações. Isso exigiria uma viagem extra para o banco de dados para retornar a imagem para o registro que está sendo editado. Portanto, queremos dois métodos de `UPDATE`. A camada de lógica de negócios determinará qual deles usar com base no fato de os dados de imagem serem fornecidos ao atualizar a categoria.

Para facilitar isso, adicione dois métodos à classe `CategoriesBLL`, ambos nomeados `UpdateCategory`. O primeiro deve aceitar três `string` s, uma matriz `byte` e um `int` como seus parâmetros de entrada; o segundo, apenas três `string` s e um `int`. Os parâmetros de entrada do `string` são para o nome da categoria, a descrição e o caminho do arquivo de folheto, a matriz de `byte` é para o conteúdo binário da categoria s e a `int` identifica a `CategoryID` do registro a ser atualizado. Observe que a primeira sobrecarga invocará a segunda se a matriz de `byte` passada for `null`:

[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample3.cs)]

## <a name="step-3-copying-over-the-insert-and-view-functionality"></a>Etapa 3: copiando sobre a funcionalidade de inserção e exibição

No [tutorial anterior](including-a-file-upload-option-when-adding-a-new-record-cs.md) , criamos uma página chamada `UploadInDetailsView.aspx` que listou todas as categorias em um GridView e forneci um DetailsView para adicionar novas categorias ao sistema. Neste tutorial, estenderemos o GridView para incluir suporte à edição e exclusão. Em vez de continuar a trabalhar a partir de `UploadInDetailsView.aspx`, vamos, em vez disso, inserir essas alterações do tutorial na página `UpdatingAndDeleting.aspx` da mesma pasta, `~/BinaryData`. Copie e cole a marcação declarativa e o código de `UploadInDetailsView.aspx` para `UpdatingAndDeleting.aspx`.

Comece abrindo a página de `UploadInDetailsView.aspx`. Copie toda a sintaxe declarativa dentro do elemento `<asp:Content>`, conforme mostrado na Figura 3. Em seguida, abra `UpdatingAndDeleting.aspx` e cole essa marcação dentro de seu elemento `<asp:Content>`. Da mesma forma, copie o código da classe `UploadInDetailsView.aspx` s code-behind da página para `UpdatingAndDeleting.aspx`.

[![copiar a marcação declarativa de UploadInDetailsView. aspx](updating-and-deleting-existing-binary-data-cs/_static/image3.gif)](updating-and-deleting-existing-binary-data-cs/_static/image5.png)

**Figura 3**: copiar a marcação declarativa de `UploadInDetailsView.aspx` ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image6.png))

Depois de copiar sobre a marcação declarativa e o código, visite `UpdatingAndDeleting.aspx`. Você deve ver a mesma saída e ter a mesma experiência de usuário que com `UploadInDetailsView.aspx` página do tutorial anterior.

## <a name="step-4-adding-deleting-support-to-the-objectdatasource-and-gridview"></a>Etapa 4: adicionar suporte à exclusão para ObjectDataSource e GridView

Como discutimos de volta na [visão geral do tutorial inserir, atualizar e excluir dados](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) , o GridView fornece recursos internos de exclusão e esses recursos podem ser habilitados no tique de uma caixa de seleção se a fonte de dados subjacente de grade s der suporte à exclusão. Atualmente, o ObjectDataSource em que o GridView está associado (`CategoriesDataSource`) não oferece suporte à exclusão.

Para corrigir isso, clique na opção Configurar fonte de dados na marca inteligente ObjectDataSource s para iniciar o assistente. A primeira tela mostra que o ObjectDataSource está configurado para trabalhar com a classe `CategoriesBLL`. Clique em Avançar. Atualmente, somente as propriedades ObjectDataSource s `InsertMethod` e `SelectMethod` são especificadas. No entanto, o assistente preencheu automaticamente as listas suspensas nas guias atualizar e excluir com os métodos `UpdateCategory` e `DeleteCategory`, respectivamente. Isso ocorre porque, na classe `CategoriesBLL` marcamos esses métodos usando o `DataObjectMethodAttribute` como os métodos padrão para atualização e exclusão.

Por enquanto, defina a lista suspensa da guia de atualização como (nenhum), mas deixe a lista suspensa de excluir tabulações definida como `DeleteCategory`. Retornaremos a esse assistente na etapa 6 para adicionar suporte à atualização.

[![configurar o ObjectDataSource para usar o método DeleteCategory](updating-and-deleting-existing-binary-data-cs/_static/image4.gif)](updating-and-deleting-existing-binary-data-cs/_static/image7.png)

**Figura 4**: configurar o ObjectDataSource para usar o método `DeleteCategory` ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image8.png))

> [!NOTE]
> Ao concluir o assistente, o Visual Studio pode perguntar se você deseja atualizar campos e chaves, o que irá gerar os campos de controles da Web de dados. Escolha não, porque a escolha de Sim substituirá as personalizações de campo que você tenha feito.

Agora, o ObjectDataSource incluirá um valor para sua propriedade `DeleteMethod`, bem como um `DeleteParameter`. Lembre-se de que, ao usar o assistente para especificar os métodos, o Visual Studio define a Propriedade ObjectDataSource s `OldValuesParameterFormatString` como `original_{0}`, o que causa problemas com as invocações do método Update e Delete. Portanto, desmarque essa propriedade completamente ou redefina-a para o padrão, `{0}`. Se você precisar atualizar sua memória nessa Propriedade ObjectDataSource, consulte o tutorial [visão geral de inserção, atualização e exclusão de dados](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) .

Depois de concluir o assistente e corrigir o `OldValuesParameterFormatString`, a marcação declarativa s de ObjectDataSource deve ser semelhante à seguinte:

[!code-aspx[Main](updating-and-deleting-existing-binary-data-cs/samples/sample4.aspx)]

Depois de configurar o ObjectDataSource, adicione recursos de exclusão ao GridView marcando a caixa de seleção Habilitar exclusão na marca inteligente de GridView. Isso adicionará um CommandField ao GridView cuja propriedade `ShowDeleteButton` está definida como `true`.

[![habilitar o suporte para exclusão em GridView](updating-and-deleting-existing-binary-data-cs/_static/image5.gif)](updating-and-deleting-existing-binary-data-cs/_static/image9.png)

**Figura 5**: habilitar o suporte para exclusão em GridView ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image10.png))

Reserve um tempo para testar a funcionalidade de exclusão. Há uma chave estrangeira entre a tabela de `Products` s `CategoryID` e a `CategoryID`da tabela `Categories`, portanto, você obterá uma exceção de violação de restrição de chave estrangeira se tentar excluir qualquer uma das oito primeiras categorias. Para testar essa funcionalidade, adicione uma nova categoria, fornecendo um folheto e uma imagem. Minha categoria de teste, mostrada na Figura 6, inclui um arquivo de folheto de teste chamado `Test.pdf` e uma imagem de teste. A Figura 7 mostra o GridView após a adição da categoria de teste.

[![adicionar uma categoria de teste com um folheto e uma imagem](updating-and-deleting-existing-binary-data-cs/_static/image6.gif)](updating-and-deleting-existing-binary-data-cs/_static/image11.png)

**Figura 6**: adicionar uma categoria de teste com um folheto e uma imagem ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image12.png))

[![depois de inserir a categoria de teste, ela é exibida no GridView](updating-and-deleting-existing-binary-data-cs/_static/image7.gif)](updating-and-deleting-existing-binary-data-cs/_static/image13.png)

**Figura 7**: depois de inserir a categoria de teste, ela é exibida no GridView ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image14.png))

No Visual Studio, atualize o Gerenciador de Soluções. Agora você deve ver um novo arquivo na pasta `~/Brochures`, `Test.pdf` (consulte a Figura 8).

Em seguida, clique no link excluir na linha de categoria de teste, fazendo com que a página seja repostada e a classe `CategoriesBLL` s `DeleteCategory` método a ser acionado. Isso invocará o método da DAL s `Delete`, fazendo com que a instrução `DELETE` apropriada seja enviada ao banco de dados. Os dados são então reassociados ao GridView e a marcação é enviada de volta para o cliente com a categoria de teste que não está mais presente.

Embora o fluxo de trabalho de exclusão tenha removido com êxito o registro de categoria de teste da tabela `Categories`, ele não removeu o arquivo de folheto do sistema de arquivos do servidor Web. Atualize o Gerenciador de Soluções e você verá que `Test.pdf` ainda está localizado na pasta `~/Brochures`.

![O arquivo Test. pdf não foi excluído do sistema de arquivos do servidor Web](updating-and-deleting-existing-binary-data-cs/_static/image8.gif)

**Figura 8**: o arquivo de `Test.pdf` não foi excluído do sistema de arquivos do servidor Web

## <a name="step-5-removing-the-deleted-category-s-brochure-file"></a>Etapa 5: removendo o arquivo de folheto da categoria excluída

Uma das desvantagens de armazenar dados binários externos ao banco de dado é que etapas adicionais devem ser executadas para limpar esses arquivos quando o registro de banco de dados associado é excluído. O GridView e o ObjectDataSource fornecem eventos que são acionados antes e depois que o comando de exclusão é executado. Na verdade, precisamos criar manipuladores de eventos para os eventos de pré e pós-ação. Antes que o registro de `Categories` seja excluído, precisamos determinar seu caminho de arquivo PDF, mas não queremos excluir o PDF antes que a categoria seja excluída caso haja alguma exceção e a categoria não seja excluída.

O evento GridView s [`RowDeleting`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx) é acionado antes do comando ObjectDataSource s Delete ser invocado, enquanto seu [evento`RowDeleted`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleted.aspx) é acionado após. Crie manipuladores de eventos para esses dois eventos usando o seguinte código:

[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample5.cs)]

No manipulador de eventos `RowDeleting`, a `CategoryID` da linha que está sendo excluída é capturada da coleção de `DataKeys` do GridView, que pode ser acessada nesse manipulador de eventos por meio da coleção `e.Keys`. Em seguida, o `GetCategoryByCategoryID(categoryID)` da classe `CategoriesBLL` é invocado para retornar informações sobre o registro que está sendo excluído. Se o objeto de `CategoriesDataRow` retornado tiver um valor não`NULL``BrochurePath`, ele será armazenado na variável de página `deletedCategorysPdfPath` para que o arquivo possa ser excluído no manipulador de eventos `RowDeleted`.

> [!NOTE]
> Em vez de recuperar os detalhes de `BrochurePath` para o registro de `Categories` que está sendo excluído no manipulador de eventos `RowDeleting`, poderíamos ter adicionado opcionalmente o `BrochurePath` à Propriedade GridView s `DataKeyNames` e acessado o valor do registro s por meio da coleção de `e.Keys`. Isso aumentaria ligeiramente o tamanho do estado de exibição de GridView, mas reduziria a quantidade de código necessária e salvaria uma viagem no banco de dados.

Depois que o comando de exclusão subjacente de ObjectDataSource s for invocado, o manipulador de eventos GridView s `RowDeleted` será acionado. Se não houvesse nenhuma exceção na exclusão dos dados e houver um valor para `deletedCategorysPdfPath`, o PDF será excluído do sistema de arquivos. Observe que esse código extra não é necessário para limpar os dados binários da categoria associados à sua imagem. Esses s porque os dados da imagem são armazenados diretamente no banco de dado, portanto, excluir a linha de `Categories` também exclui os dados da categoria s Picture.

Depois de adicionar os dois manipuladores de eventos, execute este caso de teste novamente. Ao excluir a categoria, seu PDF associado também será excluído.

A atualização de um registro existente dos dados binários associados fornece alguns desafios interessantes. O restante deste tutorial investiga a adição de recursos de atualização ao folheto e à imagem. A etapa 6 explora técnicas para atualizar as informações do folheto enquanto a etapa 7 examina a atualização da imagem.

## <a name="step-6-updating-a-category-s-brochure"></a>Etapa 6: atualizando um folheto de categoria s

Conforme discutido em [uma visão geral do tutorial inserir, atualizar e excluir dados](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) , o GridView oferece suporte de edição em nível de linha interno que pode ser implementado pelo tique de uma caixa de seleção se sua fonte de dados subjacente estiver configurada adequadamente. Atualmente, o `CategoriesDataSource` ObjectDataSource ainda não está configurado para incluir suporte à atualização, portanto, vamos adicioná-lo em.

Clique no link configurar fonte de dados do assistente ObjectDataSource s e vá para a segunda etapa. Devido ao `DataObjectMethodAttribute` usado em `CategoriesBLL`, a lista suspensa UPDATE deve ser preenchida automaticamente com a sobrecarga `UpdateCategory` que aceita quatro parâmetros de entrada (para todas as colunas, mas `Picture`). Altere isso para que ele use a sobrecarga com cinco parâmetros.

[![configurar o ObjectDataSource para usar o método UpdateCategory que inclui um parâmetro para a imagem](updating-and-deleting-existing-binary-data-cs/_static/image9.gif)](updating-and-deleting-existing-binary-data-cs/_static/image15.png)

**Figura 9**: configurar o ObjectDataSource para usar o método `UpdateCategory` que inclui um parâmetro para `Picture` ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image16.png))

Agora, o ObjectDataSource incluirá um valor para sua propriedade `UpdateMethod`, bem como `UpdateParameter` s correspondentes. Conforme observado na etapa 4, o Visual Studio define a Propriedade ObjectDataSource s `OldValuesParameterFormatString` como `original_{0}` ao usar o assistente para configurar fonte de dados. Isso causará problemas com as invocações do método Update e Delete. Portanto, desmarque essa propriedade completamente ou redefina-a para o padrão, `{0}`.

Depois de concluir o assistente e corrigir o `OldValuesParameterFormatString`, a marcação declarativa s de ObjectDataSource deve ser semelhante ao seguinte:

[!code-aspx[Main](updating-and-deleting-existing-binary-data-cs/samples/sample6.aspx)]

Para ativar os recursos de edição internos do GridView, marque a opção Habilitar edição na marca inteligente s GridView. Isso definirá a Propriedade CommandField s `ShowEditButton` como `true`, resultando no acréscimo de um botão de edição (e nos botões atualizar e cancelar da linha que está sendo editada).

[![configurar o GridView para dar suporte à edição](updating-and-deleting-existing-binary-data-cs/_static/image10.gif)](updating-and-deleting-existing-binary-data-cs/_static/image17.png)

**Figura 10**: configurar o GridView para dar suporte à edição ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image18.png))

Visite a página por meio de um navegador e clique em um dos botões de edição das linhas. Os `CategoryName` e `Description` BoundFields são renderizados como caixas de Text. O `BrochurePath` TemplateField não tem um `EditItemTemplate`, portanto, ele continua mostrando seu `ItemTemplate` um link para o folheto. O `Picture` ImageField é renderizado como uma caixa de texto cuja propriedade `Text` é atribuída ao valor do valor `DataImageUrlField` s ImageField, nesse caso `CategoryID`.

[![o GridView não tem uma interface de edição para BrochurePath](updating-and-deleting-existing-binary-data-cs/_static/image11.gif)](updating-and-deleting-existing-binary-data-cs/_static/image19.png)

**Figura 11**: o GridView não tem uma interface de edição para `BrochurePath` ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image20.png))

## <a name="customizing-thebrochurepaths-editing-interface"></a>Personalizando a interface de edição de`BrochurePath`s

Precisamos criar uma interface de edição para o `BrochurePath` TemplateField, que permite ao usuário:

- Deixe a categoria s folheto como está,
- Atualize o folheto da categoria s carregando um novo folheto ou
- Remova o folheto da categoria s completamente (caso a categoria não tenha mais um folheto associado).

Também precisamos atualizar a interface de edição do `Picture` ImageField, mas vamos chegar a isso na etapa 7.

Na marca inteligente de GridView, clique no link editar modelos e selecione a `BrochurePath` s `EditItemTemplate` de modelos na lista suspensa. Adicione um controle Web RadioButtonList a esse modelo, definindo sua propriedade `ID` como `BrochureOptions` e sua propriedade `AutoPostBack` como `true`. Na janela Propriedades, clique nas reticências na propriedade `Items`, que abrirá o editor de coleção de `ListItem`. Adicione as três opções a seguir com `Value` s 1, 2 e 3, respectivamente:

- Usar o folheto atual
- Remover folheto atual
- Carregar novo folheto

Defina a primeira Propriedade `ListItem` s `Selected` como `true`.

![Adicionar três ListItems à RadioButtonList](updating-and-deleting-existing-binary-data-cs/_static/image12.gif)

**Figura 12**: adicionar três `ListItem` s à RadioButtonList

Abaixo da RadioButtonList, adicione um controle FileUpload chamado `BrochureUpload`. Defina sua propriedade `Visible` como `false`.

[![adicionar um controle RadioButtonList e FileUpload para o EditItemTemplate](updating-and-deleting-existing-binary-data-cs/_static/image13.gif)](updating-and-deleting-existing-binary-data-cs/_static/image21.png)

**Figura 13**: adicionar um controle RadioButtonList e FileUpload à `EditItemTemplate` ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image22.png))

Essa RadioButtonList fornece as três opções para o usuário. A ideia é que o controle FileUpload será exibido somente se a última opção, carregar novo folheto, estiver selecionada. Para fazer isso, crie um manipulador de eventos para o evento `SelectedIndexChanged` RadioButtonList s e adicione o seguinte código:

[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample7.cs)]

Como os controles RadioButtonList e FileUpload estão dentro de um modelo, precisamos escrever um pouco de código para acessar esses controles programaticamente. O manipulador de eventos `SelectedIndexChanged` é passado uma referência de RadioButtonList no parâmetro de entrada `sender`. Para obter o controle FileUpload, precisamos obter o controle pai RadioButtonList s e usar o método `FindControl("controlID")` a partir daí. Assim que tivermos uma referência para os controles RadioButtonList e FileUpload, a propriedade s `Visible` Control do controle FileUpload será definida como `true` somente se o RadioButtonList s `SelectedValue` for igual a 3, que é o `Value` para o novo `ListItem`de folheto de upload.

Com esse código em vigor, Reserve um tempo para testar a interface de edição. Clique no botão Editar para uma linha. Inicialmente, a opção usar folheto atual deve ser selecionada. A alteração do índice selecionado causa um postback. Se a terceira opção for selecionada, o controle FileUpload será exibido, caso contrário, ficará oculto. A Figura 14 mostra a interface de edição quando o botão Editar é clicado pela primeira vez; A Figura 15 mostra a interface após a opção carregar novo folheto ser selecionada.

[![inicialmente, a opção usar folheto atual está selecionada](updating-and-deleting-existing-binary-data-cs/_static/image14.gif)](updating-and-deleting-existing-binary-data-cs/_static/image23.png)

**Figura 14**: inicialmente, a opção usar o folheto atual está selecionada ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image24.png))

[![escolher a opção carregar novo folheto exibe o controle FileUpload](updating-and-deleting-existing-binary-data-cs/_static/image15.gif)](updating-and-deleting-existing-binary-data-cs/_static/image25.png)

**Figura 15**: a escolha da opção carregar novo folheto exibe o controle FileUpload ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image26.png))

## <a name="saving-the-brochure-file-and-updating-thebrochurepathcolumn"></a>Salvando o arquivo de folheto e atualizando a coluna`BrochurePath`

Quando o botão de atualização de GridView é clicado, seu evento de `RowUpdating` é acionado. O comando ObjectDataSource s Update é invocado e, em seguida, o evento GridView s `RowUpdated` é acionado. Assim como com o fluxo de trabalho de exclusão, precisamos criar manipuladores de eventos para ambos os eventos. No manipulador de eventos `RowUpdating`, precisamos determinar a ação a ser tomada com base na `SelectedValue` do `BrochureOptions` RadioButtonList:

- Se o `SelectedValue` for 1, queremos continuar usando a mesma configuração de `BrochurePath`. Portanto, precisamos definir o parâmetro ObjectDataSource s `brochurePath` como o valor `BrochurePath` existente do registro que está sendo atualizado. O parâmetro ObjectDataSource s `brochurePath` pode ser definido usando `e.NewValues["brochurePath"] = value`.
- Se o `SelectedValue` for 2, queremos definir o valor de `BrochurePath` do registro como `NULL`. Isso pode ser feito definindo o parâmetro ObjectDataSource s `brochurePath` como `Nothing`, o que resulta em um banco de dados `NULL` sendo usado na instrução `UPDATE`. Se houver um arquivo de folheto existente que está sendo removido, precisamos excluir o arquivo existente. No entanto, só queremos fazer isso se a atualização for concluída sem gerar uma exceção.
- Se o `SelectedValue` for 3, queremos garantir que o usuário carregou um arquivo PDF e, em seguida, salve-o no sistema de arquivos e atualize o valor da coluna de `BrochurePath` do registro. Além disso, se houver um arquivo de folheto existente que está sendo substituído, precisamos excluir o arquivo anterior. No entanto, só queremos fazer isso se a atualização for concluída sem gerar uma exceção.

As etapas necessárias para serem concluídas quando a `SelectedValue` de RadioButtonList s é 3 são praticamente idênticas àquelas usadas pelo manipulador de eventos de `ItemInserting` de DetailsView. Esse manipulador de eventos é executado quando um novo registro de categoria é adicionado do controle DetailsView que adicionamos no [tutorial anterior](including-a-file-upload-option-when-adding-a-new-record-cs.md). Portanto, ele nos convém a refatorar essa funcionalidade em métodos separados. Especificamente, movi a funcionalidade comum em dois métodos:

- `ProcessBrochureUpload(FileUpload, out bool)` aceita como entrada uma instância de controle FileUpload e um valor booliano de saída que especifica se a operação de exclusão ou de edição deve continuar ou se deve ser cancelada devido a algum erro de validação. Esse método retorna o caminho para o arquivo salvo ou `null` se nenhum arquivo foi salvo.
- `DeleteRememberedBrochurePath` exclui o arquivo especificado pelo caminho na variável de página `deletedCategorysPdfPath` se `deletedCategorysPdfPath` não for `null`.

O código para esses dois métodos segue. Observe a similaridade entre `ProcessBrochureUpload` e o manipulador de eventos de `ItemInserting` de DetailsView do tutorial anterior. Neste tutorial, atualizei os manipuladores de eventos de DetailsView s para usar esses novos métodos. Baixe o código associado a este tutorial para ver as modificações nos manipuladores de eventos de DetailsView.

[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample8.cs)]

Os manipuladores de evento GridView s `RowUpdating` e `RowUpdated` usam os métodos `ProcessBrochureUpload` e `DeleteRememberedBrochurePath`, como mostra o código a seguir:

[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample9.cs)]

Observe como o manipulador de eventos `RowUpdating` usa uma série de instruções condicionais para executar a ação apropriada com base no valor da propriedade `BrochureOptions` RadioButtonList s `SelectedValue`.

Com esse código em vigor, você pode editar uma categoria e fazer com que ela use seu folheto atual, não use nenhum folheto ou carregue uma nova. Vá em frente e experimente. Defina pontos de interrupção no `RowUpdating` e `RowUpdated` manipuladores de eventos para obter uma noção do fluxo de trabalho.

## <a name="step-7-uploading-a-new-picture"></a>Etapa 7: carregando uma nova imagem

A interface de edição do `Picture` ImageField é renderizada como uma caixa de texto populada com o valor de sua propriedade `DataImageUrlField`. Durante o fluxo de trabalho de edição, o GridView passa um parâmetro para o ObjectDataSource com o parâmetro s Name o valor da Propriedade ImageField s `DataImageUrlField` e o valor s do parâmetro o valor inserido na caixa de texto na interface de edição. Esse comportamento é adequado quando a imagem é salva como um arquivo no sistema de arquivos e o `DataImageUrlField` contém a URL completa da imagem. Com essas circunstâncias, a interface de edição exibe a URL de s da imagem na caixa de texto, que o usuário pode alterar e salvou de volta no banco de dados. Concedida, essa interface padrão não permite que o usuário carregue uma nova imagem, mas permite que elas alterem a URL da imagem do valor atual para outro. Para este tutorial, no entanto, a interface de edição padrão do ImageField não é suficiente porque os dados binários `Picture` estão sendo armazenados diretamente no banco e a propriedade `DataImageUrlField` mantém apenas a `CategoryID`.

Para entender melhor o que acontece em nosso tutorial quando um usuário edita uma linha com um ImageField, considere o exemplo a seguir: um usuário edita uma linha com `CategoryID` 10, fazendo com que o `Picture` ImageField seja renderizado como uma caixa de texto com o valor 10. Imagine que o usuário altere o valor nessa caixa de texto para 50 e clique no botão atualizar. Um postback ocorre e o GridView inicialmente cria um parâmetro chamado `CategoryID` com o valor 50. No entanto, antes que o GridView envie esse parâmetro (e os parâmetros `CategoryName` e `Description`), ele adiciona os valores da coleção de `DataKeys`. Portanto, ele substitui o parâmetro `CategoryID` pela linha atual, com base em `CategoryID` valor, 10. Em suma, a interface de edição do ImageField s não afeta o fluxo de trabalho de edição deste tutorial porque os nomes da propriedade de `DataImageUrlField` s do ImageField e do valor da grade s `DataKey` são um no mesmo.

Embora o ImageField facilite a exibição de uma imagem com base nos dados do banco, não queremos fornecer uma TextBox na interface de edição. Em vez disso, desejamos oferecer um controle FileUpload que o usuário final possa usar para alterar a categoria s Picture. Ao contrário do valor `BrochurePath`, para esses tutoriais, decidimos exigir que cada categoria tenha uma imagem. Portanto, não precisamos deixar que o usuário indique que não há nenhuma imagem associada que o usuário possa carregar uma nova imagem ou deixar a imagem atual no estado em que se encontra.

Para personalizar a interface de edição do ImageField, precisamos convertê-la em um TemplateField. Na marca inteligente de GridView, clique no link Editar colunas, selecione o ImageField e clique no link converter este campo em um modelo.

![Converter o ImageField em um TemplateField](updating-and-deleting-existing-binary-data-cs/_static/image16.gif)

**Figura 16**: converter o ImageField em um TemplateField

Converter o ImageField em um TemplateField dessa maneira gera um TemplateField com dois modelos. Como mostra a sintaxe declarativa a seguir, o `ItemTemplate` contém um controle da Web de imagem cuja propriedade `ImageUrl` é atribuída usando a sintaxe DataBinding com base nas propriedades `DataImageUrlField` ImageField s e `DataImageUrlFormatString`. O `EditItemTemplate` contém uma caixa de texto cuja propriedade `Text` está associada ao valor especificado pela propriedade `DataImageUrlField`.

[!code-aspx[Main](updating-and-deleting-existing-binary-data-cs/samples/sample10.aspx)]

Precisamos atualizar o `EditItemTemplate` para usar um controle FileUpload. Na marca inteligente de GridView s, clique no link editar modelos e, em seguida, selecione a `Picture` s `EditItemTemplate` de modelos na lista suspensa. No modelo, você deve ver uma caixa de texto para removê-la. Em seguida, arraste um controle FileUpload da caixa de ferramentas para o modelo, definindo sua `ID` como `PictureUpload`. Além disso, adicione o texto para alterar a figura s da categoria, especifique uma nova imagem. Para manter a mesma categoria, deixe o campo vazio para o modelo também.

[![adicionar um controle FileUpload ao EditItemTemplate](updating-and-deleting-existing-binary-data-cs/_static/image17.gif)](updating-and-deleting-existing-binary-data-cs/_static/image27.png)

**Figura 17**: adicionar um controle FileUpload ao `EditItemTemplate` ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image28.png))

Depois de personalizar a interface de edição, exiba seu progresso em um navegador. Ao exibir uma linha no modo somente leitura, a imagem da categoria s é mostrada como estava antes, mas clicar no botão Editar renderiza a coluna de imagem como texto com um controle FileUpload.

[![a interface de edição inclui um controle FileUpload](updating-and-deleting-existing-binary-data-cs/_static/image18.gif)](updating-and-deleting-existing-binary-data-cs/_static/image29.png)

**Figura 18**: a interface de edição inclui um controle FileUpload ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image30.png))

Lembre-se de que o ObjectDataSource está configurado para chamar a classe `CategoriesBLL` `UpdateCategory` método que aceita como entrada os dados binários da imagem como uma matriz `byte`. No entanto, se essa matriz tiver um valor de `null`, a sobrecarga de `UpdateCategory` alternativa será chamada, que emitirá a instrução SQL `UPDATE` que não modifica a coluna de `Picture`, deixando assim a imagem de s atual intacta. Portanto, no manipulador de eventos GridView s `RowUpdating` precisamos fazer referência programaticamente ao controle FileUpload `PictureUpload` e determinar se um arquivo foi carregado. Se um não tiver sido carregado, *não* queremos especificar um valor para o parâmetro `picture`. Por outro lado, se um arquivo foi carregado no controle FileUpload `PictureUpload`, queremos garantir que ele seja um arquivo JPG. Se for, podemos enviar seu conteúdo binário para o ObjectDataSource por meio do parâmetro `picture`.

Assim como com o código usado na etapa 6, grande parte do código necessário aqui já existe no manipulador de eventos de `ItemInserting` de DetailsView. Portanto, eu me refatorei a funcionalidade comum em um novo método, `ValidPictureUpload`e atualizei o manipulador de eventos `ItemInserting` para usar esse método.

Adicione o seguinte código ao início do manipulador de eventos de `RowUpdating` do GridView. É importante que esse código venha antes do código que salva o arquivo de folheto, pois não queremos salvar o folheto no sistema de arquivos do servidor Web se um arquivo de imagem inválido for carregado.

[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample11.cs)]

O método `ValidPictureUpload(FileUpload)` usa um controle FileUpload como seu único parâmetro de entrada e verifica a extensão de s do arquivo carregado para garantir que o arquivo carregado seja um JPG; Ele será chamado somente se um arquivo de imagem for carregado. Se nenhum arquivo for carregado, o parâmetro de imagem não será definido e, portanto, usará seu valor padrão de `null`. Se uma imagem foi carregada e `ValidPictureUpload` retorna `true`, o parâmetro `picture` é atribuído aos dados binários da imagem carregada; Se o método retornar `false`, o fluxo de trabalho de atualização será cancelado e o manipulador de eventos sairá.

O código do método de `ValidPictureUpload(FileUpload)`, que foi refatorado do manipulador de eventos de `ItemInserting` DetailsView, segue:

[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample12.cs)]

## <a name="step-8-replacing-the-original-categories-pictures-with-jpgs"></a>Etapa 8: substituindo as imagens de categorias originais por JPGs

Lembre-se de que as oito categorias originais de imagens são arquivos de bitmap encapsulados em um cabeçalho OLE. Agora que adicionamos a capacidade de editar uma imagem do registro existente, Reserve um instante para substituir esses bitmaps por JPGs. Se você quiser continuar a usar as imagens da categoria atual, poderá convertê-las em JPGs executando as seguintes etapas:

1. Salve as imagens de bitmap no disco rígido. Visite a página `UpdatingAndDeleting.aspx` no navegador e para cada uma das oito primeiras categorias, clique com o botão direito do mouse na imagem e escolha salvar a imagem.
2. Abra a imagem no editor de imagem de sua escolha. Você pode usar o Microsoft Paint, por exemplo.
3. Salve o bitmap como uma imagem JPG.
4. Atualize a categoria s Picture por meio da interface de edição, usando o arquivo JPG.

Depois de editar uma categoria e carregar a imagem JPG, a imagem não será renderizada no navegador porque a página `DisplayCategoryPicture.aspx` está removendo os primeiros 78 bytes das imagens das primeiras oito categorias. Corrija isso removendo o código que executa a remoção do cabeçalho OLE. Depois de fazer isso, o manipulador de eventos `DisplayCategoryPicture.aspx``Page_Load` deve ter apenas o seguinte código:

[!code-vb[Main](updating-and-deleting-existing-binary-data-cs/samples/sample13.vb)]

> [!NOTE]
> As `UpdatingAndDeleting.aspx` páginas s inserindo e editando interfaces poderiam usar um pouco mais de trabalho. O `CategoryName` e `Description` BoundFields no DetailsView e no GridView devem ser convertidos em TemplateFields. Como `CategoryName` não permite valores de `NULL`, um RequiredFieldValidator deve ser adicionado. E a caixa de texto `Description` deve provavelmente ser convertida em uma caixa de texto de várias linhas. Eu deixe esses toques de acabamento como um exercício para você.

## <a name="summary"></a>Resumo

Este tutorial conclui nossa visão de como trabalhar com dados binários. Neste tutorial e nos três anteriores, vimos como dados binários podem ser armazenados no sistema de arquivos ou diretamente no banco de dados. Um usuário fornece dados binários para o sistema selecionando um arquivo do disco rígido e carregando-o para o servidor Web, onde ele pode ser armazenado no sistema de arquivos ou inserido no banco de dados. O ASP.NET 2,0 inclui um controle FileUpload que torna o fornecimento de uma interface tão fácil quanto arrastar e soltar. No entanto, como observado no tutorial [carregando arquivos](uploading-files-cs.md) , o controle FileUpload só é adequado para carregamentos de arquivos relativamente pequenos, o que, de preferência, não excede um megabyte. Também exploramos como associar dados carregados ao modelo de dados subjacente, bem como editar e excluir os dados binários de registros existentes.

Nosso próximo conjunto de tutoriais explora várias técnicas de cache. O caching fornece um meio para melhorar o desempenho geral de um aplicativo, obtendo os resultados de operações caras e armazenando-os em um local que pode ser acessado mais rapidamente.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. O revisor de Lead para este tutorial foi Teresa Murphy. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](including-a-file-upload-option-when-adding-a-new-record-cs.md)
> [Próximo](uploading-files-vb.md)
