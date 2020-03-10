---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: Acessando os dados do modelo de um controlador | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 5d882d765133d32d3acdba9ffb5d43b69119a273
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78615916"
---
# <a name="accessing-your-models-data-from-a-controller"></a>Acessar dados do seu modelo por meio de um controlador

por [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [Tutorial Note](index.md)]

Nesta seção, você criará uma nova classe de `MoviesController` e escreverá o código que recupera os dados do filme e os exibe no navegador usando um modelo de exibição.

**Compile o aplicativo** antes de prosseguir para a próxima etapa. Se você não compilar o aplicativo, obterá um erro ao adicionar um controlador.

Em Gerenciador de Soluções, clique com o botão direito do mouse na pasta *controladores* e clique em **Adicionar**e em **controlador**.

![](accessing-your-models-data-from-a-controller/_static/image1.png)

Na caixa de diálogo **Adicionar Scaffold** , clique em **controlador MVC 5 com exibições, usando Entity Framework**e, em seguida, clique em **Adicionar**.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- Selecione **filme (MvcMovie. Models)** para a classe de modelo.
- Selecione **MovieDBContext (MvcMovie. Models)** para a classe de contexto de dados.
- Para o nome do controlador, insira **MoviesController**.

  A imagem abaixo mostra a caixa de diálogo concluído.  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

Clique em **Adicionar**. (Se você receber um erro, provavelmente não criou o aplicativo antes de começar a adicionar o controlador.) O Visual Studio cria os seguintes arquivos e pastas:

- *Um arquivo MoviesController.cs* na pasta *Controllers* .
- Uma pasta *Views\Movies* .
- *Crie. cshtml, Delete. cshtml, details. cshtml, Edit. cshtml*e *index. cshtml* na nova pasta *Views\Movies*

