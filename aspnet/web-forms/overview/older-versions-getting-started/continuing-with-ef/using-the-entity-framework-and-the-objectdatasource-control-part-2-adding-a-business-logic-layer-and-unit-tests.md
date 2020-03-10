---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
title: 'Usando o Entity Framework 4,0 e o controle ObjectDataSource, parte 2: adicionando uma camada de lógica de negócios e testes de unidade | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais se baseia no aplicativo Web da Contoso University criado pelo Introdução com a série de tutoriais do Entity Framework 4,0. I...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: efb0e677-10b8-48dc-93d3-9ba3902dd807
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
msc.type: authoredcontent
ms.openlocfilehash: 24344cc33d7c26d7c408db26c0530ef2c708a7d3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78546763"
---
# <a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests"></a>Usando o Entity Framework 4,0 e o controle ObjectDataSource, parte 2: adicionando uma camada de lógica de negócios e testes de unidade

por [Tom Dykstra](https://github.com/tdykstra)

> Esta série de tutoriais se baseia no aplicativo Web da Contoso University criado pelo [introdução com a série de tutoriais do Entity Framework 4,0](https://asp.net/entity-framework/tutorials#Getting%20Started) . Se você não concluiu os tutoriais anteriores, como ponto de partida para este tutorial, você pode [baixar o aplicativo](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que você criou. Você também pode [baixar o aplicativo](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) que é criado pela série de tutoriais completa. Se você tiver dúvidas sobre os tutoriais, poderá lançá-los no [Fórum de Entity Framework ASP.net](https://forums.asp.net/1227.aspx).

No tutorial anterior, você criou um aplicativo Web de n camadas usando o Entity Framework e o controle de `ObjectDataSource`. Este tutorial mostra como adicionar lógica de negócios enquanto mantém a camada de lógica de negócios (BLL) e a camada de acesso a dados (DAL) separadas e mostra como criar testes de unidade automatizados para a BLL.

Neste tutorial, você concluirá as seguintes tarefas:

- Crie uma interface de repositório que declare os métodos de acesso de dados de que você precisa.
- Implemente a interface do repositório na classe Repository.
- Crie uma classe de lógica de negócios que chama a classe de repositório para executar funções de acesso a dados.
- Conecte o controle de `ObjectDataSource` à classe de lógica de negócios em vez de à classe de repositório.
- Crie um projeto de teste de unidade e uma classe de repositório que usa coleções na memória para seu armazenamento de dados.
- Crie um teste de unidade para a lógica de negócios que você deseja adicionar à classe de lógica de negócios e, em seguida, execute o teste e veja se ele falha.
- Implemente a lógica de negócios na classe de lógica de negócios e execute novamente o teste de unidade e veja ele passar.

Você trabalhará com as páginas *departamentos. aspx* e *DepartmentsAdd. aspx* que você criou no tutorial anterior.

## <a name="creating-a-repository-interface"></a>Criando uma interface de repositório

Você começará criando a interface do repositório.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image1.png)

Na pasta *Dal* , crie um novo arquivo de classe, nomeie-o *ISchoolRepository.cs*e substitua o código existente pelo código a seguir:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample1.cs)]

A interface define um método para cada um dos métodos CRUD (criar, ler, atualizar, excluir) que você criou na classe de repositório.

Na classe `SchoolRepository` em *SchoolRepository.cs*, indique que essa classe implementa a interface `ISchoolRepository`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample2.cs)]

## <a name="creating-a-business-logic-class"></a>Criando uma classe de lógica de negócios

Em seguida, você criará a classe de lógica de negócios. Isso pode ser feito para que você possa adicionar lógica de negócios que será executada pelo controle de `ObjectDataSource`, embora você ainda não faça isso. Por enquanto, a nova classe de lógica de negócios executará apenas as mesmas operações CRUD que o repositório faz.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image3.png)

Crie uma nova pasta e nomeie-a de *BLL*. (Em um aplicativo do mundo real, a camada de lógica de negócios normalmente seria implementada como uma biblioteca de classes — um projeto separado — mas para manter este tutorial simples, as classes de BLL serão mantidas em uma pasta de projeto.)

