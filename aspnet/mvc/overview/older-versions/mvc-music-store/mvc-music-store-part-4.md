---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: 'Parte 4: modelos e acesso a dados | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo de loja de música MVC do ASP.NET. A parte 4 abrange modelos e acesso a dados.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 402be340f1ea3344675e7b859cea8c5130cfc8ee
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559671"
---
# <a name="part-4-models-and-data-access"></a><span data-ttu-id="14625-104">Parte 4: modelos e acesso a dados</span><span class="sxs-lookup"><span data-stu-id="14625-104">Part 4: Models and Data Access</span></span>

<span data-ttu-id="14625-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="14625-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="14625-106">A loja de música MVC é um aplicativo de tutorial que apresenta e explica passo a passo como usar o ASP.NET MVC e o Visual Studio para desenvolvimento para a Web.</span><span class="sxs-lookup"><span data-stu-id="14625-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="14625-107">A loja de música MVC é uma implementação de armazenamento de exemplo leve que vende os álbuns de música online e implementa a administração básica de site, a entrada do usuário e a funcionalidade do carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="14625-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>
> 
> <span data-ttu-id="14625-108">Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo de loja de música MVC do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="14625-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="14625-109">A parte 4 abrange modelos e acesso a dados.</span><span class="sxs-lookup"><span data-stu-id="14625-109">Part 4 covers Models and Data Access.</span></span>

<span data-ttu-id="14625-110">Até agora, acabamos passando "dados fictícios" de nossos controladores para nossos modelos de exibição.</span><span class="sxs-lookup"><span data-stu-id="14625-110">So far, we've just been passing "dummy data" from our Controllers to our View templates.</span></span> <span data-ttu-id="14625-111">Agora estamos prontos para conectar um banco de dados real.</span><span class="sxs-lookup"><span data-stu-id="14625-111">Now we're ready to hook up a real database.</span></span> <span data-ttu-id="14625-112">Neste tutorial, abordaremos como usar o SQL Server Compact Edition (geralmente chamado de SQL CE) como nosso mecanismo de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="14625-112">In this tutorial we'll be covering how to use SQL Server Compact Edition (often called SQL CE) as our database engine.</span></span> <span data-ttu-id="14625-113">O SQL CE é um banco de dados baseado em arquivo, inserido e gratuito que não requer instalação ou configuração, o que o torna muito conveniente para o desenvolvimento local.</span><span class="sxs-lookup"><span data-stu-id="14625-113">SQL CE is a free, embedded, file based database that doesn't require any installation or configuration, which makes it really convenient for local development.</span></span>

## <a name="database-access-with-entity-framework-code-first"></a><span data-ttu-id="14625-114">Acesso ao banco de dados com Entity Framework Code-First</span><span class="sxs-lookup"><span data-stu-id="14625-114">Database access with Entity Framework Code-First</span></span>

<span data-ttu-id="14625-115">Usaremos o suporte do Entity Framework (EF) que está incluído nos projetos do ASP.NET MVC 3 para consultar e atualizar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="14625-115">We'll use the Entity Framework (EF) support that is included in ASP.NET MVC 3 projects to query and update the database.</span></span> <span data-ttu-id="14625-116">O EF é uma API de dados de ORM (mapeamento relacional de objeto) flexível que permite aos desenvolvedores consultar e atualizar os dados armazenados em um banco de dado de forma orientada a objeto.</span><span class="sxs-lookup"><span data-stu-id="14625-116">EF is a flexible object relational mapping (ORM) data API that enables developers to query and update data stored in a database in an object-oriented way.</span></span>

<span data-ttu-id="14625-117">Entity Framework versão 4 dá suporte a um paradigma de desenvolvimento chamado Code-First.</span><span class="sxs-lookup"><span data-stu-id="14625-117">Entity Framework version 4 supports a development paradigm called code-first.</span></span> <span data-ttu-id="14625-118">O Code-First permite que você crie um objeto de modelo escrevendo classes simples (também conhecidas como POCO de objetos CLR "Plain-Old") e pode até mesmo criar o banco de dados em tempo real a partir de suas classes.</span><span class="sxs-lookup"><span data-stu-id="14625-118">Code-first allows you to create model object by writing simple classes (also known as POCO from "plain-old" CLR objects), and can even create the database on the fly from your classes.</span></span>

### <a name="changes-to-our-model-classes"></a><span data-ttu-id="14625-119">Alterações em nossas classes de modelo</span><span class="sxs-lookup"><span data-stu-id="14625-119">Changes to our Model Classes</span></span>