O Visual Studio criou automaticamente os métodos de ação [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (criar, ler, atualizar e excluir) para você (a criação automática de métodos e exibições de ação CRUD é conhecida como scaffolding). Agora você tem um aplicativo Web totalmente funcional que permite criar, listar, editar e excluir entradas de filme.

Execute o aplicativo e clique no link do **filme do MVC** (ou navegue até o controlador `Movies` acrescentando */Movies* à URL na barra de endereços do seu navegador). Como o aplicativo depende do roteamento padrão (definido no arquivo de\_do *aplicativo Start\RouteConfig.cs* ), a solicitação do navegador `http://localhost:xxxxx/Movies` é roteada para o método de ação de `Index` padrão do controlador de `Movies`. Em outras palavras, a solicitação do navegador `http://localhost:xxxxx/Movies` é efetivamente a mesma que a solicitação do navegador `http://localhost:xxxxx/Movies/Index`. O resultado é uma lista vazia de filmes, pois você ainda não adicionou nenhum.

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a>Criando um filme

Selecione o link **Criar Novo**. Insira alguns detalhes sobre um filme e, em seguida, clique no botão **criar** .

![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> Talvez você não consiga inserir pontos decimais ou vírgulas no campo preço. Para dar suporte à validação do jQuery para localidades não inglesas que usam uma vírgula (&quot;,&quot;) para um ponto decimal, e os formatos de data em inglês dos EUA, você deve incluir *globalizable. js* e seu arquivo *culturas/globalizate. culturas* específico (de [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) e JavaScript para usar `Globalize.parseFloat`. Mostrarei como fazer isso no próximo tutorial. Por enquanto, insira apenas números inteiros como 10.

Clicar no botão **criar** faz com que o formulário seja Postado no servidor, onde as informações do filme são salvas no banco de dados. Em seguida, você será redirecionado para a URL */Movies* , onde poderá ver o filme recém-criado na lista.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Crie duas mais entradas de filme adicionais. Experimente os links **Editar**, **Detalhes** e **Excluir**, que estão todos funcionais.

## <a name="examining-the-generated-code"></a>Examinando o código gerado

Abra o arquivo *Controllers\MoviesController.cs* e examine o método `Index` gerado. Uma parte do controlador de filme com o método `Index` é mostrada abaixo.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

Uma solicitação para o controlador de `Movies` retorna todas as entradas na tabela `Movies` e, em seguida, passa os resultados para a exibição `Index`. A linha a seguir da classe `MoviesController` instancia um contexto de banco de dados de filme, conforme descrito anteriormente. Você pode usar o contexto de banco de dados de filme para consultar, editar e excluir filmes.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a>Modelos fortemente tipados e a palavra-chave @model

Anteriormente neste tutorial, você viu como um controlador pode passar dados ou objetos para um modelo de exibição usando o objeto `ViewBag`. O `ViewBag` é um objeto dinâmico que fornece uma maneira conveniente de ligação tardia para passar informações para uma exibição.

O MVC também fornece a capacidade de passar objetos *fortemente* tipados para um modelo de exibição. Essa abordagem fortemente tipada permite uma melhor verificação de tempo de compilação do seu código e do [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) mais rico no editor do Visual Studio. O mecanismo scaffolding no Visual Studio usava essa abordagem (isto é, passando um modelo *fortemente* tipado) com a classe `MoviesController` e os modelos de exibição ao criar os métodos e exibições.

No arquivo *Controllers\MoviesController.cs* , examine o método `Details` gerado. O método `Details` é mostrado abaixo.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

O parâmetro `id` geralmente é passado como dados de rota, por exemplo `http://localhost:1234/movies/details/1` definirá o controlador para o controlador de filme, a ação a ser `details`da e a `id` como 1. Você também pode passar a ID com uma cadeia de caracteres de consulta da seguinte maneira:

`http://localhost:1234/movies/details?id=1`

Se um `Movie` for encontrado, uma instância do modelo de `Movie` será passada para a exibição `Details`:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

Examine o conteúdo do arquivo *Views\Movies\Details.cshtml* :

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

Ao incluir uma instrução `@model` na parte superior do arquivo de modelo de exibição, você pode especificar o tipo de objeto esperado pela exibição. Quando você criou o controlador de filmes, o Visual Studio incluiu automaticamente a seguinte instrução `@model` na parte superior do arquivo *Details.cshtml*:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

Esta diretiva `@model` permite acessar o filme que o controlador passou para a exibição usando um objeto `Model` fortemente tipado. Por exemplo, no modelo *Details. cshtml* , o código passa cada campo de filme para os auxiliares de HTML `DisplayNameFor` e [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) com o objeto de `Model` fortemente tipado. Os métodos `Create` e `Edit` e modelos de exibição também passam um objeto de modelo de filme.

Examine o modelo de exibição *index. cshtml* e o método `Index` no arquivo *MoviesController.cs* . Observe como o código cria um objeto [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) ao chamar o método auxiliar `View` no método de ação `Index`. Em seguida, o código passa essa `Movies` lista do método de ação `Index` para a exibição:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

Quando você criou o controlador de filme, o Visual Studio inclui automaticamente a seguinte instrução de `@model` na parte superior do arquivo *index. cshtml* :

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

Essa diretiva de `@model` permite que você acesse a lista de filmes que o controlador passou para a exibição usando um objeto `Model` que é fortemente tipado. Por exemplo, no modelo *index. cshtml* , o código percorre os filmes fazendo uma instrução `foreach` sobre o objeto `Model` fortemente tipado:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

Como o objeto `Model` é fortemente tipado (como um objeto `IEnumerable<Movie>`), cada objeto `item` no loop é digitado como `Movie`. Entre outros benefícios, isso significa que você obtém a verificação em tempo de compilação do código e o suporte total ao IntelliSense no editor de códigos:

![ModelIntelliSense](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a>Trabalhando com o SQL Server LocalDB

Entity Framework Code First detectou que a cadeia de conexão do banco de dados fornecida apontava para um banco de dados `Movies` que ainda não existia, portanto, Code First criou o banco de dados automaticamente. Você pode verificar se ele foi criado examinando a pasta de *dados de\_do aplicativo* . Se você não vir o arquivo *Movies. MDF* , clique no botão **Mostrar todos os arquivos** na barra de ferramentas **Gerenciador de soluções** , clique no botão **Atualizar** e expanda a pasta *\_dados do aplicativo* .

![](accessing-your-models-data-from-a-controller/_static/image9.png)

Clique duas vezes em *Movies. MDF* para abrir o **Gerenciador de servidores**e, em seguida, expanda a pasta **tabelas** para ver a tabela de filmes. Observe o ícone de chave ao lado de ID. Por padrão, o EF fará com que uma propriedade nomeada ID seja a chave primária. Para obter mais informações sobre o EF e o MVC, consulte o excelente tutorial de Tom Dykstra sobre [MVC e EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")

Clique com o botão direito do mouse na tabela `Movies` e selecione **Mostrar dados da tabela** para ver os dados que você criou.

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

Clique com o botão direito do mouse na tabela `Movies` e selecione **Abrir definição de tabela** para ver a estrutura de tabela que Entity Framework Code First criada para você.

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

Observe como o esquema da tabela de `Movies` é mapeado para a classe `Movie` que você criou anteriormente. Entity Framework Code First criado automaticamente esse esquema para você com base em sua classe de `Movie`.

Quando tiver terminado, feche a conexão clicando com o botão direito do mouse em *MovieDBContext* e selecionando **fechar conexão**. (Se você não fechar a conexão, poderá receber um erro na próxima vez que executar o projeto).

![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")

Agora você tem um banco de dados e páginas para exibir, editar, atualizar e excluir dados. No próximo tutorial, examinaremos o restante do código com Scaffold e adicionaremos um método `SearchIndex` e uma exibição `SearchIndex` que permite pesquisar filmes nesse banco de dados. Para obter mais informações sobre como usar Entity Framework com o MVC, consulte [criando um modelo de dados de Entity Framework para um aplicativo MVC ASP.net](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

> [!div class="step-by-step"]
> [Anterior](creating-a-connection-string.md)
> [Próximo](examining-the-edit-methods-and-edit-view.md)
