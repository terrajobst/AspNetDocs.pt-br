---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: 'Tutorial: introdução ao Entity Framework 6 Code First usando o MVC 5 | Microsoft Docs'
description: Nesta série de tutoriais, você aprende a criar um aplicativo ASP.NET MVC 5 que usa o Entity Framework 6 para acesso a dados.
author: tdykstra
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: 00bc8b51-32ed-4fd3-9745-be4c2a9c1eaf
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: a00a5a3aa295c2584b90abbf931c258c9e1b58ca
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616126"
---
# <a name="tutorial-get-started-with-entity-framework-6-code-first-using-mvc-5"></a>Tutorial: introdução ao Entity Framework 6 Code First usando o MVC 5

> [!NOTE]
> Para um novo desenvolvimento, recomendamos [ASP.NET Core Razor Pages](/aspnet/core/razor-pages) sobre os controladores e exibições do ASP.NET MVC. Para obter uma série de tutoriais semelhante a esta usando Razor Pages, consulte [tutorial: introdução ao Razor pages no ASP.NET Core](/aspnet/core/tutorials/razor-pages/razor-pages-start). O novo tutorial:
> * É mais fácil de acompanhar.
> * Fornece mais práticas recomendadas do EF Core.
> * Usa consultas mais eficientes.
> * É mais atual com a API mais recente.
> * Aborda mais recursos.
> * É a abordagem preferencial para o desenvolvimento de novos aplicativos.

Nesta série de tutoriais, você aprende a criar um aplicativo ASP.NET MVC 5 que usa o Entity Framework 6 para acesso a dados. Este tutorial usa o fluxo de trabalho Code First. Para obter informações sobre como escolher entre Code First, Database First e Model First, consulte [criar um modelo](/ef/ef6/modeling/).

Esta série de tutoriais explica como criar o aplicativo de exemplo Contoso University. O aplicativo de exemplo é um site de Universidade simples. Com ele, você pode exibir e atualizar informações de aluno, curso e instrutor. Aqui estão duas das telas que você cria:

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![Editar aluno](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

Neste tutorial, você:

> [!div class="checklist"]
> * Criar um aplicativo Web MVC
> * Configurar o estilo do site
> * Instalar o Entity Framework 6
> * Criar o modelo de dados
> * Criar o contexto de banco de dados
> * Inicializar o BD com os dados de teste
> * Configurar o EF 6 para usar o LocalDB
> * Criar um controlador e exibições
> * Exibir o banco de dados

## <a name="prerequisites"></a>Prerequisites

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)

## <a name="create-an-mvc-web-app"></a>Criar um aplicativo Web MVC

1. Abra o Visual Studio e crie C# um projeto Web usando o modelo de **aplicativo web ASP.net (.NET Framework)** . Nomeie o projeto *ContosoUniversity* e selecione **OK**.

   ![Caixa de diálogo novo projeto no Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/new-project-dialog.png)

1. Em **novo ASP.NET Web Application-ContosoUniversity**, selecione **MVC**.

   ![Caixa de diálogo novo aplicativo Web no Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/new-web-app-dialog.png)

    > [!NOTE]
    > Por padrão, a opção de **autenticação** é definida como **sem autenticação**. Para este tutorial, o aplicativo Web não exige que os usuários entrem. Além disso, ele não restringe o acesso com base em quem está conectado.

1. Selecione **OK** para criar o projeto.

## <a name="set-up-the-site-style"></a>Configurar o estilo do site

Algumas alterações simples configurarão o menu do site, o layout e a home page.

1. Abra *Views\Shared\\_Layout. cshtml*e faça as seguintes alterações:

   - Altere cada ocorrência de "meu aplicativo ASP.NET" e "nome do aplicativo" para "Contoso University".
   - Adicione entradas de menu para alunos, cursos, instrutores e departamentos e exclua a entrada de contato.

   As alterações são realçadas no seguinte trecho de código:

   [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=6,19,24-27,38)]

