---
uid: signalr/overview/advanced/dependency-injection
title: Injeção de dependência no Signalr | Microsoft Docs
author: bradygaster
description: Versões de software usadas neste tópico Visual Studio 2013 o .NET 4,5 Signalr versão 2 versões anteriores deste tópico para obter informações sobre versões anteriores do...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: a14121ae-02cf-4024-8af0-9dd0dc810690
msc.legacyurl: /signalr/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 52978b10b6c131ac8eff4535216cc60b43fdf3de
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78537327"
---
# <a name="dependency-injection-in-signalr"></a>Injeção de dependência no SignalR

por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a>Versões de software usadas neste tópico
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - Sinalização versão 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Versões anteriores deste tópico
>
> Para obter informações sobre versões anteriores do Signalr, confira [versões mais antigas do signalr](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Perguntas e comentários
>
> Deixe comentários sobre como você gostou deste tutorial e o que poderíamos melhorar nos comentários na parte inferior da página. Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, poderá lançá-las no fórum do [signalr ASP.net](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [stackoverflow.com](http://stackoverflow.com/).

A injeção de dependência é uma maneira de remover dependências embutidas em código entre objetos, facilitando a substituição de dependências de um objeto, seja para teste (usando objetos fictícios) ou para alterar o comportamento em tempo de execução. Este tutorial mostra como executar a injeção de dependência em hubs de sinalização. Ele também mostra como usar contêineres IoC com Signalr. Um contêiner IoC é uma estrutura geral para injeção de dependência.

## <a name="what-is-dependency-injection"></a>O que é injeção de dependência?

Ignore esta seção se você já estiver familiarizado com a injeção de dependência.

*Injeção de dependência* (di) é um padrão em que os objetos não são responsáveis por criar suas próprias dependências. Aqui está um exemplo simples para motivar DI. Suponha que você tenha um objeto que precise registrar mensagens. Você pode definir uma interface de log:

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

Em seu objeto, você pode criar um `ILogger` para registrar mensagens:

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

Isso funciona, mas não é o melhor design. Se você quiser substituir `FileLogger` por outra implementação de `ILogger`, precisará modificar `SomeComponent`. Suponhamos que muitos outros objetos usam `FileLogger`, será necessário alterar todos eles. Ou, se você decidir tornar `FileLogger` um singleton, também precisará fazer alterações em todo o aplicativo.

Uma abordagem melhor é "injetar" um `ILogger` no objeto — por exemplo, usando um argumento de construtor:

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

Agora o objeto não é responsável por selecionar qual `ILogger` usar. Você pode alternar `ILogger` implementações sem alterar os objetos que dependem dela.

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

Esse padrão é chamado de [injeção de Construtor](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection). Outro padrão é a injeção de setter, em que você define a dependência por meio de um método ou propriedade setter.

## <a name="simple-dependency-injection-in-signalr"></a>Injeção de dependência simples no Signalr

Considere o aplicativo de chat do tutorial [introdução com o signalr](../getting-started/tutorial-getting-started-with-signalr.md). Esta é a classe de Hub desse aplicativo:

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

Suponha que você deseja armazenar mensagens de chat no servidor antes de enviá-las. Você pode definir uma interface que abstrai essa funcionalidade e usar DI para injetar a interface na classe `ChatHub`.

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

O único problema é que um aplicativo Signalr não cria hubs diretamente; O signalr os cria para você. Por padrão, o Signalr espera que uma classe de Hub tenha um construtor sem parâmetros. No entanto, você pode registrar facilmente uma função para criar instâncias de Hub e usar essa função para executar DI. Registre a função chamando **GlobalHost. DependencyResolver. Register**.

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

Agora, o Signalr invocará essa função anônima sempre que precisar criar uma instância de `ChatHub`.

## <a name="ioc-containers"></a>Contêineres IoC

O código anterior é adequado para casos simples. Mas você ainda teve de escrever isso:

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

Em um aplicativo complexo com muitas dependências, talvez seja necessário escrever muitos desse código de "fiação". Esse código pode ser difícil de manter, especialmente se as dependências estiverem aninhadas. Também é difícil de testar unidade.

Uma solução é usar um contêiner IoC. Um contêiner IoC é um componente de software que é responsável por gerenciar dependências. Você registra os tipos com o contêiner e, em seguida, usa o contêiner para criar objetos. O contêiner ilustra automaticamente as relações de dependência. Muitos contêineres IoC também permitem que você controle coisas como o tempo de vida e o escopo do objeto.

> [!NOTE]
> "IoC" significa "inversão de controle", que é um padrão geral em que uma estrutura chama o código do aplicativo. Um contêiner IoC constrói seus objetos para você, que "inverte" o fluxo de controle usual.

## <a name="using-ioc-containers-in-signalr"></a>Usando contêineres IoC no Signalr

O aplicativo de chat provavelmente é muito simples de se beneficiar de um contêiner IoC. Em vez disso, vamos dar uma olhada no exemplo de [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) .

O exemplo StockTicker define duas classes principais:

- `StockTickerHub`: a classe Hub, que gerencia as conexões de cliente.
- `StockTicker`: um singleton que contém preços de estoque e os atualiza periodicamente.

`StockTickerHub` mantém uma referência ao singleton `StockTicker`, enquanto `StockTicker` mantém uma referência ao **IHubConnectionContext** para o `StockTickerHub`. Ele usa essa interface para se comunicar com instâncias de `StockTickerHub`. (Para obter mais informações, consulte [transmissão de servidor com signalr ASP.net](../getting-started/tutorial-server-broadcast-with-signalr.md).)

Podemos usar um contêiner IoC para Untangle essas dependências um pouco. Primeiro, vamos simplificar as classes `StockTickerHub` e `StockTicker`. No código a seguir, eu comentei as partes que não precisamos.

Remova o construtor sem parâmetros de `StockTickerHub`. Em vez disso, sempre usaremos DI para criar o Hub.

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

Para StockTicker, remova a instância singleton. Posteriormente, usaremos o contêiner IoC para controlar o tempo de vida do StockTicker. Além disso, torne o construtor público.

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

Em seguida, podemos refatorar o código criando uma interface para `StockTicker`. Usaremos essa interface para desacoplar a `StockTickerHub` da classe `StockTicker`.

O Visual Studio torna esse tipo de refatoração fácil. Abra o arquivo StockTicker.cs, clique com o botão direito do mouse na declaração de classe `StockTicker` e selecione **Refactor** ... **Extrair interface**.

![](dependency-injection/_static/image1.png)

Na caixa de diálogo **extrair interface** , clique em **selecionar tudo**. Deixe os outros padrões. Clique em **OK**.

![](dependency-injection/_static/image2.png)

O Visual Studio cria uma nova interface chamada `IStockTicker`e também altera `StockTicker` para derivar de `IStockTicker`.

Abra o arquivo IStockTicker.cs e altere a interface para **Public**.

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

Na classe `StockTickerHub`, altere as duas instâncias de `StockTicker` para `IStockTicker`:

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

A criação de uma interface de `IStockTicker` não é estritamente necessária, mas eu queria mostrar como a DI pode ajudar a reduzir o acoplamento entre componentes em seu aplicativo.

## <a name="add-the-ninject-library"></a>Adicionar a biblioteca Ninject

Há muitos contêineres IoC de código aberto para .NET. Para este tutorial, usarei [Ninject](http://www.ninject.org/). (Outras bibliotecas populares incluem [Castle Windsor](http://www.castleproject.org/), [Spring.net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity)e [StructureMap](http://docs.structuremap.net).)

Use o Gerenciador de pacotes NuGet para instalar a [biblioteca Ninject](https://nuget.org/packages/Ninject/3.0.1.10). No Visual Studio, no menu **ferramentas** , selecione **Gerenciador de pacotes NuGet** > **console do Gerenciador de pacotes**. Na janela Console do Gerenciador de Pacotes, digite o seguinte comando:

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a>Substituir o resolvedor de dependência do Signalr

Para usar o Ninject no Signalr, crie uma classe derivada de **DefaultDependencyResolver**.

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

Essa classe substitui os métodos **GetService** e **GetServices** de **DefaultDependencyResolver**. O signalr chama esses métodos para criar vários objetos em tempo de execução, incluindo instâncias de Hub, bem como vários serviços usados internamente pelo Signalr.

- O método **GetService** cria uma única instância de um tipo. Substitua esse método para chamar o método **TryGet** do kernel Ninject. Se esse método retornar NULL, volte para o resolvedor padrão.
- O método **GetServices** cria uma coleção de objetos de um tipo especificado. Substitua esse método para concatenar os resultados de Ninject com os resultados do resolvedor padrão.

## <a name="configure-ninject-bindings"></a>Configurar associações Ninject

Agora, usaremos Ninject para declarar associações de tipo.

Abra a classe Startup.cs do aplicativo (que você criou manualmente de acordo com as instruções do pacote em `readme.txt`ou que foi criado adicionando a autenticação ao seu projeto). No método `Startup.Configuration`, crie o contêiner Ninject, que Ninject chama o *kernel*.

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

Crie uma instância do nosso resolvedor de dependências personalizado:

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

Crie uma associação para `IStockTicker` da seguinte maneira:

[!code-csharp[Main](dependency-injection/samples/sample17.cs)]

Esse código está dizendo duas coisas. Primeiro, sempre que o aplicativo precisar de uma `IStockTicker`, o kernel deverá criar uma instância do `StockTicker`. Em segundo lugar, a classe `StockTicker` deve ser uma criada como um objeto singleton. Ninject criará uma instância do objeto e retornará a mesma instância para cada solicitação.

Crie uma associação para **IHubConnectionContext** da seguinte maneira:

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

Esse código cria uma função anônima que retorna um **IHubConnection**. O método **WhenInjectedInto** informa Ninject para usar essa função somente ao criar `IStockTicker` instâncias. O motivo é que o Signalr cria instâncias **IHubConnectionContext** internamente e não queremos substituir como o signalr as cria. Essa função se aplica somente à nossa classe de `StockTicker`.

Passe o resolvedor de dependência para o método **MapSignalR** adicionando uma configuração de Hub:

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

Atualize o método Startup. ConfigureSignalR na classe de inicialização do exemplo com o novo parâmetro:

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

Agora, o Signalr usará o resolvedor especificado em **MapSignalR**, em vez do resolvedor padrão.

Aqui está a listagem de código completa para `Startup.Configuration`.

[!code-csharp[Main](dependency-injection/samples/sample21.cs)]

Para executar o aplicativo StockTicker no Visual Studio, pressione F5. Na janela do navegador, navegue até `http://localhost:*port*/SignalR.Sample/StockTicker.html`.

![](dependency-injection/_static/image3.png)

O aplicativo tem exatamente a mesma funcionalidade que antes. (Para obter uma descrição, consulte [transmissão de servidor com signalr ASP.net](../getting-started/tutorial-server-broadcast-with-signalr.md).) Não alteramos o comportamento; apenas tornaria o código mais fácil de testar, manter e evoluir.
