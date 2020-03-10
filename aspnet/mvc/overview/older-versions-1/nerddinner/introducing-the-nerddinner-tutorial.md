---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: Apresentando o tutorial do NerdDinner | Microsoft Docs
author: shanselman
description: A melhor maneira de aprender uma nova estrutura é criar algo com ela. Este tutorial explica como criar um aplicativo pequeno, mas completo usando o ASP.NE...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 154cfe6694cf723c0a1f8e33bfdb42c97594518f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78580573"
---
# <a name="introducing-the-nerddinner-tutorial"></a><span data-ttu-id="2c21f-104">Introdução ao Tutorial do NerdDinner</span><span class="sxs-lookup"><span data-stu-id="2c21f-104">Introducing the NerdDinner Tutorial</span></span>

<span data-ttu-id="2c21f-105">por [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="2c21f-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

[<span data-ttu-id="2c21f-106">Baixar PDF</span><span class="sxs-lookup"><span data-stu-id="2c21f-106">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="2c21f-107">A melhor maneira de aprender uma nova estrutura é criar algo com ela.</span><span class="sxs-lookup"><span data-stu-id="2c21f-107">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="2c21f-108">Este tutorial explica como criar um aplicativo pequeno, mas completo usando o ASP.NET MVC 1, e apresenta alguns dos conceitos básicos por trás dele.</span><span class="sxs-lookup"><span data-stu-id="2c21f-108">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC 1, and introduces some of the core concepts behind it.</span></span>
> 
> <span data-ttu-id="2c21f-109">Se você estiver usando o ASP.NET MVC 3, recomendamos seguir as [introdução com](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) os tutoriais da [loja de música](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) MVC 3 ou MVC.</span><span class="sxs-lookup"><span data-stu-id="2c21f-109">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>

## <a name="nerddinner-tutorial"></a><span data-ttu-id="2c21f-110">Tutorial do NerdDinner</span><span class="sxs-lookup"><span data-stu-id="2c21f-110">NerdDinner Tutorial</span></span>

<span data-ttu-id="2c21f-111">A melhor maneira de aprender uma nova estrutura é criar algo com ela.</span><span class="sxs-lookup"><span data-stu-id="2c21f-111">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="2c21f-112">Este tutorial explica como criar um aplicativo pequeno, mas completo usando o ASP.NET MVC, e apresenta alguns dos conceitos básicos por trás dele.</span><span class="sxs-lookup"><span data-stu-id="2c21f-112">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC, and introduces some of the core concepts behind it.</span></span>

<span data-ttu-id="2c21f-113">O aplicativo que vamos criar é chamado de "NerdDinner".</span><span class="sxs-lookup"><span data-stu-id="2c21f-113">The application we are going to build is called "NerdDinner".</span></span> <span data-ttu-id="2c21f-114">O NerdDinner fornece uma maneira fácil para as pessoas encontrarem e organizarem os jantares online:</span><span class="sxs-lookup"><span data-stu-id="2c21f-114">NerdDinner provides an easy way for people to find and organize dinners online:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image1.png)

<span data-ttu-id="2c21f-115">O NerdDinner permite que os usuários registrados criem, editem e excluam jantares.</span><span class="sxs-lookup"><span data-stu-id="2c21f-115">NerdDinner enables registered users to create, edit and delete dinners.</span></span> <span data-ttu-id="2c21f-116">Ele impõe um conjunto consistente de regras de validação e de negócios em todo o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="2c21f-116">It enforces a consistent set of validation and business rules across the application:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image2.png)

<span data-ttu-id="2c21f-117">Os visitantes podem usar um mapa baseado em AJAX para pesquisar os próximos JANTARS sendo mantidos próximos deles:</span><span class="sxs-lookup"><span data-stu-id="2c21f-117">Visitors can use an AJAX-based map to search for upcoming dinners being held near them:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image3.png)

