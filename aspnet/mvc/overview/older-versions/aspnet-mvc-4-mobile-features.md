---
uid: mvc/overview/older-versions/aspnet-mvc-4-mobile-features
title: Recursos móveis do ASP.NET MVC 4 | Microsoft Docs
author: Rick-Anderson
description: Agora há uma versão MVC 5 deste tutorial com exemplos de código em implantar um aplicativo Web móvel ASP.NET MVC 5 nos sites do Azure.
ms.author: riande
ms.date: 08/15/2012
ms.assetid: 27dc4fc8-1b51-43b0-933f-fc1b52476523
msc.legacyurl: /mvc/overview/older-versions/aspnet-mvc-4-mobile-features
msc.type: authoredcontent
ms.openlocfilehash: 907a16946c93761cd543135b0b226c8696b041f0
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74594641"
---
# <a name="aspnet-mvc-4-mobile-features"></a>Recursos de celular do ASP.NET MVC 4

por [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Agora há uma versão MVC 5 deste tutorial com exemplos de código em [implantar um aplicativo Web móvel ASP.NET MVC 5 nos sites do Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/).

Este tutorial ensinará a você noções básicas de como trabalhar com recursos móveis em um aplicativo Web ASP.NET MVC 4. Para este tutorial, você pode usar [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) ou Visual Web developer 2010 Express Service Pack 1 (&quot;Visual Web Developer ou VWD&quot;). Você pode usar a versão Professional do Visual Studio se já tiver isso.

Antes de começar, verifique se você instalou os pré-requisitos listados abaixo.

- [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) (recomendado) ou Visual Studio Web Developer Express SP1. O Visual Studio 2012 contém o ASP.NET MVC 4. Se você estiver usando o Visual Web Developer 2010, deverá instalar o [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392).

Você também precisará de um emulador de navegador móvel. Qualquer um dos seguintes funcionará:

- [Emulador de telefone do Windows 7](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). (Esse é o emulador que é usado na maioria das capturas de tela neste tutorial.)
- Altere a cadeia de caracteres do agente do usuário para emular um iPhone. Consulte [esta](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) entrada de blog.
- [Emulador móvel Opera](http://www.opera.com/developer/tools/mobile/)
- [Apple Safari](http://www.apple.com/safari/download/) com o agente do usuário definido como iPhone. Para obter instruções sobre como definir o agente do usuário no Safari como "iPhone", consulte [como permitir que o Safari finja que ele é o IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) no blog de David Alison.

Os projetos do Visual C# Studio com código-fonte estão disponíveis para acompanhar este tópico:

- [Download do projeto inicial](https://go.microsoft.com/fwlink/?linkid=228307&amp;clcid=0x409)
- [Download do projeto concluído](https://go.microsoft.com/fwlink/?linkid=228306&amp;clcid=0x409)

### <a name="what-youll-build"></a>O que você criará

Para este tutorial, você adicionará recursos móveis ao aplicativo de listagem de conferência simples que é fornecido no [projeto inicial](https://go.microsoft.com/fwlink/?LinkId=228307). A captura de tela a seguir mostra a página marcas do aplicativo concluído, como visto no [Windows 7 Phone Emulator](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). Consulte [mapeamento de teclado para o emulador de Windows Phone](https://msdn.microsoft.com/library/ff754352(v=vs.92).aspx) para simplificar a entrada do teclado.

[![p1_Tags_CompletedProj](aspnet-mvc-4-mobile-features/_static/image2.png)](aspnet-mvc-4-mobile-features/_static/image1.png)

Você pode usar o Internet Explorer versão 9 ou 10, FireFox ou Chrome para desenvolver seu aplicativo móvel definindo a [cadeia de caracteres do agente do usuário](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/). A imagem a seguir mostra o tutorial concluído usando o Internet Explorer emulando um iPhone. Você pode usar as ferramentas de desenvolvedor do Internet Explorer F-12 e a [ferramenta Fiddler](http://www.fiddler2.com/fiddler2/) para ajudar a depurar seu aplicativo.

![](aspnet-mvc-4-mobile-features/_static/image3.png)

### <a name="skills-youll-learn"></a>Habilidades que você aprenderá

Veja o que você aprenderá:

- Como os modelos do ASP.NET MVC 4 usam o atributo `viewport` do HTML5 e a renderização adaptável para melhorar a exibição em dispositivos móveis.
- Como criar exibições específicas para dispositivos móveis.
- Como criar um seletor de exibição que permite aos usuários alternar entre uma exibição móvel e uma exibição de área de trabalho do aplicativo.

### <a name="getting-started"></a>Guia de Introdução

Baixe o aplicativo de listagem de conferência para o projeto inicial usando o seguinte link: [Download](https://go.microsoft.com/fwlink/?LinkId=228307). Em seguida, no Windows Explorer, clique com o botão direito do mouse no arquivo *MvcMobile. zip* e escolha **Propriedades**. Na caixa de diálogo **Propriedades de MvcMobile. zip** , escolha o botão **desbloquear** . (Desbloquear impede um aviso de segurança que ocorre quando você tenta usar um arquivo *. zip* que você baixou da Web.)

![p1_unBlock](aspnet-mvc-4-mobile-features/_static/image4.png)

Clique com o botão direito do mouse no arquivo *MvcMobile. zip* e selecione **extrair tudo** para descompactar o arquivo. No Visual Studio, abra o arquivo *MvcMobile. sln* .

Pressione CTRL + F5 para executar o aplicativo, que o exibirá no navegador da área de trabalho. Inicie o emulador do navegador móvel, copie a URL para o aplicativo de conferência no emulador e, em seguida, clique no link **procurar por marca** . Se você estiver usando o emulador de Windows Phone, clique na barra de URL e pressione a tecla PAUSE para obter acesso ao teclado. A imagem abaixo mostra o modo de exibição de *marcas* de seleção (escolhendo **procurar por marca**).

[![p1_browseTag](aspnet-mvc-4-mobile-features/_static/image6.png)](aspnet-mvc-4-mobile-features/_static/image5.png)

A exibição é muito legível em um dispositivo móvel. Escolha o link ASP.NET.

[![p1_tagged_ASPNET](aspnet-mvc-4-mobile-features/_static/image8.png)](aspnet-mvc-4-mobile-features/_static/image7.png)

A exibição de marca ASP.NET é muito obstruída. Por exemplo, a coluna de **Data** é muito difícil de ler. Posteriormente, no tutorial, você criará uma versão do modo de exibição de *marcas* , que é especificamente para navegadores móveis, e isso tornará a exibição legível.

Observação: no momento, existe um bug no mecanismo de cache móvel. Para aplicativos de produção, você deve instalar o pacote preciosidade [DisplayModes fixos](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) . Consulte [bug do cache móvel do ASP.NET MVC 4 corrigido](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) para obter detalhes sobre a correção.

## <a name="css-media-queries"></a>Consultas de mídia do CSS

As [consultas de mídia do CSS](http://www.w3.org/TR/css3-mediaqueries/) são uma extensão do CSS para tipos de mídia. Eles permitem que você crie regras que substituem as regras padrão de CSS para navegadores específicos (agentes de usuário). Uma regra comum para CSS que tem como alvo navegadores móveis é definir o tamanho máximo da tela. O arquivo *Content\Site.css* criado quando você cria um novo projeto de Internet do MVC 4 da ASP.net contém a seguinte consulta de mídia:

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample1.css)]

Se a janela do navegador tiver 850 pixels de largura ou menos, ela usará as regras de CSS dentro desse bloco de mídia. Você pode usar consultas de mídia do CSS como esta para fornecer uma exibição melhor do conteúdo HTML em navegadores pequenos (como navegadores móveis) do que as regras CSS padrão que são projetadas para as exibições mais amplas dos navegadores da área de trabalho.

## <a name="the-viewport-meta-tag"></a>A marca meta viewport

A maioria dos navegadores móveis definem a largura da janela do navegador virtual (o *visor*) que é muito maior do que a largura real do dispositivo móvel. Isso permite que os navegadores móveis se ajustem a toda a página da Web dentro da exibição virtual. Os usuários podem ampliar o conteúdo interessante. No entanto, se você definir a largura do visor como a largura real do dispositivo, não será necessário aplicar zoom, pois o conteúdo se encaixa no navegador móvel.

A marca de `<meta>` do visor no arquivo de layout do ASP.NET MVC 4 define o visor para a largura do dispositivo. A linha a seguir mostra a marca de `<meta>` do visor no arquivo de layout do ASP.NET MVC 4.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample2.html)]

## <a name="examining-the-effect-of-css-media-queries-and-the-viewport-meta-tag"></a>Examinando o efeito de consultas de mídia do CSS e a marca meta do visor

Abra o arquivo *Views\Shared\\_Layout. cshtml* no editor e comente a marca de `<meta>` do visor. A marcação a seguir mostra a linha comentada.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample3.cshtml)]

Abra o arquivo *MvcMobile\Content\Site.css* no editor e altere a largura máxima na consulta de mídia para zero pixels. Isso impedirá que as regras de CSS sejam usadas em navegadores móveis. A linha a seguir mostra a consulta de mídia modificada:

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample4.css)]

Salve as alterações e navegue até o aplicativo de conferência em um emulador de navegador móvel. O pequeno texto na imagem a seguir é o resultado da remoção da marca de `<meta>` do visor. Sem marca de `<meta>` do visor, o navegador está ampliando para a largura padrão do visor (850 pixels ou mais largo para a maioria dos navegadores móveis.)

[![p1_noViewPort](aspnet-mvc-4-mobile-features/_static/image10.png)](aspnet-mvc-4-mobile-features/_static/image9.png)

Desfazer as alterações — remova a marca de comentário da marcação de `<meta>` do visor no arquivo de layout e restaure a consulta de mídia para 850 pixels no arquivo *site. css* . Salve as alterações e atualize o navegador móvel para verificar se a exibição amigável para dispositivos móveis foi restaurada.

A marca de `<meta>` do visor e a consulta de mídia do CSS não são específicas do ASP.NET MVC 4, e você pode aproveitar esses recursos em qualquer aplicativo Web. Mas eles agora são incorporados aos arquivos que são gerados quando você cria um novo projeto do ASP.NET MVC 4.

Para obter mais informações sobre a marca de `<meta>` do visor, consulte [uma história de dois viewports — parte dois](http://www.quirksmode.org/mobile/viewports2.html).

Na próxima seção, você verá como fornecer exibições específicas do navegador móvel.

## <a name="overriding-views-layouts-and-partial-views"></a>Substituindo exibições, layouts e exibições parciais

Um novo recurso significativo no ASP.NET MVC 4 é um mecanismo simples que permite substituir qualquer modo de exibição (incluindo layouts e exibições parciais) para navegadores móveis em geral, para um navegador móvel individual ou para qualquer navegador específico. Para fornecer uma exibição específica ao celular, você pode copiar um arquivo de exibição e adicionar *. Móvel* para o nome do arquivo. Por exemplo, para criar uma exibição de *índice* móvel, copie *Views\Home\Index.cshtml* para *Views\Home\Index.Mobile.cshtml*.

Nesta seção, você criará um arquivo de layout específico do Mobile.

Para iniciar, copie *Views\Shared\\_Layout. cshtml* para *Views\Shared\\_Layout. Mobile. cshtml*. Abra *\_layout. Mobile. cshtml* e altere o título de **MVC4 Conference** para **Conference (Mobile)** .

Em cada chamada de `Html.ActionLink`, remova "procurar por" em cada link *ActionLink*. O código a seguir mostra a seção de corpo concluída do arquivo de layout móvel.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample5.cshtml)]

