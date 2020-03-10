---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: Páginas da Web do ASP.NET de programação (Razor) usando o Visual Studio | Microsoft Docs
author: Rick-Anderson
description: Este apêndice explica como você pode usar o Visual Studio 2010 ou o Visual Web Developer 2010 Express para programar Páginas da Web do ASP.NET com o sintaxe Razor.
ms.author: riande
ms.date: 02/13/2014
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: 1a76098779d05912bf7bdf2de5fdce024770752c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78633507"
---
# <a name="programming-aspnet-web-pages-razor-using-visual-studio"></a>Páginas da Web do ASP.NET de programação (Razor) usando o Visual Studio

por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo explica como você pode usar o Visual Studio ou o Visual Web Developer Express para programar sites Páginas da Web do ASP.NET (Razor).
>
> O que você aprenderá
>
> - O que você precisa instalar (se houver alguma coisa) para trabalhar com Páginas da Web do ASP.NET em sua versão do Visual Studio.
> - Como adicionar suporte para Páginas da Web do ASP.NET ao Visual Web Developer 2010 Express.
> - Como usar os recursos do Visual Studio para trabalhar com páginas Razor do ASP.NET, incluindo o IntelliSense e o depurador.
>
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
>
>
> - Páginas da Web do ASP.NET (Razor) 3
> - Visual Studio 2013
> - WebMatrix 3
>
>
> Este tutorial também funciona com Páginas da Web do ASP.NET 2, Visual Studio 2012, Visual Studio 2010 e WebMatrix 2.

Você pode programar páginas da Web ASP.NET com sintaxe Razor usando o WebMatrix ou muitos outros editores de código. Você também pode usar o Microsoft Visual Studio que é um ambiente de desenvolvimento integrado completo (IDE) que fornece um conjunto poderoso de ferramentas para criar vários tipos de aplicativos (não apenas sites). Para trabalhar com páginas Razor do ASP.NET, você pode usar o [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).

Dois recursos particularmente úteis que o Visual Studio fornece para programação com páginas da Web ASP.NET Razor são:

- *IntelliSense*. O recurso IntelliSense interno do Visual Studio é mais abrangente do que o IntelliSense no WebMatrix.
- *Depurador*. O depurador permite solucionar problemas de seu código interrompendo um programa enquanto ele está em execução, examinando variáveis e percorrendo o código linha por linha.

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a>Usando o Visual Studio com versões diferentes do Páginas da Web do ASP.NET

Para desenvolver aplicativos Web ASP.NET no Visual Studio 2017, instale a carga de trabalho de **desenvolvimento de ASP.net e Web** .

O Visual Studio 2012 e o Visual Studio 2013 incluem suporte para Páginas da Web do ASP.NET. (Os pacotes necessários para dar suporte ao Páginas da Web do ASP.NET são instalados quando você instala o Visual Studio.)

O Visual Studio 2010 não inclui suporte por padrão para Páginas da Web do ASP.NET. Para usar Páginas da Web do ASP.NET com o Visual Studio 2010, você deve instalar o pacote MVC do ASP.NET. Para obter Páginas da Web do ASP.NET 2, instale o ASP.NET MVC 4.

A tabela a seguir resume o suporte para Páginas da Web do ASP.NET em versões diferentes do Visual Studio.

|  | Visual Studio 2010 | Visual Studio 2012 | Visual Studio 2013 |
| --- | --- | --- | --- |
| **Páginas da Web do ASP.NET 2** | Instalar o ASP.NET MVC 4 | Incluídos | Incluídos |
| **Páginas da Web do ASP.NET 3** |  | Atualizar para o Páginas da Web do ASP.NET 3 por meio do NuGet | Incluídos |

