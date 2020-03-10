---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: 'Parte 9: registro e check-out | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo de loja de música MVC do ASP.NET. A parte 9 aborda o registro e o check-out.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: 040bc0ccef889fb9a7c3d9b5ce88c75b7b754248
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559531"
---
# <a name="part-9-registration-and-checkout"></a><span data-ttu-id="efbd8-104">Parte 9: registro e check-out</span><span class="sxs-lookup"><span data-stu-id="efbd8-104">Part 9: Registration and Checkout</span></span>

<span data-ttu-id="efbd8-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="efbd8-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="efbd8-106">A loja de música MVC é um aplicativo de tutorial que apresenta e explica passo a passo como usar o ASP.NET MVC e o Visual Studio para desenvolvimento para a Web.</span><span class="sxs-lookup"><span data-stu-id="efbd8-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="efbd8-107">A loja de música MVC é uma implementação de armazenamento de exemplo leve que vende os álbuns de música online e implementa a administração básica de site, a entrada do usuário e a funcionalidade do carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="efbd8-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="efbd8-108">Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo de loja de música MVC do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="efbd8-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="efbd8-109">A parte 9 aborda o registro e o check-out.</span><span class="sxs-lookup"><span data-stu-id="efbd8-109">Part 9 covers Registration and Checkout.</span></span>

<span data-ttu-id="efbd8-110">Nesta seção, criaremos um CheckoutController que coletará as informações de endereço e pagamento do comprador.</span><span class="sxs-lookup"><span data-stu-id="efbd8-110">In this section, we will be creating a CheckoutController which will collect the shopper's address and payment information.</span></span> <span data-ttu-id="efbd8-111">Exigiremos que os usuários se registrem em nosso site antes de fazer o check-out, portanto, esse controlador exigirá autorização.</span><span class="sxs-lookup"><span data-stu-id="efbd8-111">We will require users to register with our site prior to checking out, so this controller will require authorization.</span></span>

<span data-ttu-id="efbd8-112">Os usuários navegarão até o processo de check-out de seu carrinho de compras clicando no botão "check-out".</span><span class="sxs-lookup"><span data-stu-id="efbd8-112">Users will navigate to the checkout process from their shopping cart by clicking the "Checkout" button.</span></span>

![](mvc-music-store-part-9/_static/image1.jpg)

<span data-ttu-id="efbd8-113">Se o usuário não estiver conectado, ele será solicitado a fazê-lo.</span><span class="sxs-lookup"><span data-stu-id="efbd8-113">If the user is not logged in, they will be prompted to.</span></span>

![](mvc-music-store-part-9/_static/image1.png)

<span data-ttu-id="efbd8-114">Após o logon bem-sucedido, o usuário mostra a exibição de endereço e de pagamento.</span><span class="sxs-lookup"><span data-stu-id="efbd8-114">Upon successful login, the user is then shown the Address and Payment view.</span></span>

![](mvc-music-store-part-9/_static/image2.png)

<span data-ttu-id="efbd8-115">Depois de preencher o formulário e enviar o pedido, eles serão mostrados na tela de confirmação do pedido.</span><span class="sxs-lookup"><span data-stu-id="efbd8-115">Once they have filled the form and submitted the order, they will be shown the order confirmation screen.</span></span>

![](mvc-music-store-part-9/_static/image3.png)

<span data-ttu-id="efbd8-116">A tentativa de exibir uma ordem inexistente ou uma ordem que não pertence a você mostrará o modo de exibição de erro.</span><span class="sxs-lookup"><span data-stu-id="efbd8-116">Attempting to view either a non-existent order or an order that doesn't belong to you will show the Error view.</span></span>

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a><span data-ttu-id="efbd8-117">Migrando o carrinho de compras</span><span class="sxs-lookup"><span data-stu-id="efbd8-117">Migrating the Shopping Cart</span></span>

<span data-ttu-id="efbd8-118">Embora o processo de compra seja anônimo, quando o usuário clica no botão de check-out, ele será solicitado a se registrar e fazer logon.</span><span class="sxs-lookup"><span data-stu-id="efbd8-118">While the shopping process is anonymous, when the user clicks on the Checkout button, they will be required to register and login.</span></span> <span data-ttu-id="efbd8-119">Os usuários esperam que manteremos suas informações de carrinho de compras entre visitas, portanto, precisaremos associar as informações do carrinho de compras a um usuário quando ele concluir o registro ou o logon.</span><span class="sxs-lookup"><span data-stu-id="efbd8-119">Users will expect that we will maintain their shopping cart information between visits, so we will need to associate the shopping cart information with a user when they complete registration or login.</span></span>

