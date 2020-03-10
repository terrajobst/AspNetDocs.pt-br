---
uid: web-api/overview/formats-and-model-binding/content-negotiation
title: Negociação de conteúdo em ASP.NET Web API-ASP.NET 4. x
author: MikeWasson
description: Descreve como ASP.NET Web API implementa a negociação de conteúdo HTTP para ASP.NET 4. x.
ms.author: riande
ms.date: 05/20/2012
ms.custom: seoapril2019
ms.assetid: 0dd51b30-bf5a-419f-a1b7-2817ccca3c7d
msc.legacyurl: /web-api/overview/formats-and-model-binding/content-negotiation
msc.type: authoredcontent
ms.openlocfilehash: cb6668ff6de276d3778ce11f27ce597d8bf1f9c7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622265"
---
# <a name="content-negotiation-in-aspnet-web-api"></a>Negociação de conteúdo no ASP.NET Web API

por [Mike Wasson](https://github.com/MikeWasson)

Este artigo descreve como ASP.NET Web API implementa a negociação de conteúdo para ASP.NET 4. x.

A especificação HTTP (RFC 2616) define a negociação de conteúdo como "o processo de selecionar a melhor representação para uma determinada resposta quando há várias representações disponíveis". O mecanismo primário para a negociação de conteúdo em HTTP são estes cabeçalhos de solicitação:

- **Aceitar:** Quais tipos de mídia são aceitáveis para a resposta, como "Application/JSON", "application/xml", ou um tipo de mídia personalizado, como &quot;application/vnd. example + XML&quot;
- **Accept-charset:** Quais conjuntos de caracteres são aceitáveis, como UTF-8 ou ISO 8859-1.
- **Aceitação-codificação:** Quais codificações de conteúdo são aceitáveis, como gzip.
- **Accept-Language:** A linguagem natural preferida, como "en-US".

O servidor também pode examinar outras partes da solicitação HTTP. Por exemplo, se a solicitação contiver um cabeçalho X solicitado-com, indicando uma solicitação AJAX, o servidor poderá padronizar para JSON se não houver cabeçalho Accept.

Neste artigo, veremos como a API da Web usa os cabeçalhos Accept e Accept-charset. (Neste momento, não há suporte interno para Accept-Encoding ou Accept-Language.)

## <a name="serialization"></a>Serialização

Se um controlador da API Web retornar um recurso como tipo CLR, o pipeline serializará o valor de retorno e o gravará no corpo da resposta HTTP.

Por exemplo, considere a seguinte ação do controlador:

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

Um cliente pode enviar esta solicitação HTTP:

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

Em resposta, o servidor pode enviar:

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

Neste exemplo, o cliente solicitou JSON, JavaScript ou "qualquer coisa" (\*/\*). O servidor respondeu com uma representação JSON do objeto `Product`. Observe que o cabeçalho Content-Type na resposta é definido como &quot;Application/JSON&quot;.

Um controlador também pode retornar um objeto **HttpResponseMessage** . Para especificar um objeto CLR para o corpo da resposta, chame o método de extensão **CreateResponse** :

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

Essa opção oferece mais controle sobre os detalhes da resposta. Você pode definir o código de status, adicionar cabeçalhos HTTP e assim por diante.

O objeto que serializa o recurso é chamado de *formatador de mídia*. Os formatadores de mídia derivam da classe **MediaTypeFormatter** . A API Web fornece formatadores de mídia para XML e JSON, e você pode criar formatadores personalizados para dar suporte a outros tipos de mídia. Para obter informações sobre como escrever um formatador personalizado, consulte [formatadores de mídia](media-formatters.md).

## <a name="how-content-negotiation-works"></a>Como a negociação de conteúdo funciona

Primeiro, o pipeline Obtém o serviço **IContentNegotiator** do objeto **HttpConfiguration** . Ele também obtém a lista de formatadores de mídia da coleção **HttpConfiguration. Formatters** .

Em seguida, o pipeline chama **IContentNegotiator. Negotiate**, passando em:

- O tipo de objeto a ser serializado
- A coleção de formatadores de mídia
- A solicitação HTTP

O método **Negotiate** retorna duas partes de informação:

- Qual formatador usar
- O tipo de mídia para a resposta

Se nenhum formatador for encontrado, o método **Negotiate** retornará **NULL**e o cliente receberá o erro http 406 (não aceitável).

O código a seguir mostra como um controlador pode invocar diretamente a negociação de conteúdo:

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

Esse código é equivalente ao que o pipeline faz automaticamente.

## <a name="default-content-negotiator"></a>Negociador de conteúdo padrão

A classe **DefaultContentNegotiator** fornece a implementação padrão de **IContentNegotiator**. Ele usa vários critérios para selecionar um formatador.

Primeiro, o formatador deve ser capaz de serializar o tipo. Isso é verificado chamando **MediaTypeFormatter. Canwritetype**.

Em seguida, o conteúdo negociador examina cada formatador e avalia o quão bem ele corresponde à solicitação HTTP. Para avaliar a correspondência, o conteúdo negociador examina duas coisas no formatador:

- A coleção **SupportedMediaTypes** , que contém uma lista de tipos de mídia com suporte. O conteúdo negociador tenta corresponder a essa lista com o cabeçalho Accept de solicitação. Observe que o cabeçalho Accept pode incluir intervalos. Por exemplo, "texto/simples" é uma correspondência para texto/\* ou \*/\*.
- A coleção **MediaTypeMappings** , que contém uma lista de objetos **MediaTypeMapping** . A classe **MediaTypeMapping** fornece uma maneira genérica de fazer a correspondência de solicitações HTTP com tipos de mídia. Por exemplo, ele poderia mapear um cabeçalho HTTP personalizado para um tipo de mídia específico.

Se houver várias correspondências, a correspondência com o fator de qualidade mais alto vence. Por exemplo:

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

Neste exemplo, Application/JSON tem um fator de qualidade implícito de 1,0, portanto, é preferível em Application/XML.

Se nenhuma correspondência for encontrada, o conteúdo negociador tentará corresponder ao tipo de mídia do corpo da solicitação, se houver. Por exemplo, se a solicitação contiver dados JSON, o conteúdo negociador procurará um formatador JSON.

Se ainda não houver nenhuma correspondência, o conteúdo negociador simplesmente escolhe o primeiro formatador que pode serializar o tipo.

## <a name="selecting-a-character-encoding"></a>Selecionando uma codificação de caracteres

Depois que um formatador é selecionado, o conteúdo negociador escolhe a melhor codificação de caractere examinando a propriedade **SupportedEncodings** no formatador e correspondendo-o com o cabeçalho Accept-charset na solicitação (se houver).
