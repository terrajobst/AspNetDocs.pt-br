---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
title: Usando controles do Kit de ferramentas de controle AJAX e extensores de controle (c#) | Microsoft Docs
author: microsoft
description: Saiba como adicionar controles do AJAX Control Toolkit e extensores para suas páginas ASP.NET.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: c1e6b51c-3bc3-4bf7-9916-9991197af3dd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
msc.type: authoredcontent
ms.openlocfilehash: 82fae91e40ec2f1508fe5c82992eeef4abc4e19a
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59419217"
---
# <a name="using-ajax-control-toolkit-controls-and-control-extenders-c"></a><span data-ttu-id="77aad-103">Uso de controles e extensores de controle do AJAX Control Toolkit (C#)</span><span class="sxs-lookup"><span data-stu-id="77aad-103">Using AJAX Control Toolkit Controls and Control Extenders (C#)</span></span>

<span data-ttu-id="77aad-104">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="77aad-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="77aad-105">Saiba como adicionar controles do AJAX Control Toolkit e extensores para suas páginas ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="77aad-105">Learn how to add AJAX Control Toolkit controls and extenders to your ASP.NET pages.</span></span>


<span data-ttu-id="77aad-106">O AJAX Control Toolkit contém um conjunto de controles e extensores de controle.</span><span class="sxs-lookup"><span data-stu-id="77aad-106">The AJAX Control Toolkit contains a set of controls and control extenders.</span></span> <span data-ttu-id="77aad-107">Este breve tutorial, você aprenderá como adicionar controles e extensores de controle a uma página ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="77aad-107">In this brief tutorial, you learn how to add both controls and control extenders to an ASP.NET page.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="77aad-108">Para obter instruções sobre como instalar o AJAX Control Toolkit e adicionando o AJAX Control Toolkit para a caixa de ferramentas do Visual Studio/Visual Web Developer, consulte o tutorial [Introdução ao AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs.md).</span><span class="sxs-lookup"><span data-stu-id="77aad-108">For instructions on installing the AJAX Control Toolkit and adding the AJAX Control Toolkit to the Visual Studio/Visual Web Developer toolbox, see the tutorial [Get Started with the AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs.md).</span></span>


## <a name="using-ajax-control-toolkit-controls"></a><span data-ttu-id="77aad-109">Usando controles do Kit de ferramentas de controle AJAX</span><span class="sxs-lookup"><span data-stu-id="77aad-109">Using AJAX Control Toolkit Controls</span></span>

<span data-ttu-id="77aad-110">Um controle do AJAX Control Toolkit funciona como um controle ASP.NET normal.</span><span class="sxs-lookup"><span data-stu-id="77aad-110">An AJAX Control Toolkit control works just like a normal ASP.NET control.</span></span> <span data-ttu-id="77aad-111">Você pode arrastar o controle da caixa de ferramentas em uma página ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="77aad-111">You can drag the control from the toolbox onto an ASP.NET page.</span></span> <span data-ttu-id="77aad-112">Você pode adicionar o controle para a página no modo de exibição de Design ou exibição da fonte.</span><span class="sxs-lookup"><span data-stu-id="77aad-112">You can add the control to the page in either Design view or Source view.</span></span>

<span data-ttu-id="77aad-113">Há um requisito especial ao usar os controles do AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="77aad-113">There is one special requirement when using the controls from the AJAX Control Toolkit.</span></span> <span data-ttu-id="77aad-114">A página deve conter um controle ScriptManager.</span><span class="sxs-lookup"><span data-stu-id="77aad-114">The page must contain a ScriptManager control.</span></span> <span data-ttu-id="77aad-115">O controle ScriptManager é responsável por incluir todos o JavaScript necessário necessário pelos controles AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="77aad-115">The ScriptManager control is responsible for including all of the necessary JavaScript required by the AJAX Control Toolkit controls.</span></span>

<span data-ttu-id="77aad-116">Por exemplo, na guia do AJAX Control Toolkit inclui um controle chamado o controle do Editor.</span><span class="sxs-lookup"><span data-stu-id="77aad-116">For example, the AJAX Control Toolkit tab includes a control named the Editor control.</span></span> <span data-ttu-id="77aad-117">Esse controle exibe um rico editor de HTML.</span><span class="sxs-lookup"><span data-stu-id="77aad-117">This control displays a rich HTML editor.</span></span> <span data-ttu-id="77aad-118">Siga estas etapas para adicionar o controle de Editor para uma página:</span><span class="sxs-lookup"><span data-stu-id="77aad-118">Follow these steps to add the Editor control to a page:</span></span>

1. <span data-ttu-id="77aad-119">Criar uma nova página do ASP.NET chamada ShowEditor.aspx</span><span class="sxs-lookup"><span data-stu-id="77aad-119">Create a new ASP.NET page named ShowEditor.aspx</span></span>
2. <span data-ttu-id="77aad-120">Selecione o controle ScriptManager do guia Extensões AJAX na caixa de ferramentas e arraste o controle para a página.</span><span class="sxs-lookup"><span data-stu-id="77aad-120">Select the ScriptManager control from beneath the AJAX Extensions tab in the toolbox and drag the control onto the page.</span></span>
3. <span data-ttu-id="77aad-121">Selecione o controle de Editor do AJAX Control Toolkit guia na caixa de ferramentas e arraste o controle para a página (consulte a Figura 1).</span><span class="sxs-lookup"><span data-stu-id="77aad-121">Select the Editor control from beneath the AJAX Control Toolkit tab in the toolbox and drag the control onto the page (see Figure 1).</span></span> <span data-ttu-id="77aad-122">O Designer deve ser semelhante a Figura 2.</span><span class="sxs-lookup"><span data-stu-id="77aad-122">The Designer should look like Figure 2.</span></span>
4. <span data-ttu-id="77aad-123">Executar o site selecionando a opção de menu **depurar, iniciar depuração** ou pressionando a tecla F5.</span><span class="sxs-lookup"><span data-stu-id="77aad-123">Run the web site by selecting the menu option **Debug, Start Debugging** or hitting the F5 key.</span></span>
5. <span data-ttu-id="77aad-124">Você verá a página na Figura 3.</span><span class="sxs-lookup"><span data-stu-id="77aad-124">You should see the page in Figure 3.</span></span>


[![S<span data-ttu-id="77aad-125">eleger o controle de Editor de HTML]</span><span class="sxs-lookup"><span data-stu-id="77aad-125">electing the HTML Editor control]</span></span>(using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.png)

<span data-ttu-id="77aad-126">**Figura 01**: Selecionando o controle de Editor de HTML ([clique para exibir a imagem em tamanho normal](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="77aad-126">**Figure 01**: Selecting the HTML Editor control([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.png))</span></span>


[![V<span data-ttu-id="77aad-127">Visual Studio Designer com o controle ScriptManager e edite]</span><span class="sxs-lookup"><span data-stu-id="77aad-127">isual Studio Designer with ScriptManager and Edit control]</span></span>(using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.png)

<span data-ttu-id="77aad-128">**Figura 02**: Designer do Visual Studio com o controle ScriptManager e edição ([clique para exibir a imagem em tamanho normal](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="77aad-128">**Figure 02**: Visual Studio Designer with ScriptManager and Edit control([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.png))</span></span>


[![T<span data-ttu-id="77aad-129">página de DisplayEditor.aspx he]</span><span class="sxs-lookup"><span data-stu-id="77aad-129">he DisplayEditor.aspx page]</span></span>(using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.png)

<span data-ttu-id="77aad-130">**Figura 03**: A página DisplayEditor.aspx ([clique para exibir a imagem em tamanho normal](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="77aad-130">**Figure 03**: The DisplayEditor.aspx page([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.png))</span></span>


## <a name="using-ajax-control-toolkit-control-extenders"></a><span data-ttu-id="77aad-131">Usando extensores de controle do Kit de ferramentas de controle do AJAX</span><span class="sxs-lookup"><span data-stu-id="77aad-131">Using AJAX Control Toolkit Control Extenders</span></span>

<span data-ttu-id="77aad-132">O AJAX Control Toolkit também contém os extensores de controle.</span><span class="sxs-lookup"><span data-stu-id="77aad-132">The AJAX Control Toolkit also contains control extenders.</span></span> <span data-ttu-id="77aad-133">Como o nome sugere, um extensor de controle estende a funcionalidade de um controle existente.</span><span class="sxs-lookup"><span data-stu-id="77aad-133">As its name suggests, a control extender extends the functionality of an existing control.</span></span> <span data-ttu-id="77aad-134">Por exemplo, o extensor ConfirmButton do controle estende o controle de botão do ASP.NET padrão.</span><span class="sxs-lookup"><span data-stu-id="77aad-134">For example, the ConfirmButton control extender extends the standard ASP.NET Button control.</span></span> <span data-ttu-id="77aad-135">O extensor altera o comportamento de s de controle de botão para que o botão exibe uma caixa de diálogo de confirmação quando você clica nele.</span><span class="sxs-lookup"><span data-stu-id="77aad-135">The extender changes the Button control�s behavior so that the Button displays a confirmation dialog when you click it.</span></span>

<span data-ttu-id="77aad-136">Um extensor de controle, assim como um controle do AJAX Control Toolkit, requer um controle ScriptManager.</span><span class="sxs-lookup"><span data-stu-id="77aad-136">A control extender, just like an AJAX Control Toolkit control, requires a ScriptManager control.</span></span> <span data-ttu-id="77aad-137">Você deve adicionar um controle ScriptManager para uma página para começar a usar extensores de controle na página.</span><span class="sxs-lookup"><span data-stu-id="77aad-137">You must add a ScriptManager control to a page before you start using control extenders in the page.</span></span>

<span data-ttu-id="77aad-138">Siga estas etapas para usar o extensor do controle ConfirmButton:</span><span class="sxs-lookup"><span data-stu-id="77aad-138">Follow these steps to use the ConfirmButton control extender:</span></span>

1. <span data-ttu-id="77aad-139">Criar uma nova página do ASP.NET chamada ShowConfirmButton.aspx</span><span class="sxs-lookup"><span data-stu-id="77aad-139">Create a new ASP.NET page named ShowConfirmButton.aspx</span></span>
2. <span data-ttu-id="77aad-140">Adicione um controle ScriptManager à página, arrastando o controle para a página de guia Extensões AJAX.</span><span class="sxs-lookup"><span data-stu-id="77aad-140">Add a ScriptManager control to the page by dragging the control onto the page from beneath the AJAX Extensions tab.</span></span>
3. <span data-ttu-id="77aad-141">Adicione um controle de botão padrão para a página, arrastando o botão de guia padrão na caixa de ferramentas para a superfície de Designer.</span><span class="sxs-lookup"><span data-stu-id="77aad-141">Add a standard Button control to the page by dragging the Button from beneath the Standard tab in the toolbox onto the Designer surface.</span></span>
4. <span data-ttu-id="77aad-142">Clique o **adicionar extensor** opção de tarefa (consulte a Figura 4).</span><span class="sxs-lookup"><span data-stu-id="77aad-142">Click the **Add Extender** task option (see Figure 4).</span></span>
5. <span data-ttu-id="77aad-143">Na caixa de diálogo Choose Extender, selecione ConfirmButtonExtender (consulte a Figura 5) e clique no botão Okey.</span><span class="sxs-lookup"><span data-stu-id="77aad-143">In the Choose Extender dialog, select ConfirmButtonExtender (see Figure 5) and click the OK button.</span></span>
6. <span data-ttu-id="77aad-144">Selecione o controle de botão no Designer e expanda os extensores, Button1\_ConfirmButtonExtender nó na janela Propriedades (veja a Figura 6).</span><span class="sxs-lookup"><span data-stu-id="77aad-144">Select the Button control in the Designer and expand the Extenders, Button1\_ConfirmButtonExtender node in the Properties window (see Figure 6).</span></span> <span data-ttu-id="77aad-145">Atribua o valor *realmente?* para a propriedade ConfirmText.</span><span class="sxs-lookup"><span data-stu-id="77aad-145">Assign the value *�Really?�* to the ConfirmText property.</span></span>
7. <span data-ttu-id="77aad-146">Execute a página, selecionando a opção de menu **depurar, iniciar depuração** ou pressionar a tecla F5.</span><span class="sxs-lookup"><span data-stu-id="77aad-146">Run the page by selecting the menu option **Debug, Start Debugging** or hit the F5 key.</span></span>


[![T<span data-ttu-id="77aad-147">ele opção de tarefa de adicionar extensor]</span><span class="sxs-lookup"><span data-stu-id="77aad-147">he Add Extender task option]</span></span>(using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.png)

<span data-ttu-id="77aad-148">**Figura 04**: A opção de tarefa de adicionar extensor ([clique para exibir a imagem em tamanho normal](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="77aad-148">**Figure 04**: The Add Extender task option([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image8.png))</span></span>


[![S<span data-ttu-id="77aad-149">eleger o extensor ConfirmButton do controle]</span><span class="sxs-lookup"><span data-stu-id="77aad-149">electing the ConfirmButton control extender]</span></span>(using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image9.png)

<span data-ttu-id="77aad-150">**Figura 05**: Selecionando o extensor ConfirmButton do controle ([clique para exibir a imagem em tamanho normal](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="77aad-150">**Figure 05**: Selecting the ConfirmButton control extender([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image10.png))</span></span>


[![S<span data-ttu-id="77aad-151">configuração de uma propriedade ConfirmButton]</span><span class="sxs-lookup"><span data-stu-id="77aad-151">etting a ConfirmButton property]</span></span>(using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image11.png)

<span data-ttu-id="77aad-152">**Figura 06**: Definir uma propriedade ConfirmButton ([clique para exibir a imagem em tamanho normal](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="77aad-152">**Figure 06**: Setting a ConfirmButton property([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image12.png))</span></span>


<span data-ttu-id="77aad-153">Quando a página é aberta, você verá um botão.</span><span class="sxs-lookup"><span data-stu-id="77aad-153">When the page opens, you should see a button.</span></span> <span data-ttu-id="77aad-154">Quando você clica no botão, você obtém a caixa de diálogo de confirmação na Figura 7.</span><span class="sxs-lookup"><span data-stu-id="77aad-154">When you click the button, you get the confirmation dialog in Figure 7.</span></span>


[![D<span data-ttu-id="77aad-155">a caixa de diálogo de confirmação de isplaying]</span><span class="sxs-lookup"><span data-stu-id="77aad-155">isplaying the confirmation dialog]</span></span>(using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image13.png)

<span data-ttu-id="77aad-156">**Figura 07**: Exibir a caixa de diálogo de confirmação ([clique para exibir a imagem em tamanho normal](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="77aad-156">**Figure 07**: Displaying the confirmation dialog([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image14.png))</span></span>


<span data-ttu-id="77aad-157">Observe que você normalmente não arrastar um extensor de controle para uma página.</span><span class="sxs-lookup"><span data-stu-id="77aad-157">Notice that you normally do not drag a control extender onto a page.</span></span> <span data-ttu-id="77aad-158">Em vez disso, você usa o **adicionar extensor** opção para adicionar um extensor a um controle que você já tiver adicionado a uma página de tarefas.</span><span class="sxs-lookup"><span data-stu-id="77aad-158">Instead, you use the **Add Extender** task option to add an extender to a control that you have already added to a page.</span></span> <span data-ttu-id="77aad-159">Além disso, observe que você defina controle extensor propriedades abrindo a folha de propriedades para o controle que está sendo estendido.</span><span class="sxs-lookup"><span data-stu-id="77aad-159">Notice, furthermore, that you set control extender properties by opening the property sheet for the control being extended.</span></span>

<span data-ttu-id="77aad-160">Um único controle ASP.NET pode ser estendido por vários extensores de controle.</span><span class="sxs-lookup"><span data-stu-id="77aad-160">A single ASP.NET control can be extended by multiple control extenders.</span></span> <span data-ttu-id="77aad-161">A folha de propriedades para o controle que está sendo estendido lista todos os extensores de controle associados ao controle.</span><span class="sxs-lookup"><span data-stu-id="77aad-161">The property sheet for the control being extended will list all of the control extenders associated with the control.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="77aad-162">[Anterior](get-started-with-the-ajax-control-toolkit-cs.md)
> [Próximo](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)</span><span class="sxs-lookup"><span data-stu-id="77aad-162">[Previous](get-started-with-the-ajax-control-toolkit-cs.md)
[Next](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)</span></span>
