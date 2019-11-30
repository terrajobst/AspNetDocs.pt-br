---
uid: mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
title: Prevenção de ataques de injeção de JavaScript (VB) | Microsoft Docs
author: StephenWalther
description: Impeça que ataques de injeção de JavaScript e ataques de script entre sites aconteçam a você. Neste tutorial, Stephen Walther explica como você pode facilmente de...
ms.author: riande
ms.date: 08/19/2008
ms.assetid: 9274a72e-34dd-4dae-8452-ed733ae71377
msc.legacyurl: /mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
msc.type: authoredcontent
ms.openlocfilehash: dfe09085f26c62c566649bc6f570aa25367a0f07
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74594800"
---
# <a name="preventing-javascript-injection-attacks-vb"></a>Impedir ataques de injeção de JavaScript (VB)

por [Stephen Walther](https://github.com/StephenWalther)

[Baixar PDF](https://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_06_VB.pdf)

> Impeça que ataques de injeção de JavaScript e ataques de script entre sites aconteçam a você. Neste tutorial, Stephen Walther explica como você pode facilmente derrotar esses tipos de ataques por codificação HTML de seu conteúdo.

O objetivo deste tutorial é explicar como você pode impedir ataques de injeção de JavaScript em seus aplicativos MVC do ASP.NET. Este tutorial aborda duas abordagens para defender seu site contra um ataque de injeção de JavaScript. Você aprende a evitar ataques de injeção de JavaScript codificando os dados exibidos. Você também aprende como evitar ataques de injeção de JavaScript codificando os dados que aceita.

## <a name="what-is-a-javascript-injection-attack"></a>O que é um ataque de injeção de JavaScript?

Sempre que você aceita a entrada do usuário e reexibe a entrada do usuário, você abre seu site para ataques de injeção de JavaScript. Vamos examinar um aplicativo concreto que está aberto para ataques de injeção de JavaScript.

Imagine que você criou um site de comentários do cliente (veja a Figura 1). Os clientes podem visitar o site e inserir comentários sobre sua experiência usando seus produtos. Quando um cliente envia seus comentários, os comentários são exibidos novamente na página de comentários.

[![site de comentários do cliente](preventing-javascript-injection-attacks-vb/_static/image2.png)](preventing-javascript-injection-attacks-vb/_static/image1.png)

**Figura 01**: site de comentários do cliente ([clique para exibir a imagem em tamanho normal](preventing-javascript-injection-attacks-vb/_static/image3.png))

O site de comentários do cliente usa o `controller` na Listagem 1. Essa `controller` contém duas ações chamadas `Index()` e `Create()`.

**Listagem 1 – `HomeController.vb`**

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample1.vb)]

O método `Index()` exibe a exibição `Index`. Esse método passa todos os comentários do cliente anterior para a exibição de `Index` recuperando os comentários do banco de dados (usando uma consulta LINQ to SQL).

O método `Create()` cria um novo item de comentário e o adiciona ao banco de dados. A mensagem que o cliente insere no formulário é passada para o método `Create()` no parâmetro Message. Um item de comentário é criado e a mensagem é atribuída à propriedade `Message` do item de comentário. O item de comentário é enviado ao banco de dados com a chamada do método `DataContext.SubmitChanges()`. Por fim, o visitante é Redirecionado de volta para a `Index` exibição onde todos os comentários são exibidos.

A exibição `Index` está contida na Listagem 2.

**Listagem 2 – `Index.aspx`**

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample2.aspx)]

O modo de exibição de `Index` tem duas seções. A seção superior contém o formulário de comentários do cliente real. A seção inferior contém uma para.. Cada loop que percorre todos os itens de comentários do cliente anteriores e exibe as propriedades de EntryDate e mensagem para cada item de comentário.

O site de comentários do cliente é um site simples. Infelizmente, o site está aberto para ataques de injeção de JavaScript.

Imagine que você insira o seguinte texto no formulário de comentários do cliente:

[!code-html[Main](preventing-javascript-injection-attacks-vb/samples/sample3.html)]

Esse texto representa um script JavaScript que exibe uma caixa de mensagem de alerta. Depois que alguém enviar esse script para o formulário de comentários, a mensagem <em>Boo!</em> aparecerá sempre que alguém visitar o site de comentários do cliente no futuro (veja a Figura 2).

[Injeção de JavaScript ![](preventing-javascript-injection-attacks-vb/_static/image5.png)](preventing-javascript-injection-attacks-vb/_static/image4.png)

**Figura 02**: injeção de JavaScript ([clique para exibir a imagem em tamanho normal](preventing-javascript-injection-attacks-vb/_static/image6.png))

Agora, sua resposta inicial aos ataques de injeção de JavaScript pode ser apatia. Você pode imaginar que os ataques de injeção de JavaScript são simplesmente um tipo de ataque de *desfiguração* . Você pode acreditar que ninguém pode fazer algo realmente ruim confirmando um ataque de injeção de JavaScript.

