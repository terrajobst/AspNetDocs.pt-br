---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-vb
title: Implementando a simultaneidade otimista com o SqlDataSource (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, examinaremos os conceitos básicos do controle de simultaneidade otimista e, em seguida, exploraremos como implementá-lo usando o controle SqlDataSource.
ms.author: riande
ms.date: 02/20/2007
ms.assetid: a8fa72ee-8328-4854-a419-c1b271772303
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: 431734b5245c20ac840147cf0827fa7f8d1e4d17
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78553014"
---
# <a name="implementing-optimistic-concurrency-with-the-sqldatasource-vb"></a>Implementar a simultaneidade otimista com o SqlDataSource (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_50_VB.exe) ou [baixar PDF](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/datatutorial50vb1.pdf)

> Neste tutorial, examinaremos os conceitos básicos do controle de simultaneidade otimista e, em seguida, exploraremos como implementá-lo usando o controle SqlDataSource.

## <a name="introduction"></a>Introdução

No tutorial anterior, examinamos como adicionar recursos de inserção, atualização e exclusão ao controle SqlDataSource. Em suma, para fornecer esses recursos, precisávamos especificar o `INSERT`correspondente, `UPDATE`ou `DELETE` instrução SQL nas propriedades `InsertCommand`, `UpdateCommand`ou `DeleteCommand` de controle, juntamente com os parâmetros apropriados nas coleções `InsertParameters`, `UpdateParameters`e `DeleteParameters`. Embora essas propriedades e coleções possam ser especificadas manualmente, o botão configurar do assistente de fonte de dados oferece uma caixa de seleção gerar `INSERT`, `UPDATE`e instruções de `DELETE` que criará automaticamente essas instruções com base na instrução `SELECT`.

Junto com a caixa de seleção gerar `INSERT`, `UPDATE`e `DELETE` instruções, as caixas de diálogo opções de geração de SQL avançadas incluem uma opção usar simultaneidade otimista (consulte a Figura 1). Quando marcada, as cláusulas de `WHERE` nas instruções de `UPDATE` e `DELETE` geradas automaticamente são modificadas para executar somente a atualização ou exclusão se os dados subjacentes não tiverem sido modificados desde que o usuário carregou os dados pela última vez na grade.

![Você pode adicionar suporte a simultaneidade otimista da caixa de diálogo opções de geração de SQL avançadas](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image1.gif)

**Figura 1**: você pode adicionar suporte de simultaneidade otimista da caixa de diálogo opções de geração de SQL avançadas

De volta ao tutorial [implementação de simultaneidade otimista](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) , examinamos os conceitos básicos do controle de simultaneidade otimista e como adicioná-lo ao ObjectDataSource. Neste tutorial, vamos retocar os conceitos básicos do controle de simultaneidade otimista e, em seguida, explorar como implementá-lo usando o SqlDataSource.

## <a name="a-recap-of-optimistic-concurrency"></a>Uma recapitulação da simultaneidade otimista

Para aplicativos Web que permitem que vários usuários simultâneos editem ou excluam os mesmos dados, existe uma possibilidade de que um usuário substitua acidentalmente outras alterações de s. No tutorial [implementando simultaneidade otimista](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) , forneci o seguinte exemplo:

Imagine que dois usuários, Jisun e Sam, estavam visitando uma página em um aplicativo que permitia que os visitantes atualizassem e excluíssem produtos por meio de um controle GridView. Ambos clicam no botão Editar para Chai ao mesmo tempo. Jisun altera o nome do produto para Chai chá e clica no botão atualizar. O resultado líquido é uma instrução `UPDATE` que é enviada ao banco de dados, que define *todos* os campos atualizáveis dos produtos (mesmo que Jisun tenha atualizado apenas um campo, `ProductName`). Nesse momento, o banco de dados tem os valores de Chai chá, das bebidas da categoria, do fornecedor exóticas liquids e assim por diante para esse produto específico. No entanto, a tela GridView em Sam s ainda mostra o nome do produto na linha GridView editável como Chai. Alguns segundos depois que as alterações de Jisun foram confirmadas, o Sam atualiza a categoria para condiments e clica em atualizar. Isso resulta em uma instrução `UPDATE` enviada ao banco de dados que define o nome do produto como Chai, a `CategoryID` para a ID da categoria condiments correspondente e assim por diante. As alterações de Jisun no nome do produto foram substituídas.

A Figura 2 ilustra essa interação.

[![quando dois usuários atualizam simultaneamente um registro com potencial para as alterações de um usuário para substituir as outras s](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image2.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image1.png)

**Figura 2**: quando dois usuários atualizam simultaneamente um registro, há um potencial para as alterações de um usuário para substituir os outros s ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image2.png))

