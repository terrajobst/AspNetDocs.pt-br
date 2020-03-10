---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Tutorial: usar os procedimentos assíncronos e armazenados com o EF em um aplicativo MVC ASP.NET'
description: Neste tutorial, você verá como implementar o modelo de programação assíncrona e aprender a usar procedimentos armazenados.
author: tdykstra
ms.author: riande
ms.date: 01/18/2019
ms.topic: tutorial
ms.assetid: 27d110fc-d1b7-4628-a763-26f1e6087549
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 5612f2f25d06feb904a205505ed8f048d2263266
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583436"
---
# <a name="tutorial-use-async-and-stored-procedures-with-ef-in-an-aspnet-mvc-app"></a><span data-ttu-id="10d4f-103">Tutorial: usar os procedimentos assíncronos e armazenados com o EF em um aplicativo MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="10d4f-103">Tutorial: Use async and stored procedures with EF in an ASP.NET MVC App</span></span>

<span data-ttu-id="10d4f-104">Nos tutoriais anteriores, você aprendeu a ler e atualizar dados usando o modelo de programação síncrona.</span><span class="sxs-lookup"><span data-stu-id="10d4f-104">In earlier tutorials you learned how to read and update data using the synchronous programming model.</span></span> <span data-ttu-id="10d4f-105">Neste tutorial, você verá como implementar o modelo de programação assíncrona.</span><span class="sxs-lookup"><span data-stu-id="10d4f-105">In this tutorial you see how to implement the asynchronous programming model.</span></span> <span data-ttu-id="10d4f-106">O código assíncrono pode ajudar um aplicativo a funcionar melhor porque faz uso melhor dos recursos do servidor.</span><span class="sxs-lookup"><span data-stu-id="10d4f-106">Asynchronous code can help an application perform better because it makes better use of server resources.</span></span>

<span data-ttu-id="10d4f-107">Neste tutorial, você também verá como usar procedimentos armazenados para operações de inserção, atualização e exclusão em uma entidade.</span><span class="sxs-lookup"><span data-stu-id="10d4f-107">In this tutorial you also see how to use stored procedures for insert, update, and delete operations on an entity.</span></span>

<span data-ttu-id="10d4f-108">Por fim, reimplante o aplicativo no Azure, juntamente com todas as alterações de banco de dados que você implementou desde a primeira vez que você implantou.</span><span class="sxs-lookup"><span data-stu-id="10d4f-108">Finally, you redeploy the application to Azure, along with all of the database changes that you've implemented since the first time you deployed.</span></span>

<span data-ttu-id="10d4f-109">As ilustrações a seguir mostram algumas das páginas com as quais você trabalhará.</span><span class="sxs-lookup"><span data-stu-id="10d4f-109">The following illustrations show some of the pages that you'll work with.</span></span>

