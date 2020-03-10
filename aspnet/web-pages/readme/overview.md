---
uid: web-pages/readme/overview
title: Leiame do WebMatrix | Microsoft Docs
author: rick-anderson
description: Leiame do WebMatrix and Páginas da Web do ASP.NET (Razor) 1,0 versão
ms.author: riande
ms.date: 01/06/2011
ms.assetid: 36c5beeb-45a7-48a0-9c30-f82cdf5c5f5f
msc.legacyurl: /web-pages/readme
msc.type: content
ms.openlocfilehash: fac53e935860a90d8f2aa96699d56d66ade3a40f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78563465"
---
# <a name="webmatrix-readme"></a>Leiame do WebMatrix

13 de janeiro de 2011

## <a name="contents"></a>Conteúdo

> [!NOTE]
> Este Leiame se aplica à versão 1,0 do WebMatrix.

- [Visão geral](#Overview)
- [Instalação](#Installation_Notes)
- [Como publicar aplicativos](#InstructionsForPublishingApplications)
- [Alterações e problemas](#ChangesAndIssues)

    - [Instalação do WebMatrix 1,0](#Known_Issues_Installation)
    - [Páginas da Web do ASP.NET](#Known_Issues_ASPNET)
    - [WebMatrix](#Known_Issues_WebMatrix)
    - [IIS Express](#Known_Issues_IISExpress)
    - [SQL Server Compact](#Known_Issues_SQLServerCompact)
    - [Instalando aplicativos](#Known_Issues_Installing_Applications)
    - [Publicando aplicativos](#Known_Issues_Publishing_Applications)
- [Para obter mais informações](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>Visão geral

> O Microsoft WebMatrix 1,0 é uma pilha de desenvolvimento na Web gratuita que é instalada em minutos. Ele integra um servidor Web com estruturas de banco de dados e programação para criar uma experiência única e integrada. Você pode usar o WebMatrix para simplificar a maneira como você codifica, testa e publica seu próprio site ASP.NET ou PHP, ou pode usar o WebMatrix para iniciar um novo site usando aplicativos de software livre populares, como o DotNetNuke, o Umbraco, o WordPress ou o Joomla. O WebMatrix usa o mesmo ambiente avançado de servidor Web, mecanismo de banco de dados e estruturas que executará seu site na Internet, o que torna a transição de desenvolvimento para produção tranqüila e contínua.

<a id="Installation_Notes"></a>

## <a name="installation"></a>Instalação

> Para instalar o WebMatrix 1,0, você deve primeiro instalar o [Microsoft Web Platform Installer 3,0](https://go.microsoft.com/fwlink/?LinkID=194638). Depois de instalar o Web Platform Installer, você pode usá-lo para instalar o WebMatrix.
> 
> Se você tiver problemas durante a instalação, consulte [Solucionando problemas com Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).

<a id="InstructionsForPublishingApplications"></a>
## <a name="how-to-publish-applications"></a>Como publicar aplicativos

> Consulte [as instruções passo a passo para publicar aplicativos](https://go.microsoft.com/fwlink/?LinkID=196149)

<a id="ChangesAndIssues"></a>

## <a name="changes-and-issues"></a>Alterações e problemas

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-10-installation-issues"></a>Problemas de instalação do WebMatrix 1,0

#### <a name="issue-webmatrix-10-is-available-only-on-platforms-that-support-microsoft-net-framework-4"></a>Problema: o WebMatrix 1,0 está disponível apenas em plataformas que dão suporte ao Microsoft .NET Framework 4

> O .NET Framework versão 4 é necessário para o WebMatrix. Em determinados casos, o instalador do WebMatrix 1,0 permitirá que você tente instalar o em uma plataforma que não faça parte do conjunto de configuração com suporte. Em particular, o Windows Vista sem a atualização do SP1 permitirá que você comece a instalação do WebMatrix, mas o componente .NET Framework 4 falhará e bloqueará a instalação.
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

#### <a name="issue-cannot-install-webmatrix-10-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Problema: não é possível instalar o WebMatrix 1,0 se o Microsoft Visual Studio 2008 for instalado sem Microsoft Visual Studio 2008 SP1

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

Esta seção do documento descreve os novos recursos, as alterações e os problemas conhecidos com a versão 1,0 do Páginas da Web do ASP.NET com sintaxe Razor.

- [Novos recursos](#NewFeatures)
- [Alterações](#Changes)
- [Problemas](#Issues)

#### <a id="NewFeatures"></a>Novos recursos

#### <a name="new-configuration-setting-added-to-disable-the-package-manager"></a>Novo: definição de configuração adicionada para desabilitar o Gerenciador de pacotes

> Uma nova chave de `asp:AdminManagerEnabled` está disponível para o elemento `<appSettings>` no arquivo *Web. config* , o que permite desabilitar completamente o Gerenciador de pacotes. O valor padrão para esse elemento é true, o que significa que, se ele não estiver incluído no arquivo *Web. config* , o Gerenciador de pacotes será habilitado. Para desabilitar o Gerenciador de pacotes, adicione o seguinte elemento ao arquivo *Web. config* na raiz do site:
> 
> [!code-xml[Main](overview/samples/sample1.xml)]

#### <a id="Changes"></a>For

#### <a name="change-webpagesadminfoldervirtualpath-key-renamed-to-aspadminfoldervirtualpath"></a>Alteração: a chave "webpages: AdminFolderVirtualPath" foi renomeada para "ASP: AdminFolderVirtualPath"

> A chave de `webPages:AdminFolderVirtualPath` que pode ser adicionada ao arquivo *Web. config* para especificar o local do Gerenciador de pacotes foi renomeada para usar o namespace `asp:` em vez do namespace `webPages`. Se você tiver usado esse elemento, deverá renomeá-lo no arquivo de configuração.

#### <a id="Issues"></a>  Problemas Conhecidos

#### <a name="issue-passwords-for-membership-users-no-longer-recognized"></a>Problema: senhas para usuários da associação não são mais reconhecidas

> O algoritmo para criar e armazenar senhas de associação (logon) foi alterado para ser mais seguro. Como resultado, as senhas armazenadas para Membros (usuários) criadas em versões beta do ASP.NET Razor não serão reconhecidas. 
> 
> **Solução alternativa** Se o site ainda não tiver sido colocado em produção, remova os registros de usuário do banco de dados de associação. Se o banco de dados estiver ativo, gere programaticamente as senhas existentes no banco de dados de associação.

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>Problema: comportamento inesperado ao usar uma tabela de usuário personalizada para associação

> Para inicializar o provedor de associação para um site do Razor do ASP.NET, chame o método `WebSecurity.InitializeDatabaseConnection`. (No WebMatrix, o modelo de site inicial inclui uma chamada para esse método no arquivo *\_AppStart. cshtml* ). Se o parâmetro `autoCreateTables` desse método for definido como true (por padrão, ele será definido como true no modelo de site inicial) e, se um nome de tabela não reconhecido for passado para o método (o segundo parâmetro), o método não gerará um erro. Em vez disso, ele cria automaticamente a tabela.
> 
> Isso pode ser um problema se você pretender usar uma tabela de usuário personalizada para associação, mas passar o nome de tabela incorreto para o método `WebSecurity.InitializeDatabaseConnection`. Como o método não gera, por padrão, um erro se a tabela especificada não existir e, em vez disso, criar uma nova tabela, o aplicativo poderá parecer estar funcionando. No entanto, o código do aplicativo que depende da sua tabela de usuário personalizada (e em campos) pode eventualmente falhar com erros inesperados.
> 
> **Solução alternativa**  
> Verifique se o nome passado no método `InitializeDatabaseConnection` corresponde à tabela de perfil de usuário no banco de dados de associação ou certifique-se de que o parâmetro `autoCreateTables` está definido como false.

#### <a name="issue-error-message-the-admin-module-requires-access-to-app_data"></a>Problema: mensagem de erro "o módulo de administrador requer acesso a ~/app dados de\_"

> Em algumas circunstâncias, tentar criar usuários ou, de outra forma, trabalhar com o sistema de associação ASP.NET pode fazer com que a página exiba o erro *o módulo de administração requer acesso a ~/App\_dados*. Isso ocorre se a conta em que o IIS ou o IIS Express está sendo executado não tem permissões para criar e gravar na pasta de *dados do aplicativo\_* na raiz do site. 
> 
> **Solução alternativa** Crie manualmente uma pasta de *dados de\_de aplicativo* para o site. Em seguida, verifique se a conta do Windows em que o aplicativo é executado (normalmente serviço de rede) tem permissões de leitura/gravação para pastas raiz do aplicativo e para subpastas como dados de\_de aplicativo. Informações mais detalhadas estão disponíveis nos problemas do artigo da base de conhecimento [com SQL Server Express instanciação de usuário e projetos de aplicativo Web ASP.net](https://support.microsoft.com/kb/2002980).

#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>Problema: erro "falha ao gerar uma instância de usuário do SQL Server"

> Se um aplicativo Web do WebMatrix usar SQL Server Express e estiver executando o IIS 7,5 no Windows 7 ou no Windows Server 2008 R2, você poderá ver um erro que indica que SQL Server não é possível recuperar o caminho do aplicativo local do usuário em tempo de execução.
> 
> **Solução alternativa** Verifique se a conta do Windows em que o aplicativo é executado (normalmente serviço de rede) tem permissões de leitura/gravação para pastas raiz do aplicativo e para subpastas como *dados de\_de aplicativo*. Informações mais detalhadas estão disponíveis nos problemas do artigo da base de conhecimento [com SQL Server Express instanciação de usuário e projetos de aplicativo Web ASP.net](https://support.microsoft.com/kb/2002980).

#### <a name="issue-files-that-contains-package-manager-resources-or-package-manager-passwords-are-servable-under-iis-60-and-earlier"></a>Problema: os arquivos que contêm recursos do Gerenciador de pacotes ou senhas do Gerenciador de pacotes podem ser servveis no IIS 6,0 e versões anteriores

> Se você implantar um aplicativo Páginas da Web do ASP.NET (Razor) que foi criado usando a versão RC2, e se o aplicativo contiver um arquivo *password. txt* ou *PackageSources. txt* em */app\_data/admin*, o IIS 6,0 servirá o arquivo se solicitado, expondo potencialmente as senhas para a instância do Gerenciador de pacotes. 
> 
> **Solução alternativa** Renomeie o arquivo *password. txt* ou *PackageSources. txt* para *password. config* ou *PackageSources. config*. Por padrão, o IIS 6,0 não atende aos arquivos que têm a extensão *. config* . (No IIS 7, nenhum arquivo na pasta de *dados de\_de aplicativo* é servido, portanto, não é necessário renomear os arquivos.)

#### <a name="issue-uninstalling-packages-installed-using-the-beta-3-release-does-not-completely-remove-package-components"></a>Problema: a desinstalação de pacotes instalados usando a versão beta 3 não remove completamente os componentes do pacote

> Se você instalou um pacote usando o Gerenciador de pacotes na versão beta 3 e, em seguida, tentar desinstalá-lo usando a versão atual, o pacote não será completamente desinstalado. Usar o botão **desinstalar** do Gerenciador de pacotes remove alguns componentes, mas deixa o código da biblioteca do pacote e não atualiza o arquivo *Package. config* .
> 
> **Solução alternativa**   
> Execute estas etapas:  
> 1. Exclua o *aplicativo\_pasta Data\packages* Isso remove todos os pacotes.   
> 2. Exclua o arquivo *Packages. config* na raiz do site.

#### <a name="issue-in-visual-studio-invoking-the-web-based-package-manager-takes-the-application-offline"></a>Problema: no Visual Studio, invocar o Gerenciador de pacotes baseado na Web coloca o aplicativo offline

> Se você estiver trabalhando no Visual Studio (não no WebMatrix) e usar a funcionalidade de *Administração de\_* para iniciar o Gerenciador de pacotes, o Visual Studio coloca o aplicativo offline e posta o *aplicativo\_offline. htm* na raiz do site, o que interrompe a sua capacidade de usar o Gerenciador de pacotes.
> 
> [!NOTE]
> Embora você normalmente Veja esse comportamento ao usar a interface do Gerenciador de pacotes baseada na Web, o mesmo comportamento ocorrerá se você adicionar, remover ou modificar todos os arquivos na pasta de *dados do\_de aplicativos* .
> 
> **Solução alternativa**   
> Para trabalhar com pacotes no Visual Studio, use a extensão NuGet em vez do Gerenciador de pacotes baseado na Web. Para obter informações, consulte a [documentação do NuGet](https://docs.microsoft.com/nuget/). Se você estiver trabalhando com outros arquivos na pasta de *dados do\_de aplicativos* , considere manter os arquivos em outro lugar para evitar esse problema. Se isso não for prático, exclua o arquivo *offline. htm do aplicativo\_* manualmente ou aguarde até que o site volte a ficar online automaticamente (por padrão, após 30 segundos).

#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>Problema: Visual Studio IntelliSense e modelos de projeto disponíveis somente no ASP.NET MVC versão 3

> A instalação do Páginas da Web do ASP.NET não também instala ferramentas para o Visual Studio, como IntelliSense e modelos de projeto para Páginas da Web do ASP.NET aplicativos.
> 
> **Solução alternativa** Para usar o IntelliSense e modelos de projeto para Páginas da Web do ASP.NET aplicativos no Visual Studio, instale o ASP.NET MVC 3 RC por meio do Web Platform Installer ou do [instalador](https://go.microsoft.com/fwlink/?LinkID=191797)autônomo.

#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>Problema: lendo feeds ou outros dados externos por meio de um servidor proxy

> Se o servidor que está executando o site estiver protegido por um servidor proxy, talvez seja necessário configurar as informações de proxy no arquivo *Web. config* para poder ler as informações provenientes de fora do site. Por exemplo, se você usar o auxiliar de `ReCaptcha`, o auxiliar se comunicará com o serviço reCAPTCHA, mas poderá ser bloqueado pelo servidor proxy. Da mesma forma, os feeds usados no Páginas da Web do ASP.NET, como o feed usado pelo Gerenciador de pacotes, podem exigir a configuração de proxy.
> 
> Se você tiver problemas ao trabalhar com um serviço externo ou trabalhar com o feed de pacote, coloque os seguintes elementos no arquivo *Web. config* da raiz do aplicativo:
> 
> [!code-xml[Main](overview/samples/sample2.xml)]
> 
> Para obter mais informações sobre como configurar um servidor proxy, consulte [&lt;elemento&gt; proxy (configurações de rede)](https://msdn.microsoft.com/library/sa91de1e.aspx) no site do MSDN.

#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>Problema: a desinstalação do .NET Framework versão 4 desabilita Páginas da Web do ASP.NET com a sintaxe do Razor

> Se você desinstalar o .NET Framework versão 4 e, em seguida, reinstalá-lo, Páginas da Web do ASP.NET com sintaxe Razor será desabilitado. As páginas com a extensão *. cshtml* não são executadas corretamente. Páginas da Web do ASP.NET registra um assembly no arquivo *Web. config da* raiz do computador e a remoção do .NET Framework remove esse arquivo. Reinstalar o .NET Framework instala uma nova versão do arquivo de configuração, mas não adiciona a referência para o assembly Páginas da Web do ASP.NET.
> 
> **Solução alternativa** Depois de reinstalar o .NET Framework, reinstale o Páginas da Web do ASP.NET com sintaxe Razor. Isso adiciona o seguinte elemento ao arquivo *Web. config* na raiz do computador, que normalmente está no seguinte local:  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](overview/samples/sample3.xml)]

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
>     [!code-xml[Main](overview/samples/sample4.xml)]

#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>Problema: Implantando um aplicativo em um computador que não tem SQL Server Compact instalado

> Os aplicativos que incluem bancos de dados do SQL Server Compact podem ser executados em um computador onde o SQL Server Compact não está instalado. O Microsoft WebMatrix 1,0 copia automaticamente esses binários para você e executa as transformações de arquivo *Web. config* apropriadas.
> 
> **Solução alternativa** Se você precisar copiar esses arquivos e fazer com que o arquivo *Web. config* seja alterado manualmente, faça o seguinte:
> 
> 1. Copie os assemblies do mecanismo de banco de dados para a pasta *bin* (e subpastas) do aplicativo no computador de destino:  
> 
>    - Copie *c:\Arquivos de programas\microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll*   
>      **para** *\Bin*
>    - Copie *c:\Arquivos de programas\microsoft SQL Server Compact Edition\v4.0\Private\x86\\* **para** *\Bin\x86*
>    - Copie *c:\Arquivos de programas\microsoft SQL Server Compact Edition\v4.0\Private\amd64\\* * **para** *\Bin\amd64*
> 
> 2. Na pasta raiz do site, crie ou abra um arquivo *Web. config* . (No WebMatrix 1,0, esse tipo de arquivo estará disponível se você clicar em **tudo** na caixa de diálogo **escolher um tipo de arquivo** .)
> 3. Adicione o seguinte elemento como um filho do elemento `<configuration>` (não dentro do elemento `<system.web>`):
> 
>     [!code-xml[Main](overview/samples/sample5.xml)]

#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>Problema: os auxiliares "banco de dados" e "WebGrid" não funcionam em confiança média em Visual Basic

> Se você estiver usando Visual Basic (criando arquivos *. vbhtml* ), os auxiliares `Database` e `WebGrid` não funcionarão se o aplicativo for definido para usar a confiança média.
> 
> **Solução alternativa**  
> Se você usar o Visual Studio 2010, poderá resolver esse problema instalando a versão do Service Pack 1. Até que a versão final da versão SP1 esteja disponível, você pode baixar a versão beta do SP1 na página [Microsoft Visual Studio 2010 Service Pack 1 beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) no centro de download da Microsoft.   
>   
> Se isso não for prático ou se você não usar o Visual Studio 2010, poderá definir temporariamente o aplicativo para usar a confiança total.

#### <a name="issue-applicationpart-resources-are-externally-accessible"></a>Problema: os recursos "ApplicationPart" são externamente acessíveis

> Se um assembly contiver objetos que derivam da classe `ApplicationPart`, os recursos do assembly serão expostos pela classe `ResourceRouteHandler`. Por exemplo, considere a seguinte URL:  
>   
> `~/r.ashx/System.Web.WebPages.Administration/Resources/AdminResources.resources`  
>   
> Essa solicitação baixa todas as cadeias de caracteres de recurso no assembly *System. Web. webpages. Administration. dll* . Todos os recursos incorporados (mesmo aqueles que não devem ser servidos como conteúdo estático) são baixados. Se os recursos inseridos contiverem informações confidenciais, isso poderá representar um risco à segurança. 
> 
> **Solução alternativa**   
> Se você criar um objeto **ApplicationPart** , certifique-se de que os recursos inseridos associados ao assembly do objeto **ApplicationPart** não contenham informações confidenciais.

<a id="Known_Issues_WebMatrix"></a>

### <a name="webmatrix"></a>WebMatrix

> [!NOTE]
> Para obter informações sobre problemas de instalação do WebMatrix, consulte [problemas de instalação do WebMatrix](#Known_Issues_Installation) anteriormente neste documento.

Esta seção do documento descreve problemas conhecidos do ambiente de desenvolvimento do WebMatrix.

#### <a name="issue-changes-in-the-username-or-password-of-a-database-connection-string-in-a-webconfig-file-are-not-reflected-in-the-databases-workspace"></a>Problema: alterações no nome de usuário ou senha de uma cadeia de conexão de banco de dados em um arquivo Web. config não são refletidas no espaço de trabalho bancos

> **Solução alternativa**  
> 
> 1. No arquivo *Web. config* , altere o nome do banco de dados na cadeia de conexão (por exemplo, adicione "1" a ele).
> 2. Salve o arquivo *Web. config* .
> 3. Clique em **bancos de dados** e em atualizar.
> 4. Altere o nome do banco de dados na cadeia de conexão no arquivo *Web. config* de volta para o nome do banco de dados original.
> 5. Salve o arquivo *Web. config* .
> 6. Clique em **bancos de dados** e em atualizar.

#### <a name="issue-folders-created-by-webmatrix-cannot-be-deleted"></a>Problema: pastas criadas pelo WebMatrix não podem ser excluídas

> Se o WebMatrix estiver sendo executado usando permissões elevadas (ou seja, você iniciou o WebMatrix usando a opção **Executar como administrador** no Windows), as pastas criadas pelo WebMatrix não poderão ser excluídas usando o Windows Explorer.
> 
> **Solução alternativa**  
> Execute o Windows Explorer usando permissões elevadas. Siga estas etapas:  
> 
> 1. No Windows, clique em **Iniciar**.
> 2. Insira "Windows Explorer" e clique com o botão direito do mouse na entrada do **Windows Explorer**.
> 3. Clique em **Executar como administrador**. Em seguida, você pode excluir as pastas.

#### <a name="issue-webmatrix-10-is-unable-to-perform-certain-tasks-that-require-elevation"></a>Problema: o WebMatrix 1,0 não pode executar determinadas tarefas que exigem elevação

> O WebMatrix 1,0 não pode executar determinadas tarefas que exigem elevação, como a instalação de componentes adicionais nas seguintes situações:
> 
> - No Windows Vista ou no Windows 7, você está conectado com uma conta que não tem privilégios administrativos e o UAC (controle de conta de usuário) está desabilitado.
> - Você está usando o Microsoft Windows XP ou o Microsoft Windows Server 2003.
> 
> **Solução alternativa**  
> A maioria das tarefas no WebMatrix 1,0 não exigem permissão administrativa. Para aqueles que fazem isso, você pode executar a operação como administrador ou seguir estas etapas:
> 
> - No Windows Vista ou no Windows 7, habilite o UAC.
> - No Windows XP, adicione o usuário ao grupo de segurança Administradores.

#### <a name="issue-site-from-web-gallery-is-disabled"></a>Problema: "site da Galeria da Web" está desabilitado

> A opção **site da Galeria da Web** será desabilitada se o Web Platform Installer 3,0 não estiver instalado.
> 
> **Solução alternativa**  
> Instale o [Microsoft Web Platform Installer 3,0](https://go.microsoft.com/fwlink/?LinkID=194638).

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

#### <a name="issue-intellisense-is-not-available-in-webmatrix-for-razor-syntax-c-or-visual-basic"></a>Problema: o IntelliSense não está disponível no WebMatrix para sintaxe Razor C#, ou Visual Basic

> O IntelliSense tem suporte no WebMatrix para HTML e CSS. No entanto, ele não está disponível para outros idiomas. 
> 
> **Solução alternativa**   
> Nenhum.

#### <a name="issue-intellisense-for-html-and-css-suggests-elements-that-are-not-contextually-appropriate"></a>Problema: o IntelliSense para HTML e CSS sugere elementos que não são contextualmente apropriados

> O IntelliSense para marcação no WebMatrix dá suporte a HTML usando o [esquema de transição XHTML 1,0](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) e o CSS usando o [esquema CSS 2,1](http://www.w3.org/TR/CSS2/). Como o IntelliSense é baseado nesses esquemas específicos, determinadas marcas, atributos ou propriedades podem ser sugeridas que não são apropriadas para a definição de página ou estilo atual. Para HTML, ele também pode levar a sugestões inesperadas no conteúdo que podem ser interpretadas como XHTML malformado (por exemplo, quando as marcas não são fechadas). Esse problema pode ser mais perceptível se o ponto de inserção estiver dentro de uma marca incompleta; Nesse caso, o IntelliSense pode sugerir novas marcas de abertura ou oferecer outras sugestões incorretas. 
> 
> **Solução alternativa**   
> Para HTML, verifique se você está trabalhando em uma página XHTML completa e bem formada. Para o CSS, não há nenhuma solução alternativa.

#### <a name="issue-intellisense-is-not-invoked-while-you-type"></a>Problema: o IntelliSense não é invocado enquanto você digita

> Às vezes, o IntelliSense pode não ser invocado, pois HTML ou CSS está sendo inserido no editor. Em particular, isso pode acontecer quando o ponto de inserção está diretamente ao lado de outro elemento ou no final de um arquivo. 
> 
> **Solução alternativa**   
> Verifique se há espaço em branco ao lado do ponto de inserção e se o ponto de inserção não está no final de um arquivo. Você também pode invocar o IntelliSense manualmente pressionando CTRL + espaço.

#### <a name="issue-no-ui-is-available-for-disabling-intellisense"></a>Problema: nenhuma interface do usuário está disponível para desabilitar o IntelliSense

> O WebMatrix 1,0 não fornece nenhuma interface do usuário nem gesto para desabilitar o IntelliSense. 
> 
> **Solução alternativa**   
> Inicie o WebMatrix usando o comando a seguir, que inclui uma opção que desabilita o IntelliSense:  
>   
> `WebMatrix.exe #ExecuteCommand# EditorIntelliSense off`

<a id="Known_Issues_IISExpress"></a>
### <a name="iis-express"></a>IIS Express

IIS Express tem seu próprio arquivo Leiame, que está disponível na seguinte URL:

[https://go.microsoft.com/fwlink/?LinkID=207675&amp; clcid = 0x409](https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409)

<a id="Known_Issues_SQLServerCompact"></a>

### <a name="sql-server-compact"></a>SQL Server Compact

SQL Server Compact tem seu próprio arquivo Leiame, que está disponível na seguinte URL:

[https://go.microsoft.com/fwlink/?LinkID=208545](https://go.microsoft.com/fwlink/?LinkID=208545&amp;clcid=0x409)

Para obter informações sobre problemas que envolvem a instalação do SQL Server Compact como parte do WebMatrix, consulte [problemas de instalação do WebMatrix](#Known_Issues_Installation) anteriormente neste documento.

### <a id="Known_Issues_Installing_Applications"></a>Instalando aplicativos

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>Problema: a instalação de um aplicativo pode levar muito tempo se a pasta meus documentos do usuário for redirecionada para um compartilhamento de rede

> **Solução alternativa**  
> Nenhum. O aplicativo pode levar algum tempo para ser instalado, mas será instalado corretamente.

### <a id="Known_Issues_Publishing_Applications"></a>Publicando aplicativos

#### <a name="issue-required-permissions-cannot-be-acquired-error-when-publishing-a-sql-compact-database"></a>Problema: erro "não é possível adquirir as permissões necessárias" ao publicar um banco de dados do SQL Compact

> O WebMatrix não dá suporte total à implantação de binários de suporte para SQL Server Compact a um servidor que esteja executando o .NET Framework versão 3,5 com uma configuração de confiança média.
> 
> **Solução alternativa**  
> A solução alternativa preferida é instalar o .NET Framework 4 no servidor. Como alternativa, faça o seguinte:
> 
> 1. Adicione os seguintes elementos à seção `SecurityClasses` no arquivo *Web\_MediumTrust. config* :
> 
>     [!code-html[Main](overview/samples/sample6.html)]
> 2. Crie um novo conjunto de permissões no arquivo *Web\_MediumTrust. config* com as seguintes permissões necessárias:
> 
>     [!code-html[Main](overview/samples/sample7.html)]
> 3. Aplique o conjunto de permissões a SQL Server Compact colocando os seguintes elementos no arquivo *Web\_MediumTrust. config* :
> 
>     [!code-html[Main](overview/samples/sample8.html)]

#### <a name="issue-gallery-and-phpbb-web-applications-display-a-service-is-unavailable-error-after-publishing"></a>Problema: os aplicativos Web da galeria e do PhpBB exibem um erro "Serviço indisponível" após a publicação

> Em algumas circunstâncias, a publicação de um aplicativo causa um erro "Serviço indisponível".
> 
> **Solução alternativa**  
> No WebMatrix, adicione uma barra invertida (\) ao final do nome do servidor na janela de **configurações de publicação** e, em seguida, publique o aplicativo novamente.

#### <a name="issue-moodle-website-layout-and-links-are-broken-after-publishing"></a>Problema: layout e links do site Moodle são quebrados após a publicação

> Depois que você publicar um aplicativo Moodle, o aplicativo não funcionará corretamente.
> 
> **Solução alternativa**  
> No WebMatrix, adicione uma barra (/) ao final do campo **nome do site** na janela **configurações de publicação** e, em seguida, publique o aplicativo novamente.

#### <a name="issue-publishing-nopcommerce-fails-with-a-database-error"></a>Problema: a publicação de nopCommerce falha com um erro de banco de dados

> A publicação de nopCommerce falha e relata um erro de banco de dados como "inserir na tabela de log de\_de Nop falhou".
> 
> **Solução alternativa**  
> 
> 1. No WebMatrix, clique em **executar** para iniciar o Nopcommerce localmente.
> 2. Faça logon na página de administração.
> 3. Clique no menu **sistema** .
> 4. Clique na opção **log** .
> 5. Clique no botão **Limpar Log**.
> 6. Publique nopCommerce novamente.

#### <a name="issue-silverstripe-cms-displays-a-http-500-php-fcgi-error-when-you-download-a-published-site"></a>Problema: o Silverstripe CMS exibe um erro "HTTP 500 PHP FCGI" ao baixar um site publicado

> **Solução alternativa**  
> Depois de clicar em **baixar site publicado**, pule `silverstripe-cache/manifest_main` na **visualização de publicação**. Esse arquivo é usado para fins de cache e é específico para cada computador.

#### <a name="issue-subtext-displays-server-error-in--application-when-you-download-a-published-site"></a>Problema: o subtexto exibe "erro de servidor no aplicativo"/"quando você baixa um site publicado

> **Solução alternativa**  
> Abra o arquivo *Web. config* do site e substitua a ID de usuário e a senha na cadeia de conexão do banco de dados pelo SQL Server credenciais de administrador (as credenciais "SA").
> 
> Como alternativa, siga estas etapas para dar à conta de usuário com a qual você está conectado `db_owner` permissões:
> 
> 1. Instale SQL Server Management Studio usando o Web Platform Installer.
> 2. Conecte-se à instância de SQL Server Express local (por padrão, `.\SQLEXPRESS`).
> 3. Clique em **bancos de dados** &gt; *[LocalSubtextDatabase]* &gt; **segurança** &gt; **usuários** &gt; *[localSubtextUser*] (o padrão é `subtextuser`], clique com o botão direito do mouse e clique em **Propriedades**.
> 4. Selecione **db\_proprietário** na seção Associação de função.

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

#### <a name="issue-some-links-are-not-visible-in-dotnetnuke-after-publishing-or-downloading-the-site"></a>Problema: alguns links não são visíveis no DotNetNuke após a publicação ou o download do site

> Se você publicar ou baixar um site do DotNetNuke, talvez seja necessário limpar o cache para obter os novos links a serem exibidos no site.
> 
> **Solução alternativa**
> 
> 1. Faça logon como "host".
> 2. Vá para o menu host e selecione **configurações do host**.
> 3. Role para baixo e, em **Configurações avançadas**, expanda **configurações de desempenho**.
> 4. Clique no link **Limpar cache** para páginas.
> 5. Vá para a parte inferior da página e reinicie o aplicativo.

#### <a name="issue-some-links-in-atomsite-are-broken-after-you-download-a-published-site"></a>Problema: alguns links em AtomSite são quebrados após o download de um site publicado

> **Solução alternativa**  
> No arquivo *Service. config* , no arquivo *users. config* e em todos os arquivos *. xml* , substitua a cadeia de caracteres da URL (por exemplo, `http://myhost.com/atomsite`) pelo local um (por exemplo, `http://localhost:1239`).

#### <a name="issue-mysql-based-applications-like-wordpress-fail-to-publish-and-report-a-database-error"></a>Problema: aplicativos baseados em MySQL como o WordPress falham ao publicar e relatar um erro de banco de dados

> Por padrão, o WebMatrix instala o MySQL com o conjunto de caracteres UTF-8. Se você instalar o MySQL por conta própria e o conjunto de caracteres não for UTF-8 (por exemplo, é Latino1), o processo de publicação para bancos de dados poderá falhar.
> 
> **Solução alternativa**
> 
> 1. Altere o conjunto de caracteres para MySQL para UTF-8. (Para obter detalhes, consulte [conjunto de caracteres do servidor e agrupamento](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) no site do MySQL.)
> 2. Reinstale o aplicativo.
> 3. Republique o aplicativo.

#### <a name="issue-download-published-site-fails-for-applications-that-have-browser-based-setup"></a>Problema: "baixar site publicado" falha para aplicativos que têm a instalação baseada em navegador

> Alguns aplicativos (por exemplo, Kentico CMS) exigem que você os inicie no navegador para executar a instalação após a instalação, como a criação de um banco de dados. Se você publicar um aplicativo como este sem concluir a instalação baseada em navegador, a tentativa de baixar o mesmo site de um servidor remoto falhará.
> 
> **Solução alternativa**  
> Conclua a instalação baseada em navegador antes de publicar o site.

#### <a name="issue-download-published-site-fails-with-a-database-error-for-dotnetnuke-and-kooboo-cms"></a>Problema: "baixar site publicado" falha com um erro de banco de dados para o DotNetNuke e o Kooboo CMS

> Se você tentar baixar um aplicativo de um servidor e tiver credenciais de administrador na cadeia de conexão do banco de dados na caixa de diálogo **configurações de publicação** , poderá ver o seguinte erro no log de publicação:
> 
> [!code-console[Main](overview/samples/sample9.cmd)]
> 
> **Solução alternativa**  
> Se for prático, Republique o site (ou publique-o) usando credenciais de não administrador para o banco de dados.

<a id="More_Info"></a>

## <a name="for-more-information"></a>Para obter mais informações

Para obter mais informações sobre o WebMatrix 1,0, consulte os seguintes sites:

- [IIS.net](http://iis.net/)
- [ASP.NET](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

© 2011 Microsoft Corporation. Todos os direitos reservados. [Termos de uso](https://msdn.microsoft.cos/cc300389.aspx).
