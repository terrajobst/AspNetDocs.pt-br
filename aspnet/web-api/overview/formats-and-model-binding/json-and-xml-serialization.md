---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: JSON e a serialização de XML na API Web ASP.NET - ASP.NET 4.x
author: MikeWasson
description: Descreve os formatadores JSON e XML na API Web ASP.NET para ASP.NET 4. x.
ms.author: riande
ms.date: 05/30/2012
ms.custom: seoapril2019
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: a9e7ed63a55c146976e0221214e722f3a2292fee
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59408271"
---
# <a name="json-and-xml-serialization-in-aspnet-web-api"></a><span data-ttu-id="d7791-103">JSON e a serialização de XML na API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d7791-103">JSON and XML Serialization in ASP.NET Web API</span></span>

<span data-ttu-id="d7791-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="d7791-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="d7791-105">Este artigo descreve os formatadores JSON e XML na API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d7791-105">This article describes the JSON and XML formatters in ASP.NET Web API.</span></span>

<span data-ttu-id="d7791-106">Na API Web ASP.NET, uma *formatador do tipo de mídia* é um objeto que pode:</span><span class="sxs-lookup"><span data-stu-id="d7791-106">In ASP.NET Web API, a *media-type formatter* is an object that can:</span></span>

- <span data-ttu-id="d7791-107">Corpo da mensagem de objetos CLR de leitura de um HTTP</span><span class="sxs-lookup"><span data-stu-id="d7791-107">Read CLR objects from an HTTP message body</span></span>
- <span data-ttu-id="d7791-108">Gravar objetos CLR em um corpo de mensagem HTTP</span><span class="sxs-lookup"><span data-stu-id="d7791-108">Write CLR objects into an HTTP message body</span></span>

<span data-ttu-id="d7791-109">API da Web fornece formatadores de tipo de mídia para JSON e XML.</span><span class="sxs-lookup"><span data-stu-id="d7791-109">Web API provides media-type formatters for both JSON and XML.</span></span> <span data-ttu-id="d7791-110">O framework insere essas formatadores no pipeline por padrão.</span><span class="sxs-lookup"><span data-stu-id="d7791-110">The framework inserts these formatters into the pipeline by default.</span></span> <span data-ttu-id="d7791-111">Os clientes podem solicitar o JSON ou XML no cabeçalho Accept da solicitação HTTP.</span><span class="sxs-lookup"><span data-stu-id="d7791-111">Clients can request either JSON or XML in the Accept header of the HTTP request.</span></span>

## <a name="contents"></a><span data-ttu-id="d7791-112">Conteúdo</span><span class="sxs-lookup"><span data-stu-id="d7791-112">Contents</span></span>

