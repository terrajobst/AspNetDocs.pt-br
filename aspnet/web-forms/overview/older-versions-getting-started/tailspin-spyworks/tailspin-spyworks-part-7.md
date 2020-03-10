---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: 'Parte 7: adicionando recursos | Microsoft Docs'
author: JoeStagner
description: Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo Tailspin Spyworks. A parte 7 adiciona recursos adicionais, como a conta Revie...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: ffd2b862c727db9572c272b7b21bcc33c822fffa
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78641991"
---
# <a name="part-7-adding-features"></a><span data-ttu-id="76ce1-104">Parte 7: adicionando recursos</span><span class="sxs-lookup"><span data-stu-id="76ce1-104">Part 7: Adding Features</span></span>

<span data-ttu-id="76ce1-105">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="76ce1-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="76ce1-106">A Tailspin Spyworks demonstra como é extremamente simples criar aplicativos poderosos e escalonáveis para a plataforma .NET.</span><span class="sxs-lookup"><span data-stu-id="76ce1-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="76ce1-107">Ele mostra como usar os ótimos novos recursos do ASP.NET 4 para criar uma loja online, incluindo compras, check-out e administração.</span><span class="sxs-lookup"><span data-stu-id="76ce1-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="76ce1-108">Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="76ce1-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="76ce1-109">A parte 7 adiciona recursos adicionais, como revisão de conta, revisões de produtos e controles de usuário "itens populares" e "também comprados".</span><span class="sxs-lookup"><span data-stu-id="76ce1-109">Part 7 adds additional features, such as account review, product reviews, and "popular items" and "also purchased" user controls.</span></span>

## <a id="_Toc260221673"></a><span data-ttu-id="76ce1-110">Adicionando recursos</span><span class="sxs-lookup"><span data-stu-id="76ce1-110">Adding Features</span></span>

<span data-ttu-id="76ce1-111">Embora os usuários possam navegar em nosso catálogo, inserir itens em seu carrinho de compras e concluir o processo de check-out, há vários recursos de suporte que incluiremos para melhorar nosso site.</span><span class="sxs-lookup"><span data-stu-id="76ce1-111">Though users can browse our catalog, place items in their shopping cart, and complete the checkout process, there are a number of supporting features that we will include to improve our site.</span></span>

1. <span data-ttu-id="76ce1-112">Revisão da conta (listar pedidos colocados e exibir detalhes.)</span><span class="sxs-lookup"><span data-stu-id="76ce1-112">Account Review (List orders placed and view details.)</span></span>
2. <span data-ttu-id="76ce1-113">Adicione um conteúdo específico de contexto à página inicial.</span><span class="sxs-lookup"><span data-stu-id="76ce1-113">Add some context specific content to the front page.</span></span>
3. <span data-ttu-id="76ce1-114">Adicione um recurso para permitir que os usuários examinem os produtos no catálogo.</span><span class="sxs-lookup"><span data-stu-id="76ce1-114">Add a feature to let users Review the products in the catalog.</span></span>
4. <span data-ttu-id="76ce1-115">Crie um controle de usuário para exibir itens populares e coloque esse controle na página inicial.</span><span class="sxs-lookup"><span data-stu-id="76ce1-115">Create a User Control to display Popular Items and Place that control on the front page.</span></span>
5. <span data-ttu-id="76ce1-116">Crie um controle de usuário "também adquirido" e adicione-o à página de detalhes do produto.</span><span class="sxs-lookup"><span data-stu-id="76ce1-116">Create an "Also Purchased" user control and add it to the product details page.</span></span>
6. <span data-ttu-id="76ce1-117">Adicione uma página de contato.</span><span class="sxs-lookup"><span data-stu-id="76ce1-117">Add a Contact Page.</span></span>
7. <span data-ttu-id="76ce1-118">Adicione uma página About.</span><span class="sxs-lookup"><span data-stu-id="76ce1-118">Add an About Page.</span></span>
8. <span data-ttu-id="76ce1-119">Erro global</span><span class="sxs-lookup"><span data-stu-id="76ce1-119">Global Error</span></span>

## <a id="_Toc260221674"></a><span data-ttu-id="76ce1-120">Revisão da conta</span><span class="sxs-lookup"><span data-stu-id="76ce1-120">Account Review</span></span>

<span data-ttu-id="76ce1-121">Na pasta "Account", crie duas páginas. aspx, uma denominada OrderList. aspx e a outra chamada OrderDetails. aspx</span><span class="sxs-lookup"><span data-stu-id="76ce1-121">In the "Account" folder create two .aspx pages one named OrderList.aspx and the other named OrderDetails.aspx</span></span>

