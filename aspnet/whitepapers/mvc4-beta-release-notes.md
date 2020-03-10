---
uid: whitepapers/mvc4-beta-release-notes
title: ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: Este documento descreve o lançamento do ASP.NET MVC 4 beta para Visual Studio 2010.
ms.author: riande
ms.date: 09/09/2011
ms.assetid: 666407bb-81de-4319-89ba-0302c382a208
msc.legacyurl: /whitepapers/mvc4-beta-release-notes
msc.type: content
ms.openlocfilehash: 17800dfe091bbb7afb25f7f41e3bd885b882edb0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78523299"
---
# <a name="aspnet-mvc-4"></a>ASP.NET MVC 4

> Este documento descreve o lançamento do ASP.NET MVC 4 beta para Visual Studio 2010.
> 
> > [!NOTE]
> > Essa não é a versão mais atual. As notas de versão do ASP.NET MVC 4 RC estão disponíveis [aqui](mvc4-release-notes.md).

- [Notas de instalação](#_Toc303253802)
- [Documentação](#_Toc303253803)
- [Suporte](#_Toc303253804)
- [Requisitos de software](#_Toc303253805)
- [Atualizando um projeto do ASP.NET MVC 3 para o ASP.NET MVC 4](#_Toc303253806)
- [Novos recursos no ASP.NET MVC 4 beta](#_Toc303253807)

    - [ASP.NET Web API](#_Toc317096197)
    - [Aplicativo de página única do ASP.NET](#_Toc317096198)
    - [Aprimoramentos para modelos de projeto padrão](#_Toc303253808)
    - [Modelo de projeto móvel](#_Toc303253809)
    - [Modos de exibição](#_Toc303253810)
    - [jQuery Mobile, o seletor de exibição e a substituição do navegador](#_Toc303253811)
    - [Receitas para geração de código no Visual Studio](#_Toc303253812)
    - [Suporte a tarefas para controladores assíncronos](#_Toc303253813)
    - [SDK do Azure](#_Toc303253814)
    - [Problemas conhecidos e alterações recentes](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Notas de instalação

O ASP.NET MVC 4 beta para Visual Studio 2010 pode ser instalado por meio do [ASP.NET MVC 4 Home Page](../mvc/mvc4.md) usando o Web Platform Installer.

Você deve desinstalar todas as visualizações previamente instaladas do ASP.NET MVC 4 antes de instalar o ASP.NET MVC 4 beta.

Esta versão não é compatível com a visualização do .NET Framework Developer 4,5. Você deve desinstalar o .NET 4,5 Developer Preview antes de instalar o ASP.NET MVC 4 beta.

O ASP.NET MVC 4 pode ser instalado e pode ser executado lado a lado com o ASP.NET MVC 3.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Documentação

A documentação do ASP.NET MVC está disponível no site do MSDN na seguinte URL:

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

Os tutoriais e outras informações sobre o ASP.NET MVC estão disponíveis na página MVC 4 do site do ASP.NET ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).

<a id="_Toc303253804"></a>
## <a name="support"></a>Suporte

Esta é uma versão de visualização e não é oficialmente suportada. Se você tiver dúvidas sobre como trabalhar com esta versão, poste-as no ASP.NET MVC Forum ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), em que os membros da comunidade do ASP.net freqüentemente podem fornecer suporte informal.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Requisitos de software

Os componentes do ASP.NET MVC 4 para Visual Studio exigem o PowerShell 2,0 e o Visual Studio 2010 com Service Pack 1 ou o Visual Web Developer Express 2010 com Service Pack 1.

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>Atualizando um projeto do ASP.NET MVC 3 para o ASP.NET MVC 4

O ASP.NET MVC 4 pode ser instalado lado a lado com o ASP.NET MVC 3 no mesmo computador, que oferece flexibilidade na escolha de quando atualizar um aplicativo ASP.NET MVC 3 para o ASP.NET MVC 4.

A maneira mais simples de atualizar é criar um novo projeto ASP.NET MVC 4 e copiar todos os modos de exibição, controladores, código e arquivos de conteúdo do projeto MVC 3 existente para o novo projeto e, em seguida, atualizar as referências de assembly no novo projeto para corresponder ao projeto antigo. Se você tiver feito alterações no arquivo Web. config no projeto MVC 3, também deverá mesclar essas alterações no arquivo Web. config no projeto MVC 4.

Para atualizar manualmente um aplicativo existente do ASP.NET MVC 3 para a versão 4, faça o seguinte:

1. Em todos os arquivos Web. config do projeto (há um na raiz do projeto, um na pasta views e outro na pasta views de cada área em seu projeto), substitua cada instância do seguinte texto:

    [!code-console[Main](mvc4-beta-release-notes/samples/sample1.cmd)]

    pelo seguinte texto correspondente:

    [!code-console[Main](mvc4-beta-release-notes/samples/sample2.cmd)]
2. No arquivo Web. config raiz, atualize o elemento *webpages: Version* para "2.0.0.0" e adicione uma nova chave *PreserveLoginUrl* que tenha o valor "true":

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample3.xml)]
3. Em Gerenciador de Soluções, exclua a referência a *System. Web. Mvc* (que aponta para a DLL da versão 3). Em seguida, adicione uma referência a *System. Web. Mvc* (v 4.0.0.0). Em particular, faça as seguintes alterações para atualizar as referências de assembly. Estes são os detalhes:

    1. Em Gerenciador de Soluções, exclua as referências para os seguintes assemblies: 

        - *System. Web. Mvc*(v 3.0.0.0)
        - *System. Web. Page*(v 1.0.0.0)
        - *System. Web. Razor*(v 1.0.0.0)
        - *System. Web. páginas Web. Deployment*(v 1.0.0.0)
        - *System. Web. Netpages. Razor*(v 1.0.0.0)
    2. Adicione uma referência aos seguintes assemblies: 

        - *System. Web. Mvc*(v 4.0.0.0)
        - *System. Web. Page*(v 2.0.0.0)
        - *System. Web. Razor*(v 2.0.0.0)
        - *System. Web. webpages. Deployment*(v 2.0.0.0)
        - *System. Web. webpages. Razor*(v 2.0.0.0)
4. Em Gerenciador de Soluções, clique com o botão direito do mouse no nome do projeto e selecione descarregar projeto. Em seguida, clique com o botão direito do mouse no nome novamente e selecione Editar *ProjectName*. csproj.
5. Localize o elemento *ProjectTypeGuids* e substitua {E53F8FEA-EAE0-44A6-8774-FFD645390401} por {E3E379DF-F4C6-4180-9B81-6769533ABE47}.
6. Salve as alterações, feche o arquivo de projeto (. csproj) que você estava editando, clique com o botão direito do mouse no projeto e selecione Recarregar projeto.
7. Se o projeto fizer referência a todas as bibliotecas de terceiros que são compiladas usando versões anteriores do ASP.NET MVC, abra o arquivo Web. config raiz e adicione os três elementos *bindingRedirect* a seguir na seção de *configuração* : 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample4.xml)]

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4-beta"></a>Novos recursos no ASP.NET MVC 4 beta

Esta seção descreve os recursos que foram introduzidos na versão beta do ASP.NET MVC 4.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>ASP.NET Web API

O ASP.NET MVC 4 agora inclui ASP.NET Web API, uma nova estrutura para a criação de serviços HTTP que podem alcançar uma ampla variedade de clientes, incluindo navegadores e dispositivos móveis. ASP.NET Web API também é uma plataforma ideal para a criação de serviços RESTful.

O ASP.NET Web API inclui suporte para os seguintes recursos:

- **Modelo de programação de http moderno:** Acesse e manipule diretamente solicitações e respostas HTTP em suas APIs Web usando um novo modelo de objeto HTTP com rigidez de tipos. O mesmo modelo de programação e o pipeline HTTP estão disponíveis simetricamente no cliente por meio do novo tipo HttpClient.
- **Suporte completo para rotas**: as APIs Web agora dão suporte ao conjunto completo de recursos de rota que sempre fizeram parte da pilha da Web, incluindo parâmetros de rota e restrições. Além disso, o mapeamento para ações tem suporte total para convenções, portanto, você não precisa mais aplicar atributos como [HttpPost] às suas classes e métodos.
- **Negociação de conteúdo**: o cliente e o servidor podem trabalhar juntos para determinar o formato correto dos dados retornados de uma API. Fornecemos suporte padrão para XML, JSON e formatos codificados por URL de formulário, e você pode estender esse suporte adicionando seus próprios formatadores ou até mesmo substituir a estratégia de negociação de conteúdo padrão.
- **Validação e Associação de modelo:** Os associadores de modelo fornecem uma maneira fácil de extrair dados de várias partes de uma solicitação HTTP e converter essas partes de mensagem em objetos .NET que podem ser usados pelas ações da API Web.
- **Filtros:** As APIs da Web agora dão suporte a filtros, incluindo filtros conhecidos, como o atributo [Authorize]. Você pode criar e conectar seus próprios filtros para ações, autorização e manipulação de exceção.
- **Composição de consulta:** Simplesmente retornando IQueryable&lt;T&gt;, sua API Web dará suporte à consulta por meio das convenções de URL do OData.
- **Capacidade de teste aprimorada dos detalhes de http:** Em vez de definir detalhes de HTTP em objetos de contexto estático, as ações de API da Web agora podem trabalhar com instâncias de HttpRequestMessage e HttpResponseMessage. As versões genéricas desses objetos também existem para permitir que você trabalhe com seus tipos personalizados, além dos tipos de HTTP.
- **Melhor inversão de controle (IOC) via DependencyResolver:** A API Web agora usa o padrão de localizador de serviço implementado pelo resolvedor de dependências do MVC para obter instâncias para vários recursos diferentes.
- **Configuração baseada em código:** A configuração da API Web é realizada exclusivamente por meio de código, deixando os arquivos de configuração limpos.
- **Auto-host:** As APIs da Web podem ser hospedadas em seu próprio processo, além do IIS, enquanto ainda usam o poder total das rotas e outros recursos da API Web.

Para obter mais detalhes sobre ASP.NET Web API, visite [https://www.asp.net/web-api](../web-api/index.md).

<a id="_Toc317096198"></a>
### <a name="aspnet-single-page-application"></a>Aplicativo de página única do ASP.NET

O ASP.NET MVC 4 agora inclui uma visualização antecipada da experiência de criação de aplicativos de página única com interações significativas do lado do cliente usando JavaScript e APIs da Web. Esse suporte inclui:

- Um conjunto de bibliotecas JavaScript para interações locais mais avançadas com dados armazenados em cache
- Componentes adicionais da API Web para o suporte da unidade de trabalho e da DAL
- Um modelo de projeto do MVC com scaffolding para começar rapidamente

Para obter mais detalhes sobre o suporte a aplicativos de página única no ASP.NET MVC 4, visite [https://www.asp.net/single-page-application](../single-page-application/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Aprimoramentos para modelos de projeto padrão

O modelo usado para criar novos projetos do ASP.NET MVC 4 foi atualizado para criar um site de aparência mais moderno:

![](mvc4-beta-release-notes/_static/image1.png)

Além das melhorias superficials, há uma funcionalidade aprimorada no novo modelo. O modelo emprega uma técnica chamada processamento adaptável para parecer bom em navegadores de desktops e navegadores móveis sem qualquer personalização.

![](mvc4-beta-release-notes/_static/image2.png)

Para ver a renderização adaptável em ação, você pode usar um emulador móvel ou apenas tentar redimensionar a janela do navegador da área de trabalho para ser menor. Quando a janela do navegador ficar pequena o suficiente, o layout da página será alterado.

Outro aprimoramento do modelo de projeto padrão é o uso do JavaScript para fornecer uma interface do usuário mais rica. Os links de logon e registro que são usados no modelo são exemplos de como usar a caixa de diálogo da interface do usuário do jQuery para apresentar uma tela de logon avançada:

![](mvc4-beta-release-notes/_static/image3.png)

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Modelo de projeto móvel

Se você estiver iniciando um novo projeto e quiser criar um site especificamente para navegadores móveis e do Tablet, você poderá usar o novo modelo de projeto de aplicativo móvel. Isso se baseia no jQuery Mobile, uma biblioteca de software livre para a criação da interface do usuário otimizada para toque:

![](mvc4-beta-release-notes/_static/image4.png)

Esse modelo contém a mesma estrutura de aplicativo que o modelo de aplicativo de Internet (e o código do controlador é praticamente idêntico), mas tem o estilo de usar o jQuery Mobile para parecer bom e se comportar bem em dispositivos móveis baseados em toque. Para saber mais sobre como estruturar e estilizar a interface do usuário móvel, consulte o [site do projeto do jQuery Mobile](http://jquerymobile.com/).

Se você já tiver um site orientado à área de trabalho ao qual deseja adicionar exibições otimizadas para mobilidade ou se quiser criar um único site que sirva exibições com estilo diferente para desktops e navegadores móveis, você poderá usar o novo recurso de modos de exibição. (Consulte a próxima seção.)

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Modos de exibição

O novo recurso de modos de exibição permite que um aplicativo selecione exibições dependendo do navegador que está fazendo a solicitação. Por exemplo, se um navegador da área de trabalho solicitar a página inicial, o aplicativo poderá usar o modelo Views\Home\Index.cshtml. Se um navegador móvel solicitar a página inicial, o aplicativo poderá retornar o modelo Views\Home\Index.mobile.cshtml.

Layouts e parciais também podem ser substituídos para determinados tipos de navegador. Por exemplo:

- Se sua pasta Views\Shared contiver os modelos \_layout. cshtml e \_layout. Mobile. cshtml, por padrão, o aplicativo usará o \_layout. Mobile. cshtml durante solicitações de navegadores móveis e \_layout. cshtml durante outras solicitações.
- Se uma pasta contiver \_mypartl. cshtml e \_mypartl. Mobile. cshtml, a instrução @Html.Partial("\_mypartl") será processada \_mypartl. Mobile. cshtml durante as solicitações de navegadores móveis e \_mypartss. cshtml durante outras solicitações.

Se você quiser criar exibições, layouts ou exibições parciais mais específicas para outros dispositivos, poderá registrar uma nova instância *Defaultdisplaymode* para especificar o nome a ser pesquisado quando uma solicitação atender a condições específicas. Por exemplo, você pode adicionar o código a seguir ao *aplicativo\_método Start* no arquivo global. asax para registrar a cadeia de caracteres "iPhone" como um modo de exibição que se aplica quando o navegador Apple iPhone faz uma solicitação:

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample5.cs)]

Depois que esse código for executado, quando um navegador Apple iPhone fizer uma solicitação, seu aplicativo usará o layout Views\Shared\\_Layout. iPhone. cshtml (se existir).

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-the-view-switcher-and-browser-overriding"></a>jQuery Mobile, o seletor de exibição e a substituição do navegador

o jQuery Mobile é uma biblioteca de software livre para a criação de interface do usuário da Web otimizada para toque. Se você quiser usar o jQuery Mobile com um aplicativo ASP.NET MVC 4, poderá baixar e instalar um pacote NuGet que ajuda você a começar. Para instalá-lo no console do Gerenciador de pacotes do Visual Studio, digite o seguinte comando:

[!code-powershell[Main](mvc4-beta-release-notes/samples/sample6.ps1)]

Isso instala o jQuery Mobile e alguns arquivos auxiliares, incluindo o seguinte:

- Views/Shared/\_layout. Mobile. cshtml, que é um layout baseado no jQuery Mobile.
- Um componente de seletor de exibição, que consiste na exibição parcial views/Shared/\_ViewSwitcher. cshtml e no controlador ViewSwitcherController.cs.

Depois de instalar o pacote, execute o aplicativo usando um navegador móvel (ou equivalente, como o complemento de [seletor de agente do usuário](http://chrispederick.com/work/user-agent-switcher/) do Firefox). Você verá que suas páginas parecem bastante diferentes, pois o jQuery Mobile lida com layout e estilo. Para tirar proveito disso, você pode fazer o seguinte:

- Crie substituições de exibição específicas para dispositivos móveis conforme descrito em [exibir modos](#_Toc303253810) anteriores (por exemplo, criar Views\Home\Index.Mobile.cshtml para substituir Views\Home\Index.cshtml para navegadores móveis).
- Leia a [documentação do jQuery Mobile](http://jquerymobile.com/) para saber mais sobre como adicionar elementos de interface do usuário otimizados para toque em exibições móveis.

Uma Convenção para páginas da Web otimizadas para dispositivos móveis é adicionar um link cujo texto seja algo como exibição de área de trabalho ou modo de site completo que permite aos usuários alternar para uma versão de área de trabalho da página. O pacote jQuery. Mobile. MVC inclui um exemplo de componente de seletor de exibição para essa finalidade. Ele é usado no modo de exibição padrão Views\Shared\\_Layout. Mobile. cshtml e é semelhante a este quando a página é renderizada:

![](mvc4-beta-release-notes/_static/image5.png)

Se os visitantes clicarem no link, eles serão alternados para a versão de área de trabalho da mesma página.

Como o layout da área de trabalho não incluirá um seletor de exibição por padrão, os visitantes não terão uma maneira de chegar ao modo móvel. Para habilitar isso, adicione a seguinte referência a *\_ViewSwitcher* ao layout da área de trabalho, apenas dentro do elemento *&gt;do corpo de&lt;* :

[!code-cshtml[Main](mvc4-beta-release-notes/samples/sample7.cshtml)]

O alternador de exibição usa um novo recurso chamado navegador substituindo. Esse recurso permite que seu aplicativo trate as solicitações como se estivessem provenientes de um navegador diferente (agente do usuário) do que aquela da qual elas realmente estão. A tabela a seguir lista os métodos que a substituição do navegador fornece.

| `HttpContext.SetOverriddenBrowser(userAgentString)` | Substitui o valor do agente de usuário real da solicitação usando o agente de usuário especificado. |
| --- | --- |
| `HttpContext.GetOverriddenUserAgent()` | Retorna o valor de substituição do agente do usuário da solicitação ou a cadeia de caracteres do agente do usuário real se nenhuma substituição tiver sido especificada. |
| `HttpContext.GetOverriddenBrowser()` | Retorna uma instância de *HttpBrowserCapabilitiesBase* que corresponde ao agente do usuário definido atualmente para a solicitação (real ou substituído). Você pode usar esse valor para obter propriedades como *IsMobileDevice*. |
| `HttpContext.ClearOverriddenBrowser()` | Remove qualquer agente de usuário substituído da solicitação atual. |

A substituição do navegador é um recurso fundamental do ASP.NET MVC 4 e está disponível mesmo que você não instale o pacote jQuery. Mobile. MVC. No entanto, ele afeta apenas a exibição, o layout e a seleção de exibição parcial — ele não afeta nenhum outro recurso ASP.NET que dependa do objeto *Request. browser* .

Por padrão, a substituição do agente do usuário é armazenada usando um cookie. Se você quiser armazenar a substituição em outro lugar (por exemplo, em um banco de dados), poderá substituir o provedor padrão (*BrowserOverrideStores. Current*). A documentação para esse provedor estará disponível para acompanhar uma versão posterior do ASP.NET MVC.

<a id="_Toc303253812"></a>
### <a name="recipes-for-code-generation-in-visual-studio"></a>Receitas para geração de código no Visual Studio

O novo recurso de receitas permite que o Visual Studio gere código específico da solução com base em pacotes que você pode instalar usando o NuGet. A estrutura de receitas facilita para os desenvolvedores a gravação de plug-ins de geração de código, que você também pode usar para substituir os geradores de código internos para adicionar área, adicionar controlador e adicionar modo de exibição. Como as receitas são implantadas como pacotes NuGet, elas podem ser facilmente verificadas no controle do código-fonte e compartilhadas com todos os desenvolvedores no projeto automaticamente. Eles também estão disponíveis em uma base por solução.

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Suporte a tarefas para controladores assíncronos

Agora você pode escrever métodos de ação assíncronas como métodos únicos que retornam um objeto do tipo *Task* ou *task&lt;ActionResult&gt;* .

Por exemplo, se você estiver usando o C# Visual 5 (ou usando a [CTP assíncrona](https://msdn.microsoft.com/vstudio/async.aspx)), poderá criar um método de ação assíncrono semelhante ao seguinte:

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample8.cs)]

No método Action anterior, as chamadas para *newsService. GetHeadlinesAsync* e *sportsService. GetScoresAsync* são chamadas de forma assíncrona e não bloqueiam um thread do pool de threads.

Os métodos de ação assíncronas que retornam instâncias de *tarefa* também podem dar suporte a tempos limite. Para tornar o método de ação cancelável, adicione um parâmetro do tipo *CancellationToken* à assinatura do método de ação. O exemplo a seguir mostra um método de ação assíncrona que tem um tempo limite de 2500 milissegundos e que exibe uma exibição *TimedOut* para o cliente se ocorrer um tempo limite.

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample9.cs)]

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>SDK do Azure

O ASP.NET MVC 4 beta dá suporte à versão de setembro de 2011 1,5 do SDK do Windows Azure.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemas conhecidos e alterações recentes

- **Depois de instalar o ASP.NET MVC 4 beta, o Editor CSHTML/VBHTML no Visual Studio 2010 Service Pack 1 CSHTML/VBHTML pode ser pausado por um longo tempo depois de digitar o trecho de código ou JavaScript dentro de arquivos cshtml ou vbhtml.** Isso ocorre somente em aplicativos ASP.NET MVC 4 que acabam de ser criados e ainda não foram compilados.

    A solução alternativa é compilar o projeto para obter os assemblies na pasta bin. Observe que, se você limpar o projeto que remove os assemblies da pasta bin, o problema do editor voltará a ser retornado.

    Isso será corrigido na próxima versão.
- **C#Os modelos de projeto para o Visual Studio 11 Beta contêm uma cadeia de conexão incorreta no Global.asax.cs.** A conexão padrão especificada no método de início do aplicativo\_para projetos criados no Visual Studio 11 Beta contém uma cadeia de conexão do LocalDB que contém uma barra invertida sem escape (\) caractere. Isso resulta em um erro de conexão durante tentativas de acessar um Entity Framework DbContext, que gera uma SqlException.

    Para corrigir esse problema, escape o caractere de barra invertida no aplicativo\_método de início de Global.asax.cs para que ele se leia da seguinte maneira:

    [!code-csharp[Main](mvc4-beta-release-notes/samples/sample10.cs)]
- **Os aplicativos ASP.NET MVC 4 que visam o .NET 4,5 lançarão uma FileLoadException ao tentar acessar o assembly System. net. http. dll quando executado no .NET 4,0.** Os aplicativos ASP.NET MVC 4 criados no .NET 4,5 contêm um redirecionamento de associação que resultará em um FileLoadException que declara "não foi possível carregar o arquivo ou o assembly ' System .net. http ' ou uma de suas dependências." Quando o aplicativo é executado em um sistema com o .NET 4,0 instalado. Para corrigir esse problema, remova o redirecionamento de associação a seguir de Web. config:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample11.xml)]

    O elemento de associação de assembly no Web. config modificado deve aparecer da seguinte maneira:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample12.xml)]
- <strong>O modelo de item "Adicionar controlador" em projetos de Visual Basic gera um namespace incorreto quando invocado</strong><strong>de dentro de uma área.</strong> Quando você adiciona um controlador a uma área em um projeto MVC ASP.NET que usa Visual Basic, o modelo de item insere o namespace errado no controlador. O resultado é um erro de "arquivo não encontrado" quando você navega para qualquer ação no controlador.  
  
  O namespace gerado omite tudo após o namespace raiz. Por exemplo, o namespace gerado é *RootNamespace* , mas deve ser *RootNamespace. areas. AreaName. Controllers* .
- **Alterações recentes no mecanismo de exibição do Razor.** Como parte de uma reescrita do analisador Razor, os seguintes tipos foram removidos de *System. Web. Mvc. Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

  Os métodos a seguir também foram removidos: 

    - *MvcCSharpRazorCodeParser. ParseInheritsStatement (System. Web. Razor. Parser. CodeBlockInfo)*
    - *MvcWebPageRazorHost. DecorateCodeGenerator (System. Web. Razor. Generator. RazorCodeGenerator)*
    - *MvcVBRazorCodeParser. ParseInheritsStatement (System. Web. Razor. Parser. CodeBlockInfo)*
- **Quando o WebMatrix. WebData. dll está incluído no diretório/bin de um aplicativo ASP.NET MVC 4, ele assume a URL para a autenticação de formulários.** Adicionar o assembly WebMatrix. WebData. dll ao seu aplicativo (por exemplo, selecionando "Páginas da Web do ASP.NET com a sintaxe do Razor" ao usar a caixa de diálogo Adicionar dependências implantáveis) substituirá o redirecionamento de logon de autenticação para/Account/logon em vez de/Account/Login como esperado pelo controlador de conta padrão do ASP.NET MVC. Para evitar esse comportamento e usar a URL especificada já na seção de autenticação do Web. config, você pode adicionar um appSetting chamado PreserveLoginUrl e defini-lo como true: 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample13.xml)]
- **O Gerenciador de pacotes NuGet não é instalado ao tentar instalar o ASP.NET MVC 4 para instalações lado a lado do Visual Studio 2010 e do Visual Web Developer 2010.** Para executar o Visual Studio 2010 e o Visual Web Developer 2010 lado a lado com o ASP.NET MVC 4, você deve instalar o ASP.NET MVC 4 depois que ambas as versões do Visual Studio já tiverem sido instaladas.
- **A desinstalação do ASP.NET MVC 4 falhará se os pré-requisitos já tiverem sido desinstalados.** Para desinstalar corretamente o ASP.NET MVC 4you, é necessário desinstalar o ASP.NET MVC 4 antes da desinstalação do Visual Studio.
- **A execução de um projeto de API Web padrão mostra instruções que direcionam incorretamente o usuário para adicionar rotas usando o método RegisterApis, que não existe.** As rotas devem ser adicionadas no método RegisterRoutes usando a tabela de rotas ASP.NET.
- **A instalação do ASP.NET MVC 4 beta interrompe os aplicativos ASP.NET MVC 3 RTM.** Os aplicativos ASP.NET MVC 3 que foram criados com a versão RTM (não com a versão de atualização das ferramentas do MVC 3 do ASP.NET) exigem as seguintes alterações para trabalhar lado a lado com o ASP.NET MVC 4 beta. Compilar o projeto sem fazer essas atualizações resulta em erros de compilação. 

    **Atualizações necessárias**

  1. No arquivo Web. config de raiz, adicione um novo *&lt;appsettings&gt;* entrada com a chave *páginas da Web: versão* e o valor *1.0.0.0*.

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample14.xml)]
  2. Em Gerenciador de Soluções, clique com o botão direito do mouse no nome do projeto e selecione descarregar projeto. Em seguida, clique com o botão direito do mouse no nome novamente e selecione Editar *ProjectName*. csproj.
  3. Localize as seguintes referências de assembly: 

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample15.xml)]

      Substitua-os pelo seguinte:

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample16.xml)]
  4. Salve as alterações, feche o arquivo de projeto (. csproj) que você estava editando e, em seguida, clique com o botão direito do mouse no projeto e selecione Recarregar.
