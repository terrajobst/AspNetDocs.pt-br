---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: 'Parte 1: visão geral e criação do projeto | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/03/2012
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: a76a18f2bd95969358452085ef342fdca8a386e2
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600321"
---
# <a name="part-1-overview-and-creating-the-project"></a>Parte 1: visão geral e criação do projeto

por [Mike Wasson](https://github.com/MikeWasson)

[Baixar projeto concluído](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

Entity Framework é uma estrutura de mapeamento relacional/objeto. Ele mapeia os objetos de domínio em seu código para entidades em um banco de dados relacional. Na maioria das vezes, você não precisa se preocupar com a camada de banco de dados, pois Entity Framework cuida dela para você. Seu código manipula os objetos e as alterações são persistidas em um banco de dados.

## <a name="about-the-tutorial"></a>Sobre o tutorial

Neste tutorial, você criará um aplicativo de armazenamento simples. Há duas partes principais para o aplicativo. Os usuários normais podem exibir produtos e criar pedidos:

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

Os administradores podem criar, excluir ou editar produtos:

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a>Habilidades que você aprenderá

Veja o que você aprenderá:

- Como usar Entity Framework com ASP.NET Web API.
- Como usar knockout. js para criar uma interface do usuário dinâmica do cliente.
- Como usar a autenticação de formulários com a API da Web para autenticar usuários.

Embora este tutorial seja independente, você talvez queira ler primeiro os seguintes tutoriais:

- [Sua primeira ASP.NET Web API](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [Criando uma API Web que dá suporte a operações CRUD](../creating-a-web-api-that-supports-crud-operations.md)

Algum conhecimento do [ASP.NET MVC](../../../../mvc/index.md) também é útil.

## <a name="overview"></a>{1&gt;Visão Geral&lt;1}

Em um alto nível, aqui está a arquitetura do aplicativo:

- O ASP.NET MVC gera as páginas HTML para o cliente.
- ASP.NET Web API expõe operações CRUD nos dados (produtos e pedidos).
- Entity Framework converte os modelos usados C# pela API da Web em entidades de banco de dados.

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

O diagrama a seguir mostra como os objetos de domínio são representados em várias camadas do aplicativo: a camada de banco de dados, o modelo de objeto e, por fim, o formato de conexão, que é usado para transmitir dados para o cliente via HTTP.

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a>Criar o projeto do Visual Studio

Você pode criar o projeto do tutorial usando o Visual Web Developer Express ou a versão completa do Visual Studio.

Na página **inicial** , clique em **novo projeto**.

No painel **modelos** , selecione **modelos instalados** e expanda o **nó C# Visual** . Em **Visual C#** , selecione **Web**. Na lista de modelos de projeto, selecione **aplicativo Web ASP.NET MVC 4**. Nomeie o projeto "ProductStore" e clique em **OK**.

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

Na caixa de diálogo **novo projeto ASP.NET MVC 4** , selecione **aplicativo de Internet** e clique em **OK**.

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

O modelo "aplicativo de Internet" cria um aplicativo MVC ASP.NET que dá suporte à autenticação de formulários. Se você executar o aplicativo agora, ele já terá alguns recursos:

- Novos usuários podem se registrar clicando no link "registrar" no canto superior direito.
- Os usuários registrados podem fazer logon clicando no link "logon".

As informações de associação são mantidas em um banco de dados que é criado automaticamente. Para obter mais informações sobre a autenticação de formulários no ASP.NET MVC, consulte [passo a passos: usando a autenticação de formulários no ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).

## <a name="update-the-css-file"></a>Atualizar o arquivo CSS

Essa etapa é superficial, mas fará com que as páginas sejam renderizadas como as capturas de tela anteriores.

Em Gerenciador de Soluções, expanda a pasta conteúdo e abra o arquivo chamado site. css. Adicione os seguintes estilos de CSS:

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

> [!div class="step-by-step"]
> [Avançar](using-web-api-with-entity-framework-part-2.md)
