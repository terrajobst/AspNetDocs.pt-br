---
uid: web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
title: Usando o Inspetor de Página no Visual Studio 2012 | Microsoft Docs
author: rick-anderson
description: Neste laboratório prático, você descobrirá uma nova ferramenta para localizar e corrigir problemas de página da Web no Visual Studio – o Inspetor de Página. Inspetor de Página é uma nova ferramenta que b...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 73232292-a5fe-4720-82a1-8f6553effd1f
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: f42b1be2697ba7d1145b3e334fe8f4ebf019cd12
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78586250"
---
# <a name="using-page-inspector-in-visual-studio-2012"></a>Uso do Inspetor de Página no Visual Studio 2012

por [equipe de acampamentos da Web](https://twitter.com/webcamps)

> Neste laboratório prático, você descobrirá uma nova ferramenta para localizar e corrigir problemas de página da Web no Visual Studio – o Inspetor de Página.
> 
> Inspetor de Página é uma nova ferramenta que traz as ferramentas de diagnóstico do navegador para o Visual Studio e fornece uma experiência integrada entre o navegador, o ASP.NET e o código-fonte. Ele renderiza uma página da Web (HTML, Web Forms, ASP.NET MVC ou páginas da Web) diretamente no IDE do Visual Studio e permite que você examine o código-fonte e a saída resultante. O Inspetor de Página permite decompor facilmente um site, criar páginas rapidamente do zero e diagnosticar rapidamente problemas.
> 
> Hoje temos várias estruturas da Web que criam sites flexíveis e escalonáveis em tempo hábil, como ASP.NET MVC e WebForms. Por outro lado, é mais difícil encontrar problemas nas páginas porque o IDE não oferece suporte à exibição de designer em páginas baseadas em modelo e conteúdo dinâmico. Portanto, esses sites precisam ser abertos em um navegador para ver como eles aparecem para um usuário.
> 
> Os desenvolvedores da Web usam ferramentas externas para encontrar problemas que são executados regularmente no navegador. Em seguida, eles retornam ao IDE e começam a ser corrigidos. Essa atividade de fundo entre o IDE, o navegador e as ferramentas de criação de perfil podem ser ineficientes e, às vezes, requer uma nova implantação e limpeza de cache a cada vez que você desejar reproduzir um problema.
> 
> A Inspetor de Página preenche uma lacuna no desenvolvimento para a Web entre o cliente (ferramentas de navegador) e o servidor (ASP.NET e código-fonte) reunindo o melhor dos dois mundos usando um conjunto combinado de recursos.
> 
> Usando Inspetor de Página, você pode ver quais elementos nos arquivos de origem (incluindo o código do servidor) produziram a marcação HTML a ser renderizada no navegador. Inspetor de Página também permite modificar propriedades de CSS e atributos de elemento DOM para ver as alterações refletidas imediatamente no navegador.
> 
> Este laboratório prático explicará os recursos de Inspetor de Página e mostrará como você pode usá-los para corrigir problemas em aplicativos Web. **Este laboratório contém dois exercícios usando fluxos semelhantes, mas que têm como alvo diferentes tecnologias. Se você for um desenvolvedor ASP.NET MVC, siga um exercício; Se você for um desenvolvedor de WebForms, siga o exercício dois**.
> 
> Este laboratório orienta você pelos aprimoramentos e novos recursos descritos anteriormente, aplicando pequenas alterações a um aplicativo Web de exemplo fornecido na pasta de origem.
> 
> Todos os códigos de exemplo e trechos de código estão incluídos no kit de treinamento do acampamentos da Web, disponível em [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

Neste laboratório prático, você aprenderá a:

- Decompor um site usando Inspetor de Página
- Inspecionar e Visualizar alterações de estilos CSS com Inspetor de Página
- Detectar e corrigir problemas em suas páginas da Web usando Inspetor de Página

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisites

Você deve ter os seguintes itens para concluir este laboratório:

- [Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou superior (leia [o apêndice a](#AppendixA) para obter instruções sobre como instalá-lo).
- Internet Explorer 9 ou superior

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Exercícios

Este laboratório prático inclui os seguintes exercícios:

1. [Exercício 1: usando Inspetor de Página em projetos MVC ASP.NET](#Exercise1)
2. [Exercício 2: usando Inspetor de Página em projetos de WebForms](#Exercise2)

> [!NOTE]
> Cada exercício é acompanhado por uma solução inicial, localizada na pasta Begin do exercício, que permite que você siga cada exercício independentemente dos outros. Dentro do código-fonte de um exercício, você também encontrará uma pasta final contendo uma solução do Visual Studio com o código que resulta da conclusão das etapas no exercício correspondente. Você pode usar essas soluções como diretrizes se precisar de ajuda adicional ao trabalhar com este laboratório prático.

Tempo estimado para concluir este laboratório: **30 minutos**.

<a id="Exercise1"></a>

<a id="Exercise_1_Using_Page_Inspector_in_ASPNET_MVC_Projects"></a>
### <a name="exercise-1-using-page-inspector-in-aspnet-mvc-projects"></a>Exercício 1: usando Inspetor de Página em projetos MVC ASP.NET

Neste exercício, você aprenderá a visualizar e depurar uma solução **ASP.NET MVC 4** usando **Inspetor de página**. Primeiro, você executará um breve colo sobre a ferramenta para aprender os recursos que facilitam o processo de depuração na Web. Em seguida, você trabalhará em uma página da Web que contém problemas de estilo. Você aprenderá a usar Inspetor de Página para localizar o código-fonte que gera o problema e corrigi-lo.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>Tarefa 1-explorando Inspetor de Página

Nesta tarefa, você aprenderá a usar o Inspetor de Página no contexto de um projeto do ASP.NET MVC 4 que mostra uma galeria de fotos.

1. Abra a solução **inicial** localizada na **origem/EX1-MVC4/início/** pasta.

   1. Será necessário baixar alguns pacotes NuGet ausentes antes de continuar. Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.
   2. Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, Compile a solução clicando em **build** | **Compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto. É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.
2. Na Gerenciador de Soluções, localize o modo de exibição **index. cshtml** na pasta do projeto **/views/Home** , clique com o botão direito do mouse nele e selecione **Exibir no Inspetor de página**.

    ![Selecionando um arquivo para visualização no Inspetor de Página](using-page-inspector-in-visual-studio-2012/_static/image1.png "Selecionando um arquivo para visualização no Inspetor de Página")

    *Selecionando um arquivo para visualização no Inspetor de Página*
3. A janela Inspetor de Página mostrará a URL */Home/index* mapeada para a exibição da fonte selecionada.

    ![O primeiro contato com PageInspector](using-page-inspector-in-visual-studio-2012/_static/image2.png)

    *O primeiro contato com Inspetor de Página*

    A ferramenta de Inspetor de Página está integrada ao seu ambiente do Visual Studio. O Inspetor contém um navegador incorporado, junto com um poderoso criador de perfil de HTML. Observe que você não precisa executar a solução para ver a aparência de suas páginas.

    > [!NOTE]
    > Quando a largura do navegador Inspetor de Página for menor que a largura da página aberta, você não verá a página corretamente. Se isso acontecer, ajuste a largura do Inspetor de Página.
4. Clique na guia **arquivos** em Inspetor de página.

    Você verá todos os arquivos de origem que estão compondo a página de índice. Esse recurso ajuda a identificar todos os elementos em um relance, especialmente quando você está trabalhando com exibições e modelos parciais. Observe que você também pode abrir cada um dos arquivos se clicar nos links.

    ![A guia-files](using-page-inspector-in-visual-studio-2012/_static/image3.png)

    *A guia arquivos*
5. Clique no botão **Ativar/desativar modo de inspeção** , localizado à esquerda das guias.

    Essa ferramenta permitirá que você selecione qualquer elemento da página e veja seu código HTML e Razor.

    ![Toggle-Inspection-Mode-button](using-page-inspector-in-visual-studio-2012/_static/image4.png)

    *Botão de alternância Modo de Inspeção*
6. No navegador Inspetor de Página, mova o ponteiro do mouse sobre os elementos da página. Enquanto você move o ponteiro do mouse sobre qualquer parte da página renderizada, o tipo de elemento é exibido e a marcação de origem ou o código correspondente é realçado no editor do Visual Studio.

    ![Modo de inspeção em ação](using-page-inspector-in-visual-studio-2012/_static/image5.png)

    *Modo de inspeção em ação*

    > [!NOTE]
    > Não maximize a janela Inspetor de Página ou você não poderá ver a guia Visualização mostrando o código-fonte. Se você clicar no elemento em Inspetor de Página quando ele for maximizado, o código-fonte da seleção será exibido, mas ocultará a janela de Inspetor de Página.

    Se você prestar atenção ao arquivo **index. cshtml** , observará que a parte do código-fonte que gera o elemento selecionado é realçada. Esse recurso facilita a edição de arquivos de origem longos, fornecendo uma maneira direta e rápida de acessar o código.

    ![Inspecionando elementos](using-page-inspector-in-visual-studio-2012/_static/image6.png)

    *Inspecionando elementos*
7. Clique no botão **Ativar/desativar modo de inspeção** (![Selecione a guia HTML para exibir o código HTML renderizado no navegador Inspetor de página.](using-page-inspector-in-visual-studio-2012/_static/image7.png "Selecione a guia HTML para exibir o código HTML renderizado no navegador Inspetor de Página.") ) para desabilitar o cursor.
8. Selecione a guia **HTML** para exibir o código HTML renderizado no navegador Inspetor de página.
9. Na marcação HTML, localize o item de lista com o link Koala e selecione-o.

    Observe que, quando você seleciona o código, a saída correspondente é realçada automaticamente no navegador. Esse recurso é útil para ver como um bloco HTML é renderizado na página.

    ![Selecionando o elemento HTML na página](using-page-inspector-in-visual-studio-2012/_static/image8.png "Selecionando o elemento HTML na página")

    *Selecionando o elemento HTML na página*
10. Clique no botão **Ativar/desativar modo de inspeção** para habilitar *modo de inspeção* e clique na barra de navegação. À direita do código HTML, no painel estilos, você verá uma lista com os estilos CSS aplicados ao elemento selecionado.

    > [!NOTE]
    > Como o cabeçalho faz parte do layout do site, Inspetor de Página também abrirá \_arquivo layout. cshtml e realçará o segmento de código afetado.

    ![Descobrindo estilos](using-page-inspector-in-visual-studio-2012/_static/image9.png)

    *Descobrindo estilos e arquivos de origem de um elemento selecionado*
11. Com o ponteiro de inspeção de alternância habilitado, mova o ponteiro do mouse para baixo da barra em destaque azul e clique no meio do círculo.

    ![Selecionando um elemento](using-page-inspector-in-visual-studio-2012/_static/image10.png "Selecionando um elemento")

    *Selecionando um elemento*
12. No painel estilos, localize o item de **imagem em segundo plano** no grupo **. Main-Content** . **Desmarque** a **imagem de plano de fundo** e veja o que acontece. Você observará que o navegador refletirá as alterações imediatamente e o círculo ficará oculto.

    > [!NOTE]
    > As alterações aplicadas na guia estilos de Inspetor de Página não afetam a folha de estilo original. Você pode desmarcar os estilos ou alterar seus valores quantas vezes desejar, mas eles serão restaurados após a atualização da página.

    ![Habilitando e desabilitando estilos CSS](using-page-inspector-in-visual-studio-2012/_static/image11.png "Habilitando e desabilitando estilos CSS")

    *Habilitando e desabilitando estilos CSS*
13. Agora, clique no texto '**seu logotipo aqui**' no cabeçalho usando o modo de inspeção.
14. Na guia **estilos** , localize o atributo **fonte-tamanho** CSS no grupo **. site-título** . Clique duas vezes no valor do atributo e substitua o valor 2,3 em por **3**em e pressione **Enter**. Observe que o título é maior.

    ![Alterando valores de CSS no Inspetor de Página](using-page-inspector-in-visual-studio-2012/_static/image12.png "Alterando valores de CSS no Inspetor de Página")

    *Alterando valores de CSS no Inspetor de Página*
15. Clique na guia **estilos de rastreamento** , localizada no painel à direita de Inspetor de página. Essa é uma maneira alternativa de ver todos os estilos aplicados à seleção, ordenados pelo nome do atributo.

    ![Rastreamento de estilos CSS](using-page-inspector-in-visual-studio-2012/_static/image13.png)

    *Rastreamento de estilos CSS do elemento selecionado*
16. Outro recurso do Inspetor de Página é o painel de layout. Usando o modo de inspeção, selecione a barra de navegação e, em seguida, clique na guia **layout** no painel à direita. Você verá o tamanho exato do elemento selecionado, bem como seu deslocamento, margem, preenchimento e tamanho da borda. Observe que você também pode modificar os valores dessa exibição.

    ![Layout do elemento no Inspetor de Página](using-page-inspector-in-visual-studio-2012/_static/image14.png "Layout do elemento no Inspetor de Página")

    *Layout do elemento no Inspetor de Página*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>Tarefa 2-localizando e corrigindo problemas de estilo na Galeria de fotos

Como você diagnosticaria problemas de páginas da Web com versões anteriores do Visual Studio? Você provavelmente está familiarizado com as ferramentas de depuração da Web que são executadas fora do IDE do Visual Studio, como o Internet Explorer Ferramentas para Desenvolvedores ou o Firebug. Os navegadores entendem apenas HTML, scripts e estilos, enquanto uma estrutura subjacente gera o HTML que será renderizado. Por esse motivo, muitas vezes você precisa implantar o site inteiro para ver como as páginas da Web são semelhantes.

Você provavelmente seguiu essas etapas quando quisesse detectar e corrigir um problema em seu site da Web:

1. Execute a solução no Visual Studio ou implante a página no servidor Web.
2. No navegador, abra as ferramentas de desenvolvedor que você usa ou simplesmente abra o código-fonte e os estilos e tente corresponder ao problema. Para localizar os arquivos envolvidos, você teria usado o &quot;pesquisa&quot; ou &quot;pesquisa em arquivos&quot; recursos com o nome das classes de estilo.
3. Depois que o erro for detectado, pare o navegador da Web e o servidor.
4. Limpe o cache do navegador.
5. Retorne ao Visual Studio para aplicar uma correção. Repita todas as etapas para testar.

Como não há nenhum WYSIWYG real no ASP.NET MVC 4, a maioria dos problemas de estilo são detectados em um estágio posterior, após a execução ou a implantação do aplicativo Web. Agora, com Inspetor de Página, é possível visualizar qualquer página sem executar a solução.

Nesta tarefa, você usará o Inspetor de página e corrigirá alguns problemas do aplicativo Galeria de fotos.

1. Usando Inspetor de Página, localize o **registro** e o **log em** links no lado esquerdo do cabeçalho.

    Observe que os links não são exibidos no lugar esperado à direita e são mostrados como uma lista com marcadores. Agora, você alinhará os links à direita e os reestilizará de acordo.

    ![Localizando os links de registro e logon](using-page-inspector-in-visual-studio-2012/_static/image15.png "Localizando os links de registro e logon")

    *Localizando os links de registro e logon*
2. Com ativar/desativar Modo de Inspeção selecionado, clique em fechar para, mas não em, o link registrar para abrir seu código.

    Observe que o código-fonte dos links está localizado no arquivo **\_LoginPartial. cshtml** , não no index. cshtml nem no \_layout. cshtml, que são os locais que você pode procurar em primeiro lugar. Você foi colocado diretamente no arquivo de origem correto.
3. Na guia **estilos** , localize e clique na **seção\<> item #login** , que é o contêiner HTML para esses links.

    Observe que o estilo de **#login** está localizado automaticamente em **site. css** depois de clicar em. Além disso, o código agora está realçado.

    ![Selecionando os estilos CSS](using-page-inspector-in-visual-studio-2012/_static/image16.png "Selecionando os estilos CSS")

    *Selecionando os estilos CSS*
4. Remova a marca de comentário do atributo **text-align** no código realçado removendo os caracteres de abertura e fechamento e salve o arquivo **site. css** .

    Inspetor de Página está ciente de todos os diferentes arquivos que compõem a página atual e pode detectar quando qualquer um desses arquivos é alterado. Ele alertará você sempre que a página atual no navegador não estiver sincronizada com os arquivos de origem.
5. No navegador Inspetor de Página, clique na barra localizada abaixo da barra de endereços para recarregar a página.

    ![Recarregando a página](using-page-inspector-in-visual-studio-2012/_static/image17.png)

    *Recarregando a página*

    Os links estão agora à direita, mas eles ainda se parecem com uma lista com marcadores. Agora, você removerá os marcadores e alinhará os links horizontalmente.

    ![Página atualizada](using-page-inspector-in-visual-studio-2012/_static/image18.png)

    *Página atualizada*
6. Usando o modo de inspeção, selecione qualquer um dos itens de **&gt;&lt;li** que contenham o &quot;registrar&quot; e &quot;log nos links&quot;. Em seguida, clique na **seção&lt;&gt; #login** item para acessar **estilos. código CSS** .

    ![Encontrando o estilo](using-page-inspector-in-visual-studio-2012/_static/image19.png "Encontrando o estilo")

    *Encontrando o estilo*
7. Em **Style. css**, remova a marca de comentário do código para **#login itens li** . O estilo que você está adicionando ocultará o marcador e exibirá os itens horizontalmente.

    ![Reestilizando os links de logon](using-page-inspector-in-visual-studio-2012/_static/image20.png "Reestilizando os links de logon")

    *Reestilizando os links de logon*
8. Salve o arquivo **Style. css** e clique uma vez na barra localizada abaixo do endereço para recarregar a página. Observe que os links são exibidos corretamente.

    ![Links alinhados ao lado direito](using-page-inspector-in-visual-studio-2012/_static/image21.png "Links alinhados ao lado direito")

    *Links alinhados ao lado direito*
9. Por fim, você vai alterar o título do cabeçalho. Use o modo de inspeção para clicar no **logotipo aqui** e chegar ao código-fonte que o gera.
10. Agora você está em **\_layout. cshtml**, substitua o texto "**seu logotipo aqui**" por "**Galeria de fotos**". Salve e atualize o navegador Inspetor de Página.

    ![Atribuindo um novo título](using-page-inspector-in-visual-studio-2012/_static/image22.png "Atribuindo um novo título")

    *Atribuindo um novo título*

    ![Página Galeria de fotos](using-page-inspector-in-visual-studio-2012/_static/image23.png)

    *Página da Galeria de fotos atualizada*
11. Por fim, selecione o projeto do **Gallery** e pressione **F5** para executar o aplicativo. Confira todas as alterações funcionam conforme o esperado.

---

<a id="Exercise2"></a>

<a id="Exercise_2_Using_Page_Inspector_in_WebForms_Projects"></a>
### <a name="exercise-2-using-page-inspector-in-webforms-projects"></a>Exercício 2: usando Inspetor de Página em projetos de WebForms

Neste exercício, você aprenderá a visualizar e depurar uma solução WebForms usando Inspetor de Página. Primeiro, você executará um breve colo sobre a ferramenta para aprender os recursos de Inspetor de Página que facilitam o processo de depuração na Web. Em seguida, você trabalhará em uma página da Web que contém problemas de estilo. Você aprenderá a usar Inspetor de Página para localizar o código-fonte que gera o problema e corrigi-lo.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>Tarefa 1-explorando Inspetor de Página

Nesta tarefa, você aprenderá a usar os recursos de Inspetor de Página no contexto de um projeto WebForms que mostra uma galeria de fotos.

1. Abra a solução **inicial** localizada em **origem/EX2-WebForms/Begin/** Folder.

   1. Será necessário baixar alguns pacotes NuGet ausentes antes de continuar. Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.
   2. Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, Compile a solução clicando em **build** | **Compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto. É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.
2. Na Gerenciador de Soluções, localize a página **Default. aspx** , clique nela com o botão direito do mouse e selecione **Exibir em Inspetor de página**.

    ![Abrindo default. aspx com Inspetor de Página](using-page-inspector-in-visual-studio-2012/_static/image24.png "Abrindo default. aspx com Inspetor de Página")

    *Abrindo default. aspx com Inspetor de Página*
3. A janela de Inspetor de Página mostrará default. aspx.

    ![Exibindo default. aspx no Inspetor de Página](using-page-inspector-in-visual-studio-2012/_static/image25.png "Exibindo default. aspx no Inspetor de Página")

    *Exibindo default. aspx no Inspetor de Página*

    A ferramenta de Inspetor de Página está integrada ao seu ambiente do Visual Studio. O Inspetor contém um navegador incorporado, junto com um criador de perfil de HTML poderoso que mostrará o código selecionado. Observe que você não precisa executar a solução para ver a aparência de suas páginas.

    > [!NOTE]
    > Quando a largura do navegador Inspetor de Página for menor que a largura da página aberta, você não verá a página corretamente. Se isso acontecer, ajuste a largura do Inspetor de Página.
4. Clique na guia **arquivos** em Inspetor de página.

    Você verá todos os arquivos de origem que estão compondo a página padrão renderizada. Esse é um recurso útil para identificar todos os elementos em um relance, especialmente quando você está trabalhando com controles de usuário e páginas mestras. Observe que você também pode navegar para cada um dos arquivos.

    ![A guia arquivos](using-page-inspector-in-visual-studio-2012/_static/image26.png "A guia arquivos")

    *A guia arquivos*
5. Clique no botão **Ativar/desativar modo de inspeção** , localizado à esquerda das guias.

    Essa ferramenta permitirá que você selecione qualquer elemento da página e veja seu código HTML e a origem. aspx.

    ![Botão de alternância Modo de Inspeção](using-page-inspector-in-visual-studio-2012/_static/image27.png "Botão de alternância Modo de Inspeção")

    *Botão de alternância Modo de Inspeção*
6. No navegador Inspetor de Página, mova o mouse sobre os elementos da página. Enquanto você move o ponteiro do mouse sobre qualquer parte da página renderizada, o tipo de elemento é exibido e a marcação de origem ou o código correspondente é realçado no editor do Visual Studio.

    ![Modo de inspeção em ação](using-page-inspector-in-visual-studio-2012/_static/image28.png "Modo de inspeção em ação")

    *Modo de inspeção em ação*

    > [!NOTE]
    > Não maximize a janela Inspetor de Página ou você não poderá ver a guia Visualização mostrando o código-fonte. Se você clicar no elemento em Inspetor de Página quando ele for maximizado, o código-fonte da seleção será exibido, mas ocultará a janela de Inspetor de Página.

    Se você prestar atenção ao arquivo **Default. aspx** , observará que a parte do código-fonte que gera o elemento selecionado é realçada. Esse recurso facilita a edição de arquivos de origem longos, fornecendo uma maneira direta e rápida de acessar o código.

    ![Inspecionando elementos](using-page-inspector-in-visual-studio-2012/_static/image29.png "Inspecionando elementos")

    *Inspecionando elementos*
7. Clique no botão **alternar modo de inspeção** (![selecione-The-HTML-Tab-to-display-o-HTML-Code-render-in-the-Page-Inspector-browser.](using-page-inspector-in-visual-studio-2012/_static/image30.png "Selecione-The-HTML-Tab-to-display-o-HTML-Code-rendered-in-the-Page-Inspector-browser.") ), localizada nas guias Inspetor de Página, para desabilitar o cursor.
8. Selecione a guia **HTML** para exibir o código HTML renderizado no navegador Inspetor de página.
9. No código HTML, localize o item de lista com o link Koala e selecione-o.

    Observe que, quando você seleciona o código, a saída correspondente é automaticamente realçada no navegador. Esse recurso é útil para ver como um bloco HTML é renderizado na página.

    ![Selecionando um elemento HTML na página](using-page-inspector-in-visual-studio-2012/_static/image31.png "Selecionando um elemento HTML na página")

    *Selecionando um elemento HTML na página*
10. Clique no botão **Ativar/desativar modo de inspeção** para habilitar *modo de inspeção* e clique na barra de navegação. À direita do código HTML, no painel estilos, você verá uma lista com os estilos CSS aplicados ao elemento selecionado.

    > [!NOTE]
    > como o cabeçalho faz parte do layout do site, Inspetor de Página também abrirá o arquivo site. Master e realçará o segmento de código afetado.

    ![Descobrindo os estilos de WebForms](using-page-inspector-in-visual-studio-2012/_static/image32.png "Descobrindo estilos e arquivos de origem de um elemento selecionado")

    *Descobrindo estilos e arquivos de origem de um elemento selecionado*
11. Com o ponteiro de inspeção de alternância habilitado, mova o ponteiro do mouse para baixo da barra de menus e clique no meio-círculo em branco.

    ![Selecionando um elemento](using-page-inspector-in-visual-studio-2012/_static/image33.png "Selecionando um elemento")

    *Selecionando um elemento*
12. No painel estilos, localize o item de **imagem em segundo plano** no grupo **. Main-Content** . **Desmarque** a **imagem de plano de fundo** e veja o que acontece. Você observará que o navegador refletirá as alterações imediatamente e o círculo ficará oculto.

    > [!NOTE]
    > As alterações aplicadas na guia estilos de Inspetor de Página não afetam a folha de estilo original. Você pode desmarcar os estilos ou alterar seus valores quantas vezes desejar, mas eles serão restaurados após a atualização da página.

    ![Habilitando e desabilitando styles2 CSS](using-page-inspector-in-visual-studio-2012/_static/image34.png "Habilitando e desabilitando estilos CSS")

    *Habilitando e desabilitando estilos CSS*
13. Agora, clique no texto '**seu** **logotipo aqui '** no cabeçalho usando o modo de inspeção.
14. Na guia **estilos** , localize o atributo **fonte-tamanho** CSS no grupo **. site-título** . Clique duas vezes no atributo uma vez para editar seu valor. Substitua o valor 2.3 em por **3em**e pressione Enter. Observe que o título é maior.

    ![Alterando valores de CSS na página Inspector2](using-page-inspector-in-visual-studio-2012/_static/image35.png "Alterando valores de CSS no Inspetor de Página")

    *Alterando valores de CSS no Inspetor de Página*
15. Clique na guia **estilos de rastreamento** , localizada no painel à direita de Inspetor de página. Essa é uma maneira alternativa de ver todos os estilos aplicados à seleção, ordenados pelo nome do atributo.

    ![Rastreamento de estilos CSS do elemento selecionado](using-page-inspector-in-visual-studio-2012/_static/image36.png "Rastreamento de estilos CSS do elemento selecionado")

    *Rastreamento de estilos CSS do elemento selecionado*
16. Outro recurso do Inspetor de Página é o painel de layout. Usando o modo de inspeção, selecione a barra de navegação e, em seguida, clique na guia **layout** no painel à direita. Você verá o tamanho exato do elemento selecionado, bem como seu deslocamento, margem, preenchimento e tamanho da borda. Observe que você também pode modificar os valores dessa exibição.

    ![Layout do elemento no Inspetor de Página](using-page-inspector-in-visual-studio-2012/_static/image37.png "Layout do elemento no Inspetor de Página")

    *Layout do elemento no Inspetor de Página*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>Tarefa 2-localizando e corrigindo problemas de estilo na Galeria de fotos

Como você diagnosticaria problemas de páginas da Web com versões anteriores do Visual Studio? Você provavelmente está familiarizado com as ferramentas de depuração da Web que são executadas fora do IDE do Visual Studio, como o Internet Explorer Ferramentas para Desenvolvedores ou o Firebug. Os navegadores entendem apenas HTML, scripts e estilos, enquanto uma estrutura subjacente gera o HTML que será renderizado. Por esse motivo, muitas vezes você precisa implantar o site inteiro para ver como as páginas da Web são semelhantes.

Você provavelmente seguiu essas etapas quando quisesse detectar e corrigir um problema em seu site da Web:

1. Execute a solução no Visual Studio ou implante a página no servidor Web.
2. No navegador, abra as ferramentas de desenvolvedor que você usa ou simplesmente abra o código-fonte e os estilos e tente corresponder ao problema. Para localizar os arquivos envolvidos, você usou o &quot;pesquisa&quot; ou &quot;pesquisa em arquivos&quot; recursos com o nome das classes de estilo.
3. Depois que o erro for detectado, pare o navegador da Web e o servidor.
4. Limpe o cache do navegador.
5. Retorne ao Visual Studio para aplicar uma correção. Repita todas as etapas para testar.

Como não há nenhum WYSIWYG real em WebForms do ASP.NET, alguns problemas de estilo são detectados em um estágio posterior, após a execução ou a implantação. Agora, com Inspetor de Página, é possível visualizar qualquer página sem executar a solução.

Nesta tarefa, você usará o Inspetor de página para corrigir alguns problemas do aplicativo Galeria de fotos. Nas etapas a seguir, você detectará e corrigirá rapidamente alguns problemas de estilo simples no cabeçalho.

1. Usando a inspeção de página, localize o **registro** e o **log em** links no lado esquerdo do cabeçalho.

    Observe que o link não é exibido no local esperado à direita. Agora, você alinhará o link à direita e o reestilizará adequadamente.

    ![Link de logon posicionado à esquerda](using-page-inspector-in-visual-studio-2012/_static/image38.png "Link de logon posicionado à esquerda")

    *Link de logon posicionado à esquerda*
2. Com ativar/desativar Modo de Inspeção selecionado, selecione o link fazer logon para abrir seu código.

    Observe que o código-fonte do link está localizado no arquivo **site. Master** , não na página default. aspx, que é o local que você pode procurar em primeiro lugar; Você foi colocado diretamente no arquivo de origem correto.
3. Na guia **estilos** , localize e clique na **seção&lt;&gt; item #login** , que é o contêiner HTML para esses links.

    Observe que o estilo de **#login** está localizado automaticamente em **site. css** depois de clicar em. Além disso, o código agora está realçado.

    ![Selecionando os estilos CSS](using-page-inspector-in-visual-studio-2012/_static/image39.png "Selecionando os estilos CSS")

    *Selecionando os estilos CSS*
4. Remova a marca de comentário do atributo **text-align** no código realçado removendo os caracteres de abertura e fechamento e salve o arquivo **site. css** .

    Inspetor de Página está ciente de todos os diferentes arquivos que compõem a página atual e pode detectar quando qualquer um desses arquivos é alterado. Ele alertará você sempre que a página atual no navegador não estiver sincronizada com os arquivos de origem.
5. No navegador Inspetor de Página, clique na barra localizada abaixo da barra de endereços para salvar as alterações e recarregar a página.

    ![Recarregando a página](using-page-inspector-in-visual-studio-2012/_static/image40.png)

    *Recarregando a página*

    Os links estão agora à direita, mas eles ainda se parecem com uma lista com marcadores. Agora, você removerá os marcadores e alinhará os links horizontalmente.

    ![Página atualizada](using-page-inspector-in-visual-studio-2012/_static/image41.png)

    *Página atualizada*
6. Usando o modo de inspeção, selecione qualquer um dos itens de **&gt;&lt;li** que contenham o &quot;registrar&quot; e &quot;log nos links&quot;. Em seguida, clique na **seção&lt;&gt; #login** item para acessar **estilos. código CSS** .

    ![Encontrando o estilo](using-page-inspector-in-visual-studio-2012/_static/image42.png "Encontrando o estilo")

    *Encontrando o estilo*
7. Em **Style. css**, remova a marca de comentário do código para **#login itens li** . O estilo que você está adicionando ocultará o marcador e exibirá os itens horizontalmente.

    ![Reestilizando os links de logon](using-page-inspector-in-visual-studio-2012/_static/image43.png "Reestilizando os links de logon")

    *Reestilizando os links de logon*
8. Salve o arquivo **Style. css** e clique uma vez na barra localizada abaixo do endereço para recarregar a página. Observe que os links são exibidos corretamente.

    ![Links alinhados ao lado direito](using-page-inspector-in-visual-studio-2012/_static/image44.png "Links alinhados ao lado direito")

    *Links alinhados ao lado direito*
9. Por fim, você vai alterar o título do cabeçalho. Em vez de procurar o texto '**seu logotipo aqui '** em todos os arquivos, use o modo de inspeção para clicar no texto e chegar ao código-fonte que o gera.

    ![Localizando o título do site](using-page-inspector-in-visual-studio-2012/_static/image45.png "Localizando o título do site")

    *Localizando o título do site*
10. Agora você está no **site. Master**, substitua o texto '**seu logotipo aqui**' por '**Galeria de fotos**'. Salve e atualize o navegador Inspetor de Página.

    ![Página da Galeria de fotos atualizada](using-page-inspector-in-visual-studio-2012/_static/image46.png "Página da Galeria de fotos atualizada")

    *Página da Galeria de fotos atualizada*
11. Por fim, pressione **F5** para executar o aplicativo para verificar se todas as alterações funcionam conforme o esperado.

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Resumo

Ao concluir este laboratório prático, você aprenderá a usar Inspetor de Página para visualizar seu aplicativo Web sem precisar recompilar e executar o site em um navegador. Além disso, você aprenderá a localizar e corrigir rapidamente os bugs acessando diretamente da saída renderizada para o código-fonte.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apêndice A: Instalando o Visual Studio Express 2012 para Web

Você pode instalar o **Microsoft Visual Studio Express 2012 para Web** ou outra versão &quot;Express&quot; usando o **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** . As instruções a seguir orientam você pelas etapas necessárias para instalar o *Visual Studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.

1. Vá para [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Como alternativa, se você já tiver instalado o Web Platform Installer, poderá abri-lo e pesquisar o produto &quot;<em>Visual Studio Express 2012 para Web com o SDK do Windows Azure</em>&quot;.
2. Clique em **instalar agora**. Se você não tiver **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.
3. Quando **Web Platform Installer** estiver aberto, clique em **instalar** para iniciar a instalação.

    ![Instalar Visual Studio Express](using-page-inspector-in-visual-studio-2012/_static/image47.png "Instalar Visual Studio Express")

    *Instalar Visual Studio Express*
4. Leia todos os termos e licenças de produtos e clique em **aceito** para continuar.

    ![Aceitando os termos de licença](using-page-inspector-in-visual-studio-2012/_static/image48.png)

    *Aceitando os termos de licença*
5. Aguarde até que o processo de download e instalação seja concluído.

    ![Progresso da instalação](using-page-inspector-in-visual-studio-2012/_static/image49.png)

    *Progresso da instalação*
6. Quando a instalação for concluída, clique em **concluir**.

    ![Instalação concluída](using-page-inspector-in-visual-studio-2012/_static/image50.png)

    *Instalação concluída*
7. Clique em **sair** para fechar Web Platform Installer.
8. Para abrir o Visual Studio Express para Web, vá para a tela **Iniciar** e comece a escrever &quot;**vs Express**&quot;e, em seguida, clique no bloco **vs Express para Web** .

    ![Bloco VS Express para Web](using-page-inspector-in-visual-studio-2012/_static/image51.png)

    *Bloco VS Express para Web*
