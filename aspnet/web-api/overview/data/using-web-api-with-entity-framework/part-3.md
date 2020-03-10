---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: Usar Migrações do Code First para propagar o banco de dados | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: 257bd06848adb949330856cc71eeb3d685e9d036
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557452"
---
# <a name="use-code-first-migrations-to-seed-the-database"></a>Usar Migrações do Code First para propagar o banco de dados

por [Mike Wasson](https://github.com/MikeWasson)

[Baixar projeto concluído](https://github.com/MikeWasson/BookService)

Nesta seção, você usará o [migrações do Code First](https://msdn.microsoft.com/data/jj591621) no EF para propagar o banco de dados com dado de teste.

No menu **ferramentas** , selecione **Gerenciador de pacotes NuGet**e, em seguida, selecione **console do Gerenciador de pacotes**. Na janela Console do Gerenciador de Pacotes, digite o seguinte comando:

[!code-console[Main](part-3/samples/sample1.cmd)]

Esse comando adiciona uma pasta chamada migrações ao seu projeto, além de um arquivo de código chamado Configuration.cs na pasta Migrations.

![](part-3/_static/image1.png)

Abra o arquivo Configuration.cs. Adicione a seguinte instrução **using** .

[!code-csharp[Main](part-3/samples/sample2.cs)]

Em seguida, adicione o seguinte código ao método **Configuration. semente** :

[!code-csharp[Main](part-3/samples/sample3.cs)]

Na janela do console do Gerenciador de pacotes, digite os seguintes comandos:

[!code-console[Main](part-3/samples/sample4.cmd)]

O primeiro comando gera o código que cria o banco de dados e o segundo comando executa esse código. O banco de dados é criado localmente, usando o [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a>Explorar a API (opcional)

Pressione F5 para executar o aplicativo em modo de depuração. O Visual Studio inicia IIS Express e executa seu aplicativo Web. Em seguida, o Visual Studio inicia um navegador e abre a home page do aplicativo.

Quando o Visual Studio executa um projeto Web, ele atribui um número de porta. Na imagem abaixo, o número da porta é 50524. Ao executar o aplicativo, você verá um número de porta diferente.

![](part-3/_static/image3.png)

O home page é implementado usando o MVC do ASP.NET. Na parte superior da página, há um link que diz "API". Esse link leva você a uma página de ajuda gerada automaticamente para a API Web. (Para saber como essa página de ajuda é gerada e como você pode adicionar sua própria documentação à página, consulte [criando páginas de ajuda para ASP.NET Web API](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) Você pode clicar nos links da página de ajuda para ver detalhes sobre a API, incluindo o formato de solicitação e resposta.

![](part-3/_static/image4.png)

A API habilita as operações CRUD no banco de dados. O seguinte resume a API.

| Autores |  |
| --- | -- |
| GET api/authors | Obter todos os autores. |
| GET api/authors/{id} | Obtenha um autor por ID. |
| POST /api/authors | Crie um novo autor. |
| PUT /api/authors/{id} | Atualizar um autor existente. |
| DELETE /api/authors/{id} | Excluir um autor. |

| Manuais |  |
| --- | -- |
| GET /api/books | Obter todos os livros. |
| GET /api/books/{id} | Obter um livro por ID. |
| POST /api/books | Crie um novo livro. |
| PUT /api/books/{id} | Atualizar um livro existente. |
| DELETE /api/books/{id} | Excluir um livro. |

## <a name="view-the-database-optional"></a>Exibir o banco de dados (opcional)

Quando você executou o comando Update-Database, o EF criou o banco de dados e chamou o método `Seed`. Quando você executa o aplicativo localmente, o EF usa o [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx). Você pode exibir o banco de dados no Visual Studio. No menu **Visualizar**, selecione **Pesquisador de objetos do SQL Server**.

![](part-3/_static/image5.png)

No diálogo **conectar ao servidor** , na caixa de edição **nome do servidor** , digite "(LocalDB) \v11.0". Deixe a opção de **autenticação** como "autenticação do Windows". Clique em **Conectar**.

![](part-3/_static/image6.png)

O Visual Studio se conecta ao LocalDB e mostra os bancos de dados existentes na janela Pesquisador de Objetos do SQL Server. Você pode expandir os nós para ver as tabelas que o EF criou.

![](part-3/_static/image7.png)

Para exibir os dados, clique com o botão direito do mouse em uma tabela e selecione **exibir dados**.

![](part-3/_static/image8.png)

A captura de tela a seguir mostra os resultados da tabela Books. Observe que o EF populava o banco de dados com o semente data e a tabela contém a chave estrangeira para a tabela authors.

![](part-3/_static/image9.png)

> [!div class="step-by-step"]
> [Anterior](part-2.md)
> [Próximo](part-4.md)
