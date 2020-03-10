---
uid: web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
title: Armazenando dados em cache em um site Páginas da Web do ASP.NET (Razor) para melhorar o desempenho | Microsoft Docs
author: Rick-Anderson
description: Você pode acelerar seu site fazendo com que ele armazene, ou seja, armazenar em cache-os resultados dos dados que normalmente levam um tempo considerável para recuperar ou processar um...
ms.author: riande
ms.date: 02/14/2014
ms.assetid: 961e525b-7700-469e-8a68-d7010b6fb68c
msc.legacyurl: /web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
msc.type: authoredcontent
ms.openlocfilehash: 01796d3ca699a6af5d9162b22a926551435c2040
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78641515"
---
# <a name="caching-data-in-an-aspnet-web-pages-razor-site-for-better-performance"></a>Armazenando dados em cache em um site Páginas da Web do ASP.NET (Razor) para melhorar o desempenho

por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo explica como usar um auxiliar para armazenar em cache informações para um desempenho mais rápido em um site Páginas da Web do ASP.NET (Razor). Você pode acelerar seu site fazendo com &#8212; que ele seja armazenado, armazenando &#8212; em cache os resultados dos dados que normalmente levam um tempo considerável para recuperar ou processar e isso não é alterado com frequência.
> 
> **O que você aprenderá:** 
> 
> - Como usar o Caching para melhorar a capacidade de resposta do seu site.
> 
> Estes são os recursos do ASP.NET apresentados no artigo:
> 
> - O auxiliar de `WebCache`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 3
>   
> 
> Este tutorial também funciona com o Páginas da Web do ASP.NET 2.

Toda vez que alguém solicita uma página do seu site, o servidor Web precisa fazer algum trabalho para atender à solicitação. Para algumas de suas páginas, o servidor pode ter que executar tarefas que levam um tempo longo (comparativamente), como recuperar dados de um banco de dado. Mesmo que essas tarefas não demorem muito em termos absolutos, se o seu site experimentar muito tráfego, uma série completa de solicitações individuais que fazem com que o servidor Web execute a tarefa complicada ou lenta pode aumentar muito o trabalho. Isso pode, por fim, afetar o desempenho do site.

Uma maneira de melhorar o desempenho do seu site em circunstâncias como essa é armazenar os dados em cache. Se o seu site receber solicitações repetidas para as mesmas informações e se as informações não precisarem ser modificadas para cada pessoa, e não for um tempo sensível, em vez de buscar novamente ou recalcular, você poderá buscar os dados uma vez e, em seguida, armazenar os resultados. Na próxima vez que uma solicitação chegar para essas informações, você simplesmente a obtém do cache.

Em geral, você armazena em cache informações que não são alteradas com frequência. Quando você coloca as informações no cache, elas são armazenadas na memória no servidor Web. Você pode especificar quanto tempo deve ser armazenado em cache, de segundos a dias. Quando o período de cache expira, as informações são automaticamente removidas do cache.

> [!NOTE]
> As entradas no cache podem ser removidas por razões diferentes da expiração. Por exemplo, o servidor Web pode ser executado temporariamente com pouca memória e uma maneira de recuperar memória é emitindo entradas do cache. Como você verá, mesmo que tenha colocado informações no cache, precisará verificar se ele ainda está lá quando você precisar dele.

Imagine que seu site tem uma página que exibe a temperatura atual e a previsão do tempo. Para obter esse tipo de informação, você pode enviar uma solicitação para um serviço externo. Como essas informações não mudam muito (dentro de um período de duas horas, por exemplo) e como as chamadas externas exigem tempo e largura de banda, é um bom candidato para Caching.

## <a name="adding-caching-to-a-page"></a>Adicionando cache a uma página

O ASP.NET inclui um `WebCache` auxiliar que facilita a adição de cache ao seu site e a adição de dados ao cache. Neste procedimento, você criará uma página que armazena em cache a hora atual. Esse não é um exemplo do mundo real, já que a hora atual é algo que muda com frequência e que não é complexo calcular. No entanto, é uma boa maneira de ilustrar o Caching em ação.

1. Adicione uma nova página chamada *WebCache. cshtml* ao site.
2. Adicione o seguinte código e marcação à página:

    [!code-cshtml[Main](15-caching-to-improve-the-performance-of-your-website/samples/sample1.cshtml)]

    Ao armazenar dados em cache, você o coloca no cache usando um nome que é exclusivo em todo o site. Nesse caso, você usará uma entrada de cache chamada `CachedTime`. Este é o `cacheItemKey` mostrado no exemplo de código.

    O código primeiro lê a entrada de cache `CachedTime`. Se um valor for retornado (ou seja, se a entrada de cache não for nula), o código apenas definirá o valor da variável de tempo para os dados de cache.

    No entanto, se a entrada de cache não existir (ou seja, for nula), o código definirá o valor de hora, o adicionará ao cache e definirá um valor de expiração como um minuto. Após um minuto, a entrada de cache é descartada. (O valor de expiração padrão para um item no cache é de 20 minutos.) O comando `WebCache.Set(cacheItemKey, time, 1, false)` mostra como adicionar o valor de hora atual ao cache e definir sua expiração como 1 minuto. Definir o parâmetro *slidingExpiration* como `false` significa que a hora de expiração não é renovada sempre que for solicitada. Ele expirará exatamente um minuto após ter sido inicialmente adicionado ao cache. Se você definir esse valor como `true` o tempo de expiração de 1 minuto será redefinido cada vez que o valor for solicitado do cache.

    Esse código ilustra o padrão que você sempre deve usar ao armazenar dados em cache. Antes de obter algo fora do cache, sempre verifique primeiro se o método de `WebCache.Get` retornou nulo. Lembre-se de que a entrada de cache pode ter expirado ou pode ter sido removida por algum outro motivo, de modo que qualquer entrada não terá garantia de estar no cache.
3. Execute *WebCache. cshtml* em um navegador. (Verifique se a página está selecionada no espaço de trabalho **arquivos** antes de executá-la.) Na primeira vez que você solicitar a página, os dados de hora não estão no cache e o código precisa adicionar o valor de tempo ao cache.

    ![cache-1](15-caching-to-improve-the-performance-of-your-website/_static/image1.jpg)
4. Atualize *WebCache. cshtml* no navegador. Desta vez, os dados de hora estão no cache. Observe que o tempo não foi alterado desde a última vez que você exibiu a página.

    ![cache-2](15-caching-to-improve-the-performance-of-your-website/_static/image2.jpg)
5. Aguarde um minuto para que o cache seja esvaziado e, em seguida, atualize a página. A página novamente indica que os dados de tempo não foram encontrados no cache e a hora atualizada é adicionada ao cache.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais

- [Exibindo dados em um gráfico](https://go.microsoft.com/fwlink/?LinkId=202895)
- [Referência da API do WebCache](https://msdn.microsoft.com/library/system.web.helpers.webcache(v=vs.99).aspx) (MSDN)
