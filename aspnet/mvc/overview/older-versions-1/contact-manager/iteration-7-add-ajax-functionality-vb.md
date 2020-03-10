---
uid: mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-vb
title: '#7 de iteração – adicionar funcionalidade Ajax (VB) | Microsoft Docs'
author: microsoft
description: Na sétima iteração, melhoramos a capacidade de resposta e o desempenho do nosso aplicativo adicionando suporte para AJAX.
ms.author: riande
ms.date: 02/20/2009
ms.assetid: f640e063-150e-453d-8cfc-7e54a6ce0f1e
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-vb
msc.type: authoredcontent
ms.openlocfilehash: cee2b6e7c7517a1e03ae26d5233fc438857a030c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601650"
---
# <a name="iteration-7--add-ajax-functionality-vb"></a>#7 de iteração – adicionar funcionalidade Ajax (VB)

pela [Microsoft](https://github.com/microsoft)

[Código de download](iteration-7-add-ajax-functionality-vb/_static/contactmanager_7_vb1.zip)

> Na sétima iteração, melhoramos a capacidade de resposta e o desempenho do nosso aplicativo adicionando suporte para AJAX.

## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Criando um aplicativo MVC de gerenciamento de contatos ASP.NET (VB)

Nesta série de tutoriais, criamos um aplicativo de gerenciamento de contatos inteiro do início ao fim. O aplicativo Contact Manager permite armazenar informações de contato-nomes, números de telefone e endereços de email – para obter uma lista de pessoas.

Criamos o aplicativo em várias iterações. Com cada iteração, aprimoramos gradualmente o aplicativo. O objetivo dessa abordagem de várias iterações é permitir que você entenda o motivo de cada alteração.

- #1 de iteração – crie o aplicativo. Na primeira iteração, criamos o gerente de contatos da maneira mais simples possível. Adicionamos suporte para operações básicas de banco de dados: criar, ler, atualizar e excluir (CRUD).

- #2 de iteração – faça com que o aplicativo fique bom. Nessa iteração, melhoramos a aparência do aplicativo modificando a página mestra de exibição do ASP.NET MVC padrão e a folha de estilos em cascata.

- #3 de iteração – adicionar validação de formulário. Na terceira iteração, adicionamos a validação básica de formulário. Impedimos que as pessoas enviem um formulário sem concluir os campos de formulário necessários. Também validamos endereços de email e números de telefone.

- #4 de iteração-torne o aplicativo levemente acoplado. Nesta quarta iteração, aproveitamos os vários padrões de design de software para facilitar a manutenção e a modificação do aplicativo Contact Manager. Por exemplo, podemos refatorar nosso aplicativo para usar o padrão de repositório e o padrão de injeção de dependência.

- #5 de iteração – criar testes de unidade. Na quinta iteração, tornamos o nosso aplicativo mais fácil de manter e modificar adicionando testes de unidade. Simulamos nossas classes de modelo de dados e criamos testes de unidade para nossos controladores e lógica de validação.

- #6 de iteração – use o desenvolvimento controlado por testes. Na sexta-iteração, adicionamos nova funcionalidade ao nosso aplicativo escrevendo testes de unidade primeiro e escrevendo código em relação aos testes de unidade. Nessa iteração, adicionamos grupos de contatos.

- #7 de iteração – adicione funcionalidade Ajax. Na sétima iteração, melhoramos a capacidade de resposta e o desempenho do nosso aplicativo adicionando suporte para AJAX.

## <a name="this-iteration"></a>Esta iteração

Nesta iteração do aplicativo Contact Manager, podemos refatorar nosso aplicativo para fazer uso do Ajax. Aproveitando o Ajax, tornamos nosso aplicativo mais responsivo. Podemos evitar a renderização de uma página inteira quando precisamos atualizar apenas uma determinada região em uma página.

Vamos refatorar nossa exibição de índice para que não seja necessário reexibir a página inteira sempre que alguém selecionar um novo grupo de contatos. Em vez disso, quando alguém clicar em um grupo de contatos, apenas atualizaremos a lista de contatos e deixaremos o resto da página sozinha.

Também alteraremos a maneira como o nosso link de exclusão funciona. Em vez de exibir uma página de confirmação separada, vamos exibir uma caixa de diálogo de confirmação de JavaScript. Se você confirmar que deseja excluir um contato, uma operação HTTP DELETE será executada no servidor para excluir o registro de contato do banco de dados.

Além disso, aproveitaremos o jQuery para adicionar efeitos de animação à nossa exibição de índice. Vamos exibir uma animação quando a nova lista de contatos estiver sendo buscada a partir do servidor.

Por fim, aproveitaremos o suporte do ASP.NET AJAX Framework para gerenciar o histórico do navegador. Criaremos pontos de histórico sempre que executarmos uma chamada AJAX para atualizar a lista de contatos. Dessa forma, os botões para trás e para frente do navegador funcionarão.

## <a name="why-use-ajax"></a>Por que usar o Ajax?

O uso do AJAX tem muitos benefícios. Primeiro, a adição da funcionalidade Ajax a um aplicativo resulta em uma melhor experiência do usuário. Em um aplicativo Web normal, a página inteira deve ser postada de volta ao servidor cada e sempre que um usuário executar uma ação. Sempre que você executar uma ação, o navegador será bloqueado e o usuário deverá aguardar até que toda a página seja buscada e exibida novamente.

Essa seria uma experiência inaceitável no caso de um aplicativo de desktop. Mas, tradicionalmente, acabamos com essa experiência de usuário inadequada no caso de um aplicativo Web, pois não sabíamos que poderíamos fazer melhor. Achamos que era uma limitação de aplicativos Web quando, na realidade, era apenas uma limitação de nossas imaginaçãos.

Em um aplicativo AJAX, você não precisa levar a experiência do usuário a parar apenas para atualizar uma página. Em vez disso, você pode executar uma solicitação assíncrona em segundo plano para atualizar a página. Você não força o usuário a aguardar enquanto parte da página é atualizada.

Aproveitando o Ajax, você também pode melhorar o desempenho do seu aplicativo. Considere como o aplicativo Contact Manager funciona agora sem a funcionalidade do Ajax. Quando você clica em um grupo de contatos, o modo de exibição de índice inteiro deve ser exibido novamente. A lista de contatos e a lista de grupos de contatos devem ser recuperados do servidor de banco de dados. Todos esses dados devem ser passados pela conexão do servidor Web para o navegador da Web.

No entanto, depois de adicionarmos a funcionalidade Ajax ao nosso aplicativo, podemos evitar a reexibição da página inteira quando um usuário clica em um grupo de contatos. Não precisamos mais pegar os grupos de contatos do banco de dados. Também não precisamos enviar por push toda a exibição do índice pela conexão. Aproveitando o Ajax, reduzimos a quantidade de trabalho que nosso servidor de banco de dados deve executar e reduzimos a quantidade de tráfego de rede exigido pelo nosso aplicativo.

## <a name="don-t-be-afraid-of-ajax"></a>Don t não seja medo do AJAX

Alguns desenvolvedores evitam usar o Ajax porque se preocupam com navegadores de nível inferior. Eles querem garantir que seus aplicativos Web ainda funcionarão quando acessados por um navegador que não ofereça suporte a JavaScript. Como o Ajax depende do JavaScript, alguns desenvolvedores evitam o uso do Ajax.

No entanto, se você tiver cuidado ao implementar o Ajax, poderá criar aplicativos que funcionem com navegadores uplevel e de nível inferior. Nosso aplicativo Contact Manager funcionará com navegadores que dão suporte a JavaScript e navegadores que não o fazem.

Se você usar o aplicativo Contact Manager com um navegador que dê suporte a JavaScript, terá uma experiência de usuário melhor. Por exemplo, quando você clica em um grupo de contatos, somente a região da página que exibe os contatos será atualizada.

Se, por outro lado, você usar o aplicativo Contact Manager com um navegador que não ofereça suporte a JavaScript (ou que tenha o JavaScript desabilitado), terá uma experiência de usuário um pouco menos desejável. Por exemplo, quando você clica em um grupo de contatos, a exibição do índice inteiro deve ser enviada de volta ao navegador para exibir a lista correspondente de contatos.

## <a name="adding-the-required-javascript-files"></a>Adicionando os arquivos JavaScript necessários

Precisaremos usar três arquivos JavaScript para adicionar a funcionalidade Ajax ao nosso aplicativo. Todos esses três arquivos estão incluídos na pasta scripts de um novo aplicativo MVC ASP.NET.

Se você planeja usar o AJAX em várias páginas em seu aplicativo, faz sentido incluir os arquivos JavaScript necessários na página mestra de exibição do seu aplicativo. Dessa forma, os arquivos JavaScript serão incluídos em todas as páginas em seu aplicativo automaticamente.

Adicione o seguinte JavaScript inclui dentro da marca &lt;Head&gt; da sua página mestra de exibição:

[!code-html[Main](iteration-7-add-ajax-functionality-vb/samples/sample1.html)]

## <a name="refactoring-the-index-view-to-use-ajax"></a>Refatorando a exibição de índice para usar o Ajax

Vamos começar modificando nossa exibição de índice para que clicar em um grupo de contatos Atualize apenas a região da exibição que exibe os contatos. A caixa vermelha na Figura 1 contém a região que queremos atualizar.

[![atualizar somente contatos](iteration-7-add-ajax-functionality-vb/_static/image1.jpg)](iteration-7-add-ajax-functionality-vb/_static/image1.png)

**Figura 01**: Atualizando somente contatos ([clique para exibir a imagem em tamanho normal](iteration-7-add-ajax-functionality-vb/_static/image2.png))

A primeira etapa é separar a parte da exibição que queremos atualizar de forma assíncrona em um parcial separado (View User Control). A seção do modo de exibição de índice que exibe a tabela de contatos foi movida para o parcial na Listagem 1.

**Listagem 1-Views\Contact\ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample2.aspx)]

