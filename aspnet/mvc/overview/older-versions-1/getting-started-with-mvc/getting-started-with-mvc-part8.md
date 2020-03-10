---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Adicionando uma coluna ao modelo | Microsoft Docs
author: shanselman
description: Este é um tutorial principiante que apresenta os fundamentos do ASP.NET MVC. Crie um aplicativo Web simples que lê e grava de um banco de dados.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 1cf092c3db3959d6f47006f1be2ba82833c5dc06
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78543578"
---
# <a name="adding-a-column-to-the-model"></a>Adicionar uma coluna ao modelo

por [Scott Hanselman](https://github.com/shanselman)

> Este é um tutorial principiante que apresenta os fundamentos do ASP.NET MVC. Você criará um aplicativo Web simples que lê e grava de um banco de dados. Visite o [ASP.NET MVC Learning Center](../../../index.md) para encontrar outros tutoriais e exemplos do ASP.NET MVC.

Nesta seção, vamos examinar como podemos fazer alterações no esquema do nosso banco de dados e lidar com as alterações em nosso aplicativo.

Vamos adicionar uma coluna de "classificação" à tabela de filmes. Volte para o IDE e clique no Gerenciador de Banco de Dados. Clique com o botão direito do mouse na tabela filme e selecione Abrir definição de tabela.

Adicione uma coluna de "classificação", como mostrado abaixo. Como não temos nenhuma classificação agora, a coluna pode permitir valores nulos. Clique em Salvar.

[![editar a tabela de filmes](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)

Em seguida, retorne ao Gerenciador de Soluções e abra o arquivo Movies. edmx (que está na pasta \Models). Clique com o botão direito do mouse na superfície de design (a área branca) e selecione Atualizar modelo no banco de dados.

[Filmes de ![-Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)

Isso abrirá o "Assistente de atualização". Clique na guia atualizar dentro dela e clique em concluir. Nossa classe de modelo de filme será atualizada com a nova coluna.

![Assistente de atualização (2)](getting-started-with-mvc-part8/_static/image5.png)

Depois de clicar em concluir, você poderá ver que a nova coluna de classificação foi adicionada à entidade de filme em nosso modelo.

[![entidade de filme](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)

Adicionamos uma coluna no modelo de banco de dados, mas as exibições não sabem sobre ela.

## <a name="update-views-with-model-changes"></a>Atualizar exibições com alterações de modelo

Há algumas maneiras de atualizar nossos modelos de exibição para refletir a nova coluna de classificação. Como criamos essas exibições gerando-as por meio da caixa de diálogo Adicionar exibição, poderíamos excluí-las e recriá-las novamente. No entanto, normalmente as pessoas já terão feito modificações em seus modelos de exibição da geração com Scaffold inicial e desejarão adicionar ou excluir campos manualmente, exatamente como fizemos com o campo ID para Create.

Abra o modelo \Views\Movies\Index.aspx e adicione uma classificação de&gt;&lt;&lt;/th&gt; no início da tabela de filmes. Adicionei o meu após o gênero. Em seguida, na mesma posição de coluna, mas inferior, adicione uma linha para gerar a nova classificação.

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

Nosso modelo index. aspx final terá a seguinte aparência:

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

Em seguida, vamos abrir o modelo \Views\Movies\Create.aspx e adicionar um rótulo e uma caixa de texto para nossa nova propriedade de classificação:

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

Nosso modelo Create. aspx final será semelhante a este, e vamos alterar o título do navegador e o secundário &lt;H2&gt; título para algo como "criar um filme" enquanto estamos aqui!

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

Execute seu aplicativo e agora você tem um novo campo no banco de dados que foi adicionado à página criar. Adicione um novo filme-desta vez com uma classificação-e clique em criar.

[![criar um filme-Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)

Depois de clicar em criar, você será enviado para a página de índice em que o novo filme é listado com a nova coluna de classificação no banco de dados

[Lista de filmes ![-Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)

Este tutorial básico começou a tornar os controladores, associando-os a exibições e passando dados embutidos em código. Em seguida, criamos e projetamos um banco de dados e colocamos algum dado nele. Recuperamos os dados do banco e os exibimos em uma tabela HTML. Em seguida, adicionamos um formulário de criação que permite ao usuário adicionar dados ao próprio banco de dado de dentro do aplicativo Web. Adicionamos validação e, em seguida, fizemos a validação usando JavaScript no lado do cliente. Por fim, alteramos o banco de dados para incluir uma nova coluna de data e, em seguida, atualizamos nossas duas páginas para criar e exibir esses novos dados.

Agora recomendo que você passe para nosso tutorial de nível intermediário "loja de[música MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)", bem como os vários vídeos e recursos em [https://asp.net/mvc](https://asp.net/mvc) para aprender ainda mais sobre o ASP.NET MVC!

Aproveite!

- Scott Hanselman- [http://hanselman.com](http://hanselman.com) e [@shanselman](http://twitter.com/shanselman) no Twitter.

> [!div class="step-by-step"]
> [Anterior](getting-started-with-mvc-part7.md)
