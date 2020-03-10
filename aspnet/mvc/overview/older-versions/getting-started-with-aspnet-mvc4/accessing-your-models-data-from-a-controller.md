---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
title: Acessando os dados do modelo de um controlador | Microsoft Docs
author: Rick-Anderson
description: 'Observação: uma versão atualizada deste tutorial está disponível aqui que usa o ASP.NET MVC 5 e o Visual Studio 2013. É mais seguro, muito mais simples de seguir e demonstrar...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 61e0206d-7f32-4018-992d-0a51b48b37dc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 7c4aa34567ac4fb31d1ed874cf65986c4e779e66
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78540386"
---
# <a name="accessing-your-models-data-from-a-controller"></a>Acessar dados do seu modelo por meio de um controlador

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Uma versão atualizada deste tutorial está disponível [aqui](../../getting-started/introduction/getting-started.md) que usa o ASP.NET MVC 5 e o Visual Studio 2013. É mais seguro, muito mais simples de seguir e demonstra mais recursos.

Nesta seção, você criará uma nova classe de `MoviesController` e escreverá o código que recupera os dados do filme e os exibe no navegador usando um modelo de exibição.

**Compile o aplicativo** antes de prosseguir para a próxima etapa.

Clique com o botão direito do mouse na pasta *controladores* e crie um novo controlador de `MoviesController`. As opções a seguir não serão exibidas até você compilar seu aplicativo. Selecione as seguintes opções:

- Nome do controlador: **MoviesController**. (Esse é o padrão. )
- Modelo: **controlador MVC com ações de leitura/gravação e exibições, usando Entity Framework**.
- Classe de modelo: **filme (MvcMovie. Models)** .
- Classe de contexto de dados: **MovieDBContext (MvcMovie. Models)** .
- Exibições: **Razor (cshtml)** . (O padrão.)

![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image1.png)

Clique em **Adicionar**. Visual Studio Express cria os seguintes arquivos e pastas:

- *Um arquivo MoviesController.cs* na pasta de *controladores* do projeto.
- Uma pasta de *filmes* na pasta *views* do projeto.
- *Crie. cshtml, Delete. cshtml, details. cshtml, Edit. cshtml*e *index. cshtml* na nova pasta *Views\Movies*

O ASP.NET MVC 4 criou automaticamente os métodos de ação CRUD (criar, ler, atualizar e excluir) para você (a criação automática de métodos e exibições de ação CRUD é conhecida como scaffolding). Agora você tem um aplicativo Web totalmente funcional que permite criar, listar, editar e excluir entradas de filme.

Execute o aplicativo e navegue até o controlador de `Movies` anexando */Movies* à URL na barra de endereços do seu navegador. Como o aplicativo está contando com o roteamento padrão (definido no arquivo *global. asax* ), a solicitação do navegador `http://localhost:xxxxx/Movies` é roteada para o método de ação `Index` padrão do controlador de `Movies`. Em outras palavras, a solicitação do navegador `http://localhost:xxxxx/Movies` é efetivamente a mesma que a solicitação do navegador `http://localhost:xxxxx/Movies/Index`. O resultado é uma lista vazia de filmes, pois você ainda não adicionou nenhum.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

### <a name="creating-a-movie"></a>Criando um filme

Selecione o link **Criar Novo**. Insira alguns detalhes sobre um filme e, em seguida, clique no botão **criar** .

![](accessing-your-models-data-from-a-controller/_static/image3.png)

Clicar no botão **criar** faz com que o formulário seja Postado no servidor, onde as informações do filme são salvas no banco de dados. Em seguida, você será redirecionado para a URL */Movies* , onde poderá ver o filme recém-criado na lista.

![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")

Crie duas mais entradas de filme adicionais. Experimente os links **Editar**, **Detalhes** e **Excluir**, que estão todos funcionais.

## <a name="examining-the-generated-code"></a>Examinando o código gerado

Abra o arquivo *Controllers\MoviesController.cs* e examine o método `Index` gerado. Uma parte do controlador de filme com o método `Index` é mostrada abaixo.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

A linha a seguir da classe `MoviesController` instancia um contexto de banco de dados de filme, conforme descrito anteriormente. Você pode usar o contexto de banco de dados de filme para consultar, editar e excluir filmes.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

Uma solicitação para o controlador de `Movies` retorna todas as entradas na tabela `Movies` do banco de dados de filmes e, em seguida, passa os resultados para a exibição `Index`.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Modelos fortemente tipados e a palavra-chave @model

Anteriormente neste tutorial, você viu como um controlador pode passar dados ou objetos para um modelo de exibição usando o objeto `ViewBag`. O `ViewBag` é um objeto dinâmico que fornece uma maneira conveniente de ligação tardia para passar informações para uma exibição.

O ASP.NET MVC também fornece a capacidade de passar objetos ou dados com rigidez de tipos para um modelo de exibição. Essa abordagem fortemente tipada permite uma melhor verificação de tempo de compilação do seu código e do IntelliSense mais rico no editor do Visual Studio. O mecanismo scaffolding no Visual Studio usava essa abordagem com a classe `MoviesController` e os modelos de exibição ao criar os métodos e exibições.

No arquivo *Controllers\MoviesController.cs* , examine o método `Details` gerado. Uma parte do controlador de filme com o método `Details` é mostrada abaixo.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs?highlight=3,8)]