Observe que o parcial na Listagem 1 tem um modelo diferente do modo de exibição de índice. O atributo *Inherits* na diretiva &lt;% @ page%&gt; especifica que as herda parciais da classe&gt; do grupo de&lt;ViewUserControl.

A exibição de índice atualizada está contida na Listagem 2.

**Listagem 2-Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample3.aspx)]

Há duas coisas que você deve observar sobre a exibição atualizada na Listagem 2. Primeiro, observe que todo o conteúdo movido para o parcial é substituído por uma chamada para HTML. RenderPartial (). O método html. RenderPartial () é chamado quando a exibição do índice é solicitada pela primeira vez para exibir o conjunto inicial de contatos.

Em segundo lugar, observe que o HTML. ActionLink () usado para exibir os grupos de contatos foi substituído por um Ajax. ActionLink (). O Ajax. ActionLink () é chamado com os seguintes parâmetros:

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample4.aspx)]

O primeiro parâmetro representa o texto a ser exibido para o link, o segundo parâmetro representa os valores de rota e o terceiro parâmetro representa as opções do Ajax. Nesse caso, usamos a opção UpdateTargetId Ajax para apontar para a marca HTML &lt;div&gt; que desejamos atualizar após a conclusão da solicitação Ajax. Queremos atualizar a marca &lt;div&gt; com a nova lista de contatos.

