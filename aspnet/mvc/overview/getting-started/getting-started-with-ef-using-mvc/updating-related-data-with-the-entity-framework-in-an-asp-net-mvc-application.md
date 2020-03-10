---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Tutorial: atualizar dados relacionados com o EF em um aplicativo MVC ASP.NET'
description: Neste tutorial, você atualizará os dados relacionados. Para a maioria das relações, isso pode ser feito atualizando os campos de chave estrangeira ou as propriedades de navegação.
author: tdykstra
ms.author: riande
ms.date: 01/19/2019
ms.topic: tutorial
ms.assetid: 7ba88418-5d0a-437d-b6dc-7c3816d4ec07
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 4d5f6447fdccefdcdf9497a9e94f23243302a0e1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616000"
---
# <a name="tutorial-update-related-data-with-ef-in-an-aspnet-mvc-app"></a>Tutorial: atualizar dados relacionados com o EF em um aplicativo MVC ASP.NET

No tutorial anterior, você exibiu dados relacionados. Neste tutorial, você atualizará os dados relacionados. Para a maioria das relações, isso pode ser feito atualizando os campos de chave estrangeira ou as propriedades de navegação. Para relações muitos para muitos, o Entity Framework não expõe a tabela de junção diretamente, portanto, você adiciona e remove entidades de e para as propriedades de navegação apropriadas.

As ilustrações a seguir mostram algumas das páginas com as quais você trabalhará.

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

![Edição do instrutor com cursos](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Neste tutorial, você:

> [!div class="checklist"]
> * Personalizar páginas de cursos
> * Página Adicionar o Office à instrutor
> * Página Adicionar cursos a instrutores
> * Atualizar DeleteConfirmed
> * Adicionar local de escritório e cursos para a página Criar

## <a name="prerequisites"></a>Prerequisites

* [Leitura de dados relacionados](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="customize-courses-pages"></a>Personalizar páginas de cursos

Quando uma nova entidade de curso é criada, ela precisa ter uma relação com um departamento existente. Para facilitar isso, o código gerado por scaffolding inclui métodos do controlador e exibições Criar e Editar que incluem uma lista suspensa para seleção do departamento. A lista suspensa define a propriedade de chave estrangeira `Course.DepartmentID`, e isso é tudo o que Entity Framework precisa para carregar a propriedade de navegação `Department` com a entidade `Department` apropriada. Você usará o código gerado por scaffolding, mas o alterará ligeiramente para adicionar tratamento de erro e classificação à lista suspensa.

No *CourseController.cs*, exclua os quatro métodos `Create` e `Edit` e substitua-os pelo código a seguir:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,11-12,19-25,40,44,46,48-78)]

Adicione a seguinte instrução de `using` no início do arquivo:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

O método `PopulateDepartmentsDropDownList` Obtém uma lista de todos os departamentos classificados por nome, cria uma coleção de `SelectList` para uma lista suspensa e passa a coleção para a exibição em uma propriedade `ViewBag`. O método aceita o parâmetro `selectedDepartment` opcional que permite que o código de chamada especifique o item que será selecionado quando a lista suspensa for renderizada. A exibição passará o nome `DepartmentID` para o auxiliar [DropDownList](../../older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md) , e o auxiliar saberá procurar no objeto `ViewBag` para um `SelectList` chamado `DepartmentID`.

O método `HttpGet` `Create` chama o método `PopulateDepartmentsDropDownList` sem definir o item selecionado, porque, para um novo curso, o departamento ainda não foi estabelecido:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

O método `HttpGet` `Edit` define o item selecionado, com base na ID do departamento que já está atribuído ao curso que está sendo editado:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=12)]

Os métodos `HttpPost` para `Create` e `Edit` também incluem o código que define o item selecionado ao reexibir a página após um erro:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=6)]

Esse código garante que quando a página for exibida novamente para mostrar a mensagem de erro, qualquer departamento selecionado permanecerá selecionado.

