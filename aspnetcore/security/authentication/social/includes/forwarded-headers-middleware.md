---
ms.openlocfilehash: d7ef9b11af8ee11e4ec84404f8cdeb0b89384a3c
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57130444"
---
## <a name="forward-request-information-with-a-proxy-or-load-balancer"></a>Solicitar informações com um proxy para frente ou balanceador de carga

Se o aplicativo for implantado atrás de um servidor proxy ou um balanceador de carga, algumas das informações de solicitação original podem ser encaminhadas para o aplicativo nos cabeçalhos de solicitação. Essas informações geralmente incluem o esquema de solicitação de seguro (`https`), host e endereço IP do cliente. Aplicativos não lidas automaticamente esses cabeçalhos de solicitação para descobrir e usar as informações da solicitação original.

O esquema é usado na geração de link que afeta o fluxo de autenticação com provedores externos. Perder o esquema de seguro (`https`) resulta no aplicativo de geração de URLs de redirecionamento inseguro incorreto.

Use o Middleware de cabeçalhos encaminhados para disponibilizar as informações da solicitação original para o aplicativo para o processamento da solicitação.

Para obter mais informações, consulte <xref:host-and-deploy/proxy-load-balancer>.
