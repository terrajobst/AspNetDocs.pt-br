---
uid: web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
title: ASP.NET Web Forms resiliência de conexão e interceptação de comando | Microsoft Docs
author: Erikre
description: Este tutorial descreve como modificar um aplicativo de exemplo para dar suporte à resiliência de conexão e à interceptação de comando.
ms.author: riande
ms.date: 03/31/2014
ms.assetid: 6d497001-fa80-4765-b4cc-181fe90b894e
msc.legacyurl: /web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
msc.type: authoredcontent
ms.openlocfilehash: 95f0b5635c12d5ef88622e5766c1278c6570dd4d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598423"
---
# <a name="aspnet-web-forms-connection-resiliency-and-command-interception"></a>Resiliência de conexão do Web Forms do ASP.NET e interceptação de comando

por [Erik Reitan](https://github.com/Erikre)

Neste tutorial, você modificará o aplicativo de exemplo Wingtip Toys para dar suporte à resiliência de conexão e à interceptação de comando. Ao habilitar a resiliência de conexão, o aplicativo de exemplo Wingtip Toys tentará automaticamente as chamadas de dados quando erros transitórios típicos de um ambiente de nuvem ocorrerem. Além disso, ao implementar a interceptação de comando, o aplicativo de exemplo Wingtip Toys capturará todas as consultas SQL enviadas ao banco de dados para fazer o log ou alterá-las.

> [!NOTE] 
> 
> Este tutorial de Web Forms foi baseado no seguinte tutorial do MVC de Tom Dykstra:  
> [Resiliência de conexão e interceptação de comando com o Entity Framework em um aplicativo MVC ASP.NET](../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="what-youll-learn"></a>O que você aprenderá:

- Como fornecer resiliência de conexão.
- Como implementar a interceptação de comando.

## <a name="prerequisites"></a>Prerequisites

Antes de começar, verifique se você tem o seguinte software instalado em seu computador:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) ou [Microsoft Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). O .NET Framework é instalado automaticamente.
- O projeto de exemplo Wingtip Toys, para que você possa implementar a funcionalidade mencionada neste tutorial dentro do projeto Wingtip Toys. O link a seguir fornece detalhes de download:

    - [Introdução com ASP.NET 4.5.1 Web Forms-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) (C#)
- Antes de concluir este tutorial, considere revisar a série de tutoriais relacionados, [introdução com ASP.NET 4,5 Web Forms e Visual Studio 2013](../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). A série de tutoriais ajudará você a se familiarizar com o projeto e o código do **WingtipToys** .

## <a name="connection-resiliency"></a>Resiliência de conexão

Ao considerar a implantação de um aplicativo no Windows Azure, uma opção a ser considerada é a implantação do banco de dados no banco de dados **SQL do** **Windows** Azure, um serviço de banco de dados de nuvem. Geralmente, os erros de conexão transitórias são mais frequentes quando você se conecta a um serviço de banco de dados de nuvem do que quando o servidor Web e o servidor de banco de dados estão diretamente conectados juntos no mesmo data center. Mesmo que um servidor Web de nuvem e um serviço de banco de dados de nuvem estejam hospedados na mesma data center, há mais conexões de rede entre eles que podem ter problemas, como balanceadores de carga.

Além disso, um serviço de nuvem é normalmente compartilhado por outros usuários, o que significa que sua capacidade de resposta pode ser afetada por eles. E seu acesso ao banco de dados pode estar sujeito à limitação. A limitação significa que o serviço de banco de dados gera exceções quando você tenta acessá-lo com mais frequência do que é permitido em seu SLA ( *contrato de nível de serviço* ).

Muitos ou mais problemas de conexão que ocorrem quando você está acessando um serviço de nuvem são transitórios, ou seja, eles se resolvem em um curto período de tempo. Portanto, quando você tenta uma operação de banco de dados e Obtém um tipo de erro que é normalmente transitório, você pode tentar a operação novamente após uma breve espera e a operação pode ser bem-sucedida. Você pode fornecer uma experiência muito melhor para seus usuários se lidar com erros transitórios ao tentar novamente de novo, tornando-os invisíveis para o cliente. O recurso de resiliência de conexão no Entity Framework 6 automatiza esse processo de repetição de consultas SQL com falha.

O recurso de resiliência de conexão deve ser configurado adequadamente para um serviço de banco de dados específico:

1. Ele precisa saber quais exceções provavelmente serão transitórias. Você deseja repetir erros causados por uma perda temporária na conectividade de rede, não erros causados por bugs de programa, por exemplo.
2. É necessário aguardar uma quantidade apropriada de tempo entre as tentativas de uma operação com falha. Você pode esperar mais tempo entre as tentativas para um processo em lotes do que você pode para uma página da Web online em que um usuário está aguardando uma resposta.
3. Ele precisa repetir um número apropriado de vezes antes de desistir. Talvez você queira repetir mais vezes em um processo em lote que você usaria em um aplicativo online.

Você pode definir essas configurações manualmente para qualquer ambiente de banco de dados com suporte de um provedor de Entity Framework.

Tudo o que você precisa fazer para habilitar a resiliência de conexão é criar uma classe no assembly que derive da classe `DbConfiguration` e, nessa classe, definir a estratégia de execução do banco de dados SQL, que, em Entity Framework, é outro termo para a política de repetição.

### <a name="implementing-connection-resiliency"></a>Implementando resiliência de conexão

1. Baixe e abra o aplicativo Web Forms de exemplo [WingtipToys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) no Visual Studio.
2. Na pasta *lógica* do aplicativo **WingtipToys** , adicione um arquivo de classe chamado *WingtipToysConfiguration.cs*.
3. Substitua o código existente pelo seguinte código:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample1.cs)]

