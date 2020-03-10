---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: Roteamento e seleção de ação no ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 12/14/2018
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: 62114e56fb29e80c93b82dcb78ce2bc2a123a83b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554883"
---
# <a name="routing-and-action-selection-in-aspnet-web-api"></a>Roteamento e seleção de ação no ASP.NET Web API

por [Mike Wasson](https://github.com/MikeWasson)

Este artigo descreve como ASP.NET Web API roteia uma solicitação HTTP para uma ação específica em um controlador.

> [!NOTE]
> Para obter uma visão geral de alto nível do roteamento, consulte [Roteamento no ASP.NET Web API](routing-in-aspnet-web-api.md).

Este artigo analisa os detalhes do processo de roteamento. Se você criar um projeto de API Web e achar que algumas solicitações não são roteadas da maneira esperada, espero que este artigo o ajude.

O roteamento tem três fases principais:

1. Correspondendo o URI a um modelo de rota.
2. Selecionando um controlador.
3. Selecionando uma ação.

Você pode substituir algumas partes do processo por seus próprios comportamentos personalizados. Neste artigo, descrevo o comportamento padrão. No final, observei os locais em que você pode personalizar o comportamento.

## <a name="route-templates"></a>Modelos de rota

Um modelo de rota é semelhante a um caminho de URI, mas pode ter valores de espaço reservado, indicado com chaves:

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

Ao criar uma rota, você pode fornecer valores padrão para alguns ou todos os espaços reservados:

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

Você também pode fornecer restrições, que restringem como um segmento URI pode corresponder a um espaço reservado:

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

A estrutura tenta corresponder os segmentos no caminho do URI para o modelo. Os literais no modelo devem corresponder exatamente. Um espaço reservado corresponde a qualquer valor, a menos que você especifique restrições. A estrutura não corresponde a outras partes do URI, como o nome do host ou os parâmetros de consulta. A estrutura seleciona a primeira rota na tabela de rotas que corresponde ao URI.

Há dois espaços reservados especiais: "{Controller}" e "{Action}".

- "{Controller}" fornece o nome do controlador.
- "{Action}" fornece o nome da ação. Na API Web, a convenção usual é omitir "{Action}".

### <a name="defaults"></a>Padrões

Se você fornecer padrões, a rota corresponderá a um URI que não tem esses segmentos. Por exemplo:

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

Os URIs `http://localhost/api/products/all` e `http://localhost/api/products` correspondem à rota anterior. No último URI, o valor padrão `all`é atribuído ao segmento de `{category}` ausente.

### <a name="route-dictionary"></a>Dicionário de rotas

Se a estrutura encontrar uma correspondência para um URI, ela criará um dicionário que contém o valor de cada espaço reservado. As chaves são os nomes de espaço reservado, não incluindo as chaves. Os valores são extraídos do caminho do URI ou dos padrões. O dicionário é armazenado no objeto **IHttpRouteData** .

Durante essa fase de correspondência de rota, os espaços reservados especiais "{Controller}" e "{Action}" são tratados exatamente como os outros espaços reservados. Eles são simplesmente armazenados no dicionário com os outros valores.

Um padrão pode ter o valor especial **RouteParameter. optional**. Se um espaço reservado receber esse valor, o valor não será adicionado ao dicionário de rota. Por exemplo:

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

Para o caminho do URI "API/produtos", o dicionário da rota conterá:

- controlador: "produtos"
- Categoria: "todos"

Para "API/produtos/Toys/123", no entanto, o dicionário de rota conterá:

- controlador: "produtos"
- Categoria: "brinquedos"
- id: "123"

Os padrões também podem incluir um valor que não aparece em nenhum lugar no modelo de rota. Se a rota corresponder, esse valor será armazenado no dicionário. Por exemplo:

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

Se o caminho do URI for "API/root/8", o dicionário conterá dois valores:

- controlador: "clientes"
- id: "8"

## <a name="selecting-a-controller"></a>Selecionando um controlador

A seleção do controlador é tratada pelo método **IHttpControllerSelector. SelectController** . Esse método usa uma instância **HttpRequestMessage** e retorna um **HttpControllerDescriptor**. A implementação padrão é fornecida pela classe **DefaultHttpControllerSelector** . Essa classe usa um algoritmo simples:

1. Examine o dicionário de rotas para a chave "Controller".
2. Pegue o valor dessa chave e acrescente a cadeia de caracteres "Controller" para obter o nome do tipo de controlador.
3. Procure um controlador de API da Web com este nome de tipo.

Por exemplo, se o dicionário de rotas contiver o par chave-valor "Controller" = "Products", o tipo de controlador será "ProductsController". Se não houver nenhum tipo correspondente ou várias correspondências, a estrutura retornará um erro ao cliente.

Para a etapa 3, **DefaultHttpControllerSelector** usa a interface **IHttpControllerTypeResolver** para obter a lista de tipos de controlador da API Web. A implementação padrão de **IHttpControllerTypeResolver** retorna todas as classes públicas que (a) implementam **IHttpController**, (b) não são abstratas e (c) têm um nome que termina em "Controller".

## <a name="action-selection"></a>Seleção de ação

Depois de selecionar o controlador, a estrutura seleciona a ação chamando o método **IHttpActionSelector. SelectAction** . Esse método usa um **HttpControllerContext** e retorna um **HttpActionDescriptor**.

A implementação padrão é fornecida pela classe **ApiControllerActionSelector** . Para selecionar uma ação, ela examina o seguinte:

- O método HTTP da solicitação.
- O espaço reservado "{Action}" no modelo de rota, se presente.
- Os parâmetros das ações no controlador.

Antes de examinar o algoritmo de seleção, precisamos entender algumas coisas sobre as ações do controlador.

**Quais métodos no controlador são considerados "ações"?** Ao selecionar uma ação, a estrutura examina apenas os métodos de instância pública no controlador. Além disso, ele exclui métodos de ["nome especial"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) (construtores, eventos, sobrecargas de operador e assim por diante) e métodos herdados da classe **ApiController** .

**Métodos HTTP.** A estrutura escolhe apenas as ações que correspondem ao método HTTP da solicitação, determinado da seguinte maneira:

1. Você pode especificar o método HTTP com um atributo: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**, **httpoptions**, **HttpPatch**, **HttpPost**ou **HttpPut**.
2. Caso contrário, se o nome do método do controlador começar com "Get", "post", "Put", "excluir", "Head", "Options" ou "patch", por convenção, a ação dará suporte a esse método HTTP.
3. Se não houver nenhum dos anteriores, o método dará suporte a POST.

**Associações de parâmetro.** Uma associação de parâmetro é como a API da Web cria um valor para um parâmetro. Aqui está a regra padrão para a associação de parâmetro:

- Os tipos simples são obtidos do URI.
- Os tipos complexos são extraídos do corpo da solicitação.

Os tipos simples incluem todos os [tipos primitivos .NET Framework](https://msdn.microsoft.com/library/system.type.isprimitive), mais **DateTime**, **decimal**, **GUID**, **cadeia de caracteres**e **TimeSpan**. Para cada ação, no máximo um parâmetro pode ler o corpo da solicitação.

> [!NOTE]
> É possível substituir as regras de associação padrão. Consulte [Associação de parâmetro WebAPI nos bastidores](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).

Com esse plano de fundo, aqui está o algoritmo de seleção de ação.

1. Crie uma lista de todas as ações no controlador que correspondem ao método de solicitação HTTP.
2. Se o dicionário de rotas tiver uma entrada "Action", remova as ações cujo nome não corresponde a esse valor.
3. Tente corresponder os parâmetros de ação ao URI, da seguinte maneira: 

    1. Para cada ação, obtenha uma lista dos parâmetros que são um tipo simples, em que a associação Obtém o parâmetro do URI. Exclua os parâmetros opcionais.
    2. Nessa lista, tente encontrar uma correspondência para cada nome de parâmetro, seja no dicionário de rota ou na cadeia de caracteres de consulta de URI. As correspondências não diferenciam maiúsculas de minúsculas e não dependem da ordem dos parâmetros.
    3. Selecione uma ação em que cada parâmetro na lista tenha uma correspondência no URI.
    4. Se mais de uma ação atender a esses critérios, escolha aquela com as correspondências mais parâmetro.
4. Ignore ações com o atributo **[NonAction]** .

A etapa #3 é provavelmente a mais confusa. A ideia básica é que um parâmetro pode obter seu valor a partir do URI, do corpo da solicitação ou de uma associação personalizada. Para parâmetros provenientes do URI, queremos garantir que o URI realmente contenha um valor para esse parâmetro, seja no caminho (por meio do dicionário de rota) ou na cadeia de caracteres de consulta.

Por exemplo, considere a seguinte ação:

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

O parâmetro *ID* é associado ao URI. Portanto, essa ação só pode corresponder a um URI que contém um valor para "ID", no dicionário de rota ou na cadeia de caracteres de consulta.

Os parâmetros opcionais são uma exceção, pois são opcionais. Para um parâmetro opcional, ele será OK se a associação não puder obter o valor do URI.

Tipos complexos são uma exceção por um motivo diferente. Um tipo complexo só pode ser associado ao URI por meio de uma associação personalizada. Mas nesse caso, a estrutura não pode saber com antecedência se o parâmetro se associaria a um URI específico. Para descobrir, seria necessário invocar a associação. A meta do algoritmo de seleção é selecionar uma ação da descrição estática, antes de invocar qualquer associação. Portanto, tipos complexos são excluídos do algoritmo de correspondência.

Depois que a ação é selecionada, todas as associações de parâmetro são invocadas.

Resumo:

- A ação deve corresponder ao método HTTP da solicitação.
- O nome da ação deve corresponder à entrada "ação" no dicionário de rota, se presente.
- Para cada parâmetro da ação, se o parâmetro for extraído do URI, o nome do parâmetro deverá ser encontrado no dicionário de rota ou na cadeia de caracteres de consulta do URI. (Parâmetros opcionais e parâmetros com tipos complexos são excluídos.)
- Tente corresponder ao maior número de parâmetros. A melhor correspondência pode ser um método sem parâmetros.

## <a name="extended-example"></a>Exemplo estendido

Circula

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

Controlador:

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

Solicitação HTTP:

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a>Correspondência de rota

O URI corresponde à rota chamada "DefaultApi". O dicionário de rotas contém as seguintes entradas:

- controlador: "produtos"
- id: "1"

O dicionário de rotas não contém os parâmetros de cadeia de caracteres de consulta, "Version" e "Details", mas eles ainda serão considerados durante a seleção da ação.

### <a name="controller-selection"></a>Seleção de controlador

A partir da entrada "controlador" no dicionário de rota, o tipo de controlador é `ProductsController`.

### <a name="action-selection"></a>Seleção de ação

A solicitação HTTP é uma solicitação GET. As ações do controlador que dão suporte a GET são `GetAll`, `GetById`e `FindProductsByName`. O dicionário de rotas não contém uma entrada para "Action", portanto, não precisamos corresponder ao nome da ação.

Em seguida, tentamos corresponder os nomes de parâmetro das ações, observando apenas as ações GET.

| Ação | Parâmetros para correspondência |
| --- | --- |
| `GetAll` | none |
| `GetById` | "id" |
| `FindProductsByName` | nomes |

Observe que o parâmetro de *versão* de `GetById` não é considerado, porque é um parâmetro opcional.

O método `GetAll` corresponde trivialmente. O método `GetById` também corresponde, porque o dicionário de rotas contém "ID". O método de `FindProductsByName` não corresponde.

O método `GetById` vence, pois ele corresponde a um parâmetro, versus nenhum parâmetro para `GetAll`. O método é invocado com os seguintes valores de parâmetro:

- *ID* = 1
- *versão* = 1,5

Observe que, embora a *versão* não tenha sido usada no algoritmo de seleção, o valor do parâmetro é proveniente da cadeia de caracteres de consulta do URI.

## <a name="extension-points"></a>Pontos de extensão

A API Web fornece pontos de extensão para algumas partes do processo de roteamento.

| Interface | DESCRIÇÃO |
| --- | --- |
| **IHttpControllerSelector** | Seleciona o controlador. |
| **IHttpControllerTypeResolver** | Obtém a lista de tipos de controlador. O **DefaultHttpControllerSelector** escolhe o tipo de controlador nesta lista. |
| **IAssembliesResolver** | Obtém a lista de assemblies do projeto. A interface **IHttpControllerTypeResolver** usa essa lista para localizar os tipos de controlador. |
| **IHttpControllerActivator** | Cria novas instâncias do controlador. |
| **IHttpActionSelector** | Seleciona a ação. |
| **IHttpActionInvoker** | Invoca a ação. |

Para fornecer sua própria implementação para qualquer uma dessas interfaces, use a coleção de **Serviços** no objeto **HttpConfiguration** :

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
