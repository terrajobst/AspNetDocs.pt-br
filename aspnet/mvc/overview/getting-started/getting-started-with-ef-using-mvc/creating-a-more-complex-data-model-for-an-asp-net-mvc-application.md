---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
title: 'Tutorial: Criar um modelo de dados mais complexo para um aplicativo MVC ASP.NET'
author: tdykstra
description: Neste tutorial, você adicionará mais entidades e relações e personalizará o modelo de dados especificando a formatação, a validação e as regras de mapeamento de banco de dado.
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: 46f7f3c9-274f-4649-811d-92222a9b27e2
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 933354b09eb4674e6f523f8a65816410521f026f
ms.sourcegitcommit: aa3c2efd56466fc6bdc387ee01ad6f50a261665b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/26/2019
ms.locfileid: "70021002"
---
# <a name="tutorial-create-a-more-complex-data-model-for-an-aspnet-mvc-app"></a>Tutorial: Criar um modelo de dados mais complexo para um aplicativo MVC ASP.NET

Nos tutoriais anteriores, você trabalhou com um modelo de dados simples que era composto por três entidades. Neste tutorial, você adicionará mais entidades e relações e personalizará o modelo de dados especificando a formatação, a validação e as regras de mapeamento de Database. Este artigo mostra duas maneiras de personalizar o modelo de dados: Adicionando atributos a classes de entidade e adicionando código à classe de contexto Database.

Quando terminar, as classes de entidade formarão o modelo de dados concluído mostrado na seguinte ilustração:

![School_class_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

Neste tutorial, você:

> [!div class="checklist"]
> * Personalizar o modelo de dados
> * Atualizar entidade de aluno
> * Criar a entidade Instructor
> * Criar a entidade OfficeAssignment
> * Modificar a entidade do curso
> * Criar a entidade Department
> * Modificar a entidade Enrollment
> * Adicionar código ao contexto do banco de dados
> * Propagar o banco de dados com dados de teste
> * Adicionar uma migração
> * Atualizar o banco de dados

## <a name="prerequisites"></a>Prerequisites

* [Code First migrações e implantação](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="customize-the-data-model"></a>Personalizar o modelo de dados

Nesta seção, você verá como personalizar o modelo de dados usando atributos que especificam formatação, validação e regras de mapeamento de banco de dados. Em seguida, em várias das seções a seguir, você criará o modelo de dados completo `School` adicionando atributos às classes que você já criou e criando novas classes para os tipos de entidade restantes no modelo.

### <a name="the-datatype-attribute"></a>O atributo DataType

Para datas de registro de alunos, todas as páginas da Web atualmente exibem a hora junto com a data, embora tudo o que você deseje exibir nesse campo seja a data. Usando atributos de anotação de dados, você pode fazer uma alteração de código que corrigirá o formato de exibição em cada exibição que mostra os dados. Para ver um exemplo de como fazer isso, você adicionará um atributo à propriedade `EnrollmentDate` na classe `Student`.

No *Models\Student.cs*, adicione uma `using` instrução para o `System.ComponentModel.DataAnnotations` `DataType` namespacee`DisplayFormat` adicione atributos e à propriedade,conformemostradonoexemploaseguir:`EnrollmentDate`

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,12-13)]

O atributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) é usado para especificar um tipo de dados que seja mais específico do que o tipo intrínseco de Database. Nesse caso, apenas desejamos acompanhar a data, não a data e a hora. A [Enumeração DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) fornece vários tipos de dados, como *data, hora, PhoneNumber, moeda, EmailAddress* e muito mais. O atributo `DataType` também pode permitir que o aplicativo forneça automaticamente recursos específicos a um tipo. Por exemplo, um `mailto:` link pode ser criado para [DataType. EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)e um seletor de data pode ser fornecido para [DataType. Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) em navegadores que dão suporte a [HTML5](http://html5.org/). Os atributos [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) emite os atributos [de dados](http://ejohn.org/blog/html-5-data-attributes/) HTML 5-(pronuncia-se *os dados Dash*) que os navegadores HTML 5 podem entender. Os atributos de [tipo de dados](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) não fornecem nenhuma validação.

`DataType.Date` não especifica o formato da data exibida. Por padrão, o campo de dados é exibido de acordo com os formatos padrão com base no [CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx)do servidor.

O atributo `DisplayFormat` é usado para especificar explicitamente o formato de data:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample2.cs)]

