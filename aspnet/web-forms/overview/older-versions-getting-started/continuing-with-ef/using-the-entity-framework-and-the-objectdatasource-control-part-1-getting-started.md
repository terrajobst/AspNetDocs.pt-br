---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
title: 'Usando o Entity Framework 4,0 e o controle ObjectDataSource, parte 1: Introdução | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais se baseia no aplicativo Web da Contoso University criado pelo Introdução com a série de tutoriais Entity Framework. Se yo...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 244278c1-fec8-4255-8a8a-13bde491c4f5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
msc.type: authoredcontent
ms.openlocfilehash: 2f14707eb058d438495dd2bc4c17b976c471fc97
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78547288"
---
# <a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-1-getting-started"></a>Usando o Entity Framework 4,0 e o controle ObjectDataSource, parte 1: Introdução

por [Tom Dykstra](https://github.com/tdykstra)

> Esta série de tutoriais se baseia no aplicativo Web da Contoso University criado pelo [introdução com a série de tutoriais do Entity Framework 4,0](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) . Se você não concluiu os tutoriais anteriores, como ponto de partida para este tutorial, você pode [baixar o aplicativo](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que você criou. Você também pode [baixar o aplicativo](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) que é criado pela série de tutoriais completa.
> 
> O aplicativo Web de exemplo Contoso University demonstra como criar ASP.NET Web Forms aplicativos usando o Entity Framework 4,0 e o Visual Studio 2010. O aplicativo de exemplo é um site para uma Universidade fictícia da contoso. Ele inclui funcionalidades como admissão de alunos, criação de cursos e atribuições de instrutor.
> 
> O tutorial mostra exemplos em C#. O [exemplo para download](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) contém código em ambos C# e Visual Basic.
> 
> ## <a name="database-first"></a>Database First
> 
> Há três maneiras de trabalhar com dados no Entity Framework: *Database First*, *Model First*e *Code First*. Este tutorial é para Database First. Para obter informações sobre as diferenças entre esses fluxos de trabalho e diretrizes sobre como escolher o melhor para seu cenário, consulte [Entity Framework fluxos de trabalho de desenvolvimento](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>Formulários da Web
> 
> Como a série Introdução, esta série de tutoriais usa o modelo de Web Forms ASP.NET e pressupõe que você saiba como trabalhar com ASP.NET Web Forms no Visual Studio. Se não tiver, consulte [introdução com ASP.NET 4,5 Web Forms](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Se você preferir trabalhar com a estrutura MVC do ASP.NET, consulte [introdução com o Entity Framework usando o ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="software-versions"></a>Versões de software
> 
> | **Mostrado no tutorial** | **Também funciona com** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Visual Studio 2010 Express para Web. O tutorial não foi testado com versões posteriores do Visual Studio. Há muitas diferenças em seleções de menu, caixas de diálogo e modelos. |
> | .NET 4 | O .NET 4,5 é compatível com o .NET 4, mas o tutorial não foi testado com o .NET 4,5. |
> | Entity Framework 4 | O tutorial não foi testado com versões posteriores do Entity Framework. A partir do Entity Framework 5, o EF usa por padrão o `DbContext API` que foi introduzido com o EF 4,1. O controle de EntityDataSource foi projetado para usar a API `ObjectContext`. Para obter informações sobre como usar o controle EntityDataSource com a API `DbContext`, consulte [esta postagem de blog](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx). |
> 
> ## <a name="questions"></a>Perguntas
> 
> Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, poderá lançá-las no fórum de [Entity Framework ASP.net](https://forums.asp.net/1227.aspx), no [Entity Framework e no LINQ to Entities forum](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/)ou [stackoverflow.com](http://stackoverflow.com/).

O controle de `EntityDataSource` permite que você crie um aplicativo muito rapidamente, mas normalmente exige que você mantenha uma quantidade significativa de lógica de negócios e lógica de acesso a dados em suas páginas *. aspx* . Se você espera que seu aplicativo cresça em complexidade e exija manutenção contínua, você pode investir mais tempo de desenvolvimento antecipadamente para criar uma estrutura de aplicativo de *n camadas ou em* *camadas* que seja mais passível de manutenção. Para implementar essa arquitetura, você separa a camada de apresentação da camada de lógica de negócios (BLL) e a camada de acesso a dados (DAL). Uma maneira de implementar essa estrutura é usar o controle de `ObjectDataSource` em vez do controle `EntityDataSource`. Quando você usa o controle de `ObjectDataSource`, você implementa seu próprio código de acesso a dados e, em seguida, o invoca em páginas *. aspx* usando um controle que tem muitos dos mesmos recursos que outros controles de fonte de dados. Isso permite que você combine as vantagens de uma abordagem de n camadas com os benefícios de usar um controle de Web Forms para acesso a dados.

O controle de `ObjectDataSource` proporciona mais flexibilidade de outras maneiras também. Como você escreve seu próprio código de acesso a dados, é mais fácil fazer mais do que apenas ler, inserir, atualizar ou excluir um tipo de entidade específico, que são as tarefas que o controle de `EntityDataSource` foi projetado para executar. Por exemplo, você pode executar o registro em log toda vez que uma entidade é atualizada, arquivar dados sempre que uma entidade é excluída ou verificar automaticamente e atualizar os dados relacionados conforme necessário ao inserir uma linha com um valor de chave estrangeira.

## <a name="business-logic-and-repository-classes"></a>Lógica de negócios e classes de repositório

Um controle de `ObjectDataSource` funciona invocando uma classe que você cria. A classe inclui métodos que recuperam e atualizam dados, e você fornece os nomes desses métodos para o controle de `ObjectDataSource` na marcação. Durante a renderização ou o processamento de postback, o `ObjectDataSource` chama os métodos que você especificou.

Além das operações CRUD básicas, a classe que você cria para usar com o controle de `ObjectDataSource` pode precisar executar a lógica de negócios quando o `ObjectDataSource` lê ou atualiza dados. Por exemplo, ao atualizar um departamento, talvez seja necessário validar que nenhum outro departamento tenha o mesmo administrador, pois uma pessoa não pode ser administrador de mais de um departamento.

Em algumas `ObjectDataSource` documentação, como a [visão geral da classe ObjectDataSource](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.aspx), o controle chama uma classe referida como um *objeto comercial* que inclui a lógica de negócios e a lógica de acesso a dados. Neste tutorial, você criará classes separadas para a lógica de negócios e para a lógica de acesso a dados. A classe que encapsula a lógica de acesso a dados é chamada de *repositório*. A classe de lógica de negócios inclui métodos de lógica de negócios e métodos de acesso a dados, mas os métodos de acesso a dados chamam o repositório para executar tarefas de acesso a dados.

Você também criará uma camada de abstração entre sua BLL e a DAL, que facilita o teste de unidade automatizado da BLL. Essa camada de abstração é implementada pela criação de uma interface e usando a interface quando você instancia o repositório na classe de lógica de negócios. Isso possibilita que você forneça a classe de lógica de negócios com uma referência a qualquer objeto que implemente a interface do repositório. Para a operação normal, você fornece um objeto de repositório que funciona com o Entity Framework. Para teste, você fornece um objeto de repositório que funciona com os dados armazenados de uma maneira que você possa manipular facilmente, como as variáveis de classe definidas como coleções.

A ilustração a seguir mostra a diferença entre uma classe lógica de negócios que inclui a lógica de acesso a dados sem um repositório e outra que usa um repositório.

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image1.png)

Você começará criando páginas da Web nas quais o controle de `ObjectDataSource` está associado diretamente a um repositório porque ele só executa tarefas básicas de acesso a dados. No próximo tutorial, você criará uma classe de lógica de negócios com a lógica de validação e associará o controle de `ObjectDataSource` a essa classe em vez de à classe de repositório. Você também criará testes de unidade para a lógica de validação. No terceiro tutorial desta série, você adicionará a funcionalidade de classificação e filtragem ao aplicativo.

As páginas criadas neste tutorial funcionam com o conjunto de entidades de `Departments` do modelo de dados que você criou na [série de tutoriais de introdução](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md).

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image3.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image5.png)

## <a name="updating-the-database-and-the-data-model"></a>Atualizando o banco de dados e o modelo de dado

Você começará este tutorial fazendo duas alterações no banco de dados, as quais exigirão alterações correspondentes no modelo que você criou na [introdução com os tutoriais Entity Framework e Web Forms](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) . Em um desses tutoriais, você fez alterações no Designer manualmente para sincronizar o modelo de dados com o banco de dado após uma alteração no banco de dados. Neste tutorial, você usará o modelo de atualização do designer **da ferramenta de banco** de dados para atualizar o modelo de dado automaticamente.

### <a name="adding-a-relationship-to-the-database"></a>Adicionando uma relação ao banco de dados

No Visual Studio, abra o aplicativo Web da Contoso University criado na [introdução com a Entity Framework e Web Forms](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) série de tutoriais e, em seguida, abra o diagrama do banco de dados `SchoolDiagram`.

Se você olhar a tabela `Department` no diagrama de banco de dados, verá que ela tem uma coluna `Administrator`. Essa coluna é uma chave estrangeira para a tabela de `Person`, mas nenhuma relação de chave estrangeira é definida no banco de dados. Você precisa criar a relação e atualizar o modelo de dados para que o Entity Framework possa lidar automaticamente com essa relação.

No diagrama de banco de dados, clique com o botão direito do mouse na tabela `Department` e selecione **relações**.

[![Image80](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image7.png)

Na caixa **relações de chave estrangeira** , clique em **Adicionar**e, em seguida, clique na especificação de reticências de **tabelas e colunas**.

[![Image81](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image9.png)

Na caixa de diálogo **tabelas e colunas** , defina a tabela de chave primária e o campo como `Person` e `PersonID`e defina a tabela de chave estrangeira e o campo como `Department` e `Administrator`. (Quando você fizer isso, o nome da relação será alterado de `FK_Department_Department` para `FK_Department_Person`.)

[![Image82](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image11.png)

Clique em **OK** na caixa **tabelas e colunas** , clique em **fechar** na caixa relações de **chave estrangeira** e salve as alterações. Se você for perguntado se deseja salvar as tabelas `Person` e `Department`, clique em **Sim**.

> [!NOTE]
> Se você tiver excluído `Person` linhas que correspondem aos dados que já estão na coluna `Administrator`, não será possível salvar essa alteração. Nesse caso, use o editor de tabela no **Gerenciador de servidores** para certificar-se de que o valor `Administrator` em cada `Department` linha contém a ID de um registro que realmente existe na tabela `Person`.
> 
> Depois de salvar a alteração, você não poderá excluir uma linha da tabela `Person` se essa pessoa for um administrador do departamento. Em um aplicativo de produção, você forneceria uma mensagem de erro específica quando uma restrição de banco de dados impedir uma exclusão ou você especificar uma exclusão em cascata. Para obter um exemplo de como especificar uma exclusão em cascata, consulte [a Entity Framework e a ASP.net – introdução parte 2](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2.md).

### <a name="adding-a-view-to-the-database"></a>Adicionando uma exibição ao banco de dados

Na nova página *departamentos. aspx* que você criará, você deseja fornecer uma lista suspensa de instrutores, com nomes no formato "último, primeiro" para que os usuários possam selecionar administradores de departamento. Para facilitar isso, você criará uma exibição no banco de dados. O modo de exibição consistirá apenas nos dados necessários para a lista suspensa: o nome completo (corretamente formatado) e a chave de registro.

Em **Gerenciador de servidores**, expanda *School. MDF*, clique com o botão direito do mouse na pasta **exibições** e selecione **Adicionar nova exibição**.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image13.png)

Clique em **fechar** quando a caixa de diálogo **Adicionar tabela** for exibida e cole a seguinte instrução SQL no painel SQL:

[!code-sql[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample1.sql)]

Salve o modo de exibição como `vInstructorName`.

### <a name="updating-the-data-model"></a>Atualizando o modelo de dados

Na pasta *Dal* , abra o arquivo *SchoolModel. edmx* , clique com o botão direito do mouse na superfície de design e selecione **atualizar modelo do banco de dados**.

[![Image07](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image15.png)

Na caixa de diálogo **escolher seu objeto de banco de dados** , selecione a guia **Adicionar** e selecione o modo de exibição que você acabou de criar.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image17.png)

Clique em **Concluir**.

No designer, você vê que a ferramenta criou uma entidade `vInstructorName` e uma nova associação entre as entidades `Department` e `Person`.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image19.png)

> [!NOTE]
> Na **saída** e **lista de erros** janelas, você poderá ver uma mensagem de aviso informando que a ferramenta criou automaticamente uma chave primária para a nova exibição de `vInstructorName`. Este comportamento é esperado.

Quando você faz referência à nova entidade `vInstructorName` no código, não deseja usar a Convenção de banco de dados de prefixar um "v" minúsculo para ele. Portanto, você renomeará a entidade e o conjunto de entidades no modelo.

Abra o **navegador de modelos**. Você vê `vInstructorName` listadas como um tipo de entidade e uma exibição.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image21.png)

Em **SchoolModel** (não **SchoolModel. Store**), clique com o botão direito do mouse em **vInstructorName** e selecione **Propriedades**. Na janela **Propriedades** , altere a propriedade **nome** para "instrutor" e altere a propriedade **nome do conjunto de entidades** para "instrutornames".

[![Image15](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image24.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image23.png)

Salve e feche o modelo de dados e recompile o projeto.

## <a name="using-a-repository-class-and-an-objectdatasource-control"></a>Usando uma classe de repositório e um controle ObjectDataSource

Crie um novo arquivo de classe na pasta *Dal* , nomeie-o *SchoolRepository.cs*e substitua o código existente pelo código a seguir:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample2.cs)]

Esse código fornece um único método `GetDepartments` que retorna todas as entidades no conjunto de entidades `Departments`. Como você sabe que estará acessando a propriedade de navegação `Person` para cada linha retornada, especifique o carregamento adiantado para essa propriedade usando o método `Include`. A classe também implementa a interface `IDisposable` para garantir que a conexão do banco de dados seja liberada quando o objeto for descartado.

> [!NOTE]
> Uma prática comum é criar uma classe de repositório para cada tipo de entidade. Neste tutorial, é usada uma classe de repositório para vários tipos de entidade. Para obter mais informações sobre o padrão de repositório, consulte as postagens no [blog da equipe de Entity Framework](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) e no [blog de Julie Lerman](http://thedatafarm.com/blog/data-access/agile-ef4-repository-part-3-fine-tuning-the-repository/).

O método `GetDepartments` retorna um objeto `IEnumerable` em vez de um objeto `IQueryable` para garantir que a coleção retornada seja utilizável mesmo depois que o próprio objeto de repositório for descartado. Um objeto de `IQueryable` pode causar acesso ao banco de dados sempre que ele é acessado, mas o objeto de repositório pode ser Descartado pelo tempo que um controle de ligação de dados tenta renderizar o dado. Você pode retornar outro tipo de coleção, como um objeto `IList` em vez de um objeto `IEnumerable`. No entanto, retornar um objeto de `IEnumerable` garante que você possa executar tarefas típicas de processamento de lista somente leitura, como loops de `foreach` e consultas LINQ, mas não pode adicionar ou remover itens da coleção, o que pode implicar que essas alterações seriam persistidas no banco de dados.

Crie uma página Departments *. aspx* que usa a página mestra *site. Master* e adicione a seguinte marcação no controle de `Content` chamado `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample3.aspx)]

Essa marcação cria um controle de `ObjectDataSource` que usa a classe de repositório que você acabou de criar e um controle de `GridView` para exibir os dados. O controle de `GridView` Especifica comandos de **Editar** e **excluir** , mas você ainda não adicionou código para dar suporte a eles.

Várias colunas usam controles `DynamicField` para que você possa aproveitar a funcionalidade de validação e a formatação automática de dados. Para que isso funcione, você precisará chamar o método `EnableDynamicData` no manipulador de eventos `Page_Init`. (os controles de`DynamicControl` não são usados no campo `Administrator` porque não funcionam com propriedades de navegação.)

Os atributos de `Vertical-Align="Top"` se tornarão importantes mais tarde, quando você adicionar uma coluna que tenha um controle de `GridView` aninhado à grade.

Abra o arquivo *Departments.aspx.cs* e adicione a seguinte instrução de `using`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample4.cs)]

Em seguida, adicione o seguinte manipulador ao evento de `Init` da página:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample5.cs)]

Na pasta *Dal* , crie um novo arquivo de classe chamado *Department.cs* e substitua o código existente pelo código a seguir:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample6.cs)]

Esse código adiciona metadados ao modelo de dados. Ele especifica que a propriedade `Budget` da entidade `Department` realmente representa a moeda, embora seu tipo de dados seja `Decimal`e especifica que o valor deve estar entre 0 e $1000000. Também especifica que a propriedade `StartDate` deve ser formatada como uma data no formato mm/dd/aaaa.

Execute a página *departamentos. aspx* .

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image26.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image25.png)

Observe que, embora você não tenha especificado uma cadeia de caracteres de formato na marcação de página Departments *. aspx* para as colunas de **data de início** ou de **orçamento** , a formatação de moeda e data padrão foi aplicada a elas pelos controles de `DynamicField`, usando os metadados fornecidos no arquivo *Department.cs* .

## <a name="adding-insert-and-delete-functionality"></a>Adicionando funcionalidade de inserção e exclusão

Abra *SchoolRepository.cs*, adicione o código a seguir para criar um método `Insert` e um método `Delete`. O código também inclui um método chamado `GenerateDepartmentID` que calcula o próximo valor de chave de registro disponível para uso pelo método `Insert`. Isso é necessário porque o banco de dados não está configurado para calcular isso automaticamente para a tabela de `Department`.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample7.cs)]

### <a name="the-attach-method"></a>O método Attach

O método `DeleteDepartment` chama o método `Attach` para restabelecer o vínculo que é mantido no Gerenciador de estado de objeto do contexto de objeto entre a entidade na memória e a linha de banco de dados que ele representa. Isso deve ocorrer antes que o método chame o método `SaveChanges`.

O termo *contexto de objeto* refere-se à classe Entity Framework que deriva da classe `ObjectContext` que você usa para acessar seus conjuntos de entidade e entidades. No código para este projeto, a classe é chamada `SchoolEntities`e uma instância dele sempre é nomeada `context`. O *Gerenciador de estado de objeto* do contexto de objeto é uma classe derivada da classe `ObjectStateManager`. O contato do objeto usa o Gerenciador de estado de objeto para armazenar objetos de entidade e controlar se cada um está em sincronia com sua linha de tabela ou linhas correspondentes no banco de dados.

Quando você lê uma entidade, o contexto do objeto a armazena no Gerenciador de estado de objeto e controla se essa representação do objeto está em sincronia com o banco de dados. Por exemplo, se você alterar um valor de propriedade, um sinalizador será definido para indicar que a propriedade que você alterou não está mais em sincronia com o banco de dados. Em seguida, quando você chama o método `SaveChanges`, o contexto do objeto sabe o que fazer no banco de dados, pois o Gerenciador de estado do objeto sabe exatamente o que é diferente entre o estado atual da entidade e o estado do banco de dados.

No entanto, esse processo normalmente não funciona em um aplicativo Web, porque a instância de contexto de objeto que lê uma entidade, junto com tudo em seu Gerenciador de estado de objeto, é descartada depois que uma página é renderizada. A instância de contexto de objeto que deve aplicar alterações é uma nova que é instanciada para o processamento de postback. No caso do método `DeleteDepartment`, o controle `ObjectDataSource` recria a versão original da entidade para você a partir de valores no estado de exibição, mas essa entidade `Department` recriada não existe no Gerenciador de estado de objeto. Se você chamou o método `DeleteObject` nessa entidade recriada, a chamada falhará porque o contexto do objeto não sabe se a entidade está em sincronia com o banco de dados. No entanto, chamar o método `Attach` restabelece o mesmo acompanhamento entre a entidade recriada e os valores no banco de dados que foi originalmente feito automaticamente quando a entidade foi lida em uma instância anterior do contexto do objeto.

Há ocasiões em que você não quer que o contexto de objeto rastreie entidades no Gerenciador de estado de objeto e pode definir sinalizadores para impedi-lo de fazer isso. Exemplos disso são mostrados nos tutoriais posteriores desta série.

### <a name="the-savechanges-method"></a>O método SaveChanges

Essa simples classe de repositório ilustra os princípios básicos de como executar operações CRUD. Neste exemplo, o método `SaveChanges` é chamado imediatamente após cada atualização. Em um aplicativo de produção, você pode querer chamar o método `SaveChanges` de um método separado para oferecer mais controle sobre quando o banco de dados é atualizado. (No final do próximo tutorial, você encontrará um link para um white paper que aborda o padrão de unidade de trabalho, que é uma abordagem para coordenar atualizações relacionadas.) Observe também que, no exemplo, o método `DeleteDepartment` não inclui código para lidar com conflitos de simultaneidade; o código para fazer isso será adicionado em um tutorial posterior nesta série.

### <a name="retrieving-instructor-names-to-select-when-inserting"></a>Recuperando nomes de instrutor para selecionar ao inserir

Os usuários devem ser capazes de selecionar um administrador em uma lista suspensa, ao criar novos departamentos. Portanto, adicione o seguinte código a *SchoolRepository.cs* para criar um método para recuperar a lista de instrutores usando a exibição que você criou anteriormente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample8.cs)]

### <a name="creating-a-page-for-inserting-departments"></a>Criando uma página para inserir departamentos

Crie uma página *DepartmentsAdd. aspx* que usa a página *site. Master* e adicione a seguinte marcação no controle de `Content` chamado `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample9.aspx)]

Essa marcação cria dois controles de `ObjectDataSource`, um para inserir novas entidades de `Department` e outra para recuperar nomes de instrutor para o controle de `DropDownList` usado para selecionar administradores de departamento. A marcação cria um controle de `DetailsView` para inserir novos departamentos e especifica um manipulador para o evento de `ItemInserting` do controle para que você possa definir o valor da chave estrangeira `Administrator`. No final é um controle de `ValidationSummary` para exibir mensagens de erro.

Abra *DepartmentsAdd.aspx.cs* e adicione a seguinte instrução de `using`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample10.cs)]

