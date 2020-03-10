---
uid: web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
title: 'Laboratório prático: criar um aplicativo de página única (SPA) com ASP.NET Web API e angular. js-ASP.NET 4. x'
author: rick-anderson
description: 'Código passo a passo: Crie um aplicativo de página única (SPA) com ASP.NET Web API e angular. js para ASP.NET 4. x.'
ms.author: riande
ms.date: 09/30/2015
ms.custom: seoapril2019
ms.assetid: 719727b7-bef3-45ad-bfe9-ba5bcdb2305f
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
msc.type: authoredcontent
ms.openlocfilehash: 86833a890da759e489dd11dc9afb128a9b7a75e3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557039"
---
# <a name="hands-on-lab-build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs"></a>Laboratório prático: criar um aplicativo de página única (SPA) com ASP.NET Web API e angular. js

por [equipe de acampamentos da Web](https://twitter.com/webcamps)

[Baixe o kit de treinamento do Web acampamentos](https://aka.ms/webcamps-training-kit)

Este laboratório prático mostra como criar um aplicativo de página única (SPA) com ASP.NET Web API e angular. js para ASP.NET 4. x.

Neste laboratório prático, você aproveitará essas tecnologias para implementar um teste de pau, um site Trívia com base no conceito de SPA. Primeiro, você implementará a camada de serviço com ASP.NET Web API para expor os pontos de extremidade necessários para recuperar as perguntas do teste e armazenar as respostas. Em seguida, você criará uma interface do usuário avançada e responsiva usando efeitos de transformação AngularJS e CSS3.

Em aplicativos Web tradicionais, o cliente (navegador) inicia a comunicação com o servidor solicitando uma página. Em seguida, o servidor processa a solicitação e envia o HTML da página para o cliente. Em interações subsequentes com a página – por exemplo, o usuário navega para um link ou envia um formulário com dados – uma nova solicitação é enviada ao servidor e o fluxo é iniciado novamente: o servidor processa a solicitação e envia uma nova página para o navegador em resposta à nova solicitação de ação Ed pelo cliente.
> 
> Em aplicativos de página única (SPAs), a página inteira é carregada no navegador após a solicitação inicial, mas as interações subsequentes ocorrem por meio de solicitações Ajax. Isso significa que o navegador precisa atualizar apenas a parte da página que foi alterada; Não é necessário recarregar a página inteira. A abordagem SPA reduz o tempo gasto pelo aplicativo para responder às ações do usuário, resultando em uma experiência mais fluida.
> 
> A arquitetura de um SPA envolve determinados desafios que não estão presentes em aplicativos Web tradicionais. No entanto, as tecnologias emergentes, como ASP.NET Web API, estruturas JavaScript como AngularJS e novos recursos de estilo fornecidos pelo CSS3 tornam muito fácil criar e criar SPAs.
> 
> 
> Todos os códigos de exemplo e trechos de código estão incluídos no kit de treinamento do acampamentos da Web, disponível em [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit).

## <a name="overview"></a>Visão geral

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

Neste laboratório prático, você aprenderá a:

- Criar um serviço de ASP.NET Web API para enviar e receber dados JSON
- Criar uma interface do usuário responsiva usando AngularJS
- Aprimore a experiência de interface do usuário com transformações de CSS3

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisites

O seguinte é necessário para concluir este laboratório prático:

- [Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/) ou superior

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

1. [Criando uma API Web](#Exercise1)
2. [Criando uma interface SPA](#Exercise2)

Tempo estimado para concluir este laboratório: **60 minutos**

> [!NOTE]
> Ao iniciar o Visual Studio pela primeira vez, você deve selecionar uma das coleções de configurações predefinidas. Cada coleção predefinida é projetada para corresponder a um estilo de desenvolvimento específico e determina layouts de janela, comportamento do editor, trechos de código IntelliSense e opções da caixa de diálogo. Os procedimentos neste laboratório descrevem as ações necessárias para realizar uma determinada tarefa no Visual Studio ao usar a coleção de **configurações de desenvolvimento geral** . Se você escolher uma coleção de configurações diferentes para seu ambiente de desenvolvimento, poderá haver diferenças nas etapas que você deve levar em conta.

<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-web-api"></a>Exercício 1: criando uma API Web

Uma das principais partes de um SPA é a camada de serviço. Ele é responsável por processar as chamadas AJAX enviadas pela interface do usuário e retornar dados em resposta a essa chamada. Os dados recuperados devem ser apresentados em um formato legível por máquina para serem analisados e consumidos pelo cliente.

A estrutura da API Web faz parte da pilha ASP.NET e é projetada para facilitar a implementação de serviços HTTP, geralmente enviando e recebendo dados formatados em JSON ou XML por meio de uma API RESTful. Neste exercício, você criará o site para hospedar o aplicativo de teste de especialista e, em seguida, implementará o serviço de back-end para expor e manter os dados do teste usando ASP.NET Web API.

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-the-initial-project-for-geek-quiz"></a>Tarefa 1 – criando o projeto inicial para o teste de pau

Nesta tarefa, você começará a criar um novo projeto MVC do ASP.NET com suporte para ASP.NET Web API com base em um tipo de projeto **ASP.net** que vem com o Visual Studio. **Uma ASP.net** unifica todas as tecnologias de ASP.net e oferece a opção de misturar e CORRESP-las conforme desejado. Em seguida, você adicionará as classes de modelo do Entity Framework e o inicializador de banco de dados para inserir as perguntas do teste.

1. Abra **Visual Studio Express 2013 para Web** e selecione **arquivo | Novo projeto...** para iniciar uma nova solução.

    ![Criando um novo projeto](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image1.png "Criando um novo projeto")

    *Criando um novo projeto*
2. Na caixa de diálogo **novo projeto** , selecione **ASP.NET aplicativo Web** no **Visual C# | Guia da Web** . Verifique se **.NET Framework 4,5** está selecionado, nomeie-o *GeekQuiz*, escolha um **local** e clique em **OK**.

    ![Criando um novo projeto de aplicativo Web ASP.NET](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image2.png "Criando um novo projeto de aplicativo Web ASP.NET")

    *Criando um novo projeto de aplicativo Web ASP.NET*
3. Na caixa de diálogo **novo projeto ASP.net** , selecione o modelo **MVC** e selecione a opção **API Web** . Além disso, verifique se a opção de **autenticação** está definida para **contas de usuário individuais**. Clique em **OK** para continuar.

    ![Criando um novo projeto com o modelo MVC, incluindo componentes da API Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image3.png)

    *Criando um novo projeto com o modelo MVC, incluindo componentes da API Web*
4. Em **Gerenciador de soluções**, clique com o botão direito do mouse na pasta **modelos** do projeto **GeekQuiz** e selecione **Adicionar | Item existente...** .

    ![Adicionando um item existente](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image4.png "Adicionando um item existente")

    *Adicionando um item existente*
5. Na caixa de diálogo **Adicionar item existente** , navegue até a pasta **origem/ativos/modelos** e selecione todos os arquivos. Clique em **Adicionar**.

    ![Adicionando os ativos do modelo](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image5.png "Adicionando os ativos do modelo")

    *Adicionando os ativos do modelo*

    > [!NOTE]
    > Ao adicionar esses arquivos, você está adicionando o modelo de dados, o contexto de banco de dado do Entity Framework e o inicializador de banco de dados para o aplicativo de teste de especialista.
    > 
    > **Entity Framework (EF)** é um mapeador relacional de objeto (ORM) que permite que você crie aplicativos de acesso a dados programando com um modelo de aplicativo conceitual em vez de programar diretamente usando um esquema de armazenamento relacional. Você pode saber mais sobre Entity Framework [aqui](../../../entity-framework.md).
    > 
    > Veja a seguir uma descrição das classes que você acabou de adicionar:
    > 
    > - **TriviaOption:** representa uma única opção associada a uma pergunta de teste
    > - **TriviaQuestion:** representa uma pergunta de teste e expõe as opções associadas por meio da propriedade **Options**
    > - **TriviaAnswer:** representa a opção selecionada pelo usuário em resposta a uma pergunta de teste
    > - **TriviaContext:** representa o contexto de banco de dados do Entity Framework do aplicativo de teste de especialista. Essa classe deriva de **DContext** e expõe as propriedades **DbSet** que representam as coleções das entidades descritas acima.
    > - **TriviaDatabaseInitializer:** a implementação do inicializador de Entity Framework para a classe **TriviaContext** que herda de **CreateDatabaseIfNotExists**. O comportamento padrão dessa classe é criar o banco de dados somente se ele não existir, inserindo as entidades especificadas no método **semente** .
6. Abra o arquivo **global.asax.cs** e adicione a instrução using a seguir.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample1.cs)]
7. Adicione o seguinte código no início do **aplicativo\_** método de início para definir o **TriviaDatabaseInitializer** como inicializador de banco de dados.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample2.cs)]
8. Modifique o controlador **inicial** para restringir o acesso a usuários autenticados. Para fazer isso, abra o arquivo **HomeController.cs** dentro da pasta **controladores** e adicione o atributo **autorizar** à definição de classe **HomeController** .

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample3.cs)]

    > [!NOTE]
    > O filtro **autorizar** verifica se o usuário está autenticado. Se o usuário não for autenticado, ele retornará o código de status HTTP 401 (não autorizado) sem invocar a ação. Você pode aplicar o filtro globalmente, no nível do controlador ou no nível de ações individuais.
