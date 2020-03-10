---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
title: Introdução com Entity Framework 4,0 Database First e ASP.NET 4 Web Forms-parte 2 | Microsoft Docs
author: tdykstra
description: O aplicativo Web de exemplo Contoso University demonstra como criar ASP.NET Web Forms aplicativos usando o Entity Framework. O aplicativo de exemplo é...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: fb63a326-a4ae-4b0c-a4f5-412327197216
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
msc.type: authoredcontent
ms.openlocfilehash: bd6a2e29e6f0df04e39be29160e2e08cc99c4706
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78566006"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-2"></a>Introdução com Entity Framework 4,0 Database First e ASP.NET 4 Web Forms-parte 2

por [Tom Dykstra](https://github.com/tdykstra)

> O aplicativo Web de exemplo Contoso University demonstra como criar ASP.NET Web Forms aplicativos usando o Entity Framework 4,0 e o Visual Studio 2010. Para obter informações sobre a série de tutoriais, consulte [o primeiro tutorial da série](the-entity-framework-and-aspnet-getting-started-part-1.md)

## <a name="the-entitydatasource-control"></a>O controle de EntityDataSource

No tutorial anterior, você criou um site da Web, um banco de dados e um modelo. Neste tutorial, você trabalha com o controle de `EntityDataSource` que o ASP.NET fornece para facilitar o trabalho com um modelo de dados de Entity Framework. Você criará um controle de `GridView` para exibir e editar dados de alunos, um controle de `DetailsView` para adicionar novos alunos e um controle de `DropDownList` para selecionar um departamento (que você usará posteriormente para exibir os cursos associados).

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image1.png)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image3.png)

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image5.png)

Observe que, neste aplicativo, você não adicionará validação de entrada às páginas que atualizam o banco de dados, e algumas das manipulações de erro não serão tão robustas quanto seriam necessárias em um aplicativo de produção. Isso mantém o tutorial focalizado na Entity Framework e impede que ele fique muito longo. Para obter detalhes sobre como adicionar esses recursos ao seu aplicativo, consulte [validando a entrada do usuário em páginas da Web do ASP.net](https://msdn.microsoft.com/library/7kh55542.aspx) e [tratamento de erros em páginas e aplicativos do ASP.net](https://msdn.microsoft.com/library/w16865z6.aspx).

## <a name="adding-and-configuring-the-entitydatasource-control"></a>Adicionando e Configurando o controle de EntityDataSource

Você começará Configurando um controle de `EntityDataSource` para ler `Person` entidades do conjunto de entidades `People`.

Verifique se você tem o Visual Studio aberto e se está trabalhando com o projeto que você criou na parte 1. Se você não tiver criado o projeto desde que criou o modelo de dados ou desde a última alteração feita a ele, compile o projeto agora. As alterações no modelo de dados não são disponibilizadas para o designer até que o projeto seja compilado.

Crie uma nova página da Web usando o **formulário da Web usando** o modelo de página mestra e nomeie-a como *Student. aspx*.

[![Image23](the-entity-framework-and-aspnet-getting-started-part-2/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image7.png)

Especifique *site. Master* como a página mestra. Todas as páginas que você criar para esses tutoriais usarão essa página mestra.

[![Image24](the-entity-framework-and-aspnet-getting-started-part-2/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image9.png)

No modo de exibição de **origem** , adicione um cabeçalho de `h2` ao controle de `Content` chamado `Content2`, conforme mostrado no exemplo a seguir:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample1.aspx)]

Na guia **dados** da caixa de **ferramentas**, arraste um controle de `EntityDataSource` para a página, solte-o abaixo do título e altere a ID para `StudentsEntityDataSource`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample2.aspx)]

Alterne para o modo de exibição de **design** , clique na marca inteligente do controle da fonte de dados e clique em **Configurar fonte de dados** para iniciar o assistente para **Configurar fonte de dados** .

[![Image01](the-entity-framework-and-aspnet-getting-started-part-2/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image11.png)

Na etapa **Configurar** o assistente de ObjectContext, selecione **SchoolEntities** como o valor para **conexão nomeada**e selecione **SchoolEntities** como o valor **DefaultContainerName** . Em seguida, clique em **Próximo**.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-2/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image13.png)

Observação: se você receber a seguinte caixa de diálogo neste ponto, precisará compilar o projeto antes de continuar.

