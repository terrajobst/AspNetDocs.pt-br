---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: Usando a API Web com ASP.NET Web Forms-ASP.NET 4. x
author: MikeWasson
description: Tutorial com código passo a passo para adicionar a API Web a um aplicativo do ASP.NET Forms para o ASP.NET 4. x
ms.author: riande
ms.date: 04/03/2012
ms.custom: seoapril2019
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: ae553b62998fefd128e12711cbde958ea42d8c63
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556773"
---
# <a name="using-web-api-with-aspnet-web-forms"></a>Usar a API da Web com formulários da Web do ASP.NET

por [Mike Wasson](https://github.com/MikeWasson)

Este tutorial orienta você pelas etapas para adicionar a API da Web a um aplicativo ASP.NET Web Forms tradicional no ASP.NET 4. x. 

## <a name="overview"></a>Visão geral

Embora ASP.NET Web API seja empacotado com o ASP.NET MVC, é fácil adicionar a API Web a um aplicativo ASP.NET Web Forms tradicional.

Para usar a API Web em um aplicativo Web Forms, há duas etapas principais:

- Adicione um controlador de API da Web derivado da classe **ApiController** .
- Adicione uma tabela de rotas ao método **Start do\_do aplicativo** .

## <a name="create-a-web-forms-project"></a>Criar um projeto Web Forms

Inicie o Visual Studio e selecione **novo projeto** na página **inicial** . Ou, no menu **arquivo** , selecione **novo** e **projeto**.

No painel **modelos** , selecione **modelos instalados** e expanda o **nó C# Visual** . Em **Visual C#** , selecione **Web**. Na lista de modelos de projeto, selecione **ASP.NET Web Forms aplicativo**. Insira um nome para o projeto e clique em **OK**.

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a>Criar o modelo e o controlador

Este tutorial usa as mesmas classes de modelo e controlador do tutorial de [introdução](tutorial-your-first-web-api.md) .

Primeiro, adicione uma classe de modelo. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar classe**. Nomeie o produto de classe e adicione a seguinte implementação:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

Em seguida, adicione um controlador de API Web ao projeto., um *controlador* é o objeto que MANIPULA solicitações HTTP para a API da Web.

No **Gerenciador de Soluções**, clique com o botão direito do mouse no nome do projeto. Selecione **Adicionar novo item**.

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

Em **modelos instalados**, expanda **Visual C#**  e selecione **Web**. Em seguida, na lista de modelos, selecione **classe de controlador da API Web**. Nomeie o controlador como "ProductsController" e clique em **Adicionar**.

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

O assistente **Adicionar novo item** criará um arquivo chamado ProductsController.cs. Exclua os métodos incluídos pelo assistente e adicione os seguintes métodos:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

Para obter mais informações sobre o código nesse controlador, consulte o tutorial de [introdução](tutorial-your-first-web-api.md) .

## <a name="add-routing-information"></a>Adicionar informações de roteamento

Em seguida, adicionaremos uma rota de URI para que os URIs do formulário &quot;/API/Products/&quot; sejam roteados para o controlador.

Em **Gerenciador de soluções**, clique duas vezes em global. asax para abrir o arquivo code-behind global.asax.cs. Adicione a seguinte instrução **using** .

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

Em seguida, adicione o seguinte código ao **aplicativo\_** método de início:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

Para obter mais informações sobre tabelas de roteamento, consulte [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).

## <a name="add-client-side-ajax"></a>Adicionar AJAX do lado do cliente

Isso é tudo o que você precisa para criar uma API Web que os clientes podem acessar. Agora, vamos adicionar uma página HTML que usa jQuery para chamar a API.

Verifique se a página mestra (por exemplo, *site. Master*) inclui um `ContentPlaceHolder` com `ID="HeadContent"`:

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

Abra o arquivo default. aspx. Substitua o texto clichê que está na seção de conteúdo principal, conforme mostrado:

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

Em seguida, adicione uma referência ao arquivo de origem jQuery na seção `HeaderContent`:

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

Observação: você pode adicionar facilmente a referência de script arrastando e soltando o arquivo de **Gerenciador de soluções** para a janela Editor de código.

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

Abaixo da marca de script jQuery, adicione o seguinte bloco de script:

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

Quando o documento é carregado, esse script faz uma solicitação AJAX para &quot;&quot;de API/produtos. A solicitação retorna uma lista de produtos no formato JSON. O script adiciona as informações do produto à tabela HTML.

Quando você executa o aplicativo, ele deve ter a seguinte aparência:

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
