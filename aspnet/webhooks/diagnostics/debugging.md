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
# <a name="aspnet-webhooks-debugging"></a><span data-ttu-id="1a84a-103">Depuração de WebHooks do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1a84a-103">ASP.NET WebHooks debugging</span></span>  

## <a name="debugging-in-azure"></a><span data-ttu-id="1a84a-104">Depuração no Azure</span><span class="sxs-lookup"><span data-stu-id="1a84a-104">Debugging in Azure</span></span>

<span data-ttu-id="1a84a-105">Para depurar seu aplicativo Web durante a execução no Azure, consulte o tutorial [solucionar problemas de um aplicativo Web no serviço de Azure App usando o Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span><span class="sxs-lookup"><span data-stu-id="1a84a-105">To debug your Web Application while running in Azure, please see the tutorial [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="debugging-with-source-and-symbols"></a><span data-ttu-id="1a84a-106">Depuração com fonte e símbolos</span><span class="sxs-lookup"><span data-stu-id="1a84a-106">Debugging with Source and Symbols</span></span>

<span data-ttu-id="1a84a-107">Além de depurar seu próprio código, é possível depurá-lo diretamente em Microsoft ASP.NET WebHooks e, na verdade, em todo o .NET.</span><span class="sxs-lookup"><span data-stu-id="1a84a-107">In addition to debugging your own code, it is possible to debug directly into Microsoft ASP.NET WebHooks, and in fact all of .NET.</span></span> <span data-ttu-id="1a84a-108">Isso funciona independentemente de você Depurar local ou remotamente.</span><span class="sxs-lookup"><span data-stu-id="1a84a-108">This works regardless of whether you debug locally or remotely.</span></span> <span data-ttu-id="1a84a-109">Primeiro, configure o Visual Studio para localizar a origem e os símbolos indo para **debug** e, em seguida, **Opções e configurações**.</span><span class="sxs-lookup"><span data-stu-id="1a84a-109">First, configure Visual Studio to find the source and symbols by going to **Debug** and then **Options and Settings**.</span></span> <span data-ttu-id="1a84a-110">Defina as opções como esta:</span><span class="sxs-lookup"><span data-stu-id="1a84a-110">Set the options like this:</span></span>

![Opções e configurações](_static/SourceSymbols.png)

<span data-ttu-id="1a84a-112">Em seguida, adicione um link para [symbolsource.org](http://symbolsource.org) para baixar a origem e os símbolos.</span><span class="sxs-lookup"><span data-stu-id="1a84a-112">Then add a link to [symbolsource.org](http://symbolsource.org) for downloading the source and symbols.</span></span> <span data-ttu-id="1a84a-113">Vá para a guia **símbolos** do menu acima e adicione o seguinte como um local de símbolo:</span><span class="sxs-lookup"><span data-stu-id="1a84a-113">Go to the **Symbols** tab of the menu above and add the following as a symbol location:</span></span>

```
http://srv.symbolsource.org/pdb/Public
```

<span data-ttu-id="1a84a-114">Além disso, certifique-se de que o diretório de cache tem um nome curto; caso contrário, os nomes de arquivo podem ficar muito longos, o que fará com que os símbolos não sejam carregados.</span><span class="sxs-lookup"><span data-stu-id="1a84a-114">In addition, make sure that the cache directory has a short name; otherwise the file names can get too long which will cause the symbols to not load.</span></span> <span data-ttu-id="1a84a-115">Um caminho de exemplo é:</span><span class="sxs-lookup"><span data-stu-id="1a84a-115">A sample path is:</span></span>

```
C:\SymCache
```

<span data-ttu-id="1a84a-116">As configurações devem ser semelhantes a esta:</span><span class="sxs-lookup"><span data-stu-id="1a84a-116">The settings should look similar to this:</span></span>

![Exemplo de local do arquivo de símbolos de opções](_static/SymSource.png)
