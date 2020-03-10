---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: Criar um banco de dados | Microsoft Docs
author: microsoft
description: A etapa 2 mostra as etapas para criar o banco de dados que contém todo o jantar e o RSVP para nosso aplicativo NerdDinner.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: b0aa7c8cdf741f44e09ed18e2b2f73fe6bf786ae
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78581000"
---
# <a name="create-a-database"></a><span data-ttu-id="334c9-103">Criar um banco de dados</span><span class="sxs-lookup"><span data-stu-id="334c9-103">Create a Database</span></span>

<span data-ttu-id="334c9-104">pela [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="334c9-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="334c9-105">Baixar PDF</span><span class="sxs-lookup"><span data-stu-id="334c9-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="334c9-106">Esta é a etapa 2 de um [tutorial de aplicativo "NerdDinner"](introducing-the-nerddinner-tutorial.md) gratuito que percorre como criar um aplicativo Web pequeno, mas completo usando o ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="334c9-106">This is step 2 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="334c9-107">A etapa 2 mostra as etapas para criar o banco de dados que contém todo o jantar e o RSVP para nosso aplicativo NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="334c9-107">Step 2 shows the steps to create the database holding all of the dinner and RSVP data for our NerdDinner application.</span></span>
> 
> <span data-ttu-id="334c9-108">Se você estiver usando o ASP.NET MVC 3, recomendamos seguir as [introdução com](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) os tutoriais da [loja de música](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) MVC 3 ou MVC.</span><span class="sxs-lookup"><span data-stu-id="334c9-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>

## <a name="nerddinner-step-2-creating-the-database"></a><span data-ttu-id="334c9-109">Etapa 2 do NerdDinner: criando o banco de dados</span><span class="sxs-lookup"><span data-stu-id="334c9-109">NerdDinner Step 2: Creating the Database</span></span>

<span data-ttu-id="334c9-110">Vamos usar um banco de dados para armazenar todo o jantar e o RSVP para nosso aplicativo NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="334c9-110">We'll be using a database to store all of the Dinner and RSVP data for our NerdDinner application.</span></span>

<span data-ttu-id="334c9-111">As etapas a seguir mostram como criar o banco de dados usando a edição gratuita do SQL Server Express (que você pode facilmente instalar usando a v2 do [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)).</span><span class="sxs-lookup"><span data-stu-id="334c9-111">The steps below show creating the database using the free SQL Server Express edition (which you can easily install using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)).</span></span> <span data-ttu-id="334c9-112">Todo o código que escreveremos funcionará com SQL Server Express e o SQL Server completo.</span><span class="sxs-lookup"><span data-stu-id="334c9-112">All of the code we'll write works with both SQL Server Express and the full SQL Server.</span></span>

### <a name="creating-a-new-sql-server-express-database"></a><span data-ttu-id="334c9-113">Criando um novo banco de dados SQL Server Express</span><span class="sxs-lookup"><span data-stu-id="334c9-113">Creating a new SQL Server Express database</span></span>

<span data-ttu-id="334c9-114">Começaremos clicando com o botão direito do mouse em nosso projeto Web e selecionaremos o comando de menu **adicionar&gt;novo item** :</span><span class="sxs-lookup"><span data-stu-id="334c9-114">We'll begin by right-clicking on our web project, and then select the **Add-&gt;New Item** menu command:</span></span>

![](create-a-database/_static/image1.png)

<span data-ttu-id="334c9-115">Isso abrirá a caixa de diálogo "Adicionar novo item" do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="334c9-115">This will bring up Visual Studio's "Add New Item" dialog.</span></span> <span data-ttu-id="334c9-116">Vamos filtrar pela categoria "data" e selecionar o modelo de item "SQL Server banco de dados":</span><span class="sxs-lookup"><span data-stu-id="334c9-116">We'll filter by the "Data" category and select the "SQL Server Database" item template:</span></span>

![](create-a-database/_static/image2.png)