O Entity Framework executa automaticamente o código encontrado em uma classe derivada de `DbConfiguration`. Você pode usar a classe `DbConfiguration` para realizar tarefas de configuração no código que, de outra forma, faria no arquivo *Web. config* . Para obter mais informações, consulte [Configuração baseada em código do EntityFramework](https://msdn.microsoft.com/data/jj680699).

1. Na pasta *lógica* , abra o arquivo *AddProducts.cs* .
2. Adicione uma instrução `using` para `System.Data.Entity.Infrastructure` conforme mostrado em amarelo:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample2.cs?highlight=6)]
3. Adicione um bloco de `catch` ao método `AddProduct` para que o `RetryLimitExceededException` seja registrado como realçado em amarelo:   

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample3.cs?highlight=14-15,17-22)]

Ao adicionar a exceção de `RetryLimitExceededException`, você pode fornecer um log melhor ou exibir uma mensagem de erro para o usuário em que ele pode optar por tentar o processo novamente. Ao capturar a exceção de `RetryLimitExceededException`, os únicos erros que provavelmente serão transitórios já terão sido testados e terão falhado várias vezes. A exceção real retornada será encapsulada na exceção de `RetryLimitExceededException`. Além disso, você também adicionou um bloco catch geral. Para obter mais informações sobre a exceção de `RetryLimitExceededException`, consulte [Entity Framework lógica de repetição/resiliência de conexão](https://msdn.microsoft.com/data/dn456835).

## <a name="command-interception"></a>Interceptação de comando

Agora que você ativou uma política de repetição, como testar para verificar se ela está funcionando conforme o esperado? Não é tão fácil forçar um erro transitório a acontecer, especialmente quando você está executando localmente, e seria especialmente difícil integrar erros transitórios reais em um teste de unidade automatizado. Para testar o recurso de resiliência de conexão, você precisa de uma maneira de interceptar consultas que Entity Framework envia para SQL Server e substituir a resposta SQL Server por um tipo de exceção que normalmente é transitório.

Você também pode usar a interceptação de consulta para implementar uma prática recomendada para aplicativos de nuvem: Registre a latência e o êxito ou a falha de todas as chamadas para serviços externos, como serviços de banco de dados.

Nesta seção do tutorial, você usará o [*recurso de interceptação*](https://msdn.microsoft.com/data/dn469464) do Entity Framework para registrar em log e para simular erros transitórios.

### <a name="create-a-logging-interface-and-class"></a>Criar uma interface e uma classe de log

Uma prática recomendada para o registro em log é fazer isso usando uma [`interface`](https://msdn.microsoft.com/library/ms173156.aspx) em vez de chamadas de codificação embutidas para `System.Diagnostics.Trace` ou uma classe de log. Isso facilita a alteração do mecanismo de registro em log posteriormente, se você precisar fazer isso. Portanto, nesta seção, você criará a interface de log e uma classe para implementá-la.

Com base no procedimento acima, você baixou e abriu o aplicativo de exemplo **WingtipToys** no Visual Studio.

1. Crie uma pasta no projeto **WingtipToys** e nomeie-a como *log*.
2. Na pasta *log* , crie um arquivo de classe chamado *ILogger.cs* e substitua o código padrão pelo código a seguir:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample4.cs)]

   A interface fornece três níveis de rastreamento para indicar a importância relativa dos logs e um projetado para fornecer informações de latência para chamadas de serviço externas, como consultas de banco de dados. Os métodos de registro em log têm sobrecargas que permitem que você passe uma exceção. Isso é para que as informações de exceção, incluindo o rastreamento de pilha e as exceções internas, sejam registradas de forma confiável pela classe que implementa a interface, em vez de contar com isso sendo feita em cada chamada de método de log em todo o aplicativo.  
  
   Os métodos de `TraceApi` permitem acompanhar a latência de cada chamada para um serviço externo, como o banco de dados SQL.
3. Na pasta *log* , crie um arquivo de classe chamado *Logger.cs* e substitua o código padrão pelo código a seguir:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample5.cs)]

