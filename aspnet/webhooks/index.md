---
uid: webhooks/index
title: Visão geral de WebHooks do ASP.NET | Microsoft Docs
author: rick-anderson
description: Uma introdução aos WebHooks do ASP.NET.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5e2843f0-f499-448f-a712-33d4e9858321
ms.openlocfilehash: 1e21c92e950893c0ff87c63f03f4710a158441fd
ms.sourcegitcommit: e365196c75ce93cd8967412b1cfdc27121816110
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77075080"
---
# <a name="aspnet-webhooks-overview"></a>Visão geral de WebHooks do ASP.NET

WebHooks é um padrão HTTP leve que fornece um modelo pub/sub simples para ligar APIs Web e serviços SaaS. Quando um evento acontece em um serviço, uma notificação é enviada na forma de uma solicitação HTTP POST para assinantes registrados. A solicitação POST contém informações sobre o evento que possibilitam que o destinatário aja de forma adequada.

Devido à sua simplicidade, os WebHooks já estão expostos por um grande número de serviços, incluindo [Dropbox](http://dropbox.com/), [GitHub](https://www.github.com/), [bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [paypal](http://www.paypal.com/), [margem de atraso](http://www.slack.com), [distribuição](http://www.stripe.com), [Trello](http://www.trello.com/)e muito mais. Por exemplo, um webhook pode indicar que um arquivo foi alterado no [Dropbox](http://dropbox.com/), ou uma alteração de código foi confirmada no GitHub, ou um pagamento foi iniciado no [paypal](http://www.paypal.com/)ou um cartão foi criado no [Trello](http://www.trello.com/). As possibilidades são intermináveis!

Microsoft ASP.NET WebHooks facilita o envio e o recebimento de WebHooks como parte do aplicativo ASP.NET:

* No lado do recebimento, ele fornece um modelo comum para receber e processar WebHooks de qualquer número de provedores de webhook. Ele é fornecido com suporte para [Dropbox](http://dropbox.com/), [GitHub](https://www.github.com/), [bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [paypal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [margem de atraso](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) e [zendesk](https://www.zendesk.com/) , mas é fácil adicionar suporte para mais.

* No lado de envio, ele fornece suporte para gerenciar e armazenar assinaturas, bem como para enviar notificações de eventos para o conjunto correto de assinantes. Isso permite que você defina seu próprio conjunto de eventos que os assinantes podem assinar e notificá-los quando as coisas acontecem.

As duas partes podem ser usadas juntas ou separadas, dependendo do seu cenário. Se você só precisa receber WebHooks de outros serviços, você pode usar apenas a parte receptora; Se você quiser apenas expor WebHooks para que outras pessoas consumam, você pode fazer exatamente isso.

O código tem como alvo ASP.NET Web API 2 e ASP.NET MVC 5 e está disponível como [OSS no GitHub](https://github.com/aspnet/WebHooks).

## <a name="webhooks-overview"></a>Visão geral de WebHooks

WebHooks é um padrão que significa que ele varia como ele é usado do serviço para o serviço, mas a ideia básica é a mesma. Você pode considerar os WebHooks como um modelo pub/sub simples, onde um usuário pode assinar eventos que ocorrem em outro lugar. As notificações de eventos são propagadas como solicitações HTTP POST que contêm informações sobre o próprio evento.

Normalmente, a solicitação HTTP POST contém um objeto JSON ou dados de formulário HTML determinados pelo remetente do webhook, incluindo informações sobre o evento que está fazendo com que o webhook seja disparado. Por exemplo, um corpo de solicitação de POSTAgem de webhook do [GitHub](https://www.github.com/) tem esta aparência como resultado de uma nova questão ser aberta em um repositório específico:

```json
{
  "action": "opened",
  "issue": {
      "url": "https://api.github.com/repos/octocat/Hello-World/issues/1347",
      "number": 1347,
      ...
  },
  "repository": {
      "id": 1296269,
      "full_name": "octocat/Hello-World",
      "owner": {
          "login": "octocat",
          "id": 1
          ...
      },
      ...
  },
  "sender": {
      "login": "octocat",
      "id": 1,
      ...
  }
}
```

Para garantir que o webhook seja realmente do remetente pretendido, a solicitação POST é protegida de alguma forma e, em seguida, verificada pelo receptor. Por exemplo, os [WebHooks do GitHub](https://developer.github.com/webhooks/) incluem um cabeçalho http *X-Hub-Signature* com um hash do corpo da solicitação que é verificado pela implementação do receptor para que você não precise se preocupar com ele.

O fluxo de webhook geralmente é algo assim:

* O remetente do webhook expõe eventos que um cliente pode assinar. Os eventos descrevem alterações observáveis no sistema, por exemplo, que um novo item de dados foi inserido, que um processo foi concluído ou algo mais.

* O receptor de webhook assina registrando um webhook que consiste em quatro coisas:

     1. Um URI para onde a notificação de eventos deve ser postada na forma de uma solicitação HTTP POST;

     2. Um conjunto de filtros que descreve os eventos específicos para os quais o webhook deve ser acionado;

     3. Uma chave secreta que é usada para assinar a solicitação HTTP POST;

     4. Dados adicionais que serão incluídos na solicitação HTTP POST. Isso pode, por exemplo, ser campos de cabeçalho HTTP adicionais ou Propriedades incluídas no corpo da solicitação HTTP POST.

* Quando um evento acontece, os registros de webhook correspondentes são encontrados e as solicitações HTTP POST são enviadas. Normalmente, a geração das solicitações HTTP POST é repetida várias vezes se, por algum motivo, o destinatário não estiver respondendo ou a solicitação HTTP POST resultar em uma resposta de erro.

## <a name="webhooks-processing-pipeline"></a>Pipeline de processamento de WebHooks

O pipeline de processamento de WebHooks Microsoft ASP.NET para WebHooks de entrada é semelhante ao seguinte:

![Pipeline de processamento de WebHooks do ASP.NET](_static/WebHookReceivers.png)

Os dois conceitos principais aqui são *receptores* e *manipuladores*:

* Os *receptores* são responsáveis por lidar com o tipo específico de webhook de um determinado remetente e para impor verificações de segurança para garantir que a solicitação de webhook realmente seja do remetente pretendido.

* Os *manipuladores* são normalmente onde o código do usuário executa o processamento do webhook específico.

Nos nós a seguir, esses conceitos são descritos em mais detalhes.
