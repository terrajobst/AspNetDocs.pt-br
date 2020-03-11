---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-vb
title: Criando procedimentos armazenados e funções definidas pelo usuário com o código gerenciado (VB) | Microsoft Docs
author: rick-anderson
description: O Microsoft SQL Server 2005 integra-se com o Common Language Runtime do .NET para permitir que os desenvolvedores criem objetos de banco de dados por meio de código gerenciado. Este tutorial...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: 8be9a51b-ea6b-46c7-bfa2-476d9b14c24c
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 0ac5f71d519689a9dc84fb82a04196d520cca6e1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78551299"
---
# <a name="creating-stored-procedures-and-user-defined-functions-with-managed-code-vb"></a>Criar procedimentos armazenados e funções definidas pelo usuário com código gerenciado (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_75_VB.zip) ou [baixar PDF](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/datatutorial75vb1.pdf)

> O Microsoft SQL Server 2005 integra-se com o Common Language Runtime do .NET para permitir que os desenvolvedores criem objetos de banco de dados por meio de código gerenciado. Este tutorial mostra como criar procedimentos armazenados gerenciados e funções gerenciadas definidas pelo usuário com seu Visual Basic C# ou código. Também vemos como essas edições do Visual Studio permitem que você depure esses objetos de banco de dados gerenciado.

## <a name="introduction"></a>Introdução

