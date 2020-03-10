---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: Detecção de classe de inicialização OWIN | Microsoft Docs
author: Praburaj
description: Este tutorial mostra como configurar qual classe de inicialização OWIN é carregada. Para obter mais informações sobre o OWIN, consulte uma visão geral do Project katana. Este tutorial foi...
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: e1670c32049ec33dd4d1941a091a429d3929c452
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78617043"
---
# <a name="owin-startup-class-detection"></a>Detecção de classe de inicialização OWIN

> Este tutorial mostra como configurar qual classe de inicialização OWIN é carregada. Para obter mais informações sobre o OWIN, consulte [uma visão geral do Project Katana](an-overview-of-project-katana.md). Este tutorial foi escrito por Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan e Howard dierking ( [@howard\_Dierking](https://twitter.com/howard_dierking) ).
>
> ## <a name="prerequisites"></a>Prerequisites
>
> [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)

## <a name="owin-startup-class-detection"></a>Detecção de classe de inicialização OWIN

 Cada aplicativo OWIN tem uma classe de inicialização na qual você especifica componentes para o pipeline de aplicativo. Há diferentes maneiras pelas quais você pode conectar sua classe de inicialização com o tempo de execução, dependendo do modelo de hospedagem que você escolher (OwinHost, IIS e IIS-Express). A classe de inicialização mostrada neste tutorial pode ser usada em todos os aplicativos de hospedagem. Você conecta a classe de inicialização ao tempo de execução de hospedagem usando uma das abordagens a seguir:

1. **Convenção de nomenclatura**: Katana procura uma classe chamada `Startup` no namespace que corresponde ao nome do assembly ou ao namespace global.
2. **Atributo OwinStartup**: essa é a abordagem que a maioria dos desenvolvedores demorará para especificar a classe de inicialização. O atributo a seguir definirá a classe de inicialização para a classe `TestStartup` no namespace `StartupDemo`.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

   O atributo `OwinStartup` substitui a Convenção de nomenclatura. Você também pode especificar um nome amigável com esse atributo, no entanto, usar um nome amigável requer que você também use o elemento `appSetting` no arquivo de configuração.
3. **O elemento appSetting no arquivo de configuração**: o elemento `appSetting` substitui o atributo `OwinStartup` e a Convenção de nomenclatura. Você pode ter várias classes de inicialização (cada uma usando um atributo `OwinStartup`) e configurar qual classe de inicialização será carregada em um arquivo de configuração usando marcação semelhante à seguinte:

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

   A chave a seguir, que especifica explicitamente a classe de inicialização e o assembly também pode ser usada:

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

   O XML a seguir no arquivo de configuração especifica um nome de classe de inicialização amigável de `ProductionConfiguration`.

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

   A marcação acima deve ser usada com o atributo `OwinStartup` a seguir, que especifica um nome amigável e faz com que a classe `ProductionStartup2` seja executada.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. Para desabilitar a descoberta de inicialização do OWIN, adicione o `appSetting owin:AutomaticAppStartup` com um valor de `"false"` no arquivo Web. config.

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a>Criar um aplicativo Web ASP.NET usando a inicialização do OWIN

1. Crie um aplicativo Web Asp.Net vazio e nomeie-o **StartupDemo**. -Instale `Microsoft.Owin.Host.SystemWeb` usando o Gerenciador de pacotes NuGet. No menu **ferramentas** , selecione **Gerenciador de pacotes NuGet**e, em seguida, **console do Gerenciador de pacotes**. Insira o seguinte comando:

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
2. Adicione uma classe de inicialização OWIN. No Visual Studio 2017, clique com o botão direito do mouse no projeto e selecione **Adicionar classe**.-na caixa de diálogo **Adicionar novo item** , digite *OWIN* no campo de pesquisa e altere o nome para startup.cs e, em seguida, selecione **Adicionar**.

     ![](owin-startup-class-detection/_static/image1.png)

   Na próxima vez que você quiser adicionar uma *classe de inicialização Owin*, ela estará disponível no menu **Adicionar** .

     ![](owin-startup-class-detection/_static/image2.png)

   Como alternativa, você pode clicar com o botão direito do mouse no projeto e selecionar **Adicionar**, selecionar **novo item**e, em seguida, selecionar a **classe de inicialização Owin**.

     ![](owin-startup-class-detection/_static/image3.png)

- Substitua o código gerado no arquivo *Startup.cs* pelo seguinte:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]

  A `app.Use` expressão lambda é usada para registrar o componente de middleware especificado no pipeline OWIN. Nesse caso, estamos Configurando o log de solicitações de entrada antes de responder à solicitação de entrada. O parâmetro `next` é o delegado ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [Task](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt;) para o próximo componente no pipeline. O `app.Run` expressão lambda conecta o pipeline a solicitações de entrada e fornece o mecanismo de resposta.
     > [!NOTE]
     > No código acima, comentamos o atributo `OwinStartup` e estamos contando com a Convenção de executar a classe chamada `Startup`.-Pressione ***F5*** para executar o aplicativo. Pressione atualizar algumas vezes.

    ![](owin-startup-class-detection/_static/image4.png) Observação: o número mostrado nas imagens neste tutorial não corresponderá ao número que você vê. A cadeia de caracteres de milissegundos é usada para mostrar uma nova resposta quando você atualiza a página.
  Você pode ver as informações de rastreamento na janela **saída** .

    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a>Adicionar mais classes de inicialização

Nesta seção, vamos adicionar outra classe Startup. Você pode adicionar várias classes de inicialização OWIN ao seu aplicativo. Por exemplo, talvez você queira criar classes de inicialização para desenvolvimento, teste e produção.

1. Crie uma nova classe de inicialização OWIN e nomeie-a `ProductionStartup`.
2. Substitua o código gerado pelo seguinte:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. Pressione Control F5 para executar o aplicativo. O atributo `OwinStartup` especifica que a classe de inicialização de produção é executada.

    ![](owin-startup-class-detection/_static/image6.png)
4. Crie outra classe de inicialização OWIN e nomeie-a `TestStartup`.
5. Substitua o código gerado pelo seguinte:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

   A sobrecarga de atributo `OwinStartup` acima Especifica `TestingConfiguration` como o nome *amigável* da classe de inicialização.
6. Abra o arquivo *Web. config* e adicione a chave de inicialização do aplicativo OWIN que especifica o nome amigável da classe de inicialização:

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. Pressione Control F5 para executar o aplicativo. O elemento de configurações do aplicativo usa o precedente e a configuração de teste é executada.

    ![](owin-startup-class-detection/_static/image7.png)
8. Remova o nome *amigável* do atributo `OwinStartup` na classe `TestStartup`.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. Substitua a chave de inicialização do aplicativo OWIN no arquivo *Web. config* pelo seguinte:

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. Reverta o atributo `OwinStartup` em cada classe para o código de atributo padrão gerado pelo Visual Studio:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

    Cada uma das chaves de inicialização do aplicativo OWIN abaixo fará com que a classe de produção seja executada.

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

    A última chave de inicialização especifica o método de configuração de inicialização. A chave de inicialização do aplicativo OWIN a seguir permite que você altere o nome da classe de configuração para `MyConfiguration`.

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a>Usando Owinhost. exe

1. Substitua o arquivo Web. config pela seguinte marcação:

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

   A última chave vence, portanto, nesse caso `TestStartup` é especificado.
2. Instale o Owinhost do PMC:

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. Navegue até a pasta do aplicativo (a pasta que contém o arquivo *Web. config* ) e, em um prompt de comando, digite:

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

   A janela de comando mostrará:

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. Inicie um navegador com a URL `http://localhost:5000/`.

    ![](owin-startup-class-detection/_static/image8.png)

   OwinHost homenageau as convenções de inicialização listadas acima.
5. Na janela de comando, pressione ENTER para sair de OwinHost.
6. Na classe `ProductionStartup`, adicione o seguinte atributo OwinStartup que especifica um nome amigável de *ProductionConfiguration*.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. No prompt de comando e digite:

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

   A classe de inicialização de produção é carregada.
    ![](owin-startup-class-detection/_static/image9.png)

   Nosso aplicativo tem várias classes de inicialização e, neste exemplo, adiamos qual classe de inicialização carregar até o tempo de execução.
8. Teste as seguintes opções de inicialização de tempo de execução:

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
