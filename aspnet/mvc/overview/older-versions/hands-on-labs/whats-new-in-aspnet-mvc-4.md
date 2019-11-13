---
uid: mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
title: O que há de novo no ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: O ASP.NET MVC 4 é uma estrutura para a criação de aplicativos Web escalonáveis e baseados em padrões usando padrões de design bem estabelecidos e o poder do ASP.NET e...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 48f7feb3-872f-485d-b96f-e30011ff8c4a
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 4235f4fe666cdeb7d0821127a2b349f2ff30cd6e
ms.sourcegitcommit: 295cf898a4c87e264b0c35c7254b0fa4169f2278
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/13/2019
ms.locfileid: "74057039"
---
# <a name="whats-new-in-aspnet-mvc-4"></a>Novidades no ASP.NET MVC 4

Por [equipe de acampamentos da Web](https://twitter.com/webcamps)

[Baixe o kit de treinamento do Web acampamentos](https://aka.ms/webcamps-training-kit)

O ASP.NET MVC 4 é uma estrutura para a criação de aplicativos Web escalonáveis e baseados em padrões usando padrões de design bem estabelecidos e o poder do ASP.NET e do .NET Framework. Essa nova e quarta versão da estrutura se concentra em facilitar o desenvolvimento de aplicativos Web para dispositivos móveis.

Para começar, quando você cria um novo projeto do ASP.NET MVC 4, agora há um modelo de projeto de aplicativo móvel que você pode usar para criar um aplicativo autônomo especificamente para dispositivos móveis. Além disso, o ASP.NET MVC 4 integra-se ao jQuery Mobile por meio de um pacote NuGet jQuery. Mobile. MVC. o jQuery Mobile é uma estrutura baseada em HTML5 para o desenvolvimento de aplicativos Web que são compatíveis com todas as plataformas de dispositivos móveis populares, incluindo Windows Phone, iPhone, Android e assim por diante. No entanto, se você precisar de especialização, o ASP.NET MVC 4 também permitirá que você atenda diferentes modos de exibição para diferentes dispositivos e forneça otimizações específicas do dispositivo.

Neste laboratório prático, você começará com o modelo de projeto do ASP.NET MVC 4 &quot;Internet Application&quot; para criar um aplicativo da Galeria de fotos. Você aprimorará progressivamente o aplicativo usando os novos recursos do jQuery Mobile e do ASP.NET MVC 4 para torná-lo compatível com diferentes dispositivos móveis e navegadores da Web da área de trabalho. Você também aprenderá sobre novas receitas de código para geração de código e como o ASP.NET MVC 4 facilita a gravação de métodos de ação assíncronos por meio de suporte à tarefa&lt;ActionResult&gt; tipos de retorno.

> [!NOTE]
> Todos os códigos de exemplo e trechos de código estão incluídos no Web acampamentos Training Kit, disponível nas [versões Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). O projeto específico deste laboratório está disponível no [que há de novo no Web Forms no ASP.NET 4,5](https://github.com/Microsoft-Web/HOL-ASPNETWebForms).

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

Neste laboratório prático, você aprenderá a:

- Aproveite os aprimoramentos feitos nos modelos de projeto do ASP.NET MVC – incluindo o novo modelo de projeto de aplicativo móvel
- Use o atributo visor do HTML5 e as consultas de mídia do CSS para melhorar a exibição em dispositivos móveis
- Use o jQuery Mobile para aprimoramentos progressivos e para a criação de interface do usuário da Web otimizada para toque
- Criar exibições específicas para dispositivos móveis
- Use o componente de seletor de exibição para alternar entre exibições móveis e de área de trabalho no aplicativo
- Criar controladores assíncronos usando o suporte a tarefas

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisites

Você deve ter os seguintes itens para concluir este laboratório:

- [Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou superior (leia o [Apêndice B](#AppendixB) para obter instruções sobre como instalá-lo).
- [ASP.NET MVC 4](../../../mvc4.md) (incluído na instalação do Microsoft Visual Studio 2012)
- Emulador de Windows Phone (incluído no [SDK do 7.1.1 Windows Phone](https://www.microsoft.com/download/details.aspx?id=29233))
- Opcional- [WebMatrix 2](https://www.microsoft.com/web/webmatrix/) com extensão de **simulador de iPhone Black** (somente para o exercício 3 usado para navegar pelo aplicativo Web com um simulador de iPhone)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Configuração

Em todo o documento do laboratório, você será instruído a inserir blocos de código. Para sua conveniência, a maior parte desse código é fornecida como trechos de Visual Studio Code, que podem ser usados no Visual Studio para evitar a necessidade de adicioná-lo manualmente.

Para instalar os trechos de código:

1. Abra uma janela do Windows Explorer e navegue até a pasta **Source\Setup** do laboratório.
2. Clique duas vezes no arquivo **Setup. cmd** nessa pasta para instalar os trechos de código do Visual Studio.

Se você não estiver familiarizado com os trechos de Visual Studio Code e quiser saber como usá-los, consulte o apêndice deste documento &quot;[Apêndice a: usando trechos de código](#AppendixA)&quot;.

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Exercícios

Este laboratório prático inclui os seguintes exercícios:

1. [Novos modelos de projeto do MVC 4 ASP.NET](#Exercise1)
2. [Criando o aplicativo Web Galeria de fotos](#Exercise2)
3. [Adicionando suporte para dispositivos móveis](#Exercise3)
4. [Usando controladores assíncronos](#Exercise4)

> [!NOTE]
> Cada exercício é acompanhado por uma pasta **final** que contém a solução resultante que você deve obter depois de concluir os exercícios. Você pode usar essa solução como um guia se precisar de ajuda adicional para trabalhar com os exercícios.

Tempo estimado para concluir este laboratório: **60 minutos**.

<a id="Exercise1"></a>

<a id="Exercise_1_New_ASPNET_MVC_4_Project_Templates"></a>
### <a name="exercise-1-new-aspnet-mvc-4-project-templates"></a>Exercício 1: novos modelos de projeto do ASP.NET MVC 4

Neste exercício, você irá explorar os aprimoramentos nos modelos de projeto do ASP.NET MVC 4. Além do modelo de aplicativo de Internet, já presente no MVC 3, essa versão agora inclui um modelo separado para aplicativos móveis. Primeiro, você examinará alguns recursos relevantes de cada um dos modelos. Em seguida, você trabalhará para renderizar sua página corretamente nas diferentes plataformas usando a abordagem certa.

<a id="Task_1_-_Exploring_the_Internet_Application_Template"></a>
#### <a name="task-1---exploring-the-internet-application-template"></a>Tarefa 1-explorando o modelo de aplicativo de Internet

1. Abra o **Visual Studio**.
2. Selecione o **arquivo | Novo |** Comando de menu do projeto. Na caixa de diálogo **novo projeto** , selecione **o C# Visual | Modelo da Web** na árvore do painel esquerdo e escolha **aplicativo Web ASP.NET MVC 4.** Nomeie o projeto **Galeria**, selecione um local (ou deixe o padrão) e clique em **OK**.

    > [!NOTE]
    > Posteriormente, você personalizará a solução ASP.NET MVC 4 do Gallery que está criando agora.

    ![Criando um novo projeto](whats-new-in-aspnet-mvc-4/_static/image1.png "Criando um novo projeto")

    *Criando um novo projeto*
3. Na caixa de diálogo **novo projeto do ASP.NET MVC 4** , selecione o modelo de projeto de **aplicativo da Internet** e clique em **OK**. Verifique se você selecionou Razor como o mecanismo de exibição.

    ![Criando um novo aplicativo de Internet ASP.NET MVC 4](whats-new-in-aspnet-mvc-4/_static/image2.png "Criando um novo aplicativo de Internet ASP.NET MVC 4")

    *Criando um novo aplicativo de Internet ASP.NET MVC 4*

    > [!NOTE]
    > Sintaxe Razor foi introduzido no ASP.NET MVC 3. Seu objetivo é minimizar o número de caracteres e os pressionamentos de teclas necessários em um arquivo, permitindo um fluxo de trabalho de codificação rápido e fluido. O Razor aproveita as C# habilidades de linguagem existentes/vb (ou outras) e fornece uma sintaxe de marcação de modelo que habilita um fluxo de trabalho de construção HTML incrível.
4. Pressione **F5** para executar a solução e ver os modelos renovados. Você pode conferir os seguintes recursos:

    **Modelos de estilo moderno**

    Os modelos foram renovados, fornecendo mais estilos de aparência moderna.

    ![Modelos reestilizados do ASP.NET MVC 4](whats-new-in-aspnet-mvc-4/_static/image3.png "Modelos com reestilo MVC 4")

    *Modelos reestilizados do ASP.NET MVC 4*

    ![Nova página de contato](whats-new-in-aspnet-mvc-4/_static/image4.png "Nova página de contato")

    *Nova página de contato*

    **Renderização adaptável**

    Confira redimensionar a janela do navegador e observe como o layout da página se adapta dinamicamente ao novo tamanho da janela. Esses modelos usam a técnica de renderização adaptável para renderizar adequadamente nas plataformas desktop e móvel sem qualquer personalização.

    ![Modelo de projeto ASP.NET MVC 4 em diferentes tamanhos de navegador](whats-new-in-aspnet-mvc-4/_static/image5.png "Modelo de projeto ASP.NET MVC 4 em diferentes tamanhos de navegador")

    *Modelo de projeto ASP.NET MVC 4 em diferentes tamanhos de navegador*

    **Interface do usuário mais rica com JavaScript**

    Outro aprimoramento dos modelos de projeto padrão é o uso do JavaScript para fornecer um JavaScript mais interativo. Os links login e Register usados no modelo exemplificam como usar as validações do jQuery para validar os campos de entrada do lado do cliente.

    ![Validação do jQuery](whats-new-in-aspnet-mvc-4/_static/image6.png)

    *Validação do jQuery*

    > [!NOTE]
    > Observe as duas seções de logon, na primeira seção, você pode fazer logon usando uma conta registrada do site e, na segunda seção, você pode fazer logon como alternativa usando outro serviço de autenticação como o Google (desabilitado por padrão).
5. Feche o navegador para parar o depurador e retornar ao Visual Studio.
6. Abra o arquivo **authconfig.cs** localizado na pasta **\_iniciar do aplicativo** .
7. Remova o comentário da última linha para registrar o Google Client para autenticação *OAuth* .

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample1.cs)]

    > [!NOTE]
    > Observe que você pode habilitar a autenticação facilmente usando qualquer serviço OpenID ou OAuth, como Facebook, Twitter, Microsoft, etc.
8. Pressione **F5** para executar a solução e navegar até a página de logon.
9. Selecione serviço do **Google** para fazer logon.

    ![Selecionando o serviço de logon](whats-new-in-aspnet-mvc-4/_static/image7.png)

    *Selecionando o serviço de logon*
10. Faça logon usando sua conta do Google.
11. Permitir que o site (localhost) recupere informações da conta do Google.
12. Por fim, você precisará se registrar no site para associar a conta do Google.

   ![Associar sua conta do Google](whats-new-in-aspnet-mvc-4/_static/image8.png)

   *Associando sua conta do Google*
13. Feche o navegador para parar o depurador e retornar ao Visual Studio.
14. Agora Explore a solução para conferir alguns outros novos recursos introduzidos pelo ASP.NET MVC 4 no modelo de projeto.

   ![O modelo de projeto de aplicativo de Internet do ASP.NET MVC 4](whats-new-in-aspnet-mvc-4/_static/image9.png "O modelo de projeto de aplicativo de Internet do ASP.NET MVC 4")

   *O modelo de projeto de aplicativo de Internet do ASP.NET MVC 4*

    - **Marcação HTML 5**

       Procurar modos de exibição de modelo para descobrir a nova marcação de tema.

       ![Novo modelo, usando a marcação Razor e HTML5 sobre. cshtml.](whats-new-in-aspnet-mvc-4/_static/image10.png "Novo modelo, usando a marcação Razor e HTML5 sobre. cshtml.")

       *Novo modelo, usando marcação Razor e HTML5 (about. cshtml).*
    - **Bibliotecas JavaScript atualizadas**

       O modelo padrão ASP.NET MVC 4 agora inclui o KnockoutJS, uma estrutura MVVM do JavaScript que permite criar aplicativos Web avançados e altamente responsivos usando JavaScript e HTML. Como no MVC3, as bibliotecas de interface do usuário do jQuery e do jQuery também estão incluídas no ASP.NET MVC 4.

     > [!NOTE]
     > Você pode obter mais informações sobre a biblioteca KnockOutJS neste link: [[http://learn.knockoutjs.com/](http://learn.knockoutjs.com/)](http://learn.knockoutjs.com/). Além disso, você pode aprender sobre o jQuery e a interface do usuário do jQuery no [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).

<a id="Task_2_-_Exploring_the_Mobile_Application_Template"></a>
#### <a name="task-2---exploring-the-mobile-application-template"></a>Tarefa 2-explorando o modelo de aplicativo móvel

O ASP.NET MVC 4 facilita o desenvolvimento de sites para navegadores móveis e tablets. Esse modelo tem a mesma estrutura de aplicativo que o modelo de aplicativo de Internet (Observe que o código do controlador é praticamente idêntico), mas seu estilo foi modificado para renderizar corretamente em dispositivos móveis baseados em toque.

1. Selecione o **arquivo | Novo |** Comando de menu do projeto. Na caixa de diálogo **novo projeto** , selecione **o C# Visual | Modelo da Web** na árvore do painel esquerdo e escolha o **aplicativo Web ASP.NET MVC 4.** Nomeie o projeto **Galeria de galerias. móvel**, selecione um local (ou deixe o padrão), selecione &quot;adicionar ao Solution&quot; e clique em **OK**.
2. Na caixa de diálogo **novo projeto do ASP.NET MVC 4** , selecione o modelo de projeto de **aplicativo móvel** e clique em **OK**. Verifique se você selecionou Razor como o mecanismo de exibição.

    ![Criando um novo aplicativo móvel ASP.NET MVC 4](whats-new-in-aspnet-mvc-4/_static/image11.png "Criando um novo aplicativo móvel ASP.NET MVC 4")

    *Criando um novo aplicativo móvel ASP.NET MVC 4*
3. Agora você pode explorar a solução e conferir alguns dos novos recursos introduzidos pelo modelo de solução ASP.NET MVC 4 para dispositivos móveis:

    - **Biblioteca do jQuery Mobile**

        O modelo de projeto de aplicativo móvel inclui a biblioteca do jQuery Mobile, que é uma biblioteca de software livre para compatibilidade com navegadores móveis. o jQuery Mobile aplica aprimoramento progressivo a navegadores móveis que dão suporte a CSS e a JavaScript. O aprimoramento progressivo permite que todos os navegadores exibam o conteúdo básico de uma página da Web, enquanto só permite que os navegadores mais poderosos exibam o conteúdo rico. Os arquivos JavaScript e CSS, incluídos no estilo do jQuery Mobile, ajudam os navegadores móveis a ajustar o conteúdo na tela sem fazer nenhuma alteração na marcação da página.

        ![jQuery-mobile-Library-Included-in-the-template](whats-new-in-aspnet-mvc-4/_static/image12.png)

        *biblioteca do jQuery Mobile incluída no modelo*
    - **Marcação baseada em HTML5**

        ![Mobile-aplicativo-modelo-using-HTML5-Markup](whats-new-in-aspnet-mvc-4/_static/image13.png)

        *Modelo de aplicativo móvel usando marcação HTML5, (login. cshtml e index. cshtml)*
4. Pressione **F5** para executar a solução.
5. Abra o **emulador Windows Phone 7**.
6. Na tela iniciar telefone, abra o Internet Explorer. Confira a URL em que o aplicativo de área de trabalho foi iniciado e navegue até a URL do telefone (por exemplo, `http://localhost:[PortNumber]/`).
7. Agora você pode inserir a página de logon ou conferir a página sobre. Observe que o estilo do site é baseado no novo aplicativo metro para dispositivos móveis. O modelo de projeto ASP.NET MVC 4 é exibido corretamente em dispositivos móveis, garantindo que todos os elementos da página estejam visíveis e habilitados. Observe que os links no cabeçalho são grandes o suficiente para serem clicados ou tocados.

    ![Páginas de modelo de projeto em um dispositivo móvel](whats-new-in-aspnet-mvc-4/_static/image14.png "Páginas de modelo de projeto em um dispositivo móvel")

    *Páginas de modelo de projeto em um dispositivo móvel*
8. O novo modelo também usa a **marca meta viewport**. A maioria dos navegadores móveis definem uma largura para uma janela de navegador virtual ou &quot;&quot;de visor, que é maior do que a largura real do dispositivo móvel. Isso permite que os navegadores móveis exibam toda a página da Web dentro da exibição virtual. A **marca meta viewport** permite que os desenvolvedores da Web definam a largura, a altura e a escala da área do navegador em dispositivos móveis **.** O modelo ASP.NET MVC 4 para aplicativos móveis define o visor para a largura do dispositivo (&quot;largura =&quot;largura do dispositivo) no modelo de layout (*Views\Shared\_layout. cshtml*), de modo que todas as páginas terão seu visor definido como a largura da tela do dispositivo. Observe que a marca meta viewport não alterará o modo de exibição de navegador padrão.
9. Abra **\_layout. cshtml**, localizado nas **exibições | Pasta compartilhada** e comente a marca meta do visor. Execute o aplicativo, se ainda não estiver aberto, e Confira as diferenças.

[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample2.cshtml)]

![O site após comentar a marca meta do visor](whats-new-in-aspnet-mvc-4/_static/image15.png "O site após comentar a marca meta do visor")

*O site após comentar a marca meta do visor*
10. No Visual Studio, pressione **SHIFT** + **F5** para parar a depuração do aplicativo.
11. Remova o comentário da marca meta do visor.

[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample3.cshtml)]

<a id="Task_3_-_Using_Adaptive_Rendering"></a>
#### <a name="task-3---using-adaptive-rendering"></a>Tarefa 3-usando a renderização adaptável

Nesta tarefa, você aprenderá outro método para renderizar uma página da Web corretamente em dispositivos móveis e navegadores da Web ao mesmo tempo sem qualquer personalização. Você já usou a marca meta viewport com uma finalidade semelhante. Agora, você atenderá a outro método poderoso: *renderização adaptável*.

A renderização adaptável é uma técnica que usa **consultas de mídia do CSS3** para personalizar o estilo aplicado a uma página. As consultas de mídia definem condições dentro de uma folha de estilos, agrupando estilos CSS em uma determinada condição. Somente quando a condição for verdadeira, o estilo será aplicado aos objetos declarados.

A flexibilidade fornecida pela técnica de renderização adaptável permite qualquer personalização para exibir o site em diferentes dispositivos. Você pode definir quantos estilos desejar em uma única folha de estilo sem escrever código lógico para escolher o estilo. Portanto, é uma maneira muito boa de adaptar os estilos de página, pois reduz a quantidade de código duplicado e a lógica para fins de processamento. Por outro lado, o consumo de largura de banda aumentaria, pois o tamanho dos arquivos CSS pode crescer de forma marginal.

Usando a técnica de renderização adaptável, seu site será **exibido corretamente, independentemente do navegador.** No entanto, você deve considerar se a carga extra de largura de banda é uma preocupação.

> [!NOTE]
> O formato básico de uma consulta de mídia é: @media \[escopo: todos | Handheld | imprimir | projeção |\] de tela ([Property: value] e... [Propriedade: valor])

Exemplos de consultas de mídia: &gt; **@media todos e (Max-width: 1000px) e (min-width: 700px) {}:** para todas as resoluções entre 700px e 1000px.

> **tela de@media e (min-width: 400px) e (Max-width: 700px) {...}:** Somente para telas. A resolução deve estar entre 400 e 700px.
> 
> **@media handheld e (min-width: 20Em), tela e (min-width: 20Em) {...}:** Para portáteis (dispositivos móveis e) e telas. A largura mínima deve ser maior que 20Em.
> 
> Você pode encontrar mais informações sobre isso no [site W3C](http://www.w3.org/TR/css3-mediaqueries/).

Agora você vai explorar como a renderização adaptável funciona, melhorando a legibilidade do modelo de site padrão do ASP.NET MVC 4.

1. Abra a solução do **Gallery. sln** que você criou na tarefa 1 e selecione o projeto do **Gallery** . Pressione **F5** para executar a solução.
2. Redimensione a largura do navegador, definindo o Windows para a metade ou para menos de um trimestre de seu tamanho original. Observe o que acontece com os itens no cabeçalho: alguns elementos não serão exibidos na área visível do cabeçalho.
3. Abra o arquivo **site. css** no Gerenciador de soluções do Visual Studio, localizado na pasta de projeto de **conteúdo** . Pressione **Ctrl + F** para abrir a pesquisa integrada do Visual Studio e gravar `@media` para localizar a **consulta de mídia do CSS**.

    A condição de consulta de mídia definida neste modelo funciona dessa forma: quando o tamanho da janela do navegador está abaixo de **850 PX**, as regras de CSS aplicadas são aquelas definidas dentro desse bloco de mídia.

    ![Localizando a consulta de mídia](whats-new-in-aspnet-mvc-4/_static/image16.png "Localizando a consulta de mídia")

    *Localizando a consulta de mídia*
4. Substitua o valor do atributo Max-Width definido em 850 px por **10px**, para desabilitar a renderização adaptável e pressione **Ctrl + S** para salvar as alterações. Retorne ao navegador e pressione **Ctrl + F5** para atualizar a página com as alterações feitas. Observe as diferenças em ambas as páginas ao ajustar a largura da janela.

    ![À esquerda, a página está aplicando o estilo de @media, à direita, o estilo é omitido](whats-new-in-aspnet-mvc-4/_static/image17.png "À esquerda, a página está aplicando o estilo de @media, à direita, o estilo é omitido")

    *À esquerda, a página está aplicando o estilo de @media, à direita, o estilo é omitido*

    Agora, vamos verificar o que acontece em dispositivos móveis:

    ![À esquerda, a página está aplicando o estilo de @media, à direita, o estilo é omitido](whats-new-in-aspnet-mvc-4/_static/image18.png "À esquerda, a página está aplicando o estilo de @media, à direita, o estilo é omitido")

    *À esquerda, a página está aplicando o estilo de @media, à direita, o estilo é omitido*

    Embora você perceba que as alterações quando a página é renderizada em um navegador da Web não são muito significativas, ao usar um dispositivo móvel, as diferenças se tornam mais óbvias. No lado esquerdo da imagem, podemos ver que o estilo personalizado melhorou a legibilidade.

    A renderização adaptável pode ser usada em muitos outros cenários, facilitando a aplicação de estilos condicionais a um site e resolvendo problemas comuns de estilo com uma abordagem organizada.

    A marca meta do visor e as consultas de mídia do CSS não são específicas do ASP.NET MVC 4, para que você possa aproveitar esses recursos em qualquer aplicativo Web.
5. No Visual Studio, pressione **SHIFT** + **F5** para parar a depuração do aplicativo.

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_the_Photo_Gallery_Web_Application"></a>
### <a name="exercise-2-creating-the-photo-gallery-web-application"></a>Exercício 2: criando o aplicativo Web da Galeria de fotos

Neste exercício, você trabalhará em um aplicativo da Galeria de fotos para exibir fotos. Você começará com o modelo de projeto ASP.NET MVC 4 e adicionará um recurso para recuperar fotos de um serviço e exibi-las no home page.

No exercício a seguir, você atualizará essa solução para aprimorar a maneira como ela é exibida em dispositivos móveis.

<a id="Task_1_-_Creating_a_Mock_Photo_Service"></a>
#### <a name="task-1---creating-a-mock-photo-service"></a>Tarefa 1-Criando um serviço de foto fictício

Nesta tarefa, você criará uma simulação do serviço de foto para recuperar o conteúdo que será exibido na galeria. Para fazer isso, você adicionará um novo controlador que simplesmente retornará um arquivo JSON com os dados de cada foto.

1. Abra o **Visual Studio** , se ainda não estiver aberto.
2. Selecione o **arquivo | Novo |** Comando de menu do projeto. Na caixa de diálogo **novo projeto** , selecione **o C# Visual | Modelo da Web** na árvore do painel esquerdo e escolha **aplicativo Web ASP.NET MVC 4.** Nomeie o projeto **Galeria**, selecione um local (ou deixe o padrão) e clique em **OK**. Como alternativa, você pode continuar trabalhando com sua solução de aplicativo de **Internet** existente do ASP.NET MVC 4 do **exercício 1** e ignorar a próxima etapa.
3. Na caixa de diálogo **novo projeto do ASP.NET MVC 4** , selecione o modelo de projeto de **aplicativo da Internet** e clique em **OK**. Verifique se você selecionou o Razor como o mecanismo de exibição.
4. Na **Gerenciador de soluções**, clique com o botão direito do mouse na pasta **\_dados do aplicativo** do seu projeto e selecione **Adicionar | Item existente**. Navegue até a pasta de **dados do Source\Assets\App\_** deste laboratório e adicione o arquivo **photos. JSON** .
5. Crie um novo controlador com o nome **metacontroller**. Para fazer isso, clique com o botão direito do mouse na pasta **controladores** , vá para **Adicionar** e selecione **controlador.** Complete o nome do controlador, deixe o modelo do **controlador MVC vazio** e clique em **Adicionar**.

    ![Adicionando o mocontroller](whats-new-in-aspnet-mvc-4/_static/image19.png "Adicionando o mocontroller")

    *Adicionando o mocontroller*
6. Substitua o método **index** pela seguinte ação da **Galeria** e retorne o conteúdo do arquivo JSON que você adicionou recentemente ao projeto.

    (Trecho de código- *ASP.NET MVC 4 Lab-Ex02-a ação da Galeria*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample4.cs)]
7. Pressione **F5** para executar a solução e, em seguida, navegue até a seguinte URL para testar o serviço de foto fictício: `http://localhost:[port]/photo/gallery` (o valor [Port] depende da porta atual na qual o aplicativo foi iniciado). A solicitação para essa URL deve recuperar o conteúdo do arquivo **photos. JSON** .

    ![Testando o serviço de foto fictício](whats-new-in-aspnet-mvc-4/_static/image20.png "Testando o serviço de foto fictício")

    *Testando o serviço de foto fictício*

Em uma implementação real, você pode usar [ASP.NET Web API](../../../../web-api/index.md) para implementar o serviço Galeria de fotos. ASP.NET Web API é uma estrutura que facilita a criação de serviços HTTP que atingem uma ampla variedade de clientes, incluindo navegadores e dispositivos móveis. O ASP.NET Web API é uma plataforma ideal para o desenvolvimento de aplicativos RESTful no .NET Framework.

<a id="Task_2_-_Displaying_the_Photo_Gallery"></a>
#### <a name="task-2---displaying-the-photo-gallery"></a>Tarefa 2 – exibindo a Galeria de fotos

Nesta tarefa, você atualizará a Home Page para mostrar a Galeria de fotos usando o serviço fictício criado na primeira tarefa deste exercício. Você adicionará arquivos de modelo e atualizará as exibições da galeria.

1. No Visual Studio, pressione **SHIFT** + **F5** para parar a depuração do aplicativo.
2. Crie a classe **Photo** na pasta **modelos** . Para fazer isso, clique com o botão direito do mouse na pasta **modelos** , selecione **Adicionar** e clique em **classe**. Em seguida, defina o nome como **Photo.cs** e clique em **Adicionar**.
3. Adicione os membros a seguir à classe **Photo** .

    (Trecho de código- *ASP.NET MVC 4 Lab-Ex02-modelo de foto*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample5.cs)]
4. Abra o arquivo **HomeController.cs** da pasta **Controladores**.
5. Adicione as seguintes instruções using.

    (Trecho de código- *ASP.NET MVC 4 Lab-Ex02-HomeController usa*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample6.cs)]
6. Atualize a ação de **índice** para usar o **HttpClient** para recuperar os dados da galeria e, em seguida, use o **JavaScriptSerializer** para desserializá-lo para o modelo de exibição.

    (Trecho de código- *ASP.NET MVC 4 Lab-Ex02-action-index*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample7.cs)]
7. Abra o arquivo **index. cshtml** localizado na pasta **views\home** e substitua todo o conteúdo pelo código a seguir.

    Esse código percorre todas as fotos recuperadas do serviço e as exibe em uma lista não ordenada.

    (Trecho de código- *ASP.NET MVC 4 Lab-Ex02-lista de fotos*)

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample8.cshtml)]
8. Na **Gerenciador de soluções**, clique com o botão direito do mouse na pasta de **conteúdo** do seu projeto e selecione **Adicionar | Item existente**. Navegue até a pasta **Source\Assets\Content** deste laboratório e adicione o arquivo **site. css** . Você precisará confirmar sua substituição. Se você tiver o arquivo **site. css** aberto, precisará confirmar para recarregar o arquivo também.
9. Abra o explorador de arquivos e copie a pasta **fotos** inteira localizada na pasta **Source\Assets** deste laboratório para a pasta raiz do seu projeto no Gerenciador de soluções.
10. Execute o aplicativo. Agora você deve ver o home page exibindo as fotos na galeria.

    ![Galeria de fotos](whats-new-in-aspnet-mvc-4/_static/image21.png "Galeria de fotos")

    *Galeria de fotos*
11. No Visual Studio, pressione **SHIFT** + **F5** para parar a depuração do aplicativo.

<a id="Exercise3"></a>

<a id="Exercise_3_Adding_support_for_mobile_devices"></a>
### <a name="exercise-3-adding-support-for-mobile-devices"></a>Exercício 3: adicionando suporte para dispositivos móveis

Uma das principais atualizações no ASP.NET MVC 4 é o suporte para desenvolvimento móvel. Neste exercício, você vai explorar os novos recursos do ASP.NET MVC 4 para aplicativos móveis, estendendo a solução da Galeria de galerias criada no exercício anterior.

<a id="Task_1_-_Installing_jQuery_Mobile_in_an_ASPNET_MVC_4_Application"></a>
#### <a name="task-1---installing-jquery-mobile-in-an-aspnet-mvc-4-application"></a>Tarefa 1-Instalando o jQuery Mobile em um aplicativo ASP.NET MVC 4

1. Abra a solução **inicial** localizada na **origem/EX3-MobileSupport/início/** pasta. Caso contrário, você pode continuar usando a solução **final** obtida concluindo o exercício anterior.

   1. Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar. Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.
   2. Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, Compile a solução clicando em **build** | **Compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto. É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.
2. Abra o **console do Gerenciador de pacotes** clicando na opção de menu **ferramentas** > **Gerenciador de pacotes NuGet** > **console do Gerenciador de pacotes** .

    ![Abrindo o console do Gerenciador de pacotes NuGet](whats-new-in-aspnet-mvc-4/_static/image22.png "Abrindo o console do Gerenciador de pacotes NuGet")

    *Abrindo o console do Gerenciador de pacotes NuGet*
3. No console do Gerenciador de pacotes, execute o seguinte comando para instalar o pacote **jQuery. Mobile. Mvc** .

    o jQuery Mobile é uma biblioteca de software livre para a criação de interface do usuário da Web otimizada para toque. O pacote NuGet jQuery. Mobile. MVC inclui auxiliares para usar o jQuery Mobile com um aplicativo ASP.NET MVC 4.

    > [!NOTE]
    > Ao executar o comando a seguir, você baixará a biblioteca jQuery. Mobile. MVC do NuGet.

    PM

    [!code-powershell[Main](whats-new-in-aspnet-mvc-4/samples/sample9.ps1)]

    Esse comando instala o jQuery Mobile e alguns arquivos auxiliares, incluindo o seguinte:

    - **Views/Shared/\_layout. Mobile. cshtml**: é um layout baseado em Mobile do jQuery otimizado para uma tela menor. Quando o site receber uma solicitação de um navegador móvel, ele substituirá o layout original (\_layout. cshtml) por este.
    - Um componente de seletor de exibição: consiste na exibição parcial views **/Shared/\_ViewSwitcher. cshtml** e no controlador **ViewSwitcherController.cs** . Este componente mostrará um link em navegadores móveis para permitir que os usuários alternem para a versão de área de trabalho da página.  
        ![Projeto da Galeria de fotos com suporte móvel](whats-new-in-aspnet-mvc-4/_static/image23.png "Phprojeto da galeria do cronização com suporte móvel ")

        *Projeto da Galeria de fotos com suporte móvel*
4. Registre os pacotes móveis. Para fazer isso, abra o arquivo **global.asax.cs** e adicione a linha a seguir.

    (Trecho de código- *ASP.NET MVC 4 Lab-Ex03-registrar pacotes móveis*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample10.cs)]
5. Execute o aplicativo usando um navegador da Web da área de trabalho.
6. Abra o **emulador Windows Phone 7,** localizado no **menu iniciar | Todos os programas | SDK do Windows Phone 7,1 | Emulador de Windows Phone.**
7. Na tela iniciar telefone, abra o Internet Explorer. Confira a URL em que o aplicativo foi iniciado e navegue até essa URL com o navegador de telefone (por exemplo, `http://localhost:[PortNumber]/`).

    Você observará que seu aplicativo terá uma aparência diferente no emulador de Windows Phone, já que o jQuery. Mobile. MVC criou novos ativos em seu projeto que mostram exibições otimizadas para dispositivos móveis.

    Observe a mensagem na parte superior do telefone, mostrando o link que alterna para a exibição da área de trabalho. Além disso, o layout do **\_layout. Mobile. cshtml** criado pelo pacote que você instalou está incluindo um layout diferente no aplicativo.

    > [!NOTE]
    > Até agora, não há nenhum link para voltar ao modo de exibição móvel. Ele será incluído em versões posteriores.

    ![Exibição móvel da Home Page da Galeria de fotos](whats-new-in-aspnet-mvc-4/_static/image24.png "Exibição móvel da Home Page da Galeria de fotos")

    *Exibição móvel da Home Page da Galeria de fotos*
8. No Visual Studio, pressione **SHIFT** + **F5** para parar a depuração do aplicativo.

<a id="Task_2_-_Creating_Mobile_Views"></a>
#### <a name="task-2---creating-mobile-views"></a>Tarefa 2-Criando exibições móveis

Nesta tarefa, você criará uma versão móvel do modo de exibição de índice com conteúdo adaptado para melhor aparência em dispositivos móveis.

1. Copie a exibição **Views\Home\Index.cshtml** e cole-a para criar uma cópia, renomeie o novo arquivo para **index. Mobile. cshtml**.
2. Abra a nova exibição **index. Mobile. cshtml** criada e substitua a marca &lt;UL&gt; existente por esse código. Ao fazer isso, você atualizará a marca &lt;UL&gt; com as anotações de dados móveis do jQuery para usar os temas móveis do jQuery.

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample11.html)]

    > [!NOTE] 
    > 
    > Observe que:
    > 
    > - O atributo de **função de dados** definido como **ListView** renderizará a lista usando os estilos de ListView.
    > 
    > - O atributo de **inserção de dados** definido como true mostrará a lista com borda arredondada e margem.
    > 
    > - O atributo de **filtro de dados** definido como **true** irá gerar uma caixa de pesquisa.
    > 
    > Você pode saber mais sobre as convenções móveis do jQuery na documentação do projeto: [ [http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)
3. Pressione **Ctrl + S** para salvar as alterações.
4. Alterne para o **emulador de Windows Phone** e atualize o site. Observe a nova aparência da lista da galeria, bem como a nova caixa de pesquisa localizada na parte superior. Em seguida, digite uma palavra na caixa de pesquisa (por exemplo, **tulips**) para iniciar uma pesquisa na Galeria de fotos.

    ![Galeria usando estilo ListView com filtragem](whats-new-in-aspnet-mvc-4/_static/image25.png "Galeria usando estilo ListView com filtragem")

    *Galeria usando estilo ListView com filtragem*

    Para resumir, você usou a receita exibir mobilizador para criar uma cópia da exibição de índice com o sufixo &quot;&quot; móvel. Esse sufixo indica ao ASP.NET MVC 4 que cada solicitação gerada de um dispositivo móvel usará essa cópia do índice. Além disso, você atualizou a versão móvel da exibição de índice para usar o jQuery Mobile para aprimorar a aparência do site em dispositivos móveis.
5. Volte para o Visual Studio e abra **site. Mobile. css** localizado na pasta **conteúdo** .
6. Corrija o posicionamento do título da foto para que ele seja exibido no lado direito da imagem. Para fazer isso, adicione o código a seguir ao arquivo **site. Mobile. css** .

    CSS

    [!code-css[Main](whats-new-in-aspnet-mvc-4/samples/sample12.css)]
7. Pressione **Ctrl + S** para salvar as alterações.
8. Volte para o **emulador de Windows Phone** e atualize o site. Observe que o título da foto está posicionado corretamente agora.

    ![Título posicionado no lado direito da imagem](whats-new-in-aspnet-mvc-4/_static/image26.png "Título posicionado no lado direito da imagem")

    *Título posicionado no lado direito da imagem*

<a id="Task_3_-_jQuery_Mobile_Themes"></a>
#### <a name="task-3---jquery-mobile-themes"></a>Tarefa 3-temas móveis do jQuery

Todos os layouts e widgets no jQuery Mobile são projetados em uma nova estrutura CSS orientada a objeto que possibilita a aplicação de um tema de Design Visual unificado completo a sites e aplicativos.

o tema padrão do jQuery Mobile inclui 5 amostras que recebem letras (a, b, c, d, e) para referência rápida.

Nesta tarefa, você atualizará o layout móvel para usar um tema diferente do padrão.

1. Volte para o Visual Studio.
2. Abra o arquivo **layout de\_. Mobile. cshtml** localizado em **Views\Shared**.
3. Localize o elemento div com a função de dados definida como &quot;página&quot; e atualize o **tema de dados** para &quot;**e**&quot;.

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample13.html)]
4. Pressione **Ctrl + S** para salvar as alterações.
5. Atualize o site no **emulador de Windows Phone** e observe o novo esquema de cores.

    ![Layout móvel com um esquema de cores diferente](whats-new-in-aspnet-mvc-4/_static/image27.png "Layout móvel com um esquema de cores diferente")

    *Layout móvel com um esquema de cores diferente*

<a id="Task_4_-_Using_the_View-Switcher_Component_and_the_Browser_Overriding_Features"></a>
#### <a name="task-4---using-the-view-switcher-component-and-the-browser-overriding-features"></a>Tarefa 4-usando o componente de seletor de exibição e os recursos de substituição do navegador

Uma Convenção para páginas da Web otimizadas para dispositivos móveis é adicionar um link cujo texto seja algo como exibição de área de trabalho ou modo de site completo que permite aos usuários alternar para uma versão de área de trabalho da página. O pacote jQuery. Mobile. MVC inclui um exemplo de componente de **seletor de exibição** para essa finalidade usada no modo de exibição **\_layout. Mobile. cshtml** .

![Link para alternar para o modo de exibição de desktop](whats-new-in-aspnet-mvc-4/_static/image28.png "Link para alternar para o modo de exibição de desktop")

*Link para alternar para o modo de exibição de desktop*

O alternador de exibição usa um novo recurso chamado **navegador substituindo**. Esse recurso permite que seu aplicativo trate as solicitações como se estivessem provenientes de um navegador diferente (agente do usuário) do que aquela da qual eles estão realmente vindo.

Nesta tarefa, você irá explorar a implementação de exemplo de um seletor de exibição adicionado por jQuery. Mobile. MVC e o novo navegador que substitui recursos no ASP.NET MVC 4.

1. Volte para o Visual Studio.
2. Abra a exibição **\_layout. Mobile. cshtml** localizada na pasta **Views\Shared** e observe o componente de seletor de exibição que está sendo referenciado como uma exibição parcial.

    ![Layout móvel usando o componente de seletor de exibição](whats-new-in-aspnet-mvc-4/_static/image29.png "Layout móvel usando o componente de seletor de exibição")

    *Layout móvel usando o componente de seletor de exibição*
3. Abra a exibição parcial **\_ViewSwitcher. cshtml** .

    A exibição parcial usa o novo método **ViewContext. HttpContext. GetOverriddenBrowser ()** para determinar a origem da solicitação da Web e mostrar o link correspondente para alternar para as exibições de desktop ou móvel.

    O método **GetOverriddenBrowser** retorna uma instância de **HttpBrowserCapabilitiesBase** que corresponde ao agente do usuário definido atualmente para a solicitação (real ou substituído). Você pode usar esse valor para obter propriedades como **IsMobileDevice**.

    ![Exibição parcial de ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image30.png "Exibição parcial de ViewSwitcher")

    *Exibição parcial de ViewSwitcher*
4. Abra a classe **ViewSwitcherController.cs** localizada na pasta **controladores** . Confira se a ação SwitchView é chamada pelo link no componente ViewSwitcher e observe os novos métodos HttpContext.

    - O método **HttpContext. ClearOverriddenBrowser ()** remove qualquer agente de usuário substituído da solicitação atual.
    - O método **HttpContext. SetOverriddenBrowser ()** substitui o valor real do agente do usuário da solicitação usando o agente do usuário especificado.  
        ![Controlador ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image31.png "ViControlador ewSwitcher ")  
*Controlador ViewSwitcher*

        A substituição do navegador é um recurso fundamental do ASP.NET MVC 4, que também está disponível mesmo se você não instalar o pacote jQuery. Mobile. MVC. No entanto, esse recurso afeta apenas exibição, layout e exibição parcial e não afeta nenhum dos recursos que dependem do objeto Request. browser.

<a id="Task_5_-_Adding_the_View-Switcher_in_the_Desktop_View"></a>
#### <a name="task-5---adding-the-view-switcher-in-the-desktop-view"></a>Tarefa 5-adicionando o seletor de exibição no modo de exibição de área de trabalho

Nesta tarefa, você atualizará o layout da área de trabalho para incluir o seletor de exibição. Isso permitirá que os usuários móveis voltem ao modo de exibição móvel ao navegar na exibição da área de trabalho.

1. Atualize o site no **emulador de Windows Phone**.
2. Clique no link **exibição da área de trabalho** na parte superior da galeria. Observe que não há nenhum seletor de exibição no modo de exibição de área de trabalho para permitir que você retorne ao modo de exibição móvel.
3. Volte para o Visual Studio e abra a exibição **\_layout. cshtml** .
4. Localize a seção de logon e insira uma chamada para renderizar a exibição parcial de **\_ViewSwitcher** abaixo da exibição parcial **\_LogOnPartial** . Em seguida, pressione **Ctrl + S** para salvar as alterações.

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample14.cshtml)]
5. Pressione **Ctrl + S** para salvar as alterações.
6. Atualize a página no emulador de Windows Phone e clique duas vezes na tela para ampliar. Observe que o home page agora mostra o link do **modo de exibição móvel** que alterna da exibição móvel para a área de trabalho.

    ![Exibir seletor renderizado na exibição de área de trabalho](whats-new-in-aspnet-mvc-4/_static/image32.png "Exibir seletor renderizado na exibição de área de trabalho")

    *Exibir seletor renderizado na exibição de área de trabalho*
7. Alterne para o modo de exibição móvel novamente e navegue até a página **sobre** (http://localhost [porta]/Home/about). Observe que, mesmo que você não tenha criado uma exibição about. Mobile. cshtml, a página About é exibida usando o layout móvel (\_layout. Mobile. cshtml).

    ![Página sobre](whats-new-in-aspnet-mvc-4/_static/image33.png "Página Sobre")

    *Página sobre*
8. Por fim, abra o site em um navegador da Web da área de trabalho. Observe que nenhuma das atualizações anteriores afetou a exibição da área de trabalho.

    ![Exibição de área de trabalho do Gallery](whats-new-in-aspnet-mvc-4/_static/image34.png "Exibição de área de trabalho do Gallery")

    *Exibição de área de trabalho do Gallery*

<a id="Task_6_-_Creating_New_Display_Modes"></a>
#### <a name="task-6---creating-new-display-modes"></a>Tarefa 6-criando novos modos de exibição

O novo recurso de modos de exibição permite que um aplicativo selecione exibições dependendo do navegador que está gerando a solicitação. Por exemplo, se um navegador da área de trabalho solicitar a página inicial, o aplicativo retornará o modelo **Views\Home\Index.cshtml** . Em seguida, se um navegador móvel solicitar a página inicial, o aplicativo retornará o modelo **Views\Home\Index.Mobile.cshtml** .

Nesta tarefa, você criará um layout personalizado para dispositivos iPhone e terá que simular solicitações de dispositivos iPhone. Para fazer isso, você pode usar um emulador/simulador do iPhone (como um [simulador móvel elétrico](http://www.electricplum.com/)) ou um navegador com complementos que modificam o agente do usuário. Para obter instruções sobre como definir a cadeia de caracteres do agente do usuário em um navegador Safari para emular um iPhone, consulte [como deixar o Safari fingir ser o IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) no blog de David Alison.

**Observe que essa tarefa é opcional e você pode continuar em todo o laboratório sem executá-la.**

1. No Visual Studio, pressione **SHIFT** + **F5** para parar a depuração do aplicativo.
2. Abra **global.asax.cs** e adicione a instrução using a seguir.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample15.cs)]
3. Adicione o seguinte código realçado ao método Start do aplicativo\_.

    (Trecho de código- *ASP.NET MVC 4 Lab-Ex03-iPhone DisplayMode*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample16.cs)]

Você registrou um novo **Defaultdisplaymode chamado &quot;iPhone&quot;** , na lista estática **displaymodeprovider. Instance. modes** estática, que será correspondido em cada solicitação de entrada. Se a solicitação de entrada contiver a cadeia de caracteres &quot;iPhone&quot;, o ASP.NET MVC encontrará as exibições cujo nome contém o sufixo de&quot; do iPhone &quot;. O parâmetro 0 indica quão específico é o novo modo; por exemplo, essa exibição é mais específica do que a regra geral &quot;. Mobile&quot; que corresponde a solicitações de dispositivos móveis.

Depois que esse código for executado, quando um navegador do iPhone gerar uma solicitação, seu aplicativo usará o layout **Views\Shared\\_Layout. iPhone. cshtml** que será criado nas próximas etapas.

> [!NOTE]
> Essa forma de testar a solicitação de iPhone foi simplificada para fins de demonstração e pode não funcionar conforme o esperado para cada cadeia de caracteres do agente do usuário do iPhone (por exemplo, o teste diferencia maiúsculas de minúsculas).

4. Crie uma cópia do arquivo **layout de\_. Mobile. cshtml** na pasta **Views\Shared** e renomeie a cópia para &quot; **\_Layout. iPhone. cshtml**&quot;.
5. Abra **\_layout. iPhone. cshtml** criado na etapa anterior.
6. Localize o elemento div com o atributo de função de dados definido como **página** e altere o atributo **Data-Theme** para &quot;**um**&quot;.

[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample17.cshtml)]

