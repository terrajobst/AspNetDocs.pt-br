---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: 'Enviando dados de formulário HTML na API Web ASP.NET: Upload de arquivo e com diversas partes MIME - ASP.NET 4.x'
author: MikeWasson
description: Este tutorial mostra como carregar arquivos para uma API da web. Ele também descreve como processar dados com várias partes de MIME.
ms.author: riande
ms.date: 06/21/2012
ms.custom: seoapril2019
ms.assetid: a7f3c1b5-69d9-4261-b082-19ffafa5f16a
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-2
msc.type: authoredcontent
ms.openlocfilehash: f5aaebb96f631dfb6b0da1fbca96cd93a6a7fe2d
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65126235"
---
# <a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a><span data-ttu-id="22f2b-104">Enviando dados de formulário HTML na API Web ASP.NET: Carregamento do arquivo e MIME com diversas partes</span><span class="sxs-lookup"><span data-stu-id="22f2b-104">Sending HTML Form Data in ASP.NET Web API: File Upload and Multipart MIME</span></span>

<span data-ttu-id="22f2b-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="22f2b-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-2-file-upload-and-multipart-mime"></a><span data-ttu-id="22f2b-106">Parte 2: Carregamento do arquivo e MIME com diversas partes</span><span class="sxs-lookup"><span data-stu-id="22f2b-106">Part 2: File Upload and Multipart MIME</span></span>

<span data-ttu-id="22f2b-107">Este tutorial mostra como carregar arquivos para uma API da web.</span><span class="sxs-lookup"><span data-stu-id="22f2b-107">This tutorial shows how to upload files to a web API.</span></span> <span data-ttu-id="22f2b-108">Ele também descreve como processar dados com várias partes de MIME.</span><span class="sxs-lookup"><span data-stu-id="22f2b-108">It also describes how to process multipart MIME data.</span></span>