![Página departamentos](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Criar departamento](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

<span data-ttu-id="10d4f-112">Neste tutorial, você:</span><span class="sxs-lookup"><span data-stu-id="10d4f-112">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="10d4f-113">Saiba mais sobre o código assíncrono</span><span class="sxs-lookup"><span data-stu-id="10d4f-113">Learn about asynchronous code</span></span>
> * <span data-ttu-id="10d4f-114">Criar um controlador de departamento</span><span class="sxs-lookup"><span data-stu-id="10d4f-114">Create a Department controller</span></span>
> * <span data-ttu-id="10d4f-115">Usar procedimentos armazenados</span><span class="sxs-lookup"><span data-stu-id="10d4f-115">Use stored procedures</span></span>
> * <span data-ttu-id="10d4f-116">Implantar no Azure</span><span class="sxs-lookup"><span data-stu-id="10d4f-116">Deploy to Azure</span></span>

## <a name="prerequisites"></a><span data-ttu-id="10d4f-117">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="10d4f-117">Prerequisites</span></span>

* [<span data-ttu-id="10d4f-118">Atualização de dados relacionados</span><span class="sxs-lookup"><span data-stu-id="10d4f-118">Updating Related Data</span></span>](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="why-use-asynchronous-code"></a><span data-ttu-id="10d4f-119">Por que usar código assíncrono</span><span class="sxs-lookup"><span data-stu-id="10d4f-119">Why use asynchronous code</span></span>

<span data-ttu-id="10d4f-120">Um servidor Web tem um número limitado de threads disponíveis e, em situações de alta carga, todos os threads disponíveis podem estar em uso.</span><span class="sxs-lookup"><span data-stu-id="10d4f-120">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="10d4f-121">Quando isso acontece, o servidor não pode processar novas solicitações até que os threads são liberados.</span><span class="sxs-lookup"><span data-stu-id="10d4f-121">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="10d4f-122">Com um código síncrono, muitos threads podem ser vinculados enquanto realmente não são fazendo nenhum trabalho porque estão aguardando a conclusão da E/S.</span><span class="sxs-lookup"><span data-stu-id="10d4f-122">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="10d4f-123">Com um código assíncrono, quando um processo está aguardando a conclusão da E/S, seu thread é liberado para o servidor para ser usado para processar outras solicitações.</span><span class="sxs-lookup"><span data-stu-id="10d4f-123">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="10d4f-124">Como resultado, o código assíncrono permite que os recursos do servidor sejam usados com mais eficiência e o servidor está habilitado para lidar com mais tráfego sem atrasos.</span><span class="sxs-lookup"><span data-stu-id="10d4f-124">As a result, asynchronous code enables server resources to be use more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="10d4f-125">Em versões anteriores do .NET, escrever e testar código assíncrono era complexo, propenso a erros e difícil de depurar.</span><span class="sxs-lookup"><span data-stu-id="10d4f-125">In earlier versions of .NET, writing and testing asynchronous code was complex, error prone, and hard to debug.</span></span> <span data-ttu-id="10d4f-126">No .NET 4,5, escrever, testar e depurar código assíncrono é muito mais fácil que você geralmente deve escrever código assíncrono, a menos que tenha um motivo para não.</span><span class="sxs-lookup"><span data-stu-id="10d4f-126">In .NET 4.5, writing, testing, and debugging asynchronous code is so much easier that you should generally write asynchronous code unless you have a reason not to.</span></span> <span data-ttu-id="10d4f-127">O código assíncrono apresenta uma pequena quantidade de sobrecarga, mas para situações de tráfego baixo, o impacto no desempenho é insignificante, enquanto para situações de tráfego alto, a possível melhoria no desempenho é substancial.</span><span class="sxs-lookup"><span data-stu-id="10d4f-127">Asynchronous code does introduce a small amount of overhead, but for low traffic situations the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="10d4f-128">Para obter mais informações sobre programação assíncrona, consulte [usar o suporte assíncrono do .NET 4.5 para evitar chamadas de bloqueio](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).</span><span class="sxs-lookup"><span data-stu-id="10d4f-128">For more information about asynchronous programming, see [Use .NET 4.5's async support to avoid blocking calls](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).</span></span>

## <a name="create-department-controller"></a><span data-ttu-id="10d4f-129">Criar controlador de departamento</span><span class="sxs-lookup"><span data-stu-id="10d4f-129">Create Department controller</span></span>

<span data-ttu-id="10d4f-130">Crie um controlador de departamento da mesma maneira que fez com os controladores anteriores, exceto desta vez marque a caixa de seleção **usar ações do controlador assíncrono** .</span><span class="sxs-lookup"><span data-stu-id="10d4f-130">Create a Department controller the same way you did the earlier controllers, except this time select the **Use async controller actions** check box.</span></span>

<span data-ttu-id="10d4f-131">Os destaques a seguir mostram o que foi adicionado ao código síncrono para o método `Index` para torná-lo assíncrono:</span><span class="sxs-lookup"><span data-stu-id="10d4f-131">The following highlights show what was added to the synchronous code for the `Index` method to make it asynchronous:</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=1,4)]

<span data-ttu-id="10d4f-132">Quatro alterações foram aplicadas para permitir que o Entity Framework consulta de banco de dados seja executado de forma assíncrona:</span><span class="sxs-lookup"><span data-stu-id="10d4f-132">Four changes were applied to enable the Entity Framework database query to execute asynchronously:</span></span>

- <span data-ttu-id="10d4f-133">O método é marcado com a palavra-chave `async`, que informa ao compilador para gerar retornos de chamada para partes do corpo do método e criar automaticamente o objeto de `Task<ActionResult>` retornado.</span><span class="sxs-lookup"><span data-stu-id="10d4f-133">The method is marked with the `async` keyword, which tells the compiler to generate callbacks for parts of the method body and to automatically create the `Task<ActionResult>` object that is returned.</span></span>
- <span data-ttu-id="10d4f-134">O tipo de retorno foi alterado de `ActionResult` para `Task<ActionResult>`.</span><span class="sxs-lookup"><span data-stu-id="10d4f-134">The return type was changed from `ActionResult` to `Task<ActionResult>`.</span></span> <span data-ttu-id="10d4f-135">O tipo de `Task<T>` representa o trabalho em andamento com um resultado do tipo `T`.</span><span class="sxs-lookup"><span data-stu-id="10d4f-135">The `Task<T>` type represents ongoing work with a result of type `T`.</span></span>
- <span data-ttu-id="10d4f-136">A palavra-chave `await` foi aplicada à chamada de serviço Web.</span><span class="sxs-lookup"><span data-stu-id="10d4f-136">The `await` keyword was applied to the web service call.</span></span> <span data-ttu-id="10d4f-137">Quando o compilador vê essa palavra-chave, por trás das cenas, ela divide o método em duas partes.</span><span class="sxs-lookup"><span data-stu-id="10d4f-137">When the compiler sees this keyword, behind the scenes it splits the method into two parts.</span></span> <span data-ttu-id="10d4f-138">A primeira parte termina com a operação que é iniciada de forma assíncrona.</span><span class="sxs-lookup"><span data-stu-id="10d4f-138">The first part ends with the operation that is started asynchronously.</span></span> <span data-ttu-id="10d4f-139">A segunda parte é colocada em um método de retorno de chamada que é chamado quando a operação é concluída.</span><span class="sxs-lookup"><span data-stu-id="10d4f-139">The second part is put into a callback method that is called when the operation completes.</span></span>
- <span data-ttu-id="10d4f-140">A versão assíncrona do método de extensão de `ToList` foi chamada.</span><span class="sxs-lookup"><span data-stu-id="10d4f-140">The asynchronous version of the `ToList` extension method was called.</span></span>

<span data-ttu-id="10d4f-141">Por que a instrução `departments.ToList` modificada, mas não a instrução `departments = db.Departments`?</span><span class="sxs-lookup"><span data-stu-id="10d4f-141">Why is the `departments.ToList` statement modified but not the `departments = db.Departments` statement?</span></span> <span data-ttu-id="10d4f-142">O motivo é que apenas as instruções que fazem com que consultas ou comandos sejam enviadas ao banco de dados são executadas de forma assíncrona.</span><span class="sxs-lookup"><span data-stu-id="10d4f-142">The reason is that only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="10d4f-143">A instrução `departments = db.Departments` configura uma consulta, mas a consulta não é executada até que o método `ToList` seja chamado.</span><span class="sxs-lookup"><span data-stu-id="10d4f-143">The `departments = db.Departments` statement sets up a query but the query is not executed until the `ToList` method is called.</span></span> <span data-ttu-id="10d4f-144">Portanto, somente o método `ToList` é executado de forma assíncrona.</span><span class="sxs-lookup"><span data-stu-id="10d4f-144">Therefore, only the `ToList` method is executed asynchronously.</span></span>

<span data-ttu-id="10d4f-145">No método `Details` e os métodos `HttpGet` `Edit` e `Delete`, o método `Find` é aquele que faz com que uma consulta seja enviada ao banco de dados, portanto, esse é o método que é executado de forma assíncrona:</span><span class="sxs-lookup"><span data-stu-id="10d4f-145">In the `Details` method and the `HttpGet` `Edit` and `Delete` methods, the `Find` method is the one that causes a query to be sent to the database, so that's the method that gets executed asynchronously:</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs?highlight=7)]

<span data-ttu-id="10d4f-146">Nos métodos `Create`, `HttpPost Edit`e `DeleteConfirmed`, é a chamada do método `SaveChanges` que faz com que um comando seja executado, não instruções como `db.Departments.Add(department)` que fazem com que apenas entidades na memória sejam modificadas.</span><span class="sxs-lookup"><span data-stu-id="10d4f-146">In the `Create`, `HttpPost Edit`, and `DeleteConfirmed` methods, it is the `SaveChanges` method call that causes a command to be executed, not statements such as `db.Departments.Add(department)` which only cause entities in memory to be modified.</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=6)]

