---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: Tipos abertos no OData V4 com ASP.NET Web API | Microsoft Docs
author: microsoft
description: No OData v4, um tipo aberto é um tipo estruturado que contém propriedades dinâmicas, além de quaisquer propriedades declaradas na definição de tipo. Abrir...
ms.author: riande
ms.date: 09/15/2014
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 950442c071bf50d2c8c1588971f13f85c4891436
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622174"
---
# <a name="open-types-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="a4277-104">Tipos abertos no OData V4 com ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="a4277-104">Open Types in OData v4 with ASP.NET Web API</span></span>

<span data-ttu-id="a4277-105">pela [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="a4277-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="a4277-106">No OData v4, um *tipo aberto* é um tipo estruturado que contém propriedades dinâmicas, além de quaisquer propriedades declaradas na definição de tipo.</span><span class="sxs-lookup"><span data-stu-id="a4277-106">In OData v4, an *open type* is a structured type that contains dynamic properties, in addition to any properties that are declared in the type definition.</span></span> <span data-ttu-id="a4277-107">Os tipos abertos permitem que você adicione flexibilidade aos seus modelos de dados.</span><span class="sxs-lookup"><span data-stu-id="a4277-107">Open types let you add flexibility to your data models.</span></span> <span data-ttu-id="a4277-108">Este tutorial mostra como usar tipos abertos no ASP.NET Web API OData.</span><span class="sxs-lookup"><span data-stu-id="a4277-108">This tutorial shows how to use open types in ASP.NET Web API OData.</span></span>
> 
> <span data-ttu-id="a4277-109">Este tutorial pressupõe que você já sabe como criar um ponto de extremidade OData no ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="a4277-109">This tutorial assumes that you already know how to create an OData endpoint in ASP.NET Web API.</span></span> <span data-ttu-id="a4277-110">Caso contrário, comece lendo [criar um ponto de extremidade do OData v4](create-an-odata-v4-endpoint.md) primeiro.</span><span class="sxs-lookup"><span data-stu-id="a4277-110">If not, start by reading [Create an OData v4 Endpoint](create-an-odata-v4-endpoint.md) first.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="a4277-111">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="a4277-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="a4277-112">API Web OData 5,3</span><span class="sxs-lookup"><span data-stu-id="a4277-112">Web API OData 5.3</span></span>
> - <span data-ttu-id="a4277-113">OData v4</span><span class="sxs-lookup"><span data-stu-id="a4277-113">OData v4</span></span>

<span data-ttu-id="a4277-114">Primeiro, algumas terminologias do OData:</span><span class="sxs-lookup"><span data-stu-id="a4277-114">First, some OData terminology:</span></span>

- <span data-ttu-id="a4277-115">Tipo de entidade: um tipo estruturado com uma chave.</span><span class="sxs-lookup"><span data-stu-id="a4277-115">Entity type: A structured type with a key.</span></span>
- <span data-ttu-id="a4277-116">Tipo complexo: um tipo estruturado sem uma chave.</span><span class="sxs-lookup"><span data-stu-id="a4277-116">Complex type: A structured type without a key.</span></span>
- <span data-ttu-id="a4277-117">Open Type: um tipo com propriedades dinâmicas.</span><span class="sxs-lookup"><span data-stu-id="a4277-117">Open type: A type with dynamic properties.</span></span> <span data-ttu-id="a4277-118">Os tipos de entidade e os tipos complexos podem ser abertos.</span><span class="sxs-lookup"><span data-stu-id="a4277-118">Both entity types and complex types can be open.</span></span>

<span data-ttu-id="a4277-119">O valor de uma propriedade dinâmica pode ser um tipo primitivo, tipo complexo ou tipo de enumeração; ou uma coleção de qualquer um desses tipos.</span><span class="sxs-lookup"><span data-stu-id="a4277-119">The value of a dynamic property can be a primitive type, complex type, or enumeration type; or a collection of any of those types.</span></span> <span data-ttu-id="a4277-120">Para obter mais informações sobre tipos abertos, consulte a [especificação do OData v4](http://www.odata.org/documentation/odata-version-4-0/).</span><span class="sxs-lookup"><span data-stu-id="a4277-120">For more information about open types, see the [OData v4 specification](http://www.odata.org/documentation/odata-version-4-0/).</span></span>

## <a name="install-the-web-odata-libraries"></a><span data-ttu-id="a4277-121">Instalar as bibliotecas do Web OData</span><span class="sxs-lookup"><span data-stu-id="a4277-121">Install the Web OData Libraries</span></span>

<span data-ttu-id="a4277-122">Use o Gerenciador de pacotes NuGet para instalar as bibliotecas OData da API Web mais recentes.</span><span class="sxs-lookup"><span data-stu-id="a4277-122">Use NuGet Package Manager to install the latest Web API OData libraries.</span></span> <span data-ttu-id="a4277-123">Na janela do console do Gerenciador de pacotes:</span><span class="sxs-lookup"><span data-stu-id="a4277-123">From the Package Manager Console window:</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a><span data-ttu-id="a4277-124">Definir os tipos CLR</span><span class="sxs-lookup"><span data-stu-id="a4277-124">Define the CLR Types</span></span>

<span data-ttu-id="a4277-125">Comece definindo os modelos do EDM como tipos CLR.</span><span class="sxs-lookup"><span data-stu-id="a4277-125">Start by defining the EDM models as CLR types.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="a4277-126">Quando o Modelo de Dados de Entidade (EDM) é criado,</span><span class="sxs-lookup"><span data-stu-id="a4277-126">When the Entity Data Model (EDM) is created,</span></span>

- <span data-ttu-id="a4277-127">`Category` é um tipo de enumeração.</span><span class="sxs-lookup"><span data-stu-id="a4277-127">`Category` is an enumeration type.</span></span>
- <span data-ttu-id="a4277-128">`Address` é um tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="a4277-128">`Address` is a complex type.</span></span> <span data-ttu-id="a4277-129">(Ele não tem uma chave, portanto, não é um tipo de entidade.)</span><span class="sxs-lookup"><span data-stu-id="a4277-129">(It does not have a key, so it is not an entity type.)</span></span>
- <span data-ttu-id="a4277-130">`Customer` é um tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="a4277-130">`Customer` is an entity type.</span></span> <span data-ttu-id="a4277-131">(Tem uma chave.)</span><span class="sxs-lookup"><span data-stu-id="a4277-131">(It has a key.)</span></span>
- <span data-ttu-id="a4277-132">`Press` é um tipo complexo aberto.</span><span class="sxs-lookup"><span data-stu-id="a4277-132">`Press` is an open complex type.</span></span>
- <span data-ttu-id="a4277-133">`Book` é um tipo de entidade aberta.</span><span class="sxs-lookup"><span data-stu-id="a4277-133">`Book` is an open entity type.</span></span>

<span data-ttu-id="a4277-134">Para criar um tipo aberto, o tipo CLR deve ter uma propriedade do tipo `IDictionary<string, object>`, que contém as propriedades dinâmicas.</span><span class="sxs-lookup"><span data-stu-id="a4277-134">To create an open type, the CLR type must have a property of type `IDictionary<string, object>`, which holds the dynamic properties.</span></span>

## <a name="build-the-edm-model"></a><span data-ttu-id="a4277-135">Compilar o modelo EDM</span><span class="sxs-lookup"><span data-stu-id="a4277-135">Build the EDM Model</span></span>

<span data-ttu-id="a4277-136">Se você usar o **ODataConventionModelBuilder** para criar o EDM, `Press` e `Book` serão adicionados automaticamente como tipos abertos, com base na presença de uma propriedade `IDictionary<string, object>`.</span><span class="sxs-lookup"><span data-stu-id="a4277-136">If you use **ODataConventionModelBuilder** to create the EDM, `Press` and `Book` are automatically added as open types, based on the presence of a `IDictionary<string, object>` property.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="a4277-137">Você também pode criar o EDM explicitamente, usando o **ODataModelBuilder**.</span><span class="sxs-lookup"><span data-stu-id="a4277-137">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a><span data-ttu-id="a4277-138">Adicionar um controlador OData</span><span class="sxs-lookup"><span data-stu-id="a4277-138">Add an OData Controller</span></span>

<span data-ttu-id="a4277-139">Em seguida, adicione um controlador OData.</span><span class="sxs-lookup"><span data-stu-id="a4277-139">Next, add an OData controller.</span></span> <span data-ttu-id="a4277-140">Para este tutorial, usaremos um controlador simplificado que dá suporte apenas a solicitações GET e POST e usa uma lista na memória para armazenar entidades.</span><span class="sxs-lookup"><span data-stu-id="a4277-140">For this tutorial, we'll use a simplified controller that just supports GET and POST requests, and uses an in-memory list to store entities.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

<span data-ttu-id="a4277-141">Observe que a primeira instância de `Book` não tem propriedades dinâmicas.</span><span class="sxs-lookup"><span data-stu-id="a4277-141">Notice that the first `Book` instance has no dynamic properties.</span></span> <span data-ttu-id="a4277-142">A segunda instância de `Book` tem as seguintes propriedades dinâmicas:</span><span class="sxs-lookup"><span data-stu-id="a4277-142">The second `Book` instance has the following dynamic properties:</span></span>

- <span data-ttu-id="a4277-143">"Published": tipo primitivo</span><span class="sxs-lookup"><span data-stu-id="a4277-143">"Published": Primitive type</span></span>
- <span data-ttu-id="a4277-144">"Authors": coleção de tipos primitivos</span><span class="sxs-lookup"><span data-stu-id="a4277-144">"Authors": Collection of primitive types</span></span>
- <span data-ttu-id="a4277-145">"OtherCategories": coleção de tipos de enumeração.</span><span class="sxs-lookup"><span data-stu-id="a4277-145">"OtherCategories": Collection of enumeration types.</span></span>

<span data-ttu-id="a4277-146">Além disso, a propriedade `Press` dessa instância de `Book` tem as seguintes propriedades dinâmicas:</span><span class="sxs-lookup"><span data-stu-id="a4277-146">Also, the `Press` property of that `Book` instance has the following dynamic properties:</span></span>

- <span data-ttu-id="a4277-147">"Blog": tipo primitivo</span><span class="sxs-lookup"><span data-stu-id="a4277-147">"Blog": Primitive type</span></span>
- <span data-ttu-id="a4277-148">"Address": tipo complexo</span><span class="sxs-lookup"><span data-stu-id="a4277-148">"Address": Complex type</span></span>

## <a name="query-the-metadata"></a><span data-ttu-id="a4277-149">Consultar os metadados</span><span class="sxs-lookup"><span data-stu-id="a4277-149">Query the Metadata</span></span>

<span data-ttu-id="a4277-150">Para obter o documento de metadados OData, envie uma solicitação GET para `~/$metadata`.</span><span class="sxs-lookup"><span data-stu-id="a4277-150">To get the OData metadata document, send a GET request to `~/$metadata`.</span></span> <span data-ttu-id="a4277-151">O corpo da resposta deve ser semelhante a este:</span><span class="sxs-lookup"><span data-stu-id="a4277-151">The response body should look similar to this:</span></span>

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

<span data-ttu-id="a4277-152">No documento de metadados, você pode ver que:</span><span class="sxs-lookup"><span data-stu-id="a4277-152">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="a4277-153">Para os tipos `Book` e `Press`, o valor do atributo `OpenType` é true.</span><span class="sxs-lookup"><span data-stu-id="a4277-153">For the `Book` and `Press` types, the value of the `OpenType` attribute is true.</span></span> <span data-ttu-id="a4277-154">Os tipos `Customer` e `Address` não têm esse atributo.</span><span class="sxs-lookup"><span data-stu-id="a4277-154">The `Customer` and `Address` types don't have this attribute.</span></span>
- <span data-ttu-id="a4277-155">O tipo de entidade `Book` tem três propriedades declaradas: ISBN, title e pressione.</span><span class="sxs-lookup"><span data-stu-id="a4277-155">The `Book` entity type has three declared properties: ISBN, Title, and Press.</span></span> <span data-ttu-id="a4277-156">Os metadados OData não incluem a propriedade `Book.Properties` da classe CLR.</span><span class="sxs-lookup"><span data-stu-id="a4277-156">The OData metadata does not include the `Book.Properties` property from the CLR class.</span></span>
- <span data-ttu-id="a4277-157">Da mesma forma, o tipo complexo `Press` tem apenas duas propriedades declaradas: nome e categoria.</span><span class="sxs-lookup"><span data-stu-id="a4277-157">Similarly, the `Press` complex type has only two declared properties: Name and Category.</span></span> <span data-ttu-id="a4277-158">Os metadados não incluem a propriedade `Press.DynamicProperties` da classe CLR.</span><span class="sxs-lookup"><span data-stu-id="a4277-158">The metadata does not include the `Press.DynamicProperties` property from the CLR class.</span></span>

## <a name="query-an-entity"></a><span data-ttu-id="a4277-159">Consultar uma entidade</span><span class="sxs-lookup"><span data-stu-id="a4277-159">Query an Entity</span></span>

<span data-ttu-id="a4277-160">Para obter o livro com ISBN igual a "978-0-7356-7942-9", envie uma solicitação GET para `~/Books('978-0-7356-7942-9')`.</span><span class="sxs-lookup"><span data-stu-id="a4277-160">To get the book with ISBN equal to "978-0-7356-7942-9", send a GET request to `~/Books('978-0-7356-7942-9')`.</span></span> <span data-ttu-id="a4277-161">O corpo da resposta deve ser semelhante ao seguinte.</span><span class="sxs-lookup"><span data-stu-id="a4277-161">The response body should look similar to the following.</span></span> <span data-ttu-id="a4277-162">(Recuado para torná-lo mais legível.)</span><span class="sxs-lookup"><span data-stu-id="a4277-162">(Indented to make it more readable.)</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

<span data-ttu-id="a4277-163">Observe que as propriedades dinâmicas são incluídas embutidas com as propriedades declaradas.</span><span class="sxs-lookup"><span data-stu-id="a4277-163">Notice that the dynamic properties are included inline with the declared properties.</span></span>

## <a name="post-an-entity"></a><span data-ttu-id="a4277-164">POSTAR uma entidade</span><span class="sxs-lookup"><span data-stu-id="a4277-164">POST an Entity</span></span>

<span data-ttu-id="a4277-165">Para adicionar uma entidade de livro, envie uma solicitação POST para `~/Books`.</span><span class="sxs-lookup"><span data-stu-id="a4277-165">To add a Book entity, send a POST request to `~/Books`.</span></span> <span data-ttu-id="a4277-166">O cliente pode definir propriedades dinâmicas na carga de solicitação.</span><span class="sxs-lookup"><span data-stu-id="a4277-166">The client can set dynamic properties in the request payload.</span></span>

<span data-ttu-id="a4277-167">Aqui está uma solicitação de exemplo.</span><span class="sxs-lookup"><span data-stu-id="a4277-167">Here is an example request.</span></span> <span data-ttu-id="a4277-168">Observe as propriedades "Price" e "published".</span><span class="sxs-lookup"><span data-stu-id="a4277-168">Note the "Price" and "Published" properties.</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

<span data-ttu-id="a4277-169">Se você definir um ponto de interrupção no método do controlador, poderá ver que a API Web adicionou essas propriedades ao dicionário de `Properties`.</span><span class="sxs-lookup"><span data-stu-id="a4277-169">If you set a breakpoint in the controller method, you can see that Web API added these properties to the `Properties` dictionary.</span></span>

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a><span data-ttu-id="a4277-170">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="a4277-170">Additional Resources</span></span>

[<span data-ttu-id="a4277-171">Exemplo de tipo Open do OData</span><span class="sxs-lookup"><span data-stu-id="a4277-171">OData Open Type Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