Infelizmente, um hacker pode fazer alguma coisa realmente ruim, injetando JavaScript em um site. Você pode usar um ataque de injeção de JavaScript para executar um ataque XSS (script entre sites). Em um ataque de script entre sites, você rouba informações confidenciais do usuário e envia as informações para outro site.

Por exemplo, um hacker pode usar um ataque de injeção de JavaScript para roubar os valores de cookies do navegador de outros usuários. Se informações confidenciais, como senhas, números de cartão de crédito ou números de segurança social, forem armazenadas nos cookies do navegador, um hacker poderá usar um ataque de injeção de JavaScript para roubar essas informações. Ou, se um usuário inserir informações confidenciais em um campo de formulário contido em uma página comprometida com um ataque de JavaScript, o hacker poderá usar o JavaScript injetado para obter os dados do formulário e enviá-los para outro site.

*Seja medo*. Tome os ataques de injeção de JavaScript seriamente e proteja as informações confidenciais do usuário. Nas próximas duas seções, discutiremos duas técnicas que você pode usar para defender seus aplicativos MVC ASP.NET de ataques de injeção de JavaScript.

## <a name="approach-1-html-encode-in-the-view"></a>Abordagem #1: codificação HTML na exibição

Um método fácil de impedir ataques de injeção de JavaScript é codificar em HTML os dados inseridos pelos usuários do site quando você reexibe os dados em uma exibição. A exibição `Index` atualizada na Listagem 3 segue essa abordagem.

**Listagem 3 – `Index.aspx` (HTML codificado)**

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample4.aspx)]

Observe que o valor de `feedback.Message` é codificado em HTML antes de o valor ser exibido com o seguinte código:

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample5.aspx)]

O que significa codificar uma cadeia de caracteres em HTML? Quando você codifica em HTML uma cadeia de caracteres, caracteres perigosos, como `<` e `>`, são substituídos por referências de entidade HTML, como `&lt;` e `&gt;`. Assim, quando a cadeia de caracteres `<script>alert("Boo!")</script>` é codificada em HTML, ela é convertida em `&lt;script&gt;alert(&quot;Boo!&quot;)&lt;/script&gt;`. A cadeia de caracteres codificada não é mais executada como um script JavaScript quando interpretada por um navegador. Em vez disso, você obtém a página inofensiva na Figura 3.

[![ataque de JavaScript em derrota](preventing-javascript-injection-attacks-vb/_static/image8.png)](preventing-javascript-injection-attacks-vb/_static/image7.png)

**Figura 03**: ataque de JavaScript com derrotas ([clique para exibir a imagem em tamanho normal](preventing-javascript-injection-attacks-vb/_static/image9.png))

Observe que na exibição de `Index` na Listagem 3, somente o valor de `feedback.Message` é codificado. O valor de `feedback.EntryDate` não é codificado. Você só precisa codificar dados inseridos por um usuário. Como o valor de EntryDate foi gerado no controlador, você não precisa codificar esse valor em HTML.

## <a name="approach-2-html-encode-in-the-controller"></a>Abordagem #2: codificação HTML no controlador

Em vez de dados de codificação HTML quando você exibe os dados em uma exibição, você pode codificar os dados apenas antes de enviar os dados para o banco de dado. Essa segunda abordagem é feita no caso do `controller` na Listagem 4.

**Listagem 4 – `HomeController.cs` (HTML codificado)**

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample6.vb)]

Observe que o valor da mensagem é codificado em HTML antes que o valor seja enviado ao banco de dados dentro da ação de `Create()`. Quando a mensagem é exibida novamente na exibição, a mensagem é codificada em HTML e qualquer JavaScript injetado na mensagem não é executado.

Normalmente, você deve favorecer a primeira abordagem abordada neste tutorial sobre essa segunda abordagem. O problema dessa segunda abordagem é que você acaba com os dados codificados em HTML em seu banco de dados. Em outras palavras, os dados do banco de dados são sujosdos com caracteres de aparências engraçados.

Por que isso é insatisfatório? Se você precisar exibir os dados do banco de dados em algo diferente de uma página da Web, terá problemas. Por exemplo, você não pode mais exibir facilmente os dados em um aplicativo Windows Forms.

## <a name="summary"></a>Resumo

A finalidade deste tutorial era assustam-lo sobre o potencial de um ataque de injeção de JavaScript. Este tutorial abordou duas abordagens para proteger seus aplicativos ASP.NET MVC contra ataques de injeção de JavaScript: você pode codificar dados enviados pelo usuário na exibição ou pode codificar dados enviados pelo usuário no controlador.

> [!div class="step-by-step"]
> [Anterior](authenticating-users-with-windows-authentication-vb.md)
