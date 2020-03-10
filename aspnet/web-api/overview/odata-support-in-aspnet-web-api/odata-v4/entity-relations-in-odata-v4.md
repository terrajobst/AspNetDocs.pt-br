---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
title: Relações de entidade no OData v4 usando o ASP.NET Web API 2,2 | Microsoft Docs
author: MikeWasson
description: 'A maioria dos conjuntos de dados define as relações entre as entidades: os clientes têm pedidos; os livros têm autores; os produtos têm fornecedores. Usando o OData, os clientes podem navegar...'
ms.author: riande
ms.date: 06/26/2014
ms.assetid: 72657550-ec09-4779-9bfc-2fb15ecd51c7
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: fbafb2b2346689271905db5790cdddeeb809b070
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598689"
---
# <a name="entity-relations-in-odata-v4-using-aspnet-web-api-22"></a>Relações de entidade no OData v4 usando o ASP.NET Web API 2,2

por [Mike Wasson](https://github.com/MikeWasson)

> A maioria dos conjuntos de dados define as relações entre as entidades: os clientes têm pedidos; os livros têm autores; os produtos têm fornecedores. Usando o OData, os clientes podem navegar pelas relações de entidade. Dado um produto, você pode encontrar o fornecedor. Você também pode criar ou remover relações. Por exemplo, você pode definir o fornecedor de um produto.
>
> Este tutorial mostra como dar suporte a essas operações no OData v4 usando ASP.NET Web API. O tutorial se baseia no tutorial [criar um ponto de extremidade do OData v4 usando o ASP.NET Web API 2](create-an-odata-v4-endpoint.md).
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
>
> - API Web 2,1
> - OData v4
> - Visual Studio 2013 (Baixe o Visual Studio 2017 [aqui](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))
> - Entity Framework 6
> - .NET 4.5
>
> ## <a name="tutorial-versions"></a>Versões do tutorial
>
> Para o OData versão 3, consulte [relações de entidade de suporte no OData v3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).

## <a name="add-a-supplier-entity"></a>Adicionar uma entidade de fornecedor

> [!NOTE]
> O tutorial se baseia no tutorial [criar um ponto de extremidade do OData v4 usando o ASP.NET Web API 2](create-an-odata-v4-endpoint.md).

Primeiro, precisamos de uma entidade relacionada. Adicione uma classe chamada `Supplier` na pasta modelos.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample1.cs)]

Adicione uma propriedade de navegação à classe `Product`:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample2.cs?highlight=13-15)]

Adicione um novo **DbSet** à classe `ProductsContext`, para que Entity Framework inclua a tabela Supplier no banco de dados.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample3.cs?highlight=10)]

No WebApiConfig.cs, adicione um &quot;fornecedores&quot; entidade definida ao modelo de dados de entidade:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample4.cs?highlight=6)]

## <a name="add-a-suppliers-controller"></a>Adicionar um controlador de fornecedores

Adicione uma classe de `SuppliersController` à pasta controladores.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample5.cs)]

Não mostrarei como adicionar operações CRUD para esse controlador. As etapas são as mesmas para o controlador de produtos (consulte [criar um ponto de extremidade do OData v4](create-an-odata-v4-endpoint.md)).

## <a name="getting-related-entities"></a>Obtendo entidades relacionadas

Para obter o fornecedor de um produto, o cliente envia uma solicitação GET:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample6.cmd)]

Para dar suporte a essa solicitação, adicione o seguinte método à classe `ProductsController`:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample7.cs)]

Esse método usa uma Convenção de nomenclatura padrão

- Nome do método: GetX, em que X é a propriedade de navegação.
- Nome do parâmetro: *chave*

Se você seguir essa Convenção de nomenclatura, a API da Web mapeará automaticamente a solicitação HTTP para o método do controlador.

Exemplo de solicitação HTTP:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample8.cmd)]

Resposta HTTP de exemplo:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample9.cmd)]

