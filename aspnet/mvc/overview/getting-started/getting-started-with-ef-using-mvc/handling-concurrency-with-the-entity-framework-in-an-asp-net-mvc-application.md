---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Tutorial: tratar a simultaneidade com o EF em um aplicativo ASP.NET MVC 5'
description: Este tutorial mostra como usar a simultaneidade otimista para lidar com conflitos quando vários usuários atualizam a mesma entidade ao mesmo tempo.
author: tdykstra
ms.author: riande
ms.topic: tutorial
ms.date: 01/15/2019
ms.assetid: be0c098a-1fb2-457e-b815-ddca601afc65
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 43c5fdff5601c9bff32300d3460de0079a498d28
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616105"
---
# <a name="tutorial-handle-concurrency-with-ef-in-an-aspnet-mvc-5-app"></a>Tutorial: tratar a simultaneidade com o EF em um aplicativo ASP.NET MVC 5

Nos tutoriais anteriores, você aprendeu a atualizar os dados. Este tutorial mostra como usar a simultaneidade otimista para lidar com conflitos quando vários usuários atualizam a mesma entidade ao mesmo tempo. Você altera as páginas da Web que funcionam com a entidade `Department` para que elas manipulem erros de simultaneidade. As ilustrações a seguir mostram as páginas Editar e Excluir, incluindo algumas mensagens exibidas se ocorre um conflito de simultaneidade.

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

Neste tutorial, você:

> [!div class="checklist"]
> * Aprender sobre conflitos de simultaneidade
> * Adicionar simultaneidade otimista
> * Modificar controlador do departamento
> * Manipulação de simultaneidade de teste
> * Atualizar a página Excluir

## <a name="prerequisites"></a>Prerequisites

