---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-vb
title: Depuração de procedimentos armazenados (VB) | Microsoft Docs
author: rick-anderson
description: As edições Visual Studio Professional e Team System permitem que você defina pontos de interrupção e passe para procedimentos armazenados no SQL Server, tornando a depuração armazenada...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: 9ed8ccb5-5f31-4eb4-976d-cabf4b45ca09
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-vb
msc.type: authoredcontent
ms.openlocfilehash: 13d213ef4baf493a4f05a82daae8d2dc3b0aa61b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78551271"
---
# <a name="debugging-stored-procedures-vb"></a>Depuração de procedimentos armazenados (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_74_VB.zip) ou [baixar PDF](debugging-stored-procedures-vb/_static/datatutorial74vb1.pdf)

> As edições Visual Studio Professional e Team System permitem que você defina pontos de interrupção e passe para procedimentos armazenados no SQL Server, tornando os procedimentos armazenados de depuração tão fáceis quanto depurar o código do aplicativo. Este tutorial demonstra depuração direta de banco de dados e depuração de aplicativos de procedimentos armazenados.

## <a name="introduction"></a>Introdução

O Visual Studio fornece uma experiência de depuração rica. Com alguns pressionamentos de teclas ou cliques do mouse, é possível usar pontos de interrupção para interromper a execução de um programa e examinar seu estado e o fluxo de controle. Junto com o código do aplicativo de depuração, o Visual Studio oferece suporte para depuração de procedimentos armazenados de dentro de SQL Server. Assim como os pontos de interrupção podem ser definidos dentro do código de uma classe code-behind ASP.NET ou de camada de lógica de negócios, também podem ser colocados em procedimentos armazenados.

Neste tutorial, veremos a depuração em procedimentos armazenados do Gerenciador de Servidores no Visual Studio, bem como a definição de pontos de interrupção que são atingidos quando o procedimento armazenado é chamado do aplicativo ASP.NET em execução.

> [!NOTE]
> Infelizmente, os procedimentos armazenados só podem ser divididos em etapas e depurados por meio das versões Professional e Team Systems do Visual Studio. Se você estiver usando o Visual Web Developer ou a versão padrão do Visual Studio, você poderá ler ao longo das etapas necessárias para depurar procedimentos armazenados, mas não poderá replicar essas etapas em seu computador.

## <a name="sql-server-debugging-concepts"></a>Conceitos de depuração de SQL Server

