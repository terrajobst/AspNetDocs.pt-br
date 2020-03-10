---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: Auxiliares, formulários e validação do ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: Nos modelos do ASP.NET MVC 4 e no laboratório prático de acesso a dados, você está carregando e exibindo dados do banco de dado. Neste laboratório prático, você adicionará ao...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: 0e2605a4188eaf814f6ab0ebfeaabed4457bcfa3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539574"
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a>Validação, formulários e auxiliares do ASP.NET MVC 4

por [equipe de acampamentos da Web](https://twitter.com/webcamps)

[Baixe o kit de treinamento do Web acampamentos](https://aka.ms/webcamps-training-kit)

Nos **modelos do ASP.NET MVC 4 e** no laboratório prático de acesso a dados, você está carregando e exibindo dados do banco de dado. Neste laboratório prático, você adicionará ao aplicativo de loja de **música** a capacidade de editar esses dados.

Com essa meta em mente, primeiro você criará o controlador que dará suporte às ações CRUD (criar, ler, atualizar e excluir) dos álbuns. Você vai gerar um modelo de exibição de índice aproveitando o recurso scaffolding do ASP.NET MVC para exibir as propriedades de álbuns em uma tabela HTML. Para aprimorar essa exibição, você adicionará um auxiliar HTML personalizado que truncará descrições longas.

Posteriormente, você adicionará as exibições editar e criar que permitirão que você altere os álbuns no banco de dados, com a ajuda de elementos de formulário como listas suspensas.

Por fim, você permitirá que os usuários excluam um álbum e também os impedirão de inserir dados errados Validando sua entrada.

Este laboratório prático pressupõe que você tenha conhecimento básico do **ASP.NET MVC**. Se você não usou o **ASP.NET MVC** antes, recomendamos que você vá para o laboratório prático do **ASP.NET MVC Fundamentals** .

Este laboratório orienta você pelos aprimoramentos e novos recursos descritos anteriormente, aplicando pequenas alterações a um aplicativo Web de exemplo fornecido na pasta de origem.

> [!NOTE]
> Todos os códigos de exemplo e trechos de código estão incluídos no Web acampamentos Training Kit, disponível nas [versões Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). O projeto específico deste laboratório está disponível em [auxiliares, formulários e validação do ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

Neste laboratório prático, você aprenderá a:

- Criar um controlador para dar suporte a operações CRUD
- Gerar uma exibição de índice para exibir propriedades de entidade em uma tabela HTML
- Adicionar um auxiliar HTML personalizado
- Criar e personalizar um modo de exibição de edição
- Diferenciar os métodos de ação que reagem às chamadas HTTP-GET ou HTTP-POST
- Adicionar e personalizar um modo de exibição de criação
- Manipular a exclusão de uma entidade
- Valide a entrada do usuário

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisites

Você deve ter os seguintes itens para concluir este laboratório:

- [Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou superior (leia [o apêndice a](#AppendixA) para obter instruções sobre como instalá-lo).

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Instalação

**Instalando trechos de código**

Para sua conveniência, grande parte do código que você gerenciará ao longo deste laboratório está disponível como trechos de código do Visual Studio. Para instalar os trechos de código, execute o arquivo **.\Source\Setup\CodeSnippets.VSI** .

Se você não estiver familiarizado com os trechos de Visual Studio Code e quiser saber como usá-los, consulte o apêndice deste documento &quot;[Apêndice B: usando trechos de código](#AppendixB)&quot;.

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Exercícios

Os seguintes exercícios compõem este laboratório prático:

1. [Criando o controlador do Store Manager e sua exibição de índice](#Exercise1)
2. [Adicionando um auxiliar HTML](#Exercise2)
3. [Criando o modo de exibição de edição](#Exercise3)
4. [Adicionando um modo de exibição de criação](#Exercise4)
5. [Tratamento da exclusão](#Exercise5)
6. [Adicionando uma Validação](#Exercise6)
7. [Usando o jQuery discreto no lado do cliente](#Exercise7)

> [!NOTE]
> Cada exercício é acompanhado por uma pasta **final** que contém a solução resultante que você deve obter depois de concluir os exercícios. Você pode usar essa solução como um guia se precisar de ajuda adicional para trabalhar com os exercícios.

Tempo estimado para concluir este laboratório: **60 minutos**

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a>Exercício 1: criando o controlador do Store Manager e sua exibição de índice

Neste exercício, você aprenderá a criar um novo controlador para dar suporte a operações CRUD, personalizar seu método de ação de índice para retornar uma lista de álbuns do banco de dados e, finalmente, gerar um modelo de exibição de índice aproveitando o scaffolding do ASP.NET MVC recurso para exibir as propriedades de álbuns em uma tabela HTML.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a>Tarefa 1-criando o StoreManagerController

Nesta tarefa, você criará um novo controlador chamado **StoreManagerController** para dar suporte a operações CRUD.

1. Abra a solução **inicial** localizada na **origem/EX1-CreatingTheStoreManagerController/início/** pasta.

   1. Será necessário baixar alguns pacotes NuGet ausentes antes de continuar. Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.
   2. Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, Compile a solução clicando em **build** | **Compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto. É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.
2. Adicione um novo controlador. Para fazer isso, clique com o botão direito do mouse na pasta **controladores** dentro da Gerenciador de soluções, selecione **Adicionar** e, em seguida, o comando **controlador** . Altere o **nome** do controlador para **StoreManagerController** e verifique se a opção **controlador MVC com ações de leitura/gravação vazias** está selecionada. Clique em **Adicionar**.

    ![Caixa de diálogo Adicionar controlador](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Caixa de diálogo Adicionar controlador")

    *Caixa de diálogo Adicionar controlador*

    Uma nova classe de controlador é gerada. Como você indicou para adicionar ações de leitura/gravação, métodos de stub para eles, ações CRUD comuns são criadas com comentários TODO preenchidos, solicitando que incluam a lógica específica do aplicativo.

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a>Tarefa 2-Personalizando o índice Storemanager

Nesta tarefa, você personalizará o método de ação de índice Storemanager para retornar uma exibição com a lista de álbuns do banco de dados.

1. Na classe StoreManagerController, adicione as seguintes diretivas *using* .

    (Trecho de código- *auxiliares e formulários do ASP.NET MVC 4 e validação-EX1 usando MvcMusicStore*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. Adicione um campo ao **StoreManagerController** para manter uma instância de **MusicStoreEntities.**

    (Trecho de código- *auxiliares ASP.NET MVC 4 e Forms e Validation-EX1 MusicStoreEntities*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. Implemente a ação de índice StoreManagerController para retornar uma exibição com a lista de álbuns.

    A lógica de ação do controlador será muito semelhante à ação de índice do StoreController gravada anteriormente. Use o LINQ para recuperar todos os álbuns, incluindo informações de gênero e artista para exibição.

    (Trecho de código- *ASP.net e auxiliares MVC 4 e Forms e Validation-EX1 StoreManagerController index*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a>Tarefa 3-criando a exibição de índice

Nesta tarefa, você criará o modelo de exibição de índice para exibir a lista de álbuns retornados pelo controlador **storemanager** .

1. Antes de criar o novo modelo de exibição, você deve compilar o projeto para que a **caixa de diálogo Adicionar exibição** saiba sobre a classe de **álbum** a ser usada. Selecionar **compilação | Crie MvcMusicStore** para compilar o projeto.
2. Clique com o botão direito do mouse dentro do método de ação de **índice** e selecione **Adicionar exibição**. Isso abrirá a caixa de diálogo **Adicionar exibição** .

    ![Adicionar modo de exibição](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Adicionar modo de exibição")

    *Adicionando uma exibição de dentro do método de índice*
3. Na caixa de diálogo Adicionar exibição, verifique se o nome da exibição é **índice**. Selecione a opção **criar uma exibição fortemente tipada** e selecione **álbum (MvcMusicStore. Models)** na lista suspensa **classe de modelo** . Selecione **list na lista** suspensa **modelo de Scaffold** . Deixe o **mecanismo de exibição** para o **Razor** e os outros campos com seu valor padrão e clique em **Adicionar**.

    ![Adicionando uma exibição de índice](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Adicionando uma exibição de índice")

    *Adicionando uma exibição de índice*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a>Tarefa 4-Personalizando o scaffold da exibição do índice

Nesta tarefa, você ajustará o modelo de exibição simples criado com o recurso ASP.NET MVC scaffolding para que ele exiba os campos desejados.

> [!NOTE]
> O suporte do **scaffolding** no ASP.NET MVC gera um modelo de exibição simples que lista todos os campos no modelo de álbum. O **scaffolding** fornece uma maneira rápida de começar a usar uma exibição fortemente tipada: em vez de ter que escrever o modelo de exibição manualmente, o scaffolding gera rapidamente um modelo padrão e, em seguida, você pode modificar o código gerado.

1. Examine o código criado. A lista de campos gerada fará parte da seguinte tabela HTML que o **scaffolding** está usando para exibir dados tabulares.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. Substitua a **tabela&lt;&gt;** código pelo código a seguir para exibir apenas os campos **gênero**, **artista**, **título do álbum**e **preço** . Isso exclui as colunas de URL do **álbumid** e da **arte do álbum** . Além disso, ele altera as colunas Gêneroid e Artistaid para exibir suas propriedades de classe vinculadas de **Artist.Name** e **Genre.Name**e remove o link **Details** .

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. Altere as descrições a seguir.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Tarefa 5-executando o aplicativo

Nesta tarefa, você testará que o modelo de exibição de **índice** do **storemanager** exibe uma lista de álbuns de acordo com o design das etapas anteriores.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home Page. Altere a URL para **/StoreManager** para verificar se uma lista de álbuns é exibida, mostrando seu **título**, **artista** e **gênero**.

    ![Navegando na lista de álbuns](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Navegando na lista de álbuns")

    *Navegando na lista de álbuns*

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a>Exercício 2: adicionando um auxiliar HTML

A página de índice do Storemanager tem um possível problema: as propriedades de título e de nome do artista podem ser longas o suficiente para lançar a formatação da tabela. Neste exercício, você aprenderá a adicionar um auxiliar HTML personalizado para truncar esse texto.

Na figura a seguir, você pode ver como o formato é modificado devido ao comprimento do texto quando você usa um pequeno tamanho de navegador.

![Navegando na lista de álbuns com texto não truncado](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "Navegando na lista de álbuns com texto não truncado")

*Navegando na lista de álbuns com texto não truncado*

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a>Tarefa 1-estendendo o auxiliar HTML

Nesta tarefa, você adicionará um novo método **truncado** para o objeto **HTML** exposto nas exibições do ASP.NET MVC. Para fazer isso, você vai implementar um **método de extensão** para a classe **System. Web. Mvc. HtmlHelper** interna fornecida pelo ASP.NET MVC.

> [!NOTE]
> Para ler mais sobre os **métodos de extensão**, visite este artigo do MSDN. [https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).

1. Abra a solução **inicial** localizada na **origem/EX2-AddingAnHTMLHelper/início/** pasta. Caso contrário, você pode continuar usando a solução **final** obtida concluindo o exercício anterior.

   1. Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar. Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.
   2. Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, Compile a solução clicando em **build** | **Compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto. É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.
2. Abra a exibição de índice do Storemanager. Para fazer isso, no Gerenciador de Soluções expanda a pasta **exibições** , em seguida, o **storemanager** e abra o arquivo **index. cshtml** .
3. Adicione o seguinte código abaixo da diretiva <strong>@model</strong> para definir o método auxiliar <strong>TRUNCATE</strong> .

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a>Tarefa 2 – truncando texto na página

Nesta tarefa, você usará o método **TRUNCATE** para truncar o texto no modelo de exibição.

1. Abra a exibição de índice do Storemanager. Para fazer isso, no Gerenciador de Soluções expanda a pasta **exibições** , em seguida, o **storemanager** e abra o arquivo **index. cshtml** .
2. Substitua as linhas que mostram o **nome do artista** e o **título**do álbum. Para fazer isso, substitua as linhas a seguir.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tarefa 3-executando o aplicativo

Nesta tarefa, você testará que o modelo de exibição de **índice** do **storemanager** trunca o título e o nome do artista do álbum.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home Page. Altere a URL para **/StoreManager** para verificar se textos longos na coluna **título** e **artista** estão truncados.

    ![Nomes de títulos e artistas truncados](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "Nomes de títulos e artistas truncados")

    *Títulos truncados e nomes de artistas*

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a>Exercício 3: criando o modo de exibição de edição

Neste exercício, você aprenderá a criar um formulário para permitir que os gerentes de loja editem um álbum. Eles procurarão a URL **/StoreManager/Edit/ID** (**ID** sendo a ID exclusiva do álbum a ser editada), fazendo assim uma chamada HTTP-Get para o servidor.

O método de ação de edição do controlador recuperará o álbum apropriado do banco de dados, criará um objeto **StoreManagerViewModel** para encapsulá-lo (junto com uma lista de artistas e gêneros) e, em seguida, passá-lo para um modelo de exibição para renderizar a página HTML de volta para o usuário. Esta página conterá um **&lt;formulário&gt;** elemento com caixas de Texte listas suspensas para editar as propriedades do álbum.

Depois que o usuário atualiza os valores de formulário do álbum e clica no botão **salvar** , as alterações são enviadas por meio de uma chamada http-post de volta para **/StoreManager/Edit/ID**. Embora a URL permaneça a mesma da última chamada, o ASP.NET MVC identifica que, desta vez é um HTTP-POST e, portanto, executa um método de ação de edição diferente (um decorado com **[HttpPost]** ).

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a>Tarefa 1-implementando o método de ação de edição HTTP-GET

Nesta tarefa, você implementará a versão HTTP-GET do método editar ação para recuperar o álbum apropriado do banco de dados, bem como uma lista de todos os gêneros e artistas. Ele empacotará esses dados no objeto **StoreManagerViewModel** definido na última etapa, que será então passada para um modelo de exibição para renderizar a resposta.

1. Abra a solução **inicial** localizada na **origem/EX3-CreatingTheEditView/início/** pasta. Caso contrário, você pode continuar usando a solução **final** obtida concluindo o exercício anterior.

   1. Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar. Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.
   2. Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, Compile a solução clicando em **build** | **Compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto. É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.
2. Abra a classe **StoreManagerController** . Para fazer isso, expanda a pasta **controladores** e clique duas vezes em **StoreManagerController.cs**.
3. Substitua o método de ação de **edição http-Get** pelo código a seguir para recuperar o **álbum** apropriado, bem como as listas de **gêneros** e **artistas** .

    (Trecho de código- *ASP.net e auxiliares MVC 4 e formulários e validação – EX3 STOREMANAGERCONTROLLER http – obter ação de edição*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > Você está usando a lista de **seleção** de **System. Web. Mvc** para artistas e gêneros em vez de **System. Collections. genérica** List.
    > 
    > A lista de **seleção** é uma maneira mais limpa de preencher listas suspensas HTML e gerenciar coisas como a seleção atual. Instanciar e posteriormente configurar esses objetos ViewModel na ação do controlador tornará o limpador do cenário Editar formulário.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a>Tarefa 2-Criando o modo de exibição de edição

Nesta tarefa, você criará um modelo de exibição de edição que exibirá posteriormente as propriedades do álbum.

1. Crie o modo de exibição de edição. Para fazer isso, clique com o botão direito do mouse dentro do método **Editar** ação e selecione **Adicionar exibição**.
2. Na caixa de diálogo Adicionar exibição, verifique se o nome da exibição é **Editar**. Marque a caixa de seleção **criar uma exibição fortemente tipada** e selecione **álbum (MvcMusicStore. Models)** na lista suspensa **Exibir classe de dados** . Selecione **Editar** na lista suspensa **modelo de Scaffold** . Deixe os outros campos com o valor padrão e clique em **Adicionar**.

    ![Adicionando uma exibição de edição](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Adicionando uma exibição de edição")

    *Adicionando uma exibição de edição*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tarefa 3-executando o aplicativo

Nesta tarefa, você testará que a página de exibição de **edição** do **storemanager** exibe os valores de propriedades para o álbum passado como parâmetro.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home Page. Altere a URL para **/StoreManager/Edit/1** para verificar se os valores das propriedades para o álbum passado são exibidos.

    ![Modo de exibição de edição do álbum de navegação](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Modo de exibição de edição do álbum de navegação")

    *Modo de exibição de edição do álbum de navegação*

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a>Tarefa 4 – implementando os menus suspensos no modelo do editor de álbuns

Nesta tarefa, você adicionará os menus suspensos ao modelo de exibição criado na última tarefa, para que o usuário possa selecionar em uma lista de artistas e gêneros.

1. Substitua todo o código do fieldset do **álbum** pelo seguinte:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > Um auxiliar **HTML. DropDownList** foi adicionado para os menus suspensos de renderização para escolher artistas e gêneros. Os parâmetros passados para **HTML. DropDownList** são:
    > 
    > 1. O nome do campo de formulário ( **&quot;artistaid&quot;** ).
    > 2. A lista de valores de **Select** para o menu suspenso.

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Tarefa 5-executando o aplicativo

Nesta tarefa, você testará que a página de exibição de **edição** do **storemanager** exibe menus suspensos em vez dos campos de texto ID do artista e gênero.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home Page. Altere a URL para **/StoreManager/Edit/1** para verificar se ela exibe os menus suspensos em vez dos campos de texto ID do artista e gênero.

    ![Navegando na exibição de edição do álbum com os menus suspensos](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Navegando na exibição de edição do álbum com os menus suspensos")

    *Pesquisando o modo de exibição de álbum, desta vez com listas suspensas*

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a>Tarefa 6-implementando o método de ação HTTP-POST Edit

Agora que o modo de exibição de edição é exibido conforme o esperado, você precisa implementar o método de ação HTTP-POST Edit para salvar as alterações feitas no álbum.

1. Feche o navegador, se necessário, para retornar à janela do Visual Studio. Abra o **StoreManagerController** na pasta **controladores** .
2. Substitua o código do método de ação **http-Post Edit** pelo seguinte (Observe que o método que deve ser substituído é a versão sobrecarregada que recebe dois parâmetros):

    (Trecho de código- *ASP.net e auxiliares MVC 4 e Forms e Validation – EX3 STOREMANAGERCONTROLLER http-Post Edit Action*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > Esse método será executado quando o usuário clicar no botão **salvar** da exibição e executará um http-post dos valores do formulário de volta para o servidor para mantê-los no banco de dados. O decorador **[HttpPost]** indica que o método deve ser usado para os cenários http-post. O método usa um objeto de **álbum** . O ASP.NET MVC criará automaticamente o objeto de álbum do formulário &lt;Postado&gt; valores.
    > 
    > O método executará estas etapas:
    > 
    > 1. Se o modelo for válido:
    > 
    >     1. Atualize a entrada do álbum no contexto para marcá-la como um objeto modificado.
    >     2. Salve as alterações e redirecione-as para a exibição de índice.
    > 2. Se o modelo não for válido, ele preencherá ViewBag com o **gêneroid** e a **artistaid**, retornará a exibição com o objeto de álbum recebido para permitir que o usuário execute qualquer atualização necessária.

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>Tarefa 7-executando o aplicativo

Nesta tarefa, você testará que a página de exibição de **edição do storemanager** , na verdade, salva os dados de álbum atualizados no banco de dado.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home Page. Altere a URL para **/StoreManager/Edit/1**. Altere o título do álbum para **carregar** e clique em **salvar**. Verifique se o título do álbum realmente mudou na lista de álbuns.

    ![Atualizando um álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "Atualizando um álbum")

    *Atualizando um álbum*

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a>Exercício 4: adicionando um modo de exibição de criação

Agora que o **StoreManagerController** dá suporte à capacidade de **edição** , neste exercício, você aprenderá a adicionar um modelo Create View para permitir que os gerentes de armazenamento adicionem novos álbuns ao aplicativo.

Como você fez com a funcionalidade de edição, implementará o cenário de criação usando dois métodos separados dentro da classe **StoreManagerController** :

1. Um método de ação exibirá um formulário vazio quando os gerentes de loja visitarem primeiro a URL **/StoreManager/Create** .
2. Um segundo método de ação manipulará o cenário em que o Gerenciador de loja clica no botão **salvar** dentro do formulário e envia os valores de volta para a URL **/STOREMANAGER/Create** como um http-post.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a>Tarefa 1-implementando o método de ação de criação HTTP-GET

Nesta tarefa, você implementará a versão HTTP-GET do método criar ação para recuperar uma lista de todos os gêneros e artistas, empacotando esses dados em um objeto **StoreManagerViewModel** , que será então passado para um modelo de exibição.

1. Abra a solução **inicial** localizada na **origem/Ex4-AddingACreateView/início/** pasta. Caso contrário, você pode continuar usando a solução **final** obtida concluindo o exercício anterior.

   1. Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar. Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.
   2. Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, Compile a solução clicando em **build** | **Compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto. É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.
2. Abra a classe **StoreManagerController** . Para fazer isso, expanda a pasta **controladores** e clique duas vezes em **StoreManagerController.cs**.
3. Substitua o código de método de ação **Create** pelo seguinte:

    (Trecho de código- *auxiliares do ASP.NET MVC 4 e formulários e validação-Ex4 STOREMANAGERCONTROLLER http-Get criar ação*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a>Tarefa 2-adicionando o modo de exibição de criação

Nesta tarefa, você adicionará o modelo criar exibição que exibirá um novo formulário de álbum (vazio).

1. Clique com o botão direito do mouse dentro do método **criar** ação e selecione **Adicionar exibição**. Isso abrirá a caixa de diálogo Adicionar exibição.
2. Na caixa de diálogo Adicionar exibição, verifique se o nome da exibição é **criar**. Selecione a opção **criar uma exibição fortemente tipada** e selecione **álbum (MvcMusicStore. Models)** na lista suspensa **classe de modelo** e **criar** na lista suspensa modelo de **Scaffold** . Deixe os outros campos com o valor padrão e clique em **Adicionar**.

    ![Adicionando um modo de exibição de criação](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "adding-a-Create-View. png")

    *Adicionando o modo de exibição de criação*
3. Atualize os campos **gêneroid** e **artistaid** para usar uma lista suspensa, conforme mostrado abaixo:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tarefa 3-executando o aplicativo

Nesta tarefa, você testará que a página **criar** exibição do **storemanager** exibe um formulário de álbum vazio.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home Page. Altere a URL para **/StoreManager/Create**. Verifique se um formulário vazio é exibido para preencher as novas propriedades do álbum.

    ![Criar modo de exibição com um formulário vazio](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Criar modo de exibição com um formulário vazio")

    *Criar modo de exibição com um formulário vazio*

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a>Tarefa 4-implementando o método de ação HTTP-POST Create

Nesta tarefa, você implementará a versão HTTP-POST do método criar ação que será invocado quando um usuário clicar no botão **salvar** . O método deve salvar o novo álbum no banco de dados.

1. Feche o navegador, se necessário, para retornar à janela do Visual Studio. Abra a classe **StoreManagerController** . Para fazer isso, expanda a pasta **controladores** e clique duas vezes em **StoreManagerController.cs**.
2. Substitua o código do método de ação **http-post Create** pelo seguinte:

    (Trecho de código- *ASP.net e auxiliares MVC 4 e formulários e validação – Ex4 STOREMANAGERCONTROLLER http-post criar ação*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > A ação criar é bastante semelhante ao método de ação editar anterior, mas em vez de definir o objeto como modificado, ele está sendo adicionado ao contexto.

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Tarefa 5-executando o aplicativo

Nesta tarefa, você testará que a página **criar exibição do storemanager** permite criar um novo álbum e, em seguida, redireciona para a exibição do índice storemanager.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home Page. Altere a URL para **/StoreManager/Create**. Preencha todos os campos de formulário com dados para um novo álbum, como aquele na figura a seguir:

    ![Criando um álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "Criando um álbum")

    *Criando um álbum*
3. Verifique se você foi redirecionado para a exibição de índice do Storemanager que inclui o novo álbum recém-criado.

    ![Novo álbum criado](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "Novo álbum criado")

    *Novo álbum criado*

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a>Exercício 5: manipulando a exclusão

A capacidade de excluir álbuns ainda não foi implementada. É isso que se trata deste exercício. Assim como antes, você implementará o cenário de exclusão usando dois métodos separados dentro da classe **StoreManagerController** :

1. Um método de ação exibirá um formulário de confirmação
2. Um segundo método de ação manipulará o envio do formulário

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a>Tarefa 1-implementando o método de ação de exclusão HTTP-GET

Nesta tarefa, você implementará a versão HTTP-GET do método Excluir ação para recuperar as informações do álbum.

1. Abra a solução **inicial** localizada na **origem/Ex5-HandlingDeletion/início/** pasta. Caso contrário, você pode continuar usando a solução **final** obtida concluindo o exercício anterior.

   1. Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar. Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.
   2. Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, Compile a solução clicando em **build** | **Compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto. É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.
2. Abra a classe **StoreManagerController** . Para fazer isso, expanda a pasta **controladores** e clique duas vezes em **StoreManagerController.cs**.
3. A ação Excluir controlador é exatamente igual à ação do controlador de detalhes da loja anterior: consulta o objeto de **álbum** do banco de dados usando a **ID** fornecida na URL e retorna a **exibição**apropriada. Para fazer isso, substitua o código do método de ação de **exclusão** http-Get pelo seguinte:

    (Trecho de código- *auxiliares do ASP.NET MVC 4 e formulários e validação-Ex5 tratamento de exclusão http-obter ação de exclusão*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. Clique com o botão direito do mouse dentro do método de ação **excluir** e selecione **Adicionar exibição**. Isso abrirá a caixa de diálogo Adicionar exibição.
5. Na caixa de diálogo Adicionar exibição, verifique se o nome da exibição é **excluir**. Selecione a opção **criar uma exibição fortemente tipada** e selecione **álbum (MvcMusicStore. Models)** na lista suspensa **classe de modelo** . Selecione **excluir** na lista suspensa **modelo de Scaffold** . Deixe os outros campos com o valor padrão e clique em **Adicionar**.

    ![Adicionando uma exibição de exclusão](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Adicionando uma exibição de exclusão")

    *Adicionando uma exibição de exclusão*
6. O modelo de exclusão mostra todos os campos do modelo. Você mostrará apenas o título do álbum. Para fazer isso, substitua o conteúdo da exibição pelo código a seguir:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Tarefa 2-executando o aplicativo

Nesta tarefa, você testará que a página de exibição **excluir** do **storemanager** exibe um formulário de exclusão de confirmação.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home Page. Altere a URL para **/StoreManager**. Selecione um álbum para excluir clicando em **excluir** e verifique se o novo modo de exibição foi carregado.

    ![Excluindo um álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Excluindo um álbum")

    *Excluindo um álbum*

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a>Tarefa 3-implementando o método de ação HTTP-POST Delete

Nesta tarefa, você implementará a versão HTTP-POST do método Excluir ação que será invocado quando um usuário clicar no botão **excluir** . O método deve excluir o álbum no banco de dados.

1. Feche o navegador, se necessário, para retornar à janela do Visual Studio. Abra a classe **StoreManagerController** . Para fazer isso, expanda a pasta **controladores** e clique duas vezes em **StoreManagerController.cs**.
2. Substitua o código do método de ação **http-post Delete** pelo seguinte:

    (Trecho de código- *ASP.net e auxiliares MVC 4 e formulários e validação-Ex5 tratamento exclusão http-post excluir ação*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Tarefa 4-executando o aplicativo

Nesta tarefa, você testará que a página de exibição **excluir do storemanager** permite excluir um álbum e, em seguida, redireciona para a exibição de índice storemanager.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home Page. Altere a URL para **/StoreManager**. Selecione um álbum para excluir clicando em **excluir.** Confirme a exclusão clicando no botão **excluir** :

    ![Excluindo um álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Excluindo um álbum")

    *Excluindo um álbum*
3. Verifique se o álbum foi excluído, pois ele não aparece na página de **índice** .

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a>Exercício 6: adicionando validação

Atualmente, os formulários de criação e edição que você tem em vigor não executam nenhum tipo de validação. Se o usuário deixar um campo obrigatório em branco ou digitar letras no campo preço, o primeiro erro será obtido do banco de dados.

Você pode adicionar validação ao aplicativo Adicionando anotações de dados à sua classe de modelo. As anotações de dados permitem descrever as regras que você deseja aplicar às propriedades do modelo e o ASP.NET MVC cuidará da imposição e exibição da mensagem apropriada aos usuários.

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a>Tarefa 1-Adicionando anotações de dados

Nesta tarefa, você adicionará anotações de dados ao modelo de álbum que fará com que a página criar e editar exiba mensagens de validação quando apropriado.

Para uma classe de modelo simples, a adição de uma anotação de dados é tratada apenas com a adição de uma instrução **using** para **System. ComponentModel. DataAnnotation**e, em seguida, a colocação de um atributo **[Required]** nas propriedades apropriadas. O exemplo a seguir tornaria a propriedade **Name** um campo obrigatório na exibição.

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

Isso é um pouco mais complexo em casos como esse aplicativo onde o Modelo de Dados de Entidade é gerado. Se você adicionou anotações de dados diretamente às classes de modelo, elas serão substituídas se você atualizar o modelo do banco de dados. Em vez disso, você pode fazer uso de classes parciais de metadados que existirão para manter as anotações e estão associadas às classes de modelo usando o atributo **[MetadataType]** .

1. Abra a solução **inicial** localizada na **origem/Ex6-AddingValidation/início/** pasta. Caso contrário, você pode continuar usando a solução **final** obtida concluindo o exercício anterior.

   1. Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar. Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.
   2. Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, Compile a solução clicando em **build** | **Compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto. É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.
2. Abra o **Album.cs** na pasta **modelos** .
3. Substitua o conteúdo do **Album.cs** pelo código realçado, para que ele seja semelhante ao seguinte:

    > [!NOTE]
    > A linha **[DisplayFormat (ConvertEmptyStringToNull = false)]** indica que as cadeias de caracteres vazias do modelo não serão convertidas em NULL quando o campo de dados for atualizado na fonte de dados. Essa configuração evitará uma exceção quando o Entity Framework atribuir valores nulos ao modelo antes de a anotação de dados validar os campos.

    (Trecho de código- *ASP.net e auxiliares MVC 4 e formulários e validação – classe parcial de metadados de álbum Ex6*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > Essa classe parcial do **álbum** tem um atributo **MetadataType** que aponta para a classe **AlbumMetaData** para as anotações de dados. Estes são alguns dos atributos de anotação de dados que você está usando para anotar o modelo de álbum:
    > 
    > - Obrigatório-indica que a propriedade é um campo obrigatório
    > - DisplayName-define o texto a ser usado em campos de formulário e mensagens de validação
    > - DisplayFormat-especifica como os campos de dados são exibidos e formatados.
    > - StringLength-define um comprimento máximo para um campo de cadeia de caracteres
    > - Range-fornece um valor máximo e mínimo para um campo numérico
    > - ScaffoldColumn-permite ocultar campos de formulários do editor

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Tarefa 2-executando o aplicativo

Nesta tarefa, você testará que a página criar e editar páginas validam os campos, usando os nomes de exibição escolhidos na última tarefa.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home Page. Altere a URL para **/StoreManager/Create**. Verifique se os nomes de exibição correspondem àqueles na classe Partial (como a **URL de arte do álbum** em vez de **AlbumArtUrl**)
3. Clique em **criar**, sem preencher o formulário. Verifique se você obtém as mensagens de validação correspondentes.

    ![Campos validados na página criar](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Campos validados na página criar")

    *Campos validados na página criar*
4. Você pode verificar se o mesmo ocorre com a página **Editar** . Altere a URL para **/StoreManager/Edit/1** e verifique se os nomes de exibição correspondem àqueles na classe Partial (como a **URL de arte do álbum** em vez de **AlbumArtUrl**). Esvazie os campos **título** e **Preço** e clique em **salvar**. Verifique se você obtém as mensagens de validação correspondentes.

    ![Campos validados na página Editar](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    *Campos validados na página Editar*

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a>Exercício 7: usando o jQuery discreto no lado do cliente

Neste exercício, você aprenderá a habilitar a validação de jQuery do MVC 4 discreta no lado do cliente.

> [!NOTE]
> O jQuery não intrusivo usa JavaScript de prefixo de AJAX de dados para invocar métodos de ação no servidor em vez de emitir scripts de cliente embutidos de forma invasiva.

<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a>Tarefa 1-executando o aplicativo antes de habilitar o jQuery discreto

Nesta tarefa, você executará o aplicativo antes de incluir o jQuery para comparar os dois modelos de validação.

1. Abra a solução **inicial** localizada na **origem/Ex7-UnobtrusivejQueryValidation/início/** pasta. Caso contrário, você pode continuar usando a solução **final** obtida concluindo o exercício anterior.

   1. Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar. Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.
   2. Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, Compile a solução clicando em **build** | **Compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto. É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.
2. Pressione **F5** para executar o aplicativo.
3. O projeto é iniciado na Home Page. Procure **/StoreManager/Create** e clique em **criar** sem preencher o formulário para verificar se você obtém as mensagens de validação:

    ![Validação do cliente desabilitada](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "Validação do cliente desabilitada")

    *Validação do cliente desabilitada*
4. No navegador, abra o código-fonte HTML:

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a>Tarefa 2-habilitando a validação de cliente discreta

Nesta tarefa, você habilitará a **validação de cliente** do jQuery discreta do arquivo **Web. config** , que é definido por padrão como false em todos os novos projetos do ASP.NET MVC 4. Além disso, você adicionará as referências de scripts necessárias para tornar o jQuery não invasivo o trabalho de validação do cliente.

1. Abra o arquivo **Web. config** na raiz do projeto e verifique se os valores das chaves **ClientValidationEnabled** e **UnobtrusiveJavaScriptEnabled** estão definidos como **true**.

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > Você também pode habilitar a validação do cliente pelo código em Global.asax.cs para obter os mesmos resultados:
    > 
    > **HtmlHelper. ClientValidationEnabled = true;**
    > 
    > Além disso, você pode atribuir o atributo ClientValidationEnabled a qualquer controlador para ter um comportamento personalizado.
2. Abra **Create. cshtml** em **Views\StoreManager**.
3. Verifique se os arquivos de script a seguir, **jQuery. Validate** e **jQuery. Validate. nondiscretos**, são referenciados na exibição por meio do pacote de&quot; &quot; **~/Bundles/jqueryval** .

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > Todas essas bibliotecas jQuery estão incluídas nos novos projetos do MVC 4. Você pode encontrar mais bibliotecas na pasta **/scripts** do projeto.
    > 
    > Para fazer com que essas bibliotecas de validação funcionem, você precisa adicionar uma referência à biblioteca do jQuery Framework. Como essa referência já foi adicionada no arquivo **layout. cshtml de\_** , você não precisa adicioná-la neste modo de exibição específico.

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a>Tarefa 3 – executando o aplicativo usando a validação não invasiva do jQuery

Nesta tarefa, você testará que o modelo de exibição de criação do **storemanager** executa a validação do lado do cliente usando bibliotecas jQuery quando o usuário cria um novo álbum.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home Page. Procure **/StoreManager/Create** e clique em **criar** sem preencher o formulário para verificar se você obtém as mensagens de validação:

    ![Validação de cliente com jQuery habilitada](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Validação de cliente com jQuery habilitada")

    *Validação de cliente com jQuery habilitada*
3. No navegador, abra o código-fonte para criar modo de exibição:

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

   > [!NOTE]
   > Para cada regra de validação de cliente, o jQuery discreto adiciona um atributo com data-Val-*rulename*=&quot;*mensagem*&quot;. Abaixo está uma lista de marcas que o jQuery discreto insere no campo de entrada HTML para executar a validação do cliente:
   > 
   > - Valor de dados
   > - Data-val-número
   > - Data-Val-intervalo
   > - Data-Val-Range-min/Data-Val-Range-Max
   > - Data-Val-obrigatório
   > - Data-Val-Length
   > - Data-Val-Length-Max/data-Val-Length-min
   > 
   > Todos os valores de dados são preenchidos com a **anotação de dados**de modelo. Em seguida, toda a lógica que funciona no lado do servidor pode ser executada no lado do cliente. Por exemplo, o atributo de preço tem a seguinte anotação de dados no modelo:
   > 
   > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
   > 
   > Depois de usar o jQuery discreto, o código gerado é:
   > 
   > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Resumo

Ao concluir este laboratório prático, você aprendeu a permitir que os usuários alterem os dados armazenados no banco de dados com o uso do seguinte:

- Ações do controlador, como índice, criar, editar, excluir
- Recurso scaffolding do ASP.NET MVC para exibir propriedades em uma tabela HTML
- Auxiliares HTML personalizados para melhorar a experiência do usuário
- Métodos de ação que reagem às chamadas HTTP-GET ou HTTP-POST
- Um modelo de editor compartilhado para modelos de exibição semelhantes, como criar e editar
- Elementos de formulário como menus suspensos
- Anotações de dados para validação de modelo
- Validação do lado do cliente usando a biblioteca discreta do jQuery

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apêndice A: Instalando o Visual Studio Express 2012 para Web

Você pode instalar o **Microsoft Visual Studio Express 2012 para Web** ou outra versão &quot;Express&quot; usando o **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** . As instruções a seguir orientam você pelas etapas necessárias para instalar o *Visual Studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.

1. Vá para [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Como alternativa, se você já tiver instalado o Web Platform Installer, poderá abri-lo e pesquisar o produto &quot;<em>Visual Studio Express 2012 para Web com o SDK do Windows Azure</em>&quot;.
2. Clique em **instalar agora**. Se você não tiver **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.
3. Quando **Web Platform Installer** estiver aberto, clique em **instalar** para iniciar a instalação.

    ![Instalar Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Instalar Visual Studio Express")

    *Instalar Visual Studio Express*
4. Leia todos os termos e licenças de produtos e clique em **aceito** para continuar.

    ![Aceitando os termos de licença](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    *Aceitando os termos de licença*
5. Aguarde até que o processo de download e instalação seja concluído.

    ![Progresso da instalação](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    *Progresso da instalação*
6. Quando a instalação for concluída, clique em **concluir**.

    ![Instalação concluída](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    *Instalação concluída*
7. Clique em **sair** para fechar Web Platform Installer.
8. Para abrir o Visual Studio Express para Web, vá para a tela **Iniciar** e comece a escrever &quot;**vs Express**&quot;e, em seguida, clique no bloco **vs Express para Web** .

    ![Bloco VS Express para Web](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    *Bloco VS Express para Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Apêndice B: usando trechos de código

Com trechos de código, você tem todo o código de que precisa ao seu alcance. O documento de laboratório informará exatamente quando você pode usá-los, conforme mostrado na figura a seguir.

![Usando trechos de código do Visual Studio para inserir código em seu projeto](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "Usando trechos de código do Visual Studio para inserir código em seu projeto")

*Usando trechos de código do Visual Studio para inserir código em seu projeto*

***Para adicionar um trecho de código usando o tecladoC# (somente)***

1. Coloque o cursor onde você deseja inserir o código.
2. Comece digitando o nome do trecho de código (sem espaços ou hifens).
3. Observe como o IntelliSense exibe nomes de trechos de código correspondentes.
4. Selecione o trecho correto (ou continue digitando até que o nome do trecho inteiro seja selecionado).
5. Pressione a tecla TAB duas vezes para inserir o trecho de código no local do cursor.

![Comece a digitar o nome do trecho](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Comece a digitar o nome do trecho")

*Comece a digitar o nome do trecho*

![Pressione Tab para selecionar o trecho realçado](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "Pressione Tab para selecionar o trecho realçado")

*Pressione Tab para selecionar o trecho realçado*

![Pressione Tab novamente e o trecho será expandido](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "Pressione Tab novamente e o trecho será expandido")

*Pressione Tab novamente e o trecho será expandido*

***Para adicionar um trecho de código usando o mouseC#(, Visual Basic e XML)*** uma. Clique com o botão direito do mouse no local em que você deseja inserir o trecho de código.

1. Selecione **Inserir trecho** seguido por **meus trechos de código**.
2. Selecione o trecho relevante na lista clicando nele.

![Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho")

*Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho*

![Selecione o trecho relevante na lista clicando nele](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "Selecione o trecho relevante na lista clicando nele")

*Selecione o trecho relevante na lista clicando nele*
