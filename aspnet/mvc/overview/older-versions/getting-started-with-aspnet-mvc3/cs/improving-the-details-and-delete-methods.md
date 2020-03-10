---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/improving-the-details-and-delete-methods
title: Aprimorando os métodos de detalhes eC#de exclusão () | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensinará as noções básicas da criação de um aplicativo Web ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 3f42edd9-c5b8-4712-9055-970f7d38e350
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/improving-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: 54d7be8fe1bff604ae9c9e9914d7c6426ab85c1c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78615307"
---
# <a name="improving-the-details-and-delete-methods-c"></a>Aprimorar os métodos Details e Delete (C#)

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Uma versão atualizada deste tutorial está disponível [aqui](../../../getting-started/introduction/getting-started.md) que usa o ASP.NET MVC 5 e o Visual Studio 2013. É mais seguro, muito mais simples de seguir e demonstra mais recursos.
> 
> 
> Este tutorial ensinará as noções básicas da criação de um aplicativo Web ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é uma versão gratuita do Microsoft Visual Studio. Antes de começar, verifique se você instalou os pré-requisitos listados abaixo. Você pode instalar todos eles clicando no seguinte link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, você pode instalar os pré-requisitos individualmente usando os seguintes links:
> 
> - [Pré-requisitos do Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Atualização de ferramentas do ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(suporte + ferramentas de tempo de execução)
> 
> Se você estiver usando o Visual Studio 2010 em vez do Visual Web Developer 2010, instale os pré-requisitos clicando no seguinte link: [pré-requisitos do Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Um projeto do Visual Web Developer com C# código-fonte está disponível para acompanhar este tópico. [Baixe a C# versão](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se preferir Visual Basic, alterne para a [versão Visual Basic](../vb/intro-to-aspnet-mvc-3.md) deste tutorial.

Nesta parte do tutorial, você fará algumas melhorias nos métodos gerados automaticamente `Details` e `Delete`. Essas alterações não são necessárias, mas com apenas alguns pequenos bits de código, você pode facilmente aprimorar o aplicativo.

## <a name="improving-the-details-and-delete-methods"></a>Aprimorando os métodos de detalhes e de exclusão

Quando você com Scaffold o controlador de `Movie`, o ASP.NET MVC gerou código que funcionou muito bem, mas isso pode se tornar mais robusto com apenas algumas pequenas alterações.

Abra o controlador de `Movie` e modifique o método `Details` retornando `HttpNotFound` quando um filme não for encontrado. Você também deve modificar o método `Details` para definir um valor padrão para a ID que é passada para ele. (Você fez alterações semelhantes ao método `Edit` na [parte 6](examining-the-edit-methods-and-edit-view.md) deste tutorial.) No entanto, você deve alterar o tipo de retorno do método `Details` de `ViewResult` para `ActionResult`, porque o método `HttpNotFound` não retorna um objeto `ViewResult`. O exemplo a seguir mostra o método de `Details` modificado.

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample1.cs)]

Code First facilita a pesquisa de dados usando o método `Find`. Um recurso de segurança importante que criamos no método é que o código verifica se o método `Find` encontrou um filme antes de o código tentar fazer algo com ele. Por exemplo, um hacker pode introduzir erros no site, alterando a URL criada pelos links de `http://localhost:xxxx/Movies/Details/1` para algo como `http://localhost:xxxx/Movies/Details/12345` (ou algum outro valor que não represente um filme real). Se você não verificar um filme nulo, isso poderá resultar em um erro de banco de dados.

Da mesma forma, altere os métodos `Delete` e `DeleteConfirmed` para especificar um valor padrão para o parâmetro ID e retornar `HttpNotFound` quando um filme não for encontrado. Os métodos de `Delete` atualizados no controlador de `Movie` são mostrados abaixo.

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample2.cs)]

Observe que o método `Delete` não exclui os dados. A execução de uma operação de exclusão em resposta a uma solicitação GET (ou, de fato, a execução de uma operação de edição, criação ou qualquer outra operação que altera dados) abre uma falha de segurança. Para obter mais informações sobre isso, consulte a entrada de blog de Stephen Walther [ASP.net de dicas do MVC #46 – não use os links de exclusão porque eles criam brechas de segurança](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

O método `HttpPost` que exclui os dados é chamado `DeleteConfirmed` para fornecer ao método HTTP POST um nome ou uma assinatura exclusiva. As duas assinaturas de método são mostradas abaixo:

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample3.cs)]