Bancos de dados como o Microsoft s SQL Server 2005 usam o [Transact-linguagem SQL (T-SQL)](http://en.wikipedia.org/wiki/Transact-SQL) para inserção, modificação e recuperação. A maioria dos sistemas de banco de dados inclui construções para agrupar uma série de instruções SQL que podem ser executadas como uma única unidade reutilizável. Os procedimentos armazenados são um exemplo. Outra é UDFs ( *funções definidas pelo usuário*), uma construção que examinaremos mais detalhadamente na etapa 9.

Em seu núcleo, o SQL foi projetado para trabalhar com conjuntos de dados. As instruções `SELECT`, `UPDATE`e `DELETE` se aplicam inerentemente a todos os registros na tabela correspondente e são limitadas apenas por suas cláusulas `WHERE`. Ainda há muitos recursos de linguagem projetados para trabalhar com um registro por vez e para manipular dados escalares. [`CURSOR` s](http://www.sqlteam.com/item.asp?ItemID=553) permitem que um conjunto de registros seja executado em loop por um de cada vez. Funções de manipulação de cadeia de caracteres como `LEFT`, `CHARINDEX`e `PATINDEX` funcionam com dados escalares. O SQL também inclui instruções de fluxo de controle como `IF` e `WHILE`.

Antes do Microsoft SQL Server 2005, os procedimentos armazenados e UDFs só podiam ser definidos como uma coleção de instruções T-SQL. O SQL Server 2005, no entanto, foi projetado para fornecer integração com o [CLR (Common Language Runtime)](https://msdn.microsoft.com/netframework/aa497266.aspx), que é o tempo de execução usado por todos os assemblies do .net. Consequentemente, os procedimentos armazenados e UDFs em um banco de dados SQL Server 2005 podem ser criados usando código gerenciado. Ou seja, você pode criar um procedimento armazenado ou UDF como um método em uma classe Visual Basic. Isso permite que esses procedimentos armazenados e UDFs utilizem a funcionalidade no .NET Framework e de suas próprias classes personalizadas.

Neste tutorial, examinaremos como criar procedimentos armazenados gerenciados e funções definidas pelo usuário e como integrá-los ao nosso banco de dados Northwind. Vamos começar!

> [!NOTE]
> Os objetos de banco de dados gerenciado oferecem algumas vantagens em relação às suas contrapartes do SQL. A riqueza e a familiaridade da linguagem e a capacidade de reutilizar o código e a lógica existentes são as principais vantagens. Mas os objetos de banco de dados gerenciados provavelmente serão menos eficientes ao trabalhar com conjuntos de data que não envolvem muita lógica de procedimento. Para obter uma discussão mais completa sobre as vantagens de usar código gerenciado versus T-SQL, Confira as [vantagens de usar código gerenciado para criar objetos de banco de dados](https://msdn.microsoft.com/library/k2e1fb36(VS.80).aspx).

## <a name="step-1-moving-the-northwind-database-out-ofapp_data"></a>Etapa 1: movendo o banco de dados Northwind para fora do`App_Data`

Todos os nossos tutoriais, até agora, usaram um arquivo de banco de dados Microsoft SQL Server 2005 Express Edition na pasta aplicativos Web s `App_Data`. Colocar o banco de dados no `App_Data` simplifica a distribuição e a execução desses tutoriais, pois todos os arquivos estavam localizados em um diretório e não exigiam nenhuma etapa de configuração adicional para testar o tutorial.

Para este tutorial, no entanto, vamos mover o banco de dados Northwind de `App_Data` e registrá-lo explicitamente com a instância de banco de dados SQL Server 2005 Express Edition. Embora possamos executar as etapas para este tutorial com o banco de dados na pasta `App_Data`, várias etapas são feitas de forma muito mais simples, registrando explicitamente o banco de dados com a instância de banco de dados SQL Server 2005 Express Edition.

O download para este tutorial tem os dois arquivos de banco de dados – `NORTHWND.MDF` e `NORTHWND_log.LDF` colocados em uma pasta chamada `DataFiles`. Se você estiver acompanhando sua própria implementação dos tutoriais, feche o Visual Studio e mova o `NORTHWND.MDF` e `NORTHWND_log.LDF` arquivos da pasta `App_Data` do site para uma pasta fora do site. Depois que os arquivos de banco de dados tiverem sido movidos para outra pasta, precisamos registrar o banco de dados Northwind com a instância de banco de dados do SQL Server 2005 Express Edition. Isso pode ser feito em SQL Server Management Studio. Se você tiver uma edição não Express do SQL Server 2005 instalada em seu computador, provavelmente já terá Management Studio instalado. Se você só tiver SQL Server 2005 Express Edition em seu computador, Reserve um momento para baixar e instalar o [Microsoft SQL Server Management Studio Express](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796).

Inicialização do SQL Server Management Studio. Como mostra a Figura 1, Management Studio começa solicitando a qual servidor se conectar. Digite localhost\SQLExpress para o nome do servidor, escolha autenticação do Windows na lista suspensa autenticação e clique em conectar.

![Conectar-se à instância de banco de dados apropriada](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image1.png)

**Figura 1**: conectar-se à instância de banco de dados apropriada

Quando você estiver conectado, a janela pesquisador de objetos listará informações sobre a SQL Server 2005 Express Edition instância do banco de dados, incluindo seus bancos, informações de segurança, opções de gerenciamento e assim por diante.

Precisamos anexar o banco de dados Northwind na pasta `DataFiles` (ou onde você pode tê-la movido) para a instância de banco de dados SQL Server 2005 Express Edition. Clique com o botão direito do mouse na pasta bancos de dados e escolha a opção anexar no menu de contexto. Isso abrirá a caixa de diálogo anexar bancos de dados. Clique no botão Adicionar, faça uma busca detalhada no arquivo de `NORTHWND.MDF` apropriado e clique em OK. Neste ponto, sua tela deve ser semelhante à figura 2.

[![conectar-se à instância de banco de dados apropriada](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image3.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image2.png)

**Figura 2**: conectar-se à instância de banco de dados apropriada ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image4.png))

> [!NOTE]
> Ao se conectar à instância de SQL Server 2005 Express Edition por meio do Management Studio a caixa de diálogo anexar bancos de dados não permite que você faça uma busca detalhada nos diretórios de perfil do usuário, como meus documentos. Portanto, certifique-se de colocar o `NORTHWND.MDF` e `NORTHWND_log.LDF` arquivos em um diretório de perfil que não seja de usuário.

Clique no botão OK para anexar o banco de dados. A caixa de diálogo anexar bancos de dados será fechada e o pesquisador de objetos agora deverá listar o banco de dados que acabou de ser anexado. É provável que o banco de dados Northwind tenha um nome como `9FE54661B32FDD967F51D71D0D5145CC_LINE ARTICLES\DATATUTORIALS\VOLUME 3\CSHARP\73\ASPNET_DATA_TUTORIAL_75_CS\APP_DATA\NORTHWND.MDF`. Renomeie o banco de dados para Northwind clicando com o botão direito do mouse no banco de dados e escolhendo renomear.

![Renomear o banco de dados como Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image5.png)

**Figura 3**: renomear o banco de dados para Northwind

## <a name="step-2-creating-a-new-solution-and-sql-server-project-in-visual-studio"></a>Etapa 2: criar uma nova solução e SQL Server projeto no Visual Studio

Para criar procedimentos armazenados gerenciados ou UDFs no SQL Server 2005, escreveremos o procedimento armazenado e a lógica UDF como Visual Basic código em uma classe. Depois que o código tiver sido gravado, precisaremos compilar essa classe em um assembly (um arquivo de `.dll`), registrar o assembly com o banco de dados de SQL Server e, em seguida, criar um procedimento armazenado ou objeto UDF no banco de dados que aponta para o método correspondente no assembly. Essas etapas podem ser executadas manualmente. Podemos criar o código em qualquer editor de texto, compilá-lo a partir da linha de comando usando o compilador de Visual Basic (`vbc.exe`), registrá-lo com o banco de dados usando o comando [`CREATE ASSEMBLY`](https://msdn.microsoft.com/library/ms189524.aspx) ou de Management Studio e adicionar o procedimento armazenado ou o objeto UDF por meio de meios semelhantes. Felizmente, as versões Professional e Team Systems do Visual Studio incluem um tipo de projeto SQL Server que automatiza essas tarefas. Neste tutorial, veremos como usar o tipo de projeto SQL Server para criar um procedimento armazenado gerenciado e UDF.

> [!NOTE]
> Se você estiver usando o Visual Web Developer ou a Standard Edition do Visual Studio, precisará usar a abordagem manual em vez disso. A etapa 13 fornece instruções detalhadas para executar essas etapas manualmente. Recomendo que você leia as etapas 2 a 12 antes de ler a etapa 13, já que essas etapas incluem importantes SQL Server instruções de configuração que devem ser aplicadas, independentemente de qual versão do Visual Studio você está usando.

Comece abrindo o Visual Studio. No menu arquivo, escolha novo projeto para exibir a caixa de diálogo novo projeto (consulte a Figura 4). Faça uma busca detalhada no tipo de projeto de banco de dados e, em seguida, nos modelos listados à direita, escolha criar um novo projeto de SQL Server. Optei por nomear este projeto `ManagedDatabaseConstructs` e o coloquei em uma solução chamada `Tutorial75`.

[![criar um novo projeto de SQL Server](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image7.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image6.png)

**Figura 4**: criar um novo projeto de SQL Server ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image8.png))

Clique no botão OK na caixa de diálogo novo projeto para criar a solução e SQL Server projeto.

Um projeto SQL Server está vinculado a um banco de dados específico. Consequentemente, depois de criar o novo projeto SQL Server, imediatamente solicitamos que especifiquemos essas informações. A Figura 5 mostra a caixa de diálogo nova referência de banco de dados que foi preenchida para apontar para o banco de dados Northwind que registramos na instância de banco de dados SQL Server 2005 Express Edition de volta na etapa 1.

![Associar o projeto de SQL Server ao banco de dados Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image9.png)

**Figura 5**: associar o projeto de SQL Server ao banco de dados Northwind

Para depurar os procedimentos armazenados gerenciados e UDFs que criaremos dentro desse projeto, precisamos habilitar o suporte à depuração SQL/CLR para a conexão. Sempre que associar um projeto SQL Server com um novo banco de dados (como fizemos na Figura 5), o Visual Studio perguntará se desejamos habilitar a depuração SQL/CLR na conexão (veja a Figura 6). Clique em Sim.

![Habilitar depuração SQL/CLR](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image10.png)

**Figura 6**: habilitar a depuração SQL/CLR

Neste ponto, o novo projeto de SQL Server foi adicionado à solução. Ele contém uma pasta chamada `Test Scripts` com um arquivo chamado `Test.sql`, que é usado para depurar os objetos de banco de dados gerenciados criados no projeto. Veremos a depuração na etapa 12.

Agora, podemos adicionar novos procedimentos armazenados gerenciados e UDFs a esse projeto, mas antes de deixarmos que os s incluam primeiro nosso aplicativo Web existente na solução. No menu arquivo, selecione a opção Adicionar e escolha site existente. Navegue até a pasta do site apropriada e clique em OK. Como mostra a Figura 7, isso atualizará a solução para incluir dois projetos: o site e o `ManagedDatabaseConstructs` SQL Server projeto.

![O Gerenciador de Soluções agora inclui dois projetos](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image11.png)

**Figura 7**: a Gerenciador de soluções agora inclui dois projetos

O valor `NORTHWNDConnectionString` no `Web.config` atualmente faz referência ao arquivo `NORTHWND.MDF` na pasta `App_Data`. Como removemos esse banco de dados de `App_Data` e o registramos explicitamente na instância do banco de dados SQL Server 2005 Express Edition, precisamos atualizar o valor de `NORTHWNDConnectionString` de forma correspondente. Abra o arquivo `Web.config` no site e altere o valor `NORTHWNDConnectionString` para que a cadeia de conexão Leia: `Data Source=localhost\SQLExpress;Initial Catalog=Northwind;Integrated Security=True`. Após essa alteração, sua seção `<connectionStrings>` em `Web.config` deve ser semelhante ao seguinte:

[!code-xml[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample1.xml)]

> [!NOTE]
> Conforme discutido no [tutorial anterior](debugging-stored-procedures-vb.md), ao depurar um objeto de SQL Server de um aplicativo cliente, como um site do ASP.net, precisamos desabilitar o pool de conexões. A cadeia de conexão mostrada acima desabilita o pooling de conexão (`Pooling=false`). Se você não planeja depurar os procedimentos armazenados gerenciados e UDFs do site do ASP.NET, habilite o pool de conexões.

## <a name="step-3-creating-a-managed-stored-procedure"></a>Etapa 3: Criando um procedimento armazenado gerenciado

Para adicionar um procedimento armazenado gerenciado ao banco de dados Northwind, primeiro precisamos criar o procedimento armazenado como um método no projeto SQL Server. No Gerenciador de Soluções, clique com o botão direito do mouse no nome do projeto `ManagedDatabaseConstructs` e escolha Adicionar um novo item. Isso exibirá a caixa de diálogo Adicionar novo item, que lista os tipos de objetos de banco de dados gerenciados que podem ser adicionados ao projeto. Como mostra a Figura 8, isso inclui procedimentos armazenados e funções definidas pelo usuário, entre outros.

Deixe que os s comecem adicionando um procedimento armazenado que simplesmente retorna todos os produtos que foram descontinuados. Nomeie o novo arquivo de procedimento armazenado `GetDiscontinuedProducts.vb`.

[![adicionar um novo procedimento armazenado chamado GetDiscontinuedProducts. vb](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image13.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image12.png)

**Figura 8**: adicionar um novo procedimento armazenado chamado `GetDiscontinuedProducts.vb` ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image14.png))

Isso criará um novo arquivo de classe Visual Basic com o seguinte conteúdo:

[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample2.vb)]

Observe que o procedimento armazenado é implementado como um método `Shared` dentro de um arquivo de classe `Partial` chamado `StoredProcedures`. Além disso, o método `GetDiscontinuedProducts` é decorado com o [atributo`SqlProcedure`](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlprocedureattribute.aspx), que marca o método como um procedimento armazenado.

O código a seguir cria um objeto `SqlCommand` e define sua `CommandText` como uma consulta `SELECT` que retorna todas as colunas da tabela `Products` para produtos cujo campo `Discontinued` é igual a 1. Em seguida, ele executa o comando e envia os resultados de volta para o aplicativo cliente. Adicione este código ao método `GetDiscontinuedProducts`.

[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample3.vb)]

