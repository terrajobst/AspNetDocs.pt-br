---
uid: web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-cs
title: Testando a força de uma senhaC#() | Microsoft Docs
author: wenz
description: As senhas são necessárias em qualquer lugar, de modo que os usuários lentos tendem a escolher senhas simples que sejam fáceis de quebrar. O controle PasswordStrength no ASP. N...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: cb4afbae-9b8f-483d-9729-476d4b9f85fc
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-cs
msc.type: authoredcontent
ms.openlocfilehash: e55eab9feebc18f39dd40c59cfb423208296b6c5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78627291"
---
# <a name="testing-the-strength-of-a-password-c"></a>Teste da força de uma senha (C#)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0CS.pdf)

> As senhas são necessárias em qualquer lugar, de modo que os usuários lentos tendem a escolher senhas simples que sejam fáceis de quebrar. O controle PasswordStrength no ASP.NET AJAX Control Toolkit pode verificar a boa senha.

## <a name="overview"></a>Visão geral

As senhas são necessárias em qualquer lugar, de modo que os usuários lentos tendem a escolher senhas simples que sejam fáceis de quebrar. O controle de `PasswordStrength` no ASP.NET AJAX Control Toolkit pode verificar a qualidade de uma senha.

## <a name="steps"></a>Etapas

O controle de `PasswordStrength` estende uma caixa de texto e verifica se a senha nela é boa o suficiente. Ele oferece uma infinidade de opções por meio de atributos; Aqui estão apenas algumas delas:

- `MinimumNumericCharacters` número mínimo de caracteres numéricos necessários na senha
- `MinimumSymbolCharacters` número mínimo de caracteres de símbolo (não letras e dígitos) necessários na senha
- `PreferredPasswordLength` o comprimento mínimo da senha
- `RequiresUpperAndLowerCaseCharacters` se a senha precisa usar caracteres maiúsculos e minúsculos

O `StrengthIndicatorType` fornece as informações sobre como apresentar a força da senha, como texto (valor `"Text"`) ou como um tipo de barra de progresso (valor `"BarIndicator"`). No atributo `DisplayPosition`, você configura onde as informações são exibidas. Aqui está um exemplo completo, incluindo o controle de `ScriptManager` AJAX ASP.NET, o controle de `PasswordStrength` e, obviamente, uma caixa de texto na qual o usuário pode inserir uma senha. Para fins de demonstração, o último campo de formulário é um campo de texto normal e não um campo de senha para que você possa ver durante o desenvolvimento o que está digitando.

[!code-aspx[Main](testing-the-strength-of-a-password-cs/samples/sample1.aspx)]

Execute a página e digite: somente depois de inserir letras minúsculas, letras maiúsculas, dígitos e símbolos, a senha será considerada inquebrável.

[![agora a senha é (bem) boa](testing-the-strength-of-a-password-cs/_static/image2.png)](testing-the-strength-of-a-password-cs/_static/image1.png)

Agora, a senha é (bem) boa ([clique para exibir a imagem em tamanho normal](testing-the-strength-of-a-password-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Próximo](testing-the-strength-of-a-password-vb.md)
