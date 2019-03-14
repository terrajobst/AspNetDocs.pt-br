---
ms.openlocfilehash: 85fa9f8b18dc7dfb3e9d37703549dc5c28dc7a4a
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57062593"
---
<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="a6561-101">Fazer scaffolding do modelo de filme</span><span class="sxs-lookup"><span data-stu-id="a6561-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="a6561-102">Execute o seguinte na linha de comando (o diretório do projeto que contém os arquivos *Program.cs*, *Startup.cs* e *.csproj*):</span><span class="sxs-lookup"><span data-stu-id="a6561-102">Run the following from the command line (in the project directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files):</span></span>

  ```console
  dotnet restore
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

<span data-ttu-id="a6561-103">Se você obtiver o erro:</span><span class="sxs-lookup"><span data-stu-id="a6561-103">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="a6561-104">O erro anterior ocorre quando você está no diretório errado.</span><span class="sxs-lookup"><span data-stu-id="a6561-104">The preceeding error happens when you are in the wrong directory.</span></span> <span data-ttu-id="a6561-105">Abra um shell de comando para o diretório do projeto (o diretório que contém os arquivos *Program.cs*, *Startup.cs* e *.csproj*) e, em seguida, execute o comando anterior.</span><span class="sxs-lookup"><span data-stu-id="a6561-105">Open a command shell to the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files), and then run the preceeding command.</span></span>

<span data-ttu-id="a6561-106">Se você obtiver o erro:</span><span class="sxs-lookup"><span data-stu-id="a6561-106">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="a6561-107">Saia do Visual Studio e execute o comando novamente.</span><span class="sxs-lookup"><span data-stu-id="a6561-107">Exit Visual Studio and run the command again.</span></span>
