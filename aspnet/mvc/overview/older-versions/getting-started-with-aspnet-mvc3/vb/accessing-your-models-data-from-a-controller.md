---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/accessing-your-models-data-from-a-controller
title: Acessando os dados do modelo de um controlador (VB) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensinará as noções básicas da criação de um aplicativo Web ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: cad00de1-3c68-4ff4-a436-54236d449459
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 37f45d8f12e3ab5c485718bcf2c59934ad272118
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78615251"
---
# <a name="accessing-your-models-data-from-a-controller-vb"></a>Acessar dados de seu modelo por meio de um controlador (VB)

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> Este tutorial ensinará as noções básicas da criação de um aplicativo Web ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é uma versão gratuita do Microsoft Visual Studio. Antes de começar, verifique se você instalou os pré-requisitos listados abaixo. Você pode instalar todos eles clicando no seguinte link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, você pode instalar os pré-requisitos individualmente usando os seguintes links:
> 
> - [Pré-requisitos do Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Atualização de ferramentas do ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(suporte + ferramentas de tempo de execução)
> 
> Se você estiver usando o Visual Studio 2010 em vez do Visual Web Developer 2010, instale os pré-requisitos clicando no seguinte link: [pré-requisitos do Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Um projeto do Visual Web Developer com código-fonte VB.NET está disponível para acompanhar este tópico. [Baixe a versão do VB.net](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se preferir C#, alterne para a [ C# versão](../cs/accessing-your-models-data-from-a-controller.md) deste tutorial.

Nesta seção, você criará uma nova classe de `MoviesController` e escreverá o código que recupera os dados do filme e os exibe no navegador usando um modelo de exibição. Certifique-se de compilar seu aplicativo antes de continuar.

Clique com o botão direito do mouse na pasta *controladores* e crie um novo controlador de `MoviesController`. Selecione as seguintes opções:

- Nome do controlador: **MoviesController**. (Esse é o padrão.)
- Modelo: **controlador com ações de leitura/gravação e exibições, usando Entity Framework**.
- Classe de modelo: **filme (MvcMovie. Models)** .
- Classe de contexto de dados: **MovieDBContext (MvcMovie. Models)** .
- Exibições: **Razor (cshtml)** . (O padrão.)

[![5addMovieController](accessing-your-models-data-from-a-controller/_static/image2.png)](accessing-your-models-data-from-a-controller/_static/image1.png)

Clique em **Adicionar**. O Visual Web Developer cria os seguintes arquivos e pastas:

- *Um arquivo MoviesController. vb* na pasta *controladores* do projeto.
- Uma pasta de *filmes* na pasta *views* do projeto.
- *Crie. vbhtml, Delete. vbhtml, details. vbhtml, Edit. vbhtml*e *index. vbhtml* na nova pasta *Views\Movies*

[![5_ScaffoldMovie](accessing-your-models-data-from-a-controller/_static/image4.png)](accessing-your-models-data-from-a-controller/_static/image3.png)

O mecanismo ASP.NET MVC 3 scaffolding criou automaticamente os métodos de ação CRUD (criar, ler, atualizar e excluir) para você. Agora você tem um aplicativo Web totalmente funcional que permite criar, listar, editar e excluir entradas de filme.

Execute o aplicativo e navegue até o controlador de `Movies` anexando */Movies* à URL na barra de endereços do seu navegador. Como o aplicativo está contando com o roteamento padrão (definido no arquivo *global. asax* ), a solicitação do navegador `http://localhost:xxxxx/Movies` é roteada para o método de ação `Index` padrão do controlador de `Movies`. Em outras palavras, a solicitação do navegador `http://localhost:xxxxx/Movies` é efetivamente a mesma que a solicitação do navegador `http://localhost:xxxxx/Movies/Index`. O resultado é uma lista vazia de filmes, pois você ainda não adicionou nenhum.

![](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="creating-a-movie"></a>Criando um filme

Selecione o link **Criar Novo**. Insira alguns detalhes sobre um filme e, em seguida, clique no botão **criar** .

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Clicar no botão **criar** faz com que o formulário seja Postado no servidor, onde as informações do filme são salvas no banco de dados. Em seguida, você será redirecionado para a URL */Movies* , onde poderá ver o filme recém-criado na lista.

[![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image8.png)](accessing-your-models-data-from-a-controller/_static/image7.png)

Crie duas mais entradas de filme adicionais. Experimente os links **Editar**, **Detalhes** e **Excluir**, que estão todos funcionais.

## <a name="examining-the-generated-code"></a>Examinando o código gerado

Abra o arquivo *Controllers\MoviesController.vb* e examine o método `Index` gerado. Uma parte do controlador de filme com o método `Index` é mostrada abaixo.

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample1.vb)]

A linha a seguir da classe `MoviesController` instancia um contexto de banco de dados de filme, conforme descrito anteriormente. Você pode usar o contexto de banco de dados de filme para consultar, editar e excluir filmes.

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample2.vb)]

Uma solicitação para o controlador de `Movies` retorna todas as entradas na tabela `Movies` do banco de dados de filmes e, em seguida, passa os resultados para a exibição `Index`.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Modelos fortemente tipados e a palavra-chave @model

