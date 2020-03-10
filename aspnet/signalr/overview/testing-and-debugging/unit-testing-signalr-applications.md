---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: Aplicativos de sinalizador de teste de unidade | Microsoft Docs
author: bradygaster
description: Este artigo descreve como usar os recursos de teste de unidade do Signalr 2,0.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: 2cf2e88f141d89971439dc1fc4979849f8dded47
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578669"
---
# <a name="unit-testing-signalr-applications"></a>Teste de unidade em aplicativos do SignalR

por [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Este artigo descreve como usar os recursos de teste de unidade do Signalr 2.
>
> ## <a name="software-versions-used-in-this-topic"></a>Versões de software usadas neste tópico
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - Sinalização versão 2
>
>
>
> ## <a name="questions-and-comments"></a>Perguntas e comentários
>
> Deixe comentários sobre como você gostou deste tutorial e o que poderíamos melhorar nos comentários na parte inferior da página. Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, poderá lançá-las no fórum do [signalr ASP.net](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [stackoverflow.com](http://stackoverflow.com/).

<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a>Aplicativos de sinalizador de teste de unidade

Você pode usar os recursos de teste de unidade no Signalr 2 para criar testes de unidade para seu aplicativo Signalr. O signalr 2 inclui a interface [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) , que pode ser usada para criar um objeto fictício para simular seus métodos de Hub para teste.

Nesta seção, você adicionará testes de unidade para o aplicativo criado no [tutorial de introdução](../getting-started/tutorial-getting-started-with-signalr.md) usando [xUnit.net](https://github.com/xunit/xunit) e [MOQ](https://github.com/Moq/moq4).

XUnit.net será usado para controlar o teste; MOQ será usado para criar um objeto [fictício](http://en.wikipedia.org/wiki/Mock_object) para teste. Outras estruturas fictícias podem ser usadas se desejado; [NSubstitute](http://nsubstitute.github.io/) também é uma boa opção. Este tutorial demonstra como configurar o objeto fictício de duas maneiras: primeiro, usando um objeto `dynamic` (introduzido no .NET Framework 4) e segundo, usando uma interface.

### <a name="contents"></a>Conteúdo

Este tutorial contém as seções a seguir.

- [Teste de unidade com dinâmico](#dynamic)
- [Teste de unidade por tipo](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a>Teste de unidade com dinâmico

Nesta seção, você adicionará um teste de unidade para o aplicativo criado no [tutorial de introdução](../getting-started/tutorial-getting-started-with-signalr.md) usando um objeto dinâmico.

1. Instale a [extensão do executor xUnit](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) para Visual Studio 2013.
2. Conclua o [tutorial de introdução](../getting-started/tutorial-getting-started-with-signalr.md)ou baixe o aplicativo concluído na [Galeria de códigos do MSDN](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).
3. Se você estiver usando a versão de download do aplicativo Introdução, abra o **console do Gerenciador de pacotes** e clique em **restaurar** para adicionar o pacote do signalr ao projeto.

    ![Restaurar pacotes](unit-testing-signalr-applications/_static/image1.png)
4. Adicione um projeto à solução para o teste de unidade. Clique com o botão direito do mouse em sua solução em **Gerenciador de soluções** e selecione **Adicionar**, **novo projeto...** . No **C#** nó, selecione o nó **Windows** . Selecione **biblioteca de classes**. Nomeie o novo projeto **TestLibrary** e clique em **OK**.

    ![Criar biblioteca de testes](unit-testing-signalr-applications/_static/image2.png)
5. Adicione uma referência no projeto de biblioteca de teste ao projeto SignalRChat. Clique com o botão direito do mouse no projeto **TestLibrary** e selecione **Adicionar**, **referência...** . Selecione o nó **projetos** no nó da **solução** e marque **SignalRChat**. Clique em **OK**.

    ![Adicionar referência de projeto](unit-testing-signalr-applications/_static/image3.png)
6. Adicione os pacotes Signalr, MOQ e XUnit ao projeto **TestLibrary** . No **console do Gerenciador de pacotes**, defina a lista suspensa **projeto padrão** como **TestLibrary**. Execute os seguintes comandos na janela do console:

   - `Install-Package Microsoft.AspNet.SignalR`
   - `Install-Package Moq`
   - `Install-Package XUnit`

     ![Instalar pacotes](unit-testing-signalr-applications/_static/image4.png)
7. Crie o arquivo de teste. Clique com o botão direito do mouse no projeto **TestLibrary** e clique em **Adicionar...** , **classe**. Nomeie a nova classe **Tests.cs**.
8. Substitua o conteúdo de Tests.cs pelo código a seguir.

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    No código acima, um cliente de teste é criado usando o objeto `Mock` da biblioteca [MOQ](https://github.com/Moq/moq4) , do tipo [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (no signalr 2,1, atribua `dynamic` para o parâmetro de tipo.) A interface `IHubCallerConnectionContext` é o objeto proxy com o qual você invoca métodos no cliente. A função `broadcastMessage` é então definida para o cliente fictício para que possa ser chamada pela classe `ChatHub`. Em seguida, o mecanismo de teste chama o método `Send` da classe `ChatHub`, que, por sua vez, chama a função de `broadcastMessage` fictícia.
9. Crie a solução pressionando **F6**.
10. Execute o teste de unidade. No Visual Studio, selecione **teste**, **Windows**, **Gerenciador de testes**. Na janela Gerenciador de testes, clique com o botão direito do mouse em **HubsAreMockableViaDynamic** e selecione **executar testes selecionados**.

    ![Gerenciador de Testes](unit-testing-signalr-applications/_static/image5.png)
11. Verifique se o teste foi aprovado, verificando o painel inferior na janela Gerenciador de testes. A janela mostrará que o teste foi aprovado.

    ![Teste aprovado](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a>Teste de unidade por tipo

Nesta seção, você adicionará um teste para o aplicativo criado no tutorial de [introdução](../getting-started/tutorial-getting-started-with-signalr.md) usando uma interface que contém o método a ser testado.

1. Conclua as etapas 1-7 no [teste de unidade com](#dynamic) o tutorial dinâmico acima.
2. Substitua o conteúdo de Tests.cs pelo código a seguir.

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    No código acima, uma interface é criada definindo a assinatura do método `broadcastMessage` para o qual o mecanismo de teste criará um cliente fictício. Um cliente fictício é criado usando o objeto `Mock`, do tipo [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (no signalr 2,1, atribui `dynamic` para o parâmetro de tipo.) A interface `IHubCallerConnectionContext` é o objeto proxy com o qual você invoca métodos no cliente.

    Em seguida, o teste cria uma instância de `ChatHub`e, em seguida, cria uma versão fictícia do método `broadcastMessage`, que, por sua vez, é invocado chamando o método `Send` no Hub.
3. Crie a solução pressionando **F6**.
4. Execute o teste de unidade. No Visual Studio, selecione **teste**, **Windows**, **Gerenciador de testes**. Na janela Gerenciador de testes, clique com o botão direito do mouse em **HubsAreMockableViaDynamic** e selecione **executar testes selecionados**.

    ![Gerenciador de Testes](unit-testing-signalr-applications/_static/image7.png)
5. Verifique se o teste foi aprovado, verificando o painel inferior na janela Gerenciador de testes. A janela mostrará que o teste foi aprovado.

    ![Teste aprovado](unit-testing-signalr-applications/_static/image8.png)
