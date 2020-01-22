---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: Alterar a chave primária para usuários em ASP.NET Identity-ASP.NET 4. x
author: Rick-Anderson
description: No Visual Studio 2013, o aplicativo Web padrão usa um valor de cadeia de caracteres para a chave para contas de usuário. ASP.NET Identity permite que você altere o tipo de...
ms.author: riande
ms.date: 09/30/2014
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 0afea8eacfc646f1489b87629fdb2d437815d88c
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519135"
---
# <a name="change-primary-key-for-users-in-aspnet-identity"></a><span data-ttu-id="bd533-104">Alterar a chave primária de usuários na Identidade do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="bd533-104">Change Primary Key for Users in ASP.NET Identity</span></span>

<span data-ttu-id="bd533-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="bd533-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="bd533-106">No Visual Studio 2013, o aplicativo Web padrão usa um valor de cadeia de caracteres para a chave para contas de usuário.</span><span class="sxs-lookup"><span data-stu-id="bd533-106">In Visual Studio 2013, the default web application uses a string value for the key for user accounts.</span></span> <span data-ttu-id="bd533-107">ASP.NET Identity permite que você altere o tipo da chave para atender aos seus requisitos de dados.</span><span class="sxs-lookup"><span data-stu-id="bd533-107">ASP.NET Identity enables you to change the type of the key to meet your data requirements.</span></span> <span data-ttu-id="bd533-108">Por exemplo, você pode alterar o tipo da chave de uma cadeia de caracteres para um número inteiro.</span><span class="sxs-lookup"><span data-stu-id="bd533-108">For example, you can change the type of the key from a string to an integer.</span></span>
> 
> <span data-ttu-id="bd533-109">Este tópico mostra como começar com o aplicativo Web padrão e alterar a chave de conta de usuário para um número inteiro.</span><span class="sxs-lookup"><span data-stu-id="bd533-109">This topic shows how to start with the default web application and change the user account key to an integer.</span></span> <span data-ttu-id="bd533-110">Você pode usar as mesmas modificações para implementar qualquer tipo de chave em seu projeto.</span><span class="sxs-lookup"><span data-stu-id="bd533-110">You can use the same modifications to implement any type of key in your project.</span></span> <span data-ttu-id="bd533-111">Ele mostra como fazer essas alterações no aplicativo Web padrão, mas você pode aplicar modificações semelhantes a um aplicativo personalizado.</span><span class="sxs-lookup"><span data-stu-id="bd533-111">It shows how to make these changes in the default web application, but you could apply similar modifications to a customized application.</span></span> <span data-ttu-id="bd533-112">Ele mostra as alterações necessárias ao trabalhar com MVC ou Web Forms.</span><span class="sxs-lookup"><span data-stu-id="bd533-112">It shows the changes needed when working with MVC or Web Forms.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="bd533-113">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="bd533-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="bd533-114">Visual Studio 2013 com a atualização 2 (ou posterior)</span><span class="sxs-lookup"><span data-stu-id="bd533-114">Visual Studio 2013 with Update 2 (or later)</span></span>
> - <span data-ttu-id="bd533-115">ASP.NET Identity 2,1 ou posterior</span><span class="sxs-lookup"><span data-stu-id="bd533-115">ASP.NET Identity 2.1 or later</span></span>

<span data-ttu-id="bd533-116">Para executar as etapas neste tutorial, você deve ter Visual Studio 2013 atualização 2 (ou posterior) e um aplicativo Web criado com base no modelo de aplicativo Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bd533-116">To perform the steps in this tutorial, you must have Visual Studio 2013 Update 2 (or later), and a web application created from the ASP.NET Web Application template.</span></span> <span data-ttu-id="bd533-117">O modelo foi alterado na atualização 3.</span><span class="sxs-lookup"><span data-stu-id="bd533-117">The template changed in Update 3.</span></span> <span data-ttu-id="bd533-118">Este tópico mostra como alterar o modelo na atualização 2 e na atualização 3.</span><span class="sxs-lookup"><span data-stu-id="bd533-118">This topic shows how to change the template in Update 2 and Update 3.</span></span>

<span data-ttu-id="bd533-119">Esse tópico contém as seguintes seções:</span><span class="sxs-lookup"><span data-stu-id="bd533-119">This topic contains the following sections:</span></span>

