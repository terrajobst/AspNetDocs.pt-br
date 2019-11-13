---
uid: whitepapers/mvc3-release-notes
title: ASP.NET MVC 3 | Microsoft Docs
author: rick-anderson
description: ''
ms.author: riande
ms.date: 10/06/2010
ms.assetid: f44c166e-7e91-48a0-a6f8-d9285f3594e5
msc.legacyurl: /whitepapers/mvc3-release-notes
msc.type: content
ms.openlocfilehash: 46d051a5eba6501cf36910b7674ce6400597de8a
ms.sourcegitcommit: 295cf898a4c87e264b0c35c7254b0fa4169f2278
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/13/2019
ms.locfileid: "74057024"
---
# <a name="aspnet-mvc-3"></a>ASP.NET MVC 3

- [Visão Geral](#overview)
- [Notas de instalação](#installation-notes)
- [Requisitos de software](#software-requirements)
- [Documentação](#documentation)
- [Suporte](#support)
- [Atualizando um projeto do ASP.NET MVC 2 para a atualização das ferramentas do ASP.NET MVC 3](#upgrading)
- [Atualização das ferramentas do ASP.NET MVC 3 (12 de abril de 2011)](#tu-changes)

    - [A caixa de diálogo "Adicionar controlador" agora pode Scaffold controladores com exibições e código de acesso a dados](#tu-AddControllerDialog)
    - [Melhorias na caixa de diálogo "ASP.NET MVC 3 New Project"](#tu-ImprovementsNewDialogBox)
    - [Os modelos de projeto agora incluem o Modernizr 1,7](#tu-Modernizr)
    - [Os modelos de projeto incluem versões atualizadas do jQuery, da interface do usuário do jQuery e da validação do jQuery](#tu-UpdatedJQuery)
    - [Os modelos de projeto agora incluem ADO.NET Entity Framework 4,1 como um pacote NuGet pré-instalado](#tu-EF)
    - [Os modelos de projeto incluem bibliotecas JavaScript como pacotes NuGet pré-instalados](#tu-JavaScriptLibsNuget)
    - [Problemas conhecidos](#tu-KI)
- [ASP.NET MVC 3 RTM (13 de janeiro de 2011)](#MVC3RTM)

    - [Alteração: atualização da versão da interface do usuário do jQuery para 1.8.7](#RTM-1)
    - [Alterar: o ModelMetadataProvider padrão foi alterado de volta para DataAnnotationsModelMetadataProvider](#RTM-2)
    - [Corrigido: a colagem de parte de uma expressão Razor que contém os resultados de espaço em branco são revertidas](#RTM-3)
    - [Corrigido: a renomeação de um arquivo Razor aberto no editor desabilita a colorização de sintaxe e o IntelliSense](#RTM-4)
    - [Problemas conhecidos](#RTM-KI)
    - [Alterações recentes](#RTM-BC)
- [ASP.NET MVC 3 versão Release Candidate 2 (10 de dezembro de 2010)](#_Toc2)

    - [Os modelos de projeto mudaram para incluir jQuery 1.4.4, jQuery Validation 1,7 e jQuery UI 1.8.6 y interface do usuário 1.8.6](#_Toc2_1)
    - [Classe "AdditionalMetadataAttribute" adicionada](#_Toc2_2)
    - [Scaffolding de exibição aprimorada](#_Toc2_3)
    - [Método html. Raw adicionado](#_Toc2_3)
    - [Propriedade "Controller. ViewModel" renomeada e a propriedade "View" como "ViewBag"](#_Toc2_4)
    - [A classe "ControllerSessionStateAttribute" foi renomeada como "SessionStateattribute"](#_Toc2_5)
    - [Renomear a propriedade "Fields" de Remoteattribute como "AdditionalFields"](#_Toc2_6)
    - ["SkipRequestValidationAttribute" renomeado para "AllowHtmlAttribute"](#_Toc2_7)
    - [O método "HTML. ValidationMessage" foi alterado para exibir a primeira mensagem de erro útil](#_Toc2_8)
    - [Correção @model declaração para não adicionar espaço em branco ao documento](#_Toc2_9)
    - [Adicionada a propriedade "FileExtensions" para exibir mecanismos para dar suporte a nomes de arquivo específicos do mecanismo](#_Toc2_10)
    - [Correção do auxiliar "LabelFor" para emitir o valor correto para o atributo "for"](#_Toc2_11)
    - [Corrigido o método "Renderingaction" para dar precedência de valores explícitos durante a associação de modelo](#_Toc2_12)
    - [Alterações recentes](#_Toc2_BC)
    - [Problemas conhecidos](#_Toc2_KI)
- [ASP.NET MVC 3 Release Candidate (9 de novembro de 2010)](#TOC_ASP_NET_3_RC)

    - [Novos recursos no ASP.NET MVC 3 RC](#_Toc276711785)
    - [Gerenciador de pacotes NuGet](#_Toc276711786)
    - [Caixa de diálogo "novo projeto" aprimorada](#_Toc276711787)
    - [Controladores de sessão](#_Toc276711788)
    - [Novos atributos de validação](#_Toc276711789)
    - [Novas sobrecargas para métodos "LabelFor" e "LabelForModel"](#_Toc276711790)
    - [Cache de saída de ação filho](#_Toc276711791)
    - [Aprimoramentos da caixa de diálogo "Adicionar exibição"](#_Toc276711792)
    - [Validação de solicitação granular](#_Toc276711793)
    - [Alterações recentes](#_Toc276711794)
    - [Problemas conhecidos](#_Toc276711795)
- [ASP. Notas beta do MVC 3 (6 de outubro de 2010)](#TOC_ASP_NET_3_Beta)

    - [Novos recursos no ASP.NET MVC 3 beta](#0.1__Toc274034215)
    - [Gerenciador de pacotes NuPack](#0.1__Toc274034216)
    - [Caixa de diálogo novo projeto aprimorada](#0.1__Toc274034217)
    - [Maneira simplificada de especificar modelos fortemente tipados em exibições do Razor](#0.1__Toc274034218)
    - [Suporte para novos métodos auxiliares Páginas da Web do ASP.NET](#0.1__Toc274034219)
    - [Suporte adicional à injeção de dependência](#0.1__Toc274034220)
    - [Novo suporte para AJAX não intrusivo baseado em jQuery](#0.1__Toc274034221)
    - [Novo suporte para validação não invasiva do jQuery](#0.1__Toc274034222)
    - [Novos sinalizadores de todo o aplicativo para validação de cliente e JavaScript discreto](#0.1__Toc274034223)
    - [Novo suporte para o código que é executado antes da execução de views](#0.1__Toc274034224)
    - [Novo suporte para a sintaxe VBHTML Razor](#0.1__Toc274034225)
    - [Controle mais granular sobre ValidateInputAttribute](#0.1__Toc274034226)
    - [Os auxiliares convertem sublinhados em hifens para nomes de atributo HTML especificados usando objetos anônimos](#0.1__Toc274034227)
    - [Correções de bugs](#0.1__Toc274034228)
    - [Alterações recentes](#0.1__Toc274034229)
    - [Problemas conhecidos](#0.1__Toc274034230)
- [Enção](#0.1__Toc274034231)

<a id="overview"></a>
## <a name="overview"></a>Visão Geral

Este documento descreve o lançamento do ASP.NET MVC 3 RTM para Visual Studio 2010. O ASP.NET MVC é uma estrutura para o desenvolvimento de aplicativos Web que usa o padrão MVC (Model-View-Controller). O instalador do ASP.NET MVC 3 inclui os seguintes componentes:

- Componentes de tempo de execução do ASP.NET MVC 3
- Ferramentas do ASP.NET MVC 3 Visual Studio 2010
- Páginas da Web do ASP.NET os componentes de tempo de execução
- Ferramentas do Páginas da Web do ASP.NET Visual Studio 2010
- NuGet (Gerenciador de pacotes da Microsoft para .NET)
- Uma atualização para o Visual Studio 2010 que habilita o suporte para sintaxe Razor. (Para obter detalhes, consulte o artigo 2483190 da base de conhecimento.)

O conjunto completo de notas de versão para cada versão de pré-lançamento do ASP.NET MVC 3 pode ser encontrado no site do ASP.NET na seguinte URL:

https://www.asp.net/learn/whitepapers/mvc3-release-notes

<a id="installation-notes"></a>
## <a name="installation-notes"></a>Notas de instalação

Para instalar o ASP.NET MVC 3 RTM usando o Web Platform Installer (Web PI), visite a seguinte página:

[https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3](https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3)

Como alternativa, você pode baixar o instalador do ASP.NET MVC 3 RTM para Visual Studio 2010 na seguinte página:

https://go.microsoft.com/fwlink/?LinkID=208140

O ASP.NET MVC 3 pode ser instalado e pode ser executado lado a lado com o ASP.NET MVC 2.

<a id="software-requirements"></a>
## <a name="software-requirements"></a>Requisitos de software

Os componentes de tempo de execução do ASP.NET MVC 3 exigem o seguinte software:

- .NET Framework versão 4. 

    As ferramentas do ASP.NET MVC 3 Visual Studio 2010 exigem o seguinte software:
- Visual Studio 2010 ou Visual Web Developer 2010 Express.

<a id="documentation"></a>
## <a name="documentation"></a>Documentação

A documentação do ASP.NET MVC está disponível no site do MSDN na seguinte URL:

[https://go.microsoft.com/fwlink/?LinkId=205717](https://go.microsoft.com/fwlink/?LinkId=205717)

Os tutoriais e outras informações sobre o ASP.NET MVC estão disponíveis na página MVC do site da ASP.NET na seguinte URL:

[https://www.asp.net/mvc/](../mvc/index.md)

<a id="support"></a>
## <a name="support"></a>Suporte

Esta é uma versão totalmente suportada. Informações sobre como obter suporte técnico podem ser encontradas no [site suporte da Microsoft](https://support.microsoft.com/).

Também fique à vontade para postar perguntas sobre esta versão no fórum do ASP.NET MVC, em que os membros da Comunidade do ASP.NET freqüentemente podem fornecer suporte informal:

[https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)

<a id="upgrading"></a>
## <a name="upgrading-an-aspnet-mvc-2-project-to-aspnet-mvc-3-tools-update"></a>Atualizando um projeto do ASP.NET MVC 2 para a atualização das ferramentas do ASP.NET MVC 3

O ASP.NET MVC 3 pode ser instalado lado a lado com o ASP.NET MVC 2 no mesmo computador, que oferece flexibilidade na escolha de quando atualizar um aplicativo ASP.NET MVC 2 para o ASP.NET MVC 3.

Para atualizar manualmente um aplicativo existente do ASP.NET MVC 2 para a versão 3, faça o seguinte:

1. Crie um novo projeto do ASP.NET MVC 3 vazio em seu computador. Este projeto conterá alguns arquivos necessários para a atualização.
2. Copie os seguintes arquivos do projeto ASP.NET MVC 3 para o local correspondente do seu projeto ASP.NET MVC 2. Você precisará atualizar todas as referências à biblioteca jQuery para considerar o novo nome de arquivo (jQuery-1.5.1. js): 

    - /Views/Web.config
    - /packages.config
    - /scripts/\*. js
    - \*/content/themes/.\*
3. Copie a pasta *pacotes* na raiz da solução de projeto ASP.NET MVC 3 vazia na raiz da solução, que está no diretório em que o arquivo. sln da solução está localizado.
4. Se seu projeto do ASP.NET MVC 2 contiver quaisquer áreas, copie o arquivo/Views/Web.config para a pasta *views* de cada área.
5. Em ambos os arquivos Web. config no projeto ASP.NET MVC 2, pesquise globalmente e substitua a versão MVC do ASP.NET. Localize o seguinte: 

    [!code-console[Main](mvc3-release-notes/samples/sample1.cmd)]

    Substitua-o pelo seguinte:

    [!code-console[Main](mvc3-release-notes/samples/sample2.cmd)]
6. Em Gerenciador de Soluções, exclua a referência a *System. Web. Mvc* (que aponta para a DLL da versão 2) e, em seguida, adicione uma referência a *System. Web. Mvc* (v 3.0.0.0).
7. Adicione uma referência a System. Web. webpages. dll e System. Web. Helpers. dll. Esses assemblies estão localizados nas seguintes pastas: 

    - % ProgramFiles% \ Assemblies do Microsoft ASP. NET\ASP.NET MVC 3 \
    - % ProgramFiles% \ Microsoft ASP. NET\ASP.NET Web Pages\v1.0\Assemblies
8. Em Gerenciador de Soluções, clique com o botão direito do mouse no nome do projeto e selecione descarregar projeto. Em seguida, clique com o botão direito do mouse no nome do projeto novamente e selecione Editar *ProjectName*. csproj.
9. Localize o elemento *ProjectTypeGuids* e substitua {F85E285D-A4E0-4152-9332-AB1D724D3325} por {E53F8FEA-EAE0-44A6-8774-FFD645390401}.
10. Salve as alterações, clique com o botão direito do mouse no projeto e selecione Recarregar projeto.
11. No arquivo Web. config da raiz do aplicativo, adicione as seguintes configurações à seção *assemblies* . 

    [!code-xml[Main](mvc3-release-notes/samples/sample3.xml)]
12. Se o projeto fizer referência a todas as bibliotecas de terceiros que são compiladas usando o ASP.NET MVC 2, adicione o seguinte elemento *bindingRedirect* realçado ao arquivo Web. config na raiz do aplicativo na seção de *configuração* : 

    [!code-xml[Main](mvc3-release-notes/samples/sample4.xml)]

<a id="tu-changes"></a>
## <a name="changes-in-aspnet-mvc-3-tools-update"></a>Alterações na atualização de ferramentas do ASP.NET MVC 3

Esta seção descreve as alterações feitas na versão de atualização das ferramentas do ASP.NET MVC 3 desde a versão RTM do ASP.NET MVC 3.

<a id="tu-AddControllerDialog"></a>
### <a name="add-controller-dialog-box-can-now-scaffold-controllers-with-views-and-data-access-code"></a>A caixa de diálogo "Adicionar controlador" agora pode Scaffold controladores com exibições e código de acesso a dados

Scaffolding é uma maneira de gerar rapidamente um controlador e exibições para seu aplicativo. Depois que o código tiver sido gerado, você poderá editá-lo para atender aos requisitos do seu projeto.

Para iniciar a caixa de diálogo *Adicionar controlador* no ASP.NET MVC 3, clique com o botão direito do mouse na pasta *controladores* em *Gerenciador de soluções*, clique em *Adicionar*e em *controlador*. A caixa de diálogo foi aprimorada para oferecer opções de scaffolding adicionais.

![](mvc3-release-notes/_static/image1.png)

Há três modelos de scaffolding disponíveis por padrão.

#### <a name="empty-controller"></a>Controlador vazio

Este modelo gera um arquivo de controlador vazio. Este modelo é equivalente a não verificar a *adição de ações para criar, editar, detalhar cenários de exclusão* em versões anteriores do ASP.NET MVC. Se você escolher esta opção, não haverá mais opções disponíveis.

#### <a name="controller-with-empty-readwrite-actions"></a>Controlador com ações de leitura/gravação vazias

Este modelo gera um arquivo de controlador que tem todos os métodos de ação necessários, mas nenhum código de implementação nos métodos. Este modelo é equivalente à verificação de *Adicionar ações para criar, editar, detalhar cenários de exclusão* em versões anteriores do ASP.NET MVC. Se você escolher esta opção, não haverá mais opções disponíveis.

#### <a name="controller-with-readwrite-actions-and-views-using-entity-framework"></a>Controlador com ações de leitura/gravação e exibições, usando Entity Framework

Este modelo permite que você crie rapidamente uma interface de usuário de entrada de dados de trabalho. Ele gera um código que manipula uma variedade de requisitos e cenários comuns, como o seguinte:

- *Acesso a dados*. O código gerado lê e grava entidades em um banco de dados. Ele funciona com a abordagem de Code First Entity Framework se você escolher uma classe de contexto de dados existente ou se permitir que o modelo gere uma nova classe *DbContext* . Ele também funciona com a abordagem Entity Framework Database First ou Model First se você escolher uma classe *ObjectContext* existente.
- *Validação*. O código gerado usa associação de modelo MVC ASP.NET e recursos de metadados para que os envios de formulário sejam validados de acordo com as regras declaradas em sua classe de modelo. Isso inclui regras de validação internas, como os atributos *Required* e *StringLength* , e regras de validação personalizadas.
- *Relações um-para-muitos*. Se você definir relações de chave estrangeira de um para muitos entre suas classes de modelo, o código gerado produzirá listas suspensas para selecionar entidades relacionadas. Por exemplo, você pode definir as seguintes classes de modelo seguindo as convenções de Code First de Entity Framework: 

    [!code-csharp[Main](mvc3-release-notes/samples/sample5.cs)]

    Quando você Scaffold um controlador para a classe *Product* , suas exibições permitirão que os usuários escolham um objeto *Category* para cada instância do *produto* .

    Este modelo habilita opções adicionais na caixa de diálogo *Adicionar controlador* . Para a *classe de modelo*, você pode escolher qualquer classe de modelo em sua solução, que determina o tipo de dados que os usuários poderão criar ou editar:
- Se você quiser usar Entity Framework Code First, poderá escolher qualquer classe de modelo.
- Se você estiver usando Entity Framework Database First ou Entity Framework Model First, certifique-se de escolher uma classe de entidade definida em seu modelo conceitual.

Para a *classe de contexto de dados*, você pode fazer estas escolhas:

- Se você quiser usar Code First e não tiver nenhuma classe de contexto de dados existente, escolha * * novo contexto de dados * *. Uma classe de contexto de dados será gerada para você.
- Se você quiser usar Code First e tiver uma classe de contexto de dados existente, escolha-a aqui. Ele será atualizado para persistir a classe de modelo que você selecionou.
- Se você estiver usando Database First ou Model First, escolha a classe de contexto do objeto aqui.

Para exibições, escolha o mecanismo de exibição que você deseja usar ou escolha nenhum se não quiser Scaffold nenhuma exibição.

Você pode selecionar opções avançadas para especificar mais opções para as exibições geradas. Por exemplo, você pode escolher o layout ou a página mestra a ser usada.

<a id="tu-ImprovementsNewDialogBox"></a>
### <a name="improvements-to-the-aspnet-mvc-3-new-project-dialog-box"></a>Melhorias na caixa de diálogo "ASP.NET MVC 3 New Project"

A caixa de diálogo que você usa para criar novos projetos do ASP.NET MVC 3 inclui vários aprimoramentos, conforme listado abaixo.

![](mvc3-release-notes/_static/image2.png)

#### <a name="new-intranet-project-template"></a>Novo modelo de "projeto de intranet"

A lista de modelos de projeto inclui um novo modelo de aplicativo de intranet. Este modelo contém configurações para criar um aplicativo Web usando a autenticação do Windows em vez da autenticação de formulários. Como um aplicativo de intranet requer algumas configurações do IIS que não podem ser encapsuladas em um modelo de projeto, o modelo inclui um arquivo Leiame com instruções sobre como fazer o modelo de projeto funcionar no IIS. A documentação para o novo modelo de aplicativo de intranet está disponível no site do MSDN na seguinte URL:

[https://msdn.microsoft.com/library/gg703322(VS.98).aspx](https://msdn.microsoft.com/library/gg703322(VS.98).aspx)

#### <a name="project-templates-are-now-html5-enabled"></a>Modelos de projeto agora estão habilitados para HTML5

A caixa de diálogo New-Project agora contém uma opção para adicionar recursos específicos do HTML5 aos modelos de projeto. A seleção da opção faz com que sejam geradas exibições que contêm os novos elementos `<header>`HTML5, `<footer>`e `<navigation>`.

Observe que versões anteriores de navegadores não oferecem suporte a marcas específicas do HTML5. Para resolver essa limitação, os modelos de projeto HTML5 incluem uma referência à biblioteca do Modernizr. (Consulte a próxima seção.)

<a id="tu-Modernizr"></a>
### <a name="project-templates-now-include-modernizr-17"></a>Os modelos de projeto agora incluem o Modernizr 1,7

O Modernizr é uma biblioteca JavaScript que habilita o suporte para CSS 3 e HTML5 em navegadores que ainda não dão suporte a esses recursos. Essa biblioteca é incluída como um pacote do NuGet pré-instalado em modelos para projetos do ASP.NET MVC 3. Para obter mais informações sobre o Modernizr, consulte [http://www.modernizr.com/](http://www.modernizr.com/).

<a id="tu-UpdatedJQuery"></a>
### <a name="project-templates-include-updated-versions-of-jquery-jquery-ui-and-jquery-validation"></a>Os modelos de projeto incluem versões atualizadas do jQuery, da interface do usuário do jQuery e da validação do jQuery

Os modelos de projeto agora incluem as seguintes versões dos scripts do jQuery:

- jQuery 1.5.1
- Validação do jQuery 1,8
- 1\.8.11 da IU do jQuery

Essas bibliotecas são incluídas como pacotes do NuGet pré-instalados.

<a id="tu-EF"></a>
### <a name="project-templates-now-include-adonet-entity-framework-41-as-a-pre-installed-nuget-package"></a>Os modelos de projeto agora incluem ADO.NET Entity Framework 4,1 como um pacote NuGet pré-instalado

O ADO.NET Entity Framework 4,1 inclui o recurso Code First. Code First é um novo padrão de desenvolvimento para o Entity Framework ADO.NET que fornece uma alternativa para os padrões existentes de Database First e Model First.

Code First concentra-se em definir seu modelo usando classes POCO ("objetos antigos do CLR") escritos em Visual Basic C#ou. Essas classes podem então ser mapeadas para um banco de dados existente ou usadas para gerar um esquema de banco de dados. A configuração adicional pode ser fornecida usando atributos *Annotations* ou usando APIs fluentes.

A documentação para usar o Code Firstwith ASP.NET MVC está disponível no site do ASP.NET nas seguintes URLs:

[https://www.asp.net/mvc/tutorials/getting-started-with-mvc3-part1-cs](../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) [https://www.asp.net/entity-framework/tutorials/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)

<a id="tu-JavaScriptLibsNuget"></a>
### <a name="project-templates-include-javascript-libraries-as-pre-installed-nuget-packages"></a>Os modelos de projeto incluem bibliotecas JavaScript como pacotes NuGet pré-instalados

Quando você cria um novo projeto do ASP.NET MVC 3, o projeto inclui os arquivos JavaScript mencionados anteriormente (por exemplo, a biblioteca do Modernizr), instalando-os usando o NuGet em vez de adicionar os scripts diretamente à pasta scripts no modelo de projeto índice. Isso permite que você use o NuGet para atualizar os scripts para a versão mais recente quando novas versões dos scripts são lançadas.

Por exemplo, considerando a frequência de novas versões do jQuery, a versão do jQuery incluída no modelo de projeto em algum momento estará desatualizada. No entanto, como o jQuery está incluído como um pacote NuGet instalado, você será notificado na caixa de diálogo NuGet quando versões mais recentes do jQuery estiverem disponíveis.

Como o jQuery inclui o número de versão no nome do arquivo, atualizar o jQuery para a versão mais recente também requer a atualização da marca de `<script>` que faz referência ao arquivo jQuery para usar o novo nome de arquivo. Outras bibliotecas de scripts incluídas não incluem o número de versão no nome do script, para que possam ser atualizadas com mais facilidade para suas versões mais recentes.

<a id="tu-KI"></a>
## <a name="known-issues"></a>Problemas Conhecidos

- Em alguns casos, a instalação pode falhar com a mensagem de erro "falha na instalação com código de erro (0x80070643)". Para obter informações sobre como contornar esse problema, consulte o [artigo 2531566 da base de conhecimento](https://support.microsoft.com/kb/2531566).
- O scaffolding para adicionar um controlador não Scaffold entidades que aproveitam o suporte à herança de entidade no Entity Framework. Por exemplo, considerando uma classe *Person* base herdada por uma classe *Student* , scaffolding a classe *Student* resultará em um código gerado que não é compilado.
- Criar um novo projeto do ASP.NET MVC 3 dentro de uma pasta de solução causa um erro *NullReferenceException* . A solução alternativa é criar o projeto do ASP.NET MVC 3 na raiz da solução e, em seguida, movê-lo para a pasta da solução.
- O IntelliSense para sintaxe Razor não funciona quando o remais nítido é instalado. Se você tiver o reASP.NET instalado e quiser aproveitar o suporte ao IntelliSense do Razor no MVC 3, consulte a entrada [Razor IntelliSense e](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) retomar no blog do Hadi Hariri, que discute maneiras de usá-los em conjunto hoje.
- Durante a instalação, a caixa de diálogo aceitação do EULA exibe os termos de licença em uma janela que é menor do que o esperado.
- Quando você estiver editando um modo de exibição Razor (. cshtml ou. *arquivo vbhtml* ), exibições. O ASP.NET MVC 3 não inclui nenhum trecho de código para exibições do Razor. aspxselecting um trecho de código para o ASP.NET MVC mostrará trechos para
- Se você instalar o ASP.NET MVC 3 para Visual Web Developer Express em um computador em que o Visual Studio não está instalado e depois instalar o Visual Studio, será necessário reinstalar o ASP.NET MVC 3. O Visual Studio e o Visual Web Developer Express compartilham componentes que são atualizados pelo instalador do ASP.NET MVC 3. O mesmo problema se aplica se você instalar o ASP.NET MVC 3 para Visual Studio em um computador que não tenha o Visual Web Developer Express e, depois, instalar o Visual Web Developer Express.

<a id="MVC3RTM"></a>
## <a name="changes-in-aspnet-mvc-3-rtm"></a>Alterações no ASP.NET MVC 3 RTM

Esta seção descreve as alterações e correções de bugs feitas na versão RTM do ASP.NET MVC 3 desde a versão RC2.

<a id="RTM-1"></a>
### <a name="change-updated-the-version-of-jquery-ui-to-187"></a>Alteração: atualização da versão da interface do usuário do jQuery para 1.8.7

Os modelos de projeto do ASP.NET MVC para Visual Studio foram atualizados para incluir a versão mais recente da biblioteca da interface do usuário do jQuery. Os modelos também incluem o conjunto mínimo de arquivos de recursos exigidos pela interface do usuário do jQuery, como os arquivos CSS e de imagem associados.

<a id="RTM-2"></a>
### <a name="change-changed-the-default-modelmetadataprovider-back-to-dataannotationsmodelmetadataprovider"></a>Alterar: o ModelMetadataProvider padrão foi alterado de volta para DataAnnotationsModelMetadataProvider

A versão RC2 do ASP.NET MVC 3 introduziu uma classe *CachedDataAnnotationsMetadataProvider* que fornecia o Caching sobre a classe *DataAnnotationsModelMetadataProvider* existente como uma melhoria no desempenho. No entanto, alguns bugs foram relatados com essa implementação, portanto, a alteração foi revertida e movida para o projeto de futuros do MVC, que está disponível em [ASP.net Webstack](https://github.com/aspnet/AspNetWebStack).

<a id="RTM-3"></a>
### <a name="fixed-pasting-part-of-a-razor-expression-that-contains-whitespace-results-in-it-being-reversed"></a>Corrigido: a colagem de parte de uma expressão Razor que contém os resultados de espaço em branco são revertidas

Nas versões de pré-lançamento do ASP.NET MVC 3, quando você cola uma parte de uma expressão Razor que contém espaço em branco em um arquivo Razor, a expressão resultante é invertida. Por exemplo, considere o seguinte bloco de código do Razor:

[!code-cshtml[Main](mvc3-release-notes/samples/sample6.cshtml)]

Se você selecionar o texto "primeiro param" no primeiro método e colá-lo como um argumento no segundo método, o resultado será o seguinte:

[!code-cshtml[Main](mvc3-release-notes/samples/sample7.cshtml)]

O comportamento correto é que a operação de colagem deve resultar no seguinte:

[!code-cshtml[Main](mvc3-release-notes/samples/sample8.cshtml)]

Esse problema foi corrigido na versão RTM para que a expressão seja preservada corretamente durante a operação de colagem.

<a id="RTM-4"></a>
### <a name="fixed-renaming-a-razor-file-that-is-opened-in-the-editor-disables-syntax-colorization-and-intellisense"></a>Corrigido: a renomeação de um arquivo Razor aberto no editor desabilita a colorização de sintaxe e o IntelliSense

Renomear um arquivo Razor usando Gerenciador de Soluções enquanto o arquivo é aberto na janela do editor faz com que o realce da sintaxe e o IntelliSense parem de funcionar para esse arquivo. Isso foi corrigido para que o realce e o IntelliSense sejam mantidos após uma renomeação.

<a id="RTM-KI"></a>
## <a name="known-issues"></a>Problemas Conhecidos

- Se você fechar o Visual Studio 2010 SP1 Beta enquanto o console do Gerenciador de pacotes NuGet estiver aberto, o Visual Studio falhará e tentará reiniciar. Isso será corrigido na versão RTM do Visual Studio 2010 SP1.
- O instalador do ASP.NET MVC 3 é capaz de instalar apenas uma versão inicial do Gerenciador de pacotes NuGet. Depois de instalar a versão inicial, o NuGet pode ser instalado e atualizado usando o Gerenciador de extensões do Visual Studio. Se você já tiver o NuGet instalado, vá para a Galeria de extensões do Visual Studio para atualizar para a versão mais recente do NuGet.
- Criar um novo projeto do ASP.NET MVC 3 dentro de uma pasta de solução causa um erro *NullReferenceException* . A solução alternativa é criar o projeto do ASP.NET MVC 3 na raiz da solução e, em seguida, movê-lo para a pasta da solução.
- O instalador pode levar muito mais tempo do que as versões anteriores do ASP.NET MVC para serem concluídas. Isso ocorre porque ele atualiza os componentes do Visual Studio 2010.
- O IntelliSense para sintaxe Razor não funciona quando o remais nítido é instalado. Se você tiver o reASP.NET instalado e quiser aproveitar o suporte ao IntelliSense do Razor no MVC 3, consulte a entrada [Razor IntelliSense e](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) retomar no blog do Hadi Hariri, que discute maneiras de usá-los em conjunto hoje.
- As exibições CCSHTML e VBHTML criadas com a versão beta do ASP.NET MVC 3 não têm suas ações de compilação definidas corretamente, com o resultado de que esses tipos de exibição são omitidos quando o projeto é publicado. O valor da ação de compilação para esses arquivos deve ser definido como "conteúdo". O ASP.NET MVC 3 RTM corrige esse problema para novos arquivos, mas não corrige a configuração de arquivos existentes para um projeto criado com versões de pré-lançamento.
- ![](mvc3-release-notes/_static/image3.png)
- Durante a instalação, a caixa de diálogo aceitação do EULA exibe os termos de licença em uma janela que é menor do que o esperado.
- Quando você estiver editando uma exibição Razor (arquivo. cshtml), o item de menu ir para controlador no Visual Studio não estará disponível e não haverá trechos de código.
- Se você instalar o ASP.NET MVC 3 para Visual Web Developer Express em um computador em que o Visual Studio não está instalado e depois instalar o Visual Studio, será necessário reinstalar o ASP.NET MVC 3. O Visual Studio e o Visual Web Developer Express compartilham componentes que são atualizados pelo instalador do ASP.NET MVC 3. O mesmo problema se aplica se você instalar o ASP.NET MVC 3 para Visual Studio em um computador que não tenha o Visual Web Developer Express e, depois, instalar o Visual Web Developer Express.

<a id="RTM-BC"></a>
## <a name="breaking-changes"></a>Alterações significativas

- Nas versões anteriores do ASP.NET MVC, os filtros de ação são criados por solicitação, exceto em alguns casos. Esse comportamento nunca foi um comportamento garantido, mas meramente um detalhe de implementação e o contrato de filtros era considerá-los sem monitoração de estado. No ASP.NET MVC 3, os filtros são armazenados em cache de forma mais agressiva. Portanto, qualquer filtro de ação personalizada que armazena incorretamente o estado da instância pode ser rompido.
- A ordem de execução dos filtros de exceção foi alterada para filtros de exceção que têm o mesmo valor de *ordem* . No ASP.NET MVC 2 e versões anteriores, os filtros de exceção no controlador que têm o mesmo valor de *ordem* que aqueles em um método de ação são executados antes dos filtros de exceção no método de ação. Normalmente, esse seria o caso quando os filtros de exceção são aplicados sem um valor de *ordem* especificado. No ASP.NET MVC 3, esse pedido foi revertido para que o manipulador de exceção mais específico seja executado primeiro. Como nas versões anteriores, se a propriedade *Order* for explicitamente especificada, os filtros serão executados na ordem especificada.
- Uma nova propriedade chamada *FileExtensions* foi adicionada à classe base *VirtualPathProviderViewEngine* . Quando ASP.NET pesquisa uma exibição pelo caminho (não por nome), somente as exibições com uma extensão de arquivo contida na lista especificada por essa nova propriedade são consideradas. Essa é uma alteração significativa nos aplicativos em que um provedor de compilação personalizado é registrado para habilitar uma extensão de arquivo Personalizada para exibições de formulário da Web e onde o provedor faz referência a essas exibições usando um caminho completo em vez de um nome. A solução alternativa é modificar o valor da propriedade *FileExtensions* para incluir a extensão de arquivo personalizado.
- As implementações de fábrica de controlador personalizado que implementam diretamente a interface *IControllerFactory* devem fornecer uma implementação do novo método *GetControllerSessionBehavior* que foi adicionado à interface nesta versão. Em geral, é recomendável que você não implemente essa interface diretamente e, em vez disso, derive sua classe de *DefaultControllerFactory*.

<a id="_Toc2"></a>
## <a name="changes-in-aspnet-mvc-3-rc2"></a>Alterações no ASP.NET MVC 3 RC2

Esta seção descreve as alterações (novos recursos e correções de bugs) feitas na versão do ASP.NET MVC 3 RC2 desde a versão RC.

<a id="_Toc2_1"></a>
### <a name="project-templates-changed-to-include-jquery-144-jquery-validation-17-and-jquery-ui-186"></a>Os modelos de projeto mudaram para incluir jQuery 1.4.4, jQuery Validation 1,7 e jQuery UI 1.8.6

Os modelos de projeto para o ASP.NET MVC 3 agora incluem as versões mais recentes do jQuery, da validação do jQuery e da interface do usuário do jQuery. a interface do usuário do jQuery é uma nova adição aos modelos de projeto e fornece widgets úteis da interface do usuário. Para obter mais informações sobre a interface do usuário do jQuery, visite sua home page: [http://jqueryui.com/](http://jqueryui.com/).

<a id="_Toc2_2"></a>
### <a name="added-additionalmetadataattribute-class"></a>Classe "AdditionalMetadataAttribute" adicionada

Você pode usar a classe *AdditionalMetadataAttribute* para popular o dicionário *ModelMetadata. AdditionalValues* para uma propriedade de modelo.

Por exemplo, suponha que um modelo de exibição tenha propriedades que devem ser exibidas somente para um administrador. Esse modelo pode ser anotado com o novo atributo usando AdminOnly como a chave e true como o valor, como no exemplo a seguir:

[!code-csharp[Main](mvc3-release-notes/samples/sample9.cs)]

Esses metadados são disponibilizados para qualquer modelo de exibição ou editor quando um modelo de exibição de produto é renderizado. Cabe a você como desenvolvedor de aplicativos interpretar as informações de metadados.

<a id="_Toc2_3"></a>
### <a name="improved-view-scaffolding"></a>Scaffolding de exibição aprimorada

Os modelos T4 usados para exibições scaffolding agora geram chamadas para métodos auxiliares de modelo, como *EditorFor* , em vez de auxiliares, como *TextBoxFor*. Essa alteração melhora o suporte para metadados no modelo na forma de atributos de anotação de dados quando a caixa de diálogo Adicionar exibição gera uma exibição.

A scaffolding de exibição Add também inclui detecção e uso aprimorados de informações de chave primária no modelo, com base na Convenção. Por exemplo, a caixa de diálogo Adicionar exibição usa essas informações para garantir que o valor da chave primária não seja com Scaffold como um campo de formulário editável.

Os modelos de editar e criar padrão incluem referências aos scripts jQuery necessários para a validação do cliente.

<a id="_Toc2_4"></a>
### <a name="added-htmlraw-method"></a>Método html. Raw adicionado

Por padrão, o código HTML do mecanismo de exibição Razor codifica todos os valores. Por exemplo, o trecho de código a seguir codifica o HTML dentro da variável greeting para que ele seja exibido na página como `<strong>Hello World!</strong>`.

[!code-cshtml[Main](mvc3-release-notes/samples/sample10.cshtml)]

O novo método *HTML. Raw* fornece uma maneira simples de exibir HTML não codificado quando o conteúdo é conhecido como seguro. O exemplo a seguir exibe a mesma cadeia de caracteres, mas a cadeia de caracteres é renderizada como marcação:

[!code-cshtml[Main](mvc3-release-notes/samples/sample11.cshtml)]

<a id="_Toc2_5"></a>
### <a name="renamed-controllerviewmodel-property-and-the-view-property-to-viewbag"></a>Propriedade "Controller. ViewModel" renomeada e a propriedade "View" como "ViewBag"

Anteriormente, a propriedade *ViewModel* do *controlador* corresponderam à propriedade *View* de uma exibição. Essas duas propriedades fornecem uma maneira de acessar valores do objeto *ViewDataDictionary* usando a sintaxe de acessador de propriedade dinâmica. Ambas as propriedades foram renomeadas para serem as mesmas para evitar confusão e serem mais consistentes.

<a id="_Toc2_6"></a>
### <a name="renamed-controllersessionstateattribute-class-to-sessionstateattribute"></a>A classe "ControllerSessionStateAttribute" foi renomeada como "SessionStateattribute"

A classe *ControllerSessionStateAttribute* foi introduzida na versão RC do ASP.NET MVC 3. A propriedade foi renomeada para ser mais sucinta.

<a id="_Toc2_7"></a>
### <a name="renamed-remoteattribute-fields-property-to-additionalfields"></a>Renomear a propriedade "Fields" de Remoteattribute como "AdditionalFields"

A propriedade *Fields* da classe *remoteattribute* causou uma confusão entre os usuários. Renomear essa propriedade como *AdditionalFields* esclarece sua intenção.

<a id="_Toc2_8"></a>
### <a name="renamed-skiprequestvalidationattribute-to-allowhtmlattribute"></a>"SkipRequestValidationAttribute" renomeado para "AllowHtmlAttribute"

O atributo *SkipRequestValidationAttribute* foi renomeado para *AllowHtmlAttribute* para representar melhor seu uso pretendido.

<a id="_Toc2_9"></a>
### <a name="changed-htmlvalidationmessage-method-to-display-the-first-useful-error-message"></a>O método "HTML. ValidationMessage" foi alterado para exibir a primeira mensagem de erro útil

O método *HTML. ValidationMessage* foi corrigido para mostrar a primeira mensagem de erro útil em vez de simplesmente exibir o primeiro erro.

Durante a associação de modelo, o dicionário *ModelState* pode ser populado de várias fontes com mensagens de erro sobre a propriedade, incluindo o próprio modelo (se ele implementa *IValidatableObject*), de atributos de validação aplicados à propriedade e de exceções geradas enquanto a propriedade está sendo acessada.

Quando o método *HTML. ValidationMessage* exibe uma mensagem de validação, ele ignora as entradas de estado de modelo que incluem uma exceção, pois elas geralmente não são pretendidas para o usuário final. Em vez disso, o método procura a primeira mensagem de validação que não está associada a uma exceção e exibe essa mensagem. Se nenhuma mensagem desse tipo for encontrada, o padrão será uma mensagem de erro genérica associada à primeira exceção.

<a id="_Toc2_10"></a>
### <a name="fixed-model-declaration-to-not-add-whitespace-to-the-document"></a>Correção @model declaração para não adicionar espaço em branco ao documento

Em versões anteriores, a declaração de `@model` na parte superior de uma exibição adicionou uma linha em branco à saída HTML renderizada. Isso foi corrigido para que a declaração não introduza o espaço em branco.

<a id="_Toc2_11"></a>
### <a name="added-fileextensions-property-to-view-engines-to-support-engine-specific-file-names"></a>Adicionada a propriedade "FileExtensions" para exibir mecanismos para dar suporte a nomes de arquivo específicos do mecanismo

Um mecanismo de exibição pode retornar uma exibição usando um caminho de exibição explícito, como no exemplo a seguir:

[!code-csharp[Main](mvc3-release-notes/samples/sample12.cs)]

O primeiro mecanismo de exibição sempre tenta renderizar a exibição. Por padrão, o mecanismo de exibição de Web Forms é o primeiro mecanismo de exibição; como o mecanismo de Web Forms não pode renderizar um modo de exibição Razor, ocorre um erro. Os mecanismos de exibição agora têm uma propriedade *FileExtensions* que é usada para especificar a quais extensões de arquivo eles dão suporte. Essa propriedade é verificada quando ASP.NET determina se um mecanismo de exibição pode renderizar um arquivo. Essa é uma alteração significativa e mais detalhes estão incluídos na seção de [alterações significativas](#_Toc2_BC) deste documento.

<a id="_Toc2_12"></a>
### <a name="fixed-labelfor-helper-to-emit-the-correct-value-for-the-for-attribute"></a>Correção do auxiliar "LabelFor" para emitir o valor correto para o atributo "for"

Um bug foi corrigido quando o método *LabelFor* renderizava um atributo *for* que corresponde ao atributo *Name* do elemento *Input* em vez de sua ID. De acordo com o W3C, o atributo *for* deve corresponder à ID do elemento de *entrada* .

<a id="_Toc2_13"></a>
### <a name="fixed-renderaction-method-to-give-explicit-values-precedence-during-model-binding"></a>Corrigido o método "Renderingaction" para dar precedência de valores explícitos durante a associação de modelo

Em versões anteriores, os valores explícitos que foram passados para o método *RenderAction* estavam sendo ignorados em favor dos valores do formulário atual durante a associação de modelo dentro de uma ação filho. A correção garante que os valores explícitos tenham precedência durante a associação de modelo.

<a id="_Toc2_BC"></a>
## <a name="breaking-changes"></a>Alterações significativas

- Nas versões anteriores do ASP.NET MVC, os filtros de ação foram criados por solicitação, exceto em alguns casos. Esse comportamento nunca foi um comportamento garantido, mas meramente um detalhe de implementação e o contrato de filtros era considerá-los sem monitoração de estado. No ASP.NET MVC 3, os filtros são armazenados em cache de forma mais agressiva. Portanto, qualquer filtro de ação personalizada que armazena incorretamente o estado da instância pode ser rompido.
- A ordem de execução dos filtros de exceção foi alterada para filtros de exceção que têm o mesmo valor de *ordem* . No ASP.NET MVC 2 e versões anteriores, os filtros de exceção no controlador que tinha o mesmo valor de *pedido* que aqueles em um método de ação foram executados antes dos filtros de exceção no método de ação. Normalmente, esse seria o caso quando filtros de exceção foram aplicados sem um valor de *ordem* especificado. No ASP.NET MVC 3, esse pedido foi revertido para que o manipulador de exceção mais específico seja executado primeiro. Como nas versões anteriores, se a propriedade *Order* for explicitamente especificada, os filtros serão executados na ordem especificada.
- Uma nova propriedade chamada *FileExtensions* foi adicionada à classe base *VirtualPathProviderViewEngine* . Quando ASP.NET pesquisa uma exibição pelo caminho (não por nome), somente as exibições com uma extensão de arquivo contida na lista especificada por essa nova propriedade são consideradas. Essa é uma alteração significativa nos aplicativos em que um provedor de compilação personalizado é registrado para habilitar uma extensão de arquivo Personalizada para exibições de formulário da Web e onde o provedor faz referência a essas exibições usando um caminho completo em vez de um nome. A solução alternativa é modificar o valor da propriedade *FileExtensions* para incluir a extensão de arquivo personalizado.
- As implementações de fábrica de controlador personalizado que implementam diretamente a interface *IControllerFactory* devem fornecer uma implementação do novo método *GetControllerSessionBehavior* que foi adicionado à interface nesta versão. Em geral, é recomendável que você não implemente essa interface diretamente e, em vez disso, derive sua classe de *DefaultControllerFactory*.

<a id="_Toc2_KI"></a>
## <a name="known-issues"></a>Problemas Conhecidos

- O instalador do ASP.NET MVC 3 é capaz de instalar apenas uma versão inicial do Gerenciador de pacotes NuGet. Depois de instalar a versão inicial, o NuGet pode ser instalado e atualizado usando o Gerenciador de extensões do Visual Studio. Se você já tiver o NuGet instalado, vá para a Galeria de extensões do Visual Studio para atualizar para a versão mais recente do NuGet.
- Criar um novo projeto do ASP.NET MVC 3 dentro de uma pasta de solução causa um erro *NullReferenceException* . A solução alternativa é criar o projeto do ASP.NET MVC 3 na raiz da solução e, em seguida, movê-lo para a pasta da solução.
- O instalador pode levar muito mais tempo do que as versões anteriores do ASP.NET MVC para serem concluídas. Isso ocorre porque ele atualiza os componentes do Visual Studio 2010.
- O IntelliSense para sintaxe Razor não funciona quando o remais nítido é instalado. Se você tiver o reASP.NET instalado e desejar tirar proveito do suporte ao IntelliSense do Razor no Hadi MVC 3 RC2, consulte a entrada [Razor IntelliSense e](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) retomar no blog do Hariri, que discute maneiras de usá-los em conjunto hoje.
- As exibições CSHTML e VBHTML criadas com a versão beta do ASP.NET MVC 3 não têm suas ações de compilação definidas corretamente, com o resultado de que esses tipos de exibição são omitidos quando o projeto é publicado. O valor da *ação de compilação* para esses arquivos deve ser definido como conteúdo ". O ASP.NET MVC 3 RC2 corrige esse problema para novos arquivos, mas não corrige a configuração de arquivos existentes para um projeto criado com a versão beta.![](mvc3-release-notes/_static/image4.png)
- Durante a instalação, a caixa de diálogo aceitação do EULA exibe os termos de licença em uma janela que é menor do que o esperado.
- Quando você estiver editando uma exibição Razor (arquivo. cshtml), o item de menu ir para controlador no Visual Studio não estará disponível e não haverá trechos de código.
- Se você instalar o ASP.NET MVC 3 para Visual Web Developer Express em um computador em que o Visual Studio não está instalado e depois instalar o Visual Studio, será necessário reinstalar o ASP.NET MVC 3. O Visual Studio e o Visual Web Developer Express compartilham componentes que são atualizados pelo instalador do ASP.NET MVC 3. O mesmo problema se aplica se você instalar o ASP.NET MVC 3 para Visual Studio em um computador que não tenha o Visual Web Developer Express e, depois, instalar o Visual Web Developer Express.
- A instalação do ASP.NET MVC 3 RC 2 não atualiza o NuGet se você já o tiver instalado. Para atualizar o NuGet, vá para o Gerenciador de extensões do Visual Studio e ele deve aparecer como uma atualização disponível. Você pode atualizar o NuGet para a versão mais recente a partir daí.

<a id="TOC_ASP_NET_3_RC"></a>
## <a name="aspnet-mvc-3-release-candidate"></a>ASP.NET MVC 3 versão Release Candidate

O ASP.NET MVC Release Candidate foi lançado em 9 de novembro de 2010.

<a id="_Toc276711785"></a>
## <a name="new-features-in-aspnet-mvc-3-rc"></a>Novos recursos no ASP.NET MVC 3 RC

Esta seção descreve os recursos que foram introduzidos na versão RC do ASP.NET MVC 3 desde a versão beta.

<a id="_Toc276711786"></a>
### <a name="nuget-package-manager"></a>Gerenciador de pacotes do NuGet

O ASP.NET MVC 3 inclui o Gerenciador de pacotes NuGet (anteriormente conhecido como NuPack), que é uma ferramenta de gerenciamento de pacotes integrada para adicionar bibliotecas e ferramentas a projetos do Visual Studio. Essa ferramenta automatiza as etapas que os desenvolvedores levam hoje para obter uma biblioteca em sua árvore de origem.

Você pode trabalhar com o NuGet como uma ferramenta de linha de comando, como uma janela de console integrada dentro do Visual Studio 2010, no menu de contexto do Visual Studio e como um conjunto de cmdlets do PowerShell.

Para obter mais informações sobre o NuGet, leia a [documentação do NuGet](https://docs.microsoft.com/nuget/).

<a id="_Toc276711787"></a>
### <a name="improved-new-project-dialog-box"></a>Caixa de diálogo "novo projeto" aprimorada

Quando você cria um novo projeto, a caixa de diálogo novo projeto agora permite especificar o mecanismo de exibição, bem como um tipo de projeto ASP.NET MVC.

![](mvc3-release-notes/_static/image5.png)

O suporte para modificar a lista de modelos e mecanismos de exibição listados na caixa de diálogo está incluído nesta versão.

Os modelos padrão são os seguintes:

Vazio. Contém um conjunto mínimo de arquivos para um projeto MVC ASP.NET, incluindo a estrutura de diretório padrão para projetos MVC ASP.NET, um arquivo site. CSS que contém os estilos padrão MVC ASP.NET e um diretório de scripts que contém os arquivos JavaScript padrão.

Aplicativo de Internet. Contém a funcionalidade de exemplo que demonstra como usar o provedor de associação com o ASP.NET MVC.

A lista de modelos de projeto que é exibida na caixa de diálogo é especificada no registro do Windows.

<a id="_Toc276711788"></a>
### <a name="sessionless-controllers"></a>Controladores de sessão

O novo *ControllerSessionStateAttribute* oferece mais controle sobre o comportamento de estado da sessão para controladores especificando um valor de enumeração [System. Web. SessionState. SessionStateBehavior](https://msdn.microsoft.com/library/system.web.sessionstate.sessionstatebehavior.aspx) .

O exemplo a seguir mostra como desativar o estado de sessão para todas as solicitações para um controlador.

[!code-csharp[Main](mvc3-release-notes/samples/sample13.cs)]

O exemplo a seguir mostra como definir o estado de sessão somente leitura para todas as solicitações para um controlador.

[!code-csharp[Main](mvc3-release-notes/samples/sample14.cs)]

<a id="_Toc276711789"></a>
### <a name="new-validation-attributes"></a>Novos atributos de validação

#### <a name="compareattribute"></a>CompareAttribute

O novo atributo de validação *compareAttribute* permite comparar os valores de duas propriedades diferentes de um modelo. No exemplo a seguir, a propriedade *ComparePassword* deve corresponder ao campo *password* para ser válida.

[!code-csharp[Main](mvc3-release-notes/samples/sample15.cs)]

#### <a name="remoteattribute"></a>Controle remoto

O novo atributo de validação *remoteattribute* aproveita o validador remoto do plug-in de validação do jQuery, que permite que a validação do lado do cliente chame um método no servidor que executa a lógica de validação real.

No exemplo a seguir, a propriedade *username* tem o *remoteattribute* aplicado. Ao editar essa propriedade em um modo de exibição de edição, a validação do cliente chamará uma ação chamada *UserNameAvailable* na classe *UsersController* para validar esse campo.

[!code-csharp[Main](mvc3-release-notes/samples/sample16.cs)]

O exemplo a seguir mostra o controlador correspondente.

[!code-csharp[Main](mvc3-release-notes/samples/sample17.cs)]

Por padrão, o nome da propriedade ao qual o atributo é aplicado é enviado para o método de ação como um parâmetro de cadeia de caracteres de consulta.

<a id="_Toc276711790"></a>
### <a name="new-overloads-for-labelfor-and-labelformodel-methods"></a>Novas sobrecargas para métodos "LabelFor" e "LabelForModel"

Foram adicionadas novas sobrecargas para os métodos *LabelFor* e *LabelForModel* que permitem especificar o texto do rótulo. O exemplo a seguir mostra como usar essas sobrecargas.

[!code-cshtml[Main](mvc3-release-notes/samples/sample18.cshtml)]

<a id="_Toc276711791"></a>
### <a name="child-action-output-caching"></a>Cache de saída de ação filho

O *OutputCacheAttribute* dá suporte ao cache de saída de ações filhas que são chamadas usando os métodos auxiliares *HTML. RenderAction* ou *HTML. Action* . O exemplo a seguir mostra uma exibição que chama outra ação.

[!code-cshtml[Main](mvc3-release-notes/samples/sample19.cshtml)]

A ação *GETDATE* é anotada com o *OutputCacheAttribute*:

[!code-csharp[Main](mvc3-release-notes/samples/sample20.cs)]

Quando esse código é executado, o resultado da chamada para HTML. Action ("GetDate") é armazenado em cache por 100 segundos.

<a id="_Toc276711792"></a>
### <a name="add-view-dialog-box-improvements"></a>Aprimoramentos da caixa de diálogo "Adicionar exibição"

Quando você adiciona uma exibição fortemente tipada, a caixa de diálogo Adicionar exibição agora filtra mais tipos não aplicáveis do que em versões anteriores, como muitos tipos de .NET Framework de núcleo. Além disso, a lista agora está classificada pelo nome da classe e não pelo nome do tipo totalmente qualificado, o que torna mais fácil localizar tipos. Por exemplo, o nome do tipo agora é exibido como no exemplo a seguir:

ClassName (namespace)

Em versões anteriores, isso teria sido exibido da seguinte maneira:

Namespace. ClassName

<a id="_Toc276711793"></a>
### <a name="granular-request-validation"></a>Validação de solicitação granular

A propriedade *Exclude* de *ValidateInputAttribute* não existe mais. Em vez disso, para que a validação de solicitação seja ignorada para propriedades específicas de um modelo durante a associação de modelo, use o novo *SkipRequestValidationAttribute*.

Por exemplo, suponha que um método de ação seja usado para editar uma postagem no blog:

[!code-csharp[Main](mvc3-release-notes/samples/sample21.cs)]

O exemplo a seguir mostra o modelo de exibição para uma postagem de blog.

[!code-csharp[Main](mvc3-release-notes/samples/sample22.cs)]

Quando um usuário envia alguma marcação para a propriedade Description, a associação de modelo falhará devido à validação da solicitação. Para desabilitar a validação de solicitação durante a associação de modelo para a descrição da postagem do blog, aplique o *SkipRequpestValidationAttribute* à propriedade, conforme mostrado neste exemplo:.

[!code-csharp[Main](mvc3-release-notes/samples/sample23.cs)]

Como alternativa, para desativar a validação de solicitação para cada propriedade do modelo, aplique *ValidateInputAttribute* com um valor de *false* ao método de ação:

[!code-csharp[Main](mvc3-release-notes/samples/sample24.cs)]

<a id="_Toc276711794"></a>
## <a name="breaking-changes"></a>Alterações significativas

- A ordem de execução dos filtros de exceção foi alterada para filtros de exceção que têm o mesmo valor de *ordem* . No ASP.NET MVC 2 e versões anteriores, os filtros de exceção no controlador que tinha a mesma *ordem* que aqueles em um método de ação foram executados antes dos filtros de exceção no método de ação. Normalmente, esse seria o caso quando filtros de exceção foram aplicados sem um valor de *ordem* especificado. No ASP.NET MVC 3, esse pedido foi revertido para que o manipulador de exceção mais específico seja executado primeiro. Como nas versões anteriores, se a propriedade *Order* for explicitamente especificada, os filtros serão executados na ordem especificada.
- Adicionada uma nova propriedade chamada *FileExtensions* à classe base *VirtualPathProviderViewEngine* . Ao pesquisar uma exibição por caminho (e não por nome), somente as exibições com uma extensão de arquivo contida na lista especificada por essa nova propriedade serão consideradas. Essa é uma alteração significativa para aqueles que registram um provedor de compilação personalizado para habilitar uma extensão de arquivo Personalizada para exibições de formulário da Web e estão fazendo referência a essas exibições usando um caminho completo em vez de um nome. A solução alternativa é modificar o valor da propriedade *FileExtensions* para incluir a extensão de arquivo personalizado.

<a id="_Toc276711795"></a>
## <a name="known-issues"></a>Problemas Conhecidos

- O instalador pode levar muito mais tempo do que as versões anteriores do ASP.NET MVC para serem concluídas porque atualiza os componentes do Visual Studio 2010.
- O Add View scaffolding ao selecionar a exibição tipada astrongly aplica Scaffold propriedades somente gravação. Eles devem ser sempre ignorados pelo scaffolding. A caixa de diálogo Adicionar exibição também aplica Scaffold propriedades somente leitura ao gerar uma exibição "Editar" ou "criar". As propriedades somente leitura só devem ser com Scaffold para as exibições de exibição e lista.
- A depuração não funciona quando o ASP.NET MVC 3 é instalado junto com a CTP assíncrona. O ASP.NET MVC 3 não pode ser instalado lado a lado com o CTP assíncrono. Desinstale a CTP assíncrona para reparar a depuração. Para obter mais detalhes, leia [esta postagem de blog](http://drew-prog.blogspot.com/2010/11/how-to-uninstall-microsoft-aspnet-mvc-3.html) sobre como desinstalar todas as partes do ASP.NET MVC 3 RC.
- O Razor IntelliSense não funciona quando o remais nítido é instalado. Se você tiver o reASP.NET instalado e desejar tirar proveito do suporte ao IntelliSense do Razor no MVC 3 RC, leia [esta postagem de blog](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) de JetBrains que discute maneiras de usá-las em conjunto hoje.
- As exibições CSHTML e VBHTML criadas com beta do ASP.NET MVC 3 não têm sua ação de compilação corretamente, o que as omite da publicação. A *ação de Build* para esses arquivos deve ser definida como "conteúdo". O ASP.NET MVC 3 RC corrige esse problema para novos arquivos, mas não corrige a configuração de arquivos existentes para um projeto criado com a versão beta.
- O instalador pode levar muito mais tempo do que as versões anteriores do ASP.NET MVC para serem concluídas porque atualiza os componentes do Visual Studio 2010.
- O Add View scaffolding ao selecionar uma exibição com rigidez de tipos aplica Scaffold propriedades somente leitura. Da mesma forma, as propriedades somente gravação são com Scaffold para exibições de "exibição".
- Durante a instalação, a caixa de diálogo aceitação do EULA exibe os termos de licença em uma janela que é menor do que o esperado.
- A instalação do Visual Studio Async CTP causa um conflito com a versão Razor incluída como parte da instalação de ferramentas do ASP.NET MVC 3. Certifique-se de não tentar instalar o Visual Studio Async CTP e a versão Razor no mesmo computador.
- Quando você estiver editando uma exibição Razor (arquivo. cshtml), o item de menu ir para controlador no Visual Studio não estará disponível e não haverá trechos de código.

<a id="TOC_ASP_NET_3_Beta"></a>
## <a name="aspnet-mvc-3-beta"></a>ASP.NET MVC 3 beta

O ASP.NET MVC 3 beta foi lançado em 6 de outubro de 2010. As observações a seguir são específicas para a versão beta e estão sujeitas a quaisquer atualizações ou alterações referenciadas na seção Release Candidate do ASP.NET MVC 3 acima.

## <a id="0.1__Toc274034215"></a>Novos recursos do ASP.NET MVC 3 beta

<a id="0.1__Default_validation_system"></a>Esta seção descreve os recursos que foram introduzidos na versão beta do ASP.NET MVC 3.

### <a id="0.1__Toc274034216"></a>Gerenciador de pacotes NuGet

O ASP.NET MVC 3 inclui o Gerenciador de pacotes NuGet, que é uma ferramenta de gerenciamento de pacotes integrada para adicionar bibliotecas e ferramentas a projetos do Visual Studio. Na maior parte, Ele automatiza as etapas que os desenvolvedores levam hoje para obter uma biblioteca em sua árvore de origem.

Você pode trabalhar com o NuGet como uma ferramenta de linha de comando, como uma janela de console integrada dentro do Visual Studio 2010, no menu de contexto do Visual Studio e como um conjunto de cmdlets do PowerShell.

Para obter mais informações sobre o NuGet, leia a [documentação do NuGet](https://docs.microsoft.com/nuget/).

### <a id="0.1__Toc274034217"></a>Caixa de diálogo novo projeto aprimorada

Quando você cria um novo projeto, a caixa de diálogo novo projeto agora permite especificar o mecanismo de exibição, bem como um tipo de projeto ASP.NET MVC.

![](mvc3-release-notes/_static/image6.png)

O suporte para modificar a lista de modelos e mecanismos de exibição listados na caixa de diálogo não está incluído nesta versão.

Os modelos padrão são os seguintes:

Vazio. Contém um conjunto mínimo de arquivos para um projeto MVC ASP.NET, incluindo a estrutura de diretório padrão para projetos MVC ASP.NET, um arquivo. CSS de um pequeno site que contém os estilos padrão MVC ASP.NET e um diretório de scripts que contém os arquivos JavaScript padrão.

Aplicativo de Internet. Contém a funcionalidade de exemplo que demonstra como usar o provedor de associação dentro do ASP.NET MVC.

### <a id="0.1__Toc274034218"></a>Maneira simplificada de especificar modelos fortemente tipados em exibições do Razor

A maneira de especificar o tipo de modelo para exibições do Razor com rigidez de tipos foi simplificada usando a nova diretiva @model para exibições de CSHTML e @ModelType diretiva para exibições de VBHTML. Em versões anteriores do ASP.NET MVC, você especificaria um modelo fortemente tipado para exibições do Razor desta forma:

[!code-cshtml[Main](mvc3-release-notes/samples/sample25.cshtml)]

Nesta versão, você pode usar a seguinte sintaxe:

[!code-cshtml[Main](mvc3-release-notes/samples/sample26.cshtml)]

### <a id="0.1__Toc274034219"></a>Suporte para novos métodos auxiliares Páginas da Web do ASP.NET

A nova tecnologia de Páginas da Web do ASP.NET inclui um conjunto de métodos auxiliares que são úteis para adicionar funcionalidades comumente usadas a exibições e controladores. O ASP.NET MVC 3 dá suporte ao uso desses métodos auxiliares dentro de controladores e exibições (quando apropriado). Esses métodos estão contidos no assembly System. Web. Helpers. A tabela a seguir lista alguns dos métodos auxiliares Páginas da Web do ASP.NET.

| **Auxiliar** | **Descrição** |
| --- | --- |
| Gráfico | Renderiza um gráfico em uma exibição. Contém métodos como Chart. ToWebImage, Char. Save e Chart. Write. |
| Criptografia | Usa algoritmos de hash para criar senhas com Salt e hash corretamente. |
| WebGrid | Renderiza uma coleção de objetos (normalmente, dados de um banco de dado) como uma grade. Dá suporte à paginação e classificação. |
| WebImage | Renderiza uma imagem. |
| WebMail | Envia uma mensagem de email. |

Um tópico de referência rápida que lista os auxiliares e a sintaxe básica está disponível como parte da documentação de sintaxe Razor do ASP.NET na seguinte URL:

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-api-reference](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)

### <a id="0.1__Toc274034220"></a>Suporte adicional à injeção de dependência

Com base na versão do ASP.NET MVC 3 Preview 1, a versão atual inclui suporte adicional para dois novos serviços e quatro serviços existentes e suporte aprimorado para a resolução de dependência e o localizador de serviço comum.

#### <a name="new-icontrolleractivator-interface-for-fine-grained-controller-instantiation"></a>Nova interface IControllerActivator para instanciação de controlador refinada

A nova interface IControllerActivator fornece um controle mais refinado sobre como os controladores são instanciados por meio da injeção de dependência. O exemplo a seguir mostra a interface:

[!code-csharp[Main](mvc3-release-notes/samples/sample27.cs)]

Compare isso com a função da fábrica do controlador. Um alocador de controlador é uma implementação da interface IControllerFactory, que é responsável por localizar um tipo de controlador e instanciar uma instância desse tipo de controlador.

Ativadores de controlador são responsáveis apenas por instanciar uma instância de um tipo de controlador. Eles não executam a pesquisa de tipo de controlador. Depois de localizar um tipo de controlador apropriado, as fábricas de controlador devem delegar a uma instância de IControllerActivator para lidar com a instanciação real do controlador.

A classe DefaultControllerFactory tem um novo construtor que aceita uma instância de IControllerFactory. Isso permite aplicar injeção de dependência para gerenciar esse aspecto da criação do controlador sem a necessidade de substituir o comportamento de pesquisa do tipo de controlador padrão.

#### <a name="iservicelocator-interface-replaced-with-idependencyresolver"></a>Interface IServiceLocator substituída por IDependencyResolver

Com base nos comentários da Comunidade, a versão beta do ASP.NET MVC 3 substituiu o uso da interface IServiceLocator com uma interface IDependencyResolver mais lenta, específica às necessidades do ASP.NET MVC. O exemplo a seguir mostra a nova interface:

[!code-csharp[Main](mvc3-release-notes/samples/sample28.cs)]

Como parte dessa alteração, a classe do objectlocator também foi substituída pela classe DependencyResolver. O registro de um resolvedor de dependência é semelhante às versões anteriores do ASP.NET MVC:

[!code-csharp[Main](mvc3-release-notes/samples/sample29.cs)]

As implementações dessa interface devem simplesmente delegar ao contêiner de injeção de dependência subjacente para fornecer o serviço registrado para o tipo solicitado.

Quando não há serviços registrados do tipo solicitado, o ASP.NET MVC espera que as implementações dessa interface retornem NULL de GetService e retornem uma coleção vazia de GetServices.

A nova classe DependencyResolver permite que você registre classes que implementam a nova interface IDependencyResolver ou a interface do localizador de serviço (IServiceLocator) comum. Para obter mais informações sobre o localizador de serviço comum, consulte [CommonServiceLocator no GitHub](https://github.com/unitycontainer/commonservicelocator).

<a id="0.1__Breaking_Changes"></a>

#### <a name="new-iviewactivator-interface-for-fine-grained-view-page-instantiation"></a>Nova interface IViewActivator para a instanciação da página de exibição refinada

A nova interface IViewPageActivator fornece um controle mais refinado sobre como as páginas de exibição são instanciadas por meio da injeção de dependência. Isso se aplica a instâncias do WebFormView e instâncias de RazorView. O exemplo a seguir mostra a nova interface:

[!code-csharp[Main](mvc3-release-notes/samples/sample30.cs)]

Essas classes agora aceitam um argumento de Construtor IViewPageActivator, que permite usar injeção de dependência para controlar como os tipos ViewPage, ViewUserControl e WebViewPage são instanciados.

#### <a name="new-dependency-resolver-support-for-existing-services"></a>Novo suporte ao resolvedor de dependência para serviços existentes

A nova versão inclui suporte à resolução de dependência para os seguintes serviços:

- Provedores de validação de modelo. As classes que implementam ModelValidatorProvider podem ser registradas no resolvedor de dependência e o sistema as usará para dar suporte à validação do lado do cliente e do servidor.
- Provedor de metadados de modelo. Uma única classe que implementa ModelMetadataProvider pode ser registrada no resolvedor de dependência e o sistema a usará para fornecer metadados para os sistemas de modelagem e validação.
- Provedores de valor. As classes que implementam ValueProviderFactory podem ser registradas no resolvedor de dependência e o sistema as usará para criar provedores de valor que são consumidos pelo controlador e durante a associação de modelo.
- ASSOCIADORES de modelo. As classes que implementam IModelBinderProvider podem ser registradas no resolvedor de dependência e o sistema as usará para criar ASSOCIADORES de modelo consumidos pelo sistema de associação de modelo.

### <a id="0.1__Toc274034221"></a>Novo suporte para AJAX não intrusivo baseado em jQuery

O ASP.NET MVC inclui métodos auxiliares do Ajax, como o seguinte:

- Ajax. ActionLink
- Ajax. RouteLink
- Ajax. BeginForm
- Ajax. BeginRouteForm

Esses métodos usam o JavaScript para invocar um método de ação no servidor em vez de usar um postback completo. Essa funcionalidade foi atualizada para aproveitar o jQuery de maneira discreta. Em vez de emitir os scripts de cliente embutidos de forma invasiva, esses métodos auxiliares separam o comportamento da marcação emitindo atributos HTML5 usando o prefixo *Data-Ajax* . O comportamento é então aplicado à marcação referenciando os arquivos JavaScript apropriados. Verifique se os seguintes arquivos JavaScript são referenciados:

- jQuery-1.4.1. js
- jQuery. não invasivo. Ajax. js

Esse recurso é habilitado por padrão no arquivo Web. config nos modelos de novo projeto do ASP.NET MVC 3, mas está desabilitado por padrão para projetos existentes. Para obter mais informações, consulte [sinalizadores de todo o aplicativo adicionados para validação de cliente e JavaScript discreto](#0.1_AddedApplicationWideFlagsForClientValida) mais adiante neste documento.

### <a id="0.1__Toc274034222"></a>Novo suporte para validação não invasiva do jQuery

Por padrão, o ASP.NET MVC 3 beta usa a validação do jQuery de maneira discreta para executar a validação do lado do cliente. Para habilitar a validação de cliente discreta, faça uma chamada como a seguinte de dentro de uma exibição:

[!code-csharp[Main](mvc3-release-notes/samples/sample31.cs)]

Isso requer que a Propriedade ViewContext. UnobtrusiveJavaScriptEnabled seja definida como true, o que pode ser feito fazendo a seguinte chamada:

[!code-csharp[Main](mvc3-release-notes/samples/sample32.cs)]

Verifique também se os seguintes arquivos JavaScript são referenciados.

- jQuery-1.4.1. js
- jQuery. Validate. js
- jQuery. Validate. discreta. js

Esse recurso está habilitado no por padrão no arquivo Web. config nos novos modelos de projeto do ASP.NET MVC 3, mas está desabilitado por padrão para projetos existentes. Para obter mais informações, consulte [novos sinalizadores de todo o aplicativo para validação de cliente e JavaScript discreto](#0.1_AddedApplicationWideFlagsForClientValida) mais adiante neste documento.

<a id="0.1__Toc274034223"></a>

### <a id="0.1_AddedApplicationWideFlagsForClientValida"></a>Novos sinalizadores de todo o aplicativo para validação de cliente e JavaScript discreto

Você pode habilitar ou desabilitar a validação do cliente e o JavaScript discreto globalmente usando membros estáticos da classe HtmlHelper, como no exemplo a seguir:

[!code-csharp[Main](mvc3-release-notes/samples/sample33.cs)]

Os modelos de projeto padrão habilitam JavaScript discreto por padrão. Você também pode habilitar ou desabilitar esses recursos no arquivo Web. config raiz do seu aplicativo usando as seguintes configurações:

[!code-xml[Main](mvc3-release-notes/samples/sample34.xml)]

Como você pode habilitar esses recursos por padrão, novas sobrecargas foram introduzidas na classe HtmlHelper, que permitem que você substitua as configurações padrão, conforme mostrado nos exemplos a seguir:

[!code-csharp[Main](mvc3-release-notes/samples/sample35.cs)]

Para compatibilidade com versões anteriores, esses dois recursos são desabilitados por padrão.

### <a id="0.1__Toc274034224"></a>Novo suporte para o código que é executado antes da execução de views

Agora você pode colocar um arquivo chamado \_viewstart. cshtml (ou \_viewstart. vbhtml) no diretório views e adicionar código a ele que será compartilhado entre várias exibições nesse diretório e em seus subdiretórios. Por exemplo, você pode colocar o código a seguir na página \_viewstart. cshtml na pasta ~/views:

[!code-cshtml[Main](mvc3-release-notes/samples/sample36.cshtml)]

Isso define a página de layout para cada exibição dentro da pasta views e todas as suas subpastas de forma recursiva. Quando uma exibição está sendo processada, o código no arquivo \_viewstart. cshtml é executado antes da execução do código de exibição. O código \_viewstart. cshtml se aplica a todas as exibições nessa pasta.

Por padrão, o código no arquivo \_viewstart. cshtml também se aplica a exibições em qualquer subpasta. No entanto, as subpastas individuais podem ter sua própria versão do arquivo \_viewstart. cshtml; Nesse caso, a versão local tem precedência. Por exemplo, para executar o código que é comum a todas as exibições para o HomeController, coloque um arquivo de \_viewstart. cshtml na pasta ~/Views/Home.

### <a id="0.1__Toc274034225"></a>Novo suporte para a sintaxe VBHTML Razor

A versão prévia anterior do ASP.NET MVC incluía suporte para exibições C#usando sintaxe Razor com base em. Essas exibições usam a extensão de arquivo. cshtml. Como parte do trabalho em andamento para dar suporte a Razor, o ASP.NET MVC 3 beta apresenta suporte para o sintaxe Razor no Visual Basic, que usa a extensão de arquivo. vbhtml.

Para obter uma introdução ao uso da sintaxe Visual Basic em páginas VBHTML, consulte o tutorial na seguinte URL:

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-visual-basic](../web-pages/overview/getting-started/introducing-razor-syntax-vb.md)

### <a id="0.1__Toc274034226"></a>Controle mais granular sobre ValidateInputAttribute

O ASP.NET MVC sempre incluiu a classe ValidateInputAttribute, que invoca a infraestrutura de validação de solicitação do core ASP.NET para garantir que a solicitação de entrada não contenha entradas potencialmente mal-intencionadas. Por padrão, a validação de entrada está habilitada. É possível desabilitar a validação de solicitação usando o atributo ValidateInputAttribute, como no exemplo a seguir:

[!code-csharp[Main](mvc3-release-notes/samples/sample37.cs)]

No entanto, muitos aplicativos Web têm campos de formulário individuais que precisam permitir HTML, enquanto os campos restantes não deveriam. A classe ValidateInputAttribute agora permite que você especifique uma lista de campos que não devem ser incluídos na validação da solicitação.

Por exemplo, se você estiver desenvolvendo um mecanismo de blog, talvez queira permitir a marcação nos campos corpo e resumo. Esses campos podem ser representados por dois elementos Input, cada um com um atributo Name correspondente ao nome da propriedade ("Body" e "Summary"). Para desabilitar a validação de solicitação somente para esses campos, especifique os nomes (separados por vírgula) na propriedade Exclude da classe ValidateInput, como no exemplo a seguir:

[!code-csharp[Main](mvc3-release-notes/samples/sample38.cs)]

### <a id="0.1__Toc274034227"></a>Os auxiliares convertem sublinhados em hifens para nomes de atributo HTML especificados usando objetos anônimos

Os métodos auxiliares permitem que você especifique pares de nome/valor de atributo usando um objeto anônimo, como no exemplo a seguir:

[!code-csharp[Main](mvc3-release-notes/samples/sample39.cs)]

Essa abordagem não permite que você use hifens no nome do atributo, porque um hífen não pode ser usado para um nome de propriedade em ASP.NET. No entanto, hifens são importantes para atributos HTML5 personalizados; por exemplo, o HTML5 usa o prefixo "data-".

Ao mesmo tempo, sublinhados não podem ser usados para nomes de atributo em HTML, mas são válidos dentro de nomes de propriedade. Portanto, se você especificar atributos usando um objeto anônimo, e se os nomes de atributo incluírem um sublinhado, os métodos auxiliares converterão os sublinhados em hifens. Por exemplo, a seguinte sintaxe auxiliar usa um sublinhado:

[!code-csharp[Main](mvc3-release-notes/samples/sample40.cs)]

O exemplo anterior renderiza a seguinte marcação quando o auxiliar é executado:

[!code-html[Main](mvc3-release-notes/samples/sample41.html)]

## <a id="0.1__Toc274034228"></a>Correções de bugs

O modelo de objeto padrão para os auxiliares de modelo EditorFor e DisplayFor agora oferece suporte à ordenação especificada na Propriedade DisplayAttribute. Order. (Em versões anteriores, a configuração do pedido não foi usada.)

A validação do cliente agora dá suporte à validação de Propriedades substituídas que têm atributos de validação aplicados.

O JsonValueProviderFactory agora está registrado por padrão.

## <a id="0.1__Toc274034229"></a>Alterações recentes

A ordem de execução dos filtros de exceção foi alterada para filtros de exceção que têm o mesmo valor de ordem. No ASP.NET MVC 2 e versões anteriores, os filtros de exceção no controlador com a mesma ordem que aqueles em um método de ação foram executados antes dos filtros de exceção no método de ação. Normalmente, esse seria o caso quando filtros de exceção foram aplicados sem um valor de ordem especificado. No ASP.NET MVC 3, esse pedido foi revertido para que o manipulador de exceção mais específico seja executado primeiro. Como nas versões anteriores, se a propriedade Order for explicitamente especificada, os filtros serão executados na ordem especificada.

## <a id="0.1__Toc274034230"></a>Problemas conhecidos

Durante a instalação, a caixa de diálogo aceitação do EULA exibe os termos de licença em uma janela que é menor do que o esperado.

Os modos de exibição do Razor não têm suporte IntelliSense nem realce de sintaxe. É esperado que o suporte para sintaxe Razor no Visual Studio seja incluído como parte de uma versão posterior.

Quando você estiver editando um modo de exibição Razor (arquivo cshtml <a id="0.1__Toc224729061"></a> <a id="0.1__Toc238051347"></a> ), o item de menu ir para controlador no Visual Studio não estará disponível e não haverá trechos de código.

Ao usar a sintaxe de @model para especificar uma exibição de CSHTML com rigidez de tipos, os atalhos específicos a um idioma não são reconhecidos. Por exemplo, @model int não funcionará, mas @model Int32 funcionará. A solução alternativa para esse bug é usar o nome do tipo real ao especificar o tipo de modelo.

Ao usar a sintaxe de @model para especificar uma exibição do tipo CSHTML com rigidez de tipos (ou @ModelType para especificar uma exibição VBHTML com rigidez de tipo), não há suporte para tipo anulável e declarações de matriz. Por exemplo, @model int? Não tem suporte. Em vez disso, use `@model Nullable<Int32>`. Também não há suporte para a sintaxe @model String []; em vez disso, use `@model IList<string>`.

Ao atualizar um projeto do ASP.NET MVC 2 para o ASP.NET MVC 3, certifique-se de adicionar o seguinte à seção appSettings do arquivo Web. config:

[!code-xml[Main](mvc3-release-notes/samples/sample42.xml)]

Há um problema conhecido que faz com que a autenticação de formulários sempre redirecione usuários não autenticados para ~/Account/Login, ignorando a configuração de autenticação de formulários usada em Web. config. A solução alternativa é adicionar a configuração de aplicativo a seguir.

[!code-xml[Main](mvc3-release-notes/samples/sample43.xml)]

## <a id="0.1__Toc274034231"></a>Enção

© 2011 Microsoft Corporation. Todos os direitos reservados. Este documento é fornecido "no estado em que se encontra". As informações e as exibições expressas neste documento, incluindo URLs e outras referências a sites da Internet, podem ser alteradas sem aviso prévio. Você assume o risco de utilizá-las.

Este documento não lhe concede nenhum direito legal à nenhuma propriedade intelectual de nenhum produto da Microsoft. Você pode copiar e usar este documento para fins internos de referência.
