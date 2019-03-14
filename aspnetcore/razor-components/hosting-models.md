---
title: Modelos de hospedagem de componentes do Razor
author: guardrex
description: Entenda Blazor do lado do cliente e componentes do lado do servidor ASP.NET Core Razor modelos de hospedagem.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/hosting-models
ms.openlocfilehash: d1e0c472d7d10eeb4cef0da735cf703c98dd1645
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57042433"
---
# <a name="razor-components-hosting-models"></a>Modelos de hospedagem de componentes do Razor

Por [Daniel Roth](https://github.com/danroth27)

Componentes do Razor é uma estrutura de web projetada para ser executado lado do cliente no navegador em um tempo de execução do .NET com base em WebAssembly (*Blazor*) ou do servidor no ASP.NET Core (*componentes do ASP.NET Core Razor*). Independentemente dos modelos de hospedagem de modelo, o aplicativo e o componente *permanecem os mesmos*. Este artigo discute os modelos de hospedagem disponíveis.

## <a name="client-side-hosting-model"></a>Modelo de hospedagem do lado do cliente

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

O principal modelo de hospedagem para Blazor é o lado do cliente em execução no navegador. Nesse modelo, o aplicativo Blazor, suas dependências e o tempo de execução do .NET são baixados para o navegador. O aplicativo é executado diretamente no thread da interface do usuário do navegador. Todas as atualizações de interface do usuário e a manipulação de eventos ocorre dentro do mesmo processo. Os ativos de aplicativo podem ser implantados como arquivos estáticos usando qualquer servidor web é preferencial (consulte [Host e implantar](xref:host-and-deploy/razor-components/index)).

![Blazor lado do cliente: O aplicativo Blazor é executado em um thread de interface do usuário dentro do navegador.](hosting-models/_static/client-side.png)

Para criar um aplicativo de Blazor usando o modelo de hospedagem do lado do cliente, use o **Blazor** ou **Blazor (ASP.NET Core hospedados)** modelos de projeto (`blazor` ou `blazorhosted` modelo ao usar o [dotnet new](/dotnet/core/tools/dotnet-new) em um prompt de comando). Os incluídos *blazor.webassembly.js* identificadores de script:

* Baixando o tempo de execução do .NET, o aplicativo e suas dependências.
* Inicialização de tempo de execução para executar o aplicativo.

O modelo de hospedagem do lado do cliente oferece vários benefícios. Blazor do lado do cliente:

* Não tenha nenhuma dependência do lado do servidor de .NET.
* Tem uma avançada interface do usuário interativo.
* Aproveita totalmente funções e recursos de cliente.
* Descarregamentos de trabalham do servidor para o cliente.
* Dá suporte a cenários offline.

Há desvantagens para hospedagem do lado do cliente. Blazor do lado do cliente:

* Restringe o aplicativo para os recursos do navegador.
* Requer hardware compatível com cliente e software (por exemplo, WebAssembly suporte).
* Tem um tamanho maior de download e o aplicativo mais do que o tempo de carregamento.
* Tem menos mature tempo de execução do .NET e ferramentas de suporte (por exemplo, as limitações no suporte a .NET Standard e depuração).

O Visual Studio inclui o **Blazor (ASP.NET Core hospedado)** modelo de projeto para criar um aplicativo de Blazor que é executado em WebAssembly e é hospedado em um servidor do ASP.NET Core. O aplicativo ASP.NET Core atua como o aplicativo Blazor para clientes, mas caso contrário, é um processo separado. O aplicativo de Blazor do lado do cliente pode interagir com o servidor pela rede usando chamadas à API Web ou conexões do SignalR.

> [!IMPORTANT]
> Se um aplicativo do lado do cliente Blazor é atendido por um aplicativo ASP.NET Core hospedado como um subaplicativo do IIS, desabilite o manipulador herdado do módulo do ASP.NET Core. Defina o caminho base do aplicativo no aplicativo do Blazor *index. HTML* arquivo para o alias IIS usado ao configurar o subaplicativo no IIS.
>
> Para obter mais informações, consulte [aplicativo base do caminho](xref:host-and-deploy/razor-components/index#app-base-path).

## <a name="server-side-hosting-model"></a>Modelo de hospedagem do lado do servidor

O modelo de hospedagem de servidor de componentes do ASP.NET Core Razor, o aplicativo é executado no servidor de dentro de um aplicativo ASP.NET Core. As atualizações da interface do usuário, a manipulação de eventos e as chamadas de JavaScript são realizadas por uma conexão SignalR.

![ASP.NET Core Razor componentes do servidor: O navegador interage com o aplicativo (hospedado dentro de um aplicativo ASP.NET Core) no servidor ao longo de uma conexão SignalR.](hosting-models/_static/server-side.png)

Para criar um aplicativo de componentes do Razor usando o modelo de hospedagem do lado do servidor, use o **Blazor (lado do servidor no ASP.NET Core)** modelo (`blazorserver` ao usar [dotnet novo](/dotnet/core/tools/dotnet-new) em um prompt de comando). Um aplicativo ASP.NET Core hospeda o aplicativo do lado do servidor de componentes do Razor e configura o ponto de extremidade do SignalR em que os clientes se conectam. O aplicativo ASP.NET Core faz referência do aplicativo `Startup` classe a ser adicionada:

* Serviços de componentes do Razor do lado do servidor.
* O aplicativo para o pipeline de tratamento de solicitação.

[!code-csharp[](hosting-models/samples_snapshot/Startup.cs?highlight=5,27)]

O *blazor.server.js* script&dagger; estabelece a conexão de cliente. É responsabilidade do aplicativo para manter e restaurar o estado do aplicativo conforme necessário (por exemplo, no caso de uma conexão de rede perdidas).

O modelo de hospedagem do lado do servidor oferece vários benefícios:

* Permite que você escreva todo o seu aplicativo com o .NET e C# usando o modelo de componente.
* Fornece uma noção interativa avançada e evita as atualizações de página desnecessários.
* Tem um tamanho de aplicativo significativamente menor do que um aplicativo do lado do cliente Blazor e carrega muito mais rapidamente.
* Lógica do componente pode tirar proveito total de recursos de servidor, incluindo o uso de quaisquer APIs compatíveis do .NET Core.
* É executado no .NET Core no servidor, portanto, .NET existentes das ferramentas, como depuração, funciona conforme o esperado.
* Funciona com clientes finos (por exemplo, os navegadores que não dão suporte a recursos e WebAssembly restrita dispositivos).

Há desvantagens para o lado do servidor de hospedagem:

* Tem uma latência mais alta: Cada interação do usuário envolve um salto de rede.
* Não oferece nenhum suporte offline: Se a conexão de cliente falhar, o aplicativo deixará de funcionar.
* Reduziu a escalabilidade: O servidor deve gerenciar várias conexões de cliente e manipular o estado do cliente.
* Requer um servidor do ASP.NET Core para servir o aplicativo. Implantação sem um servidor (por exemplo, de uma CDN) não é possível.

&dagger;O *blazor.server.js* script é publicado para o seguinte caminho: *bin / {depurar | Versão} / {TARGET FRAMEWORK} /publish/ {nome do aplicativo}. Aplicativo/dist/estrutura de _*.
