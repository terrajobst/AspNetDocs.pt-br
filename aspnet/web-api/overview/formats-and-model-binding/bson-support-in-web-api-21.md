---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: Suporte a BSON no ASP.NET Web API 2,1-ASP.NET 4. x
author: MikeWasson
description: mostra como usar o BSON em um controlador de API da Web (lado do servidor) e em um aplicativo cliente .NET para o ASP.NET 4. x.
ms.author: riande
ms.date: 01/20/2014
ms.custom: seoapril2019
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: ccbc0372120301b1cd8d4cdc86bd9fba9404d8ae
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622293"
---
# <a name="bson-support-in-aspnet-web-api-21"></a>Suporte do BSON no ASP.NET Web API 2,1

por [Mike Wasson](https://github.com/MikeWasson)

Este tópico mostra como usar o BSON em seu controlador de API da Web (lado do servidor) e em um aplicativo cliente .NET. A API da Web 2,1 apresenta suporte para BSON. 

## <a name="what-is-bson"></a>O que é o BSON?

[BSON](http://bsonspec.org/) é um formato de serialização binária. "BSON" significa "JSON binário", mas BSON e JSON são serializados de forma muito diferente. BSON é "JSON-like", porque os objetos são representados como pares de nome-valor, semelhante ao JSON. Diferentemente do JSON, os tipos de dados numéricos são armazenados como bytes, não cadeias de caracteres

O BSON foi projetado para ser leve, fácil de examinar e rápido de codificar/decodificar.

- BSON é comparável em tamanho para JSON. Dependendo dos dados, uma carga BSON pode ser menor ou maior do que uma carga JSON. Para serializar dados binários, como um arquivo de imagem, BSON é menor que o JSON, pois os dados binários não são codificados em base64.
- Documentos BSON são fáceis de verificar porque os elementos são prefixados com um campo de comprimento, de modo que um analisador pode ignorar elementos sem decodificá-los.
- Codificação e decodificação são eficientes, pois os tipos de dados numéricos são armazenados como números, não cadeias de caracteres.

Clientes nativos, como aplicativos cliente .NET, podem se beneficiar do uso do BSON no lugar de formatos baseados em texto, como JSON ou XML. Para clientes de navegador, você provavelmente desejará usar JSON, porque o JavaScript pode converter diretamente a carga JSON.

Felizmente, a API Web usa a [negociação de conteúdo](content-negotiation.md)para que sua API possa dar suporte a ambos os formatos e permitir que o cliente escolha.

## <a name="enabling-bson-on-the-server"></a>Habilitando BSON no servidor

Na configuração da API Web, adicione o **BsonMediaTypeFormatter** à coleção de formatadores.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

Agora, se o cliente solicitar "Application/BSON", a API da Web usará o formatador BSON.

Para associar o BSON a outros tipos de mídia, adicione-os à coleção SupportedMediaTypes. O código a seguir adiciona "application/vnd. contoso" aos tipos de mídia com suporte:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a>Exemplo de sessão HTTP

Para este exemplo, usaremos a seguinte classe de modelo mais um controlador de API Web simples:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

Um cliente pode enviar a seguinte solicitação HTTP:

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

Aqui está a resposta:

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

Aqui, substituí os dados binários por &quot;.&quot; caracteres. A captura de tela a seguir do Fiddler mostra os valores hexadecimais brutos.

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a>Usando BSON com HttpClient

Aplicativos de clientes .NET podem usar o formatador BSON com **HttpClient**. Para obter mais informações sobre **HttpClient**, consulte [chamando uma API Web de um cliente .net](../advanced/calling-a-web-api-from-a-net-client.md).

O código a seguir envia uma solicitação GET que aceita BSON e, em seguida, desserializa a carga BSON na resposta.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

Para solicitar BSON do servidor, defina o cabeçalho Accept como "Application/BSON":

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

Para desserializar o corpo da resposta, use o **BsonMediaTypeFormatter**. Este formatador não está na coleção de formatadores padrão, portanto, você precisa especificá-lo ao ler o corpo da resposta:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

O exemplo a seguir mostra como enviar uma solicitação POST que contém BSON.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

Grande parte desse código é igual ao exemplo anterior. Mas, no método **IsAsync** , especifique **BsonMediaTypeFormatter** como o formatador:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a>Serializando tipos primitivos de nível superior

Cada documento BSON é uma lista de pares de chave/valor. A especificação BSON não define uma sintaxe para serializar um único valor bruto, como um inteiro ou uma cadeia de caracteres.

Para contornar essa limitação, o **BsonMediaTypeFormatter** trata tipos primitivos como um caso especial. Antes de serializar, ele converte o valor em um par de chave/valor com o "valor" da chave. Por exemplo, suponha que seu controlador de API retorna um inteiro:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

Antes de serializar, o formatador BSON converte isso para o seguinte par de chave/valor:

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

Quando você desserializar, o formatador converte os dados de volta para o valor original. No entanto, os clientes que usam um analisador BSON diferente precisarão lidar com esse caso, se sua API Web retornar valores brutos. Em geral, você deve considerar retornar dados estruturados, em vez de valores brutos.

## <a name="additional-resources"></a>Recursos adicionais

[Exemplo de BSON de API Web](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/BSONSample/)

[Formatadores de mídia](media-formatters.md)