A `ApplyFormatInEditMode` configuração especifica que a formatação especificada também deve ser aplicada quando o valor é exibido em uma caixa de texto para edição. (Talvez você não queira que, para alguns campos — por exemplo, para valores de moeda, talvez não queira o símbolo de moeda na caixa de texto para edição.)

Você pode usar o atributo [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) por si só, mas, em geral, é uma boa ideia usar o atributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) também. O `DataType` atributo transmite a *semântica* dos dados em vez de como renderizá-los em uma tela e fornece os seguintes benefícios que `DisplayFormat`você não obtém:

- O navegador pode habilitar os recursos do HTML5 (por exemplo, mostrar um controle de calendário, o símbolo de moeda apropriado à localidade, links de email, uma validação de entrada do lado do cliente, etc.).
- Por padrão, o navegador renderizará dados usando o formato correto com base em sua [localidade](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx).
- O atributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) pode habilitar o MVC a escolher o modelo de campo à direita para renderizar os dados (o [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) usa o modelo de cadeia de caracteres). Para obter mais informações, consulte Brad Wilson ' s [ASP.NET MVC 2 templates](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). (Embora seja escrito para MVC 2, este artigo ainda se aplica à versão atual do ASP.NET MVC.)

Se você usar o `DataType` atributo com um campo de data, precisará especificar o `DisplayFormat` atributo também para garantir que o campo seja renderizado corretamente em navegadores Chrome. Para obter mais informações, consulte [este thread StackOverflow](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

Para obter mais informações sobre como lidar com outros formatos de data no MVC, [acesse a introdução do MVC 5: Examinando os métodos de edição e](../introduction/examining-the-edit-methods-and-edit-view.md) o modo de exibição de edição &quot;e pesquisa&quot;na página para internacionalização.

Execute a página de índice de estudante novamente e observe que os horários não são mais exibidos para as datas de registro. O mesmo será verdadeiro para qualquer exibição que use o `Student` modelo.

![Students_index_page_with_formatted_date](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image2.png)

### <a name="the-stringlengthattribute"></a>O StringLengthAttribute

Você também pode especificar regras de validação de dados e mensagens de erro de validação usando atributos. O [atributo StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) define o comprimento máximo no banco de dados e fornece validação do lado do cliente e do servidor para o ASP.NET MVC. Você também pode especificar o tamanho mínimo da cadeia de caracteres nesse atributo, mas o valor mínimo não tem nenhum impacto sobre o esquema de banco de dados.

Suponha que você deseje garantir que os usuários não insiram mais de 50 caracteres em um nome. Para adicionar essa limitação, adicione atributos [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) às `LastName` Propriedades e `FirstMidName` , conforme mostrado no exemplo a seguir:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=10,12)]

O atributo [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) não impedirá que um usuário insira espaço em branco para um nome. Você pode usar o atributo [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) para aplicar restrições à entrada. Por exemplo, o código a seguir requer que o primeiro caractere esteja em letras maiúsculas e os caracteres restantes sejam alfabéticos:

`[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]`

O atributo [MaxLength](https://msdn.microsoft.com/library/System.ComponentModel.DataAnnotations.MaxLengthAttribute.aspx) fornece funcionalidade semelhante ao atributo [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) , mas não fornece validação do lado do cliente.

Execute o aplicativo e clique na guia **alunos** . Você Obtém o seguinte erro:

*O modelo que faz o backup do contexto ' SchoolContext ' foi alterado desde que o banco de dados foi criado. Considere o uso de Migrações do Code First para atualizar o[https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)banco de dados ().*

O modelo de banco de dados foi alterado de uma maneira que requer uma alteração no esquema de banco de dados e Entity Framework detectado. Você usará as migrações para atualizar o esquema sem perder os dados que você adicionou ao banco de dado usando a interface do usuário. Se você alterou os dados que foram criados `Seed` pelo método, eles serão alterados de volta para seu estado original devido ao método [AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) que você `Seed` está usando no método. ([AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) é equivalente a uma operação "Upsert" da terminologia do banco de dados.)

No PMC (Console do Gerenciador de Pacotes), Insira os seguintes comandos:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample4.cmd)]

O `add-migration` comando cria um arquivo chamado  *&lt;timestamp&gt;\_MaxLengthOnNames.cs*. Esse arquivo contém o código no método `Up` que atualizará o banco de dados para que ele corresponda ao modelo de dados atual. O comando `update-database` executou esse código.