O método de índice () atualizado do controlador de contato está contido na Listagem 3.

**Listagem 3-Controllers\ContactController.vb (método de índice)**

[!code-vb[Main](iteration-7-add-ajax-functionality-vb/samples/sample5.vb)]

A ação atualizar índice () condicionalmente retorna uma das duas coisas. Se a ação index () for invocada por uma solicitação Ajax, o controlador retornará um parcial. Caso contrário, a ação index () retornará uma exibição inteira.

Observe que a ação index () não precisa retornar tantos dados quando invocados por uma solicitação Ajax. No contexto de uma solicitação normal, a ação de índice retorna uma lista de todos os grupos de contatos e o grupo de contatos selecionado. No contexto de uma solicitação Ajax, a ação index () retorna apenas o grupo selecionado. Ajax significa menos trabalho no servidor de banco de dados.

Nossa exibição de índice modificada funciona no caso de navegadores uplevel e de nível inferior. Se você clicar em um grupo de contatos e seu navegador oferecer suporte a JavaScript, somente a região da exibição que contém a lista de contatos será atualizada. Se, por outro lado, o navegador não oferecer suporte a JavaScript, a exibição inteira será atualizada.

Nosso modo de exibição de índice atualizado tem um problema. Quando você clica em um grupo de contatos, o grupo selecionado não é realçado. Como a lista de grupos é exibida fora da região que é atualizada durante uma solicitação Ajax, o grupo correto não é realçado. Corrigiremos esse problema na próxima seção.