O Common Language Runtime (CLR) requer que os métodos sobrecarregados tenham uma assinatura exclusiva (mesmo nome, lista de parâmetros diferente). No entanto, aqui você precisa de dois métodos Delete, um para GET e outro para POST – que ambos exigem a mesma assinatura. (Ambos precisam aceitar um único inteiro como parâmetro.)

Para classificar isso, você pode fazer algumas coisas. Uma é fornecer aos métodos nomes diferentes. É isso o que fizemos no exemplo anterior. No entanto, isso apresenta um pequeno problema: o ASP.NET mapeia os segmentos de URL para os métodos de ação por nome e se você renomear um método, o roteamento normalmente não conseguirá encontrar esse método. A solução é o que você vê no exemplo, que é adicionar o atributo `ActionName("Delete")` ao método `DeleteConfirmed`. Isso efetivamente executa o mapeamento para o sistema de roteamento para que uma URL que inclua <em>/delete/</em>para uma solicitação post encontre o método `DeleteConfirmed`.

Outra maneira de evitar um problema com métodos que têm nomes e assinaturas idênticos é alterar artificialmente a assinatura do método POST para incluir um parâmetro não utilizado. Por exemplo, alguns desenvolvedores adicionam um tipo de parâmetro `FormCollection` que é passado para o método POST e, em seguida, simplesmente não usam o parâmetro:

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="wrapping-up"></a>Resumindo

Agora você tem um aplicativo MVC completo do ASP.NET que armazena dados em um banco de dado SQL Server Compact. Você pode criar, ler, atualizar, excluir e Pesquisar filmes.

![](improving-the-details-and-delete-methods/_static/image1.png)

Este tutorial básico começou a tornar os controladores, associá-los a exibições e transmitir dados embutidos em código. Em seguida, você criou e projetou um modelo de dados. Entity Framework Code First criou um banco de dados a partir do modelo em tempo real, e o sistema ASP.NET MVC scaffolding gerou automaticamente os métodos de ação e modos de exibição para operações CRUD básicas. Em seguida, você adicionou um formulário de pesquisa que permite aos usuários pesquisar o banco de dados. Você alterou o banco de dados para incluir uma nova coluna de data e, em seguida, atualizou duas páginas para criar e exibir esses novos dados. Você adicionou a validação marcando o modelo de dados com atributos do namespace `DataAnnotations`. A validação resultante é executada no cliente e no servidor.

Se você quiser implantar seu aplicativo, é útil primeiro testar o aplicativo em seu servidor local do IIS 7. Você pode usar esse [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=ASPNET;) link para habilitar a configuração do IIS para aplicativos ASP.net. Consulte os seguintes links de implantação:

- [Mapa de conteúdo de implantação do ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx)
- [Habilitando o IIS 7. x](https://blogs.msdn.com/b/rickandy/archive/2011/03/14/enabling-iis-7-x-on-windows-7-vista-sp1-windows-2008-windows-2008-r2.aspx)
- [Implantação de projetos de aplicativos Web](https://msdn.microsoft.com/library/dd394698.aspx)

Agora recomendo que você passe para nosso nível intermediário [criando um modelo de dados Entity Framework para um aplicativo mvc ASP.net](../../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) e tutoriais da [loja de música MVC](../../mvc-music-store/mvc-music-store-part-1.md) , para explorar os [artigos do ASP.net no MSDN](https://msdn.microsoft.com/library/gg416514(VS.98).aspx)e para conferir os vários vídeos e recursos em [https://asp.net/mvc](https://asp.net/mvc) para aprender ainda mais sobre o ASP.NET MVC! Os [fóruns do ASP.NET MVC](https://forums.asp.net/1146.aspx) são um ótimo lugar para fazer perguntas.

Aproveite!

— Scott Hanselman ([http://hanselman.com](http://hanselman.com) e [@shanselman](http://twitter.com/shanselman) no Twitter) e Rick Anderson [Blogs.msdn.com/rickAndy](https://blogs.msdn.com/rickAndy)

> [!div class="step-by-step"]
> [Anterior](adding-validation-to-the-model.md)
