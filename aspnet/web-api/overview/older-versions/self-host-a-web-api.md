---
uid: web-api/overview/older-versions/self-host-a-web-api
title: Auto-host ASP.NET Web API 1 (C#)-ASP.NET 4. x
author: MikeWasson
description: O tutorial com código mostra como hospedar uma API Web dentro de um aplicativo de console.
ms.author: riande
ms.date: 01/26/2012
ms.custom: seoapril2019
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: bae1737ba5b16bc67fa0ed0474ff04df0add1b3a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78525084"
---
# <a name="self-host-aspnet-web-api-1-c"></a>Auto-host ASP.NET Web API 1 (C#)

por [Mike Wasson](https://github.com/MikeWasson)

> Este tutorial mostra como hospedar uma API Web dentro de um aplicativo de console. ASP.NET Web API não exige o IIS. Você pode hospedar automaticamente uma API Web em seu próprio processo de host. 
> 
> **Os novos aplicativos devem usar o OWIN para a API Web de hospedagem própria.** Confira [usar o OWIN para o autohost ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - API Web 1
> - Visual Studio 2012

## <a name="create-the-console-application-project"></a>Criar o projeto de aplicativo de console

Inicie o Visual Studio e selecione **novo projeto** na página **inicial** . Ou, no menu **arquivo** , selecione **novo** e **projeto**.

No painel **modelos** , selecione **modelos instalados** e expanda o **nó C# Visual** . Em **Visual C#** , selecione **Windows**. Na lista de modelos de projeto, selecione **aplicativo de console**. Nomeie o projeto &quot;SelfHost&quot; e clique em **OK**.

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a>Definir a estrutura de destino (Visual Studio 2010)

Se você estiver usando o Visual Studio 2010, altere a estrutura de destino para .NET Framework 4,0. (Por padrão, o modelo de projeto destina-se ao [perfil de cliente do .NET Framework](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)

Em Gerenciador de Soluções, clique com o botão direito do mouse no projeto e selecione **Propriedades**. Na lista suspensa **estrutura de destino** , altere a estrutura de destino para .NET Framework 4,0. Quando for solicitado a aplicar a alteração, clique em **Sim**.

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a>Instalar o Gerenciador de pacotes NuGet

O Gerenciador de pacotes NuGet é a maneira mais fácil de adicionar os assemblies da API Web a um projeto do non-ASP.NET.

Para verificar se o Gerenciador de pacotes NuGet está instalado, clique no menu **ferramentas** no Visual Studio. Se você vir um item de menu chamado **Gerenciador de pacotes NuGet**, você terá o Gerenciador de pacotes NuGet.

Para instalar o Gerenciador de pacotes NuGet:

1. Inicie o Visual Studio.
2. No menu **Ferramentas**, selecione **Extensões e atualizações**.
3. Na caixa de diálogo **extensões e atualizações** , selecione **online**.
4. Se você não vir o "Gerenciador de pacotes NuGet", digite "Gerenciador de pacotes NuGet" na caixa de pesquisa.
5. Selecione o Gerenciador de pacotes NuGet e clique em **baixar**.
6. Após a conclusão do download, você será solicitado a instalar o.
7. Após a conclusão da instalação, talvez seja solicitado que você reinicie o Visual Studio.

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a>Adicionar o pacote NuGet da API Web

Depois que o Gerenciador de pacotes NuGet estiver instalado, adicione o pacote de autohospedagem da API Web ao seu projeto.

1. No menu **ferramentas** , selecione **Gerenciador de pacotes NuGet**. *Observação*: se você não vir esse item de menu, verifique se o Gerenciador de pacotes NuGet foi instalado corretamente.
2. Selecione **gerenciar pacotes NuGet para solução**
3. Na caixa de diálogo **gerenciar pacotes do preciosidade** , selecione **online**.
4. Na caixa de pesquisa, digite &quot;Microsoft. AspNet. WebApi. SelfHost&quot;.
5. Selecione o pacote do ASP.NET Web API Self host e clique em **instalar**.
6. Após a instalação do pacote, clique em **fechar** para fechar a caixa de diálogo.

> [!NOTE]
> Certifique-se de instalar o pacote denominado Microsoft. AspNet. WebApi. SelfHost, não AspNetWebApi. SelfHost.

![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a>Criar o modelo e o controlador

Este tutorial usa as mesmas classes de modelo e controlador do tutorial de [introdução](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) .

Adicione uma classe pública chamada `Product`.

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

Adicione uma classe pública chamada `ProductsController`. Derive essa classe de **System. Web. http. ApiController**.

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

Para obter mais informações sobre o código nesse controlador, consulte o tutorial de [introdução](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) . Esse controlador define três ações GET:

| URI | DESCRIÇÃO |
| --- | --- |
| /api/products | Obtenha uma lista de todos os produtos. |
| *ID* do/API/Products/ | Obter um produto por ID. |
| /API/Products/? categoria =*categoria* | Obter uma lista de produtos por categoria. |

## <a name="host-the-web-api"></a>Hospedar a API Web

Abra o arquivo Program.cs e adicione as seguintes instruções using:

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

Adicione o código a seguir à classe **programa** .

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a>Adicional Adicionar uma reserva de namespace de URL HTTP

Este aplicativo escuta `http://localhost:8080/`. Por padrão, a escuta em um endereço HTTP específico requer privilégios de administrador. Ao executar o tutorial, portanto, você pode receber esse erro: "HTTP não pôde registrar a URL http://+:8080/" há duas maneiras de evitar esse erro:

- Execute o Visual Studio com permissões de administrador elevadas ou
- Use o netsh. exe para conceder permissões de conta para reservar a URL.

Para usar o netsh. exe, abra um prompt de comando com privilégios de administrador e insira o comando a seguir:

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

em que *nomedeusuário* é sua conta de usuário.

Quando você terminar a hospedagem interna, certifique-se de excluir a reserva:

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a>Chamar a API da Web de um aplicativo clienteC#()

Vamos escrever um aplicativo de console simples que chama a API da Web.

Adicione um novo projeto de aplicativo de console à solução:

- Em Gerenciador de Soluções, clique com o botão direito do mouse na solução e selecione **Adicionar novo projeto**.
- Crie um novo aplicativo de console chamado &quot;ClientApp&quot;.

![](self-host-a-web-api/_static/image5.png)

Use o Gerenciador de pacotes NuGet para adicionar o pacote ASP.NET Web API Core Libraries:

- No menu ferramentas, selecione **Gerenciador de pacotes NuGet**.
- Selecione **gerenciar pacotes NuGet para solução**
- Na caixa de diálogo **gerenciar pacotes NuGet** , selecione **online**.
- Na caixa de pesquisa, digite &quot;Microsoft. AspNet. WebApi. Client&quot;.
- Selecione o pacote Microsoft ASP.NET bibliotecas de cliente da API Web e clique em **instalar**.

Adicione uma referência em ClientApp ao projeto SelfHost:

- Em Gerenciador de Soluções, clique com o botão direito do mouse no projeto ClientApp.
- Selecione **Adicionar Referência**.
- Na caixa de diálogo **Gerenciador de referências** , em **solução**, selecione **projetos**.
- Selecione o projeto SelfHost.
- Clique em **OK**.

![](self-host-a-web-api/_static/image6.png)

Abra o arquivo cliente/programa. cs. Adicione a seguinte instrução **using** :

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

Adicione uma instância **HttpClient** estática:

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

Adicione os seguintes métodos para listar todos os produtos, listar um produto por ID e listar produtos por categoria.

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

Cada um desses métodos segue o mesmo padrão:

1. Chame **HttpClient. getasync** para enviar uma solicitação get para o URI apropriado.
2. Chame **HttpResponseMessage. EnsureSuccessStatusCode**. Esse método gera uma exceção se o status de resposta HTTP é um código de erro.
3. Chame **ReadAsAsync&lt;t&gt;** para desserializar um tipo CLR da resposta http. Esse método é um método de extensão, definido em **System .net. http. HttpContentExtensions**.

Os métodos **getasync** e **ReadAsAsync** são assíncronos. Elas retornam objetos de **tarefa** que representam a operação assíncrona. Obter a propriedade **Result** bloqueia o thread até que a operação seja concluída.

Para obter mais informações sobre como usar HttpClient, incluindo como fazer chamadas sem bloqueio, consulte [chamando uma API da Web de um cliente .net](../advanced/calling-a-web-api-from-a-net-client.md).

Antes de chamar esses métodos, defina a propriedade BaseAddress na instância HttpClient como "`http://localhost:8080`". Por exemplo:

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

Isso deve gerar o seguinte. (Lembre-se de executar o aplicativo SelfHost primeiro.)

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