As exibições do curso já estão com scaffolddas com listas suspensas para o campo departamento, mas você não quer a legenda DepartmentID para esse campo, portanto, faça a seguinte alteração realçada no arquivo *Views\Course\Create.cshtml* para alterar a legenda.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml?highlight=43)]

Faça a mesma alteração em *Views\Course\Edit.cshtml*.

Normalmente, o scaffolder não Scaffold uma chave primária porque o valor da chave é gerado pelo banco de dados e não pode ser alterado e não é um valor significativo a ser exibido aos usuários. Para entidades de curso, o scaffolder inclui uma caixa de texto para o campo `CourseID` porque ele entende que o atributo `DatabaseGeneratedOption.None` significa que o usuário deve ser capaz de inserir o valor da chave primária. Mas não entende que, como o número é significativo, você deseja vê-lo nas outras exibições, portanto, você precisa adicioná-lo manualmente.

Em *Views\Course\Edit.cshtml*, adicione um campo de número de curso antes do campo **título** . Como é a chave primária, ela é exibida, mas não pode ser alterada.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cshtml)]

Já existe um campo oculto (`Html.HiddenFor` auxiliar) para o número do curso no modo de exibição de edição. Adicionar um auxiliar *HTML. LabelFor* não elimina a necessidade do campo oculto porque ele não faz com que o número do curso seja incluído nos dados postados quando o usuário clica em **salvar** na página Editar.

Em *Views\Course\Delete.cshtml* e *Views\Course\Details.cshtml*, altere a legenda do nome do departamento de "nome" para "departamento" e adicione um campo de número do curso antes do campo **título** .

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cshtml?highlight=2,9-15)]

Execute a página **criar** (exibir a página de índice do curso e clique em **criar nova**) e insira dados para um novo curso:

| Valor | Configuração |
| ----- | ------- |
| Número | Insira *1000*. |
| Title | Digite *Algebra*. |
| Credits | Insira *4*. |
|department | Selecione **matemática**. |

Clique em **Criar**. A página de índice do curso é exibida com o novo curso adicionado à lista. O nome do departamento na lista de páginas de Índice é obtido da propriedade de navegação, mostrando que a relação foi estabelecida corretamente.

Execute a página **Editar** (exiba a página de índice do curso e clique em **Editar** em um curso).

Altere dados na página e clique em **Salvar**. A página de índice do curso é exibida com os dados do curso atualizados.

## <a name="add-office-to-instructors-page"></a>Página Adicionar o Office à instrutor

Quando você edita um registro de instrutor, deseja poder atualizar a atribuição de escritório do instrutor. A entidade `Instructor` tem uma relação um-para-zero-ou-um com a entidade `OfficeAssignment`, o que significa que você deve lidar com as seguintes situações:

- Se o usuário desmarcar a atribuição do Office e tiver originalmente um valor, você deverá remover e excluir a entidade `OfficeAssignment`.
- Se o usuário inserir um valor de atribuição do Office e ele tiver sido originalmente vazio, você deverá criar uma nova entidade de `OfficeAssignment`.
- Se o usuário alterar o valor de uma atribuição do Office, você deverá alterar o valor em uma entidade de `OfficeAssignment` existente.

Abra *InstructorController.cs* e examine o método de `Edit` `HttpGet`:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

O código com Scaffold aqui não é o que você deseja. Ele está configurando dados para uma lista suspensa, mas você precisa de uma caixa de texto. Substitua este método pelo código a seguir:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=7-10)]

Esse código descarta a instrução `ViewBag` e adiciona o carregamento adiantado para a entidade `OfficeAssignment` associada. Não é possível executar o carregamento adiantado com o método `Find`, para que os métodos `Where` e `Single` sejam usados em vez de selecionar o instrutor.

Substitua o método `HttpPost` `Edit` pelo código a seguir. que lida com atualizações de atribuição do Office:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

A referência a `RetryLimitExceededException` requer uma instrução `using`; para adicioná-lo, passe o mouse sobre `RetryLimitExceededException`. A seguinte mensagem é exibida: ![ mensagem de exceção de repetição](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)

