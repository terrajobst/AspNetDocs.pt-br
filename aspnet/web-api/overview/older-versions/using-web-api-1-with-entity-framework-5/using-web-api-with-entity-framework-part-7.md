---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: 'Parte 7: criando a página principal | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: fe4074c701159a137be3644d65ca844f160c2399
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598675"
---
# <a name="part-7-creating-the-main-page"></a>Parte 7: criando a página principal

por [Mike Wasson](https://github.com/MikeWasson)

[Baixar projeto concluído](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a>Criação da página principal

Nesta seção, você criará a página principal do aplicativo. Essa página será mais complexa do que a página de administração, portanto, vamos abordá-la em várias etapas. Ao longo do caminho, você verá algumas técnicas mais avançadas de Knockout. js. Este é o layout básico da página:

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- "Produtos" contém uma matriz de produtos.
- "Carrinho" contém uma matriz de produtos com quantidades. Clicar em "adicionar ao carrinho" atualizará o carrinho.
- "Orders" mantém uma matriz de IDs de pedido.
- "Detalhes" contém um detalhe do pedido, que é uma matriz de itens (produtos com quantidades)

Vamos começar definindo um layout básico em HTML, sem vinculação de dados ou script. Abra o arquivo views/home/index. cshtml e substitua todo o conteúdo pelo seguinte:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

Em seguida, adicione uma seção scripts e crie um modelo de exibição vazio:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

Com base no design esboçado anteriormente, nosso modelo de exibição precisa de observáveis para produtos, carrinho, pedidos e detalhes. Adicione as seguintes variáveis ao objeto `AppViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

Os usuários podem adicionar itens da lista de produtos no carrinho e remover itens do carrinho. Para encapsular essas funções, criaremos outra classe View-Model que representa um produto. Adicione o seguinte código a `AppViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

A classe `ProductViewModel` contém duas funções que são usadas para mover o produto de e para o carrinho: `addItemToCart` adiciona uma unidade do produto ao carrinho e `removeAllFromCart` remove todas as quantidades do produto.

Os usuários podem selecionar um pedido existente e obter os detalhes do pedido. Encapsularemos essa funcionalidade em outro View-Model:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

O `OrderDetailsViewModel` é inicializado com um pedido e busca os detalhes do pedido enviando uma solicitação AJAX ao servidor.

Além disso, observe a propriedade `total` no `OrderDetailsViewModel`. Essa propriedade é um tipo especial de observável chamado de [observed calculado](http://knockoutjs.com/documentation/computedObservables.html). Como o nome indica, um observed calculado permite que você vincule dados a um valor&#8212;calculado nesse caso, o custo total do pedido.

Em seguida, adicione essas funções a `AppViewModel`:

- `resetCart` remove todos os itens do carrinho.
- `getDetails` Obtém os detalhes de um pedido (enviando um novo `OrderDetailsViewModel` para a lista de `details`).
- `createOrder` cria um novo pedido e esvazia o carrinho.

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

Por fim, inicialize o modelo de exibição fazendo solicitações AJAX para os produtos e pedidos:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

OK, isso é muito código, mas nós o criamos passo a passo, então espero que o design esteja claro. Agora, podemos adicionar algumas associações knockout. js ao HTML.

**Produtos**

Aqui estão as associações para a lista de produtos:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

Isso itera na matriz de produtos e exibe o nome e o preço. O botão "adicionar à ordem" só fica visível quando o usuário está conectado.

O botão "adicionar à ordem" chama `addItemToCart` na instância de `ProductViewModel` do produto. Isso demonstra um bom recurso de Knockout. js: quando um View-Model contém outros modos de exibição-modelos, você pode aplicar as associações ao modelo interno. Neste exemplo, as associações dentro do `foreach` são aplicadas a cada uma das instâncias de `ProductViewModel`. Essa abordagem é muito mais limpa do que colocar toda a funcionalidade em um único modelo de exibição.

**Carrinho**

Aqui estão as associações para o carrinho:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

Isso itera na matriz de carrinhos e exibe o nome, o preço e a quantidade. Observe que o link "remover" e o botão "criar ordem" estão associados às funções de modelo de exibição.

**Solicitar**

Aqui estão as associações para a lista Orders:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

Isso itera sobre os pedidos e mostra a ID do pedido. O evento de clique no link está associado à função `getDetails`.

**Detalhes do pedido**

Aqui estão as associações para os detalhes do pedido:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

Isso itera sobre os itens na ordem e exibe o produto, o preço e a quantidade. A div ao redor só será visível se a matriz de detalhes contiver um ou mais itens.

## <a name="conclusion"></a>Conclusão

Neste tutorial, você criou um aplicativo que usa Entity Framework para se comunicar com o banco de dados e ASP.NET Web API para fornecer uma interface voltada para o público sobre a camada de dado. Usamos o ASP.NET MVC 4 para renderizar as páginas HTML e knockout. js Plus jQuery para fornecer interações dinâmicas sem recargas de página.

Recursos adicionais:

- [Mapa de conteúdo de acesso a dados do ASP.NET](https://msdn.microsoft.com/library/6759sth4.aspx)
- [Centro de desenvolvedores Entity Framework](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [Anterior](using-web-api-with-entity-framework-part-6.md)
