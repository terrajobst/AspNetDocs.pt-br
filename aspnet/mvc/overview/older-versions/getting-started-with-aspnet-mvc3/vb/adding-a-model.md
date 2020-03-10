---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
title: Adicionando um modelo (VB) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensinará as noções básicas da criação de um aplicativo Web ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: b3aa7720-5c78-4ca2-baef-9a52234fb7ce
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: e69d59aed4d74f08f1c653c4965b128c4dbe20ff
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78540519"
---
# <a name="adding-a-model-vb"></a><span data-ttu-id="f6749-103">Adicionar um modelo (VB)</span><span class="sxs-lookup"><span data-stu-id="f6749-103">Adding a Model (VB)</span></span>

<span data-ttu-id="f6749-104">por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f6749-104">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> <span data-ttu-id="f6749-105">Este tutorial ensinará as noções básicas da criação de um aplicativo Web ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é uma versão gratuita do Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f6749-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="f6749-106">Antes de começar, verifique se você instalou os pré-requisitos listados abaixo.</span><span class="sxs-lookup"><span data-stu-id="f6749-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="f6749-107">Você pode instalar todos eles clicando no seguinte link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="f6749-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="f6749-108">Como alternativa, você pode instalar os pré-requisitos individualmente usando os seguintes links:</span><span class="sxs-lookup"><span data-stu-id="f6749-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="f6749-109">Pré-requisitos do Visual Studio Web Developer Express SP1</span><span class="sxs-lookup"><span data-stu-id="f6749-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="f6749-110">Atualização de ferramentas do ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="f6749-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="f6749-111">[SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(suporte + ferramentas de tempo de execução)</span><span class="sxs-lookup"><span data-stu-id="f6749-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="f6749-112">Se você estiver usando o Visual Studio 2010 em vez do Visual Web Developer 2010, instale os pré-requisitos clicando no seguinte link: [pré-requisitos do Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="f6749-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="f6749-113">Um projeto do Visual Web Developer com código-fonte VB.NET está disponível para acompanhar este tópico.</span><span class="sxs-lookup"><span data-stu-id="f6749-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="f6749-114">[Baixe a versão do VB.net](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="f6749-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="f6749-115">Se preferir C#, alterne para a [ C# versão](../cs/adding-a-model.md) deste tutorial.</span><span class="sxs-lookup"><span data-stu-id="f6749-115">If you prefer C#, switch to the [C# version](../cs/adding-a-model.md) of this tutorial.</span></span>

## <a name="adding-a-model"></a><span data-ttu-id="f6749-116">Adicionar um modelo</span><span class="sxs-lookup"><span data-stu-id="f6749-116">Adding a Model</span></span>

<span data-ttu-id="f6749-117">Nesta seção, você adicionará algumas classes para gerenciar filmes em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f6749-117">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="f6749-118">Essas classes serão a parte "modelo" do aplicativo MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f6749-118">These classes will be the "model" part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="f6749-119">Você usará uma .NET Framework tecnologia de acesso a dados conhecida como Entity Framework para definir e trabalhar com essas classes de modelo.</span><span class="sxs-lookup"><span data-stu-id="f6749-119">You'll use a .NET Framework data-access technology known as the Entity Framework to define and work with these model classes.</span></span> <span data-ttu-id="f6749-120">A Entity Framework (geralmente conhecida como EF) dá suporte a um paradigma de desenvolvimento chamado *Code First*.</span><span class="sxs-lookup"><span data-stu-id="f6749-120">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="f6749-121">Code First permite que você crie objetos de modelo escrevendo classes simples.</span><span class="sxs-lookup"><span data-stu-id="f6749-121">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="f6749-122">(Elas também são conhecidas como classes POCO, de "objetos CLR antigos".) Em seguida, você pode fazer com que o banco de dados seja criado dinamicamente de suas classes, o que permite um fluxo de trabalho de desenvolvimento muito limpo e rápido.</span><span class="sxs-lookup"><span data-stu-id="f6749-122">(These are also known as POCO classes, from "plain-old CLR objects.") You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="f6749-123">Adicionando classes de modelo</span><span class="sxs-lookup"><span data-stu-id="f6749-123">Adding Model Classes</span></span>

<span data-ttu-id="f6749-124">Em **Gerenciador de soluções**, clique com o botão direito do mouse na pasta *modelos* , selecione **Adicionar**e, em seguida, selecione **classe**.</span><span class="sxs-lookup"><span data-stu-id="f6749-124">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="f6749-125">Nomeie a classe "filme".</span><span class="sxs-lookup"><span data-stu-id="f6749-125">Name the class "Movie".</span></span>

<span data-ttu-id="f6749-126">Adicione as cinco propriedades a seguir à classe `Movie`:</span><span class="sxs-lookup"><span data-stu-id="f6749-126">Add the following five properties to the `Movie` class:</span></span>

[!code-vb[Main](adding-a-model/samples/sample1.vb)]

<span data-ttu-id="f6749-127">Usaremos a classe `Movie` para representar filmes em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f6749-127">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="f6749-128">Cada instância de um objeto `Movie` corresponderá a uma linha em uma tabela de banco de dados, e cada propriedade da classe `Movie` será mapeada para uma coluna na tabela.</span><span class="sxs-lookup"><span data-stu-id="f6749-128">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="f6749-129">No mesmo arquivo, adicione a seguinte classe de `MovieDBContext`:</span><span class="sxs-lookup"><span data-stu-id="f6749-129">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-vb[Main](adding-a-model/samples/sample2.vb)]

<span data-ttu-id="f6749-130">A classe `MovieDBContext` representa o contexto de banco de dados Entity Framework filme, que lida com a busca, o armazenamento e a atualização de `Movie` instâncias de classe em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f6749-130">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="f6749-131">O `MovieDBContext` deriva da classe base `DbContext` fornecida pelo Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f6749-131">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span> <span data-ttu-id="f6749-132">Para obter mais informações sobre `DbContext` e `DbSet`, consulte [melhorias de produtividade para o Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span><span class="sxs-lookup"><span data-stu-id="f6749-132">For more information about `DbContext` and `DbSet`, see [Productivity Improvements for the Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span></span>

<span data-ttu-id="f6749-133">Para poder referenciar `DbContext` e `DbSet`, você precisa adicionar a seguinte instrução `imports` na parte superior do arquivo:</span><span class="sxs-lookup"><span data-stu-id="f6749-133">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `imports` statement at the top of the file:</span></span>

[!code-vb[Main](adding-a-model/samples/sample3.vb)]

<span data-ttu-id="f6749-134">O arquivo *Movie. vb* completo é mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="f6749-134">The complete *Movie.vb* file is shown below.</span></span>

[!code-vb[Main](adding-a-model/samples/sample4.vb)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a><span data-ttu-id="f6749-135">Criando uma cadeia de conexão e trabalhando com SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="f6749-135">Creating a Connection String and Working with SQL Server Compact</span></span>

<span data-ttu-id="f6749-136">A classe `MovieDBContext` que você criou manipula a tarefa de conexão com o banco de dados e o mapeamento de objetos `Movie` para registros de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f6749-136">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="f6749-137">No entanto, uma pergunta que você pode fazer é como especificar a qual banco de dados ele será conectado.</span><span class="sxs-lookup"><span data-stu-id="f6749-137">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="f6749-138">Você fará isso adicionando informações de conexão no arquivo *Web. config* do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f6749-138">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="f6749-139">Abra o arquivo *Web. config da* raiz do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f6749-139">Open the application root *Web.config* file.</span></span> <span data-ttu-id="f6749-140">(Não o arquivo *Web. config* na pasta *views* .) A imagem abaixo mostra os arquivos *Web. config* ; Abra o arquivo *Web. config* circulado em vermelho.</span><span class="sxs-lookup"><span data-stu-id="f6749-140">(Not the *Web.config* file in the *Views* folder.) The image below show both *Web.config* files; open the *Web.config* file circled in red.</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="f6749-141">Adicione a seguinte cadeia de conexão ao elemento `<connectionStrings>` no arquivo *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="f6749-141">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="f6749-142">O exemplo a seguir mostra uma parte do arquivo *Web. config* com a nova cadeia de conexão adicionada:</span><span class="sxs-lookup"><span data-stu-id="f6749-142">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

<span data-ttu-id="f6749-143">Essa pequena quantidade de código e XML é tudo o que você precisa para escrever a fim de representar e armazenar os dados do filme em um banco de dado.</span><span class="sxs-lookup"><span data-stu-id="f6749-143">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="f6749-144">Em seguida, você criará uma nova classe `MoviesController` que pode ser usada para exibir os dados do filme e permitir que os usuários criem novas listagens de filmes.</span><span class="sxs-lookup"><span data-stu-id="f6749-144">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f6749-145">[Anterior](adding-a-view.md)
> [Próximo](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="f6749-145">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