Copie o arquivo *Views\Home\AllTags.cshtml* para *Views\Home\AllTags.Mobile.cshtml*. Abra o novo arquivo e altere o elemento `<h2>` de "tags" para "Tags (M)":

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample6.html)]

Navegue até a página marcas usando um navegador da área de trabalho e usando o emulador do navegador móvel. O emulador do navegador móvel mostra as duas alterações feitas.

[![p2m_layoutTags. Mobile](aspnet-mvc-4-mobile-features/_static/image12.png)](aspnet-mvc-4-mobile-features/_static/image11.png)

Por outro lado, a exibição da área de trabalho não foi alterada.

[![p2_layoutTagsDesktop](aspnet-mvc-4-mobile-features/_static/image14.png)](aspnet-mvc-4-mobile-features/_static/image13.png)

## <a name="browser-specific-views"></a>Exibições específicas do navegador

Além de exibições específicas para dispositivos móveis e de desktop, você pode criar modos de exibição para um navegador individual. Por exemplo, você pode criar exibições que são especificamente para o navegador iPhone. Nesta seção, você criará um layout para o navegador do iPhone e uma versão do iPhone do modo de exibição de *marcas* .

Abra o arquivo *global. asax* e adicione o código a seguir ao método `Application_Start`.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample7.cs)]

