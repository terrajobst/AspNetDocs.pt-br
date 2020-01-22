---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: Criando uma cadeia de conexão e trabalhando com SQL Server LocalDB | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: d3c6e736c5dcf4a3615e3c72cfc033effc7cc8e6
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519304"
---
# <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Criar uma cadeia de conexão e trabalhando com LocalDB do SQL Server

por [Rick Anderson]((https://twitter.com/RickAndMSFT))

[!INCLUDE [Tutorial Note](index.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Criar uma cadeia de conexão e trabalhando com LocalDB do SQL Server

A classe `MovieDBContext` que você criou manipula a tarefa de conexão com o banco de dados e o mapeamento de objetos `Movie` para registros de banco de dados. No entanto, uma pergunta que você pode fazer é como especificar a qual banco de dados ele será conectado. Na verdade, você não precisa especificar qual banco de dados usar, Entity Framework usará o [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb)como padrão. Nesta seção, adicionaremos explicitamente uma cadeia de conexão no arquivo *Web. config* do aplicativo.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

O [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) é uma versão leve do Mecanismo de Banco de Dados de SQL Server Express que inicia sob demanda e é executado no modo de usuário. O LocalDB é executado em um modo de execução especial de SQL Server Express que permite que você trabalhe com bancos de dados como arquivos *. MDF* . Normalmente, os arquivos de banco de dados LocalDB são mantidos na pasta *Data\_do aplicativo* de um projeto Web.

O SQL Server Express não é recomendado para uso em aplicativos Web de produção. O LocalDB, em particular, não deve ser usado para produção com um aplicativo Web porque ele não foi projetado para funcionar com o IIS. No entanto, um banco de dados LocalDB pode ser facilmente migrado para SQL Server ou SQL Azure.

No Visual Studio 2017, o LocalDB é instalado por padrão com o Visual Studio.

Por padrão, a Entity Framework procura uma cadeia de conexão denominada igual à classe de contexto de objeto (`MovieDBContext` para este projeto). Para obter mais informações, consulte [SQL Server cadeias de conexão para aplicativos Web ASP.net](https://msdn.microsoft.com/library/jj653752.aspx).

Abra o arquivo *Web. config da* raiz do aplicativo mostrado abaixo. (Não o arquivo *Web. config* na pasta *views* .)

![](creating-a-connection-string/_static/image1.png)

Localize o elemento `<connectionStrings>`:

![](creating-a-connection-string/_static/image2.png)

Adicione a seguinte cadeia de conexão ao elemento `<connectionStrings>` no arquivo *Web. config* .

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

O exemplo a seguir mostra uma parte do arquivo *Web. config* com a nova cadeia de conexão adicionada:

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

As duas cadeias de conexão são muito semelhantes. A primeira cadeia de conexão é denominada `DefaultConnection` e é usada para que o banco de dados de associação controle quem pode acessar o aplicativo. A cadeia de conexão adicionada especifica um banco de dados LocalDB chamado *Movie. MDF* localizado na pasta de *dados do aplicativo\_* . Não usaremos o banco de dados Membership neste tutorial, para obter mais informações sobre associação, autenticação e segurança, consulte meu tutorial [criar um aplicativo MVC ASP.NET com autenticação e banco de dados SQL e implantar no serviço Azure app](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).

O nome da cadeia de conexão deve corresponder ao nome da classe [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) .

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

Na verdade, você não precisa adicionar a cadeia de conexão `MovieDBContext`. Se você não especificar uma cadeia de conexão, Entity Framework criará um banco de dados LocalDB no diretório Users com o nome totalmente qualificado da classe [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) (nesse caso, `MvcMovie.Models.MovieDBContext`). Você pode nomear o banco de dados como desejar, desde que ele tenha o *.* Sufixo de MDF. Por exemplo, poderíamos nomear o banco de dados *Myfilmes. MDF*.

Em seguida, você criará uma nova classe `MoviesController` que pode ser usada para exibir os dados do filme e permitir que os usuários criem novas listagens de filmes.

> [!div class="step-by-step"]
> [Anterior](adding-a-model.md)
> [Próximo](accessing-your-models-data-from-a-controller.md)
