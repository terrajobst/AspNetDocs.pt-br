---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Tutorial: ler dados relacionados com o EF em um aplicativo MVC ASP.NET'
description: Neste tutorial, você lerá e exibirá os dados relacionados, ou seja, os dados que o Entity Framework carrega nas propriedades de navegação.
author: tdykstra
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: 18cdd896-8ed9-4547-b143-114711e3eafb
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: d804c8dd45ad131949260c85d9d9c6683bfe9646
ms.sourcegitcommit: 84b1681d4e6253e30468c8df8a09fe03beea9309
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/02/2019
ms.locfileid: "73445651"
---
# <a name="tutorial-read-related-data-with-ef-in-an-aspnet-mvc-app"></a>Tutorial: ler dados relacionados com o EF em um aplicativo MVC ASP.NET

No tutorial anterior, você concluiu o modelo de dados da escola. Neste tutorial, você lerá e exibirá os dados relacionados, ou seja, os dados que o Entity Framework carrega nas propriedades de navegação.

As ilustrações a seguir mostram as páginas com as quais você trabalhará.

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

[Baixar projeto concluído](https://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

> O aplicativo Web de exemplo da Contoso University demonstra como criar aplicativos ASP.NET MVC 5 usando o Entity Framework 6 Code First e o Visual Studio. Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

Neste tutorial, você:

> [!div class="checklist"]
> * Aprender a carregar entidades relacionadas
> * Criar uma página Cursos
> * Criar uma página Instrutores

## <a name="prerequisites"></a>Prerequisites

* [Criar um modelo de dados mais complexo](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)

## <a name="learn-how-to-load-related-data"></a>Aprender a carregar entidades relacionadas

Há várias maneiras pelas quais o Entity Framework pode carregar dados relacionados nas propriedades de navegação de uma entidade:

- *Carregamento lento*. Quando a entidade é lida pela primeira vez, os dados relacionados não são recuperados. No entanto, na primeira vez que você tenta acessar uma propriedade de navegação, os dados necessários para essa propriedade de navegação são recuperados automaticamente. Isso resulta em várias consultas enviadas ao banco de dados — uma para a própria entidade e uma a cada vez que os dados relacionados para a entidade devem ser recuperados. A classe `DbContext` habilita o carregamento lento por padrão.

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *Carregamento adiantado*. Quando a entidade é lida, os dados relacionados são recuperados com ela. Normalmente, isso resulta em uma única consulta de junção que recupera todos os dados necessários. Você especifica o carregamento adiantado usando o método `Include`.

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *Carregamento explícito*. Isso é semelhante ao carregamento lento, exceto que você recupera explicitamente os dados relacionados no código; Ele não acontece automaticamente quando você acessa uma propriedade de navegação. Você carrega dados relacionados manualmente obtendo a entrada do Gerenciador de estado de objeto para uma entidade e chamando o método [Collection. Load](https://msdn.microsoft.com/library/gg696220(v=vs.103).aspx) para coleções ou o método [Reference. Load](https://msdn.microsoft.com/library/gg679166(v=vs.103).aspx) para propriedades que mantêm uma única entidade. (No exemplo a seguir, se você quisesse carregar a propriedade de navegação do administrador, você substituirá `Collection(x => x.Courses)` por `Reference(x => x.Administrator)`.) Normalmente, você usaria o carregamento explícito somente quando você ativou o carregamento lento.

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

Como eles não recuperam imediatamente os valores de propriedade, o carregamento lento e o carregamento explícito também são conhecidos como *carregamento adiado*.

### <a name="performance-considerations"></a>Considerações sobre desempenho

Se você sabe que precisa de dados relacionados para cada entidade recuperada, o carregamento adiantado costuma oferecer o melhor desempenho, porque uma única consulta enviada para o banco de dados é geralmente mais eficiente do que consultas separadas para cada entidade recuperada. Por exemplo, nos exemplos acima, suponha que cada departamento tenha dez cursos relacionados. O exemplo de carregamento adiantado resultaria em apenas uma única consulta (junção) e uma única viagem de ida e volta ao banco de dados. Os exemplos de carregamento lento e carregamento explícito resultariam em onze consultas e onze viagens de ida e volta ao banco de dados. As viagens de ida e volta extras para o banco de dados são especialmente prejudiciais ao desempenho quando a latência é alta.

Por outro lado, em alguns cenários, o carregamento lento é mais eficiente. O carregamento adiantado pode fazer com que uma junção muito complexa seja gerada, o que SQL Server não pode processar com eficiência. Ou, se você precisar acessar as propriedades de navegação de uma entidade somente para um subconjunto de um conjunto de entidades que está processando, o carregamento lento poderá ser melhor executado porque o carregamento adiantado recuperaria mais dados do que o necessário. Se o desempenho for crítico, será melhor testar o desempenho das duas maneiras para fazer a melhor escolha.

O carregamento lento pode mascarar o código que causa problemas de desempenho. Por exemplo, o código que não especifica carregamento adiantado ou explícito, mas processa um alto volume de entidades e usa várias propriedades de navegação em cada iteração pode ser muito ineficiente (devido a muitas viagens de ida e volta ao banco de dados). Um aplicativo que executa bem no desenvolvimento usando um SQL Server local pode ter problemas de desempenho quando movido para o banco de dados SQL do Azure devido à latência aumentada e ao carregamento lento. A criação de perfil de consultas de banco de dados com uma carga de teste realista ajudará você a determinar se o carregamento lento é apropriado. Para obter mais informações, consulte [Desmistificando estratégias de Entity Framework: carregando dados relacionados](https://msdn.microsoft.com/magazine/hh205756.aspx) e [usando o Entity Framework para reduzir a latência de rede para SQL Azure](https://msdn.microsoft.com/magazine/gg309181.aspx).

### <a name="disable-lazy-loading-before-serialization"></a>Desabilitar carregamento lento antes da serialização

Se você deixar o carregamento lento habilitado durante a serialização, poderá acabar consultando significativamente mais dados do que o desejado. A serialização geralmente funciona acessando cada propriedade em uma instância de um tipo. O acesso à propriedade dispara o carregamento lento e essas entidades carregadas lentas são serializadas. Em seguida, o processo de serialização acessa cada propriedade das entidades carregadas com o Lazy, potencialmente causando um carregamento e uma serialização mais lentos. Para evitar essa reação da cadeia de execução, desative o carregamento lento antes de serializar uma entidade.

A serialização também pode ser complicada pelas classes de proxy que o Entity Framework usa, conforme explicado no [tutorial cenários avançados](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#proxies).

Uma maneira de evitar problemas de serialização é serializar os DTOs (objetos de transferência de dados) em vez de objetos de entidade, conforme mostrado no tutorial [usando a API Web com Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-5.md) .

Se você não usar DTOs, poderá desabilitar o carregamento lento e evitar problemas de proxy [desabilitando a criação de proxy](https://msdn.microsoft.com/data/jj592886.aspx).

Aqui estão algumas outras [maneiras de desabilitar o carregamento lento](https://msdn.microsoft.com/data/jj574232):

- Para propriedades de navegação específicas, omita a palavra-chave `virtual` ao declarar a propriedade.
- Para todas as propriedades de navegação, defina `LazyLoadingEnabled` como `false`, coloque o seguinte código no construtor da sua classe de contexto:

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="create-a-courses-page"></a>Criar uma página Cursos

A entidade `Course` inclui uma propriedade de navegação que contém a entidade `Department` do departamento ao qual o curso está atribuído. Para exibir o nome do departamento atribuído em uma lista de cursos, você precisa obter a propriedade `Name` da entidade `Department` que está na propriedade de navegação `Course.Department`.

Crie um controlador chamado `CourseController` (não CoursesController) para o tipo de entidade `Course`, usando as mesmas opções para o **controlador MVC 5 com exibições, usando Entity Framework** scaffolder que você fez anteriormente para o controlador de `Student`:

| Configuração | Valor |
| ------- | ----- |
| Classe de modelo | Selecione **curso (ContosoUniversity. Models)** . |
| Classe de contexto de dados | Selecione **SchoolContext (ContosoUniversity. Dal)** . |
| Nome do controlador | Insira *CourseController*. Novamente, não *CoursesController* com um *s*. Quando você selecionou **curso (ContosoUniversity. Models)** , o valor do **nome do controlador** foi populado automaticamente. Você precisa alterar o valor. |

Deixe os outros valores padrão e adicione o controlador.

Abra *Controllers\CourseController.cs* e examine o método `Index`:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

O scaffolding automático especificou o carregamento adiantado para a propriedade de navegação `Department` usando o método `Include`.

Abra *Views\Course\Index.cshtml* e substitua o código do modelo pelo código a seguir. As alterações são realçadas:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,7,14-16,23-25,31-33,40-42)]

Você fez as seguintes alterações no código gerado por scaffolding:

- Alterado o título de **índice** para **cursos**.
- Adicionou uma coluna **Número** que mostra o valor da propriedade `CourseID`. Por padrão, as chaves primárias não são com Scaffold porque normalmente não têm significado para os usuários finais. No entanto, nesse caso, a chave primária é significativa e você deseja mostrá-la.
- Moveu a coluna **Department** para o lado direito e alterou seu título. O scaffolder optou corretamente por exibir a propriedade `Name` da entidade `Department`, mas aqui na página do curso o título da coluna deve ser **Department** , e não **Name**.

Observe que, para a coluna Department, o código com Scaffold exibe a propriedade `Name` da entidade `Department` que é carregada na propriedade de navegação `Department`:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=2)]

Execute a página (selecione a guia **cursos** no Home Page da Contoso University) para ver a lista com nomes de departamento.

## <a name="create-an-instructors-page"></a>Criar uma página Instrutores

Nesta seção, você criará um controlador e uma exibição para a `Instructor` entidade para exibir a página de instrutores. Essa página lê e exibe dados relacionados das seguintes maneiras:

- A lista de instrutores exibe dados relacionados da entidade `OfficeAssignment`. As entidades `Instructor` e `OfficeAssignment` estão em uma relação um para zero ou um. Você usará o carregamento adiantado para as entidades de `OfficeAssignment`. Conforme explicado anteriormente, o carregamento adiantado é geralmente mais eficiente quando você precisa dos dados relacionados para todas as linhas recuperadas da tabela primária. Nesse caso, você deseja exibir atribuições de escritório para todos os instrutores exibidos.
- Quando o usuário seleciona um instrutor, as entidades `Course` relacionadas são exibidas. As entidades `Instructor` e `Course` estão em uma relação muitos para muitos. Você usará o carregamento adiantado para as entidades de `Course` e suas entidades `Department` relacionadas. Nesse caso, o carregamento lento pode ser mais eficiente, pois você precisa de cursos apenas para o instrutor selecionado. No entanto, este exemplo mostra como usar o carregamento adiantado para propriedades de navegação em entidades que estão nas propriedades de navegação.
- Quando o usuário seleciona um curso, os dados relacionados do conjunto de entidades `Enrollments` são exibidos. As entidades `Course` e `Enrollment` estão em uma relação um-para-muitos. Você adicionará o carregamento explícito para entidades de `Enrollment` e suas entidades `Student` relacionadas. (O carregamento explícito não é necessário porque o carregamento lento está habilitado, mas isso mostra como fazer o carregamento explícito.)

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Criar um modelo de exibição para a exibição de índice do instrutor

A página instrutores mostra três tabelas diferentes. Portanto, você criará um modelo de exibição que inclui três propriedades, cada uma contendo os dados de uma das tabelas.

Na pasta *ViewModels* , crie *InstructorIndexData.cs* e substitua o código existente pelo código a seguir:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="create-the-instructor-controller-and-views"></a>Criar o controlador do instrutor e as exibições

Criar um controlador `InstructorController` (não InstructorsController) com a ação de leitura/gravação do EF:

| Configuração | Valor |
| ------- | ----- |
| Classe de modelo | Selecione **instrutor (ContosoUniversity. Models)** . |
| Classe de contexto de dados | Selecione **SchoolContext (ContosoUniversity. Dal)** . |
| Nome do controlador | Insira *InstructorController*. Novamente, não *InstructorsController* com um *s*. Quando você selecionou **curso (ContosoUniversity. Models)** , o valor do **nome do controlador** foi populado automaticamente. Você precisa alterar o valor. |

Deixe os outros valores padrão e adicione o controlador.

Abra *Controllers\InstructorController.cs* e adicione uma instrução `using` para o namespace `ViewModels`:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

O código com Scaffold no método `Index` especifica o carregamento adiantado somente para a propriedade de navegação `OfficeAssignment`:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Substitua o método `Index` pelo código a seguir para carregar dados relacionados adicionais e colocá-los no modelo de exibição:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

O método aceita dados de rota opcionais (`id`) e um parâmetro de cadeia de caracteres de consulta (`courseID`) que fornece os valores de ID do instrutor selecionado e do curso selecionado e transmite todos os dados necessários para a exibição. Os parâmetros são fornecidos pelos hiperlinks **Selecionar** na página.

O código começa com a criação de uma instância do modelo de exibição e colocando-a na lista de instrutores. O código especifica o carregamento adiantado para o `Instructor.OfficeAssignment` e a propriedade de navegação `Instructor.Courses`.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=3-4)]

