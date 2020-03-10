---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Publicar o aplicativo no serviço de Azure App do Azure | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: a9a7b74a07c71b47253c0af304c7a26ffa4eaae5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622384"
---
# <a name="publish-the-app-to-azure-azure-app-service"></a>Publicar o aplicativo no serviço de Azure App do Azure

por [Mike Wasson](https://github.com/MikeWasson)

[Baixar projeto concluído](https://github.com/MikeWasson/BookService)

Como a última etapa, você publicará o aplicativo no Azure. Em Gerenciador de Soluções, clique com o botão direito do mouse no projeto e selecione **publicar**.

![](part-10/_static/image1.png)

Clicar em **publicar** invoca a caixa de diálogo **publicar na Web** . Se você tiver marcado **host na nuvem** quando criou o projeto pela primeira vez, a conexão e as configurações já estarão configuradas. Nesse caso, basta clicar na guia **configurações** e verificar &quot;executar migrações do Code First&quot;. (Se você não verificou o **host na nuvem** no início, siga as etapas na [próxima seção](#new-website).)

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

Para implantar o aplicativo, clique em **publicar**. Você pode exibir o progresso da publicação na janela **atividade de publicação na Web** . (No menu **Exibir** , selecione **outras janelas**e, em seguida, selecione **atividade de publicação na Web**.)

![](part-10/_static/image4.png)

Quando o Visual Studio termina de implantar o aplicativo, o navegador padrão é aberto automaticamente na URL do site implantado e o aplicativo que você criou agora está em execução na nuvem. A URL na barra de endereços do navegador mostra que o site está sendo carregado da Internet.

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a>Implantando em um novo site

Se você não verificou o **host na nuvem** quando criou o projeto pela primeira vez, você pode configurar um novo aplicativo Web agora. Em Gerenciador de Soluções, clique com o botão direito do mouse no projeto e selecione **publicar**. Selecione a guia **perfil** e clique em **Microsoft Azure sites**. Se você não estiver conectado atualmente ao Azure, você será solicitado a entrar.

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

Na caixa de diálogo **sites existentes** , clique em **novo**.

![](part-10/_static/image9.png)

Insira um nome de site. Selecione sua assinatura do Azure e a região. Em **servidor de banco de dados**, selecione **criar novo servidor**ou selecione um servidor existente. Clique em **Criar**.

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

Clique na guia **configurações** e marque &quot;executar migrações do Code First&quot;. Em seguida, clique em **Publicar**.

> [!div class="step-by-step"]
> [Anterior](part-9.md)