A implementação usa `System.Diagnostics` para fazer o rastreamento. Esse é um recurso interno do .NET que torna mais fácil gerar e usar informações de rastreamento. Há muitos ouvintes &quot;&quot; você pode usar com rastreamento de `System.Diagnostics` para gravar logs em arquivos, por exemplo, ou gravá-los no armazenamento de BLOBs no Windows Azure. Veja algumas das opções e links para outros recursos para obter mais informações, em [solução de problemas de sites do Windows Azure no Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Para este tutorial, você só examinará os logs na janela de **saída** do Visual Studio.

Em um aplicativo de produção, talvez você queira considerar o uso de estruturas de rastreamento diferentes de `System.Diagnostics`, e a interface `ILogger` torna relativamente fácil alternar para um mecanismo de rastreamento diferente se você decidir fazer isso.

### <a name="create-interceptor-classes"></a>Criar classes de interceptador

Em seguida, você criará as classes que o Entity Framework chamará sempre que vai enviar uma consulta para o banco de dados, uma para simular erros transitórios e outra para fazer o registro em log. Essas classes de interceptador devem derivar da classe `DbCommandInterceptor`. Nela, você escreve substituições de método que são chamadas automaticamente quando a consulta está prestes a ser executada. Nesses métodos, você pode examinar ou registrar em log a consulta que está sendo enviada para o banco de dados e pode alterar a consulta antes que ela seja enviada ao banco de dados ou retornar algo para Entity Framework você mesmo sem passar a consulta para o banco de dados.

1. Para criar a classe intercepter que registrará cada consulta SQL antes de ser enviada ao banco de dados, crie um arquivo de classe chamado *InterceptorLogging.cs* na pasta *lógica* e substitua o código padrão pelo código a seguir:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample6.cs)]

   Para consultas ou comandos bem-sucedidos, esse código grava um log de informações com informações de latência. Para exceções, ele cria um log de erros.
2. Para criar a classe intercepter que gerará erros transitórios falsos quando você inserir &quot;lançar&quot; na caixa de texto **nome** na página chamada *AdminPage. aspx*, crie um arquivo de classe chamado *InterceptorTransientErrors.cs* na pasta *lógica* e substitua o código padrão pelo código a seguir:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample7.cs)]

    Esse código apenas substitui o método `ReaderExecuting`, que é chamado para consultas que podem retornar várias linhas de dados. Se você quisesse verificar a resiliência da conexão para outros tipos de consultas, também poderá substituir os métodos `NonQueryExecuting` e `ScalarExecuting`, como faz o interceptador de registro em log.  
  
   Posteriormente, você fará logon como "admin" e selecionará o link de **administrador** na barra de navegação superior. Em seguida, na página *AdminPage. aspx* , você adicionará um produto chamado &quot;throw&quot;. O código cria uma exceção de banco de dados SQL fictícia para o número de erro 20, um tipo conhecido como normalmente transitório. Outros números de erro atualmente reconhecidos como transitórios são 64, 233, 10053, 10054, 10060, 10928, 10929, 40197, 40501 e 40613, mas estão sujeitos a alterações em novas versões do banco de dados SQL. O produto será renomeado para "TransientErrorExample", que você pode seguir no código do arquivo *InterceptorTransientErrors.cs* .  
  
   O código retorna a exceção a Entity Framework em vez de executar a consulta e passar os resultados. A exceção transitória é retornada *quatro* vezes e, em seguida, o código reverte para o procedimento normal de passar a consulta para o banco de dados.

    Como tudo é registrado, você poderá ver que Entity Framework tenta executar a consulta quatro vezes antes de finalmente ter sucesso, e a única diferença no aplicativo é que leva mais tempo para renderizar uma página com resultados da consulta.  
  
   O número de vezes que o Entity Framework tentará ser configurado novamente; o código especifica quatro vezes porque esse é o valor padrão para a política de execução do banco de dados SQL. Se você alterar a política de execução, também alteraria o código aqui que especifica quantas vezes erros transitórios são gerados. Você também pode alterar o código para gerar mais exceções, de modo que Entity Framework gerará a `RetryLimitExceededException` exceção.
3. No *global. asax*, adicione as seguintes instruções using:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample8.cs)]
4. Em seguida, adicione as linhas realçadas ao método `Application_Start`:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample9.cs?highlight=17-20)]

Essas linhas de código são o que faz com que o código do Interceptor seja executado quando Entity Framework envia consultas ao banco de dados. Observe que, como você criou classes de interceptador separadas para simulação de erro transitória e registro em log, você pode habilitá-las e desabilitá-las de forma independente.   
  
 Você pode adicionar interceptores usando o método `DbInterception.Add` em qualquer lugar em seu código; Ele não precisa estar no método `Application_Start`. Outra opção, se você não adicionou interceptadores no método `Application_Start`, seria atualizar ou adicionar a classe chamada *WingtipToysConfiguration.cs* e colocar o código acima no final do construtor da classe `WingtipToysConfiguration`.

