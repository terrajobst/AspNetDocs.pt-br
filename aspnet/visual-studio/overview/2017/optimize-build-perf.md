---
uid: visual-studio/overview/2017/optimize-build-perf
title: Otimizar o desempenho de build para a solução
author: AngelosP
description: Otimizar o desempenho de build para a solução
ms.author: riande
ms.date: 08/29/2018
msc.type: authoredcontent
ms.openlocfilehash: c1a5cf5e59374b4c0dd7150c5dd62fbde42af555
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622636"
---
# <a name="optimize-build-performance-for-solution"></a><span data-ttu-id="5f860-103">Otimizar o desempenho de build para a solução</span><span class="sxs-lookup"><span data-stu-id="5f860-103">Optimize build performance for solution</span></span>

<span data-ttu-id="5f860-104">O Visual Studio 2017 15,8 ou posterior inclui um item de menu: **compilar** > **compilação de ASP.net** > **otimizar o desempenho da compilação para solução**.</span><span class="sxs-lookup"><span data-stu-id="5f860-104">Visual Studio 2017 15.8 or later include a menu item: **Build** > **ASP.NET Compilation** > **Optimize Build Performance for Solution**.</span></span>

![Captura de tela do novo item de menu](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

<span data-ttu-id="5f860-106">O ASP.NET compila suas exibições em tempo de execução, o que significa que um projeto ASP.NET transporta uma cópia do compilador.</span><span class="sxs-lookup"><span data-stu-id="5f860-106">ASP.NET compiles its views at runtime, which means that an ASP.NET project carries with it a copy of the compiler.</span></span> <span data-ttu-id="5f860-107">No entanto, em um computador de desenvolvedor quando a cópia do compilador não corresponde à cópia do Visual Studio, o desempenho da compilação é afetado na ordem de 1-3 segundos por compilação incremental.</span><span class="sxs-lookup"><span data-stu-id="5f860-107">However on a developer machine when the copy of the compiler doesn't match Visual Studio's copy, build performance is impacted on the order of 1-3 seconds per incremental build.</span></span> <span data-ttu-id="5f860-108">Esse recurso atualiza a cópia do compilador do projeto para corresponder ao Visual Studio, que geralmente acelera as compilações incrementais.</span><span class="sxs-lookup"><span data-stu-id="5f860-108">This feature updates your project's copy of the compiler to match Visual Studio's, which usually speeds up incremental builds.</span></span>

<span data-ttu-id="5f860-109">**Isso se aplica somente ao ASP.NET Framework 4.7.1 ou projetos posteriores; ele não se aplica a ASP.NET Core.**</span><span class="sxs-lookup"><span data-stu-id="5f860-109">**This is applicable to ASP.NET Framework 4.7.1 or later projects only, it does not apply to ASP.NET Core.**</span></span>
