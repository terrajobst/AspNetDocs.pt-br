---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: Usando um CAPTCHA para impedir que os bots usem seu site do ASP.NET Web Razor) | Microsoft Docs
author: microsoft
description: Este artigo explica como usar o ReCaptcha (uma medida de segurança) para impedir que os bots (programas automatizados) executem tarefas em um Páginas da Web do ASP.NET (Razor)...
ms.author: riande
ms.date: 05/21/2012
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: 2647a3155893a3dfb3214795a5f9cf1e8931fa91
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78547043"
---
# <a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a>Usando um CAPTCHA para impedir que os bots usem seu site do ASP.NET Web Razor)

pela [Microsoft](https://github.com/microsoft)

> Este artigo explica como usar o ReCaptcha (uma medida de segurança) para impedir que os bots (programas automatizados) executem tarefas em um site Páginas da Web do ASP.NET (Razor).
> 
> **O que você aprenderá:** 
> 
> - Como adicionar um teste do CAPTCHA ao seu site.
> 
> Estes são os recursos do ASP.NET apresentados no artigo:
> 
> - O auxiliar de `ReCaptcha`.
> 
> > [!NOTE]
> > As informações neste artigo se aplicam a Páginas da Web do ASP.NET 1,0 e páginas da Web 2.

## <a name="about-captchas"></a>Sobre o CAPTCHAs

Sempre que você permitir que as pessoas se registrem no seu site, ou mesmo apenas insiram um nome e uma URL (como para um comentário de blog), você poderá obter uma inundação de nomes falsos. Muitas vezes, eles são deixados por bots (programas automatizados) que tentam deixar URLs em todos os sites que eles podem encontrar. (Uma motivação comum é lançar as URLs de produtos para venda.)

Você pode ajudar a garantir que um usuário é uma pessoa real e não um programa de computador usando um *captcha* para validar os usuários quando eles se registram ou, de outra forma, inserem seu nome e site. CAPTCHA significa um teste de Turing público completamente automatizado para informar os computadores e os seres humanos. Um CAPTCHA é um teste de *desafio/resposta* no qual o usuário é solicitado a fazer algo que é fácil para uma pessoa, mas difícil de um programa automatizado. O tipo mais comum de CAPTCHA é aquele em que você vê algumas letras distorcidas e é solicitado a digitá-las. (A distorção deve dificultar a decifração das cartas pelos bots.)

## <a name="adding-a-recaptcha-test"></a>Adicionando um teste do ReCaptcha

Em páginas ASP.NET, você pode usar o auxiliar de `ReCaptcha` para processar um teste CAPTCHA baseado no serviço ReCaptcha ([http://recaptcha.net](http://recaptcha.net)). O auxiliar de `ReCaptcha` exibe uma imagem de duas palavras distorcidas que os usuários precisam inserir corretamente antes que a página seja validada. A resposta do usuário é validada pelo serviço ReCaptcha.Net.

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. Registre seu site em ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)). Quando você concluir o registro, obterá uma chave pública e uma chave privada.
2. Adicione a biblioteca de auxiliares Web do ASP.NET ao seu site, conforme descrito em [instalando auxiliares em um páginas da Web do ASP.net site](https://go.microsoft.com/fwlink/?LinkId=252372), se ainda não tiver feito isso.
3. Se você ainda não tiver um arquivo *\_AppStart. cshtml* , na pasta raiz de um site, crie um arquivo chamado *\_AppStart. cshtml*.
4. Adicione as seguintes configurações auxiliares de `Recaptcha` no arquivo *\_AppStart. cshtml* : 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. Defina as propriedades `PublicKey` e `PrivateKey` usando suas próprias chaves públicas e privadas.
6. Salve o arquivo *\_AppStart. cshtml* e feche-o.
7. Na pasta raiz de um site, crie uma nova página chamada *reCAPTCHA. cshtml*.
8. Substitua o conteúdo existente pelo seguinte: 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. Execute a página *reCAPTCHA. cshtml* em um navegador. Se o valor de `PrivateKey` for válido, a página exibirá o controle ReCaptcha e um botão. Se você não tiver definido as chaves globalmente em *\_AppStart. html*, a página exibiria um erro. 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. Insira as palavras para o teste. Se você passar no teste ReCaptcha, verá uma mensagem para esse efeito. Caso contrário, você verá uma mensagem de erro e o controle ReCaptcha será exibido novamente.

> [!NOTE]
> Se o computador estiver em um domínio que usa o servidor proxy, talvez seja necessário configurar o elemento `defaultproxy` do arquivo *Web. config* . O exemplo a seguir mostra um arquivo *Web. config* com o elemento `defaultproxy` configurado para permitir que o serviço reCAPTCHA funcione.
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais

- [Personalizando o comportamento de todo o site para sites Páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Site do ReCaptcha](https://www.google.com/recaptcha)
