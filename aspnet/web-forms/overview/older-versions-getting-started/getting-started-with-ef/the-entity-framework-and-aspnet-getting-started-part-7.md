---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
title: Introdução com Entity Framework 4,0 Database First e ASP.NET 4 Web Forms-parte 7 | Microsoft Docs
author: tdykstra
description: O aplicativo Web de exemplo Contoso University demonstra como criar ASP.NET Web Forms aplicativos usando o Entity Framework. O aplicativo de exemplo é...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: f8afb245-b705-419c-8790-0b295e90d5e2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
msc.type: authoredcontent
ms.openlocfilehash: 18d4b44c5e23fd6942c3adf48a33a5602e6df6d0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78603428"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-7"></a>Introdução com Entity Framework 4,0 Database First e ASP.NET 4 Web Forms-parte 7

por [Tom Dykstra](https://github.com/tdykstra)

> O aplicativo Web de exemplo Contoso University demonstra como criar ASP.NET Web Forms aplicativos usando o Entity Framework 4,0 e o Visual Studio 2010. Para obter informações sobre a série de tutoriais, consulte [o primeiro tutorial da série](the-entity-framework-and-aspnet-getting-started-part-1.md)

## <a name="using-stored-procedures"></a>Usando procedimentos armazenados

No tutorial anterior, você implementou um padrão de herança de tabela por hierarquia. Este tutorial mostrará como usar procedimentos armazenados para obter mais controle sobre o acesso ao banco de dados.

O Entity Framework permite que você especifique que ele deve usar procedimentos armazenados para acesso ao banco de dados. Para qualquer tipo de entidade, você pode especificar um procedimento armazenado a ser usado para criar, atualizar ou excluir entidades desse tipo. Em seguida, no modelo de dados, você pode adicionar referências a procedimentos armazenados que você pode usar para executar tarefas como a recuperação de conjuntos de entidades.

O uso de procedimentos armazenados é um requisito comum para o acesso ao banco de dados. Em alguns casos, um administrador de banco de dados pode exigir que todo o acesso ao banco de dados passe pelos procedimentos armazenados por motivos de segurança. Em outros casos, talvez você queira criar lógica de negócios em alguns dos processos que o Entity Framework usa ao atualizar o banco de dados. Por exemplo, sempre que uma entidade for excluída, talvez você queira copiá-la para um banco de dados de arquivamento. Ou sempre que uma linha for atualizada, talvez você queira gravar uma linha em uma tabela de log que registra quem fez a alteração. Você pode executar esses tipos de tarefas em um procedimento armazenado que é chamado sempre que o Entity Framework exclui uma entidade ou atualiza uma entidade.

Como no tutorial anterior, você não criará nenhuma nova página. Em vez disso, você alterará a maneira como o Entity Framework acessa o banco de dados para algumas das páginas que você já criou.

Neste tutorial, você criará procedimentos armazenados no banco de dados para inserir `Student` e `Instructor` entidades. Você os adicionará ao modelo de dados e especificará que o Entity Framework deve usá-los para adicionar `Student` e `Instructor` entidades ao banco de dados. Você também criará um procedimento armazenado que pode ser usado para recuperar `Course` entidades.

## <a name="creating-stored-procedures-in-the-database"></a>Criando procedimentos armazenados no banco de dados

(Se você estiver usando o arquivo *School. MDF* do projeto disponível para download com este tutorial, poderá ignorar esta seção porque os procedimentos armazenados já existem.)

Em **Gerenciador de servidores**, expanda *School. MDF*, clique com o botão direito do mouse em **procedimentos armazenados**e selecione **Adicionar novo procedimento armazenado**.

[![image15](the-entity-framework-and-aspnet-getting-started-part-7/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image1.png)

Copie as seguintes instruções SQL e cole-as na janela procedimento armazenado, substituindo o esqueleto do procedimento armazenado.

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample1.sql)]