## <a name="adding-jquery-animation-effects"></a>Adicionando efeitos de animação jQuery

Normalmente, quando você clica em um link em uma página da Web, pode usar a barra de progresso do navegador para detectar se o navegador está ou não buscando ativamente o conteúdo atualizado. Ao executar uma solicitação Ajax, por outro lado, a barra de progresso do navegador não mostra nenhum progresso. Isso pode tornar os usuários preocupados. Como saber se o navegador está congelado?

Há várias maneiras que você pode indicar a um usuário que o trabalho está sendo executado durante a execução de uma solicitação Ajax. Uma abordagem é exibir uma animação simples. Por exemplo, você pode esmaecer uma região quando uma solicitação Ajax começa e esmaece na região quando a solicitação é concluída.

Usaremos a biblioteca jQuery, que está incluída com o Microsoft ASP.NET MVC Framework, para criar os efeitos de animação. A exibição de índice atualizada está contida na Listagem 4.

**Listagem 4-Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample6.aspx)]

Observe que a exibição de índice atualizada contém três novas funções JavaScript. As duas primeiras funções usam jQuery para esmaecer e esmaecer na lista de contatos quando você clica em um novo grupo de contatos. A terceira função exibe uma mensagem de erro quando uma solicitação Ajax resulta em um erro (por exemplo, tempo limite de rede).

A primeira função também cuida do realce do grupo selecionado. Um atributo Class = Selected é adicionado ao elemento pai (o elemento LI) do elemento clicado. Mais uma vez, o jQuery facilita a seleção do elemento correto e a adição da classe CSS.

Esses scripts estão vinculados aos links de grupo com a ajuda do parâmetro Ajax. ActionLink () AjaxOptions. A chamada do método Ajax. ActionLink () atualizada é parecida com esta:

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample7.aspx)]

## <a name="adding-browser-history-support"></a>Adicionando suporte ao histórico do navegador

Normalmente, quando você clica em um link para atualizar uma página, o histórico do navegador é atualizado. Dessa forma, você pode clicar no botão voltar do navegador para voltar no tempo para o estado anterior da página. Por exemplo, se você clicar no grupo de contatos amigos e, em seguida, clicar no grupo contato comercial, poderá clicar no botão voltar do navegador para navegar de volta para o estado da página quando o grupo de contatos amigos tiver sido selecionado.

Infelizmente, a execução de uma solicitação Ajax não atualiza o histórico do navegador automaticamente. Se você clicar em um grupo de contatos e a lista de contatos correspondentes for recuperada com uma solicitação Ajax, o histórico do navegador não será atualizado. Você não pode usar o botão voltar do navegador para navegar de volta para um grupo de contatos depois de selecionar um novo grupo de contatos.

Se você quiser que os usuários possam usar o botão voltar do navegador depois de executar as solicitações do Ajax, você precisará executar um pouco mais de trabalho. Você precisa tirar proveito da funcionalidade de gerenciamento de histórico do navegador criada na estrutura do ASP.NET AJAX.

ASP.NET o histórico do navegador AJAX, você precisa fazer três coisas:

1. Habilite o histórico do navegador definindo a propriedade enableBrowserHistory como true.
2. Salve os pontos do histórico quando o estado de uma exibição for alterado chamando o método addHistoryPoint ().
3. Reconstrua o estado da exibição quando o evento de navegação é gerado.

A exibição de índice atualizado está contida na listagem 5.

**Listagem 5-Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample8.aspx)]

Na listagem 5, o histórico do navegador é habilitado na função pageInit (). A função pageInit () também é usada para configurar o manipulador de eventos para o evento Navigate. O evento Navigate é gerado sempre que o botão Avançar ou voltar do navegador faz com que o estado da página seja alterado.

O método beginContactList () é chamado quando você clica em um grupo de contatos. Esse método cria um novo ponto de histórico chamando o método addHistoryPoint (). A ID do grupo de contatos clicada é adicionada ao histórico.

A ID do grupo é recuperada de um atributo expando no link do grupo de contatos. O link é processado com a chamada a seguir para AJAX. ActionLink ().

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample9.aspx)]

O último parâmetro passado para AJAX. ActionLink () adiciona um atributo expando chamado GroupId ao link (em minúsculas para compatibilidade com XHTML).

