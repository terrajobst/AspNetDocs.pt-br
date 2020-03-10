---
uid: web-pages/overview/testing-and-debugging/introduction-to-debugging
title: Introdução à depuração de sites Páginas da Web do ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: A depuração é o processo de localizar e corrigir erros em suas páginas de código. Este capítulo mostra algumas ferramentas e técnicas que você pode usar para depurar e analyz...
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 68de4326-7611-4b9b-b5f6-79b7adc3069f
msc.legacyurl: /web-pages/overview/testing-and-debugging/introduction-to-debugging
msc.type: authoredcontent
ms.openlocfilehash: ae7d871e56326610c043dc20fe6e0919e1b4ac89
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78624365"
---
# <a name="introduction-to-debugging-aspnet-web-pages-razor-sites"></a>Introdução à depuração de sites Páginas da Web do ASP.NET (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo explica várias maneiras de depurar páginas em um site Páginas da Web do ASP.NET (Razor). A depuração é o processo de localizar e corrigir erros em suas páginas de código.
>
> **O que você aprenderá:**
>
> - Como exibir informações que ajudam a analisar e depurar páginas.
> - Como usar as ferramentas de depuração no Visual Studio.
>
>
> Estes são os recursos do ASP.NET apresentados no artigo:
>
> - O auxiliar de `ServerInfo`.
> - `ObjectInfo` auxiliar.
>
>
> ## <a name="software-versions"></a>Versões de software
>
>
> - Páginas da Web do ASP.NET (Razor) 3
> - Visual Studio 2013
>
>
> Este tutorial também funciona com o Páginas da Web do ASP.NET 2. Você pode usar o WebMatrix 3, mas não há suporte para o depurador integrado.

