---
uid: web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
title: Personalizando o comportamento de todo o site para sites Páginas da Web do ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Este capítulo explica como fazer configurações em todo o seu site ou em uma pasta inteira, em vez de apenas uma página.
ms.author: riande
ms.date: 02/17/2014
ms.assetid: e158bed7-226f-4275-b02e-7553bd58c669
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
msc.type: authoredcontent
ms.openlocfilehash: f05e05f725d9209bce283ce18659ae5efe4de2ee
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634620"
---
# <a name="customizing-site-wide-behavior-for-aspnet-web-pages-razor-sites"></a>Personalizando o comportamento de todo o site para sites Páginas da Web do ASP.NET (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo explica como fazer as configurações do lado do site para páginas em um site Páginas da Web do ASP.NET (Razor).
> 
> O que você aprenderá:
> 
> - Como executar o código que permite definir valores (valores globais ou configurações auxiliares) para todas as páginas em um site.
> - Como executar o código que permite definir valores para todas as páginas em uma pasta.
> - Como executar o código antes e depois que uma página é carregada.
> - Como enviar erros para uma página de erro central.
> - Como adicionar autenticação a todas as páginas em uma pasta.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 2
> - WebMatrix 3
> - Biblioteca de auxiliares Web do ASP.NET (pacote NuGet)
>   
> 
> Este tutorial também funciona com Páginas da Web do ASP.NET 3 e Visual Studio 2013 (ou Visual Studio Express 2013 para Web), exceto que você não pode usar a biblioteca de auxiliares Web do ASP.NET.

<a id="Adding_Website_Startup_Code"></a>
## <a name="adding-website-startup-code-for-aspnet-web-pages"></a>Adicionando código de inicialização do site para Páginas da Web do ASP.NET

Para grande parte do código que você escreve no Páginas da Web do ASP.NET, uma página individual pode conter todo o código necessário para essa página. Por exemplo, se uma página envia uma mensagem de email, é possível colocar todo o código para essa operação em uma única página. Isso pode incluir o código para inicializar as configurações de envio de email (ou seja, para o servidor SMTP) e para enviar a mensagem de email.

No entanto, em algumas situações, talvez você queira executar algum código antes que qualquer página no site seja executada. Isso é útil para definir valores que podem ser usados em qualquer lugar no site (chamados de *valores globais*). Por exemplo, alguns auxiliares exigem que você forneça valores como configurações de email ou chaves de conta. Pode ser útil manter essas configurações em valores globais.

Você pode fazer isso criando uma página chamada *\_AppStart. cshtml* na raiz do site. Se essa página existir, ela será executada na primeira vez em que qualquer página do site for solicitada. Portanto, é um bom local para executar o código para definir valores globais. (Como *\_AppStart. cshtml* tem um prefixo de sublinhado, o ASP.net não enviará a página para um navegador, mesmo que os usuários solicitem diretamente.)

O diagrama a seguir mostra como a página *\_AppStart. cshtml* funciona. Quando uma solicitação chega para uma página e, se esta for a primeira solicitação de qualquer página no site, o ASP.NET verificará primeiro se existe uma página *\_AppStart. cshtml* . Nesse caso, qualquer código na página *\_AppStart. cshtml* é executado e a página solicitada é executada.

![[imagem]](18-customizing-site-wide-behavior/_static/image1.jpg)

## <a name="setting-global-values-for-your-website"></a>Definindo valores globais para seu site

1. Na pasta raiz de um site do WebMatrix, crie um arquivo chamado *\_AppStart. cshtml*. O arquivo deve estar na raiz do site.
2. Substitua o conteúdo existente pelo seguinte: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample1.cshtml)]

    Esse código armazena um valor no dicionário de `AppState`, que está automaticamente disponível para todas as páginas no site. Observe que o *\_arquivo AppStart. cshtml* não tem nenhuma marcação. A página executará o código e redirecionará para a página que foi solicitada originalmente.

    > [!NOTE]
    > Tenha cuidado ao colocar o código no arquivo *\_AppStart. cshtml* . Se ocorrerem erros no código do arquivo *\_AppStart. cshtml* , o site não será iniciado.
