---
uid: web-forms/overview/getting-started/code-editing-in-web-forms-pages
title: ASP.NET de edição de código Web Forms em Visual Studio 2013 | Microsoft Docs
author: Erikre
description: ''
ms.author: riande
ms.date: 03/03/2014
ms.assetid: 5344b74e-b888-479a-92bc-601a33bd61a2
msc.legacyurl: /web-forms/overview/getting-started/code-editing-in-web-forms-pages
msc.type: authoredcontent
ms.openlocfilehash: 3473ad476fbbebc58e12586334b4600f57cf17ed
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78632744"
---
# <a name="code-editing-aspnet-web-forms-in-visual-studio-2013"></a>Edição de código de Web Forms do ASP.NET no Visual Studio 2013

por [Erik Reitan](https://github.com/Erikre)

Em muitas páginas de formulário da Web do ASP.NET, você escreve código C#em Visual Basic, ou em outro idioma. O editor de código do Visual Studio pode ajudá-lo a escrever código rapidamente enquanto ajuda a evitar erros. Além disso, o Editor fornece maneiras de criar código reutilizável para ajudar a reduzir a quantidade de trabalho que você precisa fazer.

Este tutorial ilustra vários recursos do editor de código do Visual Studio.

Durante este passo a passo, você aprenderá a:

- Corrigir erros de codificação embutidos.
- Refatorar e renomear código.
- Renomear variáveis e objetos.
- Inserir trechos de código.

## <a name="prerequisites"></a>Prerequisites

Para concluir este passo a passo, você precisará de:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) ou [Microsoft Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). O .NET Framework é instalado automaticamente. 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 e Microsoft Visual Studio Express 2013 para a Web geralmente serão chamados de Visual Studio durante esta série de tutoriais.  
    >   
    > Se você estiver usando o Visual Studio, este passo a passos pressupõe que você selecionou a coleção de configurações de **desenvolvimento da Web** na primeira vez que iniciou o Visual Studio. Para obter mais informações, consulte [como: selecionar configurações de ambiente de desenvolvimento da Web](https://msdn.microsoft.com/library/ff521558.aspx).

## <a name="creating-a-web-application-project-and-a-page"></a>Criando um projeto de aplicativo Web e uma página

<a id="sectionToggle0"></a>

Nesta parte do passo a passos, você criará um projeto de aplicativo Web e adicionará uma nova página a ele.

### <a name="to-create-a-web-application-project"></a>Para criar um projeto de aplicativo Web

1. Abra o Microsoft Visual Studio.
2. No menu **Arquivo**, selecione **Novo Projeto**.  
    ![menu arquivo](code-editing-in-web-forms-pages/_static/image1.png)

    A caixa de diálogo **Novo Projeto** aparecerá.
3. Selecione os **modelos** -&gt; **Visual C#**  -&gt; grupo de modelos **da Web** à esquerda.
4. Selecione o modelo **Aplicativo Web ASP.NET** na coluna central.
5. Nomeie o projeto ***BasicWebApp*** e clique no botão **OK** .   
![Caixa de diálogo Novo Projeto](code-editing-in-web-forms-pages/_static/image2.png)
6. Em seguida, selecione o modelo de **Web Forms** e clique no botão **OK** para criar o projeto.  
![Caixa de diálogo Novo Projeto ASP .NET](code-editing-in-web-forms-pages/_static/image3.png)  

    O Visual Studio cria um novo projeto que inclui uma funcionalidade predefinida com base no modelo de Web Forms.

## <a name="creating-a-new-aspnet-web-forms-page"></a>Criando uma nova página de Web Forms do ASP.NET

Quando você cria um novo aplicativo de Web Forms usando o modelo de projeto de **aplicativo Web ASP.net** , o Visual Studio adiciona uma página ASP.net (página Web Forms) denominada *Default. aspx*, bem como vários outros arquivos e pastas. Você pode usar a página *Default. aspx* como o Home Page para seu aplicativo Web. No entanto, para este tutorial, você criará e trabalhará com uma nova página.

### <a name="to-add-a-page-to-the-web-application"></a>Para adicionar uma página ao aplicativo Web

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no nome do aplicativo Web (neste tutorial, o nome do aplicativo é **BasicWebSite**) e, em seguida, clique em **Adicionar** -&gt; **novo item**.   
A caixa de diálogo **Adicionar novo item** é exibida.
2. Selecione o grupo modelos **da Web** do **Visual C#**  -&gt; à esquerda. Em seguida, selecione **formulário da Web** na lista intermediária e nomeie-o *FirstWebPage. aspx*.   
    ![Caixa de diálogo Adicionar Novo Item](code-editing-in-web-forms-pages/_static/image4.png)
3. Clique em **Adicionar** para adicionar a página de Web Forms ao seu projeto.  
 O Visual Studio cria a nova página e a abre.
4. Em seguida, defina essa nova página como a página de inicialização padrão. Em **Gerenciador de soluções**, clique com o botão direito do mouse na nova página chamada *FirstWebPage. aspx* e selecione **definir como página inicial**. Na próxima vez que você executar esse aplicativo para testar nosso progresso, você verá automaticamente essa nova página no navegador.

## <a name="correcting-inline-coding-errors"></a>Corrigindo erros de codificação embutida

O editor de código do Visual Studio ajuda a evitar erros à medida que você escreve o código e, se você tiver cometido um erro, o editor de código o ajudará a corrigir o erro. Nesta parte, você escreverá uma linha de código que ilustra os recursos de correção de erros no editor.

### <a name="to-correct-simple-coding-errors-in-visual-studio"></a>Para corrigir erros de codificação simples no Visual Studio

1. No modo **design** , clique duas vezes na página em branco para criar um manipulador para o evento de **carregamento** da página.   
   Você está usando o manipulador de eventos somente como um local para escrever algum código.
2. Dentro do manipulador, digite a seguinte linha que contém um erro e pressione **Enter**:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample1.cs)]

   Quando você pressiona **Enter**, o editor de código coloca sublinhados em verde e vermelho (normalmente chamada &quot;linhas de&quot; onduladas) em áreas do código que apresentam problemas. Um sublinhado verde indica um aviso. Um sublinhado vermelho indica um erro que você deve corrigir. 

    Mantenha o ponteiro do mouse sobre `myStr` para ver uma dica de ferramenta que informa sobre o aviso. Além disso, mantenha o ponteiro do mouse sobre o sublinhado vermelho para ver a mensagem de erro.

    A imagem a seguir mostra o código com os sublinhados.

    ![Texto de boas-vindas no modo de exibição de Design](code-editing-in-web-forms-pages/_static/image5.png "Texto de boas-vindas no modo de exibição de Design")  
   O erro deve ser corrigido adicionando-se um ponto-e-vírgula `;` ao final da linha. O aviso simplesmente notifica que você ainda não usou a variável `myStr`.  

    > [!NOTE] 
    > 
    > Você exibe suas configurações de formatação de código atuais no Visual Studio selecionando **ferramentas** -**opções** de &gt; -&gt; **fontes e cores**.

