---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: Chamando um serviço OData de um cliente .NET (C#) | Microsoft Docs
author: MikeWasson
description: Este tutorial mostra como chamar um serviço OData de um C# aplicativo cliente. Versões de software usadas no tutorial Visual Studio 2013 (funciona com Visual S...
ms.author: riande
ms.date: 02/26/2014
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 6a289fcb843634eeeefef1e0767e04e0be8b6973
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614677"
---
# <a name="calling-an-odata-service-from-a-net-client-c"></a>Chamar um serviço OData em um cliente .NET (C#)

por [Mike Wasson](https://github.com/MikeWasson)

[Baixar projeto concluído](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> Este tutorial mostra como chamar um serviço OData de um C# aplicativo cliente.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) (funciona com o Visual Studio 2012)
> - [WCF Data Services Client Library](https://msdn.microsoft.com/library/cc668772.aspx) (Biblioteca de clientes do WCF Data Services)
> - API Web 2. (O exemplo de serviço OData é criado usando a API da Web 2, mas o aplicativo cliente não depende da API da Web.)

Neste tutorial, examinarei a criação de um aplicativo cliente que chama um serviço OData. O serviço OData expõe as seguintes entidades:

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

Os artigos a seguir descrevem como implementar o serviço OData na API da Web. (No entanto, você não precisa lê-los para entender este tutorial.)

- [Criando um ponto de extremidade OData na API Web 2](creating-an-odata-endpoint.md)
- [Relações de entidade OData na API Web 2](working-with-entity-relations.md)
- [Ações de OData na API Web 2](odata-actions.md)

## <a name="generate-the-service-proxy"></a>Gerar o proxy de serviço

A primeira etapa é gerar um proxy de serviço. O proxy de serviço é uma classe .NET que define métodos para acessar o serviço OData. O proxy traduz as chamadas de método em solicitações HTTP.

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

Comece abrindo o projeto de serviço OData no Visual Studio. Pressione CTRL + F5 para executar o serviço localmente no IIS Express. Observe o endereço local, incluindo o número da porta que o Visual Studio atribui. Você precisará desse endereço ao criar o proxy.

Em seguida, abra outra instância do Visual Studio e crie um projeto de aplicativo de console. O aplicativo de console será nosso aplicativo cliente OData. (Você também pode adicionar o projeto à mesma solução que o serviço.)

> [!NOTE]
> As etapas restantes referem-se ao projeto de console.

Em Gerenciador de Soluções, clique com o botão direito do mouse em **referências** e selecione **Adicionar referência de serviço**.

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

Na caixa de diálogo **Adicionar referência de serviço** , digite o endereço do serviço OData:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

em que *Port* é o número da porta.

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

Para **namespace**, digite "ProductService". Essa opção define o namespace da classe proxy.

Clique em **Ir**. O Visual Studio lê o documento de metadados OData para descobrir as entidades no serviço.

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

Clique em **OK** para adicionar a classe de proxy ao seu projeto.

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a>Criar uma instância da classe de proxy de serviço

Dentro de seu método `Main`, crie uma nova instância da classe proxy, da seguinte maneira:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

Novamente, use o número da porta real em que o serviço está em execução. Ao implantar seu serviço, você usará o URI do serviço ativo. Você não precisa atualizar o proxy.

O código a seguir adiciona um manipulador de eventos que imprime os URIs de solicitação na janela do console. Essa etapa não é necessária, mas é interessante ver os URIs de cada consulta.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a>Consultar o serviço

O código a seguir obtém a lista de produtos do serviço OData.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

Observe que você não precisa escrever nenhum código para enviar a solicitação HTTP ou analisar a resposta. A classe proxy faz isso automaticamente quando você enumera a coleção de `Container.Products` no loop **foreach** .

Quando você executa o aplicativo, a saída deve ser semelhante à seguinte:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

Para obter uma entidade por ID, use uma cláusula `where`.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

Para o restante deste tópico, não mostrarei toda a função de `Main`, apenas o código necessário para chamar o serviço.

## <a name="apply-query-options"></a>Aplicar opções de consulta

O OData define [Opções de consulta](../supporting-odata-query-options.md) que podem ser usadas para filtrar, classificar, paginar dados e assim por diante. No proxy de serviço, você pode aplicar essas opções usando várias expressões LINQ.

Nesta seção, mostrarei exemplos breves. Para obter mais detalhes, consulte o tópico [Considerações sobre LINQ (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) no msdn.

### <a name="filtering-filter"></a>Filtragem ($filter)

Para filtrar, use uma cláusula `where`. O exemplo a seguir filtra por categoria de produto.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

Esse código corresponde à consulta OData a seguir.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

Observe que o proxy converte a cláusula `where` em uma expressão de `$filter` OData.

### <a name="sorting-orderby"></a>Classificação ($orderby)

Para classificar, use uma cláusula `orderby`. O exemplo a seguir classifica por preço, do mais alto para o mais baixo.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

Aqui está a solicitação de OData correspondente.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a>Paginação do lado do cliente ($skip e $top)

Para conjuntos de entidades grandes, o cliente pode querer limitar o número de resultados. Por exemplo, um cliente pode mostrar 10 entradas por vez. Isso é chamado *de paginação do lado do cliente*. (Também há uma [paginação do servidor](../supporting-odata-query-options.md#server-paging), onde o servidor limita o número de resultados.) Para executar a paginação do lado do cliente, use os métodos **Skip** e **Take** do LINQ. O exemplo a seguir ignora os primeiros 40 resultados e usa os próximos 10.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

Aqui está a solicitação de OData correspondente:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a>Select ($select) e Expand ($expand)

Para incluir entidades relacionadas, use o método `DataServiceQuery<t>.Expand`. Por exemplo, para incluir o `Supplier` para cada `Product`:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

Aqui está a solicitação de OData correspondente:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

Para alterar a forma da resposta, use a cláusula **Select** do LINQ. O exemplo a seguir obtém apenas o nome de cada produto, sem outras propriedades.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

Aqui está a solicitação de OData correspondente:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

Uma cláusula SELECT pode incluir entidades relacionadas. Nesse caso, não chame **expandir**; o proxy inclui automaticamente a expansão nesse caso. O exemplo a seguir obtém o nome e o fornecedor de cada produto.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

Aqui está a solicitação de OData correspondente. Observe que ele inclui a opção **$Expand** .

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

Para obter mais informações sobre $select e $expand, consulte [usando $Select, $Expand e $value na API Web 2](../using-select-expand-and-value.md).

## <a name="add-a-new-entity"></a>Adicionar uma nova entidade

Para adicionar uma nova entidade a um conjunto de entidades, chame `AddToEntitySet`, em que *EntitySet* é o nome do conjunto de entidades. Por exemplo, `AddToProducts` adiciona um novo `Product` ao conjunto de entidades de `Products`. Quando você gera o proxy, WCF Data Services cria automaticamente esses métodos **AddTo** fortemente tipados.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

Para adicionar um link entre duas entidades, use os métodos **AddLink** e **SetLink** . O código a seguir adiciona um novo fornecedor e um novo produto e, em seguida, cria links entre eles.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

Use **AddLink** quando a propriedade de navegação for uma coleção. Neste exemplo, estamos adicionando um produto à coleção de `Products` no fornecedor.

Use **SetLink** quando a propriedade de navegação for uma única entidade. Neste exemplo, estamos definindo a propriedade `Supplier` no produto.

## <a name="update--patch"></a>Atualização/patch

Para atualizar uma entidade, chame o método **UpdateObject** .

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

A atualização é executada quando você chama **SaveChanges**. Por padrão, o WCF envia uma solicitação de MESCLAgem HTTP. A opção **PatchOnUpdate** informa ao WCF para enviar um patch http em vez disso.

> [!NOTE]
> Por que PATCH versus MESCLAgem? A especificação HTTP 1,1 original ([RCF 2616](http://tools.ietf.org/html/rfc2616)) não definiu nenhum método http com a semântica "atualização parcial". Para dar suporte a atualizações parciais, a especificação OData definiu o método MERGE. Em 2010, a [RFC 5789](http://tools.ietf.org/html/rfc5789) definiu o método de patch para atualizações parciais. Você pode ler parte do histórico nesta [postagem de blog](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) no blog WCF Data Services. Hoje, o PATCH é preferencial em relação à MESCLAgem. O controlador OData criado pela API Web scaffolding dá suporte a ambos os métodos.

Se você quiser substituir toda a entidade (colocar semântica), especifique a opção **ReplaceOnUpdate** . Isso faz com que o WCF envie uma solicitação HTTP PUT.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a>Excluir uma entidade

Para excluir uma entidade, chame **ExcluirObjeto**.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a>Invocar uma ação do OData

No OData, as [ações](odata-actions.md) são uma maneira de adicionar comportamentos do lado do servidor que não são facilmente definidos como operações CRUD em entidades.

Embora o documento de metadados OData Descreva as ações, a classe proxy não cria nenhum método fortemente tipado para elas. Você ainda pode invocar uma ação OData usando o método genérico **Execute** . No entanto, você precisará saber os tipos de dados dos parâmetros e o valor de retorno.

Por exemplo, a ação `RateProduct` usa o parâmetro chamado "rating" do tipo `Int32` e retorna um `double`. O código a seguir mostra como invocar essa ação.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

Para obter mais informações, consulte[chamando operações e ações de serviço](https://msdn.microsoft.com/library/hh230677.aspx).

Uma opção é estender a classe de **contêiner** para fornecer um método fortemente tipado que invoque a ação:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