[![image14](the-entity-framework-and-aspnet-getting-started-part-7/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image3.png)

`Student` entidades têm quatro propriedades: `PersonID`, `LastName`, `FirstName`e `EnrollmentDate`. O banco de dados gera o valor de ID automaticamente e o procedimento armazenado aceita parâmetros para os outros três. O procedimento armazenado retorna o valor da chave de registro da nova linha para que o Entity Framework possa controlar isso na versão da entidade que ela mantém na memória.

Salve e feche a janela procedimento armazenado.

Crie um procedimento armazenado `InsertInstructor` da mesma maneira, usando as seguintes instruções SQL:

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample2.sql)]

Crie `Update` procedimentos armazenados para `Student` e `Instructor` entidades também. (O banco de dados já tem um procedimento armazenado `DeletePerson` que funcionará para `Instructor` e `Student` entidades.)

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample3.sql)]

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample4.sql)]

Neste tutorial, você mapeará todas as três funções – inserir, atualizar e excluir--para cada tipo de entidade. O Entity Framework versão 4 permite mapear apenas uma ou duas dessas funções para procedimentos armazenados sem mapear os outros, com uma exceção: se você mapear a função Update, mas não a função Delete, o Entity Framework gerará uma exceção quando você tentativa de excluir uma entidade. Na versão Entity Framework 3,5, você não tinha muito flexibilidade no mapeamento de procedimentos armazenados: se você mapeou uma função, era necessário mapear todas as três.

Para criar um procedimento armazenado que leia em vez de atualizar os dados, crie um que selecione todas as entidades de `Course`, usando as seguintes instruções SQL:

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample5.sql)]

## <a name="adding-the-stored-procedures-to-the-data-model"></a>Adicionando os procedimentos armazenados ao modelo de dados

Os procedimentos armazenados agora são definidos no banco de dados, mas eles devem ser adicionados ao modelo de dado para disponibilizá-los para o Entity Framework. Abra *SchoolModel. edmx*, clique com o botão direito do mouse na superfície de design e selecione **atualizar modelo do banco de dados**. Na guia **Adicionar** da caixa de diálogo **escolher seu objeto de banco de dados** , expanda **procedimentos armazenados**, selecione os procedimentos armazenados recém-criados e o procedimento armazenado `DeletePerson` e clique em **concluir**.

