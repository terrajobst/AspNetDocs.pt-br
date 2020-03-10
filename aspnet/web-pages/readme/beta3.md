---
uid: web-pages/readme/beta3
title: Leiame da versão beta 3 do Web Matrix e do Páginas da Web do ASP.NET (Razor) | Microsoft Docs
author: rick-anderson
description: Leiame do Web Matrix e de Páginas da Web do ASP.NET (Razor) versão beta 3
ms.author: riande
ms.date: 01/10/2011
ms.assetid: ffa3d5c9-91e5-4da3-b409-560b0c7fbbf0
msc.legacyurl: /web-pages/readme/beta3
msc.type: content
ms.openlocfilehash: dc1d9237c04a7fcdbf4db6ccc8c36d255f6de003
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628894"
---
# <a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a>Leiame do Web Matrix e de Páginas da Web do ASP.NET (Razor) versão beta 3

> Leiame do Web Matrix e de Páginas da Web do ASP.NET (Razor) versão beta 3

9 de novembro de 2010

## <a name="contents"></a>Conteúdo

- [Visão geral](#Overview)
- [Instalação](#Installation_Notes)
- [Novos recursos, alterações e problemas conhecidos na versão beta 3](#Known_Issues)

    - [Problemas de instalação do WebMatrix](#Known_Issues_Installation)
    - [Páginas da Web do ASP.NET](#Known_Issues_ASPNET)
    - [SQL Server Compact](#Known_Issues_SQL_Server_Compact)
    - [Instalando aplicativos](#Known_Issues_Installing_Applications)
    - [Publicando aplicativos](#Known_Issues_Publishing_Applications)
    - [Outros problemas](#Known_Issues_Other_Issues)
- [Para obter mais informações](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>Visão geral

> O Microsoft WebMatrix beta é uma pilha de desenvolvimento na Web gratuita que é instalada em minutos. Ele integra um servidor Web com estruturas de banco de dados e programação para criar uma experiência única e integrada. Você pode usar o WebMatrix beta para simplificar a maneira como você codifica, testa e publica seu próprio site ASP.NET ou PHP ou pode usar o WebMatrix beta para iniciar um novo site usando aplicativos de software livre populares, como o DotNetNuke, o Umbraco, o WordPress ou o Joomla. O WebMatrix beta usa o mesmo ambiente avançado de servidor Web, mecanismo de banco de dados e estruturas que executará seu site na Internet, o que torna a transição do desenvolvimento para a produção tranqüila e contínua.

<a id="Installation_Notes"></a>

## <a name="installation"></a>Instalação

> Para instalar o WebMatrix Beta 3, você usa [Microsoft Web Platform Installer 3,0](https://go.microsoft.com/fwlink/?LinkID=194638). Depois de instalar o Web Platform Installer, você poderá usá-lo para instalar o WebMatrix Beta 3.
> 
> Se você tiver problemas durante a instalação, consulte [Solucionando problemas com Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).

<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a>Instruções para publicação de aplicativos

> Consulte [as instruções passo a passo para publicar aplicativos](https://go.microsoft.com/fwlink/?LinkID=196149)

<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a>Novos recursos, alterações, problemas de andKnown

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a>Instalação do WebMatrix Beta 3

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a>Problema: o WebMatrix Beta 3 só está disponível em plataformas que dão suporte ao Microsoft .NET Framework 4

> O .NET Framework versão 4 é necessário para o WebMatrix beta. Em determinados casos, o instalador do WebMatrix beta permitirá que você tente instalar o em uma plataforma que não faça parte do conjunto de configuração com suporte. Em particular, o Windows Vista sem a atualização do SP1 permitirá que você comece a instalação do WebMatrix beta, mas o componente .NET Framework 4 falhará e bloqueará sua instalação.
> 
> **Solução alternativa**  
> Instale o em uma plataforma com suporte, que inclui:
> 
> - Windows 7
> - Windows Server 2008
> - Windows Server 2008 R2
> - Windows Vista SP1 ou posterior
> - Windows XP SP3
> - Windows Server 2003 SP2

#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Problema: não é possível instalar o WebMatrix Beta 3 se o Microsoft Visual Studio 2008 for instalado sem Microsoft Visual Studio 2008 SP1

> **Solução alternativa**  
> Instale o [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) no centro de download da Microsoft.

#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a>Problema: alguns assemblies para o SQL Server Compact 4,0 não estão instalados no GAC

> Os assemblies gerenciados para SQL Server Compact 4,0 não são colocados no GAC (cache de assembly global) quando você instala o SQL Server Compact 4,0 em um computador de 64 bits e o computador tem apenas o perfil de cliente do .NET Framework 3,5 SP1 instalado. Os assemblies gerenciados que não estão instalados no GAC são:
> 
> - *System. Data. SqlServerCe. dll* (provedor ADO.net)
> - *System. Data. SqlServerCe. Entity. dll* (ADO.NET Entity Framework)
> 
> **Solução alternativa**  
> Desinstale o SQL Server Compact 4,0. Baixe e instale a versão completa do .NET Framework 3,5 SP1 no seguinte local:  
>   
> [Microsoft .NET Framework 3,5 Service Pack 1 (pacote completo)](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> Em seguida, reinstale o SQL Server Compact 4,0.

#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a>Problema: não é possível desinstalar SQL Server Compact usando a linha de comando

> A desinstalação do SQL Server Compact usando opções de linha de comando não funciona nesta versão.
> 
> **Solução alternativa**  
> Use *programas e recursos* no painel de controle do Windows para desinstalar o Microsoft SQL Server Compact 4,0.

<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a>Páginas da Web do ASP.NET

Esta seção do documento descreve novos recursos, alterações e problemas conhecidos com a versão beta 3 do Páginas da Web do ASP.NET com sintaxe Razor.

- [Novos recursos](#NewFeatures)
- [Alterações](#Changes)
- [Problemas](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Novos recursos na versão beta 3 para Páginas da Web do ASP.NET com a sintaxe do Razor

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a>Novo: o método "HTML. Raw" renderiza a marcação não codificada

> O novo método `Html.Raw` permite que você processe a marcação HTML como marcação em vez de renderizar a saída codificada. (Por padrão, o ASP.NET Razor codifica cadeias de caracteres antes de renderizá-las.) A sintaxe é:
> 
> `Html.Raw(value)`
> 
> O exemplo a seguir mostra como usar `Html.Raw`:
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]

<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Alterações na versão beta 3 para Páginas da Web do ASP.NET com a sintaxe do Razor

#### <a name="change-hrefattribute-method-removed"></a>Alteração: método "Hrefattribute" removido

> O método `HrefAttribute` da classe `WebPage` foi removido. Esse auxiliar foi usado para codificar caracteres não seguros em URLs. Ele não é mais necessário porque o ASP.NET Razor codifica automaticamente as cadeias de caracteres. (Use o novo método `Html.Raw` para renderizar cadeias de caracteres não codificadas.)

#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a>Alteração: sintaxe para auxiliares "@helper" declarativos alterada

> Na versão beta 3, o ASP.NET altera como ele analisa auxiliares criados usando a sintaxe `@helper`. Em essência, a sintaxe de `@helper` agora é analisada como um bloco de código em vez de como um bloco de marcação que pode incluir código. Portanto, o código dentro do auxiliar não precisa ser colocado em blocos de `@{ }`. Por outro lado, a marcação dentro do auxiliar deve ser incluída explicitamente em elementos HTML ou em marcas de `<text></text>` Razor do ASP.NET.
> 
> Por exemplo, a sintaxe de `@helper` a seguir funciona na versão beta 3:
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> Na versão beta 3, esse auxiliar deve ser alterado para ser semelhante ao exemplo a seguir:
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> Observe que o `@{ }` caracteres em volta do código inicial no auxiliar não é mais usado. Isso ocorre porque o conteúdo dos auxiliares é tratado como um bloco de código por padrão. O auxiliar renderiza a marcação, que começa com a marca de `<a>` de abertura. Se o auxiliar precisar renderizar texto sem formatação ou marcas que não incluam uma marca de fechamento (por exemplo, marcas de `<meta>`), o conteúdo a ser renderizado deverá estar em marcas `<text></text>`.

#### <a name="change-webpagecontexthttpcontext-removed"></a>Alteração: "WebPageContext. HttpContext" removido

> A propriedade `WebPageContext.HttpContext` foi removida. Use `HttpContext.Current` em vez disso. (A propriedade `WebPageContext.HttpContext` simplesmente encapsulava isso.)

#### <a name="change-facebook-helper-moved-to-new-package"></a>Alteração: o auxiliar "Facebook" foi movido para o novo pacote

> O auxiliar de `Facebook` foi movido para a biblioteca do *Facebook. Helper* , que inclui o auxiliar de `Facebook` e a funcionalidade adicional. Você deve instalar essa biblioteca como um pacote separado, conforme descrito em "Instalando auxiliares com o Gerenciador de pacotes" no tutorial [introdução com páginas ASP.net](https://go.microsoft.com/fwlink/?LinkId=202889).

#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a>Alteração: Associação, função e tipos de segurança são movidos para o novo assembly

> Os seguintes tipos foram movidos para o assembly `WebMatrix.WebData`:
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`

#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a>Alteração: a classe "TagBuilder" foi movida para o assembly System. Web. webpages. dll

> A classe `TagBuilde` r foi movida para o assembly System. Web. webpages. dll. Anteriormente, isso estava em um assembly que era parte do ASP.NET MVC. Essa alteração significa que você não precisa instalar o ASP.NET MVC para usar a classe `TagBuilder`.
> 
> No entanto, a classe ainda está no namespace `System.Web.Mvc`. Para usar a classe `TagBuilder` (por exemplo, em um auxiliar personalizado do Razor do ASP.NET), você deve referenciar o namespace (por exemplo, adicionando `@using System.Web.Mvc` ao seu código).

#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a>Alteração: sintaxe de validação de solicitação alterada; Classe de "validação" removida

> Na versão beta 3, para desabilitar a validação de um campo individual ou de um conjunto de campos, você pode chamar o método `Validation.Exclude`, passando o nome ou os nomes dos campos a serem excluídos da validação. Uma nova sintaxe está disponível na versão beta 3 para ignorar a validação. O método `Validation` usado na versão beta 3 foi removido.
> 
> > [!NOTE]
> > Se você não desabilitar a validação de solicitação, se os usuários tentarem carregar a marcação HTML (por exemplo, usando um editor de Rich Text em uma página), o site relatará um erro como *um valor de solicitação potencialmente perigoso. formulário foi detectado no cliente* e a entrada do usuário não será aceita. Se você desabilitar a validação de solicitação, deverá verificar manualmente a entrada do usuário para certificar-se de que ela não contém marcação potencialmente perigosa ou script usando algo como a [biblioteca de scripts do Microsoft anti-cross site v 4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).
> 
> 
> Para desabilitar a validação automática de solicitação, chame o método `Request.Unvalidated`, passando o nome do campo ou outro objeto post para o qual você deseja ignorar a validação de solicitação. Você pode usar esse método para ignorar a validação de todos os itens nas coleções `Form`, `QueryString`, `Cookies`e `ServerVariables`. Os exemplos a seguir mostram como usar o método `Unvalidated`:
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]

<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a>Problemas conhecidos para Páginas da Web do ASP.NET com a sintaxe do Razor

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>Problema: comportamento inesperado ao usar uma tabela de usuário personalizada para associação

> Para inicializar o provedor de associação para um site do Razor do ASP.NET, chame o método `WebSecurity.InitializeDatabaseConnection`. (No WebMatrix, o modelo de site inicial inclui uma chamada para esse método no arquivo *\_AppStart. cshtml* ). Se o parâmetro `autoCreateTables` desse método for definido como true (por padrão, ele será definido como true no modelo de site inicial) e, se um nome de tabela não reconhecido for passado para o método (o segundo parâmetro), o método não gerará um erro. Em vez disso, ele cria automaticamente a tabela.
> 
> Isso pode ser um problema se você pretender usar uma tabela de usuário personalizada para associação, mas passar o nome de tabela incorreto para o método `WebSecurity.InitializeDatabaseConnection`. Como o método não gera, por padrão, um erro se a tabela especificada não existir e, em vez disso, criar uma nova tabela, o aplicativo poderá parecer estar funcionando. No entanto, o código do aplicativo que depende da sua tabela de usuário personalizada (e em campos) pode eventualmente falhar com erros inesperados.
> 
> **Solução alternativa**  
> Verifique se o nome passado no método `InitializeDatabaseConnection` corresponde à tabela de perfil de usuário no banco de dados de associação ou certifique-se de que o parâmetro `autoCreateTables` está definido como false.

#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>Problema: erro "falha ao gerar uma instância de usuário do SQL Server"

> Se um aplicativo Web do WebMatrix usar SQL Server Express e estiver executando o IIS 7,5 no Windows 7 ou no Windows Server 2008 R2, você poderá ver um erro que indica que SQL Server não é possível recuperar o caminho do aplicativo local do usuário em tempo de execução.
> 
> **Solução alternativa** Verifique se a conta do Windows em que o aplicativo é executado (normalmente serviço de rede) tem permissões de leitura/gravação para pastas raiz do aplicativo e para subpastas como *dados de\_de aplicativo*. Informações mais detalhadas estão disponíveis nos problemas do artigo da base de conhecimento [com SQL Server Express instanciação de usuário e projetos de aplicativo Web ASP.net](https://support.microsoft.com/kb/2002980).

#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a>Problema: no Visual Studio, os namespaces para assemblies personalizados (DLLs) não são importados automaticamente

> Se você usar assemblies personalizados em um projeto no Visual Studio, os namespaces declarados nesses assemblies não serão importados automaticamente em tempo de design. Como resultado, as referências a tipos personalizados podem não ser reconhecidas em tempo de design e são marcadas como não reconhecidas no Visual Studio (usando um "rabisco"). Esse problema ocorre apenas em tempo de design no Visual Studio; o aplicativo em si é executado corretamente.
> 
> **Solução alternativa**  
> Inclua uma instrução `using` (`imports` em Visual Basic) que faça referência às entidades que não são reconhecidas no tempo de design.

#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>Problema: Visual Studio IntelliSense e modelos de projeto disponíveis somente no ASP.NET MVC versão 3

> A instalação do Páginas da Web do ASP.NET não também instala ferramentas para o Visual Studio, como IntelliSense e modelos de projeto para Páginas da Web do ASP.NET aplicativos.
> 
> **Solução alternativa** Para usar o IntelliSense e modelos de projeto para Páginas da Web do ASP.NET aplicativos no Visual Studio, instale o ASP.NET MVC 3 RC por meio do Web Platform Installer ou do [instalador](https://go.microsoft.com/fwlink/?LinkID=191797)autônomo.

#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a>Problema: erro "&lt;auxiliar&gt; classe não pode ser encontrado"

> Depois de atualizar para a versão beta 3, você poderá ver um erro de que uma classe auxiliar (por exemplo, a classe `Facebook`) não pode ser encontrada. A partir da versão beta 2 e continuando na versão beta 3, os auxiliares foram movidos para os pacotes que você deve instalar explicitamente. Os sites existentes não são atualizados para incluir esses pacotes; Isso inclui sites nas pastas da Web de *\Meus Documents\IISExpress* ou *\Meus documentos\Minhas sites* . Em particular, você verá esse erro se usar o site padrão em *meus sites* (WebSite1), que inclui uma referência ao auxiliar de `Twitter`.
> 
> **Solução alternativa**  
> Comente as chamadas para os auxiliares no site, execute a página *\_admin* e instale o pacote ou os pacotes que incluem os auxiliares que você deseja usar. Depois de instalar o pacote, você pode remover os comentários das linhas que fazem referência aos auxiliares.

#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a>Problema: a implantação de assemblies do Razor do beta 3 ASP.NET na pasta bin pode não funcionar em sites de hospedagem

> Se você implantar um site Páginas da Web do ASP.NET em um site de hospedagem e, se implantar os assemblies do ASP.NET Razor Beta 3 na pasta *bin* do site, poderá encontrar erros, incluindo o seguinte:
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> Isso pode acontecer se o provedor de hospedagem tiver instalado os assemblies Páginas da Web do ASP.NET beta 1 no cache de aplicativo global (GAC) do servidor. Os assemblies no GAC obtêm precedência sobre assemblies instalados localmente na pasta *bin* .
> 
> **Solução alternativa** Entre em contato com seu provedor de hospedagem para confirmar se os erros que você está vendo estão devido a um conflito entre as versões do provedor dos assemblies e seus. Nesse caso, solicite que o provedor de hospedagem atualize os assemblies no GAC do servidor.

#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>Problema: lendo feeds ou outros dados externos por meio de um servidor proxy

> Se o servidor que está executando o site estiver protegido por um servidor proxy, talvez seja necessário configurar as informações de proxy no arquivo *Web. config* para poder ler as informações provenientes de fora do site. Por exemplo, se você usar o auxiliar de `ReCaptcha`, o auxiliar se comunicará com o serviço reCAPTCHA, mas poderá ser bloqueado pelo servidor proxy. Da mesma forma, os feeds usados no Páginas da Web do ASP.NET, como o feed usado pelo Gerenciador de pacotes, podem exigir a configuração de proxy.
> 
> Se você tiver problemas ao trabalhar com um serviço externo ou trabalhar com o feed de pacote, coloque os seguintes elementos no arquivo *Web. config* da raiz do aplicativo:
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> Para obter mais informações sobre como configurar um servidor proxy, consulte [&lt;elemento&gt; proxy (configurações de rede)](https://msdn.microsoft.com/library/sa91de1e.aspx) no site do MSDN.

#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a>Problema: erro "Microsoft. Web. Infrastructure. dll não pode ser carregado"

> Se você tiver instalado anteriormente a versão beta 1 do Páginas da Web do ASP.NET com sintaxe Razor e, em seguida, instalar a versão beta 3, todos os assemblies apropriados serão instalados no GAC, exceto *Microsoft. Web. Infrastructure. dll*. Como consequência, ao executar ASP.NET páginas Razor, você verá um erro que indica que *Microsoft. Web. Infrastructure. dll* não pôde ser carregado.
> 
> Esse problema não ocorrerá se você carregou a versão beta 3 em um computador limpo.
> 
> **Solução alternativa**  
> No painel de controle, desinstale o Páginas da Web do ASP.NET. Em seguida, reinstale a versão beta 3.

#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>Problema: a desinstalação do .NET Framework versão 4 desabilita Páginas da Web do ASP.NET com a sintaxe do Razor

> Se você desinstalar o .NET Framework versão 4 e, em seguida, reinstalá-lo, Páginas da Web do ASP.NET com sintaxe Razor será desabilitado. As páginas com a extensão *. cshtml* não são executadas corretamente. Páginas da Web do ASP.NET registra um assembly no arquivo *Web. config da* raiz do computador e a remoção do .NET Framework remove esse arquivo. Reinstalar o .NET Framework instala uma nova versão do arquivo de configuração, mas não adiciona a referência para o assembly Páginas da Web do ASP.NET.
> 
> **Solução alternativa** Depois de reinstalar o .NET Framework, reinstale o Páginas da Web do ASP.NET com sintaxe Razor. Isso adiciona o seguinte elemento ao arquivo *Web. config* na raiz do computador, que normalmente está no seguinte local:  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> 
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]

#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a>Problema: os aplicativos implantados anteriormente com assemblies ASP.NET na pasta bin apresentam erros

> Durante a implantação, as cópias dos assemblies de Páginas da Web do ASP.NET (por exemplo, *Microsoft. webpages. dll*) para a pasta *bin* do site no servidor. (Isso pode ter ocorrido automaticamente durante a implantação ou porque o desenvolvedor copiou explicitamente os assemblies.) No entanto, quando a versão beta 3 estiver instalada, ocorrerão erros, como erros que determinados tipos não podem ser encontrados. Isso ocorre porque vários tipos de Páginas da Web do ASP.NET foram movidos para namespaces diferentes para a versão beta 3.
> 
> **Solução alternativa**   
> Limpe a pasta *bin* do aplicativo implantado, copie os novos assemblies para a pasta (ou reimplante o aplicativo) e reinicie o aplicativo.

#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a>Problema: URLs sem extensão não localizam arquivos. cshtml/. vbhtml no IIS 7 ou IIS 7,5

> No IIS 7 ou IIS 7,5, as solicitações com uma URL como esta não são capazes de localizar páginas que tenham a extensão *. cshtml* ou *. vbhtml* :  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> O problema ocorre porque a regravação de URL não está habilitada por padrão para o IIS 7 ou o IIS 7,5. O cenário likeliest é que você não vê o problema ao testar localmente usando IIS Express, mas você o experimenta quando implanta seu site em um site de hospedagem.
> 
> **Solução alternativa**
> 
> - Se você tiver controle sobre o computador servidor, no computador servidor, instale a atualização descrita em [uma atualização disponível que permite que determinados manipuladores do iis 7,0 ou iis 7,5 manipulem solicitações cujas URLs não terminam com um ponto](https://support.microsoft.com/kb/980368).
> - Se você não tiver controle sobre o computador servidor (por exemplo, você está implantando em um site de hospedagem), adicione o seguinte ao arquivo *Web. config* do site:
> 
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]

#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a>Problema: usando o projeto de aplicativo Web ou o ASP.NET MVC e as páginas da Web do ASP.NET no mesmo aplicativo

> Se você estiver usando Páginas da Web do ASP.NET em um projeto de aplicativo Web ou em um aplicativo ASP.NET MVC, poderá ver um erro de que *WebPageHttpApplication* não pode ser encontrado.
> 
> **Solução alternativa**  
> Se você receber esse erro, altere a classe base da qual o aplicativo deriva. No arquivo *global. asax* , altere a seguinte linha:
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> Para isso:
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> Isso, em vigor, reverte uma alteração que foi introduzida para a versão beta 1 do Páginas da Web do ASP.NET com sintaxe Razor.

#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>Problema: Implantando um aplicativo em um computador que não tem SQL Server Compact instalado

> Os aplicativos que incluem bancos de dados do SQL Server Compact podem ser executados em um computador onde o SQL Server Compact não está instalado. O Microsoft WebMatrix Beta 3 copia automaticamente esses binários para você e executa as transformações de arquivo *Web. config* apropriadas.
> 
> **Solução alternativa** Se você precisar copiar esses arquivos e fazer com que o arquivo *Web. config* seja alterado manualmente, faça o seguinte:
> 
> 1. Copie os assemblies do mecanismo de banco de dados para a pasta *bin* (e subpastas) do aplicativo no computador de destino: 
> 
>     - Copiar *c:\Arquivos de programas\microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **para** *\Bin*
>     - Copie *c:\Arquivos de programas\microsoft SQL Server Compact Edition\v4.0\Private\x86\\* * **para** *\Bin\x86*
>     - Copie *c:\Arquivos de programas\microsoft SQL Server Compact Edition\v4.0\Private\amd64\\* * **para** *\Bin\amd64*
> 2. Na pasta raiz do site, crie ou abra um arquivo *Web. config* . (No WebMatrix Beta 3, esse tipo de arquivo estará disponível se você clicar em **tudo** na caixa de diálogo **escolher um tipo de arquivo** .)
> 3. Adicione o seguinte elemento como um filho do elemento **&lt;configuration&gt;** (não dentro do elemento **&lt;system. Web&gt;** ):
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]

#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>Problema: os auxiliares de banco de dados e WebGrid não funcionam em confiança média no Visual Basic

> Se você estiver usando Visual Basic (criando arquivos *. vbhtml* ), os auxiliares `Database` e `WebGrid` não funcionarão se o aplicativo for definido para usar a confiança média.
> 
> **Solução alternativa**  
> Defina temporariamente o aplicativo para usar confiança total.

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a>SQL Server Compact

#### <a name="issue-encrypt-property-is-not-recognized"></a>Problema: a propriedade "Encrypt" não é reconhecida

> SQL Server Compact 4,0 não reconhece a propriedade `Encrypt` da classe `SqlCeConnection`. Você não deve usar essa propriedade para criptografar arquivos de banco de dados. A propriedade `Encrypt` foi preterida na versão SQL Server Compact 3,5 e foi retida apenas para compatibilidade com versões anteriores. 
> 
> **Solução alternativa**  
> Use a propriedade `Encryption Mode` da classe `SqlCeConnection` para criptografar arquivos de banco de dados SQL Server Compact 4,0. O exemplo a seguir mostra como criar um banco de dados criptografado SQL Server Compact 4,0 usando a propriedade `Encryption Mode`:
> 
> [!code-csharp[Main](beta3/samples/sample11.cs)]
> 
> [!code-vb[Main](beta3/samples/sample12.vb)]
> 
> Para alterar o modo de criptografia de um banco de dados SQL Server Compact 4,0 existente, faça o seguinte:
> 
> [!code-csharp[Main](beta3/samples/sample13.cs)]
> 
> [!code-vb[Main](beta3/samples/sample14.vb)]
> 
> Para criptografar um banco de dados do SQL Server Compact 4,0 não criptografado, faça o seguinte:
> 
> [!code-csharp[Main](beta3/samples/sample15.cs)]
> 
> [!code-vb[Main](beta3/samples/sample16.vb)]

#### <a name="issue-microsoft-visual-c-2008-runtime-libraries-are-required"></a>Problema: as bibliotecas C++ de tempo de execução do Microsoft Visual 2008 são necessárias

> As DLLs nativas do SQL Server Compact 4,0 precisam das bibliotecas C++ de tempo de execução do Microsoft Visual 2008 (x86, IA64 e x64), Service Pack 1.
> 
> **Solução alternativa**  
> Instale o .NET Framework 3,5 SP1. Isso também instala as bibliotecas C++ de tempo de execução do Visual 2008 SP1. Você pode baixar as bibliotecas do seguinte local:   
>   
> [Atualização de C++ segurança da ATL do pacote redistribuível do Microsoft Visual 2008 Service Pack 1](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> Observe que a instalação do .NET Framework 2,0, 3,0 ou 4 *não* instala as bibliotecas de C++ tempo de execução do Visual 2008 SP1.

#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a>Problema: se o SQL Server Compact for instalado antes da instalação do .NET Framework no computador, o nome invariável do provedor não será registrado no arquivo Machine. config do .NET Framework

> SQL Server Compact pode ser instalado em um computador que não tenha o .NET Framework instalado porque SQL Server Compact requer o .NET Framework. Se nenhuma .NET Framework versão 3,5 nem 4 estiver instalada antes de instalar o SQL Server Compact, a instalação do SQL Server Compact não registrará seu nome invariável do provedor no arquivo *Machine. config* . Qualquer aplicativo que dependa da entrada SQL Server Compact no arquivo *Machine. config* falhará. A entrada de registro de nome invariável em *Machine. config* é semelhante ao exemplo a seguir:
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> **Solução alternativa**  
> Desinstale SQL Server Compact 4,0 CTP1. Baixe e instale as versões completas do .NET Framework do seguinte local:
> 
> [Microsoft .NET Framework 3,5 Service Pack 1 (pacote completo)](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [Versão do Microsoft .NET Framework 4,0 (pacote completo)](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> Em seguida, reinstale [SQL Server Compact 4,0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).

<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a>Instalando aplicativos

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>Problema: a instalação de um aplicativo pode levar muito tempo se a pasta meus documentos do usuário for redirecionada para um compartilhamento de rede

> **Solução alternativa**  
> Nenhum. O aplicativo pode levar algum tempo para ser instalado, mas será instalado corretamente.

<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a>Publicando aplicativos

#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a>Problema: o site pode não funcionar após a publicação se o campo "URL de destino" não for prefixado com http://ou https://

> Na caixa de diálogo **configurações de publicação** , se a URL de destino não começar com `http://` ou `https://`, o site poderá não funcionar após a implantação.
> 
> **Solução alternativa**  
> Certifique-se de que, antes de publicar um site, a URL de destino na caixa de diálogo **configurações de publicação** comece com `http://` ou `https://`.

#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a>Problema: a publicação de um banco de dados MySQL falha com o erro "falha ao publicar o banco de dados. Isso pode acontecer se o banco de dados remoto não puder executar o script. "

> O erro pode ocorrer por vários motivos. Um motivo para você ver esse erro é se o script de banco de dados contiver um caractere de aspas simples (') e o conjunto de caracteres padrão do banco de dados MySQL de destino não for UTF-8.
> 
> **Solução alternativa**  
> Defina o conjunto de caracteres padrão para o banco de dados MySQL remoto como UTF-8.

<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a>Outros problemas

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a>Problema: a pesquisa/filtro não funciona em relatórios para Group by: tipo de problema

> Ao executar um relatório para um site, se você inserir texto na caixa *Filtrar por URL* e clicar em *Pesquisar*, nada acontecerá. Isso ocorre porque esse controle não é funcional enquanto o estado *Group by* do relatório é definido como *tipo de emissão*, que é o padrão.
> 
> **Solução alternativa** Na guia *Agrupar por* da faixa de, clique em *URL* para agrupar as entradas por sua URL de origem. A caixa de texto e o botão para filtrar as entradas são funcionais enquanto estão nesse estado.

#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a>Problema: falha na execução de aplicativos WCF com o IIS Express

> A navegação para um aplicativo WCF resulta em um erro como o seguinte:
> 
> *Não foi possível carregar o arquivo ou o assembly ' Microsoft. Web. Administration, versão = 7.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35 ' ou uma de suas dependências. O sistema não pode localizar o arquivo especificado.*
> 
> Isso ocorre porque IIS Express versão beta não dá suporte ao WCF por padrão.
> 
> **Solução alternativa** Use qualquer uma das seguintes soluções alternativas (a solução alternativa #2 requer o Microsoft Windows Vista ou superior):
> 
> 
> 1. Copie os assemblies *Microsoft. Web. dll* e *Microsoft. Web. Administration. dll* do local de instalação do WebMatrix para o diretório *bin* do aplicativo WCF. Por padrão, o WebMatrix é instalado na subpasta *Microsoft WebMatrix* na pasta *arquivos de programas* do sistema.
> 2. No Microsoft Windows Vista ou superior, crie um symlink para os assemblies no diretório *bin* usando os comandos a seguir. (Essa abordagem tem a vantagem de que ele não cria uma cópia dos assemblies.)
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. Instale os dois assemblies no GAC. Em um prompt com privilégios elevados, execute os seguintes comandos:
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]

#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a>Problema: o WebMatrix Beta 3 não pode executar determinadas tarefas que exigem elevação

> O WebMatrix Beta 3 não pode executar determinadas tarefas que exigem elevação, como a instalação de componentes adicionais nas seguintes situações:
> 
> - No Windows Vista ou no Windows 7, você está conectado com uma conta que não tem privilégios administrativos e o UAC (controle de conta de usuário) está desabilitado.
> - Você está usando o Microsoft Windows XP ou o Microsoft Windows Server 2003.
> 
> **Solução alternativa**  
> A maioria das tarefas no WebMatrix Beta 3 não requer permissão administrativa. Para aqueles que fazem isso, você pode executar a operação como administrador ou seguir estas etapas:
> 
> - No Windows Vista ou no Windows 7, habilite o UAC.
> - No Windows XP, adicione o usuário ao grupo de segurança Administradores.

#### <a name="issue-site-from-web-gallery-is-disabled"></a>Problema: "site da Galeria da Web" está desabilitado

> A opção **site da Galeria da Web** será desabilitada se o Web Platform Installer 3,0 não estiver instalado.
> 
> **Solução alternativa**  
> Instale o [Microsoft Web Platform Installer 3,0](https://go.microsoft.com/fwlink/?LinkID=194638).

#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a>Problema: no Windows Server 2003, o IIS Express não é iniciado para um usuário não administrativo

> No Windows Server 2003, quando você inicia uma página ou inicia IIS Express, IIS Express não é iniciado. Para páginas da Web, é exibido um erro que indica que o aplicativo foi iniciado por um usuário não administrativo.
> 
> **Solução alternativa**  
> Inicie o WebMatrix Beta 3 como um usuário administrativo. Para obter mais detalhes, consulte o seguinte artigo da base de conhecimento:  
>   
> [Um aplicativo que é iniciado por um usuário não administrativo não pode escutar o tráfego HTTP do computador no qual o aplicativo está sendo executado no Windows Vista, no Windows Server 2003 ou no Windows XP.](https://support.microsoft.com/kb/939786)

#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a>Problema: o Google Chrome não está disponível como uma opção de execução

> O Google Chrome não é exibido na lista de navegadores em **executar** na guia **início** .
> 
> **Solução alternativa**  
> Algumas versões do Google Chrome não se registram corretamente com o recurso de programas padrão no Windows. Como alternativa, inicie o Google Chrome, clique no menu *Personalizar e controlar Google Chrome* , clique em *Opções*e, em seguida, clique em *tornar o Google Chrome meu navegador padrão*.

#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a>Problema: a caixa de diálogo "chave estrangeira" não permite inserir uma chave primária

> A caixa de diálogo **chave estrangeira** não permite que você insira o nome da chave primária da tabela de chave primária.
> 
> **Solução alternativa**  
> Isso é intencional. Você não precisa inserir o nome da chave primária da tabela de chave primária.

#### <a name="issue-the-relationships-button-is-disabled"></a>Problema: o botão "relações" está desabilitado

> O botão **relações** na guia **tabela** do espaço de trabalho **bancos de dados** está desabilitado para bancos de dados SQL Server Compact.
> 
> **Solução alternativa**  
> Nenhum. SQL Server Compact não oferece suporte a relações entre tabelas.

#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a>Problema: as consultas SQL parametrizadas geram exceções

> No SQL Server Compact 4,0, se você não especificar um tipo de dados como `SqlDbType` ou `DbType` para parâmetros em consultas parametrizadas, uma exceção será lançada quando a consulta for executada.
> 
> **Solução alternativa**  
> Defina explicitamente o tipo de dados para parâmetros como `SqlDbType` ou `DbType`. Isso é crítico no caso de tipos de dados de BLOB (`image` e `ntext`). Use um código semelhante ao seguinte:
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
> 
> [!code-vb[Main](beta3/samples/sample21.vb)]

<a id="More_Info"></a>

## <a name="for-more-information"></a>Para obter mais informações

Para obter mais informações sobre o WebMatrix Beta 3, consulte os seguintes sites:

- [IIS.net](http://iis.net/)
- [ASP.NET](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

---

© 2010 Microsoft Corporation. Todos os direitos reservados. [Termos de uso](https://msdn.microsoft.cos/cc300389.aspx).