Sempre que você colocar esse código, tenha cuidado para não executar `DbInterception.Add` para o mesmo Interceptor mais de uma vez ou você obterá instâncias adicionais do Interceptor. Por exemplo, se você adicionar o interceptador de log duas vezes, verá dois logs para cada consulta SQL.

Os interceptores são executados na ordem de registro (a ordem na qual o método de `DbInterception.Add` é chamado). A ordem pode ser importante dependendo do que você está fazendo no Interceptor. Por exemplo, um interceptador pode alterar o comando SQL que ele obtém na propriedade `CommandText`. Se ele alterar o comando SQL, o interceptador seguinte receberá o comando SQL alterado, não o comando SQL original.

Você escreveu o código de simulação de erro transitório de forma que permita que você cause erros transitórios inserindo um valor diferente na interface do usuário. Como alternativa, você poderia escrever o código do Interceptor para sempre gerar a sequência de exceções transitórias sem verificar um valor de parâmetro específico. Você poderia então adicionar o interceptador somente quando quiser gerar erros transitórios. No entanto, se você fizer isso, não adicione o Interceptor até que a inicialização do banco de dados seja concluída. Em outras palavras, faça pelo menos uma operação de banco de dados, como uma consulta em um de seus conjuntos de entidades, antes de começar a gerar erros transitórios. A Entity Framework executa várias consultas durante a inicialização do banco de dados e elas não são executadas em uma transação, de modo que os erros durante a inicialização podem fazer com que o contexto entre em um estado inconsistente.

## <a name="test-logging-and-connection-resiliency"></a>Log de teste e resiliência de conexão

1. No Visual Studio, pressione **F5** para executar o aplicativo no modo de depuração e, em seguida, faça logon como "admin" usando "Pa $ $Word" como a senha.
2. Selecione **administrador** na barra de navegação na parte superior.
3. Insira um novo produto chamado "Throw" com a descrição apropriada, o preço e o arquivo de imagem.
4. Pressione o botão **Adicionar produto** .  
   Você observará que o navegador parece parar por vários segundos enquanto Entity Framework está repetindo a consulta várias vezes. A primeira repetição ocorre muito rapidamente, então a espera aumenta antes de cada repetição adicional. Esse processo de espera por mais tempo antes de cada repetição é chamado de *retirada exponencial* .
5. Aguarde até que a página não esteja mais tentando carregar.
6. Pare o projeto e examine a janela de **saída** do Visual Studio para ver a saída de rastreamento. Você pode encontrar a janela de **saída** selecionando **debug** -&gt; **Windows** -&gt; **saída**. Talvez seja necessário rolar vários outros logs gravados por seu agente de log.  
  
   Observe que você pode ver as consultas SQL reais enviadas ao banco de dados. Você vê algumas consultas e comandos iniciais que Entity Framework faz para começar, verificando a versão do banco de dados e a tabela de histórico de migração.   
    ![Janela de Saída](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image1.png)   
   Observe que você não pode repetir esse teste, a menos que você interrompa o aplicativo e o reinicie. Se você quisesse ser capaz de testar a resiliência da conexão várias vezes em uma única execução do aplicativo, você poderia escrever código para redefinir o contador de erros em `InterceptorTransientErrors`.
7. Para ver a diferença que a estratégia de execução (política de repetição) faz, comente a linha de `SetExecutionStrategy` no arquivo *WingtipToysConfiguration.cs* na pasta *lógica* , execute a página de **Administração** no modo de depuração novamente e adicione o produto chamado &quot;throw&quot; novamente.  
  
   Desta vez, o depurador parará na primeira exceção gerada imediatamente quando tentar executar a consulta pela primeira vez.  
    ![Depuração-detalhes da exibição](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image2.png)
8. Remova a marca de comentário da linha de `SetExecutionStrategy` no arquivo *WingtipToysConfiguration.cs* .

## <a name="summary"></a>Resumo

Neste tutorial, você viu como modificar um aplicativo de exemplo Web Forms para dar suporte à resiliência de conexão e à interceptação de comando.

## <a name="next-steps"></a>Próximas etapas

Depois de revisar a resiliência de conexão e a interceptação de comando no ASP.NET Web Forms, examine os métodos assíncronos Web Forms tópico do ASP.NET [no ASP.NET 4,5](../performance-and-caching/using-asynchronous-methods-in-aspnet-45.md). O tópico ensinará as noções básicas da criação de um aplicativo ASP.NET Web Forms assíncrono usando o Visual Studio.
