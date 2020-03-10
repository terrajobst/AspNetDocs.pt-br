---
uid: web-forms/overview/getting-started/creating-a-basic-web-forms-page
title: Usando Visual Studio 2013 para criar uma página de Web Forms do ASP.NET 4,5 básico
author: Erikre
description: ''
ms.author: riande
ms.date: 03/03/2014
ms.assetid: a2f1c635-0817-4a9a-8c13-d5b5d29727c0
msc.legacyurl: /web-forms/overview/getting-started/creating-a-basic-web-forms-page
msc.type: authoredcontent
ms.openlocfilehash: 5d13a51128eecd92a82cfd06054448582a348e11
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78629755"
---
# <a name="using-visual-studio-2013-to-create-a-basic-aspnet-45-web-forms-page"></a>Usando Visual Studio 2013 para criar uma página de Web Forms do ASP.NET 4,5 básico

por [Erik Reitan](https://github.com/Erikre)

[!INCLUDE[](~/includes/rp.md)]

Este tutorial fornece uma introdução ao ambiente de desenvolvimento da Web no [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) e no [Microsoft Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). Este tutorial explica como criar uma página Web Forms ASP.NET simples e ilustra as técnicas básicas de criação de uma nova página, adição de controles e gravação de código.

As tarefas ilustradas neste passo a passo incluem:

- Criar um sistema de arquivos Web Forms projeto de aplicativo.
- Familiarizando-se com o Visual Studio.
- Criando uma página ASP.NET.
- Adicionando controles.
- Adicionando manipuladores de eventos.
- Executar e testar uma página do Visual Studio.

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

Nesta parte do passo a passos, você criará um projeto de aplicativo Web e adicionará uma nova página a ele. Você também adicionará texto HTML e executará a página em seu navegador.

### <a name="to-create-a-web-application-project"></a>Para criar um projeto de aplicativo Web

1. Abra o Microsoft Visual Studio.
2. No menu **Arquivo**, selecione **Novo Projeto**.  
    ![menu arquivo](creating-a-basic-web-forms-page/_static/image1.png)

    A caixa de diálogo **Novo Projeto** aparecerá.
3. Selecione os **modelos** -&gt; **Visual C#**  -&gt; grupo de modelos **da Web** à esquerda.
4. Selecione o modelo **Aplicativo Web ASP.NET** na coluna central.
5. Nomeie o projeto ***BasicWebApp*** e clique no botão **OK** .   
![Caixa de diálogo Novo Projeto](creating-a-basic-web-forms-page/_static/image2.png)
6. Em seguida, selecione o modelo de **Web Forms** e clique no botão **OK** para criar o projeto.  
![Caixa de diálogo Novo Projeto ASP .NET](creating-a-basic-web-forms-page/_static/image3.png)  

    O Visual Studio cria um novo projeto que inclui uma funcionalidade predefinida com base no modelo de Web Forms. Ele não apenas fornece uma página *Home. aspx* , uma página *about. aspx* , uma página *Contact. aspx* , mas também inclui a funcionalidade de associação que registra os usuários e salva suas credenciais para que eles possam fazer logon no seu site. Quando uma nova página é criada, por padrão, o Visual Studio exibe a página no modo de exibição de **código-fonte** , onde você pode ver os elementos HTML da página. A ilustração a seguir mostra o que você veria no modo de exibição de **origem** se você criou uma nova página da Web chamada *BasicWebApp. aspx*.  
    ![Modo de Exibição de Fonte](creating-a-basic-web-forms-page/_static/image4.png)

### <a name="a-tour-of-the-visual-studio-web-development-environment"></a>Um tour pelo ambiente de desenvolvimento da Web do Visual Studio

Antes de continuar modificando a página, é útil se familiarizar com o ambiente de desenvolvimento do Visual Studio. A ilustração a seguir mostra as janelas e ferramentas que estão disponíveis no Visual Studio e Visual Studio Express para Web.

> [!NOTE] 
> 
> Este diagrama mostra os locais padrão do Windows e da janela. O menu **Exibir** permite que você exiba janelas adicionais e reorganize e redimensione o Windows de acordo com suas preferências. Se as alterações já tiverem sido feitas no arranjo da janela, o que você verá não corresponderá à ilustração.

 O ambiente do Visual Studio

![Ambiente do Visual Studio](creating-a-basic-web-forms-page/_static/image5.png)

### <a name="familiarize-yourself-with-the-web-designer"></a>Familiarize-se com o Web designer

Examine a ilustração acima e corresponda o texto à lista a seguir, que descreve as janelas e ferramentas mais usadas. (Nem todas as janelas e ferramentas que você vê estão listadas aqui, somente aquelas marcadas na ilustração anterior.)

- Barras. Forneça comandos para formatar texto, localizar texto e assim por diante. Algumas barras de ferramentas estão disponíveis somente quando você está trabalhando no modo **design** .
- **Gerenciador de soluções** janela. Exibe os arquivos e pastas em seu aplicativo Web.
- Janela do documento. Exibe os documentos nos quais você está trabalhando em janelas com guias. Você pode alternar entre documentos clicando em guias.
- Janela **Propriedades** . Permite alterar as configurações da página, elementos HTML, controles e outros objetos.
- Exibir guias. Apresente a você diferentes exibições do mesmo documento. O modo **design** é uma superfície de edição quase WYSIWYG. O modo de exibição de **origem** é o editor de HTML da página. O modo de exibição de **divisão** exibe a exibição de **design** e a exibição de **código-fonte** do documento. Você trabalhará com as exibições de **design** e **código-fonte** posteriormente neste guia. Se você preferir abrir páginas da Web no modo de exibição de **design** , no menu **ferramentas** , clique em **Opções**, selecione o nó **Designer de HTML** e altere as **páginas iniciais na** opção.
- **Caixa de ferramentas**. Fornece controles e elementos HTML que você pode arrastar para sua página. Os elementos da **caixa de ferramentas** são agrupados por função comum.
- **Gerenciador do ervidor**. Exibe conexões de banco de dados. Se Gerenciador de Servidores não estiver visível, no menu Exibir, clique em Gerenciador de Servidores.

### <a name="creating-a-new-aspnet-web-forms-page"></a>Criando uma nova página de Web Forms do ASP.NET

Quando você cria um novo aplicativo de Web Forms usando o modelo de projeto de **aplicativo Web ASP.net** , o Visual Studio adiciona uma página ASP.net (página Web Forms) denominada *Default. aspx*, bem como vários outros arquivos e pastas. Você pode usar a página *Default. aspx* como o Home Page para seu aplicativo Web. No entanto, para este tutorial, você criará e trabalhará com uma nova página.

### <a name="to-add-a-page-to-the-web-application"></a>Para adicionar uma página ao aplicativo Web

1. Feche a página *Default. aspx* . Para fazer isso, clique na guia que exibe o nome do arquivo e, em seguida, clique na opção fechar.
2. Em **Gerenciador de soluções**, clique com o botão direito do mouse no nome do aplicativo Web (neste tutorial, o nome do aplicativo é **BasicWebSite**) e, em seguida, clique em **Adicionar** -&gt; **novo item**.   
A caixa de diálogo **Adicionar novo item** é exibida.
3. Selecione o grupo modelos **da Web** do **Visual C#**  -&gt; à esquerda. Em seguida, selecione **formulário da Web** na lista intermediária e nomeie-o *FirstWebPage. aspx*.   
    ![Caixa de diálogo Adicionar Novo Item](creating-a-basic-web-forms-page/_static/image6.png)
4. Clique em **Adicionar** para adicionar a página da Web ao seu projeto.  
O Visual Studio cria a nova página e a abre.

### <a name="adding-html-to-the-page"></a>Adicionando HTML à página

Nesta parte do passo a passos, você adicionará um texto estático à página.

### <a name="to-add-text-to-the-page"></a>Para adicionar texto à página

1. Na parte inferior da janela do documento, clique na guia **design** para alternar para o modo **design** .

    Modo de exibição de Design exibe a página atual de forma semelhante a WYSIWYG. Neste ponto, você não tem nenhum texto ou controle na página, portanto, a página está em branco, exceto por uma linha tracejada que descreve um retângulo. Esse retângulo representa um elemento **div** na página.
2. Clique dentro do retângulo que é descrito por uma linha tracejada.
3. Digite **Bem-vindo ao Visual Web Developer** e pressione **Enter** duas vezes.

    A ilustração a seguir mostra o texto que você digitou no modo **design** .

    ![Texto de boas-vindas no modo de exibição de Design](creating-a-basic-web-forms-page/_static/image7.png "Texto de boas-vindas no modo de exibição de Design")
4. Alternar para o modo de exibição de **código-fonte** .

    Você pode ver o HTML na exibição de **código-fonte** que você criou quando digitou no modo **design** .  
    ![página da Web com texto estático](creating-a-basic-web-forms-page/_static/image8.png)

### <a name="running-the-page"></a>Executando a página

Antes de continuar adicionando controles à página, você pode primeiro executá-lo.

### <a name="to-run-the-page"></a>Para executar a página

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse em *FirstWebPage. aspx* e selecione **definir como página inicial**.
2. Pressione **Ctrl + F5** para executar a página.

    A página é exibida no navegador. Embora a página que você criou tenha uma extensão de nome de arquivo *. aspx*, ela é executada atualmente como qualquer página HTML.

    Para exibir uma página no navegador, você também pode clicar com o botão direito do mouse na página em **Gerenciador de soluções** e selecionar **Exibir no navegador**.
3. Feche o navegador para parar o aplicativo Web.

## <a name="adding-and-programming-controls"></a>Adicionando e programando controles

<a id="sectionToggle1"></a>

Agora, você adicionará controles de servidor à página. Os controles de servidor, como botões, rótulos, caixas de texto e outros controles familiares, fornecem recursos típicos de processamento de formulário para suas páginas de Web Forms. No entanto, você pode programar os controles com o código que é executado no servidor, em vez do cliente.

Você adicionará um controle de [botão](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) , um controle [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) e um controle [rótulo](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) à página e escreverá o código para manipular o evento [Click](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) para o controle [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) .

### <a name="to-add-controls-to-the-page"></a>Para adicionar controles à página

1. Clique na guia **design** para alternar para o modo **design** .
2. Coloque o ponto de inserção no final do texto **Bem-vindo ao Visual Web Developer** e pressione **Enter** cinco ou mais vezes para criar algum espaço na caixa elemento **div** .
3. Na **caixa de ferramentas**, expanda o grupo **padrão** se ele ainda não estiver expandido.  
Observe que talvez seja necessário expandir a janela **caixa de ferramentas** à esquerda para exibi-la.
4. Arraste um controle [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) para a página e solte-o no meio da caixa do elemento **div** que tenha **Bem-vindo ao Visual Web Developer** na primeira linha.
5. Arraste um controle de [botão](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) para a página e solte-o à direita do controle [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) .
6. Arraste um controle [rótulo](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) para a página e solte-o em uma linha separada abaixo do controle [botão](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) .
7. Coloque o ponto de inserção acima do controle [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) e digite **insira seu nome:** .

    Esse texto HTML estático é a legenda do controle [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) . Você pode misturar controles HTML estáticos e de servidor na mesma página. A ilustração a seguir mostra como os três controles aparecem no modo **design** .

    ![Três controles no modo de exibição de Design](creating-a-basic-web-forms-page/_static/image9.png "Três controles no modo de exibição de Design")

### <a name="setting-control-properties"></a>Definindo propriedades do controle

O Visual Studio oferece várias maneiras de definir as propriedades de controles na página. Nesta parte do passo a passos, você definirá Propriedades no modo de exibição de **design** e no modo de exibição de **código-fonte** .

### <a name="to-set-control-properties"></a>Para definir as propriedades de controle

1. Primeiro, exiba as janelas de **Propriedades** selecionando no menu **Exibir** –&gt; **outra** **janela Propriedades**de &gt; de -do Windows. Como alternativa, você pode selecionar **F4** para exibir a janela **Propriedades** .
2. Selecione o controle [botão](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) e, na janela **Propriedades** , defina o valor do **texto** como nome para **exibição**. O texto que você inseriu aparece no botão no designer, conforme mostrado na ilustração a seguir.

    ![Definir texto do botão](creating-a-basic-web-forms-page/_static/image10.png "Definir texto do botão")
3. Alternar para o modo de exibição de **código-fonte** .

    Exibição de **código-fonte** EXIBE o HTML da página, incluindo os elementos que o Visual Studio criou para os controles de servidor. Os controles são declarados usando a sintaxe do tipo HTML, exceto que as marcas usam o prefixo **asp:** e incluem o atributo **runat =&quot;Server&quot;** .

    As propriedades de controle são declaradas como atributos. Por exemplo, quando você define a propriedade [Text](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.text.aspx) para o controle [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) , na etapa 1, na verdade você estava definindo o atributo **Text** na marcação do controle.

    > [!NOTE] 
    > 
    > Todos os controles estão dentro de um elemento **Form** , que também tem o atributo **runat =&quot;Server&quot;** . O atributo **runat =&quot;server&quot;** e o prefixo **asp:** para controlar marcas de controle marcam os controles para que sejam processados pelo ASP.net no servidor quando a página é executada. Código fora do **&lt;forma runat =&quot;server&quot;&gt;** e **&lt;script runat =&quot;Server&quot;&gt;** elementos são enviados inalterados para o navegador, motivo pelo qual o código ASP.NET deve estar dentro de um elemento cuja marca de abertura contenha o atributo **runat =&quot;Server&quot;** .
4. Em seguida, você adicionará uma propriedade adicional ao controle [rótulo](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) . Coloque o ponto de inserção diretamente após **asp: Label** na marca **&lt;asp: Label&gt;** e, em seguida, pressione **SPACEBAR**.

    É exibida uma lista suspensa que exibe a lista de propriedades disponíveis que podem ser definidas para um controle [rótulo](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) . Esse recurso, conhecido como **IntelliSense**, ajuda na exibição de **código-fonte** com a sintaxe de controles de servidor, elementos HTML e outros itens na página. A ilustração a seguir mostra a lista suspensa do **IntelliSense** para o controle [rótulo](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) .

    ![Atributos do IntelliSense](creating-a-basic-web-forms-page/_static/image11.png "Atributos do IntelliSense")
5. Selecione **ForeColor** e digite um sinal de igual.

    O IntelliSense exibe uma lista de cores.

    > [!NOTE] 
    > 
    > Você pode exibir uma lista suspensa do **IntelliSense** a qualquer momento pressionando **Ctrl + J** ao exibir o código.
6. Selecione uma cor para o texto do controle de **[rótulo](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)** . Certifique-se de selecionar uma cor que seja escura o suficiente para ler em um plano de fundo branco.

    O atributo **ForeColor** é concluído com a cor que você selecionou, incluindo as aspas de fechamento.

### <a name="programming-the-button-control"></a>Programando o controle de botão

Para esta explicação, você escreverá o código que lê o nome que o usuário insere na caixa de texto e, em seguida, exibe o nome no controle [rótulo](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) .

### <a name="add-a-default-button-event-handler"></a>Adicionar um manipulador de eventos de botão padrão

1. Alterne para o modo de exibição de **design** .
2. Clique duas vezes no controle de [botão](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) .

    Por padrão, o Visual Studio alterna para um arquivo code-behind e cria um manipulador de eventos de esqueleto para o evento padrão do controle de [botão](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) , o evento [Click](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) . O arquivo code-behind separa sua marcação de interface do usuário (como HTML) do seu código de servidor ( C#como).   
   O cursor está posicionado no código adicionado para este manipulador de eventos.

    > [!NOTE] 
    > 
    > Clicar duas vezes em um controle no modo de exibição de **design** é apenas uma das várias maneiras que você pode criar manipuladores de eventos.
3. Dentro do **Button1\_clique** no manipulador de eventos, digite **Label1** seguido por um ponto ( **.** ).

    Quando você digita o período após a **ID** do rótulo (**Label1**), o Visual Studio exibe uma lista de membros disponíveis para o controle [rótulo](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) , como mostrado na ilustração a seguir. Um membro normalmente é uma propriedade, um método ou um evento.

    ![IntelliSense na exibição de código](creating-a-basic-web-forms-page/_static/image12.png "IntelliSense na exibição de código")
4. Conclua o manipulador de eventos de **clique** para o botão para que ele seja lido conforme mostrado no exemplo de código a seguir.

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample1.cs?highlight=3)]

    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample2.vb?highlight=2)]
