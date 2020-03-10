---
uid: whitepapers/aspnet-and-iis6
title: Executando o ASP.NET 1,1 com IIS 6,0 | Microsoft Docs
author: rick-anderson
description: Embora o Windows Server 2003 inclua o IIS 6,0 e o ASP.NET 1,1, esses componentes são desabilitados por padrão. Este white paper descreve como habilitar o IIS 6,0 um...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 2e7812f34481afe9a71927c0d9ba2a9abc9632e4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78523306"
---
# <a name="running-aspnet-11-with-iis-60"></a><span data-ttu-id="f1963-104">Executar o ASP.NET 1.1 com o IIS 6.0</span><span class="sxs-lookup"><span data-stu-id="f1963-104">Running ASP.NET 1.1 with IIS 6.0</span></span>

> <span data-ttu-id="f1963-105">Embora o Windows Server 2003 inclua o IIS 6,0 e o ASP.NET 1,1, esses componentes são desabilitados por padrão.</span><span class="sxs-lookup"><span data-stu-id="f1963-105">While Windows Server 2003 includes both IIS 6.0 and ASP.NET 1.1, these components are disabled by default.</span></span> <span data-ttu-id="f1963-106">Este white paper descreve como habilitar o IIS 6,0 e o ASP.NET 1,1 e recomenda várias definições de configuração para obter o desempenho ideal do IIS e do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f1963-106">This whitepaper describes how to enable IIS 6.0 and ASP.NET 1.1, and recommends several configuration settings to get the optimal performance from IIS and ASP.NET.</span></span>
> 
> <span data-ttu-id="f1963-107">Aplica-se ao ASP.NET 1,1 e ao IIS 6,0.</span><span class="sxs-lookup"><span data-stu-id="f1963-107">Applies to ASP.NET 1.1 and IIS 6.0.</span></span>

<span data-ttu-id="f1963-108">O ASP.NET 1,1 é fornecido com o Windows Server 2003, que também inclui a versão mais recente do IIS (Internet Information Server) versão 6,0.</span><span class="sxs-lookup"><span data-stu-id="f1963-108">ASP.NET 1.1 ships with Windows Server 2003, which also includes the latest version of Internet Information Server (IIS) version 6.0.</span></span> <span data-ttu-id="f1963-109">O IIS 6,0 e o ASP.NET 1,1 foram projetados para integrar diretamente e ASP.NET agora usa como padrão o novo modelo de processo de trabalho do IIS 6,0.</span><span class="sxs-lookup"><span data-stu-id="f1963-109">IIS 6.0 and ASP.NET 1.1 are designed to integrate seamlessly and ASP.NET now defaults to the new IIS 6.0 worker process model.</span></span>

## <a name="aspnet-11-is-not-installed-by-default"></a><span data-ttu-id="f1963-110">O ASP.NET 1,1 não está instalado por padrão</span><span class="sxs-lookup"><span data-stu-id="f1963-110">ASP.NET 1.1 is not installed by default</span></span>

<span data-ttu-id="f1963-111">Ao contrário das versões anteriores dos sistemas operacionais de servidor da Microsoft, o IIS (servidor de informações da Internet) não está habilitado por padrão; Nem é ASP.NET 1,1.</span><span class="sxs-lookup"><span data-stu-id="f1963-111">Unlike previous versions of Microsoft's server operating systems, Internet Information Server (IIS) is not enabled by default; nor is ASP.NET 1.1.</span></span> <span data-ttu-id="f1963-112">Há duas opções para habilitar o IIS:</span><span class="sxs-lookup"><span data-stu-id="f1963-112">There are two options for enabling IIS:</span></span>

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a><span data-ttu-id="f1963-113">Habilitando o IIS, opção #1 Assistente para configurar o servidor</span><span class="sxs-lookup"><span data-stu-id="f1963-113">Enabling IIS, option #1 - Configure Your Server Wizard</span></span>

<span data-ttu-id="f1963-114">O Windows Server 2003 envia um novo ' Assistente para configurar o servidor ' para ajudá-lo a configurar corretamente o servidor no modo desejado.</span><span class="sxs-lookup"><span data-stu-id="f1963-114">Windows Server 2003 ships a new 'Configure Your Server Wizard' to help you properly configure your server in the desired mode.</span></span>