- [<span data-ttu-id="bd533-120">Alterar o tipo da chave na classe de usuário de identidade</span><span class="sxs-lookup"><span data-stu-id="bd533-120">Change the type of the key in the Identity user class</span></span>](#userclass)
- [<span data-ttu-id="bd533-121">Adicionar classes de identidade personalizadas que usam o tipo de chave</span><span class="sxs-lookup"><span data-stu-id="bd533-121">Add customized Identity classes that use the key type</span></span>](#customclass)
- [<span data-ttu-id="bd533-122">Alterar a classe de contexto e o Gerenciador de usuários para usar o tipo de chave</span><span class="sxs-lookup"><span data-stu-id="bd533-122">Change the context class and user manager to use the key type</span></span>](#context)
- [<span data-ttu-id="bd533-123">Alterar a configuração de inicialização para usar o tipo de chave</span><span class="sxs-lookup"><span data-stu-id="bd533-123">Change start-up configuration to use the key type</span></span>](#startup)
- [<span data-ttu-id="bd533-124">Para MVC com atualização 2, altere o AccountController para passar o tipo de chave</span><span class="sxs-lookup"><span data-stu-id="bd533-124">For MVC with Update 2, change the AccountController to pass the key type</span></span>](#mvcupdate2)
- [<span data-ttu-id="bd533-125">Para MVC com atualização 3, altere AccountController e ManageController para passar o tipo de chave</span><span class="sxs-lookup"><span data-stu-id="bd533-125">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>](#mvcupdate3)
- [<span data-ttu-id="bd533-126">Para Web Forms com a atualização 2, altere as páginas da conta para passar o tipo de chave</span><span class="sxs-lookup"><span data-stu-id="bd533-126">For Web Forms with Update 2, change Account pages to pass the key type</span></span>](#webformsupdate2)
- [<span data-ttu-id="bd533-127">Para Web Forms com a atualização 3, altere as páginas da conta para passar o tipo de chave</span><span class="sxs-lookup"><span data-stu-id="bd533-127">For Web Forms with Update 3, change Account pages to pass the key type</span></span>](#webformsupdate3)
- [<span data-ttu-id="bd533-128">Executar aplicativo</span><span class="sxs-lookup"><span data-stu-id="bd533-128">Run application</span></span>](#run)
- [<span data-ttu-id="bd533-129">Outros recursos</span><span class="sxs-lookup"><span data-stu-id="bd533-129">Other resources</span></span>](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a><span data-ttu-id="bd533-130">Alterar o tipo da chave na classe de usuário de identidade</span><span class="sxs-lookup"><span data-stu-id="bd533-130">Change the type of the key in the Identity user class</span></span>

<span data-ttu-id="bd533-131">Em seu projeto criado no modelo de aplicativo Web ASP.NET, especifique que a classe ApplicationUser usa um inteiro para a chave para contas de usuário.</span><span class="sxs-lookup"><span data-stu-id="bd533-131">In your project created from the ASP.NET Web Application template, specify that the ApplicationUser class uses an integer for the key for user accounts.</span></span> <span data-ttu-id="bd533-132">Em IdentityModels.cs, altere a classe ApplicationUser para herdar de IdentityUser que tem um tipo de **int** para o parâmetro genérico TKey.</span><span class="sxs-lookup"><span data-stu-id="bd533-132">In IdentityModels.cs, change the ApplicationUser class to inherit from IdentityUser that has a type of **int** for the TKey generic parameter.</span></span> <span data-ttu-id="bd533-133">Você também passa os nomes de três classes personalizadas que ainda não implementou.</span><span class="sxs-lookup"><span data-stu-id="bd533-133">You also pass the names of three customized class which you have not implemented yet.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

<span data-ttu-id="bd533-134">Você alterou o tipo da chave, mas, por padrão, o restante do aplicativo ainda assume que a chave é uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="bd533-134">You have changed the type of the key, but, by default, the rest of the application still assumes the key is a string.</span></span> <span data-ttu-id="bd533-135">Você deve indicar explicitamente o tipo da chave no código que assume uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="bd533-135">You must explicitly indicate the type of the key in code that assumes a string.</span></span>

<span data-ttu-id="bd533-136">Na classe **ApplicationUser** , altere o método **GenerateUserIdentityAsync** para incluir int, conforme mostrado no código realçado abaixo.</span><span class="sxs-lookup"><span data-stu-id="bd533-136">In the **ApplicationUser** class, change the **GenerateUserIdentityAsync** method to include int, as shown in the highlighted code below.</span></span> <span data-ttu-id="bd533-137">Essa alteração não é necessária para projetos Web Forms com o modelo atualização 3.</span><span class="sxs-lookup"><span data-stu-id="bd533-137">This change is not necessary for Web Forms projects with the Update 3 template.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a><span data-ttu-id="bd533-138">Adicionar classes de identidade personalizadas que usam o tipo de chave</span><span class="sxs-lookup"><span data-stu-id="bd533-138">Add customized Identity classes that use the key type</span></span>

<span data-ttu-id="bd533-139">As outras classes de identidade, como IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, USERSTORE, RoleStore, ainda estão configuradas para usar uma chave de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="bd533-139">The other Identity classes, such as IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, UserStore, RoleStore, are still set up to use a string key.</span></span> <span data-ttu-id="bd533-140">Crie novas versões dessas classes que especificam um número inteiro para a chave.</span><span class="sxs-lookup"><span data-stu-id="bd533-140">Create new versions of these classes that specify an integer for the key.</span></span> <span data-ttu-id="bd533-141">Você não precisa fornecer muito código de implementação nessas classes, você está apenas definindo int como a chave.</span><span class="sxs-lookup"><span data-stu-id="bd533-141">You do not need to provide much implementation code in these classes, you are primarily just setting int as the key.</span></span>

<span data-ttu-id="bd533-142">Adicione as seguintes classes ao arquivo IdentityModels.cs.</span><span class="sxs-lookup"><span data-stu-id="bd533-142">Add the following classes to your IdentityModels.cs file.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a><span data-ttu-id="bd533-143">Alterar a classe de contexto e o Gerenciador de usuários para usar o tipo de chave</span><span class="sxs-lookup"><span data-stu-id="bd533-143">Change the context class and user manager to use the key type</span></span>

<span data-ttu-id="bd533-144">No IdentityModels.cs, altere a definição da classe **ApplicationDbContext** para usar suas novas classes personalizadas e um **int** para a chave, conforme mostrado no código realçado.</span><span class="sxs-lookup"><span data-stu-id="bd533-144">In IdentityModels.cs, change the definition of the **ApplicationDbContext** class to use your new customized classes and an **int** for the key, as shown in the highlighted code.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

<span data-ttu-id="bd533-145">O parâmetro ThrowIfV1Schema não é mais válido no construtor.</span><span class="sxs-lookup"><span data-stu-id="bd533-145">The ThrowIfV1Schema parameter is no longer valid in the constructor.</span></span> <span data-ttu-id="bd533-146">Altere o construtor para que ele não passe um valor ThrowIfV1Schema.</span><span class="sxs-lookup"><span data-stu-id="bd533-146">Change the constructor so it does not pass a ThrowIfV1Schema value.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

<span data-ttu-id="bd533-147">Abra IdentityConfig.cs e altere a classe **ApplicationUserManger** para usar sua nova classe de armazenamento de usuário para manter dados e um **int** para a chave.</span><span class="sxs-lookup"><span data-stu-id="bd533-147">Open IdentityConfig.cs, and change the **ApplicationUserManger** class to use your new user store class for persisting data and an **int** for the key.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

<span data-ttu-id="bd533-148">No modelo atualização 3, você deve alterar a classe ApplicationSignInManager.</span><span class="sxs-lookup"><span data-stu-id="bd533-148">In the Update 3 template, you must change the ApplicationSignInManager class.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a><span data-ttu-id="bd533-149">Alterar a configuração de inicialização para usar o tipo de chave</span><span class="sxs-lookup"><span data-stu-id="bd533-149">Change start-up configuration to use the key type</span></span>

<span data-ttu-id="bd533-150">Em Startup.Auth.cs, substitua o código OnValidateIdentity, conforme realçado abaixo.</span><span class="sxs-lookup"><span data-stu-id="bd533-150">In Startup.Auth.cs, replace the OnValidateIdentity code, as highlighted below.</span></span> <span data-ttu-id="bd533-151">Observe que a definição de getUserIdCallback, analisa o valor da cadeia de caracteres em um número inteiro.</span><span class="sxs-lookup"><span data-stu-id="bd533-151">Notice that the getUserIdCallback definition, parses the string value into an integer.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

<span data-ttu-id="bd533-152">Se o seu projeto não reconhecer a implementação genérica do método **GetUserID** , talvez seja necessário atualizar o pacote ASP.net Identity NuGet para a versão 2,1</span><span class="sxs-lookup"><span data-stu-id="bd533-152">If your project does not recognize the generic implementation of the **GetUserId** method, you may need to update the ASP.NET Identity NuGet package to version 2.1</span></span>

<span data-ttu-id="bd533-153">Você fez muitas alterações nas classes de infraestrutura usadas pelo ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="bd533-153">You have made a lot of changes to the infrastructure classes used by ASP.NET Identity.</span></span> <span data-ttu-id="bd533-154">Se você tentar compilar o projeto, observará muitos erros.</span><span class="sxs-lookup"><span data-stu-id="bd533-154">If you try compiling the project, you will notice a lot of errors.</span></span> <span data-ttu-id="bd533-155">Felizmente, os erros restantes são todos semelhantes.</span><span class="sxs-lookup"><span data-stu-id="bd533-155">Fortunately, the remaining errors are all similar.</span></span> <span data-ttu-id="bd533-156">A classe Identity espera um inteiro para a chave, mas o controlador (ou formulário da Web) está passando um valor de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="bd533-156">The Identity class expects an integer for the key, but the controller (or Web Form) is passing a string value.</span></span> <span data-ttu-id="bd533-157">Em cada caso, você precisa converter de uma cadeia de caracteres para e inteiro chamando **Getuserid&lt;int&gt;** .</span><span class="sxs-lookup"><span data-stu-id="bd533-157">In each case, you need to convert from a string to and integer by calling **GetUserId&lt;int&gt;**.</span></span> <span data-ttu-id="bd533-158">Você pode trabalhar na lista de erros da compilação ou seguir as alterações abaixo.</span><span class="sxs-lookup"><span data-stu-id="bd533-158">You can either work through the error list from compilation or follow the changes below.</span></span>

<span data-ttu-id="bd533-159">As alterações restantes dependem do tipo de projeto que você está criando e da atualização instalada no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bd533-159">The remaining changes depend on the type of project you are creating and which update you have installed in Visual Studio.</span></span> <span data-ttu-id="bd533-160">Você pode ir diretamente para a seção relevante por meio dos links a seguir</span><span class="sxs-lookup"><span data-stu-id="bd533-160">You can go directly to the relevant section through the following links</span></span>

- [<span data-ttu-id="bd533-161">Para MVC com atualização 2, altere o AccountController para passar o tipo de chave</span><span class="sxs-lookup"><span data-stu-id="bd533-161">For MVC with Update 2, change the AccountController to pass the key type</span></span>](#mvcupdate2)
- [<span data-ttu-id="bd533-162">Para MVC com atualização 3, altere AccountController e ManageController para passar o tipo de chave</span><span class="sxs-lookup"><span data-stu-id="bd533-162">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>](#mvcupdate3)
- [<span data-ttu-id="bd533-163">Para Web Forms com a atualização 2, altere as páginas da conta para passar o tipo de chave</span><span class="sxs-lookup"><span data-stu-id="bd533-163">For Web Forms with Update 2, change Account pages to pass the key type</span></span>](#webformsupdate2)
- [<span data-ttu-id="bd533-164">Para Web Forms com a atualização 3, altere as páginas da conta para passar o tipo de chave</span><span class="sxs-lookup"><span data-stu-id="bd533-164">For Web Forms with Update 3, change Account pages to pass the key type</span></span>](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a><span data-ttu-id="bd533-165">Para MVC com atualização 2, altere o AccountController para passar o tipo de chave</span><span class="sxs-lookup"><span data-stu-id="bd533-165">For MVC with Update 2, change the AccountController to pass the key type</span></span>

<span data-ttu-id="bd533-166">Abra o arquivo AccountController.cs.</span><span class="sxs-lookup"><span data-stu-id="bd533-166">Open the AccountController.cs file.</span></span> <span data-ttu-id="bd533-167">Você precisa alterar os métodos a seguir.</span><span class="sxs-lookup"><span data-stu-id="bd533-167">You need to change the following methods.</span></span>

<span data-ttu-id="bd533-168">Método **ConfirmEmail**</span><span class="sxs-lookup"><span data-stu-id="bd533-168">**ConfirmEmail** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample9.cs?highlight=1,3)]

<span data-ttu-id="bd533-169">**Desassociar** método</span><span class="sxs-lookup"><span data-stu-id="bd533-169">**Disassociate** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample10.cs?highlight=5,9)]

<span data-ttu-id="bd533-170">Método **Manage (ManageUserViewModel)**</span><span class="sxs-lookup"><span data-stu-id="bd533-170">**Manage(ManageUserViewModel)** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample11.cs?highlight=11,17,41)]

<span data-ttu-id="bd533-171">Método **LinkLoginCallback**</span><span class="sxs-lookup"><span data-stu-id="bd533-171">**LinkLoginCallback** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample12.cs?highlight=10)]

<span data-ttu-id="bd533-172">Método **RemoveAccountList**</span><span class="sxs-lookup"><span data-stu-id="bd533-172">**RemoveAccountList** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample13.cs?highlight=3)]

<span data-ttu-id="bd533-173">Método **HasPassword**</span><span class="sxs-lookup"><span data-stu-id="bd533-173">**HasPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample14.cs?highlight=3)]

<span data-ttu-id="bd533-174">Agora você pode [executar o aplicativo](#run) e registrar um novo usuário.</span><span class="sxs-lookup"><span data-stu-id="bd533-174">You can now [run the application](#run) and register a new user.</span></span>

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a><span data-ttu-id="bd533-175">Para MVC com atualização 3, altere AccountController e ManageController para passar o tipo de chave</span><span class="sxs-lookup"><span data-stu-id="bd533-175">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>

<span data-ttu-id="bd533-176">Abra o arquivo AccountController.cs.</span><span class="sxs-lookup"><span data-stu-id="bd533-176">Open the AccountController.cs file.</span></span> <span data-ttu-id="bd533-177">Você precisa alterar o método a seguir.</span><span class="sxs-lookup"><span data-stu-id="bd533-177">You need to change the following method.</span></span>

<span data-ttu-id="bd533-178">Método **ConfirmEmail**</span><span class="sxs-lookup"><span data-stu-id="bd533-178">**ConfirmEmail** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample15.cs?highlight=1,3)]

<span data-ttu-id="bd533-179">Método **SendCode**</span><span class="sxs-lookup"><span data-stu-id="bd533-179">**SendCode** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

<span data-ttu-id="bd533-180">Abra o arquivo ManageController.cs.</span><span class="sxs-lookup"><span data-stu-id="bd533-180">Open the ManageController.cs file.</span></span> <span data-ttu-id="bd533-181">Você precisa alterar os métodos a seguir.</span><span class="sxs-lookup"><span data-stu-id="bd533-181">You need to change the following methods.</span></span>

<span data-ttu-id="bd533-182">Método de **índice**</span><span class="sxs-lookup"><span data-stu-id="bd533-182">**Index** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample17.cs?highlight=15-17)]

