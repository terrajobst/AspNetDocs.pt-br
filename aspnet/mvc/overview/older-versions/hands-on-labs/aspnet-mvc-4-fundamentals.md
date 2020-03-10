---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: Conceitos básicos do ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: Este laboratório prático baseia-se na loja de música MVC (Model View Controller), um aplicativo de tutorial que apresenta e explica passo a passo como usar o ASP.NET MV...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: 95e9b9f55b2080c0ed01dc34e3a32f9f1c905644
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598815"
---
# <a name="aspnet-mvc-4-fundamentals"></a>Conceitos básicos do ASP.NET MVC 4

por [equipe de acampamentos da Web](https://twitter.com/webcamps)

[Baixe o kit de treinamento do Web acampamentos](https://aka.ms/webcamps-training-kit)

Este laboratório prático baseia-se na loja de música MVC (Model View Controller), um aplicativo de tutorial que apresenta e explica passo a passo como usar o ASP.NET MVC e o Visual Studio. Em todo o laboratório, você aprenderá a simplicidade, mas também a poder de usar essas tecnologias juntas. Você começará com um aplicativo simples e o criará até ter um aplicativo Web ASP.NET MVC 4 totalmente funcional.

Este laboratório funciona com o ASP.NET MVC 4.

Se você quiser explorar a versão do ASP.NET MVC 3 do aplicativo tutorial, poderá encontrá-la no [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).

Este laboratório prático pressupõe que o desenvolvedor tenha experiência em tecnologias de desenvolvimento para a Web, como HTML e JavaScript.

> [!NOTE]
> Todos os códigos de exemplo e trechos de código estão incluídos no Web acampamentos Training Kit, disponível nas [versões Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). O projeto específico deste laboratório está disponível nos [conceitos básicos do ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a>O aplicativo da loja de música

O aplicativo Web da loja de música que será criado em todo este laboratório é composto por três partes principais: compras, check-out e administração. Os visitantes poderão procurar álbuns por gênero, adicionar álbuns ao seu carrinho, revisar sua seleção e, por fim, continuar a fazer o check-out e concluir o pedido. Além disso, os administradores da loja poderão gerenciar os álbuns disponíveis, bem como suas propriedades principais.

![Telas da loja de música](aspnet-mvc-4-fundamentals/_static/image1.png "Telas da loja de música")

*Telas da loja de música*

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a>ASP.NET MVC 4 Essentials

O aplicativo da loja de música será criado usando o **MVC (Model View Controller)** , um padrão de arquitetura que separa um aplicativo em três componentes principais:

- **Modelos**: objetos de modelo são as partes do aplicativo que implementam a lógica de domínio. Geralmente, os objetos de modelo também recuperam e armazenam o estado do modelo em um banco de dados.
- **Modos de exibição:** As exibições são os componentes que exibem a interface do usuário do aplicativo. Normalmente, esta IU é criada a partir dos dados do modelo. Um exemplo seria o modo de exibição de edição de álbuns que exibe caixas de texto e uma lista suspensa com base no estado atual de um objeto de álbum.
- **Controladores:** Os controladores são os componentes que manipulam a interação do usuário, manipulam o modelo e, por fim, selecionam uma exibição para renderizar a interface de usuário. Em um aplicativo MVC, a exibição só mostra informações; o controlador manipula e responde à entrada e à interação do usuário.

O padrão MVC ajuda a criar aplicativos que separam os diferentes aspectos do aplicativo (lógica de entrada, lógica de negócios e lógica de interface do usuário), fornecendo, ao mesmo tempo, um acoplamento flexível entre esses elementos. Essa separação ajuda a gerenciar a complexidade quando você cria um aplicativo, pois permite que você se concentre em um aspecto da implementação de cada vez. Além disso, o padrão MVC facilita o teste de aplicativos, também incentivando o uso do TDD (desenvolvimento controlado por testes) para a criação de aplicativos.

O **ASP.NET MVC** Framework fornece uma alternativa ao padrão de Web Forms ASP.net para criar aplicativos Web baseados em ASP.NET MVC. O **ASP.NET MVC** Framework é uma estrutura de apresentação leve e altamente testada que (assim como os aplicativos baseados em formulários da Web) é integrada aos recursos existentes do ASP.net, como páginas mestras e autenticação baseada em associação para que você obtenha todo o poder do .NET Framework principal. Isso será útil se você já estiver familiarizado com ASP.NET Web Forms porque todas as bibliotecas que você já usa também estão disponíveis no ASP.NET MVC 4.

Além disso, o acoplamento flexível entre os três componentes principais de um aplicativo MVC também promove o desenvolvimento paralelo. Por exemplo, um desenvolvedor pode trabalhar na exibição, um segundo desenvolvedor pode trabalhar na lógica do controlador e um terceiro desenvolvedor pode se concentrar na lógica de negócios no modelo.

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

Neste laboratório prático, você aprenderá a:

- Criar um aplicativo MVC ASP.NET do zero com base no tutorial do aplicativo de loja de música
- Adicionar controladores para lidar com URLs na home page do site e para procurar sua funcionalidade principal
- Adicionar uma exibição para personalizar o conteúdo exibido junto com seu estilo
- Adicionar classes de modelo para conter e gerenciar dados e lógica de domínio
- Use o padrão de modelo de exibição para passar informações de ações do controlador para os modelos de exibição
- Explore o novo modelo do ASP.NET MVC 4 para aplicativos de Internet

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisites

Você deve ter os seguintes itens para concluir este laboratório:

- [Visual Studio 2012 Express para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (leia [o apêndice a](#AppendixA) para obter instruções sobre como instalá-lo)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Instalação

**Instalando trechos de código**

Para sua conveniência, grande parte do código que você gerenciará ao longo deste laboratório está disponível como trechos de código do Visual Studio. Para instalar os trechos de código, execute o arquivo **.\Source\Setup\CodeSnippets.VSI** .

Se você não estiver familiarizado com os trechos de Visual Studio Code e quiser saber como usá-los, consulte o apêndice deste documento &quot;[Apêndice C: usando trechos de código](#AppendixC)&quot;.

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Exercícios

Este laboratório prático é composto pelos seguintes exercícios:

1. [Exercício 1: criando o projeto de aplicativo Web MusicStore ASP.NET MVC](#Exercise1)
2. [Exercício 2: Criando um controlador](#Exercise2)
3. [Exercício 3: passando parâmetros para um controlador](#Exercise3)
4. [Exercício 4: criando uma exibição](#Exercise4)
5. [Exercício 5: Criando um modelo de exibição](#Exercise5)
6. [Exercício 6: usando parâmetros na exibição](#Exercise6)
7. [Exercício 7: um colo sobre o novo modelo do MVC 4 do ASP.NET](#Exercise7)

> [!NOTE]
> Cada exercício é acompanhado por uma pasta **final** que contém a solução resultante que você deve obter depois de concluir os exercícios. Você pode usar essa solução como um guia se precisar de ajuda adicional para trabalhar com os exercícios.

Tempo estimado para concluir este laboratório: **60 minutos**.

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a>Exercício 1: criando o projeto de aplicativo Web MusicStore ASP.NET MVC

Neste exercício, você aprenderá a criar um aplicativo MVC ASP.NET no Visual Studio 2012 Express para Web, bem como em sua organização de pastas principais. Além disso, você aprenderá como adicionar um novo controlador e fazer com que ele exiba uma cadeia de caracteres simples no home page do aplicativo.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a>Tarefa 1-criando o projeto de aplicativo Web do ASP.NET MVC

1. Nesta tarefa, você criará um projeto de aplicativo ASP.NET MVC vazio usando o modelo do Visual Studio MVC. Iniciar **vs Express para Web**.
2. No menu **Arquivo**, clique em **Novo Projeto**.
3. Na caixa de diálogo **novo projeto** , selecione o tipo de projeto de **aplicativo Web ASP.NET MVC 4** , localizado em **Visual C#, lista de** modelos **da Web** .
4. Altere o **nome** para *MvcMusicStore*.
5. Defina o local da solução dentro de uma nova pasta **inicial** na pasta de origem deste exercício, por exemplo **[Your-Chol-Path] \Source\Ex01-CreatingMusicStoreProject\Begin**. Clique em **OK**.

    ![Caixa de diálogo Criar novo projeto](aspnet-mvc-4-fundamentals/_static/image2.png "Caixa de diálogo Criar novo projeto")

    *Caixa de diálogo Criar novo projeto*
6. Na caixa de diálogo **novo projeto do ASP.NET MVC 4** , selecione o modelo **básico** e verifique se o **mecanismo de exibição** selecionado é **Razor**. Clique em **OK**.

    ![Caixa de diálogo novo projeto do ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image3.png "Caixa de diálogo novo projeto do ASP.NET MVC 4")

    *Caixa de diálogo novo projeto do ASP.NET MVC 4*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a>Tarefa 2-explorando a estrutura da solução

O ASP.NET MVC Framework inclui um modelo de projeto do Visual Studio que ajuda a criar aplicativos Web que dão suporte ao padrão MVC. Este modelo cria um novo aplicativo Web ASP.NET MVC com as pastas necessárias, os modelos de item e as entradas de arquivo de configuração.

Nesta tarefa, você examinará a estrutura da solução para entender os elementos envolvidos e suas relações. As seguintes pastas estão incluídas em todos os aplicativos ASP.NET MVC, pois a estrutura MVC do ASP.NET, por padrão, usa uma Convenção de &quot;sobre a abordagem de&quot; de configuração e faz algumas suposições padrão com base nas convenções de nomenclatura de pastas.

1. Depois que o projeto for criado, examine a estrutura de pastas que foi criada na Gerenciador de Soluções no lado direito:

    ![Estrutura de pastas MVC do ASP.NET no Gerenciador de Soluções](aspnet-mvc-4-fundamentals/_static/image4.png "Estrutura de pastas MVC do ASP.NET no Gerenciador de Soluções")

    *Estrutura de pastas MVC do ASP.NET no Gerenciador de Soluções*

   1. **Controladores**. Essa pasta conterá as classes do controlador. Em um aplicativo baseado em MVC, os controladores são responsáveis por lidar com a interação do usuário final, manipulando o modelo e, por fim, escolhendo uma exibição para renderizar a interface do usuário.

       > [!NOTE]
       > A MVC Framework exige que os nomes de todos os controladores terminem com &quot;controlador&quot;-por exemplo, HomeController, LoginController ou ProductController.
   2. **Modelos**. Essa pasta é fornecida para classes que representam o modelo de aplicativo para o aplicativo Web MVC. Isso geralmente inclui um código que define objetos e a lógica para interagir com o armazenamento de dados. Normalmente, os objetos de modelo reais estarão em bibliotecas de classes separadas. No entanto, ao criar um novo aplicativo, você pode incluir classes e, em seguida, movê-las para bibliotecas de classes separadas em um ponto posterior no ciclo de desenvolvimento.
   3. **Exibições**. Essa pasta é o local recomendado para exibições, os componentes responsáveis por exibir a interface do usuário do aplicativo. Os modos de exibição usam arquivos. aspx,. ascx,. cshtml e. Master, além de quaisquer outros arquivos relacionados às exibições de renderização. A pasta views contém uma pasta para cada controlador; a pasta é nomeada com o prefixo do nome do controlador. Por exemplo, se você tiver um controlador chamado **HomeController**, a pasta views conterá uma pasta chamada Home. Por padrão, quando o ASP.NET MVC Framework carrega uma exibição, ele procura um arquivo. aspx com o nome de exibição solicitado na pasta Views\controllerName (**views [ControllerName] [Action]. aspx**) ou (**views [ControllerName] [Action]. cshtml**) para exibições do Razor.

      > [!NOTE]
      > Além das pastas listadas anteriormente, um aplicativo Web MVC usa o arquivo **global. asax** para definir padrões de roteamento de URL global e usa o arquivo **Web. config** para configurar o aplicativo.

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a>Tarefa 3-adicionando um HomeController

Em aplicativos ASP.NET que não usam a MVC Framework, a interação do usuário é organizada em volta das páginas e, ao gerar e manipular eventos a partir dessas páginas. Por outro lado, a interação do usuário com aplicativos ASP.NET MVC é organizada em relação a controladores e seus métodos de ação.

Por outro lado, o ASP.NET MVC Framework mapeia URLs para classes que são conhecidas como controladores. Os controladores processam solicitações de entrada, lidam com a entrada e as interações do usuário, executam a lógica do aplicativo apropriada e determinam a resposta a ser enviada de volta ao cliente (exibir HTML, baixar um arquivo, redirecionar para uma URL diferente, etc.). No caso da exibição de HTML, uma classe de controlador normalmente chama um componente de exibição separado para gerar a marcação HTML para a solicitação. Em um aplicativo MVC, a exibição só mostra informações; o controlador manipula e responde à entrada e à interação do usuário.

Nesta tarefa, você adicionará uma classe de controlador que tratará URLs para a home page do site da loja de música.

1. Clique com o botão direito do mouse na pasta **controladores** dentro da Gerenciador de soluções, selecione **Adicionar** e, em seguida, comando do **controlador** :

    ![Adicionar um comando de controlador](aspnet-mvc-4-fundamentals/_static/image5.png "Adicionar um comando de controlador")

    *Comando adicionar controlador*
2. A caixa de diálogo **Adicionar controlador** é exibida. Nomeie o controlador *HomeController* e pressione **Adicionar**.

    ![Caixa de diálogo Adicionar controlador](aspnet-mvc-4-fundamentals/_static/image6.png "Caixa de diálogo Adicionar controlador")

    *Caixa de diálogo Adicionar controlador*
3. O arquivo **HomeController.cs** é criado na pasta **controladores** . Para que o **HomeController** retorne uma cadeia de caracteres em sua ação de índice, substitua o método de **índice** pelo código a seguir:

    (Trecho de código- *ASP.NET MVC 4 Fundamentals-EX1 HomeController index*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Tarefa 4-executando o aplicativo

Nesta tarefa, você experimentará o aplicativo em um navegador da Web.

1. Pressione **F5** para executar o aplicativo. O projeto é compilado e o servidor Web do IIS local é iniciado. O servidor Web local do IIS abrirá automaticamente um navegador da Web apontando para a URL do servidor Web.

    ![Aplicativo em execução em um navegador da Web](aspnet-mvc-4-fundamentals/_static/image7.png "Aplicativo em execução em um navegador da Web")

    *Aplicativo em execução em um navegador da Web*

    > [!NOTE]
    > O servidor Web local do IIS executará o site em um número de porta livre aleatório. Na figura acima, o site está em execução em `http://localhost:50103/`, portanto, ele está usando a porta 50103. O número da porta pode variar.
2. Feche o navegador.

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a>Exercício 2: Criando um controlador

Neste exercício, você aprenderá a atualizar o controlador para implementar a funcionalidade simples do aplicativo de loja de música. Esse controlador definirá métodos de ação para lidar com cada uma das seguintes solicitações específicas:

- Uma página de listagem dos gêneros musicais na loja de música
- Uma página de navegação que lista todos os álbuns de música para um gênero específico
- Uma página de detalhes que mostra informações sobre um álbum de música específico

Para o escopo deste exercício, essas ações simplesmente retornarão uma cadeia de caracteres agora.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a>Tarefa 1-adicionando uma nova classe StoreController

Nesta tarefa, você adicionará um novo controlador.

1. Se ainda não estiver aberto, inicie o **VS Express para Web 2012**.
2. No menu **arquivo** , escolha **Abrir projeto**. Na caixa de diálogo Abrir projeto, navegue até **Source\Ex02-CreatingAController\Begin**, selecione **Iniciar. sln** e clique em **abrir**. Como alternativa, você pode continuar com a solução obtida depois de concluir o exercício anterior.

   1. Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar. Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.
   2. Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, Compile a solução clicando em **build** | **Compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto. É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.
3. Adicione o novo controlador. Para fazer isso, clique com o botão direito do mouse na pasta **controladores** dentro da Gerenciador de soluções, selecione **Adicionar** e, em seguida, o comando **controlador** . Altere o **nome do controlador** para *StoreController*e clique em **Adicionar**.

    ![Caixa de diálogo Adicionar controlador](aspnet-mvc-4-fundamentals/_static/image8.png "Caixa de diálogo Adicionar controlador")

    *Caixa de diálogo Adicionar controlador*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a>Tarefa 2-modificando as ações do StoreController

Nesta tarefa, você modificará os métodos do controlador que são chamados de **ações**. As ações são responsáveis pelo tratamento de solicitações de URL e pela determinação do conteúdo que deve ser enviado de volta ao navegador ou ao usuário que invocou a URL.

1. A classe **StoreController** já tem um método **index** . Você o usará posteriormente neste laboratório para implementar a página que lista todos os gêneros da loja de música. Por enquanto, basta substituir o método de **índice** pelo código a seguir que retorna uma cadeia de caracteres &quot;Olá de Store. Index ()&quot;:

    (Trecho de código- *ASP.NET MVC 4 Fundamentals-EX2 StoreController index*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. Adicione os métodos **Browse** e **Details** . Para fazer isso, adicione o seguinte código ao **StoreController**:

    (Trecho de código- *ASP.NET MVC 4 Fundamentals-EX2 StoreController BrowseAndDetails*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tarefa 3-executando o aplicativo

Nesta tarefa, você experimentará o aplicativo em um navegador da Web.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na **Home** Page. Altere a URL para verificar a implementação de cada ação.

    1. **/Store**. Você verá **&quot;Olá do Store. Index ()&quot;** .
    2. **/Store/Browse**. Você verá **&quot;Olá de Store. Browse ()&quot;** .
    3. **/Store/Details**. Você verá **&quot;Olá da Store. Details ()&quot;** .

        ![Navegando em StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "Navegando em StoreBrowse")

        *Navegando em/Store/Browse*
3. Feche o navegador.

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a>Exercício 3: passando parâmetros para um controlador

Até agora, você está retornando cadeias de caracteres constantes dos controladores. Neste exercício, você aprenderá a passar parâmetros para um controlador usando a URL e a QueryString e, em seguida, fazer com que as ações do método respondam com o texto para o navegador.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a>Tarefa 1-adicionando o parâmetro de gênero a StoreController

Nesta tarefa, você usará a **QueryString** para enviar parâmetros para o método de ação **procurar** no **StoreController**.

1. Se ainda não estiver aberto, inicie o **vs Express para Web**.
2. No menu **arquivo** , escolha **Abrir projeto**. Na caixa de diálogo Abrir projeto, navegue até **Source\Ex03-PassingParametersToAController\Begin**, selecione **Iniciar. sln** e clique em **abrir**. Como alternativa, você pode continuar com a solução obtida depois de concluir o exercício anterior.

   1. Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar. Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.
   2. Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, Compile a solução clicando em **build** | **Compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto. É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.
3. Abra a classe **StoreController** . Para fazer isso, na **Gerenciador de soluções**, expanda a pasta **controladores** e clique duas vezes em **StoreController.cs**.
4. Altere o método **procurar** , adicionando um parâmetro de cadeia de caracteres para solicitar um gênero específico. O ASP.NET MVC passará automaticamente qualquer parâmetro QueryString ou formulário post chamado **gênero** para esse método de ação quando invocado. Para fazer isso, substitua o método **Browse** pelo seguinte código:

    (Trecho de código- *ASP.NET MVC 4 Fundamentals-EX3 StoreController BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

> [!NOTE]
> Você está usando o método do utilitário **HttpUtility. HtmlEncode** para impedir que os usuários insiram JavaScript na exibição com um link como **/Store/Browse? Gênero =&lt;script&gt;janela. Location = '[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;** .
> 
> Para obter mais explicações, visite [Este artigo do MSDN](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Tarefa 2-executando o aplicativo

Nesta tarefa, você experimentará o aplicativo em um navegador da Web e usará o parâmetro **gênero** .

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na **Home** Page. Alterar a URL para */Store/Browse? Gênero = disco* para verificar se a ação recebe o parâmetro gênero.

    ![Navegando em StoreBrowseGenre = disco](aspnet-mvc-4-fundamentals/_static/image10.png "Navegando em StoreBrowseGenre = disco")

    *Procurando/Store/Browse? Gênero = disco*
3. Feche o navegador.

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a>Tarefa 3-adicionando um parâmetro de ID inserido na URL

Nesta tarefa, você usará a **URL** para passar um parâmetro de **ID** para o método de ação de **detalhes** do **StoreController**. A Convenção de roteamento padrão do ASP.NET MVC é tratar o segmento de uma URL após o nome do método de ação como um parâmetro chamado **ID**. Se o método de ação tiver um parâmetro chamado ID, o ASP.NET MVC passará automaticamente o segmento de URL para você como um parâmetro. Na URL **Store/Details/5**, a **ID** será interpretada como **5**.

1. Altere o método **Details** do **StoreController**, adicionando um parâmetro **int** chamado **ID**. Para fazer isso, substitua o método **Details** pelo seguinte código:

    (Trecho de código- *ASP.NET MVC 4 Fundamentals-EX3 StoreController DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Tarefa 4-executando o aplicativo

Nesta tarefa, você experimentará o aplicativo em um navegador da Web e usará o parâmetro **ID** .

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na **Home** Page. Altere a URL para */Store/Details/5* para verificar se a ação recebe o parâmetro ID.

    ![Navegando em StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "Navegando em StoreDetails5")

    *Navegando em/Store/Details/5*

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a>Exercício 4: criando uma exibição

Até agora, você está retornando cadeias de caracteres de ações do controlador. Embora essa seja uma maneira útil de entender como os controladores funcionam, não é assim que seus aplicativos Web reais são criados. Modos de exibição são componentes que fornecem uma abordagem melhor para gerar o HTML de volta para o navegador com o uso de arquivos de modelo.

Neste exercício, você aprenderá a adicionar uma página mestra de layout para configurar um modelo para conteúdo HTML comum, uma folha de estilos para aprimorar a aparência do site e, finalmente, um modelo de exibição para permitir que o HomeController retorne HTML.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-_layoutcshtml"></a>Tarefa 1-modificando o arquivo \_layout. cshtml

O arquivo **~/Views/Shared/\_layout. cshtml** permite configurar um modelo para que o HTML comum seja usado em todo o site. Nesta tarefa, você adicionará uma página mestra de layout com um cabeçalho comum com links para a página inicial e área de armazenamento.

1. Se ainda não estiver aberto, inicie o **vs Express para Web**.
2. No menu **arquivo** , escolha **Abrir projeto**. Na caixa de diálogo Abrir projeto, navegue até **Source\Ex04-CreatingAView\Begin**, selecione **Iniciar. sln** e clique em **abrir**. Como alternativa, você pode continuar com a solução obtida depois de concluir o exercício anterior.

   1. Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar. Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.
   2. Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, Compile a solução clicando em **build** | **Compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto. É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.
3. O arquivo <strong>\_layout. cshtml</strong> contém o layout do contêiner HTML para todas as páginas no site. Ele inclui o elemento <strong>&lt;html&gt;</strong> para a resposta HTML, bem como os elementos <strong>&gt;Head&lt;</strong> e&lt;<strong>Body</strong>&gt;. <strong>@RenderBody()</strong> no corpo de HTML identificam as regiões que exibem modelos que poderão preencher com conteúdo dinâmico.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. Adicione um cabeçalho comum com links para a Home Page e área de armazenamento em todas as páginas do site. Para fazer isso, adicione o seguinte código abaixo &lt;instrução&gt; do corpo.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. Inclua um div para renderizar a seção de corpo de cada página. Substitua <strong>@RenderBody()</strong> pelo seguinte código realçado:C#()

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > Você sabia? O Visual Studio 2012 tem trechos que facilitam a adição de código comumente usado em HTML, arquivos de código e muito mais! Experimente, digitando **&lt;div&gt;** e pressionando **Tab** duas vezes para inserir uma marca **div** completa.

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a>Tarefa 2-Adicionando folha de estilos CSS

O modelo de projeto vazio inclui um arquivo CSS muito simplificado que inclui apenas estilos usados para exibir formulários básicos e mensagens de validação. Você usará CSS e imagens adicionais (potencialmente fornecidos por um designer) para aprimorar a aparência do site.

Nesta tarefa, você adicionará uma folha de estilo CSS para definir os estilos do site.

1. O arquivo CSS e as imagens são incluídos na pasta **Source\Assets\Content** deste laboratório. Para adicioná-los ao aplicativo, arraste o conteúdo de uma janela do **Windows Explorer** para o **Gerenciador de soluções** no Visual Studio Express para Web, conforme mostrado abaixo:

    ![Arrastando conteúdo do estilo](aspnet-mvc-4-fundamentals/_static/image12.png "Arrastando conteúdo do estilo")

    *Arrastando conteúdo do estilo*
2. Uma caixa de diálogo de aviso será exibida, solicitando a confirmação para substituir o arquivo **site. css** e algumas imagens existentes. Marque **aplicar a todos os itens** e clique em **Sim**.

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a>Tarefa 3-adicionando um modelo de exibição

Nesta tarefa, você adicionará um modelo de exibição para gerar a resposta HTML que usará a página mestra de layout e a CSS adicionada neste exercício.

1. Para usar um modelo de exibição ao navegar na home page do site, primeiro será necessário indicar que, em vez de retornar uma cadeia de caracteres, o método de **índice HomeController** retornará uma **exibição**. Abra a classe **HomeController** e altere seu método de **índice** para retornar um **ActionResult**e faça com que ele retorne **View ()** .

    (Trecho de código- *ASP.NET MVC 4 Fundamentals-Ex4 HomeController index*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. Agora, você precisa adicionar um modelo de exibição apropriado. Para fazer isso, **clique com o botão direito do mouse** dentro do método de ação de **índice** e selecione **Adicionar exibição**. Isso abrirá a caixa de diálogo **Adicionar exibição** .

    ![Adicionando uma exibição de dentro do método de índice](aspnet-mvc-4-fundamentals/_static/image13.png "Adicionando uma exibição de dentro do método de índice")

    *Adicionando uma exibição de dentro do método de índice*
3. A caixa de diálogo **Adicionar exibição** será exibida para gerar um arquivo de modelo de exibição. Por padrão, essa caixa de diálogo popula previamente o nome do modelo de exibição para que ele corresponda ao método de ação que o usará. Como você usou o menu de contexto **Adicionar exibição** dentro do método de ação de **índice** dentro de HomeController, a caixa de diálogo **Adicionar exibição** tem índice como o nome de exibição padrão. Clique em **Adicionar**.

    ![Caixa de diálogo Adicionar exibição](aspnet-mvc-4-fundamentals/_static/image14.png "Caixa de diálogo Adicionar exibição")

    *Caixa de diálogo Adicionar exibição*
4. O Visual Studio gera um modelo de exibição **index. cshtml** dentro da pasta **views\home** e, em seguida, abre-o.

    ![Exibição do índice inicial criada](aspnet-mvc-4-fundamentals/_static/image15.png "Exibição do índice inicial criada")

    *Exibição do índice inicial criada*

    > [!NOTE]
    > o nome e o local do arquivo **index. cshtml** são relevantes e seguem as convenções de nomenclatura ASP.NET MVC padrão.
    > 
    > A pasta \Views\**Home** corresponde ao nome do controlador (controlador**doméstico** ). O nome do modelo de exibição (**índice**) corresponde ao método de ação do controlador que exibirá a exibição.
    > 
    > Dessa forma, o ASP.NET MVC evita que você precise especificar explicitamente o nome ou o local de um modelo de exibição ao usar essa Convenção de nomenclatura para retornar uma exibição.
5. O modelo de exibição gerado é baseado no modelo **\_layout. cshtml** definido anteriormente. Atualize a propriedade ViewBag. title para **página inicial**e altere o conteúdo principal para **esta é a Home Page**, conforme mostrado no código abaixo:

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. Selecione o projeto **MvcMusicStore** na Gerenciador de soluções e pressione **F5** para executar o aplicativo.

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a>Tarefa 4: verificação

Para verificar se você executou corretamente todas as etapas no exercício anterior, faça o seguinte:

Com o aplicativo aberto em um navegador, você deve observar que:

1. O método de ação de índice do HomeController encontrou e exibiu o modelo de exibição **\Views\Home\Index.cshtml** , embora o código tenha chamado de **modo de exibição de retorno ()** , porque o modelo de exibição seguiu a Convenção de nomenclatura padrão.
2. A Home Page exibe a mensagem de boas-vindas definida no modelo de exibição **\Views\Home\Index.cshtml** .
3. A Home Page está usando o modelo **\_layout. cshtml** e, portanto, a mensagem de boas-vindas está contida no layout HTML do site padrão.

    ![Exibição de índice inicial usando o LayoutPage e o estilo definidos](aspnet-mvc-4-fundamentals/_static/image16.png "Exibição de índice inicial usando o LayoutPage e o estilo definidos")

    *Exibição de índice inicial usando o LayoutPage e o estilo definidos*

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a>Exercício 5: Criando um modelo de exibição

Até agora, você fez suas exibições exibir HTML codificado, mas, para criar aplicativos Web dinâmicos, o modelo de exibição deve receber informações do controlador. Uma técnica comum a ser usada para essa finalidade é o padrão **ViewModel** , que permite que um controlador empacote todas as informações necessárias para gerar a resposta HTML apropriada.

Neste exercício, você aprenderá a criar uma classe ViewModel e a adicionar as propriedades necessárias: o número de gêneros no repositório e uma lista desses gêneros. Você também atualizará o StoreController para usar o ViewModel criado e, por fim, criará um novo modelo de exibição que exibirá as propriedades mencionadas na página.

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a>Tarefa 1-criando uma classe ViewModel

Nesta tarefa, você criará uma classe ViewModel que implementará o cenário de listagem de gênero de repositório.

1. Se ainda não estiver aberto, inicie o **vs Express para Web**.
2. No menu **arquivo** , escolha **Abrir projeto**. Na caixa de diálogo Abrir projeto, navegue até **Source\Ex05-CreatingAViewModel\Begin**, selecione **Iniciar. sln** e clique em **abrir**. Como alternativa, você pode continuar com a solução obtida depois de concluir o exercício anterior.

   1. Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar. Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.
   2. Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, Compile a solução clicando em **build** | **Compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto. É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.
3. Crie uma pasta **ViewModels** para manter o ViewModel. Para fazer isso, clique com o botão direito do mouse no projeto **MvcMusicStore** de nível superior, selecione **Adicionar** e **nova pasta**.

    ![Adicionando uma nova pasta](aspnet-mvc-4-fundamentals/_static/image17.png "Adicionando uma nova pasta")

    *Adicionando uma nova pasta*
4. Nomeie a pasta *ViewModels*.

    ![Pasta ViewModels no Gerenciador de Soluções](aspnet-mvc-4-fundamentals/_static/image18.png "Pasta ViewModels no Gerenciador de Soluções")

    *Pasta ViewModels no Gerenciador de Soluções*
5. Crie uma classe **ViewModel** . Para fazer isso, clique com o botão direito do mouse na pasta **ViewModels** criada recentemente, selecione **Adicionar** e **novo item**. Em **código**, escolha o item de **classe** e nomeie o arquivo *StoreIndexViewModel.cs*e clique em **Adicionar**.

    ![Adicionando uma nova classe](aspnet-mvc-4-fundamentals/_static/image19.png "Adicionando uma nova classe")

    *Adicionando uma nova classe*

    ![Criando classe StoreIndexViewModel](aspnet-mvc-4-fundamentals/_static/image20.png "Criando classe StoreIndexViewModel")

    *Criando classe StoreIndexViewModel*

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a>Tarefa 2-Adicionando propriedades à classe ViewModel

Há dois parâmetros a serem passados do StoreController para o modelo de exibição a fim de gerar a resposta HTML esperada: o número de gêneros na loja e uma lista desses gêneros.

Nesta tarefa, você adicionará essas duas propriedades à classe **StoreIndexViewModel** : **NumberOfGenres** (um inteiro) e **gêneros** (uma lista de cadeias de caracteres).

1. Adicione as propriedades **NumberOfGenres** e **gêneros** à classe **StoreIndexViewModel** . Para fazer isso, adicione as duas linhas a seguir à definição de classe:

    (Trecho de código- *ASP.NET MVC 4 Fundamentals-Ex5 StoreIndexViewModel Propriedades*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

> [!NOTE]
> A notação **{Get; Set;}** faz uso C#do recurso de propriedades implementadas automaticamente. Ele fornece os benefícios de uma propriedade sem exigir que declaremos um campo de apoio.

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a>Tarefa 3-atualizando StoreController para usar o StoreIndexViewModel

A classe **StoreIndexViewModel** encapsula as informações necessárias para passar do método de **índice** de **StoreController**para um modelo de exibição a fim de gerar uma resposta.

Nesta tarefa, você atualizará o **StoreController** para usar o **StoreIndexViewModel**.

1. Abra a classe **StoreController** .

    ![Abrindo classe StoreController](aspnet-mvc-4-fundamentals/_static/image21.png "Abrindo classe StoreController")

    *Abrindo classe StoreController*
2. Para usar a classe **StoreIndexViewModel** do **StoreController**, adicione o seguinte namespace na parte superior do código **StoreController** :

    (Trecho de código- *ASP.NET MVC 4 Fundamentals-Ex5 StoreIndexViewModel usando ViewModels*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. Altere o método de ação de **índice** de **StoreController**para que ele crie e preencha um objeto **StoreIndexViewModel** e, em seguida, o passe para um modelo de exibição para gerar uma resposta HTML com ele.

    > [!NOTE]
    > No laboratório &quot;modelos e acesso a dados do ASP.NET MVC&quot; você escreverá um código que recupere a lista de gêneros da loja de um banco de dados. No código a seguir, você criará uma **lista** de gêneros de dados fictícios que preencherão o **StoreIndexViewModel**.
    > 
    > Depois de criar e configurar o objeto **StoreIndexViewModel** , ele será passado como um argumento para o método **View** . Isso indica que o modelo de exibição usará esse objeto para gerar uma resposta HTML com ele.
4. Substitua o método de **índice** pelo seguinte código:

    (Trecho de código- *ASP.NET MVC 4 Fundamentals-Ex5 StoreController index Method*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

> [!NOTE]
> Se você não estiver familiarizado com C#o, poderá pressupor que o uso de **var** significa que a variável **viewModel** é de associação tardia. Isso não está correto-o C# compilador está usando a inferência de tipos com base no que você atribui à variável para determinar que **viewModel** é do tipo **StoreIndexViewModel**. Além disso, ao compilar a variável **viewModel** local como um tipo **StoreIndexViewModel** , você obtém a verificação de tempo de compilação e o suporte ao editor de código do Visual Studio.

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a>Tarefa 4-Criando um modelo de exibição que usa StoreIndexViewModel

Nesta tarefa, você criará um modelo de exibição que usará um objeto StoreIndexViewModel passado do controlador para exibir uma lista de gêneros.

1. Antes de criar o novo modelo de exibição, vamos criar o projeto para que a **caixa de diálogo Adicionar exibição** saiba mais sobre a classe **StoreIndexViewModel** . Crie o projeto selecionando o item de menu **Build** e, em seguida, **Build MvcMusicStore**.

    ![Compilando o projeto](aspnet-mvc-4-fundamentals/_static/image22.png "Compilando o projeto")

    *Compilando o projeto*
2. Crie um novo modelo de exibição. Para fazer isso, clique com o botão direito do mouse dentro do método **index** e selecione **Add View**.

    ![Adicionando uma exibição](aspnet-mvc-4-fundamentals/_static/image23.png "Adicionar uma exibição")

    *Adicionando uma exibição*
3. Como a **caixa de diálogo Adicionar exibição** foi invocada do **StoreController**, ela adicionará o modelo de exibição por padrão em um arquivo **\Views\Store\Index.cshtml** . Marque a caixa de seleção **criar uma exibição fortemente tipada** e, em seguida, selecione **StoreIndexViewModel** como a **classe de modelo**. Além disso, verifique se o mecanismo de exibição selecionado é **Razor**. Clique em **Adicionar**.

    ![Caixa de diálogo Adicionar exibição](aspnet-mvc-4-fundamentals/_static/image24.png "Caixa de diálogo Adicionar exibição")

    *Caixa de diálogo Adicionar exibição*

    O arquivo de modelo de exibição **\Views\Store\Index.cshtml** é criado e aberto. Com base nas informações fornecidas para a caixa de diálogo **Adicionar exibição** na última etapa, o modelo de exibição esperará uma instância de **StoreIndexViewModel** como os dados a serem usados para gerar uma resposta em HTML. Você observará que o modelo herda um `ViewPage<musicstore.viewmodels.storeindexviewmodel>` no C#.

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a>Tarefa 5-Atualizando o modelo de exibição

Nesta tarefa, você atualizará o modelo de exibição criado na última tarefa para recuperar o número de gêneros e seus nomes dentro da página.

> [!NOTE]
> Você usará @ Syntax (geralmente conhecido como &quot;código Nuggets&quot;) para executar código dentro do modelo de exibição.

1. No arquivo **index. cshtml** , dentro da pasta **Store** , substitua seu código pelo seguinte:

[!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > As soon as you finish typing the period after the word **Model**, Visual Studio's Intellisense will show a list of possible properties and methods to choose from.
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > *Getting Model properties and methods with Visual Studio's IntelliSense*
    > 
    > The **Model** property references the **StoreIndexViewModel** object that the Controller passed to the View template. This means that you can access all of the data passed from the Controller to the View template via the **Model** property, and format it into an appropriate HTML response within the View template.
    > 
    > You can just select the **NumberOfGenres** property from the Intellisense list rather than typing it in and then it will auto-complete it by pressing the **tab key**.
2. Faça um loop sobre a lista de gênero em **StoreIndexViewModel** e crie uma lista de **&gt;de&lt;UL** de HTML usando um loop **foreach** .
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. Pressione **F5** para executar o aplicativo e procurar **/Store**. Você verá a lista de gêneros passados no objeto **StoreIndexViewModel** do **StoreController** para o modelo de exibição.

    ![Exibir exibindo uma lista de gêneros](aspnet-mvc-4-fundamentals/_static/image26.png "Exibir exibindo uma lista de gêneros")

    *Exibir exibindo uma lista de gêneros*
4. Feche o navegador.

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a>Exercício 6: usando parâmetros na exibição

No exercício 3, você aprendeu a passar parâmetros para o controlador. Neste exercício, você aprenderá a usar esses parâmetros no modelo de exibição. Para essa finalidade, você será apresentado primeiro a classes de modelo que o ajudarão a gerenciar seus dados e a lógica do domínio. Além disso, você aprenderá a criar links para páginas dentro do aplicativo MVC ASP.NET sem se preocupar com a codificação de caminhos de URL.

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a>Tarefa 1-adicionando classes de modelo

Ao contrário de ViewModels, que são criados apenas para passar informações do controlador para a exibição, as classes de modelo são criadas para conter e gerenciar dados e lógica de domínio. Nesta tarefa, você adicionará duas classes de modelo para representar esses conceitos: **gênero** e **Album**.

1. Se ainda não estiver aberto, inicie **vs Express para Web**
2. No menu **arquivo** , escolha **Abrir projeto**. Na caixa de diálogo Abrir projeto, navegue até **Source\Ex06-UsingParametersInView\Begin**, selecione **Iniciar. sln** e clique em **abrir**. Como alternativa, você pode continuar com a solução obtida depois de concluir o exercício anterior.

   1. Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar. Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.
   2. Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, Compile a solução clicando em **build** | **Compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto. É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.
3. Adicione uma classe de modelo de **gênero** . Para fazer isso, clique com o botão direito do mouse na pasta **modelos** na **Gerenciador de soluções**, selecione **Adicionar** e, em seguida, a opção **novo item** . Em **código**, escolha o item de **classe** e nomeie o arquivo *Genre.cs*e clique em **Adicionar**.

    ![Adicionando uma classe](aspnet-mvc-4-fundamentals/_static/image27.png "Adicionando uma classe")

    *Adicionando um novo item*

    ![Adicionar classe de modelo de gênero](aspnet-mvc-4-fundamentals/_static/image28.png "Adicionar classe de modelo de gênero")

    *Adicionar classe de modelo de gênero*
4. Adicione uma propriedade **Name** à classe gênero. Para fazer isso, adicione o seguinte código:

    (Trecho de código- *ASP.NET MVC 4 Fundamentals-Ex6 gênero*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. Seguindo o mesmo procedimento que antes, adicione uma classe de **álbum** . Para fazer isso, clique com o botão direito do mouse na pasta **modelos** na **Gerenciador de soluções**, selecione **Adicionar** e, em seguida, a opção **novo item** . Em **código**, escolha o item de **classe** e nomeie o arquivo *Album.cs*e clique em **Adicionar**.
6. Adicione duas propriedades à classe Album: **gênero** e **title**. Para fazer isso, adicione o seguinte código:

    (Trecho de código- *ASP.NET MVC 4 Fundamentals – álbum Ex6*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a>Tarefa 2-Adicionando um StoreBrowseViewModel

Um **StoreBrowseViewModel** será usado nesta tarefa para mostrar os álbuns que correspondem a um gênero selecionado. Nesta tarefa, você criará essa classe e, em seguida, adicionará duas propriedades para manipular o **gênero** e a lista do seu **álbum**.

1. Adicione uma classe **StoreBrowseViewModel** . Para fazer isso, clique com o botão direito do mouse na pasta **ViewModels** na **Gerenciador de soluções**, selecione **Adicionar** e, em seguida, a opção **novo item** . Em **código**, escolha o item de **classe** e nomeie o arquivo *StoreBrowseViewModel.cs*e clique em **Adicionar**.
2. Adicione uma referência aos modelos na classe **StoreBrowseViewModel** . Para fazer isso, adicione o seguinte usando o namespace:

    (Trecho de código- *ASP.NET MVC 4 Fundamentals-Ex6 UsingModel*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. Adicione duas propriedades à classe **StoreBrowseViewModel** : **gênero** e **álbuns**. Para fazer isso, adicione o seguinte código:

    (Trecho de código- *ASP.NET MVC 4 Fundamentals – Ex6 modelproperties*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

> [!NOTE]
> O que é a **lista&lt;álbum&gt;** ?: essa definição está usando a **lista&lt;t&gt;** tipo, em que **t** restringe o tipo ao qual os elementos dessa **lista** pertencem, neste caso, o **álbum** (ou qualquer um de seus descendentes).
> 
> Essa capacidade de criar classes e métodos que adiam a especificação de um ou mais tipos até que a classe ou o método seja declarado e instanciado pelo código do cliente é C# um recurso do idioma chamado **genéricos**.
> 
> **List&lt;t&gt;** é o equivalente genérico do tipo **ArrayList** e está disponível no namespace **System. Collections. Generic** . Um dos benefícios de usar os **genéricos** é que, como o tipo é especificado, você não precisa cuidar das operações de verificação de tipo, como a conversão dos elementos no **álbum** como faria com uma **ArrayList**.

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a>Tarefa 3-usando o novo ViewModel no StoreController

Nesta tarefa, você modificará os métodos de ação **procurar** e **detalhes** do **StoreController**para usar o novo **StoreBrowseViewModel**.

1. Adicione uma referência à pasta modelos na classe **StoreController** . Para fazer isso, expanda a pasta **controladores** no **Gerenciador de soluções** e abra a classe **StoreController** . Em seguida, adicione o seguinte código:

    (Trecho de código- *ASP.NET MVC 4 Fundamentals-Ex6 UsingModelInController*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. Substitua o método de ação **procurar** para usar a classe **StoreViewBrowseController** . Você criará um gênero e dois novos objetos de álbuns com dados fictícios (no próximo laboratório prático, você consumirá dados reais de um banco de dado). Para fazer isso, substitua o método **Browse** pelo seguinte código:

    (Trecho de código- *ASP.NET MVC 4 Fundamentals-Ex6 BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. Substitua o método de ação de **detalhes** para usar a classe **StoreViewBrowseController** . Você criará um novo objeto de **álbum** a ser retornado para a **exibição**. Para fazer isso, substitua o método **Details** pelo seguinte código:

    (Trecho de código- *ASP.NET MVC 4 Fundamentals-Ex6 DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a>Tarefa 4-adicionando um modelo de exibição de navegação

Nesta tarefa, você adicionará um modo de exibição de **navegação** para mostrar os álbuns encontrados para um gênero específico.

1. Antes de criar o novo modelo de exibição, você deve compilar o projeto para que a caixa de diálogo **Adicionar exibição** saiba sobre a classe **ViewModel** a ser usada. Crie o projeto selecionando o item de menu **Build** e, em seguida, **Build MvcMusicStore**.
2. Adicione um modo de exibição de **navegação** . Para fazer isso, clique com o botão direito do mouse no método de ação **procurar** do **StoreController** e clique em **Adicionar exibição**.
3. Na caixa de diálogo **Adicionar exibição** , verifique se o nome da exibição é **procurar**. Marque a caixa de seleção **criar uma exibição fortemente tipada** e selecione **StoreBrowseViewModel** na lista suspensa **classe de modelo** . Deixe os outros campos com o valor padrão. Clique em **Adicionar**.

    ![Adicionando um modo de exibição de navegação](aspnet-mvc-4-fundamentals/_static/image29.png "Adicionando um modo de exibição de navegação")

    *Adicionando um modo de exibição de navegação*
4. Modifique o **Browse. cshtml** para exibir as informações do gênero, acessando o objeto **StoreBrowseViewModel** que é passado para o modelo de exibição. Para fazer isso, substitua o conteúdo pelo seguinte: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Tarefa 5-executando o aplicativo

Nesta tarefa, você testará que o método **Browse** recupera álbuns da ação do método **procurar** .

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home Page. Alterar a URL para **/Store/Browse? Gênero = disco** para verificar se a ação retorna dois álbuns.

    ![Pesquisar álbuns do disco do repositório](aspnet-mvc-4-fundamentals/_static/image30.png "Pesquisar álbuns do disco do repositório")

    *Pesquisar álbuns do disco do repositório*

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a>Tarefa 6-exibindo informações sobre um álbum específico

Nesta tarefa, você implementará a exibição de **Armazenamento/detalhes** para exibir informações sobre um álbum específico. Neste laboratório prático, tudo o que você exibirá sobre o álbum já está contido no modelo de **exibição** . Portanto, em vez de criar uma classe **StoreDetailsViewModel** , você usará o modelo **StoreBrowseViewModel** atual passando o álbum para ele.

1. Feche o navegador, se necessário, para retornar à janela do Visual Studio. Adicione uma nova exibição de **detalhes** para o método de ação **detalhes** do **StoreController**. Para fazer isso, clique com o botão direito do mouse no método **Details** na classe **StoreController** e clique em **Adicionar exibição**.
2. Na caixa de diálogo **Adicionar exibição** , verifique se o **nome da exibição** é **detalhes**. Marque a caixa de seleção **criar uma exibição fortemente tipada** e selecione **álbum** na lista suspensa **classe de modelo** . Deixe os outros campos com o valor padrão. Clique em **Adicionar**. Isso criará e abrirá um arquivo **\Views\Store\Details.cshtml** .

    ![Adicionando uma exibição de detalhes](aspnet-mvc-4-fundamentals/_static/image31.png "Adicionando uma exibição de detalhes")

    *Adicionando uma exibição de detalhes*
3. Modifique o arquivo **Details. cshtml** para exibir as informações do álbum, acessando o objeto de **álbum** que é passado para o modelo de exibição. Para fazer isso, substitua o conteúdo pelo seguinte: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>Tarefa 7-executando o aplicativo

Nesta tarefa, você testará que a exibição de **detalhes** recupera as informações do álbum do método de **ação de detalhes** .

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na **Home** Page. Altere a URL para **/Store/Details/5** para verificar as informações do álbum.

    ![Detalhando os álbuns de navegação](aspnet-mvc-4-fundamentals/_static/image32.png "Detalhando os álbuns de navegação")

    *Pesquisando detalhes do álbum*

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a>Tarefa 8-adicionando links entre páginas

Nesta tarefa, você adicionará um link na exibição da loja para ter um link em cada nome de gênero para a URL **/Store/Browse** apropriada. Dessa forma, quando você clicar em um gênero, por exemplo, o **disco**será navegado para **/Store/Browse? gênero =** URL de disco.

1. Feche o navegador, se necessário, para retornar à janela do Visual Studio. Atualize a página de **índice** para adicionar um link à página de **navegação** . Para fazer isso, no **Gerenciador de soluções** expanda a pasta **exibições** , em seguida, a pasta de **armazenamento** e clique duas vezes na página **index. cshtml** .
2. Adicione um link para o modo de exibição de navegação indicando o gênero selecionado. Para fazer isso, substitua o seguinte código realçado dentro das marcas de **&gt;&lt;li** : (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

   > [!NOTE]
   > outra abordagem seria vincular diretamente à página, com um código semelhante ao seguinte:
   > 
   > &lt;a href =&quot;/Store/Browse? gênero =@genreName&quot;&gt;@genreName&lt;/a&gt;
   > 
   > Embora essa abordagem funcione, ela depende de uma cadeia de caracteres codificada. Se posteriormente você renomear o controlador, precisará alterar essa instrução manualmente. Uma alternativa melhor é usar um método **auxiliar HTML** . O ASP.NET MVC inclui um método auxiliar HTML que está disponível para essas tarefas. O método auxiliar **HTML. ActionLink ()** facilita a criação **de html&lt;um&gt;** links, garantindo que os caminhos de URL sejam codificados corretamente em URL.
   > 
   > HTML. ActionLink tem várias sobrecargas. Neste exercício, você usará um que usa três parâmetros:
   > 
   > 1. Texto do link, que exibirá o nome do gênero
   > 2. Nome da ação do controlador (**procurar**)
   > 3. Rotear valores de parâmetro, especificando o nome (**gênero**) e o valor (**nome do gênero**)

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a>Tarefa 9-executando o aplicativo

Nesta tarefa, você testará que cada gênero é exibido com um link para a URL **/Store/Browse** apropriada.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home Page. Altere a URL para **/Store** para verificar se cada gênero está vinculado à URL **/Store/Browse** apropriada.

    ![Navegando em gêneros com links para a página de navegação](aspnet-mvc-4-fundamentals/_static/image33.png "Navegando em gêneros com links para a página de navegação")

    *Navegando em gêneros com links para a página de navegação*

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a>Tarefa 10-usando a coleção ViewModel dinâmica para passar valores

Nesta tarefa, você aprenderá um método simples e poderoso para passar valores entre o controlador e a exibição sem fazer nenhuma alteração no modelo. O ASP.NET MVC 4 fornece a coleção &quot;ViewModel&quot;, que pode ser atribuída a qualquer valor dinâmico e acessado dentro de controladores e exibições também.

Agora, você usará a coleção dinâmica ViewBag para passar uma lista de &quot;**gêneros estrelado**&quot; do controlador para a exibição. A exibição do índice de repositório será acessada pelo **ViewModel** e exibirá as informações.

1. Feche o navegador, se necessário, para retornar à janela do Visual Studio. Abra **StoreController.cs** e modifique o método **index** para criar uma lista de gêneros estrelado na coleção ViewModel:

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > Você também pode usar a sintaxe **ViewBag [&quot;estrelado&quot;]** para acessar as propriedades.
2. O ícone de estrela **&quot;o&quot;estrelado. png** está incluído na pasta **Source\Assets\Images** deste laboratório. Para adicioná-lo ao aplicativo, arraste o conteúdo de uma janela do **Windows Explorer** para o **Gerenciador de soluções** no Visual Web Developer Express, conforme mostrado abaixo:

    ![Adicionando imagem em estrela à solução](aspnet-mvc-4-fundamentals/_static/image34.png "Adicionando imagem em estrela à solução")

    *Adicionando imagem em estrela à solução*
3. Abra a exibição **Store/index. cshtml** e modifique o conteúdo. Você lerá a propriedade &quot;estrelado&quot; na coleção **ViewBag** e perguntará se o nome do gênero atual está na lista. Nesse caso, você mostrará um ícone de estrela à direita para o link de gênero.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a>Tarefa 11-executando o aplicativo

Nesta tarefa, você testará que os gêneros estrelado exibem um ícone de estrela.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na **Home** Page. Altere a URL para **/Store** para verificar se cada gênero em destaque tem o rótulo de respeito:

    ![Navegando em gêneros com elementos estrelado](aspnet-mvc-4-fundamentals/_static/image35.png "Navegando em gêneros com elementos estrelado")

    *Navegando em gêneros com elementos estrelado*

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a>Exercício 7: um colo sobre o novo modelo do MVC 4 do ASP.NET

Neste exercício, você explorará os aprimoramentos nos modelos de projeto do ASP.NET MVC 4, dando uma olhada nos recursos mais relevantes do novo modelo.

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a>Tarefa 1: explorando o modelo de aplicativo de Internet do ASP.NET MVC 4

1. Se ainda não estiver aberto, inicie **vs Express para Web**
2. Selecione o **arquivo | Novo |** Comando de menu do projeto. Na caixa de diálogo **novo projeto** , selecione **o C#Visual | Modelo da Web** na árvore do painel esquerdo e escolha o **aplicativo Web ASP.NET MVC 4**. **Nomeie** o projeto *MusicStore* e atualize o **nome da solução** para *começar*, em seguida, selecione um local (ou deixe o padrão) e clique em **OK**.

    ![Criando um novo projeto do ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image36.png "Criando um novo projeto do ASP.NET MVC 4")

    *Criando um novo projeto do ASP.NET MVC 4*
3. Na caixa de diálogo **novo projeto do ASP.NET MVC 4** , selecione o modelo de projeto de **aplicativo da Internet** e clique em **OK**. Observe que você pode selecionar o Razor ou o ASPX como o mecanismo de exibição.

    ![Criando um novo aplicativo de Internet ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image37.png "Criando um novo aplicativo de Internet ASP.NET MVC 4")

    *Criando um novo aplicativo de Internet ASP.NET MVC 4*

    > [!NOTE]
    > Sintaxe Razor foi introduzido no ASP.NET MVC 3. Seu objetivo é minimizar o número de caracteres e os pressionamentos de teclas necessários em um arquivo, permitindo um fluxo de trabalho de codificação rápido e fluido. O Razor aproveita as C#habilidades da linguagem/vb (ou outras) existentes e fornece uma sintaxe de marcação de modelo que habilita um fluxo de trabalho de construção HTML incrível.
4. Pressione **F5** para executar a solução e ver o modelo renovado. Você pode conferir os seguintes recursos:

    1. **Modelos de estilo moderno**

        Os modelos foram renovados, fornecendo mais estilos de aparência moderna.

        ![Modelos reestilizados do ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image38.png "Modelos reestilizados do ASP.NET MVC 4")

        *Modelos reestilizados do ASP.NET MVC 4*
    2. **Renderização adaptável**

        Confira redimensionar a janela do navegador e observe como o layout da página se adapta dinamicamente ao novo tamanho da janela. Esses modelos usam a técnica de renderização adaptável para renderizar adequadamente nas plataformas desktop e móvel sem qualquer personalização.

        ![Modelo de projeto ASP.NET MVC 4 em diferentes tamanhos de navegador](aspnet-mvc-4-fundamentals/_static/image39.png "Modelo de projeto ASP.NET MVC 4 em diferentes tamanhos de navegador")

        *Modelo de projeto ASP.NET MVC 4 em diferentes tamanhos de navegador*
5. Feche o navegador para parar o depurador e retornar ao Visual Studio.
6. Agora você pode explorar a solução e conferir alguns dos novos recursos introduzidos pelo ASP.NET MVC 4 no modelo de projeto.

    ![ASP.NET MVC4-Internet-Application-Project-template](aspnet-mvc-4-fundamentals/_static/image40.png "O modelo de projeto de aplicativo de Internet do ASP.NET MVC 4")

    *O modelo de projeto de aplicativo de Internet do ASP.NET MVC 4*

   1. **Marcação HTML5**

       Procurar exibições de modelo para descobrir a nova marcação de tema, por exemplo, abra a exibição **about. cshtml** na pasta **base** .

       ![Novo modelo, usando marcação Razor e HTML5](aspnet-mvc-4-fundamentals/_static/image41.png "Novo modelo, usando marcação Razor e HTML5")

       *Novo modelo, usando marcação Razor e HTML5*
   2. **Bibliotecas JavaScript incluídas**

      1. **jQuery**: o jQuery simplifica a travessia de documentos HTML, manipulação de eventos, animação e interações com AJAX.
      2. **interface do usuário do jQuery**: essa biblioteca fornece abstrações para interação e animação de baixo nível, efeitos avançados e widgets passíveis, criados sobre a biblioteca JavaScript do jQuery.

         > [!NOTE]
         > Você pode aprender sobre o jQuery e a interface do usuário do jQuery no [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).
      3. **KnockoutJS**: o modelo padrão ASP.NET MVC 4 agora inclui o **KnockoutJS**, uma estrutura MVVM do JavaScript que permite criar aplicativos Web avançados e altamente responsivos usando JavaScript e HTML. Como nas bibliotecas de interface do usuário do ASP.NET MVC 3, jQuery e jQuery também estão incluídas no ASP.NET MVC 4.

          > [!NOTE]
          > Você pode obter mais informações sobre a biblioteca KnockOutJS neste link: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).
      4. **Modernizr**: essa biblioteca é executada automaticamente, tornando seu site compatível com navegadores mais antigos ao usar as tecnologias HTML5 e CSS3.

          > [!NOTE]
          > Você pode obter mais informações sobre a biblioteca do Modernizr neste link: [http://www.modernizr.com/](http://www.modernizr.com/).
   3. **SimpleMembership incluídos na solução**

       O SimpleMembership foi projetado como uma substituição para a função ASP.NET anterior e o sistema do provedor de associação. Ele tem muitos recursos novos que tornam mais fácil para o desenvolvedor proteger páginas da Web de maneira mais flexível.

       O modelo da Internet já configurou algumas coisas para integrar o SimpleMembership, por exemplo, o AccountController está preparado para usar o OAuthWebSecurity (para registro de conta OAuth, logon, gerenciamento, etc.) e segurança da Web.

       ![SimpleMembership incluídos na solução](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership incluídos na solução")

       *SimpleMembership incluídos na solução*

       > [!NOTE]
       > Encontre mais informações sobre o [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) no msdn.

> [!NOTE]
> Além disso, você pode implantar esse aplicativo em sites do Windows Azure seguindo [o apêndice B: publicando um aplicativo ASP.NET MVC 4 usando implantação da Web](#AppendixB).

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Resumo

Ao concluir este laboratório prático, você aprendeu os conceitos básicos do ASP.NET MVC:

- Os principais elementos de um aplicativo MVC e como eles interagem
- Como criar um aplicativo MVC do ASP.NET
- Como adicionar e configurar controladores para lidar com parâmetros passados por meio da URL e da QueryString
- Como adicionar uma página mestra de layout para configurar um modelo para conteúdo HTML comum, uma folha de estilos para aprimorar a aparência e um modelo de exibição para exibir conteúdo HTML
- Como usar o padrão ViewModel para passar Propriedades para o modelo de exibição para exibir informações dinâmicas
- Como usar parâmetros passados para controladores no modelo de exibição
- Como adicionar links a páginas dentro do aplicativo MVC ASP.NET
- Como adicionar e usar propriedades dinâmicas em uma exibição
- Os aprimoramentos nos modelos de projeto do ASP.NET MVC 4

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apêndice A: Instalando o Visual Studio Express 2012 para Web

Você pode instalar o **Microsoft Visual Studio Express 2012 para Web** ou outra versão &quot;Express&quot; usando o **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** . As instruções a seguir orientam você pelas etapas necessárias para instalar o *Visual Studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.

1. Vá para [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Como alternativa, se você já tiver instalado o Web Platform Installer, poderá abri-lo e pesquisar o produto &quot;<em>Visual Studio Express 2012 para Web com o SDK do Windows Azure</em>&quot;.
2. Clique em **instalar agora**. Se você não tiver **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.
3. Quando **Web Platform Installer** estiver aberto, clique em **instalar** para iniciar a instalação.

    ![Instalar Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "Instalar Visual Studio Express")

    *Instalar Visual Studio Express*
4. Leia todos os termos e licenças de produtos e clique em **aceito** para continuar.

    ![Aceitando os termos de licença](aspnet-mvc-4-fundamentals/_static/image44.png)

    *Aceitando os termos de licença*
5. Aguarde até que o processo de download e instalação seja concluído.

    ![Progresso da instalação](aspnet-mvc-4-fundamentals/_static/image45.png)

    *Progresso da instalação*
6. Quando a instalação for concluída, clique em **concluir**.

    ![Instalação concluída](aspnet-mvc-4-fundamentals/_static/image46.png)

    *Instalação concluída*
7. Clique em **sair** para fechar Web Platform Installer.
8. Para abrir o Visual Studio Express para Web, vá para a tela **Iniciar** e comece a escrever &quot;**vs Express**&quot;e, em seguida, clique no bloco **vs Express para Web** .

    ![Bloco VS Express para Web](aspnet-mvc-4-fundamentals/_static/image47.png)

    *Bloco VS Express para Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Apêndice B: publicando um aplicativo ASP.NET MVC 4 usando Implantação da Web

Este apêndice mostrará como criar um novo site da Web do Windows Azure Portal de Gerenciamento e publicar o aplicativo obtido seguindo o laboratório, aproveitando o recurso de publicação de Implantação da Web fornecido pelo Windows Azure.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Tarefa 1-Criando um novo site no portal do Windows Azure

1. Acesse o [portal de gerenciamento do Windows Azure](https://manage.windowsazure.com/) e entre usando as credenciais da Microsoft associadas à sua assinatura.

    > [!NOTE]
    > Com o Windows Azure, você pode hospedar 10 sites da Web de ASP.NET gratuitamente e, em seguida, dimensionar conforme o tráfego cresce. Você pode se inscrever [aqui](https://aka.ms/aspnet-hol-azure).

    ![Fazer logon no Windows portal do Azure](aspnet-mvc-4-fundamentals/_static/image48.png "Fazer logon no Windows portal do Azure")

    *Fazer logon no Windows Azure Portal de Gerenciamento*
2. Clique em **novo** na barra de comandos.

    ![Criando um novo site](aspnet-mvc-4-fundamentals/_static/image49.png "Criando um novo site")

    *Criando um novo site*
3. Clique em **computação** | **site**. Em seguida, selecione a opção **criação rápida** . Forneça uma URL disponível para o novo site e clique em **criar site**.

    > [!NOTE]
    > Um site do Windows Azure é o host de um aplicativo Web em execução na nuvem que você pode controlar e gerenciar. A opção criação rápida permite que você implante um aplicativo Web concluído no site do Windows Azure de fora do Portal. Ele não inclui etapas para configurar um banco de dados.

    ![Criando um novo site usando a criação rápida](aspnet-mvc-4-fundamentals/_static/image50.png "Criando um novo site usando a criação rápida")

    *Criando um novo site usando a criação rápida*
4. Aguarde até que o novo **site** seja criado.
5. Depois que o site for criado, clique no link sob a coluna **URL** . Verifique se o novo site está funcionando.

    ![Navegando até o novo site](aspnet-mvc-4-fundamentals/_static/image51.png "Navegando até o novo site")

    *Navegando até o novo site*

    ![Site em execução](aspnet-mvc-4-fundamentals/_static/image52.png "Site em execução")

    *Site em execução*
6. Volte para o portal e clique no nome do site na coluna **nome** para exibir as páginas de gerenciamento.

    ![Abrindo as páginas de gerenciamento de site](aspnet-mvc-4-fundamentals/_static/image53.png "Abrindo as páginas de gerenciamento de site")

    *Abrindo as páginas de gerenciamento de site*
7. Na página **painel** , na seção **visão rápida** , clique no link **baixar perfil de publicação** .

    > [!NOTE]
    > O *perfil de publicação* contém todas as informações necessárias para publicar um aplicativo Web em um site do Windows Azure para cada método de publicação habilitado. O perfil de publicação contém as URLs, as credenciais de usuário e as cadeias de conexão de banco de dados necessárias para conectar-se e autenticar cada um dos pontos de extremidade para os quais um método de publicação é habilitado. **O Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** e **Microsoft Visual Studio 2012** dão suporte à leitura de perfis de publicação para automatizar a configuração desses programas para a publicação de aplicativos Web em sites do Windows Azure.

    ![Baixando o perfil de publicação do site](aspnet-mvc-4-fundamentals/_static/image54.png "Baixando o perfil de publicação do site")

    *Baixando o perfil de publicação do site*
8. Baixe o arquivo de perfil de publicação em um local conhecido. Neste exercício, você verá como usar esse arquivo para publicar um aplicativo Web em um site do Windows Azure a partir do Visual Studio.

    ![Salvando o arquivo de perfil de publicação](aspnet-mvc-4-fundamentals/_static/image55.png "Salvando o perfil de publicação")

    *Salvando o arquivo de perfil de publicação*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tarefa 2-Configurando o servidor de banco de dados

Se seu aplicativo utiliza bancos de dados SQL Server, você precisará criar um servidor de banco de dados SQL. Se você quiser implantar um aplicativo simples que não usa SQL Server você pode ignorar essa tarefa.

1. Você precisará de um servidor do banco de dados SQL para armazenar o banco de dados do aplicativo. Você pode exibir os servidores do banco de dados SQL de sua assinatura no portal de gerenciamento do Windows Azure em bancos de dados **sql** | **servidores** | **painel do servidor**. Se você não tiver um servidor criado, poderá criar um usando o botão **Adicionar** na barra de comandos. Anote o nome do **servidor e a URL, o nome de logon e a senha do administrador**, pois você irá usá-los nas próximas tarefas. Não crie o banco de dados ainda, pois ele será criado em um estágio posterior.

    ![Painel do servidor do banco de dados SQL](aspnet-mvc-4-fundamentals/_static/image56.png "Painel do servidor do banco de dados SQL")

    *Painel do servidor do banco de dados SQL*
2. Na próxima tarefa, você testará a conexão de banco de dados do Visual Studio, por esse motivo, será necessário incluir o endereço IP local na lista de **endereços IP permitidos**do servidor. Para fazer isso, clique em **Configurar**, selecione o endereço IP do **endereço IP do cliente atual** e cole-o nas caixas de texto endereço IP **inicial** e **endereço IP final** e clique no botão ![adicionar-cliente-IP-endereço-OK-botão](aspnet-mvc-4-fundamentals/_static/image57.png).

    ![Adicionando endereço IP do cliente](aspnet-mvc-4-fundamentals/_static/image58.png)

    *Adicionando endereço IP do cliente*
3. Depois que o **endereço IP do cliente** for adicionado à lista endereços IP permitidos, clique em **salvar** para confirmar as alterações.

    ![Confirmar alterações](aspnet-mvc-4-fundamentals/_static/image59.png)

    *Confirmar alterações*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tarefa 3-publicando um aplicativo ASP.NET MVC 4 usando Implantação da Web

1. Volte para a solução ASP.NET MVC 4. Na **Gerenciador de soluções**, clique com o botão direito do mouse no projeto de site e selecione **publicar**.

    ![Publicando o aplicativo](aspnet-mvc-4-fundamentals/_static/image60.png "Publicando o aplicativo")

    *Publicando o site*
2. Importe o perfil de publicação salvo na primeira tarefa.

    ![Importando o perfil de publicação](aspnet-mvc-4-fundamentals/_static/image61.png "Importando o perfil de publicação")

    *Importando perfil de publicação*
3. Clique em **validar conexão**. Depois que a validação for concluída, clique em **Avançar**.

    > [!NOTE]
    > A validação será concluída quando você vir uma marca de seleção verde ao lado do botão Validar conexão.

    ![Validando conexão](aspnet-mvc-4-fundamentals/_static/image62.png "Validando conexão")

    *Validando conexão*
4. Na página **configurações** , na seção **bancos** de dados, clique no botão ao lado da caixa de texto da sua conexão de banco (ou seja, **DefaultConnection**).

    ![Configuração de implantação da Web](aspnet-mvc-4-fundamentals/_static/image63.png "Configuração de implantação da Web")

    *Configuração de implantação da Web*
5. Configure a conexão de banco de dados da seguinte maneira:

   - No **nome do servidor** , digite a URL do servidor do banco de dados SQL usando o prefixo *TCP:* .
   - Em **nome de usuário** , digite o nome de logon do administrador do servidor.
   - Em **senha** , digite a senha de logon do administrador do servidor.
   - Digite um novo nome de banco de dados, por exemplo: *MVC4SampleDB*.

     ![Configurando a cadeia de conexão de destino](aspnet-mvc-4-fundamentals/_static/image64.png "Configurando a cadeia de conexão de destino")

     *Configurando a cadeia de conexão de destino*
6. Em seguida, clique em **OK**. Quando for solicitado a criar o banco de dados, clique em **Sim**.

    ![Criando o banco de dados](aspnet-mvc-4-fundamentals/_static/image65.png "Criando a cadeia de caracteres do banco de dados")

    *Criando o banco de dados*
7. A cadeia de conexão que você usará para se conectar ao banco de dados SQL no Windows Azure é mostrada na caixa de texto conexão padrão. Em seguida, clique em **Próximo**.

    ![Cadeia de conexão apontando para o banco de dados SQL](aspnet-mvc-4-fundamentals/_static/image66.png "Cadeia de conexão apontando para o banco de dados SQL")

    *Cadeia de conexão apontando para o banco de dados SQL*
8. Na página **Visualização** , clique em **publicar**.

    ![Publicando o aplicativo Web](aspnet-mvc-4-fundamentals/_static/image67.png "Publicando o aplicativo Web")

    *Publicando o aplicativo Web*
9. Quando o processo de publicação for concluído, o navegador padrão abrirá o site publicado.

    ![Aplicativo publicado no Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "Aplicativo publicado no Windows Azure")

    *Aplicativo publicado no Windows Azure*

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Apêndice C: usando trechos de código

Com trechos de código, você tem todo o código de que precisa ao seu alcance. O documento de laboratório informará exatamente quando você pode usá-los, conforme mostrado na figura a seguir.

![Usando trechos de código do Visual Studio para inserir código em seu projeto](aspnet-mvc-4-fundamentals/_static/image69.png "Usando trechos de código do Visual Studio para inserir código em seu projeto")

*Usando trechos de código do Visual Studio para inserir código em seu projeto*

***Para adicionar um trecho de código usando o tecladoC# (somente)***

1. Coloque o cursor onde você deseja inserir o código.
2. Comece digitando o nome do trecho de código (sem espaços ou hifens).
3. Observe como o IntelliSense exibe nomes de trechos de código correspondentes.
4. Selecione o trecho correto (ou continue digitando até que o nome do trecho inteiro seja selecionado).
5. Pressione a tecla TAB duas vezes para inserir o trecho de código no local do cursor.

![Comece a digitar o nome do trecho](aspnet-mvc-4-fundamentals/_static/image70.png "Comece a digitar o nome do trecho")

*Comece a digitar o nome do trecho*

![Pressione Tab para selecionar o trecho realçado](aspnet-mvc-4-fundamentals/_static/image71.png "Pressione Tab para selecionar o trecho realçado")

*Pressione Tab para selecionar o trecho realçado*

![Pressione Tab novamente e o trecho será expandido](aspnet-mvc-4-fundamentals/_static/image72.png "Pressione Tab novamente e o trecho será expandido")

*Pressione Tab novamente e o trecho será expandido*

***Para adicionar um trecho de código usando o mouseC#(, Visual Basic e XML)*** uma. Clique com o botão direito do mouse no local em que você deseja inserir o trecho de código.

1. Selecione **Inserir trecho** seguido por **meus trechos de código**.
2. Selecione o trecho relevante na lista clicando nele.

![Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho](aspnet-mvc-4-fundamentals/_static/image73.png "Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho")

*Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho*

![Selecione o trecho relevante na lista clicando nele](aspnet-mvc-4-fundamentals/_static/image74.png "Selecione o trecho relevante na lista clicando nele")

*Selecione o trecho relevante na lista clicando nele*
