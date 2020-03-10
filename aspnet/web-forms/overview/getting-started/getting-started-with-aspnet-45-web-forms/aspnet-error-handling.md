---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
title: Tratamento de erro ASP.NET | Microsoft Docs
author: Erikre
description: Esta série de tutoriais ensinará as noções básicas da criação de um aplicativo ASP.NET Web Forms usando o ASP.NET 4,5 e o Microsoft Visual Studio Express 2013 para nós...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 423498f7-1a4b-44a1-b342-5f39d0bcf94f
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
msc.type: authoredcontent
ms.openlocfilehash: 9514142ca50b33470a3f4c033e4f8e319a9ee09b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78566678"
---
# <a name="aspnet-error-handling"></a>Tratamento de erro do ASP.NET

por [Erik Reitan](https://github.com/Erikre)

[Baixar o projeto de exemplo WingtipC#Toys ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [baixar o livro eletrônico (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta série de tutoriais ensinará as noções básicas da criação de um aplicativo ASP.NET Web Forms usando o ASP.NET 4,5 e o Microsoft Visual Studio Express 2013 para a Web. Um [projeto Visual Studio 2013 com C# código-fonte](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) está disponível para acompanhar esta série de tutoriais.

Neste tutorial, você modificará o aplicativo de exemplo Wingtip Toys para incluir o tratamento de erros e o log de erros. O tratamento de erros permitirá que o aplicativo manipule erros e exiba mensagens de erro de forma adequada. O log de erros permitirá que você localize e corrija os erros que ocorreram. Este tutorial se baseia no tutorial anterior "roteamento de URL" e faz parte da série de tutoriais da Wingtip Toys.

## <a name="what-youll-learn"></a>O que você aprenderá:

- Como adicionar tratamento de erro global à configuração do aplicativo.
- Como adicionar tratamento de erros nos níveis de aplicativo, página e código.
- Como registrar em log erros para revisão posterior.
- Como exibir mensagens de erro que não comprometem a segurança.
- Como implementar o log de erros de módulos e manipuladores de log de erros (ELMAH).

## <a name="overview"></a>Visão geral

Os aplicativos ASP.NET devem ser capazes de lidar com erros que ocorrem durante a execução de maneira consistente. O ASP.NET usa o Common Language Runtime (CLR), que fornece uma maneira de notificar aplicativos de erros de maneira uniforme. Quando ocorre um erro, uma exceção é lançada. Uma exceção é qualquer erro, condição ou comportamento inesperado que um aplicativo encontra.

Na .NET Framework, uma exceção é um objeto herdado da classe `System.Exception`. Uma exceção é lançada de uma área do código em que ocorreu um problema. A exceção é transmitida à pilha de chamadas para um local onde o aplicativo fornece código para manipular a exceção. Se o aplicativo não tratar a exceção, o navegador será forçado a exibir os detalhes do erro.

Como prática recomendada, trate os erros no no nível de código em `Try`/`Catch`/`Finally` blocos dentro de seu código. Tente posicionar esses blocos para que o usuário possa corrigir problemas no contexto em que eles ocorrem. Se os blocos de tratamento de erros estiverem muito longe de onde o erro ocorreu, será mais difícil fornecer aos usuários as informações de que eles precisam para corrigir o problema.

### <a name="exception-class"></a>Classe Exception

A classe de exceção é a classe base da qual as exceções herdam. A maioria dos objetos de exceção são instâncias de alguma classe derivada da classe Exception, como a classe `SystemException`, a classe `IndexOutOfRangeException` ou a classe `ArgumentNullException`. A classe de exceção tem propriedades, como a propriedade `StackTrace`, a propriedade `InnerException` e a propriedade `Message`, que fornecem informações específicas sobre o erro que ocorreu.

### <a name="exception-inheritance-hierarchy"></a>Hierarquia de herança de exceção

O tempo de execução tem um conjunto base de exceções derivadas da classe `SystemException` que o tempo de execução gera quando uma exceção é encontrada. A maioria das classes que herdam da classe de exceção, como a classe `IndexOutOfRangeException` e a classe `ArgumentNullException`, não implementam membros adicionais. Portanto, as informações mais importantes para uma exceção podem ser encontradas na hierarquia de exceções, no nome da exceção e nas informações contidas na exceção.

### <a name="exception-handling-hierarchy"></a>Hierarquia de tratamento de exceção

Em um aplicativo de Web Forms ASP.NET, exceções podem ser manipuladas com base em uma hierarquia de manipulação específica. Uma exceção pode ser tratada nos seguintes níveis:

- Nível de aplicativo
- Nível de página
- Nível de código

Quando um aplicativo manipula exceções, as informações adicionais sobre a exceção herdadas da classe de exceção geralmente podem ser recuperadas e exibidas para o usuário. Além do aplicativo, da página e do nível de código, você também pode tratar exceções no nível do módulo HTTP e usando um manipulador personalizado do IIS.

### <a name="application-level-error-handling"></a>Tratamento de erros no nível do aplicativo

Você pode manipular erros padrão no nível do aplicativo modificando a configuração do aplicativo ou adicionando um manipulador de `Application_Error` no arquivo *global. asax* do seu aplicativo.

Você pode manipular erros padrão e erros de HTTP adicionando uma seção `customErrors` ao arquivo *Web. config* . A seção `customErrors` permite que você especifique uma página padrão para a qual os usuários serão redirecionados quando ocorrer um erro. Ele também permite que você especifique páginas individuais para erros de código de status específicos.

[!code-xml[Main](aspnet-error-handling/samples/sample1.xml?highlight=3-5)]

Infelizmente, ao usar a configuração para redirecionar o usuário para uma página diferente, você não tem os detalhes do erro ocorrido.

No entanto, você pode interceptar erros que ocorrem em qualquer lugar em seu aplicativo adicionando código ao manipulador de `Application_Error` no arquivo *global. asax* .

[!code-csharp[Main](aspnet-error-handling/samples/sample2.cs)]

### <a name="page-level-error-event-handling"></a>Manipulação de eventos de erro no nível da página

Um manipulador de nível de página retorna o usuário para a página em que o erro ocorreu, mas como as instâncias de controles não são mantidas, não haverá mais nada na página. Para fornecer os detalhes do erro para o usuário do aplicativo, você deve gravar especificamente os detalhes do erro na página.

Normalmente, você usaria um manipulador de erros de nível de página para registrar erros sem tratamento ou para levar o usuário a uma página que pode exibir informações úteis.

Este exemplo de código mostra um manipulador para o evento de erro em uma página da Web ASP.NET. Esse manipulador captura todas as exceções que ainda não foram tratadas em `try`/`catch` blocos na página.

[!code-csharp[Main](aspnet-error-handling/samples/sample3.cs)]

Depois de manipular um erro, você deve limpá-lo chamando o método `ClearError` do objeto Server (`HttpServerUtility` classe), caso contrário, verá um erro que ocorreu anteriormente.

### <a name="code-level-error-handling"></a>Tratamento de erros de nível de código

A instrução try-catch consiste em um bloco try seguido por uma ou mais cláusulas catch, que especificam manipuladores para exceções diferentes. Quando uma exceção é lançada, o Common Language Runtime (CLR) procura a instrução catch que manipula essa exceção. Se o método atualmente em execução não contiver um bloco catch, o CLR examinará o método que chamou o método atual e assim por diante, a pilha de chamadas. Se nenhum bloco catch for encontrado, o CLR exibirá uma mensagem de exceção sem tratamento para o usuário e interromperá a execução do programa.

O exemplo de código a seguir mostra uma maneira comum de usar `try`/`catch`/`finally` para lidar com erros.

[!code-csharp[Main](aspnet-error-handling/samples/sample4.cs)]

No código acima, o bloco try contém o código que precisa ser protegido contra uma possível exceção. O bloco é executado até que uma exceção seja lançada ou o bloco seja concluído com êxito. Se uma exceção de `FileNotFoundException` ou uma exceção de `IOException` ocorrer, a execução será transferida para uma página diferente. Em seguida, o código contido no bloco finally é executado, independentemente de um erro ter ocorrido ou não.

## <a name="adding-error-logging-support"></a>Adicionando suporte ao log de erros

Antes de adicionar o tratamento de erros ao aplicativo de exemplo Wingtip Toys, você adicionará suporte ao log de erros adicionando uma classe de `ExceptionUtility` à pasta *lógica* . Ao fazer isso, sempre que o aplicativo tratar um erro, os detalhes do erro serão adicionados ao arquivo de log de erros.

1. Clique com o botão direito do mouse na pasta *lógica* e selecione **Adicionar** -&gt; **novo item**.   
   A caixa de diálogo **Adicionar novo item** é exibida.
2. Selecione o grupo de modelos de **código** do **Visual C#**  -&gt; à esquerda. Em seguida, selecione **classe**na lista intermediária e nomeie-a **ExceptionUtility.cs**.
3. Escolha **Adicionar**. O novo arquivo de classe é exibido.
4. Substitua o código existente pelo seguinte:  

    [!code-csharp[Main](aspnet-error-handling/samples/sample5.cs)]

Quando ocorre uma exceção, a exceção pode ser gravada em um arquivo de log de exceção chamando o método `LogException`. Esse método usa dois parâmetros, o objeto de exceção e uma cadeia de caracteres que contém detalhes sobre a origem da exceção. O log de exceções é gravado no arquivo cafiler *. txt* na pasta de *dados do\_de aplicativos* .

### <a name="adding-an-error-page"></a>Adicionando uma página de erro

No aplicativo de exemplo Wingtip Toys, uma página será usada para exibir erros. A página de erro foi projetada para mostrar uma mensagem de erro segura para os usuários do site. No entanto, se o usuário for um desenvolvedor que faz uma solicitação HTTP que está sendo servida localmente no computador onde o código reside, detalhes adicionais do erro serão exibidos na página de erro.

1. Clique com o botão direito do mouse no nome do projeto (**Wingtip Toys**) em **Gerenciador de soluções** e selecione **Adicionar** -&gt; **novo item**.   
   A caixa de diálogo **Adicionar novo item** é exibida.
2. Selecione o grupo modelos **da Web** do **Visual C#**  -&gt; à esquerda. Na lista intermediária, selecione **Web Form com página mestra**e nomeie-o **ErrorPage. aspx**.
3. Clique em **Adicionar**.
4. Selecione o arquivo *site. Master* como a página mestra e escolha **OK**.
5. Substitua a marcação existente pelo seguinte:   

    [!code-aspx[Main](aspnet-error-handling/samples/sample6.aspx)]
6. Substitua o código existente do code-behind (*ErrorPage.aspx.cs*) para que ele apareça da seguinte maneira:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample7.cs)]

