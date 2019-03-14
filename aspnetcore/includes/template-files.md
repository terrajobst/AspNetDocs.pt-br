---
ms.openlocfilehash: eb2f83504129ae2b19ff5d85a2bc7d90293b43d6
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57043393"
---
* Startup.cs: [Classe de inicialização](xref:fundamentals/startup) -classe configura o pipeline de solicitação que manipula todas as solicitações feitas ao aplicativo.
* Program.cs : [Classe de programa](xref:fundamentals/index) que contém o ponto de entrada principal do aplicativo.
* firstapp.csproj : [Arquivo de projeto](/dotnet/articles/core/preview3/tools/csproj) formato de arquivo de projeto do MSBuild para aplicativos ASP.NET Core. Contém referências de projeto a projeto, referências de NuGet e outros itens relacionados do projeto.
* appSettings. JSON / appsettings. Development.JSON: Arquivo de configuração de configurações do ambiente base de aplicativo. [Consulte a configuração](xref:fundamentals/configuration/index).
* bower.json : Dependências de pacote bower para o projeto.
* .bowerrc : Arquivo de configuração do bower que define onde instalar os componentes quando Bower baixa os ativos.
* bundleconfig.JSON: arquivos de configuração para o agrupamento e minificação de ativos front-end de JavaScript e CSS.
* Modos de exibição: Contém os modos de exibição do Razor. são os componentes que exibem a interface do usuário do aplicativo. Em geral, essa interface do usuário exibe os dados de modelo.
* Controladores: Contém os controladores do MVC, inicialmente *HomeController.cs*. Os controladores são classes que manipulam as solicitações do navegador.
* wwwroot: Pasta raiz do aplicativo Web.

Para obter mais informações, consulte [o padrão MVC padrão](xref:mvc/overview).