<span data-ttu-id="efbd8-120">Na verdade, isso é muito simples, pois nossa classe ShoppingCart já tem um método que associará todos os itens do carrinho atual a um nome de usuário.</span><span class="sxs-lookup"><span data-stu-id="efbd8-120">This is actually very simple to do, as our ShoppingCart class already has a method which will associate all the items in the current cart with a username.</span></span> <span data-ttu-id="efbd8-121">Só precisaremos chamar esse método quando um usuário concluir o registro ou o logon.</span><span class="sxs-lookup"><span data-stu-id="efbd8-121">We will just need to call this method when a user completes registration or login.</span></span>

<span data-ttu-id="efbd8-122">Abra a classe **AccountController** que adicionamos ao configurar a associação e a autorização.</span><span class="sxs-lookup"><span data-stu-id="efbd8-122">Open the **AccountController** class that we added when we were setting up Membership and Authorization.</span></span> <span data-ttu-id="efbd8-123">Adicione uma instrução using referenciando MvcMusicStore. Models e, em seguida, adicione o seguinte método MigrateShoppingCart:</span><span class="sxs-lookup"><span data-stu-id="efbd8-123">Add a using statement referencing MvcMusicStore.Models, then add the following MigrateShoppingCart method:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

<span data-ttu-id="efbd8-124">Em seguida, modifique a ação de postagem de LogOn para chamar MigrateShoppingCart depois que o usuário tiver sido validado, conforme mostrado abaixo:</span><span class="sxs-lookup"><span data-stu-id="efbd8-124">Next, modify the LogOn post action to call MigrateShoppingCart after the user has been validated, as shown below:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

<span data-ttu-id="efbd8-125">Faça a mesma alteração na ação de postagem de registro imediatamente após a criação bem-sucedida da conta de usuário:</span><span class="sxs-lookup"><span data-stu-id="efbd8-125">Make the same change to the Register post action, immediately after the user account is successfully created:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

<span data-ttu-id="efbd8-126">É isso – agora um carrinho de compras anônimo será transferido automaticamente para uma conta de usuário após o registro ou logon bem-sucedido.</span><span class="sxs-lookup"><span data-stu-id="efbd8-126">That's it - now an anonymous shopping cart will be automatically transferred to a user account upon successful registration or login.</span></span>

## <a name="creating-the-checkoutcontroller"></a><span data-ttu-id="efbd8-127">Criando o CheckoutController</span><span class="sxs-lookup"><span data-stu-id="efbd8-127">Creating the CheckoutController</span></span>

<span data-ttu-id="efbd8-128">Clique com o botão direito do mouse na pasta controladores e adicione um novo controlador ao projeto chamado CheckoutController usando o modelo de controlador vazio.</span><span class="sxs-lookup"><span data-stu-id="efbd8-128">Right-click on the Controllers folder and add a new Controller to the project named CheckoutController using the Empty controller template.</span></span>

![](mvc-music-store-part-9/_static/image5.png)

<span data-ttu-id="efbd8-129">Primeiro, adicione o atributo Authorize acima da declaração da classe Controller para exigir que os usuários se registrem antes do check-out:</span><span class="sxs-lookup"><span data-stu-id="efbd8-129">First, add the Authorize attribute above the Controller class declaration to require users to register before checkout:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

<span data-ttu-id="efbd8-130">*Observação: isso é semelhante à alteração que fizemos anteriormente no StoreManagerController, mas, nesse caso, o atributo Authorize exigia que o usuário esteja em uma função de administrador. No controlador de check-out, estamos exigindo que o usuário esteja conectado, mas não exigindo que eles sejam administradores.*</span><span class="sxs-lookup"><span data-stu-id="efbd8-130">*Note: This is similar to the change we previously made to the StoreManagerController, but in that case the Authorize attribute required that the user be in an Administrator role. In the Checkout Controller, we're requiring the user be logged in but aren't requiring that they be administrators.*</span></span>

