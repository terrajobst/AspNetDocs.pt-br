---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: Adicionando um modelo | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: c5525cfe940cadff5113c63eb0508d15697b5606
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456537"
---
# <a name="adding-a-model"></a>Adicionar um modelo

por [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [Tutorial Note](index.md)]

Nesta seção, você adicionará algumas classes para gerenciar filmes em um banco de dados. Essas classes serão o modelo de &quot;&quot; parte do aplicativo MVC ASP.NET.

Você usará uma .NET Framework tecnologia de acesso a dados conhecida como [Entity Framework](https://docs.microsoft.com/ef/) para definir e trabalhar com essas classes de modelo. A Entity Framework (geralmente conhecida como EF) dá suporte a um paradigma de desenvolvimento chamado *Code First*. Code First permite que você crie objetos de modelo escrevendo classes simples. (Elas também são conhecidas como classes POCO, de &quot;objetos CLR comuns.&quot;) Em seguida, você pode fazer com que o banco de dados seja criado dinamicamente de suas classes, o que permite um fluxo de trabalho de desenvolvimento muito limpo e rápido. Se for necessário primeiro criar o banco de dados, você ainda poderá seguir este tutorial para saber mais sobre o desenvolvimento de aplicativos MVC e EF. Em seguida, você pode seguir Tom Fizmakens [ASP.net scaffolding](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) tutorial, que aborda a primeira abordagem do banco de dados.

## <a name="adding-model-classes"></a>Adicionando classes de modelo

Em **Gerenciador de soluções**, clique com o botão direito do mouse na pasta *modelos* , selecione **Adicionar**e, em seguida, selecione **classe**.

![](adding-a-model/_static/image1.png)

Insira o nome da *classe* &quot;filme&quot;.

Adicione as cinco propriedades a seguir à classe `Movie`:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Usaremos a classe `Movie` para representar filmes em um banco de dados. Cada instância de um objeto `Movie` corresponderá a uma linha em uma tabela de banco de dados, e cada propriedade da classe `Movie` será mapeada para uma coluna na tabela.

Observação: para usar System. Data. Entity e a classe relacionada, você precisa instalar o [Entity Framework pacote NuGet](https://www.nuget.org/packages/EntityFramework/). Siga o link para obter mais instruções.

No mesmo arquivo, adicione a seguinte classe de `MovieDBContext`:

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

A classe `MovieDBContext` representa o contexto de banco de dados Entity Framework filme, que lida com a busca, o armazenamento e a atualização de `Movie` instâncias de classe em um banco de dados. O `MovieDBContext` deriva da classe base `DbContext` fornecida pelo Entity Framework.

Para poder referenciar `DbContext` e `DbSet`, você precisa adicionar a seguinte instrução `using` na parte superior do arquivo:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

Você pode fazer isso adicionando manualmente a instrução using, ou pode passar o mouse sobre as linhas onduladas vermelhas, clicar em `Show potential fixes` e clicar em `using System.Data.Entity;`

![](adding-a-model/_static/image2.png)

Observação: várias instruções `using` não utilizadas foram removidas. O Visual Studio mostrará dependências não utilizadas como cinza. Você pode remover dependências não utilizadas passando o mouse sobre as dependências de cinza, clicar em `Show potential fixes` e clicar em **remover não usado usando.**

![](adding-a-model/_static/image3.png)

Finalmente adicionamos um modelo (o M no MVC). Na próxima seção, você trabalhará com a cadeia de conexão do banco de dados.

> [!div class="step-by-step"]
> [Anterior](adding-a-view.md)
> [Próximo](creating-a-connection-string.md)