<span data-ttu-id="10d4f-147">Abra *Views\Department\Index.cshtml*e substitua o código do modelo pelo código a seguir:</span><span class="sxs-lookup"><span data-stu-id="10d4f-147">Open *Views\Department\Index.cshtml*, and replace the template code with the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=3,5,20-22,36-38)]

<span data-ttu-id="10d4f-148">Esse código altera o título de índice para departamentos, move o nome do administrador para a direita e fornece o nome completo do administrador.</span><span class="sxs-lookup"><span data-stu-id="10d4f-148">This code changes the title from Index to Departments, moves the Administrator name to the right, and provides the full name of the administrator.</span></span>

<span data-ttu-id="10d4f-149">Nas exibições criar, excluir, detalhar e editar, altere a legenda do campo `InstructorID` para "administrador" da mesma maneira que você alterou o campo nome do departamento para "departamento" nos modos de exibição do curso.</span><span class="sxs-lookup"><span data-stu-id="10d4f-149">In the Create, Delete, Details, and Edit views, change the caption for the `InstructorID` field to "Administrator" the same way you changed the department name field to "Department" in the Course views.</span></span>

<span data-ttu-id="10d4f-150">Nas exibições criar e editar, use o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="10d4f-150">In the Create and Edit views use the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml)]

