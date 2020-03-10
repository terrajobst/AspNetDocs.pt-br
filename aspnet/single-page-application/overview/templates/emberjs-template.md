---
uid: single-page-application/overview/templates/emberjs-template
title: Modelo de EmberJS | Microsoft Docs
author: xqiu
description: Modelo EmberJS
ms.author: riande
ms.date: 01/30/2013
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 1aefa46dd0841b1b06675409cc8a09f9a218d7ac
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578501"
---
# <a name="emberjs-template"></a><span data-ttu-id="bcf1b-103">Modelo EmberJS</span><span class="sxs-lookup"><span data-stu-id="bcf1b-103">EmberJS template</span></span>

<span data-ttu-id="bcf1b-104">por [Xinyang Qiu](https://github.com/xqiu)</span><span class="sxs-lookup"><span data-stu-id="bcf1b-104">by [Xinyang Qiu](https://github.com/xqiu)</span></span>

> <span data-ttu-id="bcf1b-105">O modelo MVC EmberJS é escrito por Nathan Totten, Thiago Santos e Xinyang Qiu.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-105">The EmberJS MVC Template is written by Nathan Totten, Thiago Santos, and Xinyang Qiu.</span></span>
> 
> [<span data-ttu-id="bcf1b-106">Baixar o modelo MVC do EmberJS</span><span class="sxs-lookup"><span data-stu-id="bcf1b-106">Download the EmberJS MVC Template</span></span>](https://go.microsoft.com/fwlink/?LinkId=282647)

<span data-ttu-id="bcf1b-107">O modelo SPA EmberJS foi projetado para começar a criar rapidamente aplicativos Web do lado do cliente interativos usando o EmberJS.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-107">The EmberJS SPA template is designed to get you started quickly building interactive client-side web apps using EmberJS.</span></span>

<span data-ttu-id="bcf1b-108">O "aplicativo de página única" (SPA) é o termo geral para um aplicativo Web que carrega uma única página HTML e, em seguida, atualiza a página dinamicamente, em vez de carregar novas páginas.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-108">"Single-page application" (SPA) is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="bcf1b-109">Após o carregamento inicial da página, o SPA se comunica com o servidor por meio de solicitações AJAX.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-109">After the initial page load, the SPA talks with the server through AJAX requests.</span></span>

![](emberjs-template/_static/image1.png)

<span data-ttu-id="bcf1b-110">O AJAX não é nada novo, mas hoje há estruturas de JavaScript que facilitam a criação e a manutenção de um aplicativo SPA sofisticado grande.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-110">AJAX is nothing new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="bcf1b-111">Além disso, o HTML 5 e o CSS3 estão facilitando a criação de UIs avançadas.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-111">Also, HTML 5 and CSS3 are making it easier to create rich UIs.</span></span>

<span data-ttu-id="bcf1b-112">O modelo SPA EmberJS usa a biblioteca [Ember](http://emberjs.com/) JavaScript para lidar com atualizações de página de solicitações Ajax.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-112">The EmberJS SPA Template uses the [Ember](http://emberjs.com/) JavaScript library to handle page updates from AJAX requests.</span></span> <span data-ttu-id="bcf1b-113">O Ember. js usa a vinculação de dados para sincronizar a página com os dados mais recentes.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-113">Ember.js uses data binding to synchronize the page with the latest data.</span></span> <span data-ttu-id="bcf1b-114">Dessa forma, você não precisa escrever nenhum código que percorra os dados JSON e atualize o DOM.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-114">That way, you don't have to write any of the code that walks through the JSON data and updates the DOM.</span></span> <span data-ttu-id="bcf1b-115">Em vez disso, você coloca atributos declarativos no HTML que dizem ao Ember. js como apresentar os dados.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-115">Instead, you put declarative attributes in the HTML that tell Ember.js how to present the data.</span></span>

<span data-ttu-id="bcf1b-116">No lado do servidor, o modelo EmberJS é quase idêntico ao [modelo Spa KnockoutJS](../introduction/knockoutjs-template.md).</span><span class="sxs-lookup"><span data-stu-id="bcf1b-116">On the server side, the EmberJS template is almost identical to the [KnockoutJS SPA template](../introduction/knockoutjs-template.md).</span></span> <span data-ttu-id="bcf1b-117">Ele usa o ASP.NET MVC para servir documentos HTML e ASP.NET Web API para lidar com solicitações AJAX do cliente.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-117">It uses ASP.NET MVC to serve HTML documents, and ASP.NET Web API to handle AJAX requests from the client.</span></span> <span data-ttu-id="bcf1b-118">Para obter mais informações sobre esses aspectos do modelo, consulte a documentação do [modelo KnockoutJS](../introduction/knockoutjs-template.md) .</span><span class="sxs-lookup"><span data-stu-id="bcf1b-118">For more information about those aspects of the template, refer to the [KnockoutJS template](../introduction/knockoutjs-template.md) documentation.</span></span> <span data-ttu-id="bcf1b-119">Este tópico se concentra nas diferenças entre o modelo de Knockout e o modelo EmberJS.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-119">This topic focuses on the differences between the Knockout template and the EmberJS template.</span></span>

## <a name="create-an-emberjs-spa-template-project"></a><span data-ttu-id="bcf1b-120">Criar um projeto de modelo SPA EmberJS</span><span class="sxs-lookup"><span data-stu-id="bcf1b-120">Create an EmberJS SPA Template Project</span></span>

<span data-ttu-id="bcf1b-121">Baixe e instale o modelo clicando no botão baixar acima.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-121">Download and install the template by clicking the Download button above.</span></span> <span data-ttu-id="bcf1b-122">Talvez seja necessário reiniciar o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-122">You might need to restart Visual Studio.</span></span>

<span data-ttu-id="bcf1b-123">No painel **modelos** , selecione **modelos instalados** e expanda o **nó C# Visual** .</span><span class="sxs-lookup"><span data-stu-id="bcf1b-123">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="bcf1b-124">Em **Visual C#** , selecione **Web**.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-124">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="bcf1b-125">Na lista de modelos de projeto, selecione **aplicativo Web ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-125">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="bcf1b-126">Nomeie o projeto e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-126">Name the project and click **OK**.</span></span>

![](emberjs-template/_static/image2.png)

<span data-ttu-id="bcf1b-127">No assistente de **novo projeto** , selecione **projeto Spa Ember. js**.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-127">In the **New Project** wizard, select **Ember.js SPA Project**.</span></span>

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a><span data-ttu-id="bcf1b-128">Visão geral do modelo SPA EmberJS</span><span class="sxs-lookup"><span data-stu-id="bcf1b-128">EmberJS SPA Template Overview</span></span>

<span data-ttu-id="bcf1b-129">O modelo EmberJS usa uma combinação de jQuery, Ember. js, Handlebars. js para criar uma interface do usuário tranqüila e interativa.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-129">The EmberJS template uses a combination of jQuery, Ember.js, Handlebars.js to create a smooth, interactive UI.</span></span>

<span data-ttu-id="bcf1b-130">Ember. js é uma biblioteca JavaScript que usa um padrão MVC do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-130">Ember.js is a JavaScript library that uses a client-side MVC pattern.</span></span>

- <span data-ttu-id="bcf1b-131">Um *modelo*, escrito na linguagem de modelagem Handlebars, descreve a interface do usuário do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-131">A *template*, written in the Handlebars templating language, describes the application user interface.</span></span> <span data-ttu-id="bcf1b-132">No modo de liberação, o [compilador Handlebars](https://github.com/Myslik/csharp-ember-handlebars) é usado para agrupar e compilar o modelo Handlebars.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-132">In release mode, the [Handlebars compiler](https://github.com/Myslik/csharp-ember-handlebars) is used to bundle and compile the handlebars template.</span></span>
- <span data-ttu-id="bcf1b-133">Um *modelo* armazena os dados do aplicativo que ele obtém do servidor (listas de tarefas pendentes e itens de tarefas).</span><span class="sxs-lookup"><span data-stu-id="bcf1b-133">A *model* stores the application data that it gets from the server (ToDo lists and ToDo items).</span></span>
- <span data-ttu-id="bcf1b-134">Um *controlador* armazena o estado do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-134">A *controller* stores application state.</span></span> <span data-ttu-id="bcf1b-135">Os controladores geralmente apresentam dados de modelo aos modelos correspondentes.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-135">Controllers often present model data to the corresponding templates.</span></span>
- <span data-ttu-id="bcf1b-136">Uma *exibição* traduz eventos primitivos do aplicativo e os passa para o controlador.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-136">A *view* translates primitive events from the application and passes these to the controller.</span></span>
- <span data-ttu-id="bcf1b-137">Um *roteador* gerencia o estado do aplicativo, mantendo URLs e modelos em sincronia.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-137">A *router* manages application state, keeping URLs and templates in sync.</span></span>

<span data-ttu-id="bcf1b-138">Além disso, a biblioteca de dados Ember pode ser usada para sincronizar objetos JSON (obtidos do servidor por meio de uma API RESTful) e os modelos de cliente.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-138">In addition, the Ember Data library can be used to synchronize JSON objects (obtained from the server through a RESTful API) and the client models.</span></span>

<span data-ttu-id="bcf1b-139">O modelo SPA EmberJS organiza os scripts em oito camadas:</span><span class="sxs-lookup"><span data-stu-id="bcf1b-139">The EmberJS SPA template organizes the scripts into eight layers:</span></span>

- <span data-ttu-id="bcf1b-140">webAPI\_Adapter. js, webAPI\_Serialization. js: estende a biblioteca de dados do Ember para trabalhar com ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-140">webapi\_adapter.js, webapi\_serializer.js: Extends the Ember Data library to work with ASP.NET Web API.</span></span>
- <span data-ttu-id="bcf1b-141">Scripts/Helpers. js: define novos auxiliares do Ember Handlebars.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-141">Scripts/helpers.js: Defines new Ember Handlebars helpers.</span></span>
- <span data-ttu-id="bcf1b-142">Scripts/app. js: cria o aplicativo e configura o adaptador e o serializador.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-142">Scripts/app.js: Creates the app and configures the adapter and serializer.</span></span>
- <span data-ttu-id="bcf1b-143">Scripts/app/Models/\*. js: define os modelos.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-143">Scripts/app/models/\*.js: Defines the models.</span></span>
- <span data-ttu-id="bcf1b-144">Scripts/app/views/\*. js: define as exibições.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-144">Scripts/app/views/\*.js: Defines the views.</span></span>
- <span data-ttu-id="bcf1b-145">Scripts/app/Controllers/\*. js: define os controladores.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-145">Scripts/app/controllers/\*.js: Defines the controllers.</span></span>
- <span data-ttu-id="bcf1b-146">Scripts/aplicativos/rotas, scripts/app/router. js: define as rotas.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-146">Scripts/app/routes, Scripts/app/router.js: Defines the routes.</span></span>
- <span data-ttu-id="bcf1b-147">Templates/\*. HBS: define os modelos de Handlebars.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-147">Templates/\*.hbs: Defines the handlebars templates.</span></span>

<span data-ttu-id="bcf1b-148">Vamos examinar alguns desses scripts mais detalhadamente.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-148">Let's look at some of these scripts in more detail.</span></span>

## <a name="models"></a><span data-ttu-id="bcf1b-149">Modelos</span><span class="sxs-lookup"><span data-stu-id="bcf1b-149">Models</span></span>

<span data-ttu-id="bcf1b-150">Os modelos são definidos na pasta scripts/aplicativos/modelos.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-150">The models are defined in the Scripts/app/models folder.</span></span> <span data-ttu-id="bcf1b-151">Há dois arquivos de modelo: todoItem. js e ToDoList. js.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-151">There are two model files: todoItem.js and todoList.js.</span></span>

<span data-ttu-id="bcf1b-152">**todo. Model. js** define os modelos do lado do cliente (navegador) para as listas de tarefas pendentes.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-152">**todo.model.js** defines the client-side (browser) models for the to-do lists.</span></span> <span data-ttu-id="bcf1b-153">Há duas classes de modelo: todoItem e ToDoList.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-153">There are two model classes: todoItem and todoList.</span></span> <span data-ttu-id="bcf1b-154">No Ember, os modelos são subclasses de DS. Deprecia.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-154">In Ember, models are subclasses of DS.Model.</span></span> <span data-ttu-id="bcf1b-155">Um modelo pode ter propriedades com atributos:</span><span class="sxs-lookup"><span data-stu-id="bcf1b-155">A model can have properties with attributes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

<span data-ttu-id="bcf1b-156">Os modelos podem definir relações com outros modelos:</span><span class="sxs-lookup"><span data-stu-id="bcf1b-156">Models can define relationships to other models:</span></span>

[!code-css[Main](emberjs-template/samples/sample2.css)]

<span data-ttu-id="bcf1b-157">Os modelos podem ter propriedades computadas que se associam a outras propriedades:</span><span class="sxs-lookup"><span data-stu-id="bcf1b-157">Models can have computed properties that bind to other properties:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

<span data-ttu-id="bcf1b-158">Os modelos podem ter funções de observador, que são invocadas quando uma propriedade observada é alterada:</span><span class="sxs-lookup"><span data-stu-id="bcf1b-158">Models can have observer functions, which are invoked when an observed property changes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a><span data-ttu-id="bcf1b-159">Exibições</span><span class="sxs-lookup"><span data-stu-id="bcf1b-159">Views</span></span>

<span data-ttu-id="bcf1b-160">As exibições são definidas na pasta scripts/aplicativos/exibições.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-160">The views are defined in the Scripts/app/views folder.</span></span> <span data-ttu-id="bcf1b-161">Uma exibição traduz eventos da interface do usuário do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-161">A view translates events from the application UI.</span></span> <span data-ttu-id="bcf1b-162">Um manipulador de eventos pode chamar de volta para funções de controlador ou simplesmente chamar o contexto de dados diretamente.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-162">An event handler can call back to controller functions, or simply call the data context directly.</span></span>

<span data-ttu-id="bcf1b-163">Por exemplo, o código a seguir é de views/TodoItemEditView. js.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-163">For example, the following code is from views/TodoItemEditView.js.</span></span> <span data-ttu-id="bcf1b-164">Ele define a manipulação de eventos para um campo de texto de entrada.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-164">It defines the event handling for an input text field.</span></span>

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a><span data-ttu-id="bcf1b-165">Controller</span><span class="sxs-lookup"><span data-stu-id="bcf1b-165">Controller</span></span>

<span data-ttu-id="bcf1b-166">Os controladores são definidos na pasta scripts/aplicativos/controladores.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-166">The controllers are defined in the Scripts/app/controllers folder.</span></span> <span data-ttu-id="bcf1b-167">Para representar um único modelo, estenda `Ember.ObjectController`:</span><span class="sxs-lookup"><span data-stu-id="bcf1b-167">To represent a single model, extend `Ember.ObjectController`:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

<span data-ttu-id="bcf1b-168">Um controlador também pode representar uma coleção de modelos estendendo `Ember.ArrayController`.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-168">A controller can also represent a collection of models by extending `Ember.ArrayController`.</span></span> <span data-ttu-id="bcf1b-169">Por exemplo, o TodoListController representa uma matriz de objetos `todoList`.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-169">For example, the TodoListController represents an array of `todoList` objects.</span></span> <span data-ttu-id="bcf1b-170">O controlador classifica por ID de ToDoList, em ordem decrescente:</span><span class="sxs-lookup"><span data-stu-id="bcf1b-170">The controller sorts by todoList ID, in descending order:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

<span data-ttu-id="bcf1b-171">O controlador define uma função chamada `addTodoList`, que cria uma nova lista de tarefas e a adiciona à matriz.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-171">The controller defines a function named `addTodoList`, which creates a new todoList and adds it to the array.</span></span> <span data-ttu-id="bcf1b-172">Para ver como essa função é chamada, abra o arquivo de modelo chamado todoListTemplate. html, na pasta modelos.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-172">To see how this function gets called, open the template file named todoListTemplate.html, in the Templates folder.</span></span> <span data-ttu-id="bcf1b-173">O código de modelo a seguir associa um botão à função `addTodoList`:</span><span class="sxs-lookup"><span data-stu-id="bcf1b-173">The following template code binds a button to the `addTodoList` function:</span></span>

[!code-html[Main](emberjs-template/samples/sample8.html)]

<span data-ttu-id="bcf1b-174">O controlador também contém uma propriedade `error`, que contém uma mensagem de erro.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-174">The controller also contains an `error` property, which holds an error message.</span></span> <span data-ttu-id="bcf1b-175">Aqui está o código de modelo para exibir a mensagem de erro (também em todoListTemplate. html):</span><span class="sxs-lookup"><span data-stu-id="bcf1b-175">Here is the template code to display the error message (also in todoListTemplate.html):</span></span>

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a><span data-ttu-id="bcf1b-176">Rotas</span><span class="sxs-lookup"><span data-stu-id="bcf1b-176">Routes</span></span>

<span data-ttu-id="bcf1b-177">O router. js define as rotas e o modelo padrão a ser exibido, configura o estado do aplicativo e corresponde às URLs às rotas:</span><span class="sxs-lookup"><span data-stu-id="bcf1b-177">Router.js defines the routes and the default template to display, sets up application state, and matches URLs to routes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

<span data-ttu-id="bcf1b-178">O TodoListRoute. js carrega os dados para o TodoListRoute substituindo a função setupController:</span><span class="sxs-lookup"><span data-stu-id="bcf1b-178">TodoListRoute.js loads data for the TodoListRoute by overriding the setupController function:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

<span data-ttu-id="bcf1b-179">O Ember usa convenções de nomenclatura para corresponder URLs, nomes de rotas, controladores e modelos.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-179">Ember uses naming conventions to match URLs, route names, controllers, and templates.</span></span> <span data-ttu-id="bcf1b-180">Para obter mais informações, consulte [http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) na documentação do EmberJS.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-180">For more information, see [http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) at the EmberJS documentation.</span></span>

## <a name="templates"></a><span data-ttu-id="bcf1b-181">Modelos</span><span class="sxs-lookup"><span data-stu-id="bcf1b-181">Templates</span></span>

<span data-ttu-id="bcf1b-182">A pasta modelos contém quatro modelos:</span><span class="sxs-lookup"><span data-stu-id="bcf1b-182">The Templates folder contains four templates:</span></span>

- <span data-ttu-id="bcf1b-183">Application. HBS: o modelo padrão que é renderizado quando o aplicativo é iniciado.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-183">application.hbs: The default template that is rendered when the application is started.</span></span>
- <span data-ttu-id="bcf1b-184">About. HBS: o modelo para a rota "/About".</span><span class="sxs-lookup"><span data-stu-id="bcf1b-184">about.hbs: The template for the "/about" route.</span></span>
- <span data-ttu-id="bcf1b-185">index. HBS: o modelo para a rota "/" raiz.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-185">index.hbs: The template for the root "/" route.</span></span>
- <span data-ttu-id="bcf1b-186">ToDoList. HBS: o modelo para a rota "/todo".</span><span class="sxs-lookup"><span data-stu-id="bcf1b-186">todoList.hbs: The template for the "/todo" route.</span></span>
- <span data-ttu-id="bcf1b-187">\_Navigation. HBS: o modelo define o menu de navegação.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-187">\_navbar.hbs: The template defines the navigation menu.</span></span>

<span data-ttu-id="bcf1b-188">O modelo de aplicativo funciona como uma página mestra.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-188">The application template acts like a master page.</span></span> <span data-ttu-id="bcf1b-189">Ele contém um cabeçalho, um rodapé e um "{{tomada}}" para inserir outros modelos, dependendo da rota.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-189">It contains a header, a footer, and an "{{outlet}}" to insert other templates in depending on the route.</span></span> <span data-ttu-id="bcf1b-190">Para obter mais informações sobre modelos de aplicativos no Ember, consulte [http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).</span><span class="sxs-lookup"><span data-stu-id="bcf1b-190">For more information about application templates in Ember, see [http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).</span></span>

<span data-ttu-id="bcf1b-191">O modelo "/todoList" contém duas expressões de loop.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-191">The "/todoList" template contains two loop expressions.</span></span> <span data-ttu-id="bcf1b-192">O loop externo é `{{#each controller}}`e o loop interno é `{{#each todos}}`.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-192">The outside loop is `{{#each controller}}`, and the inside loop is `{{#each todos}}`.</span></span> <span data-ttu-id="bcf1b-193">O código a seguir mostra uma exibição `Ember.Checkbox` interna, uma `App.TodoItemEditView`personalizada e um link com uma ação `deleteTodo`.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-193">The following code shows a built-in `Ember.Checkbox` view, a customized `App.TodoItemEditView`, and a link with a `deleteTodo` action.</span></span>

[!code-html[Main](emberjs-template/samples/sample12.html)]

<span data-ttu-id="bcf1b-194">A classe `HtmlHelperExtensions`, definida em Controllers/HtmlHelperExtensions. cs, define uma função auxiliar para armazenar em cache e inserir arquivos de modelo quando **debug** é definido como **true** no arquivo Web. config.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-194">The `HtmlHelperExtensions` class, defined in Controllers/HtmlHelperExtensions.cs, defines a helper function to cache and insert template files when **debug** is set to **true** in the Web.config file.</span></span> <span data-ttu-id="bcf1b-195">Essa função é chamada do arquivo de exibição ASP.NET MVC definido em views/home/app. cshtml:</span><span class="sxs-lookup"><span data-stu-id="bcf1b-195">This function is called from the ASP.NET MVC view file defined in Views/Home/App.cshtml:</span></span>

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

<span data-ttu-id="bcf1b-196">Chamado sem argumentos, a função renderiza todos os arquivos de modelo na pasta modelos.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-196">Called with no arguments, the function renders all of the template files in the Templates folder.</span></span> <span data-ttu-id="bcf1b-197">Você também pode especificar uma subpasta ou um arquivo de modelo específico.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-197">You can also specify a subfolder or a specific template file.</span></span>

<span data-ttu-id="bcf1b-198">Quando **debug** for **false** em Web. config, o aplicativo incluirá o item "~/Bundles/templates" do pacote.</span><span class="sxs-lookup"><span data-stu-id="bcf1b-198">When **debug** is **false** in Web.config, the application includes the bundle item "~/bundles/templates".</span></span> <span data-ttu-id="bcf1b-199">Esse item de pacote é adicionado em BundleConfig.cs, usando a biblioteca do compilador Handlebars:</span><span class="sxs-lookup"><span data-stu-id="bcf1b-199">This bundle item is added in BundleConfig.cs, using the Handlebars compiler library:</span></span>

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
