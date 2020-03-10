---
uid: visual-studio/overview/2013/release-notes
title: ASP.NET and Web Tools para notas de versão do Visual Studio 2013 | Microsoft Docs
author: microsoft
description: Este documento descreve a versão do ASP.NET and Web Tools para Visual Studio 2013.
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 08815768-2702-42ae-ae85-0a59934a11d1
msc.legacyurl: /visual-studio/overview/2013/release-notes
msc.type: authoredcontent
ms.openlocfilehash: d8af9c8e7ee1316a5eac90c5959d07c628154e09
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557928"
---
# <a name="aspnet-and-web-tools-for-visual-studio-2013-release-notes"></a>Notas de versão do ASP.NET and Web Tools para Visual Studio 2013

pela [Microsoft](https://github.com/microsoft)

> Este documento descreve a versão do ASP.NET and Web Tools para Visual Studio 2013.

## <a name="contents"></a>Conteúdo

- [Notas de instalação](#TOC1)
- [Documentação](#TOC2)
- [Requisitos de software](#TOC4)

### <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Novos recursos no ASP.NET and Web Tools para Visual Studio 2013

- [Um ASP.NET](#TOC6)
- [Nova experiência de projeto Web](#newproj)
- [ASP.NET scaffolding](#scaffold)
- [Link do navegador](#browser-link)
- [Aprimoramentos do editor da Web do Visual Studio](#web-editor)
- [Suporte ao Azure App de aplicativos Web do serviço no Visual Studio](#waws)
- [Aprimoramentos de publicação na Web](#publish)
- [NuGet 2.7](#nuget)
- [Web Forms ASP.NET](#TOC9)
- [ASP.NET MVC 5](#TOC10)
- [ASP.NET Web API 2](#TOC11)
- [ASP.NET SignalR](#TOC13)
- [ASP.NET Identity](#TOC8)
- [Componentes do Microsoft OWIN](#TOC7)
- [Entity Framework 6](#ef6)
- [ASP.NET Razor 3](#TOC14)
- [Suspensão do aplicativo ASP.NET](#TOC15)
- [Problemas conhecidos e alterações recentes](#knownissues)

<a id="TOC1"></a>
## <a name="installation-notes"></a>Notas de instalação

ASP.NET and Web Tools para Visual Studio 2013 são agrupadas no instalador principal e podem ser baixadas [aqui](https://www.asp.net/downloads).

<a id="TOC2"></a>
## <a name="documentation"></a>Documentação

Os tutoriais e outras informações sobre ASP.NET and Web Tools para Visual Studio 2013 estão disponíveis no [site da Web do ASP.net](https://www.asp.net/).

<a id="TOC4"></a>
## <a name="software-requirements"></a>Requisitos de software

ASP.NET and Web Tools requer Visual Studio 2013.

<a id="TOC5"></a>
## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Novos recursos no ASP.NET and Web Tools para Visual Studio 2013

As seções a seguir descrevem os recursos que foram introduzidos na versão.

<a id="TOC6"></a>
## <a name="one-aspnet"></a>Um ASP.NET

Com o lançamento do Visual Studio 2013, fizemos uma etapa para unificar a experiência de usar tecnologias ASP.NETs, para que você possa misturar e corresponder facilmente às que quiser. Por exemplo, você pode iniciar um projeto usando MVC e adicionar facilmente Web Forms páginas ao projeto mais tarde, ou Scaffold APIs Web em um projeto Web Forms. Um ASP.NET é tudo sobre tornar mais fácil para você como desenvolvedor fazer as coisas que você adora no ASP.NET. Independentemente da tecnologia escolhida, você pode ter certeza de que está criando a estrutura subjacente confiável de um ASP.NET.

<a id="newproj"></a>
## <a name="new-web-project-experience"></a>Nova experiência de projeto Web

Aprimoramos a experiência de criação de novos projetos da Web no Visual Studio 2013. Na caixa de diálogo **novo projeto Web ASP.net** , você pode selecionar o tipo de projeto desejado, configurar qualquer combinação de tecnologias (Web Forms, MVC, API da Web), configurar opções de autenticação e adicionar um projeto de teste de unidade.

![Novo projeto ASP.NET](release-notes/_static/image1.png)

A nova caixa de diálogo permite que você altere as opções de autenticação padrão para muitos dos modelos. Por exemplo, ao criar um projeto de Web Forms ASP.NET, você pode selecionar qualquer uma das seguintes opções:

- Sem Autenticação
- Contas de usuário individuais (associação ASP.NET ou logon de provedor social)
- Contas organizacionais (Active Directory em um aplicativo de Internet)
- Autenticação do Windows (Active Directory em um aplicativo de intranet)

![Opções de autenticação](release-notes/_static/image2.png)

Para obter mais informações sobre o novo processo de criação de projetos da Web, consulte [creating ASP.NET Web Projects in Visual Studio 2013](creating-web-projects-in-visual-studio.md). Para obter mais informações sobre as novas opções de autenticação, consulte [ASP.net Identity](#TOC8) mais adiante neste documento.

<a id="scaffold"></a>
## <a name="aspnet-scaffolding"></a>ASP.NET scaffolding

ASP.NET scaffolding é uma estrutura de geração de código para aplicativos ASP.NET Web. Ele facilita a adição de código clichê ao seu projeto que interage com um modelo de dados.

Nas versões anteriores do Visual Studio, o scaffolding era limitado a projetos do ASP.NET MVC. Com Visual Studio 2013, agora você pode usar scaffolding para qualquer projeto ASP.NET, incluindo Web Forms. No momento, o Visual Studio 2013 não dá suporte à geração de páginas para um projeto Web Forms, mas você ainda pode usar o scaffolding com Web Forms adicionando dependências MVC ao projeto. O suporte para gerar páginas para Web Forms será adicionado em uma atualização futura.

Ao usar o scaffolding, garantimos que todas as dependências necessárias sejam instaladas no projeto. Por exemplo, se você começar com um projeto ASP.NET Web Forms e, em seguida, usar scaffolding para adicionar um controlador de API Web, os pacotes e as referências do NuGet necessários serão adicionados automaticamente ao seu projeto.

Para adicionar o MVC scaffolding a um projeto Web Forms, adicione um **novo item com Scaffold** e selecione **dependências do MVC 5** na janela de diálogo. Há duas opções para o scaffolding MVC; Mínimo e completo. Se você selecionar mínimo, somente os pacotes e as referências do NuGet para ASP.NET MVC serão adicionados ao seu projeto. Se você selecionar a opção completa, as dependências mínimas serão adicionadas, bem como os arquivos de conteúdo necessários para um projeto MVC.

O suporte para controladores assíncronos scaffolding usa os novos recursos assíncronos do Entity Framework 6.

Para obter mais informações e tutoriais, consulte [visão geral do ASP.net scaffolding](aspnet-scaffolding-overview.md).

<a id="browser-link"></a>
## <a name="browser-link--signalr-channel-between-browser-and-visual-studio"></a>Link do navegador – canal do Signalr entre o navegador e o Visual Studio

O novo recurso [link do navegador](using-browser-link.md) permite que você conecte vários navegadores ao Visual Studio e atualize todos eles clicando em um botão na barra de ferramentas. Você pode conectar vários navegadores ao seu site de desenvolvimento, incluindo os emuladores móveis, e clicar em atualizar para atualizar todos os navegadores ao mesmo tempo. O link do navegador também expõe uma API para permitir que os desenvolvedores escrevam extensões de link do navegador.

![](release-notes/_static/image3.png)

Ao permitir que os desenvolvedores tirem proveito da API de link do navegador, é possível criar cenários muito avançados que cruzem os limites entre o Visual Studio e qualquer navegador que esteja conectado. O Web Essentials aproveita a API para criar uma experiência integrada entre o Visual Studio e as ferramentas de desenvolvedor do navegador, o controle remoto de emuladores móveis e muito mais.

<a id="web-editor"></a>
## <a name="visual-studio-web-editor-enhancements"></a>Aprimoramentos do editor da Web do Visual Studio

Visual Studio 2013 inclui um novo editor de HTML para arquivos do Razor e arquivos HTML em aplicativos Web. O novo editor de HTML fornece um único esquema unificado baseado em HTML5. Ele tem preenchimento automático de chaves, interface do usuário do jQuery e IntelliSense do atributo AngularJS, agrupamento IntelliSense de atributo, ID e nome de classe IntelliSense e outras melhorias, incluindo melhor desempenho, formatação e marcas inteligentes.

A captura de tela a seguir demonstra o uso do IntelliSense do atributo Bootstrap no editor de HTML.

![IntelliSense no editor de HTML](release-notes/_static/image4.png)

Visual Studio 2013 também vem com CoffeeScript e menos editores internos. O editor LESS vem com todos os recursos interessantes do editor de CSS e tem um IntelliSense específico para variáveis e mesclas em todos os menos documentos na cadeia de @import.

<a id="waws"></a>
## <a name="azure-app-service-web-apps-support-in-visual-studio"></a>Suporte ao Azure App de aplicativos Web do serviço no Visual Studio

No Visual Studio 2013 com o SDK do Azure para .NET 2,2, você pode usar **Gerenciador de servidores** para interagir diretamente com seus aplicativos Web remotos. Você pode entrar em sua conta do Azure, criar novos aplicativos Web, configurar aplicativos, exibir logs em tempo real e muito mais. Logo após o lançamento do SDK 2,2, você poderá executar o no modo de depuração remotamente no Azure. A maioria dos novos recursos para aplicativos Web do Azure App Service também funciona no Visual Studio 2012 quando você instala a versão atual do SDK do Azure para .NET.

Para saber mais, consulte os recursos a seguir:

- [Criar um aplicativo Web ASP.NET no serviço Azure App](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)
- [Solucionar problemas de um aplicativo Web no Serviço de Aplicativo do Azure usando o Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)

<a id="publish"></a>
## <a name="web-publish-enhancements"></a>Aprimoramentos de publicação na Web

O Visual Studio 2013 inclui recursos novos e aprimorados de publicação na Web. Veja algumas opções:

- Automatize facilmente a [criptografia de arquivo Web. config](https://go.microsoft.com/fwlink/?LinkId=325529). (Esse link e o seguinte apontam para a documentação no MSDN que podem não estar disponíveis até o final do dia em 10/17.)
- [Automatize facilmente a colocação de um aplicativo offline durante a implantação](https://go.microsoft.com/fwlink/?LinkId=325530).
- Configure Implantação da Web para [usar a soma de verificação de arquivo em vez da data da última alteração](https://go.microsoft.com/fwlink/?LinkId=325531) para determinar quais arquivos devem ser copiados para o servidor.
- Publique rapidamente arquivos individuais selecionados (incluindo Web. config) quando você estiver usando os métodos de publicação de FTP ou sistema de arquivos, bem como com Implantação da Web.

Para obter mais informações sobre a implantação da Web do ASP.NET, consulte [o site do ASP.net](https://go.microsoft.com/fwlink/?LinkId=322027).

<a id="nuget"></a>
## <a name="nuget-27"></a>NuGet 2.7

O NuGet 2,7 inclui um conjunto avançado de novos recursos que são descritos em detalhes nas [notas de versão do NuGet 2,7](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Esta versão do NuGet também elimina a necessidade de fornecer consentimento explícito para o recurso de restauração de pacote do NuGet para baixar pacotes. O consentimento (e a caixa de seleção associada na caixa de diálogo Preferências do NuGet) agora é concedido pela instalação do NuGet. Agora, a restauração do pacote simplesmente funciona por padrão.

<a id="TOC9"></a>
## <a name="aspnet-web-forms"></a>Web Forms do ASP.NET

### <a name="one-aspnet"></a>Um ASP.NET

Os modelos de projeto Web Forms se integram perfeitamente com a nova experiência de ASP.NET. Você pode adicionar o MVC e o suporte à API Web ao seu projeto Web Forms, e você pode configurar a autenticação usando o assistente de criação de projeto ASP.NET. Para obter mais informações, consulte [Creating ASP.NET Web Projects in Visual Studio 2013](creating-web-projects-in-visual-studio.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Os modelos de projeto Web Forms dão suporte à nova estrutura de ASP.NET Identity. Além disso, os modelos agora dão suporte à criação de um projeto Web Forms intranet. Para obter mais informações, consulte [métodos de autenticação](creating-web-projects-in-visual-studio.md#auth) em **Creating ASP.NET Web projects in Visual Studio 2013**.

### <a name="bootstrap"></a>Inicialização

Os modelos de Web Forms usam o [Bootstrap](http://twitter.github.io/bootstrap/) para fornecer uma aparência elegante e responsiva que você pode personalizar facilmente. Para obter mais informações, consulte [Bootstrap in the Visual Studio 2013 Web Project templates](creating-web-projects-in-visual-studio.md#bootstrap).

<a id="TOC10"></a>
## <a name="aspnet-mvc-5"></a>ASP.NET MVC 5

### <a name="one-aspnet"></a>Um ASP.NET

Os modelos de projeto Web MVC integram-se perfeitamente com a nova experiência de ASP.NET. Você pode personalizar seu projeto MVC e configurar a autenticação usando o assistente de criação de projeto ASP.NET. Um tutorial introdutório do ASP.NET MVC 5 pode ser encontrado em [introdução com o ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

Para obter informações sobre como atualizar projetos MVC 4 para MVC 5, consulte [como atualizar um projeto de API Web e MVC 4 do ASP.net para o ASP.NET MVC 5 e a API da Web 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Os modelos de projeto do MVC foram atualizados para usar ASP.NET Identity para autenticação e gerenciamento de identidades. Um tutorial sobre a autenticação do Facebook e do Google e a nova API Membership podem ser encontrados em [criar um aplicativo ASP.NET MVC 5 com o Facebook e o Google OAuth2 e o logon OpenID](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) e [criar um aplicativo ASP.NET MVC com autenticação e BD SQL e implantar no serviço Azure app](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/).

### <a name="bootstrap"></a>Inicialização

O modelo de projeto do MVC foi atualizado para usar a [inicialização](http://getbootstrap.com/) para fornecer uma aparência elegante e responsiva que você pode personalizar facilmente. Para obter mais informações, consulte [Bootstrap in the Visual Studio 2013 Web Project templates](creating-web-projects-in-visual-studio.md#bootstrap).

### <a name="authentication-filters"></a>Filtros de autenticação

Os filtros de autenticação são um novo tipo de filtro no ASP.NET MVC executado antes dos filtros de autorização no pipeline MVC do ASP.NET e permitem que você especifique a lógica de autenticação por ação, por controlador ou globalmente para todos os controladores. Os filtros de autenticação do processam as credenciais na solicitação e fornecem uma entidade de segurança correspondente. Os filtros de autenticação também podem adicionar desafios de autenticação em resposta a solicitações não autorizadas.

### <a name="filter-overrides"></a>Substituições de filtro

Agora você pode substituir quais filtros se aplicam a um determinado método de ação ou controlador especificando um filtro de substituição. Filtros de substituição especificam um conjunto de tipos de filtro que não devem ser executados para um determinado escopo (ação ou controlador). Isso permite que você configure filtros que se aplicam globalmente, mas, em seguida, exclua certos filtros globais de serem aplicados a ações ou controladores específicos.

### <a name="attribute-routing"></a>Roteamento de atributo

O ASP.NET MVC agora dá suporte ao roteamento de atributos, graças a uma contribuição de Tim McCall, o autor de [http://attributerouting.net](http://attributerouting.net). Com o roteamento de atributos, você pode especificar suas rotas anotando suas ações e controladores.

<a id="TOC11"></a>
## <a name="aspnet-web-api-2"></a>ASP.NET Web API 2

### <a name="attribute-routing"></a>Roteamento de atributo

O ASP.NET Web API agora dá suporte ao roteamento de atributos, graças a uma contribuição de Tim McCall, o autor de [http://attributerouting.net](http://attributerouting.net). Com o roteamento de atributos, você pode especificar suas rotas da API Web anotando suas ações e controladores como este:

[!code-csharp[Main](release-notes/samples/sample1.cs)]

O roteamento de atributos oferece mais controle sobre os URIs em sua API Web. Por exemplo, você pode definir facilmente uma hierarquia de recursos usando um controlador de API único:

[!code-csharp[Main](release-notes/samples/sample2.cs)]

O roteamento de atributos também fornece uma sintaxe conveniente para especificar parâmetros opcionais, valores padrão e restrições de rota:

[!code-csharp[Main](release-notes/samples/sample3.cs)]

Para obter mais informações sobre roteamento de atributos, consulte [Roteamento de atributos na API Web 2](../../../web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2.md).

### <a name="oauth-20"></a>OAuth 2.0

Os modelos de projeto de aplicativo de página única e API Web agora dão suporte à autorização usando OAuth 2,0. O OAuth 2,0 é uma estrutura para autorizar o acesso do cliente a recursos protegidos. Ele funciona para uma variedade de clientes, incluindo navegadores e dispositivos móveis.

O suporte para OAuth 2,0 baseia-se no novo middleware de segurança fornecido pelos componentes do Microsoft OWIN para autenticação de portador e pela implementação da função de servidor de autorização. Como alternativa, os clientes podem ser autorizados usando um servidor de autorização organizacional, como Azure Active Directory ou ADFS no Windows Server 2012 R2.

### <a name="odata-improvements"></a>Aprimoramentos do OData

**Suporte para $select, $expand, $batch e $value**

ASP.NET Web API OData agora tem suporte completo para $select, $expand e $value. Você também pode usar $batch para o processamento em lote de solicitações e para processar conjuntos de alterações.

As opções $select e $expand permitem alterar a forma dos dados retornados de um ponto de extremidade OData. Para obter mais informações, consulte [introducing $Select and $Expand support in Web API OData](../../../web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value.md).

**Extensibilidade aprimorada**

Os formatadores OData agora são extensíveis. Você pode adicionar metadados de entrada de Atom, dar suporte a entradas de link de fluxo e mídia nomeadas, adicionar anotações de instância e personalizar como os links são gerados.

**Suporte sem tipo**

Agora você pode criar serviços OData sem a necessidade de definir tipos CLR para seus tipos de entidade. Em vez disso, os controladores OData podem pegar ou retornar instâncias do **IEdmObject**, que são os formatadores OData serializam/desserializam.

**Reutilizar um modelo existente**

Se você já tiver um EDM (modelo de dados de entidade) existente, agora poderá reutilizá-lo diretamente, em vez de ter que criar um novo. Por exemplo, se você estiver usando Entity Framework, poderá usar o EDM criado pelo EF para você.

### <a name="request-batching"></a>Envio em lote de solicitações

O envio em lote de solicitações combina várias operações em uma única solicitação HTTP POST, para reduzir o tráfego de rede e fornecer uma interface de usuário mais suave e menos informativa. O ASP.NET Web API agora dá suporte a várias estratégias para o envio em lote de solicitações:

- Use o ponto de extremidade $batch de um serviço OData.
- Empacote várias solicitações em uma única solicitação de MIME com várias partes.
- Use um formato de lote personalizado.

Para habilitar o envio em lote de solicitações, basta adicionar uma rota com um manipulador de envio em lote à sua configuração da API Web:

[!code-csharp[Main](release-notes/samples/sample4.cs)]

Você também pode controlar se as solicitações ou são executadas em sequência ou em qualquer ordem.

### <a name="portable-aspnet-web-api-client"></a>Cliente ASP.NET Web API portátil

Agora você pode usar o cliente ASP.NET Web API para criar bibliotecas de classes portáteis que funcionam em seus aplicativos da Windows Store e Windows Phone 8. Você também pode criar formatadores portáteis que podem ser compartilhados entre o cliente e o servidor.

### <a name="improved-testability"></a>Capacidade de teste aprimorada

A API da Web 2 torna muito mais fácil o teste de unidade de seus controladores de API. Basta instanciar seu controlador de API com sua mensagem de solicitação e configuração e, em seguida, chamar o método de ação que você deseja testar. Também é fácil simular a classe **UrlHelper** , para métodos de ação que executam a geração de links.

### <a name="ihttpactionresult"></a>IHttpActionResult

Agora você pode implementar IHttpActionResult para encapsular o resultado de seus métodos de ação da API Web. Um IHttpActionResult retornado de um método de ação da API Web é executado pelo ASP.NET Web API Runtime para produzir a mensagem de resposta resultante. Um IHttpActionResult pode ser retornado de qualquer ação da API Web para simplificar o teste de unidade da sua implementação da API Web. Para conveniência, várias implementações de IHttpActionResult são fornecidas prontamente, incluindo resultados para retornar códigos de status específicos, conteúdo formatado ou respostas negociadas por conteúdo.

### <a name="httprequestcontext"></a>HttpRequestContext

O novo **HttpRequestContext** rastreia qualquer Estado que esteja vinculado à solicitação, mas que não está imediatamente disponível na solicitação. Por exemplo, você pode usar o **HttpRequestContext** para obter dados de rota, a entidade de segurança associada à solicitação, o certificado do cliente, o **UrlHelper** e a raiz do caminho virtual. Você pode criar facilmente um **HttpRequestContext** para fins de teste de unidade.

Como a entidade para a solicitação está fluindo com a solicitação em vez de depender de **thread. CurrentPrincipal**, a entidade de segurança agora está disponível durante todo o tempo de vida da solicitação enquanto estiver no pipeline da API Web.

### <a name="cors"></a>CORS

Graças a outra grande contribuição da Brock Allen, o ASP.NET agora dá suporte total ao CORS (compartilhamento de solicitação entre origens).

A segurança do navegador impede que uma página da Web envie solicitações do AJAX para outro domínio. O [CORS](http://www.w3.org/TR/cors/) é um padrão W3C que permite que um servidor Relaxe a política de mesma origem. Usando o CORS, um servidor pode explicitamente permitir algumas solicitações entre origens e rejeitar outras.

A API da Web 2 agora oferece suporte a CORS, incluindo a manipulação automática de solicitações de simulação. Para saber mais, confira [Permitindo solicitações entre origens na ASP.NET Web API](../../../web-api/overview/security/enabling-cross-origin-requests-in-web-api.md).

### <a name="authentication-filters"></a>Filtros de autenticação

Os filtros de autenticação são um novo tipo de filtro em ASP.NET Web API que são executados antes dos filtros de autorização no pipeline de ASP.NET Web API e permitem que você especifique a lógica de autenticação por ação, por controlador ou globalmente para todos os controladores. Os filtros de autenticação do processam as credenciais na solicitação e fornecem uma entidade de segurança correspondente. Os filtros de autenticação também podem adicionar desafios de autenticação em resposta a solicitações não autorizadas.

### <a name="filter-overrides"></a>Substituições de filtro

Agora você pode substituir quais filtros se aplicam a um determinado método de ação ou controlador, especificando um filtro de substituição. Filtros de substituição especificam um conjunto de tipos de filtro que não devem ser executados para um determinado escopo (ação ou controlador). Isso permite que você adicione filtros globais, mas exclua alguns de ações ou controladores específicos.

### <a name="owin-integration"></a>Integração do OWIN

O ASP.NET Web API agora dá suporte total a OWIN e pode ser executado em qualquer host compatível com OWIN. Também está incluído um **HostAuthenticationFilter** que fornece integração com o sistema de autenticação do OWIN.

Com a integração do OWIN, você pode hospedar automaticamente a API Web em seu próprio processo junto com outros middlewares OWIN, como Signalr. Para obter mais informações, consulte [use OWIN to self-Host ASP.NET Web API](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="TOC13"></a>
## <a name="aspnet-signalr-20"></a>ASP.NET Signalr 2,0

As seções a seguir descrevem os recursos do Signalr 2,0.

- [Criado em OWIN](#builtonowin)
- [MapHubs e MapConnection agora são MapSignalR](#MapSignalR)
- [Suporte entre domínios](#crossdomain)
- [suporte para iOS e Android via MonoTouch e MonoDroid](#mobile)
- [Cliente .NET portátil](#portable)
- [Novo pacote de hospedagem interna](#selfhost)
- [Suporte a servidor compatível com versões anteriores](#backwardcompat)
- [Removido o suporte do servidor para .NET 4,0](#remove40)
- [Enviando uma mensagem para uma lista de clientes e grupos](#messagelist)
- [Enviando uma mensagem para um usuário específico](#sendtouser)
- [Melhor suporte ao tratamento de erros](#errorhandling)
- [Teste de unidade mais fácil de hubs](#unittesting)
- [Tratamento de erro de JavaScript](#javascripterror)

Para obter um exemplo de como atualizar um projeto existente do 1. x para o Signalr 2,0, consulte [atualizando um projeto do signalr 1. x](../../../signalr/overview/releases/upgrading-signalr-1x-projects-to-20.md).

<a id="builtonowin"></a>
### <a name="built-on-owin"></a>Criado em OWIN

O signalr 2,0 foi criado completamente em [OWIN (a interface da Web aberta para .net)](http://owin.org/). Essa alteração torna o processo de configuração para o Signalr muito mais consistente entre aplicativos de Signaler hospedados na Web e hospedados automaticamente, mas também exigia uma série de alterações de API.

<a id="MapSignalR"></a>

### <a name="maphubs-and-mapconnection-are-now-mapsignalr"></a>MapHubs e MapConnection agora são MapSignalR

Para compatibilidade com os padrões OWIN, esses métodos foram renomeados para `MapSignalR`. `MapSignalR` chamado sem parâmetros mapeará todos os hubs (como `MapHubs` na versão 1. x); para mapear objetos **PersistentConnection** individuais, especifique o tipo de conexão como o parâmetro de tipo e a extensão de URL para a conexão como o primeiro argumento.

O método `MapSignalR` é chamado em uma classe de inicialização Owin. Visual Studio 2013 contém um novo modelo para uma classe de inicialização Owin; para usar este modelo, faça o seguinte:

1. Clique com o botão direito do mouse no projeto
2. Selecione **Adicionar**, **novo item...**
3. Selecione a **classe de inicialização Owin**. Nomeie a nova classe **Startup.cs**.

Em um **aplicativo Web,** a classe de inicialização Owin que contém o método `MapSignalR` é adicionada ao processo de inicialização do Owin usando uma entrada no nó configurações do aplicativo do arquivo Web. config, como mostrado abaixo.

Em um **aplicativo auto-hospedado**, a classe de inicialização é passada como o parâmetro de tipo do método `WebApp.Start`.

**Mapeando hubs e conexões no Signalr 1. x (do arquivo de aplicativo global em um aplicativo Web):** 

[!code-csharp[Main](release-notes/samples/sample5.cs)]

**Mapeando hubs e conexões no Signalr 2,0 (de um arquivo de classe de inicialização Owin):** 

[!code-csharp[Main](release-notes/samples/sample6.cs)]

Em um **aplicativo auto-hospedado**, a classe de inicialização é passada como o parâmetro de tipo para o método `WebApp.Start`, conforme mostrado abaixo.

[!code-csharp[Main](release-notes/samples/sample7.cs)]

<a id="crossdomain"></a>

### <a name="cross-domain-support"></a>Suporte entre domínios

No Signalr 1. x, as solicitações entre domínios foram controladas por um único sinalizador EnableCrossDomain. Esse sinalizador controlava as solicitações JSONP e CORS. Para maior flexibilidade, todo o suporte a CORS foi removido do componente de servidor do Signalr (os clientes JavaScript ainda usam CORS normalmente se detectar que o navegador dá suporte a ele) e o novo middleware OWIN foi disponibilizado para dar suporte a esses cenários.

No Signalr 2,0, se JSONP for necessário no cliente (para dar suporte a solicitações entre domínios em navegadores mais antigos), será necessário habilitá-lo explicitamente definindo `EnableJSONP` no objeto `HubConfiguration` como `true`, conforme mostrado abaixo. O JSONP é desabilitado por padrão, pois é menos seguro do que o CORS.

Para adicionar o novo middleware de CORS no Signalr 2,0, adicione a biblioteca de `Microsoft.Owin.Cors` ao seu projeto e chame `UseCors` antes do middleware Signalr, conforme mostrado na seção abaixo.

**Adicionando Microsoft. Owin. CORS ao seu projeto**: para instalar essa biblioteca, execute o seguinte comando no console do Gerenciador de pacotes:

[!code-powershell[Main](release-notes/samples/sample8.ps1)]

Este comando adicionará a versão 2.0.0 do pacote ao seu projeto.

**Chamando UseCors**

Os trechos de código a seguir demonstram como implementar conexões entre domínios no Signalr 1. x e 2,0.

**Implementando solicitações entre domínios no Signalr 1. x (do arquivo de aplicativo global)**

[!code-csharp[Main](release-notes/samples/sample9.cs)]

**Implementando solicitações entre domínios no Signalr 2,0 (a partir C# de um arquivo de código)**

O código a seguir demonstra como habilitar CORS ou JSONP em um projeto do Signalr 2,0. Este exemplo de código usa `Map` e `RunSignalR` em vez de `MapSignalR`, para que o middleware do CORS seja executado somente para solicitações do Signalr que exigem suporte a CORS (em vez de para todo o tráfego no caminho especificado em `MapSignalR`). `Map` também pode ser usado para qualquer outro middleware que precise ser executado para um prefixo de URL específico, em vez de para todo o aplicativo.

[!code-csharp[Main](release-notes/samples/sample10.cs)]

<a id="mobile"></a>

### <a name="ios-and-android-support-via-monotouch-and-monodroid"></a>suporte para iOS e Android via MonoTouch e MonoDroid

Foi adicionado suporte para clientes iOS e Android usando componentes MonoTouch e MonoDroid da biblioteca do [Xamarin](https://xamarin.com/). Para obter mais informações sobre como usá-los, consulte [usando componentes do Xamarin](https://github.com/SignalR/SignalR/wiki/Building-Mono.Mobile.sln). Esses componentes estarão disponíveis na loja do [Xamarin](https://store.xamarin.com/) quando a versão do signalr RTW estiver disponível.

<a id="portable"></a># # # Cliente .NET portátil

Para facilitar o desenvolvimento de várias plataformas, os clientes Silverlight, WinRT e Windows Phone foram substituídos por um único cliente .NET portátil que dá suporte às seguintes plataformas:

- REDE 4,5
- Silverlight 5
- WinRT (.NET para aplicativos da Windows Store)
- Windows Phone 8

<a id="selfhost"></a>

### <a name="new-self-host-package"></a>Novo pacote de hospedagem interna

Agora há um pacote NuGet para facilitar a introdução ao auto-host do Signalr (aplicativos de Signalr que são hospedados em um processo ou outro aplicativo, em vez de serem hospedados em um servidor Web). Para atualizar um projeto de hospedagem interna criado com o Signalr 1. x, remova o pacote Microsoft. AspNet. Signalr. Owin e adicione o pacote Microsoft. AspNet. Signalr. SelfHost. Para obter mais informações sobre como começar a usar o pacote de hospedagem interna, consulte [tutorial: auto-host do signalr](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="backwardcompat"></a>

### <a name="backward-compatible-server-support"></a>Suporte a servidor compatível com versões anteriores

Nas versões anteriores do Signalr, as versões do pacote do Signalr usadas no cliente e no servidor precisavam ser idênticas. Para oferecer suporte a aplicativos cliente espesso que seriam difíceis de atualizar, o Signalr 2,0 agora dá suporte ao uso de uma versão de servidor mais recente com um cliente mais antigo. **Observação: o Signalr 2,0 não oferece suporte a servidores criados com versões mais antigas com clientes mais recentes.**

<a id="remove40"></a>

### <a name="removed-server-support-for-net-40"></a>Removido o suporte do servidor para .NET 4,0

O signalr 2,0 descartou o suporte para a interoperabilidade do servidor com o .NET 4,0. O .NET 4,5 deve ser usado com servidores Signalr 2,0. Ainda há um cliente .NET 4,0 para o Signalr 2,0.

<a id="messagelist"></a>

### <a name="sending-a-message-to-a-list-of-clients-and-groups"></a>Enviando uma mensagem para uma lista de clientes e grupos

No Signalr 2,0, é possível enviar uma mensagem usando uma lista de IDs de cliente e grupo. Os trechos de código a seguir demonstram como fazer isso.

**Enviando uma mensagem para uma lista de clientes e grupos usando o PersistentConnection**

[!code-csharp[Main](release-notes/samples/sample11.cs)]

**Enviando uma mensagem para uma lista de clientes e grupos usando hubs**

[!code-csharp[Main](release-notes/samples/sample12.cs)]

<a id="sendtouser"></a>

### <a name="sending-a-message-to-a-specific-user"></a>Enviando uma mensagem para um usuário específico

Esse recurso permite que os usuários especifiquem o que a userId se baseia em um IRequest por meio de uma nova interface IUserIdProvider:

**A interface IUserIdProvider**

[!code-csharp[Main](release-notes/samples/sample13.cs)]

Por padrão, haverá uma implementação que usa o IPrincipal.Identity.Name do usuário como o nome de usuário.

Em hubs, você poderá enviar mensagens para esses usuários por meio de uma nova API:

**Usando a API clients. User**

[!code-csharp[Main](release-notes/samples/sample14.cs)]

<a id="errorhandling"></a>

### <a name="better-error-handling-support"></a>Melhor suporte ao tratamento de erros

Agora, os usuários podem lançar **HubException** de qualquer invocação de Hub. O construtor do **HubException** pode pegar uma mensagem de cadeia de caracteres e um objeto de dados de erro extra. O signalr irá serializar automaticamente a exceção e enviá-la para o cliente, onde será usada para rejeitar/reprovar a invocação do método de Hub.

A configuração **Mostrar exceções de Hub detalhadas** não tem nenhuma influência sobre **HubException** sendo enviada de volta ao cliente ou não; Ele é sempre enviado.

**Código do lado do servidor demonstrando o envio de um HubException para o cliente**

[!code-csharp[Main](release-notes/samples/sample15.cs)]

**Código de cliente JavaScript que demonstra a resposta a um HubException enviado do servidor**

[!code-html[Main](release-notes/samples/sample16.html)]

**Código do cliente .NET que demonstra a resposta a um HubException enviado do servidor**

[!code-csharp[Main](release-notes/samples/sample17.cs)]

<a id="unittesting"></a>

### <a name="easier-unit-testing-of-hubs"></a>Teste de unidade mais fácil de hubs

O signalr 2,0 inclui uma interface chamada `IHubCallerConnectionContext` em hubs que facilita a criação de invocações do lado do cliente fictícios. Os trechos de código a seguir demonstram como usar essa interface com os [xUnit.net](https://github.com/xunit/xunit) e [MOQ](https://code.google.com/p/moq/)de teste populares.

**Sinalizador de teste de unidade com xUnit.net**

[!code-csharp[Main](release-notes/samples/sample18.cs)]

**Sinalizador de teste de unidade com MOQ**

[!code-csharp[Main](release-notes/samples/sample19.cs)]

<a id="javascripterror"></a>

### <a name="javascript-error-handling"></a>Tratamento de erro de JavaScript

No Signalr 2,0, todos os retornos de chamada de tratamento de erros JavaScript retornam objetos de erro JavaScript em vez de cadeias de caracteres brutas Isso permite que o Signalr flua informações mais avançadas para seus manipuladores de erro. Você pode obter a exceção interna da propriedade `source` do erro.

**Código de cliente JavaScript que manipula a exceção de iniciar. Fail**

[!code-javascript[Main](release-notes/samples/sample20.js)]

<a id="TOC8"></a>
## <a name="aspnet-identity"></a>ASP.NET Identity

### <a name="new-aspnet-membership-system"></a>Novo sistema de associação do ASP.NET

ASP.NET Identity é o novo sistema de associação para aplicativos ASP.NET. ASP.NET Identity facilita a integração de dados de perfil específicos do usuário com os dados do aplicativo. ASP.NET Identity também permite que você escolha o modelo de persistência para perfis de usuário em seu aplicativo. Você pode armazenar os dados em um banco de dados de SQL Server ou em outro repositório, incluindo armazenamentos de dados NoSQL, como tabelas de armazenamento do Azure. Para obter mais informações, consulte [contas de usuário individuais](creating-web-projects-in-visual-studio.md#indauth) em **criando projetos Web ASP.net no Visual Studio 2013**.

### <a name="claims-based-authentication"></a>Autenticação baseada em declarações

O ASP.NET agora dá suporte à autenticação baseada em declarações, em que a identidade do usuário é representada como um conjunto de declarações de um emissor confiável. Os usuários podem ser autenticados usando um nome de usuário e senha mantidos em um banco de dados de aplicativo ou usando provedores de identidade social (por exemplo: contas da Microsoft, Facebook, Google, Twitter) ou usando contas organizacionais por meio de Azure Active Directory ou Serviços de Federação do Active Directory (AD FS) (ADFS).

### <a name="integration-with-azure-active-directory-and-windows-server-active-directory"></a>Integração com o Azure Active Directory e o Windows Server Active Directory

Agora você pode criar projetos do ASP.NET que usam o Azure Active Directory ou o Windows Server Active Directory (AD) para autenticação. Para obter mais informações, consulte [contas organizacionais](creating-web-projects-in-visual-studio.md#orgauth) em **Creating ASP.NET Web projects in Visual Studio 2013**.

### <a name="owin-integration"></a>Integração do OWIN

A autenticação ASP.NET agora é baseada no middleware OWIN que pode ser usado em qualquer host baseado em OWIN. Para obter mais informações sobre o OWIN, consulte a seção de [componentes do Microsoft OWIN](#TOC7) a seguir.

<a id="TOC7"></a>
## <a name="microsoft-owin-components"></a>Componentes do Microsoft OWIN

O [Open Web interface for .net](http://owin.org/) (OWIN) define uma abstração entre servidores Web e aplicativos Web do .net. OWIN dissocia o aplicativo Web do servidor, tornando os aplicativos Web independentes de host. Por exemplo, você pode hospedar um aplicativo Web baseado em OWIN no IIS ou hospedá-lo internamente em um processo personalizado.

As alterações introduzidas nos componentes do Microsoft OWIN (também conhecidos como o projeto Katana) incluem novos componentes de host e servidor, novas bibliotecas auxiliares e middleware e novo middleware de autenticação.

Para obter mais informações sobre OWIN e Katana, consulte [What ' s New in OWIN and Katana](../../../aspnet/overview/owin-and-katana/index.md).

**Observação: os aplicativos [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) não podem ser executados no modo clássico do IIS; Eles devem ser executados no modo integrado.**

**Observação: os aplicativos [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) devem ser executados em confiança total.**

### <a name="new-servers-and-hosts"></a>Novos servidores e hosts

Com esta versão, novos componentes foram adicionados para habilitar cenários de hospedagem interna. Esses componentes incluem os seguintes pacotes NuGet:

- **Microsoft. Owin. host. HttpListener**. Fornece um servidor OWIN que usa **HttpListener** para escutar solicitações HTTP e direcioná-las para o pipeline OWIN.
- **Microsoft. Owin. Hosting** fornece uma biblioteca para desenvolvedores que desejam hospedar automaticamente um pipeline do Owin em um processo personalizado, como um aplicativo de console ou um serviço do Windows.
- **OwinHost**. Fornece um executável autônomo que encapsula `Microsoft.Owin.Hosting` e permite que você hospede automaticamente um pipeline do OWIN sem precisar escrever um aplicativo host personalizado.

Além disso, o pacote de `Microsoft.Owin.Host.SystemWeb` agora permite que o middleware forneça dicas para o servidor **SystemWeb** , indicando que o middleware deve ser chamado durante um estágio de pipeline ASP.net específico. Esse recurso é particularmente útil para middleware de autenticação, que deve ser executado no início do pipeline ASP.NET.

### <a name="helper-libraries-and-middleware"></a>Bibliotecas auxiliares e middleware

Embora você possa escrever componentes OWIN usando apenas as definições de função e tipo da especificação OWIN, o novo pacote de `Microsoft.Owin` fornece um conjunto de abstrações mais amigáveis para o usuário. Esse pacote combina vários pacotes anteriores (por exemplo, `Owin.Extensions`, `Owin.Types`) em um único modelo de objeto bem estruturado que pode ser facilmente usado por outros componentes do OWIN. Na verdade, a maioria dos componentes do Microsoft OWIN agora usa esse pacote.

> [!NOTE]
> Os aplicativos [OWIN](http://www.owin.org) não podem ser executados no modo clássico do IIS; Eles devem ser executados no modo integrado.

> [!NOTE]
> Os aplicativos [OWIN](http://www.owin.org) devem ser executados em confiança total.

Esta versão também inclui o pacote Microsoft. Owin. Diagnostics, que inclui middleware para validar um aplicativo OWIN em execução, além do middleware de página de erro para ajudar a investigar falhas.

### <a name="authentication-components"></a>Componentes de autenticação

Os seguintes componentes de autenticação estão disponíveis.

- **Microsoft. Owin. Security. ActiveDirectory**. Habilita a autenticação usando serviços de diretório locais ou baseados em nuvem.
- **Microsoft. Owin. Security. cookies** habilita a autenticação usando cookies. Este pacote foi chamado anteriormente de `Microsoft.Owin.Security.Forms`.
- **Microsoft. Owin. Security. Facebook** habilita a autenticação usando o serviço baseado em OAuth do Facebook.
- **Microsoft. Owin. Security. Google** habilita a autenticação usando o serviço baseado em OpenID do Google.
- **Microsoft. Owin. Security. JWT** habilita a autenticação usando tokens JWT.
- **Microsoft. Owin. Security. MicrosoftAccount** permite a autenticação usando contas da Microsoft.
- **Microsoft. Owin. Security. OAuth**. Fornece um servidor de autorização OAuth, bem como middleware para autenticar tokens de portador.
- **Microsoft. Owin. Security. Twitter** habilita a autenticação usando o serviço baseado em OAuth do Twitter.

Esta versão também inclui o pacote `Microsoft.Owin.Cors`, que contém middleware para processar solicitações HTTP entre origens.

> [!NOTE]
> O suporte para assinatura JWT foi removido na versão final do Visual Studio 2013.

<a id="ef6"></a>
## <a name="entity-framework-6"></a>Entity Framework 6

Para obter uma lista dos novos recursos e outras alterações no Entity Framework 6, consulte [Entity Framework histórico de versões](https://msdn.com/data/jj574253).

<a id="TOC14"></a>
## <a name="aspnet-razor-3"></a>ASP.NET Razor 3

O ASP.NET Razor 3 inclui os seguintes novos recursos:

- Suporte para edição de guias. Anteriormente, o comando **Formatar documento** , recuo automático e formatação automática no Visual Studio não funcionavam corretamente ao usar a opção **manter guias** . Essa alteração corrige a formatação do Visual Studio para o código Razor para formatação de tabulação.
- Suporte para regras de reescrita de URL ao gerar links.
- Remoção do atributo transparente de segurança.
  > [!NOTE]
  > Essa é uma alteração significativa e torna o Razor 3 incompatível com o MVC4 e anterior, enquanto o Razor 2 é incompatível com MVC5 ou assemblies compilados em MVC5.

Os problemas do Razor 3 corrigidos em Visual Studio 2013 de versões de pré-lançamento podem ser encontrados [aqui](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Resolved%7cClosed&type=All&priority=All&release=All%7cv5.0%2bPreview%7cv5.0%2bRC%7cv5.0%2bRTM&assignedTo=All&component=Web%2bPages%252fRazor&reasonClosed=Fixed&sortField=LastUpdatedDate&sortDirection=Descending&page=0).

<a id="TOC15"></a>
## <a name="aspnet-app-suspend"></a>Suspensão do aplicativo ASP.NET

O ASP.NET app Suspend é um recurso que muda de jogo no .NET Framework 4.5.1 que altera radicalmente a experiência do usuário e o modelo econômico para hospedar grandes números de sites ASP.NET em um único computador. Para obter mais informações, consulte [ASP.net app Suspend – responsivo .NET Web Hosting compartilhado](https://blogs.msdn.com/b/dotnet/archive/2013/10/09/asp-net-app-suspend-responsive-shared-net-web-hosting.aspx).

<a id="knownissues"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemas conhecidos e alterações recentes

Esta seção descreve problemas conhecidos e alterações recentes na ASP.NET and Web Tools para Visual Studio 2013.

### <a name="nuget"></a>NuGet

- A [nova restauração de pacote não funciona no mono ao usar o arquivo sln](https://nuget.codeplex.com/workitem/3596) – será corrigida em um futuro download do NuGet. exe e atualização do [pacote NuGet. CommandLine](http://www.nuget.org/packages/NuGet.CommandLine/) .
- A [nova restauração do pacote não funciona com projetos do WiX](https://nuget.codeplex.com/workitem/3598) – será corrigida em um futuro download do NuGet. exe e atualização do [pacote NuGet. CommandLine](http://www.nuget.org/packages/NuGet.CommandLine/) .
- A [restauração automática de pacote não funciona para projetos em uma pasta de solução](https://nuget.codeplex.com/workitem/3625) – será corrigida no NuGet 2,8.

### <a name="aspnet-web-api"></a>ASP.NET Web API

1. `ODataQueryOptions<T>.ApplyTo(IQueryable)` não retorna `IQueryable<T>` sempre, à medida que adicionamos suporte para `$select` e `$expand`.

    Nossos exemplos anteriores para `ODataQueryOptions<T>` sempre converteram o valor de retorno de `ApplyTo` para `IQueryable<T>`. Isso funcionou anteriormente porque as opções de consulta com suporte anteriormente (`$filter`, `$orderby`, `$skip``$top`) não alteram a forma da consulta. Agora que damos suporte a `$select` e `$expand` o valor de retorno de `ApplyTo` não será `IQueryable<T>` sempre.

    [!code-csharp[Main](release-notes/samples/sample21.cs)]

    Se você estiver usando o código de exemplo anterior, ele continuará funcionando se o cliente não enviar `$select` e `$expand`. No entanto, se você quiser dar suporte a `$select` e `$expand` você precisa alterar esse código para isso.

    [!code-csharp[Main](release-notes/samples/sample22.cs)]
2. **Request. URL ou RequestContext. URL é nulo durante uma solicitação em lote**

    Em um cenário de envio em lote, **UrlHelper** é nulo quando acessado de **Request. URL** ou **RequestContext. URL**.

    Esse problema é rastreado atualmente aqui: [BatchRequestContext. URL é nulo para solicitação de envio em lote](http://aspnetwebstack.codeplex.com/workitem/1301).

    A solução alternativa para esse problema é criar uma nova instância de **UrlHelper**, como no exemplo a seguir:

    **Criando uma nova instância de UrlHelper**

    [!code-csharp[Main](release-notes/samples/sample23.cs)]

### <a name="aspnet-mvc"></a>ASP.NET MVC

1. Ao usar MVC5 e OrgAuth, se você tiver exibições que fazem validação de AntiForgerToken, você poderá vir ao longo do seguinte erro ao postar dados para a exibição:

    **Erro**:

    *Erro de servidor no aplicativo '/'.*

    <em>Uma declaração do tipo '<http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier>' ou '<https://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider>' não estava presente no ClaimsIdentity fornecido. Para habilitar o suporte a token antifalsificação com autenticação baseada em declarações, verifique se o provedor de declarações configurado está fornecendo essas duas declarações nas instâncias de ClaimsIdentity que ele gera. Se o provedor de declarações configurado usar um tipo de declaração diferente como um identificador exclusivo, ele poderá ser configurado definindo a propriedade estática AntiForgeryConfig. UniqueClaimTypeIdentifier.</em>

    **Solução alternativa**:

    Adicione a seguinte linha em global. asax para corrigi-lo:

    `AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.Name;`

    Isso será corrigido para a próxima versão.
2. Depois de atualizar um aplicativo MVC4 para o MVC5, Compile a solução e inicie-a. Você deverá ver o seguinte erro:

    Um System. Web. webpages. Razor. Configuration. HostSection não pode ser convertido em [B] System. Web. webpages. Razor. Configuration. HostSection. Digite A origem de ' System. Web. webpages. Razor, Version = 2.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35 ' no contexto ' default ' no local ' C:\windows\Microsoft.Net\assembly\GAC\_MSIL\System.Web.WebPages.Razor\v4.0\_2.0.0.0\_\_31bf3856ad364e35 \ System. Web. webpages. Razor. dll '. O tipo B tem origem em ' System. Web. webpages. Razor, versão = 3.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35 ' no contexto ' default ' no local ' C:\Windows\Microsoft.NET\Framework\v4.0.30319\Temporary ASP.NET Files\root\6d05bbd0\e8b5908e\assembly\dl3\c9cbca63\f8910382\_6273ce01 \ System. Web. webpages. Razor. dll '.

    Para corrigir o erro acima, abra *todos* os arquivos Web. config (incluindo aqueles na pasta views) em seu projeto e faça o seguinte:

   1. Atualize todas as ocorrências da versão "4.0.0.0" de "System. Web. Mvc" para "5.0.0.0".
   2. Atualize todas as ocorrências da versão "2.0.0.0" de "System. Web. Helpers", &quot;System. Web. webpages&quot; e &quot;System. Web. webpages. Razor&quot; para "3.0.0.0"

      Por exemplo, depois de fazer as alterações acima, as associações de assembly devem ter a seguinte aparência:

      [!code-xml[Main](release-notes/samples/sample24.xml)]

      Para obter informações sobre como atualizar projetos MVC 4 para MVC 5, consulte [como atualizar um projeto de API Web e MVC 4 do ASP.net para o ASP.NET MVC 5 e a API da Web 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).
3. Ao usar a validação do lado do cliente com a validação não invasiva do jQuery, a mensagem de validação, às vezes, é incorreta para um elemento de entrada HTML com tipo = ' número '. O erro de validação para um valor necessário ("o campo idade é obrigatório") é mostrado quando um número inválido é inserido em vez da mensagem correta de que um número válido é necessário.

    Esse problema geralmente é encontrado com o código com Scaffold para um modelo com uma propriedade Integer nas exibições criar e editar.

    Para contornar esse problema, altere o auxiliar do editor de:

    `@Html.EditorFor(person => person.Age)`

    Para:

    `@Html.TextBoxFor(person => person.Age)`
4. O ASP.NET MVC 5 não dá mais suporte à confiança parcial. Os projetos que se vinculam aos binários MVC ou WebAPI devem remover o atributo [SecurityTransparent](https://msdn.microsoft.com/library/system.security.securitytransparentattribute.aspx) e o atributo [AllowPartiallyTrustedCallers](https://msdn.microsoft.com/library/system.security.allowpartiallytrustedcallersattribute.aspx) . A remoção desses atributos eliminará erros do compilador, como o seguinte.

    `Attempt by security transparent method ‘MyComponent' to access security critical type 'System.Web.Mvc.MvcHtmlString' failed. Assembly 'PagedList.Mvc, Version=4.3.0.0, Culture=neutral, PublicKeyToken=abbb863e9397c5e1' is marked with the AllowPartiallyTrustedCallersAttribute, and uses the level 2 security transparency model. Level 2 transparency causes all methods in AllowPartiallyTrustedCallers assemblies to become security transparent by default, which may be the cause of this exception.`

    > Observe que, como um efeito colateral disso, você não pode usar assemblies 4,0 e 5,0 no mesmo aplicativo. Você precisa atualizar todos eles para 5,0.

### <a name="spa-template-with-facebook-authorization-may-cause-instability-in-ie-while-the-web-site-is-hosted-in-intranet-zone"></a>O modelo SPA com autorização do Facebook pode causar instabilidade no IE enquanto o site está hospedado na zona da intranet

O modelo SPA fornece logon externo com o Facebook. Quando o projeto criado com o modelo está em execução localmente, entrar pode fazer com que o IE falhe.

Solução:

1. Hospedar o site na zona da Internet; or

2. Teste o cenário em um navegador que não seja o IE.

### <a name="web-forms-scaffolding"></a>Scaffolding de Web Forms

Web Forms scaffolding foi removido do VS2013 e estará disponível em uma atualização futura do Visual Studio. No entanto, você ainda pode usar o scaffolding em um projeto Web Forms adicionando dependências MVC e gerando scaffolding para MVC. Seu projeto conterá uma combinação de Web Forms e MVC.

Para adicionar o MVC ao seu projeto Web Forms, adicione um novo item com Scaffold e selecione **dependências do MVC 5**. Selecione mínimo ou completo, dependendo se você precisa de todos os arquivos de conteúdo, como scripts. Em seguida, adicione um item com Scaffold para MVC, que criará exibições e um controlador em seu projeto.

### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC e API Web scaffolding-HTTP 404, erro não encontrado

Se for encontrado um erro ao adicionar um item com Scaffold a um projeto, será possível que seu projeto seja deixado em um estado inconsistente. Algumas das alterações feitas no scaffolding serão revertidas, mas outras alterações, como os pacotes NuGet instalados, não serão revertidas. Se as alterações de configuração de roteamento forem revertidas, os usuários receberão um erro HTTP 404 ao navegar para itens com Scaffold.

Solução alternativa:

- Para corrigir esse erro para o MVC, adicione um novo item com Scaffold e selecione dependências MVC 5 (mínima ou completa). Esse processo adicionará todas as alterações necessárias ao seu projeto.
- Para corrigir esse erro para a API Web:

  1. Adicione a classe WebApiConfig ao seu projeto.

      [!code-csharp[Main](release-notes/samples/sample25.cs)]

      [!code-vb[Main](release-notes/samples/sample26.vb)]
  2. Configure WebApiConfig. Register no aplicativo\_o método Start no global. asax da seguinte maneira:

      [!code-csharp[Main](release-notes/samples/sample27.cs)]

      [!code-vb[Main](release-notes/samples/sample28.vb)]
