---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 'Parte 6: Associação de ASP.NET | Microsoft Docs'
author: JoeStagner
description: Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo Tailspin Spyworks. A parte 6 adiciona associação ASP.NET.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: b0caa89dc9ffb5bb7451fa2d9d346c7db2bf1466
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78564179"
---
# <a name="part-6-aspnet-membership"></a><span data-ttu-id="6d161-104">Parte 6: Associação do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="6d161-104">Part 6: ASP.NET Membership</span></span>

<span data-ttu-id="6d161-105">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="6d161-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="6d161-106">A Tailspin Spyworks demonstra como é extremamente simples criar aplicativos poderosos e escalonáveis para a plataforma .NET.</span><span class="sxs-lookup"><span data-stu-id="6d161-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="6d161-107">Ele mostra como usar os ótimos novos recursos do ASP.NET 4 para criar uma loja online, incluindo compras, check-out e administração.</span><span class="sxs-lookup"><span data-stu-id="6d161-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="6d161-108">Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="6d161-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="6d161-109">A parte 6 adiciona associação ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6d161-109">Part 6 adds ASP.NET Membership.</span></span>

## <a id="_Toc260221672"></a><span data-ttu-id="6d161-110">Trabalhando com associação ASP.NET</span><span class="sxs-lookup"><span data-stu-id="6d161-110">Working with ASP.NET Membership</span></span>

![](tailspin-spyworks-part-6/_static/image1.png)

<span data-ttu-id="6d161-111">Clique em segurança</span><span class="sxs-lookup"><span data-stu-id="6d161-111">Click Security</span></span>

![](tailspin-spyworks-part-6/_static/image1.jpg)

<span data-ttu-id="6d161-112">Certifique-se de que estamos usando a autenticação de formulários.</span><span class="sxs-lookup"><span data-stu-id="6d161-112">Make sure that we are using forms authentication.</span></span>

![](tailspin-spyworks-part-6/_static/image2.jpg)

<span data-ttu-id="6d161-113">Use o link "criar usuário" para criar alguns usuários.</span><span class="sxs-lookup"><span data-stu-id="6d161-113">Use the "Create User" link to create a couple of users.</span></span>

![](tailspin-spyworks-part-6/_static/image3.jpg)

<span data-ttu-id="6d161-114">Quando terminar, consulte a janela de Gerenciador de Soluções e atualize a exibição.</span><span class="sxs-lookup"><span data-stu-id="6d161-114">When done, refer to the Solution Explorer window and refresh the view.</span></span>

![](tailspin-spyworks-part-6/_static/image2.png)

<span data-ttu-id="6d161-115">Observe que o ASPNETDB. A multa do MDF foi criada.</span><span class="sxs-lookup"><span data-stu-id="6d161-115">Note that the ASPNETDB.MDF fine has been created.</span></span> <span data-ttu-id="6d161-116">Esse arquivo contém as tabelas para dar suporte aos serviços principais do ASP.NET, como associação.</span><span class="sxs-lookup"><span data-stu-id="6d161-116">This file contains the tables to support the core ASP.NET services like membership.</span></span>

<span data-ttu-id="6d161-117">Agora, podemos começar a implementar o processo de check-out.</span><span class="sxs-lookup"><span data-stu-id="6d161-117">Now we can begin implementing the checkout process.</span></span>

<span data-ttu-id="6d161-118">Comece criando uma página CheckOut. aspx.</span><span class="sxs-lookup"><span data-stu-id="6d161-118">Begin by creating a CheckOut.aspx page.</span></span>

<span data-ttu-id="6d161-119">A página CheckOut. aspx só deve estar disponível para os usuários que estão conectados, portanto, restringiremos o acesso a usuários conectados e redirecionarão os usuários que não estão conectados à página de logon.</span><span class="sxs-lookup"><span data-stu-id="6d161-119">The CheckOut.aspx page should only be available to users who are logged in so we will restrict access to logged in users and redirect users who are not logged in to the LogIn page.</span></span>

<span data-ttu-id="6d161-120">Para fazer isso, adicionaremos o seguinte à seção de configuração do nosso arquivo Web. config.</span><span class="sxs-lookup"><span data-stu-id="6d161-120">To do this we'll add the following to the configuration section of our web.config file.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

<span data-ttu-id="6d161-121">O modelo para ASP.NET Web Forms aplicativos adicionou automaticamente uma seção de autenticação ao nosso arquivo Web. config e estabeleceu a página de logon padrão.</span><span class="sxs-lookup"><span data-stu-id="6d161-121">The template for ASP.NET Web Forms applications automatically added an authentication section to our web.config file and established the default login page.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

<span data-ttu-id="6d161-122">É necessário modificar o arquivo. aspx de login do. ini para migrar um carrinho de compras anônimo quando o usuário fizer logon.</span><span class="sxs-lookup"><span data-stu-id="6d161-122">We must modify the Login.aspx code behind file to migrate an anonymous shopping cart when the user logs in.</span></span> <span data-ttu-id="6d161-123">Altere a página\_evento de carregamento da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="6d161-123">Change the Page\_Load event as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