<span data-ttu-id="2c21f-118">Clicar em um jantar vai levá-los para uma página de detalhes em que eles podem saber mais sobre isso:</span><span class="sxs-lookup"><span data-stu-id="2c21f-118">Clicking a dinner will take them to a details page where they can learn more about it:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image4.png)

<span data-ttu-id="2c21f-119">Se eles estiverem interessados em participar do jantar, eles poderão fazer logon ou se registrarem no site:</span><span class="sxs-lookup"><span data-stu-id="2c21f-119">If they are interested in attending the dinner they can login or register on the site:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image5.png)

<span data-ttu-id="2c21f-120">Em seguida, eles podem clicar em um link RSVP baseado em AJAX para participar do evento:</span><span class="sxs-lookup"><span data-stu-id="2c21f-120">They can then click an AJAX-based RSVP link to attend the event:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a><span data-ttu-id="2c21f-121">Implementando NerdDinner</span><span class="sxs-lookup"><span data-stu-id="2c21f-121">Implementing NerdDinner</span></span>

<span data-ttu-id="2c21f-122">Vamos começar nosso aplicativo NerdDinner usando o comando File-&gt;New Project no Visual Studio para criar um projeto MVC totalmente novo ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2c21f-122">We are going to begin our NerdDinner application by using the File-&gt;New Project command within Visual Studio to create a brand new ASP.NET MVC project.</span></span> <span data-ttu-id="2c21f-123">Em seguida, adicionaremos recursos e funcionalidades de forma incremental.</span><span class="sxs-lookup"><span data-stu-id="2c21f-123">We will then incrementally add functionality and features.</span></span> <span data-ttu-id="2c21f-124">Ao longo do caminho, abordaremos:</span><span class="sxs-lookup"><span data-stu-id="2c21f-124">Along the way we'll cover:</span></span>