* [Assíncrono e procedimentos armazenados](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="concurrency-conflicts"></a>Conflitos de simultaneidade

Um conflito de simultaneidade ocorre quando um usuário exibe dados de uma entidade para editá-los e, em seguida, outro usuário atualiza os mesmos dados da entidade antes que a primeira alteração do usuário seja gravada no banco de dados. Se você não habilitar a detecção desses conflitos, a última pessoa que atualizar o banco de dados substituirá as outras alterações do usuário. Em muitos aplicativos, esse risco é aceitável: se houver poucos usuários ou poucas atualizações ou se não for realmente crítico se algumas alterações forem substituídas, o custo de programação para simultaneidade poderá superar o benefício. Nesse caso, você não precisa configurar o aplicativo para lidar com conflitos de simultaneidade.

### <a name="pessimistic-concurrency-locking"></a>Simultaneidade pessimista (bloqueio)

Se o aplicativo precisar evitar a perda acidental de dados em cenários de simultaneidade, uma maneira de fazer isso será usar bloqueios de banco de dados. Isso é chamado de *simultaneidade pessimista*. Por exemplo, antes de ler uma linha de um banco de dados, você solicita um bloqueio para o acesso somente leitura ou de atualização. Se você bloquear uma linha para o acesso de atualização, nenhum outro usuário terá permissão para bloquear a linha para o acesso somente leitura ou de atualização, porque ele obterá uma cópia dos dados que estão sendo alterados. Se você bloquear uma linha para o acesso somente leitura, outros também poderão bloqueá-la para o acesso somente leitura, mas não para atualização.

O gerenciamento de bloqueios traz desvantagens. Ele pode ser complexo de ser programado. Exige recursos de gerenciamento de banco de dados significativos e pode causar problemas de desempenho, conforme o número de usuários de um aplicativo aumenta. Por esses motivos, nem todos os sistemas de gerenciamento de banco de dados dão suporte à simultaneidade pessimista. O Entity Framework não fornece suporte interno para ele, e este tutorial não mostra como implementá-lo.

### <a name="optimistic-concurrency"></a>Simultaneidade otimista

A alternativa à simultaneidade pessimista é a *simultaneidade otimista*. Simultaneidade otimista significa permitir que conflitos de simultaneidade ocorram e responder adequadamente se eles ocorrerem. Por exemplo, João executa a página Editar departamentos, altera o valor do **orçamento** do departamento em inglês de $350000 para $0.

Antes de João clicar em **salvar**, Jane executa a mesma página e altera o campo **data de início** de 9/1/2007 para 8/8/2013.

João clica em **salvar** primeiro e vê sua alteração quando o navegador retorna para a página de índice, então Jane clica em **salvar**. O que acontece em seguida é determinado pela forma como você lida com conflitos de simultaneidade. Algumas das opções incluem o seguinte:

- Controle qual propriedade um usuário modificou e atualize apenas as colunas correspondentes no banco de dados. No cenário de exemplo, nenhum dado é perdido, porque propriedades diferentes foram atualizadas pelos dois usuários. Na próxima vez que alguém procurar o departamento em inglês, ele verá as alterações de João e Jane — uma data de início de 8/8/2013 e um orçamento de zero dólares.

    Esse método de atualização pode reduzir a quantidade de conflitos que podem resultar em perda de dados, mas ele não poderá evitar a perda de dados se forem feitas alterações concorrentes à mesma propriedade de uma entidade. Se o Entity Framework funciona dessa maneira depende de como o código de atualização é implementado. Geralmente, isso não é prático em um aplicativo Web, porque pode exigir que você mantenha grandes quantidades de estado para manter o controle de todos os valores de propriedade originais de uma entidade, bem como novos valores. A manutenção de grandes quantidades de estado pode afetar o desempenho do aplicativo, pois exige recursos do servidor ou deve ser incluída na própria página da Web (por exemplo, em campos ocultos) ou em um cookie.
- Você pode deixar a alteração de Jane substituir a alteração de João. Na próxima vez que alguém procurar o departamento em inglês, ele verá 8/8/2013 e o valor $350000 restaurado. Isso é chamado de um cenário *O cliente vence* ou *O último vence*. (Todos os valores do cliente têm precedência sobre o que está no armazenamento de dados.) Conforme observado na introdução a esta seção, se você não fizer qualquer codificação para manipulação de simultaneidade, isso ocorrerá automaticamente.
- Você pode impedir que a alteração de Jane seja atualizada no banco de dados. Normalmente, você exibiria uma mensagem de erro, mostrará o estado atual dos dados e permitirá que ela reaplique as alterações se ainda quiser fazê-las. Isso é chamado de um cenário *O armazenamento vence*. (Os valores de armazenamento de dados têm precedência sobre os valores enviados pelo cliente.) Você implementará o cenário armazenar vence neste tutorial. Esse método garante que nenhuma alteração é substituída sem que um usuário seja alertado sobre o que está acontecendo.

### <a name="detecting-concurrency-conflicts"></a>Detectando conflitos de simultaneidade

Você pode resolver conflitos manipulando exceções [OptimisticConcurrencyException](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) que o Entity Framework gera. Para saber quando gerar essas exceções, o Entity Framework precisa poder detectar conflitos. Portanto, é necessário configurar o banco de dados e o modelo de dados de forma adequada. Algumas opções para habilitar a detecção de conflitos incluem as seguintes:

- Na tabela de banco de dados, inclua uma coluna de acompanhamento que pode ser usada para determinar quando uma linha é alterada. Em seguida, você pode configurar o Entity Framework para incluir essa coluna na cláusula `Where` dos comandos SQL `Update` ou `Delete`.

    Normalmente, o tipo de dados da coluna de rastreamento é a [versão](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)do. O [valor de](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) RowNumber é um número sequencial que é incrementado toda vez que a linha é atualizada. Em um comando `Update` ou `Delete`, a cláusula `Where` inclui o valor original da coluna de rastreamento (a versão da linha original). Se a linha que está sendo atualizada tiver sido alterada por outro usuário, o valor na coluna `rowversion` será diferente do valor original, portanto, a instrução `Update` ou `Delete` não poderá localizar a linha a ser atualizada devido à cláusula `Where`. Quando o Entity Framework descobre que nenhuma linha foi atualizada pelo `Update` ou `Delete` comando (ou seja, quando o número de linhas afetadas é zero), ele interpreta isso como um conflito de simultaneidade.
- Configure o Entity Framework para incluir os valores originais de cada coluna na tabela na cláusula `Where` dos comandos `Update` e `Delete`.

    Como na primeira opção, se qualquer coisa na linha tiver sido alterada desde que a linha foi lida pela primeira vez, a cláusula `Where` não retornará uma linha a ser atualizada, que o Entity Framework interpretará como um conflito de simultaneidade. Para tabelas de banco de dados que têm muitas colunas, essa abordagem pode resultar em cláusulas de `Where` muito grandes e pode exigir que você mantenha grandes quantidades de estado. Conforme observado anteriormente, manter grandes quantidades de estado pode afetar o desempenho do aplicativo. Portanto, essa abordagem geralmente não é recomendada e não é o método usado neste tutorial.

    Se você quiser implementar essa abordagem para simultaneidade, será necessário marcar todas as propriedades de chave não primária na entidade para a qual você deseja acompanhar a simultaneidade adicionando o atributo [ConcurrencyCheck](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx) a elas. Essa alteração permite que a Entity Framework inclua todas as colunas na cláusula SQL `WHERE` de instruções `UPDATE`.

No restante deste tutorial, você adicionará uma propriedade de controle do [Multiversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) à entidade `Department`, criará um controlador e exibições e testará para verificar se tudo está funcionando corretamente.

## <a name="add-optimistic-concurrency"></a>Adicionar simultaneidade otimista

Em *Models\Department.cs*, adicione uma propriedade de rastreamento chamada `RowVersion`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=20-22)]