## <a name="refactoring-and-renaming"></a>Refatoração e renomeação

A refatoração é uma metodologia de software que envolve a reestruturação do código para facilitar a compreensão e a manutenção, enquanto preserva sua funcionalidade. Um exemplo simples pode ser que você escreva o código em um manipulador de eventos para obter dados de um banco de dado. Ao desenvolver sua página, você descobre que precisa acessar os dados de vários manipuladores diferentes. Portanto, você refatoria o código da página criando um método de acesso a dados na página e inserindo chamadas para o método nos manipuladores.

O editor de código inclui ferramentas para ajudá-lo a executar várias tarefas de refatoração. Neste tutorial, você trabalhará com duas técnicas de refatoração: renomeando variáveis e extraindo métodos. Outras opções de refatoração incluem o encapsulamento de campos, a promoção de variáveis locais para parâmetros de método e o gerenciamento de parâmetros de método. A disponibilidade dessas opções de refatoração depende do local no código.

### <a name="refactoring-code"></a>Código de refatoração

Um cenário de refatoração comum é criar (extrair) um método do código que está dentro de outro membro, como um método. Isso reduz o tamanho do membro original e torna o código extraído reutilizável.

Nesta parte, você escreverá um código simples e, em seguida, extrairá um método dele. A refatoração tem suporte C#para o, portanto, você criará uma C# página que usa como linguagem de programação.

### <a name="to-extract-a-method-in-a-c-page"></a>Para extrair um método em uma C# página

1. Alterne para o modo de exibição de **design** .
2. Na **caixa de ferramentas**, na guia **padrão** , arraste um controle de [botão](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) para a página.
3. Clique duas vezes no controle de **botão** para criar um manipulador para seu evento de [clique](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) e, em seguida, adicione o seguinte código realçado:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample2.cs?highlight=3-16)]

   O código cria um objeto **ArrayList** , usa um loop para carregá-lo com valores e, em seguida, usa outro loop para exibir o conteúdo do objeto **ArrayList** .