2. No *Views\Home\Index.cshtml*, substitua o conteúdo do arquivo pelo código a seguir para substituir o texto sobre ASP.net e MVC por texto sobre este aplicativo:

   [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

3. Pressione CTRL + F5 para executar o site. Você verá a home page com o menu principal.

## <a name="install-entity-framework-6"></a>Instalar o Entity Framework 6

1. No menu **ferramentas** , escolha **Gerenciador de pacotes NuGet**e, em seguida, escolha **console do Gerenciador de pacotes**.

2. Na janela **Console do Gerenciador de Pacotes** , digite o seguinte comando:

   ```text
   Install-Package EntityFramework
   ```

Esta etapa é uma das algumas etapas que este tutorial tem feito manualmente, mas que poderia ter sido feita automaticamente pelo recurso scaffolding MVC ASP.NET. Você está fazendo isso manualmente para que possa ver as etapas necessárias para usar Entity Framework (EF). Você usará o scaffolding mais tarde para criar o controlador e as exibições do MVC. Uma alternativa é permitir que o scaffolding instale automaticamente o pacote NuGet do EF, crie a classe de contexto do banco de dados e crie a cadeia de conexão. Quando você estiver pronto para fazer isso, tudo o que precisa fazer é ignorar essas etapas e scaffoldr seu controlador MVC depois de criar suas classes de entidade.

## <a name="create-the-data-model"></a>Criar o modelo de dados

Em seguida, você criará as classes de entidade para o aplicativo Contoso University. Você começará com as três seguintes entidades:

**Curso** <-> **registro** <-> **aluno**

| Entidades | Relação |
| -------- | ------------ |
| Curso para registro | Um-para-muitos |
| Aluno para registro | Um-para-muitos |

Há uma relação um-para-muitos entre as entidades `Student` e `Enrollment`, e uma relação um-para-muitos entre as entidades `Course` e `Enrollment`. Em outras palavras, um aluno pode ser registrado em qualquer quantidade de cursos e um curso pode ter qualquer quantidade de alunos registrados.

Nas seções a seguir, você criará uma classe para cada uma dessas entidades.

> [!NOTE]
> Se você tentar compilar o projeto antes de concluir a criação de todas essas classes de entidade, obterá erros do compilador.

### <a name="the-student-entity"></a>A entidade Student

- Na pasta *modelos* , crie um arquivo de classe chamado *Student.cs* clicando com o botão direito do mouse na pasta em **Gerenciador de soluções** e escolhendo **Adicionar** > **classe**. Substitua o código do modelo pelo seguinte código:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs)]

A propriedade `ID` se tornará a coluna de chave primária da tabela de banco de dados que corresponde a essa classe. Por padrão, Entity Framework interpreta uma propriedade chamada `ID` ou *classname* `ID` como a chave primária.

A propriedade `Enrollments` é uma *propriedade de navegação*. As propriedades de navegação armazenam outras entidades que estão relacionadas a essa entidade. Nesse caso, a propriedade `Enrollments` de uma entidade `Student` manterá todas as entidades `Enrollment` relacionadas a essa entidade `Student`. Em outras palavras, se uma determinada linha de `Student` no banco de dados tiver duas linhas de `Enrollment` relacionadas (linhas que contêm o valor de chave primária do aluno em sua `StudentID` coluna de chave estrangeira), essa `Student` propriedade de navegação `Enrollments` da entidade conterá essas duas entidades de `Enrollment`.

As propriedades de navegação são normalmente definidas como `virtual` para que possam aproveitar determinadas funcionalidades de Entity Framework, como o *carregamento lento*. (O carregamento lento será explicado posteriormente, no tutorial [lendo dados relacionados](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) mais adiante nesta série.)

Se uma propriedade de navegação pode armazenar várias entidades (como em relações muitos para muitos ou um-para-muitos), o tipo precisa ser uma lista na qual entradas podem ser adicionadas, excluídas e atualizadas, como `ICollection`.

### <a name="the-enrollment-entity"></a>A entidade Enrollment

- Na pasta *Models*, crie *Enrollment.cs* e substitua o código existente pelo seguinte código:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

A propriedade `EnrollmentID` será a chave primária; essa entidade usa o padrão *classname* `ID` em vez de `ID` por si só como visto na entidade `Student`. Normalmente, você escolhe um padrão e usa-o em todo o modelo de dados. Aqui, a variação ilustra que você pode usar qualquer um dos padrões. Em um tutorial posterior, você verá como usar `ID` sem `classname` torna mais fácil implementar a herança no modelo de dados.

A propriedade `Grade` é uma [Enumeração](/ef/ef6/modeling/code-first/data-types/enums). O ponto de interrogação após a declaração de tipo `Grade` indica que a propriedade `Grade` permite valor [anulável](/dotnet/csharp/programming-guide/nullable-types/using-nullable-types). Uma classificação que é nula é diferente de uma taxa zero: null significa que uma classificação não é conhecida ou ainda não foi atribuída.

