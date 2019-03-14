---
ms.openlocfilehash: 22c540058e0b3b9ae612f1b2612cecc08627ea2b
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57063983"
---
<a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="eb2e6-101">Testar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="eb2e6-101">Test the app</span></span>

* <span data-ttu-id="eb2e6-102">Executar o aplicativo e acrescentar `/Movies` à URL no navegador (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="eb2e6-102">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="eb2e6-103">Teste o link **Criar**.</span><span class="sxs-lookup"><span data-stu-id="eb2e6-103">Test the **Create** link.</span></span>

  ![Criar página](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="eb2e6-105">Teste os links **Editar**, **Detalhes** e **Excluir**.</span><span class="sxs-lookup"><span data-stu-id="eb2e6-105">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="eb2e6-106">Se você receber o seguinte erro, verifique se você executou migrações e atualizou o banco de dados:</span><span class="sxs-lookup"><span data-stu-id="eb2e6-106">If you get the following error, verify you have run migrations and updated the database:</span></span>

```
An unhandled exception occurred while processing the request.
SqliteException: SQLite Error 1: 'no such table: Movie'.
Microsoft.Data.Sqlite.SqliteException.ThrowExceptionForRC(int rc, sqlite3 db)
```
