---
uid: overview
title: Visão geral do ASP.NET | Microsoft Docs
author: rick-anderson
description: Introdução ao ASP.NET, uma estrutura gratuita para a criação de sites, aplicativos Web e APIs da Web.
ms.assetid: 3a309468-f1ca-4e51-b9c3-536af79d7a8b
ms.author: riande
ms.date: 08/10/2019
msc.legacyurl: ''
msc.type: content
ms.openlocfilehash: 9a6d08849f09c9d7a779df64f70e8770d2af3c87
ms.sourcegitcommit: b67ffd5b2c5cff01ec4c8eb12a21f693f2e11887
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/23/2019
ms.locfileid: "69995281"
---
# <a name="aspnet-overview"></a>Visão geral do ASP.NET

O ASP.NET é uma estrutura da Web gratuita para a criação de sites e aplicativos Web excelentes usando HTML, CSS e JavaScript. Você também pode criar APIs Web e usar tecnologias em tempo real como Web Sockets.

[ASP.NET Core](https://docs.microsoft.com/aspnet/core/) é uma alternativa ao ASP.net.  Consulte a [orientação sobre como escolher entre ASP.net e ASP.NET Core](https://docs.microsoft.com/aspnet/core/choose-aspnet-framework).

## <a name="get-started"></a>Introdução

Instale o [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community Edition, um IDE gratuito para ASP.net no Windows.

## <a name="websites-and-web-applications"></a>Sites e aplicativos Web

 O ASP.NET oferece três estruturas para a criação de aplicativos Web: Web Forms, ASP.NET MVC e Páginas da Web do ASP.NET. Todas as três estruturas são estáveis e maduras, e você pode criar ótimos aplicativos Web com qualquer um deles. Independentemente da estrutura escolhida, você terá todos os benefícios e recursos do ASP.NET em todos os lugares.

Cada estrutura tem como alvo um estilo de desenvolvimento diferente. O que você escolhe depende de uma combinação de seus ativos de programação (conhecimento, habilidades e experiência de desenvolvimento), o tipo de aplicativo que você está criando e a abordagem de desenvolvimento com a qual você está familiarizado.

Veja abaixo uma visão geral de cada uma das estruturas e algumas ideias sobre como escolher entre elas. Se você preferir uma introdução ao vídeo, consulte [fazendo sites com ASP.net](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/Making-Websites-with-ASPNET) e [o que são as ferramentas da Web?](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/what-is-web-tools)

|   | Se você tiver experiência em | Estilo de desenvolvimento | Conhecimentos |
|-----------|----------------------|-----------------------------------------------------|----------------|
| Web Forms | Win Forms, WPF, .NET | Desenvolvimento rápido usando uma biblioteca rica de controles que encapsulam marcação HTML | RAD avançado e de nível intermediário |
| MVC       | Ruby on Rails, .NET  | Controle total sobre a marcação HTML, o código e a marcação separados e os testes fáceis de escrever. A melhor opção para aplicativos móveis e de página única (SPA). | Nível intermediário, avançado |
| Páginas da Web  | ASP clássico, PHP     | Marcação HTML e seu código juntos no mesmo arquivo | Novo, de nível intermediário |

### <a name="web-forms"></a>Web Forms

Com o ASP.NET Web Forms, você pode criar sites dinâmicos usando um modelo familiar de arrastar e soltar orientado por evento. Uma superfície de design e centenas de controles e componentes permitem que você crie rapidamente sites sofisticados e avançados orientados por interface do usuário com acesso a dados.

[Saiba mais sobre o Web Forms](web-forms/index.md)

### <a name="mvc"></a>MVC

O ASP.NET MVC oferece uma maneira poderosa e baseada em padrões para a criação de sites dinâmicos que permitem uma separação clara de preocupações e que oferece controle total sobre a marcação de desenvolvimento agradável e ágil. O ASP.NET MVC inclui muitos recursos que permitem um desenvolvimento rápido e fácil de TDD para a criação de aplicativos sofisticados que usam os mais recentes padrões da Web.

[Saiba mais sobre o MVC](mvc/index.md)

### <a name="aspnet-web-pages"></a>Páginas da Web do ASP.NET

Páginas da Web do ASP.NET e o sintaxe Razor fornecem uma maneira rápida, acessível e leve de combinar o código do servidor com HTML para criar conteúdo da Web dinâmico. Conecte-se a bancos de dados, adicione vídeo, vincule a sites de rede social e inclua muitos outros recursos que o ajudarão a criar um lindo site que esteja em conformidade com os padrões da Web mais recentes.

[Saiba mais sobre páginas da Web](web-pages/index.md)

### <a name="notes-about-web-forms-mvc-and-web-pages"></a>Observações sobre Web Forms, MVC e páginas da Web

Todas as três estruturas ASP.NET são baseadas no .NET Framework e compartilham a funcionalidade básica do .NET e do ASP.NET. Por exemplo, todas as três estruturas oferecem um modelo de segurança de logon baseado em relação à associação, e todas as três compartilham os mesmos recursos para gerenciar solicitações, manipular sessões e assim por diante que fazem parte da funcionalidade principal do ASP.NET.

Além disso, as três estruturas não são totalmente independentes e a escolha de uma não impede o uso de outra. Como as estruturas podem coexistir no mesmo aplicativo Web, não é incomum ver componentes individuais de aplicativos escritos usando estruturas diferentes. Por exemplo, partes voltadas para o cliente de um aplicativo podem ser desenvolvidas no MVC para otimizar a marcação, enquanto o acesso a dados e as partes administrativas são desenvolvidos em Web Forms para aproveitar os controles de dados e o acesso a dados simples.

## <a name="web-apis"></a>APIs da Web

ASP.NET Web API é uma estrutura que facilita a criação de serviços HTTP que atingem uma ampla variedade de clientes, incluindo navegadores e dispositivos móveis. O ASP.NET Web API é uma plataforma ideal para o desenvolvimento de aplicativos RESTful no .NET Framework.

[Saiba mais sobre a API Web](web-api/index.md)

<!-- Put first under Web API TOC:  Watch video (9 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/services-and-aspnet -->

## <a name="real-time-technologies"></a>Tecnologias em tempo real

O Signalr ASP.NET é uma nova biblioteca para desenvolvedores de ASP.NET que facilita o desenvolvimento de funcionalidades da Web em tempo real. O signalr permite a comunicação bidirecional entre o servidor e o cliente. Os servidores podem enviar conteúdo por push a clientes conectados instantaneamente, pois eles ficam disponíveis. O signalr oferece suporte a Web Sockets e volta a outras técnicas compatíveis para navegadores mais antigos. O signalr inclui APIs para gerenciamento de conexão (por exemplo, eventos de conexão e desconexão), agrupamento de conexões e autorização.

[Saiba mais sobre o Signalr](signalr/index.md)

<!-- Put first under SignalR TOC:  Watch video (6 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/signalr-and-the-real-time-web -->

## <a name="mobile-apps-and-sites"></a>Aplicativos móveis e sites

O ASP.NET pode alimentar aplicativos móveis nativos com um back-end de API da Web, bem como sites móveis usando estruturas de design responsivas, como o bootstrap do Twitter. Se você estiver criando um aplicativo móvel nativo, será fácil criar uma API Web baseada em JSON para lidar com acesso a dados, autenticação e notificações por push para seu aplicativo. Se você estiver criando um site móvel responsivo, poderá usar qualquer estrutura CSS ou sistema de grade aberto que preferir, ou selecionar um sistema móvel poderoso como jQuery Mobile ou sencha e excelentes aplicativos móveis com PhoneGap.

[Saiba mais sobre o desenvolvimento de sites e aplicativos móveis](mobile/index.md)

<!-- Put first under mobile TOC:  Watch video (11 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/aspnet-and-mobile -->

## <a name="single-page-applications"></a>Aplicativos de página única

O aplicativo de página única (SPA) do ASP.NET ajuda a criar aplicativos que incluem interações significativas do lado do cliente usando HTML 5, CSS 3 e JavaScript. O Visual Studio inclui um modelo para a criação de aplicativos de página única usando knockout. js e ASP.NET Web API. Além do modelo SPA interno, os modelos SPA criados pela Comunidade também estão disponíveis para download.

[Saiba mais sobre o desenvolvimento de aplicativos de página única](single-page-application/index.md)

## <a name="webhooks"></a>WebHooks

WebHooks é um padrão HTTP leve que fornece um modelo pub/sub simples para ligar APIs Web e serviços SaaS. Quando um evento acontece em um serviço, uma notificação é enviada na forma de uma solicitação HTTP POST para assinantes registrados. A solicitação POST contém informações sobre o evento que possibilitam que o destinatário aja de forma adequada.

Os WebHooks são expostos por um grande número de serviços, incluindo Dropbox, GitHub, Instagram, MailChimp, PayPal, margem de atraso, Trello e muito mais. Por exemplo, um webhook pode indicar que um arquivo foi alterado no Dropbox, ou uma alteração de código foi confirmada no GitHub, ou um pagamento foi iniciado no PayPal ou um cartão foi criado no Trello.

[Saiba mais sobre WebHooks](webhooks/index.md)

<!--
Create Deployment TOC based on https://www.asp.net/aspnet/overview/deployment
Copy deployment content map to MVC, WebForms, Web Pages, Web API sections.
Copy Web Deployment in Enterprise from WebForms to MVC
Move under ASP.NET Best practices
    What not to do in ASP.NET, and what to do instead https://review.docs.microsoft.cus/aspnet/aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
    Async and await https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/async-and-await
    Building Real World Cloud Apps with Azure https://review.docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
    Hands on Lab: Maintainable Azure Websites: Managing Change and Scale https://review.docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale

-->
