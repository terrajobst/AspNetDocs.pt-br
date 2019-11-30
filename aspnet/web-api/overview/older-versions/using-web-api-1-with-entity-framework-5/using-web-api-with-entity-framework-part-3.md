---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 'Parte 3: Criando um controlador de administração | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: f39be7a84e85db93487d246e9f8cb59c401fe5ce
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600036"
---
# <a name="part-3-creating-an-admin-controller"></a>Parte 3: Criando um controlador de administração

por [Mike Wasson](https://github.com/MikeWasson)

[Baixar projeto concluído](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a>Adicionar um controlador de administrador

Nesta seção, adicionaremos um controlador de API Web que oferece suporte a operações CRUD (criar, ler, atualizar e excluir) em produtos. O controlador usará Entity Framework para se comunicar com a camada de banco de dados. Somente os administradores poderão usar esse controlador. Os clientes acessarão os produtos por meio de outro controlador.

Em Gerenciador de Soluções, clique com o botão direito do mouse na pasta controladores. Selecione **Adicionar** e, em seguida, **controlador**.

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

Na caixa de diálogo **Adicionar controlador** , nomeie o controlador `AdminController`. Em **modelo**, selecione &quot;controlador de API com ações de leitura/gravação, usando Entity Framework&quot;. Em **classe de modelo**, selecione "produto (ProductStore. Models)". Em **contexto de dados**, selecione "&lt;novo contexto de dados&gt;".

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> Se a lista suspensa **classe de modelo** não mostrar classes de modelo, verifique se você compilou o projeto. Entity Framework usa reflexão, portanto, precisa do assembly compilado.

Se você selecionar "&lt;novo contexto de dados&gt;" abrirá a caixa de diálogo **novo contexto de dados** . Nomeie o `ProductStore.Models.OrdersContext`de contexto de dados.

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

Clique em **OK** para ignorar a caixa de diálogo **novo contexto de dados** . Na caixa de diálogo **Adicionar controlador** , clique em **Adicionar**.

Veja o que foi adicionado ao projeto:

- Uma classe chamada `OrdersContext` que deriva de **DbContext**. Essa classe fornece a cola entre os modelos POCO e o banco de dados.
- Um controlador de API da Web chamado `AdminController`. Esse controlador oferece suporte a operações CRUD em instâncias de `Product`. Ele usa a classe `OrdersContext` para se comunicar com Entity Framework.
- Uma nova cadeia de conexão de banco de dados no arquivo Web. config.

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

Abra o arquivo OrdersContext.cs. Observe que o construtor Especifica o nome da cadeia de conexão do banco de dados. Esse nome se refere à cadeia de conexão que foi adicionada ao Web. config.

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

Adicione as seguintes propriedades à classe `OrdersContext`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

Um **DbSet** representa um conjunto de entidades que podem ser consultadas. Aqui está a listagem completa para a classe `OrdersContext`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

A classe `AdminController` define cinco métodos que implementam a funcionalidade CRUD básica. Cada método corresponde a um URI que o cliente pode invocar:

| Método de controlador | Descrição | {1&gt;URI&lt;1} | Método HTTP |
| --- | --- | --- | --- |
| GetProducts | Obtém todos os produtos. | API/produtos | Obter |
| Getproduct | Localiza um produto por ID. | API/produtos/*ID* | Obter |
| PutProduct | Atualiza um produto. | API/produtos/*ID* | PUT |
| Produto | Cria um novo produto. | API/produtos | Postar |
| DeleteProduct | Exclui um produto. | API/produtos/*ID* | DELETE |

Cada método chama `OrdersContext` para consultar o banco de dados. Os métodos que modificam a coleção (PUT, POST e DELETE) chamam `db.SaveChanges` para persistir as alterações no banco de dados. Os controladores são criados por solicitação HTTP e, em seguida, descartados, portanto, é necessário manter as alterações antes que um método seja retornado.

## <a name="add-a-database-initializer"></a>Adicionar um inicializador de banco de dados

Entity Framework tem um bom recurso que permite que você preencha o banco de dados na inicialização e recrie o banco de dados automaticamente sempre que os modelos forem alterados. Esse recurso é útil durante o desenvolvimento, porque você sempre tem alguns dados de teste, mesmo se você alterar os modelos.

Em Gerenciador de Soluções, clique com o botão direito do mouse na pasta modelos e crie uma nova classe chamada `OrdersContextInitializer`. Cole a seguinte implementação:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

Ao herdar da classe **DropCreateDatabaseIfModelChanges** , estamos dizendo Entity Framework para descartar o banco de dados sempre que modificarmos as classes de modelo. Quando Entity Framework cria (ou recria) o banco de dados, ele chama o método **semente** para popular as tabelas. Usamos o método **semente** para adicionar alguns produtos de exemplo mais um exemplo de ordem.

Esse recurso é ótimo para teste, mas não use a classe **DropCreateDatabaseIfModelChanges** em produção, porque você poderá perder seus dados se alguém alterar uma classe de modelo.

Em seguida, abra global. asax e adicione o seguinte código ao **aplicativo\_** método de início:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a>Enviar uma solicitação para o controlador

Neste ponto, não escrevemos nenhum código de cliente, mas você pode invocar a API Web usando um navegador da Web ou uma ferramenta de depuração de HTTP, como o [Fiddler](http://www.fiddler2.com/fiddler2/). No Visual Studio, pressione F5 para iniciar a depuração. Seu navegador da Web será aberto para `http://localhost:*portnum*/`, em que *PortNum* é um número de porta.

Enviar uma solicitação HTTP para "`http://localhost:*portnum*/api/admin`. A primeira solicitação pode ser lenta para ser concluída, pois Entity Framework precisa criar e propagar o banco de dados. A resposta deve ter algo semelhante ao seguinte:

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> [Anterior](using-web-api-with-entity-framework-part-2.md)
> [Próximo](using-web-api-with-entity-framework-part-4.md)
