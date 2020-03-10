---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: Formatadores de mídia no ASP.NET Web API 2-ASP.NET 4. x
author: MikeWasson
description: Mostra como dar suporte a formatos de mídia adicionais em ASP.NET Web API para ASP.NET 4. x.
ms.author: riande
ms.date: 01/20/2014
ms.custom: seoapril2019
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: da0c566dad302054d7d0a6435e4c6df178c64772
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557249"
---
# <a name="media-formatters-in-aspnet-web-api-2"></a><span data-ttu-id="c51a6-103">Formatadores de mídia no ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="c51a6-103">Media Formatters in ASP.NET Web API 2</span></span>

<span data-ttu-id="c51a6-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c51a6-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="c51a6-105">Este tutorial mostra como dar suporte a formatos de mídia adicionais no ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="c51a6-105">This tutorial shows how to support additional media formats in ASP.NET Web API.</span></span>

## <a name="internet-media-types"></a><span data-ttu-id="c51a6-106">Tipos de mídia da Internet</span><span class="sxs-lookup"><span data-stu-id="c51a6-106">Internet Media Types</span></span>

<span data-ttu-id="c51a6-107">Um tipo de mídia, também chamado de tipo MIME, identifica o formato de um dado.</span><span class="sxs-lookup"><span data-stu-id="c51a6-107">A media type, also called a MIME type, identifies the format of a piece of data.</span></span> <span data-ttu-id="c51a6-108">No HTTP, os tipos de mídia descrevem o formato do corpo da mensagem.</span><span class="sxs-lookup"><span data-stu-id="c51a6-108">In HTTP, media types describe the format of the message body.</span></span> <span data-ttu-id="c51a6-109">Um tipo de mídia consiste em duas cadeias de caracteres, um tipo e um subtipo.</span><span class="sxs-lookup"><span data-stu-id="c51a6-109">A media type consists of two strings, a type and a subtype.</span></span> <span data-ttu-id="c51a6-110">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="c51a6-110">For example:</span></span>

- <span data-ttu-id="c51a6-111">texto/html</span><span class="sxs-lookup"><span data-stu-id="c51a6-111">text/html</span></span>
- <span data-ttu-id="c51a6-112">image/png</span><span class="sxs-lookup"><span data-stu-id="c51a6-112">image/png</span></span>
- <span data-ttu-id="c51a6-113">aplicativo/json</span><span class="sxs-lookup"><span data-stu-id="c51a6-113">application/json</span></span>

<span data-ttu-id="c51a6-114">Quando uma mensagem HTTP contém um corpo de entidade, o cabeçalho Content-Type Especifica o formato do corpo da mensagem.</span><span class="sxs-lookup"><span data-stu-id="c51a6-114">When an HTTP message contains an entity-body, the Content-Type header specifies the format of the message body.</span></span> <span data-ttu-id="c51a6-115">Isso informa ao receptor como analisar o conteúdo do corpo da mensagem.</span><span class="sxs-lookup"><span data-stu-id="c51a6-115">This tells the receiver how to parse the contents of the message body.</span></span>

<span data-ttu-id="c51a6-116">Por exemplo, se uma resposta HTTP contiver uma imagem PNG, a resposta poderá ter os seguintes cabeçalhos.</span><span class="sxs-lookup"><span data-stu-id="c51a6-116">For example, if an HTTP response contains a PNG image, the response might have the following headers.</span></span>

[!code-console[Main](media-formatters/samples/sample1.cmd)]

<span data-ttu-id="c51a6-117">Quando o cliente envia uma mensagem de solicitação, ele pode incluir um cabeçalho Accept.</span><span class="sxs-lookup"><span data-stu-id="c51a6-117">When the client sends a request message, it can include an Accept header.</span></span> <span data-ttu-id="c51a6-118">O cabeçalho Accept informa ao servidor qual (is) tipo (s) de mídia o cliente deseja do servidor.</span><span class="sxs-lookup"><span data-stu-id="c51a6-118">The Accept header tells the server which media type(s) the client wants from the server.</span></span> <span data-ttu-id="c51a6-119">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="c51a6-119">For example:</span></span>

[!code-console[Main](media-formatters/samples/sample2.cmd)]

<span data-ttu-id="c51a6-120">Esse cabeçalho informa ao servidor que o cliente deseja HTML, XHTML ou XML.</span><span class="sxs-lookup"><span data-stu-id="c51a6-120">This header tells the server that the client wants either HTML, XHTML, or XML.</span></span>