<span data-ttu-id="14625-120">Usaremos o recurso de criação de banco de dados no Entity Framework neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="14625-120">We will be leveraging the database creation feature in Entity Framework in this tutorial.</span></span> <span data-ttu-id="14625-121">No entanto, antes de fazermos isso, vamos fazer algumas pequenas alterações em nossas classes de modelo para adicionar algumas coisas que usaremos posteriormente.</span><span class="sxs-lookup"><span data-stu-id="14625-121">Before we do that, though, let's make a few minor changes to our model classes to add in some things we'll be using later on.</span></span>

#### <a name="adding-the-artist-model-classes"></a><span data-ttu-id="14625-122">Adicionando as classes de modelo do artista</span><span class="sxs-lookup"><span data-stu-id="14625-122">Adding the Artist Model Classes</span></span>

<span data-ttu-id="14625-123">Nossos álbuns serão associados a artistas. portanto, vamos adicionar uma classe de modelo simples para descrever um artista.</span><span class="sxs-lookup"><span data-stu-id="14625-123">Our Albums will be associated with Artists, so we'll add a simple model class to describe an Artist.</span></span> <span data-ttu-id="14625-124">Adicione uma nova classe à pasta modelos chamada Artist.cs usando o código mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="14625-124">Add a new class to the Models folder named Artist.cs using the code shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a><span data-ttu-id="14625-125">Atualizando nossas classes de modelo</span><span class="sxs-lookup"><span data-stu-id="14625-125">Updating our Model Classes</span></span>

<span data-ttu-id="14625-126">Atualize a classe de álbum, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="14625-126">Update the Album class as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

<span data-ttu-id="14625-127">Em seguida, faça as seguintes atualizações para a classe gênero.</span><span class="sxs-lookup"><span data-stu-id="14625-127">Next, make the following updates to the Genre class.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-app_data-folder"></a><span data-ttu-id="14625-128">Adicionando a pasta de dados do\_de aplicativos</span><span class="sxs-lookup"><span data-stu-id="14625-128">Adding the App\_Data folder</span></span>

<span data-ttu-id="14625-129">Adicionaremos um diretório de dados do\_de aplicativos ao nosso projeto para manter nossos arquivos de banco de SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="14625-129">We'll add an App\_Data directory to our project to hold our SQL Server Express database files.</span></span> <span data-ttu-id="14625-130">Os dados de\_de aplicativo são um diretório especial no ASP.NET que já tem as permissões de acesso de segurança corretas para acesso ao banco de dado.</span><span class="sxs-lookup"><span data-stu-id="14625-130">App\_Data is a special directory in ASP.NET which already has the correct security access permissions for database access.</span></span> <span data-ttu-id="14625-131">No menu projeto, selecione Adicionar pasta ASP.NET e, em seguida,\_dados do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="14625-131">From the Project menu, select Add ASP.NET Folder, then App\_Data.</span></span>

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a><span data-ttu-id="14625-132">Criando uma cadeia de conexão no arquivo Web. config</span><span class="sxs-lookup"><span data-stu-id="14625-132">Creating a Connection String in the web.config file</span></span>

<span data-ttu-id="14625-133">Adicionaremos algumas linhas ao arquivo de configuração do site para que Entity Framework saiba como se conectar ao nosso banco de dados.</span><span class="sxs-lookup"><span data-stu-id="14625-133">We will add a few lines to the website's configuration file so that Entity Framework knows how to connect to our database.</span></span> <span data-ttu-id="14625-134">Clique duas vezes no arquivo Web. config localizado na raiz do projeto.</span><span class="sxs-lookup"><span data-stu-id="14625-134">Double-click on the Web.config file located in the root of the project.</span></span>

![](mvc-music-store-part-4/_static/image2.png)

<span data-ttu-id="14625-135">Role até a parte inferior deste arquivo e adicione um &lt;connectionStrings&gt; seção diretamente acima da última linha, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="14625-135">Scroll to the bottom of this file and add a &lt;connectionStrings&gt; section directly above the last line, as shown below.</span></span>

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a><span data-ttu-id="14625-136">Adicionando uma classe de contexto</span><span class="sxs-lookup"><span data-stu-id="14625-136">Adding a Context Class</span></span>

<span data-ttu-id="14625-137">Clique com o botão direito do mouse na pasta modelos e adicione uma nova classe chamada MusicStoreEntities.cs.</span><span class="sxs-lookup"><span data-stu-id="14625-137">Right-click the Models folder and add a new class named MusicStoreEntities.cs.</span></span>

![](mvc-music-store-part-4/_static/image3.png)