Esse código define um novo modo de exibição chamado "iPhone" que será correspondido em cada solicitação de entrada. Se a solicitação de entrada corresponder à condição que você definiu (ou seja, se o agente do usuário contiver a cadeia de caracteres "iPhone"), o ASP.NET MVC procurará exibições cujo nome contenha o sufixo "iPhone".

No código, clique com o botão direito do mouse em `DefaultDisplayMode`, escolha **resolver**e, em seguida, escolha `using System.Web.WebPages;`. Isso adiciona uma referência ao namespace `System.Web.WebPages`, que é onde os tipos de `DisplayModes` e `DefaultDisplayMode` são definidos.

[![p2_resolve](aspnet-mvc-4-mobile-features/_static/image16.png)](aspnet-mvc-4-mobile-features/_static/image15.png)

Como alternativa, você pode apenas adicionar manualmente a seguinte linha à seção `using` do arquivo.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample8.cs)]

O conteúdo completo do arquivo *global. asax* é mostrado abaixo.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample9.cs)]

Salve as alterações. Copie o arquivo *MvcMobile\Views\Shared\\_Layout. Mobile. cshtml* para *MvcMobile\Views\Shared\\_Layout. iPhone. cshtml*. Abra o novo arquivo e altere o cabeçalho `h1` de `Conference (Mobile)` para `Conference (iPhone)`.