<span data-ttu-id="334c9-117">Vamos nomear o banco de dados SQL Server Express que queremos criar "NerdDinner. MDF" e clicar em OK.</span><span class="sxs-lookup"><span data-stu-id="334c9-117">We'll name the SQL Server Express database we want to create "NerdDinner.mdf" and hit ok.</span></span> <span data-ttu-id="334c9-118">O Visual Studio nos perguntará se queremos adicionar esse arquivo ao nosso diretório de dados de\_de \App (que é um diretório já configurado com ACLs de segurança de leitura e gravação):</span><span class="sxs-lookup"><span data-stu-id="334c9-118">Visual Studio will then ask us if we want to add this file to our \App\_Data directory (which is a directory already setup with both read and write security ACLs):</span></span>

![](create-a-database/_static/image3.png)

<span data-ttu-id="334c9-119">Clicaremos em "Sim" e nosso novo banco de dados será criado e adicionado ao nosso Gerenciador de Soluções:</span><span class="sxs-lookup"><span data-stu-id="334c9-119">We'll click "Yes" and our new database will be created and added to our Solution Explorer:</span></span>

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a><span data-ttu-id="334c9-120">Criando tabelas dentro de nosso banco de dados</span><span class="sxs-lookup"><span data-stu-id="334c9-120">Creating Tables within our Database</span></span>

<span data-ttu-id="334c9-121">Agora temos um novo banco de dados vazio.</span><span class="sxs-lookup"><span data-stu-id="334c9-121">We now have a new empty database.</span></span> <span data-ttu-id="334c9-122">Vamos adicionar algumas tabelas a ele.</span><span class="sxs-lookup"><span data-stu-id="334c9-122">Let's add some tables to it.</span></span>

<span data-ttu-id="334c9-123">Para fazer isso, navegaremos até a janela de guia "Gerenciador de Servidores" no Visual Studio, que nos permite gerenciar bancos de dados e servidores.</span><span class="sxs-lookup"><span data-stu-id="334c9-123">To do this we'll navigate to the "Server Explorer" tab window within Visual Studio, which enables us to manage databases and servers.</span></span> <span data-ttu-id="334c9-124">SQL Server Express bancos de dados armazenados na pasta \App\_data do nosso aplicativo aparecerão automaticamente dentro do Gerenciador de Servidores.</span><span class="sxs-lookup"><span data-stu-id="334c9-124">SQL Server Express databases stored in the \App\_Data folder of our application will automatically show up within the Server Explorer.</span></span> <span data-ttu-id="334c9-125">Opcionalmente, podemos usar o ícone "conectar ao banco de dados" na parte superior da janela "Gerenciador de Servidores" para adicionar mais bancos de dados SQL Server (locais e remotos) à lista também:</span><span class="sxs-lookup"><span data-stu-id="334c9-125">We can optionally use the "Connect to Database" icon on the top of the "Server Explorer" window to add additional SQL Server databases (both local and remote) to the list as well:</span></span>

![](create-a-database/_static/image5.png)

<span data-ttu-id="334c9-126">Adicionaremos duas tabelas ao nosso banco de dados NerdDinner – uma para armazenar nossos jantares e a outra para rastrear as aceitação de RSVP para eles.</span><span class="sxs-lookup"><span data-stu-id="334c9-126">We will add two tables to our NerdDinner database – one to store our Dinners, and the other to track RSVP acceptances to them.</span></span> <span data-ttu-id="334c9-127">Podemos criar novas tabelas clicando com o botão direito do mouse na pasta "tabelas" em nosso banco de dados e escolhendo o comando de menu "Adicionar nova tabela":</span><span class="sxs-lookup"><span data-stu-id="334c9-127">We can create new tables by right-clicking on the "Tables" folder within our database and choosing the "Add New Table" menu command:</span></span>

![](create-a-database/_static/image6.png)

<span data-ttu-id="334c9-128">Isso abrirá um designer de tabela que nos permite configurar o esquema da nossa tabela.</span><span class="sxs-lookup"><span data-stu-id="334c9-128">This will open up a table designer that allows us to configure the schema of our table.</span></span> <span data-ttu-id="334c9-129">Para a nossa tabela de "JANTARS", adicionaremos 10 colunas de dados:</span><span class="sxs-lookup"><span data-stu-id="334c9-129">For our "Dinners" table we will add 10 columns of data:</span></span>

