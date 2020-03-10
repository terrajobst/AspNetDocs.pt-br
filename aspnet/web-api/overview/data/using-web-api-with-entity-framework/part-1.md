---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Usando a API Web 2 com Entity Framework 6 | Microsoft Docs
author: MikeWasson
description: Este tutorial ensinará as noções básicas da criação de um aplicativo Web com um back-end ASP.NET Web API. O tutorial usa Entity Framework 6 para o Lay-in de dados...
ms.author: riande
ms.date: 01/17/2019
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: 0f5dc960f494af5bd4ce87863a510d1892319908
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622475"
---
# <a name="using-web-api-2-with-entity-framework-6"></a>Uso da API Web 2 com o Entity Framework 6

[Baixar projeto concluído](https://github.com/MikeWasson/BookService)

> Este tutorial ensina as noções básicas da criação de um aplicativo Web com um back-end ASP.NET Web API. O tutorial usa Entity Framework 6 para a camada de dados e knockout. js para o aplicativo JavaScript do lado do cliente. O tutorial também mostra como implantar o aplicativo em aplicativos Web do Azure App Service.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
>
> - API Web 2,1
> - Visual Studio 2017 (Baixe o Visual Studio 2017 [aqui](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))
> - Entity Framework 6
> - .NET 4.7
> - [Knockout. js](http://knockoutjs.com/) 3,1

Este tutorial usa ASP.NET Web API 2 com Entity Framework 6 para criar um aplicativo Web que manipula um banco de dados back-end. Aqui está uma captura de tela do aplicativo que você criará.

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

O aplicativo usa um design de aplicativo de página única (SPA). "Aplicativo de página única" é o termo geral para um aplicativo Web que carrega uma única página HTML e, em seguida, atualiza a página dinamicamente, em vez de carregar novas páginas. Após o carregamento inicial da página, o aplicativo conversa com o servidor por meio de solicitações AJAX. As solicitações AJAX retornam dados JSON, que o aplicativo usa para atualizar a interface do usuário.

O AJAX não é novo, mas hoje há estruturas de JavaScript que facilitam a criação e a manutenção de um aplicativo SPA sofisticado grande. Este tutorial usa [knockout. js](http://knockoutjs.com/), mas você pode usar qualquer estrutura de cliente JavaScript.

Aqui estão os principais blocos de construção para este aplicativo:

- O ASP.NET MVC cria a página HTML.
- ASP.NET Web API manipula as solicitações AJAX e retorna dados JSON.
- Dados knockout. js – associa os elementos HTML aos dados JSON.
- Entity Framework conversa com o banco de dados.

## <a name="see-this-app-running-on-azure"></a>Consulte este aplicativo em execução no Azure

Você gostaria de ver o site concluído em execução como um aplicativo Web ativo? Você pode implantar uma versão completa do aplicativo em sua conta do Azure selecionando o botão a seguir.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

Você precisa de uma conta do Azure para implantar essa solução no Azure. Se você ainda não tiver uma conta, terá as seguintes opções:

- [Abra uma conta do Azure gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -você obtém créditos que podem ser usados para experimentar serviços pagos do Azure e, mesmo depois que eles são usados, você pode manter a conta e usar os serviços gratuitos do Azure.
- [Ativar os benefícios do assinante do MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -sua assinatura do MSDN fornece créditos todos os meses que você pode usar para serviços pagos do Azure.

## <a name="create-the-project"></a>Criar o projeto

Abra o Visual Studio. No menu **arquivo** , selecione **novo**e **projeto**. (Ou selecione **novo projeto** na página inicial.)

Na caixa de diálogo **novo projeto** , selecione **Web** no painel esquerdo e **ASP.NET aplicativo Web (.NET Framework)** no painel central. Nomeie o projeto **BookService** e selecione **OK**.

[![](part-1/_static/image11.png)](part-1/_static/image11.png)

Na caixa de diálogo **novo projeto ASP.net** , selecione o modelo **API Web** .

[![](part-1/_static/image12.png)](part-1/_static/image12.png)

Selecione **OK** para criar o projeto.

## <a name="configure-azure-settings-optional"></a>Definir configurações do Azure (opcional)

Depois de criar o projeto, você pode optar por implantar o Azure App aplicativos Web do serviço a qualquer momento. 

1. Em Gerenciador de Soluções, clique com o botão direito do mouse em seu projeto e selecione **publicar**.

2. Na janela exibida, selecione **Iniciar**. A caixa de diálogo **selecionar um destino de publicação** é exibida.

   [![](part-1/_static/image14.png)](part-1/_static/image14.png)

3. Selecione **Criar Perfil**. A caixa de diálogo **Criar Serviço de Aplicativo** é exibida.

   [![](part-1/_static/image15.png)](part-1/_static/image15.png)

   Aceite os padrões ou insira valores diferentes para o nome do aplicativo, grupo de recursos, plano de hospedagem, assinatura do Azure e região geográfica. 

4. Selecione **criar um banco de dados SQL**. A caixa de diálogo **configurar SQL Server** é exibida. 

   [![](part-1/_static/image16.png)](part-1/_static/image16.png)

   Aceite os padrões ou insira valores diferentes. Insira um **nome de usuário de administrador** e uma **senha de administrador** para o novo banco de dados. Selecione **OK** quando terminar. A página **Criar serviço de aplicativo** é exibida novamente.

5. Selecione **criar** para criar seu perfil. Uma mensagem é exibida no canto inferior direito, indicando que a implantação está em andamento. Depois de um pouco, a janela de **publicação** reaparece.

    [![](part-1/_static/image17.png)](part-1/_static/image17.png)
   
    O perfil que você criou para implantar o aplicativo agora está disponível. 

> [!div class="step-by-step"]
> [Próximo](part-2.md)