Quando a página de erro é exibida, o manipulador de eventos `Page_Load` é executado. No manipulador de `Page_Load`, o local onde o erro foi manipulado pela primeira vez é determinado. Em seguida, o último erro que ocorreu é determinado pela chamada ao método `GetLastError` do objeto Server. Se a exceção não existir mais, uma exceção genérica será criada. Em seguida, se a solicitação HTTP tiver sido feita localmente, todos os detalhes do erro serão mostrados. Nesse caso, somente o computador local que executa o aplicativo Web verá esses detalhes de erro. Depois que as informações de erro forem exibidas, o erro será adicionado ao arquivo de log e o erro será removido do servidor.

### <a name="displaying-unhandled-error-messages-for-the-application"></a>Exibindo mensagens de erro sem tratamento para o aplicativo

Ao adicionar uma seção de `customErrors` ao arquivo *Web. config* , você pode manipular rapidamente erros simples que ocorrem em todo o aplicativo. Você também pode especificar como tratar erros com base em seu valor de código de status, como 404-arquivo não encontrado.

#### <a name="update-the-configuration"></a>Atualizar a configuração

Atualize a configuração adicionando uma seção `customErrors` ao arquivo *Web. config* .

1. Em **Gerenciador de soluções**, localize e abra o arquivo *Web. config* na raiz do aplicativo de exemplo Wingtip Toys.
2. Adicione a seção `customErrors` ao arquivo *Web. config* dentro do nó `<system.web>` da seguinte maneira:   

    [!code-xml[Main](aspnet-error-handling/samples/sample8.xml?highlight=3-5)]