[![image20](the-entity-framework-and-aspnet-getting-started-part-7/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image5.png)

## <a name="mapping-the-stored-procedures"></a>Mapeando os procedimentos armazenados

No designer de modelo de dados, clique com o botão direito do mouse na entidade `Student` e selecione **mapeamento de procedimento armazenado**.

[![image21](the-entity-framework-and-aspnet-getting-started-part-7/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image7.png)

A janela **detalhes do mapeamento** é exibida, na qual você pode especificar procedimentos armazenados que o Entity Framework deve usar para inserir, atualizar e excluir entidades desse tipo.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-7/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image9.png)

Defina a função **Insert** como **InsertStudent**. A janela mostra uma lista de parâmetros de procedimento armazenado, que devem ser mapeados para uma propriedade de entidade. Dois deles são mapeados automaticamente porque os nomes são os mesmos. Não há nenhuma propriedade de entidade chamada `FirstName`, portanto, você deve selecionar manualmente `FirstMidName` em uma lista suspensa que mostra as propriedades de entidade disponíveis. (Isso ocorre porque você alterou o nome da propriedade `FirstName` para `FirstMidName` no primeiro tutorial.)

[![image23](the-entity-framework-and-aspnet-getting-started-part-7/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image11.png)

Na mesma janela de **detalhes de mapeamento** , mapeie a função `Update` para o procedimento armazenado `UpdateStudent` (certifique-se de especificar `FirstMidName` como o valor do parâmetro para `FirstName`, como fez para o procedimento armazenado `Insert`) e a função `Delete` para o procedimento armazenado `DeletePerson`.

[![image01](the-entity-framework-and-aspnet-getting-started-part-7/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image13.png)

Siga o mesmo procedimento para mapear os procedimentos armazenados de inserção, atualização e exclusão dos instrutores para a entidade `Instructor`.

[![image02](the-entity-framework-and-aspnet-getting-started-part-7/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image15.png)

Para procedimentos armazenados que lêem, em vez de dados de atualização, você usa a janela do **navegador de modelos** para mapear o procedimento armazenado para o tipo de entidade que ele retorna. No designer de modelo de dados, clique com o botão direito do mouse na superfície de design e selecione **navegador de modelos**. Abra o nó **SchoolModel. Store** e abra o nó **procedimentos armazenados** . Em seguida, clique com o botão direito do mouse no procedimento armazenado `GetCourses` e selecione **Adicionar função importar**.

[![image24](the-entity-framework-and-aspnet-getting-started-part-7/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image17.png)

Na caixa de diálogo **Adicionar importação de função** , em **retorna uma coleção de** **entidades**Select e, em seguida, selecione `Course` como o tipo de entidade retornado. Quando terminar, clique em **OK**. Salve e feche o arquivo *. edmx* .

[![image25](the-entity-framework-and-aspnet-getting-started-part-7/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image19.png)

## <a name="using-insert-update-and-delete-stored-procedures"></a>Usando os procedimentos armazenados INSERT, Update e Delete

Os procedimentos armazenados para inserir, atualizar e excluir dados são usados pelo Entity Framework automaticamente depois de você adicioná-los ao modelo de dados e mapeá-los para as entidades apropriadas. Agora você pode executar a página *StudentsAdd. aspx* e, toda vez que criar um novo aluno, o Entity Framework usará o procedimento armazenado `InsertStudent` para adicionar a nova linha à tabela `Student`.

[![image03](the-entity-framework-and-aspnet-getting-started-part-7/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image21.png)

Execute a página *students. aspx* e o novo aluno aparecerá na lista.

[![image04](the-entity-framework-and-aspnet-getting-started-part-7/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image23.png)

Altere o nome para verificar se a função Update funciona e, em seguida, exclua o aluno para verificar se a função delete funciona.

[![image05](the-entity-framework-and-aspnet-getting-started-part-7/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image25.png)

## <a name="using-select-stored-procedures"></a>Usando os procedimentos armazenados Select

O Entity Framework não executa automaticamente procedimentos armazenados, como `GetCourses`, e você não pode usá-los com o controle de `EntityDataSource`. Para usá-los, você deve chamá-los do código.

Abra o arquivo *InstructorsCourses.aspx.cs* . O método `PopulateDropDownLists` usa uma consulta LINQ-to-Entities para recuperar todas as entidades de curso para que ele possa executar um loop na lista e determinar a quais um instrutor está atribuído e quais deles não são atribuídos:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample6.cs)]

Substituir pelo seguinte código:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample7.cs)]

Agora, a página usa o procedimento armazenado `GetCourses` para recuperar a lista de todos os cursos. Execute a página para verificar se ela funciona como antes.

(As propriedades de navegação de entidades recuperadas por um procedimento armazenado podem não ser preenchidas automaticamente com os dados relacionados a essas entidades, dependendo das configurações padrão do `ObjectContext`. Para obter mais informações, consulte [carregando objetos relacionados](https://msdn.microsoft.com/library/bb896272.aspx) na biblioteca MSDN.)

No próximo tutorial, você aprenderá a usar Dados Dinâmicos funcionalidade para tornar mais fácil programar e testar regras de validação e formatação de dados. Em vez de especificar em cada regra de página da Web, como cadeias de caracteres de formato de dados e se um campo é necessário, você pode especificar essas regras em metadados de modelo de dados e elas são aplicadas automaticamente em cada página.

> [!div class="step-by-step"]
> [Anterior](the-entity-framework-and-aspnet-getting-started-part-6.md)
> [Próximo](the-entity-framework-and-aspnet-getting-started-part-8.md)
