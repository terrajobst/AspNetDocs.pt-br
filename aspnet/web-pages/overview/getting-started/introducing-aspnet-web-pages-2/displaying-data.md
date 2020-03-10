---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
title: Introdução à Páginas da Web do ASP.NET-exibindo dados | Microsoft Docs
author: Rick-Anderson
description: Este tutorial mostra como criar um banco de dados do no WebMatrix e como exibir o banco de dados em uma página quando você usa Páginas da Web do ASP.NET (Razor). Ele assume y...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: b3a006a0-3ea2-4d45-b833-e20e3a3c0a1a
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
msc.type: authoredcontent
ms.openlocfilehash: 9e665ca8dd064c23a8b8bd3593014969d0c3da48
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78525217"
---
# <a name="introducing-aspnet-web-pages---displaying-data"></a>Introdução à Páginas da Web do ASP.NET-exibindo dados

por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial mostra como criar um banco de dados do no WebMatrix e como exibir o banco de dados em uma página quando você usa Páginas da Web do ASP.NET (Razor). Ele pressupõe que você concluiu a série por meio da [introdução à programação de páginas da Web do ASP.net](../introducing-razor-syntax-c.md).
> 
> O que você aprenderá:
> 
> - Como usar as ferramentas do WebMatrix para criar um banco de dados e tabelas de banco de dados.
> - Como usar as ferramentas do WebMatrix para adicionar dados a um banco de dado.
> - Como exibir dados do banco de dados em uma página.
> - Como executar comandos SQL no Páginas da Web do ASP.NET.
> - Como personalizar o auxiliar de `WebGrid` para alterar a exibição de dados e adicionar paginação e classificação.
>   
> 
> Recursos/tecnologias abordados:
> 
> - Ferramentas de banco de dados do WebMatrix.
> - `WebGrid` auxiliar.

## <a name="what-youll-build"></a>O que você vai construir

No tutorial anterior, você foi apresentado a Páginas da Web do ASP.NET (arquivos *. cshtml* ), aos fundamentos do sintaxe Razor e aos auxiliares. Neste tutorial, você começará a criar o aplicativo Web real que será usado para o restante da série. O aplicativo é um aplicativo de filme simples que permite exibir, adicionar, alterar e excluir informações sobre filmes.

Quando terminar este tutorial, você poderá exibir uma listagem de filmes semelhante a esta página:

![Exibição do WebGrid que inclui parâmetros definidos para nomes de classe CSS](displaying-data/_static/image1.png)

Mas, para começar, você precisa criar um banco de dados.

## <a name="a-very-brief-introduction-to-databases"></a>Uma introdução breve aos bancos de dados

Este tutorial fornecerá apenas a introdução mais breve aos bancos de dados. Se você tiver experiência com o banco de dados, poderá ignorar esta pequena seção.

Um banco de dados contém uma ou mais tabelas que contêm informações &mdash; por exemplo, tabelas para clientes, pedidos e fornecedores, ou para alunos, professores, classes e notas. Estruturalmente, uma tabela de banco de dados é como uma planilha. Imagine um catálogo de endereços típico. Para cada entrada no catálogo de endereços (ou seja, para cada pessoa), você tem várias informações, como nome, sobrenome, endereço, endereço de email e número de telefone.

![Exemplo de tabela de banco de dados como uma grade simples](displaying-data/_static/image2.png)

(Às vezes, as linhas são chamadas de *registros*, e as colunas são, às vezes, chamadas de *campos*.)

Para a maioria das tabelas de banco de dados, a tabela deve ter uma coluna que contenha um valor exclusivo, como um número de cliente, um número de conta e assim por diante. Esse valor é conhecido como a *chave primária*da tabela e você o usa para identificar cada linha na tabela. No exemplo, a coluna ID é a chave primária para o catálogo de endereços mostrado no exemplo anterior.

Grande parte do trabalho que você faz em aplicativos Web consiste em ler as informações do banco de dados e exibi-las em uma página. Geralmente, você também coletará informações de usuários e as adicionará a um banco de dados ou modificará os registros que já estão no banco de dados. (Abordaremos todas essas operações no decorrer deste tutorial definido.)

O trabalho do banco de dados pode ser enormemente complexo e pode exigir conhecimento especializado. Para este tutorial definido, no entanto, você precisa entender apenas os conceitos básicos, que serão todos explicados conforme o uso.

## <a name="creating-a-database"></a>Criar um banco de dados

