---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: Suporte a ações OData no ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: 'No OData, as ações são uma maneira de adicionar comportamentos do lado do servidor que não são facilmente definidos como operações CRUD em entidades. Alguns usos para ações incluem: implementar...'
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: ae8b23f0868f992cb2bbbf14ee3f7ac848501515
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600346"
---
# <a name="supporting-odata-actions-in-aspnet-web-api-2"></a>Suporte a ações OData no ASP.NET Web API 2

por [Mike Wasson](https://github.com/MikeWasson)

[Baixar projeto concluído](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> No OData, as *ações* são uma maneira de adicionar comportamentos do lado do servidor que não são facilmente definidos como operações CRUD em entidades. Alguns usos para ações incluem:
> 
> - Implementando transações complexas.
> - Manipulando várias entidades ao mesmo tempo.
> - Permitir atualizações apenas para determinadas propriedades de uma entidade.
> - Enviando informações para o servidor que não está definido em uma entidade.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - API Web 2
> - Versão 3 do OData
> - Entity Framework 6

## <a name="example-rating-a-product"></a>Exemplo: classificando um produto

Neste exemplo, queremos permitir que os usuários classifiquem os produtos e, em seguida, exponham as classificações médias de cada produto. No banco de dados, armazenaremos uma lista de classificações, com chave para produtos.

Aqui está o modelo que poderemos usar para representar as classificações em Entity Framework:

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

Mas não queremos que os clientes POSTEm um objeto `ProductRating` em uma coleção de "classificações". Intuitivamente, a classificação é associada à coleção de produtos e o cliente deve precisar apenas lançar o valor de classificação.

Portanto, em vez de usar as operações CRUD normais, definimos uma ação que um cliente pode invocar em um produto. Na terminologia do OData, a ação está *associada* às entidades do produto.

>As ações têm efeitos colaterais no servidor. Por esse motivo, eles são invocados usando solicitações HTTP POST. As ações podem ter parâmetros e tipos de retorno, que são descritos nos metadados do serviço. O cliente envia os parâmetros no corpo da solicitação e o servidor envia o valor de retorno no corpo da resposta. Para invocar a ação "produto de taxa", o cliente envia uma POSTAgem para um URI como o seguinte:

[!code-console[Main](odata-actions/samples/sample2.cmd)]

Os dados na solicitação POST são simplesmente a classificação do produto:

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a>Declarar a ação no Modelo de Dados de Entidade

Na configuração da API Web, adicione a ação ao EDM (modelo de dados de entidade):

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

Esse código define "RateProduct" como uma ação que pode ser executada em entidades de produto. Ele também declara que a ação usa um parâmetro **int** chamado "rating" e retorna um valor **int** .

## <a name="add-the-action-to-the-controller"></a>Adicionar a ação ao controlador

A ação "RateProduct" está associada a entidades de produto. Para implementar a ação, adicione um método chamado `RateProduct` ao controlador de produtos:

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

Observe que o nome do método corresponde ao nome da ação no EDM. O método tem dois parâmetros:

- *chave*: a chave do produto a ser avaliada.
- *Parameters*: um dicionário de valores de parâmetro de ação.

Se você estiver usando as convenções de roteamento padrão, o parâmetro de chave deverá ser nomeado "Key". Também é importante incluir o atributo **[FromOdataUri]** , conforme mostrado. Esse atributo informa à API Web para usar as regras de sintaxe do OData quando ele analisa a chave do URI de solicitação.

Use o dicionário de *parâmetros* para obter os parâmetros de ação:

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

Se o cliente enviar os parâmetros de ação no formato correto, o valor de **ModelState. IsValid** será true. Nesse caso, você pode usar o dicionário **ODataActionParameters** para obter os valores de parâmetro. Neste exemplo, a ação de `RateProduct` usa um único parâmetro chamado "rating".

## <a name="action-metadata"></a>Metadados de ação

Para exibir os metadados de serviço, envie uma solicitação GET para o/OData/$metadata. Aqui está a parte dos metadados que declara a ação de `RateProduct`:

[!code-xml[Main](odata-actions/samples/sample7.xml)]

O elemento **FunctionImport** declara a ação. A maioria dos campos é auto-explicativa, mas dois vale a pena observar:

- **Isassociable** significa que a ação pode ser invocada na entidade de destino, pelo menos algumas das vezes.
- **IsAlwaysBindable** significa que a ação sempre pode ser invocada na entidade de destino.

A diferença é que algumas ações estão sempre disponíveis para clientes, mas outras ações podem depender do estado da entidade. Por exemplo, suponha que você defina uma ação de "compra". Você só pode comprar um item que esteja em estoque. Se o item estiver fora de estoque, um cliente não poderá invocar essa ação.

Quando você define o EDM, o método de **ação** cria uma ação sempre vinculável:

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

Falarei sobre ações não sempre vinculáveis (também chamadas de ações *transitórias* ) mais adiante neste tópico.

## <a name="invoking-the-action"></a>Invocando a ação

Agora, vamos ver como um cliente invocaria essa ação. Suponha que o cliente queira fornecer uma classificação de 2 para o produto com ID = 4. Aqui está um exemplo de mensagem de solicitação, usando o formato JSON para o corpo da solicitação:

[!code-console[Main](odata-actions/samples/sample9.cmd)]

Esta é a mensagem de resposta:

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a>Associando uma ação a um conjunto de entidades

No exemplo anterior, a ação está associada a uma única entidade: o cliente classifica um único produto. Você também pode associar uma ação a uma coleção de entidades. Basta fazer as seguintes alterações:

No EDM, adicione a ação à propriedade de **coleção** da entidade.

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

No método de controlador, omita o parâmetro de *chave* .

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

Agora, o cliente invoca a ação no conjunto de entidades produtos:

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a>Ações com parâmetros de coleta

As ações podem ter parâmetros que usam uma coleção de valores. No EDM, use **CollectionParameter&lt;t&gt;** para declarar o parâmetro.

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

Isso declara um parâmetro chamado "ratings" que usa uma coleção de valores **int** . No método de controlador, você ainda Obtém o valor do parâmetro do objeto **ODataActionParameters** , mas agora o valor é um valor de **&lt;int&gt;de ICollection** :

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a>Ações transitórias

No exemplo "RateProduct", os usuários sempre podem classificar um produto, portanto, a ação está sempre disponível. Mas algumas ações dependem do estado da entidade. Por exemplo, em um serviço de aluguel de vídeo, a ação de "check-out" nem sempre está disponível. (Depende se uma cópia desse vídeo está disponível.) Esse tipo de ação é chamado de ação *transitória* .

Nos metadados do serviço, uma ação transitória tem **IsAlwaysBindable** igual a false. Esse é, na verdade, o valor padrão, portanto, os metadados terão a seguinte aparência:

[!code-xml[Main](odata-actions/samples/sample16.xml)]

Aqui está o motivo pelo qual isso é importante: se uma ação for transitória, o servidor precisará informar ao cliente quando a ação estiver disponível. Ele faz isso incluindo um link para a ação na entidade. Aqui está um exemplo de uma entidade de filme:

[!code-console[Main](odata-actions/samples/sample17.cmd)]

A propriedade "#CheckOut" contém um link para a ação de check-out. Se a ação não estiver disponível, o servidor omite o link.

Para declarar uma ação transitória no EDM, chame o método **transitóriaaction** :

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

Além disso, você deve fornecer uma função que retorne um link de ação para uma determinada entidade. Defina essa função chamando **HasActionLink**. Você pode escrever a função como uma expressão lambda:

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

Se a ação estiver disponível, a expressão lambda retornará um link para a ação. O serializador OData inclui esse link quando ele serializa a entidade. Quando a ação não está disponível, a função retorna `null`.

## <a name="additional-resources"></a>Recursos adicionais

[Exemplo de ações de OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