O carimbo de data/hora anexado ao nome do arquivo de migrações é usado pelo Entity Framework para ordenar as migrações. Você pode criar várias migrações antes de executar `update-database` o comando e, em seguida, todas as migrações são aplicadas na ordem em que foram criadas.

Execute a página **criar** e insira um nome com mais de 50 caracteres. Quando você clica em **criar**, a validação do lado do cliente mostra uma mensagem de erro: *O campo LastName deve ser uma cadeia de caracteres com um comprimento máximo de 50.*

### <a name="the-column-attribute"></a>O atributo de coluna

Você também pode usar atributos para controlar como as classes e propriedades são mapeadas para o banco de dados. Suponha que você tenha usado o nome `FirstMidName` para o campo de nome porque o campo também pode conter um sobrenome. Mas você deseja que a coluna do banco de dados seja nomeada `FirstName`, pois os usuários que escreverão consultas ad hoc no banco de dados estão acostumados com esse nome. Para fazer esse mapeamento, use o atributo `Column`.

O atributo `Column` especifica que quando o banco de dados for criado, a coluna da tabela `Student` que é mapeada para a propriedade `FirstMidName` será nomeada `FirstName`. Em outras palavras, quando o código se referir a `Student.FirstMidName`, os dados serão obtidos ou atualizados na coluna `FirstName` da tabela `Student`. Se você não especificar nomes de coluna, eles receberão o mesmo nome que o nome da propriedade.

No arquivo *Student.cs* , adicione uma `using` instrução para [System. ComponentModel. Annotations. Schema](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.aspx) e adicione `FirstMidName` o atributo nome da coluna à propriedade, conforme mostrado no seguinte código realçado:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample5.cs?highlight=4,14)]

A adição do [atributo Column](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) altera o modelo de backup do SchoolContext, de modo que ele não corresponderá ao banco de dados. Insira os seguintes comandos no PMC para criar outra migração:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample6.cmd)]

No **Gerenciador de servidores**, abra o designer de tabela do *estudante* clicando duas vezes na tabela *Student* .

A imagem a seguir mostra o nome da coluna original como era antes de aplicar as duas primeiras migrações. Além do nome da coluna que muda de `FirstMidName` para `FirstName`, as duas colunas de nome foram alteradas de `MAX` comprimento para 50 caracteres.

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image5.png)

Você também pode fazer alterações de mapeamento de banco de dados usando a [API Fluent](https://msdn.microsoft.com/data/jj591617), como você verá mais adiante neste tutorial.

> [!NOTE]
> Se você tentar compilar antes de concluir a criação de todas as classes de entidade nas seções a seguir, poderá receber erros do compilador.

## <a name="update-student-entity"></a>Atualizar entidade de aluno

No *Models\Student.cs*, substitua o código que você adicionou anteriormente com o código a seguir. As alterações são realçadas.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=11,13,15,18,22,25-32)]

### <a name="the-required-attribute"></a>O atributo necessário

O [Atributo Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) torna os campos de propriedades de nome obrigatórios. O `Required attribute` não é necessário para tipos de valor como DateTime, int, Double e float. Tipos de valor não podem ser atribuídos a um valor nulo, portanto, são tratados inerentemente como campos obrigatórios. 

O atributo `Required` precisa ser usado com `MinimumLength` para que `MinimumLength` seja imposto.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample8.cs?highlight=2)]

`MinimumLength` e `Required` permitem que o espaço em branco atenda à validação. Use o `RegularExpression` atributo para controll completo sobre a cadeia de caracteres.

### <a name="the-display-attribute"></a>O atributo de exibição

O atributo `Display` especifica que a legenda para as caixas de texto deve ser "Nome", "Sobrenome", "Nome Completo" e "Data de Registro", em vez do nome de a propriedade em cada instância (que não tem nenhum espaço entre as palavras).

### <a name="the-fullname-calculated-property"></a>A propriedade calculada FullName

`FullName` é uma propriedade calculada que retorna um valor criado pela concatenação de duas outras propriedades. Portanto, ele tem apenas `get` um acessador `FullName` e nenhuma coluna será gerada no banco de dados.

## <a name="create-instructor-entity"></a>Criar a entidade Instructor

