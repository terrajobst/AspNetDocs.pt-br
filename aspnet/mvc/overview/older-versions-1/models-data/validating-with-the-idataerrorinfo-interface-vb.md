---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
title: Validando com a interface IDataErrorInfo (VB) | Microsoft Docs
author: StephenWalther
description: Stephen Walther mostra como exibir mensagens de erro de validação personalizadas implementando a interface IDataErrorInfo em uma classe de modelo.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 3a8a9d9f-82dd-4959-b7c6-960e9ce95df1
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 8ff3adc5db8114dcca2c66d937e1706f0bac0d30
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78542500"
---
# <a name="validating-with-the-idataerrorinfo-interface-vb"></a><span data-ttu-id="ce6a2-103">Validação com a interface IDataErrorInfo (VB)</span><span class="sxs-lookup"><span data-stu-id="ce6a2-103">Validating with the IDataErrorInfo Interface (VB)</span></span>

<span data-ttu-id="ce6a2-104">por [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="ce6a2-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="ce6a2-105">Stephen Walther mostra como exibir mensagens de erro de validação personalizadas implementando a interface IDataErrorInfo em uma classe de modelo.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-105">Stephen Walther shows you how to display custom validation error messages by implementing the IDataErrorInfo interface in a model class.</span></span>

<span data-ttu-id="ce6a2-106">O objetivo deste tutorial é explicar uma abordagem para executar a validação em um aplicativo MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-106">The goal of this tutorial is to explain one approach to performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="ce6a2-107">Você aprende a impedir que alguém envie um formulário HTML sem fornecer valores para os campos de formulário necessários.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-107">You learn how to prevent someone from submitting an HTML form without providing values for required form fields.</span></span> <span data-ttu-id="ce6a2-108">Neste tutorial, você aprenderá a executar a validação usando a interface IErrorDataInfo.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-108">In this tutorial, you learn how to perform validation by using the IErrorDataInfo interface.</span></span>

## <a name="assumptions"></a><span data-ttu-id="ce6a2-109">Suposições</span><span class="sxs-lookup"><span data-stu-id="ce6a2-109">Assumptions</span></span>

<span data-ttu-id="ce6a2-110">Neste tutorial, usarei o banco de dados MoviesDB e a tabela de banco de dados de filmes.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-110">In this tutorial, I'll use the MoviesDB database and the Movies database table.</span></span> <span data-ttu-id="ce6a2-111">Essa tabela tem as seguintes colunas:</span><span class="sxs-lookup"><span data-stu-id="ce6a2-111">This table has the following columns:</span></span>

<a id="0.6_table01"></a>

| <span data-ttu-id="ce6a2-112">**Nome da Coluna**</span><span class="sxs-lookup"><span data-stu-id="ce6a2-112">**Column Name**</span></span> | <span data-ttu-id="ce6a2-113">**Tipo de Dados**</span><span class="sxs-lookup"><span data-stu-id="ce6a2-113">**Data Type**</span></span> | <span data-ttu-id="ce6a2-114">**Permitir Nulos**</span><span class="sxs-lookup"><span data-stu-id="ce6a2-114">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ce6a2-115">ID</span><span class="sxs-lookup"><span data-stu-id="ce6a2-115">Id</span></span> | <span data-ttu-id="ce6a2-116">Int</span><span class="sxs-lookup"><span data-stu-id="ce6a2-116">Int</span></span> | <span data-ttu-id="ce6a2-117">Falso</span><span class="sxs-lookup"><span data-stu-id="ce6a2-117">False</span></span> |
| <span data-ttu-id="ce6a2-118">Title</span><span class="sxs-lookup"><span data-stu-id="ce6a2-118">Title</span></span> | <span data-ttu-id="ce6a2-119">Nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="ce6a2-119">Nvarchar(100)</span></span> | <span data-ttu-id="ce6a2-120">Falso</span><span class="sxs-lookup"><span data-stu-id="ce6a2-120">False</span></span> |
| <span data-ttu-id="ce6a2-121">Diretor</span><span class="sxs-lookup"><span data-stu-id="ce6a2-121">Director</span></span> | <span data-ttu-id="ce6a2-122">Nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="ce6a2-122">Nvarchar(100)</span></span> | <span data-ttu-id="ce6a2-123">Falso</span><span class="sxs-lookup"><span data-stu-id="ce6a2-123">False</span></span> |
| <span data-ttu-id="ce6a2-124">DateReleased</span><span class="sxs-lookup"><span data-stu-id="ce6a2-124">DateReleased</span></span> | <span data-ttu-id="ce6a2-125">Datetime</span><span class="sxs-lookup"><span data-stu-id="ce6a2-125">DateTime</span></span> | <span data-ttu-id="ce6a2-126">Falso</span><span class="sxs-lookup"><span data-stu-id="ce6a2-126">False</span></span> |

<span data-ttu-id="ce6a2-127">Neste tutorial, uso o Microsoft Entity Framework para gerar minhas classes de modelo de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-127">In this tutorial, I use the Microsoft Entity Framework to generate my database model classes.</span></span> <span data-ttu-id="ce6a2-128">A classe de filme gerada pelo Entity Framework é exibida na Figura 1.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-128">The Movie class generated by the Entity Framework is displayed in Figure 1.</span></span>

<span data-ttu-id="ce6a2-129">[![a entidade de filme](validating-with-the-idataerrorinfo-interface-vb/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ce6a2-129">[![The Movie entity](validating-with-the-idataerrorinfo-interface-vb/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image1.png)</span></span>

<span data-ttu-id="ce6a2-130">**Figura 01**: a entidade Movie ([clique para exibir a imagem em tamanho normal](validating-with-the-idataerrorinfo-interface-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="ce6a2-130">**Figure 01**: The Movie entity([Click to view full-size image](validating-with-the-idataerrorinfo-interface-vb/_static/image2.png))</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="ce6a2-131">Para saber mais sobre como usar o Entity Framework para gerar suas classes de modelo de banco de dados, consulte meu tutorial intitulado criando classes de modelo com o Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-131">To learn more about using the Entity Framework to generate your database model classes, see my tutorial entitled Creating Model Classes with the Entity Framework.</span></span>

## <a name="the-controller-class"></a><span data-ttu-id="ce6a2-132">A classe do controlador</span><span class="sxs-lookup"><span data-stu-id="ce6a2-132">The Controller Class</span></span>

<span data-ttu-id="ce6a2-133">Usamos o controlador Home para listar filmes e criar novos filmes.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-133">We use the Home controller to list movies and create new movies.</span></span> <span data-ttu-id="ce6a2-134">O código para essa classe está contido na Listagem 1.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-134">The code for this class is contained in Listing 1.</span></span>

<span data-ttu-id="ce6a2-135">**Listagem 1-Controllers\HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="ce6a2-135">**Listing 1 - Controllers\HomeController.vb**</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample1.vb)]

<span data-ttu-id="ce6a2-136">A classe de controlador Home na Listagem 1 contém duas ações Create ().</span><span class="sxs-lookup"><span data-stu-id="ce6a2-136">The Home controller class in Listing 1 contains two Create() actions.</span></span> <span data-ttu-id="ce6a2-137">A primeira ação exibe o formulário HTML para criar um novo filme.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-137">The first action displays the HTML form for creating a new movie.</span></span> <span data-ttu-id="ce6a2-138">A segunda ação Create () executa a inserção real do novo filme no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-138">The second Create() action performs the actual insert of the new movie into the database.</span></span> <span data-ttu-id="ce6a2-139">A segunda ação criar () é invocada quando o formulário exibido pela primeira ação criar () é enviado ao servidor.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-139">The second Create() action is invoked when the form displayed by the first Create() action is submitted to the server.</span></span>

<span data-ttu-id="ce6a2-140">Observe que a segunda ação Create () contém as seguintes linhas de código:</span><span class="sxs-lookup"><span data-stu-id="ce6a2-140">Notice that the second Create() action contains the following lines of code:</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample2.vb)]

<span data-ttu-id="ce6a2-141">A Propriedade IsValid retorna false quando há um erro de validação.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-141">The IsValid property returns false when there is a validation error.</span></span> <span data-ttu-id="ce6a2-142">Nesse caso, a exibição criar que contém o formulário HTML para criar um filme é reexibida.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-142">In that case, the Create view that contains the HTML form for creating a movie is redisplayed.</span></span>

## <a name="creating-a-partial-class"></a><span data-ttu-id="ce6a2-143">Criando uma classe parcial</span><span class="sxs-lookup"><span data-stu-id="ce6a2-143">Creating a Partial Class</span></span>

<span data-ttu-id="ce6a2-144">A classe de filme é gerada pelo Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-144">The Movie class is generated by the Entity Framework.</span></span> <span data-ttu-id="ce6a2-145">Você poderá ver o código da classe Movie se expandir o arquivo MoviesDBModel. edmx na janela Gerenciador de Soluções e abrir o arquivo MoviesDBModel. designer. vb no editor de código (consulte a Figura 2).</span><span class="sxs-lookup"><span data-stu-id="ce6a2-145">You can see the code for the Movie class if you expand the MoviesDBModel.edmx file in the Solution Explorer window and open the MoviesDBModel.Designer.vb file in the Code Editor (see Figure 2).</span></span>

<span data-ttu-id="ce6a2-146">[![o código para a entidade de filme](validating-with-the-idataerrorinfo-interface-vb/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="ce6a2-146">[![The code for the Movie entity](validating-with-the-idataerrorinfo-interface-vb/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image3.png)</span></span>

<span data-ttu-id="ce6a2-147">**Figura 02**: o código para a entidade de filme ([clique para exibir a imagem em tamanho normal](validating-with-the-idataerrorinfo-interface-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="ce6a2-147">**Figure 02**: The code for the Movie entity([Click to view full-size image](validating-with-the-idataerrorinfo-interface-vb/_static/image4.png))</span></span>

<span data-ttu-id="ce6a2-148">A classe Movie é uma classe parcial.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-148">The Movie class is a partial class.</span></span> <span data-ttu-id="ce6a2-149">Isso significa que podemos adicionar outra classe parcial com o mesmo nome para estender a funcionalidade da classe Movie.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-149">That means that we can add another partial class with the same name to extend the functionality of the Movie class.</span></span> <span data-ttu-id="ce6a2-150">Vamos adicionar nossa lógica de validação à nova classe parcial.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-150">We'll add our validation logic to the new partial class.</span></span>

<span data-ttu-id="ce6a2-151">Adicione a classe na Listagem 2 à pasta modelos.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-151">Add the class in Listing 2 to the Models folder.</span></span>

<span data-ttu-id="ce6a2-152">**Listagem 2-Models\Movie.vb**</span><span class="sxs-lookup"><span data-stu-id="ce6a2-152">**Listing 2 - Models\Movie.vb**</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample3.vb)]

<span data-ttu-id="ce6a2-153">Observe que a classe na Listagem 2 inclui o modificador *parcial* .</span><span class="sxs-lookup"><span data-stu-id="ce6a2-153">Notice that the class in Listing 2 includes the *partial* modifier.</span></span> <span data-ttu-id="ce6a2-154">Quaisquer métodos ou propriedades que você adicionar a essa classe se tornarão parte da classe de filme gerada pelo Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-154">Any methods or properties that you add to this class become part of the Movie class generated by the Entity Framework.</span></span>

## <a name="adding-onchanging-and-onchanged-partial-methods"></a><span data-ttu-id="ce6a2-155">Adicionando métodos parciais OnChange e onaltered</span><span class="sxs-lookup"><span data-stu-id="ce6a2-155">Adding OnChanging and OnChanged Partial Methods</span></span>

<span data-ttu-id="ce6a2-156">Quando o Entity Framework gera uma classe de entidade, o Entity Framework adiciona métodos parciais à classe automaticamente.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-156">When the Entity Framework generates an entity class, the Entity Framework adds partial methods to the class automatically.</span></span> <span data-ttu-id="ce6a2-157">O Entity Framework gera métodos parciais onalterion e OnChanged que correspondem a cada propriedade da classe.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-157">The Entity Framework generates OnChanging and OnChanged partial methods that correspond to each property of the class.</span></span>

<span data-ttu-id="ce6a2-158">No caso da classe Movie, o Entity Framework cria os seguintes métodos:</span><span class="sxs-lookup"><span data-stu-id="ce6a2-158">In the case of the Movie class, the Entity Framework creates the following methods:</span></span>

- <span data-ttu-id="ce6a2-159">OnIdChanging</span><span class="sxs-lookup"><span data-stu-id="ce6a2-159">OnIdChanging</span></span>
- <span data-ttu-id="ce6a2-160">OnIdChanged</span><span class="sxs-lookup"><span data-stu-id="ce6a2-160">OnIdChanged</span></span>
- <span data-ttu-id="ce6a2-161">OnTitleChanging</span><span class="sxs-lookup"><span data-stu-id="ce6a2-161">OnTitleChanging</span></span>
- <span data-ttu-id="ce6a2-162">Ontitlechanged</span><span class="sxs-lookup"><span data-stu-id="ce6a2-162">OnTitleChanged</span></span>
- <span data-ttu-id="ce6a2-163">OnDirectorChanging</span><span class="sxs-lookup"><span data-stu-id="ce6a2-163">OnDirectorChanging</span></span>
- <span data-ttu-id="ce6a2-164">Ondirectorchanged</span><span class="sxs-lookup"><span data-stu-id="ce6a2-164">OnDirectorChanged</span></span>
- <span data-ttu-id="ce6a2-165">OnDateReleasedChanging</span><span class="sxs-lookup"><span data-stu-id="ce6a2-165">OnDateReleasedChanging</span></span>
- <span data-ttu-id="ce6a2-166">OnDateReleasedChanged</span><span class="sxs-lookup"><span data-stu-id="ce6a2-166">OnDateReleasedChanged</span></span>

<span data-ttu-id="ce6a2-167">O método onmutate é chamado logo antes que a propriedade correspondente seja alterada.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-167">The OnChanging method is called right before the corresponding property is changed.</span></span> <span data-ttu-id="ce6a2-168">O método OnChanged é chamado logo depois que a propriedade é alterada.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-168">The OnChanged method is called right after the property is changed.</span></span>

<span data-ttu-id="ce6a2-169">Você pode aproveitar esses métodos parciais para adicionar lógica de validação à classe Movie.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-169">You can take advantage of these partial methods to add validation logic to the Movie class.</span></span> <span data-ttu-id="ce6a2-170">A classe de filme de atualização na Listagem 3 verifica se as propriedades Title e director recebem valores não vazios.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-170">The update Movie class in Listing 3 verifies that the Title and Director properties are assigned nonempty values.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="ce6a2-171">Um método parcial é um método definido em uma classe que você não precisa implementar.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-171">A partial method is a method defined in a class that you are not required to implement.</span></span> <span data-ttu-id="ce6a2-172">Se você não implementar um método parcial, o compilador removerá a assinatura do método e todas as chamadas para o método, de modo que não haja custos de tempo de execução associados ao método parcial.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-172">If you don't implement a partial method then the compiler removes the method signature and all calls to the method so there are no run-time costs associated with the partial method.</span></span> <span data-ttu-id="ce6a2-173">No editor de Visual Studio Code, você pode adicionar um método parcial digitando a palavra-chave *parcial* seguida por um espaço para exibir uma lista de parciais a serem implementados.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-173">In the Visual Studio Code Editor, you can add a partial method by typing the keyword *partial* followed by a space to view a list of partials to implement.</span></span>

<span data-ttu-id="ce6a2-174">**Listagem 3-Models\Movie.vb**</span><span class="sxs-lookup"><span data-stu-id="ce6a2-174">**Listing 3 - Models\Movie.vb**</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample4.vb)]

<span data-ttu-id="ce6a2-175">Por exemplo, se você tentar atribuir uma cadeia de caracteres vazia à propriedade Title, uma mensagem de erro será atribuída a um dicionário chamado \_erros.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-175">For example, if you attempt to assign an empty string to the Title property, then an error message is assigned to a Dictionary named \_errors.</span></span>

<span data-ttu-id="ce6a2-176">Neste ponto, nada realmente acontece quando você atribui uma cadeia de caracteres vazia à propriedade Title e um erro é adicionado ao campo Private \_Errors.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-176">At this point, nothing actually happens when you assign an empty string to the Title property and an error is added to the private \_errors field.</span></span> <span data-ttu-id="ce6a2-177">Precisamos implementar a interface IDataErrorInfo para expor esses erros de validação para a estrutura MVC do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-177">We need to implement the IDataErrorInfo interface to expose these validation errors to the ASP.NET MVC framework.</span></span>

## <a name="implementing-the-idataerrorinfo-interface"></a><span data-ttu-id="ce6a2-178">Implementando a interface IDataErrorInfo</span><span class="sxs-lookup"><span data-stu-id="ce6a2-178">Implementing the IDataErrorInfo Interface</span></span>

<span data-ttu-id="ce6a2-179">A interface IDataErrorInfo faz parte do .NET Framework desde a primeira versão.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-179">The IDataErrorInfo interface has been part of the .NET framework since the first version.</span></span> <span data-ttu-id="ce6a2-180">Essa interface é uma interface muito simples:</span><span class="sxs-lookup"><span data-stu-id="ce6a2-180">This interface is a very simple interface:</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample5.vb)]

<span data-ttu-id="ce6a2-181">Se uma classe implementar a interface IDataErrorInfo, a estrutura MVC do ASP.NET usará essa interface ao criar uma instância da classe.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-181">If a class implements the IDataErrorInfo interface, the ASP.NET MVC framework will use this interface when creating an instance of the class.</span></span> <span data-ttu-id="ce6a2-182">Por exemplo, a ação Create () do controlador Home aceita uma instância da classe Movie:</span><span class="sxs-lookup"><span data-stu-id="ce6a2-182">For example, the Home controller Create() action accepts an instance of the Movie class:</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample6.vb)]

<span data-ttu-id="ce6a2-183">A estrutura MVC do ASP.NET cria a instância do filme passada para a ação criar () usando um associador de modelo (o DefaultModelBinder).</span><span class="sxs-lookup"><span data-stu-id="ce6a2-183">The ASP.NET MVC framework creates the instance of the Movie passed to the Create() action by using a model binder (the DefaultModelBinder).</span></span> <span data-ttu-id="ce6a2-184">O associador de modelo é responsável por criar uma instância do objeto de filme ligando os campos de formulário HTML a uma instância do objeto de filme.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-184">The model binder is responsible for creating an instance of the Movie object by binding the HTML form fields to an instance of the Movie object.</span></span>

<span data-ttu-id="ce6a2-185">O DefaultModelBinder detecta se uma classe implementa ou não a interface IDataErrorInfo.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-185">The DefaultModelBinder detects whether or not a class implements the IDataErrorInfo interface.</span></span> <span data-ttu-id="ce6a2-186">Se uma classe implementar essa interface, o associador de modelo invocará o IDataErrorInfo. esse indexador para cada propriedade da classe.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-186">If a class implements this interface then the model binder invokes the IDataErrorInfo.this indexer for each property of the class.</span></span> <span data-ttu-id="ce6a2-187">Se o indexador retornar uma mensagem de erro, o associador de modelo adicionará essa mensagem de erro ao estado do modelo automaticamente.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-187">If the indexer returns an error message then the model binder adds this error message to model state automatically.</span></span>

<span data-ttu-id="ce6a2-188">O DefaultModelBinder também verifica a propriedade IDataErrorInfo. Error.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-188">The DefaultModelBinder also checks the IDataErrorInfo.Error property.</span></span> <span data-ttu-id="ce6a2-189">Esta propriedade destina-se a representar erros de validação específicos de não Propriedade associados à classe.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-189">This property is intended to represent non-property specific validation errors associated with the class.</span></span> <span data-ttu-id="ce6a2-190">Por exemplo, talvez você queira impor uma regra de validação que dependa dos valores de várias propriedades da classe Movie.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-190">For example, you might want to enforce a validation rule that depends on the values of multiple properties of the Movie class.</span></span> <span data-ttu-id="ce6a2-191">Nesse caso, você retornaria um erro de validação da Propriedade Error.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-191">In that case, you would return a validation error from the Error property.</span></span>

<span data-ttu-id="ce6a2-192">A classe de filme atualizada na Listagem 4 implementa a interface IDataErrorInfo.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-192">The updated Movie class in Listing 4 implements the IDataErrorInfo interface.</span></span>

<span data-ttu-id="ce6a2-193">**Listagem 4-Models\Movie.vb (implementa IDataErrorInfo)**</span><span class="sxs-lookup"><span data-stu-id="ce6a2-193">**Listing 4 - Models\Movie.vb (implements IDataErrorInfo)**</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample7.vb)]