Microsoft SQL Server 2005 foi projetado para fornecer integração com o [CLR (Common Language Runtime)](https://msdn.microsoft.com/netframework/aa497266.aspx), que é o tempo de execução usado por todos os assemblies do .net. Consequentemente, SQL Server 2005 dá suporte a objetos de banco de dados gerenciados. Ou seja, você pode criar objetos de banco de dados como procedimentos armazenados e UDFs (funções definidas pelo usuário) como métodos em uma classe de Visual Basic. Isso permite que esses procedimentos armazenados e UDFs utilizem a funcionalidade no .NET Framework e de suas próprias classes personalizadas. É claro que SQL Server 2005 também fornece suporte para objetos de banco de dados T-SQL.

O SQL Server 2005 oferece suporte à depuração para T-SQL e objetos de banco de dados gerenciados. No entanto, esses objetos só podem ser depurados por meio do Visual Studio 2005 Professional e das edições do Team Systems. Neste tutorial, examinaremos depuração de objetos de banco de dados T-SQL. O tutorial subsequente examina a depuração de objetos de banco de dados gerenciados.

A [visão geral da depuração do T-SQL e do CLR na](https://blogs.msdn.com/sqlclr/archive/2006/06/29/651644.aspx) entrada de blog do SQL Server 2005 da [equipe de integração do SQL Server 2005 CLR](https://blogs.msdn.com/sqlclr/default.aspx) realça as três maneiras de depurar SQL Server objetos 2005 do Visual Studio:

- A **depuração de banco de dados direto (DDD)** -de Gerenciador de servidores podemos entrar em qualquer objeto de banco de dados T-SQL, como procedimentos armazenados e UDFs. Examinaremos o DDD na etapa 1.
- **Depuração de aplicativo** – podemos definir pontos de interrupção dentro de um objeto de banco de dados e, em seguida, executar nosso aplicativo ASP.net. Quando o objeto de banco de dados for executado, o ponto de interrupção será atingido e controle passará para o depurador. Observe que, com a depuração de aplicativo, não podemos entrar em um objeto de banco de dados do código do aplicativo. Devemos definir explicitamente pontos de interrupção nesses procedimentos armazenados ou UDFs onde queremos que o depurador pare. A depuração de aplicativo é examinada a partir da etapa 2.
- A **depuração de um projeto SQL Server** -Visual Studio Professional e as edições de sistemas de equipe incluem um tipo de projeto de SQL Server que é comumente usado para criar objetos de banco de dados gerenciados. Examinaremos o uso de projetos SQL Server e depuraremos seu conteúdo no próximo tutorial.

O Visual Studio pode depurar procedimentos armazenados em instâncias de SQL Server locais e remotas. Uma instância de SQL Server local é aquela instalada no mesmo computador que o Visual Studio. Se o banco de dados SQL Server que você está usando não estiver localizado em seu computador de desenvolvimento, ele será considerado uma instância remota. Para esses tutoriais, estamos usando instâncias de SQL Server locais. A depuração de procedimentos armazenados em uma instância remota do SQL Server requer mais etapas de configuração do que ao depurar procedimentos armazenados em uma instância local.

Se você estiver usando uma instância de SQL Server local, poderá começar com a etapa 1 e trabalhar com este tutorial no final. Se você estiver usando uma instância de SQL Server remota, no entanto, primeiro precisará garantir que, durante a depuração, você esteja conectado ao computador de desenvolvimento com uma conta de usuário do Windows que tenha um logon de SQL Server na instância remota. Além disso, o logon do banco de dados e o logon do banco de dados usados para se conectar ao banco de dados do aplicativo ASP.NET em execução devem ser membros da função `sysadmin`. Consulte a seção depuração de objetos do banco de dados T-SQL em instâncias remotas no final deste tutorial para obter mais informações sobre como configurar o Visual Studio e SQL Server para depurar uma instância remota.

Por fim, entenda que o suporte de depuração para objetos de banco de dados T-SQL não é tão rico em recursos como suporte à depuração para aplicativos .NET. Por exemplo, não há suporte para condições e filtros de ponto de interrupção, apenas um subconjunto das janelas de depuração estão disponíveis, você não pode usar editar e continuar, a janela imediata é renderizada inúteis e assim por diante. Confira [limitações nos comandos e recursos do depurador](https://msdn.microsoft.com/library/ms165035(VS.80).aspx) para obter mais informações.

## <a name="step-1-directly-stepping-into-a-stored-procedure"></a>Etapa 1: entrando diretamente em um procedimento armazenado

O Visual Studio facilita a depuração direta de um objeto de banco de dados. Vamos examinar como usar o recurso DDD (depuração de banco de dados direto) para entrar no procedimento armazenado `Products_SelectByCategoryID` no banco de dados Northwind. Como o nome indica, `Products_SelectByCategoryID` retorna informações do produto para uma determinada categoria; Ele foi criado no tutorial [usando os procedimentos armazenados existentes para os TableAdapters do DataSet s tipados](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) . Comece indo para a Gerenciador de Servidores e expanda o nó do banco de dados Northwind. Em seguida, faça uma busca detalhada na pasta procedimentos armazenados, clique com o botão direito do mouse no procedimento armazenado `Products_SelectByCategoryID` e escolha a opção etapa em procedimento armazenado no menu de contexto. Isso iniciará o depurador.

Como o procedimento armazenado `Products_SelectByCategoryID` espera um parâmetro de entrada `@CategoryID`, é solicitado que forneçamos esse valor. Insira 1, que retornará informações sobre as bebidas.

![Use o valor 1 para o parâmetro @CategoryID](debugging-stored-procedures-vb/_static/image1.png)

**Figura 1**: usar o valor 1 para o parâmetro `@CategoryID`

Depois de fornecer o valor para o parâmetro `@CategoryID`, o procedimento armazenado é executado. Em vez de executar até a conclusão, no entanto, o depurador interrompe a execução na primeira instrução. Observe a seta amarela na margem, indicando o local atual no procedimento armazenado. Você pode exibir e editar valores de parâmetro por meio da janela Inspeção ou passando o mouse sobre o nome do parâmetro no procedimento armazenado.

[![o depurador foi interrompido na primeira instrução do procedimento armazenado](debugging-stored-procedures-vb/_static/image3.png)](debugging-stored-procedures-vb/_static/image2.png)

**Figura 2**: o depurador foi interrompido na primeira instrução do procedimento armazenado ([clique para exibir a imagem em tamanho normal](debugging-stored-procedures-vb/_static/image4.png))

Para percorrer o procedimento armazenado uma instrução de cada vez, clique no botão passar sobre na barra de ferramentas ou pressione a tecla F10. O procedimento armazenado `Products_SelectByCategoryID` contém uma única instrução `SELECT`, portanto pressionar F10 passará por uma única instrução e concluirá a execução do procedimento armazenado. Depois que o procedimento armazenado for concluído, sua saída será exibida na janela de saída e o depurador será encerrado.

> [!NOTE]
> A depuração T-SQL ocorre no nível de instrução; Não é possível entrar em uma instrução `SELECT`.

## <a name="step-2-configuring-the-website-for-application-debugging"></a>Etapa 2: Configurando o site para depuração de aplicativo

Embora a depuração de um procedimento armazenado diretamente da Gerenciador de Servidores seja útil, em muitos cenários, estamos mais interessados em depurar o procedimento armazenado quando ele é chamado de nosso aplicativo ASP.NET. Podemos adicionar pontos de interrupção a um procedimento armazenado de dentro do Visual Studio e, em seguida, iniciar a depuração do aplicativo ASP.NET. Quando um procedimento armazenado com pontos de interrupção é invocado a partir do aplicativo, a execução será interrompida no ponto de interrupção e poderemos exibir e alterar os valores de parâmetro s do procedimento armazenado e percorrer suas instruções, exatamente como fizemos na etapa 1.

Antes que possamos iniciar a depuração de procedimentos armazenados chamados do aplicativo, precisamos instruir o aplicativo Web ASP.NET a integrar com o depurador de SQL Server. Comece clicando com o botão direito do mouse no nome do site na Gerenciador de Soluções (`ASPNET_Data_Tutorial_74_VB`). Escolha a opção páginas de propriedades no menu de contexto, selecione o item iniciar opções à esquerda e marque a caixa de seleção SQL Server na seção depuradores (veja a Figura 3).

[![marque a caixa de seleção SQL Server nas páginas de propriedades do aplicativo](debugging-stored-procedures-vb/_static/image6.png)](debugging-stored-procedures-vb/_static/image5.png)

**Figura 3**: marque a caixa de seleção SQL Server nas páginas de propriedades do aplicativo ([clique para exibir a imagem em tamanho normal](debugging-stored-procedures-vb/_static/image7.png))

Além disso, precisamos atualizar a cadeia de conexão do banco de dados usada pelo aplicativo para que o pool de conexões seja desabilitado. Quando uma conexão com um banco de dados é fechada, o objeto de `SqlConnection` correspondente é colocado em um pool de conexões disponíveis. Ao estabelecer uma conexão com um banco de dados, um objeto de conexão disponível pode ser recuperado desse pool em vez de ter que criar e estabelecer uma nova conexão. Esse pool de objetos de conexão é um aprimoramento de desempenho e é habilitado por padrão. No entanto, ao depurar, desejamos desativar o pool de conexões porque a infraestrutura de depuração não é restabelecida corretamente ao trabalhar com uma conexão tirada do pool.

Para desabilitar o pool de conexões, atualize o `NORTHWNDConnectionString` em `Web.config` para que ele inclua a configuração `Pooling=false`.

[!code-xml[Main](debugging-stored-procedures-vb/samples/sample1.xml)]

> [!NOTE]
> Depois de concluir a depuração SQL Server por meio do aplicativo ASP.NET, certifique-se de restabelecer o pool de conexões removendo a configuração de `Pooling` da cadeia de conexão (ou definindo-a como `Pooling=true`).

Neste ponto, o aplicativo ASP.NET foi configurado para permitir que o Visual Studio depure SQL Server objetos de banco de dados quando chamados por meio do aplicativo Web. Tudo o que resta agora é adicionar um ponto de interrupção a um procedimento armazenado e iniciar a depuração!

## <a name="step-3-adding-a-breakpoint-and-debugging"></a>Etapa 3: adicionando um ponto de interrupção e Depurando

Abra o procedimento armazenado `Products_SelectByCategoryID` e defina um ponto de interrupção no início da instrução `SELECT` clicando na margem no local apropriado ou colocando o cursor no início da instrução `SELECT` e atingindo F9. Como a Figura 4 ilustra, o ponto de interrupção aparece como um círculo vermelho na margem.

[![definir um ponto de interrupção no procedimento armazenado Products_SelectByCategoryID](debugging-stored-procedures-vb/_static/image9.png)](debugging-stored-procedures-vb/_static/image8.png)

**Figura 4**: definir um ponto de interrupção no procedimento armazenado `Products_SelectByCategoryID` ([clique para exibir a imagem em tamanho normal](debugging-stored-procedures-vb/_static/image10.png))

Para que um objeto de banco de dados SQL seja depurado por meio de um aplicativo cliente, é imperativo que o banco de dados seja configurado para dar suporte à depuração de aplicativos. Quando você define um ponto de interrupção pela primeira vez, essa configuração deve ser ativada automaticamente, mas é prudente fazer uma verificação dupla. Clique com o botão direito do mouse no nó `NORTHWND.MDF` na Gerenciador de Servidores. O menu de contexto deve incluir um item de menu selecionado chamado depuração de aplicativo.

![Verifique se a opção de depuração do aplicativo está habilitada](debugging-stored-procedures-vb/_static/image11.png)

**Figura 5**: verificar se a opção de depuração do aplicativo está habilitada

Com o ponto de interrupção definido e a opção de depuração de aplicativo habilitada, estamos prontos para depurar o procedimento armazenado quando chamado do aplicativo ASP.NET. Inicie o depurador acessando o menu Depurar e escolhendo iniciar depuração, pressionando F5 ou clicando no ícone de execução verde na barra de ferramentas. Isso iniciará o depurador e iniciará o site.

O procedimento armazenado `Products_SelectByCategoryID` foi criado no tutorial [usando os procedimentos armazenados existentes para o DataSet s tipados](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) . Sua página da Web correspondente (`~/AdvancedDAL/ExistingSprocs.aspx`) contém um GridView que exibe os resultados retornados por esse procedimento armazenado. Visite esta página por meio do navegador. Ao chegar à página, o ponto de interrupção na `Products_SelectByCategoryID` procedimento armazenado será atingido e o controle retornado ao Visual Studio. Assim como na etapa 1, você pode percorrer as instruções do procedimento armazenado s e exibir e modificar os valores de parâmetro.

[![a página ExistingSprocs. aspx inicialmente exibirá as bebidas](debugging-stored-procedures-vb/_static/image13.png)](debugging-stored-procedures-vb/_static/image12.png)

**Figura 6**: a página de `ExistingSprocs.aspx` exibe inicialmente as bebidas ([clique para exibir a imagem em tamanho normal](debugging-stored-procedures-vb/_static/image14.png))

[![o ponto de interrupção s do procedimento armazenado foi atingido](debugging-stored-procedures-vb/_static/image16.png)](debugging-stored-procedures-vb/_static/image15.png)

**Figura 7**: o ponto de interrupção s do procedimento armazenado foi atingido ([clique para exibir a imagem em tamanho normal](debugging-stored-procedures-vb/_static/image17.png))

Como mostra o janela Inspeção na Figura 7, o valor do parâmetro `@CategoryID` é 1. Isso ocorre porque a página de `ExistingSprocs.aspx` exibe inicialmente os produtos na categoria bebidas, que tem um valor de `CategoryID` de 1. Escolha uma categoria diferente na lista suspensa. Isso causa um postback e executa novamente o procedimento armazenado `Products_SelectByCategoryID`. O ponto de interrupção é atingido novamente, mas desta vez o valor do `@CategoryID` parâmetro s reflete o item de lista suspensa selecionado s `CategoryID`.

[![escolher uma categoria diferente na lista suspensa](debugging-stored-procedures-vb/_static/image19.png)](debugging-stored-procedures-vb/_static/image18.png)

**Figura 8**: escolha uma categoria diferente na lista suspensa ([clique para exibir a imagem em tamanho normal](debugging-stored-procedures-vb/_static/image20.png))

[![o parâmetro @CategoryID reflete a categoria selecionada na página da Web](debugging-stored-procedures-vb/_static/image22.png)](debugging-stored-procedures-vb/_static/image21.png)

**Figura 9**: o parâmetro `@CategoryID` reflete a categoria selecionada na página da Web ([clique para exibir a imagem em tamanho normal](debugging-stored-procedures-vb/_static/image23.png))

> [!NOTE]
> Se o ponto de interrupção no procedimento armazenado `Products_SelectByCategoryID` não for atingido ao visitar a página `ExistingSprocs.aspx`, verifique se a caixa de seleção SQL Server foi marcada na seção depuradores da página de propriedades do aplicativo ASP.NET, se o pool de conexões foi desabilitado e se a opção de depuração do aplicativo Database s está habilitada. Se você ainda tiver problemas, reinicie o Visual Studio e tente novamente.

## <a name="debugging-t-sql-database-objects-on-remote-instances"></a>Depurando objetos de banco de dados T-SQL em instâncias remotas

A depuração de objetos de banco de dados por meio do Visual Studio é bastante simples quando a instância do banco de dados SQL Server está no mesmo computador que o Visual Studio. No entanto, se SQL Server e o Visual Studio residirem em máquinas diferentes, uma configuração cuidadosa será necessária para que tudo funcione corretamente. Há duas tarefas principais com as quais estamos enfrentando:

- Verifique se o logon usado para se conectar ao banco de dados via ADO.NET pertence à função de `sysadmin`.
- Verifique se a conta de usuário do Windows usada pelo Visual Studio no computador de desenvolvimento é uma conta de logon SQL Server válida que pertence à função `sysadmin`.

A primeira etapa é relativamente simples. Primeiro, identifique a conta de usuário usada para se conectar ao banco de dados do aplicativo ASP.NET e, em seguida, em SQL Server Management Studio, adicione essa conta de logon à função `sysadmin`.

A segunda tarefa requer que a conta de usuário do Windows usada para depurar o aplicativo seja um logon válido no banco de dados remoto. No entanto, é provável que a conta do Windows na qual você fez logon na estação de trabalho não seja um logon válido no SQL Server. Em vez de adicionar sua conta de logon específica a SQL Server, uma opção melhor seria designar uma conta de usuário do Windows como a conta de depuração de SQL Server. Em seguida, para depurar os objetos de banco de dados de uma instância de SQL Server remota, você executaria o Visual Studio usando as credenciais da conta de logon do Windows.

Um exemplo deve ajudar a esclarecer as coisas. Imagine que haja uma conta do Windows chamada `SQLDebug` no domínio do Windows. Essa conta precisa ser adicionada à instância de SQL Server remota como um logon válido e como um membro da função `sysadmin`. Em seguida, para depurar a instância de SQL Server remota do Visual Studio, precisaremos executar o Visual Studio como o usuário `SQLDebug`. Isso pode ser feito fazendo logout de nossa estação de trabalho, fazendo logon novamente como `SQLDebug`e, em seguida, iniciando o Visual Studio, mas uma abordagem mais simples seria fazer logon em nossa estação de trabalho usando nossas próprias credenciais e, em seguida, usar `runas.exe` para iniciar o Visual Studio como o usuário `SQLDebug`. `runas.exe` permite que um aplicativo específico seja executado sob o forma de uma conta de usuário diferente. Para iniciar o Visual Studio como `SQLDebug`, você pode inserir a seguinte instrução na linha de comando:

[!code-console[Main](debugging-stored-procedures-vb/samples/sample2.cmd)]

Para obter uma explicação mais detalhada sobre esse processo, consulte [William R. Vaughn](http://betav.com/BLOG/billva/) s *Hitchhiker ' s s Guide to Visual Studio and SQL Server, sétima Edition,* bem como [definir permissões de SQL Server para depuração](https://msdn.microsoft.com/library/w1bhybwz(VS.80).aspx).

> [!NOTE]
> Se o computador de desenvolvimento estiver executando o Windows XP Service Pack 2, você precisará configurar o firewall de conexão com a Internet para permitir a depuração remota. [O artigo como habilitar a depuração SQL Server 2005](https://msdn.microsoft.com/library/s0fk6z6e(VS.80).aspx) , que envolve duas etapas: (a) no computador host do Visual Studio, você deve adicionar `Devenv.exe` à lista de exceções e abrir a porta TCP 135; e (b) na máquina remota (SQL), você deve abrir a porta TCP 135 e adicionar `sqlservr.exe` à lista de exceções. Se sua política de domínio exigir que a comunicação de rede seja feita por meio do IPSec, você deverá abrir as portas UDP 4500 e UDP 500.

## <a name="summary"></a>Resumo

Além de fornecer suporte de depuração para o código do aplicativo .NET, o Visual Studio também fornece uma variedade de opções de depuração para o SQL Server 2005. Neste tutorial, examinamos duas dessas opções: depuração direta de banco de dados e depuração de aplicativo. Para depurar diretamente um objeto de banco de dados T-SQL, localize o objeto por meio do Gerenciador de Servidores clique com o botão direito do mouse nele e escolha Step Into. Isso inicia o depurador e é interrompido na primeira instrução no objeto de banco de dados, em que ponto você pode percorrer as instruções do objeto s e exibir e modificar os valores de parâmetro. Na etapa 1, usamos essa abordagem para entrar no procedimento armazenado `Products_SelectByCategoryID`.

A depuração de aplicativo permite que os pontos de interrupção sejam definidos diretamente dentro dos objetos de banco de dados. Quando um objeto de banco de dados com pontos de interrupção é invocado de um aplicativo cliente (como um aplicativo Web ASP.NET), o programa é interrompido conforme o depurador assume. A depuração de aplicativo é útil porque mostra mais claramente qual ação de aplicativo faz com que um determinado objeto de banco de dados seja invocado. No entanto, ele requer um pouco mais de configuração e instalação do que a depuração direta do banco de dados.

Os objetos de banco de dados também podem ser depurados por meio de projetos SQL Server. Veremos como usar SQL Server projetos e como usá-los para criar e depurar objetos de banco de dados gerenciados no próximo tutorial.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](protecting-connection-strings-and-other-configuration-information-vb.md)
> [Próximo](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb.md)