Para evitar que esse cenário seja desdobrado, uma forma de [controle de simultaneidade](http://en.wikipedia.org/wiki/Concurrency_control) deve ser implementada. [Simultaneidade otimista](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) o foco deste tutorial funciona na suposição de que, embora possa haver conflitos de simultaneidade a cada momento e, em seguida, a grande maioria do tempo que esses conflitos não surgirão. Portanto, se ocorrer um conflito, o controle de simultaneidade otimista simplesmente informa ao usuário que suas alterações não podem ser salvas, pois outro usuário modificou os mesmos dados.

> [!NOTE]
> Para aplicativos em que se supõe que haverá muitos conflitos de simultaneidade ou se esses conflitos não forem tolerantes, o controle de simultaneidade pessimista poderá ser usado em seu lugar. Consulte o tutorial [implementando simultaneidade otimista](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) para obter uma discussão mais completa sobre o controle de simultaneidade pessimista.

O controle de simultaneidade otimista funciona garantindo que o registro que está sendo atualizado ou excluído tenha os mesmos valores que faziam quando o processo de atualização ou exclusão foi iniciado. Por exemplo, ao clicar no botão Editar em um GridView editável, os valores de s de registro são lidos do banco de dados e exibidos em caixas de Texte outros controles da Web. Esses valores originais são salvos pelo GridView. Posteriormente, depois que o usuário fizer suas alterações e clicar no botão atualizar, a instrução `UPDATE` usada deverá levar em conta os valores originais mais os novos valores e atualizar somente o registro de banco de dados subjacente se os valores originais que o usuário iniciou editando forem idênticos aos valores que ainda estão no banco de dados. A Figura 3 descreve essa sequência de eventos.

[![para que a atualização ou exclusão seja realizada com sucesso, os valores originais devem ser iguais aos valores do banco de dados atual](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image3.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image3.png)

**Figura 3**: para que a atualização ou exclusão seja realizada com sucesso, os valores originais devem ser iguais aos valores do banco de dados atual ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image4.png))

Há várias abordagens para implementar a simultaneidade otimista (consulte a [lógica de atualização de simultaneidade otimista](http://www.eggheadcafe.com/articles/20050719.asp) de [Peter A. Bromberg](http://peterbromberg.net/)para obter uma breve visão de várias opções). A técnica usada pelo SqlDataSource (bem como pelos conjuntos de dados digitados pelo ADO.NET usados em nossa camada de acesso a data) aumenta a cláusula de `WHERE` para incluir uma comparação de todos os valores originais. A instrução `UPDATE` a seguir, por exemplo, atualiza o nome e o preço de um produto somente se os valores do banco de dados atual forem iguais aos valores que foram originalmente recuperados ao atualizar o registro no GridView. Os parâmetros `@ProductName` e `@UnitPrice` contêm os novos valores inseridos pelo usuário, enquanto `@original_ProductName` e `@original_UnitPrice` contêm os valores que foram carregados originalmente no GridView quando o botão Editar foi clicado:

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample1.sql)]

Como veremos neste tutorial, habilitar o controle de simultaneidade otimista com o SqlDataSource é tão simples quanto marcar uma caixa de seleção.

## <a name="step-1-creating-a-sqldatasource-that-supports-optimistic-concurrency"></a>Etapa 1: Criando um SqlDataSource que dá suporte à simultaneidade otimista

Comece abrindo a página de `OptimisticConcurrency.aspx` da pasta `SqlDataSource`. Arraste um controle SqlDataSource da caixa de ferramentas para o designer, configurações sua propriedade `ID` como `ProductsDataSourceWithOptimisticConcurrency`. Em seguida, clique no link configurar fonte de dados na marca inteligente controlar s. Na primeira tela do assistente, escolha trabalhar com o `NORTHWINDConnectionString` e clique em Avançar.

[![optar por trabalhar com o NORTHWINDConnectionString](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image4.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image5.png)

**Figura 4**: escolha para trabalhar com a `NORTHWINDConnectionString` ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image6.png))

Para este exemplo, vamos adicionar um GridView que permite aos usuários editar a tabela `Products`. Portanto, na tela configurar a instrução SELECT, escolha a tabela `Products` na lista suspensa e selecione as colunas `ProductID`, `ProductName`, `UnitPrice`e `Discontinued`, conforme mostrado na Figura 5.

