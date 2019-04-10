---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: Suporte a relações de entidade no OData v3 com a API Web 2 | Microsoft Docs
author: MikeWasson
description: 'A maioria dos conjuntos de dados definem relações entre entidades: Os clientes tiverem pedidos; os livros têm autores; os produtos têm fornecedores. Usar o OData, os clientes podem navegar sobre...'
ms.author: riande
ms.date: 02/26/2014
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: c78787aac83720eb9e8d6e9e0499f30a31951bc2
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59393854"
---
# <a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a><span data-ttu-id="ee90b-104">Suporte a relações de entidade no OData v3 com a API Web 2</span><span class="sxs-lookup"><span data-stu-id="ee90b-104">Supporting Entity Relations in OData v3 with Web API 2</span></span>

<span data-ttu-id="ee90b-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ee90b-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="ee90b-106">Baixe o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="ee90b-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="ee90b-107">A maioria dos conjuntos de dados definem relações entre entidades: Os clientes tiverem pedidos; os livros têm autores; os produtos têm fornecedores.</span><span class="sxs-lookup"><span data-stu-id="ee90b-107">Most data sets define relations between entities: Customers have orders; books have authors; products have suppliers.</span></span> <span data-ttu-id="ee90b-108">Usar o OData, os clientes podem navegar sobre relações de entidade.</span><span class="sxs-lookup"><span data-stu-id="ee90b-108">Using OData, clients can navigate over entity relations.</span></span> <span data-ttu-id="ee90b-109">Um produto, você pode encontrar o fornecedor.</span><span class="sxs-lookup"><span data-stu-id="ee90b-109">Given a product, you can find the supplier.</span></span> <span data-ttu-id="ee90b-110">Você também pode criar ou remover relações.</span><span class="sxs-lookup"><span data-stu-id="ee90b-110">You can also create or remove relationships.</span></span> <span data-ttu-id="ee90b-111">Por exemplo, você pode definir o fornecedor para um produto.</span><span class="sxs-lookup"><span data-stu-id="ee90b-111">For example, you can set the supplier for a product.</span></span>
> 
> <span data-ttu-id="ee90b-112">Este tutorial mostra como dar suporte a essas operações na API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ee90b-112">This tutorial shows how to support these operations in ASP.NET Web API.</span></span> <span data-ttu-id="ee90b-113">O tutorial se baseia no tutorial [criando um ponto de extremidade OData v3 com a API Web 2](creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="ee90b-113">The tutorial builds on the tutorial [Creating an OData v3 Endpoint with Web API 2](creating-an-odata-endpoint.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="ee90b-114">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="ee90b-114">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="ee90b-115">API Web 2</span><span class="sxs-lookup"><span data-stu-id="ee90b-115">Web API 2</span></span>
> - <span data-ttu-id="ee90b-116">OData versão 3</span><span class="sxs-lookup"><span data-stu-id="ee90b-116">OData Version 3</span></span>
> - <span data-ttu-id="ee90b-117">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="ee90b-117">Entity Framework 6</span></span>


## <a name="add-a-supplier-entity"></a><span data-ttu-id="ee90b-118">Adicionar uma entidade do fornecedor</span><span class="sxs-lookup"><span data-stu-id="ee90b-118">Add a Supplier Entity</span></span>

<span data-ttu-id="ee90b-119">Primeiro, precisamos adicionar um novo tipo de entidade para nosso feed OData.</span><span class="sxs-lookup"><span data-stu-id="ee90b-119">First we need to add a new entity type to our OData feed.</span></span> <span data-ttu-id="ee90b-120">Vamos adicionar um `Supplier` classe.</span><span class="sxs-lookup"><span data-stu-id="ee90b-120">We'll add a `Supplier` class.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

<span data-ttu-id="ee90b-121">Essa classe usa uma cadeia de caracteres para a chave de entidade.</span><span class="sxs-lookup"><span data-stu-id="ee90b-121">This class uses a string for the entity key.</span></span> <span data-ttu-id="ee90b-122">Na prática, que pode ser menos comum do que usando uma chave de inteiro.</span><span class="sxs-lookup"><span data-stu-id="ee90b-122">In practice, that might be less common than using an integer key.</span></span> <span data-ttu-id="ee90b-123">Mas vale a pena vendo como OData lida com outros tipos de chave além de números inteiros.</span><span class="sxs-lookup"><span data-stu-id="ee90b-123">But it's worth seeing how OData handles other key types besides integers.</span></span>

