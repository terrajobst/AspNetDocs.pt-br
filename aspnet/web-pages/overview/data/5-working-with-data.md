---
uid: web-pages/overview/data/5-working-with-data
title: Introdução ao trabalho com um banco de dados em sites Páginas da Web do ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Este capítulo descreve como acessar dados de um banco de dado e exibi-los usando Páginas da Web do ASP.NET.
ms.author: riande
ms.date: 02/18/2014
ms.assetid: 673d502f-2c16-4a6f-bb63-dbfd9a77ef47
msc.legacyurl: /web-pages/overview/data/5-working-with-data
msc.type: authoredcontent
ms.openlocfilehash: 45e988d037465e59ad352bb9444af2c69fd3cd70
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78586530"
---
# <a name="introduction-to-working-with-a-database-in-aspnet-web-pages-razor-sites"></a>Introdução ao trabalho com um banco de dados em sites Páginas da Web do ASP.NET (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo descreve como usar as ferramentas do Microsoft WebMatrix para criar um banco de dados em um site Páginas da Web do ASP.NET (Razor) e como criar páginas que permitem exibir, adicionar, editar e excluir dados.
> 
> **O que você aprenderá:** 
> 
> - Como criar um banco de dados.
> - Como se conectar a um banco de dados.
> - Como exibir dados em uma página da Web.
> - Como inserir, atualizar e excluir registros de banco de dados.
> 
> Estes são os recursos apresentados no artigo:
> 
> - Trabalhando com um banco de dados do Microsoft SQL Server Compact Edition.
> - Trabalhando com consultas SQL.
> - A classe `Database`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 2
> - WebMatrix 2
>   
> 
> Este tutorial também funciona com o WebMatrix 3. Você pode usar Páginas da Web do ASP.NET 3 e Visual Studio 2013 (ou Visual Studio Express 2013 para Web); no entanto, a interface do usuário será diferente.

## <a name="introduction-to-databases"></a>Introdução aos bancos de dados

Imagine um catálogo de endereços típico. Para cada entrada no catálogo de endereços (ou seja, para cada pessoa), você tem várias informações, como nome, sobrenome, endereço, endereço de email e número de telefone.

Uma maneira comum de imagem de dados como essa é uma tabela com linhas e colunas. Em termos de banco de dados, cada linha é geralmente chamada de registro. Cada coluna (às vezes conhecida como campos) contém um valor para cada tipo de dados: nome, sobrenome e assim por diante.

| **ID** | **Nome** | **Sobrenome** | **Endereço** | **Email** | **Telefone** |
| --- | --- | --- | --- | --- | --- |
| 1 | Jim | Abrus | 210 100º St SE orcas WA 98031 | jim@contoso.com | 555 0100 |
| 2 | Terry | Adams | 1234 principal St. Seattle WA 99011 | terry@cohowinery.com | 555 0101 |

Para a maioria das tabelas de banco de dados, a tabela deve ter uma coluna que contenha um identificador exclusivo, como um número de cliente, um número de conta, etc. Isso é conhecido como a *chave primária*da tabela e você o usa para identificar cada linha na tabela. No exemplo, a coluna ID é a chave primária para o catálogo de endereços.

Com essa compreensão básica dos bancos de dados, você está pronto para aprender a criar um banco de dados simples e executar operações, como adicionar, modificar e excluir.

> [!TIP] 
> 
> **Bancos de dados relacionais**
> 
> Você pode armazenar dados de várias maneiras, incluindo arquivos de texto e planilhas. No entanto, para a maioria dos usos comerciais, os dados são armazenados em um banco de dados relacional.
> 
> Este artigo não se aprofunda muito em bancos de dados. No entanto, pode ser útil entender um pouco sobre eles. Em um banco de dados relacional, as informações são divididas logicamente em tabelas separadas. Por exemplo, um banco de dados para uma escola pode conter tabelas separadas para estudantes e ofertas de classe. O software de banco de dados (como SQL Server) oferece suporte a comandos poderosos que permitem estabelecer relações dinamicamente entre as tabelas. Por exemplo, você pode usar o banco de dados relacional para estabelecer uma relação lógica entre alunos e classes a fim de criar uma agenda. O armazenamento de dados em tabelas separadas reduz a complexidade da estrutura da tabela e reduz a necessidade de manter dados redundantes em tabelas.