5. Volte para exibir a exibição de **origem** da marcação HTML clicando com o botão direito do mouse em *FirstWebPage. aspx* na **Gerenciador de soluções** e selecionando **Exibir marcação**.
6. Role até o elemento **&lt;ASP: Button&gt;** . Observe que o elemento do **&lt;ASP: Button&gt;** agora tem o atributo **onclick =&quot;Button1\_clique em&quot;** .

    Esse atributo associa o evento [Click](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) do botão ao método de manipulador que você códigou na etapa anterior.

    Os métodos do manipulador de eventos podem ter qualquer nome; o nome que você vê é o nome padrão criado pelo Visual Studio. O ponto importante é que o nome usado para o atributo **onclick** no HTML deve corresponder ao nome de um método definido no code-behind.

### <a name="running-the-page"></a>Executando a página

Agora você pode testar os controles de servidor na página.

### <a name="to-run-the-page"></a>Para executar a página

1. Pressione **Ctrl + F5** para executar a página no navegador. Se ocorrer um erro, verifique novamente as etapas acima.
2. Insira um nome na caixa de texto e clique no botão **exibir nome** .

    O nome que você inseriu é exibido no controle [rótulo](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) . Observe que, quando você clica no botão, a página é postada no servidor Web. Em seguida, ASP.NET recria a página, executa seu código (nesse caso, o manipulador de eventos [Click](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) do controle [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) é executado) e, em seguida, envia a nova página para o navegador. Se você observar a barra de status no navegador, poderá ver que a página está fazendo uma viagem de ida e volta ao servidor Web cada vez que você clicar no botão.
