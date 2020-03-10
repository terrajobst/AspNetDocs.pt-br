---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: 'Parte 1: arquivo-> novo projeto | Microsoft Docs'
author: JoeStagner
description: Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo Tailspin Spyworks. A parte 1 abrange visão geral e arquivo/novo projeto.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: 05a3ace3d8fef9c1f3593f7948e42b4725d70134
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78636125"
---
# <a name="part-1-file--new-project"></a>Parte 1: arquivo-> novo projeto

por [Joe Stagner](https://github.com/JoeStagner)

> A Tailspin Spyworks demonstra como é extremamente simples criar aplicativos poderosos e escalonáveis para a plataforma .NET. Ele mostra como usar os ótimos novos recursos do ASP.NET 4 para criar uma loja online, incluindo compras, check-out e administração.
> 
> Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo Tailspin Spyworks. A parte 1 abrange visão geral e arquivo/novo projeto.

## <a id="_Toc260221666"></a>Sobre

Este tutorial é uma introdução aos WebForms do ASP.NET. Começaremos lentamente, então a experiência de desenvolvimento para a Web de nível principiante está ok.

O aplicativo que iremos criar é um armazenamento online simples.

![](tailspin-spyworks-part-1/_static/image1.jpg)

Os visitantes podem procurar produtos por categoria:

![](tailspin-spyworks-part-1/_static/image2.jpg)

Eles podem exibir um único produto e adicioná-lo ao seu carrinho:

![](tailspin-spyworks-part-1/_static/image3.jpg)

Eles podem revisar seus carrinhos, removendo os itens que não desejam mais:

![](tailspin-spyworks-part-1/_static/image4.jpg)

Prosseguir para o check-out solicitará a eles

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

Após a ordenação, eles veem uma tela de confirmação simples:

![](tailspin-spyworks-part-1/_static/image7.jpg)

Vamos começar criando um novo projeto WebForms do ASP.NET no Visual Studio 2010 e, incrementalmente, adicionaremos recursos para criar um aplicativo funcional completo. Ao longo do caminho, abordaremos o acesso ao banco de dados, exibições de lista e de grade, páginas de atualização, validação de dados, uso de páginas mestras para layout de página consistente, AJAX, validação, associação de usuário e muito mais.

Você pode acompanhar o passo a passo ou pode baixar o aplicativo concluído de [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)

Você pode usar o Visual Studio 2010 ou a versão gratuita do Visual Web Developer 2010 do [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/). Para compilar o aplicativo, você pode usar SQL Server ou o SQL Server Express gratuito para hospedar o banco de dados.

## <a id="_Toc260221667"></a>Arquivo/novo projeto

Vamos começar selecionando o novo projeto no menu arquivo no Visual Studio. Isso abre a caixa de diálogo novo projeto.

![](tailspin-spyworks-part-1/_static/image8.jpg)

Selecionaremos o grupo de C# modelos visuais/Web à esquerda e, em seguida, escolheremos o modelo "aplicativo Web ASP.net" na coluna central. Nomeie o projeto TailspinSpyworks e pressione o botão OK.

![](tailspin-spyworks-part-1/_static/image9.jpg)

Isso criará nosso projeto. Vamos dar uma olhada nas pastas que estão incluídas em nosso aplicativo na Gerenciador de Soluções no lado direito.

![](tailspin-spyworks-part-1/_static/image10.jpg)

A solução vazia não está completamente vazia – ela adiciona uma estrutura de pastas básica:

![](tailspin-spyworks-part-1/_static/image1.png)

Observe as convenções implementadas pelo modelo de projeto padrão ASP.NET 4.

- A pasta "Account" implementa uma interface de usuário básica para ASP. Subsistema de associação da rede.
- A pasta "scripts" serve como o repositório para arquivos JavaScript do lado do cliente e os arquivos principais do jQuery. js são disponibilizados por padrão.
- A pasta "estilos" é usada para organizar nossos elementos visuais de site (folhas de estilo CSS)

Quando pressionamos F5 para executar nosso aplicativo e renderizamos a página default. aspx, vemos o seguinte.

![](tailspin-spyworks-part-1/_static/image11.jpg)

Nosso primeiro aprimoramento de aplicativo será substituir o arquivo Style. CSS do modelo WebForms padrão pelas classes CSS e arquivos de imagem associados que processarão o Visual Asthetics que desejamos para nosso aplicativo Tailspin Spyworks.

Depois de fazer isso, nossa página default. aspx é renderizada dessa forma.

![](tailspin-spyworks-part-1/_static/image12.jpg)

Observe os links de imagem na parte superior direita da página e os itens de menu que foram adicionados à página mestra. Somente os links "entrar" e "conta" apontam para páginas que existem (geradas pelo modelo padrão) e o restante das páginas que iremos implementar à medida que criamos nosso aplicativo.

Também vamos realocar a página mestra para o diretório de estilos. Embora essa seja apenas uma preferência, isso pode tornar as coisas um pouco mais fáceis se decidirmos tornar nosso aplicativo "em pele" no futuro.

Depois de fazer isso, precisaremos alterar as referências da página mestra em todos os arquivos. aspx gerados pelas páginas WebForms padrão do ASP.NET.

> [!div class="step-by-step"]
> [Próximo](tailspin-spyworks-part-2.md)
