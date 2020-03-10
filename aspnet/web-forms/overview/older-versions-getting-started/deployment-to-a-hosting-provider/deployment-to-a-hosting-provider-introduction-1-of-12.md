---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
title: 'Implantando um aplicativo Web ASP.NET com SQL Server Compact usando o Visual Studio: Introdução-1 de 12 | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um projeto de aplicativo Web ASP.NET que inclui um banco de dados SQL Server Compact usando o Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: a2d7f33b-8c4a-4b48-9fb1-9139cf9b9878
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
msc.type: authoredcontent
ms.openlocfilehash: ea88da1e6d510f706fc7ca370cfa32974c1243f8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78525630"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-introduction---1-of-12"></a>Implantando um aplicativo Web ASP.NET com SQL Server Compact usando o Visual Studio: Introdução-1 de 12

por [Tom Dykstra](https://github.com/tdykstra)

[Baixar o projeto inicial](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta série de tutoriais mostra como implantar (publicar) um projeto de aplicativo Web ASP.NET que inclui um banco de dados SQL Server Compact usando o Visual Studio 2012 RC ou o Visual Studio Express 2012 RC para Web. Você também pode usar o Visual Studio 2010 se instalar a atualização de publicação na Web.
> 
> Para ver um tutorial que mostra os recursos de implantação introduzidos após a versão RC do Visual Studio 2012, mostra como implantar SQL Server edições diferentes de SQL Server Compact e mostra como implantar o Azure App aplicativos Web do serviço, consulte [implantação da Web do ASP.NET usando o Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).
> 
> Esses tutoriais orientam você pela implantação primeiro no IIS em seu computador de desenvolvimento local para teste e, em seguida, para um provedor de Hospedagem de terceiros. O aplicativo que você implantará usa um banco de dados de aplicativo e um banco de dados de associação do ASP.NET. Você começa a usar o SQL Server Compact e a implantação no SQL Server Compact, e os tutoriais posteriores mostram como implantar alterações no banco de dados e como migrar para o SQL Server.
> 
> Os tutoriais pressupõem que você saiba como trabalhar com o ASP.NET no Visual Studio. Se você não fizer isso, um bom lugar para começar é um [tutorial básico de Web Forms ASP.net](../tailspin-spyworks/tailspin-spyworks-part-1.md) ou um [tutorial ASP.NET MVC básico](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md).
> 
> Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, poderá lançá-las no fórum de [implantação do ASP.net](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment).

## <a name="overview"></a>Visão geral

Esses tutoriais orientam você pela implantação primeiro no IIS em seu computador de desenvolvimento local para teste e, em seguida, para um provedor de Hospedagem de terceiros. O aplicativo que você implantará usa um banco de dados de aplicativo e um banco de dados de associação do ASP.NET. Você começa a usar o SQL Server Compact e a implantação no SQL Server Compact, e os tutoriais posteriores mostram como implantar alterações no banco de dados e como migrar para o SQL Server.

O número de tutoriais – 11 em tudo mais uma página de solução de problemas – pode fazer com que o processo de implantação pareça assustador. Na verdade, os procedimentos básicos para implantar um site compõem uma parte relativamente pequena do conjunto de tutoriais. No entanto, em situações do mundo real, muitas vezes você precisa de informações sobre um aspecto extra pequeno, mas importante de implantação, por exemplo, definindo permissões de pasta no servidor de destino. Incluímos muitas dessas técnicas adicionais nos tutoriais, com a esperança de que os tutoriais não saiam de informações que possam impedi-lo de implantar com êxito um aplicativo real.

Os tutoriais são projetados para serem executados em sequência e cada parte se baseia na parte anterior. No entanto, você pode ignorar partes que não são relevantes para sua situação. (Ignorar partes pode exigir que você ajuste os procedimentos em Tutoriais posteriores.)

## <a name="intended-audience"></a>Público-alvo

Os tutoriais se destinam a desenvolvedores de ASP.NET que trabalham em pequenas organizações ou em outros ambientes em que:

- Um processo de integração contínua (builds e implantação automatizados) não é usado.
- O ambiente de produção é um provedor de Hospedagem de terceiros.
- Normalmente, uma pessoa preenche várias funções (a mesma pessoa desenvolve, testa e implanta).

Em ambientes corporativos, é mais comum implementar processos de integração contínua, e o ambiente de produção geralmente é hospedado pelos servidores próprios da empresa. Pessoas diferentes normalmente também executam funções diferentes. Para obter informações sobre a implantação corporativa, consulte [Implantando aplicativos Web em cenários empresariais](../../deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md).

Organizações de todos os tamanhos também podem implantar aplicativos Web no Azure, e a maioria dos procedimentos mostrados nesses tutoriais também se aplicam a aplicativos Web de Azure App serviços. Para obter uma introdução ao Azure, consulte [https://azure.microsoft.com](https://azure.microsoft.com).

## <a name="the-hosting-provider-shown-in-the-tutorials"></a>O provedor de hospedagem mostrado nos tutoriais

Os tutoriais orientam você pelo processo de configuração de uma conta com uma empresa de hospedagem e implantação do aplicativo no provedor de hospedagem. Uma empresa de hospedagem específica foi escolhida para que os tutoriais possam ilustrar a experiência completa de implantação em um site ativo. Cada empresa de hospedagem fornece recursos diferentes, e a experiência de implantação em seus servidores varia de certa forma; no entanto, o processo descrito neste tutorial é típico para o processo geral.

O provedor de hospedagem usado para este tutorial, Cytanium.com, é um dos muitos que estão disponíveis e seu uso neste tutorial não constitui um endosso ou recomendação.

## <a name="deploying-web-site-projects"></a>Implantando projetos de site

A Contoso University é um projeto de aplicativo Web do Visual Studio. A maioria dos métodos e ferramentas de implantação demonstrados neste tutorial não se aplicam a [projetos de site](https://msdn.microsoft.com/library/dd547590.aspx). Para obter informações sobre como implantar projetos de site, consulte [mapa de conteúdo da implantação do ASP.net](https://msdn.microsoft.com/library/bb386521.aspx#deployment_for_web_site_projects).

## <a name="deploying-aspnet-mvc-projects"></a>Implantando projetos do ASP.NET MVC

Para este tutorial, você implanta um projeto ASP.NET Web Forms, mas tudo o que você aprende como fazer também se aplica ao MVC do ASP.NET. Um projeto do MVC do Visual Studio é apenas outra forma de projeto de aplicativo Web. A única diferença é que, se você estiver implantando em um provedor de hospedagem que não dá suporte ao MVC ASP.NET ou à sua versão de destino, certifique-se de ter instalado o pacote NuGet apropriado ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0) ou [MVC 4](http://nuget.org/packages/aspnetmvc)) em seu projeto.

## <a name="programming-language"></a>Linguagem de programação

O aplicativo de exemplo C# usa, mas os tutoriais não exigem conhecimento C#do, e as técnicas de implantação mostradas pelos tutoriais não são específicas do idioma.

## <a name="troubleshooting-during-this-tutorial"></a>Solução de problemas durante este tutorial

Quando ocorrer um erro durante a implantação ou se o site implantado não for executado corretamente, as mensagens de erro nem sempre fornecerão uma solução. Para ajudá-lo com alguns cenários de problema comuns, uma [página de referência de solução de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md) está disponível. Se você receber uma mensagem de erro ou algo não funcionar enquanto percorre os tutoriais, certifique-se de verificar a página de solução de problemas.

## <a name="comments-welcome"></a>Comentários-boas-vindas

Os comentários sobre os tutoriais são bem-vindos e, quando o tutorial for atualizado, todos os esforços serão feitos para levar em conta as correções ou sugestões para melhorias fornecidas nos comentários do tutorial.

## <a name="prerequisites"></a>Prerequisites

Antes de começar, verifique se você tem o Windows 7 ou posterior e um dos seguintes produtos instalados em seu computador:

- [Visual Studio 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
- [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VWD2010SP1Pack)
- [Visual Studio 2012 RC ou Visual Studio Express 2012 RC para Web](https://go.microsoft.com/fwlink/?LinkId=240162)

Se você tiver o Visual Studio 2010 SP1 ou o Visual Web Developer Express 2010 SP1, instale os seguintes produtos também:

- [SDK do Azure para .net (VS 2010 SP1)](https://go.microsoft.com/fwlink/?LinkID=208120) (inclui a atualização de publicação na Web)
- [Microsoft Visual Studio 2010 SP1 Tools para SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCEVSTools)

Algum outro software é necessário para concluir o tutorial, mas você ainda não precisa tê-lo carregado. O tutorial irá orientá-lo pelas etapas para instalá-lo quando necessário.

## <a name="downloading-the-sample-application"></a>Baixando o aplicativo de exemplo

O aplicativo que você implantará é chamado de Contoso University e já foi criado para você. É uma versão simplificada de um site da Universidade, com base no aplicativo da Contoso University descrito nos tutoriais de [Entity Framework no site do ASP.net](https://asp.net/entity-framework/tutorials).

Quando tiver os pré-requisitos instalados, baixe o [aplicativo Web da Contoso University](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b). O arquivo *. zip* contém várias versões do projeto e um arquivo PDF que contém todos os 12 tutoriais. Para trabalhar nas etapas do tutorial, comece com ContosoUniversity-Begin. Para ver a aparência do projeto no final dos tutoriais, abra ContosoUniversity-end. Para ver a aparência do projeto antes da migração para o SQL Server completo no tutorial 10, abra ContosoUniversity-AfterTutorial09.

Para se preparar para trabalhar nas etapas do tutorial, salve ContosoUniversity-Begin em qualquer pasta usada para trabalhar com projetos do Visual Studio. Por padrão, esta é a seguinte pasta:

`C:\Users\<username>\Documents\Visual Studio 2012\Projects`

(Para as capturas de tela deste tutorial, a pasta do projeto está localizada no diretório raiz na unidade `C`:.)

Inicie o Visual Studio, abra o projeto e pressione CTRL-F5 para executá-lo.

[![Home_page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image2.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image1.png)

As páginas do site podem ser acessadas na barra de menus e permitem que você execute as seguintes funções:

- Exibir estatísticas de alunos (a página sobre).
- Exibir, editar, excluir e adicionar alunos.
- Exibir e editar cursos.
- Exibir e editar instrutores.
- Exibir e editar departamentos.

Veja a seguir as capturas de tela de algumas páginas representativas.

[![Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image4.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image3.png)

[![Add_Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image6.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image5.png)

## <a name="reviewing-application-features-that-affect-deployment"></a>Revisando recursos do aplicativo que afetam a implantação

Os seguintes recursos do aplicativo afetam a maneira como você o implanta ou o que você precisa fazer para implantá-lo. Cada um deles é explicado com mais detalhes nos seguintes tutoriais da série.

- A Contoso University usa um banco de dados de SQL Server Compact para armazenar aplicativos de aplicativo, como nomes de alunos e instrutores. O banco de dados contém uma combinação de dado de teste e dados de produção e, ao implantar na produção, você precisa excluir os dados de teste. Posteriormente na série de tutoriais, você migrará de SQL Server Compact para SQL Server.
- O aplicativo usa o sistema de associação ASP.NET, que armazena informações de conta de usuário em um banco de dados SQL Server Compact. O aplicativo define um usuário administrador que tem acesso a algumas informações restritas. Você precisa implantar o banco de dados de associação sem contas de teste, mas com uma conta de administrador.
- Como o banco de dados do aplicativo e o banco de dados de associação usam SQL Server Compact como o mecanismo de banco de dados, você precisa implantar o mecanismo de banco de dados no provedor de hospedagem, bem como os próprios bancos.
- O aplicativo usa provedores de Associação Universal do ASP.NET para que o sistema de associação possa armazenar seus dados em um banco de SQL Server Compact. O assembly que contém os provedores de Associação Universal deve ser implantado com o aplicativo.
- O aplicativo usa o Entity Framework 5,0 para acessar dados no banco de dado do aplicativo. O assembly que contém Entity Framework 5,0 deve ser implantado com o aplicativo.
- O aplicativo usa um utilitário de relatório e registro em log de erros de terceiros. Esse utilitário é fornecido em um assembly que deve ser implantado com o aplicativo.
- O utilitário de log de erros grava informações de erro em arquivos XML em uma pasta de arquivos. Você precisa certificar-se de que a conta que ASP.NET é executada no site implantado tem permissão de gravação para essa pasta, e você precisa excluir essa pasta da implantação. (Caso contrário, os dados de log de erros do ambiente de teste podem ser implantados em arquivos de log de erros de produção e/ou de produção podem ser excluídos.)
- O aplicativo inclui algumas configurações que devem ser alteradas no arquivo *Web. config* implantado dependendo do ambiente de destino (teste ou produção) e outras configurações que devem ser alteradas dependendo da configuração da compilação (depuração ou versão).
- A solução do Visual Studio inclui um projeto de biblioteca de classes. Somente o assembly que este projeto gera deve ser implantado, não o projeto em si.

Neste primeiro tutorial da série, você baixou o projeto de exemplo do Visual Studio e revisou os recursos do site que afetam a forma como você implanta o aplicativo. Nos tutoriais a seguir, você se prepara para a implantação Configurando algumas dessas coisas para serem tratadas automaticamente. Outros que você cuida manualmente.

> [!div class="step-by-step"]
> [Próximo](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