> [!NOTE]
> <span data-ttu-id="22f2b-109">[Baixe o projeto concluído](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span><span class="sxs-lookup"><span data-stu-id="22f2b-109">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span></span>

<span data-ttu-id="22f2b-110">Aqui está um exemplo de um formulário HTML para carregar um arquivo:</span><span class="sxs-lookup"><span data-stu-id="22f2b-110">Here is an example of an HTML form for uploading a file:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

<span data-ttu-id="22f2b-111">Este formulário contém um controle de entrada de texto e um controle de entrada do arquivo.</span><span class="sxs-lookup"><span data-stu-id="22f2b-111">This form contains a text input control and a file input control.</span></span> <span data-ttu-id="22f2b-112">Quando um formulário contém um controle de entrada de arquivo, o **enctype** atributo deve ser sempre &quot;multipart/form-data&quot;, que especifica que o formulário será enviado como uma mensagem MIME de várias partes.</span><span class="sxs-lookup"><span data-stu-id="22f2b-112">When a form contains a file input control, the **enctype** attribute should always be &quot;multipart/form-data&quot;, which specifies that the form will be sent as a multipart MIME message.</span></span>

<span data-ttu-id="22f2b-113">O formato de uma mensagem MIME de várias partes é mais fácil de entender examinando um exemplo de solicitação:</span><span class="sxs-lookup"><span data-stu-id="22f2b-113">The format of a multipart MIME message is easiest to understand by looking at an example request:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

<span data-ttu-id="22f2b-114">Esta mensagem é dividida em duas *partes*, uma para cada controle de formulário.</span><span class="sxs-lookup"><span data-stu-id="22f2b-114">This message is divided into two *parts*, one for each form control.</span></span> <span data-ttu-id="22f2b-115">Limites de parte são indicados por linhas que começam com traços.</span><span class="sxs-lookup"><span data-stu-id="22f2b-115">Part boundaries are indicated by the lines that start with dashes.</span></span>

> [!NOTE]
> <span data-ttu-id="22f2b-116">O limite de parte inclui um componente aleatório (&quot;41184676334&quot;) para garantir que a cadeia de caracteres de limite não acidentalmente aparece dentro de uma parte da mensagem.</span><span class="sxs-lookup"><span data-stu-id="22f2b-116">The part boundary includes a random component (&quot;41184676334&quot;) to ensure that the boundary string does not accidentally appear inside a message part.</span></span>

<span data-ttu-id="22f2b-117">Cada parte da mensagem contém um ou mais cabeçalhos, seguidos pelo conteúdo de parte.</span><span class="sxs-lookup"><span data-stu-id="22f2b-117">Each message part contains one or more headers, followed by the part contents.</span></span>

- <span data-ttu-id="22f2b-118">O cabeçalho Content-Disposition inclui o nome do controle.</span><span class="sxs-lookup"><span data-stu-id="22f2b-118">The Content-Disposition header includes the name of the control.</span></span> <span data-ttu-id="22f2b-119">Para arquivos, ele também contém o nome do arquivo.</span><span class="sxs-lookup"><span data-stu-id="22f2b-119">For files, it also contains the file name.</span></span>
- <span data-ttu-id="22f2b-120">O cabeçalho Content-Type descreve os dados na parte.</span><span class="sxs-lookup"><span data-stu-id="22f2b-120">The Content-Type header describes the data in the part.</span></span> <span data-ttu-id="22f2b-121">Se esse cabeçalho for omitido, o padrão é texto/simples.</span><span class="sxs-lookup"><span data-stu-id="22f2b-121">If this header is omitted, the default is text/plain.</span></span>

<span data-ttu-id="22f2b-122">No exemplo anterior, o usuário carregou um arquivo chamado GrandCanyon.jpg, com o tipo de conteúdo de imagem/jpeg; e o valor da entrada de texto era &quot;férias de verão&quot;.</span><span class="sxs-lookup"><span data-stu-id="22f2b-122">In the previous example, the user uploaded a file named GrandCanyon.jpg, with content type image/jpeg; and the value of the text input was &quot;Summer Vacation&quot;.</span></span>

## <a name="file-upload"></a><span data-ttu-id="22f2b-123">Upload de arquivo</span><span class="sxs-lookup"><span data-stu-id="22f2b-123">File Upload</span></span>

<span data-ttu-id="22f2b-124">Agora vamos dar uma olhada em um controlador de API da Web que lê arquivos de uma mensagem MIME de várias partes.</span><span class="sxs-lookup"><span data-stu-id="22f2b-124">Now let's look at a Web API controller that reads files from a multipart MIME message.</span></span> <span data-ttu-id="22f2b-125">O controlador lerá os arquivos de forma assíncrona.</span><span class="sxs-lookup"><span data-stu-id="22f2b-125">The controller will read the files asynchronously.</span></span> <span data-ttu-id="22f2b-126">API da Web dá suporte a ações assíncronas usando o [modelo de programação baseado em tarefa](https://msdn.microsoft.com/library/dd460693.aspx).</span><span class="sxs-lookup"><span data-stu-id="22f2b-126">Web API supports asynchronous actions using the [task-based programming model](https://msdn.microsoft.com/library/dd460693.aspx).</span></span> <span data-ttu-id="22f2b-127">Primeiro, aqui está o código se você estiver direcionando o .NET Framework 4.5, que oferece suporte a **async** e **await** palavras-chave.</span><span class="sxs-lookup"><span data-stu-id="22f2b-127">First, here is the code if you are targeting .NET Framework 4.5, which supports the **async** and **await** keywords.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

<span data-ttu-id="22f2b-128">Observe que a ação do controlador não leva nenhum parâmetro.</span><span class="sxs-lookup"><span data-stu-id="22f2b-128">Notice that the controller action does not take any parameters.</span></span> <span data-ttu-id="22f2b-129">Isso ocorre porque podemos processar o corpo da solicitação dentro da ação, sem invocar um formatador de tipo de mídia.</span><span class="sxs-lookup"><span data-stu-id="22f2b-129">That's because we process the request body inside the action, without invoking a media-type formatter.</span></span>

<span data-ttu-id="22f2b-130">O **IsMultipartContent** método verifica se a solicitação contém uma mensagem MIME de várias partes.</span><span class="sxs-lookup"><span data-stu-id="22f2b-130">The **IsMultipartContent** method checks whether the request contains a multipart MIME message.</span></span> <span data-ttu-id="22f2b-131">Caso contrário, o controlador retorna o código de status HTTP 415 (tipo de mídia sem suporte).</span><span class="sxs-lookup"><span data-stu-id="22f2b-131">If not, the controller returns HTTP status code 415 (Unsupported Media Type).</span></span>

<span data-ttu-id="22f2b-132">O **MultipartFormDataStreamProvider** classe é um objeto auxiliar que aloca fluxos de arquivos para arquivos carregados.</span><span class="sxs-lookup"><span data-stu-id="22f2b-132">The **MultipartFormDataStreamProvider** class is a helper object that allocates file streams for uploaded files.</span></span> <span data-ttu-id="22f2b-133">Para ler a mensagem MIME de várias partes, chame o **ReadAsMultipartAsync** método.</span><span class="sxs-lookup"><span data-stu-id="22f2b-133">To read the multipart MIME message, call the **ReadAsMultipartAsync** method.</span></span> <span data-ttu-id="22f2b-134">Esse método extrai todas as partes da mensagem e grava-os em fluxos fornecidos pelo **MultipartFormDataStreamProvider**.</span><span class="sxs-lookup"><span data-stu-id="22f2b-134">This method extracts all of the message parts and writes them into the streams provided by the **MultipartFormDataStreamProvider**.</span></span>

<span data-ttu-id="22f2b-135">Quando o método for concluído, você pode obter informações sobre os arquivos a partir de **FileData** propriedade, que é uma coleção de **MultipartFileData** objetos.</span><span class="sxs-lookup"><span data-stu-id="22f2b-135">When the method completes, you can get information about the files from the **FileData** property, which is a collection of **MultipartFileData** objects.</span></span>

- <span data-ttu-id="22f2b-136">**MultipartFileData.FileName** é o nome de arquivo local no servidor, onde o arquivo foi salvo.</span><span class="sxs-lookup"><span data-stu-id="22f2b-136">**MultipartFileData.FileName** is the local file name on the server, where the file was saved.</span></span>
- <span data-ttu-id="22f2b-137">**MultipartFileData.Headers** contém o cabeçalho da parte (*não* o cabeçalho de solicitação).</span><span class="sxs-lookup"><span data-stu-id="22f2b-137">**MultipartFileData.Headers** contains the part header (*not* the request header).</span></span> <span data-ttu-id="22f2b-138">Você pode usar isso para acessar o conteúdo\_cabeçalhos Disposition e Content-Type.</span><span class="sxs-lookup"><span data-stu-id="22f2b-138">You can use this to access the Content\_Disposition and Content-Type headers.</span></span>

<span data-ttu-id="22f2b-139">Como o nome sugere, **ReadAsMultipartAsync** é um método assíncrono.</span><span class="sxs-lookup"><span data-stu-id="22f2b-139">As the name suggests, **ReadAsMultipartAsync** is an asynchronous method.</span></span> <span data-ttu-id="22f2b-140">Para executar o trabalho depois que o método for concluído, use uma [tarefa de continuação](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) ou o **await** palavra-chave (.NET 4.5).</span><span class="sxs-lookup"><span data-stu-id="22f2b-140">To perform work after the method completes, use a [continuation task](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) or the **await** keyword (.NET 4.5).</span></span>

<span data-ttu-id="22f2b-141">Aqui está a versão do .NET Framework 4.0 do código anterior:</span><span class="sxs-lookup"><span data-stu-id="22f2b-141">Here is the .NET Framework 4.0 version of the previous code:</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a><span data-ttu-id="22f2b-142">Leitura de dados de controle de formulário</span><span class="sxs-lookup"><span data-stu-id="22f2b-142">Reading Form Control Data</span></span>

<span data-ttu-id="22f2b-143">O formulário HTML que mostrei anteriormente tinha um controle de entrada de texto.</span><span class="sxs-lookup"><span data-stu-id="22f2b-143">The HTML form that I showed earlier had a text input control.</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

<span data-ttu-id="22f2b-144">Você pode obter o valor do controle do **FormData** propriedade da **MultipartFormDataStreamProvider**.</span><span class="sxs-lookup"><span data-stu-id="22f2b-144">You can get the value of the control from the **FormData** property of the **MultipartFormDataStreamProvider**.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

<span data-ttu-id="22f2b-145">**FormData** é um **NameValueCollection** que contém pares nome/valor para os controles de formulário.</span><span class="sxs-lookup"><span data-stu-id="22f2b-145">**FormData** is a **NameValueCollection** that contains name/value pairs for the form controls.</span></span> <span data-ttu-id="22f2b-146">A coleção pode conter chaves duplicadas.</span><span class="sxs-lookup"><span data-stu-id="22f2b-146">The collection can contain duplicate keys.</span></span> <span data-ttu-id="22f2b-147">Considere este formulário:</span><span class="sxs-lookup"><span data-stu-id="22f2b-147">Consider this form:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

<span data-ttu-id="22f2b-148">O corpo da solicitação pode ter esta aparência:</span><span class="sxs-lookup"><span data-stu-id="22f2b-148">The request body might look like this:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

<span data-ttu-id="22f2b-149">Nesse caso, o **FormData** coleção conteria os seguintes pares chave/valor:</span><span class="sxs-lookup"><span data-stu-id="22f2b-149">In that case, the **FormData** collection would contain the following key/value pairs:</span></span>

- <span data-ttu-id="22f2b-150">viagem: ida e volta</span><span class="sxs-lookup"><span data-stu-id="22f2b-150">trip: round-trip</span></span>
- <span data-ttu-id="22f2b-151">options: nonstop</span><span class="sxs-lookup"><span data-stu-id="22f2b-151">options: nonstop</span></span>
- <span data-ttu-id="22f2b-152">opções: datas</span><span class="sxs-lookup"><span data-stu-id="22f2b-152">options: dates</span></span>
- <span data-ttu-id="22f2b-153">estação: janela</span><span class="sxs-lookup"><span data-stu-id="22f2b-153">seat: window</span></span>
