---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
title: Introdução ao Páginas da Web do ASP.NET-atualizando dados do banco de dados | Microsoft Docs
author: Rick-Anderson
description: Este tutorial mostra como atualizar (alterar) uma entrada de banco de dados existente quando você usa Páginas da Web do ASP.NET (Razor). Ele pressupõe que você concluiu a série th...
ms.author: riande
ms.date: 01/02/2018
ms.assetid: ac86ec9c-6b69-485b-b9e0-8b9127b13e6b
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
msc.type: authoredcontent
ms.openlocfilehash: 8f8bcfb7d9d2416a2699776cadbdaae8e12415ba
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78574224"
---
# <a name="introducing-aspnet-web-pages---updating-database-data"></a>Introdução ao Páginas da Web do ASP.NET-atualizando dados do banco

por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial mostra como atualizar (alterar) uma entrada de banco de dados existente quando você usa Páginas da Web do ASP.NET (Razor). Ele pressupõe que você concluiu a série por meio da [inserção de dados usando formulários usando páginas da Web do ASP.net](entering-data.md).
> 
> O que você aprenderá:
> 
> - Como selecionar um registro individual no auxiliar de `WebGrid`.
> - Como ler um único registro de um banco de dados.
> - Como pré-carregar um formulário com valores do registro do banco de dados.
> - Como atualizar um registro existente em um banco de dados.
> - Como armazenar informações na página sem exibi-las.
> - Como usar um campo oculto para armazenar informações.
>   
> 
> Recursos/tecnologias abordados:
> 
> - O auxiliar de `WebGrid`.
> - O comando SQL `Update`.
> - O método `Database.Execute`.
> - Campos ocultos (`<input type="hidden">`).

## <a name="what-youll-build"></a>O que você vai construir

No tutorial anterior, você aprendeu a adicionar um registro a um banco de dados. Aqui, você aprenderá a exibir um registro para edição. Na página *filmes* , você atualizará o `WebGrid` auxiliar para que ele exiba um link de **edição** ao lado de cada filme:

![Exibição do WebGrid, incluindo um link de ' Editar ' para cada filme](updating-data/_static/image1.png)

Quando você clica no link **Editar** , ele leva para uma página diferente, onde as informações do filme já estão em um formato:

![Editar página de filme mostrando o filme a ser editado](updating-data/_static/image2.png)

Você pode alterar qualquer um dos valores. Quando você envia as alterações, o código na página atualiza o banco de dados e o leva de volta à listagem de filmes.

Essa parte do processo funciona quase exatamente como a página *addmovie. cshtml* criada no tutorial anterior, portanto, grande parte deste tutorial será familiar.

Há várias maneiras de implementar um modo de edição de um filme individual. A abordagem mostrada foi escolhida porque é fácil de ser implementada e fácil de entender.

## <a name="adding-an-edit-link-to-the-movie-listing"></a>Adicionando um link de edição à listagem de filmes

Para começar, você atualizará a página de *filmes* para que cada listagem de filmes também contenha um link de **edição** .

Abra o arquivo *Movies. cshtml* .

No corpo da página, altere a marcação de `WebGrid` adicionando uma coluna. Aqui está a marcação modificada:

[!code-html[Main](updating-data/samples/sample1.html?highlight=6)]

A nova coluna é esta:

[!code-html[Main](updating-data/samples/sample2.html)]

O ponto desta coluna é mostrar um link (`<a>` elemento) cujo texto diz "Editar". O que estamos procurando é criar um link semelhante ao seguinte quando a página é executada, com o valor `id` diferente para cada filme:

[!code-css[Main](updating-data/samples/sample3.css)]

Esse link invocará uma página chamada *EditMovie*e passará a cadeia de caracteres de consulta `?id=7` para essa página.

A sintaxe da nova coluna pode parecer um pouco complexa, mas isso só ocorre porque ele reúne vários elementos. Cada elemento individual é simples. Se você se concentrar apenas no elemento `<a>`, verá esta marcação:

[!code-html[Main](updating-data/samples/sample4.html)]

Alguns planos de fundo sobre como a grade funciona: a grade exibe linhas, uma para cada registro de banco de dados e exibe colunas para cada campo no registro do banco de dados. Enquanto cada linha de grade está sendo construída, o objeto `item` contém o registro de banco de dados (item) para essa linha. Esse arranjo fornece uma maneira no código para obter os dados dessa linha. É isso o que você vê aqui: a expressão `item.ID` está obtendo o valor de ID do item de banco de dados atual. Você pode obter qualquer um dos valores do banco de dados (título, gênero ou ano) da mesma maneira usando `item.Title`, `item.Genre`ou `item.Year`.

