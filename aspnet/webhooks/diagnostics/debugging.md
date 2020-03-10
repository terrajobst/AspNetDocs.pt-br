---
uid: webhooks/diagnostics/debugging
title: Depuração de WebHooks do ASP.NET | Microsoft Docs
author: rick-anderson
description: Como depurar WebHooks do ASP.NET.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.openlocfilehash: 517d282fc22703b5861b748aea51023fa0a12a26
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78640864"
---
# <a name="aspnet-webhooks-debugging"></a>Depuração de WebHooks do ASP.NET  

## <a name="debugging-in-azure"></a>Depuração no Azure

Para depurar seu aplicativo Web durante a execução no Azure, consulte o tutorial [solucionar problemas de um aplicativo Web no serviço de Azure App usando o Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="debugging-with-source-and-symbols"></a>Depuração com fonte e símbolos

Além de depurar seu próprio código, é possível depurá-lo diretamente em Microsoft ASP.NET WebHooks e, na verdade, em todo o .NET. Isso funciona independentemente de você Depurar local ou remotamente. Primeiro, configure o Visual Studio para localizar a origem e os símbolos indo para **debug** e, em seguida, **Opções e configurações**. Defina as opções como esta:

![Opções e configurações](_static/SourceSymbols.png)

Em seguida, adicione um link para [symbolsource.org](http://symbolsource.org) para baixar a origem e os símbolos. Vá para a guia **símbolos** do menu acima e adicione o seguinte como um local de símbolo:

```
http://srv.symbolsource.org/pdb/Public
```

Além disso, certifique-se de que o diretório de cache tem um nome curto; caso contrário, os nomes de arquivo podem ficar muito longos, o que fará com que os símbolos não sejam carregados. Um caminho de exemplo é:

```
C:\SymCache
```

As configurações devem ser semelhantes a esta:

![Exemplo de local do arquivo de símbolos de opções](_static/SymSource.png)