Crie *Models\Instructor.cs*, substituindo o código do modelo pelo código a seguir:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample9.cs)]

Observe que várias propriedades são iguais nas entidades `Student` e `Instructor`. No tutorial [Implementando a herança](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md) mais adiante nesta série, você refatorará esse código para eliminar a redundância.

Você pode colocar vários atributos em uma linha, portanto, você também pode escrever a classe do instrutor da seguinte maneira:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

### <a name="the-courses-and-officeassignment-navigation-properties"></a>As propriedades de navegação de cursos e OfficeAssignment

As propriedades `Courses` e `OfficeAssignment` são propriedades de navegação. Como foi explicado anteriormente, eles normalmente são definidos como [virtuais](https://msdn.microsoft.com/library/9fkccyh4(v=vs.110).aspx) para que possam aproveitar um recurso Entity Framework chamado [carregamento lento](https://msdn.microsoft.com/magazine/hh205756.aspx). Além disso, se uma propriedade de navegação puder conter várias entidades, seu tipo deverá implementar [a&lt;interface&gt; T do ICollection](https://msdn.microsoft.com/library/92t2ye13.aspx) . Por exemplo [,&lt;IList&gt; T](https://msdn.microsoft.com/library/5y536ey6.aspx) é qualificado, mas não [&lt;IEnumerable t&gt; ](https://msdn.microsoft.com/library/9eekhta0.aspx) porque `IEnumerable<T>` não implementa [Add](https://msdn.microsoft.com/library/63ywd54z.aspx).

Um instrutor pode ensinar qualquer número de cursos, portanto `Courses` , é definido como uma coleção `Course` de entidades.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

Nossas regras de negócio afirmam que um instrutor só pode ter no máximo um `OfficeAssignment` escritório, portanto, é `OfficeAssignment` definido como uma única entidade `null` (que pode ser se nenhum escritório for atribuído).

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

## <a name="create-officeassignment-entity"></a>Criar a entidade OfficeAssignment

Crie *Models\OfficeAssignment.cs* com o seguinte código:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

Compile o projeto, que salva as alterações e verifica se você não fez nenhum erro de cópia e colagem que o compilador pode detectar.

### <a name="the-key-attribute"></a>O atributo de chave

Há uma relação um-para-zero-ou-um entre as `Instructor` `OfficeAssignment` entidades e. Uma atribuição do Office existe somente em relação ao instrutor ao qual ele está atribuído e, portanto, sua chave primária também é sua chave estrangeira `Instructor` para a entidade. Mas o Entity Framework não pode reconhecer `InstructorID` automaticamente como a chave primária desta entidade porque seu nome não segue a `ID` Convenção de nomenclatura ou *ClassName* `ID` . Portanto, o atributo `Key` é usado para identificá-la como a chave:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample14.cs)]

Você também pode usar o `Key` atributo se a entidade tiver sua própria chave primária, mas desejar nomear a propriedade como algo diferente de `classnameID` ou `ID`. Por padrão, o EF trata a chave como não gerada pelo banco de dados porque a coluna é para uma relação de identificação.

### <a name="the-foreignkey-attribute"></a>O atributo ForeignKey

Quando há uma relação um-para-zero-ou-um ou uma relação um-para-um entre duas entidades (como entre `OfficeAssignment` e `Instructor`), o EF não pode solucionar qual extremidade da relação é a entidade de segurança e qual final é dependente. Relações um-para-um têm uma propriedade de navegação de referência em cada classe para a outra classe. O [atributo ForeignKey](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx) pode ser aplicado à classe dependent para estabelecer a relação. Se você omitir o [atributo ForeignKey](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx), obterá o seguinte erro ao tentar criar a migração:

*Não é possível determinar a extremidade principal de uma associação entre os tipos ' ContosoUniversity. Models. OfficeAssignment ' e ' ContosoUniversity. Models. instrutor '. A extremidade principal dessa associação deve ser explicitamente configurada usando a API Fluent ou as anotações de dados da relação.*

Posteriormente no tutorial, você verá como configurar essa relação com a API Fluent.

### <a name="the-instructor-navigation-property"></a>A propriedade de navegação do instrutor