![](create-a-database/_static/image7.png)

<span data-ttu-id="334c9-130">Queremos que a coluna "Jantarid" seja uma chave primária exclusiva para a tabela.</span><span class="sxs-lookup"><span data-stu-id="334c9-130">We want the "DinnerID" column to be a unique primary key for the table.</span></span> <span data-ttu-id="334c9-131">Podemos configurar isso clicando com o botão direito do mouse na coluna "Jantarid" e escolhendo o item de menu "definir chave primária":</span><span class="sxs-lookup"><span data-stu-id="334c9-131">We can configure this by right-clicking on the "DinnerID" column and choosing the "Set Primary Key" menu item:</span></span>

![](create-a-database/_static/image8.png)

<span data-ttu-id="334c9-132">Além de tornar o Jantarid uma chave primária, também queremos configurá-lo como uma coluna de "identidade" cujo valor seja incrementado automaticamente à medida que novas linhas de dados são adicionadas à tabela (o que significa que a primeira linha de jantar inserida terá um Jantarid de 1, a segunda linha inserida terá um Jantarid de 2, etc.).</span><span class="sxs-lookup"><span data-stu-id="334c9-132">In addition to making DinnerID a primary key, we also want configure it as an "identity" column whose value is automatically incremented as new rows of data are added to the table (meaning the first inserted Dinner row will have a DinnerID of 1, the second inserted row will have a DinnerID of 2, etc).</span></span>

<span data-ttu-id="334c9-133">Podemos fazer isso selecionando a coluna "Jantarid" e, em seguida, usar o editor "Propriedades da coluna" para definir a propriedade "(is Identity)" na coluna como "Yes".</span><span class="sxs-lookup"><span data-stu-id="334c9-133">We can do this by selecting the "DinnerID" column and then use the "Column Properties" editor to set the "(Is Identity)" property on the column to "Yes".</span></span> <span data-ttu-id="334c9-134">Usaremos os padrões de identidade padrão (comece com 1 e incrementos 1 em cada nova linha de jantar):</span><span class="sxs-lookup"><span data-stu-id="334c9-134">We will use the standard identity defaults (start at 1 and increment 1 on each new Dinner row):</span></span>

![](create-a-database/_static/image9.png)

<span data-ttu-id="334c9-135">Em seguida, salvaremos nossa tabela digitando Ctrl-S ou usando o comando **File-&gt;Save** menu.</span><span class="sxs-lookup"><span data-stu-id="334c9-135">We'll then save our table by typing Ctrl-S or by using the **File-&gt;Save** menu command.</span></span> <span data-ttu-id="334c9-136">Isso nos solicitará nomear a tabela.</span><span class="sxs-lookup"><span data-stu-id="334c9-136">This will prompt us to name the table.</span></span> <span data-ttu-id="334c9-137">Vamos nomeá-lo como "jantares":</span><span class="sxs-lookup"><span data-stu-id="334c9-137">We'll name it "Dinners":</span></span>

![](create-a-database/_static/image10.png)

<span data-ttu-id="334c9-138">Nossa nova tabela de jantares será exibida em nosso banco de dados no Gerenciador de servidores.</span><span class="sxs-lookup"><span data-stu-id="334c9-138">Our new Dinners table will then show up within our database in the server explorer.</span></span>

<span data-ttu-id="334c9-139">Em seguida, repetiremos as etapas acima e criaremos uma tabela "RSVP".</span><span class="sxs-lookup"><span data-stu-id="334c9-139">We'll then repeat the above steps and create a "RSVP" table.</span></span> <span data-ttu-id="334c9-140">Esta tabela com três colunas.</span><span class="sxs-lookup"><span data-stu-id="334c9-140">This table with have 3 columns.</span></span> <span data-ttu-id="334c9-141">Iremos configurar a coluna Rsvpid como a chave primária e também torná-la uma coluna de identidade:</span><span class="sxs-lookup"><span data-stu-id="334c9-141">We will setup the RsvpID column as the primary key, and also make it an identity column:</span></span>