O segundo método de `Include` carrega cursos e, para cada curso carregado, ele faz o carregamento rápido da propriedade de navegação `Course.Department`.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Como mencionado anteriormente, o carregamento adiantado não é necessário, mas é feito para melhorar o desempenho. Como a exibição sempre exige a entidade `OfficeAssignment`, é mais eficiente buscar isso na mesma consulta. `Course` entidades são necessárias quando um instrutor é selecionado na página da Web, portanto, o carregamento adiantado é melhor do que o carregamento lento somente se a página for exibida com mais frequência com um curso selecionado do que sem.

Se uma ID de instrutor foi selecionada, o instrutor selecionado será recuperado da lista de instrutores no modelo de exibição. A propriedade `Courses` do modelo de exibição é então carregada com as entidades de `Course` da propriedade de navegação `Courses` do instrutor.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

O método `Where` retorna uma coleção, mas, nesse caso, os critérios passados para esse método resultam em uma única entidade `Instructor` que está sendo retornada. O método `Single` converte a coleção em uma única entidade `Instructor`, que fornece acesso à propriedade `Courses` da entidade.

Você usa o método [único](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx) em uma coleção quando sabe que a coleção terá apenas um item. O método `Single` gera uma exceção se a coleção passada para ela está vazia ou se há mais de um item. Uma alternativa é [SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx), que retorna um valor padrão (`null` nesse caso) se a coleção estiver vazia. No entanto, nesse caso, isso ainda resultaria em uma exceção (de tentar encontrar uma propriedade de `Courses` em uma referência de `null`), e a mensagem de exceção indicaria menos clareza a causa do problema. Ao chamar o método `Single`, você também pode passar a condição `Where` em vez de chamar o método `Where` separadamente:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