Na pasta *BLL* , crie um novo arquivo de classe, nomeie-o *SchoolBL.cs*e substitua o código existente pelo código a seguir:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample3.cs)]

Esse código cria os mesmos métodos CRUD que você viu anteriormente na classe Repository, mas em vez de acessar os métodos de Entity Framework diretamente, ele chama os métodos de classe Repository.

A variável de classe que mantém uma referência à classe Repository é definida como um tipo de interface, e o código que instancia a classe Repository está contido em dois construtores. O construtor sem parâmetros será usado pelo controle de `ObjectDataSource`. Ele cria uma instância da classe `SchoolRepository` que você criou anteriormente. O outro construtor permite que qualquer código que instancie a classe de lógica de negócios passe em qualquer objeto que implemente a interface do repositório.

Os métodos CRUD que chamam a classe Repository e os dois construtores possibilitam o uso da classe de lógica de negócios com qualquer armazenamento de dados back-end que você escolher. A classe de lógica de negócios não precisa estar ciente de como a classe que ela está chamando persiste os dados. (Isso geralmente é chamado de *persistência ignorância*.) Isso facilita o teste de unidade, pois você pode conectar a classe de lógica de negócios a uma implementação de repositório que usa algo tão simples quanto as coleções de `List` na memória para armazenar dados.

> [!NOTE]
> Tecnicamente, os objetos de entidade ainda não são Persistence-que desconhecem, pois eles são instanciados a partir de classes que herdam da classe `EntityObject` do Entity Framework. Para ignorância de persistência completas, você pode usar *objetos CLR simples*ou *pocos*, no lugar de objetos que herdam da classe `EntityObject`. O uso do POCOs está além do escopo deste tutorial. Para obter mais informações, consulte [capacidade de teste e Entity Framework 4,0](https://msdn.microsoft.com/library/ff714955.aspx) no site do MSDN.)

Agora você pode conectar os controles de `ObjectDataSource` à classe de lógica de negócios em vez de ao repositório e verificar se tudo funciona como antes.

Em *departamentos. aspx* e *DepartmentsAdd. aspx*, altere cada ocorrência de `TypeName="ContosoUniversity.DAL.SchoolRepository"` para `TypeName="ContosoUniversity.BLL.SchoolBL`". (Há quatro instâncias.)

Execute as páginas *departamentos. aspx* e *DepartmentsAdd. aspx* para verificar se elas ainda funcionam como faziam antes.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image5.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image7.png)

## <a name="creating-a-unit-test-project-and-repository-implementation"></a>Criando uma implementação de projeto de teste de unidade e de repositório

Adicione um novo projeto à solução usando o modelo de **projeto de teste** e nomeie-o `ContosoUniversity.Tests`.

No projeto de teste, adicione uma referência a `System.Data.Entity` e adicione uma referência de projeto ao projeto `ContosoUniversity`.

Agora você pode criar a classe de repositório que será usada com testes de unidade. O repositório de dados para esse repositório estará dentro da classe.

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image9.png)

No projeto de teste, crie um novo arquivo de classe, nomeie-o *MockSchoolRepository.cs*e substitua o código existente pelo código a seguir:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample4.cs)]

Essa classe de repositório tem os mesmos métodos CRUD que o acessa o Entity Framework diretamente, mas eles trabalham com `List` coleções na memória, em vez de com um banco de dados. Isso torna mais fácil para uma classe de teste configurar e validar os testes de unidade para a classe de lógica de negócios.

## <a name="creating-unit-tests"></a>Criando testes de unidade

O modelo de projeto de **teste** criou uma classe de teste de unidade de stub para você, e a próxima tarefa é modificar essa classe adicionando métodos de teste de unidade a ela para lógica de negócios que você deseja adicionar à classe de lógica de negócios.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image11.png)

Na Contoso University, qualquer instrutor individual pode ser apenas o administrador de um único departamento e você precisa adicionar lógica de negócios para impor essa regra. Você começará adicionando testes e executando os testes para vê-los falhar. Em seguida, você adicionará o código e executará novamente os testes, esperando que eles sejam aprovados.

Abra o arquivo *UnitTest1.cs* e adicione instruções `using` para a lógica de negócios e camadas de acesso a dados que você criou no projeto ContosoUniversity:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample5.cs)]