9. Agora, você personalizará o layout das páginas da Web e da identidade visual. Para fazer isso, abra o arquivo **\_layout. cshtml** dentro dos **modos de exibição | Pasta compartilhada** e atualize o conteúdo do elemento **&lt;title&gt;** substituindo *meu aplicativo ASP.net* por *especialista em pau*.

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample4.cshtml)]
10. No mesmo arquivo, atualize a barra de navegação removendo os links *about* e *Contact* e renomeando o link *Home* para *reproduzir*. Além disso, renomeie o link do *nome do aplicativo* para *teste de especialista*. O HTML da barra de navegação deve ser semelhante ao código a seguir.

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample5.cshtml)]
11. Atualize o rodapé da página de layout substituindo *meu aplicativo ASP.net* por *especialista em pau*. Para fazer isso, substitua o conteúdo do elemento **&lt;footer&gt;** pelo código realçado a seguir.

    [!code-html[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample6.html)]

<a id="Ex1Task2"></a>
#### <a name="task-2--creating-the-triviacontroller-web-api"></a>Tarefa 2 – Criando a API Web TriviaController

Na tarefa anterior, você criou a estrutura inicial do aplicativo Web de teste de especialista. Agora, você criará um serviço de API Web simples que interage com o modelo de dados de teste e expõe as seguintes ações:

- **Get/API/Trivia**: recupera a próxima pergunta da lista de testes a ser respondida pelo usuário autenticado.
- **Post/API/Trivia**: armazena a resposta do teste especificada pelo usuário autenticado.

Você usará as ferramentas do ASP.NET scaffolding fornecidas pelo Visual Studio para criar a linha de base para a classe do controlador da API Web.

1. Abra o arquivo **WebApiConfig.cs** dentro do **aplicativo\_pasta inicial** . Esse arquivo define a configuração do serviço de API Web, como as rotas são mapeadas para as ações do controlador da API Web.
2. Adicione a seguinte instrução using no início do arquivo.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample7.cs)]
3. Adicione o seguinte código realçado ao método **Register** para configurar globalmente o formatador para os dados JSON recuperados pelos métodos de ação da API Web.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample8.cs)]

    > [!NOTE]
    > O **CamelCasePropertyNamesContractResolver** converte automaticamente os nomes de propriedade em *Camel* Case, que é a Convenção geral para nomes de propriedade em JavaScript.
4. Em **Gerenciador de soluções**, clique com o botão direito do mouse na pasta **controladores** do projeto **GeekQuiz** e selecione **Adicionar | Novo item de com Scaffold...**

    ![Criando um novo item com Scaffold](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image6.png "Criando um novo item com Scaffold")

    *Criando um novo item com Scaffold*
