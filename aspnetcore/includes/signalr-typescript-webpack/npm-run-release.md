---
ms.openlocfilehash: 939231583679d469d01724cc1a405bd45e46d2c5
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57175871"
---
```console
npm run release
```

<span data-ttu-id="0a841-101">Este comando suspende o fornecimento dos ativos do lado do cliente ao executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0a841-101">This command yields the client-side assets to be served when running the app.</span></span> <span data-ttu-id="0a841-102">Os ativos são colocados na pasta *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="0a841-102">The assets are placed in the *wwwroot* folder.</span></span>

<span data-ttu-id="0a841-103">O Webpack concluiu as seguintes tarefas:</span><span class="sxs-lookup"><span data-stu-id="0a841-103">Webpack completed the following tasks:</span></span>

* <span data-ttu-id="0a841-104">Limpou os conteúdos do diretório *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="0a841-104">Purged the contents of the *wwwroot* directory.</span></span>
* <span data-ttu-id="0a841-105">Converteu o TypeScript para JavaScript, um processo conhecido como *transpilação*.</span><span class="sxs-lookup"><span data-stu-id="0a841-105">Converted the TypeScript to JavaScript&mdash;a process known as *transpilation*.</span></span>
* <span data-ttu-id="0a841-106">Reduziu o tamanho do arquivo JavaScript gerado, um processo conhecido como *minificação*.</span><span class="sxs-lookup"><span data-stu-id="0a841-106">Mangled the generated JavaScript to reduce file size&mdash;a process known as *minification*.</span></span>
* <span data-ttu-id="0a841-107">Copiou os arquivos JavaScript, CSS e HTML processados do *src* para o diretório *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="0a841-107">Copied the processed JavaScript, CSS, and HTML files from *src* to the *wwwroot* directory.</span></span>
* <span data-ttu-id="0a841-108">Injetou os seguintes elementos no arquivo *wwwroot/index.html*:</span><span class="sxs-lookup"><span data-stu-id="0a841-108">Injected the following elements into the *wwwroot/index.html* file:</span></span>
    * <span data-ttu-id="0a841-109">Uma marca `<link>`, que referencia o arquivo *wwwroot/main.\<hash\>.css*.</span><span class="sxs-lookup"><span data-stu-id="0a841-109">A `<link>` tag, referencing the *wwwroot/main.\<hash\>.css* file.</span></span> <span data-ttu-id="0a841-110">Essa marca é colocada imediatamente antes do fim da marca `</head>`.</span><span class="sxs-lookup"><span data-stu-id="0a841-110">This tag is placed immediately before the closing `</head>` tag.</span></span>
    * <span data-ttu-id="0a841-111">Uma marca `<script>`, que referencia o arquivo minificado *wwwroot/main.\<hash\>.js*.</span><span class="sxs-lookup"><span data-stu-id="0a841-111">A `<script>` tag, referencing the minified *wwwroot/main.\<hash\>.js* file.</span></span> <span data-ttu-id="0a841-112">Essa marca é colocada imediatamente antes do fim da marca `</body>`.</span><span class="sxs-lookup"><span data-stu-id="0a841-112">This tag is placed immediately before the closing `</body>` tag.</span></span>