O WebMatrix inclui ferramentas que facilitam a criação de um banco de dados e a criação de tabelas no banco de dados. (A estrutura de um banco de dados é chamada de *esquema*do banco de dados.) Para este tutorial definido, você criará um banco de dados que tem apenas uma tabela em &mdash; filmes.

Abra o WebMatrix se você ainda não tiver feito isso e abra o site do WebPagesMovies que você criou no tutorial anterior.

No painel esquerdo, clique no espaço de trabalho **banco de dados** .

![Guia espaço de trabalho do WebMatrix](displaying-data/_static/image3.png)

A faixa de faixas muda para mostrar tarefas relacionadas ao banco de dados. Na faixa de faixas, clique em **novo banco de dados**.

![Botão "novo banco de dados" na faixa de Ribbon do WebMatrix](displaying-data/_static/image4.png)

O WebMatrix cria um banco de dados SQL Server CE (um arquivo *. sdf* ) que tem o mesmo nome que o seu site &mdash; *WebPagesMovies. sdf*. (Você não fará isso aqui, mas poderá renomear o arquivo para qualquer coisa que desejar, desde que tenha uma extensão *. sdf* ).

## <a name="creating-a-table"></a>Criando uma tabela

Na faixa de faixas, clique em **nova tabela**. O WebMatrix abre o designer de tabela em uma nova guia. (se a nova opção de tabela não estiver disponível, verifique se o novo banco de dados está selecionado no modo de exibição de árvore à esquerda.)