A `Instructor` entidade tem uma propriedade `OfficeAssignment` de navegação anulável (porque um instrutor pode não ter uma atribuição do Office) e `OfficeAssignment` a entidade tem uma propriedade de `Instructor` navegação não anulável (porque uma atribuição do Office não pode existir sem um instrutor – `InstructorID` é não anulável. Quando uma `Instructor` entidade tem uma entidade `OfficeAssignment` relacionada, cada entidade terá uma referência para a outra em sua propriedade de navegação.

Você pode colocar um `[Required]` atributo na propriedade de navegação do instrutor para especificar que deve haver um instrutor relacionado, mas você não precisa fazer isso porque a chave estrangeira do instrutorid (que também é a chave para essa tabela) é não anulável.

## <a name="modify-the-course-entity"></a>Modificar a entidade do curso

No *Models\Course.cs*, substitua o código que você adicionou anteriormente com o seguinte código:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

A entidade curso tem uma propriedade `DepartmentID` Foreign Key que aponta para a entidade relacionada `Department` e tem uma propriedade `Department` de navegação. O Entity Framework não exige que você adicione uma propriedade de chave estrangeira ao modelo de dados quando você tem uma propriedade de navegação para uma entidade relacionada. O EF cria automaticamente chaves estrangeiras no banco de dados onde quer que sejam necessárias. No entanto, ter a chave estrangeira no modelo de dados pode tornar as atualizações mais simples e mais eficientes. Por exemplo, quando você buscar uma entidade de curso para editar, `Department` a entidade será nula se você não carregá-la, então, ao atualizar a entidade do curso, você precisará primeiro `Department` buscar a entidade. Quando a propriedade `DepartmentID` de chave estrangeira é incluída no modelo de dados, você não precisa buscar a `Department` entidade antes de atualizar.

### <a name="the-databasegenerated-attribute"></a>O atributo DatabaseGenerated

O [atributo DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute.aspx) com o parâmetro [None](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.110).aspx) na propriedade `CourseID` especifica que os valores de chave primária são fornecidos pelo usuário em vez de gerados pelo banco de dados.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

Por padrão, o Entity Framework assume que os valores de chave primária são gerados pelo banco de dados. É isso que você quer na maioria dos cenários. No entanto `Course` , para entidades, você usará um número de curso especificado pelo usuário, como uma série 1000 para um departamento, uma série 2000 para outro departamento e assim por diante.

### <a name="foreign-key-and-navigation-properties"></a>Chave estrangeira e propriedades de navegação

As propriedades de chave estrangeira e as propriedades de `Course` navegação na entidade refletem as seguintes relações:

- Um curso é atribuído a um departamento e, portanto, há uma propriedade de chave estrangeira `DepartmentID` e de navegação `Department` pelas razões mencionadas acima.

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]
- Um curso pode ter qualquer quantidade de estudantes inscritos; portanto, a propriedade de navegação `Enrollments` é uma coleção:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample18.cs)]
- Um curso pode ser ministrado por vários instrutores; portanto, a propriedade de navegação `Instructors` é uma coleção:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample19.cs)]

## <a name="create-the-department-entity"></a>Criar a entidade Department

Crie *Models\Department.cs* com o seguinte código:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample20.cs)]

### <a name="the-column-attribute"></a>O atributo de coluna

Anteriormente, você usou o [atributo Column](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) para alterar o mapeamento de nome de coluna. No código para a `Department` entidade, o `Column` atributo está sendo usado para alterar o mapeamento de tipo de dados SQL para que a coluna seja definida usando o tipo de SQL Server [Money](https://msdn.microsoft.com/library/ms179882.aspx) no banco de dado:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample21.cs)]

O mapeamento de coluna geralmente não é necessário, porque o Entity Framework geralmente escolhe o tipo de dados apropriado SQL Server com base no tipo CLR que você define para a propriedade. O tipo `decimal` CLR é mapeado para um tipo `decimal` SQL Server. Mas, nesse caso, você sabe que a coluna manterá valores de moeda e o tipo de dados [Money](https://msdn.microsoft.com/library/ms179882.aspx) é mais apropriado para isso. Para obter mais informações sobre tipos de dados CLR e como eles correspondem a SQL Server tipos de dados, consulte [SqlClient para Entity FrameworkTypes](https://msdn.microsoft.com/library/bb896344.aspx).

### <a name="foreign-key-and-navigation-properties"></a>Chave estrangeira e propriedades de navegação

As propriedades de navegação e de chave estrangeira refletem as seguintes relações:

- Um departamento pode ou não ter um administrador, e um administrador é sempre um instrutor. Portanto, `InstructorID` a propriedade é incluída como a chave estrangeira para `Instructor` a entidade e um ponto de interrogação é adicionado `int` após a designação de tipo para marcar a propriedade como anulável. A propriedade de navegação é `Administrator` chamada, mas `Instructor` contém uma entidade:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample22.cs)]
- Um departamento pode ter muitos cursos, portanto, há uma `Courses` propriedade de navegação:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample23.cs)]

  > [!NOTE]
  > Por convenção, o Entity Framework habilita a exclusão em cascata para chaves estrangeiras que não permitem valor nulo e em relações muitos para muitos. Isso pode resultar em regras de exclusão em cascata circular, que causará uma exceção quando você tentar adicionar uma migração. Por exemplo, se você não definiu `Department.InstructorID` a propriedade como anulável, obteria a seguinte mensagem de exceção: "A relação referencial resultará em uma referência cíclica que não é permitida." Se a propriedade necessária `InstructorID` de regras de negócio for não anulável, você precisaria usar a seguinte instrução de API fluente para desabilitar a exclusão em cascata na relação:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample24.cs)]

