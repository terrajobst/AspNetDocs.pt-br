---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: Roteamento de atributos no ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 7da1805d8a7066e82743dc9bd7e024cc9813ee89
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554981"
---
# <a name="attribute-routing-in-aspnet-web-api-2"></a>Roteamento de atributos no ASP.NET Web API 2

por [Mike Wasson](https://github.com/MikeWasson)

O *Roteamento* é como a API da Web corresponde a um URI para uma ação. A API da Web 2 dá suporte a um novo tipo de roteamento, chamado *Roteamento de atributos*. Como o nome indica, o roteamento de atributo usa atributos para definir rotas. O roteamento de atributos oferece mais controle sobre os URIs em sua API Web. Por exemplo, você pode criar facilmente URIs que descrevem hierarquias de recursos.

O estilo anterior de roteamento, chamado de roteamento baseado em Convenção, ainda é totalmente suportado. Na verdade, você pode combinar as duas técnicas no mesmo projeto.

Este tópico mostra como habilitar o roteamento de atributos e descreve as várias opções para o roteamento de atributos. Para obter um tutorial de ponta a ponta que usa o roteamento de atributos, consulte [criar uma API REST com roteamento de atributos na API Web 2](create-a-rest-api-with-attribute-routing.md).

## <a name="prerequisites"></a>Prerequisites

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional ou Enterprise Edition

Como alternativa, use o Gerenciador de pacotes NuGet para instalar os pacotes necessários. No menu **ferramentas** no Visual Studio, selecione **Gerenciador de pacotes NuGet**e, em seguida, selecione **console do Gerenciador de pacotes**. Digite o seguinte comando na janela do console do Gerenciador de pacotes:

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a>Por que o roteamento de atributos?

A primeira versão da API Web usada *pelo roteamento baseado em Convenção* . Nesse tipo de roteamento, você define um ou mais modelos de rota, que são basicamente cadeias de caracteres parametrizadas. Quando a estrutura recebe uma solicitação, ela corresponde ao URI em relação ao modelo de rota. (Para obter mais informações sobre roteamento baseado em Convenção, consulte [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).

Uma vantagem do roteamento baseado em Convenção é que os modelos são definidos em um único local e as regras de roteamento são aplicadas consistentemente em todos os controladores. Infelizmente, o roteamento baseado em convenção torna difícil dar suporte a determinados padrões de URI que são comuns em APIs RESTful. Por exemplo, os recursos geralmente contêm recursos filho: os clientes têm pedidos, filmes têm atores, livros têm autores e assim por diante. É natural criar URIs que reflitam essas relações:

`/customers/1/orders`

Esse tipo de URI é difícil de criar usando o roteamento baseado em convenção. Embora isso possa ser feito, os resultados não são bem dimensionados se você tiver muitos controladores ou tipos de recursos.

Com o roteamento de atributos, é trivial definir uma rota para esse URI. Basta adicionar um atributo à ação do controlador:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

Aqui estão alguns outros padrões que o roteamento de atributos facilita.

**Controle de versão de API**

Neste exemplo, "/API/v1/Products" seria roteado para um controlador diferente de "/API/v2/Products".

`/api/v1/products`
`/api/v2/products`

**Segmentos de URI sobrecarregados**

Neste exemplo, "1" é um número de pedido, mas "pendente" é mapeado para uma coleção.

`/orders/1`
`/orders/pending`

**Vários tipos de parâmetro**

Neste exemplo, "1" é um número de pedido, mas "2013/06/16" especifica uma data.

`/orders/1`
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a>Habilitando o roteamento de atributos

Para habilitar o roteamento de atributos, chame **MapHttpAttributeRoutes** durante a configuração. Esse método de extensão é definido na classe **System. Web. http. HttpConfigurationExtensions** .

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

O roteamento de atributos pode ser combinado com roteamento [baseado em Convenção](routing-in-aspnet-web-api.md) . Para definir rotas baseadas em Convenção, chame o método **MapHttpRoute** .

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

Para obter mais informações sobre como configurar a API Web, consulte [Configurando o ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a>Observação: migrando da API Web 1

Antes da API Web 2, os modelos de projeto de API Web geraram código como este:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

Se o roteamento de atributos estiver habilitado, esse código gerará uma exceção. Se você atualizar um projeto de API Web existente para usar o roteamento de atributos, atualize esse código de configuração para o seguinte:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> Para obter mais informações, consulte [Configurando a API Web com hospedagem ASP.net](../advanced/configuring-aspnet-web-api.md#webhost).

<a id="add-routes"></a>
## <a name="adding-route-attributes"></a>Adicionando atributos de rota

Aqui está um exemplo de uma rota definida usando um atributo:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

A cadeia de caracteres &quot;Customers/{customerId}/Orders&quot; é o modelo de URI para a rota. A API Web tenta corresponder o URI de solicitação ao modelo. Neste exemplo, "Customers" e "Orders" são segmentos literais e "{customerId}" é um parâmetro de variável. Os seguintes URIs corresponderão a este modelo:

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

Você pode restringir a correspondência usando [restrições](#constraints), descritas posteriormente neste tópico.

Observe que o parâmetro &quot;{customerId}&quot; no modelo de rota corresponde ao nome do parâmetro *CustomerID* no método. Quando a API da Web chama a ação do controlador, ela tenta associar os parâmetros de rota. Por exemplo, se o URI for `http://example.com/customers/1/orders`, a API Web tentará associar o valor "1" ao parâmetro *CustomerID* na ação.

Um modelo de URI pode ter vários parâmetros:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

Qualquer método de controlador que não tenha um atributo de rota usa roteamento baseado em convenção. Dessa forma, você pode combinar os dois tipos de roteamento no mesmo projeto.

## <a name="http-methods"></a>Métodos HTTP

A API Web também seleciona ações com base no método HTTP da solicitação (GET, POST etc.). Por padrão, a API Web procura uma correspondência que não diferencia maiúsculas de minúsculas com o início do nome do método do controlador. Por exemplo, um método de controlador chamado `PutCustomers` corresponde a uma solicitação HTTP PUT.

Você pode substituir essa Convenção decorando o método com qualquer um dos seguintes atributos:

- **HttpDelete**
- **[HttpGet]**
- **[HttpHead]**
- **[Httpoptions]**
- **[HttpPatch]**
- **HttpPost**
- **HttpPut**

O exemplo a seguir mapeia o método createbook para solicitações HTTP POST.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

Para todos os outros métodos HTTP, incluindo métodos não padrão, use o atributo **AcceptVerbs** , que usa uma lista de métodos http.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a>Prefixos de rota

Muitas vezes, as rotas em um controlador começam com o mesmo prefixo. Por exemplo:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

Você pode definir um prefixo comum para um controlador inteiro usando o atributo **[RoutePrefix]** :

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

Use um til (~) no atributo Method para substituir o prefixo de rota:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

O prefixo de rota pode incluir parâmetros:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a>Restrições de rota

As restrições de rota permitem restringir como os parâmetros no modelo de rota são correspondidos. A sintaxe geral é &quot;{Parameter: Constraint}&quot;. Por exemplo:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

Aqui, a primeira rota só será selecionada se a ID de &quot;&quot; segmento do URI for um número inteiro. Caso contrário, a segunda rota será escolhida.

A tabela a seguir lista as restrições com suporte.

| Constraint | DESCRIÇÃO | Exemplo |
| --- | --- | --- |
| alfa | Corresponde a caracteres de alfabeto latino maiúsculos ou minúsculos (a-z, A-Z) | {x:alpha} |
| bool | Corresponde a um valor booliano. | {x:bool} |
| DATETIME | Corresponde a um valor **DateTime** . | {x:datetime} |
| decimal | Corresponde a um valor decimal. | {x:decimal} |
| double | Corresponde a um valor de ponto flutuante de 64 bits. | {x:double} |
| FLOAT | Corresponde a um valor de ponto flutuante de 32 bits. | {x:float} |
| guid | Corresponde a um valor de GUID. | {x:guid} |
| INT | Corresponde a um valor inteiro de 32 bits. | {x:int} |
| comprimento | Corresponde a uma cadeia de caracteres com o comprimento especificado ou dentro de um intervalo especificado de comprimentos. | {x:length(6)} {x:length(1,20)} |
| long | Corresponde a um valor inteiro de 64 bits. | {x:long} |
| max | Faz a correspondência de um inteiro com um valor máximo. | {x:max(10)} |
| determinado | Corresponde a uma cadeia de caracteres com um comprimento máximo. | {x:maxlength(10)} |
| Min | Faz a correspondência de um inteiro com um valor mínimo. | {x:min(10)} |
| minLength | Corresponde a uma cadeia de caracteres com um comprimento mínimo. | {x:minlength(10)} |
| range | Corresponde a um inteiro dentro de um intervalo de valores. | {x:range(10,50)} |
| regex | Corresponde a uma expressão regular. | {x:Regex (^ \d{3}-\d{3}-\d{4}$)} |

Observe que algumas das restrições, como &quot;min&quot;, levam argumentos entre parênteses. Você pode aplicar várias restrições a um parâmetro, separados por dois-pontos.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a>Restrições de rota personalizadas

Você pode criar restrições de rotas personalizadas implementando a interface **IHttpRouteConstraint** . Por exemplo, a restrição a seguir restringe um parâmetro a um valor inteiro diferente de zero.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

O código a seguir mostra como registrar a restrição:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

Agora você pode aplicar a restrição em suas rotas:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

Você também pode substituir toda a classe **DefaultInlineConstraintResolver** implementando a interface **IInlineConstraintResolver** . Isso substituirá todas as restrições internas, a menos que sua implementação do **IInlineConstraintResolver** as adicione especificamente.

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a>Parâmetros de URI opcionais e valores padrão

Você pode tornar um parâmetro de URI opcional adicionando um ponto de interrogação ao parâmetro de rota. Se um parâmetro de rota for opcional, você deverá definir um valor padrão para o parâmetro do método.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

Neste exemplo, `/api/books/locale/1033` e `/api/books/locale` retornam o mesmo recurso.

Como alternativa, você pode especificar um valor padrão dentro do modelo de rota, da seguinte maneira:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

Isso é quase o mesmo que o exemplo anterior, mas há uma pequena diferença de comportamento quando o valor padrão é aplicado.

- No primeiro exemplo ("{LCID: int?}"), o valor padrão de 1033 é atribuído diretamente ao parâmetro do método, de modo que o parâmetro terá esse valor exato.
- No segundo exemplo ("{LCID: int = 1033}"), o valor padrão de "1033" passa pelo processo de vinculação de modelo. O modelo-associador padrão converterá "1033" para o valor numérico 1033. No entanto, você pode conectar um associador de modelo personalizado, o que pode fazer algo diferente.

(Na maioria dos casos, a menos que você tenha ASSOCIADORES de modelo personalizados em seu pipeline, os dois formulários serão equivalentes.)

<a id="route-names"></a>
## <a name="route-names"></a>Nomes de rota

Na API Web, cada rota tem um nome. Os nomes de rota são úteis para a geração de links, para que você possa incluir um link em uma resposta HTTP.

Para especificar o nome da rota, defina a propriedade **Name** no atributo. O exemplo a seguir mostra como definir o nome da rota e também como usar o nome da rota ao gerar um link.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a>Ordem de rota

Quando a estrutura tenta corresponder a um URI com uma rota, ela avalia as rotas em uma ordem específica. Para especificar a ordem, defina a propriedade **Order** no atributo Route. Os valores mais baixos são avaliados primeiro. O valor da ordem padrão é zero.

Veja como a ordenação total é determinada:

1. Compare a propriedade **Order** do atributo Route.
2. Examine cada segmento URI no modelo de rota. Para cada segmento, Order da seguinte maneira:

    1. Segmentos literais.
    2. Rotear parâmetros com restrições.
    3. Parâmetros de rota sem restrições.
    4. Segmentos de parâmetro curinga com restrições.
    5. Segmentos de parâmetro curinga sem restrições.
3. No caso de um empate, as rotas são ordenadas por uma[OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)(comparação de cadeia de caracteres ordinal) que não diferencia maiúsculas de minúsculas do modelo de rota.

Veja um exemplo. Suponha que você defina o controlador a seguir:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

Essas rotas são ordenadas da seguinte maneira.

1. pedidos/detalhes
2. Orders/{ID}
3. orders/{customerName}
4. Orders/{\*Date}
5. pedidos/pendentes

Observe que "Details" é um segmento literal e aparece antes de "{ID}", mas "Pending" aparece por último porque a propriedade **Order** é 1. (Este exemplo pressupõe que não há clientes chamados "detalhes" ou "pendentes". Em geral, tente evitar rotas ambíguas. Neste exemplo, um modelo de rota melhor para `GetByCustomer` é "Customers/{CustomerName}")