3. No navegador, exiba a origem da página que você está executando clicando com o botão direito do mouse na página e selecionando **Exibir origem**.

    No código-fonte da página, você vê HTML sem nenhum código de servidor. Especificamente, você não vê os elementos **&lt;ASP:&gt;** com os quais estava trabalhando na exibição de **código-fonte** . Quando a página é executada, o ASP.NET processa os controles de servidor e renderiza os elementos HTML para a página que executa as funções que representam o controle. Por exemplo, o controle de **&gt;de botão&lt;ASP:** é renderizado como o elemento HTML **&lt;entrada Type =&quot;enviar&quot;&gt;** Element.
4. Feche o navegador.

## <a name="working-with-additional-controls"></a>Trabalhando com controles adicionais

<a id="sectionToggle2"></a>

Nesta parte do passo a passos, você trabalhará com o controle [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) , que exibe datas um mês por vez. O controle [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) é um controle mais complexo do que o botão, a caixa de texto e o rótulo com os quais você tem trabalhado e ilustra alguns recursos adicionais dos controles de servidor.

Nesta seção, você adicionará um controle [System. Web. UI. WebControls. Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) à página e o formatará.

### <a name="to-add-a-calendar-control"></a>Para adicionar um controle de calendário

1. No Visual Studio, alterne para o modo de exibição de **design** .
2. Na seção **padrão** da caixa de **ferramentas**, arraste um controle de [calendário](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) para a página e solte-o abaixo do elemento **div** que contém os outros controles.

    O painel de marca inteligente do calendário é exibido. O painel exibe comandos que facilitam a execução das tarefas mais comuns para o controle selecionado. A ilustração a seguir mostra o controle de [calendário](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) como processado no modo de exibição de **design** .

    ![Controle de calendário no modo de exibição de Design](creating-a-basic-web-forms-page/_static/image13.png "Controle de calendário no modo de exibição de Design")