[![Image25](the-entity-framework-and-aspnet-getting-started-part-2/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image15.png)

Na etapa **Configurar seleção de dados** , selecione **pessoas** como o valor de **EntitySetName**. Em **selecionar**, verifique se a caixa de seleção **selecionar uma** ll está marcada. Em seguida, selecione as opções para habilitar atualizar e excluir. Quando terminar, clique em **concluir**.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-2/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image17.png)

## <a name="configuring-database-rules-to-allow-deletion"></a>Configurando regras de banco de dados para permitir a exclusão

Você criará uma página que permite aos usuários excluir alunos da tabela `Person`, que tem três relações com outras tabelas (`Course`, `StudentGrade`e `OfficeAssignment`). Por padrão, o banco de dados impedirá que você exclua uma linha em `Person` se houver linhas relacionadas em uma das outras tabelas. Você pode excluir manualmente as linhas relacionadas primeiro ou pode configurar o banco de dados para excluí-las automaticamente quando você exclui uma linha de `Person`. Para os registros de aluno deste tutorial, você configurará o banco de dados para excluir automaticamente os dados relacionados. Como os alunos podem ter linhas relacionadas apenas na tabela `StudentGrade`, você precisa configurar apenas uma das três relações.

Se você estiver usando o arquivo *School. MDF* que você baixou do projeto que acompanha este tutorial, poderá ignorar esta seção porque essas alterações de configuração já foram feitas. Se você criou o banco de dados executando um script, configure o banco de dados executando os procedimentos a seguir.

No **Gerenciador de servidores**, abra o diagrama de banco de dados que você criou na parte 1. Clique com o botão direito do mouse na relação entre `Person` e `StudentGrade` (a linha entre as tabelas) e selecione **Propriedades**.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-2/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image19.png)

Na janela **Propriedades** , expanda **especificação INSERT e Update** e defina a propriedade **DeleteRule** como **Cascade**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-2/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image21.png)

Salve e feche o diagrama. Se você for perguntado se deseja atualizar o banco de dados, clique em **Sim**.

Para certificar-se de que o modelo Mantenha as entidades que estão na memória sincronizadas com o que o banco de dados está fazendo, você deve definir as regras correspondentes no modelo de dado. Abra *SchoolModel. edmx*, clique com o botão direito do mouse na linha de associação entre `Person` e `StudentGrade`e, em seguida, selecione **Propriedades**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-2/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image23.png)

Na janela **Propriedades** , defina **End1 OnDelete** para **Cascade**.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-2/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image25.png)

Salve e feche o arquivo *SchoolModel. edmx* e recompile o projeto.

Em geral, quando o banco de dados é alterado, você tem várias opções de como sincronizar o modelo:

- Para determinados tipos de alterações (como adicionar ou atualizar tabelas, exibições ou procedimentos armazenados), clique com o botão direito do mouse no designer e selecione **atualizar modelo do banco de dados** para fazer com que o designer faça as alterações automaticamente.
- Gere novamente o modelo de dados.
- Faça atualizações manuais como esta.

Nesse caso, você poderia ter regenerado o modelo ou atualizado as tabelas afetadas pela alteração de relação, mas, em seguida, você precisaria fazer com que o nome do campo seja alterado novamente (de `FirstName` para `FirstMidName`).

## <a name="using-a-gridview-control-to-read-and-update-entities"></a>Usando um controle GridView para ler e atualizar entidades

Nesta seção, você usará um controle de `GridView` para exibir, atualizar ou excluir alunos.

Abra ou alterne para *students. aspx* e alterne para o modo de exibição de **design** . Na guia **dados** da caixa de **ferramentas**, arraste um controle `GridView` à direita do controle `EntityDataSource`, nomeie-o `StudentsGridView`, clique na marca inteligente e, em seguida, selecione **StudentsEntityDataSource** como a fonte de dados.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-2/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image27.png)

Clique em **Atualizar esquema** (clique em **Sim** se for solicitado a confirmar), clique em **habilitar paginação**, **Habilitar classificação**, **Habilitar edição**e **Habilitar exclusão**.

Clique em **Editar colunas**.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-2/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image29.png)