[![da tabela produtos, retorne as colunas ProductID, ProductName, PreçoUnitário e descontinuadas](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image5.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image7.png)

**Figura 5**: na tabela `Products`, retorne as colunas `ProductID`, `ProductName`, `UnitPrice`e `Discontinued` ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image8.png))

Depois de escolher as colunas, clique no botão Avançado para exibir a caixa de diálogo opções de geração de SQL avançadas. Marque as instruções Generate `INSERT`, `UPDATE`e `DELETE` e use as caixas de seleção de simultaneidade otimista e clique em OK (consulte novamente a Figura 1 para obter uma captura de tela). Conclua o assistente clicando em avançar e em concluir.

Depois de concluir o assistente para configurar fonte de dados, Reserve um momento para examinar as propriedades resultantes `DeleteCommand` e `UpdateCommand` e as coleções `DeleteParameters` e `UpdateParameters`. A maneira mais fácil de fazer isso é clicar na guia origem no canto inferior esquerdo para ver a sintaxe declarativa de s da página. Lá, você encontrará um valor `UpdateCommand` de:

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample2.sql)]

Com sete parâmetros na coleção de `UpdateParameters`:

[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample3.aspx)]

Da mesma forma, a propriedade `DeleteCommand` e a coleção de `DeleteParameters` devem ser semelhantes às seguintes:

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample4.sql)]

[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample5.aspx)]

Além de aumentar as cláusulas de `WHERE` das propriedades `UpdateCommand` e `DeleteCommand` (e adicionar os parâmetros adicionais às respectivas coleções de parâmetros), a seleção da opção usar simultaneidade otimista ajusta duas outras propriedades:

- Altera a [propriedade`ConflictDetection`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.conflictdetection.aspx) de `OverwriteChanges` (o padrão) para `CompareAllValues`
- Altera a [propriedade`OldValuesParameterFormatString`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.oldvaluesparameterformatstring.aspx) de {0} (o padrão) para {0} original\_.

Quando o controle da Web de dados chama o método SqlDataSource s `Update()` ou `Delete()`, ele passa os valores originais. Se a Propriedade SqlDataSource s `ConflictDetection` for definida como `CompareAllValues`, esses valores originais serão adicionados ao comando. A propriedade `OldValuesParameterFormatString` fornece o padrão de nomenclatura usado para esses parâmetros de valor original. O assistente para configurar fonte de dados usa o\_original {0} e nomeia cada parâmetro original nas propriedades `UpdateCommand` e `DeleteCommand` e as coleções `UpdateParameters` e `DeleteParameters` de acordo.

> [!NOTE]
> Como não usamos os recursos de inserção do controle SqlDataSource, sinta-se à vontade para remover a propriedade `InsertCommand` e sua coleção de `InsertParameters`.

## <a name="correctly-handlingnullvalues"></a>Manipulando corretamente`NULL`valores

Infelizmente, as instruções aumentadas de `UPDATE` e `DELETE` geradas automaticamente pelo Assistente para configurar fonte de dados ao usar a simultaneidade otimista *não* funcionam com registros que contêm valores de `NULL`. Para ver o porquê, considere nossa `UpdateCommand`de SqlDataSource s:

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample6.sql)]

A coluna `UnitPrice` na tabela `Products` pode ter valores de `NULL`. Se um registro específico tiver um valor `NULL` para `UnitPrice`, a parte da cláusula `WHERE` `[UnitPrice] = @original_UnitPrice` será *sempre* avaliada como false porque `NULL = NULL` sempre retorna false. Portanto, `UPDATE` os registros que contêm `NULL` valores não podem ser editados ou excluídos, pois as cláusulas `WHERE` e `DELETE` não retornarão linhas a serem atualizadas ou excluídas.

> [!NOTE]
> Esse bug foi reportado pela primeira vez para a Microsoft em junho de 2004 em [SqlDataSource gera instruções SQL incorretas](https://connect.microsoft.com/VisualStudio/feedback/ViewFeedback.aspx?FeedbackID=93937) e está relatórios agendado para ser corrigido na próxima versão do ASP.net.

Para corrigir isso, precisamos atualizar manualmente as cláusulas `WHERE` nas propriedades `UpdateCommand` e `DeleteCommand` para **todas as** colunas que podem ter `NULL` valores. Em geral, altere `[ColumnName] = @original_ColumnName` para:

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample7.sql)]