Anteriormente neste tutorial, você viu como um controlador pode passar dados ou objetos para um modelo de exibição usando o objeto `ViewBag`. O `ViewBag` é um objeto dinâmico que fornece uma maneira conveniente de ligação tardia para passar informações para uma exibição.

O ASP.NET MVC também fornece a capacidade de passar objetos ou dados com rigidez de tipos para um modelo de exibição. Essa abordagem fortemente tipada permite uma melhor verificação de tempo de compilação do seu código e do IntelliSense mais rico no editor do Visual Web Developer. Estamos usando essa abordagem com a classe `MoviesController` e o modelo de exibição *index. vbhtml* .

Observe como o código cria um objeto [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) ao chamar o método auxiliar `View` no método de ação `Index`. Em seguida, o código passa essa lista de `Movies` do controlador para a exibição:

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample3.vb)]

Ao incluir uma instrução `@ModelType` na parte superior do arquivo de modelo de exibição, você pode especificar o tipo de objeto esperado pela exibição. Quando você criou o controlador de filme, o Visual Web Developer inclui automaticamente a seguinte instrução de `@model` na parte superior do arquivo *index. vbhtml* :

[!code-vbhtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.vbhtml)]

Essa diretiva de `@ModelType` permite que você acesse a lista de filmes que o controlador passou para a exibição usando um objeto `Model` que é fortemente tipado. Por exemplo, no modelo *index. vbhtml* , o código percorre os filmes fazendo uma instrução `foreach` sobre o objeto `Model` fortemente tipado:

[!code-vbhtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.vbhtml)]

Como o objeto `Model` é fortemente tipado (como um objeto `IEnumerable<Movie>`), cada objeto `item` no loop é digitado como `Movie`. Entre outros benefícios, isso significa que você obtém a verificação em tempo de compilação do código e o suporte total ao IntelliSense no editor de códigos:

[![5_Intellisense](accessing-your-models-data-from-a-controller/_static/image10.png)](accessing-your-models-data-from-a-controller/_static/image9.png)

## <a name="working-with-sql-server-compact"></a>Trabalhando com SQL Server Compact

Entity Framework Code First detectou que a cadeia de conexão do banco de dados fornecida apontava para um banco de dados `Movies` que ainda não existia, portanto, Code First criou o banco de dados automaticamente. Você pode verificar se ele foi criado examinando a pasta de *dados de\_do aplicativo* . Se você não vir o arquivo *Movies. sdf* , clique no botão **Mostrar todos os arquivos** na barra de ferramentas **Gerenciador de soluções** , clique no botão **Atualizar** e expanda a pasta *\_dados do aplicativo* .

[![SDF_in_SolnExp](accessing-your-models-data-from-a-controller/_static/image12.png)](accessing-your-models-data-from-a-controller/_static/image11.png)

Clique duas vezes em *Movies. sdf* para abrir **Gerenciador de servidores**. Em seguida, expanda a pasta **tabelas** para ver as tabelas que foram criadas no banco de dados.

> [!NOTE]
> Se você receber um erro ao clicar duas vezes em *Movies. sdf*, verifique se instalou as **Ferramentas do Visual Studio 2010 SP1 para SQL Server Compact 4,0**. (Para obter links para o software, consulte a lista de pré-requisitos na parte 1 desta série de tutoriais.) Se você instalar a versão agora, precisará fechar e abrir novamente o Visual Web Developer.

[![DB_explorer](accessing-your-models-data-from-a-controller/_static/image14.png)](accessing-your-models-data-from-a-controller/_static/image13.png)

Há duas tabelas, uma para a entidade `Movie` definida e, em seguida, a tabela `EdmMetadata`. A tabela `EdmMetadata` é usada pelo Entity Framework para determinar quando o modelo e o banco de dados estão fora de sincronia.

Clique com o botão direito do mouse na tabela `Movies` e selecione **Mostrar dados da tabela** para ver os dados que você criou.

[![Movietable](accessing-your-models-data-from-a-controller/_static/image16.png)](accessing-your-models-data-from-a-controller/_static/image15.png)

Clique com o botão direito do mouse na tabela `Movies` e selecione **Editar esquema de tabela**.

[![EditTableSchema](accessing-your-models-data-from-a-controller/_static/image18.png)](accessing-your-models-data-from-a-controller/_static/image17.png)

![TableSchemaSM](accessing-your-models-data-from-a-controller/_static/image19.png)

Observe como o esquema da tabela de `Movies` é mapeado para a classe `Movie` que você criou anteriormente. Entity Framework Code First criado automaticamente esse esquema para você com base em sua classe de `Movie`.

Quando tiver terminado, feche a conexão. (Se você não fechar a conexão, poderá receber um erro na próxima vez que executar o projeto).

[![CloseConnection](accessing-your-models-data-from-a-controller/_static/image21.png)](accessing-your-models-data-from-a-controller/_static/image20.png)

Agora você tem o banco de dados e uma página de listagem simples para exibir o conteúdo dele. No próximo tutorial, examinaremos o restante do código com Scaffold e adicionaremos um método `SearchIndex` e uma exibição `SearchIndex` que permite pesquisar filmes nesse banco de dados.

> [!div class="step-by-step"]
> [Anterior](adding-a-model.md)
> [Próximo](examining-the-edit-methods-and-edit-view.md)