3. Na pasta raiz, crie uma nova página chamada *AppName. cshtml*.
4. Substitua a marcação e o código padrão pelo seguinte: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample2.html)]

    Esse código extrai o valor do objeto `AppState` que você definiu na página *\_AppStart. cshtml* .
5. Execute a página *AppName. cshtml* em um navegador. (Verifique se a página está selecionada no espaço de trabalho **arquivos** antes de executá-la.) A página exibe o valor global. 

    ![[imagem]](18-customizing-site-wide-behavior/_static/image2.jpg)

<a id="Setting_Values_For_Helpers"></a>
## <a name="setting-values-for-helpers"></a>Definindo valores para auxiliares

Um bom uso para o arquivo *\_AppStart. cshtml* é definir valores para auxiliares que você usa em seu site e que precisam ser inicializados. Exemplos típicos são configurações de email para o auxiliar de `WebMail` e as chaves pública e privada para o auxiliar de `ReCaptcha`. Em casos como esses, você pode definir os valores uma vez no *\_AppStart. cshtml* e, em seguida, eles já estão definidos para todas as páginas no seu site.

Este procedimento mostra como definir `WebMail` configurações globalmente. (Para obter mais informações sobre como usar o auxiliar de `WebMail`, consulte [adicionando email a um site do páginas da Web do ASP.net](../getting-started/11-adding-email-to-your-web-site.md).)

1. Adicione a biblioteca de auxiliares Web do ASP.NET ao seu site, conforme descrito em [instalando auxiliares em um páginas da Web do ASP.net site](https://go.microsoft.com/fwlink/?LinkId=252372), se você ainda não o tiver adicionado.
2. Se você ainda não tiver um arquivo *\_AppStart. cshtml* , na pasta raiz de um site, crie um arquivo chamado *\_AppStart. cshtml*.
3. Adicione as seguintes configurações de `WebMail` ao arquivo *\_AppStart. cshtml* : 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample3.cshtml?highlight=2-7)]

    Modifique as seguintes configurações relacionadas a email no código:

   - Defina `your-SMTP-host` como o nome do servidor SMTP ao qual você tem acesso.
   - Defina `your-user-name-here` como o nome de usuário da sua conta do servidor SMTP.
   - Defina `your-account-password` para a senha da sua conta do servidor SMTP.
   - Defina `your-email-address-here` para seu próprio endereço de email. Esse é o endereço de email do qual a mensagem é enviada. (Alguns provedores de email não permitem que você especifique um endereço de `From` diferente e usará seu nome de usuário como o endereço de `From`.)

     Para obter mais informações sobre configurações de SMTP, consulte [definindo configurações de email](https://go.microsoft.com/fwlink/?LinkID=202899#configuring_email_settings) no artigo [enviando email de um site páginas da Web do ASP.net (Razor)](https://go.microsoft.com/fwlink/?LinkID=202899) e [problemas com o envio de email](https://go.microsoft.com/fwlink/?LinkId=253001#email) no guia de solução de problemas do [páginas da Web do ASP.net (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001).
4. Salve o arquivo *\_AppStart. cshtml* e feche-o.
5. Na pasta raiz de um site, crie uma nova página chamada *TestEmail. cshtml*.
6. Substitua o conteúdo existente pelo seguinte: 

     [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample4.cshtml)]
7. Execute a página *TestEmail. cshtml* em um navegador.
8. Preencha os campos para enviar uma mensagem de email e, em seguida, clique em **Enviar**.
9. Verifique seu email para certificar-se de que você obteve a mensagem.

A parte importante deste exemplo é que as configurações que você não costuma alterar, como o nome do seu servidor SMTP e suas credenciais de email, são definidas no arquivo *\_AppStart. cshtml* . Dessa forma, você não precisará defini-las novamente em cada página em que você enviar email. (Embora, por alguma razão, você precise alterar essas configurações, você pode defini-las individualmente em uma página.) Na página, você só define os valores que normalmente mudam a cada vez, como o destinatário e o corpo da mensagem de email.

