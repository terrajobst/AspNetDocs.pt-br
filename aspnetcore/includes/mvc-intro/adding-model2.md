---
ms.openlocfilehash: f323482d6f8bfaebf7bf6673d5fb91608430760a
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57044073"
---
## <a name="add-initial-migration-and-update-the-database"></a>Adicione a migração inicial e atualize o banco de dados

* Abra um prompt de comando e navegue até o diretório do projeto. (O diretório que contém o *Startup.cs* arquivo).

* Execute os seguintes comandos no prompt de comando:

  ```console
  dotnet restore
  dotnet ef migrations add Initial
  dotnet ef database update
  ```
  
  [.NET core](/dotnet/core/tools/index) é uma implementação de plataforma cruzada do .NET. Aqui está o que fazem estes comandos:

  * [dotnet restore](/dotnet/core/tools/dotnet-restore): Baixa os pacotes do NuGet especificados na *. csproj* arquivo.
  * `dotnet ef migrations add Initial` Executa o comando de migrações do Entity Framework a CLI do .NET Core e cria a migração inicial. O parâmetro depois de "Adicionar" é um nome que você atribui para a migração. Aqui você está nomeando a migração "Inicial" porque ele é a migração de banco de dados inicial. Esta operação cria o *dados/migrações/\<data e hora > Initial* arquivo que contém os comandos de migração para adicionar o *filme* tabela no banco de dados.
  * `dotnet ef database update`  Atualiza o banco de dados com a migração que acabamos de criar.

Você saberá mais sobre a cadeia de caracteres de conexão e de banco de dados no próximo tutorial. Você saberá mais sobre as alterações do modelo de dados na [adicionar um campo](xref:tutorials/first-mvc-app/new-field) tutorial.
