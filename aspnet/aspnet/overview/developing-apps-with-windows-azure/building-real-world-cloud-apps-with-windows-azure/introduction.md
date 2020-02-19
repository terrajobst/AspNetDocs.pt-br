---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
title: Criando aplicativos de nuvem do mundo real com o Azure | Microsoft Docs
author: MikeWasson
description: Este livro eletrônico orienta você por uma abordagem baseada em padrões para a criação de soluções de nuvem do mundo real. Os padrões se aplicam ao processo de desenvolvimento, bem como a...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: accfa16a-ab15-4c26-9ad4-babdc2a77d2e
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
msc.type: authoredcontent
ms.openlocfilehash: b88573b3702b755b155e8da35f5f8a67931bafc6
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457109"
---
# <a name="building-real-world-cloud-apps-with-azure"></a>Criando aplicativos de nuvem do mundo real com o Azure

por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Baixar o projeto de ti](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [baixar o livro eletrônico](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Este livro eletrônico orienta você por uma abordagem baseada em padrões para a criação de soluções de nuvem do mundo real. Os padrões se aplicam ao processo de desenvolvimento, bem como às práticas de arquitetura e codificação.
> 
> O conteúdo é baseado em uma apresentação desenvolvida por Scott Guthrie e entregue por ele na conferência de desenvolvedores de norueguês (NDC) em junho de 2013 ([parte 1](http://vimeo.com/68215538), [parte 2](http://vimeo.com/68215602)) e no Microsoft Tech Ed austrália em setembro de 2013 ([parte 1](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324), [parte 2](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325)). [Muitos outros](more-patterns-and-guidance.md#acknowledgments) atualizaram e aumentaram o conteúdo ao fazer a transição do vídeo para o formato escrito.

## <a name="intended-audience"></a>Público-Alvo

Os desenvolvedores que estão curiosos sobre o desenvolvimento para a nuvem, considerando uma mudança para a nuvem ou são novos no desenvolvimento na nuvem encontrarão aqui uma visão geral concisa dos conceitos e das práticas mais importantes que eles precisam saber. Os conceitos são ilustrados com exemplos concretos e cada capítulo vincula-se a outros recursos para obter informações mais detalhadas. Os exemplos e os links para recursos adicionais são para Microsoft frameworks e serviços, mas os princípios ilustrados também se aplicam a outras estruturas de desenvolvimento da Web e ambientes de nuvem.

Os desenvolvedores que já estão desenvolvendo para a nuvem podem encontrar ideias aqui que o ajudarão a torná-las mais bem-sucedidas. Cada capítulo da série pode ser lido de forma independente, para que você possa escolher os tópicos nos quais está interessado.

Qualquer pessoa que observou Scott Guthrie *criando aplicativos de nuvem do mundo real com a apresentação do Azure* e quer mais detalhes e informações atualizadas descobrirá isso aqui.

<a id="patterns"></a>
## <a name="cloud-development-patterns"></a>Padrões de desenvolvimento de nuvem

Este livro eletrônico explica treze padrões recomendados para o desenvolvimento em nuvem. "Pattern" é usado aqui em um sentido mais amplo para significar uma maneira recomendada de fazer coisas: a melhor forma de desenvolver, projetar e codificar aplicativos de nuvem. Esses são os principais padrões que o ajudarão a "entrar no Pit de sucesso" se você os seguir.

- [Automatize tudo](automate-everything.md).

    - Use scripts para maximizar a eficiência e minimizar erros em processos repetitivos.
    - Demonstração: scripts de gerenciamento do Azure.
- [Controle do código-fonte](source-control.md). 

    - Configure a estrutura de ramificação no controle do código-fonte para facilitar o fluxo de trabalho do DevOps.
    - Demonstração: adicionar scripts ao controle do código-fonte.
    - Demonstração: manter dados confidenciais fora do controle do código-fonte.
    - Demonstração: Use o Git no Visual Studio.
- [Integração e entrega contínuas](continuous-integration-and-continuous-delivery.md). 

    - Automatize a compilação e a implantação com cada check-in de controle do código-fonte.
- [Práticas recomendadas de desenvolvimento](web-development-best-practices.md)para a Web. 

    - Manter sem estado da camada da Web.
    - Demonstração: dimensionamento e dimensionamento automático em aplicativos Web no serviço Azure App.
    - Evite o estado da sessão.
    - Use uma CDN com um fallback quando a CDN não estiver disponível.
    - Use o modelo de programação assíncrona.
    - Demonstração: Async no ASP.NET MVC e Entity Framework.
- [Logon único](single-sign-on.md). 

    - Introdução ao Azure Active Directory.
    - Demonstração: Crie um aplicativo ASP.NET que usa Azure Active Directory.
- [Opções de armazenamento de dados](data-storage-options.md). 

    - Tipos de armazenamentos de dados.
    - Como escolher o armazenamento de dados correto.
    - Demonstração: banco de dados SQL do Azure.
- [Estratégias de particionamento de dados](data-partitioning-strategies.md). 

    - Particione os dados verticalmente, horizontalmente ou ambos para facilitar o dimensionamento de um banco de dados relacional.
- [Armazenamento de BLOBs não estruturado](unstructured-blob-storage.md). 

    - Armazene arquivos na nuvem usando o serviço BLOB.
    - Demonstração: usando o armazenamento de BLOBs no aplicativo corrigir ti.
- [Design para sobreviver a falhas](design-to-survive-failures.md). 

    - Tipos de falhas.
    - Escopo de falha.
    - Noções básicas sobre SLAs.
- [Monitoramento e telemetria](monitoring-and-telemetry.md). 

    - Por que você deve comprar um aplicativo de telemetria e escrever seu próprio código para instrumentar seu aplicativo.
    - Demonstração: novo Relic para o Azure
    - Demonstração: registrar em log o código no aplicativo corrigir ti.
    - Demonstração: injeção de dependência no aplicativo corrigir ti.
    - Demonstração: suporte a log interno no Azure.
- [Tratamento de falhas transitórias](transient-fault-handling.md). 

    - Use a lógica de repetição/retirada inteligente para atenuar o efeito de falhas transitórias.
    - Demonstração: repetição/retirada no Entity Framework 6.
- [Caching distribuído](distributed-caching.md). 

    - Melhorar a escalabilidade e reduzir os custos de transações do banco de dados usando o Caching distribuído.
- [Padrão de trabalho centrado em fila](queue-centric-work-pattern.md). 

    - Habilite a alta disponibilidade e melhore a escalabilidade ao acoplar de forma flexível as camadas Web e de trabalho.
    - Demonstração: filas do armazenamento do Azure no aplicativo corrigir ti.
- [Mais padrões e diretrizes de aplicativos de nuvem](more-patterns-and-guidance.md).
- [Apêndice: o aplicativo de exemplo Fix It](the-fix-it-sample-application.md)

    - Problemas conhecidos
    - Práticas recomendadas
    - Como baixar, compilar, executar e implantar.

Esses padrões se aplicam a todos os ambientes de nuvem, mas os ilustraremos usando exemplos baseados em tecnologias e serviços da Microsoft, como o Visual Studio, o Team Foundation Service, o ASP.NET e o Azure.

Este restante deste capítulo apresenta o aplicativo de exemplo Fix it e os aplicativos Web em Azure App ambiente de nuvem do serviço em que o aplicativo Fix it é executado.

<a id="fixit"></a>
## <a name="the-fix-it-sample-application"></a>O aplicativo de exemplo corrigir a ti

A maioria das capturas de tela e os exemplos de código mostrados neste livro eletrônico são baseados no aplicativo Fix it originalmente desenvolvido por [Scott Guthrie](https://weblogs.asp.net/scottgu/) para demonstrar padrões e práticas recomendadas de desenvolvimento de aplicativos em nuvem.

![Corrigir home page de aplicativo de ti](introduction/_static/image1.png)

O aplicativo de exemplo é um sistema de tíquete de item de trabalho simples. Quando você precisar de algo corrigido, crie um tíquete e atribua-o a alguém, e outros podem fazer logon e ver os tíquetes atribuídos a eles e marcar tíquetes como concluídos quando o trabalho for concluído.

É um projeto Web padrão do Visual Studio. Ele se baseia no ASP.NET MVC e usa um banco de dados SQL Server. Ele pode ser executado localmente no IIS Express e pode ser implantado em um site do Azure para ser executado na nuvem. Você pode fazer logon usando a autenticação de formulários e um banco de dados local ou usando um provedor social, como o Google. (Posteriormente, também mostraremos como fazer logon com uma conta organizacional Active Directory.)

![Página de logon](introduction/_static/image2.png)

Quando você estiver conectado, poderá criar um tíquete, atribuí-lo a alguém e carregar uma imagem do que você deseja corrigir.

![Criar uma tarefa corrigir ti](introduction/_static/image3.png)

![Tarefa de correção de ti criada](introduction/_static/image4.png)

Você pode acompanhar o progresso dos itens de trabalho que criou, Ver os tíquetes atribuídos a você, exibir detalhes do tíquete e marcar itens como concluídos.

Esse é um aplicativo muito simples de uma perspectiva de recurso, mas você verá como compilá-lo para que ele possa ser dimensionado para milhões de usuários e será resiliente a coisas como falhas de banco de dados e terminações de conexão. Você também verá como criar um fluxo de trabalho de desenvolvimento automatizado e ágil, que permite que você comece de forma simples e melhore o aplicativo Iterando o ciclo de desenvolvimento com eficiência e rapidez.

<a id="waws"></a>
## <a name="web-apps-in-azure-app-service"></a>Aplicativos Web no serviço Azure App

O ambiente de nuvem usado para corrigir o aplicativo de ti é um serviço do Azure que chamamos de sites da Web. Esse serviço é uma maneira de hospedar seu próprio aplicativo Web no Azure sem a necessidade de criar VMs e mantê-las atualizadas, instalar e configurar o IIS, etc. Hospedamos seu site em nossas VMs e fornecemos automaticamente backup e recuperação e outros serviços para você. O serviço de sites funciona com ASP.NET, Node. js, PHP e Python. Ele permite que você implante muito rapidamente usando o Visual Studio, Implantação da Web, FTP, git ou TFS. Geralmente, é apenas alguns segundos entre o tempo que você inicia uma implantação e a hora em que a atualização está disponível pela Internet. Tudo está livre para começar, e você pode escalar verticalmente à medida que o tráfego cresce.

Nos bastidores, os aplicativos Web no serviço de Azure App fornecem muitos componentes e recursos de arquitetura que você teria que criar por conta própria se fosse hospedar um site usando o IIS em suas próprias VMs. Um componente é um ponto de extremidade de implantação que configura automaticamente o IIS e instala seu aplicativo em tantas VMs quanto você desejar para executar seu site.

![Serviço de implantação](introduction/_static/image5.png)

Quando um usuário acessa o site, eles não atingem as VMs do IIS diretamente, eles passam pelos balanceadores [de carga Application Request Routing (ARR)](https://www.iis.net/downloads/microsoft/application-request-routing) . Você pode usá-los com seus próprios servidores, mas a vantagem aqui é que eles são configurados para você automaticamente. Eles usam uma heurística inteligente que leva em consideração fatores como afinidade de sessão, profundidade da fila no IIS e uso da CPU em cada máquina para direcionar o tráfego para as VMs que hospedam seu site.

![Balanceador de carga ARR](introduction/_static/image6.png)

Se um computador falhar, o Azure o extrairá automaticamente da rotação, criará uma nova instância de VM e começará a direcionar o tráfego para a nova instância, tudo isso sem tempo de inatividade para seu aplicativo.

![Recuperação automática de falha do computador](introduction/_static/image7.png)

Tudo isso ocorre automaticamente. Tudo o que você precisa fazer é criar um site da Web e implantar seu aplicativo nele, usando o Windows PowerShell, o Visual Studio ou o portal de gerenciamento do Azure.

Para obter um tutorial passo a passo rápido e fácil que mostra como criar um aplicativo Web no Visual Studio e implantá-lo em um site do Azure, consulte Introdução [ao Azure e ASP.net](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).

<a id="summary"></a>
## <a name="summary"></a>Resumo

Esta introdução forneceu uma lista de tópicos que o livro abrangerá, capturas de tela do aplicativo de exemplo e uma breve visão geral dos aplicativos Web em Azure App ambiente de nuvem do serviço. Uma das grandes vantagens do desenvolvimento de aplicativos no e na nuvem é que é fácil automatizar tarefas de desenvolvimento repetitivas, como criar um ambiente de teste e implantar seu código nele. Como fazer isso é o assunto do [próximo capítulo](automate-everything.md).

## <a name="resources"></a>Recursos

Para obter mais informações sobre os tópicos abordados neste capítulo, consulte os recursos a seguir.

Documentação:

- [Aplicativos Web no serviço Azure app](https://azure.microsoft.com/services/app-service/web/). Página do portal para a documentação do Azure sobre aplicativos Web.
- [Aplicativos Web, serviços de nuvem e VMs: quando usar o que?](https://azure.microsoft.com/documentation/articles/choose-web-site-cloud-service-vm/) WAWS, conforme mostrado neste capítulo, é apenas uma das três maneiras que você pode executar aplicativos Web no Azure. Este artigo explica as diferenças entre as três maneiras e fornece orientação sobre como escolher qual delas é a ideal para seu cenário. Como os sites da Web, os serviços de nuvem são um recurso de PaaS do Azure. As VMs são um recurso de IaaS. Para obter uma explicação de PaaS versus IaaS, consulte o capítulo [Opções de dados](data-storage-options.md#paasiaas) .

Vídeos:

- [Scott Guthrie começa na etapa 0 – o que é o Cloud OS do Azure?](https://azure.microsoft.com/documentation/videos/what-is-the-cloud-os-scottgu/)
- [Arquitetura de sites-com Stefan Schackow](https://azure.microsoft.com/documentation/videos/why-azure-web-sites-plus-architecture/).
- [Sites da Web do Azure internos com nir Mashkowski](https://channel9.msdn.com/Shows/Web+Camps+TV/Windows-Azure-Web-Sites-Internals-with-Nir-Mashkowski).

> [!div class="step-by-step"]
> [Próximo](automate-everything.md)
