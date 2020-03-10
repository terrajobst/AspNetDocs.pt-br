---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
title: Lidando com a simultaneidade com o Entity Framework 4,0 em um aplicativo Web ASP.NET 4 | Microsoft Docs
author: tdykstra
description: Esta série de tutoriais se baseia no aplicativo Web da Contoso University criado pelo Introdução com a série de tutoriais do Entity Framework 4,0. I...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: a5aa22a6-fb7f-4f41-9c7f-addda151940b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: 3df5f7d9c8fb22e1ea34fe16560bdb9a1309bb56
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78632583"
---
# <a name="handling-concurrency-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Lidando com a simultaneidade com o Entity Framework 4,0 em um aplicativo Web ASP.NET 4

por [Tom Dykstra](https://github.com/tdykstra)

> Esta série de tutoriais se baseia no aplicativo Web da Contoso University criado pelo [introdução com a série de tutoriais do Entity Framework 4,0](https://asp.net/entity-framework/tutorials#Getting%20Started) . Se você não concluiu os tutoriais anteriores, como ponto de partida para este tutorial, você pode [baixar o aplicativo](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que você criou. Você também pode [baixar o aplicativo](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) que é criado pela série de tutoriais completa. Se você tiver dúvidas sobre os tutoriais, poderá lançá-los no [Fórum de Entity Framework ASP.net](https://forums.asp.net/1227.aspx).

No tutorial anterior, você aprendeu como classificar e filtrar dados usando o controle de `ObjectDataSource` e o Entity Framework. Este tutorial mostra opções para lidar com a simultaneidade em um aplicativo Web ASP.NET que usa o Entity Framework. Você criará uma nova página da Web dedicada à atualização de atribuições de escritório do instrutor. Você tratará problemas de simultaneidade nessa página e na página departamentos que você criou anteriormente.

[![Image06](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

[![Image01](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

## <a name="concurrency-conflicts"></a>Conflitos de simultaneidade

Um conflito de simultaneidade ocorre quando um usuário edita um registro e outro usuário edita o mesmo registro antes que a alteração do primeiro usuário seja gravada no banco de dados. Se você não configurar o Entity Framework para detectar tais conflitos, quem atualizará o banco de dados por último substitui as alterações do outro usuário. Em muitos aplicativos, esse risco é aceitável e você não precisa configurar o aplicativo para lidar com possíveis conflitos de simultaneidade. (Se houver poucos usuários, ou poucas atualizações, ou se não for realmente crítico se algumas alterações forem substituídas, o custo da programação para simultaneidade poderá superar o benefício.) Se você não precisar se preocupar com conflitos de simultaneidade, poderá ignorar este tutorial; os dois tutoriais restantes da série não dependem de nada que você crie neste.

### <a name="pessimistic-concurrency-locking"></a>Simultaneidade pessimista (bloqueio)

Se o aplicativo precisar evitar a perda acidental de dados em cenários de simultaneidade, uma maneira de fazer isso será usar bloqueios de banco de dados. Isso é chamado de *simultaneidade pessimista*. Por exemplo, antes de ler uma linha de um banco de dados, você solicita um bloqueio para o acesso somente leitura ou de atualização. Se você bloquear uma linha para o acesso de atualização, nenhum outro usuário terá permissão para bloquear a linha para o acesso somente leitura ou de atualização, porque ele obterá uma cópia dos dados que estão sendo alterados. Se você bloquear uma linha para o acesso somente leitura, outros também poderão bloqueá-la para o acesso somente leitura, mas não para atualização.

O gerenciamento de bloqueios tem algumas desvantagens. Ele pode ser complexo de ser programado. Ele requer recursos de gerenciamento de banco de dados significativos e pode causar problemas de desempenho à medida que o número de usuários de um aplicativo aumenta (ou seja, ele não é bem dimensionado). Por esses motivos, nem todos os sistemas de gerenciamento de banco de dados dão suporte à simultaneidade pessimista. O Entity Framework não fornece suporte interno para ele, e este tutorial não mostra como implementá-lo.

### <a name="optimistic-concurrency"></a>Simultaneidade otimista

A alternativa à simultaneidade pessimista é a *simultaneidade otimista*. Simultaneidade otimista significa permitir que conflitos de simultaneidade ocorram e responder adequadamente se eles ocorrerem. Por exemplo, João executa a página *Department. aspx* , clica no link **Editar** para o departamento do histórico e reduz o valor do **orçamento** de $1000000 para $125000. (João administra um departamento de concorrentes e deseja liberar dinheiro para seu próprio departamento.)

[![Image07](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

Antes de João clicar em **Atualizar**, Jane executa a mesma página, clica no link **Editar** para o departamento do histórico e, em seguida, altera o campo **data de início** de 1/10/2011 para 1/1/1999. (Jane administra o departamento de histórico e quer dar mais tempo de ti).

[![Image08](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

João clica em **Atualizar** primeiro e Jane clica em **Atualizar**. O navegador de Jane agora lista o valor de **orçamento** como $1000000, mas isso está incorreto porque o valor foi alterado por joão para $125000.

Algumas das ações que você pode executar neste cenário incluem o seguinte:

- Controle qual propriedade um usuário modificou e atualize apenas as colunas correspondentes no banco de dados. No cenário de exemplo, nenhum dado é perdido, porque propriedades diferentes foram atualizadas pelos dois usuários. Na próxima vez que alguém procurar o departamento de histórico, ele verá 1/1/1999 e $125000. 

    Esse é o comportamento padrão no Entity Framework e pode reduzir substancialmente o número de conflitos que podem resultar em perda de dados. No entanto, esse comportamento não evita a perda de dados se forem feitas alterações concorrentes na mesma propriedade de uma entidade. Além disso, esse comportamento nem sempre é possível; Quando você mapeia procedimentos armazenados para um tipo de entidade, todas as propriedades de uma entidade são atualizadas quando qualquer alteração na entidade é feita no banco de dados.
- Você pode deixar a alteração de Jane substituir a alteração de João. Depois que Jane clica em **Atualizar**, o valor do **orçamento** volta para $1000000. Isso é chamado de um cenário *O cliente vence* ou *O último vence*. (Os valores do cliente têm precedência sobre o que está no armazenamento de dados.)
- Você pode impedir que a alteração de Jane seja atualizada no banco de dados. Normalmente, você exibiria uma mensagem de erro, mostrará o estado atual dos dados e permitirá que ela reinsira as alterações se ainda quiser fazê-las. Você pode automatizar ainda mais o processo salvando sua entrada e dando a ela uma oportunidade de reaplicá-la sem precisar reinseri-la. Isso é chamado de um cenário *O armazenamento vence*. (Os valores do armazenamento de dados têm precedência sobre os valores enviados pelo cliente.)

### <a name="detecting-concurrency-conflicts"></a>Detectando conflitos de simultaneidade

No Entity Framework, você pode resolver conflitos manipulando `OptimisticConcurrencyException` exceções que o Entity Framework gera. Para saber quando gerar essas exceções, o Entity Framework precisa poder detectar conflitos. Portanto, é necessário configurar o banco de dados e o modelo de dados de forma adequada. Algumas opções para habilitar a detecção de conflitos incluem as seguintes:

- No banco de dados, inclua uma coluna de tabela que pode ser usada para determinar quando uma linha foi alterada. Em seguida, você pode configurar o Entity Framework para incluir essa coluna na cláusula `Where` dos comandos SQL `Update` ou `Delete`.

    Essa é a finalidade da coluna `Timestamp` na tabela `OfficeAssignment`.

    [![Image09](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

    O tipo de dados da coluna `Timestamp` também é chamado de `Timestamp`. No entanto, a coluna na verdade não contém um valor de data ou hora. Em vez disso, o valor é um número sequencial incrementado cada vez que a linha é atualizada. Em um comando `Update` ou `Delete`, a cláusula `Where` inclui o valor original `Timestamp`. Se a linha que está sendo atualizada tiver sido alterada por outro usuário, o valor em `Timestamp` será diferente do valor original, portanto, a cláusula `Where` não retornará nenhuma linha para atualizar. Quando o Entity Framework descobre que nenhuma linha foi atualizada pelo comando `Update` ou `Delete` atual (ou seja, quando o número de linhas afetadas é zero), ele interpreta isso como um conflito de simultaneidade.
- Configure o Entity Framework para incluir os valores originais de cada coluna na tabela na cláusula `Where` dos comandos `Update` e `Delete`.

    Como na primeira opção, se qualquer coisa na linha tiver sido alterada desde que a linha foi lida pela primeira vez, a cláusula `Where` não retornará uma linha a ser atualizada, que o Entity Framework interpretará como um conflito de simultaneidade. Esse método é tão eficaz quanto usar um campo de `Timestamp`, mas pode ser ineficiente. Para tabelas de banco de dados que têm muitas colunas, isso pode resultar em cláusulas de `Where` muito grandes e em um aplicativo Web, ele pode exigir que você mantenha grandes quantidades de estado. Manter grandes quantidades de estado pode afetar o desempenho do aplicativo porque ele requer recursos do servidor (por exemplo, estado de sessão) ou deve ser incluído na própria página da Web (por exemplo, estado de exibição).

Neste tutorial, você adicionará o tratamento de erros para conflitos de simultaneidade otimista para uma entidade que não tem uma propriedade de rastreamento (a entidade de `Department`) e para uma entidade que tem uma propriedade de rastreamento (a entidade `OfficeAssignment`).

## <a name="handling-optimistic-concurrency-without-a-tracking-property"></a>Lidando com a simultaneidade otimista sem uma propriedade de controle

Para implementar a simultaneidade otimista para a entidade `Department`, que não tem uma propriedade Tracking (`Timestamp`), você concluirá as seguintes tarefas:

- Altere o modelo de dados para habilitar o acompanhamento de simultaneidade para entidades de `Department`.
- Na classe `SchoolRepository`, manipule exceções de simultaneidade no método `SaveChanges`.
- Na página Departments *. aspx* , manipule exceções de simultaneidade exibindo uma mensagem para o usuário aviso de que as alterações tentadas não foram bem-sucedidas. O usuário pode ver os valores atuais e tentar as alterações novamente se elas ainda forem necessárias.

### <a name="enabling-concurrency-tracking-in-the-data-model"></a>Habilitando o controle de simultaneidade no modelo de dados

No Visual Studio, abra o aplicativo Web da Contoso University com o qual você estava trabalhando no tutorial anterior desta série.

Abra *SchoolModel. edmx*e, no designer de modelo de dados, clique com o botão direito do mouse na propriedade `Name` na entidade `Department` e clique em **Propriedades**. Na janela **Propriedades** , altere a propriedade `ConcurrencyMode` para `Fixed`.

[![Image16](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

Faça o mesmo para as outras propriedades escalares não primárias (`Budget`, `StartDate`e `Administrator`.) (Você não pode fazer isso para propriedades de navegação.) Isso especifica que sempre que o Entity Framework gera um `Update` ou `Delete` comando SQL para atualizar a entidade `Department` no banco de dados, essas colunas (com valores originais) devem ser incluídas na cláusula `Where`. Se nenhuma linha for encontrada quando o comando `Update` ou `Delete` for executado, o Entity Framework gerará uma exceção de simultaneidade otimista.

Salve e feche o modelo de dados.

### <a name="handling-concurrency-exceptions-in-the-dal"></a>Manipulando exceções de simultaneidade na DAL

Abra *SchoolRepository.cs* e adicione a seguinte instrução de `using` para o namespace `System.Data`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

Adicione o novo método `SaveChanges` a seguir, que lida com exceções de simultaneidade otimista:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Se ocorrer um erro de simultaneidade quando esse método for chamado, os valores de propriedade da entidade na memória serão substituídos pelos valores atualmente no banco de dados. A exceção de simultaneidade é relançada para que a página da Web possa tratá-la.

Nos métodos `DeleteDepartment` e `UpdateDepartment`, substitua a chamada existente para `context.SaveChanges()` por uma chamada para `SaveChanges()` a fim de invocar o novo método.

### <a name="handling-concurrency-exceptions-in-the-presentation-layer"></a>Manipulando exceções de simultaneidade na camada de apresentação

Abra Departments *. aspx* e adicione um atributo `OnDeleted="DepartmentsObjectDataSource_Deleted"` ao controle de `DepartmentsObjectDataSource`. A marca de abertura para o controle agora será semelhante ao exemplo a seguir.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

No controle de `DepartmentsGridView`, especifique todas as colunas de tabela no atributo `DataKeyNames`, conforme mostrado no exemplo a seguir. Observe que isso criará campos de estado de exibição muito grandes, que é um motivo pelo qual usar um campo de controle é geralmente a maneira preferida de rastrear conflitos de simultaneidade.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

Abra *Departments.aspx.cs* e adicione a seguinte instrução de `using` para o namespace `System.Data`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

Adicione o novo método a seguir, que você chamará do `Updated` do controle da fonte de dados e `Deleted` manipuladores de eventos para lidar com exceções de simultaneidade:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

Esse código verifica o tipo de exceção e, se for uma exceção de simultaneidade, o código criará dinamicamente um controle de `CustomValidator` que, por sua vez, exibirá uma mensagem no controle de `ValidationSummary`.

Chame o novo método do manipulador de eventos `Updated` que você adicionou anteriormente. Além disso, crie um novo manipulador de eventos `Deleted` que chama o mesmo método (mas não faz mais nada):

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

### <a name="testing-optimistic-concurrency-in-the-departments-page"></a>Testando a simultaneidade otimista na página departamentos

Execute a página *departamentos. aspx* .

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

Clique em **Editar** em uma linha e altere o valor na coluna **orçamento** . (Lembre-se de que você só pode editar os registros que criou para este tutorial, pois os registros de banco de dados do `School` existentes contêm algum dado inválido. O registro do departamento de economia é seguro para experimentar.)

[![Image18](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

Abra uma nova janela do navegador e execute a página novamente (Copie a URL da caixa de endereço da primeira janela do navegador para a segunda janela do navegador).

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Clique em **Editar** na mesma linha que você editou anteriormente e altere o valor de **orçamento** para algo diferente.

[![Image19](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

Na segunda janela do navegador, clique em **Atualizar**. O valor do **orçamento** foi alterado com êxito para esse novo valor.

[![Image20](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

Na primeira janela do navegador, clique em **Atualizar**. A atualização falha. O valor do **orçamento** é exibido novamente usando o valor definido na segunda janela do navegador e você vê uma mensagem de erro.

[![Image21](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

## <a name="handling-optimistic-concurrency-using-a-tracking-property"></a>Lidando com a simultaneidade otimista usando uma propriedade de controle

Para lidar com a simultaneidade otimista para uma entidade que tem uma propriedade de rastreamento, você concluirá as seguintes tarefas:

- Adicione procedimentos armazenados ao modelo de dados para gerenciar entidades de `OfficeAssignment`. (As propriedades de rastreamento e os procedimentos armazenados não precisam ser usados juntos; eles estão apenas agrupados aqui para ilustração.)
- Adicione métodos CRUD à DAL e à BLL para entidades de `OfficeAssignment`, incluindo o código para lidar com exceções de simultaneidade otimistas na DAL.
- Crie uma página da Web de atribuições do Office.
- Teste a simultaneidade otimista na nova página da Web.

### <a name="adding-officeassignment-stored-procedures-to-the-data-model"></a>Adicionando procedimentos armazenados OfficeAssignment ao modelo de dados

Abra o arquivo *SchoolModel. edmx* no designer de modelo, clique com o botão direito do mouse na superfície de design e clique em **atualizar modelo do banco de dados**. Na guia **Adicionar** da caixa de diálogo **escolher seu objeto de banco de dados** , expanda **procedimentos armazenados** e selecione os três `OfficeAssignment` procedimentos armazenados (consulte a captura de tela a seguir) e clique em **concluir**. (Esses procedimentos armazenados já estavam no banco de dados quando você fez o download ou o criou usando um script.)

[![Image02](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Clique com o botão direito do mouse na entidade `OfficeAssignment` e selecione **mapeamento de procedimento armazenado**.

[![Image03](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

Defina as funções **Insert**, **Update**e **delete** para usar seus procedimentos armazenados correspondentes. Para o parâmetro `OrigTimestamp` da função `Update`, defina a **Propriedade** como `Timestamp` e selecione a opção **usar valor original** .

[![Image04](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image30.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image29.png)

Quando o Entity Framework chama o procedimento armazenado `UpdateOfficeAssignment`, ele passará o valor original da coluna `Timestamp` no parâmetro `OrigTimestamp`. O procedimento armazenado usa esse parâmetro em sua cláusula `Where`:

[!code-sql[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.sql)]

O procedimento armazenado também seleciona o novo valor da coluna `Timestamp` após a atualização para que o Entity Framework possa manter a entidade `OfficeAssignment` que está na memória sincronizada com a linha de banco de dados correspondente.

(Observe que o procedimento armazenado para excluir uma atribuição do Office não tem um parâmetro `OrigTimestamp`. Por isso, o Entity Framework não pode verificar se uma entidade está inalterada antes de excluí-la.)

Salve e feche o modelo de dados.

### <a name="adding-officeassignment-methods-to-the-dal"></a>Adicionando métodos OfficeAssignment à DAL

Abra *ISchoolRepository.cs* e adicione os seguintes métodos CRUD para o conjunto de entidades `OfficeAssignment`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

Adicione os novos métodos a seguir a *SchoolRepository.cs*. No método `UpdateOfficeAssignment`, você chama o método `SaveChanges` local em vez de `context.SaveChanges`.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

No projeto de teste, abra *MockSchoolRepository.cs* e adicione a seguinte coleção de `OfficeAssignment` e métodos CRUD a ele. (O repositório de simulação deve implementar a interface do repositório ou a solução não será compilada.)

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

### <a name="adding-officeassignment-methods-to-the-bll"></a>Adicionando métodos OfficeAssignment à BLL

No projeto principal, abra *SchoolBL.cs* e adicione os seguintes métodos CRUD para a entidade `OfficeAssignment` definida como:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

## <a name="creating-an-officeassignments-web-page"></a>Criando uma página da Web do OfficeAssignments

Crie uma nova página da Web que usa a página mestra *site. Master* e nomeie-a *OfficeAssignments. aspx*. Adicione a seguinte marcação ao controle de `Content` chamado `Content2`:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.aspx)]

Observe que, no atributo `DataKeyNames`, a marcação Especifica a propriedade `Timestamp`, bem como a chave de registro (`InstructorID`). Especificar propriedades no atributo `DataKeyNames` faz com que o controle os salve no estado de controle (que é semelhante ao estado de exibição) para que os valores originais estejam disponíveis durante o processamento de postback.

Se você não salvou o valor `Timestamp`, o Entity Framework não o terá para a cláusula `Where` do comando SQL `Update`. Consequentemente, nada seria encontrado para atualizar. Como resultado, o Entity Framework geraria uma exceção de simultaneidade otimista sempre que uma entidade de `OfficeAssignment` for atualizada.

Abra *OfficeAssignments.aspx.cs* e adicione a seguinte instrução de `using` para a camada de acesso de dados:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Adicione o seguinte método `Page_Init`, que habilita a funcionalidade de Dados Dinâmicos. Além disso, adicione o seguinte manipulador para o evento de `Updated` do controle de `ObjectDataSource` para verificar se há erros de simultaneidade:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

### <a name="testing-optimistic-concurrency-in-the-officeassignments-page"></a>Testando a simultaneidade otimista na página OfficeAssignments

Execute a página *OfficeAssignments. aspx* .

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image32.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image31.png)

Clique em **Editar** em uma linha e altere o valor na coluna **local** .

[![Image11](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image34.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image33.png)

Abra uma nova janela do navegador e execute a página novamente (Copie a URL da primeira janela do navegador para a segunda janela do navegador).

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image36.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image35.png)

Clique em **Editar** na mesma linha que você editou anteriormente e altere o valor de **local** para algo diferente.

[![Image12](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image38.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image37.png)

Na segunda janela do navegador, clique em **Atualizar**.

[![Image13](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image40.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image39.png)

Alterne para a primeira janela do navegador e clique em **Atualizar**.

[![Image15](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image42.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image41.png)

Você verá uma mensagem de erro e o valor do **local** foi atualizado para mostrar o valor que você alterou para a segunda janela do navegador.

## <a name="handling-concurrency-with-the-entitydatasource-control"></a>Manipulando a simultaneidade com o controle EntityDataSource

O controle de `EntityDataSource` inclui uma lógica interna que reconhece as configurações de simultaneidade no modelo de dados e manipula as operações de atualização e exclusão adequadamente. No entanto, assim como acontece com todas as exceções, você deve manipular `OptimisticConcurrencyException` exceções por conta própria para fornecer uma mensagem de erro amigável.

Em seguida, você irá configurar a página *courses. aspx* (que usa um controle `EntityDataSource`) para permitir operações de atualização e exclusão e exibir uma mensagem de erro se ocorrer um conflito de simultaneidade. A entidade `Course` não tem uma coluna de acompanhamento de simultaneidade, portanto, você usará o mesmo método que fez com a entidade `Department`: acompanhe os valores de todas as propriedades que não são de chave.

Abra o arquivo *SchoolModel. edmx* . Para as propriedades não chave da entidade `Course` (`Title`, `Credits`e `DepartmentID`), defina a propriedade modo de **simultaneidade** como `Fixed`. Em seguida, salve e feche o modelo de dados.

Abra a página *cursos. aspx* e faça as seguintes alterações:

- No controle `CoursesEntityDataSource`, adicione `EnableUpdate="true"` e `EnableDelete="true"` atributos. A marca de abertura para esse controle agora é semelhante ao exemplo a seguir:

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample16.aspx)]
- No controle `CoursesGridView`, altere o valor do atributo `DataKeyNames` para `"CourseID,Title,Credits,DepartmentID"`. Em seguida, adicione um elemento `CommandField` ao elemento `Columns` que mostra os botões **Editar** e **excluir** (`<asp:CommandField ShowEditButton="True" ShowDeleteButton="True" />`). O controle `GridView` agora é semelhante ao exemplo a seguir:

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample17.aspx)]

Execute a página e crie uma situação de conflito como fazia antes na página departamentos. Execute a página em duas janelas do navegador, clique em **Editar** na mesma linha em cada janela e faça uma alteração diferente em cada uma delas. Clique em **Atualizar** em uma janela e, em seguida, clique em **Atualizar** na outra janela. Ao clicar em **Atualizar** na segunda vez, você verá a página de erro resultante de uma exceção de simultaneidade sem tratamento.

[![Image22](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image44.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image43.png)

Você lida com esse erro de maneira muito semelhante à forma como você o tratou para o controle de `ObjectDataSource`. Abra a página *cursos. aspx* e, no controle `CoursesEntityDataSource`, especifique os manipuladores para os eventos `Deleted` e `Updated`. A marca de abertura do controle agora é semelhante ao exemplo a seguir:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample18.aspx)]

Antes do controle de `CoursesGridView`, adicione o seguinte controle de `ValidationSummary`:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample19.aspx)]

No *courses.aspx.cs*, adicione uma instrução de `using` para o namespace `System.Data`, adicione um método que verifica as exceções de simultaneidade e adicione manipuladores para os manipuladores `Deleted` e `Updated` do controle de `EntityDataSource`. O código se parecerá com o seguinte:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample20.cs)]

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample21.cs)]

A única diferença entre esse código e o que você fez para o controle de `ObjectDataSource` é que, nesse caso, a exceção de simultaneidade está na propriedade `Exception` do objeto de argumentos de evento em vez de na propriedade `InnerException` dessa exceção.

Execute a página e crie um conflito de simultaneidade novamente. Desta vez, você verá uma mensagem de erro:

[![Image23](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image46.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image45.png)

Isso conclui a introdução à manipulação de conflitos de simultaneidade. O próximo tutorial fornecerá orientações sobre como melhorar o desempenho em um aplicativo Web que usa o Entity Framework.

> [!div class="step-by-step"]
> [Anterior](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
> [Próximo](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
