---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
title: 'Implantação da Web do ASP.NET usando o Visual Studio: Implantando arquivos extras | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um aplicativo Web ASP.NET em aplicativos Web do serviço Azure App ou em um provedor de Hospedagem de terceiros, por usin...
ms.author: riande
ms.date: 03/23/2015
ms.assetid: 1cd91055-84bc-42c6-9d80-646f41429d4d
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
msc.type: authoredcontent
ms.openlocfilehash: eaa3141c22980f0c816e2f33b5597ac9fe69c23c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74594900"
---
# <a name="aspnet-web-deployment-using-visual-studio-deploying-extra-files"></a>Implantação da Web do ASP.NET usando o Visual Studio: Implantando arquivos extras

por [Tom Dykstra](https://github.com/tdykstra)

[Baixar o projeto inicial](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta série de tutoriais mostra como implantar (publicar) um aplicativo Web ASP.NET em aplicativos Web do serviço Azure App ou em um provedor de Hospedagem de terceiros usando o Visual Studio 2012 ou o Visual Studio 2010. Para obter informações sobre a série, consulte [o primeiro tutorial da série](introduction.md).

## <a name="overview"></a>{1&gt;Visão Geral&lt;1}

Este tutorial mostra como estender o pipeline de publicação da Web do Visual Studio para realizar uma tarefa adicional durante a implantação. A tarefa é copiar arquivos extras que não estão na pasta do projeto para o site de destino.

Para este tutorial, você copiará um arquivo extra: *robots. txt*. Você deseja implantar esse arquivo para preparo, mas não para produção. No tutorial [implantando na produção](deploying-to-production.md) , você adicionou esse arquivo ao projeto e configurou o perfil de publicação de produção para excluí-lo. Neste tutorial, você verá um método alternativo para lidar com essa situação, uma que será útil para todos os arquivos que você deseja implantar, mas que não deseja incluir no projeto.

## <a name="move-the-robotstxt-file"></a>Mover o arquivo robots. txt

Para se preparar para um método diferente de manipulação do *robots. txt*, nesta seção do tutorial, você move o arquivo para uma pasta que não está incluída no projeto e exclui o *robots. txt* do ambiente de preparo. É necessário excluir o arquivo de preparo para que você possa verificar se o novo método de implantação do arquivo nesse ambiente está funcionando corretamente.

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no arquivo *robots. txt* e clique em **excluir do projeto**.
2. Usando o explorador de arquivos do Windows, crie uma nova pasta na pasta da solução e nomeie-a *ExtraFiles*.
3. Mova o arquivo *robots. txt* da pasta do projeto *ContosoUniversity* para a pasta *ExtraFiles* .

    ![Pasta ExtraFiles](deploying-extra-files/_static/image1.png)
4. Usando a ferramenta FTP, exclua o arquivo *robots. txt* do site de preparo.

    Como alternativa, você pode selecionar **remover arquivos adicionais no destino** em **Opções de publicação de arquivo** na guia **configurações** do perfil de publicação de preparo e republicar no preparo.

## <a name="update-the-publish-profile-file"></a>Atualizar o arquivo de perfil de publicação

Você precisa apenas de *robots. txt* no preparo, portanto, o único perfil de publicação que você precisa atualizar para implantá-lo é o preparo.

1. No Visual Studio, abra *preparo. pubxml*.
2. No final do arquivo, antes da marca de `</Project>` de fechamento, adicione a seguinte marcação:

    [!code-xml[Main](deploying-extra-files/samples/sample1.xml)]

    Esse código cria um novo *destino* que coletará arquivos adicionais a serem implantados. Um destino é composto por uma ou mais tarefas que serão executadas pelo MSBuild com base nas condições especificadas.

    O atributo `Include` especifica que a pasta na qual os arquivos são encontrados é *ExtraFiles*, localizada no mesmo nível que a pasta do projeto. O MSBuild coletará todos os arquivos dessa pasta e recursivamente de quaisquer subpastas (o asterisco duplo especifica subpastas recursivas). Com esse código, você pode colocar vários arquivos e arquivos em subpastas dentro da pasta *ExtraFiles* e todos serão implantados.

    O elemento `DestinationRelativePath` especifica que as pastas e os arquivos devem ser copiados para a pasta raiz do site de destino, na mesma estrutura de arquivos e pastas que são encontrados na pasta *ExtraFiles* . Se você quisesse copiar a pasta *ExtraFiles* , o valor `DestinationRelativePath` seria *ExtraFiles\%(RecursiveDir)% (filename)% (Extension)* .
3. No final do arquivo, antes da marca de `</Project>` de fechamento, adicione a marcação a seguir que especifica quando executar o novo destino.

    [!code-xml[Main](deploying-extra-files/samples/sample2.xml)]

    Esse código faz com que o novo destino de `CustomCollectFiles` seja executado sempre que o destino que copia arquivos para a pasta de destino é executado. Há um destino separado para a criação de pacote de publicação e implantação, e o novo destino é injetado em ambos os destinos, caso você decida implantar usando um pacote de implantação em vez de publicar.

    O arquivo *. pubxml* agora é semelhante ao exemplo a seguir:

    [!code-xml[Main](deploying-extra-files/samples/sample3.xml?highlight=53-71)]
4. Salve e feche o arquivo *. pubxml de preparo* .

## <a name="publish-to-staging"></a>Publicar para preparo

Usando a publicação de um clique ou a linha de comando, publique o aplicativo usando o perfil de preparo.

Se você usar a publicação com um clique, poderá verificar na janela de **Visualização** que o *robots. txt* será copiado. Caso contrário, use a ferramenta de FTP para verificar se o arquivo *robots. txt* está na pasta raiz do site da Web após a implantação.

## <a name="summary"></a>Resumo

Isso conclui esta série de tutoriais sobre como implantar um aplicativo Web ASP.NET em um provedor de Hospedagem de terceiros. Para obter mais informações sobre qualquer um dos tópicos abordados nesses tutoriais, consulte o [mapa de conteúdo de implantação do ASP.net](https://go.microsoft.com/fwlink/p/?LinkId=282413).

## <a name="more-information"></a>Mais informações

Se você souber como trabalhar com arquivos MSBuild, poderá automatizar muitas outras tarefas de implantação escrevendo código em arquivos *. pubxml* (para tarefas específicas de perfil) ou o arquivo Project *. WPP. targets* (para tarefas que se aplicam a todos os perfis). Para obter mais informações sobre arquivos *. pubxml* e *. WPP. targets* , consulte [como: Editar configurações de implantação em arquivos de perfil de publicação (. pubxml) e o arquivo. WPP. targets em projetos Web do Visual Studio](https://msdn.microsoft.com/library/ff398069). Para obter uma introdução básica ao código do MSBuild, consulte **a anatomia de um arquivo de projeto** na [série de implantação corporativa: Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Para saber como trabalhar com arquivos MSBuild para executar tarefas para seus próprios cenários, consulte este livro: [dentro do Microsoft Build Engine: usando o MSBuild e o Team Foundation Build](http://msbuildbook.com) de Sayed Ibraham Hashimi e William Bartholomew.

## <a name="acknowledgements"></a>Agradecimentos

Gostaria de agradecer às seguintes pessoas que fizeram contribuições significativas para o conteúdo desta série de tutoriais:

- [Alberto Poblacion, MVP &amp; MCT, Espanha](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- Jarod Ferguson, MVP de desenvolvimento de plataforma de dados, Estados Unidos
- Adverso do Mittal, Microsoft
- [Jon Galloway](https://weblogs.asp.net/jgalloway) (twitter: [@jongalloway](http://twitter.com/jongalloway))
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike Papa, Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava, Microsoft
- [Raffaele Rialdi, Itália](http://www.iamraf.net/)
- [Rick Anderson, Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [@sayedihashimi](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](http://twitter.com/shanselman))
- [Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [@coolcsh](http://twitter.com/coolcsh))
- [Srđan Božović, Sérvia](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [@vishalrjoshi](http://twitter.com/vishalrjoshi))

> [!div class="step-by-step"]
> [Anterior](command-line-deployment.md)
> [Próximo](troubleshooting.md)
