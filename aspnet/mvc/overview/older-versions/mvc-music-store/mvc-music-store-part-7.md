---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: 'Parte 7: Associação e autorização | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo de loja de música MVC do ASP.NET. A parte 7 aborda associação e autorização.
ms.author: riande
ms.date: 10/13/2010
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: a6a1a936e0ea29ea36721ba78f35845401f74c01
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539196"
---
# <a name="part-7-membership-and-authorization"></a><span data-ttu-id="08021-104">Parte 7: Associação e autorização</span><span class="sxs-lookup"><span data-stu-id="08021-104">Part 7: Membership and Authorization</span></span>

<span data-ttu-id="08021-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="08021-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="08021-106">A loja de música MVC é um aplicativo de tutorial que apresenta e explica passo a passo como usar o ASP.NET MVC e o Visual Studio para desenvolvimento para a Web.</span><span class="sxs-lookup"><span data-stu-id="08021-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="08021-107">A loja de música MVC é uma implementação de armazenamento de exemplo leve que vende os álbuns de música online e implementa a administração básica de site, a entrada do usuário e a funcionalidade do carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="08021-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="08021-108">Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo de loja de música MVC do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="08021-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="08021-109">A parte 7 aborda associação e autorização.</span><span class="sxs-lookup"><span data-stu-id="08021-109">Part 7 covers Membership and Authorization.</span></span>

<span data-ttu-id="08021-110">Nosso controlador Store Manager está acessível no momento para qualquer pessoa que visitar nosso site.</span><span class="sxs-lookup"><span data-stu-id="08021-110">Our Store Manager controller is currently accessible to anyone visiting our site.</span></span> <span data-ttu-id="08021-111">Vamos alterar isso para restringir a permissão aos administradores do site.</span><span class="sxs-lookup"><span data-stu-id="08021-111">Let's change this to restrict permission to site administrators.</span></span>

## <a name="adding-the-accountcontroller-and-views"></a><span data-ttu-id="08021-112">Adicionando AccountController e exibições</span><span class="sxs-lookup"><span data-stu-id="08021-112">Adding the AccountController and Views</span></span>

<span data-ttu-id="08021-113">Uma diferença entre o modelo completo de aplicativo Web do ASP.NET MVC 3 e o modelo de aplicativo Web do ASP.NET MVC 3 vazio é que o modelo vazio não inclui um controlador de conta.</span><span class="sxs-lookup"><span data-stu-id="08021-113">One difference between the full ASP.NET MVC 3 Web Application template and the ASP.NET MVC 3 Empty Web Application template is that the empty template doesn't include an Account Controller.</span></span> <span data-ttu-id="08021-114">Adicionaremos um controlador de conta copiando alguns arquivos de um novo aplicativo MVC ASP.NET criado a partir do modelo de aplicativo Web completo do ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="08021-114">We'll add an Account Controller by copying a few files from a new ASP.NET MVC application created from the full ASP.NET MVC 3 Web Application template.</span></span>

<span data-ttu-id="08021-115">Crie um novo aplicativo MVC do ASP.NET usando o modelo completo de aplicativo Web do ASP.NET MVC 3 e copie os seguintes arquivos para os mesmos diretórios em nosso projeto:</span><span class="sxs-lookup"><span data-stu-id="08021-115">Create a new ASP.NET MVC application using the full ASP.NET MVC 3 Web Application template and copy the following files into the same directories in our project:</span></span>

1. <span data-ttu-id="08021-116">Copiar AccountController.cs no diretório de controladores</span><span class="sxs-lookup"><span data-stu-id="08021-116">Copy AccountController.cs in the Controllers directory</span></span>
2. <span data-ttu-id="08021-117">Copiar AccountModels no diretório de modelos</span><span class="sxs-lookup"><span data-stu-id="08021-117">Copy AccountModels in the Models directory</span></span>
3. <span data-ttu-id="08021-118">Crie um diretório de conta dentro do diretório views e copie todas as quatro exibições em</span><span class="sxs-lookup"><span data-stu-id="08021-118">Create an Account directory inside the Views directory and copy all four views in</span></span>

<span data-ttu-id="08021-119">Altere o namespace para as classes de controlador e modelo para que comecem com MvcMusicStore.</span><span class="sxs-lookup"><span data-stu-id="08021-119">Change the namespace for the Controller and Model classes so they begin with MvcMusicStore.</span></span> <span data-ttu-id="08021-120">A classe AccountController deve usar o namespace MvcMusicStore. Controllers e a classe AccountModels deve usar o namespace MvcMusicStore. Models.</span><span class="sxs-lookup"><span data-stu-id="08021-120">The AccountController class should use the MvcMusicStore.Controllers namespace, and the AccountModels class should use the MvcMusicStore.Models namespace.</span></span>

