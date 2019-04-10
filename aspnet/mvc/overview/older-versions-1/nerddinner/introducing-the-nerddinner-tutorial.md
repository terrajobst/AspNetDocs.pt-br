---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: Introdução ao Tutorial do NerdDinner | Microsoft Docs
author: shanselman
description: É a melhor maneira de aprender uma nova estrutura criar algo com ele. Este tutorial explica como criar um aplicativo pequeno, mas completo usando ASP.NE...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: ebd49295ea165ba4ef1a25398cff7dddcfa54f11
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59392190"
---
# <a name="introducing-the-nerddinner-tutorial"></a>Introdução ao Tutorial do NerdDinner

por [Scott Hanselman](https://github.com/shanselman)

[Baixar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> É a melhor maneira de aprender uma nova estrutura criar algo com ele. Este tutorial explica como criar um pequeno, mas concluir, o aplicativo usando o ASP.NET MVC 1 e apresenta alguns dos principais conceitos por trás dele.
> 
> Se você estiver usando o ASP.NET MVC 3, recomendamos que você siga a [obtendo iniciado com o MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [Store de música do MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriais.


## <a name="nerddinner-tutorial"></a>Tutorial do NerdDinner

É a melhor maneira de aprender uma nova estrutura criar algo com ele. Este tutorial explica como criar um pequeno, mas concluir, o aplicativo usando o ASP.NET MVC e apresenta alguns dos principais conceitos por trás dele.

O aplicativo, vamos compilar é chamado "NerdDinner". NerdDinner fornece uma maneira fácil para as pessoas a localizar e organizar os jantares online:

![](introducing-the-nerddinner-tutorial/_static/image1.png)

NerdDinner permite que os usuários registrados para criar, editar e excluir os jantares. Impõe um conjunto consistente de validação e regras de negócios em todo o aplicativo:

![](introducing-the-nerddinner-tutorial/_static/image2.png)

Os visitantes podem usar um mapa de baseados em AJAX para procurar próximos jantares sendo mantidos próximo a eles:

![](introducing-the-nerddinner-tutorial/_static/image3.png)

Clicar em um jantar vai levá-los a uma página de detalhes em que eles possam aprender mais sobre ele:

![](introducing-the-nerddinner-tutorial/_static/image4.png)

Se ele está interessado em participar da dinner podem fazer logon ou registre-se no site:

![](introducing-the-nerddinner-tutorial/_static/image5.png)

Eles podem, em seguida, clique em um link de RSVP baseados em AJAX para participar do evento:

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a>A implementação do NerdDinner

Vamos começar nosso aplicativo NerdDinner, usando o arquivo -&gt;comando de novo projeto no Visual Studio para criar um novo projeto do ASP.NET MVC. Em seguida, incrementalmente adicionaremos recursos e funcionalidades. Ao longo do caminho que abordaremos:

1. [Como criar um novo projeto ASP.NET MVC](create-a-new-aspnet-mvc-project.md)
2. [Como criar um banco de dados](create-a-database.md)
3. [Como criar um modelo com validações de regra de negócios](build-a-model-with-business-rule-validations.md)
4. [Como usar controladores e exibições para implementar uma interface do usuário de listagem/detalhes](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
5. [Como fornecer CRUD (criar, ler, atualizar e excluir) dados de suporte de entrada de formulário](provide-crud-create-read-update-delete-data-form-entry-support.md)
6. [Como usar ViewData e implementar classes ViewModel](use-viewdata-and-implement-viewmodel-classes.md)
7. [Como usar novamente a interface do usuário usando páginas mestras e parciais](re-use-ui-using-master-pages-and-partials.md)
8. [Como implementar a paginação eficiente de dados](implement-efficient-data-paging.md)
9. [Como proteger aplicativos usando a autenticação e autorização](secure-applications-using-authentication-and-authorization.md)
10. [Como usar o AJAX para fornecer atualizações dinâmicas](use-ajax-to-deliver-dynamic-updates.md)
11. [Como usar o AJAX para implementar cenários de mapeamento](use-ajax-to-implement-mapping-scenarios.md)
12. [Como habilitar o teste de unidade automatizado](enable-automated-unit-testing.md)

Você pode criar sua própria cópia do NerdDinner do zero ao concluir cada etapa, passo a passo neste capítulo. Como alternativa, você pode baixar uma versão completa do código-fonte aqui: [NerdDinner no GitHub](https://github.com/AspNetMVPSamples/NerdDinner). Também é possível também opcionalmente [baixar uma versão gratuita do PDF deste tutorial](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) se você quiser ler o tutorial off-line.

Você pode usar o Visual Studio 2008 ou o gratuito Visual Web Developer 2008 Express para compilar o aplicativo. Você pode usar o SQL Server ou o Express do SQL Server gratuito para o banco de dados.

Você pode instalar o ASP.NET MVC, Visual Web Developer 2008 Express e SQL Server Express (gratuita) o usando V2 do [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)

### <a name="now-lets-get-started"></a>Agora vamos começar...

Agora que já abordamos o que é NerdDinner, vamos arregaçar nossas mangas e escrever algum código.

Vamos começar usando arquivo -&gt;novo projeto no Visual Studio para criar o aplicativo NerdDinner.

> [!div class="step-by-step"]
> [Avançar](create-a-new-aspnet-mvc-project.md)
