---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: 'Laboratório prático: Visual Studio 2013 Web Tools | Microsoft Docs'
author: rick-anderson
description: O Visual Studio é um excelente ambiente de desenvolvimento para o. Projetos da Web e do Windows baseados em rede. Ele inclui um editor de texto poderoso que pode ser facilmente usado para...
ms.author: riande
ms.date: 07/16/2014
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: 2fb987dd9b26ad9f0e8a88fd881bde4505ec4148
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622671"
---
# <a name="hands-on-lab-visual-studio-2013-web-tools"></a>Laboratório prático: ferramentas da Web do Visual Studio 2013

por [equipe de acampamentos da Web](https://twitter.com/webcamps)

[Baixe o kit de treinamento do Web acampamentos](https://aka.ms/webcamps-training-kit)

> O Visual Studio é um excelente ambiente de desenvolvimento para o. Projetos da Web e do Windows baseados em rede. Ele inclui um editor de texto poderoso que pode ser facilmente usado para editar arquivos autônomos sem um projeto.
> 
> O Visual Studio mantém uma árvore de análise com recursos completos conforme você edita cada arquivo. Isso permite que o Visual Studio forneça ações de preenchimento automático e com base em documentos sem paralelo e, ao mesmo tempo, torna a experiência de desenvolvimento muito mais rápida e agradável. Esses recursos são especialmente poderosos em documentos HTML e CSS.
> 
> Toda essa potência também está disponível para extensões, tornando simples estender os editores com novos recursos avançados para atender às suas necessidades. O Web Essentials é uma coleção (principalmente) aprimoramentos relacionados à Web para o Visual Studio. Ele inclui muitas novas conclusões do IntelliSense (especialmente para CSS), novos recursos de link do navegador, JSHint automático para arquivos JavaScript, novos avisos para HTML e CSS e muitos outros recursos que são essenciais para o desenvolvimento para a Web moderno.
> 
> Todos os códigos de exemplo e trechos de código estão incluídos no kit de treinamento do acampamentos da Web, disponível em [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit).

<a id="Overview"></a>
## <a name="overview"></a>Visão geral

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

Neste laboratório prático, você aprenderá a:

- Usar novos recursos do editor de HTML incluídos no Web Essentials, como trechos de código do HTML5 e codificação de Zen avançados
- Usar os novos recursos do editor de CSS incluídos no Web Essentials, como o seletor de cores e a dica de ferramenta da matriz do navegador
- Usar novos recursos do editor de JavaScript incluídos no Web Essentials, como extrair para arquivo e IntelliSense para todos os elementos HTML
- Trocar dados entre o navegador e o Visual Studio usando o link do navegador

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisites

O seguinte é necessário para concluir este laboratório prático:

- [Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) ou superior
- [Web Essentials 2013](http://vswebessentials.com/)
- [Google Chrome](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a>Instalação

Para executar os exercícios neste laboratório prático, você precisará configurar seu ambiente primeiro...

1. Abra uma janela do Windows Explorer e navegue até a pasta de **origem** do laboratório.
2. Clique com o botão direito do mouse em **Setup. cmd** e selecione **Executar como administrador** para iniciar o processo de instalação que irá configurar seu ambiente e instalar os trechos de código do Visual Studio para este laboratório.
3. Se a caixa de diálogo controle de conta de usuário for exibida, confirme a ação para continuar.

> [!NOTE]
> Verifique se você verificou todas as dependências deste laboratório antes de executar a instalação.

<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Usando os trechos de código

Em todo o documento do laboratório, você será instruído a inserir blocos de código. Para sua conveniência, a maior parte desse código é fornecida como trechos de Visual Studio Code, que podem ser acessados em Visual Studio 2013 para evitar a necessidade de adicioná-lo manualmente.

> [!NOTE]
> Cada exercício é acompanhado por uma solução inicial localizada na pasta **begin** do exercício que permite que você siga cada exercício independentemente dos outros. Lembre-se de que os trechos de código que são adicionados durante um exercício estão ausentes dessas soluções iniciais e podem não funcionar até que você conclua o exercício. Dentro do código-fonte de um exercício, você também encontrará uma pasta **final** contendo uma solução do Visual Studio com o código que resulta da conclusão das etapas no exercício correspondente. Você pode usar essas soluções como diretrizes se precisar de ajuda adicional ao trabalhar com este laboratório prático.

---

<a id="Exercises"></a>
## <a name="exercises"></a>Exercícios

Este laboratório prático inclui os seguintes exercícios:

1. [Trabalhando com o link do navegador e o Web Essentials](#Exercise1)
2. [Tirando proveito dos trechos de código e do IntelliSense](#Exercise2)

> [!NOTE]
> Ao iniciar o Visual Studio pela primeira vez, você deve selecionar uma das coleções de configurações predefinidas. Cada coleção predefinida é projetada para corresponder a um estilo de desenvolvimento específico e determina layouts de janela, comportamento do editor, trechos de código IntelliSense e opções da caixa de diálogo. Os procedimentos neste laboratório descrevem as ações necessárias para realizar uma determinada tarefa no Visual Studio ao usar a coleção de **configurações de desenvolvimento geral** . Se você escolher uma coleção de configurações diferentes para seu ambiente de desenvolvimento, poderá haver diferenças nas etapas que você deve levar em conta.

<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a>Exercício 1: trabalhando com o link do navegador e o Web Essentials

O **Web Essentials** é uma extensão do Visual Studio que adiciona uma variedade de recursos úteis para o desenvolvimento para a Web moderno, principalmente focado em tornar a experiência de desenvolvimento para a Web muito mais rápida e agradável. Você pode instalar o Web Essentials da Galeria de extensões no Visual Studio.

O **link do navegador** é um novo recurso incluído no Visual Studio 2013 que fornece um canal entre o IDE do Visual Studio e qualquer navegador aberto para trocar dados entre o aplicativo Web e o Visual Studio. O Web Essentials estende o link do navegador com ferramentas para manipular o modelo de objeto DOM e os estilos CSS de suas páginas da Web diretamente do navegador.

Neste exercício, você explorará alguns dos recursos com suporte no **Web Essentials** e no **link do navegador** para aprimorar uma página de teste simples.

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a>Tarefa 1-executando o projeto em vários navegadores

Nesta tarefa, você configurará seu aplicativo Web para ser executado em vários navegadores de uma vez, o que é útil para testes entre navegadores.

1. Abra **Microsoft Visual Studio**.
2. No menu **arquivo** , selecione **abrir | Projeto/solução...** e navegue até **EX1-WorkingwithBrowserLinkandWebEssentials\Begin** na pasta de **origem** do laboratório (C:\WebCampsTK\HOL\VSWebTooling\Source). Selecione **Iniciar. sln** e clique em **abrir**.
3. Na barra de ferramentas do Visual Studio, expanda o menu navegador e selecione **procurar com.** ...

    ![Navegar com opção de menu](visual-studio-2013-web-tools/_static/image1.png "Procurar com... no menu do navegador")

    *Navegar com opção de menu*
4. Na caixa de diálogo **procurar por** , selecione o **Google Chrome** e o **Internet Explorer** mantendo a tecla **Ctrl** pressionada e clique em **definir como padrão**.

    ![Caixa de diálogo Procurar com](visual-studio-2013-web-tools/_static/image2.png "Caixa de diálogo Procurar com")

    *Selecionando vários navegadores padrão*
5. O Google Chrome e o Internet Explorer agora devem aparecer como os navegadores padrão. Clique em **Cancelar** para fechar a caixa de diálogo.

    ![Google Chrome e Internet Explorer como navegadores padrão](visual-studio-2013-web-tools/_static/image3.png "Navegadores padrão do Google Chrome e do Internet Explorer")

    *Google Chrome e Internet Explorer como navegadores padrão*

    > [!NOTE]
    > Depois de configurar os navegadores padrão, a opção **vários navegadores** é selecionada no menu do navegador.
    > 
    > ![Vários navegadores](visual-studio-2013-web-tools/_static/image4.png "Vários navegadores")
6. Pressione **CTRL** + **F5** para executar o aplicativo sem depuração.
7. Quando as janelas do navegador estiverem abertas, coloque uma delas acima da outra para ver as atualizações em ambos os navegadores simultaneamente. Os navegadores devem exibir uma página da Web com um retângulo azul claro.

    ![Colocando um navegador acima do outro](visual-studio-2013-web-tools/_static/image5.png "Colocando um navegador acima do outro")

    *Colocando um navegador acima do outro*
8. Não feche os navegadores. Você os usará na próxima tarefa.

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a>Tarefa 2-usando a codificação de Zen para criar elementos HTML

A **codificação Zen** é um plug-in de editor para código e edição de HTML de alta velocidade, XML, XSL (ou qualquer outro formato de código estruturado). O núcleo desse plug-in é um mecanismo de abreviação poderoso que permite expandir expressões, semelhantes aos seletores CSS, em código HTML. A codificação do Zen é uma maneira rápida de escrever HTML usando uma sintaxe de seletor de estilo CSS.

Neste exercício, você usará o recurso de codificação do Zen fornecido pelo Web Essentials para gerar os botões HTML que representam as opções da pergunta.

1. Volte para o Visual Studio.
2. Abra o arquivo **index. cshtml** localizado nas **exibições** | pasta **base** .
3. Substitua o **&lt;!--todo: adicionar opções aqui--&gt;** comentário com o código a seguir e pressione **Tab**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. O código deve ser expandido para HTML.

    ![HTML expandido](visual-studio-2013-web-tools/_static/image6.png "HTML expandido")

    *HTML expandido*

    > [!NOTE]
    > Para saber mais sobre a sintaxe de codificação do Zen, consulte o [artigo](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/)a seguir.
5. Clique no botão **Atualizar navegadores vinculados** para atualizar ambos os navegadores.

    ![Atualizar navegadores vinculados](visual-studio-2013-web-tools/_static/image7.png "Atualizar navegadores vinculados")

    *Atualizar navegadores vinculados*

    ![Internet Explorer-página atualizada com quatro botões](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer-página atualizada com quatro botões")

    *Internet Explorer-página atualizada com quatro botões*

    ![Google Chrome – página atualizada com quatro botões](visual-studio-2013-web-tools/_static/image9.png "Google Chrome – página atualizada com quatro botões")

    *Google Chrome – página atualizada com quatro botões*
6. Volte para o Visual Studio.
7. Você adicionou os botões à página, mas ainda precisa adicionar uma pergunta de modelo. Para fazer isso, você usará um novo recurso no Web Essentials chamado **gerador do Lorem Ipsum**. Localize o elemento **div** com o atributo de **classe** **front**.
8. Adicione o código a seguir como o primeiro elemento filho da **div**e pressione **Tab**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. O código deve ser expandido para HTML.

    ![Lorem com a ipsum gerada automaticamente](visual-studio-2013-web-tools/_static/image10.png "Lorem com a ipsum gerada automaticamente")

    *Lorem com a ipsum gerada automaticamente*

    > [!NOTE]
    > Como parte da codificação do Zen, agora você pode gerar código Lorem Ipsum diretamente no editor de HTML. Basta digitar **Lorem** e pressionar **Tab** e um texto de 30 palavras Lorem Ipsum será inserido. Por ex.: *lorem10* insere dez palavras Lorem Ipsum.
10. Você adicionará um logotipo na parte superior da pergunta usando outro recurso novo no Web Essentials chamado de **gerador de pixels de lorem**. Adicione o código a seguir como o primeiro elemento filho do elemento **div** com **contêiner** como valor de **classe** e pressione **Tab**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. O código deve expandir para HTML.

    ![Pixel gerado automaticamente em Lorem](visual-studio-2013-web-tools/_static/image11.png "Pixel gerado automaticamente em Lorem")

    *Pixel gerado automaticamente em Lorem*

    > [!NOTE]
    > Como parte da codificação do Zen, você também pode gerar código de pixel Lorem diretamente no editor de HTML. Basta digitar o **PIX-200 x 200-animais** e a **guia** de pressionamento e uma marca **img** com uma imagem 200 x 200 de um animal de animais será inserida. Para obter mais informações, consulte [Lorem pixel](http://www.lorempixel.com).
12. Clique no botão **Atualizar navegadores vinculados** para atualizar ambos os navegadores.

    ![Internet Explorer-imagem e texto gerados automaticamente](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer-imagem e texto gerados automaticamente")

    *Internet Explorer-imagem e texto gerados automaticamente*

    ![Google Chrome-imagem e texto gerados automaticamente](visual-studio-2013-web-tools/_static/image13.png "Google Chrome-imagem e texto gerados automaticamente")

    *Google Chrome-imagem e texto gerados automaticamente*

    > [!NOTE]
    > Como a imagem é selecionada aleatoriamente ao adicionar o trecho de código, a imagem mostrada nos navegadores pode diferir.
13. Não feche os navegadores. Você os usará na próxima tarefa.

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a>Tarefa 3-atualizando uma propriedade de estilo

Nesta tarefa, você usará o recurso **modo inspecionar** do link do navegador para detectar o local exato em que o elemento DOM específico é gerado e, em seguida, atualizar a Propriedade Color desse elemento usando um seletor de cores fornecido pelo Web Essentials.

1. No navegador Internet Explorer, pressione **CTRL** + **ALT** ** + para** habilitar o modo de inspeção.
2. Mova o ponteiro sobre a borda azul clara e clique em.

    ![Movendo o ponteiro sobre a borda azul clara](visual-studio-2013-web-tools/_static/image14.png "Movendo o ponteiro sobre a borda azul clara")

    *Movendo o ponteiro sobre a borda azul clara*
3. Volte para o Visual Studio. Observe como o elemento HTML que você selecionou no navegador também está selecionado no editor de HTML do Visual Studio.

    ![Elemento HTML selecionado no editor de HTML do Visual Studio](visual-studio-2013-web-tools/_static/image15.png "Elemento HTML selecionado no editor de HTML do Visual Studio")

    *Elemento HTML selecionado no editor de HTML do Visual Studio*
4. Agora, você atualizará a classe CSS **frontal** para alterar o estilo do elemento selecionado. Para fazer isso, pressione **CTRL** + **para** abrir a caixa **navegar até a** pesquisa. Digite **site. css** e pressione **Enter** para abrir o arquivo.

    ![Abrindo o arquivo site. css](visual-studio-2013-web-tools/_static/image16.png "Abrindo o arquivo site. css")

    *Abrindo o arquivo site. css*
5. Pressione **CTRL** + **F** e digite **. flip-container. Front** para localizar o seletor de CSS.
6. Clique no quadrado azul claro na propriedade border da classe para abrir o seletor de cores.

    ![Abrindo o seletor de cores](visual-studio-2013-web-tools/_static/image17.png "Abrindo o seletor de cores")

    *Abrindo o seletor de cores*
7. Expanda o seletor de cores clicando no botão de divisa e selecione uma nova cor.

    ![Expandindo o seletor de cores](visual-studio-2013-web-tools/_static/image18.png "Expandindo o seletor de cores")

    *Expandindo o seletor de cores*
8. Pressione **CTRL** + **ALT** + **Enter** para atualizar os navegadores vinculados.
9. Alterne para o Internet Explorer e observe como a cor da borda foi alterada.

    ![Internet Explorer-cor da borda atualizada](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer-cor da borda atualizada")

    *Internet Explorer-cor da borda atualizada*
10. Alterne para o Google Chrome e observe como a cor da borda foi alterada.

    ![Google Chrome-cor da borda atualizada](visual-studio-2013-web-tools/_static/image20.png "Google Chrome-cor da borda atualizada")

    *Google Chrome-cor da borda atualizada*
11. Volte para o Visual Studio.
12. Vá para o final do arquivo **site. css** e pressione **Ctrl** + **F** para localizar o seletor **. btn** .
13. Observe que a propriedade **-webkit-border-radius** está sublinhada em verde.

    ![-webkit-border-radius Propriedade do seletor de BTN](visual-studio-2013-web-tools/_static/image21.png "-webkit-border-radius Propriedade do seletor de BTN")

    *-webkit-border-radius Propriedade do seletor de BTN*
14. Coloque o acento circunflexo na propriedade **-webkit-border-radius** . Uma linha azul deve aparecer na primeira letra da primeira palavra da propriedade. Esta é a **marca inteligente**.
15. Pressione **CTRL** +  **.** para abrir o menu sugestões e clique em **adicionar propriedade padrão ausente (borda-raio)** .

    ![Adicionar sugestão de propriedade padrão ausente](visual-studio-2013-web-tools/_static/image22.png "Adicionar sugestão de propriedade padrão ausente")

    *Adicionar sugestão de propriedade padrão ausente*
16. A propriedade **border-radius** é adicionada automaticamente à regra de CSS.

    ![Propriedade padrão ausente adicionada](visual-studio-2013-web-tools/_static/image23.png "Propriedade padrão ausente adicionada")

    *Propriedade padrão ausente adicionada*
17. Mova o ponteiro sobre a propriedade **border-radius** para exibir a **dica de ferramenta da matriz do navegador**. A **dica de ferramenta de matriz do navegador** mostra a disponibilidade da propriedade em cada navegador.

    ![Dica de ferramenta de matriz do navegador](visual-studio-2013-web-tools/_static/image24.png "Dica de ferramenta de matriz do navegador")

    *Dica de ferramenta de matriz do navegador*
18. Observe que o valor da propriedade **border-radius** ainda está sublinhado. Mova o ponteiro sobre o valor para ver a mensagem de aviso.

    ![Aviso de valor da propriedade border-radius](visual-studio-2013-web-tools/_static/image25.png "Aviso de valor da propriedade border-radius")

    *Aviso de valor da propriedade border-radius*
19. Remova a unidade do valor da propriedade **border-radius** conforme sugerido pela dica de ferramenta.
20. Como **border-radius** é a propriedade padrão para definir como os cantos de borda arredondados são, você pode remover a propriedade **-webkit-border-radius** e o valor da regra de CSS.
21. Coloque o cursor na propriedade de **quebra automática de texto** e observe que a marca inteligente também aparece abaixo.
22. Abra o menu e clique em **Adicionar especificações de fornecedor ausentes**.

    ![Adicionar sugestão de especificações de fornecedor ausente](visual-studio-2013-web-tools/_static/image26.png "Adicionar sugestão de especificações de fornecedor ausente")

    *Adicionar sugestão de especificações de fornecedor ausente*
23. A propriedade **-MS-Word-Wrap** é adicionada automaticamente à regra de CSS.

    ![Propriedade específica do fornecedor adicionada](visual-studio-2013-web-tools/_static/image27.png "Propriedade específica do fornecedor adicionada")

    *Propriedade específica do fornecedor adicionada*

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a>Tarefa 4-Atualizando o código HTML do navegador

Nesta tarefa, você usará o recurso de modo de **design** do link do navegador para editar o objeto dom do navegador e transferir as alterações para o arquivo de origem HTML no Visual Studio.

1. No Google Chrome, pressione **CTRL** + **ALT** + **D** para habilitar o modo de design.
2. Mova o ponteiro sobre o rótulo **Lorem Ipsum dolor Sit Amet** e clique em.

    ![Editando a pergunta](visual-studio-2013-web-tools/_static/image28.png "Editando a pergunta")

    *Editando a pergunta*
3. Um cursor deve aparecer. Substitua o texto original pelo *que ele parece quando escrevo uma pergunta mais longa?* e pressione **ESC** para sair do modo de design.

    ![Pergunta editada](visual-studio-2013-web-tools/_static/image29.png "Pergunta editada")

    *Pergunta editada*
4. Volte para o Visual Studio e abra **index. cshtml**, se ainda não estiver aberto. Observe que o texto interno do elemento **&lt;p&gt;** foi atualizado.

    ![Pergunta atualizada na página HTML](visual-studio-2013-web-tools/_static/image30.png "Pergunta atualizada na página HTML")

    *Pergunta atualizada na página HTML*

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a>Tarefa 5-revisando avisos relacionados à SEO

A SEO ( **otimização do mecanismo de pesquisa** ) é o processo de tornar uma classificação de site mais alta na lista de resultados de um mecanismo de pesquisa. Quanto mais alta for a classificação do site e mais consistentemente ela estiver listada, mais visitantes o site receberá desse mecanismo de pesquisa. O Web Essentials incorpora uma ferramenta analítica que examina o HTML, relata os problemas encontrados e fornece assistência para corrigi-los.

1. Vá para o menu **Exibir** e clique em **lista de erros** para abrir a janela **lista de erros** .

    ![Lista de Erros no menu Exibir](visual-studio-2013-web-tools/_static/image31.png "Lista de Erros no menu Exibir")

    *Lista de Erros no menu Exibir*
2. Observe que há um aviso de SEO notificando que uma marca de **&gt;de&lt;** para a descrição da página está ausente. Clique duas vezes na entrada de aviso do SEO para corrigi-lo.

    ![Janela Lista de Erros](visual-studio-2013-web-tools/_static/image32.png "Janela Lista de Erros")

    *Janela Lista de Erros*
3. Na caixa de diálogo **Web Essentials** , clique em **Sim** para inserir uma descrição &lt;marca de meta&gt;.

    ![Caixa de diálogo Web Essentials](visual-studio-2013-web-tools/_static/image33.png "Caixa de diálogo Web Essentials")

    *Caixa de diálogo Web Essentials*
4. O editor para **\_layout. cshtml** é aberto e a marca **&lt;meta&gt;** é adicionada automaticamente à seção de **cabeçalho** do arquivo HTML.

    ![Marca meta adicionada automaticamente na página _Layout](visual-studio-2013-web-tools/_static/image34.png "Marca meta adicionada automaticamente na página _Layout")

    *Marca meta adicionada automaticamente à página de layout \_*
5. Altere o valor do atributo **Content** para *GeekQuiz* e salve o arquivo.

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a>Exercício 2: tirando proveito dos trechos de código e do IntelliSense

Com o Web Essentials, o editor de HTML foi estendido com funcionalidade extra. Neste exercício, você verá alguns recursos novos que são úteis ao desenvolver aplicativos Web.

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a>Tarefa 1-usando o IntelliSense em documentos HTML

O primeiro recurso novo que você verá nessa tarefa é chamado de **Dynamic IntelliSense**. O IntelliSense dinâmico lê outras marcas e atributos para inferir as possíveis IDs que serão usadas.

Nesta tarefa, você criará um novo elemento de formulário HTML que contém um rótulo e um campo de entrada. Em seguida, você adicionará um atributo **for** ao rótulo para associá-lo à entrada e verá as sugestões do IntelliSense com base nas IDs das entradas no escopo.

1. Abra **Visual Studio Express 2013 para Web** e a solução **begin. sln** localizada na pasta **Source/EX2-TakingAdvantageofCodeSnippetsandIntelliSense/Begin** . Como alternativa, você pode continuar com a solução que você obteve no exercício anterior.
2. No **Gerenciador de soluções**, abra o arquivo **index. cshtml** localizado nas **exibições** | pasta **base** .
3. Adicione o seguinte formulário dentro do elemento **&lt;seção&gt;** .

    (Trecho de código- *VisualStudio2013WebTooling* - *EX2* - *Form*)

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. A marca de entrada deve ser precedida por um rótulo com alguma descrição do campo. Adicione o seguinte rótulo antes da marca de entrada.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. O atributo **for** de um **rótulo de&lt;&gt;** especifica a qual elemento de formulário um rótulo está associado. O valor do atributo deve ser igual à ID do elemento relacionado. Adicione o atributo **for** ao elemento de **&lt;rótulo&gt;** . Conforme mostrado na figura a seguir, o nome da &quot;&quot; valor é exibido na caixa IntelliSense, com base na ID dos elementos dentro do mesmo escopo (o **&gt;do formulário de&lt;** de circunscrição).

    ![Mostrando a ID no IntelliSense](visual-studio-2013-web-tools/_static/image35.png "Mostrando a ID no IntelliSense")

    *Mostrando a ID no IntelliSense*
6. Exclua o **formulário de&lt;** adicionado recentemente&gt;elemento e seu conteúdo.

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a>Tarefa 2-usando trechos de código HTML

O HTML5 introduziu mais de 25 novas marcas semânticas. O Visual Studio já tinha suporte ao IntelliSense para essas marcas, mas Visual Studio 2013 torna mais rápido e fácil escrever marcação adicionando novos trechos de código. Embora essas marcas não sejam complicadas, elas vêm com algumas sutilezas pequenas, como adicionar os fallbacks de codec corretos para a marca de *áudio* . Nesta tarefa, você verá os trechos de código HTML para a marca de áudio.

1. No arquivo **index. cshtml** , digite **&lt;AUD** dentro da **seção&lt;&gt;** elemento, conforme mostrado na figura a seguir.

    ![Inserindo um elemento de áudio](visual-studio-2013-web-tools/_static/image36.png "Inserindo um elemento de áudio")

    *Inserindo um elemento de áudio*
2. Pressione **Tab** duas vezes e observe como o código a seguir é adicionado na página e o cursor é colocado no atributo **src** da primeira origem.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > Ao pressionar a tecla **Tab** duas vezes, o trecho de código é inserido. O trecho de áudio mostra o uso padrão da marca de *áudio* , com dois arquivos de origem para obter suporte aprimorado.
3. Exclua a segunda linha e atualize a origem da primeira linha com o link a seguir para o WebCampsTV Katana show: [http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3). O código resultante é mostrado abaixo.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > O arquivo de origem *KatanaProject. mp3* é usado como exemplo. Você pode usar outra fonte, se preferir.
4. Pressione **CTRL** + **S** para salvar o arquivo.
5. Pressione **CTRL** + **F5** para iniciar o aplicativo.
6. Observe que um player de áudio foi adicionado ao aplicativo.

    ![Player de áudio no Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Player de áudio no Internet Explorer")

    *Player de áudio no Internet Explorer*

    ![Player de áudio no Google Chrome](visual-studio-2013-web-tools/_static/image38.png "Player de áudio no Google Chrome")

    *Player de áudio no Google Chrome*
7. Não feche os navegadores. Você os usará na próxima tarefa.

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a>Tarefa 3-usando o IntelliSense em documentos JavaScript

Com o Web Essentials 2013, as folhas de estilo e as páginas HTML produzem uma lista de IDs e nomes de classe. Nesta tarefa, você aprenderá como essas listas aprimoram o suporte ao JavaScript IntelliSense no Web Essentials 2013.

1. No arquivo **index. cshtml** , adicione o código a seguir para definir uma marca de **script** para o código JavaScript.

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. Adicione o código a seguir dentro da marca de **script** para definir a função de retorno de chamada pronta.

    (Trecho de código- *VisualStudio2013WebTooling* - *EX2* - *ReadyFunction*)

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. Coloque o cursor na marca de **script** e pressione **Ctrl** +  **.** para abrir o menu de sugestões.
4. Clique em **extrair para arquivo**.

    ![Sugestão de extração de JavaScript para arquivo](visual-studio-2013-web-tools/_static/image39.png "Sugestão de extração de JavaScript para arquivo")

    *Sugestão de extração de JavaScript para arquivo*
5. Na janela **salvar como** , selecione a pasta **scripts** , nomeie o arquivo como **init. js** e clique em **salvar**.

    ![Salvar como janela](visual-studio-2013-web-tools/_static/image40.png "Salvar como janela")

    *Salvar como janela*

    > [!NOTE]
    > O arquivo **init. js** é criado e o conteúdo do script é movido para o arquivo.
    > 
    > ![Arquivo init. js criado com o conteúdo incluído](visual-studio-2013-web-tools/_static/image41.png "Arquivo init. js criado com o conteúdo incluído")
    > 
    > *Arquivo init. js criado com o conteúdo incluído*
6. Abra o arquivo **index. cshtml** e verifique se a marca do script foi substituída por uma referência ao arquivo **init. js** .

    ![Referência HTML init. js](visual-studio-2013-web-tools/_static/image42.png "Referência HTML init. js")

    *Referência HTML init. js*
7. Vá para a **Gerenciador de soluções** e observe que o arquivo **init. js** foi incluído automaticamente na solução.

    ![Arquivo init. js incluído na solução](visual-studio-2013-web-tools/_static/image43.png "Arquivo init. js incluído na solução")

    *Arquivo init. js incluído na solução*
8. Volte para o arquivo **init. js** para atualizar o retorno de chamada da função **pronta** .
9. Dentro da definição de retorno de chamada de função que é passada para *pronto*, adicione o código a seguir para obter todos os elementos por um atributo de classe específico.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. Pressione **CTRL** + **espaço** entre as aspas dentro da chamada de função **getElementsByClassName** .

    ![Mostrando o IntelliSense para a função getElementsByClassName](visual-studio-2013-web-tools/_static/image44.png "Mostrando o IntelliSense para a função getElementsByClassName")

    *Mostrando o IntelliSense para a função getElementsByClassName*

    > [!NOTE]
    > Observe que o IntelliSense mostra as classes definidas nas folhas de estilo do projeto.
11. Substitua a linha que você criou com o código a seguir.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. Posicione o cursor após a **au** dentro das aspas na função **GetElementsByTagName** e pressione **Ctrl** + **espaço**.

    ![Mostrando o IntelliSense para o método getElementByTagName](visual-studio-2013-web-tools/_static/image45.png "Mostrando o IntelliSense para o método getElementByTagName")

    *Mostrando o IntelliSense para o método getElementsByTagName*
13. Selecione **&quot;&quot;de áudio** na lista e pressione **Enter**. O resultado é mostrado na figura a seguir.

    ![Recuperando elementos de áudio](visual-studio-2013-web-tools/_static/image46.png "Recuperando elementos de áudio")

    *Recuperando elementos de áudio*
14. Em **Gerenciador de soluções**, clique com o botão direito do mouse no arquivo **init. js** na pasta **scripts** e selecione **arquivo (s) JavaScript reduzir** no menu do **Web Essentials** .

    ![Arquivo (s) reduzir JavaScript](visual-studio-2013-web-tools/_static/image47.png "Arquivos JavaScript reduzir")

    *Arquivo (s) reduzir JavaScript*
15. Quando for solicitado a habilitar o minificação automático quando o arquivo de origem for alterado, clique em **Sim**.

    ![Habilitando aviso automático de minificação](visual-studio-2013-web-tools/_static/image48.png "Habilitando aviso automático de minificação")

    *Habilitando aviso automático de minificação*

    > [!NOTE]
    > O **init. min. js** é criado e adicionado como uma dependência do arquivo **init. js** .
    > 
    > ![Arquivo init. min. js criado](visual-studio-2013-web-tools/_static/image49.png "Arquivo init. min. js criado")
    > 
    > *Arquivo init. min. js criado*
16. Abra o arquivo **init. min. js** e observe que o arquivo é reduzidos.

    ![Conteúdo do arquivo init. min. js](visual-studio-2013-web-tools/_static/image50.png "Conteúdo do arquivo init. min. js")

    *Conteúdo do arquivo init. min. js*
17. No arquivo **init. js** , adicione o seguinte código abaixo da chamada de função **GetElementsByTagName** para reproduzir todos os elementos de áudio.

    (Trecho de código- *VisualStudio2013WebTooling* - *EX2* - *PlayAudioElements*)

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. Clique em **CTRL** + **S** para salvar o arquivo. Como o arquivo reduzidos já está aberto, você verá uma caixa de diálogo informando que o arquivo foi modificado fora do editor de origem. Clique em **Sim**.

    ![Aviso de Microsoft Visual Studio](visual-studio-2013-web-tools/_static/image51.png "Aviso de Microsoft Visual Studio")

    *Aviso de Microsoft Visual Studio*
19. Volte para o arquivo **init. min. js** para verificar se o arquivo foi atualizado com o novo código.

    ![Arquivo init. min. js atualizado](visual-studio-2013-web-tools/_static/image52.png "Arquivo init. min. js atualizado")

    *Arquivo init. min. js atualizado*
20. Clique no botão **Atualizar do link do navegador** .
21. Depois que ambos os navegadores forem atualizados, os players de áudio vistos na tarefa anterior começarão a ser automaticamente reproduzidos.

    ![Player de áudio incluído na exibição](visual-studio-2013-web-tools/_static/image53.png "Player de áudio incluído na exibição")

    *Player de áudio incluído na exibição*

---

<a id="Summary"></a>
## <a name="summary"></a>Resumo

Ao concluir este laboratório prático, você aprendeu a:

- Usar novos recursos do editor de HTML incluídos no Web Essentials, como trechos de código do HTML5 e codificação de Zen avançados
- Usar os novos recursos do editor de CSS incluídos no Web Essentials, como o seletor de cores e a dica de ferramenta da matriz do navegador
- Usar novos recursos do editor de JavaScript incluídos no Web Essentials, como extrair para arquivo e IntelliSense para todos os elementos HTML
- Trocar dados entre o navegador e o Visual Studio usando o link do navegador