Adicione a seguinte variável de classe e métodos:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample11.cs)]

O método `Page_Init` permite Dados Dinâmicos funcionalidade. O manipulador para o evento de `Init` do controle de `DropDownList` salva uma referência ao controle e o manipulador para o evento de `Inserting` do controle de `DetailsView` usa essa referência para obter o valor de `PersonID` do instrutor selecionado e atualizar a propriedade de chave estrangeira `Administrator` da entidade `Department`.

Execute a página, adicione informações para um novo departamento e, em seguida, clique no link **Inserir** .

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image28.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image27.png)

Insira valores para outro novo departamento. Insira um número maior que 1000000, 0 no campo de **orçamento** e Tab para o próximo campo. Um asterisco aparece no campo e, se você mantiver o ponteiro do mouse sobre ele, poderá ver a mensagem de erro que você inseriu nos metadados para esse campo.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image30.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image29.png)

Clique em **Inserir**e você verá a mensagem de erro exibida pelo controle de `ValidationSummary` na parte inferior da página.

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image32.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image31.png)

Em seguida, feche o navegador e abra a página *departamentos. aspx* . Adicione a funcionalidade de exclusão à página *departamentos. aspx* adicionando um atributo `DeleteMethod` ao controle `ObjectDataSource` e um atributo `DataKeyNames` ao controle `GridView`. As marcas de abertura para esses controles agora serão semelhantes ao exemplo a seguir:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample12.aspx)]