Todos os objetos de banco de dados gerenciado têm acesso a um [objeto`SqlContext`](https://msdn.microsoft.com/library/ms131108.aspx) que representa o contexto do chamador. O `SqlContext` fornece acesso a um [objeto`SqlPipe`](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlpipe.aspx) por meio de sua [Propriedade`Pipe`](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlcontext.pipe.aspx). Esse objeto de `SqlPipe` é usado para ferry informações entre o banco de dados SQL Server e o aplicativo de chamada. Como o nome indica, o [método`ExecuteAndSend`](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlpipe.executeandsend.aspx) executa um objeto de `SqlCommand` passado e envia os resultados de volta para o aplicativo cliente.

> [!NOTE]
> Os objetos de banco de dados gerenciados são mais adequados para procedimentos armazenados e UDFs que usam lógica de procedimento em vez da lógica baseada em conjunto. A lógica de procedimento envolve trabalhar com conjuntos de dados linha por linha ou trabalhar com dados escalares. No entanto, o método `GetDiscontinuedProducts` que acabamos de criar, não envolve nenhuma lógica de procedimento. Portanto, o ideal seria ser implementado como um procedimento armazenado T-SQL. Ele é implementado como um procedimento armazenado gerenciado para demonstrar as etapas necessárias para criar e implantar procedimentos armazenados gerenciados.

## <a name="step-4-deploying-the-managed-stored-procedure"></a>Etapa 4: Implantando o procedimento armazenado gerenciado

Com esse código concluído, estamos prontos para implantá-lo no banco de dados Northwind. Implantar um projeto de SQL Server compila o código em um assembly, registra o assembly com o banco de dados e cria os objetos correspondentes no banco de dados, vinculando-os aos métodos apropriados no assembly. O conjunto exato de tarefas executadas pela opção implantar é escrito mais precisamente na etapa 13. Clique com o botão direito do mouse no nome do projeto `ManagedDatabaseConstructs` na Gerenciador de Soluções e escolha a opção implantar. No entanto, a implantação falha com o seguinte erro: sintaxe incorreta próxima a ' EXTERNAL '. Talvez seja necessário definir o nível de compatibilidade do banco de dados atual em um valor mais alto para habilitar este recurso. Consulte a ajuda para o procedimento armazenado `sp_dbcmptlevel`.

Essa mensagem de erro ocorre ao tentar registrar o assembly com o banco de dados Northwind. Para registrar um assembly com um banco de dados SQL Server 2005, o nível de compatibilidade do banco de dados deve ser definido como 90. Por padrão, novos bancos de dados SQL Server 2005 têm um nível de compatibilidade de 90. No entanto, os bancos de dados criados usando Microsoft SQL Server 2000 têm um nível de compatibilidade padrão de 80. Como o banco de dados Northwind era inicialmente um banco de dados Microsoft SQL Server 2000, seu nível de compatibilidade está atualmente definido como 80 e, portanto, precisa ser aumentado para 90 a fim de registrar objetos de banco de dados gerenciados.

Para atualizar o nível de compatibilidade do banco de dados, abra uma nova janela de consulta no Management Studio e digite:

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample4.sql)]

Clique no ícone executar na barra de ferramentas para executar a consulta acima.

[![atualizar o nível de compatibilidade do banco de dados Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image16.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image15.png)

**Figura 9**: atualizar o nível de compatibilidade do banco de dados Northwind ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image17.png))

Depois de atualizar o nível de compatibilidade, reimplante o projeto SQL Server. Desta vez, a implantação deve ser concluída sem erros.

Volte para SQL Server Management Studio, clique com o botão direito do mouse no banco de dados Northwind no Pesquisador de objetos e escolha Atualizar. Em seguida, faça uma busca detalhada na pasta de programação e expanda a pasta assemblies. Como mostra a Figura 10, o banco de dados Northwind agora inclui o assembly gerado pelo projeto `ManagedDatabaseConstructs`.

![O assembly ManagedDatabaseConstructs agora está registrado com o banco de dados Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image18.png)

**Figura 10**: o assembly `ManagedDatabaseConstructs` agora está registrado com o banco de dados Northwind

Expanda também a pasta procedimentos armazenados. Lá, você verá um procedimento armazenado chamado `GetDiscontinuedProducts`. Esse procedimento armazenado foi criado pelo processo de implantação e aponta para o método `GetDiscontinuedProducts` no assembly `ManagedDatabaseConstructs`. Quando o procedimento armazenado `GetDiscontinuedProducts` é executado, ele, por sua vez, executa o método `GetDiscontinuedProducts`. Como esse é um procedimento armazenado gerenciado, ele não pode ser editado por meio de Management Studio (portanto, o ícone de bloqueio ao lado do nome do procedimento armazenado).

![O procedimento armazenado GetDiscontinuedProducts está listado na pasta procedimentos armazenados](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image19.png)

**Figura 11**: o `GetDiscontinuedProducts` procedimento armazenado está listado na pasta procedimentos armazenados

Ainda há mais um obstáculo que precisamos superar antes de chamarmos o procedimento armazenado gerenciado: o banco de dados está configurado para evitar a execução de código gerenciado. Verifique isso abrindo uma nova janela de consulta e executando o procedimento armazenado `GetDiscontinuedProducts`. Você receberá a seguinte mensagem de erro: a execução do código do usuário no .NET Framework está desabilitada. Habilitar a opção de configuração CLR Enabled.