4. Pressione **Ctrl + F5** para executar a página e, em seguida, clique no **botão** para verificar se você vê a seguinte saída:   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample3.html)]
5. Retorne ao editor de códigos e, em seguida, selecione as linhas a seguir no manipulador de eventos.   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample4.html)]
6. Clique com o botão direito do mouse na seleção, clique em **Refactor**e escolha **extrair método**. 

    A caixa de diálogo **extrair método** é exibida.
7. Na caixa **nome do novo método** , digite **DisplayArray**e clique em **OK**. 

    O editor de código cria um novo método chamado `DisplayArray`e coloca uma chamada para o novo método no manipulador de **cliques** em que o loop foi originalmente.

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample5.cs?highlight=12)]
8. Pressione **Ctrl + F5** para executar a página novamente e clique no **botão**.

    A página funciona da mesma forma que antes. O método `DisplayArray` agora pode ser chamado de qualquer lugar na classe de página.

## <a name="renaming-variables"></a>Renomeando variáveis

Quando você trabalha com variáveis, bem como objetos, talvez queira renomeá-las depois que elas já estiverem referenciadas em seu código. No entanto, renomear variáveis e objetos pode fazer com que o código seja interrompido se você perder a renomeação de uma das referências. Portanto, você pode usar a refatoração para executar a renomeação.

### <a name="to-use-refactoring-to-rename-a-variable"></a>Para usar a refatoração para renomear uma variável

1. No manipulador de eventos de **clique** , localize a seguinte linha:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample6.cs)]
2. Clique com o botão direito do mouse no nome da variável `alist`, escolha **Refactor**e, em seguida, escolha **renomear**.

    A caixa de diálogo **renomear** é exibida.
3. Na caixa **novo nome** , digite **ArrayList1** e verifique se a caixa de seleção **alterações de referência de visualização** foi selecionada. Em seguida, clique em **OK**.

    A caixa de diálogo **Visualizar alterações** é exibida e exibe uma árvore que contém todas as referências à variável que você está renomeando.
4. Clique em **aplicar** para fechar a caixa de diálogo **Visualizar alterações** .

    As variáveis que se referem especificamente à instância que você selecionou são renomeadas. No entanto, observe que a variável `alist` na linha a seguir não é renomeada.

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample7.cs)]

    A variável `alist` nesta linha não foi renomeada porque não representa o mesmo valor que a variável `alist` que você renomeou. A variável `alist` na declaração de `DisplayArray` é uma variável local para esse método. Isso ilustra que o uso de refatoração para renomear variáveis é diferente de simplesmente executar uma ação de localizar e substituir no editor; a refatoração renomeia variáveis com o conhecimento da semântica da variável com a qual ela está trabalhando.

## <a name="inserting-snippets"></a>Inserindo snippets

Como há muitas tarefas de codificação que Web Forms os desenvolvedores frequentemente precisam executar, o editor de código fornece uma biblioteca de trechos de código ou blocos de códigos pré-gravados. Você pode inserir esses trechos de código em sua página.

Cada idioma que você usa no Visual Studio tem pequenas diferenças na maneira como você insere trechos de código. Para obter informações sobre como inserir trechos, consulte [Visual Basic trechos de código IntelliSense](https://msdn.microsoft.com/library/18yz4be4.aspx). Para obter informações sobre como inserir trechos no C#Visual, [consulte C# trechos de código Visual](https://msdn.microsoft.com/library/z41h7fat.aspx).

## <a name="next-steps"></a>Próximas etapas

Este tutorial ilustrou os recursos básicos do editor de código do Visual Studio 2010 para corrigir erros em seu código, refatorar o código, renomear variáveis e inserir trechos de código em seu código. Recursos adicionais no editor podem tornar o desenvolvimento de aplicativos rápido e fácil. Por exemplo, você pode querer:

- Saiba mais sobre os recursos do IntelliSense, como a modificação de opções do IntelliSense, o gerenciamento de trechos de código e a pesquisa de trechos de código online. Para obter mais informações, veja [Usando o IntelliSense](https://msdn.microsoft.com/library/hcw1s69b.aspx).
- Saiba como criar seus próprios trechos de código. Para obter mais informações, consulte [criando e usando trechos de código IntelliSense](https://msdn.microsoft.com/library/ms165392.aspx)
- Saiba mais sobre os recursos específicos do Visual Basic de trechos de código IntelliSense, como a personalização dos trechos e a solução de problemas. Para obter mais informações, consulte [Visual Basic trechos de código IntelliSense](https://msdn.microsoft.com/library/18yz4be4.aspx)
- Saiba mais sobre os C#recursos específicos do IntelliSense, como refatoração e trechos de código. Para obter mais informações, [consulte C# Visual IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx).