<span data-ttu-id="f1963-115">Para iniciar o assistente-observação, para executar o assistente, você deve estar conectado como administrador-ir para: iniciar | Programas | Ferramentas administrativas e selecione ' configurar seu servidor '.</span><span class="sxs-lookup"><span data-stu-id="f1963-115">To start the wizard - note, to run the wizard you must be logged in as an administrator - go to: Start | Programs | Administrative Tools and select 'Configure Your Server'.</span></span>

<span data-ttu-id="f1963-116">Depois de selecionado, você deverá ver a tela de abertura ' Assistente para configurar o servidor ':</span><span class="sxs-lookup"><span data-stu-id="f1963-116">Once selected you should see the 'Configure Your Server Wizard' opening screen:</span></span>

![](aspnet-and-iis6/_static/image1.jpg)

<span data-ttu-id="f1963-117">Clique em ' próximo &gt;':</span><span class="sxs-lookup"><span data-stu-id="f1963-117">Click 'Next &gt;':</span></span>

![](aspnet-and-iis6/_static/image2.jpg)

<span data-ttu-id="f1963-118">Clique em ' Avançar &gt;'</span><span class="sxs-lookup"><span data-stu-id="f1963-118">Click 'Next &gt;'</span></span>

![](aspnet-and-iis6/_static/image3.jpg)

<span data-ttu-id="f1963-119">Nessa tela, você precisará selecionar ' servidor de aplicativos (IIS, ASP.NET) como as opções a serem configuradas.</span><span class="sxs-lookup"><span data-stu-id="f1963-119">On this screen you will need to select 'Application server (IIS, ASP.NET) as the options to configure.</span></span>

<span data-ttu-id="f1963-120">Clique em ' Avançar &gt;'.</span><span class="sxs-lookup"><span data-stu-id="f1963-120">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image4.jpg)

<span data-ttu-id="f1963-121">Depois de selecionar para configurar o servidor como um servidor de aplicativos, essa tela será exibida solicitando quais recursos adicionais devem ser instalados.</span><span class="sxs-lookup"><span data-stu-id="f1963-121">After selecting to configure the server as an Application Server, this screen will be displayed prompting what additional capabilities should be installed.</span></span> <span data-ttu-id="f1963-122">Nenhuma opção é selecionada por padrão.</span><span class="sxs-lookup"><span data-stu-id="f1963-122">Neither option is selected by default.</span></span> <span data-ttu-id="f1963-123">Para habilitar o ASP.NET automaticamente, você precisa selecionar ' habilitar ASP. NET '.</span><span class="sxs-lookup"><span data-stu-id="f1963-123">To enable ASP.NET automatically, you need to select 'Enable ASP.NET'.</span></span>

<span data-ttu-id="f1963-124">Clique em ' Avançar &gt;'.</span><span class="sxs-lookup"><span data-stu-id="f1963-124">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image5.jpg)

<span data-ttu-id="f1963-125">Esta tela exibe as opções que devem ser instaladas.</span><span class="sxs-lookup"><span data-stu-id="f1963-125">This screen displays what options are to be installed.</span></span>

<span data-ttu-id="f1963-126">Clique em ' Avançar &gt;'.</span><span class="sxs-lookup"><span data-stu-id="f1963-126">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image6.jpg)

<span data-ttu-id="f1963-127">Você verá essa tela enquanto as opções selecionadas estiverem sendo instaladas.</span><span class="sxs-lookup"><span data-stu-id="f1963-127">You will see this screen while the options you selected are being installed.</span></span> <span data-ttu-id="f1963-128">É normal ver outras caixas de diálogo exibidas à medida que os serviços estão sendo instalados.</span><span class="sxs-lookup"><span data-stu-id="f1963-128">It is normal to see other dialog boxes appear as services are being installed.</span></span> <span data-ttu-id="f1963-129">Além disso, você pode ser solicitado a fornecer o local do CD de instalação do Windows 2003 Server.</span><span class="sxs-lookup"><span data-stu-id="f1963-129">You may additionally be prompted for the location of the Windows 2003 Server installation CD.</span></span>

<span data-ttu-id="f1963-130">Clique em ' Avançar &gt;' ao concluir.</span><span class="sxs-lookup"><span data-stu-id="f1963-130">Click 'Next &gt;' when complete.</span></span>

