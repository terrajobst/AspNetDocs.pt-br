---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: Criar o cliente JavaScript | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: 74f2cc4e5e401d690042b05b028dfc0c46ae282a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622342"
---
# <a name="create-the-javascript-client"></a><span data-ttu-id="3f648-102">Criar o cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="3f648-102">Create the JavaScript Client</span></span>

<span data-ttu-id="3f648-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3f648-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="3f648-104">Baixar projeto concluído</span><span class="sxs-lookup"><span data-stu-id="3f648-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="3f648-105">Nesta seção, você criará o cliente para o aplicativo, usando HTML, JavaScript e a biblioteca [knockout. js](http://knockoutjs.com/) .</span><span class="sxs-lookup"><span data-stu-id="3f648-105">In this section, you will create the client for the application, using HTML, JavaScript, and the [Knockout.js](http://knockoutjs.com/) library.</span></span> <span data-ttu-id="3f648-106">Vamos criar o aplicativo cliente em estágios:</span><span class="sxs-lookup"><span data-stu-id="3f648-106">We'll build the client app in stages:</span></span>

- <span data-ttu-id="3f648-107">Mostrando uma lista de livros.</span><span class="sxs-lookup"><span data-stu-id="3f648-107">Showing a list of books.</span></span>
- <span data-ttu-id="3f648-108">Mostrando um detalhe do livro.</span><span class="sxs-lookup"><span data-stu-id="3f648-108">Showing a book detail.</span></span>
- <span data-ttu-id="3f648-109">Adicionando um novo livro.</span><span class="sxs-lookup"><span data-stu-id="3f648-109">Adding a new book.</span></span>

<span data-ttu-id="3f648-110">A biblioteca de Knockout usa o padrão Model-View-ViewModel (MVVM):</span><span class="sxs-lookup"><span data-stu-id="3f648-110">The Knockout library uses the Model-View-ViewModel (MVVM) pattern:</span></span>

- <span data-ttu-id="3f648-111">O **modelo** é a representação do lado do servidor dos dados no domínio de negócios (no nosso caso, livros e autores).</span><span class="sxs-lookup"><span data-stu-id="3f648-111">The **model** is the server-side representation of the data in the business domain (in our case, books and authors).</span></span>
- <span data-ttu-id="3f648-112">A **exibição** é a camada de apresentação (HTML).</span><span class="sxs-lookup"><span data-stu-id="3f648-112">The **view** is the presentation layer (HTML).</span></span>
- <span data-ttu-id="3f648-113">O **modelo de exibição** é um objeto JavaScript que contém os modelos.</span><span class="sxs-lookup"><span data-stu-id="3f648-113">The **view model** is a JavaScript object that holds the models.</span></span> <span data-ttu-id="3f648-114">O modelo de exibição é uma abstração de código da interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="3f648-114">The view model is a code abstraction of the UI.</span></span> <span data-ttu-id="3f648-115">Ele não tem conhecimento da representação HTML.</span><span class="sxs-lookup"><span data-stu-id="3f648-115">It has no knowledge of the HTML representation.</span></span> <span data-ttu-id="3f648-116">Em vez disso, ele representa recursos abstratos da exibição, como &quot;uma lista de livros&quot;.</span><span class="sxs-lookup"><span data-stu-id="3f648-116">Instead, it represents abstract features of the view, such as &quot;a list of books&quot;.</span></span>

<span data-ttu-id="3f648-117">A exibição é associada a dados para o modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="3f648-117">The view is data-bound to the view model.</span></span> <span data-ttu-id="3f648-118">As atualizações para o modelo de exibição são refletidas automaticamente na exibição.</span><span class="sxs-lookup"><span data-stu-id="3f648-118">Updates to the view model are automatically reflected in the view.</span></span> <span data-ttu-id="3f648-119">O modelo de exibição também obtém eventos da exibição, como cliques de botão.</span><span class="sxs-lookup"><span data-stu-id="3f648-119">The view model also gets events from the view, such as button clicks.</span></span>

![](part-6/_static/image1.png)

<span data-ttu-id="3f648-120">Essa abordagem facilita a alteração do layout e da interface do usuário do seu aplicativo, pois você pode alterar as associações sem reescrever nenhum código.</span><span class="sxs-lookup"><span data-stu-id="3f648-120">This approach makes it easy to change the layout and UI of your app, because you can change the bindings, without rewriting any code.</span></span> <span data-ttu-id="3f648-121">Por exemplo, você pode mostrar uma lista de itens como um `<ul>`e, em seguida, alterá-lo mais tarde para uma tabela.</span><span class="sxs-lookup"><span data-stu-id="3f648-121">For example, you might show a list of items as a `<ul>`, then change it later to a table.</span></span>

## <a name="add-the-knockout-library"></a><span data-ttu-id="3f648-122">Adicionar a biblioteca de Knockouts</span><span class="sxs-lookup"><span data-stu-id="3f648-122">Add the Knockout Library</span></span>

<span data-ttu-id="3f648-123">No Visual Studio, no menu **ferramentas** , selecione **Gerenciador de pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="3f648-123">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**.</span></span> <span data-ttu-id="3f648-124">Em seguida, selecione o **Console do Gerenciador de Pacotes**.</span><span class="sxs-lookup"><span data-stu-id="3f648-124">Then select **Package Manager Console**.</span></span> <span data-ttu-id="3f648-125">Na janela Console do Gerenciador de Pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="3f648-125">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-6/samples/sample1.cmd)]

