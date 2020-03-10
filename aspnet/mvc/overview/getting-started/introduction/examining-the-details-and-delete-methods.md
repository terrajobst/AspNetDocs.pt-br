---
uid: mvc/overview/getting-started/introduction/examining-the-details-and-delete-methods
title: Examinando os métodos Details e Delete | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 03/26/2015
ms.assetid: f1d2a916-626c-4a54-8df4-77e6b9fff355
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: da06815b5c1d76a939fdfb77ce11774081dfb881
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78582505"
---
# <a name="examining-the-details-and-delete-methods"></a>Examinar os métodos Details e Delete

por [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [Tutorial Note](index.md)]

Nesta parte do tutorial, você examinará os métodos `Details` e `Delete` gerados automaticamente.

## <a name="examining-the-details-and-delete-methods"></a>Examinar os métodos Details e Delete

Abra o controlador de `Movie` e examine o método `Details`.

![](examining-the-details-and-delete-methods/_static/image1.png)

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample1.cs)]

O mecanismo de scaffolding do MVC que criou esse método de ação adiciona um comentário mostrando uma solicitação HTTP que invoca o método. Nesse caso, é uma solicitação de `GET` com três segmentos de URL, o controlador de `Movies`, o método de `Details` e um valor de `ID`.

Code First facilita a pesquisa de dados usando o método `Find`. Um recurso de segurança importante interno do método é que o código verifica se o método `Find` encontrou um filme antes de o código tentar fazer algo com ele. Por exemplo, um hacker pode introduzir erros no site, alterando a URL criada pelos links de `http://localhost:xxxx/Movies/Details/1` para algo como `http://localhost:xxxx/Movies/Details/12345` (ou algum outro valor que não represente um filme real). Se você não tiver verificado um filme nulo, um filme nulo resultaria em um erro de banco de dados.

Examine os métodos `Delete` e `DeleteConfirmed`.

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample2.cs?highlight=17)]

Observe que o método HTTP GET `Delete` não exclui o filme especificado, ele retorna uma exibição do filme onde você pode enviar (`HttpPost`) a exclusão. A execução de uma operação de exclusão em resposta a uma solicitação GET (ou, de fato, a execução de uma operação de edição, criação ou qualquer outra operação que altera dados) abre uma falha de segurança. Para obter mais informações sobre isso, consulte a entrada de blog de Stephen Walther [ASP.net de dicas do MVC #46 – não use os links de exclusão porque eles criam brechas de segurança](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

O método `HttpPost` que exclui os dados é chamado `DeleteConfirmed` para fornecer ao método HTTP POST um nome ou uma assinatura exclusiva. As duas assinaturas de método são mostradas abaixo:

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample3.cs)]

O CLR (Common Language Runtime) exige que os métodos sobrecarregados tenham uma assinatura de parâmetro exclusiva (mesmo nome de método, mas uma lista diferente de parâmetros). No entanto, aqui você precisa de dois métodos Delete, um para GET e outro para POST – que ambos tenham a mesma assinatura de parâmetro. (Ambos precisam aceitar um único inteiro como parâmetro.)

Para classificar isso, você pode fazer algumas coisas. Uma é fornecer aos métodos nomes diferentes. Foi isso o que o mecanismo de scaffolding fez no exemplo anterior. No entanto, isso apresenta um pequeno problema: o ASP.NET mapeia os segmentos de URL para os métodos de ação por nome e se você renomear um método, o roteamento normalmente não conseguirá encontrar esse método. A solução é o que você vê no exemplo, que é adicionar o atributo `ActionName("Delete")` ao método `DeleteConfirmed`. Isso efetivamente executa o mapeamento para o sistema de roteamento para que uma URL que inclua */delete/* para uma solicitação post encontre o método `DeleteConfirmed`.

Outra maneira comum de evitar um problema com métodos que têm nomes e assinaturas idênticos é alterar artificialmente a assinatura do método POST para incluir um parâmetro não utilizado. Por exemplo, alguns desenvolvedores adicionam um tipo de parâmetro `FormCollection` que é passado para o método POST e, em seguida, simplesmente não usam o parâmetro:

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="summary"></a>Resumo

Agora você tem um aplicativo MVC ASP.NET completo que armazena dados em um banco de dado do BD local. Você pode criar, ler, atualizar, excluir e Pesquisar filmes.

![](examining-the-details-and-delete-methods/_static/image2.png)

## <a name="next-steps"></a>Próximas etapas

Depois de criar e testar um aplicativo Web, a próxima etapa é disponibilizá-lo para outras pessoas usarem pela Internet. Para fazer isso, você precisa implantá-lo em um provedor de hospedagem na Web. A Microsoft oferece hospedagem na Web gratuita para até 10 sites em uma [conta de avaliação gratuita do Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Sugiro que você siga o meu tutorial [implantar um aplicativo MVC do ASP.net seguro com associação, OAuth e banco de dados SQL no Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Um tutorial excelente é o nível intermediário de Tom Dykstra [criando um modelo de dados Entity Framework para um aplicativo MVC ASP.net](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Os fóruns do [StackOverflow](http://stackoverflow.com/help) e do [ASP.NET MVC](https://forums.asp.net/1146.aspx) são um ótimo lugar para fazer perguntas. Siga [-me](https://twitter.com/RickAndMSFT) no Twitter para que você possa obter atualizações sobre meus tutoriais mais recentes.

Os comentários são bem-vindos.

– [Rick Anderson](https://blogs.msdn.com/rickAndy) twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT)  
— [Scott Hanselman](http://www.hanselman.com/blog/) twitter: [@shanselman](https://twitter.com/shanselman)

> [!div class="step-by-step"]
> [Anterior](adding-validation.md)
