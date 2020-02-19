---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: Criando uma cadeia de conexão e trabalhando com SQL Server LocalDB | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: 20781ad760d3a0e4559ec4c7e18528f3686dcc02
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456511"
---
# <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="804d9-102">Criar uma cadeia de conexão e trabalhando com LocalDB do SQL Server</span><span class="sxs-lookup"><span data-stu-id="804d9-102">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="804d9-103">por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="804d9-103">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [Tutorial Note](index.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="804d9-104">Criar uma cadeia de conexão e trabalhando com LocalDB do SQL Server</span><span class="sxs-lookup"><span data-stu-id="804d9-104">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="804d9-105">A classe `MovieDBContext` que você criou manipula a tarefa de conexão com o banco de dados e o mapeamento de objetos `Movie` para registros de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="804d9-105">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="804d9-106">No entanto, uma pergunta que você pode fazer é como especificar a qual banco de dados ele será conectado.</span><span class="sxs-lookup"><span data-stu-id="804d9-106">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="804d9-107">Na verdade, você não precisa especificar qual banco de dados usar, Entity Framework usará o [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb)como padrão.</span><span class="sxs-lookup"><span data-stu-id="804d9-107">You don't actually have to specify which database to use, Entity Framework will default to using [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="804d9-108">Nesta seção, adicionaremos explicitamente uma cadeia de conexão no arquivo *Web. config* do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="804d9-108">In this section we'll explicitly add a connection string in the *Web.config* file of the application.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="804d9-109">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="804d9-109">SQL Server Express LocalDB</span></span>

<span data-ttu-id="804d9-110">O [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) é uma versão leve do Mecanismo de Banco de Dados de SQL Server Express que inicia sob demanda e é executado no modo de usuário.</span><span class="sxs-lookup"><span data-stu-id="804d9-110">[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) is a lightweight version of the SQL Server Express Database Engine that starts on demand and runs in user mode.</span></span> <span data-ttu-id="804d9-111">O LocalDB é executado em um modo de execução especial de SQL Server Express que permite que você trabalhe com bancos de dados como arquivos *. MDF* .</span><span class="sxs-lookup"><span data-stu-id="804d9-111">LocalDB runs in a special execution mode of SQL Server Express that enables you to work with databases as *.mdf* files.</span></span> <span data-ttu-id="804d9-112">Normalmente, os arquivos de banco de dados LocalDB são mantidos na pasta *Data\_do aplicativo* de um projeto Web.</span><span class="sxs-lookup"><span data-stu-id="804d9-112">Typically, LocalDB database files are kept in the *App\_Data* folder of a web project.</span></span>

<span data-ttu-id="804d9-113">O SQL Server Express não é recomendado para uso em aplicativos Web de produção.</span><span class="sxs-lookup"><span data-stu-id="804d9-113">SQL Server Express is not recommended for use in production web applications.</span></span> <span data-ttu-id="804d9-114">O LocalDB, em particular, não deve ser usado para produção com um aplicativo Web porque ele não foi projetado para funcionar com o IIS.</span><span class="sxs-lookup"><span data-stu-id="804d9-114">LocalDB in particular should not be used for production with a web application because it is not designed to work with IIS.</span></span> <span data-ttu-id="804d9-115">No entanto, um banco de dados LocalDB pode ser facilmente migrado para SQL Server ou SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="804d9-115">However, a LocalDB database can be easily migrated to SQL Server or SQL Azure.</span></span>

<span data-ttu-id="804d9-116">No Visual Studio 2017, o LocalDB é instalado por padrão com o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="804d9-116">In Visual Studio 2017, LocalDB is installed by default with Visual Studio.</span></span>

