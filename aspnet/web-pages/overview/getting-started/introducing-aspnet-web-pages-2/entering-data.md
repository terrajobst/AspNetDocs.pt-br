---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
title: Introdução à Páginas da Web do ASP.NET-inserindo dados de banco usando formulários | Microsoft Docs
author: Rick-Anderson
description: Este tutorial mostra como criar um formulário de entrada e, em seguida, inserir os dados obtidos do formulário em uma tabela de banco de dado quando você usa Páginas da Web do ASP.NET (...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: d37c93fc-25fd-4e94-8671-0d437beef206
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
msc.type: authoredcontent
ms.openlocfilehash: b9354a7b97a7df9020a681f709e16a92650cfcf0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78624533"
---
# <a name="introducing-aspnet-web-pages---entering-database-data-by-using-forms"></a>Introdução à Páginas da Web do ASP.NET-inserindo dados de banco usando formulários

por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial mostra como criar um formulário de entrada e, em seguida, inserir os dados obtidos do formulário em uma tabela do banco de dados quando você usa o Páginas da Web do ASP.NET (Razor). Ele pressupõe que você tenha concluído a série por meio [de noções básicas de formulários HTML no páginas da Web do ASP.net](https://go.microsoft.com/fwlink/?LinkId=251581).
> 
> O que você aprenderá:
> 
> - Mais sobre como processar formulários de entrada.
> - Como adicionar (inserir) dados em um banco de dado.
> - Como certificar-se de que os usuários inseriram um valor necessário em um formulário (como validar a entrada do usuário).
> - Como exibir erros de validação.
> - Como ir para outra página da página atual.
>   
> 
> Recursos/tecnologias abordados:
> 
> - O método `Database.Execute`.
> - A instrução SQL `Insert Into`
> - O auxiliar de `Validation`.
> - O método `Response.Redirect`.

## <a name="what-youll-build"></a>O que você vai construir

No tutorial anterior que mostrou como criar um banco de dados, você inseriu o banco de dado, editando-o diretamente no WebMatrix, trabalhando **no espaço de** trabalho. No entanto, na maioria dos aplicativos, essa não é uma maneira prática de colocar dados no banco de dados. Portanto, neste tutorial, você criará uma interface baseada na Web que permite que você ou qualquer pessoa insira dados e salve-os no banco de dado.

Você criará uma página onde poderá inserir novos filmes. A página conterá um formulário de entrada com campos (caixas de texto) em que você pode inserir um título, gênero e ano do filme. A página se parecerá com esta página:

![Página ' Adicionar filme ' no navegador](entering-data/_static/image1.png)

As caixas de texto serão HTML `<input>` elementos que se parecerão com esta marcação:

`<input type="text" name="genre" value="" />`

## <a name="creating-the-basic-entry-form"></a>Criando o formulário de entrada básico

Crie uma página chamada *addmovie. cshtml*.

Substitua o que está no arquivo pela marcação a seguir. Substituir tudo; Você adicionará um bloco de código na parte superior em breve.

[!code-cshtml[Main](entering-data/samples/sample1.cshtml)]

Este exemplo mostra HTML típico para criar um formulário. Ele usa elementos `<input>` para as caixas de texto e para o botão enviar. As legendas para as caixas de texto são criadas usando elementos de `<label>` padrão. Os elementos `<fieldset>` e `<legend>` colocam uma ótima caixa ao lado do formulário.

Observe que nessa página, o elemento `<form>` usa `post` como o valor para o atributo `method`. No tutorial anterior, você criou um formulário que usou o método `get`. Isso estava correto, porque embora o formulário tenha enviado valores para o servidor, a solicitação não fez nenhuma alteração. Tudo o que ele tinha foi buscar dados de maneiras diferentes. No entanto, nesta página você *fará* alterações — você vai adicionar novos registros de banco de dados. Portanto, esse formulário deve usar o método `post`. (Para obter mais informações sobre a diferença entre as operações de `GET` e `POST`, consulte a barra lateral de[segurança do verbo GET, post e http](https://go.microsoft.com/fwlink/?LinkId=251581#GET,_POST,_and_HTTP_Verb_Safety) no tutorial anterior.)

Observe que cada caixa de texto tem um elemento `name` (`title`, `genre``year`). Como vimos no tutorial anterior, esses nomes são importantes porque você deve ter esses nomes para poder obter a entrada do usuário mais tarde. Você pode usar qualquer nome. É útil usar nomes significativos que o ajudarão a se lembrar de quais dados você está trabalhando.

O atributo `value` de cada elemento `<input>` contém um pouco de código Razor (por exemplo, `Request.Form["title"]`). Você aprendeu uma versão desse truque no tutorial anterior para preservar o valor inserido na caixa de texto (se houver) após o envio do formulário.

## <a name="getting-the-form-values"></a>Obtendo os valores do formulário

Em seguida, você adiciona o código que processa o formulário. Em contorno, você fará o seguinte:

1. Verifique se a página está sendo postada (enviada). Você deseja que seu código seja executado somente quando os usuários clicarem no botão, não quando a página for executada pela primeira vez.
2. Obtenha os valores inseridos pelo usuário nas caixas de texto. Nesse caso, como o formulário está usando o `POST` verbo, você obtém os valores de formulário da coleção de `Request.Form`.
3. Insira os valores como um novo registro na tabela de banco de dados de *filmes* .

Na parte superior do arquivo, adicione o seguinte código:

[!code-cshtml[Main](entering-data/samples/sample2.cshtml)]

As primeiras linhas criam variáveis (`title`, `genre`e `year`) para manter os valores das caixas de texto. A linha `if(IsPost)` garante que as variáveis sejam definidas *somente* quando os usuários clicarem no botão **Adicionar filme** — ou seja, quando o formulário tiver sido postado.

Como vimos em um tutorial anterior, você obtém o valor de uma caixa de texto usando uma expressão como `Request.Form["name"]`, em que *Name* é o nome do elemento `<input>`.

Os nomes das variáveis (`title`, `genre`e `year`) são arbitrários. Como os nomes que você atribui a elementos `<input>`, você pode chamá-los de qualquer coisa que desejar. (Os nomes das variáveis não precisam corresponder aos atributos de nome dos elementos `<input>` no formulário.) Mas, assim como acontece com os elementos de `<input>`, é uma boa ideia usar nomes de variáveis que reflitam os dados que eles contêm. Quando você escreve o código, os nomes consistentes facilitam a memorização dos dados com os quais você está trabalhando.

## <a name="adding-data-to-the-database"></a>Adicionando dados ao banco de dado

No bloco de código que você acabou de adicionar, apenas *dentro* da chave de fechamento (`}`) do bloco de `if` (não apenas dentro do bloco de código), adicione o seguinte código:

[!code-csharp[Main](entering-data/samples/sample3.cs)]

Este exemplo é semelhante ao código usado em um tutorial anterior para buscar e exibir dados. A linha que começa com `db =` abre o banco de dados, como antes, e a próxima linha define uma instrução SQL, novamente como visto anteriormente. No entanto, desta vez ele define uma instrução SQL `Insert Into`. O exemplo a seguir mostra a sintaxe geral da instrução `Insert Into`:

`INSERT INTO table (column1, column2, column3, ...) VALUES (value1, value2, value3, ...)`

Em outras palavras, especifique a tabela a ser inserida e, em seguida, liste as colunas a serem inseridas e, em seguida, liste os valores a serem inseridos. (Como observado anteriormente, o SQL não diferencia maiúsculas de minúsculas, mas algumas pessoas capitalizam as palavras-chave para facilitar a leitura do comando.)

As colunas que você está inserindo já estão listadas no comando — `(Title, Genre, Year)`. A parte interessante é como você obtém os valores das caixas de texto na `VALUES` parte do comando. Em vez de valores reais, você vê `@0`, `@1`e `@2`, que são de espaços reservados do curso. Ao executar o comando (na linha de `db.Execute`), você passa os valores obtidos nas caixas de texto.

**Importante!** Lembre-se de que a única maneira de incluir dados inseridos online por um usuário em uma instrução SQL é usar espaços reservados, como você pode ver aqui (`VALUES(@0, @1, @2)`). Se você concatenar a entrada do usuário em uma instrução SQL, você se abre em um ataque de injeção de SQL, conforme explicado em [noções básicas de formulário no páginas da Web do ASP.net](https://go.microsoft.com/fwlink/?LinkId=251581) (o tutorial anterior).

Ainda dentro do bloco de `if`, adicione a seguinte linha após a linha de `db.Execute`:

[!code-css[Main](entering-data/samples/sample4.css)]

Depois que o novo filme tiver sido inserido no banco de dados, essa linha saltará você (redireciona) para a página de *filmes* para que você possa ver o filme que acabou de inserir. O operador de `~` significa "raiz do site". (O operador `~` só funciona em páginas ASP.NET, e não em HTML geralmente.)

O bloco de código completo é semelhante a este exemplo:

[!code-cshtml[Main](entering-data/samples/sample5.cshtml)]

## <a name="testing-the-insert-command-so-far"></a>Testando o comando Insert (até agora)

Você ainda não terminou, mas agora é um bom momento para testar.

No modo de exibição de árvore de arquivos no WebMatrix, clique com o botão direito do mouse na página *addmovie. cshtml* e clique em **Iniciar no navegador**.

![Página ' Adicionar filme ' no navegador](entering-data/_static/image2.png)

(Se você terminar com uma página diferente no navegador, certifique-se de que a URL é `http://localhost:nnnnn/AddMovie`), em que *nnnnn* é o número da porta que você está usando.)

Você obteve uma página de erro? Nesse caso, leia com cuidado e verifique se o código é exatamente o que foi listado anteriormente.

Insira um filme no formato &mdash; por exemplo, use "Cidadão Kane", "de baixo" e "1941". (Ou qualquer coisa.) Em seguida, clique em **Adicionar filme**.

Se tudo correr bem, você será redirecionado para a página *filmes* . Verifique se o novo filme está listado.

![Página de filmes mostrando o filme recém adicionado](entering-data/_static/image3.png)

## <a name="validating-user-input"></a>Validando entradas de usuário

Volte para a página *addmovie* ou execute-a novamente. Insira outro filme, mas, desta vez, insira apenas o título &mdash; por exemplo, digite "Singin' na chuva". Em seguida, clique em **Adicionar filme**.

Você será redirecionado para a página de *filmes* novamente. Você pode encontrar o novo filme, mas ele está incompleto.

![Página de filmes mostrando um novo filme que não tem alguns valores](entering-data/_static/image4.png)

Quando você criou a tabela de *filmes* , disse explicitamente que nenhum dos campos pode ser nulo. Aqui você tem um formulário de entrada para novos filmes e está deixando os campos em branco. Isso é um erro.

Nesse caso, o banco de dados não gerou (ou *gerava*) um erro. Você não forneceu um gênero ou um ano, portanto, o código na página *addmovie* tratava esses valores como chamadas *cadeias de caracteres vazias*. Quando o comando SQL `Insert Into` é executado, os campos gênero e year não tinham dados úteis neles, mas não eram nulos.

Obviamente, você não deseja permitir que os usuários insiram informações de filmes semivazias no banco de dados. A solução é validar a entrada do usuário. Inicialmente, a validação simplesmente garantirá que o usuário tenha inserido um valor para todos os campos (ou seja, que nenhum deles contenha uma cadeia de caracteres vazia).

> [!TIP]
> 
> **Cadeias de caracteres nulas e vazias**
> 
> Em programação, há uma distinção entre as diferentes noções de "nenhum valor". Em geral, um valor será *nulo* se nunca tiver sido definido ou inicializado de forma alguma. Por outro lado, uma variável que espera dados de caractere (cadeias de caracteres) pode ser definida como uma *cadeia de caracteres vazia*. Nesse caso, o valor não é nulo; Ele acabou de ser definido explicitamente como uma cadeia de caracteres cujo comprimento é zero. Essas duas instruções mostram a diferença:
> 
> [!code-csharp[Main](entering-data/samples/sample6.cs)]
> 
> É um pouco mais complicado do que isso, mas o ponto importante é que `null` representa um tipo de estado indeterminado.
> 
> Agora, é importante entender exatamente quando um valor é nulo e quando é apenas uma cadeia de caracteres vazia. No código da página *addmovie* , você obtém os valores das caixas de texto usando `Request.Form["title"]` e assim por diante. Quando a página é executada pela primeira vez (antes de você clicar no botão), o valor de `Request.Form["title"]` é nulo. Mas quando você envia o formulário, `Request.Form["title"]` Obtém o valor da caixa de texto `title`. Não é óbvio, mas uma caixa de texto vazia não é nula; Ele tem apenas uma cadeia de caracteres vazia. Assim, quando o código é executado em resposta ao clique do botão, `Request.Form["title"]` tem uma cadeia de caracteres vazia.
> 
> Por que essa distinção é importante? Quando você criou a tabela de *filmes* , disse explicitamente que nenhum dos campos pode ser nulo. Mas aqui você tem um formulário de entrada para novos filmes e está deixando os campos em branco. Você esperaria que o banco de dados reclamar quando tentou salvar novos filmes que não tinham valores para gênero ou year. Mas esse é o ponto &mdash; mesmo que você deixe essas caixas de texto em branco, os valores não são nulos; Elas são cadeias de caracteres vazias. Como resultado, você pode salvar novos filmes no banco de dados com essas colunas vazias &mdash; mas não nulas! valores &mdash;. Portanto, você precisa certificar-se de que os usuários não enviem uma cadeia de caracteres vazia, o que pode ser feito validando a entrada do usuário.

### <a name="the-validation-helper"></a>O auxiliar de validação

O Páginas da Web do ASP.NET inclui um auxiliar &mdash; o &mdash; auxiliar do `Validation` que você pode usar para garantir que os usuários insiram dados que atendam às suas necessidades. O auxiliar de `Validation` é um dos auxiliares que é integrado ao Páginas da Web do ASP.NET, de modo que você não precisa instalá-lo como um pacote usando o NuGet, como você instalou o auxiliar Gravatar em um tutorial anterior.

Para validar a entrada do usuário, você fará o seguinte:

- Use o código para especificar que você deseja exigir valores nas caixas de texto da página.
- Coloque um teste no código para que as informações do filme sejam adicionadas ao banco de dados somente se tudo for validado corretamente.
- Adicione o código à marcação para exibir mensagens de erro.

No bloco de código da página *addmovie* , diretamente na parte superior antes das declarações de variável, adicione o seguinte código:

[!code-csharp[Main](entering-data/samples/sample7.cs)]

Você chama `Validation.RequireField` uma vez para cada campo (elemento`<input>`) em que você deseja exigir uma entrada. Você também pode adicionar uma mensagem de erro personalizada para cada chamada, como você vê aqui. (Variamos as mensagens apenas para mostrar que você pode colocar qualquer coisa que desejar.)

Se houver um problema, você desejará impedir que as novas informações do filme sejam inseridas no banco de dados. No bloco de `if(IsPost)`, use `&&` (AND lógico) para adicionar outra condição que teste `Validation.IsValid()`. Quando terminar, o bloco de `if(IsPost)` inteiro será semelhante a este código:

[!code-csharp[Main](entering-data/samples/sample8.cs)]

Se houver um erro de validação com qualquer um dos campos que você registrou usando o auxiliar de `Validation`, o método `Validation.IsValid` retornará false. E nesse caso, nenhum código nesse bloco será executado, portanto, nenhuma entrada de filme inválida será inserida no banco de dados. E, claro, você não é redirecionado para a página de *filmes* .

O bloco de código completo, incluindo o código de validação, agora é semelhante a este exemplo:

[!code-cshtml[Main](entering-data/samples/sample9.cshtml?highlight=10)]

## <a name="displaying-validation-errors"></a>Exibindo erros de validação

A última etapa é exibir todas as mensagens de erro. Você pode exibir mensagens individuais para cada erro de validação ou pode exibir um resumo, ou ambos. Para este tutorial, você fará ambos para que possa ver como ele funciona.

Ao lado de cada elemento `<input>` que você está validando, chame o método `Html.ValidationMessage` e passe-o o nome do elemento `<input>` que você está validando. Você coloca o método `Html.ValidationMessage` logo em que deseja que a mensagem de erro seja exibida. Quando a página é executada, o método `Html.ValidationMessage` processa um elemento `<span>` em que o erro de validação será usado. (Se não houver erro, o elemento `<span>` será renderizado, mas não há nenhum texto nele.)

Altere a marcação na página para que ela inclua um método `Html.ValidationMessage` para cada um dos três elementos `<input>` na página, como neste exemplo:

[!code-cshtml[Main](entering-data/samples/sample10.cshtml?highlight=3,8,13)]

Para ver como o resumo funciona, adicione também a seguinte marcação e código logo após o elemento `<h1>Add a Movie</h1>` na página:

[!code-cshtml[Main](entering-data/samples/sample11.cshtml)]

Por padrão, o método `Html.ValidationSummary` exibe todas as mensagens de validação em uma lista (um elemento `<ul>` que está dentro de um elemento `<div>`). Assim como com o método `Html.ValidationMessage`, a marcação para o resumo de validação é sempre renderizada; Se não houver erros, nenhum item de lista será renderizado.

O resumo pode ser uma maneira alternativa de exibir mensagens de validação em vez de usar o método `Html.ValidationMessage` para exibir cada erro específico de campo. Ou você pode usar um resumo e os detalhes. Ou você pode usar o método `Html.ValidationSummary` para exibir um erro genérico e, em seguida, usar chamadas `Html.ValidationMessage` individuais para exibir detalhes.

A página completa agora se parece com este exemplo:

[!code-cshtml[Main](entering-data/samples/sample12.cshtml)]

É isso. Agora você pode testar a página adicionando um filme, mas deixando um ou mais campos. Ao fazer isso, você verá a seguinte exibição de erro:

![Adicionar página de filme mostrando mensagens de erro de validação](entering-data/_static/image5.png)

## <a name="styling-the-validation-error-messages"></a>Estilizando as mensagens de erro de validação

Você pode ver que há mensagens de erro, mas elas não realmente se destacam muito bem. No entanto, há uma maneira fácil de estilizar as mensagens de erro.

Para estilizar as mensagens de erro individuais que são exibidas pelo `Html.ValidationMessage`, crie uma classe de estilo CSS chamada `field-validation-error`. Para definir a aparência do Resumo de validação, crie uma classe de estilo CSS chamada `validation-summary-errors`.

Para ver como essa técnica funciona, adicione um elemento `<style>` dentro da seção `<head>` da página. Em seguida, defina classes de estilo chamadas `field-validation-error` e `validation-summary-errors` que contenham as seguintes regras:

[!code-cshtml[Main](entering-data/samples/sample13.cshtml?highlight=4-17)]

Normalmente, você provavelmente colocaria as informações de estilo em um arquivo *. css* separado, mas, para simplificar, você pode colocá-las na página por enquanto. (Posteriormente neste conjunto de tutorial, você moverá as regras de CSS para um arquivo *. css* separado.)

Se houver um erro de validação, o método `Html.ValidationMessage` renderizará um elemento `<span>` que inclui `class="field-validation-error"`. Ao adicionar uma definição de estilo para essa classe, você pode configurar a aparência da mensagem. Se houver erros, o método `ValidationSummary` da mesma forma dinamicamente processará o atributo `class="validation-summary-errors"`.

Execute a página novamente e desmarque deliberadamente alguns campos. Os erros agora são mais perceptíveis. (Na verdade, eles são abuso, mas isso é apenas para mostrar o que você pode fazer.)

![Adicionar página de filme mostrando erros de validação que foram estilizados](entering-data/_static/image6.png)

## <a name="adding-a-link-to-the-movies-page"></a>Adicionando um link à página de filmes

Uma etapa final é tornar conveniente chegar à página *addmovie* da listagem de filmes original.

Abra a página de *filmes* novamente. Após a marcação de `</div>` de fechamento que segue o auxiliar de `WebGrid`, adicione a seguinte marcação:

[!code-cshtml[Main](entering-data/samples/sample14.cshtml)]

Como vimos anteriormente, o ASP.NET interpreta o operador de `~` como a raiz do site. Você não precisa usar o operador de `~`; Você pode usar o `<a href="./AddMovie">Add a movie</a>` de marcação ou alguma outra maneira de definir o caminho que o HTML entende. Mas o operador de `~` é uma boa abordagem geral quando você cria links para páginas Razor, pois torna o site mais flexível — se você mover a página atual para uma subpasta, o link ainda vai para a página *addmovie* . (Lembre-se de que o operador de `~` só funciona em páginas *. cshtml* . ASP.NET entende isso, mas não é HTML padrão.)

Quando terminar, execute a página *filmes* . Ele se parecerá com esta página:

![Página de filmes com link para a página ' Adicionar filmes '](entering-data/_static/image7.png)

Clique no link **Adicionar um filme** para certificar-se de que ele vá para a página *addmovie* .

## <a name="coming-up-next"></a>Chegando em seguida

No próximo tutorial, você aprenderá a permitir que os usuários editem dados que já estão no banco de dado.

## <a name="complete-listing-for-addmovie-page"></a>Listagem completa para a página addmovie

[!code-cshtml[Main](entering-data/samples/sample15.cshtml)]

## <a name="additional-resources"></a>Recursos adicionais

- [Introdução à programação da Web do ASP.NET usando a sintaxe do Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Instrução SQL INSERT INTO](http://www.w3schools.com/sql/sql_insert.asp) no site W3Schools
- [Validando a entrada do usuário em páginas da Web do ASP.net sites](https://go.microsoft.com/fwlink/?LinkId=253002). Mais informações sobre como trabalhar com o auxiliar de `Validation`.

> [!div class="step-by-step"]
> [Anterior](form-basics.md)
> [Próximo](updating-data.md)