<span data-ttu-id="efbd8-131">Para simplificar, não lidaremos com as informações de pagamento neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="efbd8-131">For the sake of simplicity, we won't be dealing with payment information in this tutorial.</span></span> <span data-ttu-id="efbd8-132">Em vez disso, estamos permitindo que os usuários confiram usando um código promocional.</span><span class="sxs-lookup"><span data-stu-id="efbd8-132">Instead, we are allowing users to check out using a promotional code.</span></span> <span data-ttu-id="efbd8-133">Armazenaremos esse código promocional usando uma constante chamada código promocional.</span><span class="sxs-lookup"><span data-stu-id="efbd8-133">We will store this promotional code using a constant named PromoCode.</span></span>

<span data-ttu-id="efbd8-134">Como no StoreController, vamos declarar um campo para manter uma instância da classe MusicStoreEntities, chamada storeDB.</span><span class="sxs-lookup"><span data-stu-id="efbd8-134">As in the StoreController, we'll declare a field to hold an instance of the MusicStoreEntities class, named storeDB.</span></span> <span data-ttu-id="efbd8-135">Para fazer uso da classe MusicStoreEntities, precisaremos adicionar uma instrução using para o namespace MvcMusicStore. Models.</span><span class="sxs-lookup"><span data-stu-id="efbd8-135">In order to make use of the MusicStoreEntities class, we will need to add a using statement for the MvcMusicStore.Models namespace.</span></span> <span data-ttu-id="efbd8-136">A parte superior do nosso controlador de check-out aparece abaixo.</span><span class="sxs-lookup"><span data-stu-id="efbd8-136">The top of our Checkout controller appears below.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

<span data-ttu-id="efbd8-137">O CheckoutController terá as seguintes ações do controlador:</span><span class="sxs-lookup"><span data-stu-id="efbd8-137">The CheckoutController will have the following controller actions:</span></span>

<span data-ttu-id="efbd8-138">**AddressAndPayment (Get Method)** exibirá um formulário para permitir que o usuário insira suas informações.</span><span class="sxs-lookup"><span data-stu-id="efbd8-138">**AddressAndPayment (GET method)** will display a form to allow the user to enter their information.</span></span>

<span data-ttu-id="efbd8-139">**AddressAndPayment (método post)** validará a entrada e processará a ordem.</span><span class="sxs-lookup"><span data-stu-id="efbd8-139">**AddressAndPayment (POST method)** will validate the input and process the order.</span></span>

<span data-ttu-id="efbd8-140">**Concluir** será mostrado depois que um usuário tiver concluído com êxito o processo de check-out.</span><span class="sxs-lookup"><span data-stu-id="efbd8-140">**Complete** will be shown after a user has successfully finished the checkout process.</span></span> <span data-ttu-id="efbd8-141">Essa exibição incluirá o número do pedido do usuário, como confirmação.</span><span class="sxs-lookup"><span data-stu-id="efbd8-141">This view will include the user's order number, as confirmation.</span></span>

<span data-ttu-id="efbd8-142">Primeiro, vamos renomear a ação do controlador de índice (que foi gerado quando criamos o controlador) para AddressAndPayment.</span><span class="sxs-lookup"><span data-stu-id="efbd8-142">First, let's rename the Index controller action (which was generated when we created the controller) to AddressAndPayment.</span></span> <span data-ttu-id="efbd8-143">Essa ação do controlador apenas exibe o formulário de check-out, portanto, não requer nenhuma informação de modelo.</span><span class="sxs-lookup"><span data-stu-id="efbd8-143">This controller action just displays the checkout form, so it doesn't require any model information.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

<span data-ttu-id="efbd8-144">Nosso método POST AddressAndPayment seguirá o mesmo padrão que usamos no StoreManagerController: ele tentará aceitar o envio do formulário e concluirá o pedido e exibirá novamente o formulário se ele falhar.</span><span class="sxs-lookup"><span data-stu-id="efbd8-144">Our AddressAndPayment POST method will follow the same pattern we used in the StoreManagerController: it will try to accept the form submission and complete the order, and will re-display the form if it fails.</span></span>

<span data-ttu-id="efbd8-145">Depois de validar que a entrada do formulário atende aos nossos requisitos de validação para um pedido, verificaremos o valor do formulário código promocional diretamente.</span><span class="sxs-lookup"><span data-stu-id="efbd8-145">After validating the form input meets our validation requirements for an Order, we will check the PromoCode form value directly.</span></span> <span data-ttu-id="efbd8-146">Supondo que tudo esteja correto, salvaremos as informações atualizadas com o pedido, informaremos ao objeto ShoppingCart para concluir o processo do pedido e redirecionarei para a ação completa.</span><span class="sxs-lookup"><span data-stu-id="efbd8-146">Assuming everything is correct, we will save the updated information with the order, tell the ShoppingCart object to complete the order process, and redirect to the Complete action.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