<span data-ttu-id="c51a6-121">O tipo de mídia determina como a API da Web serializa e desserializa o corpo da mensagem HTTP.</span><span class="sxs-lookup"><span data-stu-id="c51a6-121">The media type determines how Web API serializes and deserializes the HTTP message body.</span></span> <span data-ttu-id="c51a6-122">A API Web tem suporte interno para dados XML, JSON, BSON e de formulário UrlEncode, e você pode dar suporte a tipos de mídia adicionais escrevendo um *formatador de mídia*.</span><span class="sxs-lookup"><span data-stu-id="c51a6-122">Web API has built-in support for XML, JSON, BSON, and form-urlencoded data, and you can support additional media types by writing a *media formatter*.</span></span>

<span data-ttu-id="c51a6-123">Para criar um formatador de mídia, derive de uma destas classes:</span><span class="sxs-lookup"><span data-stu-id="c51a6-123">To create a media formatter, derive from one of these classes:</span></span>

- <span data-ttu-id="c51a6-124">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="c51a6-124">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span></span> <span data-ttu-id="c51a6-125">Essa classe usa métodos assíncronos de leitura e gravação.</span><span class="sxs-lookup"><span data-stu-id="c51a6-125">This class uses asynchronous read and write methods.</span></span>
- <span data-ttu-id="c51a6-126">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="c51a6-126">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span></span> <span data-ttu-id="c51a6-127">Essa classe deriva de **MediaTypeFormatter** , mas usa métodos síncronos de leitura/gravação.</span><span class="sxs-lookup"><span data-stu-id="c51a6-127">This class derives from **MediaTypeFormatter** but uses synchronous read/write methods.</span></span>

<span data-ttu-id="c51a6-128">Derivar de **BufferedMediaTypeFormatter** é mais simples, porque não há código assíncrono, mas também significa que o thread de chamada pode ser bloqueado durante e/s.</span><span class="sxs-lookup"><span data-stu-id="c51a6-128">Deriving from **BufferedMediaTypeFormatter** is simpler, because there is no asynchronous code, but it also means the calling thread can block during I/O.</span></span>

## <a name="example-creating-a-csv-media-formatter"></a><span data-ttu-id="c51a6-129">Exemplo: Criando um formatador de mídia CSV</span><span class="sxs-lookup"><span data-stu-id="c51a6-129">Example: Creating a CSV Media Formatter</span></span>