3. Salve o arquivo *Web.config* .

A seção `customErrors` especifica o modo, que é definido como "on". Ele também especifica o `defaultRedirect`, que informa ao aplicativo em qual página navegar quando ocorre um erro. Além disso, você adicionou um elemento error específico que especifica como tratar um erro 404 quando uma página não é encontrada. Posteriormente neste tutorial, você adicionará tratamento de erro adicional que capturará os detalhes de um erro no nível do aplicativo.

#### <a name="running-the-application"></a>Executando o aplicativo

Você pode executar o aplicativo agora para ver as rotas atualizadas.

1. Pressione **F5** para executar o aplicativo de exemplo Wingtip Toys.  
 O navegador é aberto e mostra a página *Default. aspx* .
2. Insira a URL a seguir no navegador (certifique-se de usar **o número da** porta):  
    `https://localhost:44300/NoPage.aspx`
3. Examine o *ErrorPage. aspx* exibido no navegador. 

    ![Tratamento de erro ASP.NET-erro de página não encontrada](aspnet-error-handling/_static/image1.png)

Quando você solicitar a página *noPage. aspx* , que não existe, a página de erro mostrará a mensagem de erro simples e as informações de erro detalhadas se houver detalhes adicionais disponíveis. No entanto, se o usuário solicitou uma página não existente de um local remoto, a página de erro mostraria apenas a mensagem de erro em vermelho.

