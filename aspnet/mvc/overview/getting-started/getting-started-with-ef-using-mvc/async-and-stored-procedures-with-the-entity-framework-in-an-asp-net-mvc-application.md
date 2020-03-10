---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Tutorial: usar os procedimentos assíncronos e armazenados com o EF em um aplicativo MVC ASP.NET'
description: Neste tutorial, você verá como implementar o modelo de programação assíncrona e aprender a usar procedimentos armazenados.
author: tdykstra
ms.author: riande
ms.date: 01/18/2019
ms.topic: tutorial
ms.assetid: 27d110fc-d1b7-4628-a763-26f1e6087549
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 5612f2f25d06feb904a205505ed8f048d2263266
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583436"
---
# <a name="tutorial-use-async-and-stored-procedures-with-ef-in-an-aspnet-mvc-app"></a>Tutorial: usar os procedimentos assíncronos e armazenados com o EF em um aplicativo MVC ASP.NET

Nos tutoriais anteriores, você aprendeu a ler e atualizar dados usando o modelo de programação síncrona. Neste tutorial, você verá como implementar o modelo de programação assíncrona. O código assíncrono pode ajudar um aplicativo a funcionar melhor porque faz uso melhor dos recursos do servidor.

Neste tutorial, você também verá como usar procedimentos armazenados para operações de inserção, atualização e exclusão em uma entidade.

Por fim, reimplante o aplicativo no Azure, juntamente com todas as alterações de banco de dados que você implementou desde a primeira vez que você implantou.

As ilustrações a seguir mostram algumas das páginas com as quais você trabalhará.

![Página departamentos](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Criar departamento](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Neste tutorial, você:

> [!div class="checklist"]
> * Saiba mais sobre o código assíncrono
> * Criar um controlador de departamento
> * Usar procedimentos armazenados
> * Implantar no Azure

## <a name="prerequisites"></a>Prerequisites

* [Atualização de dados relacionados](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="why-use-asynchronous-code"></a>Por que usar código assíncrono

Um servidor Web tem um número limitado de threads disponíveis e, em situações de alta carga, todos os threads disponíveis podem estar em uso. Quando isso acontece, o servidor não pode processar novas solicitações até que os threads são liberados. Com um código síncrono, muitos threads podem ser vinculados enquanto realmente não são fazendo nenhum trabalho porque estão aguardando a conclusão da E/S. Com um código assíncrono, quando um processo está aguardando a conclusão da E/S, seu thread é liberado para o servidor para ser usado para processar outras solicitações. Como resultado, o código assíncrono permite que os recursos do servidor sejam usados com mais eficiência e o servidor está habilitado para lidar com mais tráfego sem atrasos.

Em versões anteriores do .NET, escrever e testar código assíncrono era complexo, propenso a erros e difícil de depurar. No .NET 4,5, escrever, testar e depurar código assíncrono é muito mais fácil que você geralmente deve escrever código assíncrono, a menos que tenha um motivo para não. O código assíncrono apresenta uma pequena quantidade de sobrecarga, mas para situações de tráfego baixo, o impacto no desempenho é insignificante, enquanto para situações de tráfego alto, a possível melhoria no desempenho é substancial.

Para obter mais informações sobre programação assíncrona, consulte [usar o suporte assíncrono do .NET 4.5 para evitar chamadas de bloqueio](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).

## <a name="create-department-controller"></a>Criar controlador de departamento

Crie um controlador de departamento da mesma maneira que fez com os controladores anteriores, exceto desta vez marque a caixa de seleção **usar ações do controlador assíncrono** .

Os destaques a seguir mostram o que foi adicionado ao código síncrono para o método `Index` para torná-lo assíncrono:

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=1,4)]

Quatro alterações foram aplicadas para permitir que o Entity Framework consulta de banco de dados seja executado de forma assíncrona:

- O método é marcado com a palavra-chave `async`, que informa ao compilador para gerar retornos de chamada para partes do corpo do método e criar automaticamente o objeto de `Task<ActionResult>` retornado.
- O tipo de retorno foi alterado de `ActionResult` para `Task<ActionResult>`. O tipo de `Task<T>` representa o trabalho em andamento com um resultado do tipo `T`.
- A palavra-chave `await` foi aplicada à chamada de serviço Web. Quando o compilador vê essa palavra-chave, por trás das cenas, ela divide o método em duas partes. A primeira parte termina com a operação que é iniciada de forma assíncrona. A segunda parte é colocada em um método de retorno de chamada que é chamado quando a operação é concluída.
- A versão assíncrona do método de extensão de `ToList` foi chamada.

Por que a instrução `departments.ToList` modificada, mas não a instrução `departments = db.Departments`? O motivo é que apenas as instruções que fazem com que consultas ou comandos sejam enviadas ao banco de dados são executadas de forma assíncrona. A instrução `departments = db.Departments` configura uma consulta, mas a consulta não é executada até que o método `ToList` seja chamado. Portanto, somente o método `ToList` é executado de forma assíncrona.

No método `Details` e os métodos `HttpGet` `Edit` e `Delete`, o método `Find` é aquele que faz com que uma consulta seja enviada ao banco de dados, portanto, esse é o método que é executado de forma assíncrona:

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs?highlight=7)]

Nos métodos `Create`, `HttpPost Edit`e `DeleteConfirmed`, é a chamada do método `SaveChanges` que faz com que um comando seja executado, não instruções como `db.Departments.Add(department)` que fazem com que apenas entidades na memória sejam modificadas.

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=6)]