![](aspnet-and-iis6/_static/image7.jpg)

<span data-ttu-id="f1963-131">Clique em ' Concluir '-o Windows Server 2003 agora está configurado para dar suporte ao IIS 6,0 e ao ASP.NET 1,1.</span><span class="sxs-lookup"><span data-stu-id="f1963-131">Click 'Finish' - the Windows Server 2003 is now configured to support IIS 6.0 and ASP.NET 1.1.</span></span>

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a><span data-ttu-id="f1963-132">Habilitando o IIS, opção #2-configurar manualmente o IIS e o ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f1963-132">Enabling IIS, option #2 - Manually configuring IIS and ASP.NET</span></span>

<span data-ttu-id="f1963-133">Se você não quiser usar o ' Assistente para configurar o servidor ', poderá, opcionalmente, instalar o IIS 6,0 e o ASP.NET 1,1 usando "Adicionar ou remover programas" no painel de controle.</span><span class="sxs-lookup"><span data-stu-id="f1963-133">If you do not wish to use the 'Configure Your Server Wizard' you can optionally install IIS 6.0 and ASP.NET 1.1 using 'Add or Remove Programs' from the Control Panel.</span></span>

<span data-ttu-id="f1963-134">Primeiro, abra o painel de controle:</span><span class="sxs-lookup"><span data-stu-id="f1963-134">First open the Control Panel:</span></span>

![](aspnet-and-iis6/_static/image8.jpg)

<span data-ttu-id="f1963-135">Em seguida, clique em "Adicionar/remover componentes do Windows", que abrirá o "Assistente de componentes do Windows":</span><span class="sxs-lookup"><span data-stu-id="f1963-135">Next, click on 'Add/Remove Windows Components' which will open the 'Windows Components Wizard':</span></span>

![](aspnet-and-iis6/_static/image9.jpg)

<span data-ttu-id="f1963-136">Realce e marque ' servidor de aplicativos ' e clique em ' Detalhes? '</span><span class="sxs-lookup"><span data-stu-id="f1963-136">Highlight and check 'Application Server' and then click the 'Details?'</span></span> <span data-ttu-id="f1963-137">Button</span><span class="sxs-lookup"><span data-stu-id="f1963-137">button:</span></span>

![](aspnet-and-iis6/_static/image10.jpg)

<span data-ttu-id="f1963-138">Para instalar o ASP.NET, marque ' ASP. NET '.</span><span class="sxs-lookup"><span data-stu-id="f1963-138">To install ASP.NET, check 'ASP.NET'.</span></span>

<span data-ttu-id="f1963-139">Clique em ' OK ' para retornar ao assistente de componente do Windows.</span><span class="sxs-lookup"><span data-stu-id="f1963-139">Click 'OK' to return to the Windows Component Wizard.</span></span> <span data-ttu-id="f1963-140">Clique em ' Avançar &gt;' no assistente de componentes do Windows para começar a instalar o:</span><span class="sxs-lookup"><span data-stu-id="f1963-140">Click 'Next &gt;' from the Windows Component Wizard to begin installing:</span></span>

![](aspnet-and-iis6/_static/image11.jpg)

<span data-ttu-id="f1963-141">É normal ver outras caixas de diálogo exibidas à medida que os serviços estão sendo instalados.</span><span class="sxs-lookup"><span data-stu-id="f1963-141">It is normal to see other dialog boxes appear as services are being installed.</span></span> <span data-ttu-id="f1963-142">Além disso, você pode ser solicitado a fornecer o local do CD de instalação do Windows 2003 Server.</span><span class="sxs-lookup"><span data-stu-id="f1963-142">You may additionally be prompted for the location of the Windows 2003 Server installation CD.</span></span>

<span data-ttu-id="f1963-143">Quando a instalação for concluída, você verá a última tela do assistente de componentes do Windows:</span><span class="sxs-lookup"><span data-stu-id="f1963-143">When installation is complete you will see the last screen of the Windows Component Wizard:</span></span>

![](aspnet-and-iis6/_static/image12.jpg)

