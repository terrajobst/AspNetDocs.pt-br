---
ms.openlocfilehash: f323482d6f8bfaebf7bf6673d5fb91608430760a
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57044073"
---
## <a name="add-initial-migration-and-update-the-database"></a><span data-ttu-id="14776-101">Adicione a migração inicial e atualize o banco de dados</span><span class="sxs-lookup"><span data-stu-id="14776-101">Add initial migration and update the database</span></span>

* <span data-ttu-id="14776-102">Abra um prompt de comando e navegue até o diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="14776-102">Open a command prompt and navigate to the project directory.</span></span> <span data-ttu-id="14776-103">(O diretório que contém o *Startup.cs* arquivo).</span><span class="sxs-lookup"><span data-stu-id="14776-103">(The directory containing the *Startup.cs* file).</span></span>

* <span data-ttu-id="14776-104">Execute os seguintes comandos no prompt de comando:</span><span class="sxs-lookup"><span data-stu-id="14776-104">Run the following commands in the command prompt:</span></span>

  ```console
  dotnet restore
  dotnet ef migrations add Initial
  dotnet ef database update
  ```
  
  <span data-ttu-id="14776-105">[.NET core](/dotnet/core/tools/index) é uma implementação de plataforma cruzada do .NET.</span><span class="sxs-lookup"><span data-stu-id="14776-105">[.NET Core](/dotnet/core/tools/index) is a cross-platform implementation of .NET.</span></span> <span data-ttu-id="14776-106">Aqui está o que fazem estes comandos:</span><span class="sxs-lookup"><span data-stu-id="14776-106">Here is what these commands do:</span></span>

  * <span data-ttu-id="14776-107">[dotnet restore](/dotnet/core/tools/dotnet-restore): Baixa os pacotes do NuGet especificados na *. csproj* arquivo.</span><span class="sxs-lookup"><span data-stu-id="14776-107">[dotnet restore](/dotnet/core/tools/dotnet-restore): Downloads the NuGet packages specified in the *.csproj* file.</span></span>
  * <span data-ttu-id="14776-108">`dotnet ef migrations add Initial` Executa o comando de migrações do Entity Framework a CLI do .NET Core e cria a migração inicial.</span><span class="sxs-lookup"><span data-stu-id="14776-108">`dotnet ef migrations add Initial` Runs the Entity Framework .NET Core CLI migrations command and creates the initial migration.</span></span> <span data-ttu-id="14776-109">O parâmetro depois de "Adicionar" é um nome que você atribui para a migração.</span><span class="sxs-lookup"><span data-stu-id="14776-109">The parameter after "add" is a name that you assign to the migration.</span></span> <span data-ttu-id="14776-110">Aqui você está nomeando a migração "Inicial" porque ele é a migração de banco de dados inicial.</span><span class="sxs-lookup"><span data-stu-id="14776-110">Here you're naming the migration "Initial" because it's the initial database migration.</span></span> <span data-ttu-id="14776-111">Esta operação cria o *dados/migrações/\<data e hora > Initial* arquivo que contém os comandos de migração para adicionar o *filme* tabela no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="14776-111">This operation creates the *Data/Migrations/\<date-time>_Initial.cs* file containing the migration commands to add the *Movie* table to the database.</span></span>
  * <span data-ttu-id="14776-112">`dotnet ef database update`  Atualiza o banco de dados com a migração que acabamos de criar.</span><span class="sxs-lookup"><span data-stu-id="14776-112">`dotnet ef database update`  Updates the database with the migration we just created.</span></span>

<span data-ttu-id="14776-113">Você saberá mais sobre a cadeia de caracteres de conexão e de banco de dados no próximo tutorial.</span><span class="sxs-lookup"><span data-stu-id="14776-113">You'll learn about the database and connection string in the next tutorial.</span></span> <span data-ttu-id="14776-114">Você saberá mais sobre as alterações do modelo de dados na [adicionar um campo](xref:tutorials/first-mvc-app/new-field) tutorial.</span><span class="sxs-lookup"><span data-stu-id="14776-114">You'll learn about data model changes in the [Add a field](xref:tutorials/first-mvc-app/new-field) tutorial.</span></span>
