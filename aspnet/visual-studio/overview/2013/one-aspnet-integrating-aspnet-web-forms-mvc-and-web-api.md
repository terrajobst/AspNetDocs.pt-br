---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: 'Laboratório prático: um ASP.NET: integrando o ASP.NET Web Forms, MVC e API da Web | Microsoft Docs'
author: rick-anderson
description: O ASP.NET é uma estrutura para a criação de sites, aplicativos e serviços usando tecnologias especializadas, como MVC, API Web e outros. Com a expansão ASP.NET h...
ms.author: riande
ms.date: 07/16/2014
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: 165d104b5d3ef3281af449cc8673ad96f531d628
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78623196"
---
# <a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a>Laboratório prático: um ASP.NET: integrando o ASP.NET Web Forms, MVC e API da Web

por [equipe de acampamentos da Web](https://twitter.com/webcamps)

[Baixe o kit de treinamento do Web acampamentos](https://aka.ms/webcamps-training-kit)

> O ASP.NET é uma estrutura para a criação de sites, aplicativos e serviços usando tecnologias especializadas, como MVC, API Web e outros. Com o ASP.NET de expansão visto desde sua criação e a expressa necessidade de ter essas tecnologias integradas, houve esforços recentes em trabalhar em direção a **uma ASP.net**.
> 
> Visual Studio 2013 introduz um novo sistema de projeto unificado que permite criar um aplicativo e usar todas as tecnologias ASP.NET em um projeto. Esse recurso elimina a necessidade de escolher uma tecnologia no início de um projeto e aderir a ela e, em vez disso, incentiva o uso de várias estruturas ASP.NET dentro de um projeto.
> 
> Todos os códigos de exemplo e trechos de código estão incluídos no kit de treinamento do acampamentos da Web, disponível em [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit).

<a id="Overview"></a>
## <a name="overview"></a>Visão geral

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

Neste laboratório prático, você aprenderá a:

- Criar um site com base em um tipo de projeto **ASP.net**
- Usar estruturas **ASP.net** diferentes, como **MVC** e **API Web** , no mesmo projeto
- Identificar os principais componentes de um aplicativo **ASP.net**
- Aproveite a estrutura **ASP.net scaffolding** para criar automaticamente controladores e exibições para executar operações CRUD com base em suas classes de modelo
- Expor o mesmo conjunto de informações em formatos legíveis para computador e humano usando a ferramenta certa para cada trabalho

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisites

O seguinte é necessário para concluir este laboratório prático:

- [Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/) ou superior
- [Visual Studio 2013 Atualização 1](https://go.microsoft.com/fwlink/?LinkId=301714)

<a id="Setup"></a>
### <a name="setup"></a>Instalação

Para executar os exercícios neste laboratório prático, você precisará configurar seu ambiente primeiro...

1. Abra o Windows Explorer e navegue até a pasta de **origem** do laboratório.
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

1. [Criando um novo projeto de Web Forms](#Exercise1)
2. [Criando um controlador MVC usando scaffolding](#Exercise2)
3. [Criando um controlador de API Web usando scaffolding](#Exercise3)

Tempo estimado para concluir este laboratório: **60 minutos**

> [!NOTE]
> Ao iniciar o Visual Studio pela primeira vez, você deve selecionar uma das coleções de configurações predefinidas. Cada coleção predefinida é projetada para corresponder a um estilo de desenvolvimento específico e determina layouts de janela, comportamento do editor, trechos de código IntelliSense e opções da caixa de diálogo. Os procedimentos neste laboratório descrevem as ações necessárias para realizar uma determinada tarefa no Visual Studio ao usar a coleção de **configurações de desenvolvimento geral** . Se você escolher uma coleção de configurações diferentes para seu ambiente de desenvolvimento, poderá haver diferenças nas etapas que você deve levar em conta.

<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a>Exercício 1: Criando um novo projeto de Web Forms

Neste exercício, você criará um novo site Web Forms no Visual Studio 2013 usando a experiência de projeto unificada **ASP.net** , que permitirá que você integre facilmente os componentes Web Forms, MVC e API Web no mesmo aplicativo. Em seguida, você irá explorar a solução gerada e identificar suas partes e, por fim, verá o site em ação.

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a>Tarefa 1 – Criando um novo site usando uma experiência ASP.NET

Nesta tarefa, você começará a criar um novo site no Visual Studio com base em **um** tipo de projeto ASP.net. **Uma ASP.net** unifica todas as tecnologias de ASP.net e oferece a opção de misturar e CORRESP-las conforme desejado. Em seguida, você reconhecerá os diferentes componentes do Web Forms, MVC e API Web que residem lado a lado em seu aplicativo.

1. Abra **Visual Studio Express 2013 para Web** e selecione **arquivo | Novo projeto...** para iniciar uma nova solução.

    ![Criando um novo projeto](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    *Criando um novo projeto*
2. Na caixa de diálogo **novo projeto** , selecione **ASP.NET aplicativo Web** no **Visual C# |** Na guia Web e verifique se **.NET Framework 4,5** está selecionado. Nomeie o projeto *MyHybridSite*, escolha um **local** e clique em **OK**.

    ![Novo projeto de aplicativo Web ASP.NET](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    *Criando um novo projeto de aplicativo Web ASP.NET*
3. Na caixa de diálogo **novo projeto ASP.net** , selecione o modelo de **Web Forms** e selecione as opções **MVC** e **API da Web** . Além disso, verifique se a opção de **autenticação** está definida para **contas de usuário individuais**. Clique em **OK** para continuar.

    ![Criando um novo projeto com o modelo de Web Forms, incluindo componentes da API Web e MVC](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    *Criando um novo projeto com o modelo de Web Forms, incluindo componentes da API Web e MVC*
4. Agora você pode explorar a estrutura da solução gerada.

    ![Explorando a solução gerada](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    *Explorando a solução gerada*

    1. **Conta:** Essa pasta contém as páginas de formulário da Web para registrar, fazer logon e gerenciar as contas de usuário do aplicativo. Essa pasta é adicionada quando a opção de autenticação de **contas de usuário individuais** é selecionada durante a configuração do modelo de projeto Web Forms.
    2. **Modelos:** Essa pasta conterá as classes que representam os dados do aplicativo.
    3. **Controladores** e **exibições**: essas pastas são necessárias para os componentes **ASP.NET MVC** e **ASP.NET Web API** . Você explorará as tecnologias MVC e API da Web nos próximos exercícios.
    4. Os arquivos **Default. aspx**, **Contact. aspx** e **about. aspx** são páginas de formulário da Web predefinidas que você pode usar como pontos de partida para criar as páginas específicas ao seu aplicativo. A lógica de programação desses arquivos reside em um arquivo separado referido como o &quot;&quot; arquivo de código, que tem uma extensão &quot;. aspx. vb&quot; ou &quot;. aspx.cs&quot; (dependendo do idioma usado). A lógica code-behind é executada no servidor e produz dinamicamente a saída HTML para sua página.
    5. As páginas **site. Master** e **site. Mobile. Master** definem a aparência e o comportamento padrão de todas as páginas no aplicativo.
5. Clique duas vezes no arquivo **Default. aspx** para explorar o conteúdo da página.

    ![Explorando a página default. aspx](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    *Explorando a página default. aspx*

    > [!NOTE]
    > A diretiva **Page** na parte superior do arquivo define os atributos da página Web Forms. Por exemplo, o atributo **MasterPageFile** especifica o caminho para a página mestra – nesse caso, a página *site. Master* -e o atributo **Inherits** definem a classe code-behind da página a ser herdada. Essa classe está localizada no arquivo determinado pelo atributo **Codebehind** .
    > 
    > O controle **asp: content** contém o conteúdo real da página (texto, marcação e controles) e é mapeado para um controle **asp: ContentPlaceHolder** na página mestra. Nesse caso, o conteúdo da página será renderizado dentro do controle *mainContent* definido na página *site. Master* .
6. Expanda o **aplicativo\_pasta inicial** e observe o arquivo **WebApiConfig.cs** . O Visual Studio incluiu esse arquivo na solução gerada porque você incluiu a API da Web ao configurar seu projeto com o modelo One ASP.NET.
7. Abra o arquivo **WebApiConfig.cs** . Na classe *WebApiConfig* , você encontrará a configuração associada à API Web, que mapeia as rotas http para os **controladores da API Web**.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. Abra o arquivo **RouteConfig.cs** . Dentro do método *RegisterRoutes* , você encontrará a configuração associada ao MVC, que mapeia as rotas http para **controladores MVC**.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a>Tarefa 2 – executando a solução

Nesta tarefa, você executará a solução gerada, explorará o aplicativo e alguns de seus recursos, como a regravação de URL e a autenticação interna.

1. Para executar a solução, pressione **F5** ou clique no botão **Iniciar** localizado na barra de ferramentas. O home page do aplicativo deve ser aberto no navegador.

    ![Executando a solução](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. Verifique se as páginas Web Forms estão sendo invocadas. Para fazer isso, acrescente **/Contact.aspx** à URL na barra de endereços e pressione **Enter**.

    ![URLs amigáveis](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    *URLs amigáveis*

    > [!NOTE]
    > Como você pode ver, a URL muda para **/Contact**. A partir do **ASP.NET 4**, os recursos de roteamento de URL foram adicionados ao Web Forms, para que você possa gravar URLs como *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)* em vez de *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)* . Para obter mais informações, consulte [Roteamento de URL](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).
3. Agora, você explorará o fluxo de autenticação integrado ao aplicativo. Para fazer isso, clique em **registrar** no canto superior direito da página.

    ![Registrando um novo usuário](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    *Registrando um novo usuário*
4. Na página **registrar** , insira um **nome de usuário** e **senha**e, em seguida, clique em **registrar**.

    ![Página de registro](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    *Página de registro*
5. O aplicativo registra a nova conta e o usuário é autenticado.

    ![Usuário autenticado](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    *Usuário autenticado*
6. Volte para o Visual Studio e pressione **Shift + F5** para parar a depuração.

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a>Exercício 2: Criando um controlador MVC usando o scaffolding

Neste exercício, você aproveitará a estrutura ASP.NET scaffolding fornecida pelo Visual Studio para criar um controlador ASP.NET MVC 5 com ações e exibições do Razor para executar operações CRUD, sem escrever uma única linha de código. O processo scaffolding usará Entity Framework Code First para gerar o contexto de dados e o esquema de banco de dado no banco de dados SQL.

**Sobre Entity Framework Code First**

Entity Framework (EF) é um mapeador relacional de objeto (ORM) que permite que você crie aplicativos de acesso a dados programando com um modelo de aplicativo conceitual em vez de programar diretamente usando um esquema de armazenamento relacional.

O fluxo de trabalho de modelagem de Code First de Entity Framework permite que você use suas próprias classes de domínio para representar o modelo que o EF depende ao executar consultas, controle de alterações e atualização de funções. Usando o fluxo de trabalho de desenvolvimento Code First, você não precisa iniciar seu aplicativo Criando um banco de dados ou especificando um esquema. Em vez disso, você pode escrever classes .NET padrão que definem os objetos de modelo de domínio mais apropriados para seu aplicativo e Entity Framework criará o banco de dados para você.

> [!NOTE]
> Você pode saber mais sobre Entity Framework [aqui](../../../entity-framework.md).

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a>Tarefa 1 – Criando um novo modelo

Agora você vai definir uma classe **Person** , que será o modelo usado pelo processo scaffolding para criar o controlador MVC e as exibições. Você começará criando uma classe de modelo **Person** e as operações CRUD no controlador serão criadas automaticamente usando os recursos do scaffolding.

1. Abra **Visual Studio Express 2013 para Web** e a solução **MyHybridSite. sln** localizada na pasta **Source/EX2-MvcScaffolding/Begin** . Como alternativa, você pode continuar com a solução que você obteve no exercício anterior.
2. Em **Gerenciador de soluções**, clique com o botão direito do mouse na pasta **modelos** do projeto **MyHybridSite** e selecione **Adicionar | Classe...** .

    ![Adicionando a classe de modelo Person](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    *Adicionando a classe de modelo Person*
3. Na caixa de diálogo **Adicionar novo item** , nomeie o arquivo *Person.cs* e clique em **Adicionar**.

    ![Criando a classe de modelo Person](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    *Criando a classe de modelo Person*
4. Substitua o conteúdo do arquivo **Person.cs** pelo código a seguir. Pressione **Ctrl + S** para salvar as alterações.

    (Trecho de código- *BringingTogetherOneAspNet-EX2-PersonClass*)

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto **MyHybridSite** e selecione **COMPILAR**ou pressione **Ctrl + Shift + B** para compilar o projeto.

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a>Tarefa 2 – Criando um controlador MVC

Agora que o modelo **Person** foi criado, você usará o ASP.NET MVC scaffolding com Entity Framework para criar as ações do controlador CRUD e exibições para **Person**.

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse na pasta **controladores** do projeto **MyHybridSite** e selecione **Adicionar | Novo item de com Scaffold...**

    ![Criando um novo controlador com Scaffold](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    *Criando um novo controlador com Scaffold*
2. Na caixa de diálogo **Adicionar Scaffold** , selecione **controlador MVC 5 com exibições, usando Entity Framework** e clique em **Adicionar.**

    ![Selecionando o controlador MVC 5 com exibições e Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    *Selecionando o controlador MVC 5 com exibições e Entity Framework*
3. Defina *MvcPersonController* como o **nome do controlador**, selecione a opção **usar ações do controlador assíncrono** e selecione **pessoa (MyHybridSite. Models)** como a **classe do modelo**.

    ![Adicionando um controlador MVC com scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    *Adicionando um controlador MVC com scaffolding*
4. Em **classe de contexto de dados**, clique em **novo contexto de dados...** .

    ![Criando um novo contexto de dados](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    *Criando um novo contexto de dados*
5. Na caixa de diálogo **novo contexto de dados** , nomeie o novo contexto de dados *PersonContext* e clique em **Adicionar**.

    ![Criando o novo PersonContext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    *Criando o novo tipo de PersonContext*
6. Clique em **Adicionar** para criar o novo controlador para **pessoa** com scaffolding. O Visual Studio irá gerar as ações do controlador, o contexto de dados da pessoa e os modos de exibição do Razor.

    ![Depois de criar o controlador MVC com scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    *Depois de criar o controlador MVC com scaffolding*
7. Abra o arquivo **MvcPersonController.cs** na pasta **controladores** . Observe que os métodos de ação CRUD foram gerados automaticamente.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > Marcando a caixa de seleção **usar ações do controlador assíncrono** nas opções scaffolding nas etapas anteriores, o Visual Studio gera métodos de ação assíncronas para todas as ações que envolvem o acesso ao contexto de dados Person. É recomendável que você use métodos de ação assíncronas para solicitações de execução longa e não de CPU para evitar impedir que o servidor Web execute o trabalho enquanto a solicitação está sendo processada.

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a>Tarefa 3 – executando a solução

Nesta tarefa, você executará a solução novamente para verificar se as exibições da **pessoa** estão funcionando conforme o esperado. Você adicionará uma nova pessoa para verificar se ela foi salva com êxito no banco de dados.

1. Pressione **F5** para executar a solução.
2. Navegue até **/MvcPerson**. A exibição com Scaffold que mostra a lista de pessoas deve aparecer.
3. Clique em **criar novo** para adicionar uma nova pessoa.

    ![Navegando para as exibições do com Scaffold MVC](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    *Navegando para as exibições do com Scaffold MVC*
4. Na exibição **criar** , forneça um **nome** e uma **idade** para a pessoa e clique em **criar**.

    ![Adicionando uma nova pessoa](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    *Adicionando uma nova pessoa*
5. A nova pessoa é adicionada à lista. Na lista elemento, clique em **detalhes** para exibir a exibição de detalhes da pessoa. Em seguida, no modo de exibição de **detalhes** , clique em **voltar à lista** para voltar para a exibição de lista.

    ![Exibição de detalhes da pessoa](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    *Exibição de detalhes da pessoa*
6. Clique no link **excluir** para excluir a pessoa. Na exibição **excluir** , clique em **excluir** para confirmar a operação.

    ![Excluindo uma pessoa](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    *Excluindo uma pessoa*
7. Volte para o Visual Studio e pressione **Shift + F5** para parar a depuração.

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a>Exercício 3: Criando um controlador de API Web usando scaffolding

A estrutura da API Web faz parte da pilha ASP.NET e foi projetada para tornar a implementação de serviços HTTP mais fácil, geralmente enviando e recebendo dados formatados em JSON ou XML por meio de uma API RESTful.

Neste exercício, você usará ASP.NET scaffolding novamente para gerar um controlador de API da Web. Você usará as mesmas classes **Person** e **PersonContext** do exercício anterior para fornecer os mesmos dados de pessoa no formato JSON. Você verá como é possível expor os mesmos recursos de diferentes maneiras no mesmo aplicativo ASP.NET.

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a>Tarefa 1 – Criando um controlador de API da Web

Nesta tarefa, você criará um novo **controlador de API Web** que exporá os dados da pessoa em um formato de consumo de máquina como o JSON.

1. Se ainda não estiver aberto, abra **Visual Studio Express 2013 para Web** e abra a solução **MyHybridSite. sln** localizada na pasta **Source/EX3-WebAPI/Begin** . Como alternativa, você pode continuar com a solução que você obteve no exercício anterior.

    > [!NOTE]
    > Se você começar com a solução inicial do exercício 3, pressione **Ctrl + Shift + B** para compilar a solução.
2. Em **Gerenciador de soluções**, clique com o botão direito do mouse na pasta **controladores** do projeto **MyHybridSite** e selecione **Adicionar | Novo item de com Scaffold...**

    ![Criando um novo controlador com Scaffold](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    *Criando um novo controlador com Scaffold*
3. Na caixa de diálogo **Adicionar Scaffold** , selecione **API Web** no painel esquerdo, em seguida, **controlador da API Web 2 com ações, usando Entity Framework** no painel central e, em seguida, clique em **Adicionar.**

    ![Selecionando o controlador da API Web 2 com ações e Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "Selecionando o controlador da API Web 2 com ações e Entity Framework")

    *Selecionando o controlador da API Web 2 com ações e Entity Framework*
4. Defina *ApiPersonController* como o **nome do controlador**, selecione a opção **usar ações do controlador assíncrono** e selecione **pessoa (MyHybridSite. Models)** e **PersonContext (MyHybridSite. Models)** como as classes **modelo** e **contexto de dados** , respectivamente. Clique em **Adicionar**.

    ![Adicionando um controlador de API Web com scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "Adicionando um controlador de API Web com scaffolding")

    *Adicionando um controlador de API Web com scaffolding*
5. Em seguida, o Visual Studio irá gerar a classe **ApiPersonController** com as quatro ações CRUD para trabalhar com seus dados.

    ![Depois de criar o controlador da API Web com scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "Depois de criar o controlador da API Web com scaffolding")

    *Depois de criar o controlador da API Web com scaffolding*
6. Abra o arquivo **ApiPersonController.cs** e inspecione o método de ação *GetPeople* . Esse método consulta o campo DB do tipo **PersonContext** para obter os dados de pessoas.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. Agora, observe o comentário acima da definição do método. Ele fornece o URI que expõe essa ação que será usada na próxima tarefa.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > Por padrão, a API Web é configurada para capturar as consultas para o caminho */API* para evitar colisões com controladores MVC. Se você precisar alterar essa configuração, consulte [Roteamento em ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a>Tarefa 2 – executando a solução

Nesta tarefa, você usará as ferramentas de **desenvolvedor** do Internet Explorer F12 para inspecionar a resposta completa do controlador da API Web. Você verá como é possível capturar o tráfego de rede para obter mais informações sobre os dados do aplicativo.

> [!NOTE]
> Verifique se o **Internet Explorer** está selecionado no botão **Iniciar** localizado na barra de ferramentas do Visual Studio.
> 
> ![Opção do Internet Explorer](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> As **ferramentas de desenvolvedor F12** têm um amplo conjunto de funcionalidades que não são abordadas neste laboratório prático. Se você quiser saber mais sobre isso, consulte [usando as ferramentas de desenvolvedor F12](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).

1. Pressione **F5** para executar a solução.

    > [!NOTE]
    > Para seguir essa tarefa corretamente, seu aplicativo precisa ter dados. Se o banco de dados estiver vazio, você poderá voltar para a tarefa 3 no exercício 2 e seguir as etapas sobre como criar uma nova pessoa usando as exibições do MVC.
2. No navegador, pressione **F12** para abrir o painel de **ferramentas para desenvolvedores** . Pressione **CTRL** + **4** ou clique no ícone de **rede** e, em seguida, clique no botão de seta verde para começar a capturar o tráfego de rede.

    ![Iniciando a captura de rede da API Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "Iniciando a captura de rede da API Web")

    *Iniciando a captura de rede da API Web*
3. Acrescente **API/ApiPerson** à URL na barra de endereços do navegador. Agora, você inspecionará os detalhes da resposta do **ApiPersonController**.

    ![Recuperando dados da pessoa por meio da API da Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "Recuperando dados da pessoa por meio da API da Web")

    *Recuperando dados da pessoa por meio da API da Web*

    > [!NOTE]
    > Quando o download for concluído, você será solicitado a fazer uma ação com o arquivo baixado. Deixe a caixa de diálogo aberta para poder assistir ao conteúdo da resposta por meio da janela de ferramentas de desenvolvedores.
4. Agora, você inspecionará o corpo da resposta. Para fazer isso, clique na guia **detalhes** e, em seguida, clique em **corpo da resposta**. Você pode verificar se os dados baixados são uma lista de objetos com a **ID**de propriedades, o **nome** e a **idade** que correspondem à classe **Person** .

    ![Exibindo o corpo da resposta da API Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "Exibindo o corpo da resposta da API Web")

    *Exibindo o corpo da resposta da API Web*

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a>Tarefa 3 – adicionando páginas de ajuda da API Web

Quando você cria uma API da Web, é útil criar uma página de ajuda para que outros desenvolvedores saibam como chamar sua API. Você pode criar e atualizar as páginas de documentação manualmente, mas é melhor gerá-las automaticamente para evitar ter que fazer o trabalho de manutenção. Nesta tarefa, você usará um pacote NuGet para gerar automaticamente páginas de ajuda da API Web para a solução.

1. No menu **ferramentas** no Visual Studio, selecione **Gerenciador de pacotes NuGet**e clique em **console do Gerenciador de pacotes**.
2. Na janela do **console do Gerenciador de pacotes** , execute o seguinte comando:

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > O pacote **Microsoft. AspNet. WebApi. HelpPage** instala os assemblies necessários e adiciona exibições MVC para as páginas de ajuda na pasta **areas/HelpPage** .

    ![Área HelpPage](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "Área HelpPage")

    *Área HelpPage*
3. Por padrão, as páginas de ajuda têm cadeias de caracteres de espaço reservado para documentação. Você pode usar comentários de documentação XML para criar a documentação. Para habilitar esse recurso, abra o arquivo **HelpPageConfig.cs** localizado na pasta **areas/HelpPage/app\_Start** e remova a marca de comentário da seguinte linha:

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto **MyHybridSite**, selecione **Propriedades** e clique na guia **Compilar** .

    ![Guia compilar](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "Seção de Build")

    *Guia compilar*
5. Em **saída**, selecione **arquivo de documentação XML**. Na caixa de edição, digite **App\_data/XmlDocument. xml**.

    ![Seção de saída na guia Build](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "Seção de saída na guia Build")

    *Seção de saída na guia Build*
6. Pressione **CTRL** + **S** para salvar as alterações.
7. Abra o arquivo **ApiPersonController.cs** na pasta **controladores** .
8. Insira uma nova linha entre a assinatura do método *GetPeople* e o comentário *//Get API/ApiPerson* e, em seguida, digite três barras invertidas.

    > [!NOTE]
    > O Visual Studio insere automaticamente os elementos XML que definem a documentação do método.
9. Adicione um texto de resumo e o valor de retorno para o método *GetPeople* . Ele deverá ter a seguinte aparência.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. Pressione **F5** para executar a solução.
11. Acrescente **/Help** à URL na barra de endereços para navegar até a página de ajuda.

    ![Página de ajuda do ASP.NET Web API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "Página de ajuda do ASP.NET Web API")

    *Página de ajuda do ASP.NET Web API*

    > [!NOTE]
    > O conteúdo principal da página é uma tabela de APIs, agrupada por controlador. As entradas de tabela são geradas dinamicamente, usando a interface **IApiExplorer** . Se você adicionar ou atualizar um controlador de API, a tabela será atualizada automaticamente na próxima vez que você compilar o aplicativo.
    > 
    > A coluna **API** lista o método http e o URI relativo. A coluna **Descrição** contém informações que foram extraídas da documentação do método.
12. Observe que a descrição que você adicionou acima da definição do método é exibida na coluna Descrição.

    ![Descrição do método de API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "Descrição do método de API")

    *Descrição do método de API*
13. Clique em um dos métodos de API para navegar até uma página com informações mais detalhadas, incluindo corpos de resposta de exemplo.

    ![Página informações de detalhes](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "Página informações de detalhes")

    *Página de informações detalhadas*

---

<a id="Summary"></a>
## <a name="summary"></a>Resumo

Ao concluir este laboratório prático, você aprendeu a:

- Criar um novo aplicativo Web usando a experiência de ASP.NET no Visual Studio 2013
- Integre várias tecnologias ASP.NET em um único projeto
- Gerar os controladores MVC e exibições de suas classes de modelo usando ASP.NET scaffolding
- Gerar controladores de API Web, que usam recursos como programação assíncrona e acesso a dados por meio de Entity Framework
- Gerar automaticamente páginas de ajuda da API Web para seus controladores
