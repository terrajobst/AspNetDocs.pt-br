---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
title: Combatendo bots (VB) | Microsoft Docs
author: wenz
description: Weblogs automáticos de bots Plaster e outros sites com spam, enviando formulários de comentários sem nenhuma interação do usuário. O controle NoBot no ASP.NET AJAX con...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e9803150-452d-4521-97e3-d75d5599383c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
msc.type: authoredcontent
ms.openlocfilehash: a8ca71b96cb84c97b1a60ae6a3d1a129cd1b0b10
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606377"
---
# <a name="fighting-bots-vb"></a>Bots de combate (VB)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.vb.zip) ou [baixar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0VB.pdf)

> Weblogs automáticos de bots Plaster e outros sites com spam, enviando formulários de comentários sem nenhuma interação do usuário. O controle NoBot no ASP.NET AJAX Control Toolkit pode ajudar a combater esses bots.

## <a name="overview"></a>{1&gt;Visão Geral&lt;1}

Weblogs automáticos de bots Plaster e outros sites com spam, enviando formulários de comentários sem nenhuma interação do usuário. O controle NoBot no ASP.NET AJAX Control Toolkit pode ajudar a combater esses bots.

## <a name="steps"></a>Etapas

Uma abordagem comum para derrotar os bots é usar o teste de Turing público CAPTCHAs completamente automatizado para informar os computadores e os seres humanos. Um teste do Turing era originalmente um teste em que alguém precisava decidir se um parceiro de comunicação é humano ou um computador. Na Web, um CAPTCHA geralmente consiste em uma imagem com algumas letras distorcidas nela. A ideia é que apenas um humano possa ler as letras da imagem, enquanto os algoritmos de OCR falharão.

Há várias vantagens e desvantagens nessa abordagem, mas uma discussão disso está além do escopo deste tutorial. No entanto, há um controle no ASP.NET AJAX Control Toolkit que fornece uma abordagem semelhante: `NoBot`. É mais fácil superar do que uma CAPTCHA, mas é muito fácil de usar e se torna extremamente bem em sites como Blogs, em que é considerado um sucesso se a maioria das tentativas de spam é derrotada, o que o controle de `NoBot` pode fazer.

`NoBot` intercepta o postback do formulário da Web ASP.NET atual se pelo menos uma dessas condições for atendida:

- O navegador não resolve um quebra-cabeça de JavaScript (por exemplo, quando o JavaScript é desativado)
- O usuário enviou o formulário para rápido
- O endereço IP do cliente enviou o formulário com muita frequência em um determinado período de tempo.

Para verificar essas condições, o controle de `NoBot` requer esses atributos (todos eles opcionais):

- `ResponseMinimumDelaySeconds` quantidade mínima de segundos entre postbacks
- `CutoffWindowSeconds` duração do intervalo de tempo no qual os postbacks de um IP são medidas
- `CutoffMaximumInstances` quantidade máxima de segundos por intervalo de tempo

A marcação a seguir exige que pelo menos dois segundos decorram entre postbacks e que haja apenas cinco postbacks ou menos dentro de um intervalo de 30 segundos:

[!code-aspx[Main](fighting-bots-vb/samples/sample1.aspx)]

Em seguida, como sempre, certifique-se de incluir o `ScriptManager` na página para que a biblioteca do ASP.NET AJAX seja carregada e o kit de ferramentas de controle possa ser usado:

[!code-aspx[Main](fighting-bots-vb/samples/sample2.aspx)]

Como a maioria das verificações `NoBot` está ocorrendo no lado do servidor, você precisa verificar o resultado dessas validações. Isso pode ser feito chamando o método `IsValid()` do `NoBot`. Ele tem um argumento (como `out` parâmetro/`ByRef` parâmetro) que é do tipo `NoBotState`. Sua representação de cadeia de caracteres contém o motivo pelo qual a verificação falha e `Valid` de outra forma. O código a seguir gera uma mensagem de acordo com o resultado de `NoBot`:

[!code-aspx[Main](fighting-bots-vb/samples/sample3.aspx)]

Por fim, você precisa de um formulário para enviar e um elemento de rótulo para gerar a mensagem e pronto!

[!code-aspx[Main](fighting-bots-vb/samples/sample4.aspx)]

Quando você executar esse script e desativar o JavaScript ou enviar o formulário dentro dos dois primeiros segundos ou enviar o formulário sete vezes em trinta segundos, receberá uma mensagem de erro. No entanto, use esse controle de forma inteligente, pois apenas cerca de 90-95% dos usuários têm o JavaScript ativado, portanto, 5-10% dos usuários falharão no teste `NoBot`.

[![essa mensagem de erro pode ter sido causada por um bot](fighting-bots-vb/_static/image2.png)](fighting-bots-vb/_static/image1.png)

Essa mensagem de erro pode ter sido causada por um bot ([clique para exibir a imagem em tamanho normal](fighting-bots-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](fighting-bots-cs.md)
