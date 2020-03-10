---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: Ações e funções no OData v4 usando ASP.NET Web API 2,2 | Microsoft Docs
author: MikeWasson
description: No OData, as ações e as funções são uma maneira de adicionar comportamentos do lado do servidor que não são facilmente definidos como operações CRUD em entidades. Este tutorial mostra como...
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: f5af94e93e5b7f2351d40febbf1a468d635c9db1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556220"
---
# <a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a>Ações e funções no OData v4 usando o ASP.NET Web API 2,2

por [Mike Wasson](https://github.com/MikeWasson)

> No OData, as ações e as funções são uma maneira de adicionar comportamentos do lado do servidor que não são facilmente definidos como operações CRUD em entidades. Este tutorial mostra como adicionar ações e funções a um ponto de extremidade do OData v4, usando a API da Web 2,2. O tutorial se baseia no tutorial [criar um ponto de extremidade do OData v4 usando ASP.NET Web API 2](create-an-odata-v4-endpoint.md)
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
>
> - API Web 2,2
> - OData v4
> - Visual Studio 2013 (Baixe o Visual Studio 2017 [aqui](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))
> - .NET 4.5
>
> ## <a name="tutorial-versions"></a>Versões do tutorial
>
> Para o OData versão 3, consulte [ações de OData no ASP.NET Web API 2](../odata-v3/odata-actions.md).

A diferença entre *ações* e *funções* é que as ações podem ter efeitos colaterais e funções não. As ações e as funções podem retornar dados. Alguns usos para ações incluem:

- Transações complexas.
- Manipulando várias entidades ao mesmo tempo.
- Permitir atualizações apenas para determinadas propriedades de uma entidade.
- Enviando dados que não são uma entidade.

As funções são úteis para retornar informações que não correspondem diretamente a uma entidade ou coleção.

Uma ação (ou função) pode ter como destino uma única entidade ou uma coleção. Na terminologia do OData, essa é a *Associação*. Você também pode ter &quot;&quot; ações/funções desassociadas, que são chamadas de operações estáticas no serviço.

## <a name="example-adding-an-action"></a>Exemplo: adicionando uma ação

Vamos definir uma ação para classificar um produto.

> [!NOTE]
> Este tutorial se baseia no tutorial [criar um ponto de extremidade do OData v4 usando ASP.NET Web API 2](create-an-odata-v4-endpoint.md)

Primeiro, adicione um modelo de `ProductRating` para representar as classificações.

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

Além disso, adicione um **DbSet** à classe `ProductsContext`, para que o EF crie uma tabela de classificações no banco de dados.

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a>Adicionar a ação ao EDM

No WebApiConfig.cs, adicione o seguinte código:

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

O método **EntityTypeConfiguration. Action** adiciona uma ação ao EDM (modelo de dados de entidade). O método de **parâmetro** especifica um parâmetro tipado para a ação.

Esse código também define o namespace para o EDM. O namespace é importante porque o URI para a ação inclui o nome de ação totalmente qualificado:

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> Em uma configuração típica do IIS, o ponto nessa URL fará com que o IIS retorne o erro 404. Você pode resolver isso adicionando a seção a seguir ao seu arquivo Web. config:

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a>Adicionar um método de controlador para a ação

Para habilitar a &quot;taxa&quot; ação, adicione o seguinte método a `ProductsController`:

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

Observe que o nome do método corresponde ao nome da ação. O atributo **[HttpPost]** especifica que o método é http post.

Para invocar a ação, o cliente envia uma solicitação HTTP POST como a seguinte:

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

A &quot;taxa&quot; ação está associada a instâncias de produtos, portanto, o URI para a ação é o nome de ação totalmente qualificado anexado ao URI da entidade. (Lembre-se de que definimos o namespace do EDM como &quot;ProductService&quot;, portanto, o nome da ação totalmente qualificado é &quot;ProductService. Rate&quot;.)

O corpo da solicitação contém os parâmetros de ação como uma carga JSON. A API da Web converte automaticamente a carga JSON em um objeto **ODataActionParameters** , que é apenas um dicionário de valores de parâmetro. Use este dicionário para acessar os parâmetros no método do controlador.

Se o cliente enviar os parâmetros de ação no formato incorreto, o valor de **ModelState. IsValid** será false. Marque esse sinalizador no método do controlador e retorne um erro se **IsValid** for false.

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a>Exemplo: adicionando uma função

Agora, vamos adicionar uma função OData que retorna o produto mais caro. Como antes, a primeira etapa é adicionar a função ao EDM. No WebApiConfig.cs, adicione o código a seguir.

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

Nesse caso, a função é associada à coleção Products, em vez de instâncias individuais Product. Os clientes invocam a função enviando uma solicitação GET:

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

Este é o método de controlador para esta função:

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

Observe que o nome do método corresponde ao nome da função. O atributo **[HttpGet]** especifica que o método é um método HTTP Get.

Aqui está a resposta HTTP:

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a>Exemplo: adicionando uma função não associada

O exemplo anterior era uma função associada a uma coleção. Neste próximo exemplo, criaremos uma função *não associada* . As funções não associadas são chamadas como operações estáticas no serviço. A função neste exemplo retornará o imposto sobre vendas para um determinado código postal.

No arquivo WebApiConfig, adicione a função ao EDM:

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

Observe que estamos chamando a **função** diretamente no **ODataModelBuilder**, em vez do tipo de entidade ou da coleção. Isso informa ao construtor de modelos que a função está desassociada.

Este é o método de controlador que implementa a função:

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

Não importa em qual controlador de API Web você coloca esse método. Você pode colocá-lo em `ProductsController`ou definir um controlador separado. O atributo **[ODataRoute]** define o modelo de URI para a função.

Aqui está um exemplo de solicitação de cliente:

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

A resposta HTTP:

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
