---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: 'Parte 5: lógica de negócios | Microsoft Docs'
author: JoeStagner
description: Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo Tailspin Spyworks. A parte 5 adiciona alguma lógica de negócios.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: c60eece9223c47304786d7b0d0ca4b11ac0572e9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78630301"
---
# <a name="part-5-business-logic"></a><span data-ttu-id="e7058-104">Parte 5: lógica de negócios</span><span class="sxs-lookup"><span data-stu-id="e7058-104">Part 5: Business Logic</span></span>

<span data-ttu-id="e7058-105">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="e7058-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="e7058-106">A Tailspin Spyworks demonstra como é extremamente simples criar aplicativos poderosos e escalonáveis para a plataforma .NET.</span><span class="sxs-lookup"><span data-stu-id="e7058-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="e7058-107">Ele mostra como usar os ótimos novos recursos do ASP.NET 4 para criar uma loja online, incluindo compras, check-out e administração.</span><span class="sxs-lookup"><span data-stu-id="e7058-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="e7058-108">Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="e7058-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="e7058-109">A parte 5 adiciona alguma lógica de negócios.</span><span class="sxs-lookup"><span data-stu-id="e7058-109">Part 5 adds some business logic.</span></span>

## <a id="_Toc260221671"></a><span data-ttu-id="e7058-110">Adicionando alguma lógica de negócios</span><span class="sxs-lookup"><span data-stu-id="e7058-110">Adding Some Business Logic</span></span>

<span data-ttu-id="e7058-111">Queremos que nossa experiência de compra esteja disponível sempre que alguém visitar nosso site.</span><span class="sxs-lookup"><span data-stu-id="e7058-111">We want our shopping experience to be available whenever someone visits our web site.</span></span> <span data-ttu-id="e7058-112">Os visitantes poderão procurar e adicionar itens ao carrinho de compras, mesmo que não estejam registrados ou conectados.</span><span class="sxs-lookup"><span data-stu-id="e7058-112">Visitors will be able to browse and add items to the shopping cart even if they are not registered or logged in.</span></span> <span data-ttu-id="e7058-113">Quando estiverem prontos para fazer check-out, eles terão a opção de autenticar e, se ainda não forem membros, eles poderão criar uma conta.</span><span class="sxs-lookup"><span data-stu-id="e7058-113">When they are ready to check out they will be given the option to authenticate and if they are not yet members they will be able to create an account.</span></span>

<span data-ttu-id="e7058-114">Isso significa que precisaremos implementar a lógica para converter o carrinho de compras de um estado anônimo para um estado de "usuário registrado".</span><span class="sxs-lookup"><span data-stu-id="e7058-114">This means that we will need to implement the logic to convert the shopping cart from an anonymous state to a "Registered User" state.</span></span>

<span data-ttu-id="e7058-115">Vamos criar um diretório chamado "classes" e clicar com o botão direito do mouse na pasta e criar um novo arquivo de "classe" chamado MyShoppingCart.cs</span><span class="sxs-lookup"><span data-stu-id="e7058-115">Let's create a directory named "Classes" then Right-Click on the folder and create a new "Class" file named MyShoppingCart.cs</span></span>

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

<span data-ttu-id="e7058-116">Como mencionado anteriormente, estenderemos a classe que implementa a página MyShoppingCart. aspx e faremos isso usando. Construção "classe parcial" da rede avançada.</span><span class="sxs-lookup"><span data-stu-id="e7058-116">As previously mentioned we will be extending the class that implements the MyShoppingCart.aspx page and we will do this using .NET's powerful "Partial Class" construct.</span></span>

<span data-ttu-id="e7058-117">A chamada gerada para o nosso arquivo MyShoppingCart.aspx.cf tem esta aparência.</span><span class="sxs-lookup"><span data-stu-id="e7058-117">The generated call for our MyShoppingCart.aspx.cf file looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

<span data-ttu-id="e7058-118">Observe o uso da palavra-chave "partial".</span><span class="sxs-lookup"><span data-stu-id="e7058-118">Note the use of the "partial" keyword.</span></span>

<span data-ttu-id="e7058-119">O arquivo de classe que acabamos de gerar é semelhante a este.</span><span class="sxs-lookup"><span data-stu-id="e7058-119">The class file that we just generated looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

