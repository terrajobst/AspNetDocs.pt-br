---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Tutorial: implementar a herança com o EF em um aplicativo ASP.NET MVC 5'
description: Este tutorial mostrará como implementar a herança no modelo de dados.
author: tdykstra
ms.author: riande
ms.date: 01/21/2019
ms.topic: tutorial
ms.assetid: 08834147-77ec-454a-bb7a-d931d2a40dab
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 73a01ed47b0935a1a9734c197377470defb1fe36
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519382"
---
# <a name="tutorial-implement-inheritance-with-ef-in-an-aspnet-mvc-5-app"></a>Tutorial: implementar a herança com o EF em um aplicativo MVC 5 do ASP.NET

No tutorial anterior, você tratou exceções de simultaneidade. Este tutorial mostrará como implementar a herança no modelo de dados.

Na programação orientada a objeto, você pode usar a [herança](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) para facilitar a [reutilização de código](http://en.wikipedia.org/wiki/Code_reuse). Neste tutorial, você alterará as classes `Instructor` e `Student`, de modo que elas derivem de uma classe base `Person` que contém propriedades, como `LastName`, comuns a instrutores e alunos. Você não adicionará nem alterará as páginas da Web, mas alterará uma parte do código, e essas alterações serão refletidas automaticamente no banco de dados.

Neste tutorial, você:

> [!div class="checklist"]
> * Saiba como mapear a herança para o banco de dados
> * Criar a classe Person
> * Atualizará Instructor e Student
> * Adicionar pessoa ao modelo
> * Criar e atualizar migrações
> * Testará a implementação
> * Implantar no Azure

## <a name="prerequisites"></a>{1&gt;{2&gt;Pré-requisitos&lt;2}&lt;1}

* [Tratamento de simultaneidade](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="map-inheritance-to-database"></a>Mapeará a herança para o banco de dados

As classes `Instructor` e `Student` no modelo de dados `School` têm várias propriedades idênticas:

![Student_and_Instructor_classes](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

Suponha que você deseje eliminar o código redundante para as propriedades compartilhadas pelas entidades `Instructor` e `Student`. Ou você deseja escrever um serviço que pode formatar nomes sem se preocupar se o nome foi obtido de um instrutor ou um aluno. Você pode criar uma classe base `Person` que contém apenas essas propriedades compartilhadas e, em seguida, fazer com que as entidades `Instructor` e `Student` herdem dessa classe base, conforme mostrado na ilustração a seguir:

![Student_and_Instructor_classes_deriving_from_Person_class](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Há várias maneiras pelas quais essa estrutura de herança pode ser representada no banco de dados. Você pode ter uma tabela `Person` que inclui informações sobre alunos e instrutores em uma única tabela. Algumas das colunas podem se aplicar somente a instrutores (`HireDate`), apenas a alunos (`EnrollmentDate`), algumas para ambos (`LastName``FirstName`). Normalmente, você teria uma coluna *discriminadora* para indicar qual tipo cada linha representa. Por exemplo, a coluna discriminatória pode ter "Instrutor" para instrutores e "Aluno" para alunos.

![Table-per-hierarchy_example](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Esse padrão de gerar uma estrutura de herança de entidade a partir de uma única tabela de banco de dados é chamada de herança de *tabela por hierarquia* (TPH).

Uma alternativa é fazer com que o banco de dados se pareça mais com a estrutura de herança. Por exemplo, você pode ter apenas os campos nome na tabela `Person` e ter tabelas `Instructor` e `Student` separadas com os campos de data.

![Tabela por type_inheritance](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Esse padrão de fazer uma tabela de banco de dados para cada classe de entidade é chamado de herança de *tabela por tipo* (TPT).

Outra opção é mapear todos os tipos não abstratos para tabelas individuais. Todas as propriedades de uma classe, incluindo propriedades herdadas, são mapeadas para colunas da tabela correspondente. Esse padrão é chamado de herança TPC (Tabela por Classe Concreta). Se você implementou a herança do TPC para as classes `Person`, `Student`e `Instructor`, conforme mostrado anteriormente, as tabelas `Student` e `Instructor` não serão diferentes depois de implementar a herança do que antes.

Os padrões de herança do TPC e do TPH geralmente oferecem melhor desempenho no Entity Framework que os padrões de herança do TPT, pois os padrões de TPT podem resultar em consultas de junção complexas.

Este tutorial demonstra como implementar a herança TPH. TPH é o padrão de herança padrão no Entity Framework, portanto, tudo o que você precisa fazer é criar uma classe `Person`, alterar as classes `Instructor` e `Student` para derivar de `Person`, adicionar a nova classe à `DbContext`e criar uma migração. (Para obter informações sobre como implementar os outros padrões de herança, consulte [mapeando a herança de tabela por tipo (TPT)](https://msdn.microsoft.com/data/jj591617#2.5) e [mapeando a herança da classe de tabela por concreto (TPC)](https://msdn.microsoft.com/data/jj591617#2.6) na documentação do MSDN Entity Framework.)

## <a name="create-the-person-class"></a>Criar a classe Person

Na pasta *modelos* , crie *Person.cs* e substitua o código do modelo pelo código a seguir:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="update-instructor-and-student"></a>Atualizará Instructor e Student

Agora, atualize *Instructor.cs* e *Student.cs* para herdar valores do *Person.SC*.

No *Instructor.cs*, derive a classe `Instructor` da classe `Person` e remova os campos de chave e nome. O código será semelhante ao seguinte exemplo:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Faça alterações semelhantes em *Student.cs*. A classe `Student` se parecerá com o exemplo a seguir:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="add-person-to-the-model"></a>Adicionar pessoa ao modelo

No *SchoolContext.cs*, adicione uma propriedade `DbSet` para o tipo de entidade `Person`:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Isso é tudo o que o Entity Framework precisa para configurar a herança de tabela por hierarquia. Como você verá, quando o banco de dados for atualizado, ele terá uma tabela `Person` no lugar das tabelas `Student` e `Instructor`.

## <a name="create-and-update-migrations"></a>Criar e atualizar migrações

No console do Gerenciador de pacotes (PMC), digite o seguinte comando:

`Add-Migration Inheritance`

Execute o comando `Update-Database` no PMC. O comando falhará neste ponto porque temos dados existentes que as migrações não sabem como lidar. Você receberá uma mensagem de erro semelhante à seguinte:

> *Não foi possível descartar o objeto ' dbo. Instrutor ' porque é referenciado por uma restrição FOREIGN KEY.*

Abra as *migrações\&lt; timestamp&gt;\_Inheritance.cs* e substitua o método `Up` pelo código a seguir:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

Este código é responsável pelas seguintes tarefas de atualização de banco de dados:

- Remove as restrições de chave estrangeira e índices que apontam para a tabela Aluno.
- Renomeia a tabela Instrutor como Pessoa e faz as alterações necessárias para que ela armazene dados de Aluno:

    - Adiciona EnrollmentDate que permite valo nulo para os alunos.
    - Adiciona a coluna Discriminatória para indicar se uma linha refere-se a um aluno ou um instrutor.
    - Faz com que HireDate permita valor nulo, pois linhas de alunos não terão datas de contratação.
    - Adiciona um campo temporário que será usado para atualizar chaves estrangeiras que apontam para alunos. Quando você copiar os alunos para a tabela Person, eles obterão novos valores de chave primária.
- Copia os dados da tabela Aluno para a tabela Pessoa. Isso faz com que os alunos recebam novos valores de chave primária.
- Corrige valores de chave estrangeira que apontam para alunos.
- Recria restrições de chave estrangeira e índices, agora apontando-os para a tabela Person.

(Se você tiver usado o GUID, em vez de inteiro como o tipo de chave primária, os valores de chave primária dos alunos não precisarão ser alterados e várias dessas etapas poderão ser omitidas.)

Execute o comando `update-database` novamente.

(Em um sistema de produção, você deve fazer alterações correspondentes no método inoperante, caso você tenha feito isso para voltar para a versão anterior do banco de dados. Para este tutorial, você não usará o método down.)

> [!NOTE]
> É possível obter outros erros ao migrar dados e fazer alterações de esquema. Se você obtiver erros de migração que não pode resolver, poderá continuar com o tutorial alterando a cadeia de conexão no arquivo *Web. config* ou excluindo o banco de dados. A abordagem mais simples é renomear o banco de dados no arquivo *Web. config* . Por exemplo, altere o nome do banco de dados para ContosoUniversity2, conforme mostrado no exemplo a seguir:
>
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.xml?highlight=2)]
>
> Com um novo banco de dados, não há nenhum dado a ser migrado, e o comando `update-database` é muito mais provável de ser concluído sem erros. Para obter instruções sobre como excluir o banco de dados, consulte [como remover um banco de dados do Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/). Se você usar essa abordagem para continuar com o tutorial, ignore a etapa de implantação no final deste tutorial ou implante em um novo site e banco de dados. Se você implantar uma atualização no mesmo site em que já estava implantado, o EF receberá o mesmo erro quando executar migrações automaticamente. Se você quiser solucionar um erro de migrações, o melhor recurso é um dos fóruns Entity Framework ou StackOverflow.com.

## <a name="test-the-implementation"></a>Testará a implementação

Execute o site e tente várias páginas. Tudo funciona da mesma maneira que antes.

Em **Gerenciador de servidores,** expanda **Data Connections\SchoolContext** e, em seguida, **tabelas**, e você verá que as tabelas **Student** e **instrutor** foram substituídas por uma tabela **Person** . Expanda a tabela **Person** e você verá que ela tem todas as colunas que costumava estar nas tabelas **Student** e **instrutor** .

Clique com o botão direito do mouse na tabela Person e, em seguida, clique em **Mostrar Dados da Tabela** para ver a coluna discriminatória.

O diagrama a seguir ilustra a estrutura do novo banco de dados escolar:

![School_database_diagram](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

## <a name="deploy-to-azure"></a>Implantar no Azure

Esta seção exige que você tenha concluído a seção opcional **implantando o aplicativo no Azure** na [parte 3, classificando, filtrando e paginação](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) desta série de tutoriais. Se houver erros de migrações que você resolveu excluindo o banco de dados em seu projeto local, ignore esta etapa; ou crie um novo site e banco de dados e implante no novo ambiente.

1. No Visual Studio, clique com o botão direito do mouse no projeto no **Gerenciador de Soluções** e selecione **Publicar** no menu de contexto.

2. Clique em **Publicar**.

    O aplicativo Web é aberto no navegador padrão.

3. Teste o aplicativo para verificar se ele está funcionando.

    Na primeira vez que você executar uma página que acessa o banco de dados do, a Entity Framework executará todas as migrações `Up` métodos necessários para atualizar o banco de dados com o modelo de dado atual.

## <a name="get-the-code"></a>Obter o código

[Baixar projeto concluído](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Recursos adicionais

Links para outros recursos de Entity Framework podem ser encontrados nos [recursos de acesso a dados ASP.net-recomendado](../../../../whitepapers/aspnet-data-access-content-map.md).

Para obter mais informações sobre essa e outras estruturas de herança, consulte [padrão de herança de TPT](https://msdn.microsoft.com/data/jj618293) e padrão de herança de [TPH](https://msdn.microsoft.com/data/jj618292) no msdn. No próximo tutorial, você verá como lidar com uma variedade de cenários relativamente avançados do Entity Framework.

## <a name="next-steps"></a>{1&gt;{2&gt;Próximas etapas&lt;2}&lt;1}

Neste tutorial, você:

> [!div class="checklist"]
> * Aprendeu a mapear a herança para o banco de dados
> * Criou a classe Person
> * Atualizou Instructor e Student
> * Pessoa adicionada ao modelo
> * Migrações criadas e atualizadas
> * Testou a implementação
> * Implantado no Azure

Avance para o próximo artigo para saber mais sobre os tópicos que são úteis para conhecer quando você vai além dos conceitos básicos do desenvolvimento de aplicativos Web ASP.NET que usam Entity Framework Code First.
> [!div class="nextstepaction"]
> [Cenários de Entity Framework avançados](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