5. Na caixa de diálogo **Adicionar Scaffold** , verifique se o nó **comum** está selecionado no painel esquerdo. Em seguida, selecione o modelo **controlador da API Web 2 – vazio** no painel central e clique em **Adicionar**.

    ![Selecionando o modelo vazio do controlador da API Web 2](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image7.png "Selecionando o modelo vazio do controlador da API Web 2")

    *Selecionando o modelo vazio do controlador da API Web 2*

    > [!NOTE]
    > **ASP.net scaffolding** é uma estrutura de geração de código para aplicativos ASP.NET Web. Visual Studio 2013 inclui geradores de código pré-instalados para projetos MVC e de API Web. Você deve usar o scaffolding em seu projeto quando desejar adicionar rapidamente o código que interage com modelos de dados para reduzir a quantidade de tempo necessária para desenvolver operações de dados padrão.
    > 
    > O processo scaffolding também garante que todas as dependências necessárias sejam instaladas no projeto. Por exemplo, se você começar com um projeto ASP.NET vazio e, em seguida, usar scaffolding para adicionar um controlador de API Web, os pacotes e as referências do NuGet da API Web necessários serão adicionados ao seu projeto automaticamente.
6. Na caixa de diálogo **Adicionar controlador** , digite *TriviaController* na caixa de texto **nome do controlador** e clique em **Adicionar**.

    ![Adicionando o controlador Trívia](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image8.png "Adicionando o controlador Trívia")

    *Adicionando o controlador Trívia*
7. O arquivo **TriviaController.cs** é então adicionado à pasta **controladores** do projeto **GeekQuiz** , que contém uma classe **TriviaController** vazia. Adicione as seguintes instruções using no início do arquivo.

    (Trecho de código- *AspNetWebApiSpa-EX1-TriviaControllerUsings*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample9.cs)]
8. Adicione o seguinte código no início da classe **TriviaController** para definir, inicializar e descartar a instância **TriviaContext** no controlador.

    (Trecho de código- *AspNetWebApiSpa-EX1-TriviaControllerContext*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample10.cs)]

    > [!NOTE]
    > O método **Dispose** de **TriviaController** invoca o método **Dispose** da instância **TriviaContext** , que garante que todos os recursos usados pelo objeto Context sejam liberados quando a instância **TriviaContext** for descartada ou coletada como lixo. Isso inclui fechar todas as conexões de banco de dados abertas pelo Entity Framework.
9. Adicione o seguinte método auxiliar ao final da classe **TriviaController** . Esse método recupera a seguinte pergunta do teste do banco de dados a ser respondida pelo usuário especificado.

    (Trecho de código- *AspNetWebApiSpa-EX1-TriviaControllerNextQuestion*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample11.cs)]
10. Adicione o seguinte método **Get** Action à classe **TriviaController** . Esse método de ação chama o método auxiliar **NextQuestionAsync** definido na etapa anterior para recuperar a próxima pergunta para o usuário autenticado.

    (Trecho de código- *AspNetWebApiSpa-EX1-TriviaControllerGetAction*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample12.cs)]
11. Adicione o seguinte método auxiliar ao final da classe **TriviaController** . Esse método armazena a resposta especificada no banco de dados e retorna um valor booliano que indica se a resposta está correta ou não.

    (Trecho de código- *AspNetWebApiSpa-EX1-TriviaControllerStoreAsync*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample13.cs)]