A propriedade `StudentID` é uma chave estrangeira e a propriedade de navegação correspondente é `Student`. Uma entidade `Enrollment` é associada a uma entidade `Student`, de modo que a propriedade possa armazenar apenas uma única entidade `Student` (ao contrário da propriedade de navegação `Student.Enrollments` que você viu anteriormente, que pode armazenar várias entidades `Enrollment`).

A propriedade `CourseID` é uma chave estrangeira e a propriedade de navegação correspondente é `Course`. Uma entidade `Enrollment` está associada a uma entidade `Course`.

Entity Framework interpreta uma propriedade como uma propriedade de chave estrangeira se ela for nomeada *&lt;nome da propriedade de navegação&gt;&lt;nome da propriedade de chave primária&gt;* (por exemplo, `StudentID` para a propriedade de navegação `Student`, pois a chave primária da entidade `Student` é `ID`). As propriedades de chave estrangeira também podem ser nomeadas com a mesma simples *&lt;nome da propriedade de chave primária&gt;* (por exemplo, `CourseID` uma vez que a chave primária da entidade `Course` é `CourseID`).

### <a name="the-course-entity"></a>A entidade Course

- Na pasta *modelos* , crie *Course.cs*, substituindo o código do modelo pelo código a seguir:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

A propriedade `Enrollments` é uma propriedade de navegação. Uma entidade `Course` pode estar relacionada a qualquer quantidade de entidades `Enrollment`.

Vamos falar mais sobre o atributo <xref:System.ComponentModel.DataAnnotations.Schema.DatabaseGeneratedAttribute> em um tutorial posterior nesta série. Basicamente, esse atributo permite que você insira a chave primária do curso, em vez de fazer com que ela seja gerada pelo banco de dados.

## <a name="create-the-database-context"></a>Criar o contexto de banco de dados

A classe principal que coordena Entity Framework funcionalidade para um determinado modelo de dados é a classe de *contexto do banco* de dado. Você cria essa classe derivando da classe [System. Data. Entity. DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.113).aspx) . No seu código, você especifica quais entidades estão incluídas no modelo de dados. Também personalize o comportamento específico do Entity Framework. Neste projeto, a classe é chamada `SchoolContext`.

- Para criar uma pasta no projeto ContosoUniversity, clique com o botão direito do mouse no projeto no **Gerenciador de soluções** , clique em **Adicionar**e, em seguida, clique em **nova pasta**. Nomeie a nova pasta *Dal* (para camada de acesso a dados). Nessa pasta, crie um novo arquivo de classe chamado *SchoolContext.cs*e substitua o código do modelo pelo código a seguir:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

### <a name="specify-entity-sets"></a>Especificar conjuntos de entidades

Esse código cria uma propriedade [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.113).aspx) para cada conjunto de entidades. Na terminologia Entity Framework, um *conjunto de entidades* geralmente corresponde a uma tabela de banco de dados e uma *entidade* corresponde a uma linha na tabela.

> [!NOTE]
>
> Você pode omitir as instruções `DbSet<Enrollment>` e `DbSet<Course>` e ela funcionaria da mesma. Entity Framework os incluiria implicitamente porque a entidade `Student` faz referência à entidade `Enrollment` e a entidade `Enrollment` faz referência à entidade `Course`.

### <a name="specify-the-connection-string"></a>Especificar a cadeia de conexão

O nome da cadeia de conexão (que você adicionará ao arquivo Web. config posteriormente) é passado para o construtor.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=1)]

Você também pode passar a cadeia de conexão em si, em vez do nome de um que está armazenado no arquivo Web. config. Para obter mais informações sobre as opções para especificar o banco de dados a ser usado, consulte [cadeias de conexão e modelos](/ef/ef6/fundamentals/configuring/connection-strings).

Se você não especificar uma cadeia de conexão ou o nome de uma explicitamente, Entity Framework pressupõe que o nome da cadeia de conexão é o mesmo que o nome da classe. O nome da cadeia de conexão padrão neste exemplo seria `SchoolContext`, o mesmo que o que você está especificando explicitamente.

### <a name="specify-singular-table-names"></a>Especificar nomes de tabela singulares

