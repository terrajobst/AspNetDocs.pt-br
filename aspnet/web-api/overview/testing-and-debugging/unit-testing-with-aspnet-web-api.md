---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: Teste de unidade ASP.NET Web API 2 | Microsoft Docs
author: Rick-Anderson
description: Este guia e aplicativo demonstram como criar testes de unidade simples para seu aplicativo Web API 2. Este tutorial mostra como incluir um teste de unidade proj...
ms.author: riande
ms.date: 06/05/2014
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: f2d60b977475e048a3a74aabff4adc768ee22baf
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554967"
---
# <a name="unit-testing-aspnet-web-api-2"></a>Teste de unidade ASP.NET Web API 2

por [Tom FitzMacken](https://github.com/tfitzmac)

[Baixar projeto concluído](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> Este guia e aplicativo demonstram como criar testes de unidade simples para seu aplicativo Web API 2. Este tutorial mostra como incluir um projeto de teste de unidade em sua solução e escrever métodos de teste que verificam os valores retornados de um método de controlador.
>
> Este tutorial pressupõe que você esteja familiarizado com os conceitos básicos do ASP.NET Web API. Para obter um tutorial introdutório, consulte [introdução com ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).
>
> Os testes de unidade neste tópico são intencionalmente limitados a cenários de dados simples. Para testes de unidade de cenários de dados mais avançados, consulte [simulando Entity Framework quando o teste de unidade ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).
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
    - [Adicionar projeto de teste de unidade ao criar o aplicativo](#whencreate)
    - [Adicionar projeto de teste de unidade a um aplicativo existente](#addtoexisting)
- [Configurar o aplicativo da API Web 2](#setupproject)
- [Instalar pacotes NuGet no projeto de teste](#testpackages)
- [Criar testes](#tests)
- [Executar testes](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a>Prerequisites

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional ou Enterprise Edition

<a id="download"></a>
## <a name="download-code"></a>Código de download

Baixe o [projeto concluído](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11). O projeto que pode ser baixado inclui o código de teste de unidade para este tópico e para a [simulação de Entity Framework quando o teste de unidade ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) tópico.

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>Criar aplicativo com projeto de teste de unidade

Você pode criar um projeto de teste de unidade ao criar seu aplicativo ou adicionar um projeto de teste de unidade a um aplicativo existente. Este tutorial mostra os dois métodos para criar um projeto de teste de unidade. Para seguir este tutorial, você pode usar qualquer uma das abordagens.

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a>Adicionar projeto de teste de unidade ao criar o aplicativo

Crie um novo aplicativo Web ASP.NET chamado **StoreApp**.

![criar projeto](unit-testing-with-aspnet-web-api/_static/image1.png)

Nas janelas do novo projeto ASP.NET, selecione o modelo **vazio** e adicione as pastas e as referências principais para a API da Web. Selecione a opção **Adicionar testes de unidade** . O projeto de teste de unidade é chamado automaticamente de **StoreApp. Tests**. Você pode manter esse nome.

![Criar projeto de teste de unidade](unit-testing-with-aspnet-web-api/_static/image2.png)

Depois de criar o aplicativo, você verá que ele contém dois projetos.

![dois projetos](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a>Adicionar projeto de teste de unidade a um aplicativo existente

Se você não criou o projeto de teste de unidade quando criou seu aplicativo, você pode adicionar um a qualquer momento. Por exemplo, suponha que você já tenha um aplicativo chamado StoreApp e queira adicionar testes de unidade. Para adicionar um projeto de teste de unidade, clique com o botão direito do mouse em sua solução e selecione **Adicionar** e **novo projeto**.

![Adicionar novo projeto à solução](unit-testing-with-aspnet-web-api/_static/image4.png)

Selecione **teste** no painel esquerdo e selecione projeto de **teste de unidade** para o tipo de projeto. Nomeie o projeto **StoreApp. Tests**.

![Adicionar projeto de teste de unidade](unit-testing-with-aspnet-web-api/_static/image5.png)

Você verá o projeto de teste de unidade em sua solução.

No projeto de teste de unidade, adicione uma referência de projeto ao projeto original.

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a>Configurar o aplicativo da API Web 2

Em seu projeto StoreApp, adicione um arquivo de classe à pasta **modelos** chamada **Product.cs**. Substitua o conteúdo do arquivo pelo código a seguir.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

Compile a solução.

Clique com o botão direito do mouse na pasta controladores e selecione **Adicionar** e **novo item com Scaffold**. Selecione **controlador da API Web 2-vazio**.

![Adicionar novo controlador](unit-testing-with-aspnet-web-api/_static/image6.png)

Defina o nome do controlador como **SimpleProductController**e clique em **Adicionar**.

![especificar controlador](unit-testing-with-aspnet-web-api/_static/image7.png)

Substitua o código existente pelo código a seguir. Para simplificar esse exemplo, os dados são armazenados em uma lista em vez de em um banco de dados. A lista definida nessa classe representa os dados de produção. Observe que o controlador inclui um construtor que usa como parâmetro uma lista de objetos Product. Esse construtor permite que você passe dados de teste quando o teste de unidade. O controlador também inclui dois métodos **assíncronos** para ilustrar os testes de unidade de métodos assíncronos. Esses métodos assíncronos foram implementados chamando **Task. FromResult** para minimizar códigos incorretos, mas normalmente os métodos incluem operações com uso intensivo de recursos.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

O método getproduct retorna uma instância da interface **IHttpActionResult** . O IHttpActionResult é um dos novos recursos da API Web 2 e simplifica o desenvolvimento de testes de unidade. As classes que implementam a interface IHttpActionResult são encontradas no namespace [System. Web. http. Results](https://msdn.microsoft.com/library/system.web.http.results.aspx) . Essas classes representam possíveis respostas de uma solicitação de ação e correspondem aos códigos de status HTTP.

Compile a solução.

Agora você está pronto para configurar o projeto de teste.

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>Instalar pacotes NuGet no projeto de teste

Quando você usa o modelo vazio para criar um aplicativo, o projeto de teste de unidade (StoreApp. Tests) não inclui nenhum pacote NuGet instalado. Outros modelos, como o modelo de API Web, incluem alguns pacotes NuGet no projeto de teste de unidade. Para este tutorial, você deve incluir o pacote do Microsoft ASP.NET Web API 2 Core no projeto de teste.

Clique com o botão direito do mouse no projeto StoreApp. Tests e selecione **gerenciar pacotes NuGet**. Você deve selecionar o projeto StoreApp. Tests para adicionar os pacotes a esse projeto.

![gerenciar pacotes](unit-testing-with-aspnet-web-api/_static/image8.png)

Localize e instale Microsoft ASP.NET pacote de núcleo da API Web 2.

![instalar o pacote de núcleo da API Web](unit-testing-with-aspnet-web-api/_static/image9.png)

Feche a janela gerenciar pacotes NuGet.

<a id="tests"></a>
## <a name="create-tests"></a>Criar testes

Por padrão, o projeto de teste inclui um arquivo de teste vazio chamado UnitTest1.cs. Esse arquivo mostra os atributos que você usa para criar métodos de teste. Para os testes de unidade, você pode usar esse arquivo ou criar seu próprio arquivo.

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

Para este tutorial, você criará sua própria classe de teste. Você pode excluir o arquivo UnitTest1.cs. Adicione uma classe chamada **TestSimpleProductController.cs**e substitua o código pelo código a seguir.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>Executar testes

Agora você está pronto para executar os testes. Todos os métodos marcados com o atributo **TestMethod** serão testados. No item de menu de **teste** , execute os testes.

![executar testes](unit-testing-with-aspnet-web-api/_static/image11.png)

Abra a janela **Gerenciador de testes** e observe os resultados dos testes.

![resultados de testes](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a>Resumo

Você concluiu este tutorial. Os dados neste tutorial foram intencionalmente simplificados para se concentrar em condições de teste de unidade. Para testes de unidade de cenários de dados mais avançados, consulte [simulando Entity Framework quando o teste de unidade ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).