<span data-ttu-id="ee90b-124">Em seguida, vamos criar uma relação com a adição de um `Supplier` propriedade para o `Product` classe:</span><span class="sxs-lookup"><span data-stu-id="ee90b-124">Next, we'll create a relation by adding a `Supplier` property to the `Product` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

<span data-ttu-id="ee90b-125">Adicione um novo **DbSet** para o `ProductServiceContext` classe, para que o Entity Framework incluirá o `Supplier` tabela no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ee90b-125">Add a new **DbSet** to the `ProductServiceContext` class, so that Entity Framework will include the `Supplier` table in the database.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

<span data-ttu-id="ee90b-126">Em WebApiConfig.cs, adicione uma entidade "Fornecedores" para o modelo EDM:</span><span class="sxs-lookup"><span data-stu-id="ee90b-126">In WebApiConfig.cs, add a "Suppliers" entity to the EDM model:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a><span data-ttu-id="ee90b-127">Propriedades de navegação</span><span class="sxs-lookup"><span data-stu-id="ee90b-127">Navigation Properties</span></span>

<span data-ttu-id="ee90b-128">Para obter o fornecedor para um produto, o cliente envia uma solicitação GET:</span><span class="sxs-lookup"><span data-stu-id="ee90b-128">To get the supplier for a product, the client sends a GET request:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

<span data-ttu-id="ee90b-129">Aqui o "Fornecedor" é uma propriedade de navegação no `Product` tipo.</span><span class="sxs-lookup"><span data-stu-id="ee90b-129">Here "Supplier" is a navigation property on the `Product` type.</span></span> <span data-ttu-id="ee90b-130">Nesse caso, `Supplier` refere-se a um único item, mas uma navegação de propriedade também pode retornar uma coleção (relação um-para-muitos ou muitos-para-muitos).</span><span class="sxs-lookup"><span data-stu-id="ee90b-130">In this case, `Supplier` refers to a single item, but a navigation property can also return a collection (one-to-many or many-to-many relation).</span></span>

<span data-ttu-id="ee90b-131">Para dar suporte a essa solicitação, adicione o seguinte método para o `ProductsController` classe:</span><span class="sxs-lookup"><span data-stu-id="ee90b-131">To support this request, add the following method to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

<span data-ttu-id="ee90b-132">O *chave* parâmetro é a chave do produto.</span><span class="sxs-lookup"><span data-stu-id="ee90b-132">The *key* parameter is the key of the product.</span></span> <span data-ttu-id="ee90b-133">O método retorna a entidade relacionada&#8212;nesse caso, um `Supplier` instância.</span><span class="sxs-lookup"><span data-stu-id="ee90b-133">The method returns the related entity&#8212;in this case, a `Supplier` instance.</span></span> <span data-ttu-id="ee90b-134">O nome do método e o nome de parâmetro são importantes.</span><span class="sxs-lookup"><span data-stu-id="ee90b-134">The method name and parameter name are both important.</span></span> <span data-ttu-id="ee90b-135">Em geral, se a propriedade de navegação é chamada "X", você precisará adicionar um método chamado "GetX".</span><span class="sxs-lookup"><span data-stu-id="ee90b-135">In general, if the navigation property is named "X", you need to add a method named "GetX".</span></span> <span data-ttu-id="ee90b-136">O método deve aceitar um parâmetro denominado "*chave*" que corresponde ao tipo de dados da chave do pai.</span><span class="sxs-lookup"><span data-stu-id="ee90b-136">The method must take a parameter named "*key*" that matches the data type of the parent's key.</span></span>

<span data-ttu-id="ee90b-137">Também é importante incluir a **[FromOdataUri]** atributo na *chave* parâmetro.</span><span class="sxs-lookup"><span data-stu-id="ee90b-137">It is also important to include the **[FromOdataUri]** attribute in the *key* parameter.</span></span> <span data-ttu-id="ee90b-138">Esse atributo instrui a API da Web para usar regras de sintaxe do OData, quando ele analisa a chave do URI da solicitação.</span><span class="sxs-lookup"><span data-stu-id="ee90b-138">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

## <a name="creating-and-deleting-links"></a><span data-ttu-id="ee90b-139">Criação e exclusão de Links</span><span class="sxs-lookup"><span data-stu-id="ee90b-139">Creating and Deleting Links</span></span>

