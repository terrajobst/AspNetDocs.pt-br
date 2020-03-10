---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
title: Adicionando um novo campo ao modelo de filme e à tabelaC#() | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensinará as noções básicas da criação de um aplicativo Web ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: b4e76c1a-f66e-43a0-aa72-f39df79c07c1
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 40b02a2f608f07091ce6b5339688a1e6290e2e37
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78540799"
---
# <a name="adding-a-new-field-to-the-movie-model-and-table-c"></a>Adicionar um novo campo ao modelo de filme e à tabela (C#)

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Uma versão atualizada deste tutorial está disponível [aqui](../../../getting-started/introduction/getting-started.md) que usa o ASP.NET MVC 5 e o Visual Studio 2013. É mais seguro, muito mais simples de seguir e demonstra mais recursos.
> 
> 
> Este tutorial ensinará as noções básicas da criação de um aplicativo Web ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é uma versão gratuita do Microsoft Visual Studio. Antes de começar, verifique se você instalou os pré-requisitos listados abaixo. Você pode instalar todos eles clicando no seguinte link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, você pode instalar os pré-requisitos individualmente usando os seguintes links:
> 
> - [Pré-requisitos do Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Atualização de ferramentas do ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(suporte + ferramentas de tempo de execução)
> 
> Se você estiver usando o Visual Studio 2010 em vez do Visual Web Developer 2010, instale os pré-requisitos clicando no seguinte link: [pré-requisitos do Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Um projeto do Visual Web Developer com C# código-fonte está disponível para acompanhar este tópico. [Baixe a C# versão](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se preferir Visual Basic, alterne para a [versão Visual Basic](../vb/intro-to-aspnet-mvc-3.md) deste tutorial.

Nesta seção, você fará algumas alterações nas classes de modelo e aprenderá como é possível atualizar o esquema de banco de dados para corresponder às alterações de modelo.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Adicionando uma propriedade de classificação ao modelo de filme

Comece adicionando uma nova propriedade `Rating` à classe `Movie` existente. Abra o arquivo *Movie.cs* e adicione a propriedade `Rating` como esta:

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

A classe completa `Movie` agora é semelhante ao seguinte código:

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

Recompile o aplicativo usando o comando **Debug** &gt;**Build Movie** menu.

Agora que você atualizou a classe `Model`, também precisa atualizar os modelos de exibição *\Views\Movies\Index.cshtml* e *\Views\Movies\Create.cshtml* para dar suporte à nova propriedade `Rating`.

Abra o arquivo *\Views\Movies\Index.cshtml* e adicione um título de coluna `<th>Rating</th>` logo após a coluna **Price** . Em seguida, adicione uma coluna `<td>` próximo ao final do modelo para renderizar o valor de `@item.Rating`. Abaixo está a aparência do modelo de exibição *index. cshtml* atualizado:

[!code-cshtml[Main](adding-a-new-field/samples/sample3.cshtml)]

Em seguida, abra o arquivo *\Views\Movies\Create.cshtml* e adicione a marcação a seguir próximo ao final do formulário. Isso renderiza uma caixa de texto para que você possa especificar uma classificação quando um novo filme é criado.

[!code-cshtml[Main](adding-a-new-field/samples/sample4.cshtml)]

## <a name="managing-model-and-database-schema-differences"></a>Gerenciando diferenças de modelo e esquema de banco de dados

Agora você atualizou o código do aplicativo para dar suporte à nova propriedade `Rating`.

Agora, execute o aplicativo e navegue até a URL */Movies* . No entanto, ao fazer isso, você verá o seguinte erro:

![](adding-a-new-field/_static/image1.png)

Você está vendo esse erro porque a classe do modelo de `Movie` atualizado no aplicativo agora é diferente do esquema da tabela `Movie` do banco de dados existente. (Não há nenhuma coluna `Rating` na tabela de banco de dados.)

Por padrão, quando você usa Entity Framework Code First para criar automaticamente um banco de dados, como fazia anteriormente neste tutorial, Code First adiciona uma tabela ao banco de dados para ajudar a controlar se o esquema do banco de dados está em sincronia com as classes de modelo das quais ele foi gerado. Se eles não estiverem em sincronia, o Entity Framework gerará um erro. Isso facilita o rastreamento de problemas em tempo de desenvolvimento que, de outra forma, você pode localizar (por erros obscuros) em tempo de execução. O recurso de verificação de sincronização é o que faz com que a mensagem de erro seja exibida que você acabou de ver.

Há duas abordagens para resolver o erro:

1. Faça com que o Entity Framework remova automaticamente e recrie o banco de dados com base no novo esquema de classe de modelo. Essa abordagem é muito conveniente ao fazer o desenvolvimento ativo em um banco de dados de teste, pois ele permite que você evolua rapidamente o modelo e o esquema de banco de dados juntos. No entanto, a desvantagem é que você perde os dados existentes no banco de dados – portanto, *não* convém usar essa abordagem em um banco de dados de produção!
2. Modifique explicitamente o esquema do banco de dados existente para que ele corresponda às classes de modelo. A vantagem dessa abordagem é que você mantém os dados. Faça essa alteração manualmente ou criando um script de alteração de banco de dados.

Para este tutorial, usaremos a primeira abordagem — você terá o Entity Framework Code First recriará o banco de dados automaticamente sempre que o modelo for alterado.

## <a name="automatically-re-creating-the-database-on-model-changes"></a>Recriar automaticamente o banco de dados em alterações de modelo

Vamos atualizar o aplicativo para que Code First automaticamente descartar e recriar o banco de dados sempre que você alterar o modelo para o aplicativo.

> [!NOTE] 
> 
> **Aviso** Você deve habilitar essa abordagem de descartar e recriar automaticamente o banco de dados somente quando estiver usando um banco de dados de desenvolvimento ou de teste, e *nunca* em um banco de dado de produção que contenha real Data. Usá-lo em um servidor de produção pode levar à perda de dados.

Em **Gerenciador de soluções**, clique com o botão direito do mouse na pasta *modelos* , selecione **Adicionar**e, em seguida, selecione **classe**.

![](adding-a-new-field/_static/image2.png)

Nomeie a classe "MovieInitializer". Atualize a classe `MovieInitializer` para conter o seguinte código:

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

A classe `MovieInitializer` especifica que o banco de dados usado pelo modelo deve ser descartado e recriado automaticamente se as classes de modelo forem alteradas. O código inclui um método de `Seed` para especificar alguns dados padrão a serem adicionados automaticamente ao Database sempre que ele for criado (ou recriado). Isso fornece uma maneira útil de preencher o banco de dados com alguns exemplos de dado, sem a necessidade de preenchê-lo manualmente sempre que você fizer uma alteração no modelo.

Agora que você definiu a classe `MovieInitializer`, você desejará conectá-la para que cada vez que o aplicativo for executado, ele verificará se as classes de modelo são diferentes do esquema no banco de dados. Em caso afirmativo, você pode executar o inicializador para recriar o banco de dados para corresponder ao modelo e, em seguida, preencher o banco de dado com os data de exemplo.

Abra o arquivo *global. asax* que está na raiz do projeto `MvcMovies`:

[![](adding-a-new-field/_static/image4.png)](adding-a-new-field/_static/image3.png)

O arquivo *global. asax* contém a classe que define o aplicativo inteiro para o projeto e contém um manipulador de eventos `Application_Start` que é executado quando o aplicativo é iniciado pela primeira vez.

Vamos adicionar duas instruções using à parte superior do arquivo. A primeira referencia o namespace Entity Framework e o segundo faz referência ao namespace em que a nossa classe `MovieInitializer` reside:

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs)]

