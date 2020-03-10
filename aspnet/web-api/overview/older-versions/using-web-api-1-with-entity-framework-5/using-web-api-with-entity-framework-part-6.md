---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: 'Parte 6: criando controladores de produtos e pedidos | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: e0bf88e3477acbde910cde956042449bc86ce79a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78524986"
---
# <a name="part-6-creating-product-and-order-controllers"></a>Parte 6: criando controladores de produtos e pedidos

por [Mike Wasson](https://github.com/MikeWasson)

[Baixar projeto concluído](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a>Adicionar um controlador de produtos

O controlador de administração é para usuários que têm privilégios de administrador. Os clientes, por outro lado, podem exibir produtos, mas não podem criá-los, atualizá-los ou excluí-los.

Podemos restringir facilmente o acesso aos métodos post, PUT e Delete, deixando os métodos get abertos. Mas examine os dados que são retornados para um produto:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

A propriedade `ActualCost` não deve ser visível para os clientes! A solução é definir um DTO ( *objeto de transferência de dados* ) que inclua um subconjunto de propriedades que deve ser visível aos clientes. Usaremos o LINQ para projetar `Product` instâncias para `ProductDTO` instâncias.

Adicione uma classe chamada `ProductDTO` à pasta modelos.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

Agora, adicione o controlador. Em Gerenciador de Soluções, clique com o botão direito do mouse na pasta controladores. Selecione **Adicionar**e, em seguida, selecione **controlador**. Na caixa de diálogo **Adicionar controlador** , nomeie o controlador &quot;ProductsController&quot;. Em **modelo**, selecione **controlador de API vazio**.

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

Substitua tudo no arquivo de origem pelo código a seguir:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

O controlador ainda usa o `OrdersContext` para consultar o banco de dados. Mas, em vez de retornar `Product` instâncias diretamente, chamamos `MapProducts` para projetar em `ProductDTO` instâncias:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

O método `MapProducts` retorna um **IQueryable**, portanto, podemos compor o resultado com outros parâmetros de consulta. Você pode ver isso no método `GetProduct`, que adiciona uma cláusula **Where** à consulta:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a>Adicionar um controlador de pedidos

Em seguida, adicione um controlador que permita aos usuários criar e exibir pedidos.

Começaremos com outro DTO. Em Gerenciador de Soluções, clique com o botão direito do mouse na pasta modelos e adicione uma classe chamada `OrderDTO` use a seguinte implementação:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

Agora, adicione o controlador. Em Gerenciador de Soluções, clique com o botão direito do mouse na pasta controladores. Selecione **Adicionar**e, em seguida, selecione **controlador**. Na caixa de diálogo **Adicionar controlador** , defina as seguintes opções:

- Em **nome do controlador**, insira "OrdersController".
- Em **modelo**, selecione "controlador de API com ações de leitura/gravação, usando Entity Framework".
- Em **classe de modelo**, selecione &quot;ordem (ProductStore. Models)&quot;.
- Em **classe de contexto de dados**, selecione &quot;OrdersContext (ProductStore. Models)&quot;.

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

Clique em **Adicionar**. Isso adiciona um arquivo chamado OrdersController.cs. Em seguida, precisamos modificar a implementação padrão do controlador.

Primeiro, exclua os métodos `PutOrder` e `DeleteOrder`. Para este exemplo, os clientes não podem modificar ou excluir pedidos existentes. Em um aplicativo real, você precisaria de muita lógica de back-end para lidar com esses casos. (Por exemplo, o pedido já foi enviado?)

Altere o método `GetOrders` para retornar apenas os pedidos que pertencem ao usuário:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

Altere o método `GetOrder` da seguinte maneira:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

Aqui estão as alterações que fizemos no método:

- O valor de retorno é uma instância de `OrderDTO`, em vez de uma `Order`.
- Quando consultamos o banco de dados para o pedido, usamos o método [DbQuery. include](https://msdn.microsoft.com/library/gg696395) para buscar as entidades relacionadas `OrderDetail` e `Product`.
- Nós mesclamos o resultado usando uma projeção.

A resposta HTTP conterá uma matriz de produtos com quantidades:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

Esse formato é mais fácil para os clientes consumirem do que o grafo do objeto original, que contém entidades aninhadas (ordem, detalhes e produtos).

O último método a ser considerado `PostOrder`. No momento, esse método usa uma instância de `Order`. Mas considere o que acontece se um cliente envia um corpo de solicitação como este:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

Essa é uma ordem bem estruturada e Entity Framework irá inseri-la em um banco de dados. Mas ele contém uma entidade de produto que não existia anteriormente. O cliente acabou de criar um novo produto em nosso banco de dados! Isso será uma surpresa para o departamento de preenchimento de pedidos, quando eles veem um pedido de Koala. A moral é ter muito cuidado com os dados que você aceita em uma solicitação POST ou PUT.

Para evitar esse problema, altere o método `PostOrder` para tomar uma instância `OrderDTO`. Use o `OrderDTO` para criar o `Order`.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

Observe que usamos as propriedades `ProductID` e `Quantity` e ignoramos quaisquer valores que o cliente enviou para o nome ou o preço do produto. Se a ID do produto não for válida, ela violará a restrição FOREIGN KEY no banco de dados e a inserção falhará, como deveria.

Aqui está o método de `PostOrder` completo:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

Por fim, adicione o atributo **Authorize** ao controlador:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

Agora, somente os usuários registrados podem criar ou exibir pedidos.

> [!div class="step-by-step"]
> [Anterior](using-web-api-with-entity-framework-part-5.md)
> [Próximo](using-web-api-with-entity-framework-part-7.md)