Essa modificação pode ser feita diretamente por meio da marcação declarativa, por meio das opções UpdateQuery ou DeleteQuery da janela Propriedades ou pelas guias UPDATE e DELETE na opção especificar uma instrução SQL personalizada ou um procedimento armazenado em configurar dados Assistente de origem. Novamente, essa modificação deve ser feita para *cada* coluna na cláusula `UpdateCommand` e `DeleteCommand` s `WHERE` que podem conter valores de `NULL`.

Aplicar isso ao nosso exemplo resulta nos seguintes `UpdateCommand` modificados e valores de `DeleteCommand`:

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample8.sql)]

## <a name="step-2-adding-a-gridview-with-edit-and-delete-options"></a>Etapa 2: adicionando um GridView com opções de editar e excluir

Com o SqlDataSource configurado para dar suporte à simultaneidade otimista, tudo o que resta é adicionar um controle da Web de dados à página que utiliza esse controle de simultaneidade. Para este tutorial, vamos adicionar um GridView que fornece a funcionalidade de edição e exclusão. Para fazer isso, arraste um GridView da caixa de ferramentas para o designer e defina seu `ID` como `Products`. Na marca inteligente do GridView, associe-o ao controle `ProductsDataSourceWithOptimisticConcurrency` SqlDataSource adicionado na etapa 1. Por fim, marque a opção Habilitar edição e habilitar a exclusão na marca inteligente.

[![associar o GridView ao SqlDataSource e habilitar a edição e a exclusão](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image6.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image9.png)

**Figura 6**: associar o GridView ao SqlDataSource e habilitar a edição e a exclusão ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image10.png))

Depois de adicionar o GridView, configure sua aparência removendo o `ProductID` BoundField, alterando a propriedade `ProductName` BoundField s `HeaderText` para produto e atualizando o `UnitPrice` BoundField para que sua propriedade `HeaderText` seja simplesmente o preço. O ideal é aprimorar a interface de edição para incluir um RequiredFieldValidator para o valor de `ProductName` e um CompareValidator para o valor de `UnitPrice` (para garantir que ele seja um valor numérico corretamente formatado). Consulte o tutorial [Personalizando a interface de modificação de dados](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) para obter uma visão mais detalhada sobre como personalizar a interface de edição de GridView.

> [!NOTE]
> O estado de exibição de GridView deve ser habilitado, pois os valores originais passados do GridView para o SqlDataSource são armazenados no estado de exibição.

Depois de fazer essas modificações no GridView, a marcação declarativa GridView e SqlDataSource deve ser semelhante ao seguinte:

[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample9.aspx)]

Para ver o controle de simultaneidade otimista em ação, abra duas janelas do navegador e carregue a página `OptimisticConcurrency.aspx` em ambos. Clique nos botões de edição do primeiro produto em ambos os navegadores. Em um navegador, altere o nome do produto e clique em atualizar. O navegador será postback e o GridView retornará ao seu modo de pré-edição, mostrando o novo nome do produto para o registro que acabou de ser editado.

Na segunda janela do navegador, altere o preço (mas deixe o nome do produto como seu valor original) e clique em atualizar. No postback, a grade retorna ao seu modo de pré-edição, mas a alteração no preço não é registrada. O segundo navegador mostra o mesmo valor que o primeiro nome do novo produto com o preço antigo. As alterações feitas na segunda janela do navegador foram perdidas. Além disso, as alterações foram perdidas em silêncio, pois não havia nenhuma exceção ou mensagem indicando que ocorreu uma violação de simultaneidade.

[![as alterações na segunda janela do navegador foram silenciosamente perdidas](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image7.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image11.png)

**Figura 7**: as alterações na segunda janela do navegador foram silenciosamente perdidas ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image12.png))

O motivo pelo qual as alterações de s do segundo navegador não foram confirmadas era porque a cláusula `UPDATE` s `WHERE` filtrou todos os registros e, portanto, não afetou nenhuma linha. Vamos examinar a instrução `UPDATE` novamente:

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample10.sql)]

Quando a segunda janela do navegador atualiza o registro, o nome do produto original especificado na cláusula `WHERE` não corresponde ao nome do produto existente (já que ele foi alterado pelo primeiro navegador). Portanto, a instrução `[ProductName] = @original_ProductName` retorna false e a `UPDATE` não afeta nenhum registro.

