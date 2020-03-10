---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: Introdução ao MVC do ASP.NET | Microsoft Docs
author: shanselman
description: Este é um tutorial principiante que apresenta os fundamentos do ASP.NET MVC. Crie um aplicativo Web simples que lê e grava de um banco de dados.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: f8f0014776ba1313119e8c39c63a216b0fc864e7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78581581"
---
# <a name="intro-to-aspnet-mvc"></a>Introdução ao ASP.NET MVC

por [Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > Uma versão atualizada se este tutorial estiver disponível [aqui](../../getting-started/introduction/getting-started.md) usando [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013). O novo tutorial usa o ASP.NET MVC 5, que fornece vários aprimoramentos sobre este tutorial.
>
>
> Este é um tutorial principiante que apresenta os fundamentos do ASP.NET MVC. Você criará um aplicativo Web simples que lê e grava de um banco de dados. Visite o [ASP.NET MVC Learning Center](../../../index.md) para encontrar outros tutoriais e exemplos do ASP.NET MVC.

Vamos criar nosso primeiro aplicativo Web ASP.NET MVC usando o [Visual Web developer 2010 Express](https://www.microsoft.com/express/Web/). Vamos fazer um pequeno aplicativo de lista de filmes que nos permitirá criar e listar filmes.

## <a name="what-youll-build"></a>O que você vai construir

Aqui estão duas capturas de tela do aplicativo que você criará. Você terá uma tabela simples de filmes com várias colunas.

[Lista de filmes ![-Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)

E você terá um formulário criar para que possamos adicionar filmes à lista.

[![criar um filme-Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)

## <a name="skills-youll-learn"></a>Qualificações que você aprenderá

Este tutorial ensinará as noções básicas da criação de um aplicativo Web ASP.NET MVC usando o Visual Studio. O que você aprenderá:

- Como criar um novo projeto MVC do ASP.NET
- Como criar um novo banco de dados com SQL Server
- Como criar controladores e exibições do ASP.NET MVC
- Como recuperar e exibir dados
- Como editar dados e habilitar a validação de dados
- Como atualizar o esquema de banco de dados

## <a name="get-started"></a>Introdução

Comece executando o Visual Web Developer 2010 Express (vou chamá-lo de "VWD" de agora em diante) e selecione novo projeto na tela inicial.

O Visual Web Developer é um IDE ou um ambiente de desenvolvedor integrado. Assim como você usa o Microsoft Word para escrever documentos, você usará um IDE para criar aplicativos. Há uma barra de ferramentas na parte superior mostrando várias opções disponíveis para você, bem como o menu que você também pode ter usado para selecionar arquivo | Novo projeto.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)

## <a name="creating-your-first-application"></a>Criando seu primeiro aplicativo

Você pode criar aplicativos usando Visual Basic ou Visual C#. Por enquanto, selecione Visual C# à esquerda e, em seguida, selecione "aplicativo Web ASP.NET MVC 2". Nomeie o seu projeto como "filmes" e clique em OK.

[![novo projeto](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)

No lado direito está o Gerenciador de Soluções mostrar todos os arquivos e pastas em seu aplicativo. A grande janela no meio é onde você edita seu código e passa a maior parte do tempo. O Visual Studio usou um modelo padrão para o projeto MVC ASP.NET que você acabou de criar, portanto, você tem um aplicativo funcional agora sem fazer nada! Este é um simples "Olá, Mundo! projeto, e é um bom ponto de partida para o nosso aplicativo.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)

Selecione o botão "reproduzir" na barra de ferramentas.

![Iniciar Depuração](getting-started-with-mvc-part1/_static/image11.png)

É uma seta verde apontando para a direita que compilará seu programa e iniciará seu aplicativo em um navegador da Web.

*Observação: em vez disso, você pode pressionar F5 no teclado ou selecionar Debug-&gt;iniciar a depuração no menu "depurar".*

Isso fará com que o Visual Web Developer inicie um servidor Web de desenvolvimento e execute nosso aplicativo Web (não há nenhuma configuração ou etapa manual necessária para habilitá-lo). Em seguida, ele iniciará um navegador e o configurará para procurar a página inicial do aplicativo. Observe abaixo que a barra de endereços do navegador diz "localhost", e não algo como example.com. Isso ocorre porque o localhost sempre aponta para seu próprio computador local – que, nesse caso, está executando o aplicativo que acabamos de criar.

[Página inicial do ![](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)

Esse modelo padrão fornece duas páginas para você visitar e uma página de logon básica. Vamos alterar como esse aplicativo funciona e aprender um pouco sobre o ASP.NET MVC no processo. Feche o navegador e altere algum código.

> [!div class="step-by-step"]
> [Próximo](getting-started-with-mvc-part2.md)