<span data-ttu-id="efbd8-147">Após a conclusão bem-sucedida do processo de check-out, os usuários serão redirecionados para a ação completa do controlador.</span><span class="sxs-lookup"><span data-stu-id="efbd8-147">Upon successful completion of the checkout process, users will be redirected to the Complete controller action.</span></span> <span data-ttu-id="efbd8-148">Essa ação executará uma verificação simples para validar que o pedido realmente pertence ao usuário conectado antes de mostrar o número do pedido como uma confirmação.</span><span class="sxs-lookup"><span data-stu-id="efbd8-148">This action will perform a simple check to validate that the order does indeed belong to the logged-in user before showing the order number as a confirmation.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

<span data-ttu-id="efbd8-149">*Observação: o modo de exibição de erro foi criado automaticamente para nós na pasta/Views/Shared quando começamos o projeto.*</span><span class="sxs-lookup"><span data-stu-id="efbd8-149">*Note: The Error view was automatically created for us in the /Views/Shared folder when we began the project.*</span></span>

<span data-ttu-id="efbd8-150">O código completo do CheckoutController é o seguinte:</span><span class="sxs-lookup"><span data-stu-id="efbd8-150">The complete CheckoutController code is as follows:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a><span data-ttu-id="efbd8-151">Adicionando a exibição AddressAndPayment</span><span class="sxs-lookup"><span data-stu-id="efbd8-151">Adding the AddressAndPayment view</span></span>

<span data-ttu-id="efbd8-152">Agora, vamos criar a exibição AddressAndPayment.</span><span class="sxs-lookup"><span data-stu-id="efbd8-152">Now, let's create the AddressAndPayment view.</span></span> <span data-ttu-id="efbd8-153">Clique com o botão direito do mouse em uma das ações do controlador AddressAndPayment e adicione uma exibição chamada AddressAndPayment, que é fortemente tipada como uma ordem, e usa o modelo de edição, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="efbd8-153">Right-click on one of the AddressAndPayment controller actions and add a view named AddressAndPayment which is strongly typed as an Order and uses the Edit template, as shown below.</span></span>

![](mvc-music-store-part-9/_static/image6.png)

<span data-ttu-id="efbd8-154">Essa exibição fará uso de duas das técnicas que vimos ao criar a exibição StoreManagerEdit:</span><span class="sxs-lookup"><span data-stu-id="efbd8-154">This view will make use of two of the techniques we looked at while building the StoreManagerEdit view:</span></span>

- <span data-ttu-id="efbd8-155">Usaremos HTML. EditorForModel () para exibir campos de formulário para o modelo de pedido</span><span class="sxs-lookup"><span data-stu-id="efbd8-155">We will use Html.EditorForModel() to display form fields for the Order model</span></span>
- <span data-ttu-id="efbd8-156">Aproveitaremos as regras de validação usando uma classe Order com atributos de validação</span><span class="sxs-lookup"><span data-stu-id="efbd8-156">We will leverage validation rules using an Order class with validation attributes</span></span>

<span data-ttu-id="efbd8-157">Começaremos atualizando o código do formulário para usar HTML. EditorForModel (), seguido por uma caixa de texto adicional para o código promocional.</span><span class="sxs-lookup"><span data-stu-id="efbd8-157">We'll start by updating the form code to use Html.EditorForModel(), followed by an additional textbox for the Promo Code.</span></span> <span data-ttu-id="efbd8-158">O código completo da exibição AddressAndPayment é mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="efbd8-158">The complete code for the AddressAndPayment view is shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a><span data-ttu-id="efbd8-159">Definindo regras de validação para o pedido</span><span class="sxs-lookup"><span data-stu-id="efbd8-159">Defining validation rules for the Order</span></span>

