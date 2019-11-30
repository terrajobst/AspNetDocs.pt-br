---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: 'Parte 2: criando os modelos de domínio | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/03/2012
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: 7c5ed1bdb4b390c94907b14e208231f16ad42d96
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600368"
---
# <a name="part-2-creating-the-domain-models"></a>Parte 2: criando os modelos de domínio

por [Mike Wasson](https://github.com/MikeWasson)

[Baixar projeto concluído](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a>Adicionar modelos

Há três maneiras de se abordar Entity Framework:

- Banco de dados-primeiro: você começa com um banco de dados e Entity Framework gera o código.
- Modelo-primeiro: você começa com um modelo visual e Entity Framework gera o banco de dados e o código.
- Primeiro código: você começa com o código e Entity Framework gera o banco de dados.

Estamos usando a abordagem Code-First, portanto, começamos definindo nossos objetos de domínio como POCOs (objetos CLR antigos). Com a abordagem de primeiro código, os objetos de domínio não precisam de nenhum código extra para dar suporte à camada de banco de dados, como transações ou persistência. (Especificamente, eles não precisam herdar da classe [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) .) Você ainda pode usar anotações de dados para controlar como Entity Framework cria o esquema de banco de dado.

Como POCOs não carregam nenhuma propriedade extra que descreva o [estado do banco de dados](https://msdn.microsoft.com/library/system.data.entitystate.aspx), eles podem ser facilmente serializados para JSON ou XML. No entanto, isso não significa que você sempre deve expor seus modelos de Entity Framework diretamente aos clientes, como veremos mais adiante no tutorial.

Criaremos o seguinte POCOs:

- Produto
- Ordem
- OrderDetail

Para criar cada classe, clique com o botão direito do mouse na pasta modelos em Gerenciador de Soluções. No menu de contexto, selecione **Adicionar** e, em seguida, selecione **classe.**

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

Adicione uma classe de `Product` com a seguinte implementação:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

Por convenção, Entity Framework usa a propriedade `Id` como a chave primária e a mapeia para uma coluna de identidade na tabela de banco de dados. Ao criar uma nova instância de `Product`, você não definirá um valor para `Id`, porque o banco de dados gera o valor.

O atributo **ScaffoldColumn** informa ao ASP.NET MVC para ignorar a propriedade `Id` ao gerar um formulário do editor. O atributo **Required** é usado para validar o modelo. Ele especifica que a propriedade `Name` deve ser uma cadeia de caracteres não vazia.

Adicione a classe `Order`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

Adicione a classe `OrderDetail`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a>Relações de chave estrangeira

Um pedido contém muitos detalhes do pedido e cada detalhe do pedido se refere a um único produto. Para representar essas relações, a classe `OrderDetail` define as propriedades chamadas `OrderId` e `ProductId`. Entity Framework inferirá que essas propriedades representam chaves estrangeiras e adicionará restrições Foreign-Key ao banco de dados.

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

As classes `Order` e `OrderDetail` também incluem propriedades de "navegação", que contêm referências aos objetos relacionados. Dado um pedido, você pode navegar para os produtos na ordem seguindo as propriedades de navegação.

Compile o projeto agora. Entity Framework usa a reflexão para descobrir as propriedades dos modelos, portanto, ele requer um assembly compilado para criar o esquema de banco de dados.

## <a name="configure-the-media-type-formatters"></a>Configurar os formatadores de tipo de mídia

Um [formatador de tipo de mídia](../../formats-and-model-binding/media-formatters.md) é um objeto que serializa seus dados quando a API da Web grava o corpo da resposta http. Os formatadores internos dão suporte à saída JSON e XML. Por padrão, esses dois formatadores serializam todos os objetos por valor.

A serialização por valor criará um problema se um gráfico de objetos contiver referências circulares. Isso é exatamente o caso com as classes `Order` e `OrderDetail`, porque cada uma tem uma referência para a outra. O formatador seguirá as referências, gravando cada objeto por valor e acessará os círculos. Portanto, precisamos alterar o comportamento padrão.

Em Gerenciador de Soluções, expanda a pasta\_iniciar o aplicativo e abra o arquivo chamado WebApiConfig.cs. Adicione o seguinte código à classe `WebApiConfig`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

Esse código define o formatador JSON para preservar referências de objeto e remove totalmente o formatador XML do pipeline. (Você pode configurar o formatador XML para preservar referências de objeto, mas é um pouco mais de trabalho, e precisamos apenas de JSON para esse aplicativo. Para obter mais informações, consulte [lidando com referências a objetos circulares](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)

> [!div class="step-by-step"]
> [Anterior](using-web-api-with-entity-framework-part-1.md)
> [Próximo](using-web-api-with-entity-framework-part-3.md)
