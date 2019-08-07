---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Tutorial: Adicionar classificação, filtragem e paginação com o Entity Framework em um aplicativo MVC ASP.NET | Microsoft Docs'
author: tdykstra
description: Neste tutorial, você adiciona a funcionalidade de classificação, filtragem e paginação à página de índice dos **alunos** . Você também cria uma página de agrupamento simples.
ms.author: riande
ms.date: 01/14/2019
ms.assetid: d5723e46-41fe-4d09-850a-e03b9e285bfa
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: d9fadc12aa83a8095f364cf39e5376243a7d0670
ms.sourcegitcommit: f774732a3960fca079438a88a5472c37cf7be08a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68810760"
---
# <a name="tutorial-add-sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Tutorial: Adicionar classificação, filtragem e paginação com o Entity Framework em um aplicativo MVC ASP.NET

No [tutorial anterior](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md), você implementou um conjunto de páginas da Web para operações CRUD básicas `Student` para entidades. Neste tutorial, você adiciona a funcionalidade de classificação, filtragem e paginação à página de índice dos **alunos** . Você também cria uma página de agrupamento simples.

A imagem a seguir mostra como a página será exibida quando você terminar. Os títulos de coluna são links que o usuário pode clicar para classificar por essa coluna. Clicar em um título de coluna alterna repetidamente entre a ordem de classificação ascendente e descendente.

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

Neste tutorial, você:

> [!div class="checklist"]
> * Adicionar links de classificação de coluna
> * Adicionar uma caixa Pesquisa
> * Adicionar paginação
> * Criar uma página Sobre

## <a name="prerequisites"></a>Pré-requisitos

* [Implementar a funcionalidade CRUD básica](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)

## <a name="add-column-sort-links"></a>Adicionar links de classificação de coluna

Para adicionar a classificação à página de índice de estudante, você alterará o `Index` método `Student` do controlador e adicionará o código à exibição de `Student` índice.

### <a name="add-sorting-functionality-to-the-index-method"></a>Adicionar funcionalidade de classificação ao método de índice

- No *Controllers\StudentController.cs*, substitua o `Index` método pelo código a seguir:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Esse código recebe um parâmetro `sortOrder` da cadeia de caracteres de consulta na URL. O valor da cadeia de caracteres de consulta é fornecido pelo ASP.NET MVC como um parâmetro para o método de ação. O parâmetro é uma cadeia de caracteres que é "Name" ou "Date", opcionalmente seguido por um sublinhado e a cadeia de caracteres "desc" para especificar a ordem decrescente. A ordem de classificação crescente é padrão.

Na primeira vez que a página Índice é solicitada, não há nenhuma cadeia de caracteres de consulta. Os alunos são exibidos em ordem crescente por `LastName`, que é o padrão, conforme estabelecido pelo caso de outono `switch` na instrução. Quando o usuário clica em um hiperlink de título de coluna, o valor `sortOrder` apropriado é fornecido na cadeia de caracteres de consulta.

As duas `ViewBag` variáveis são usadas para que a exibição possa configurar os hiperlinks de título de coluna com os valores de cadeia de caracteres de consulta apropriados:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Essas são instruções ternárias. O primeiro especifica que, se o `sortOrder` parâmetro for nulo ou vazio, `ViewBag.NameSortParm` deve ser definido como "nome\_desc"; caso contrário, ele deve ser definido como uma cadeia de caracteres vazia. Essas duas instruções permitem que a exibição defina os hiperlinks de título de coluna da seguinte maneira:

| Ordem de classificação atual | Hiperlink do sobrenome | Hiperlink de data |
| --- | --- | --- |
| Sobrenome ascendente | descending | ascending |
| Sobrenome descendente | ascending | ascending |
| Data ascendente | ascending | descending |
| Data descendente | ascending | ascending |

O método usa [LINQ to Entities](/dotnet/framework/data/adonet/ef/language-reference/linq-to-entities) para especificar a coluna pela qual classificar. O código cria uma <xref:System.Linq.IQueryable%601> variável antes da `switch` instrução, `switch` modifica-a na instrução e chama o `ToList` método após a `switch` instrução. Quando você cria e modifica variáveis `IQueryable`, nenhuma consulta é enviada para o banco de dados. A consulta não é executada até que você converta o `IQueryable` objeto em uma coleção chamando um método `ToList`como. Portanto, esse código resulta em uma única consulta que não é executada até a `return View` instrução.

