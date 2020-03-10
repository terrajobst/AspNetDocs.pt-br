---
uid: mvc/overview/security/preventing-open-redirection-attacks
title: Impedindo ataques de redirecionamentoC#aberto () | Microsoft Docs
author: jongalloway
description: Este tutorial explica como você pode impedir ataques de redirecionamento abertos em seus aplicativos MVC do ASP.NET. Este tutorial discute as alterações que foram feitas...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 69fb02e0-f5b7-4c35-878c-fa87164fc785
msc.legacyurl: /mvc/overview/security/preventing-open-redirection-attacks
msc.type: authoredcontent
ms.openlocfilehash: cfa635d4fd14d031993c5b452325cbe334f82dc2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78538167"
---
# <a name="preventing-open-redirection-attacks-c"></a>Impedir ataques de redirecionamento aberto (C#)

por [Jon Galloway](https://github.com/jongalloway)

> Este tutorial explica como você pode impedir ataques de redirecionamento abertos em seus aplicativos MVC do ASP.NET. Este tutorial discute as alterações feitas no AccountController no ASP.NET MVC 3 e demonstra como você pode aplicar essas alterações em seus aplicativos ASP.NET MVC 1,0 e 2 existentes.

## <a name="what-is-an-open-redirection-attack"></a>O que é um ataque de redirecionamento aberto?

Qualquer aplicativo Web que redireciona para uma URL especificada por meio da solicitação, como a QueryString ou os dados de formulário, pode ser adulterado para redirecionar os usuários para uma URL externa e mal-intencionada. Essa violação é chamada de ataque de redirecionamento aberto.

Sempre que a lógica do aplicativo redireciona para uma URL especificada, você deve verificar se a URL de redirecionamento não foi violada. O logon usado no AccountController padrão para o ASP.NET MVC 1,0 e o ASP.NET MVC 2 é vulnerável a ataques de redirecionamento abertos. Felizmente, é fácil atualizar seus aplicativos existentes para usar as correções da visualização do ASP.NET MVC 3.

Para entender a vulnerabilidade, vejamos como o redirecionamento de logon funciona em um projeto de aplicativo Web do ASP.NET MVC 2 padrão. Neste aplicativo, a tentativa de visitar uma ação do controlador que tenha o atributo [autorizar] redirecionará usuários não autorizados para a exibição/Account/LogOn. Esse redirecionamento para/Account/LogOn incluirá um parâmetro returnUrl QueryString para que o usuário possa ser retornado para a URL solicitada originalmente depois que tiver feito logon com êxito.

Na captura de tela abaixo, podemos ver que uma tentativa de acessar a exibição/Account/ChangePassword quando não fez logon resulta em um redirecionamento para/Account/LogOn? ReturnUrl =% 2fAccount% 2fChangePassword% 2F.

[![](preventing-open-redirection-attacks/_static/image2.png)](preventing-open-redirection-attacks/_static/image1.png)

**Figura 01**: página de logon com um redirecionamento aberto

Como o parâmetro ReturnUrl QueryString não é validado, um invasor pode modificá-lo para injetar qualquer endereço URL no parâmetro para conduzir um ataque de redirecionamento aberto. Para demonstrar isso, podemos modificar o parâmetro ReturnUrl para [http://bing.com](http://bing.com), de modo que a URL de logon resultante será/Account/logon? ReturnUrl =<http://www.bing.com/>. Após fazer logon com êxito no site, somos redirecionados para [http://bing.com](http://bing.com). Uma vez que esse redirecionamento não é validado, ele pode apontar para um site mal-intencionado que tenta enganar o usuário.

### <a name="a-more-complex-open-redirection-attack"></a>Um ataque de redirecionamento aberto mais complexo

Os ataques de redirecionamento abertos são especialmente perigosos porque um invasor sabe que estamos tentando fazer logon em um site específico, o que nos torna vulnerável a um [ataque de phishing](https://www.microsoft.com/protect/fraud/phishing/symptoms.aspx). Por exemplo, um invasor pode enviar emails mal-intencionados aos usuários do site em uma tentativa de capturar suas senhas. Vejamos como isso funcionaria no site do NerdDinner. (Observe que o site NerdDinner ao vivo foi atualizado para proteger contra ataques de redirecionamento abertos.)

Primeiro, um invasor nos envia um link para a página de logon no NerdDinner que inclui um redirecionamento para sua página forjada:

[http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn](http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn)

Observe que a URL de retorno aponta para nerddiner.com, que não tem um "n" do jantar de palavras. Neste exemplo, esse é um domínio que o invasor controla. Quando acessamos o link acima, vamos para a página de logon NerdDinner.com legítima.

[![](preventing-open-redirection-attacks/_static/image4.png)](preventing-open-redirection-attacks/_static/image3.png)

**Figura 02**: página de logon do NerdDinner com um redirecionamento aberto

Quando fazemos logon corretamente, a ação de LogOn do ASP.NET MVC AccountController redireciona-nos para a URL especificada no parâmetro returnUrl QueryString. Nesse caso, é a URL que o invasor inseriu, que é [http://nerddiner.com/Account/LogOn](http://nerddiner.com/Account/LogOn). A menos que seja extremamente olhar, é muito provável que não Notemos isso, especialmente porque o invasor tem cuidado para se certificar de que a página forjada é exatamente como a página de logon legítima. Essa página de logon inclui uma mensagem de erro solicitando que façamos logon novamente. Atarefado, devemos ter digitado a nossa senha inalguma.

[![](preventing-open-redirection-attacks/_static/image6.png)](preventing-open-redirection-attacks/_static/image5.png)

**Figura 03**: tela de logon do NerdDinner forjado

Quando redigitamos nosso nome de usuário e senha, a página de logon forjado salva as informações e nos envia de volta para o site NerdDinner.com legítimo. Neste ponto, o site do NerdDinner.com já se autenticou, portanto, a página de logon forjada pode redirecionar diretamente para essa página. O resultado final é que o invasor tem nosso nome de usuário e senha, e não estamos cientes de que fornecemos a eles.

## <a name="looking-at-the-vulnerable-code-in-the-accountcontroller-logon-action"></a>Examinando o código vulnerável na ação de LogOn do AccountController

O código para a ação de LogOn em um aplicativo ASP.NET MVC 2 é mostrado abaixo. Observe que, após um logon bem-sucedido, o controlador retorna um redirecionamento para a returnUrl. Você pode ver que nenhuma validação está sendo executada em relação ao parâmetro returnUrl.

**Listagem 1 – ASP.NET a ação de LogOn MVC 2 no `AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample1.cs)]

Agora, vejamos as alterações na ação de LogOn do ASP.NET MVC 3. Esse código foi alterado para validar o parâmetro returnUrl chamando um novo método na classe auxiliar System. Web. Mvc. URL chamada `IsLocalUrl()`.

**Listagem 2 – ação de LogOn do ASP.NET MVC 3 no `AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample2.cs)]

Isso foi alterado para validar o parâmetro de URL de retorno chamando um novo método na classe auxiliar System. Web. Mvc. URL, `IsLocalUrl()`.

## <a name="protecting-your-aspnet-mvc-10-and-mvc-2-applications"></a>Protegendo seus aplicativos ASP.NET MVC 1,0 e MVC 2

Podemos aproveitar as alterações do ASP.NET MVC 3 em nossos aplicativos ASP.NET MVC 1,0 e 2 existentes adicionando o método auxiliar IsLocalUrl () e atualizando a ação de LogOn para validar o parâmetro returnUrl.

O método UrlHelper IsLocalUrl (), na verdade, simplesmente chamando um método em System. Web. webpages, pois essa validação também é usada por aplicativos Páginas da Web do ASP.NET.

**Listagem 3 – o método IsLocalUrl () do ASP.NET MVC 3 UrlHelper `class`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample3.cs)]

