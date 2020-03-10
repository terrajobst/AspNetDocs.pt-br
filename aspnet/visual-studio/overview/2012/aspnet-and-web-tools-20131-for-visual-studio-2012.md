---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: Notas de versão do ASP.NET and Web Tools 2013,1 para Visual Studio 2012 | Microsoft Docs
author: microsoft
description: Este documento descreve a versão do ASP.NET and Web Tools 2013,1 para Visual Studio 2012.
ms.author: riande
ms.date: 11/13/2013
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 260af1018064d60d80cbb1002001f28c4daffffd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578438"
---
# <a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>Notas de versão do ASP.NET and Web Tools 2013.1 para Visual Studio 2012

pela [Microsoft](https://github.com/microsoft)

> Este documento descreve a versão do ASP.NET and Web Tools 2013,1 para Visual Studio 2012.

## <a name="contents"></a>Conteúdo

- [Notas de instalação](#install)
- [Requisitos de software](#requirements)
- Novos recursos no ASP.NET and Web Tools 2013,1 para Visual Studio 2012

    - [Bootstrap](#bootstrap)
    - [Modelos](#templates)

        - [Modelo MVC 5 do ASP.NET](#mvc5template)
        - [Modelo de ASP.NET Web API 2](#apitemplate)
        - [Modelos de item](#itemtemplate)
    - [Entity Framework 6](#ef6)
    - [ASP.NET scaffolding](#scaffold)
    - [Editor Razor](#razor)
    - [NuGet 2.7](#nuget)
- Problemas conhecidos e alterações recentes

    - [ASP.NET scaffolding](#issuescaffolding)

        - [MVC e API Web scaffolding-HTTP 404, erro não encontrado](#404issue)
        - [Visual Studio Express 2012 para Web parar de funcionar depois de adicionar um item com Scaffold](#expressissue)
    - [ASP.NET Razor 3](#issuerazor)

        - [A exibição do arquivo cshtml com browse com ou F5 causa um erro de servidor](#browseissue)
        - [Regravação de URL e til (~)](#rewriteissue)
    - [Modelos](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a>Notas de instalação

[Instalar](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) o ASP.NET and Web Tools 2013,1 para Visual Studio 2012.

<a id="requirements"></a>
## <a name="software-requirements"></a>Requisitos de software

Você deve ter o Visual Studio 2012 ou o Visual Studio Express 2012 para Web.

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>Novos recursos no ASP.NET and Web Tools 2013,1 para Visual Studio 2012

<a id="bootstrap"></a>
### <a name="bootstrap"></a>Inicialização

Quando você Scaffold os controladores e exibições MVC 5, a marcação para as exibições usa a [inicialização](http://getbootstrap.com/).

<a id="templates"></a>
### <a name="templates"></a>Modelos

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a>Modelo MVC 5 do ASP.NET

Adicionamos um novo modelo MVC 5. Ele faz referência aos últimos pacotes NuGet do MVC 5 e você pode usar scaffolding para adicionar controladores e exibições.

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a>Modelo de ASP.NET Web API 2

Adicionamos um novo modelo de API Web 2. Ele faz referência aos pacotes NuGet mais recentes da API Web 2, e você pode usar scaffolding para adicionar controladores e exibições.

<a id="itemtemplate"></a>
#### <a name="item-templates"></a>Modelos de item

Adicionamos novos modelos de item para exibições MVC 5, páginas da Web (Razor 3) e controladores da API Web 2. Eles instalam os pacotes do NuGet relacionados ao projeto ao adicionar novos itens.

<a id="ef6"></a>
### <a name="entity-framework-6"></a>Entity Framework 6

Quando você Scaffold um MVC ou um controlador de API Web usando Entity Framework, usamos o Framework 6. Para obter mais informações sobre Entity Framework, consulte o [histórico de versão do Entity Framework](https://msdn.com/data/jj574253).

Você também pode baixar e instalar as ferramentas do Entity Framework 6 para Visual Studio 2012. Consulte [obter Entity Framework](https://msdn.com/data/ee712906#tooling).

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET scaffolding

ASP.NET scaffolding é uma estrutura de geração de código para aplicativos ASP.NET Web. Ele facilita a adição de código clichê ao seu projeto que interage com um modelo de dados.

Nas versões anteriores do Visual Studio, o scaffolding era limitado a projetos do ASP.NET MVC. Com essa atualização, agora você pode usar scaffolding para qualquer projeto ASP.NET, incluindo Web Forms. Essa atualização não oferece suporte à geração de páginas para um projeto Web Forms, mas você ainda pode usar scaffolding com Web Forms adicionando dependências MVC ao projeto. O suporte para gerar páginas para Web Forms será adicionado em uma atualização futura.

Ao usar o scaffolding, garantimos que todas as dependências necessárias sejam instaladas no projeto. Por exemplo, se você começar com um projeto ASP.NET Web Forms e, em seguida, usar scaffolding para adicionar um controlador de API Web, os pacotes e as referências do NuGet necessários serão adicionados automaticamente ao seu projeto.

Para adicionar o MVC scaffolding a um projeto Web Forms, adicione um **novo item com Scaffold** e selecione **dependências do MVC 5** na janela de diálogo. Há duas opções para o scaffolding MVC; Mínimo e completo. Se você selecionar mínimo, somente os pacotes e as referências do NuGet para ASP.NET MVC serão adicionados ao seu projeto. Se você selecionar a opção completa, as dependências mínimas serão adicionadas, bem como os arquivos de conteúdo necessários para um projeto MVC.

O suporte para controladores assíncronos scaffolding usa os novos recursos assíncronos do Entity Framework 6.

Para obter mais informações e tutoriais, consulte [visão geral do ASP.net scaffolding](../2013/aspnet-scaffolding-overview.md). Esses tutoriais mostram scaffolding com Visual Studio 2013, mas também são aplicáveis a ASP.NET and Web Tools 2013,1 para Visual Studio 2012.

<a id="razor"></a>
### <a name="razor-editor"></a>Editor Razor

Com essa atualização, o Visual Studio 2012 agora oferece suporte a ferramentas/edição do Razor 3.

<a id="nuget"></a>
### <a name="nuget-27"></a>NuGet 2.7

O NuGet 2,7 inclui um conjunto avançado de novos recursos que são descritos em detalhes nas [notas de versão do NuGet 2,7](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Esta versão do NuGet elimina a necessidade de os usuários permitirem explicitamente que o NuGet restaure pacotes ausentes. Ao instalar o NuGet 2,7, os usuários concordam implicitamente em restaurar automaticamente os pacotes ausentes. Os usuários podem explicitamente recusar a restauração do pacote por meio das configurações do NuGet no Visual Studio. Essa alteração simplifica a forma como a restauração de pacotes funciona.

## <a name="known-issues-and-breaking-changes"></a>Problemas conhecidos e alterações recentes

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET scaffolding

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC e API Web scaffolding-HTTP 404, erro não encontrado

Se você encontrar um erro ao adicionar um item com Scaffold a um projeto, é possível que seu projeto seja deixado em um estado inconsistente. Algumas das alterações feitas no scaffolding serão revertidas, mas outras alterações, como os pacotes NuGet instalados, não serão revertidas. Se as alterações de configuração de roteamento forem revertidas, os usuários receberão um erro HTTP 404 ao navegar para itens com Scaffold.

Para corrigir esse erro para o MVC, adicione um novo item com Scaffold e selecione dependências MVC 5 (mínima ou completa). Esse processo adicionará todas as alterações necessárias ao seu projeto.

Para corrigir esse erro para a API Web:

1. Adicione a seguinte classe WebApiConfig ao seu projeto.

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. Configure WebApiConfig. Register no aplicativo\_o método Start no global. asax da seguinte maneira:

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a>Visual Studio Express 2012 para Web parar de funcionar depois de adicionar um item com Scaffold

Se Visual Studio Express 2012 para a Web parar de funcionar depois de adicionar o item com Scaffold com Entity Framework (como o controlador Web API 2 com ações, usando Entity Framework), é possível que Visual Studio Express falha ao carregar a imagem nativa de um assembly dependente de System. Web. Extensions.

Para corrigir esse problema, configure Visual Studio Express para trabalhar com a imagem MSIL de System. Web. Extensions:

1. Abra o prompt de comando no modo de administrador.
2. Vá para%ProgramFiles%\Microsoft Visual Studio 11.0 \ Common7\IDE ou% ProgramFiles (x86)% \ Microsoft Visual Studio 11.0 \ Common7\IDE (para o Windows de 64 bits).
3. Abra VWDExpress. exe. config em um editor de texto.
4. Adicione a seguinte linha sob a &lt;configuração&gt;/elemento&gt; de tempo de execução &lt;:  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. Reinicie o Visual Studio Express 2012 para Web.

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a>ASP.NET Razor 3

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-with-browse-with-or-f5-causes-a-server-error"></a>A exibição do arquivo cshtml com browse com ou F5 causa um erro de servidor

Ao criar um projeto MVC 5 no Visual Studio 2012 (ou abrir no Visual Studio 2012 um projeto MVC 5 criado no Visual Studio 2013) e tentar exibir um arquivo cshtml usando procurar ou F5, você receberá um erro informando que o **erro do servidor no aplicativo '/'** é exibido. O servidor tenta navegar para `http://localhost:XXXX/Views/../XXXX.cshtml`

Para resolver esse problema, altere a configuração **ação de início** em seu projeto para **página específica**. Você não precisa fornecer um valor para a página.

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

Depois de fazer essa alteração, selecionar F5 navega para a raiz do seu aplicativo (`http://localhost:XXXX`). Esse comportamento não é o mesmo que o comportamento para projetos MVC 5 no Visual Studio 2013, em que a configuração da **página atual** inicia a página aberta.

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a>Regravação de URL e til (~)

Após a atualização para ASP.NET Razor 3 ou ASP.NET MVC 5, a notação til (~) poderá não funcionar corretamente se você estiver usando regravações de URL. A reescrita de URL afeta a notação de til (~) em elementos HTML, como &lt;A/&gt;, &lt;SCRIPT/&gt;, &lt;LINK/&gt;e, como resultado, o til não é mais mapeado para o diretório raiz.

Por exemplo, se você reescrever solicitações de **ASP.net/content** para **ASP.net**, o atributo href em &lt;a href = "~/content/"/&gt; será resolvido para **/content/content/** em vez de **/** . Para suprimir essa alteração, você pode definir o contexto de **WasUrlRewritten do IIS\_** como false em cada página da Web ou no **aplicativo\_BeginRequest** em global. asax.

<a id="templateissue"></a>
### <a name="templates"></a>Modelos

Quando você cria projetos do ASP.NET MVC com o Visual Studio 2012 no Windows 8.1 ou no Windows Server 2012 R2, o Visual Studio exibe uma mensagem de erro informando "Configurando Web [url] para ASP.NET 4,5 falhou".

![erro de configuração](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

Você verá esse erro porque o Visual Studio 2012 não habilita o recurso ASP.NET 4,5 quando ele está instalado nessas versões do Windows. Para habilitar o ASP.NET 4,5, execute as etapas descritas em [Ativar ou desativar recursos do Windows](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).

![ativar ou desativar os recursos do Windows](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

Como alternativa, você pode habilitar o ASP.NET 4,5 por meio da linha de comando.

1. Abra o prompt de comando no modo de administrador.
2. Execute o comando a seguir para habilitar o ASP.NET 4,5.  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