<span data-ttu-id="f1963-144">O IIS 6,0 e o ASP.NET 1,1 agora estão configurados e disponíveis.</span><span class="sxs-lookup"><span data-stu-id="f1963-144">IIS 6.0 and ASP.NET 1.1 are now configured and available.</span></span>

## <a name="recommended-settings"></a><span data-ttu-id="f1963-145">Configurações recomendadas</span><span class="sxs-lookup"><span data-stu-id="f1963-145">Recommended Settings</span></span>

<span data-ttu-id="f1963-146">Ao executar o ASP.NET 1,1 com IIS 6,0, há várias definições de configuração que são recomendadas para obter o desempenho ideal do ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="f1963-146">When running ASP.NET 1.1 with IIS 6.0 there are several configuration settings that are recommended to get the optimal performance from ASP.NET:</span></span>

- <span data-ttu-id="f1963-147">Configurando limites de memória de processo de trabalho</span><span class="sxs-lookup"><span data-stu-id="f1963-147">Configuring worker process memory limits</span></span>
- <span data-ttu-id="f1963-148">Configurando a reciclagem de processo de trabalho</span><span class="sxs-lookup"><span data-stu-id="f1963-148">Configuring worker process recycling</span></span>

### <a name="configuring-worker-process-memory-limits"></a><span data-ttu-id="f1963-149">Configurando limites de memória de processo de trabalho</span><span class="sxs-lookup"><span data-stu-id="f1963-149">Configuring worker process memory limits</span></span>

<span data-ttu-id="f1963-150">Por padrão, o IIS 6,0 não define um limite na quantidade de memória que o IIS tem permissão para usar.</span><span class="sxs-lookup"><span data-stu-id="f1963-150">By default IIS 6.0 does not set a limit on the amount of memory that IIS is allowed to use.</span></span> <span data-ttu-id="f1963-151">ASP. O recurso de cache do NET depende de uma limitação de memória para que o cache possa remover proativamente itens não utilizados da memória.</span><span class="sxs-lookup"><span data-stu-id="f1963-151">ASP.NET's Cache feature relies on a limitation of memory so the Cache can proactively remove unused items from memory.</span></span>

<span data-ttu-id="f1963-152">É recomendável que você configure o recurso de reciclagem de memória do IIS 6,0.</span><span class="sxs-lookup"><span data-stu-id="f1963-152">It is recommended that you configure the memory recycling feature of IIS 6.0.</span></span> <span data-ttu-id="f1963-153">Para configurar este gerenciador de Serviços de Informações da Internet aberto (iniciar | Programas | Ferramentas administrativas | Serviços de Informações da Internet).</span><span class="sxs-lookup"><span data-stu-id="f1963-153">To configure this open Internet Information Services Manager (Start | Programs | Administrative Tools | Internet Information Services).</span></span> <span data-ttu-id="f1963-154">Depois de abrir, expanda a pasta ' pools de aplicativos ':</span><span class="sxs-lookup"><span data-stu-id="f1963-154">Once open, expand the 'Application Pools' folder:</span></span>

<span data-ttu-id="f1963-155">Para cada pool de aplicativos:</span><span class="sxs-lookup"><span data-stu-id="f1963-155">For each application pool:</span></span>

![](aspnet-and-iis6/_static/image13.jpg)

1. <span data-ttu-id="f1963-156">Clique com o botão direito do mouse no pool de aplicativos, por exemplo, ' DefaultAppPool ' e selecione ' Propriedades ':</span><span class="sxs-lookup"><span data-stu-id="f1963-156">Right-click on the application pool, e.g. 'DefaultAppPool', and select 'Properties':</span></span>

![](aspnet-and-iis6/_static/image14.jpg)

