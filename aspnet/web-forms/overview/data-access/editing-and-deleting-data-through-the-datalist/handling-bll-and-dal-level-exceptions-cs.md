---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-cs
title: Manipulando exceções de nível de BLL e DALC#() | Microsoft Docs
author: rick-anderson
description: Neste tutorial, veremos como tactfully tratar exceções geradas durante um fluxo de trabalho de atualização de DataList editável.
ms.author: riande
ms.date: 10/30/2006
ms.assetid: f8fd58e2-f932-4f08-ab3d-fbf8ff3295d2
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-cs
msc.type: authoredcontent
ms.openlocfilehash: 35ff60be6ed67ea8d1bf226ae70f590100597757
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78610946"
---
# <a name="handling-bll--and-dal-level-exceptions-c"></a>Tratar de exceções de nível BLL e DAL (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_38_CS.exe) ou [baixar PDF](handling-bll-and-dal-level-exceptions-cs/_static/datatutorial38cs1.pdf)

> Neste tutorial, veremos como tactfully tratar exceções geradas durante um fluxo de trabalho de atualização de DataList editável.

## <a name="introduction"></a>Introdução

Na [visão geral da edição e exclusão de dados no tutorial DataList](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md) , criamos um DataList que oferecia recursos simples de edição e exclusão. Embora seja totalmente funcional, ele não era amigável para o usuário, pois qualquer erro ocorrido durante o processo de edição ou exclusão resultou em uma exceção sem tratamento. Por exemplo, omitir o nome do produto ou, ao editar um produto, inserir um valor de preço muito acessível!, gera uma exceção. Como essa exceção não é capturada no código, ela se parece com o tempo de execução ASP.NET, que exibe os detalhes da exceção na página da Web.

Como vimos na manipulação de [exceções de nível de BLL e Dal em um tutorial de página do ASP.net](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) , se uma exceção for gerada a partir das profundidades da lógica de negócios ou das camadas de acesso a dados, os detalhes da exceção serão retornados para o ObjectDataSource e, em seguida, para o GridView. Vimos como lidar normalmente com essas exceções criando `Updated` ou `RowUpdated` manipuladores de eventos para ObjectDataSource ou GridView, verificando uma exceção e, em seguida, indicando que a exceção foi tratada.

No entanto, nossos tutoriais de DataList não estão usando o ObjectDataSource para atualizar e excluir dados. Em vez disso, estamos trabalhando diretamente em relação à BLL. Para detectar exceções provenientes da BLL ou da DAL, precisamos implementar o código de tratamento de exceção dentro do code-behind de nossa página ASP.NET. Neste tutorial, veremos como mais tactfully tratar exceções geradas durante um fluxo de trabalho de atualização de DataList s editável.

> [!NOTE]
> Na *visão geral da edição e exclusão de dados no tutorial DataList* , discutimos técnicas diferentes para editar e excluir dados do DataList, algumas técnicas envolvidas usando um ObjectDataSource para atualização e exclusão. Se você empregar essas técnicas, poderá manipular exceções da BLL ou da DAL por meio dos manipuladores de eventos ObjectDataSource s `Updated` ou `Deleted`.

## <a name="step-1-creating-an-editable-datalist"></a>Etapa 1: Criando um DataList editável

Antes de nos preocuparmos em manipular exceções que ocorrem durante o fluxo de trabalho de atualização, vamos primeiro criar um DataList editável. Abra a página `ErrorHandling.aspx` na pasta `EditDeleteDataList`, adicione um DataList ao designer, defina sua propriedade `ID` como `Products`e adicione um novo ObjectDataSource chamado `ProductsDataSource`. Configure o ObjectDataSource para usar o método de `GetProducts()` da classe `ProductsBLL` para selecionar registros; Defina as listas suspensas nas guias inserir, atualizar e excluir como (nenhum).

[![retornar as informações do produto usando o método GetProducts ()](handling-bll-and-dal-level-exceptions-cs/_static/image2.png)](handling-bll-and-dal-level-exceptions-cs/_static/image1.png)

**Figura 1**: retornar as informações do produto usando o método `GetProducts()` ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-cs/_static/image3.png))

Depois de concluir o assistente ObjectDataSource, o Visual Studio criará automaticamente um `ItemTemplate` para o DataList. Substitua isso por um `ItemTemplate` que exibe o nome e o preço de cada produto e inclui um botão de edição. Em seguida, crie um `EditItemTemplate` com um controle de caixa de texto da Web para os botões nome e preço e atualizar e cancelar. Por fim, defina a propriedade DataList s `RepeatColumns` como 2.

