---
uid: mvc/overview/getting-started/introduction/getting-started
title: Introdução com o ASP.NET MVC 5 | Microsoft Docs
author: Rick-Anderson
ms.author: riande
ms.date: 10/04/2018
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: c74daa37f68dda641cae97d3b0c19718f62d474d
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456381"
---
# <a name="getting-started-with-aspnet-mvc-5"></a>Introdução ao ASP.NET MVC 5

por [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [consider RP](../../../../includes/razor.md)]

Este tutorial ensina as noções básicas da criação de um aplicativo Web ASP.NET MVC 5 usando o [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017). O código-fonte final do tutorial está localizado no [GitHub](https://github.com/aspnet/AspNetDocs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie).

Este tutorial foi escrito por [Scott Guthrie](https://weblogs.asp.net/scottgu/) (Twitter[@scottgu](https://twitter.com/scottgu) ), [scott Hanselman](http://www.hanselman.com/blog/) (Twitter: [@shanselman](https://twitter.com/shanselman) ) e [Rick Anderson](https://twitter.com/RickAndMSFT) ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) )

Você precisa de uma conta do Azure para implantar este aplicativo no Azure:

- Você pode [abrir uma conta do Azure gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -você obtém créditos que podem ser usados para experimentar serviços pagos do Azure e, mesmo depois que eles são usados, você pode manter a conta e usar os serviços gratuitos do Azure.
- Você pode [ativar benefícios para assinantes do MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Todos os meses, sua assinatura do MSDN oferece créditos que podem ser usados para serviços pagos do Azure.

## <a name="get-started"></a>Introdução

Comece [instalando o Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017). Em seguida, abra o Visual Studio.

O Visual Studio é um IDE ou um ambiente de desenvolvimento integrado. Assim como você usa o Microsoft Word para escrever documentos, você usará um IDE para criar aplicativos. No Visual Studio, há uma lista ao longo da parte inferior mostrando várias opções disponíveis para você. Há também um menu que fornece outra maneira de executar tarefas no IDE. Por exemplo, em vez de selecionar **novo projeto** na **página inicial**, você pode usar a barra de menus e selecionar **arquivo** > **novo projeto**.

![](getting-started/_static/image1.png)

## <a name="create-your-first-app"></a>Crie seu primeiro aplicativo

Na **página inicial**, selecione **novo projeto**. Na caixa de diálogo **novo projeto** , selecione a **categoria C# Visual** à esquerda, em seguida **Web**e, em seguida, selecione o modelo de projeto do **aplicativo Web ASP.net (.NET Framework)** . Nomeie seu projeto como "MvcMovie" e escolha **OK**.

![](getting-started/_static/image2.png)

Na caixa de diálogo **novo aplicativo Web ASP.net** , escolha **MVC** e, em seguida, escolha **OK**.

![](getting-started/_static/image3.png)

O Visual Studio usou um modelo padrão para o projeto MVC ASP.NET que você acabou de criar, portanto, você tem um aplicativo funcional agora sem fazer nada! Este é um "Olá, Mundo!" simples projeto, e é um bom lugar para iniciar seu aplicativo.

![](getting-started/_static/image4.png)

Pressione **F5** para iniciar a depuração. Quando você pressiona **F5**, o Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) e executa seu aplicativo Web. Em seguida, o Visual Studio inicia um navegador e abre a home page do aplicativo. Observe que a barra de endereços do navegador indica `localhost:port#` e não algo como `example.com`. Isso ocorre porque `localhost` sempre aponta para seu próprio computador local, que nesse caso está executando o aplicativo que você acabou de criar. Quando o Visual Studio executa um projeto Web, uma porta aleatória é usada para o servidor Web. Na imagem abaixo, o número da porta é 1234. Ao executar o aplicativo, você verá um número de porta diferente.

![](getting-started/_static/image5.png)

Pronto para uso, esse modelo padrão fornece as páginas `Home`, `Contact`e `About`. A imagem abaixo não mostra os links **início**, **sobre**e **contato** . Dependendo do tamanho da janela do navegador, talvez seja necessário clicar no ícone de navegação para ver esses links.

![](getting-started/_static/image6.png)

O aplicativo também fornece suporte para registrar e fazer logon. A próxima etapa é alterar a forma como esse aplicativo funciona e aprender um pouco sobre o ASP.NET MVC. Feche o aplicativo MVC ASP.NET e vamos alterar algum código.

Para obter uma lista dos tutoriais atuais, consulte os [artigos recomendados do MVC](../mvc-learning-sequence.md).

## <a name="see-this-app-running-on-azure"></a>Consulte este aplicativo em execução no Azure

Você gostaria de ver o site concluído em execução como um aplicativo Web ativo? Você pode implantar uma versão completa do aplicativo em sua conta do Azure simplesmente clicando no botão a seguir.

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/AspNetDocs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

Você precisa de uma conta do Azure para implantar essa solução no Azure. Se você ainda não tiver uma conta, use uma das seguintes opções para criar uma:

- [Abra uma conta do Azure gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -você obtém créditos que podem ser usados para experimentar serviços pagos do Azure e, mesmo depois que eles são usados, você pode manter a conta e usar os serviços gratuitos do Azure.
- [Ativar os benefícios do assinante do Visual Studio](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers) -sua assinatura do Visual Studio fornece créditos todos os meses que você pode usar para serviços pagos do Azure.

> [!div class="step-by-step"]
> [Próximo](adding-a-controller.md)
