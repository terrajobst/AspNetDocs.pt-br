---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: Suporte a relações de entidade no OData v3 com API Web 2 | Microsoft Docs
author: MikeWasson
description: 'A maioria dos conjuntos de dados define as relações entre as entidades: os clientes têm pedidos; os livros têm autores; os produtos têm fornecedores. Usando o OData, os clientes podem navegar...'
ms.author: riande
ms.date: 02/26/2014
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: 726a7d51123805e05f6831ef9cd7eaa84b6c44bd
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600306"
---
# <a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a>Suporte a relações de entidade no OData v3 com API Web 2

por [Mike Wasson](https://github.com/MikeWasson)

[Baixar projeto concluído](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> A maioria dos conjuntos de dados define as relações entre as entidades: os clientes têm pedidos; os livros têm autores; os produtos têm fornecedores. Usando o OData, os clientes podem navegar pelas relações de entidade. Dado um produto, você pode encontrar o fornecedor. Você também pode criar ou remover relações. Por exemplo, você pode definir o fornecedor de um produto.
> 
> Este tutorial mostra como dar suporte a essas operações no ASP.NET Web API. O tutorial se baseia no tutorial [criando um ponto de extremidade OData v3 com a API Web 2](creating-an-odata-endpoint.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - API Web 2
> - Versão 3 do OData
> - Entity Framework 6

## <a name="add-a-supplier-entity"></a>Adicionar uma entidade de fornecedor

Primeiro, precisamos adicionar um novo tipo de entidade ao nosso feed OData. Vamos adicionar uma classe `Supplier`.

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

Essa classe usa uma cadeia de caracteres para a chave de entidade. Na prática, isso pode ser menos comum do que usar uma chave de inteiro. Mas vale a pena ver como o OData lida com outros tipos de chave além dos inteiros.

Em seguida, criaremos uma relação adicionando uma propriedade `Supplier` à classe `Product`:

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

Adicione um novo **DbSet** à classe `ProductServiceContext`, de modo que Entity Framework incluirá a tabela `Supplier` no banco de dados.

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

No WebApiConfig.cs, adicione uma entidade "Suppliers" ao modelo do EDM:

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a>Propriedades de navegação

Para obter o fornecedor de um produto, o cliente envia uma solicitação GET:

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

Aqui, "Supplier" é uma propriedade de navegação no tipo de `Product`. Nesse caso, `Supplier` se refere a um único item, mas uma propriedade de navegação também pode retornar uma coleção (relação um-para-muitos ou muitos para muitos).

Para dar suporte a essa solicitação, adicione o seguinte método à classe `ProductsController`:

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

O parâmetro de *chave* é a chave do produto. O método retorna a entidade&#8212;relacionada nesse caso, uma instância de `Supplier`. O nome do método e o nome do parâmetro são importantes. Em geral, se a propriedade de navegação for denominada "X", você precisará adicionar um método chamado "GetX". O método deve usar um parâmetro chamado "*Key*" que corresponda ao tipo de dados da chave do pai.

Também é importante incluir o atributo **[FromOdataUri]** no parâmetro de *chave* . Esse atributo informa à API Web para usar as regras de sintaxe do OData quando ele analisa a chave do URI de solicitação.

## <a name="creating-and-deleting-links"></a>Criando e excluindo links

O OData dá suporte à criação ou remoção de relações entre duas entidades. Na terminologia do OData, a relação é um "link". Cada link tem um URI com a *entidade*/$links/*entidade*do formulário. Por exemplo, o link de produto para fornecedor tem esta aparência:

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

Para criar um novo link, o cliente envia uma solicitação POST para o URI do link. O corpo da solicitação é o URI da entidade de destino. Por exemplo, suponha que haja um fornecedor com a chave "CTSO". Para criar um link de "Product (1)" para "Supplier (' CTSO ')", o cliente envia uma solicitação semelhante à seguinte:

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

Para excluir um link, o cliente envia uma solicitação de exclusão para o URI de link.

**Criando links**

Para permitir que um cliente crie links do fornecedor do produto, adicione o seguinte código à classe `ProductsController`:

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

Esse método usa três parâmetros:

- *chave*: a chave para a entidade pai (o produto)
- *NavigationProperty*: o nome da propriedade de navegação. Neste exemplo, a única propriedade de navegação válida é "Supplier".
- *link*: o URI do OData da entidade relacionada. Esse valor é obtido do corpo da solicitação. Por exemplo, o URI do link pode ser "`http://localhost/odata/Suppliers('CTSO')`, ou seja, o fornecedor com ID = ' CTSO '.

O método usa o link para pesquisar o fornecedor. Se o fornecedor correspondente for encontrado, o método definirá a propriedade `Product.Supplier` e salvará o resultado no banco de dados.

A parte mais difícil é analisar o URI do link. Basicamente, você precisa simular o resultado do envio de uma solicitação GET para esse URI. O método auxiliar a seguir mostra como fazer isso. O método invoca o processo de roteamento da API Web e retorna uma instância **ODataPath** que representa o caminho OData analisado. Para um URI de link, um dos segmentos deve ser a chave de entidade. (Caso contrário, o cliente enviou um URI inválido.)

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

**Excluindo links**

Para excluir um link, adicione o seguinte código à classe `ProductsController`:

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

Neste exemplo, a propriedade de navegação é uma única entidade de `Supplier`. Se a propriedade de navegação for uma coleção, o URI para excluir um link deverá incluir uma chave para a entidade relacionada. Por exemplo:

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

Essa solicitação remove a ordem 1 do cliente 1. Nesse caso, o método DeleteLink terá a seguinte assinatura:

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

O parâmetro *relatedKey* fornece a chave para a entidade relacionada. Portanto, em seu método de `DeleteLink`, pesquise a entidade principal pelo parâmetro de *chave* , localize a entidade relacionada pelo parâmetro *relatedKey* e, em seguida, remova a associação. Dependendo do modelo de dados, talvez seja necessário implementar as duas versões do `DeleteLink`. A API da Web chamará a versão correta com base no URI de solicitação.
