---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Tutorial: usar resiliência de conexão e interceptação de comando com o EF em um aplicativo MVC ASP.NET'
author: tdykstra
description: Neste tutorial, você aprenderá a usar a resiliência de conexão e a interceptação de comando. Eles são dois recursos importantes do Entity Framework 6.
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: c89d809f-6c65-4425-a3fa-c9f6e8ac89f2
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 276266f8ae9df38529d44742ebe6ac0dc8e79727
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583450"
---
# <a name="tutorial-use-connection-resiliency-and-command-interception-with-entity-framework-in-an-aspnet-mvc-app"></a>Tutorial: usar a resiliência de conexão e a interceptação de comando com Entity Framework em um aplicativo MVC ASP.NET

Até agora, o aplicativo está em execução localmente em IIS Express no seu computador de desenvolvimento. Para tornar um aplicativo real disponível para outras pessoas usarem pela Internet, você precisa implantá-lo em um provedor de hospedagem na Web, e você precisa implantar o banco de dados em um servidor de banco de dados.

Neste tutorial, você aprenderá a usar a resiliência de conexão e a interceptação de comando. Eles são dois recursos importantes do Entity Framework 6 que são especialmente valiosos quando você está implantando no ambiente de nuvem: resiliência de conexão (tentativas automáticas de erros transitórios) e interceptação de comando (capturar todas as consultas SQL enviadas ao banco de dados para fazer o log ou alterá-los).

Este tutorial de resiliência de conexão e de interceptação de comando é opcional. Se você ignorar este tutorial, alguns ajustes secundários precisarão ser feitos nos tutoriais subsequentes.

Neste tutorial, você:

> [!div class="checklist"]
> * Habilitar resiliência de conexão
> * Habilitar interceptação de comando
> * Testar a nova configuração

## <a name="prerequisites"></a>Prerequisites

* [Classificação, filtragem e paginação](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="enable-connection-resiliency"></a>Habilitar resiliência de conexão

Ao implantar o aplicativo no Windows Azure, você implantará o banco de dados no banco de dados SQL do Windows Azure, um serviço de banco de dados de nuvem. Geralmente, os erros de conexão transitórias são mais frequentes quando você se conecta a um serviço de banco de dados de nuvem do que quando o servidor Web e o servidor de banco de dados estão diretamente conectados juntos no mesmo data center. Mesmo que um servidor Web de nuvem e um serviço de banco de dados de nuvem estejam hospedados na mesma data center, há mais conexões de rede entre eles que podem ter problemas, como balanceadores de carga.

Além disso, um serviço de nuvem é normalmente compartilhado por outros usuários, o que significa que sua capacidade de resposta pode ser afetada por eles. E seu acesso ao banco de dados pode estar sujeito à limitação. A limitação significa que o serviço de banco de dados gera exceções quando você tenta acessá-lo com mais frequência do que é permitido em seu SLA (Contrato de Nível de Serviço).

Muitos ou mais problemas de conexão quando você está acessando um serviço de nuvem são temporários, ou seja, eles se resolvem em um curto período de tempo. Portanto, quando você tenta uma operação de banco de dados e Obtém um tipo de erro que é normalmente transitório, você pode tentar a operação novamente após uma breve espera e a operação pode ser bem-sucedida. Você pode fornecer uma experiência muito melhor para seus usuários se lidar com erros transitórios ao tentar novamente de novo, tornando-os invisíveis para o cliente. O recurso de resiliência de conexão no Entity Framework 6 automatiza esse processo de repetição de consultas SQL com falha.

O recurso de resiliência de conexão deve ser configurado adequadamente para um serviço de banco de dados específico:

- Ele precisa saber quais exceções provavelmente serão transitórias. Você deseja repetir erros causados por uma perda temporária na conectividade de rede, não erros causados por bugs de programa, por exemplo.
- É necessário aguardar uma quantidade apropriada de tempo entre as tentativas de uma operação com falha. Você pode esperar mais tempo entre as tentativas para um processo em lotes do que você pode para uma página da Web online em que um usuário está aguardando uma resposta.
- Ele precisa repetir um número apropriado de vezes antes de desistir. Talvez você queira repetir mais vezes em um processo em lote que você usaria em um aplicativo online.

Você pode definir essas configurações manualmente para qualquer ambiente de banco de dados com suporte de um provedor de Entity Framework, mas os valores padrão que normalmente funcionam bem para um aplicativo online que usa o banco de dados SQL do Windows Azure já foram configurados para você e Essas são as configurações que você implementará para o aplicativo da Contoso University.

Tudo o que você precisa fazer para habilitar a resiliência de conexão é criar uma classe no assembly que derive da classe [DbConfiguration](https://msdn.microsoft.com/data/jj680699.aspx) e nessa classe define a *estratégia de execução*do banco de dados SQL, que no EF é outro termo para a política de *repetição*.

1. Na pasta DAL, adicione um arquivo de classe chamado *SchoolConfiguration.cs*.
2. Substitua o código do modelo pelo seguinte código:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

    O Entity Framework executa automaticamente o código encontrado em uma classe derivada de `DbConfiguration`. Você pode usar a classe `DbConfiguration` para realizar tarefas de configuração no código que, de outra forma, faria no arquivo *Web. config* . Para obter mais informações, consulte [Configuração baseada em código do EntityFramework](https://msdn.microsoft.com/data/jj680699).
3. No *StudentController.cs*, adicione uma instrução `using` para `System.Data.Entity.Infrastructure`.

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]
4. Altere todos os blocos de `catch` que capturam `DataException` exceções para que eles detectem `RetryLimitExceededException` exceções em vez disso. Por exemplo:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=1)]

    Você estava usando `DataException` para tentar identificar erros que podem ser transitórios a fim de fornecer uma mensagem amigável "tentar novamente". Mas agora que você ativou uma política de repetição, os únicos erros provavelmente serão transitórios já terão sido experimentados e falharam várias vezes e a exceção real retornada será encapsulada na exceção `RetryLimitExceededException`.