<span data-ttu-id="bd533-183">Métodos **RemoveLogin**</span><span class="sxs-lookup"><span data-stu-id="bd533-183">**RemoveLogin** methods</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample18.cs?highlight=3,13,17)]

<span data-ttu-id="bd533-184">Método **AddPhoneNumber**</span><span class="sxs-lookup"><span data-stu-id="bd533-184">**AddPhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample19.cs?highlight=9)]

<span data-ttu-id="bd533-185">Método **EnableTwoFactorAuthentication**</span><span class="sxs-lookup"><span data-stu-id="bd533-185">**EnableTwoFactorAuthentication** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample20.cs?highlight=3-4)]

<span data-ttu-id="bd533-186">Método **DisableTwoFactorAuthentication**</span><span class="sxs-lookup"><span data-stu-id="bd533-186">**DisableTwoFactorAuthentication** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample21.cs?highlight=3-4)]

<span data-ttu-id="bd533-187">Métodos **VerifyPhoneNumber**</span><span class="sxs-lookup"><span data-stu-id="bd533-187">**VerifyPhoneNumber** methods</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample22.cs?highlight=4,18,21)]

<span data-ttu-id="bd533-188">Método **RemovePhoneNumber**</span><span class="sxs-lookup"><span data-stu-id="bd533-188">**RemovePhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample23.cs?highlight=3,8)]