Como uma alternativa para gravar diferentes instruções LINQ para cada ordem de classificação, você pode criar dinamicamente uma instrução LINQ. Para obter informações sobre LINQ dinâmico, consulte [Dynamic LINQ](https://go.microsoft.com/fwlink/?LinkID=323957).

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Adicionar hiperlinks de título de coluna à exibição de índice de estudante

1. No *Views\Student\Index.cshtml*, substitua os `<tr>` elementos `<th>` e da linha de cabeçalho pelo código realçado:

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

   Esse código usa as informações nas `ViewBag` Propriedades para configurar hiperlinks com os valores de cadeia de caracteres de consulta apropriados.

2. Execute a página e clique nos títulos de coluna de data do **último nome** e do **registro** para verificar se a classificação funciona.

   Depois de clicar no título do **sobrenome** , os alunos serão exibidos em ordem decrescente do último nome.

## <a name="add-a-search-box"></a>Adicionar uma caixa Pesquisa

Para adicionar a filtragem à página de índice estudantes, você adicionará uma caixa de texto e um botão enviar à exibição e fará as alterações correspondentes `Index` no método. A caixa de texto permite que você insira uma cadeia de caracteres a ser pesquisada nos campos nome e sobrenome.

### <a name="add-filtering-functionality-to-the-index-method"></a>Adicionar a funcionalidade de filtragem a método Index

- No *Controllers\StudentController.cs*, substitua o `Index` método pelo código a seguir (as alterações são realçadas):

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

O código adiciona um `searchString` parâmetro `Index` ao método. O valor de cadeia de caracteres de pesquisa é recebido em uma caixa de texto que você adicionará à exibição Índice. Ele também adiciona uma `where` cláusula à instrução LINQ que seleciona somente alunos cujo nome ou sobrenome contêm a cadeia de caracteres de pesquisa. A instrução que adiciona a <xref:System.Linq.Queryable.Where%2A> cláusula será executada somente se houver um valor a ser procurado.

> [!NOTE]
> Em muitos casos, você pode chamar o mesmo método em um conjunto de entidades Entity Framework ou como um método de extensão em uma coleção na memória. Os resultados são normalmente os mesmos, mas em alguns casos podem ser diferentes.
>
> Por exemplo, a implementação de .NET Framework do `Contains` método retorna todas as linhas quando você passa uma cadeia de caracteres vazia para ela, mas o provedor de Entity Framework para SQL Server Compact 4,0 retorna zero linhas para cadeias de caracteres vazias. Portanto, o código no exemplo (colocando a `Where` instrução dentro de `if` uma instrução) garante que você obtenha os mesmos resultados para todas as versões do SQL Server. Além disso, a implementação de .NET Framework `Contains` do método executa uma comparação que diferencia maiúsculas de minúsculas por padrão, mas Entity Framework provedores de SQL Server executam comparações que não diferenciam maiúsculas de minúsculas por padrão. Portanto, chamar o `ToUpper` método para fazer o teste explicitamente não diferencia maiúsculas de minúsculas garante que os resultados não sejam alterados quando você altera o código posteriormente para usar um repositório, `IEnumerable` o que retornará uma `IQueryable` coleção em vez de um objeto. (Quando você chama o método `Contains` em uma coleção `IEnumerable`, obtém a implementação do .NET Framework; quando chama-o em um objeto `IQueryable`, obtém a implementação do provedor de banco de dados.)
>
> A manipulação nula também pode ser diferente para provedores de banco de dados diferentes ou `IQueryable` quando você usa um objeto comparado ao `IEnumerable` uso de uma coleção. Por exemplo, em alguns cenários, `Where` uma condição `table.Column != 0` como não pode retornar colunas que tenham `null` o valor. Por padrão, o EF gera operadores SQL adicionais para que a igualdade entre valores nulos funcione no banco de dados como funciona na memória, mas você pode definir o sinalizador [UseDatabaseNullSemantics](https://docs.microsoft.com/dotnet/api/system.data.entity.infrastructure.dbcontextconfiguration.usedatabasenullsemantics) em EF6 ou chamar o método [UseRelationalNulls](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.infrastructure.relationaldbcontextoptionsbuilder-2.userelationalnulls) em EF Core para Configure esse comportamento.

### <a name="add-a-search-box-to-the-student-index-view"></a>Adicionar uma caixa de pesquisa à exibição de índice de estudante

1. No *Views\Student\Index.cshtml*, adicione o código realçado imediatamente antes `table` da marca de abertura para criar uma legenda, uma caixa de texto e um botão de **pesquisa** .

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=4-11)]