Para trabalhar com o Visual Studio 2010, consulte [instalando o suporte para páginas da Web do ASP.net no visual studio 2010](#vs2010support).

## <a name="launching-visual-studio-from-webmatrix"></a>Iniciando o Visual Studio do WebMatrix

Se você tiver iniciado um projeto no WebMatrix e quiser alternar para o Visual Studio, o WebMatrix fornecerá um botão para abrir facilmente o projeto no Visual Studio. Você deve ter o Visual Studio instalado em seu computador para que este botão seja habilitado. A imagem a seguir mostra o botão no WebMatrix.

![iniciar o Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

Quando você clica no botão, o projeto é aberto no Visual Studio. Você pode alternar entre o WebMatrix e o Visual Studio sem nenhum problema. Você será notificado se algum arquivo tiver sido alterado no outro ambiente e precisar ser recarregado para obter as alterações mais recentes.

## <a name="creating-aspnet-razor-site-in-visual-studio"></a>Criando o site do Razor do ASP.NET no Visual Studio

Para criar um site do Razor do ASP.NET no Visual Studio:

1. Abra o Visual Studio.
2. No menu **arquivo** , clique em **novo site**.

    ![criar novo site](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. Na caixa de diálogo **novo site da Web** , selecione o idioma a ser usado C# (Visual ou Visual Basic).
4. Selecione o modelo **site da ASP.net (Razor)** .

    ![site do Razor](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. Clique em **OK**.

O novo projeto existe e é preenchido com algumas páginas da Web padrão para ajudá-lo a começar.

### <a name="using-intellisense"></a>Usando o IntelliSense

Agora que você criou um site, pode ver como o IntelliSense funciona no Visual Studio.

1. No site que você acabou de criar, abra a página *Default. cshtml* .
2. Após as marcas de `<h3>` na página, digite `@ServerInfo.` (incluindo o ponto). Observe como o IntelliSense exibe os métodos disponíveis para o auxiliar de `ServerInfo` em uma lista suspensa.

    ![IntelliSense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. Selecione o método `GetHtml` na lista e pressione Enter. O IntelliSense preenche automaticamente o método. (Assim como ocorre com qualquer C#método no, você deve adicionar `()` caracteres após o método.) O código completo para o método `GetHtml` é semelhante ao exemplo a seguir:

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. Pressione CTRL + F5 para executar a página. Essa é a aparência da página quando exibida em um navegador:

    ![página padrão no navegador](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. Feche o navegador.

### <a name="using-the-debugger"></a>Usando o depurador

1. Na parte superior da página *Default. cshtml* , após a linha que começa com `Page.Title`, adicione a seguinte linha de código:

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. Na margem cinza do editor à esquerda do código, clique em avançar para essa nova linha para adicionar um *ponto de interrupção*. Um ponto de interrupção é um marcador que informa ao depurador para interromper a execução do programa nesse ponto, para que você possa ver o que está acontecendo.

    ![Definir ponto de interrupção](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. Remova a chamada para o método `ServerInfo.GetHtml` e adicione uma chamada para a variável `@myTime` em seu lugar. Essa chamada exibe o valor de hora atual que é retornado pela nova linha de código.
4. Pressione F5 para executar a página no depurador. A página para o ponto de interrupção que você definiu. A imagem a seguir mostra a aparência da página no editor com o ponto de interrupção (em amarelo).

    ![Depurar ponto de interrupção](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. Na barra de ferramentas depurar, clique no botão **Step Into** (ou pressione F11) para executar a próxima linha de código. Cada vez que você clica nesse botão, avança a execução para a próxima linha de código.

    ![Botão entrar](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. Examine o valor da variável `myTime` segurando o ponteiro do mouse sobre ela ou inspecionando os valores exibidos nas janelas **locais** e pilha de **chamadas** . O Visual Studio exibe o valor da variável.

    ![Mostrar valor de tempo](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. Quando terminar de examinar a variável e percorrer o código, pressione F5 para continuar a execução da página sem parar em cada linha. Quando você terminar de percorrer todo o código, o navegador exibirá a página.

Para saber mais sobre o depurador e sobre como depurar código no Visual Studio, consulte [passo a passos: Depurando páginas da Web no Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a>Usando o Razor em projetos do ASP.NET MVC com o Visual Studio

O sintaxe Razor também é usado extensivamente em projetos do ASP.NET MVC. O MVC é uma maneira poderosa e baseada em padrões para criar sites dinâmicos. Se seu site de Páginas da Web do ASP.NET se tornar difícil de manter, convém considerar a conversão para um aplicativo MVC ASP.NET. Para obter um exemplo de como criar um aplicativo MVC, consulte [introdução com o ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a>Instalando o suporte para Páginas da Web do ASP.NET no Visual Studio 2010

Esta seção mostra como instalar o Visual Web Developer Express 2010 e as ferramentas de Páginas da Web do ASP.NET para o Visual Studio.

1. Se você ainda não tiver o Web Platform Installer, baixe-o da seguinte URL:

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. Execute o Web Platform Installer.
3. Clique na guia **produtos** .

    ![Guia produtos do WebPI](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. Pesquise **ASP.NET MVC 4** (para páginas da Web do ASP.NET 2) e clique em **Adicionar**. Esses produtos incluem as ferramentas do Visual Studio para criar sites do ASP.NET Razor.

    ![Opções de instalação do WebPi](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. Clique em **instalar** para concluir a instalação.
