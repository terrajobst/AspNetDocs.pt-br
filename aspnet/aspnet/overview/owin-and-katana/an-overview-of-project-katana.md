---
uid: aspnet/overview/owin-and-katana/an-overview-of-project-katana
title: Uma visão geral do Project Katana | Microsoft Docs
author: howarddierking
description: A estrutura ASP.NET já existe há mais de dez anos, e a plataforma habilitou o desenvolvimento de inúmeros sites e serviços da Web. Como aplicativo Intelligence da Web...
ms.author: riande
ms.date: 08/30/2013
ms.assetid: 0ee21741-c1bf-4025-a9b0-24580cae24bc
msc.legacyurl: /aspnet/overview/owin-and-katana/an-overview-of-project-katana
msc.type: authoredcontent
ms.openlocfilehash: 1f28db822930cdfd2ebf4cf9bb27d173f4aa4201
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78617232"
---
# <a name="an-overview-of-project-katana"></a>Uma visão geral do projeto Katana

de [Howard Dierking](https://github.com/howarddierking)

> A estrutura ASP.NET já existe há mais de dez anos, e a plataforma habilitou o desenvolvimento de inúmeros sites e serviços da Web. À medida que as estratégias de desenvolvimento de aplicativos Web evoluíram, a estrutura foi capaz de evoluir em etapa com tecnologias como ASP.NET MVC e ASP.NET Web API. À medida que o desenvolvimento de aplicativos da Web leva à próxima etapa evolucionário do mundo da computação em nuvem, o Project [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) fornece o conjunto subjacente de componentes para aplicativos ASP.net, permitindo que eles sejam flexíveis, portáteis, leves e ofereçam melhor desempenho – em outras palavras, o Project [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) cloud otimiza seus aplicativos ASP.net.

## <a name="why-katana--why-now"></a>Por que Katana – por que agora?

 Independentemente se um estiver discutindo uma estrutura de desenvolvedor ou um produto de usuário final, é importante entender as motivações subjacentes para criar o produto – e parte do que inclui saber de quem o produto foi criado. O ASP.NET foi criado originalmente com dois clientes em mente.   
  
**O primeiro grupo de clientes era desenvolvedores de ASP clássicos.** No momento, o ASP era uma das principais tecnologias para a criação de sites e aplicativos dinâmicos e controlados por dados pela marcação intercalação e pelo script do lado do servidor. O tempo de execução do ASP fornecia um script do lado do servidor com um conjunto de objetos que abstrairam aspectos principais do protocolo HTTP subjacente e do servidor Web e forneceu acesso a serviços adicionais, como gerenciamento de estado de aplicativo e sessão, cache, etc. Embora os aplicativos ASP clássicos e avançados se tornaram um desafio para gerenciar à medida que cresceram em tamanho e complexidade. Isso foi basicamente devido à falta de estrutura encontrada em ambientes de script, juntamente com a duplicação de código resultante da intercalação de código e marcação. Para aproveitar os pontos fortes do ASP clássico enquanto aborda alguns dos seus desafios, o ASP.NET aproveitou a organização do código fornecida pelas linguagens orientadas a objeto do .NET Framework enquanto preserva o modelo de programação no lado do servidor para o qual os desenvolvedores de ASP clássicos cresceram acostumados.

**O segundo grupo de clientes de destino para ASP.NET foi o Windows Business Application Developers.** Ao contrário dos desenvolvedores de ASP clássicos, que estavam acostumados a escrever marcação HTML e o código para gerar mais marcação HTML, os desenvolvedores de WinForms (como os desenvolvedores de VB6 antes deles) estavam acostumados com uma experiência de tempo de design que incluía uma tela e um conjunto avançado de usuários controles de interface. A primeira versão do ASP.NET – também conhecida como "Web Forms" forneceu uma experiência de tempo de design semelhante, juntamente com um modelo de evento do lado do servidor para componentes de interface do usuário e um conjunto de recursos de infraestrutura (como ViewState) para criar uma experiência de desenvolvedor integrada entre a programação do cliente e do servidor. Web Forms efetivamente HID a natureza sem estado da Web sob um modelo de eventos com estado que era familiar para os desenvolvedores do WinForms.

### <a name="challenges-raised-by-the-historical-model"></a>Desafios gerados pelo modelo histórico

**O resultado líquido era um modelo de programação de desenvolvedor e de tempo de execução repleto de recursos e completo.** No entanto, com esse recurso, a riqueza apresentou alguns desafios notáveis. Em primeiro lugar, a estrutura era **monolítica**, com unidades de funcionalidade separadas logicamente que estão intimamente ligadas no mesmo assembly System. Web. dll (por exemplo, os principais objetos http com a estrutura do Web Forms). Em segundo lugar, o ASP.NET foi incluído como parte do .NET Framework maior, o que significa que o **tempo entre as versões estava na ordem de anos.** Isso dificultou para o ASP.NET acompanhar todas as alterações que estão ocorrendo no desenvolvimento para a Web em rápida evolução. Por fim, System. Web. dll em si foi acoplado de algumas maneiras diferentes a uma opção de hospedagem Web específica: Serviços de Informações da Internet (IIS).

### <a name="evolutionary-steps-aspnet-mvc-and-aspnet-web-api"></a>Etapas evolucionários: ASP.NET MVC e ASP.NET Web API

E muitas alterações estavam acontecendo no desenvolvimento para a Web! Os aplicativos Web estavam sendo cada vez mais desenvolvidos como uma série de componentes pequenos e concentrados em vez de estruturas grandes. O número de componentes, bem como a frequência com que foram liberados, estava aumentando a uma taxa sempre mais rápida. Ficou claro que manter o ritmo com a Web exigiria que as estruturas fossem menores, desacopladas e mais concentradas, em vez de maiores e mais ricas em recursos, portanto, a **equipe de ASP.net levou várias etapas evolucionários para habilitar o ASP.net como uma família de componentes da Web conectáveis em vez de uma única estrutura**.

Uma das primeiras alterações foi o aumento na popularidade do padrão de design MVC (Model-View-Controller) bem conhecido graças às estruturas de desenvolvimento da Web, como Ruby on Rails. Esse estilo de criação de aplicativos Web deu ao desenvolvedor um maior controle sobre a marcação do aplicativo enquanto preserva a separação da marcação e da lógica de negócios, que era um dos pontos de venda iniciais do ASP.NET. Para atender à demanda por esse estilo de desenvolvimento de aplicativos Web, a Microsoft tomou a oportunidade de se posicionar melhor para o futuro ao **desenvolver o ASP.NET MVC fora da banda** (e não incluí-lo no .NET Framework). O ASP.NET MVC foi lançado como um download independente. Isso deu à equipe de engenharia a flexibilidade de fornecer atualizações com muito mais frequência do que era possível anteriormente.

Outra mudança importante no desenvolvimento de aplicativos Web foi a mudança das páginas da Web dinâmicas, geradas pelo servidor, para a marcação inicial estática com seções dinâmicas da página geradas por meio do script do lado do cliente **com APIs da Web de back-end por meio de solicitações Ajax**. Essa mudança de arquitetura ajudou a lançarr o aumento das APIs da Web e o desenvolvimento da estrutura de ASP.NET Web API. Como no caso do ASP.NET MVC, a versão do ASP.NET Web API forneceu outra oportunidade de desenvolver o ASP.NET mais detalhadamente como uma estrutura mais modular. A equipe de engenharia aproveitou a oportunidade e **criou ASP.NET Web API de modo que não havia nenhuma dependência em nenhum dos principais tipos de estrutura encontrados em System. Web. dll**. Isso habilitou duas coisas: primeiro, isso significava que ASP.NET Web API poderia evoluir de uma maneira totalmente independente (e poderia continuar a iterar rapidamente porque ela é entregue via NuGet). Segundo, como não havia dependências externas para System. Web. dll e, portanto, nenhuma dependência para o IIS, ASP.NET Web API incluía a capacidade de execução em um host personalizado (por exemplo, um aplicativo de console, serviço do Windows, etc.)

### <a name="the-future-a-nimble-framework"></a>O futuro: uma estrutura descuidado

Ao dissociar componentes da estrutura uns dos outros e, em seguida, liberá-los no NuGet, as estruturas podem agora ser **iteradas de forma mais independente e mais rápida**. Além disso, o poder e a flexibilidade do recurso de hospedagem interna da API da Web provou muito atraente para os desenvolvedores que desejavam um **host pequeno e leve** para seus serviços. Ele provou tão atraente, na verdade, que outras estruturas também queriam esse recurso, e isso apresentava um novo desafio em que cada estrutura foi executada em seu próprio processo de host em seu próprio endereço base e precisava ser gerenciada (iniciada, interrompida, etc.) de forma independente. Um aplicativo Web moderno geralmente dá suporte A serviços de arquivos estáticos, geração de página dinâmica, API da Web e notificações por push/em tempo real mais recentes. Esperar que cada um desses serviços deva ser executado e gerenciado de forma independente simplesmente não é realista.

O que era necessário era uma abstração de hospedagem única que permitisse a um desenvolvedor compor um aplicativo de uma variedade de componentes e estruturas diferentes e, em seguida, executar esse aplicativo em um host de suporte.

## <a name="the-open-web-interface-for-net-owin"></a>A interface da Web aberta para .NET (OWIN)

 Inspirado pelos benefícios obtidos pelo [rack](http://rack.github.io/) na comunidade Ruby, vários membros da Comunidade .net configuraram para criar uma abstração entre servidores Web e componentes de estrutura. Duas metas de design para a abstração de OWIN era que era simples e que levou o menor número possível de dependências em outros tipos de estrutura. Esses dois objetivos ajudam a garantir:

- Novos componentes podem ser desenvolvidos e consumidos com mais facilidade.
- Os aplicativos podem ser portados com mais facilidade entre hosts e sistemas operacionais e plataformas potencialmente inteiros.

A abstração resultante consiste em dois elementos principais. A primeira é o dicionário de ambiente. Essa estrutura de dados é responsável por armazenar todo o estado necessário para processar uma solicitação e resposta HTTP, bem como qualquer estado de servidor relevante. O dicionário de ambiente é definido da seguinte maneira:

[!code-console[Main](an-overview-of-project-katana/samples/sample1.cmd)]

Um servidor Web compatível com OWIN é responsável por preencher o dicionário de ambiente com dados como os fluxos de corpo e coleções de cabeçalho para uma resposta e solicitação HTTP. Em seguida, é responsabilidade dos componentes do aplicativo ou da estrutura preencherem ou atualizarem o dicionário com valores adicionais e gravar no fluxo do corpo da resposta.

Além de especificar o tipo para o dicionário de ambiente, a especificação OWIN define uma lista de pares principais de valor de chave de dicionário. Por exemplo, a tabela a seguir mostra as chaves de dicionário necessárias para uma solicitação HTTP:

| Nome da Chave | Descrição do valor |
| --- | --- |
| `"owin.RequestBody"` | Um fluxo com o corpo da solicitação, se houver. Stream. NULL poderá ser usado como um espaço reservado se não houver um corpo de solicitação. Consulte o [corpo da solicitação](http://owin.org/html/owin.html#34-request-body-100-continue-and-completed-semantics). |
| `"owin.RequestHeaders"` | Um `IDictionary<string, string[]>` de cabeçalhos de solicitação. Consulte [cabeçalhos](http://owin.org/html/owin.html#3-3-headers). |
| `"owin.RequestMethod"` | Um `string` que contém o método de solicitação HTTP da solicitação (por exemplo, `"GET"`, `"POST"`). |
| `"owin.RequestPath"` | Um `string` que contém o caminho da solicitação. O caminho deve ser relativo à "raiz" do representante do aplicativo; consulte [caminhos](http://owin.org/html/owin.html#5-3-paths). |
| `"owin.RequestPathBase"` | Um `string` que contém a parte do caminho da solicitação correspondente à "raiz" do representante do aplicativo; consulte [caminhos](http://owin.org/html/owin.html#5-3-paths). |
| `"owin.RequestProtocol"` | Um `string` que contém o nome do protocolo e a versão (por exemplo, `"HTTP/1.0"` ou `"HTTP/1.1"`). |
| `"owin.RequestQueryString"` | Um `string` que contém o componente de cadeia de caracteres de consulta do URI de solicitação HTTP, sem o "?" à esquerda (por exemplo, `"foo=bar&baz=quux"`). O valor pode ser uma cadeia de caracteres vazia. |
| `"owin.RequestScheme"` | Um `string` que contém o esquema de URI usado para a solicitação (por exemplo, `"http"`, `"https"`); consulte [esquema de URI](http://owin.org/html/owin.html#5-1-uri-scheme). |

O segundo elemento de chave de OWIN é o delegado do aplicativo. Essa é uma assinatura de função que serve como a interface primária entre todos os componentes em um aplicativo OWIN. A definição para o delegado do aplicativo é a seguinte:

`Func<IDictionary<string, object>, Task>;`

Em seguida, o delegado do aplicativo é simplesmente uma implementação do tipo delegado Func, em que a função aceita o dicionário do ambiente como entrada e retorna uma tarefa. Esse design tem várias implicações para os desenvolvedores:

- Há um número muito pequeno de dependências de tipo necessárias para a gravação de componentes OWIN. Isso aumenta muito a acessibilidade do OWIN para os desenvolvedores.
- O design assíncrono permite que a abstração seja eficiente com sua manipulação de recursos de computação, especialmente em operações com uso intensivo de e/s.
- Como o representante do aplicativo é uma unidade atômica de execução e como o dicionário do ambiente é transportado como um parâmetro no delegado, os componentes do OWIN podem ser facilmente encadeados para criar pipelines de processamento HTTP complexos.

Do ponto de vista da implementação, OWIN é uma especificação ([http://owin.org/html/owin.html](http://owin.org/html/owin.html)). Seu objetivo é não ser a próxima estrutura da Web, mas sim uma especificação de como as estruturas da Web e os servidores Web interagem.

Se você investigou [OWIN](http://owin.org/) ou [Katana](https://github.com/aspnet/AspNetKatana/wiki), talvez também tenha notado o [pacote NuGet do OWIN](http://nuget.org/packages/Owin) e o OWIN. dll. Essa biblioteca contém uma única interface, [IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs), que formalizes e codifica a seqüência de inicialização descrita na [seção 4](http://owin.org/html/owin.html#4-application-startup) da especificação OWIN. Embora não seja necessário para criar servidores OWIN, a interface [IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs) fornece um ponto de referência concreto e é usada pelos componentes do projeto katana.

## <a name="project-katana"></a>Katana do projeto

Enquanto a especificação [OWIN](http://owin.org/html/owin.html) e a *OWIN. dll* são de propriedade da Comunidade e da Comunidade, o projeto do [Katana](https://github.com/aspnet/AspNetKatana/wiki) representa o conjunto de componentes do OWIN que, ao mesmo tempo, o código-fonte aberto, são criados e liberados pela Microsoft. Esses componentes incluem componentes de infraestrutura, como hosts e servidores, bem como componentes funcionais, como componentes de autenticação e associações a estruturas como [signalr](../../../signalr/index.md) e [ASP.NET Web API](../../../web-api/overview/getting-started-with-aspnet-web-api/index.md). O projeto tem três seguintes metas de alto nível: 

- **Portátil** – os componentes devem ser capazes de ser facilmente substituídos por novos componentes à medida que forem disponibilizados. Isso inclui todos os tipos de componentes do Framework para o servidor e o host. A implicação desse objetivo é que as estruturas de terceiros podem ser executadas diretamente em servidores Microsoft, enquanto as estruturas da Microsoft podem potencialmente ser executadas em servidores e hosts de terceiros.
- **Modular/flexível**– ao contrário de muitas estruturas que incluem uma infinidade de recursos que são ativados por padrão, os componentes do projeto Katana devem ser pequenos e concentrados, dando controle ao desenvolvedor do aplicativo para determinar quais componentes usar em seu aplicativo.
- **Leve/de alto desempenho/escalonável** – dividindo a noção tradicional de uma estrutura em um conjunto de componentes pequenos e focados que são adicionados explicitamente pelo desenvolvedor do aplicativo, um aplicativo Katana resultante pode consumir menos recursos de computação e, como resultado, lidar com mais carga, do que com outros tipos de servidores e estruturas. Como os requisitos do aplicativo exigem mais recursos da infraestrutura subjacente, eles podem ser adicionados ao pipeline do OWIN, mas isso deve ser uma decisão explícita sobre a parte do desenvolvedor do aplicativo. Além disso, a substituição de componentes de nível inferior significa que, à medida que eles se tornam disponíveis, novos servidores de alto desempenho podem ser introduzidos diretamente para melhorar o desempenho dos aplicativos OWIN sem interromper esses aplicativos.

## <a name="getting-started-with-katana-components"></a>Introdução com componentes Katana

Quando ele foi introduzido pela primeira vez, um aspecto da estrutura [node. js](http://nodejs.org/) que atraiu imediatamente a atenção de pessoas foi a simplicidade com a qual poderia criar e executar um servidor Web. Se as metas do katana fossem enquadradas na luz do [node. js](http://nodejs.org/), pode-se resumir isso dizendo que o Katana traz muitos dos benefícios do [node. js](http://nodejs.org/) (e das estruturas como ele) sem forçar o desenvolvedor a lançar tudo o que ele sabe sobre o desenvolvimento de aplicativos Web ASP.net. Para que essa instrução se mantenha verdadeira, a introdução ao projeto Katana deve ser igualmente simples por natureza do [node. js](http://nodejs.org/).

## <a name="creating-hello-world"></a>Criando "Olá, Mundo!"

Uma diferença notável entre o desenvolvimento de JavaScript e .NET é a presença (ou ausência) de um compilador. Como tal, o ponto de partida para um servidor Katana simples é um projeto do Visual Studio. No entanto, podemos começar com o mínimo de tipos de projeto: o aplicativo Web ASP.NET vazio.

[![](an-overview-of-project-katana/_static/image1.png)](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb)

Em seguida, instalaremos o pacote NuGet [Microsoft. Owin. host. SystemWeb](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) no projeto. Esse pacote fornece um servidor OWIN que é executado no pipeline de solicitação do ASP.NET. Ele pode ser encontrado na [Galeria do NuGet](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) e pode ser instalado usando a caixa de diálogo Gerenciador de pacotes do Visual Studio ou o console do Gerenciador de pacotes com o seguinte comando:

[!code-console[Main](an-overview-of-project-katana/samples/sample2.cmd)]

A instalação do pacote de `Microsoft.Owin.Host.SystemWeb` instalará alguns pacotes adicionais como dependências. Uma dessas dependências é `Microsoft.Owin`, uma biblioteca que fornece vários tipos auxiliares e métodos para o desenvolvimento de aplicativos OWIN. Podemos usar esses tipos para escrever rapidamente o seguinte servidor "Olá mundo".

[!code-csharp[Main](an-overview-of-project-katana/samples/sample3.cs)]

Esse servidor Web muito simples agora pode ser executado usando o comando **F5** do Visual Studio e inclui suporte completo para depuração.

## <a name="switching-hosts"></a>Alternando hosts

Por padrão, o exemplo anterior "Olá, mundo" é executado no pipeline de solicitação ASP.NET, que usa System. Web no contexto do IIS. Isso pode, por si só, adicionar um enorme valor, pois ele nos permite aproveitar a flexibilidade e a capacidade de composição de um pipeline OWIN com os recursos de gerenciamento e a maturidade geral do IIS. No entanto, pode haver casos em que os benefícios fornecidos pelo IIS não são necessários e o desejo é para um host menor e mais leve. O que é necessário e, em seguida, executar nosso servidor Web simples fora do IIS e do System. Web?

Para ilustrar a meta de portabilidade, mover de um host de servidor Web para um host de linha de comando requer simplesmente adicionar as novas dependências de servidor e host à pasta de saída do projeto e, em seguida, iniciar o host. Neste exemplo, vamos hospedar nosso servidor Web em um host Katana chamado `OwinHost.exe` e usará o servidor baseado em Katana HttpListener. Da mesma forma que os outros componentes do katana, eles serão adquiridos do NuGet usando o seguinte comando:

[!code-console[Main](an-overview-of-project-katana/samples/sample4.cmd)]

Na linha de comando, podemos navegar até a pasta raiz do projeto e simplesmente executar o `OwinHost.exe` (que foi instalado na pasta Tools de seu respectivo pacote NuGet). Por padrão, o `OwinHost.exe` está configurado para procurar o servidor baseado em HttpListener e, portanto, nenhuma configuração adicional é necessária. Navegar em um navegador da Web para `http://localhost:5000/` mostra o aplicativo agora em execução no console do.

![](an-overview-of-project-katana/_static/image2.png)

## <a name="katana-architecture"></a>Arquitetura do katana

 A arquitetura do componente Katana divide um aplicativo em quatro camadas lógicas, conforme descrito abaixo: *host, servidor, middleware* e *aplicativo*. A arquitetura do componente é fatorada de forma que as implementações dessas camadas possam ser facilmente substituídas, em muitos casos, sem a necessidade de recompilação do aplicativo.   

![](an-overview-of-project-katana/_static/image3.png)

## <a name="host"></a>Host

 O host é responsável por:

- Gerenciando o processo subjacente.
- Orquestrar o fluxo de trabalho que resulta na seleção de um servidor e na construção de um pipeline OWIN por meio do qual as solicitações serão tratadas.

  No momento, há três opções de hospedagem primária para aplicativos baseados em katana:  
  
**IIS/ASP. net**: usando os tipos HttpModule e HttpHandler padrão, os pipelines OWIN podem ser executados no IIS como parte de um fluxo de solicitação ASP.net. O suporte à hospedagem ASP.NET é habilitado pela instalação do pacote NuGet Microsoft. AspNet. host. SystemWeb em um projeto de aplicativo Web. Além disso, como o IIS atua como um host e um servidor, a distinção de servidor/host OWIN é conplanada nesse pacote NuGet, o que significa que, se usar o host SystemWeb, um desenvolvedor não poderá substituir uma implementação de servidor alternativo.  
  
**Host personalizado**: o Katana Component Suite oferece a um desenvolvedor a capacidade de hospedar aplicativos em seu próprio processo personalizado, seja um aplicativo de console, serviço do Windows, etc. Esse recurso é semelhante ao recurso de hospedagem interna fornecido pela API Web. O exemplo a seguir mostra um host personalizado do código da API Web:  

[!code-csharp[Main](an-overview-of-project-katana/samples/sample5.cs)]

A configuração de auto-host para um aplicativo Katana é semelhante:

[!code-csharp[Main](an-overview-of-project-katana/samples/sample6.cs)]

Uma diferença notável entre a API Web e os exemplos de autohost Katana é que o código de configuração da API Web está faltando no exemplo de autohost katana. Para habilitar a portabilidade e a capacidade de composição, o Katana separa o código que inicia o servidor do código que configura o pipeline de processamento de solicitações. O código que configura a API Web, em seguida, está contido na inicialização da classe, que é adicionalmente especificada como o parâmetro de tipo em WebApplication. Start.

[!code-csharp[Main](an-overview-of-project-katana/samples/sample7.cs)]

A classe de inicialização será discutida mais detalhadamente mais adiante neste artigo. No entanto, o código necessário para iniciar um processo de autohost do katana parece bastante semelhante ao código que você pode usar hoje em ASP.NET Web API aplicativos de hospedagem interna.

**OwinHost. exe**: embora alguns queiram escrever um processo personalizado para executar aplicativos Web do katana, muitos preferem simplesmente iniciar um executável predefinido que pode iniciar um servidor e executar seu aplicativo. Para esse cenário, o pacote de componentes do katana inclui `OwinHost.exe`. Quando executado de dentro do diretório raiz de um projeto, esse executável iniciará um servidor (ele usa o servidor HttpListener por padrão) e usará as convenções para localizar e executar a classe de inicialização do usuário. Para um controle mais granular, o executável fornece vários parâmetros de linha de comando adicionais.

![](an-overview-of-project-katana/_static/image4.png)

## <a name="server"></a>Servidor

 Embora o host seja responsável por iniciar e manter o processo no qual o aplicativo é executado, a responsabilidade do servidor é abrir um soquete de rede, escutar solicitações e enviá-las por meio do pipeline de componentes OWIN especificados pelo usuário (como você pode já ter notado, esse pipeline é especificado na classe de inicialização do desenvolvedor do aplicativo). Atualmente, o projeto Katana inclui duas implementações de servidor: 

- **Microsoft. Owin. host. SystemWeb**: como mencionado anteriormente, o IIS em conjunto com o pipeline ASP.net atua como um host e um servidor. Portanto, ao escolher essa opção de hospedagem, o IIS gerencia questões de nível de host, como ativação de processo e escuta solicitações HTTP. Para aplicativos Web ASP.NET, ele envia as solicitações para o pipeline ASP.NET. O host Katana SystemWeb registra um ASP.NET HttpModule e um HttpHandler para interceptar solicitações conforme elas fluem pelo pipeline HTTP e as envia por meio do pipeline OWIN especificado pelo usuário.
- **Microsoft. Owin. host. HttpListener**: como o nome indica, esse servidor Katana usa a classe HttpListener do .NET Framework para abrir um soquete e enviar solicitações para um pipeline de Owin especificado pelo desenvolvedor. Atualmente, essa é a seleção de servidor padrão para a API de auto-host Katana e OwinHost. exe.

## <a name="middlewareframework"></a>Middleware/estrutura

 Como mencionado anteriormente, quando o servidor aceita uma solicitação de um cliente, ele é responsável por passá-lo por meio de um pipeline de componentes OWIN, que são especificados pelo código de inicialização do desenvolvedor. Esses componentes de pipeline são conhecidos como middleware.  
 Em um nível muito básico, um componente de middleware OWIN simplesmente precisa implementar o delegado de aplicativo OWIN para que ele possa ser chamado.

[!code-console[Main](an-overview-of-project-katana/samples/sample8.cmd)]

No entanto, para simplificar o desenvolvimento e a composição de componentes de middleware, o Katana dá suporte a algumas convenções e tipos auxiliares para componentes de middleware. As mais comuns são a classe `OwinMiddleware`. Um componente de middleware personalizado criado com essa classe seria semelhante ao seguinte: 

[!code-csharp[Main](an-overview-of-project-katana/samples/sample9.cs)]

 Essa classe deriva de `OwinMiddleware`, implementa um construtor que aceita uma instância do próximo middleware no pipeline como um de seus argumentos e, em seguida, a passa para o construtor base. Argumentos adicionais usados para configurar o middleware também são declarados como parâmetros de Construtor após o próximo parâmetro de middleware.   
  
Em tempo de execução, o middleware é executado por meio do método de `Invoke` substituído. Esse método usa um único argumento do tipo `OwinContext`. Esse objeto de contexto é fornecido pelo pacote NuGet `Microsoft.Owin` descrito anteriormente e fornece acesso fortemente tipado à solicitação, à resposta e ao dicionário de ambiente, juntamente com alguns tipos auxiliares adicionais.   
  
A classe middleware pode ser facilmente adicionada ao pipeline OWIN no código de inicialização do aplicativo da seguinte maneira:   

[!code-csharp[Main](an-overview-of-project-katana/samples/sample10.cs)]

Como a infraestrutura Katana simplesmente cria um pipeline de componentes de middleware OWIN, e como os componentes simplesmente precisam dar suporte ao representante do aplicativo para participar do pipeline, os componentes de middleware podem variar em complexidade desde agentes simples até estruturas inteiras, como ASP.NET, API da Web ou [signalr](../../../signalr/index.md). Por exemplo, a adição de ASP.NET Web API ao pipeline OWIN anterior requer a adição do seguinte código de inicialização:

[!code-csharp[Main](an-overview-of-project-katana/samples/sample11.cs)]

A infraestrutura Katana criará o pipeline de componentes de middleware com base na ordem em que foram adicionados ao objeto IAppBuilder no método de configuração. Em nosso exemplo, LoggerMiddleware pode lidar com todas as solicitações que fluem pelo pipeline, independentemente de como essas solicitações são tratadas por fim. Isso permite cenários poderosos em que um componente de middleware (por exemplo, um componente de autenticação) pode processar solicitações para um pipeline que inclui vários componentes e estruturas (por exemplo, ASP.NET Web API, Signalr e um servidor de arquivos estático).
 
## <a name="applications"></a>Aplicativos

Conforme ilustrado pelos exemplos anteriores, OWIN e o projeto Katana não devem ser considerados como um novo modelo de programação de aplicativo, mas sim como uma abstração para dissociar modelos e estruturas de programação de aplicativos do servidor e da infraestrutura de hospedagem. Por exemplo, ao criar aplicativos de API Web, a estrutura do desenvolvedor continuará a usar o ASP.NET Web API Framework, independentemente de o aplicativo ser executado ou não em um pipeline OWIN usando componentes do projeto katana. O único lugar em que o código relacionado ao OWIN ficará visível para o desenvolvedor do aplicativo será o código de inicialização do aplicativo, onde o desenvolvedor compõe o pipeline do OWIN. No código de inicialização, o desenvolvedor registrará uma série de instruções UseXx, geralmente uma para cada componente de middleware que processará as solicitações de entrada. Essa experiência terá o mesmo efeito que registrar módulos HTTP no mundo atual do sistema. Web. Normalmente, um middleware de estrutura maior, como ASP.NET Web API ou [signalr](../../../signalr/index.md) , será registrado no final do pipeline. Os componentes de middleware abrangentes, como aqueles para autenticação ou cache, geralmente são registrados em direção ao início do pipeline para que eles processem solicitações para todas as estruturas e componentes registrados posteriormente no pipeline. Essa separação dos componentes de middleware uns dos outros e dos componentes de infraestrutura subjacentes permite que os componentes evoluam em diferentes velocidades, garantindo que o sistema geral permaneça estável.

## <a name="components--nuget-packages"></a>Componentes – pacotes NuGet

Como muitas bibliotecas e estruturas atuais, os componentes do projeto Katana são fornecidos como um conjunto de pacotes NuGet. Para a próxima versão 2,0, o grafo de dependência do pacote Katana tem a seguinte aparência. (Clique na imagem para exibição maior.)

[![](an-overview-of-project-katana/_static/image6.png)](an-overview-of-project-katana/_static/image5.png)

Quase todos os pacotes no projeto Katana dependem, direta ou indiretamente, no pacote Owin. Você pode se lembrar de que esse é o pacote que contém a interface IAppBuilder, que fornece uma implementação concreta da sequência de inicialização do aplicativo descrita na seção 4 da especificação OWIN. Além disso, muitos dos pacotes dependem de Microsoft. Owin, que fornece um conjunto de tipos auxiliares para trabalhar com solicitações e respostas HTTP. O restante do pacote pode ser classificado como Hospedagem de pacotes de infraestrutura (servidores ou hosts) ou middleware. Os pacotes e as dependências que são externos ao projeto Katana são exibidos em laranja.

A infraestrutura de hospedagem para Katana 2,0 inclui os servidores baseados em SystemWeb e HttpListener, o pacote OwinHost para executar aplicativos do OWIN usando o OwinHost. exe e o pacote Microsoft. Owin. Hosting para aplicativos OWIN de hospedagem interna em um host personalizado (por exemplo, aplicativo de console, serviço do Windows, etc.)

Para o Katana 2,0, os componentes de middleware se concentram principalmente em fornecer meios diferentes de autenticação. Um componente de middleware adicional para diagnóstico é fornecido, que habilita o suporte para uma página de início e erro. À medida que o OWIN cresce na abstração de hospedagem real, o ecossistema de componentes de middleware, ambos desenvolvidos pela Microsoft e por terceiros, também crescerá em número.

## <a name="conclusion"></a>Conclusão

 Desde o início, a meta do projeto Katana não foi criar e, portanto, forçaria os desenvolvedores a aprenderem outra estrutura da Web. Em vez disso, o objetivo foi criar uma abstração para dar aos desenvolvedores de aplicativos Web do .NET mais opções do que antes era possível. Ao dividir as camadas lógicas de uma pilha de aplicativos Web típica em um conjunto de componentes substituíveis, o projeto Katana permite que os componentes em toda a pilha se aprimorem em qualquer taxa que faça sentido para esses componentes. Ao criar todos os componentes em relação à abstração OWIN simples, o Katana permite que as estruturas e os aplicativos criados com base neles sejam portáteis em vários servidores e hosts diferentes. Ao colocar o desenvolvedor no controle da pilha, o Katana garante que o desenvolvedor faça a escolha final sobre o quão leve ou como a pilha da Web é mais rica em recursos.  

## <a name="for-more-information-about-katana"></a>Para obter mais informações sobre Katana

- O projeto Katana no GitHub: [https://github.com/aspnet/AspNetKatana/](https://github.com/aspnet/AspNetKatana/).
- Vídeo: [The Katana Project-OWIN for ASP.net](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET), de Howard Dierking.

## <a name="acknowledgements"></a>Confirmações

- [Rick Anderson](https://blogs.msdn.com/b/rickandy/): (Twitter [@RickAndMSFT](http://twitter.com/RickAndMSFT) ) Rick é um escritor de programação sênior para a Microsoft focado no Azure e no MVC.
- [Scott Hanselman](http://www.hanselman.com/blog/): (Twitter [@shanselman](https://twitter.com/shanselman) )
- [Jon Galloway](https://weblogs.asp.net/jgalloway/default.aspx): (Twitter [@jongalloway](https://twitter.com/jongalloway) )
