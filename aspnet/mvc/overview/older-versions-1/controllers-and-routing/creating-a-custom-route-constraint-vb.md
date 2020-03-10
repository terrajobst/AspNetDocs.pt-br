---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
title: Criando uma restrição de rota personalizada (VB) | Microsoft Docs
author: StephenWalther
description: Stephen Walther demonstra como você pode criar uma restrição de rota personalizada. Implementamos uma restrição personalizada simples que impede que uma rota seja correspondida w...
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 892edb27-1cc2-4eaf-8314-dbc2efc6228a
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 2330708cf4a28180ce8a05f4696bf7a7a32092d6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601405"
---
# <a name="creating-a-custom-route-constraint-vb"></a>Criação de uma restrição de rota personalizada (VB)

por [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther demonstra como você pode criar uma restrição de rota personalizada. Implementamos uma restrição personalizada simples que impede a correspondência de uma rota quando uma solicitação do navegador é feita de um computador remoto.

O objetivo deste tutorial é demonstrar como você pode criar uma restrição de rota personalizada. Uma restrição de rota personalizada permite impedir que uma rota seja correspondida, a menos que alguma condição personalizada seja correspondida.

Neste tutorial, criamos uma restrição de rota de localhost. A restrição de rota do localhost só corresponde a solicitações feitas do computador local. As solicitações remotas de pela Internet não são correspondidas.

Implemente uma restrição de rota personalizada implementando a interface IRouteConstraint. Esta é uma interface extremamente simples que descreve um único método:

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample1.vb)]

O método retorna um valor booliano. Se você retornar false, a rota associada à restrição não corresponderá à solicitação do navegador.

A restrição de localhost está contida na Listagem 1.

**Listagem 1-LocalhostConstraint. vb**

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample2.vb)]

A restrição na Listagem 1 tira proveito da Propriedade IsLocal exposta pela classe HttpRequest. Essa propriedade retornará true quando o endereço IP da solicitação for 127.0.0.1 ou quando o IP da solicitação for o mesmo que o endereço IP do servidor.

Você usa uma restrição personalizada dentro de uma rota definida no arquivo global. asax. O arquivo global. asax na Listagem 2 usa a restrição localhost para impedir que qualquer pessoa solicite uma página de administração, a menos que faça a solicitação do servidor local. Por exemplo, uma solicitação de/Admin/DeleteAll falhará quando feita de um servidor remoto.

**Listagem 2-global. asax**

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample3.vb)]

A restrição de localhost é usada na definição da rota do administrador. Essa rota não será correspondida por uma solicitação de navegador remoto. No entanto, perceba que outras rotas definidas em global. asax podem corresponder à mesma solicitação. É importante entender que uma restrição impede que uma rota específica corresponda a uma solicitação e não a todas as rotas definidas no arquivo global. asax.

Observe que a rota padrão foi comentada do arquivo global. asax na Listagem 2. Se você incluir a rota padrão, a rota padrão corresponderia às solicitações do controlador de administração. Nesse caso, os usuários remotos ainda podiam invocar ações do controlador de administração, mesmo que suas solicitações não correspondam à rota do administrador.

> [!div class="step-by-step"]
> [Anterior](creating-a-route-constraint-vb.md)
