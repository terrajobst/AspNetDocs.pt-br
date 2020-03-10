---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: Filtros de ação personalizada do ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: O ASP.NET MVC fornece filtros de ação para executar a lógica de filtragem antes ou depois de um método de ação ser chamado. Os filtros de ação são atributos personalizados tha...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: eaeb32180f79fabf557cbc38ff067eb26b47fea7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579691"
---
# <a name="aspnet-mvc-4-custom-action-filters"></a>Filtros de ação personalizada do ASP.NET MVC 4

por [equipe de acampamentos da Web](https://twitter.com/webcamps)

[Baixe o kit de treinamento do Web acampamentos](https://aka.ms/webcamps-training-kit)

O ASP.NET MVC fornece filtros de ação para executar a lógica de filtragem antes ou depois de um método de ação ser chamado. Os filtros de ação são atributos personalizados que fornecem meios declarativos para adicionar o comportamento de ação prévia e ação aos métodos de ação do controlador.

Neste laboratório prático, você criará um atributo de filtro de ação personalizado na solução MvcMusicStore para capturar as solicitações do controlador e registrar a atividade de um site em uma tabela de banco de dados. Você poderá adicionar o filtro de log por injeção a qualquer controlador ou ação. Por fim, você verá o modo de exibição de log que mostra a lista de visitantes.

Este laboratório prático pressupõe que você tenha conhecimento básico do **ASP.NET MVC**. Se você não usou o **ASP.NET MVC** antes, recomendamos que você vá para o laboratório prático do **ASP.NET MVC 4 Fundamentals** .

> [!NOTE]
> Todos os códigos de exemplo e trechos de código estão incluídos no Web acampamentos Training Kit, disponível em [Microsoft-Web/WebCampTrainingKit releases](https://aka.ms/webcamps-training-kit). O projeto específico deste laboratório está disponível em [filtros de ação personalizada do ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

Neste laboratório prático, você aprenderá a:

- Criar um atributo de filtro de ação personalizado para estender os recursos de filtragem
- Aplicar um atributo de filtro personalizado por injeção a um nível específico
- Registrar um filtro de ação personalizada globalmente

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

Se você não estiver familiarizado com os trechos de Visual Studio Code e quiser saber como usá-los, consulte o apêndice deste documento &quot;[Apêndice C: usando trechos de código](#AppendixC)&quot;.

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Exercícios

Este laboratório prático é composto pelos seguintes exercícios:

1. [Exercício 1: ações de log](#Exercise1)
2. [Exercício 2: gerenciando vários filtros de ação](#Exercise2)

Tempo estimado para concluir este laboratório: **30 minutos**.

> [!NOTE]
> Cada exercício é acompanhado por uma pasta **final** que contém a solução resultante que você deve obter depois de concluir os exercícios. Você pode usar essa solução como um guia se precisar de ajuda adicional para trabalhar com os exercícios.

<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a>Exercício 1: ações de log

Neste exercício, você aprenderá a criar um filtro de log de ação personalizado usando os provedores de filtros do ASP.NET MVC 4. Para essa finalidade, você aplicará um filtro de log ao site MusicStore, que registrará todas as atividades nos controladores selecionados.

O filtro estenderá **ActionFilterAttributeClass** e substituirá o método **OnActionExecuting** para capturar cada solicitação e, em seguida, executará as ações de log. As informações de contexto sobre solicitações HTTP, executando métodos, resultados e parâmetros serão fornecidas pela classe ASP.NET MVC ActionExecutingContext **.**

> [!NOTE]
> O ASP.NET MVC 4 também tem provedores de filtros padrão que você pode usar sem criar um filtro personalizado. O ASP.NET MVC 4 fornece os seguintes tipos de filtros:
> 
> - Filtro de **autorização** , que toma decisões de segurança sobre a execução de um método de ação, como executar a autenticação ou validar as propriedades da solicitação.
> - Filtro de **ação** , que encapsula a execução do método de ação. Esse filtro pode executar processamento adicional, como fornecer dados extras ao método de ação, inspecionar o valor de retorno ou cancelar a execução do método de ação
> - Filtro de **resultado** , que encapsula a execução do objeto ActionResult. Esse filtro pode executar processamento adicional do resultado, como modificar a resposta HTTP.
> - Filtro de **exceção** , que é executado se houver uma exceção sem tratamento gerada em algum lugar no método de ação, começando com os filtros de autorização e terminando com a execução do resultado. Os filtros de exceção podem ser usados para tarefas como registro em log ou exibição de uma página de erro.
> 
> Para obter mais informações sobre os provedores de filtros, visite este link do MSDN: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)).

<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a>Sobre o recurso de log de aplicativo da loja de música MVC

Esta solução de repositório de música tem uma nova tabela de modelo de dados para log de site, **ActionLog**, com os seguintes campos: nome do controlador que recebeu uma solicitação, chamada ação, IP do cliente e carimbo de data/hora.

![Modelo de dados. Tabela ActionLog.](aspnet-mvc-4-custom-action-filters/_static/image1.png "Modelo de dados. Tabela ActionLog.")

*Modelo de dados-tabela ActionLog*

A solução fornece uma exibição MVC do ASP.NET para o log de ações que pode ser encontrada em **MvcMusicStores/views/ActionLog**:

![Exibição do log de ações](aspnet-mvc-4-custom-action-filters/_static/image2.png "Exibição do log de ações")

*Exibição do log de ações*

Com essa estrutura determinada, todo o trabalho será focado na solicitação de interrupção do controlador e na execução do registro em log usando a filtragem personalizada.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a>Tarefa 1-Criando um filtro personalizado para capturar a solicitação de um controlador

Nesta tarefa, você criará uma classe de atributo de filtro personalizado que conterá a lógica de log. Para essa finalidade, você estenderá a classe ASP.NET MVC **ActionFilterAttribute** e implementará a interface **IActionFilter**.

> [!NOTE]
> O **ActionFilterAttribute** é a classe base para todos os filtros de atributo. Ele fornece os seguintes métodos para executar uma lógica específica após e antes da execução da ação do controlador:
> 
> - **OnActionExecuting**(ActionExecutingContext filterContext): logo antes do método de ação ser chamado.
> - **OnActionExecuted**(ActionExecutedContext filterContext): depois que o método de ação é chamado e antes do resultado ser executado (antes de exibir o processamento).
> - **OnResultExecuting**(ResultExecutingContext filterContext): logo antes de o resultado ser executado (antes de exibir o processamento).
> - **OnResultExecuted**(ResultExecutedContext filterContext): depois que o resultado é executado (depois que a exibição é renderizada).
> 
> Ao substituir qualquer um desses métodos em uma classe derivada, você pode executar seu próprio código de filtragem.

1. Abra a solução **inicial** localizada na pasta **\Source\Ex01-LoggingActions\Begin** .

   1. Você precisará baixar alguns pacotes NuGet ausentes antes de continuar. Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.
   2. Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, Compile a solução clicando em **build** | **Compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto. É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.
      > 
      > Para obter mais informações, consulte este artigo: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Adicione uma nova C# classe à pasta **filtros** e nomeie-a *CustomActionFilter.cs*. Essa pasta irá armazenar todos os filtros personalizados.
3. Abra **CustomActionFilter.cs** e adicione uma referência aos namespaces **System. Web. Mvc** e **MvcMusicStore. Models** :

    (Trecho de código – *ASP.net de ação personalizada do MVC 4-EX1-CustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. Herde a classe **CustomActionFilter** de **ActionFilterAttribute** e, em seguida, torne a classe **CustomActionFilter** implementar a interface **IActionFilter** .

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. Faça com que a classe **CustomActionFilter** substitua o método **OnActionExecuting** e adicione a lógica necessária para registrar a execução do filtro. Para fazer isso, adicione o seguinte código realçado na classe **CustomActionFilter** .

    (Trecho de código – *ASP.net de ação personalizada do MVC 4-EX1-LoggingActions*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > O método **OnActionExecuting** está usando **Entity Framework** para adicionar um novo registro ActionLog. Ele cria e preenche uma nova instância de entidade com as informações de contexto de **filterContext**.
    > 
    > Você pode ler mais sobre a classe **ControllerContext** no [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a>Tarefa 2-injetando um interceptador de código na classe do controlador de armazenamento

Nesta tarefa, você adicionará o filtro personalizado injetando-o a todas as classes de controlador e ações de controlador que serão registradas em log. Para fins deste exercício, a classe do controlador de loja terá um log.

O método **OnActionExecuting** do filtro personalizado **ActionLogFilterAttribute** é executado quando um elemento injetado é chamado.

Também é possível interceptar um método de controlador específico.

1. Abra o **StoreController** em **MvcMusicStore\Controllers** e adicione uma referência ao namespace de **filtros** :

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. Insira o filtro personalizado **CustomActionFilter** na classe **StoreController** adicionando o atributo **[CustomActionFilter]** antes da declaração de classe.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

   > [!NOTE]
   > Quando um filtro é injetado em uma classe de controlador, todas as suas ações também são injetadas. Se você quiser aplicar o filtro somente para um conjunto de ações, precisará inserir **[CustomActionFilter]** em cada um deles:
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tarefa 3-executando o aplicativo

Nesta tarefa, você testará se o filtro de log está funcionando. Você iniciará o aplicativo e visitará a loja e, em seguida, verificará as atividades registradas.

1. Pressione **F5** para executar o aplicativo.
2. Navegue até **/ActionLog** para ver o estado inicial do modo de exibição de log:

    ![Status do controlador de log antes da atividade da página](aspnet-mvc-4-custom-action-filters/_static/image3.png "Status do controlador de log antes da atividade da página")

    *Status do controlador de log antes da atividade da página*

   > [!NOTE]
   > Por padrão, ele sempre mostrará um item que é gerado ao recuperar os gêneros existentes para o menu.
   > 
   > Para fins de simplicidade, estamos limpando a tabela **ActionLog** cada vez que o aplicativo é executado, portanto, ele mostrará apenas os logs de cada verificação de tarefa específica.
   > 
   > Talvez seja necessário remover o código a seguir da **sessão\_** método de início (na classe **global. asax** ), para salvar um log histórico de todas as ações executadas no controlador da loja.
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. Clique em um dos **gêneros** no menu e execute algumas ações, como navegar por um álbum disponível.
4. Navegue até **/ActionLog** e, se o log estiver vazio, pressione **F5** para atualizar a página. Verifique se suas visitas foram rastreadas:

    ![Log de ação com atividade registrada](aspnet-mvc-4-custom-action-filters/_static/image4.png "Log de ação com atividade registrada")

    *Log de ação com atividade registrada*

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a>Exercício 2: gerenciando vários filtros de ação

Neste exercício, você adicionará um segundo filtro de ação personalizada à classe StoreController e definirá a ordem específica na qual os dois filtros serão executados. Em seguida, você atualizará o código para registrar o filtro globalmente.

Há diferentes opções a serem levadas em conta ao definir a ordem de execução dos filtros. Por exemplo, a propriedade Order e o escopo dos filtros:

Você pode definir um **escopo** para cada um dos filtros, por exemplo, você pode fazer o escopo de todos os filtros de ação a serem executados dentro do **escopo do controlador**e todos os filtros de autorização para serem executados no **escopo global**. Os escopos têm uma ordem de execução definida.

Além disso, cada filtro de ação tem uma propriedade Order, que é usada para determinar a ordem de execução no escopo do filtro.

Para obter mais informações sobre a ordem de execução de filtros de ação personalizada, visite este artigo do MSDN: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a>Tarefa 1: Criando um novo filtro de ação personalizado

Nesta tarefa, você criará um novo filtro de ação personalizado para injetar na classe StoreController, aprendendo a gerenciar a ordem de execução dos filtros.

1. Abra a solução **inicial** localizada na pasta **\Source\Ex02-ManagingMultipleActionFilters\Begin** . Caso contrário, você pode continuar usando a solução **final** obtida concluindo o exercício anterior.

    1. Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar. Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.
    2. Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.
    3. Por fim, Compile a solução clicando em **build** | **Compilar solução**.

        > [!NOTE]
        > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto. É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.
        > 
        > Para obter mais informações, consulte este artigo: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Adicione uma nova C# classe à pasta **filtros** e nomeie-a *MyNewCustomActionFilter.cs*
3. Abra **MyNewCustomActionFilter.cs** e adicione uma referência a **System. Web. Mvc** e ao namespace **MvcMusicStore. Models** :

    (Trecho de código – *ASP.net de ação personalizada do MVC 4-EX2-MyNewCustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. Substitua a declaração de classe padrão pelo código a seguir.

    (Trecho de código – *ASP.net de ação personalizada do MVC 4-EX2-MyNewCustomActionFilterClass*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > Esse filtro de ação personalizada é quase igual ao que você criou no exercício anterior. A principal diferença é que ele tem o *&quot;registrado pelo atributo&quot;* atualizado com esse novo nome de classe para identificar qual filtro registrou o log.

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a>Tarefa 2: injetando um novo interceptador de código na classe StoreController

Nesta tarefa, você adicionará um novo filtro personalizado à classe StoreController e executará a solução para verificar como os dois filtros funcionam juntos.

1. Abra a classe **StoreController** localizada em **MvcMusicStore\Controllers** e insira o novo filtro personalizado **MyNewCustomActionFilter** na classe **StoreController** , como é mostrado no código a seguir.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. Agora, execute o aplicativo para ver como esses dois filtros de ação personalizados funcionam. Para fazer isso, pressione **F5** e aguarde até que o aplicativo seja iniciado.
3. Navegue até **/ActionLog** para ver o estado inicial da exibição de log.

    ![Status do controlador de log antes da atividade da página](aspnet-mvc-4-custom-action-filters/_static/image5.png "Status do controlador de log antes da atividade da página")

    *Status do controlador de log antes da atividade da página*
4. Clique em um dos **gêneros** no menu e execute algumas ações, como navegar por um álbum disponível.
5. Verifique esta hora; suas visitas foram rastreadas duas vezes: uma vez para cada um dos filtros de ação personalizada que você adicionou na classe **StorageController** .

    ![Log de ação com atividade registrada](aspnet-mvc-4-custom-action-filters/_static/image6.png "Log de ação com atividade registrada")

    *Log de ação com atividade registrada*
6. Feche o navegador.

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a>Tarefa 3: Gerenciando a ordenação de filtro

Nesta tarefa, você aprenderá a gerenciar a ordem de execução dos filtros usando a propriedade Order.

1. Abra a classe **StoreController** localizada em **MvcMusicStore\Controllers** e especifique a propriedade **Order** em ambos os filtros, como mostrado abaixo.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. Agora, verifique como os filtros são executados, dependendo do valor da propriedade Order. Você verá que o filtro com o menor valor de pedido (**CustomActionFilter**) é o primeiro que é executado. Pressione **F5** e aguarde até que o aplicativo seja iniciado.
3. Navegue até **/ActionLog** para ver o estado inicial da exibição de log.

    ![Status do controlador de log antes da atividade da página](aspnet-mvc-4-custom-action-filters/_static/image7.png "Status do controlador de log antes da atividade da página")

    *Status do controlador de log antes da atividade da página*
4. Clique em um dos **gêneros** no menu e execute algumas ações, como navegar por um álbum disponível.
5. Verifique se, desta vez, suas visitas foram rastreadas ordenadas pelo valor de ordem dos filtros: **CustomActionFilter** logs ' primeiro.

    ![Log de ação com atividade registrada](aspnet-mvc-4-custom-action-filters/_static/image8.png "Log de ação com atividade registrada")

    *Log de ação com atividade registrada*
6. Agora, você atualizará o valor da ordem dos filtros e verificará como a ordem de registro em log é alterada. Na classe **StoreController** , atualize o valor de ordem dos filtros, como mostrado abaixo.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. Execute o aplicativo novamente pressionando **F5**.
8. Clique em um dos **gêneros** no menu e execute algumas ações, como navegar por um álbum disponível.
9. Verifique se, desta vez, os logs criados pelo filtro **MyNewCustomActionFilter** aparecem primeiro.

    ![Log de ação com atividade registrada](aspnet-mvc-4-custom-action-filters/_static/image9.png "Log de ação com atividade registrada")

    *Log de ação com atividade registrada*

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a>Tarefa 4: registrando filtros globalmente

Nesta tarefa, você atualizará a solução para registrar o novo filtro (**MyNewCustomActionFilter**) como um filtro global. Ao fazer isso, ele será disparado por todas as ações executadas no aplicativo e não apenas nos StoreControllers como na tarefa anterior.

1. Na classe **StoreController** , remova o atributo **[MyNewCustomActionFilter]** e a propriedade Order de **[CustomActionFilter]** . Sua tela deverá ter a seguinte aparência:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. Abra o arquivo **global. asax** e localize o **aplicativo\_método iniciar** . Observe que sempre que o aplicativo é iniciado, ele está registrando os filtros globais chamando o método **RegisterGlobalFilters** na classe **FilterConfig** .

    ![Registrando filtros globais no global. asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "Registrando filtros globais no global. asax")

    *Registrando filtros globais no global. asax*
3. Abra o arquivo **FilterConfig.cs** no **aplicativo\_pasta inicial** .
4. Adicione uma referência ao usando System. Web. Mvc; usando MvcMusicStore. Filters; namespace.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. Atualize o método **RegisterGlobalFilters** adicionando seu filtro personalizado. Para fazer isso, adicione o código realçado:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. Execute o aplicativo pressionando **F5**.
7. Clique em um dos **gêneros** no menu e execute algumas ações, como navegar por um álbum disponível.
8. Verifique se agora **[MyNewCustomActionFilter]** está sendo injetado em HomeController e ActionLogController também.

    ![Log de ação com atividade registrada](aspnet-mvc-4-custom-action-filters/_static/image11.png "Log de ação com atividade registrada")

    *Log de ação com atividade global registrada*

> [!NOTE]
> Além disso, você pode implantar esse aplicativo em sites do Windows Azure seguindo [o apêndice B: publicando um aplicativo ASP.NET MVC 4 usando implantação da Web](#AppendixB).

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Resumo

Ao concluir este laboratório prático, você aprendeu a estender um filtro de ação para executar ações personalizadas. Você também aprendeu como injetar qualquer filtro em seus controladores de página. Os seguintes conceitos foram usados:

- Como criar filtros de ação personalizados com a classe ASP.NET MVC ActionFilterAttribute
- Como injetar filtros em controladores MVC do ASP.NET
- Como gerenciar a ordenação de filtro usando a propriedade Order
- Como registrar filtros globalmente

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apêndice A: Instalando o Visual Studio Express 2012 para Web

Você pode instalar o **Microsoft Visual Studio Express 2012 para Web** ou outra versão &quot;Express&quot; usando o **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** . As instruções a seguir orientam você pelas etapas necessárias para instalar o *Visual Studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.

1. Ir para [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169). Como alternativa, se você já tiver instalado o Web Platform Installer, poderá abri-lo e pesquisar o produto &quot;<em>Visual Studio Express 2012 para Web com o SDK do Windows Azure</em>&quot;.
2. Clique em **instalar agora**. Se você não tiver **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.
3. Quando **Web Platform Installer** estiver aberto, clique em **instalar** para iniciar a instalação.

    ![Instalar Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "Instalar Visual Studio Express")

    *Instalar Visual Studio Express*
4. Leia todos os termos e licenças de produtos e clique em **aceito** para continuar.

    ![Aceitando os termos de licença](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    *Aceitando os termos de licença*
5. Aguarde até que o processo de download e instalação seja concluído.

    ![Progresso da instalação](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    *Progresso da instalação*
6. Quando a instalação for concluída, clique em **concluir**.

    ![Instalação concluída](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    *Instalação concluída*
7. Clique em **sair** para fechar Web Platform Installer.
8. Para abrir o Visual Studio Express para Web, vá para a tela **Iniciar** e comece a escrever &quot;**vs Express**&quot;e, em seguida, clique no bloco **vs Express para Web** .

    ![Bloco VS Express para Web](aspnet-mvc-4-custom-action-filters/_static/image16.png)

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

    ![Fazer logon no Windows portal do Azure](aspnet-mvc-4-custom-action-filters/_static/image17.png "Fazer logon no Windows portal do Azure")

    *Fazer logon no Windows Azure Portal de Gerenciamento*
2. Clique em **novo** na barra de comandos.

    ![Criando um novo site](aspnet-mvc-4-custom-action-filters/_static/image18.png "Criando um novo site")

    *Criando um novo site*
3. Clique em **computação** | **site**. Em seguida, selecione a opção **criação rápida** . Forneça uma URL disponível para o novo site e clique em **criar site**.

    > [!NOTE]
    > Um site do Windows Azure é o host de um aplicativo Web em execução na nuvem que você pode controlar e gerenciar. A opção criação rápida permite que você implante um aplicativo Web concluído no site do Windows Azure de fora do Portal. Ele não inclui etapas para configurar um banco de dados.

    ![Criando um novo site usando a criação rápida](aspnet-mvc-4-custom-action-filters/_static/image19.png "Criando um novo site usando a criação rápida")

    *Criando um novo site usando a criação rápida*
4. Aguarde até que o novo **site** seja criado.
5. Depois que o site for criado, clique no link sob a coluna **URL** . Verifique se o novo site está funcionando.

    ![Navegando até o novo site](aspnet-mvc-4-custom-action-filters/_static/image20.png "Navegando até o novo site")

    *Navegando até o novo site*

    ![Site em execução](aspnet-mvc-4-custom-action-filters/_static/image21.png "Site em execução")

    *Site em execução*
6. Volte para o portal e clique no nome do site na coluna **nome** para exibir as páginas de gerenciamento.

    ![Abrindo as páginas de gerenciamento de site](aspnet-mvc-4-custom-action-filters/_static/image22.png "Abrindo as páginas de gerenciamento de site")

    *Abrindo as páginas de gerenciamento de site*
7. Na página **painel** , na seção **visão rápida** , clique no link **baixar perfil de publicação** .

    > [!NOTE]
    > O *perfil de publicação* contém todas as informações necessárias para publicar um aplicativo Web em um site do Windows Azure para cada método de publicação habilitado. O perfil de publicação contém as URLs, as credenciais de usuário e as cadeias de conexão de banco de dados necessárias para conectar-se e autenticar cada um dos pontos de extremidade para os quais um método de publicação é habilitado. **O Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** e **Microsoft Visual Studio 2012** dão suporte à leitura de perfis de publicação para automatizar a configuração desses programas para a publicação de aplicativos Web em sites do Windows Azure.

    ![Baixando o perfil de publicação do site](aspnet-mvc-4-custom-action-filters/_static/image23.png "Baixando o perfil de publicação do site")

    *Baixando o perfil de publicação do site*
8. Baixe o arquivo de perfil de publicação em um local conhecido. Neste exercício, você verá como usar esse arquivo para publicar um aplicativo Web em um site do Windows Azure a partir do Visual Studio.

    ![Salvando o arquivo de perfil de publicação](aspnet-mvc-4-custom-action-filters/_static/image24.png "Salvando o perfil de publicação")

    *Salvando o arquivo de perfil de publicação*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tarefa 2-Configurando o servidor de banco de dados

Se seu aplicativo utiliza bancos de dados SQL Server, você precisará criar um servidor de banco de dados SQL. Se você quiser implantar um aplicativo simples que não usa SQL Server você pode ignorar essa tarefa.

1. Você precisará de um servidor do banco de dados SQL para armazenar o banco de dados do aplicativo. Você pode exibir os servidores do banco de dados SQL de sua assinatura no portal de gerenciamento do Windows Azure em bancos de dados **sql** | **servidores** | **painel do servidor**. Se você não tiver um servidor criado, poderá criar um usando o botão **Adicionar** na barra de comandos. Anote o nome do **servidor e a URL, o nome de logon e a senha do administrador**, pois você irá usá-los nas próximas tarefas. Não crie o banco de dados ainda, pois ele será criado em um estágio posterior.

    ![Painel do servidor do banco de dados SQL](aspnet-mvc-4-custom-action-filters/_static/image25.png "Painel do servidor do banco de dados SQL")

    *Painel do servidor do banco de dados SQL*
2. Na próxima tarefa, você testará a conexão de banco de dados do Visual Studio, por esse motivo, será necessário incluir o endereço IP local na lista de **endereços IP permitidos**do servidor. Para fazer isso, clique em **Configurar**, selecione o endereço IP do **endereço IP do cliente atual** e cole-o nas caixas de texto endereço IP **inicial** e **endereço IP final** e clique no botão ![adicionar-cliente-IP-endereço-OK-botão](aspnet-mvc-4-custom-action-filters/_static/image26.png).

    ![Adicionando endereço IP do cliente](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    *Adicionando endereço IP do cliente*
3. Depois que o **endereço IP do cliente** for adicionado à lista endereços IP permitidos, clique em **salvar** para confirmar as alterações.

    ![Confirmar alterações](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    *Confirmar alterações*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tarefa 3-publicando um aplicativo ASP.NET MVC 4 usando Implantação da Web

1. Volte para a solução ASP.NET MVC 4. Na **Gerenciador de soluções**, clique com o botão direito do mouse no projeto de site e selecione **publicar**.

    ![Publicando o aplicativo](aspnet-mvc-4-custom-action-filters/_static/image29.png "Publicando o aplicativo")

    *Publicando o site*
2. Importe o perfil de publicação salvo na primeira tarefa.

    ![Importando o perfil de publicação](aspnet-mvc-4-custom-action-filters/_static/image30.png "Importando o perfil de publicação")

    *Importando perfil de publicação*
3. Clique em **validar conexão**. Depois que a validação for concluída, clique em **Avançar**.

    > [!NOTE]
    > A validação será concluída quando você vir uma marca de seleção verde ao lado do botão Validar conexão.

    ![Validando conexão](aspnet-mvc-4-custom-action-filters/_static/image31.png "Validando conexão")

    *Validando conexão*
4. Na página **configurações** , na seção **bancos** de dados, clique no botão ao lado da caixa de texto da sua conexão de banco (ou seja, **DefaultConnection**).

    ![Configuração de implantação da Web](aspnet-mvc-4-custom-action-filters/_static/image32.png "Configuração de implantação da Web")

    *Configuração de implantação da Web*
5. Configure a conexão de banco de dados da seguinte maneira:

   - No **nome do servidor** , digite a URL do servidor do banco de dados SQL usando o prefixo *TCP:* .
   - Em **nome de usuário** , digite o nome de logon do administrador do servidor.
   - Em **senha** , digite a senha de logon do administrador do servidor.
   - Digite um novo nome de banco de dados.

     ![Configurando a cadeia de conexão de destino](aspnet-mvc-4-custom-action-filters/_static/image33.png "Configurando a cadeia de conexão de destino")

     *Configurando a cadeia de conexão de destino*
6. Em seguida, clique em **OK**. Quando for solicitado a criar o banco de dados, clique em **Sim**.

    ![Criando o banco de dados](aspnet-mvc-4-custom-action-filters/_static/image34.png "Criando a cadeia de caracteres do banco de dados")

    *Criando o banco de dados*
7. A cadeia de conexão que você usará para se conectar ao banco de dados SQL no Windows Azure é mostrada na caixa de texto conexão padrão. Em seguida, clique em **Próximo**.

    ![Cadeia de conexão apontando para o banco de dados SQL](aspnet-mvc-4-custom-action-filters/_static/image35.png "Cadeia de conexão apontando para o banco de dados SQL")

    *Cadeia de conexão apontando para o banco de dados SQL*
8. Na página **Visualização** , clique em **publicar**.

    ![Publicando o aplicativo Web](aspnet-mvc-4-custom-action-filters/_static/image36.png "Publicando o aplicativo Web")

    *Publicando o aplicativo Web*
9. Quando o processo de publicação for concluído, o navegador padrão abrirá o site publicado.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Apêndice C: usando trechos de código

Com trechos de código, você tem todo o código de que precisa ao seu alcance. O documento de laboratório informará exatamente quando você pode usá-los, conforme mostrado na figura a seguir.

![Usando trechos de código do Visual Studio para inserir código em seu projeto](aspnet-mvc-4-custom-action-filters/_static/image37.png "Usando trechos de código do Visual Studio para inserir código em seu projeto")

*Usando trechos de código do Visual Studio para inserir código em seu projeto*

***Para adicionar um trecho de código usando o tecladoC# (somente)***

1. Coloque o cursor onde você deseja inserir o código.
2. Comece digitando o nome do trecho de código (sem espaços ou hifens).
3. Observe como o IntelliSense exibe nomes de trechos de código correspondentes.
4. Selecione o trecho correto (ou continue digitando até que o nome do trecho inteiro seja selecionado).
5. Pressione a tecla TAB duas vezes para inserir o trecho de código no local do cursor.

![Comece a digitar o nome do trecho](aspnet-mvc-4-custom-action-filters/_static/image38.png "Comece a digitar o nome do trecho")

*Comece a digitar o nome do trecho*

![Pressione Tab para selecionar o trecho realçado](aspnet-mvc-4-custom-action-filters/_static/image39.png "Pressione Tab para selecionar o trecho realçado")

*Pressione Tab para selecionar o trecho realçado*

![Pressione Tab novamente e o trecho será expandido](aspnet-mvc-4-custom-action-filters/_static/image40.png "Pressione Tab novamente e o trecho será expandido")

*Pressione Tab novamente e o trecho será expandido*

***Para adicionar um trecho de código usando o mouseC#(, Visual Basic e XML)*** uma. Clique com o botão direito do mouse no local em que você deseja inserir o trecho de código.

1. Selecione **Inserir trecho** seguido por **meus trechos de código**.
2. Selecione o trecho relevante na lista clicando nele.

![Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho](aspnet-mvc-4-custom-action-filters/_static/image41.png "Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho")

*Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho*

![Selecione o trecho relevante na lista clicando nele](aspnet-mvc-4-custom-action-filters/_static/image42.png "Selecione o trecho relevante na lista clicando nele")

*Selecione o trecho relevante na lista clicando nele*
