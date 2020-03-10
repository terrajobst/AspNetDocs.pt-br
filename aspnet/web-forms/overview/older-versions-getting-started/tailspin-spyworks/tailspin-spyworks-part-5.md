---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: 'Parte 5: lógica de negócios | Microsoft Docs'
author: JoeStagner
description: Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo Tailspin Spyworks. A parte 5 adiciona alguma lógica de negócios.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: c60eece9223c47304786d7b0d0ca4b11ac0572e9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78630301"
---
# <a name="part-5-business-logic"></a>Parte 5: lógica de negócios

por [Joe Stagner](https://github.com/JoeStagner)

> A Tailspin Spyworks demonstra como é extremamente simples criar aplicativos poderosos e escalonáveis para a plataforma .NET. Ele mostra como usar os ótimos novos recursos do ASP.NET 4 para criar uma loja online, incluindo compras, check-out e administração.
> 
> Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo Tailspin Spyworks. A parte 5 adiciona alguma lógica de negócios.

## <a id="_Toc260221671"></a>Adicionando alguma lógica de negócios

Queremos que nossa experiência de compra esteja disponível sempre que alguém visitar nosso site. Os visitantes poderão procurar e adicionar itens ao carrinho de compras, mesmo que não estejam registrados ou conectados. Quando estiverem prontos para fazer check-out, eles terão a opção de autenticar e, se ainda não forem membros, eles poderão criar uma conta.

Isso significa que precisaremos implementar a lógica para converter o carrinho de compras de um estado anônimo para um estado de "usuário registrado".

Vamos criar um diretório chamado "classes" e clicar com o botão direito do mouse na pasta e criar um novo arquivo de "classe" chamado MyShoppingCart.cs

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

Como mencionado anteriormente, estenderemos a classe que implementa a página MyShoppingCart. aspx e faremos isso usando. Construção "classe parcial" da rede avançada.

A chamada gerada para o nosso arquivo MyShoppingCart.aspx.cf tem esta aparência.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

Observe o uso da palavra-chave "partial".

O arquivo de classe que acabamos de gerar é semelhante a este.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

Mesclaremos nossas implementações adicionando a palavra-chave Partial a esse arquivo também.

Nosso novo arquivo de classe agora tem esta aparência.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

O primeiro método que adicionaremos à nossa classe é o método "AddItem". Esse é o método que será finalmente chamado quando o usuário clicar nos links "adicionar à arte" na lista de produtos e nas páginas de detalhes do produto.

Acrescente o seguinte às instruções using na parte superior da página.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

E adicione esse método à classe MyShoppingCart.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

Estamos usando LINQ to Entities para ver se o item já está no carrinho. Nesse caso, atualizamos a quantidade de pedidos do item; caso contrário, criamos uma nova entrada para o item selecionado

Para chamar esse método, implementaremos uma página AddToCart. aspx que não apenas faz a classe desse método, mas, em seguida, exibia o atual shopping a = carrinho após o item ter sido adicionado.

Clique com o botão direito do mouse no nome da solução no Gerenciador de soluções e adicione uma nova página chamada AddToCart. aspx como fizemos anteriormente.

Embora possamos usar essa página para exibir resultados provisórios, como problemas de estoque baixo, etc. em nossa implementação, a página não será realmente renderizada, mas, em vez disso, chamamos a lógica de "adição" e o redirecionamento.

Para fazer isso, adicionaremos o código a seguir à página\_evento de carregamento.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

Observe que estamos recuperando o produto para adicionar ao carrinho de compras de um parâmetro QueryString e chamar o método AddItem de nossa classe.

Supondo que nenhum erro seja encontrado, o controle é passado para a página SHoppingCart. aspx, que implementaremos totalmente a seguir. Se houver um erro, lançaremos uma exceção.

No momento, ainda não implementamos um manipulador de erro global, portanto, essa exceção não seria tratada pelo nosso aplicativo, mas corrigiremos isso em breve.

Observe também o uso da instrução Debug. Fail () (disponível via `using System.Diagnostics;)`

O aplicativo está sendo executado dentro do depurador, esse método exibirá uma caixa de diálogo detalhada com informações sobre o estado dos aplicativos junto com a mensagem de erro que especificamos.

Durante a execução na produção, a instrução Debug. Fail () é ignorada.

Você notará no código acima uma chamada para um método em nossos nomes de classe de carrinho de compras "GetShoppingCartId".

Adicione o código para implementar o método da seguinte maneira.

Observe que também adicionamos os botões Update e checkout e um rótulo onde podemos exibir o carrinho "total".

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

Agora podemos adicionar itens ao nosso carrinho de compras, mas não implementamos a lógica para exibir o carrinho após a adição de um produto.

Portanto, na página MyShoppingCart. aspx, adicionaremos um controle EntityDataSource e um controle GridVire da seguinte maneira.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

Chame o formulário no designer para que você possa clicar duas vezes no botão atualizar carrinho e gerar o manipulador de eventos de clique que é especificado na declaração na marcação.

Vamos implementar os detalhes mais tarde, mas fazer isso nos permitirá criar e executar nosso aplicativo sem erros.

Ao executar o aplicativo e adicionar um item ao carrinho de compras, você verá isso.

![](tailspin-spyworks-part-5/_static/image2.jpg)

Observe que desviamos da exibição de grade "padrão" Implementando três colunas personalizadas.

O primeiro é um campo editável, "associado" para a quantidade:

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

A seguir está uma coluna "calculada" que exibe o total do item de linha (o custo do item vezes a quantidade a ser ordenada):

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

Por fim, temos uma coluna personalizada que contém um controle de caixa de seleção que o usuário usará para indicar que o item deve ser removido do gráfico de compras.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

Como você pode ver, a linha total do pedido está vazia, então vamos adicionar uma lógica para calcular o total do pedido.

Primeiro, implementaremos um método "GetTotal" para nossa classe MyShoppingCart.

No arquivo MyShoppingCart.cs, adicione o código a seguir.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

Em seguida, na página\_carregar manipulador de eventos, poderemos chamar nosso método GetTotal. Ao mesmo tempo, vamos adicionar um teste para ver se o carrinho de compras está vazio e ajustar a exibição adequadamente, se for.

Agora, se o carrinho de compras estiver vazio, obteremos:

![](tailspin-spyworks-part-5/_static/image4.jpg)

E, se não, veremos nosso total.

![](tailspin-spyworks-part-5/_static/image5.jpg)

No entanto, essa página ainda não foi concluída.

Precisaremos de uma lógica adicional para recalcular o carrinho de compras removendo itens marcados para remoção e determinando novos valores de quantidade, já que alguns podem ter sido alterados na grade pelo usuário.

Permite adicionar um método "RemoveItem" à nossa classe de carrinho de compras em MyShoppingCart.cs para lidar com o caso quando um usuário marca um item para remoção.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

Agora, vamos ao AD um método para lidar com a circunstância quando um usuário simplesmente altera a qualidade a ser ordenada no GridView.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

Com os recursos básicos de remoção e atualização em vigor, podemos implementar a lógica que realmente atualiza o carrinho de compras no banco de dados. (Em MyShoppingCart.cs)

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

Você observará que esse método espera dois parâmetros. Uma é a ID do carrinho de compras e a outra é uma matriz de objetos do tipo definido pelo usuário.

Portanto, para minimizar a dependência de nossa lógica em especificidades de interface do usuário, definimos uma estrutura de dados que podemos usar para passar os itens do carrinho de compras para nosso código sem que nosso método precise acessar diretamente o controle GridView.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

Em nosso arquivo MyShoppingCart.aspx.cs, podemos usar essa estrutura em nosso manipulador de eventos de clique de botão de atualização da seguinte maneira. Observe que, além de atualizar o carrinho, atualizaremos o total do carrinho também.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

Observação com interesse específico esta linha de código:

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

GetValues () é uma função auxiliar especial que iremos implementar em MyShoppingCart.aspx.cs da seguinte maneira.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

Isso fornece uma maneira limpa de acessar os valores dos elementos associados em nosso controle GridView. Como nosso controle de caixa de seleção "Remover item" não está associado, acessaremos por meio do método FindControl ().

Neste estágio do desenvolvimento do projeto, estamos preparando para implementar o processo de check-out.

Antes de fazer isso, vamos usar o Visual Studio para gerar o banco de dados de associação e adicionar um usuário ao repositório de associação.

> [!div class="step-by-step"]
> [Anterior](tailspin-spyworks-part-4.md)
> [Próximo](tailspin-spyworks-part-6.md)
