---
uid: aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
title: Middleware OWIN no pipeline integrado do IIS | Microsoft Docs
author: Praburaj
description: Este artigo mostra como executar os componentes de middleware do OWIN (OMCs) no pipeline integrado do IIS e como definir o evento de pipeline em que um OMC é executado. Você deve...
ms.author: riande
ms.date: 11/07/2013
ms.assetid: d031c021-33c2-45a5-bf9f-98f8fa78c2ab
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
msc.type: authoredcontent
ms.openlocfilehash: 7d157fb6bd9e2ae9b55af41ef06c1eb5e6310ce1
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456706"
---
# <a name="owin-middleware-in-the-iis-integrated-pipeline"></a>Middleware OWIN no pipeline integrado do IIS

por [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://twitter.com/RickAndMSFT)

> Este artigo mostra como executar os componentes de middleware do OWIN (OMCs) no pipeline integrado do IIS e como definir o evento de pipeline em que um OMC é executado. Você deve examinar [uma visão geral do projeto Katana e da detecção da](an-overview-of-project-katana.md) [classe de inicialização do OWIN](owin-startup-class-detection.md) antes de ler este tutorial. Este tutorial foi escrito por Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), Chris Ross, Praburaj Thiagarajan e Howard Dierking ( [@howard\_Dierking](https://twitter.com/howard_dierking) ).

Embora os componentes de middleware [OWIN](an-overview-of-project-katana.md) (OMCs) sejam projetados principalmente para serem executados em um pipeline independente de servidor, é possível executar um OMC no pipeline integrado do IIS também ( ***não* há suporte para o modo clássico**). Um OMC pode ser feito para funcionar no pipeline integrado do IIS instalando o seguinte pacote do Package Manager Console (PMC):

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample1.cmd)]

Isso significa que todas as estruturas de aplicativo, mesmo aquelas que ainda não podem ser executadas fora do IIS e do System. Web, podem se beneficiar de componentes de middleware OWIN existentes. 

> [!NOTE]
> Todos os pacotes de `Microsoft.Owin.Security.*` fornecidos com o novo sistema de identidade no Visual Studio 2013 (por exemplo: cookies, conta da Microsoft, Google, Facebook, Twitter, [token de portador](http://self-issued.info/docs/draft-ietf-oauth-v2-bearer.html), OAuth, servidor de autorização, JWT, Azure Active Directory e serviços de Federação do Active Directory) são criados como OMCs e podem ser usados em cenários de hospedagem interna e hospedado pelo IIS.

## <a name="how-owin-middleware-executes-in-the-iis-integrated-pipeline"></a>Como o middleware OWIN é executado no pipeline integrado do IIS

Para aplicativos de console do OWIN, o pipeline de aplicativo criado usando a [configuração de inicialização](owin-startup-class-detection.md) é definido pela ordem em que os componentes são adicionados usando o método `IAppBuilder.Use`. Ou seja, o pipeline OWIN no tempo de execução do [Katana](an-overview-of-project-katana.md) processará OMCs na ordem em que foram registrados usando `IAppBuilder.Use`. No pipeline integrado do IIS, o pipeline de solicitação consiste em [HttpModules](https://msdn.microsoft.com/library/ms178468(v=vs.85).aspx) inscritos em um conjunto predefinido de eventos de pipeline, como [BeginRequest](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx), [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx), [AuthorizeRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx), etc.

Se compararmos um OMC com o de um [HttpModule](https://msdn.microsoft.com/library/zec9k340(v=vs.85).aspx) no mundo ASP.net, um OMC deverá ser registrado para o evento de pipeline predefinido correto. Por exemplo, o `MyModule` HttpModule será invocado quando uma solicitação chegar ao estágio [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) no pipeline:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample2.cs?highlight=10)]

Para que um OMC participe dessa mesma ordem de execução baseada em evento, o código de tempo de execução do [Katana](an-overview-of-project-katana.md) examina a [configuração de inicialização](owin-startup-class-detection.md) e assina cada um dos componentes de middleware em um evento de pipeline integrado. Por exemplo, o OMC e o código de registro a seguir permitem que você veja o registro de eventos padrão de componentes de middleware. (Para obter instruções mais detalhadas sobre como criar uma classe de inicialização OWIN, consulte [detecção de classe de inicialização do OWIN](owin-startup-class-detection.md).)

1. Crie um projeto de aplicativo Web vazio e nomeie-o **owin2**.
2. No console do Gerenciador de pacotes (PMC), execute o seguinte comando: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample3.cmd)]
3. Adicione um `OWIN Startup Class` e nomeie-o `Startup`. Substitua o código gerado pelo seguinte (as alterações são realçadas):  

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample4.cs?highlight=5-7,15-36)]
4. Pressione F5 para executar o aplicativo.