## <a name="creating-a-database"></a>Criar um banco de dados

Este procedimento mostra como criar um banco de dados chamado SmallBakery usando a ferramenta de design de banco de dados SQL Server Compact que está incluída no WebMatrix. Embora você possa criar um banco de dados usando código, é mais comum criar o banco de dados e as tabelas de banco de dados usando uma ferramenta de design como o WebMatrix.

1. Inicie o WebMatrix e, na página Início Rápido, clique em **site do modelo**.
2. Selecione **site vazio**e, na caixa **nome do site** , digite "SmallBakery" e clique em **OK**. O site é criado e exibido no WebMatrix.
3. No painel esquerdo, clique no espaço de trabalho **bancos de dados** .
4. Na faixa de faixas, clique em **novo banco de dados**. Um banco de dados vazio é criado com o mesmo nome do seu site.
5. No painel esquerdo, expanda o nó **SmallBakery. sdf** e clique em **tabelas**.
6. Na faixa de faixas, clique em **nova tabela**. O WebMatrix abre o designer de tabela.

    ![[imagem]](5-working-with-data/_static/image1.jpg)
7. Clique na coluna **nome** e insira &quot;ID&quot;.
8. Na coluna **tipo de dados** , selecione **int**.
9. Define a **chave primária?** e **está identificando?** opções para **Sim**.

    Como o nome sugere, a **chave primária** informa ao banco de dados que essa será a chave primária da tabela. **Is Identity** diz ao banco de dados para criar automaticamente um número de identificação para cada novo registro e atribuir a ele o próximo número sequencial (começando em 1).
