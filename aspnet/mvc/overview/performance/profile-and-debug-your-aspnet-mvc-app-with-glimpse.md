---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: Criar um perfil e depurar seu aplicativo MVC do ASP.NET com a visão | Microsoft Docs
author: Rick-Anderson
description: A idéia é uma família de sucesso e crescente de pacotes NuGet de software livre que fornece informações detalhadas de desempenho, depuração e diagnóstico para ASP.NET a...
ms.author: riande
ms.date: 03/26/2015
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: d3689147a3bc3aa1f4180c377d2483a94bdd95a9
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457655"
---
# <a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a>Analisar e depurar seu aplicativo do ASP.NET MVC com Glimpse

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> A visão é uma família de sucesso e crescente de pacotes NuGet de software livre que fornece informações detalhadas de desempenho, depuração e diagnóstico para aplicativos ASP.NET. É trivial instalar, leve, extremamente rápido e exibir as principais métricas de desempenho na parte inferior de cada página. Ele permite que você faça uma busca detalhada em seu aplicativo quando precisar descobrir o que está acontecendo no servidor. A idéia fornece informações valiosas que recomendamos que você o use em todo o ciclo de desenvolvimento, incluindo o ambiente de teste do Azure. Embora o [Fiddler](http://www.telerik.com/fiddler) e as [ferramentas de desenvolvimento F-12](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) forneçam uma exibição do lado do cliente, a idéia fornece uma exibição detalhada do servidor. Este tutorial se concentrará no uso dos pacotes ASP.NET MVC e EF da visão, mas muitos outros pacotes estarão disponíveis. Quando possível, vou vincular aos documentos de [amostra](http://getglimpse.com/Docs/) apropriados que eu ajude a manter. A idéia é um projeto de software livre, você também pode contribuir para o código-fonte e os documentos.

- [Instalando a visão](#ig)
- [Habilitar a visão do localhost](#eg)
- [A guia linha do tempo](#Time)
- [Model binding](#mb)
- [Rotas](#route)
- [Usando a visão no Azure](#da)
- [Recursos adicionais](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a>Instalando a visão

Você pode instalar a visão do console do Gerenciador de pacotes NuGet ou do console **gerenciar pacotes NuGet** . Para esta demonstração, instalarei os pacotes Mvc5 e EF6:

![instalar a visão do NuGet Dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

Pesquisar por *idéia. EF*

![. EF da instalação do NuGet Dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

Selecionando **pacotes instalados**, você pode ver os módulos dependentes da visão instalados:

![Pacotes de amostra instalados de DLg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

Os comandos a seguir instalam os módulos MVC5 e EF6 do console do Gerenciador de pacotes:

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a>Habilitar a visão do localhost

Navegue até http://localhost:&lt;p classificar #&gt;/glimpse.axd e clique no botão <strong>Ativar visão</strong> .

![Página de visualização do axd](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

Se sua barra de favoritos for exibida, você poderá arrastar e soltar os botões de visão e adicioná-los como bookmarklets:

![IE com bookmarklets de visão](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

Agora você pode navegar no seu aplicativo e a **exibição de cabeçotes** (HUD) é mostrada na parte inferior da página.

![Página do Gerenciador de contatos com HUD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

A [página de visão de HUD](http://getglimpse.com/Docs/Heads-up-Display) detalha as informações de tempo mostradas acima. Os dados de desempenho discretos exibidos pelo HUD podem notificá-lo de um problema imediatamente-antes de chegar ao ciclo de teste. Clicar no &quot;g&quot; no canto inferior direito abre o painel de visão:

![Painel de visão](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

Na imagem acima, a [guia execução](http://getglimpse.com/Docs/Execution-Tab) é selecionada, que mostra os detalhes de tempo das ações e dos filtros no pipeline. Você pode ver meu [temporizador parar filtro de monitoramento](http://www.nuget.org/packages/StopWatch/) iniciar no estágio 6 do pipeline. Embora meu temporizador leve possa fornecer dados úteis de perfil/tempo, ele perde o tempo gasto na autorização e renderizando a exibição. Você pode ler sobre meu temporizador no [perfil e cronometrar seu aplicativo ASP.NET MVC até o Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx). A página [guias](http://getglimpse.com/Docs/Tabs) fornece links para informações detalhadas sobre cada guia.

<a id="Time"></a>
## <a name="the-timeline-tab"></a>A guia linha do tempo

Modifiquei o [tutorial do EF 6/MVC 5](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) pendente de Tom Dykstra com a seguinte alteração de código para o controlador de instrutores:

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

O código acima permite que eu transmita uma cadeia de caracteres de consulta (`eager`) para controlar o carregamento rápido ou explícito dos dados. Na imagem abaixo, o carregamento explícito é usado e a página de tempo mostra cada registro carregado no método de ação `Index`:

![carregamento explícito](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

No código a seguir, o adiantamento é especificado e cada registro é obtido depois que a exibição de `Index` é chamada:

![adiantado é especificado](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

Você pode focalizar um segmento de tempo para obter informações detalhadas de tempo:

![focalizar para ver o tempo detalhado](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a>Model binding

A [guia Associação de modelo](http://getglimpse.com/Docs/Model-Binding-Tab) fornece uma grande quantidade de informações para ajudá-lo a entender como as variáveis de formulário são ligadas e por que algumas delas não estão associadas como esperado. A imagem abaixo mostra o **?** , no qual você pode clicar para exibir a página de ajuda de visão desse recurso.

![exibição de associação de modelo de visão](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a>Rotas

 A guia mostrar rotas pode ajudá-lo a depurar e entender o roteamento. Na imagem abaixo, a rota do produto é selecionada (e mostra em verde, uma Convenção de visão). ![nome do produto selecionado](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) restrições de rota, as áreas e os tokens de dados também são exibidos. Consulte [visão de rotas](http://getglimpse.com/Docs/Routes-Tab) e [Roteamento de atributos no ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) para obter mais informações. 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a>Usando a visão no Azure

A política de segurança padrão de exibição só permite que os dados da amostra sejam exibidos do host local. Você pode alterar essa política de segurança para que possa exibir esses dados em um servidor remoto (como um aplicativo Web no Azure). Para ambientes de teste no Azure, adicione a marca realçada à parte inferior do arquivo *Web. config* para habilitar a visão:

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

Com essa alteração sozinha, qualquer usuário pode ver seus dados de sua amostra em um site remoto. Considere adicionar a marcação acima a um perfil de publicação para que ele seja implantado apenas quando você usar esse perfil de publicação (por exemplo, seu perfil de teste do Azure). Para restringir os dados da visão, adicionaremos a função `canViewGlimpseData` e apenas permitirá que os usuários nessa função exibam dados de visão.

Remova os comentários do arquivo *GlimpseSecurityPolicy.cs* e altere a chamada [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) de `Administrator` para a função `canViewGlimpseData`:

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> Segurança-os dados avançados fornecidos pela visão podem expor a segurança do seu aplicativo. A Microsoft não realizou uma auditoria de segurança de idéia para uso em aplicativos de produções.

Para obter informações sobre como adicionar funções, consulte o tutorial [implantar um aplicativo Web do ASP.NET MVC 5 com associação, OAuth e banco de dados SQL no Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) .

<a id="addRes"></a>
## <a name="additional-resources"></a>Recursos adicionais

- [Implantar um aplicativo Secure ASP.NET MVC 5 com associação, OAuth e banco de dados SQL no Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- [Visão](http://getglimpse.com/Docs/Configuration) da página configuração – documento na configuração de guias, política de tempo de execução, registro em log e muito mais.
