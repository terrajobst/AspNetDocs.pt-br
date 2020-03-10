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
# <a name="media-formatters-in-aspnet-web-api-2"></a>Formatadores de mídia no ASP.NET Web API 2

por [Mike Wasson](https://github.com/MikeWasson)

Este tutorial mostra como dar suporte a formatos de mídia adicionais no ASP.NET Web API.

## <a name="internet-media-types"></a>Tipos de mídia da Internet

Um tipo de mídia, também chamado de tipo MIME, identifica o formato de um dado. No HTTP, os tipos de mídia descrevem o formato do corpo da mensagem. Um tipo de mídia consiste em duas cadeias de caracteres, um tipo e um subtipo. Por exemplo:

- texto/html
- image/png
- aplicativo/json

Quando uma mensagem HTTP contém um corpo de entidade, o cabeçalho Content-Type Especifica o formato do corpo da mensagem. Isso informa ao receptor como analisar o conteúdo do corpo da mensagem.

Por exemplo, se uma resposta HTTP contiver uma imagem PNG, a resposta poderá ter os seguintes cabeçalhos.

[!code-console[Main](media-formatters/samples/sample1.cmd)]

Quando o cliente envia uma mensagem de solicitação, ele pode incluir um cabeçalho Accept. O cabeçalho Accept informa ao servidor qual (is) tipo (s) de mídia o cliente deseja do servidor. Por exemplo:

[!code-console[Main](media-formatters/samples/sample2.cmd)]

Esse cabeçalho informa ao servidor que o cliente deseja HTML, XHTML ou XML.

O tipo de mídia determina como a API da Web serializa e desserializa o corpo da mensagem HTTP. A API Web tem suporte interno para dados XML, JSON, BSON e de formulário UrlEncode, e você pode dar suporte a tipos de mídia adicionais escrevendo um *formatador de mídia*.

Para criar um formatador de mídia, derive de uma destas classes:

- [MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx). Essa classe usa métodos assíncronos de leitura e gravação.
- [BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx). Essa classe deriva de **MediaTypeFormatter** , mas usa métodos síncronos de leitura/gravação.

Derivar de **BufferedMediaTypeFormatter** é mais simples, porque não há código assíncrono, mas também significa que o thread de chamada pode ser bloqueado durante e/s.

## <a name="example-creating-a-csv-media-formatter"></a>Exemplo: Criando um formatador de mídia CSV

O exemplo a seguir mostra um formatador de tipo de mídia que pode serializar um objeto Product em um formato CSV (valores separados por vírgula). Este exemplo usa o tipo de produto definido no tutorial [criando uma API Web que oferece suporte a operações CRUD](../older-versions/creating-a-web-api-that-supports-crud-operations.md). Aqui está a definição do objeto Product:

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

Para implementar um formatador CSV, defina uma classe derivada de **BufferedMediaTypeFormatter**:

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

No construtor, adicione os tipos de mídia aos quais o formatador dá suporte. Neste exemplo, o formatador dá suporte a um único tipo de mídia, &quot;texto/CSV&quot;:

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

Substitua o método **Canwritetype** para indicar quais tipos o formatador pode serializar:

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

Neste exemplo, o formatador pode serializar objetos de `Product` único, bem como coleções de objetos `Product`.

Da mesma forma, substitua o método **Canreadtype** para indicar quais tipos o formatador pode desserializar. Neste exemplo, o formatador não oferece suporte à desserialização, portanto, o método simplesmente retorna **false**.

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

Por fim, substitua o método **WriteToStream** . Esse método serializa um tipo gravando-o em um fluxo. Se o formatador der suporte à desserialização, substitua também o método **ReadFromStream** .

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a>Adicionando um formatador de mídia ao pipeline da API Web

Para adicionar um formatador de tipo de mídia ao pipeline da API Web, use a propriedade **Formatters** no objeto **HttpConfiguration** .

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a>Codificações de caracteres

Opcionalmente, um formatador de mídia pode dar suporte a várias codificações de caracteres, como UTF-8 ou ISO 8859-1.

No construtor, adicione um ou mais tipos [System. Text. Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) à coleção **SupportedEncodings** . Coloque a codificação padrão primeiro.

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

Nos métodos **WriteToStream** e **ReadFromStream** , chame [MediaTypeFormatter. SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) para selecionar a codificação de caractere preferencial. Esse método corresponde aos cabeçalhos de solicitação em relação à lista de codificações com suporte. Use a **codificação** retornada ao ler ou gravar a partir do fluxo:

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