![](create-a-database/_static/image11.png)

<span data-ttu-id="334c9-142">Vamos salvá-lo e dar a ele o nome "RSVP".</span><span class="sxs-lookup"><span data-stu-id="334c9-142">We'll save it and give it the name "RSVP".</span></span>

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a><span data-ttu-id="334c9-143">Configurando uma relação de chave estrangeira entre tabelas</span><span class="sxs-lookup"><span data-stu-id="334c9-143">Setting up a Foreign Key Relationship between Tables</span></span>

<span data-ttu-id="334c9-144">Agora temos duas tabelas em nosso banco de dados.</span><span class="sxs-lookup"><span data-stu-id="334c9-144">We now have two tables within our database.</span></span> <span data-ttu-id="334c9-145">Nossa última etapa de design de esquema será configurar uma relação de "um para muitos" entre essas duas tabelas – para que possamos associar cada linha de jantar com zero ou mais linhas RSVP que se aplicam a ela.</span><span class="sxs-lookup"><span data-stu-id="334c9-145">Our last schema design step will be to setup a "one-to-many" relationship between these two tables – so that we can associate each Dinner row with zero or more RSVP rows that apply to it.</span></span> <span data-ttu-id="334c9-146">Vamos fazer isso Configurando a coluna "Jantarid" da tabela RSVP para ter uma relação de chave estrangeira com a coluna "Jantarid" na tabela "jantares".</span><span class="sxs-lookup"><span data-stu-id="334c9-146">We will do this by configuring the RSVP table's "DinnerID" column to have a foreign-key relationship to the "DinnerID" column in the "Dinners" table.</span></span>

<span data-ttu-id="334c9-147">Para fazer isso, vamos abrir a tabela RSVP dentro do designer de tabela clicando duas vezes nela no Gerenciador de servidores.</span><span class="sxs-lookup"><span data-stu-id="334c9-147">To do this we'll open up the RSVP table within the table designer by double-clicking it in the server explorer.</span></span> <span data-ttu-id="334c9-148">Em seguida, selecionaremos a coluna "Jantarid" dentro dela, clicamos com o botão direito do mouse e escolhemos as opções "relações..." comando de menu de contexto:</span><span class="sxs-lookup"><span data-stu-id="334c9-148">We'll then select the "DinnerID" column within it, right-click, and choose the "Relationships…" context menu command:</span></span>

![](create-a-database/_static/image12.png)

<span data-ttu-id="334c9-149">Isso abrirá uma caixa de diálogo que podemos usar para configurar relações entre tabelas:</span><span class="sxs-lookup"><span data-stu-id="334c9-149">This will bring up a dialog that we can use to setup relationships between tables:</span></span>

![](create-a-database/_static/image13.png)

<span data-ttu-id="334c9-150">Clicaremos no botão "Adicionar" para adicionar uma nova relação à caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="334c9-150">We'll click the "Add" button to add a new relationship to the dialog.</span></span> <span data-ttu-id="334c9-151">Depois que uma relação tiver sido adicionada, expandiremos o nó "especificação de tabelas e colunas" da exibição de árvore na grade de propriedades à direita da caixa de diálogo e, em seguida, clicaremos no botão "..." botão à direita dele:</span><span class="sxs-lookup"><span data-stu-id="334c9-151">Once a relationship has been added, we'll expand the "Tables and Column Specification" tree-view node within the property grid to the right of the dialog, and then click the "…" button to the right of it:</span></span>

![](create-a-database/_static/image14.png)

<span data-ttu-id="334c9-152">Clicando no botão "..." abrirá outra caixa de diálogo que nos permite especificar quais tabelas e colunas estão envolvidas na relação, bem como nos permitir nomear a relação.</span><span class="sxs-lookup"><span data-stu-id="334c9-152">Clicking the "…" button will bring up another dialog that allows us to specify which tables and columns are involved in the relationship, as well as allow us to name the relationship.</span></span>

