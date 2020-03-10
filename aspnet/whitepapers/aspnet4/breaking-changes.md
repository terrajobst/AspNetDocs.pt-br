---
uid: whitepapers/aspnet4/breaking-changes
title: ASP.NET 4 alterações significativas | Microsoft Docs
author: rick-anderson
description: Este documento descreve as alterações que foram feitas para o .NET Framework versão 4 que pode afetar potencialmente os aplicativos que foram criados usando o...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: d601c540-f86b-4feb-890c-20c806b3da6c
msc.legacyurl: /whitepapers/aspnet4/breaking-changes
msc.type: content
ms.openlocfilehash: 8ccad3b40a723c92a3164de082e1f94577141008
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78546245"
---
# <a name="aspnet-4-breaking-changes"></a>Alterações significativas do ASP.NET 4

> Este documento descreve as alterações que foram feitas para o .NET Framework versão 4 que pode afetar potencialmente os aplicativos que foram criados usando versões anteriores, incluindo as versões ASP.NET 4 beta 1 e beta 2.
> 
> [Baixe este White Paper](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_Breaking_Changes.pdf)

<a id="0.1__Toc256768952"></a><a id="0.1__Toc256770056"></a>

## <a name="contents"></a>Conteúdo

[Configuração de ControlRenderingCompatibilityVersion no arquivo Web. config](#0.1__Toc256770141 "_Toc256770141")  
[Alterações de ClientIDmode](#0.1__Toc256770142 "_Toc256770142")  
[HtmlEncode e UrlEncode agora codificam aspas simples](#0.1__Toc256770143 "_Toc256770143")  
[O analisador de página ASP.NET (. aspx) é mais estrito](#0.1__Toc256770144 "_Toc256770144")  
[Arquivos de definição de navegador atualizados](#0.1__Toc256770145 "_Toc256770145")  
[System. Web. Mobile. dll removido do arquivo de configuração da Web raiz](#0.1__Toc256770146 "_Toc256770146")  
[Validação de solicitação ASP.NET](#0.1__Toc256770147 "_Toc256770147")  
[O algoritmo de hash padrão agora é HMACSHA256](#0.1__Toc256770148 "_Toc256770148")  
[Erros de configuração relacionados à nova configuração raiz do ASP.NET 4](#0.1__Toc256770149 "_Toc256770149")  
[Aplicativos filho ASP.NET 4 falham ao iniciar quando em aplicativos ASP.NET 2,0 ou ASP.NET 3,5](#0.1__Toc256770150 "_Toc256770150")  
[Os sites ASP.NET 4 falham ao iniciar em computadores em que o SharePoint está instalado](#0.1__Toc256770151 "_Toc256770151")  
[A Propriedade HttpRequest. FilePath não inclui mais valores PathInfo](#0.1__Toc256770152 "_Toc256770152")  
[Os aplicativos ASP.NET 2,0 podem gerar erros HttpException que referenciam eurl. axd](#0.1__Toc256770153 "_Toc256770153")  
[Manipuladores de eventos podem não ser gerados em um documento padrão no modo integrado do IIS 7 ou IIS 7,5](#0.1__Toc256770154 "_Toc256770154")  
[Alterações na implementação de CAS (segurança de acesso ao código) do ASP.NET](#0.1__Toc256770155 "_Toc256770155")  
[O MembershipUser e outros tipos no namespace System. Web. Security foram movidos](#0.1__Toc256770156 "_Toc256770156")  
[Alterações de cache de saída para variar \* cabeçalho HTTP](#0.1__Toc256770157 "_Toc256770157")  
[Os tipos System. Web. Security para Passport estão obsoletos](#0.1__Toc256770158 "_Toc256770158")  
[A Propriedade MenuItem. PopOutImageUrl não renderiza uma imagem em ASP.NET 4](#0.1__Toc256770159 "_Toc256770159")  
[Menu. StaticPopOutImageUrl e menu. DynamicPopOutImageUrl falha ao renderizar imagens quando os caminhos contêm barras invertidas](#0.1__Toc256770160 "_Toc256770160")  
[Enção](#0.1__Toc256770161 "_Toc256770161")

<a id="0.1__ControlRenderingCompatibilityVersio"></a><a id="0.1__Toc245724853"></a><a id="0.1__Toc255587630"></a><a id="0.1__Toc256770141"></a>

## <a name="controlrenderingcompatibilityversion-setting-in-the-webconfig-file"></a>Configuração de ControlRenderingCompatibilityVersion no arquivo Web. config

Os controles ASP.NET foram modificados no .NET Framework versão 4 para permitir que você especifique mais precisamente como eles renderizam a marcação. Em versões anteriores do .NET Framework, alguns controles emitiram marcação que não era possível desabilitar. Por padrão, ASP.NET 4 esse tipo de marcação não é mais gerado.

Se você usar o Visual Studio 2010 para atualizar seu aplicativo do ASP.NET 2,0 ou do ASP.NET 3,5, a ferramenta adicionará automaticamente uma configuração ao arquivo de `Web.config` que preserva a renderização herdada. No entanto, se você atualizar um aplicativo alterando o pool de aplicativos no IIS para direcionar o .NET Framework 4, o ASP.NET usará o novo modo de renderização por padrão. Para desabilitar o novo modo de renderização, adicione a seguinte configuração no arquivo de `Web.config`:

[!code-xml[Main](breaking-changes/samples/sample1.xml)]

As alterações de renderização principais que o novo comportamento traz são as seguintes:

- Os controles **Image** e **ImageButton** não renderizam mais um atributo `border="0"`.
- A classe **BaseValidator** e os controles de validação que derivam dele não renderizam mais o texto vermelho por padrão.
- O controle **HtmlForm** não processa um atributo **Name** .
- O controle **tabela** não renderiza mais um atributo `border="0"`.
- Controles que não são projetados para entrada do usuário (por exemplo, o controle **rótulo** ) não renderizam mais o atributo `disabled="disabled"` se sua propriedade **Enabled** é definida como **false** (ou se eles herdam essa configuração de um controle de contêiner).

<a id="0.1__Toc245724854"></a><a id="0.1__Toc255587631"></a><a id="0.1__Toc256770142"></a>

## <a name="clientidmode-changes"></a>Alterações de ClientIDmode

A configuração **ClientIDMode** no ASP.NET 4 permite especificar como o ASP.NET gera o atributo **ID** para elementos HTML. Nas versões anteriores do ASP.NET, o comportamento padrão era equivalente à configuração **AutoID** de **ClientIDMode**. No entanto, a configuração padrão agora é **previsível**.

Se você usar o Visual Studio 2010 para atualizar seu aplicativo do ASP.NET 2,0 ou do ASP.NET 3,5, a ferramenta adicionará automaticamente uma configuração ao arquivo de `Web.config` que preserva o comportamento das versões anteriores do .NET Framework. No entanto, se você atualizar um aplicativo alterando o pool de aplicativos no IIS para direcionar o .NET Framework 4, o ASP.NET usará o novo modo por padrão. Para desabilitar o novo modo de ID de cliente, adicione a seguinte configuração no arquivo de `Web.config`:

[!code-xml[Main](breaking-changes/samples/sample2.xml)]

<a id="0.1__Toc245724855"></a><a id="0.1__Toc255587632"></a><a id="0.1__Toc256770143"></a>

## <a name="htmlencode-and-urlencode-now-encode-single-quotation-marks"></a>HtmlEncode e UrlEncode agora codificam aspas simples

No ASP.NET 4, os métodos **HtmlEncode** e **UrlEncode** das classes **HttpUtility** e **HttpServerUtility** foram atualizados para codificar o caractere aspa simples (') da seguinte maneira:

- O método **HtmlEncode** codifica instâncias de aspa simples como '.
- O método **UrlEncode** codifica instâncias de aspa simples como %27.

<a id="0.1__Toc255587633"></a><a id="0.1__Toc256770144"></a><a id="0.1__Toc245724856"></a>

## <a name="aspnet-page-aspx-parser-is-stricter"></a>O analisador de página ASP.NET (. aspx) é mais estrito

O analisador de página para ASP.NET páginas (arquivos`.aspx`) e controles de usuário (`.ascx` arquivos) é mais estrito no ASP.NET 4 e rejeitará mais instâncias de marcação inválida. Por exemplo, os dois trechos de código a seguir serão analisados com êxito em versões anteriores do ASP.NET, mas agora gerarão erros do analisador no ASP.NET 4.

[!code-aspx[Main](breaking-changes/samples/sample3.aspx)]

Observe o ponto-e-vírgula inválido no final da marca **HiddenField** .

[!code-aspx[Main](breaking-changes/samples/sample4.aspx)]

Observe o atributo de **estilo** não fechado que é executado no atributo **CssClass** .

<a id="0.1__Toc255587634"></a><a id="0.1__Toc256770145"></a>

## <a name="browser-definition-files-updated"></a>Arquivos de definição de navegador atualizados

Os arquivos de definição do navegador foram atualizados para incluir informações sobre dispositivos e navegadores novos e atualizados. Navegadores e dispositivos mais antigos como o Netscape Navigator foram removidos e navegadores e dispositivos mais novos como o Google Chrome e Apple iPhone foram adicionados.

Se seu aplicativo contiver definições do navegador personalizadas herdadas de uma das definições do navegador que foram removidas, você verá um erro. Por exemplo, se a pasta `App_Browsers` contiver uma definição de navegador herdada da definição de navegador IE2, você receberá a seguinte mensagem de erro de configuração:

- Não é possível encontrar o elemento de navegador ou de gateway com a ID ' IE2 '.

> [!NOTE]
> O objeto **HttpBrowserCapabilities** (que é exposto pela propriedade **Request. browser** da página) é orientado pelos arquivos de definições do navegador. Portanto, as informações retornadas por meio do acesso a uma propriedade desse objeto no ASP.NET 4 podem ser diferentes das informações retornadas em uma versão anterior do ASP.NET.

Você pode reverter para os arquivos de definição de navegador antigos copiando os arquivos de definição do navegador da seguinte pasta:

[!code-console[Main](breaking-changes/samples/sample5.cmd)]

Copie os arquivos para a pasta `\CONFIG\Browsers` correspondente para ASP.NET 4. Depois de copiar os arquivos, execute a ferramenta de linha de comando ASPNET\_regbrowsers. exe.

<a id="0.1__Toc255587635"></a><a id="0.1__Toc256770146"></a>

## <a name="systemwebmobiledll-removed-from-root-web-configuration-file"></a>System. Web. Mobile. dll removido do arquivo de configuração da Web raiz

Nas versões anteriores do ASP.NET, uma referência ao assembly System. Web. Mobile. dll foi incluída no arquivo raiz `Web.config` na seção **assemblies** em. Para melhorar o desempenho, a referência a esse assembly foi removida.

O assembly System. Web. Mobile. dll está incluído no ASP.NET 4, mas foi preterido. Se você quiser usar tipos do assembly System. Web. Mobile. dll, adicione uma referência a esse assembly ao arquivo de `Web.config` raiz ou a um arquivo de `Web.config` de aplicativo. Por exemplo, se você quiser usar qualquer um dos controles móveis ASP.NET (preteridos), deverá adicionar uma referência ao assembly System. Web. Mobile. dll no arquivo `Web.config`.

<a id="0.1__Toc245724857"></a><a id="0.1__Toc255587636"></a><a id="0.1__Toc256770147"></a>

## <a name="aspnet-request-validation"></a>Validação de solicitação ASP.NET

O recurso de validação de solicitação no ASP.NET fornece um certo nível de proteção padrão contra ataques XSS (script entre sites). Nas versões anteriores do ASP.NET, a validação de solicitação foi habilitada por padrão. No entanto, ela é aplicada somente a páginas ASP.NET (`.aspx` arquivos e seus arquivos de classe) e somente quando essas páginas estavam em execução.

No ASP.NET 4, por padrão, a validação de solicitação está habilitada para todas as solicitações, porque ela está habilitada antes da fase **BeginRequest** de uma solicitação HTTP. Como resultado, a validação de solicitação se aplica a solicitações para todos os recursos de ASP.NET, não apenas a solicitações de página. aspx. Isso inclui solicitações como chamadas de serviço Web e manipuladores HTTP personalizados. A validação de solicitação também está ativa quando módulos HTTP personalizados estão lendo o conteúdo de uma solicitação HTTP.

Como resultado, os erros de validação de solicitação agora podem ocorrer para solicitações que anteriormente não dispararam erros. Para reverter para o comportamento do recurso de validação de solicitação do ASP.NET 2,0, adicione a seguinte configuração no arquivo de `Web.config`:

[!code-xml[Main](breaking-changes/samples/sample6.xml)]

No entanto, recomendamos que você analise quaisquer erros de validação de solicitação para determinar se os manipuladores, módulos ou outros códigos personalizados podem acessar entradas HTTP potencialmente inseguras que poderiam ser vetores de ataque XSS.

<a id="0.1__Toc245724858"></a><a id="0.1__Toc255587637"></a><a id="0.1__Toc256770148"></a>

## <a name="default-hashing-algorithm-is-now-hmacsha256"></a>O algoritmo de hash padrão agora é HMACSHA256

O ASP.NET usa algoritmos de hash e criptografia para ajudar a proteger dados como cookies de autenticação de formulários e estado de exibição. Por padrão, o ASP.NET 4 agora usa o algoritmo HMACSHA256 para operações de hash em cookies e no estado de exibição. As versões anteriores do ASP.NET usavam o algoritmo HMACSHA1 mais antigo.

Seus aplicativos poderão ser afetados se você executar ambientes mistos ASP.NET 2.0/ASP. NET 4, em que dados como cookies de autenticação de formulários devem funcionar em versões do across.NET Framework. Para configurar um aplicativo Web ASP.NET 4 para usar o algoritmo HMACSHA1 mais antigo, adicione a seguinte configuração no arquivo `Web.config`:

[!code-xml[Main](breaking-changes/samples/sample7.xml)]

<a id="0.1__Toc245724859"></a><a id="0.1__Toc255587638"></a><a id="0.1__Toc256770149"></a>

## <a name="configuration-errors-related-to-new-aspnet-4-root-configuration"></a>Erros de configuração relacionados à nova configuração raiz do ASP.NET 4

Os arquivos de configuração raiz (o arquivo de `machine.config` e o arquivo de `Web.config` raiz) para o .NET Framework 4 (e, portanto, ASP.NET 4) foram atualizados para incluir a maioria das informações de configuração clichê que no ASP.NET 3,5 foi encontrado nos arquivos de `Web.config` de aplicativos. Devido à complexidade dos sistemas de configuração gerenciados do IIS 7 e do IIS 7,5, a execução de aplicativos ASP.NET 3,5 em ASP.NET 4 e no IIS 7 e IIS 7,5 pode resultar em erros de configuração do ASP.NET ou do IIS.

Recomendamos que você atualize os aplicativos ASP.NET 3,5 para o ASP.NET 4 usando as ferramentas de atualização de projeto no Visual Studio 2010, se for prático. O Visual Studio 2010 modifica automaticamente o arquivo de `Web.config` do aplicativo ASP.NET 3,5 para conter as configurações apropriadas para o ASP.NET 4.

No entanto, é um cenário com suporte para executar aplicativos ASP.NET 3,5 usando o .NET Framework 4 sem recompilação. Nesse caso, talvez seja necessário modificar manualmente o arquivo de `Web.config` do aplicativo antes de executar o aplicativo no .NET Framework 4 e no IIS 7 ou no IIS 7,5.

As próximas duas seções descrevem as alterações que talvez você precise fazer para diferentes combinações de software.

**Windows Vista SP1 ou Windows Server 2008 SP1, onde nenhum hotfix KB958854 nem o SP2 está instalado.** Nessa configuração, o sistema de configuração do IIS 7 mescla incorretamente a configuração gerenciada de um aplicativo, comparando o arquivo de `Web.config` de nível de aplicativo com os arquivos de `machine.config` do ASP.NET 2,0. Por isso, os arquivos de `Web.config` no nível de aplicativo do .NET Framework 3,5 ou posterior devem ter uma definição de seção de configuração **System. Web. Extensions** (o elemento) para não causar uma falha na validação do IIS 7.

No entanto, as entradas de arquivo de `Web.config` no nível do aplicativo modificadas manualmente que não correspondem precisamente às definições da seção de configuração do texto clichê original que foram introduzidas com o Visual Studio 2008 causarão erros de configuração do ASP.NET. (As entradas de configuração padrão geradas pelo Visual Studio 2008 funcionam corretamente.) Um problema comum é que os arquivos de `Web.config` modificados manualmente deixam os atributos de configuração **AllowDefinition** e **RequirePermission** encontrados em várias definições de seção de configuração. Isso causa uma incompatibilidade entre a seção de configuração abreviada em arquivos de `Web.config` no nível do aplicativo e a definição completa no arquivo ASP.NET 4 `machine.config`. Como resultado, em tempo de execução, o sistema de configuração do ASP.NET 4 gera um erro de configuração.

**Windows Vista SP2, Windows Server 2008 SP2, Windows 7, Windows Server 2008 R2 e também Windows Vista SP1 e Windows Server 2008 SP1 em que o hotfix KB958854 está instalado.**

Nesse cenário, o sistema de configuração nativa IIS 7 e IIS 7,5 retorna um erro de configuração porque ele executa uma comparação de texto no atributo **Type** que é definido para um manipulador de seção de configuração gerenciada. Como todos os arquivos de `Web.config` gerados pelo Visual Studio 2008 e pelo Visual Studio 2008 SP1 têm "3,5" na cadeia de caracteres de tipo para o sistema. manipuladores de seção de configuração **Web. Extensions** (e relacionados), e como o arquivo ASP.NET 4 `machine.config` tem "4,0" no atributo **Type** para os mesmos manipuladores de seção de configuração, os aplicativos gerados no Visual Studio 2008 ou no Visual Studio 2008 SP1 sempre falham na validação de configuração no IIS 7 e no IIS 7,5.

<a id="0.1__Toc251910248"></a>

### <a name="resolving-these-issues"></a>Resolvendo esses problemas

A solução alternativa para o primeiro cenário é atualizar o arquivo de `Web.config` no nível do aplicativo, incluindo o texto de configuração clichê de um arquivo de `Web.config` que foi gerado automaticamente pelo Visual Studio 2008.

Uma alternativa alternativa para o primeiro cenário é instalar o Service Pack 2 para vista ou o Windows Server 2008 em seu computador ou instalar o hotfix KB958854 ([https://support.microsoft.com/kb/958854](https://support.microsoft.com/kb/958854)) para corrigir o comportamento incorreto da mesclagem de configuração do sistema de configuração do IIS. No entanto, depois de executar qualquer uma dessas ações, o aplicativo provavelmente encontrará um erro de configuração devido ao problema descrito para o segundo cenário.

A solução alternativa para o segundo cenário é excluir ou comentar todas as definições de seção de configuração de **System. Web. Extensions** e definições de grupos de seções de configuração do arquivo de `Web.config` no nível do aplicativo. Essas definições geralmente estão na parte superior do arquivo de `Web.config` no nível do aplicativo e podem ser identificadas pelo elemento **configSections** e seus filhos.

Para ambos os cenários, é recomendável que você também exclua manualmente a seção **System. CodeDom** , embora isso não seja necessário.

<a id="0.1__Toc252995490"></a><a id="0.1__Toc255587639"></a><a id="0.1__Toc256770150"></a><a id="0.1__Toc245724860"></a>

## <a name="aspnet-4-child-applications-fail-to-start-when-under-aspnet-20-or-aspnet-35-applications"></a>Aplicativos filho ASP.NET 4 falham ao iniciar quando em aplicativos ASP.NET 2,0 ou ASP.NET 3,5

Os aplicativos ASP.NET 4 configurados como filhos de aplicativos que executam versões anteriores do ASP.NET talvez falhem ao iniciar devido a erros de configuração ou de compilação. O exemplo a seguir mostra uma estrutura de diretório para um aplicativo afetado.

`/parentwebapp` (configurado para usar ASP.NET 2,0 ou ASP.NET 3,5)  
`/childwebapp` (configurado para usar o ASP.NET 4)

O aplicativo na pasta `childwebapp` não será iniciado no IIS 7 ou no IIS 7,5 e relatará um erro de configuração. O texto do erro incluirá uma mensagem semelhante à seguinte:

- `The requested page cannot be accessed because the related configuration data for the page is invalid.`

- `The configuration section 'configSections' cannot be read because it is missing a section declaration.`

No IIS 6, o aplicativo na pasta `childwebapp` também falhará ao iniciar, mas ele relatará um erro diferente. Por exemplo, o texto de erro pode indicar o seguinte:

- `The value for the 'compilerVersion' attribute in the provider options must be 'v4.0' or later if you are compiling for version 4.0 or later of the .NET Framework. To compile this Web application for version 3.5 or earlier of the .NET Framework, remove the 'targetFramework' attribute from the element of the Web.config file`

Esses cenários ocorrem porque as informações de configuração do aplicativo pai na pasta `parentwebapp` fazem parte da hierarquia de informações de configuração que determinam os parâmetros de configuração mesclados finais usados pelo aplicativo Web filho na pasta `childwebapp`. Dependendo se o aplicativo Web ASP.NET 4 está em execução no IIS 7 (ou IIS 7,5) ou no IIS 6, o sistema de configuração do IIS ou o sistema de compilação ASP.NET 4 retornará um erro.

As etapas que você deve seguir para resolver esse problema e obter o aplicativo filho ASP.NET 4 para trabalhar dependem de o aplicativo ASP.NET 4 ser executado no IIS 6 ou no IIS 7 (ou no IIS 7,5).

### <a name="step-1-iis-7-or-iis-75-only"></a>Etapa 1 (somente IIS 7 ou IIS 7,5)

Esta etapa é necessária apenas em sistemas operacionais que executam o IIS 7 ou o IIS 7,5, que inclui o Windows Vista, o Windows Server 2008, o Windows 7 e o Windows Server 2008 R2.

Mova a definição de **configSections** no arquivo de `Web.config` do aplicativo pai (o aplicativo que executa ASP.NET 2,0 ou ASP.net 3,5) para o arquivo de `Web.config` raiz do the.NET Framework 2,0. O sistema de configuração nativo do IIS 7 e IIS 7,5 examina o elemento **configSections** quando mescla a hierarquia dos arquivos de configuração. Mover a definição de **configSections** do arquivo de `Web.config` do aplicativo Web pai para o arquivo de `Web.config` raiz oculta efetivamente o elemento do processo de mesclagem de configuração que ocorre para o aplicativo filho ASP.NET 4.

Em um sistema operacional de 32 bits ou em pools de aplicativos de 32 bits, o arquivo de `Web.config` raiz para ASP.NET 2,0 e ASP.NET 3,5 está localizado normalmente na seguinte pasta:

`C:\Windows\Microsoft.NET\Framework\v2.0.50727\CONFIG`

Em um sistema operacional de 64 bits ou em pools de aplicativos de 64 bits, o arquivo de `Web.config` raiz para ASP.NET 2,0 e ASP.NET 3,5 está localizado normalmente na seguinte pasta:

`C:\Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG`

Se você executar aplicativos Web de 32 bits e 64 bits em um computador de 64 bits, deverá mover o elemento **configSections** para cima até os arquivos de `Web.config` raiz para os sistemas de 32 bits e de 64 bits.

Ao colocar o elemento **configSections** no arquivo de `Web.config` raiz, Cole a seção imediatamente após o elemento de **configuração** . O exemplo a seguir mostra qual será a aparência da parte superior do arquivo de `Web.config` raiz quando você terminar de mover os elementos.

> [!NOTE]
> No exemplo a seguir, as linhas foram encapsuladas para facilitar a leitura.

[!code-xml[Main](breaking-changes/samples/sample8.xml)]

### <a name="step-2-all-versions-of-iis"></a>Etapa 2 (todas as versões do IIS)

Esta etapa é necessária se o aplicativo Web filho ASP.NET 4 estiver em execução no IIS 6 ou no IIS 7 (ou no IIS 7,5).

No arquivo de `Web.config` do aplicativo Web pai que está executando ASP.NET 2 ou ASP.NET 3,5, adicione uma marca de **local** que especifica explicitamente (para os sistemas de configuração IIS e ASP.net) que as entradas de configuração se aplicam somente ao aplicativo Web pai. O exemplo a seguir mostra a sintaxe do elemento **Location** a ser adicionado:

[!code-xml[Main](breaking-changes/samples/sample9.xml)]

O exemplo a seguir mostra como a marca **Location** é usada para encapsular todas as seções de configuração começando com a seção **appSettings** e terminando com a seção **System. WebServer** .

[!code-xml[Main](breaking-changes/samples/sample10.xml)]

Quando você tiver concluído as etapas 1 e 2, os aplicativos da Web filho ASP.NET 4 serão iniciados sem erros.

<a id="0.1__Toc252995491"></a><a id="0.1__Toc255587640"></a><a id="0.1__Toc256770151"></a>

## <a name="aspnet-4-web-sites-fail-to-start-on-computers-where-sharepoint-is-installed"></a>Os sites ASP.NET 4 falham ao iniciar em computadores em que o SharePoint está instalado

Os servidores Web que executam o SharePoint têm um arquivo `Web.config` que é implantado na raiz de um site do SharePoint (por exemplo, `c:\inetpub\wwwroot\web.config` para site padrão da Web). Neste `Web.config` arquivo, o SharePoint define um nível de confiança parcial personalizado chamado WSS\_mínimo.

Se você tentar executar um site do ASP.NET 4 que é implantado como um filho desse tipo de site do SharePoint, verá o seguinte erro:

`Could not find permission set named 'ASP.NET'.`

Esse erro ocorre porque a infraestrutura de CAS (segurança de acesso ao código) do ASP.NET 4 procura um conjunto de permissões chamado ASP.NET. No entanto, o arquivo de configuração de confiança parcial que é referenciado pelo WSS\_mínimo não contém nenhum conjunto de permissões com esse nome.

Atualmente, não há uma versão do SharePoint disponível que seja compatível com o ASP.NET. Como resultado, você não deve tentar executar um site da Web ASP.NET 4 como um site filho sob sites do SharePoint.

<a id="0.1__Toc255587641"></a><a id="0.1__Toc256770152"></a>

## <a name="the-httprequestfilepath-property-no-longer-includes-pathinfo-values"></a>A Propriedade HttpRequest. FilePath não inclui mais valores PathInfo

As versões anteriores do ASP.NET incluíam um valor de **PathInfo** no valor retornado de várias propriedades relacionadas a caminho de arquivo, incluindo **HttpRequest. FilePath**, **HttpRequest. AppRelativeCurrentExecutionFilePath**e **HttpRequest. CurrentExecutionFilePath**. O ASP.NET 4 não inclui mais o valor de **PathInfo** nos valores de retorno dessas propriedades. Em vez disso, as informações de **PathInfo** estão disponíveis em **HttpRequest. PathInfo**. Por exemplo, imagine o fragmento da URL a seguir:

`/testapp/Action.mvc/SomeAction`

Nas versões anteriores do ASP.NET, as propriedades **HttpRequest** têm os seguintes valores:

**HttpRequest. FilePath**: `/testapp/Action.mvc/SomeAction`

**HttpRequest. PathInfo**: (vazio)

Nas propriedades ASP.NET 4, **HttpRequest** , em vez disso, tem os seguintes valores:

**HttpRequest. FilePath**: `/testapp/Action.mvc`

**HttpRequest. PathInfo**: `SomeAction`

<a id="0.1__Toc252995493"></a><a id="0.1__Toc255587642"></a><a id="0.1__Toc256770153"></a><a id="0.1__Toc245724861"></a>

## <a name="aspnet-20-applications-might-generate-httpexception-errors-that-reference-eurlaxd"></a>Os aplicativos ASP.NET 2,0 podem gerar erros HttpException que referenciam eurl. axd

Depois que o ASP.NET 4 tiver sido habilitado no IIS 6, os aplicativos ASP.NET 2.0 executados no IIS 6 (no Windows Server 2003 ou Windows Server 2003 R2) poderão gerar erros como o seguinte:

`System.Web.HttpException: Path '/[yourApplicationRoot]/eurl.axd/[Value]' was not found.`

Esse erro ocorre porque quando o ASP.NET detecta que um site está configurado para usar o ASP.NET 4, um componente nativo do ASP.NET 4 passa uma URL de extensão para a parte gerenciada de ASP.NET para processamento adicional. No entanto, se os diretórios virtuais que estão abaixo de um site do ASP.NET 4 estiverem configurados para usar o ASP.NET 2,0, o processamento da URL sem extensão dessa forma resultará em uma URL modificada que contém a cadeia de caracteres "eurl. axd". Essa URL modificada é enviada para o aplicativo ASP.NET 2,0. O ASP.NET 2,0 não reconhece o formato "eurl. axd". Portanto, ASP.NET 2,0 tenta encontrar um arquivo chamado `eurl.axd` e executá-lo. Como esse arquivo não existe, a solicitação falha com uma exceção **HttpException** .

Você pode contornar esse problema usando uma das opções a seguir.

### <a name="option-1"></a>Opção 1

Se ASP.NET 4 não for necessário para executar o site, remapeie o site para usar o ASP.NET 2,0 em vez disso.

### <a name="option-2"></a>Opção 2

Se ASP.NET 4 for necessário para executar o site, mova quaisquer diretórios virtuais ASP.NET 2,0 filho para um site diferente que esteja mapeado para ASP.NET 2,0.

### <a name="option-3"></a>Opção 3

Se não for prático remapear o site para ASP.NET 2,0 ou alterar o local de um diretório virtual, desabilite explicitamente o processamento de URL sem extensão no ASP.NET 4. Use este procedimento:

1. No registro do Windows, abra o seguinte nó:

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ASP.NET\4.0.30319.0`

1. Crie um novo valor **DWORD** chamado **EnableExtensionlessUrls**.
2. Defina **EnableExtensionlessUrls** como 0. Isso desabilita o comportamento da URL de extensão.
3. Salve o valor do registro e feche o editor do registro.
4. Execute a ferramenta de linha de comando **iisreset** , que faz com que o IIS Leia o novo valor de registro.

> [!NOTE]
> A definição de **EnableExtensionlessUrls** como 1 habilita o comportamento de URL de extensão. Essa será a configuração padrão se nenhum valor for especificado.

<a id="0.1__Toc252995494"></a><a id="0.1__Toc255587643"></a><a id="0.1__Toc256770154"></a><a id="0.1__Toc245724862"></a>

## <a name="event-handlers-might-not-be-not-raised-in-a-default-document-in-iis-7-or-iis-75-integrated-mode"></a>Manipuladores de eventos podem não ser gerados em um documento padrão no modo integrado do IIS 7 ou IIS 7,5

O ASP.NET 4 inclui modificações que alteram como o atributo **Action** do elemento HTML **Form** é RENDERIZAdo quando uma URL sem extensão é resolvida para um documento padrão. Um exemplo de uma URL com extensão para resolver um documento padrão seria [http://contoso.com/](http://contoso.com/), resultando em uma solicitação para [http://contoso.com/Default.aspx](http://contoso.com/Default.aspx).

O ASP.NET 4 agora renderiza o valor do atributo de **ação** do elemento de **formulário** HTML como uma cadeia de caracteres vazia quando uma solicitação é feita a uma URL de extensão que tem um documento padrão mapeado para ele. Por exemplo, em versões anteriores do ASP.NET, uma solicitação para [http://contoso.com](http://contoso.com) resultaria em uma solicitação para `Default.aspx`. Nesse documento, a marca de **formulário** de abertura seria renderizada como no exemplo a seguir:

`<form action="Default.aspx" />`

No ASP.NET 4, uma solicitação para [http://contoso.com](http://contoso.com) também resulta em uma solicitação para `Default.aspx`. No entanto, ASP.NET agora renderiza a marca HTML de abertura do **formulário** como no exemplo a seguir:

`<form action="" />`

Essa diferença na forma como o atributo de **ação** é renderizado pode causar alterações sutis em como uma postagem de formulário é processada pelo IIS e ASP.net. Quando o atributo **Action** for uma cadeia de caracteres vazia, o objeto IIS **DefaultDocumentModule** criará uma solicitação filho para `Default.aspx`. Na maioria das condições, essa solicitação filho é transparente para o código do aplicativo e a página de `Default.aspx` é executada normalmente.

No entanto, uma eventual interação entre código gerenciado e o modo integrado do IIS 7 ou IIS 7.5 pode fazer páginas .aspx gerenciadas pararem de funcionar adequadamente durante a solicitação filho. Se as condições a seguir ocorrerem, a solicitação filho para um documento `Default.aspx` resultará em um erro ou em um comportamento inesperado:

1. Uma página. aspx é enviada ao navegador com o atributo **Action** do elemento **Form** definido como "".
2. O formulário é Postado de volta para ASP.NET.
3. Um módulo HTTP gerenciado lê parte do corpo da entidade. Por exemplo, um módulo lê **solicitação. Form** ou **Request. params**. Isso faz o corpo da entidade da solicitação POST ser lido na memória gerenciada. Como resultado, o corpo da entidade não está mais disponível para quaisquer módulos de código nativo em execução no modo integrado do IIS 7 ou do IIS 7.5.
4. O objeto IIS **DefaultDocumentModule** eventualmente é executado e cria uma solicitação filho para o documento `Default.aspx`. No entanto, como o corpo da entidade já foi lido por um trecho de código gerenciado, não há nenhum corpo de entidade disponível para enviar a solicitação filho.
5. Quando o pipeline HTTP é executado para a solicitação filho, o manipulador para `.aspx` arquivos é executado durante a fase de execução do manipulador.
6. Como não há nenhum corpo de entidade, não há nenhuma variável de formulário e nenhum estado de exibição e, portanto, nenhuma informação está disponível para o manipulador de página. aspx para determinar qual evento (se houver) deve ser gerado. Como resultado, nenhum manipulador de eventos de postback para a página .aspx afetada é executado.

Você pode contornar esse comportamento das seguintes maneiras:

- Identifique o módulo HTTP que está acessando o corpo da entidade da solicitação durante as solicitações de documento padrão e determine se ele pode ser configurado para ser executado somente para solicitações gerenciadas. No modo integrado para IIS 7 e IIS 7,5, os módulos HTTP podem ser marcados para execução somente para solicitações gerenciadas adicionando o seguinte atributo à entrada **System. NetServer/modules** do módulo:

- `precondition="managedHandler"`

- Essa configuração desabilita o módulo para solicitações que o IIS 7 e o IIS 7,5 determinam como não sendo solicitações gerenciadas. Para solicitações de documento padrão, a primeira solicitação é para uma URL de extensão. Portanto, o IIS não executa nenhum módulo gerenciado que esteja marcado com uma pré-condição do manipulador gerenciado durante o processamento de solicitação inicial. Como resultado, os módulos gerenciados não lêem acidentalmente o corpo da entidade e, portanto, o corpo da entidade ainda está disponível e é passado para a solicitação filho e para o documento padrão.

- Se os módulos HTTP problemáticos precisarem ser executados para todas as solicitações (para arquivos estáticos, para URLs sem extensão que resolvem para o objeto **DefaultDocumentModule** , para solicitações gerenciadas etc.), modifique as páginas. aspx afetadas definindo explicitamente a propriedade **Action** do controle **System. Web. UI. HtmlControls. HtmlForm** para uma cadeia de caracteres não vazia. Por exemplo, se o documento padrão for `Default.aspx`, modifique o código da página para definir explicitamente a propriedade **Action** do controle **HtmlForm** como "default. aspx".

<a id="0.1__Toc255587644"></a><a id="0.1__Toc256770155"></a>

## <a name="changes-to-the-aspnet-code-access-security-cas-implementation"></a>Alterações na implementação de CAS (segurança de acesso ao código) do ASP.NET

ASP.NET 2,0 e, por extensão, os recursos do ASP.NET que foram adicionados em 3,5, use o modelo de CAS .NET Framework 1,1 e 2,0 (segurança de acesso a código). No entanto, a implementação da CAS no ASP.NET 4 foi substancialmente revisada. Como resultado, os aplicativos ASP.NET de confiança parcial que dependem de código confiável em execução no GAC (cache de assembly global) podem falhar com várias exceções de segurança. Os aplicativos de confiança parcial que dependem de modificações extensivas na política de CAS do computador também podem falhar com exceções de segurança.

Você pode reverter os aplicativos de confiança parcial ASP.NET 4 para o comportamento de ASP.NET 1,1 e 2,0 usando o novo atributo **LegacyCasModel** no elemento de configuração de **confiança** , conforme mostrado no exemplo a seguir:

`<trust level= "Medium" legacyCasModel="true" />`

Quando você reverte para o modelo de CAS herdado, os seguintes comportamentos de CAS antigos são habilitados:

- A política de CAS do computador é respeitada.
- Vários conjuntos de permissões diferentes em um único domínio de aplicativo são permitidos.
- As declarações de permissão explícitas não são necessárias para assemblies no GAC que são invocados quando apenas ASP.NET ou outro código de .NET Framework está na pilha.

Um cenário não pode ser revertido no .NET Framework 4: aplicativos de confiança parcial não Web não podem mais chamar determinadas APIs em System. Web. dll e System. Web. Extensions. dll. Nas versões anteriores do .NET Framework, era possível que aplicativos de confiança parcial não Web recebam explicitamente permissões de **AspNetHostingPermission** . Esses aplicativos poderiam, então, usar **System. Web. HttpUtility**, tipos nos namespaces **System. Web. clienteservices.\*** e tipos relacionados à associação, às funções e aos perfis. A chamada a esses tipos de aplicativos de confiança parcial não Web não tem mais suporte no .NET Framework 4.

> [!NOTE]
> A funcionalidade **HtmlEncode** e **HtmlDecode** da classe **System. Web. HttpUtility** foi movida para a nova classe .NET Framework 4 do **sistema .net. WebUtility** . Se essa fosse a única funcionalidade de ASP.NET que estava sendo usada, modifique o código do aplicativo para usar a nova classe **WebUtility** em vez disso.

Veja a seguir um resumo de alto nível das alterações na implementação de CAS padrão no ASP.NET 4:

- Os domínios de aplicativo ASP.NET agora são domínios de aplicativo homogêneos. Somente os conjuntos de concessão de confiança parcial e de confiança total estão disponíveis em um domínio de aplicativo.
- Os conjuntos de concessão de confiança parcial ASP.NET são independentes de qualquer política de CAS de nível de computador ou de nível empresarial.
- Os assemblies do ASP.NET fornecidos em 3,5 e 3,5 SP1 foram convertidos para usar o modelo de transparência .NET Framework 4.
- O uso do atributo ASP.NET **AspNetHostingPermission** foi substancialmente reduzido. A maioria das instâncias deste atributo foi removida das APIs públicas do ASP.NET.
- Os assemblies compilados dinamicamente que são criados por provedores de compilação ASP.NET foram atualizados para marcar explicitamente os assemblies como Transparent.
- Todos os assemblies ASP.NET agora são marcados de forma que o atributo APTCA seja respeitado somente em ambientes de hospedagem na Web. Ambientes de hospedagem não Web parcialmente confiáveis, como o ClickOnce, não poderão chamar assemblies ASP.NET.

Para obter mais informações sobre o novo modelo de segurança de acesso ao código do ASP.NET 4, consulte [usando a segurança de acesso ao código em aplicativos ASP.net](https://msdn.microsoft.com/library/dd984947%28VS.100%29.aspx) no site do MSDN.

<a id="0.1__Toc256770156"></a><a id="0.1__Toc245724863"></a><a id="0.1__Toc252995496"></a><a id="0.1__Toc255587645"></a><a id="0.1__Toc245724864"></a>

## <a name="membershipuser-and-other-types-in-the-systemwebsecurity-namespace-have-been-moved"></a>O MembershipUser e outros tipos no namespace System. Web. Security foram movidos

Alguns tipos que são usados na associação ASP.NET foram movidos de `System.Web.dll` para o novo assembly System. Web. ApplicationServices. dll. Os tipos foram movidos para resolver as dependências de camadas de arquitetura entre os tipos no cliente e nos SKUs do .NET Framework estendido.

Os projetos de site da Web não têm problemas como resultado da movimentação desses tipos, porque System. Web. ApplicationServices. dll foi adicionado à lista de assemblies referenciados que é usada por padrão pelo sistema de compilação ASP.NET. Se você atualizar um projeto de site da Web que foi criado usando uma versão anterior do ASP.NET para o ASP.NET 4 abrindo-o no Visual Studio 2010, o projeto será compilado sem erros.

Da mesma forma, se você atualizar um projeto de aplicativo Web que foi criado em uma versão anterior do ASP.NET para o ASP.NET 4 abrindo-o no Visual Studio 2010, o processo de atualização adicionará uma referência a System. Web. ApplicationServices. dll ao projeto. Portanto, os projetos de aplicativos Web atualizados também serão compilados sem erros.

Arquivos compilados (binários) que foram criados usando versões anteriores do ASP.NET também serão executados sem erros no ASP.NET 4, mesmo que os tipos de associação tenham sido movidos para um assembly diferente. As informações de encaminhamento de tipo foram adicionadas à versão ASP.NET 4 do `System.Web.dll` que roteia automaticamente as referências de tempo de execução para esses tipos para o novo local para os tipos.

No entanto, as bibliotecas de classes que usam tipos de associação específicos e que foram atualizadas de versões anteriores do ASP.NET não serão compiladas quando usadas em um projeto ASP.NET 4. Por exemplo, um projeto de biblioteca de classes pode falhar ao compilar e relatar um erro, como o seguinte:

- `The type 'System.Web.Security.MembershipUser' is defined in an assembly that is not referenced. You must add a reference to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`

- `The type name 'MembershipUser' could not be found. This type has been forwarded to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'. Consider adding a reference to that assembly.`

Você pode contornar esse problema adicionando uma referência em seu projeto de biblioteca de classes a System. Web. ApplicationServices. dll.

A lista a seguir mostra os tipos *System. Web. Security* que foram movidos de `System.Web.dll` para System. Web. ApplicationServices. dll:

- *System. Web. Security. MembershipCreateStatus*
- *System. Web. Security. Membership. createuserexception*
- *System. Web. Security. MembershipPasswordException*
- *System. Web. Security. MembershipPasswordFormat*
- *System. Web. Security. MembershipProvider*
- *System. Web. Security. MembershipProvidercollection*
- *System. Web. Security. MembershipUser*
- *System. Web. Security. MembershipUserCollection*
- *System. Web. Security. MembershipValidatePasswordEventHandler*
- *System. Web. Security. ValidatePasswordEventArgs*
- *System. Web. Security. RoleProvider*
- <a id="0.1_a"></a>*System. Web. Configuration. MembershipPasswordCompatibilityMode*

<a id="0.1__Toc256770157"></a>

## <a name="output-caching-changes-to-vary--http-header"></a>Alterações de cache de saída para variar \* cabeçalho HTTP

No ASP.NET 1,0, um bug causou páginas em cache que especificavam `Location="ServerAndClient"` como uma configuração de cache de saída para emitir um cabeçalho HTTP `Vary:*` na resposta. O efeito disso era informar os navegadores de clientes a nunca armazenar a página em cache localmente.

No ASP.NET 1,1, o método **System. Web. HttpCachePolicy. SetOmitVaryStar** foi adicionado, que você poderia chamar para suprimir o cabeçalho `Vary:*`. Esse método foi escolhido porque a alteração do cabeçalho HTTP emitido foi considerada uma alteração potencialmente significativa no momento. No entanto, os desenvolvedores têm sido confundidos pelo comportamento no ASP.NET, e os relatórios de bugs sugerem que os desenvolvedores não sabem do comportamento de **SetOmitVaryStar** existente.

No ASP.NET 4, a decisão foi tomada para corrigir o problema raiz. O cabeçalho HTTP `Vary:*` não é mais emitido de respostas que especificam a seguinte diretiva:

`<%@OutputCache Location="ServerAndClient" %>`

Como resultado, o **SetOmitVaryStar** não é mais necessário para suprimir o cabeçalho `Vary:*`.

Em aplicativos que especificam `Location="ServerAndClient"` na diretiva **@ OutputCache** em uma página, agora você verá o comportamento implícito pelo nome do valor do atributo de **local** – ou seja, as páginas poderão ser armazenadas em cache no navegador sem exigir que você chame o método **SetOmitVaryStar** .

Se as páginas em seu aplicativo devem emitir `Vary:*`, chame o método **AppendHeader** , como no exemplo a seguir:

`HttpResponse.AppendHeader("Vary","*");`

Como alternativa, você pode alterar o valor do atributo de **local** do cache de saída para "Server".

<a id="0.1__Toc255587646"></a><a id="0.1__Toc256770158"></a>

## <a name="systemwebsecurity-types-for-passport-are-obsolete"></a>Os tipos System. Web. Security para Passport estão obsoletos

O suporte do Passport integrado ao ASP.NET 2,0 foi obsoleto e não há suporte por alguns anos devido a alterações no Passport (agora LiveID). Como resultado, os cinco tipos relacionados ao Passport em **System. Web. Security** agora são marcados com o atributo **ObsoleteAttribute** .

<a id="0.1__The_MenuItem.PopOutImageUrl_Propert"></a><a id="0.1__Toc256770159"></a>

## <a name="the-menuitempopoutimageurl-property-fails-to-render-an-image-in-aspnet-4"></a>A Propriedade MenuItem. PopOutImageUrl não renderiza uma imagem em ASP.NET 4

No ASP.NET 3,5, a propriedade *MenuItem. PopOutImageUrl* permite especificar a URL de uma imagem exibida em um item de menu para indicar que o item de menu tem um submenu dinâmico. O exemplo a seguir mostra como especificar essa propriedade na marcação no ASP.NET 3,5.

[!code-aspx[Main](breaking-changes/samples/sample11.aspx)]

Como resultado de uma alteração de design no ASP.NET 4, nenhuma saída será renderizada para o *PopOutImageUrl* se a propriedade for definida para a classe *MenuItem* . Em vez disso, você deve especificar uma URL de imagem diretamente no controle de *menu* usando a propriedade *StaticPopOutImageUrl* ou a propriedade *DynamicPopOutImageUrl* . Quando você trabalha com um menu estático, a propriedade *menu. StaticPopOutImageUrl* especifica a URL de uma imagem que é exibida para indicar que o item de menu estático tem um submenu, como mostrado no exemplo a seguir:

[!code-aspx[Main](breaking-changes/samples/sample12.aspx)]

Se você estiver trabalhando com um menu dinâmico, use a propriedade *menu. DynamicPopOutImageUrl* para especificar a URL de uma imagem que indica que um item de menu dinâmico tem um submenu. O exemplo a seguir é semelhante ao anterior, mas mostra como definir a propriedade *DynamicPopOutImageUrl* para um menu dinâmico.

[!code-aspx[Main](breaking-changes/samples/sample13.aspx)]

Se a propriedade *menu. DynamicPopOutImageUrl* não estiver definida e a propriedade *menu. DynamicEnableDefaultPopOutImage* for definida como *false*, nenhuma imagem será exibida. Da mesma forma, se a propriedade *StaticPopOutImageUrl* não estiver definida e a propriedade *StaticEnableDefaultPopOutImage* for definida como *false*, nenhuma imagem será exibida.

Ao definir os caminhos para essas propriedades, use uma barra (/) em vez de uma barra invertida (\). Para obter mais informações, consulte [menu. StaticPopOutImageUrl e menu. DynamicPopOutImageUrl falha ao renderizar imagens quando os caminhos contêm barras invertidas](#0.1__Menu.StaticPopOutImageUrl_and_Menu. "_Menu. StaticPopOutImageUrl_and_Menu.") em outro lugar neste documento.

<a id="0.1__Menu.StaticPopOutImageUrl_and_Menu."></a><a id="0.1__Toc256770160"></a>

## <a name="menustaticpopoutimageurl-and-menudynamicpopoutimageurl-fail-to-render-images-when-paths-contain-backslashes"></a>Menu. StaticPopOutImageUrl e menu. DynamicPopOutImageUrl falha ao renderizar imagens quando os caminhos contêm barras invertidas

No ASP.NET 4, as imagens que você especificar usando as propriedades *menu. StaticPopOutImageUrl* e *menu. DynamicPopOutImageUrl* não serão renderizadas se o caminho contiver backlashes (\). Essa é uma alteração de versões anteriores do ASP.NET.

O exemplo a seguir de marcação de controle de *menu* mostra o conjunto de propriedades *StaticPopOutImageUrl* usando um caminho que contém uma barra invertida. No ASP.NET 4, a imagem especificada na propriedade não será renderizada.

[!code-aspx[Main](breaking-changes/samples/sample14.aspx)]

Para corrigir esse problema, altere os valores de caminho que são especificados nas propriedades *StaticPopOutImageUrl* e *DynamicPopOutImageUrl* para usar barras "/". O exemplo a seguir mostra essa alteração:

[!code-aspx[Main](breaking-changes/samples/sample15.aspx)]

Observe que os aplicativos que foram migrados de versões anteriores do ASP.NET para o ASP.NET 4 também podem ser afetados, pois a propriedade *MenuItem. PopOutImageUrl* foi alterada. Para obter mais informações, consulte [a Propriedade MenuItem. PopOutImageUrl falha ao renderizar uma imagem no ASP.NET 4](#0.1__The_MenuItem.PopOutImageUrl_Propert "_The_MenuItem. PopOutImageUrl_Propert") em outro lugar neste documento.

<a id="0.1__Toc224729061"></a><a id="0.1__Toc255587647"></a><a id="0.1__Toc256770161"></a>

## <a name="disclaimer"></a>Isenção de responsabilidade

Este é um documento preliminar e pode ser alterado substancialmente antes da versão comercial final do software descrito aqui.

As informações contidas neste documento representam a visão atual da Microsoft Corporation acerca das questões discutidas até a data da publicação. Como a Microsoft deve reagir às dinâmicas condições do mercado, essas informações não devem ser interpretadas como um compromisso por parte da Microsoft e a Microsoft não garante a precisão de qualquer informação apresentada após a data de publicação.

Este white paper é fornecido apenas para fins informativos. A MICROSOFT NÃO OFERECE NENHUMA GARANTIA EXPRESSA, IMPLÍCITA OU ESTATUTÁRIA EM RELAÇÃO ÀS INFORMAÇÕES CONTIDAS NESTE DOCUMENTO.

Obedecer a todas as leis de direitos autorais aplicáveis é responsabilidade do usuário. Sem limitar os direitos autorais, nenhuma parte deste documento pode ser reproduzida, armazenada ou introduzida em um sistema de recuperação, ou transmitida de qualquer forma ou por qualquer meio (seja eletrônico, mecânico, fotocópia, gravação ou outro), ou para qualquer finalidade, sem a permissão expressa e por escrito da Microsoft Corporation.

A Microsoft pode ter patentes ou requisições para obtenção de patente, marcas comerciais, direitos autorais ou outros direitos de propriedade intelectual que abrangem o conteúdo deste documento. A posse deste documento não lhe confere nenhum direito sobre patentes, marcas comerciais, direitos autorais ou outros direitos de propriedade intelectual, salvo aqueles expressamente mencionados em um contrato de licença, por escrito, da Microsoft.

Salvo indicação em contrário, os exemplos de empresas, organizações, produtos, nomes de domínio, endereços de email, logotipos, pessoas, lugares e eventos aqui mencionados são fictícios e nenhuma associação com qualquer empresa, organização, produto, nome de domínio, emails reais Endereço, logotipo, pessoa, local ou evento é intencional ou deve ser inferido.

© 2010 Microsoft Corporation. Todos os direitos reservados.

Microsoft e Windows são marcas registradas ou comerciais da Microsoft Corporation nos Estados Unidos e/ou em outros países.

Os nomes de empresas e produtos reais mencionados aqui podem ser marcas comerciais de seus respectivos proprietários.