Após essas alterações, a marcação declarativa de s de página deve ser semelhante à seguinte. Clique duas vezes para certificar-se de que os botões editar, cancelar e atualizar tenham suas propriedades `CommandName` definidas como editar, cancelar e atualizar, respectivamente.

[!code-aspx[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample1.aspx)]

> [!NOTE]
> Para este tutorial, o estado de exibição DataList s deve ser habilitado.

Reserve um tempo para exibir nosso progresso por meio de um navegador (veja a Figura 2).

[![cada produto inclui um botão de edição](handling-bll-and-dal-level-exceptions-cs/_static/image5.png)](handling-bll-and-dal-level-exceptions-cs/_static/image4.png)

**Figura 2**: cada produto inclui um botão de edição ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-cs/_static/image6.png))

Atualmente, o botão Editar só causa um postback que ainda não torna o produto editável. Para habilitar a edição, precisamos criar manipuladores de eventos para os eventos DataList s `EditCommand`, `CancelCommand`e `UpdateCommand`. Os eventos `EditCommand` e `CancelCommand` simplesmente atualizam a propriedade DataList s `EditItemIndex` e reassociam os dados ao DataList:

[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample2.cs)]

O manipulador de eventos `UpdateCommand` é um pouco mais envolvido. Ele precisa ler o `ProductID` do produto editado na coleção de `DataKeys` junto com o nome e o preço do produto das caixas de Textno `EditItemTemplate`e, em seguida, chamar o método de `UpdateProduct` da classe `ProductsBLL` antes de retornar o DataList ao estado de pré-edição.

Por enquanto, vamos usar exatamente o mesmo código do manipulador de eventos `UpdateCommand` na *visão geral da edição e exclusão de dados no tutorial DataList* . Vamos adicionar o código para tratar exceções normalmente na etapa 2.

[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample3.cs)]

Diante de uma entrada inválida que pode estar na forma de um preço unitário formatado incorretamente, um valor de preço unitário ilegal como-$5 ou a omissão do nome do produto s, uma exceção será gerada. Como o manipulador de eventos `UpdateCommand` não inclui nenhum código de tratamento de exceção neste ponto, a exceção será propagada até o tempo de execução ASP.NET, onde ele será exibido para o usuário final (consulte a Figura 3).

![Quando ocorre uma exceção sem tratamento, o usuário final vê uma página de erro](handling-bll-and-dal-level-exceptions-cs/_static/image7.png)

**Figura 3**: quando ocorre uma exceção sem tratamento, o usuário final vê uma página de erro

## <a name="step-2-gracefully-handling-exceptions-in-the-updatecommand-event-handler"></a>Etapa 2: manipulando exceções normalmente no manipulador de eventos UpdateCommand

Durante o fluxo de trabalho de atualização, as exceções podem ocorrer no manipulador de eventos `UpdateCommand`, na BLL ou na DAL. Por exemplo, se um usuário inserir um preço muito caro, a instrução `Decimal.Parse` no manipulador de eventos `UpdateCommand` gerará uma exceção `FormatException`. Se o usuário omitir o nome do produto ou se o preço tiver um valor negativo, a DAL emitirá uma exceção.

Quando ocorre uma exceção, queremos exibir uma mensagem informativa dentro da própria página. Adicione um controle de rótulo da Web à página cujo `ID` está definido como `ExceptionDetails`. Configure o texto do rótulo para exibir em uma fonte vermelha, extra grande, negrito e itálico atribuindo sua propriedade `CssClass` à classe CSS `Warning`, que é definida no arquivo `Styles.css`.

Quando ocorre um erro, queremos apenas que o rótulo seja exibido uma vez. Ou seja, em postbacks subsequentes, a mensagem de aviso do rótulo s deve desaparecer. Isso pode ser feito desmarcando o rótulo s `Text` propriedade ou configurações sua propriedade `Visible` como `False` no manipulador de eventos de `Page_Load` (como fizemos de volta no [tratamento de exceções de nível de BLL e Dal em um tutorial de página ASP.net](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) ) ou desabilitando o rótulo s Exibir suporte ao estado. Deixe que os s usem a última opção.

[!code-aspx[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample4.aspx)]

Quando uma exceção é gerada, atribuíremos os detalhes da exceção à propriedade do `ExceptionDetails` controle de rótulo s `Text`. Como seu estado de exibição é desabilitado, nos postbacks subsequentes as alterações programáticas de propriedade de `Text` s serão perdidas, revertendo para o texto padrão (uma cadeia de caracteres vazia), ocultando assim a mensagem de aviso.