<span data-ttu-id="10d4f-151">Nas exibições excluir e detalhes, use o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="10d4f-151">In the Delete and Details views use the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

<span data-ttu-id="10d4f-152">Execute o aplicativo e clique na guia **departamentos** .</span><span class="sxs-lookup"><span data-stu-id="10d4f-152">Run the application, and click the **Departments** tab.</span></span>

<span data-ttu-id="10d4f-153">Tudo funciona da mesma forma que nos outros controladores, mas nesse controlador todas as consultas SQL são executadas de forma assíncrona.</span><span class="sxs-lookup"><span data-stu-id="10d4f-153">Everything works the same as in the other controllers, but in this controller all of the SQL queries are executing asynchronously.</span></span>

<span data-ttu-id="10d4f-154">Algumas coisas a serem consideradas quando você estiver usando a programação assíncrona com o Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="10d4f-154">Some things to be aware of when you are using asynchronous programming with the Entity Framework:</span></span>

- <span data-ttu-id="10d4f-155">O código assíncrono não é thread-safe.</span><span class="sxs-lookup"><span data-stu-id="10d4f-155">The async code is not thread safe.</span></span> <span data-ttu-id="10d4f-156">Em outras palavras, em outras palavras, não tente realizar várias operações em paralelo usando a mesma instância de contexto.</span><span class="sxs-lookup"><span data-stu-id="10d4f-156">In other words, in other words, don't try to do multiple operations in parallel using the same context instance.</span></span>
- <span data-ttu-id="10d4f-157">Se desejar aproveitar os benefícios de desempenho do código assíncrono, verifique se os pacotes de biblioteca que você está usando (por exemplo, para paginação) também usam o código assíncrono se eles chamam métodos do Entity Framework que fazem com que consultas sejam enviadas ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="10d4f-157">If you want to take advantage of the performance benefits of async code, make sure that any library packages that you're using (such as for paging), also use async if they call any Entity Framework methods that cause queries to be sent to the database.</span></span>

## <a name="use-stored-procedures"></a><span data-ttu-id="10d4f-158">Usar procedimentos armazenados</span><span class="sxs-lookup"><span data-stu-id="10d4f-158">Use stored procedures</span></span>

<span data-ttu-id="10d4f-159">Alguns desenvolvedores e DBAs preferem usar procedimentos armazenados para acesso ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="10d4f-159">Some developers and DBAs prefer to use stored procedures for database access.</span></span> <span data-ttu-id="10d4f-160">Em versões anteriores do Entity Framework você pode recuperar dados usando um procedimento armazenado [executando uma consulta SQL bruta](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), mas não pode instruir o EF a usar procedimentos armazenados para operações de atualização.</span><span class="sxs-lookup"><span data-stu-id="10d4f-160">In earlier versions of Entity Framework you can retrieve data using a stored procedure by [executing a raw SQL query](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), but you can't instruct EF to use stored procedures for update operations.</span></span> <span data-ttu-id="10d4f-161">No EF 6, é fácil configurar Code First usar procedimentos armazenados.</span><span class="sxs-lookup"><span data-stu-id="10d4f-161">In EF 6 it's easy to configure Code First to use stored procedures.</span></span>