Selecione **Mostrar possíveis correções**e, em seguida, **usando System. Data. Entity. Infrastructure**

![Resolver exceção de repetição](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

O código faz o seguinte:

- Altera o nome do método para `EditPost` porque a assinatura agora é a mesma do método `HttpGet` (o atributo `ActionName` especifica que a URL/Edit/ainda é usada).
- Obtém a entidade `Instructor` atual do banco de dados usando o carregamento adiantado para a propriedade de navegação `OfficeAssignment`. Isso é o mesmo que você fez no método de `Edit` `HttpGet`.
- Atualiza a entidade `Instructor` recuperada com valores do associador de modelos. A sobrecarga [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) usada permite que você acesse as propriedades que deseja incluir na *lista* de permissões. Isso impede o excesso de postagens, conforme explicado no [segundo tutorial](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]
- Se o local do escritório estiver em branco, o definirá a propriedade `Instructor.OfficeAssignment` como nula, de modo que a linha relacionada na tabela `OfficeAssignment` será excluída.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]
- Salva as alterações no banco de dados.

No *Views\Instructor\Edit.cshtml*, após os elementos de `div` para o campo **data de contratação** , adicione um novo campo para editar o local do escritório:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml)]

Execute a página (selecione a guia **instrutores** e clique em **Editar** em um instrutor). Altere o **Local do Escritório** e clique em **Salvar**.

## <a name="add-courses-to-instructors-page"></a>Página Adicionar cursos a instrutores

Os instrutores podem ministrar a quantidade de cursos que desejarem. Agora você aprimorará a página de edição do instrutor adicionando a capacidade de alterar as atribuições de curso usando um grupo de caixas de seleção.

A relação entre as entidades `Course` e `Instructor` é muitos para muitos, o que significa que você não tem acesso direto às propriedades de chave estrangeira que estão na tabela de junção. Em vez disso, você adiciona e remove entidades de e para a propriedade de navegação `Instructor.Courses`.

A interface do usuário que permite alterar a quais cursos um instrutor é atribuído é um grupo de caixas de seleção. Uma caixa de seleção é exibida para cada curso no banco de dados, e aqueles aos quais o instrutor está atribuído no momento são marcados. O usuário pode marcar ou desmarcar as caixas de seleção para alterar as atribuições de curso. Se o número de cursos fosse muito maior, provavelmente você desejaria usar um método diferente de apresentar os dados na exibição, mas usará o mesmo método de manipulação de propriedades de navegação para criar ou excluir relações.

Para fornecer dados à exibição para a lista de caixas de seleção, você usará uma classe de modelo de exibição. Crie *AssignedCourseData.cs* na pasta *ViewModels* e substitua o código existente pelo código a seguir:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

No *InstructorController.cs*, substitua o método `HttpGet` `Edit` pelo código a seguir. As alterações são realçadas.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs?highlight=9,12,20-35)]

O código adiciona o carregamento adiantado à propriedade de navegação `Courses` e chama o novo método `PopulateAssignedCourseData` para fornecer informações para a matriz de caixa de seleção usando a classe de modelo de exibição `AssignedCourseData`.

O código no método `PopulateAssignedCourseData` lê todas as entidades de `Course` para carregar uma lista de cursos usando a classe de modelo de exibição. Para cada curso, o código verifica se o curso existe na propriedade de navegação `Courses` do instrutor. Para criar uma pesquisa eficiente ao verificar se um curso está atribuído ao instrutor, os cursos atribuídos ao instrutor são colocados em uma coleção [HashSet](https://msdn.microsoft.com/library/bb359438.aspx) . A propriedade `Assigned` é definida como `true` para cursos que o instrutor está atribuído. A exibição usará essa propriedade para determinar quais caixas de seleção precisam ser exibidas como selecionadas. Por fim, a lista é passada para a exibição em uma propriedade `ViewBag`.

Em seguida, adicione o código que é executado quando o usuário clica em **Salvar**. Substitua o método `EditPost` pelo código a seguir, que chama um novo método que atualiza a propriedade de navegação `Courses` da entidade `Instructor`. As alterações são realçadas.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs?highlight=3,11,25,37,40-68)]