Para determinar quando um erro foi gerado para exibir uma mensagem útil na página, precisamos adicionar um bloco de `Try ... Catch` ao manipulador de eventos `UpdateCommand`. A parte `Try` contém código que pode levar a uma exceção, enquanto o bloco de `Catch` contém código que é executado na face de uma exceção. Confira a seção [conceitos básicos de manipulação de exceções](https://msdn.microsoft.com/library/2w8f0bss.aspx) na documentação do .NET Framework para obter mais informações sobre o bloco de `Try ... Catch`.

[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample5.cs)]

Quando uma exceção de qualquer tipo é gerada pelo código dentro do bloco de `Try`, o código s do bloco de `Catch` começará a ser executado. O tipo de exceção que é gerada `DbException`, `NoNullAllowedException`, `ArgumentException`e assim por diante depende do que, exatamente, precipitou o erro em primeiro lugar. Se houver um problema no nível do banco de dados, um `DbException` será gerado. Se um valor ilegal for inserido para os campos `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`ou `ReorderLevel`, um `ArgumentException` será gerado, à medida que adicionamos o código para validar esses valores de campo na classe `ProductsDataTable` (consulte o tutorial [criando uma camada de lógica de negócios](../introduction/creating-a-business-logic-layer-cs.md) ).

Podemos fornecer uma explicação mais útil para o usuário final, baseando o texto da mensagem no tipo de exceção detectada. O código a seguir, que foi usado de forma praticamente idêntica no [tratamento de exceções de nível de BLL e Dal em um tutorial de página do ASP.net](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) , fornece esse nível de detalhe:

[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample6.cs)]

Para concluir este tutorial, basta chamar o método `DisplayExceptionDetails` do bloco de `Catch` passando pela instância `Exception` capturada (`ex`).

Com o bloco de `Try ... Catch` em vigor, os usuários recebem uma mensagem de erro mais informativa, conforme as figuras 4 e 5 são mostradas. Observe que, diante de uma exceção, o DataList permanece no modo de edição. Isso ocorre porque, após a exceção, o fluxo de controle é redirecionado imediatamente para o bloco de `Catch`, ignorando o código que retorna o DataList para seu estado de pré-edição.

[![uma mensagem de erro será exibida se um usuário omitir um campo obrigatório](handling-bll-and-dal-level-exceptions-cs/_static/image9.png)](handling-bll-and-dal-level-exceptions-cs/_static/image8.png)

**Figura 4**: uma mensagem de erro será exibida se um usuário omitir um campo necessário ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-cs/_static/image10.png))

[![uma mensagem de erro é exibida ao inserir um preço negativo](handling-bll-and-dal-level-exceptions-cs/_static/image12.png)](handling-bll-and-dal-level-exceptions-cs/_static/image11.png)

**Figura 5**: uma mensagem de erro é exibida ao inserir um preço negativo ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-cs/_static/image13.png))

## <a name="summary"></a>Resumo

O GridView e o ObjectDataSource fornecem manipuladores de eventos de nível posterior que incluem informações sobre quaisquer exceções que foram geradas durante o fluxo de trabalho de atualização e exclusão, bem como propriedades que podem ser definidas para indicar se a exceção foi ou não cargo. No entanto, esses recursos não estão disponíveis ao trabalhar com o DataList e usar a BLL diretamente. Em vez disso, somos responsáveis por implementar a manipulação de exceções.

Neste tutorial, vimos como adicionar tratamento de exceções a um fluxo de trabalho de atualização do DataList s editável adicionando um bloco de `Try ... Catch` ao manipulador de eventos `UpdateCommand`. Se uma exceção for gerada durante o fluxo de trabalho de atualização, o código s do bloco de `Catch` será executado, exibindo informações úteis no rótulo de `ExceptionDetails`.

Neste ponto, o DataList não faz nenhum esforço para evitar que exceções ocorram em primeiro lugar. Mesmo que saibamos que um preço negativo resultará em uma exceção, ainda não adicionamos nenhuma funcionalidade para impedir que um usuário insira tal entrada inválida. Em nosso próximo tutorial, veremos como ajudar a reduzir as exceções causadas por entrada de usuário inválida adicionando controles de validação no `EditItemTemplate`.

Boa programação!

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Diretrizes de design para exceções](https://msdn.microsoft.com/library/ms298399.aspx)
- [Módulos e manipuladores de log de erros (ELMAH)](http://workspaces.gotdotnet.com/elmah) (uma biblioteca de código aberto para registrar erros)
- [Enterprise Library para .NET Framework 2,0](https://www.microsoft.com/downloads/details.aspx?familyid=5A14E870-406B-4F2A-B723-97BA84AE80B5&amp;displaylang=en) (inclui o bloco de aplicativo de gerenciamento de exceções)

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. O revisor de cliente potencial para este tutorial foi Ken Pespisa. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](performing-batch-updates-cs.md)
> [Próximo](adding-validation-controls-to-the-datalist-s-editing-interface-cs.md)
