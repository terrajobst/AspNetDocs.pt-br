---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: 'Parte 8: carrinho de compras com atualizações do AJAX | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo de loja de música MVC do ASP.NET. A parte 8 aborda o carrinho de compras com as atualizações do Ajax.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 89897ad41b217764cbd17317d4bf5d6a5c5d488f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539252"
---
# <a name="part-8-shopping-cart-with-ajax-updates"></a>Parte 8: carrinho de compras com atualizações do AJAX

por [Jon Galloway](https://github.com/jongalloway)

> A loja de música MVC é um aplicativo de tutorial que apresenta e explica passo a passo como usar o ASP.NET MVC e o Visual Studio para desenvolvimento para a Web.  
>   
> A loja de música MVC é uma implementação de armazenamento de exemplo leve que vende os álbuns de música online e implementa a administração básica de site, a entrada do usuário e a funcionalidade do carrinho de compras.  
>   
> Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo de loja de música MVC do ASP.NET. A parte 8 aborda o carrinho de compras com as atualizações do Ajax.

Vamos permitir que os usuários coloquem álbuns em seu carrinho sem se registrar, mas eles precisarão se registrar como convidados para concluir o check-out. O processo de compra e de check-out será separado em dois controladores: um controlador ShoppingCart que permite adicionar itens anonimamente a um carrinho e um controlador de check-out que manipula o processo de check-out. Vamos começar com o carrinho de compras nesta seção e, em seguida, criar o processo de check-out na seção a seguir.

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a>Adicionando as classes de modelo de carrinho, ordem e OrderDetail

Nosso carrinho de compras e os processos de check-out farão uso de algumas novas classes. Clique com o botão direito do mouse na pasta modelos e adicione uma classe de carrinho (Cart.cs) com o código a seguir.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

Essa classe é muito parecida com as outras que usamos até agora, com exceção do atributo [key] para a propriedade recordId. Nossos itens de carrinho terão um identificador de cadeia de caracteres chamado Cartid para permitir compras anônimas, mas a tabela inclui uma chave primária de inteiro chamada recordId. Por convenção, Entity Framework código primeiro espera que a chave primária para uma tabela denominada cart seja a Cartid ou ID, mas podemos substituir facilmente isso por meio de anotações ou código, se desejarmos. Este é um exemplo de como podemos usar as convenções simples em Entity Framework Code-First quando elas se adequarem a nós, mas não estamos restritos por elas quando não estiverem.

Em seguida, adicione uma classe Order (Order.cs) com o código a seguir.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

Essa classe acompanha as informações de resumo e entrega de um pedido. **Ele ainda não será compilado**, pois tem uma propriedade de navegação OrderDetails que depende de uma classe que ainda não criamos. Vamos corrigir isso agora adicionando uma classe chamada OrderDetail.cs, adicionando o código a seguir.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

Vamos fazer uma última atualização para nossa classe MusicStoreEntities para incluir DbSets que expõem essas novas classes de modelo, incluindo também uma&gt;de&lt;artista. A classe MusicStoreEntities atualizada aparece como abaixo.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a>Gerenciando a lógica de negócios do carrinho de compras

Em seguida, criaremos a classe ShoppingCart na pasta modelos. O modelo ShoppingCart manipula o acesso a dados para a tabela de carrinhos. Além disso, ele tratará a lógica de negócios para a adição e remoção de itens do carrinho de compras.

Como não queremos exigir que os usuários se inscrevam para uma conta apenas para adicionar itens ao carrinho de compras, atribuíremos aos usuários um identificador exclusivo temporário (usando um GUID ou um identificador global exclusivo) quando acessarem o carrinho de compras. Armazenaremos essa ID usando a classe de sessão ASP.NET.

*Observação: a sessão ASP.NET é um local conveniente para armazenar informações específicas do usuário, que expirarão depois que saírem do site. Embora o uso indevido do estado da sessão possa causar implicações de desempenho em sites maiores, nosso uso leve funcionará bem para fins de demonstração.*

A classe ShoppingCart expõe os seguintes métodos:

**AddToCart** usa um álbum como um parâmetro e o adiciona ao carrinho do usuário. Como a tabela de carrinhos rastreia a quantidade de cada álbum, ela inclui a lógica para criar uma nova linha, se necessário, ou apenas incrementar a quantidade se o usuário já tiver solicitado uma cópia do álbum.

**RemoveFromCart** usa uma ID de álbum e a remove do carrinho do usuário. Se o usuário tiver apenas uma cópia do álbum em seu carrinho, a linha será removida.

**EmptyCart** remove todos os itens do carrinho de compras de um usuário.

**GetCartItems** recupera uma lista de CartItems para exibição ou processamento.

**GetCount** recupera um número total de álbuns que um usuário tem em seu carrinho de compras.

**GetTotal** calcula o custo total de todos os itens no carrinho.

**CreateOrder** converte o carrinho de compras em um pedido durante a fase de check-out.

**Getcart** é um método estático que permite que nossos controladores obtenham um objeto de carrinho. Ele usa o método **Getcarrinhoid** para lidar com a leitura de cartid a partir da sessão do usuário. O método getcarrinhoid requer o HttpContextBase para que ele possa ler o Carrinhoid do usuário a partir da sessão do usuário.

Aqui está a **classe ShoppingCart**completa:

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a>ViewModels

Nosso controlador de carrinho de compras precisará comunicar algumas informações complexas para suas exibições que não são mapeadas corretamente para nossos objetos de modelo. Não queremos modificar nossos modelos para se adequar às nossas exibições; Classes de modelo devem representar nosso domínio, não a interface do usuário. Uma solução seria passar as informações para nossas exibições usando a classe ViewBag, como fizemos com as informações suspensas do Store Manager, mas passar muitas informações por meio de ViewBag é difícil de gerenciar.

Uma solução para isso é usar o padrão *ViewModel* . Ao usar esse padrão, criamos classes fortemente tipadas que são otimizadas para nossos cenários de exibição específicos e que expõem propriedades para os valores dinâmicos/conteúdo exigidos por nossos modelos de exibição. Nossas classes de controlador podem então popular e passar essas classes otimizadas para exibição para nosso modelo de exibição a ser usado. Isso habilita a segurança de tipo, a verificação de tempo de compilação e o IntelliSense do editor em modelos de exibição.

Criaremos dois modelos de exibição para uso em nosso controlador de carrinho de compras: o ShoppingCartViewModel manterá o conteúdo do carrinho de compras do usuário e o ShoppingCartRemoveViewModel será usado para exibir informações de confirmação quando um usuário remover algo do carrinho.

Vamos criar uma nova pasta ViewModels na raiz do nosso projeto para manter as coisas organizadas. Clique com o botão direito do mouse no projeto, selecione Adicionar/nova pasta.

![](mvc-music-store-part-8/_static/image1.jpg)

Nomeie a pasta ViewModels.

![](mvc-music-store-part-8/_static/image1.png)

Em seguida, adicione a classe ShoppingCartViewModel na pasta ViewModels. Ele tem duas propriedades: uma lista de itens do carrinho e um valor decimal para manter o preço total de todos os itens no carrinho.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

Agora, adicione o ShoppingCartRemoveViewModel à pasta ViewModels, com as quatro propriedades a seguir.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a>O controlador do carrinho de compras

O controlador do carrinho de compras tem três objetivos principais: adicionar itens a um carrinho, remover itens do carrinho e exibir itens no carrinho. Ele fará uso das três classes que acabamos de criar: ShoppingCartViewModel, ShoppingCartRemoveViewModel e ShoppingCart. Como no StoreController e no StoreManagerController, adicionaremos um campo para manter uma instância de MusicStoreEntities.

Adicione um novo controlador de carrinho de compras ao projeto usando o modelo de controlador vazio.

![](mvc-music-store-part-8/_static/image2.png)

Aqui está o controlador ShoppingCart completo. As ações de índice e adicionar controlador devem parecer muito familiares. As ações do controlador remove e CartSummary tratam de dois casos especiais, que abordaremos na seção a seguir.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a>Atualizações do AJAX com o jQuery

Em seguida, criaremos uma página de índice do carrinho de compras que é fortemente tipada para o ShoppingCartViewModel e usa o modelo de exibição de lista usando o mesmo método de antes.

![](mvc-music-store-part-8/_static/image3.png)

No entanto, em vez de usar um HTML. ActionLink para remover itens do carrinho, usaremos o jQuery para "vincular" o evento Click de todos os links nesta exibição que têm a classe HTML RemoveLink. Em vez de lançar o formulário, esse manipulador de eventos de clique simplesmente fará um retorno de chamada AJAX para nossa ação do controlador RemoveFromCart. O RemoveFromCart retorna um resultado serializado JSON, que nosso retorno de chamada do jQuery, em seguida, analisa e executa quatro atualizações rápidas na página usando o jQuery:

- 1. Remove o álbum excluído da lista
- 2. Atualiza a contagem de carrinhos no cabeçalho
- 3. Exibe uma mensagem de atualização para o usuário
- 4. Atualiza o preço total do carrinho

Como o cenário de remoção está sendo manipulado por um retorno de chamada AJAX dentro da exibição de índice, não precisamos de uma exibição adicional para a ação RemoveFromCart. Aqui está o código completo para a exibição/ShoppingCart/Index:

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

Para testar isso, precisamos ser capazes de adicionar itens ao nosso carrinho de compras. Atualizaremos nossa exibição de **detalhes da loja** para incluir um botão "adicionar ao carrinho". Enquanto estamos com ele, podemos incluir algumas das informações adicionais do álbum que adicionamos desde a última atualização deste modo de exibição: gênero, artista, preço e arte do álbum. O código de exibição de detalhes do repositório atualizado aparece como mostrado abaixo.

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

Agora, podemos clicar na loja e testar adicionar e remover álbuns de e para nosso carrinho de compras. Execute o aplicativo e navegue até o índice da loja.

![](mvc-music-store-part-8/_static/image4.png)

Em seguida, clique em um gênero para exibir uma lista de álbuns.

![](mvc-music-store-part-8/_static/image5.png)

Clicar em um título de álbum agora mostra nosso modo de exibição de detalhes do álbum atualizado, incluindo o botão "adicionar ao carrinho".

![](mvc-music-store-part-8/_static/image6.png)

Clicar no botão "adicionar ao carrinho" mostra nossa exibição de índice do carrinho de compras com a lista de resumo do carrinho de compras.

![](mvc-music-store-part-8/_static/image7.png)

Depois de carregar seu carrinho de compras, você pode clicar no link remover do carrinho para ver a atualização do AJAX para seu carrinho de compras.

![](mvc-music-store-part-8/_static/image8.png)

Criamos um carrinho de compras funcional que permite que usuários não registrados adicionem itens ao seu carrinho. Na seção a seguir, vamos permitir que eles se registrem e concluam o processo de check-out.

> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-7.md)
> [Próximo](mvc-music-store-part-9.md)
