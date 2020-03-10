---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: 'Parte 4: adicionando uma exibição de administrador | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: 664aeb33031e933322886a6d6bdd989277e9fda2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556010"
---
# <a name="part-4-adding-an-admin-view"></a><span data-ttu-id="5f2d3-102">Parte 4: adicionando uma exibição de administrador</span><span class="sxs-lookup"><span data-stu-id="5f2d3-102">Part 4: Adding an Admin View</span></span>

<span data-ttu-id="5f2d3-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="5f2d3-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="5f2d3-104">Baixar projeto concluído</span><span class="sxs-lookup"><span data-stu-id="5f2d3-104">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a><span data-ttu-id="5f2d3-105">Adicionar uma exibição de administrador</span><span class="sxs-lookup"><span data-stu-id="5f2d3-105">Add an Admin View</span></span>

<span data-ttu-id="5f2d3-106">Agora, vamos voltar ao lado do cliente e adicionar uma página que pode consumir dados do controlador de administração.</span><span class="sxs-lookup"><span data-stu-id="5f2d3-106">Now we'll turn to the client side, and add a page that can consume data from the Admin controller.</span></span> <span data-ttu-id="5f2d3-107">A página permitirá que os usuários criem, editem ou excluam produtos, enviando solicitações AJAX para o controlador.</span><span class="sxs-lookup"><span data-stu-id="5f2d3-107">The page will allow users to create, edit, or delete products, by sending AJAX requests to the controller.</span></span>

<span data-ttu-id="5f2d3-108">Em Gerenciador de Soluções, expanda a pasta controladores e abra o arquivo chamado HomeController.cs.</span><span class="sxs-lookup"><span data-stu-id="5f2d3-108">In Solution Explorer, expand the Controllers folder and open the file named HomeController.cs.</span></span> <span data-ttu-id="5f2d3-109">Esse arquivo contém um controlador MVC.</span><span class="sxs-lookup"><span data-stu-id="5f2d3-109">This file contains an MVC controller.</span></span> <span data-ttu-id="5f2d3-110">Adicione um método chamado `Admin`:</span><span class="sxs-lookup"><span data-stu-id="5f2d3-110">Add a method named `Admin`:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

<span data-ttu-id="5f2d3-111">O método **HttpRouteUrl** cria o URI para a API da Web e armazenamos isso no recipiente de exibição para mais tarde.</span><span class="sxs-lookup"><span data-stu-id="5f2d3-111">The **HttpRouteUrl** method creates the URI to the web API, and we store this in the view bag for later.</span></span>

<span data-ttu-id="5f2d3-112">Em seguida, posicione o cursor de texto dentro do método de ação `Admin`, clique com o botão direito do mouse e selecione **Adicionar exibição**.</span><span class="sxs-lookup"><span data-stu-id="5f2d3-112">Next, position the text cursor within the `Admin` action method, then right-click and select **Add View**.</span></span> <span data-ttu-id="5f2d3-113">Isso abrirá a caixa de diálogo **Adicionar exibição** .</span><span class="sxs-lookup"><span data-stu-id="5f2d3-113">This will bring up the **Add View** dialog.</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

<span data-ttu-id="5f2d3-114">Na caixa de diálogo **Adicionar exibição** , nomeie o modo de exibição "admin".</span><span class="sxs-lookup"><span data-stu-id="5f2d3-114">In the **Add View** dialog, name the view "Admin".</span></span> <span data-ttu-id="5f2d3-115">Marque a caixa de seleção rotulada **criar uma exibição fortemente tipada**.</span><span class="sxs-lookup"><span data-stu-id="5f2d3-115">Select the check box labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="5f2d3-116">Em **classe de modelo**, selecione "produto (ProductStore. Models)".</span><span class="sxs-lookup"><span data-stu-id="5f2d3-116">Under **Model Class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="5f2d3-117">Deixe todas as outras opções como seus valores padrão.</span><span class="sxs-lookup"><span data-stu-id="5f2d3-117">Leave all the other options as their default values.</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

