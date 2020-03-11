---
uid: mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-cs
title: Noções básicas sobre modelos, exibições eC#controladores () | Microsoft Docs
author: StephenWalther
description: Confuso sobre modelos, exibições e controladores? Neste tutorial, Stephen Walther apresenta as diferentes partes de um aplicativo ASP.NET MVC.
ms.author: riande
ms.date: 08/19/2008
ms.assetid: 87313792-0a96-4caf-89fc-1457d54e5c1e
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-cs
msc.type: authoredcontent
ms.openlocfilehash: 57dc82d02d38adc2514aa2c02c6f156ed0fb88a6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78600586"
---
# <a name="understanding-models-views-and-controllers-c"></a>Noções básicas sobre modelos, exibições e controladores (C#)

por [Stephen Walther](https://github.com/StephenWalther)

> Confuso sobre modelos, exibições e controladores? Neste tutorial, Stephen Walther apresenta as diferentes partes de um aplicativo ASP.NET MVC.

Este tutorial fornece uma visão geral de alto nível dos modelos, exibições e controladores do ASP.NET MVC. Em outras palavras, ele explica o M ', V ' e o C ' no ASP.NET MVC.

Depois de ler este tutorial, você deve entender como as diferentes partes de um aplicativo ASP.NET MVC funcionam em conjunto. Você também deve entender como a arquitetura de um aplicativo MVC ASP.NET difere de um aplicativo ASP.NET Web Forms ou de aplicativo de páginas de Active Server.

## <a name="the-sample-aspnet-mvc-application"></a>O aplicativo ASP.NET MVC de exemplo

O modelo padrão do Visual Studio para criar aplicativos Web ASP.NET MVC inclui um aplicativo de exemplo extremamente simples que pode ser usado para entender as diferentes partes de um aplicativo MVC ASP.NET. Aproveitamos este aplicativo simples neste tutorial.

Você cria um novo aplicativo MVC do ASP.NET com o modelo MVC iniciando o Visual Studio 2008 e selecionando o arquivo de opção de menu, novo projeto (veja a Figura 1). Na caixa de diálogo novo projeto, selecione sua linguagem de programação favorita em tipos de projeto C#(Visual Basic ou) e selecione **aplicativo Web ASP.NET MVC** em modelos. Clique no botão OK.

[Caixa de diálogo ![novo projeto](understanding-models-views-and-controllers-cs/_static/image1.jpg)](understanding-models-views-and-controllers-cs/_static/image1.png)

**Figura 01**: caixa de diálogo novo projeto ([clique para exibir a imagem em tamanho normal](understanding-models-views-and-controllers-cs/_static/image2.png))

Quando você cria um novo aplicativo MVC ASP.NET, a caixa de diálogo **criar projeto de teste de unidade** é exibida (consulte a Figura 2). Esta caixa de diálogo permite que você crie um projeto separado em sua solução para testar seu aplicativo MVC ASP.NET. Selecione a opção **não, não crie um projeto de teste de unidade** e clique no botão **OK** .

[Caixa de diálogo ![criar teste de unidade](understanding-models-views-and-controllers-cs/_static/image2.jpg)](understanding-models-views-and-controllers-cs/_static/image3.png)

**Figura 02**: criar caixa de diálogo de teste de unidade ([clique para exibir a imagem em tamanho normal](understanding-models-views-and-controllers-cs/_static/image4.png))

Depois que o novo aplicativo MVC do ASP.NET for criado. Você verá várias pastas e arquivos na janela Gerenciador de Soluções. Em particular, você verá três pastas nomeadas modelos, exibições e controladores. Como você pode imaginar com os nomes de pasta, essas pastas contêm os arquivos para implementar modelos, exibições e controladores.

Se você expandir a pasta controladores, deverá ver um arquivo chamado AccountController.cs e um arquivo chamado HomeController.cs. Se você expandir a pasta exibições, verá três subpastas denominadas conta, página inicial e compartilhada. Se você expandir a pasta base, verá dois arquivos adicionais chamados about. aspx e index. aspx (consulte a Figura 3). Esses arquivos compõem o aplicativo de exemplo incluído no modelo MVC padrão do ASP.NET.

[![a janela de Gerenciador de Soluções](understanding-models-views-and-controllers-cs/_static/image3.jpg)](understanding-models-views-and-controllers-cs/_static/image5.png)

**Figura 03**: a janela de Gerenciador de soluções ([clique para exibir a imagem em tamanho normal](understanding-models-views-and-controllers-cs/_static/image6.png))

Você pode executar o aplicativo de exemplo selecionando a opção de menu **depurar, iniciar depuração**. Como alternativa, você pode pressionar a tecla F5.

Quando você executa um aplicativo ASP.NET pela primeira vez, a caixa de diálogo da Figura 4 é exibida, recomendando que você habilite o modo de depuração. Clique no botão OK e o aplicativo será executado.

[caixa de diálogo ![depuração não habilitada](understanding-models-views-and-controllers-cs/_static/image4.jpg)](understanding-models-views-and-controllers-cs/_static/image7.png)

**Figura 04**: caixa de diálogo depuração não habilitada ([clique para exibir a imagem em tamanho normal](understanding-models-views-and-controllers-cs/_static/image8.png))

Quando você executa um aplicativo MVC do ASP.NET, o Visual Studio inicia o aplicativo em seu navegador da Web. O aplicativo de exemplo consiste em apenas duas páginas: a página de índice e a página sobre. Quando o aplicativo é iniciado pela primeira vez, a página índice é exibida (veja a Figura 5). Você pode navegar até a página sobre clicando no link de menu na parte superior direita do aplicativo.

[![a página de índice](understanding-models-views-and-controllers-cs/_static/image10.png)](understanding-models-views-and-controllers-cs/_static/image9.png)

**Figura 05**: a página de índice ([clique para exibir a imagem em tamanho normal](understanding-models-views-and-controllers-cs/_static/image11.png))

Observe as URLs na barra de endereços do seu navegador. Por exemplo, quando você clica no link do menu sobre, a URL na barra de endereços do navegador é alterada para **/Home/about**.

Se você fechar a janela do navegador e retornar ao Visual Studio, não será possível localizar um arquivo com o caminho Home/about. Os arquivos não existem. Como é possível?

## <a name="a-url-does-not-equal-a-page"></a>Uma URL não é igual a uma página

Quando você cria um aplicativo ASP.NET Web Forms tradicional ou um aplicativo de páginas Active Server, há uma correspondência um-para-um entre uma URL e uma página. Se você solicitar uma página chamada SomePage. aspx do servidor, então, havia uma página em disco denominada SomePage. aspx. Se o arquivo SomePage. aspx não existir, você obterá um erro feio **404-página não encontrada** .

Ao criar um aplicativo MVC ASP.NET, por outro lado, não há nenhuma correspondência entre a URL digitada na barra de endereços do navegador e os arquivos que você encontrar em seu aplicativo. Em um aplicativo MVC ASP.NET, uma URL corresponde a uma ação do controlador em vez de uma página no disco.

Em um aplicativo ASP.NET ou ASP tradicional, as solicitações do navegador são mapeadas para páginas. Em um aplicativo MVC ASP.NET, por outro lado, as solicitações do navegador são mapeadas para ações do controlador. Um aplicativo ASP.NET Web Forms é centrado em conteúdo. Um aplicativo MVC ASP.NET, por outro lado, é centrado na lógica do aplicativo.

## <a name="understanding-aspnet-routing"></a>Entendendo o roteamento do ASP.NET

Uma solicitação de navegador é mapeada para uma ação do controlador por meio de um recurso da estrutura ASP.NET chamado *roteamento ASP.net*. O roteamento ASP.NET é usado pelo ASP.NET MVC Framework para *rotear* solicitações de entrada para ações do controlador.

O roteamento do ASP.NET usa uma tabela de rotas para lidar com solicitações de entrada. Essa tabela de rotas é criada quando o aplicativo Web é iniciado pela primeira vez. A tabela de rotas é configurada no arquivo global. asax. O arquivo global. asax do MVC padrão está contido na Listagem 1.

**Listagem 1-global. asax**

[!code-csharp[Main](understanding-models-views-and-controllers-cs/samples/sample1.cs)]

Quando um aplicativo ASP.NET é iniciado pela primeira vez, o aplicativo\_método Start () é chamado. Na Listagem 1, esse método chama o método RegisterRoutes () e o método RegisterRoutes () cria a tabela de rotas padrão.

A tabela de rotas padrão consiste em uma rota. Essa rota padrão interrompe todas as solicitações de entrada em três segmentos (um segmento de URL é qualquer coisa entre barras "/"). O primeiro segmento é mapeado para um nome de controlador, o segundo segmento é mapeado para um nome de ação e o segmento final é mapeado para um parâmetro passado para a ação chamada ID.

Por exemplo, considere a seguinte URL:

/Product/Details/3

Essa URL é analisada em três parâmetros como este:

Controlador = produto

Ação = detalhes

Id = 3

A rota padrão definida no arquivo global. asax inclui valores padrão para todos os três parâmetros. O controlador padrão é home, a ação padrão é index e a ID padrão é uma cadeia de caracteres vazia. Com esses padrões em mente, considere como a seguinte URL é analisada:

/Employee

Essa URL é analisada em três parâmetros como este:

Controlador = funcionário

ação = índice

Id = ��

Por fim, se você abrir um aplicativo MVC ASP.NET sem fornecer nenhuma URL (por exemplo, `http://localhost`), a URL será analisada da seguinte maneira:

controlador = página inicial

ação = índice

Id = ��

A solicitação é roteada para a ação index () na classe HomeController.

## <a name="understanding-controllers"></a>Noções básicas sobre controladores

Um controlador é responsável por controlar a maneira como um usuário interage com um aplicativo MVC. Um controlador contém a lógica de controle de fluxo para um aplicativo MVC ASP.NET. Um controlador determina qual resposta enviar de volta a um usuário quando um usuário faz uma solicitação de navegador.

Um controlador é apenas uma classe (por exemplo, uma Visual Basic ou C# classe). O aplicativo ASP.NET MVC de exemplo inclui um controlador chamado HomeController.cs localizado na pasta controladores. O conteúdo do arquivo HomeController.cs é reproduzido na Listagem 2.

**Listagem 2-HomeController.cs**

[!code-csharp[Main](understanding-models-views-and-controllers-cs/samples/sample2.cs)]

Observe que o HomeController tem dois métodos chamados index () e About (). Esses dois métodos correspondem às duas ações expostas pelo controlador. A URL/Home/Index invoca o método HomeController. Index () e a URL/Home/About invoca o método HomeController. About ().

Qualquer método público em um controlador é exposto como uma ação do controlador. Você precisa ter cuidado com isso. Isso significa que qualquer método público contido em um controlador pode ser invocado por qualquer pessoa com acesso à Internet inserindo a URL correta em um navegador.

## <a name="understanding-views"></a>Compreendendo as exibições

As duas ações do controlador expostas pela classe HomeController, index () e About (), retornam uma exibição. Uma exibição contém a marcação HTML e o conteúdo que é enviado para o navegador. Uma exibição é o equivalente de uma página ao trabalhar com um aplicativo MVC ASP.NET.

Você deve criar suas exibições no local certo. A ação HomeController. Index () retorna uma exibição localizada no seguinte caminho:

\Views\Home\Index.aspx

A ação HomeController. About () retorna uma exibição localizada no seguinte caminho:

\Views\Home\About.aspx

Em geral, se você quiser retornar um modo de exibição para uma ação do controlador, precisará criar uma subpasta na pasta views com o mesmo nome que o controlador. Na subpasta, você deve criar um arquivo. aspx com o mesmo nome que a ação do controlador.

O arquivo na Listagem 3 contém a exibição about. aspx.

**Listagem 3-about. aspx**

[!code-aspx[Main](understanding-models-views-and-controllers-cs/samples/sample3.aspx)]

Se você ignorar a primeira linha na Listagem 3, a maior parte do restante da exibição consistirá em HTML padrão. Você pode modificar o conteúdo da exibição inserindo qualquer HTML que desejar aqui.

Uma exibição é muito semelhante a uma página em Active Server páginas ou ASP.NET Web Forms. Uma exibição pode conter conteúdo e scripts HTML. Você pode escrever os scripts em sua linguagem de programação do .NET favorita (por C# exemplo, ou Visual Basic .net). Você usa scripts para exibir conteúdo dinâmico, como dados de banco de dados.

## <a name="understanding-models"></a>Entendendo modelos

Discutimos os controladores e discutimos os modos de exibição. O último tópico que precisamos discutir são os modelos. O que é um modelo MVC?

Um modelo MVC contém toda a lógica do aplicativo que não está contida em uma exibição ou controlador. O modelo deve conter toda a lógica de negócios do aplicativo, a lógica de validação e a lógica de acesso ao banco de dados. Por exemplo, se você estiver usando o Entity Framework da Microsoft para acessar seu banco de dados, você criará suas classes de Entity Framework (seu arquivo. edmx) na pasta modelos.

Uma exibição deve conter somente a lógica relacionada à geração da interface do usuário. Um controlador deve conter apenas o mínimo de lógica necessária para retornar o modo de exibição correto ou redirecionar o usuário para outra ação (controle de fluxo). Todo o resto deve estar contido no modelo.

Em geral, você deve buscar modelos de FAT e controladores skinnáveis. Os métodos do controlador devem conter apenas algumas linhas de código. Se uma ação do controlador ficar muito grande, você deve considerar mover a lógica para uma nova classe na pasta modelos.

## <a name="summary"></a>Resumo

Este tutorial forneceu uma visão geral de alto nível das diferentes partes de um aplicativo Web ASP.NET MVC. Você aprendeu como o ASP.NET Routing mapeia solicitações de navegador recebidas para ações específicas do controlador. Você aprendeu como os controladores orquestram como as exibições são retornadas ao navegador. Por fim, você aprendeu como os modelos contêm a lógica de acesso de banco de dados, validação e negócios de aplicativo.