<span data-ttu-id="14625-138">Essa classe representará o contexto de banco de dados Entity Framework e tratará nossas operações de criação, leitura, atualização e exclusão para nós.</span><span class="sxs-lookup"><span data-stu-id="14625-138">This class will represent the Entity Framework database context, and will handle our create, read, update, and delete operations for us.</span></span> <span data-ttu-id="14625-139">O código para essa classe é mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="14625-139">The code for this class is shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

<span data-ttu-id="14625-140">Isso é isso – não há outra configuração, interfaces especiais, etc. Ao estender a classe base DbContext, nossa classe MusicStoreEntities é capaz de lidar com nossas operações de banco de dados para nós.</span><span class="sxs-lookup"><span data-stu-id="14625-140">That's it - there's no other configuration, special interfaces, etc. By extending the DbContext base class, our MusicStoreEntities class is able to handle our database operations for us.</span></span> <span data-ttu-id="14625-141">Agora que temos sido conectados, vamos adicionar mais algumas propriedades às nossas classes de modelo para aproveitar algumas das informações adicionais em nosso banco de dados.</span><span class="sxs-lookup"><span data-stu-id="14625-141">Now that we've got that hooked up, let's add a few more properties to our model classes to take advantage of some of the additional information in our database.</span></span>

### <a name="adding-our-store-catalog-data"></a><span data-ttu-id="14625-142">Adicionando os dados do catálogo da loja</span><span class="sxs-lookup"><span data-stu-id="14625-142">Adding our store catalog data</span></span>

<span data-ttu-id="14625-143">Aproveitaremos um recurso no Entity Framework que adiciona dados de "semente" a um banco de dado recém-criado.</span><span class="sxs-lookup"><span data-stu-id="14625-143">We will take advantage of a feature in Entity Framework which adds "seed" data to a newly created database.</span></span> <span data-ttu-id="14625-144">Isso preencherá previamente nosso catálogo de lojas com uma lista de gêneros, artistas e álbuns.</span><span class="sxs-lookup"><span data-stu-id="14625-144">This will pre-populate our store catalog with a list of Genres, Artists, and Albums.</span></span> <span data-ttu-id="14625-145">O download de MvcMusicStore-Assets. zip-que incluía nossos arquivos de design de site usados anteriormente neste tutorial – tem um arquivo de classe com esses dados de semente, localizado em uma pasta chamada Code.</span><span class="sxs-lookup"><span data-stu-id="14625-145">The MvcMusicStore-Assets.zip download - which included our site design files used earlier in this tutorial - has a class file with this seed data, located in a folder named Code.</span></span>

<span data-ttu-id="14625-146">Na pasta Code/Models, localize o arquivo SampleData.cs e solte-o na pasta modelos em nosso projeto, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="14625-146">Within the Code / Models folder, locate the SampleData.cs file and drop it into the Models folder in our project, as shown below.</span></span>

![](mvc-music-store-part-4/_static/image4.png)

<span data-ttu-id="14625-147">Agora precisamos adicionar uma linha de código para informar Entity Framework sobre essa classe SampleData.</span><span class="sxs-lookup"><span data-stu-id="14625-147">Now we need to add one line of code to tell Entity Framework about that SampleData class.</span></span> <span data-ttu-id="14625-148">Clique duas vezes no arquivo global. asax na raiz do projeto para abri-lo e adicione a seguinte linha à parte superior do método de início do aplicativo\_.</span><span class="sxs-lookup"><span data-stu-id="14625-148">Double-click on the Global.asax file in the root of the project to open it and add the following line to the top the Application\_Start method.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

<span data-ttu-id="14625-149">Neste ponto, concluimos o trabalho necessário para configurar Entity Framework para o nosso projeto.</span><span class="sxs-lookup"><span data-stu-id="14625-149">At this point, we've completed the work necessary to configure Entity Framework for our project.</span></span>

## <a name="querying-the-database"></a><span data-ttu-id="14625-150">Consultando o banco de dados</span><span class="sxs-lookup"><span data-stu-id="14625-150">Querying the Database</span></span>

<span data-ttu-id="14625-151">Agora, vamos atualizar nosso StoreController para que, em vez de usar "dados fictícios", em vez disso, ele chame nosso banco de dados para consultar todas as suas informações.</span><span class="sxs-lookup"><span data-stu-id="14625-151">Now let's update our StoreController so that instead of using "dummy data" it instead calls into our database to query all of its information.</span></span> <span data-ttu-id="14625-152">Vamos começar declarando um campo no **StoreController** para manter uma instância da classe MusicStoreEntities, chamada storeDB:</span><span class="sxs-lookup"><span data-stu-id="14625-152">We'll start by declaring a field on the **StoreController** to hold an instance of the MusicStoreEntities class, named storeDB:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a><span data-ttu-id="14625-153">Atualizando o índice de repositório para consultar o banco de dados</span><span class="sxs-lookup"><span data-stu-id="14625-153">Updating the Store Index to query the database</span></span>

