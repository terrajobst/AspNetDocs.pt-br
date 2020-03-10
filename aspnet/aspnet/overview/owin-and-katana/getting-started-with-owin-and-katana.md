---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: Introdução com OWIN e Katana | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 09/27/2013
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: 4dfd7b8ebb2bb48d7ef800fd522b79a7b4a045c2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78584668"
---
# <a name="getting-started-with-owin-and-katana"></a>Introdução a OWIN e Katana

por [Mike Wasson](https://github.com/MikeWasson)

O [Open Web interface for .net (OWIN)](http://owin.org/) define uma abstração entre servidores Web e aplicativos Web do .net. Ao desacoplar o servidor Web do aplicativo, o OWIN facilita a criação de middleware para desenvolvimento para a Web .NET. Além disso, o OWIN facilita a porta de aplicativos Web para outros&#8212;hosts, por exemplo, hospedagem interna em um serviço do Windows ou outro processo.

OWIN é uma especificação de propriedade da Comunidade, não uma implementação. O projeto Katana é um conjunto de componentes de OWIN de software livre desenvolvidos pela Microsoft. Para obter uma visão geral do OWIN e do katana, consulte [uma visão geral do Project Katana](an-overview-of-project-katana.md). Neste artigo, vou ir direto para o código para começar.

Este tutorial usa [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), mas você também pode usar o Visual Studio 2012. Algumas das etapas são diferentes no Visual Studio 2012, que eu denotarei abaixo.

## <a name="host-owin-in-iis"></a>Host OWIN no IIS

Nesta seção, vamos hospedar OWIN no IIS. Essa opção oferece a flexibilidade e a capacidade de composição de um pipeline OWIN junto com o conjunto de recursos maduro do IIS. Usando essa opção, o aplicativo OWIN é executado no pipeline de solicitação ASP.NET.

Primeiro, crie um novo projeto de aplicativo Web ASP.NET. (No Visual Studio 2012, use o tipo de projeto de aplicativo Web ASP.NET vazio.)

![](getting-started-with-owin-and-katana/_static/image1.png)

Na caixa de diálogo **novo projeto ASP.net** , selecione o modelo **vazio** .

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a>Adicionar pacotes do NuGet

Em seguida, adicione os pacotes NuGet necessários. No menu **ferramentas** , selecione **Gerenciador de pacotes NuGet**e, em seguida, selecione **console do Gerenciador de pacotes**. Na janela do console do Gerenciador de pacotes, digite o seguinte comando:

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a>Adicionar uma classe de inicialização

Em seguida, adicione uma classe de inicialização OWIN. Em Gerenciador de Soluções, clique com o botão direito do mouse no projeto e selecione **Adicionar**e, em seguida, selecione **novo item**. Na caixa de diálogo **Adicionar novo item** , selecione **classe de inicialização Owin**. Para obter mais informações sobre como configurar a classe de inicialização, consulte [detecção de classe de inicialização OWIN](owin-startup-class-detection.md).

![](getting-started-with-owin-and-katana/_static/image4.png)

Adicione o seguinte código ao método `Startup1.Configuration`:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

Esse código adiciona uma parte simples do middleware ao pipeline OWIN, implementado como uma função que recebe uma instância **Microsoft. OWIN. IOwinContext** . Quando o servidor recebe uma solicitação HTTP, o pipeline OWIN invoca o middleware. O middleware define o tipo de conteúdo para a resposta e grava o corpo da resposta.

> [!NOTE]
> O modelo de classe de inicialização OWIN está disponível em Visual Studio 2013. Se você estiver usando o Visual Studio 2012, basta adicionar uma nova classe vazia denominada `Startup1`e colar o código a seguir:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a>Executar o aplicativo

Pressione F5 para iniciar a depuração. O Visual Studio abrirá uma janela do navegador para `http://localhost:*port*/`. A página deve ser parecida com a seguinte:

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a>OWIN de hospedagem interna em um aplicativo de console

É fácil converter esse aplicativo da hospedagem do IIS para hospedagem interna em um processo personalizado. Com a hospedagem do IIS, o IIS atua como o servidor HTTP e como o processo que hospeda o serviço. Com a hospedagem interna, seu aplicativo cria o processo e usa a classe **HttpListener** como o servidor http.

No Visual Studio, crie um novo aplicativo de console. Na janela do console do Gerenciador de pacotes, digite o seguinte comando:

`Install-Package Microsoft.Owin.SelfHost -Pre`

Adicione uma classe de `Startup1` da parte 1 deste tutorial ao projeto. Você não precisa modificar essa classe.

Implemente o método de `Main` do aplicativo da seguinte maneira.

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

Quando você executa o aplicativo de console, o servidor começa a escutar `http://localhost:9000`. Se você navegar até esse endereço em um navegador da Web, verá a página "Olá, mundo".

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a>Adicionar diagnóstico de OWIN

O pacote Microsoft. Owin. Diagnostics contém middleware que captura exceções sem tratamento e exibe uma página HTML com detalhes do erro. Essa página funciona de forma muito parecida com a página de erro ASP.NET que, às vezes, é chamada de "[tela amarela de morte](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD). Assim como o YSOD, a página de erro Katana é útil durante o desenvolvimento, mas é uma boa prática desabilitá-la no modo de produção.

Para instalar o pacote de diagnóstico em seu projeto, digite o seguinte comando na janela do console do Gerenciador de pacotes:

`install-package Microsoft.Owin.Diagnostics –Pre`

Altere o código em seu método `Startup1.Configuration` da seguinte maneira:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

Agora use CTRL + F5 para executar o aplicativo sem depuração, de modo que o Visual Studio não irá interromper a exceção. O aplicativo se comporta da mesma forma que antes, até que você navegue até `http://localhost/fail`, ponto em que o aplicativo gera a exceção. O middleware da página de erro irá capturar a exceção e exibir uma página HTML com informações sobre o erro. Você pode clicar nas guias para ver as variáveis de ambiente pilha, Cadeia de caracteres de consulta, cookies, cabeçalho de solicitação e OWIN.

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a>Próximas etapas

- [Detecção de classe de inicialização OWIN](owin-startup-class-detection.md)
- [Usar o OWIN para hospedar automaticamente ASP.NET Web API](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [Usar o OWIN para o Signalr de autohost](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
