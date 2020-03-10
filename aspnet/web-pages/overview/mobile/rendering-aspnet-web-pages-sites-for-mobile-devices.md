---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: Renderizando Páginas da Web do ASP.NET (Razor) sites para dispositivos móveis | Microsoft Docs
author: Rick-Anderson
description: 'Este artigo descreve como criar páginas em um site Páginas da Web do ASP.NET (Razor) que será renderizado adequadamente em dispositivos móveis. O que você aprenderá: como fazer isso...'
ms.author: riande
ms.date: 02/17/2014
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: c012348d65e48a275cb0e4808fef2a7f31e5fb33
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78563563"
---
# <a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a>Renderizando Páginas da Web do ASP.NET (Razor) sites para dispositivos móveis

por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo descreve como criar páginas em um site Páginas da Web do ASP.NET (Razor) que será renderizado adequadamente em dispositivos móveis.
> 
> O que você aprenderá:
> 
> - Como usar uma Convenção de nomenclatura para especificar que uma página seja projetada especificamente para dispositivos móveis.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 3
>   
> 
> Este tutorial também funciona com o Páginas da Web do ASP.NET 2.

Páginas da Web do ASP.NET permite que você crie exibições personalizadas para o processamento de conteúdo em dispositivos móveis ou outros.

A maneira mais simples de criar uma página específica do dispositivo em um Páginas da Web do ASP.NET site é usando um padrão de nomeação de arquivo como este: *filename. Mobile. cshtml*. Você pode criar duas versões de uma página (por exemplo, uma chamada *MyFile. cshtml* e outra chamada *MyFile. Mobile. cshtml*). Em tempo de execução, quando um dispositivo móvel solicita *MyFile. cshtml*, ASP.net renderiza o conteúdo de *MyFile. Mobile. cshtml*. Caso contrário, *MyFile. cshtml* será renderizado.

O exemplo a seguir mostra como habilitar a renderização móvel adicionando uma página de conteúdo para dispositivos móveis. *Página1. cshtml* contém conteúdo mais uma barra lateral de navegação. *Página1. Mobile. cshtml* contém o mesmo conteúdo, mas omite a barra lateral.

1. Em um Páginas da Web do ASP.NET site, crie um arquivo chamado *página1. cshtml* e substitua o conteúdo atual pela marcação a seguir.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. Crie um arquivo chamado *página1. Mobile. cshtml* e substitua o conteúdo existente pela marcação a seguir. Observe que a versão móvel da página omite a seção de navegação para uma melhor renderização em uma tela menor.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. Execute um navegador da área de trabalho e navegue até *página1. cshtml*. ![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)
4. Execute um navegador móvel (ou um emulador de dispositivo móvel) e navegue até *página1. cshtml*. (Observe que você não inclui o *. Mobile.* como parte da URL.) Embora a solicitação seja para *página1. cshtml*, ASP.net renderiza *página1. Mobile. cshtml*.

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> Para testar as páginas móveis, você pode usar um simulador de dispositivo móvel que é executado em um computador desktop. Essa ferramenta permite que você teste páginas da Web da mesma forma que elas examinarão dispositivos móveis (ou seja, normalmente com uma área de exibição muito menor). Um exemplo de simulador é o [complemento de seletor de agente do usuário](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) para Mozilla Firefox, que permite emular vários navegadores móveis de uma versão de área de trabalho do Firefox.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais

[Emulador de Windows Phone](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)