A expressão `"~/EditMovie?id=@item.ID` combina a parte embutida em código da URL de destino (`~/EditMovie?id=`) com essa ID derivada dinamicamente. (Você viu o operador de `~` no tutorial anterior; é um operador ASP.NET que representa a raiz do site atual.)

O resultado é que essa parte da marcação na coluna simplesmente produz algo semelhante à marcação a seguir em tempo de execução:

[!code-xml[Main](updating-data/samples/sample5.xml)]

Naturalmente, o valor real de `id` será diferente para cada linha.

## <a name="creating-a-custom-display-for-a-grid-column"></a>Criando uma exibição personalizada para uma coluna de grade

Agora, volte para a coluna de grade. As três colunas que você tinha originalmente na grade exibiram apenas valores de dados (título, gênero e ano). Você especificou essa exibição passando o nome da coluna de banco de dados &mdash; por exemplo, `grid.Column("Title")`.

Essa nova coluna de link de **edição** é diferente. Em vez de especificar um nome de coluna, você está passando um parâmetro de `format`. Esse parâmetro permite que você defina a marcação que o auxiliar de `WebGrid` processará junto com o valor de `item` para exibir os dados da coluna como negrito ou verde ou em qualquer formato desejado. Por exemplo, se você quisesse que o título aparecesse em negrito, você poderia criar uma coluna como este exemplo:

[!code-html[Main](updating-data/samples/sample6.html)]

(Os vários `@` caracteres que você vê na propriedade `format` marcam a transição entre marcação e um valor de código.)

Depois de conhecer a propriedade `format`, é mais fácil entender como a nova coluna de link de **edição** é agrupada:

[!code-html[Main](updating-data/samples/sample7.html)]

A coluna consiste *apenas* na marcação que renderiza o link, além de algumas informações (a ID) que são extraídas do registro de banco de dados para a linha.

> [!TIP]
> 
> **Parâmetros nomeados e parâmetros posicionais para um método**
> 
> Muitas vezes, quando você chamou um método e passou parâmetros para ele, você simplesmente listou os valores de parâmetro separados por vírgulas. Aqui estão alguns exemplos:
> 
> `db.Execute(insertCommand, title, genre, year)`
> 
> `Validation.RequireField("title", "You must enter a title")`
> 
> Não mencionamos o problema quando você viu o código pela primeira vez, mas, em cada caso, você está passando parâmetros para os métodos em uma ordem específica &mdash; ou seja, a ordem na qual os parâmetros são definidos nesse método. Para `db.Execute` e `Validation.RequireFields`, se você misturou a ordem dos valores que passa, receberá uma mensagem de erro quando a página for executada ou, pelo menos, alguns resultados estranhos. Claramente, você precisa saber o pedido para passar os parâmetros. (No WebMatrix, o IntelliSense pode ajudá-lo a aprender a descobrir o nome, o tipo e a ordem dos parâmetros.)
> 
> Como uma alternativa para passar valores na ordem, você pode usar *parâmetros nomeados*. (Passar parâmetros na ordem é conhecido como usar *parâmetros posicionais*.) Para parâmetros nomeados, você inclui explicitamente o nome do parâmetro ao passar seu valor. Você usou parâmetros nomeados já várias vezes nesses tutoriais. Por exemplo:
> 
> [!code-csharp[Main](updating-data/samples/sample8.cs)]
> 
> e
> 
> [!code-css[Main](updating-data/samples/sample9.css)]
> 
> Os parâmetros nomeados são úteis para algumas situações, especialmente quando um método usa muitos parâmetros. Uma é quando você deseja passar apenas um ou dois parâmetros, mas os valores que você deseja passar não estão entre as primeiras posições na lista de parâmetros. Outra situação é quando você deseja tornar seu código mais legível passando os parâmetros na ordem que faz mais sentido para você.
> 
> Obviamente, para usar parâmetros nomeados, você precisa saber os nomes dos parâmetros. O WebMatrix IntelliSense pode *Mostrar* os nomes, mas ele não pode preenchê-los no momento para você.

## <a name="creating-the-edit-page"></a>Criando a página de edição

Agora você pode criar a página *EditMovie* . Quando os usuários clicarem no link **Editar** , eles terminarão nesta página.