<span data-ttu-id="e7058-120">Mesclaremos nossas implementações adicionando a palavra-chave Partial a esse arquivo também.</span><span class="sxs-lookup"><span data-stu-id="e7058-120">We will merge our implementations by adding the partial keyword to this file as well.</span></span>

<span data-ttu-id="e7058-121">Nosso novo arquivo de classe agora tem esta aparência.</span><span class="sxs-lookup"><span data-stu-id="e7058-121">Our new class file now looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

<span data-ttu-id="e7058-122">O primeiro método que adicionaremos à nossa classe é o método "AddItem".</span><span class="sxs-lookup"><span data-stu-id="e7058-122">The first method that we will add to our class is the "AddItem" method.</span></span> <span data-ttu-id="e7058-123">Esse é o método que será finalmente chamado quando o usuário clicar nos links "adicionar à arte" na lista de produtos e nas páginas de detalhes do produto.</span><span class="sxs-lookup"><span data-stu-id="e7058-123">This is the method that will ultimately be called when the user clicks on the "Add to Art" links on the Product List and Product Details pages.</span></span>

<span data-ttu-id="e7058-124">Acrescente o seguinte às instruções using na parte superior da página.</span><span class="sxs-lookup"><span data-stu-id="e7058-124">Append the following to the using statements at the top of the page.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

<span data-ttu-id="e7058-125">E adicione esse método à classe MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="e7058-125">And add this method to the MyShoppingCart class.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

<span data-ttu-id="e7058-126">Estamos usando LINQ to Entities para ver se o item já está no carrinho.</span><span class="sxs-lookup"><span data-stu-id="e7058-126">We are using LINQ to Entities to see if the item is already in the cart.</span></span> <span data-ttu-id="e7058-127">Nesse caso, atualizamos a quantidade de pedidos do item; caso contrário, criamos uma nova entrada para o item selecionado</span><span class="sxs-lookup"><span data-stu-id="e7058-127">If so, we update the order quantity of the item, otherwise we create a new entry for the selected item</span></span>

<span data-ttu-id="e7058-128">Para chamar esse método, implementaremos uma página AddToCart. aspx que não apenas faz a classe desse método, mas, em seguida, exibia o atual shopping a = carrinho após o item ter sido adicionado.</span><span class="sxs-lookup"><span data-stu-id="e7058-128">In order to call this method we will implement an AddToCart.aspx page that not only class this method but then displayed the current shopping a=cart after the item has been added.</span></span>

<span data-ttu-id="e7058-129">Clique com o botão direito do mouse no nome da solução no Gerenciador de soluções e adicione uma nova página chamada AddToCart. aspx como fizemos anteriormente.</span><span class="sxs-lookup"><span data-stu-id="e7058-129">Right-Click on the solution name in the solution explorer and add and new page named AddToCart.aspx as we have done previously.</span></span>

<span data-ttu-id="e7058-130">Embora possamos usar essa página para exibir resultados provisórios, como problemas de estoque baixo, etc. em nossa implementação, a página não será realmente renderizada, mas, em vez disso, chamamos a lógica de "adição" e o redirecionamento.</span><span class="sxs-lookup"><span data-stu-id="e7058-130">While we could use this page to display interim results like low stock issues, etc, in our implementation, the page will not actually render, but rather call the "Add" logic and redirect.</span></span>

<span data-ttu-id="e7058-131">Para fazer isso, adicionaremos o código a seguir à página\_evento de carregamento.</span><span class="sxs-lookup"><span data-stu-id="e7058-131">To accomplish this we'll add the following code to the Page\_Load event.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

<span data-ttu-id="e7058-132">Observe que estamos recuperando o produto para adicionar ao carrinho de compras de um parâmetro QueryString e chamar o método AddItem de nossa classe.</span><span class="sxs-lookup"><span data-stu-id="e7058-132">Note that we are retrieving the product to add to the shopping cart from a QueryString parameter and calling the AddItem method of our class.</span></span>