3. No painel de marcas inteligentes, escolha **formato automático**.

    A caixa de diálogo **formatação automática** é exibida, permitindo que você selecione um esquema de formatação para o calendário. A ilustração a seguir mostra a caixa de diálogo **formatação automática** do controle de [calendário](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) .

    ![Caixa de diálogo formatação automática (controle de calendário)](creating-a-basic-web-forms-page/_static/image14.png "Caixa de diálogo formatação automática (controle de calendário)")
4. Na lista **selecionar um esquema** , selecione **simples** e clique em **OK**.
5. Alternar para o modo de exibição de **código-fonte** .

    Você pode ver o elemento **&lt;ASP: Calendar&gt;** . Esse elemento é muito mais longo do que os elementos para os controles simples que você criou anteriormente. Ele também inclui subelementos, como **&lt;WeekEndDayStyle&gt;** , que representam várias configurações de formatação. A ilustração a seguir mostra o controle de [calendário](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) na exibição de **origem** . (A marcação exata que você vê no modo de exibição de **código-fonte** pode ser ligeiramente diferente da ilustração.)

    ![Controle de calendário na exibição de origem](creating-a-basic-web-forms-page/_static/image15.png "Controle de calendário na exibição de origem")

### <a name="programming-the-calendar-control"></a>Programando o controle de calendário

