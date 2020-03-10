---
uid: signalr/overview/getting-started/introduction-to-signalr
title: Introdução ao Signalr | Microsoft Docs
author: bradygaster
description: Este artigo descreve qual sinalização é, e algumas das soluções que ele foi projetada para criar.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 0fab5e35-8c1f-43d4-8635-b8aba8766a71
msc.legacyurl: /signalr/overview/getting-started/introduction-to-signalr
msc.type: authoredcontent
ms.openlocfilehash: 8dbc31a5c8d59fa55dc5b513c1a51d24d18a685f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536431"
---
# <a name="introduction-to-signalr"></a>Introdução ao SignalR

por [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Este artigo descreve qual sinalização é, e algumas das soluções que ele foi projetada para criar. 
> 
> ## <a name="questions-and-comments"></a>Perguntas e comentários
> 
> Deixe comentários sobre como você gostou deste tutorial e o que poderíamos melhorar nos comentários na parte inferior da página. Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, poderá lançá-las no fórum do [signalr ASP.net](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [stackoverflow.com](https://stackoverflow.com/questions/tagged/signalr).

## <a name="what-is-signalr"></a>O que é o Signalr?

O Signalr ASP.NET é uma biblioteca para desenvolvedores de ASP.NET que simplifica o processo de adicionar funcionalidade da Web em tempo real a aplicativos. A funcionalidade da Web em tempo real é a capacidade de ter o conteúdo de push de código do servidor para clientes conectados instantaneamente, pois ele se torna disponível, em vez de ter o servidor aguardar que um cliente solicite novos dados.

O signalr pode ser usado para adicionar qualquer tipo de funcionalidade da Web "em tempo real" ao aplicativo ASP.NET. Embora o chat seja geralmente usado como exemplo, você pode fazer muito mais. Sempre que um usuário atualizar uma página da Web para ver novos dados ou a página implementar a [sondagem longa](http://en.wikipedia.org/wiki/Push_technology#Long_polling) para recuperar novos dados, será um candidato ao uso do signalr. Os exemplos incluem painéis e aplicativos de monitoramento, aplicativos colaborativos (como a edição simultânea de documentos), atualizações de progresso do trabalho e formulários em tempo real.

O signalr também permite tipos completamente novos de aplicativos Web que exigem atualizações de alta frequência do servidor, por exemplo, jogos em tempo real.

O signalr fornece uma API simples para criar chamadas de procedimento remoto (RPC) de servidor para cliente que chamam funções JavaScript em navegadores de cliente (e outras plataformas de cliente) do código .NET do lado do servidor. O signalr também inclui a API para gerenciamento de conexão (por exemplo, eventos de conexão e desconexão) e agrupamento de conexões.

![Invocando métodos com Signalr](introduction-to-signalr/_static/image1.png)

O signalr lida automaticamente com o gerenciamento de conexões e permite que você transmita mensagens a todos os clientes conectados simultaneamente, como uma sala de bate-papo. Você também pode enviar mensagens para clientes específicos. A conexão entre o cliente e o servidor é persistente, ao contrário de uma conexão HTTP clássica, que é restabelecida para cada comunicação.

O signalr dá suporte à funcionalidade de "push de servidor", na qual o código do servidor pode chamar o código do cliente no navegador usando RPC (chamadas de procedimento remoto), em vez do modelo de solicitação-resposta comum na Web hoje.

Os aplicativos signalr podem ser expandidos para milhares de clientes usando provedores de expansão internos e de terceiros.

Os provedores internos incluem:
* [Barramento de Serviço](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3)
* [SQL Server](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
* [Redis](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Redis)

Os provedores de terceiros incluem:
* [NCache](https://www.alachisoft.com/ncache/asp-net-core-signalr.html).

O signalr é um código-fonte aberto, acessível por meio do [GitHub](https://github.com/signalr).

## <a name="signalr-and-websocket"></a>Signalr e WebSocket

O signalr usa o novo transporte WebSocket quando disponível e retorna a Transportações mais antigas, quando necessário. Embora você possa, certamente, escrever seu aplicativo usando o WebSocket diretamente, usar o Signalr significa que muitas das funcionalidades adicionais que você precisaria implementar já foram feitas para você. Mais importante, isso significa que você pode codificar seu aplicativo para tirar proveito do WebSocket sem precisar se preocupar com a criação de um caminho de código separado para clientes mais antigos. O signalr também protege você de ter que se preocupar com atualizações no WebSocket, pois o Signalr é atualizado para dar suporte a alterações no transporte subjacente, fornecendo ao seu aplicativo uma interface consistente entre versões do WebSocket.

<a id="transports"></a>

## <a name="transports-and-fallbacks"></a>Transportes e fallbacks

O signalr é uma abstração de alguns dos transportes que são necessários para realizar o trabalho em tempo real entre o cliente e o servidor. Uma conexão de Signalr é iniciada como HTTP e, em seguida, é promovida para uma conexão WebSocket, se disponível. WebSocket é o transporte ideal para Signalr, pois ele faz o uso mais eficiente da memória do servidor, tem a menor latência e tem os recursos mais subjacentes (como a comunicação full duplex entre o cliente e o servidor), mas também tem o mais estrito requisitos: o WebSocket requer que o servidor esteja usando o Windows Server 2012 ou Windows 8 e .NET Framework 4,5. Se esses requisitos não forem atendidos, o Signalr tentará usar outros transportes para fazer suas conexões.

### <a name="html-5-transports"></a>Transportes HTML 5

Esses transportes dependem do suporte para [HTML 5](http://en.wikipedia.org/wiki/HTML5). Se o navegador do cliente não oferecer suporte ao padrão HTML 5, os transportes mais antigos serão usados.

- **WebSocket** (se o servidor e o navegador indicarem que podem dar suporte ao WebSocket). WebSocket é o único transporte que estabelece uma conexão true persistente e bidirecional entre o cliente e o servidor. No entanto, o WebSocket também tem os requisitos mais rígidos; Ele tem suporte total apenas nas versões mais recentes do Microsoft Internet Explorer, Google Chrome e Mozilla Firefox, e tem apenas uma implementação parcial em outros navegadores, como opera e Safari.
- O **servidor enviou eventos**, também conhecidos como EventSource (se o navegador oferecer suporte a eventos de servidor enviados, que é basicamente todos os navegadores, exceto o Internet Explorer.)

### <a name="comet-transports"></a>Transportes de Comet

Os transportes a seguir são baseados no modelo de aplicativo Web [Comet](http://en.wikipedia.org/wiki/Comet_(programming)) , no qual um navegador ou outro cliente mantém uma solicitação HTTP demorada, que o servidor pode usar para enviar dados por push ao cliente sem que o cliente o solicite especificamente.

- **Quadro para sempre** (somente para o Internet Explorer). O quadro contínuo cria um IFrame oculto que faz uma solicitação para um ponto de extremidade no servidor que não é concluído. Em seguida, o servidor envia continuamente o script para o cliente que é executado imediatamente, fornecendo uma conexão de tempo real unidirecional do servidor para o cliente. A conexão do cliente com o servidor usa uma conexão separada do servidor para a conexão do cliente e, como uma solicitação HTTP padrão, uma nova conexão é criada para cada parte dos dados que precisam ser enviados.
- **Sondagem longa do AJAX**. A sondagem longa não cria uma conexão persistente, mas, em vez disso, pesquisa o servidor com uma solicitação que permanece aberta até que o servidor responda, ponto em que a conexão é fechada e uma nova conexão é solicitada imediatamente. Isso pode introduzir alguma latência enquanto a conexão é redefinida.

Para obter mais informações sobre quais transportes têm suporte em quais configurações, consulte [plataformas com suporte](supported-platforms.md).

### <a name="transport-selection-process"></a>Processo de seleção de transporte

A lista a seguir mostra as etapas que o Signalr usa para decidir qual transporte usar.

1. Se o navegador for o Internet Explorer 8 ou anterior, a sondagem longa será usada.
2. Se JSONP estiver configurado (ou seja, o parâmetro `jsonp` será definido como `true` quando a conexão for iniciada), a sondagem longa será usada.
3. Se uma conexão entre domínios estiver sendo feita (ou seja, se o ponto de extremidade do Signalr não estiver no mesmo domínio que a página de hospedagem), o WebSocket será usado se os critérios a seguir forem atendidos:

   - O cliente oferece suporte a CORS (compartilhamento de recursos entre origens). Para obter detalhes sobre quais clientes dão suporte a CORS, consulte [CORS em caniuse.com](http://www.caniuse.com/CORS).
   - O cliente dá suporte ao WebSocket
   - O servidor dá suporte ao WebSocket

     Se qualquer um desses critérios não for atendido, a sondagem longa será usada. Para obter mais informações sobre conexões entre domínios, consulte [como estabelecer uma conexão entre domínios](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).
4. Se JSONP não estiver configurado e a conexão não for de domínio cruzado, o WebSocket será usado se o cliente e o servidor tiverem suporte para ele.
5. Se o cliente ou o servidor não oferecer suporte ao WebSocket, os eventos enviados do servidor serão usados se estiverem disponíveis.
6. Se o servidor enviou eventos não estiver disponível, será feita uma tentativa de quadro contínuo.
7. Se o quadro contínuo falhar, a sondagem longa será usada.

<a id="MonitoringTransports"></a>
### <a name="monitoring-transports"></a>Monitoramento de transportes

Você pode determinar qual transporte seu aplicativo está usando habilitando o registro em log no Hub e abrindo a janela do console em seu navegador.

Para habilitar o registro em log para os eventos do Hub em um navegador, adicione o seguinte comando ao seu aplicativo cliente:

`$.connection.hub.logging = true;`

- No Internet Explorer, abra as ferramentas de desenvolvedor pressionando f12 e clique na guia Console.

    ![Console do no Microsoft Internet Explorer](introduction-to-signalr/_static/image2.png)
- No Chrome, abra o console pressionando Ctrl + Shift + J.

    ![Console no Google Chrome](introduction-to-signalr/_static/image3.png)

Com o console aberto e o registro em log habilitado, você poderá ver qual transporte está sendo usado pelo Signalr.

![Console no Internet Explorer mostrando transporte WebSocket](introduction-to-signalr/_static/image4.png)

### <a name="specifying-a-transport"></a>Especificando um transporte

Negociar um transporte leva um determinado período de tempo e recursos de cliente/servidor. Se os recursos do cliente forem conhecidos, um transporte poderá ser especificado quando a conexão do cliente for iniciada. O trecho de código a seguir demonstra como iniciar uma conexão usando o transporte de sondagem longa Ajax, como seria usado se fosse conhecido que o cliente não dava suporte a nenhum outro protocolo:

`connection.start({ transport: 'longPolling' });`

Você pode especificar uma ordem de fallback se desejar que um cliente tente transportes específicos na ordem. O trecho de código a seguir demonstra a tentativa do WebSocket e a falha, indo diretamente para a sondagem longa.

`connection.start({ transport: ['webSockets','longPolling'] });`

As constantes de cadeia de caracteres para especificar transportes são definidas da seguinte maneira:

- `webSockets`
- `foreverFrame`
- `serverSentEvents`
- `longPolling`

## <a name="connections-and-hubs"></a>Conexões e hubs

A API do Signalr contém dois modelos para a comunicação entre clientes e servidores: conexões persistentes e hubs.

Uma conexão representa um ponto de extremidade simples para enviar mensagens de destinatário único, agrupadas ou de difusão. A API de conexão persistente (representada no código .NET pela classe PersistentConnection) fornece ao desenvolvedor acesso direto ao protocolo de comunicação de baixo nível que o Signalr expõe. Usar o modelo de comunicação de conexões será familiar aos desenvolvedores que usaram APIs baseadas em conexão, como Windows Communication Foundation.

Um hub é um pipeline de nível mais alto criado com base na API de conexão que permite que o cliente e o servidor chamem métodos entre si diretamente. O signalr lida com a expedição entre limites de máquina como se fosse mágica, permitindo que os clientes chamem métodos no servidor tão facilmente quanto os métodos locais, e vice-versa. Usar o modelo de comunicação de hubs será familiar aos desenvolvedores que usaram APIs de invocação remota, como a comunicação remota do .NET. O uso de um hub também permite que você passe parâmetros fortemente tipados para métodos, habilitando a associação de modelo.

### <a name="architecture-diagram"></a>Diagrama de arquitetura

O diagrama a seguir mostra a relação entre hubs, conexões persistentes e as tecnologias subjacentes usadas para transportes.

![Diagrama de arquitetura do signalr mostrando APIs, transportes e clientes](introduction-to-signalr/_static/image5.png)

### <a name="how-hubs-work"></a>Como os hubs funcionam

Quando o código do lado do servidor chama um método no cliente, um pacote é enviado pelo transporte ativo que contém o nome e os parâmetros do método a ser chamado (quando um objeto é enviado como um parâmetro de método, ele é serializado usando JSON). O cliente então corresponde ao nome do método para métodos definidos no código do lado do cliente. Se houver uma correspondência, o método de cliente será executado usando os dados de parâmetro desserializados.

A chamada de método pode ser monitorada usando ferramentas como o [Fiddler.](http://fiddler2.com/) A imagem a seguir mostra uma chamada de método enviada de um servidor de sinalização para um cliente de navegador da Web no painel logs do Fiddler. A chamada de método está sendo enviada de um Hub chamado `MoveShapeHub`, e o método que está sendo invocado é chamado de `updateShape`.

![Exibição do log Fiddler mostrando o tráfego do Signalr](introduction-to-signalr/_static/image6.png)

Neste exemplo, o nome do hub é identificado com o parâmetro `H`; o nome do método é identificado com o parâmetro `M` e os dados que estão sendo enviados para o método são identificados com o parâmetro `A`. O aplicativo que gerou essa mensagem é criado no tutorial em [tempo real de alta frequência](tutorial-high-frequency-realtime-with-signalr.md) .

### <a name="choosing-a-communication-model"></a>Escolhendo um modelo de comunicação

A maioria dos aplicativos deve usar a API de hubs. A API de conexões pode ser usada nas seguintes circunstâncias:

- O formato da mensagem real enviada precisa ser especificado.
- O desenvolvedor prefere trabalhar com um modelo de mensagens e expedição em vez de um modelo de invocação remota.
- Um aplicativo existente que usa um modelo de mensagens está sendo portado para usar o Signalr.
