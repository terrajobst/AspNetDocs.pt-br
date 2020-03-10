---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: 'Parte 2: camada de acesso a dados | Microsoft Docs'
author: JoeStagner
description: Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo Tailspin Spyworks. A parte 2 aborda a adição da camada de acesso a dados.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: 342d2c54dfba5d052570e890f85dcf9739f9884f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78573244"
---
# <a name="part-2-data-access-layer"></a>Parte 2: camada de acesso a dados

por [Joe Stagner](https://github.com/JoeStagner)

> A Tailspin Spyworks demonstra como é extremamente simples criar aplicativos poderosos e escalonáveis para a plataforma .NET. Ele mostra como usar os ótimos novos recursos do ASP.NET 4 para criar uma loja online, incluindo compras, check-out e administração.
> 
> Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo Tailspin Spyworks. A parte 2 aborda a adição da camada de acesso a dados.

## <a id="_Toc260221668"></a>Adicionando a camada de acesso a dados

Nosso aplicativo de comércio eletrônico dependerá de dois bancos de dados.

Para obter informações do cliente, usaremos o banco de dados de associação do ASP.NET padrão. Para nosso carrinho de compras e catálogo de produtos, implementaremos um banco de dados do SQL Express da seguinte maneira.

![](tailspin-spyworks-part-2/_static/image1.jpg)

Depois de criar o banco de dados (Commerce. MDF) na pasta do aplicativo\_data, podemos continuar criando nossa camada de acesso a dados usando o Entity Framework .NET.

Criaremos uma pasta denominada "data\_Access" e as clicaremos com o botão direito do mouse nessa pasta e selecionaremos "Adicionar novo item".

No item "modelos instalados" e, em seguida, selecione "ADO.NET Modelo de Dados de Entidade" Insira EDM\_Commerce. edmx como o nome e clique no botão "Adicionar".

![](tailspin-spyworks-part-2/_static/image2.jpg)

Escolha "gerar do banco de dados".

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

Salve e compile.

Agora estamos prontos para adicionar nosso primeiro recurso – um menu de categorias de produtos.

> [!div class="step-by-step"]
> [Anterior](tailspin-spyworks-part-1.md)
> [Próximo](tailspin-spyworks-part-3.md)