Na caixa **campos selecionados** , exclua **PersonID**, **LastName**e **HireDate**. Normalmente, você não exibe uma chave de registro para os usuários, a data de contratação não é relevante para os alunos e você colocará as duas partes do nome em um campo, portanto, você precisa apenas de um dos campos de nome.)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-2/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image31.png)

Selecione o campo **FirstMidName** e, em seguida, clique em **converter este campo em um TemplateField**.

Faça o mesmo para **EnrollmentDate**.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-2/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image33.png)

Clique em **OK** e alterne para o modo de exibição de **código-fonte** . As alterações restantes serão mais fáceis de fazer diretamente na marcação. A marcação de controle de `GridView` agora é semelhante ao exemplo a seguir.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample3.aspx)]

A primeira coluna após o campo de comando é um campo de modelo que atualmente exibe o nome. Altere a marcação para este campo de modelo para que se pareça com o exemplo a seguir:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample4.aspx)]

No modo de exibição, dois controles de `Label` exibem o nome e o sobrenome. No modo de edição, são fornecidas duas caixas de texto para que você possa alterar o nome e o sobrenome. Assim como com os controles de `Label` no modo de exibição, você usa `Bind` e `Eval` expressões exatamente como faria com controles de fonte de dados ASP.NET que se conectam diretamente aos bancos de dado. A única diferença é que você está especificando propriedades de entidade em vez de colunas de banco de dados.

A última coluna é um campo de modelo que exibe a data de registro. Altere a marcação para este campo para que fique semelhante ao exemplo a seguir:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample5.aspx)]

No modo de exibição e de edição, a cadeia de caracteres de formato "{0, d}" faz com que a data seja exibida no formato "data abreviada". (Seu computador pode estar configurado para exibir esse formato de forma diferente das imagens de tela mostradas neste tutorial.)

Observe que em cada um desses campos de modelo, o designer usou uma expressão de `Bind` por padrão, mas você alterou isso para uma expressão de `Eval` nos elementos de `ItemTemplate`. A expressão `Bind` torna os dados disponíveis em Propriedades de controle de `GridView`, caso você precise acessar os dados no código. Nesta página, você não precisa acessar esses dados no código para poder usar `Eval`, o que é mais eficiente. Para obter mais informações, consulte [obtendo seus dados dos controles de dados](https://weblogs.asp.net/davidfowler/archive/2008/12/12/getting-your-data-out-of-the-data-controls.aspx).

## <a name="revising-entitydatasource-control-markup-to-improve-performance"></a>Revisando marcação de controle de EntityDataSource para melhorar o desempenho

Na marcação para o controle de `EntityDataSource`, remova os atributos `ConnectionString` e `DefaultContainerName` e substitua-os por um atributo `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"`. Essa é uma alteração que você deve fazer sempre que criar um controle de `EntityDataSource`, a menos que você precise usar uma conexão diferente da que é embutida em código na classe de contexto de objeto. O uso do atributo `ContextTypeName` fornece os seguintes benefícios:

- Melhor desempenho. Quando o controle de `EntityDataSource` Inicializa o modelo de dados usando os atributos `ConnectionString` e `DefaultContainerName`, ele executa trabalho adicional para carregar metadados em cada solicitação. Isso não será necessário se você especificar o atributo `ContextTypeName`.
- O carregamento lento é ativado por padrão nas classes de contexto de objeto gerado (como `SchoolEntities` neste tutorial) em Entity Framework 4,0. Isso significa que as propriedades de navegação são carregadas com os dados relacionados automaticamente quando você precisar. O carregamento lento é explicado em mais detalhes posteriormente neste tutorial.
- Todas as personalizações que você aplicou à classe de contexto de objeto (nesse caso, a classe `SchoolEntities`) estarão disponíveis para controles que usam o controle `EntityDataSource`. A personalização da classe de contexto de objeto é um tópico avançado que não é abordado nesta série de tutoriais. Para obter mais informações, consulte [Estendendo Entity Framework tipos gerados](https://msdn.microsoft.com/library/dd456844.aspx).

A marcação agora será semelhante ao exemplo a seguir (a ordem das propriedades pode ser diferente):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample6.aspx)]