![Botão ' nova tabela ' na faixa de ' do WebMatrix](displaying-data/_static/image5.png)

Na caixa de texto na parte superior (em que a marca d' água diz "inserir nome da tabela"), insira "filmes".

![Inserindo um nome de tabela no designer de banco de dados do WebMatrix](displaying-data/_static/image6.png)

O painel sob o nome da tabela é onde você define colunas individuais. Para a tabela de *filmes* neste tutorial, você criará apenas algumas colunas: *ID*, *título*, *gênero*e *ano*.

Na caixa **nome** , digite "ID". Inserir um valor aqui ativa todos os controles para a nova coluna.

Na lista **tipo de dados** e escolha **int**. Esse valor especifica que a coluna de ID conterá dados inteiros (número).

> [!NOTE]
> Não vamos chamá-lo mais aqui (muito), mas você pode usar gestos de teclado padrão do Windows para navegar nessa grade. Por exemplo, você pode fazer uma Tabulação entre os campos, basta começar a digitar para selecionar um item em uma lista e assim por diante.

Na caixa **valor padrão** (ou seja, deixe em branco). Na caixa de seleção **é chave primária** e selecione-a. Essa opção informa ao banco de dados que a coluna de *ID* conterá o dado que identifica linhas individuais. (Ou seja, cada linha terá um valor exclusivo na coluna de ID que você pode usar para localizar essa linha.)

Escolha a opção **is Identity** . Essa opção informa ao banco de dados que ele deve gerar automaticamente o próximo número sequencial para cada nova linha. (A opção **is Identity** só funcionará se você também tiver selecionado a opção **is Primary Key** .)

Clique na próxima linha de grade ou pressione TAB duas vezes para concluir a linha atual. Qualquer um dos gestos salva a linha atual e inicia a próxima. Observe que a coluna **valor padrão** agora diz **NULL**. (NULL é o valor padrão para o valor padrão, para falar.)

Quando você terminar de definir a nova coluna de **ID** , o designer se parecerá com esta ilustração:

![Designer de banco de dados do WebMatrix depois de definir a coluna de ID para a tabela de filmes](displaying-data/_static/image7.png)

Para criar a próxima coluna, clique na caixa na coluna **nome** . Digite "title" para a coluna e, em seguida, selecione **nvarchar** para o valor do **tipo de dados** . A parte de "var" do **nvarchar** informa ao banco de dados que a data dessa coluna será uma cadeia de caracteres cujo tamanho pode variar de registro para registro. (O prefixo "n" representa "National", que indica que o campo pode conter dados de caracteres para qualquer alfabeto ou sistema de escrita — ou seja, o campo contém dados Unicode.)

Quando você escolhe **nvarchar**, outra caixa é exibida, onde você pode inserir o comprimento máximo para o campo. Insira 50, supondo que nenhum título de filme com o qual você trabalhará neste tutorial terá mais de 50 caracteres.

Ignore o **valor padrão** e desmarque a opção **permitir nulos** . Você não quer que o banco de dados permita que nenhum filme seja inserido no banco de dados que não tenha um título.

Quando terminar e passar para a próxima linha, o designer será semelhante a esta ilustração:

![Designer de banco de dados do WebMatrix depois de definir a coluna de título para a tabela de filmes](displaying-data/_static/image8.png)

Repita essas etapas para criar uma coluna denominada "gênero", exceto para o comprimento, defina-a como apenas 30.

Crie outra coluna chamada "Year". Para o tipo de dados, escolha **nchar** (não **nvarchar**) e defina o comprimento como 4. Para o ano, você usará um número de 4 dígitos, como "1995" ou "2010", para não precisar de uma coluna de tamanho variável.

Esta é a aparência do design concluído:

![Designer de banco de dados do WebMatrix após a definição de todos os campos para a tabela de filmes](displaying-data/_static/image9.png)

Pressione Ctrl + S ou clique no botão **salvar** na barra de ferramentas de acesso rápido. Feche o designer de banco de dados fechando a guia.

## <a name="adding-some-example-data"></a>Adicionando alguns dados de exemplo

Posteriormente nesta série de tutoriais, você criará uma página na qual poderá inserir novos filmes em um formulário. Por enquanto, no entanto, você pode adicionar alguns dados de exemplo que você pode exibir em uma página.

No espaço de trabalho **banco de dados** no WebMatrix, observe que há uma árvore que mostra o arquivo *. sdf* criado anteriormente. Abra o nó para o novo arquivo *. sdf* e, em seguida, abra o nó **tabelas** .

![Espaço de trabalho de banco de dados do WebMatrix com árvore aberta para tabela de filmes](displaying-data/_static/image10.png)

Clique com o botão direito do mouse no nó **filmes** e escolha **dados**. O WebMatrix abre uma grade na qual você pode inserir dados para a tabela de *filmes* :

![Grade de entrada de banco de dados no WebMatrix (vazia)](displaying-data/_static/image11.png)

Clique na coluna **título** e insira "quando Jaime atendeu a Sally". Mova para a coluna **gênero** (você pode usar a tecla Tab) e digite "romântico comédia". Mova para a coluna **year** e insira "1989":

![Grade de entrada de banco de dados no WebMatrix com um registro](displaying-data/_static/image12.png)

Pressione Enter e o WebMatrix salvará o novo filme. Observe que a coluna **ID** foi preenchida.

![Grade de entrada de banco de dados no WebMatrix com um registro e uma ID gerada automaticamente](displaying-data/_static/image13.png)

Insira outro filme (por exemplo, "desaparecido com o vento", "inativo", "1939"). A coluna ID é preenchida novamente:

![Grade de entrada de banco de dados no WebMatrix com dois registros e IDs geradas automaticamente](displaying-data/_static/image14.png)

Insira um terceiro filme (por exemplo, "Ghostbusters", "comédia"). Como um experimento, deixe a coluna **year** em branco e pressione Enter. Como você desselecionou a opção **permitir nulos** , o banco de dados mostra um erro:

![Erro de ' dados inválidos ' exibido se um valor de coluna necessário for deixado em branco](displaying-data/_static/image15.png)

Clique em **OK** para voltar e corrigir a entrada (o ano de "Ghostbusters" é 1984) e pressione Enter.

Preencha vários filmes até que você tenha 8. (Inserir 8 torna mais fácil trabalhar com a paginação mais tarde. Mas se isso for muito grande, insira apenas alguns por enquanto.) Os dados reais não são importantes.

![Grade de entrada de banco de dados no WebMatrix com registros](displaying-data/_static/image16.png)

Se você inseriu todos os filmes sem erros, os valores de ID são sequenciais. Se você tentou salvar um registro de filme incompleto, os números de ID podem não ser sequenciais. Nesse caso, tudo bem. Os números não têm nenhum significado inerente, e a única coisa importante é que eles são exclusivos na tabela de *filmes* .

Feche a guia que contém o designer de banco de dados.

Agora você pode ativar a exibição desses dados em uma página da Web.

## <a name="displaying-data-in-a-page-by-using-the-webgrid-helper"></a>Exibindo dados em uma página usando o auxiliar do WebGrid

Para exibir dados em uma página, você vai usar o auxiliar de `WebGrid`. Esse auxiliar produz uma exibição em uma grade ou tabela (linhas e colunas). Como você verá, você poderá refinar a grade com formatação e outros recursos.

Para executar a grade, você precisará escrever algumas linhas de código. Essas poucas linhas servirão como um tipo de padrão para quase todo o acesso a dados que você fizer neste tutorial.

> [!NOTE]
> Você realmente tem muitas opções para exibir dados em uma página; o auxiliar de `WebGrid` é apenas um. Escolhemos isso para este tutorial porque é a maneira mais fácil de exibir dados e porque é razoavelmente flexível. No próximo conjunto de tutorial, você verá como usar uma maneira mais "manual" para trabalhar com dados na página, o que lhe dá um controle mais direto sobre como exibir os dados.

No painel esquerdo no WebMatrix, clique no espaço de trabalho **arquivos** .

O novo banco de dados que você criou está na pasta *Data\_app* . Se a pasta ainda não existir, o WebMatrix a criou para o novo banco de dados. (A pasta pode ter existido se você instalou os auxiliares anteriormente.)

No modo de exibição de árvore, selecione a raiz do site. Você deve selecionar a raiz do site; caso contrário, o novo arquivo pode ser adicionado à pasta de dados do\_de aplicativos.

Na faixa de faixas, clique em **novo**. Na caixa **escolher um tipo de arquivo** , escolha **cshtml**.

Na caixa **nome** , nomeie a nova página "Movies. cshtml":

![Caixa de diálogo ' escolher um tipo de arquivo ' mostrando a página ' filmes '](displaying-data/_static/image17.png)

Clique no botão **OK**. O WebMatrix abre um novo arquivo com alguns elementos de esqueleto nele. Primeiro, você escreverá um código para obter os dados do banco de dado. Em seguida, você adicionará marcação à página para realmente exibir os dados.

### <a name="writing-the-data-query-code"></a>Gravando o código de consulta de dados

Na parte superior da página, entre os caracteres `@{` e `}`, insira o código a seguir. (Certifique-se de inserir esse código entre as chaves de abertura e fechamento.)

[!code-csharp[Main](displaying-data/samples/sample1.cs)]

A primeira linha abre o banco de dados que você criou anteriormente, que é sempre a primeira etapa antes de fazer algo com o banco de dados. Você informa ao nome do método de `Database.Open` do banco de dados a ser aberto. Observe que você não inclui o *. sdf* no nome. O método `Open` pressupõe que ele está procurando um arquivo *. sdf* (ou seja, *WebPagesMovies. sdf*) e que o arquivo *. sdf* está na pasta de *dados do aplicativo\_* . (Anteriormente, observamos que a pasta de *dados de\_de aplicativo* está reservada; esse cenário é um dos lugares onde ASP.net faz suposições sobre esse nome.)

Quando o banco de dados é aberto, uma referência a ele é colocada na variável chamada `db`. (Que pode ser nomeado como qualquer coisa.) A variável `db` é como você acabará interagindo com o banco de dados.

A segunda linha, na verdade, busca os dados do banco usando o método `Query`. Observe como esse código funciona: a variável `db` tem uma referência ao banco de dados aberto e você invoca o método `Query` usando a variável `db` (`db.Query`).

A própria consulta é uma instrução SQL `Select`. (Para obter uma pequena experiência sobre o SQL, consulte a explicação mais tarde.) Na instrução, `Movies` identifica a tabela a ser consultada. O caractere `*` especifica que a consulta deve retornar todas as colunas da tabela. (Você também pode listar colunas individualmente, separadas por vírgulas.)

Os resultados da consulta, se houver, são retornados e disponibilizados na variável `selectedData`. Novamente, a variável pode ser nomeada como qualquer coisa.

Por fim, a terceira linha informa ASP.NET que você deseja usar uma instância do auxiliar de `WebGrid`. Você cria (*instancia*) o objeto auxiliar usando a palavra-chave `new` e passa os resultados da consulta por meio da variável `selectedData`. O novo objeto `WebGrid`, junto com os resultados da consulta de banco de dados, são disponibilizados na variável `grid`. Você precisará desse resultado em alguns instantes para exibir os dados na página.

Neste estágio, o banco de dados foi aberto, você obteve os dados desejados e preparou o auxiliar de `WebGrid` com esses dados. A seguir, é criar a marcação na página.

> [!TIP] 
> 
> **SQL (Structured Query Language)**
> 
> O SQL é uma linguagem que é usada na maioria dos bancos de dados relacionais para o gerenciamento em um banco de dado. Ele inclui comandos que permitem recuperar dados e atualizá-los, e isso permite criar, modificar e gerenciar dados em tabelas de banco de dado. O SQL é diferente de uma linguagem de programação C#(como). Com o SQL, você informa ao banco de dados o que deseja, e é o trabalho do banco de dado para descobrir como obter ou executar a tarefa. Aqui estão exemplos de alguns comandos SQL e o que eles fazem:
> 
> `Select * From Movies`
> 
> `SELECT ID, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> A primeira instrução `Select` obtém todas as colunas (especificadas por `*`) da tabela de *filmes* .
> 
> A segunda instrução `Select` busca a ID, o nome e as colunas de preço dos registros na tabela de *produtos* cujo valor da coluna de preço é maior que 10. O comando retorna os resultados em ordem alfabética com base nos valores da coluna Name. Se nenhum registro corresponder aos critérios de preço, o comando retornará um conjunto vazio.
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ('Croissant', 'A flaky delight', 1.99)`
> 
> Esse comando insere um novo registro na tabela *Product* , definindo a coluna Name como "croissant", a coluna Description como "a instável fascinam" e o preço como 1,99.
> 
> Observe que quando você está especificando um valor não numérico, o valor é colocado entre aspas simples (não aspas duplas, como em C#). Você usa essas aspas ao contrário de valores de texto ou de data, mas não de números.
> 
> `DELETE FROM Product WHERE ExpirationDate < '01/01/2008'`
> 
> Este comando exclui os registros na tabela *Product* cuja coluna data de expiração é anterior a 1º de janeiro de 2008. (O comando pressupõe que a tabela de *produtos* tem uma coluna como essa, é claro). A data é inserida aqui no formato MM/DD/AAAA, mas deve ser inserida no formato usado para sua localidade.
> 
> Os comandos `Insert` e `Delete` não retornam conjuntos de resultados. Em vez disso, eles retornam um número que informa quantos registros foram afetados pelo comando.
> 
> Para algumas dessas operações (como inserção e exclusão de registros), o processo que está solicitando a operação deve ter as permissões apropriadas no banco de dados. É por isso que, para bancos de dados de produção, você geralmente precisa fornecer um nome de usuário e uma senha ao se conectar ao banco de dados.
> 
> Há dezenas de comandos SQL, mas todos seguem um padrão como os comandos que você vê aqui. Você pode usar comandos SQL para criar tabelas de banco de dados, contar o número de registros em uma tabela, calcular preços e executar muitas outras operações.

### <a name="adding-markup-to-display-the-data"></a>Adicionando marcação para exibir os dados

Dentro do elemento `<head>`, defina o conteúdo do elemento `<title>` como "filmes":

[!code-html[Main](displaying-data/samples/sample2.html?highlight=3)]

Dentro do elemento `<body>` da página, adicione o seguinte:

[!code-html[Main](displaying-data/samples/sample3.html)]

É isso. A variável `grid` é o valor que você criou quando criou o objeto `WebGrid` no código anterior.

No modo de exibição de árvore do WebMatrix, clique com o botão direito do mouse na página e selecione **Iniciar no navegador**. Você verá algo semelhante a esta página:

![Saída do auxiliar do WebGrid padrão da tabela de filmes](displaying-data/_static/image18.png)

Clique em um link de título de coluna para classificar por essa coluna. Ser capaz de classificar clicando em um título é um recurso interno do auxiliar do **WebGrid** .

O método `GetHtml`, como seu nome sugere, gera marcação que exibe os dados. Por padrão, o método `GetHtml` gera um elemento HTML `<table>`. (Se desejar, você pode verificar a renderização examinando a origem da página no navegador.)

## <a name="modifying-the-look-of-the-grid"></a>Modificando a aparência da grade

Usar o `WebGrid` auxiliar como você acabou de ser feito é fácil, mas a exibição resultante é simples. O auxiliar de `WebGrid` tem todos os tipos de opções que permitem controlar como os dados são exibidos. Há muito o que explorar neste tutorial, mas esta seção lhe dará uma ideia de algumas dessas opções. Algumas opções adicionais serão abordadas nos tutoriais posteriores desta série.

### <a name="specifying-individual-columns-to-display"></a>Especificando colunas individuais para exibição

Para começar, você pode especificar que deseja exibir apenas determinadas colunas. Por padrão, como você viu, a grade mostra todas as quatro colunas da tabela de *filmes* .

No arquivo *Movies. cshtml* , substitua a marcação `@grid.GetHtml()` que você acabou de adicionar com o seguinte:

[!code-css[Main](displaying-data/samples/sample4.css)]

Para informar ao auxiliar quais colunas exibir, você inclui um parâmetro `columns` para o método `GetHtml` e passa uma coleção de colunas. Na coleção, você especifica cada coluna a ser incluída. Você especifica uma coluna individual a ser exibida, incluindo um objeto `grid.Column` e passa o nome da coluna de dados desejada. (Essas colunas devem ser incluídas nos resultados da consulta SQL — o auxiliar de `WebGrid` não pode exibir colunas que não foram retornadas pela consulta.)

Inicie novamente a página *Movies. cshtml* no navegador e, desta vez, você receberá uma exibição semelhante à seguinte (Observe que nenhuma coluna de ID é exibida):

![Exibição do WebGrid mostrando apenas as colunas selecionadas](displaying-data/_static/image19.png)

### <a name="changing-the-look-of-the-grid"></a>Alterando a aparência da grade

Há algumas outras opções para exibir colunas, algumas das quais serão exploradas em Tutoriais posteriores neste conjunto. Por enquanto, esta seção apresentará as maneiras pelas quais você pode estilizar a grade como um todo.

Dentro da seção `<head>` da página, logo antes da marca `</head>` de fechamento, adicione o seguinte elemento `<style>`:

[!code-css[Main](displaying-data/samples/sample5.css)]

Essa marcação CSS define classes chamadas `grid`, `head`e assim por diante. Você também pode colocar essas definições de estilo em um arquivo *. css* separado e vinculá-la à página. (Na verdade, você fará isso mais tarde neste conjunto de tutorial.) Mas para facilitar as coisas para este tutorial, elas estão dentro da mesma página que exibe os dados.

Agora você pode obter o auxiliar de `WebGrid` para usar essas classes de estilo. O auxiliar tem várias propriedades (por exemplo, `tableStyle`) apenas para essa finalidade — você atribui um nome de classe de estilo CSS a eles e esse nome de classe é renderizado como parte da marcação que é renderizada pelo auxiliar.

Altere a marcação `grid.GetHtml` para que agora ela se pareça com este código:

[!code-css[Main](displaying-data/samples/sample6.css)]

O que há de novo aqui é que você adicionou parâmetros `tableStyle`, `headerStyle`e `alternatingRowStyle` ao método `GetHtml`. Esses parâmetros foram definidos para os nomes dos estilos CSS que você adicionou há algum tempo.

Execute a página e, desta vez, você verá uma grade que parece muito menos simples do que antes:

![Exibição do WebGrid que inclui parâmetros definidos para nomes de classe CSS](displaying-data/_static/image20.png)

Para ver o que o método `GetHtml` gerou, você pode examinar a origem da página no navegador. Não entraremos em detalhes aqui, mas o ponto importante é que, ao especificar parâmetros como `tableStyle`, você fez com que a grade gerasse marcas HTML como as seguintes:

`<table class="grid">`

A marca de `<table>` teve um atributo `class` adicionado a ele que faz referência a uma das regras de CSS que você adicionou anteriormente. Esse código mostra o padrão básico &mdash; parâmetros diferentes para o método `GetHtml` permitem que você referencie classes CSS que o método gera, em seguida, junto com a marcação. O que você faz com as classes CSS cabe a você.

## <a name="adding-paging"></a>Adicionando paginação

Como a última tarefa deste tutorial, você adicionará a paginação à grade. No momento, não há problema em Exibir todos os seus filmes de uma só vez. Mas, se você tiver adicionado centenas de filmes, a exibição da página chegará longa.

No código da página, altere a linha que cria o objeto `WebGrid` para o código a seguir:

[!code-csharp[Main](displaying-data/samples/sample7.cs)]

A única diferença de Before é que você adicionou um parâmetro `rowsPerPage` que é definido como 3.

Execute a página. A grade exibe 3 linhas por vez, além de links de navegação que permitem que você percorra os filmes em seu banco de dados:

![Exibição do WebGrid com paginação](displaying-data/_static/image21.png)

## <a name="coming-up-next"></a>Chegando em seguida

No próximo tutorial, você aprenderá a usar o Razor e C# o código para obter a entrada do usuário em um formulário. Você adicionará uma caixa de pesquisa à página filmes para que possa encontrar filmes por título ou gênero.

## <a name="complete-listing-for-movies-page"></a>Listagem completa da página de filmes

[!code-cshtml[Main](displaying-data/samples/sample8.cshtml)]

## <a name="additional-resources"></a>Recursos adicionais

- [Introdução à programação da Web do ASP.NET usando a sintaxe do Razor](https://go.microsoft.com/fwlink/?LinkID=202890)

> [!div class="step-by-step"]
> [Anterior](intro-to-web-pages-programming.md)
> [Próximo](form-basics.md)