Abra *Views\Department\Index.cshtml*e substitua o código do modelo pelo código a seguir:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=3,5,20-22,36-38)]

Esse código altera o título de índice para departamentos, move o nome do administrador para a direita e fornece o nome completo do administrador.

Nas exibições criar, excluir, detalhar e editar, altere a legenda do campo `InstructorID` para "administrador" da mesma maneira que você alterou o campo nome do departamento para "departamento" nos modos de exibição do curso.

Nas exibições criar e editar, use o seguinte código:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml)]

Nas exibições excluir e detalhes, use o seguinte código:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Execute o aplicativo e clique na guia **departamentos** .

Tudo funciona da mesma forma que nos outros controladores, mas nesse controlador todas as consultas SQL são executadas de forma assíncrona.

Algumas coisas a serem consideradas quando você estiver usando a programação assíncrona com o Entity Framework:

- O código assíncrono não é thread-safe. Em outras palavras, em outras palavras, não tente realizar várias operações em paralelo usando a mesma instância de contexto.
- Se desejar aproveitar os benefícios de desempenho do código assíncrono, verifique se os pacotes de biblioteca que você está usando (por exemplo, para paginação) também usam o código assíncrono se eles chamam métodos do Entity Framework que fazem com que consultas sejam enviadas ao banco de dados.

## <a name="use-stored-procedures"></a>Usar procedimentos armazenados

Alguns desenvolvedores e DBAs preferem usar procedimentos armazenados para acesso ao banco de dados. Em versões anteriores do Entity Framework você pode recuperar dados usando um procedimento armazenado [executando uma consulta SQL bruta](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), mas não pode instruir o EF a usar procedimentos armazenados para operações de atualização. No EF 6, é fácil configurar Code First usar procedimentos armazenados.

1. No *DAL\SchoolContext.cs*, adicione o código realçado ao método `OnModelCreating`.

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=9)]

    Esse código instrui Entity Framework a usar procedimentos armazenados para operações de inserção, atualização e exclusão na entidade `Department`.
2. No console gerenciar do pacote, digite o seguinte comando:

    `add-migration DepartmentSP`

    As *migrações abertas\\carimbo de data/hora &lt;&gt;\_DepartmentSP.cs* para ver o código no método `Up` que cria procedimentos armazenados INSERT, Update e Delete:

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs?highlight=3-4,26-27,42-43)]
3. No console gerenciar do pacote, digite o seguinte comando:

     `update-database`
4. Execute o aplicativo no modo de depuração, clique na guia **departamentos** e, em seguida, clique em **criar novo**.
5. Insira dados para um novo departamento e, em seguida, clique em **criar**.

6. No Visual Studio, examine os logs na janela **saída** para ver que um procedimento armazenado foi usado para inserir a nova linha de departamento.

     ![SP de inserção do departamento](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Code First cria nomes de procedimento armazenado padrão. Se você estiver usando um banco de dados existente, talvez seja necessário personalizar os nomes dos procedimentos armazenados para usar os procedimentos armazenados já definidos no banco de dados. Para obter informações sobre como fazer isso, consulte [Entity Framework Code First inserir/atualizar/excluir procedimentos armazenados](https://msdn.microsoft.com/data/dn468673).

Se desejar personalizar o que os procedimentos armazenados gerados fazem, você pode editar o código com Scaffold para as migrações `Up` método que cria o procedimento armazenado. Dessa forma, as alterações serão refletidas sempre que a migração for executada e serão aplicadas ao seu banco de dados de produção quando as migrações forem executadas automaticamente na produção após a implantação.

Se você quiser alterar um procedimento armazenado existente que foi criado em uma migração anterior, poderá usar o comando Add-Migration para gerar uma migração em branco e, em seguida, escrever manualmente o código que chama o método [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) .

## <a name="deploy-to-azure"></a>Implantar no Azure

Esta seção exige que você tenha concluído a seção **implantação opcional do aplicativo no Azure** no tutorial de [migrações e implantação](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) desta série. Se houver erros de migrações que você resolveu excluindo o banco de dados em seu projeto local, pule esta seção.

1. No Visual Studio, clique com o botão direito do mouse no projeto no **Gerenciador de Soluções** e selecione **Publicar** no menu de contexto.
2. Clique em **Publicar**.

    O Visual Studio implanta o aplicativo no Azure e o aplicativo é aberto no navegador padrão, em execução no Azure.
3. Teste o aplicativo para verificar se ele está funcionando.

    Na primeira vez que você executar uma página que acessa o banco de dados do, a Entity Framework executará todas as migrações `Up` métodos necessários para atualizar o banco de dados com o modelo de dado atual. Agora você pode usar todas as páginas da Web que adicionou desde a última vez que implantou o, incluindo as páginas do departamento que você adicionou neste tutorial.

## <a name="get-the-code"></a>Obter o código

[Baixar o projeto concluído](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Recursos adicionais

Links para outros recursos de Entity Framework podem ser encontrados nos [recursos de acesso a dados ASP.net-recomendado](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você:

> [!div class="checklist"]
> * Aprendeu sobre código assíncrono
> * Criou um controlador de departamento
> * Procedimentos armazenados usados
> * Implantado no Azure

Avance para o próximo artigo para saber como lidar com conflitos quando vários usuários atualizam a mesma entidade ao mesmo tempo.
> [!div class="nextstepaction"]
> [Manipulando a simultaneidade](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