<span data-ttu-id="3f648-126">Esse comando adiciona os arquivos de Knockout à pasta scripts.</span><span class="sxs-lookup"><span data-stu-id="3f648-126">This command adds the Knockout files to the Scripts folder.</span></span>

## <a name="create-the-view-model"></a><span data-ttu-id="3f648-127">Criar o modelo de exibição</span><span class="sxs-lookup"><span data-stu-id="3f648-127">Create the View Model</span></span>

<span data-ttu-id="3f648-128">Adicione um arquivo JavaScript chamado App. js à pasta scripts.</span><span class="sxs-lookup"><span data-stu-id="3f648-128">Add a JavaScript file named app.js to the Scripts folder.</span></span> <span data-ttu-id="3f648-129">(Em Gerenciador de Soluções, clique com o botão direito do mouse na pasta scripts, selecione **Adicionar**e, em seguida, selecione **arquivo JavaScript**.) Cole o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="3f648-129">(In Solution Explorer, right-click the Scripts folder, select **Add**, then select **JavaScript File**.) Paste in the following code:</span></span>

[!code-javascript[Main](part-6/samples/sample2.js)]

<span data-ttu-id="3f648-130">No knockout, a classe `observable` permite a vinculação de dados.</span><span class="sxs-lookup"><span data-stu-id="3f648-130">In Knockout, the `observable` class enables data-binding.</span></span> <span data-ttu-id="3f648-131">Quando o conteúdo de uma alteração observável, o observável notifica todos os controles vinculados a dados para que eles possam se atualizar.</span><span class="sxs-lookup"><span data-stu-id="3f648-131">When the contents of an observable change, the observable notifies all of the data-bound controls, so they can update themselves.</span></span> <span data-ttu-id="3f648-132">(A classe `observableArray` é a versão de matriz de *Observ*.) Para começar, nosso modelo de exibição tem dois observáveis:</span><span class="sxs-lookup"><span data-stu-id="3f648-132">(The `observableArray` class is the array version of *observable*.) To start with, our view model has two observables:</span></span>

- <span data-ttu-id="3f648-133">`books` mantém a lista de livros.</span><span class="sxs-lookup"><span data-stu-id="3f648-133">`books` holds the list of books.</span></span>
- <span data-ttu-id="3f648-134">`error` contém uma mensagem de erro se uma chamada AJAX falhar.</span><span class="sxs-lookup"><span data-stu-id="3f648-134">`error` contains an error message if an AJAX call fails.</span></span>

<span data-ttu-id="3f648-135">O método `getAllBooks` faz uma chamada AJAX para obter a lista de livros.</span><span class="sxs-lookup"><span data-stu-id="3f648-135">The `getAllBooks` method makes an AJAX call to get the list of books.</span></span> <span data-ttu-id="3f648-136">Em seguida, ele envia o resultado para a matriz de `books`.</span><span class="sxs-lookup"><span data-stu-id="3f648-136">Then it pushes the result onto the `books` array.</span></span>

<span data-ttu-id="3f648-137">O método `ko.applyBindings` faz parte da biblioteca de Knockouts.</span><span class="sxs-lookup"><span data-stu-id="3f648-137">The `ko.applyBindings` method is part of the Knockout library.</span></span> <span data-ttu-id="3f648-138">Ele usa o modelo de exibição como um parâmetro e configura a vinculação de dados.</span><span class="sxs-lookup"><span data-stu-id="3f648-138">It takes the view model as a parameter and sets up the data binding.</span></span>

## <a name="add-a-script-bundle"></a><span data-ttu-id="3f648-139">Adicionar um pacote de script</span><span class="sxs-lookup"><span data-stu-id="3f648-139">Add a Script Bundle</span></span>

<span data-ttu-id="3f648-140">O agrupamento é um recurso do ASP.NET 4,5 que torna mais fácil combinar ou agrupar vários arquivos em um único arquivo.</span><span class="sxs-lookup"><span data-stu-id="3f648-140">Bundling is a feature in ASP.NET 4.5 that makes it easy to combine or bundle multiple files into a single file.</span></span> <span data-ttu-id="3f648-141">O agrupamento reduz o número de solicitações para o servidor, o que pode melhorar o tempo de carregamento da página.</span><span class="sxs-lookup"><span data-stu-id="3f648-141">Bundling reduces the number of requests to the server, which can improve page load time.</span></span>

<span data-ttu-id="3f648-142">Abra o aplicativo de arquivo\_Start/BundleConfig. cs.</span><span class="sxs-lookup"><span data-stu-id="3f648-142">Open the file App\_Start/BundleConfig.cs.</span></span> <span data-ttu-id="3f648-143">Adicione o código a seguir ao método RegisterBundles.</span><span class="sxs-lookup"><span data-stu-id="3f648-143">Add the following code to the RegisterBundles method.</span></span>

[!code-csharp[Main](part-6/samples/sample3.cs)]

> [!div class="step-by-step"]
> <span data-ttu-id="3f648-144">[Anterior](part-5.md)
> [Próximo](part-7.md)</span><span class="sxs-lookup"><span data-stu-id="3f648-144">[Previous](part-5.md)
[Next](part-7.md)</span></span>