O atributo [timestamp](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) especifica que essa coluna será incluída na cláusula `Where` dos comandos `Update` e `Delete` enviados ao banco de dados. O atributo é chamado [timestamp](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) porque as versões anteriores do SQL Server usavam um tipo de dados [timestamp](https://msdn.microsoft.com/library/ms182776(v=SQL.90).aspx) SQL antes que a [versão](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) do SQL seja substituída. O tipo .net para o de série é uma [matriz de bytes](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) .

Se preferir usar a API fluente, você poderá usar o método [IsConcurrencyToken](https://msdn.microsoft.com/library/gg679501(v=VS.103).aspx) para especificar a propriedade de rastreamento, conforme mostrado no exemplo a seguir:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Adicionando uma propriedade, você alterou o modelo de banco de dados e, portanto, precisa fazer outra migração. No PMC (Console do Gerenciador de Pacotes), Insira os seguintes comandos:

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="modify-department-controller"></a>Modificar controlador do departamento

Em *Controllers\DepartmentController.cs*, adicione uma instrução `using`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

No arquivo *DepartmentController.cs* , altere todas as quatro ocorrências de "LastName" para "FullName" para que as listas suspensas do administrador do departamento contenham o nome completo do instrutor, em vez de apenas o sobrenome.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

Substitua o código existente para o método `HttpPost` `Edit` pelo seguinte código:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Se o método `FindAsync` retornar nulo, isso indicará que o departamento foi excluído por outro usuário. O código mostrado usa os valores de formulário postados para criar uma entidade de departamento para que a página de edição possa ser exibida novamente com uma mensagem de erro. Como alternativa, você não precisará recriar a entidade de departamento se exibir apenas uma mensagem de erro sem exibir novamente os campos de departamento.

A exibição armazena o valor de `RowVersion` original em um campo oculto e o método o recebe no parâmetro `rowVersion`. Antes de chamar `SaveChanges`, você precisa colocar isso no valor da propriedade `RowVersion` original na coleção `OriginalValues` da entidade. Em seguida, quando o Entity Framework cria um comando SQL `UPDATE`, esse comando incluirá uma cláusula `WHERE` que procura uma linha que tenha o valor de `RowVersion` original.

Se nenhuma linha for afetada pelo comando de `UPDATE` (nenhuma linha tem o valor de `RowVersion` original), a Entity Framework lançará uma exceção de `DbUpdateConcurrencyException` e o código no bloco de `catch` obterá a entidade de `Department` afetada do objeto de exceção.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Esse objeto tem os novos valores inseridos pelo usuário em sua propriedade `Entity`, e você pode obter os valores lidos do banco de dados chamando o método `GetDatabaseValues`.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

O método `GetDatabaseValues` retornará NULL se alguém tiver excluído a linha do banco de dados; caso contrário, você precisará converter o objeto retornado para a classe `Department` para acessar as propriedades `Department`. (Como você já verificou a exclusão, `databaseEntry` seria nulo somente se o departamento foi excluído depois que `FindAsync` é executado e antes de `SaveChanges` ser executado.)

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Em seguida, o código adiciona uma mensagem de erro personalizada para cada coluna que tem valores de banco de dados diferentes do que o usuário inseriu na página de edição:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Uma mensagem de erro mais longa explica o que aconteceu e o que fazer sobre ele:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Por fim, o código define o valor `RowVersion` do objeto `Department` para o novo valor recuperado do banco de dados. Esse novo valor `RowVersion` será armazenado no campo oculto quando a página Editar for exibida novamente, e na próxima vez que o usuário clicar em **Salvar**, somente os erros de simultaneidade que ocorrem desde a nova exibição da página Editar serão capturados.

Em *Views\Department\Edit.cshtml*, adicione um campo oculto para salvar o valor da propriedade `RowVersion`, imediatamente após o campo oculto para a propriedade `DepartmentID`:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=18)]

## <a name="test-concurrency-handling"></a>Manipulação de simultaneidade de teste

Execute o site e clique em **departamentos**.

Clique com o botão direito do mouse no hiperlink **Editar** para o departamento em inglês e selecione **abrir na nova guia e,** em seguida, clique no hiperlink **Editar** para o departamento em inglês. As duas guias exibem as mesmas informações.

Altere um campo na primeira guia do navegador e clique em **Salvar**.

O navegador mostra a página Índice com o valor alterado.

Altere um campo na segunda guia do navegador e clique em **salvar**. Você verá uma mensagem de erro:

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Clique em **Salvar** novamente. O valor que você inseriu na segunda guia do navegador é salvo junto com o valor original dos dados que você alterou no primeiro navegador. Você verá os valores salvos quando a página Índice for exibida.

## <a name="update-the-delete-page"></a>Atualizar a página Excluir

