---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: Tipos abertos no OData V4 com ASP.NET Web API | Microsoft Docs
author: microsoft
description: No OData v4, um tipo aberto é um tipo estruturado que contém propriedades dinâmicas, além de quaisquer propriedades declaradas na definição de tipo. Abrir...
ms.author: riande
ms.date: 09/15/2014
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 950442c071bf50d2c8c1588971f13f85c4891436
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622174"
---
# <a name="open-types-in-odata-v4-with-aspnet-web-api"></a>Tipos abertos no OData V4 com ASP.NET Web API

pela [Microsoft](https://github.com/microsoft)

> No OData v4, um *tipo aberto* é um tipo estruturado que contém propriedades dinâmicas, além de quaisquer propriedades declaradas na definição de tipo. Os tipos abertos permitem que você adicione flexibilidade aos seus modelos de dados. Este tutorial mostra como usar tipos abertos no ASP.NET Web API OData.
> 
> Este tutorial pressupõe que você já sabe como criar um ponto de extremidade OData no ASP.NET Web API. Caso contrário, comece lendo [criar um ponto de extremidade do OData v4](create-an-odata-v4-endpoint.md) primeiro.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - API Web OData 5,3
> - OData v4

Primeiro, algumas terminologias do OData:

- Tipo de entidade: um tipo estruturado com uma chave.
- Tipo complexo: um tipo estruturado sem uma chave.
- Open Type: um tipo com propriedades dinâmicas. Os tipos de entidade e os tipos complexos podem ser abertos.

O valor de uma propriedade dinâmica pode ser um tipo primitivo, tipo complexo ou tipo de enumeração; ou uma coleção de qualquer um desses tipos. Para obter mais informações sobre tipos abertos, consulte a [especificação do OData v4](http://www.odata.org/documentation/odata-version-4-0/).

## <a name="install-the-web-odata-libraries"></a>Instalar as bibliotecas do Web OData

Use o Gerenciador de pacotes NuGet para instalar as bibliotecas OData da API Web mais recentes. Na janela do console do Gerenciador de pacotes:

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a>Definir os tipos CLR

Comece definindo os modelos do EDM como tipos CLR.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

Quando o Modelo de Dados de Entidade (EDM) é criado,

- `Category` é um tipo de enumeração.
- `Address` é um tipo complexo. (Ele não tem uma chave, portanto, não é um tipo de entidade.)
- `Customer` é um tipo de entidade. (Tem uma chave.)
- `Press` é um tipo complexo aberto.
- `Book` é um tipo de entidade aberta.

Para criar um tipo aberto, o tipo CLR deve ter uma propriedade do tipo `IDictionary<string, object>`, que contém as propriedades dinâmicas.

## <a name="build-the-edm-model"></a>Compilar o modelo EDM

Se você usar o **ODataConventionModelBuilder** para criar o EDM, `Press` e `Book` serão adicionados automaticamente como tipos abertos, com base na presença de uma propriedade `IDictionary<string, object>`.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

Você também pode criar o EDM explicitamente, usando o **ODataModelBuilder**.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a>Adicionar um controlador OData

Em seguida, adicione um controlador OData. Para este tutorial, usaremos um controlador simplificado que dá suporte apenas a solicitações GET e POST e usa uma lista na memória para armazenar entidades.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

Observe que a primeira instância de `Book` não tem propriedades dinâmicas. A segunda instância de `Book` tem as seguintes propriedades dinâmicas:

- "Published": tipo primitivo
- "Authors": coleção de tipos primitivos
- "OtherCategories": coleção de tipos de enumeração.

Além disso, a propriedade `Press` dessa instância de `Book` tem as seguintes propriedades dinâmicas:

- "Blog": tipo primitivo
- "Address": tipo complexo

## <a name="query-the-metadata"></a>Consultar os metadados

Para obter o documento de metadados OData, envie uma solicitação GET para `~/$metadata`. O corpo da resposta deve ser semelhante a este:

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

No documento de metadados, você pode ver que:

- Para os tipos `Book` e `Press`, o valor do atributo `OpenType` é true. Os tipos `Customer` e `Address` não têm esse atributo.
- O tipo de entidade `Book` tem três propriedades declaradas: ISBN, title e pressione. Os metadados OData não incluem a propriedade `Book.Properties` da classe CLR.
- Da mesma forma, o tipo complexo `Press` tem apenas duas propriedades declaradas: nome e categoria. Os metadados não incluem a propriedade `Press.DynamicProperties` da classe CLR.

## <a name="query-an-entity"></a>Consultar uma entidade

Para obter o livro com ISBN igual a "978-0-7356-7942-9", envie uma solicitação GET para `~/Books('978-0-7356-7942-9')`. O corpo da resposta deve ser semelhante ao seguinte. (Recuado para torná-lo mais legível.)

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

Observe que as propriedades dinâmicas são incluídas embutidas com as propriedades declaradas.

## <a name="post-an-entity"></a>POSTAR uma entidade

Para adicionar uma entidade de livro, envie uma solicitação POST para `~/Books`. O cliente pode definir propriedades dinâmicas na carga de solicitação.

Aqui está uma solicitação de exemplo. Observe as propriedades "Price" e "published".

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

Se você definir um ponto de interrupção no método do controlador, poderá ver que a API Web adicionou essas propriedades ao dicionário de `Properties`.

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a>Recursos adicionais

[Exemplo de tipo Open do OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
