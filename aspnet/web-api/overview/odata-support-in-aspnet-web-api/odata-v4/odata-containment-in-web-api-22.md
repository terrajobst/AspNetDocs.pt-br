---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: Contenção no OData v4 usando a API Web 2,2 | Microsoft Docs
author: rick-anderson
description: Tradicionalmente, uma entidade só poderia ser acessada se fosse encapsulada dentro de um conjunto de entidades. Mas o OData v4 fornece duas opções adicionais, singleton e con...
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 50050e40c4c42bf6d769d077c27864ee6417d4db
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78525119"
---
# <a name="containment-in-odata-v4-using-web-api-22"></a>Contenção no OData v4 usando a API Web 2,2

por JinFu Tan

> Tradicionalmente, uma entidade só poderia ser acessada se fosse encapsulada dentro de um conjunto de entidades. Mas o OData v4 fornece duas opções adicionais, singleton e confinamento, que são compatíveis com o WebAPI 2,2.

Este tópico mostra como definir uma contenção em um ponto de extremidade OData no WebApi 2,2. Para obter mais informações sobre contenção, consulte [a contenção é proveniente do OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx). Para criar um ponto de extremidade do OData v4 na API Web, consulte [criar um ponto de extremidade do OData v4 usando ASP.NET Web API 2,2](create-an-odata-v4-endpoint.md).

Primeiro, criaremos um modelo de domínio de confinamento no serviço OData, usando este modelo de dados:

![Modelo de dados](odata-containment-in-web-api-22/_static/image1.png)

Uma conta contém muitos PaymentInstruments (PI), mas não definimos um conjunto de entidades para um PI. Em vez disso, o PIs só pode ser acessado por meio de uma conta.

Você pode baixar a solução usada neste tópico do [codeplex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).

## <a name="defining-the-data-model"></a>Definindo o modelo de dados

1. Defina os tipos CLR.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    O atributo `Contained` é usado para propriedades de navegação de confinamento.
2. Gere o modelo EDM com base nos tipos CLR.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    O `ODataConventionModelBuilder` tratará da criação do modelo EDM se o atributo `Contained` for adicionado à propriedade de navegação correspondente. Se a propriedade for um tipo de coleção, uma função `GetCount(string NameContains)` também será criada.

    Os metadados gerados serão semelhantes ao seguinte:

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    O atributo `ContainsTarget` indica que a propriedade de navegação é uma contenção.

## <a name="define-the-containing-entity-set-controller"></a>Definir o controlador de conjunto de entidades de contenção

Entidades contidas não têm seu próprio controlador; a ação é definida no controlador de conjunto de entidades que a contém. Neste exemplo, há um AccountsController, mas nenhum PaymentInstrumentsController.

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

Se o caminho OData for de 4 ou mais segmentos, somente o roteamento de atributos funcionará, como `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` no controlador acima. Caso contrário, o atributo e o roteamento convencional funcionarão: por exemplo, `GetPayInPIs(int key)` corresponde a `GET ~/Accounts(1)/PayinPIs`.

*Agradecemos ao Leo Hu pelo conteúdo original deste artigo.*