Em vez de:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Em seguida, se um curso foi selecionado, o curso selecionado é recuperado na lista de cursos no modelo de exibição. Em seguida, a propriedade `Enrollments` do modelo de exibição é carregada com as entidades de `Enrollment` da propriedade de navegação `Enrollments` do curso.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

### <a name="modify-the-instructor-index-view"></a>Modificar a exibição do índice do instrutor

No *Views\Instructor\Index.cshtml*, substitua o código do modelo pelo código a seguir. As alterações são realçadas:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1,4,14-18,21,23-28,38-43,45)]

Você fez as seguintes alterações no código existente:

- Alterou a classe de modelo para `InstructorIndexData`.
- Alterou o título de página de **Índice** para **Instrutores**.
- Adicionada uma coluna **do Office** que exibe `item.OfficeAssignment.Location` somente se `item.OfficeAssignment` não for nulo. (Como essa é uma relação um-para-zero-ou-um, pode não haver uma entidade de `OfficeAssignment` relacionada.)

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]
- Foi adicionado o código que adicionará `class="success"` dinamicamente ao elemento `tr` do instrutor selecionado. Isso define uma cor de plano de fundo para a linha selecionada usando uma classe [Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap) .

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]
- Adição de um novo `ActionLink` rotulado **Select** imediatamente antes dos outros links em cada linha, o que faz com que a ID de instrutor selecionada seja enviada ao método `Index`.

