---
uid: signalr/overview/older-versions/troubleshooting
title: Solução de problemas de sinalização (Signalr 1. x) | Microsoft Docs
author: bradygaster
description: Este artigo descreve problemas comuns com o desenvolvimento de aplicativos Signalr.
ms.author: bradyg
ms.date: 06/05/2013
ms.assetid: 347210ba-c452-4feb-886f-b51d89f58971
msc.legacyurl: /signalr/overview/older-versions/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: 3dbf091ac2daa4da477b405852bb4d1316584fcd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579600"
---
# <a name="signalr-troubleshooting-signalr-1x"></a>Solução de problemas do SignalR (SignalR 1.x)

por [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Este documento descreve problemas comuns de solução de problemas com o Signalr.

Este documento contém as seções a seguir.

- [A chamada de métodos entre o cliente e o servidor falha silenciosamente](#connection)
- [Outros problemas de conexão](#other)
- [Compilação e erros do lado do servidor](#server)
- [Problemas do Visual Studio](#vs)
- [Problemas de Serviços de Informações da Internet](#iis)
- [Problemas do Azure](#azure)

<a id="connection"></a>

## <a name="calling-methods-between-the-client-and-server-silently-fails"></a>A chamada de métodos entre o cliente e o servidor falha silenciosamente

Esta seção descreve as possíveis causas de uma chamada de método entre o cliente e o servidor falhar sem uma mensagem de erro significativa. Em um aplicativo Signalr, o servidor não tem informações sobre os métodos que o cliente implementa; Quando o servidor invoca um método de cliente, o nome do método e os dados do parâmetro são enviados ao cliente, e o método é executado somente se ele existir no formato especificado pelo servidor. Se nenhum método correspondente for encontrado no cliente, nada acontecerá e nenhuma mensagem de erro será gerada no servidor.

Para investigar melhor os métodos de cliente que não estão sendo chamados, você pode ativar o registro em log antes de chamar o método Start no Hub para ver quais chamadas são provenientes do servidor. Para habilitar o log em um aplicativo JavaScript, consulte [como habilitar o log do lado do cliente (versão do cliente JavaScript)](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging). Para habilitar o log em um aplicativo cliente .NET, consulte [como habilitar o log do lado do cliente (versão do cliente .net)](../guide-to-the-api/hubs-api-guide-net-client.md#logging).

### <a name="misspelled-method-incorrect-method-signature-or-incorrect-hub-name"></a>Método digitado incorretamente, assinatura de método incorreta ou nome de Hub incorreto

Se o nome ou a assinatura de um método chamado não corresponder exatamente a um método apropriado no cliente, a chamada falhará. Verifique se o nome do método chamado pelo servidor corresponde ao nome do método no cliente. Além disso, o Signalr cria o proxy de Hub usando métodos camel-case, como é apropriado no JavaScript, de modo que um método chamado `SendMessage` no servidor seria chamado `sendMessage` no proxy do cliente. Se você usar o atributo `HubName` no código do servidor, verifique se o nome usado corresponde ao nome usado para criar o Hub no cliente. Se você não usar o atributo `HubName`, verifique se o nome do Hub em um cliente JavaScript é camel-case, como chatHub em vez de ChatHub.

### <a name="duplicate-method-name-on-client"></a>Nome de método duplicado no cliente

Verifique se você não tem um método duplicado no cliente que difere somente por maiúsculas e minúsculas. Se o aplicativo cliente tiver um método chamado `sendMessage`, verifique se também não há um método chamado `SendMessage`.

### <a name="missing-json-parser-on-the-client"></a>Analisador JSON ausente no cliente

O signalr requer que um analisador JSON esteja presente para serializar chamadas entre o servidor e o cliente. Se o cliente não tiver um analisador JSON interno (como o Internet Explorer 7), você precisará incluir um em seu aplicativo. Você pode baixar o analisador JSON [aqui](http://nuget.org/packages/json2).

### <a name="mixing-hub-and-persistentconnection-syntax"></a>Mistura de Hub e sintaxe PersistentConnection

O signalr usa dois modelos de comunicação: hubs e PersistentConnections. A sintaxe para chamar esses dois modelos de comunicação é diferente no código do cliente. Se você tiver adicionado um Hub em seu código de servidor, verifique se todo o seu código de cliente usa a sintaxe de Hub apropriada.

**Código de cliente JavaScript que cria um PersistentConnection em um cliente JavaScript**

[!code-javascript[Main](troubleshooting/samples/sample1.js)]

**Código de cliente JavaScript que cria um proxy de Hub em um cliente JavaScript**

[!code-javascript[Main](troubleshooting/samples/sample2.js)]

**C#código de servidor que mapeia uma rota para um PersistentConnection**

[!code-csharp[Main](troubleshooting/samples/sample3.cs)]

**C#código de servidor que mapeia uma rota para um Hub ou para vários hubs se você tiver vários aplicativos**

[!code-csharp[Main](troubleshooting/samples/sample4.cs)]

### <a name="connection-started-before-subscriptions-are-added"></a>Conexão iniciada antes da adição de assinaturas

Se a conexão do Hub for iniciada antes que os métodos que podem ser chamados do servidor sejam adicionados ao proxy, as mensagens não serão recebidas. O código JavaScript a seguir não iniciará o Hub corretamente:

**Código de cliente JavaScript incorreto que não permitirá que as mensagens de hubs sejam recebidas**

[!code-javascript[Main](troubleshooting/samples/sample5.js)]

Em vez disso, adicione as assinaturas do método antes de chamar Start:

**Código de cliente JavaScript que adiciona corretamente assinaturas a um hub**

[!code-javascript[Main](troubleshooting/samples/sample6.js)]

### <a name="missing-method-name-on-the-hub-proxy"></a>Nome de método ausente no proxy de Hub

Verifique se o método definido no servidor está inscrito no cliente. Embora o servidor defina o método, ele ainda deve ser adicionado ao proxy do cliente. Os métodos podem ser adicionados ao proxy do cliente das seguintes maneiras (Observe que o método é adicionado ao membro `client` do Hub, e não ao Hub diretamente):

**Código de cliente JavaScript que adiciona métodos a um proxy de Hub**

[!code-javascript[Main](troubleshooting/samples/sample7.js)]

### <a name="hub-or-hub-methods-not-declared-as-public"></a>Métodos de Hub ou Hub não declarados como públicos

Para estar visível no cliente, a implementação e os métodos do Hub devem ser declarados como `public`.

### <a name="accessing-hub-from-a-different-application"></a>Acessando o Hub de um aplicativo diferente

Os hubs de sinalização só podem ser acessados por meio de aplicativos que implementam clientes do Signalr. O signalr não pode interoperar com outras bibliotecas de comunicação (como SOAP ou WCF Web Services.) Se não houver nenhum cliente Signalr disponível para sua plataforma de destino, você não poderá acessar o ponto de extremidade do servidor diretamente.

### <a name="manually-serializing-data"></a>Serializando dados manualmente

O signalr usará automaticamente o JSON para serializar os parâmetros do método – não é necessário fazê-lo por conta própria.

### <a name="remote-hub-method-not-executed-on-client-in-ondisconnected-function"></a>Método de Hub remoto não executado no cliente na função ondisconnectd

Este comportamento ocorre por design. Quando `OnDisconnected` é chamado, o Hub já entrou no estado de `Disconnected`, o que não permite que mais métodos de Hub sejam chamados.

**C#código do servidor que executa corretamente o código no evento OnDisconnected**

[!code-csharp[Main](troubleshooting/samples/sample8.cs)]

### <a name="connection-limit-reached"></a>Limite de conexão atingido

Ao usar a versão completa do IIS em um sistema operacional cliente como o Windows 7, um limite de 10 conexões é imposto. Ao usar um sistema operacional cliente, use IIS Express em vez disso para evitar esse limite.

### <a name="cross-domain-connection-not-set-up-properly"></a>Conexão entre domínios não configurada corretamente

Se uma conexão entre domínios (uma conexão para a qual a URL de sinalização não estiver no mesmo domínio que a página de hospedagem) não estiver configurada corretamente, a conexão poderá falhar sem uma mensagem de erro. Para obter informações sobre como habilitar a comunicação entre domínios, consulte [como estabelecer uma conexão entre domínios](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).

### <a name="connection-using-ntlm-active-directory-not-working-in-net-client"></a>Conexão usando NTLM (Active Directory) não está funcionando no cliente .NET

Uma conexão em um aplicativo cliente .NET que usa segurança de domínio poderá falhar se a conexão não estiver configurada corretamente. Para usar o Signalr em um ambiente de domínio, defina a propriedade de conexão requisito da seguinte maneira:

**C#código do cliente que implementa as credenciais de conexão**

[!code-csharp[Main](troubleshooting/samples/sample9.cs)]

<a id="other"></a>

## <a name="other-connection-issues"></a>Outros problemas de conexão

Esta seção descreve as causas e soluções para sintomas específicos ou mensagens de erro que ocorrem durante uma conexão.

### <a name="start-must-be-called-before-data-can-be-sent-error"></a>Erro "o início deve ser chamado antes que os dados possam ser enviados"

Esse erro geralmente é visto se o código faz referência a objetos de sinalização antes da conexão ser iniciada. O wireup para manipuladores e semelhantes que chamarão os métodos definidos no servidor deve ser adicionado após a conclusão da conexão. Observe que a chamada para `Start` é assíncrona, portanto, o código após a chamada pode ser executado antes de ser concluído. A melhor maneira de adicionar manipuladores depois que uma conexão é iniciada por completo é colocá-los em uma função de retorno de chamada que é passada como um parâmetro para o método Start:

**Código de cliente JavaScript que adiciona corretamente manipuladores de eventos que referenciam objetos de sinalização**

[!code-javascript[Main](troubleshooting/samples/sample10.js?highlight=1)]

Esse erro também será visto se uma conexão for interrompida enquanto os objetos de sinalização ainda estão sendo referenciados.

### <a name="301-moved-permanently-or-302-moved-temporarily-error"></a>erro "301 movido permanentemente" ou "302 movido temporariamente"

Esse erro pode ser visto se o projeto contiver uma pasta chamada Signalr, que irá interferir no proxy criado automaticamente. Para evitar esse erro, não use uma pasta chamada `SignalR` em seu aplicativo ou desative a geração automática de proxy. Consulte [o proxy gerado e o que ele faz para você](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy) para obter mais detalhes.

### <a name="403-forbidden-error-in-net-or-silverlight-client"></a>erro "403 Proibido" no cliente .NET ou Silverlight

Esse erro pode ocorrer em ambientes de domínio cruzado em que a comunicação entre domínios não está habilitada corretamente. Para obter informações sobre como habilitar a comunicação entre domínios, consulte [como estabelecer uma conexão entre domínios](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain). Para estabelecer uma conexão entre domínios em um cliente Silverlight, consulte [conexões entre domínios de clientes do Silverlight](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain).

### <a name="404-not-found-error"></a>erro "404 não encontrado"

Há várias causas para esse problema. Verifique todos os itens a seguir:

- **Referência de endereço de proxy de Hub não formatada corretamente:** Esse erro é normalmente visto se a referência ao endereço de proxy de Hub gerado não estiver formatada corretamente. Verifique se a referência ao endereço do Hub foi feita corretamente. Consulte [como referenciar o proxy gerado dinamicamente](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy) para obter detalhes.
- **Adicionando rotas ao aplicativo antes de adicionar a rota do Hub:** Se seu aplicativo usar outras rotas, verifique se a primeira rota adicionada é a chamada para `MapHubs`.

### <a name="500-internal-server-error"></a>"erro interno do servidor 500"

Esse é um erro muito genérico que pode ter uma ampla variedade de causas. Os detalhes do erro devem aparecer no log de eventos do servidor ou podem ser encontrados por meio da depuração do servidor. Informações de erro mais detalhadas podem ser obtidas ativando erros detalhados no servidor. Para obter mais informações, consulte [como tratar erros na classe Hub](../guide-to-the-api/hubs-api-guide-server.md#handleErrors).

### <a name="typeerror-lthubtypegt-is-undefined-error"></a>Erro "TypeError: &lt;hubType&gt; indefinido"

Esse erro será resultado se a chamada para `MapHubs` não for feita corretamente. Consulte [como registrar a rota do signalr e configurar as opções do signalr](../guide-to-the-api/hubs-api-guide-server.md#route) para obter mais informações.

### <a name="jsonserializationexception-was-unhandled-by-user-code"></a>JsonSerializationException não foi tratado pelo código do usuário

Verifique se os parâmetros que você envia para seus métodos não incluem tipos não serializáveis (como identificadores de arquivo ou conexões de banco de dados). Se você precisar usar membros em um objeto do lado do servidor que não deseja que sejam enviados ao cliente (para segurança ou por motivos de serialização), use o atributo `JSONIgnore`.

### <a name="protocol-error-unknown-transport-error"></a>Erro de "erro de protocolo: transporte desconhecido"

Esse erro pode ocorrer se o cliente não oferecer suporte aos transportes que o Signalr usa. Consulte [transportes e fallbacks](../getting-started/introduction-to-signalr.md#transports) para obter informações sobre quais navegadores podem ser usados com o signalr.

### <a name="javascript-hub-proxy-generation-has-been-disabled"></a>"A geração de proxy de Hub JavaScript foi desabilitada".

Esse erro ocorrerá se `DisableJavaScriptProxies` estiver definido, incluindo também uma referência ao proxy gerado dinamicamente em `signalr/hubs`. Para obter mais informações sobre como criar o proxy manualmente, consulte [o proxy gerado e o que ele faz para você](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy).

### <a name="the-connection-id-is-in-the-incorrect-format-or-the-user-identity-cannot-change-during-an-active-signalr-connection-error"></a>"O ID da conexão está no formato incorreto" ou "a identidade do usuário não pode ser alterada durante um erro de conexão de sinal ativo"

Esse erro pode ser visto se a autenticação estiver sendo usada e o cliente for desconectado antes de a conexão ser interrompida. A solução é interromper a conexão do Signalr antes de registrar o cliente.

### <a name="uncaught-error-signalr-jquery-not-found-please-ensure-jquery-is-referenced-before-the-signalrjs-file-error"></a>"Erro não detectado: Signalr: jQuery não encontrado. Verifique se o jQuery é referenciado antes do arquivo Signalr. js "

O cliente do Signalr JavaScript requer que o jQuery seja executado. Verifique se a referência ao jQuery está correta, se o caminho usado é válido e se a referência ao jQuery é anterior à referência ao Signalr.

### <a name="uncaught-typeerror-cannot-read-property-ltpropertygt-of-undefined-error"></a>"Uncapturoud TypeError: não é possível ler a propriedade '&lt;Property&gt;' of undefined" Error

Esse erro resulta de não ter jQuery ou o proxy de hubs referenciado corretamente. Verifique se a referência ao jQuery e ao proxy de hubs está correta, se o caminho usado é válido e se a referência ao jQuery é anterior à referência ao proxy de hubs. A referência padrão ao proxy de hubs deve ser semelhante ao seguinte:

**Código HTML do lado do cliente que referencia corretamente o proxy de hubs**

[!code-html[Main](troubleshooting/samples/sample11.html)]

### <a name="runtimebinderexception-was-unhandled-by-user-code-error"></a>Erro "RuntimeBinderException não foi tratado pelo código do usuário"

Esse erro pode ocorrer quando a sobrecarga incorreta de `Hub.On` é usada. Se o método tiver um valor de retorno, o tipo de retorno deverá ser especificado como um parâmetro de tipo genérico:

**Método definido no cliente (sem proxy gerado)**

[!code-html[Main](troubleshooting/samples/sample12.html?highlight=1)]

### <a name="connection-id-is-inconsistent-or-connection-breaks-between-page-loads"></a>A ID de conexão é inconsistente ou quebras de conexão entre cargas de página

Este comportamento ocorre por design. Como o objeto Hub é hospedado no objeto Page, o Hub é destruído quando a página é atualizada. Um aplicativo de várias páginas precisa manter a associação entre os usuários e as IDs de conexão para que eles sejam consistentes entre os carregamentos de página. As IDs de conexão podem ser armazenadas no servidor em um objeto `ConcurrentDictionary` ou em um banco de dados.

### <a name="value-cannot-be-null-error"></a>Erro "o valor não pode ser nulo"

Os métodos do lado do servidor com parâmetros opcionais não têm suporte no momento; Se o parâmetro opcional for omitido, o método falhará. Para obter mais informações, consulte [parâmetros opcionais](https://github.com/SignalR/SignalR/issues/324).

### <a name="firefox-cant-establish-a-connection-to-the-server-at-ltaddressgt-error-in-firebug"></a>Erro "o Firefox não pode estabelecer uma conexão com o servidor no endereço &lt;&gt;" no Firebug

Essa mensagem de erro pode ser vista no Firebug se a negociação do transporte do WebSocket falhar e outro transporte for usado em seu lugar. Este comportamento ocorre por design.

### <a name="the-remote-certificate-is-invalid-according-to-the-validation-procedure-error-in-net-client-application"></a>Erro "o certificado remoto é inválido de acordo com o procedimento de validação" no aplicativo cliente .NET

Se o servidor exigir certificados de cliente personalizados, você poderá adicionar um X509Certificate à conexão antes que a solicitação seja feita. Adicione o certificado à conexão usando `Connection.AddClientCertificate`.

### <a name="connection-drops-after-authentication-times-out"></a>A conexão cai após a autenticação expirar

Este comportamento ocorre por design. As credenciais de autenticação não podem ser modificadas enquanto uma conexão estiver ativa; para atualizar as credenciais, a conexão deve ser interrompida e reiniciada.

### <a name="onconnected-gets-called-twice-when-using-jquery-mobile"></a>Onconnected é chamado duas vezes ao usar o jQuery Mobile

a função `initializePage` do jQuery Mobile força os scripts em cada página a ser executada novamente, criando uma segunda conexão. As soluções para esse problema incluem:

- Inclua a referência ao jQuery Mobile antes do arquivo JavaScript.
- Desabilite a função `initializePage` definindo `$.mobile.autoInitializePage = false`.
- Aguarde até que a página termine a inicialização antes de iniciar a conexão.

### <a name="messages-are-delayed-in-silverlight-applications-using-server-sent-events"></a>As mensagens são atrasadas em aplicativos Silverlight usando eventos enviados pelo servidor

As mensagens são atrasadas ao usar eventos de servidor enviados no Silverlight. Para forçar o uso da sondagem longa, use o seguinte ao iniciar a conexão:

[!code-css[Main](troubleshooting/samples/sample13.css)]

### <a name="permission-denied-using-forever-frame-protocol"></a>"Permissão negada" usando o protocolo de quadro contínuo

Esse é um problema conhecido, descrito [aqui](https://github.com/SignalR/SignalR/issues/1963). Esse sintoma pode ser visto usando a biblioteca JQuery mais recente; a solução alternativa é fazer downgrade de seu aplicativo para JQuery 1.8.2.

<a id="server"></a>

## <a name="compilation-and-server-side-errors"></a>Compilação e erros do lado do servidor

 A seção a seguir contém possíveis soluções para erros de tempo de execução do compilador e do servidor. 

### <a name="reference-to-hub-instance-is-null"></a>A referência à instância de Hub é nula

Como uma instância de Hub é criada para cada conexão, você mesmo não pode criar uma instância de um Hub no seu código. Para chamar métodos em um hub de fora do próprio Hub, consulte [como chamar métodos de cliente e gerenciar grupos de fora da classe Hub](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub) para saber como obter uma referência ao contexto do Hub.

### <a name="httpcontextcurrentsession-is-null"></a>HTTPContext. Current. Session é nulo

Este comportamento ocorre por design. O signalr não dá suporte ao estado de sessão ASP.NET, pois habilitar o estado de sessão quebraria mensagens duplex.

### <a name="no-suitable-method-to-override"></a>Nenhum método adequado para substituir

Você poderá ver esse erro se estiver usando código de documentação ou Blogs mais antigos. Verifique se você não está referenciando nomes de métodos que foram alterados ou preteridos (como `OnConnectedAsync`).

### <a name="hostcontextextensionswebsocketserverurl-is-null"></a>HostContextExtensions. WebSocketServerUrl é nulo

Este comportamento ocorre por design. Este membro foi preterido e não deve ser usado.

### <a name="a-route-named-signalrhubs-is-already-in-the-route-collection-error"></a>"Uma rota chamada ' signalr. hubs ' já está no erro de coleção de rotas"

Esse erro será visto se `MapHubs` for chamado duas vezes pelo seu aplicativo. Alguns aplicativos de exemplo chamam `MapHubs` diretamente no arquivo de aplicativo global; outros fazem a chamada em uma classe wrapper. Certifique-se de que seu aplicativo não faça ambos.

<a id="vs"></a>

## <a name="visual-studio-issues"></a>Problemas do Visual Studio

Esta seção descreve os problemas encontrados no Visual Studio.

### <a name="script-documents-node-does-not-appear-in-solution-explorer"></a>O nó de documentos de script não aparece no Gerenciador de Soluções

Alguns dos nossos tutoriais direcionam você ao nó "documentos de script" no Gerenciador de Soluções durante a depuração. Esse nó é produzido pelo depurador do JavaScript e só aparecerá durante a depuração de clientes do navegador no Internet Explorer; o nó não será exibido se o Chrome ou Firefox forem usados. O depurador do JavaScript também não será executado se outro depurador de cliente estiver em execução, como o depurador do Silverlight.

### <a name="signalr-does-not-work-on-visual-studio-2008-or-earlier"></a>O signalr não funciona no Visual Studio 2008 ou anterior

Este comportamento ocorre por design. O signalr requer .NET Framework 4 ou posterior; Isso exige que os aplicativos Signalr sejam desenvolvidos no Visual Studio 2010 ou posterior.

<a id="iis"></a>

## <a name="iis-issues"></a>Problemas do IIS

Esta seção contém problemas com o Serviços de Informações da Internet.

### <a name="web-site-crashes-after-maphubs-call"></a>O site falha após a chamada MapHubs

Esse problema foi corrigido na versão mais recente do Signalr. Verifique se você está usando a versão mais recente do Signalr, atualizando sua instalação usando o NuGet.

<a id="azure"></a>

## <a name="azure-issues"></a>Problemas do Azure

Esta seção contém problemas com o Microsoft Azure.

### <a name="messages-are-not-received-through-the-azure-backplane-after-altering-topic-names"></a>As mensagens não são recebidas pelo backplane do Azure após a alteração dos nomes de tópico

Os tópicos usados pelo Azure backplane não se destinam a ser configuráveis pelo usuário.
