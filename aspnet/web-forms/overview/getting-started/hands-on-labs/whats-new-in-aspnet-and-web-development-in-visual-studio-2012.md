---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
title: O que há de novo no ASP.NET e no desenvolvimento para a Web no Visual Studio 2012 | Microsoft Docs
author: rick-anderson
description: A nova versão do Visual Studio apresenta vários aprimoramentos que se concentram em melhorar a experiência e o desempenho ao trabalhar com tecnologias da Web...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 6d40d276-1642-4a77-b6c9-02ac914f6805
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 80c77ec65ed86b06e417d3f6ba608e404c46768b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78525931"
---
# <a name="whats-new-in-aspnet-and-web-development-in-visual-studio-2012"></a>Novidades no ASP.NET e desenvolvimento da Web no Visual Studio 2012

por [equipe de acampamentos da Web](https://twitter.com/webcamps)

> A nova versão do Visual Studio apresenta vários aprimoramentos que se concentram em melhorar a experiência e o desempenho ao trabalhar com tecnologias da Web. Os editores do Visual Studio para CSS, JavaScript e HTML foram completamente remodelados para incluir muitos dos auxílios de código mais sob demanda, como IntelliSense e recuo automático. Em relação ao desempenho, o agrupamento e o minificação agora são integrados como recursos internos para reduzir facilmente o tempo de carregamento da página.
> 
> O Visual Studio permite que você trabalhe com as tecnologias mais recentes do site. Você pode usar os trechos de código CSS3 entre navegadores para garantir que seu site funcione independentemente da plataforma cliente, aproveitando também os novos elementos e recursos do HTML5.
> 
> Escrever e criar perfis de código JavaScript deve ser mais fácil com esta versão do Visual Studio. As listas do IntelliSense, a documentação de XML integrada e os recursos de navegação agora estão disponíveis para o código JavaScript. Agora você tem o catálogo do JavaScript ao seu alcance. Além disso, você pode verificar a conformidade do ECMAScript5 com seus scripts e detectar erros de sintaxe em um estágio inicial.
> 
> Por último, mas não menos importante, essa versão do Visual Studio implementa o Agrupamento interno e o minificação. Os arquivos de script e as folhas de estilo serão empacotados e compactados para que o site seja executado mais rapidamente.
> 
> Este laboratório orienta você pelos aprimoramentos e novos recursos descritos anteriormente, aplicando pequenas alterações a um aplicativo Web de exemplo fornecido na pasta de origem.
> 
> Todos os códigos de exemplo e trechos de código estão incluídos no kit de treinamento do acampamentos da Web, disponível em [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

Neste laboratório prático, você aprenderá a:

- Usar os novos recursos e aprimoramentos no editor de CSS
- Usar os novos recursos e aprimoramentos no editor de HTML
- Usar os novos recursos e aprimoramentos no editor de JavaScript
- Configurar e usar agrupamento e minificação

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisites

- [Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou superior (leia [o apêndice a](#AppendixA) para obter instruções sobre como instalá-lo).
- [Windows PowerShell](https://support.microsoft.com/kb/968930/) (para scripts de instalação – já instalado no Windows 8 e no windows Server 2008 R2)
- [Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home) -ou um navegador compatível com HTML5

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Exercícios

Este laboratório prático inclui os seguintes exercícios:

1. [Exercício 1: o que há de novo no editor de CSS](#Exercise1)
2. [Exercício 2: o que há de novo no editor de HTML](#Exercise2)
3. [Exercício 3: o que há de novo no editor de JavaScript](#Exercise3)
4. [Exercício 4: agrupamento e Minificação](#Exercise4)

Tempo estimado para concluir este laboratório: **60 minutos**.

<a id="Exercise1"></a>

<a id="Exercise_1_Whats_New_in_the_CSS_Editor"></a>
### <a name="exercise-1-whats-new-in-the-css-editor"></a>Exercício 1: o que há de novo no editor de CSS

Os desenvolvedores da Web devem estar familiarizados com muitas das dificuldades relacionadas à edição de CSS. Um dos maiores problemas de estilo de CSS é a compatibilidade entre navegadores. Em geral, acontece que, depois de aplicar os estilos ao seu site, você observa que ele parece diferente se você o abre em outro navegador ou dispositivo. Portanto, você pode dedicar um tempo considerável ao corrigir esses problemas visuais para perceber que, quando finalmente faz isso funcionar em um navegador, ele é quebrado nos outros.

O Visual Studio agora inclui recursos que ajudam os desenvolvedores a acessar, trabalhar e organizar folhas de estilo CSS com eficiência. Ao longo deste exercício, você conhecerá os novos recursos para uma organização e edição em vigor, bem como os trechos de código do CSS3 para compatibilidade entre navegadores.

<a id="Ex1Task1"></a>

<a id="Task_1_-_New_Editor_Features"></a>
#### <a name="task-1---new-editor-features"></a>Tarefa 1-novos recursos do editor

Nesta tarefa, você descobrirá os novos recursos do editor de CSS. Esse novo editor o ajudará a aumentar sua produtividade aproveitando o novo recuo inteligente, os comentários de código aprimorados e a lista aprimorada do IntelliSense.

1. Inicie o **Visual Studio** e abra a solução **WhatsNewASPNET. sln** localizada na pasta **Source\WhatsNewASPNET** deste laboratório.
2. Em Gerenciador de Soluções, abra o arquivo **site. css** localizado na pasta **estilos** . Verifique se as ferramentas do **Editor de texto** estão visíveis na barra de ferramentas. Para fazer isso, selecione a opção de menu **exibir** | **barras de ferramentas** e marque as opções do **Editor de texto** . Você observará que, como essa nova versão, o botão de **Comentário** (![comentário-botão](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png)) e **o botão remover comentário (** ![botão Remover comentário](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png)) também são habilitados para o editor de CSS.

    ![Habilitando ferramentas de editor e CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "Habilitando ferramentas de editor e CSS")

    *Habilitando ferramentas de editor e CSS*
3. Role o código e selecione qualquer definição de classe CSS. Clique no botão **Comentário** (![comentário – botão](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png)) para comentar as linhas selecionadas. Em seguida, clique no botão **Remover comentário** (![botão remover comentário](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png)) para desfazer as alterações.
4. Clique nos botões **recolher** (![recolher](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png)) e **expandir** (![expandir](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png)) localizados na margem esquerda do texto. Observe que agora você pode ocultar os estilos que você não usa para ter uma exibição mais limpa.

    ![Recolhendo classes CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "Recolhendo classes CSS")

    *Recolhendo classes CSS*
5. Verifique se o recurso de recuo inteligente está habilitado. Selecione a opção de menu **ferramentas** | **Opções** e, em seguida, selecione a página **Editor de texto** | **CSS** | **formatação** no painel esquerdo da tela. Verifique a opção de **recuo hierárquico** .

    ![Habilitando recuo hierárquico](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "Habilitando recuo hierárquico")

    *Habilitando recuo hierárquico*
6. Localize a definição de classe principal (. Main) e acrescente um estilo aos elementos div. Você observará que o código se alinha automaticamente, ajudando os usuários a encontrar as classes pai em um relance.

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample1.css)]

    ![Alinhamento hierárquico em CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "Alinhamento hierárquico em CSS")

    *Alinhamento hierárquico em CSS*
7. Na classe **div principal** , localize o cursor no final da **borda: 0px;** e pressione **Enter** para exibir a lista do IntelliSense. Comece a digitar **Top** e observe como a lista é filtrada conforme você digita. A lista exibirá os elementos que contêm **Top** em qualquer parte da palavra (em versões anteriores do Visual Studio, a lista será filtrada pelos itens que *começam* com o termo).

    ![Aprimoramentos do IntelliSense no CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "Aprimoramentos do IntelliSense no CSS")

    *Aprimoramentos do IntelliSense no CSS*

<a id="Ex1Task2"></a>

<a id="Task_2_-_The_Color_Picker"></a>
#### <a name="task-2---the-color-picker"></a>Tarefa 2-o seletor de cores

Nesta tarefa, você descobrirá o novo seletor de cores CSS integrado ao Visual Studio IntelliSense.

1. Em **site. CSS,** localize a definição de classe de cabeçalho (. Header) e coloque o cursor ao lado do atributo **de cor de plano de fundo** , entre o &quot;:&quot; e &quot;#caracteres de &quot; na linha de código **.**

    ![Localizando o cursor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "Localizando o cursor")

    *Localizando o cursor*
2. Exclua os **dois-pontos** (:) e grave-a novamente para exibir o seletor de cores. Observe que as primeiras cores que você verá são as cores mais frequentes de seu site. Se você clicar na cor branca, seu código de cor HTML (#fff) substituirá o código de cor atual na folha de estilos.

    ![Seletor de cores](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "Seletor de cor")

    *Seletor de cores*
3. Pressione o botão **expandir** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png)) no seletor de cores para exibir o gradiente de cor e, em seguida, arraste o cursor gradiente para selecionar uma cor diferente. Depois disso, clique no botão **conta-gotas** e selecione qualquer cor na tela. Observe que o valor da cor do plano de fundo é alterado dinamicamente enquanto você move o cursor.

    ![Gradiente do seletor de cor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "Gradiente do seletor de cor")

    *Gradiente do seletor de cor*
4. No controle deslizante **opacidade** , mova o seletor para o centro da barra para reduzir a opacidade. Observe que o valor de cor de fundo agora altera sua escala para RGBA.

    ![Opacidade do seletor de cor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "Opacidade do seletor de cor")

    *Opacidade do seletor de cor*

    > [!NOTE]
    > A definição de cor RGBA (vermelha, verde, azul, alfa) no CSS3 permite que você defina o valor de opacidade de cor para um único item. Ao contrário de **Opacity,** um atributo CSS semelhante **-** cores RGBA também são compatíveis com os navegadores mais recentes.

<a id="Ex1Task3"></a>

<a id="Task_3_-_CSS_Compatible_Code_Snippets"></a>
#### <a name="task-3---css-compatible-code-snippets"></a>Tarefa 3-trechos de código compatíveis com CSS

Nesta tarefa, você aprenderá a usar os trechos de código CSS3 compatíveis com vários navegadores para implementar alguns recursos em seu site.

1. No arquivo **site. css** , localize a definição de classe CSS do **cabeçalho** (. Header) e coloque o cursor abaixo do **/\*raio de borda\*/** espaço reservado para adicionar um novo trecho de código. Pressione **Enter** para exibir a lista do IntelliSense e digite **RADIUS** para filtrar a lista. Selecione a opção **border-radius** na lista com um clique duplo e, em seguida, pressione a tecla **Tab** para inserir o trecho de código. Em seguida, digite um tamanho de raio em pixels e pressione **Enter**. Por exemplo, digite **15px**.

    Os atributos do CSS3 adicionados pelo trecho de código processarão bordas arredondadas na maioria dos navegadores de conformidade do HTML5, incluindo navegadores baseados em Mozilla e WebKit.

    ![Usando um trecho de borda-raio](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "Usando um trecho de borda-raio")

    *Usando um trecho de borda-raio*
2. Aplique os mesmos trechos de **borda** no estilo da página (. Page).

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample2.css)]
3. Pressione **F5** para executar a solução. Observe que cada página agora tem bordas arredondadas.

    ![Cantos arredondados](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "Cantos arredondados")

    *Cantos arredondados*
4. Feche o navegador e retorne ao Visual Studio.
5. Abra o arquivo **. CSS personalizado** localizado na pasta **estilos** e coloque o cursor dentro da definição da classe **div. images ul li** .
6. Pressione Enter para exibir a lista do IntelliSense, digite **Box-Shadow** e pressione a tecla **Tab** duas vezes para inserir o trecho de código de sombra padrão dentro da definição de classe. Defina os valores de sombra como **10px 10px 5px #888**. Em seguida, digite **border-radius** e insira o trecho de código. Digite **15px** para definir o tamanho do raio e pressione **Enter**.

    ![Cantos arredondados com sombra](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "Cantos arredondados com sombra")

    *Cantos arredondados com sombra*

    > [!NOTE]
    > Neste momento, o atributo Shadow é inserido com o prefixo correspondente (Moz, WebKit, o) para dar suporte a navegadores Mozilla e WebKit (Chrome, Safari, Konkeror).
7. Crie uma nova classe **div. images ul li img: focalize** abaixo da definição da classe **div. images ul li img** e coloque o cursor dentro dos colchetes **.**

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample3.css)]
8. Digite **Transform** e pressione a tecla **Tab** duas vezes para inserir o trecho de transformação. Em seguida, digite **Rotate (-15deg)** para alterar o valor do ângulo de rotação quando as imagens forem focalizadas.

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample4.css)]
9. Pressione **F5** para executar a solução e navegar até a página do CSS3. Observe que as imagens têm cantos arredondados e sombras de caixa. Passe o mouse sobre as imagens e veja-as girando.

    ![Transformar trecho girando uma imagem](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "Transformar trecho girando uma imagem")

    *Transformar trecho girando uma imagem*

    > [!NOTE]
    > Se você estiver usando o Internet Explorer 10 e não puder ver as sombras, verifique se o modo de documento está definido como IE10 padrões. Pressione **F12** para abrir as ferramentas de desenvolvedor do Internet Explorer e clique em **modo de documento** para alterar para os padrões de IE10.

    ![about-us](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image21.png)

<a id="Exercise2"></a>

<a id="Exercise_2_Whats_New_in_the_HTML_Editor"></a>
### <a name="exercise-2-whats-new-in-the-html-editor"></a>Exercício 2: o que há de novo no editor de HTML

O Visual Studio tem um editor de HTML aprimorado. Alguns dos aprimoramentos incluídos nesta versão são o recuo inteligente em documentos HTML, trechos de código HTML5, correspondência de marca de início e término de HTML e validação de HTML. Ao longo deste exercício, você verá como essas alterações melhoram seu fluência ao trabalhar na marcação do site.

Como o editor de CSS, o editor de HTML também foi melhorado. A maioria desses aprimoramentos são pequenos que facilitam a vida do desenvolvedor da Web. Coisas como mais trechos de código para HTML5, recuo inteligente, correspondência de marcas de início e término quando a edição e a validação visando o documento HTML DOCTYPE são alguns desses aprimoramentos.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Improved_DOCTYPE_Validation"></a>
#### <a name="task-1---improved-doctype-validation"></a>Tarefa 1-validação de DOCTYPE aprimorada

O editor de HTML agora tem a capacidade de verificar o DOCTYPE de sua página, mesmo que a definição possa estar na página mestra. Dependendo do DOCTYPE da sua página, o editor de HTML será validado com o conjunto correto de regras e filtrará a lista do IntelliSense considerando os elementos DOCTYPE.

Nesta tarefa, você alterará o DOCTYPE de uma página para ver como o comportamento do editor de HTML é alterado de acordo.

1. Se ainda não estiver aberto, inicie o **Visual Studio** e abra a solução **WhatsNewASPNET. sln** localizada na pasta **Source\WhatsNewASPNET** deste laboratório.
2. Abra a página **site. Master** .
3. Observe o esquema de destino para a barra de ferramentas de validação. A maneira como o editor de HTML se comporta (validação, IntelliSense, etc.) mudará corretamente para se ajustar ao DOCTYPE selecionado.

    ![Usar DOCTYPE na barra de ferramentas de edição de código-fonte HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "Usar DOCTYPE na barra de ferramentas de edição de código-fonte HTML")

    *Usar DOCTYPE na barra de ferramentas de edição de código-fonte HTML*
4. Altere o esquema de destino para HTML 4, 1.

    ![Alterando o DOCTYPE na barra de ferramentas de edição de origem HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "Alterando o DOCTYPE na barra de ferramentas de edição de origem HTML")

    *Alterando o DOCTYPE na barra de ferramentas de edição de origem HTML*
5. Coloque o cursor sob o elemento **Body** e comece a digitar o nome de um elemento HTML5 (por exemplo, **vídeo**). Observe que o elemento não está disponível na lista do IntelliSense.

    ![Elementos HTML5 não listados](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "Elementos HTML5 não listados")

    *Elementos HTML5 não listados*
6. Desfaça as alterações no esquema de destino para a barra de ferramentas de validação, separando o DOCTYPE: XHTML5 na lista suspensa.

    ![Usar DOCTYPE na barra de ferramentas de edição de código-fonte HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "Usar DOCTYPE na barra de ferramentas de edição de código-fonte HTML")

    *Redefinir DOCTYPE na barra de ferramentas de edição de código-fonte HTML*
7. Coloque o cursor sob o elemento **Body** e comece a digitar um elemento HTML5 novamente (por exemplo, como **vídeo**). Observe que os elementos HTML5 agora estão disponíveis na lista do IntelliSense.

    ![Elementos HTML5 sendo listados](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "Elementos HTML5 sendo listados")

    *Elementos HTML5 sendo listados*

<a id="Ex2Task2"></a>

<a id="Task_2_-_StartEnd_Tags_Automatic_Update"></a>
#### <a name="task-2---startend-tags-automatic-update"></a>Tarefa 2-atualização automática de marcas de início/término

O Visual Studio agora atualiza as marcas HTML de abertura ou de fechamento do elemento que você está editando para corresponder umas às outras. Esse novo recurso melhorará sua produtividade ao editar marcas HTML.

1. Na página **Default. aspx** , adicione um elemento **H3** com um título (por exemplo, Visual Studio 2012 Rocks!).

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample5.aspx)]
2. Altere a marca **H3** e digite **H2** ou **H1.**

    Observe que a marca de fim é atualizada automaticamente. Você também pode modificar a marca de fim para ver que a marca de início é atualizada de forma adequada.

    ![Atualização automática da marca de fim](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "Atualização automática da marca de fim")

    *Atualização automática da marca de fim*

<a id="Ex2Task3"></a>

<a id="Task_3_-_New_HTML5_Code_Snippets"></a>
#### <a name="task-3---new-html5-code-snippets"></a>Tarefa 3-novos trechos de código HTML5

O Visual Studio agora inclui vários trechos de código HTML5. Nesta tarefa, você usará alguns desses trechos de código.

1. Adicione uma nova pasta chamada **áudio** à raiz da pasta do site. Abra o Windows Explorer e copie qualquer arquivo de áudio para a pasta de **áudio** da solução **WhatsNewASPNET. sln** .
2. Na página **Default. aspx** , localize o cursor em Web11 Rocks! Cabeçalho. Digite **áudio** e pressione a tecla Tab.

    O novo editor de HTML inclui trechos de código para conteúdo HTML5. Lembre-se de usar a definição de DOCTYPE apropriada para habilitar os trechos de código HTML5.

    ![Inserindo trechos de código HTML5](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "Inserindo trechos de código HTML5")

    *Inserindo trechos de código HTML5*
3. Atualize a fonte de áudio para apontar para um arquivo de áudio existente.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample6.aspx)]

    > [!NOTE]
    > Será necessário adicionar o arquivo de áudio à solução.
4. Pressione **F5** para executar o site e reproduzir o áudio.

    ![Executando o controle de áudio](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "Executando o controle de áudio")

    *Executando o controle de áudio*

    > [!NOTE]
    > Você também pode experimentar mais trechos de código incluídos no Visual Studio, como vídeo, figura, etc.
5. Agora, tente inserir um controle em alguma parte da página. Por exemplo, tente inserir um controle **GridView** , mas em vez de digitar **&lt;GRI,** comece a digitar **&lt;GV**. Observe que a lista do IntelliSense mostra o controle **asp: GridView** .

    O IntelliSense no editor de HTML agora fornece pesquisa de uso de título, bem como correspondência parcial (recuperando todos os elementos que contêm o termo).

    ![Inserindo um GridView com listas do IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "Inserindo um GridView com listas do IntelliSense")

    *Inserindo um GridView com listas do IntelliSense*

    Se você digitar **&lt;grade** , obterá todos os itens que correspondem ao termo, mas o Visual Studio irá sugerir o controle **GridView** :

    ![Inserindo um GridView com listas do IntelliSense e correspondência parcial](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "Inserindo um GridView com listas do IntelliSense e correspondência parcial")

    *Inserindo um GridView com listas do IntelliSense e correspondência parcial*

<a id="Ex2Task4"></a>

<a id="Task_4_-_HTML_Editor_Smart_Tags"></a>
#### <a name="task-4---html-editor-smart-tags"></a>Tarefa 4-marcas inteligentes do editor de HTML

Outra melhoria no editor de HTML é o recurso de marcas inteligentes. Marcas inteligentes facilitam a execução de tarefas de desenvolvimento comuns ou repetitivas de acordo com cada controle. Esse recurso já estava disponível no designer de HTML, mas não no editor de HTML.

1. Abra **site. Master** e localize o elemento **asp: menu** . Coloque o cursor na marca de início e observe que o glifo pequeno é exibido na parte inferior do elemento. clique nele para abrir o menu tarefas inteligentes. Observe que você tem acesso rápido a algumas tarefas relacionadas ao controle menu.

    ![Tarefas inteligentes para o controle de menu](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "Tarefas inteligentes para o controle de menu")

    *Tarefas inteligentes para o controle de menu*

<a id="Ex2Task5"></a>

<a id="Task_5_-_Smart_Indentation"></a>
#### <a name="task-5---smart-indentation"></a>Tarefa 5-recuo inteligente

Uma das práticas recomendadas em HTML é recuar os elementos aninhados para manter o código legível. No Visual Studio 2012, você observará que o editor recua automaticamente os elementos enquanto você estiver escrevendo o código.

> [!NOTE]
> Na versão anterior do Visual Studio, o recuo inteligente estava disponível no editor de XML, mas não no editor de HTML.

1. Verifique se a configuração de recuo no editor de HTML está definida como recuo inteligente. Para fazer isso, selecione as **ferramentas |** Opção do menu opções e, em seguida, selecione o **Editor de texto | HTML |** Página de guias no painel esquerdo da tela. Selecione a opção de recuo inteligente.

    ![Configurações do editor de HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "Configurações do editor de HTML")

    *Configurações do editor de HTML*
2. Na página **Default. aspx** , remova todo o conteúdo do elemento Audio.
3. Coloque o cursor no final do elemento de **áudio** de abertura e pressione **Enter**.

    Observe que a nova posição de cursor tem um nível de recuo adicional.

    ![Recuo inteligente no editor de HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "Recuo inteligente no editor de HTML")

    *Recuo inteligente no editor de HTML*
4. Restaure a marca de áudio com o conteúdo que você removeu ou feche **Default. aspx** sem salvar as alterações.

<a id="Ex2Task6"></a>

<a id="Task_6_-_Extract_to_User_Control"></a>
#### <a name="task-6---extract-to-user-control"></a>Tarefa 6 – extrair para controle de usuário

As ferramentas de refatoração incluídas no Visual Studio, como a extração de uma parte do código para uma função, são ótimos recursos que facilitam a melhoria e a refatoração do código existente. A contraparte para ASP.NET pages seria a extração de código HTML para um controle de usuário. Fazer isso manualmente envolveria várias etapas, como criar um novo controle de usuário, mover a seção de código para o controle de usuário, registrar um prefixo de marca para o controle de usuário e, por fim, instanciar o controle de usuário nas páginas. Agora, a nova *extração para a ferramenta de controle do usuário* executa automaticamente todas essas etapas para você.

Nesta tarefa, você usará a nova operação de extrair para controle de usuário contextual para gerar um novo controle de usuário a partir do código selecionado.

1. Na página **Default. aspx** , selecione os elementos **H2** e **Audio** .
2. Clique com o botão direito do mouse e selecione **extrair para controle de usuário**.

    ![Opção de menu extrair para controle do usuário](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "Opção de menu extrair para controle do usuário")

    *Opção de menu extrair para controle do usuário*
3. Digite um nome para o novo controle de usuário. Por exemplo, **jukebox. ascx**e clique em **OK**.

    ![Salvando o controle de usuário extraído](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "Salvando o controle de usuário extraído")

    *Salvando o controle de usuário extraído*
4. Observe que o código selecionado foi extraído para um controle de usuário e o local original do código selecionado foi substituído por uma instância do novo controle de usuário.

    ![Página atualizada automaticamente para usar o novo controle de usuário](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "Página atualizada automaticamente para usar o novo controle de usuário")

    *Página atualizada automaticamente para usar o novo controle de usuário*
5. Pressione **F5** para executar a página e verificar se o controle funciona.

<a id="Exercise3"></a>

<a id="Exercise_3_Whats_New_in_the_JavaScript_Editor"></a>
### <a name="exercise-3-whats-new-in-the-javascript-editor"></a>Exercício 3: o que há de novo no editor de JavaScript

Escrever ou editar código JavaScript não é uma tarefa fácil, especialmente quando seu aplicativo começa a crescer em tamanho e você se encontra lidando com arquivos longos e centenas de funções. Os desenvolvedores de script geralmente precisam fazer algum trabalho extra para manter a legibilidade do código e navegar pelos arquivos. Com a inclusão de bibliotecas JavaScript como o jQuery, a navegação de scripts se tornou um desafio por si só devido ao tamanho do código.

O Visual Studio renovou o editor de JavaScript com a promessa de tornar o modo de código acessível e organizado. Muitos recursos do Visual Studio que já existiam no ou os editores do C# VB agora são implementados no editor de JavaScript: Vá para definição, recuo automático, documentação e validação quando estiver escrevendo. Com a lista renovada do IntelliSense, você terá o catálogo de funções do JavaScript ao seu alcance.

Neste exercício, você aprenderá alguns dos novos recursos e melhorias do editor de JavaScript. Você procurará arquivos de exemplo e descobrirá cada uma das novas características que tornarão sua programação JavaScript mais eficiente no Visual Studio 2012.

<a id="Ex3Task1"></a>

<a id="Task_1_-_JavaScript_Editor_New_Features"></a>
#### <a name="task-1---javascript-editor-new-features"></a>Tarefa 1-novos recursos do editor de JavaScript

Essa tarefa apresentará alguns dos novos recursos do editor de JavaScript, que se concentram em organizar seu código e oferecer uma melhor experiência ao usuário.

1. Se ainda não estiver aberto, inicie o **Visual Studio** e abra a solução **WhatsNewASPNET. sln** localizada na pasta **Source\WhatsNewASPNET** deste laboratório.
2. Pressione **F5** para executar o aplicativo e, em seguida, clique no link JavaScript na barra de navegação. Atualize a página várias vezes e verifique como o contador é incrementado.

    ![Contador de páginas](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "Contador de páginas")

    *Contador de páginas*
3. Feche o navegador e volte para o Visual Studio.
4. Abra a página **JavaScript. aspx** e localize o bloco de **&gt;de script&lt;** (mostrado abaixo).

    O código a seguir usa o armazenamento local do HTML5 para armazenar uma variável *pageLoadCount* que armazena o número de vezes que a página foi visitada pelo usuário atual. O armazenamento local é um banco de dados de valor-chave do lado do cliente introduzido com o padrão HTML5. Os dados são salvos no computador local, dentro do navegador do usuário.

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample7.html)]

    > [!NOTE]
    > Verifique se o DOCTYPE está definido como XHTML5 antes de prosseguir com as próximas etapas.
5. Edite o código e observe que o IntelliSense para JavaScript inclui recursos do HTML5, como o armazenamento local e seus métodos internos.

    ![Recursos JavaScript do HTML5 em JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "Recursos JavaScript do HTML5 em JavaScript")

    *Recursos JavaScript do HTML5 em JavaScript*
6. Clique em qualquer colchete de abertura ( **{** ) do código de script e observe que os colchetes são realçados.

    ![Os colchetes são realçados](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "Os colchetes são realçados")

    *Os colchetes são realçados*
7. Remova a marca de comentário da função **testAutoAlign ()** (selecione as três linhas e você pode usar **Ctrl** + **K**; **CTRL** + **U**) e localize o cursor dentro do código da função. Pressione Enter para acrescentar uma segunda linha. Observe que o código agora está **alinhado** e **recuado automaticamente**.

    ![O código JavaScript é alinhado automaticamente](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "O código JavaScript é alinhado automaticamente")

    *O código JavaScript é alinhado automaticamente*

<a id="Ex3Task2"></a>

<a id="Task_2_-_Validating_JavaScript"></a>
#### <a name="task-2---validating-javascript"></a>Tarefa 2-Validando o JavaScript

Nesta tarefa, você descobrirá a nova validação de JavaScript para o padrão ECMAScript5. Esse recurso ajudará você a escrever código JavaScript em conformidade, ao mesmo tempo que impede problemas de script antes da implantação do site.

> [!NOTE]
> O Visual Studio 2010 implementou a conformidade do ECMAStript3, enquanto o Visual Studio 2012 fornece a conformidade com o ECMAScript5.

1. Abra o **ECMA5script5. js** localizado na pasta **Scripts\custom** Project. Agora, você testará a validação do ECMAScript5 Standard.

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample8.html)]

    Você pode conferir o &quot; **usar** a direção de &quot; estrita na primeira linha do arquivo, o que habilita o **modo estrito**ECMAScript5. Esse modo consiste em um subconjunto da linguagem que esclarece as ambiguidades da edição anterior e adiciona alguns recursos novos, como getters e setters, suporte à biblioteca para JSON e reflexo mais completo nas propriedades do objeto.
2. Abrir o **lista de erros** se ainda não estiver aberto (menu**Exibir** | **Lista de erros**). Observe que a declaração da **função** é sublinhada. Isso ocorre porque as funções padrão ECMA5 não podem ser aninhadas dentro de estruturas de linguagem. Na lista de erros abaixo, você verá os detalhes do aviso.

    ![Mensagem de erro de validação de JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "Mensagem de erro de validação de JavaScript")

    *Mensagem de erro de validação de JavaScript*
3. Comente o **&quot;usar a direção&quot;estrita** e observe que os erros desaparecem, mas os avisos permanecem.
4. Na última linha do arquivo, escreva qualquer cadeia de caracteres como **&quot;&quot;de teste** (inclua as aspas para indicar que ele é uma cadeia de caracteres). Escreva um período ao lado da cadeia de caracteres para exibir a lista do IntelliSense e selecione a opção **aparar** .

    No padrão ECMAScript5, valores de cadeia de caracteres e variáveis também têm métodos de cadeia de caracteres definidos, como Trim, UpperCase, Pesquisar e substituir.

    ![Lista do IntelliSense em JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "Lista do IntelliSense em JavaScript")

    *Lista do IntelliSense em JavaScript*

<a id="Ex3Task3"></a>

<a id="Task_3_-_XML_Documentation_for_JavaScript"></a>
#### <a name="task-3---xml-documentation-for-javascript"></a>Tarefa 3-documentação XML para JavaScript

Nesta tarefa, você irá explorar os recursos do Visual Studio para documentação XML em JavaScript. Você verá que a lista JavaScript IntelliSense agora mostra a documentação XML de cada função. Além disso, você descobrirá o recurso de navegação em JavaScript.

1. Abra o arquivo **XMLDoc. js** localizado na pasta **scripts/projeto personalizado** . Esse arquivo contém a documentação XML em cada uma das funções JavaScript.

    ![Documentação XML do JavaScript integrada ao IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "Documentação XML do JavaScript integrada ao IntelliSense")

    *Documentação XML do JavaScript integrada ao IntelliSense*
2. Abaixo de **Adicionar** a função no arquivo **XMLDoc. js** , crie uma nova função chamada **Test**.
3. Na função de **teste** , chame a função de **multiplicação** que recebe dois parâmetros. Observe que a caixa dica de ferramenta está mostrando a documentação da função de **multiplicação** .

    [!code-javascript[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample9.js)]

    ![Documentação XML para funções JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "Documentação XML para funções JavaScript")

    *Documentação XML para funções JavaScript*
4. Conclua a instrução de chamada de função e digite um *ponto* para abrir a lista do IntelliSense no valor retornado. Observe que o Visual Studio está detectando o valor de **retorno** na documentação, tratando o valor como um número.

    ![Documentação XML para tipos de retorno](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "Documentação XML para tipos de retorno")

    *Documentação XML para tipos de retorno*
5. Agora, insira uma chamada para adicionar função. Observe que o editor de JavaScript agora dá suporte a sobrecargas de função. Ao escrever um nome de função, você poderá selecionar qualquer uma das sobrecargas disponíveis especificadas na documentação.

    ![Documentação XML para sobrecargas](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "Documentação XML para sobrecargas")

    *Documentação XML para sobrecargas*
6. Abra o arquivo **GotoDefinition. js** e localize a chamada de função **$ (). html ()** . Localize o cursor em **HTML**.
7. Pressione **F12** e navegue até a definição. Observe que agora você pode acessar e navegar em seu código JavaScript sem usar a ferramenta **Localizar** .
8. Localize o cursor na instrução jQuery antes do bloco de assinatura na parte inferior do arquivo de código. Pressione **F12**. Você navegará até o arquivo de biblioteca jQuery. Observe que você também pode navegar pelos arquivos do jQuery usando **F12**.

    ![Navegando para definições do jQuery](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "Navegando para definições do jQuery")

    *Navegando para definições do jQuery*

> [!NOTE]
> Certifique-se de que GotoDefinition. js não tenha erros de sintaxe antes de salvar o arquivo.

<a id="Exercise4"></a>

<a id="Exercise_4_Bundling_and_Minification"></a>
### <a name="exercise-4-bundling-and-minification"></a>Exercício 4: agrupamento e Minificação

Quantas vezes seus sites incluem mais de um arquivo JavaScript ou CSS? Esse é um cenário muito comum em que o agrupamento e o minificação podem ajudar a reduzir o tamanho do arquivo e fazer com que o site seja executado mais rapidamente. O novo recurso de agrupamento no ASP.NET 4,5 empacota um conjunto de arquivos JS ou CSS em um único elemento e reduz seu tamanho minificarndo o conteúdo (ou seja, removendo espaços em branco não necessários, removendo comentários, reduzindo identificadores).

O agrupamento e o minificação no ASP.NET 4,5 são executados em tempo de execução, de modo que o processo possa identificar o agente do usuário (por exemplo, IE, Mozilla, etc.) e, portanto, melhorar a compactação direcionando o navegador do usuário para (por exemplo, removendo as coisas específicas do Mozilla Quando a solicitação é proveniente do IE).

Neste exercício, você aprenderá a habilitar e usar os diferentes tipos de agrupamento e minificação no ASP.NET 4,5.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Installing_the_Bundling_and_Minification_Package_from_NuGet"></a>
#### <a name="task-1---installing-the-bundling-and-minification-package-from-nuget"></a>Tarefa 1-Instalando o pacote de agrupamento e Minificação do NuGet

1. Se ainda não estiver aberto, inicie o **Visual Studio** e abra a solução **WhatsNewASPNET. sln** localizada na pasta **Source\WhatsNewASPNET** deste laboratório.
2. Abra o console do Gerenciador de pacotes NuGet. Para fazer isso, use a **exibição** de menu | outro **console do Gerenciador de pacotes**do **Windows** | .

    ![Abrindo o Gerenciador de pacotes file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "Abrindo o console do Gerenciador de pacotes")

    *Abrindo o console do Gerenciador de pacotes*
3. No **console do Gerenciador de pacotes,** digite **install-Package Microsoft. Web. Optimization** e pressione **Enter**.

<a id="Ex4Task2"></a>

<a id="Task_2_-_Default_Bundles"></a>
#### <a name="task-2---default-bundles"></a>Tarefa 2-pacotes padrão

A maneira mais simples de usar o agrupamento e o minificação é habilitar os pacotes padrão. Esse método usa convenções para permitir que você referencie a versão agrupada e reduzidos para os arquivos JS e CSS em uma pasta.

Nesta tarefa, você aprenderá a habilitar e a fazer referência aos arquivos. JS e reduzidos de pacote e do CSS e exibir a saída resultante.

1. Se ainda não estiver aberto, inicie o **Visual Studio** e abra a solução **WhatsNewASPNET. sln** localizada na pasta **Source\WhatsNewASPNET** deste laboratório.
2. No **Gerenciador de soluções**, expanda as pastas **Styles**, **Scripts\custom** e **Scripts\bundle** .

    Observe que o aplicativo está usando mais de um arquivo CSS e JS.

    ![Várias folhas de estilo e arquivos JavaScript no aplicativo](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "Várias folhas de estilo e arquivos JavaScript no aplicativo")

    *Várias folhas de estilo e arquivos JavaScript no aplicativo*
3. Abra o arquivo **global.asax.cs** .

    Observe que o novo namespace **Microsoft. Web. Optimization** é comentado no início do arquivo. Remova a marca de comentário da diretiva using para incluir os recursos de agrupamento e minificação.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample10.cs)]
4. Localize o método de **início do aplicativo\_** .

    Nesse método, remova a marca de comentário da chamada EnableDefaultBundles, conforme mostrado no trecho abaixo. Isso nos permite referenciar uma coleção agrupada de arquivos CSS em uma pasta usando o caminho para essa pasta, além do &quot;&quot; CSS ou o sufixo de&quot; do &quot;JS.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample11.cs)]
5. Abra o arquivo **Optimization. aspx** e localize o controle de conteúdo para **HeadContent**.

    Observe que os arquivos CSS e os arquivos JS têm uma única marca referenciada.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample12.aspx)]

    > [!NOTE]
    > Esse código é para fins de demonstração. Idealmente, você fará referência aos grupos no arquivo site. master. Neste código de exemplo, você verá que alguns dos arquivos agrupados também estão sendo referenciados pelo arquivo site. Master, tornando essa última referência redundante.
6. Observe que os links estão usando as convenções de agrupamento no atributo **href** para obter todos os arquivos CSS ou js da pasta Styles e Scripts\custom, respectivamente.

    Você pode usar o caminho **scripts/Custom/js** , conforme mostrado abaixo para agrupar e reduzirr todos os arquivos JS dentro de um **script/pasta personalizada** . Esse é o comportamento padrão com os pacotes padrão.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample13.aspx)]
7. Abra o arquivo **Styles\Site.css** .

    Observe que o arquivo CSS original contém código recuado, espaços em branco e comentários que aumentam o arquivo. (Também o arquivo JavaScript contém espaços em branco e comentários).

    ![Um dos arquivos CSS originais na pasta scripts](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "Um dos arquivos CSS originais na pasta scripts")

    *Um dos arquivos CSS originais na pasta scripts*
8. Pressione **F5** para executar o aplicativo e navegue até a página **otimização** .
9. Clique no link do **pacote CSS** para baixar e abrir o arquivo.

    Confira o arquivo empacotado reduzidos. Você observará que todos os espaços em branco, comentários e caracteres de recuo foram removidos, gerando um arquivo menor.

    ![Arquivos CSS agrupados](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "Arquivos CSS agrupados")

    *Arquivos CSS agrupados*
10. Agora, clique no link do **pacote js** para abrir o arquivo de pacote do JavaScript. Você pode desconsiderar o aviso do Explorer com segurança. Observe que os arquivos JavaScript na pasta **personalizada** também são agrupados e reduzidos.

    ![Arquivos JavaScript agrupados](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "Arquivos JavaScript agrupados")

    *Arquivos JavaScript agrupados*

    Habilitar a compactação para arquivos CSS ou JS era muito mais complicado na versão anterior do ASP.NET. Agora, como você viu, basta adicionar uma linha no arquivo *global. asax* para habilitar o agrupamento e, em seguida, fazer referência aos arquivos agrupados do seu site.

<a id="Ex4Task3"></a>

<a id="Task_3_-_Static_Bundles"></a>
#### <a name="task-3---static-bundles"></a>Tarefa 3-pacotes estáticos

A abordagem de pacote estático permite que você personalize o conjunto de arquivos a serem agrupados, a referência e o método minificação que será usado.

Nesta tarefa, você configurará um pacote estático para definir um conjunto específico de arquivos para agrupar e reduzir.

1. Feche o navegador.
2. Abra o arquivo **global.asax.cs** e localize o **aplicativo\_método iniciar** .
3. Remova a marca de comentário do código do pacote estático, conforme mostrado no código abaixo.

    Você está definindo um pacote estático que será referenciado com o caminho virtual &quot; **~/StaticBundle**&quot; e usará **JsMinify** para minificação de todos os arquivos especificados com o método **AddFile** . Por fim, você está adicionando o pacote estático ao **conjunto de pacotes** e habilitando-o.

    Observe que os arquivos não estão localizados no mesmo local; Essa é outra vantagem sobre o agrupamento padrão.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample14.cs)]
4. Abra o arquivo **Optimization. aspx** .

    Observe que o link para o **pacote js estático** está usando o caminho declarado quando você configurou o pacote estático no arquivo global.asax.cs: **/StaticBundle**.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample15.aspx)]
5. Pressione **F5** para executar o aplicativo e, em seguida, navegue até a página **otimização** .
6. Clique no link do **pacote js estático** para abrir o arquivo.

    Observe que o arquivo reduzidos de JavaScript agrupado é a saída de todos os arquivos JavaScript configurados no arquivo de pacote estático no caminho &quot;/StaticBundle&quot;.

    ![Pacote de arquivos JavaScript estáticos](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "Pacote de arquivos JavaScript estáticos")

    *Pacote de arquivos JavaScript estáticos*
7. Feche o navegador e retorne ao Visual Studio.

<a id="Ex4Task4"></a>

<a id="Task_4_-_Dynamic_Folder_Bundles"></a>
#### <a name="task-4---dynamic-folder-bundles"></a>Tarefa 4-grupos de pastas dinâmicas

Nesta tarefa, você aprenderá a configurar os grupos de pastas dinâmicas. O poder do agrupamento dinâmico é que você pode incluir JavaScript estático, bem como outros arquivos em linguagens que são compilados em JavaScript e, portanto, exigem algum processamento antes da execução do agrupamento.

Neste exemplo, você aprenderá a usar a classe **DynamicFolderBundle** para criar um pacote dinâmico para arquivos gravados em CofeeScript. CofeeScript é uma linguagem de programação que é compilada em JavaScript e fornece uma sintaxe mais simples para escrever código JavaScript, aprimorando a brevidade e a legibilidade do JavaScript.

1. Abra o arquivo **global.asax.cs** e localize o **aplicativo\_método iniciar** .
2. Remova a marca de comentário do código do pacote dinâmico, conforme mostrado no código abaixo.

    Você está definindo um grupo de pastas dinâmicas que usará o processador minificação personalizado **CoffeeMinify** que se aplicará somente aos arquivos com a extensão &quot; **. café**&quot; (arquivos CoffeeScript). Observe que você pode usar um padrão de pesquisa para selecionar os arquivos a serem agrupados em uma pasta, como '\*. Coffee '.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample16.cs)]
3. Abra o console do Gerenciador de pacotes NuGet. Para fazer isso, use a **exibição** de menu | outro **console do Gerenciador de pacotes**do **Windows** | .
4. No **console do Gerenciador de pacotes,** digite **install-Package CoffeeSharp** e pressione **Enter**.
5. Clique no botão **Mostrar todos os arquivos** na janela **Gerenciador de soluções**

    ![Mostrando todos os arquivos](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "Mostrando todos os arquivos")

    *Mostrando todos os arquivos*
6. Clique com o botão direito do mouse no arquivo **CoffeeMinify.cs** na **Gerenciador de soluções** e selecione **incluir no projeto**

    ![Incluir o arquivo CoffeeMinify.cs no projeto](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "Incluir o arquivo CoffeeMinify.cs no projeto")

    *Incluir o arquivo CoffeeMinify.cs no projeto*
7. Abra o arquivo **CoffeeMinify.cs** .

    Essa classe herda de JsMinify para reduzir a saída de JavaScript resultante da compilação de código CoffeeScript. Ele chama o compilador CoffeeScript para gerar o código JavaScript primeiro e, em seguida, envia-o para o método JsMinify. Process para reduzir o código resultante.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample17.cs)]
8. Abra os arquivos **Script1. Coffee** e **Script2. café** da pasta **scripts/pacote** .

    Esses arquivos incluirão o código CoffeScript a ser compilado ao executar o agrupamento com a classe CoffeeMinify.

    Para fins de simplicidade, os arquivos CoffeeScript fornecidos estão apenas incluindo o código de exemplo CoffeeScript. Os comentários são excluídos pelo processo JsMinify.

    ![Arquivos CoffeeScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "Arquivos CoffeeScript")

    *Arquivos CoffeeScript*

    > [!NOTE]
    > O [CofeeScript](https://github.com/jashkenas/coffeescript/) fornece uma sintaxe mais simples para escrever código JavaScript, aprimorar a brevidade e a legibilidade do JavaScript, além de adicionar outros recursos, como a compreensão da matriz e a correspondência de padrões.
9. Abra o arquivo **Optimization. aspx** e localize os links de pacote.

    Observe que o link para o **pacote js dinâmico** está referenciando a pasta **scripts/pacote** usando o sufixo **/Coffee** configurado para o grupo de pastas dinâmicas.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample18.aspx)]
