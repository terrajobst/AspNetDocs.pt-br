---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-cs
title: Opções de hospedagem ASP.NETC#() | Microsoft Docs
author: rick-anderson
description: Os aplicativos Web ASP.NET são normalmente projetados, criados e testados em um ambiente de desenvolvimento local e precisam ser implantados em um ambiente de produção o...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: 89a1d2bc-fdfd-4c5c-a3b0-49a08baaf63a
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-cs
msc.type: authoredcontent
ms.openlocfilehash: 2eafa750167d89fa996a442633e79dce3d5b85bd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78637168"
---
# <a name="aspnet-hosting-options-c"></a>Opções de hospedagem do ASP.NET (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar PDF](https://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial01_Basics_cs.pdf)

> Os aplicativos Web ASP.NET são normalmente projetados, criados e testados em um ambiente de desenvolvimento local e precisam ser implantados em um ambiente de produção quando estiverem prontos para o lançamento. Este tutorial fornece uma visão geral de alto nível do processo de implantação e serve como uma introdução a esta série de tutoriais.

## <a name="introduction"></a>Introdução

Normalmente, os aplicativos Web são projetados, criados e testados em um ambiente de desenvolvimento que é acessível somente aos programadores que trabalham no site. Depois que o aplicativo estiver pronto para ser liberado, ele será movido para um ambiente de produção em que o site pode ser acessado por qualquer pessoa na Internet. Esse processo de implantação apresenta vários desafios:

- Um ambiente de produção deve existir e ser configurado corretamente para que um aplicativo ASP.NET possa ser implantado; Além disso, o ambiente de produção deve ser mantido atualizado com os patches de segurança mais recentes.
- O conjunto correto de arquivos de marcação, arquivos de código e arquivos de suporte deve ser copiado do ambiente de desenvolvimento para o ambiente de produção. Para aplicativos controlados por dados, isso também pode exigir a cópia do esquema de banco de dados e/ou do.
- Pode haver diferenças de configuração entre os dois ambientes. A cadeia de conexão do banco de dados ou o servidor de email usado no ambiente de desenvolvimento provavelmente será diferente do ambiente de produção. E o que é mais, o comportamento do aplicativo pode depender do ambiente. Por exemplo, quando ocorre um erro no desenvolvimento, os detalhes do erro podem ser exibidos na tela, mas quando ocorre um erro na produção, uma página de erro amigável deve ser exibida e os detalhes do erro são enviados por email aos desenvolvedores.

Para eliminarr o primeiro desafio – Configurando e mantendo um ambiente de produção, muitos indivíduos e empresas terceirizam seus ambientes de produção para *provedores de hospedagem na Web*. Um provedor de hospedagem na Web é uma empresa que gerencia o ambiente de produção em seu nome. Há inúmeros provedores de host da Web, cada um com preços e níveis de serviço variados; consulte a seção "Localizando um provedor de host da Web" para obter dicas sobre como localizar um provedor de serviços.

Este é o primeiro de uma série de tutoriais que examinam as etapas envolvidas na implantação de um aplicativo Web ASP.NET em um ambiente de produção gerenciado por um provedor de host da Web. Ao longo desses tutoriais, examinaremos:

- Quais arquivos precisam ser implantados no provedor de host da Web.
- Ferramentas para simplificar o processo de implantação.
- Como implantar um banco de dados.
- Dicas para implantar um banco de dados que usa [o provedor de associações e funções baseadas em SQL](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs.md), juntamente com maneiras de imitar a ferramenta de administração do site em um ambiente de produção.
- Estratégias para atualização tranqüila do banco de dados em produção com as alterações feitas durante o desenvolvimento.
- Técnicas para registrar em log erros que ocorrem na produção e maneiras de notificar os desenvolvedores quando ocorrer um erro.

Esses tutoriais são voltados para serem concisos e fornecem instruções passo a passo com muitas capturas de tela para orientá-lo no processo visualmente. Este tutorial do inaugural fornece uma visão geral do processo de implantação do ASP.NET e conselhos sobre como encontrar um provedor de hospedagem na Web. Vamos começar!

## <a name="an-overview-of-the-aspnet-deployment-process"></a>Uma visão geral do processo de implantação do ASP.NET

Resumindo, implantar um aplicativo ASP.NET envolve as três etapas a seguir:

