---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: Introdução ao Páginas da Web do ASP.NET-excluindo dados do banco de dados | Microsoft Docs
author: Rick-Anderson
description: Este tutorial mostra como excluir uma entrada de banco de dados individual. Ele pressupõe que você concluiu a série por meio da atualização de dados do banco de dados no ASP.NET Web PA...
ms.author: riande
ms.date: 01/02/2018
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: c8620fc1abc61d514bdc039c66f7a84e67e89abe
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78629034"
---
# <a name="introducing-aspnet-web-pages---deleting-database-data"></a>Apresentando Páginas da Web do ASP.NET-excluindo dados do banco

por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial mostra como excluir uma entrada de banco de dados individual. Ele pressupõe que você concluiu a série por meio da [atualização de dados do banco de dados no páginas da Web do ASP.net](updating-data.md).
> 
> O que você aprenderá:
> 
> - Como selecionar um registro individual de uma lista de registros.
> - Como excluir um único registro de um banco de dados.
> - Como verificar se um botão específico foi clicado em um formulário.
>   
> 
> Recursos/tecnologias abordados:
> 
> - O auxiliar de `WebGrid`.
> - O comando SQL `Delete`.
> - O método `Database.Execute` para executar um comando SQL `Delete`.

## <a name="what-youll-build"></a>O que você vai construir

No tutorial anterior, você aprendeu a atualizar um registro de banco de dados existente. Este tutorial é semelhante, exceto que, em vez de atualizar o registro, você o excluirá. Os processos são muito semelhantes, exceto que a exclusão é mais simples, portanto, este tutorial será curto.

Na página *filmes* , você atualizará o `WebGrid` auxiliar para que ele exiba um link de **exclusão** ao lado de cada filme para acompanhar o link de **edição** adicionado anteriormente.

![Página de filmes mostrando um link de exclusão para cada filme](deleting-data/_static/image1.png)

Assim como acontece com a edição, quando você clica no link **excluir** , ele leva para uma página diferente, onde as informações do filme já estão em um formato:

![Excluir página de filme com um filme exibido](deleting-data/_static/image2.png)

Em seguida, você pode clicar no botão para excluir o registro permanentemente.

## <a name="adding-a-delete-link-to-the-movie-listing"></a>Adicionando um link de exclusão à listagem de filmes

Você começará adicionando um link de **exclusão** ao auxiliar de `WebGrid`. Esse link é semelhante ao link de **edição** que você adicionou em um tutorial anterior.

Abra o arquivo *Movies. cshtml* .

Altere a marcação de `WebGrid` no corpo da página adicionando uma coluna. Aqui está a marcação modificada:

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

A nova coluna é esta:

[!code-html[Main](deleting-data/samples/sample2.html)]

A maneira como a grade é configurada, a coluna de **edição** fica mais à esquerda na grade e a coluna **excluir** é a mais à direita. (Há uma vírgula após a `Year` coluna agora, caso você não tenha notado isso.) Não há nada de especial sobre onde essas colunas de link vão, e você poderia colocá-las facilmente ao lado umas das outras. Nesse caso, eles são separados para torná-los mais difíceis de serem misturados.

![Página de filmes com links de edição e detalhes marcados para mostrar que eles não estão próximos uns dos outros](deleting-data/_static/image3.png)

A nova coluna mostra um link (`<a>` elemento) cujo texto diz "excluir". O destino do link (seu atributo `href`) é o código que, em última instância, é resolvido para algo como essa URL, com o valor de `id` diferente para cada filme:

[!code-css[Main](deleting-data/samples/sample3.css)]

Esse link invocará uma página chamada *DeleteMovie* e passará a ID do filme que você selecionou.

Este tutorial não entrará em detalhes sobre como esse link é construído, pois ele é quase idêntico ao link de **edição** do tutorial anterior ([atualizando dados de banco em páginas da Web do ASP.net](updating-data.md)).

## <a name="creating-the-delete-page"></a>Criando a página excluir

Agora você pode criar a página que será o destino do link de **exclusão** na grade.

> [!NOTE] 
> 
> **Importante** A técnica de primeiro selecionar um registro para excluir e, em seguida, usar uma página e um botão separados para confirmar se o processo é extremamente importante para a segurança. Como você leu nos tutoriais anteriores, fazer *qualquer* tipo de alteração em seu site *sempre* deve ser feito usando um formulário &mdash; ou seja, usando uma operação http post. Se você tornou possível alterar o site apenas clicando em um link (ou seja, usando uma operação GET), as pessoas podem fazer solicitações simples ao seu site e excluir seus dados. Até mesmo um rastreador de mecanismo de pesquisa que está indexando seu site pode excluir dados inadvertidamente apenas pelos links a seguir.
> 
> Quando seu aplicativo permite que as pessoas alterem um registro, você precisa apresentar o registro ao usuário para edição de qualquer forma. Mas você pode estar tentado a ignorar esta etapa para excluir um registro. No entanto, não pule essa etapa. (Também é útil para os usuários ver o registro e confirmar que estão excluindo o registro que pretendiam.)
> 
> Em um conjunto de tutorial subsequente, você verá como adicionar a funcionalidade de logon para que um usuário precise fazer logon antes de excluir um registro.

Crie uma página chamada *DeleteMovie. cshtml* e substitua o que está no arquivo pela seguinte marcação:

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