<span data-ttu-id="14625-154">A classe MusicStoreEntities é mantida pelo Entity Framework e expõe uma propriedade de coleção para cada tabela em nosso banco de dados.</span><span class="sxs-lookup"><span data-stu-id="14625-154">The MusicStoreEntities class is maintained by the Entity Framework and exposes a collection property for each table in our database.</span></span> <span data-ttu-id="14625-155">Vamos atualizar nossa ação de índice do StoreController para recuperar todos os gêneros em nosso banco de dados.</span><span class="sxs-lookup"><span data-stu-id="14625-155">Let's update our StoreController's Index action to retrieve all Genres in our database.</span></span> <span data-ttu-id="14625-156">Anteriormente, fizemos isso por dados de cadeia de caracteres embutidos em código.</span><span class="sxs-lookup"><span data-stu-id="14625-156">Previously we did this by hard-coding string data.</span></span> <span data-ttu-id="14625-157">Agora, podemos simplesmente usar a coleção de Generes de contexto de Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="14625-157">Now we can instead just use the Entity Framework context Generes collection:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

<span data-ttu-id="14625-158">Nenhuma alteração precisa acontecer ao nosso modelo de exibição, já que ainda estamos retornando o mesmo StoreIndexViewModel que retornamos antes-estamos apenas retornando dados dinâmicos do nosso banco de dado agora.</span><span class="sxs-lookup"><span data-stu-id="14625-158">No changes need to happen to our View template since we're still returning the same StoreIndexViewModel we returned before - we're just returning live data from our database now.</span></span>

<span data-ttu-id="14625-159">Quando executarmos o projeto novamente e visitar a URL "/Store", agora veremos uma lista de todos os gêneros em nosso banco de dados:</span><span class="sxs-lookup"><span data-stu-id="14625-159">When we run the project again and visit the "/Store" URL, we'll now see a list of all Genres in our database:</span></span>

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a><span data-ttu-id="14625-160">Atualizando a procura e os detalhes do repositório para usar dados dinâmicos</span><span class="sxs-lookup"><span data-stu-id="14625-160">Updating Store Browse and Details to use live data</span></span>

