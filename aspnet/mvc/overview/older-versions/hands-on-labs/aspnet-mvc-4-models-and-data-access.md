---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: Modelos e acesso a dados do ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: 'Observação: este laboratório prático pressupõe que você tenha conhecimento básico do ASP.NET MVC. Se você não usou o ASP.NET MVC antes, recomendamos que você percorra o ASP.NET MVC 4...'
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: 90635b617930d0a9c126795f4c8790d542e33dc9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78560196"
---
# <a name="aspnet-mvc-4-models-and-data-access"></a><span data-ttu-id="0a3ae-104">Acesso a dados e modelos do ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="0a3ae-104">ASP.NET MVC 4 Models and Data Access</span></span>

<span data-ttu-id="0a3ae-105">por [equipe de acampamentos da Web](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="0a3ae-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="0a3ae-106">Baixe o kit de treinamento do Web acampamentos</span><span class="sxs-lookup"><span data-stu-id="0a3ae-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="0a3ae-107">Este laboratório prático pressupõe que você tenha conhecimento básico do **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-107">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="0a3ae-108">Se você não usou o **ASP.NET MVC** antes, recomendamos que você vá para o laboratório prático do **ASP.NET MVC 4 Fundamentals** .</span><span class="sxs-lookup"><span data-stu-id="0a3ae-108">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="0a3ae-109">Este laboratório orienta você pelos aprimoramentos e novos recursos descritos anteriormente, aplicando pequenas alterações a um aplicativo Web de exemplo fornecido na pasta de origem.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-109">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="0a3ae-110">Todos os códigos de exemplo e trechos de código estão incluídos no Web acampamentos Training Kit, disponível nas [versões Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="0a3ae-110">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="0a3ae-111">O projeto específico deste laboratório está disponível em [modelos do ASP.NET MVC 4 e acesso a dados](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).</span><span class="sxs-lookup"><span data-stu-id="0a3ae-111">The project specific to this lab is available at [ASP.NET MVC 4 Models and Data Access](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).</span></span>

<span data-ttu-id="0a3ae-112">No laboratório prático dos **conceitos básicos do ASP.NET MVC** , você passou a passar dados embutidos em código dos controladores para os modelos de exibição.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-112">In **ASP.NET MVC Fundamentals** Hands-on Lab, you have been passing hard-coded data from the Controllers to the View templates.</span></span> <span data-ttu-id="0a3ae-113">Mas, para criar um aplicativo Web real, talvez você queira usar um banco de dados real.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-113">But, in order to build a real Web application, you might want to use a real database.</span></span>

<span data-ttu-id="0a3ae-114">Este laboratório prático mostrará como usar um mecanismo de banco de dados para armazenar e recuperar os dados necessários para o aplicativo de loja de música.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-114">This Hands-on Lab will show you how to use a database engine in order to store and retrieve the data needed for the Music Store application.</span></span> <span data-ttu-id="0a3ae-115">Para fazer isso, você começará com um banco de dados existente e criará o Modelo de Dados de Entidade a partir dele.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-115">To accomplish that, you will start with an existing database and create the Entity Data Model from it.</span></span> <span data-ttu-id="0a3ae-116">Ao longo deste laboratório, você encontrará a abordagem de **Database First** , bem como a abordagem de **Code First** .</span><span class="sxs-lookup"><span data-stu-id="0a3ae-116">Throughout this lab, you will meet the **Database First** approach as well as the **Code First** approach.</span></span>

<span data-ttu-id="0a3ae-117">No entanto, você também pode usar a abordagem **Model First** , criar o mesmo modelo usando as ferramentas e, em seguida, gerar o banco de dados a partir dele.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-117">However, you can also use the **Model First** approach, create the same model using the tools, and then generate the database from it.</span></span>

<span data-ttu-id="0a3ae-118">![Database First versus Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First versus Model First")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-118">![Database First vs. Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First vs. Model First")</span></span>

<span data-ttu-id="0a3ae-119">*Database First versus Model First*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-119">*Database First vs. Model First*</span></span>

<span data-ttu-id="0a3ae-120">Depois de gerar o modelo, você fará os ajustes adequados no StoreController para fornecer as exibições de armazenamento com os dados extraídos do banco de dados, em vez de usar os dado embutido em código.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-120">After generating the Model, you will make the proper adjustments in the StoreController to provide the Store Views with the data taken from the database, instead of using hard-coded data.</span></span> <span data-ttu-id="0a3ae-121">Você não precisará fazer nenhuma alteração nos modelos de exibição porque o StoreController retornará os mesmos ViewModels para os modelos de exibição, embora desta vez os dados sejam provenientes do banco de dado.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-121">You will not need to make any change to the View templates because the StoreController will be returning the same ViewModels to the View templates, although this time the data will come from the database.</span></span>

<span data-ttu-id="0a3ae-122">**A abordagem de Code First**</span><span class="sxs-lookup"><span data-stu-id="0a3ae-122">**The Code First Approach**</span></span>

<span data-ttu-id="0a3ae-123">A abordagem Code First nos permite definir o modelo a partir do código sem gerar classes que geralmente são acopladas à estrutura.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-123">The Code First approach allows us to define the model from the code without generating classes that are generally coupled with the framework.</span></span>

<span data-ttu-id="0a3ae-124">No Code First, os objetos de modelo são definidos com POCOs, &quot;objetos CLR antigos simples&quot;.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-124">In code first, model objects are defined with POCOs, &quot;Plain Old CLR Objects&quot;.</span></span> <span data-ttu-id="0a3ae-125">POCOs são classes simples, sem herança, e não implementam interfaces.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-125">POCOs are simple plain classes that have no inheritance and do not implement interfaces.</span></span> <span data-ttu-id="0a3ae-126">Podemos gerar automaticamente o banco de dados a partir deles ou podemos usar um banco de dados existente e gerar o mapeamento de classe do código.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-126">We can automatically generate the database from them, or we can use an existing database and generate the class mapping from the code.</span></span>

<span data-ttu-id="0a3ae-127">Os benefícios de usar essa abordagem é que o modelo permanece independente da estrutura de persistência (nesse caso, Entity Framework), já que as classes POCOs não são acopladas à estrutura de mapeamento.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-127">The benefits of using this approach is that the Model remains independent from the persistence framework (in this case, Entity Framework), as the POCOs classes are not coupled with the mapping framework.</span></span>

> [!NOTE]
> <span data-ttu-id="0a3ae-128">Este laboratório se baseia no ASP.NET MVC 4 e em uma versão do aplicativo de exemplo da loja de música personalizada e minimizada para se ajustar apenas aos recursos mostrados neste laboratório prático.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-128">This Lab is based on ASP.NET MVC 4 and a version of the Music Store sample application customized and minimized to fit only the features shown in this Hands-On Lab.</span></span>
> 
> <span data-ttu-id="0a3ae-129">Se você quiser explorar o aplicativo de tutorial da **loja de música** inteiro, poderá encontrá-lo no [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="0a3ae-129">If you wish to explore the whole **Music Store** tutorial application you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="0a3ae-130">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="0a3ae-130">Prerequisites</span></span>

<span data-ttu-id="0a3ae-131">Você deve ter os seguintes itens para concluir este laboratório:</span><span class="sxs-lookup"><span data-stu-id="0a3ae-131">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="0a3ae-132">[Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou superior (leia [o apêndice a](#AppendixA) para obter instruções sobre como instalá-lo).</span><span class="sxs-lookup"><span data-stu-id="0a3ae-132">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="0a3ae-133">Instalação</span><span class="sxs-lookup"><span data-stu-id="0a3ae-133">Setup</span></span>

<span data-ttu-id="0a3ae-134">**Instalando trechos de código**</span><span class="sxs-lookup"><span data-stu-id="0a3ae-134">**Installing Code Snippets**</span></span>

<span data-ttu-id="0a3ae-135">Para sua conveniência, grande parte do código que você gerenciará ao longo deste laboratório está disponível como trechos de código do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-135">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="0a3ae-136">Para instalar os trechos de código, execute o arquivo **.\Source\Setup\CodeSnippets.VSI** .</span><span class="sxs-lookup"><span data-stu-id="0a3ae-136">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="0a3ae-137">Se você não estiver familiarizado com os trechos de Visual Studio Code e quiser saber como usá-los, consulte o apêndice deste documento &quot;[Apêndice C: usando trechos de código](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-137">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="0a3ae-138">Exercícios</span><span class="sxs-lookup"><span data-stu-id="0a3ae-138">Exercises</span></span>

<span data-ttu-id="0a3ae-139">Este laboratório prático é composto pelos seguintes exercícios:</span><span class="sxs-lookup"><span data-stu-id="0a3ae-139">This Hands-on Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="0a3ae-140">Exercício 1: adicionando um banco de dados</span><span class="sxs-lookup"><span data-stu-id="0a3ae-140">Exercise 1: Adding a Database</span></span>](#Exercise1)
2. [<span data-ttu-id="0a3ae-141">Exercício 2: Criando um banco de dados usando Code First</span><span class="sxs-lookup"><span data-stu-id="0a3ae-141">Exercise 2: Creating a Database using Code First</span></span>](#Exercise2)
3. [<span data-ttu-id="0a3ae-142">Exercício 3: consultando o banco de dados com parâmetros</span><span class="sxs-lookup"><span data-stu-id="0a3ae-142">Exercise 3: Querying the Database with Parameters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="0a3ae-143">Cada exercício é acompanhado por uma pasta **final** que contém a solução resultante que você deve obter depois de concluir os exercícios.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-143">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="0a3ae-144">Você pode usar essa solução como um guia se precisar de ajuda adicional para trabalhar com os exercícios.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-144">You can use this solution as a guide if you need additional help working through the exercises.</span></span>

<span data-ttu-id="0a3ae-145">Tempo estimado para concluir este laboratório: **35 minutos**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-145">Estimated time to complete this lab: **35 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a><span data-ttu-id="0a3ae-146">Exercício 1: adicionando um banco de dados</span><span class="sxs-lookup"><span data-stu-id="0a3ae-146">Exercise 1: Adding a Database</span></span>

<span data-ttu-id="0a3ae-147">Neste exercício, você aprenderá a adicionar um banco de dados com as tabelas do aplicativo MusicStore à solução a fim de consumir os mesmos.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-147">In this exercise, you will learn how to add a database with the tables of the MusicStore application to the solution in order to consume its data.</span></span> <span data-ttu-id="0a3ae-148">Depois que o banco de dados for gerado com o modelo e adicionado à solução, você modificará a classe StoreController para fornecer o modelo de exibição com os dados extraídos do banco de dado, em vez de usar valores embutidos em código.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-148">Once the database is generated with the model, and added to the solution, you will modify the StoreController class to provide the View template with the data taken from the database, instead of using hard-coded values.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a><span data-ttu-id="0a3ae-149">Tarefa 1-adicionando um banco de dados</span><span class="sxs-lookup"><span data-stu-id="0a3ae-149">Task 1 - Adding a Database</span></span>

<span data-ttu-id="0a3ae-150">Nesta tarefa, você adicionará um banco de dados já criado com as tabelas principais do aplicativo MusicStore à solução.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-150">In this task, you will add an already created database with the main tables of the MusicStore application to the solution.</span></span>

1. <span data-ttu-id="0a3ae-151">Abra a solução **inicial** localizada na **origem/EX1-AddingADatabaseDBFirst/início/** pasta.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-151">Open the **Begin** solution located at **Source/Ex1-AddingADatabaseDBFirst/Begin/** folder.</span></span>

   1. <span data-ttu-id="0a3ae-152">Será necessário baixar alguns pacotes NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-152">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="0a3ae-153">Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-153">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="0a3ae-154">Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-154">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="0a3ae-155">Por fim, Compile a solução clicando em **build** | **Compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-155">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="0a3ae-156">Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-156">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="0a3ae-157">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-157">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="0a3ae-158">É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-158">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="0a3ae-159">Adicionar arquivo de banco de dados **MvcMusicStore** .</span><span class="sxs-lookup"><span data-stu-id="0a3ae-159">Add **MvcMusicStore** database file.</span></span> <span data-ttu-id="0a3ae-160">Neste laboratório prático, você usará um banco de dados já criado chamado **MvcMusicStore. MDF**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-160">In this Hands-on Lab, you will use an already created database called **MvcMusicStore.mdf**.</span></span> <span data-ttu-id="0a3ae-161">Para fazer isso, clique com o botão direito do mouse em **App\_data** Folder, aponte para **Adicionar** e clique em **Item existente**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-161">To do that, right-click **App\_Data** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="0a3ae-162">Navegue até **\Source\Assets** e selecione o arquivo **MvcMusicStore. MDF** .</span><span class="sxs-lookup"><span data-stu-id="0a3ae-162">Browse to **\Source\Assets** and select the **MvcMusicStore.mdf** file.</span></span>

    <span data-ttu-id="0a3ae-163">![Adicionando um item existente](aspnet-mvc-4-models-and-data-access/_static/image2.png "Adicionando um item existente")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-163">![Adding an Existing Item](aspnet-mvc-4-models-and-data-access/_static/image2.png "Adding an Existing Item")</span></span>

    <span data-ttu-id="0a3ae-164">*Adicionando um item existente*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-164">*Adding an Existing Item*</span></span>

    <span data-ttu-id="0a3ae-165">![Arquivo de banco de dados MvcMusicStore. MDF](aspnet-mvc-4-models-and-data-access/_static/image3.png "Arquivo de banco de dados MvcMusicStore. MDF")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-165">![MvcMusicStore.mdf database file](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf database file")</span></span>

    <span data-ttu-id="0a3ae-166">*Arquivo de banco de dados MvcMusicStore. MDF*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-166">*MvcMusicStore.mdf database file*</span></span>

    <span data-ttu-id="0a3ae-167">O banco de dados foi adicionado ao projeto.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-167">The database has been added to the project.</span></span> <span data-ttu-id="0a3ae-168">Mesmo quando o banco de dados está localizado dentro da solução, você pode consultá-lo e atualizá-lo como hospedado em um servidor de banco de dados diferente.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-168">Even when the database is located inside the solution, you can query and update it as it was hosted in a different database server.</span></span>

    <span data-ttu-id="0a3ae-169">![Banco de dados MvcMusicStore no Gerenciador de Soluções](aspnet-mvc-4-models-and-data-access/_static/image4.png "Banco de dados MvcMusicStore no Gerenciador de Soluções")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-169">![MvcMusicStore database in Solution Explorer](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore database in Solution Explorer")</span></span>

    <span data-ttu-id="0a3ae-170">*Banco de dados MvcMusicStore no Gerenciador de Soluções*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-170">*MvcMusicStore database in Solution Explorer*</span></span>
3. <span data-ttu-id="0a3ae-171">Verifique a conexão com o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-171">Verify the connection to the database.</span></span> <span data-ttu-id="0a3ae-172">Para fazer isso, clique duas vezes em **MvcMusicStore. MDF** para estabelecer uma conexão.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-172">To do this, double-click **MvcMusicStore.mdf** to establish a connection.</span></span>

    <span data-ttu-id="0a3ae-173">![Conectando-se ao MvcMusicStore. MDF](aspnet-mvc-4-models-and-data-access/_static/image5.png "Conectando-se ao MvcMusicStore. MDF")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-173">![Connecting to MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "Connecting to MvcMusicStore.mdf")</span></span>

    <span data-ttu-id="0a3ae-174">*Conectando-se ao MvcMusicStore. MDF*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-174">*Connecting to MvcMusicStore.mdf*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a><span data-ttu-id="0a3ae-175">Tarefa 2-Criando um modelo de dados</span><span class="sxs-lookup"><span data-stu-id="0a3ae-175">Task 2 - Creating a Data Model</span></span>

<span data-ttu-id="0a3ae-176">Nesta tarefa, você criará um modelo de dados para interagir com o banco de dados adicionado na tarefa anterior.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-176">In this task, you will create a data model to interact with the database added in the previous task.</span></span>

1. <span data-ttu-id="0a3ae-177">Crie um modelo de dados que representará o banco de dado.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-177">Create a data model that will represent the database.</span></span> <span data-ttu-id="0a3ae-178">Para fazer isso, em Gerenciador de Soluções clique com o botão direito do mouse na pasta **modelos** , aponte para **Adicionar** e clique em **novo item**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-178">To do this, in Solution Explorer right-click the **Models** folder, point to **Add** and then click **New Item**.</span></span> <span data-ttu-id="0a3ae-179">Na caixa de diálogo **Adicionar novo item** , selecione o modelo de **dados** e, em seguida, **ADO.NET modelo de dados de entidade** item.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-179">In the **Add New Item** dialog, select the **Data** template and then the **ADO.NET Entity Data Model** item.</span></span> <span data-ttu-id="0a3ae-180">Altere o nome do modelo de dados para **StoreDB. edmx** e clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-180">Change the data model name to **StoreDB.edmx** and click **Add**.</span></span>

    <span data-ttu-id="0a3ae-181">![Adicionando StoreDB ADO.NET Modelo de Dados de Entidade](aspnet-mvc-4-models-and-data-access/_static/image6.png "Adicionando StoreDB ADO.NET Modelo de Dados de Entidade")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-181">![Adding the StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "Adding the StoreDB ADO.NET Entity Data Model")</span></span>

    <span data-ttu-id="0a3ae-182">*Adicionando StoreDB ADO.NET Modelo de Dados de Entidade*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-182">*Adding the StoreDB ADO.NET Entity Data Model*</span></span>
2. <span data-ttu-id="0a3ae-183">O **Assistente de modelo de dados de entidade** será exibido.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-183">The **Entity Data Model Wizard** will appear.</span></span> <span data-ttu-id="0a3ae-184">Este assistente o guiará pela criação da camada do modelo.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-184">This wizard will guide you through the creation of the model layer.</span></span> <span data-ttu-id="0a3ae-185">Como o modelo deve ser criado com base no banco de dados existente adicionado recentemente, selecione **gerar do banco de dados** e clique em **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-185">Since the model should be created based on the existing database recently added, select **Generate from database** and click **Next**.</span></span>

    <span data-ttu-id="0a3ae-186">![Escolhendo o conteúdo do modelo](aspnet-mvc-4-models-and-data-access/_static/image7.png "Escolhendo o conteúdo do modelo")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-186">![Choosing the model content](aspnet-mvc-4-models-and-data-access/_static/image7.png "Choosing the model content")</span></span>

    <span data-ttu-id="0a3ae-187">*Escolhendo o conteúdo do modelo*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-187">*Choosing the model content*</span></span>
3. <span data-ttu-id="0a3ae-188">Como você está gerando um modelo de um banco de dados, será necessário especificar a conexão a ser usada.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-188">Since you are generating a model from a database, you will need to specify the connection to use.</span></span> <span data-ttu-id="0a3ae-189">Clique em **nova conexão**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-189">Click **New Connection**.</span></span>
4. <span data-ttu-id="0a3ae-190">Selecione **Microsoft SQL Server arquivo de banco de dados** e clique em **continuar**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-190">Select **Microsoft SQL Server Database File** and click **Continue**.</span></span>

    <span data-ttu-id="0a3ae-191">![Escolher fonte de dados](aspnet-mvc-4-models-and-data-access/_static/image8.png "Escolher fonte de dados")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-191">![Choose data source](aspnet-mvc-4-models-and-data-access/_static/image8.png "Choose data source")</span></span>

    <span data-ttu-id="0a3ae-192">*Caixa de diálogo escolher fonte de dados*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-192">*Choose data source dialog*</span></span>
5. <span data-ttu-id="0a3ae-193">Clique em **procurar** e selecione o banco de dados **MvcMusicStore. MDF** que você localizou na pasta **Data\_de aplicativos** e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-193">Click **Browse** and select the database **MvcMusicStore.mdf** you located in the **App\_Data** folder and click **OK**.</span></span>

    <span data-ttu-id="0a3ae-194">![Propriedades da conexão](aspnet-mvc-4-models-and-data-access/_static/image9.png "Propriedades da conexão")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-194">![Connection properties](aspnet-mvc-4-models-and-data-access/_static/image9.png "Connection properties")</span></span>

    <span data-ttu-id="0a3ae-195">*Propriedades da conexão*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-195">*Connection properties*</span></span>
6. <span data-ttu-id="0a3ae-196">A classe gerada deve ter o mesmo nome que a cadeia de conexão de entidade, portanto, altere seu nome para **MusicStoreEntities** e clique em **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-196">The generated class should have the same name as the entity connection string, so change its name to **MusicStoreEntities** and click **Next**.</span></span>

    <span data-ttu-id="0a3ae-197">![Escolhendo a conexão de dados](aspnet-mvc-4-models-and-data-access/_static/image10.png "Escolhendo a conexão de dados")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-197">![Choosing the data connection](aspnet-mvc-4-models-and-data-access/_static/image10.png "Choosing the data connection")</span></span>

    <span data-ttu-id="0a3ae-198">*Escolhendo a conexão de dados*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-198">*Choosing the data connection*</span></span>
7. <span data-ttu-id="0a3ae-199">Escolha os objetos de banco de dados a serem usados.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-199">Choose the database objects to use.</span></span> <span data-ttu-id="0a3ae-200">Como o modelo de entidade usará apenas as tabelas do banco de dados, selecione a opção **tabelas** e verifique se as opções **incluir colunas de chave estrangeira no modelo** e **Pluralize ou permitir nomes de objeto gerados** também estão selecionadas.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-200">As the Entity Model will use just the database's tables, select the **Tables** option, and make sure that the **Include foreign key columns in the model** and **Pluralize or singularize generated object names** options are also selected.</span></span> <span data-ttu-id="0a3ae-201">Altere o namespace do modelo para **MvcMusicStore. Model** e clique em **concluir**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-201">Change the Model Namespace to **MvcMusicStore.Model** and click **Finish**.</span></span>

    <span data-ttu-id="0a3ae-202">![Escolhendo os objetos de banco de dados](aspnet-mvc-4-models-and-data-access/_static/image11.png "Escolhendo os objetos de banco de dados")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-202">![Choosing the database objects](aspnet-mvc-4-models-and-data-access/_static/image11.png "Choosing the database objects")</span></span>

    <span data-ttu-id="0a3ae-203">*Escolhendo os objetos de banco de dados*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-203">*Choosing the database objects*</span></span>

    > [!NOTE]
    > <span data-ttu-id="0a3ae-204">Se uma caixa de diálogo de aviso de segurança for exibida, clique em **OK** para executar o modelo e gerar as classes para as entidades de modelo.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-204">If a Security Warning dialog is shown, click **OK** to run the template and generate the classes for the model entities.</span></span>
8. <span data-ttu-id="0a3ae-205">Um diagrama de entidade para o banco de dados será exibido, enquanto uma classe separada que mapeia cada tabela para o banco de dados será criada.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-205">An entity diagram for the database will appear, while a separate class that maps each table to the database will be created.</span></span> <span data-ttu-id="0a3ae-206">Por exemplo, a tabela **álbuns** será representada por uma classe de **álbum** , em que cada coluna na tabela será mapeada para uma propriedade de classe.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-206">For example, the **Albums** table will be represented by an **Album** class, where each column in the table will map to a class property.</span></span> <span data-ttu-id="0a3ae-207">Isso permitirá que você consulte e trabalhe com objetos que representam linhas no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-207">This will allow you to query and work with objects that represent rows in the database.</span></span>

    <span data-ttu-id="0a3ae-208">![Diagrama de entidade](aspnet-mvc-4-models-and-data-access/_static/image12.png "Diagrama de entidade")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-208">![Entity diagram](aspnet-mvc-4-models-and-data-access/_static/image12.png "Entity diagram")</span></span>

    <span data-ttu-id="0a3ae-209">*Diagrama de entidade*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-209">*Entity diagram*</span></span>

    > [!NOTE]
    > <span data-ttu-id="0a3ae-210">Os modelos T4 (. TT) executam o código para gerar as classes de entidades e substituirão as classes existentes com o mesmo nome.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-210">The T4 templates (.tt) run code to generate the entities classes and will overwrite the existing classes with the same name.</span></span> <span data-ttu-id="0a3ae-211">Neste exemplo, as classes &quot;álbum&quot;, &quot;gênero&quot; e &quot;artista&quot; foram substituídas pelo código gerado.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-211">In this example, the classes &quot;Album&quot;, &quot;Genre&quot; and &quot;Artist&quot; were overwritten with the generated code.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a><span data-ttu-id="0a3ae-212">Tarefa 3-criando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="0a3ae-212">Task 3 - Building the Application</span></span>

<span data-ttu-id="0a3ae-213">Nesta tarefa, você verificará que, embora a geração de modelo tenha removido as classes de modelo de **álbum**, **gênero** e **artista** , o projeto é compilado com êxito usando as novas classes de modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-213">In this task, you will check that, although the model generation have removed the **Album**, **Genre** and **Artist** model classes, the project builds successfully by using the new data model classes.</span></span>

1. <span data-ttu-id="0a3ae-214">Crie o projeto selecionando o item de menu **Build** e, em seguida, **Build MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-214">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="0a3ae-215">![Compilando o projeto](aspnet-mvc-4-models-and-data-access/_static/image13.png "Compilando o projeto")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-215">![Building the project](aspnet-mvc-4-models-and-data-access/_static/image13.png "Building the project")</span></span>

    <span data-ttu-id="0a3ae-216">*Compilando o projeto*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-216">*Building the project*</span></span>
2. <span data-ttu-id="0a3ae-217">O projeto é compilado com êxito.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-217">The project builds successfully.</span></span> <span data-ttu-id="0a3ae-218">Por que ele ainda funciona?</span><span class="sxs-lookup"><span data-stu-id="0a3ae-218">Why does it still work?</span></span> <span data-ttu-id="0a3ae-219">Ele funciona porque as tabelas de banco de dados têm campos que incluem as propriedades que você estava usando no **álbum** e no **gênero**das classes removidas.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-219">It works because the database tables have fields that include the properties that you were using in the removed classes **Album** and **Genre**.</span></span>

    <span data-ttu-id="0a3ae-220">![Compilações com êxito](aspnet-mvc-4-models-and-data-access/_static/image14.png "Compilações com êxito")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-220">![Builds succeeded](aspnet-mvc-4-models-and-data-access/_static/image14.png "Builds succeeded")</span></span>

    <span data-ttu-id="0a3ae-221">*Compilações com êxito*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-221">*Builds succeeded*</span></span>
3. <span data-ttu-id="0a3ae-222">Embora o designer exiba as entidades em um formato de diagrama, elas são C# realmente classes.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-222">While the designer displays the entities in a diagram format, they are really C# classes.</span></span> <span data-ttu-id="0a3ae-223">Expanda o nó **StoreDB. edmx** no Gerenciador de soluções e, em seguida, **StoreDB.tt**, você verá as novas entidades geradas.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-223">Expand the **StoreDB.edmx** node in the Solution Explorer and then **StoreDB.tt**, you will see the new generated entities.</span></span>

    <span data-ttu-id="0a3ae-224">![Arquivos gerados](aspnet-mvc-4-models-and-data-access/_static/image15.png "Arquivos gerados")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-224">![Generated files](aspnet-mvc-4-models-and-data-access/_static/image15.png "Generated files")</span></span>

    <span data-ttu-id="0a3ae-225">*Arquivos gerados*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-225">*Generated files*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="0a3ae-226">Tarefa 4-consultando o banco de dados</span><span class="sxs-lookup"><span data-stu-id="0a3ae-226">Task 4 - Querying the Database</span></span>

<span data-ttu-id="0a3ae-227">Nesta tarefa, você atualizará a classe StoreController para que, em vez de usar dados codificados, ele consultará o banco de dado para recuperar as informações.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-227">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will query the database to retrieve the information.</span></span>

1. <span data-ttu-id="0a3ae-228">Abra **Controllers\StoreController.cs** e adicione o seguinte campo à classe para manter uma instância da classe **MusicStoreEntities** , chamada **storeDB**:</span><span class="sxs-lookup"><span data-stu-id="0a3ae-228">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="0a3ae-229">(Trecho de código- *modelos e acesso a dados-EX1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="0a3ae-229">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
2. <span data-ttu-id="0a3ae-230">A classe **MusicStoreEntities** expõe uma propriedade de coleção para cada tabela no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-230">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="0a3ae-231">Atualize o método de ação de **procura** para recuperar um gênero com todos os **álbuns**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-231">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="0a3ae-232">(Trecho de código- *modelos e acesso a dados-navegação do EX1 Store*)</span><span class="sxs-lookup"><span data-stu-id="0a3ae-232">(Code Snippet - *Models And Data Access - Ex1 Store Browse*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

    > [!NOTE]
    > <span data-ttu-id="0a3ae-233">Você está usando uma funcionalidade do .NET chamada **LINQ** (consulta integrada à linguagem) para gravar expressões de consulta fortemente tipadas em relação a essas coleções, o que executará o código no banco de dados e retornará objetos com os quais você pode programar.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-233">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
    > 
    > <span data-ttu-id="0a3ae-234">Para obter mais informações sobre o LINQ, visite o [site do MSDN](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="0a3ae-234">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span></span>
3. <span data-ttu-id="0a3ae-235">Atualizar o método de ação de **índice** para recuperar todos os gêneros.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-235">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="0a3ae-236">(Trecho de código- *modelos e acesso a dados-índice de armazenamento EX1*)</span><span class="sxs-lookup"><span data-stu-id="0a3ae-236">(Code Snippet - *Models And Data Access - Ex1 Store Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
4. <span data-ttu-id="0a3ae-237">Atualizar o método de ação de **índice** para recuperar todos os gêneros e transformar a coleção em uma lista.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-237">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="0a3ae-238">(Trecho de código- *modelos e acesso a dados-EX1 Store GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="0a3ae-238">(Code Snippet - *Models And Data Access - Ex1 Store GenreMenu*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="0a3ae-239">Tarefa 5-executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="0a3ae-239">Task 5 - Running the Application</span></span>

<span data-ttu-id="0a3ae-240">Nesta tarefa, você verificará se a página de índice da loja agora exibirá os gêneros armazenados no banco de dados em vez dos codificados.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-240">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="0a3ae-241">Não é necessário alterar o modelo de exibição porque o **StoreController** está retornando as mesmas entidades que antes, embora desta vez os dados serão provenientes do banco de dado.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-241">There is no need to change the View template because the **StoreController** is returning the same entities as before, although this time the data will come from the database.</span></span>

1. <span data-ttu-id="0a3ae-242">Recompile a solução e pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-242">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="0a3ae-243">O projeto é iniciado na Home Page.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-243">The project starts in the Home page.</span></span> <span data-ttu-id="0a3ae-244">Verifique se o menu de **gêneros** não é mais uma lista embutida em código e se os dados são recuperados diretamente do banco de dado.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-244">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    <span data-ttu-id="0a3ae-246">*Pesquisando gêneros do banco de dados*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-246">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="0a3ae-247">Agora, navegue até qualquer gênero e verifique se os álbuns estão populados do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-247">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="0a3ae-248">![Pesquisando álbuns do banco de dados](aspnet-mvc-4-models-and-data-access/_static/image17.png "Pesquisando álbuns do banco de dados")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-248">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image17.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="0a3ae-249">*Pesquisando álbuns do banco de dados*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-249">*Browsing Albums from the database*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a><span data-ttu-id="0a3ae-250">Exercício 2: Criando um banco de dados usando Code First</span><span class="sxs-lookup"><span data-stu-id="0a3ae-250">Exercise 2: Creating a Database Using Code First</span></span>

<span data-ttu-id="0a3ae-251">Neste exercício, você aprenderá a usar a abordagem de Code First para criar um banco de dados com as tabelas do aplicativo MusicStore e como acessar seus dados.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-251">In this exercise, you will learn how to use the Code First approach to create a database with the tables of the MusicStore application, and how to access its data.</span></span>

<span data-ttu-id="0a3ae-252">Depois que o modelo for gerado, você modificará o StoreController para fornecer o modelo de exibição com os dados extraídos do banco de dado, em vez de usar valores codificados.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-252">Once the model is generated, you will modify the StoreController to provide the View template with the data taken from the database, instead of using hardcoded values.</span></span>

> [!NOTE]
> <span data-ttu-id="0a3ae-253">Se você concluiu o exercício 1 e já trabalhou com a abordagem de Database First, agora você aprenderá a obter os mesmos resultados com um processo diferente.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-253">If you have completed Exercise 1 and have already worked with the Database First approach, you will now learn how to get the same results with a different process.</span></span> <span data-ttu-id="0a3ae-254">As tarefas que estão em comum com o exercício 1 foram marcadas para facilitar sua leitura.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-254">The tasks that are in common with Exercise 1 have been marked to make your reading easier.</span></span> <span data-ttu-id="0a3ae-255">Se você não concluiu o exercício 1, mas gostaria de aprender a abordagem Code First, poderá iniciar neste exercício e obter uma cobertura completa do tópico.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-255">If you have not completed Exercise 1 but would like to learn the Code First approach, you can start from this exercise and get a full coverage of the topic.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a><span data-ttu-id="0a3ae-256">Tarefa 1-preenchendo dados de exemplo</span><span class="sxs-lookup"><span data-stu-id="0a3ae-256">Task 1 - Populating Sample Data</span></span>

<span data-ttu-id="0a3ae-257">Nesta tarefa, você preencherá o banco de dados com exemplos de dado quando ele for inicialmente criado usando Code-First.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-257">In this task, you will populate the database with sample data when it is initially created using Code-First.</span></span>

1. <span data-ttu-id="0a3ae-258">Abra a solução **inicial** localizada na **origem/EX2-CreatingADatabaseCodeFirst/início/** pasta.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-258">Open the **Begin** solution located at **Source/Ex2-CreatingADatabaseCodeFirst/Begin/** folder.</span></span> <span data-ttu-id="0a3ae-259">Caso contrário, você pode continuar usando a solução **final** obtida concluindo o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-259">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="0a3ae-260">Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-260">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="0a3ae-261">Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-261">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="0a3ae-262">Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-262">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="0a3ae-263">Por fim, Compile a solução clicando em **build** | **Compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-263">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="0a3ae-264">Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-264">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="0a3ae-265">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-265">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="0a3ae-266">É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-266">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="0a3ae-267">Adicione o arquivo **SampleData.cs** à pasta **modelos** .</span><span class="sxs-lookup"><span data-stu-id="0a3ae-267">Add the **SampleData.cs** file to the **Models** folder.</span></span> <span data-ttu-id="0a3ae-268">Para fazer isso, clique com o botão direito do mouse na pasta **modelos** , aponte para **Adicionar** e clique em **Item existente**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-268">To do that, right-click **Models** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="0a3ae-269">Navegue até **\Source\Assets** e selecione o arquivo **SampleData.cs** .</span><span class="sxs-lookup"><span data-stu-id="0a3ae-269">Browse to **\Source\Assets** and select the **SampleData.cs** file.</span></span>

    <span data-ttu-id="0a3ae-270">![Código de preenchimento de dados de exemplo](aspnet-mvc-4-models-and-data-access/_static/image18.png "Código de preenchimento de dados de exemplo")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-270">![Sample data populate code](aspnet-mvc-4-models-and-data-access/_static/image18.png "Sample data populate code")</span></span>

    <span data-ttu-id="0a3ae-271">*Código de preenchimento de dados de exemplo*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-271">*Sample data populate code*</span></span>
3. <span data-ttu-id="0a3ae-272">Abra o arquivo **global.asax.cs** e adicione as instruções *using* a seguir.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-272">Open the **Global.asax.cs** file and add the following *using* statements.</span></span>

    <span data-ttu-id="0a3ae-273">(Trecho de código- *modelos e acesso a dados-EX2 global asax usa*)</span><span class="sxs-lookup"><span data-stu-id="0a3ae-273">(Code Snippet - *Models And Data Access - Ex2 Global Asax Usings*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
4. <span data-ttu-id="0a3ae-274">No **aplicativo\_método Start ()** , adicione a linha a seguir para definir o inicializador de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-274">In the **Application\_Start()** method add the following line to set the database initializer.</span></span>

    <span data-ttu-id="0a3ae-275">(Trecho de código- *modelos e acesso a dados – EX2 global asax setinitialize*)</span><span class="sxs-lookup"><span data-stu-id="0a3ae-275">(Code Snippet - *Models And Data Access - Ex2 Global Asax SetInitializer*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a><span data-ttu-id="0a3ae-276">Tarefa 2-Configurando a conexão com o banco de dados</span><span class="sxs-lookup"><span data-stu-id="0a3ae-276">Task 2 - Configuring the connection to the Database</span></span>

<span data-ttu-id="0a3ae-277">Agora que você já adicionou um banco de dados ao nosso projeto, escreverá no arquivo **Web. config** a cadeia de conexão.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-277">Now that you have already added a database to our project, you will write in the **Web.config** file the connection string.</span></span>

1. <span data-ttu-id="0a3ae-278">Adicione uma cadeia de conexão em **Web. config**. Para fazer isso, abra o **Web. config** na raiz do projeto e substitua a cadeia de conexão chamada DefaultConnection por essa linha na seção **&lt;connectionStrings&gt;** :</span><span class="sxs-lookup"><span data-stu-id="0a3ae-278">Add a connection string at **Web.config**. To do that, open **Web.config** at project root and replace the connection string named DefaultConnection with this line in the **&lt;connectionStrings&gt;** section:</span></span>

    <span data-ttu-id="0a3ae-279">![Local do arquivo Web. config](aspnet-mvc-4-models-and-data-access/_static/image19.png "Local do arquivo Web. config")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-279">![Web.config file location](aspnet-mvc-4-models-and-data-access/_static/image19.png "Web.config file location")</span></span>

    <span data-ttu-id="0a3ae-280">*Local do arquivo Web. config*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-280">*Web.config file location*</span></span>

    [!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a><span data-ttu-id="0a3ae-281">Tarefa 3-trabalhando com o modelo</span><span class="sxs-lookup"><span data-stu-id="0a3ae-281">Task 3 - Working with the Model</span></span>

<span data-ttu-id="0a3ae-282">Agora que você já configurou a conexão com o banco de dados, você vinculará o modelo com as tabelas de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-282">Now that you have already configured the connection to the database, you will link the model with the database tables.</span></span> <span data-ttu-id="0a3ae-283">Nesta tarefa, você criará uma classe que será vinculada ao banco de dados com Code First.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-283">In this task, you will create a class that will be linked to the database with Code First.</span></span> <span data-ttu-id="0a3ae-284">Lembre-se de que há uma classe de modelo POCO existente que deve ser modificada.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-284">Remember that there is an existent POCO model class that should be modified.</span></span>

> [!NOTE]
> <span data-ttu-id="0a3ae-285">Se você concluiu o exercício 1, observará que essa etapa foi executada por um assistente.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-285">If you completed Exercise 1, you will note that this step was performed by a wizard.</span></span> <span data-ttu-id="0a3ae-286">Ao fazer Code First, você criará manualmente as classes que serão vinculadas a entidades de dados.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-286">By doing Code First, you will manually create classes that will be linked to data entities.</span></span>

1. <span data-ttu-id="0a3ae-287">Abra o **gênero** classe de modelo poco da pasta **modelos** projeto e inclua uma ID.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-287">Open the POCO model class **Genre** from **Models** project folder and include an ID.</span></span> <span data-ttu-id="0a3ae-288">Use uma propriedade int com o nome **gêneroid**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-288">Use an int property with the name **GenreId**.</span></span>

    <span data-ttu-id="0a3ae-289">(Trecho de código- *modelos e acesso a dados-Ex2 Code First gênero*)</span><span class="sxs-lookup"><span data-stu-id="0a3ae-289">(Code Snippet - *Models And Data Access - Ex2 Code First Genre*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

    > [!NOTE]
    > <span data-ttu-id="0a3ae-290">Para trabalhar com as convenções de Code First, o gênero de classe deve ter uma propriedade de chave primária que será detectada automaticamente.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-290">To work with Code First conventions, the class Genre must have a primary key property that will be automatically detected.</span></span>
    > 
    > <span data-ttu-id="0a3ae-291">Você pode ler mais sobre as convenções de Code First neste [artigo do MSDN](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="0a3ae-291">You can read more about Code First Conventions in this [msdn article](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).</span></span>
2. <span data-ttu-id="0a3ae-292">Agora, abra o **álbum** de classes do modelo poco na pasta **modelos** do projeto e inclua as chaves estrangeiras, crie propriedades com os nomes **gêneroid** e **artistaid**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-292">Now, open the POCO model class **Album** from **Models** project folder and include the foreign keys, create properties with the names **GenreId** and **ArtistId**.</span></span> <span data-ttu-id="0a3ae-293">Esta classe já tem o **gêneroid** para a chave primária.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-293">This class already have the **GenreId** for the primary key.</span></span>

    <span data-ttu-id="0a3ae-294">(Trecho de código- *modelos e acesso a dados-Ex2 Code First álbum*)</span><span class="sxs-lookup"><span data-stu-id="0a3ae-294">(Code Snippet - *Models And Data Access - Ex2 Code First Album*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
3. <span data-ttu-id="0a3ae-295">Abra o **artista** de classe do modelo poco e inclua a propriedade **artistaid** .</span><span class="sxs-lookup"><span data-stu-id="0a3ae-295">Open the POCO model class **Artist** and include the **ArtistId** property.</span></span>

    <span data-ttu-id="0a3ae-296">(Trecho de código- *modelos e acesso a dados-Ex2 Code First artista*)</span><span class="sxs-lookup"><span data-stu-id="0a3ae-296">(Code Snippet - *Models And Data Access - Ex2 Code First Artist*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
4. <span data-ttu-id="0a3ae-297">Clique com o botão direito do mouse na pasta **modelos** e selecione **Adicionar | Classe**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-297">Right-click the **Models** project folder and select **Add | Class**.</span></span> <span data-ttu-id="0a3ae-298">Nomeie o arquivo **MusicStoreEntities.cs**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-298">Name the file **MusicStoreEntities.cs**.</span></span> <span data-ttu-id="0a3ae-299">Em seguida, clique em **Adicionar.**</span><span class="sxs-lookup"><span data-stu-id="0a3ae-299">Then, click **Add.**</span></span>

    <span data-ttu-id="0a3ae-300">![Adicionando uma classe](aspnet-mvc-4-models-and-data-access/_static/image20.png "Adicionando uma classe")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-300">![Adding a class](aspnet-mvc-4-models-and-data-access/_static/image20.png "Adding a class")</span></span>

    <span data-ttu-id="0a3ae-301">*Adicionando um novo item*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-301">*Adding a new item*</span></span>

    <span data-ttu-id="0a3ae-302">![Adicionando um class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Adicionando um class2")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-302">![Adding a class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Adding a class2")</span></span>

    <span data-ttu-id="0a3ae-303">*Adicionando uma classe*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-303">*Adding a class*</span></span>
5. <span data-ttu-id="0a3ae-304">Abra a classe que você acabou de criar, **MusicStoreEntities.cs**e inclua os namespaces **System. Data. Entity** e **System. Data. Entity. Infrastructure**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-304">Open the class you have just created, **MusicStoreEntities.cs**, and include the namespaces **System.Data.Entity** and **System.Data.Entity.Infrastructure**.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
6. <span data-ttu-id="0a3ae-305">Substitua a declaração de classe para estender a classe **DbContext** : declare um **DBSet** público e substitua o método **OnModelCreating** .</span><span class="sxs-lookup"><span data-stu-id="0a3ae-305">Replace the class declaration to extend the **DbContext** class: declare a public **DBSet** and override **OnModelCreating** method.</span></span> <span data-ttu-id="0a3ae-306">Após essa etapa, você obterá uma classe de domínio que vinculará seu modelo ao Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-306">After this step you will get a domain class that will link your model with the Entity Framework.</span></span> <span data-ttu-id="0a3ae-307">Para fazer isso, substitua o código de classe pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="0a3ae-307">In order to do that, replace the class code with the following:</span></span>

    <span data-ttu-id="0a3ae-308">(Trecho de código- *modelos e acesso a dados-Ex2 Code First MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="0a3ae-308">(Code Snippet - *Models And Data Access - Ex2 Code First MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> <span data-ttu-id="0a3ae-309">Com Entity Framework **DbContext** e **DBSet** , você poderá consultar o gênero da classe poco.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-309">With Entity Framework **DbContext** and **DBSet** you will be able to query the POCO class Genre.</span></span> <span data-ttu-id="0a3ae-310">Ao estender o método **OnModelCreating** , você está especificando no **código** como o gênero será mapeado para uma tabela de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-310">By extending **OnModelCreating** method, you are specifying in the **code** how Genre will be mapped to a database table.</span></span> <span data-ttu-id="0a3ae-311">Você pode encontrar mais informações sobre DBContext e DBSet neste artigo do MSDN: [link](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)</span><span class="sxs-lookup"><span data-stu-id="0a3ae-311">You can find more information about DBContext and DBSet in this msdn article: [link](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="0a3ae-312">Tarefa 4-consultando o banco de dados</span><span class="sxs-lookup"><span data-stu-id="0a3ae-312">Task 4 - Querying the Database</span></span>

<span data-ttu-id="0a3ae-313">Nesta tarefa, você atualizará a classe StoreController para que, em vez de usar os dados codificados, ele a recuperará do banco de dado.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-313">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will retrieve it from the database.</span></span>

> [!NOTE]
> <span data-ttu-id="0a3ae-314">Essa tarefa é comum com o exercício 1.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-314">This task is in common with Exercise 1.</span></span>
> 
> <span data-ttu-id="0a3ae-315">Se você concluiu o exercício 1, observará que essas etapas são as mesmas em ambas as abordagens (primeiro banco de dados ou código primeiro).</span><span class="sxs-lookup"><span data-stu-id="0a3ae-315">If you completed Exercise 1 you will note these steps are the same in both approaches (Database first or Code first).</span></span> <span data-ttu-id="0a3ae-316">Eles são diferentes na forma como os dados são vinculados ao modelo, mas o acesso às entidades de dados ainda é transparente do controlador.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-316">They are different in how the data is linked with the model, but the access to data entities is yet transparent from the controller.</span></span>

1. <span data-ttu-id="0a3ae-317">Abra **Controllers\StoreController.cs** e adicione o seguinte campo à classe para manter uma instância da classe **MusicStoreEntities** , chamada **storeDB**:</span><span class="sxs-lookup"><span data-stu-id="0a3ae-317">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="0a3ae-318">(Trecho de código- *modelos e acesso a dados-EX1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="0a3ae-318">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
2. <span data-ttu-id="0a3ae-319">A classe **MusicStoreEntities** expõe uma propriedade de coleção para cada tabela no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-319">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="0a3ae-320">Atualize o método de ação de **procura** para recuperar um gênero com todos os **álbuns**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-320">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="0a3ae-321">(Trecho de código- *modelos e acesso a dados-navegação do EX2 Store*)</span><span class="sxs-lookup"><span data-stu-id="0a3ae-321">(Code Snippet - *Models And Data Access - Ex2 Store Browse*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="0a3ae-322">Você está usando uma funcionalidade do .NET chamada **LINQ** (consulta integrada à linguagem) para gravar expressões de consulta fortemente tipadas em relação a essas coleções, o que executará o código no banco de dados e retornará objetos com os quais você pode programar.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-322">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
    > 
    > <span data-ttu-id="0a3ae-323">Para obter mais informações sobre o LINQ, visite o [site do MSDN](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="0a3ae-323">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).</span></span>
3. <span data-ttu-id="0a3ae-324">Atualizar o método de ação de **índice** para recuperar todos os gêneros.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-324">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="0a3ae-325">(Trecho de código- *modelos e acesso a dados-índice de armazenamento EX2*)</span><span class="sxs-lookup"><span data-stu-id="0a3ae-325">(Code Snippet - *Models And Data Access - Ex2 Store Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
4. <span data-ttu-id="0a3ae-326">Atualizar o método de ação de **índice** para recuperar todos os gêneros e transformar a coleção em uma lista.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-326">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="0a3ae-327">(Trecho de código- *modelos e acesso a dados-EX2 Store GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="0a3ae-327">(Code Snippet - *Models And Data Access - Ex2 Store GenreMenu*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="0a3ae-328">Tarefa 5-executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="0a3ae-328">Task 5 - Running the Application</span></span>

<span data-ttu-id="0a3ae-329">Nesta tarefa, você verificará se a página de índice da loja agora exibirá os gêneros armazenados no banco de dados em vez dos codificados.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-329">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="0a3ae-330">Não é necessário alterar o modelo de exibição porque o **StoreController** está retornando o mesmo **StoreIndexViewModel** que antes, mas desta vez os dados serão provenientes do banco de dado.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-330">There is no need to change the View template because the **StoreController** is returning the same **StoreIndexViewModel** as before, but this time the data will come from the database.</span></span>

1. <span data-ttu-id="0a3ae-331">Recompile a solução e pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-331">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="0a3ae-332">O projeto é iniciado na Home Page.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-332">The project starts in the Home page.</span></span> <span data-ttu-id="0a3ae-333">Verifique se o menu de **gêneros** não é mais uma lista embutida em código e se os dados são recuperados diretamente do banco de dado.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-333">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    <span data-ttu-id="0a3ae-335">*Pesquisando gêneros do banco de dados*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-335">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="0a3ae-336">Agora, navegue até qualquer gênero e verifique se os álbuns estão populados do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-336">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="0a3ae-337">![Pesquisando álbuns do banco de dados](aspnet-mvc-4-models-and-data-access/_static/image23.png "Pesquisando álbuns do banco de dados")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-337">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image23.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="0a3ae-338">*Pesquisando álbuns do banco de dados*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-338">*Browsing Albums from the database*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a><span data-ttu-id="0a3ae-339">Exercício 3: consultando o banco de dados com parâmetros</span><span class="sxs-lookup"><span data-stu-id="0a3ae-339">Exercise 3: Querying the Database with Parameters</span></span>

<span data-ttu-id="0a3ae-340">Neste exercício, você aprenderá a consultar o banco de dados usando parâmetros e a usar a formatação de resultados da consulta, um recurso que reduz o número de acessos de banco de dados que recuperam os mesmos de maneira mais eficiente.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-340">In this exercise, you will learn how to query the database using parameters, and how to use Query Result Shaping, a feature that reduces the number database accesses retrieving data in a more efficient way.</span></span>

> [!NOTE]
> <span data-ttu-id="0a3ae-341">Para obter mais informações sobre o formato de resultado da consulta, visite o seguinte [artigo do MSDN](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="0a3ae-341">For further information on Query Result Shaping, visit the following [msdn article](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a><span data-ttu-id="0a3ae-342">Tarefa 1-modificando StoreController para recuperar álbuns do banco de dados</span><span class="sxs-lookup"><span data-stu-id="0a3ae-342">Task 1 - Modifying StoreController to Retrieve Albums from Database</span></span>

<span data-ttu-id="0a3ae-343">Nesta tarefa, você vai alterar a classe **StoreController** para acessar o banco de dados para recuperar álbuns de um gênero específico.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-343">In this task, you will change the **StoreController** class to access the database to retrieve albums from a specific genre.</span></span>

1. <span data-ttu-id="0a3ae-344">Abra a solução **inicial** localizada na pasta **Source\Ex3-QueryingTheDatabaseWithParametersCodeFirst\Begin** se você quiser usar a abordagem de primeiro código ou a pasta **Source\Ex3-QueryingTheDatabaseWithParametersDBFirst\Begin** se quiser usar a abordagem do banco de dados primeiro.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-344">Open the **Begin** solution located at the **Source\Ex3-QueryingTheDatabaseWithParametersCodeFirst\Begin** folder if you want to use Code-First approach or **Source\Ex3-QueryingTheDatabaseWithParametersDBFirst\Begin** folder if you want to use Database-First approach.</span></span> <span data-ttu-id="0a3ae-345">Caso contrário, você pode continuar usando a solução **final** obtida concluindo o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-345">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="0a3ae-346">Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-346">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="0a3ae-347">Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-347">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="0a3ae-348">Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-348">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="0a3ae-349">Por fim, Compile a solução clicando em **build** | **Compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-349">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="0a3ae-350">Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-350">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="0a3ae-351">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-351">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="0a3ae-352">É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-352">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="0a3ae-353">Abra a classe **StoreController** para alterar o método de ação de **procura** .</span><span class="sxs-lookup"><span data-stu-id="0a3ae-353">Open the **StoreController** class to change the **Browse** action method.</span></span> <span data-ttu-id="0a3ae-354">Para fazer isso, na **Gerenciador de soluções**, expanda a pasta **controladores** e clique duas vezes em **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-354">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
3. <span data-ttu-id="0a3ae-355">Altere o método de ação **procurar** para recuperar álbuns de um gênero específico.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-355">Change the **Browse** action method to retrieve albums for a specific genre.</span></span> <span data-ttu-id="0a3ae-356">Para fazer isso, substitua o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="0a3ae-356">To do this, replace the following code:</span></span>

    <span data-ttu-id="0a3ae-357">(Trecho de código- *modelos e acesso a dados-EX3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="0a3ae-357">(Code Snippet - *Models And Data Access - Ex3 StoreController BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> <span data-ttu-id="0a3ae-358">Para preencher uma coleção da entidade, você precisa usar o método **include** para especificar que deseja recuperar os álbuns também.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-358">To populate a collection of the entity, you need to use the **Include** method to specify you want to retrieve the albums too.</span></span> <span data-ttu-id="0a3ae-359">Você pode usar o. Extensão **Single ()** no LINQ porque, nesse caso, apenas um gênero é esperado para um álbum.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-359">You can use the .**Single()** extension in LINQ because in this case only one genre is expected for an album.</span></span> <span data-ttu-id="0a3ae-360">O método **Single ()** usa uma expressão lambda como um parâmetro, que nesse caso especifica um único objeto de gênero, de modo que seu nome corresponde ao valor definido.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-360">The **Single()** method takes a Lambda expression as a parameter, which in this case specifies a single Genre object such that its name matches the value defined.</span></span>
> 
> <span data-ttu-id="0a3ae-361">Você aproveitará um recurso que permite que você indique outras entidades relacionadas que deseja carregar também quando o objeto gênero for recuperado.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-361">You will take advantage of a feature that allows you to indicate other related entities you want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="0a3ae-362">Esse recurso é chamado de **formato de resultado de consulta**e permite reduzir o número de vezes necessário para acessar o banco de dados para recuperar informações.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-362">This feature is called **Query Result Shaping**, and enables you to reduce the number of times needed to access the database to retrieve information.</span></span> <span data-ttu-id="0a3ae-363">Nesse cenário, você desejará buscar previamente os álbuns para o gênero recuperado.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-363">In this scenario, you will want to pre-fetch the Albums for the Genre you retrieve.</span></span>
> 
> <span data-ttu-id="0a3ae-364">A consulta inclui **gêneros. include (&quot;álbuns&quot;)** para indicar que você também deseja álbuns relacionados.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-364">The query includes **Genres.Include(&quot;Albums&quot;)** to indicate that you want related albums as well.</span></span> <span data-ttu-id="0a3ae-365">Isso resultará em um aplicativo mais eficiente, pois ele recuperará os dados de gênero e de álbum em uma única solicitação de banco de dado.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-365">This will result in a more efficient application, since it will retrieve both Genre and Album data in a single database request.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="0a3ae-366">Tarefa 2-executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="0a3ae-366">Task 2 - Running the Application</span></span>

<span data-ttu-id="0a3ae-367">Nesta tarefa, você executará o aplicativo e recuperará os álbuns de um gênero específico do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-367">In this task, you will run the application and retrieve albums of a specific genre from the database.</span></span>

1. <span data-ttu-id="0a3ae-368">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-368">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="0a3ae-369">O projeto é iniciado na Home Page.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-369">The project starts in the Home page.</span></span> <span data-ttu-id="0a3ae-370">Altere a URL para **/Store/Browse? gênero = pop** para verificar se os resultados estão sendo recuperados do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-370">Change the URL to **/Store/Browse?genre=Pop** to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="0a3ae-371">![Pesquisando por gênero](aspnet-mvc-4-models-and-data-access/_static/image24.png "Pesquisando por gênero")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-371">![Browsing by genre](aspnet-mvc-4-models-and-data-access/_static/image24.png "Browsing by genre")</span></span>

    <span data-ttu-id="0a3ae-372">*Navegando em/Store/Browse? gênero = pop*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-372">*Browsing /Store/Browse?genre=Pop*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a><span data-ttu-id="0a3ae-373">Tarefa 3-acessando álbuns por ID</span><span class="sxs-lookup"><span data-stu-id="0a3ae-373">Task 3 - Accessing Albums by Id</span></span>

<span data-ttu-id="0a3ae-374">Nesta tarefa, você repetirá o procedimento anterior para obter álbuns por sua ID.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-374">In this task, you will repeat the previous procedure to get albums by their Id.</span></span>

1. <span data-ttu-id="0a3ae-375">Feche o navegador, se necessário, para retornar ao Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-375">Close the browser if needed, to return to Visual Studio.</span></span> <span data-ttu-id="0a3ae-376">Abra a classe **StoreController** para alterar o método de ação de **detalhes** .</span><span class="sxs-lookup"><span data-stu-id="0a3ae-376">Open the **StoreController** class to change the **Details** action method.</span></span> <span data-ttu-id="0a3ae-377">Para fazer isso, na **Gerenciador de soluções**, expanda a pasta **controladores** e clique duas vezes em **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-377">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
2. <span data-ttu-id="0a3ae-378">Altere o método de ação de **detalhes** para recuperar os detalhes dos álbuns com base em sua **ID**. Para fazer isso, substitua o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="0a3ae-378">Change the **Details** action method to retrieve albums details based on their **Id**. To do this, replace the following code:</span></span>

    <span data-ttu-id="0a3ae-379">(Trecho de código- *modelos e acesso a dados-EX3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="0a3ae-379">(Code Snippet - *Models And Data Access - Ex3 StoreController DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="0a3ae-380">Tarefa 4-executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="0a3ae-380">Task 4 - Running the Application</span></span>

<span data-ttu-id="0a3ae-381">Nesta tarefa, você executará o aplicativo em um navegador da Web e obterá detalhes do álbum por sua ID.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-381">In this task, you will run the Application in a web browser and obtain album details by their Id.</span></span>

1. <span data-ttu-id="0a3ae-382">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-382">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="0a3ae-383">O projeto é iniciado na Home Page.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-383">The project starts in the Home page.</span></span> <span data-ttu-id="0a3ae-384">Altere a URL para **/Store/Details/51** ou navegue pelos gêneros e selecione um álbum para verificar se os resultados estão sendo recuperados do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-384">Change the URL to **/Store/Details/51** or browse the genres and select an album to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="0a3ae-385">![Detalhes da navegação](aspnet-mvc-4-models-and-data-access/_static/image25.png "Detalhes da navegação")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-385">![Browsing Details](aspnet-mvc-4-models-and-data-access/_static/image25.png "Browsing Details")</span></span>

    <span data-ttu-id="0a3ae-386">*Navegando em/Store/Details/51*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-386">*Browsing /Store/Details/51*</span></span>

> [!NOTE]
> <span data-ttu-id="0a3ae-387">Além disso, você pode implantar esse aplicativo em sites do Windows Azure seguindo [o apêndice B: publicando um aplicativo ASP.NET MVC 4 usando implantação da Web](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="0a3ae-387">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="0a3ae-388">Resumo</span><span class="sxs-lookup"><span data-stu-id="0a3ae-388">Summary</span></span>

<span data-ttu-id="0a3ae-389">Ao concluir este laboratório prático, você aprendeu os conceitos básicos dos modelos e do acesso a dados do ASP.NET MVC, usando a abordagem de **Database First** , bem como a abordagem de **Code First** :</span><span class="sxs-lookup"><span data-stu-id="0a3ae-389">By completing this Hands-on Lab you have learned the fundamentals of ASP.NET MVC Models and Data Access, using the **Database First** approach as well as the **Code First** Approach:</span></span>

- <span data-ttu-id="0a3ae-390">Como adicionar um banco de dados à solução a fim de consumir seu dado</span><span class="sxs-lookup"><span data-stu-id="0a3ae-390">How to add a database to the solution in order to consume its data</span></span>
- <span data-ttu-id="0a3ae-391">Como atualizar controladores para fornecer modelos de exibição com os dados extraídos do banco de dado em vez de embutidos em código um</span><span class="sxs-lookup"><span data-stu-id="0a3ae-391">How to update Controllers to provide View templates with the data taken from the database instead of hard-coded one</span></span>
- <span data-ttu-id="0a3ae-392">Como consultar o banco de dados usando parâmetros</span><span class="sxs-lookup"><span data-stu-id="0a3ae-392">How to query the database using parameters</span></span>
- <span data-ttu-id="0a3ae-393">Como usar o formato de resultado da consulta, um recurso que reduz o número de acessos ao banco de dados, recuperando os dados de maneira mais eficiente</span><span class="sxs-lookup"><span data-stu-id="0a3ae-393">How to use the Query Result Shaping, a feature that reduces the number of database accesses, retrieving data in a more efficient way</span></span>
- <span data-ttu-id="0a3ae-394">Como usar as abordagens Database First e Code First no Microsoft Entity Framework para vincular o banco de dados ao modelo</span><span class="sxs-lookup"><span data-stu-id="0a3ae-394">How to use both Database First and Code First approaches in Microsoft Entity Framework to link the database with the model</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="0a3ae-395">Apêndice A: Instalando o Visual Studio Express 2012 para Web</span><span class="sxs-lookup"><span data-stu-id="0a3ae-395">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="0a3ae-396">Você pode instalar o **Microsoft Visual Studio Express 2012 para Web** ou outra versão &quot;Express&quot; usando o **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="0a3ae-396">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="0a3ae-397">As instruções a seguir orientam você pelas etapas necessárias para instalar o *Visual Studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-397">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="0a3ae-398">Vá para [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="0a3ae-398">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="0a3ae-399">Como alternativa, se você já tiver instalado o Web Platform Installer, poderá abri-lo e pesquisar o produto &quot;<em>Visual Studio Express 2012 para Web com o SDK do Windows Azure</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-399">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="0a3ae-400">Clique em **instalar agora**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-400">Click on **Install Now**.</span></span> <span data-ttu-id="0a3ae-401">Se você não tiver **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-401">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="0a3ae-402">Quando **Web Platform Installer** estiver aberto, clique em **instalar** para iniciar a instalação.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-402">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="0a3ae-403">![Instalar Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "Instalar Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-403">![Install Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="0a3ae-404">*Instalar Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-404">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="0a3ae-405">Leia todos os termos e licenças de produtos e clique em **aceito** para continuar.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-405">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Aceitando os termos de licença](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    <span data-ttu-id="0a3ae-407">*Aceitando os termos de licença*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-407">*Accepting the license terms*</span></span>
5. <span data-ttu-id="0a3ae-408">Aguarde até que o processo de download e instalação seja concluído.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-408">Wait until the downloading and installation process completes.</span></span>

    ![Progresso da instalação](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    <span data-ttu-id="0a3ae-410">*Progresso da instalação*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-410">*Installation progress*</span></span>
6. <span data-ttu-id="0a3ae-411">Quando a instalação for concluída, clique em **concluir**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-411">When the installation completes, click **Finish**.</span></span>

    ![Instalação concluída](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    <span data-ttu-id="0a3ae-413">*Instalação concluída*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-413">*Installation completed*</span></span>
7. <span data-ttu-id="0a3ae-414">Clique em **sair** para fechar Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-414">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="0a3ae-415">Para abrir o Visual Studio Express para Web, vá para a tela **Iniciar** e comece a escrever &quot;**vs Express**&quot;e, em seguida, clique no bloco **vs Express para Web** .</span><span class="sxs-lookup"><span data-stu-id="0a3ae-415">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Bloco VS Express para Web](aspnet-mvc-4-models-and-data-access/_static/image30.png)

    <span data-ttu-id="0a3ae-417">*Bloco VS Express para Web*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-417">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="0a3ae-418">Apêndice B: publicando um aplicativo ASP.NET MVC 4 usando Implantação da Web</span><span class="sxs-lookup"><span data-stu-id="0a3ae-418">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="0a3ae-419">Este apêndice mostrará como criar um novo site da Web do Windows Azure Portal de Gerenciamento e publicar o aplicativo obtido seguindo o laboratório, aproveitando o recurso de publicação de Implantação da Web fornecido pelo Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-419">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="0a3ae-420">Tarefa 1-Criando um novo site no portal do Windows Azure</span><span class="sxs-lookup"><span data-stu-id="0a3ae-420">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="0a3ae-421">Acesse o [portal de gerenciamento do Windows Azure](https://manage.windowsazure.com/) e entre usando as credenciais da Microsoft associadas à sua assinatura.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-421">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="0a3ae-422">Com o Windows Azure, você pode hospedar 10 sites da Web de ASP.NET gratuitamente e, em seguida, dimensionar conforme o tráfego cresce.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-422">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="0a3ae-423">Você pode se inscrever [aqui](https://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="0a3ae-423">You can sign up [here](https://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="0a3ae-424">![Fazer logon no Windows portal do Azure](aspnet-mvc-4-models-and-data-access/_static/image31.png "Fazer logon no Windows portal do Azure")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-424">![Log on to Windows Azure portal](aspnet-mvc-4-models-and-data-access/_static/image31.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="0a3ae-425">*Fazer logon no Windows Azure Portal de Gerenciamento*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-425">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="0a3ae-426">Clique em **novo** na barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-426">Click **New** on the command bar.</span></span>

    <span data-ttu-id="0a3ae-427">![Criando um novo site](aspnet-mvc-4-models-and-data-access/_static/image32.png "Criando um novo site")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-427">![Creating a new Web Site](aspnet-mvc-4-models-and-data-access/_static/image32.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="0a3ae-428">*Criando um novo site*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-428">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="0a3ae-429">Clique em **computação** | **site**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-429">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="0a3ae-430">Em seguida, selecione a opção **criação rápida** .</span><span class="sxs-lookup"><span data-stu-id="0a3ae-430">Then select **Quick Create** option.</span></span> <span data-ttu-id="0a3ae-431">Forneça uma URL disponível para o novo site e clique em **criar site**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-431">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="0a3ae-432">Um site do Windows Azure é o host de um aplicativo Web em execução na nuvem que você pode controlar e gerenciar.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-432">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="0a3ae-433">A opção criação rápida permite que você implante um aplicativo Web concluído no site do Windows Azure de fora do Portal.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-433">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="0a3ae-434">Ele não inclui etapas para configurar um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-434">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="0a3ae-435">![Criando um novo site usando a criação rápida](aspnet-mvc-4-models-and-data-access/_static/image33.png "Criando um novo site usando a criação rápida")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-435">![Creating a new Web Site using Quick Create](aspnet-mvc-4-models-and-data-access/_static/image33.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="0a3ae-436">*Criando um novo site usando a criação rápida*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-436">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="0a3ae-437">Aguarde até que o novo **site** seja criado.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-437">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="0a3ae-438">Depois que o site for criado, clique no link sob a coluna **URL** .</span><span class="sxs-lookup"><span data-stu-id="0a3ae-438">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="0a3ae-439">Verifique se o novo site está funcionando.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-439">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="0a3ae-440">![Navegando até o novo site](aspnet-mvc-4-models-and-data-access/_static/image34.png "Navegando até o novo site")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-440">![Browsing to the new web site](aspnet-mvc-4-models-and-data-access/_static/image34.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="0a3ae-441">*Navegando até o novo site*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-441">*Browsing to the new web site*</span></span>

    <span data-ttu-id="0a3ae-442">![Site em execução](aspnet-mvc-4-models-and-data-access/_static/image35.png "Site em execução")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-442">![Web site running](aspnet-mvc-4-models-and-data-access/_static/image35.png "Web site running")</span></span>

    <span data-ttu-id="0a3ae-443">*Site em execução*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-443">*Web site running*</span></span>
6. <span data-ttu-id="0a3ae-444">Volte para o portal e clique no nome do site na coluna **nome** para exibir as páginas de gerenciamento.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-444">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="0a3ae-445">![Abrindo as páginas de gerenciamento de site](aspnet-mvc-4-models-and-data-access/_static/image36.png "Abrindo as páginas de gerenciamento de site")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-445">![Opening the web site management pages](aspnet-mvc-4-models-and-data-access/_static/image36.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="0a3ae-446">*Abrindo as páginas de gerenciamento de site*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-446">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="0a3ae-447">Na página **painel** , na seção **visão rápida** , clique no link **baixar perfil de publicação** .</span><span class="sxs-lookup"><span data-stu-id="0a3ae-447">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="0a3ae-448">O *perfil de publicação* contém todas as informações necessárias para publicar um aplicativo Web em um site do Windows Azure para cada método de publicação habilitado.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-448">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="0a3ae-449">O perfil de publicação contém as URLs, as credenciais de usuário e as cadeias de conexão de banco de dados necessárias para conectar-se e autenticar cada um dos pontos de extremidade para os quais um método de publicação é habilitado.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-449">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="0a3ae-450">**O Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** e **Microsoft Visual Studio 2012** dão suporte à leitura de perfis de publicação para automatizar a configuração desses programas para a publicação de aplicativos Web em sites do Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-450">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="0a3ae-451">![Baixando o perfil de publicação do site](aspnet-mvc-4-models-and-data-access/_static/image37.png "Baixando o perfil de publicação do site")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-451">![Downloading the web site publish profile](aspnet-mvc-4-models-and-data-access/_static/image37.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="0a3ae-452">*Baixando o perfil de publicação do site*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-452">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="0a3ae-453">Baixe o arquivo de perfil de publicação em um local conhecido.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-453">Download the publish profile file to a known location.</span></span> <span data-ttu-id="0a3ae-454">Neste exercício, você verá como usar esse arquivo para publicar um aplicativo Web em um site do Windows Azure a partir do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-454">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="0a3ae-455">![Salvando o arquivo de perfil de publicação](aspnet-mvc-4-models-and-data-access/_static/image38.png "Salvando o perfil de publicação")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-455">![Saving the publish profile file](aspnet-mvc-4-models-and-data-access/_static/image38.png "Saving the publish profile")</span></span>

    <span data-ttu-id="0a3ae-456">*Salvando o arquivo de perfil de publicação*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-456">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="0a3ae-457">Tarefa 2-Configurando o servidor de banco de dados</span><span class="sxs-lookup"><span data-stu-id="0a3ae-457">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="0a3ae-458">Se seu aplicativo utiliza bancos de dados SQL Server, você precisará criar um servidor de banco de dados SQL.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-458">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="0a3ae-459">Se você quiser implantar um aplicativo simples que não usa SQL Server você pode ignorar essa tarefa.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-459">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="0a3ae-460">Você precisará de um servidor do banco de dados SQL para armazenar o banco de dados do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-460">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="0a3ae-461">Você pode exibir os servidores do banco de dados SQL de sua assinatura no portal de gerenciamento do Windows Azure em bancos de dados **sql** | **servidores** | **painel do servidor**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-461">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="0a3ae-462">Se você não tiver um servidor criado, poderá criar um usando o botão **Adicionar** na barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-462">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="0a3ae-463">Anote o nome do **servidor e a URL, o nome de logon e a senha do administrador**, pois você irá usá-los nas próximas tarefas.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-463">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="0a3ae-464">Não crie o banco de dados ainda, pois ele será criado em um estágio posterior.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-464">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="0a3ae-465">![Painel do servidor do banco de dados SQL](aspnet-mvc-4-models-and-data-access/_static/image39.png "Painel do servidor do banco de dados SQL")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-465">![SQL Database Server Dashboard](aspnet-mvc-4-models-and-data-access/_static/image39.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="0a3ae-466">*Painel do servidor do banco de dados SQL*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-466">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="0a3ae-467">Na próxima tarefa, você testará a conexão de banco de dados do Visual Studio, por esse motivo, será necessário incluir o endereço IP local na lista de **endereços IP permitidos**do servidor.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-467">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="0a3ae-468">Para fazer isso, clique em **Configurar**, selecione o endereço IP do **endereço IP do cliente atual** e cole-o nas caixas de texto endereço IP **inicial** e **endereço IP final** e clique no botão ![adicionar-cliente-IP-endereço-OK-botão](aspnet-mvc-4-models-and-data-access/_static/image40.png).</span><span class="sxs-lookup"><span data-stu-id="0a3ae-468">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) button.</span></span>

    ![Adicionando endereço IP do cliente](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    <span data-ttu-id="0a3ae-470">*Adicionando endereço IP do cliente*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-470">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="0a3ae-471">Depois que o **endereço IP do cliente** for adicionado à lista endereços IP permitidos, clique em **salvar** para confirmar as alterações.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-471">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confirmar alterações](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    <span data-ttu-id="0a3ae-473">*Confirmar alterações*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-473">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="0a3ae-474">Tarefa 3-publicando um aplicativo ASP.NET MVC 4 usando Implantação da Web</span><span class="sxs-lookup"><span data-stu-id="0a3ae-474">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="0a3ae-475">Volte para a solução ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-475">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="0a3ae-476">Na **Gerenciador de soluções**, clique com o botão direito do mouse no projeto de site e selecione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-476">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="0a3ae-477">![Publicando o aplicativo](aspnet-mvc-4-models-and-data-access/_static/image43.png "Publicando o aplicativo")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-477">![Publishing the Application](aspnet-mvc-4-models-and-data-access/_static/image43.png "Publishing the Application")</span></span>

    <span data-ttu-id="0a3ae-478">*Publicando o site*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-478">*Publishing the web site*</span></span>
2. <span data-ttu-id="0a3ae-479">Importe o perfil de publicação salvo na primeira tarefa.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-479">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="0a3ae-480">![Importando o perfil de publicação](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importando o perfil de publicação")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-480">![Importing the publish profile](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importing the publish profile")</span></span>

    <span data-ttu-id="0a3ae-481">*Importando perfil de publicação*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-481">*Importing publish profile*</span></span>
3. <span data-ttu-id="0a3ae-482">Clique em **validar conexão**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-482">Click **Validate Connection**.</span></span> <span data-ttu-id="0a3ae-483">Depois que a validação for concluída, clique em **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-483">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="0a3ae-484">A validação será concluída quando você vir uma marca de seleção verde ao lado do botão Validar conexão.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-484">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="0a3ae-485">![Validando conexão](aspnet-mvc-4-models-and-data-access/_static/image45.png "Validando conexão")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-485">![Validating connection](aspnet-mvc-4-models-and-data-access/_static/image45.png "Validating connection")</span></span>

    <span data-ttu-id="0a3ae-486">*Validando conexão*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-486">*Validating connection*</span></span>
4. <span data-ttu-id="0a3ae-487">Na página **configurações** , na seção **bancos** de dados, clique no botão ao lado da caixa de texto da sua conexão de banco (ou seja, **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="0a3ae-487">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="0a3ae-488">![Configuração de implantação da Web](aspnet-mvc-4-models-and-data-access/_static/image46.png "Configuração de implantação da Web")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-488">![Web deploy configuration](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web deploy configuration")</span></span>

    <span data-ttu-id="0a3ae-489">*Configuração de implantação da Web*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-489">*Web deploy configuration*</span></span>
5. <span data-ttu-id="0a3ae-490">Configure a conexão de banco de dados da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="0a3ae-490">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="0a3ae-491">No **nome do servidor** , digite a URL do servidor do banco de dados SQL usando o prefixo *TCP:* .</span><span class="sxs-lookup"><span data-stu-id="0a3ae-491">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="0a3ae-492">Em **nome de usuário** , digite o nome de logon do administrador do servidor.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-492">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="0a3ae-493">Em **senha** , digite a senha de logon do administrador do servidor.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-493">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="0a3ae-494">Digite um novo nome de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-494">Type a new database name.</span></span>

     <span data-ttu-id="0a3ae-495">![Configurando a cadeia de conexão de destino](aspnet-mvc-4-models-and-data-access/_static/image47.png "Configurando a cadeia de conexão de destino")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-495">![Configuring destination connection string](aspnet-mvc-4-models-and-data-access/_static/image47.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="0a3ae-496">*Configurando a cadeia de conexão de destino*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-496">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="0a3ae-497">Em seguida, clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-497">Then click **OK**.</span></span> <span data-ttu-id="0a3ae-498">Quando for solicitado a criar o banco de dados, clique em **Sim**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-498">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="0a3ae-499">![Criando o banco de dados](aspnet-mvc-4-models-and-data-access/_static/image48.png "Criando a cadeia de caracteres do banco de dados")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-499">![Creating the database](aspnet-mvc-4-models-and-data-access/_static/image48.png "Creating the database string")</span></span>

    <span data-ttu-id="0a3ae-500">*Criando o banco de dados*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-500">*Creating the database*</span></span>
7. <span data-ttu-id="0a3ae-501">A cadeia de conexão que você usará para se conectar ao banco de dados SQL no Windows Azure é mostrada na caixa de texto conexão padrão.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-501">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="0a3ae-502">Em seguida, clique em **Próximo**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-502">Then click **Next**.</span></span>

    <span data-ttu-id="0a3ae-503">![Cadeia de conexão apontando para o banco de dados SQL](aspnet-mvc-4-models-and-data-access/_static/image49.png "Cadeia de conexão apontando para o banco de dados SQL")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-503">![Connection string pointing to SQL Database](aspnet-mvc-4-models-and-data-access/_static/image49.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="0a3ae-504">*Cadeia de conexão apontando para o banco de dados SQL*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-504">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="0a3ae-505">Na página **Visualização** , clique em **publicar**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-505">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="0a3ae-506">![Publicando o aplicativo Web](aspnet-mvc-4-models-and-data-access/_static/image50.png "Publicando o aplicativo Web")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-506">![Publishing the web application](aspnet-mvc-4-models-and-data-access/_static/image50.png "Publishing the web application")</span></span>

    <span data-ttu-id="0a3ae-507">*Publicando o aplicativo Web*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-507">*Publishing the web application*</span></span>
9. <span data-ttu-id="0a3ae-508">Quando o processo de publicação for concluído, o navegador padrão abrirá o site publicado.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-508">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="0a3ae-509">Apêndice C: usando trechos de código</span><span class="sxs-lookup"><span data-stu-id="0a3ae-509">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="0a3ae-510">Com trechos de código, você tem todo o código de que precisa ao seu alcance.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-510">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="0a3ae-511">O documento de laboratório informará exatamente quando você pode usá-los, conforme mostrado na figura a seguir.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-511">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="0a3ae-512">![Usando trechos de código do Visual Studio para inserir código em seu projeto](aspnet-mvc-4-models-and-data-access/_static/image51.png "Usando trechos de código do Visual Studio para inserir código em seu projeto")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-512">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-models-and-data-access/_static/image51.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="0a3ae-513">*Usando trechos de código do Visual Studio para inserir código em seu projeto*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-513">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="0a3ae-514">***Para adicionar um trecho de código usando o tecladoC# (somente)***</span><span class="sxs-lookup"><span data-stu-id="0a3ae-514">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="0a3ae-515">Coloque o cursor onde você deseja inserir o código.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-515">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="0a3ae-516">Comece digitando o nome do trecho de código (sem espaços ou hifens).</span><span class="sxs-lookup"><span data-stu-id="0a3ae-516">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="0a3ae-517">Observe como o IntelliSense exibe nomes de trechos de código correspondentes.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-517">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="0a3ae-518">Selecione o trecho correto (ou continue digitando até que o nome do trecho inteiro seja selecionado).</span><span class="sxs-lookup"><span data-stu-id="0a3ae-518">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="0a3ae-519">Pressione a tecla TAB duas vezes para inserir o trecho de código no local do cursor.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-519">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="0a3ae-520">![Comece a digitar o nome do trecho](aspnet-mvc-4-models-and-data-access/_static/image52.png "Comece a digitar o nome do trecho")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-520">![Start typing the snippet name](aspnet-mvc-4-models-and-data-access/_static/image52.png "Start typing the snippet name")</span></span>

<span data-ttu-id="0a3ae-521">*Comece a digitar o nome do trecho*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-521">*Start typing the snippet name*</span></span>

<span data-ttu-id="0a3ae-522">![Pressione Tab para selecionar o trecho realçado](aspnet-mvc-4-models-and-data-access/_static/image53.png "Pressione Tab para selecionar o trecho realçado")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-522">![Press Tab to select the highlighted snippet](aspnet-mvc-4-models-and-data-access/_static/image53.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="0a3ae-523">*Pressione Tab para selecionar o trecho realçado*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-523">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="0a3ae-524">![Pressione Tab novamente e o trecho será expandido](aspnet-mvc-4-models-and-data-access/_static/image54.png "Pressione Tab novamente e o trecho será expandido")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-524">![Press Tab again and the snippet will expand](aspnet-mvc-4-models-and-data-access/_static/image54.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="0a3ae-525">*Pressione Tab novamente e o trecho será expandido*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-525">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="0a3ae-526">***Para adicionar um trecho de código usando o mouseC#(, Visual Basic e XML)*** uma.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-526">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="0a3ae-527">Clique com o botão direito do mouse no local em que você deseja inserir o trecho de código.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-527">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="0a3ae-528">Selecione **Inserir trecho** seguido por **meus trechos de código**.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-528">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="0a3ae-529">Selecione o trecho relevante na lista clicando nele.</span><span class="sxs-lookup"><span data-stu-id="0a3ae-529">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="0a3ae-530">![Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho](aspnet-mvc-4-models-and-data-access/_static/image55.png "Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-530">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-models-and-data-access/_static/image55.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="0a3ae-531">*Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-531">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="0a3ae-532">![Selecione o trecho relevante na lista clicando nele](aspnet-mvc-4-models-and-data-access/_static/image56.png "Selecione o trecho relevante na lista clicando nele")</span><span class="sxs-lookup"><span data-stu-id="0a3ae-532">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-models-and-data-access/_static/image56.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="0a3ae-533">*Selecione o trecho relevante na lista clicando nele*</span><span class="sxs-lookup"><span data-stu-id="0a3ae-533">*Pick the relevant snippet from the list, by clicking on it*</span></span>
