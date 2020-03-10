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
# <a name="optimize-build-performance-for-solution"></a>Otimizar o desempenho de build para a solução

O Visual Studio 2017 15,8 ou posterior inclui um item de menu: **compilar** > **compilação de ASP.net** > **otimizar o desempenho da compilação para solução**.

![Captura de tela do novo item de menu](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

O ASP.NET compila suas exibições em tempo de execução, o que significa que um projeto ASP.NET transporta uma cópia do compilador. No entanto, em um computador de desenvolvedor quando a cópia do compilador não corresponde à cópia do Visual Studio, o desempenho da compilação é afetado na ordem de 1-3 segundos por compilação incremental. Esse recurso atualiza a cópia do compilador do projeto para corresponder ao Visual Studio, que geralmente acelera as compilações incrementais.

**Isso se aplica somente ao ASP.NET Framework 4.7.1 ou projetos posteriores; ele não se aplica a ASP.NET Core.**
