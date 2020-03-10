---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Tutorial: implementar a herança com o EF em um aplicativo ASP.NET MVC 5'
description: Este tutorial mostrará como implementar a herança no modelo de dados.
author: tdykstra
ms.author: riande
ms.date: 01/21/2019
ms.topic: tutorial
ms.assetid: 08834147-77ec-454a-bb7a-d931d2a40dab
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 73a01ed47b0935a1a9734c197377470defb1fe36
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583058"
---
# <a name="tutorial-implement-inheritance-with-ef-in-an-aspnet-mvc-5-app"></a><span data-ttu-id="295f5-103">Tutorial: implementar a herança com o EF em um aplicativo MVC 5 do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="295f5-103">Tutorial: Implement Inheritance with EF in an ASP.NET MVC 5 app</span></span>

<span data-ttu-id="295f5-104">No tutorial anterior, você tratou exceções de simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="295f5-104">In the previous tutorial you handled concurrency exceptions.</span></span> <span data-ttu-id="295f5-105">Este tutorial mostrará como implementar a herança no modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="295f5-105">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="295f5-106">Na programação orientada a objeto, você pode usar a [herança](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) para facilitar a [reutilização de código](http://en.wikipedia.org/wiki/Code_reuse).</span><span class="sxs-lookup"><span data-stu-id="295f5-106">In object-oriented programming, you can use [inheritance](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) to facilitate [code reuse](http://en.wikipedia.org/wiki/Code_reuse).</span></span> <span data-ttu-id="295f5-107">Neste tutorial, você alterará as classes `Instructor` e `Student`, de modo que elas derivem de uma classe base `Person` que contém propriedades, como `LastName`, comuns a instrutores e alunos.</span><span class="sxs-lookup"><span data-stu-id="295f5-107">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="295f5-108">Você não adicionará nem alterará as páginas da Web, mas alterará uma parte do código, e essas alterações serão refletidas automaticamente no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="295f5-108">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

<span data-ttu-id="295f5-109">Neste tutorial, você:</span><span class="sxs-lookup"><span data-stu-id="295f5-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="295f5-110">Saiba como mapear a herança para o banco de dados</span><span class="sxs-lookup"><span data-stu-id="295f5-110">Learn to map inheritance to database</span></span>
> * <span data-ttu-id="295f5-111">Criar a classe Person</span><span class="sxs-lookup"><span data-stu-id="295f5-111">Create the Person class</span></span>
> * <span data-ttu-id="295f5-112">Atualizará Instructor e Student</span><span class="sxs-lookup"><span data-stu-id="295f5-112">Update Instructor and Student</span></span>
> * <span data-ttu-id="295f5-113">Adicionar pessoa ao modelo</span><span class="sxs-lookup"><span data-stu-id="295f5-113">Add Person to the Model</span></span>
> * <span data-ttu-id="295f5-114">Criar e atualizar migrações</span><span class="sxs-lookup"><span data-stu-id="295f5-114">Create and Update Migrations</span></span>
> * <span data-ttu-id="295f5-115">Testará a implementação</span><span class="sxs-lookup"><span data-stu-id="295f5-115">Test the implementation</span></span>
> * <span data-ttu-id="295f5-116">Implantar no Azure</span><span class="sxs-lookup"><span data-stu-id="295f5-116">Deploy to Azure</span></span>

## <a name="prerequisites"></a><span data-ttu-id="295f5-117">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="295f5-117">Prerequisites</span></span>

* [<span data-ttu-id="295f5-118">Tratamento de simultaneidade</span><span class="sxs-lookup"><span data-stu-id="295f5-118">Handling Concurrency</span></span>](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="map-inheritance-to-database"></a><span data-ttu-id="295f5-119">Mapeará a herança para o banco de dados</span><span class="sxs-lookup"><span data-stu-id="295f5-119">Map inheritance to database</span></span>

<span data-ttu-id="295f5-120">As classes `Instructor` e `Student` no modelo de dados `School` têm várias propriedades idênticas:</span><span class="sxs-lookup"><span data-stu-id="295f5-120">The `Instructor` and `Student` classes in the `School` data model have several properties that are identical:</span></span>

![Student_and_Instructor_classes](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

<span data-ttu-id="295f5-122">Suponha que você deseje eliminar o código redundante para as propriedades compartilhadas pelas entidades `Instructor` e `Student`.</span><span class="sxs-lookup"><span data-stu-id="295f5-122">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="295f5-123">Ou você deseja escrever um serviço que pode formatar nomes sem se preocupar se o nome foi obtido de um instrutor ou um aluno.</span><span class="sxs-lookup"><span data-stu-id="295f5-123">Or you want to write a service that can format names without caring whether the name came from an instructor or a student.</span></span> <span data-ttu-id="295f5-124">Você pode criar uma classe base `Person` que contém apenas essas propriedades compartilhadas e, em seguida, fazer com que as entidades `Instructor` e `Student` herdem dessa classe base, conforme mostrado na ilustração a seguir:</span><span class="sxs-lookup"><span data-stu-id="295f5-124">You could create a `Person` base class which contains only those shared properties, then make the `Instructor` and `Student` entities inherit from that base class, as shown in the following illustration:</span></span>

![Student_and_Instructor_classes_deriving_from_Person_class](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

<span data-ttu-id="295f5-126">Há várias maneiras pelas quais essa estrutura de herança pode ser representada no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="295f5-126">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="295f5-127">Você pode ter uma tabela `Person` que inclui informações sobre alunos e instrutores em uma única tabela.</span><span class="sxs-lookup"><span data-stu-id="295f5-127">You could have a `Person` table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="295f5-128">Algumas das colunas podem se aplicar somente a instrutores (`HireDate`), apenas a alunos (`EnrollmentDate`), algumas para ambos (`LastName``FirstName`).</span><span class="sxs-lookup"><span data-stu-id="295f5-128">Some of the columns could apply only to instructors (`HireDate`), some only to students (`EnrollmentDate`), some to both (`LastName`, `FirstName`).</span></span> <span data-ttu-id="295f5-129">Normalmente, você teria uma coluna *discriminadora* para indicar qual tipo cada linha representa.</span><span class="sxs-lookup"><span data-stu-id="295f5-129">Typically, you'd have a *discriminator* column to indicate which type each row represents.</span></span> <span data-ttu-id="295f5-130">Por exemplo, a coluna discriminatória pode ter "Instrutor" para instrutores e "Aluno" para alunos.</span><span class="sxs-lookup"><span data-stu-id="295f5-130">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![Table-per-hierarchy_example](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

<span data-ttu-id="295f5-132">Esse padrão de gerar uma estrutura de herança de entidade a partir de uma única tabela de banco de dados é chamada de herança de *tabela por hierarquia* (TPH).</span><span class="sxs-lookup"><span data-stu-id="295f5-132">This pattern of generating an entity inheritance structure from a single database table is called *table-per-hierarchy* (TPH) inheritance.</span></span>

<span data-ttu-id="295f5-133">Uma alternativa é fazer com que o banco de dados se pareça mais com a estrutura de herança.</span><span class="sxs-lookup"><span data-stu-id="295f5-133">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="295f5-134">Por exemplo, você pode ter apenas os campos nome na tabela `Person` e ter tabelas `Instructor` e `Student` separadas com os campos de data.</span><span class="sxs-lookup"><span data-stu-id="295f5-134">For example, you could have only the name fields in the `Person` table and have separate `Instructor` and `Student` tables with the date fields.</span></span>

![Tabela por type_inheritance](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

<span data-ttu-id="295f5-136">Esse padrão de fazer uma tabela de banco de dados para cada classe de entidade é chamado de herança de *tabela por tipo* (TPT).</span><span class="sxs-lookup"><span data-stu-id="295f5-136">This pattern of making a database table for each entity class is called *table per type* (TPT) inheritance.</span></span>

<span data-ttu-id="295f5-137">Outra opção é mapear todos os tipos não abstratos para tabelas individuais.</span><span class="sxs-lookup"><span data-stu-id="295f5-137">Yet another option is to map all non-abstract types to individual tables.</span></span> <span data-ttu-id="295f5-138">Todas as propriedades de uma classe, incluindo propriedades herdadas, são mapeadas para colunas da tabela correspondente.</span><span class="sxs-lookup"><span data-stu-id="295f5-138">All properties of a class, including inherited properties, map to columns of the corresponding table.</span></span> <span data-ttu-id="295f5-139">Esse padrão é chamado de herança TPC (Tabela por Classe Concreta).</span><span class="sxs-lookup"><span data-stu-id="295f5-139">This pattern is called Table-per-Concrete Class (TPC) inheritance.</span></span> <span data-ttu-id="295f5-140">Se você implementou a herança do TPC para as classes `Person`, `Student`e `Instructor`, conforme mostrado anteriormente, as tabelas `Student` e `Instructor` não serão diferentes depois de implementar a herança do que antes.</span><span class="sxs-lookup"><span data-stu-id="295f5-140">If you implemented TPC inheritance for the `Person`, `Student`, and `Instructor` classes as shown earlier, the `Student` and `Instructor` tables would look no different after implementing inheritance than they did before.</span></span>

<span data-ttu-id="295f5-141">Os padrões de herança do TPC e do TPH geralmente oferecem melhor desempenho no Entity Framework que os padrões de herança do TPT, pois os padrões de TPT podem resultar em consultas de junção complexas.</span><span class="sxs-lookup"><span data-stu-id="295f5-141">TPC and TPH inheritance patterns generally deliver better performance in the Entity Framework than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span>

<span data-ttu-id="295f5-142">Este tutorial demonstra como implementar a herança TPH.</span><span class="sxs-lookup"><span data-stu-id="295f5-142">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="295f5-143">TPH é o padrão de herança padrão no Entity Framework, portanto, tudo o que você precisa fazer é criar uma classe `Person`, alterar as classes `Instructor` e `Student` para derivar de `Person`, adicionar a nova classe à `DbContext`e criar uma migração.</span><span class="sxs-lookup"><span data-stu-id="295f5-143">TPH is the default inheritance pattern in the Entity Framework, so all you have to do is create a `Person` class, change the `Instructor` and `Student` classes to derive from `Person`, add the new class to the `DbContext`, and create a migration.</span></span> <span data-ttu-id="295f5-144">(Para obter informações sobre como implementar os outros padrões de herança, consulte [mapeando a herança de tabela por tipo (TPT)](https://msdn.microsoft.com/data/jj591617#2.5) e [mapeando a herança da classe de tabela por concreto (TPC)](https://msdn.microsoft.com/data/jj591617#2.6) na documentação do MSDN Entity Framework.)</span><span class="sxs-lookup"><span data-stu-id="295f5-144">(For information about how to implement the other inheritance patterns, see [Mapping the Table-Per-Type (TPT) Inheritance](https://msdn.microsoft.com/data/jj591617#2.5) and [Mapping the Table-Per-Concrete Class (TPC) Inheritance](https://msdn.microsoft.com/data/jj591617#2.6) in the MSDN Entity Framework documentation.)</span></span>

## <a name="create-the-person-class"></a><span data-ttu-id="295f5-145">Criar a classe Person</span><span class="sxs-lookup"><span data-stu-id="295f5-145">Create the Person class</span></span>

<span data-ttu-id="295f5-146">Na pasta *modelos* , crie *Person.cs* e substitua o código do modelo pelo código a seguir:</span><span class="sxs-lookup"><span data-stu-id="295f5-146">In the *Models* folder, create *Person.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="update-instructor-and-student"></a><span data-ttu-id="295f5-147">Atualizará Instructor e Student</span><span class="sxs-lookup"><span data-stu-id="295f5-147">Update Instructor and Student</span></span>

<span data-ttu-id="295f5-148">Agora, atualize *Instructor.cs* e *Student.cs* para herdar valores do *Person.SC*.</span><span class="sxs-lookup"><span data-stu-id="295f5-148">Now update the *Instructor.cs* and *Student.cs* to inherit values from the *Person.sc*.</span></span>

<span data-ttu-id="295f5-149">No *Instructor.cs*, derive a classe `Instructor` da classe `Person` e remova os campos de chave e nome.</span><span class="sxs-lookup"><span data-stu-id="295f5-149">In *Instructor.cs*, derive the `Instructor` class from the `Person` class and remove the key and name fields.</span></span> <span data-ttu-id="295f5-150">O código será semelhante ao seguinte exemplo:</span><span class="sxs-lookup"><span data-stu-id="295f5-150">The code will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="295f5-151">Faça alterações semelhantes em *Student.cs*.</span><span class="sxs-lookup"><span data-stu-id="295f5-151">Make similar changes to *Student.cs*.</span></span> <span data-ttu-id="295f5-152">A classe `Student` se parecerá com o exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="295f5-152">The `Student` class will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="add-person-to-the-model"></a><span data-ttu-id="295f5-153">Adicionar pessoa ao modelo</span><span class="sxs-lookup"><span data-stu-id="295f5-153">Add Person to the Model</span></span>

<span data-ttu-id="295f5-154">No *SchoolContext.cs*, adicione uma propriedade `DbSet` para o tipo de entidade `Person`:</span><span class="sxs-lookup"><span data-stu-id="295f5-154">In *SchoolContext.cs*, add a `DbSet` property for the `Person` entity type:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

<span data-ttu-id="295f5-155">Isso é tudo o que o Entity Framework precisa para configurar a herança de tabela por hierarquia.</span><span class="sxs-lookup"><span data-stu-id="295f5-155">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="295f5-156">Como você verá, quando o banco de dados for atualizado, ele terá uma tabela `Person` no lugar das tabelas `Student` e `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="295f5-156">As you'll see, when the database is updated, it will have a `Person` table in place of the `Student` and `Instructor` tables.</span></span>

## <a name="create-and-update-migrations"></a><span data-ttu-id="295f5-157">Criar e atualizar migrações</span><span class="sxs-lookup"><span data-stu-id="295f5-157">Create and Update Migrations</span></span>

<span data-ttu-id="295f5-158">No console do Gerenciador de pacotes (PMC), digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="295f5-158">In the Package Manager Console (PMC), enter the following command:</span></span>

`Add-Migration Inheritance`

<span data-ttu-id="295f5-159">Execute o comando `Update-Database` no PMC.</span><span class="sxs-lookup"><span data-stu-id="295f5-159">Run the `Update-Database` command in the PMC.</span></span> <span data-ttu-id="295f5-160">O comando falhará neste ponto porque temos dados existentes que as migrações não sabem como lidar.</span><span class="sxs-lookup"><span data-stu-id="295f5-160">The command will fail at this point because we have existing data that migrations doesn't know how to handle.</span></span> <span data-ttu-id="295f5-161">Você receberá uma mensagem de erro semelhante à seguinte:</span><span class="sxs-lookup"><span data-stu-id="295f5-161">You get an error message like the following one:</span></span>

> <span data-ttu-id="295f5-162">*Não foi possível descartar o objeto ' dbo. Instrutor ' porque é referenciado por uma restrição FOREIGN KEY.*</span><span class="sxs-lookup"><span data-stu-id="295f5-162">*Could not drop object 'dbo.Instructor' because it is referenced by a FOREIGN KEY constraint.*</span></span>

<span data-ttu-id="295f5-163">Abra as *migrações\&lt; timestamp&gt;\_Inheritance.cs* e substitua o método `Up` pelo código a seguir:</span><span class="sxs-lookup"><span data-stu-id="295f5-163">Open *Migrations\&lt;timestamp&gt;\_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

<span data-ttu-id="295f5-164">Este código é responsável pelas seguintes tarefas de atualização de banco de dados:</span><span class="sxs-lookup"><span data-stu-id="295f5-164">This code takes care of the following database update tasks:</span></span>

- <span data-ttu-id="295f5-165">Remove as restrições de chave estrangeira e índices que apontam para a tabela Aluno.</span><span class="sxs-lookup"><span data-stu-id="295f5-165">Removes foreign key constraints and indexes that point to the Student table.</span></span>
- <span data-ttu-id="295f5-166">Renomeia a tabela Instrutor como Pessoa e faz as alterações necessárias para que ela armazene dados de Aluno:</span><span class="sxs-lookup"><span data-stu-id="295f5-166">Renames the Instructor table as Person and makes changes needed for it to store Student data:</span></span>

    - <span data-ttu-id="295f5-167">Adiciona EnrollmentDate que permite valo nulo para os alunos.</span><span class="sxs-lookup"><span data-stu-id="295f5-167">Adds nullable EnrollmentDate for students.</span></span>
    - <span data-ttu-id="295f5-168">Adiciona a coluna Discriminatória para indicar se uma linha refere-se a um aluno ou um instrutor.</span><span class="sxs-lookup"><span data-stu-id="295f5-168">Adds Discriminator column to indicate whether a row is for a student or an instructor.</span></span>
    - <span data-ttu-id="295f5-169">Faz com que HireDate permita valor nulo, pois linhas de alunos não terão datas de contratação.</span><span class="sxs-lookup"><span data-stu-id="295f5-169">Makes HireDate nullable since student rows won't have hire dates.</span></span>
    - <span data-ttu-id="295f5-170">Adiciona um campo temporário que será usado para atualizar chaves estrangeiras que apontam para alunos.</span><span class="sxs-lookup"><span data-stu-id="295f5-170">Adds a temporary field that will be used to update foreign keys that point to students.</span></span> <span data-ttu-id="295f5-171">Quando você copiar os alunos para a tabela Person, eles obterão novos valores de chave primária.</span><span class="sxs-lookup"><span data-stu-id="295f5-171">When you copy students into the Person table they'll get new primary key values.</span></span>
- <span data-ttu-id="295f5-172">Copia os dados da tabela Aluno para a tabela Pessoa.</span><span class="sxs-lookup"><span data-stu-id="295f5-172">Copies data from the Student table into the Person table.</span></span> <span data-ttu-id="295f5-173">Isso faz com que os alunos recebam novos valores de chave primária.</span><span class="sxs-lookup"><span data-stu-id="295f5-173">This causes students to get assigned new primary key values.</span></span>
- <span data-ttu-id="295f5-174">Corrige valores de chave estrangeira que apontam para alunos.</span><span class="sxs-lookup"><span data-stu-id="295f5-174">Fixes foreign key values that point to students.</span></span>
- <span data-ttu-id="295f5-175">Recria restrições de chave estrangeira e índices, agora apontando-os para a tabela Person.</span><span class="sxs-lookup"><span data-stu-id="295f5-175">Re-creates foreign key constraints and indexes, now pointing them to the Person table.</span></span>

<span data-ttu-id="295f5-176">(Se você tiver usado o GUID, em vez de inteiro como o tipo de chave primária, os valores de chave primária dos alunos não precisarão ser alterados e várias dessas etapas poderão ser omitidas.)</span><span class="sxs-lookup"><span data-stu-id="295f5-176">(If you had used GUID instead of integer as the primary key type, the student primary key values wouldn't have to change, and several of these steps could have been omitted.)</span></span>

<span data-ttu-id="295f5-177">Execute o comando `update-database` novamente.</span><span class="sxs-lookup"><span data-stu-id="295f5-177">Run the `update-database` command again.</span></span>

<span data-ttu-id="295f5-178">(Em um sistema de produção, você deve fazer alterações correspondentes no método inoperante, caso você tenha feito isso para voltar para a versão anterior do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="295f5-178">(In a production system you would make corresponding changes to the Down method in case you ever had to use that to go back to the previous database version.</span></span> <span data-ttu-id="295f5-179">Para este tutorial, você não usará o método down.)</span><span class="sxs-lookup"><span data-stu-id="295f5-179">For this tutorial you won't be using the Down method.)</span></span>

> [!NOTE]
> <span data-ttu-id="295f5-180">É possível obter outros erros ao migrar dados e fazer alterações de esquema.</span><span class="sxs-lookup"><span data-stu-id="295f5-180">It's possible to get other errors when migrating data and making schema changes.</span></span> <span data-ttu-id="295f5-181">Se você obtiver erros de migração que não pode resolver, poderá continuar com o tutorial alterando a cadeia de conexão no arquivo *Web. config* ou excluindo o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="295f5-181">If you get migration errors you can't resolve, you can continue with the tutorial by changing the connection string in the *Web.config* file or by deleting the database.</span></span> <span data-ttu-id="295f5-182">A abordagem mais simples é renomear o banco de dados no arquivo *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="295f5-182">The simplest approach is to rename the database in the *Web.config* file.</span></span> <span data-ttu-id="295f5-183">Por exemplo, altere o nome do banco de dados para ContosoUniversity2, conforme mostrado no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="295f5-183">For example, change the database name to ContosoUniversity2 as shown in the following example:</span></span>
>
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.xml?highlight=2)]
>
> <span data-ttu-id="295f5-184">Com um novo banco de dados, não há nenhum dado a ser migrado, e o comando `update-database` é muito mais provável de ser concluído sem erros.</span><span class="sxs-lookup"><span data-stu-id="295f5-184">With a new database, there is no data to migrate, and the `update-database` command is much more likely to complete without errors.</span></span> <span data-ttu-id="295f5-185">Para obter instruções sobre como excluir o banco de dados, consulte [como remover um banco de dados do Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span><span class="sxs-lookup"><span data-stu-id="295f5-185">For instructions on how to delete the database, see [How to Drop a Database from Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span></span> <span data-ttu-id="295f5-186">Se você usar essa abordagem para continuar com o tutorial, ignore a etapa de implantação no final deste tutorial ou implante em um novo site e banco de dados.</span><span class="sxs-lookup"><span data-stu-id="295f5-186">If you take this approach in order to continue with the tutorial, skip the deployment step at the end of this tutorial or deploy to a new site and database.</span></span> <span data-ttu-id="295f5-187">Se você implantar uma atualização no mesmo site em que já estava implantado, o EF receberá o mesmo erro quando executar migrações automaticamente.</span><span class="sxs-lookup"><span data-stu-id="295f5-187">If you deploy an update to the same site you've been deploying to already, EF will get the same error there when it runs migrations automatically.</span></span> <span data-ttu-id="295f5-188">Se você quiser solucionar um erro de migrações, o melhor recurso é um dos fóruns Entity Framework ou StackOverflow.com.</span><span class="sxs-lookup"><span data-stu-id="295f5-188">If you want to troubleshoot a migrations error, the best resource is one of the Entity Framework forums or StackOverflow.com.</span></span>

## <a name="test-the-implementation"></a><span data-ttu-id="295f5-189">Testará a implementação</span><span class="sxs-lookup"><span data-stu-id="295f5-189">Test the implementation</span></span>

<span data-ttu-id="295f5-190">Execute o site e tente várias páginas.</span><span class="sxs-lookup"><span data-stu-id="295f5-190">Run the site and try various pages.</span></span> <span data-ttu-id="295f5-191">Tudo funciona da mesma maneira que antes.</span><span class="sxs-lookup"><span data-stu-id="295f5-191">Everything works the same as it did before.</span></span>

<span data-ttu-id="295f5-192">Em **Gerenciador de servidores,** expanda **Data Connections\SchoolContext** e, em seguida, **tabelas**, e você verá que as tabelas **Student** e **instrutor** foram substituídas por uma tabela **Person** .</span><span class="sxs-lookup"><span data-stu-id="295f5-192">In **Server Explorer,** expand **Data Connections\SchoolContext** and then **Tables**, and you see that the **Student** and **Instructor** tables have been replaced by a **Person** table.</span></span> <span data-ttu-id="295f5-193">Expanda a tabela **Person** e você verá que ela tem todas as colunas que costumava estar nas tabelas **Student** e **instrutor** .</span><span class="sxs-lookup"><span data-stu-id="295f5-193">Expand the **Person** table and you see that it has all of the columns that used to be in the **Student** and **Instructor** tables.</span></span>

<span data-ttu-id="295f5-194">Clique com o botão direito do mouse na tabela Person e, em seguida, clique em **Mostrar Dados da Tabela** para ver a coluna discriminatória.</span><span class="sxs-lookup"><span data-stu-id="295f5-194">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

<span data-ttu-id="295f5-195">O diagrama a seguir ilustra a estrutura do novo banco de dados escolar:</span><span class="sxs-lookup"><span data-stu-id="295f5-195">The following diagram illustrates the structure of the new School database:</span></span>

![School_database_diagram](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="295f5-197">Implantar no Azure</span><span class="sxs-lookup"><span data-stu-id="295f5-197">Deploy to Azure</span></span>

<span data-ttu-id="295f5-198">Esta seção exige que você tenha concluído a seção opcional **implantando o aplicativo no Azure** na [parte 3, classificando, filtrando e paginação](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) desta série de tutoriais.</span><span class="sxs-lookup"><span data-stu-id="295f5-198">This section requires you to have completed the optional **Deploying the app to Azure** section in [Part 3, Sorting, Filtering, and Paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) of this tutorial series.</span></span> <span data-ttu-id="295f5-199">Se houver erros de migrações que você resolveu excluindo o banco de dados em seu projeto local, ignore esta etapa; ou crie um novo site e banco de dados e implante no novo ambiente.</span><span class="sxs-lookup"><span data-stu-id="295f5-199">If you had migrations errors that you resolved by deleting the database in your local project, skip this step; or create a new site and database, and deploy to the new environment.</span></span>

1. <span data-ttu-id="295f5-200">No Visual Studio, clique com o botão direito do mouse no projeto no **Gerenciador de Soluções** e selecione **Publicar** no menu de contexto.</span><span class="sxs-lookup"><span data-stu-id="295f5-200">In Visual Studio, right-click the project in **Solution Explorer** and select **Publish** from the context menu.</span></span>

2. <span data-ttu-id="295f5-201">Clique em **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="295f5-201">Click **Publish**.</span></span>

    <span data-ttu-id="295f5-202">O aplicativo Web é aberto no navegador padrão.</span><span class="sxs-lookup"><span data-stu-id="295f5-202">The Web app opens in your default browser.</span></span>

3. <span data-ttu-id="295f5-203">Teste o aplicativo para verificar se ele está funcionando.</span><span class="sxs-lookup"><span data-stu-id="295f5-203">Test the application to verify it's working.</span></span>

    <span data-ttu-id="295f5-204">Na primeira vez que você executar uma página que acessa o banco de dados do, a Entity Framework executará todas as migrações `Up` métodos necessários para atualizar o banco de dados com o modelo de dado atual.</span><span class="sxs-lookup"><span data-stu-id="295f5-204">The first time you run a page that accesses the database, the Entity Framework runs all of the migrations `Up` methods required to bring the database up to date with the current data model.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="295f5-205">Obter o código</span><span class="sxs-lookup"><span data-stu-id="295f5-205">Get the code</span></span>

[<span data-ttu-id="295f5-206">Baixar projeto concluído</span><span class="sxs-lookup"><span data-stu-id="295f5-206">Download Completed Project</span></span>](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a><span data-ttu-id="295f5-207">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="295f5-207">Additional resources</span></span>

<span data-ttu-id="295f5-208">Links para outros recursos de Entity Framework podem ser encontrados nos [recursos de acesso a dados ASP.net-recomendado](../../../../whitepapers/aspnet-data-access-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="295f5-208">Links to other Entity Framework resources can be found in the [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

<span data-ttu-id="295f5-209">Para obter mais informações sobre essa e outras estruturas de herança, consulte [padrão de herança de TPT](https://msdn.microsoft.com/data/jj618293) e padrão de herança de [TPH](https://msdn.microsoft.com/data/jj618292) no msdn.</span><span class="sxs-lookup"><span data-stu-id="295f5-209">For more information about this and other inheritance structures, see [TPT Inheritance Pattern](https://msdn.microsoft.com/data/jj618293) and [TPH Inheritance Pattern](https://msdn.microsoft.com/data/jj618292) on MSDN.</span></span> <span data-ttu-id="295f5-210">No próximo tutorial, você verá como lidar com uma variedade de cenários relativamente avançados do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="295f5-210">In the next tutorial you'll see how to handle a variety of relatively advanced Entity Framework scenarios.</span></span>

## <a name="next-steps"></a><span data-ttu-id="295f5-211">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="295f5-211">Next steps</span></span>

<span data-ttu-id="295f5-212">Neste tutorial, você:</span><span class="sxs-lookup"><span data-stu-id="295f5-212">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="295f5-213">Aprendeu a mapear a herança para o banco de dados</span><span class="sxs-lookup"><span data-stu-id="295f5-213">Learned to map inheritance to database</span></span>
> * <span data-ttu-id="295f5-214">Criou a classe Person</span><span class="sxs-lookup"><span data-stu-id="295f5-214">Created the Person class</span></span>
> * <span data-ttu-id="295f5-215">Atualizou Instructor e Student</span><span class="sxs-lookup"><span data-stu-id="295f5-215">Updated Instructor and Student</span></span>
> * <span data-ttu-id="295f5-216">Pessoa adicionada ao modelo</span><span class="sxs-lookup"><span data-stu-id="295f5-216">Added Person to the Model</span></span>
> * <span data-ttu-id="295f5-217">Migrações criadas e atualizadas</span><span class="sxs-lookup"><span data-stu-id="295f5-217">Created and Update Migrations</span></span>
> * <span data-ttu-id="295f5-218">Testou a implementação</span><span class="sxs-lookup"><span data-stu-id="295f5-218">Tested the implementation</span></span>
> * <span data-ttu-id="295f5-219">Implantado no Azure</span><span class="sxs-lookup"><span data-stu-id="295f5-219">Deployed to Azure</span></span>

<span data-ttu-id="295f5-220">Avance para o próximo artigo para saber mais sobre os tópicos que são úteis para conhecer quando você vai além dos conceitos básicos do desenvolvimento de aplicativos Web ASP.NET que usam Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="295f5-220">Advance to the next article to learn about topics that are useful to be aware of when you go beyond the basics of developing ASP.NET web applications that use Entity Framework Code First.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="295f5-221">Cenários de Entity Framework avançados</span><span class="sxs-lookup"><span data-stu-id="295f5-221">Advanced Entity Framework Scenarios</span></span>](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