<span data-ttu-id="ce6a2-194">Na Listagem 4, a propriedade indexador verifica a coleção de erros \_para ver se ela contém uma chave que corresponde ao nome da propriedade passado para o indexador.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-194">In Listing 4, the indexer property checks the \_errors collection to see if it contains a key that corresponds to the property name passed to the indexer.</span></span> <span data-ttu-id="ce6a2-195">Se não houver nenhum erro de validação associado à propriedade, uma cadeia de caracteres vazia será retornada.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-195">If there is no validation error associated with the property then an empty string is returned.</span></span>

<span data-ttu-id="ce6a2-196">Você não precisa modificar o controlador inicial de qualquer forma para usar a classe de filme modificada.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-196">You don't need to modify the Home controller in any way to use the modified Movie class.</span></span> <span data-ttu-id="ce6a2-197">A página exibida na Figura 3 ilustra o que acontece quando nenhum valor é inserido para os campos de formulário título ou diretor.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-197">The page displayed in Figure 3 illustrates what happens when no value is entered for the Title or Director form fields.</span></span>

<span data-ttu-id="ce6a2-198">[![criar métodos de ação automaticamente](validating-with-the-idataerrorinfo-interface-vb/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="ce6a2-198">[![Creating action methods automatically](validating-with-the-idataerrorinfo-interface-vb/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image5.png)</span></span>

<span data-ttu-id="ce6a2-199">**Figura 03**: um formulário com valores ausentes ([clique para exibir a imagem em tamanho normal](validating-with-the-idataerrorinfo-interface-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="ce6a2-199">**Figure 03**: A form with missing values ([Click to view full-size image](validating-with-the-idataerrorinfo-interface-vb/_static/image6.png))</span></span>

<span data-ttu-id="ce6a2-200">Observe que o valor de DateReleased é validado automaticamente.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-200">Notice that the DateReleased value is validated automatically.</span></span> <span data-ttu-id="ce6a2-201">Como a propriedade DateReleased não aceita valores nulos, o DefaultModelBinder gera um erro de validação para essa propriedade automaticamente quando não tem um valor.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-201">Because the DateReleased property does not accept NULL values, the DefaultModelBinder generates a validation error for this property automatically when it does not have a value.</span></span> <span data-ttu-id="ce6a2-202">Se você quiser modificar a mensagem de erro para a propriedade DateReleased, precisará criar um associador de modelo personalizado.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-202">If you want to modify the error message for the DateReleased property then you need to create a custom model binder.</span></span>

## <a name="summary"></a><span data-ttu-id="ce6a2-203">Resumo</span><span class="sxs-lookup"><span data-stu-id="ce6a2-203">Summary</span></span>

<span data-ttu-id="ce6a2-204">Neste tutorial, você aprendeu a usar a interface IDataErrorInfo para gerar mensagens de erro de validação.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-204">In this tutorial, you learned how to use the IDataErrorInfo interface to generate validation error messages.</span></span> <span data-ttu-id="ce6a2-205">Primeiro, criamos uma classe de filme parcial que estende a funcionalidade da classe de filme parcial gerada pelo Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-205">First, we created a partial Movie class that extends the functionality of the partial Movie class generated by the Entity Framework.</span></span> <span data-ttu-id="ce6a2-206">Em seguida, adicionamos lógica de validação aos métodos parciais OnTitleChanging () e OnDirectorChanging () da classe Movie.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-206">Next, we added validation logic to the Movie class OnTitleChanging() and OnDirectorChanging() partial methods.</span></span> <span data-ttu-id="ce6a2-207">Por fim, implementamos a interface IDataErrorInfo para expor essas mensagens de validação para a estrutura MVC do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ce6a2-207">Finally, we implemented the IDataErrorInfo interface in order to expose these validation messages to the ASP.NET MVC framework.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ce6a2-208">[Anterior](performing-simple-validation-vb.md)
> [Próximo](validating-with-a-service-layer-vb.md)</span><span class="sxs-lookup"><span data-stu-id="ce6a2-208">[Previous](performing-simple-validation-vb.md)
[Next](validating-with-a-service-layer-vb.md)</span></span>
