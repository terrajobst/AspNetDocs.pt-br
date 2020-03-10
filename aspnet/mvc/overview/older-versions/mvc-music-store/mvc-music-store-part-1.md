---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 'Parte 1: visão geral e arquivo-> novo projeto | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo de loja de música MVC do ASP.NET. A parte 1 abrange visão geral e arquivo-> novo projeto.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: 48428ff4ab5888253ed93ac41e79006eec823ad2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559979"
---
# <a name="part-1-overview-and-file-new-project"></a>Parte 1: visão geral e arquivo-> novo projeto

por [Jon Galloway](https://github.com/jongalloway)

> A loja de música MVC é um aplicativo de tutorial que apresenta e explica passo a passo como usar o ASP.NET MVC e o Visual Studio para desenvolvimento para a Web.  
>   
> A loja de música MVC é uma implementação de armazenamento de exemplo leve que vende os álbuns de música online e implementa a administração básica de site, a entrada do usuário e a funcionalidade do carrinho de compras.  
>   
> Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo de loja de música MVC do ASP.NET. A parte 1 abrange visão geral e arquivo-&gt;novo projeto.

## <a name="overview"></a>Visão geral

A loja de música MVC é um aplicativo de tutorial que apresenta e explica passo a passo como usar o ASP.NET MVC e o Visual Web Developer para desenvolvimento para a Web. Começaremos lentamente, então a experiência de desenvolvimento para a Web de nível principiante está ok.

O aplicativo que iremos criar é uma loja de música simples. Há três partes principais para o aplicativo: compras, check-out e administração.

![](mvc-music-store-part-1/_static/image1.jpg)

Os visitantes podem procurar álbuns por gênero:

![](mvc-music-store-part-1/_static/image2.jpg)

Eles podem exibir um único álbum e adicioná-lo ao seu carrinho:

![](mvc-music-store-part-1/_static/image3.jpg)

Eles podem revisar seus carrinhos, removendo os itens que não desejam mais:

![](mvc-music-store-part-1/_static/image4.jpg)

Prosseguir com o check-out solicitará que eles façam logon ou registrem-se para uma conta de usuário.

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

Depois de criar uma conta, eles podem concluir o pedido preenchendo as informações de envio e pagamento. Para simplificar as coisas, estamos executando uma promoção incrível: tudo está livre se inserir o código de promoção "gratuito"!

![](mvc-music-store-part-1/_static/image5.jpg)

Após a ordenação, eles veem uma tela de confirmação simples:

![](mvc-music-store-part-1/_static/image6.jpg)

Além das páginas voltadas para o cliente, também criaremos uma seção de administrador que mostra uma lista de álbuns dos quais os administradores podem criar, editar e excluir álbuns:

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a>1. arquivo-&gt; novo projeto

### <a name="installing-the-software"></a>Instalando o software

Este tutorial começará criando um novo projeto do ASP.NET MVC 3 usando o Visual Web Developer 2010 Express gratuito (que é gratuito) e, em seguida, adicionaremos os recursos de forma incremental para criar um aplicativo funcional completo. Ao longo do caminho, abordaremos o acesso ao banco de dados, os cenários de lançamento de formulário, a validação, a utilização de páginas mestras para layout de página consistente, usando AJAX para atualizações de página e validação, logon de usuário e muito mais.

Você pode acompanhar o passo a passo ou pode baixar o aplicativo concluído do [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).

Você pode usar o Visual Studio 2010 SP1 ou o Visual Web Developer 2010 Express SP1 (uma versão gratuita do Visual Studio 2010) para compilar o aplicativo. Vamos usar o SQL Server Compact (também gratuito) para hospedar o banco de dados. Antes de começar, verifique se você instalou os pré-requisitos listados abaixo.

- [Pré-requisitos do Visual Studio Web Developer Express SP1]
- [Atualização de ferramentas do MVC 3 do ASP.NET]
- [SQL Server Compact 4,0]-incluindo suporte para tempo de execução e ferramentas

### <a name="creating-a-new-aspnet-mvc-3-project"></a>Criando um novo projeto do ASP.NET MVC 3

Vamos começar selecionando "novo projeto" no menu arquivo no Visual Web Developer. Isso abre a caixa de diálogo novo projeto.

![](mvc-music-store-part-1/_static/image5.png)

Selecionaremos o grupo modelos C# da Web do Visual&gt; à esquerda e, em seguida, escolheremos o modelo "aplicativo Web ASP.NET MVC 3" na coluna central. Nomeie o projeto MvcMusicStore e pressione o botão OK.

![](mvc-music-store-part-1/_static/image8.jpg)

Isso exibirá uma caixa de diálogo secundária, que nos permite fazer algumas configurações específicas do MVC para o nosso projeto. Selecione o seguinte:

Modelo de projeto-selecione vazio

Exibir mecanismo-selecione Razor

Usar marcação semântica HTML5-verificada

Verifique se as configurações são as mostradas abaixo e pressione o botão OK.

![](mvc-music-store-part-1/_static/image9.jpg)

Isso criará nosso projeto. Vamos dar uma olhada nas pastas que foram adicionadas ao nosso aplicativo na Gerenciador de Soluções no lado direito.

![](mvc-music-store-part-1/_static/image10.jpg)

O modelo MVC 3 vazio não está completamente vazio – ele adiciona uma estrutura de pastas básica:

![](mvc-music-store-part-1/_static/image6.png)

O ASP.NET MVC usa algumas convenções de nomenclatura básicas para nomes de pastas:

| **Pasta** | **Finalidade** |
| --- | --- |
| **/Controllers** | Os controladores respondem à entrada do navegador, decidem o que fazer com ele e retornam a resposta ao usuário. |
| **/Views** | Os modos de exibição mantêm nossos modelos de interface do usuário |
| **/Models** | Modelos mantêm e manipulam dados |
| **/Content** | Essa pasta contém nossas imagens, CSS e qualquer outro conteúdo estático |
| **/Scripts** | Essa pasta contém nossos arquivos JavaScript |

Essas pastas são incluídas mesmo em um aplicativo ASP.NET MVC vazio, pois a estrutura MVC do ASP.NET, por padrão, usa uma abordagem "Convenção sobre configuração" e faz algumas suposições padrão com base nas convenções de nomenclatura de pastas. Por exemplo, controladores procuram exibições na pasta views por padrão sem a necessidade de especificar explicitamente isso em seu código. Aprimorar as convenções padrão reduz a quantidade de código que você precisa escrever e também pode facilitar para outros desenvolvedores a compreensão do seu projeto. Explicaremos essas convenções mais à medida que criarmos nosso aplicativo.

> [!div class="step-by-step"]
> [Próximo](mvc-music-store-part-2.md)
