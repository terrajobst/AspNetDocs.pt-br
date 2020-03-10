---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
title: Introdução com Entity Framework 4,0 Database First e ASP.NET 4 Web Forms-parte 5 | Microsoft Docs
author: tdykstra
description: O aplicativo Web de exemplo Contoso University demonstra como criar ASP.NET Web Forms aplicativos usando o Entity Framework. O aplicativo de exemplo é...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: 24ad4379-3fb2-44dc-ba59-85fe0ffcb2ae
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
msc.type: authoredcontent
ms.openlocfilehash: 75328e67abb4295b619cac5423a9eb970942fff7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78527996"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-5"></a>Introdução com Entity Framework 4,0 Database First e ASP.NET 4 Web Forms-parte 5

por [Tom Dykstra](https://github.com/tdykstra)

> O aplicativo Web de exemplo Contoso University demonstra como criar ASP.NET Web Forms aplicativos usando o Entity Framework 4,0 e o Visual Studio 2010. Para obter informações sobre a série de tutoriais, consulte [o primeiro tutorial da série](the-entity-framework-and-aspnet-getting-started-part-1.md)

## <a name="working-with-related-data-continued"></a>Trabalhando com dados relacionados, continuação

No tutorial anterior, você começou a usar o controle de `EntityDataSource` para trabalhar com dados relacionados. Você exibiu vários níveis de hierarquia e os dados editados nas propriedades de navegação. Neste tutorial, você continuará a trabalhar com os dados relacionados adicionando e excluindo relações e adicionando uma nova entidade que tem uma relação com uma entidade existente.

Você criará uma página que adiciona cursos atribuídos aos departamentos. Os departamentos já existem e, ao criar um novo curso, ao mesmo tempo você estabelecerá uma relação entre ele e um departamento existente.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image1.png)

Você também criará uma página que funciona com uma relação muitos para muitos atribuindo um instrutor a um curso (adicionando uma relação entre duas entidades que você selecionar) ou removendo um instrutor de um curso (removendo uma relação entre duas entidades que você Selecione). No banco de dados, a adição de uma relação entre um instrutor e um curso resulta em uma nova linha adicionada à tabela de associação de `CourseInstructor`; a remoção de uma relação envolve a exclusão de uma linha da tabela de associação `CourseInstructor`. No entanto, você faz isso no Entity Framework definindo propriedades de navegação, sem se referir à tabela de `CourseInstructor` explicitamente.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image3.png)

## <a name="adding-an-entity-with-a-relationship-to-an-existing-entity"></a>Adicionando uma entidade com uma relação a uma entidade existente

Crie uma nova página da Web chamada *CoursesAdd. aspx* que usa a página mestra *site. Master* e adicione a marcação a seguir ao controle de `Content` chamado `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample1.aspx)]

Essa marcação cria um controle de `EntityDataSource` que seleciona cursos, que habilita a inserção e que especifica um manipulador para o evento `Inserting`. Você usará o manipulador para atualizar a propriedade de navegação `Department` quando uma nova entidade de `Course` for criada.

A marcação também cria um controle de `DetailsView` a ser usado para adicionar novas entidades de `Course`. A marcação usa campos associados para propriedades `Course` entidade. Você precisa inserir o valor de `CourseID` porque este não é um campo de ID gerado pelo sistema. Em vez disso, é um número de curso que deve ser especificado manualmente quando o curso é criado.

Você usa um campo de modelo para a propriedade de navegação `Department` porque as propriedades de navegação não podem ser usadas com controles `BoundField`. O campo modelo fornece uma lista suspensa para selecionar o departamento. A lista suspensa é associada à entidade `Departments` definida usando `Eval` em vez de `Bind`, novamente porque você não pode associar diretamente as propriedades de navegação para atualizá-las. Você especifica um manipulador para o evento de `Init` do controle de `DropDownList` para que você possa armazenar uma referência ao controle para uso pelo código que atualiza a chave estrangeira `DepartmentID`.

No *CoursesAdd.aspx.cs* logo após a declaração de classe parcial, adicione um campo de classe para manter uma referência ao controle de `DepartmentsDropDownList`:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample2.cs)]

Adicione um manipulador para o evento de `Init` do controle de `DepartmentsDropDownList` para que você possa armazenar uma referência ao controle. Isso permite obter o valor que o usuário inseriu e usá-lo para atualizar o `DepartmentID` valor da entidade `Course`.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample3.cs)]

Adicione um manipulador para o evento de `Inserting` do controle de `DetailsView`:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample4.cs)]

Quando o usuário clica em `Insert`, o evento `Inserting` é gerado antes que o novo registro seja inserido. O código no manipulador obtém a `DepartmentID` do controle de `DropDownList` e a usa para definir o valor que será usado para a propriedade `DepartmentID` da entidade `Course`.

O Entity Framework se encarregará de adicionar esse curso à propriedade de navegação `Courses` da entidade `Department` associada. Ele também adiciona o departamento à propriedade de navegação `Department` da entidade `Course`.

Execute a página.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image5.png)

Insira uma ID, um título, um número de créditos e selecione um departamento e, em seguida, clique em **Inserir**.

Execute a página *cursos. aspx* e selecione o mesmo departamento para ver o novo curso.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-5/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image7.png)

## <a name="working-with-many-to-many-relationships"></a>Trabalhando com relações muitos para muitos

A relação entre o conjunto de entidades de `Courses` e o conjunto de entidades de `People` é uma relação muitos para muitos. Uma entidade `Course` tem uma propriedade de navegação chamada `People` que pode conter zero, uma ou mais entidades de `Person` relacionadas (representando os instrutores atribuídos para ensinar esse curso). E uma entidade `Person` tem uma propriedade de navegação chamada `Courses` que pode conter zero, uma ou mais entidades de `Course` relacionadas (representando cursos que o instrutor está atribuído a ensinar). Um instrutor pode ensinar vários cursos, e um curso pode ser ensinado por vários instrutores. Nesta seção, você adicionará e removerá relações entre `Person` e `Course` entidades atualizando as propriedades de navegação das entidades relacionadas.

Crie uma nova página da Web chamada *InstructorsCourses. aspx* que usa a página mestra *site. Master* e adicione a marcação a seguir ao controle de `Content` chamado `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample5.aspx)]

