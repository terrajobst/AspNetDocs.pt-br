---
uid: mvc/mvc3
title: ASP.NET MVC 3
author: rick-anderson
description: (inclui atualização de ferramentas de abril de 2011) O ASP.NET MVC 3 é uma estrutura para a criação de aplicativos Web escalonáveis e baseados em padrões usando um padrão de design bem estabelecido...
ms.author: riande
ms.date: 10/05/2010
ms.assetid: dddc8812-a0bc-49f9-aafb-caf2064c2b8c
msc.legacyurl: /mvc/mvc3
msc.type: content
ms.openlocfilehash: 421a06c89d4dbcb05d4080033813cc6558b7c698
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74586744"
---
# <a name="aspnet-mvc-3"></a>ASP.NET MVC 3

> *(inclui atualização de ferramentas de abril de 2011)*
> 
> O ASP.NET MVC 3 é uma estrutura para a criação de aplicativos Web escalonáveis e baseados em padrões usando padrões de design bem estabelecidos e o poder do ASP.NET e do .NET Framework.
> 
> Ele é instalado lado a lado com o ASP.NET MVC 2, portanto, comece a usá-lo hoje mesmo!
> 
> Baixe o [instalador aqui](https://go.microsoft.com/fwlink/?LinkID=208140)

## <a name="top-features"></a>Principais recursos

- Sistema scaffolding integrado extensível via NuGet
- Modelos de projeto habilitados para HTML 5
- Exibições expressivas, incluindo o novo mecanismo de exibição do Razor
- Ganchos poderosos com injeção de dependência e filtros de ação globais
- Suporte a JavaScript avançado com JavaScript discreto, validação de jQuery e Associação JSON
- *Leia a lista completa de recursos [abaixo](#overview)*

## <a name="top-links"></a>Principais links

O que há de novo no ASP.NET MVC 3

- Phil Haack: [ASP.NET MVC 3 lançado](http://haacked.com/archive/2011/01/13/aspnetmvc3-released.aspx)
- Scott Hanselman: [ASP.net MvC3, WebMatrix, NuGet, IIS Express e Orchard lançados – a versão da Web de Janeiro da Microsoft em contexto](http://www.hanselman.com/blog/ASPNETMVC3WebMatrixNuGetIISExpressAndOrchardReleasedTheMicrosoftJanuaryWebReleaseInContext.aspx)
- Scott Guthrie: [anunciando o lançamento do ASP.NET MVC 3, IIS Express, SQL CE 4, Web farm Framework, Orchard, WebMatrix](https://weblogs.asp.net/scottgu/archive/2011/01/13/announcing-release-of-asp-net-mvc-3-iis-express-sql-ce-4-web-farm-framework-orchard-webmatrix.aspx)
- [Notas de versão do ASP.NET MVC 3](../whitepapers/mvc3-release-notes.md)

Instalação e ajuda

- Instalar o ASP.NET MVC 3 usando o [Web Platform Installer (recomendado)](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MVC3)
- Instalar o ASP.NET MVC 3 usando o [executável do instalador](https://go.microsoft.com/fwlink/?LinkID=208140)
- Instalar o [ASP.NET MVC 3 para Visual Studio 11 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=208140)
- Leia o [tutorial introdução ao ASP.NET MVC 3](overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)
- Obtenha ajuda e discuta o ASP.NET MVC 3 nos [fóruns](https://forums.asp.net/1146.aspx)

<a id="overview"></a>
## <a name="aspnet-mvc-3-overview"></a>Visão geral do ASP.NET MVC 3

O ASP.NET MVC 3 se baseia no ASP.NET MVC 1 e 2, adicionando ótimos recursos que simplificam seu código e permitem uma extensibilidade mais profunda. Este tópico fornece uma visão geral de muitos dos novos recursos incluídos nesta versão, organizados nas seguintes seções:

- [Scaffolding extensível com integração MvcScaffold](#BM_MvcScaffolding)
- [Modelos de projeto habilitados para HTML 5](#BM_HTML5)
- [O mecanismo de exibição do Razor](#BM_TheRazorViewEngine)
- [Suporte para vários mecanismos de exibição](#BM_Support_for_Multiple_View_Engines)
- [Aprimoramentos do controlador](#BM_Controller_Improvements)
- [JavaScript e Ajax](#BM_JavaScript_and_Ajax_Improvements)
- [Aprimoramentos de validação de modelo](#BM_Model_Validation_Improvements)
- [Melhorias de injeção de dependência](#BM_Dependency_Injection_Improvements)
- [Outros novos recursos](#BM_Other_New_Features)

<a id="BM_MvcScaffolding"></a>

## <a name="extensible-scaffolding-with-mvcscaffold-integration"></a>Scaffolding extensível com integração MvcScaffold

O novo sistema scaffolding torna mais fácil pegar e começar a usar de forma produtiva se você for totalmente novo na estrutura e para automatizar tarefas comuns de desenvolvimento se você tiver experiência e já souber o que está fazendo.

Isso é suportado pelo novo pacote NuGet *scaffolding* chamado **MvcScaffolding**. O termo "scaffolding" é usado por muitas tecnologias de software para significar "gerar rapidamente uma estrutura de tópicos básica de seu software que você pode editar e personalizar". O pacote scaffolding que estamos criando para o ASP.NET MVC é bastante benéfico em vários cenários:

- **Se você estiver aprendendo o ASP.NET MVC pela primeira vez**, porque ele oferece uma maneira rápida de obter um código útil e funcional, que você pode editar e adaptar de acordo com suas necessidades. Ele o poupa do trauma de olhar para uma página em branco e não ter idéia de onde começar!
- **Se você conhece bem o ASP.NET MVC e agora está explorando alguma nova tecnologia complementar** , como um mapeador relacional de objeto, um mecanismo de exibição, uma biblioteca de testes, etc., porque o criador dessa tecnologia também pode ter criado um pacote scaffolding para ele.
- **Se seu trabalho envolve criar repetidamente classes semelhantes ou arquivos de algum tipo**, porque você pode criar scaffolders personalizadas que geram artefatos de teste, scripts de implantação ou qualquer outra coisa que você precise. Todos em sua equipe também podem usar o scaffolders personalizado.

Outros recursos do MvcScaffolding incluem:

- Suporte para C# projetos do e do vb
- Suporte para os mecanismos de exibição Razor e ASPX
- Dá suporte a scaffolding em áreas MVC ASP.NET e usando layouts/mestres de exibição personalizados
- Você pode personalizar facilmente a saída editando modelos T4
- Você pode adicionar scaffolders totalmente novos usando a lógica personalizada do PowerShell e modelos T4 personalizados. Esses (e quaisquer parâmetros personalizados que você os forneceu) aparecem automaticamente na lista guia de conclusão do console.
- Você pode obter pacotes NuGet que contêm scaffolders adicionais para tecnologias diferentes (por exemplo, há uma prova de conceito para LINQ to SQL agora) e misturá-los e combiná-los juntos

A atualização das ferramentas do ASP.NET MVC 3 inclui um excelente suporte ao Visual Studio para esse sistema scaffolding, como:

- A caixa de diálogo Adicionar controlador agora dá suporte a scaffolding automática completa de ações de criação, leitura, atualização e exclusão do controlador e exibições correspondentes. Por padrão, esse aplica Scaffold código de acesso a dados usando o EF Code First.
- A caixa de diálogo Adicionar controlador dá suporte ao *extensível aplica Scaffold* por meio de pacotes NuGet, como *MvcScaffolding*. Isso permite a conexão em aplica Scaffold personalizadas na caixa de diálogo, o que permitiria criar aplica Scaffold para outras tecnologias de acesso a dados, como NHibernate ou até mesmo JET com ODBCDirect, se você for tão inclinado!

Para obter mais informações sobre o scaffolding no ASP.NET MVC 3, consulte os seguintes recursos:

- Série post de Steve Sanderson, incluindo: 

    1. [Introdução: Scaffold seu projeto do ASP.NET MVC 3 com o pacote MvcScaffolding](http://blog.stevensanderson.com/2011/01/13/scaffold-your-aspnet-mvc-3-project-with-the-mvcscaffolding-package/)
    2. [Uso padrão: casos e opções de uso típicos](http://blog.stevensanderson.com/2011/01/13/mvcscaffolding-standard-usage/)
    3. [Relações um-para-muitos](http://blog.stevensanderson.com/2011/01/28/mvcscaffolding-one-to-many-relationships/)
    4. [Ações e testes de unidade do scaffolding](http://blog.stevensanderson.com/2011/03/28/scaffolding-actions-and-unit-tests-with-mvcscaffolding/)
    5. [Substituindo os modelos T4](http://blog.stevensanderson.com/2011/04/06/mvcscaffolding-overriding-the-t4-templates/)
    6. [Esta postagem: criando scaffolders personalizadas](http://blog.stevensanderson.com/2011/04/07/mvcscaffolding-creating-custom-scaffolders/)
- Postagem de Scott Hanselman de sua sessão do PDC 2010 [criando um blog com a Microsoft "pacote sem nome de amor da Web"](http://www.hanselman.com/blog/PDC10BuildingABlogWithMicrosoftUnnamedPackageOfWebLove.aspx)
- [Notas de versão do MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_HTML5"></a>

## <a name="html-5-project-templates"></a>Modelos de projeto HTML 5

A caixa de diálogo novo projeto inclui uma caixa de seleção Habilitar versões HTML 5 de modelos de projeto. Esses modelos aproveitam o Modernizr 1,7 para fornecer suporte de compatibilidade para HTML 5 e CSS 3 em navegadores de nível inferior.

<a id="BM_TheRazorViewEngine"></a>

## <a name="the-razor-view-engine"></a>O mecanismo de exibição do Razor

O ASP.NET MVC 3 vem com um novo mecanismo de exibição chamado Razor, que oferece os seguintes benefícios:

- Sintaxe Razor é limpo e conciso, exigindo um número mínimo de pressionamentos de tecla.
- O Razor é fácil de aprender, em parte porque é baseado em linguagens existentes C# como e Visual Basic.
- O Visual Studio inclui IntelliSense e colorização de código para sintaxe Razor.
- Os modos de exibição do Razor podem ser testados por unidade sem a necessidade de executar o aplicativo ou iniciar um servidor Web.

Alguns novos recursos do Razor incluem o seguinte:

- `@model` sintaxe para especificar o tipo que está sendo passado para a exibição.
- `@* *@` sintaxe de comentário.
- A capacidade de especificar padrões (como `layoutpage`) uma vez para um site inteiro.
- O método `Html.Raw` para exibir texto sem codificação HTML.
- Suporte para compartilhamento de código entre vários modos de exibição ( *\_arquivos viewstart. cshtml* ou *\_viewstart. vbhtml* ).

O Razor também inclui novos auxiliares HTML, como o seguinte:

- `Chart`. Renderiza um gráfico, oferecendo os mesmos recursos que o controle de gráfico no ASP.NET 4.
- `WebGrid`. Renderiza uma grade de dados, completa com a funcionalidade de paginação e classificação.
- `Crypto`. Usa algoritmos de hash para criar senhas com Salt e hash corretamente.
- `WebImage`. Renderiza uma imagem.
- `WebMail`. Envia uma mensagem de email.

Para obter mais informações sobre o Razor, consulte os seguintes recursos:

- [Postagem do blog de Scott Guthrie apresentando o Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)
- [Postagem do blog de Scott Guthrie apresentando a palavra-chave @model](https://weblogs.asp.net/scottgu/archive/2010/10/19/asp-net-mvc-3-new-model-directive-support-in-razor.aspx)
- [Postagem do blog de Scott Guthrie introdução aos layouts do Razor](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)
- [Referência rápida da API do Razor](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)
- [Notas de versão do MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Support_for_Multiple_View_Engines"></a>

## <a name="support-for-multiple-view-engines"></a>Suporte para vários mecanismos de exibição

A caixa de diálogo **Adicionar exibição** no ASP.NET MVC 3 permite que você escolha o mecanismo de exibição com o qual deseja trabalhar e a caixa de diálogo **novo projeto** permite especificar o mecanismo de exibição padrão para um projeto. Você pode escolher o mecanismo de exibição do Web Forms (ASPX), Razor ou um mecanismo de exibição de código-fonte aberto, como [Spark](http://sparkviewengine.com/), [NHaml](https://code.google.com/p/nhaml/)ou [NDjango](http://ndjango.org/).

<a id="BM_Controller_Improvements"></a>

## <a name="controller-improvements"></a>Aprimoramentos do controlador

### <a name="global-action-filters"></a>Filtros de ação globais

Às vezes, você deseja executar a lógica antes que um método de ação seja executado ou após a execução de um método de ação. Para dar suporte a isso, o ASP.NET MVC 2 forneceu filtros de ação. Os filtros de ação são atributos personalizados que fornecem um meio declarativo para adicionar o comportamento de ação prévia e ação a métodos específicos de ações do controlador. No entanto, em alguns casos, talvez você queira especificar o comportamento de ação prévia ou ação que se aplica a todos os métodos de ação. O MVC 3 permite que você especifique filtros globais adicionando-os à coleção de `GlobalFilters`. Para obter mais informações sobre filtros de ação globais, consulte os seguintes recursos:

- [Blog de Scott Guthrie na versão prévia do MVC 3](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)
- [Filtragem no MVC do ASP.NET](https://msdn.microsoft.com/library/gg416513(VS.98).aspx)

### <a name="new-viewbag-property"></a>Nova propriedade "ViewBag"

Os controladores MVC 2 dão suporte a uma propriedade `ViewData` que permite passar dados para um modelo de exibição usando uma API de dicionário de associação tardia. No MVC 3, você também pode usar uma sintaxe um pouco mais simples com a propriedade `ViewBag` para atingir a mesma finalidade. Por exemplo, em vez de escrever `ViewData["Message"]="text"`, você pode escrever `ViewBag.Message="text"`. Você não precisa definir classes fortemente tipadas para usar a propriedade `ViewBag`. Como é uma propriedade dinâmica, você pode apenas obter ou definir as propriedades e ela as resolverá dinamicamente em tempo de execução. Internamente, as propriedades de `ViewBag` são armazenadas como pares de nome/valor no dicionário de `ViewData`. (Observação: na maioria das versões de pré-lançamento do MVC 3, a propriedade `ViewBag` foi nomeada a propriedade `ViewModel`.)

### <a name="new-actionresult-types"></a>Novos tipos "ActionResult"

Os seguintes tipos de `ActionResult` e métodos auxiliares correspondentes são novos ou aprimorados no MVC 3:

- [HttpNotFoundResult](https://msdn.microsoft.com/library/system.web.mvc.httpnotfoundresult(v=vs.98).aspx). Retorna um código de status HTTP 404 para o cliente.
- [RedirectResult](https://msdn.microsoft.com/library/system.web.mvc.redirectresult(v=VS.98).aspx). Retorna um redirecionamento temporário (código de status HTTP 302) ou um redirecionamento permanente (código de status HTTP 301), dependendo de um parâmetro booliano. Em conjunto com essa alteração, a classe [Controller](https://msdn.microsoft.com/library/system.web.mvc.controller(v=VS.98).aspx) agora tem três métodos para executar redirecionamentos permanentes: `RedirectPermanent`, `RedirectToRoutePermanent`e `RedirectToActionPermanent`. Esses métodos retornam uma instância de `RedirectResult` com a propriedade `Permanent` definida como `true`.
- [HttpStatusCodeResult](https://msdn.microsoft.com/library/system.web.mvc.httpstatuscoderesult(v=VS.98).aspx). Retorna um código de status HTTP especificado pelo usuário.

<a id="BM_JavaScript_and_Ajax_Improvements"></a>

## <a name="javascript-and-ajax-improvements"></a>Aprimoramentos em JavaScript e Ajax

Por padrão, o Ajax e auxiliares de validação no MVC 3 usam uma abordagem de JavaScript discreta. O JavaScript discreto evita a injeção de JavaScript embutido em HTML. Isso torna o HTML menor e menos confuso e torna mais fácil trocar ou personalizar bibliotecas JavaScript. Os auxiliares de validação no MVC 3 também usam o plug-in `jQueryValidate` por padrão. Se você quiser o comportamento do MVC 2, poderá desabilitar o JavaScript discreto usando uma configuração de arquivo *Web. config* . Para obter mais informações sobre as melhorias de JavaScript e Ajax, consulte os seguintes recursos:

- [Introdução básica ao JavaScript discreto no site da Wikipédia](http://en.wikipedia.org/wiki/Unobtrusive_JavaScript)
- [A postagem do JavaScript discreto de Brad Wilson](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-ajax.html)
- [Coluna de validação de JavaScript discreto de Brad Wilson](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)
- [Criando um aplicativo MVC 3 com Razor e JavaScript discreto](overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript.md) (tutorial no site ASP.net)
- [Notas de versão do MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="client-side-validation-enabled-by-default"></a>Validação do lado do cliente habilitada por padrão

Em versões anteriores do MVC, você precisa chamar explicitamente o método `Html.EnableClientValidation` de uma exibição para habilitar a validação do lado do cliente. No MVC 3, isso não é mais necessário porque a validação do lado do cliente está habilitada por padrão. (Você pode desabilitar isso usando uma configuração no arquivo *Web. config* .)

Para que a validação do lado do cliente funcione, você ainda precisa fazer referência às bibliotecas de validação jQuery e jQuery apropriadas em seu site. Você pode hospedar essas bibliotecas em seu próprio servidor ou referenciá-las de uma CDN (rede de distribuição de conteúdo) como o CDNs da Microsoft ou do Google.

### <a name="remote-validator"></a>Validador remoto

O ASP.NET MVC 3 dá suporte à nova classe [remoteattribute](https://msdn.microsoft.com/library/system.web.mvc.remoteattribute(v=VS.98).aspx) que permite que você aproveite o suporte ao validador remoto do plug-in de validação do jQuery. Isso permite que a biblioteca de validação do lado do cliente chame automaticamente um método personalizado que você define no servidor para executar a lógica de validação que só pode ser feita no lado do servidor.

No exemplo a seguir, o atributo `Remote` especifica que a validação do cliente chamará uma ação chamada `UserNameAvailable` na classe `UsersController` para validar o campo `UserName`.

[!code-csharp[Main](mvc3/samples/sample1.cs)]

O exemplo a seguir mostra o controlador correspondente.

[!code-csharp[Main](mvc3/samples/sample2.cs)]

Para obter mais informações sobre como usar o atributo `Remote`, consulte [como implementar a validação remota no ASP.NET MVC](https://msdn.microsoft.com/library/gg508808(VS.98).aspx) na biblioteca do MSDN.

### <a name="json-binding-support"></a>Suporte à associação JSON

O ASP.NET MVC 3 inclui suporte interno à ligação JSON que permite que os métodos de ação recebam dados codificados em JSON e associem o modelo a parâmetros de método de ação. Esse recurso é útil em cenários que envolvem modelos de cliente e vinculação de dados. (Os modelos de cliente permitem Formatar e exibir um único item de dados ou um conjunto de itens de dados usando modelos que são executados no cliente.) O MVC 3 permite que você conecte facilmente modelos de cliente com métodos de ação no servidor que enviam e recebem dados JSON. Para obter mais informações sobre o suporte à ligação JSON, consulte a seção de **aprimoramentos do JavaScript e do AJAX** da [postagem do blog do Scott Guthrie no MVC 3 preview](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx).

<a id="BM_Model_Validation_Improvements"></a>

## <a name="model-validation-improvements"></a>Aprimoramentos de validação de modelo

### <a name="dataannotations-metadata-attributes"></a>Atributos de metadados de "Annotations"

O ASP.NET MVC 3 dá suporte a atributos de metadados `DataAnnotations` como `DisplayAttribute`.

### <a name="validationattribute-class"></a>Classe "ValidationAttribute"

A classe `ValidationAttribute` foi aprimorada no .NET Framework 4 para dar suporte a uma nova sobrecarga de `IsValid` que fornece mais informações sobre o contexto de validação atual, como qual objeto está sendo validado. Isso permite cenários mais ricos em que você pode validar o valor atual com base em outra Propriedade do modelo. Por exemplo, o novo atributo `CompareAttribute` permite que você compare os valores de duas propriedades de um modelo. No exemplo a seguir, a propriedade `ComparePassword` deve coincidir com o campo `Password` para ser válido.

[!code-csharp[Main](mvc3/samples/sample3.cs)]

### <a name="validation-interfaces"></a>Interfaces de validação

A interface [IValidatableObject](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.ivalidatableobject.aspx) permite que você execute a validação em nível de modelo e permite que você forneça mensagens de erro de validação que são específicas para o estado do modelo geral ou entre duas propriedades dentro do modelo. O MVC 3 agora recupera erros da interface `IValidatableObject` quando a associação de modelo e automaticamente sinaliza ou realça os campos afetados em uma exibição usando os auxiliares de formulário HTML internos.

A interface [IClientValidatable](https://msdn.microsoft.com/library/system.web.mvc.iclientvalidatable(v=VS.98).aspx) permite que o ASP.NET MVC descubra em tempo de execução se um validador tem suporte para validação do cliente. Essa interface foi projetada para que possa ser integrada com uma variedade de estruturas de validação.

Para obter mais informações sobre interfaces de validação, consulte a seção **aprimoramentos de validação de modelo** da [postagem do blog MVC 3 de Scott Guthrie](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx). (No entanto, observe que a referência a "IValidateObject" no blog deve ser "IValidatableObject".)

<a id="BM_Dependency_Injection_Improvements"></a>

## <a name="dependency-injection-improvements"></a>Melhorias de injeção de dependência

O ASP.NET MVC 3 fornece um melhor suporte para aplicar injeção de dependência (DI) e para a integração com injeção de dependência ou contêineres IOC (inversão de controle). O suporte para DI foi adicionado nas seguintes áreas:

- Controladores (registrando e injetando fábricas de controlador, injetando controladores).
- Exibições (registrando e injetando mecanismos de exibição, injetando dependências em páginas de exibição).
- Filtros de ação (localizando e injetando filtros).
- ASSOCIADORES de modelo (registro e injeção).
- Provedores de validação de modelo (registro e injeção).
- Provedores de metadados de modelo (registro e injeção).
- Provedores de valor (registro e injeção).

O MVC 3 dá suporte à biblioteca do [localizador de serviços comum](https://github.com/unitycontainer/commonservicelocator) e a qualquer contêiner de injeção de suporte que ofereça a interface `IServiceLocator` da biblioteca. Ele também dá suporte a uma nova interface `IDependencyResolver` que torna mais fácil integrar estruturas DI.

Para obter mais informações sobre DI no MVC 3, consulte os seguintes recursos:

- [A série de Brad Wilson de postagens de blog no local do serviço](http://bradwilson.typepad.com/blog/2010/07/service-location-pt1-introduction.html)
- [Notas de versão do MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Other_New_Features"></a>

## <a name="other-new-features"></a>Outros novos recursos

### <a name="nuget-integration"></a>Integração do NuGet

O ASP.NET MVC 3 instala e habilita automaticamente o NuGet como parte de sua configuração. O NuGet é um Gerenciador de pacotes de código-fonte aberto gratuito que facilita a localização, a instalação e o uso de bibliotecas e ferramentas do .NET em seus projetos. Ele funciona com todos os tipos de projeto do Visual Studio (incluindo ASP.NET Web Forms e ASP.NET MVC).

O NuGet habilita os desenvolvedores que mantêm projetos de software livre (por exemplo, projetos como MOQ, NHibernate, Ninject, StructureMap, NUnit, Windsor, RhinoMocks e ELMAH) para empacotar suas bibliotecas e registrá-los em uma galeria online. É fácil para os desenvolvedores do .NET que desejam usar uma dessas bibliotecas para localizar o pacote e instalá-lo em projetos em que estão trabalhando.

Com a atualização das ferramentas do ASP.NET 3, os modelos de projeto incluem os pacotes do NuGet pré-instalados para as bibliotecas JavaScript, para que sejam atualizáveis por meio do NuGet. Entity Framework Code First também é pré-instalado como um pacote NuGet.

Para saber mais sobre o NuGet, veja a [documentação do NuGet](https://docs.microsoft.com/nuget/).

### <a name="partial-page-output-caching"></a>Cache de saída de página parcial

O ASP.NET MVC tem suporte para cache de saída de respostas de página inteira desde a versão 1. O MVC 3 também dá suporte a cache de saída de página parcial, que permite que você armazene facilmente regiões ou fragmentos de uma resposta em cache. Para obter mais informações sobre cache, consulte a seção **cache de saída de página parcial** da [postagem do blog de Scott Guthrie na seção Release Candidate do MVC 3](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx) e o **cache de saída da ação filho** das [notas de versão do MVC 3](../whitepapers/mvc3-release-notes.md).

### <a name="granular-control-over-request-validation"></a>Controle granular sobre a validação de solicitação

O ASP.NET MVC tem uma validação de solicitação interna que ajuda automaticamente a proteger contra ataques de injeção de HTML e XSS. No entanto, às vezes você deseja desabilitar explicitamente a validação de solicitação, como se você quiser permitir que os usuários publiquem o conteúdo HTML (por exemplo, em entradas de blog ou conteúdo CMS). Agora você pode adicionar um atributo [AllowHtml](https://msdn.microsoft.com/library/system.web.mvc.allowhtmlattribute(v=VS.98).aspx) a modelos ou modelos de exibição para desabilitar a validação de solicitação por Propriedade durante a associação de modelo. Para obter mais informações sobre a validação de solicitação, consulte os seguintes recursos:

- A seção **JavaScript e validação discretas** na [postagem do blog de Scott Guthrie no Release Candidate do MVC 3](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx).
- [Notas de versão do MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="extensible-new-project-dialog-box"></a>Caixa de diálogo extensível "novo projeto"

No ASP.NET MVC 3, você pode adicionar modelos de projeto, mecanismos de exibição e estruturas de projeto de teste de unidade à caixa de diálogo **novo projeto** .

### <a name="template-scaffolding-improvements"></a>Aprimoramentos do modelo scaffolding

Os modelos ASP.NET MVC 3 scaffolding fazem um trabalho melhor de identificar as propriedades de chave primária em modelos e tratá-las de forma adequada do que nas versões anteriores do MVC. (Por exemplo, os modelos scaffolding agora garantem que a chave primária não é com Scaffold como um campo de formulário editável.)

Por padrão, o aplica Scaffold criar e editar agora usa o auxiliar de `Html.EditorFor` em vez do auxiliar de `Html.TextBoxFor`. Isso melhora o suporte para metadados no modelo na forma de atributos de anotação de dados quando a caixa de diálogo **Adicionar exibição** gera uma exibição.

### <a name="new-overloads-for-htmllabelfor-and-htmllabelformodel"></a>Novas sobrecargas para "HTML. LabelFor" e "HTML. LabelForModel"

Foram adicionadas novas sobrecargas de método para os métodos auxiliares `LabelFor` e `LabelForModel`. As novas sobrecargas permitem que você especifique ou substitua o texto do rótulo.

### <a name="sessionless-controller-support"></a>Suporte a controlador sem sessão

No ASP.NET MVC 3, você pode indicar se deseja que uma classe de controlador use o estado da sessão e, em caso afirmativo, se o estado da sessão deve ser de leitura/gravação ou somente leitura. Para obter mais informações sobre o suporte a controlador sem sessão, consulte [notas de versão do MVC 3](../whitepapers/mvc3-release-notes.md).

### <a name="new-additionalmetadataattribute-class"></a>Nova classe "AdditionalMetadataAttribute"

Você pode usar o atributo [AdditionalMetadata](https://msdn.microsoft.com/library/system.web.mvc.additionalmetadataattribute(v=VS.98).aspx) para popular o dicionário de `ModelMetadata.AdditionalValues` para uma propriedade de modelo. Por exemplo, se um modelo de exibição tiver uma propriedade que deve ser exibida somente para um administrador, você poderá anotar essa propriedade, conforme mostrado no exemplo a seguir:

[!code-csharp[Main](mvc3/samples/sample4.cs)]

Esses metadados são disponibilizados para qualquer modelo de exibição ou editor quando um modelo de exibição de produto é renderizado. Cabe a você interpretar as informações de metadados.

### <a name="accountcontroller-improvements"></a>Aprimoramentos do AccountController

O AccountController no modelo de projeto de Internet foi bastante melhorado.

### <a name="new-intranet-project-template"></a>Novo modelo de projeto de intranet

Um novo modelo de projeto de intranet é incluído, o que habilita a autenticação do Windows e remove o AccountController.