1. <span data-ttu-id="10d4f-162">No *DAL\SchoolContext.cs*, adicione o código realçado ao método `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="10d4f-162">In *DAL\SchoolContext.cs*, add the highlighted code to the `OnModelCreating` method.</span></span>

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=9)]

    <span data-ttu-id="10d4f-163">Esse código instrui Entity Framework a usar procedimentos armazenados para operações de inserção, atualização e exclusão na entidade `Department`.</span><span class="sxs-lookup"><span data-stu-id="10d4f-163">This code instructs Entity Framework to use stored procedures for insert, update, and delete operations on the `Department` entity.</span></span>
2. <span data-ttu-id="10d4f-164">No console gerenciar do pacote, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="10d4f-164">In Package Manage Console, enter the following command:</span></span>

    `add-migration DepartmentSP`

    <span data-ttu-id="10d4f-165">As *migrações abertas\\carimbo de data/hora &lt;&gt;\_DepartmentSP.cs* para ver o código no método `Up` que cria procedimentos armazenados INSERT, Update e Delete:</span><span class="sxs-lookup"><span data-stu-id="10d4f-165">Open *Migrations\\&lt;timestamp&gt;\_DepartmentSP.cs* to see the code in the `Up` method that creates Insert, Update, and Delete stored procedures:</span></span>

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs?highlight=3-4,26-27,42-43)]
3. <span data-ttu-id="10d4f-166">No console gerenciar do pacote, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="10d4f-166">In Package Manage Console, enter the following command:</span></span>

     `update-database`
4. <span data-ttu-id="10d4f-167">Execute o aplicativo no modo de depuração, clique na guia **departamentos** e, em seguida, clique em **criar novo**.</span><span class="sxs-lookup"><span data-stu-id="10d4f-167">Run the application in debug mode, click the **Departments** tab, and then click **Create New**.</span></span>
5. <span data-ttu-id="10d4f-168">Insira dados para um novo departamento e, em seguida, clique em **criar**.</span><span class="sxs-lookup"><span data-stu-id="10d4f-168">Enter data for a new department, and then click **Create**.</span></span>

6. <span data-ttu-id="10d4f-169">No Visual Studio, examine os logs na janela **saída** para ver que um procedimento armazenado foi usado para inserir a nova linha de departamento.</span><span class="sxs-lookup"><span data-stu-id="10d4f-169">In Visual Studio, look at the logs in the **Output** window to see that a stored procedure was used to insert the new Department row.</span></span>

     ![SP de inserção do departamento](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="10d4f-171">Code First cria nomes de procedimento armazenado padrão.</span><span class="sxs-lookup"><span data-stu-id="10d4f-171">Code First creates default stored procedure names.</span></span> <span data-ttu-id="10d4f-172">Se você estiver usando um banco de dados existente, talvez seja necessário personalizar os nomes dos procedimentos armazenados para usar os procedimentos armazenados já definidos no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="10d4f-172">If you are using an existing database, you might need to customize the stored procedure names in order to use stored procedures already defined in the database.</span></span> <span data-ttu-id="10d4f-173">Para obter informações sobre como fazer isso, consulte [Entity Framework Code First inserir/atualizar/excluir procedimentos armazenados](https://msdn.microsoft.com/data/dn468673).</span><span class="sxs-lookup"><span data-stu-id="10d4f-173">For information about how to do that, see [Entity Framework Code First Insert/Update/Delete Stored Procedures](https://msdn.microsoft.com/data/dn468673).</span></span>

<span data-ttu-id="10d4f-174">Se desejar personalizar o que os procedimentos armazenados gerados fazem, você pode editar o código com Scaffold para as migrações `Up` método que cria o procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="10d4f-174">If you want to customize what generated stored procedures do, you can edit the scaffolded code for the migrations `Up` method that creates the stored procedure.</span></span> <span data-ttu-id="10d4f-175">Dessa forma, as alterações serão refletidas sempre que a migração for executada e serão aplicadas ao seu banco de dados de produção quando as migrações forem executadas automaticamente na produção após a implantação.</span><span class="sxs-lookup"><span data-stu-id="10d4f-175">That way your changes are reflected whenever that migration is run and will be applied to your production database when migrations runs automatically in production after deployment.</span></span>

<span data-ttu-id="10d4f-176">Se você quiser alterar um procedimento armazenado existente que foi criado em uma migração anterior, poderá usar o comando Add-Migration para gerar uma migração em branco e, em seguida, escrever manualmente o código que chama o método [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) .</span><span class="sxs-lookup"><span data-stu-id="10d4f-176">If you want to change an existing stored procedure that was created in a previous migration, you can use the Add-Migration command to generate a blank migration, and then manually write code that calls the [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) method.</span></span>

## <a name="deploy-to-azure"></a><span data-ttu-id="10d4f-177">Implantar no Azure</span><span class="sxs-lookup"><span data-stu-id="10d4f-177">Deploy to Azure</span></span>

<span data-ttu-id="10d4f-178">Esta seção exige que você tenha concluído a seção **implantação opcional do aplicativo no Azure** no tutorial de [migrações e implantação](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) desta série.</span><span class="sxs-lookup"><span data-stu-id="10d4f-178">This section requires you to have completed the optional **Deploying the app to Azure** section in the [Migrations and Deployment](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial of this series.</span></span> <span data-ttu-id="10d4f-179">Se houver erros de migrações que você resolveu excluindo o banco de dados em seu projeto local, pule esta seção.</span><span class="sxs-lookup"><span data-stu-id="10d4f-179">If you had migrations errors that you resolved by deleting the database in your local project, skip this section.</span></span>

1. <span data-ttu-id="10d4f-180">No Visual Studio, clique com o botão direito do mouse no projeto no **Gerenciador de Soluções** e selecione **Publicar** no menu de contexto.</span><span class="sxs-lookup"><span data-stu-id="10d4f-180">In Visual Studio, right-click the project in **Solution Explorer** and select **Publish** from the context menu.</span></span>
2. <span data-ttu-id="10d4f-181">Clique em **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="10d4f-181">Click **Publish**.</span></span>

    <span data-ttu-id="10d4f-182">O Visual Studio implanta o aplicativo no Azure e o aplicativo é aberto no navegador padrão, em execução no Azure.</span><span class="sxs-lookup"><span data-stu-id="10d4f-182">Visual Studio deploys the application to Azure, and the application opens in your default browser, running in Azure.</span></span>
3. <span data-ttu-id="10d4f-183">Teste o aplicativo para verificar se ele está funcionando.</span><span class="sxs-lookup"><span data-stu-id="10d4f-183">Test the application to verify it's working.</span></span>

    <span data-ttu-id="10d4f-184">Na primeira vez que você executar uma página que acessa o banco de dados do, a Entity Framework executará todas as migrações `Up` métodos necessários para atualizar o banco de dados com o modelo de dado atual.</span><span class="sxs-lookup"><span data-stu-id="10d4f-184">The first time you run a page that accesses the database, the Entity Framework runs all of the migrations `Up` methods required to bring the database up to date with the current data model.</span></span> <span data-ttu-id="10d4f-185">Agora você pode usar todas as páginas da Web que adicionou desde a última vez que implantou o, incluindo as páginas do departamento que você adicionou neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="10d4f-185">You can now use all of the web pages that you added since the last time you deployed, including the Department pages that you added in this tutorial.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="10d4f-186">Obter o código</span><span class="sxs-lookup"><span data-stu-id="10d4f-186">Get the code</span></span>

[<span data-ttu-id="10d4f-187">Baixar o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="10d4f-187">Download the Completed Project</span></span>](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a><span data-ttu-id="10d4f-188">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="10d4f-188">Additional resources</span></span>

<span data-ttu-id="10d4f-189">Links para outros recursos de Entity Framework podem ser encontrados nos [recursos de acesso a dados ASP.net-recomendado](../../../../whitepapers/aspnet-data-access-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="10d4f-189">Links to other Entity Framework resources can be found in the [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="10d4f-190">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="10d4f-190">Next steps</span></span>

<span data-ttu-id="10d4f-191">Neste tutorial, você:</span><span class="sxs-lookup"><span data-stu-id="10d4f-191">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="10d4f-192">Aprendeu sobre código assíncrono</span><span class="sxs-lookup"><span data-stu-id="10d4f-192">Learned about asynchronous code</span></span>
> * <span data-ttu-id="10d4f-193">Criou um controlador de departamento</span><span class="sxs-lookup"><span data-stu-id="10d4f-193">Created a Department controller</span></span>
> * <span data-ttu-id="10d4f-194">Procedimentos armazenados usados</span><span class="sxs-lookup"><span data-stu-id="10d4f-194">Used stored procedures</span></span>
> * <span data-ttu-id="10d4f-195">Implantado no Azure</span><span class="sxs-lookup"><span data-stu-id="10d4f-195">Deployed to Azure</span></span>

<span data-ttu-id="10d4f-196">Avance para o próximo artigo para saber como lidar com conflitos quando vários usuários atualizam a mesma entidade ao mesmo tempo.</span><span class="sxs-lookup"><span data-stu-id="10d4f-196">Advance to the next article to learn how to handle conflicts when multiple users update the same entity at the same time.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="10d4f-197">Manipulando a simultaneidade</span><span class="sxs-lookup"><span data-stu-id="10d4f-197">Handling concurrency</span></span>](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