<span data-ttu-id="ee90b-140">OData dá suporte à criação ou remover relações entre duas entidades.</span><span class="sxs-lookup"><span data-stu-id="ee90b-140">OData supports creating or removing relationships between two entities.</span></span> <span data-ttu-id="ee90b-141">Na terminologia de OData, a relação é um "link".</span><span class="sxs-lookup"><span data-stu-id="ee90b-141">In OData terminology, the relationship is a "link."</span></span> <span data-ttu-id="ee90b-142">Cada link tem um URI com o formulário *entity*/$links /*entidade*.</span><span class="sxs-lookup"><span data-stu-id="ee90b-142">Each link has a URI with the form *entity*/$links/*entity*.</span></span> <span data-ttu-id="ee90b-143">Por exemplo, o link do produto para fornecedor esta aparência:</span><span class="sxs-lookup"><span data-stu-id="ee90b-143">For example, the link from product to supplier looks like this:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

<span data-ttu-id="ee90b-144">Para criar um novo link, o cliente envia uma solicitação POST para o URI de link.</span><span class="sxs-lookup"><span data-stu-id="ee90b-144">To create a new link, the client sends a POST request to the link URI.</span></span> <span data-ttu-id="ee90b-145">O corpo da solicitação é o URI da entidade de destino.</span><span class="sxs-lookup"><span data-stu-id="ee90b-145">The body of the request is the URI of the target entity.</span></span> <span data-ttu-id="ee90b-146">Por exemplo, suponha que há um fornecedor com a chave "CTSO".</span><span class="sxs-lookup"><span data-stu-id="ee90b-146">For example, suppose there is a supplier with the key "CTSO".</span></span> <span data-ttu-id="ee90b-147">Para criar um link de "Product(1)" para "Supplier('CTSO')", o cliente envia uma solicitação semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="ee90b-147">To create a link from "Product(1)" to "Supplier('CTSO')", the client sends a request like the following:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

<span data-ttu-id="ee90b-148">Para excluir um link, o cliente envia uma solicitação de exclusão para o URI de link.</span><span class="sxs-lookup"><span data-stu-id="ee90b-148">To delete a link, the client sends a DELETE request to the link URI.</span></span>

**<span data-ttu-id="ee90b-149">Criação de Links</span><span class="sxs-lookup"><span data-stu-id="ee90b-149">Creating Links</span></span>**

<span data-ttu-id="ee90b-150">Para habilitar um cliente criar links de fornecedor do produto, adicione o seguinte código para o `ProductsController` classe:</span><span class="sxs-lookup"><span data-stu-id="ee90b-150">To enable a client to create product-supplier links, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

<span data-ttu-id="ee90b-151">Esse método usa três parâmetros:</span><span class="sxs-lookup"><span data-stu-id="ee90b-151">This method takes three parameters:</span></span>

- <span data-ttu-id="ee90b-152">*key*: A chave para a entidade pai (produto)</span><span class="sxs-lookup"><span data-stu-id="ee90b-152">*key*: The key to the parent entity (the product)</span></span>
- <span data-ttu-id="ee90b-153">*navigationProperty*: O nome da propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="ee90b-153">*navigationProperty*: The name of the navigation property.</span></span> <span data-ttu-id="ee90b-154">Neste exemplo, a propriedade de navegação só é válida é "Fornecedor".</span><span class="sxs-lookup"><span data-stu-id="ee90b-154">In this example, the only valid navigation property is "Supplier".</span></span>
- <span data-ttu-id="ee90b-155">*link*: O URI do OData da entidade relacionada.</span><span class="sxs-lookup"><span data-stu-id="ee90b-155">*link*: The OData URI of the related entity.</span></span> <span data-ttu-id="ee90b-156">Esse valor é obtido do corpo da solicitação.</span><span class="sxs-lookup"><span data-stu-id="ee90b-156">This value is taken from the request body.</span></span> <span data-ttu-id="ee90b-157">Por exemplo, o link do URI pode ser "`http://localhost/odata/Suppliers('CTSO')`, que significa que o fornecedor com ID = 'CTSO'.</span><span class="sxs-lookup"><span data-stu-id="ee90b-157">For example, the link URI might be "`http://localhost/odata/Suppliers('CTSO')`, meaning the supplier with ID = ‘CTSO'.</span></span>

<span data-ttu-id="ee90b-158">O método usa o link para pesquisar o fornecedor.</span><span class="sxs-lookup"><span data-stu-id="ee90b-158">The method uses the link to look up the supplier.</span></span> <span data-ttu-id="ee90b-159">Se o fornecedor correspondente for encontrado, o método define o `Product.Supplier` propriedade e salva o resultado para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ee90b-159">If the matching supplier is found, the method sets the `Product.Supplier` property and saves the result to the database.</span></span>

<span data-ttu-id="ee90b-160">A parte mais difícil é analisar o URI de link.</span><span class="sxs-lookup"><span data-stu-id="ee90b-160">The hardest part is parsing the link URI.</span></span> <span data-ttu-id="ee90b-161">Basicamente, você precisa simular o resultado de enviar uma solicitação GET para esse URI.</span><span class="sxs-lookup"><span data-stu-id="ee90b-161">Basically, you need to simulate the result of sending a GET request to that URI.</span></span> <span data-ttu-id="ee90b-162">O método auxiliar a seguir mostra como fazer isso.</span><span class="sxs-lookup"><span data-stu-id="ee90b-162">The following helper method shows how to do this.</span></span> <span data-ttu-id="ee90b-163">O método invoca o processo de roteamento de API da Web e recebe de volta uma **ODataPath** instância que representa o caminho de OData analisado.</span><span class="sxs-lookup"><span data-stu-id="ee90b-163">The method invokes the Web API routing process and gets back an **ODataPath** instance that represents the parsed OData path.</span></span> <span data-ttu-id="ee90b-164">Para um URI de link, um dos segmentos deve ser a chave de entidade.</span><span class="sxs-lookup"><span data-stu-id="ee90b-164">For a link URI, one of the segments should be the entity key.</span></span> <span data-ttu-id="ee90b-165">(Caso contrário, o cliente enviou um URI inválido.)</span><span class="sxs-lookup"><span data-stu-id="ee90b-165">(If not, the client sent a bad URI.)</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

**<span data-ttu-id="ee90b-166">Exclusão de Links</span><span class="sxs-lookup"><span data-stu-id="ee90b-166">Deleting Links</span></span>**

<span data-ttu-id="ee90b-167">Para excluir um link, adicione o seguinte código para o `ProductsController` classe:</span><span class="sxs-lookup"><span data-stu-id="ee90b-167">To delete a link, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

<span data-ttu-id="ee90b-168">Neste exemplo, a propriedade de navegação é um único `Supplier` entidade.</span><span class="sxs-lookup"><span data-stu-id="ee90b-168">In this example, the navigation property is a single `Supplier` entity.</span></span> <span data-ttu-id="ee90b-169">Se a propriedade de navegação é uma coleção, o URI para excluir um link deve incluir uma chave para a entidade relacionada.</span><span class="sxs-lookup"><span data-stu-id="ee90b-169">If the navigation property is a collection, the URI to delete a link must include a key for the related entity.</span></span> <span data-ttu-id="ee90b-170">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ee90b-170">For example:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

<span data-ttu-id="ee90b-171">Essa solicitação remove ordem 1 do cliente 1.</span><span class="sxs-lookup"><span data-stu-id="ee90b-171">This request removes order 1 from customer 1.</span></span> <span data-ttu-id="ee90b-172">Nesse caso, o método DeleteLink terá a seguinte assinatura:</span><span class="sxs-lookup"><span data-stu-id="ee90b-172">In this case, the DeleteLink method will have the following signature:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

<span data-ttu-id="ee90b-173">O *relatedKey* parâmetro fornece a chave para a entidade relacionada.</span><span class="sxs-lookup"><span data-stu-id="ee90b-173">The *relatedKey* parameter gives the key for the related entity.</span></span> <span data-ttu-id="ee90b-174">Portanto, no seu `DeleteLink` método, pesquisar a entidade principal pelo *chave* parâmetro, encontrar a entidade relacionada pelo *relatedKey* parâmetro e, em seguida, remova a associação.</span><span class="sxs-lookup"><span data-stu-id="ee90b-174">So in your `DeleteLink` method, look up the primary entity by the *key* parameter, find the related entity by the *relatedKey* parameter, and then remove the association.</span></span> <span data-ttu-id="ee90b-175">Dependendo de seu modelo de dados, talvez você precise implementar ambas as versões do `DeleteLink`.</span><span class="sxs-lookup"><span data-stu-id="ee90b-175">Depending on your data model, you might need to implement both versions of `DeleteLink`.</span></span> <span data-ttu-id="ee90b-176">API da Web chamará a versão correta com base no URI da solicitação.</span><span class="sxs-lookup"><span data-stu-id="ee90b-176">Web API will call the correct version based on the request URI.</span></span>