<span data-ttu-id="5f2d3-118">Clicar em **Adicionar** adiciona um arquivo chamado admin. cshtml em exibições/início.</span><span class="sxs-lookup"><span data-stu-id="5f2d3-118">Clicking **Add** adds a file named Admin.cshtml under Views/Home.</span></span> <span data-ttu-id="5f2d3-119">Abra este arquivo e adicione o seguinte HTML.</span><span class="sxs-lookup"><span data-stu-id="5f2d3-119">Open this file and add the following HTML.</span></span> <span data-ttu-id="5f2d3-120">Esse HTML define a estrutura da página, mas nenhuma funcionalidade está conectada ainda.</span><span class="sxs-lookup"><span data-stu-id="5f2d3-120">This HTML defines the structure of the page, but no functionality is wired up yet.</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a><span data-ttu-id="5f2d3-121">Criar um link para a página de administração</span><span class="sxs-lookup"><span data-stu-id="5f2d3-121">Create a Link to the Admin Page</span></span>

<span data-ttu-id="5f2d3-122">Em Gerenciador de Soluções, expanda a pasta exibições e expanda a pasta compartilhada.</span><span class="sxs-lookup"><span data-stu-id="5f2d3-122">In Solution Explorer, expand the Views folder and then expand the Shared folder.</span></span> <span data-ttu-id="5f2d3-123">Abra o arquivo chamado \_layout. cshtml.</span><span class="sxs-lookup"><span data-stu-id="5f2d3-123">Open the file named \_Layout.cshtml.</span></span> <span data-ttu-id="5f2d3-124">Localize o elemento **UL** com ID = "menu" e um link de ação para a exibição de administrador:</span><span class="sxs-lookup"><span data-stu-id="5f2d3-124">Locate the **ul** element with id = "menu", and an action link for the Admin view:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> <span data-ttu-id="5f2d3-125">No projeto de exemplo, fiz algumas outras alterações superficiais, como substituir a cadeia de caracteres "seu logotipo aqui".</span><span class="sxs-lookup"><span data-stu-id="5f2d3-125">In the sample project, I made a few other cosmetic changes, such as replacing the string "Your logo here".</span></span> <span data-ttu-id="5f2d3-126">Eles não afetam a funcionalidade do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5f2d3-126">These don't affect the functionality of the application.</span></span> <span data-ttu-id="5f2d3-127">Você pode baixar o projeto e comparar os arquivos.</span><span class="sxs-lookup"><span data-stu-id="5f2d3-127">You can download the project and compare the files.</span></span>

<span data-ttu-id="5f2d3-128">Execute o aplicativo e clique no link "admin" que aparece na parte superior do home page.</span><span class="sxs-lookup"><span data-stu-id="5f2d3-128">Run the application and click the "Admin" link that appears at the top of the home page.</span></span> <span data-ttu-id="5f2d3-129">A página de administração deve ser parecida com a seguinte:</span><span class="sxs-lookup"><span data-stu-id="5f2d3-129">The Admin page should look like the following:</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

<span data-ttu-id="5f2d3-130">No momento, a página não faz nada.</span><span class="sxs-lookup"><span data-stu-id="5f2d3-130">Right now, the page doesn't do anything.</span></span> <span data-ttu-id="5f2d3-131">Na próxima seção, usaremos knockout. js para criar uma interface do usuário dinâmica.</span><span class="sxs-lookup"><span data-stu-id="5f2d3-131">In the next section, we'll use Knockout.js to create a dynamic UI.</span></span>

## <a name="add-authorization"></a><span data-ttu-id="5f2d3-132">Adicionar autorização</span><span class="sxs-lookup"><span data-stu-id="5f2d3-132">Add Authorization</span></span>

<span data-ttu-id="5f2d3-133">A página de administração está acessível no momento para qualquer pessoa que visitar o site.</span><span class="sxs-lookup"><span data-stu-id="5f2d3-133">The Admin page is currently accessible to anyone visiting the site.</span></span> <span data-ttu-id="5f2d3-134">Vamos alterar isso para restringir a permissão aos administradores.</span><span class="sxs-lookup"><span data-stu-id="5f2d3-134">Let's change this to restrict permission to administrators.</span></span>

