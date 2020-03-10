---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: Criando um banco de dados | Microsoft Docs
author: shanselman
description: Este é um tutorial principiante que apresenta os fundamentos do ASP.NET MVC. Crie um aplicativo Web simples que lê e grava de um banco de dados.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: 995db714ce6365415d06dc458aee84a31c7c8fd6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78581434"
---
# <a name="creating-a-database"></a><span data-ttu-id="f4e24-104">Criar um banco de dados</span><span class="sxs-lookup"><span data-stu-id="f4e24-104">Creating a Database</span></span>

<span data-ttu-id="f4e24-105">por [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="f4e24-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="f4e24-106">Este é um tutorial principiante que apresenta os fundamentos do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f4e24-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="f4e24-107">Você criará um aplicativo Web simples que lê e grava de um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f4e24-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="f4e24-108">Visite o [ASP.NET MVC Learning Center](../../../index.md) para encontrar outros tutoriais e exemplos do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f4e24-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>

<span data-ttu-id="f4e24-109">Nesta seção, vamos criar um novo banco de dados do SQL Express que usaremos para armazenar e recuperar os nossos dados de filme.</span><span class="sxs-lookup"><span data-stu-id="f4e24-109">In this section we are going to create a new SQL Express database that we'll use to store and retrieve our movie data.</span></span> <span data-ttu-id="f4e24-110">No IDE do Visual Web Developer, selecione Exibir | Gerenciador de Servidores.</span><span class="sxs-lookup"><span data-stu-id="f4e24-110">From within the Visual Web Developer IDE, select View | Server Explorer.</span></span> <span data-ttu-id="f4e24-111">Clique com o botão direito em conexões de dados e clique em Adicionar conexão...</span><span class="sxs-lookup"><span data-stu-id="f4e24-111">Right click on Data Connections and click Add Connection...</span></span>

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

<span data-ttu-id="f4e24-113">Na caixa de diálogo escolher fonte de dados, selecione Microsoft SQL Server e selecione continuar.</span><span class="sxs-lookup"><span data-stu-id="f4e24-113">In the Choose Data Source dialog, select Microsoft SQL Server and select Continue.</span></span>

![](getting-started-with-mvc-part4/_static/image2.png)

<span data-ttu-id="f4e24-114">Na caixa de diálogo Adicionar conexão, digite ".\SQLEXPRESS" para o nome do servidor e insira "filmes" como o nome do novo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f4e24-114">In the Add Connection dialog, enter ".\SQLEXPRESS" for your Server Name, and enter "Movies" as the name for your new database.</span></span>

<span data-ttu-id="f4e24-115">[caixa de diálogo ![Adicionar conexão](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="f4e24-115">[![Add Connection dialog](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span></span>

<span data-ttu-id="f4e24-116">Clique em OK e você será perguntado se deseja criar esse banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f4e24-116">Click OK and you'll be asked if you want to create that database.</span></span> <span data-ttu-id="f4e24-117">Selecione Sim.</span><span class="sxs-lookup"><span data-stu-id="f4e24-117">Select yes.</span></span>

<span data-ttu-id="f4e24-118">[![criar filmes?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="f4e24-118">[![Create Movies?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span></span>

<span data-ttu-id="f4e24-119">Agora você tem um banco de dados vazio no Gerenciador de Servidores.</span><span class="sxs-lookup"><span data-stu-id="f4e24-119">Now you've got an empty database in Server Explorer.</span></span>

![Adicionar nova tabela](getting-started-with-mvc-part4/_static/image7.png)

<span data-ttu-id="f4e24-121">Clique com o botão direito do mouse em tabelas e clique em Adicionar tabela.</span><span class="sxs-lookup"><span data-stu-id="f4e24-121">Right click on Tables and click Add Table.</span></span> <span data-ttu-id="f4e24-122">O Designer de Tabela será exibido.</span><span class="sxs-lookup"><span data-stu-id="f4e24-122">The Table Designer will appear.</span></span> <span data-ttu-id="f4e24-123">Adicione colunas para ID, título, liberado, gênero e preço.</span><span class="sxs-lookup"><span data-stu-id="f4e24-123">Add columns for Id, Title, ReleaseDate, Genre, and Price.</span></span> <span data-ttu-id="f4e24-124">Clique com o botão direito do mouse na coluna ID e clique em definir chave primária.</span><span class="sxs-lookup"><span data-stu-id="f4e24-124">Right click on the ID column and click set Primary Key.</span></span> <span data-ttu-id="f4e24-125">Veja a aparência de minhas áreas de design.</span><span class="sxs-lookup"><span data-stu-id="f4e24-125">Here's what my design areas looks like.</span></span>

<span data-ttu-id="f4e24-126">[Editor de tabela de banco de dados ![](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="f4e24-126">[![Database Table Editor](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span></span>

<span data-ttu-id="f4e24-127">Além disso, selecione a coluna ID e, em Propriedades da coluna abaixo, altere "especificação de identidade" para "Sim".</span><span class="sxs-lookup"><span data-stu-id="f4e24-127">Also, select the Id column and under Column Properties below change "Identity Specification" to "Yes."</span></span>

<span data-ttu-id="f4e24-128">[![ndo IsIdentity-Propriedades da coluna](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="f4e24-128">[![IsIdentity - Column Properties](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span></span>

<span data-ttu-id="f4e24-129">Quando tiver feito isso, clique no ícone salvar na barra de ferramentas ou selecione Arquivo | Salve no menu e nomeie a tabela "**filme**" (singular).</span><span class="sxs-lookup"><span data-stu-id="f4e24-129">When you've got it done, click the Save icon in the toolbar or select File | Save from the menu, and name your table "**Movie**" (singular).</span></span> <span data-ttu-id="f4e24-130">Temos um banco de dados e uma tabela!</span><span class="sxs-lookup"><span data-stu-id="f4e24-130">We've got a database and a table!</span></span>

<span data-ttu-id="f4e24-131">[![escolher nome](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="f4e24-131">[![Choose Name](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span></span>

<span data-ttu-id="f4e24-132">Volte para Gerenciador de Servidores e clique com o botão direito do mouse na tabela filme e selecione "mostrar dados da tabela".</span><span class="sxs-lookup"><span data-stu-id="f4e24-132">Go back to Server Explorer and right click the Movie table, then select "Show Table Data."</span></span> <span data-ttu-id="f4e24-133">Insira alguns filmes para que o nosso banco de dados tenha algum dado.</span><span class="sxs-lookup"><span data-stu-id="f4e24-133">Enter a few movies so our database has some data.</span></span>

<span data-ttu-id="f4e24-134">[Edição de tabela de banco de dados ![](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span><span class="sxs-lookup"><span data-stu-id="f4e24-134">[![Database Table Editing](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span></span>

## <a name="creating-a-model"></a><span data-ttu-id="f4e24-135">Criar um Modelo</span><span class="sxs-lookup"><span data-stu-id="f4e24-135">Creating a Model</span></span>

<span data-ttu-id="f4e24-136">Agora, volte para a Gerenciador de Soluções no lado direito do IDE e clique com o botão direito do mouse na pasta modelos e selecione Adicionar | Novo item.</span><span class="sxs-lookup"><span data-stu-id="f4e24-136">Now, switch back to the Solution Explorer on the right hand side of the IDE and right-click on the Models folder and select Add | New Item.</span></span>

<span data-ttu-id="f4e24-137">[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="f4e24-137">[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span></span>

<span data-ttu-id="f4e24-138">Vamos criar um modelo de entidade do nosso novo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f4e24-138">We're going to create an Entity Model from our new database.</span></span> <span data-ttu-id="f4e24-139">Isso adicionará um conjunto de classes ao nosso projeto que facilitará a consulta e a manipulação dos dados em nosso banco de dado.</span><span class="sxs-lookup"><span data-stu-id="f4e24-139">This will add a set of classes to our project that makes it easy for us to query and manipulate the data within our database.</span></span> <span data-ttu-id="f4e24-140">Selecione o nó de dados no lado esquerdo da caixa de diálogo e, em seguida, selecione o modelo de item de Modelo de Dados de Entidade ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="f4e24-140">Select the Data node on the left-hand side of the dialog, and then select the ADO.NET Entity Data Model item template.</span></span> <span data-ttu-id="f4e24-141">Nomeie-o como Movies. edmx.</span><span class="sxs-lookup"><span data-stu-id="f4e24-141">Name it Movies.edmx.</span></span>

<span data-ttu-id="f4e24-142">[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span><span class="sxs-lookup"><span data-stu-id="f4e24-142">[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span></span>

<span data-ttu-id="f4e24-143">Clique no botão "Adicionar".</span><span class="sxs-lookup"><span data-stu-id="f4e24-143">Click the "Add" button.</span></span> <span data-ttu-id="f4e24-144">Isso iniciará o "Assistente de Modelo de Dados de Entidade".</span><span class="sxs-lookup"><span data-stu-id="f4e24-144">This will then launch the "Entity Data Model Wizard".</span></span>

<span data-ttu-id="f4e24-145">Na caixa de diálogo Nova que aparece, selecione gerar do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f4e24-145">In the new dialog that pops up, select Generate from Database.</span></span> <span data-ttu-id="f4e24-146">Como acabamos de criar um banco de dados, só precisaremos dizer ao Entity Framework sobre nosso novo banco de dados e sua tabela.</span><span class="sxs-lookup"><span data-stu-id="f4e24-146">Since we've just made a database, we'll only need to tell the Entity Framework about our new database and its table.</span></span> <span data-ttu-id="f4e24-147">Clique em avançar para salvar nossa conexão de banco de dados na configuração de nosso aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="f4e24-147">Click Next to save our database connection in our web application's configuration.</span></span> <span data-ttu-id="f4e24-148">Agora, marque a caixa de seleção tabelas e filmes e clique em concluir.</span><span class="sxs-lookup"><span data-stu-id="f4e24-148">Now, check the Tables and Movie checkbox and click Finish.</span></span>

<span data-ttu-id="f4e24-149">[Assistente de Modelo de Dados de Entidade de ![](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span><span class="sxs-lookup"><span data-stu-id="f4e24-149">[![Entity Data Model Wizard](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span></span>

<span data-ttu-id="f4e24-150">Agora, podemos ver nossa nova tabela de filmes na Entity Framework Designer e acessá-la a partir do código.</span><span class="sxs-lookup"><span data-stu-id="f4e24-150">Now we can see our new Movie table in the Entity Framework Designer and access it from code.</span></span>

<span data-ttu-id="f4e24-151">[Filmes de ![-Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="f4e24-151">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span></span>

<span data-ttu-id="f4e24-152">Na superfície de design, você pode ver uma classe de "filme".</span><span class="sxs-lookup"><span data-stu-id="f4e24-152">On the design surface you can see a "Movie" class.</span></span> <span data-ttu-id="f4e24-153">Essa classe é mapeada para a tabela "filme" em nosso banco de dados, e cada propriedade dentro dela é mapeada para uma coluna com a tabela.</span><span class="sxs-lookup"><span data-stu-id="f4e24-153">This class maps to the "Movie" table in our database, and each property within it maps to a column with the table.</span></span> <span data-ttu-id="f4e24-154">Cada instância de uma classe "filme" corresponderá a uma linha na tabela "filme".</span><span class="sxs-lookup"><span data-stu-id="f4e24-154">Each instance of a "Movie" class will correspond to a row within the "Movie" table.</span></span>

<span data-ttu-id="f4e24-155">Se você não gostar das convenções de nomenclatura e mapeamento padrão usadas pelo Entity Framework, poderá usar o designer de Entity Framework para alterá-las ou personalizá-las.</span><span class="sxs-lookup"><span data-stu-id="f4e24-155">If you don't like the default naming and mapping conventions used by the Entity Framework, you can use the Entity Framework designer to change or customize them.</span></span> <span data-ttu-id="f4e24-156">Para este aplicativo, usaremos os padrões e apenas salvaremos o arquivo como está.</span><span class="sxs-lookup"><span data-stu-id="f4e24-156">For this application we'll use the defaults and just save the file as-is.</span></span>

<span data-ttu-id="f4e24-157">Agora, vamos trabalhar com alguns dados reais!</span><span class="sxs-lookup"><span data-stu-id="f4e24-157">Now, let's work with some real data!</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f4e24-158">[Anterior](getting-started-with-mvc-part3.md)
> [Próximo](getting-started-with-mvc-part5.md)</span><span class="sxs-lookup"><span data-stu-id="f4e24-158">[Previous](getting-started-with-mvc-part3.md)
[Next](getting-started-with-mvc-part5.md)</span></span>
