---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: 'Parte 4: listando produtos | Microsoft Docs'
author: JoeStagner
description: Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo Tailspin Spyworks. A parte 4 aborda a listagem de produtos com o GridView contr...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: 7af1b8afa2ecc8df9846f2edd2091b26b93a811c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78566979"
---
# <a name="part-4-listing-products"></a>Parte 4: listando produtos

por [Joe Stagner](https://github.com/JoeStagner)

> A Tailspin Spyworks demonstra como é extremamente simples criar aplicativos poderosos e escalonáveis para a plataforma .NET. Ele mostra como usar os ótimos novos recursos do ASP.NET 4 para criar uma loja online, incluindo compras, check-out e administração.
> 
> Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo Tailspin Spyworks. A parte 4 aborda a listagem de produtos com o controle GridView.

## <a id="_Toc260221670"></a>Listando produtos com o controle GridView

Vamos começar a implementar nossa página ProductList. aspx "clicando com o botão direito do mouse" em nossa solução e selecionando "Add" e "New Item".

![](tailspin-spyworks-part-4/_static/image1.jpg)

Escolha "formulário da Web usando a página mestra" e insira um nome de página de ProductList. aspx ".

Clique em "Adicionar".

![](tailspin-spyworks-part-4/_static/image2.jpg)

Em seguida, escolha a pasta "estilos" onde colocamos a página site. Master e a selecionamos na janela "conteúdo da pasta".

![](tailspin-spyworks-part-4/_static/image3.jpg)

Clique em "OK" para criar a página.

Nosso banco de dados é populado com a data do produto, como mostrado abaixo.

![](tailspin-spyworks-part-4/_static/image4.jpg)

Depois que nossa página for criada, usaremos novamente uma fonte de dados de entidade para acessar os dados do produto, mas, nesta instância, precisamos selecionar as entidades do produto e precisamos restringir os itens que são retornados apenas para aqueles da categoria selecionada.

Para fazer isso, vamos dizer à EntityDataSource para gerar automaticamente a cláusula WHERE e especificaremos o WhereParameter.

Você se lembrará de que quando criamos os itens de menu em nosso "menu de categoria do produto", criamos dinamicamente o link adicionando o CategoryID à QueryString para cada link. Informaremos a fonte de dados de entidade para derivar o parâmetro WHERE do parâmetro QueryString.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

Em seguida, configuraremos o controle ListView para exibir uma lista de produtos. Para criar uma experiência de compra ideal, compactaremos vários recursos concisos em cada produto individual exibido em nosso ListVew.

- O nome do produto será um link para a exibição de detalhes do produto.
- O preço do produto será exibido.
- Uma imagem do produto será exibida e selecionaremos dinamicamente a imagem de um diretório de imagens de catálogo em nosso aplicativo.
- Incluiremos um link para adicionar imediatamente o produto específico ao carrinho de compras.

Aqui está a marcação para nossa instância de controle ListView.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

Estamos criando dinamicamente vários links para cada produto exibido.

Além disso, antes de testarmos a própria página nova, precisamos criar a estrutura de diretório para as imagens do catálogo de produtos da seguinte maneira.

![](tailspin-spyworks-part-4/_static/image1.png)

Depois que nossas imagens de produto estiverem acessíveis, podemos testar nossa página de lista de produtos.

![](tailspin-spyworks-part-4/_static/image5.jpg)

Na home page do site, clique em um dos links da lista de categorias.

![](tailspin-spyworks-part-4/_static/image6.jpg)

Agora precisamos implementar a página ProductDetails. aspx e a funcionalidade AddToCart.

Use o arquivo-&gt;novo para criar um nome de página ProductDetails. aspx usando a página mestra do site como fizemos anteriormente.

Usaremos novamente um controle EntityDataSource para acessar o registro específico do produto no banco de dados e usaremos um controle FormView ASP.NET para exibir os dados do produto da seguinte maneira.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

Não se preocupe se a formatação parecer um pouco engraçado para você. A marcação acima deixa espaço no layout de exibição para alguns recursos que implementaremos mais tarde.

O carrinho de compras representará a lógica mais complexa em nosso aplicativo. Para começar, use File-&gt;New para criar uma página chamada MyShoppingCart. aspx.

Observe que não estamos escolhendo o nome ShoppingCart. aspx.

Nosso banco de dados contém uma tabela chamada "ShoppingCart". Quando geramos um Modelo de Dados de Entidade uma classe foi criada para cada tabela no banco de dados. Portanto, o Modelo de Dados de Entidade gerou uma classe de entidade chamada "ShoppingCart". Poderíamos editar o modelo para que possamos usar esse nome para a implementação do carrinho de compras ou estendê-lo para nossas necessidades, mas, em vez disso, optaremos por simplesmente selecionar um nome que evitará o conflito.

Também vale a pena observar que iremos criar um carrinho de compras simples e inserir a lógica do carrinho de compras com a exibição do carrinho de compras. Também poderemos optar por implementar nosso carrinho de compras em uma camada comercial completamente separada.

> [!div class="step-by-step"]
> [Anterior](tailspin-spyworks-part-3.md)
> [Próximo](tailspin-spyworks-part-5.md)
