---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: Detecção de classe de inicialização OWIN | Microsoft Docs
author: Praburaj
description: Este tutorial mostra como configurar qual classe de inicialização OWIN é carregada. Para obter mais informações sobre o OWIN, consulte uma visão geral do Project katana. Este tutorial foi...
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: e1670c32049ec33dd4d1941a091a429d3929c452
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78617043"
---
# <a name="owin-startup-class-detection"></a><span data-ttu-id="c33fb-105">Detecção de classe de inicialização OWIN</span><span class="sxs-lookup"><span data-stu-id="c33fb-105">OWIN Startup Class Detection</span></span>

> <span data-ttu-id="c33fb-106">Este tutorial mostra como configurar qual classe de inicialização OWIN é carregada.</span><span class="sxs-lookup"><span data-stu-id="c33fb-106">This tutorial shows how to configure which OWIN startup class is loaded.</span></span> <span data-ttu-id="c33fb-107">Para obter mais informações sobre o OWIN, consulte [uma visão geral do Project Katana](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="c33fb-107">For more information on OWIN, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="c33fb-108">Este tutorial foi escrito por Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan e Howard dierking ( [@howard\_Dierking](https://twitter.com/howard_dierking) ).</span><span class="sxs-lookup"><span data-stu-id="c33fb-108">This tutorial was written by Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan, and Howard Dierking ( [@howard\_dierking](https://twitter.com/howard_dierking) ).</span></span>
>
> ## <a name="prerequisites"></a><span data-ttu-id="c33fb-109">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="c33fb-109">Prerequisites</span></span>
>
> [<span data-ttu-id="c33fb-110">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="c33fb-110">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/)

## <a name="owin-startup-class-detection"></a><span data-ttu-id="c33fb-111">Detecção de classe de inicialização OWIN</span><span class="sxs-lookup"><span data-stu-id="c33fb-111">OWIN Startup Class Detection</span></span>

 <span data-ttu-id="c33fb-112">Cada aplicativo OWIN tem uma classe de inicialização na qual você especifica componentes para o pipeline de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c33fb-112">Every OWIN Application has a startup class where you specify components for the application pipeline.</span></span> <span data-ttu-id="c33fb-113">Há diferentes maneiras pelas quais você pode conectar sua classe de inicialização com o tempo de execução, dependendo do modelo de hospedagem que você escolher (OwinHost, IIS e IIS-Express).</span><span class="sxs-lookup"><span data-stu-id="c33fb-113">There are different ways you can connect your startup class with the runtime, depending on the hosting model you choose (OwinHost, IIS, and IIS-Express).</span></span> <span data-ttu-id="c33fb-114">A classe de inicialização mostrada neste tutorial pode ser usada em todos os aplicativos de hospedagem.</span><span class="sxs-lookup"><span data-stu-id="c33fb-114">The startup class shown in this tutorial can be used in every hosting application.</span></span> <span data-ttu-id="c33fb-115">Você conecta a classe de inicialização ao tempo de execução de hospedagem usando uma das abordagens a seguir:</span><span class="sxs-lookup"><span data-stu-id="c33fb-115">You connect the startup class with the hosting runtime using one of the these approaches:</span></span>

1. <span data-ttu-id="c33fb-116">**Convenção de nomenclatura**: Katana procura uma classe chamada `Startup` no namespace que corresponde ao nome do assembly ou ao namespace global.</span><span class="sxs-lookup"><span data-stu-id="c33fb-116">**Naming Convention**: Katana looks for a class named `Startup` in namespace matching the assembly name or the global namespace.</span></span>
2. <span data-ttu-id="c33fb-117">**Atributo OwinStartup**: essa é a abordagem que a maioria dos desenvolvedores demorará para especificar a classe de inicialização.</span><span class="sxs-lookup"><span data-stu-id="c33fb-117">**OwinStartup Attribute**: This is the approach most developers will take to specify the startup class.</span></span> <span data-ttu-id="c33fb-118">O atributo a seguir definirá a classe de inicialização para a classe `TestStartup` no namespace `StartupDemo`.</span><span class="sxs-lookup"><span data-stu-id="c33fb-118">The following attribute will set the startup class to the `TestStartup` class in the `StartupDemo` namespace.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

   <span data-ttu-id="c33fb-119">O atributo `OwinStartup` substitui a Convenção de nomenclatura.</span><span class="sxs-lookup"><span data-stu-id="c33fb-119">The `OwinStartup` attribute overrides the naming convention.</span></span> <span data-ttu-id="c33fb-120">Você também pode especificar um nome amigável com esse atributo, no entanto, usar um nome amigável requer que você também use o elemento `appSetting` no arquivo de configuração.</span><span class="sxs-lookup"><span data-stu-id="c33fb-120">You can also specify a friendly name with this attribute, however, using a friendly name requires you to also use the `appSetting` element in the configuration file.</span></span>
3. <span data-ttu-id="c33fb-121">**O elemento appSetting no arquivo de configuração**: o elemento `appSetting` substitui o atributo `OwinStartup` e a Convenção de nomenclatura.</span><span class="sxs-lookup"><span data-stu-id="c33fb-121">**The appSetting element in the Configuration file**: The `appSetting` element overrides the `OwinStartup` attribute and naming convention.</span></span> <span data-ttu-id="c33fb-122">Você pode ter várias classes de inicialização (cada uma usando um atributo `OwinStartup`) e configurar qual classe de inicialização será carregada em um arquivo de configuração usando marcação semelhante à seguinte:</span><span class="sxs-lookup"><span data-stu-id="c33fb-122">You can have multiple startup classes (each using an `OwinStartup` attribute) and configure which startup class will be loaded in a configuration file using markup similar to the following:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

   <span data-ttu-id="c33fb-123">A chave a seguir, que especifica explicitamente a classe de inicialização e o assembly também pode ser usada:</span><span class="sxs-lookup"><span data-stu-id="c33fb-123">The following key, which explicitly specifies the startup class and assembly can also be used:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

   <span data-ttu-id="c33fb-124">O XML a seguir no arquivo de configuração especifica um nome de classe de inicialização amigável de `ProductionConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="c33fb-124">The following XML in the configuration file specifies a friendly startup class name of `ProductionConfiguration`.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

   <span data-ttu-id="c33fb-125">A marcação acima deve ser usada com o atributo `OwinStartup` a seguir, que especifica um nome amigável e faz com que a classe `ProductionStartup2` seja executada.</span><span class="sxs-lookup"><span data-stu-id="c33fb-125">The above markup must be used with the following `OwinStartup` attribute which specifies a friendly name and causes the `ProductionStartup2` class to run.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. <span data-ttu-id="c33fb-126">Para desabilitar a descoberta de inicialização do OWIN, adicione o `appSetting owin:AutomaticAppStartup` com um valor de `"false"` no arquivo Web. config.</span><span class="sxs-lookup"><span data-stu-id="c33fb-126">To disable OWIN startup discovery add the `appSetting owin:AutomaticAppStartup` with a value of `"false"` in the web.config file.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a><span data-ttu-id="c33fb-127">Criar um aplicativo Web ASP.NET usando a inicialização do OWIN</span><span class="sxs-lookup"><span data-stu-id="c33fb-127">Create an ASP.NET Web App using OWIN Startup</span></span>

1. <span data-ttu-id="c33fb-128">Crie um aplicativo Web Asp.Net vazio e nomeie-o **StartupDemo**.</span><span class="sxs-lookup"><span data-stu-id="c33fb-128">Create an empty Asp.Net web application and name it **StartupDemo**.</span></span> <span data-ttu-id="c33fb-129">-Instale `Microsoft.Owin.Host.SystemWeb` usando o Gerenciador de pacotes NuGet.</span><span class="sxs-lookup"><span data-stu-id="c33fb-129">- Install `Microsoft.Owin.Host.SystemWeb` using the NuGet package manager.</span></span> <span data-ttu-id="c33fb-130">No menu **ferramentas** , selecione **Gerenciador de pacotes NuGet**e, em seguida, **console do Gerenciador de pacotes**.</span><span class="sxs-lookup"><span data-stu-id="c33fb-130">From the **Tools** menu, select **NuGet Package Manager**, and then **Package Manager Console**.</span></span> <span data-ttu-id="c33fb-131">Insira o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="c33fb-131">Enter the following command:</span></span>

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
2. <span data-ttu-id="c33fb-132">Adicione uma classe de inicialização OWIN.</span><span class="sxs-lookup"><span data-stu-id="c33fb-132">Add an OWIN startup class.</span></span> <span data-ttu-id="c33fb-133">No Visual Studio 2017, clique com o botão direito do mouse no projeto e selecione **Adicionar classe**.-na caixa de diálogo **Adicionar novo item** , digite *OWIN* no campo de pesquisa e altere o nome para startup.cs e, em seguida, selecione **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="c33fb-133">In Visual Studio 2017 right-click the project and select **Add Class**.- In the **Add New Item** dialog box, enter *OWIN* in the search field, and change the name to Startup.cs, and then select **Add**.</span></span>

     ![](owin-startup-class-detection/_static/image1.png)

   <span data-ttu-id="c33fb-134">Na próxima vez que você quiser adicionar uma *classe de inicialização Owin*, ela estará disponível no menu **Adicionar** .</span><span class="sxs-lookup"><span data-stu-id="c33fb-134">The next time you want to add an *Owin Startup class*, it will be in available from the **Add** menu.</span></span>

     ![](owin-startup-class-detection/_static/image2.png)

   <span data-ttu-id="c33fb-135">Como alternativa, você pode clicar com o botão direito do mouse no projeto e selecionar **Adicionar**, selecionar **novo item**e, em seguida, selecionar a **classe de inicialização Owin**.</span><span class="sxs-lookup"><span data-stu-id="c33fb-135">Alternatively, you can right-click the project and select **Add**, then select **New Item**, and then select the **Owin Startup class**.</span></span>

     ![](owin-startup-class-detection/_static/image3.png)

- <span data-ttu-id="c33fb-136">Substitua o código gerado no arquivo *Startup.cs* pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="c33fb-136">Replace the generated code in the *Startup.cs* file with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]

  <span data-ttu-id="c33fb-137">A `app.Use` expressão lambda é usada para registrar o componente de middleware especificado no pipeline OWIN.</span><span class="sxs-lookup"><span data-stu-id="c33fb-137">The `app.Use` lambda expression is used to register the specified middleware component to the OWIN pipeline.</span></span> <span data-ttu-id="c33fb-138">Nesse caso, estamos Configurando o log de solicitações de entrada antes de responder à solicitação de entrada.</span><span class="sxs-lookup"><span data-stu-id="c33fb-138">In this case we are setting up logging of incoming requests before responding to the incoming request.</span></span> <span data-ttu-id="c33fb-139">O parâmetro `next` é o delegado ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [Task](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt;) para o próximo componente no pipeline.</span><span class="sxs-lookup"><span data-stu-id="c33fb-139">The `next` parameter is the delegate ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [Task](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) to the next component in the pipeline.</span></span> <span data-ttu-id="c33fb-140">O `app.Run` expressão lambda conecta o pipeline a solicitações de entrada e fornece o mecanismo de resposta.</span><span class="sxs-lookup"><span data-stu-id="c33fb-140">The `app.Run` lambda expression hooks up the pipeline to incoming requests and provides the response mechanism.</span></span>
     > [!NOTE]
     > <span data-ttu-id="c33fb-141">No código acima, comentamos o atributo `OwinStartup` e estamos contando com a Convenção de executar a classe chamada `Startup`.-Pressione ***F5*** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c33fb-141">In the code above we have commented out the `OwinStartup` attribute and we're relying on the convention of running the class named `Startup` .- Press ***F5*** to run the application.</span></span> <span data-ttu-id="c33fb-142">Pressione atualizar algumas vezes.</span><span class="sxs-lookup"><span data-stu-id="c33fb-142">Hit refresh a few times.</span></span>

    <span data-ttu-id="c33fb-143">![](owin-startup-class-detection/_static/image4.png) Observação: o número mostrado nas imagens neste tutorial não corresponderá ao número que você vê.</span><span class="sxs-lookup"><span data-stu-id="c33fb-143">![](owin-startup-class-detection/_static/image4.png) Note: The number shown in the images in this tutorial will not match the number you see.</span></span> <span data-ttu-id="c33fb-144">A cadeia de caracteres de milissegundos é usada para mostrar uma nova resposta quando você atualiza a página.</span><span class="sxs-lookup"><span data-stu-id="c33fb-144">The millisecond string is used to show a new response when you refresh the page.</span></span>
  <span data-ttu-id="c33fb-145">Você pode ver as informações de rastreamento na janela **saída** .</span><span class="sxs-lookup"><span data-stu-id="c33fb-145">You can see the trace information in the **Output** window.</span></span>

    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a><span data-ttu-id="c33fb-146">Adicionar mais classes de inicialização</span><span class="sxs-lookup"><span data-stu-id="c33fb-146">Add More Startup Classes</span></span>

<span data-ttu-id="c33fb-147">Nesta seção, vamos adicionar outra classe Startup.</span><span class="sxs-lookup"><span data-stu-id="c33fb-147">In this section we'll add another Startup class.</span></span> <span data-ttu-id="c33fb-148">Você pode adicionar várias classes de inicialização OWIN ao seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c33fb-148">You can add multiple OWIN startup class to your application.</span></span> <span data-ttu-id="c33fb-149">Por exemplo, talvez você queira criar classes de inicialização para desenvolvimento, teste e produção.</span><span class="sxs-lookup"><span data-stu-id="c33fb-149">For example, you might want to create startup classes for development, testing and production.</span></span>

1. <span data-ttu-id="c33fb-150">Crie uma nova classe de inicialização OWIN e nomeie-a `ProductionStartup`.</span><span class="sxs-lookup"><span data-stu-id="c33fb-150">Create a new OWIN Startup class and name it `ProductionStartup`.</span></span>
2. <span data-ttu-id="c33fb-151">Substitua o código gerado pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="c33fb-151">Replace the generated code with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. <span data-ttu-id="c33fb-152">Pressione Control F5 para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c33fb-152">Press Control F5 to run the app.</span></span> <span data-ttu-id="c33fb-153">O atributo `OwinStartup` especifica que a classe de inicialização de produção é executada.</span><span class="sxs-lookup"><span data-stu-id="c33fb-153">The `OwinStartup` attribute specifies the production startup class is run.</span></span>

    ![](owin-startup-class-detection/_static/image6.png)
4. <span data-ttu-id="c33fb-154">Crie outra classe de inicialização OWIN e nomeie-a `TestStartup`.</span><span class="sxs-lookup"><span data-stu-id="c33fb-154">Create another OWIN Startup class and name it `TestStartup`.</span></span>
5. <span data-ttu-id="c33fb-155">Substitua o código gerado pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="c33fb-155">Replace the generated code with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

   <span data-ttu-id="c33fb-156">A sobrecarga de atributo `OwinStartup` acima Especifica `TestingConfiguration` como o nome *amigável* da classe de inicialização.</span><span class="sxs-lookup"><span data-stu-id="c33fb-156">The `OwinStartup` attribute overload above specifies `TestingConfiguration` as the *friendly* name of the Startup class.</span></span>
6. <span data-ttu-id="c33fb-157">Abra o arquivo *Web. config* e adicione a chave de inicialização do aplicativo OWIN que especifica o nome amigável da classe de inicialização:</span><span class="sxs-lookup"><span data-stu-id="c33fb-157">Open the *web.config* file and add the OWIN App startup key which specifies the friendly name of the Startup class:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. <span data-ttu-id="c33fb-158">Pressione Control F5 para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c33fb-158">Press Control F5 to run the app.</span></span> <span data-ttu-id="c33fb-159">O elemento de configurações do aplicativo usa o precedente e a configuração de teste é executada.</span><span class="sxs-lookup"><span data-stu-id="c33fb-159">The app settings element takes precedent, and the test configuration is run.</span></span>

    ![](owin-startup-class-detection/_static/image7.png)
8. <span data-ttu-id="c33fb-160">Remova o nome *amigável* do atributo `OwinStartup` na classe `TestStartup`.</span><span class="sxs-lookup"><span data-stu-id="c33fb-160">Remove the *friendly* name from the `OwinStartup` attribute in the `TestStartup` class.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. <span data-ttu-id="c33fb-161">Substitua a chave de inicialização do aplicativo OWIN no arquivo *Web. config* pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="c33fb-161">Replace the OWIN App startup key in the *web.config* file with the following:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. <span data-ttu-id="c33fb-162">Reverta o atributo `OwinStartup` em cada classe para o código de atributo padrão gerado pelo Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="c33fb-162">Revert the `OwinStartup` attribute in each class to the default attribute code generated by Visual Studio:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

    <span data-ttu-id="c33fb-163">Cada uma das chaves de inicialização do aplicativo OWIN abaixo fará com que a classe de produção seja executada.</span><span class="sxs-lookup"><span data-stu-id="c33fb-163">Each of the OWIN App startup keys below will cause the production class to run.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

    <span data-ttu-id="c33fb-164">A última chave de inicialização especifica o método de configuração de inicialização.</span><span class="sxs-lookup"><span data-stu-id="c33fb-164">The last startup key specifies the startup configuration method.</span></span> <span data-ttu-id="c33fb-165">A chave de inicialização do aplicativo OWIN a seguir permite que você altere o nome da classe de configuração para `MyConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="c33fb-165">The following OWIN App startup key allows you to change the name of the configuration class to `MyConfiguration` .</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a><span data-ttu-id="c33fb-166">Usando Owinhost. exe</span><span class="sxs-lookup"><span data-stu-id="c33fb-166">Using Owinhost.exe</span></span>

1. <span data-ttu-id="c33fb-167">Substitua o arquivo Web. config pela seguinte marcação:</span><span class="sxs-lookup"><span data-stu-id="c33fb-167">Replace the Web.config file with the following markup:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

   <span data-ttu-id="c33fb-168">A última chave vence, portanto, nesse caso `TestStartup` é especificado.</span><span class="sxs-lookup"><span data-stu-id="c33fb-168">The last key wins, so in this case `TestStartup` is specified.</span></span>
2. <span data-ttu-id="c33fb-169">Instale o Owinhost do PMC:</span><span class="sxs-lookup"><span data-stu-id="c33fb-169">Install Owinhost from the PMC:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. <span data-ttu-id="c33fb-170">Navegue até a pasta do aplicativo (a pasta que contém o arquivo *Web. config* ) e, em um prompt de comando, digite:</span><span class="sxs-lookup"><span data-stu-id="c33fb-170">Navigate to the application folder (the folder containing the *Web.config* file) and in a command prompt and type:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

   <span data-ttu-id="c33fb-171">A janela de comando mostrará:</span><span class="sxs-lookup"><span data-stu-id="c33fb-171">The command window will show:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. <span data-ttu-id="c33fb-172">Inicie um navegador com a URL `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="c33fb-172">Launch a browser with the URL `http://localhost:5000/`.</span></span>

    ![](owin-startup-class-detection/_static/image8.png)

   <span data-ttu-id="c33fb-173">OwinHost homenageau as convenções de inicialização listadas acima.</span><span class="sxs-lookup"><span data-stu-id="c33fb-173">OwinHost honored the startup conventions listed above.</span></span>
5. <span data-ttu-id="c33fb-174">Na janela de comando, pressione ENTER para sair de OwinHost.</span><span class="sxs-lookup"><span data-stu-id="c33fb-174">In the command window, press Enter to exit OwinHost.</span></span>
6. <span data-ttu-id="c33fb-175">Na classe `ProductionStartup`, adicione o seguinte atributo OwinStartup que especifica um nome amigável de *ProductionConfiguration*.</span><span class="sxs-lookup"><span data-stu-id="c33fb-175">In the `ProductionStartup` class, add the following OwinStartup attribute which specifies a friendly name of *ProductionConfiguration*.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. <span data-ttu-id="c33fb-176">No prompt de comando e digite:</span><span class="sxs-lookup"><span data-stu-id="c33fb-176">In the command prompt and type:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

   <span data-ttu-id="c33fb-177">A classe de inicialização de produção é carregada.</span><span class="sxs-lookup"><span data-stu-id="c33fb-177">The Production startup class is loaded.</span></span>
    ![](owin-startup-class-detection/_static/image9.png)

   <span data-ttu-id="c33fb-178">Nosso aplicativo tem várias classes de inicialização e, neste exemplo, adiamos qual classe de inicialização carregar até o tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="c33fb-178">Our application has multiple startup classes, and in this example we have deferred which startup class to load until runtime.</span></span>
8. <span data-ttu-id="c33fb-179">Teste as seguintes opções de inicialização de tempo de execução:</span><span class="sxs-lookup"><span data-stu-id="c33fb-179">Test the following runtime startup options:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
