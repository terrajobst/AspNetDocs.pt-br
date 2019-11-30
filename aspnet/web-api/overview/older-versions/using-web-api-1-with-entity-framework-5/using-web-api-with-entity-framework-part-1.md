---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: 'Parte 1: visão geral e criação do projeto | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/03/2012
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: a76a18f2bd95969358452085ef342fdca8a386e2
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600321"
---
# <a name="part-1-overview-and-creating-the-project"></a><span data-ttu-id="a19c9-102">Parte 1: visão geral e criação do projeto</span><span class="sxs-lookup"><span data-stu-id="a19c9-102">Part 1: Overview and Creating the Project</span></span>

<span data-ttu-id="a19c9-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a19c9-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="a19c9-104">Baixar projeto concluído</span><span class="sxs-lookup"><span data-stu-id="a19c9-104">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

<span data-ttu-id="a19c9-105">Entity Framework é uma estrutura de mapeamento relacional/objeto.</span><span class="sxs-lookup"><span data-stu-id="a19c9-105">Entity Framework is an object/relational mapping framework.</span></span> <span data-ttu-id="a19c9-106">Ele mapeia os objetos de domínio em seu código para entidades em um banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="a19c9-106">It maps the domain objects in your code to entities in a relational database.</span></span> <span data-ttu-id="a19c9-107">Na maioria das vezes, você não precisa se preocupar com a camada de banco de dados, pois Entity Framework cuida dela para você.</span><span class="sxs-lookup"><span data-stu-id="a19c9-107">For the most part, you do not have to worry about the database layer, because Entity Framework takes care of it for you.</span></span> <span data-ttu-id="a19c9-108">Seu código manipula os objetos e as alterações são persistidas em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a19c9-108">Your code manipulates the objects, and changes are persisted to a database.</span></span>

## <a name="about-the-tutorial"></a><span data-ttu-id="a19c9-109">Sobre o tutorial</span><span class="sxs-lookup"><span data-stu-id="a19c9-109">About the Tutorial</span></span>

<span data-ttu-id="a19c9-110">Neste tutorial, você criará um aplicativo de armazenamento simples.</span><span class="sxs-lookup"><span data-stu-id="a19c9-110">In this tutorial, you will create a simple store application.</span></span> <span data-ttu-id="a19c9-111">Há duas partes principais para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="a19c9-111">There are two main parts to the application.</span></span> <span data-ttu-id="a19c9-112">Os usuários normais podem exibir produtos e criar pedidos:</span><span class="sxs-lookup"><span data-stu-id="a19c9-112">Normal users can view products and create orders:</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

<span data-ttu-id="a19c9-113">Os administradores podem criar, excluir ou editar produtos:</span><span class="sxs-lookup"><span data-stu-id="a19c9-113">Administrators can create, delete, or edit products:</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a><span data-ttu-id="a19c9-114">Habilidades que você aprenderá</span><span class="sxs-lookup"><span data-stu-id="a19c9-114">Skills You'll Learn</span></span>

<span data-ttu-id="a19c9-115">Veja o que você aprenderá:</span><span class="sxs-lookup"><span data-stu-id="a19c9-115">Here's what you'll learn:</span></span>

- <span data-ttu-id="a19c9-116">Como usar Entity Framework com ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="a19c9-116">How to use Entity Framework with ASP.NET Web API.</span></span>
- <span data-ttu-id="a19c9-117">Como usar knockout. js para criar uma interface do usuário dinâmica do cliente.</span><span class="sxs-lookup"><span data-stu-id="a19c9-117">How to use knockout.js to create a dynamic client UI.</span></span>
- <span data-ttu-id="a19c9-118">Como usar a autenticação de formulários com a API da Web para autenticar usuários.</span><span class="sxs-lookup"><span data-stu-id="a19c9-118">How to use forms authentication with Web API to authenticate users.</span></span>

<span data-ttu-id="a19c9-119">Embora este tutorial seja independente, você talvez queira ler primeiro os seguintes tutoriais:</span><span class="sxs-lookup"><span data-stu-id="a19c9-119">Although this tutorial is self-contained, you might want to read the following tutorials first:</span></span>