<span data-ttu-id="334c9-153">Alteraremos a tabela de chaves primárias para "jantares" e selecionaremos a coluna "Jantarid" na tabela de jantares como a chave primária.</span><span class="sxs-lookup"><span data-stu-id="334c9-153">We will change the Primary Key Table to be "Dinners", and select the "DinnerID" column within the Dinners table as the primary key.</span></span> <span data-ttu-id="334c9-154">Nossa tabela RSVP será a tabela de chave estrangeira e o RSVP. A coluna do jantarid será associada como chave estrangeira:</span><span class="sxs-lookup"><span data-stu-id="334c9-154">Our RSVP table will be the foreign-key table, and the RSVP.DinnerID column will be associated as the foreign-key:</span></span>

![](create-a-database/_static/image15.png)

<span data-ttu-id="334c9-155">Agora, cada linha na tabela RSVP será associada a uma linha na tabela de jantar.</span><span class="sxs-lookup"><span data-stu-id="334c9-155">Now each row in the RSVP table will be associated with a row in the Dinner table.</span></span> <span data-ttu-id="334c9-156">SQL Server manterá a integridade referencial para nós – e nos impedirá de adicionar uma nova linha RSVP se ela não apontar para uma linha de jantar válida.</span><span class="sxs-lookup"><span data-stu-id="334c9-156">SQL Server will maintain referential integrity for us – and prevent us from adding a new RSVP row if it does not point to a valid Dinner row.</span></span> <span data-ttu-id="334c9-157">Ele também nos impedirá de excluir uma linha de jantar se ainda houver linhas RSVP fazendo referência a ela.</span><span class="sxs-lookup"><span data-stu-id="334c9-157">It will also prevent us from deleting a Dinner row if there are still RSVP rows referring to it.</span></span>

### <a name="adding-data-to-our-tables"></a><span data-ttu-id="334c9-158">Adicionando dados a nossas tabelas</span><span class="sxs-lookup"><span data-stu-id="334c9-158">Adding Data to our Tables</span></span>

<span data-ttu-id="334c9-159">Vamos concluir adicionando alguns dados de exemplo à nossa tabela de jantares.</span><span class="sxs-lookup"><span data-stu-id="334c9-159">Let's finish by adding some sample data to our Dinners table.</span></span> <span data-ttu-id="334c9-160">Podemos adicionar dados a uma tabela clicando com o botão direito do mouse nele dentro do Gerenciador de Servidores e escolhendo o comando "mostrar dados da tabela":</span><span class="sxs-lookup"><span data-stu-id="334c9-160">We can add data to a table by right-clicking on it within the Server Explorer and choosing the "Show Table Data" command:</span></span>

![](create-a-database/_static/image16.png)

<span data-ttu-id="334c9-161">Adicionaremos algumas linhas de dados de jantar que podemos usar mais tarde, já que começamos a implementar o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="334c9-161">We'll add a few rows of Dinner data that we can use later as we start implementing the application:</span></span>

![](create-a-database/_static/image17.png)

### <a name="next-step"></a><span data-ttu-id="334c9-162">Próxima etapa</span><span class="sxs-lookup"><span data-stu-id="334c9-162">Next Step</span></span>

<span data-ttu-id="334c9-163">Terminamos de criar nosso banco de dados.</span><span class="sxs-lookup"><span data-stu-id="334c9-163">We've finished creating our database.</span></span> <span data-ttu-id="334c9-164">Agora, vamos criar classes de modelo que podemos usar para consultá-la e atualizá-la.</span><span class="sxs-lookup"><span data-stu-id="334c9-164">Let's now create model classes that we can use to query and update it.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="334c9-165">[Anterior](create-a-new-aspnet-mvc-project.md)
> [Próximo](build-a-model-with-business-rule-validations.md)</span><span class="sxs-lookup"><span data-stu-id="334c9-165">[Previous](create-a-new-aspnet-mvc-project.md)
[Next](build-a-model-with-business-rule-validations.md)</span></span>