Execute o aplicativo e selecione a guia **instrutores** . A página exibe a propriedade `Location` de entidades de `OfficeAssignment` relacionadas e uma célula de tabela vazia quando não há nenhuma entidade de `OfficeAssignment` relacionada.

No arquivo *Views\Instructor\Index.cshtml* , após o elemento `table` de fechamento (no final do arquivo), adicione o código a seguir. Esse código exibe uma lista de cursos relacionados a um instrutor quando um instrutor é selecionado.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Esse código lê a propriedade `Courses` do modelo de exibição para exibir uma lista de cursos. Ele também fornece um `Select` hiperlink que envia a ID do curso selecionado para o método de ação `Index`.

Execute a página e selecione um instrutor. Agora, você verá uma grade que exibe os cursos atribuídos ao instrutor selecionado, e para cada curso, verá o nome do departamento atribuído.

Após o bloco de código que você acabou de adicionar, adicione o código a seguir. Isso exibe uma lista dos alunos que estão registrados em um curso quando esse curso é selecionado.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Esse código lê a propriedade `Enrollments` do modelo de exibição para exibir uma lista de alunos registrados no curso.

Execute a página e selecione um instrutor. Em seguida, selecione um curso para ver a lista de alunos registrados e suas notas.

### <a name="adding-explicit-loading"></a>Adicionando carregamento explícito