10. Pressione **F5** para executar o aplicativo e, em seguida, navegue até a página **otimização** .
11. Clique no link do **pacote js dinâmico** para abrir o arquivo gerado.

    Observe que o conteúdo que foi incluído neste pacote contém apenas arquivos **. café** . Você também pode ver que o código CoffeeScript foi compilado para JavaScript e que as linhas comentadas foram removidas.

    ![Pacote de arquivos JS dinâmicos](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "Pacote de arquivos JS dinâmicos")

    *Pacote de arquivos JS dinâmicos*

> [!NOTE]
> Além disso, você pode implantar esse aplicativo em sites do Windows Azure seguindo [o apêndice B: publicando um aplicativo ASP.NET MVC 4 usando implantação da Web](#AppendixB).

<a id="Summary"></a>
## <a name="summary"></a>Resumo

Este laboratório ajuda você a entender o que há de novo no ASP.NET e no desenvolvimento para a Web no Visual Studio 2012 e como aproveitar a variedade de aprimoramentos no Visual Studio 2012.

Ao concluir este laboratório prático, você aprenderá a usar os novos recursos e aprimoramentos dos editores do Visual Studio 2012 para CSS, JavaScript e HTML. Além disso, você aprenderá como o Visual Studio 2012 implementa o Agrupamento interno e o minificação.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apêndice A: Instalando o Visual Studio Express 2012 para Web

Você pode instalar o **Microsoft Visual Studio Express 2012 para Web** ou outra versão &quot;Express&quot; usando o **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** . As instruções a seguir orientam você pelas etapas necessárias para instalar o *Visual Studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.

1. Vá para [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Como alternativa, se você já tiver instalado o Web Platform Installer, poderá abri-lo e pesquisar o produto &quot;<em>Visual Studio Express 2012 para Web com o SDK do Windows Azure</em>&quot;.
2. Clique em **instalar agora**. Se você não tiver **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.
3. Quando **Web Platform Installer** estiver aberto, clique em **instalar** para iniciar a instalação.

    ![Instalar Visual Studio Express](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "Instalar Visual Studio Express")

    *Instalar Visual Studio Express*
4. Leia todos os termos e licenças de produtos e clique em **aceito** para continuar.

    ![Aceitando os termos de licença](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image60.png)

    *Aceitando os termos de licença*
5. Aguarde até que o processo de download e instalação seja concluído.

    ![Progresso da instalação](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image61.png)

    *Progresso da instalação*
6. Quando a instalação for concluída, clique em **concluir**.

    ![Instalação concluída](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image62.png)

    *Instalação concluída*
7. Clique em **sair** para fechar Web Platform Installer.
8. Para abrir o Visual Studio Express para Web, vá para a tela **Iniciar** e comece a escrever &quot;**vs Express**&quot;e, em seguida, clique no bloco **vs Express para Web** .

    ![Bloco VS Express para Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image63.png)

    *Bloco VS Express para Web*

---

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Apêndice B: publicando um aplicativo ASP.NET MVC 4 usando Implantação da Web

Este apêndice mostrará como criar um novo site da Web do Windows Azure Portal de Gerenciamento e publicar o aplicativo obtido seguindo o laboratório, aproveitando o recurso de publicação de Implantação da Web fornecido pelo Windows Azure.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Tarefa 1-Criando um novo site no portal do Windows Azure

1. Acesse o [portal de gerenciamento do Windows Azure](https://manage.windowsazure.com/) e entre usando as credenciais da Microsoft associadas à sua assinatura.

    > [!NOTE]
    > Com o Windows Azure, você pode hospedar 10 sites da Web de ASP.NET gratuitamente e, em seguida, dimensionar conforme o tráfego cresce. Você pode se inscrever [aqui](https://aka.ms/aspnet-hol-azure).

    ![Fazer logon no Windows portal do Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "Fazer logon no Windows portal do Azure")

    *Fazer logon no Windows Azure Portal de Gerenciamento*
2. Clique em **novo** na barra de comandos.

    ![Criando um novo site](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "Criando um novo site")

    *Criando um novo site*
3. Clique em **computação** | **site**. Em seguida, selecione a opção **criação rápida** . Forneça uma URL disponível para o novo site e clique em **criar site**.

    > [!NOTE]
    > Um site do Windows Azure é o host de um aplicativo Web em execução na nuvem que você pode controlar e gerenciar. A opção criação rápida permite que você implante um aplicativo Web concluído no site do Windows Azure de fora do Portal. Ele não inclui etapas para configurar um banco de dados.

    ![Criando um novo site usando a criação rápida](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "Criando um novo site usando a criação rápida")

    *Criando um novo site usando a criação rápida*
4. Aguarde até que o novo **site** seja criado.
5. Depois que o site for criado, clique no link sob a coluna **URL** . Verifique se o novo site está funcionando.

    ![Navegando até o novo site](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "Navegando até o novo site")

    *Navegando até o novo site*

    ![Site em execução](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "Site em execução")

    *Site em execução*
6. Volte para o portal e clique no nome do site na coluna **nome** para exibir as páginas de gerenciamento.

    ![Abrindo as páginas de gerenciamento de site](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "Abrindo as páginas de gerenciamento de site")

    *Abrindo as páginas de gerenciamento de site*
7. Na página **painel** , na seção **visão rápida** , clique no link **baixar perfil de publicação** .

    > [!NOTE]
    > O *perfil de publicação* contém todas as informações necessárias para publicar um aplicativo Web em um site do Windows Azure para cada método de publicação habilitado. O perfil de publicação contém as URLs, as credenciais de usuário e as cadeias de conexão de banco de dados necessárias para conectar-se e autenticar cada um dos pontos de extremidade para os quais um método de publicação é habilitado. **O Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** e **Microsoft Visual Studio 2012** dão suporte à leitura de perfis de publicação para automatizar a configuração desses programas para a publicação de aplicativos Web em sites do Windows Azure.

    ![Baixando o perfil de publicação do site](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "Baixando o perfil de publicação do site")

    *Baixando o perfil de publicação do site*
8. Baixe o arquivo de perfil de publicação em um local conhecido. Neste exercício, você verá como usar esse arquivo para publicar um aplicativo Web em um site do Windows Azure a partir do Visual Studio.

    ![Salvando o arquivo de perfil de publicação](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "Salvando o perfil de publicação")

    *Salvando o arquivo de perfil de publicação*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tarefa 2-Configurando o servidor de banco de dados

Se seu aplicativo utiliza bancos de dados SQL Server, você precisará criar um servidor de banco de dados SQL. Se você quiser implantar um aplicativo simples que não usa SQL Server você pode ignorar essa tarefa.

1. Você precisará de um servidor do banco de dados SQL para armazenar o banco de dados do aplicativo. Você pode exibir os servidores do banco de dados SQL de sua assinatura no portal de gerenciamento do Windows Azure em bancos de dados **sql** | **servidores** | **painel do servidor**. Se você não tiver um servidor criado, poderá criar um usando o botão **Adicionar** na barra de comandos. Anote o nome do **servidor e a URL, o nome de logon e a senha do administrador**, pois você irá usá-los nas próximas tarefas. Não crie o banco de dados ainda, pois ele será criado em um estágio posterior.

    ![Painel do servidor do banco de dados SQL](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "Painel do servidor do banco de dados SQL")

    *Painel do servidor do banco de dados SQL*
2. Na próxima tarefa, você testará a conexão de banco de dados do Visual Studio, por esse motivo, será necessário incluir o endereço IP local na lista de **endereços IP permitidos**do servidor. Para fazer isso, clique em **Configurar**, selecione o endereço IP do **endereço IP do cliente atual** e cole-o nas caixas de texto endereço IP **inicial** e **endereço IP final** . Insira um nome para a regra e clique no botão ![adicionar-Client-IP-address-Ok-Button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png).

    ![Adicionando endereço IP do cliente](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image74.png)

    *Adicionando endereço IP do cliente*
3. Depois que o **endereço IP do cliente** for adicionado à lista endereços IP permitidos, clique em **salvar** para confirmar as alterações.

    ![Confirmar alterações](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image75.png)

    *Confirmar alterações*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tarefa 3-publicando um aplicativo ASP.NET MVC 4 usando Implantação da Web

1. Volte para a solução ASP.NET MVC 4. Na **Gerenciador de soluções**, clique com o botão direito do mouse no projeto de site e selecione **publicar**.

    ![Publicando o aplicativo](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "Publicando o aplicativo")

    *Publicando o site*
2. Importe o perfil de publicação salvo na primeira tarefa.

    ![Importando o perfil de publicação](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "Importando o perfil de publicação")

    *Importando perfil de publicação*
3. Clique em **validar conexão**. Depois que a validação for concluída, clique em **Avançar**.

    > [!NOTE]
    > A validação será concluída quando você vir uma marca de seleção verde ao lado do botão Validar conexão.

    ![Validando conexão](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "Validando conexão")

    *Validando conexão*
4. Na página **configurações** , na seção **bancos** de dados, clique no botão ao lado da caixa de texto da sua conexão de banco (ou seja, **DefaultConnection**).

    ![Configuração de implantação da Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "Configuração de implantação da Web")

    *Configuração de implantação da Web*
5. Configure a conexão de banco de dados da seguinte maneira:

   - No **nome do servidor** , digite a URL do servidor do banco de dados SQL usando o prefixo *TCP:* .
   - Em **nome de usuário** , digite o nome de logon do administrador do servidor.
   - Em **senha** , digite a senha de logon do administrador do servidor.
   - Digite um novo nome de banco de dados, por exemplo: *MVC4SampleDB*.

     ![Configurando a cadeia de conexão de destino](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "Configurando a cadeia de conexão de destino")

     *Configurando a cadeia de conexão de destino*
6. Em seguida, clique em **OK**. Quando for solicitado a criar o banco de dados, clique em **Sim**.

    ![Criando o banco de dados](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "Criando a cadeia de caracteres do banco de dados")

    *Criando o banco de dados*
7. A cadeia de conexão que você usará para se conectar ao banco de dados SQL no Windows Azure é mostrada na caixa de texto conexão padrão. Em seguida, clique em **Próximo**.

    ![Cadeia de conexão apontando para o banco de dados SQL](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "Cadeia de conexão apontando para o banco de dados SQL")

    *Cadeia de conexão apontando para o banco de dados SQL*
8. Na página **Visualização** , clique em **publicar**.

    ![Publicando o aplicativo Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "Publicando o aplicativo Web")

    *Publicando o aplicativo Web*
9. Quando o processo de publicação for concluído, o navegador padrão abrirá o site publicado.

    ![Aplicativo publicado no Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "Aplicativo publicado no Windows Azure")

    *Aplicativo publicado no Windows Azure*