- [<span data-ttu-id="a19c9-120">Sua primeira ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="a19c9-120">Your First ASP.NET Web API</span></span>](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [<span data-ttu-id="a19c9-121">Criando uma API Web que dá suporte a operações CRUD</span><span class="sxs-lookup"><span data-stu-id="a19c9-121">Creating a Web API that Supports CRUD Operations</span></span>](../creating-a-web-api-that-supports-crud-operations.md)

<span data-ttu-id="a19c9-122">Algum conhecimento do [ASP.NET MVC](../../../../mvc/index.md) também é útil.</span><span class="sxs-lookup"><span data-stu-id="a19c9-122">Some knowledge of [ASP.NET MVC](../../../../mvc/index.md) is also helpful.</span></span>

## <a name="overview"></a><span data-ttu-id="a19c9-123">{1&gt;Visão Geral&lt;1}</span><span class="sxs-lookup"><span data-stu-id="a19c9-123">Overview</span></span>

<span data-ttu-id="a19c9-124">Em um alto nível, aqui está a arquitetura do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="a19c9-124">At a high level, here is the architecture of the application:</span></span>

- <span data-ttu-id="a19c9-125">O ASP.NET MVC gera as páginas HTML para o cliente.</span><span class="sxs-lookup"><span data-stu-id="a19c9-125">ASP.NET MVC generates the HTML pages for the client.</span></span>
- <span data-ttu-id="a19c9-126">ASP.NET Web API expõe operações CRUD nos dados (produtos e pedidos).</span><span class="sxs-lookup"><span data-stu-id="a19c9-126">ASP.NET Web API exposes CRUD operations on the data (products and orders).</span></span>
- <span data-ttu-id="a19c9-127">Entity Framework converte os modelos usados C# pela API da Web em entidades de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a19c9-127">Entity Framework translates the C# models used by Web API into database entities.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

<span data-ttu-id="a19c9-128">O diagrama a seguir mostra como os objetos de domínio são representados em várias camadas do aplicativo: a camada de banco de dados, o modelo de objeto e, por fim, o formato de conexão, que é usado para transmitir dados para o cliente via HTTP.</span><span class="sxs-lookup"><span data-stu-id="a19c9-128">The following diagram shows how the domain objects are represented at various layers of the application: The database layer, the object model, and finally the wire format, which is used to transmit data to the client via HTTP.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="a19c9-129">Criar o projeto do Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a19c9-129">Create the Visual Studio Project</span></span>

<span data-ttu-id="a19c9-130">Você pode criar o projeto do tutorial usando o Visual Web Developer Express ou a versão completa do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a19c9-130">You can create the tutorial project using either Visual Web Developer Express or the full version of Visual Studio.</span></span>

<span data-ttu-id="a19c9-131">Na página **inicial** , clique em **novo projeto**.</span><span class="sxs-lookup"><span data-stu-id="a19c9-131">From the **Start** page, click **New Project**.</span></span>

<span data-ttu-id="a19c9-132">No painel **modelos** , selecione **modelos instalados** e expanda o **nó C# Visual** .</span><span class="sxs-lookup"><span data-stu-id="a19c9-132">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="a19c9-133">Em **Visual C#** , selecione **Web**.</span><span class="sxs-lookup"><span data-stu-id="a19c9-133">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="a19c9-134">Na lista de modelos de projeto, selecione **aplicativo Web ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="a19c9-134">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="a19c9-135">Nomeie o projeto "ProductStore" e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="a19c9-135">Name the project "ProductStore" and click **OK**.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

<span data-ttu-id="a19c9-136">Na caixa de diálogo **novo projeto ASP.NET MVC 4** , selecione **aplicativo de Internet** e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="a19c9-136">In the **New ASP.NET MVC 4 Project** dialog, select **Internet Application** and click **OK**.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

<span data-ttu-id="a19c9-137">O modelo "aplicativo de Internet" cria um aplicativo MVC ASP.NET que dá suporte à autenticação de formulários.</span><span class="sxs-lookup"><span data-stu-id="a19c9-137">The "Internet Application" template creates an ASP.NET MVC application that supports forms authentication.</span></span> <span data-ttu-id="a19c9-138">Se você executar o aplicativo agora, ele já terá alguns recursos:</span><span class="sxs-lookup"><span data-stu-id="a19c9-138">If you run the application now, it already has some features:</span></span>

- <span data-ttu-id="a19c9-139">Novos usuários podem se registrar clicando no link "registrar" no canto superior direito.</span><span class="sxs-lookup"><span data-stu-id="a19c9-139">New users can register by clicking the "Register" link in the upper right corner.</span></span>
- <span data-ttu-id="a19c9-140">Os usuários registrados podem fazer logon clicando no link "logon".</span><span class="sxs-lookup"><span data-stu-id="a19c9-140">Registered users can log in by clicking the "Log in" link.</span></span>

<span data-ttu-id="a19c9-141">As informações de associação são mantidas em um banco de dados que é criado automaticamente.</span><span class="sxs-lookup"><span data-stu-id="a19c9-141">Membership information is persisted in a database that gets created automatically.</span></span> <span data-ttu-id="a19c9-142">Para obter mais informações sobre a autenticação de formulários no ASP.NET MVC, consulte [passo a passos: usando a autenticação de formulários no ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).</span><span class="sxs-lookup"><span data-stu-id="a19c9-142">For more information about forms authentication in ASP.NET MVC, see [Walkthrough: Using Forms Authentication in ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).</span></span>

## <a name="update-the-css-file"></a><span data-ttu-id="a19c9-143">Atualizar o arquivo CSS</span><span class="sxs-lookup"><span data-stu-id="a19c9-143">Update the CSS File</span></span>

<span data-ttu-id="a19c9-144">Essa etapa é superficial, mas fará com que as páginas sejam renderizadas como as capturas de tela anteriores.</span><span class="sxs-lookup"><span data-stu-id="a19c9-144">This step is cosmetic, but it will make the pages render like the earlier screen shots.</span></span>

<span data-ttu-id="a19c9-145">Em Gerenciador de Soluções, expanda a pasta conteúdo e abra o arquivo chamado site. css.</span><span class="sxs-lookup"><span data-stu-id="a19c9-145">In Solution Explorer, expand the Content folder and open the file named Site.css.</span></span> <span data-ttu-id="a19c9-146">Adicione os seguintes estilos de CSS:</span><span class="sxs-lookup"><span data-stu-id="a19c9-146">Add the following CSS styles:</span></span>

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

> [!div class="step-by-step"]
> [<span data-ttu-id="a19c9-147">Avançar</span><span class="sxs-lookup"><span data-stu-id="a19c9-147">Next</span></span>](using-web-api-with-entity-framework-part-2.md)
