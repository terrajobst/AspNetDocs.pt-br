---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
title: Criando uma ação (C#) | Microsoft Docs
author: microsoft
description: Saiba como adicionar uma nova ação a um controlador MVC ASP.NET. Saiba mais sobre os requisitos para um método ser uma ação.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: cb33b28c-3025-4bd1-a1fa-eaa3af7bb56f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
msc.type: authoredcontent
ms.openlocfilehash: ebba935383819935ad85c95245666f4eaf6a0dca
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78582050"
---
# <a name="creating-an-action-c"></a>Criar uma ação (C#)

pela [Microsoft](https://github.com/microsoft)

> Saiba como adicionar uma nova ação a um controlador MVC ASP.NET. Saiba mais sobre os requisitos para um método ser uma ação.

O objetivo deste tutorial é explicar como você pode criar uma nova ação do controlador. Você aprende sobre os requisitos de um método de ação. Você também aprenderá a impedir que um método seja exposto como uma ação.

## <a name="adding-an-action-to-a-controller"></a>Adicionando uma ação a um controlador

Você adiciona uma nova ação a um controlador adicionando um novo método ao controlador. Por exemplo, o controlador na Listagem 1 contém uma ação denominada index () e uma ação chamada SayHello (). Ambos os métodos são expostos como ações.

**Listagem 1-Controllers\HomeController.cs**

[!code-csharp[Main](creating-an-action-cs/samples/sample1.cs)]

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

Se você precisar criar um método público em uma classe de controlador e não quiser expor o método como uma ação de controlador, poderá impedir que o método seja invocado usando o atributo [NonAction]. Por exemplo, o controlador na Listagem 2 contém um método público chamado CompanySecrets () que é decorado com o atributo [NonAction].

**Listagem 2-Controllers\WorkController.cs**

[!code-csharp[Main](creating-an-action-cs/samples/sample2.cs)]

Se você tentar invocar a ação do controlador CompanySecrets () digitando/Work/CompanySecrets na barra de endereços do navegador, você receberá a mensagem de erro na Figura 1.

[![invocando um método que não é de ação](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)

**Figura 01**: invocando um método que não é de ação ([clique para exibir a imagem em tamanho normal](creating-an-action-cs/_static/image2.png))

> [!div class="step-by-step"]
> [Anterior](creating-a-controller-cs.md)
> [Próximo](asp-net-mvc-routing-overview-vb.md)