A instrução `modelBuilder.Conventions.Remove` no método [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) impede que nomes de tabela sejam pluraled. Se você não fez isso, as tabelas geradas no banco de dados seriam nomeadas `Students`, `Courses`e `Enrollments`. Em vez disso, os nomes de tabela serão `Student`, `Course`e `Enrollment`. Os desenvolvedores não concordam sobre se os nomes de tabela devem ser pluralizados ou não. Este tutorial usa o formulário singular, mas o ponto importante é que você pode selecionar qualquer formulário que preferir, incluindo ou omitindo esta linha de código.

## <a name="initialize-db-with-test-data"></a>Inicializar o BD com os dados de teste

Entity Framework pode criar (ou remover e recriar) automaticamente um banco de dados para você quando o aplicativo é executado. Você pode especificar que isso deve ser feito toda vez que seu aplicativo for executado ou somente quando o modelo estiver fora de sincronia com o banco de dados existente. Você também pode escrever um método de `Seed` que Entity Framework chamará automaticamente depois de criar o banco de dados para preenchê-lo com dado de teste.

O comportamento padrão é criar um banco de dados somente se ele não existir (e lançar uma exceção se o modelo tiver sido alterado e o banco de dados já existir). Nesta seção, você especificará que o banco de dados deve ser descartado e recriado sempre que o modelo for alterado. Descartar o banco de dados causa a perda de todos eles. Isso geralmente é bom durante o desenvolvimento, pois o método `Seed` será executado quando o banco de dados for recriado e recriará os dado de teste. Mas, em produção, geralmente você não deseja perder todos os dados sempre que precisar alterar o esquema do banco de dados. Posteriormente, você verá como lidar com alterações de modelo usando Migrações do Code First para alterar o esquema de banco de dados em vez de descartar e recriar o banco de dados.

1. Na pasta DAL, crie um novo arquivo de classe chamado *SchoolInitializer.cs* e substitua o código do modelo pelo código a seguir, o que faz com que um banco de dados seja criado quando necessário e carregue os dados de teste no novo banco de dado.

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

   O método `Seed` usa o objeto de contexto de banco de dados como um parâmetro de entrada, e o código no método usa esse objeto para adicionar novas entidades ao banco de dados. Para cada tipo de entidade, o código cria uma coleção de novas entidades, adiciona-as à propriedade de `DbSet` apropriada e salva as alterações no banco de dados. Não é necessário chamar o método `SaveChanges` depois de cada grupo de entidades, como é feito aqui, mas fazer isso ajuda a localizar a origem de um problema se ocorrer uma exceção enquanto o código estiver gravando no banco de dados.

2. Para dizer Entity Framework usar sua classe de inicializador, adicione um elemento ao elemento `entityFramework` no arquivo *Web. config* do aplicativo (aquele na pasta raiz do projeto), conforme mostrado no exemplo a seguir:

   [!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.xml?highlight=2-6)]

   O `context type` especifica o nome da classe de contexto totalmente qualificado e o assembly em que ele está, e o `databaseinitializer type` especifica o nome totalmente qualificado da classe do inicializador e o assembly no qual ele está. (Quando você não quiser que o EF use o inicializador, poderá definir um atributo no elemento `context`: `disableDatabaseInitialization="true"`.) Para obter mais informações, consulte [configurações do arquivo de configuração](/ef/ef6/fundamentals/configuring/config-file).

   Uma alternativa para definir o inicializador no arquivo *Web. config* é fazê-lo no código adicionando uma instrução `Database.SetInitializer` ao método `Application_Start` no arquivo *global.asax.cs* . Para obter mais informações, consulte [noções básicas sobre inicializadores de banco de dados no Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm).

O aplicativo agora está configurado para que, quando você acessar o banco de dados pela primeira vez em uma determinada execução do aplicativo, Entity Framework Compare o banco de dados com o modelo (suas `SchoolContext` e classes de entidade). Se houver uma diferença, o aplicativo descartará e criará novamente o banco de dados.

> [!NOTE]
> Ao implantar um aplicativo em um servidor Web de produção, você deve remover ou desabilitar o código que descarta e recria o banco de dados. Você fará isso em um tutorial posterior nesta série.

## <a name="set-up-ef-6-to-use-localdb"></a>Configurar o EF 6 para usar o LocalDB

