---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: 'Tutorial: auto-host do Signalr | Microsoft Docs'
author: bradygaster
description: Este tutorial mostra como criar um servidor Signaler 2 auto-hospedado e como se conectar a ele com um cliente JavaScript. Versões de software usadas no tutorial V...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: 41c8c3803923e76ef238a5c5937cbe7f81e6aa82
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558677"
---
# <a name="tutorial-signalr-self-host"></a>Tutorial: auto-host do Signalr

por [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

[Baixar projeto concluído](https://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> Este tutorial mostra como criar um servidor Signaler 2 auto-hospedado e como se conectar a ele com um cliente JavaScript.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - Sinalização versão 2
>
>
>
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>Usando o Visual Studio 2012 com este tutorial
>
>
> Para usar o Visual Studio 2012 com este tutorial, faça o seguinte:
>
> - Atualize o [Gerenciador de pacotes](http://docs.nuget.org/docs/start-here/installing-nuget) para a versão mais recente.
> - Instale o [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).
> - No Web Platform Installer, procure e instale o **ASP.NET and Web Tools 2013,1 para Visual Studio 2012**. Isso instalará modelos do Visual Studio para classes de Signalr, como **Hub**.
> - Alguns modelos (como a **classe de inicialização OWIN**) não estarão disponíveis; para isso, use um arquivo de classe em vez disso.
>
>
> ## <a name="questions-and-comments"></a>Perguntas e comentários
>
> Deixe comentários sobre como você gostou deste tutorial e o que poderíamos melhorar nos comentários na parte inferior da página. Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, poderá lançá-las no fórum do [signalr ASP.net](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [stackoverflow.com](http://stackoverflow.com/).

## <a name="overview"></a>Visão geral

Geralmente, um servidor de sinalização é hospedado em um aplicativo ASP.NET no IIS, mas também pode ser hospedado internamente (como em um aplicativo de console ou serviço do Windows) usando a biblioteca de autohost. Essa biblioteca, como todo o Signalr 2, se baseia no OWIN ([Open Web interface para .net](http://owin.org)). OWIN define uma abstração entre os servidores Web e os aplicativos Web do .NET. OWIN dissocia o aplicativo Web do servidor, o que torna o OWIN ideal para hospedar internamente um aplicativo Web em seu próprio processo, fora do IIS.

Os motivos para não hospedar no IIS incluem:

- Ambientes em que o IIS não está disponível ou é desejável, como um farm de servidores existente sem o IIS.
- A sobrecarga de desempenho do IIS precisa ser evitada.
- A funcionalidade do signalr deve ser adicionada a um aplicativo existente que é executado em um serviço do Windows, na função de trabalho do Azure ou em outro processo.

Se uma solução estiver sendo desenvolvida como hospedagem interna por motivos de desempenho, é recomendável também testar o aplicativo hospedado no IIS para determinar o benefício do desempenho.

Este tutorial contém as seguintes seções:

- [Criando o servidor](#server)
- [Acessando o servidor com um cliente JavaScript](#js)

<a id="server"></a>

## <a name="creating-the-server"></a>Criando o servidor

Neste tutorial, você criará um servidor que está hospedado em um aplicativo de console, mas o servidor pode ser hospedado em qualquer tipo de processo, como um serviço do Windows ou uma função de trabalho do Azure. Para obter um exemplo de código para hospedar um servidor de sinalização em um serviço do Windows, consulte [auto-hospedagem de signalr em um serviço do Windows](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).

1. Abra Visual Studio 2013 com privilégios de administrador. Selecione **arquivo**, **novo projeto**. Selecione **Windows** no nó **Visual C#**  no painel **modelos** e selecione o modelo aplicativo de **console** . Nomeie o novo projeto como "SignalRSelfHost" e clique em **OK**.

    ![](tutorial-signalr-self-host/_static/image1.png)
2. Abra o console do Gerenciador de pacotes NuGet selecionando **ferramentas** > **Gerenciador de pacotes NuGet** > **console do Gerenciador de pacotes**.
3. No console do Gerenciador de pacotes, digite o seguinte comando:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    Esse comando adiciona as bibliotecas de hospedagem interna do Signalr 2 ao projeto.
4. No console do Gerenciador de pacotes, digite o seguinte comando:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    Esse comando adiciona a biblioteca Microsoft. Owin. CORS ao projeto. Essa biblioteca será usada para suporte entre domínios, que é necessário para aplicativos que hospedam o Signalr e um cliente de página da Web em domínios diferentes. Como você hospedará o servidor do Signalr e o cliente Web em portas diferentes, isso significa que o domínio cruzado deve estar habilitado para comunicação entre esses componentes.
5. Substitua os conteúdos do Program.cs pelo código a seguir.

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    O código acima inclui três classes:

    - **Programa**, incluindo o método **Main** que define o caminho principal de execução. Nesse método, um aplicativo Web do tipo **Startup** é iniciado na URL especificada (`http://localhost:8080`). Se a segurança for necessária no ponto de extremidade, o SSL poderá ser implementado. Consulte [como: configurar uma porta com um certificado SSL](https://msdn.microsoft.com/library/ms733791.aspx) para obter mais informações.
    - **Inicialização**, a classe que contém a configuração para o servidor signalr (a única configuração que este tutorial usa é a chamada para `UseCors`) e a chamada para `MapSignalR`, que cria rotas para quaisquer objetos de Hub no projeto.
    - **MyHub**, a classe de Hub do signalr que o aplicativo fornecerá aos clientes. Essa classe tem um único método, **Send**, que os clientes chamarão para transmitir uma mensagem a todos os outros clientes conectados.
6. Compile e execute o aplicativo. O endereço que o servidor está executando deve aparecer em uma janela de console.

    ![](tutorial-signalr-self-host/_static/image2.png)
7. Se a execução falhar com a exceção `System.Reflection.TargetInvocationException was unhandled`, será necessário reiniciar o Visual Studio com privilégios de administrador.
8. Pare o aplicativo antes de prosseguir para a próxima seção.

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a>Acessando o servidor com um cliente JavaScript

Nesta seção, você usará o mesmo cliente JavaScript do [tutorial introdução](../getting-started/tutorial-getting-started-with-signalr.md). Faremos apenas uma modificação no cliente, que é definir explicitamente a URL do Hub. Com um aplicativo auto-hospedado, o servidor pode não estar necessariamente no mesmo endereço que a URL de conexão (devido a proxies invertidos e balanceadores de carga), portanto, a URL precisa ser definida explicitamente.

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse na solução e selecione **Adicionar**, **novo projeto**. Selecione o nó **da Web** e selecione o modelo de **aplicativo Web ASP.net** . Nomeie o projeto "JavascriptClient" e clique em **OK**.

    ![](tutorial-signalr-self-host/_static/image3.png)
2. Selecione o modelo **vazio** e deixe as opções restantes desmarcadas. Selecione **criar projeto**.

    ![](tutorial-signalr-self-host/_static/image4.png)
3. No console do Gerenciador de pacotes, selecione o projeto "JavascriptClient" na lista suspensa **projeto padrão** e execute o seguinte comando:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    Esse comando instala as bibliotecas Signalr e JQuery que você precisará no cliente.
4. Clique com o botão direito do mouse em seu projeto e selecione **Adicionar**, **novo item**. Selecione o nó **da Web** e selecione página HTML. Nomeie a página **como default. html**.

    ![](tutorial-signalr-self-host/_static/image5.png)
5. Substitua o conteúdo da nova página HTML pelo código a seguir. Verifique se as referências de script aqui correspondem aos scripts na pasta scripts do projeto.

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    O código a seguir (realçado no exemplo de código acima) é a adição que você fez ao cliente usado no tutorial de introdução (além de atualizar o código para o Signalr versão 2 beta). Essa linha de código define explicitamente a URL de conexão base para o Signalr no servidor.

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. Clique com o botão direito do mouse na solução e selecione **definir projetos de inicialização...** . Selecione o botão de opção **vários projetos de inicialização** e defina a **ação** de ambos os projetos como **Iniciar**.

    ![](tutorial-signalr-self-host/_static/image6.png)
7. Clique com o botão direito do mouse em "default. html" e selecione **definir como página inicial**.
8. Execute o aplicativo. O servidor e a página serão iniciados. Talvez seja necessário recarregar a página da Web (ou selecione **continuar** no depurador) se a página for carregada antes de o servidor ser iniciado.
9. No navegador, forneça um nome de usuário quando solicitado. Copie a URL da página em outra guia ou janela do navegador e forneça um nome de usuário diferente. Você poderá enviar mensagens de um painel do navegador para o outro, como no tutorial de Introdução.
