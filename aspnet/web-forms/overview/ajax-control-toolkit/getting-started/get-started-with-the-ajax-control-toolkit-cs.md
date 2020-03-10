---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
title: Introdução ao AJAX Control Toolkit (C#) | Microsoft Docs
author: microsoft
description: Saiba tudo o que você precisa saber para começar a usar o AJAX Control Toolkit.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 16dc5c11-65be-4eae-a818-9fad7f8259c6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
msc.type: authoredcontent
ms.openlocfilehash: 7478b090ec52778572d70065983de6be8bdb4e6b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78621600"
---
# <a name="get-started-with-the-ajax-control-toolkit-c"></a><span data-ttu-id="ff657-103">Introdução ao AJAX Control Toolkit (C#)</span><span class="sxs-lookup"><span data-stu-id="ff657-103">Get Started with the AJAX Control Toolkit (C#)</span></span>

<span data-ttu-id="ff657-104">pela [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="ff657-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="ff657-105">Saiba tudo o que você precisa saber para começar a usar o AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="ff657-105">Learn all you need to know to get started using the AJAX Control Toolkit.</span></span>

<span data-ttu-id="ff657-106">O AJAX Control Toolkit contém mais de 30 controles gratuitos que você pode usar em seus aplicativos ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ff657-106">The AJAX Control Toolkit contains more than 30 free controls that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="ff657-107">Neste tutorial, você aprenderá a baixar o AJAX Control Toolkit e adicionar os controles do kit de ferramentas à sua caixa de ferramentas do Visual Studio/Visual Web Developer Express.</span><span class="sxs-lookup"><span data-stu-id="ff657-107">In this tutorial, you learn how to download the AJAX Control Toolkit and add the toolkit controls to your Visual Studio/Visual Web Developer Express toolbox.</span></span>

## <a name="downloading-the-ajax-control-toolkit"></a><span data-ttu-id="ff657-108">Baixando o AJAX Control Toolkit</span><span class="sxs-lookup"><span data-stu-id="ff657-108">Downloading the AJAX Control Toolkit</span></span>

<span data-ttu-id="ff657-109">O [AJAX Control Toolkit](http://devexpress.com/act) é um projeto de software livre desenvolvido pelos membros da comunidade do ASP.net e da equipe do ASP.net.</span><span class="sxs-lookup"><span data-stu-id="ff657-109">The [AJAX Control Toolkit](http://devexpress.com/act) is an open source project developed by the members of the ASP.NET community and the ASP.NET team.</span></span> 

<span data-ttu-id="ff657-110">[![baixar o AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ff657-110">[![Downloading the AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)</span></span>

<span data-ttu-id="ff657-111">**Figura 01**: baixando o AJAX Control Toolkit ([clique para exibir a imagem em tamanho normal](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="ff657-111">**Figure 01**: Downloading the AJAX Control Toolkit([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))</span></span>

<span data-ttu-id="ff657-112">Depois de baixar o arquivo, você precisará desbloquear o arquivo.</span><span class="sxs-lookup"><span data-stu-id="ff657-112">After you download the file, you need to unblock the file.</span></span> <span data-ttu-id="ff657-113">Clique com o botão direito do mouse no arquivo, selecione Propriedades e clique no botão **desbloquear** (veja a Figura 2).</span><span class="sxs-lookup"><span data-stu-id="ff657-113">Right-click the file, select Properties, and click the **Unblock** button (see Figure 2).</span></span>

<span data-ttu-id="ff657-114">[![desbloquear o arquivo ZIP do AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="ff657-114">[![Unblocking the AJAX Control Toolkit ZIP file](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)</span></span>

<span data-ttu-id="ff657-115">**Figura 02**: desbloqueio do arquivo zip do AJAX Control Toolkit ([clique para exibir a imagem em tamanho normal](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="ff657-115">**Figure 02**: Unblocking the AJAX Control Toolkit ZIP file([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))</span></span>

<span data-ttu-id="ff657-116">Depois de desbloquear o arquivo, você pode descompactar o arquivo: clique com o botão direito do mouse no arquivo e selecione a opção de menu **extrair tudo** .</span><span class="sxs-lookup"><span data-stu-id="ff657-116">After you unblock the file, you can unzip the file: Right-click the file and select the **Extract All** menu option.</span></span> <span data-ttu-id="ff657-117">Agora, estamos prontos para adicionar o Toolkit à caixa de ferramentas do Visual Studio/Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="ff657-117">Now, we are ready to add the toolkit to the Visual Studio/Visual Web Developer toolbox.</span></span>

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a><span data-ttu-id="ff657-118">Adicionando o AJAX Control Toolkit à caixa de ferramentas</span><span class="sxs-lookup"><span data-stu-id="ff657-118">Adding the AJAX Control Toolkit to the Toolbox</span></span>

<span data-ttu-id="ff657-119">A maneira mais fácil de usar o AJAX Control Toolkit é adicionar o Toolkit à sua caixa de ferramentas do Visual Studio/Visual Web Developer (consulte a Figura 3).</span><span class="sxs-lookup"><span data-stu-id="ff657-119">The easiest way to use the AJAX Control Toolkit is to add the toolkit to your Visual Studio/Visual Web Developer toolbox (see Figure 3).</span></span> <span data-ttu-id="ff657-120">Dessa forma, você pode simplesmente arrastar um controle do kit de ferramentas para uma página quando quiser usá-lo.</span><span class="sxs-lookup"><span data-stu-id="ff657-120">That way, you can simply drag a toolkit control onto a page when you want to use it.</span></span>

<span data-ttu-id="ff657-121">[![AJAX Control Toolkit aparece na caixa de ferramentas](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="ff657-121">[![AJAX Control Toolkit appears in toolbox](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)</span></span>

<span data-ttu-id="ff657-122">**Figura 03**: o AJAX Control Toolkit aparece na caixa de ferramentas ([clique para exibir a imagem em tamanho normal](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="ff657-122">**Figure 03**: AJAX Control Toolkit appears in toolbox([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))</span></span>

<span data-ttu-id="ff657-123">Primeiro, você precisa adicionar uma guia do AJAX Control Toolkit à caixa de ferramentas.</span><span class="sxs-lookup"><span data-stu-id="ff657-123">First, you need to add an AJAX Control Toolkit tab to the toolbox.</span></span> <span data-ttu-id="ff657-124">Siga estas etapas.</span><span class="sxs-lookup"><span data-stu-id="ff657-124">Follow these steps.</span></span>

1. <span data-ttu-id="ff657-125">Crie um novo site do ASP.NET selecionando o arquivo de opção de menu, novo site.</span><span class="sxs-lookup"><span data-stu-id="ff657-125">Create a new ASP.NET Website by selecting the menu option File, New Website.</span></span> <span data-ttu-id="ff657-126">Clique duas vezes em Default. aspx na janela Gerenciador de Soluções para abrir o arquivo no editor.</span><span class="sxs-lookup"><span data-stu-id="ff657-126">Double-click the Default.aspx in the Solution Explorer window to open the file in the editor.</span></span>
2. <span data-ttu-id="ff657-127">Clique com o botão direito do mouse na caixa de ferramentas abaixo da guia geral e selecione a opção de menu **Adicionar guia** (consulte a Figura 4).</span><span class="sxs-lookup"><span data-stu-id="ff657-127">Right-click the Toolbox beneath the General Tab and select the menu option **Add Tab** (see Figure 4).</span></span>
3. <span data-ttu-id="ff657-128">Insira uma nova guia chamada AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="ff657-128">Enter a new tab named AJAX Control Toolkit.</span></span>

<span data-ttu-id="ff657-129">[![adicionar uma nova guia](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="ff657-129">[![Adding a new tab](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)</span></span>

<span data-ttu-id="ff657-130">**Figura 04**: adicionando uma nova guia ([clique para exibir a imagem em tamanho normal](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="ff657-130">**Figure 04**: Adding a new tab([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png))</span></span>

<span data-ttu-id="ff657-131">Em seguida, você precisa adicionar os controles do AJAX Control Toolkit à nova guia. Siga estas etapas:</span><span class="sxs-lookup"><span data-stu-id="ff657-131">Next, you need to add the AJAX Control Toolkit controls to the new tab. Follow these steps:</span></span>

- <span data-ttu-id="ff657-132">Clique com o botão direito do mouse abaixo da guia AJAX Control Toolkit e selecione a opção de menu **escolher itens (veja a Figura 5)** .</span><span class="sxs-lookup"><span data-stu-id="ff657-132">Right-click beneath the AJAX Control Toolkit tab and select the menu option **Choose Items (see Figure 5)**.</span></span>
- <span data-ttu-id="ff657-133">Navegue até o local onde você descompactou o AJAX Control Toolkit e selecione o assembly AjaxControlToolkit. dll.</span><span class="sxs-lookup"><span data-stu-id="ff657-133">Browse to the location where you unzipped the AJAX Control Toolkit and select the AjaxControlToolkit.dll assembly.</span></span>

<span data-ttu-id="ff657-134">[![escolher itens para adicionar à caixa de ferramentas](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="ff657-134">[![Choose items to add to the toolbox](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)</span></span>

<span data-ttu-id="ff657-135">**Figura 05**: escolher itens para adicionar à caixa de ferramentas ([clique para exibir a imagem em tamanho normal](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="ff657-135">**Figure 05**: Choose items to add to the toolbox([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))</span></span>

<span data-ttu-id="ff657-136">Depois de concluir essas etapas, todos os controles do kit de ferramentas serão exibidos na caixa de ferramentas.</span><span class="sxs-lookup"><span data-stu-id="ff657-136">After you complete these steps, all of the toolkit controls will appear in your toolbox.</span></span>

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a><span data-ttu-id="ff657-137">Atualizando para uma nova versão do kit de ferramentas</span><span class="sxs-lookup"><span data-stu-id="ff657-137">Upgrading to a New Version of the Toolkit</span></span>

<span data-ttu-id="ff657-138">Se você estava usando uma versão mais antiga do kit de ferramentas e agora precisa migrar para uma versão posterior aqui estão as etapas recomendadas:</span><span class="sxs-lookup"><span data-stu-id="ff657-138">If you were using an older release of the Toolkit and now need to move to a later version here are the recommended steps:</span></span>

- <span data-ttu-id="ff657-139">Binários – exclua a versão antiga do assembly AjaxControlToolkit. dll da pasta bin do seu site.</span><span class="sxs-lookup"><span data-stu-id="ff657-139">Binaries - Delete the old version of the AjaxControlToolkit.dll assembly from your website Bin folder.</span></span>
- <span data-ttu-id="ff657-140">Itens da caixa de ferramentas – exclua a guia AJAX Control Toolkit e siga as etapas acima para recriar a guia com a nova versão do assembly AjaxControlToolkit. dll.</span><span class="sxs-lookup"><span data-stu-id="ff657-140">Toolbox Items - Delete the AJAX Control Toolkit tab and follow the steps above to re-create the tab with the new version of the AjaxControlToolkit.dll assembly.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ff657-141">Próximo</span><span class="sxs-lookup"><span data-stu-id="ff657-141">Next</span></span>](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