Para examinar as informações de configuração de s do banco de dados Northwind, insira e execute o comando `exec sp_configure` na janela de consulta. Isso mostra que a configuração CLR habilitado está definida atualmente como 0.

[![a configuração CLR Enabled está definida atualmente como 0](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image21.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image20.png)

**Figura 12**: a configuração CLR Enabled está definida atualmente como 0 ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image22.png))

Observe que cada definição de configuração na Figura 12 tem quatro valores listados com ela: os valores mínimo e máximo e os valores de configuração e de execução. Para atualizar o valor de configuração para a configuração CLR Enabled, execute o seguinte comando:

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample5.sql)]

Se você executar novamente o `exec sp_configure`, verá que a instrução acima atualizou o valor de configuração de s de configurações do CLR habilitado para 1, mas que o valor de execução ainda está definido como 0. Para que essa alteração de configuração entre em vigor, precisamos executar o [comando`RECONFIGURE`](https://msdn.microsoft.com/library/ms176069.aspx), que definirá o valor de execução para o valor de configuração atual. Basta inserir `RECONFIGURE` na janela de consulta e clicar no ícone executar na barra de ferramentas. Se você executar `exec sp_configure` agora, você deverá ver um valor de 1 para a configuração habilitada do CLR config e valores de execução.

Com a configuração habilitada para CLR concluída, estamos prontos para executar o procedimento armazenado `GetDiscontinuedProducts` gerenciado. Na janela de consulta, insira e execute o comando `exec` `GetDiscontinuedProducts`. Invocar o procedimento armazenado faz com que o código gerenciado correspondente no método de `GetDiscontinuedProducts` seja executado. Esse código emite uma consulta `SELECT` para retornar todos os produtos que foram descontinuados e retorna esses dados para o aplicativo de chamada, que é SQL Server Management Studio nessa instância. Management Studio recebe esses resultados e os exibe na janela resultados.

[![o procedimento armazenado GetDiscontinuedProducts retorna todos os produtos descontinuados](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image24.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image23.png)

**Figura 13**: o procedimento armazenado `GetDiscontinuedProducts` retorna todos os produtos descontinuados ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image25.png))

## <a name="step-5-creating-managed-stored-procedures-that-accept-input-parameters"></a>Etapa 5: Criando procedimentos armazenados gerenciados que aceitam parâmetros de entrada

Muitas das consultas e procedimentos armazenados que criamos em todos esses tutoriais usaram *parâmetros*. Por exemplo, no tutorial [criando novos procedimentos armazenados para o conjunto de dados do tipo s TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) , criamos um procedimento armazenado chamado `GetProductsByCategoryID` que aceitava um parâmetro de entrada chamado `@CategoryID`. Em seguida, o procedimento armazenado retornou todos os produtos cujo campo `CategoryID` corresponde ao valor do parâmetro `@CategoryID` fornecido.

Para criar um procedimento armazenado gerenciado que aceita parâmetros de entrada, basta especificar esses parâmetros na definição do método. Para ilustrar isso, vamos adicionar outro procedimento armazenado gerenciado ao projeto `ManagedDatabaseConstructs` chamado `GetProductsWithPriceLessThan`. Esse procedimento armazenado gerenciado aceitará um parâmetro de entrada especificando um preço e retornará todos os produtos cujo campo `UnitPrice` seja menor que o valor s do parâmetro.

Para adicionar um novo procedimento armazenado ao projeto, clique com o botão direito do mouse no nome do projeto `ManagedDatabaseConstructs` e escolha Adicionar um novo procedimento armazenado. Nomeie o arquivo `GetProductsWithPriceLessThan.vb`. Como vimos na etapa 3, isso criará um novo arquivo de classe Visual Basic com um método chamado `GetProductsWithPriceLessThan` colocado dentro do `StoredProcedures`de classe de `Partial`.

Atualize a definição de `GetProductsWithPriceLessThan` do método para que aceite um parâmetro de entrada de [`SqlMoney`](https://msdn.microsoft.com/library/system.data.sqltypes.sqlmoney.aspx) chamado `price` e escreva o código a ser executado e retorne os resultados da consulta:

[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample6.vb)]

A definição e o código do método s de `GetProductsWithPriceLessThan` se assemelham à definição e ao código do método `GetDiscontinuedProducts` criado na etapa 3. As únicas diferenças são que o método de `GetProductsWithPriceLessThan` aceita como parâmetro de entrada (`price`), a consulta de `SqlCommand` s inclui um parâmetro (`@MaxPrice`) e um parâmetro é adicionado à coleção de `SqlCommand` de `Parameters` s e recebe o valor da variável `price`.

Depois de adicionar esse código, reimplante o projeto SQL Server. Em seguida, retorne para SQL Server Management Studio e atualize a pasta procedimentos armazenados. Você deverá ver uma nova entrada, `GetProductsWithPriceLessThan`. Em uma janela de consulta, insira e execute o comando `exec GetProductsWithPriceLessThan 25`, que listará todos os produtos com menos de $25, como mostra a Figura 14.

[![produtos em $25 são exibidos](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image27.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image26.png)

**Figura 14**: produtos em $25 são exibidos ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image28.png))

## <a name="step-6-calling-the-managed-stored-procedure-from-the-data-access-layer"></a>Etapa 6: chamando o procedimento armazenado gerenciado da camada de acesso a dados

Neste ponto, adicionamos o `GetDiscontinuedProducts` e `GetProductsWithPriceLessThan` procedimentos armazenados gerenciados ao projeto de `ManagedDatabaseConstructs` e os registramos com o banco de dados do Northwind SQL Server. Também invocamos esses procedimentos armazenados gerenciados de SQL Server Management Studio (consulte as figuras 13 e 14). No entanto, para que nosso aplicativo ASP.NET use esses procedimentos armazenados gerenciados, precisamos adicioná-los às camadas de acesso a dados e lógica de negócios na arquitetura. Nesta etapa, adicionaremos dois novos métodos ao `ProductsTableAdapter` no conjunto de `NorthwindWithSprocs` tipado, que foi inicialmente criado no tutorial [criando novos procedimentos armazenados para o DataSets s tipados](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) . Na etapa 7, adicionaremos métodos correspondentes à BLL.

Abra o `NorthwindWithSprocs` DataSet tipado no Visual Studio e comece adicionando um novo método ao `ProductsTableAdapter` chamado `GetDiscontinuedProducts`. Para adicionar um novo método a um TableAdapter, clique com o botão direito do mouse no nome do TableAdapter s no designer e escolha a opção Adicionar consulta no menu de contexto.

> [!NOTE]
> Como mudamos o banco de dados Northwind da pasta `App_Data` para a instância do banco de dados SQL Server 2005 Express Edition, é imperativo que a cadeia de conexão correspondente no Web. config seja atualizada para refletir essa alteração. Na etapa 2, discutimos a atualização do valor de `NORTHWNDConnectionString` no `Web.config`. Se você esqueceu de fazer essa atualização, verá a mensagem de erro falha ao adicionar consulta. Não é possível localizar a `NORTHWNDConnectionString` de conexão para o objeto `Web.config` em uma caixa de diálogo ao tentar adicionar um novo método ao TableAdapter. Para resolver esse erro, clique em OK e, em seguida, vá para `Web.config` e atualize o valor `NORTHWNDConnectionString` conforme discutido na etapa 2. Em seguida, tente adicionar novamente o método ao TableAdapter. Desta vez, ele deve funcionar sem erros.

