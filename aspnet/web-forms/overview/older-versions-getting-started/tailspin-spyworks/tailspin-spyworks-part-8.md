---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: 'Parte 8: páginas finais, manipulação de exceção e conclusão | Microsoft Docs'
author: JoeStagner
description: Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo Tailspin Spyworks. A parte 8 adiciona uma página de contato, sobre a página e a exceção...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: 707dc9d87ae324a7897c971a451e40bc54c96cb3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78586880"
---
# <a name="part-8-final-pages-exception-handling-and-conclusion"></a>Parte 8: páginas finais, manipulação de exceção e conclusão

por [Joe Stagner](https://github.com/JoeStagner)

> A Tailspin Spyworks demonstra como é extremamente simples criar aplicativos poderosos e escalonáveis para a plataforma .NET. Ele mostra como usar os ótimos novos recursos do ASP.NET 4 para criar uma loja online, incluindo compras, check-out e administração.
> 
> Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo Tailspin Spyworks. A parte 8 adiciona uma página de contato, sobre a página e a manipulação de exceções. Esta é a conclusão da série.

## <a id="_Toc260221680"></a>Página de contato (enviando email de ASP.NET)

Crie uma nova página chamada contactus. aspx

Usando o designer, crie o seguinte formulário fazendo uma observação especial para incluir o ToolkitScriptManager e o controle editor do AjaxControlToolkit. .

![](tailspin-spyworks-part-8/_static/image1.jpg)

Clique duas vezes no botão "enviar" para gerar um manipulador de eventos de clique no arquivo code-behind e implemente um método para enviar as informações de contato como um email.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

Esse código requer que o arquivo Web. config contenha uma entrada na seção de configuração que especifica o servidor SMTP a ser usado para enviar email.

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a>Página sobre

Crie uma página chamada AboutUs. aspx e adicione qualquer conteúdo que desejar.

## <a id="_Toc260221682"></a>Manipulador de exceção global

Por fim, em todo o aplicativo geramos exceções e há circunstâncias imprevistas que frios também causam exceções sem tratamento em nosso aplicativo Web.

Nunca queremos que uma exceção sem tratamento seja exibida para um visitante de site.

![](tailspin-spyworks-part-8/_static/image2.jpg)

Além de ser uma experiência de usuário terrível, as exceções não tratadas também podem ser um problema de segurança.

Para resolver esse problema, implementaremos um manipulador de exceção global.

Para fazer isso, abra o arquivo global. asax e observe o seguinte manipulador de eventos gerado previamente.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

Adicione o código para implementar o aplicativo\_manipulador de erro da seguinte maneira.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

Em seguida, adicione uma página chamada Error. aspx à solução e adicione este trecho de marcação.

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

Agora, na página\_manipulador de eventos de carregamento, extraia as mensagens de erro do objeto de solicitação.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a>Final

Vimos que os WebForms do ASP.NET facilitam a criação de um site sofisticado com acesso ao banco de dados, associação, AJAX, etc. muito rapidamente.

Espero que este tutorial tenha lhe dado as ferramentas necessárias para começar a criar seus próprios aplicativos WebForms do ASP.NET!

> [!div class="step-by-step"]
> [Anterior](tailspin-spyworks-part-7.md)
