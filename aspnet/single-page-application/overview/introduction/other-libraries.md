---
uid: single-page-application/overview/introduction/other-libraries
title: Conhece uma biblioteca diferente do Knockout? | Microsoft Docs
author: madskristensen
description: Conhece uma biblioteca diferente do Knockout?
ms.author: riande
ms.date: 02/05/2013
ms.assetid: a8367c6d-ef94-4dff-a010-5eff9e6eea96
msc.legacyurl: /single-page-application/overview/introduction/other-libraries
msc.type: authoredcontent
ms.openlocfilehash: 64a4ad1fb411f7291a5cba634afdf4d2fdb16d55
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578543"
---
# <a name="know-a-library-other-than-knockout"></a>Conhece uma biblioteca diferente do Knockout?

por [Mads Kristensen](https://github.com/madskristensen)

O [modelo Spa (aplicativo de página única)](knockoutjs-template.md) é uma ótima maneira de começar a escrever aplicativos de página única. O modelo usa [KnockoutJS](http://knockoutjs.com/) para associar dados de aplicativo a elementos DOM.

Mas o Knockout não é a única biblioteca JavaScript para a criação de aplicativos cliente avançados. Outras bibliotecas resolvem desafios semelhantes de maneiras diferentes. Você pode preferir uma única biblioteca sobre outra, portanto, disponibilizamos vários modelos criados pela Comunidade para download. Cada um desses modelos usa uma combinação diferente de bibliotecas JavaScript de cliente.

Para instalar um modelo criado pela Comunidade, visite uma das páginas de modelo listadas abaixo e clique no botão baixar. Os modelos são fornecidos como arquivos VSIX.

## <a name="backbonejs"></a>BackboneJS

[Modelo Spa do backbone. js](../templates/backbonejs-template.md). Este modelo fornece um esqueleto inicial para o desenvolvimento de um aplicativo [backbone. js](http://backbonejs.org/) no ASP.NET MVC. Ele fornece a funcionalidade básica de logon de usuário, incluindo a inscrição, a entrada, a redefinição de senha e a confirmação do usuário com modelos de email básicos.

## <a name="breezejs"></a>BreezeJS

O [BreezeJS](http://www.breezejs.com/?utm_source=ms-spa) é uma biblioteca de software livre para gerenciar dados avançados em um cliente JavaScript. A Breeze lida com consultas, Caching, controle de alterações, validação e muito mais. A Breeze dos recursos de dois modelos:

- O modelo de [Breeze/Knockout](../templates/breezeknockout-template.md) estende o modelo de spa do knockout, mostrando como é fácil criar um aplicativo de página única com a Breeze para gerenciamento de dados e KnockoutJS para vinculação de dados.
- O modelo [Breeze/angular](../templates/breezeangular-template.md) também estende o modelo de spa do Knockout com a Breeze, mas usando a biblioteca [AngularJS](http://angularjs.org) para associação de dados, injeção de dependência e gerenciamento de tela.

Além disso, o [modelo de spa quente](../templates/hottowel-template.md) usa BreezeJS.

## <a name="emberjs"></a>EmberJS

[Modelo Spa EmberJS](../templates/emberjs-template.md). Este modelo usa o [Ember](http://emberjs.com/), uma poderosa biblioteca de JavaScript do MVC que resolve uma ampla gama de desafios para a criação de aplicativos cliente avançados.

O modelo SPA Ember é uma reimplementação do modelo de SPA do knockout, usando Templating EmberJS e Handlebars.

## <a name="hot-towel"></a>Toalha quente

[Modelo de spa de alta-toalha](../templates/hottowel-template.md). Este modelo traz várias bibliotecas JavaScript, incluindo Breeze, Knockout, RequireJS e bootstrap do Twitter.

Em comparação com os outros modelos listados aqui, o modelo de toalha quente fornece um aplicativo mais completo do qual você pode criar o seu próprio. Há mais conceitos a serem considerados, mas quando você os entende, esse modelo pode ser apenas o que você está procurando. Se você quiser criar um SPA, mas não puder decidir onde começar, usar a toalha quente e, em segundos, terá um SPA e todas as ferramentas necessárias para criá-lo.

## <a name="feature-table"></a>Tabela de recursos

Aqui estão os recursos fornecidos por cada modelo de SPA:

|                        | ASP.NET SPA | Corporativa | Breeze/angular | Breeze/KO |  Ember   | Toalha quente |
|------------------------|-------------|----------|----------------|-----------|----------|-----------|
|      Exemplo de ToDo       |  &#10003;   |          |    &#10003;    | &#10003;  | &#10003; |           |
|     Modelo simples      |             | &#10003; |                |           |          | &#10003;  |
| Navegação e histórico |             | &#10003; |    &#10003;    |           | &#10003; | &#10003;  |
|        Bibliotecas       |             |          |                |           |          |           |
|        Angular         |             |          |    &#10003;    |           |          |           |
|    &#8195;Backbone     |             | &#10003; |                |           |          |           |
|         Breeze         |             |          |    &#10003;    | &#10003;  |          | &#10003;  |
|        Durandal        |             |          |                |           |          | &#10003;  |
|         Ember          |             |          |                |           | &#10003; |           |
|        Separação        |  &#10003;   |          |                | &#10003;  |          | &#10003;  |