Copie o arquivo *MvcMobile\Views\Home\AllTags.Mobile.cshtml* para *MvcMobile\Views\Home\AllTags.iPhone.cshtml*. No novo arquivo, altere o elemento `<h2>` de "Tags (M)" para "Tags (iPhone)".

Execute o aplicativo. Execute um emulador de navegador móvel, verifique se o agente do usuário está definido como "iPhone" e navegue até o modo de exibição de *marcas* . A captura de tela a seguir mostra o modo de exibição de todas as *marcas* renderizado no navegador [Safari](http://www.apple.com/safari/download/) . Você pode baixar o Safari para Windows [aqui](https://support.apple.com/kb/DL1531).

[![p2_iphoneView](aspnet-mvc-4-mobile-features/_static/image18.png)](aspnet-mvc-4-mobile-features/_static/image17.png)

Nesta seção, vimos como criar layouts e exibições móveis e como criar layouts e exibições para dispositivos específicos, como o iPhone. Na próxima seção, você verá como aproveitar o jQuery Mobile para exibições móveis mais atraentes.

## <a name="using-jquery-mobile"></a>Usando o jQuery Mobile

A biblioteca do [jQuery Mobile](http://jquerymobile.com/demos/1.0b3/#/demos/1.0b3/docs/about/intro.html) fornece uma estrutura de interface do usuário que funciona em todos os principais navegadores móveis. o jQuery Mobile aplica *aprimoramento progressivo* a navegadores móveis que dão suporte a CSS e a JavaScript. O aprimoramento progressivo permite que todos os navegadores exibam o conteúdo básico de uma página da Web, permitindo que navegadores e dispositivos mais poderosos tenham uma exibição mais rica. Os arquivos JavaScript e CSS que estão incluídos com o jQuery Mobile Style muitos elementos para se ajustar aos navegadores móveis sem fazer nenhuma alteração de marcação.

Nesta seção, você instalará o pacote NuGet *jQuery. Mobile. Mvc* , que instala o jQuery Mobile e um widget de seletor de exibição.

Para iniciar, exclua os arquivos *compartilhados\\_Layout. Mobile. cshtml* e *compartilhados\\_Layout. iPhone. cshtml* que você criou anteriormente.

Renomeie os arquivos *Views\Home\AllTags.Mobile.cshtml* e *Views\Home\AllTags.iPhone.cshtml* para *Views\Home\AllTags.iPhone.cshtml.Hide* e *Views\Home\AllTags.Mobile.cshtml.Hide*. Como os arquivos não têm mais uma extensão *. cshtml* , eles não serão usados pelo tempo de execução do MVC do ASP.net para renderizar o modo de exibição de *marcas* .

Instale o pacote NuGet *jQuery. Mobile. Mvc* fazendo isso:

1. No menu **ferramentas** , selecione **Gerenciador de pacotes NuGet**e, em seguida, selecione **console do Gerenciador de pacotes**.

    [![p3_packageMgr](aspnet-mvc-4-mobile-features/_static/image20.png)](aspnet-mvc-4-mobile-features/_static/image19.png)
2. No **console do Gerenciador de pacotes**, digite `Install-Package jQuery.Mobile.MVC -version 1.0.0`

A imagem a seguir mostra os arquivos adicionados e alterados para o projeto MvcMobile pelo pacote NuGet jQuery. Mobile. MVC. Os arquivos adicionados têm [adicionar] anexados após o nome do arquivo. A imagem não mostra os arquivos GIF e PNG adicionados à pasta *Content\images* .

![](aspnet-mvc-4-mobile-features/_static/image21.png)

O pacote NuGet jQuery. Mobile. MVC instala o seguinte:

- O *aplicativo\_arquivo Start\BundleMobileConfig.cs* , que é necessário para fazer referência aos arquivos JavaScript e CSS do jQuery adicionados. Você deve seguir as instruções abaixo e fazer referência ao pacote móvel definido neste arquivo.
- arquivos CSS móveis jQuery.
- Um widget do controlador de `ViewSwitcher` (*Controllers\ViewSwitcherController.cs*).
- arquivos JavaScript do jQuery Mobile.
- Um arquivo de layout com estilo móvel do jQuery (*Views\Shared\\_Layout. Mobile. cshtml*).
- Uma exibição parcial do seletor de exibição *(MvcMobile\Views\Shared\\_ViewSwitcher. cshtml*) que fornece um link na parte superior de cada página para alternar da exibição da área de trabalho para a exibição móvel e vice-versa.
- Vários arquivos de imagem<em>. png</em> e <em>. gif</em> na pasta <em>Content\images</em>

Abra o arquivo *global. asax* e adicione o código a seguir como a última linha do método de `Application_Start`.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample10.cs)]