A assinatura do método agora é diferente do método `HttpGet` `Edit`, portanto, o nome do método muda de `EditPost` de volta para `Edit`.

Como a exibição não tem uma coleção de entidades de `Course`, o associador de modelo não pode atualizar automaticamente a propriedade de navegação `Courses`. Em vez de usar o associador de modelo para atualizar a propriedade de navegação `Courses`, você fará isso no novo método `UpdateInstructorCourses`. Portanto, você precisa excluir a propriedade `Courses` do model binding. Isso não requer nenhuma alteração no código que chama [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) porque você está usando a sobrecarga de *lista* de permissões e `Courses` não está na lista de inclusões.

Se nenhuma caixa de seleção tiver sido selecionada, o código em `UpdateInstructorCourses` inicializará a propriedade de navegação `Courses` com uma coleção vazia:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

Em seguida, o código executa um loop em todos os cursos no banco de dados e verifica cada curso em relação àqueles atribuídos no momento ao instrutor e em relação àqueles que foram selecionados na exibição. Para facilitar pesquisas eficientes, as últimas duas coleções são armazenadas em objetos `HashSet`.

Se a caixa de seleção para um curso foi marcada, mas o curso não está na propriedade de navegação `Instructor.Courses`, o curso é adicionado à coleção na propriedade de navegação.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Se a caixa de seleção para um curso não foi marcada, mas o curso está na propriedade de navegação `Instructor.Courses`, o curso é removido da propriedade de navegação.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

No *Views\Instructor\Edit.cshtml*, adicione um campo de **cursos** com uma matriz de caixas de seleção adicionando o código a seguir imediatamente após os elementos de `div` para o campo `OfficeAssignment` e antes do elemento `div` para o botão **salvar** :

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml)]

Depois de colar o código, se as quebras de linha e o recuo não parecerem com eles, corrija manualmente tudo para que ele se pareça com o que você vê aqui. O recuo não precisa ser perfeito, mas cada uma das linhas `@</tr><tr>`, `@:<td>`, `@:</td>` e `@</tr>` precisa estar em uma única linha, conforme mostrado, ou você receberá um erro de runtime.

Esse código cria uma tabela HTML que contém três colunas. Em cada coluna há uma caixa de seleção, seguida de uma legenda que consiste no número e título do curso. Todas as caixas de seleção têm o mesmo nome ("selectedCourses"), que informa ao associador de modelo que eles devem ser tratados como um grupo. O atributo `value` de cada caixa de seleção é definido como o valor de `CourseID.` quando a página é postada, o associador de modelo passa uma matriz para o controlador que consiste nos valores de `CourseID` apenas para as caixas de seleção que estão selecionadas.

Quando as caixas de seleção são processadas inicialmente, as que são para cursos atribuídos ao instrutor têm `checked` atributos, o que os seleciona (exibe as marcações).

Depois de alterar as atribuições de curso, você desejará poder verificar as alterações quando o site retornar à página `Index`. Portanto, você precisa adicionar uma coluna à tabela nessa página. Nesse caso, não é necessário usar o objeto `ViewBag`, porque as informações que você deseja exibir já estão na propriedade de navegação `Courses` da entidade `Instructor` que você está passando para a página como modelo.

No *Views\Instructor\Index.cshtml*, adicione um título de **cursos** imediatamente após o título do **Office** , conforme mostrado no exemplo a seguir:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml?highlight=6)]

Em seguida, adicione uma nova célula de detalhes imediatamente após a célula de detalhes do local do escritório:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml?highlight=7-14)]

Execute a página de **índice do instrutor** para ver os cursos atribuídos a cada instrutor.

Clique em **Editar** em um instrutor para ver a página Editar.

