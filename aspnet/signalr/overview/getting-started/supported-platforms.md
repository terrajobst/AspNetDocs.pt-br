---
uid: signalr/overview/getting-started/supported-platforms
title: Plataformas com suporte | Microsoft Docs
author: bradygaster
description: Este artigo descreve o que é oferecido suporte a clientes e servidores pelo Signalr.
ms.author: bradyg
ms.date: 04/18/2018
ms.assetid: eac31beb-0f46-4afa-9def-e80904dea4f0
msc.legacyurl: /signalr/overview/getting-started/supported-platforms
msc.type: authoredcontent
ms.openlocfilehash: 89730e591bb94022d16fe1a78a4350b38e0bf7a4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558628"
---
# <a name="supported-platforms"></a>Plataformas com suporte

por [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Este artigo descreve o que é oferecido suporte a clientes e servidores pelo Signalr. 
> 
> ## <a name="questions-and-comments"></a>Perguntas e comentários
> 
> Deixe comentários sobre como você gostou deste tutorial e o que poderíamos melhorar nos comentários na parte inferior da página. Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, poderá lançá-las no fórum do [signalr ASP.net](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [stackoverflow.com](http://stackoverflow.com/).

O signalr tem suporte em uma variedade de configurações de servidor e cliente. Além disso, cada opção de transporte tem um conjunto de requisitos próprios; Se os requisitos do sistema para um transporte não estiverem disponíveis, o Signalr executará um failover normal para outros transportes. Para obter mais informações sobre os transportes aos quais o Signalr dá suporte, consulte [transportes e fallbacks](introduction-to-signalr.md#transports).

## <a name="server-system-requirements"></a>Requisitos do sistema do servidor do

O componente de servidor Signalr pode ser hospedado em uma variedade de configurações de servidor. Esta seção descreve as versões com suporte dos sistemas operacionais, do .NET Framework, do Internet Information Server e de outros componentes.

### <a name="supported-server-operating-systems"></a>Sistemas operacionais de servidor com suporte

O componente de servidor Signalr pode ser hospedado nos seguintes sistemas operacionais de servidor ou cliente. Observe que para o Signalr usar Websockets, o Windows Server 2012, o Windows Server 2016 ou o Windows 8 é necessário (WebSocket pode ser usado em sites do Windows Azure, desde que a versão do .NET Framework do site esteja definida como 4,5 e o Web Sockets esteja habilitado no Página de configuração).

- Windows Server 2016
- Windows Server 2012
- Windows Server 2008 r2
- Windows 10
- Windows 8
- Windows 7
- Windows Azure

### <a name="supported-server-net-framework-version"></a>Versão de .NET Framework de servidor com suporte

Só há suporte para o signalr 2 no .NET Framework 4,5. Consulte a seção [atualizações recomendadas](#updates) para obter atualizações que melhoram a confiabilidade, a compatibilidade, a estabilidade e o desempenho.

### <a name="supported-server-iis-versions"></a>Versões do IIS do servidor com suporte

Quando o Signalr é hospedado no IIS, há suporte para as seguintes versões. Observe que, se um sistema operacional cliente for usado, como para desenvolvimento (Windows 8 ou Windows 7), as versões completas do IIS ou Cassini não devem ser usadas, já que haverá um limite de 10 conexões simultâneas impostas, que serão atingidas muito rapidamente desde as conexões são transitórios, frequentemente restabelecidas e não são descartados imediatamente após não serem mais usados. IIS Express deve ser usado em sistemas operacionais cliente.

Observe também que para o Signalr usar WebSocket, o IIS 8 ou o IIS 8 Express deve ser usado, o servidor deve usar o Windows 8, o Windows Server 2012 ou posterior e o WebSocket deve estar habilitado no IIS. Para obter informações sobre como habilitar o WebSocket no IIS, consulte [suporte ao protocolo WebSocket do iis 8,0](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support).

- IIS 8 ou IIS 8 Express.
- IIS 7 e 7,5. O suporte para [URLs sem extensão](https://support.microsoft.com/kb/980368) é necessário.
- O IIS deve estar sendo executado no modo integrado; Não há suporte para o modo clássico. Os atrasos de mensagens de até 30 segundos poderão ser experimentados se o IIS for executado no modo clássico usando o transporte de eventos enviados pelo servidor.
- O aplicativo de hospedagem deve estar sendo executado no modo de confiança total.

## <a name="client-system-requirements"></a>Requisitos de sistema para o Cliente do

O signalr pode ser usado em uma variedade de plataformas de cliente. Esta seção descreve os requisitos de sistema para usar o Signalr em navegadores da Web, aplicativos de área de trabalho do Windows, aplicativos Silverlight e dispositivos móveis.

### <a name="web-browsers"></a>Navegadores da Web

O signalr pode ser usado em vários navegadores da Web, mas, em geral, há suporte apenas para as duas versões mais recentes.

Os aplicativos que usam o Signalr em navegadores devem usar o jQuery versão 1.6.4 ou versões posteriores principais (como 1.7.2, 1.8.2 ou 1.9.1).

O signalr pode ser usado nos seguintes navegadores:

- Microsoft Internet Explorer versões 8, 9, 10 e 11. As versões modernas, desktop e móvel têm suporte.
- Mozilla Firefox: versão atual-1, versões do Windows e Mac.
- Google Chrome: versão atual-1, versões do Windows e Mac.
- Safari: versão atual-1, versões Mac e iOS.
- Opera: versão atual-1, somente Windows.
- Navegador Android

Além de exigir determinados navegadores, os vários transportes que o Signalr usa têm seus próprios requisitos. Os seguintes transportes têm suporte nas seguintes configurações:

<a id="browser"></a>

**Requisitos de transporte do navegador da Web**

| Transporte | Internet Explorer | Chrome (Windows ou iOS) | Firefox | Safari (OSX ou iOS) | Android |
| --- | --- | --- | --- | --- | --- |
| WebSockets | 10+ | atual-1 | atual-1 | atual-1 | N/D |
| Eventos enviados pelo servidor | N/D | atual-1 | atual-1 | atual-1 | N/D |
| ForeverFrame | 8+ | N/D | N/D | N/D | 4.1 |
| Sondagem longa | 8+ | atual-1 | atual-1 | atual-1 | 4.1 |

\*: 6 + necessário para a funcionalidade completa.

#### <a name="unsupported-browsers"></a>Navegadores sem suporte

Embora o Signalr *possa* ser executado sem problemas importantes nas versões mais antigas do navegador, não testamos ativamente o signalr neles e geralmente não corrigem os bugs que podem aparecer neles.

### <a name="windows-desktop-and-silverlight-applications"></a>Aplicativos do Windows desktop e do Silverlight

Além de serem executados em um navegador da Web, o Signalr pode ser hospedado em aplicativos Windows Client ou Silverlight autônomos. Os aplicativos de sinalização do Windows desktop e do Silverlight têm os seguintes requisitos de sistema.

- Os aplicativos que usam o .NET 4 têm suporte no Windows XP SP3 ou posterior.
- Os aplicativos que usam .NET Framework 4,5 têm suporte no Windows Vista ou posterior.

Além dos requisitos do sistema operacional e do .NET Framework, os transportes disponíveis para o Signalr têm seus próprios requisitos. Os seguintes transportes têm suporte nas seguintes configurações:

**Requisitos de transporte do Windows desktop e do Silverlight**

| Transporte | Aplicativo .NET | Silverlight |
| --- | --- | --- |
| Soquetes Web | Windows 8 + e .NET 4.5 + | N/D |
| Quadro para sempre | N/D | N/D |
| Eventos enviados pelo servidor | .NET 4+ | 5+ |
| Sondagem longa | .NET 4+ | 5+ |

<a id="android"></a>

### <a name="windows-store-and-windows-phone-applications"></a>Aplicativos da Windows Store e Windows Phone

O signalr pode ser usado em aplicativos da Windows Store e Windows Phone 8 aplicativos. Os seguintes transportes têm suporte nas seguintes configurações:

**Requisitos de transporte da Windows Store e Windows Phone**

| Transporte | Windows Store/.NET | Windows Store/JavaScript | Windows Phone/IE | Windows Phone/.NET |
| --- | --- | --- | --- | --- |
| WebSockets | N/D | Win8 + | 8+ | N/D |
| Quadro para sempre | N/D | Win8 + | 7.5+ | N/D |
| Eventos enviados pelo servidor | Win8 + | N/D | N/D | 8+ |
| Sondagem longa | Win8 + | Win8 + | 7.5+ | 8+ |

<a id="updates"></a>

## <a name="recommended-updates"></a>Atualizações recomendadas

As atualizações a seguir são recomendadas para servidores Signalr:

- Uma atualização para o .NET Framework 4,5 está disponível [aqui](https://support.microsoft.com/kb/2750149).
- A Microsoft lançará periodicamente QFEs para ASP.NET. Eles devem ser aplicados como disponíveis.