1. [<span data-ttu-id="2c21f-125">Como criar um novo projeto MVC do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="2c21f-125">How to create a new ASP.NET MVC Project</span></span>](create-a-new-aspnet-mvc-project.md)
2. [<span data-ttu-id="2c21f-126">Como criar um banco de dados</span><span class="sxs-lookup"><span data-stu-id="2c21f-126">How to create a database</span></span>](create-a-database.md)
3. [<span data-ttu-id="2c21f-127">Como criar um modelo com validações de regra de negócio</span><span class="sxs-lookup"><span data-stu-id="2c21f-127">How to build a model with business rule validations</span></span>](build-a-model-with-business-rule-validations.md)
4. [<span data-ttu-id="2c21f-128">Como usar controladores e exibições para implementar uma interface de usuário de listagem/detalhes</span><span class="sxs-lookup"><span data-stu-id="2c21f-128">How to use controllers and views to implement a listing/details UI</span></span>](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
5. [<span data-ttu-id="2c21f-129">Como fornecer suporte à entrada do formulário de dados CRUD (criar, ler, atualizar, excluir)</span><span class="sxs-lookup"><span data-stu-id="2c21f-129">How to provide CRUD (create, read, update, delete) data form entry support</span></span>](provide-crud-create-read-update-delete-data-form-entry-support.md)
6. [<span data-ttu-id="2c21f-130">Como usar ViewData e implementar classes ViewModel</span><span class="sxs-lookup"><span data-stu-id="2c21f-130">How to use ViewData and implement ViewModel classes</span></span>](use-viewdata-and-implement-viewmodel-classes.md)
7. [<span data-ttu-id="2c21f-131">Como reutilizar a interface do usuário usando páginas mestras e parciais</span><span class="sxs-lookup"><span data-stu-id="2c21f-131">How to re-use UI using master pages and partials</span></span>](re-use-ui-using-master-pages-and-partials.md)
8. [<span data-ttu-id="2c21f-132">Como implementar a paginação de dados eficiente</span><span class="sxs-lookup"><span data-stu-id="2c21f-132">How to implement efficient data paging</span></span>](implement-efficient-data-paging.md)
9. [<span data-ttu-id="2c21f-133">Como proteger aplicativos usando autenticação e autorização</span><span class="sxs-lookup"><span data-stu-id="2c21f-133">How to secure applications using authentication and authorization</span></span>](secure-applications-using-authentication-and-authorization.md)
10. [<span data-ttu-id="2c21f-134">Como usar o AJAX para fornecer atualizações dinâmicas</span><span class="sxs-lookup"><span data-stu-id="2c21f-134">How to use AJAX to deliver dynamic updates</span></span>](use-ajax-to-deliver-dynamic-updates.md)
11. [<span data-ttu-id="2c21f-135">Como usar o AJAX para implementar cenários de mapeamento</span><span class="sxs-lookup"><span data-stu-id="2c21f-135">How to use AJAX to implement mapping scenarios</span></span>](use-ajax-to-implement-mapping-scenarios.md)
12. [<span data-ttu-id="2c21f-136">Como habilitar o teste de unidade automatizado</span><span class="sxs-lookup"><span data-stu-id="2c21f-136">How to enable automated unit testing</span></span>](enable-automated-unit-testing.md)

<span data-ttu-id="2c21f-137">Você pode criar sua própria cópia do NerdDinner a partir do zero, concluindo cada etapa neste capítulo.</span><span class="sxs-lookup"><span data-stu-id="2c21f-137">You can build your own copy of NerdDinner from scratch by completing each step we walkthrough in this chapter.</span></span> <span data-ttu-id="2c21f-138">Como alternativa, você pode baixar uma versão completa do código-fonte aqui: [NerdDinner no GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span><span class="sxs-lookup"><span data-stu-id="2c21f-138">Alternatively, you can download a completed version of the source code here: [NerdDinner on GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span></span> <span data-ttu-id="2c21f-139">Também é possível também [baixar uma versão de PDF gratuita deste tutorial](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) se você quiser ler o tutorial offline.</span><span class="sxs-lookup"><span data-stu-id="2c21f-139">You can also optionally also [download a free PDF version of this tutorial](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) if you want to read the tutorial offline.</span></span>

<span data-ttu-id="2c21f-140">Você pode usar o Visual Studio 2008 ou o Visual Web Developer 2008 Express gratuito para compilar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2c21f-140">You can use either Visual Studio 2008 or the free Visual Web Developer 2008 Express to build the application.</span></span> <span data-ttu-id="2c21f-141">Você pode usar SQL Server ou o SQL Server Express gratuito para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="2c21f-141">You can use either SQL Server or the free SQL Server Express for the database.</span></span>

<span data-ttu-id="2c21f-142">Você pode instalar o ASP.NET MVC, o Visual Web Developer 2008 Express e o SQL Server Express (tudo gratuito) usando a v2 do [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)</span><span class="sxs-lookup"><span data-stu-id="2c21f-142">You can install ASP.NET MVC, Visual Web Developer 2008 Express, and SQL Server Express (all free) using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)</span></span>

### <a name="now-lets-get-started"></a><span data-ttu-id="2c21f-143">Agora, vamos começar...</span><span class="sxs-lookup"><span data-stu-id="2c21f-143">Now let's get started....</span></span>

<span data-ttu-id="2c21f-144">Agora que já abordamos o que NerdDinner é, vamos acumular nossas mangas e escrever algum código.</span><span class="sxs-lookup"><span data-stu-id="2c21f-144">Now that we've covered what NerdDinner is, let's roll up our sleeves and write some code.</span></span>

<span data-ttu-id="2c21f-145">Começaremos usando File-&gt;novo projeto no Visual Studio para criar o aplicativo NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="2c21f-145">We'll begin by using File-&gt;New Project within Visual Studio to create the NerdDinner application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="2c21f-146">Próximo</span><span class="sxs-lookup"><span data-stu-id="2c21f-146">Next</span></span>](create-a-new-aspnet-mvc-project.md)
