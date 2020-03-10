---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: Criando páginas de ajuda para o ASP.NET Web API-ASP.NET 4. x
author: MikeWasson
description: Este tutorial com código mostra como criar páginas de ajuda para ASP.NET Web API no ASP.NET 4. x.
ms.author: riande
ms.date: 04/01/2013
ms.custom: seoapril2019
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 8308dab8bd66aa8f5a3c5fb4133fc7a3df78f671
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556871"
---
# <a name="creating-help-pages-for-aspnet-web-api"></a>Criando páginas de ajuda para ASP.NET Web API

por [Mike Wasson](https://github.com/MikeWasson)

Este tutorial com código mostra como criar páginas de ajuda para ASP.NET Web API no ASP.NET 4. x.

Quando você cria uma API da Web, geralmente é útil criar uma página de ajuda para que outros desenvolvedores saibam como chamar sua API. Você pode criar toda a documentação manualmente, mas é melhor gerar o máximo possível. Para facilitar essa tarefa, ASP.NET Web API fornece uma biblioteca para gerar automaticamente páginas de ajuda em tempo de execução.

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a>Criando páginas de ajuda da API

Instale a [atualização do ASP.NET and Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650). Essa atualização integra as páginas de ajuda no modelo de projeto de API Web.

Em seguida, crie um novo projeto do ASP.NET MVC 4 e selecione o modelo de projeto da API Web. O modelo de projeto cria um controlador de API de exemplo chamado `ValuesController`. O modelo também cria as páginas de ajuda da API. Todos os arquivos de código da página de ajuda são colocados na pasta áreas do projeto.

![](creating-api-help-pages/_static/image2.png)

Quando você executa o aplicativo, o home page contém um link para a página de ajuda da API. No home page, o caminho relativo é/help.

![](creating-api-help-pages/_static/image3.png)

Esse link leva você a uma página de resumo da API.

![](creating-api-help-pages/_static/image4.png)

A exibição MVC desta página é definida em areas/HelpPage/views/help/index. cshtml. Você pode editar essa página para modificar o layout, a introdução, o título, os estilos e assim por diante.

A parte principal da página é uma tabela de APIs, agrupada por controlador. As entradas de tabela são geradas dinamicamente, usando a interface **IApiExplorer** . (Falarei mais sobre essa interface mais tarde.) Se você adicionar um novo controlador de API, a tabela será atualizada automaticamente em tempo de execução.

A coluna "API" lista o método HTTP e o URI relativo. A coluna "Descrição" contém a documentação para cada API. Inicialmente, a documentação é apenas texto de espaço reservado. Na próxima seção, mostrarei como adicionar documentação de comentários XML.

Cada API tem um link para uma página com informações mais detalhadas, incluindo corpos de solicitação e resposta de exemplo.

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a>Adicionando páginas de ajuda a um projeto existente

Você pode adicionar páginas de ajuda a um projeto de API Web existente usando o Gerenciador de pacotes NuGet. Essa opção é útil que você inicia em um modelo de projeto diferente do modelo "API Web".

No menu **ferramentas** , selecione **Gerenciador de pacotes NuGet**e, em seguida, selecione **console do Gerenciador de pacotes**. Na janela do [console do Gerenciador de pacotes](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) , digite um dos seguintes comandos:

Para um **C#** aplicativo: `Install-Package Microsoft.AspNet.WebApi.HelpPage`

Para um aplicativo **Visual Basic** : `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`

Há dois pacotes, um para C# e um para Visual Basic. Certifique-se de usar aquela que corresponda ao seu projeto.

Esse comando instala os assemblies necessários e adiciona as exibições do MVC para as páginas de ajuda (localizadas na pasta areas/HelpPage). Você precisará adicionar manualmente um link à página de ajuda. O URI é/help. Para criar um link em uma exibição do Razor, adicione o seguinte:

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

Além disso, certifique-se de registrar áreas. No arquivo global. asax, adicione o seguinte código ao **aplicativo\_** método de início, se ainda não estiver lá:

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a>Adicionando documentação da API

Por padrão, as páginas de ajuda têm cadeias de caracteres de espaço reservado para documentação. Você pode usar [comentários de documentação XML](https://msdn.microsoft.com/library/b2s063f7.aspx) para criar a documentação. Para habilitar esse recurso, abra o arquivo areas/HelpPage/app\_Start/HelpPageConfig. cs e remova a marca de comentário da seguinte linha:

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

Agora, habilite a documentação XML. Em Gerenciador de Soluções, clique com o botão direito do mouse no projeto e selecione **Propriedades**. Selecione a página de **compilação** .

![](creating-api-help-pages/_static/image6.png)

Em **saída**, verifique o **arquivo de documentação XML**. Na caixa de edição, digite "app\_data/XmlDocument. xml".

![](creating-api-help-pages/_static/image7.png)

Em seguida, abra o código para o controlador de API `ValuesController`, que é definido em/Controllers/ValuesController.cs. Adicione alguns comentários de documentação aos métodos do controlador. Por exemplo:

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> Dica: se você posicionar o cursor na linha acima do método e digitar três barras invertidas, o Visual Studio inserirá automaticamente os elementos XML. Em seguida, você pode preencher os espaços em branco.

Agora, compile e execute o aplicativo novamente e navegue até as páginas de ajuda. As cadeias de caracteres de documentação devem aparecer na tabela de API.

![](creating-api-help-pages/_static/image8.png)

A página de ajuda lê as cadeias de caracteres do arquivo XML em tempo de execução. (Quando você implantar o aplicativo, certifique-se de implantar o arquivo XML.)

## <a name="under-the-hood"></a>Nos bastidores

As páginas de ajuda são criadas sobre a classe **ApiExplorer** , que faz parte da estrutura da API Web. A classe **ApiExplorer** fornece o material bruto para criar uma página de ajuda. Para cada API, **ApiExplorer** contém um **ApiDescription** que descreve a API. Para essa finalidade, uma "API" é definida como a combinação do método HTTP e do URI relativo. Por exemplo, aqui estão algumas APIs distintas:

- OBTER/api/Products
- OBTER/api/Products/{id}
- POSTAR/api/Products

Se uma ação do controlador oferecer suporte a vários métodos HTTP, o **ApiExplorer** tratará cada método como uma API distinta.

Para ocultar uma API do **ApiExplorer**, adicione o atributo **ApiExplorerSettings** à ação e defina *IgnoreApi* como true.

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

Você também pode adicionar esse atributo ao controlador para excluir o controlador inteiro.

A classe ApiExplorer Obtém as cadeias de caracteres de documentação da interface **IDocumentationProvider** . Como vimos anteriormente, a biblioteca de páginas de ajuda fornece um **IDocumentationProvider** que obtém a documentação de cadeias de caracteres de documentação XML. O código está localizado em/Areas/HelpPage/XmlDocumentationProvider.cs. Você pode obter a documentação de outra fonte escrevendo seu próprio **IDocumentationProvider**. Para conectá-lo, chame o método de extensão **Setdocumentaprovider** , definido em **HelpPageConfigurationExtensions**

O **ApiExplorer** chama automaticamente a interface **IDocumentationProvider** para obter cadeias de caracteres de documentação para cada API. Ele os armazena na propriedade de **documentação** dos objetos **ApiDescription** e **ApiParameterDescription** .

## <a name="next-steps"></a>Próximas etapas

Você não está limitado às páginas de ajuda mostradas aqui. Na verdade, o **ApiExplorer** não está limitado à criação de páginas de ajuda. A Yao huangy escreveu algumas boas postagens no blog para que você tenha pensado na caixa:

- [Adicionando um cliente de teste simples à página ASP.NET Web API ajuda](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [Fazendo ASP.NET Web API página de ajuda para trabalhar em serviços hospedados internamente](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [Geração de tempo de design da página de ajuda (ou cliente) para ASP.NET Web API](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [Personalizações de página de ajuda avançada](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
