---
ms.openlocfilehash: 85fa9f8b18dc7dfb3e9d37703549dc5c28dc7a4a
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57062593"
---
<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a>Fazer scaffolding do modelo de filme

* Execute o seguinte na linha de comando (o diretório do projeto que contém os arquivos *Program.cs*, *Startup.cs* e *.csproj*):

  ```console
  dotnet restore
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

Se você obtiver o erro:
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

O erro anterior ocorre quando você está no diretório errado. Abra um shell de comando para o diretório do projeto (o diretório que contém os arquivos *Program.cs*, *Startup.cs* e *.csproj*) e, em seguida, execute o comando anterior.

Se você obtiver o erro:
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

Saia do Visual Studio e execute o comando novamente.
