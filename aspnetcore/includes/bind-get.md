---
ms.openlocfilehash: 545448e3673b02abc7e685bd987f2cf5f71375b4
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57051953"
---
> [!WARNING]
> <span data-ttu-id="90a25-101">Por motivos de segurança, você deve aceitar associar os dados da solicitação `GET` às propriedades do modelo de página.</span><span class="sxs-lookup"><span data-stu-id="90a25-101">For security reasons, you must opt in to binding `GET` request data to page model properties.</span></span> <span data-ttu-id="90a25-102">Verifique a entrada do usuário antes de mapeá-la para as propriedades.</span><span class="sxs-lookup"><span data-stu-id="90a25-102">Verify user input before mapping it to properties.</span></span> <span data-ttu-id="90a25-103">Aceitar a associação de `GET` é útil ao lidar com cenários que contam com a cadeia de caracteres de consulta ou com os valores de rota.</span><span class="sxs-lookup"><span data-stu-id="90a25-103">Opting in to `GET` binding is useful when addressing scenarios which rely on query string or route values.</span></span>
>
> <span data-ttu-id="90a25-104">Para associar uma propriedade às solicitações `GET`, defina a propriedade `SupportsGet` do atributo [[BindProperty]](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) como `true`: `[BindProperty(SupportsGet = true)]`</span><span class="sxs-lookup"><span data-stu-id="90a25-104">To bind a property on `GET` requests, set the [[BindProperty]](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) attribute's `SupportsGet` property to `true`: `[BindProperty(SupportsGet = true)]`</span></span>