2. <span data-ttu-id="f1963-157">Em seguida, habilite a reciclagem de memória clicando em ' máximo de memória usada (em megabytes): '.</span><span class="sxs-lookup"><span data-stu-id="f1963-157">Next, enable Memory recycling by clicking on either 'Maximum used memory (in megabytes):'.</span></span> <span data-ttu-id="f1963-158">O valor não deve ser maior que a quantidade de memória física (não virtual) no servidor, uma boa aproximação é de 60% da memória física, ou seja, para um servidor com 512 MB de memória física, selecione 310.</span><span class="sxs-lookup"><span data-stu-id="f1963-158">The value should not be more than the amount of physical (not virtual) memory on the server, a good approximation is 60% of the physical memory, i.e. for a server with 512MB of physical memory select 310.</span></span> <span data-ttu-id="f1963-159">Também é recomendável que o máximo não exceda 800 MB ao usar um espaço de endereço de 2GB.</span><span class="sxs-lookup"><span data-stu-id="f1963-159">It is also recommended that the maximum not exceed 800MB when using a 2GB address space.</span></span> <span data-ttu-id="f1963-160">Se o espaço de endereço de memória do servidor for 3GB, o limite máximo de memória para o processo de trabalho pode ser tão alto quanto 1, 800 MB:</span><span class="sxs-lookup"><span data-stu-id="f1963-160">If the memory address space of the server is 3GB, the maximum memory limit for the worker process can be as high as 1,800MB:</span></span>

![](aspnet-and-iis6/_static/image15.jpg)

<span data-ttu-id="f1963-161">Clique em ' Aplicar ' e em ' OK ' para sair da caixa de diálogo de propriedades.</span><span class="sxs-lookup"><span data-stu-id="f1963-161">Click 'Apply' and the 'OK' to exit the properties dialog.</span></span> <span data-ttu-id="f1963-162">Repita isso para todos os pools de aplicativos disponíveis.</span><span class="sxs-lookup"><span data-stu-id="f1963-162">Repeat this for all available application pools.</span></span>

### <a name="configuring-worker-recycling"></a><span data-ttu-id="f1963-163">Configurando a reciclagem de trabalho</span><span class="sxs-lookup"><span data-stu-id="f1963-163">Configuring worker recycling</span></span>

<span data-ttu-id="f1963-164">Por padrão, o IIS 6,0 é configurado para reciclar seu processo de trabalho a cada 29 horas.</span><span class="sxs-lookup"><span data-stu-id="f1963-164">By default IIS 6.0 is configured to recycle its worker process every 29 hours.</span></span> <span data-ttu-id="f1963-165">Isso é um pouco agressivo para um aplicativo que executa o ASP.NET e é recomendável que a reciclagem automática do processo de trabalho esteja desabilitada.</span><span class="sxs-lookup"><span data-stu-id="f1963-165">This is a bit aggressive for an application running ASP.NET and it is recommended that automatic worker process recycling is disabled.</span></span>

<span data-ttu-id="f1963-166">Para desabilitar a reciclagem automática do processo de trabalho, primeiro abra o Gerenciador de Serviços de Informações da Internet (iniciar | Programas | Ferramentas administrativas | Serviços de Informações da Internet).</span><span class="sxs-lookup"><span data-stu-id="f1963-166">To disable automatic worker process recycling, first open Internet Information Services Manager (Start | Programs | Administrative Tools | Internet Information Services).</span></span> <span data-ttu-id="f1963-167">Depois de abrir, expanda a pasta ' pools de aplicativos ':</span><span class="sxs-lookup"><span data-stu-id="f1963-167">Once open, expand the 'Application Pools' folder:</span></span>

![](aspnet-and-iis6/_static/image16.jpg)

<span data-ttu-id="f1963-168">Para cada pool de aplicativos:</span><span class="sxs-lookup"><span data-stu-id="f1963-168">For each application pool:</span></span>

1. <span data-ttu-id="f1963-169">Clique com o botão direito do mouse no pool de aplicativos, por exemplo, ' DefaultAppPool ' e selecione ' Propriedades ':</span><span class="sxs-lookup"><span data-stu-id="f1963-169">Right-click on the application pool, e.g. 'DefaultAppPool', and select 'Properties':</span></span>

![](aspnet-and-iis6/_static/image17.jpg)

2. <span data-ttu-id="f1963-170">Desmarque ' reciclar processo de trabalho (em minutos): ':</span><span class="sxs-lookup"><span data-stu-id="f1963-170">Uncheck 'Recycle worker process (in minutes):':</span></span>

![](aspnet-and-iis6/_static/image18.jpg)

<span data-ttu-id="f1963-171">Clique em ' Aplicar ' e em ' OK ' para sair da caixa de diálogo de propriedades.</span><span class="sxs-lookup"><span data-stu-id="f1963-171">Click 'Apply' and the 'OK' to exit the properties dialog.</span></span> <span data-ttu-id="f1963-172">Repita isso para todos os pools de aplicativos disponíveis.</span><span class="sxs-lookup"><span data-stu-id="f1963-172">Repeat this for all available application pools.</span></span>