<a id="Running_Code_Before_and_After"></a>
## <a name="running-code-before-and-after-files-in-a-folder"></a>Executando código antes e depois dos arquivos em uma pasta

Assim como você pode usar *\_AppStart. cshtml* para escrever código antes de as páginas no site serem executadas, você pode escrever o código que é executado antes (e depois) que qualquer página em uma pasta específica seja executada. Isso é útil para coisas como definir a mesma página de layout para todas as páginas em uma pasta ou para verificar se um usuário está conectado antes de executar uma página na pasta.

Para páginas em pastas específicas, você pode criar código em um arquivo chamado *\_PageStart. cshtml*. O diagrama a seguir mostra como a página *\_PageStart. cshtml* funciona. Quando uma solicitação chega para uma página, o ASP.NET primeiro verifica uma página *\_AppStart. cshtml* e executa isso. Em seguida, ASP.NET verifica se há uma página *\_PageStart. cshtml* e, nesse caso, é executado. Em seguida, ele executa a página solicitada.

Dentro da página *\_PageStart. cshtml* , você pode especificar onde durante o processamento deseja que a página solicitada seja executada, incluindo um método `RunPage`. Isso permite que você execute o código antes que a página solicitada seja executada e depois. Se você não incluir `RunPage`, todo o código em *\_PageStart. cshtml* será executado e a página solicitada será executada automaticamente.

![[imagem]](18-customizing-site-wide-behavior/_static/image3.jpg)

O ASP.NET permite criar uma hierarquia de arquivos do *\_PageStart. cshtml* . Você pode colocar um arquivo *\_PageStart. cshtml* na raiz do site e em qualquer subpasta. Quando uma página é solicitada, o arquivo de *\_PageStart. cshtml* no nível mais alto (mais próximo à raiz do site) é executado, seguido pelo arquivo *\_PageStart. cshtml* na subpasta seguinte e, por fim, na estrutura da subpasta até que a solicitação atinja a pasta que contém a página solicitada. Depois que todos os arquivos *PageStart. cshtml do\_* aplicáveis forem executados, a página solicitada será executada.

Por exemplo, você pode ter a seguinte combinação de *\_arquivos PageStart. cshtml* e o arquivo *Default. cshtml* :

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample5.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample6.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample7.cshtml)]

Ao executar o */MyFolder/default.cshtml*, você verá o seguinte:

[!code-console[Main](18-customizing-site-wide-behavior/samples/sample8.cmd)]

## <a name="running-initialization-code-for-all-pages-in-a-folder"></a>Executando o código de inicialização para todas as páginas em uma pasta

Um bom uso para *\_arquivos PageStart. cshtml* é inicializar a mesma página de layout para todos os arquivos em uma única pasta.

1. Na pasta raiz, crie uma nova pasta chamada *InitPages*.
2. Na pasta *InitPages* do seu site, crie um arquivo chamado *\_PageStart. cshtml* e substitua a marcação e o código padrão pelo seguinte: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample9.cshtml)]
3. Na raiz do site, crie uma pasta chamada *Shared*.
4. Na pasta *compartilhada* , crie um arquivo chamado *\_Layout1. cshtml* e substitua a marcação e o código padrão pelo seguinte: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample10.cshtml)]
5. Na pasta *InitPages* , crie um arquivo chamado *Content1. cshtml* e substitua o conteúdo existente pelo seguinte: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample11.html)]
6. Na pasta *InitPages* , crie outro arquivo chamado *Content2. cshtml* e substitua a marcação padrão pelo seguinte: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample12.html)]
7. Execute *Content1. cshtml* em um navegador. 

    ![[imagem]](18-customizing-site-wide-behavior/_static/image4.jpg)

    Quando a página *Content1. cshtml* é executada, o arquivo *\_PageStart. cshtml* define `Layout` e também define `PageData["MyBackground"]` como uma cor. No *Content1. cshtml*, o layout e a cor são aplicados.
8. Exiba *Content2. cshtml* em um navegador. 

    O layout é o mesmo, porque as duas páginas usam a mesma página de layout e a mesma cor que foi inicializada em *\_PageStart. cshtml*.

