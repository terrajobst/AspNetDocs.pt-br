---
uid: signalr/overview/older-versions/hub-authorization
title: Autenticação e autorização para os hubs de sinalização (Signalr 1. x) | Microsoft Docs
author: bradygaster
description: Este tópico descreve como restringir quais usuários ou funções podem acessar métodos de Hub.
ms.author: bradyg
ms.date: 10/17/2013
ms.assetid: 3d2dfc0e-eac2-4076-a468-325d3d01cc7b
msc.legacyurl: /signalr/overview/older-versions/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: 8182677c8931f060d98d17008b16ad545bee4e69
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558530"
---
# <a name="authentication-and-authorization-for-signalr-hubs-signalr-1x"></a>Autenticação e autorização para hubs do SignalR (SignalR 1.x)

por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Este tópico descreve como restringir quais usuários ou funções podem acessar métodos de Hub.

## <a name="overview"></a>Visão geral

Este tópico contém as seguintes seções:

- [Autorizar atributo](#authorizeattribute)
- [Exigir autenticação para todos os hubs](#requireauth)
- [Autorização personalizada](#custom)
- [Passar informações de autenticação para clientes](#passauth)
- [Opções de autenticação para clientes .NET](#authoptions)

    - [Cookie com autenticação de formulários](#cookie)
    - [Autenticação do Windows](#windows)
    - [Cabeçalho de conexão](#header)
    - [Certificado](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a>Autorizar atributo

O signalr fornece o atributo [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) para especificar quais usuários ou funções têm acesso a um Hub ou método. Esse atributo está localizado no namespace `Microsoft.AspNet.SignalR`. Você aplica o atributo `Authorize` a um Hub ou métodos específicos em um Hub. Quando você aplica o atributo `Authorize` a uma classe de Hub, o requisito de autorização especificado é aplicado a todos os métodos no Hub. Os diferentes tipos de requisitos de autorização que você pode aplicar são mostrados abaixo. Sem o atributo `Authorize`, todos os métodos públicos no Hub estão disponíveis para um cliente que está conectado ao Hub.

Se você tiver definido uma função denominada "admin" em seu aplicativo Web, poderá especificar que somente os usuários nessa função possam acessar um hub com o código a seguir.

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

Ou você pode especificar que um hub contém um método que está disponível para todos os usuários e um segundo método que só está disponível para usuários autenticados, como mostrado abaixo.

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

Os exemplos a seguir abordam diferentes cenários de autorização:

- `[Authorize]` – somente usuários autenticados
- `[Authorize(Roles = "Admin,Manager")]` – somente usuários autenticados nas funções especificadas
- `[Authorize(Users = "user1,user2")]` – somente usuários autenticados com os nomes de usuário especificados
- `[Authorize(RequireOutgoing=false)]` – somente usuários autenticados podem invocar o Hub, mas as chamadas do servidor de volta para os clientes não são limitadas pela autorização, como, quando apenas determinados usuários podem enviar uma mensagem, mas todas as outras pessoas podem receber a mensagem. A propriedade RequireOutgoing só pode ser aplicada a todo o Hub, não a métodos de indivíduos dentro do Hub. Quando RequireOutgoing não está definido como false, somente os usuários que atendem ao requisito de autorização são chamados do servidor.

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a>Exigir autenticação para todos os hubs

Você pode exigir autenticação para todos os hubs e métodos de Hub em seu aplicativo chamando o método [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) quando o aplicativo é iniciado. Você pode usar esse método quando tiver vários hubs e desejar impor um requisito de autenticação para todos eles. Com esse método, você não pode especificar a função, o usuário ou a autorização de saída. Você só pode especificar que o acesso aos métodos de Hub seja restrito a usuários autenticados. No entanto, você ainda pode aplicar o atributo autorizar a hubs ou métodos para especificar requisitos adicionais. Qualquer requisito especificado em atributos é aplicado além do requisito básico de autenticação.

O exemplo a seguir mostra um arquivo global. asax que restringe todos os métodos de Hub para usuários autenticados.

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

Se você chamar o método `RequireAuthentication()` depois que uma solicitação de Signalr for processada, o Signalr gerará uma exceção de `InvalidOperationException`. Essa exceção é gerada porque você não pode adicionar um módulo ao HubPipeline depois que o pipeline é invocado. O exemplo anterior mostra a chamada do método `RequireAuthentication` no método `Application_Start` que é executado uma vez antes da manipulação da primeira solicitação.

<a id="custom"></a>

## <a name="customized-authorization"></a>Autorização personalizada

Se você precisar personalizar como a autorização é determinada, você pode criar uma classe que deriva de `AuthorizeAttribute` e substituir o método [Userautoted](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) . Esse método é chamado para cada solicitação para determinar se o usuário está autorizado a concluir a solicitação. No método substituído, você fornece a lógica necessária para seu cenário de autorização. O exemplo a seguir mostra como impor a autorização por meio de identidade baseada em declarações.

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a>Passar informações de autenticação para clientes

Talvez seja necessário usar as informações de autenticação no código que é executado no cliente. Você passa as informações necessárias ao chamar os métodos no cliente. Por exemplo, um método de aplicativo de chat pode passar como um parâmetro o nome de usuário da pessoa que está postando uma mensagem, como mostrado abaixo.

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

Ou, você pode criar um objeto para representar as informações de autenticação e passar esse objeto como um parâmetro, conforme mostrado abaixo.

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

Você nunca deve passar a ID de conexão de um cliente para outros clientes, uma vez que um usuário mal-intencionado pode usá-lo para imitar uma solicitação desse cliente.

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a>Opções de autenticação para clientes .NET

Quando você tem um cliente .NET, como um aplicativo de console, que interage com um hub limitado a usuários autenticados, você pode passar as credenciais de autenticação em um cookie, no cabeçalho de conexão ou em um certificado. Os exemplos nesta seção mostram como usar esses métodos diferentes para autenticar um usuário. Eles não são aplicativos de Signaler totalmente funcionais. Para obter mais informações sobre clientes .NET com o Signalr, consulte [Guia de API de hubs-cliente .net](../guide-to-the-api/hubs-api-guide-net-client.md).

<a id="cookie"></a>

### <a name="cookie"></a>Cookie

Quando o cliente .NET interage com um Hub que usa a autenticação de formulários ASP.NET, você precisará definir manualmente o cookie de autenticação na conexão. Você adiciona o cookie à propriedade `CookieContainer` no objeto [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) . O exemplo a seguir mostra um aplicativo de console que recupera um cookie de autenticação de uma página da Web e adiciona esse cookie à conexão. A URL `https://www.contoso.com/RemoteLogin` no exemplo aponta para uma página da Web que você precisa criar. A página recuperaria o nome de usuário e a senha postados e tentaria fazer logon no usuário com as credenciais.

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

O aplicativo de console posta as credenciais para www.contoso.com/RemoteLogin que podem se referir a uma página vazia que contém o seguinte arquivo code-behind.

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a>Autenticação do Windows

Ao usar a autenticação do Windows, você pode passar as credenciais do usuário atual usando a propriedade [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) . Você define as credenciais para a conexão com o valor de DefaultCredentials.

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a>Cabeçalho de conexão

Se seu aplicativo não estiver usando cookies, você poderá passar informações do usuário no cabeçalho de conexão. Por exemplo, você pode passar um token no cabeçalho de conexão.

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

Em seguida, no Hub, você verificaria o token do usuário.

<a id="certificate"></a>

### <a name="certificate"></a>Certificado

Você pode passar um certificado de cliente para verificar o usuário. Você adiciona o certificado ao criar a conexão. O exemplo a seguir mostra apenas como adicionar um certificado de cliente à conexão; Ele não mostra o aplicativo de console completo. Ele usa a classe [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) , que fornece várias maneiras diferentes de criar o certificado.

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