<span data-ttu-id="c51a6-130">O exemplo a seguir mostra um formatador de tipo de mídia que pode serializar um objeto Product em um formato CSV (valores separados por vírgula).</span><span class="sxs-lookup"><span data-stu-id="c51a6-130">The following example shows a media type formatter that can serialize a Product object to a comma-separated values (CSV) format.</span></span> <span data-ttu-id="c51a6-131">Este exemplo usa o tipo de produto definido no tutorial [criando uma API Web que oferece suporte a operações CRUD](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span><span class="sxs-lookup"><span data-stu-id="c51a6-131">This example uses the Product type defined in the tutorial [Creating a Web API that Supports CRUD Operations](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span></span> <span data-ttu-id="c51a6-132">Aqui está a definição do objeto Product:</span><span class="sxs-lookup"><span data-stu-id="c51a6-132">Here is the definition of the Product object:</span></span>

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

<span data-ttu-id="c51a6-133">Para implementar um formatador CSV, defina uma classe derivada de **BufferedMediaTypeFormatter**:</span><span class="sxs-lookup"><span data-stu-id="c51a6-133">To implement a CSV formatter, define a class that derives from **BufferedMediaTypeFormatter**:</span></span>

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

<span data-ttu-id="c51a6-134">No construtor, adicione os tipos de mídia aos quais o formatador dá suporte.</span><span class="sxs-lookup"><span data-stu-id="c51a6-134">In the constructor, add the media types that the formatter supports.</span></span> <span data-ttu-id="c51a6-135">Neste exemplo, o formatador dá suporte a um único tipo de mídia, &quot;texto/CSV&quot;:</span><span class="sxs-lookup"><span data-stu-id="c51a6-135">In this example, the formatter supports a single media type, &quot;text/csv&quot;:</span></span>

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

<span data-ttu-id="c51a6-136">Substitua o método **Canwritetype** para indicar quais tipos o formatador pode serializar:</span><span class="sxs-lookup"><span data-stu-id="c51a6-136">Override the **CanWriteType** method to indicate which types the formatter can serialize:</span></span>

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

<span data-ttu-id="c51a6-137">Neste exemplo, o formatador pode serializar objetos de `Product` único, bem como coleções de objetos `Product`.</span><span class="sxs-lookup"><span data-stu-id="c51a6-137">In this example, the formatter can serialize single `Product` objects as well as collections of `Product` objects.</span></span>

<span data-ttu-id="c51a6-138">Da mesma forma, substitua o método **Canreadtype** para indicar quais tipos o formatador pode desserializar.</span><span class="sxs-lookup"><span data-stu-id="c51a6-138">Similarly, override the **CanReadType** method to indicate which types the formatter can deserialize.</span></span> <span data-ttu-id="c51a6-139">Neste exemplo, o formatador não oferece suporte à desserialização, portanto, o método simplesmente retorna **false**.</span><span class="sxs-lookup"><span data-stu-id="c51a6-139">In this example, the formatter does not support deserialization, so the method simply returns **false**.</span></span>

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

<span data-ttu-id="c51a6-140">Por fim, substitua o método **WriteToStream** .</span><span class="sxs-lookup"><span data-stu-id="c51a6-140">Finally, override the **WriteToStream** method.</span></span> <span data-ttu-id="c51a6-141">Esse método serializa um tipo gravando-o em um fluxo.</span><span class="sxs-lookup"><span data-stu-id="c51a6-141">This method serializes a type by writing it to a stream.</span></span> <span data-ttu-id="c51a6-142">Se o formatador der suporte à desserialização, substitua também o método **ReadFromStream** .</span><span class="sxs-lookup"><span data-stu-id="c51a6-142">If your formatter supports deserialization, also override the **ReadFromStream** method.</span></span>

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a><span data-ttu-id="c51a6-143">Adicionando um formatador de mídia ao pipeline da API Web</span><span class="sxs-lookup"><span data-stu-id="c51a6-143">Adding a Media Formatter to the Web API Pipeline</span></span>

<span data-ttu-id="c51a6-144">Para adicionar um formatador de tipo de mídia ao pipeline da API Web, use a propriedade **Formatters** no objeto **HttpConfiguration** .</span><span class="sxs-lookup"><span data-stu-id="c51a6-144">To add a media type formatter to the Web API pipeline, use the **Formatters** property on the **HttpConfiguration** object.</span></span>

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a><span data-ttu-id="c51a6-145">Codificações de caracteres</span><span class="sxs-lookup"><span data-stu-id="c51a6-145">Character Encodings</span></span>

<span data-ttu-id="c51a6-146">Opcionalmente, um formatador de mídia pode dar suporte a várias codificações de caracteres, como UTF-8 ou ISO 8859-1.</span><span class="sxs-lookup"><span data-stu-id="c51a6-146">Optionally, a media formatter can support multiple character encodings, such as UTF-8 or ISO 8859-1.</span></span>

<span data-ttu-id="c51a6-147">No construtor, adicione um ou mais tipos [System. Text. Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) à coleção **SupportedEncodings** .</span><span class="sxs-lookup"><span data-stu-id="c51a6-147">In the constructor, add one or more [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) types to the **SupportedEncodings** collection.</span></span> <span data-ttu-id="c51a6-148">Coloque a codificação padrão primeiro.</span><span class="sxs-lookup"><span data-stu-id="c51a6-148">Put the default encoding first.</span></span>

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

<span data-ttu-id="c51a6-149">Nos métodos **WriteToStream** e **ReadFromStream** , chame [MediaTypeFormatter. SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) para selecionar a codificação de caractere preferencial.</span><span class="sxs-lookup"><span data-stu-id="c51a6-149">In the **WriteToStream** and **ReadFromStream** methods, call [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) to select the preferred character encoding.</span></span> <span data-ttu-id="c51a6-150">Esse método corresponde aos cabeçalhos de solicitação em relação à lista de codificações com suporte.</span><span class="sxs-lookup"><span data-stu-id="c51a6-150">This method matches the request headers against the list of supported encodings.</span></span> <span data-ttu-id="c51a6-151">Use a **codificação** retornada ao ler ou gravar a partir do fluxo:</span><span class="sxs-lookup"><span data-stu-id="c51a6-151">Use the returned **Encoding** when you read or write from the stream:</span></span>

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
