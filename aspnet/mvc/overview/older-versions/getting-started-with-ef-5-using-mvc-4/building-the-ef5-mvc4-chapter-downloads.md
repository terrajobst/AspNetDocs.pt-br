---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
title: Criando o capítulo downloads para os tutoriais do EF 5 MVC 4 | Microsoft Docs
author: Rick-Anderson
description: O aplicativo Web de exemplo da Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Entity Framework 5 Code First e o Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: d0a89089-eed8-4f61-a478-c5ffa30186f5
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
msc.type: authoredcontent
ms.openlocfilehash: 10237af40e3914b65e5181f17555697e86adea4b
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457850"
---
# <a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a><span data-ttu-id="aac88-103">Criando o capítulo downloads para os tutoriais do EF 5 MVC 4</span><span class="sxs-lookup"><span data-stu-id="aac88-103">Building the Chapter Downloads for the EF 5 MVC 4 Tutorials</span></span>

<span data-ttu-id="aac88-104">por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="aac88-104">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[<span data-ttu-id="aac88-105">Baixar projeto concluído</span><span class="sxs-lookup"><span data-stu-id="aac88-105">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="aac88-106">O aplicativo Web de exemplo da Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Entity Framework 5 Code First e o Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="aac88-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="aac88-107">Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="aac88-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

## <a name="building-the-chapter-downloads"></a><span data-ttu-id="aac88-108">Criação dos downloads do capítulo</span><span class="sxs-lookup"><span data-stu-id="aac88-108">Building the Chapter Downloads</span></span>

1. <span data-ttu-id="aac88-109">Baixe e descompacte o arquivo zip de exemplo do projeto.</span><span class="sxs-lookup"><span data-stu-id="aac88-109">Download and unzip the  project sample zip file.</span></span> <span data-ttu-id="aac88-110">No pacote de download descompactado, você encontrará arquivos zip adicionais, um para a conclusão de cada capítulo.</span><span class="sxs-lookup"><span data-stu-id="aac88-110">In the unzipped download package, you will find additional zip files, one for the completion of each chapter.</span></span>
2. <span data-ttu-id="aac88-111">Clique com o botão direito do mouse no arquivo zip desejado, clique em **Propriedades**e clique no botão **desbloquear** .</span><span class="sxs-lookup"><span data-stu-id="aac88-111">Right click on the desired zip file, click **Properties**, and click the **Unblock** button.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. <span data-ttu-id="aac88-112">Descompacte o arquivo.</span><span class="sxs-lookup"><span data-stu-id="aac88-112">Unzip the file.</span></span>
4. <span data-ttu-id="aac88-113">Clique duas vezes no arquivo *CUx. sln* para iniciar o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="aac88-113">Double-click the *CUx.sln* file to launch Visual Studio.</span></span>
5. <span data-ttu-id="aac88-114">No menu **ferramentas** , clique em **Gerenciador de pacotes NuGet**e em **console do Gerenciador de pacotes**.</span><span class="sxs-lookup"><span data-stu-id="aac88-114">From the **Tools** menu, click **NuGet Package Manager**, then **Package Manager Console**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. <span data-ttu-id="aac88-115">No console do Gerenciador de pacotes (PMC), clique em **restaurar**.</span><span class="sxs-lookup"><span data-stu-id="aac88-115">In the Package Manager Console (PMC), click **Restore**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. <span data-ttu-id="aac88-116">Saia do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="aac88-116">Exit Visual Studio.</span></span>
8. <span data-ttu-id="aac88-117">Reinicie o Visual Studio, abrindo o arquivo da solução que você fechou na etapa anterior.</span><span class="sxs-lookup"><span data-stu-id="aac88-117">Restart Visual Studio, opening the solution file you closed in the step above.</span></span>
9. <span data-ttu-id="aac88-118">No console do Gerenciador de pacotes (PMC), insira o comando `Update-Database`:</span><span class="sxs-lookup"><span data-stu-id="aac88-118">In the Package Manager Console (PMC), enter the `Update-Database` command:</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > <span data-ttu-id="aac88-119">Se você receber o seguinte erro:</span><span class="sxs-lookup"><span data-stu-id="aac88-119">If you get the following error:</span></span>  
    >   
    >  <span data-ttu-id="aac88-120">*O termo ' Update-Database ' não é reconhecido como o nome de um cmdlet, função, arquivo de script ou programa operável. Verifique a ortografia do nome ou, se um caminho foi incluído, verifique se o caminho está correto e tente novamente.*</span><span class="sxs-lookup"><span data-stu-id="aac88-120">*The term 'Update-Database' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.*</span></span>  
    > <span data-ttu-id="aac88-121">Saia e reinicie o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="aac88-121">Exit and restart Visual Studio.</span></span>

    <span data-ttu-id="aac88-122">Cada migração será executada e o método de semente será executado.</span><span class="sxs-lookup"><span data-stu-id="aac88-122">Each migration will run, then the seed method will run.</span></span> <span data-ttu-id="aac88-123">Agora você pode executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="aac88-123">You can now run the app.</span></span>

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

> [!div class="step-by-step"]
> [<span data-ttu-id="aac88-124">Anterior</span><span class="sxs-lookup"><span data-stu-id="aac88-124">Previous</span></span>](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
