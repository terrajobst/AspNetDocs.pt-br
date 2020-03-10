---
uid: mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
title: Ciclo de vida de um aplicativo ASP.NET MVC 5 | Microsoft Docs
author: cephalin
description: Baixe um documento PDF que mostra graficamente o ciclo de vida de um aplicativo ASP.NET MVC 5. Este documento de ciclo de vida fornece uma exibição de alto nível do ciclo de vida do MVC...
ms.author: riande
ms.date: 02/28/2014
ms.assetid: 9c1e3a75-b644-4480-8326-11300b1ec4b3
msc.legacyurl: /mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
msc.type: authoredcontent
ms.openlocfilehash: f4a9b3fb61552b070db11fba617b5627fcd71cd5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78582197"
---
# <a name="lifecycle-of-an-aspnet-mvc-5-application"></a>Ciclo de vida de um aplicativo do ASP.NET MVC 5

por [Cephas Lin](https://github.com/cephalin)

[Baixar documento PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)

Aqui, você pode baixar um documento em PDF que faz o gráfico do ciclo de vida de cada aplicativo ASP.NET MVC 5, desde o recebimento da solicitação HTTP até o envio da resposta HTTP para o cliente. Ele foi projetado como uma ferramenta educacional para aqueles que são novos no ASP.NET MVC e também como uma referência para aqueles que precisam analisar aspectos específicos do aplicativo. O documento PDF tem os seguintes recursos:

- Os estágios de [HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) relevantes para ajudá-lo a entender onde o MVC se integra ao [ciclo de vida do aplicativo ASP.net](https://msdn.microsoft.com/library/bb470252.aspx).
- Uma exibição de alto nível do ciclo de vida do aplicativo MVC, onde você pode entender os principais estágios que cada aplicativo MVC passa no pipeline de processamento de solicitações.  
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image1.jpg)
- Um modo de exibição de detalhes que mostra a busca detalhada dos detalhes do pipeline de processamento de solicitações. Você pode comparar a exibição de alto nível e a exibição de detalhes para ver como os detalhes dos ciclos de vida são coletados em vários estágios. [Baixe PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) para ver uma exibição maior.
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image2.jpg)
- Posicionamento e finalidade de todos os métodos substituíveis no objeto do [controlador](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx) no pipeline de processamento de solicitação. Você pode ou não ter a necessidade de substituir qualquer método, mas é importante que você entenda sua função no ciclo de vida do aplicativo para que você possa escrever código no estágio do ciclo de vida apropriado para o efeito que você pretende.
- Diagramas de explosão mostrando como cada um dos tipos de filtro (autenticação, autorização, ação e resultado) é invocado.
- Link para um artigo ou blog útil de cada ponto de interesse na exibição de detalhes.

## <a name="next-steps"></a>Próximas etapas

Este documento atende às suas necessidades? Agradecemos seus comentários. Se você tiver alguma dúvida sobre o ciclo de vida do ASP.NET MVC em seu aplicativo, [StackOverflow](http://stackoverflow.com/help) e os [fóruns do ASP.NET MVC](https://forums.asp.net/1146.aspx) serão ótimos lugares a serem solicitados. Siga [-me](https://twitter.com/Cephas_MSFT) no Twitter para que você possa obter atualizações sobre meus tutoriais mais recentes.