O [LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb?view=sql-server-2017) é uma versão leve do mecanismo de banco de dados SQL Server Express. É fácil instalar e configurar o, inicia sob demanda e é executado no modo de usuário. O LocalDB é executado em um modo de execução especial de SQL Server Express que permite que você trabalhe com bancos de dados como arquivos *. MDF* . Você pode colocar os arquivos de banco de dados do LocalDB na pasta do *aplicativo\_data* de um projeto Web se quiser poder copiar o banco de dado com o projeto. O recurso de instância de usuário no SQL Server Express também permite que você trabalhe com arquivos *. MDF* , mas o recurso de instância de usuário foi preterido; Portanto, o LocalDB é recomendado para trabalhar com arquivos *. MDF* . O LocalDB é instalado por padrão com o Visual Studio.

Normalmente, SQL Server Express não é usado para aplicativos Web de produção. O LocalDB, em particular, não é recomendado para uso em produção com um aplicativo Web porque ele não foi projetado para funcionar com o IIS.

- Neste tutorial, você trabalhará com o LocalDB. Abra o arquivo *Web. config* do aplicativo e adicione um elemento `connectionStrings` anterior ao elemento `appSettings`, conforme mostrado no exemplo a seguir. (Certifique-se de atualizar o arquivo *Web. config* na pasta do projeto raiz. Há também um arquivo *Web. config* na subpasta *views* que você não precisa atualizar.)

   [!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.xml?highlight=1-3)]

A cadeia de conexão que você adicionou especifica que Entity Framework usará um banco de dados LocalDB chamado *ContosoUniversity1. MDF*. (O banco de dados ainda não existe, mas o EF o criará.) Se você quiser criar o banco de dados em sua pasta *\_data do aplicativo* , poderá adicionar `AttachDBFilename=|DataDirectory|\ContosoUniversity1.mdf` à cadeia de conexão. Para obter mais informações sobre cadeias de conexão, consulte [SQL Server cadeias de conexão para aplicativos Web ASP.net](/previous-versions/aspnet/jj653752(v=vs.110)).

Na verdade, você não precisa de uma cadeia de conexão no arquivo *Web. config* . Se você não fornecer uma cadeia de conexão, Entity Framework usará uma cadeia de conexão padrão com base em sua classe de contexto. Para obter mais informações, consulte [Code First para um novo banco de dados](/ef/ef6/modeling/code-first/workflows/new-database).

## <a name="create-controller-and-views"></a>Criar um controlador e exibições

Agora você criará uma página da Web para exibir dados. O processo de solicitar os dados dispara automaticamente a criação do banco de dado. Você começará criando um novo controlador. Mas antes de fazer isso, compile o projeto para tornar as classes de modelo e de contexto disponíveis para o controlador MVC scaffolding.

1. Clique com o botão direito do mouse na pasta **controladores** em **Gerenciador de soluções**, selecione **Adicionar**e clique em **novo item com Scaffold**.
2. Na caixa de diálogo **Adicionar Scaffold** , selecione **controlador MVC 5 com exibições, usando Entity Framework**e, em seguida, escolha **Adicionar**.

     ![Adicionar caixa de diálogo Scaffold no Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/add-scaffold.png)

3. Na caixa de diálogo **Adicionar controlador** , faça as seguintes seleções e, em seguida, escolha **Adicionar**:

   - Classe de modelo: **aluno (ContosoUniversity. Models)** . (Se você não vir essa opção na lista suspensa, compile o projeto e tente novamente.)
   - Classe de contexto de dados: **SchoolContext (ContosoUniversity. Dal)** .
   - Nome do controlador: **StudentController** (não StudentsController).
   - Deixe os valores padrão para os outros campos.

     Quando você clica em **Adicionar**, o scaffolder cria um arquivo *StudentController.cs* e um conjunto de exibições (arquivos *. cshtml* ) que funcionam com o controlador. No futuro, ao criar projetos que usam Entity Framework, você também pode aproveitar algumas funcionalidades adicionais do scaffolder: criar sua primeira classe de modelo, não criar uma cadeia de conexão e, em seguida, na caixa **Adicionar controlador** , especifique o **novo contexto de dados** selecionando o botão **+** ao lado da classe de **contexto de dados**. O scaffolder criará sua classe `DbContext` e a cadeia de conexão, bem como o controlador e as exibições.