12. Adicione o seguinte método de ação **post** à classe **TriviaController** . Esse método de ação associa a resposta ao usuário autenticado e chama o método auxiliar **StoreAsync** . Em seguida, ele envia uma resposta com o valor booliano retornado pelo método auxiliar.

    (Trecho de código- *AspNetWebApiSpa-EX1-TriviaControllerPostAction*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample14.cs)]
13. Modifique o controlador da API Web para restringir o acesso a usuários autenticados adicionando o atributo **Authorize** à definição de classe **TriviaController** .

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample15.cs)]

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>Tarefa 3 – executando a solução

Nesta tarefa, você verificará se o serviço de API Web criado na tarefa anterior está funcionando conforme o esperado. Você usará o Ferramentas para Desenvolvedores de **F12** do Internet Explorer para capturar o tráfego de rede e inspecionar a resposta completa do serviço de API Web.

> [!NOTE]
> Verifique se o **Internet Explorer** está selecionado no botão **Iniciar** localizado na barra de ferramentas do Visual Studio.
> 
> ![Opção do Internet Explorer](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image9.png)

1. Pressione **F5** para executar a solução. A página de **logon** deve aparecer no navegador.

    > [!NOTE]
    > Quando o aplicativo é iniciado, a rota padrão do MVC é disparada, que por padrão é mapeada para a ação de **índice** da classe **HomeController** . Como o **HomeController** é restrito a usuários autenticados (Lembre-se de que você decorau essa classe com o atributo **Authorize** no exercício 1) e não há nenhum usuário autenticado ainda, o aplicativo redireciona a solicitação original para a página de logon.

    ![Executando a solução](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image10.png "Executando a solução")

    *Executando a solução*
2. Clique em **registrar** para criar um novo usuário.

    ![Registrando um novo usuário](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image11.png "Registrando um novo usuário")

    *Registrando um novo usuário*
3. Na página **registrar** , insira um **nome de usuário** e **senha**e, em seguida, clique em **registrar**.

    ![Página de registro](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image12.png "Página de registro")

    *Página de registro*
4. O aplicativo registra a nova conta e o usuário é autenticado e Redirecionado de volta para a home page.

    ![O usuário está autenticado](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image13.png "Usuário autenticado")

    *O usuário está autenticado*
5. No navegador, pressione **F12** para abrir o painel de **ferramentas para desenvolvedores** . Pressione **Ctrl + 4** ou clique no ícone de **rede** e, em seguida, clique no botão de seta verde para começar a capturar o tráfego de rede.

    ![Iniciando a captura de rede da API Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image14.png "Iniciando a captura de rede da API Web")

    *Iniciando a captura de rede da API Web*
6. Acrescente **API/Trívia** à URL na barra de endereços do navegador. Agora, você inspecionará os detalhes da resposta do método **Get** Action em **TriviaController**.

    ![Recuperando os dados da próxima pergunta por meio da API da Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image15.png "Recuperando os dados da próxima pergunta por meio da API da Web")

    *Recuperando os dados da próxima pergunta por meio da API da Web*

    > [!NOTE]
    > Quando o download for concluído, você será solicitado a fazer uma ação com o arquivo baixado. Deixe a caixa de diálogo aberta para poder assistir ao conteúdo da resposta por meio da janela de ferramentas de desenvolvedores.
7. Agora, você inspecionará o corpo da resposta. Para fazer isso, clique na guia **detalhes** e, em seguida, clique em **corpo da resposta**. Você pode verificar se os dados baixados são um objeto com as **Opções** de Propriedades (que é uma lista de objetos **TriviaOption** ), **ID** e **título** que correspondem à classe **TriviaQuestion** .

    ![Exibindo o corpo da resposta da API Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image16.png "Exibindo o corpo da resposta da API Web")

    *Exibindo o corpo da resposta da API Web*
