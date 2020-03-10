---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
title: Criando uma ação (VB) | Microsoft Docs
author: microsoft
description: Saiba como adicionar uma nova ação a um controlador MVC ASP.NET. Saiba mais sobre os requisitos para um método ser uma ação.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: c8d93e11-ef78-4a30-afbc-f30419000a60
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
msc.type: authoredcontent
ms.openlocfilehash: b1b53bea899deecef203551b23c087944e3990ab
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78582001"
---
# <a name="creating-an-action-vb"></a>Criar uma ação (VB)

pela [Microsoft](https://github.com/microsoft)

> Saiba como adicionar uma nova ação a um controlador MVC ASP.NET. Saiba mais sobre os requisitos para um método ser uma ação.

O objetivo deste tutorial é explicar como você pode criar uma nova ação do controlador. Você aprende sobre os requisitos de um método de ação. Você também aprenderá a impedir que um método seja exposto como uma ação.

## <a name="adding-an-action-to-a-controller"></a>Adicionando uma ação a um controlador

Você adiciona uma nova ação a um controlador adicionando um novo método ao controlador. Por exemplo, o controlador na Listagem 1 contém uma ação denominada index () e uma ação chamada SayHello (). Ambos os métodos são expostos como ações.

**Listagem 1-Controllers\HomeController.vb**

[!code-vb[Main](creating-an-action-vb/samples/sample1.vb)]

Para ser exposto ao universo como uma ação, um método deve atender a determinados requisitos:

- O método deve ser público.
- O método não pode ser um método estático.
- O método não pode ser um método de extensão.
- O método não pode ser um construtor, getter ou setter.
- O método não pode ter tipos genéricos abertos.
- O método não é um método da classe base do controlador.
- O método não pode conter parâmetros **ref** ou **out** .

Observe que não há restrições no tipo de retorno de uma ação de controlador. Uma ação do controlador pode retornar uma cadeia de caracteres, um DateTime, uma instância da classe Random ou void. O ASP.NET MVC Framework converterá qualquer tipo de retorno que não seja um resultado de ação em uma cadeia de caracteres e renderizará a cadeia de caracteres para o navegador.

Quando você adiciona qualquer método que não viole esses requisitos a um controlador, o método é exposto como uma ação do controlador. Tenha cuidado aqui. Uma ação do controlador pode ser invocada por qualquer pessoa conectada à Internet. Não, por exemplo, criar uma ação do controlador DeleteMyWebsite ().

## <a name="preventing-a-public-method-from-being-invoked"></a>Impedindo que um método público seja invocado

Se você precisar criar um método público em uma classe de controlador e não quiser expor o método como uma ação de controlador, poderá impedir que o método seja invocado usando o atributo de&gt; &lt;não ação. Por exemplo, o controlador na Listagem 2 contém um método público denominado CompanySecrets () que é decorado com o atributo &lt;&gt; não ação.

**Listagem 2-Controllers\WorkController.vb**

[!code-vb[Main](creating-an-action-vb/samples/sample2.vb)]

Se você tentar invocar a ação do controlador CompanySecrets () digitando/Work/CompanySecrets na barra de endereços do navegador, você receberá a mensagem de erro na Figura 1.

[![invocando um método que não é de ação](creating-an-action-vb/_static/image1.jpg)](creating-an-action-vb/_static/image1.png)

**Figura 01**: invocando um método que não é de ação ([clique para exibir a imagem em tamanho normal](creating-an-action-vb/_static/image2.png))

> [!div class="step-by-step"]
> [Anterior](creating-a-controller-vb.md)
> [Próximo](aspnet-mvc-controllers-overview-cs.md)
