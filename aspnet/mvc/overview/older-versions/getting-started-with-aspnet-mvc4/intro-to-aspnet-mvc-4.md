---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
title: Introdução ao ASP.NET MVC 4 | Microsoft Docs
author: Rick-Anderson
description: Uma versão atualizada se este tutorial estiver disponível aqui usando Visual Studio 2013. O novo tutorial usa o ASP.NET MVC 5, que fornece muitas melhorias em relação a t...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: ed66530a-04d5-49eb-b76a-85be1f57c437
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 51709a9c6ddb39b8fcd1cd94cd08d530a595825a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78599641"
---
# <a name="intro-to-aspnet-mvc-4"></a>Introdução ao ASP.NET MVC 4

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> Uma versão atualizada se este tutorial estiver disponível [aqui](../../getting-started/introduction/getting-started.md) usando [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013). O novo tutorial usa o ASP.NET MVC 5, que fornece vários aprimoramentos sobre este tutorial.
>
> Este tutorial ensinará as noções básicas da criação de um aplicativo Web ASP.NET MVC 4 usando o Microsoft [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) ou o Visual Web developer 2010 Express Service Pack 1. O Visual Studio 2012 é recomendado, você não precisará instalar nada para concluir o tutorial. Se você estiver usando o Visual Studio 2010, deverá instalar os componentes abaixo. Você pode instalar todos eles clicando nos links a seguir:
>
> - [Pré-requisitos do Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Instalador de WPI para ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)
> - [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0)
> - [SSDT](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx)
>
> Se você estiver usando o Visual Studio 2010 em vez do Visual Web Developer 2010, instale o [instalador de WPI para ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392) e o: [pré-requisitos do Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
>
> Um projeto do Visual Web Developer com C# código-fonte está disponível para acompanhar este tópico. [Baixe a C# versão](https://code.msdn.microsoft.com/Intro-to-ASPNET-MVC-4-61d0219d/file/114480/1/MvcMovie.zip).
>
> No tutorial, você executa o aplicativo no Visual Studio. Você também pode tornar o aplicativo disponível pela Internet implantando-o em um provedor de hospedagem. A Microsoft oferece hospedagem na Web gratuita para até 10 sites em uma [conta de avaliação gratuita do Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Para obter informações sobre como implantar um projeto Web do Visual Studio em um site do Windows Azure, consulte [criar e implantar um site ASP.net e um banco de dados SQL com o Visual Studio](https://docs.microsoft.com/dotnet/azure/). Esse tutorial também mostra como usar Migrações do Entity Framework Code First para implantar o banco de dados do SQL Server no banco de dados SQL do Windows Azure (anteriormente SQL Azure).
>
> Este tutorial foi escrito por Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ).

## <a name="what-youll-build"></a>O que você vai construir

> [!NOTE]
> Uma versão atualizada se este tutorial estiver disponível [aqui](../../getting-started/introduction/getting-started.md) usando [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013). O novo tutorial usa o ASP.NET MVC 5, que fornece vários aprimoramentos sobre este tutorial.

Você implementará um aplicativo simples de listagem de filmes que dá suporte à criação, edição, pesquisa e listagem de filmes de um banco de dados. Abaixo estão duas capturas de tela do aplicativo que você criará. Ele inclui uma página que exibe uma lista de filmes de um banco de dados:

![](intro-to-aspnet-mvc-4/_static/image1.png)

O aplicativo também permite que você adicione, edite e exclua filmes, bem como veja detalhes sobre aqueles individuais. Todos os cenários de entrada de dados incluem validação para garantir que os dados armazenados no banco de dado estejam corretos.

![](intro-to-aspnet-mvc-4/_static/image2.png)

## <a name="getting-started"></a>Introdução

Comece executando o Visual Studio Express 2012 ou o Visual Web Developer 2010 Express. A maioria das capturas de tela nesta série usa Visual Studio Express 2012, mas você pode concluir este tutorial com o Visual Studio 2010/SP1, Visual Studio 2012, Visual Studio Express 2012 ou Visual Web Developer 2010 Express. Selecione **novo projeto** na página **inicial** .

O Visual Studio é um IDE ou um ambiente de desenvolvimento integrado. Assim como você usa o Microsoft Word para escrever documentos, você usará um IDE para criar aplicativos. No Visual Studio, há uma barra de ferramentas ao longo da parte superior mostrando várias opções disponíveis. Há também um menu que fornece outra maneira de executar tarefas no IDE. (Por exemplo, em vez de selecionar **novo projeto** na página **inicial** , você pode usar o menu e selecionar **arquivo** &gt; **novo projeto**.)

![](intro-to-aspnet-mvc-4/_static/image3.png)

## <a name="creating-your-first-application"></a>Criando seu primeiro aplicativo

Você pode criar aplicativos usando o Visual Basic ou o C# Visual como a linguagem de programação. Selecione Visual C# à esquerda e, em seguida, selecione **aplicativo Web ASP.NET MVC 4**. Nomeie seu projeto &quot;MvcMovie&quot; e clique em **OK**.

![](intro-to-aspnet-mvc-4/_static/image4.png)

Na caixa de diálogo **novo projeto do ASP.NET MVC 4** , selecione **aplicativo de Internet**. Deixe o **Razor** como o mecanismo de exibição padrão.

![](intro-to-aspnet-mvc-4/_static/image5.png)

Clique em **OK**. O Visual Studio usou um modelo padrão para o projeto MVC ASP.NET que você acabou de criar, portanto, você tem um aplicativo funcional agora sem fazer nada! Este é um &quot;simples Olá, Mundo!&quot; projeto e é um bom lugar para iniciar seu aplicativo.

![](intro-to-aspnet-mvc-4/_static/image6.png)

No menu **Depuração**, selecione **Iniciar Depuração**.

![](intro-to-aspnet-mvc-4/_static/image7.png)

Observe que o atalho de teclado para iniciar a depuração é F5.

F5 faz com que o Visual Studio inicie IIS Express e execute seu aplicativo Web. Em seguida, o Visual Studio inicia um navegador e abre a home page do aplicativo. Observe que a barra de endereços do navegador indica `localhost` e não algo como `example.com`. Isso ocorre porque `localhost` sempre aponta para seu próprio computador local, que nesse caso está executando o aplicativo que você acabou de criar. Quando o Visual Studio executa um projeto Web, uma porta aleatória é usada para o servidor Web. Na imagem abaixo, o número da porta é 41788. Ao executar o aplicativo, você provavelmente verá um número de porta diferente.

![](intro-to-aspnet-mvc-4/_static/image8.png)

Pronto para uso, esse modelo padrão fornece as páginas inicial, contato e sobre. Ele também fornece suporte para registrar e fazer logon e links para o Facebook e o Twitter. A próxima etapa é alterar a forma como esse aplicativo funciona e aprender um pouco sobre o ASP.NET MVC. Feche o navegador e vamos alterar algum código.

> [!div class="step-by-step"]
> [Próximo](adding-a-controller.md)