A configuração de inicialização configura um pipeline com três componentes de middleware, os dois primeiros exibindo informações de diagnóstico e o último respondendo a eventos (e também exibindo informações de diagnóstico). O método `PrintCurrentIntegratedPipelineStage` exibe o evento de pipeline integrado em que esse middleware é invocado e uma mensagem. As janelas de saída exibem o seguinte:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample5.cmd)]

O tempo de execução Katana mapeou cada um dos componentes de middleware OWIN para [PreExecuteRequestHandler](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx) por padrão, que corresponde ao evento de pipeline do IIS [PreRequestHandlerExecute](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx).

## <a name="stage-markers"></a>Marcadores de estágio

Você pode marcar OMCs para executar em estágios específicos do pipeline usando o método de extensão `IAppBuilder UseStageMarker()`. Para executar um conjunto de componentes de middleware durante um estágio específico, insira um marcador de estágio logo após o último componente ser definido durante o registro. Há regras em qual estágio do pipeline você pode executar o middleware e os componentes do pedido devem ser executados (as regras são explicadas posteriormente no tutorial). Adicione o método `UseStageMarker` ao código de `Configuration`, conforme mostrado abaixo:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample6.cs?highlight=13,19)]

A chamada `app.UseStageMarker(PipelineStage.Authenticate)` configura todos os componentes de middleware registrados anteriormente (nesse caso, nossos dois componentes de diagnóstico) para serem executados no estágio de autenticação do pipeline. O último componente de middleware (que exibe o diagnóstico e responde a solicitações) será executado no estágio de `ResolveCache` (o evento [ResolveRequestCache](https://msdn.microsoft.com/library/system.web.httpapplication.resolverequestcache.aspx) ).

Pressione F5 para executar o aplicativo. A janela saída mostra o seguinte:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample7.cmd)]

## <a name="stage-marker-rules"></a>Regras de marcador de estágio

Os componentes de middleware Owin (OMC) podem ser configurados para executar nos seguintes eventos de estágio de pipeline OWIN:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample8.cs)]

1. Por padrão, OMCs é executado no último evento (`PreHandlerExecute`). É por isso que nosso primeiro código de exemplo exibia "PreExecuteRequestHandler".
2. Você pode usar o método `app.UseStageMarker` para registrar um OMC a ser executado anteriormente, em qualquer estágio do pipeline OWIN listado na `PipelineStage` enum.
3. O pipeline do OWIN e o pipeline do IIS são ordenados, portanto, chamadas para `app.UseStageMarker` devem estar em ordem. Você não pode definir o manipulador de eventos para um evento que precede o último evento registrado com para `app.UseStageMarker`. Por exemplo, *depois* de chamar:

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample9.cmd)]

   chamadas para `app.UseStageMarker` passando `Authenticate` ou `PostAuthenticate` não serão respeitadas e nenhuma exceção será lançada. OMCs executado no estágio mais recente, que por padrão é `PreHandlerExecute`. Os marcadores de estágio são usados para fazer com que eles sejam executados anteriormente. Se você especificar marcadores de estágio fora de ordem, arredondamos para o marcador anterior. Em outras palavras, a adição de um marcador de estágio diz "executar não depois do estágio X". A execução do OMC no marcador de estágio mais anterior foi adicionado depois dele no pipeline OWIN.
4. O estágio mais antigo de chamadas para `app.UseStageMarker` WINS. Por exemplo, se você alternar a ordem de chamadas de `app.UseStageMarker` de nosso exemplo anterior:

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample10.cs?highlight=13,19)]

   A janela saída será exibida: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample11.cmd)]

   OMCs todos são executados no estágio `AuthenticateRequest`, porque o último OMC registrado com o evento `Authenticate` e o evento `Authenticate` precede todos os outros eventos.