O código a seguir mostra o arquivo *global. asax* completo.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample11.cs?highlight=26)]

> [!NOTE]
> Se você estiver usando o Internet Explorer 9 e não vir a linha de `BundleMobileConfig` acima em destaque amarelo, clique no [botão modo de exibição de compatibilidade](https://windows.microsoft.com/windows7/How-to-use-Compatibility-View-in-Internet-Explorer-9)![imagem do botão exibição de compatibilidade (desativado)](https://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "Imagem do botão modo de exibição de compatibilidade (desativado)") no IE para fazer com que o ícone mude de uma ![imagem de estrutura de tópicos do botão de exibição de compatibilidade (desativado)](https://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "Imagem do botão modo de exibição de compatibilidade (desativado)") para uma imagem de cor sólida ![do botão de exibição de compatibilidade (ativado)](https://res1.windows.microsoft.com/resbox/en/Windows 7/main/156805ff-3130-481b-a12d-4d3a96470f36_14.jpg "Imagem do botão modo de exibição de compatibilidade (ativado)"). Como alternativa, você pode exibir este tutorial no FireFox ou no Chrome.

Abra o arquivo *MvcMobile\Views\Shared\\_Layout. Mobile. cshtml* e adicione a seguinte marcação diretamente após a chamada de `Html.Partial`:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample12.cshtml)]

O arquivo completo *MvcMobile\Views\Shared\\_Layout. Mobile. cshtml* é mostrado abaixo:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample13.cshtml)]

Compile o aplicativo e, no emulador do navegador móvel, navegue até o modo de exibição de *marcas* . Você verá o seguinte:

[![p3_afterNuGet](aspnet-mvc-4-mobile-features/_static/image23.png)](aspnet-mvc-4-mobile-features/_static/image22.png)

