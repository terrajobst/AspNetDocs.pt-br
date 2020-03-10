---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: Simulação de Entity Framework quando o teste de unidade ASP.NET Web API 2 | Microsoft Docs
author: Rick-Anderson
description: Este guia e aplicativo demonstram como criar testes de unidade para seu aplicativo Web API 2 que usa o Entity Framework. Ele mostra como modificar o...
ms.author: riande
ms.date: 12/13/2013
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 258450107ee7443c4efd43a3b8e4851249745227
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555086"
---
# <a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a>Simulação de Entity Framework quando o teste de unidade ASP.NET Web API 2

por [Tom FitzMacken](https://github.com/tfitzmac)

[Baixar projeto concluído](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> Este guia e aplicativo demonstram como criar testes de unidade para seu aplicativo Web API 2 que usa o Entity Framework. Ele mostra como modificar o controlador com Scaffold para habilitar a passagem de um objeto de contexto para teste e como criar objetos de teste que funcionam com Entity Framework.
>
> Para obter uma introdução ao teste de unidade com ASP.NET Web API, consulte [testes de unidade com ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).
>
> Este tutorial pressupõe que você esteja familiarizado com os conceitos básicos do ASP.NET Web API. Para obter um tutorial introdutório, consulte [introdução com ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - API Web 2

## <a name="in-this-topic"></a>Neste tópico

Este tópico contém as seguintes seções:

- [Pré-requisitos](#prereqs)
- [Código de download](#download)
- [Criar aplicativo com projeto de teste de unidade](#appwithunittest)
- [Criar a classe de modelo](#modelclass)
- [Adicionar controlador](#controller)
- [Adicionar injeção de dependência](#dependency)
- [Instalar pacotes NuGet no projeto de teste](#testpackages)
- [Criar contexto de teste](#testcontext)
- [Criar testes](#tests)
- [Executar testes](#runtests)

Se você já tiver concluído as etapas em [testes de unidade com ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), poderá pular para a seção [Adicionar o controlador](#controller).

<a id="prereqs"></a>
## <a name="prerequisites"></a>Prerequisites

Visual Studio 2017 Community, Professional ou Enterprise Edition

<a id="download"></a>
## <a name="download-code"></a>Código de download

Baixe o [projeto concluído](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11). O projeto que pode ser baixado inclui o código de teste de unidade para este tópico e para o tópico [teste de unidade ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) .

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>Criar aplicativo com projeto de teste de unidade

Você pode criar um projeto de teste de unidade ao criar seu aplicativo ou adicionar um projeto de teste de unidade a um aplicativo existente. Este tutorial mostra como criar um projeto de teste de unidade ao criar o aplicativo.

Crie um novo aplicativo Web ASP.NET chamado **StoreApp**.

Nas janelas do novo projeto ASP.NET, selecione o modelo **vazio** e adicione as pastas e as referências principais para a API da Web. Selecione a opção **Adicionar testes de unidade** . O projeto de teste de unidade é chamado automaticamente de **StoreApp. Tests**. Você pode manter esse nome.

![Criar projeto de teste de unidade](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

Depois de criar o aplicativo, você verá que ele contém dois projetos- **StoreApp** e **StoreApp. Tests**.

<a id="modelclass"></a>
## <a name="create-the-model-class"></a>Criar a classe de modelo

Em seu projeto StoreApp, adicione um arquivo de classe à pasta **modelos** chamada **Product.cs**. Substitua o conteúdo do arquivo pelo código a seguir.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

Compile a solução.

<a id="controller"></a>
## <a name="add-the-controller"></a>Adicionar controlador

Clique com o botão direito do mouse na pasta controladores e selecione **Adicionar** e **novo item com Scaffold**. Selecione controlador Web API 2 com ações, usando Entity Framework.

![Adicionar novo controlador](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

Defina os seguintes valores:

- Nome do controlador: **ProductController**
- Classe de modelo: **produto**
- Classe de contexto de dados: [botão Selecionar **novo contexto de dados** que preenche os valores exibidos abaixo]

![especificar controlador](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

Clique em **Adicionar** para criar o controlador com o código gerado automaticamente. O código inclui métodos para criar, recuperar, atualizar e excluir instâncias da classe Product. O código a seguir mostra o método para adicionar um produto. Observe que o método retorna uma instância de **IHttpActionResult**.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

O IHttpActionResult é um dos novos recursos da API Web 2 e simplifica o desenvolvimento de testes de unidade.

Na próxima seção, você personalizará o código gerado para facilitar a passagem de objetos de teste para o controlador.

<a id="dependency"></a>
## <a name="add-dependency-injection"></a>Adicionar injeção de dependência

Atualmente, a classe ProductController é embutida em código para usar uma instância da classe StoreAppContext. Você usará um padrão chamado injeção de dependência para modificar seu aplicativo e remover essa dependência embutida em código. Ao dividir essa dependência, você pode passar um objeto fictício durante o teste.

Clique com o botão direito do mouse na pasta **modelos** e adicione uma nova interface chamada **IStoreAppContext**.

Substitua o código pelo código a seguir.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

Abra o arquivo StoreAppContext.cs e faça as seguintes alterações realçadas. As alterações importantes a serem observadas são:

- Classe StoreAppContext implementa a interface IStoreAppContext
- O método MarkAsModified é implementado

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

Abra o arquivo ProductController.cs. Altere o código existente para corresponder ao código realçado. Essas alterações quebram a dependência em StoreAppContext e permitem que outras classes passem em um objeto diferente para a classe de contexto. Essa alteração permitirá que você passe um contexto de teste durante os testes de unidade.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

Há mais uma alteração que você deve fazer em ProductController. No método **PutProduct** , substitua a linha que define o estado da entidade como modificado com uma chamada para o método MarkAsModified.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

Compile a solução.

Agora você está pronto para configurar o projeto de teste.

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>Instalar pacotes NuGet no projeto de teste

Quando você usa o modelo vazio para criar um aplicativo, o projeto de teste de unidade (StoreApp. Tests) não inclui nenhum pacote NuGet instalado. Outros modelos, como o modelo de API Web, incluem alguns pacotes NuGet no projeto de teste de unidade. Para este tutorial, você deve incluir o pacote de Entity Framework e o pacote de Microsoft ASP.NET Web API 2 core para o projeto de teste.

Clique com o botão direito do mouse no projeto StoreApp. Tests e selecione **gerenciar pacotes NuGet**. Você deve selecionar o projeto StoreApp. Tests para adicionar os pacotes a esse projeto.

![gerenciar pacotes](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

Nos pacotes online, localize e instale o pacote do EntityFramework (versão 6,0 ou posterior). Se parecer que o pacote do EntityFramework já está instalado, você pode ter selecionado o projeto StoreApp em vez do projeto StoreApp. Tests.

![Adicionar Entity Framework](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

Localize e instale Microsoft ASP.NET pacote de núcleo da API Web 2.

![instalar o pacote de núcleo da API Web](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

Feche a janela gerenciar pacotes NuGet.

<a id="testcontext"></a>
## <a name="create-test-context"></a>Criar contexto de teste

Adicione uma classe chamada **TestDbSet** ao projeto de teste. Essa classe serve como a classe base para o conjunto de dados de teste. Substitua o código pelo código a seguir.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

Adicione uma classe chamada **TestProductDbSet** ao projeto de teste que contém o código a seguir.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

Adicione uma classe chamada **TestStoreAppContext** e substitua o código existente pelo código a seguir.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a>Criar testes

Por padrão, o projeto de teste inclui um arquivo de teste vazio chamado **UnitTest1.cs**. Esse arquivo mostra os atributos que você usa para criar métodos de teste. Para este tutorial, você pode excluir este arquivo porque você adicionará uma nova classe de teste.

Adicione uma classe chamada **TestProductController** ao projeto de teste. Substitua o código pelo código a seguir.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>Executar testes

Agora você está pronto para executar os testes. Todos os métodos marcados com o atributo **TestMethod** serão testados. No item de menu de **teste** , execute os testes.

![executar testes](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

Abra a janela **Gerenciador de testes** e observe os resultados dos testes.

![resultados de testes](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