1. Configure o aplicativo Web, o servidor Web e o banco de dados no ambiente de produção.
2. Sincronize as páginas ASP.NET, os arquivos de código, os assemblies na pasta `Bin` e os arquivos de suporte relacionados a HTML, como arquivos CSS e JavaScript.
3. Sincronize o esquema de banco de dados e/ou o Data.

As informações de configuração de um aplicativo Web normalmente estão localizadas no arquivo de `Web.config` e incluem cadeias de conexão de banco de dados, critérios de tratamento de erros, regras de regravação de URL e informações do servidor de email. Muitas vezes, essas informações são diferentes para um aplicativo em desenvolvimento versus o mesmo aplicativo em produção. Por exemplo, ao desenvolver um aplicativo, é melhor usar um banco de dados de desenvolvimento para que você não esteja testando no banco de dados de produção. Como resultado, as cadeias de conexão de banco de dados normalmente são diferentes entre aplicativos de desenvolvimento e produção. Devido a essas diferenças, parte da implantação envolve fazer alterações nas informações de configuração do aplicativo Web.

Além das alterações de configuração do aplicativo Web, a etapa 1 também pode envolver a configuração do servidor Web e do banco de dados. Por exemplo, se uma página ASP.NET criar ou excluir arquivos de um diretório no servidor Web, o servidor Web precisará ser configurado para permitir essas modificações no sistema de arquivos. Da mesma forma, pode haver permissões ou configurações de autenticação que precisam ser feitas no banco de dados.

A etapa 2 envolve a sincronização do conjunto de páginas ASP.NET essenciais e arquivos de suporte entre os ambientes de desenvolvimento e produção. O conjunto específico de arquivos relacionados ao ASP.NET que precisam ser sincronizados entre os dois ambientes depende do tipo de projeto criado no Visual Studio e é a discussão no próximo tutorial, [*determinando quais arquivos precisam ser implantados*](determining-what-files-need-to-be-deployed-cs.md). O terceiro e o quarto tutorial – [*implantando seu site usando o FTP*](deploying-your-site-using-an-ftp-client-cs.md) e [*implantando seu site usando o Visual Studio*](deploying-your-site-using-visual-studio-cs.md) -examine diferentes ferramentas e técnicas para sincronizar esses arquivos.

Durante a criação de aplicativos controlados por dados, normalmente há dois bancos de dado sendo usados: um para desenvolvimento e outro na produção. Durante o desenvolvimento, o esquema do banco de dados de desenvolvimento pode ser modificado para incluir novas tabelas, colunas, procedimentos armazenados e gatilhos, ou pode ser modificado para remover ou renomear objetos de banco de dados existentes. Entre o momento em que essas alterações são feitas e a hora em que o aplicativo é implantado na produção, os bancos de dados de desenvolvimento e produção estão fora de sincronia. Essa assincronia precisa ser corrigida durante o processo de implantação. Esses desafios serão examinados em Tutoriais futuros.

## <a name="finding-a-web-host-provider"></a>Localizando um provedor de host da Web

Os aplicativos ASP.NET podem ser implantados em qualquer servidor Web que tenha o .NET Framework e o Serviços de Informações da Internet (IIS) instalados. Você pode hospedar um site do seu computador pessoal, supondo que você tenha uma conexão de banda larga com a Internet e saiba como configurar seu roteador para permitir solicitações de entrada da Web. Você também pode hospedar um site de um computador em uma intranet, pois muitas empresas fazem. No entanto, o foco desses tutoriais é hospedar seu site com um provedor de host da Web.