Abra *InstructorController.cs* e veja como o método de `Index` Obtém a lista de registros de um curso selecionado:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

Quando você recuperou a lista de instrutores, você especificou o carregamento adiantado para a propriedade de navegação `Courses` e para a propriedade `Department` de cada curso. Em seguida, você coloca a coleção de `Courses` no modelo de exibição e agora está acessando a propriedade de navegação `Enrollments` de uma entidade nessa coleção. Como você não especificou o carregamento adiantado para a propriedade de navegação `Course.Enrollments`, os dados dessa propriedade são exibidos na página como resultado do carregamento lento.

Se você desabilitou o carregamento lento sem alterar o código de nenhuma outra forma, a propriedade `Enrollments` seria nula, independentemente de quantos registros o curso realmente tinha. Nesse caso, para carregar a propriedade `Enrollments`, você precisaria especificar o carregamento adiantado ou o carregamento explícito. Você já viu como fazer o carregamento adiantado. Para ver um exemplo de carregamento explícito, substitua o método `Index` pelo código a seguir, que carrega explicitamente a propriedade `Enrollments`. O código alterado está realçado.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs?highlight=20-31)]

Depois de obter a entidade `Course` selecionada, o novo código carrega explicitamente a propriedade de navegação `Enrollments` do curso:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

Em seguida, ele carrega explicitamente cada entidade de `Student` relacionada à entidade de `Enrollment`:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

Observe que você usa o método `Collection` para carregar uma propriedade de coleção, mas para uma propriedade que contém apenas uma entidade, você usa o método `Reference`.

Execute a página de índice do instrutor agora e você não verá nenhuma diferença no que é exibido na página, embora tenha alterado como os dados são recuperados.

## <a name="get-the-code"></a>Obter o código

[Baixar projeto concluído](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Recursos adicionais

Links para outros recursos de Entity Framework podem ser encontrados nos [recursos de acesso a dados ASP.net-recomendado](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você:

> [!div class="checklist"]
> * Aprendeu a carregar dados relacionados
> * Criou uma página Cursos
> * Criou uma página Instrutores

Vá para o próximo artigo para aprender a atualizar dados relacionados.

> [!div class="nextstepaction"]
> [Atualizar dados relacionados](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