- [<span data-ttu-id="d7791-113">Formatador de tipo de mídia do JSON</span><span class="sxs-lookup"><span data-stu-id="d7791-113">JSON Media-Type Formatter</span></span>](#json_media_type_formatter)

    - [<span data-ttu-id="d7791-114">Propriedades somente leitura</span><span class="sxs-lookup"><span data-stu-id="d7791-114">Read-Only Properties</span></span>](#json_readonly)
    - [<span data-ttu-id="d7791-115">Datas</span><span class="sxs-lookup"><span data-stu-id="d7791-115">Dates</span></span>](#json_dates)
    - [<span data-ttu-id="d7791-116">Recuar</span><span class="sxs-lookup"><span data-stu-id="d7791-116">Indenting</span></span>](#json_indenting)
    - [<span data-ttu-id="d7791-117">Concatenação com maiusculas e minúsculas</span><span class="sxs-lookup"><span data-stu-id="d7791-117">Camel Casing</span></span>](#json_camelcasing)
    - [<span data-ttu-id="d7791-118">Objetos anônimos e com tipagem fraca</span><span class="sxs-lookup"><span data-stu-id="d7791-118">Anonymous and Weakly-Typed Objects</span></span>](#json_anon)
- [<span data-ttu-id="d7791-119">Formatador de tipo de mídia do XML</span><span class="sxs-lookup"><span data-stu-id="d7791-119">XML Media-Type Formatter</span></span>](#xml_media_type_formatter)

    - [<span data-ttu-id="d7791-120">Propriedades somente leitura</span><span class="sxs-lookup"><span data-stu-id="d7791-120">Read-Only Properties</span></span>](#xml_readonly)
    - [<span data-ttu-id="d7791-121">Datas</span><span class="sxs-lookup"><span data-stu-id="d7791-121">Dates</span></span>](#xml_dates)
    - [<span data-ttu-id="d7791-122">Recuar</span><span class="sxs-lookup"><span data-stu-id="d7791-122">Indenting</span></span>](#xml_indenting)
    - [<span data-ttu-id="d7791-123">Serializadores XML de por tipo de configuração</span><span class="sxs-lookup"><span data-stu-id="d7791-123">Setting Per-Type XML Serializers</span></span>](#xml_pertype)
- [<span data-ttu-id="d7791-124">Removendo o formatador XML ou JSON</span><span class="sxs-lookup"><span data-stu-id="d7791-124">Removing the JSON or XML Formatter</span></span>](#removing_the_json_or_xml_formatter)
- [<span data-ttu-id="d7791-125">Tratamento de referências de objeto Circular</span><span class="sxs-lookup"><span data-stu-id="d7791-125">Handling Circular Object References</span></span>](#handling_circular_object_references)
- [<span data-ttu-id="d7791-126">Testando a serialização de objeto</span><span class="sxs-lookup"><span data-stu-id="d7791-126">Testing Object Serialization</span></span>](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a><span data-ttu-id="d7791-127">Formatador de tipo de mídia do JSON</span><span class="sxs-lookup"><span data-stu-id="d7791-127">JSON Media-Type Formatter</span></span>

<span data-ttu-id="d7791-128">Formatação do JSON é fornecido pelo **JsonMediaTypeFormatter** classe.</span><span class="sxs-lookup"><span data-stu-id="d7791-128">JSON formatting is provided by the **JsonMediaTypeFormatter** class.</span></span> <span data-ttu-id="d7791-129">Por padrão, **JsonMediaTypeFormatter** usa o [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) biblioteca para realizar a serialização.</span><span class="sxs-lookup"><span data-stu-id="d7791-129">By default, **JsonMediaTypeFormatter** uses the [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) library to perform serialization.</span></span> <span data-ttu-id="d7791-130">Json.NET é um projeto de software livre de terceiros.</span><span class="sxs-lookup"><span data-stu-id="d7791-130">Json.NET is a third-party open source project.</span></span>

<span data-ttu-id="d7791-131">Se você preferir, você pode configurar o **JsonMediaTypeFormatter** classe para usar o **DataContractJsonSerializer** em vez de Json.NET.</span><span class="sxs-lookup"><span data-stu-id="d7791-131">If you prefer, you can configure the **JsonMediaTypeFormatter** class to use the **DataContractJsonSerializer** instead of Json.NET.</span></span> <span data-ttu-id="d7791-132">Para fazer isso, defina as **UseDataContractJsonSerializer** propriedade **verdadeiro**:</span><span class="sxs-lookup"><span data-stu-id="d7791-132">To do so, set the **UseDataContractJsonSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a><span data-ttu-id="d7791-133">Serialização JSON</span><span class="sxs-lookup"><span data-stu-id="d7791-133">JSON Serialization</span></span>

<span data-ttu-id="d7791-134">Esta seção descreve alguns comportamentos específicos do formatador JSON, usando o padrão [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializador.</span><span class="sxs-lookup"><span data-stu-id="d7791-134">This section describes some specific behaviors of the JSON formatter, using the default [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializer.</span></span> <span data-ttu-id="d7791-135">Isso não deve ser uma documentação abrangente da biblioteca Json.NET; Para obter mais informações, consulte o [documentação do Json.NET](http://james.newtonking.com/projects/json/help/).</span><span class="sxs-lookup"><span data-stu-id="d7791-135">This is not meant to be comprehensive documentation of the Json.NET library; for more information, see the [Json.NET Documentation](http://james.newtonking.com/projects/json/help/).</span></span>

#### <a name="what-gets-serialized"></a><span data-ttu-id="d7791-136">O que é serializado?</span><span class="sxs-lookup"><span data-stu-id="d7791-136">What Gets Serialized?</span></span>

<span data-ttu-id="d7791-137">Por padrão, todas as propriedades e campos públicos são incluídos no JSON serializado.</span><span class="sxs-lookup"><span data-stu-id="d7791-137">By default, all public properties and fields are included in the serialized JSON.</span></span> <span data-ttu-id="d7791-138">Para omitir uma propriedade ou campo, decorá-la com o **JsonIgnore** atributo.</span><span class="sxs-lookup"><span data-stu-id="d7791-138">To omit a property or field, decorate it with the **JsonIgnore** attribute.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

<span data-ttu-id="d7791-139">Se você preferir uma &quot;aceitação&quot; abordar, decore a classe com o **DataContract** atributo.</span><span class="sxs-lookup"><span data-stu-id="d7791-139">If you prefer an &quot;opt-in&quot; approach, decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="d7791-140">Se esse atributo estiver presente, os membros serão ignorados a menos que tenham o **DataMember**.</span><span class="sxs-lookup"><span data-stu-id="d7791-140">If this attribute is present, members are ignored unless they have the **DataMember**.</span></span> <span data-ttu-id="d7791-141">Você também pode usar **DataMember** para serializar os membros privados.</span><span class="sxs-lookup"><span data-stu-id="d7791-141">You can also use **DataMember** to serialize private members.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="d7791-142">Propriedades somente leitura</span><span class="sxs-lookup"><span data-stu-id="d7791-142">Read-Only Properties</span></span>

<span data-ttu-id="d7791-143">Propriedades somente leitura são serializadas por padrão.</span><span class="sxs-lookup"><span data-stu-id="d7791-143">Read-only properties are serialized by default.</span></span>

<a id="json_dates"></a>
### <a name="dates"></a><span data-ttu-id="d7791-144">Datas</span><span class="sxs-lookup"><span data-stu-id="d7791-144">Dates</span></span>

<span data-ttu-id="d7791-145">Por padrão, o Json.NET grava as datas [ISO 8601](http://www.w3.org/TR/NOTE-datetime) formato.</span><span class="sxs-lookup"><span data-stu-id="d7791-145">By default, Json.NET writes dates in [ISO 8601](http://www.w3.org/TR/NOTE-datetime) format.</span></span> <span data-ttu-id="d7791-146">As datas em UTC (Coordinated Universal Time) são gravadas com um sufixo "Z".</span><span class="sxs-lookup"><span data-stu-id="d7791-146">Dates in UTC (Coordinated Universal Time) are written with a "Z" suffix.</span></span> <span data-ttu-id="d7791-147">As datas no horário local incluem um deslocamento de fuso horário.</span><span class="sxs-lookup"><span data-stu-id="d7791-147">Dates in local time include a time-zone offset.</span></span> <span data-ttu-id="d7791-148">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="d7791-148">For example:</span></span>

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

<span data-ttu-id="d7791-149">Por padrão, o Json.NET preserva o fuso horário.</span><span class="sxs-lookup"><span data-stu-id="d7791-149">By default, Json.NET preserves the time zone.</span></span> <span data-ttu-id="d7791-150">Você pode substituir isso definindo a propriedade DateTimeZoneHandling:</span><span class="sxs-lookup"><span data-stu-id="d7791-150">You can override this by setting the DateTimeZoneHandling property:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

<span data-ttu-id="d7791-151">Se você preferir usar [formato de data JSON Microsoft](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) em vez de ISO 8601, defina as **DateFormatHandling** propriedade nas configurações do serializador:</span><span class="sxs-lookup"><span data-stu-id="d7791-151">If you prefer to use [Microsoft JSON date format](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) instead of ISO 8601, set the **DateFormatHandling** property on the serializer settings:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="d7791-152">Recuar</span><span class="sxs-lookup"><span data-stu-id="d7791-152">Indenting</span></span>

<span data-ttu-id="d7791-153">Para gravar recuado JSON, defina as **formatação** definir como **Formatting.Indented**:</span><span class="sxs-lookup"><span data-stu-id="d7791-153">To write indented JSON, set the **Formatting** setting to **Formatting.Indented**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a><span data-ttu-id="d7791-154">Concatenação com maiusculas e minúsculas</span><span class="sxs-lookup"><span data-stu-id="d7791-154">Camel Casing</span></span>

<span data-ttu-id="d7791-155">Para gravar os nomes de propriedade JSON com concatenação com maiusculas e minúsculas, sem alterar seu modelo de dados, defina a **CamelCasePropertyNamesContractResolver** sobre o serializador:</span><span class="sxs-lookup"><span data-stu-id="d7791-155">To write JSON property names with camel casing, without changing your data model, set the **CamelCasePropertyNamesContractResolver** on the serializer:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a><span data-ttu-id="d7791-156">Objetos anônimos e com tipagem fraca</span><span class="sxs-lookup"><span data-stu-id="d7791-156">Anonymous and Weakly-Typed Objects</span></span>

<span data-ttu-id="d7791-157">Um método de ação pode retornar um objeto anônimo e serializá-lo em JSON.</span><span class="sxs-lookup"><span data-stu-id="d7791-157">An action method can return an anonymous object and serialize it to JSON.</span></span> <span data-ttu-id="d7791-158">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="d7791-158">For example:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

<span data-ttu-id="d7791-159">O corpo da mensagem de resposta conterá o JSON a seguir:</span><span class="sxs-lookup"><span data-stu-id="d7791-159">The response message body will contain the following JSON:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

<span data-ttu-id="d7791-160">Se sua web API recebe vagamente estruturados objetos JSON de clientes, é possível desserializar o corpo da solicitação para um **Newtonsoft.Json.Linq.JObject** tipo.</span><span class="sxs-lookup"><span data-stu-id="d7791-160">If your web API receives loosely structured JSON objects from clients, you can deserialize the request body to a **Newtonsoft.Json.Linq.JObject** type.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

<span data-ttu-id="d7791-161">No entanto, é melhor usar objetos com rigidez de tipos de dados.</span><span class="sxs-lookup"><span data-stu-id="d7791-161">However, it is usually better to use strongly typed data objects.</span></span> <span data-ttu-id="d7791-162">Em seguida, você não precisa analisar os dados por conta própria, e você obtém os benefícios da validação do modelo.</span><span class="sxs-lookup"><span data-stu-id="d7791-162">Then you don't need to parse the data yourself, and you get the benefits of model validation.</span></span>

<span data-ttu-id="d7791-163">O serializador XML não oferece suporte a tipos anônimos ou **JObject** instâncias.</span><span class="sxs-lookup"><span data-stu-id="d7791-163">The XML serializer does not support anonymous types or **JObject** instances.</span></span> <span data-ttu-id="d7791-164">Se você usar esses recursos para os dados JSON, você deve remover o formatador XML do pipeline, conforme descrito mais adiante neste artigo.</span><span class="sxs-lookup"><span data-stu-id="d7791-164">If you use these features for your JSON data, you should remove the XML formatter from the pipeline, as described later in this article.</span></span>

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a><span data-ttu-id="d7791-165">Formatador de tipo de mídia do XML</span><span class="sxs-lookup"><span data-stu-id="d7791-165">XML Media-Type Formatter</span></span>

<span data-ttu-id="d7791-166">Formatação XML é fornecido pelo **XmlMediaTypeFormatter** classe.</span><span class="sxs-lookup"><span data-stu-id="d7791-166">XML formatting is provided by the **XmlMediaTypeFormatter** class.</span></span> <span data-ttu-id="d7791-167">Por padrão, **XmlMediaTypeFormatter** usa o **DataContractSerializer** classe para executar a serialização.</span><span class="sxs-lookup"><span data-stu-id="d7791-167">By default, **XmlMediaTypeFormatter** uses the **DataContractSerializer** class to perform serialization.</span></span>

<span data-ttu-id="d7791-168">Se você preferir, você pode configurar o **XmlMediaTypeFormatter** usar o **XmlSerializer** em vez da **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="d7791-168">If you prefer, you can configure the **XmlMediaTypeFormatter** to use the **XmlSerializer** instead of the **DataContractSerializer**.</span></span> <span data-ttu-id="d7791-169">Para fazer isso, defina as **UseXmlSerializer** propriedade **verdadeiro**:</span><span class="sxs-lookup"><span data-stu-id="d7791-169">To do so, set the **UseXmlSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

<span data-ttu-id="d7791-170">O **XmlSerializer** classe dá suporte a um conjunto mais estreito de tipos que **DataContractSerializer**, mas oferece mais controle sobre o XML resultante.</span><span class="sxs-lookup"><span data-stu-id="d7791-170">The **XmlSerializer** class supports a narrower set of types than **DataContractSerializer**, but gives more control over the resulting XML.</span></span> <span data-ttu-id="d7791-171">Considere o uso de **XmlSerializer** se você precisar corresponder a um esquema XML existente.</span><span class="sxs-lookup"><span data-stu-id="d7791-171">Consider using **XmlSerializer** if you need to match an existing XML schema.</span></span>

### <a name="xml-serialization"></a><span data-ttu-id="d7791-172">Serialização XML</span><span class="sxs-lookup"><span data-stu-id="d7791-172">XML Serialization</span></span>

<span data-ttu-id="d7791-173">Esta seção descreve alguns comportamentos específicos do formatador XML, usando o padrão **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="d7791-173">This section describes some specific behaviors of the XML formatter, using the default **DataContractSerializer**.</span></span>

<span data-ttu-id="d7791-174">Por padrão, o DataContractSerializer se comporta da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="d7791-174">By default, the DataContractSerializer behaves as follows:</span></span>

- <span data-ttu-id="d7791-175">Todos os campos e propriedades de leitura/gravação pública são serializados.</span><span class="sxs-lookup"><span data-stu-id="d7791-175">All public read/write properties and fields are serialized.</span></span> <span data-ttu-id="d7791-176">Para omitir uma propriedade ou campo, decorá-la com o **IgnoreDataMember** atributo.</span><span class="sxs-lookup"><span data-stu-id="d7791-176">To omit a property or field, decorate it with the **IgnoreDataMember** attribute.</span></span>
- <span data-ttu-id="d7791-177">Membros particulares e protegidos não são serializados.</span><span class="sxs-lookup"><span data-stu-id="d7791-177">Private and protected members are not serialized.</span></span>
- <span data-ttu-id="d7791-178">Propriedades somente leitura não são serializadas.</span><span class="sxs-lookup"><span data-stu-id="d7791-178">Read-only properties are not serialized.</span></span> <span data-ttu-id="d7791-179">(No entanto, o conteúdo de uma propriedade de coleção somente leitura é serializado.)</span><span class="sxs-lookup"><span data-stu-id="d7791-179">(However, the contents of a read-only collection property are serialized.)</span></span>
- <span data-ttu-id="d7791-180">Nomes de classe e membro são gravados no XML exatamente como aparecem na declaração da classe.</span><span class="sxs-lookup"><span data-stu-id="d7791-180">Class and member names are written in the XML exactly as they appear in the class declaration.</span></span>
- <span data-ttu-id="d7791-181">Um namespace XML padrão é usado.</span><span class="sxs-lookup"><span data-stu-id="d7791-181">A default XML namespace is used.</span></span>

<span data-ttu-id="d7791-182">Se você precisar de mais controle sobre a serialização, você pode decorar a classe com o **DataContract** atributo.</span><span class="sxs-lookup"><span data-stu-id="d7791-182">If you need more control over the serialization, you can decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="d7791-183">Quando esse atributo estiver presente, a classe é serializada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="d7791-183">When this attribute is present, the class is serialized as follows:</span></span>

- <span data-ttu-id="d7791-184">&quot;Aceitar&quot; abordagem: Propriedades e campos não são serializados por padrão.</span><span class="sxs-lookup"><span data-stu-id="d7791-184">&quot;Opt in&quot; approach: Properties and fields are not serialized by default.</span></span> <span data-ttu-id="d7791-185">Para serializar uma propriedade ou campo, decorá-la com o **DataMember** atributo.</span><span class="sxs-lookup"><span data-stu-id="d7791-185">To serialize a property or field, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="d7791-186">Para serializar um membro particular ou protegido, decorá-la com o **DataMember** atributo.</span><span class="sxs-lookup"><span data-stu-id="d7791-186">To serialize a private or protected member, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="d7791-187">Propriedades somente leitura não são serializadas.</span><span class="sxs-lookup"><span data-stu-id="d7791-187">Read-only properties are not serialized.</span></span>
- <span data-ttu-id="d7791-188">Para alterar como o nome de classe aparece no XML, defina as *nome* parâmetro na **DataContract** atributo.</span><span class="sxs-lookup"><span data-stu-id="d7791-188">To change how the class name appears in the XML, set the *Name* parameter in the **DataContract** attribute.</span></span>
- <span data-ttu-id="d7791-189">Para alterar a aparência de um nome de membro no XML, defina as *nome* parâmetro na **DataMember** atributo.</span><span class="sxs-lookup"><span data-stu-id="d7791-189">To change how a member name appears in the XML, set the *Name* parameter in the **DataMember** attribute.</span></span>
- <span data-ttu-id="d7791-190">Para alterar o namespace de XML, defina as *Namespace* parâmetro na **DataContract** classe.</span><span class="sxs-lookup"><span data-stu-id="d7791-190">To change the XML namespace, set the *Namespace* parameter in the **DataContract** class.</span></span>

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="d7791-191">Propriedades somente leitura</span><span class="sxs-lookup"><span data-stu-id="d7791-191">Read-Only Properties</span></span>

<span data-ttu-id="d7791-192">Propriedades somente leitura não são serializadas.</span><span class="sxs-lookup"><span data-stu-id="d7791-192">Read-only properties are not serialized.</span></span> <span data-ttu-id="d7791-193">Se uma propriedade somente leitura tem um campo de suporte particular, você pode marcar um campo particular com o **DataMember** atributo.</span><span class="sxs-lookup"><span data-stu-id="d7791-193">If a read-only property has a backing private field, you can mark the private field with the **DataMember** attribute.</span></span> <span data-ttu-id="d7791-194">Essa abordagem requer o **DataContract** atributo na classe.</span><span class="sxs-lookup"><span data-stu-id="d7791-194">This approach requires the **DataContract** attribute on the class.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a><span data-ttu-id="d7791-195">Datas</span><span class="sxs-lookup"><span data-stu-id="d7791-195">Dates</span></span>

<span data-ttu-id="d7791-196">As datas são gravadas no formato ISO 8601.</span><span class="sxs-lookup"><span data-stu-id="d7791-196">Dates are written in ISO 8601 format.</span></span> <span data-ttu-id="d7791-197">Por exemplo, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span><span class="sxs-lookup"><span data-stu-id="d7791-197">For example, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span></span>

<a id="xml_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="d7791-198">Recuar</span><span class="sxs-lookup"><span data-stu-id="d7791-198">Indenting</span></span>

<span data-ttu-id="d7791-199">Para gravar o XML recuado, defina as **Recuar** propriedade **verdadeiro**:</span><span class="sxs-lookup"><span data-stu-id="d7791-199">To write indented XML, set the **Indent** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a><span data-ttu-id="d7791-200">Serializadores XML de por tipo de configuração</span><span class="sxs-lookup"><span data-stu-id="d7791-200">Setting Per-Type XML Serializers</span></span>

<span data-ttu-id="d7791-201">Você pode definir diferentes serializadores XML para diferentes tipos CLR.</span><span class="sxs-lookup"><span data-stu-id="d7791-201">You can set different XML serializers for different CLR types.</span></span> <span data-ttu-id="d7791-202">Por exemplo, você pode ter um objeto de dados específico que requer **XmlSerializer** para compatibilidade com versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="d7791-202">For example, you might have a particular data object that requires **XmlSerializer** for backward compatibility.</span></span> <span data-ttu-id="d7791-203">Você pode usar **XmlSerializer** para este objeto e continuar a usar **DataContractSerializer** para outros tipos.</span><span class="sxs-lookup"><span data-stu-id="d7791-203">You can use **XmlSerializer** for this object and continue to use **DataContractSerializer** for other types.</span></span>

<span data-ttu-id="d7791-204">Para definir um serializador XML para um tipo específico, chame **SetSerializer**.</span><span class="sxs-lookup"><span data-stu-id="d7791-204">To set an XML serializer for a particular type, call **SetSerializer**.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

<span data-ttu-id="d7791-205">Você pode especificar uma **XmlSerializer** ou qualquer objeto derivado de **XmlObjectSerializer**.</span><span class="sxs-lookup"><span data-stu-id="d7791-205">You can specify an **XmlSerializer** or any object that derives from **XmlObjectSerializer**.</span></span>

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a><span data-ttu-id="d7791-206">Removendo o formatador XML ou JSON</span><span class="sxs-lookup"><span data-stu-id="d7791-206">Removing the JSON or XML Formatter</span></span>

<span data-ttu-id="d7791-207">Você pode remover o formatador JSON ou o formatador XML da lista de formatadores, se você não quiser usá-los.</span><span class="sxs-lookup"><span data-stu-id="d7791-207">You can remove the JSON formatter or the XML formatter from the list of formatters, if you do not want to use them.</span></span> <span data-ttu-id="d7791-208">As principais razões para fazer isso são:</span><span class="sxs-lookup"><span data-stu-id="d7791-208">The main reasons to do this are:</span></span>

- <span data-ttu-id="d7791-209">Para restringir suas respostas de API da web para um determinado tipo de mídia.</span><span class="sxs-lookup"><span data-stu-id="d7791-209">To restrict your web API responses to a particular media type.</span></span> <span data-ttu-id="d7791-210">Por exemplo, você pode decidir dar suporte a apenas as respostas em JSON e remover o formatador XML.</span><span class="sxs-lookup"><span data-stu-id="d7791-210">For example, you might decide to support only JSON responses, and remove the XML formatter.</span></span>
- <span data-ttu-id="d7791-211">Para substituir o formatador padrão por um formatador personalizado.</span><span class="sxs-lookup"><span data-stu-id="d7791-211">To replace the default formatter with a custom formatter.</span></span> <span data-ttu-id="d7791-212">Por exemplo, você poderia substituir o formatador JSON com sua própria implementação personalizada de um formatador JSON.</span><span class="sxs-lookup"><span data-stu-id="d7791-212">For example, you could replace the JSON formatter with your own custom implementation of a JSON formatter.</span></span>

<span data-ttu-id="d7791-213">O código a seguir mostra como remover os formatadores de padrão.</span><span class="sxs-lookup"><span data-stu-id="d7791-213">The following code shows how to remove the default formatters.</span></span> <span data-ttu-id="d7791-214">Chamá-lo do seu **Application\_iniciar** método, definido no global. asax.</span><span class="sxs-lookup"><span data-stu-id="d7791-214">Call this from your **Application\_Start** method, defined in Global.asax.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a><span data-ttu-id="d7791-215">Tratamento de referências de objeto Circular</span><span class="sxs-lookup"><span data-stu-id="d7791-215">Handling Circular Object References</span></span>

<span data-ttu-id="d7791-216">Por padrão, os formatadores JSON e XML gravam todos os objetos como valores.</span><span class="sxs-lookup"><span data-stu-id="d7791-216">By default, the JSON and XML formatters write all objects as values.</span></span> <span data-ttu-id="d7791-217">Se duas propriedades se referem ao mesmo objeto, ou se o mesmo objeto aparece duas vezes em uma coleção, o formatador de serializar o objeto duas vezes.</span><span class="sxs-lookup"><span data-stu-id="d7791-217">If two properties refer to the same object, or if the same object appears twice in a collection, the formatter will serialize the object twice.</span></span> <span data-ttu-id="d7791-218">Isso é um problema específico se seu gráfico de objeto contém ciclos, pois o serializador lançará uma exceção quando ela detecta um loop no gráfico.</span><span class="sxs-lookup"><span data-stu-id="d7791-218">This is a particular problem if your object graph contains cycles, because the serializer will throw an exception when it detects a loop in the graph.</span></span>

<span data-ttu-id="d7791-219">Considere os seguintes modelos de objeto e o controlador.</span><span class="sxs-lookup"><span data-stu-id="d7791-219">Consider the following object models and controller.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

<span data-ttu-id="d7791-220">Invocar essa ação fará com que o formatador a ser gerada uma exceção, o que resulta em um status código 500 (erro interno do servidor) de resposta ao cliente.</span><span class="sxs-lookup"><span data-stu-id="d7791-220">Invoking this action will cause the formatter to thrown an exception, which translates to a status code 500 (Internal Server Error) response to the client.</span></span>

<span data-ttu-id="d7791-221">Para preservar as referências de objeto em JSON, adicione o seguinte código ao **Application\_iniciar** método no arquivo global. asax:</span><span class="sxs-lookup"><span data-stu-id="d7791-221">To preserve object references in JSON, add the following code to **Application\_Start** method in the Global.asax file:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

<span data-ttu-id="d7791-222">Agora, a ação do controlador retornará JSON tem esta aparência:</span><span class="sxs-lookup"><span data-stu-id="d7791-222">Now the controller action will return JSON that looks like this:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

<span data-ttu-id="d7791-223">Observe que o serializador adiciona uma &quot;$id&quot; propriedade para os dois objetos.</span><span class="sxs-lookup"><span data-stu-id="d7791-223">Notice that the serializer adds an &quot;$id&quot; property to both objects.</span></span> <span data-ttu-id="d7791-224">Além disso, ele detecta que a propriedade Employee.Department cria um loop, portanto, ele substitui o valor com uma referência de objeto: {&quot;$ref&quot;:&quot;1&quot;}.</span><span class="sxs-lookup"><span data-stu-id="d7791-224">Also, it detects that the Employee.Department property creates a loop, so it replaces the value with an object reference: {&quot;$ref&quot;:&quot;1&quot;}.</span></span>

> [!NOTE]
> <span data-ttu-id="d7791-225">Referências de objeto não são padronizadas em JSON.</span><span class="sxs-lookup"><span data-stu-id="d7791-225">Object references are not standard in JSON.</span></span> <span data-ttu-id="d7791-226">Antes de usar esse recurso, considere se os clientes poderão analisar os resultados.</span><span class="sxs-lookup"><span data-stu-id="d7791-226">Before using this feature, consider whether your clients will be able to parse the results.</span></span> <span data-ttu-id="d7791-227">Talvez seja melhor simplesmente remover ciclos do gráfico.</span><span class="sxs-lookup"><span data-stu-id="d7791-227">It might be better simply to remove cycles from the graph.</span></span> <span data-ttu-id="d7791-228">Por exemplo, o link do funcionário para departamento não é realmente necessário neste exemplo.</span><span class="sxs-lookup"><span data-stu-id="d7791-228">For example, the link from Employee back to Department is not really needed in this example.</span></span>


<span data-ttu-id="d7791-229">Para preservar as referências de objeto no XML, você tem duas opções.</span><span class="sxs-lookup"><span data-stu-id="d7791-229">To preserve object references in XML, you have two options.</span></span> <span data-ttu-id="d7791-230">A opção mais simples é adicionar `[DataContract(IsReference=true)]` à sua classe de modelo.</span><span class="sxs-lookup"><span data-stu-id="d7791-230">The simpler option is to add `[DataContract(IsReference=true)]` to your model class.</span></span> <span data-ttu-id="d7791-231">O *IsReference* parâmetro permite que as referências de objeto.</span><span class="sxs-lookup"><span data-stu-id="d7791-231">The *IsReference* parameter enables object references.</span></span> <span data-ttu-id="d7791-232">Lembre-se de que **DataContract** torna serialização opt-in, portanto, você também precisará adicionar **DataMember** atributos para as propriedades:</span><span class="sxs-lookup"><span data-stu-id="d7791-232">Remember that **DataContract** makes serialization opt-in, so you will also need to add **DataMember** attributes to the properties:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

<span data-ttu-id="d7791-233">Agora, o formatador produzirá XML semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="d7791-233">Now the formatter will produce XML similar to following:</span></span>

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

<span data-ttu-id="d7791-234">Se você quiser evitar atributos em sua classe de modelo, há outra opção: Criar um novo tipo específico **DataContractSerializer** da instância e defina *preserveObjectReferences* para **verdadeiro** no construtor.</span><span class="sxs-lookup"><span data-stu-id="d7791-234">If you want to avoid attributes on your model class, there is another option: Create a new type-specific **DataContractSerializer** instance and set *preserveObjectReferences* to **true** in the constructor.</span></span> <span data-ttu-id="d7791-235">Defina essa instância como um serializador por tipo no formatador XML tipo de mídia.</span><span class="sxs-lookup"><span data-stu-id="d7791-235">Then set this instance as a per-type serializer on the XML media-type formatter.</span></span> <span data-ttu-id="d7791-236">O código a seguir mostram como fazer isso:</span><span class="sxs-lookup"><span data-stu-id="d7791-236">The following code show how to do this:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a><span data-ttu-id="d7791-237">Testando a serialização de objeto</span><span class="sxs-lookup"><span data-stu-id="d7791-237">Testing Object Serialization</span></span>

<span data-ttu-id="d7791-238">Ao projetar sua API da web, é útil testar como seus objetos de dados serão serializados.</span><span class="sxs-lookup"><span data-stu-id="d7791-238">As you design your web API, it is useful to test how your data objects will be serialized.</span></span> <span data-ttu-id="d7791-239">Você pode fazer isso sem precisar criar um controlador ou invocar uma ação do controlador.</span><span class="sxs-lookup"><span data-stu-id="d7791-239">You can do this without creating a controller or invoking a controller action.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
