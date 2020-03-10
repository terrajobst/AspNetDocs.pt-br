---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: Apresentando o tutorial do NerdDinner | Microsoft Docs
author: shanselman
description: A melhor maneira de aprender uma nova estrutura é criar algo com ela. Este tutorial explica como criar um aplicativo pequeno, mas completo usando o ASP.NE...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 154cfe6694cf723c0a1f8e33bfdb42c97594518f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78580573"
---
# <a name="introducing-the-nerddinner-tutorial"></a>Introdução ao Tutorial do NerdDinner

por [Scott Hanselman](https://github.com/shanselman)

[Baixar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> A melhor maneira de aprender uma nova estrutura é criar algo com ela. Este tutorial explica como criar um aplicativo pequeno, mas completo usando o ASP.NET MVC 1, e apresenta alguns dos conceitos básicos por trás dele.
> 
> Se você estiver usando o ASP.NET MVC 3, recomendamos seguir as [introdução com](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) os tutoriais da [loja de música](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) MVC 3 ou MVC.

## <a name="nerddinner-tutorial"></a>Tutorial do NerdDinner

A melhor maneira de aprender uma nova estrutura é criar algo com ela. Este tutorial explica como criar um aplicativo pequeno, mas completo usando o ASP.NET MVC, e apresenta alguns dos conceitos básicos por trás dele.

O aplicativo que vamos criar é chamado de "NerdDinner". O NerdDinner fornece uma maneira fácil para as pessoas encontrarem e organizarem os jantares online:

![](introducing-the-nerddinner-tutorial/_static/image1.png)

O NerdDinner permite que os usuários registrados criem, editem e excluam jantares. Ele impõe um conjunto consistente de regras de validação e de negócios em todo o aplicativo:

![](introducing-the-nerddinner-tutorial/_static/image2.png)

Os visitantes podem usar um mapa baseado em AJAX para pesquisar os próximos JANTARS sendo mantidos próximos deles:

![](introducing-the-nerddinner-tutorial/_static/image3.png)

Clicar em um jantar vai levá-los para uma página de detalhes em que eles podem saber mais sobre isso:

![](introducing-the-nerddinner-tutorial/_static/image4.png)

Se eles estiverem interessados em participar do jantar, eles poderão fazer logon ou se registrarem no site:

![](introducing-the-nerddinner-tutorial/_static/image5.png)

Em seguida, eles podem clicar em um link RSVP baseado em AJAX para participar do evento:

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a>Implementando NerdDinner

Vamos começar nosso aplicativo NerdDinner usando o comando File-&gt;New Project no Visual Studio para criar um projeto MVC totalmente novo ASP.NET. Em seguida, adicionaremos recursos e funcionalidades de forma incremental. Ao longo do caminho, abordaremos:

1. [Como criar um novo projeto MVC do ASP.NET](create-a-new-aspnet-mvc-project.md)
2. [Como criar um banco de dados](create-a-database.md)
3. [Como criar um modelo com validações de regra de negócio](build-a-model-with-business-rule-validations.md)
4. [Como usar controladores e exibições para implementar uma interface de usuário de listagem/detalhes](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
5. [Como fornecer suporte à entrada do formulário de dados CRUD (criar, ler, atualizar, excluir)](provide-crud-create-read-update-delete-data-form-entry-support.md)
6. [Como usar ViewData e implementar classes ViewModel](use-viewdata-and-implement-viewmodel-classes.md)
7. [Como reutilizar a interface do usuário usando páginas mestras e parciais](re-use-ui-using-master-pages-and-partials.md)
8. [Como implementar a paginação de dados eficiente](implement-efficient-data-paging.md)
9. [Como proteger aplicativos usando autenticação e autorização](secure-applications-using-authentication-and-authorization.md)
10. [Como usar o AJAX para fornecer atualizações dinâmicas](use-ajax-to-deliver-dynamic-updates.md)
11. [Como usar o AJAX para implementar cenários de mapeamento](use-ajax-to-implement-mapping-scenarios.md)
12. [Como habilitar o teste de unidade automatizado](enable-automated-unit-testing.md)

Você pode criar sua própria cópia do NerdDinner a partir do zero, concluindo cada etapa neste capítulo. Como alternativa, você pode baixar uma versão completa do código-fonte aqui: [NerdDinner no GitHub](https://github.com/AspNetMVPSamples/NerdDinner). Também é possível também [baixar uma versão de PDF gratuita deste tutorial](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) se você quiser ler o tutorial offline.

Você pode usar o Visual Studio 2008 ou o Visual Web Developer 2008 Express gratuito para compilar o aplicativo. Você pode usar SQL Server ou o SQL Server Express gratuito para o banco de dados.

Você pode instalar o ASP.NET MVC, o Visual Web Developer 2008 Express e o SQL Server Express (tudo gratuito) usando a v2 do [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)

### <a name="now-lets-get-started"></a>Agora, vamos começar...

Agora que já abordamos o que NerdDinner é, vamos acumular nossas mangas e escrever algum código.

Começaremos usando File-&gt;novo projeto no Visual Studio para criar o aplicativo NerdDinner.

> [!div class="step-by-step"]
> [Próximo](create-a-new-aspnet-mvc-project.md)
