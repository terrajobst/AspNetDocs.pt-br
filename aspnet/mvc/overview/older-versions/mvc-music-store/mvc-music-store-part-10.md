---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: 'Parte 10: atualizações finais de navegação e design de site, conclusão | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo de loja de música MVC do ASP.NET. A parte 10 aborda as atualizações finais para navegação e S...
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: f701d1fbabc3e1a97c3750d00e96bf8dba1105cd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539364"
---
# <a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a>Parte 10: atualizações finais de navegação e design de site, conclusão

por [Jon Galloway](https://github.com/jongalloway)

> A loja de música MVC é um aplicativo de tutorial que apresenta e explica passo a passo como usar o ASP.NET MVC e o Visual Studio para desenvolvimento para a Web.  
>   
> A loja de música MVC é uma implementação de armazenamento de exemplo leve que vende os álbuns de música online e implementa a administração básica de site, a entrada do usuário e a funcionalidade do carrinho de compras.  
>   
> Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo de loja de música MVC do ASP.NET. A parte 10 aborda as atualizações finais de navegação e design de site, conclusão.

Concluímos toda a funcionalidade principal para nosso site, mas ainda temos alguns recursos para adicionar à navegação do site, ao home page e à página de navegação da loja.

## <a name="creating-the-shopping-cart-summary-partial-view"></a>Criando a exibição parcial do resumo do carrinho de compras

Queremos expor o número de itens no carrinho de compras do usuário em todo o site.

![](mvc-music-store-part-10/_static/image1.png)

Podemos implementar isso facilmente criando uma exibição parcial que é adicionada ao nosso site. master.

Como mostrado anteriormente, o controlador ShoppingCart inclui um método de ação CartSummary que retorna uma exibição parcial:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

Para criar a exibição parcial de CartSummary, clique com o botão direito do mouse na pasta views/ShoppingCart e selecione Add View. Nomeie a exibição CartSummary e marque a caixa de seleção "criar uma exibição parcial", conforme mostrado abaixo.

![](mvc-music-store-part-10/_static/image2.png)

A exibição parcial CartSummary é realmente simples – é apenas um link para a exibição do índice ShoppingCart que mostra o número de itens no carrinho. O código completo para CartSummary. cshtml é o seguinte:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

Podemos incluir uma exibição parcial em qualquer página no site, incluindo o site mestre, usando o método html. RenderAction. Renderingaction exige que especifiquemos o nome da ação ("CartSummary") e o nome do controlador ("ShoppingCart") como mostrado abaixo.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

Antes de adicioná-lo ao layout do site, também criaremos o menu de gênero para que possamos fazer todas as nossas atualizações de site. Master ao mesmo tempo.

## <a name="creating-the-genre-menu-partial-view"></a>Criando a exibição parcial do menu de gênero

Podemos tornar muito mais fácil para nossos usuários navegar pela loja adicionando um menu de gênero que lista todos os gêneros disponíveis em nossa loja.

![](mvc-music-store-part-10/_static/image3.png)

Seguiremos as mesmas etapas também criam uma exibição parcial GenreMenu e, em seguida, podemos adicioná-las ao mestre do site. Primeiro, adicione a seguinte ação do controlador GenreMenu ao StoreController:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

Essa ação retorna uma lista de gêneros que serão exibidos pela exibição parcial, que criaremos a seguir.

*Observação: adicionamos o atributo [ChildActionOnly] a essa ação do controlador, que indica que só queremos que essa ação seja usada em uma exibição parcial. Esse atributo impedirá que a ação do controlador seja executada navegando até/Store/GenreMenu. Isso não é necessário para exibições parciais, mas é uma boa prática, pois queremos garantir que nossas ações do controlador sejam usadas como pretendida. Também estamos retornando PartialView em vez de View, que permite ao mecanismo de exibição saber que ele não deve usar o layout dessa exibição, pois está sendo incluído em outras exibições.*

Clique com o botão direito do mouse na ação do controlador GenreMenu e crie uma exibição parcial chamada GenreMenu, que é fortemente tipada usando a classe de dados de exibição de gênero, conforme mostrado abaixo.

![](mvc-music-store-part-10/_static/image4.png)

Atualize o código de exibição para a exibição parcial de GenreMenu para exibir os itens usando uma lista não ordenada da seguinte maneira.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a>Atualizando layout do site para exibir nossas exibições parciais

Podemos adicionar nossas exibições parciais ao layout do site (/Views/Shared/\_layout. cshtml) chamando HTML. RenderAction (). Vamos adicioná-los no, bem como algumas marcações adicionais para exibi-los, conforme mostrado abaixo:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

Agora, quando executarmos o aplicativo, veremos o gênero na área de navegação à esquerda e o resumo do carrinho na parte superior.

## <a name="update-to-the-store-browse-page"></a>Atualizar para a página de navegação da loja

A página de navegação da loja é funcional, mas não parece muito boa. Podemos atualizar a página para mostrar os álbuns em um layout melhor atualizando o código de exibição (encontrado em/Views/Store/Browse.cshtml) da seguinte maneira:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

Aqui estamos fazendo uso de URL. Action em vez de HTML. ActionLink para que possamos aplicar formatação especial ao link para incluir a arte-final do álbum.

*Observação: estamos exibindo uma capa de álbum genérico para esses álbuns. Essas informações são armazenadas no banco de dados e são editáveis por meio do Gerenciador da loja. Você é bem-vindo a adicionar seu próprio trabalho artístico.*

Agora, quando navegarmos até um gênero, veremos os álbuns mostrados em uma grade com a arte-final do álbum.

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a>Atualizando a Home Page para mostrar os principais álbuns de vendas

Queremos apresentar nossos principais álbuns de vendas na home page para aumentar as vendas. Faremos algumas atualizações em nosso HomeController para lidar com isso e também adicionaremos alguns elementos gráficos adicionais.

Primeiro, adicionaremos uma propriedade de navegação à nossa classe de álbum para que o EntityFramework saiba que está associado. As últimas linhas da nossa classe de **álbum** agora devem ser assim:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

*Observação: isso exigirá a adição de uma instrução using para trazer o namespace System. Collections. Generic.*

Primeiro, adicionaremos um campo storeDB e o MvcMusicStore. Models usando instruções, como em nossos outros controladores. Em seguida, adicionaremos o seguinte método ao HomeController, que consulta nosso banco de dados para localizar os principais álbuns de vendas de acordo com OrderDetails.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

Esse é um método particular, pois não queremos disponibilizá-lo como uma ação de controlador. Estamos incluindo isso no HomeController para simplificar, mas você é incentivado a mover sua lógica de negócios para classes de serviço separadas, conforme apropriado.

Com isso em vigor, podemos atualizar a ação do controlador de índice para consultar os 5 principais álbuns de vendas e retorná-los à exibição.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

O código completo para o HomeController atualizado é mostrado abaixo.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

Por fim, precisaremos atualizar nossa exibição de índice doméstico para que ela possa exibir uma lista de álbuns atualizando o tipo de modelo e adicionando a lista de álbuns à parte inferior. Levaremos essa oportunidade para adicionar também um título e uma seção de promoção à página.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

Agora, quando executarmos o aplicativo, veremos os home page atualizados com os principais álbuns de vendas e nossa mensagem promocional.

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a>Conclusão

Vimos que o ASP.NET MVC facilita a criação de um site sofisticado com acesso ao banco de dados, associação, AJAX, etc. muito rapidamente. Espero que este tutorial tenha lhe dado as ferramentas necessárias para começar a criar seus próprios aplicativos MVC do ASP.NET!

> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-9.md)
