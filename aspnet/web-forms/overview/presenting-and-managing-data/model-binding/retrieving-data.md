---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: Recuperando e exibindo dados com associação de modelo e formulários da Web | Microsoft Docs
author: Rick-Anderson
description: Esta série de tutoriais demonstra os aspectos básicos do uso de associação de modelo com um projeto ASP.NET Web Forms. A associação de modelo torna a interação de dados mais direta-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: 81cca22cb4752d071d2a68986ae9ac2bed737594
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74633181"
---
# <a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a>Recuperando e exibindo dados com associação de modelo e formulários da Web

> Esta série de tutoriais demonstra os aspectos básicos do uso de associação de modelo com um projeto ASP.NET Web Forms. A associação de modelo torna a interação de dados mais direta do que lidar com objetos de fonte de dados (como ObjectDataSource ou SqlDataSource). Esta série começa com material introdutório e passa para conceitos mais avançados em Tutoriais posteriores.
> 
> O padrão de associação de modelo funciona com qualquer tecnologia de acesso a dados. Neste tutorial, você usará Entity Framework, mas poderá usar a tecnologia de acesso a dados mais familiar para você. De um controle de servidor associado a dados, como um controle GridView, ListView, DetailsView ou FormView, você especifica os nomes dos métodos a serem usados para selecionar, atualizar, excluir e criar dados. Neste tutorial, você especificará um valor para SelectMethod. 
> 
> Dentro desse método, você fornece a lógica para recuperar os dados. No próximo tutorial, você definirá valores para UpdateMethod, DeleteMethod e InsertMethod.
>
> Você pode [baixar](https://go.microsoft.com/fwlink/?LinkId=286116) o projeto completo no C# ou Visual Basic. O código que pode ser baixado funciona com o Visual Studio 2012 e posterior. Ele usa o modelo do Visual Studio 2012, que é ligeiramente diferente do modelo do Visual Studio 2017 mostrado neste tutorial.
> 
> No tutorial, você executa o aplicativo no Visual Studio. Você também pode implantar o aplicativo em um provedor de hospedagem e disponibilizá-lo pela Internet. A Microsoft oferece hospedagem na Web gratuita para até 10 sites em um  
> [conta de avaliação gratuita do Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604). Para obter informações sobre como implantar um projeto Web do Visual Studio em aplicativos Web do Azure App Service, consulte a [implantação da Web do ASP.NET usando o Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) Series. Esse tutorial também mostra como usar Migrações do Entity Framework Code First para implantar o banco de dados do SQL Server no banco de dados SQL do Azure.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> - Microsoft Visual Studio 2017 ou Microsoft Visual Studio Community 2017
>   
> Este tutorial também funciona com o Visual Studio 2012 e Visual Studio 2013, mas há algumas diferenças na interface do usuário e no modelo de projeto.

## <a name="what-youll-build"></a>O que você criará

Neste tutorial, você vai:

* Crie objetos de dados que reflitam uma universidade com alunos registrados em cursos
* Criar tabelas de banco de dados a partir dos objetos
* Popule o banco de dados com dado de teste
* Exibir dados em um formulário da Web

## <a name="create-the-project"></a>Criar o projeto

1. No Visual Studio 2017, crie um projeto de **aplicativo Web ASP.net (.NET Framework)** chamado **ContosoUniversityModelBinding**.

   ![Criar projeto](retrieving-data/_static/image19.png)

2. Selecione **OK**. A caixa de diálogo para selecionar um modelo é exibida.

   ![selecionar formulários da Web](retrieving-data/_static/image3.png)

3. Selecione o modelo de **Web Forms** . 

4. Se necessário, altere a autenticação para **contas de usuário individuais**. 

5. Selecione **OK** para criar o projeto.

## <a name="modify-site-appearance"></a>Modificar a aparência do site

   Faça algumas alterações para personalizar a aparência do site. 
   
   1. Abra o arquivo site. master.
   
   2. Altere o título para exibir a **Contoso University** e não **meu aplicativo ASP.net**.

      [!code-aspx-csharp[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

   3. Altere o texto do cabeçalho do **nome do aplicativo** para **Contoso University**.

      [!code-aspx-csharp[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

   4. Altere os links de cabeçalho de navegação para os sites apropriados. 
   
      Remova os links para **sobre** e **contate** e, em vez disso, vincule a uma página de **alunos** que você criará.

      [!code-aspx-csharp[Main](retrieving-data/samples/sample3.aspx)]

   5. Salve site. master.

## <a name="add-a-web-form-to-display-student-data"></a>Adicionar um formulário da Web para exibir dados do aluno

   1. Em **Gerenciador de soluções**, clique com o botão direito do mouse em seu projeto, selecione **Adicionar** e **novo item**. 
   
   2. Na caixa de diálogo **Adicionar novo item** , selecione o modelo **formulário da Web com página mestra** e nomeie-o como **Student. aspx**.

      ![criar página](retrieving-data/_static/image5.png)

   3. Selecione **Adicionar**.
   
   4. Para a página mestra do formulário da Web, selecione **site. Master**.
   
   5. Selecione **OK**.

## <a name="add-the-data-model"></a>Adicionar o modelo de dados

Na pasta **modelos** , adicione uma classe chamada **UniversityModels.cs**.

   1. Clique com o botão direito do mouse em **modelos**, selecione **Adicionar**e **novo item**. A caixa de diálogo **Adicionar Novo Item** é exibida.

   2. No menu de navegação à esquerda, selecione **código**e **classe**.

      ![criar classe de modelo](retrieving-data/_static/image20.png)

   3. Nomeie a classe **UniversityModels.cs** e selecione **Adicionar**.

      Nesse arquivo, defina as classes `SchoolContext`, `Student`, `Enrollment`e `Course` da seguinte maneira:

      [!code-csharp[Main](retrieving-data/samples/sample4.cs)]

      A classe de `SchoolContext` deriva de `DbContext`, que gerencia a conexão de banco de dados e as alterações nos mesmos.

      Na classe `Student`, observe os atributos aplicados às propriedades `FirstName`, `LastName`e `Year`. Este tutorial usa esses atributos para validação de dados. Para simplificar o código, somente essas propriedades são marcadas com atributos de validação de dados. Em um projeto real, você aplicaria atributos de validação a todas as propriedades que precisam de validação.

   4. Salve UniversityModels.cs.

## <a name="set-up-the-database-based-on-classes"></a>Configurar o banco de dados com base em classes

Este tutorial usa [migrações do Code First](https://docs.microsoft.com/ef/ef6/modeling/code-first/migrations/) para criar objetos e tabelas de banco de dados. Essas tabelas armazenam informações sobre os alunos e seus cursos.

   1. Selecione **ferramentas** > **Gerenciador de pacotes NuGet** > **console do Gerenciador de pacotes**.

   2. No **console do Gerenciador de pacotes**, execute este comando:  
      `enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

      Se o comando for concluído com êxito, será exibida uma mensagem informando que as migrações foram habilitadas.

      ![Habilitar migrações](retrieving-data/_static/image8.png)

      Observe que um arquivo chamado *Configuration.cs* foi criado. A classe `Configuration` tem um método `Seed`, que pode preencher previamente as tabelas de banco de dados com dado de teste.

## <a name="pre-populate-the-database"></a>Preencher previamente o banco de dados

   1. Abra Configuration.cs.
   
   2. Adicione o seguinte código ao método de `Seed` . Além disso, adicione uma instrução `using` para o namespace `ContosoUniversityModelBinding. Models`.

      [!code-csharp[Main](retrieving-data/samples/sample5.cs)]

   3. Salve Configuration.cs.

   4. No console do Gerenciador de pacotes, execute o comando **Adicionar-migração inicial**.

   5. Execute o comando **Update-Database**.

      Se você receber uma exceção ao executar esse comando, os valores `StudentID` e `CourseID` poderão ser diferentes dos valores do método `Seed`. Abra essas tabelas de banco de dados e encontre os valores existentes para `StudentID` e `CourseID`. Adicione esses valores ao código para propagar a tabela `Enrollments`.

## <a name="add-a-gridview-control"></a>Adicionar um controle GridView

Com os dados populados do banco, agora você está pronto para recuperar esses dados e exibi-los. 

1. Abra o students. aspx.

2. Localize o espaço reservado `MainContent`. Dentro desse espaço reservado, adicione um controle **GridView** que inclua esse código.

   [!code-aspx-csharp[Main](retrieving-data/samples/sample6.aspx)]

   Itens a serem observados:
   * Observe o valor definido para a propriedade `SelectMethod` no elemento GridView. Esse valor especifica o método usado para recuperar dados GridView, que você cria na próxima etapa. 
   
   * A propriedade `ItemType` é definida como a classe `Student` criada anteriormente. Essa configuração permite que você referencie Propriedades de classe na marcação. Por exemplo, a classe `Student` tem uma coleção chamada `Enrollments`. Você pode usar `Item.Enrollments` para recuperar essa coleção e, em seguida, usar a [sintaxe LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) para recuperar a soma dos créditos registrados de cada aluno.
   
3. Salve students. aspx.

## <a name="add-code-to-retrieve-data"></a>Adicionar código para recuperar dados

   No arquivo code-behind dos alunos. aspx, adicione o método especificado para o valor `SelectMethod`. 
   
   1. Abra Students.aspx.cs.
   
   2. Adicione `using` instruções para os namespaces de `ContosoUniversityModelBinding. Models` e `System.Data.Entity`.

      [!code-csharp[Main](retrieving-data/samples/sample7.cs)]

   3. Adicione o método especificado para `SelectMethod`:

      [!code-csharp[Main](retrieving-data/samples/sample8.cs)]

      A cláusula `Include` melhora o desempenho da consulta, mas não é necessária. Sem a cláusula `Include`, os dados são recuperados usando o [*carregamento lento*](https://en.wikipedia.org/wiki/Lazy_loading), o que envolve o envio de uma consulta separada para o banco de dados cada vez que forem recuperados. Com a cláusula `Include`, os dados são recuperados usando o *carregamento adiantado*, o que significa que uma única consulta de banco de dados recupera todos os dados relacionados. Se os dados relacionados não forem usados, o carregamento adiantado é menos eficiente, pois mais dados são recuperados. No entanto, nesse caso, o carregamento adiantado oferece o melhor desempenho porque os dados relacionados são exibidos para cada registro.

      Para obter mais informações sobre considerações de desempenho ao carregar dados relacionados, consulte a seção **carregamento lento, adiantado e explícito de dados relacionados** no artigo [lendo dados relacionados com o Entity Framework em um aplicativo MVC ASP.net](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) .

      Por padrão, os dados são classificados pelos valores da propriedade marcada como a chave. Você pode adicionar uma cláusula `OrderBy` para especificar um valor de classificação diferente. Neste exemplo, a propriedade de `StudentID` padrão é usada para classificação. No artigo [classificando, paginando e Filtrando dados](sorting-paging-and-filtering-data.md) , o usuário está habilitado para selecionar uma coluna para classificação.
 
   4. Salve Students.aspx.cs.

## <a name="run-your-application"></a>Executar seu aplicativo 

Execute seu aplicativo Web (**F5**) e navegue até a página **alunos** , que exibe o seguinte:

   ![Mostrar dados](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a>Geração automática de métodos de associação de modelo

Ao trabalhar nesta série de tutoriais, você pode simplesmente copiar o código do tutorial para o seu projeto. No entanto, uma desvantagem dessa abordagem é que você pode não estar ciente do recurso fornecido pelo Visual Studio para gerar automaticamente o código para métodos de associação de modelo. Ao trabalhar em seus próprios projetos, a geração automática de código pode poupar tempo e ajudá-lo a ter uma noção de como implementar uma operação. Esta seção descreve o recurso de geração de código automático. Esta seção é informativa apenas e não contém nenhum código que você precise implementar em seu projeto. 

Ao definir um valor para as propriedades `SelectMethod`, `UpdateMethod`, `InsertMethod`ou `DeleteMethod` no código de marcação, você pode selecionar a opção **criar novo método** .

![criar um método](retrieving-data/_static/image18.png)

O Visual Studio não apenas cria um método no code-behind com a assinatura apropriada, mas também gera código de implementação para executar a operação. Se você definir primeiro a propriedade `ItemType` antes de usar o recurso de geração de código automático, o código gerado usará esse tipo para as operações. Por exemplo, ao definir a propriedade `UpdateMethod`, o código a seguir é gerado automaticamente:

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

Novamente, esse código não precisa ser adicionado ao seu projeto. No próximo tutorial, você implementará métodos para atualizar, excluir e adicionar novos dados.

## <a name="summary"></a>Resumo

Neste tutorial, você criou classes de modelo de dados e gerou um banco de dado dessas classes. Você preencheu as tabelas de banco de dados com test data. Você usou a associação de modelo para recuperar dados do banco e, em seguida, exibiu os dados em um GridView.

No próximo [tutorial](updating-deleting-and-creating-data.md) desta série, você habilitará a atualização, a exclusão e a criação de dados.

> [!div class="step-by-step"]
> [Avançar](updating-deleting-and-creating-data.md)
