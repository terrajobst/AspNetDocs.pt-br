---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
title: Convenções de roteamento no ASP.NET Web API 2 OData-ASP.NET 4. x
author: MikeWasson
description: Descreve as convenções de roteamento que a API da Web 2 no ASP.NET 4. x usa para pontos de extremidade OData.
ms.author: riande
ms.date: 07/31/2013
ms.custom: seoapril2019
ms.assetid: adbc175a-14eb-4ab2-a441-d056ffa8266f
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
msc.type: authoredcontent
ms.openlocfilehash: 63df4a82cd8df92631485b2544117844cfd0ca56
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614712"
---
# <a name="routing-conventions-in-aspnet-web-api-2-odata"></a>Convenções de roteamento no OData ASP.NET Web API 2

por [Mike Wasson](https://github.com/MikeWasson)

> Este artigo descreve as convenções de roteamento que a API da Web 2 no ASP.NET 4. x usa para pontos de extremidade OData.

Quando a API da Web recebe uma solicitação OData, ela mapeia a solicitação para um nome de controlador e um nome de ação. O mapeamento é baseado no método HTTP e no URI. Por exemplo, `GET /odata/Products(1)` é mapeado para `ProductsController.GetProduct`.

Na parte 1 deste artigo, descrevo as convenções internas de roteamento do OData. Essas convenções são projetadas especificamente para pontos de extremidade OData e substituem o sistema de roteamento da API Web padrão. (A substituição ocorre quando você chama **MapODataRoute**.)

Na parte 2, eu mostro como adicionar convenções de roteamento personalizadas. Atualmente, as convenções internas não abrangem todo o intervalo de URIs OData, mas você pode estendê-las para lidar com casos adicionais.

- [Convenções de roteamento internas](#conventions)
- [Convenções de roteamento personalizadas](#custom)

<a id="conventions"></a>
## <a name="built-in-routing-conventions"></a>Convenções de roteamento internas

Antes de descrever as convenções de roteamento do OData na API Web, é útil entender os URIs do OData. Um [URI do OData](http://www.odata.org/documentation/odata-v3-documentation/url-conventions/) consiste em:

- A raiz do serviço
- O caminho do recurso
- Opções de consulta

![](odata-routing-conventions/_static/image1.png)

Para o roteamento, a parte importante é o caminho do recurso. O caminho do recurso é dividido em segmentos. Por exemplo, `/Products(1)/Supplier` tem três segmentos:

- `Products` se refere a um conjunto de entidades chamado "produtos".
- `1` é uma chave de entidade, selecionando uma única entidade do conjunto.
- `Supplier` é uma propriedade de navegação que seleciona uma entidade relacionada.

Portanto, esse caminho escolhe o fornecedor do produto 1.

> [!NOTE]
> Os segmentos de caminho OData nem sempre correspondem aos segmentos de URI. Por exemplo, "1" é considerado um segmento de caminho.

**Nomes de controlador.** O nome do controlador é sempre derivado do conjunto de entidades na raiz do caminho do recurso. Por exemplo, se o caminho do recurso for `/Products(1)/Supplier`, a API Web procurará um controlador chamado `ProductsController`.

**Nomes de ação.** Os nomes de ação são derivados dos segmentos de caminho mais o EDM (modelo de dados de entidade), conforme listado nas tabelas a seguir. Em alguns casos, você tem duas opções para o nome da ação. Por exemplo, "Get" ou &quot;GetProducts&quot;.

**Consultando entidades**

| Solicitação | URI de exemplo | Nome da ação | Exemplo de ação |
| --- | --- | --- | --- |
| OBTER/EntitySet | /Products | Getentityset ou Get | GetProducts |
| OBTER/EntitySet (chave) | /Products (1) | Getentitytype ou Get | Getproduct |
| OBTER/EntitySet (chave)/Cast | /Products (1)/Models.Book | Getentitytype ou Get | GetBook |

Para obter mais informações, consulte [criar um ponto de extremidade OData somente leitura](odata-v3/creating-an-odata-endpoint.md).

**Criando, atualizando e excluindo entidades**

| Solicitação | URI de exemplo | Nome da ação | Exemplo de ação |
| --- | --- | --- | --- |
| POSTAR/EntitySet | /Products | Createentitytype ou post | Produto |
| COLOCAR/EntitySet (chave) | /Products (1) | PutEntityType ou put | PutProduct |
| COLOCAR/EntitySet (chave)/Cast | /Products (1)/Models.Book | PutEntityType ou put | PutBook |
| PATCH/EntitySet (chave) | /Products (1) | PatchEntityType ou patch | PatchProduct |
| PATCH/EntitySet (Key)/Cast | /Products (1)/Models.Book | PatchEntityType ou patch | PatchBook |
| EXCLUIR/EntitySet (chave) | /Products (1) | DeleteEntityType ou Delete | DeleteProduct |
| EXCLUIR/EntitySet (chave)/Cast | /Products (1)/Models.Book | DeleteEntityType ou Delete | DeleteBook |

**Consultando uma propriedade de navegação**

| Solicitação | URI de exemplo | Nome da ação | Exemplo de ação |
| --- | --- | --- | --- |
| OBTER/EntitySet (chave)/Navigation | /Products (1)/Supplier | GetNavigationFromEntityType ou getnavegation | GetSupplierFromProduct |
| OBTER/EntitySet (chave)/Cast/Navigation | /Products (1)/Models.Book/Author | GetNavigationFromEntityType ou getnavegation | GetAuthorFromBook |

Para obter mais informações, consulte [trabalhando com relações de entidade](odata-v3/working-with-entity-relations.md).

**Criando e excluindo links**

| Solicitação | URI de exemplo | Nome da ação |
| --- | --- | --- |
| POST/EntitySet (chave)/$links/Navigation | /Products (1)/$links/Supplier | CreateLink |
| COLOCAR/EntitySet (Key)/$links/Navigation | /Products (1)/$links/Supplier | CreateLink |
| EXCLUIR/EntitySet (chave)/$links/Navigation | /Products (1)/$links/Supplier | DeleteLink |
| DELETE /entityset(key)/$links/navigation(relatedKey) | /Products/(1)/$links/suppliers (1) | DeleteLink |

Para obter mais informações, consulte [trabalhando com relações de entidade](odata-v3/working-with-entity-relations.md).

**Propriedades**

*Requer API Web 2*

| Solicitação | URI de exemplo | Nome da ação | Exemplo de ação |
| --- | --- | --- | --- |
| OBTER/EntitySet (chave)/Property | /Products (1)/Name | GetPropertyFromEntityType ou GetProperty | GetNameFromProduct |
| OBTER/EntitySet (chave)/Cast/Property | /Products (1)/Models.Book/Author | GetPropertyFromEntityType ou GetProperty | GetTitleFromBook |

**Ações**

| Solicitação | URI de exemplo | Nome da ação | Exemplo de ação |
| --- | --- | --- | --- |
| POST/EntitySet (chave)/Action | /Products (1)/Rate | ActionNameOnEntityType ou ActionName | RateOnProduct |
| POST/EntitySet (chave)/Cast/Action | /Products(1)/Models.Book/CheckOut | ActionNameOnEntityType ou ActionName | CheckOutOnBook |

Para obter mais informações, consulte [ações do OData](odata-v3/odata-actions.md).

**Assinaturas de método**

Aqui estão algumas regras para as assinaturas de método:

- Se o caminho contiver uma chave, a ação deverá ter um parâmetro chamado *Key*.
- Se o caminho contiver uma chave em uma propriedade de navegação, a ação deverá ter um parâmetro chamado *relatedKey*.
- Decorar os parâmetros *Key* e *relatedKey* com o parâmetro **[FromODataUri]** .
- As solicitações POST e PUT usam um parâmetro do tipo de entidade.
- As solicitações de PATCH assumem um parâmetro do tipo **Delta&lt;t&gt;** , em que *t* é o tipo de entidade.

Para referência, aqui está um exemplo que mostra as assinaturas de método para cada convenção interna de roteamento OData.

[!code-csharp[Main](odata-routing-conventions/samples/sample1.cs)]

<a id="custom"></a>
## <a name="custom-routing-conventions"></a>Convenções de roteamento personalizadas

Atualmente, as convenções internas não abrangem todos os URIs OData possíveis. Você pode adicionar novas convenções implementando a interface **IODataRoutingConvention** . Essa interface tem dois métodos:

[!code-csharp[Main](odata-routing-conventions/samples/sample2.cs)]

- **SelectController** retorna o nome do controlador.
- **SelectAction** retorna o nome da ação.

Para ambos os métodos, se a Convenção não se aplicar a essa solicitação, o método deverá retornar NULL.

O parâmetro **ODataPath** representa o caminho de recurso OData analisado. Ele contém uma lista de instâncias de **[ODataPathSegment](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatapathsegment.aspx)** , uma para cada segmento do caminho do recurso. **ODataPathSegment** é uma classe abstrata; cada tipo de segmento é representado por uma classe derivada de **ODataPathSegment**.

A propriedade **ODataPath. TemplatePath** é uma cadeia de caracteres que representa a concatenação de todos os segmentos de caminho. Por exemplo, se o URI for `/Products(1)/Supplier`, o modelo de caminho será &quot;~/EntitySet/Key/Navigation&quot;. Observe que os segmentos não correspondem diretamente aos segmentos de URI. Por exemplo, a chave de entidade (1) é representada como seu próprio **ODataPathSegment**.

Normalmente, uma implementação de **IODataRoutingConvention** faz o seguinte:

1. Compare o modelo de caminho para ver se essa convenção se aplica à solicitação atual. Se não for aplicável, retornará NULL.
2. Se a Convenção se aplicar, use as propriedades das instâncias **ODataPathSegment** para derivar os nomes do controlador e da ação.
3. Para ações, adicione quaisquer valores ao dicionário de rotas que devem ser associados aos parâmetros de ação (normalmente, chaves de entidade).

Vejamos um exemplo específico. As convenções de roteamento internas não dão suporte à indexação em uma coleção de navegação. Em outras palavras, não há nenhuma convenção para URIs como a seguinte:

[!code-javascript[Main](odata-routing-conventions/samples/sample3.js)]

Aqui está uma Convenção de roteamento personalizada para lidar com esse tipo de consulta.

[!code-csharp[Main](odata-routing-conventions/samples/sample4.cs)]

Observações:

1. Eu derivo de **EntitySetRoutingConvention**, pois o método **SelectController** nessa classe é apropriado para essa nova Convenção de roteamento. Isso significa que eu não preciso implementar novamente o **SelectController**.
2. A Convenção se aplica somente a solicitações GET e somente quando o modelo de caminho é &quot;~/EntitySet/Key/Navigation/Key&quot;.
3. O nome da ação é &quot;obter {EntityType}&quot;, em que *{EntityType}* é o tipo da coleção de navegação. Por exemplo, &quot;getsupplier&quot;. Você pode usar qualquer Convenção de nomenclatura que desejar &#8212; apenas para verificar se as ações do controlador correspondem.
4. A ação usa dois parâmetros chamados *Key* e *relatedKey*. (Para obter uma lista de alguns nomes de parâmetro predefinidos, consulte [ODataRouteConstants](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatarouteconstants.aspx).)

A próxima etapa é adicionar a nova Convenção à lista de convenções de roteamento. Isso ocorre durante a configuração, conforme mostrado no código a seguir:

[!code-csharp[Main](odata-routing-conventions/samples/sample5.cs)]

Aqui estão algumas outras convenções de roteamento de exemplo que são úteis para estudar:

- [CompositeKeyRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataCompositeKeySample/ODataCompositeKeySample/Extensions/CompositeKeyRoutingConvention.cs)
- [CustomNavigationRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataServiceSample/ODataService/Extensions/CustomNavigationRoutingConvention.cs)
- [NonBindableActionRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataActionsSample/ODataActionsSample/NonBindableActionRoutingConvention.cs)
- [ODataVersionRouteConstraint](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataVersioningSample/ODataVersioningSample/Extensions/ODataVersionRouteConstraint.cs)

E, naturalmente, a própria API da Web é de código-fonte aberto, para que você possa ver o [código-fonte](http://aspnetwebstack.codeplex.com/) das convenções de roteamento internas. Eles são definidos no namespace **System. Web. http. OData. Routing. Conventions** .