<span data-ttu-id="bd533-189">Método **ChangePassword**</span><span class="sxs-lookup"><span data-stu-id="bd533-189">**ChangePassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample24.cs?highlight=10,13)]

<span data-ttu-id="bd533-190">Método **SetPassword**</span><span class="sxs-lookup"><span data-stu-id="bd533-190">**SetPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample25.cs?highlight=5,8)]

<span data-ttu-id="bd533-191">Método **ManageLogins**</span><span class="sxs-lookup"><span data-stu-id="bd533-191">**ManageLogins** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample26.cs?highlight=7,12)]

<span data-ttu-id="bd533-192">Método **LinkLoginCallback**</span><span class="sxs-lookup"><span data-stu-id="bd533-192">**LinkLoginCallback** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample27.cs?highlight=8)]

<span data-ttu-id="bd533-193">Método **HasPassword**</span><span class="sxs-lookup"><span data-stu-id="bd533-193">**HasPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample28.cs?highlight=3)]

<span data-ttu-id="bd533-194">Método **HasPhoneNumber**</span><span class="sxs-lookup"><span data-stu-id="bd533-194">**HasPhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample29.cs?highlight=3)]

<span data-ttu-id="bd533-195">Agora você pode [executar o aplicativo](#run) e registrar um novo usuário.</span><span class="sxs-lookup"><span data-stu-id="bd533-195">You can now [run the application](#run) and register a new user.</span></span>

<a id="webformsupdate2"></a>
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a><span data-ttu-id="bd533-196">Para Web Forms com a atualização 2, altere as páginas da conta para passar o tipo de chave</span><span class="sxs-lookup"><span data-stu-id="bd533-196">For Web Forms with Update 2, change Account pages to pass the key type</span></span>

<span data-ttu-id="bd533-197">Para Web Forms com a atualização 2, você precisa alterar as páginas a seguir.</span><span class="sxs-lookup"><span data-stu-id="bd533-197">For Web Forms with Update 2, you need to change the following pages.</span></span>

<span data-ttu-id="bd533-198">**Confirm.aspx.cx**</span><span class="sxs-lookup"><span data-stu-id="bd533-198">**Confirm.aspx.cx**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

<span data-ttu-id="bd533-199">**RegisterExternalLogin.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="bd533-199">**RegisterExternalLogin.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

<span data-ttu-id="bd533-200">**Manage.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="bd533-200">**Manage.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

<span data-ttu-id="bd533-201">Agora você pode [executar o aplicativo](#run) e registrar um novo usuário.</span><span class="sxs-lookup"><span data-stu-id="bd533-201">You can now [run the application](#run) and register a new user.</span></span>

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a><span data-ttu-id="bd533-202">Para Web Forms com a atualização 3, altere as páginas da conta para passar o tipo de chave</span><span class="sxs-lookup"><span data-stu-id="bd533-202">For Web Forms with Update 3, change Account pages to pass the key type</span></span>

<span data-ttu-id="bd533-203">Para Web Forms com a atualização 3, você precisa alterar as páginas a seguir.</span><span class="sxs-lookup"><span data-stu-id="bd533-203">For Web Forms with Update 3, you need to change the following pages.</span></span>

<span data-ttu-id="bd533-204">**Confirm.aspx.cx**</span><span class="sxs-lookup"><span data-stu-id="bd533-204">**Confirm.aspx.cx**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample33.cs?highlight=8)]