<span data-ttu-id="08021-121">*Observação: esses arquivos também estão disponíveis no download MvcMusicStore-Assets. zip do qual copiamos nossos arquivos de design de site no início do tutorial. Os arquivos de associação estão localizados no diretório de código.*</span><span class="sxs-lookup"><span data-stu-id="08021-121">*Note: These files are also available in the MvcMusicStore-Assets.zip download from which we copied our site design files at the beginning of the tutorial. The Membership files are located in the Code directory.*</span></span>

<span data-ttu-id="08021-122">A solução atualizada deve ser parecida com a seguinte:</span><span class="sxs-lookup"><span data-stu-id="08021-122">The updated solution should look like the following:</span></span>

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a><span data-ttu-id="08021-123">Adicionar um usuário administrativo com o site de configuração do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="08021-123">Adding an Administrative User with the ASP.NET Configuration site</span></span>

<span data-ttu-id="08021-124">Antes de exigirmos autorização em nosso site, precisaremos criar um usuário com acesso.</span><span class="sxs-lookup"><span data-stu-id="08021-124">Before we require Authorization in our website, we'll need to create a user with access.</span></span> <span data-ttu-id="08021-125">A maneira mais fácil de criar um usuário é usar o site interno de configuração do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="08021-125">The easiest way to create a user is to use the built-in ASP.NET Configuration website.</span></span>

<span data-ttu-id="08021-126">Inicie o site de configuração do ASP.NET clicando em seguir o ícone na Gerenciador de Soluções.</span><span class="sxs-lookup"><span data-stu-id="08021-126">Launch the ASP.NET Configuration website by clicking following the icon in the Solution Explorer.</span></span>

![](mvc-music-store-part-7/_static/image2.png)

<span data-ttu-id="08021-127">Isso inicia um site de configuração.</span><span class="sxs-lookup"><span data-stu-id="08021-127">This launches a configuration website.</span></span> <span data-ttu-id="08021-128">Clique na guia Segurança na tela inicial e, em seguida, clique no link "Habilitar funções" no centro da tela.</span><span class="sxs-lookup"><span data-stu-id="08021-128">Click on the Security tab on the home screen, then click the "Enable roles" link in the center of the screen.</span></span>

![](mvc-music-store-part-7/_static/image3.png)

<span data-ttu-id="08021-129">Clique no link "criar ou gerenciar funções".</span><span class="sxs-lookup"><span data-stu-id="08021-129">Click the "Create or Manage roles" link.</span></span>

![](mvc-music-store-part-7/_static/image4.png)

<span data-ttu-id="08021-130">Insira "administrador" como o nome da função e pressione o botão Adicionar função.</span><span class="sxs-lookup"><span data-stu-id="08021-130">Enter "Administrator" as the role name and press the Add Role button.</span></span>

![](mvc-music-store-part-7/_static/image5.png)

<span data-ttu-id="08021-131">Clique no botão voltar e, em seguida, clique no link criar usuário no lado esquerdo.</span><span class="sxs-lookup"><span data-stu-id="08021-131">Click the Back button, then click on the Create user link on the left side.</span></span>

![](mvc-music-store-part-7/_static/image6.png)

<span data-ttu-id="08021-132">Preencha os campos de informações do usuário à esquerda usando as seguintes informações:</span><span class="sxs-lookup"><span data-stu-id="08021-132">Fill in the user information fields on the left using the following information:</span></span>