Execute a página.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image34.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image33.png)

Exclua o departamento que você adicionou quando executou a página *DepartmentsAdd. aspx* .

## <a name="adding-update-functionality"></a>Adicionando a funcionalidade de atualização

Abra *SchoolRepository.cs* e adicione o seguinte método de `Update`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample13.cs)]

Quando você clica em **Atualizar** na página *departamentos. aspx* , o controle de `ObjectDataSource` cria duas entidades `Department` para passar para o método `UpdateDepartment`. Uma contém os valores originais que foram armazenados no estado de exibição e o outro contém os novos valores que foram inseridos no controle de `GridView`. O código no método `UpdateDepartment` passa a entidade `Department` que tem os valores originais para o método `Attach` a fim de estabelecer o acompanhamento entre a entidade e o que está no banco de dados. Em seguida, o código passa a entidade `Department` que tem os novos valores para o método `ApplyCurrentValues`. O contexto do objeto compara os valores novos e antigos. Se um novo valor for diferente de um valor antigo, o contexto do objeto alterará o valor da propriedade. Em seguida, o método `SaveChanges` atualiza apenas as colunas alteradas no banco de dados. (No entanto, se a função Update para essa entidade foi mapeada para um procedimento armazenado, toda a linha seria atualizada, independentemente de quais colunas foram alteradas.)

