---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-2
title: Adicionar modelos e controladores | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 88908ff8-51a9-40eb-931c-a8139128b680
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-2
msc.type: authoredcontent
ms.openlocfilehash: 57dacda421968f341284d89c9a3ad80040c16e25
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557536"
---
# <a name="add-models-and-controllers"></a>Adicionar modelos e controladores

por [Mike Wasson](https://github.com/MikeWasson)

[Baixar projeto concluído](https://github.com/MikeWasson/BookService)

Nesta seção, você adicionará classes de modelo que definem as entidades de banco de dados. Em seguida, você adicionará controladores de API da Web que executam operações CRUD nessas entidades.

## <a name="add-model-classes"></a>Adicionar classes de modelo

Neste tutorial, criaremos o banco de dados usando a abordagem "Code First" para Entity Framework (EF). Com Code First, você escreve C# classes que correspondem às tabelas de banco de dados e o EF cria o banco de dados. (Para obter mais informações, consulte [Entity Framework abordagens de desenvolvimento](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)

Começamos definindo nossos objetos de domínio como POCOs (objetos CLR antigos). Criaremos o seguinte POCOs:

- Autor
- Livro

Em Gerenciador de Soluções, clique com o botão direito do mouse na pasta modelos. Selecione **Adicionar**e selecione **classe**. Nome da classe `Author`.

![](part-2/_static/image1.png)

Substitua todo o código clichê em Author.cs pelo código a seguir.

[!code-csharp[Main](part-2/samples/sample1.cs)]

Adicione outra classe chamada `Book`, com o código a seguir.

[!code-csharp[Main](part-2/samples/sample2.cs)]

Entity Framework usará esses modelos para criar tabelas de banco de dados. Para cada modelo, a propriedade `Id` se tornará a coluna de chave primária da tabela de banco de dados.

Na classe Book, o `AuthorId` define uma chave estrangeira na tabela `Author`. (Para simplificar, estou supondo que cada livro tenha um único autor.) A classe Book também contém uma propriedade de navegação para o `Author`relacionado. Você pode usar a propriedade de navegação para acessar os `Author` relacionados no código. Eu digo mais sobre as propriedades de navegação na parte 4, [tratando as relações entre entidades](part-4.md).

## <a name="add-web-api-controllers"></a>Adicionar controladores de API Web

Nesta seção, adicionaremos controladores de API Web que dão suporte a operações CRUD (criar, ler, atualizar e excluir). Os controladores usarão Entity Framework para se comunicar com a camada de banco de dados.

Primeiro, você pode excluir os controladores de arquivo/ValuesController. cs. Esse arquivo contém um exemplo de controlador de API da Web, mas você não precisa dele para este tutorial.

![](part-2/_static/image2.png)

Em seguida, compile o projeto. A API Web scaffolding usa a reflexão para localizar as classes de modelo, portanto, ele precisa do assembly compilado.

Em Gerenciador de Soluções, clique com o botão direito do mouse na pasta controladores. Selecione **Adicionar**e, em seguida, selecione **controlador**.

![](part-2/_static/image3.png)

Na caixa de diálogo **Adicionar Scaffold** , selecione "controlador da API Web 2 com ações, usando Entity Framework". Clique em **Adicionar**.

![](part-2/_static/image4.png)

Na caixa de diálogo **Adicionar controlador** , faça o seguinte:

1. Na lista suspensa **classe de modelo** , selecione a classe `Author`. (Se você não o vir listado na lista suspensa, certifique-se de que você criou o projeto.)
2. Marque "usar ações do controlador assíncrono".
3. Deixe o nome do controlador como &quot;AuthorsController&quot;.
4. Clique no botão mais (+) ao lado de **classe de contexto de dados**.

![](part-2/_static/image5.png)

Na caixa de diálogo **novo contexto de dados** , deixe o nome padrão e clique em **Adicionar**.

![](part-2/_static/image6.png)

Clique em **Adicionar** para concluir a caixa de diálogo **Adicionar controlador** . A caixa de diálogo adiciona duas classes ao seu projeto:

- `AuthorsController` define um controlador de API da Web. O controlador implementa a API REST que os clientes usam para executar operações CRUD na lista de autores.
- o `BookServiceContext` gerencia objetos de entidade durante o tempo de execução, o que inclui o preenchimento de objetos com dados de um Database, controle de alterações e persistência de dados no banco de dado. Herda de `DbContext`.

![](part-2/_static/image7.png)

Neste ponto, compile o projeto novamente. Agora, siga as mesmas etapas para adicionar um controlador de API para `Book` entidades. Desta vez, selecione `Book` para a classe de modelo e selecione a classe `BookServiceContext` existente para a classe de contexto de dados. (Não crie um novo contexto de dados.) Clique em **Adicionar** para adicionar o controlador.

![](part-2/_static/image8.png)

> [!div class="step-by-step"]
> [Anterior](part-1.md)
> [Próximo](part-3.md)