<span data-ttu-id="76ce1-122">OrderList. aspx aproveitará os controles GridView e EntityDataSource da mesma forma que antes.</span><span class="sxs-lookup"><span data-stu-id="76ce1-122">OrderList.aspx will leverage the GridView and EntityDataSource controls much as we have previously.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

<span data-ttu-id="76ce1-123">A EntityDataSource seleciona registros da tabela Orders filtrados no nome de usuário (consulte o WhereParameter) que definimos em uma variável de sessão quando o usuário faz logon.</span><span class="sxs-lookup"><span data-stu-id="76ce1-123">The EntityDataSource selects records from the Orders table filtered on the UserName (see the WhereParameter) which we set in a session variable when the user log's in.</span></span>

<span data-ttu-id="76ce1-124">Observe também esses parâmetros no HyperlinkField do GridView:</span><span class="sxs-lookup"><span data-stu-id="76ce1-124">Note also these parameters in the HyperlinkField of the GridView:</span></span>

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

<span data-ttu-id="76ce1-125">Eles especificam o link para a exibição detalhes do pedido para cada produto especificando o campo OrderID como um parâmetro QueryString para a página OrderDetails. aspx.</span><span class="sxs-lookup"><span data-stu-id="76ce1-125">These specify the link to the Order details view for each product specifying the OrderID field as a QueryString parameter to the OrderDetails.aspx page.</span></span>

## <a id="_Toc260221675"></a><span data-ttu-id="76ce1-126">OrderDetails. aspx</span><span class="sxs-lookup"><span data-stu-id="76ce1-126">OrderDetails.aspx</span></span>

<span data-ttu-id="76ce1-127">Usaremos um controle de EntityDataSource para acessar os pedidos e um FormView para exibir os dados do pedido e outra EntityDataSource com um GridView para exibir todos os itens de linha da ordem.</span><span class="sxs-lookup"><span data-stu-id="76ce1-127">We will use an EntityDataSource control to access the Orders and a FormView to display the Order data and another EntityDataSource with a GridView to display all the Order's line items.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

<span data-ttu-id="76ce1-128">No arquivo code-behind (OrderDetails.aspx.cs), temos dois pequenos bits de manutenção.</span><span class="sxs-lookup"><span data-stu-id="76ce1-128">In the Code Behind file (OrderDetails.aspx.cs) we have two little bits of housekeeping.</span></span>

<span data-ttu-id="76ce1-129">Primeiro, precisamos garantir que OrderDetails sempre obtenha um OrderId.</span><span class="sxs-lookup"><span data-stu-id="76ce1-129">First we need to make sure that OrderDetails always gets an OrderId.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

<span data-ttu-id="76ce1-130">Também precisamos calcular e exibir o total do pedido dos itens de linha.</span><span class="sxs-lookup"><span data-stu-id="76ce1-130">We also need to calculate and display the order total from the line items.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a><span data-ttu-id="76ce1-131">A Home Page</span><span class="sxs-lookup"><span data-stu-id="76ce1-131">The Home Page</span></span>

<span data-ttu-id="76ce1-132">Vamos adicionar um conteúdo estático à página default. aspx.</span><span class="sxs-lookup"><span data-stu-id="76ce1-132">Let's add some static content to the Default.aspx page.</span></span>

<span data-ttu-id="76ce1-133">Primeiro, criarei uma pasta "content" e dentro dela uma pasta images (e incluirei uma imagem a ser usada na home page.)</span><span class="sxs-lookup"><span data-stu-id="76ce1-133">First I'll create a "Content" folder and within it an Images folder (and I'll include an image to be used on the home page.)</span></span>

<span data-ttu-id="76ce1-134">No espaço reservado inferior da página default. aspx, adicione a marcação a seguir.</span><span class="sxs-lookup"><span data-stu-id="76ce1-134">Into the bottom placeholder of the Default.aspx page, add the following markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a><span data-ttu-id="76ce1-135">Análises de produtos</span><span class="sxs-lookup"><span data-stu-id="76ce1-135">Product Reviews</span></span>

<span data-ttu-id="76ce1-136">Primeiro, adicionaremos um botão com um link para um formulário que podemos usar para inserir uma revisão do produto.</span><span class="sxs-lookup"><span data-stu-id="76ce1-136">First we'll add a button with a link to a form that we can use to enter a product review.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

