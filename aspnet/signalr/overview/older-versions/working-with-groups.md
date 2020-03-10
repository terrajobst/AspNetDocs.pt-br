---
uid: signalr/overview/older-versions/working-with-groups
title: Trabalhando com grupos no Signalr 1. x | Microsoft Docs
author: bradygaster
description: Este tópico descreve como manter informações de associação de grupo com a API de Hub.
ms.author: bradyg
ms.date: 10/21/2013
ms.assetid: 22929efd-68c9-4609-b76d-f8ba42fda01e
msc.legacyurl: /signalr/overview/older-versions/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: 5f50dc162d6cdcfbf2261e6a751f5f99078d5c54
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579362"
---
# <a name="working-with-groups-in-signalr-1x"></a>Trabalhar com grupos no SignalR 1.x

por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Este tópico descreve como adicionar usuários a grupos e manter informações de associação de grupo.

## <a name="overview"></a>Visão geral

Os grupos no Signalr fornecem um método para transmitir mensagens para os subconjuntos especificados de clientes conectados. Um grupo pode ter qualquer número de clientes, e um cliente pode ser um membro de qualquer número de grupos. Você não precisa criar grupos explicitamente. Na verdade, um grupo é criado automaticamente na primeira vez que você especifica seu nome em uma chamada para groups. Add, e é excluído quando você remove a última conexão da associação. Para obter uma introdução ao uso de grupos, consulte [como gerenciar a associação de grupo da classe Hub](index.md) no guia da API de hubs-servidor.

Não há API para obter uma lista de associação de grupo ou uma lista de grupos. O signalr envia mensagens para clientes e grupos com base em um modelo pub/sub e o servidor não mantém listas de grupos ou associações de grupo. Isso ajuda a maximizar a escalabilidade, porque sempre que você adiciona um nó a um web farm, qualquer Estado que o Signalr mantém deve ser propagado para o novo nó.

Quando você adiciona um usuário a um grupo usando o método `Groups.Add`, o usuário recebe mensagens direcionadas para esse grupo durante a conexão atual, mas a associação do usuário nesse grupo não é mantida além da conexão atual. Se você quiser reter permanentemente informações sobre grupos e Associação de grupo, deverá armazenar esses dados em um repositório, como um banco de dado ou armazenamento de tabelas do Azure. Em seguida, cada vez que um usuário se conecta ao seu aplicativo, você recupera do repositório a que pertence o usuário e adiciona manualmente esse usuário a esses grupos.

Ao se reconectar após uma interrupção temporária, o usuário automaticamente reingressa os grupos atribuídos anteriormente. A rejunção automática de um grupo só se aplica ao se reconectar, não ao estabelecer uma nova conexão. Um token assinado digitalmente é passado do cliente que contém a lista de grupos atribuídos anteriormente. Se você quiser verificar se o usuário pertence aos grupos solicitados, você pode substituir o comportamento padrão.

Este tópico inclui as seções a seguir:

