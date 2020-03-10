---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: Adicionando um modelo | Microsoft Docs
author: Rick-Anderson
description: 'Observação: uma versão atualizada deste tutorial está disponível aqui que usa o ASP.NET MVC 5 e o Visual Studio 2013. É mais seguro, muito mais simples de seguir e demonstrar...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: a9851d93dde495814f67fa0c807d3534f5f0d8ef
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78540302"
---
# <a name="adding-a-model"></a><span data-ttu-id="bf658-104">Adicionar um modelo</span><span class="sxs-lookup"><span data-stu-id="bf658-104">Adding a Model</span></span>

<span data-ttu-id="bf658-105">por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bf658-105">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> > [!NOTE]
> > <span data-ttu-id="bf658-106">Uma versão atualizada deste tutorial está disponível [aqui](../../getting-started/introduction/getting-started.md) que usa o ASP.NET MVC 5 e o Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="bf658-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="bf658-107">É mais seguro, muito mais simples de seguir e demonstra mais recursos.</span><span class="sxs-lookup"><span data-stu-id="bf658-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>

<span data-ttu-id="bf658-108">Nesta seção, você adicionará algumas classes para gerenciar filmes em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="bf658-108">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="bf658-109">Essas classes serão o modelo de &quot;&quot; parte do aplicativo MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bf658-109">These classes will be the &quot;model&quot; part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="bf658-110">Você usará uma .NET Framework tecnologia de acesso a dados conhecida como [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) para definir e trabalhar com essas classes de modelo.</span><span class="sxs-lookup"><span data-stu-id="bf658-110">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) to define and work with these model classes.</span></span> <span data-ttu-id="bf658-111">A Entity Framework (geralmente conhecida como EF) dá suporte a um paradigma de desenvolvimento chamado *Code First*.</span><span class="sxs-lookup"><span data-stu-id="bf658-111">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="bf658-112">Code First permite que você crie objetos de modelo escrevendo classes simples.</span><span class="sxs-lookup"><span data-stu-id="bf658-112">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="bf658-113">(Elas também são conhecidas como classes POCO, de &quot;objetos CLR comuns.&quot;) Em seguida, você pode fazer com que o banco de dados seja criado dinamicamente de suas classes, o que permite um fluxo de trabalho de desenvolvimento muito limpo e rápido.</span><span class="sxs-lookup"><span data-stu-id="bf658-113">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="bf658-114">Adicionando classes de modelo</span><span class="sxs-lookup"><span data-stu-id="bf658-114">Adding Model Classes</span></span>

<span data-ttu-id="bf658-115">Em **Gerenciador de soluções**, clique com o botão direito do mouse na pasta *modelos* , selecione **Adicionar**e, em seguida, selecione **classe**.</span><span class="sxs-lookup"><span data-stu-id="bf658-115">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="bf658-116">Insira o nome da *classe* &quot;filme&quot;.</span><span class="sxs-lookup"><span data-stu-id="bf658-116">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="bf658-117">Adicione as cinco propriedades a seguir à classe `Movie`:</span><span class="sxs-lookup"><span data-stu-id="bf658-117">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="bf658-118">Usaremos a classe `Movie` para representar filmes em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="bf658-118">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="bf658-119">Cada instância de um objeto `Movie` corresponderá a uma linha em uma tabela de banco de dados, e cada propriedade da classe `Movie` será mapeada para uma coluna na tabela.</span><span class="sxs-lookup"><span data-stu-id="bf658-119">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="bf658-120">No mesmo arquivo, adicione a seguinte classe de `MovieDBContext`:</span><span class="sxs-lookup"><span data-stu-id="bf658-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="bf658-121">A classe `MovieDBContext` representa o contexto de banco de dados Entity Framework filme, que lida com a busca, o armazenamento e a atualização de `Movie` instâncias de classe em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="bf658-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="bf658-122">O `MovieDBContext` deriva da classe base `DbContext` fornecida pelo Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="bf658-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="bf658-123">Para poder referenciar `DbContext` e `DbSet`, você precisa adicionar a seguinte instrução `using` na parte superior do arquivo:</span><span class="sxs-lookup"><span data-stu-id="bf658-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="bf658-124">O arquivo *Movie.cs* completo é mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="bf658-124">The complete *Movie.cs* file is shown below.</span></span> <span data-ttu-id="bf658-125">(Várias instruções using que não são necessárias foram removidas.)</span><span class="sxs-lookup"><span data-stu-id="bf658-125">(Several using statements that are not needed have been removed.)</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="bf658-126">Criar uma cadeia de conexão e trabalhando com LocalDB do SQL Server</span><span class="sxs-lookup"><span data-stu-id="bf658-126">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="bf658-127">A classe `MovieDBContext` que você criou manipula a tarefa de conexão com o banco de dados e o mapeamento de objetos `Movie` para registros de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="bf658-127">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="bf658-128">No entanto, uma pergunta que você pode fazer é como especificar a qual banco de dados ele será conectado.</span><span class="sxs-lookup"><span data-stu-id="bf658-128">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="bf658-129">Você fará isso adicionando informações de conexão no arquivo *Web. config* do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="bf658-129">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="bf658-130">Abra o arquivo *Web. config da* raiz do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="bf658-130">Open the application root *Web.config* file.</span></span> <span data-ttu-id="bf658-131">(Não o arquivo *Web. config* na pasta *views* .) Abra o arquivo *Web. config* descrito em vermelho.</span><span class="sxs-lookup"><span data-stu-id="bf658-131">(Not the *Web.config* file in the *Views* folder.) Open the *Web.config* file outlined in red.</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="bf658-132">Adicione a seguinte cadeia de conexão ao elemento `<connectionStrings>` no arquivo *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="bf658-132">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="bf658-133">O exemplo a seguir mostra uma parte do arquivo *Web. config* com a nova cadeia de conexão adicionada:</span><span class="sxs-lookup"><span data-stu-id="bf658-133">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

<span data-ttu-id="bf658-134">Essa pequena quantidade de código e XML é tudo o que você precisa para escrever a fim de representar e armazenar os dados do filme em um banco de dado.</span><span class="sxs-lookup"><span data-stu-id="bf658-134">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="bf658-135">Em seguida, você criará uma nova classe `MoviesController` que pode ser usada para exibir os dados do filme e permitir que os usuários criem novas listagens de filmes.</span><span class="sxs-lookup"><span data-stu-id="bf658-135">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="bf658-136">[Anterior](adding-a-view.md)
> [Próximo](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="bf658-136">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
