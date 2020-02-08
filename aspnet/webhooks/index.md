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
# <a name="aspnet-webhooks-overview"></a><span data-ttu-id="6210e-103">Visão geral de WebHooks do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="6210e-103">ASP.NET WebHooks overview</span></span>

<span data-ttu-id="6210e-104">WebHooks é um padrão HTTP leve que fornece um modelo pub/sub simples para ligar APIs Web e serviços SaaS.</span><span class="sxs-lookup"><span data-stu-id="6210e-104">WebHooks is a lightweight HTTP pattern providing a simple pub/sub model for wiring together Web APIs and SaaS services.</span></span> <span data-ttu-id="6210e-105">Quando um evento acontece em um serviço, uma notificação é enviada na forma de uma solicitação HTTP POST para assinantes registrados.</span><span class="sxs-lookup"><span data-stu-id="6210e-105">When an event happens in a service, a notification is sent in the form of an HTTP POST request to registered subscribers.</span></span> <span data-ttu-id="6210e-106">A solicitação POST contém informações sobre o evento que possibilitam que o destinatário aja de forma adequada.</span><span class="sxs-lookup"><span data-stu-id="6210e-106">The POST request contains information about the event which makes it possible for the receiver to act accordingly.</span></span>

<span data-ttu-id="6210e-107">Devido à sua simplicidade, os WebHooks já estão expostos por um grande número de serviços, incluindo [Dropbox](http://dropbox.com/), [GitHub](https://www.github.com/), [bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [paypal](http://www.paypal.com/), [margem de atraso](http://www.slack.com), [distribuição](http://www.stripe.com), [Trello](http://www.trello.com/)e muito mais.</span><span class="sxs-lookup"><span data-stu-id="6210e-107">Because of their simplicity, WebHooks are already exposed by a large number of services including [Dropbox](http://dropbox.com/), [GitHub](https://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/), and many more.</span></span> <span data-ttu-id="6210e-108">Por exemplo, um webhook pode indicar que um arquivo foi alterado no [Dropbox](http://dropbox.com/), ou uma alteração de código foi confirmada no GitHub, ou um pagamento foi iniciado no [paypal](http://www.paypal.com/)ou um cartão foi criado no [Trello](http://www.trello.com/).</span><span class="sxs-lookup"><span data-stu-id="6210e-108">For example, a WebHook can indicate that a file has changed in [Dropbox](http://dropbox.com/), or a code change has been committed in GitHub, or a payment has been initiated in [PayPal](http://www.paypal.com/), or a card has been created in [Trello](http://www.trello.com/).</span></span> <span data-ttu-id="6210e-109">As possibilidades são intermináveis!</span><span class="sxs-lookup"><span data-stu-id="6210e-109">The possibilities are endless!</span></span>

<span data-ttu-id="6210e-110">Microsoft ASP.NET WebHooks facilita o envio e o recebimento de WebHooks como parte do aplicativo ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="6210e-110">Microsoft ASP.NET WebHooks makes it easier to both send and receive WebHooks as part of your ASP.NET application:</span></span>

* <span data-ttu-id="6210e-111">No lado do recebimento, ele fornece um modelo comum para receber e processar WebHooks de qualquer número de provedores de webhook.</span><span class="sxs-lookup"><span data-stu-id="6210e-111">On the receiving side, it provides a common model for receiving and processing WebHooks from any number of WebHook providers.</span></span> <span data-ttu-id="6210e-112">Ele é fornecido com suporte para [Dropbox](http://dropbox.com/), [GitHub](https://www.github.com/), [bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [paypal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [margem de atraso](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) e [zendesk](https://www.zendesk.com/) , mas é fácil adicionar suporte para mais.</span><span class="sxs-lookup"><span data-stu-id="6210e-112">It comes out of the box with support for [Dropbox](http://dropbox.com/), [GitHub](https://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) and [Zendesk](https://www.zendesk.com/) but it is easy to add support for more.</span></span>

* <span data-ttu-id="6210e-113">No lado de envio, ele fornece suporte para gerenciar e armazenar assinaturas, bem como para enviar notificações de eventos para o conjunto correto de assinantes.</span><span class="sxs-lookup"><span data-stu-id="6210e-113">On the sending side it provides support for managing and storing subscriptions as well as for sending event notifications to the right set of subscribers.</span></span> <span data-ttu-id="6210e-114">Isso permite que você defina seu próprio conjunto de eventos que os assinantes podem assinar e notificá-los quando as coisas acontecem.</span><span class="sxs-lookup"><span data-stu-id="6210e-114">This allows you to define your own set of events that subscribers can subscribe to and notify them when things happens.</span></span>

<span data-ttu-id="6210e-115">As duas partes podem ser usadas juntas ou separadas, dependendo do seu cenário.</span><span class="sxs-lookup"><span data-stu-id="6210e-115">The two parts can be used together or apart depending on your scenario.</span></span> <span data-ttu-id="6210e-116">Se você só precisa receber WebHooks de outros serviços, você pode usar apenas a parte receptora; Se você quiser apenas expor WebHooks para que outras pessoas consumam, você pode fazer exatamente isso.</span><span class="sxs-lookup"><span data-stu-id="6210e-116">If you only need to receive WebHooks from other services then you can use just the receiver part; if you only want to expose WebHooks for others to consume, then you can do just that.</span></span>

<span data-ttu-id="6210e-117">O código tem como alvo ASP.NET Web API 2 e ASP.NET MVC 5 e está disponível como [OSS no GitHub](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="6210e-117">The code targets ASP.NET Web API 2 and ASP.NET MVC 5 and is available as [OSS on GitHub](https://github.com/aspnet/WebHooks).</span></span>

## <a name="webhooks-overview"></a><span data-ttu-id="6210e-118">Visão geral de WebHooks</span><span class="sxs-lookup"><span data-stu-id="6210e-118">WebHooks Overview</span></span>

<span data-ttu-id="6210e-119">WebHooks é um padrão que significa que ele varia como ele é usado do serviço para o serviço, mas a ideia básica é a mesma.</span><span class="sxs-lookup"><span data-stu-id="6210e-119">WebHooks is a pattern which means that it varies how it is used from service to service but the basic idea is the same.</span></span> <span data-ttu-id="6210e-120">Você pode considerar os WebHooks como um modelo pub/sub simples, onde um usuário pode assinar eventos que ocorrem em outro lugar.</span><span class="sxs-lookup"><span data-stu-id="6210e-120">You can think of WebHooks as a simple pub/sub model where a user can subscribe to events happening elsewhere.</span></span> <span data-ttu-id="6210e-121">As notificações de eventos são propagadas como solicitações HTTP POST que contêm informações sobre o próprio evento.</span><span class="sxs-lookup"><span data-stu-id="6210e-121">The event notifications are propagated as HTTP POST requests containing information about the event itself.</span></span>

<span data-ttu-id="6210e-122">Normalmente, a solicitação HTTP POST contém um objeto JSON ou dados de formulário HTML determinados pelo remetente do webhook, incluindo informações sobre o evento que está fazendo com que o webhook seja disparado.</span><span class="sxs-lookup"><span data-stu-id="6210e-122">Typically the HTTP POST request contains a JSON object or HTML form data determined by the WebHook sender including information about the event causing the WebHook to trigger.</span></span> <span data-ttu-id="6210e-123">Por exemplo, um corpo de solicitação de POSTAgem de webhook do [GitHub](https://www.github.com/) tem esta aparência como resultado de uma nova questão ser aberta em um repositório específico:</span><span class="sxs-lookup"><span data-stu-id="6210e-123">For example, a WebHook POST request body from [GitHub](https://www.github.com/) looks like this as a result of a new issue being opened in a particular repository:</span></span>

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

<span data-ttu-id="6210e-124">Para garantir que o webhook seja realmente do remetente pretendido, a solicitação POST é protegida de alguma forma e, em seguida, verificada pelo receptor.</span><span class="sxs-lookup"><span data-stu-id="6210e-124">To ensure that the WebHook is indeed from the intended sender, the POST request is secured in some way and then verified by the receiver.</span></span> <span data-ttu-id="6210e-125">Por exemplo, os [WebHooks do GitHub](https://developer.github.com/webhooks/) incluem um cabeçalho http *X-Hub-Signature* com um hash do corpo da solicitação que é verificado pela implementação do receptor para que você não precise se preocupar com ele.</span><span class="sxs-lookup"><span data-stu-id="6210e-125">For example, [GitHub WebHooks](https://developer.github.com/webhooks/) includes an *X-Hub-Signature* HTTP header with a hash of the request body which is checked by the receiver implementation so you don't have to worry about it.</span></span>

<span data-ttu-id="6210e-126">O fluxo de webhook geralmente é algo assim:</span><span class="sxs-lookup"><span data-stu-id="6210e-126">The WebHook flow generally goes something like this:</span></span>

* <span data-ttu-id="6210e-127">O remetente do webhook expõe eventos que um cliente pode assinar.</span><span class="sxs-lookup"><span data-stu-id="6210e-127">The WebHook sender exposes events that a client can subscribe to.</span></span> <span data-ttu-id="6210e-128">Os eventos descrevem alterações observáveis no sistema, por exemplo, que um novo item de dados foi inserido, que um processo foi concluído ou algo mais.</span><span class="sxs-lookup"><span data-stu-id="6210e-128">The events describe observable changes to the system, for example that a new data item has been inserted, that a process has completed, or something else.</span></span>

* <span data-ttu-id="6210e-129">O receptor de webhook assina registrando um webhook que consiste em quatro coisas:</span><span class="sxs-lookup"><span data-stu-id="6210e-129">The WebHook receiver subscribes by registering a WebHook consisting of four things:</span></span>

     1. <span data-ttu-id="6210e-130">Um URI para onde a notificação de eventos deve ser postada na forma de uma solicitação HTTP POST;</span><span class="sxs-lookup"><span data-stu-id="6210e-130">A URI for where the event notification should be posted in the form of an HTTP POST request;</span></span>

     2. <span data-ttu-id="6210e-131">Um conjunto de filtros que descreve os eventos específicos para os quais o webhook deve ser acionado;</span><span class="sxs-lookup"><span data-stu-id="6210e-131">A set of filters describing the particular events for which the WebHook should be fired;</span></span>

     3. <span data-ttu-id="6210e-132">Uma chave secreta que é usada para assinar a solicitação HTTP POST;</span><span class="sxs-lookup"><span data-stu-id="6210e-132">A secret key which is used to sign the HTTP POST request;</span></span>

     4. <span data-ttu-id="6210e-133">Dados adicionais que serão incluídos na solicitação HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="6210e-133">Additional data which is to be included in the HTTP POST request.</span></span> <span data-ttu-id="6210e-134">Isso pode, por exemplo, ser campos de cabeçalho HTTP adicionais ou Propriedades incluídas no corpo da solicitação HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="6210e-134">This can for example be additional HTTP header fields or properties included in the HTTP POST request body.</span></span>

* <span data-ttu-id="6210e-135">Quando um evento acontece, os registros de webhook correspondentes são encontrados e as solicitações HTTP POST são enviadas.</span><span class="sxs-lookup"><span data-stu-id="6210e-135">Once an event happens, the matching WebHook registrations are found and HTTP POST requests are submitted.</span></span> <span data-ttu-id="6210e-136">Normalmente, a geração das solicitações HTTP POST é repetida várias vezes se, por algum motivo, o destinatário não estiver respondendo ou a solicitação HTTP POST resultar em uma resposta de erro.</span><span class="sxs-lookup"><span data-stu-id="6210e-136">Typically, the generation of the HTTP POST requests are retried several times if for some reason the recipient is not responding or the HTTP POST request results in an error response.</span></span>

## <a name="webhooks-processing-pipeline"></a><span data-ttu-id="6210e-137">Pipeline de processamento de WebHooks</span><span class="sxs-lookup"><span data-stu-id="6210e-137">WebHooks Processing Pipeline</span></span>

<span data-ttu-id="6210e-138">O pipeline de processamento de WebHooks Microsoft ASP.NET para WebHooks de entrada é semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="6210e-138">The Microsoft ASP.NET WebHooks processing pipeline for incoming WebHooks looks like this:</span></span>

![Pipeline de processamento de WebHooks do ASP.NET](_static/WebHookReceivers.png)

<span data-ttu-id="6210e-140">Os dois conceitos principais aqui são *receptores* e *manipuladores*:</span><span class="sxs-lookup"><span data-stu-id="6210e-140">The two key concepts here are *Receivers* and *Handlers*:</span></span>

* <span data-ttu-id="6210e-141">Os *receptores* são responsáveis por lidar com o tipo específico de webhook de um determinado remetente e para impor verificações de segurança para garantir que a solicitação de webhook realmente seja do remetente pretendido.</span><span class="sxs-lookup"><span data-stu-id="6210e-141">*Receivers* are responsible for handling the particular flavor of WebHook from a given sender and for enforcing security checks to ensure that the WebHook request indeed is from the intended sender.</span></span>

* <span data-ttu-id="6210e-142">Os *manipuladores* são normalmente onde o código do usuário executa o processamento do webhook específico.</span><span class="sxs-lookup"><span data-stu-id="6210e-142">*Handlers* are typically where user code runs processing the particular WebHook.</span></span>

<span data-ttu-id="6210e-143">Nos nós a seguir, esses conceitos são descritos em mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="6210e-143">In the following nodes these concepts are described in more details.</span></span>
