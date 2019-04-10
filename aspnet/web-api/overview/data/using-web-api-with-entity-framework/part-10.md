---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Publicar o aplicativo no Azure App Service do Azure | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: a9a7b74a07c71b47253c0af304c7a26ffa4eaae5
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59417358"
---
# <a name="publish-the-app-to-azure-azure-app-service"></a>Publicar o aplicativo no Azure App Service do Azure

por [Mike Wasson](https://github.com/MikeWasson)

[Baixe o projeto concluído](https://github.com/MikeWasson/BookService)

Como a última etapa, você publicará o aplicativo no Azure. No Gerenciador de soluções, clique com botão direito no projeto e selecione **publicar**.

![](part-10/_static/image1.png)

Clicando em **Publish** invoca o **publicar na Web** caixa de diálogo. Se você tiver marcado **Host na nuvem** quando você criou o projeto e, em seguida, a conexão e já estão configuradas. Nesse caso, basta clicar o **as configurações** guia e verifique &quot;executar migrações do Code First&quot;. (Se você não marcou **Host na nuvem** no início, em seguida, siga as etapas a [próxima seção](#new-website).)

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

Para implantar o aplicativo, clique em **publicar**. Você pode exibir o andamento da publicação na **atividade de publicação de Web** janela. (Da **modo de exibição** menu, selecione **Other Windows**, em seguida, selecione **a atividade de publicação de Web**.)

![](part-10/_static/image4.png)

Quando o Visual Studio terminar a implantação do aplicativo, o navegador padrão abre automaticamente a URL do site da Web implantado e o aplicativo que você criou agora está em execução na nuvem. A URL na barra de endereços do navegador mostra que o site está sendo carregado da Internet.

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a>Implantando para um novo site

Se você não marcou **Host na nuvem** quando criou o projeto pela primeira vez, você pode configurar um novo aplicativo web agora. No Gerenciador de soluções, clique com botão direito no projeto e selecione **publicar**. Selecione o **perfil** guia e clique em **sites do Microsoft Azure**. Se você não entrou no Azure, você será solicitado a entrar.

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

No **sites existentes** caixa de diálogo, clique em **New**.

![](part-10/_static/image9.png)

Insira um nome de site. Selecione sua assinatura do Azure e a região. Sob **servidor de banco de dados**, selecione **criar novo servidor**, ou selecione um servidor existente. Clique em **Criar**.

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

Clique o **as configurações** guia e verifique &quot;executar migrações do Code First&quot;. Em seguida, clique em **publicar**.

> [!div class="step-by-step"]
> [Voltar](part-9.md)