O atributo `EnableFlattening` refere-se a um recurso que era necessário em versões anteriores do Entity Framework porque as colunas de chave estrangeira não eram expostas como propriedades de entidade. A versão atual torna possível usar associações de *chave estrangeira*, o que significa que as propriedades de chave estrangeira são expostas para todas as associações, exceto muitos para muitos. Se suas entidades tiverem Propriedades de chave estrangeira e nenhum [tipo complexo](https://msdn.microsoft.com/library/bb738472.aspx), você poderá deixar esse atributo definido como `False`. Não remova o atributo da marcação, pois o valor padrão é `True`. Para obter mais informações, consulte [objetos de achatamento (EntityDataSource)](https://msdn.microsoft.com/library/ee404746.aspx).

Execute a página e você verá uma lista de alunos e funcionários (você filtrará apenas para estudantes no próximo tutorial). O nome e o sobrenome são exibidos juntos.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-2/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image35.png)

Para classificar a exibição, clique em um nome de coluna.

Clique em **Editar** em qualquer linha. São exibidas caixas de texto onde você pode alterar o nome e o sobrenome.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-2/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image37.png)

O botão **excluir** também funciona. Clique em Excluir para uma linha que tenha uma data de registro e a linha desaparece. (As linhas sem uma data de registro representam instrutores e você pode obter um erro de integridade referencial. No próximo tutorial, você filtrará essa lista para incluir apenas alunos.)

## <a name="displaying-data-from-a-navigation-property"></a>Exibindo dados de uma propriedade de navegação

Agora suponha que você deseja saber quantos cursos cada aluno está inscrito. O Entity Framework fornece essas informações na propriedade de navegação `StudentGrades` da entidade `Person`. Como o design do banco de dados não permite que um aluno seja registrado em um curso sem ter uma classificação atribuída, para este tutorial, você pode pressupor que ter uma linha na linha da tabela `StudentGrade` associada a um curso seja o mesmo que está sendo registrado no curso. (A propriedade de navegação `Courses` é apenas para instrutores.)

Quando você usa o atributo `ContextTypeName` do controle `EntityDataSource`, o Entity Framework recupera automaticamente as informações de uma propriedade de navegação quando você acessa essa propriedade. Isso é chamado de *carregamento lento*. No entanto, isso pode ser ineficiente, pois resulta em uma chamada separada para o banco de dados toda vez que forem necessárias informações adicionais. Se você precisar de dados da propriedade de navegação para cada entidade retornada pelo controle de `EntityDataSource`, será mais eficiente recuperar os dados relacionados junto com a própria entidade em uma única chamada para o banco de dados. Isso é chamado de *carregamento adiantado*e você especifica o carregamento adiantado para uma propriedade de navegação definindo a propriedade `Include` do controle de `EntityDataSource`.

Em *students. aspx*, você deseja mostrar o número de cursos para cada aluno, portanto, o carregamento adiantado é a melhor opção. Se você estivesse exibindo todos os alunos, mas mostrando o número de cursos apenas para alguns deles (o que exigiria escrever algum código além da marcação), o carregamento lento pode ser uma opção melhor.

Abra ou alterne para *students. aspx*, alterne para o modo **Design** , selecione `StudentsEntityDataSource`e, na janela **Propriedades** , defina a propriedade **include** como **StudentGrades**. (Se você quisesse obter várias propriedades de navegação, poderia especificar seus nomes separados por vírgulas — por exemplo, **StudentGrades, cursos**.)

[![Image19](the-entity-framework-and-aspnet-getting-started-part-2/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image39.png)

Alternar para o modo de exibição de **código-fonte** . No controle `StudentsGridView`, após o último elemento `asp:TemplateField`, adicione o novo campo de modelo a seguir:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample7.aspx)]

Na expressão `Eval`, você pode referenciar a propriedade de navegação `StudentGrades`. Como essa propriedade contém uma coleção, ela tem uma propriedade `Count` que você pode usar para exibir o número de cursos nos quais o aluno está registrado. Em um tutorial posterior, você verá como exibir dados de propriedades de navegação que contêm entidades únicas em vez de coleções. (Observe que você não pode usar elementos de `BoundField` para exibir dados de propriedades de navegação.)

Execute a página e agora você verá quantos cursos cada aluno está inscrito.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image41.png)

## <a name="using-a-detailsview-control-to-insert-entities"></a>Usando um controle DetailsView para inserir entidades

