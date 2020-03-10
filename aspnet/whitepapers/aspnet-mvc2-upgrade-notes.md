---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: Atualizando um aplicativo ASP.NET MVC 1,0 para o ASP.NET MVC 2 | Microsoft Docs
author: rick-anderson
description: Este documento descreve como atualizar manualmente e com um assistente de um aplicativo ASP.NET MVC 1,0 para o ASP.NET MVC 2. Este documento também está disponível para d...
ms.author: riande
ms.date: 04/08/2010
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: 27589f1b1c9d5038118e5ff0cc2e7cecae17d5ed
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78637014"
---
# <a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a>Atualização de um aplicativo do ASP.NET MVC 1.0 para o ASP.NET MVC 2

> Este documento descreve como atualizar manualmente e com um assistente de um aplicativo ASP.NET MVC 1,0 para o ASP.NET MVC 2. Este documento também está disponível para [Download](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)

## <a name="introduction"></a>Introdução

O ASP.NET MVC 2 pode ser instalado lado a lado com o ASP.NET MVC 1,0 no mesmo servidor. Isso proporciona aos desenvolvedores de aplicativos flexibilidade na escolha de quando atualizar um aplicativo ASP.NET MVC 1,0 para o ASP.NET MVC 2.

O Visual Studio 2010 inclui um assistente que atualiza projetos existentes do ASP.NET MVC 1,0 criados com o Visual Studio 2008 para o ASP.NET MVC 2. O assistente de atualização é iniciado abrindo um projeto ASP.NET MVC 1,0 no Visual Studio 2010.

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a>Assistente de atualização para ASP.NET MVC 1,0 no Visual Studio 2008 SP1

Para atualizar um aplicativo ASP.NET MVC 1,0 para o ASP.NET MVC 2 no Visual Studio 2008 SP1, use o aplicativo MvcAppConverter (sem suporte). Você pode baixar este aplicativo da seguinte URL:

[https://go.microsoft.com/fwlink/?LinkID=185351](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a>Atualizando manualmente um projeto do ASP.NET MVC 1,0

Para atualizar manualmente um aplicativo existente do ASP.NET MVC 1,0 para a versão 2, siga estas etapas:

1. Faça um backup do projeto existente.
2. Em um editor de texto, abra o arquivo de projeto (o arquivo com a extensão de arquivo. csproj ou. vbproj) e localize o elemento ProjectTypeGuid. Como o valor desse elemento, substitua o GUID {603c0e0b-db56-11dc-be95-000d561079b0} por {F85E285D-A4E0-4152-9332-AB1D724D3325}. Quando terminar, o valor desse elemento deverá ser o seguinte: 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. Na pasta raiz do aplicativo Web, edite o arquivo Web. config. Pesquise System. Web. Mvc, Version = 1.0.0.0 e substitua todas as instâncias por System. Web. Mvc, Version = 2.0.0.0.
4. Repita a etapa anterior para o arquivo Web. config localizado na pasta exibições.
5. Abra o projeto usando o Visual Studio e, em **Gerenciador de soluções**, expanda o nó **referências** . Exclua a referência a System. Web. Mvc (que aponta para o assembly da versão 1,0). Adicione uma referência a System. Web. Mvc (v 2.0.0.0).
6. Adicione o seguinte elemento bindingRedirect ao arquivo Web. config na raiz do aplicativo na seção configuração:   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. Crie um novo aplicativo ASP.NET MVC 2 vazio. Copie os arquivos da pasta scripts do novo aplicativo na pasta scripts do aplicativo existente.
8. Atualize o arquivo CSS existente do aplicativos €™ s com as definições de estilo CSS no arquivo site. css.
9. Compile o aplicativo e execute-o. Se ocorrerem erros, consulte a seção alterações significativas da página [novidades na ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) .