## <a name="modify-the-enrollment-entity"></a>Modificar a entidade Enrollment

 No *Models\Enrollment.cs*, substitua o código que você adicionou anteriormente com o código a seguir

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample25.cs?highlight=1,15)]

### <a name="foreign-key-and-navigation-properties"></a>Chave estrangeira e propriedades de navegação

As propriedades de navegação e de chave estrangeira refletem as seguintes relações:

- Um registro destina-se a um único curso e, portanto, há uma propriedade de chave estrangeira `CourseID` e uma propriedade de navegação `Course`:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample26.cs)]
- Um registro destina-se a um único aluno e, portanto, há uma propriedade de chave estrangeira `StudentID` e uma propriedade de navegação `Student`:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample27.cs)]

### <a name="many-to-many-relationships"></a>Relações muitos para muitos

Há uma relação muitos-para- `Student` muitos entre as entidades e `Course` e a `Enrollment` entidade funciona como uma tabela de junção muitos para muitos *com carga* no banco de dados. Isso significa que a `Enrollment` tabela contém dados adicionais além de chaves estrangeiras para as tabelas Unidas (nesse caso, uma chave primária e uma `Grade` Propriedade).

A ilustração a seguir mostra a aparência dessas relações em um diagrama de entidades. (Este diagrama foi gerado usando o [Entity Framework Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d); criar o diagrama não faz parte do tutorial, ele está sendo usado aqui como uma ilustração.)

![Student-Course_many-to-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image12.png)

Cada linha de relação tem um 1 em uma extremidade e um asterisco\*() no outro, indicando uma relação um-para-muitos.

Se a `Enrollment` tabela não incluir informações de nível, ela só precisaria conter as duas chaves `CourseID` estrangeiras e `StudentID`. Nesse caso, ele corresponderia a uma tabela de junção muitos para muitos *sem carga* (ou uma *tabela de junção pura*) no banco de dados, e você não teria que criar uma classe de modelo para ela. As `Instructor` entidades `Course` e têm esse tipo de relação muitos-para-muitos e, como você pode ver, não há nenhuma classe de entidade entre elas:

![Instrutor – Course_many-to-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image13.png)

Uma tabela de junção é necessária no banco de dados, no entanto, conforme mostrado no diagrama de banco de dados a seguir:

![Instrutor – Course_many-to-many_relationship_tables](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image14.png)

O Entity Framework cria automaticamente a `CourseInstructor` tabela, e você lê e atualiza-a indiretamente lendo e atualizando `Course.Instructors` as propriedades de `Instructor.Courses` navegação e.

## <a name="entity-relationship-diagram"></a>Diagrama de relação de entidade

A ilustração a seguir mostra o diagrama criado pelo Entity Framework Power Tools para o modelo Escola concluído.

