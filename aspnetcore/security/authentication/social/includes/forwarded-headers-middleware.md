---
ms.openlocfilehash: d7ef9b11af8ee11e4ec84404f8cdeb0b89384a3c
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57130444"
---
## <a name="forward-request-information-with-a-proxy-or-load-balancer"></a><span data-ttu-id="b8354-101">Solicitar informações com um proxy para frente ou balanceador de carga</span><span class="sxs-lookup"><span data-stu-id="b8354-101">Forward request information with a proxy or load balancer</span></span>

<span data-ttu-id="b8354-102">Se o aplicativo for implantado atrás de um servidor proxy ou um balanceador de carga, algumas das informações de solicitação original podem ser encaminhadas para o aplicativo nos cabeçalhos de solicitação.</span><span class="sxs-lookup"><span data-stu-id="b8354-102">If the app is deployed behind a proxy server or load balancer, some of the original request information might be forwarded to the app in request headers.</span></span> <span data-ttu-id="b8354-103">Essas informações geralmente incluem o esquema de solicitação de seguro (`https`), host e endereço IP do cliente.</span><span class="sxs-lookup"><span data-stu-id="b8354-103">This information usually includes the secure request scheme (`https`), host, and client IP address.</span></span> <span data-ttu-id="b8354-104">Aplicativos não lidas automaticamente esses cabeçalhos de solicitação para descobrir e usar as informações da solicitação original.</span><span class="sxs-lookup"><span data-stu-id="b8354-104">Apps don't automatically read these request headers to discover and use the original request information.</span></span>

<span data-ttu-id="b8354-105">O esquema é usado na geração de link que afeta o fluxo de autenticação com provedores externos.</span><span class="sxs-lookup"><span data-stu-id="b8354-105">The scheme is used in link generation that affects the authentication flow with external providers.</span></span> <span data-ttu-id="b8354-106">Perder o esquema de seguro (`https`) resulta no aplicativo de geração de URLs de redirecionamento inseguro incorreto.</span><span class="sxs-lookup"><span data-stu-id="b8354-106">Losing the secure scheme (`https`) results in the app generating incorrect insecure redirect URLs.</span></span>

<span data-ttu-id="b8354-107">Use o Middleware de cabeçalhos encaminhados para disponibilizar as informações da solicitação original para o aplicativo para o processamento da solicitação.</span><span class="sxs-lookup"><span data-stu-id="b8354-107">Use Forwarded Headers Middleware to make the original request information available to the app for request processing.</span></span>

<span data-ttu-id="b8354-108">Para obter mais informações, consulte <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="b8354-108">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>
