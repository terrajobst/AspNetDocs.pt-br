---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: Usar o AJAX para fornecer atualizações dinâmicas | Microsoft Docs
author: microsoft
description: A etapa 10 implementa o suporte para usuários conectados a seu interesse em participar de um jantar, usando uma abordagem baseada em AJAX integrada nos detalhes do jantar...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 3edc02fec546609505b5e085440fa684abe7acd0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78600845"
---
# <a name="use-ajax-to-deliver-dynamic-updates"></a>Usar o AJAX para fornecer atualizações dinâmicas

pela [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Esta é a etapa 10 de um [tutorial de aplicativo "NerdDinner"](introducing-the-nerddinner-tutorial.md) gratuito que percorre como criar um aplicativo Web pequeno, mas completo usando o ASP.NET MVC 1.
> 
> A etapa 10 implementa o suporte para usuários conectados a seu interesse em participar de um jantar, usando uma abordagem baseada em AJAX integrada na página de detalhes do jantar.
> 
> Se você estiver usando o ASP.NET MVC 3, recomendamos seguir as [introdução com](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) os tutoriais da [loja de música](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) MVC 3 ou MVC.

## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a>Etapa 10 do NerdDinner: habilitação do AJAX para aceitar

Agora, vamos implementar o suporte para usuários conectados a seu interesse em participar de um jantar. Habilitaremos isso usando uma abordagem baseada em AJAX integrada na página de detalhes do jantar.

### <a name="indicating-whether-the-user-is-rsvpd"></a>Indicando se o usuário é o RSVP

Os usuários podem visitar a URL do */Dinners/Details/[ID*] para ver detalhes sobre um jantar específico:

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

O método de ação Details () é implementado da seguinte forma:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

Nossa primeira etapa para implementar o suporte a RSVP será adicionar um método auxiliar "IsUserRegistered (username)" ao nosso objeto de jantar (dentro da classe parcial Dinner.cs que criamos anteriormente). Esse método auxiliar retorna true ou false, dependendo se o usuário é atualmente RSVP para o jantar:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

Em seguida, podemos adicionar o código a seguir ao nosso modelo de exibição details. aspx para exibir uma mensagem apropriada indicando se o usuário está registrado ou não para o evento:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

E agora, quando um usuário visita um jantar, ele está registrado para receber esta mensagem:

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

E quando visitarem um jantar, eles não serão registrados para eles verão a mensagem abaixo:

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a>Implementando o método de ação de registro

Agora, vamos adicionar a funcionalidade necessária para permitir que os usuários façam o RSVP para um jantar da página de detalhes.

Para implementar isso, criaremos uma nova classe "RSVPController" clicando com o botão direito do mouse no diretório \Controllers e escolhendo o comando de menu do controlador Add-&gt;.

Vamos implementar um método de ação "Register" dentro da nova classe RSVPController que usa uma ID para um jantar como um argumento, recupera o objeto de jantar apropriado, verifica se o usuário conectado está atualmente na lista de usuários que se registraram para ele e se Não adiciona um objeto RSVP para eles:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

Observe que, acima de como estamos retornando uma cadeia de caracteres simples como a saída do método de ação. Poderíamos ter inserido essa mensagem em um modelo de exibição – mas, como é tão pequeno, usaremos o método auxiliar Content () na classe base Controller e retornaremos uma mensagem de cadeia de caracteres como acima.

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a>Chamando o método de ação RSVPForEvent usando AJAX

Usaremos o AJAX para invocar o método de ação de registro de nossa exibição de detalhes. Implementar isso é muito fácil. Primeiro, adicionaremos duas referências de biblioteca de script:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

A primeira biblioteca faz referência à biblioteca de scripts do lado do cliente do ASP.NET AJAX básico. Esse arquivo tem aproximadamente 24K de tamanho (compactado) e contém a funcionalidade básica do AJAX do lado do cliente. A segunda biblioteca contém funções utilitárias que se integram aos métodos auxiliares AJAX internos do ASP.NET MVC (que usaremos em breve).

Em seguida, podemos atualizar o código do modelo de exibição que adicionamos anteriormente para que, em vez de gerar uma mensagem "você não está registrado para esse evento", em vez disso, renderizamos um link que, quando enviado por push, executa uma chamada AJAX que invoca nosso método de ação RSVPForEvent em nosso controlador RSVP e RSVPs o usuário:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

O método auxiliar Ajax. ActionLink () usado acima é integrado ao MVC ASP.NET e é semelhante ao método auxiliar HTML. ActionLink (), exceto que, em vez de executar uma navegação padrão, ele faz uma chamada AJAX para o método de ação quando o link é clicado. Acima, estamos chamando o método de ação "Register" no controlador "RSVP" e passando o Jantarid como o parâmetro "ID" para ele. O parâmetro final AjaxOptions que estamos passando indica que queremos pegar o conteúdo retornado do método Action e atualizar o elemento HTML &lt;div&gt; na página cuja ID é "rsvpmsg".

E agora, quando um usuário navega para um jantar para o qual não está registrado, ainda assim ele verá um link para o RSVP para ele:

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

Se eles clicarem no link "RSVP para este evento", eles farão uma chamada AJAX para o método de ação registrar no controlador RSVP e, quando ele for concluído, verão uma mensagem atualizada, como a seguir:

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

A largura de banda de rede e o tráfego envolvidos ao fazer essa chamada AJAX é realmente leve. Quando o usuário clica no link "RSVP para este evento", uma pequena solicitação de rede HTTP POST é feita à URL */Dinners/Register/1* que se parece com a seguinte na conexão:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

E a resposta do nosso método de ação de registro é simplesmente:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

Essa chamada leve é rápida e funcionará mesmo em uma rede lenta.

### <a name="adding-a-jquery-animation"></a>Adicionando uma animação do jQuery

A funcionalidade do AJAX que implementamos funciona bem e rápida. Às vezes, pode acontecer tão rápido que um usuário talvez não perceba que o link RSVP foi substituído por um novo texto. Para tornar o resultado um pouco mais óbvio, podemos adicionar uma animação simples para chamar atenção para a mensagem de atualização.

O modelo de projeto ASP.NET MVC padrão inclui jQuery – uma biblioteca JavaScript de software livre excelente (e muito popular) que também é suportada pela Microsoft. o jQuery fornece vários recursos, incluindo uma boa biblioteca de efeitos e seleção de DOM HTML.

Para usar o jQuery, primeiro adicionaremos uma referência de script a ele. Como vamos usar o jQuery dentro de uma variedade de locais em nosso site, adicionaremos a referência de script em nosso site. Master Master Page file para que todas as páginas possam usá-lo.

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

*Dica: Verifique se você instalou o hotfix do JavaScript IntelliSense para VS 2008 SP1 que permite suporte mais avançado ao IntelliSense para arquivos JavaScript (incluindo jQuery). Você pode baixá-lo em: http://tinyurl.com/vs2008javascripthotfix*

O código escrito usando o JQuery geralmente usa um método JavaScript "$ ()" global que recupera um ou mais elementos HTML usando um seletor de CSS. Por exemplo, *$ ("#rsvpmsg")* seleciona qualquer elemento HTML com a ID de rsvpmsg, enquanto *$ (". algo")* selecionaria todos os elementos com o nome de classe CSS "algo". Você também pode escrever consultas mais avançadas como "retornar todos os botões de opção marcados" usando uma consulta de seletor como: *$ ("Input [@type= Radio] [@checked]")* .

Depois de selecionar elementos, você pode chamar métodos para executar ações, como ocultá-los: *$ ("#rsvpmsg"). Hide ();*

Para nosso cenário de RSVP, definiremos uma função JavaScript simples denominada "AnimateRSVPMessage", que seleciona o &lt;&gt; div "rsvpmsg" e anima o tamanho de seu conteúdo de texto. O código abaixo inicia o texto pequeno e faz com que ele aumente em um período de 400 milissegundos:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

Em seguida, podemos conectar essa função JavaScript a ser chamada depois que nossa chamada AJAX for concluída com êxito passando seu nome para nosso método auxiliar Ajax. ActionLink () (por meio da propriedade de evento AjaxOptions "OnSuccess"):

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

E agora, quando o link "RSVP para este evento" for clicado e nossa chamada AJAX for concluída com êxito, a mensagem de conteúdo enviada será animada e crescerá grande:

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

Além de fornecer um evento "OnSuccess", o objeto AjaxOptions expõe eventos OnBegin, OnFailure e OnComplete que você pode manipular (juntamente com uma variedade de outras propriedades e opções úteis).

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a>Limpeza-refatorar uma exibição parcial de RSVP

Nosso modelo de exibição de detalhes está começando a ter um pouco de tempo, que a hora extra irá torná-lo um pouco mais difícil de entender. Para ajudar a melhorar a legibilidade do código, vamos concluir criando uma exibição parcial – RSVPStatus. ascx – que encapsula todo o código de exibição de RSVP para nossa página de detalhes.

Podemos fazer isso clicando com o botão direito do mouse na pasta \Views\Dinners e escolhendo o comando de menu Add-&gt;View. Vamos pegar um objeto de jantar como seu ViewModel fortemente tipado. Em seguida, podemos copiar/colar o conteúdo RSVP de nossa exibição details. aspx nele.

Depois de fazer isso, vamos criar também outra exibição parcial – EditAndDeleteLinks. ascx, que encapsula nosso código de exibição de link de edição e exclusão. Também vamos pegar um objeto de jantar como seu ViewModel fortemente tipado e copiar/colar a lógica de edição e exclusão de nossa exibição details. aspx para ela.

Nosso modelo de exibição de detalhes pode, então, incluir apenas duas chamadas de método html. RenderPartial () na parte inferior:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

Isso torna o limpador de código ler e manter.

### <a name="next-step"></a>Próxima etapa

Agora, vamos examinar como podemos usar o AJAX ainda mais e adicionar suporte de mapeamento interativo ao nosso aplicativo.

> [!div class="step-by-step"]
> [Anterior](secure-applications-using-authentication-and-authorization.md)
> [Próximo](use-ajax-to-implement-mapping-scenarios.md)
