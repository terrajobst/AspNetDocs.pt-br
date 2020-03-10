---
uid: web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
title: Rastreamento no ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: Mostra como habilitar o rastreamento no ASP.NET Web API.
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 66a837e9-600b-4b72-97a9-19804231c64a
msc.legacyurl: /web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: a01acb649556d06ab9828ceab0fcbdf363bbc0d1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598549"
---
# <a name="tracing-in-aspnet-web-api-2"></a>Rastreamento no ASP.NET Web API 2

por [Mike Wasson](https://github.com/MikeWasson)

> Quando você está tentando depurar um aplicativo baseado na Web, não há nenhum substituto para um bom conjunto de logs de rastreamento. Este tutorial mostra como habilitar o rastreamento no ASP.NET Web API. Você pode usar esse recurso para rastrear o que a estrutura da API Web faz antes e depois de invocar seu controlador. Você também pode usá-lo para rastrear seu próprio código.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
>
> - [Visual studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) (também funciona com o visual Studio 2015)
> - API Web 2
> - [Microsoft. AspNet. WebApi. Tracing](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)

## <a name="enable-systemdiagnostics-tracing-in-web-api"></a>Habilitar o rastreamento de System. Diagnostics na API da Web

Primeiro, criaremos um novo projeto de aplicativo Web ASP.NET. No Visual Studio, no menu **arquivo** , selecione **novo** **projeto**de > . Em **modelos**, **Web**, selecione **aplicativo Web ASP.net**.

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

Escolha o modelo de projeto de API Web.

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

No menu **ferramentas** , selecione **Gerenciador de pacotes NuGet**e **pacote gerenciar console**.

Na janela do console do Gerenciador de pacotes, digite os comandos a seguir.

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

O primeiro comando instala o pacote de rastreamento de API Web mais recente. Ele também atualiza os pacotes principais da API Web. O segundo comando atualiza o pacote WebApi. Webhost para a versão mais recente.

> [!NOTE]
> Se você quiser direcionar uma versão específica da API Web, use o sinalizador-Version ao instalar o pacote de rastreamento.

Abra o arquivo WebApiConfig.cs na pasta iniciar do aplicativo\_. Adicione o código a seguir ao método **Register** .

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

Esse código adiciona a classe [SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) ao pipeline da API Web. A classe **SystemDiagnosticsTraceWriter** grava rastreamentos em [System. Diagnostics. Trace](https://msdn.microsoft.com/library/system.diagnostics.trace).

Para ver os rastreamentos, execute o aplicativo no depurador. No navegador, navegue até `/api/values`.

![](tracing-in-aspnet-web-api/_static/image5.png)

As instruções de rastreamento são gravadas na janela de saída no Visual Studio. (No menu **Exibir** , selecione **saída**).

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

Como o **SystemDiagnosticsTraceWriter** grava rastreamentos em **System. Diagnostics. Trace**, você pode registrar ouvintes de rastreamento adicionais; por exemplo, para gravar rastreamentos em um arquivo de log. Para obter mais informações sobre os gravadores de rastreamento, consulte o tópico [ouvintes de rastreamento](https://msdn.microsoft.com/library/4y5y10s7.aspx) no msdn.

### <a name="configuring-systemdiagnosticstracewriter"></a>Configurando o SystemDiagnosticsTraceWriter

O código a seguir mostra como configurar o gravador de rastreamento.

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

Há duas configurações que você pode controlar:

- IsVerbose: se false, cada rastreamento contém informações mínimas. Se for true, os rastreamentos incluem mais informações.
- MinimumLevel: define o nível de rastreamento mínimo. Os níveis de rastreamento, em ordem, são Debug, info, Warn, erro e fatal.

## <a name="adding-traces-to-your-web-api-application"></a>Adicionando rastreamentos ao seu aplicativo de API Web

Adicionar um gravador de rastreamento fornece acesso imediato aos rastreamentos criados pelo pipeline da API Web. Você também pode usar o gravador de rastreamento para rastrear seu próprio código:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

Para obter o gravador de rastreamento, chame **HttpConfiguration. Services. GetTraceWriter**. A partir de um controlador, esse método pode ser acessado por meio da propriedade **ApiController. Configuration** .

Para escrever um rastreamento, você pode chamar o método **ITraceWriter. Trace** diretamente, mas a classe [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) define alguns métodos de extensão que são mais amigáveis. Por exemplo, o método **info** mostrado acima cria um rastreamento com **informações**de nível de rastreamento.

## <a name="web-api-tracing-infrastructure"></a>Infraestrutura de rastreamento da API Web

Esta seção descreve como gravar um gravador de rastreamento personalizado para a API Web.

O pacote Microsoft. AspNet. WebApi. Tracing é criado sobre uma infraestrutura de rastreamento mais geral na API da Web. Em vez de usar o Microsoft. AspNet. WebApi. Tracing, você também pode conectar outra biblioteca de rastreamento/log, como [NLog](http://nlog-project.org/) ou [log4net](http://logging.apache.org/log4net/).

Para coletar rastreamentos, implemente a interface **ITraceWriter** . Este é um exemplo simples:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

O método **ITraceWriter. Trace** cria um rastreamento. O chamador especifica uma categoria e um nível de rastreamento. A categoria pode ser qualquer cadeia de caracteres definida pelo usuário. A implementação do **trace** deve fazer o seguinte:

1. Crie um novo **TraceRecord**. Inicialize-o com o nível de solicitação, categoria e rastreamento, conforme mostrado. Esses valores são fornecidos pelo chamador.
2. Invocar o delegado *traceaction* . Dentro desse delegado, espera-se que o chamador preencha o restante do **TraceRecord**.
3. Escreva o **TraceRecord**, usando qualquer técnica de log que desejar. O exemplo mostrado aqui simplesmente chama o **System. Diagnostics. Trace**.

## <a name="setting-the-trace-writer"></a>Configurando o gravador de rastreamento

Para habilitar o rastreamento, você deve configurar a API da Web para usar a implementação do **ITraceWriter** . Você faz isso por meio do objeto **HttpConfiguration** , conforme mostrado no código a seguir:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

Somente um gravador de rastreamento pode estar ativo. Por padrão, a API da Web define um &quot;rastreamento de&quot; não operacional que não faz nada. (O &quot;o rastreamento de&quot; não Operations existe para que o código de rastreamento não precise verificar se o gravador de rastreamento é **nulo** antes de gravar um rastreamento.)

## <a name="how-web-api-tracing-works"></a>Como o rastreamento da API Web funciona

O rastreamento na API Web usa um padrão de *fachada* : quando o rastreamento está habilitado, a API da Web encapsula várias partes do pipeline de solicitação com classes que executam chamadas de rastreamento.

Por exemplo, ao selecionar um controlador, o pipeline usa a interface **IHttpControllerSelector** . Com o rastreamento habilitado, o pipeline insere uma classe que implementa **IHttpControllerSelector** , mas chama a implementação real:

![O rastreamento de API da Web usa o padrão de fachada.](tracing-in-aspnet-web-api/_static/image8.png)

Os benefícios desse design incluem:

- Se você não adicionar um gravador de rastreamento, os componentes de rastreamento não serão instanciados e não terão impacto no desempenho.
- Se você substituir serviços padrão, como **IHttpControllerSelector** , por sua própria implementação personalizada, o rastreamento não será afetado, pois o rastreamento é feito pelo objeto wrapper.

Você também pode substituir toda a estrutura de rastreamento da API Web por sua própria estrutura personalizada, substituindo o serviço **ITraceManager** padrão:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

Implemente **ITraceManager. Initialize** para inicializar o sistema de rastreamento. Lembre-se de que isso substitui *toda* a estrutura de rastreamento, incluindo todo o código de rastreamento que é incorporado à API da Web.