### <a name="including-an-exception-for-testing-purposes"></a>Incluindo uma exceção para fins de teste

Para verificar como seu aplicativo funcionará quando ocorrer um erro, você pode criar deliberadamente condições de erro em ASP.NET. No aplicativo de exemplo Wingtip Toys, você gerará uma exceção de teste quando a página padrão for carregada para ver o que acontece.

1. Abra o code-behind da página *Default. aspx* no Visual Studio.   
   A página code-behind *Default.aspx.cs* será exibida.
2. No manipulador de `Page_Load`, adicione o código para que o manipulador seja exibido da seguinte maneira:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample9.cs?highlight=3-4)]

É possível criar vários tipos diferentes de exceções. No código acima, você está criando um `InvalidOperationException` quando a página *Default. aspx* é carregada.

#### <a name="running-the-application"></a>Executando o aplicativo

Você pode executar o aplicativo para ver como o aplicativo trata a exceção.

1. Pressione **Ctrl + F5** para executar o aplicativo de exemplo Wingtip Toys.  
 O aplicativo gera a InvalidOperationException. 

    > [!NOTE] 
    > 
    > Você deve pressionar **Ctrl + F5** para exibir a página sem dividir o código para exibir a origem do erro no Visual Studio.
2. Examine o *ErrorPage. aspx* exibido no navegador. 

    ![Tratamento de erro ASP.NET-página de erro](aspnet-error-handling/_static/image2.png)

Como você pode ver nos detalhes do erro, a exceção foi interceptada pela seção `customError` no arquivo *Web. config* .

### <a name="adding-application-level-error-handling"></a>Adicionando tratamento de erros no nível do aplicativo

Em vez de interceptar a exceção usando a seção `customErrors` no arquivo *Web. config* , no qual você obterá poucas informações sobre a exceção, você pode interceptar o erro no nível do aplicativo e recuperar os detalhes do erro.

1. Em **Gerenciador de soluções**, localize e abra o arquivo *global.asax.cs* .
2. Adicione um **aplicativo\_** manipulador de erro para que ele apareça da seguinte maneira:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample10.cs)]

Quando ocorre um erro no aplicativo, o manipulador de `Application_Error` é chamado. Nesse manipulador, a última exceção é recuperada e revisada. Se a exceção não foi tratada e a exceção contém detalhes da exceção interna (ou seja, `InnerException` não é nulo), o aplicativo transfere a execução para a página de erro onde os detalhes da exceção são exibidos.

#### <a name="running-the-application"></a>Executando o aplicativo

Você pode executar o aplicativo para ver os detalhes adicionais do erro fornecidos pelo tratamento da exceção no nível do aplicativo.

1. Pressione **Ctrl + F5** para executar o aplicativo de exemplo Wingtip Toys.  
 O aplicativo gera o `InvalidOperationException`.
2. Examine o *ErrorPage. aspx* exibido no navegador. 

    ![Tratamento de erros ASP.NET-erro no nível do aplicativo](aspnet-error-handling/_static/image3.png)

### <a name="adding-page-level-error-handling"></a>Adicionando tratamento de erros no nível da página