![School_data_model_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

Além das linhas de relação muitos para muitos (\* para \*) e as linhas de relação de um para muitos (1 a \*), você pode ver aqui a linha de relação um-para-zero-ou-um (1 a 0.. 1) entre o `Instructor` e `OfficeAssignment` o entidades e a linha de relação zero ou-um para muitos (0.. 1 a \*) entre as entidades do instrutor e do departamento.

## <a name="add-code-to-database-context"></a>Adicionar código ao contexto do banco de dados

Em seguida, você adicionará as novas entidades `SchoolContext` à classe e personalizará parte do mapeamento usando chamadas de [API fluente](https://msdn.microsoft.com/data/jj591617) . A API é "fluente" porque é frequentemente usada pela cadeia de caracteres de uma série de chamadas de método em uma única instrução, como no exemplo a seguir:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample28.cs)]

Neste tutorial, você usará a API fluente somente para mapeamento de banco de dados que não pode fazer com atributos. No entanto, você também pode usar a API fluente para especificar a maioria das regras de formatação, validação e mapeamento que pode ser feita por meio de atributos. Alguns atributos como `MinimumLength` não podem ser aplicados com a API fluente. Como mencionado anteriormente, `MinimumLength` não altera o esquema, ele aplica apenas uma regra de validação do cliente e do servidor

Alguns desenvolvedores preferem usar a API fluente exclusivamente para que possam manter suas classes de entidade "limpas". Combine atributos e a API fluente se desejar. Além disso, há algumas personalizações que podem ser feitas apenas com a API fluente, mas em geral, a prática recomendada é escolher uma dessas duas abordagens e usar isso com o máximo de consistência possível.

Para adicionar as novas entidades ao modelo de dados e executar o mapeamento de banco de dado que você não fez usando atributos, substitua o código em *DAL\SchoolContext.cs* pelo seguinte código:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample29.cs)]

A nova instrução no método [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) configura a tabela de junção muitos para muitos:

- Para a relação muitos-para-muitos entre as `Instructor` entidades `Course` e, o código especifica os nomes de tabela e coluna para a tabela de junção. Code First pode configurar a relação muitos-para-muitos para você sem esse código, mas se você não chamá-lo, obterá nomes `InstructorInstructorID` padrão, como para a `InstructorID` coluna.

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample30.cs)]

O código a seguir fornece um exemplo de como você poderia ter usado a API fluente em vez de atributos para especificar a `Instructor` relação `OfficeAssignment` entre as entidades e:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample31.cs)]

Para obter informações sobre o que as instruções "API Fluent" estão fazendo nos bastidores, consulte a postagem do blog da [API fluente](https://blogs.msdn.com/b/aspnetue/archive/2011/05/04/entity-framework-code-first-tutorial-supplement-what-is-going-on-in-a-fluent-api-call.aspx) .

## <a name="seed-database-with-test-data"></a>Propagar o banco de dados com dados de teste

Substitua o código no arquivo *Migrations\Configuration.cs* pelo código a seguir para fornecer dados de semente para as novas entidades que você criou.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample32.cs)]

Como vimos no primeiro tutorial, a maior parte desse código simplesmente atualiza ou cria novos objetos de entidade e carrega dados de exemplo em Propriedades, conforme necessário para o teste. No entanto, observe `Course` como a entidade, que tem uma relação muitos para muitos com a `Instructor` entidade, é tratada:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample33.cs)]

Ao criar um `Course` objeto, você inicializa a `Instructors` propriedade de navegação como uma coleção vazia usando o código `Instructors = new List<Instructor>()`. Isso torna possível adicionar `Instructor` entidades relacionadas a isso `Course` usando o `Instructors.Add` método. Se você não criou uma lista vazia, não poderá adicionar essas relações, pois a `Instructors` Propriedade seria nula e não teria um `Add` método. Você também pode adicionar a inicialização da lista ao construtor.

## <a name="add-a-migration"></a>Adicionar uma migração

No PMC, insira o `add-migration` comando (não faça o `update-database` comando ainda):

`add-Migration ComplexDataModel`

Se você tiver tentado executar o comando `update-database` neste ponto (não faça isso ainda), receberá o seguinte erro:

*A instrução ALTER TABLE está em conflito com a restrição FOREIGN KEY "\_FK dbo. Curso\_dbo. DepartmentID do departamento\_". O conflito ocorreu no banco de dados "ContosoUniversity", tabela "dbo. Departamento ", coluna ' DepartmentID '.*