Para obter mais informações, consulte [Entity Framework lógica de repetição/resiliência de conexão](https://msdn.microsoft.com/data/dn456835).

## <a name="enable-command-interception"></a>Habilitar interceptação de comando

Agora que você ativou uma política de repetição, como testar para verificar se ela está funcionando conforme o esperado? Não é tão fácil forçar um erro transitório a acontecer, especialmente quando você está executando localmente, e seria especialmente difícil integrar erros transitórios reais em um teste de unidade automatizado. Para testar o recurso de resiliência de conexão, você precisa de uma maneira de interceptar consultas que Entity Framework envia para SQL Server e substituir a resposta SQL Server por um tipo de exceção que normalmente é transitório.

Você também pode usar a interceptação de consulta para implementar uma prática recomendada para aplicativos de nuvem: [Registre a latência e o êxito ou a falha de todas as chamadas para serviços externos](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log) , como serviços de banco de dados. O EF6 fornece uma [API de log dedicada](https://msdn.microsoft.com/data/dn469464) que pode facilitar o registro em log, mas nesta seção do tutorial você aprenderá a usar o [recurso de interceptação](https://msdn.microsoft.com/data/dn469464) do Entity Framework diretamente, tanto para registro em log quanto para simular erros transitórios.

### <a name="create-a-logging-interface-and-class"></a>Criar uma interface e uma classe de log

Uma [prática recomendada para o registro em log](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log) é fazer isso usando uma interface em vez de chamadas de codificação embutidas para System. Diagnostics. Trace ou uma classe de registro em log. Isso facilita a alteração do mecanismo de registro em log posteriormente, se você precisar fazer isso. Portanto, nesta seção, você criará a interface de log e uma classe para implementá-la./p >

1. Crie uma pasta no projeto e nomeie-a como *log*.
2. Na pasta *log* , crie um arquivo de classe chamado *ILogger.cs*e substitua o código do modelo pelo código a seguir:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    A interface fornece três níveis de rastreamento para indicar a importância relativa dos logs e um projetado para fornecer informações de latência para chamadas de serviço externas, como consultas de banco de dados. Os métodos de registro em log têm sobrecargas que permitem que você passe uma exceção. Isso é para que as informações de exceção, incluindo o rastreamento de pilha e as exceções internas, sejam registradas de forma confiável pela classe que implementa a interface, em vez de contar com isso sendo feita em cada chamada de método de log em todo o aplicativo.

    Os métodos TraceApi permitem que você acompanhe a latência de cada chamada para um serviço externo, como o banco de dados SQL.
3. Na pasta *log* , crie um arquivo de classe chamado *Logger.cs*e substitua o código do modelo pelo código a seguir:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    A implementação usa System. Diagnostics para fazer o rastreamento. Esse é um recurso interno do .NET que torna mais fácil gerar e usar informações de rastreamento. Há muitos "ouvintes" que você pode usar com o rastreamento de System. Diagnostics para gravar logs em arquivos, por exemplo, ou para gravá-los no armazenamento de BLOBs no Azure. Veja algumas das opções e links para outros recursos para obter mais informações, em [solução de problemas de sites do Azure no Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Para este tutorial, você só examinará os logs na janela de **saída** do Visual Studio.

    Em um aplicativo de produção, talvez você queira considerar os pacotes de rastreamento diferentes de System. Diagnostics, e a interface ILogger torna relativamente fácil alternar para um mecanismo de rastreamento diferente se você decidir fazer isso.

### <a name="create-interceptor-classes"></a>Criar classes de interceptador

Em seguida, você criará as classes que o Entity Framework chamará sempre que vai enviar uma consulta para o banco de dados, uma para simular erros transitórios e outra para fazer o registro em log. Essas classes de interceptador devem derivar da classe `DbCommandInterceptor`. Nela, você escreve as substituições de método que são chamadas automaticamente quando a consulta está prestes a ser executada. Nesses métodos, você pode examinar ou registrar em log a consulta que está sendo enviada para o banco de dados e pode alterar a consulta antes que ela seja enviada ao banco de dados ou retornar algo para Entity Framework você mesmo sem passar a consulta para o banco de dados.

1. Para criar a classe intercepter que registrará cada consulta SQL enviada ao banco de dados, crie um arquivo de classe chamado *SchoolInterceptorLogging.cs* na pasta *Dal* e substitua o código do modelo pelo código a seguir:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

    Para consultas ou comandos bem-sucedidos, esse código grava um log de informações com informações de latência. Para exceções, ele cria um log de erros.
2. Para criar a classe intercepter que gerará erros transitórios falsos quando você inserir "Throw" na caixa de **pesquisa** , crie um arquivo de classe chamado *SchoolInterceptorTransientErrors.cs* na pasta *Dal* e substitua o código do modelo pelo código a seguir:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

    Esse código apenas substitui o método `ReaderExecuting`, que é chamado para consultas que podem retornar várias linhas de dados. Se você quisesse verificar a resiliência da conexão para outros tipos de consultas, também poderá substituir os métodos `NonQueryExecuting` e `ScalarExecuting`, como faz o interceptador de registro em log.

    Quando você executa a página de aluno e insere "Throw" como a cadeia de caracteres de pesquisa, esse código cria uma exceção de banco de dados SQL fictícia para o número de erro 20, um tipo conhecido para ser normalmente transitório. Outros números de erro atualmente reconhecidos como transitórios são 64, 233, 10053, 10054, 10060, 10928, 10929, 40197, 40501 e 40613, mas estão sujeitos a alterações em novas versões do banco de dados SQL.

    O código retorna a exceção a Entity Framework em vez de executar a consulta e retornar os resultados da consulta. A exceção transitória é retornada quatro vezes e, em seguida, o código reverte para o procedimento normal de passar a consulta para o banco de dados.

    Como tudo é registrado, você poderá ver que Entity Framework tenta executar a consulta quatro vezes antes de finalmente ter sucesso, e a única diferença no aplicativo é que leva mais tempo para renderizar uma página com resultados da consulta.

    O número de vezes que o Entity Framework tentará ser configurado novamente; o código especifica quatro vezes porque esse é o valor padrão para a política de execução do banco de dados SQL. Se você alterar a política de execução, também alteraria o código aqui que especifica quantas vezes erros transitórios são gerados. Você também pode alterar o código para gerar mais exceções, de modo que Entity Framework gerará a `RetryLimitExceededException` exceção.

    O valor que você inserir na caixa de pesquisa estará em `command.Parameters[0]` e `command.Parameters[1]` (um é usado para o nome e um para o sobrenome). Quando o valor "% Throw%" for encontrado, "Throw" será substituído nesses parâmetros por "a" para que alguns alunos sejam encontrados e retornados.

    Essa é apenas uma maneira conveniente de testar a resiliência da conexão com base na alteração de alguma entrada para a interface do usuário do aplicativo. Você também pode escrever código que gera erros transitórios para todas as consultas ou atualizações, conforme explicado posteriormente nos comentários sobre o método *DbInterception. Add* .
3. No *global. asax*, adicione as seguintes instruções de `using`:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]
4. Adicione as linhas realçadas ao método de `Application_Start`:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=7-8)]

    Essas linhas de código são o que faz com que o código do Interceptor seja executado quando Entity Framework envia consultas ao banco de dados. Observe que, como você criou classes de interceptador separadas para simulação de erro transitória e registro em log, você pode habilitá-las e desabilitá-las de forma independente.

    Você pode adicionar interceptores usando o método `DbInterception.Add` em qualquer lugar em seu código; Ele não precisa estar no método `Application_Start`. Outra opção é colocar esse código na classe DbConfiguration que você criou anteriormente para configurar a política de execução.

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=6-7)]

    Sempre que você colocar esse código, tenha cuidado para não executar `DbInterception.Add` para o mesmo Interceptor mais de uma vez ou você obterá instâncias adicionais do Interceptor. Por exemplo, se você adicionar o interceptador de log duas vezes, verá dois logs para cada consulta SQL.

    Os interceptores são executados na ordem de registro (a ordem na qual o método de `DbInterception.Add` é chamado). A ordem pode ser importante dependendo do que você está fazendo no Interceptor. Por exemplo, um interceptador pode alterar o comando SQL que ele obtém na propriedade `CommandText`. Se ele alterar o comando SQL, o interceptador seguinte receberá o comando SQL alterado, não o comando SQL original.

    Você escreveu o código de simulação de erro transitório de forma que permita que você cause erros transitórios inserindo um valor diferente na interface do usuário. Como alternativa, você poderia escrever o código do Interceptor para sempre gerar a sequência de exceções transitórias sem verificar um valor de parâmetro específico. Você poderia então adicionar o interceptador somente quando quiser gerar erros transitórios. No entanto, se você fizer isso, não adicione o Interceptor até que a inicialização do banco de dados seja concluída. Em outras palavras, faça pelo menos uma operação de banco de dados, como uma consulta em um de seus conjuntos de entidades, antes de começar a gerar erros transitórios. A Entity Framework executa várias consultas durante a inicialização do banco de dados e elas não são executadas em uma transação, de modo que os erros durante a inicialização podem fazer com que o contexto entre em um estado inconsistente.