Altere algumas atribuições de curso e clique em **salvar**. As alterações feitas são refletidas na página Índice.

 Observação: a abordagem aqui usada para editar os dados do curso do instrutor funciona bem quando há um número limitado de cursos. Para coleções muito maiores, uma interface do usuário e um método de atualização diferentes são necessários.

## <a name="update-deleteconfirmed"></a>Atualizar DeleteConfirmed

No *InstructorController.cs*, exclua o método `DeleteConfirmed` e insira o código a seguir em seu lugar.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=5-8,12-18)]

Esse código faz a seguinte alteração:

- Se o instrutor for atribuído como administrador de qualquer departamento, o removerá a atribuição do instrutor desse departamento. Sem esse código, você obterá um erro de integridade referencial se tentar excluir um instrutor que foi atribuído como administrador de um departamento.

Esse código não lida com o cenário de um instrutor atribuído como administrador para vários departamentos. No último tutorial, você adicionará um código que impede que esse cenário aconteça.

## <a name="add-office-location-and-courses-to-the-create-page"></a>Adicionar local de escritório e cursos para a página Criar

No *InstructorController.cs*, exclua os métodos `HttpGet` e `HttpPost` `Create` e, em seguida, adicione o seguinte código em seu lugar:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

Esse código é semelhante ao que você viu para os métodos de edição, exceto que inicialmente nenhum curso está selecionado. O método `Create` `HttpGet` chama o método `PopulateAssignedCourseData` não porque pode haver cursos selecionados, mas para fornecer uma coleção vazia para o loop de `foreach` na exibição (caso contrário, o código de exibição geraria uma exceção de referência nula).

O método HttpPost Create adiciona cada curso selecionado à propriedade de navegação de cursos antes do código de modelo que verifica erros de validação e adiciona o novo instrutor ao banco de dados. Os cursos são adicionados mesmo se houver erros de modelo para que, quando houver erros de modelo (por exemplo, o usuário tenha inserido uma data inválida) para que, quando a página for exibida novamente com uma mensagem de erro, quaisquer seleções de curso feitas sejam automaticamente restauradas.

Observe que para poder adicionar cursos à propriedade de navegação `Courses`, é necessário inicializar a propriedade como uma coleção vazia:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

Como alternativa a fazer isso no código do controlador, faça isso no modelo Instrutor alterando o getter de propriedade para criar automaticamente a coleção se ela não existir, conforme mostrado no seguinte exemplo:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample27.cs)]

Se você modificar a propriedade `Courses` dessa forma, poderá remover o código de inicialização de propriedade explícita no controlador.

No *Views\Instructor\Create.cshtml*, adicione uma caixa de texto de localização do Office e caixas de seleção do curso após o campo data de contratação e antes do botão **Enviar** .

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample28.cshtml)]

Depois de colar o código, corrija as quebras de linha e o recuo como você fez anteriormente para a página de edição.

Execute a página criar e adicione um instrutor.

<a id="transactions"></a>

## <a name="handling-transactions"></a>Tratamento de transações

Conforme explicado no [tutorial básico de funcionalidade CRUD](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md), por padrão, o Entity Framework implementa as transações implicitamente. Para cenários em que você precisa de mais controle – por exemplo, se você quiser incluir operações feitas fora do Entity Framework em uma transação--consulte [trabalhando com transações](https://msdn.microsoft.com/data/dn456843) no msdn.

## <a name="get-the-code"></a>Obter o código

[Baixar o projeto concluído](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Recursos adicionais

Links para outros recursos de Entity Framework podem ser encontrados em [recursos de acesso a dados ASP.net-recomendado](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-step"></a>Próxima etapa

Neste tutorial, você:

> [!div class="checklist"]
> * Páginas de cursos personalizados
> * Página do Office para instrutores adicionada
> * Página de cursos adicionados aos instrutores
> * DeleteConfirmed atualizado
> * Local e cursos do Office adicionados à página criar

Avance para o próximo artigo para aprender a implementar um modelo de programação assíncrona.
> [!div class="nextstepaction"]
> [Modelo de programação assíncrona](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)
