---
uid: mvc/overview/getting-started/mvc-learning-sequence
title: Tutoriais e artigos recomendados do MVC | Microsoft Docs
author: Rick-Anderson
description: Esta página contém links para os tutoriais do ASP.NET MVC e uma sequência sugerida para segui-los.
ms.author: riande
ms.date: 05/22/2015
ms.assetid: 8513a57a-2d45-4d6b-881c-15a01c5cbb1c
msc.legacyurl: /mvc/overview/getting-started/mvc-learning-sequence
msc.type: authoredcontent
ms.openlocfilehash: 46b089a896c6b1b92437ff1f5488214a3a0a9838
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78602511"
---
# <a name="mvc-recommended-tutorials-and-articles"></a>Artigos e tutoriais de MVC recomendados

por [Rick Anderson](https://twitter.com/RickAndMSFT)

<a id="pwd"></a>
## <a name="getting-started"></a>Introdução

- [Introdução com o ASP.NET MVC 5](introduction/getting-started.md) Esta série de 11 partes é um bom ponto de partida.
- [Fundamentos da Pluralsight ASP.NET MVC 5](https://pluralsight.com/training/Player?author=scott-allen&amp;name=aspdotnet-mvc5-fundamentals-m1-introduction&amp;mode=live&amp;clip=0&amp;course=aspdotnet-mvc5-fundamentals) (curso em vídeo)
- [Introdução ao ASP.NET MVC](https://channel9.msdn.com/Series/Introduction-to-ASP-NET-MVC) por Jon Galloway e Christopher Harrison
- [Ciclo de vida de um aplicativo ASP.NET MVC 5](lifecycle-of-an-aspnet-mvc-5-application.md) Documento PDF que gráficou o ciclo de vida de um aplicativo ASP.NET MVC 5.

<a id="con"></a>
## <a name="working-with-data"></a>Trabalhando com os dados

- [Introdução com o EF 6 Code First usando o MVC 5](getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) A série vencedora de Tom Dykstra se aprofunda no EF.

<a id="wj"></a>
## <a name="security"></a>Segurança

- [Criar um aplicativo MVC do ASP.NET com autenticação e banco de BD SQL e implantar no Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) Este tutorial popular orienta você pela criação de um aplicativo simples e pela adição de associações e funções.
- [Criar um aplicativo ASP.NET MVC 5 com o logon do Facebook, Twitter, LinkedIn e Google OAuth2](../security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) Este tutorial mostra como criar um aplicativo Web ASP.NET MVC 5 que permite que os usuários façam logon usando o OAuth 2,0 com credenciais de um provedor de autenticação externo, como Facebook, Twitter, LinkedIn, Microsoft ou Google.
- [Criar um aplicativo Web secure ASP.NET MVC 5 com logon, confirmação de email e redefinição de senha](../security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) Primeiro, em uma série sobre identidade, inclui código para [reenviar um link de confirmação](../security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md#rsend).
- [Aplicativo ASP.NET MVC 5 com SMS e email autenticação de dois fatores](../security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md) Segundo na série de identidades.
- [Melhores práticas para implantar senhas e outros dados confidenciais no ASP.NET e no Serviço de Aplicativo do Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)
- [Autenticação de dois fatores usando SMS e email com ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) `isPersistent` e o cookie de segurança, o código para exigir que um usuário tenha uma conta de email validada antes que possa fazer logon, como o SignInManager verifica o requisito de 2FA e muito mais.
- [Confirmação de conta e recuperação de senha com ASP.net Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Fornece detalhes sobre a identidade não encontrada em [criar um aplicativo Web secure ASP.NET MVC 5 com logon, confirmação de email e redefinição de senha](../security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) , como permitir que os usuários redefinam sua senha esquecida.

<a id="da"></a>
## <a name="azure"></a>Azure

- [Criar um aplicativo web ASP.net no Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/) Tutorial breve e simples para implantação no Azure.
- [Criar um aplicativo MVC do ASP.NET com autenticação e banco de BD SQL e implantar no Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)

<a id="perf"></a>
## <a name="performance-and-debugging"></a>Desempenho e depuração

- [Analisar e depurar seu aplicativo do ASP.NET MVC com Glimpse](../performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse.md)