Se um `Movie` for encontrado, uma instância do modelo de `Movie` será passada para a exibição de detalhes. Examine o conteúdo do arquivo *Views\Movies\Details.cshtml* .

Ao incluir uma instrução `@model` na parte superior do arquivo de modelo de exibição, você pode especificar o tipo de objeto esperado pela exibição. Quando você criou o controlador de filmes, o Visual Studio incluiu automaticamente a seguinte instrução `@model` na parte superior do arquivo *Details.cshtml*:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

Esta diretiva `@model` permite acessar o filme que o controlador passou para a exibição usando um objeto `Model` fortemente tipado. Por exemplo, no modelo *Details. cshtml* , o código passa cada campo de filme para os auxiliares de HTML `DisplayNameFor` e [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) com o objeto de `Model` fortemente tipado. Os métodos Create e Edit e View templates também passam um objeto de modelo de filme.

Examine o modelo de exibição *index. cshtml* e o método `Index` no arquivo *MoviesController.cs* . Observe como o código cria um objeto [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) ao chamar o método auxiliar `View` no método de ação `Index`. Em seguida, o código passa essa lista de `Movies` do controlador para a exibição:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample5.cs?highlight=3)]

Quando você criou o controlador de filme, Visual Studio Express incluiu automaticamente a seguinte instrução `@model` na parte superior do arquivo *index. cshtml* :

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

Essa diretiva de `@model` permite que você acesse a lista de filmes que o controlador passou para a exibição usando um objeto `Model` que é fortemente tipado. Por exemplo, no modelo *index. cshtml* , o código percorre os filmes fazendo uma instrução `foreach` sobre o objeto `Model` fortemente tipado:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample7.cshtml?highlight=1,4,7,10,13,16,19-21)]

Como o objeto `Model` é fortemente tipado (como um objeto `IEnumerable<Movie>`), cada objeto `item` no loop é digitado como `Movie`. Entre outros benefícios, isso significa que você obtém a verificação em tempo de compilação do código e o suporte total ao IntelliSense no editor de códigos:

![ModelIntelliSense](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="working-with-sql-server-localdb"></a>Trabalhando com o SQL Server LocalDB

Entity Framework Code First detectou que a cadeia de conexão do banco de dados fornecida apontava para um banco de dados `Movies` que ainda não existia, portanto, Code First criou o banco de dados automaticamente. Você pode verificar se ele foi criado examinando a pasta de *dados de\_do aplicativo* . Se você não vir o arquivo *Movies. MDF* , clique no botão **Mostrar todos os arquivos** na barra de ferramentas **Gerenciador de soluções** , clique no botão **Atualizar** e expanda a pasta *\_dados do aplicativo* .

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Clique duas vezes em *Movies. MDF* para abrir o **Gerenciador de banco de dados**e, em seguida, expanda a pasta **tabelas** para ver a tabela de filmes.

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")

> [!NOTE]
> Se o Gerenciador de banco de dados não aparecer, no menu **ferramentas** , selecione **conectar ao banco**de dados e, em seguida, cancele a caixa de diálogo **escolher fonte de dado** . Isso forçará a abrir o Gerenciador de banco de dados.

> [!NOTE]
> Se você estiver usando o VWD ou o Visual Studio 2010 e obter um erro semelhante a qualquer um dos seguintes:
> 
> - O banco de dados ' C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES. MDF ' não pode ser aberto porque é a versão 706. Este servidor dá suporte à versão 655 e anterior. Não há suporte para um caminho de downgrade.
> - &quot;exceção InvalidOperation não foi tratada pelo código do usuário&quot; a SqlConnection fornecida não especifica um catálogo inicial.
> 
> Você precisa instalar o [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) e o [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0). Verifique a cadeia de conexão `MovieDBContext` especificada na página anterior.

Clique com o botão direito do mouse na tabela `Movies` e selecione **Mostrar dados da tabela** para ver os dados que você criou.

![](accessing-your-models-data-from-a-controller/_static/image8.png)

Clique com o botão direito do mouse na tabela `Movies` e selecione **Abrir definição de tabela** para ver a estrutura de tabela que Entity Framework Code First criada para você.

![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")

![](accessing-your-models-data-from-a-controller/_static/image10.png)

Observe como o esquema da tabela de `Movies` é mapeado para a classe `Movie` que você criou anteriormente. Entity Framework Code First criado automaticamente esse esquema para você com base em sua classe de `Movie`.

Quando tiver terminado, feche a conexão clicando com o botão direito do mouse em *MovieDBContext* e selecionando **fechar conexão**. (Se você não fechar a conexão, poderá receber um erro na próxima vez que executar o projeto).

![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")

Agora você tem o banco de dados e uma página de listagem simples para exibir o conteúdo dele. No próximo tutorial, examinaremos o restante do código com Scaffold e adicionaremos um método `SearchIndex` e uma exibição `SearchIndex` que permite pesquisar filmes nesse banco de dados.

> [!div class="step-by-step"]
> [Anterior](adding-a-model.md)
> [Próximo](examining-the-edit-methods-and-edit-view.md)
