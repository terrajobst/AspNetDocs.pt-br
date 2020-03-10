---
uid: whitepapers/mvc4-release-notes
title: ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: Este documento descreve a versão do ASP.NET MVC 4.
ms.author: riande
ms.date: 09/09/2011
ms.assetid: f014524f-25c0-4094-b8e1-886d99536f00
msc.legacyurl: /whitepapers/mvc4-release-notes
msc.type: content
ms.openlocfilehash: b57480bd0274fbb76c600dfb0dd09037bdcbf1e7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78563423"
---
# <a name="aspnet-mvc-4"></a>ASP.NET MVC 4

> Este documento descreve a versão do ASP.NET MVC 4.

- [Notas de instalação](#_Toc303253802)
- [Documentação](#_Toc303253803)
- [Suporte](#_Toc303253804)
- [Requisitos de software](#_Toc303253805)
- [Novos recursos no ASP.NET MVC 4](#_Toc303253807)

    - [ASP.NET Web API](#_Toc317096197)
    - [Aprimoramentos para modelos de projeto padrão](#_Toc303253808)
    - [Modelo de projeto móvel](#_Toc303253809)
    - [Modos de exibição](#_Toc303253810)
    - [jQuery Mobile, o seletor de exibição e a substituição do navegador](#_Toc303253811)
    - [Suporte a tarefas para controladores assíncronos](#_Toc303253813)
    - [SDK do Azure](#_Toc303253814)
    - [Migrações de banco de dados](#_Toc303253818)
    - [Modelo de projeto vazio](#_Toc303253819)
    - [Adicionar controlador a qualquer pasta do projeto](#_Toc303253820)
    - [Agrupamento e minificação](#_Toc303253821)
    - [Habilitando logons do Facebook e de outros sites usando OAuth e OpenID](#_Toc303253822)
- [Atualizando um projeto do ASP.NET MVC 3 para o ASP.NET MVC 4](#_Toc303253806)
- [Alterações do ASP.NET MVC 4 Release Candidate](#_Toc303253817)
- [Problemas conhecidos e alterações recentes](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Notas de instalação

O ASP.NET MVC 4 para Visual Studio 2010 pode ser instalado por meio do [ASP.NET MVC 4 Home Page](../mvc/mvc4.md) usando o Web Platform Installer.

Recomendamos desinstalar todas as visualizações previamente instaladas do ASP.NET MVC 4 antes de instalar o ASP.NET MVC 4. Você pode atualizar o ASP.NET MVC 4 beta e Release Candidate para o ASP.NET MVC 4 sem desinstalar o.

Esta versão não é compatível com as versões de visualização do .NET Framework 4,5. Você deve atualizar separadamente as versões de visualização instaladas do .NET Framework 4,5 para a versão final antes de instalar o ASP.NET MVC 4.

O ASP.NET MVC 4 pode ser instalado e executado lado a lado com o ASP.NET MVC 3.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Documentação

A documentação do ASP.NET MVC está disponível no site do MSDN na seguinte URL:

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

Os tutoriais e outras informações sobre o ASP.NET MVC estão disponíveis na página MVC 4 do site do ASP.NET ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).

<a id="_Toc303253804"></a>
## <a name="support"></a>Suporte

O ASP.NET MVC 4 tem suporte total. Se você tiver dúvidas sobre como trabalhar com esta versão, também poderá lançá-las no ASP.NET MVC Forum ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), em que os membros da comunidade do ASP.net freqüentemente podem fornecer suporte informal.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Requisitos de software

Os componentes do ASP.NET MVC 4 para Visual Studio exigem o PowerShell 2,0 e o Visual Studio 2010 com Service Pack 1 ou o Visual Web Developer Express 2010 com Service Pack 1.

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4"></a>Novos recursos no ASP.NET MVC 4

Esta seção descreve os recursos que foram introduzidos na versão ASP.NET MVC 4.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>ASP.NET Web API

O ASP.NET MVC 4 inclui ASP.NET Web API, uma nova estrutura para a criação de serviços HTTP que podem alcançar uma ampla variedade de clientes, incluindo navegadores e dispositivos móveis. ASP.NET Web API também é uma plataforma ideal para a criação de serviços RESTful.

O ASP.NET Web API inclui suporte para os seguintes recursos:

- **Modelo de programação de http moderno:** Acesse e manipule diretamente solicitações e respostas HTTP em suas APIs Web usando um novo modelo de objeto HTTP com rigidez de tipos. O mesmo modelo de programação e o pipeline HTTP estão disponíveis simetricamente no cliente por meio do novo tipo *HttpClient* .
- **Suporte completo para rotas:** O ASP.NET Web API dá suporte ao conjunto completo de recursos de rota do roteamento ASP.NET, incluindo parâmetros de rota e restrições. Além disso, use convenções simples para mapear ações para métodos HTTP.
- **Negociação de conteúdo:** O cliente e o servidor podem trabalhar juntos para determinar o formato correto dos dados retornados de uma API da Web. ASP.NET Web API fornece suporte padrão para XML, JSON e formatos codificados por URL de formulário e você pode estender esse suporte adicionando seus próprios formatadores ou até mesmo substituir a estratégia de negociação de conteúdo padrão.
- **Validação e Associação de modelo:** Os associadores de modelo fornecem uma maneira fácil de extrair dados de várias partes de uma solicitação HTTP e converter essas partes de mensagem em objetos .NET que podem ser usados pelas ações da API Web. A validação também é executada em parâmetros de ação com base em anotações de dados.
- **Filtros:** O ASP.NET Web API dá suporte a filtros, incluindo filtros conhecidos, como o atributo *[Authorize]* . Você pode criar e conectar seus próprios filtros para ações, autorização e manipulação de exceção.
- **Composição de consulta:** Use o atributo de filtro *[consultável]* em uma ação que retorna *IQueryable* para habilitar o suporte para consultar sua API Web por meio das convenções de consulta OData.
- **Capacidade de teste aprimorada:** Em vez de definir detalhes de HTTP em objetos de contexto estático, as ações da API Web funcionam com instâncias de *HttpRequestMessage* e *HttpResponseMessage*. Crie um projeto de teste de unidade junto com seu projeto de API Web para começar a escrever rapidamente testes de unidade para sua funcionalidade de API Web.
- **Configuração baseada em código:** ASP.NET Web API configuração é realizada exclusivamente por meio de código, deixando os arquivos de configuração limpos. Use o padrão do localizador de serviço fornecido para configurar pontos de extensibilidade.
- **Suporte aprimorado para contêineres de inversão de controle (IOC):** ASP.NET Web API fornece excelente suporte para contêineres IoC por meio de uma abstração de resolvedor de dependência aprimorada
- **Auto-host:** As APIs da Web podem ser hospedadas em seu próprio processo, além do IIS, enquanto ainda usam o poder total das rotas e outros recursos da API Web.
- **Criar páginas de ajuda e teste personalizadas:** Agora você pode criar facilmente páginas de ajuda e teste personalizadas para suas APIs Web usando o novo serviço *IApiExplorer* para obter uma descrição completa do tempo de execução de suas APIs Web.
- **Monitoramento e diagnóstico:** O ASP.NET Web API agora fornece uma infra-estrutura de rastreamento de peso leve que facilita a integração com soluções de registro em log existentes, como System. Diagnostics, ETW e estruturas de registro de terceiros. Você pode habilitar o rastreamento fornecendo uma implementação de *ITraceWriter* e adicionando-o à sua configuração da API Web.
- **Geração de link:** Use o ASP.NET Web API *UrlHelper* para gerar links para recursos relacionados no mesmo aplicativo.
- **Modelo de projeto de API Web:** Selecione o novo projeto de API Web formulário o novo assistente de projeto MVC 4 para entrar em funcionamento rapidamente com ASP.NET Web API.
- **Scaffolding:** Use a caixa de diálogo **Adicionar controlador** para Scaffold rapidamente um controlador de API da Web com base em um tipo de modelo baseado em Entity Framework.

Para obter mais detalhes sobre ASP.NET Web API, visite [https://www.asp.net/web-api](../web-api/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Aprimoramentos para modelos de projeto padrão

O modelo usado para criar novos projetos do ASP.NET MVC 4 foi atualizado para criar um site de aparência mais moderno:

![](mvc4-release-notes/_static/image1.png)

Além das melhorias superficials, há uma funcionalidade aprimorada no novo modelo. O modelo emprega uma técnica chamada processamento adaptável para parecer bom em navegadores de desktops e navegadores móveis sem qualquer personalização.

![](mvc4-release-notes/_static/image2.png)

Para ver a renderização adaptável em ação, você pode usar um emulador móvel ou apenas tentar redimensionar a janela do navegador da área de trabalho para ser menor. Quando a janela do navegador ficar pequena o suficiente, o layout da página será alterado.

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Modelo de projeto móvel

Se você estiver iniciando um novo projeto e quiser criar um site especificamente para navegadores móveis e do Tablet, você poderá usar o novo modelo de projeto de aplicativo móvel. Isso se baseia no jQuery Mobile, uma biblioteca de software livre para a criação da interface do usuário otimizada para toque:

![](mvc4-release-notes/_static/image3.png)

Esse modelo contém a mesma estrutura de aplicativo que o modelo de aplicativo de Internet (e o código do controlador é praticamente idêntico), mas tem o estilo de usar o jQuery Mobile para parecer bom e se comportar bem em dispositivos móveis baseados em toque. Para saber mais sobre como estruturar e estilizar a interface do usuário móvel, consulte o [site do projeto do jQuery Mobile](http://jquerymobile.com/).

Se você já tiver um site orientado à área de trabalho ao qual deseja adicionar exibições otimizadas para mobilidade ou se quiser criar um único site que sirva exibições com estilo diferente para desktops e navegadores móveis, você poderá usar o novo recurso de modos de exibição. (Consulte a próxima seção.)

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Modos de exibição

O novo recurso de modos de exibição permite que um aplicativo selecione exibições dependendo do navegador que está fazendo a solicitação. Por exemplo, se um navegador da área de trabalho solicitar a página inicial, o aplicativo poderá usar o modelo Views\Home\Index.cshtml. Se um navegador móvel solicitar a página inicial, o aplicativo poderá retornar o modelo Views\Home\Index.mobile.cshtml.

Layouts e parciais também podem ser substituídos para determinados tipos de navegador. Por exemplo:

- Se sua pasta Views\Shared contiver os modelos \_layout. cshtml e \_layout. Mobile. cshtml, por padrão, o aplicativo usará o \_layout. Mobile. cshtml durante solicitações de navegadores móveis e \_layout. cshtml durante outras solicitações.
- Se uma pasta contiver \_mypartl. cshtml e \_mypartl. Mobile. cshtml, a instrução @Html.Partial("\_mypartl") será processada \_mypartl. Mobile. cshtml durante as solicitações de navegadores móveis e \_mypartss. cshtml durante outras solicitações.

Se você quiser criar exibições, layouts ou exibições parciais mais específicas para outros dispositivos, poderá registrar uma nova instância *Defaultdisplaymode* para especificar o nome a ser pesquisado quando uma solicitação atender a condições específicas. Por exemplo, você pode adicionar o código a seguir ao *aplicativo\_método Start* no arquivo global. asax para registrar a cadeia de caracteres "iPhone" como um modo de exibição que se aplica quando o navegador Apple iPhone faz uma solicitação:

[!code-csharp[Main](mvc4-release-notes/samples/sample1.cs)]

Depois que esse código for executado, quando um navegador Apple iPhone fizer uma solicitação, seu aplicativo usará o layout Views\Shared\\_Layout. iPhone. cshtml (se existir). Para obter mais informações sobre o modo de exibição, consulte [recursos móveis do ASP.NET MVC 4](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md). Os aplicativos que usam DisplayModeprovider devem instalar o pacote NuGet de [DisplayModes fixos](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) . A [atualização do ASP.net outono 2012](https://go.microsoft.com/fwlink/?LinkID=271322) inclui o pacote NuGet dos [DisplayModes fixos](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) nos novos modelos de projeto. Consulte [ASP.NET MVC 4 Mobile Caching bug corrigido](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) para obter detalhes sobre a correção.

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-and-mobile-features"></a>Recursos do jQuery Mobile e Mobile

Para obter informações sobre como criar aplicativos móveis com o ASP.NET MVC 4 usando o jQuery Mobile, consulte o tutorial [ASP.NET recursos móveis do MVC 4](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md).

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Suporte a tarefas para controladores assíncronos

Agora você pode escrever métodos de ação assíncronas como métodos únicos que retornam um objeto do tipo *Task* ou *task&lt;ActionResult&gt;* .

 Para obter mais informações, consulte [usando métodos assíncronos no ASP.NET MVC 4](../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>SDK do Azure

O ASP.NET MVC 4 dá suporte às versões 1,6 e mais recentes do SDK do Windows Azure.

<a id="_Toc303253818"></a>
### <a name="database-migrations"></a>Migrações de banco de dados

Os projetos do ASP.NET MVC 4 agora incluem Entity Framework 5. Um dos ótimos recursos do Entity Framework 5 é o suporte para migrações de banco de dados. Esse recurso permite que você evolua facilmente seu esquema de banco de dados usando uma migração focada em código, preservando os dados no banco de dado. Para obter mais informações sobre migrações de banco de dados, consulte [adicionando um novo campo ao modelo e à tabela do filme](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table.md) no [tutorial introdução ao ASP.NET MVC 4](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).

<a id="_Toc303253819"></a>
### <a name="empty-project-template"></a>Modelo de projeto vazio

O modelo de projeto do MVC vazio agora está realmente vazio para que você possa começar de um Tablet completamente limpo. A versão anterior do modelo de projeto vazio foi renomeada para básico.

<a id="_Toc303253820"></a>
### <a name="add-controller-to-any-project-folder"></a>Adicionar controlador a qualquer pasta do projeto

Agora você pode clicar com o botão direito do mouse e selecionar **Adicionar controlador** de qualquer pasta em seu projeto MVC. Isso proporciona mais flexibilidade para organizar seus controladores, no entanto, incluindo manter seus controladores de API da Web e MVC em pastas separadas.

<a id="_Toc303253821"></a>
### <a name="bundling-and-minification"></a>Empacotamento e minimização

A estrutura de agrupamento e minificação permite que você reduza o número de solicitações HTTP que uma página da Web precisa fazer combinando arquivos individuais em um único arquivo agrupado para scripts e CSS. Em seguida, ele pode reduzir o tamanho geral dessas solicitações, minificarndo o conteúdo do pacote. O minificar pode incluir atividades como a eliminação de espaço em branco para reduzir nomes de variáveis para até mesmo recolher seletores de CSS com base em sua semântica. Os grupos são declarados e configurados no código e são facilmente referenciados em exibições por meio de métodos auxiliares que podem gerar um único link para o pacote ou, ao depurar, vários links para o conteúdo individual do pacote. Para obter mais informações, consulte [agrupamento e minificação](../mvc/overview/performance/bundling-and-minification.md).

<a id="_Toc303253822"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>Habilitando logons do Facebook e de outros sites usando OAuth e OpenID

Os modelos padrão no modelo de projeto de Internet do ASP.NET MVC 4 agora incluem suporte para logon OAuth e OpenID usando a biblioteca DotNetOpenAuth. Para obter informações sobre como configurar um provedor OAuth ou OpenID, consulte [suporte a OAuth/OpenID para WebForms, MVC e páginas da Web](https://blogs.msdn.com/b/webdev/archive/2012/08/15/oauth-openid-support-for-webforms-mvc-and-webpages.aspx) e a [documentação do recurso OAuth e OpenID no páginas da Web do ASP.net](../web-pages/overview/releases/top-features-in-web-pages-2.md#oauthsetup).

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>Atualizando um projeto do ASP.NET MVC 3 para o ASP.NET MVC 4

O ASP.NET MVC 4 pode ser instalado lado a lado com o ASP.NET MVC 3 no mesmo computador, que oferece flexibilidade na escolha de quando atualizar um aplicativo ASP.NET MVC 3 para o ASP.NET MVC 4.

A maneira mais simples de atualizar é criar um novo projeto ASP.NET MVC 4 e copiar todos os modos de exibição, controladores, código e arquivos de conteúdo do projeto MVC 3 existente para o novo projeto e, em seguida, atualizar as referências de assembly no novo projeto para corresponder a qualquer modelo não MVC no cluded assembiles você está usando. Se você tiver feito alterações no arquivo Web. config no projeto MVC 3, também deverá mesclar essas alterações no arquivo Web. config no projeto MVC 4.

Para atualizar manualmente um aplicativo existente do ASP.NET MVC 3 para a versão 4, faça o seguinte:

1. Em todos os arquivos Web. config do projeto (há um na raiz do projeto, um na pasta views e outro na pasta views de cada área em seu projeto), substitua cada instância do texto a seguir (Observação: System. Web. webpages, Version = 1.0.0.0 não foi encontrado em projetos criados com o Visual Studio 2012): 

    [!code-console[Main](mvc4-release-notes/samples/sample2.cmd)]

    pelo seguinte texto correspondente:

    [!code-console[Main](mvc4-release-notes/samples/sample3.cmd)]
2. No arquivo Web. config raiz, atualize o elemento *webpages: Version* para "2.0.0.0" e adicione uma nova chave *PreserveLoginUrl* que tenha o valor "true": 

    [!code-xml[Main](mvc4-release-notes/samples/sample4.xml)]
3. Em Gerenciador de Soluções, clique com o botão direito do mouse nas referências e selecione gerenciar pacotes NuGet. No painel esquerdo, selecione **Online\NuGet código-fonte oficial**e, em seguida, atualize o seguinte:

    - ASP.NET MVC 4
    - (Opcional) jQuery, validação jQuery e interface do usuário jQuery
    - Adicional Entity Framework
    - (Optonal) Modernizr
4. Em Gerenciador de Soluções, clique com o botão direito do mouse no nome do projeto e selecione descarregar projeto. Em seguida, clique com o botão direito do mouse no nome novamente e selecione Editar *ProjectName*. csproj.
5. Localize o elemento *ProjectTypeGuids* e substitua {E53F8FEA-EAE0-44A6-8774-FFD645390401} por {E3E379DF-F4C6-4180-9B81-6769533ABE47}.
6. Salve as alterações, feche o arquivo de projeto (. csproj) que você estava editando, clique com o botão direito do mouse no projeto e selecione Recarregar projeto.
7. Se o projeto fizer referência a todas as bibliotecas de terceiros que são compiladas usando versões anteriores do ASP.NET MVC, abra o arquivo Web. config raiz e adicione os três elementos *bindingRedirect* a seguir na seção de *configuração* : 

    [!code-xml[Main](mvc4-release-notes/samples/sample5.xml)]

<a id="_Toc303253817"></a>
## <a name="changes-from-aspnet-mvc-4-release-candidate"></a>Alterações do ASP.NET MVC 4 Release Candidate

As notas de versão do ASP.NET MVC 4 Release Candidate podem ser encontradas aqui:

As principais alterações do ASP.NET MVC 4 Release Candidate nesta versão são resumidas abaixo:

- **Configuração por controlador:** Os controladores de ASP.NET Web API podem ser atribuídos com um atributo personalizado que implementa *IControllerConfiguration* para configurar seus próprios formatadores, seletores de ação e vinculadores de parâmetro. O *HttpControllerConfigurationAttribute* foi removido.
- **Por manipuladores de mensagens de rota:** Agora você pode especificar o manipulador de mensagens final na cadeia de solicitações para uma determinada rota. Isso permite que o suporte para estruturas de Ride-the Use o roteamento para expedição para seus próprios pontos de extremidade (não*IHttpControllers*).
- **Notificações de progresso:** O *ProgressMessageHandler* gera notificação de progresso para ambas as entidades de solicitação sendo carregadas e as entidades de resposta que estão sendo baixadas. Usando esse manipulador, é possível controlar a distância com que você está carregando um corpo de solicitação ou baixando um corpo de resposta.
- **Enviar conteúdo por push:** A classe *PushStreamContent* permite cenários em que um produtor de dados deseja gravar diretamente na solicitação ou resposta (de forma síncrona ou assíncrona) usando um fluxo. Quando o *PushStreamContent* está pronto para aceitar dados, ele chama um delegado de ação com o fluxo de saída. Em seguida, o desenvolvedor pode gravar no fluxo pelo tempo necessário e fechar o fluxo quando a gravação for concluída. O *PushStreamContent* detecta o fechamento do fluxo e conclui a *tarefa* assíncrona subjacente para gravar o conteúdo.
- **Criando respostas de erro:** Use o tipo *HttpError* para representar consistentemente informações de erro, como erros de validação e exceções, enquanto ainda respeita o *IncludeErrorDetailPolicy*. Use os novos métodos de extensão *CreateErrorResponse* para criar facilmente respostas de erro com *HttpError* como conteúdo. O conteúdo do *HttpError* é totalmente negociado com conteúdo.
- **MediaRangeMapping removido:** Os intervalos de tipo de mídia agora são tratados pelo conteúdo padrão negociador.
- **A associação de parâmetro padrão para parâmetros de tipo simples agora é [FromUri]:** Em versões anteriores do ASP.NET Web API a associação de parâmetro padrão para parâmetros de tipo simples usavam a associação de modelo. A associação de parâmetro padrão para parâmetros de tipo simples agora é *[FromUri]* .
- A **seleção de ação honra os parâmetros necessários:** A seleção de ação em ASP.NET Web API agora selecionará apenas uma ação se todos os parâmetros necessários provenientes do URI forem fornecidos. Um parâmetro pode ser especificado como opcional fornecendo um valor padrão para o argumento na assinatura do método de ação.
- **Personalizar associações de parâmetro http:** Use o *ParameterBindingAttribute* para personalizar a associação de parâmetro para um parâmetro de ação específico ou use o *ParameterBindingRules* no *HttpConfiguration* para personalizar as associações de parâmetro mais amplamente.
- **Aprimoramentos do MediaTypeFormatter:** Os formatadores agora têm acesso à instância completa do *HttpContent* .
- **Seleção de política de buffer de host:** Implemente e configure o serviço *IHostBufferPolicySelector* no ASP.NET Web API para habilitar hosts para determinar a política para quando o armazenamento em buffer for usado.
- **Acessar certificados de cliente de maneira independente de host:** Use o método de extensão *GetClientCertificate* para obter o certificado de cliente fornecido da mensagem de solicitação.
- **Extensibilidade de negociação de conteúdo:** Personalize a negociação de conteúdo derivando do *DefaultContentNegotiator* e substituindo qualquer aspecto da negociação de conteúdo que você deseja.
- **Suporte para retornar respostas 406 não aceitáveis:** Agora você pode retornar as respostas 406 não aceitáveis em ASP.NET Web API quando um formatador adequado não for encontrado criando um *DefaultContentNegotiator* com o parâmetro *excludeMatchOnTypeOnly* definido como *true*.
- **Ler dados de formulário como NameValueCollection ou JToken:** Você pode ler dados de formulário na cadeia de caracteres de consulta de URI ou no corpo da solicitação como uma *NameValueCollection* usando os métodos de extensão *ParseQueryString* e *ReadAsFormDataAsync* , respectivamente. Da mesma forma, você pode ler dados de formulário na cadeia de caracteres de consulta de URI ou no corpo da solicitação como um *JToken* usando os métodos de extensão *TryReadQueryAsJson* e *ReadAsAsync*&lt;t&gt;, respectivamente.
- **Melhorias de várias partes:** Agora é possível escrever um *MultipartStreamProvider* que seja totalmente adaptado ao tipo de dados de várias partes MIME que pode ler e apresentar o resultado da maneira ideal para o usuário. Você também pode conectar uma etapa de pós-processamento no *MultipartStreamProvider* que permite que a implementação faça qualquer que seja o processamento posterior nas partes do corpo de várias partes MIME. Por exemplo, a implementação de *MultipartFormDataStreamProvider* lê as partes de dados de formulário HTML e as adiciona a uma *NameValueCollection* para que sejam fáceis de obter a partir do chamador.
- **Aprimoramentos de geração de link:** O *UrlHelper* não depende mais do *HttpControllerContext*. Agora você pode acessar o *UrlHelper* de qualquer contexto em que o *HttpRequestMessage* está disponível.
- **Alteração da ordem de execução do manipulador de mensagens:** Os manipuladores de mensagens agora são executados na ordem em que estão configurados em vez de em ordem inversa.
- **Auxiliar para conexão de manipuladores de mensagens:** O novo *HttpClientFactory* que pode conectar *DelegatingHandlers* e criar um *HttpClient* com o pipeline desejado pronto para uso. Ele também fornece a funcionalidade de conexão com manipuladores internos alternativos (o padrão é *HttpClientHandler*), bem como a conexão ao usar o *HttpMessageInvoker* ou outro *DelegatingHandler* em vez de *HttpClient* como o chamador superior.
- **Suporte para CDNs na otimização da Web do ASP.net:** A otimização da Web do ASP.NET agora fornece suporte para caminhos alternativos da CDN, permitindo que você especifique para cada pacote uma URL adicional que aponte para o mesmo recurso em uma rede de distribuição de conteúdo. O suporte a CDNs permite que você obtenha seus pacotes de script e estilo geograficamente mais próximo aos consumidores finais de seus aplicativos Web. Os aplicativos de produção devem implementar um fallback quando a CDN não estiver disponível. Teste o fallback.
- **ASP.NET Web API rotas e a configuração foram movidas para *WebApiConfig. Registre o* método estático que pode ser resuse no código de teste.** ASP.NET Web API rotas anteriormente foram adicionadas em *RouteConfig. RegisterRoutes* juntamente com as rotas MVC padrão. O padrão ASP.NET Web API rotas e configurações agora são manipulados em um método separado *WebApiConfig. Register* para facilitar o teste.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemas conhecidos e alterações recentes

- **A versão RC e RTM do ASP.NET MVC 4 retornou incorretamente exibições de área de trabalho em cache quando as exibições móveis devem ser retornadas.**

    - Consulte [bug do cache móvel do ASP.NET MVC 4 corrigido](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) para obter detalhes sobre a correção. A correção pode ser instalada do pacote NuGet de [DisplayModes fixos](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) .
- **Alterações recentes no mecanismo de exibição do Razor**. Os seguintes tipos foram removidos de *System. Web. Mvc. Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

  Os métodos a seguir também foram removidos: 

    - *MvcCSharpRazorCodeParser. ParseInheritsStatement (System. Web. Razor. Parser. CodeBlockInfo)*
    - *MvcWebPageRazorHost. DecorateCodeGenerator (System. Web. Razor. Generator. RazorCodeGenerator)*
    - *MvcVBRazorCodeParser. ParseInheritsStatement (System. Web. Razor. Parser. CodeBlockInfo)*
- **Quando o WebMatrix. WebData. dll está incluído no diretório/bin de um aplicativo ASP.NET MVC 4, ele assume a URL para a autenticação de formulários.** Adicionar o assembly WebMatrix. WebData. dll ao seu aplicativo (por exemplo, selecionando "Páginas da Web do ASP.NET com a sintaxe do Razor" ao usar a caixa de diálogo Adicionar dependências implantáveis) substituirá o redirecionamento de logon de autenticação para/Account/logon em vez de/Account/Login como esperado pelo controlador de conta padrão do ASP.NET MVC. Para evitar esse comportamento e usar a URL especificada já na seção de autenticação do Web. config, você pode adicionar um appSetting chamado PreserveLoginUrl e defini-lo como true: 

    [!code-xml[Main](mvc4-release-notes/samples/sample6.xml)]
- **O Gerenciador de pacotes NuGet não é instalado ao tentar instalar o ASP.NET MVC 4 para instalações lado a lado do Visual Studio 2010 e do Visual Web Developer 2010.** Para executar o Visual Studio 2010 e o Visual Web Developer 2010 lado a lado com o ASP.NET MVC 4, você deve instalar o ASP.NET MVC 4 depois que ambas as versões do Visual Studio já tiverem sido instaladas.
- **A desinstalação do ASP.NET MVC 4 falhará se os pré-requisitos já tiverem sido desinstalados.** Para desinstalar corretamente o ASP.NET MVC 4you, é necessário desinstalar o ASP.NET MVC 4 antes da desinstalação do Visual Studio.
- **A instalação do ASP.NET MVC 4 interrompe os aplicativos ASP.NET MVC 3 RTM.** Os aplicativos ASP.NET MVC 3 que foram criados com a versão RTM (não com a versão de [atualização das ferramentas do MVC 3 do ASP.net](https://www.microsoft.com/download/details.aspx?id=1491) ) exigem as seguintes alterações para trabalhar lado a lado com o ASP.NET MVC 4. Compilar o projeto sem fazer essas atualizações resulta em erros de compilação. 

    **Atualizações necessárias**

  1. No arquivo Web. config de raiz, adicione um novo *&lt;appsettings&gt;* entrada com a chave *páginas da Web: versão* e o valor *1.0.0.0*. 

      [!code-xml[Main](mvc4-release-notes/samples/sample7.xml)]
  2. Em Gerenciador de Soluções, clique com o botão direito do mouse no nome do projeto e selecione descarregar projeto. Em seguida, clique com o botão direito do mouse no nome novamente e selecione Editar *ProjectName*. csproj.
  3. Localize as seguintes referências de assembly: 

      [!code-xml[Main](mvc4-release-notes/samples/sample8.xml)]

      Substitua-os pelo seguinte:

      [!code-xml[Main](mvc4-release-notes/samples/sample9.xml)]
  4. Salve as alterações, feche o arquivo de projeto (. csproj) que você estava editando e, em seguida, clique com o botão direito do mouse no projeto e selecione Recarregar.

- A **alteração de um projeto do ASP.NET MVC 4 para o destino 4,0 de 4,5 não atualiza a referência do assembly do EntityFramework:** Se você alterar um projeto do ASP.NET MVC 4 para o destino 4,0 depois de voltados 4,5, a referência ao assembly do EntityFramework ainda apontará para a versão 4,5. Para corrigir esse problema, desinstale e reinstale o pacote NuGet do EntityFramework.
- **403 proibido ao executar um aplicativo ASP.NET MVC 4 no Azure após a alteração para o destino 4,0 de 4,5:** Se você alterar um projeto ASP.NET MVC 4 para o destino 4,0 após voltados 4,5 e, em seguida, implantar no Azure, poderá ver um erro 403 proibido no tempo de execução. Para solucionar esse problema, adicione o seguinte ao seu Web. config: `<modules runAllManagedModulesForAllRequests="true" />`
- **O Visual Studio 2012 falha quando você digita um '\' em um literal de cadeia de caracteres em um arquivo Razor.** Para contornar o problema, insira primeiro as aspas de fechamento do literal de cadeia de caracteres.
- <strong>Navegar até &quot;conta/gerenciar&quot; no modelo de Internet resulta em um erro de tempo de execução para idiomas CHS, TRK e CHT.</strong> Para corrigir o problema, modifique a página para separar <em>@User.Identity.Name</em> colocando-o como o único conteúdo dentro da marca <em>&lt;Strong&gt;</em> .
- **Os provedores do Google e do LinkedIn não têm suporte nos sites do Azure.** Use provedores de autenticação alternativos ao implantar em sites do Azure.
- **Ao usar o UriPathExtensionMapping com IIS 8 Express/IIS, você receberá 404 erros não encontrados ao tentar usar a extensão.** O manipulador de arquivo estático interferirá com solicitações para APIs Web que usam *UriPathExtensionMappings*. Defina *runAllManagedModulesForAllRequests = true* em Web. config para contornar o problema.
- **O método Controller. Execute não é mais chamado.** Todos os controladores MVC agora são sempre executados de forma assíncrona.