8. Volte para o Visual Studio e pressione **Shift + F5** para parar a depuração.

<a id="Exercise2"></a>
### <a name="exercise-2-creating-the-spa-interface"></a>Exercício 2: criando a interface SPA

Neste exercício, você primeiro criará a parte de front-end da Web do teste de especialista, concentrando-se na interação do aplicativo de página única usando **AngularJS**. Em seguida, você aprimorará a experiência do usuário com o CSS3 para executar animações avançadas e fornecer um efeito visual de alternância de contexto ao fazer a transição de uma pergunta para a próxima.

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-the-spa-interface-using-angularjs"></a>Tarefa 1 – criando a interface SPA usando AngularJS

Nesta tarefa, você usará o **AngularJS** para implementar o lado do cliente do aplicativo de teste de especialista. O **AngularJS** é uma estrutura JavaScript de software livre que aumenta os aplicativos baseados em navegador com o recurso MVC ( *Model-View-Controller* ), facilitando o desenvolvimento e os testes.

Você começará instalando o AngularJS no console do Gerenciador de pacotes do Visual Studio. Em seguida, você criará o controlador para fornecer o comportamento do aplicativo de teste de especialista e a exibição para renderizar as perguntas e respostas do teste usando o mecanismo de modelo AngularJS.

> [!NOTE]
> Para obter mais informações sobre o AngularJS, consulte [[http://angularjs.org/](http://angularjs.org/)](http://angularjs.org/).

1. Abra **Visual Studio Express 2013 para Web** e abra a solução **GeekQuiz. sln** localizada na pasta **Source/EX2-CreatingASPAInterface/Begin** . Como alternativa, você pode continuar com a solução que você obteve no exercício anterior.
2. Abra o **console do Gerenciador de pacotes** em **ferramentas** > **Gerenciador de pacotes NuGet**. Digite o seguinte comando para instalar o pacote do NuGet **. Core do AngularJS** .

    [!code-powershell[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample16.ps1)]
3. Em **Gerenciador de soluções**, clique com o botão direito do mouse na pasta **scripts** do projeto **GeekQuiz** e selecione **Adicionar | Nova pasta**. Nomeie o **aplicativo** de pasta e pressione **Enter**.
4. Clique com o botão direito do mouse na pasta do **aplicativo** que você acabou de criar e selecione **Adicionar | Arquivo JavaScript**.

    ![Criando um novo arquivo JavaScript](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image17.png)

    *Criando um novo arquivo JavaScript*
5. Na caixa de diálogo **especificar nome para o item** , *digite teste-controlador* na caixa de texto **nome do item** e clique em **OK**.

    ![Nomeando o novo arquivo JavaScript](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image18.png)

    *Nomeando o novo arquivo JavaScript*
6. No arquivo **quiz-Controller. js** , adicione o código a seguir para declarar e inicializar o controlador de **QuizCtrl** AngularJS.

    (Trecho de código- *AspNetWebApiSpa-EX2-AngularQuizController*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample17.js)]

    > [!NOTE]
    > A função de construtor do controlador **QuizCtrl** espera um parâmetro injetado chamado **$Scope**. O estado inicial do escopo deve ser configurado na função do Construtor anexando Propriedades ao objeto **$Scope** . As propriedades contêm o **modelo de exibição**e estarão acessíveis ao modelo quando o controlador for registrado.
    > 
    > O controlador **QuizCtrl** é definido dentro de um módulo chamado **QuizApp**. Os módulos são unidades de trabalho que permitem dividir seu aplicativo em componentes separados. As principais vantagens de usar os módulos é que o código é mais fácil de entender e facilita o teste de unidade, reusabilidade e capacidade de manutenção.
7. Agora, você adicionará o comportamento ao escopo para reagir a eventos disparados a partir da exibição. Adicione o seguinte código ao final do controlador **QuizCtrl** para definir a função **nextQuestion** no objeto **$Scope** .

    (Trecho de código- *AspNetWebApiSpa-EX2-AngularQuizControllerNextQuestion*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample18.js)]

    > [!NOTE]
    > Essa função recupera a próxima pergunta da API Web **Trívia** criada no exercício anterior e anexa os dados da pergunta ao objeto **$Scope** .
