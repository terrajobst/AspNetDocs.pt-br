---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: Criar um ponto de extremidade do OData v4 usando ASP.NET Web API 2,2 | Microsoft Docs
author: MikeWasson
description: O Protocolo Open Data (OData) é um protocolo de acesso a dados para a Web. O OData fornece uma maneira uniforme de consultar e manipular conjuntos de dados por meio de operações CRUD...
ms.author: riande
ms.date: 01/23/2019
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 81d134cbd3231b9a0d5537ccbd1bbfe6419254af
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598731"
---
# <a name="create-an-odata-v4-endpoint-using-aspnet-web-api"></a>Criar um ponto de extremidade do OData v4 usando ASP.NET Web API 

> O Protocolo Open Data (OData) é um protocolo de acesso a dados para a Web. O OData fornece uma maneira uniforme de consultar e manipular conjuntos de dados por meio de operações CRUD (criar, ler, atualizar e excluir).
>
> O ASP.NET Web API dá suporte a V3 e v4 do protocolo. Você pode até mesmo ter um ponto de extremidade v4 que é executado lado a lado com um ponto de extremidade v3.
>
> Este tutorial mostra como criar um ponto de extremidade do OData v4 que oferece suporte a operações CRUD.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
>
> - API Web 5,2
> - OData v4
> - Visual Studio 2017 (Baixe o Visual Studio 2017 [aqui](https://visualstudio.microsoft.com/downloads/))
> - Entity Framework 6
> - .NET 4.7.2
>
> ## <a name="tutorial-versions"></a>Versões do tutorial
>
> Para o OData versão 3, consulte [criando um ponto de extremidade OData v3](../odata-v3/creating-an-odata-endpoint.md).

## <a name="create-the-visual-studio-project"></a>Criar o projeto do Visual Studio

No Visual Studio, no menu **arquivo** , selecione **novo** **projeto**de &gt;.

Expanda **instalado** &gt;  **C# Visual** &gt; **Web**e selecione o modelo **aplicativo Web do ASP.net (.NET Framework)** . Nomeie o projeto &quot;ProductService&quot;.

[![](create-an-odata-v4-endpoint/_static/image7.png)](create-an-odata-v4-endpoint/_static/image7.png)

Selecione **OK**.

[![](create-an-odata-v4-endpoint/_static/image8.png)](create-an-odata-v4-endpoint/_static/image8.png)

Selecione o modelo **Vazio**. Em **Adicionar pastas e referências principais para:** , selecione **API Web**. Selecione **OK**.

## <a name="install-the-odata-packages"></a>Instalar os pacotes OData

No menu **ferramentas** , selecione **Gerenciador de pacotes NuGet** &gt; **console do Gerenciador de pacotes**. Na janela do console do Gerenciador de pacotes, digite:

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

Esse comando instala os pacotes do NuGet do OData mais recentes.

## <a name="add-a-model-class"></a>Adicionar uma classe de modelo

Um *modelo* é um objeto que representa uma entidade de dados em seu aplicativo.

No Gerenciador de Soluções, clique com o botão direito do mouse na pasta Modelos. No menu de contexto, selecione **adicionar** &gt; **classe**.

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> Por convenção, as classes de modelo são colocadas na pasta modelos, mas você não precisa seguir essa convenção em seus próprios projetos.

Nome da classe `Product`. No arquivo Product.cs, substitua o código clichê pelo seguinte:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

A propriedade `Id` é a chave de entidade. Os clientes podem consultar entidades por chave. Por exemplo, para obter o produto com a ID 5, o URI é `/Products(5)`. A propriedade `Id` também será a chave primária no banco de dados back-end.

## <a name="enable-entity-framework"></a>Habilitar Entity Framework

Para este tutorial, usaremos Entity Framework (EF) Code First para criar o banco de dados back-end.

> [!NOTE]
> O OData da API Web não requer o EF. Use qualquer camada de acesso a dados que possa converter entidades de banco de dado em modelos.

Primeiro, instale o pacote NuGet para o EF. No menu **ferramentas** , selecione **Gerenciador de pacotes NuGet** &gt; **console do Gerenciador de pacotes**. Na janela do console do Gerenciador de pacotes, digite:

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

Abra o arquivo Web. config e adicione a seção a seguir dentro do elemento de **configuração** , após o elemento **configSections** .

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

Essa configuração adiciona uma cadeia de conexão para um banco de dados LocalDB. Esse banco de dados será usado quando você executar o aplicativo localmente.

Em seguida, adicione uma classe chamada `ProductsContext` à pasta modelos:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

No construtor, `"name=ProductsContext"` fornece o nome da cadeia de conexão.

## <a name="configure-the-odata-endpoint"></a>Configurar o ponto de extremidade OData

Abra o aplicativo de arquivo\_Start/WebApiConfig. cs. Adicione as seguintes instruções **using** :

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

Em seguida, adicione o seguinte código ao método **Register** :

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

Esse código faz duas coisas:

- Cria um Modelo de Dados de Entidade (EDM).
- Adiciona uma rota.

Um EDM é um modelo abstrato dos dados. O EDM é usado para criar o documento de metadados de serviço. A classe **ODataConventionModelBuilder** cria um EDM usando as convenções de nomenclatura padrão. Essa abordagem requer o mínimo de código. Se você quiser mais controle sobre o EDM, poderá usar a classe **ODataModelBuilder** para criar o EDM adicionando Propriedades, chaves e propriedades de navegação explicitamente.

Uma *rota* informa à API da Web como rotear solicitações HTTP para o ponto de extremidade. Para criar uma rota v4 do OData, chame o método de extensão **MapODataServiceRoute** .

Se seu aplicativo tiver vários pontos de extremidade OData, crie uma rota separada para cada um. Dê a cada rota um nome e prefixo de rota exclusivos.

## <a name="add-the-odata-controller"></a>Adicionar o controlador OData

Um *controlador* é uma classe que MANIPULA solicitações HTTP. Você cria um controlador separado para cada conjunto de entidades em seu serviço OData. Neste tutorial, você criará um controlador para a entidade de `Product`.

Em Gerenciador de Soluções, clique com o botão direito do mouse na pasta controladores e selecione **adicionar** &gt; **classe**. Nome da classe `ProductsController`.

> [!NOTE]
> A versão deste tutorial para OData v3 usa o **Add Controller** scaffolding. Atualmente, não há nenhum scaffolding para o OData v4.

Substitua o código clichê em ProductsController.cs pelo seguinte.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

O controlador usa a classe `ProductsContext` para acessar o banco de dados usando o EF. Observe que o controlador substitui o método **Dispose** para descartar o **ProductsContext**.

Este é o ponto de partida para o controlador. Em seguida, adicionaremos métodos para todas as operações CRUD.

## <a name="query-the-entity-set"></a>Consultar o conjunto de entidades

Adicione os métodos a seguir para `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

A versão sem parâmetros do método `Get` retorna a coleção de produtos inteira. O método `Get` com um parâmetro de *chave* pesquisa um produto por sua chave (nesse caso, a propriedade `Id`).

O atributo **[EnableQuery]** permite que os clientes modifiquem a consulta usando opções de consulta, como $filter, $sort e $Page. Para obter mais informações, consulte [Opções de consulta do OData de suporte](../supporting-odata-query-options.md).

## <a name="add-an-entity-to-the-entity-set"></a>Adicionar uma entidade ao conjunto de entidades

Para permitir que os clientes adicionem um novo produto ao banco de dados, adicione o método a seguir para `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="update-an-entity"></a>Atualizar uma entidade

O OData dá suporte a duas semânticas diferentes para atualizar uma entidade, um PATCH e um PUT.

- O PATCH executa uma atualização parcial. O cliente especifica apenas as propriedades a serem atualizadas.
- PUT substitui a entidade inteira.

A desvantagem de PUT é que o cliente deve enviar valores para todas as propriedades na entidade, incluindo valores que não estão sendo alterados. A [especificação do OData](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) declara que o patch é preferencial.

Em qualquer caso, aqui está o código para os métodos PATCH e PUT:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

No caso do PATCH, o controlador usa o tipo de **&gt;de&lt;de Delta** para controlar as alterações.

## <a name="delete-an-entity"></a>Excluir uma entidade

Para permitir que os clientes excluam um produto do banco de dados, adicione o seguinte método a `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
