---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: 'Parte 7: adicionando recursos | Microsoft Docs'
author: JoeStagner
description: Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo Tailspin Spyworks. A parte 7 adiciona recursos adicionais, como a conta Revie...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: ffd2b862c727db9572c272b7b21bcc33c822fffa
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78641991"
---
# <a name="part-7-adding-features"></a>Parte 7: adicionando recursos

por [Joe Stagner](https://github.com/JoeStagner)

> A Tailspin Spyworks demonstra como é extremamente simples criar aplicativos poderosos e escalonáveis para a plataforma .NET. Ele mostra como usar os ótimos novos recursos do ASP.NET 4 para criar uma loja online, incluindo compras, check-out e administração.
> 
> Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo Tailspin Spyworks. A parte 7 adiciona recursos adicionais, como revisão de conta, revisões de produtos e controles de usuário "itens populares" e "também comprados".

## <a id="_Toc260221673"></a>Adicionando recursos

Embora os usuários possam navegar em nosso catálogo, inserir itens em seu carrinho de compras e concluir o processo de check-out, há vários recursos de suporte que incluiremos para melhorar nosso site.

1. Revisão da conta (listar pedidos colocados e exibir detalhes.)
2. Adicione um conteúdo específico de contexto à página inicial.
3. Adicione um recurso para permitir que os usuários examinem os produtos no catálogo.
4. Crie um controle de usuário para exibir itens populares e coloque esse controle na página inicial.
5. Crie um controle de usuário "também adquirido" e adicione-o à página de detalhes do produto.
6. Adicione uma página de contato.
7. Adicione uma página About.
8. Erro global

## <a id="_Toc260221674"></a>Revisão da conta

Na pasta "Account", crie duas páginas. aspx, uma denominada OrderList. aspx e a outra chamada OrderDetails. aspx

OrderList. aspx aproveitará os controles GridView e EntityDataSource da mesma forma que antes.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

A EntityDataSource seleciona registros da tabela Orders filtrados no nome de usuário (consulte o WhereParameter) que definimos em uma variável de sessão quando o usuário faz logon.

Observe também esses parâmetros no HyperlinkField do GridView:

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

Eles especificam o link para a exibição detalhes do pedido para cada produto especificando o campo OrderID como um parâmetro QueryString para a página OrderDetails. aspx.

## <a id="_Toc260221675"></a>OrderDetails. aspx

Usaremos um controle de EntityDataSource para acessar os pedidos e um FormView para exibir os dados do pedido e outra EntityDataSource com um GridView para exibir todos os itens de linha da ordem.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

No arquivo code-behind (OrderDetails.aspx.cs), temos dois pequenos bits de manutenção.

Primeiro, precisamos garantir que OrderDetails sempre obtenha um OrderId.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

Também precisamos calcular e exibir o total do pedido dos itens de linha.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>A Home Page

Vamos adicionar um conteúdo estático à página default. aspx.

Primeiro, criarei uma pasta "content" e dentro dela uma pasta images (e incluirei uma imagem a ser usada na home page.)

No espaço reservado inferior da página default. aspx, adicione a marcação a seguir.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>Análises de produtos

Primeiro, adicionaremos um botão com um link para um formulário que podemos usar para inserir uma revisão do produto.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

Observe que estamos passando o ProductID na cadeia de caracteres de consulta

Em seguida, vamos adicionar a página chamada ReviewAdd. aspx

Esta página usará o kit de ferramentas de controle AJAX ASP.NET. Se ainda não tiver feito isso, você poderá baixá-lo em [DevExpress](http://devexpress.com/act) e há diretrizes sobre como configurar o kit de ferramentas para uso com o Visual Studio aqui [https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).

No modo de design, arraste controles e validadores da caixa de ferramentas e crie um formulário como o mostrado abaixo.

![](tailspin-spyworks-part-7/_static/image2.jpg)

A marcação terá uma aparência semelhante a esta.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

Agora que podemos inserir as revisões, vamos exibir essas revisões na página do produto.

Adicione essa marcação à página ProductDetails. aspx.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

Executar nosso aplicativo agora e navegar até um produto mostra as informações do produto, incluindo as revisões do cliente.

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>Controle de itens populares (criando controles de usuário)

Para aumentar as vendas em seu site, adicionaremos alguns recursos à "venda sugerida" de produtos populares ou relacionados.

O primeiro desses recursos será uma lista do produto mais popular em nosso catálogo de produtos.

Criaremos um "controle de usuário" para exibir os principais itens de vendas na home page de nosso aplicativo. Como isso será um controle, podemos usá-lo em qualquer página simplesmente arrastando e soltando o controle no designer do Visual Studio em qualquer página que desejar.

No Gerenciador de soluções do Visual Studio, clique com o botão direito do mouse no nome da solução e crie um novo diretório chamado "controles". Embora não seja necessário fazer isso, ajudaremos a manter nosso projeto organizado pela criação de todos os controles de usuário no diretório "Controls".

Clique com o botão direito do mouse na pasta controles e escolha "novo item":

![](tailspin-spyworks-part-7/_static/image4.jpg)

Especifique um nome para nosso controle de "PopularItems". Observe que a extensão de arquivo para controles de usuário é. ascx não. aspx.

Nosso controle de usuário itens populares será definido da seguinte maneira.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

Aqui, estamos usando um método que ainda não usamos neste aplicativo. Estamos usando o controle Repeater e, em vez de usar um controle da fonte de dados, estamos associando o controle Repeater aos resultados de uma consulta LINQ to Entities.

No code-behind do nosso controle, fazemos isso da seguinte maneira.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

Observe também essa linha importante na parte superior da marcação do nosso controle.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

Como os itens mais populares não serão alterados em uma base de minuto a minuto, podemos adicionar uma diretiva aching para melhorar o desempenho de nosso aplicativo. Essa diretiva fará com que o código de controles seja executado somente quando a saída armazenada em cache do controle expirar. Caso contrário, a versão em cache da saída do controle será usada.

Agora, tudo o que precisamos fazer é incluir nosso novo controle em nossa página default. aspx.

Use o recurso arrastar e soltar para colocar uma instância do controle na coluna abrir do formulário padrão.

![](tailspin-spyworks-part-7/_static/image5.jpg)

Agora, quando executamos nosso aplicativo, o home page exibe os itens mais populares.

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>Controle "também adquirido" (controles de usuário com parâmetros)

O segundo controle de usuário que criaremos fará a venda sugerida para o próximo nível adicionando a especificidade do contexto.

A lógica para calcular os itens "também comprados" principais não é trivial.

Nosso controle "também adquirido" selecionará os registros de OrderDetails (anteriormente adquiridos) para o ProductID selecionado no momento e pegará o OrderID para cada ordem exclusiva que for encontrada.

Em seguida, selecionaremos os produtos de todas essas ordens e Somaremos as quantidades compradas. Classificaremos os produtos por essa quantidade Sum e exibiremos os cinco primeiros itens.

Dada a complexidade dessa lógica, implementaremos esse algoritmo como um procedimento armazenado.

O T-SQL para o procedimento armazenado é o seguinte.

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

Observe que esse procedimento armazenado (SelectPurchasedWithProducts) existia no banco de dados quando o incluímos em nosso aplicativo e quando geramos o Modelo de Dados de Entidade que especificamos que, além das tabelas e exibições que precisávamos, o Modelo de Dados de Entidade deve incluir esse procedimento armazenado.

Para acessar o procedimento armazenado do Modelo de Dados de Entidade precisamos importar a função.

Clique duas vezes no Modelo de Dados de Entidade no Gerenciador de soluções para abri-lo no designer e abra o navegador de modelos, clique com o botão direito do mouse no designer e selecione "Adicionar importação de função".

![](tailspin-spyworks-part-7/_static/image1.png)

Ao fazer isso, você abrirá essa caixa de diálogo.

![](tailspin-spyworks-part-7/_static/image2.png)

Preencha os campos como você vê acima, selecionando o "SelectPurchasedWithProducts" e use o nome do procedimento para o nome da nossa função importada.

Clique em "OK".

Tendo feito isso, podemos simplesmente programar em relação ao procedimento armazenado, pois podemos qualquer outro item no modelo.

Portanto, em nossa pasta "Controls", crie um novo controle de usuário chamado AlsoPurchased. ascx.

A marcação para esse controle parecerá muito familiar ao controle PopularItems.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

A diferença notável é que não estão armazenando em cache a saída, uma vez que o item a ser renderizado será diferente pelo produto.

O ProductId será uma "Propriedade" para o controle.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

No manipulador de eventos PreRender do controle, eed fazer três coisas.

1. Verifique se o ProductID está definido.
2. Veja se há produtos que foram comprados com o atual.
3. Gere alguns itens conforme determinado em #2.

Observe como é fácil chamar o procedimento armazenado por meio do modelo.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

Depois de determinar que há "também adquirido", podemos simplesmente associar o repetidor aos resultados retornados pela consulta.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

Se não houver nenhum item "também adquirido", simplesmente exibiremos outros itens populares de nosso catálogo.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

Para exibir os itens "também adquiridos", abra a página ProductDetails. aspx e arraste o controle AlsoPurchased do Gerenciador de soluções para que ele apareça nessa posição na marcação.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

Isso criará uma referência ao controle na parte superior da página ProductDetails.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

Como o controle de usuário AlsoPurchased requer um número ProductId, definiremos a propriedade ProductID de nosso controle usando uma instrução eval em relação ao item do modelo de dados atual da página.

![](tailspin-spyworks-part-7/_static/image3.png)

Quando criamos e executamos agora e navegamos para um produto, vemos os itens "também adquiridos".

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> [Anterior](tailspin-spyworks-part-6.md)
> [Próximo](tailspin-spyworks-part-8.md)
