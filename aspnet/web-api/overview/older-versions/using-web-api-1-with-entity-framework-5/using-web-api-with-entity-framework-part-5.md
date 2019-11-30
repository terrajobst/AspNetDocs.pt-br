---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: 'Parte 5: criando uma interface do usuário dinâmica com knockout. js | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: bbdeba756de7986cfeb92aa10a57f4f101382f99
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600003"
---
# <a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a>Parte 5: criando uma interface do usuário dinâmica com knockout. js

por [Mike Wasson](https://github.com/MikeWasson)

[Baixar projeto concluído](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a>Criação uma interface do usuário dinâmica com Knockout.js

Nesta seção, usaremos knockout. js para adicionar funcionalidade à exibição de administrador.

[Knockout. js](http://knockoutjs.com/) é uma biblioteca JavaScript que facilita a ligação de controles HTML a dados. O knockout. js usa o padrão Model-View-ViewModel (MVVM).

- O *modelo* é a representação do lado do servidor dos dados no domínio de negócios (em nosso caso, produtos e pedidos).
- A *exibição* é a camada de apresentação (HTML).
- O *View-Model* é um objeto JavaScript que contém os dados do modelo. O View-Model é uma abstração de código da interface do usuário. Ele não tem conhecimento da representação HTML. Em vez disso, ele representa recursos abstratos da exibição, como "uma lista de itens".

A exibição é associada a dados ao modelo de exibição. As atualizações do modelo de exibição são refletidas automaticamente na exibição. O View-Model também obtém eventos da exibição, como cliques de botão, e executa operações no modelo, como criar um pedido.

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

Primeiro, definiremos o View-Model. Depois disso, Vincularemos a marcação HTML ao modelo de exibição.

Adicione a seguinte seção do Razor a admin. cshtml:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

Você pode adicionar essa seção em qualquer lugar do arquivo. Quando a exibição é renderizada, a seção é exibida na parte inferior da página HTML, logo antes do fechamento &lt;marca de&gt;/Body.

Todo o script desta página será inserido na marca de script indicada pelo comentário:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

Primeiro, defina uma classe de modelo de exibição:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

**ko. observableArray** é um tipo especial de objeto no knockout, chamado de *observável*. Na [documentação do knockout. js](http://knockoutjs.com/documentation/observables.html): um observável é um "objeto JavaScript que pode notificar os assinantes sobre alterações". Quando o conteúdo de uma alteração observável, a exibição é atualizada automaticamente para corresponder.

Para popular a matriz de `products`, faça uma solicitação AJAX para a API da Web. Lembre-se de que armazenamos o URI de base para a API na bolsa de exibição (consulte a [parte 4](using-web-api-with-entity-framework-part-4.md) do tutorial).

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

Em seguida, adicione funções ao modelo de exibição para criar, atualizar e excluir produtos. Essas funções enviam chamadas AJAX para a API da Web e usam os resultados para atualizar o modelo de exibição.

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

Agora, a parte mais importante: quando o DOM for carregado, chame a função **ko. applyBindings** e passe uma nova instância do `ProductsViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

O método **ko. applyBindings** ativa o Knockout e conecta o modelo de exibição à exibição.

Agora que temos um View-Model, podemos criar as associações. No knockout. js, você faz isso adicionando `data-bind` atributos a elementos HTML. Por exemplo, para associar uma lista HTML a uma matriz, use a associação de `foreach`:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

A associação de `foreach` itera através da matriz e cria elementos filho para cada objeto na matriz. Associações nos elementos filho podem se referir a propriedades nos objetos de matriz.

Adicione as seguintes associações à lista "atualização-produtos":

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

O elemento `<li>` ocorre dentro do escopo da Associação **foreach** . Isso significa que o Knockout renderizará o elemento uma vez para cada produto na matriz de `products`. Todas as associações dentro do elemento `<li>` referem-se à instância do produto. Por exemplo, `$data.Name` refere-se à propriedade `Name` no produto.

Para definir os valores das entradas de texto, use a associação de `value`. Os botões são associados a funções no modo de exibição de modelo, usando a associação de `click`. A instância do produto é passada como um parâmetro para cada função. Para obter mais informações, a [documentação do knockout. js](http://knockoutjs.com/documentation/observables.html) tem uma boa descrição das várias associações.

Em seguida, adicione uma associação para o evento **Enviar** no formulário Adicionar produto:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

Essa ligação chama a função `create` no View-Model para criar um novo produto.

Este é o código completo para o modo de exibição de administrador:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

Execute o aplicativo, faça logon com a conta de administrador e clique no link "admin". Você deve ver a lista de produtos e ser capaz de criar, atualizar ou excluir produtos.

> [!div class="step-by-step"]
> [Anterior](using-web-api-with-entity-framework-part-4.md)
> [Próximo](using-web-api-with-entity-framework-part-6.md)