8. Insira o código a seguir no final do controlador **QuizCtrl** para definir a função **sendAnswer** no objeto **$Scope** .

    (Trecho de código- *AspNetWebApiSpa-EX2-AngularQuizControllerSendAnswer*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample19.js)]

    > [!NOTE]
    > Essa função envia a resposta selecionada pelo usuário para a API Web **Trívia** e armazena o resultado – ou seja, se a resposta estiver correta ou não, no objeto **$Scope** .
    > 
    > As funções **nextQuestion** e **sendAnswer** acima usam o objeto de **$http** AngularJS para abstrair a comunicação com a API da Web por meio do objeto JavaScript XMLHttpRequest do navegador. O AngularJS dá suporte a outro serviço que leva um nível mais alto de abstração para executar operações CRUD em um recurso por meio de APIs RESTful. O objeto **$Resource** AngularJS tem métodos de ação que fornecem comportamentos de alto nível sem a necessidade de interagir com o objeto **$http** . Considere usar o objeto **$Resource** em cenários que exijam o modelo CRUD (informações de primeiro plano, consulte a [documentação do $Resource](https://docs.angularjs.org/api/ngResource/service/$resource)).
9. A próxima etapa é criar o modelo AngularJS que define a exibição do teste. Para fazer isso, abra o arquivo **index. cshtml** dentro dos **modos de exibição | Pasta base** e substitua o conteúdo pelo código a seguir.

    (Trecho de código- *AspNetWebApiSpa-EX2-GeekQuizView*)

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample20.cshtml)]

    > [!NOTE]
    > O modelo AngularJS é uma especificação declarativa que usa informações do modelo e do controlador para transformar a marcação estática na exibição dinâmica que o usuário vê no navegador. Veja a seguir exemplos de elementos AngularJS e atributos de elemento que podem ser usados em um modelo:
    > 
    > - A diretiva **ng-app** informa ao AngularJS o elemento DOM que representa o elemento raiz do aplicativo.
    > - A diretiva **ng-Controller** anexa um controlador ao dom no ponto em que a diretiva é declarada.
    > - A notação de chave **{{}}** denota associações às propriedades de escopo definidas no controlador.
    > - A diretiva **ng-Click** é usada para invocar as funções definidas no escopo em resposta a cliques do usuário.
10. Abra o arquivo **site. css** dentro da pasta de **conteúdo** e adicione os seguintes estilos realçados ao final do arquivo para fornecer uma aparência para a exibição do teste.

    (Trecho de código- *AspNetWebApiSpa-EX2-GeekQuizStyles*)

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample21.css)]

<a id="Ex2Task2"></a>
#### <a name="task-2--running-the-solution"></a>Tarefa 2 – executando a solução

Nesta tarefa, você executará a solução usando a nova interface do usuário criada com o AngularJS para responder a algumas das perguntas do teste.

1. Pressione **F5** para executar a solução.
2. Registrar uma nova conta de usuário. Para fazer isso, siga as etapas de registro descritas no exercício 1, tarefa 3.

    > [!NOTE]
    > Se estiver usando a solução do exercício anterior, você poderá fazer logon com a conta de usuário que você criou antes.
3. A **Home** Page deve aparecer, mostrando a primeira pergunta do teste. Responda à pergunta clicando em uma das opções. Isso irá disparar a função **sendAnswer** definida anteriormente, que envia a opção selecionada para a API Web **Trívia** .

    ![Respondendo a uma pergunta](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image19.png "Respondendo a uma pergunta")

    *Respondendo a uma pergunta*
