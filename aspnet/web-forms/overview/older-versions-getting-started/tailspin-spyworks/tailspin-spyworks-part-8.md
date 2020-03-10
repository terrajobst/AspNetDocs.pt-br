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
# <a name="part-8-final-pages-exception-handling-and-conclusion"></a><span data-ttu-id="a7e02-104">Parte 8: páginas finais, manipulação de exceção e conclusão</span><span class="sxs-lookup"><span data-stu-id="a7e02-104">Part 8: Final Pages, Exception Handling, and Conclusion</span></span>

<span data-ttu-id="a7e02-105">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="a7e02-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="a7e02-106">A Tailspin Spyworks demonstra como é extremamente simples criar aplicativos poderosos e escalonáveis para a plataforma .NET.</span><span class="sxs-lookup"><span data-stu-id="a7e02-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="a7e02-107">Ele mostra como usar os ótimos novos recursos do ASP.NET 4 para criar uma loja online, incluindo compras, check-out e administração.</span><span class="sxs-lookup"><span data-stu-id="a7e02-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="a7e02-108">Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="a7e02-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="a7e02-109">A parte 8 adiciona uma página de contato, sobre a página e a manipulação de exceções.</span><span class="sxs-lookup"><span data-stu-id="a7e02-109">Part 8 adds a contact page, about page, and exception handling.</span></span> <span data-ttu-id="a7e02-110">Esta é a conclusão da série.</span><span class="sxs-lookup"><span data-stu-id="a7e02-110">This is the conclusion of the series.</span></span>

## <a id="_Toc260221680"></a><span data-ttu-id="a7e02-111">Página de contato (enviando email de ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="a7e02-111">Contact Page (Sending email from ASP.NET)</span></span>

<span data-ttu-id="a7e02-112">Crie uma nova página chamada contactus. aspx</span><span class="sxs-lookup"><span data-stu-id="a7e02-112">Create a new page named ContactUs.aspx</span></span>

<span data-ttu-id="a7e02-113">Usando o designer, crie o seguinte formulário fazendo uma observação especial para incluir o ToolkitScriptManager e o controle editor do AjaxControlToolkit.</span><span class="sxs-lookup"><span data-stu-id="a7e02-113">Using the designer, create the following form taking special note to include the ToolkitScriptManager and the Editor control from the AjaxControlToolkit.</span></span> <span data-ttu-id="a7e02-114">.</span><span class="sxs-lookup"><span data-stu-id="a7e02-114">.</span></span>

![](tailspin-spyworks-part-8/_static/image1.jpg)

<span data-ttu-id="a7e02-115">Clique duas vezes no botão "enviar" para gerar um manipulador de eventos de clique no arquivo code-behind e implemente um método para enviar as informações de contato como um email.</span><span class="sxs-lookup"><span data-stu-id="a7e02-115">Double click on the "Submit" button to generate a click event handler in the code behind file and implement a method to send the contact information as an email.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

<span data-ttu-id="a7e02-116">Esse código requer que o arquivo Web. config contenha uma entrada na seção de configuração que especifica o servidor SMTP a ser usado para enviar email.</span><span class="sxs-lookup"><span data-stu-id="a7e02-116">This code requires that your web.config file contain an entry in the configuration section that specifies the SMTP server to use for sending mail.</span></span>

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a><span data-ttu-id="a7e02-117">Página sobre</span><span class="sxs-lookup"><span data-stu-id="a7e02-117">About Page</span></span>

<span data-ttu-id="a7e02-118">Crie uma página chamada AboutUs. aspx e adicione qualquer conteúdo que desejar.</span><span class="sxs-lookup"><span data-stu-id="a7e02-118">Create a page named AboutUs.aspx and add whatever content you like.</span></span>

## <a id="_Toc260221682"></a><span data-ttu-id="a7e02-119">Manipulador de exceção global</span><span class="sxs-lookup"><span data-stu-id="a7e02-119">Global Exception Handler</span></span>

<span data-ttu-id="a7e02-120">Por fim, em todo o aplicativo geramos exceções e há circunstâncias imprevistas que frios também causam exceções sem tratamento em nosso aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="a7e02-120">Lastly, throughout the application we have thrown exceptions and there are unforeseen circumstances that cold also cause unhandled exceptions in our web application.</span></span>

<span data-ttu-id="a7e02-121">Nunca queremos que uma exceção sem tratamento seja exibida para um visitante de site.</span><span class="sxs-lookup"><span data-stu-id="a7e02-121">We never want an unhandled exception to be displayed to a web site visitor.</span></span>

![](tailspin-spyworks-part-8/_static/image2.jpg)

<span data-ttu-id="a7e02-122">Além de ser uma experiência de usuário terrível, as exceções não tratadas também podem ser um problema de segurança.</span><span class="sxs-lookup"><span data-stu-id="a7e02-122">Apart from being a terrible user experience unhandled exceptions can also be a security problem.</span></span>

<span data-ttu-id="a7e02-123">Para resolver esse problema, implementaremos um manipulador de exceção global.</span><span class="sxs-lookup"><span data-stu-id="a7e02-123">To solve this problem we will implement a global exception handler.</span></span>

<span data-ttu-id="a7e02-124">Para fazer isso, abra o arquivo global. asax e observe o seguinte manipulador de eventos gerado previamente.</span><span class="sxs-lookup"><span data-stu-id="a7e02-124">To do this, open the Global.asax file and note the following pre-generated event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

<span data-ttu-id="a7e02-125">Adicione o código para implementar o aplicativo\_manipulador de erro da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="a7e02-125">Add code to implement the Application\_Error handler as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

<span data-ttu-id="a7e02-126">Em seguida, adicione uma página chamada Error. aspx à solução e adicione este trecho de marcação.</span><span class="sxs-lookup"><span data-stu-id="a7e02-126">Then add a page named Error.aspx to the solution and add this markup snippet.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

<span data-ttu-id="a7e02-127">Agora, na página\_manipulador de eventos de carregamento, extraia as mensagens de erro do objeto de solicitação.</span><span class="sxs-lookup"><span data-stu-id="a7e02-127">Now in the Page\_Load event handler extract the error messages from the Request Object.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a><span data-ttu-id="a7e02-128">Final</span><span class="sxs-lookup"><span data-stu-id="a7e02-128">Conclusion</span></span>

<span data-ttu-id="a7e02-129">Vimos que os WebForms do ASP.NET facilitam a criação de um site sofisticado com acesso ao banco de dados, associação, AJAX, etc.</span><span class="sxs-lookup"><span data-stu-id="a7e02-129">We've seen that ASP.NET WebForms makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="a7e02-130">muito rapidamente.</span><span class="sxs-lookup"><span data-stu-id="a7e02-130">pretty quickly.</span></span>

<span data-ttu-id="a7e02-131">Espero que este tutorial tenha lhe dado as ferramentas necessárias para começar a criar seus próprios aplicativos WebForms do ASP.NET!</span><span class="sxs-lookup"><span data-stu-id="a7e02-131">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET WebForms applications!</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a7e02-132">Anterior</span><span class="sxs-lookup"><span data-stu-id="a7e02-132">Previous</span></span>](tailspin-spyworks-part-7.md)