Nesta seção, você programará o controle de [calendário](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) para exibir a data atualmente selecionada.

### <a name="to-program-the-calendar-control"></a>Para programar o controle de calendário

1. No modo **design** , clique duas vezes no controle [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) .

    Um novo manipulador de eventos é criado e exibido no arquivo code-behind chamado *FirstWebPage.aspx.cs*.
2. Conclua o manipulador de eventos [SelectionChanged](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selectionchanged.aspx) com o código a seguir.

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample3.cs?highlight=3)]

    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample4.vb?highlight=2)]

    O código acima define o texto do controle rótulo como a data selecionada do controle Calendar.

### <a name="running-the-page"></a>Executando a página

Agora você pode testar o calendário.

### <a name="to-run-the-page"></a>Para executar a página

1. Pressione **Ctrl + F5** para executar a página no navegador.
2. Clique em uma data no calendário.

    A data em que você clicou é exibida no controle [rótulo](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) .
3. No navegador, exiba o código-fonte da página.

    Observe que o controle [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) foi renderizado para a página como uma **tabela**, com cada dia como um elemento **td** .
4. Feche o navegador.

## <a name="next-steps"></a>Próximas etapas

<a id="nextStepsToggle"></a>

Este tutorial ilustrou os recursos básicos do designer de página do Visual Studio. Agora que você entende como criar e editar uma página de Web Forms no Visual Studio, talvez queira explorar outros recursos. Por exemplo, talvez você queira fazer o seguinte:

- Saiba mais sobre o ASP.NET Web Forms seguindo a série de tutorial passo a passo [introdução com o ASP.NET 4,5 Web Forms e Visual Studio 2013](getting-started-with-aspnet-45-web-forms/introduction-and-overview.md).
- Saiba mais sobre folhas de estilo em cascata (CSS). Para obter detalhes, Confira como [trabalhar com a visão geral de CSS](https://msdn.microsoft.com/library/bb398931.aspx).
