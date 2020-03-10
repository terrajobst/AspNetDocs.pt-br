---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Adicionando uma coluna ao modelo | Microsoft Docs
author: shanselman
description: Este é um tutorial principiante que apresenta os fundamentos do ASP.NET MVC. Crie um aplicativo Web simples que lê e grava de um banco de dados.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 1cf092c3db3959d6f47006f1be2ba82833c5dc06
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78543578"
---
# <a name="adding-a-column-to-the-model"></a><span data-ttu-id="75825-104">Adicionar uma coluna ao modelo</span><span class="sxs-lookup"><span data-stu-id="75825-104">Adding a Column to the Model</span></span>

<span data-ttu-id="75825-105">por [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="75825-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="75825-106">Este é um tutorial principiante que apresenta os fundamentos do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="75825-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="75825-107">Você criará um aplicativo Web simples que lê e grava de um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="75825-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="75825-108">Visite o [ASP.NET MVC Learning Center](../../../index.md) para encontrar outros tutoriais e exemplos do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="75825-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>

<span data-ttu-id="75825-109">Nesta seção, vamos examinar como podemos fazer alterações no esquema do nosso banco de dados e lidar com as alterações em nosso aplicativo.</span><span class="sxs-lookup"><span data-stu-id="75825-109">In this section we are going to walk through how we can make changes to the schema of our database, and handle the changes within our application.</span></span>

<span data-ttu-id="75825-110">Vamos adicionar uma coluna de "classificação" à tabela de filmes.</span><span class="sxs-lookup"><span data-stu-id="75825-110">Let's add a "Rating" Column to the Movie table.</span></span> <span data-ttu-id="75825-111">Volte para o IDE e clique no Gerenciador de Banco de Dados.</span><span class="sxs-lookup"><span data-stu-id="75825-111">Go back to the IDE and click the Database Explorer.</span></span> <span data-ttu-id="75825-112">Clique com o botão direito do mouse na tabela filme e selecione Abrir definição de tabela.</span><span class="sxs-lookup"><span data-stu-id="75825-112">Right click the Movie table and select Open Table Definition.</span></span>

<span data-ttu-id="75825-113">Adicione uma coluna de "classificação", como mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="75825-113">Add a "Rating" column as seen below.</span></span> <span data-ttu-id="75825-114">Como não temos nenhuma classificação agora, a coluna pode permitir valores nulos.</span><span class="sxs-lookup"><span data-stu-id="75825-114">Since we don't have any Ratings now, the column can allow nulls.</span></span> <span data-ttu-id="75825-115">Clique em Salvar.</span><span class="sxs-lookup"><span data-stu-id="75825-115">Click Save.</span></span>

<span data-ttu-id="75825-116">[![editar a tabela de filmes](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="75825-116">[![Editing Movies Table](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span></span>

<span data-ttu-id="75825-117">Em seguida, retorne ao Gerenciador de Soluções e abra o arquivo Movies. edmx (que está na pasta \Models).</span><span class="sxs-lookup"><span data-stu-id="75825-117">Next, return to the Solution Explorer and open up the Movies.edmx file (which is in the \Models folder).</span></span> <span data-ttu-id="75825-118">Clique com o botão direito do mouse na superfície de design (a área branca) e selecione Atualizar modelo no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="75825-118">Right click on the design surface (the white area) and select Update Model from Database.</span></span>

<span data-ttu-id="75825-119">[Filmes de ![-Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="75825-119">[![Movies - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span></span>

<span data-ttu-id="75825-120">Isso abrirá o "Assistente de atualização".</span><span class="sxs-lookup"><span data-stu-id="75825-120">This will launch the "Update Wizard".</span></span> <span data-ttu-id="75825-121">Clique na guia atualizar dentro dela e clique em concluir.</span><span class="sxs-lookup"><span data-stu-id="75825-121">Click the Refresh tab within it and click Finish.</span></span> <span data-ttu-id="75825-122">Nossa classe de modelo de filme será atualizada com a nova coluna.</span><span class="sxs-lookup"><span data-stu-id="75825-122">Our Movie model class will then be updated with the new column.</span></span>

![Assistente de atualização (2)](getting-started-with-mvc-part8/_static/image5.png)

<span data-ttu-id="75825-124">Depois de clicar em concluir, você poderá ver que a nova coluna de classificação foi adicionada à entidade de filme em nosso modelo.</span><span class="sxs-lookup"><span data-stu-id="75825-124">After clicking Finish, you can see the new Rating Column has been added to the Movie Entity in our model.</span></span>

<span data-ttu-id="75825-125">[![entidade de filme](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="75825-125">[![Movie Entity](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span></span>

<span data-ttu-id="75825-126">Adicionamos uma coluna no modelo de banco de dados, mas as exibições não sabem sobre ela.</span><span class="sxs-lookup"><span data-stu-id="75825-126">We've added a column in the database model, but the Views don't know about it.</span></span>

## <a name="update-views-with-model-changes"></a><span data-ttu-id="75825-127">Atualizar exibições com alterações de modelo</span><span class="sxs-lookup"><span data-stu-id="75825-127">Update Views with Model Changes</span></span>

<span data-ttu-id="75825-128">Há algumas maneiras de atualizar nossos modelos de exibição para refletir a nova coluna de classificação.</span><span class="sxs-lookup"><span data-stu-id="75825-128">There are a few ways we could update our view templates to reflect the new Rating column.</span></span> <span data-ttu-id="75825-129">Como criamos essas exibições gerando-as por meio da caixa de diálogo Adicionar exibição, poderíamos excluí-las e recriá-las novamente.</span><span class="sxs-lookup"><span data-stu-id="75825-129">Since we created these Views by generating them via the Add View dialog, we could delete them and recreate them again.</span></span> <span data-ttu-id="75825-130">No entanto, normalmente as pessoas já terão feito modificações em seus modelos de exibição da geração com Scaffold inicial e desejarão adicionar ou excluir campos manualmente, exatamente como fizemos com o campo ID para Create.</span><span class="sxs-lookup"><span data-stu-id="75825-130">However, typically people will have already made modifications to their View templates from the initial scaffolded generation and will want to add or delete fields manually, just as we did with the ID field for Create.</span></span>

<span data-ttu-id="75825-131">Abra o modelo \Views\Movies\Index.aspx e adicione uma classificação de&gt;&lt;&lt;/th&gt; no início da tabela de filmes.</span><span class="sxs-lookup"><span data-stu-id="75825-131">Open up the \Views\Movies\Index.aspx template and add a &lt;th&gt;Rating&lt;/th&gt; to the head of the Movie table.</span></span> <span data-ttu-id="75825-132">Adicionei o meu após o gênero.</span><span class="sxs-lookup"><span data-stu-id="75825-132">I added mine after Genre.</span></span> <span data-ttu-id="75825-133">Em seguida, na mesma posição de coluna, mas inferior, adicione uma linha para gerar a nova classificação.</span><span class="sxs-lookup"><span data-stu-id="75825-133">Then, in the same column position but lower down, add a line to output our new Rating.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

<span data-ttu-id="75825-134">Nosso modelo index. aspx final terá a seguinte aparência:</span><span class="sxs-lookup"><span data-stu-id="75825-134">Our final Index.aspx template will look like this:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

<span data-ttu-id="75825-135">Em seguida, vamos abrir o modelo \Views\Movies\Create.aspx e adicionar um rótulo e uma caixa de texto para nossa nova propriedade de classificação:</span><span class="sxs-lookup"><span data-stu-id="75825-135">Let's then open up the \Views\Movies\Create.aspx template and add a Label and Textbox for our new Rating property:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

<span data-ttu-id="75825-136">Nosso modelo Create. aspx final será semelhante a este, e vamos alterar o título do navegador e o secundário &lt;H2&gt; título para algo como "criar um filme" enquanto estamos aqui!</span><span class="sxs-lookup"><span data-stu-id="75825-136">Our final Create.aspx template will look like this, and let's change our browser's title and secondary &lt;h2&gt; title to something like "Create a Movie" while we're in here!</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

<span data-ttu-id="75825-137">Execute seu aplicativo e agora você tem um novo campo no banco de dados que foi adicionado à página criar.</span><span class="sxs-lookup"><span data-stu-id="75825-137">Run your app and now you've got a new field in the database that's been added to the Create page.</span></span> <span data-ttu-id="75825-138">Adicione um novo filme-desta vez com uma classificação-e clique em criar.</span><span class="sxs-lookup"><span data-stu-id="75825-138">Add a new Movie - this time with a Rating - and click Create.</span></span>

<span data-ttu-id="75825-139">[![criar um filme-Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="75825-139">[![Create a Movie - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span></span>

<span data-ttu-id="75825-140">Depois de clicar em criar, você será enviado para a página de índice em que o novo filme é listado com a nova coluna de classificação no banco de dados</span><span class="sxs-lookup"><span data-stu-id="75825-140">After you click Create, you're sent to the Index page where you new Movie is listed with the new Rating Column in the database</span></span>

<span data-ttu-id="75825-141">[Lista de filmes ![-Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="75825-141">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span></span>

<span data-ttu-id="75825-142">Este tutorial básico começou a tornar os controladores, associando-os a exibições e passando dados embutidos em código.</span><span class="sxs-lookup"><span data-stu-id="75825-142">This basic tutorial got you started making Controllers, associating them with Views and passing around hard-coded data.</span></span> <span data-ttu-id="75825-143">Em seguida, criamos e projetamos um banco de dados e colocamos algum dado nele.</span><span class="sxs-lookup"><span data-stu-id="75825-143">Then we created and designed a Database and put some data it in.</span></span> <span data-ttu-id="75825-144">Recuperamos os dados do banco e os exibimos em uma tabela HTML.</span><span class="sxs-lookup"><span data-stu-id="75825-144">We retrieved the data from the database and displayed it in an HTML table.</span></span> <span data-ttu-id="75825-145">Em seguida, adicionamos um formulário de criação que permite ao usuário adicionar dados ao próprio banco de dado de dentro do aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="75825-145">Then we added a Create form that let the user add data to the database themselves from within the Web Application.</span></span> <span data-ttu-id="75825-146">Adicionamos validação e, em seguida, fizemos a validação usando JavaScript no lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="75825-146">We added validation, then made the validation use JavaScript on the client-side.</span></span> <span data-ttu-id="75825-147">Por fim, alteramos o banco de dados para incluir uma nova coluna de data e, em seguida, atualizamos nossas duas páginas para criar e exibir esses novos dados.</span><span class="sxs-lookup"><span data-stu-id="75825-147">Finally, we changed the database to include a new column of data, then updated our two pages to create and display this new data.</span></span>

<span data-ttu-id="75825-148">Agora recomendo que você passe para nosso tutorial de nível intermediário "loja de[música MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)", bem como os vários vídeos e recursos em [https://asp.net/mvc](https://asp.net/mvc) para aprender ainda mais sobre o ASP.NET MVC!</span><span class="sxs-lookup"><span data-stu-id="75825-148">I now encourage you to move on to our intermediate-level tutorial "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" as well as the many videos and resources at [https://asp.net/mvc](https://asp.net/mvc) to learn even more about ASP.NET MVC!</span></span>

<span data-ttu-id="75825-149">Aproveite!</span><span class="sxs-lookup"><span data-stu-id="75825-149">Enjoy!</span></span>

- <span data-ttu-id="75825-150">Scott Hanselman- [http://hanselman.com](http://hanselman.com) e [@shanselman](http://twitter.com/shanselman) no Twitter.</span><span class="sxs-lookup"><span data-stu-id="75825-150">Scott Hanselman - [http://hanselman.com](http://hanselman.com) and [@shanselman](http://twitter.com/shanselman) on Twitter.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="75825-151">Anterior</span><span class="sxs-lookup"><span data-stu-id="75825-151">Previous</span></span>](getting-started-with-mvc-part7.md)