<span data-ttu-id="804d9-117">Por padrão, a Entity Framework procura uma cadeia de conexão denominada igual à classe de contexto de objeto (`MovieDBContext` para este projeto).</span><span class="sxs-lookup"><span data-stu-id="804d9-117">By default, the Entity Framework looks for a connection string named the same as the object context class (`MovieDBContext` for this project).</span></span> <span data-ttu-id="804d9-118">Para obter mais informações, consulte [SQL Server cadeias de conexão para aplicativos Web ASP.net](https://msdn.microsoft.com/library/jj653752.aspx).</span><span class="sxs-lookup"><span data-stu-id="804d9-118">For more information see [SQL Server Connection Strings for ASP.NET Web Applications](https://msdn.microsoft.com/library/jj653752.aspx).</span></span>

<span data-ttu-id="804d9-119">Abra o arquivo *Web. config da* raiz do aplicativo mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="804d9-119">Open the application root *Web.config* file shown below.</span></span> <span data-ttu-id="804d9-120">(Não o arquivo *Web. config* na pasta *views* .)</span><span class="sxs-lookup"><span data-stu-id="804d9-120">(Not the *Web.config* file in the *Views* folder.)</span></span>

![](creating-a-connection-string/_static/image1.png)

<span data-ttu-id="804d9-121">Localize o elemento `<connectionStrings>`:</span><span class="sxs-lookup"><span data-stu-id="804d9-121">Find the `<connectionStrings>` element:</span></span>

![](creating-a-connection-string/_static/image2.png)

<span data-ttu-id="804d9-122">Adicione a seguinte cadeia de conexão ao elemento `<connectionStrings>` no arquivo *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="804d9-122">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

<span data-ttu-id="804d9-123">O exemplo a seguir mostra uma parte do arquivo *Web. config* com a nova cadeia de conexão adicionada:</span><span class="sxs-lookup"><span data-stu-id="804d9-123">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

<span data-ttu-id="804d9-124">As duas cadeias de conexão são muito semelhantes.</span><span class="sxs-lookup"><span data-stu-id="804d9-124">The two connection strings are very similar.</span></span> <span data-ttu-id="804d9-125">A primeira cadeia de conexão é denominada `DefaultConnection` e é usada para que o banco de dados de associação controle quem pode acessar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="804d9-125">The first connection string is named `DefaultConnection` and is used for the membership database to control who can access the application.</span></span> <span data-ttu-id="804d9-126">A cadeia de conexão adicionada especifica um banco de dados LocalDB chamado *Movie. MDF* localizado na pasta de *dados do aplicativo\_* .</span><span class="sxs-lookup"><span data-stu-id="804d9-126">The connection string you've added specifies a LocalDB database named *Movie.mdf* located in the *App\_Data* folder.</span></span> <span data-ttu-id="804d9-127">Não usaremos o banco de dados Membership neste tutorial, para obter mais informações sobre associação, autenticação e segurança, consulte meu tutorial [criar um aplicativo MVC ASP.NET com autenticação e banco de dados SQL e implantar no serviço Azure app](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="804d9-127">We won't use the membership database in this tutorial, for more information on membership, authentication and security, see my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span>

<span data-ttu-id="804d9-128">O nome da cadeia de conexão deve corresponder ao nome da classe [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) .</span><span class="sxs-lookup"><span data-stu-id="804d9-128">The name of the connection string must match the name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class.</span></span>

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

<span data-ttu-id="804d9-129">Na verdade, você não precisa adicionar a cadeia de conexão `MovieDBContext`.</span><span class="sxs-lookup"><span data-stu-id="804d9-129">You don't actually need to add the `MovieDBContext` connection string.</span></span> <span data-ttu-id="804d9-130">Se você não especificar uma cadeia de conexão, Entity Framework criará um banco de dados LocalDB no diretório Users com o nome totalmente qualificado da classe [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) (nesse caso, `MvcMovie.Models.MovieDBContext`).</span><span class="sxs-lookup"><span data-stu-id="804d9-130">If you don't specify a connection string, Entity Framework will create a LocalDB database in the users directory with the fully qualified name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class (in this case `MvcMovie.Models.MovieDBContext`).</span></span> <span data-ttu-id="804d9-131">Você pode nomear o banco de dados como desejar, desde que ele tenha o *.* Sufixo de MDF.</span><span class="sxs-lookup"><span data-stu-id="804d9-131">You can name the database anything you like, as long as it has the *.MDF* suffix.</span></span> <span data-ttu-id="804d9-132">Por exemplo, poderíamos nomear o banco de dados *Myfilmes. MDF*.</span><span class="sxs-lookup"><span data-stu-id="804d9-132">For example, we could name the database *MyFilms.mdf*.</span></span>

<span data-ttu-id="804d9-133">Em seguida, você criará uma nova classe `MoviesController` que pode ser usada para exibir os dados do filme e permitir que os usuários criem novas listagens de filmes.</span><span class="sxs-lookup"><span data-stu-id="804d9-133">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="804d9-134">[Anterior](adding-a-model.md)
> [Próximo](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="804d9-134">[Previous](adding-a-model.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