Você pode adicionar o tratamento de erros no nível da página a uma página usando a adição de um atributo `ErrorPage` à diretiva `@Page` da página ou adicionando um manipulador de eventos `Page_Error` ao code-behind de uma página. Nesta seção, você adicionará um manipulador de eventos `Page_Error` que irá transferir a execução para a página *ErrorPage. aspx* .

1. Em **Gerenciador de soluções**, localize e abra o arquivo *Default.aspx.cs* .
2. Adicione um manipulador de `Page_Error` para que o code-behind seja exibido da seguinte maneira:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample11.cs?highlight=18-30)]

Quando ocorre um erro na página, o manipulador de eventos `Page_Error` é chamado. Nesse manipulador, a última exceção é recuperada e revisada. Se ocorrer um `InvalidOperationException`, o manipulador de eventos `Page_Error` transfere a execução para a página de erro onde os detalhes da exceção são exibidos.

#### <a name="running-the-application"></a>Executando o aplicativo

Você pode executar o aplicativo agora para ver as rotas atualizadas.

1. Pressione **Ctrl + F5** para executar o aplicativo de exemplo Wingtip Toys.  
 O aplicativo gera o `InvalidOperationException`.
2. Examine o *ErrorPage. aspx* exibido no navegador. 

    ![Tratamento de erro ASP.NET-erro no nível da página](aspnet-error-handling/_static/image4.png)
3. Feche a janela do navegador.

### <a name="removing-the-exception-used-for-testing"></a>Removendo a exceção usada para teste

Para permitir que o aplicativo de exemplo Wingtip Toys funcione sem lançar a exceção que você adicionou anteriormente neste tutorial, remova a exceção.

1. Abra o code-behind da página *Default. aspx* .
2. No manipulador de `Page_Load`, remova o código que gera a exceção para que o manipulador seja exibido da seguinte maneira:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample12.cs)]

### <a name="adding-code-level-error-logging"></a>Adicionando log de erros no nível de código

Conforme mencionado anteriormente neste tutorial, você pode adicionar instruções Try/Catch para tentar executar uma seção de código e manipular o primeiro erro que ocorre. Neste exemplo, você só irá gravar os detalhes do erro no arquivo de log de erros para que o erro possa ser revisado posteriormente.

1. Em **Gerenciador de soluções**, na pasta *lógica* , localize e abra o arquivo *PayPalFunctions.cs* .
2. Atualize o método `HttpCall` para que o código seja exibido da seguinte maneira:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample13.cs?highlight=20,22-23)]

O código acima chama o método `LogException` que está contido na classe `ExceptionUtility`. Você adicionou o arquivo da classe *ExceptionUtility.cs* à pasta *lógica* anteriormente neste tutorial. O método `LogException` utiliza dois parâmetros. O primeiro parâmetro é o objeto de exceção. O segundo parâmetro é uma cadeia de caracteres usada para reconhecer a origem do erro.

### <a name="inspecting-the-error-logging-information"></a>Inspecionando as informações de log de erros

Conforme mencionado anteriormente, você pode usar o log de erros para determinar quais erros em seu aplicativo devem ser corrigidos primeiro. É claro que apenas os erros que foram interceptados e gravados no log de erros serão registrados.

1. Em **Gerenciador de soluções**, localize e abra o arquivo log *. txt* na pasta *\_dados do aplicativo* .   
 Talvez seja necessário selecionar a opção "**Mostrar todos os arquivos**" ou a opção "**Atualizar**" na parte superior de **Gerenciador de soluções** para ver o arquivo log de *erros. txt* .
2. Examine o log de erros exibido no Visual Studio: 

    ![Tratamento de erro ASP.NET-log de erros. txt](aspnet-error-handling/_static/image5.png)

### <a name="safe-error-messages"></a>Mensagens de erro seguras

É **importante observar** que, quando seu aplicativo exibe mensagens de erro, ele não deve fornecer informações que um usuário mal-intencionado possa achar útil para atacar seu aplicativo. Por exemplo, se o aplicativo tentar gravar sem êxito em um banco de dados, ele não deverá exibir uma mensagem de erro que inclui o nome de usuário que ele está usando. Por esse motivo, uma mensagem de erro genérica em vermelho é exibida para o usuário. Todos os detalhes de erro adicionais são exibidos apenas para o desenvolvedor no computador local.

## <a name="using-elmah"></a>Usando o ELMAH