<span data-ttu-id="6d161-124">Em seguida, adicione um manipulador de eventos "LoggedInTemplate" como este para definir o nome da sessão para o usuário conectado recentemente e altere a ID da sessão temporária no carrinho de compras para o do usuário chamando o método MigrateCart em nossa classe MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="6d161-124">Then add a "LoggedIn" event handler like this to set the session name to the newly logged in user and change the temporary session id in the shopping cart to that of the user by calling the MigrateCart method in our MyShoppingCart class.</span></span> <span data-ttu-id="6d161-125">(Implementado no arquivo. cs)</span><span class="sxs-lookup"><span data-stu-id="6d161-125">(Implemented in the .cs file)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

<span data-ttu-id="6d161-126">Implemente o método MigrateCart () como este.</span><span class="sxs-lookup"><span data-stu-id="6d161-126">Implement the MigrateCart() method like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

<span data-ttu-id="6d161-127">Em checkout. aspx, usaremos uma EntityDataSource e um GridView em nossa página de check-out como fizemos em nossa página de carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="6d161-127">In checkout.aspx we'll use an EntityDataSource and a GridView in our check out page much as we did in our shopping cart page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

<span data-ttu-id="6d161-128">Observe que nosso controle GridView especifica um manipulador de eventos "onligação" chamado myList\_de multiligação, portanto, vamos implementar esse manipulador de eventos como este.</span><span class="sxs-lookup"><span data-stu-id="6d161-128">Note that our GridView control specifies an "ondatabound" event handler named MyList\_RowDataBound so let's implement that event handler like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

<span data-ttu-id="6d161-129">Esse método mantém um total acumulado do carrinho de compras, pois cada linha é associada e atualiza a linha inferior do GridView.</span><span class="sxs-lookup"><span data-stu-id="6d161-129">This method keeps a running total of the shopping cart as each row is bound and updates the bottom row of the GridView.</span></span>

<span data-ttu-id="6d161-130">Neste estágio, implementamos uma apresentação de "revisão" do pedido a ser colocado.</span><span class="sxs-lookup"><span data-stu-id="6d161-130">At this stage we have implemented a "review" presentation of the order to be placed.</span></span>

<span data-ttu-id="6d161-131">Vamos manipular um cenário de carrinho vazio adicionando algumas linhas de código à nossa página\_evento de carregamento:</span><span class="sxs-lookup"><span data-stu-id="6d161-131">Let's handle an empty cart scenario by adding a few lines of code to our Page\_Load event:</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

<span data-ttu-id="6d161-132">Quando o usuário clicar no botão "enviar", executaremos o seguinte código no manipulador de eventos de clique no botão enviar.</span><span class="sxs-lookup"><span data-stu-id="6d161-132">When the user clicks on the "Submit" button we will execute the following code in the Submit Button Click Event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

<span data-ttu-id="6d161-133">A "carne" do processo de envio de pedidos deve ser implementada no método SubmitOrder () de nossa classe MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="6d161-133">The "meat" of the order submission process is to be implemented in the SubmitOrder() method of our MyShoppingCart class.</span></span>

<span data-ttu-id="6d161-134">SubmitOrder irá:</span><span class="sxs-lookup"><span data-stu-id="6d161-134">SubmitOrder will:</span></span>

- <span data-ttu-id="6d161-135">Pegue todos os itens de linha no carrinho de compras e use-os para criar um novo registro de pedido e os registros de OrderDetails associados.</span><span class="sxs-lookup"><span data-stu-id="6d161-135">Take all the line items in the shopping cart and use them to create a new Order Record and the associated OrderDetails records.</span></span>
- <span data-ttu-id="6d161-136">Calcular a data de envio.</span><span class="sxs-lookup"><span data-stu-id="6d161-136">Calculate Shipping Date.</span></span>
- <span data-ttu-id="6d161-137">Desmarque o carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="6d161-137">Clear the shopping cart.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

<span data-ttu-id="6d161-138">Para os fins deste aplicativo de exemplo, calcularemos uma data de remessa simplesmente adicionando dois dias à data atual.</span><span class="sxs-lookup"><span data-stu-id="6d161-138">For the purposes of this sample application we'll calculate a ship date by simply adding two days to the current date.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

<span data-ttu-id="6d161-139">A execução do aplicativo agora nos permitirá testar o processo de compra do início ao fim.</span><span class="sxs-lookup"><span data-stu-id="6d161-139">Running the application now will permit us to test the shopping process from start to finish.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6d161-140">[Anterior](tailspin-spyworks-part-5.md)
> [Próximo](tailspin-spyworks-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="6d161-140">[Previous](tailspin-spyworks-part-5.md)
[Next](tailspin-spyworks-part-7.md)</span></span>