Às vezes, quando você executa migrações com dados existentes, precisa inserir dados de stub no banco de dado para satisfazer as restrições de chave estrangeira, e é isso o que você precisa fazer agora. O código gerado no método ComplexDataModel `Up` adiciona uma chave estrangeira não anulável `DepartmentID` à `Course` tabela. Como já existem linhas na `Course` tabela quando o código é executado, a `AddColumn` operação falhará porque SQL Server não sabe qual valor deve ser colocado na coluna que não pode ser nula. Portanto, é necessário alterar o código para dar à nova coluna um valor padrão e criar um departamento de stub chamado "Temp" para atuar como o departamento padrão. Como resultado, as linhas `Course` existentes serão todas relacionadas ao departamento "Temp" depois que o `Up` método for executado. Você pode relacioná-los aos departamentos corretos no `Seed` método.

Edite &lt;o arquivo *&gt;timestamp\_ComplexDataModel.cs* , comente a linha de código que adiciona a coluna DepartmentID à tabela curso e adicione o seguinte código realçado (a linha comentada também é realçado):

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample34.cs?highlight=14-18)]

Quando o `Seed` método é executado, ele insere linhas na tabela `Department` e relaciona as linhas existentes `Course` a essas novas `Department` linhas. Se você não tiver adicionado nenhum curso na interface do usuário, não precisará mais do departamento "Temp" ou do valor padrão na `Course.DepartmentID` coluna. Para permitir a possibilidade de que alguém possa ter adicionado cursos usando o aplicativo, você também precisaria atualizar o código do `Seed` método para garantir que todas as `Course` linhas (não apenas aquelas inseridas `Seed` por execuções anteriores do método) tenham valores `DepartmentID` válidos antes de remover o valor padrão da coluna e excluir o departamento "Temp".

## <a name="update-the-database"></a>Atualizar o banco de dados

Depois de terminar de editar o &lt;arquivo *&gt;\_timestamp ComplexDataModel.cs* , insira o `update-database` comando no PMC para executar a migração.

[!code-powershell[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample35.ps1)]

> [!NOTE]
> É possível obter outros erros ao migrar dados e fazer alterações de esquema. Se você receber erros de migração que não consegue resolver, altere o nome do banco de dados na cadeia de conexão ou exclua o banco de dados. A abordagem mais simples é renomear o banco de dados no arquivo *Web. config* . O exemplo a seguir mostra o nome alterado para\_cu Test:
>
> [!code-xml[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample36.xml?highlight=1)]
>
> Com um novo banco de dados, não há nenhum dado a ser migrado e o `update-database` comando é muito mais provável de ser concluído sem erros. Para obter instruções sobre como excluir o banco de dados, consulte [como remover um banco de dados do Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).
>
> Se isso falhar, outra coisa que você pode tentar é inicializar novamente o banco de dados digitando o seguinte comando no PMC:
>
> `update-database -TargetMigration:0`

Abra o banco de dados no **Gerenciador de servidores** como fazia anteriormente e expanda o nó **tabelas** para ver que todas as tabelas foram criadas. (Se você ainda tiver **Gerenciador de servidores** aberto a partir do momento anterior, clique no botão **Atualizar** .)

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image16.png)

Você não criou uma classe de modelo para `CourseInstructor` a tabela. Conforme explicado anteriormente, trata-se de uma tabela de junção para a relação muitos para muitos `Instructor` entre `Course` as entidades e.

Clique com o botão `CourseInstructor` direito do mouse na tabela e selecione **Mostrar dados da tabela** para verificar se ela contém dados como resultado `Instructor` das entidades que você adicionou `Course.Instructors` à propriedade de navegação.

![Table_data_in_CourseInstructor_table](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image17.png)

## <a name="get-the-code"></a>Obter o código

[Baixar projeto concluído](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Recursos adicionais

Links para outros recursos de Entity Framework podem ser encontrados nos [recursos de acesso a dados ASP.net-recomendado](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você:

> [!div class="checklist"]
> * Modelo de dados personalizado
> * Entidade Student atualizada
> * Criou a entidade Instructor
> * Criou a entidade OfficeAssignment
> * A entidade do curso foi modificada
> * Criou a entidade Department
> * Modificou a entidade de registro
> * Código adicionado ao contexto do banco de dados
> * Propagou o banco de dados com os dados de teste
> * Adicionou uma migração
> * Atualizou o banco de dados

Avance para o próximo artigo para saber como ler e exibir dados relacionados que o Entity Framework carrega em Propriedades de navegação.

> [!div class="nextstepaction"]
> [Ler dados relacionados](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