Abra o arquivo Departments *. aspx* e adicione os seguintes atributos ao controle de `DepartmentsObjectDataSource`:

- `UpdateMethod="UpdateDepartment"`
- `ConflictDetection="CompareAllValues"`   
 Isso faz com que valores antigos sejam armazenados no estado de exibição para que eles possam ser comparados com os novos valores no método `Update`.
- `OldValuesParameterFormatString="orig{0}"`   
 Isso informa ao controle que o nome do parâmetro de valores originais é `origDepartment`.

A marcação para a marca de abertura do controle de `ObjectDataSource` agora é semelhante ao exemplo a seguir:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample14.aspx)]

Adicione um atributo `OnRowUpdating="DepartmentsGridView_RowUpdating"` ao controle de `GridView`. Você usará isso para definir o valor da propriedade `Administrator` com base na linha que o usuário seleciona em uma lista suspensa. O `GridView` marca de abertura agora é semelhante ao exemplo a seguir:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample15.aspx)]

Adicione um controle de `EditItemTemplate` para a coluna `Administrator` ao controle de `GridView`, imediatamente após o controle de `ItemTemplate` para essa coluna:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample16.aspx)]

Esse `EditItemTemplate` controle é semelhante ao controle de `InsertItemTemplate` na página *DepartmentsAdd. aspx* . A diferença é que o valor inicial do controle é definido usando o atributo `SelectedValue`.