<span data-ttu-id="efbd8-160">Agora que a nossa exibição está configurada, configuraremos as regras de validação para nosso modelo de pedido como fizemos anteriormente para o modelo de álbum.</span><span class="sxs-lookup"><span data-stu-id="efbd8-160">Now that our view is set up, we will set up the validation rules for our Order model as we did previously for the Album model.</span></span> <span data-ttu-id="efbd8-161">Clique com o botão direito do mouse na pasta modelos e adicione uma classe chamada Order.</span><span class="sxs-lookup"><span data-stu-id="efbd8-161">Right-click on the Models folder and add a class named Order.</span></span> <span data-ttu-id="efbd8-162">Além dos atributos de validação que usamos anteriormente para o álbum, também usaremos uma expressão regular para validar o endereço de email do usuário.</span><span class="sxs-lookup"><span data-stu-id="efbd8-162">In addition to the validation attributes we used previously for the Album, we will also be using a Regular Expression to validate the user's email address.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

<span data-ttu-id="efbd8-163">A tentativa de enviar o formulário com informações ausentes ou inválidas agora mostrará a mensagem de erro usando a validação do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="efbd8-163">Attempting to submit the form with missing or invalid information will now show error message using client-side validation.</span></span>

![](mvc-music-store-part-9/_static/image7.png)

<span data-ttu-id="efbd8-164">Ok, fizemos a maior parte do trabalho árduo para o processo de check-out; Temos apenas algumas chances e terminamos para terminar.</span><span class="sxs-lookup"><span data-stu-id="efbd8-164">Okay, we've done most of the hard work for the checkout process; we just have a few odds and ends to finish.</span></span> <span data-ttu-id="efbd8-165">Precisamos adicionar dois modos de exibição simples e precisamos cuidar da entrega das informações do carrinho durante o processo de logon.</span><span class="sxs-lookup"><span data-stu-id="efbd8-165">We need to add two simple views, and we need to take care of the handoff of the cart information during the login process.</span></span>

## <a name="adding-the-checkout-complete-view"></a><span data-ttu-id="efbd8-166">Adicionando a exibição de check-out completo</span><span class="sxs-lookup"><span data-stu-id="efbd8-166">Adding the Checkout Complete view</span></span>

<span data-ttu-id="efbd8-167">A exibição de check-out completo é muito simples, pois ela apenas precisa exibir a ID do pedido.</span><span class="sxs-lookup"><span data-stu-id="efbd8-167">The Checkout Complete view is pretty simple, as it just needs to display the Order ID.</span></span> <span data-ttu-id="efbd8-168">Clique com o botão direito do mouse na ação concluir controlador e adicione uma exibição chamada completa, que é fortemente tipada como um int.</span><span class="sxs-lookup"><span data-stu-id="efbd8-168">Right-click on the Complete controller action and add a view named Complete which is strongly typed as an int.</span></span>

![](mvc-music-store-part-9/_static/image8.png)

<span data-ttu-id="efbd8-169">Agora, atualizaremos o código de exibição para exibir a ID do pedido, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="efbd8-169">Now we will update the view code to display the Order ID, as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a><span data-ttu-id="efbd8-170">Atualizando a exibição de erro</span><span class="sxs-lookup"><span data-stu-id="efbd8-170">Updating The Error view</span></span>

<span data-ttu-id="efbd8-171">O modelo padrão inclui uma exibição de erro na pasta exibições compartilhadas para que possa ser usado novamente em outro lugar no site.</span><span class="sxs-lookup"><span data-stu-id="efbd8-171">The default template includes an Error view in the Shared views folder so that it can be re-used elsewhere in the site.</span></span> <span data-ttu-id="efbd8-172">Esta exibição de erro contém um erro muito simples e não usa nosso layout de site, portanto, nós o atualizaremos.</span><span class="sxs-lookup"><span data-stu-id="efbd8-172">This Error view contains a very simple error and doesn't use our site Layout, so we'll update it.</span></span>

<span data-ttu-id="efbd8-173">Como essa é uma página de erro genérica, o conteúdo é muito simples.</span><span class="sxs-lookup"><span data-stu-id="efbd8-173">Since this is a generic error page, the content is very simple.</span></span> <span data-ttu-id="efbd8-174">Incluiremos uma mensagem e um link para navegar até a página anterior no histórico se o usuário quiser repetir a ação.</span><span class="sxs-lookup"><span data-stu-id="efbd8-174">We'll include a message and a link to navigate to the previous page in history if the user wants to re-try their action.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]

> [!div class="step-by-step"]
> <span data-ttu-id="efbd8-175">[Anterior](mvc-music-store-part-8.md)
> [Próximo](mvc-music-store-part-10.md)</span><span class="sxs-lookup"><span data-stu-id="efbd8-175">[Previous](mvc-music-store-part-8.md)
[Next](mvc-music-store-part-10.md)</span></span>