> [!NOTE]
> Você pode depurar o código específico para dispositivos móveis [definindo a cadeia de caracteres do agente do usuário](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) para IE ou Chrome para iPhone e, em seguida, usando as ferramentas de desenvolvedor do F-12. Se seu navegador móvel não exibir os links **página inicial**, **orador**, **marca**e **Data** como botões, as referências a scripts móveis jQuery e arquivos CSS provavelmente não estão corretas.

Além das alterações de estilo, você vê exibir **modo de exibição móvel** e um link que permite alternar da exibição móvel para a área de trabalho. Escolha o link **exibição da área de trabalho** e a exibição da área de trabalho é exibida.

[![p3_desktopView](aspnet-mvc-4-mobile-features/_static/image25.png)](aspnet-mvc-4-mobile-features/_static/image24.png)

O modo de exibição de área de trabalho não fornece uma maneira de navegar diretamente para a exibição móvel. Você corrigirá isso agora. Abra o arquivo *Views\Shared\\_Layout. cshtml* . Logo abaixo da página `body` elemento, adicione o seguinte código, que renderiza o widget de seletor de exibição:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample14.cshtml)]

Atualize o modo de exibição de *marcas* no navegador móvel. Agora você pode navegar entre exibições de desktop e móveis.

[![p3_desktopViewWithMobileLink](aspnet-mvc-4-mobile-features/_static/image27.png)](aspnet-mvc-4-mobile-features/_static/image26.png)

> [!NOTE]
> Observação de depuração: você pode adicionar o seguinte código ao final do Views\Shared\\_ViewSwitcher. cshtml para ajudar a depurar exibições ao usar um navegador da cadeia de caracteres do agente do usuário definida para um dispositivo móvel.
>
> [!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample15.cs)]
>
> e adicionando o seguinte cabeçalho ao arquivo *Views\Shared\\_Layout. cshtml* .
>
> [!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample16.html)]

Navegue até a página do @ *Tags* em um navegador da área de trabalho. O widget de seletor de exibição não é exibido em um navegador de desktop porque ele é adicionado somente à página de layout móvel. Posteriormente no tutorial, você verá como adicionar o widget de seletor de exibição à exibição de área de trabalho.

[![p3_desktopBrowser](aspnet-mvc-4-mobile-features/_static/image29.png)](aspnet-mvc-4-mobile-features/_static/image28.png)

## <a name="improving-the-speakers-list"></a>Aprimorando a lista de alto-falantes

No navegador móvel, selecione o link **alto-falantes** . Como não há nenhum modo de exibição móvel (todos os*palestrantes. Mobile. cshtml* *),* a exibição de alto-falantes padrão é renderizada usando o modo de exibição de layout móvel ( *\_layout. Mobile. cshtml*).

[![p3_speakersDeskTop](aspnet-mvc-4-mobile-features/_static/image31.png)](aspnet-mvc-4-mobile-features/_static/image30.png)

Você pode desabilitar globalmente uma exibição padrão (não móvel) da renderização dentro de um layout móvel, definindo `RequireConsistentDisplayMode` como `true` nas *exibições\\arquivo _ViewStart. cshtml* , desta forma:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample17.cshtml)]

Quando `RequireConsistentDisplayMode` é definido como `true`, o layout móvel (<em>\_layout. Mobile. cshtml</em>) é usado somente para exibições móveis. (Ou seja, o arquivo de exibição tem o formato <em>* * ViewName</em><em>. Mobile. cshtml</em>.) talvez você queira definir `RequireConsistentDisplayMode` como `true` se o seu layout móvel não funcionar bem com suas exibições não móveis. A captura de tela abaixo mostra como a página de <em>alto-falantes</em> é renderizada quando `RequireConsistentDisplayMode` é definida como `true`.

[![p3_speakersConsistent](aspnet-mvc-4-mobile-features/_static/image33.png)](aspnet-mvc-4-mobile-features/_static/image32.png)

Você pode desabilitar o modo de exibição consistente em uma exibição definindo `RequireConsistentDisplayMode` como `false` no arquivo de exibição. A seguinte marcação no arquivo *Views\Home\AllSpeakers.cshtml* define `RequireConsistentDisplayMode` como `false`:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample18.cshtml)]