Um aspecto importante de solucionar problemas de erros e problemas em seu código é evitá-los em primeiro lugar. Você pode fazer isso colocando seções de seu código que provavelmente causarão erros em blocos de `try/catch`. Para obter mais informações, consulte a seção sobre como lidar [com erros na introdução à programação da Web do ASP.NET usando a sintaxe do Razor](https://go.microsoft.com/fwlink/?LinkId=202890).

O auxiliar de `ServerInfo` é uma ferramenta de diagnóstico que fornece uma visão geral das informações sobre o ambiente do servidor Web que hospeda sua página. Ele também mostra as informações de solicitação HTTP que são enviadas quando um navegador solicita a página. O auxiliar de `ServerInfo` exibe a identidade do usuário atual, o tipo de navegador que fez a solicitação e assim por diante. Esse tipo de informação pode ajudá-lo a solucionar problemas comuns.

1. Crie uma nova página da Web chamada *ServerInfo. cshtml*.
2. No final da página, logo antes da marca de `</body>` de fechamento, adicione `@ServerInfo.GetHtml()`:

    [!code-cshtml[Main](introduction-to-debugging/samples/sample1.cshtml)]

    Você pode adicionar o código de `ServerInfo` em qualquer lugar da página. Mas adicioná-lo no final manterá sua saída separada do conteúdo de outra página, o que facilitará a leitura.

    > [!NOTE]
    >
    > **Importante** Você deve remover qualquer código de diagnóstico de suas páginas da Web antes de mover as páginas da Web para um servidor de produção. Isso se aplica ao auxiliar de `ServerInfo`, bem como às outras técnicas de diagnóstico neste artigo que envolvem a adição de código a uma página. Você não quer que os visitantes do site vejam informações sobre o nome do servidor, nomes de usuário, caminhos no servidor e detalhes semelhantes, pois esse tipo de informação pode ser útil para pessoas com más intenções.
3. Salve a página e execute-a em um navegador.

    ![Depuração-1](introduction-to-debugging/_static/image1.jpg)

    O auxiliar de `ServerInfo` exibe quatro tabelas de informações na página:

   - Configuração do servidor. Esta seção fornece informações sobre o servidor Web de hospedagem, incluindo o nome do computador, a versão do ASP.NET que você está executando, o nome de domínio e a hora do servidor.
   - Variáveis de servidor ASP.NET. Esta seção fornece detalhes sobre os muitos detalhes do protocolo HTTP (chamados de variáveis HTTP) e os valores que fazem parte de cada solicitação de página da Web.
   - Informações de tempo de execução HTTP. Esta seção fornece detalhes sobre a versão do Microsoft .NET Framework na qual sua página da Web está sendo executada, o caminho, os detalhes sobre o cache e assim por diante. (Como você aprendeu na [introdução à programação da Web ASP.NET usando a sintaxe do Razor](https://go.microsoft.com/fwlink/?LinkId=202890), páginas da Web do ASP.NET usando a sintaxe Razor são criadas na tecnologia de servidor Web ASP.NET da Microsoft, que é criada em uma ampla biblioteca de desenvolvimento de software chamada .NET Framework.)
   - Variáveis de ambiente. Esta seção fornece uma lista de todas as variáveis de ambiente local e seus valores no servidor Web.

     Uma descrição completa de todas as informações de servidor e solicitação está além do escopo deste artigo, mas você pode ver que o auxiliar de `ServerInfo` retorna muitas informações de diagnóstico. Para obter mais informações sobre os valores que `ServerInfo` retorna, consulte [variáveis de ambiente reconhecidas](https://technet.microsoft.com/library/dd560744(WS.10).aspx) no site do Microsoft TechNet e nas [variáveis de servidor do IIS](https://msdn.microsoft.com/library/ms524602(VS.90).aspx) no site do MSDN.

## <a name="embedding-output-expressions-to-display-page-values"></a>Inserindo expressões de saída para exibir valores de página

Outra maneira de ver o que está acontecendo em seu código é inserir expressões de saída na página. Como você sabe, você pode produzir o valor de uma variável diretamente, adicionando algo como `@myVariable` ou `@(subTotal * 12)` à página. Para depuração, você pode posicionar essas expressões de saída em pontos estratégicos em seu código. Isso permite que você veja o valor de variáveis de chave ou o resultado de cálculos quando sua página é executada. Quando você terminar a depuração, poderá remover as expressões ou comentar. Este procedimento ilustra uma maneira típica de usar expressões inseridas para ajudar a depurar uma página.

1. Crie uma nova página do WebMatrix chamada *outputid. cshtml*.
2. Substitua o conteúdo da página pelo seguinte:

    [!code-html[Main](introduction-to-debugging/samples/sample2.html)]

    O exemplo usa uma instrução `switch` para verificar o valor da variável `weekday` e, em seguida, exibir uma mensagem de saída diferente, dependendo de qual dia da semana é. No exemplo, o bloco de `if` no primeiro bloco de código arbitrariamente altera o dia da semana adicionando um dia ao valor atual do dia da semana. Esse é um erro introduzido para fins de ilustração.
3. Salve a página e execute-a em um navegador.

    A página exibe a mensagem para o dia da semana errado. Seja qual for o dia da semana na verdade, você verá a mensagem para um dia mais tarde. Embora, nesse caso, você saiba por que a mensagem está desativada (porque o código determina deliberadamente o valor de dia incorreto), na realidade, é difícil saber onde as coisas estão erradas no código. Para depurar, você precisa descobrir o que está acontecendo com o valor de objetos-chave e variáveis como `weekday`.
4. Adicione expressões de saída inserindo `@weekday` conforme mostrado nos dois locais indicados por comentários no código. Essas expressões de saída exibirão os valores da variável nesse ponto na execução do código.

    [!code-csharp[Main](introduction-to-debugging/samples/sample3.cs?highlight=2-3,15-16)]
5. Salve e execute a página em um navegador.

    A página exibe o dia real da semana primeiro, depois o dia atualizado da semana resultante da adição de um dia e a mensagem resultante da instrução `switch`. A saída das duas expressões de variáveis (`@weekday`) não tem espaços entre os dias porque você não adicionou nenhuma marca HTML `<p>` à saída; as expressões são apenas para teste.

    ![Depuração-2](introduction-to-debugging/_static/image2.jpg)

    Agora você pode ver onde está o erro. Quando você exibe a variável `weekday` no código pela primeira vez, ela mostra o dia correto. Quando você o exibe na segunda vez, após o bloco de `if` no código, o dia é desativado em um. Portanto, você sabe que algo aconteceu entre a primeira e a segunda aparência da variável Weekday. Se esse fosse um bug real, esse tipo de abordagem o ajudaria a restringir o local do código que está causando o problema.
6. Corrija o código na página removendo as duas expressões de saída adicionadas e removendo o código que altera o dia da semana. O bloco de código completo restante é semelhante ao exemplo a seguir:

    [!code-cshtml[Main](introduction-to-debugging/samples/sample4.cshtml)]
7. Execute a página em um navegador. Desta vez, você verá a mensagem correta exibida para o dia real da semana.

## <a name="using-the-objectinfo-helper-to-display-object-values"></a>Usando o auxiliar ObjectInfo para exibir valores de objeto

O auxiliar de `ObjectInfo` exibe o tipo e o valor de cada objeto que você passa para ele. Você pode usá-lo para exibir o valor de variáveis e objetos em seu código (como você fez com expressões de saída no exemplo anterior), além de poder ver informações de tipo de dados sobre o objeto.

1. Abra o arquivo chamado *OutputTable. cshtml* que você criou anteriormente.
2. Substitua todo o código na página pelo seguinte bloco de código:

    [!code-html[Main](introduction-to-debugging/samples/sample5.html)]
3. Salve e execute a página em um navegador.

    ![Depuração-4](introduction-to-debugging/_static/image3.jpg)

    Neste exemplo, o auxiliar `ObjectInfo` exibe dois itens:

   - O tipo. Para a primeira variável, o tipo é `DayOfWeek`. Para a segunda variável, o tipo é `String`.
   - O valor. Nesse caso, como você já exibe o valor da variável greeting na página, o valor é exibido novamente quando você passa a variável para `ObjectInfo`.

     Para objetos mais complexos, o auxiliar de `ObjectInfo` pode exibir mais &#8212; informações basicamente, ele pode exibir os tipos e valores de todas as propriedades de um objeto.

## <a name="using-debugging-tools-in-visual-studio"></a>Usando ferramentas de depuração no Visual Studio

Para uma experiência de depuração mais abrangente, use o [Visual Studio](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017). Com o Visual Studio, você pode definir um ponto de interrupção em seu código na linha que você deseja inspecionar.

![Definir ponto de interrupção](introduction-to-debugging/_static/image1.png)

Quando você testa o site da Web, o código de execução é interrompido no ponto de interrupção.

![ponto de interrupção de alcance](introduction-to-debugging/_static/image2.png)

Você pode examinar os valores atuais das variáveis e percorrer o código linha por linha.

![Ver valores](introduction-to-debugging/_static/image3.png)

Para obter informações sobre como usar o depurador integrado no Visual Studio para depurar páginas ASP.NET Razor, consulte [Programming páginas da Web do ASP.net (Razor) usando o Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854).

## <a name="additional-resources"></a>Recursos adicionais

- [Páginas da Web do ASP.NET de programação (Razor) usando o Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854)
- [Variáveis do servidor IIS](https://msdn.microsoft.com/library/ms524602(VS.90).aspx) (MSDN)
- [Variáveis de ambiente reconhecidas](https://technet.microsoft.com/library/dd560744(WS.10).aspx) (TechNet)
