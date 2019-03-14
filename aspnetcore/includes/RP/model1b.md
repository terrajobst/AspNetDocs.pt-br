---
ms.openlocfilehash: 025a244812a50709bfd48de9f47a234a547c53e2
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57047413"
---
<span data-ttu-id="0b3b2-101"><!-- THIS INCLUDE USED BY MVC AND RP --> Adicione as seguintes propriedades à classe `Movie`:</span><span class="sxs-lookup"><span data-stu-id="0b3b2-101"><!-- THIS INCLUDE USED BY MVC AND RP --> Add the following properties to the `Movie` class:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/Movie.cs?name=snippet1)]

<span data-ttu-id="0b3b2-102">A classe `Movie` contém:</span><span class="sxs-lookup"><span data-stu-id="0b3b2-102">The `Movie` class contains:</span></span>

* <span data-ttu-id="0b3b2-103">O campo `ID` é necessário para o banco de dados para a chave primária.</span><span class="sxs-lookup"><span data-stu-id="0b3b2-103">The `ID` field is required by the database for the primary key.</span></span>
* <span data-ttu-id="0b3b2-104">`[DataType(DataType.Date)]`:  O atributo [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) especifica o tipo de dados (Data).</span><span class="sxs-lookup"><span data-stu-id="0b3b2-104">`[DataType(DataType.Date)]`:  The [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute specifies the type of the data (Date).</span></span> <span data-ttu-id="0b3b2-105">Com esse atributo:</span><span class="sxs-lookup"><span data-stu-id="0b3b2-105">With this attribute:</span></span>

  * <span data-ttu-id="0b3b2-106">O usuário não precisa inserir informações de tempo no campo de data.</span><span class="sxs-lookup"><span data-stu-id="0b3b2-106">The user is not required to enter time information in the date field.</span></span>
  * <span data-ttu-id="0b3b2-107">Somente a data é exibida, não as informações de tempo.</span><span class="sxs-lookup"><span data-stu-id="0b3b2-107">Only the date is displayed, not time information.</span></span>

<span data-ttu-id="0b3b2-108">[DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) são abordados em um tutorial posterior.</span><span class="sxs-lookup"><span data-stu-id="0b3b2-108">[DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) are covered in a later tutorial.</span></span>