Quando um usuário pressiona o botão voltar ou avançar do navegador, o evento Navigate é gerado e o método Navigate () é chamado. Esse método atualiza os contatos exibidos na página para corresponder ao estado da página que corresponde ao ponto de histórico do navegador passado para o método Navigate.

## <a name="performing-ajax-deletes"></a>Executando exclusões do AJAX

Atualmente, para excluir um contato, você precisa clicar no link excluir e, em seguida, clicar no botão excluir exibido na página excluir confirmação (consulte a Figura 2). Isso parece muitas solicitações de página para fazer algo simples, como excluir um registro de banco de dados.

[![a página de confirmação de exclusão](iteration-7-add-ajax-functionality-vb/_static/image2.jpg)](iteration-7-add-ajax-functionality-vb/_static/image3.png)

**Figura 02**: a página de confirmação de exclusão ([clique para exibir a imagem em tamanho normal](iteration-7-add-ajax-functionality-vb/_static/image4.png))

É tentador ignorar a página de confirmação de exclusão e excluir um contato diretamente da exibição de índice. Você deve evitar essa tentação porque levar essa abordagem abre seu aplicativo para brechas de segurança. Em geral, você não deseja executar uma operação HTTP GET ao invocar uma ação que modifica o estado do seu aplicativo Web. Ao executar uma exclusão, você deseja executar um HTTP POST, ou melhor ainda, uma operação HTTP DELETE.

O link de exclusão está contido na ContactList parcial. Uma versão atualizada da lista de contatos parcial está contida na Listagem 6.

**Listagem 6-Views\Contact\ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample10.aspx)]

O link de exclusão é processado com a seguinte chamada para o método Ajax. ImageActionLink ():

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample11.aspx)]

> [!NOTE] 
> 
> O Ajax. ImageActionLink () não é uma parte padrão da estrutura MVC do ASP.NET. O Ajax. ImageActionLink () é um método auxiliar personalizado incluído no projeto Contact Manager.

O parâmetro AjaxOptions tem duas propriedades. Primeiro, a propriedade Confirm é usada para exibir uma caixa de diálogo de confirmação de JavaScript pop-up. Em segundo lugar, a propriedade HttpMethod é usada para executar uma operação HTTP DELETE.

A listagem 7 contém uma nova ação AjaxDelete () que foi adicionada ao controlador de contato.

**Listagem 7-Controllers\ContactController.vb (AjaxDelete)**   

[!code-vb[Main](iteration-7-add-ajax-functionality-vb/samples/sample12.vb)]

A ação AjaxDelete () é decorada com um atributo AcceptVerbs. Esse atributo impede que a ação seja invocada, exceto por qualquer operação HTTP diferente de uma operação HTTP DELETE. Em particular, não é possível invocar essa ação com um HTTP GET.

Depois de excluir o registro do banco de dados, você precisa exibir a lista atualizada de contatos que não contém o registro excluído. O método AjaxDelete () retorna a lista de contatos parciais e atualizada.

## <a name="summary"></a>Resumo

Nessa iteração, adicionamos a funcionalidade Ajax ao nosso aplicativo Contact Manager. Usamos o Ajax para melhorar a capacidade de resposta e o desempenho de nosso aplicativo.

Primeiro, refatorei a exibição de índice para que clicar em um grupo de contatos não atualize a exibição inteira. Em vez disso, clicar em um grupo de contatos apenas atualiza a lista de contatos.

Em seguida, usamos os efeitos de animação jQuery para esmaecer e esmaecer na lista de contatos. Adicionar animação a um aplicativo AJAX pode ser usado para fornecer aos usuários do aplicativo o equivalente de uma barra de progresso do navegador.

Também adicionamos suporte a histórico de navegador ao nosso aplicativo AJAX. Permitimos que os usuários clicassem nos botões voltar e avançar do navegador para alterar o estado da exibição do índice.

Por fim, criamos um link de exclusão que dá suporte a operações HTTP DELETE. Ao executar exclusões do Ajax, habilitamos os usuários a excluir registros de banco de dados sem exigir que o usuário solicite uma página de confirmação de exclusão adicional.

> [!div class="step-by-step"]
> [Anterior](iteration-6-use-test-driven-development-vb.md)