<span data-ttu-id="76ce1-137">Observe que estamos passando o ProductID na cadeia de caracteres de consulta</span><span class="sxs-lookup"><span data-stu-id="76ce1-137">Note that we are passing the ProductID in the query string</span></span>

<span data-ttu-id="76ce1-138">Em seguida, vamos adicionar a página chamada ReviewAdd. aspx</span><span class="sxs-lookup"><span data-stu-id="76ce1-138">Next let's add page named ReviewAdd.aspx</span></span>

<span data-ttu-id="76ce1-139">Esta página usará o kit de ferramentas de controle AJAX ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="76ce1-139">This page will use the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="76ce1-140">Se ainda não tiver feito isso, você poderá baixá-lo em [DevExpress](http://devexpress.com/act) e há diretrizes sobre como configurar o kit de ferramentas para uso com o Visual Studio aqui [https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span><span class="sxs-lookup"><span data-stu-id="76ce1-140">If you have not already done so you can download it from [DevExpress](http://devexpress.com/act) and there is guidance on setting up the toolkit for use with Visual Studio here [https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span></span>

<span data-ttu-id="76ce1-141">No modo de design, arraste controles e validadores da caixa de ferramentas e crie um formulário como o mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="76ce1-141">In design mode, drag controls and validators from the toolbox and build a form like the one below.</span></span>

![](tailspin-spyworks-part-7/_static/image2.jpg)

<span data-ttu-id="76ce1-142">A marcação terá uma aparência semelhante a esta.</span><span class="sxs-lookup"><span data-stu-id="76ce1-142">The markup will look something like this.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

<span data-ttu-id="76ce1-143">Agora que podemos inserir as revisões, vamos exibir essas revisões na página do produto.</span><span class="sxs-lookup"><span data-stu-id="76ce1-143">Now that we can enter reviews, lets display those reviews on the product page.</span></span>

<span data-ttu-id="76ce1-144">Adicione essa marcação à página ProductDetails. aspx.</span><span class="sxs-lookup"><span data-stu-id="76ce1-144">Add this markup to the ProductDetails.aspx page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

<span data-ttu-id="76ce1-145">Executar nosso aplicativo agora e navegar até um produto mostra as informações do produto, incluindo as revisões do cliente.</span><span class="sxs-lookup"><span data-stu-id="76ce1-145">Running our application now and navigating to a product shows the product information including customer reviews.</span></span>

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a><span data-ttu-id="76ce1-146">Controle de itens populares (criando controles de usuário)</span><span class="sxs-lookup"><span data-stu-id="76ce1-146">Popular Items Control (Creating User Controls)</span></span>

<span data-ttu-id="76ce1-147">Para aumentar as vendas em seu site, adicionaremos alguns recursos à "venda sugerida" de produtos populares ou relacionados.</span><span class="sxs-lookup"><span data-stu-id="76ce1-147">In order to increase sales on your web site we will add a couple of features to "suggestive sell" popular or related products.</span></span>

<span data-ttu-id="76ce1-148">O primeiro desses recursos será uma lista do produto mais popular em nosso catálogo de produtos.</span><span class="sxs-lookup"><span data-stu-id="76ce1-148">The first of these features will be a list of the more popular product in our product catalog.</span></span>

<span data-ttu-id="76ce1-149">Criaremos um "controle de usuário" para exibir os principais itens de vendas na home page de nosso aplicativo.</span><span class="sxs-lookup"><span data-stu-id="76ce1-149">We will create a "User Control" to display the top selling items on the home page of our application.</span></span> <span data-ttu-id="76ce1-150">Como isso será um controle, podemos usá-lo em qualquer página simplesmente arrastando e soltando o controle no designer do Visual Studio em qualquer página que desejar.</span><span class="sxs-lookup"><span data-stu-id="76ce1-150">Since this will be a control, we can use it on any page by simply dragging and dropping the control in Visual Studio's designer onto any page that we like.</span></span>

<span data-ttu-id="76ce1-151">No Gerenciador de soluções do Visual Studio, clique com o botão direito do mouse no nome da solução e crie um novo diretório chamado "controles".</span><span class="sxs-lookup"><span data-stu-id="76ce1-151">In Visual Studio's solutions explorer, right-click on the solution name and create a new directory named "Controls".</span></span> <span data-ttu-id="76ce1-152">Embora não seja necessário fazer isso, ajudaremos a manter nosso projeto organizado pela criação de todos os controles de usuário no diretório "Controls".</span><span class="sxs-lookup"><span data-stu-id="76ce1-152">While it is not necessary to do so, we will help keep our project organized by creating all our user controls in the "Controls" directory.</span></span>

<span data-ttu-id="76ce1-153">Clique com o botão direito do mouse na pasta controles e escolha "novo item":</span><span class="sxs-lookup"><span data-stu-id="76ce1-153">Right-click on the controls folder and choose "New Item" :</span></span>

![](tailspin-spyworks-part-7/_static/image4.jpg)

<span data-ttu-id="76ce1-154">Especifique um nome para nosso controle de "PopularItems".</span><span class="sxs-lookup"><span data-stu-id="76ce1-154">Specify a name for our control of "PopularItems".</span></span> <span data-ttu-id="76ce1-155">Observe que a extensão de arquivo para controles de usuário é. ascx não. aspx.</span><span class="sxs-lookup"><span data-stu-id="76ce1-155">Note that the file extension for user controls is .ascx not .aspx.</span></span>

<span data-ttu-id="76ce1-156">Nosso controle de usuário itens populares será definido da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="76ce1-156">Our Popular Items User control will be defined as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

<span data-ttu-id="76ce1-157">Aqui, estamos usando um método que ainda não usamos neste aplicativo.</span><span class="sxs-lookup"><span data-stu-id="76ce1-157">Here we're using a method we have not used yet in this application.</span></span> <span data-ttu-id="76ce1-158">Estamos usando o controle Repeater e, em vez de usar um controle da fonte de dados, estamos associando o controle Repeater aos resultados de uma consulta LINQ to Entities.</span><span class="sxs-lookup"><span data-stu-id="76ce1-158">We're using the repeater control and instead of using a data source control we're binding the Repeater Control to the results of a LINQ to Entities query.</span></span>

<span data-ttu-id="76ce1-159">No code-behind do nosso controle, fazemos isso da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="76ce1-159">In the code behind of our control we do that as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

<span data-ttu-id="76ce1-160">Observe também essa linha importante na parte superior da marcação do nosso controle.</span><span class="sxs-lookup"><span data-stu-id="76ce1-160">Note also this important line at the top of our control's markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

<span data-ttu-id="76ce1-161">Como os itens mais populares não serão alterados em uma base de minuto a minuto, podemos adicionar uma diretiva aching para melhorar o desempenho de nosso aplicativo.</span><span class="sxs-lookup"><span data-stu-id="76ce1-161">Since the most popular items won't be changing on a minute to minute basis we can add a aching directive to improve the performance of our application.</span></span> <span data-ttu-id="76ce1-162">Essa diretiva fará com que o código de controles seja executado somente quando a saída armazenada em cache do controle expirar.</span><span class="sxs-lookup"><span data-stu-id="76ce1-162">This directive will cause the controls code to only be executed when the cached output of the control expires.</span></span> <span data-ttu-id="76ce1-163">Caso contrário, a versão em cache da saída do controle será usada.</span><span class="sxs-lookup"><span data-stu-id="76ce1-163">Otherwise, the cached version of the control's output will be used.</span></span>

<span data-ttu-id="76ce1-164">Agora, tudo o que precisamos fazer é incluir nosso novo controle em nossa página default. aspx.</span><span class="sxs-lookup"><span data-stu-id="76ce1-164">Now all we have to do is include our new control in our Default.aspx page.</span></span>

<span data-ttu-id="76ce1-165">Use o recurso arrastar e soltar para colocar uma instância do controle na coluna abrir do formulário padrão.</span><span class="sxs-lookup"><span data-stu-id="76ce1-165">Use drag and drop to place an instance of the control in the open column of our Default form.</span></span>

![](tailspin-spyworks-part-7/_static/image5.jpg)

<span data-ttu-id="76ce1-166">Agora, quando executamos nosso aplicativo, o home page exibe os itens mais populares.</span><span class="sxs-lookup"><span data-stu-id="76ce1-166">Now when we run our application the home page displays the most popular items.</span></span>

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a><span data-ttu-id="76ce1-167">Controle "também adquirido" (controles de usuário com parâmetros)</span><span class="sxs-lookup"><span data-stu-id="76ce1-167">"Also Purchased" Control (User Controls with Parameters)</span></span>

<span data-ttu-id="76ce1-168">O segundo controle de usuário que criaremos fará a venda sugerida para o próximo nível adicionando a especificidade do contexto.</span><span class="sxs-lookup"><span data-stu-id="76ce1-168">The second User Control that we'll create will take suggestive selling to the next level by adding context specificity.</span></span>

<span data-ttu-id="76ce1-169">A lógica para calcular os itens "também comprados" principais não é trivial.</span><span class="sxs-lookup"><span data-stu-id="76ce1-169">The logic to calculate the top "Also Purchased" items is non-trivial.</span></span>

<span data-ttu-id="76ce1-170">Nosso controle "também adquirido" selecionará os registros de OrderDetails (anteriormente adquiridos) para o ProductID selecionado no momento e pegará o OrderID para cada ordem exclusiva que for encontrada.</span><span class="sxs-lookup"><span data-stu-id="76ce1-170">Our "Also Purchased" control will select the OrderDetails records (previously purchased) for the currently selected ProductID and grab the OrderIDs for each unique order that is found.</span></span>

<span data-ttu-id="76ce1-171">Em seguida, selecionaremos os produtos de todas essas ordens e Somaremos as quantidades compradas.</span><span class="sxs-lookup"><span data-stu-id="76ce1-171">Then we will select al the products from all those Orders and sum the quantities purchased.</span></span> <span data-ttu-id="76ce1-172">Classificaremos os produtos por essa quantidade Sum e exibiremos os cinco primeiros itens.</span><span class="sxs-lookup"><span data-stu-id="76ce1-172">We'll sort the products by that quantity sum and display the top five items.</span></span>

<span data-ttu-id="76ce1-173">Dada a complexidade dessa lógica, implementaremos esse algoritmo como um procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="76ce1-173">Given the complexity of this logic, we will implement this algorithm as a stored procedure.</span></span>

<span data-ttu-id="76ce1-174">O T-SQL para o procedimento armazenado é o seguinte.</span><span class="sxs-lookup"><span data-stu-id="76ce1-174">The T-SQL for the stored procedure is as follows.</span></span>

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

<span data-ttu-id="76ce1-175">Observe que esse procedimento armazenado (SelectPurchasedWithProducts) existia no banco de dados quando o incluímos em nosso aplicativo e quando geramos o Modelo de Dados de Entidade que especificamos que, além das tabelas e exibições que precisávamos, o Modelo de Dados de Entidade deve incluir esse procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="76ce1-175">Note that this stored procedure (SelectPurchasedWithProducts) existed in the database when we included it in our application and when we generated the Entity Data Model we specified that, in addition to the Tables and Views that we needed, the Entity Data Model should include this stored procedure.</span></span>

<span data-ttu-id="76ce1-176">Para acessar o procedimento armazenado do Modelo de Dados de Entidade precisamos importar a função.</span><span class="sxs-lookup"><span data-stu-id="76ce1-176">To access the stored procedure from the Entity Data Model we need to import the function.</span></span>

<span data-ttu-id="76ce1-177">Clique duas vezes no Modelo de Dados de Entidade no Gerenciador de soluções para abri-lo no designer e abra o navegador de modelos, clique com o botão direito do mouse no designer e selecione "Adicionar importação de função".</span><span class="sxs-lookup"><span data-stu-id="76ce1-177">Double Click on the Entity Data Model in the Solutions Explorer to open it in the designer and open the Model Browser, then right-click in the designer and select "Add Function Import".</span></span>

![](tailspin-spyworks-part-7/_static/image1.png)

<span data-ttu-id="76ce1-178">Ao fazer isso, você abrirá essa caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="76ce1-178">Doing so will open this dialog.</span></span>

![](tailspin-spyworks-part-7/_static/image2.png)

<span data-ttu-id="76ce1-179">Preencha os campos como você vê acima, selecionando o "SelectPurchasedWithProducts" e use o nome do procedimento para o nome da nossa função importada.</span><span class="sxs-lookup"><span data-stu-id="76ce1-179">Fill out the fields as you see above, selecting the "SelectPurchasedWithProducts" and use the procedure name for the name of our imported function.</span></span>

<span data-ttu-id="76ce1-180">Clique em "OK".</span><span class="sxs-lookup"><span data-stu-id="76ce1-180">Click "Ok".</span></span>

<span data-ttu-id="76ce1-181">Tendo feito isso, podemos simplesmente programar em relação ao procedimento armazenado, pois podemos qualquer outro item no modelo.</span><span class="sxs-lookup"><span data-stu-id="76ce1-181">Having done this we can simply program against the stored procedure as we might any other item in the model.</span></span>

<span data-ttu-id="76ce1-182">Portanto, em nossa pasta "Controls", crie um novo controle de usuário chamado AlsoPurchased. ascx.</span><span class="sxs-lookup"><span data-stu-id="76ce1-182">So, in our "Controls" folder create a new user control named AlsoPurchased.ascx.</span></span>

<span data-ttu-id="76ce1-183">A marcação para esse controle parecerá muito familiar ao controle PopularItems.</span><span class="sxs-lookup"><span data-stu-id="76ce1-183">The markup for this control will look very familiar to the PopularItems control.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

<span data-ttu-id="76ce1-184">A diferença notável é que não estão armazenando em cache a saída, uma vez que o item a ser renderizado será diferente pelo produto.</span><span class="sxs-lookup"><span data-stu-id="76ce1-184">The notable difference is that are not caching the output since the item's to be rendered will differ by product.</span></span>

<span data-ttu-id="76ce1-185">O ProductId será uma "Propriedade" para o controle.</span><span class="sxs-lookup"><span data-stu-id="76ce1-185">The ProductId will be a "property" to the control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

<span data-ttu-id="76ce1-186">No manipulador de eventos PreRender do controle, eed fazer três coisas.</span><span class="sxs-lookup"><span data-stu-id="76ce1-186">In the control's PreRender event handler we eed to do three things.</span></span>

1. <span data-ttu-id="76ce1-187">Verifique se o ProductID está definido.</span><span class="sxs-lookup"><span data-stu-id="76ce1-187">Make sure the ProductID is set.</span></span>
2. <span data-ttu-id="76ce1-188">Veja se há produtos que foram comprados com o atual.</span><span class="sxs-lookup"><span data-stu-id="76ce1-188">See if there are any products that have been purchased with the current one.</span></span>
3. <span data-ttu-id="76ce1-189">Gere alguns itens conforme determinado em #2.</span><span class="sxs-lookup"><span data-stu-id="76ce1-189">Output some items as determined in #2.</span></span>

<span data-ttu-id="76ce1-190">Observe como é fácil chamar o procedimento armazenado por meio do modelo.</span><span class="sxs-lookup"><span data-stu-id="76ce1-190">Note how easy it is to call the stored procedure through the model.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

<span data-ttu-id="76ce1-191">Depois de determinar que há "também adquirido", podemos simplesmente associar o repetidor aos resultados retornados pela consulta.</span><span class="sxs-lookup"><span data-stu-id="76ce1-191">After determining that there ARE "also purchased" we can simply bind the repeater to the results returned by the query.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

<span data-ttu-id="76ce1-192">Se não houver nenhum item "também adquirido", simplesmente exibiremos outros itens populares de nosso catálogo.</span><span class="sxs-lookup"><span data-stu-id="76ce1-192">If there were not any "also purchased" items we'll simply display other popular items from our catalog.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

<span data-ttu-id="76ce1-193">Para exibir os itens "também adquiridos", abra a página ProductDetails. aspx e arraste o controle AlsoPurchased do Gerenciador de soluções para que ele apareça nessa posição na marcação.</span><span class="sxs-lookup"><span data-stu-id="76ce1-193">To view the "Also Purchased" items, open the ProductDetails.aspx page and drag the AlsoPurchased control from the Solutions Explorer so that it appears in this position in the markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

<span data-ttu-id="76ce1-194">Isso criará uma referência ao controle na parte superior da página ProductDetails.</span><span class="sxs-lookup"><span data-stu-id="76ce1-194">Doing so will create a reference to the control at the top of the ProductDetails page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

<span data-ttu-id="76ce1-195">Como o controle de usuário AlsoPurchased requer um número ProductId, definiremos a propriedade ProductID de nosso controle usando uma instrução eval em relação ao item do modelo de dados atual da página.</span><span class="sxs-lookup"><span data-stu-id="76ce1-195">Since the AlsoPurchased user control requires a ProductId number we will set the ProductID property of our control by using an Eval statement against the current data model item of the page.</span></span>

![](tailspin-spyworks-part-7/_static/image3.png)

<span data-ttu-id="76ce1-196">Quando criamos e executamos agora e navegamos para um produto, vemos os itens "também adquiridos".</span><span class="sxs-lookup"><span data-stu-id="76ce1-196">When we build and run now and browse to a product we see the "Also Purchased" items.</span></span>

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> <span data-ttu-id="76ce1-197">[Anterior](tailspin-spyworks-part-6.md)
> [Próximo](tailspin-spyworks-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="76ce1-197">[Previous](tailspin-spyworks-part-6.md)
[Next](tailspin-spyworks-part-8.md)</span></span>