## <a name="creating-a-mobile-speakers-view"></a>Criando uma exibição de alto-falantes móveis

Como você acabou de ver, a exibição dos *alto-falantes* é legível, mas os links são pequenos e são difíceis de tocar em um dispositivo móvel. Nesta seção, você criará um modo de exibição de *alto-falantes* específicos para dispositivos móveis que se parece com um aplicativo móvel moderno – ele exibe links grandes e fáceis de tocar e contém uma caixa de pesquisa para encontrar rapidamente os alto-falantes.

Copie os seus *Enspeakrs. cshtml* para os seus *enspeakrs. Mobile. cshtml*. Abra o arquivo de todos os *Enfalers. Mobile. cshtml* e remova o elemento de cabeçalho `<h2>`.

Na marca `<ul>`, adicione o atributo `data-role` e defina seu valor como `listview`. Assim como outros [atributos de`data-*`](http://html5doctor.com/html5-custom-data-attributes/), `data-role="listview"` torna mais fácil tocar os itens de lista grande. É assim que a marcação completa é semelhante a:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample19.cshtml)]

Atualize o navegador móvel. A exibição atualizada é parecida com esta:

[![p3_updatedSpeakerView1](aspnet-mvc-4-mobile-features/_static/image35.png)](aspnet-mvc-4-mobile-features/_static/image34.png)

Embora o modo de exibição móvel tenha melhorado, é difícil navegar pela longa lista de alto-falantes. Para corrigir isso, na marca `<ul>`, adicione o atributo `data-filter` e defina-o como `true`. O código a seguir mostra a marcação de `ul`.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample20.html)]

A imagem a seguir mostra a caixa de filtro de pesquisa na parte superior da página que resulta do atributo `data-filter`.

[![ps_Data_Filter](aspnet-mvc-4-mobile-features/_static/image37.png)](aspnet-mvc-4-mobile-features/_static/image36.png)

À medida que você digita cada letra na caixa de pesquisa, o jQuery Mobile filtra a lista exibida, conforme mostrado na imagem abaixo.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image39.png)](aspnet-mvc-4-mobile-features/_static/image38.png)

## <a name="improving-the-tags-list"></a>Melhorando a lista de marcas

Como a exibição padrão dos *alto-falantes* , o modo de exibição de *marcas* é legível, mas os links são pequenos e difíceis de tocar em um dispositivo móvel. Nesta seção, você corrigirá o modo de exibição de *marcas* da mesma maneira que corrigiu o modo de exibição de *alto-falantes* .

Remova o &quot;ocultar&quot; sufixo para o arquivo *Views\Home\AllTags.Mobile.cshtml.Hide* para que o nome seja *Views\Home\AllTags.Mobile.cshtml*. Abra o arquivo renomeado e remova o elemento `<h2>`.

Adicione os atributos `data-role` e `data-filter` à marca `<ul>`, conforme mostrado aqui:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample21.html)]

A imagem abaixo mostra a filtragem de página de marcas na letra `J`.

[![p3_tags_J](aspnet-mvc-4-mobile-features/_static/image41.png)](aspnet-mvc-4-mobile-features/_static/image40.png)

## <a name="improving-the-dates-list"></a>Melhorando a lista de datas

Você pode melhorar a exibição de *datas* , como você melhorou as exibições de *alto-falantes* e *marcas* , para que seja mais fácil de usar em um dispositivo móvel.

Copie o arquivo *Views\Home\AllDates.cshtml* para *Views\Home\AllDates.Mobile.cshtml*. Abra o novo arquivo e remova o elemento `<h2>`.

Adicione `data-role="listview"` à marca de `<ul>`, desta forma:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample22.html)]

A imagem abaixo mostra a aparência da página de **Data** com o atributo `data-role` em vigor.

[![p3_dates1](aspnet-mvc-4-mobile-features/_static/image43.png)](aspnet-mvc-4-mobile-features/_static/image42.png) Substitua o conteúdo do arquivo *Views\Home\AllDates.Mobile.cshtml* pelo código a seguir:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample23.cshtml)]

