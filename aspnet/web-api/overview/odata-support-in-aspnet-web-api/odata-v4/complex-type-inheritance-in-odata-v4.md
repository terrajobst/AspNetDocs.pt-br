---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: Herança de tipo complexo no OData V4 com ASP.NET Web API | Microsoft Docs
author: microsoft
description: De acordo com a especificação do OData v4, um tipo complexo pode herdar de outro tipo complexo. (Um tipo complexo é um tipo estruturado sem uma chave.) API da Web...
ms.author: riande
ms.date: 09/16/2014
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 3d90216c8e594055f77577eb6d8b1d978ae4c24d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556304"
---
# <a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a>Herança de tipo complexo no OData V4 com ASP.NET Web API

pela [Microsoft](https://github.com/microsoft)

> De acordo com a [especificação](http://www.odata.org/documentation/odata-version-4-0/)do OData v4, um tipo complexo pode herdar de outro tipo complexo. (Um tipo *complexo* é um tipo estruturado sem uma chave.) A API Web OData 5,3 dá suporte à herança de tipo complexo.
> 
> Este tópico mostra como criar um EDM (modelo de dados de entidade) com tipos de herança complexos. Para obter o código-fonte completo, consulte [exemplo de herança de tipo complexo do OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - API Web OData 5,3
> - OData v4

## <a name="model-hierarchy"></a>Hierarquia de modelo

Para ilustrar a herança de tipo complexo, usaremos a seguinte hierarquia de classe.

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

`Shape` é um tipo complexo abstrato. `Rectangle`, `Triangle`e `Circle` são tipos complexos derivados de `Shape`e `RoundRectangle` deriva de `Rectangle`. `Window` é um tipo de entidade e contém uma instância de `Shape`.

Aqui estão as classes CLR que definem esses tipos.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a>Compilar o modelo EDM

Para criar o EDM, você pode usar **ODataConventionModelBuilder**, que infere as relações de herança dos tipos CLR.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

Você também pode criar o EDM explicitamente, usando o **ODataModelBuilder**. Isso usa mais código, mas lhe dá mais controle sobre o EDM.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

Esses dois exemplos criam o mesmo esquema EDM.

## <a name="metadata-document"></a>Documento de metadados

Este é o documento de metadados OData, mostrando herança de tipo complexo.

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

No documento de metadados, você pode ver que:

- O tipo complexo `Shape` é abstract.
- O `Rectangle`, `Triangle`e `Circle` tipo complexo têm o tipo base `Shape`.
- O tipo de `RoundRectangle` tem o tipo base `Rectangle`.

## <a name="casting-complex-types"></a>Conversão de tipos complexos

Agora há suporte para a conversão em tipos complexos. Por exemplo, a consulta a seguir converte um `Shape` em um `Rectangle`.

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

Aqui está a carga de resposta:

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
