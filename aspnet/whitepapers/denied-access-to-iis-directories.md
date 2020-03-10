---
uid: whitepapers/denied-access-to-iis-directories
title: ASP.NET negou acesso aos diretórios do IIS | Microsoft Docs
author: rick-anderson
description: Este white paper descreve o que você deve fazer se uma solicitação ao aplicativo ASP.NET retornar o erro "acesso negado ao diretório DirectoryName. Falha em s...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: a3a53aa88abbe1bcaaea7d691406800c8f9b988b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638498"
---
# <a name="aspnet-denied-access-to-iis-directories"></a><span data-ttu-id="ae99b-104">Acesso negado do ASP.NET a diretórios do IIS</span><span class="sxs-lookup"><span data-stu-id="ae99b-104">ASP.NET Denied Access to IIS Directories</span></span>

> <span data-ttu-id="ae99b-105">Este white paper descreve o que você deve fazer se uma solicitação ao aplicativo ASP.NET retornar o erro "acesso negado ao diretório *DirectoryName* .</span><span class="sxs-lookup"><span data-stu-id="ae99b-105">This whitepaper describes what you must do if a request to your ASP.NET application returns the error, "Denied Access to *DirectoryName* directory.</span></span> <span data-ttu-id="ae99b-106">Falha ao iniciar o monitoramento de alterações de diretório. "</span><span class="sxs-lookup"><span data-stu-id="ae99b-106">Failed to start monitoring directory changes."</span></span>
> 
> <span data-ttu-id="ae99b-107">Aplica-se a ASP.NET 1,0 e ASP.NET 1,1.</span><span class="sxs-lookup"><span data-stu-id="ae99b-107">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>

<span data-ttu-id="ae99b-108">O ASP.NET v1 RTM agora é executado usando uma conta do Windows menos privilegiada – registrada como a conta "ASPNET" em um computador local.</span><span class="sxs-lookup"><span data-stu-id="ae99b-108">ASP.NET V1 RTM now runs using a less privileged windows account - registered as the "ASPNET" account on a local machine.</span></span>

<span data-ttu-id="ae99b-109">Em alguns sistemas bloqueados, essa conta pode não, por padrão, ter acesso de segurança de leitura aos diretórios de conteúdo de um site, ao diretório raiz do aplicativo ou ao diretório raiz do site.</span><span class="sxs-lookup"><span data-stu-id="ae99b-109">On some locked down systems, this account may not by default have read security access to a website's content directories, the application root directory, or the web site root directory.</span></span> <span data-ttu-id="ae99b-110">Nesse caso, você receberá o seguinte erro ao solicitar páginas de um determinado aplicativo Web:</span><span class="sxs-lookup"><span data-stu-id="ae99b-110">In this case you will receive the following error when requesting pages from a given web application:</span></span>

![](denied-access-to-iis-directories/_static/image1.jpg)

<span data-ttu-id="ae99b-111">Para corrigir isso, você precisará alterar as permissões de segurança nos diretórios apropriados.</span><span class="sxs-lookup"><span data-stu-id="ae99b-111">To fix this you will need to change the security permissions on the appropriate directories.</span></span>

<span data-ttu-id="ae99b-112">Especificamente, o ASP.NET requer acesso de leitura, execução e lista para a conta ASPNET para a raiz do site (por exemplo: c:\inetpub\wwwroot ou qualquer diretório de site alternativo que você possa ter configurado no IIS), o diretório de conteúdo e o diretório raiz do aplicativo para monitorar as alterações do arquivo de configuração.</span><span class="sxs-lookup"><span data-stu-id="ae99b-112">Specifically, ASP.NET requires read, execute, and list access for the ASPNET account for the web site root (for example: c:\inetpub\wwwroot or any alternative site directory you may have configured in IIS), the content directory and the application root directory in order to monitor for configuration file changes.</span></span> <span data-ttu-id="ae99b-113">A raiz do aplicativo corresponde ao caminho da pasta associado ao diretório virtual do aplicativo na ferramenta de administração do IIS (inetmgr).</span><span class="sxs-lookup"><span data-stu-id="ae99b-113">The application root corresponds to the folder path associated with the application virtual directory in the IIS Administration tool (inetmgr).</span></span>

<span data-ttu-id="ae99b-114">Por exemplo, considere a seguinte hierarquia de aplicativo na pasta wwwroot.</span><span class="sxs-lookup"><span data-stu-id="ae99b-114">For example, consider the following application hierarchy under the wwwroot folder.</span></span>

`C:\inetpub\wwwroot\myapp\default.aspx`