Esse código agrupa todas as sessões por dias. Ele cria um divisor de lista para cada novo dia e lista todas as sessões para cada dia em um divisor. Esta é a aparência quando este código é executado:

[![p3_dates2](aspnet-mvc-4-mobile-features/_static/image45.png)](aspnet-mvc-4-mobile-features/_static/image44.png)

## <a name="improving-the-sessionstable-view"></a>Aprimorando a exibição de sessões

Nesta seção, você criará uma exibição de sessões específica para dispositivos móveis. As alterações que faremos serão mais abrangentes do que em outros modos de exibição que criamos.

No navegador móvel, toque no botão **do orador** e, em seguida, digite `Sc` na caixa de pesquisa.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image47.png)](aspnet-mvc-4-mobile-features/_static/image46.png)

Toque no link **Scott Hanselman** .

[![p3_scottHa](aspnet-mvc-4-mobile-features/_static/image49.png)](aspnet-mvc-4-mobile-features/_static/image48.png)

Como você pode ver, a exibição é difícil de ler em um navegador móvel. A coluna de data é difícil de ler e a coluna marcas está fora da exibição. Para corrigir isso, copie *Views\Home\SessionsTable.cshtml* para *Views\Home\SessionsTable.Mobile.cshtml*e, em seguida, substitua o conteúdo do arquivo pelo código a seguir:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample24.cshtml)]

O código remove as colunas Room e Tags e formata o título, o orador e a data verticalmente, para que todas essas informações possam ser lidas em um navegador móvel. A imagem abaixo reflete as alterações de código.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image51.png)](aspnet-mvc-4-mobile-features/_static/image50.png)

## <a name="improving-the-sessionbycode-view"></a>Aprimorando a exibição do SessionByCode

Por fim, você criará uma exibição específica para celular da exibição *SessionByCode* . No navegador móvel, toque no botão **do orador** e, em seguida, digite `Sc` na caixa de pesquisa.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image53.png)](aspnet-mvc-4-mobile-features/_static/image52.png)

Toque no link **Scott Hanselman** . As sessões de Scott Hanselman são exibidas.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image55.png)](aspnet-mvc-4-mobile-features/_static/image54.png)

Escolha **uma visão geral do link de amor do MS Web Stack** .

[![ps_love](aspnet-mvc-4-mobile-features/_static/image57.png)](aspnet-mvc-4-mobile-features/_static/image56.png)

O modo de exibição de área de trabalho padrão é bom, mas você pode aprimorá-lo.

Copie o *Views\Home\SessionByCode.cshtml* para *Views\Home\SessionByCode.Mobile.cshtml* e substitua o conteúdo do arquivo *Views\Home\SessionByCode.Mobile.cshtml* pela seguinte marcação:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample25.cshtml)]

A nova marcação usa o atributo `data-role` para melhorar o layout da exibição.

Atualize o navegador móvel. A imagem a seguir reflete as alterações de código que você acabou de fazer:

[![p3_love2](aspnet-mvc-4-mobile-features/_static/image59.png)](aspnet-mvc-4-mobile-features/_static/image58.png)

## <a name="wrapup-and-review"></a>Wrapup e revisão

Este tutorial introduziu os novos recursos móveis do ASP.NET MVC 4 Developer Preview. Os recursos móveis incluem:

- A capacidade de substituir layout, exibições e exibições parciais, tanto globalmente quanto para uma exibição individual.
- Controle sobre o layout e a imposição de substituição parcial usando a propriedade `RequireConsistentDisplayMode`.
- Um widget de seletor de exibição para exibições móveis do que também pode ser exibido em exibições de área de trabalho.
- Suporte para navegadores específicos de suporte, como o navegador do iPhone.

## <a name="see-also"></a>Consulte também

- site do [jQuery Mobile](http://jquerymobile.com) .
- [Visão geral do jQuery Mobile](http://jquerymobile.com/demos/1.0b3/docs/about/intro.html)
- [Práticas recomendadas do aplicativo Web móvel de recomendação do W3C](http://www.w3.org/TR/mwabp/)
- [Recomendação do W3C candidata para consultas de mídia](http://www.w3.org/TR/css3-mediaqueries/)