## <a name="granting-write-access-to-the-file-system"></a><span data-ttu-id="f1963-173">Concedendo acesso de gravação ao sistema de arquivos</span><span class="sxs-lookup"><span data-stu-id="f1963-173">Granting write access to the file system</span></span>

<span data-ttu-id="f1963-174">Se seu aplicativo exigir acesso de gravação ao sistema de arquivos e você estiver usando NTFS, precisará modificar uma ACL (lista de controle de acesso) na pasta ou no arquivo para conceder acesso ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f1963-174">If your application requires write access to the file system and you are using NTFS you will need to modify an Access Control List (ACL) on the folder or file to grant ASP.NET access to.</span></span>

<span data-ttu-id="f1963-175">Por exemplo, para conceder acesso de gravação ASP.NET ao c:\inetpub\wwwroot First Open Explorer e navegue até o diretório:</span><span class="sxs-lookup"><span data-stu-id="f1963-175">For example, to grant ASP.NET write access to the c:\inetpub\wwwroot first open explorer and navigate to the directory:</span></span>

![](aspnet-and-iis6/_static/image19.jpg)

<span data-ttu-id="f1963-176">Em seguida, clique com o botão direito do mouse no diretório, por exemplo, ' wwwroot ' e selecione Propriedades.</span><span class="sxs-lookup"><span data-stu-id="f1963-176">Next, right-click on the directory, e.g. 'wwwroot' and select properties.</span></span> <span data-ttu-id="f1963-177">Depois que a caixa de diálogo Propriedades for aberta, selecione a guia "segurança":</span><span class="sxs-lookup"><span data-stu-id="f1963-177">After the properties dialog opens, select the 'Security' tab:</span></span>

![](aspnet-and-iis6/_static/image20.jpg)

<span data-ttu-id="f1963-178">O diretório c:\inetpub\wwwroot\ é um diretório especial no qual o grupo especial do IIS 6,0 ' IIS\_WPG ' já foi concedido a leitura &amp; execução, listar conteúdo da pasta e permissões de leitura.</span><span class="sxs-lookup"><span data-stu-id="f1963-178">The c:\inetpub\wwwroot\ directory is a special directory in that the special IIS 6.0 group 'IIS\_WPG' is already granted Read &amp; Execute, List Folder Contents, and Read permissions.</span></span> <span data-ttu-id="f1963-179">No entanto, para conceder permissão de gravação, você precisa clicar na caixa de seleção Permitir para gravação:</span><span class="sxs-lookup"><span data-stu-id="f1963-179">However, to grant Write permission, you need to click the Allow checkbox for Write:</span></span>

![](aspnet-and-iis6/_static/image21.jpg)

<span data-ttu-id="f1963-180">O IIS 6,0 agora tem permissão de gravação nessa pasta.</span><span class="sxs-lookup"><span data-stu-id="f1963-180">IIS 6.0 now has write permission on this folder.</span></span> <span data-ttu-id="f1963-181">Para conceder permissões de gravação em outras pastas, siga estas etapas-Observe que talvez seja necessário adicionar o grupo do IIS\_WPG se ele ainda não existir.</span><span class="sxs-lookup"><span data-stu-id="f1963-181">To grant write permissions on other folders, follow these steps - note, you may need to add the IIS\_WPG group if it does not already exist.</span></span>

> [!CAUTION]
> <span data-ttu-id="f1963-182">Conceder permissão de gravação ao IIS\_WPG permitirá que qualquer aplicativo ASP.NET grave nesse diretório.</span><span class="sxs-lookup"><span data-stu-id="f1963-182">Granting write permission to IIS\_WPG will allow any ASP.NET application to write to this directory.</span></span>

## <a name="supporting-integrated-authentication-with-sql-server"></a><span data-ttu-id="f1963-183">Suporte à autenticação integrada com o SQL Server</span><span class="sxs-lookup"><span data-stu-id="f1963-183">Supporting integrated authentication with SQL Server</span></span>