<span data-ttu-id="bd533-205">**RegisterExternalLogin.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="bd533-205">**RegisterExternalLogin.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample34.cs?highlight=36)]

<span data-ttu-id="bd533-206">**Manage.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="bd533-206">**Manage.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample35.cs?highlight=11,27,32,34,82,87,99,108)]

<span data-ttu-id="bd533-207">**VerifyPhoneNumber.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="bd533-207">**VerifyPhoneNumber.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample36.cs?highlight=8,23,27)]

<span data-ttu-id="bd533-208">**AddPhoneNumber.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="bd533-208">**AddPhoneNumber.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample37.cs?highlight=7)]

<span data-ttu-id="bd533-209">**ManagePassword.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="bd533-209">**ManagePassword.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample38.cs?highlight=11,47,50,68)]

<span data-ttu-id="bd533-210">**ManageLogins.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="bd533-210">**ManageLogins.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample39.cs?highlight=16,23,32,41,45)]

<span data-ttu-id="bd533-211">**TwoFactorAuthenticationSignIn.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="bd533-211">**TwoFactorAuthenticationSignIn.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample40.cs?highlight=14-15,29,53)]

<a id="run"></a>
## <a name="run-application"></a><span data-ttu-id="bd533-212">Executar aplicativo</span><span class="sxs-lookup"><span data-stu-id="bd533-212">Run application</span></span>