- [Adicionando e removendo usuários](#add)
- [Chamando membros de um grupo](#call)
- [Armazenando a associação de grupo em um banco de dados](#storedatabase)
- [Armazenando a associação de grupo no armazenamento de tabelas do Azure](#storeazuretable)
- [Verificando a associação de grupo ao reconectar](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a>Adicionando e removendo usuários

Para adicionar ou remover usuários de um grupo, você chama os métodos [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) ou [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) e passa o nome do grupo e a ID de conexão do usuário como parâmetros. Você não precisa remover manualmente um usuário de um grupo quando a conexão termina.

O exemplo a seguir mostra os métodos `Groups.Add` e `Groups.Remove` usados em métodos de Hub.

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

Os métodos `Groups.Add` e `Groups.Remove` são executados de forma assíncrona.

Se você quiser adicionar um cliente a um grupo e enviar imediatamente uma mensagem para o cliente usando o grupo, será necessário certificar-se de que o método groups. Add termine primeiro. Os exemplos de código a seguir mostram como fazer isso, um usando código que funciona no .NET 4,5 e um usando código que funciona no .NET 4.

#### <a name="asynchronous-net-45-example"></a>Exemplo do .NET 4,5 assíncrono

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

#### <a name="asynchronous-net-4-example"></a>Exemplo de .NET 4 assíncrono

[!code-csharp[Main](working-with-groups/samples/sample3.cs?highlight=3-4)]

Em geral, você não deve incluir `await` ao chamar o método `Groups.Remove` porque a ID de conexão que você está tentando remover pode não estar mais disponível. Nesse caso, `TaskCanceledException` é lançada depois que a solicitação atinge o tempo limite. Se seu aplicativo precisar garantir que o usuário tenha sido removido do grupo antes de enviar uma mensagem para o grupo, você poderá adicionar `await` antes de grupos. Remova e, em seguida, capturar a exceção `TaskCanceledException` que pode ser gerada.

<a id="call"></a>

## <a name="calling-members-of-a-group"></a>Chamando membros de um grupo

Você pode enviar mensagens para todos os membros de um grupo ou apenas membros especificados do grupo, conforme mostrado nos exemplos a seguir.

- **Todos** os clientes conectados em um grupo especificado. 

    [!code-css[Main](working-with-groups/samples/sample4.css)]
- Todos os clientes conectados em um grupo especificado, **exceto os clientes especificados**, identificados pela ID de conexão. 

    [!code-csharp[Main](working-with-groups/samples/sample5.cs)]
- Todos os clientes conectados em um grupo especificado **, exceto o cliente de chamada**. 

    [!code-css[Main](working-with-groups/samples/sample6.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a>Armazenando a associação de grupo em um banco de dados

Os exemplos a seguir mostram como manter informações de grupo e de usuário em um banco de dados. Você pode usar qualquer tecnologia de acesso a dados; no entanto, o exemplo a seguir mostra como definir modelos usando Entity Framework. Esses modelos de entidade correspondem a tabelas e campos de banco de dados. Sua estrutura de dados pode variar consideravelmente dependendo dos requisitos do seu aplicativo. Este exemplo inclui uma classe chamada `ConversationRoom` que seria exclusiva para um aplicativo que permite aos usuários ingressar em conversas sobre assuntos diferentes, como esportes ou jardinagem. Este exemplo também inclui uma classe para as conexões. A classe de conexão não é absolutamente necessária para controlar a associação de grupo, mas geralmente faz parte da solução robusta para controlar os usuários.

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

Em seguida, no Hub, você pode recuperar as informações do grupo e do usuário do banco de dados e adicionar manualmente o usuário aos grupos apropriados. O exemplo não inclui código para controlar as conexões de usuário. Neste exemplo, a palavra-chave `await` não é aplicada antes `Groups.Add` porque uma mensagem não é enviada imediatamente aos membros do grupo. Se você quiser enviar uma mensagem a todos os membros do grupo imediatamente após adicionar o novo membro, convém aplicar a palavra-chave `await` para certificar-se de que a operação assíncrona foi concluída.

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a>Armazenando a associação de grupo no armazenamento de tabelas do Azure

Usar o armazenamento de tabelas do Azure para armazenar informações de grupo e de usuário é semelhante ao uso de um banco de dados. O exemplo a seguir mostra uma entidade de tabela que armazena o nome de usuário e o nome do grupo.

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

No Hub, você recupera os grupos atribuídos quando o usuário se conecta.

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a>Verificando a associação de grupo ao reconectar

Por padrão, o Signalr automaticamente atribui novamente um usuário aos grupos apropriados ao se reconectar de uma interrupção temporária, como quando uma conexão é descartada e restabelecida antes que a conexão expire. As informações de grupo do usuário são passadas em um token durante a reconexão e esse token é verificado no servidor. Para obter informações sobre o processo de verificação para a rejunção de usuários a grupos, consulte [rejunção de grupos ao reconectar](index.md).

Em geral, você deve usar o comportamento padrão de reassociar automaticamente os grupos na reconexão. Os grupos de signalr não se destinam a um mecanismo de segurança para restringir o acesso a dados confidenciais. No entanto, se seu aplicativo precisar verificar a associação de grupo de um usuário ao reconectar-se, você poderá substituir o comportamento padrão. Alterar o comportamento padrão pode adicionar uma carga ao seu banco de dados porque a associação de grupo de um usuário deve ser recuperada para cada reconexão em vez de apenas quando o usuário se conecta.

Se você precisar verificar a associação de grupo ao reconectar, crie um novo módulo de pipeline de Hub que retorna uma lista de grupos atribuídos, conforme mostrado abaixo.

[!code-csharp[Main](working-with-groups/samples/sample11.cs)]

Em seguida, adicione esse módulo ao pipeline de Hub, conforme realçado abaixo.

[!code-csharp[Main](working-with-groups/samples/sample12.cs?highlight=10)]