O ELMAH (manipuladores e módulos de log de erros) é um recurso de log de erros que você conecta ao seu aplicativo ASP.NET como um pacote NuGet. O ELMAH fornece os seguintes recursos:

- Registro em log de exceções sem tratamento.
- Uma página da Web para exibir o log inteiro de exceções não tratadas recodificadas.
- Uma página da Web para exibir os detalhes completos de cada exceção registrada.
- Uma notificação por email de cada erro no momento em que ele ocorre.
- Um RSS Feed dos últimos 15 erros do log.

Antes de poder trabalhar com o ELMAH, você deve instalá-lo. Isso é fácil usando o instalador de pacote *NuGet* . Como mencionado anteriormente nesta série de tutoriais, o NuGet é uma extensão do Visual Studio que facilita a instalação e a atualização de bibliotecas e ferramentas de software livre no Visual Studio.

1. No Visual Studio, no menu **ferramentas** , selecione **Gerenciador de pacotes NuGet** > **gerenciar pacotes NuGet para solução**. 

    ![Tratamento de erro ASP.NET-gerenciar pacotes NuGet para a solução](aspnet-error-handling/_static/image6.png)
2. A caixa de diálogo **gerenciar pacotes NuGet** é exibida no Visual Studio.
3. Na caixa de diálogo **gerenciar pacotes NuGet** , expanda **online** à esquerda e, em seguida, selecione **NuGet.org**. Em seguida, localize e instale o pacote do **ELMAH** na lista de pacotes disponíveis online. 

    ![Tratamento de erro ASP.NET-pacote NuGet ELMA](aspnet-error-handling/_static/image7.png)
4. Você precisará ter uma conexão com a Internet para baixar o pacote.
5. Na caixa de diálogo **selecionar projetos** , verifique se a seleção **WingtipToys** está selecionada e clique em **OK**. 

    ![Tratamento de erro ASP.NET-caixa de diálogo Selecionar projetos](aspnet-error-handling/_static/image8.png)
6. Clique em **fechar** na caixa de diálogo **gerenciar pacotes NuGet** , se necessário.
7. Se o Visual Studio solicitar que você recarregue os arquivos abertos, selecione "**Sim para todos**".
8. O pacote do ELMAH adiciona entradas para si mesmo no arquivo *Web. config* na raiz do seu projeto. Se o Visual Studio perguntar se você deseja recarregar o arquivo *Web. config* modificado, clique em **Sim**.

O ELMAH agora está pronto para armazenar quaisquer erros sem tratamento que ocorram.

### <a name="viewing-the-elmah-log"></a>Exibindo o log do ELMAH

A exibição do log do ELMAH é fácil, mas primeiro você criará uma exceção sem tratamento que será registrada no log do ELMAH.

1. Pressione **Ctrl + F5** para executar o aplicativo de exemplo Wingtip Toys.
2. Para gravar uma exceção sem tratamento no log do ELMAH, navegue no navegador para a seguinte URL (usando o número da porta):  
    `https://localhost:44300/NoPage.aspx` a página de erro será exibida.
3. Para exibir o log do ELMAH, navegue no navegador para a seguinte URL (usando o número da porta):  
    `https://localhost:44300/elmah.axd`

    ![Tratamento de erro ASP.NET-log de erros do ELMAH](aspnet-error-handling/_static/image9.png)

## <a name="summary"></a>Resumo

Neste tutorial, você aprendeu sobre o tratamento de erros no nível do aplicativo, no nível da página e no nível de código. Você também aprendeu como registrar em log os erros manipulados e não tratados para revisão posterior. Você adicionou o utilitário ELMAH para fornecer log de exceções e notificação ao seu aplicativo usando o NuGet. Além disso, você aprendeu sobre a importância de mensagens de erro seguras.

## <a name="tutorial-series-conclusion"></a>Conclusão da série de tutoriais