> [!NOTE]
> O [IIS](https://www.iis.net/) é o servidor Web de nível empresarial da Microsoft. Ele é fornecido com as edições não-Home do Windows, como o Windows Server 2008 e determinadas edições do Windows Vista. Você não precisa instalar o IIS para atender aos aplicativos ASP.NET em um ambiente de desenvolvimento, pois o Visual Studio inclui o servidor Web de desenvolvimento do ASP.NET. No entanto, o servidor Web de desenvolvimento ASP.NET só aceita conexões locais e, portanto, não pode ser usado em um ambiente de produção.

Antes de implantar seu site em um provedor de host da Web, primeiro você deve decidir com qual empresa fazer negócios. Há inúmeras empresas de hospedagem na Web no Marketplace; uma pesquisa por "empresa de hospedagem na Web" retorna mais de 5 milhões resultados. Como você encontra o que é certo para você? Seu mecanismo de pesquisa favorito é um bom ponto de partida, assim como os sites como [TopHosts](http://www.tophosts.com/) e [HostCritique](http://www.hostcritique.net/), que comparam e contrastam vários serviços de hospedagem. Também recomendo que você solicite seus colegas e cotrabalhos por qualquer recomendação; Você também pode solicitar recomendações no fórum de [hospedagem aberta](https://forums.asp.net/158.aspx) aqui nos fóruns do [ASP.net](https://forums.asp.net/).

As empresas de hospedagem na Web normalmente oferecem planos de hospedagem compartilhados e planos de hospedagem dedicados. Com a hospedagem compartilhada, um único servidor Web hospeda dezenas, se não centenas de sites diferentes. Com a hospedagem dedicada, você concede um computador da empresa que atende apenas ao seu site e ao seu site. Um plano de hospedagem compartilhado pode incluir suporte para páginas ASP.NET, a capacidade de trabalhar com bancos de dados do Microsoft Access, 5 GB de espaço em disco e 100 GB de tráfego de largura de banda mensal por $9.95 por mês. Outro plano de hospedagem compartilhado pode incluir suporte para páginas ASP.NET, acesso ao servidor de banco de dados Microsoft SQL Server 2008, 10 GB de espaço em disco e 250 GB de tráfego de largura de banda mensal para $19.95 por mês. Os planos de hospedagem dedicados geralmente são muito mais caros, custando várias centenas de dólares por mês, mas oferecem melhor desempenho e mais controle do que as opções de hospedagem compartilhada. O plano escolhido dependerá do seu orçamento, da quantidade de tráfego que seu site recebe e dos recursos que você prevê que precisará.

Duas considerações importantes ao escolher um provedor de host da Web são serviço de atendimento ao cliente e qualidade de serviço. Se você tiver uma pergunta ou um problema de configuração, quanto tempo levará do envio do problema para o Helpdesk do host da Web até que você obtenha uma resposta? Quão confiáveis são os serviços da empresa? Eles frequentemente têm interrupções no banco de dados? Com que frequência seu servidor de email fica offline? Você sempre pode pedir para uma empresa fornecer detalhes sobre seu tempo de atividade e consultar sua política de atendimento ao cliente, mas uma maneira mais certeiras é solicitar os comentários dos clientes atuais e antigos, que podem ser feitos por meio de fóruns online, grupos de notícias e listservs de email .

> [!NOTE]
> Algumas empresas de hospedagem na Web concentram seus negócios em uma determinada pilha de tecnologia, como .NET ou [lâmpada](http://en.wikipedia.org/wiki/LAMP_stack) (**L** inux **, Pache,** **M** ySql e **P** HP), portanto, certifique-se de que a empresa selecionada hospeda aplicativos ASP.net. Verifique também se eles dão suporte à versão do ASP.NET que você está usando para criar seu aplicativo. E se você estiver criando um aplicativo controlado por dados, certifique-se de que o host da Web ofereça o mesmo servidor de banco de dado e versão que você está usando.

## <a name="summary"></a>Resumo

Os aplicativos Web ASP.NET são normalmente projetados, criados e testados em um ambiente de desenvolvimento local. Quando uma versão estiver pronta para o lançamento, ela será movida para um ambiente de produção. Embora seja possível hospedar sites ASP.NET em seu computador pessoal ou em servidores de sua empresa, muitas empresas e indivíduos optam por terceirizar sua hospedagem para um provedor de host da Web.

Esta série de tutoriais examina as etapas para implantar um aplicativo ASP.NET em um provedor de host da Web, explorando desafios comuns. Este tutorial ofereceu uma visão geral de alto nível do processo de implantação do ASP.NET e deu dicas para encontrar um provedor de host da Web adequado. O próximo tutorial examina quais arquivos relacionados ao ASP.NET precisam ser copiados para o ambiente de produção ao implantar seu site.

Boa programação!

### <a name="special-thanks-to"></a>Agradecimentos especiais a...

Esta série de tutoriais foi revisada por muitos revisores úteis. O revisor de Lead para este tutorial foi Teresa Murphy. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Próximo](determining-what-files-need-to-be-deployed-cs.md)