Crie uma página chamada *EditMovie. cshtml* e substitua o que está no arquivo pela seguinte marcação:

[!code-cshtml[Main](updating-data/samples/sample10.cshtml)]

Essa marcação e código é semelhante ao que você tem na página *addmovie* . Há uma pequena diferença no texto do botão enviar. Assim como acontece com a página *addmovie* , há uma chamada `Html.ValidationSummary` que exibirá erros de validação, se houver algum. Desta vez, vamos sair de chamadas para `Validation.Message`, pois os erros serão exibidos no Resumo de validação. Conforme observado no tutorial anterior, você pode usar o resumo de validação e as mensagens de erro individuais em várias combinações.

Observe novamente que o atributo `method` do elemento `<form>` está definido como `post`. Assim como acontece com a página *addmovie. cshtml* , essa página faz alterações no banco de dados. Portanto, esse formulário deve executar uma operação `POST`. (Para obter mais informações sobre a diferença entre as operações de `GET` e `POST`, consulte a barra lateral de [segurança de verbo GET, post e http](form-basics.md#GET,_POST,_and_HTTP_Verb_Safety) no tutorial em formulários HTML.)

Como vimos em um tutorial anterior, os `value` atributos das caixas de texto estão sendo definidos com o código do Razor para pré-carregar-los. Desta vez, no entanto, você está usando variáveis como `title` e `genre` para essa tarefa em vez de `Request.Form["title"]`:

`<input type="text" name="title" value="@title" />`

Como antes, essa marcação descarregará os valores da caixa de texto com os valores do filme. Você verá em um momento por que é útil usar variáveis desta vez em vez de usar o objeto `Request`.

Também há um elemento `<input type="hidden">` nesta página. Esse elemento armazena a ID do filme sem torná-la visível na página. A ID é passada inicialmente para a página usando um valor de cadeia de caracteres de consulta (`?id=7` ou semelhante na URL). Ao colocar o valor de ID em um campo oculto, você pode verificar se ele está disponível quando o formulário é enviado, mesmo que você não tenha mais acesso à URL original com a qual a página foi chamada.

Ao contrário da página *addmovie* , o código da página *EditMovie* tem duas funções distintas. A primeira função é que, quando a página é exibida pela primeira vez (e *apenas* depois), o código obtém a ID do filme da cadeia de caracteres de consulta. Em seguida, o código usa a ID para ler o filme correspondente do banco de dados e exibir (pré-carregar) nele nas caixas de texto.

A segunda função é que, quando o usuário clica no botão **enviar alterações** , o código precisa ler os valores das caixas de texto e validá-los. O código também precisa atualizar o item do banco de dados com os novos valores. Essa técnica é semelhante à adição de um registro, como vimos em *addmovie*.

## <a name="adding-code-to-read-a-single-movie"></a>Adicionando código para ler um único filme

Para executar a primeira função, adicione este código à parte superior da página:

[!code-cshtml[Main](updating-data/samples/sample11.cshtml)]

A maior parte desse código está dentro de um bloco que inicia `if(!IsPost)`. O operador de `!` significa "Not", portanto, a expressão significa que *se essa solicitação não for um envio de postagem*, que é uma maneira indireta de dizer *se essa solicitação é a primeira vez que essa página foi executada*. Conforme observado anteriormente, esse código deve ser executado *apenas* na primeira vez em que a página é executada. Se você não tiver colocado o código em `if(!IsPost)`, ele seria executado toda vez que a página for invocada, seja na primeira vez ou em resposta a um clique de botão.

Observe que o código inclui um `else` bloco desta vez. Como dissemos Quando introduzimos `if` blocos, às vezes você deseja executar um código alternativo se a condição que você está testando não for verdadeira. Esse é o caso aqui. Se a condição for aprovada (ou seja, se a ID passada para a página estiver ok), você lerá uma linha do banco de dados. No entanto, se a condição não passar, o bloco de `else` será executado e o código definirá uma mensagem de erro.

## <a name="validating-a-value-passed-to-the-page"></a>Validando um valor passado para a página

O código usa `Request.QueryString["id"]` para obter a ID que é passada para a página. O código garante que um valor foi realmente passado para a ID. Se nenhum valor foi passado, o código definirá um erro de validação.

Esse código mostra uma maneira diferente de validar informações. No tutorial anterior, você trabalhou com o auxiliar de `Validation`. Você registrou os campos para validar e ASP.NET automaticamente os erros de validação e exibição usando `Html.ValidationMessage` e `Html.ValidationSummary`. Nesse caso, no entanto, você não está realmente validando a entrada do usuário. Em vez disso, você está validando um valor que foi passado para a página de outro lugar. O auxiliar de `Validation` não faz isso para você.

Portanto, você verifica o valor por conta própria, testando-o com `if(!Request.QueryString["ID"].IsEmpty()`). Se houver um problema, você poderá exibir o erro usando `Html.ValidationSummary`, como fez com o auxiliar de `Validation`. Para fazer isso, você chama `Validation.AddFormError` e passa a ela uma mensagem a ser exibida. `Validation.AddFormError` é um método interno que permite definir mensagens personalizadas que se unem com o sistema de validação com o qual você já está familiarizado. (Mais adiante neste tutorial, falaremos sobre como tornar esse processo de validação um pouco mais robusto.)

Depois de verificar se há uma ID para o filme, o código lê o banco de dados, procurando apenas um único item de banco de dados. (Você provavelmente observou o padrão geral para operações de banco de dados: Abra o banco de dados, defina uma instrução SQL e execute a instrução.) Desta vez, a instrução SQL `Select` inclui `WHERE ID = @0`. Como a ID é exclusiva, somente um registro pode ser retornado.

A consulta é executada usando `db.QuerySingle` (não `db.Query`, como você usou para a listagem de filmes) e o código coloca o resultado na variável `row`. O nome `row` é arbitrário; Você pode nomear as variáveis como desejar. As variáveis inicializadas na parte superior são então preenchidas com os detalhes do filme para que esses valores possam ser exibidos nas caixas de texto.

## <a name="testing-the-edit-page-so-far"></a>Testando a página de edição (até agora)

Se você quiser testar sua página, execute a página *filmes* agora e clique em um link de **edição** ao lado de qualquer filme. Você verá a página *EditMovie* com os detalhes preenchidos para o filme selecionado:

![Editar página de filme mostrando o filme a ser editado](updating-data/_static/image3.png)

Observe que a URL da página inclui algo como `?id=10` (ou algum outro número). Até agora, você testou que os links de **edição** na página do *filme* funcionam, que sua página está lendo a ID da cadeia de caracteres de consulta e que a consulta do banco de dados para obter um único registro de filme está funcionando.

Você pode alterar as informações do filme, mas nada acontece quando você clica em **enviar alterações**.

## <a name="adding-code-to-update-the-movie-with-the-users-changes"></a>Adicionando código para atualizar o filme com as alterações do usuário

No arquivo *EditMovie. cshtml* , para implementar a segunda função (salvar alterações), adicione o código a seguir apenas dentro da chave de fechamento do bloco de `@`. (Se você não tiver certeza de onde colocar o código, poderá examinar a [listagem de código completa da página Editar filme](#Complete_Page_Listing_for_EditMovie) que aparece no final deste tutorial.)

[!code-csharp[Main](updating-data/samples/sample12.cs)]

Novamente, essa marcação e código é semelhante ao código em *addmovie*. O código está em um bloco de `if(IsPost)`, porque esse código é executado somente quando o usuário clica no botão **enviar alterações** &mdash; ou seja, quando (e somente quando) o formulário foi Postado. Nesse caso, você não está usando um teste como `if(IsPost && Validation.IsValid())`— ou seja, você não está combinando ambos os testes usando e. Nesta página, primeiro você determina se há um envio de formulário (`if(IsPost)`) e, em seguida, registra apenas os campos para validação. Em seguida, você pode testar os resultados da validação (`if(Validation.IsValid()`). O fluxo é ligeiramente diferente da página *addmovie. cshtml* , mas o efeito é o mesmo.

Você Obtém os valores das caixas de texto usando `Request.Form["title"]` e código semelhante para os outros elementos de `<input>`. Observe que, desta vez, o código obtém a ID do filme do campo oculto (`<input type="hidden">`). Quando a página foi executada pela primeira vez, o código obteve a ID da cadeia de caracteres de consulta. Você Obtém o valor do campo oculto para certificar-se de que está obtendo a ID do filme que foi originalmente exibido, caso a cadeia de caracteres de consulta tenha sido alterada de alguma forma, desde então.

A diferença realmente importante entre o código *addmovie* e esse código é que, nesse código, você usa a instrução SQL `Update` em vez da instrução `Insert Into`. O exemplo a seguir mostra a sintaxe da instrução SQL `Update`:

`UPDATE table SET col1="value", col2="value", col3="value" ... WHERE ID = value`

Você pode especificar quaisquer colunas em qualquer ordem, e não precisa necessariamente atualizar todas as colunas durante uma operação de `Update`. (Você não pode atualizar a própria ID, pois isso teria efeito salvar o registro como um novo registro, e isso não é permitido para uma operação de `Update`.)

> [!NOTE] 
> 
> **Importante** A cláusula `Where` com a ID é muito importante, pois é assim que o banco de dados sabe qual registro de banco de dados você deseja atualizar. Se você deixou a cláusula `Where`, o banco de dados atualizaria *todos* os registros no banco de dados. Na maioria dos casos, isso seria um desastre.

No código, os valores a serem atualizados são passados para a instrução SQL usando espaços reservados. Para repetir o que dissemos antes: por motivos de segurança, use *somente* espaços reservados para passar valores para uma instrução SQL.

Depois que o código usa `db.Execute` para executar a instrução `Update`, ele redireciona de volta para a página de listagem, onde você pode ver as alterações.

> [!TIP] 
> 
> **Diferentes instruções SQL, métodos diferentes**
> 
> Você pode ter notado que usa métodos ligeiramente diferentes para executar instruções SQL diferentes. Para executar uma consulta `Select` que potencialmente retorna vários registros, use o método `Query`. Para executar uma consulta `Select` que você sabe que retornará apenas um item de banco de dados, use o método `QuerySingle`. Para executar comandos que fazem alterações, mas que não retornam itens de banco de dados, você usa o método `Execute`.
> 
> Você precisa ter métodos diferentes porque cada um deles retorna resultados diferentes, como você já viu na diferença entre `Query` e `QuerySingle`. (O método `Execute`, na verdade, retorna um valor, também &mdash; ou seja, o número de linhas de banco de dados que foram afetadas pelo comando &mdash;, mas que você ignorou até agora.)
> 
> É claro que o método `Query` pode retornar apenas uma linha de banco de dados. No entanto, ASP.NET sempre trata os resultados do método `Query` como uma coleção. Mesmo que o método retorne apenas uma linha, você precisa extrair essa única linha da coleção. Portanto, em situações em que você *sabe* que obterá apenas uma linha, é um pouco mais conveniente usar `QuerySingle`.
> 
> Há alguns outros métodos que executam tipos específicos de operações de banco de dados. Você pode encontrar uma lista de métodos de banco de dados na [referência rápida da API de páginas da Web do ASP.net](../../api-reference/asp-net-web-pages-api-reference.md#Data).

## <a name="making-validation-for-the-id-more-robust"></a>Tornando a validação para a ID mais robusta

Na primeira vez em que a página é executada, você obtém a ID do filme da cadeia de caracteres de consulta para que possa obter o filme do banco de dados. Você se certifica de que realmente houvesse um valor para procurar, o que fazia usando este código:

[!code-csharp[Main](updating-data/samples/sample13.cs)]

Você usou esse código para garantir que, se um usuário chegar à página *EditMovies* sem primeiro selecionar um filme na página *filmes* , a página exibirá uma mensagem de erro amigável. (Caso contrário, os usuários veriam um erro que provavelmente apenas os confundem.)

No entanto, essa validação não é muito robusta. A página também pode ser chamada com estes erros:

- A ID não é um número. Por exemplo, a página pode ser chamada com uma URL como `http://localhost:nnnnn/EditMovie?id=abc`.
- A ID é um número, mas faz referência a um filme que não existe (por exemplo, `http://localhost:nnnnn/EditMovie?id=100934`).

Se você estiver curioso para ver os erros que resultam dessas URLs, execute a página *filmes* . Selecione um filme para editar e, em seguida, altere a URL da página *EditMovie* para uma URL que contenha uma ID alfabética ou a ID de um filme não existente.

Então, o que você deve fazer? A primeira correção é garantir que não apenas uma ID seja passada para a página, mas que a ID seja um número inteiro. Altere o código para o teste de `!IsPost` para que se pareça com este exemplo:

[!code-csharp[Main](updating-data/samples/sample14.cs)]

Você adicionou uma segunda condição ao teste de `IsEmpty`, vinculado com `&&` (AND lógico):

[!code-csharp[Main](updating-data/samples/sample15.cs)]

Você pode se lembrar do tutorial [introdução ao páginas da Web do ASP.net de programação](../introducing-razor-syntax-c.md) que métodos como `AsBool` um `AsInt` converter uma cadeia de caracteres em algum outro tipo de dados. O método `IsInt` (e outros, como `IsBool` e `IsDateTime`) são semelhantes. No entanto, eles são testados apenas se você *pode* converter a cadeia de caracteres, sem realmente executar a conversão. Aqui, você está basicamente dizendo *se o valor da cadeia de caracteres de consulta pode ser convertido em um inteiro...* .

O outro problema potencial é a procura por um filme que não existe. O código para obter um filme é semelhante a este código:

[!code-csharp[Main](updating-data/samples/sample16.cs)]

Se você passar um valor de `movieId` para o método `QuerySingle` que não corresponde a um filme real, nada será retornado e as instruções a seguir (por exemplo, `title=row.Title`) resultarão em erros.

Mais uma vez, há uma correção fácil. Se o método `db.QuerySingle` não retornar nenhum resultado, a variável `row` será nula. Portanto, você pode verificar se a variável de `row` é nula antes de tentar obter valores dela. O código a seguir adiciona um bloco de `if` em volta das instruções que obtêm os valores do objeto `row`:

[!code-csharp[Main](updating-data/samples/sample17.cs)]

Com esses dois testes de validação adicionais, a página se torna mais à prova de marcadores. O código completo para a ramificação `!IsPost` agora se parece com este exemplo:

[!code-csharp[Main](updating-data/samples/sample18.cs)]

Notaremos mais uma vez mais que essa tarefa é um bom uso para um bloco de `else`. Se os testes não passarem, os blocos de `else` definirão mensagens de erro.

## <a name="adding-a-link-to-return-to-the-movies-page"></a>Adicionando um link para retornar à página de filmes

Um detalhe final e útil é adicionar um link de volta à página de *filmes* . No fluxo de eventos comum, os usuários começarão na página *filmes* e clicarão em um link de **edição** . Isso os leva para a página *EditMovie* , na qual eles podem editar o filme e clicar no botão. Depois que o código processa a alteração, ele redireciona de volta para a página de *filmes* .

Porém:

- O usuário pode decidir não alterar nada.
- O usuário pode ter chegado a esta página sem primeiro clicar em um link de **edição** na página *filmes* .

De qualquer forma, você deseja facilitar para que eles retornem à listagem principal. É uma correção fácil &mdash; adicionar a marcação a seguir logo após a marcação de `</form>` de fechamento na marcação:

[!code-html[Main](updating-data/samples/sample19.html)]

Essa marcação usa a mesma sintaxe para um elemento `<a>` que você viu em outro lugar. A URL inclui `~` para significar "raiz do site".

## <a name="testing-the-movie-update-process"></a>Testando o processo de atualização do filme

Agora você pode testar. Execute a página *filmes* e clique em **Editar** ao lado de um filme. Quando a página *EditMovie* for exibida, faça alterações no filme e clique em **enviar alterações**. Quando a listagem de filmes for exibida, verifique se as alterações são mostradas.

Para verificar se a validação está funcionando, clique em **Editar** para outro filme. Quando chegar à página *EditMovie* , desmarque o campo **gênero** (ou o campo **year** ou ambos) e tente enviar suas alterações. Você verá um erro, como você esperaria:

![Editar página de filme mostrando erros de validação](updating-data/_static/image4.png)

Clique no link **retornar ao filme de listagem** para abandonar as alterações e retornar à página *filmes* .

## <a name="coming-up-next"></a>Chegando em seguida

No próximo tutorial, você verá como excluir um registro de filme.

## <a name="complete-listing-for-movie-page-updated-with-edit-links"></a>Listagem completa da página de filme (atualizada com links de edição)

[!code-cshtml[Main](updating-data/samples/sample20.cshtml)]

<a id="Complete_Page_Listing_for_EditMovie"></a>
## <a name="complete-page-listing-for-edit-movie-page"></a>Lista completa de páginas para editar filme

[!code-cshtml[Main](updating-data/samples/sample21.cshtml)]

## <a name="additional-resources"></a>Recursos adicionais

- [Introdução à programação da Web do ASP.NET usando a sintaxe do Razor](../../getting-started/introducing-razor-syntax-c.md)
- [Instrução SQL update](http://www.w3schools.com/sql/sql_update.asp) no site W3Schools

> [!div class="step-by-step"]
> [Anterior](entering-data.md)
> [Próximo](deleting-data.md)
