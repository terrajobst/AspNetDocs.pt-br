---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: Suporte a BSON na API Web ASP.NET 2.1 - ASP.NET 4.x
author: MikeWasson
description: mostra como usar o BSON em um controlador de API da Web (lado do servidor) em um aplicativo cliente .NET para o ASP.NET 4. x.
ms.author: riande
ms.date: 01/20/2014
ms.custom: seoapril2019
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 911e2abcfd277075b3cba71e624ec6390b99a15e
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59382219"
---
# <a name="bson-support-in-aspnet-web-api-21"></a><span data-ttu-id="56462-103">Suporte a BSON na API Web ASP.NET 2.1</span><span class="sxs-lookup"><span data-stu-id="56462-103">BSON Support in ASP.NET Web API 2.1</span></span>

<span data-ttu-id="56462-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="56462-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="56462-105">Este tópico mostra como usar o BSON em seu controlador de API da Web (lado do servidor) em um aplicativo cliente .NET.</span><span class="sxs-lookup"><span data-stu-id="56462-105">This topic shows how to use BSON in your Web API controller (server side) and in a .NET client app.</span></span> <span data-ttu-id="56462-106">API Web 2.1 apresenta suporte para BSON.</span><span class="sxs-lookup"><span data-stu-id="56462-106">Web API 2.1 introduces support for BSON.</span></span> 

## <a name="what-is-bson"></a><span data-ttu-id="56462-107">O que é o BSON?</span><span class="sxs-lookup"><span data-stu-id="56462-107">What is BSON?</span></span>