Em seguida, localize o método `Application_Start` e adicione uma chamada para `Database.SetInitializer` no início do método, conforme mostrado abaixo:

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs)]

A instrução de `Database.SetInitializer` que você acabou de adicionar indica que o banco de dados usado pela instância de `MovieDBContext` deve ser excluído e recriado automaticamente se o esquema e o banco de dados não corresponderem. E como você viu, ele também preencherá o banco de dados com o exemplo de dado especificado na classe `MovieInitializer`.

Feche o arquivo *global. asax* .

Execute novamente o aplicativo e navegue até a URL do */Movies* . Quando o aplicativo é iniciado, ele detecta que a estrutura do modelo não corresponde mais ao esquema do banco de dados. Ele recria automaticamente o banco de dados para corresponder à nova estrutura de modelo e popula o banco de dados com os filmes de exemplo:

![7_MyMovieList_SM](adding-a-new-field/_static/image5.png)

Clique no link **criar novo** para adicionar um novo filme. Observe que você pode adicionar uma classificação.

[![7_CreateRioII](adding-a-new-field/_static/image7.png)](adding-a-new-field/_static/image6.png)

Clique em **Criar**. O novo filme, incluindo a classificação, agora aparece na listagem de filmes:

[![7_ourNewMovie_SM](adding-a-new-field/_static/image9.png)](adding-a-new-field/_static/image8.png)

Nesta seção, você viu como é possível modificar objetos de modelo e manter o banco de dados em sincronia com as alterações. Você também aprendeu uma maneira de preencher um banco de dados recém-criado com exemplos de dado, de modo que você possa experimentar cenários. Em seguida, vamos dar uma olhada em como você pode adicionar uma lógica de validação mais rica às classes de modelo e permitir que algumas regras de negócio sejam impostas.

> [!div class="step-by-step"]
> [Anterior](examining-the-edit-methods-and-edit-view.md)
> [Próximo](adding-validation-to-the-model.md)