A adição de um novo método inicia o assistente de configuração de consulta do TableAdapter, que usamos muitas vezes nos tutoriais anteriores. A primeira etapa nos pede para especificar como o TableAdapter deve acessar o banco de dados: por meio de uma instrução SQL ad hoc ou por meio de um procedimento armazenado novo ou existente. Como já criamos e registramos o `GetDiscontinuedProducts` procedimento armazenado gerenciado com o banco de dados, escolha a opção usar procedimento armazenado existente e pressione Avançar.

[![escolher a opção usar procedimento armazenado existente](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image30.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image29.png)

**Figura 15**: escolha a opção usar procedimento armazenado existente ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image31.png))

A próxima tela nos solicitará o procedimento armazenado que o método invocará. Escolha o `GetDiscontinuedProducts` procedimento armazenado gerenciado na lista suspensa e clique em Avançar.

[![selecionar o procedimento armazenado gerenciado GetDiscontinuedProducts](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image33.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image32.png)

**Figura 16**: selecione o `GetDiscontinuedProducts` procedimento armazenado gerenciado ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image34.png))

Em seguida, solicitamos que especifiquemos se o procedimento armazenado retorna linhas, um único valor ou nada. Como `GetDiscontinuedProducts` retorna o conjunto de linhas de produtos descontinuadas, escolha a primeira opção (dados tabulares) e clique em Avançar.

[![selecionar a opção de dados tabulares](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image36.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image35.png)

**Figura 17**: selecione a opção dados tabulares ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image37.png))

A tela final do assistente nos permite especificar os padrões de acesso a dados usados e os nomes dos métodos resultantes. Deixe ambas as caixas de seleção marcadas e nomeie os métodos `FillByDiscontinued` e `GetDiscontinuedProducts`. Clique em Concluir para finalizar o assistente.

[![nomear os métodos FillByDiscontinued e GetDiscontinuedProducts](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image39.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image38.png)

**Figura 18**: nomear os métodos `FillByDiscontinued` e `GetDiscontinuedProducts` ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image40.png))

Repita essas etapas para criar métodos chamados `FillByPriceLessThan` e `GetProductsWithPriceLessThan` no `ProductsTableAdapter` para o procedimento armazenado gerenciado `GetProductsWithPriceLessThan`.

A Figura 19 mostra uma captura de tela do designer de DataSet depois de adicionar os métodos à `ProductsTableAdapter` para o `GetDiscontinuedProducts` e `GetProductsWithPriceLessThan` procedimentos armazenados gerenciados.

[![o ProductsTableAdapter inclui os novos métodos adicionados nesta etapa](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image42.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image41.png)

**Figura 19**: a `ProductsTableAdapter` inclui os novos métodos adicionados nesta etapa ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image43.png))

## <a name="step-7-adding-corresponding-methods-to-the-business-logic-layer"></a>Etapa 7: adicionando métodos correspondentes à camada de lógica de negócios

Agora que atualizamos a camada de acesso a dados para incluir métodos para chamar os procedimentos armazenados gerenciados adicionados nas etapas 4 e 5, precisamos adicionar métodos correspondentes à camada de lógica de negócios. Adicione os dois métodos a seguir à classe `ProductsBLLWithSprocs`:

[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample7.vb)]

Os dois métodos simplesmente chamam o método DAL correspondente e retornam a instância de `ProductsDataTable`. A marcação de `DataObjectMethodAttribute` acima de cada método faz com que esses métodos sejam incluídos na lista suspensa na guia selecionar do assistente de configuração de fonte de dados ObjectDataSource s.

## <a name="step-8-invoking-the-managed-stored-procedures-from-the-presentation-layer"></a>Etapa 8: invocar os procedimentos armazenados gerenciados da camada de apresentação

Com a lógica de negócios e as camadas de acesso a dados aumentadas para incluir suporte para chamar o `GetDiscontinuedProducts` e `GetProductsWithPriceLessThan` procedimentos armazenados gerenciados, agora podemos exibir esses resultados de procedimentos armazenados por meio de uma página ASP.NET.

Abra a página `ManagedFunctionsAndSprocs.aspx` na pasta `AdvancedDAL` e, na caixa de ferramentas, arraste um GridView para o designer. Defina a Propriedade GridView s `ID` como `DiscontinuedProducts` e, em sua marca inteligente, associe-a a um novo ObjectDataSource chamado `DiscontinuedProductsDataSource`. Configure o ObjectDataSource para efetuar pull de seus dados do método de `GetDiscontinuedProducts` da classe `ProductsBLLWithSprocs`.

[![configurar o ObjectDataSource para usar a classe ProductsBLLWithSprocs](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image45.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image44.png)

**Figura 20**: configurar o ObjectDataSource para usar a classe `ProductsBLLWithSprocs` ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image46.png))

[![escolha o método GetDiscontinuedProducts na lista suspensa na guia selecionar](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image48.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image47.png)

**Figura 21**: escolha o método `GetDiscontinuedProducts` na lista suspensa na guia selecionar ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image49.png))

Como essa grade será usada para exibir apenas as informações do produto, defina as listas suspensas nas guias atualizar, inserir e excluir como (nenhum) e clique em concluir.

Após a conclusão do assistente, o Visual Studio adicionará automaticamente um BoundField ou CheckBoxField para cada campo de dados na `ProductsDataTable`. Reserve um tempo para remover todos esses campos, exceto `ProductName` e `Discontinued`, no ponto em que a marcação declarativa de GridView e ObjectDataSource s deve ser semelhante ao seguinte:

[!code-aspx[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample8.aspx)]

Reserve um tempo para exibir esta página por meio de um navegador. Quando a página é visitada, o ObjectDataSource chama o método `ProductsBLLWithSprocs` Class `GetDiscontinuedProducts`. Como vimos na etapa 7, esse método chama para o método de `GetDiscontinuedProducts` da classe `ProductsDataTable` s da DAL, que invoca o procedimento armazenado `GetDiscontinuedProducts`. Esse procedimento armazenado é um procedimento armazenado gerenciado e executa o código que criamos na etapa 3, retornando os produtos descontinuados.

Os resultados retornados pelo procedimento armazenado gerenciado são empacotados em um `ProductsDataTable` pela DAL e, em seguida, retornados para a BLL, que os retorna para a camada de apresentação onde estão associados ao GridView e exibidos. Conforme esperado, a grade lista os produtos que foram descontinuados.

[![os produtos descontinuados estão listados](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image51.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image50.png)

**Figura 22**: os produtos descontinuados são listados ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image52.png))

Para uma prática adicional, adicione uma caixa de texto e outro GridView à página. Fazer com que este GridView exiba os produtos menores do que o valor inserido na caixa de texto chamando o método de `GetProductsWithPriceLessThan` da classe `ProductsBLLWithSprocs`.

