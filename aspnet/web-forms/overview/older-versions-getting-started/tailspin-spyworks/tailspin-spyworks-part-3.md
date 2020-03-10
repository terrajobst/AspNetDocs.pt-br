---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: 'Parte 3: menu de layout e categoria | Microsoft Docs'
author: JoeStagner
description: Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo Tailspin Spyworks. A parte 3 aborda a adição de layout e um menu de categoria.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: a223b97fd362ecf73ecde431e141021c1dcc6a6d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78639114"
---
# <a name="part-3-layout-and-category-menu"></a>Parte 3: menu de layout e categoria

por [Joe Stagner](https://github.com/JoeStagner)

> A Tailspin Spyworks demonstra como é extremamente simples criar aplicativos poderosos e escalonáveis para a plataforma .NET. Ele mostra como usar os ótimos novos recursos do ASP.NET 4 para criar uma loja online, incluindo compras, check-out e administração.
> 
> Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo Tailspin Spyworks. A parte 3 aborda a adição de layout e um menu de categoria.

## <a id="_Toc260221669"></a>Adicionando algum layout e um menu de categoria

Em nossa página mestra do site, adicionaremos uma div à coluna do lado esquerdo que conterá nosso menu de categoria do produto.

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

Observe que o alinhamento desejado e outras formatações serão fornecidas pela classe CSS que adicionamos ao nosso arquivo Style. css.

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

O menu categoria do produto será criado dinamicamente em tempo de execução consultando o banco de dados de comércio para obter categorias de produtos existentes e criando os itens de menu e links correspondentes.

Para fazer isso, usaremos dois do ASP. Os poderosos controles de dados do NET. O controle "fonte de dados de entidade" e o controle "ListView".

![](tailspin-spyworks-part-3/_static/image1.jpg)

Vamos mudar para "modo de exibição de design" e usar os auxiliares para configurar nossos controles.

![](tailspin-spyworks-part-3/_static/image2.jpg)

Vamos definir a propriedade de ID de EntityDataSource para o menu de\_de categoria da EDS\_e clicar em "Configurar fonte de dados".

![](tailspin-spyworks-part-3/_static/image3.jpg)

Selecione a conexão CommerceEntities que foi criada para nós quando criamos o modelo de fonte de dados de entidade para nosso banco de dado de comércio e clique em "Avançar".

![](tailspin-spyworks-part-3/_static/image4.jpg)

Selecione o nome do conjunto de entidades "categorias" e deixe o restante das opções como padrão. Clique em "Concluir".

Agora, vamos definir a propriedade ID da instância de controle ListView que colocamos em nossa página para ListView\_ProductsMenu e ativar seu auxiliar.

![](tailspin-spyworks-part-3/_static/image5.jpg)

Embora possamos usar as opções de controle para formatar a exibição e a formatação do item de dados, nossa criação de menu exigirá apenas marcação simples, portanto, entraremos no código na exibição da fonte.

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

Observe a instrução "eval": &lt;% # Eval ("CategoryName")%&gt;

A sintaxe ASP.NET &lt;% #%&gt; é uma convenção abreviada que instrui o tempo de execução para executar o que estiver contido em e gerar os resultados "in line".

A instrução Eval ("CategoryName") instrui isso, para a entrada atual na coleção associada de itens de dados, busque o valor dos nomes de item de modelo de entidade "CategoryName". Essa é uma sintaxe concisa para um recurso muito potente.

Vamos executar o aplicativo agora.

![](tailspin-spyworks-part-3/_static/image6.jpg)

Observe que nosso menu de categoria de produto agora é exibido e, quando passamos o mouse sobre um dos itens de menu de categoria, podemos ver os pontos de link do item de menu para uma página que ainda implementamos ProductList. aspx e criamos um argumento de cadeia de caracteres de consulta dinâmica que contém o  ID da categoria.

> [!div class="step-by-step"]
> [Anterior](tailspin-spyworks-part-2.md)
> [Próximo](tailspin-spyworks-part-4.md)
