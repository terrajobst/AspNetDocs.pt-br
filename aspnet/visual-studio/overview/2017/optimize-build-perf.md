---
uid: visual-studio/overview/2017/optimize-build-perf
title: Otimizar o desempenho de build para a solução
author: AngelosP
description: Otimizar o desempenho de build para a solução
ms.author: riande
ms.date: 08/29/2018
msc.type: authoredcontent
ms.openlocfilehash: c1a5cf5e59374b4c0dd7150c5dd62fbde42af555
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57050503"
---
# <a name="optimize-build-performance-for-solution"></a>Otimizar o desempenho de build para a solução

15,8 de 2017 do Visual Studio ou posterior inclui um item de menu: **Crie** > **compilação ASP.NET** > **otimizar o desempenho de compilação para a solução**.

![captura de tela do novo item de menu](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

O ASP.NET compila seus modos de exibição em tempo de execução, o que significa que um projeto ASP.NET carrega uma cópia do compilador. No entanto em um computador de desenvolvedor quando a cópia do compilador não corresponde a cópia do Visual Studio, o desempenho de compilação é afetado na ordem de 1 a 3 segundos por compilação incremental. Esse recurso atualiza a cópia do seu projeto do compilador para coincidir com o Visual Studio, que geralmente acelera a compilações incrementais.

**Isso é aplicável ao ASP.NET Framework 4.7.1 ou posterior somente a projetos, não se aplica ao ASP.NET Core.**
