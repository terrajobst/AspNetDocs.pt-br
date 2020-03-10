---
uid: web-pages/overview/getting-started/aspnet-web-pages-razor-faq
title: FAQ do Páginas da Web do ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Este artigo lista algumas perguntas frequentes sobre Páginas da Web do ASP.NET (Razor) e WebMatrix. Versões de software usadas no tutorial Páginas da Web do ASP.NET (R...
ms.author: riande
ms.date: 02/07/2014
ms.assetid: b137bd04-25e1-47cb-9d96-ef2e179ecf1f
msc.legacyurl: /web-pages/overview/getting-started/aspnet-web-pages-razor-faq
msc.type: authoredcontent
ms.openlocfilehash: 6fa1b668e915af856a693e79eafb7180ed504dc2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78640927"
---
# <a name="aspnet-web-pages-razor-faq"></a>Perguntas frequentes sobre Páginas da Web do ASP.NET (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > O WebMatrix não é mais recomendado como um ambiente de desenvolvimento integrado para Páginas da Web do ASP.NET. Use o [Visual Studio](xref:aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio) ou o [Visual Studio Code](https://code.visualstudio.com/).
>
> Este artigo lista algumas perguntas frequentes sobre Páginas da Web do ASP.NET (Razor) e WebMatrix.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 3
> - Visual Studio 2013
> - WebMatrix 3
>   
> 
> Este tutorial também funciona com o Páginas da Web do ASP.NET 2, o WebMatrix 2 e o Visual Studio 2012.

- [Qual é a diferença entre Páginas da Web do ASP.NET, ASP.NET Web Forms e MVC do ASP.NET?](#Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC)
- [Preciso do WebMatrix para trabalhar com páginas da Web?](#Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages)
- [Posso usar controles de Web Forms ASP.NET em uma página de páginas da Web?](#Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page)
- [Posso implantar um site Páginas da Web do ASP.NET sem usar o WebMatrix?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix)
- [É necessário usar o auxiliar do websecurity para dar suporte a logons?](#Do_I_have_to_use_the_WebSecurity_helper_to_support_logins)
- [Páginas da Web do ASP.NET dá suporte a HTML5?](#Does_ASP.NET_Web_Pages_support_HTML5)
- [Posso usar JavaScript e jQuery com páginas da Web?](#Can_I_use_JavaScript_and_jQuery_with_Web_Pages)
- [Recursos adicionais](#AdditionalResources)

Para dúvidas sobre erros e outros problemas, consulte o [Guia de solução de problemas do páginas da Web do ASP.net (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001).

<a id="Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC"></a>
## <a name="whats-the-difference-between-aspnet-web-pages-aspnet-web-forms-and-aspnet-mvc"></a>Qual é a diferença entre Páginas da Web do ASP.NET, ASP.NET Web Forms e MVC do ASP.NET?

Todas as três são tecnologias ASP.NET para a criação de aplicativos Web dinâmicos:

- Páginas da Web do ASP.NET concentra-se na adição de código dinâmico (lado do servidor) e acesso ao banco de dados a páginas HTML e apresenta uma sintaxe simples e leve.
- ASP.NET Web Forms é baseado em um modelo de objeto de página e em controles de tipo de janela tradicionais (botões, listas, etc.). O Web Forms usa um modelo baseado em evento que é familiar para aqueles que trabalharam com o desenvolvimento com base no cliente (Windows Forms).
- O ASP.NET MVC implementa o padrão Model-View-Controller para ASP.NET. A ênfase é "separação de preocupações" (processamento, dados e camadas de interface do usuário).

Todas as três estruturas têm suporte total e continuam sendo desenvolvidas pela equipe do ASP.NET. Em geral, a escolha da estrutura a ser usada depende do seu plano de fundo e da sua experiência com o ASP.NET.

Páginas da Web do ASP.NET em particular foi projetado para tornar mais fácil para as pessoas que já conhecem HTML adicionar o processamento de servidor às suas páginas. É uma boa opção para estudantes, entusiastas, pessoas em geral que são novas na programação. Também pode ser uma boa opção para desenvolvedores que têm experiência com tecnologias da Web do non-ASP.NET.

<a id="Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages"></a>
## <a name="do-i-need-webmatrix-in-order-to-work-with-web-pages"></a>Preciso do WebMatrix para trabalhar com páginas da Web?

Não. O WebMatrix não é mais recomendado como um ambiente de desenvolvimento integrado para Páginas da Web do ASP.NET. Use o [Visual Studio](program-asp-net-web-pages-in-visual-studio.md) ou o [Visual Studio Code](https://code.visualstudio.com/).

Se você não quiser usar o Visual Studio ou o Visual Studio Code, poderá instalar os produtos de componentes individualmente usando [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx). Você precisa dos seguintes produtos:

- Microsoft .NET Framework 4.5
- ASP.NET MVC 5 (que também instala o Páginas da Web do ASP.NET Framework)
- IIS Express (o servidor Web)
- Microsoft SQL Server Compact 4,0 (o banco de dados)

Você pode usar um editor de texto para editar páginas *. cshtml* (ou *. vbhtml*).

O gerenciamento de bancos de dados SQL Server Compact (arquivos *. sdf* ) sem uma ferramenta é um pouco mais difícil. Ferramentas de containds do Visual Studio para gerenciar bancos de dados *. sdf* . Você também pode executar comandos SQL no código para executar muitas tarefas de gerenciamento de SQL Server.

Para testar as páginas *. cshtml* sem usar um ambiente de desenvolvimento integrado (IDE), você pode implantá-las em um servidor ativo. (Veja posso [implantar um páginas da Web do ASP.net site sem usar o WebMatrix?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix))

### <a name="running-iis-express-without-using-an-ide"></a>Executando IIS Express sem usar um IDE

Se você instalar IIS Express no computador como um servidor Web, poderá usá-lo para testar as páginas. Você pode executar IIS Express na linha de comando e associá-la a um número de porta específico. Em seguida, você especifica essa porta quando solicita arquivos *. cshtml* em seu navegador.

No Windows, abra um prompt de comando com privilégios de administrador e altere para *C:\Program Files\IIS Express.* (Para sistemas de 64 bits, use a pasta *C:\Program Files (x86) \IIS Express.)* Em seguida, digite o seguinte comando usando o caminho real para seu site:

`iisexpress.exe /port:35896 /path:C:\BasicWebSite`

Você pode usar qualquer número de porta que ainda não esteja reservado por outro processo. (Os números de porta acima de 1024 normalmente são gratuitos.) Para o valor de `path`, use o caminho da pasta do site em que os arquivos *. cshtml* são.

Depois de executar esse comando para configurar IIS Express para atender às suas páginas, você pode abrir um navegador e navegar até um arquivo *. cshtml* . Use uma URL semelhante à seguinte:

`http://localhost:35896/default.cshtml`

Para obter ajuda com IIS Express opções de linha de comando, digite `iisexpress.exe /?` na linha de comando.

<a id="Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page"></a>
## <a name="can-i-use-aspnet-web-forms-controls-on-a-web-pages-page"></a>Posso usar controles de Web Forms ASP.NET em uma página de páginas da Web?

Não. Web Forms controles como o controle de [caixa de seleção](https://msdn.microsoft.com/library/system.web.ui.webcontrols.checkbox) , os [controles de validação](https://msdn.microsoft.com/library/bwd43d0x)e o controle [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview) só funcionam em Web Forms páginas (arquivos *. aspx* ). Esses controles exigem a estrutura da página Web Forms.

<a id="Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix"></a>
## <a name="can-i-deploy-an-aspnet-web-pages-site-without-using-webmatrix"></a>Posso implantar um site Páginas da Web do ASP.NET sem usar o WebMatrix?

Sim. Você pode copiar manualmente os arquivos do site para um servidor (normalmente usando FTP). Se você executar uma cópia manual, também precisará copiar os arquivos que dão suporte a SQL Server Compact (o banco de dados). Para obter detalhes, consulte a entrada de blog [Implantando aplicativos de páginas da Web sem uma ferramenta](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2317).

<a id="Do_I_have_to_use_the_WebSecurity_helper_to_support_logins"></a>
## <a name="do-i-have-to-use-the-websecurity-helper-to-support-logins"></a>É necessário usar o auxiliar do websecurity para dar suporte a logons?

Não. O provedor de `SimpleMembership` que faz parte de Páginas da Web do ASP.NET é uma opção. Os provedores de segurança que fazem parte do ASP.NET (que você pode usar para trabalhar no Web Forms) também estão disponíveis. Por exemplo, você pode usar a autenticação de formulários no Páginas da Web do ASP.NET assim como faria no Web Forms. Para obter um exemplo de como usar a autenticação de formulários, consulte o artigo Suporte da Microsoft [como implementar a autenticação baseada em formulários em seu aplicativo ASP.NET C#usando o .net](https://support.microsoft.com/kb/301240). Para baixar um exemplo simples, consulte a [versão ASP.net de "logon &amp; senha](http://www.codeguru.com/csharp/.net/net_asp/scripting/article.php/c19295/ASPNET-version-of-Login--Password.htm).

Para obter informações sobre como usar a autenticação do Windows, consulte a postagem de blog [usando a autenticação do Windows no páginas da Web do ASP.net](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2298).

<a id="Does_ASP.NET_Web_Pages_support_HTML5"></a>
## <a name="does-aspnet-web-pages-support-html5"></a>Páginas da Web do ASP.NET dá suporte a HTML5?

Sim. As páginas criadas com Páginas da Web do ASP.NET (páginas *. cshtml* ou *. vbhtml* ) são essencialmente páginas HTML que também contêm o código executado no servidor, antes que a página seja processada. Desde que o navegador do usuário dê suporte a HTML5, você pode usar elementos HTML5 em uma página *. cshtml* ou *. vbhtml* .

<a id="Can_I_use_JavaScript_and_jQuery_with_Web_Pages"></a>
## <a name="can-i-use-javascript-and-jquery-with-web-pages"></a>Posso usar JavaScript e jQuery com páginas da Web?

Com certeza. As páginas criadas com Páginas da Web do ASP.NET (páginas *. cshtml* ou *. vbhtml* ) são apenas páginas HTML com código de servidor. Portanto, tudo o que você pode fazer em uma página HTML normal usando JavaScript ou jQuery também pode fazer em uma página *. cshtml* ou *. vbhtml* .

O modelo de **site inicial** no WebMatrix contém várias bibliotecas jQuery. Se você criar um site usando esse modelo, a pasta *scripts* conterá uma biblioteca principal do jQuery (*jQuery-1.6.2. js)* e bibliotecas para validação jQuery (*jQuery. Validate. js*, etc.).

Aqui estão algumas postagens de blog que ilustram maneiras de usar o jQuery com Páginas da Web do ASP.NET:

- [Adicionando a sorte do jQuery a páginas da Web do ASP.NET usando o WebMatrix](http://rachelappel.com/jquery/adding-jquery-goodness-to-asp-net-web-pages-using-webmatrix/) por Rachel appel
- [5 min: WebMatrix + jQuery UI + JSON + jQuery templates](http://joeriks.com/2011/01/30/5-min-webmatrix-jquery-ui-json-jquery-templates/) por Jonas Eriksson
- [Formulários do WebMatrix e jQuery](http://mikesdotnetting.com/Article/155/WebMatrix-And-jQuery-Forms) por Mike Brind

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Recursos adicionais

[Guia de solução de problemas de Páginas da Web do ASP.NET (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001)

[Fórum do WebMatrix e do páginas da Web do ASP.net](https://forums.asp.net/1224.aspx/1?WebMatrix) no site do ASP.net
