---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: Criando um aplicativo MVC 3 com Razor e JavaScript discreto | Microsoft Docs
author: microsoft
description: O aplicativo Web de exemplo user list demonstra como é simples criar aplicativos ASP.NET MVC 3 usando o mecanismo de exibição do Razor. O aplicativo de exemplo s...
ms.author: riande
ms.date: 11/01/2010
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: fb63493ff22c9261fc5746a998a32f2511141f87
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78540981"
---
# <a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a><span data-ttu-id="65740-104">Criação de um aplicativo do MVC 3 com Razor e JavaScript não obstrusivo</span><span class="sxs-lookup"><span data-stu-id="65740-104">Creating a MVC 3 Application with Razor and Unobtrusive JavaScript</span></span>

<span data-ttu-id="65740-105">pela [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="65740-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="65740-106">O aplicativo Web de exemplo user list demonstra como é simples criar aplicativos ASP.NET MVC 3 usando o mecanismo de exibição do Razor.</span><span class="sxs-lookup"><span data-stu-id="65740-106">The User List sample web application demonstrates how simple it is to create ASP.NET MVC 3 applications using the Razor view engine.</span></span> <span data-ttu-id="65740-107">O aplicativo de exemplo mostra como usar o novo mecanismo de exibição do Razor com o ASP.NET MVC versão 3 e o Visual Studio 2010 para criar um site da lista de usuários fictícios que inclui funcionalidades como criar, exibir, editar e excluir usuários.</span><span class="sxs-lookup"><span data-stu-id="65740-107">The sample application shows how to use the new Razor view engine with ASP.NET MVC version 3 and Visual Studio 2010 to create a fictional User List website that includes functionality such as creating, displaying, editing, and deleting users.</span></span>
> 
> <span data-ttu-id="65740-108">Este tutorial descreve as etapas que foram tomadas para criar o aplicativo ASP.NET MVC 3 de exemplo de lista de usuários.</span><span class="sxs-lookup"><span data-stu-id="65740-108">This tutorial describes the steps that were taken in order to build the User List sample ASP.NET MVC 3 application.</span></span> <span data-ttu-id="65740-109">Um projeto do Visual Studio C# com o e o código-fonte do vb está disponível para acompanhar este tópico: [Download](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span><span class="sxs-lookup"><span data-stu-id="65740-109">A Visual Studio project with C# and VB source code is available to accompany this topic: [Download](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span></span> <span data-ttu-id="65740-110">Se você tiver dúvidas sobre este tutorial, poste-os no [Fórum do MVC](https://forums.asp.net/1146.aspx).</span><span class="sxs-lookup"><span data-stu-id="65740-110">If you have questions about this tutorial, please post them to the [MVC forum](https://forums.asp.net/1146.aspx).</span></span>

## <a name="overview"></a><span data-ttu-id="65740-111">Visão geral</span><span class="sxs-lookup"><span data-stu-id="65740-111">Overview</span></span>

<span data-ttu-id="65740-112">O aplicativo que você criará é um site de lista de usuários simples.</span><span class="sxs-lookup"><span data-stu-id="65740-112">The application you'll be building is a simple user list website.</span></span> <span data-ttu-id="65740-113">Os usuários podem inserir, exibir e atualizar as informações do usuário.</span><span class="sxs-lookup"><span data-stu-id="65740-113">Users can enter, view, and update user information.</span></span>

![Site de exemplo](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

<span data-ttu-id="65740-115">Você pode baixar o VB e C# o projeto concluído [aqui](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).</span><span class="sxs-lookup"><span data-stu-id="65740-115">You can download the VB and C# completed project [here](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).</span></span>

## <a name="creating-the-web-application"></a><span data-ttu-id="65740-116">Criando o aplicativo Web</span><span class="sxs-lookup"><span data-stu-id="65740-116">Creating the Web Application</span></span>

<span data-ttu-id="65740-117">Para iniciar o tutorial, abra o Visual Studio 2010 e crie um novo projeto usando o modelo de *aplicativo Web do ASP.NET MVC 3* .</span><span class="sxs-lookup"><span data-stu-id="65740-117">To start the tutorial, open Visual Studio 2010 and create a new project using the *ASP.NET MVC 3 Web Application* template.</span></span> <span data-ttu-id="65740-118">Nomeie o aplicativo &quot;Mvc3Razor&quot;.</span><span class="sxs-lookup"><span data-stu-id="65740-118">Name the application &quot;Mvc3Razor&quot;.</span></span>

<span data-ttu-id="65740-119">[![novo projeto MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="65740-119">[![New MVC 3 project](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span></span>

<span data-ttu-id="65740-120">Na caixa de diálogo **novo projeto ASP.NET MVC 3** , selecione **aplicativo de Internet**, selecione o mecanismo de exibição Razor e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="65740-120">In the **New ASP.NET MVC 3 Project** dialog, select **Internet Application**, select the Razor view engine, and then click **OK**.</span></span>

![Caixa de diálogo novo projeto ASP.NET MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

<span data-ttu-id="65740-122">Neste tutorial, você não usará o provedor de associação do ASP.NET, portanto, poderá excluir todos os arquivos associados ao logon e à associação.</span><span class="sxs-lookup"><span data-stu-id="65740-122">In this tutorial you will not be using the ASP.NET membership provider, so you can delete all the files associated with logon and membership.</span></span> <span data-ttu-id="65740-123">Em **Gerenciador de soluções**, remova os seguintes arquivos e diretórios:</span><span class="sxs-lookup"><span data-stu-id="65740-123">In **Solution Explorer**, remove the following files and directories:</span></span>

- <span data-ttu-id="65740-124">*Controllers\AccountController*</span><span class="sxs-lookup"><span data-stu-id="65740-124">*Controllers\AccountController*</span></span>
- <span data-ttu-id="65740-125">*Models\AccountModels*</span><span class="sxs-lookup"><span data-stu-id="65740-125">*Models\AccountModels*</span></span>
- <span data-ttu-id="65740-126">*Views\Shared\\_LogOnPartial*</span><span class="sxs-lookup"><span data-stu-id="65740-126">*Views\Shared\\_LogOnPartial*</span></span>
- <span data-ttu-id="65740-127">*Views\Account* (e todos os arquivos neste diretório)</span><span class="sxs-lookup"><span data-stu-id="65740-127">*Views\Account* (and all the files in this directory)</span></span>

![Soln exp](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

<span data-ttu-id="65740-129">Edite o arquivo <em>layout. cshtml do\_</em> e substitua a marcação dentro do elemento `<div>` chamado `logindisplay` pela mensagem <em>&quot;</em>logon desabilitado&quot;.</span><span class="sxs-lookup"><span data-stu-id="65740-129">Edit the <em>\_Layout.cshtml</em> file and replace the markup inside the `<div>` element named `logindisplay` with the message <em>&quot;</em>Login Disabled&quot;.</span></span> <span data-ttu-id="65740-130">O exemplo a seguir mostra a nova marcação:</span><span class="sxs-lookup"><span data-stu-id="65740-130">The following example shows the new markup:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a><span data-ttu-id="65740-131">Adicionando o modelo</span><span class="sxs-lookup"><span data-stu-id="65740-131">Adding the Model</span></span>

<span data-ttu-id="65740-132">Em **Gerenciador de soluções**, clique com o botão direito do mouse na pasta *modelos* , selecione **Adicionar**e clique em **classe**.</span><span class="sxs-lookup"><span data-stu-id="65740-132">In **Solution Explorer**, right-click the *Models* folder, select **Add**, and then click **Class**.</span></span>

![Nova classe MDL de usuário](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

<span data-ttu-id="65740-134">Nome da classe `UserModel`.</span><span class="sxs-lookup"><span data-stu-id="65740-134">Name the class `UserModel`.</span></span> <span data-ttu-id="65740-135">Substitua o conteúdo do arquivo *UserModel* pelo código a seguir:</span><span class="sxs-lookup"><span data-stu-id="65740-135">Replace the contents of the *UserModel* file with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

<span data-ttu-id="65740-136">A classe `UserModel` representa os usuários.</span><span class="sxs-lookup"><span data-stu-id="65740-136">The `UserModel` class represents users.</span></span> <span data-ttu-id="65740-137">Cada membro da classe é anotado com o atributo [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) do namespace [Annotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) .</span><span class="sxs-lookup"><span data-stu-id="65740-137">Each member of the class is annotated with the [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute from the [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace.</span></span> <span data-ttu-id="65740-138">Os atributos no namespace [Annotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) fornecem validação automática do cliente e do lado do servidor para aplicativos Web.</span><span class="sxs-lookup"><span data-stu-id="65740-138">The attributes in the [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace provide automatic client- and server-side validation for web applications.</span></span>

<span data-ttu-id="65740-139">Abra a classe `HomeController` e adicione uma diretiva `using` para que você possa acessar as classes `UserModel` e `Users`:</span><span class="sxs-lookup"><span data-stu-id="65740-139">Open the `HomeController` class and add a `using` directive so that you can access the `UserModel` and `Users` classes:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

<span data-ttu-id="65740-140">Logo após a declaração de `HomeController`, adicione o comentário a seguir e a referência a uma classe `Users`:</span><span class="sxs-lookup"><span data-stu-id="65740-140">Just after the `HomeController` declaration, add the following comment and the reference to a `Users` class:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

<span data-ttu-id="65740-141">A classe `Users` é um armazenamento de dados na memória simplificado que você usará neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="65740-141">The `Users` class is a simplified, in-memory data store that you'll use in this tutorial.</span></span> <span data-ttu-id="65740-142">Em um aplicativo real, você usaria um banco de dados para armazenar informações do usuário.</span><span class="sxs-lookup"><span data-stu-id="65740-142">In a real application you would use a database to store user information.</span></span> <span data-ttu-id="65740-143">As primeiras linhas do arquivo de `HomeController` são mostradas no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="65740-143">The first few lines of the `HomeController` file are shown in the following example:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

<span data-ttu-id="65740-144">Compile o aplicativo de forma que o modelo de usuário estará disponível para o assistente de scaffolding na próxima etapa.</span><span class="sxs-lookup"><span data-stu-id="65740-144">Build the application so that the user model will be available to the scaffolding wizard in the next step.</span></span>

## <a name="creating-the-default-view"></a><span data-ttu-id="65740-145">Criando a exibição padrão</span><span class="sxs-lookup"><span data-stu-id="65740-145">Creating the Default View</span></span>

<span data-ttu-id="65740-146">A próxima etapa é adicionar um método de ação e uma exibição para exibir os usuários.</span><span class="sxs-lookup"><span data-stu-id="65740-146">The next step is to add an action method and view to display the users.</span></span>

<span data-ttu-id="65740-147">Exclua o arquivo *Views\Home\Index* existente.</span><span class="sxs-lookup"><span data-stu-id="65740-147">Delete the existing *Views\Home\Index* file.</span></span> <span data-ttu-id="65740-148">Você criará um novo arquivo de *índice* para exibir os usuários.</span><span class="sxs-lookup"><span data-stu-id="65740-148">You will create a new *Index* file to display the users.</span></span>

<span data-ttu-id="65740-149">Na classe `HomeController`, substitua o conteúdo do método `Index` pelo seguinte código:</span><span class="sxs-lookup"><span data-stu-id="65740-149">In the `HomeController` class, replace the contents of the `Index` method with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

<span data-ttu-id="65740-150">Clique com o botão direito do mouse dentro do método `Index` e clique em **Adicionar exibição**.</span><span class="sxs-lookup"><span data-stu-id="65740-150">Right-click inside the `Index` method and then click **Add View**.</span></span>

![Adicionar modo de exibição](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

<span data-ttu-id="65740-152">Selecione a opção **criar uma exibição fortemente tipada** .</span><span class="sxs-lookup"><span data-stu-id="65740-152">Select the **Create a strongly-typed view** option.</span></span> <span data-ttu-id="65740-153">Para **exibir a classe de dados**, selecione **Mvc3Razor. Models. UserModel**.</span><span class="sxs-lookup"><span data-stu-id="65740-153">For **View data class**, select **Mvc3Razor.Models.UserModel**.</span></span> <span data-ttu-id="65740-154">(Se você não vir **Mvc3Razor. Models. UserModel** na caixa de **classe exibir dados** , você precisará compilar o projeto.) Verifique se o mecanismo de exibição está definido como **Razor**.</span><span class="sxs-lookup"><span data-stu-id="65740-154">(If you don't see **Mvc3Razor.Models.UserModel** in the **View data class** box, you need to build the project.) Make sure that the view engine is set to **Razor**.</span></span> <span data-ttu-id="65740-155">Defina **Exibir conteúdo** como **lista** e clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="65740-155">Set **View content** to **List** and then click **Add**.</span></span>

![Adicionar exibição de índice](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

<span data-ttu-id="65740-157">A nova exibição aplica Scaffold automaticamente os dados do usuário que são passados para a exibição `Index`.</span><span class="sxs-lookup"><span data-stu-id="65740-157">The new view automatically scaffolds the user data that's passed to the `Index` view.</span></span> <span data-ttu-id="65740-158">Examine o arquivo *Views\Home\Index* gerado recentemente.</span><span class="sxs-lookup"><span data-stu-id="65740-158">Examine the newly generated *Views\Home\Index* file.</span></span> <span data-ttu-id="65740-159">Os links **criar novo**, **Editar**, **detalhes**e **excluir** não funcionam, mas o restante da página é funcional.</span><span class="sxs-lookup"><span data-stu-id="65740-159">The **Create New**, **Edit**, **Details**, and **Delete** links don't work, but the rest of the page is functional.</span></span> <span data-ttu-id="65740-160">Execute a página.</span><span class="sxs-lookup"><span data-stu-id="65740-160">Run the page.</span></span> <span data-ttu-id="65740-161">Você verá uma lista de usuários.</span><span class="sxs-lookup"><span data-stu-id="65740-161">You see a list of users.</span></span>

![Página de índice](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

<span data-ttu-id="65740-163">Abra o arquivo *index. cshtml* e substitua a marcação de `ActionLink` para **Editar**, **detalhes**e **excluir** com o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="65740-163">Open the *Index.cshtml* file and replace the `ActionLink` markup for **Edit**, **Details**, and **Delete** with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

<span data-ttu-id="65740-164">O nome de usuário é usado como a ID para localizar o registro selecionado nos links **Editar**, **detalhes**e **excluir** .</span><span class="sxs-lookup"><span data-stu-id="65740-164">The user name is used as the ID to find the selected record in the **Edit**, **Details**, and **Delete** links.</span></span>

## <a name="creating-the-details-view"></a><span data-ttu-id="65740-165">Criando a exibição de detalhes</span><span class="sxs-lookup"><span data-stu-id="65740-165">Creating the Details View</span></span>

<span data-ttu-id="65740-166">A próxima etapa é adicionar um método de ação `Details` e exibir para exibir detalhes do usuário.</span><span class="sxs-lookup"><span data-stu-id="65740-166">The next step is to add a `Details` action method and view in order to display user details.</span></span>

![Detalhes](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

<span data-ttu-id="65740-168">Adicione o seguinte método `Details` ao controlador Home:</span><span class="sxs-lookup"><span data-stu-id="65740-168">Add the following `Details` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

<span data-ttu-id="65740-169">Clique com o botão direito do mouse dentro do método `Details` e selecione <strong>Adicionar exibição</strong>.</span><span class="sxs-lookup"><span data-stu-id="65740-169">Right-click inside the `Details` method and then select <strong>Add View</strong>.</span></span> <span data-ttu-id="65740-170">Verifique se a caixa de <strong>classe exibir dados</strong> contém <strong>Mvc3Razor. Models. UserModel</strong><em>.</em></span><span class="sxs-lookup"><span data-stu-id="65740-170">Verify that the <strong>View data class</strong> box contains <strong>Mvc3Razor.Models.UserModel</strong><em>.</em></span></span> <span data-ttu-id="65740-171">Defina <strong>Exibir conteúdo</strong> como <strong>detalhes</strong> e clique em <strong>Adicionar</strong>.</span><span class="sxs-lookup"><span data-stu-id="65740-171">Set <strong>View content</strong> to <strong>Details</strong> and then click <strong>Add</strong>.</span></span>

![Adicionar exibição de detalhes](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

<span data-ttu-id="65740-173">Execute o aplicativo e selecione um link detalhes.</span><span class="sxs-lookup"><span data-stu-id="65740-173">Run the application and select a details link.</span></span> <span data-ttu-id="65740-174">O scaffolding automático mostra cada propriedade no modelo.</span><span class="sxs-lookup"><span data-stu-id="65740-174">The automatic scaffolding shows each property in the model.</span></span>

![Detalhes](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a><span data-ttu-id="65740-176">Criando o modo de exibição de edição</span><span class="sxs-lookup"><span data-stu-id="65740-176">Creating the Edit View</span></span>

<span data-ttu-id="65740-177">Adicione o seguinte método `Edit` ao controlador Home.</span><span class="sxs-lookup"><span data-stu-id="65740-177">Add the following `Edit` method to the home controller.</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

<span data-ttu-id="65740-178">Adicione uma exibição como nas etapas anteriores, mas defina **Exibir conteúdo** para **Editar**.</span><span class="sxs-lookup"><span data-stu-id="65740-178">Add a view as in the previous steps, but set **View content** to **Edit**.</span></span>

![Adicionar modo de exibição de edição](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

<span data-ttu-id="65740-180">Execute o aplicativo e edite o nome e o sobrenome de um dos usuários.</span><span class="sxs-lookup"><span data-stu-id="65740-180">Run the application and edit the first and last name of one of the users.</span></span> <span data-ttu-id="65740-181">Se você violar quaisquer `DataAnnotation` restrições que foram aplicadas à classe `UserModel`, ao enviar o formulário, você verá erros de validação que são produzidos pelo código do servidor.</span><span class="sxs-lookup"><span data-stu-id="65740-181">If you violate any `DataAnnotation` constraints that have been applied to the `UserModel` class, when you submit the form, you will see validation errors that are produced by server code.</span></span> <span data-ttu-id="65740-182">Por exemplo, se você alterar o nome &quot;Ana&quot; para &quot;um&quot;, quando enviar o formulário, o seguinte erro será exibido no formulário:</span><span class="sxs-lookup"><span data-stu-id="65740-182">For example, if you change the first name &quot;Ann&quot; to &quot;A&quot;, when you submit the form, the following error is displayed on the form:</span></span>

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

<span data-ttu-id="65740-183">Neste tutorial, você está tratando o nome de usuário como a chave primária.</span><span class="sxs-lookup"><span data-stu-id="65740-183">In this tutorial, you're treating the user name as the primary key.</span></span> <span data-ttu-id="65740-184">Portanto, a propriedade nome de usuário não pode ser alterada.</span><span class="sxs-lookup"><span data-stu-id="65740-184">Therefore, the user name property cannot be changed.</span></span> <span data-ttu-id="65740-185">No arquivo *Edit. cshtml* , logo após a instrução `Html.BeginForm`, defina o nome de usuário como um campo oculto.</span><span class="sxs-lookup"><span data-stu-id="65740-185">In the *Edit.cshtml* file, just after the `Html.BeginForm` statement, set the user name to be a hidden field.</span></span> <span data-ttu-id="65740-186">Isso faz com que a propriedade seja passada no modelo.</span><span class="sxs-lookup"><span data-stu-id="65740-186">This causes the property to be passed in the model.</span></span> <span data-ttu-id="65740-187">O fragmento de código a seguir mostra o posicionamento da instrução de `Hidden`:</span><span class="sxs-lookup"><span data-stu-id="65740-187">The following code fragment shows the placement of the `Hidden` statement:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

<span data-ttu-id="65740-188">Substitua o `TextBoxFor` e a marcação de `ValidationMessageFor` para o nome de usuário com uma chamada `DisplayFor`.</span><span class="sxs-lookup"><span data-stu-id="65740-188">Replace the `TextBoxFor` and `ValidationMessageFor` markup for the user name with a `DisplayFor` call.</span></span> <span data-ttu-id="65740-189">O método `DisplayFor` exibe a propriedade como um elemento somente leitura.</span><span class="sxs-lookup"><span data-stu-id="65740-189">The `DisplayFor` method displays the property as a read-only element.</span></span> <span data-ttu-id="65740-190">O exemplo a seguir mostra a marcação concluída.</span><span class="sxs-lookup"><span data-stu-id="65740-190">The following example shows the completed markup.</span></span> <span data-ttu-id="65740-191">As chamadas originais `TextBoxFor` e `ValidationMessageFor` são comentadas com os caracteres de início de comentário do Razor e comentário final (`@* *@`)</span><span class="sxs-lookup"><span data-stu-id="65740-191">The original `TextBoxFor` and `ValidationMessageFor` calls are commented out with the Razor begin-comment and end-comment characters (`@* *@`)</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a><span data-ttu-id="65740-192">Habilitando a validação do lado do cliente</span><span class="sxs-lookup"><span data-stu-id="65740-192">Enabling Client-Side Validation</span></span>

<span data-ttu-id="65740-193">Para habilitar a validação do lado do cliente no ASP.NET MVC 3, você deve definir dois sinalizadores e deve incluir três arquivos JavaScript.</span><span class="sxs-lookup"><span data-stu-id="65740-193">To enable client-side validation in ASP.NET MVC 3, you must set two flags and you must include three JavaScript files.</span></span>

<span data-ttu-id="65740-194">Abra o arquivo *Web. config* do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="65740-194">Open the application's *Web.config* file.</span></span> <span data-ttu-id="65740-195">Verifique se `that ClientValidationEnabled` e `UnobtrusiveJavaScriptEnabled` estão definidos como true nas configurações do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="65740-195">Verify `that ClientValidationEnabled` and `UnobtrusiveJavaScriptEnabled` are set to true in the application settings.</span></span> <span data-ttu-id="65740-196">O fragmento a seguir do arquivo *Web. config* raiz mostra as configurações corretas:</span><span class="sxs-lookup"><span data-stu-id="65740-196">The following fragment from the root *Web.config* file shows the correct settings:</span></span>

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

<span data-ttu-id="65740-197">A definição de `UnobtrusiveJavaScriptEnabled` como True habilita a validação de cliente discreta e não discreta.</span><span class="sxs-lookup"><span data-stu-id="65740-197">Setting `UnobtrusiveJavaScriptEnabled` to true enables unobtrusive Ajax and unobtrusive client validation.</span></span> <span data-ttu-id="65740-198">Quando você usa a validação discreta, as regras de validação são transformadas em atributos HTML5.</span><span class="sxs-lookup"><span data-stu-id="65740-198">When you use unobtrusive validation, the validation rules are turned into HTML5 attributes.</span></span> <span data-ttu-id="65740-199">Os nomes de atributo HTML5 podem consistir apenas em letras minúsculas, números e traços.</span><span class="sxs-lookup"><span data-stu-id="65740-199">HTML5 attribute names can consist of only lowercase letters, numbers, and dashes.</span></span>

<span data-ttu-id="65740-200">A configuração de `ClientValidationEnabled` como True habilita a validação do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="65740-200">Setting `ClientValidationEnabled` to true enables client-side validation.</span></span> <span data-ttu-id="65740-201">Ao definir essas chaves no arquivo *Web. config* do aplicativo, você habilita a validação do cliente e o JavaScript discreto para todo o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="65740-201">By setting these keys in the application *Web.config* file, you enable client validation and unobtrusive JavaScript for the entire application.</span></span> <span data-ttu-id="65740-202">Você também pode habilitar ou desabilitar essas configurações em modos de exibição individuais ou em métodos de controlador usando o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="65740-202">You can also enable or disable these settings in individual views or in controller methods using the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

<span data-ttu-id="65740-203">Você também precisa incluir vários arquivos JavaScript na exibição renderizada.</span><span class="sxs-lookup"><span data-stu-id="65740-203">You also need to include several JavaScript files in the rendered view.</span></span> <span data-ttu-id="65740-204">Uma maneira fácil de incluir o JavaScript em todos os modos de exibição é adicioná-los ao arquivo *Views\Shared\\_Layout. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="65740-204">An easy way to include the JavaScript in all views is to add them to the *Views\Shared\\_Layout.cshtml* file.</span></span> <span data-ttu-id="65740-205">Substitua o elemento `<head>` do arquivo *layout. cshtml de\_* pelo código a seguir:</span><span class="sxs-lookup"><span data-stu-id="65740-205">Replace the `<head>` element of the *\_Layout.cshtml* file with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

<span data-ttu-id="65740-206">Os dois primeiros scripts jQuery são hospedados pela CDN (rede de distribuição de conteúdo) do Microsoft Ajax.</span><span class="sxs-lookup"><span data-stu-id="65740-206">The first two jQuery scripts are hosted by the Microsoft Ajax Content Delivery Network (CDN).</span></span> <span data-ttu-id="65740-207">Aproveitando a CDN do Microsoft Ajax, você pode melhorar significativamente o desempenho inicial de seus aplicativos.</span><span class="sxs-lookup"><span data-stu-id="65740-207">By taking advantage of the Microsoft Ajax CDN, you can significantly improve the first-hit performance of your applications.</span></span>

<span data-ttu-id="65740-208">Execute o aplicativo e clique em um link de edição.</span><span class="sxs-lookup"><span data-stu-id="65740-208">Run the application and click an edit link.</span></span> <span data-ttu-id="65740-209">Exiba a origem da página no navegador.</span><span class="sxs-lookup"><span data-stu-id="65740-209">View the page's source in the browser.</span></span> <span data-ttu-id="65740-210">A origem do navegador mostra muitos atributos do formulário `data-val` (para validação de dados).</span><span class="sxs-lookup"><span data-stu-id="65740-210">The browser source shows many attributes of the form `data-val` (for data validation).</span></span> <span data-ttu-id="65740-211">Quando a validação do cliente e o JavaScript discreto estão habilitados, os campos de entrada com uma regra de validação do cliente contêm o atributo `data-val="true"` para disparar a validação de cliente discreta.</span><span class="sxs-lookup"><span data-stu-id="65740-211">When client validation and unobtrusive JavaScript is enabled, input fields with a client-validation rule contain the `data-val="true"` attribute to trigger unobtrusive client validation.</span></span> <span data-ttu-id="65740-212">Por exemplo, o campo `City` no modelo foi decorado com o atributo [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) , que resulta no HTML mostrado no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="65740-212">For example, the `City` field in the model was decorated with the [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute, which results in the HTML shown in the following example:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

<span data-ttu-id="65740-213">Para cada regra de validação de cliente, é adicionado um atributo que tem o formulário `data-val-rulename="message"`.</span><span class="sxs-lookup"><span data-stu-id="65740-213">For each client-validation rule, an attribute is added that has the form `data-val-rulename="message"`.</span></span> <span data-ttu-id="65740-214">Usando o exemplo de campo `City` mostrado anteriormente, a regra de validação de cliente necessária gera o atributo `data-val-required` e a mensagem &quot;o campo cidade é necessário&quot;.</span><span class="sxs-lookup"><span data-stu-id="65740-214">Using the `City` field example shown earlier, the required client-validation rule generates the `data-val-required` attribute and the message &quot;The City field is required&quot;.</span></span> <span data-ttu-id="65740-215">Execute o aplicativo, edite um dos usuários e desmarque o campo `City`.</span><span class="sxs-lookup"><span data-stu-id="65740-215">Run the application, edit one of the users, and clear the `City` field.</span></span> <span data-ttu-id="65740-216">Ao sair do campo, você verá uma mensagem de erro de validação no lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="65740-216">When you tab out of the field, you see a client-side validation error message.</span></span>

![Cidade necessária](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

<span data-ttu-id="65740-218">Da mesma forma, para cada parâmetro na regra de validação do cliente, é adicionado um atributo que tem o formulário `data-val-rulename-paramname=paramvalue`.</span><span class="sxs-lookup"><span data-stu-id="65740-218">Similarly, for each parameter in the client-validation rule, an attribute is added that has the form `data-val-rulename-paramname=paramvalue`.</span></span> <span data-ttu-id="65740-219">Por exemplo, a propriedade `FirstName` é anotada com o atributo [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) e especifica um comprimento mínimo de 3 e um comprimento máximo de 8.</span><span class="sxs-lookup"><span data-stu-id="65740-219">For example, the `FirstName` property is annotated with the [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) attribute and specifies a minimum length of 3 and a maximum length of 8.</span></span> <span data-ttu-id="65740-220">A regra de validação de dados chamada `length` tem o nome do parâmetro `max` e o valor do parâmetro 8.</span><span class="sxs-lookup"><span data-stu-id="65740-220">The data validation rule named `length` has the parameter name `max` and the parameter value 8.</span></span> <span data-ttu-id="65740-221">O código a seguir mostra o HTML que é gerado para o campo `FirstName` quando você edita um dos usuários:</span><span class="sxs-lookup"><span data-stu-id="65740-221">The following shows the HTML that is generated for the `FirstName` field when you edit one of the users:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

<span data-ttu-id="65740-222">Para obter mais informações sobre a validação de cliente discreta, consulte a entrada [validação de cliente discreta no ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) no blog de Brad Wilson.</span><span class="sxs-lookup"><span data-stu-id="65740-222">For more information about unobtrusive client validation, see the entry [Unobtrusive Client Validation in ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) in Brad Wilson's blog.</span></span>

> [!NOTE]
> <span data-ttu-id="65740-223">No ASP.NET MVC 3 beta, às vezes você precisa enviar o formulário para iniciar a validação do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="65740-223">In ASP.NET MVC 3 Beta, you sometimes need to submit the form in order to start client-side validation.</span></span> <span data-ttu-id="65740-224">Isso pode ser alterado para a versão final.</span><span class="sxs-lookup"><span data-stu-id="65740-224">This might be changed for the final release.</span></span>

## <a name="creating-the-create-view"></a><span data-ttu-id="65740-225">Criando o modo de exibição de criação</span><span class="sxs-lookup"><span data-stu-id="65740-225">Creating the Create View</span></span>

<span data-ttu-id="65740-226">A próxima etapa é adicionar um método de ação `Create` e exibir para permitir que o usuário crie um novo usuário.</span><span class="sxs-lookup"><span data-stu-id="65740-226">The next step is to add a `Create` action method and view in order to enable the user to create a new user.</span></span> <span data-ttu-id="65740-227">Adicione o seguinte método `Create` ao controlador Home:</span><span class="sxs-lookup"><span data-stu-id="65740-227">Add the following `Create` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

<span data-ttu-id="65740-228">Adicione uma exibição como nas etapas anteriores, mas defina **Exibir conteúdo** a ser **criado**.</span><span class="sxs-lookup"><span data-stu-id="65740-228">Add a view as in the previous steps, but set **View content** to **Create**.</span></span>

![Create View](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

<span data-ttu-id="65740-230">Execute o aplicativo, selecione o link **criar** e adicione um novo usuário.</span><span class="sxs-lookup"><span data-stu-id="65740-230">Run the application, select the **Create** link, and add a new user.</span></span> <span data-ttu-id="65740-231">O método `Create` aproveita automaticamente a validação do lado do cliente e do servidor.</span><span class="sxs-lookup"><span data-stu-id="65740-231">The `Create` method automatically takes advantage of client-side and server-side validation.</span></span> <span data-ttu-id="65740-232">Tente inserir um nome de usuário que contenha espaço em branco, como &quot;Ben X&quot;.</span><span class="sxs-lookup"><span data-stu-id="65740-232">Try to enter a user name that contains white space, such as &quot;Ben X&quot;.</span></span> <span data-ttu-id="65740-233">Quando você faz a Tabulação do campo nome de usuário, um erro de validação no lado do cliente (`White space is not allowed`) é exibido.</span><span class="sxs-lookup"><span data-stu-id="65740-233">When you tab out of the user name field, a client-side validation error (`White space is not allowed`) is displayed.</span></span>

## <a name="add-the-delete-method"></a><span data-ttu-id="65740-234">Adicionar o método Delete</span><span class="sxs-lookup"><span data-stu-id="65740-234">Add the Delete method</span></span>

<span data-ttu-id="65740-235">Para concluir o tutorial, adicione o seguinte método `Delete` ao controlador Home:</span><span class="sxs-lookup"><span data-stu-id="65740-235">To complete the tutorial, add the following `Delete` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

<span data-ttu-id="65740-236">Adicione uma exibição de `Delete` como nas etapas anteriores, definindo o **conteúdo de exibição** a ser **excluído**.</span><span class="sxs-lookup"><span data-stu-id="65740-236">Add a `Delete` view as in the previous steps, setting **View content** to **Delete**.</span></span>

![Excluir Exibição](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

<span data-ttu-id="65740-238">Agora você tem um aplicativo ASP.NET MVC 3 simples, mas totalmente funcional, com validação.</span><span class="sxs-lookup"><span data-stu-id="65740-238">You now have a simple but fully functional ASP.NET MVC 3 application with validation.</span></span>