| <span data-ttu-id="08021-133">**Campo**</span><span class="sxs-lookup"><span data-stu-id="08021-133">**Field**</span></span> | <span data-ttu-id="08021-134">**Valor**</span><span class="sxs-lookup"><span data-stu-id="08021-134">**Value**</span></span> |
| --- | --- |
| <span data-ttu-id="08021-135">**Nome de usuário**</span><span class="sxs-lookup"><span data-stu-id="08021-135">**User Name**</span></span> | <span data-ttu-id="08021-136">Administrador</span><span class="sxs-lookup"><span data-stu-id="08021-136">Administrator</span></span> |
| <span data-ttu-id="08021-137">**Senha**</span><span class="sxs-lookup"><span data-stu-id="08021-137">**Password**</span></span> | <span data-ttu-id="08021-138">password123!</span><span class="sxs-lookup"><span data-stu-id="08021-138">password123!</span></span> |
| <span data-ttu-id="08021-139">**Confirmar Senha**</span><span class="sxs-lookup"><span data-stu-id="08021-139">**Confirm Password**</span></span> | <span data-ttu-id="08021-140">password123!</span><span class="sxs-lookup"><span data-stu-id="08021-140">password123!</span></span> |
| <span data-ttu-id="08021-141">**Email**</span><span class="sxs-lookup"><span data-stu-id="08021-141">**E-mail**</span></span> | <span data-ttu-id="08021-142">(qualquer endereço de email funcionará)</span><span class="sxs-lookup"><span data-stu-id="08021-142">(any email address will work)</span></span> |
| <span data-ttu-id="08021-143">**Pergunta de Segurança**</span><span class="sxs-lookup"><span data-stu-id="08021-143">**Security Question**</span></span> | <span data-ttu-id="08021-144">(o que você desejar)</span><span class="sxs-lookup"><span data-stu-id="08021-144">(whatever you like)</span></span> |
| <span data-ttu-id="08021-145">**Resposta de Segurança**</span><span class="sxs-lookup"><span data-stu-id="08021-145">**Security Answer**</span></span> | <span data-ttu-id="08021-146">(o que você desejar)</span><span class="sxs-lookup"><span data-stu-id="08021-146">(whatever you like)</span></span> |

<span data-ttu-id="08021-147">*Observação: é claro que você pode usar qualquer senha que desejar. As configurações de segurança de senha padrão exigem uma senha com até 7 caracteres e contém um caractere não alfanumérico.*</span><span class="sxs-lookup"><span data-stu-id="08021-147">*Note: You can of course use any password you'd like. The default password security settings require a password that is 7 characters long and contains one non-alphanumeric character.*</span></span>

<span data-ttu-id="08021-148">Selecione a função Administrador para esse usuário e clique no botão criar usuário.</span><span class="sxs-lookup"><span data-stu-id="08021-148">Select the Administrator role for this user, and click the Create User button.</span></span>

![](mvc-music-store-part-7/_static/image7.png)

<span data-ttu-id="08021-149">Neste ponto, você deverá ver uma mensagem indicando que o usuário foi criado com êxito.</span><span class="sxs-lookup"><span data-stu-id="08021-149">At this point, you should see a message indicating that the user was created successfully.</span></span>

![](mvc-music-store-part-7/_static/image8.png)

<span data-ttu-id="08021-150">Agora você pode fechar a janela do navegador.</span><span class="sxs-lookup"><span data-stu-id="08021-150">You can now close the browser window.</span></span>

## <a name="role-based-authorization"></a><span data-ttu-id="08021-151">Autorização baseada em função</span><span class="sxs-lookup"><span data-stu-id="08021-151">Role-based Authorization</span></span>

<span data-ttu-id="08021-152">Agora, podemos restringir o acesso ao StoreManagerController usando o atributo [Authorize], especificando que o usuário deve estar na função Administrador para acessar qualquer ação do controlador na classe.</span><span class="sxs-lookup"><span data-stu-id="08021-152">Now we can restrict access to the StoreManagerController using the [Authorize] attribute, specifying that the user must be in the Administrator role to access any controller action in the class.</span></span>

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

<span data-ttu-id="08021-153">*Observação: o atributo [autorizar] pode ser colocado em métodos de ação específicos, bem como no nível de classe do controlador.*</span><span class="sxs-lookup"><span data-stu-id="08021-153">*Note: The [Authorize] attribute can be placed on specific action methods as well as at the Controller class level.*</span></span>

<span data-ttu-id="08021-154">Agora, navegar até/StoreManager abre uma caixa de diálogo de logon:</span><span class="sxs-lookup"><span data-stu-id="08021-154">Now browsing to /StoreManager brings up a Log On dialog:</span></span>

![](mvc-music-store-part-7/_static/image9.png)

<span data-ttu-id="08021-155">Depois de fazer logon com nossa nova conta de administrador, podemos acessar a tela de edição do álbum como antes.</span><span class="sxs-lookup"><span data-stu-id="08021-155">After logging on with our new Administrator account, we're able to go to the Album Edit screen as before.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="08021-156">[Anterior](mvc-music-store-part-6.md)
> [Próximo](mvc-music-store-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="08021-156">[Previous](mvc-music-store-part-6.md)
[Next](mvc-music-store-part-8.md)</span></span>
