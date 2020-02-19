---
uid: webhooks/diagnostics/logging
title: Registro em log de WebHooks do ASP.NET | Microsoft Docs
author: rick-anderson
description: Como fazer o registro em log em WebHooks do ASP.NET.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: f71bc442-5f80-481b-a32c-a0ec18dee9d6
ms.openlocfilehash: a05b32c4a8f9577bcf6170bd19a9e440b1aeb75b
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457577"
---
# <a name="aspnet-webhooks-logging"></a>Registro em log de WebHooks do ASP.NET

Microsoft ASP.NET WebHooks usa o registro em log como uma maneira de relatar problemas e problemas. Por padrão, os logs são gravados usando [System. Diagnostics. Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) , onde podem ser gerenciados usando [ouvintes de rastreamento](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx) como qualquer outro fluxo de log.

Ao implantar seu aplicativo Web como um aplicativo Web do Azure, os logs são automaticamente selecionados e podem ser gerenciados junto com qualquer outro log [System. Diagnostics. Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) . Para obter detalhes, consulte [habilitar o log de diagnóstico para aplicativos Web no serviço Azure app](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)

Além disso, os logs podem ser obtidos diretamente de dentro do Visual Studio, conforme descrito em [solucionar problemas de um aplicativo Web no serviço de Azure App usando o Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="redirecting-logs"></a>Redirecionando logs

Em vez de gravar logs no [System. Diagnostics. Trace](https://msdn.microsoft.com/library/system.diagnostics.trace), é possível fornecer uma implementação de log alternativo que pode fazer logon diretamente em um Gerenciador de log, como [log4net](http://logging.apache.org/log4net/) e [NLog](http://nlog-project.org/). Basta fornecer uma implementação de [ILogger](https://github.com/aspnet/AspNetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) e registrá-lo com um mecanismo de injeção de dependência de sua escolha e ele será obtido por Microsoft ASP.net WebHooks. Consulte [injeção de dependência no ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) para obter detalhes.
