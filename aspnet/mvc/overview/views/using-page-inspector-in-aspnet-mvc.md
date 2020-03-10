---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: Usando Inspetor de Página no MVC ASP.NET | Microsoft Docs
author: rick-anderson
description: Inspetor de Página no Visual Studio 2012 é uma ferramenta de desenvolvimento para a Web com um navegador integrado. Selecione qualquer elemento no navegador integrado e Inspetor de Página i...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 5da3e142c52a770f59222c21d9f9a53cbbdbf498
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78538013"
---
# <a name="using-page-inspector-in-aspnet-mvc"></a>Usando o Inspetor de Página no ASP.NET MVC

por Tim Ammann

> Inspetor de Página no Visual Studio 2012 é uma ferramenta de desenvolvimento para a Web com um navegador integrado. Selecione qualquer elemento no navegador integrado e Inspetor de Página realça instantaneamente a origem e o CSS do elemento. Você pode procurar qualquer modo de exibição MVC, localizar rapidamente as fontes de marcação renderizada e usar as ferramentas de navegador diretamente no ambiente do Visual Studio.
> 
> [Assista ao vídeo](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> Este tutorial mostra como habilitar Modo de Inspeção e, em seguida, localizar e editar rapidamente a marcação e o CSS em seu projeto Web. O tutorial usa um projeto MVC, mas você também pode usar Inspetor de Página para [Web Forms](https://go.microsoft.com/?linkid=9802001) e outros aplicativos ASP.net.
> 
> O tutorial tem as seguintes seções:
> 
> - [Pré-requisitos](#_1_prerequisites)
> - [Criar um aplicativo Web](#_2_creating_a)
> - [Usar Inspetor de Página para navegar até um modo de exibição](#_3_using_page)
> - [Habilitar Modo de Inspeção](#_4_inspection_mode)
> - [Usar Inspetor de Página para fazer alterações na marcação](#_5_using_page)
> - [Modo de Inspeção e a janela HTML](#_6_inspection_mode)
> - [Visualizar alterações de CSS na janela estilos](#_7_previewing_css)
> - [Sincronização automática de CSS](#css_auto_sync)
> - [Usando o seletor de cores CSS](#css_color_picker)
> - [Mapeando elementos de página dinâmica para JavaScript](#map_dynamic_elements)

<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>Prerequisites

- [Visual Studio 2012](https://www.microsoft.com/visualstudio/11) ou [Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).

> [!NOTE]
> Para obter a versão mais recente do Inspetor de Página, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) para instalar o SDK do Windows Azure para .NET 2,0.

Inspetor de Página é agrupado com Microsoft Web Developer Tools. A versão mais recente é 1,3. Para verificar qual versão você tem, execute o Visual Studio e selecione **sobre Microsoft Visual Studio** no menu **ajuda** .

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>Criar um aplicativo Web

Primeiro, crie um aplicativo Web com o qual você usará Inspetor de Página. No Visual Studio, escolha **arquivo** &gt; **novo projeto**. À esquerda, expanda **Visual C#** , selecione **Web**e, em seguida, selecione **ASP.net MVC4 aplicativo Web**.

![Novo aplicativo MVC ASP.NET](using-page-inspector-in-aspnet-mvc/_static/image2.png)

Clique em **OK**.

Na caixa de diálogo **novo projeto do ASP.NET MVC 4** , selecione **aplicativo de Internet**. Deixe o **Razor** como o mecanismo de exibição padrão.

![Novo projeto MVC do ASP.NET – aplicativo de Internet](using-page-inspector-in-aspnet-mvc/_static/image4.png)

O aplicativo é aberto no modo de exibição de **código-fonte** .

![Novo aplicativo MVC ASP.NET na exibição de origem](using-page-inspector-in-aspnet-mvc/_static/image6.png)

Agora que você tem um aplicativo com o qual trabalhar, você pode usar Inspetor de Página para examiná-lo e modificá-lo.

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a>Usar Inspetor de Página para navegar até um modo de exibição

No Visual Studio 2012, você pode clicar com o botão direito do mouse em qualquer exibição em seu projeto, selecionar **Exibir no Inspetor de página**e Inspetor de página irá descobrir a rota e exibir a página.

Em **Gerenciador de soluções**, expanda a pasta **exibições** e a pasta **base** . Clique com o botão direito do mouse no arquivo index. cshtml e escolha **Exibir no Inspetor de página**.

![Exibir index. cshtml no Inspetor de Página](using-page-inspector-in-aspnet-mvc/_static/image8.png)

Por padrão, Inspetor de Página é encaixada como uma janela no lado esquerdo do ambiente do Visual Studio. Se preferir, você pode encaixá-lo em outro lugar ou desencaixar a janela. Consulte [como: organizar e encaixar janelas](https://msdn.microsoft.com/library/z4y0hsax.aspx).

O painel superior da janela de Inspetor de Página mostra a página atual em uma janela do navegador. O painel inferior mostra a página na marcação HTML, juntamente com algumas guias que permitem inspecionar diferentes aspectos da página. O painel inferior é semelhante ao [ferramentas para desenvolvedores F12](https://msdn.microsoft.com/ie/aa740478) no Internet Explorer.

![Aplicativo ASP.NET MVC no Inspetor de Página](using-page-inspector-in-aspnet-mvc/_static/image10.png)

Neste tutorial, você usará as guias **HTML** e **estilos** para navegar rapidamente e fazer alterações no aplicativo.

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a>Modo de EnableInspection

Para colocar Inspetor de Página em Modo de Inspeção, clique no botão **inspecionar** . Em Modo de Inspeção, quando você mantém o ponteiro do mouse sobre qualquer parte da página renderizada, a marcação de origem ou o código correspondente é realçado.

![Alternar Modo de Inspeção](using-page-inspector-in-aspnet-mvc/_static/image12.png)

Agora, mova o mouse sobre diferentes partes da página dentro Inspetor de Página. Como você faz, o ponteiro do mouse muda para um sinal de adição grande e o elemento abaixo é realçado:

![Passando o mouse sobre div. Content-Wrapper](using-page-inspector-in-aspnet-mvc/_static/image14.png)

À medida que você move o ponteiro do mouse, o Visual Studio realça o sintaxe Razor correspondente no arquivo de origem. Se o elemento HTML vier de outro arquivo de origem, o Visual Studio abrirá automaticamente o arquivo.

No Inspetor de Página, a guia **HTML** mostra o HTML que foi gerado a partir do sintaxe Razor. À medida que você move o ponteiro do mouse, os elementos HTML são realçados. A guia **estilos** mostra as regras de CSS para o elemento.

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>Usar Inspetor de Página para fazer alterações na marcação

Inspetor de Página permite encontrar marcação cujo local pode não ser óbvio. Em seguida, você pode modificar a marcação e ver as alterações resultantes.

Para ver isso, clique em **inspecionar** e role até a parte inferior da página na janela Inspetor de página.

Quando você move o ponteiro do mouse para a área de rodapé, Inspetor de Página abre o arquivo \_layout. cshtml e realça a seção da página de layout que você selecionou. Como você pode ver, o rodapé é definido no arquivo de layout e não na exibição em si.

![Rodapé](using-page-inspector-in-aspnet-mvc/_static/image16.png)

Agora, mova o ponteiro do mouse sobre a linha com <a id="a"> </a>o aviso de direitos autorais. Na página \_layout. cshtml, a linha correspondente é realçada.

![Linha de copyright de rodapé realçada](using-page-inspector-in-aspnet-mvc/_static/image18.png)

Adicione texto ao final da linha no arquivo layout. cshtml do \_.

&lt;p&gt;&amp;cópia; @DateTime.Now.Year-meu aplicativo MVC ASP.NET Rocks!&lt;/p&gt;

Agora, pressione Ctrl + Alt + Enter ou clique na barra de atualização para ver os resultados na janela do navegador Inspetor de Página.

![Meu aplicativo ASP.NET Rocks!](using-page-inspector-in-aspnet-mvc/_static/image20.png)

Talvez você tenha pensado no rodapé definido em index. cshtml, mas ele se tornou no \_layout. cshtml e Inspetor de Página encontrá-lo para você.

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>Modo de Inspeção e a janela HTML

Em seguida, você terá uma visão rápida da janela HTML e de como ela mapeia os elementos para você.

Clique em **inspecionar** para colocar Inspetor de Página em modo de inspeção.

Clique na parte superior da página que diz "seu logotipo aqui". Você está examinando um elemento específico em mais detalhes, portanto, a exibição na janela do navegador não é mais alterada à medida que você move o ponteiro do mouse.

Agora, mova o ponteiro do mouse para a janela **HTML** . À medida que você move o ponteiro do mouse, Inspetor de Página descreve o elemento dentro da janela **HTML** e realça o elemento correspondente na janela do navegador.

![Janela HTML](using-page-inspector-in-aspnet-mvc/_static/image22.png)

Como antes, Inspetor de Página abre o arquivo \_layout. cshtml para você em uma guia temporária. Clique na guia temporário do \_layout. cshtml e a marcação correspondente será realçada na seção&gt; do cabeçalho de &lt;para você:

![Marcação realçada](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>Visualizar alterações de CSS na janela estilos

Em seguida, você usará a janela **estilos** de Inspetor de página para visualizar as alterações no CSS.

Clique em **inspecionar** para colocar Inspetor de Página em modo de inspeção.

Na janela do navegador Inspetor de Página, mova o ponteiro do mouse sobre a seção "Home Page" até que o rótulo **div. Content-wrapper** seja exibido.

![Passando o mouse sobre div. Content-Wrapper](using-page-inspector-in-aspnet-mvc/_static/image26.png)

Clique na seção div. Content-wrapper uma vez e mova o ponteiro do mouse para a janela **estilos** . A janela **estilos** mostra todas as regras de CSS para este elemento. Role para baixo até localizar o seletor de classe. em destaque. Content-wrapper. Agora desmarque a caixa de seleção da propriedade Background-Color.

![Limpar cor da fundo](using-page-inspector-in-aspnet-mvc/_static/image28.png)

Observe como as visualizações de alteração são instantaneamente na janela Inspetor de Página navegador.

Marque a caixa de seleção novamente, clique duas vezes no valor da propriedade e altere-o para vermelho. A alteração mostra imediatamente:

![Cor de fundo vermelha](using-page-inspector-in-aspnet-mvc/_static/image30.png)

A janela **estilos** facilita o teste e a visualização das alterações de CSS antes de você confirmar as alterações na própria folha de estilos.

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>Sincronização automática de CSS

> [!NOTE]
> Este recurso requer a versão 1,3 de Inspetor de Página.

O recurso de sincronização automática de CSS permite que você edite um arquivo CSS diretamente e veja as alterações imediatamente no navegador Inspetor de Página.

Clique em **inspecionar** para colocar Inspetor de Página em modo de inspeção.

No navegador Inspetor de Página, mova o ponteiro do mouse sobre a seção "Home Page" até que o rótulo **div. Content-wrapper** seja exibido. Clique em uma vez para selecionar este elemento.

A janela **estilos** mostra todas as regras de CSS para este elemento. Role para baixo até localizar o seletor de classe. em destaque. Content-wrapper. Clique em ". em destaque. Content-wrapper". Inspetor de Página abre o arquivo CSS que define esse estilo (site. css) e realça o estilo CSS correspondente.

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

Agora, altere o valor de `background-color` para "vermelho". A alteração aparece imediatamente no navegador Inspetor de Página.

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a>Usando o seletor de cores CSS

O editor de CSS no Visual Studio 2012 tem um seletor de cores que torna mais fácil escolher e inserir cores. O seletor de cores inclui uma paleta padrão de cores, dá suporte a nomes de cores padrão, códigos de hash, cores RGB, RGBA, HSL e HSLA e mantém uma lista das cores que você usou mais recentemente no documento.

Na seção anterior, você alterou o valor da propriedade `background-color`. Para invocar o seletor de cores, coloque o ponto de inserção após o nome da propriedade e digite **#** ou **RGB (** .

![A barra do seletor de cores CSS](using-page-inspector-in-aspnet-mvc/_static/image36.png)

Clique em uma cor para selecioná-la ou pressione a tecla seta para baixo e use as teclas de seta para a esquerda e para a direita para percorrer as cores. Quando você visita uma cor, o valor hexadecimal correspondente é visualizado:

![plano de fundo-valor da propriedade de cor visualizado](using-page-inspector-in-aspnet-mvc/_static/image38.png)

Se a barra de cores não tiver a cor exata desejada, você poderá usar o pop-down seletor de cores. Para abri-lo, clique na divisa dupla na extremidade direita da barra de cores ou pressione a seta para baixo uma ou duas vezes no teclado.

![Pop-down seletor de cores CSS](using-page-inspector-in-aspnet-mvc/_static/image40.png)

Clique em uma cor na barra vertical à direita. Isso mostra um gradiente para essa cor na janela principal. Escolha uma cor diretamente na barra vertical pressionando ENTER ou clique em qualquer ponto na janela principal para escolher com maior precisão.

Se houver uma cor na tela do computador que você deseja usar (ela não precisa estar dentro da interface do usuário do Visual Studio), você poderá capturar seu valor usando a ferramenta de conta-gotas no canto inferior direito.

Você também pode alterar a opacidade de uma cor movendo o controle deslizante na parte inferior do seletor de cores. Isso altera os valores de cor para os valores RGBA, pois o formato RGBA pode representar a opacidade.

Depois de escolher uma cor, pressione Enter e, em seguida, digite um ponto e vírgula para concluir a entrada de cor de fundo no arquivo *site. css* .

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>A barra de atualização de Inspetor de Página

Inspetor de Página detecta imediatamente a alteração no arquivo *site. css* e exibe um alerta em uma barra de atualização.

![Barra de atualização](using-page-inspector-in-aspnet-mvc/_static/image42.png)

Para salvar todos os arquivos e atualizar o navegador Inspetor de Página, pressione Ctrl + Alt + Enter ou clique na barra de atualização. A alteração na cor de realce aparece no navegador.

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a>Mapeando elementos de página dinâmica para JavaScript

Em aplicativos Web modernos, os elementos na página geralmente são gerados dinamicamente com JavaScript. Isso significa que não há marcação estática (HTML ou Razor) que corresponda a esses elementos de página.

Com a versão 1,3, agora Inspetor de Página pode mapear itens que foram adicionados dinamicamente à página de volta para o código JavaScript correspondente. Para demonstrar esse recurso, usaremos o [modelo Spa (aplicativo de página única)](../../../single-page-application/overview/introduction/knockoutjs-template.md).

> [!NOTE]
> O modelo SPA requer a atualização [ASP.NET and Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650) .

No Visual Studio, escolha **arquivo** &gt; **novo projeto**. À esquerda, expanda **Visual C#** , selecione **Web**e, em seguida, selecione **ASP.net MVC4 aplicativo Web**. Clique em **OK**.

Na caixa de diálogo **novo projeto ASP.NET MVC 4** , selecione **aplicativo de página única**.

Em Gerenciador de Soluções, expanda a pasta **exibições** e a pasta **base** . Clique com o botão direito do mouse no arquivo index. cshtml e escolha **Exibir no Inspetor de página**.

A primeira coisa que é exibida no navegador de Inspetor de Página é uma página de logon. Clique em "inscrever-se" e crie um nome de usuário e senha. Depois de se inscrever, o aplicativo o conecta e cria uma lista de tarefas pendentes com alguns itens de exemplo.

Clique em **inspecionar** para colocar Inspetor de Página em modo de inspeção. No navegador Inspetor de Página, clique em um dos itens de tarefas. Observe que, em vez de ser realçado em azul, o elemento é realçado em laranja, com "JS" ao lado do nome do elemento. Isso indica que o elemento foi criado dinamicamente por meio de script.

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

Além disso, um sublinhado laranja aparece na guia **pilha de chamadas** . Isso indica que o painel **pilha de chamadas** tem mais informações sobre o elemento.

Clique na guia **pilha de chamadas** . O painel **pilha de chamadas** mostra a pilha de chamadas para a chamada JavaScript que criou o elemento. Chamadas para bibliotecas externas, como jQuery, são recolhidas, para que você possa ver facilmente as chamadas para o script do aplicativo.

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

Para ver a pilha completa, incluindo chamadas para bibliotecas externas, você pode expandir os nós rotulados como "bibliotecas externas":

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

Se você clicar em um item na pilha de chamadas, o Visual Studio abrirá o arquivo de código e destacará o script correspondente.

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a>Consulte Também

[Introdução ao ASP.NET MVC 4 com o Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (site do ASP.net)

[Introdução ao inspetor de página](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (vídeo do Channel 9)

[Inspetor de página mensagens de erro](https://go.microsoft.com/?linkid=9813062) (MSDN)