<span data-ttu-id="ae99b-115">Para este exemplo, a conta ASPNET precisa das permissões de leitura definidas acima para o conteúdo no diretório MyApp e wwwroot.</span><span class="sxs-lookup"><span data-stu-id="ae99b-115">For this example, the ASPNET account needs the read permissions defined above for content in both the myapp and the wwwroot directory.</span></span> <span data-ttu-id="ae99b-116">Uma única ACL herdada na pasta raiz também pode ser usada opcionalmente para ambos os diretórios se eles estiverem aninhados.</span><span class="sxs-lookup"><span data-stu-id="ae99b-116">A single inherited ACL on the root folder can also be optionally used for both directories if they're nested.</span></span>

<span data-ttu-id="ae99b-117">Para adicionar permissões a um diretório, execute as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="ae99b-117">To add permissions to a directory, perform the following steps:</span></span>

- <span data-ttu-id="ae99b-118">Usando o Windows Explorer, navegue até o diretório</span><span class="sxs-lookup"><span data-stu-id="ae99b-118">Using the Windows explorer, navigate to the directory</span></span>
- <span data-ttu-id="ae99b-119">Clique com o botão direito do mouse na pasta do diretório e escolha "Propriedades"</span><span class="sxs-lookup"><span data-stu-id="ae99b-119">Right click on the directory folder and choose "Properties"</span></span>
- <span data-ttu-id="ae99b-120">Navegue até a guia "segurança" na caixa de diálogo de propriedade</span><span class="sxs-lookup"><span data-stu-id="ae99b-120">Navigate to the "Security" tab on the property dialog</span></span>
- <span data-ttu-id="ae99b-121">Clique no botão "Adicionar" e insira o nome do computador seguido pelo nome da conta ASPNET.</span><span class="sxs-lookup"><span data-stu-id="ae99b-121">Click the "Add" button and enter the machine name followed by the ASPNET account name.</span></span> <span data-ttu-id="ae99b-122">Por exemplo, em um computador chamado "webdev", você deve digitar webdev\ASPNET e clicar em "OK".</span><span class="sxs-lookup"><span data-stu-id="ae99b-122">For example, on a machine named "webdev", you would enter webdev\ASPNET and hit "OK".</span></span>
- <span data-ttu-id="ae99b-123">Verifique se a conta ASPNET tem as caixas de seleção "ler &amp; executar", "listar conteúdo da pasta" e "ler" marcada.</span><span class="sxs-lookup"><span data-stu-id="ae99b-123">Ensure that the ASPNET account has the "Read &amp; Execute", "List Folder Contents", and "Read" checkboxes checked.</span></span>
- <span data-ttu-id="ae99b-124">Pressione OK para ignorar a caixa de diálogo e salvar as alterações.</span><span class="sxs-lookup"><span data-stu-id="ae99b-124">Hit OK to dismiss the dialog and save the changes.</span></span>

![](denied-access-to-iis-directories/_static/image2.jpg)

<span data-ttu-id="ae99b-125">Se desejar, essas alterações podem ser automatizadas usando scripts ou a ferramenta "Cacls. exe" fornecida com o Windows.</span><span class="sxs-lookup"><span data-stu-id="ae99b-125">If desired, these changes can be automated using scripts or the "cacls.exe" tool that ships with Windows.</span></span> <span data-ttu-id="ae99b-126">Para obter mais informações sobre a conta ASPNET, consulte o [documento de perguntas frequentes](https://go.microsoft.com/fwlink/?LinkId=5828).</span><span class="sxs-lookup"><span data-stu-id="ae99b-126">For more information on the ASPNET account, please see the [FAQ document](https://go.microsoft.com/fwlink/?LinkId=5828).</span></span>

<span data-ttu-id="ae99b-127">Se um determinado aplicativo Web depender de permissões de gravação ou modificação em uma pasta ou arquivo específico, isso poderá ser concedido seguindo o mesmo procedimento e marcando as caixas de seleção "gravar" e/ou "Modificar".</span><span class="sxs-lookup"><span data-stu-id="ae99b-127">If a given web application relies on having write or modify permissions to a particular folder or file, this can be granted by following the same procedure and checking the "Write" and/or "Modify" checkboxes.</span></span>

<span data-ttu-id="ae99b-128">Em computadores que permitem que todos ou o grupo usuários tenham acesso de leitura a esses diretórios (que é a configuração padrão), nenhum problema será encontrado e as etapas acima não serão necessárias.</span><span class="sxs-lookup"><span data-stu-id="ae99b-128">On machines that allow Everyone or the Users group read access to these directories (which is the default configuration), no issues will be encountered and the above steps will not be required.</span></span>