Essa marcação cria um controle de `EntityDataSource` que recupera o nome e `PersonID` de entidades de `Person` para instrutores. Um controle de `DropDrownList` está associado ao controle de `EntityDataSource`. O controle `DropDownList` especifica um manipulador para o evento `DataBound`. Você usará esse manipulador para associar as duas listas suspensas que exibem cursos.

A marcação também cria o seguinte grupo de controles a ser usado para atribuir um curso ao instrutor selecionado:

- Um controle de `DropDownList` para selecionar um curso a ser atribuído. Esse controle será preenchido com cursos que atualmente não estão atribuídos ao instrutor selecionado.
- Um controle de `Button` para iniciar a atribuição.
- Um controle `Label` para exibir uma mensagem de erro se a atribuição falhar.

Por fim, a marcação também cria um grupo de controles a ser usado para remover um curso do instrutor selecionado.

Em *InstructorsCourses.aspx.cs*, adicione uma instrução Using:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample6.cs)]

Adicione um método para preencher as duas listas suspensas que exibem cursos:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample7.cs)]

Esse código obtém todos os cursos da `Courses` conjunto de entidades e obtém os cursos da propriedade de navegação `Courses` da entidade `Person` para o instrutor selecionado. Em seguida, ele determina quais cursos são atribuídos a esse instrutor e preenche as listas suspensas de acordo.

Adicione um manipulador para o evento de `Click` do botão de `Assign`:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample8.cs)]

Esse código obtém a entidade `Person` para o instrutor selecionado, obtém a entidade `Course` do curso selecionado e adiciona o curso selecionado à propriedade de navegação `Courses` da entidade `Person` do instrutor. Em seguida, ele salva as alterações no banco de dados e preenche novamente as listas suspensas para que os resultados possam ser vistos imediatamente.

Adicione um manipulador para o evento de `Click` do botão de `Remove`:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample9.cs)]

Esse código obtém a entidade `Person` para o instrutor selecionado, obtém a entidade `Course` do curso selecionado e remove o curso selecionado da propriedade de navegação `Courses` da entidade `Person`. Em seguida, ele salva as alterações no banco de dados e preenche novamente as listas suspensas para que os resultados possam ser vistos imediatamente.

Adicione código ao método de `Page_Load` que verifica se as mensagens de erro não estão visíveis quando não há erro de relatório e adicione manipuladores para os `DataBound` e `SelectedIndexChanged` eventos da lista suspensa de instrutores para preencher as listas suspensas de cursos:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample10.cs)]

Execute a página.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image9.png)

Selecione um instrutor. A lista suspensa <strong>atribuir um curso</strong> exibe os cursos que o instrutor não ensina, e a lista suspensa <strong>remover um curso</strong> exibe os cursos aos quais o instrutor já está atribuído. Na seção <strong>atribuir um curso</strong> , selecione um curso e clique em <strong>atribuir</strong>. O curso se move para a lista suspensa <strong>remover um curso</strong> . Selecione um curso na seção <strong>remover um curso</strong> e clique em <strong>remover</strong><em>.</em> O curso se move para a lista suspensa <strong>atribuir um curso</strong> .

Agora você viu algumas outras maneiras de trabalhar com dados relacionados. No tutorial a seguir, você aprenderá a usar a herança no modelo de dados para melhorar a manutenção do seu aplicativo.

> [!div class="step-by-step"]
> [Anterior](the-entity-framework-and-aspnet-getting-started-part-4.md)
> [Próximo](the-entity-framework-and-aspnet-getting-started-part-6.md)