Essa marcação é como as páginas *EditMovie* , exceto que, em vez de usar caixas de texto (`<input type="text">`), a marcação inclui elementos `<span>`. Não há nada aqui para editar. Tudo o que você precisa fazer é exibir os detalhes do filme para que os usuários possam ter certeza de que estão excluindo o filme correto.

A marcação já contém um link que permite que o usuário retorne à página de listagem de filmes.

Como na página *EditMovie* , a ID do filme selecionado é armazenada em um campo oculto. (Ele é passado para a página em primeiro lugar como um valor de cadeia de caracteres de consulta.) Há uma chamada `Html.ValidationSummary` que exibirá erros de validação. Nesse caso, o erro pode ser que nenhuma ID de filme foi passada para a página ou que a ID do filme seja inválida. Essa situação pode ocorrer se alguém executar essa página sem primeiro selecionar um filme na página *filmes* .

A legenda do botão é **excluir filme**e seu atributo Name é definido como `buttonDelete`. O atributo `name` será usado no código para identificar o botão que enviou o formulário.

Você precisará escrever código para 1) ler os detalhes do filme quando a página for exibida pela primeira vez e 2), na verdade, excluir o filme quando o usuário clicar no botão.

## <a name="adding-code-to-read-a-single-movie"></a>Adicionando código para ler um único filme

Na parte superior da página *DeleteMovie. cshtml* , adicione o seguinte bloco de código:

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

Essa marcação é igual ao código correspondente na página *EditMovie* . Ele obtém a ID do filme da cadeia de caracteres de consulta e usa a ID para ler um registro do banco de dados. O código inclui o teste de validação (`IsInt()` e `row != null`) para garantir que a ID do filme que está sendo passada para a página seja válida.

Lembre-se de que esse código só deve ser executado na primeira vez em que a página for executada. Você não deseja ler novamente o registro do filme do banco de dados quando o usuário clica no botão **excluir filme** . Portanto, o código para ler o filme está dentro de um teste que diz `if(!IsPost)` &mdash; ou seja, *se a solicitação não for uma operação post (envio de formulário)* .

## <a name="adding-code-to-delete-the-selected-movie"></a>Adicionando código para excluir o filme selecionado

Para excluir o filme quando o usuário clicar no botão, adicione o seguinte código apenas dentro da chave de fechamento do bloco de `@`:

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

Esse código é semelhante ao código para atualizar um registro existente, mas mais simples. O código basicamente executa uma instrução SQL `Delete`.

 Como na página *EditMovie* , o código está em um bloco de `if(IsPost)`. Desta vez, a condição de `if()` é um pouco mais complicada: 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

Há duas condições aqui. A primeira é que a página está sendo enviada, como você viu antes de &mdash; `if(IsPost)`.

A segunda condição é `!Request["buttonDelete"].IsEmpty()`, o que significa que a solicitação tem um objeto chamado `buttonDelete`. Reconhecidamente, é uma maneira indireta de testar qual botão enviou o formulário. Se um formulário contiver vários botões de envio, somente o nome do botão que foi clicado aparecerá na solicitação. Portanto, logicamente, se o nome de um botão específico for exibido na solicitação &mdash; ou conforme indicado no código, se esse botão não estiver vazio &mdash; é o botão que enviou o formulário.

O operador de `&&` significa "and" (and lógico). Portanto, toda a condição de `if` é...

*Esta solicitação é uma postagem (não é uma solicitação de primeira vez)*  
  
 AND  
  
*O botão de* `buttonDelete`*foi o botão que enviou o formulário.*

Esse formulário (na verdade, essa página) contém apenas um botão, portanto, o teste adicional para `buttonDelete` tecnicamente não é necessário. Ainda assim, você está prestes a executar uma operação que removerá permanentemente os dados. Portanto, você deve ter a certeza de que está executando a operação somente quando o usuário a solicitou explicitamente. Por exemplo, suponha que você expandiu essa página posteriormente e adicionou outros botões a ela. Mesmo assim, o código que exclui o filme será executado somente se o botão de `buttonDelete` foi clicado.

Como na página *EditMovie* , você obtém a ID do campo oculto e, em seguida, executa o comando SQL. A sintaxe para a instrução `Delete` é:

`DELETE FROM table WHERE ID = value`

É vital incluir a cláusula `WHERE` e a ID. Se você sair da cláusula WHERE, *todos os registros na tabela serão excluídos*. Como já foi visto, você passa o valor de ID para o comando SQL usando um espaço reservado.

## <a name="testing-the-movie-delete-process"></a>Testando o processo de exclusão do filme

Agora você pode testar. Execute a página *filmes* e clique em **excluir** ao lado de um filme. Quando a página *DeleteMovie* for exibida, clique em **excluir filme**.

![Página excluir filme com o botão excluir filme realçado](deleting-data/_static/image4.png)

Quando você clica no botão, o código exclui os filmes e retorna para a listagem de filmes. Lá, você pode pesquisar o filme excluído e confirmar que ele foi excluído.

## <a name="coming-up-next"></a>Chegando em seguida

O próximo tutorial mostra como dar a você uma aparência e um layout comuns a todas as páginas de seu site.

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a>Listagem completa da página de filme (atualizada com links de exclusão)

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a>Listagem completa da página DeleteMovie

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a>Recursos adicionais

- [Introdução à programação da Web do ASP.NET usando a sintaxe do Razor](../introducing-razor-syntax-c.md)
- [Instrução SQL Delete](http://www.w3schools.com/sql/sql_delete.asp) no site W3Schools

> [!div class="step-by-step"]
> [Anterior](updating-data.md)
> [Próximo](layouts.md)
