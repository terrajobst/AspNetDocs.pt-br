---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
title: Adicionando um modelo (C#) | Microsoft Docs
author: Rick-Anderson
description: 'Observação: uma versão atualizada deste tutorial está disponível aqui que usa o ASP.NET MVC 5 e o Visual Studio 2013. É mais seguro, muito mais simples de seguir e demonstrar...'
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 42355b95-5f1f-413e-8d16-14cdfaaefcd8
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: a5f494eaa05bcfcd9d49873db728d71c1fd332c8
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457773"
---
# <a name="adding-a-model-c"></a><span data-ttu-id="f8b0b-104">Adicionar um modelo (C#)</span><span class="sxs-lookup"><span data-stu-id="f8b0b-104">Adding a Model (C#)</span></span>

<span data-ttu-id="f8b0b-105">por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f8b0b-105">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> <span data-ttu-id="f8b0b-106">Este tutorial ensinará as noções básicas da criação de um aplicativo Web ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é uma versão gratuita do Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f8b0b-106">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="f8b0b-107">Antes de começar, verifique se você instalou os pré-requisitos listados abaixo.</span><span class="sxs-lookup"><span data-stu-id="f8b0b-107">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="f8b0b-108">Você pode instalar todos eles clicando no seguinte link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="f8b0b-108">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="f8b0b-109">Como alternativa, você pode instalar os pré-requisitos individualmente usando os seguintes links:</span><span class="sxs-lookup"><span data-stu-id="f8b0b-109">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="f8b0b-110">Pré-requisitos do Visual Studio Web Developer Express SP1</span><span class="sxs-lookup"><span data-stu-id="f8b0b-110">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="f8b0b-111">Atualização de ferramentas do ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="f8b0b-111">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="f8b0b-112">[SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(suporte + ferramentas de tempo de execução)</span><span class="sxs-lookup"><span data-stu-id="f8b0b-112">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="f8b0b-113">Se você estiver usando o Visual Studio 2010 em vez do Visual Web Developer 2010, instale os pré-requisitos clicando no seguinte link: [pré-requisitos do Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="f8b0b-113">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="f8b0b-114">Um projeto do Visual Web Developer com C# código-fonte está disponível para acompanhar este tópico.</span><span class="sxs-lookup"><span data-stu-id="f8b0b-114">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="f8b0b-115">[Baixe a C# versão](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="f8b0b-115">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="f8b0b-116">Se preferir Visual Basic, alterne para a [versão Visual Basic](../vb/adding-a-model.md) deste tutorial.</span><span class="sxs-lookup"><span data-stu-id="f8b0b-116">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/adding-a-model.md) of this tutorial.</span></span>

## <a name="adding-a-model"></a><span data-ttu-id="f8b0b-117">Adicionar um modelo</span><span class="sxs-lookup"><span data-stu-id="f8b0b-117">Adding a Model</span></span>

<span data-ttu-id="f8b0b-118">Nesta seção, você adicionará algumas classes para gerenciar filmes em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f8b0b-118">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="f8b0b-119">Essas classes serão a parte "modelo" do aplicativo MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f8b0b-119">These classes will be the "model" part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="f8b0b-120">Você usará uma .NET Framework tecnologia de acesso a dados conhecida como Entity Framework para definir e trabalhar com essas classes de modelo.</span><span class="sxs-lookup"><span data-stu-id="f8b0b-120">You'll use a .NET Framework data-access technology known as the Entity Framework to define and work with these model classes.</span></span> <span data-ttu-id="f8b0b-121">A Entity Framework (geralmente conhecida como EF) dá suporte a um paradigma de desenvolvimento chamado *Code First*.</span><span class="sxs-lookup"><span data-stu-id="f8b0b-121">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="f8b0b-122">Code First permite que você crie objetos de modelo escrevendo classes simples.</span><span class="sxs-lookup"><span data-stu-id="f8b0b-122">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="f8b0b-123">(Elas também são conhecidas como classes POCO, de "objetos CLR antigos".) Em seguida, você pode fazer com que o banco de dados seja criado dinamicamente de suas classes, o que permite um fluxo de trabalho de desenvolvimento muito limpo e rápido.</span><span class="sxs-lookup"><span data-stu-id="f8b0b-123">(These are also known as POCO classes, from "plain-old CLR objects.") You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="f8b0b-124">Adicionando classes de modelo</span><span class="sxs-lookup"><span data-stu-id="f8b0b-124">Adding Model Classes</span></span>

<span data-ttu-id="f8b0b-125">Em **Gerenciador de soluções**, clique com o botão direito do mouse na pasta *modelos* , selecione **Adicionar**e, em seguida, selecione **classe**.</span><span class="sxs-lookup"><span data-stu-id="f8b0b-125">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="f8b0b-126">Nomeie a *classe* "filme".</span><span class="sxs-lookup"><span data-stu-id="f8b0b-126">Name the *class* "Movie".</span></span>

<span data-ttu-id="f8b0b-127">[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="f8b0b-127">[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)</span></span>

<span data-ttu-id="f8b0b-128">Adicione as cinco propriedades a seguir à classe `Movie`:</span><span class="sxs-lookup"><span data-stu-id="f8b0b-128">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="f8b0b-129">Usaremos a classe `Movie` para representar filmes em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f8b0b-129">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="f8b0b-130">Cada instância de um objeto `Movie` corresponderá a uma linha em uma tabela de banco de dados, e cada propriedade da classe `Movie` será mapeada para uma coluna na tabela.</span><span class="sxs-lookup"><span data-stu-id="f8b0b-130">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="f8b0b-131">No mesmo arquivo, adicione a seguinte classe de `MovieDBContext`:</span><span class="sxs-lookup"><span data-stu-id="f8b0b-131">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="f8b0b-132">A classe `MovieDBContext` representa o contexto de banco de dados Entity Framework filme, que lida com a busca, o armazenamento e a atualização de `Movie` instâncias de classe em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f8b0b-132">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="f8b0b-133">O `MovieDBContext` deriva da classe base `DbContext` fornecida pelo Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f8b0b-133">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span> <span data-ttu-id="f8b0b-134">Para obter mais informações sobre `DbContext` e `DbSet`, consulte [melhorias de produtividade para o Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span><span class="sxs-lookup"><span data-stu-id="f8b0b-134">For more information about `DbContext` and `DbSet`, see [Productivity Improvements for the Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span></span>

<span data-ttu-id="f8b0b-135">Para poder referenciar `DbContext` e `DbSet`, você precisa adicionar a seguinte instrução `using` na parte superior do arquivo:</span><span class="sxs-lookup"><span data-stu-id="f8b0b-135">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="f8b0b-136">O arquivo *Movie.cs* completo é mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="f8b0b-136">The complete *Movie.cs* file is shown below.</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a><span data-ttu-id="f8b0b-137">Criando uma cadeia de conexão e trabalhando com SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="f8b0b-137">Creating a Connection String and Working with SQL Server Compact</span></span>

<span data-ttu-id="f8b0b-138">A classe `MovieDBContext` que você criou manipula a tarefa de conexão com o banco de dados e o mapeamento de objetos `Movie` para registros de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f8b0b-138">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="f8b0b-139">No entanto, uma pergunta que você pode fazer é como especificar a qual banco de dados ele será conectado.</span><span class="sxs-lookup"><span data-stu-id="f8b0b-139">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="f8b0b-140">Você fará isso adicionando informações de conexão no arquivo *Web. config* do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f8b0b-140">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="f8b0b-141">Abra o arquivo *Web. config da* raiz do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f8b0b-141">Open the application root *Web.config* file.</span></span> <span data-ttu-id="f8b0b-142">(Não o arquivo *Web. config* na pasta *views* .) A imagem abaixo mostra os arquivos *Web. config* ; Abra o arquivo *Web. config* circulado em vermelho.</span><span class="sxs-lookup"><span data-stu-id="f8b0b-142">(Not the *Web.config* file in the *Views* folder.) The image below show both *Web.config* files; open the *Web.config* file circled in red.</span></span>

![](adding-a-model/_static/image4.png)

<span data-ttu-id="f8b0b-143">Adicione a seguinte cadeia de conexão ao elemento `<connectionStrings>` no arquivo *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="f8b0b-143">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="f8b0b-144">O exemplo a seguir mostra uma parte do arquivo *Web. config* com a nova cadeia de conexão adicionada:</span><span class="sxs-lookup"><span data-stu-id="f8b0b-144">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

<span data-ttu-id="f8b0b-145">Essa pequena quantidade de código e XML é tudo o que você precisa para escrever a fim de representar e armazenar os dados do filme em um banco de dado.</span><span class="sxs-lookup"><span data-stu-id="f8b0b-145">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="f8b0b-146">Em seguida, você criará uma nova classe `MoviesController` que pode ser usada para exibir os dados do filme e permitir que os usuários criem novas listagens de filmes.</span><span class="sxs-lookup"><span data-stu-id="f8b0b-146">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f8b0b-147">[Anterior](adding-a-view.md)
> [Próximo](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="f8b0b-147">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
