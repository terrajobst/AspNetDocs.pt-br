---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
title: Introdução com Entity Framework 4,0 Database First e ASP.NET 4 Web Forms-parte 6 | Microsoft Docs
author: tdykstra
description: O aplicativo Web de exemplo Contoso University demonstra como criar ASP.NET Web Forms aplicativos usando o Entity Framework. O aplicativo de exemplo é...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: 994a5496-c648-4830-b03c-55bb43f325d2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
msc.type: authoredcontent
ms.openlocfilehash: 8bfbe74f90eb03ea9aab7610842ef2578e80d113
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78564221"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-6"></a>Introdução com Entity Framework 4,0 Database First e ASP.NET 4 Web Forms-parte 6

por [Tom Dykstra](https://github.com/tdykstra)

> O aplicativo Web de exemplo Contoso University demonstra como criar ASP.NET Web Forms aplicativos usando o Entity Framework 4,0 e o Visual Studio 2010. Para obter informações sobre a série de tutoriais, consulte [o primeiro tutorial da série](the-entity-framework-and-aspnet-getting-started-part-1.md)

## <a name="implementing-table-per-hierarchy-inheritance"></a>Implementando a herança de tabela por hierarquia

No tutorial anterior, você trabalhou com dados relacionados adicionando e excluindo relações e adicionando uma nova entidade que tinha uma relação com uma entidade existente. Este tutorial mostrará como implementar a herança no modelo de dados.

Na programação orientada a objeto, você pode usar a herança para facilitar o trabalho com classes relacionadas. Por exemplo, você pode criar `Instructor` e `Student` classes que derivam de uma classe base `Person`. Você pode criar os mesmos tipos de estruturas de herança entre entidades no Entity Framework.

Nesta parte do tutorial, você não criará nenhuma nova página da Web. Em vez disso, você adicionará entidades derivadas ao modelo de dados e modificará as páginas existentes para usar as novas entidades.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>Herança de tabela por hierarquia versus de tabela por tipo

Um banco de dados pode armazenar informações sobre objetos relacionados em uma tabela ou em várias tabelas. Por exemplo, no banco de dados `School`, a tabela `Person` inclui informações sobre alunos e instrutores em uma única tabela. Algumas das colunas aplicam-se somente aos instrutores (`HireDate`), alguns somente aos alunos (`EnrollmentDate`) e outros (`LastName``FirstName`).

[![image11](the-entity-framework-and-aspnet-getting-started-part-6/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image1.png)

Você pode configurar o Entity Framework para criar `Instructor` e `Student` entidades que herdam da entidade `Person`. Esse padrão de gerar uma estrutura de herança de entidade a partir de uma única tabela de banco de dados é chamada de herança de *tabela por hierarquia* (TPH).

Para cursos, o banco de dados `School` usa um padrão diferente. Cursos online e cursos no local são armazenados em tabelas separadas, cada uma com uma chave estrangeira que aponta para a tabela de `Course`. As informações comuns a ambos os tipos de curso são armazenadas apenas na tabela `Course`.

[![IMAGE12](the-entity-framework-and-aspnet-getting-started-part-6/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image3.png)

Você pode configurar o modelo de dados Entity Framework para que as entidades `OnlineCourse` e `OnsiteCourse` herdam da entidade `Course`. Esse padrão de gerar uma estrutura de herança de entidade a partir de tabelas separadas para cada tipo, com cada tabela separada referindo-se a uma tabela que armazena dados comuns a todos os tipos, é chamada de herança de *tabela por tipo* (TPT).

Os padrões de herança TPH geralmente oferecem melhor desempenho no Entity Framework do que os padrões de herança do TPT, pois os padrões de TPT podem resultar em consultas de junção complexas. Este tutorial demonstra como implementar a herança TPH. Você fará isso executando as seguintes etapas:

- Crie `Instructor` e `Student` tipos de entidade que derivam de `Person`.
- Mova as propriedades que pertencem às entidades derivadas da entidade `Person` para as entidades derivadas.
- Defina restrições em Propriedades nos tipos derivados.
- Torne a entidade de `Person` uma entidade abstrata.
- Mapeie cada entidade derivada para a tabela de `Person` com uma condição que especifica como determinar se uma linha de `Person` representa o tipo derivado.

## <a name="adding-instructor-and-student-entities"></a>Adicionando entidades do instrutor e Student

Abra o arquivo <em>SchoolModel. edmx</em> , clique com o botão direito do mouse em uma área desocupada no designer, selecione <strong>Adicionar</strong>e, em seguida, selecione <strong>entidade</strong><em>.</em>

[![image01](the-entity-framework-and-aspnet-getting-started-part-6/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image5.png)

Na caixa de diálogo **Adicionar entidade** , nomeie a entidade `Instructor` e defina sua opção de **tipo base** como `Person`.

[![image02](the-entity-framework-and-aspnet-getting-started-part-6/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image7.png)

Clique em **OK**. O designer cria uma entidade `Instructor` que deriva da entidade `Person`. A nova entidade ainda não tem nenhuma propriedade.

[![image03](the-entity-framework-and-aspnet-getting-started-part-6/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image9.png)

Repita o procedimento para criar uma entidade de `Student` que também deriva de `Person`.

Somente instrutores têm datas de contratação, portanto, você precisa mover essa propriedade da entidade `Person` para a entidade `Instructor`. Na entidade `Person`, clique com o botão direito do mouse na propriedade `HireDate` e clique em **recortar**. Em seguida, clique com o botão direito do mouse em **Propriedades** na entidade `Instructor` e clique em **colar**.

[![image04](the-entity-framework-and-aspnet-getting-started-part-6/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image11.png)

A data de contratação de uma entidade de `Instructor` não pode ser nula. Clique com o botão direito do mouse na propriedade `HireDate`, clique em **Propriedades**e, na janela **propriedades** , altere `Nullable` para `False`.

[![image05](the-entity-framework-and-aspnet-getting-started-part-6/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image13.png)

Repita o procedimento para mover a propriedade `EnrollmentDate` da entidade `Person` para a entidade `Student`. Certifique-se de também definir `Nullable` como `False` para a propriedade `EnrollmentDate`.

Agora que uma entidade `Person` tem apenas as propriedades que são comuns a `Instructor` e `Student` entidades (além das propriedades de navegação, que você não está movendo), a entidade só pode ser usada como uma entidade base na estrutura de herança. Portanto, você precisa garantir que nunca seja tratado como uma entidade independente. Clique com o botão direito do mouse na entidade `Person`, selecione **Propriedades**e, na janela **Propriedades** , altere o valor da propriedade **abstract** para **true**.

[![image06](the-entity-framework-and-aspnet-getting-started-part-6/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image15.png)

## <a name="mapping-instructor-and-student-entities-to-the-person-table"></a>Mapeando entidades do instrutor e aluno para a tabela Person

Agora você precisa informar ao Entity Framework como diferenciar entre `Instructor` e `Student` entidades no banco de dados.

Clique com o botão direito do mouse na entidade `Instructor` e selecione **mapeamento de tabela**. Na janela **detalhes do mapeamento** , clique em **Adicionar uma tabela ou exibição** e selecione **pessoa**.

[![image07](the-entity-framework-and-aspnet-getting-started-part-6/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image17.png)

Clique em **Adicionar uma condição**e, em seguida, selecione **HireDate**.

[![image09](the-entity-framework-and-aspnet-getting-started-part-6/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image19.png)

Altere o **operador** para **is** e **Value/Property** para **NOT NULL**.

[![image10](the-entity-framework-and-aspnet-getting-started-part-6/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image21.png)

Repita o procedimento para a entidade `Students`, especificando que essa entidade mapeia para a tabela `Person` quando a coluna `EnrollmentDate` não é nula. Em seguida, salve e feche o modelo de dados.

Compile o projeto para criar as novas entidades como classes e torná-las disponíveis no designer.

## <a name="using-the-instructor-and-student-entities"></a>Usando as entidades do instrutor e do aluno

Quando você criou as páginas da Web que funcionam com os dados do aluno e do instrutor, você os vinculados ao conjunto de entidades de `Person` e filtrados na propriedade `HireDate` ou `EnrollmentDate` para restringir os dados retornados aos alunos ou instrutores. No entanto, agora, ao associar cada controle da fonte de dados ao conjunto de entidades `Person`, você pode especificar que somente `Student` ou `Instructor` tipos de entidade devem ser selecionados. Como o Entity Framework sabe como diferenciar alunos e instrutores no conjunto de entidades de `Person`, você pode remover as configurações de propriedade de `Where` que você inseriu manualmente para fazer isso.

No designer do Visual Studio, você pode especificar o tipo de entidade que um controle de `EntityDataSource` deve selecionar na caixa suspensa **EntityTypeFilter** do assistente de `Configure Data Source`, conforme mostrado no exemplo a seguir.

[![image13](the-entity-framework-and-aspnet-getting-started-part-6/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image23.png)

E, na janela **Propriedades** , você pode remover `Where` valores de cláusula que não são mais necessários, conforme mostrado no exemplo a seguir.

[![image14](the-entity-framework-and-aspnet-getting-started-part-6/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image25.png)

No entanto, como você alterou a marcação para `EntityDataSource` controles para usar o atributo `ContextTypeName`, não é possível executar o assistente para **Configurar fonte de dados** em controles de `EntityDataSource` que você já criou. Portanto, você fará as alterações necessárias alterando a marcação em vez disso.

Abra a página *students. aspx* . No controle `StudentsEntityDataSource`, remova o atributo `Where` e adicione um atributo `EntityTypeFilter="Student"`. A marcação agora será semelhante ao exemplo a seguir:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample1.aspx)]

Definir o atributo `EntityTypeFilter` garante que o controle de `EntityDataSource` selecionará apenas o tipo de entidade especificado. Se você quisesse recuperar os tipos de entidade `Student` e `Instructor`, você não definiria esse atributo. (Você tem a opção de recuperar vários tipos de entidade com apenas um controle `EntityDataSource` se estiver usando o controle para acesso a dados somente leitura. Se você estiver usando um controle de `EntityDataSource` para inserir, atualizar ou excluir entidades e se o conjunto de entidades a que está associado puder conter vários tipos, você só poderá trabalhar com um tipo de entidade e precisará definir esse atributo.)

Repita o procedimento para o controle `SearchEntityDataSource`, exceto remover apenas a parte do atributo `Where` que seleciona entidades `Student` em vez de remover a propriedade completamente. A marca de abertura do controle agora será semelhante ao exemplo a seguir:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample2.aspx)]

Execute a página para verificar se ela ainda funciona como antes.

[![image15](the-entity-framework-and-aspnet-getting-started-part-6/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image27.png)

Atualize as páginas a seguir que você criou nos tutoriais anteriores para que eles usem as novas entidades `Student` e `Instructor` em vez de `Person` entidades e, em seguida, execute-as para verificar se elas funcionam como faziam antes:

- Em *StudentsAdd. aspx*, adicione `EntityTypeFilter="Student"` ao controle de `StudentsEntityDataSource`. A marcação agora será semelhante ao exemplo a seguir: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample3.aspx)]

    [![image16](the-entity-framework-and-aspnet-getting-started-part-6/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image29.png)
- Em *about. aspx*, adicione `EntityTypeFilter="Student"` ao controle de `StudentStatisticsEntityDataSource` e remova `Where="it.EnrollmentDate is not null"`. A marcação agora será semelhante ao exemplo a seguir: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample4.aspx)]

    [![image17](the-entity-framework-and-aspnet-getting-started-part-6/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image31.png)
- Em *instrutores. aspx* e *InstructorsCourses. aspx*, adicione `EntityTypeFilter="Instructor"` ao controle de `InstructorsEntityDataSource` e remova `Where="it.HireDate is not null"`. A marcação em *instrutores. aspx* agora é semelhante ao exemplo a seguir: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample5.aspx)]

    [![image18](the-entity-framework-and-aspnet-getting-started-part-6/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image33.png)

    A marcação em *InstructorsCourses. aspx* agora se parecerá com o exemplo a seguir:

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample6.aspx)]

    [![image19](the-entity-framework-and-aspnet-getting-started-part-6/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image35.png)

Como resultado dessas alterações, você melhorou a manutenção do aplicativo da Contoso University de várias maneiras. Você moveu a seleção e a lógica de validação da camada da interface do usuário (marcação *. aspx* ) e fez com que ela fosse parte integrante da camada de acesso a dados. Isso ajuda a isolar o código do aplicativo de alterações que você pode fazer no futuro no esquema de banco de dados ou no modelo de dado. Por exemplo, você pode decidir que os alunos podem ser contratados como auxílios dos professores e, portanto, obterão uma data de contratação. Você poderia então adicionar uma nova propriedade para diferenciar os alunos dos instrutores e atualizar o modelo de dados. Nenhum código no aplicativo Web precisaria ser alterado, exceto onde você quisesse mostrar uma data de contratação para os alunos. Outro benefício de adicionar entidades de `Instructor` e `Student` é que seu código é mais compreensível do que quando se trata de `Person` objetos que, na verdade, são estudantes ou instrutores.

Agora você viu uma maneira de implementar um padrão de herança no Entity Framework. No tutorial a seguir, você aprenderá a usar procedimentos armazenados para ter mais controle sobre como o Entity Framework acessa o banco de dados.

> [!div class="step-by-step"]
> [Anterior](the-entity-framework-and-aspnet-getting-started-part-5.md)
> [Próximo](the-entity-framework-and-aspnet-getting-started-part-7.md)