Para a página Excluir, o Entity Framework detecta conflitos de simultaneidade causados pela edição por outra pessoa do departamento de maneira semelhante. Quando o `HttpGet` método `Delete` exibe o modo de exibição de confirmação, o modo de exibição inclui o valor `RowVersion` original em um campo oculto. Esse valor é disponibilizado para o método de `Delete` de `HttpPost` que é chamado quando o usuário confirma a exclusão. Quando o Entity Framework cria o comando SQL `DELETE`, ele inclui uma cláusula `WHERE` com o valor original `RowVersion`. Se o comando resultar em zero linhas afetadas (o que significa que a linha foi alterada após a exibição da página de confirmação de exclusão), uma exceção de simultaneidade será gerada e o método `HttpGet Delete` será chamado com um sinalizador de erro definido como `true` para reexibir a página de confirmação com uma mensagem de erro. Também é possível que zero linhas tenham sido afetadas porque a linha foi excluída por outro usuário; portanto, nesse caso, uma mensagem de erro diferente é exibida.

No *DepartmentController.cs*, substitua o método `HttpGet` `Delete` pelo seguinte código:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

O método aceita um parâmetro opcional que indica se a página está sendo exibida novamente após um erro de simultaneidade. Se esse sinalizador for `true`, uma mensagem de erro será enviada para a exibição usando uma propriedade `ViewBag`.

Substitua o código no método `HttpPost` `Delete` (chamado `DeleteConfirmed`) pelo seguinte código:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

No código gerado por scaffolding que acabou de ser substituído, esse método aceitou apenas uma ID de registro:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Você alterou esse parâmetro para uma instância de entidade `Department` criada pelo associador de modelo. Isso lhe dá acesso ao valor da propriedade `RowVersion` além da chave de registro.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Você também alterou o nome do método de ação de `DeleteConfirmed` para `Delete`. O código com Scaffold chamado `HttpPost` método `Delete` `DeleteConfirmed` para dar ao método `HttpPost` uma assinatura exclusiva. (O CLR requer métodos sobrecarregados para ter parâmetros de método diferentes.) Agora que as assinaturas são exclusivas, você pode se manter com a Convenção MVC e usar o mesmo nome para os métodos `HttpPost` e `HttpGet` Delete.

Se um erro de simultaneidade é capturado, o código exibe novamente a página Confirmação de exclusão e fornece um sinalizador que indica que ela deve exibir uma mensagem de erro de simultaneidade.

No *Views\Department\Delete.cshtml*, substitua o código com Scaffold pelo código a seguir que adiciona um campo de mensagem de erro e os campos ocultos para as propriedades DepartmentID e da IsVersion. As alterações são realçadas.

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml?highlight=9-10,21,52-54)]

Esse código adiciona uma mensagem de erro entre o `h2` e os cabeçalhos de `h3`:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Ele substitui `LastName` por `FullName` no campo `Administrator`:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Por fim, ele adiciona campos ocultos para as propriedades `DepartmentID` e `RowVersion` após a instrução `Html.BeginForm`:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cshtml)]

Execute a página de índice de departamentos. Clique com o botão direito do mouse no hiperlink **excluir** do departamento em inglês e selecione **abrir na nova guia e,** na primeira guia, clique no hiperlink **Editar** para o departamento em inglês.

Na primeira janela, altere um dos valores e clique em **salvar**.

A página de índice confirma a alteração.

Na segunda guia, clique em **Excluir**.

Você verá a mensagem de erro de simultaneidade e os valores de Departamento serão atualizados com o que está atualmente no banco de dados.

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

Se você clicar em **Excluir** novamente, será redirecionado para a página Índice, que mostra que o departamento foi excluído.

## <a name="get-the-code"></a>Obter o código

[Baixar projeto concluído](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Recursos adicionais

Links para outros recursos de Entity Framework podem ser encontrados nos [recursos de acesso a dados ASP.net-recomendado](../../../../whitepapers/aspnet-data-access-content-map.md).

Para obter informações sobre outras maneiras de lidar com vários cenários de simultaneidade, consulte [padrões de simultaneidade otimista](https://msdn.microsoft.com/data/jj592904) e [trabalhando com valores de propriedade](https://msdn.microsoft.com/data/jj592677) no msdn. O próximo tutorial mostra como implementar a herança de tabela por hierarquia para as entidades `Instructor` e `Student`.

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você:

> [!div class="checklist"]
> * Aprendeu sobre conflitos de simultaneidade
> * Adição de simultaneidade otimista
> * Controlador de departamento modificado
> * Manipulação de simultaneidade testada
> * Atualizou a página Excluir

Avance para o próximo artigo para saber como implementar a herança no modelo de dados.
> [!div class="nextstepaction"]
> [Implementar a herança no modelo de dados](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