## <a name="step-9-creating-and-calling-t-sql-udfs"></a>Etapa 9: Criando e chamando UDFs T-SQL

As funções definidas pelo usuário, ou UDFs, são objetos de banco de dados que imitam de forma semelhante a semântica das funções em linguagens de programação. Como uma função no Visual Basic, as UDFs podem incluir um número variável de parâmetros de entrada e retornar um valor de um tipo específico. Um UDF pode retornar dados escalares – uma cadeia de caracteres, um número inteiro e dados de tabela e assim por diante. Vamos dar uma olhada rápida em ambos os tipos de UDFs, começando com um UDF que retorna um tipo de dados escalar.

O UDF a seguir calcula o valor estimado do inventário para um produto específico. Ele faz isso executando três parâmetros de entrada: os valores `UnitPrice`, `UnitsInStock`e `Discontinued` para um produto específico – e retorna um valor do tipo `money`. Ele computa o valor estimado do inventário multiplicando o `UnitPrice` pelo `UnitsInStock`. Para itens descontinuados, esse valor é dividido em metade.

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample9.sql)]

Depois que esse UDF tiver sido adicionado ao banco de dados, ele poderá ser encontrado por meio de Management Studio expandindo a pasta de programação, as funções e as funções de valor escalar. Ele pode ser usado em uma consulta `SELECT` da seguinte forma:

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample10.sql)]

Adicionei o `udf_ComputeInventoryValue` UDF ao banco de dados Northwind; A Figura 23 mostra a saída da consulta de `SELECT` acima quando exibida por meio de Management Studio. Observe também que o UDF está listado na pasta funções Scalar-Value no Pesquisador de objetos.

[![os valores de inventário de cada produto estão listados](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image54.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image53.png)

**Figura 23**: cada produto s valores de inventário é listado ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image55.png))

As UDFs também podem retornar dados tabulares. Por exemplo, podemos criar um UDF que retorne os produtos que pertencem a uma determinada categoria:

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample11.sql)]

O `udf_GetProductsByCategoryID` UDF aceita um parâmetro de entrada `@CategoryID` e retorna os resultados da consulta de `SELECT` especificada. Depois de criado, esse UDF pode ser referenciado na cláusula `FROM` (ou `JOIN`) de uma consulta `SELECT`. O exemplo a seguir retornaria os valores `ProductID`, `ProductName`e `CategoryID` para cada uma das bebidas.

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample12.sql)]

Adicionei o `udf_GetProductsByCategoryID` UDF ao banco de dados Northwind; A figura 24 mostra a saída da consulta de `SELECT` acima quando exibida por meio de Management Studio. UDFs que retornam dados tabulares podem ser encontrados na pasta de funções Table-Value do pesquisador de objetos.

[![ProductID, ProductName e CategoryID são listados para cada bebida](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image57.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image56.png)

**Figura 24**: as `ProductID`, `ProductName`e `CategoryID` são listadas para cada bebida ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image58.png))

> [!NOTE]
> Para obter mais informações sobre como criar e usar UDFs, confira [introdução às funções definidas pelo usuário](http://www.sqlteam.com/item.asp?ItemID=1955). Além disso, Confira as [vantagens e desvantagens das funções definidas pelo usuário](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1).

## <a name="step-10-creating-a-managed-udf"></a>Etapa 10: Criando um UDF gerenciado

Os UDFs `udf_ComputeInventoryValue` e `udf_GetProductsByCategoryID` criados nos exemplos acima são objetos de banco de dados T-SQL. O SQL Server 2005 também dá suporte a UDFs gerenciadas, que podem ser adicionadas ao projeto `ManagedDatabaseConstructs` assim como os procedimentos armazenados gerenciados das etapas 3 e 5. Para esta etapa, vamos implementar o `udf_ComputeInventoryValue` UDF no código gerenciado.

Para adicionar um UDF gerenciado ao projeto `ManagedDatabaseConstructs`, clique com o botão direito do mouse no nome do projeto em Gerenciador de Soluções e escolha Adicionar um novo item. Selecione o modelo definido pelo usuário na caixa de diálogo Adicionar novo item e nomeie o novo arquivo UDF `udf_ComputeInventoryValue_Managed.vb`.

[![adicionar um novo UDF gerenciado ao projeto ManagedDatabaseConstructs](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image60.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image59.png)

**Figura 25**: adicionar um novo UDF gerenciado ao projeto `ManagedDatabaseConstructs` ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image61.png))

O modelo de função definido pelo usuário cria uma classe `Partial` chamada `UserDefinedFunctions` com um método cujo nome é o mesmo que o nome do arquivo de classe (`udf_ComputeInventoryValue_Managed`, nesta instância). Esse método é decorado usando o [atributo`SqlFunction`](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlfunctionattribute.aspx), que sinaliza o método como um UDF gerenciado.

[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample13.vb)]

O método `udf_ComputeInventoryValue` atualmente retorna um [objeto`SqlString`](https://msdn.microsoft.com/library/system.data.sqltypes.sqlstring.aspx) e não aceita nenhum parâmetro de entrada. Precisamos atualizar a definição do método para que ela aceite três parâmetros de entrada-`UnitPrice`, `UnitsInStock`e `Discontinued`-e retorna um objeto `SqlMoney`. A lógica para calcular o valor de inventário é idêntica à do `udf_ComputeInventoryValue` UDF T-SQL.

[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample14.vb)]

Observe que os parâmetros de entrada do método UDF são de seus tipos SQL correspondentes: `SqlMoney` para o campo `UnitPrice`, [`SqlInt16`](https://msdn.microsoft.com/library/system.data.sqltypes.sqlint16.aspx) para `UnitsInStock`e [`SqlBoolean`](https://msdn.microsoft.com/library/system.data.sqltypes.sqlboolean.aspx) para `Discontinued`. Esses tipos de dados refletem os tipos definidos na tabela `Products`: a coluna `UnitPrice` é do tipo `money`, a coluna `UnitsInStock` do tipo `smallint`e a coluna `Discontinued` do tipo `bit`.

O código começa criando uma instância de `SqlMoney` chamada `inventoryValue` que é atribuído a um valor de 0. A tabela `Products` permite valores de `NULL` de banco de dados nas colunas `UnitsInPrice` e `UnitsInStock`. Portanto, precisamos primeiro verificar se esses valores contêm `NULL` s, o que fazemos por meio da [propriedade`IsNull`](https://msdn.microsoft.com/library/system.data.sqltypes.sqlmoney.isnull.aspx)do objeto `SqlMoney`. Se `UnitPrice` e `UnitsInStock` contiverem valores não`NULL`, então computaremos o `inventoryValue` como o produto dos dois. Em seguida, se `Discontinued` for true, o valor será dividido em metade.

> [!NOTE]
> O objeto `SqlMoney` só permite que duas instâncias de `SqlMoney` sejam multiplicadas juntas. Ele não permite que uma instância de `SqlMoney` seja multiplicada por um número de ponto flutuante literal. Portanto, para metade `inventoryValue` multiplicamos por uma nova instância `SqlMoney` com o valor 0,5.

## <a name="step-11-deploying-the-managed-udf"></a>Etapa 11: Implantando o UDF gerenciado

Agora que o UDF gerenciado foi criado, estamos prontos para implantá-lo no banco de dados Northwind. Como vimos na etapa 4, os objetos gerenciados em um projeto SQL Server são implantados clicando com o botão direito do mouse no nome do projeto na Gerenciador de Soluções e escolhendo a opção implantar no menu de contexto.

Depois de implantar o projeto, retorne ao SQL Server Management Studio e atualize a pasta funções com valor escalar. Agora você deve ver duas entradas:

- `dbo.udf_ComputeInventoryValue`-o UDF do T-SQL criado na etapa 9 e
- `dbo.udf ComputeInventoryValue_Managed`-o UDF gerenciado criado na etapa 10 que acabou de ser implantado.

Para testar esse UDF gerenciado, execute a consulta a seguir no Management Studio:

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample15.sql)]

