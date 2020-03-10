---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: ASP.NET scaffolding Visual Studio 2013 | Microsoft Docs
author: Rick-Anderson
description: ASP.NET scaffolding é um novo recurso que está incluído no Visual Studio 2013.
ms.author: riande
ms.date: 04/09/2014
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: cf4669b769cee28475e2dd6a6ddf07ea1434d04d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557977"
---
# <a name="aspnet-scaffolding-in-visual-studio-2013"></a>Scaffolding do ASP.NET no Visual Studio 2013

por [Tom FitzMacken](https://github.com/tfitzmac)

> ASP.NET scaffolding é um novo recurso que está incluído no Visual Studio 2013.

## <a name="overview"></a>Visão geral

ASP.NET scaffolding é uma estrutura de geração de código para aplicativos ASP.NET Web. Visual Studio 2013 inclui geradores de código pré-instalados para projetos MVC e de API Web. Você adiciona scaffolding ao seu projeto quando deseja adicionar rapidamente o código que interage com modelos de dados. O uso de scaffolding pode reduzir a quantidade de tempo para desenvolver operações de dados padrão em seu projeto.

Por padrão, Visual Studio 2013 não dá suporte à geração de código para um projeto Web Forms, mas você pode usar o scaffolding com Web Forms adicionando dependências MVC ao projeto ou instalando uma extensão. Ambas as abordagens são mostradas abaixo.

A atualização 2 do Visual Studio 2013 (atualmente RC) fornece a capacidade de estender ASP.NET scaffolding para atender aos requisitos do seu cenário. Com essa funcionalidade, você pode criar um modelo de scaffolding personalizado e adicioná-lo à caixa de diálogo Adicionar novo Scaffold. No modelo personalizado, você especifica o código que é gerado ao adicionar um item com Scaffold. Para obter mais informações, consulte [criando um Scaffolder personalizado para o Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).

## <a name="prerequisites"></a>Prerequisites

Para usar o ASP.NET scaffolding, você deve ter:

- Microsoft Visual Studio 2013
- Ferramentas para Desenvolvedores Web (parte da instalação padrão do Visual Studio 2013)
- Estruturas e ferramentas da Web do ASP.NET 2013 (parte da instalação padrão do Visual Studio 2013)

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a>Adicionar um item com Scaffold ao MVC ou à API Web

Para adicionar um Scaffold, clique com o botão direito do mouse no projeto ou em uma pasta dentro do projeto e selecione **Adicionar** – **New com Scaffold item**, conforme mostrado na imagem a seguir.

![Adicionar item Scaffold](aspnet-scaffolding-overview/_static/image1.png)

Na janela **Adicionar Scaffold** , selecione o tipo de Scaffold a ser adicionado.

![Selecione o tipo de Scaffold](aspnet-scaffolding-overview/_static/image2.png)

A janela **Adicionar controlador** oferece a oportunidade de selecionar opções para gerar o controlador, incluindo se você deseja usar os novos recursos assíncronos do Entity Framework 6.

![Adicionar controlador](aspnet-scaffolding-overview/_static/image3.png)

As classes e as páginas relevantes são criadas para seu cenário. Por exemplo, a imagem a seguir mostra o controlador MVC e as exibições que foram criadas por meio de scaffolding para uma classe de modelo chamada filmes.

![Os arquivos criados](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a>Adicionar um item com Scaffold a Web Forms

Para adicionar scaffolding que geram Web Forms código, você deve instalar uma extensão para o Visual Studio ou adicionar dependências MVC. As duas abordagens são mostradas abaixo, mas você só precisa realizar uma dessas abordagens.

### <a name="web-forms-scaffolding-extension"></a>Web Forms extensão scaffolding

Você pode instalar uma extensão do Visual Studio que permite usar o scaffolding com um projeto Web Forms. No Visual Studio, selecione **ferramentas** e, em seguida, **extensões e atualizações**. Nessa caixa de diálogo, pesquise a galeria do Visual Studio para **Web Forms scaffolding**.

![instalar o Web Forms scaffolding](aspnet-scaffolding-overview/_static/image5.png)

Para obter mais informações, consulte [Web Forms scaffolding](https://go.microsoft.com/fwlink/p/?LinkId=396478).

### <a name="mvc-dependencies"></a>Dependências do MVC

Para adicionar dependências MVC, selecione **adicionar** - **novo item com Scaffold**. Na janela Adicionar Scaffold, selecione **dependências MVC**, conforme mostrado abaixo.

![Adicionar dependências MVC](aspnet-scaffolding-overview/_static/image6.png)

Há duas opções para o scaffolding MVC; Mínimo e completo. Se você selecionar mínimo, somente os pacotes e as referências do NuGet para ASP.NET MVC serão adicionados ao seu projeto. Se você selecionar a opção completa, as dependências mínimas serão adicionadas, bem como os arquivos de conteúdo necessários para um projeto MVC. Para usar facilmente o scaffolding, selecione dependências completas.

![selecionar dependências completas](aspnet-scaffolding-overview/_static/image7.png)

Depois de adicionar as dependências, você verá um arquivo **README. txt** . Siga cuidadosamente as instruções neste arquivo para garantir que seu projeto funcione corretamente.

Quando você tiver concluído as etapas no arquivo readme. txt, poderá adicionar um novo item com Scaffold, conforme mostrado na seção anterior sobre MVC e API Web. As exibições e o controlador gerados automaticamente funcionarão corretamente em seu projeto.

## <a name="tutorials"></a>Tutoriais

Para criar um scaffolder personalizado, consulte [criando um Scaffolder personalizado para o Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).

Para personalizar os arquivos gerados, consulte [como personalizar os arquivos gerados na caixa de diálogo novo item do com Scaffold](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).

Para obter um exemplo de como usar o scaffolding com o **desenvolvimento Database First**, consulte [EF Database First com o ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).

Para obter um exemplo de como usar scaffolding em um projeto **MVC** , consulte [Introdução com o ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

Para obter um exemplo de como usar scaffolding em um projeto de **API Web** , consulte [criar uma API REST com roteamento de atributos na API Web 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).