<span data-ttu-id="e7058-133">Supondo que nenhum erro seja encontrado, o controle é passado para a página SHoppingCart. aspx, que implementaremos totalmente a seguir.</span><span class="sxs-lookup"><span data-stu-id="e7058-133">Assuming no errors are encountered control is passed to the SHoppingCart.aspx page which we will fully implement next.</span></span> <span data-ttu-id="e7058-134">Se houver um erro, lançaremos uma exceção.</span><span class="sxs-lookup"><span data-stu-id="e7058-134">If there should be an error we throw an exception.</span></span>

<span data-ttu-id="e7058-135">No momento, ainda não implementamos um manipulador de erro global, portanto, essa exceção não seria tratada pelo nosso aplicativo, mas corrigiremos isso em breve.</span><span class="sxs-lookup"><span data-stu-id="e7058-135">Currently we have not yet implemented a global error handler so this exception would go unhandled by our application but we will remedy this shortly.</span></span>

<span data-ttu-id="e7058-136">Observe também o uso da instrução Debug. Fail () (disponível via `using System.Diagnostics;)`</span><span class="sxs-lookup"><span data-stu-id="e7058-136">Note also the use of the statement Debug.Fail() (available via `using System.Diagnostics;)`</span></span>

<span data-ttu-id="e7058-137">O aplicativo está sendo executado dentro do depurador, esse método exibirá uma caixa de diálogo detalhada com informações sobre o estado dos aplicativos junto com a mensagem de erro que especificamos.</span><span class="sxs-lookup"><span data-stu-id="e7058-137">Is the application is running inside the debugger, this method will display a detailed dialog with information about the applications state along with the error message that we specify.</span></span>

<span data-ttu-id="e7058-138">Durante a execução na produção, a instrução Debug. Fail () é ignorada.</span><span class="sxs-lookup"><span data-stu-id="e7058-138">When running in production the Debug.Fail() statement is ignored.</span></span>

<span data-ttu-id="e7058-139">Você notará no código acima uma chamada para um método em nossos nomes de classe de carrinho de compras "GetShoppingCartId".</span><span class="sxs-lookup"><span data-stu-id="e7058-139">You will note in the code above a call to a method in our shopping cart class names "GetShoppingCartId".</span></span>

<span data-ttu-id="e7058-140">Adicione o código para implementar o método da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="e7058-140">Add the code to implement the method as follows.</span></span>

<span data-ttu-id="e7058-141">Observe que também adicionamos os botões Update e checkout e um rótulo onde podemos exibir o carrinho "total".</span><span class="sxs-lookup"><span data-stu-id="e7058-141">Note that we've also added update and checkout buttons and a label where we can display the cart "total".</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

<span data-ttu-id="e7058-142">Agora podemos adicionar itens ao nosso carrinho de compras, mas não implementamos a lógica para exibir o carrinho após a adição de um produto.</span><span class="sxs-lookup"><span data-stu-id="e7058-142">We can now add items to our shopping cart but we have not implemented the logic to display the cart after a product has been added.</span></span>

<span data-ttu-id="e7058-143">Portanto, na página MyShoppingCart. aspx, adicionaremos um controle EntityDataSource e um controle GridVire da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="e7058-143">So, in the MyShoppingCart.aspx page we'll add an EntityDataSource control and a GridVire control as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

<span data-ttu-id="e7058-144">Chame o formulário no designer para que você possa clicar duas vezes no botão atualizar carrinho e gerar o manipulador de eventos de clique que é especificado na declaração na marcação.</span><span class="sxs-lookup"><span data-stu-id="e7058-144">Call up the form in the designer so that you can double click on the Update Cart button and generate the click event handler that is specified in the declaration in the markup.</span></span>

<span data-ttu-id="e7058-145">Vamos implementar os detalhes mais tarde, mas fazer isso nos permitirá criar e executar nosso aplicativo sem erros.</span><span class="sxs-lookup"><span data-stu-id="e7058-145">We'll implement the details later but doing this will let us build and run our application without errors.</span></span>

<span data-ttu-id="e7058-146">Ao executar o aplicativo e adicionar um item ao carrinho de compras, você verá isso.</span><span class="sxs-lookup"><span data-stu-id="e7058-146">When you run the application and add an item to the shopping cart you will see this.</span></span>

![](tailspin-spyworks-part-5/_static/image2.jpg)

<span data-ttu-id="e7058-147">Observe que desviamos da exibição de grade "padrão" Implementando três colunas personalizadas.</span><span class="sxs-lookup"><span data-stu-id="e7058-147">Note that we have deviated from the "default" grid display by implementing three custom columns.</span></span>

<span data-ttu-id="e7058-148">O primeiro é um campo editável, "associado" para a quantidade:</span><span class="sxs-lookup"><span data-stu-id="e7058-148">The first is an Editable, "Bound" field for the Quantity:</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

<span data-ttu-id="e7058-149">A seguir está uma coluna "calculada" que exibe o total do item de linha (o custo do item vezes a quantidade a ser ordenada):</span><span class="sxs-lookup"><span data-stu-id="e7058-149">The next is a "calculated" column that displays the line item total (the item cost times the quantity to be ordered):</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

<span data-ttu-id="e7058-150">Por fim, temos uma coluna personalizada que contém um controle de caixa de seleção que o usuário usará para indicar que o item deve ser removido do gráfico de compras.</span><span class="sxs-lookup"><span data-stu-id="e7058-150">Lastly we have a custom column that contains a CheckBox control that the user will use to indicate that the item should be removed from the shopping chart.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

<span data-ttu-id="e7058-151">Como você pode ver, a linha total do pedido está vazia, então vamos adicionar uma lógica para calcular o total do pedido.</span><span class="sxs-lookup"><span data-stu-id="e7058-151">As you can see, the Order Total line is empty so let's add some logic to calculate the Order Total.</span></span>

<span data-ttu-id="e7058-152">Primeiro, implementaremos um método "GetTotal" para nossa classe MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="e7058-152">We'll first implement a "GetTotal" method to our MyShoppingCart Class.</span></span>

<span data-ttu-id="e7058-153">No arquivo MyShoppingCart.cs, adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="e7058-153">In the MyShoppingCart.cs file add the following code.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

<span data-ttu-id="e7058-154">Em seguida, na página\_carregar manipulador de eventos, poderemos chamar nosso método GetTotal.</span><span class="sxs-lookup"><span data-stu-id="e7058-154">Then in the Page\_Load event handler we'll can call our GetTotal method.</span></span> <span data-ttu-id="e7058-155">Ao mesmo tempo, vamos adicionar um teste para ver se o carrinho de compras está vazio e ajustar a exibição adequadamente, se for.</span><span class="sxs-lookup"><span data-stu-id="e7058-155">At the same time we'll add a test to see if the shopping cart is empty and adjust the display accordingly if it is.</span></span>

<span data-ttu-id="e7058-156">Agora, se o carrinho de compras estiver vazio, obteremos:</span><span class="sxs-lookup"><span data-stu-id="e7058-156">Now if the shopping cart is empty we get this:</span></span>

![](tailspin-spyworks-part-5/_static/image4.jpg)

<span data-ttu-id="e7058-157">E, se não, veremos nosso total.</span><span class="sxs-lookup"><span data-stu-id="e7058-157">And if not, we see our total.</span></span>

![](tailspin-spyworks-part-5/_static/image5.jpg)

<span data-ttu-id="e7058-158">No entanto, essa página ainda não foi concluída.</span><span class="sxs-lookup"><span data-stu-id="e7058-158">However, this page is not yet complete.</span></span>

<span data-ttu-id="e7058-159">Precisaremos de uma lógica adicional para recalcular o carrinho de compras removendo itens marcados para remoção e determinando novos valores de quantidade, já que alguns podem ter sido alterados na grade pelo usuário.</span><span class="sxs-lookup"><span data-stu-id="e7058-159">We will need additional logic to recalculate the shopping cart by removing items marked for removal and by determining new quantity values as some may have been changed in the grid by the user.</span></span>

<span data-ttu-id="e7058-160">Permite adicionar um método "RemoveItem" à nossa classe de carrinho de compras em MyShoppingCart.cs para lidar com o caso quando um usuário marca um item para remoção.</span><span class="sxs-lookup"><span data-stu-id="e7058-160">Lets add a "RemoveItem" method to our shopping cart class in MyShoppingCart.cs to handle the case when a user marks an item for removal.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

<span data-ttu-id="e7058-161">Agora, vamos ao AD um método para lidar com a circunstância quando um usuário simplesmente altera a qualidade a ser ordenada no GridView.</span><span class="sxs-lookup"><span data-stu-id="e7058-161">Now let's ad a method to handle the circumstance when a user simply changes the quality to be ordered in the GridView.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

<span data-ttu-id="e7058-162">Com os recursos básicos de remoção e atualização em vigor, podemos implementar a lógica que realmente atualiza o carrinho de compras no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e7058-162">With the basic Remove and Update features in place we can implement the logic that actually updates the shopping cart in the database.</span></span> <span data-ttu-id="e7058-163">(Em MyShoppingCart.cs)</span><span class="sxs-lookup"><span data-stu-id="e7058-163">(In MyShoppingCart.cs)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

<span data-ttu-id="e7058-164">Você observará que esse método espera dois parâmetros.</span><span class="sxs-lookup"><span data-stu-id="e7058-164">You'll note that this method expects two parameters.</span></span> <span data-ttu-id="e7058-165">Uma é a ID do carrinho de compras e a outra é uma matriz de objetos do tipo definido pelo usuário.</span><span class="sxs-lookup"><span data-stu-id="e7058-165">One is the shopping cart Id and the other is an array of objects of user defined type.</span></span>

<span data-ttu-id="e7058-166">Portanto, para minimizar a dependência de nossa lógica em especificidades de interface do usuário, definimos uma estrutura de dados que podemos usar para passar os itens do carrinho de compras para nosso código sem que nosso método precise acessar diretamente o controle GridView.</span><span class="sxs-lookup"><span data-stu-id="e7058-166">So as to minimize the dependency of our logic on user interface specifics, we've defined a data structure that we can use to pass the shopping cart items to our code without our method needing to directly access the GridView control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

<span data-ttu-id="e7058-167">Em nosso arquivo MyShoppingCart.aspx.cs, podemos usar essa estrutura em nosso manipulador de eventos de clique de botão de atualização da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="e7058-167">In our MyShoppingCart.aspx.cs file we can use this structure in our Update Button Click Event handler as follows.</span></span> <span data-ttu-id="e7058-168">Observe que, além de atualizar o carrinho, atualizaremos o total do carrinho também.</span><span class="sxs-lookup"><span data-stu-id="e7058-168">Note that in addition to updating the cart we will update the cart total as well.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

<span data-ttu-id="e7058-169">Observação com interesse específico esta linha de código:</span><span class="sxs-lookup"><span data-stu-id="e7058-169">Note with particular interest this line of code:</span></span>

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

<span data-ttu-id="e7058-170">GetValues () é uma função auxiliar especial que iremos implementar em MyShoppingCart.aspx.cs da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="e7058-170">GetValues() is a special helper function that we will implement in MyShoppingCart.aspx.cs as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

<span data-ttu-id="e7058-171">Isso fornece uma maneira limpa de acessar os valores dos elementos associados em nosso controle GridView.</span><span class="sxs-lookup"><span data-stu-id="e7058-171">This provides a clean way to access the values of the bound elements in our GridView control.</span></span> <span data-ttu-id="e7058-172">Como nosso controle de caixa de seleção "Remover item" não está associado, acessaremos por meio do método FindControl ().</span><span class="sxs-lookup"><span data-stu-id="e7058-172">Since our "Remove Item" CheckBox Control is not bound we'll access it via the FindControl() method.</span></span>

<span data-ttu-id="e7058-173">Neste estágio do desenvolvimento do projeto, estamos preparando para implementar o processo de check-out.</span><span class="sxs-lookup"><span data-stu-id="e7058-173">At this stage in your project's development we are getting ready to implement the checkout process.</span></span>

<span data-ttu-id="e7058-174">Antes de fazer isso, vamos usar o Visual Studio para gerar o banco de dados de associação e adicionar um usuário ao repositório de associação.</span><span class="sxs-lookup"><span data-stu-id="e7058-174">Before doing so let's use Visual Studio to generate the membership database and add a user to the membership repository.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e7058-175">[Anterior](tailspin-spyworks-part-4.md)
> [Próximo](tailspin-spyworks-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="e7058-175">[Previous](tailspin-spyworks-part-4.md)
[Next](tailspin-spyworks-part-6.md)</span></span>