4. O Visual Studio abre o arquivo *Controllers\StudentController.cs* . Você verá que uma variável de classe foi criada e instancia um objeto de contexto de banco de dados:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

     O método de ação `Index` Obtém uma lista de alunos do conjunto de entidades *estudantes* lendo a propriedade `Students` da instância de contexto do banco de dados:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

     A exibição *Student\Index.cshtml* exibe essa lista em uma tabela:

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cshtml)]
5. Pressione Ctrl+F5 para executar o projeto. (Se você receber um erro "não é possível criar cópia de sombra", feche o navegador e tente novamente.)

     Clique na guia **alunos** para ver os dados de teste inseridos pelo método de `Seed`. Dependendo de como restringir a janela do navegador, você verá o link da guia do aluno na barra de endereços superior ou terá que clicar no canto superior direito para ver o link.

     ![Botão de menu](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

## <a name="view-the-database"></a>Exibir o banco de dados

Quando você executou a página estudantes e o aplicativo tentou acessar o banco de dados, o EF descobriu que não havia nenhum banco de dados e criou um. Em seguida, o EF executou o método de semente para popular o banco de dados com data.

Você pode usar o **Gerenciador de servidores** ou o **pesquisador de objetos do SQL Server** (SSOX) para exibir o banco de dados no Visual Studio. Para este tutorial, você usará **Gerenciador de servidores**.

1. Feche o navegador.
2. Em **Gerenciador de servidores**, expanda **conexões de dados** (talvez seja necessário selecionar o botão atualizar primeiro), expanda **ContosoUniversity (contexto escolar)** e, em seguida, expanda **tabelas** para ver as tabelas no novo banco de dados.

3. Clique com o botão direito do mouse na tabela **Student** e clique em **Mostrar dados da tabela** para ver as colunas que foram criadas e as linhas que foram inseridas na tabela.

4. Feche a conexão **Gerenciador de servidores** .

Os arquivos de banco de dados *ContosoUniversity1. MDF* e *. ldf* estão na pasta *% USERPROFILE%* .

Como você está usando o inicializador de `DropCreateDatabaseIfModelChanges`, agora você pode fazer uma alteração na classe `Student`, executar o aplicativo novamente e o banco de dados será recriado automaticamente para corresponder à sua alteração. Por exemplo, se você adicionar uma propriedade `EmailAddress` à classe `Student`, execute a página alunos novamente e, em seguida, examine a tabela novamente, você verá uma nova coluna `EmailAddress`.

## <a name="conventions"></a>Convenções

A quantidade de código que você tinha de escrever para que Entity Framework seja capaz de criar um banco de dados completo é o mínimo devido a *convenções*ou suposições que Entity Framework faz. Alguns deles já foram observados ou foram usados sem que você esteja atento a eles:

- As formas pluraled de nomes de classe de entidade são usadas como nomes de tabela.
- Os nomes de propriedade de entidade são usados para nomes de coluna.
- As propriedades de entidade nomeadas `ID` ou *classname* `ID` são reconhecidas como propriedades de chave primária.
- Uma propriedade será interpretada como uma propriedade de chave estrangeira se ela for nomeada *&lt;nome da propriedade de navegação&gt;&lt;nome da propriedade de chave primária&gt;* (por exemplo, `StudentID` para a propriedade de navegação `Student`, pois a chave primária da entidade `Student` é `ID`). As propriedades de chave estrangeira também podem ser nomeadas com a mesma simples &lt;nome da propriedade de chave primária&gt; (por exemplo, `EnrollmentID` uma vez que a chave primária da entidade `Enrollment` é `EnrollmentID`).

Você viu que as convenções podem ser substituídas. Por exemplo, você especificou que os nomes de tabela não devem ser plural, e você verá mais tarde como marcar explicitamente uma propriedade como uma propriedade de chave estrangeira.

## <a name="get-the-code"></a>Obter o código

[Baixar projeto concluído](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Recursos adicionais

Para saber mais sobre o EF 6, consulte estes artigos:

* [Acesso a Dados do ASP.NET – recursos recomendados](../../../../whitepapers/aspnet-data-access-content-map.md)

* [Convenções de Code First](/ef/ef6/modeling/code-first/conventions/built-in)

* [Criação de um modelo de dados mais complexo](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você:

> [!div class="checklist"]
> * Criou um aplicativo Web MVC
> * Configurar o estilo do site
> * Instalado o Entity Framework 6
> * Adicionou o modelo de dados
> * Criou o contexto de banco de dados
> * Inicializou o BD com os dados de teste
> * Configurar o EF 6 para usar o LocalDB
> * Criou um controlador e exibições
> * Exibiu o banco de dados

Avance para o próximo artigo para saber como examinar e personalizar o código CRUD (criar, ler, atualizar, excluir) em seus controladores e exibições.
> [!div class="nextstepaction"]
> [Implementar a funcionalidade CRUD básica](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)