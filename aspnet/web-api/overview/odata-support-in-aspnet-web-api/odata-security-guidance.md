---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
title: Diretrizes de segurança para ASP.NET Web API 2 OData-ASP.NET 4. x
author: MikeWasson
description: Descreve os problemas de segurança a serem considerados ao expor um DataSet por meio do OData para ASP.NET Web API 2 no ASP.NET 4. x.
ms.author: riande
ms.date: 02/06/2013
ms.custom: seoapril2019
ms.assetid: b91e6424-1544-4747-bd0b-d1f8418c9653
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
msc.type: authoredcontent
ms.openlocfilehash: 8194a368cb0629c30e32ec05bf4bed150d442ad8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556493"
---
# <a name="security-guidance-for-aspnet-web-api-2-odata"></a>Diretrizes de segurança para o ASP.NET Web API 2 OData

por [Mike Wasson](https://github.com/MikeWasson)

Este tópico descreve alguns dos problemas de segurança que você deve considerar ao expor um DataSet por meio do OData para ASP.NET Web API 2 no ASP.NET 4. x.

## <a name="edm-security"></a>Segurança do EDM

A semântica de consulta se baseia no EDM (modelo de dados de entidade), não nos tipos de modelo subjacentes. Você pode excluir uma propriedade do EDM e ela não será visível para a consulta. Por exemplo, suponha que seu modelo inclua um tipo de funcionário com uma propriedade salário. Talvez você queira excluir essa propriedade do EDM para ocultá-la dos clientes.

Há duas maneiras de excluir uma propriedade do EDM. Você pode definir o atributo **[IgnoreDataMember]** na propriedade na classe de modelo:

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

Você também pode remover a propriedade do EDM programaticamente:

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a>Segurança de consulta

Um cliente mal-intencionado ou ingênua pode ser capaz de construir uma consulta que leva muito tempo para ser executada. Na pior das hipóteses, isso pode interromper o acesso ao seu serviço.

O atributo **[passível** de consulta] é um filtro de ação que analisa, valida e aplica a consulta. O filtro converte as opções de consulta em uma expressão LINQ. Quando o controlador OData retorna um tipo **IQueryable** , o provedor de LINQ do **IQueryable** converte a expressão LINQ em uma consulta. Portanto, o desempenho depende do provedor LINQ que é usado e também das características específicas do seu conjunto de dados ou esquema de banco de dados.

Para obter mais informações sobre como usar as opções de consulta OData no ASP.NET Web API, consulte [Opções de consulta OData de suporte](supporting-odata-query-options.md).

Se você souber que todos os clientes são confiáveis (por exemplo, em um ambiente corporativo), ou se o conjunto de seus conjuntos de seus for pequeno, o desempenho da consulta poderá não ser um problema. Caso contrário, você deve considerar as recomendações a seguir.

- Teste seu serviço com várias consultas e perfil do BD.
- Habilite a paginação baseada em servidor para evitar retornar um conjunto de dados grande em uma consulta. Para obter mais informações, consulte [paginação baseada em servidor](supporting-odata-query-options.md#server-paging). 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- Você precisa de $filter e $orderby? Alguns aplicativos podem permitir a paginação de cliente, usando $top e $skip, mas desabilite as outras opções de consulta. 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- Considere restringir $orderby a propriedades em um índice clusterizado. A classificação de dados grandes sem um índice clusterizado é lenta. 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- Contagem máxima de nós: a propriedade **MaxNodeCount** em **[passível** de consulta] define o número máximo de nós permitidos na árvore de sintaxe de $Filter. O valor padrão é 100, mas talvez você queira definir um valor mais baixo, pois um grande número de nós pode ser lento para compilar. Isso é particularmente verdadeiro se você estiver usando LINQ to Objects (ou seja, consultas LINQ em uma coleção na memória, sem o uso de um provedor LINQ intermediário). 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- Considere desabilitar as funções Any () e All (), pois elas podem ser lentas. 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- Se quaisquer propriedades de cadeia de caracteres&#8212;contiverem cadeias grandes, por exemplo,&#8212;uma descrição de produto ou uma entrada de blog, considere desabilitar as funções de cadeia de caracteres. 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- Considere a despermissão de filtragem nas propriedades de navegação. A filtragem nas propriedades de navegação pode resultar em uma junção, que pode ser lenta, dependendo do seu esquema de banco de dados. O código a seguir mostra um validador de consulta que impede a filtragem nas propriedades de navegação. Para obter mais informações sobre os validadores de consulta, consulte [validação de consulta](supporting-odata-query-options.md#query-validation). 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- Considere restringir as consultas de $filter escrevendo um validador que é personalizado para seu banco de dados. Por exemplo, considere estas duas consultas: 

  - Todos os filmes com atores cujo último nome começa com ' A '.
  - Todos os filmes lançados em 1994.

    A menos que os filmes sejam indexados por atores, a primeira consulta pode exigir que o mecanismo de banco de BD examine toda a lista de filmes. Enquanto a segunda consulta pode ser aceitável, pressupondo que os filmes sejam indexados por ano de lançamento.

    O código a seguir mostra um validador que permite a filtragem nas propriedades "ReleaseYear" e "title", mas nenhuma outra propriedade.

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- Em geral, considere quais $filter funções de que você precisa. Se os clientes não precisarem da expressividade completa do $filter, você poderá limitar as funções permitidas.
