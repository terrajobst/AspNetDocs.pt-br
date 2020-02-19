---
uid: web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
title: Usando métodos assíncronos no ASP.NET 4,5 | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensinará as noções básicas da criação de um aplicativo ASP.NET Web Forms assíncrono usando o Visual Studio Express 2012 para Web, que é uma versão gratuita...
ms.author: riande
ms.date: 01/02/2019
ms.assetid: a585c9a2-7c8e-478b-9706-90f3739c50d1
msc.legacyurl: /web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: 7abc3d7acc60d7d868958f2a313bc408f96c95a4
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457564"
---
# <a name="using-asynchronous-methods-in-aspnet-45"></a>Uso de métodos assíncronos no ASP.NET 4.5

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> Este tutorial ensinará as noções básicas da criação de um aplicativo ASP.NET Web Forms assíncrono usando o [Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/11), que é uma versão gratuita do Microsoft Visual Studio. Você também pode usar o [Visual Studio 2012](https://www.microsoft.com/visualstudio/11). As seções a seguir estão incluídas neste tutorial.
> 
> - [Como as solicitações são processadas pelo pool de threads](#HowRequestsProcessedByTP)
> - [Escolhendo métodos síncronos ou assíncronos](#ChoosingSyncVasync)
> - [O aplicativo de exemplo](#SampleApp)
> - [A página síncrona utensílios](#GizmosSynch)
> - [Criando uma página utensílios assíncrona](#CreatingAsynchGizmos)
> - [Executando várias operações em paralelo](#Parallel)
> - [Usando um token de cancelamento](#CancelToken)
> - [Configuração do servidor para chamadas de serviço Web de alta simultaneidade/latência](#ServerConfig)
> 
> Um exemplo completo é fornecido para este tutorial em  
> [https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET/) no site do [GitHub](https://github.com/) .

As páginas da Web do ASP.NET 4,5 em combinação o [.net 4,5](https://msdn.microsoft.com/library/w0x726c2(VS.110).aspx) permite que você registre métodos assíncronos que retornam um objeto do tipo [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). O .NET Framework 4 introduziu um conceito de programação assíncrona referido como uma [tarefa](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) e ASP.NET 4,5 dá suporte à [tarefa](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). As tarefas são representadas pelo tipo de **tarefa** e tipos relacionados no namespace [System. Threading. Tasks](https://msdn.microsoft.com/library/system.threading.tasks.aspx) . O .NET Framework 4,5 se baseia nesse suporte assíncrono com as palavras-chave [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) e [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) que tornam o trabalho com objetos de [tarefa](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) muito menos complexo do que as abordagens assíncronas anteriores. A palavra-chave [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) é uma abreviação sintática para indicar que uma parte do código deve esperar de forma assíncrona em alguma parte do código. A palavra-chave [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) representa uma dica que você pode usar para marcar métodos como métodos assíncronos baseados em tarefas. A combinação de **Await**, **Async**e **Task** Object torna muito mais fácil escrever código assíncrono no .NET 4,5. O novo modelo para métodos assíncronos é chamado de *padrão assíncrono baseado em tarefa* (**Tap**). Este tutorial pressupõe que você tenha alguma familiaridade com o programa assíncrono usando as palavras-chave [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) e [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) e o namespace da [tarefa](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) .

Para obter mais informações sobre o uso de palavras-chave [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) e [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) e o namespace da [tarefa](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) , consulte as referências a seguir.

- [White Paper: assincronia no .NET](https://go.microsoft.com/fwlink/?LinkId=204844)
- [Perguntas frequentes sobre Async/Await](https://blogs.msdn.com/b/pfxteam/archive/2012/04/12/10293335.aspx)
- [Programação assíncrona do Visual Studio](https://msdn.microsoft.com/vstudio/gg316360)

## <a id="HowRequestsProcessedByTP"></a>Como as solicitações são processadas pelo pool de threads

No servidor Web, a .NET Framework mantém um pool de threads que são usados para atender a solicitações ASP.NET. Quando chega uma solicitação, um thread do pool é expedido para processar essa solicitação. Se a solicitação for processada de forma síncrona, o thread que processa a solicitação estará ocupado enquanto a solicitação estiver sendo processada e esse thread não poderá atender a outra solicitação.   
  
Isso pode não ser um problema, pois o pool de threads pode ser criado grande o suficiente para acomodar muitos threads ocupados. No entanto, o número de threads no pool de threads é limitado (o máximo padrão para o .NET 4,5 é 5.000). Em aplicativos grandes com alta simultaneidade de solicitações de execução longa, todos os threads disponíveis podem estar ocupados. Essa condição é conhecida como privação de thread. Quando essa condição é atingida, o servidor Web enfileira as solicitações. Se a fila de solicitações ficar cheia, o servidor Web rejeitará as solicitações com um status HTTP 503 (servidor muito ocupado). O pool de threads do CLR tem limitações sobre novas injeções de thread. Se a simultaneidade for intermitente (ou seja, o seu site pode, repentinamente, obter um grande número de solicitações) e todos os threads de solicitação disponíveis estiverem ocupados devido a chamadas de back-end com alta latência, a taxa de injeção de thread limitada poderá fazer com que seu aplicativo responda muito mal. Além disso, cada novo thread adicionado ao pool de threads tem sobrecarga (como 1 MB de memória de pilha). Um aplicativo Web que usa métodos síncronos para atender às chamadas de alta latência em que o pool de threads cresce para 4,5 o máximo de 5, 000 que os threads consumiram aproximadamente 5 GB de memória do que um aplicativo capaz de atender às mesmas solicitações usando métodos assíncronos e apenas 50 threads. Quando você estiver fazendo um trabalho assíncrono, nem sempre está usando um thread. Por exemplo, quando você faz uma solicitação de serviço Web assíncrona, o ASP.NET não usará nenhum thread entre a chamada de método **Async** e o **Await**. Usar o pool de threads para atender a solicitações com alta latência pode levar a uma grande quantidade de memória e baixa utilização do hardware do servidor.

## <a name="processing-asynchronous-requests"></a>Processando solicitações assíncronas

Em aplicativos Web que veem um grande número de solicitações simultâneas na inicialização ou que têm uma carga de intermitência (em que a simultaneidade aumenta repentinamente), fazer chamadas de serviço Web assíncrona aumentará a capacidade de resposta do seu aplicativo. Uma solicitação assíncrona leva a mesma quantidade de tempo para ser processada como uma solicitação síncrona. Por exemplo, se uma solicitação fizer uma chamada de serviço Web que exija dois segundos para ser concluída, a solicitação levará dois segundos, independentemente de ser executada de forma síncrona ou assíncrona. No entanto, durante uma chamada assíncrona, um thread não é impedido de responder a outras solicitações enquanto aguarda a conclusão da primeira solicitação. Portanto, as solicitações assíncronas impedem o enfileiramento de solicitações e o crescimento do pool de threads quando há muitas solicitações simultâneas que chamam operações de execução longa.

## <a id="ChoosingSyncVasync"></a>Escolhendo métodos síncronos ou assíncronos

Esta seção lista as diretrizes para quando usar métodos síncronos ou assíncronos. Essas são apenas diretrizes; Examine cada aplicativo individualmente para determinar se os métodos assíncronos ajudam com o desempenho.

Em geral, use métodos síncronos para as seguintes condições:

- As operações são simples ou de execução curta.
- A simplicidade é mais importante do que a eficiência.
- As operações são basicamente operações de CPU em vez de operações que envolvem sobrecarga de disco ou de rede extensiva. O uso de métodos assíncronos em operações associadas à CPU não fornece benefícios e resulta em mais sobrecarga.

Em geral, use métodos assíncronos para as seguintes condições:

- Você está chamando serviços que podem ser consumidos por meio de métodos assíncronos e está usando o .NET 4,5 ou superior.
- As operações são vinculadas à rede ou de e/s em vez de ligadas à CPU.
- O paralelismo é mais importante do que a simplicidade do código.
- Você deseja fornecer um mecanismo que permite aos usuários cancelar uma solicitação de execução longa.
- Quando o benefício de alternar threads supera o custo da alternância de contexto. Em geral, você deve tornar um método assíncrono se o método síncrono bloquear o thread de solicitação ASP.NET enquanto não estiver funcionando. Ao fazer a chamada assíncrona, o thread de solicitação ASP.NET não é bloqueado sem nenhum trabalho enquanto aguarda a conclusão da solicitação de serviço Web.
- Os testes mostram que as operações de bloqueio são um afunilamento no desempenho do site e que o IIS pode atender a mais solicitações usando métodos assíncronos para essas chamadas de bloqueio.

  O exemplo para download mostra como usar métodos assíncronos com eficiência. O exemplo fornecido foi projetado para fornecer uma demonstração simples da programação assíncrona no ASP.NET 4,5. O exemplo não pretende ser uma arquitetura de referência para a programação assíncrona no ASP.NET. O programa de exemplo chama [ASP.NET Web API](../../../web-api/index.md) métodos que, por sua vez, chamam [Task. Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) para simular chamadas de serviço Web de longa execução. A maioria dos aplicativos de produção não mostrará esses benefícios óbvios ao uso de métodos assíncronos.   
  
Alguns aplicativos exigem que todos os métodos sejam assíncronos. Frequentemente, converter alguns métodos síncronos em métodos assíncronos fornece o melhor aumento de eficiência para a quantidade de trabalho necessária.

## <a id="SampleApp"></a>O aplicativo de exemplo

Você pode baixar o aplicativo de exemplo de [https://github.com/RickAndMSFT/Async-ASP.NET](https://github.com/RickAndMSFT/Async-ASP.NET) no site do [GitHub](https://github.com/) . O repositório consiste em três projetos:

- *WebAppAsync*: o projeto de Web Forms de ASP.NET que consome o serviço de API Web **WebAPIpwg** . A maior parte do código para este tutorial é deste projeto.
- *WebAPIpgw*: o projeto de API Web do ASP.NET MVC 4 que implementa os controladores de `Products, Gizmos and Widgets`. Ele fornece os dados para o projeto *WebAppAsync* e o projeto *Mvc4Async* .
- *Mvc4Async*: o projeto ASP.NET MVC 4 que contém o código usado em outro tutorial. Ele faz chamadas à API Web para o serviço **WebAPIpwg** .

## <a id="GizmosSynch"></a>A página síncrona utensílios

 O código a seguir mostra o `Page_Load` método síncrono que é usado para exibir uma lista de utensílios. (Para este artigo, um Gizmo é um dispositivo mecânico fictício.) 

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample1.cs)]

O código a seguir mostra o método `GetGizmos` do serviço Gizmo.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample2.cs)]

O método `GizmoService GetGizmos` passa um URI para um serviço HTTP ASP.NET Web API que retorna uma lista de dados utensílios. O projeto *WebAPIpgw* contém a implementação do `gizmos, widget` de API Web e os controladores de `product`.  
A imagem a seguir mostra a página utensílios do projeto de exemplo.

![Utensílios](using-asynchronous-methods-in-aspnet-45/_static/image1.png)

## <a id="CreatingAsynchGizmos"></a>Criando uma página utensílios assíncrona

O exemplo usa as novas palavras-chave [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) e [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) (disponíveis no .NET 4,5 e no Visual Studio 2012) para permitir que o compilador seja responsável por manter as transformações complicadas necessárias para a programação assíncrona. O compilador permite escrever código usando as construções C#de fluxo de controle síncrono do e o compilador aplica automaticamente as transformações necessárias para usar retornos de chamada para evitar threads de bloqueio.

As páginas assíncronas ASP.NET devem incluir a diretiva [Page](https://msdn.microsoft.com/library/ydy4x04a.aspx) com o atributo `Async` definido como "true". O código a seguir mostra a diretiva [Page](https://msdn.microsoft.com/library/ydy4x04a.aspx) com o atributo `Async` definido como "true" para a página *GizmosAsync. aspx* .

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample3.aspx?highlight=1)]

O código a seguir mostra o método `Gizmos` síncrono `Page_Load` e a `GizmosAsync` página assíncrona. Se seu navegador der suporte ao [elemento HTML 5 &lt;mark&gt;](http://www.w3.org/wiki/HTML/Elements/mark), você verá as alterações em `GizmosAsync` em realce amarelo.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample4.cs)]

A versão assíncrona:

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample5.cs?highlight=3,6-7,9,11)]

 As seguintes alterações foram aplicadas para permitir que a página de `GizmosAsync` seja assíncrona.

- A diretiva de [página](https://msdn.microsoft.com/library/ydy4x04a.aspx) deve ter o atributo `Async` definido como "true".
- O método `RegisterAsyncTask` é usado para registrar uma tarefa assíncrona que contém o código que é executado de forma assíncrona.
- O novo método `GetGizmosSvcAsync` é marcado com a palavra-chave [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) , que informa ao compilador para gerar retornos de chamada para partes do corpo e para criar automaticamente uma `Task` retornada.
- &quot;Async&quot; foi acrescentado ao nome do método assíncrono. Acrescentar "Async" não é necessário, mas é a Convenção ao escrever métodos assíncronos.
- O tipo de retorno do novo método `GetGizmosSvcAsync` é `Task`. O tipo de retorno de `Task` representa o trabalho em andamento e fornece chamadores do método com um identificador pelo qual aguardar a conclusão da operação assíncrona.
- A palavra-chave [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) foi aplicada à chamada do serviço Web.
- A API do serviço Web assíncrono foi chamada (`GetGizmosAsync`).

Dentro do corpo do método de `GetGizmosSvcAsync`, outro método assíncrono, `GetGizmosAsync` é chamado. `GetGizmosAsync` retorna imediatamente uma `Task<List<Gizmo>>` que eventualmente será concluída quando os dados estiverem disponíveis. Como você não deseja fazer mais nada até ter os dados de Gizmo, o código aguarda a tarefa (usando a palavra-chave **Await** ). Você pode usar a palavra-chave **Await** somente em métodos anotados com a palavra-chave **Async** .

A palavra-chave **Await** não bloqueia o thread até que a tarefa seja concluída. Ele se inscreve no restante do método como um retorno de chamada na tarefa e retorna imediatamente. Quando a tarefa esperada finalmente for concluída, ela invocará esse retorno de chamada e, portanto, retomará a execução do método imediatamente onde parou. Para obter mais informações sobre como usar as palavras-chave [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) e [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) e o namespace da [tarefa](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) , consulte as [referências assíncronas](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/async).

O código a seguir mostra os métodos `GetGizmos` e `GetGizmosAsync`.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample6.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample7.cs?highlight=1,4-8)]

 As alterações assíncronas são semelhantes às feitas no **GizmosAsync** acima. 

- A assinatura do método foi anotada com a palavra-chave [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) , o tipo de retorno foi alterado para `Task<List<Gizmo>>`e *Async* foi anexada ao nome do método.
- A classe assíncrona [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) é usada em vez da classe [WebClient](https://msdn.microsoft.com/library/system.net.webclient.aspx) síncrona.
- A palavra-chave [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) foi aplicada ao método assíncrono[getasync](https://msdn.microsoft.com/library/hh158944(VS.110).aspx) [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx).

A imagem a seguir mostra a exibição assíncrona de Gizmo.

![async](using-asynchronous-methods-in-aspnet-45/_static/image2.png)

A apresentação dos navegadores dos dados do utensílios é idêntica à exibição criada pela chamada síncrona. A única diferença é que a versão assíncrona pode ser mais eficaz sob cargas pesadas.

## <a name="registerasynctask-notes"></a>Observações do RegisterAsyncTask

Os métodos conectados com `RegisterAsyncTask` serão executados imediatamente após [PreRender](https://msdn.microsoft.com/library/ms178472.aspx). Você também pode usar eventos de página void Async diretamente, conforme mostrado no código a seguir:

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample8.cs)]

A desvantagem dos eventos Async void é que os desenvolvedores não têm mais controle total sobre quando os eventos são executados. Por exemplo, se um. aspx e um. O mestre define `Page_Load` eventos e um ou ambos são assíncronos, a ordem de execução não pode ser garantida. A mesma ordem de indeterminiate para manipuladores que não são de evento (como `async void Button_Click`) se aplica. Para a maioria dos desenvolvedores, isso deve ser aceitável, mas aqueles que precisam de controle total sobre a ordem de execução só devem usar APIs como `RegisterAsyncTask` que consomem métodos que retornam um objeto de tarefa.

## <a id="Parallel"></a>Executando várias operações em paralelo

Os métodos assíncronos têm uma vantagem significativa sobre métodos síncronos quando uma ação deve executar várias operações independentes. No exemplo fornecido, a página síncrona *PWG. aspx*(para produtos, widgets e utensílios) exibe os resultados de três chamadas de serviço Web para obter uma lista de produtos, widgets e utensílios. O projeto de [ASP.NET Web API](../../../web-api/index.md) que fornece esses serviços usa [Task. Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) para simular latência ou chamadas de rede lentas. Quando o atraso é definido como 500 milissegundos, a página assíncrona *PWGasync. aspx* leva um pouco mais de 500 milissegundos para ser concluída enquanto a versão síncrona `PWG` leva mais de 1.500 milissegundos. A página *PWG. aspx* síncrona é mostrada no código a seguir.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample9.cs)]

O código de `PWGasync` assíncrono por trás é mostrado abaixo.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample10.cs?highlight=5,11,21)]

A imagem a seguir mostra a exibição retornada da página assíncrona *PWGasync. aspx* .

![](using-asynchronous-methods-in-aspnet-45/_static/image3.png)

## <a id="CancelToken"></a>Usando um token de cancelamento

Os métodos assíncronos que retornam `Task`são canceláveis, ou seja, eles usam um parâmetro [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx) quando um é fornecido com o atributo `AsyncTimeout` da diretiva [Page](https://msdn.microsoft.com/library/ydy4x04a.aspx) . O código a seguir mostra a página *GizmosCancelAsync. aspx* com um tempo limite de em segundo.

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample11.aspx?highlight=1)]

O código a seguir mostra o arquivo *GizmosCancelAsync.aspx.cs* .

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample12.cs?highlight=6,9)]

No aplicativo de exemplo fornecido, a seleção do link *GizmosCancelAsync* chama a página *GizmosCancelAsync. aspx* e demonstra o cancelamento (por tempo limite) da chamada assíncrona. Como o tempo de atraso está dentro de um intervalo aleatório, talvez seja necessário atualizar a página duas vezes para obter a mensagem de erro de tempo limite.

## <a id="ServerConfig"></a>Configuração do servidor para chamadas de serviço Web de alta simultaneidade/latência

Para obter os benefícios de um aplicativo Web assíncrono, talvez seja necessário fazer algumas alterações na configuração padrão do servidor. Tenha em mente o seguinte ao configurar e testar o teste do seu aplicativo Web assíncrono.

- O Windows 7, o Windows Vista, a janela 8 e todos os sistemas operacionais Windows Client têm um máximo de 10 solicitações simultâneas. Você precisará de um sistema operacional Windows Server para ver os benefícios dos métodos assíncronos sob alta carga.
- Registre o .NET 4,5 com o IIS em um prompt de comando elevado usando o seguinte comando:  
  %windir%\Microsoft.NET\Framework64 \v4.0.30319\aspnet\_regiis-i  
  Consulte a [ferramenta de registro do IIS ASP.net (Aspnet\_regiis. exe)](https://msdn.microsoft.com/library/k6h9cz8h.aspx)
- Talvez seja necessário aumentar o limite de filas de [http. sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) do valor padrão de 1.000 para 5.000. Se a configuração for muito baixa, você poderá ver as solicitações de rejeição de [http. sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) com um status HTTP 503. Para alterar o limite da fila de HTTP. sys:

    - Abra o Gerenciador do IIS e navegue até o painel pools de aplicativos.
    - Clique com o botão direito do mouse no pool de aplicativos de destino e selecione **Configurações avançadas**.  
        ](using-asynchronous-methods-in-aspnet-45/_static/image4.png) ![avançado
    - Na caixa de diálogo **Configurações avançadas** , altere o *comprimento da fila* de 1.000 para 5.000.  
        comprimento da fila de ![](using-asynchronous-methods-in-aspnet-45/_static/image5.png)  
  
  Observe nas imagens acima, o .NET Framework é listado como v 4.0, embora o pool de aplicativos esteja usando o .NET 4,5. Para entender essa discrepância, consulte o seguinte:

- [O controle de versão .NET e multiplataforma-.NET 4,5 é uma atualização in-loco para o .NET 4,0](http://www.hanselman.com/blog/NETVersioningAndMultiTargetingNET45IsAnInplaceUpgradeToNET40.aspx)
- [Como definir um aplicativo IIS ou AppPool para usar ASP.NET 3,5 em vez de 2,0](http://www.hanselman.com/blog/HowToSetAnIISApplicationOrAppPoolToUseASPNET35RatherThan20.aspx)
- [Versões e dependências do .NET Framework](https://msdn.microsoft.com/library/bb822049(VS.110).aspx)

- Se seu aplicativo estiver usando serviços Web ou System.NET para se comunicar com um back-end por HTTP, talvez seja necessário aumentar o elemento [ConnectionManagement/maxConnection](https://msdn.microsoft.com/library/fb6y0fyc(VS.110).aspx) . Para aplicativos ASP.NET, isso é limitado pelo recurso de configuração automática para 12 vezes o número de CPUs. Isso significa que, em um procedimento quádruplo, você pode ter no máximo 12 \* 4 = 48 conexões simultâneas com um ponto de extremidade de IP. Como isso está vinculado à [configuração automática](https://msdn.microsoft.com/library/7w2sway1(VS.110).aspx), a maneira mais fácil de aumentar `maxconnection` em um aplicativo ASP.net é definir [System .net. ServicePointManager. DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit(VS.110).aspx) programaticamente no método from `Application_Start` no arquivo *global. asax* . Consulte o download de exemplo para obter um exemplo.
- No .NET 4,5, o padrão de 5000 para [maxConcurrentRequestsPerCPU](https://blogs.msdn.com/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx) deve ser bom.

## <a name="contributors"></a>Colaboradores

- [Broderick de arrecada](http://stackoverflow.com/users/59641/levi)
- [Tom Dykstra](http://www.bing.com/search?q=site%3Aasp.net+%22Tom+Dykstra%22+-forums.asp.net&amp;qs=n&amp;form=QBRE&amp;pq=site%3Aasp.net+%22tom+dykstra%22+-forums.asp.net&amp;sc=8-42&amp;sp=-1&amp;sk=)
- [Brad Wilson](http://bradwilson.typepad.com/)
- [HongMei GE](https://blogs.msdn.com/b/hongmeig/)