Esse comando usa o UDF gerenciado `udf ComputeInventoryValue_Managed` em vez do UDF do T-SQL `udf_ComputeInventoryValue`, mas a saída é a mesma. Consulte novamente a Figura 23 para ver uma captura de tela da saída de UDF.

## <a name="step-12-debugging-the-managed-database-objects"></a>Etapa 12: Depurando os objetos de banco de dados gerenciados

No tutorial [depuração de procedimentos armazenados](debugging-stored-procedures-vb.md) , discutimos as três opções de depuração de SQL Server por meio do Visual Studio: depuração de banco de dados direta, depuração de aplicativo e depuração de um projeto SQL Server. Os objetos de banco de dados gerenciados não podem ser depurados por meio da depuração direta do banco de dados, mas podem ser depurados de um aplicativo cliente e diretamente do projeto SQL Server. No entanto, para que a depuração funcione, o banco de dados SQL Server 2005 deve permitir a depuração SQL/CLR. Lembre-se de que, quando criamos o projeto `ManagedDatabaseConstructs`, o Visual Studio nos perguntou se queríamos habilitar a depuração SQL/CLR (consulte a Figura 6 na etapa 2). Essa configuração pode ser modificada clicando com o botão direito do mouse no banco de dados na janela Gerenciador de Servidores.

![Verifique se o banco de dados permite depuração SQL/CLR](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image62.png)

**Figura 26**: verificar se o banco de dados permite a depuração SQL/CLR

Imagine que queríamos depurar o procedimento armazenado gerenciado `GetProductsWithPriceLessThan`. Começamos definindo um ponto de interrupção dentro do código do método `GetProductsWithPriceLessThan`.

[![definir um ponto de interrupção no método GetProductsWithPriceLessThan](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image64.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image63.png)

**Figura 27**: definir um ponto de interrupção no método `GetProductsWithPriceLessThan` ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image65.png))

Vamos primeiro examinar a depuração dos objetos de banco de dados gerenciados do projeto SQL Server. Como nossa solução inclui dois projetos – o `ManagedDatabaseConstructs` SQL Server projeto junto com nosso site – para depurar a partir do projeto SQL Server, precisamos instruir o Visual Studio a iniciar o `ManagedDatabaseConstructs` projeto de SQL Server quando iniciarmos a depuração. Clique com o botão direito do mouse no projeto `ManagedDatabaseConstructs` no Gerenciador de Soluções e escolha a opção Definir como projeto de inicialização no menu de contexto.

Quando o projeto de `ManagedDatabaseConstructs` é iniciado a partir do depurador, ele executa as instruções SQL no arquivo `Test.sql`, localizado na pasta `Test Scripts`. Por exemplo, para testar o `GetProductsWithPriceLessThan` procedimento armazenado gerenciado, substitua o conteúdo do arquivo de `Test.sql` existente pela instrução a seguir, que invoca o procedimento armazenado gerenciado `GetProductsWithPriceLessThan` transmitindo o valor `@CategoryID` de 14,95:

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample16.sql)]

Depois de inserir o script acima em `Test.sql`, inicie a depuração acessando o menu Depurar e escolhendo iniciar depuração ou pressionando F5 ou o ícone de execução verde na barra de ferramentas. Isso criará os projetos dentro da solução, implantará os objetos de banco de dados gerenciados no banco de dados Northwind e, em seguida, executará o script de `Test.sql`. Nesse momento, o ponto de interrupção será atingido e poderemos percorrer o método `GetProductsWithPriceLessThan`, examinar os valores dos parâmetros de entrada e assim por diante.

[![o ponto de interrupção no método GetProductsWithPriceLessThan foi atingido](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image67.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image66.png)

**Figura 28**: o ponto de interrupção no método de `GetProductsWithPriceLessThan` foi atingido ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image68.png))

Para que um objeto de banco de dados SQL seja depurado por meio de um aplicativo cliente, é imperativo que o banco de dados seja configurado para dar suporte à depuração de aplicativos. Clique com o botão direito do mouse no banco de dados em Gerenciador de Servidores e verifique se a opção de depuração do aplicativo está marcada. Além disso, precisamos configurar o aplicativo ASP.NET para integrar com o depurador do SQL e desabilitar o pool de conexões. Essas etapas foram discutidas detalhadamente na etapa 2 do tutorial [depuração de procedimentos armazenados](debugging-stored-procedures-vb.md) .

Depois de configurar o aplicativo e o banco de dados do ASP.NET, defina o site do ASP.NET como o projeto de inicialização e inicie a depuração. Se você visitar uma página que chama um dos objetos gerenciados que tem um ponto de interrupção, o aplicativo será interrompido e o controle será ativado para o depurador, no qual você pode percorrer o código, conforme mostrado na Figura 28.

## <a name="step-13-manually-compiling-and-deploying-managed-database-objects"></a>Etapa 13: Compilando e Implantando manualmente objetos de banco de dados gerenciados

SQL Server projetos facilitam a criação, a compilação e a implantação de objetos de banco de dados gerenciados. Infelizmente, os projetos SQL Server só estão disponíveis nas edições Professional e Team Systems do Visual Studio. Se você estiver usando o Visual Web Developer ou a Standard Edition do Visual Studio e quiser usar objetos de banco de dados gerenciado, será necessário criá-los e implantá-los manualmente. Isso envolve quatro etapas:

1. Criar um arquivo que contém o código-fonte do objeto de banco de dados gerenciado,
2. Compilar o objeto em um assembly,
3. Registre o assembly com o banco de dados SQL Server 2005 e
4. Crie um objeto de banco de dados no SQL Server que aponte para o método apropriado no assembly.

Para ilustrar essas tarefas, vamos criar um novo procedimento armazenado gerenciado que retorne os produtos cujo `UnitPrice` seja maior que um valor especificado. Crie um novo arquivo em seu computador chamado `GetProductsWithPriceGreaterThan.vb` e insira o código a seguir no arquivo (você pode usar o Visual Studio, o bloco de notas ou qualquer editor de texto para fazer isso):

[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample17.vb)]