<span data-ttu-id="f1963-184">A autenticação integrada permite que o SQL Server Aproveite a autenticação do Windows NT para validar SQL Server contas de logon.</span><span class="sxs-lookup"><span data-stu-id="f1963-184">Integrated authentication allows for SQL Server to leverage Windows NT authentication to validate SQL Server logon accounts.</span></span> <span data-ttu-id="f1963-185">Isso permite que o usuário ignore o processo de logon SQL Server padrão.</span><span class="sxs-lookup"><span data-stu-id="f1963-185">This allows the user to bypass the standard SQL Server logon process.</span></span> <span data-ttu-id="f1963-186">Com essa abordagem, um usuário de rede pode acessar um banco de dados SQL Server sem fornecer uma identificação de logon ou senha separada, pois SQL Server obtém as informações de usuário e senha do processo de segurança de rede do Windows NT.</span><span class="sxs-lookup"><span data-stu-id="f1963-186">With this approach, a network user can access a SQL Server database without supplying a separate logon identification or password because SQL Server obtains the user and password information from the Windows NT network security process.</span></span>

<span data-ttu-id="f1963-187">Escolher a autenticação integrada para aplicativos ASP.NET é uma boa opção porque nenhuma credencial nunca é armazenada em sua cadeia de conexão para seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f1963-187">Choosing integrated authentication for ASP.NET applications is a good choice because no credentials are ever stored within your connection string for your application.</span></span> <span data-ttu-id="f1963-188">Em vez disso, a cadeia de conexão usada para se conectar ao SQL terá a seguinte aparência:</span><span class="sxs-lookup"><span data-stu-id="f1963-188">Rather the connection string used to connect to SQL will look as follows:</span></span>

`"server=localhost; database=Northwind;Trusted_Connection=true"`

<span data-ttu-id="f1963-189">Essa cadeia de conexão informa SQL Server usar as credenciais do Windows do aplicativo tentando acessar o SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f1963-189">This connection string tells SQL Server to use the Windows credentials of the application attempting to access SQL Server.</span></span> <span data-ttu-id="f1963-190">No caso do ASP.NET/IIS 6, isso seria uma conta no grupo do IIS\_WPG.</span><span class="sxs-lookup"><span data-stu-id="f1963-190">In the case of ASP.NET/IIS 6 this would be an account in the IIS\_WPG group.</span></span>

<span data-ttu-id="f1963-191">Para habilitar a autenticação integrada entre o SQL Server e o ASP.NET, você precisará primeiro garantir que SQL Server esteja configurado para autenticação integrada ou autenticação de modo misto-Verifique com seu DBA para determinar isso.</span><span class="sxs-lookup"><span data-stu-id="f1963-191">To enable integrated authentication between SQL Server and ASP.NET, you will need to first ensure that SQL Server is configured for either Integrated authentication or Mixed-Mode authentication - check with your DBA to determine this.</span></span> <span data-ttu-id="f1963-192">Se SQL Server estiver em um desses dois modos, você poderá usar a autenticação integrada.</span><span class="sxs-lookup"><span data-stu-id="f1963-192">If SQL Server is in one of these two modes, you can use integrated authentication.</span></span>

<span data-ttu-id="f1963-193">Abrir o Gerenciador de SQL Server Enterprise (iniciar | Programas | Microsoft SQL Server | Enterprise Manager), selecione o servidor apropriado e expanda a pasta segurança:</span><span class="sxs-lookup"><span data-stu-id="f1963-193">Open SQL Server Enterprise Manager (Start | Programs | Microsoft SQL Server | Enterprise Manager), select the appropriate server, and expand the Security folder:</span></span>

![](aspnet-and-iis6/_static/image22.jpg)

<span data-ttu-id="f1963-194">Se o grupo ' BUILTINT\IIS\_WPG ' não estiver listado, clique com o botão direito do mouse em logons e selecione ' New login:</span><span class="sxs-lookup"><span data-stu-id="f1963-194">If 'BUILTINT\IIS\_WPG' group is not listed, right-click on Logins and select 'New Login':</span></span>

![](aspnet-and-iis6/_static/image23.jpg)