## <a name="using-_pagestartcshtml-to-handle-errors"></a>Usando \_PageStart. cshtml para lidar com erros

Outro bom uso para o arquivo *\_PageStart. cshtml* é criar uma maneira de lidar com erros de programação (exceções) que podem ocorrer em qualquer página *. cshtml* em uma pasta. Este exemplo mostra uma maneira de fazer isso.

1. Na pasta raiz, crie uma pasta chamada *InitCatch*.
2. Na pasta *InitCatch* do seu site, crie um arquivo chamado *\_PageStart. cshtml* e substitua a marcação e o código existentes pelo seguinte: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample13.cshtml)]

    Nesse código, você tenta executar a página solicitada explicitamente chamando o método `RunPage` dentro de um bloco de `try`. Se ocorrerem erros de programação na página solicitada, o código dentro do bloco de `catch` será executado. Nesse caso, o código redireciona para uma página (*Error. cshtml*) e passa o nome do arquivo que apresentou o erro como parte da URL. (Você criará a página em breve.)
3. Na pasta *InitCatch* do seu site, crie um arquivo chamado *Exception. cshtml* e substitua a marcação e o código existentes pelo seguinte: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample14.cshtml)]

    Para fins deste exemplo, o que você está fazendo nesta página é deliberadamente criar um erro ao tentar abrir um arquivo de banco de dados que não existe.
4. Na pasta raiz, crie um arquivo chamado *Error. cshtml* e substitua a marcação e o código existentes pelo seguinte: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample15.html)]

    Nessa página, a expressão `@Request["source"]` Obtém o valor da URL e o exibe.
5. Na barra de ferramentas, clique em **salvar**.
6. Execute *Exception. cshtml* em um navegador. 

    ![[imagem]](18-customizing-site-wide-behavior/_static/image5.jpg)

    Como ocorre um erro em *Exception. cshtml*, a página *\_PageStart. cshtml* redireciona para o arquivo *Error. cshtml* , que exibe a mensagem.

    Para obter mais informações sobre exceções, consulte [introdução à programação de páginas da Web do ASP.NET usando a sintaxe do Razor](https://go.microsoft.com/fwlink/?LinkID=251587).

<a id="Using__PageStart.cshtml_to_Restrict_Folder_Access"></a>
## <a name="using-_pagestartcshtml-to-restrict-folder-access"></a>Usando \_PageStart. cshtml para restringir o acesso à pasta

Você também pode usar o arquivo *\_PageStart. cshtml* para restringir o acesso a todos os arquivos em uma pasta.

1. No WebMatrix, crie um novo site usando a opção **site a partir do modelo** .
2. Nos modelos disponíveis, selecione **site inicial**.
3. Na pasta raiz, crie uma pasta chamada *AuthenticatedContent*.
4. Na pasta *AuthenticatedContent* , crie um arquivo chamado *\_PageStart. cshtml* e substitua a marcação e o código existentes pelo seguinte: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample16.cshtml)]

    O código começa impedindo que todos os arquivos na pasta sejam armazenados em cache. (Isso é necessário para cenários como computadores públicos, onde você não deseja que as páginas em cache de um usuário estejam disponíveis para o próximo usuário.) Em seguida, o código determina se o usuário entrou no site antes de poder exibir qualquer uma das páginas na pasta. Se o usuário não estiver conectado, o código será redirecionado para a página de logon. A página de logon pode retornar o usuário para a página que foi solicitada originalmente se você incluir um valor de cadeia de caracteres de consulta chamado `ReturnUrl`.
5. Crie uma nova página na pasta *AuthenticatedContent* chamada *Page. cshtml*.
6. Substitua a marcação padrão pelo seguinte:  

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample17.cshtml)]
7. Execute *Page. cshtml* em um navegador. O código redireciona você para uma página de logon. Você deve se registrar antes de fazer logon. Depois de registrar e fazer logon, você pode navegar até a página e exibir seu conteúdo.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais

[Introdução à programação de Páginas da Web do ASP.NET usando a sintaxe do Razor](https://go.microsoft.com/fwlink/?LinkID=251587)
