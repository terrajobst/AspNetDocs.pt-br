---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: Usando o Inspetor de Página para o Visual Studio 2012 no ASP.NET Web Forms | Microsoft Docs
author: rick-anderson
description: O Inspetor de Página para Visual Studio 2012 é uma ferramenta de desenvolvimento para a Web com um navegador integrado. Selecione qualquer elemento no navegador integrado e Inspetor de Página...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: c165bbea505b4cb8eae1312cdd587f4ed36541a0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78575918"
---
# <a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a>Uso do Inspetor de Página para Visual Studio 2012 em Web Forms do ASP.NET

por Tim Ammann

> O Inspetor de Página para Visual Studio 2012 é uma ferramenta de desenvolvimento para a Web com um navegador integrado. Selecione qualquer elemento no navegador integrado e Inspetor de Página realça instantaneamente a origem e o CSS do elemento. Você pode procurar qualquer página em seu aplicativo, localizar rapidamente as fontes de marcação renderizada e usar as ferramentas de navegador diretamente no ambiente do Visual Studio.
> 
> Este tutorial mostra como habilitar Modo de Inspeção e, em seguida, localizar e editar rapidamente as regras e o texto do CSS em seu projeto Web. O tutorial usa um projeto de aplicativo Web Forms, mas você também pode usar Inspetor de Página para projetos de site e aplicativos [MVC](https://go.microsoft.com/?linkid=9802002) .
> 
> O tutorial tem as seguintes seções:
> 
> [Pré-requisitos](#_1_prerequisites)
> 
> [Criar um aplicativo Web](#_2_creating_a)
> 
> [Usar Inspetor de Página para exibir o aplicativo](#_3_using_page)
> 
> [Habilitar Modo de Inspeção](#_4_inspection_mode)
> 
> [Usar Inspetor de Página para fazer alterações na marcação](#_5_using_page)
> 
> [Modo de Inspeção e a janela HTML](#_6_inspection_mode)
> 
> [Visualizar alterações de CSS na janela estilos](#_7_previewing_css)
> 
> [Sincronização automática de CSS](#css_auto_sync)
> 
> [Usando o seletor de cores CSS](#css_color_picker)

<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>Prerequisites

- [Visual Studio 2012](https://www.microsoft.com/visualstudio/11) ou [Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).

> [!NOTE]
> Para obter a versão mais recente do Inspetor de Página, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) para instalar o SDK do Azure para .NET 2,0.

Inspetor de Página é agrupado com Microsoft Web Developer Tools. A versão mais recente é 1,3. Para verificar qual versão você tem, execute o Visual Studio e selecione **sobre Microsoft Visual Studio** no menu **ajuda** .

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>Criar um aplicativo Web

Primeiro, você criará um aplicativo Web com o qual usará Inspetor de Página. No Visual Studio, escolha **arquivo** &gt; **novo projeto**. À esquerda, expanda **Visual C#** , selecione **Web**e, em seguida, selecione **ASP.NET Web Forms aplicativo**.

![Novo aplicativo Web Forms](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

Clique em **OK**.

O aplicativo é aberto no modo de exibição de **código-fonte** .

![Novo aplicativo Web Forms no modo de exibição de origem](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

Agora que você tem um aplicativo com o qual trabalhar, você pode usar Inspetor de Página para examiná-lo e modificá-lo.

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a>Usar Inspetor de Página para exibir o aplicativo

Em seguida, você exibirá o aplicativo com Inspetor de Página. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e escolha **Exibir no Inspetor de página**.

![Exibir em inspetor de página](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

Por padrão, quando Inspetor de Página é iniciado pela primeira vez, ele é encaixado como uma janela estreita no lado esquerdo do ambiente do Visual Studio. Deixe-o encaixado no lado esquerdo e defina-o com uma largura que seja confortável para você ou encaixe-o em uma das áreas de ferramentas na parte superior, inferior ou direita:

![Inspetor de Página posições de encaixe](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

Se você desencaixar a janela Inspetor de Página, poderá colocá-la fora do Visual Studio ou até mesmo em um segundo monitor, se tiver uma. No entanto, para ALT + TAB entre Inspetor de Página e o Visual Studio quando a janela de Inspetor de Página estiver desencaixada, vá para **ferramentas** &gt; **opções** &gt; **ambiente** &gt; **guias e janelas**e, em bem com a **guia**, desmarque a caixa de seleção chamada **Windows de ferramenta flutuante sempre fique na parte superior da janela principal**:

![Desmarque a caixa de seleção janelas de ferramentas flutuantes para ALT + TAB entre o Visual Studio e a janela de Inspetor de Página desencaixada](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

O painel superior da janela de Inspetor de Página mostra a página atual em uma janela do navegador. O painel inferior mostra a página na marcação HTML à esquerda e algumas guias à direita que permitem inspecionar diferentes aspectos da página. O painel inferior é semelhante ao [ferramentas para desenvolvedores F12](https://msdn.microsoft.com/ie/aa740478) no Internet Explorer. (No entanto, ao contrário das ferramentas de desenvolvedor, você pode usar Inspetor de Página diretamente no Visual Studio.)

![{1&gt;Inspetor de Página&lt;1}](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

Neste tutorial, você usará o painel navegador Inspetor de Página e as guias **HTML** e **estilos** para ajudá-lo a navegar rapidamente e fazer alterações no aplicativo.

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a>Habilitar Modo de Inspeção

Em seguida, você verá como o Modo de Inspeção de Inspetor de Página funciona. Na janela Inspetor de Página, clique no botão **inspecionar** .

![Inspecionar elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

Para ver o modo de inspeção em ação, mova o mouse sobre diferentes partes da página dentro da janela Inspetor de Página navegador. Como você faz, o ponteiro do mouse muda para um sinal de adição grande e o elemento abaixo é realçado:

![Passando o mouse sobre div. Content-Wrapper](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

À medida que você move o ponteiro do mouse, observe que

- O conteúdo no modo de exibição de **origem** é alterado para mostrar a marcação correspondente ao elemento selecionado na página. A marcação relevante é realçada. Se a origem estiver em outro arquivo, esse arquivo será aberto no modo de exibição de origem com a marcação relevante realçada.

- A marcação exibida na guia **HTML** em Inspetor de página também é alterada para corresponder ao elemento selecionado na página. Na guia **HTML** , a marcação relevante é descrita.

- A guia **estilos** mostra as regras de CSS relevantes para a seleção atual.

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>Usar Inspetor de Página para fazer alterações na marcação

Agora, você verá como você pode usar Inspetor de Página para localizar e fazer alterações na marcação ou texto cujo local pode não ser imediatamente óbvio.

Coloque Inspetor de Página em Modo de Inspeção e, em seguida, role até a parte inferior da home page.

Assim que você inserir a área de rodapé, Inspetor de Página abrirá o arquivo de layout *site. Master* na exibição de **origem** em uma guia temporária à direita das outras guias e destacará a seção da página mestra que você selecionou. Isso mostra como Inspetor de Página pode localizar e exibir conteúdo em uma página que pode realmente vir de um arquivo diferente daquele que você abriu originalmente.

![Realces de rodapé no Modo de Inspeção](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

Na janela do navegador Inspetor de Página, mova o ponteiro do mouse sobre a linha com o <a id="a"> </a>aviso de direitos autorais.

Na página *site. Master* , a linha correspondente é realçada.

![Linha de copyright de rodapé realçada](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

Adicione texto ao final da linha no arquivo *site. Master* .

&lt;p&gt;&amp;cópia; &lt;%: DateTime. Now. ano%&gt;-meu aplicativo ASP.NET Rocks!&lt;/p&gt;

Agora, pressione Ctrl + Alt + Enter ou clique na barra de atualização para ver os resultados na janela do navegador Inspetor de Página.

![Meu aplicativo ASP.NET Rocks!](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

Talvez você tenha pensado que o rodapé estava na página *Default. aspx* , mas que ele se tornou na página de layout mestre e Inspetor de página encontrá-lo para você.

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>Modo de Inspeção e a janela HTML

Em seguida, você terá uma visão rápida da janela HTML e de como ela mapeia os elementos para você.

Coloque Inspetor de Página em Modo de Inspeção.

![Inspecionar elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

Clique na parte superior da página que diz "seu logotipo aqui". Você está examinando um elemento específico em mais detalhes, portanto, a exibição na janela do navegador não é mais alterada à medida que você move o ponteiro do mouse.

Agora, mova o ponteiro do mouse para a janela **HTML** . À medida que você move o ponteiro do mouse, Inspetor de Página descreve o elemento dentro da janela **HTML** e realça o elemento correspondente na janela do navegador.

![Janela HTML](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

Como antes, Inspetor de Página abre o arquivo *site. Master* para você em uma guia temporária. Clique na guia site. Master e a marcação correspondente será realçada no cabeçalho &lt;&gt; seção:

![Marcação realçada](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>Visualizar alterações de CSS na janela estilos

Em seguida, você verá como é possível usar a janela **estilos** de Inspetor de página para visualizar alterações no CSS.

Clique no botão **inspecionar** para colocar Inspetor de Página em modo de inspeção.

Na janela do navegador Inspetor de Página, mova o ponteiro do mouse sobre a seção "Home Page" até que o rótulo **div. Content-wrapper** seja exibido.

![Focalizando elementos](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

Clique na seção div. Content-wrapper uma vez e mova o ponteiro do mouse para a janela **estilos** . No seletor de classe. Content-wrapper, desmarque e marque a caixa de seleção da propriedade Background-Color.

![Limpar cor da fundo](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

Observe como as visualizações de alteração são instantaneamente na janela Inspetor de Página navegador.

Marque a caixa de seleção novamente, clique duas vezes no valor da propriedade e altere-o para `red`. A alteração mostra imediatamente:

![Cor de fundo vermelha](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

A janela **estilos** facilita o teste e a visualização das alterações de CSS antes de você confirmar as alterações na própria folha de estilos.

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>Sincronização automática de CSS

> [!NOTE]
> Este recurso requer a versão 1,3 de Inspetor de Página.

O recurso de sincronização automática de CSS permite que você edite um arquivo CSS diretamente e veja as alterações imediatamente no navegador Inspetor de Página.

Clique em **inspecionar** para colocar Inspetor de Página em modo de inspeção.

No navegador Inspetor de Página, mova o ponteiro do mouse sobre a seção "Home Page" até que o rótulo **div. Content-wrapper** seja exibido. Clique em uma vez para selecionar este elemento.

A janela **estilos** mostra todas as regras de CSS para este elemento. Role para baixo até localizar o seletor de classe. em destaque. Content-wrapper. Clique em ". em destaque. Content-wrapper". Inspetor de Página abre o arquivo CSS que define esse estilo (site. css) e realça o estilo CSS correspondente.

![Arquivo CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

Agora, altere o valor de `background-color` para "vermelho". A alteração aparece imediatamente no navegador Inspetor de Página.

![Inspetor de Página navegador](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a>Usando o seletor de cores CSS

Em seguida, você aprenderá a usar Inspetor de Página para localizar e alterar rapidamente o CSS para texto realçado no aplicativo padrão. Neste exemplo, você decidiu que não gosta do realce azul e deseja alterá-lo para outra cor.

Clique no botão **inspecionar** .

![Inspecionar elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

Na janela do navegador Inspetor de Página, mova o ponteiro do mouse sobre o texto realçado "vídeos, tutoriais e amostras" para que o rótulo de "marca" de CSS seja exibido.

![Passando o mouse sobre o elemento Mark](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

Clique no texto para selecioná-lo. O seletor de marca CSS correspondente aparece na parte inferior da janela **estilos** .

![marcar seletor na janela estilos](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

Clique no seletor de marca. Isso abre o arquivo *site. css* para o aplicativo Web. Clique na guia site. css e o CSS correspondente para o seletor será realçado:

![marcar seletor na folha de estilos](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

Selecione e remova a linha com a propriedade Background-Color.

Agora, você usará o novo seletor de cores CSS do Visual Studio 2012 para escolher uma nova cor para a propriedade **Marcar** cor de segundo plano.

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a>Usando o seletor de cores CSS do Visual Studio 2012

O editor de CSS no Visual Studio 2012 tem um seletor de cores que torna mais fácil escolher e inserir cores. Ele tem uma barra de cores simples e um seletor "pop-down" que oferece controle mais preciso.

O seletor de cores inclui uma paleta padrão de cores, dá suporte a nomes de cores padrão, códigos de hash, cores RGB, RGBA, HSL e HSLA e mantém uma lista das cores que você usou mais recentemente no documento.

Na linha em que a propriedade Background-Color era, digite "BC" e pressione a seta para baixo uma vez.

Quando você digita o primeiro caractere de cada palavra em uma propriedade separada por hífen, como "background-color", o IntelliSense filtra a lista para que você mostre apenas as propriedades correspondentes:

![Valores filtrados do IntelliSense](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

Agora, digite dois-pontos. Quando você fizer isso, o nome completo da propriedade de cor de fundo será inserido. Digite **#** ou **RGB (** e a barra do seletor de cores será exibida:

![A barra do seletor de cores CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

Para ver como a barra do seletor de cores funciona, clique em suas cores com o ponteiro do mouse ou pressione a tecla seta para baixo e use as teclas de seta para a esquerda e para a direita para percorrer as cores. Quando você visita uma cor, o valor correspondente para a propriedade Background-Color é visualizado:

![plano de fundo-valor da propriedade de cor visualizado](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

Neste ponto, você pode pressionar Enter para selecionar o valor e, em seguida, um ponto-e-vírgula (;) para concluir a entrada CSS. Por enquanto, vá para a próxima seção para que você possa ver como o pop-down seletor de cores funciona.

#### <a name="using-the-color-picker-pop-down"></a>Usando o seletor de cores pop-down

Quando a barra de cores não tem a cor exata que você está procurando, você pode usar o pop-down seletor de cores.

Para abri-lo, clique na divisa dupla na extremidade direita da barra de cores ou pressione a seta para baixo uma ou duas vezes no teclado.

![Pop-down seletor de cores CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

Clique em uma cor na barra vertical à direita. Isso mostra um gradiente para essa cor na janela principal. Escolha uma cor diretamente na barra vertical pressionando ENTER ou clique em qualquer ponto na janela principal para escolher com maior precisão.

Se houver uma cor na tela do computador que você deseja usar (ela não precisa estar dentro da interface do usuário do Visual Studio), você poderá capturar seu valor usando a ferramenta de conta-gotas no canto inferior direito.

Você também pode alterar a opacidade de uma cor movendo o controle deslizante na parte inferior do seletor de cores. Isso altera os valores de cor para os valores RGBA porque o formato RGBA pode representar a opacidade.

Depois de escolher uma cor, pressione Enter e, em seguida, digite um ponto e vírgula para concluir a entrada de cor de fundo no arquivo *site. css* .

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>A barra de atualização de Inspetor de Página

Inspetor de Página detecta imediatamente a alteração no arquivo *site. css* (ou em qualquer arquivo no aplicativo) e exibe um alerta em uma barra de atualização.

![Barra de atualização](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

Para salvar todos os arquivos e atualizar o navegador Inspetor de Página, pressione Ctrl + Alt + Enter ou clique na barra de atualização. A alteração na cor de realce aparece no navegador:

![Cor do realce alterada](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a>Observe que você atualizou convenientemente o Inspetor de Página navegador diretamente de dentro do ambiente do Visual Studio. O uso de Inspetor de Página em vez de um navegador externo permite que você permaneça no editor ao desenvolver seus aplicativos Web.

## <a name="see-also"></a>Consulte Também

[Introdução ao inspetor de página](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (vídeo do Channel 9)

[Inspetor de página mensagens de erro](https://go.microsoft.com/?linkid=9813062) (MSDN)