2. Execute a página, insira uma cadeia de caracteres de pesquisa e clique em **Pesquisar** para verificar se a filtragem está funcionando.

   Observe que a URL não contém a cadeia de caracteres de pesquisa "a", o que significa que, se você marcar essa página, você não obterá a lista filtrada ao usar o indicador. Isso se aplica também aos links de classificação de coluna, pois eles classificarão a lista inteira. Você alterará o botão de **pesquisa** para usar cadeias de caracteres de consulta para critérios de filtro posteriormente no tutorial.

## <a name="add-paging"></a>Adicionar paginação

Para adicionar a paginação à página de índice estudantes, você começará instalando o pacote NuGet **PagedList. Mvc** . Em seguida, você fará alterações adicionais no `Index` método e adicionará links de paginação `Index` à exibição. O **PagedList. Mvc** é um dos muitos pacotes bons de paginação e classificação para o ASP.NET MVC, e seu uso é destinado apenas como exemplo, não como uma recomendação para ele em relação a outras opções.

### <a name="install-the-pagedlistmvc-nuget-package"></a>Instalar o pacote NuGet PagedList. MVC

O pacote NuGet **PagedList. Mvc** instala automaticamente o pacote **PagedList** como uma dependência. O pacote **PagedList** instala um `PagedList` tipo de coleção e métodos de extensão `IEnumerable` para `IQueryable` coleções e. Os métodos de extensão criam uma única página de dados em `PagedList` uma coleção fora de `IQueryable` seu `IEnumerable`ou e a `PagedList` coleção fornece várias propriedades e métodos que facilitam a paginação. O pacote **PagedList. Mvc** instala um auxiliar de paginação que exibe os botões de paginação.

1. No menu **ferramentas** , selecione **Gerenciador de pacotes NuGet** e **console do Gerenciador de pacotes**.

2. Na janela do **console do Gerenciador de pacotes** , verifique se a origem do **pacote** é **NuGet.org** e se o **projeto padrão** é **ContosoUniversity**e, em seguida, digite o seguinte comando:

   ```text
   Install-Package PagedList.Mvc
   ```

3. Compile o projeto.

### <a name="add-paging-functionality-to-the-index-method"></a>Adicionar a funcionalidade de paginação ao método Index

1. Em *Controllers\StudentController.cs*, adicione uma `using` instrução para o `PagedList` namespace:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

2. Substitua o método `Index` pelo seguinte código:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=1,3,7-16,41-43)]

   Esse código adiciona um `page` parâmetro, um parâmetro de ordem de classificação atual e um parâmetro de filtro atual à assinatura do método:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

   Na primeira vez em que a página for exibida, ou se o usuário não tiver clicado em um link de paginação ou de classificação, todos os parâmetros serão nulos. Se um link de paginação for `page` clicado, a variável conterá o número de página a ser exibido.

   Uma `ViewBag` propriedade fornece a exibição com a ordem de classificação atual, pois ela deve ser incluída nos links de paginação para manter a mesma ordem de classificação durante a paginação:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

   Outra propriedade, `ViewBag.CurrentFilter`, fornece a exibição com a cadeia de caracteres de filtro atual. Esse valor precisa ser incluído nos links de paginação para manter as configurações de filtro durante a paginação e precisa ser restaurado para a caixa de texto quando a página é exibida novamente. Se a cadeia de caracteres de pesquisa for alterada durante a paginação, a página precisará ser redefinida como 1, porque o novo filtro pode resultar na exibição de dados diferentes. A cadeia de caracteres de pesquisa é alterada quando um valor é inserido na caixa de texto e o botão enviar é pressionado. Nesse caso, o `searchString` parâmetro não é nulo.

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

   No final do método, o `ToPagedList` método de extensão no objeto dos alunos `IQueryable` converte a consulta de aluno em uma única página de alunos em um tipo de coleção que dá suporte à paginação. Essa única página de alunos é passada para a exibição:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

   O método `ToPagedList` usa um número de página. Os dois pontos de interrogação representam o [operador de União nula](/dotnet/csharp/language-reference/operators/null-coalescing-operator). O operador de união de nulo define um valor padrão para um tipo que permite valor nulo; a expressão `(page ?? 1)` significa retornar o valor de `page` se ele tiver um valor ou retornar 1 se `page` for nulo.

### <a name="add-paging-links-to-the-student-index-view"></a>Adicionar links de paginação à exibição de índice de estudante