Antes do controle de `GridView`, adicione um controle de `ValidationSummary` como você fez na página *DepartmentsAdd. aspx* .

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample17.aspx)]

Abra *Departments.aspx.cs* e imediatamente após a declaração de classe parcial, adicione o seguinte código para criar um campo particular para referenciar o controle de `DropDownList`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample18.cs)]

Em seguida, adicione manipuladores para o evento `Init` do controle de `DropDownList` e o evento `RowUpdating` do controle de `GridView`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample19.cs)]

O manipulador para o evento de `Init` salva uma referência ao controle de `DropDownList` no campo de classe. O manipulador para o evento `RowUpdating` usa a referência para obter o valor inserido pelo usuário e aplicá-lo à propriedade `Administrator` da entidade `Department`.

Use a página *DepartmentsAdd. aspx* para adicionar um novo departamento e, em seguida, execute a página Departments *. aspx* e clique em **Editar** na linha que você adicionou.

> [!NOTE]
> Você não poderá editar linhas que você não adicionou (isto é, que já estavam no banco de dados), devido a um dado inválido no banco de dados; os administradores para as linhas que foram criadas com o banco de dados são alunos. Se você tentar editar um deles, receberá uma página de erro que relata um erro como `'InstructorsDropDownList' has a SelectedValue which is invalid because it does not exist in the list of items.`

[![Image10](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image36.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image35.png)

Se você inserir um valor de **orçamento** inválido e, em seguida, clicar em **Atualizar**, verá o mesmo asterisco e mensagem de erro que você viu na página *departamentos. aspx* .

Altere um valor de campo ou selecione um administrador diferente e clique em **Atualizar**. A alteração é exibida.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image38.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image37.png)

Isso conclui a introdução ao uso do controle de `ObjectDataSource` para operações CRUD (criar, ler, atualizar e excluir) básicas com o Entity Framework. Você criou um aplicativo de n camadas simples, mas a camada de lógica de negócios ainda está rigidamente acoplada à camada de acesso a dados, o que complica o teste de unidade automatizado. No tutorial a seguir, você verá como implementar o padrão de repositório para facilitar o teste de unidade.

> [!div class="step-by-step"]
> [Próximo](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
