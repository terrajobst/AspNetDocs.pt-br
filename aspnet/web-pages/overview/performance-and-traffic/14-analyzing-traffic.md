---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: Acompanhando informações do visitante (Analytics) para um site Páginas da Web do ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Depois de acessar seu site, talvez você queira analisar o tráfego do site.
ms.author: riande
ms.date: 02/17/2014
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 095a5572c755446e0661c052ca9de82d636429fd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78525182"
---
# <a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a>Acompanhamento de informações do visitante (análise) para um site Páginas da Web do ASP.NET (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo descreve como usar um auxiliar para adicionar a análise de site a páginas em um site Páginas da Web do ASP.NET (Razor).
> 
> O que você aprenderá:
> 
> - Como enviar informações sobre o tráfego de seu site para um provedor de análise.
> 
> Estes são os recursos de programação do ASP.NET apresentados no artigo:
> 
> - O auxiliar de `Analytics`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 2
> - Biblioteca de auxiliares Web do ASP.NET (pacote NuGet)

A análise é um termo geral para a tecnologia que mede o tráfego em seu site para que você possa entender como as pessoas usam o site. Muitos serviços de análise estão disponíveis, incluindo serviços do Google, Yahoo, StatCounter e outros.

A maneira como o Analytics funciona é que você se inscreve em uma conta com o provedor de análise, no qual você registra o site que deseja acompanhar. O provedor envia um trecho de código JavaScript que inclui uma ID ou um código de rastreamento para sua conta. Você adiciona o trecho de JavaScript às páginas da Web no site que deseja acompanhar. (Normalmente, você adiciona o trecho de análise a uma página de rodapé ou de layout ou a outra marcação HTML que aparece em cada página do site.) Quando os usuários solicitam uma página que contém um desses trechos de código JavaScript, o trecho de código envia informações sobre a página atual para o provedor de análise, que registra vários detalhes sobre a página.

Quando você quiser ter uma olhada nas estatísticas do site, faça logon no site do provedor de análise. Você pode exibir todos os tipos de relatórios sobre seu site, como:

- O número de exibições de página para páginas individuais. Isso informa (aproximadamente) quantas pessoas estão visitando o site e quais páginas do seu site são as mais populares.
- Quanto tempo as pessoas gastam em páginas específicas. Isso pode lhe dizer coisas como se sua home page está mantendo o interesse das pessoas.
- Em que sites as pessoas estavam antes de visitar seu site. Isso ajuda você a entender se o tráfego está vindo de links, de pesquisas e assim por diante.
- Quando as pessoas visitam seu site e por quanto tempo elas permanecem.
- De quais países seus visitantes são.
- Quais navegadores e sistemas operacionais seus visitantes estão usando.

    ![Ch14traffic-1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a>Usando um auxiliar para adicionar análise a uma página

O Páginas da Web do ASP.NET inclui vários auxiliares de análise (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`e `Analytics.GetStatCounterHtml`) que facilitam o gerenciamento dos trechos de JavaScript usados para análise. Em vez de descobrir como e onde colocar o código JavaScript, tudo o que você precisa fazer é adicionar o auxiliar a uma página. As únicas informações que você precisa fornecer são o nome da sua conta, ID ou código de rastreamento. (Para StatCounter, você também precisa fornecer alguns valores adicionais.)

Neste procedimento, você criará uma página de layout que usa o auxiliar de `GetGoogleHtml`. Se você já tiver uma conta com um dos outros provedores de análise, poderá usar essa conta e fazer pequenos ajustes conforme necessário.

> [!NOTE]
> Ao criar uma conta de análise, você registra a URL do site que deseja controlar. Se você estiver testando tudo em seu computador local, não estará controlando o tráfego real (o único tráfego é você), portanto, você não conseguirá registrar e exibir as estatísticas do site. Mas esse procedimento mostra como você adiciona um auxiliar de análise a uma página. Quando você publicar seu site, o site ativo enviará informações ao seu provedor de análise.

1. Adicione a biblioteca de auxiliares Web do ASP.NET ao seu site, conforme descrito em [instalando auxiliares em um páginas da Web do ASP.net site](https://go.microsoft.com/fwlink/?LinkId=252372), se você ainda não o tiver adicionado.
2. Crie uma conta com o Google Analytics e registre o nome da conta.
3. Crie uma página de layout chamada *Analytics. cshtml* e adicione a seguinte marcação:

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > Você deve fazer a chamada para o auxiliar de `Analytics` no corpo da sua página da Web (antes da marca de `</body>`). Caso contrário, o navegador não executará o script.

    Se você estiver usando um provedor de análise diferente, use um dos seguintes auxiliares em vez disso:

    - (Yahoo) `@Analytics.GetYahooHtml("myaccount")`
    - (StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`
4. Substitua `myaccount` pelo nome da conta, ID ou código de rastreamento que você criou na etapa 1.
5. Execute a página no navegador. (Verifique se a página está selecionada no espaço de trabalho **arquivos** antes de executá-la.)
6. No navegador, exiba a origem da página. Você poderá ver o código de análise renderizado:

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. Faça logon no site do Google Analytics e examine as estatísticas do seu site. Se estiver executando a página em um site ativo, você verá uma entrada que registra a visita à sua página.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais

- [Site do Google Analytics](https://www.google.com/analytics/)
- [Site do Yahoo! Web Analytics](http://help.yahoo.com/l/us/yahoo/ywa/)
- [Site do StatCounter](http://statcounter.com/)