<span data-ttu-id="56462-108">[BSON](http://bsonspec.org/) é um formato de serialização binária.</span><span class="sxs-lookup"><span data-stu-id="56462-108">[BSON](http://bsonspec.org/) is a binary serialization format.</span></span> <span data-ttu-id="56462-109">"BSON" significa "Binary JSON", mas BSON e JSON são serializados de forma muito diferente.</span><span class="sxs-lookup"><span data-stu-id="56462-109">"BSON" stands for "Binary JSON", but BSON and JSON are serialized very differently.</span></span> <span data-ttu-id="56462-110">BSON é "Como JSON", porque os objetos são representados como pares nome-valor, semelhantes ao JSON.</span><span class="sxs-lookup"><span data-stu-id="56462-110">BSON is "JSON-like", because objects are represented as name-value pairs, similar to JSON.</span></span> <span data-ttu-id="56462-111">Ao contrário de JSON, os tipos de dados numéricos são armazenados como bytes, não cadeias de caracteres</span><span class="sxs-lookup"><span data-stu-id="56462-111">Unlike JSON, numeric data types are stored as bytes, not strings</span></span>

<span data-ttu-id="56462-112">BSON foi projetado para ser leve, fácil de examinar e rápido para codificação/decodificação.</span><span class="sxs-lookup"><span data-stu-id="56462-112">BSON was designed to be lightweight, easy to scan, and fast to encode/decode.</span></span>

- <span data-ttu-id="56462-113">BSON é comparável em tamanho ao JSON.</span><span class="sxs-lookup"><span data-stu-id="56462-113">BSON is comparable in size to JSON.</span></span> <span data-ttu-id="56462-114">Dependendo dos dados, uma carga BSON pode ser menor ou maior que uma carga JSON.</span><span class="sxs-lookup"><span data-stu-id="56462-114">Depending on the data, a BSON payload may be smaller or larger than a JSON payload.</span></span> <span data-ttu-id="56462-115">Para serializar os dados binários, como um arquivo de imagem BSON é menor do que o JSON, porque os dados binários não são codificada em base64.</span><span class="sxs-lookup"><span data-stu-id="56462-115">For serializing binary data, such as an image file, BSON is smaller than JSON, because the binary data is not base64-encoded.</span></span>
- <span data-ttu-id="56462-116">Documentos BSON são fáceis de examinar como os elementos são prefixados com um campo de comprimento, portanto, um analisador pode ignorar elementos sem decodificação-los.</span><span class="sxs-lookup"><span data-stu-id="56462-116">BSON documents are easy to scan because elements are prefixed with a length field, so a parser can skip elements without decoding them.</span></span>
- <span data-ttu-id="56462-117">Codificação e decodificação são eficientes, como tipos de dados numéricos são armazenados como números, não cadeias de caracteres.</span><span class="sxs-lookup"><span data-stu-id="56462-117">Encoding and decoding are efficient, because numeric data types are stored as numbers, not strings.</span></span>

<span data-ttu-id="56462-118">Clientes nativos, como aplicativos de cliente .NET, podem se beneficiar do uso BSON no lugar de formatos baseados em texto, como JSON ou XML.</span><span class="sxs-lookup"><span data-stu-id="56462-118">Native clients, such as .NET client apps, can benefit from using BSON in place of text-based formats such as JSON or XML.</span></span> <span data-ttu-id="56462-119">Para clientes de navegador, você provavelmente desejará fique com JSON, porque o JavaScript pode converter diretamente o conteúdo JSON.</span><span class="sxs-lookup"><span data-stu-id="56462-119">For browser clients, you will probably want to stick with JSON, because JavaScript can directly convert the JSON payload.</span></span>

<span data-ttu-id="56462-120">Felizmente, a API Web usa [negociação de conteúdo](content-negotiation.md), portanto, sua API pode oferecer suporte a ambos os formatos e permitir que o cliente escolher.</span><span class="sxs-lookup"><span data-stu-id="56462-120">Fortunately, Web API uses [content negotiation](content-negotiation.md), so your API can support both formats and let the client choose.</span></span>

## <a name="enabling-bson-on-the-server"></a><span data-ttu-id="56462-121">Habilitando o BSON no servidor</span><span class="sxs-lookup"><span data-stu-id="56462-121">Enabling BSON on the Server</span></span>

<span data-ttu-id="56462-122">Em sua configuração de API da Web, adicione a **BsonMediaTypeFormatter** à coleção de formatadores.</span><span class="sxs-lookup"><span data-stu-id="56462-122">In your Web API configuration, add the **BsonMediaTypeFormatter** to the formatters collection.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

<span data-ttu-id="56462-123">Agora, se o cliente solicita "application/bson", API da Web usará o formatador BSON.</span><span class="sxs-lookup"><span data-stu-id="56462-123">Now if the client requests "application/bson", Web API will use the BSON formatter.</span></span>

<span data-ttu-id="56462-124">Para associar o BSON com outros tipos de mídia, adicioná-los à coleção SupportedMediaTypes.</span><span class="sxs-lookup"><span data-stu-id="56462-124">To associate BSON with other media types, add them to the SupportedMediaTypes collection.</span></span> <span data-ttu-id="56462-125">O código a seguir adiciona "application/vnd.contoso" para os tipos de mídia com suporte:</span><span class="sxs-lookup"><span data-stu-id="56462-125">The following code adds "application/vnd.contoso" to the supported media types:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a><span data-ttu-id="56462-126">Sessão HTTP de exemplo</span><span class="sxs-lookup"><span data-stu-id="56462-126">Example HTTP Session</span></span>

<span data-ttu-id="56462-127">Neste exemplo, vamos usar a seguinte classe de modelo mais de um controlador de API Web simples:</span><span class="sxs-lookup"><span data-stu-id="56462-127">For this example, we'll use the following model class plus a simple Web API controller:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

<span data-ttu-id="56462-128">Um cliente pode enviar a solicitação HTTP a seguir:</span><span class="sxs-lookup"><span data-stu-id="56462-128">A client might send the following HTTP request:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

<span data-ttu-id="56462-129">Aqui está a resposta:</span><span class="sxs-lookup"><span data-stu-id="56462-129">Here is the response:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

<span data-ttu-id="56462-130">Aqui posso substituir os dados binários com &quot;.&quot; caracteres.</span><span class="sxs-lookup"><span data-stu-id="56462-130">Here I've replaced the binary data with &quot;.&quot; characters.</span></span> <span data-ttu-id="56462-131">A seguinte captura de tela do mostra Fiddler os valores de hexadecimais brutos.</span><span class="sxs-lookup"><span data-stu-id="56462-131">The following screen shot from Fiddler shows the raw hex values.</span></span>

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a><span data-ttu-id="56462-132">Usando o BSON com HttpClient</span><span class="sxs-lookup"><span data-stu-id="56462-132">Using BSON with HttpClient</span></span>

<span data-ttu-id="56462-133">Aplicativos de clientes .NET podem usar com o formatador de BSON **HttpClient**.</span><span class="sxs-lookup"><span data-stu-id="56462-133">.NET clients apps can use the BSON formatter with **HttpClient**.</span></span> <span data-ttu-id="56462-134">Para obter mais informações sobre **HttpClient**, consulte [chamar um Web API de um cliente .NET](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="56462-134">For more information about **HttpClient**, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="56462-135">O código a seguir envia uma solicitação GET que aceita BSON e, em seguida, desserializa o conteúdo BSON na resposta.</span><span class="sxs-lookup"><span data-stu-id="56462-135">The following code sends a GET request that accepts BSON, and then deserializes the BSON payload in the response.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

<span data-ttu-id="56462-136">Para solicitar o BSON do servidor, defina o cabeçalho Accept para "application/bson":</span><span class="sxs-lookup"><span data-stu-id="56462-136">To request BSON from the server, set the Accept header to "application/bson":</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

<span data-ttu-id="56462-137">Para desserializar o corpo da resposta, use o **BsonMediaTypeFormatter**.</span><span class="sxs-lookup"><span data-stu-id="56462-137">To deserialize the response body, use the **BsonMediaTypeFormatter**.</span></span> <span data-ttu-id="56462-138">Esse formatador não está na coleção de formatadores do padrão, portanto, você precisa especificá-lo quando você ler o corpo da resposta:</span><span class="sxs-lookup"><span data-stu-id="56462-138">This formatter is not in the default formatters collection, so you have to specify it when you read the response body:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

<span data-ttu-id="56462-139">O exemplo a seguir mostra como enviar uma solicitação POST que contém o BSON.</span><span class="sxs-lookup"><span data-stu-id="56462-139">The next example shows how to send a POST request that contains BSON.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

<span data-ttu-id="56462-140">Grande parte desse código é o mesmo do exemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="56462-140">Much of this code is the same as the previous example.</span></span> <span data-ttu-id="56462-141">Mas no **PostAsync** método, especifique **BsonMediaTypeFormatter** como o formatador:</span><span class="sxs-lookup"><span data-stu-id="56462-141">But in the **PostAsync** method, specify **BsonMediaTypeFormatter** as the formatter:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a><span data-ttu-id="56462-142">Serialização de tipos primitivos de nível superior</span><span class="sxs-lookup"><span data-stu-id="56462-142">Serializing Top-Level Primitive Types</span></span>

<span data-ttu-id="56462-143">Todos os documentos BSON é uma lista de pares chave/valor. A especificação de BSON não define uma sintaxe para serializar um único valor bruto, como um inteiro ou cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="56462-143">Every BSON document is a list of key/value pairs.The BSON specification does not define a syntax for serializing a single raw value, such as an integer or string.</span></span>

<span data-ttu-id="56462-144">Para contornar essa limitação, o **BsonMediaTypeFormatter** trata tipos primitivos como um caso especial.</span><span class="sxs-lookup"><span data-stu-id="56462-144">To work around this limitation, the **BsonMediaTypeFormatter** treats primitive types as a special case.</span></span> <span data-ttu-id="56462-145">Antes de serializar, ele converte o valor em um par chave/valor com a chave "Value".</span><span class="sxs-lookup"><span data-stu-id="56462-145">Before serializing, it converts the value into a key/value pair with the key "Value".</span></span> <span data-ttu-id="56462-146">Por exemplo, suponha que seu controlador de API retorna um inteiro:</span><span class="sxs-lookup"><span data-stu-id="56462-146">For example, suppose your API controller returns an integer:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

<span data-ttu-id="56462-147">Antes de serialização, o formatador BSON converte esse valor para o par chave/valor a seguir:</span><span class="sxs-lookup"><span data-stu-id="56462-147">Before serializing, the BSON formatter converts this to the following key/value pair:</span></span>

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

<span data-ttu-id="56462-148">Quando você desserializa, o formatador converte os dados para o valor original.</span><span class="sxs-lookup"><span data-stu-id="56462-148">When you deserialize, the formatter converts the data back to the original value.</span></span> <span data-ttu-id="56462-149">No entanto, os clientes que usam um analisador BSON diferente precisará lidar com isso, se sua API web retornar valores brutos.</span><span class="sxs-lookup"><span data-stu-id="56462-149">However, clients using a different BSON parser will need to handle this case, if your web API returns raw values.</span></span> <span data-ttu-id="56462-150">Em geral, você deve considerar retornando dados estruturados, em vez de valores brutos.</span><span class="sxs-lookup"><span data-stu-id="56462-150">In general, you should consider returning structured data, rather than raw values.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="56462-151">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="56462-151">Additional Resources</span></span>

[<span data-ttu-id="56462-152">Exemplo de API BSON Web</span><span class="sxs-lookup"><span data-stu-id="56462-152">Web API BSON Sample</span></span>](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/)

[<span data-ttu-id="56462-153">Formatadores de mídia</span><span class="sxs-lookup"><span data-stu-id="56462-153">Media Formatters</span></span>](media-formatters.md)
