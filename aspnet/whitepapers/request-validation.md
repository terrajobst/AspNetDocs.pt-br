---
uid: whitepapers/request-validation
title: Validação de solicitação-prevenção de ataques de script | Microsoft Docs
author: rick-anderson
description: Este documento descreve o recurso de validação de solicitação do ASP.NET, em que, por padrão, o aplicativo é impedido de processar o envio de conteúdo HTML não codificado...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 807cccd6fe1acdd6359b014387abd3878840d4cd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78640843"
---
# <a name="request-validation---preventing-script-attacks"></a>Solicitação de validação – Impedir ataques de Script

> Este documento descreve o recurso de validação de solicitação do ASP.NET, em que, por padrão, o aplicativo é impedido de processar o conteúdo HTML não codificado enviado ao servidor. Esse recurso de validação de solicitação pode ser desabilitado quando o aplicativo tiver sido projetado para processar dados HTML com segurança.
> 
> Aplica-se a ASP.NET 1,1 e ASP.NET 2,0.

A validação de solicitação, um recurso do ASP.NET oferecido desde a versão 1.1, impede que o servidor aceite conteúdo sem HTML codificado. Esse recurso foi criado para evitar alguns ataques de injeção de script, em que o código de script ou HTML do cliente pode ser enviado inadvertidamente para um servidor, armazenado e, em seguida, apresentado a outros usuários. Ainda recomendamos que você valide todos os dados de entrada e os codifique com HTML quando for apropriado.

Por exemplo, você cria uma página da Web que solicita o endereço de email de um usuário e, em seguida, armazena esse endereço de email em um banco de dados. Se o usuário inserir &lt;SCRIPT&gt;alerta ("Olá do script")&lt;/SCRIPT&gt; em vez de um endereço de email válido, quando esses dados forem apresentados, esse script poderá ser executado se o conteúdo não tiver sido codificado corretamente. O recurso de validação de solicitação do ASP.NET impede que isso aconteça.

## <a name="why-this-feature-is-useful"></a>Por que esse recurso é útil

Muitos sites não sabem que estão abertos para ataques de injeção de script simples. Se a finalidade desses ataques é Deface o site exibindo HTML ou potencialmente executar script de cliente para redirecionar o usuário para o site de um hacker, os ataques de injeção de script são um problema que os desenvolvedores da Web devem enfrentar.

Ataques de injeção de script são uma preocupação de todos os desenvolvedores da Web, estejam eles usando ASP.NET, ASP ou outras tecnologias de desenvolvimento para a Web.

O recurso de validação de solicitação do ASP.NET impede proativamente esses ataques, não permitindo que o conteúdo HTML não codificado seja processado pelo servidor, a menos que o desenvolvedor decida permitir esse conteúdo.

## <a name="what-to-expect-error-page"></a>O que esperar: página de erro

A captura de tela abaixo mostra alguns exemplos de código ASP.NET:

![](request-validation/_static/image1.png)

A execução desse código resulta em uma página simples que permite que você insira algum texto na TextBox, clique no botão e exiba o texto no controle rótulo:

![](request-validation/_static/image2.png)

No entanto, era o JavaScript, como `<script>alert("hello!")</script>` a ser inserido e enviado, obtemos uma exceção:

![](request-validation/_static/image3.png)

A mensagem de erro informa que um valor "possivelmente perigoso de solicitação. Form foi detectado" e fornece mais detalhes na descrição para exatamente o que ocorreu e como alterar o comportamento. Por exemplo:

A validação da solicitação detectou um valor de entrada de cliente potencialmente perigoso e o processamento da solicitação foi anulado. Esse valor pode indicar uma tentativa de comprometer a segurança do seu aplicativo, como um ataque de script entre sites. Você pode desabilitar a validação de solicitação definindo `validateRequest=false` na diretiva de página ou na seção de configuração. No entanto, é altamente recomendável que seu aplicativo Verifique explicitamente todas as entradas nesse caso.

## <a name="disabling-request-validation-on-a-page"></a>Desabilitando a validação de solicitação em uma página

Para desabilitar a validação de solicitação em uma página, você deve definir o atributo `validateRequest` da diretiva Page como `false`:

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> Quando a validação de solicitação está desabilitada, o conteúdo pode ser enviado para uma página; é responsabilidade do desenvolvedor da página garantir que o conteúdo seja codificado ou processado corretamente.

## <a name="disabling-request-validation-for-your-application"></a>Desabilitando a validação de solicitação para seu aplicativo

Para desabilitar a validação de solicitação para seu aplicativo, você deve modificar ou criar um arquivo Web. config para seu aplicativo e definir o atributo validateRequest da seção `<pages />` como `false`:

[!code-xml[Main](request-validation/samples/sample2.xml)]

Se você quiser desabilitar a validação de solicitação para todos os aplicativos em seu servidor, poderá fazer essa modificação no arquivo Machine. config.

> [!CAUTION]
> Quando a validação de solicitação está desabilitada, o conteúdo pode ser enviado para seu aplicativo; é responsabilidade do desenvolvedor do aplicativo garantir que o conteúdo seja codificado ou processado corretamente.

O código abaixo é modificado para desativar a validação da solicitação:

![](request-validation/_static/image4.png)

Agora, se o seguinte JavaScript foi inserido na caixa de texto `<script>alert("hello!")</script>` o resultado seria:

![](request-validation/_static/image5.png)

Para evitar que isso aconteça, com a validação de solicitação desativada, precisamos codificar o conteúdo em HTML.

## <a name="how-to-html-encode-content"></a>Como codificar conteúdo em HTML

Se você tiver desabilitado a validação de solicitação, é uma boa prática para codificar em HTML o conteúdo que será armazenado para uso futuro. A codificação HTML substituirá automaticamente qualquer '&lt;' ou '&gt;' (junto com vários outros símbolos) com a representação codificada HTML correspondente. Por exemplo, '&lt;' é substituído por '&amp;lt; ' e '&gt;' é substituído por '&amp;gt; '. Os navegadores usam esses códigos especiais para exibir o '&lt;' ou '&gt;' no navegador.

O conteúdo pode ser facilmente codificado em HTML no servidor usando a API `Server.HtmlEncode(string)`. O conteúdo também pode ser facilmente decodificado em HTML, ou seja, revertido para HTML padrão usando o método `Server.HtmlDecode(string)`.

![](request-validation/_static/image6.png)

Resultando em:

![](request-validation/_static/image7.png)
