---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/more-patterns-and-guidance
title: Mais padrões e diretrizes (criando aplicativos de nuvem do mundo real com o Azure) | Microsoft Docs
author: MikeWasson
description: A criação de aplicativos de nuvem do mundo real com o livro eletrônico do Azure é baseada em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas que podem...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 7e97cfc3-d830-4002-8ff7-5790d1ff49e6
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/more-patterns-and-guidance
msc.type: authoredcontent
ms.openlocfilehash: afade34477d1136883e7543d09e73dfbe435690e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74585355"
---
# <a name="more-patterns-and-guidance-building-real-world-cloud-apps-with-azure"></a>Mais padrões e diretrizes (criando aplicativos de nuvem do mundo real com o Azure)

por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Baixar o projeto de ti](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [baixar o livro eletrônico](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> A **criação de aplicativos de nuvem do mundo real com** o livro eletrônico do Azure é baseada em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas que podem ajudá-lo a desenvolver com sucesso aplicativos Web para a nuvem. Para obter informações sobre o livro eletrônico, consulte [o primeiro capítulo](introduction.md).

Agora você viu 13 padrões que fornecem orientação sobre como obter êxito na computação em nuvem. Esses são apenas alguns dos padrões que se aplicam aos aplicativos de nuvem. Aqui estão alguns tópicos mais sobre computação em nuvem e recursos para ajudar com eles:

- Migrar aplicativos locais existentes para a nuvem. 

    - [Movendo aplicativos para a nuvem](https://msdn.microsoft.com/library/ff728592.aspx). Livro eletrônico por padrões e práticas da Microsoft. Também disponível como uma [Paperback de cópia impressa](https://www.amazon.com/dp/1621140202).
    - [Migrando o ASP.net e o IIS.NET da Microsoft](https://go.microsoft.com/fwlink/?LinkId=400656). Estudo de caso de Robert McMurray.
    - [Movendo o 4º &amp; prefeito para os sites do Azure](http://www.jeff.wilcox.name/2013/04/4thandmayor-azure-websites/). Postagem de blog de Jeff Wilcox chronicling sua experiência ao mover um aplicativo Web de Amazon Web Services para aplicativos Web no serviço Azure App.
    - [Movendo aplicativos para o Azure: o que é alterado?](https://azure.microsoft.com/documentation/videos/web-sites-internals-and-the-file-system/) Vídeo curto por Stefan Schackow, explica o acesso ao sistema de arquivos em aplicativos Web no serviço Azure App.
    - [Nuvem híbrida do Azure](https://www.amazon.com/dp/B00EOP4UQW). Impresso livro ou livro eletrônico por Danny Garber, Jamal Malik e Adam Fazio.
- Problemas de segurança, autenticação e autorização exclusivos para aplicativos de nuvem

    - [Diretrizes de segurança do Azure](https://azure.microsoft.com/blog/2014/02/10/best-practices-windows-azure-websites-waws/)
    - [Práticas e padrões da Microsoft – diretrizes do Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consulte padrão de Gatekeeper, padrão de identidade federada.
    - [Segurança de rede do Azure](https://download.microsoft.com/download/4/3/9/43902EC9-410E-4875-8800-0788BE146A3D/Windows%20Azure%20Network%20Security%20Whitepaper%20-%20FINAL.docx). White Paper de Ashin Palekar.

Consulte também padrões e diretrizes adicionais de computação em nuvem em [padrões e práticas da Microsoft – diretrizes do Azure](https://msdn.microsoft.com/library/dn568099.aspx).

<a id="resources"></a>
## <a name="resources"></a>Recursos

Cada um dos capítulos deste livro eletrônico fornece links para recursos para obter mais informações sobre esse tópico específico. A lista a seguir fornece links para visões gerais de práticas recomendadas e padrões recomendados para o desenvolvimento de nuvem bem-sucedido com o Azure.

Documentação

- [Práticas recomendadas para o design de serviços em grande escala nos serviços de nuvem do Azure](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). White Paper por Mark Simms e Michael Thomassy.
- [FailSafe: Diretrizes para arquiteturas de nuvem resilientes](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). White Paper por Marc Mercuri, Ulrich Homann e Andrew TOWNHILL. Versão da página da Web da série de vídeos FailSafe.
- [Diretrizes do Azure](https://azure.microsoft.com/develop/net/guidance/) Página do portal para documentação oficial relacionada ao desenvolvimento de aplicativos para o Azure.

Vídeos

- [Criando aplicativos de nuvem do mundo real com o Azure-parte 1](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324) e [parte 2](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325). Vídeo da apresentação de Scott Guthrie em que este e-book se baseia. Apresentado em Tech Ed Austrália em setembro de 2013. Uma versão anterior da mesma apresentação foi entregue em norueguês Developers Conference (NDC) em junho de 2013: [NDC parte 1](http://vimeo.com/68215538), [NDC parte 2](http://vimeo.com/68215602).
- [FailSafe: criando serviços de nuvem escalonáveis e resilientes](https://channel9.msdn.com/Series/FailSafe). Série de vídeos de nove partes por Ulrich Homann, Marc Mercuri e marcas Simms. Apresenta uma exibição de nível 400 de como arquitetar aplicativos de nuvem. Esta série se concentra na teoria e nos motivos por trás dos padrões recomendados; para obter mais detalhes, consulte a criação de séries grandes por marca Simms.
- [Criando Big: lições aprendidas de clientes do Azure-parte 1](https://channel9.msdn.com/Events/Build/2012/3-029) e [parte 2](https://channel9.msdn.com/Events/Build/2012/3-030). Série de vídeos de duas partes de Simon Davies e Mark Simms, semelhante à série FailSafe, mas orientada mais em direção à implementação prática.

Exemplos de código

- [O aplicativo corrigir ti que acompanha este livro eletrônico](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4?cdn_id=2013-12-03-002).
- [Conceitos básicos do serviço de nuvem no C# Azure no para Visual Studio 2012](https://aka.ms/csf). O projeto que pode ser baixado no site da Galeria de código da Microsoft inclui o código e a documentação desenvolvidos pela equipe de consultoria ao cliente da Microsoft (CAT). Demonstra muitas das práticas recomendadas defensoras da FailSafe e da criação de grandes séries de vídeos e da white paper FailSafe. A página da Galeria de códigos também é vinculada a uma ampla documentação pelos autores do projeto – consulte especialmente a [coleção de wikis conceitos básicos do serviço de nuvem](https://social.technet.microsoft.com/wiki/contents/articles/17987.cloud-service-fundamentals.aspx) na caixa azul próxima à parte superior da descrição do projeto. Este projeto e a documentação para ele ainda estão sendo desenvolvidos ativamente, o que o torna uma opção melhor para informações sobre vários tópicos do que os White papers semelhantes, mas mais antigos.

Manuais de cópia impressa

- [Computação em nuvem Bíblia](https://www.amazon.com/dp/0470903562). Por Barrie Sosinsky.
- [Versão! projete e implante o software pronto para produção](https://www.amazon.com/Release-It-Production-Ready-Pragmatic-Programmers/dp/0978739213). Por Michael T. Nygard.
- [Padrões de arquitetura de nuvem: usando Microsoft Azure](http://shop.oreilly.com/product/0636920023777.do). Por Bill Wilder.
- [Plataforma Windows Azure](https://www.amazon.com/dp/1430235632). Por Tejaswi Redkar.
- [Padrões de programação do Windows Azure para Start-ups](https://www.amazon.com/dp/1849685606). Por Riccardo Becker.
- [Manual de desenvolvimento do Microsoft Windows Azure](https://www.amazon.com/dp/1849682224). Por Neil Mackenzie.

Por fim, quando você começar a criar aplicativos do mundo real e executá-los no Azure, mais cedo ou mais tarde, provavelmente precisará de assistência de especialistas. Você pode fazer perguntas em sites de comunidades, como [fóruns do Azure ou StackOverflow](https://azure.microsoft.com/support/forums/), ou pode contatar a Microsoft diretamente para o suporte do Azure. A Microsoft oferece vários níveis de suporte técnico Azure: para obter um resumo e uma comparação das opções, consulte [suporte do Azure](https://azure.microsoft.com/support/plans/).

<a id="acknowledgments"></a>
## <a name="acknowledgments"></a>Agradecimentos

Esse conteúdo foi escrito por Tom Dykstra, Rick Anderson e Mike Wasson. A maior parte do conteúdo original veio de [Scott Guthrie](https://weblogs.asp.net/scottgu/), e ele, por sua vez, atraiu material de Mark Simms e a equipe de consultoria ao cliente da Microsoft (CAT).

Muitos outros colegas da Microsoft revisaram e comentaram em rascunhos e código:

- Tim Ammann-analisamos o capítulo de automação.
- Christopher Bennage – analisou e testou o código de ti de correção.
- Ryan Berry-analisa o capítulo CD/CI.
- Vittorio Bertocci-analisamos o capítulo SSO.
- Chris Clayton – ajudou a resolver problemas técnicos nos scripts do PowerShell.
- Conor Cunningham-analisa o capítulo opções de armazenamento de dados.
- Carlos farre – analisou e testou a correção do código de ti para problemas de segurança.
- Larry Franks-revisou o capítulo telemetria e monitoramento.
- Jonathan Gao – seções do Hadoop e do MapReduce examinadas do capítulo opções de armazenamento de dados.
- Sidney Higa-analisa todos os capítulos.
- Gordon Hogenson-analisa o capítulo de controle do código-fonte.
- Tamra Myers-analisa os capítulos opções de armazenamento de dados, BLOB e filas.
- Pranav Rastogi-analisamos o capítulo SSO.
- Rogers misturador de junho-o tratamento de erros foi adicionado e ajuda para os scripts de automação do PowerShell.
- Mani Subramanian – analisou todos os capítulos e liderou o processo de revisão e teste de código para a correção do código de ti.
- Shaun Tinline-Jones-analisou o capítulo de particionamento de dados.
- Selcin Tukarslan-capítulos examinados que abrangem o banco de dados SQL e SQL Server.
- Edward Wu – código de exemplo fornecido para o capítulo de SSO.
- Guang Yang-escreveu os scripts de automação do PowerShell.

Os membros do [Conselho Consultivo de diretrizes para desenvolvedores da Microsoft](https://aka.ms/DGAC) (DGAC) também foram revisados e comentados em rascunhos:

- Jean-Luc Boucho
- Catalin Gheorghiu
- Wouter de Kort
- Carlos dos Santos
- Neil Mackenzie
- Dennis Persson
- Sunil Saba
- [Aleksey Sinyagin](http://www.linkedin.com/in/sinyagin)
- Bill Wagner
- Michael madeira

Outros membros da DGAC revisados e comentados na estrutura preliminar:

- Damir Arh
- Edward Bakker
- Srdjan Bozovic
- Ming Man
- Gianni Rosa Gallina
- Paulo MORGADO
- Jason Oliveira
- Alberto Poblacion
- Ryan Riley
- Perez Jones Tsisah
- Roger Whitehead
- Pawel Wilkosz

> [!div class="step-by-step"]
> [Anterior](queue-centric-work-pattern.md)
> [Próximo](the-fix-it-sample-application.md)