Substitua o método `TestMethod1` pelos seguintes métodos:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample6.cs)]

O método `CreateSchoolBL` cria uma instância da classe Repository que você criou para o projeto de teste de unidade, que, em seguida, passa para uma nova instância da classe de lógica de negócios. Em seguida, o método usa a classe de lógica de negócios para inserir três departamentos que você pode usar em métodos de teste.

Os métodos de teste verificam se a classe de lógica de negócios gera uma exceção se alguém tenta inserir um novo departamento com o mesmo administrador de um departamento existente ou se alguém tenta atualizar o administrador de um departamento definindo-o como a ID de uma pessoa Quem já é o administrador de outro departamento.

Você ainda não criou a classe de exceção, portanto, esse código não será compilado. Para obtê-lo para compilação, clique com o botão direito do mouse em `DuplicateAdministratorException` e selecione **gerar**e, em seguida, **classe**.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image13.png)

Isso cria uma classe no projeto de teste que você pode excluir depois de criar a classe de exceção no projeto principal. e implementou a lógica de negócios.

Execute o projeto de teste. Conforme esperado, os testes falham.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image15.png)

## <a name="adding-business-logic-to-make-a-test-pass"></a>Adicionando lógica de negócios para fazer uma aprovação de teste

Em seguida, você implementará a lógica de negócios que torna impossível definir como administrador de um departamento de alguém que já seja administrador de outro departamento. Você lançará uma exceção da camada de lógica de negócios e a capturará na camada de apresentação se um usuário editar um departamento e clicar em **Atualizar** depois de selecionar alguém que já é um administrador. (Você também pode remover instrutores da lista suspensa que já são administradores antes de renderizar a página, mas a finalidade aqui é trabalhar com a camada de lógica de negócios.)

Comece criando a classe de exceção que você lançará quando um usuário tentar tornar um instrutor o administrador de mais de um departamento. No projeto principal, crie um novo arquivo de classe na pasta *BLL* , nomeie-o *DuplicateAdministratorException.cs*e substitua o código existente pelo código a seguir:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample7.cs)]

Agora exclua o arquivo *DuplicateAdministratorException.cs* temporário que você criou no projeto de teste anteriormente para poder compilar.

No projeto principal, abra o arquivo *SchoolBL.cs* e adicione o seguinte método que contém a lógica de validação. (O código se refere a um método que você criará posteriormente.)

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample8.cs)]

Você chamará esse método quando estiver inserindo ou atualizando as entidades `Department` para verificar se outro departamento já tem o mesmo administrador.

O código chama um método para pesquisar o banco de dados em busca de uma entidade `Department` que tenha o mesmo valor de propriedade `Administrator` que a entidade que está sendo inserida ou atualizada. Se um for encontrado, o código lançará uma exceção. Nenhuma verificação de validação será necessária se a entidade que está sendo inserida ou atualizada não tiver `Administrator` valor, e nenhuma exceção será gerada se o método for chamado durante uma atualização e a entidade de `Department` encontrada corresponder à entidade de `Department` que está sendo atualizada.

Chame o novo método dos métodos `Insert` e `Update`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample9.cs)]

No *ISchoolRepository.cs*, adicione a seguinte declaração para o novo método de acesso a dados:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample10.cs)]

No *SchoolRepository.cs*, adicione a seguinte instrução de `using`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample11.cs)]

No *SchoolRepository.cs*, adicione o novo método de acesso a dados a seguir:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample12.cs)]

Esse código recupera `Department` entidades que têm um administrador especificado. Somente um departamento deve ser encontrado (se houver). No entanto, como nenhuma restrição é criada no banco de dados, o tipo de retorno é uma coleção no caso de vários departamentos serem encontrados.