<span data-ttu-id="14625-161">Com o método de ação/Store/Browse? gênero = *[some-gênero]* , Estamos pesquisando um gênero por nome.</span><span class="sxs-lookup"><span data-stu-id="14625-161">With the /Store/Browse?genre=*[some-genre]* action method, we're searching for a Genre by name.</span></span> <span data-ttu-id="14625-162">Esperamos apenas um resultado, já que não devemos ter duas entradas para o mesmo nome de gênero e, portanto, podemos usar o. Extensão single () em LINQ para consultar o objeto de gênero apropriado como este (não digite isso ainda):</span><span class="sxs-lookup"><span data-stu-id="14625-162">We only expect one result, since we shouldn't ever have two entries for the same Genre name, and so we can use the .Single() extension in LINQ to query for the appropriate Genre object like this (don't type this yet):</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

<span data-ttu-id="14625-163">O método único usa uma expressão lambda como um parâmetro, que especifica que queremos um único objeto de gênero, de modo que seu nome corresponda ao valor que definimos.</span><span class="sxs-lookup"><span data-stu-id="14625-163">The Single method takes a Lambda expression as a parameter, which specifies that we want a single Genre object such that its name matches the value we've defined.</span></span> <span data-ttu-id="14625-164">No caso acima, estamos carregando um único objeto de gênero com um valor de nome correspondente ao disco.</span><span class="sxs-lookup"><span data-stu-id="14625-164">In the case above, we are loading a single Genre object with a Name value matching Disco.</span></span>

<span data-ttu-id="14625-165">Aproveitaremos um recurso Entity Framework que nos permite indicar outras entidades relacionadas que desejamos carregar também quando o objeto gênero for recuperado.</span><span class="sxs-lookup"><span data-stu-id="14625-165">We'll take advantage of an Entity Framework feature that allows us to indicate other related entities we want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="14625-166">Esse recurso é chamado de modelagem de resultados de consulta e nos permite reduzir o número de vezes que precisamos acessar o banco de dados para recuperar todas as informações necessárias.</span><span class="sxs-lookup"><span data-stu-id="14625-166">This feature is called Query Result Shaping, and enables us to reduce the number of times we need to access the database to retrieve all of the information we need.</span></span> <span data-ttu-id="14625-167">Queremos buscar previamente os álbuns do gênero que recuperamos, portanto, atualizaremos nossa consulta para incluir de gêneros. include ("álbuns") para indicar que também queremos álbuns relacionados.</span><span class="sxs-lookup"><span data-stu-id="14625-167">We want to pre-fetch the Albums for Genre we retrieve, so we'll update our query to include from Genres.Include("Albums") to indicate that we want related albums as well.</span></span> <span data-ttu-id="14625-168">Isso é mais eficiente, já que ele recuperará nossos dados de gênero e de álbum em uma única solicitação de banco de dado.</span><span class="sxs-lookup"><span data-stu-id="14625-168">This is more efficient, since it will retrieve both our Genre and Album data in a single database request.</span></span>

<span data-ttu-id="14625-169">Com as explicações desatualizadas, veja como é a nossa ação atualizada do controlador de procura:</span><span class="sxs-lookup"><span data-stu-id="14625-169">With the explanations out of the way, here's how our updated Browse controller action looks:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

<span data-ttu-id="14625-170">Agora podemos atualizar a exibição de navegação da loja para exibir os álbuns que estão disponíveis em cada gênero.</span><span class="sxs-lookup"><span data-stu-id="14625-170">We can now update the Store Browse View to display the albums which are available in each Genre.</span></span> <span data-ttu-id="14625-171">Abra o modelo de exibição (encontrado em/Views/Store/Browse.cshtml) e adicione uma lista com marcadores de álbuns, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="14625-171">Open the view template (found in /Views/Store/Browse.cshtml) and add a bulleted list of Albums as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

<span data-ttu-id="14625-172">Executar nosso aplicativo e navegar até/Store/Browse? gênero = jazz mostra que nossos resultados agora estão sendo extraídos do banco de dados, exibindo todos os álbuns em nosso gênero selecionado.</span><span class="sxs-lookup"><span data-stu-id="14625-172">Running our application and browsing to /Store/Browse?genre=Jazz shows that our results are now being pulled from the database, displaying all albums in our selected Genre.</span></span>

![](mvc-music-store-part-4/_static/image2.jpg)

<span data-ttu-id="14625-173">Faremos a mesma alteração em nossa URL de/Store/Details/[ID] e substituíremos nossos dados fictícios por uma consulta de banco que carrega um álbum cuja ID corresponde ao valor do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="14625-173">We'll make the same change to our /Store/Details/[id] URL, and replace our dummy data with a database query which loads an Album whose ID matches the parameter value.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

<span data-ttu-id="14625-174">Executar nosso aplicativo e navegar até/Store/Details/1 mostra que nossos resultados agora estão sendo extraídos do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="14625-174">Running our application and browsing to /Store/Details/1 shows that our results are now being pulled from the database.</span></span>

![](mvc-music-store-part-4/_static/image5.png)

<span data-ttu-id="14625-175">Agora que a página de detalhes da loja está configurada para exibir um álbum pela ID do álbum, vamos atualizar o modo de exibição de **navegação** para vincular ao modo de exibição de detalhes.</span><span class="sxs-lookup"><span data-stu-id="14625-175">Now that our Store Details page is set up to display an album by the Album ID, let's update the **Browse** view to link to the Details view.</span></span> <span data-ttu-id="14625-176">Usaremos HTML. ActionLink, exatamente como fizemos para vincular do índice de repositório ao repositório de navegação no final da seção anterior.</span><span class="sxs-lookup"><span data-stu-id="14625-176">We will use Html.ActionLink, exactly as we did to link from Store Index to Store Browse at the end of the previous section.</span></span> <span data-ttu-id="14625-177">A origem completa do modo de exibição de navegação aparece abaixo.</span><span class="sxs-lookup"><span data-stu-id="14625-177">The complete source for the Browse view appears below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

<span data-ttu-id="14625-178">Agora podemos navegar de nossa página de armazenamento para uma página de gênero, que lista os álbuns disponíveis e, ao clicar em um álbum, podemos exibir detalhes para esse álbum.</span><span class="sxs-lookup"><span data-stu-id="14625-178">We're now able to browse from our Store page to a Genre page, which lists the available albums, and by clicking on an album we can view details for that album.</span></span>

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> <span data-ttu-id="14625-179">[Anterior](mvc-music-store-part-3.md)
> [Próximo](mvc-music-store-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="14625-179">[Previous](mvc-music-store-part-3.md)
[Next](mvc-music-store-part-5.md)</span></span>