## <a name="test-the-new-configuration"></a>Testar a nova configuração

1. Pressione **F5** para executar o aplicativo no modo de depuração e clique na guia **alunos** .
2. Examine a janela de **saída** do Visual Studio para ver a saída de rastreamento. Talvez seja necessário rolar para cima alguns erros de JavaScript para obter os logs gravados pelo seu agente.

    Observe que você pode ver as consultas SQL reais enviadas ao banco de dados. Você vê algumas consultas e comandos iniciais que Entity Framework faz para começar, verificando a versão do banco de dados e a tabela de histórico de migração (você aprenderá sobre as migrações no próximo tutorial). E você vê uma consulta de paginação, para descobrir quantos alunos existem e, finalmente, vê a consulta que obtém os dados do aluno.

    ![Registro em log para consulta normal](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. Na página **alunos** , digite "Throw" como a cadeia de caracteres de pesquisa e clique em **Pesquisar**.

    ![Gerar cadeia de caracteres de pesquisa](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

    Você observará que o navegador parece parar por vários segundos enquanto Entity Framework está repetindo a consulta várias vezes. A primeira repetição ocorre muito rapidamente e, em seguida, a espera antes de cada repetição adicional. Esse processo de espera por mais tempo antes de cada repetição é chamado de *retirada exponencial*.

    Quando a página for exibida, mostrando os alunos que têm "um" em seus nomes, examine a janela de saída e você verá que a mesma consulta foi tentada cinco vezes, as quatro primeiras vezes retornando exceções transitórias. Para cada erro transitório, você verá o log que você escreve ao gerar o erro transitório na classe `SchoolInterceptorTransientErrors` ("retornando erro transitório para o comando...") e você verá o log gravado quando `SchoolInterceptorLogging` Obtém a exceção.

    ![Log de saída mostrando novas tentativas](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

    Como você inseriu uma cadeia de caracteres de pesquisa, a consulta que retorna os dados dos alunos é parametrizada:

    [!code-sql[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.sql)]

    Você não está registrando o valor dos parâmetros, mas pode fazer isso. Se você quiser ver os valores de parâmetro, poderá gravar o código de log para obter os valores de parâmetro da propriedade `Parameters` do objeto `DbCommand` que você obtém nos métodos do Interceptor.

    Observe que você não pode repetir esse teste, a menos que você interrompa o aplicativo e o reinicie. Se você quisesse ser capaz de testar a resiliência da conexão várias vezes em uma única execução do aplicativo, você poderia escrever código para redefinir o contador de erros em `SchoolInterceptorTransientErrors`.
4. Para ver a diferença que a estratégia de execução (política de repetição) faz, comente a linha de `SetExecutionStrategy` em *SchoolConfiguration.cs*, execute a página estudantes no modo de depuração novamente e pesquise "Throw" novamente.

    Desta vez, o depurador parará na primeira exceção gerada imediatamente quando tentar executar a consulta pela primeira vez.

    ![Exceção fictícia](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
5. Remova a marca de comentário da linha *SetExecutionStrategy* em *SchoolConfiguration.cs*.

## <a name="get-the-code"></a>Obter o código

[Baixar projeto concluído](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Recursos adicionais

Links para outros recursos de Entity Framework podem ser encontrados em [recursos de acesso a dados ASP.net-recomendado](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você:

> [!div class="checklist"]
> * Resiliência de conexão habilitada
> * Interceptação de comando habilitada
> * Testou a nova configuração

Avance para o próximo artigo para saber mais sobre as migrações de Code First e a implantação do Azure.
> [!div class="nextstepaction"]
> [Migrações de Code First e implantação do Azure](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)