1. No *Views\Student\Index.cshtml*, substitua o código existente pelo código a seguir. As alterações são realçadas.

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=1-3,6,9,14,17,24,30,55-56,58-59)]

   A instrução `@model` na parte superior da página especifica que a exibição agora obtém um objeto `PagedList`, em vez de um objeto `List`.

   A `using` instrução para `PagedList.Mvc` fornece acesso ao auxiliar do MVC para os botões de paginação.

   O código usa uma sobrecarga de [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) que permite especificar [FormMethod. Get](/previous-versions/aspnet/dd460179(v=vs.100)).

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

   O [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) padrão envia dados de formulário com uma postagem, o que significa que os parâmetros são passados no corpo da mensagem http e não na URL como cadeias de consulta. Quando você especifica HTTP GET, os dados de formulário são passados na URL como cadeias de consulta, o que permite aos usuários marcar a URL. As [diretrizes do W3C para o uso de http Get](http://www.w3.org/2001/tag/doc/whenToUseGet.html) recomendam que você use Get quando a ação não resultar em uma atualização.

   A caixa de texto é inicializada com a cadeia de caracteres de pesquisa atual para que, ao clicar em uma nova página, você possa ver a cadeia de caracteres de pesquisa atual.

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

   Os links de cabeçalho de coluna usam a cadeia de caracteres de consulta para passar a cadeia de caracteres de pesquisa atual para o controlador, de modo que o usuário possa classificar nos resultados do filtro:

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

   A página atual e o número total de páginas são exibidos.

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

   Se não houver nenhuma página a ser exibida, "página 0 de 0" será mostrada. (Nesse caso, o número da página é maior que a contagem de `Model.PageNumber` páginas porque é 1 `Model.PageCount` e é 0.)

   Os botões de paginação são `PagedListPager` exibidos pelo auxiliar:

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

   O `PagedListPager` auxiliar fornece várias opções que você pode personalizar, incluindo URLs e estilos. Para obter mais informações, consulte [TroyGoode/PagedList](https://github.com/TroyGoode/PagedList) no site do github.

2. Execute a página.

   Clique nos links de paginação em ordens de classificação diferentes para verificar se a paginação funciona. Em seguida, insira uma cadeia de caracteres de pesquisa e tente fazer a paginação novamente para verificar se ela também funciona corretamente com a classificação e filtragem.

## <a name="create-an-about-page"></a>Criar uma página Sobre

Para a página sobre do site da Contoso University, você exibirá quantos alunos se registraram para cada data de registro. Isso exige agrupamento e cálculos simples nos grupos. Para fazer isso, você fará o seguinte:

- Criar uma classe de modelo de exibição para os dados que você precisa passar para a exibição.
- Modifique o `About` método `Home` no controlador.
- Modifique a `About` exibição.

### <a name="create-the-view-model"></a>Criar o modelo de exibição

Crie uma pasta ViewModels na pasta do projeto. Nessa pasta, adicione um arquivo de classe *EnrollmentDateGroup.cs* e substitua o código do modelo pelo código a seguir:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>Modificar o controlador Home

1. No *HomeController.cs*, adicione as seguintes `using` instruções na parte superior do arquivo:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

2. Adicione uma variável de classe para o contexto do banco de dados imediatamente após a chave de abertura para a classe:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

3. Substitua o método `About` pelo seguinte código:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

   A instrução LINQ agrupa as entidades de alunos por data de registro, calcula o número de entidades em cada grupo e armazena os resultados em uma coleção de objetos de modelo de exibição `EnrollmentDateGroup`.

4. Adicione um `Dispose` método:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>Modificar a exibição Sobre

1. Substitua o código no arquivo *Views\Home\About.cshtml* pelo código a seguir:

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

2. Execute o aplicativo e clique no link **sobre** .

   A contagem de alunos para cada data de registro é exibida em uma tabela.

   ![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="get-the-code"></a>Obter o código

[Baixar o projeto concluído](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Recursos adicionais

Links para outros recursos de Entity Framework podem ser encontrados em [recursos de acesso a dados ASP.net-recomendado](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você:

> [!div class="checklist"]
> * Adicionar links de classificação de coluna
> * Adicionar uma caixa Pesquisa
> * Adicionar paginação
> * Criar uma página Sobre

Avance para o próximo artigo para saber como usar a resiliência de conexão e a interceptação de comando.
> [!div class="nextstepaction"]
> [Resiliência de conexão e interceptação de comando](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)