### <a name="getting-a-related-collection"></a>Obtendo uma coleção relacionada

No exemplo anterior, um produto tem um fornecedor. Uma propriedade de navegação também pode retornar uma coleção. O código a seguir obtém os produtos para um fornecedor:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample10.cs)]

Nesse caso, o método retorna um **IQueryable** em vez de um **SingleResult&lt;t&gt;**

Exemplo de solicitação HTTP:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample11.cmd)]

Resposta HTTP de exemplo:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample12.cmd)]

## <a name="creating-a-relationship-between-entities"></a>Criando uma relação entre entidades

O OData dá suporte à criação ou remoção de relações entre duas entidades existentes. Na terminologia do OData v4, a relação é uma&quot;de referência de &quot;. (No OData v3, a relação foi chamada de *link*. As diferenças de protocolo não são importantes para este tutorial.)

Uma referência tem seu próprio URI, com o formulário `/Entity/NavigationProperty/$ref`. Por exemplo, aqui está o URI para endereçar a referência entre um produto e seu fornecedor:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample13.cmd)]

Para adicionar uma relação, o cliente envia uma solicitação POST ou PUT para esse endereço.

- PUT se a propriedade de navegação for uma única entidade, como `Product.Supplier`.
- POST se a propriedade de navegação for uma coleção, como `Supplier.Products`.

O corpo da solicitação contém o URI da outra entidade na relação. Aqui está um exemplo de solicitação:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample14.cmd)]

Neste exemplo, o cliente envia uma solicitação PUT para `/Products(6)/Supplier/$ref`, que é o URI de $ref para o `Supplier` do produto com ID = 6. Se a solicitação for realizada com sucesso, o servidor enviará uma resposta 204 (sem conteúdo):

[!code-console[Main](entity-relations-in-odata-v4/samples/sample15.cmd)]

Aqui está o método de controlador para adicionar uma relação a um `Product`:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample16.cs)]

O parâmetro *NavigationProperty* especifica a relação a ser definida. (Se houver mais de uma propriedade de navegação na entidade, você poderá adicionar mais instruções `case`.)

O parâmetro de *link* contém o URI do fornecedor. A API da Web analisa automaticamente o corpo da solicitação para obter o valor desse parâmetro.

Para pesquisar o fornecedor, precisamos da ID (ou da chave), que faz parte do parâmetro de *link* . Para fazer isso, use o seguinte método auxiliar:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample17.cs)]

Basicamente, esse método usa a biblioteca OData para dividir o caminho do URI em segmentos, localizar o segmento que contém a chave e converter a chave no tipo correto.

## <a name="deleting-a-relationship-between-entities"></a>Excluindo uma relação entre entidades

Para excluir uma relação, o cliente envia uma solicitação HTTP DELETE para o URI de $ref:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample18.cmd)]

Aqui está o método de controlador para excluir a relação entre um produto e um fornecedor:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample19.cs)]

Nesse caso, `Product.Supplier` é a &quot;1&quot; fim de uma relação de um para muitos, para que você possa remover a relação apenas definindo `Product.Supplier` para `null`.

Na &quot;muitos&quot; fim de uma relação, o cliente deve especificar qual entidade relacionada será removida. Para fazer isso, o cliente envia o URI da entidade relacionada na cadeia de caracteres de consulta da solicitação. Por exemplo, para remover "produto 1" do "fornecedor 1":

[!code-console[Main](entity-relations-in-odata-v4/samples/sample20.cmd?highlight=1)]

Para dar suporte a isso na API Web, precisamos incluir um parâmetro extra no método `DeleteRef`. Aqui está o método de controlador para remover um produto da relação de `Supplier.Products`.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample21.cs)]

O parâmetro de *chave* é a chave para o fornecedor e o parâmetro *relatedKey* é a chave do produto a ser removido da relação de `Products`. Observe que a API da Web obtém automaticamente a chave da cadeia de caracteres de consulta.