<span data-ttu-id="f1963-195">Na caixa de texto ' nome: ', insira ' [nome do servidor/domínio] \IIS\_WPG ' ou clique no botão de reticências para abrir o seletor de usuário/grupo do Windows NT:</span><span class="sxs-lookup"><span data-stu-id="f1963-195">In the 'Name:' textbox either enter '[Server/Domain Name]\IIS\_WPG' or click on the ellipses button to open the Windows NT user/group picker:</span></span>

![](aspnet-and-iis6/_static/image24.jpg)

<span data-ttu-id="f1963-196">Selecione o grupo IIS\_WPG do computador atual e clique em ' Adicionar ' e em OK para fechar o seletor.</span><span class="sxs-lookup"><span data-stu-id="f1963-196">Select the current machine's IIS\_WPG group and click 'Add' and OK to close the picker.</span></span>

<span data-ttu-id="f1963-197">Você também precisa definir o banco de dados padrão e as permissões para acessar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f1963-197">You then need to also set the default database and the permissions to access the database.</span></span> <span data-ttu-id="f1963-198">Para definir o banco de dados padrão, escolha na lista suspensa, por exemplo, abaixo de Northwind está selecionado:</span><span class="sxs-lookup"><span data-stu-id="f1963-198">To set the default database choose from the drop down list, e.g. below Northwind is selected:</span></span>

![](aspnet-and-iis6/_static/image25.jpg)

<span data-ttu-id="f1963-199">Em seguida, clique na guia acesso ao banco de dados:</span><span class="sxs-lookup"><span data-stu-id="f1963-199">Next, click on the Database Access tab:</span></span>

![](aspnet-and-iis6/_static/image26.jpg)

<span data-ttu-id="f1963-200">Clique na caixa de seleção Permitir para cada banco de dados ao qual você deseja permitir acesso.</span><span class="sxs-lookup"><span data-stu-id="f1963-200">Click on the Permit checkbox for every database that you wish to allow access to.</span></span> <span data-ttu-id="f1963-201">Você também precisará selecionar funções de banco de dados, verificar o proprietário do\_do BD garantirá que o logon tenha todas as permissões necessárias para gerenciar e usar o banco de dados selecionado.</span><span class="sxs-lookup"><span data-stu-id="f1963-201">You will also need to select database roles, checking db\_owner will ensure your login has all necessary permissions to manage and use the selected database.</span></span>

<span data-ttu-id="f1963-202">Clique em OK para sair da caixa de diálogo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="f1963-202">Click OK to exit the property dialog.</span></span> <span data-ttu-id="f1963-203">Seu aplicativo ASP.NET agora está configurado para dar suporte à autenticação de SQL Server integrada.</span><span class="sxs-lookup"><span data-stu-id="f1963-203">Your ASP.NET application is now configured to support integrated SQL Server authentication.</span></span>

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a><span data-ttu-id="f1963-204">Não execute o ASP.NET 1,0 no modo nativo do IIS 6,0</span><span class="sxs-lookup"><span data-stu-id="f1963-204">Don't run ASP.NET 1.0 in IIS 6.0 native mode</span></span>

<span data-ttu-id="f1963-205">O ASP.NET 1,0 no IIS 6,0 só tem suporte no modo de compatibilidade do IIS 5.</span><span class="sxs-lookup"><span data-stu-id="f1963-205">ASP.NET 1.0 on IIS 6.0 is only supported in IIS 5 compatibility mode.</span></span>

<span data-ttu-id="f1963-206">Para configurar o ASP.NET 1,0 para ser executado no modo de compatibilidade do IIS 5,0, abra Gerenciador de Serviços de Internet e clique com o botão direito do mouse em sites e selecione Propriedades:</span><span class="sxs-lookup"><span data-stu-id="f1963-206">To configure ASP.NET 1.0 to run in IIS 5.0 compatibility mode, open Internet Services Manager and right click Web Sites and select properties:</span></span>

![](aspnet-and-iis6/_static/image27.jpg)

<span data-ttu-id="f1963-207">Alternar para a guia de serviço e verificar? Executar o serviço WWW no modo de isolamento do IIS 5,0?:</span><span class="sxs-lookup"><span data-stu-id="f1963-207">Switch to the Service Tab and check ?Run WWW Service in IIS 5.0 Isolation Mode?:</span></span>

![](aspnet-and-iis6/_static/image28.jpg)