Por padrão, quando o contexto do objeto recupera entidades do banco de dados, ele controla seu Gerenciador de estado de objeto. O parâmetro `MergeOption.NoTracking` especifica que esse controle não será feito para essa consulta. Isso é necessário porque a consulta pode retornar a entidade exata que você está tentando atualizar e, em seguida, você não poderá anexar essa entidade. Por exemplo, se você editar o departamento de histórico na página *departamentos. aspx* e deixar o administrador inalterado, essa consulta retornará o departamento de histórico. Se `NoTracking` não estiver definido, o contexto do objeto já terá a entidade departamento do histórico em seu Gerenciador de estado de objeto. Em seguida, quando você anexa a entidade do departamento de histórico que é recriada a partir do estado de exibição, o contexto do objeto geraria uma exceção que diz `"An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key"`.

(Como uma alternativa para especificar `MergeOption.NoTracking`, você pode criar um novo contexto de objeto apenas para essa consulta. Como o novo contexto de objeto teria seu próprio Gerenciador de estado de objeto, não haveria nenhum conflito ao chamar o método `Attach`. O novo contexto de objeto compartilharia metadados e conexão de banco de dados com o contexto do objeto original, portanto, a penalidade de desempenho dessa abordagem alternativa seria mínima. A abordagem mostrada aqui, no entanto, apresenta a opção `NoTracking`, que você encontrará útil em outros contextos. A opção `NoTracking` será discutida mais detalhadamente em um tutorial posterior nesta série.)

No projeto de teste, adicione o novo método de acesso a dados a *MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample13.cs)]

Esse código usa o LINQ para executar a mesma seleção de dados que o repositório do projeto `ContosoUniversity` usa LINQ to Entities para o.

Execute o projeto de teste novamente. Dessa vez os testes são aprovados.

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image17.png)

## <a name="handling-objectdatasource-exceptions"></a>Manipulando exceções ObjectDataSource

No projeto `ContosoUniversity`, execute a página Departments *. aspx* e tente alterar o administrador de um departamento para alguém que já seja administrador de outro departamento. (Lembre-se de que você só pode editar os departamentos que adicionou durante este tutorial, pois o banco de dados vem pré-carregado com dado inválido.) Você Obtém a seguinte página de erro do servidor:

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image19.png)

Você não quer que os usuários vejam esse tipo de página de erro, portanto, você precisa adicionar o código de tratamento de erros. Abra Departments *. aspx* e especifique um manipulador para o evento `OnUpdated` do `DepartmentsObjectDataSource`. O `ObjectDataSource` marca de abertura agora é semelhante ao exemplo a seguir.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample14.aspx)]

No *Departments.aspx.cs*, adicione a seguinte instrução de `using`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample15.cs)]

Adicione o seguinte manipulador para o evento de `Updated`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample16.cs)]

Se o controle de `ObjectDataSource` capturar uma exceção ao tentar executar a atualização, ele passará a exceção no argumento de evento (`e`) para esse manipulador. O código no manipulador verifica para ver se a exceção é a exceção de administrador duplicada. Se for, o código criará um controle Validator que contém uma mensagem de erro para que o controle de `ValidationSummary` seja exibido.

Execute a página e tente tornar alguém o administrador de dois departamentos novamente. Desta vez, o controle de `ValidationSummary` exibe uma mensagem de erro.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image21.png)

Faça alterações semelhantes na página *DepartmentsAdd. aspx* . Em *DepartmentsAdd. aspx*, especifique um manipulador para o evento `OnInserted` do `DepartmentsObjectDataSource`. A marcação resultante será semelhante ao exemplo a seguir.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample17.aspx)]

No *DepartmentsAdd.aspx.cs*, adicione a mesma instrução `using`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample18.cs)]

Adicione o seguinte manipulador de eventos:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample19.cs)]

Agora você pode testar a página *DepartmentsAdd.aspx.cs* para verificar se ela também manipula corretamente as tentativas de tornar uma pessoa o administrador de mais de um departamento.

Isso conclui a introdução à implementação do padrão de repositório para usar o controle de `ObjectDataSource` com o Entity Framework. Para obter mais informações sobre o padrão de repositório e a capacidade de teste, consulte o artigo capacidade de teste do MSDN [e Entity Framework 4,0](https://msdn.microsoft.com/library/ff714955.aspx).

No tutorial a seguir, você verá como adicionar a funcionalidade de classificação e filtragem ao aplicativo.

> [!div class="step-by-step"]
> [Anterior](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)
> [Próximo](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