Esse código é praticamente idêntico ao do método `GetProductsWithPriceLessThan` criado na etapa 5. As únicas diferenças são os nomes dos métodos, a cláusula `WHERE` e o nome do parâmetro usado na consulta. De volta ao método `GetProductsWithPriceLessThan`, a cláusula `WHERE` lida: `WHERE UnitPrice < @MaxPrice`. Aqui, em `GetProductsWithPriceGreaterThan`, usamos: `WHERE UnitPrice > @MinPrice`.

Agora, precisamos compilar essa classe em um assembly. Na linha de comando, navegue até o diretório em que você salvou o arquivo de `GetProductsWithPriceGreaterThan.vb` e C# use o compilador (`csc.exe`) para compilar o arquivo de classe em um assembly:

[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample18.cmd)]

Se a pasta que contém v `bc.exe` não estiver na `PATH`do sistema, você terá que fazer referência completa ao caminho, `%WINDOWS%\Microsoft.NET\Framework\version\`, desta forma:

[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample19.cmd)]

[![compilar GetProductsWithPriceGreaterThan. vb em um assembly](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image70.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image69.png)

**Figura 29**: Compilar `GetProductsWithPriceGreaterThan.vb` em um assembly ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image71.png))

O sinalizador de `/t` especifica que o arquivo de classe de Visual Basic deve ser compilado em uma DLL (em vez de um executável). O sinalizador de `/out` especifica o nome do assembly resultante.

> [!NOTE]
> Em vez de compilar o arquivo de classe de `GetProductsWithPriceGreaterThan.vb` da linha de comando, você pode usar a [Visual Basic Express Edition](https://msdn.microsoft.com/vstudio/express/vb/) como alternativa ou criar um projeto de biblioteca de classes separado no Visual Studio Standard Edition. S Ren Jacob Lauritsen forneceu um projeto desse tipo Visual Basic Express Edition com código para o procedimento armazenado `GetProductsWithPriceGreaterThan` e os dois procedimentos armazenados gerenciados e o UDF criados nas etapas 3, 5 e 10. O projeto de s Ren inclui também os comandos T-SQL necessários para adicionar os objetos de banco de dados correspondentes.

Com o código compilado em um assembly, estamos prontos para registrar o assembly no banco de dados SQL Server 2005. Isso pode ser executado por meio do T-SQL, usando o comando `CREATE ASSEMBLY`ou por meio de SQL Server Management Studio. Deixe-nos concentrar-se no uso de Management Studio.

Em Management Studio, expanda a pasta programação no banco de dados Northwind. Uma de suas subpastas são assemblies. Para adicionar manualmente um novo assembly ao banco de dados, clique com o botão direito do mouse na pasta assemblies e escolha novo assembly no menu de contexto. Isso exibirá a caixa de diálogo novo assembly (consulte a figura 30). Clique no botão procurar, selecione o assembly `ManuallyCreatedDBObjects.dll` que acabamos de compilar e clique em OK para adicionar o assembly ao banco de dados. Você não deve ver o assembly `ManuallyCreatedDBObjects.dll` no Pesquisador de objetos.

[![adicionar o assembly ManuallyCreatedDBObjects. dll ao banco de dados](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image73.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image72.png)

**Figura 30**: Adicionar o assembly `ManuallyCreatedDBObjects.dll` ao banco de dados ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image74.png))

![O ManuallyCreatedDBObjects. dll é listado no Pesquisador de objetos](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image75.png)

**Figura 31**: a `ManuallyCreatedDBObjects.dll` está listada no Pesquisador de objetos

Embora tenhamos adicionado o assembly ao banco de dados Northwind, ainda estamos associados a um procedimento armazenado com o método `GetProductsWithPriceGreaterThan` no assembly. Para fazer isso, abra uma nova janela de consulta e execute o seguinte script:

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample20.sql)]

Isso cria um novo procedimento armazenado no banco de dados Northwind chamado `GetProductsWithPriceGreaterThan` e o associa ao método gerenciado `GetProductsWithPriceGreaterThan` (que está na classe `StoredProcedures`, que está no assembly `ManuallyCreatedDBObjects`).

Depois de executar o script acima, atualize a pasta procedimentos armazenados no Pesquisador de objetos. Você deverá ver uma nova entrada de procedimento armazenado-`GetProductsWithPriceGreaterThan`-que tem um ícone de bloqueio ao lado dele. Para testar esse procedimento armazenado, insira e execute o seguinte script na janela de consulta:

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample21.sql)]

Como mostra a Figura 32, o comando acima exibe informações para esses produtos com um `UnitPrice` maior que $24.95.

[![o ManuallyCreatedDBObjects. dll está listado no Pesquisador de objetos](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image77.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image76.png)

**Figura 32**: a `ManuallyCreatedDBObjects.dll` está listada no Pesquisador de objetos ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image78.png))

## <a name="summary"></a>Resumo

O Microsoft SQL Server 2005 fornece integração com o CLR (Common Language Runtime), que permite a criação de objetos de banco de dados usando código gerenciado. Anteriormente, esses objetos de banco de dados só podiam ser criados usando o T-SQL, mas agora podemos criar esses objetos usando linguagens de programação .NET como Visual Basic. Neste tutorial, criamos dois procedimentos armazenados gerenciados e uma função gerenciada definida pelo usuário.

O tipo de projeto SQL Server do Visual Studio s facilita a criação, a compilação e a implantação de objetos de banco de dados gerenciados. Além disso, ele oferece suporte à depuração avançada. No entanto, SQL Server tipos de projeto só estão disponíveis nas edições Professional e Team Systems do Visual Studio. Para aqueles que usam o Visual Web Developer ou a Standard Edition do Visual Studio, as etapas de criação, compilação e implantação devem ser executadas manualmente, como vimos na etapa 13.

Boa programação!

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Vantagens e desvantagens de funções definidas pelo usuário](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1)
- [Criando objetos SQL Server 2005 no código gerenciado](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [Criando gatilhos usando código gerenciado no SQL Server 2005](http://www.15seconds.com/issue/041006.htm)
- [Como: criar e executar um procedimento armazenado CLR SQL Server](https://msdn.microsoft.com/library/5czye81z(VS.80).aspx)
- [Como: criar e executar um CLR SQL Server função definida pelo usuário](https://msdn.microsoft.com/library/w2kae45k(VS.80).aspx)
- [Como: editar o script de `Test.sql` para executar objetos SQL](https://msdn.microsoft.com/library/ms233682(VS.80).aspx)
- [Introdução às funções definidas pelo usuário](http://www.sqlteam.com/item.asp?ItemID=1955)
- [Código gerenciado e SQL Server 2005 (vídeo)](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [Referência do Transact-SQL](https://msdn.microsoft.com/library/aa299742(SQL.80).aspx)
- [Walkthrough: Criando um procedimento armazenado no código gerenciado](https://msdn.microsoft.com/library/zxsa8hkf(VS.80).aspx)

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. O revisor de Lead para este tutorial foi S Ren Jacob Lauritsen. Além de examinar este artigo, S Ren também criou o projeto do C# Visual Express Edition incluído neste artigo S download para compilar manualmente os objetos de banco de dados gerenciado. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](debugging-stored-procedures-vb.md)