<span data-ttu-id="bd533-213">Você concluiu todas as alterações necessárias no modelo de aplicativo Web padrão.</span><span class="sxs-lookup"><span data-stu-id="bd533-213">You have finished all of the required changes to the default Web Application template.</span></span> <span data-ttu-id="bd533-214">Execute o aplicativo e registre um novo usuário.</span><span class="sxs-lookup"><span data-stu-id="bd533-214">Run the application and register a new user.</span></span> <span data-ttu-id="bd533-215">Depois de registrar o usuário, você observará que a tabela AspNetUsers tem uma coluna de ID que é um número inteiro.</span><span class="sxs-lookup"><span data-stu-id="bd533-215">After registering the user, you will notice that the AspNetUsers table has an Id column that is an integer.</span></span>

![nova chave primária](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

<span data-ttu-id="bd533-217">Se você tiver criado anteriormente as tabelas de ASP.NET Identity com uma chave primária diferente, precisará fazer algumas alterações adicionais.</span><span class="sxs-lookup"><span data-stu-id="bd533-217">If you have previously created the ASP.NET Identity tables with a different primary key, you need to make some additional changes.</span></span> <span data-ttu-id="bd533-218">Se possível, basta excluir o banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="bd533-218">If possible, just delete the existing database.</span></span> <span data-ttu-id="bd533-219">O banco de dados será recriado com o design correto quando você executar o aplicativo Web e adicionar um novo usuário.</span><span class="sxs-lookup"><span data-stu-id="bd533-219">The database will be re-created with the correct design when you run the web application and add a new user.</span></span> <span data-ttu-id="bd533-220">Se a exclusão não for possível, execute as migrações do Code First para alterar as tabelas.</span><span class="sxs-lookup"><span data-stu-id="bd533-220">If deletion is not possible, run code first migrations to change the tables.</span></span> <span data-ttu-id="bd533-221">No entanto, a nova chave primária de inteiro não será configurada como uma propriedade de identidade do SQL no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="bd533-221">However, the new integer primary key will not be set up as a SQL IDENTITY property in the database.</span></span> <span data-ttu-id="bd533-222">Você deve definir manualmente a coluna de ID como uma identidade.</span><span class="sxs-lookup"><span data-stu-id="bd533-222">You must manually set the Id column as an IDENTITY.</span></span>

<a id="other"></a>
## <a name="other-resources"></a><span data-ttu-id="bd533-223">Outros recursos</span><span class="sxs-lookup"><span data-stu-id="bd533-223">Other resources</span></span>

- [<span data-ttu-id="bd533-224">Visão geral de provedores de armazenamento personalizados para a Identidade do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="bd533-224">Overview of Custom Storage Providers for ASP.NET Identity</span></span>](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [<span data-ttu-id="bd533-225">Migração de um site existente da Associação do SQL para a Identidade do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="bd533-225">Migrating an Existing Website from SQL Membership to ASP.NET Identity</span></span>](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [<span data-ttu-id="bd533-226">Migrando dados do provedor universal para associação e perfis de usuário para ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="bd533-226">Migrating Universal Provider Data for Membership and User Profiles to ASP.NET Identity</span></span>](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- <span data-ttu-id="bd533-227">[Aplicativo de exemplo](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/ChangePK) com chave primária alterada</span><span class="sxs-lookup"><span data-stu-id="bd533-227">[Sample application](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/ChangePK) with changed primary key</span></span>