4. Depois de clicar em um dos botões, a resposta deve aparecer. Clique em **próxima pergunta** para mostrar a pergunta a seguir. Isso irá disparar a função **nextQuestion** definida no controlador.

    ![Solicitando a próxima pergunta](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image20.png "Solicitando a próxima pergunta")

    *Solicitando a próxima pergunta*
5. A próxima pergunta deve aparecer. Continue respondendo às perguntas quantas vezes desejar. Depois de concluir todas as perguntas, você deve retornar à primeira pergunta.

    ![Outra pergunta](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image21.png "Outra pergunta")

    *Próxima pergunta*
6. Volte para o Visual Studio e pressione **Shift + F5** para parar a depuração.

<a id="Ex2Task3"></a>
#### <a name="task-3--creating-a-flip-animation-using-css3"></a>Tarefa 3 – criando uma animação invertida usando CSS3

Nesta tarefa, você usará as propriedades do CSS3 para executar animações avançadas adicionando um efeito de inversão quando uma pergunta for respondida e quando a próxima pergunta for recuperada.

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse na pasta de **conteúdo** do projeto **GeekQuiz** e selecione **Adicionar | Item existente...** .

    ![Adicionando um item existente à pasta de conteúdo](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image22.png "Adicionando um item existente à pasta de conteúdo")

    *Adicionando um item existente à pasta de conteúdo*
2. Na caixa de diálogo **Adicionar item existente** , navegue até a pasta **origem/ativos** e selecione **inverter. css**. Clique em **Adicionar**.

    ![Adicionando o arquivo flip. CSS dos ativos](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image23.png "Adicionando o arquivo flip. CSS dos ativos")

    *Adicionando o arquivo flip. CSS dos ativos*
3. Abra o arquivo **flip. css** que você acabou de adicionar e inspecione seu conteúdo.
4. Localize o comentário de **transformação inverter** . Os estilos abaixo desse comentário usam a **perspectiva** de CSS e as transformações de **rotação** para gerar um efeito de&quot; de inversão de cartão &quot;.

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample22.css)]
5. Localize o **painel ocultar atrás durante** o comentário de flip. O estilo abaixo desse comentário oculta o lado do verso das faces quando eles estão voltados para fora do visualizador, definindo a propriedade CSS de **visibilidade de backface** como *Hidden*.

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample23.css)]
6. Abra o arquivo **BundleConfig.cs** dentro do **aplicativo\_pasta inicial** e adicione a referência ao arquivo **flip. css** no grupo estilo de **&quot;&quot;~/Content/CSS**

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample24.cs)]
7. Pressione **F5** para executar a solução e fazer logon com suas credenciais.
8. Responda a uma pergunta clicando em uma das opções. Observe o efeito de inverter ao fazer a transição entre exibições.

    ![Respondendo uma pergunta com o efeito de inverter](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image24.png "Respondendo uma pergunta com o efeito de inverter")

    *Respondendo uma pergunta com o efeito de inverter*
9. Clique em **próxima pergunta** para recuperar a seguinte pergunta. O efeito de inversão deve aparecer novamente.

    ![Recuperando a seguinte pergunta com o efeito de inverter](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image25.png "Recuperando a seguinte pergunta com o efeito de inverter")

    *Recuperando a seguinte pergunta com o efeito de inverter*

---

<a id="Summary"></a>
## <a name="summary"></a>Resumo

Ao concluir este laboratório prático, você aprendeu a:

- Criar um controlador de ASP.NET Web API usando ASP.NET scaffolding
- Implemente uma ação de API Web get para recuperar a próxima pergunta do quiz
- Implementar uma ação de postagem da API Web para armazenar as respostas do teste
- Instalar o AngularJS no console do Gerenciador de pacotes do Visual Studio
- Implementar modelos e controladores AngularJS
- Usar as transições do CSS3 para executar efeitos de animação