A próxima etapa é criar uma página que tenha um controle de `DetailsView` que permitirá que você adicione novos alunos. Feche o navegador e crie uma nova página da Web usando a página mestra *site. Master* . Nomeie a página *StudentsAdd. aspx*e, em seguida, alterne para o modo de exibição de **código-fonte** .

Adicione a marcação a seguir para substituir a marcação existente para o controle de `Content` chamado `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample8.aspx)]

Essa marcação cria um controle de `EntityDataSource` semelhante ao que você criou no *students. aspx*, exceto pelo fato de que permite a inserção. Assim como com o controle de `GridView`, os campos associados do controle de `DetailsView` são codificados exatamente como seriam para um controle de dados que se conecta diretamente a um banco, exceto que eles fazem referência a propriedades de entidade. Nesse caso, o controle de `DetailsView` é usado apenas para inserir linhas, de modo que você definiu o modo padrão como `Insert`.

Execute a página e adicione um novo aluno.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image43.png)

Nada acontecerá depois que você inserir um novo aluno, mas se você executar o Student *. aspx*, você verá as informações do novo aluno.

## <a name="displaying-data-in-a-drop-down-list"></a>Exibindo dados em uma lista suspensa

Nas etapas a seguir, você associará um controle de `DropDownList` a um conjunto de entidades usando um controle de `EntityDataSource`. Nesta parte do tutorial, você não fará muito com essa lista. Em partes subsequentes, no entanto, você usará a lista para permitir que os usuários selecionem um departamento para exibir os cursos associados ao departamento.

Crie uma nova página da Web chamada *courses. aspx*. No modo de exibição de **origem** , adicione um título ao controle de `Content` chamado `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample9.aspx)]

No modo **design** , adicione um controle de `EntityDataSource` à página como fazia antes, exceto por esse tempo, com o nome `DepartmentsEntityDataSource`. Selecione **departamentos** como o valor **EntitySetName** e selecione apenas as propriedades **DepartmentID** e **Name** .

[![Image15](the-entity-framework-and-aspnet-getting-started-part-2/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image45.png)

Na guia **padrão** da caixa de **ferramentas**, arraste um controle `DropDownList` para a página, nomeie-o `DepartmentsDropDownList`, clique na marca inteligente e selecione **escolher fonte de dados** para iniciar o **Assistente de configuração de DataSource**.

[![Image16](the-entity-framework-and-aspnet-getting-started-part-2/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image47.png)

Na etapa **escolher uma fonte de dados** , selecione **DepartmentsEntityDataSource** como a fonte de dados, clique em **Atualizar esquema**e, em seguida, selecione **nome** como o campo de dados a ser exibido e **DepartmentID** como o campo de dados de valor. Clique em **OK**.

[![Image17](the-entity-framework-and-aspnet-getting-started-part-2/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image49.png)

O método usado para associar o controle usando o Entity Framework é o mesmo que com outros controles de fonte de dados ASP.NET, exceto que você está especificando entidades e propriedades de entidade.

Alterne para o modo de exibição de **origem** e adicione "selecionar um departamento:" imediatamente antes do controle de `DropDownList`.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample10.aspx)]

Como lembrete, altere a marcação para o controle de `EntityDataSource` neste ponto substituindo os atributos `ConnectionString` e `DefaultContainerName` por um atributo `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"`. Geralmente, é melhor aguardar até que você tenha criado o controle de vinculação de dados que está vinculado ao controle da fonte de dados antes de alterar a marcação de controle de `EntityDataSource`, porque depois de fazer a alteração, o designer não fornecerá uma opção de **esquema de atualização** no controle de vinculação de dados.

Execute a página e você pode selecionar um departamento na lista suspensa.

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image51.png)

Isso conclui a introdução ao uso do controle de `EntityDataSource`. Trabalhar com esse controle geralmente não é diferente de trabalhar com outros controles de fonte de dados ASP.NET, exceto que você faz referência a entidades e propriedades em vez de tabelas e colunas. A única exceção é quando você deseja acessar as propriedades de navegação. No próximo tutorial, você verá que a sintaxe usada com `EntityDataSource` controle também pode ser diferente de outros controles de fonte de dados quando você filtrar, agrupar e ordenar dados.

> [!div class="step-by-step"]
> [Anterior](the-entity-framework-and-aspnet-getting-started-part-1.md)
> [Próximo](the-entity-framework-and-aspnet-getting-started-part-3.md)