Agora você tem três layouts em seu aplicativo ASP.NET MVC 4:

1. **\_layout. cshtml**: layout padrão usado para navegadores da área de trabalho.
2. **\_layout. Mobile. cshtml**: layout padrão usado para dispositivos móveis.
3. **\_layout. iPhone. cshtml**: layout específico para dispositivos iPhone, usando um esquema de cores diferente para diferenciar de \_layout. Mobile. cshtml.
7. Pressione **F5** para executar o aplicativo e procurar o site no **emulador de Windows Phone**.
8. Abra um **simulador de iPhone** (consulte o [Apêndice C](#AppendixC) para obter instruções sobre como instalar e configurar um simulador de iPhone) e navegue até o site também. Observe que cada telefone está usando o modelo específico.

    ![Usando-modos de exibição diferentes-para-cada-móvel-dispositivo2](whats-new-in-aspnet-mvc-4/_static/image35.png)

    *Usando diferentes modos de exibição para cada dispositivo móvel*

<a id="Exercise4"></a>

<a id="Exercise_4_Using_Asynchronous_Controllers"></a>
### <a name="exercise-4-using-asynchronous-controllers"></a>Exercício 4: usando controladores assíncronos

O Microsoft .NET Framework 4,5 apresenta novos recursos de C# linguagem no e Visual Basic para fornecer uma nova base para a assincronia na programação .net. Essa nova base torna a programação assíncrona semelhante à de programação tão simples quanto a síncrona. Agora você pode escrever métodos de ação assíncrona no ASP.NET MVC 4 usando a classe **AsyncController** . Você pode usar métodos de ação assíncronas para solicitações de longa execução e não de CPU. Isso evita que o bloqueio do servidor Web execute o trabalho enquanto a solicitação está sendo processada. A classe AsyncController normalmente é usada para chamadas de serviço Web de longa execução.

Este exercício explica os conceitos básicos da operação assíncrona no ASP.NET MVC 4. Se você quiser uma análise mais profunda, poderá conferir o seguinte artigo: [ [https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)

<a id="Task_1_-_Implementing_an_Asynchronous_Controller"></a>
#### <a name="task-1---implementing-an-asynchronous-controller"></a>Tarefa 1-implementando um controlador assíncrono

1. Abra a solução **inicial** localizada em **origem/Ex4-Async/Begin/** Folder. Caso contrário, você pode continuar usando a solução **final** obtida concluindo o exercício anterior.

   1. Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar. Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.
   2. Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, Compile a solução clicando em **build** | **Compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto. É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.
2. Abra a classe **HomeController.cs** na pasta **controladores** .
3. Adicione a seguinte instrução using.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample18.cs)]
4. Atualize a classe **HomeController** para herdar de **AsyncController**. Os controladores que derivam de AsyncController permitem ASP.NET processar solicitações assíncronas e ainda podem atender a métodos de ação síncronas.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample19.cs)]
5. Adicione a palavra-chave **Async** ao método **index** e faça com que ela retorne o tipo **Task&lt;ActionResult&gt;** .

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample20.cs)]

    > [!NOTE]
    > A palavra-chave **Async** é uma das novas palavras-chave que o .NET Framework 4,5 fornece; Ele informa ao compilador que esse método contém código assíncrono. Um objeto de **tarefa** representa uma operação assíncrona que pode ser concluída em algum momento no futuro.
6. Substitua o **cliente. Chamada getasync ()** com a versão assíncrona completa usando Await palavra-chave, conforme mostrado abaixo.

    (Trecho de código- *ASP.NET MVC 4 Lab-Ex04-getasync*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample21.cs)]

    > [!NOTE]
    > Na versão anterior, você estava usando a propriedade **Result** do objeto **Task** para bloquear o thread até que o resultado seja retornado (versão de sincronização).
    > 
    > Adicionar a palavra-chave **Await** informa ao compilador para aguardar de forma assíncrona a tarefa retornada da chamada de método. Isso significa que o restante do código será executado como um retorno de chamada somente depois que o método esperado for concluído. Outra coisa a ser observada é que você não precisa alterar seu bloco try-catch para fazer isso funcionar: as exceções que acontecem em plano de fundo ou em primeiro plano ainda serão detectadas sem qualquer trabalho extra usando um manipulador fornecido pela estrutura.
7. Altere o código para continuar com a implementação assíncrona, substituindo as linhas pelo novo código, conforme mostrado abaixo

    (Trecho de código- *ASP.NET MVC 4 Lab-Ex04-ReadAsStringAsync*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample22.cs)]
8. Execute o aplicativo. Você não observará nenhuma alteração importante, mas seu código não bloqueará um thread do pool de threads que faz um melhor uso dos recursos do servidor e melhorando o desempenho.

    > [!NOTE]
    > Você pode aprender mais sobre os novos recursos de programação assíncrona no laboratório &quot;**programação assíncrona no .net C# 4,5 com e Visual Basic**&quot; incluídas no kit de treinamento do Visual Studio.

<a id="Task_2_-_Handling_Time-Outs_with_Cancellation_Tokens"></a>
#### <a name="task-2---handling-time-outs-with-cancellation-tokens"></a>Tarefa 2-manipulação de tempos limite com tokens de cancelamento

Os métodos de ação assíncronas que retornam instâncias de tarefa também podem dar suporte a tempos limite. Nesta tarefa, você atualizará o código do método de índice para manipular um cenário de tempo limite usando um token de cancelamento.

1. Volte para o Visual Studio e pressione **Shift + F5** para parar a depuração.
2. Adicione a seguinte instrução using ao arquivo **HomeController.cs** .

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample23.cs)]
3. Atualize a ação de índice para receber um argumento **CancellationToken** .

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample24.cs)]
4. Atualize a chamada **getasync** para passar o token de cancelamento.

    (Trecho de código- *ASP.NET MVC 4 Lab-Ex04-SendAsync com CancellationToken*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample25.cs)]
5. Decorar o método *index* com um atributo **AsyncTimeout** definido como 500 milissegundos e um atributo **HandleError** configurado para manipular **TaskCanceledException** redirecionando para uma exibição **TimedOut** .

    (Trecho de código- *ASP.NET MVC 4 Lab-Ex04-Attributes*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample26.cs)]
6. Abra a classe do **mocontroller** e atualize o método da **Galeria** para atrasar a execução de 1000 milissegundos (1 segundo) para simular uma tarefa de execução longa.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample27.cs)]
7. Abra o arquivo **Web. config** e habilite erros personalizados adicionando o seguinte elemento.

    [!code-xml[Main](whats-new-in-aspnet-mvc-4/samples/sample28.xml)]
8. Crie uma nova exibição em **Views\Shared** chamada **TimedOut** e use o layout padrão. Na Gerenciador de Soluções, clique com o botão direito do mouse na pasta **Views\Shared** e selecione **Adicionar | Exibição**.

    ![Usando diferentes modos de exibição para cada dispositivo móvel](whats-new-in-aspnet-mvc-4/_static/image36.png "Usando diferentes modos de exibição para cada dispositivo móvel")

    *Usando diferentes modos de exibição para cada dispositivo móvel*
9. Atualize o conteúdo da exibição **TimedOut** , conforme mostrado abaixo.

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample29.cshtml)]
10. Execute o aplicativo e navegue até a URL raiz. Como você adicionou um **thread. Sleep** de 1000 milissegundos, você receberá um erro de tempo limite gerado pelo atributo **AsyncTimeout** e pegará pelo atributo **HandleError** .

    ![Exceção de tempo limite manipulada](whats-new-in-aspnet-mvc-4/_static/image37.png "Exceção de tempo limite manipulada")

    *Exceção de tempo limite manipulada*

> [!NOTE]
> Além disso, você pode implantar esse aplicativo em sites do Windows Azure seguindo o [Apêndice D: publicando um aplicativo ASP.NET MVC 4 usando implantação da Web](#AppendixD).

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Resumo

Neste laboratório prático, você observou alguns dos novos recursos residentes no ASP.NET MVC 4. Os conceitos a seguir foram discutidos:

- Aproveite os aprimoramentos feitos nos modelos de projeto do ASP.NET MVC – incluindo o novo modelo de projeto de aplicativo móvel
- Use o atributo visor do HTML5 e as consultas de mídia do CSS para melhorar a exibição em dispositivos móveis
- Use o jQuery Mobile para aprimoramentos progressivos e para a criação de interface do usuário da Web otimizada para toque
- Criar exibições específicas para dispositivos móveis
- Use o componente de seletor de exibição para alternar entre exibições móveis e de área de trabalho no aplicativo
- Criar controladores assíncronos usando o suporte a tarefas

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>Apêndice A: usando trechos de código

Com trechos de código, você tem todo o código de que precisa ao seu alcance. O documento de laboratório informará exatamente quando você pode usá-los, conforme mostrado na figura a seguir.

![Usando trechos de código do Visual Studio para inserir código em seu projeto](whats-new-in-aspnet-mvc-4/_static/image38.png "Usando trechos de código do Visual Studio para inserir código em seu projeto")

*Usando trechos de código do Visual Studio para inserir código em seu projeto*

***Para adicionar um trecho de código usando o tecladoC# (somente)***

1. Coloque o cursor onde você deseja inserir o código.
2. Comece digitando o nome do trecho de código (sem espaços ou hifens).
3. Observe como o IntelliSense exibe nomes de trechos de código correspondentes.
4. Selecione o trecho correto (ou continue digitando até que o nome do trecho inteiro seja selecionado).
5. Pressione a tecla TAB duas vezes para inserir o trecho de código no local do cursor.

![Comece a digitar o nome do trecho](whats-new-in-aspnet-mvc-4/_static/image39.png "Comece a digitar o nome do trecho")

*Comece a digitar o nome do trecho*

![Pressione Tab para selecionar o trecho realçado](whats-new-in-aspnet-mvc-4/_static/image40.png "Pressione Tab para selecionar o trecho realçado")

*Pressione Tab para selecionar o trecho realçado*

![Pressione Tab novamente e o trecho será expandido](whats-new-in-aspnet-mvc-4/_static/image41.png "Pressione Tab novamente e o trecho será expandido")

*Pressione Tab novamente e o trecho será expandido*

***Para adicionar um trecho de código usando o mouseC#(, Visual Basic e XML)***

1. Clique com o botão direito do mouse no local em que você deseja inserir o trecho de código.
2. Selecione **Inserir trecho** seguido por **meus trechos de código**.
3. Selecione o trecho relevante na lista clicando nele.

![Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho](whats-new-in-aspnet-mvc-4/_static/image42.png "Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho")

*Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho*

![Selecione o trecho relevante na lista clicando nele](whats-new-in-aspnet-mvc-4/_static/image43.png "Selecione o trecho relevante na lista clicando nele")

*Selecione o trecho relevante na lista clicando nele*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>Apêndice B: Instalando o Visual Studio Express 2012 para Web

Você pode instalar o **Microsoft Visual Studio Express 2012 para Web** ou outra versão &quot;Express&quot; usando o **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** . As instruções a seguir orientam você pelas etapas necessárias para instalar o *Visual Studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.

1. Vá para [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Como alternativa, se você já tiver instalado o Web Platform Installer, poderá abri-lo e pesquisar o produto &quot;*Visual Studio Express 2012 para Web com o SDK do Windows Azure*&quot;.
2. Clique em **instalar agora**. Se você não tiver **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.
3. Quando **Web Platform Installer** estiver aberto, clique em **instalar** para iniciar a instalação.

    ![Instalar Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "Instalar Visual Studio Express")

    *Instalar Visual Studio Express*
4. Leia todos os termos e licenças de produtos e clique em **aceito** para continuar.

    ![Aceitando os termos de licença](whats-new-in-aspnet-mvc-4/_static/image45.png)

    *Aceitando os termos de licença*
5. Aguarde até que o processo de download e instalação seja concluído.

    ![Progresso da instalação](whats-new-in-aspnet-mvc-4/_static/image46.png)

    *Progresso da instalação*
6. Quando a instalação for concluída, clique em **concluir**.

    ![Instalação concluída](whats-new-in-aspnet-mvc-4/_static/image47.png)

    *Instalação concluída*
7. Clique em **sair** para fechar Web Platform Installer.
8. Para abrir o Visual Studio Express para Web, vá para a tela **Iniciar** e comece a escrever &quot;**vs Express**&quot;e, em seguida, clique no bloco **vs Express para Web** .

    ![Bloco VS Express para Web](whats-new-in-aspnet-mvc-4/_static/image48.png)

    *Bloco VS Express para Web*

<a id="AppendixC"></a>

<a id="Appendix_C_Installing_WebMatrix_2_and_iPhone_Simulator"></a>
## <a name="appendix-c-installing-webmatrix-2-and-iphone-simulator"></a>Apêndice C: Instalando o WebMatrix 2 e o simulador de iPhone

Para executar seu site em um dispositivo iPhone simulado, você pode usar a extensão do WebMatrix &quot;o simulador móvel elétrico para o&quot;de iPhone. Além disso, você pode configurar a mesma extensão para executar o simulador do Visual Studio 2012.

<a id="ApxCTask1"></a>

<a id="Task_1_-_Installing_WebMatrix_2"></a>
#### <a name="task-1---installing-webmatrix-2"></a>Tarefa 1-Instalando o WebMatrix 2

1. Vá para [[https://go.microsoft.com/?linkid=9809776](https://go.microsoft.com/?linkid=9809776)](https://go.microsoft.com/?linkid=9810169). Como alternativa, se você já tiver instalado o Web Platform Installer, poderá abri-lo e procurar o produto &quot;*WebMatrix 2*&quot;.
2. Clique em **instalar agora**. Se você não tiver **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.
3. Quando **Web Platform Installer** estiver aberto, clique em **instalar** para iniciar a instalação.

    ![Instalar o WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "Instalar o WebMatrix 2")

    *Instalar o WebMatrix 2*
4. Leia todos os termos e licenças de produtos e clique em **aceito** para continuar.

    ![Aceitando os termos de licença](whats-new-in-aspnet-mvc-4/_static/image50.png "Aceitando os termos de licença")

    *Aceitando os termos de licença*
5. Aguarde até que o processo de download e instalação seja concluído.

    ![Progresso da instalação](whats-new-in-aspnet-mvc-4/_static/image51.png "Progresso da instalação")

    *Progresso da instalação*
6. Quando a instalação for concluída, clique em **concluir**.

    ![Instalação concluída](whats-new-in-aspnet-mvc-4/_static/image52.png "Instalação concluída")

    *Instalação concluída*
7. Clique em **sair** para fechar Web Platform Installer.

<a id="ApxCTask2"></a>

<a id="Task_2_-_Installing_the_iPhone_Simulator_Extension"></a>
#### <a name="task-2---installing-the-iphone-simulator-extension"></a>Tarefa 2-instalando a extensão do simulador do iPhone

1. Execute o **WebMatrix** e abra qualquer site existente ou crie um novo.
2. Clique no botão **executar** na faixa de opções **página inicial** e selecione **Adicionar novo**.

    ![Adicionando nova extensão do WebMatrix](whats-new-in-aspnet-mvc-4/_static/image53.png "Adicionando nova extensão do WebMatrix")

    *Adicionando nova extensão do WebMatrix*
3. Selecione **simulador do iPhone** e clique em **instalar**.

    ![Navegando pelas extensões do WebMatrix](whats-new-in-aspnet-mvc-4/_static/image54.png "Navegando pelas extensões do WebMatrix")

    *Navegando pelas extensões do WebMatrix*
4. Nos detalhes do pacote, clique em **instalar** para continuar com a instalação da extensão.

    ![extensão do simulador do iPhone](whats-new-in-aspnet-mvc-4/_static/image55.png "extensão do simulador do iPhone")

    *extensão do simulador do iPhone*
5. Leia e aceite o EULA da extensão.

    ![EULA de extensão do WebMatrix](whats-new-in-aspnet-mvc-4/_static/image56.png "EULA de extensão do WebMatrix")

    *EULA de extensão do WebMatrix*
6. Agora, você pode executar seu site do WebMatrix usando a opção do simulador do iPhone.

    ![Executar usando iPhone](whats-new-in-aspnet-mvc-4/_static/image57.png "Executar usando iPhone")

    *Executar usando iPhone*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Configuring_Visual_Studio_2012_to_run_iPhone_Simulator"></a>
#### <a name="task-3---configuring-visual-studio-2012-to-run-iphone-simulator"></a>Tarefa 3-Configurando o Visual Studio 2012 para executar o iPhone Simulator

1. Abra o **Visual Studio 2012** e abra qualquer site da Web ou crie um novo projeto.
2. Clique na seta para baixo no botão Executar e selecione **procurar com**.

    ![Navegar com](whats-new-in-aspnet-mvc-4/_static/image58.png "Navegar com")

    *Navegar com*
3. Na caixa de diálogo &quot;navegar com&quot;, clique em **Adicionar**.
4. Na caixa de diálogo &quot;adicionar programa&quot;, use os seguintes valores:

   - **Programa**: C:\Users\*{CurrentUser} * \AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe *(Atualize o caminho de acordo)*
   - **Argumentos**: &quot;1&quot;
   - **Nome amigável**: simulador de iPhone

     ![Adicionar programa](whats-new-in-aspnet-mvc-4/_static/image59.png "Adicionar programa")

     *Adicionar programa para navegar com*
5. Clique em **OK** e feche as caixas de diálogo.
6. Agora você pode executar seus aplicativos Web no simulador de iPhone do Visual Studio 2012.

    ![Navegar com o simulador do iPhone](whats-new-in-aspnet-mvc-4/_static/image60.png "Navegar com o simulador do iPhone")

    *Navegar com o simulador do iPhone*

<a id="AppendixD"></a>

<a id="Appendix_D_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-d-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Apêndice D: publicando um aplicativo ASP.NET MVC 4 usando Implantação da Web

Este apêndice mostrará como criar um novo site da Web do Windows Azure Portal de Gerenciamento e publicar o aplicativo obtido seguindo o laboratório, aproveitando o recurso de publicação de Implantação da Web fornecido pelo Windows Azure.

<a id="ApxDTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Tarefa 1-Criando um novo site no portal do Windows Azure

1. Acesse o [portal de gerenciamento do Windows Azure](https://manage.windowsazure.com/) e entre usando as credenciais da Microsoft associadas à sua assinatura.

    > [!NOTE]
    > Com o Windows Azure, você pode hospedar 10 sites da Web de ASP.NET gratuitamente e, em seguida, dimensionar conforme o tráfego cresce. Você pode se inscrever [aqui](https://aka.ms/aspnet-hol-azure).

    ![Fazer logon no Windows portal do Azure](whats-new-in-aspnet-mvc-4/_static/image61.png "Fazer logon no Windows portal do Azure")

    *Fazer logon no Windows Azure Portal de Gerenciamento*
2. Clique em **novo** na barra de comandos.

    ![Criando um novo site](whats-new-in-aspnet-mvc-4/_static/image62.png "Criando um novo site")

    *Criando um novo site*
3. Clique em **computação** | **site**. Em seguida, selecione a opção **criação rápida** . Forneça uma URL disponível para o novo site e clique em **criar site**.

    > [!NOTE]
    > Um site do Windows Azure é o host de um aplicativo Web em execução na nuvem que você pode controlar e gerenciar. A opção criação rápida permite que você implante um aplicativo Web concluído no site do Windows Azure de fora do Portal. Ele não inclui etapas para configurar um banco de dados.

    ![Criando um novo site usando a criação rápida](whats-new-in-aspnet-mvc-4/_static/image63.png "Criando um novo site usando a criação rápida")

    *Criando um novo site usando a criação rápida*
4. Aguarde até que o novo **site** seja criado.
5. Depois que o site for criado, clique no link sob a coluna **URL** . Verifique se o novo site está funcionando.

    ![Navegando até o novo site](whats-new-in-aspnet-mvc-4/_static/image64.png "Navegando até o novo site")

    *Navegando até o novo site*

    ![Site em execução](whats-new-in-aspnet-mvc-4/_static/image65.png "Site em execução")

    *Site em execução*
6. Volte para o portal e clique no nome do site na coluna **nome** para exibir as páginas de gerenciamento.

    ![Abrindo as páginas de gerenciamento de site](whats-new-in-aspnet-mvc-4/_static/image66.png "Abrindo as páginas de gerenciamento de site")

    *Abrindo as páginas de gerenciamento de site*
7. Na página **painel** , na seção **visão rápida** , clique no link **baixar perfil de publicação** .

    > [!NOTE]
    > O *perfil de publicação* contém todas as informações necessárias para publicar um aplicativo Web em um site do Windows Azure para cada método de publicação habilitado. O perfil de publicação contém as URLs, as credenciais de usuário e as cadeias de caracteres de banco de dados necessárias para se conectar e autenticar em cada um dos pontos de extremidade para os quais um método de publicação está habilitado. **O Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** e **Microsoft Visual Studio 2012** dão suporte à leitura de perfis de publicação para automatizar a configuração desses programas para a publicação de aplicativos Web em sites do Windows Azure.

    ![Baixando o perfil de publicação do site](whats-new-in-aspnet-mvc-4/_static/image67.png "Baixando o perfil de publicação do site")

    *Baixando o perfil de publicação do site*
8. Baixe o arquivo de perfil de publicação em um local conhecido. Neste exercício, você verá como usar esse arquivo para publicar um aplicativo Web em um site do Windows Azure a partir do Visual Studio.

    ![Salvando o arquivo de perfil de publicação](whats-new-in-aspnet-mvc-4/_static/image68.png "Salvando o perfil de publicação")

    *Salvando o arquivo de perfil de publicação*

<a id="ApxDTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tarefa 2-Configurando o servidor de banco de dados

Se seu aplicativo utiliza bancos de dados SQL Server, você precisará criar um servidor de banco de dados SQL. Se você quiser implantar um aplicativo simples que não usa SQL Server você pode ignorar essa tarefa.

1. Você precisará de um servidor do banco de dados SQL para armazenar o banco de dados do aplicativo. Você pode exibir os servidores do banco de dados SQL de sua assinatura no portal de gerenciamento do Windows Azure em bancos de dados **sql** | **servidores** | **painel do servidor**. Se você não tiver um servidor criado, poderá criar um usando o botão **Adicionar** na barra de comandos. Anote o nome do **servidor e a URL, o nome de logon e a senha do administrador**, pois você irá usá-los nas próximas tarefas. Não crie o banco de dados ainda, pois ele será criado em um estágio posterior.

    ![Painel do servidor do banco de dados SQL](whats-new-in-aspnet-mvc-4/_static/image69.png "Painel do servidor do banco de dados SQL")

    *Painel do servidor do banco de dados SQL*
2. Na próxima tarefa, você testará a conexão de banco de dados do Visual Studio, por esse motivo, será necessário incluir o endereço IP local na lista de **endereços IP permitidos**do servidor. Para fazer isso, clique em **Configurar**, selecione o endereço IP do **endereço IP do cliente atual** e cole-o nas caixas de texto endereço IP **inicial** e **endereço IP final** e clique no botão ![adicionar-cliente-IP-endereço-OK-botão](whats-new-in-aspnet-mvc-4/_static/image70.png).

    ![Adicionando endereço IP do cliente](whats-new-in-aspnet-mvc-4/_static/image71.png)

    *Adicionando endereço IP do cliente*
3. Depois que o **endereço IP do cliente** for adicionado à lista endereços IP permitidos, clique em **salvar** para confirmar as alterações.

    ![Confirmar alterações](whats-new-in-aspnet-mvc-4/_static/image72.png)

    *Confirmar alterações*

<a id="ApxDTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tarefa 3-publicando um aplicativo ASP.NET MVC 4 usando Implantação da Web

1. Volte para a solução ASP.NET MVC 4. Na **Gerenciador de soluções**, clique com o botão direito do mouse no projeto de site e selecione **publicar**.

    ![Publicando o aplicativo](whats-new-in-aspnet-mvc-4/_static/image73.png "Publicando o aplicativo")

    *Publicando o site*
2. Importe o perfil de publicação salvo na primeira tarefa.

    ![Importando o perfil de publicação](whats-new-in-aspnet-mvc-4/_static/image74.png "Importando o perfil de publicação")

    *Importando perfil de publicação*
3. Clique em **validar conexão**. Depois que a validação for concluída, clique em **Avançar**.

    > [!NOTE]
    > A validação será concluída quando você vir uma marca de seleção verde ao lado do botão Validar conexão.

    ![Validando conexão](whats-new-in-aspnet-mvc-4/_static/image75.png "Validando conexão")

    *Validando conexão*
4. Na página **configurações** , na seção **bancos** de dados, clique no botão ao lado da caixa de texto da sua conexão de banco (ou seja, **DefaultConnection**).

    ![Configuração de implantação da Web](whats-new-in-aspnet-mvc-4/_static/image76.png "Configuração de implantação da Web")

    *Configuração de implantação da Web*
5. Configure a conexão de banco de dados da seguinte maneira:

   - No **nome do servidor** , digite a URL do servidor do banco de dados SQL usando o prefixo *TCP:* .
   - Em **nome de usuário** , digite o nome de logon do administrador do servidor.
   - Em **senha** , digite a senha de logon do administrador do servidor.
   - Digite um novo nome de banco de dados, por exemplo: *MVC4SampleDB*.

     ![Configurando a cadeia de conexão de destino](whats-new-in-aspnet-mvc-4/_static/image77.png "Configurando a cadeia de conexão de destino")

     *Configurando a cadeia de conexão de destino*
6. Clique em **OK**. Quando for solicitado a criar o banco de dados, clique em **Sim**.

    ![Criando o banco de dados](whats-new-in-aspnet-mvc-4/_static/image78.png "Criando a cadeia de caracteres do banco de dados")

    *Criando o banco de dados*
7. A cadeia de conexão que você usará para se conectar ao banco de dados SQL no Windows Azure é mostrada na caixa de texto conexão padrão. Clique em **Avançar**.

    ![Cadeia de conexão apontando para o banco de dados SQL](whats-new-in-aspnet-mvc-4/_static/image79.png "Cadeia de conexão apontando para o banco de dados SQL")

    *Cadeia de conexão apontando para o banco de dados SQL*
8. Na página **Visualização** , clique em **publicar**.

    ![Publicando o aplicativo Web](whats-new-in-aspnet-mvc-4/_static/image80.png "Publicando o aplicativo Web")

    *Publicando o aplicativo Web*
9. Quando o processo de publicação for concluído, o navegador padrão abrirá o site publicado.

    ![Aplicativo publicado no Windows Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "Aplicativo publicado no Windows Azure")

    *Aplicativo publicado no Windows Azure*
