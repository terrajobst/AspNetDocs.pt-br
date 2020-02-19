---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: Adicionando um modelo | Microsoft Docs
author: Rick-Anderson
description: 'Observação: uma versão atualizada deste tutorial está disponível aqui que usa o ASP.NET MVC 5 e o Visual Studio 2013. É mais seguro, muito mais simples de seguir e demonstrar...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: a9851d93dde495814f67fa0c807d3534f5f0d8ef
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457798"
---
# <a name="adding-a-model"></a>Adicionar um modelo

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Uma versão atualizada deste tutorial está disponível [aqui](../../getting-started/introduction/getting-started.md) que usa o ASP.NET MVC 5 e o Visual Studio 2013. É mais seguro, muito mais simples de seguir e demonstra mais recursos.

Nesta seção, você adicionará algumas classes para gerenciar filmes em um banco de dados. Essas classes serão o modelo de &quot;&quot; parte do aplicativo MVC ASP.NET.

Você usará uma .NET Framework tecnologia de acesso a dados conhecida como [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) para definir e trabalhar com essas classes de modelo. A Entity Framework (geralmente conhecida como EF) dá suporte a um paradigma de desenvolvimento chamado *Code First*. Code First permite que você crie objetos de modelo escrevendo classes simples. (Elas também são conhecidas como classes POCO, de &quot;objetos CLR comuns.&quot;) Em seguida, você pode fazer com que o banco de dados seja criado dinamicamente de suas classes, o que permite um fluxo de trabalho de desenvolvimento muito limpo e rápido.

## <a name="adding-model-classes"></a>Adicionando classes de modelo

Em **Gerenciador de soluções**, clique com o botão direito do mouse na pasta *modelos* , selecione **Adicionar**e, em seguida, selecione **classe**.

![](adding-a-model/_static/image1.png)

Insira o nome da *classe* &quot;filme&quot;.

Adicione as cinco propriedades a seguir à classe `Movie`:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Usaremos a classe `Movie` para representar filmes em um banco de dados. Cada instância de um objeto `Movie` corresponderá a uma linha em uma tabela de banco de dados, e cada propriedade da classe `Movie` será mapeada para uma coluna na tabela.

No mesmo arquivo, adicione a seguinte classe de `MovieDBContext`:

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

A classe `MovieDBContext` representa o contexto de banco de dados Entity Framework filme, que lida com a busca, o armazenamento e a atualização de `Movie` instâncias de classe em um banco de dados. O `MovieDBContext` deriva da classe base `DbContext` fornecida pelo Entity Framework.

Para poder referenciar `DbContext` e `DbSet`, você precisa adicionar a seguinte instrução `using` na parte superior do arquivo:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

O arquivo *Movie.cs* completo é mostrado abaixo. (Várias instruções using que não são necessárias foram removidas.)

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Criar uma cadeia de conexão e trabalhando com LocalDB do SQL Server

A classe `MovieDBContext` que você criou manipula a tarefa de conexão com o banco de dados e o mapeamento de objetos `Movie` para registros de banco de dados. No entanto, uma pergunta que você pode fazer é como especificar a qual banco de dados ele será conectado. Você fará isso adicionando informações de conexão no arquivo *Web. config* do aplicativo.

Abra o arquivo *Web. config da* raiz do aplicativo. (Não o arquivo *Web. config* na pasta *views* .) Abra o arquivo *Web. config* descrito em vermelho.

![](adding-a-model/_static/image2.png)

Adicione a seguinte cadeia de conexão ao elemento `<connectionStrings>` no arquivo *Web. config* .

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

O exemplo a seguir mostra uma parte do arquivo *Web. config* com a nova cadeia de conexão adicionada:

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

Essa pequena quantidade de código e XML é tudo o que você precisa para escrever a fim de representar e armazenar os dados do filme em um banco de dado.

Em seguida, você criará uma nova classe `MoviesController` que pode ser usada para exibir os dados do filme e permitir que os usuários criem novas listagens de filmes.

> [!div class="step-by-step"]
> [Anterior](adding-a-view.md)
> [Próximo](accessing-your-models-data-from-a-controller.md)
