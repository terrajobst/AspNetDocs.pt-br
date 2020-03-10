---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: Manipulando relações de entidade | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: be4948e5443a5eb4e1824c63dd0c445a7ee1928e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557459"
---
# <a name="handling-entity-relations"></a>Tratamento de relações de entidade

por [Mike Wasson](https://github.com/MikeWasson)

[Baixar projeto concluído](https://github.com/MikeWasson/BookService)

Esta seção descreve alguns detalhes de como o EF carrega entidades relacionadas e como lidar com propriedades de navegação circular em suas classes de modelo. (Esta seção fornece conhecimento em segundo plano e não é necessária para concluir o tutorial. Se preferir, pule para a [parte 5.](part-5.md).)

## <a name="eager-loading-versus-lazy-loading"></a>Carregamento adiantado versus carregamento lento

Ao usar o EF com um banco de dados relacional, é importante entender como o EF carrega dados relacionados.

Também é útil ver as consultas SQL que o EF gera. Para rastrear o SQL, adicione a seguinte linha de código ao construtor de `BookServiceContext`:

[!code-csharp[Main](part-4/samples/sample1.cs)]

Se você enviar uma solicitação GET para/API/Books, ela retornará JSON como o seguinte:

[!code-console[Main](part-4/samples/sample2.cmd)]

Você pode ver que a propriedade autor é nula, embora o livro contenha uma AuthorId válida. Isso ocorre porque o EF não está carregando as entidades do autor relacionadas. O log de rastreamento da consulta SQL confirma isso:

[!code-console[Main](part-4/samples/sample3.sql)]

A instrução SELECT usa a tabela books e não faz referência à tabela de autor.

Para referência, aqui está o método na classe `BooksController` que retorna a lista de livros.

[!code-csharp[Main](part-4/samples/sample4.cs)]

Vejamos como podemos retornar o autor como parte dos dados JSON. Há três maneiras de carregar dados relacionados no Entity Framework: carregamento adiantado, carregamento lento e carregamento explícito. Há compensações com cada técnica, portanto, é importante entender como elas funcionam.

### <a name="eager-loading"></a>Carregamento adiantado

Com o *carregamento adiantado*, o EF carrega entidades relacionadas como parte da consulta de banco de dados inicial. Para executar o carregamento adiantado, use o método de extensão **System. Data. Entity. include** .

[!code-csharp[Main](part-4/samples/sample5.cs)]

Isso informa ao EF para incluir os dados do autor na consulta. Se você fizer essa alteração e executar o aplicativo, agora os dados JSON têm a seguinte aparência:

[!code-console[Main](part-4/samples/sample6.cmd)]

O log de rastreamento mostra que o EF executou uma junção no livro e cria tabelas.

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a>Carregamento lento

Com o carregamento lento, o EF carrega automaticamente uma entidade relacionada quando a propriedade de navegação para essa entidade é desreferenciada. Para habilitar o carregamento lento, torne a propriedade de navegação virtual. Por exemplo, na classe Book:

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

Agora, considere o seguinte código:

[!code-csharp[Main](part-4/samples/sample9.cs)]

Quando o carregamento lento está habilitado, o acesso à propriedade `Author` em `books[0]` faz com que o EF consulte o banco de dados para o autor.

O carregamento lento requer várias viagens de banco de dados, porque o EF envia uma consulta cada vez que recupera uma entidade relacionada. Em geral, você deseja que o carregamento lento seja desabilitado para objetos serializados. O serializador precisa ler todas as propriedades no modelo, que dispara o carregamento das entidades relacionadas. Por exemplo, aqui estão as consultas SQL quando o EF serializa a lista de livros com o carregamento lento habilitado. Você pode ver que o EF faz três consultas separadas para os três autores.

[!code-console[Main](part-4/samples/sample10.sql)]

Ainda há momentos em que você talvez queira usar o carregamento lento. O carregamento adiantado pode fazer com que o EF gere uma junção muito complexa. Ou talvez você precise de entidades relacionadas para um pequeno subconjunto dos dados e o carregamento lento seria mais eficiente.

Uma maneira de evitar problemas de serialização é serializar os DTOs (objetos de transferência de dados) em vez de objetos de entidade. Mostrarei essa abordagem posteriormente neste artigo.

### <a name="explicit-loading"></a>Carregamento explícito

O carregamento explícito é semelhante ao carregamento lento, exceto pelo fato de que você obtém explicitamente os dados relacionados no código; Ele não acontece automaticamente quando você acessa uma propriedade de navegação. O carregamento explícito oferece mais controle sobre quando carregar dados relacionados, mas requer código extra. Para obter mais informações sobre o carregamento explícito, consulte [carregando entidades relacionadas](https://msdn.microsoft.com/data/jj574232#explicit).

## <a name="navigation-properties-and-circular-references"></a>Propriedades de navegação e referências circulares

Quando defini os modelos Book e Author, defini uma propriedade de navegação na classe `Book` para a relação Book-Author, mas não defini uma propriedade de navegação na outra direção.

O que acontecerá se você adicionar a propriedade de navegação correspondente à classe `Author`?

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

Infelizmente, isso cria um problema ao serializar os modelos. Se você carregar os dados relacionados, ele criará um gráfico de objeto circular.

![](part-4/_static/image1.png)

Quando o formatador JSON ou XML tenta serializar o grafo, ele gerará uma exceção. Os dois formatadores lançam mensagens de exceção diferentes. Aqui está um exemplo para o formatador JSON:

[!code-console[Main](part-4/samples/sample12.cmd)]

Este é o formatador XML:

[!code-xml[Main](part-4/samples/sample13.xml)]

Uma solução é usar DTOs, que descreverei na próxima seção. Como alternativa, você pode configurar os formatadores JSON e XML para lidar com os ciclos do grafo. Para obter mais informações, consulte [lidando com referências a objetos circulares](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).

Para este tutorial, você não precisa da propriedade de navegação `Author.Book`, para que você possa deixá-la fora.

> [!div class="step-by-step"]
> [Anterior](part-3.md)
> [Próximo](part-5.md)
