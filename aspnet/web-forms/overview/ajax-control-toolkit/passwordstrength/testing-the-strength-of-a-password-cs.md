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
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598836"
---
# <a name="testing-the-strength-of-a-password-c"></a><span data-ttu-id="95eab-104">Teste da força de uma senha (C#)</span><span class="sxs-lookup"><span data-stu-id="95eab-104">Testing the Strength of a Password (C#)</span></span>

<span data-ttu-id="95eab-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="95eab-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="95eab-106">[Baixar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="95eab-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.cs.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0CS.pdf)</span></span>

> <span data-ttu-id="95eab-107">As senhas são necessárias em qualquer lugar, de modo que os usuários lentos tendem a escolher senhas simples que sejam fáceis de quebrar.</span><span class="sxs-lookup"><span data-stu-id="95eab-107">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="95eab-108">O controle PasswordStrength no ASP.NET AJAX Control Toolkit pode verificar a boa senha.</span><span class="sxs-lookup"><span data-stu-id="95eab-108">The PasswordStrength control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>

## <a name="overview"></a><span data-ttu-id="95eab-109">{1&gt;Visão Geral&lt;1}</span><span class="sxs-lookup"><span data-stu-id="95eab-109">Overview</span></span>

<span data-ttu-id="95eab-110">As senhas são necessárias em qualquer lugar, de modo que os usuários lentos tendem a escolher senhas simples que sejam fáceis de quebrar.</span><span class="sxs-lookup"><span data-stu-id="95eab-110">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="95eab-111">O controle de `PasswordStrength` no ASP.NET AJAX Control Toolkit pode verificar a qualidade de uma senha.</span><span class="sxs-lookup"><span data-stu-id="95eab-111">The `PasswordStrength` control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>

## <a name="steps"></a><span data-ttu-id="95eab-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="95eab-112">Steps</span></span>

<span data-ttu-id="95eab-113">O controle de `PasswordStrength` estende uma caixa de texto e verifica se a senha nela é boa o suficiente.</span><span class="sxs-lookup"><span data-stu-id="95eab-113">The `PasswordStrength` control extends a text box and checks whether the password in it is good enough.</span></span> <span data-ttu-id="95eab-114">Ele oferece uma infinidade de opções por meio de atributos; Aqui estão apenas algumas delas:</span><span class="sxs-lookup"><span data-stu-id="95eab-114">It offers a wealth of options via attributes; here are just some of them:</span></span>

- <span data-ttu-id="95eab-115">`MinimumNumericCharacters` número mínimo de caracteres numéricos necessários na senha</span><span class="sxs-lookup"><span data-stu-id="95eab-115">`MinimumNumericCharacters` minimum number of numeric characters required in the password</span></span>
- <span data-ttu-id="95eab-116">`MinimumSymbolCharacters` número mínimo de caracteres de símbolo (não letras e dígitos) necessários na senha</span><span class="sxs-lookup"><span data-stu-id="95eab-116">`MinimumSymbolCharacters` minimum number of symbol characters (not letters and digits) required in the password</span></span>
- <span data-ttu-id="95eab-117">`PreferredPasswordLength` o comprimento mínimo da senha</span><span class="sxs-lookup"><span data-stu-id="95eab-117">`PreferredPasswordLength` minimum length of the password</span></span>
- <span data-ttu-id="95eab-118">`RequiresUpperAndLowerCaseCharacters` se a senha precisa usar caracteres maiúsculos e minúsculos</span><span class="sxs-lookup"><span data-stu-id="95eab-118">`RequiresUpperAndLowerCaseCharacters` whether the password needs to use both uppercase and lowercase characters</span></span>

<span data-ttu-id="95eab-119">O `StrengthIndicatorType` fornece as informações sobre como apresentar a força da senha, como texto (valor `"Text"`) ou como um tipo de barra de progresso (valor `"BarIndicator"`).</span><span class="sxs-lookup"><span data-stu-id="95eab-119">The `StrengthIndicatorType` provides the information how to present the strength of the password, as text (value `"Text"`) or as a kind of progress bar (value `"BarIndicator"`).</span></span> <span data-ttu-id="95eab-120">No atributo `DisplayPosition`, você configura onde as informações são exibidas.</span><span class="sxs-lookup"><span data-stu-id="95eab-120">In the `DisplayPosition` attribute, you configure where the information appears.</span></span> <span data-ttu-id="95eab-121">Aqui está um exemplo completo, incluindo o controle de `ScriptManager` AJAX ASP.NET, o controle de `PasswordStrength` e, obviamente, uma caixa de texto na qual o usuário pode inserir uma senha.</span><span class="sxs-lookup"><span data-stu-id="95eab-121">Here is a complete example, including the ASP.NET AJAX `ScriptManager` control, the `PasswordStrength` control and of course a text box where the user may enter a password.</span></span> <span data-ttu-id="95eab-122">Para fins de demonstração, o último campo de formulário é um campo de texto normal e não um campo de senha para que você possa ver durante o desenvolvimento o que está digitando.</span><span class="sxs-lookup"><span data-stu-id="95eab-122">For the sake of demonstration, the latter form field is a regular text field and not a password field so that you can see during development what you are typing.</span></span>

[!code-aspx[Main](testing-the-strength-of-a-password-cs/samples/sample1.aspx)]

<span data-ttu-id="95eab-123">Execute a página e digite: somente depois de inserir letras minúsculas, letras maiúsculas, dígitos e símbolos, a senha será considerada inquebrável.</span><span class="sxs-lookup"><span data-stu-id="95eab-123">Run the page and type away: Only after you have entered lowercase letters, uppercase letters, digits and symbols, the password is deemed as unbreakable .</span></span>

<span data-ttu-id="95eab-124">[![agora a senha é (bem) boa](testing-the-strength-of-a-password-cs/_static/image2.png)](testing-the-strength-of-a-password-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="95eab-124">[![Now the password is (quite) good](testing-the-strength-of-a-password-cs/_static/image2.png)](testing-the-strength-of-a-password-cs/_static/image1.png)</span></span>

<span data-ttu-id="95eab-125">Agora, a senha é (bem) boa ([clique para exibir a imagem em tamanho normal](testing-the-strength-of-a-password-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="95eab-125">Now the password is (quite) good ([Click to view full-size image](testing-the-strength-of-a-password-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="95eab-126">Avançar</span><span class="sxs-lookup"><span data-stu-id="95eab-126">Next</span></span>](testing-the-strength-of-a-password-vb.md)