O método IsUrlLocalToHost contém a lógica de validação real, conforme mostrado na Listagem 4.

**Listando o método 4 – IsUrlLocalToHost () da classe System. Web. webpages RequestExtensions**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample4.cs)]

Em nosso aplicativo ASP.NET MVC 1,0 ou 2, adicionaremos um método IsLocalUrl () ao AccountController, mas você será incentivado a adicioná-lo a uma classe auxiliar separada, se possível. Vamos fazer duas pequenas alterações na versão ASP.NET MVC 3 do IsLocalUrl () para que ela funcione dentro do AccountController. Primeiro, vamos alterá-lo de um método público para um método particular, já que os métodos públicos nos controladores podem ser acessados como ações do controlador. Em segundo lugar, modificaremos a chamada que verifica o host da URL no host do aplicativo. Essa chamada faz uso de um campo RequestContext local na classe UrlHelper. Em vez de usar isso. RequestContext. HttpContext. Request. URL. host, usaremos isso. Request. URL. host. O código a seguir mostra o método IsLocalUrl () modificado para uso com uma classe de controlador nos aplicativos ASP.NET MVC 1,0 e 2.

**Listagem 5 – método IsLocalUrl (), que é modificado para uso com uma classe de controlador MVC**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample5.cs)]

Agora que o método IsLocalUrl () está em vigor, podemos chamá-lo da nossa ação de LogOn para validar o parâmetro returnUrl, conforme mostrado no código a seguir.

**Listagem 6 – método de LogOn atualizado que valida o parâmetro returnUrl**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample6.cs)]

Agora, podemos testar um ataque de redirecionamento aberto ao tentar fazer logon usando uma URL de retorno externa. Vamos usar/Account/LogOn? ReturnUrl =<http://www.bing.com/> novamente.

[![](preventing-open-redirection-attacks/_static/image8.png)](preventing-open-redirection-attacks/_static/image7.png)

**Figura 04**: testando a ação de logon atualizada

Depois de fazer logon com êxito, somos redirecionados para a ação do controlador Home/index em vez da URL externa.

[![](preventing-open-redirection-attacks/_static/image10.png)](preventing-open-redirection-attacks/_static/image9.png)

**Figura 05**: o ataque de redirecionamento de abertura é derrotado

## <a name="summary"></a>Resumo

Os ataques de redirecionamento abertos podem ocorrer quando as URLs de redirecionamento são passadas como parâmetros na URL para um aplicativo. O modelo ASP.NET MVC 3 inclui código para proteger contra ataques de redirecionamento abertos. Você pode adicionar esse código com alguma modificação nos aplicativos ASP.NET MVC 1,0 e 2. Para proteger contra ataques de redirecionamento abertos ao fazer logon em aplicativos ASP.NET 1,0 e 2, adicione um método IsLocalUrl () e valide o parâmetro returnUrl na ação de LogOn.