10. Clique na próxima linha. O editor inicia uma nova definição de coluna.
11. Para o valor de nome, insira &quot;nome&quot;.
12. Para **tipo de dados**, escolha &quot;nvarchar&quot; e defina o comprimento como 50. A parte de *var* de `nvarchar` informa ao banco de dados que o dado para essa coluna será uma cadeia de caracteres cujo tamanho pode variar de registro para registro. (O prefixo *n* representa *National*, indicando que o campo pode conter dados de caractere que representem qualquer alfabeto &#8212; ou sistema de escrita, que o campo contém dados Unicode.)
13. Defina a opção **permitir nulos** como **não**. Isso irá impor que a coluna de *nome* não seja deixada em branco.
14. Usando esse mesmo processo, crie uma coluna chamada *Descrição*. Defina o **tipo de dados** como "nvarchar" e 50 para o comprimento e defina **permitir nulos** como falso.
15. Crie uma coluna denominada *Price*. Defina o **tipo de dados como "Money"** e defina **permitir nulos** como false.
16. Na caixa na parte superior, nomeie a tabela &quot;produto&quot;.

    Quando terminar, a definição terá a seguinte aparência:

    ![[imagem]](5-working-with-data/_static/image2.png)
17. Pressione Ctrl + S para salvar a tabela.

## <a name="adding-data-to-the-database"></a>Adicionando dados ao banco de dado

Agora você pode adicionar alguns dados de exemplo ao banco de dado com o qual você trabalhará posteriormente neste artigo.

1. No painel esquerdo, expanda o nó **SmallBakery. sdf** e clique em **tabelas**.
2. Clique com o botão direito do mouse na tabela Product e clique em **Data**.
3. No painel Editar, insira os seguintes registros:

    | **Nome** | **Descrição** | **Preço** |
    | --- | --- | --- |
    | Pão | Inclusas atualizado todos os dias. | 2,99 |
    | Shortcake de morango | Feito com morangos orgânica de nosso jardim. | 9.99 |
    | Pizza da Apple | Segundo somente na pizza da sua mãe. | 12.99 |
    | Pizza pecan | Se você gosta de pecans, isso é para você. | 10.99 |
    | Pizza de casca de limão | Feito com as melhores Limãos do mundo. | 11.99 |
    | Cupcakes | Seus filhos e o infantil em vocês vão adorar isso. | 7.99 |

    Lembre-se de que você não precisa inserir nada para a coluna de *ID* . Quando você criou a coluna *ID* , você define sua propriedade **Identity** como true, o que faz com que ela seja preenchida automaticamente.

    Quando você terminar de inserir os dados, o designer de tabela terá a seguinte aparência:

    ![[imagem]](5-working-with-data/_static/image3.jpg)
4. Feche a guia que contém os dados do banco de dados.

## <a name="displaying-data-from-a-database"></a>Exibindo dados de um banco

Uma vez que você tenha um banco de dados com um, você pode exibir os dados em uma página da Web do ASP.NET. Para selecionar as linhas da tabela a serem exibidas, use uma instrução SQL, que é um comando que você passa para o banco de dados.

1. No painel esquerdo, clique no espaço de trabalho **arquivos** .
2. Na raiz do site, crie uma nova página CSHTML chamada *ListProducts. cshtml*.
3. Substitua a marcação existente pelo seguinte:

    [!code-cshtml[Main](5-working-with-data/samples/sample1.cshtml)]

    No primeiro bloco de código, você abre o arquivo *SmallBakery. sdf* (banco de dados) que você criou anteriormente. O método `Database.Open` pressupõe que o arquivo *. sdf* esteja na pasta de *dados do aplicativo\_* do seu site. (Observe que você não precisa especificar a &#8212; extensão *. sdf* na verdade, se você fizer isso, o método `Open` não funcionará.)

    > [!NOTE]
    > A pasta de *dados de\_de aplicativo* é uma pasta especial no ASP.NET que é usada para armazenar arquivos de dados. Para obter mais informações, consulte [conectar-se a um banco de dados](#SB_ConnectingToADatabase) mais adiante neste artigo.

    Em seguida, faça uma solicitação para consultar o banco de dados usando a seguinte instrução SQL `Select`:

    [!code-sql[Main](5-working-with-data/samples/sample2.sql)]

    Na instrução, `Product` identifica a tabela a ser consultada. O caractere `*` especifica que a consulta deve retornar todas as colunas da tabela. (Você também pode listar colunas individualmente, separadas por vírgulas, se quisesse ver apenas algumas das colunas.) A cláusula `Order By` indica como os dados devem ser classificados &#8212; nesse caso, pela coluna *Name* . Isso significa que os dados são classificados alfabeticamente com base no valor da coluna *Name* para cada linha.

    No corpo da página, a marcação cria uma tabela HTML que será usada para exibir os dados. Dentro do elemento `<tbody>`, você usa um loop `foreach` para obter individualmente cada linha de dados retornada pela consulta. Para cada linha de dados, você cria uma linha de tabela HTML (elemento`<tr>`). Em seguida, você cria células de tabela HTML (elementos`<td>`) para cada coluna. Cada vez que você passa pelo loop, a próxima linha disponível do banco de dados está na variável `row` (configurada na instrução `foreach`). Para obter uma coluna individual da linha, você pode usar `row.Name` ou `row.Description` ou qualquer que seja o nome da coluna desejada.
4. Execute a página em um navegador. (Verifique se a página está selecionada no espaço de trabalho **arquivos** antes de executá-la.) A página exibe uma lista semelhante à seguinte:

    ![[imagem]](5-working-with-data/_static/image4.jpg)

> [!TIP] 
> 
> **SQL (Structured Query Language)**
> 
> O SQL é uma linguagem que é usada na maioria dos bancos de dados relacionais para o gerenciamento em um banco de dado. Ele inclui comandos que permitem que você recupere dados e atualize-os e que permitem criar, modificar e gerenciar tabelas de banco de dado. O SQL é diferente de uma linguagem de programação (como a que você está usando no WebMatrix) porque, com o SQL, a ideia é que você informa ao banco de dados o que você deseja, e é o trabalho do banco de dado descobrir como obter ou executar a tarefa. Aqui estão exemplos de alguns comandos SQL e o que eles fazem:
> 
> `SELECT Id, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> Isso buscará as colunas *ID*, *nome*e *preço* dos registros na tabela *produto* se o valor do *preço* for maior que 10 e retornará os resultados em ordem alfabética com base nos valores da coluna *nome* . Esse comando retornará um conjunto de resultados que contém os registros que atendem aos critérios ou um conjunto vazio se nenhum registro corresponder.
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ("Croissant", "A flaky delight", 1.99)`
> 
> Isso insere um novo registro na tabela *produto* , definindo a coluna *nome* como &quot;croissant&quot;, a coluna *Descrição* para &quot;um fascinam de instável&quot;e o preço como 1,99.
> 
> `DELETE FROM Product WHERE ExpirationDate < "01/01/2008"`
> 
> Este comando exclui os registros na tabela *Product* cuja coluna data de expiração é anterior a 1º de janeiro de 2008. (Isso pressupõe que a tabela de *produtos* tenha uma coluna como essa, é claro). A data é inserida aqui no formato MM/DD/AAAA, mas deve ser inserida no formato usado para sua localidade.
> 
> Os comandos `Insert Into` e `Delete` não retornam conjuntos de resultados. Em vez disso, eles retornam um número que informa quantos registros foram afetados pelo comando.
> 
> Para algumas dessas operações (como inserção e exclusão de registros), o processo que está solicitando a operação deve ter as permissões apropriadas no banco de dados. É por isso que, para bancos de dados de produção, você geralmente precisa fornecer um nome de usuário e uma senha ao se conectar ao banco de dados.
> 
> Há dezenas de comandos SQL, mas todos seguem um padrão como esse. Você pode usar comandos SQL para criar tabelas de banco de dados, contar o número de registros em uma tabela, calcular preços e executar muitas outras operações.

## <a name="inserting-data-in-a-database"></a>Inserindo dados em um banco de dado

Esta seção mostra como criar uma página que permite aos usuários adicionar um novo produto à tabela de banco de dados do *produto* . Depois que um novo registro de produto é inserido, a página exibe a tabela atualizada usando a página *ListProducts. cshtml* criada na seção anterior.

A página inclui validação para certificar-se de que os dados inseridos pelo usuário são válidos para o banco de dados. Por exemplo, o código na página garante que um valor tenha sido inserido para todas as colunas necessárias.

1. No site, crie um novo arquivo CSHTML chamado *InsertProducts. cshtml*.
2. Substitua a marcação existente pelo seguinte:

    [!code-cshtml[Main](5-working-with-data/samples/sample3.cshtml)]

    O corpo da página contém um formulário HTML com três caixas de texto que permitem aos usuários inserir um nome, uma descrição e um preço. Quando os usuários clicam no botão **Inserir** , o código na parte superior da página abre uma conexão com o banco de dados *SmallBakery. sdf* . Em seguida, você obtém os valores que o usuário enviou usando o objeto `Request` e atribui esses valores a variáveis locais.

    Para validar que o usuário inseriu um valor para cada coluna necessária, registre cada elemento `<input>` que você deseja validar:

    [!code-csharp[Main](5-working-with-data/samples/sample4.cs)]

    O auxiliar de `Validation` verifica se há um valor em cada um dos campos que você registrou. Você pode testar se todos os campos passaram na validação verificando `Validation.IsValid()`, o que normalmente faz antes de processar as informações obtidas do usuário:

    [!code-csharp[Main](5-working-with-data/samples/sample5.cs)]

    (O operador de `&&` significa e – esse teste *se é um envio de formulário e todos os campos passaram na validação*.)

    Se todas as colunas forem validadas (nenhuma estava vazia), você vai criar uma instrução SQL para inserir os dados e, em seguida, executá-los conforme mostrado a seguir:

    [!code-csharp[Main](5-working-with-data/samples/sample6.cs)]

    Para os valores a serem inseridos, você inclui espaços reservados de parâmetro (`@0`, `@1`, `@2`).

    > [!NOTE]
    > Como uma precaução de segurança, sempre transmita valores para uma instrução SQL usando parâmetros, como você vê no exemplo anterior. Isso lhe dá a chance de validar os dados do usuário, além de ajudar a proteger contra tentativas de enviar comandos mal-intencionados para seu banco de dado (às vezes chamados de ataques de injeção de SQL).

    Para executar a consulta, você usa essa instrução, passando a ela as variáveis que contêm os valores a serem substituídos pelos espaços reservados:

    [!code-csharp[Main](5-working-with-data/samples/sample7.cs)]

    Depois que a instrução de `Insert Into` for executada, você enviará o usuário para a página que lista os produtos usando esta linha:

    [!code-javascript[Main](5-working-with-data/samples/sample8.js)]

    Se a validação não tiver sido concluída com sucesso, você ignorará a inserção. Em vez disso, você tem um auxiliar na página que pode exibir as mensagens de erro acumuladas (se houver):

    [!code-cshtml[Main](5-working-with-data/samples/sample9.cshtml)]

    Observe que o bloco style na marcação inclui uma definição de classe CSS chamada `.validation-summary-errors`. Esse é o nome da classe CSS que é usada por padrão para o elemento `<div>` que contém quaisquer erros de validação. Nesse caso, a classe CSS especifica que os erros de Resumo de validação são exibidos em vermelho e em negrito, mas você pode definir a classe de `.validation-summary-errors` para exibir qualquer formatação que desejar.

### <a name="testing-the-insert-page"></a>Testando a página de inserção

1. Exibir a página em um navegador. A página exibe um formulário semelhante ao que é mostrado na ilustração a seguir.

    ![[imagem]](5-working-with-data/_static/image5.jpg)
2. Insira valores para todas as colunas, mas certifique-se de deixar a coluna de *preço* em branco.
3. Clique em **Inserir**. A página exibe uma mensagem de erro, conforme mostrado na ilustração a seguir. (Nenhum novo registro é criado.)

    ![[imagem]](5-working-with-data/_static/image6.jpg)
4. Preencha o formulário completamente e, em seguida, clique em **Inserir**. Desta vez, a página *ListProducts. cshtml* é exibida e mostra o novo registro.

## <a name="updating-data-in-a-database"></a>Atualizando dados em um banco de dado

Depois que os dados tiverem sido inseridos em uma tabela, talvez seja necessário atualizá-los. Este procedimento mostra como criar duas páginas semelhantes àquelas que você criou para a inserção de dados anteriormente. A primeira página exibe produtos e permite que os usuários selecionem um para alterar. A segunda página permite que os usuários realmente façam as edições e as salvem.

> [!NOTE] 
> 
> **Importante** Em um site de produção, você normalmente restringe quem tem permissão para fazer alterações nos dados. Para obter informações sobre como configurar a associação e sobre maneiras de autorizar os usuários a executar tarefas no site, consulte [adicionando segurança e associação a um site páginas da Web do ASP.net](https://go.microsoft.com/fwlink/?LinkId=202904).

1. No site, crie um novo arquivo CSHTML chamado *EditProducts. cshtml*.
2. Substitua a marcação existente no arquivo pelo seguinte:

    [!code-cshtml[Main](5-working-with-data/samples/sample10.cshtml)]

    A única diferença entre essa página e a página *ListProducts. cshtml* do anterior é que a tabela HTML nessa página inclui uma coluna extra que exibe um link de **edição** . Quando você clica nesse link, ele leva você para a página *UpdateProducts. cshtml* (que você criará em seguida), em que você pode editar o registro selecionado.

    Examine o código que cria o link de **edição** :

    [!code-cshtml[Main](5-working-with-data/samples/sample11.cshtml)]

    Isso cria um elemento `<a>` HTML cujo atributo `href` é definido dinamicamente. O atributo `href` especifica a página a ser exibida quando o usuário clica no link. Ele também passa o valor `Id` da linha atual para o link. Quando a página é executada, a origem da página pode conter links como estes:

    [!code-html[Main](5-working-with-data/samples/sample12.html)]

    Observe que o atributo `href` é definido como `UpdateProducts/n`, em que *n* é um número de produto. Quando um usuário clica em um desses links, a URL resultante terá uma aparência semelhante a esta:

    `http://localhost:18816/UpdateProducts/6`

    Em outras palavras, o número do produto a ser editado será passado na URL.
3. Exibir a página em um navegador. A página exibe os dados em um formato como este:

    ![[imagem]](5-working-with-data/_static/image7.jpg)

    Em seguida, você criará a página que permite aos usuários realmente atualizar os dados. A página de atualização inclui validação para validar os dados que o usuário insere. Por exemplo, o código na página garante que um valor tenha sido inserido para todas as colunas necessárias.
4. No site, crie um novo arquivo CSHTML chamado *UpdateProducts. cshtml*.
5. Substitua a marcação existente no arquivo pelo seguinte.

    [!code-cshtml[Main](5-working-with-data/samples/sample13.cshtml)]

    O corpo da página contém um formulário HTML em que um produto é exibido e onde os usuários podem editá-lo. Para obter o produto a ser exibido, use esta instrução SQL:

    [!code-sql[Main](5-working-with-data/samples/sample14.sql)]

    Isso selecionará o produto cuja ID corresponde ao valor passado no parâmetro `@0`. (Como *ID* é a chave primária e, portanto, deve ser exclusivo, somente um registro de produto pode ser selecionado dessa maneira.) Para obter o valor de ID a ser transmitido para esta instrução de `Select`, você pode ler o valor que é passado para a página como parte da URL, usando a seguinte sintaxe:

    [!code-csharp[Main](5-working-with-data/samples/sample15.cs)]

    Para realmente buscar o registro do produto, use o método `QuerySingle`, que retornará apenas um registro:

    [!code-csharp[Main](5-working-with-data/samples/sample16.cs)]

    A única linha é retornada para a variável `row`. Você pode obter dados de cada coluna e atribuí-los a variáveis locais como esta:

    [!code-csharp[Main](5-working-with-data/samples/sample17.cs)]

    Na marcação do formulário, esses valores são exibidos automaticamente em caixas de texto individuais usando um código inserido como o seguinte:

    [!code-html[Main](5-working-with-data/samples/sample18.html)]

    Essa parte do código exibe o registro do produto a ser atualizado. Depois que o registro tiver sido exibido, o usuário poderá editar colunas individuais.

    Quando o usuário envia o formulário clicando no botão **Atualizar** , o código no bloco de `if(IsPost)` é executado. Isso obtém os valores do usuário do objeto `Request`, armazena os valores em variáveis e valida que cada coluna foi preenchida. Se a validação for aprovada, o código criará a seguinte instrução SQL Update:

    [!code-sql[Main](5-working-with-data/samples/sample19.sql)]

    Em uma instrução SQL `Update`, você especifica cada coluna a ser atualizada e o valor para o qual defini-la. Nesse código, os valores são especificados usando os espaços reservados de parâmetro `@0`, `@1`, `@2`e assim por diante. (Conforme observado anteriormente, por segurança, você deve sempre passar valores para uma instrução SQL usando parâmetros.)

    Ao chamar o método `db.Execute`, você passa as variáveis que contêm os valores na ordem que corresponde aos parâmetros na instrução SQL:

    [!code-csharp[Main](5-working-with-data/samples/sample20.cs)]

    Depois que a instrução de `Update` tiver sido executada, você chamará o seguinte método para redirecionar o usuário de volta para a página de edição:

    [!code-cshtml[Main](5-working-with-data/samples/sample21.cshtml)]

    O efeito é que o usuário vê uma listagem atualizada dos dados no banco de dados e pode editar outro produto.
6. Salve a página.
7. Execute a página *EditProducts. cshtml* (não a página de atualização) e clique em **Editar** para selecionar um produto a ser editado. A página *UpdateProducts. cshtml* é exibida, mostrando o registro que você selecionou.

    ![[imagem]](5-working-with-data/_static/image8.jpg)
8. Faça uma alteração e clique em **Atualizar**. A lista de produtos é mostrada novamente com seus dados atualizados.

## <a name="deleting-data-in-a-database"></a>Excluindo dados em um banco de dado

Esta seção mostra como permitir que os usuários excluam um produto da tabela de banco de dados do *produto* . O exemplo consiste em duas páginas. Na primeira página, os usuários selecionam um registro para excluir. O registro a ser excluído é exibido em uma segunda página que permite confirmar que eles desejam excluir o registro.

> [!NOTE] 
> 
> **Importante** Em um site de produção, você normalmente restringe quem tem permissão para fazer alterações nos dados. Para obter informações sobre como configurar a associação e sobre maneiras de autorizar o usuário a executar tarefas no site, consulte [adicionando segurança e associação a um site páginas da Web do ASP.net](https://go.microsoft.com/fwlink/?LinkId=202904).

1. No site, crie um novo arquivo CSHTML chamado *ListProductsForDelete. cshtml*.
2. Substitua a marcação existente pelo seguinte:

    [!code-cshtml[Main](5-working-with-data/samples/sample22.cshtml)]

    Esta página é semelhante à página *EditProducts. cshtml* anterior. No entanto, em vez de exibir um link de **edição** para cada produto, ele exibe um link de **exclusão** . O link **excluir** é criado usando o seguinte código inserido na marcação:

    [!code-cshtml[Main](5-working-with-data/samples/sample23.cshtml)]

    Isso cria uma URL semelhante a esta quando os usuários clicam no link:

    `http://<server>/DeleteProduct/4`

    A URL chama uma página chamada *DeleteProduct. cshtml* (que você criará em seguida) e a passa para a ID do produto a ser excluída (aqui, 4).
3. Salve o arquivo, mas deixe-o aberto.
4. Crie outro arquivo CHTML chamado *DeleteProduct. cshtml*. Substitua o conteúdo existente pelo seguinte:

    [!code-cshtml[Main](5-working-with-data/samples/sample24.cshtml)]

    Essa página é chamada por *ListProductsForDelete. cshtml* e permite que os usuários confirmem que desejam excluir um produto. Para listar o produto a ser excluído, você obtém a ID do produto a ser excluída da URL usando o seguinte código:

    [!code-csharp[Main](5-working-with-data/samples/sample25.cs)]

    Em seguida, a página solicita que o usuário clique em um botão para realmente excluir o registro. Essa é uma medida de segurança importante: quando você executa operações confidenciais em seu site, como atualizar ou excluir dados, essas operações sempre devem ser feitas usando uma operação POST, não uma operação GET. Se seu site estiver configurado para que uma operação de exclusão possa ser executada usando uma operação GET, qualquer pessoa poderá passar uma URL como `http://<server>/DeleteProduct/4` e excluir tudo o que quiser de seu banco de dados. Adicionando a confirmação e codificando a página para que a exclusão possa ser executada somente usando uma POSTAgem, você adiciona uma medida de segurança ao seu site.

    A operação de exclusão real é executada usando o código a seguir, que primeiro confirma que esta é uma operação post e que a ID não está vazia:

    [!code-csharp[Main](5-working-with-data/samples/sample26.cs)]

    O código executa uma instrução SQL que exclui o registro especificado e, em seguida, redireciona o usuário de volta para a página de listagem.
5. Execute *ListProductsForDelete. cshtml* em um navegador.

    ![[imagem]](5-working-with-data/_static/image9.jpg)
6. Clique no link **excluir** para um dos produtos. A página *DeleteProduct. cshtml* é exibida para confirmar que você deseja excluir esse registro.
7. Clique no botão **Excluir** . O registro do produto é excluído e a página é atualizada com uma listagem de produtos atualizada.

> [!TIP]
> 
> <a id="SB_ConnectingToADatabase"></a>
> ### <a name="connecting-to-a-database"></a>Conectando a um banco de dados
> 
> Você pode se conectar a um banco de dados de duas maneiras. A primeira é usar o método `Database.Open` e especificar o nome do arquivo de banco de dados (menos a extensão *. sdf* ):
> 
> `var db = Database.Open("SmallBakery");`
> 
> O método `Open` pressupõe que o. o arquivo *SDF* está na pasta de *dados do aplicativo\_* do site. Essa pasta foi projetada especificamente para conter dados. Por exemplo, ele tem as permissões apropriadas para permitir que o site Leia e grave dados e, como medida de segurança, o WebMatrix não permite o acesso a arquivos dessa pasta.
> 
> A segunda maneira é usar uma cadeia de conexão. Uma cadeia de conexão contém informações sobre como conectar a um banco de dados. Isso pode incluir um caminho de arquivo ou pode incluir o nome de um banco de dados SQL Server em um servidor local ou remoto, juntamente com um nome de usuário e senha para se conectar a esse servidor. (Se você mantiver dados em uma versão gerenciada centralmente do SQL Server, como no site de um provedor de hospedagem, você sempre usa uma cadeia de conexão para especificar as informações de conexão de banco de dados.)
> 
> No WebMatrix, cadeias de conexão geralmente são armazenadas em um arquivo XML chamado *Web. config*. Como o nome indica, você pode usar um arquivo *Web. config* na raiz do seu site para armazenar as informações de configuração do site, incluindo quaisquer cadeias de conexão que seu site possa exigir. Um exemplo de uma cadeia de conexão em um arquivo *Web. config* pode ser semelhante ao seguinte:
> 
> [!code-xml[Main](5-working-with-data/samples/sample27.xml)]
> 
> No exemplo, a cadeia de conexão aponta para um banco de dados em uma instância do SQL Server que está sendo executado em um servidor em algum lugar (em oposição a um arquivo *. sdf* local). Você precisa substituir os nomes apropriados para `myServer` e `myDatabase`e especificar SQL Server valores de logon para `username` e `password`. (Os valores de nome de usuário e senha não são necessariamente os mesmos que as suas credenciais do Windows ou como os valores que seu provedor de hospedagem forneceu para fazer logon em seus servidores. Verifique com o administrador os valores exatos de que você precisa.)
> 
> O método `Database.Open` é flexível, pois permite que você passe o nome de um arquivo *. sdf* de banco de dados ou o nome de uma cadeia de conexão que é armazenada no arquivo *Web. config* . O exemplo a seguir mostra como se conectar ao banco de dados usando a cadeia de conexão ilustrada no exemplo anterior:
> 
> [!code-cshtml[Main](5-working-with-data/samples/sample28.cshtml)]
> 
> Como observado, o método `Database.Open` permite que você passe um nome de banco de dados ou uma cadeia de conexão, e ele descobrirá qual deles usar. Isso é muito útil quando você implanta (publica) seu site. Você pode usar um arquivo *. sdf* na pasta de *dados do\_de aplicativos* quando estiver desenvolvendo e testando seu site. Em seguida, ao mover seu site para um servidor de produção, você pode usar uma cadeia de conexão no arquivo *Web. config* que tem o mesmo nome que o arquivo *. sdf* , mas que aponta para o banco &#8212; de dados do provedor de hospedagem sem precisar alterar seu código.
> 
> Por fim, se você quiser trabalhar diretamente com uma cadeia de conexão, poderá chamar o método `Database.OpenConnectionString` e passá-lo para a cadeia de conexão real, em vez de apenas o nome de um no arquivo *Web. config* . Isso pode ser útil em situações em que, por algum motivo, você não tem acesso à cadeia de conexão (ou valores, como o nome do arquivo *. sdf* ) até que a página esteja em execução. No entanto, para a maioria dos cenários, você pode usar `Database.Open` conforme descrito neste artigo.

## <a name="additional-resources"></a>Recursos adicionais

- [SQL Server Compact](https://www.microsoft.com/sqlserver/2008/en/us/compact.aspx)
- [Conectando-se a um banco de dados SQL Server ou MySQL no WebMatrix](https://go.microsoft.com/fwlink/?LinkId=208661)
- [Validação da entrada do usuário em sites de Páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=253002)