Obrigado pelo que vem a seguir. Espero que esse conjunto de tutoriais ajudou você a se familiarizar com o ASP.NET Web Forms. Se você precisar de mais informações sobre Web Forms recursos disponíveis no ASP.NET 4,5 e Visual Studio 2013, consulte [ASP.net and Web Tools para ver as notas de versão do Visual Studio 2013](../../../../visual-studio/overview/2013/release-notes.md). Além disso, certifique-se de dar uma olhada no tutorial mencionado na seção **próximas etapas** e defintely experimentar a [avaliação gratuita do Azure](https://azure.microsoft.com/pricing/free-trial/).

![Obrigado-Erik](aspnet-error-handling/_static/image10.png)  

## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre como implantar seu aplicativo Web para Microsoft Azure, consulte [implantar um aplicativo Secure ASP.NET Web Forms com associação, OAuth e banco de dados SQL em um site do Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/).

## <a name="free-trial"></a>Avaliação gratuita

[Avaliação gratuita Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/)  
 Publicar seu site em Microsoft Azure poupará tempo, manutenção e despesas. É um processo rápido para implantar seu aplicativo Web no Azure. Quando você precisa manter e monitorar seu aplicativo Web, o Azure oferece uma variedade de ferramentas e serviços. Gerencie dados, tráfego, identidade, backups, mensagens, mídia e desempenho no Azure. E tudo isso é fornecido em uma abordagem bastante econômica.

## <a name="additional-resources"></a>Recursos adicionais

[Log de detalhes de erro com  de monitoramento de integridade do ASP.net](../../older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs.md)  
[ELMAH](https://code.google.com/p/elmah/)

## <a name="acknowledgements"></a>Confirmações

Gostaria de agradecer às seguintes pessoas que fizeram contribuições significativas para o conteúdo desta série de tutoriais:

- [Alberto Poblacion, MVP &amp; MCT, Espanha](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- [Alex Thissen, Holanda](http://blog.alexthissen.nl/) (twitter: [@alexthissen](http://twitter.com/alexthissen))
- [Andre Tournier, EUA](http://andret503.wordpress.com/)
- Apurva Joshi, Microsoft
- [Bojan Vrhovnik, Eslovênia](http://twitter.com/bvrhovnik)
- [Bruno Sonnino, Brasil](http://msmvps.com/blogs/bsonnino) (twitter: [@bsonnino](http://twitter.com/bsonnino))
- [Carlos dos Santos, Brasil](http://www.carloscds.net/)
- [Dave Campbell, EUA](http://www.wynapse.com/) (twitter: [@windowsdevnews](http://twitter.com/windowsdevnews))
- [Jon Galloway, Microsoft](https://weblogs.asp.net/jgalloway) (twitter: [@jongalloway](http://twitter.com/jongalloway))
- [Michael sustenidos, EUA](http://www.930solutions.com/) (twitter: [@mrsharps](http://twitter.com/mrsharps))
- Mike Papa
- [Mitchel de vendedores, EUA](http://www.mitchelsellers.com/) (twitter: [@MitchelSellers](http://twitter.com/MitchelSellers))
- [Paul Cociuba, Microsoft](http://linqto.me/Links/pcociuba)
- [Paulo MORGADO, Portugal](http://paulomorgado.net/)
- [Pranav Rastogi, Microsoft](https://blogs.msdn.com/b/pranav_rastogi)
- [Tim Ammann, Microsoft](https://blogs.iis.net/timamm/default.aspx)
- [Tom Dykstra, Microsoft](https://blogs.msdn.com/aspnetue)

## <a name="community-contributions"></a>Contribuições da Comunidade

- Graham Mendick ([@grahammendick](http://twitter.com/grahammendick))  
  Exemplo de código relacionado ao Visual Studio 2012 no MSDN: [Navigation Wingtip Toys](https://code.msdn.microsoft.com/Navigation-Wingtip-Toys-5f0daba2)
- James Chaney ([jchaney@agvance.net](mailto:jchaney@agvance.net))  
  Exemplo de código relacionado ao Visual Studio 2012 no MSDN: [ASP.NET 4,5 Web Forms série de tutoriais no Visual Basic](https://code.msdn.microsoft.com/ASPNET-45-Web-Forms-f37f0f63)
- Andrielle Azevedo-colaborador técnico público da Microsoft (Twitter: @driazevedo)  
  Tradução do Visual Studio 2012: [iniciando com ASP.NET Web Forms 4,5-parte 1-Introdução e visão geral](https://andrielleazevedo.wordpress.com/2013/01/24/iniciando-com-asp-net-web-forms-4-5-introducao-e-visao-geral/)

> [!div class="step-by-step"]
> [Anterior](url-routing.md)
