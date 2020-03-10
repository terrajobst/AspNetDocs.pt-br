---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: 'Enviando dados de formulário HTML em ASP.NET Web API: formulário-urlencoded data-ASP.NET 4. x'
author: MikeWasson
description: Este artigo mostra como postar dados de forma urlencoded em um controlador de API da Web com ASP.NET 4. x
ms.author: riande
ms.date: 06/15/2012
ms.custom: seoapril2019
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 7243069dbd8051b1374ed6e0112c273b8fe26f61
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557599"
---
# <a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a>Enviando dados de formulário HTML em ASP.NET Web API: formulário-dados urlencoded

por [Mike Wasson](https://github.com/MikeWasson)

## <a name="part-1-form-urlencoded-data"></a>Parte 1: formulário – dados urlencoded

Este artigo mostra como postar dados de formulário urlencoded em um controlador de API da Web.

- [Visão geral de formulários HTML](#overview_of_html_forms)
- [Enviando tipos complexos](#sending_complex_types)
- [Enviando dados de formulário via AJAX](#sending_form_data_via_ajax)
- [Enviando tipos simples](#sending_simple_types)

> [!NOTE]
> [Baixe o projeto concluído](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).

<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a>Visão geral de formulários HTML

Os formulários HTML usam GET ou POST para enviar dados ao servidor. O atributo **Method** do elemento **Form** fornece o método http:

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

O método padrão é GET. Se o formulário usar GET, os dados do formulário serão codificados no URI como uma cadeia de caracteres de consulta. Se o formulário usar POST, os dados do formulário serão colocados no corpo da solicitação. Para dados postados, o atributo **Enctype** especifica o formato do corpo da solicitação:

| Enctype | DESCRIÇÃO |
| --- | --- |
| Application/x-www-form-urlencoded | Os dados de formulário são codificados como pares de nome/valor, semelhante a uma cadeia de caracteres de consulta de URI. Este é o formato padrão para POST. |
| multipart/form-data | Os dados de formulário são codificados como uma mensagem MIME de várias partes. Use esse formato se você estiver carregando um arquivo para o servidor. |

A parte 1 deste artigo analisa o formato x-www-form-urlencoded. A [parte 2](sending-html-form-data-part-2.md) descreve o MIME de várias partes.

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a>Enviando tipos complexos

Normalmente, você enviará um tipo complexo, composto por valores extraídos de vários controles de formulário. Considere o seguinte modelo que representa uma atualização de status:

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

Aqui está um controlador de API da Web que aceita um objeto `Update` por meio de POST.

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> Esse controlador usa o [roteamento baseado em ação](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), portanto, o modelo de rota é &quot;API/{Controller}/{Action}/{id}&quot;. O cliente irá postar os dados para &quot;/API/updates/Complex&quot;.

Agora, vamos escrever um formulário HTML para que os usuários enviem uma atualização de status.

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

Observe que o atributo **Action** no formulário é o URI da nossa ação do controlador. Aqui está o formulário com alguns valores inseridos em:

![](sending-html-form-data-part-1/_static/image1.png)

Quando o usuário clica em enviar, o navegador envia uma solicitação HTTP semelhante à seguinte:

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

Observe que o corpo da solicitação contém os dados do formulário, formatados como pares de nome/valor. A API da Web converte automaticamente os pares de nome/valor em uma instância da classe `Update`.

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a>Enviando dados de formulário via AJAX

Quando um usuário envia um formulário, o navegador navega para fora da página atual e renderiza o corpo da mensagem de resposta. Tudo bem quando a resposta é uma página HTML. Com uma API Web, no entanto, o corpo da resposta geralmente está vazio ou contém dados estruturados, como JSON. Nesse caso, faz mais sentido enviar os dados do formulário usando uma solicitação AJAX, para que a página possa processar a resposta.

O código a seguir mostra como postar dados de formulário usando o jQuery.

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

A função de **envio** do jQuery substitui a ação de formulário por uma nova função. Isso substitui o comportamento padrão do botão enviar. A função **Serialize** serializa os dados do formulário em pares de nome/valor. Para enviar os dados do formulário para o servidor, chame `$.post()`.

Quando a solicitação for concluída, o manipulador `.success()` ou `.error()` exibirá uma mensagem apropriada para o usuário.

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a>Enviando tipos simples

Nas seções anteriores, enviamos um tipo complexo, que API da Web é desserializada para uma instância de uma classe de modelo. Você também pode enviar tipos simples, como uma cadeia de caracteres.

> [!NOTE]
> Antes de enviar um tipo simples, considere encapsular o valor em um tipo complexo. Isso oferece os benefícios da validação de modelo no lado do servidor e torna mais fácil estender seu modelo, se necessário.

As etapas básicas para enviar um tipo simples são as mesmas, mas há duas diferenças sutis. Primeiro, no controlador, você deve decorar o nome do parâmetro com o atributo **FromBody** .

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

Por padrão, a API Web tenta obter tipos simples do URI de solicitação. O atributo **FromBody** informa à API da Web para ler o valor do corpo da solicitação.

> [!NOTE]
> A API Web lê o corpo da resposta no máximo uma vez, de modo que apenas um parâmetro de uma ação pode vir do corpo da solicitação. Se você precisar obter vários valores do corpo da solicitação, defina um tipo complexo.

Em segundo lugar, o cliente precisa enviar o valor com o seguinte formato:

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

Especificamente, a parte do nome do par nome/valor deve estar vazia para um tipo simples. Nem todos os navegadores dão suporte a isso para formulários HTML, mas você cria esse formato em script da seguinte maneira:

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

Veja um exemplo de formulário:

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

E aqui está o script para enviar o valor do formulário. A única diferença do script anterior é o argumento passado para a função **post** .

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

Você pode usar a mesma abordagem para enviar uma matriz de tipos simples:

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a>Recursos adicionais

[Parte 2: carregamento de arquivo e MIME de várias partes](sending-html-form-data-part-2.md)