> [!NOTE]
> Excluir funciona da mesma maneira. Com duas janelas do navegador abertas, comece editando um determinado produto com uma e, em seguida, salvando suas alterações. Depois de salvar as alterações em um navegador, clique no botão excluir para o mesmo produto no outro. Como os valores originais Don t correspondem na cláusula `DELETE` s `WHERE` instrução, a exclusão silenciosamente falha.

Da perspectiva do usuário final na segunda janela do navegador, depois de clicar no botão atualizar, a grade retorna para o modo de pré-edição, mas suas alterações foram perdidas. No entanto, não há nenhum comentário visual que suas alterações não tenham sido transformadas. Idealmente, se as alterações de s de um usuário forem perdidas em uma violação de simultaneidade, nós d os notificaremos e, talvez, mantenha a grade no modo de edição. Vamos ver como fazer isso.

## <a name="step-3-determining-when-a-concurrency-violation-has-occurred"></a>Etapa 3: Determinando quando ocorreu uma violação de simultaneidade

Como uma violação de simultaneidade rejeita as alterações feitas, seria bom alertar o usuário quando ocorreu uma violação de simultaneidade. Para alertar o usuário, vamos adicionar um controle rótulo da Web à parte superior da página chamada `ConcurrencyViolationMessage` cuja propriedade `Text` exibe a seguinte mensagem: você tentou atualizar ou excluir um registro que foi atualizado simultaneamente por outro usuário. Examine as alterações do outro usuário e refaça a atualização ou a exclusão. Defina o controle de rótulo s `CssClass` Propriedade como aviso, que é uma classe CSS definida em `Styles.css` que exibe o texto em uma fonte vermelha, itálico, negrito e grande. Por fim, defina o rótulo s `Visible` e `EnableViewState` propriedades como `False`. Isso ocultará o rótulo, exceto apenas os postbacks em que definimos explicitamente sua propriedade `Visible` como `True`.

[![adicionar um controle rótulo à página para exibir o aviso](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image8.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image13.png)

**Figura 8**: adicionar um controle rótulo à página para exibir o aviso ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image14.png))

Ao executar uma atualização ou exclusão, os manipuladores de eventos de `RowUpdated` e de `RowDeleted` GridView são acionados após o controle da fonte de dados ter executado a atualização ou exclusão solicitada. Podemos determinar quantas linhas foram afetadas pela operação desses manipuladores de eventos. Se nenhuma linha for afetada, queremos exibir o rótulo `ConcurrencyViolationMessage`.

Crie um manipulador de eventos para os eventos `RowUpdated` e `RowDeleted` e adicione o seguinte código:

[!code-vb[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample11.vb)]

Em ambos os manipuladores de eventos, verificamos a propriedade `e.AffectedRows` e, se for igual a 0, definiremos o rótulo `ConcurrencyViolationMessage` s `Visible` Propriedade como `True`. No manipulador de eventos `RowUpdated`, também instrumos o GridView a permanecer no modo de edição, definindo a propriedade `KeepInEditMode` como true. Ao fazer isso, precisamos reassociar os dados à grade para que os dados dos outros usuários sejam carregados na interface de edição. Isso é feito chamando o método GridView s `DataBind()`.

Como mostra a Figura 9, com esses dois manipuladores de eventos, uma mensagem muito perceptível é exibida sempre que ocorre uma violação de simultaneidade.

[![uma mensagem é exibida na face de uma violação de simultaneidade](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image9.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image15.png)

**Figura 9**: uma mensagem é exibida na face de uma violação de simultaneidade ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image16.png))

## <a name="summary"></a>Resumo

Ao criar um aplicativo Web em que vários usuários simultâneos podem estar editando os mesmos dados, é importante considerar as opções de controle de simultaneidade. Por padrão, os controles da Web de dados ASP.NET e os controles de fonte de dados não empregam nenhum controle de simultaneidade. Como vimos neste tutorial, a implementação do controle de simultaneidade otimista com o SqlDataSource é relativamente rápida e fácil. O SqlDataSource lida com a maioria das questões para a adição de cláusulas de `WHERE` aumentadas às instruções de `UPDATE` e `DELETE` geradas automaticamente, mas há algumas sutilezas na manipulação de colunas de valor `NULL`, conforme discutido na seção manipulando corretamente os valores de `NULL`.

Este tutorial conclui nosso exame do SqlDataSource. Nossos tutoriais restantes voltarão a trabalhar com dados usando o ObjectDataSource e a arquitetura em camadas.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](inserting-updating-and-deleting-data-with-the-sqldatasource-vb.md)