<span data-ttu-id="5f2d3-135">Comece adicionando uma função de "administrador" e um usuário administrador.</span><span class="sxs-lookup"><span data-stu-id="5f2d3-135">Start by adding an "Administrator" role and an administrator user.</span></span> <span data-ttu-id="5f2d3-136">Em Gerenciador de Soluções, expanda a pasta filtros e abra o arquivo chamado InitializeSimpleMembershipAttribute.cs.</span><span class="sxs-lookup"><span data-stu-id="5f2d3-136">In Solution Explorer, expand the Filters folder and open the file named InitializeSimpleMembershipAttribute.cs.</span></span> <span data-ttu-id="5f2d3-137">Localize o construtor de `SimpleMembershipInitializer`.</span><span class="sxs-lookup"><span data-stu-id="5f2d3-137">Locate the `SimpleMembershipInitializer` constructor.</span></span> <span data-ttu-id="5f2d3-138">Após a chamada para **websecurity. InitializeDatabaseConnection**, adicione o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="5f2d3-138">After the call to **WebSecurity.InitializeDatabaseConnection**, add the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

<span data-ttu-id="5f2d3-139">Essa é uma maneira rápida e suja de adicionar a função de "administrador" e criar um usuário para a função.</span><span class="sxs-lookup"><span data-stu-id="5f2d3-139">This is a quick-and-dirty way to add the "Administrator" role and create a user for the role.</span></span>

<span data-ttu-id="5f2d3-140">Em Gerenciador de Soluções, expanda a pasta controladores e abra o arquivo HomeController.cs.</span><span class="sxs-lookup"><span data-stu-id="5f2d3-140">In Solution Explorer, expand the Controllers folder and open the HomeController.cs file.</span></span> <span data-ttu-id="5f2d3-141">Adicione o atributo **Authorize** ao método `Admin`.</span><span class="sxs-lookup"><span data-stu-id="5f2d3-141">Add the **Authorize** attribute to the `Admin` method.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

<span data-ttu-id="5f2d3-142">Abra o arquivo AdminController.cs e adicione o atributo **autorizar** a toda a classe de `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="5f2d3-142">Open the AdminController.cs file and add the **Authorize** attribute to the entire `AdminController` class.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> <span data-ttu-id="5f2d3-143">O MVC e a API da Web definem **autorizar** atributos, em namespaces diferentes.</span><span class="sxs-lookup"><span data-stu-id="5f2d3-143">MVC and Web API both define **Authorize** attributes, in different namespaces.</span></span> <span data-ttu-id="5f2d3-144">O MVC usa **System. Web. Mvc. authorattribute**, enquanto a API Web usa **System. Web. http. authorattribute**.</span><span class="sxs-lookup"><span data-stu-id="5f2d3-144">MVC uses **System.Web.Mvc.AuthorizeAttribute**, while Web API uses **System.Web.Http.AuthorizeAttribute**.</span></span>

<span data-ttu-id="5f2d3-145">Agora, somente os administradores podem exibir a página de administração.</span><span class="sxs-lookup"><span data-stu-id="5f2d3-145">Now only administrators can view the Admin page.</span></span> <span data-ttu-id="5f2d3-146">Além disso, se você enviar uma solicitação HTTP para o controlador de administração, a solicitação deverá conter um cookie de autenticação.</span><span class="sxs-lookup"><span data-stu-id="5f2d3-146">Also, if you send an HTTP request to the Admin controller, the request must contain an authentication cookie.</span></span> <span data-ttu-id="5f2d3-147">Caso contrário, o servidor enviará uma resposta HTTP 401 (não autorizado).</span><span class="sxs-lookup"><span data-stu-id="5f2d3-147">If not, the server sends an HTTP 401 (Unauthorized) response.</span></span> <span data-ttu-id="5f2d3-148">Você pode ver isso no Fiddler enviando uma solicitação GET para `http://localhost:*port*/api/admin`.</span><span class="sxs-lookup"><span data-stu-id="5f2d3-148">You can see this in Fiddler by sending a GET request to `http://localhost:*port*/api/admin`.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5f2d3-149">[Anterior](using-web-api-with-entity-framework-part-3.md)
> [Próximo](using-web-api-with-entity-framework-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="5f2d3-149">[Previous](using-web-api-with-entity-framework-part-3.md)
[Next](using-web-api-with-entity-framework-part-5.md)</span></span>
