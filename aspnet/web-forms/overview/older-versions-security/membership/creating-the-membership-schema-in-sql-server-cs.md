---
uid: web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs
title: Criando o esquema de associação em SQL ServerC#() | Microsoft Docs
author: rick-anderson
description: Este tutorial começa examinando as técnicas para adicionar o esquema necessário ao banco de dados para usar o SqlMembershipProvider. Depois disso, nós Wi...
ms.author: riande
ms.date: 01/18/2008
ms.assetid: b4ac129d-1b8e-41ca-a38f-9b19d7c7bb0e
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs
msc.type: authoredcontent
ms.openlocfilehash: 97623e7c13ab7799b9dadbb8e52be8e0cd99e252
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78575330"
---
# <a name="creating-the-membership-schema-in-sql-server-c"></a>Criação do esquema de associação no SQL Server (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_04_CS.zip) ou [baixar PDF](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial04_MembershipSetup_cs.pdf)

> Este tutorial começa examinando as técnicas para adicionar o esquema necessário ao banco de dados para usar o SqlMembershipProvider. Depois disso, examinaremos as tabelas-chave no esquema e discutiremos sua finalidade e importância. Este tutorial termina com uma visão de como informar a um aplicativo ASP.NET qual provedor deve ser usado pela estrutura de associação.

## <a name="introduction"></a>Introdução

Os dois tutoriais anteriores examinaram o uso da autenticação de formulários para identificar os visitantes do site. A estrutura de autenticação de formulários torna mais fácil para os desenvolvedores registrar um usuário em um site e se lembrar deles entre as visitas à página por meio do uso de tíquetes de autenticação. A classe `FormsAuthentication` inclui métodos para gerar o tíquete e adicioná-lo aos cookies do visitante. O `FormsAuthenticationModule` examina todas as solicitações de entrada e, para aqueles com um tíquete de autenticação válido, o cria e associa um `GenericPrincipal` e um objeto `FormsIdentity` com a solicitação atual. A autenticação de formulários é meramente um mecanismo para conceder um tíquete de autenticação a um visitante ao fazer logon e, em solicitações subsequentes, analisar esse tíquete para determinar a identidade do usuário. Para que um aplicativo Web dê suporte a contas de usuário, ainda precisamos implementar um repositório de usuários e adicionar funcionalidade para validar credenciais, registrar novos usuários e a variedade de outras tarefas relacionadas à conta de usuário.

Antes do ASP.NET 2,0, os desenvolvedores estavam no gancho para implementar todas essas tarefas relacionadas à conta de usuário. Felizmente, a equipe de ASP.NET reconheceu essa deficiência e introduziu a estrutura de associação com o ASP.NET 2,0. A estrutura de associação é um conjunto de classes no .NET Framework que fornecem uma interface programática para realizar tarefas relacionadas à conta de usuário principal. Essa estrutura é criada sobre o [modelo de provedor](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), que permite aos desenvolvedores conectar uma implementação personalizada em uma API padronizada.

Conforme discutido no tutorial <a id="Tutorial1"> </a>de [*suporte básico de segurança e ASP.net*](../introduction/security-basics-and-asp-net-support-cs.md) , o .NET Framework é fornecido com dois provedores de associação internos: [`ActiveDirectoryMembershipProvider`](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) e [`SqlMembershipProvider`](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx). Como o nome indica, o `SqlMembershipProvider` usa um banco de dados Microsoft SQL Server como o armazenamento do usuário. Para usar esse provedor em um aplicativo, precisamos informar ao provedor qual banco de dados usar como repositório. Como você pode imaginar, o `SqlMembershipProvider` espera que o banco de dados de armazenamento do usuário tenha determinadas tabelas, exibições e procedimentos armazenados do banco de dados. Precisamos adicionar esse esquema esperado ao banco de dados selecionado.

Este tutorial começa examinando as técnicas para adicionar o esquema necessário ao banco de dados para usar o `SqlMembershipProvider`. Depois disso, examinaremos as tabelas-chave no esquema e discutiremos sua finalidade e importância. Este tutorial termina com uma visão de como informar a um aplicativo ASP.NET qual provedor deve ser usado pela estrutura de associação.

Vamos começar!

## <a name="step-1-deciding-where-to-place-the-user-store"></a>Etapa 1: decidindo onde posicionar o armazenamento do usuário

Os dados de um aplicativo ASP.NET são normalmente armazenados em várias tabelas em um banco de dados. Ao implementar o esquema de banco de dados de `SqlMembershipProvider`, devemos decidir se o esquema de associação deve ser colocado no mesmo banco de dados do aplicativo ou em um banco de dado alternativo.

É recomendável localizar o esquema de associação no mesmo banco de dados que o aplicativo pelos seguintes motivos:

- A **manutenção** de um aplicativo cujos dados são encapsulados em um banco de dado é mais fácil de entender, manter e implantar do que um aplicativo que tem dois bancos de dados separados.
- A **integridade relacional** localizando as tabelas relacionadas à associação no mesmo banco de dados que as tabelas de aplicativo é possível estabelecer [restrições Foreign Key](http://en.wikipedia.org/wiki/Foreign_key) entre as chaves primárias nas tabelas relacionadas à associação e tabelas de aplicativo relacionadas.

O desacoplamento do armazenamento do usuário e dos dados do aplicativo em bancos de dado separados só faz sentido se você tiver vários aplicativos que usam, cada um, um banco de dados separado, mas precisará compartilhar um repositório de usuários comum.

### <a name="creating-a-database"></a>Criar um banco de dados

O aplicativo que estávamos criando desde o segundo tutorial ainda não precisava de um banco de dados. Precisamos de um agora, no entanto, para o armazenamento do usuário. Vamos criar um e, em seguida, adicionar a ele o esquema exigido pelo provedor de `SqlMembershipProvider` (consulte a etapa 2).

> [!NOTE]
> Ao longo desta série de tutoriais, usaremos um banco de dados [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx) para armazenar as tabelas de aplicativos e o esquema de `SqlMembershipProvider`. Essa decisão foi feita por dois motivos: primeiro, devido a seu custo livre, a Express Edition é a versão mais legível do SQL Server 2005; em segundo lugar, os bancos de dados do SQL Server 2005 Express Edition podem ser colocados diretamente na pasta `App_Data` do aplicativo Web, tornando-o um fácil para empacotar o banco de dados e o aplicativo Web em um arquivo ZIP e reimplantá-lo sem quaisquer instruções de configuração ou opções de instalação especiais. Se você preferir acompanhar o uso de uma versão de edição não Express do SQL Server, sinta-se à vontade. As etapas são praticamente idênticas. O esquema de `SqlMembershipProvider` funcionará com qualquer versão do Microsoft SQL Server 2000 e superior.

Na Gerenciador de Soluções, clique com o botão direito do mouse na pasta `App_Data` e escolha Adicionar novo item. (Se você não vir uma pasta `App_Data` em seu projeto, clique com o botão direito do mouse no projeto em Gerenciador de Soluções, selecione Adicionar pasta ASP.NET e escolha `App_Data`.) Na caixa de diálogo Adicionar novo item, escolha Adicionar um novo banco de dados SQL chamado `SecurityTutorials.mdf`. Neste tutorial, adicionaremos o esquema de `SqlMembershipProvider` a esse banco de dados; nos tutoriais subsequentes, criaremos tabelas adicionais para capturar os dados do aplicativo.

[![adicionar um novo banco de dados SQL chamado SecurityTutorials. MDF Database à pasta App_Data](creating-the-membership-schema-in-sql-server-cs/_static/image2.png)](creating-the-membership-schema-in-sql-server-cs/_static/image1.png)

**Figura 1**: adicionar um novo banco de dados SQL chamado `SecurityTutorials.mdf` banco de dados à pasta `App_Data` ([clique para exibir a imagem em tamanho normal](creating-the-membership-schema-in-sql-server-cs/_static/image3.png))

Adicionar um banco de dados à pasta `App_Data` a inclui automaticamente na exibição Gerenciador de Banco de Dados. (Na versão de edição não Express do Visual Studio, o Gerenciador de Banco de Dados é chamado de Gerenciador de Servidores.) Vá para a Gerenciador de Banco de Dados e expanda o banco de dados recém-adicionado `SecurityTutorials`. Se você não vir o Gerenciador de Banco de Dados na tela, vá para o menu exibir e escolha Gerenciador de Banco de Dados ou pressione CTRL + ALT + S. Como mostra a Figura 2, o banco de dados `SecurityTutorials` está vazio-ele não contém tabelas, nenhuma exibição e nenhum procedimento armazenado.

[![o banco de dados SecurityTutorials está vazio no momento](creating-the-membership-schema-in-sql-server-cs/_static/image5.png)](creating-the-membership-schema-in-sql-server-cs/_static/image4.png)

**Figura 2**: o banco de dados `SecurityTutorials` está vazio no momento ([clique para exibir a imagem em tamanho normal](creating-the-membership-schema-in-sql-server-cs/_static/image6.png))

## <a name="step-2-adding-thesqlmembershipproviderschema-to-the-database"></a>Etapa 2: adicionando o esquema de`SqlMembershipProvider`ao banco de dados

O `SqlMembershipProvider` requer um conjunto específico de tabelas, exibições e procedimentos armazenados a serem instalados no banco de dados de armazenamento do usuário. Esses objetos de banco de dados de requisito podem ser adicionados usando a [ferramenta de`aspnet_regsql.exe`](https://msdn.microsoft.com/library/ms229862.aspx). Esse arquivo está localizado na pasta `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\`.

> [!NOTE]
> A ferramenta de `aspnet_regsql.exe` oferece a funcionalidade de linha de comando e uma interface gráfica do usuário. A interface gráfica é mais amigável para o usuário e é o que iremos examinar neste tutorial. A interface de linha de comando é útil quando a adição do esquema de `SqlMembershipProvider` precisa ser automatizada, como em scripts de compilação ou cenários de teste automatizados.

A ferramenta de `aspnet_regsql.exe` é usada para adicionar ou remover *serviços de aplicativos ASP.net* para um banco de dados SQL Server especificado. Os serviços de aplicativo ASP.NET englobam os esquemas para o `SqlMembershipProvider` e `SqlRoleProvider`, juntamente com os esquemas para os provedores baseados em SQL para outras estruturas ASP.NET 2,0. Precisamos fornecer dois bits de informações para a ferramenta de `aspnet_regsql.exe`:

- Se desejamos adicionar ou remover serviços de aplicativo e
- O banco de dados do qual adicionar ou remover o esquema de serviços de aplicativos

Ao solicitar o banco de dados a ser usado, a ferramenta `aspnet_regsql.exe` solicita que forneçam o nome do servidor no qual o banco de dados reside, as credenciais de segurança para conexão com o banco de dados e o nome do banco de dados. Se você estiver usando a edição não Express do SQL Server, você já deve conhecer essas informações, pois são as mesmas informações que você deve fornecer por meio de uma cadeia de conexão ao trabalhar com o banco de dados por meio de uma página da Web do ASP.NET. No entanto, determinar o nome do servidor e do banco de dados ao usar um SQL Server 2005 Express Edition banco de dados na pasta `App_Data` é um pouco mais envolvido.

A seção a seguir examina uma maneira simples de especificar o nome do servidor e do banco de dados para um SQL Server 2005 Express Edition banco de dados na pasta `App_Data`. Se você não estiver usando SQL Server 2005 Express Edition fique à vontade para pular para a seção Instalando a Serviços de Aplicativos.

### <a name="determining-the-server-and-database-name-for-a-sql-server-2005-express-edition-database-in-theapp_datafolder"></a>Determinando o nome do servidor e do banco de dados para um banco de dados SQL Server 2005 Express Edition na pasta`App_Data`

Para usar a ferramenta de `aspnet_regsql.exe`, precisamos saber os nomes de servidor e de banco de dados. O nome do servidor é `localhost\InstanceName`. Provavelmente, *InstanceName* é `SQLExpress`. No entanto, se você instalou o SQL Server 2005 Express Edition manualmente (ou seja, você não o instalou automaticamente durante a instalação do Visual Studio), é possível que você tenha selecionado um nome de instância diferente.

O nome do banco de dados é um pouco mais complicado de determinar. Os bancos de dados na pasta `App_Data` normalmente têm um nome de banco que inclui um [identificador global exclusivo](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) junto com o caminho para o arquivo de banco de dados. Precisamos determinar esse nome de banco de dados para adicionar o esquema de serviços de aplicativos por meio de `aspnet_regsql.exe`.

A maneira mais fácil de verificar o nome do banco de dados é examiná-lo por meio de SQL Server Management Studio. O SQL Server Management Studio fornece uma interface gráfica para gerenciar bancos de dados do SQL Server 2005, mas ele não é fornecido com a Express Edition do SQL Server 2005. A boa notícia é que [você pode baixar](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) a Express Edition gratuita do SQL Server Management Studio.

> [!NOTE]
> Se você também tiver uma versão de edição não Express do SQL Server 2005 instalada em sua área de trabalho, a versão completa do Management Studio provavelmente será instalada. Você pode usar a versão completa para determinar o nome do banco de dados, seguindo as mesmas etapas descritas abaixo para a edição Express.

Comece fechando o Visual Studio para garantir que todos os bloqueios impostos pelo Visual Studio no arquivo de banco de dados sejam fechados. Em seguida, inicie SQL Server Management Studio e conecte-se ao banco de dados `localhost\InstanceName` para SQL Server 2005 Express Edition. Conforme observado anteriormente, é provável que o nome da instância seja `SQLExpress`. Para a opção de autenticação, selecione Autenticação do Windows.

[![conectar-se à instância de SQL Server 2005 Express Edition](creating-the-membership-schema-in-sql-server-cs/_static/image8.png)](creating-the-membership-schema-in-sql-server-cs/_static/image7.png)

**Figura 3**: conectar-se à instância de SQL Server 2005 Express Edition ([clique para exibir a imagem em tamanho normal](creating-the-membership-schema-in-sql-server-cs/_static/image9.png))

Depois de se conectar à instância de SQL Server 2005 Express Edition, Management Studio exibe pastas para os bancos de dados, as configurações de segurança, os objetos de servidor e assim por diante. Se você expandir a guia bancos de dados, verá que o banco de `SecurityTutorials.mdf` *não* está registrado na instância do banco de dados. precisamos anexar o banco de dados primeiro.

Clique com o botão direito do mouse na pasta bancos de dados e escolha anexar no menu de contexto. Isso exibirá a caixa de diálogo anexar bancos de dados. Aqui, clique no botão Adicionar, navegue até o banco de dados `SecurityTutorials.mdf` e clique em OK. A Figura 4 mostra a caixa de diálogo anexar bancos de dados após a seleção do `SecurityTutorials.mdf` Database. A Figura 5 mostra o pesquisador de objetos do Management Studio depois que o banco de dados foi anexado com êxito.

[![anexar o banco de dados SecurityTutorials. MDF](creating-the-membership-schema-in-sql-server-cs/_static/image11.png)](creating-the-membership-schema-in-sql-server-cs/_static/image10.png)

**Figura 4**: anexar o banco de dados de `SecurityTutorials.mdf` ([clique para exibir a imagem em tamanho normal](creating-the-membership-schema-in-sql-server-cs/_static/image12.png))

[![o banco de dados SecurityTutorials. MDF aparece na pasta databases](creating-the-membership-schema-in-sql-server-cs/_static/image14.png)](creating-the-membership-schema-in-sql-server-cs/_static/image13.png)

**Figura 5**: o banco de dados de `SecurityTutorials.mdf` é exibido na pasta databases ([clique para exibir a imagem em tamanho normal](creating-the-membership-schema-in-sql-server-cs/_static/image15.png))

Como mostra a Figura 5, o banco de dados `SecurityTutorials.mdf` tem um nome ABSTRUSE. Vamos alterá-lo para um nome mais fácil de memorizar (e mais fácil de digitar). Clique com o botão direito do mouse no banco de dados, escolha Renomear no menu de contexto e renomeie-o `SecurityTutorialsDatabase`. Isso não altera o nome do arquivo, apenas o que o banco de dados usa para se identificar para SQL Server.

[![renomear o banco de dados como SecurityTutorialsDatabase](creating-the-membership-schema-in-sql-server-cs/_static/image17.png)](creating-the-membership-schema-in-sql-server-cs/_static/image16.png)

**Figura 6**: renomear o banco de dados para `SecurityTutorialsDatabase`([clique para exibir a imagem em tamanho normal](creating-the-membership-schema-in-sql-server-cs/_static/image18.png))

Neste ponto, sabemos os nomes de servidor e de banco de dados para o arquivo de banco de dados `SecurityTutorials.mdf`: `localhost\InstanceName` e `SecurityTutorialsDatabase`, respectivamente. Agora estamos prontos para instalar os serviços de aplicativos por meio da ferramenta de `aspnet_regsql.exe`.

### <a name="installing-the-application-services"></a>Instalando o Serviços de Aplicativos

Para iniciar a ferramenta de `aspnet_regsql.exe`, vá para o menu iniciar e escolha Executar. Insira `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\aspnet_regsql.exe` na caixa de texto e clique em OK. Como alternativa, você pode usar o Windows Explorer para fazer uma busca detalhada na pasta apropriada e clicar duas vezes no arquivo `aspnet_regsql.exe`. Qualquer uma das abordagens fará a rede dos mesmos resultados.

A execução da ferramenta de `aspnet_regsql.exe` sem nenhum argumento de linha de comando inicia a interface gráfica do usuário do assistente de instalação do ASP.NET SQL Server. O assistente facilita a adição ou remoção dos serviços de aplicativos ASP.NET em um banco de dados especificado. A primeira tela do assistente, mostrada na Figura 7, descreve a finalidade da ferramenta.

[![usar o assistente de instalação do ASP.NET SQL Server permite adicionar o esquema de associação](creating-the-membership-schema-in-sql-server-cs/_static/image20.png)](creating-the-membership-schema-in-sql-server-cs/_static/image19.png)

**Figura 7**: Use o assistente de instalação do ASP.NET SQL Server para adicionar o esquema de associação ([clique para exibir a imagem em tamanho normal](creating-the-membership-schema-in-sql-server-cs/_static/image21.png))

A segunda etapa do assistente nos pergunta se desejamos adicionar os serviços de aplicativo ou removê-los. Como queremos adicionar as tabelas, exibições e procedimentos armazenados necessários para o `SqlMembershipProvider`, escolha a opção Configurar SQL Server para serviços de aplicativos. Posteriormente, se você quiser remover esse esquema do banco de dados, execute novamente esse assistente, mas escolha a opção remover informações dos serviços de aplicativo de um banco de dados existente.

[![escolher a opção Configurar SQL Server para Serviços de Aplicativos](creating-the-membership-schema-in-sql-server-cs/_static/image23.png)](creating-the-membership-schema-in-sql-server-cs/_static/image22.png)

**Figura 8**: escolha a opção Configurar SQL Server para serviços de aplicativos ([clique para exibir a imagem em tamanho normal](creating-the-membership-schema-in-sql-server-cs/_static/image24.png))

A terceira etapa solicita as informações do banco de dados: o nome do servidor, as informações de autenticação e o nome do banco de dados. Se você esteve acompanhando este tutorial e adicionou o banco de dados `SecurityTutorials.mdf` ao `App_Data`, anexou-o a `localhost\InstanceName`e renomeou-o para `SecurityTutorialsDatabase`, use os seguintes valores:

- Servidor: `localhost\InstanceName`
- Autenticação do Windows
- Banco de dados: `SecurityTutorialsDatabase`

[![inserir as informações do banco de dados](creating-the-membership-schema-in-sql-server-cs/_static/image26.png)](creating-the-membership-schema-in-sql-server-cs/_static/image25.png)

**Figura 9**: inserir as informações do banco de dados ([clique para exibir a imagem em tamanho normal](creating-the-membership-schema-in-sql-server-cs/_static/image27.png))

Depois de inserir as informações do banco de dados, clique em Avançar. A etapa final resume as etapas que serão tomadas. Clique em avançar para instalar os serviços de aplicativo e em concluir para concluir o assistente.

> [!NOTE]
> Se você usou Management Studio para anexar o banco de dados e renomear o arquivo de banco de dados, lembre-se de desanexar o banco de dados e fechar Management Studio antes de reabrir o Visual Studio. Para desanexar o banco de dados `SecurityTutorialsDatabase`, clique com o botão direito do mouse no nome do banco de dados e, no menu tarefas, escolha desanexar.

Após a conclusão do assistente, retorne ao Visual Studio e navegue até a Gerenciador de Banco de Dados. Expanda a pasta Tabelas. Você deve ver uma série de tabelas cujos nomes começam com o prefixo `aspnet_`. Da mesma forma, uma variedade de exibições e procedimentos armazenados podem ser encontrados nas pastas exibições e procedimentos armazenados. Esses objetos de banco de dados compõem o esquema de serviços de aplicativos. Examinaremos os objetos de banco de dados específicos da associação e da função na etapa 3.

[![uma variedade de tabelas, exibições e procedimentos armazenados foram adicionados ao banco de dados](creating-the-membership-schema-in-sql-server-cs/_static/image29.png)](creating-the-membership-schema-in-sql-server-cs/_static/image28.png)

**Figura 10**: uma variedade de tabelas, exibições e procedimentos armazenados foram adicionados ao banco de dados ([clique para exibir a imagem em tamanho normal](creating-the-membership-schema-in-sql-server-cs/_static/image30.png))

> [!NOTE]
> A interface gráfica do usuário da ferramenta de `aspnet_regsql.exe` instala todo o esquema de serviços de aplicativos. Mas, ao executar `aspnet_regsql.exe` da linha de comando, você pode especificar quais componentes de serviços de aplicativos específicos instalar (ou remover). Portanto, se você quiser adicionar apenas as tabelas, exibições e procedimentos armazenados necessários para os provedores de `SqlMembershipProvider` e `SqlRoleProvider`, execute `aspnet_regsql.exe` na linha de comando. Como alternativa, você pode executar manualmente o subconjunto apropriado de scripts de criação de T-SQL usados pelo `aspnet_regsql.exe`. Esses scripts estão localizados na pasta `WINDIR%\Microsoft.Net\Framework\v2.0.50727\` com nomes como `InstallCommon.sql`,`InstallMembership.sql`,`InstallRoles.sql`, `InstallProfile.sql`,`InstallSqlState.sql`e assim por diante.

Neste ponto, criamos os objetos de banco de dados necessários para o `SqlMembershipProvider`. No entanto, ainda precisamos instruir a estrutura de associação de que ela deve usar o `SqlMembershipProvider` (versus, digamos, a `ActiveDirectoryMembershipProvider`) e que o `SqlMembershipProvider` deve usar o banco de dados `SecurityTutorials`. Veremos como especificar qual provedor deve ser usado e como personalizar as configurações do provedor selecionado na etapa 4. Mas primeiro, vamos examinar mais detalhadamente os objetos de banco de dados que acabaram de ser criados.

## <a name="step-3-a-look-at-the-schemas-core-tables"></a>Etapa 3: examinar as tabelas principais do esquema

Ao trabalhar com as estruturas de associação e funções em um aplicativo ASP.NET, os detalhes da implementação são encapsulados pelo provedor. Em Tutoriais futuros, vamos criar uma interface com essas estruturas por meio das classes `Membership` e `Roles` do .NET Framework. Ao usar essas APIs de alto nível, não precisamos nos preocupar com os detalhes de baixo nível, como as consultas são executadas ou quais tabelas são modificadas pelo `SqlMembershipProvider` e `SqlRoleProvider`.

Considerando isso, poderíamos usar com confiança as estruturas de associação e funções sem ter explorado o esquema de banco de dados criado na etapa 2. No entanto, ao criar as tabelas para armazenar dados de aplicativos, talvez seja necessário criar entidades relacionadas a usuários ou funções. Ele ajuda a ter uma familiaridade com os esquemas `SqlMembershipProvider` e `SqlRoleProvider` ao estabelecer restrições de chave estrangeira entre as tabelas de dados do aplicativo e as tabelas criadas na etapa 2. Além disso, em determinadas circunstâncias raras, poderemos ser necessário interagir com o usuário e os armazenamentos de função diretamente no nível do banco de dados (em vez de através das classes `Membership` ou `Roles`).

### <a name="partitioning-the-user-store-into-applications"></a>Particionando o armazenamento do usuário em aplicativos

As estruturas de associação e de funções são projetadas de modo que um único usuário e armazenamento de função possam ser compartilhados entre vários aplicativos diferentes. Um aplicativo ASP.NET que usa as estruturas de associação ou funções deve especificar qual partição de aplicativo usar. Em suma, vários aplicativos Web podem usar os mesmos armazenamentos de usuário e de função. A Figura 11 descreve os armazenamentos de usuário e de função que são particionados em três aplicativos: HRSite, CustomerSite e SalesSite. Esses três aplicativos Web têm seus próprios usuários e funções exclusivos, ainda que todos armazenem fisicamente suas informações de conta e função de usuário nas mesmas tabelas de banco de dados.

[![contas de usuário podem ser particionadas em vários aplicativos](creating-the-membership-schema-in-sql-server-cs/_static/image32.png)](creating-the-membership-schema-in-sql-server-cs/_static/image31.png)

**Figura 11**: as contas de usuário podem ser particionadas em vários aplicativos ([clique para exibir a imagem em tamanho normal](creating-the-membership-schema-in-sql-server-cs/_static/image33.png))

A tabela `aspnet_Applications` é o que define essas partições. Cada aplicativo que usa o banco de dados para armazenar informações de conta de usuário é representado por uma linha nesta tabela. A tabela `aspnet_Applications` tem quatro colunas: `ApplicationId`, `ApplicationName`, `LoweredApplicationName`e `Description`. `ApplicationId` é do tipo [`uniqueidentifier`](https://msdn.microsoft.com/library/ms187942.aspx) e é a chave primária da tabela; `ApplicationName` fornece um nome amigável exclusivo para cada aplicativo.

As outras tabelas relacionadas à associação e à função são vinculadas de volta ao campo `ApplicationId` no `aspnet_Applications`. Por exemplo, a tabela `aspnet_Users`, que contém um registro para cada conta de usuário, tem um campo de chave estrangeira `ApplicationId`; Idem para a tabela de `aspnet_Roles`. O campo `ApplicationId` nessas tabelas especifica a partição de aplicativo à qual a conta de usuário ou função pertence.

### <a name="storing-user-account-information"></a>Armazenando informações de conta de usuário

As informações de conta de usuário são armazenadas em duas tabelas: `aspnet_Users` e `aspnet_Membership`. A tabela `aspnet_Users` contém campos que contêm as informações essenciais da conta de usuário. As três colunas mais pertinentes são:

- `UserId`
- `UserName`
- `ApplicationId`

`UserId` é a chave primária (e do tipo `uniqueidentifier`). `UserName` é do tipo `nvarchar(256)` e, junto com a senha, compõe as credenciais do usuário. (A senha de um usuário é armazenada na tabela `aspnet_Membership`.) `ApplicationId` vincula a conta de usuário a um aplicativo específico no `aspnet_Applications`. Há uma restrição de [`UNIQUE`](https://msdn.microsoft.com/library/ms191166.aspx) de composição nas colunas `UserName` e `ApplicationId`. Isso garante que, em um determinado aplicativo, cada nome de usuário seja exclusivo, mas permite que o mesmo `UserName` seja usado em aplicativos diferentes.

A tabela de `aspnet_Membership` inclui informações de conta de usuário adicionais, como a senha do usuário, o endereço de email, a data e hora do último logon e assim por diante. Há uma correspondência um-para-um entre registros nas tabelas `aspnet_Users` e `aspnet_Membership`. Essa relação é garantida pelo campo `UserId` em `aspnet_Membership`, que serve como a chave primária da tabela. Assim como a tabela `aspnet_Users`, `aspnet_Membership` inclui um campo `ApplicationId` que vincula essas informações a uma partição de aplicativo específica.

### <a name="securing-passwords"></a>Protegendo senhas

As informações de senha são armazenadas na tabela `aspnet_Membership`. O `SqlMembershipProvider` permite que as senhas sejam armazenadas no banco de dados usando uma das três técnicas a seguir:

- **Clear** -a senha é armazenada no banco de dados como texto sem formatação. Eu não recomendo o uso dessa opção. Se o banco de dados estiver comprometido, seja por um hacker que encontre um back door ou um funcionário insatisfeito que tenha acesso ao banco de dados-todas as credenciais de cada usuário estão lá para o que está assumindo.
- **Hash** -as senhas são com hash usando um algoritmo de hash unidirecional e um valor de Salt gerado aleatoriamente. Esse valor com hash (juntamente com o Salt) é armazenado no banco de dados.
- **Criptografado** -uma versão criptografada da senha é armazenada no banco de dados.

A técnica de armazenamento de senha usada depende das configurações de `SqlMembershipProvider` especificadas em `Web.config`. Veremos como personalizar as configurações de `SqlMembershipProvider` na etapa 4. O comportamento padrão é armazenar o hash da senha.

As colunas responsáveis por armazenar a senha são `Password`, `PasswordFormat`e `PasswordSalt`. `PasswordFormat` é um campo do tipo `int` cujo valor indica a técnica usada para armazenar a senha: 0 para claro; 1 para hash; 2 para criptografado. `PasswordSalt` é atribuída a uma cadeia de caracteres gerada aleatoriamente, independentemente da técnica de armazenamento de senha usada; o valor de `PasswordSalt` é usado apenas ao computar o hash da senha. Por fim, a coluna `Password` contém os dados de senha reais, seja a senha de texto sem formatação, o hash da senha ou a senha criptografada.

A tabela 1 ilustra o que essas três colunas podem parecer para as várias técnicas de armazenamento ao armazenar a senha MySecret! .

| **Técnica de armazenamento&lt;\_O3A\_p/&gt;** | **&lt;de senha \_O3A\_p/&gt;** | **PasswordFormat&lt;\_O3A\_p/&gt;** | **PasswordSalt&lt;\_O3A\_p/&gt;** |
| --- | --- | --- | --- |
| Liberada | MySecret! | 0 | tTnkPlesqissc2y2SMEygA== |
| Hash | 2oXm6sZHWbTHFgjgkGQsc2Ec9ZM= | 1 | wFgjUfhdUFOCKQiI61vtiQ== |
| Criptografado | 62RZgDvhxykkqsMchZ0Yly7HS6onhpaoCYaRxV8g0F4CW56OXUU3e7Inza9j9BKp | 2 | LSRzhGS/aa/oqAXGLHJNBw== |

**Tabela 1**: valores de exemplo para os campos relacionados à senha ao armazenar a senha MySecret!

> [!NOTE]
> O algoritmo de criptografia ou hash específico usado pelo `SqlMembershipProvider` é determinado pelas configurações no elemento `<machineKey>`. Discutimos esse elemento de configuração na etapa 3 do <a id="Tutorial3"> </a>tutorial [*configuração de autenticação de formulários e tópicos avançados*](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) .

### <a name="storing-roles-and-role-associations"></a>Armazenando funções e associações de função

A estrutura de funções permite que os desenvolvedores definam um conjunto de funções e especifiquem quais usuários pertencem a quais funções. Essas informações são capturadas no banco de dados por meio de duas tabelas: `aspnet_Roles` e `aspnet_UsersInRoles`. Cada registro na tabela de `aspnet_Roles` representa uma função para um aplicativo específico. Assim como a tabela `aspnet_Users`, a tabela `aspnet_Roles` tem três colunas pertinentes à nossa discussão:

- `RoleId`
- `RoleName`
- `ApplicationId`

`RoleId` é a chave primária (e do tipo `uniqueidentifier`). `RoleName` é do tipo `nvarchar(256)`. E `ApplicationId` vincula a conta de usuário a um aplicativo específico no `aspnet_Applications`. Há uma restrição de `UNIQUE` composta nas colunas `RoleName` e `ApplicationId`, garantindo que, em um determinado aplicativo, cada nome de função seja exclusivo.

A tabela `aspnet_UsersInRoles` serve como um mapeamento entre usuários e funções. Há apenas duas colunas – `UserId` e `RoleId`-e juntas elas compõem uma chave primária composta.

## <a name="step-4-specifying-the-provider-and-customizing-its-settings"></a>Etapa 4: especificando o provedor e personalizando suas configurações

Todas as estruturas que dão suporte ao modelo de provedor, como as estruturas de associação e funções, não têm detalhes de implementação em si e, em vez disso, delegam essa responsabilidade a uma classe de provedor. No caso da estrutura de associação, a classe `Membership` define a API para gerenciar contas de usuário, mas não interage diretamente com nenhum armazenamento de usuário. Em vez disso, os métodos da classe `Membership` entregam a solicitação para o provedor configurado – usaremos a `SqlMembershipProvider`. Quando invocamos um dos métodos na classe `Membership`, como a estrutura de associação sabe delegar a chamada para o `SqlMembershipProvider`?

A classe `Membership` tem uma [propriedade`Providers`](https://msdn.microsoft.com/library/system.web.security.membership.providers.aspx) que contém uma referência a todas as classes de provedor registradas disponíveis para uso pela estrutura de associação. Cada provedor registrado tem um nome e tipo associados. O nome oferece uma maneira amigável para fazer referência a um provedor específico na coleção de `Providers`, enquanto o tipo identifica a classe do provedor. Além disso, cada provedor registrado pode incluir definições de configuração. As definições de configuração para a estrutura de associação incluem `passwordFormat` e `requiresUniqueEmail`, entre muitas outras. Consulte a tabela 2 para obter uma lista completa dos parâmetros de configuração usados pelo `SqlMembershipProvider`.

O conteúdo da propriedade `Providers` é especificado por meio de definições de configuração do aplicativo Web. Por padrão, todos os aplicativos Web têm um provedor chamado `AspNetSqlMembershipProvider` do tipo `SqlMembershipProvider`. Esse provedor de associação padrão está registrado no `machine.config` (localizado em `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG)`:

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample1.xml)]

Como mostra a marcação acima, o [elemento`<membership>`](https://msdn.microsoft.com/library/1b9hw62f.aspx) define as definições de configuração para a estrutura de associação, enquanto o [elemento filho`<providers>`](https://msdn.microsoft.com/library/6d4936ht.aspx) especifica os provedores registrados. Os provedores podem ser adicionados ou removidos usando os elementos [`<add>`](https://msdn.microsoft.com/library/whae3t94.aspx) ou [`<remove>`](https://msdn.microsoft.com/library/aykw9a6d.aspx) ; Use o elemento [`<clear>`](https://msdn.microsoft.com/library/t062y6yc.aspx) para remover todos os provedores registrados no momento. Como mostra a marcação acima, `machine.config` adiciona um provedor chamado `AspNetSqlMembershipProvider` do tipo `SqlMembershipProvider`.

Além dos atributos `name` e `type`, o elemento `<add>` contém atributos que definem os valores para várias configurações de configuração. A tabela 2 lista as definições de configuração específicas de `SqlMembershipProvider`disponíveis, juntamente com uma descrição de cada uma.

> [!NOTE]
> Todos os valores padrão indicados na tabela 2 referem-se aos valores padrão definidos na classe `SqlMembershipProvider`. Observe que nem todas as definições de configuração no `AspNetSqlMembershipProvider` correspondem aos valores padrão da classe `SqlMembershipProvider`. Por exemplo, se não for especificado em um provedor de associação, a configuração de `requiresUniqueEmail` padrão será true. No entanto, o `AspNetSqlMembershipProvider` substitui esse valor padrão especificando explicitamente um valor de `false`.

| **Configurando&lt;\_O3A\_p/&gt;** | **Descrição&lt;\_O3A\_p/&gt;** |
| --- | --- |
| `ApplicationName` | Lembre-se de que a estrutura de associação permite que um único repositório de usuário seja particionado em vários aplicativos. Essa configuração indica o nome da partição de aplicativo usada pelo provedor de associação. Se esse valor não for especificado explicitamente, ele será definido, em tempo de execução, para o valor do caminho da raiz virtual do aplicativo. |
| `commandTimeout` | Especifica o valor de tempo limite do comando SQL (em segundos). O valor padrão é 30. |
| `connectionStringName` | O nome da cadeia de conexão no elemento `<connectionStrings>` a ser usado para se conectar ao banco de dados de armazenamento do usuário. Esse valor é necessário. |
| `description` | Fornece uma descrição amigável do provedor registrado. |
| `enablePasswordRetrieval` | Especifica se os usuários podem recuperar sua senha esquecida. O valor padrão é `false`. |
| `enablePasswordReset` | Indica se os usuários têm permissão para redefinir sua senha. Usa `true` como padrão. |
| `maxInvalidPasswordAttempts` | O número máximo de tentativas de logon malsucedidas que podem ocorrer para um determinado usuário durante o `passwordAttemptWindow` especificado antes que o usuário seja bloqueado. O valor padrão é 5. |
| `minRequiredNonalphanumericCharacters` | O número mínimo de caracteres não alfanuméricos que devem aparecer na senha de um usuário. Esse valor deve estar entre 0 e 128; o padrão é 1. |
| `minRequiredPasswordLength` | O número mínimo de caracteres necessários em uma senha. Esse valor deve estar entre 0 e 128; o padrão é 7. |
| `name` | O nome do provedor registrado. Esse valor é necessário. |
| `passwordAttemptWindow` | O número de minutos durante os quais as tentativas de logon com falha são rastreadas. Se um usuário fornecer credenciais de logon inválidas `maxInvalidPasswordAttempts` vezes nessa janela especificada, elas serão bloqueadas. O valor padrão é 10. |
| `PasswordFormat` | O formato de armazenamento de senha: `Clear`, `Hashed`ou `Encrypted`. O padrão é `Hashed`. |
| `passwordStrengthRegularExpression` | Se fornecido, essa expressão regular é usada para avaliar a intensidade da senha selecionada do usuário ao criar uma nova conta ou ao alterar sua senha. O valor padrão é uma cadeia de caracteres vazia. |
| `requiresQuestionAndAnswer` | Especifica se um usuário deve responder a sua pergunta de segurança ao recuperar ou redefinir sua senha. O valor padrão é `true`. |
| `requiresUniqueEmail` | Indica se todas as contas de usuário em uma determinada partição de aplicativo devem ter um endereço de email exclusivo. O valor padrão é `true`. |
| `type` | Especifica o tipo do provedor. Esse valor é necessário. |

**Tabela 2**: definições de configuração de associação e `SqlMembershipProvider`

Além de `AspNetSqlMembershipProvider`, outros provedores de associação podem ser registrados em uma base de aplicativo por aplicativo, adicionando marcação semelhante ao arquivo de `Web.config`.

> [!NOTE]
> A estrutura de funções funciona quase da mesma maneira: há um provedor de função registrado padrão no `machine.config` e os provedores registrados podem ser personalizados em uma base de aplicativo por aplicativo no `Web.config`. Examinaremos a estrutura de funções e sua marcação de configuração em detalhes em um tutorial futuro.

### <a name="customizing-thesqlmembershipprovidersettings"></a>Personalizando as configurações de`SqlMembershipProvider`

O `SqlMembershipProvider` padrão (`AspNetSqlMembershipProvider`) tem seu atributo `connectionStringName` definido como `LocalSqlServer`. Assim como o provedor de `AspNetSqlMembershipProvider`, o nome da cadeia de conexão `LocalSqlServer` é definido em `machine.config`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample2.xml)]

Como você pode ver, essa cadeia de conexão define um banco de dados do SQL 2005 Express Edition localizado em | DataDirectory | aspnetdb. MDF. A cadeia de caracteres | DataDirectory | é convertido em tempo de execução para apontar para o diretório `~/App_Data/`, portanto, o caminho do banco de dados | DataDirectory | aspnetdb. MDF "está relacionado a `~/App_Data`/`aspnet.mdf`.

Se não especificarmos nenhuma informação de provedor de associação no arquivo de `Web.config` do nosso aplicativo, o aplicativo usará o provedor de associação registrado padrão, `AspNetSqlMembershipProvider`. Se o banco de dados `~/App_Data/aspnet.mdf` não existir, o tempo de execução do ASP.NET o criará automaticamente e adicionará o esquema de serviços de aplicativo. No entanto, não queremos usar o banco de dados `aspnet.mdf`; em vez disso, queremos usar o banco de dados `SecurityTutorials.mdf` que criamos na etapa 2. Essa modificação pode ser realizada de duas maneiras:

- <strong>Especifique um valor para o nome da</strong> cadeia de conexão<strong>`LocalSqlServer`</strong> <strong>em</strong> <strong>`Web.config`</strong> <strong>.</strong> Ao substituir o valor do nome da cadeia de conexão `LocalSqlServer` no `Web.config`, podemos usar o provedor de associação registrado padrão (`AspNetSqlMembershipProvider`) e fazê-lo funcionar corretamente com o banco de dados `SecurityTutorials.mdf`. Essa abordagem será boa se você for conteúdo com as definições de configuração especificadas por `AspNetSqlMembershipProvider`. Para obter mais informações sobre essa técnica, consulte a postagem de blog de [Scott Guthrie](https://weblogs.asp.net/scottgu/), [configurando ASP.NET 2,0 Serviços de Aplicativos para usar SQL Server 2000 ou SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- <strong>Adicione um novo provedor registrado do tipo</strong> <strong>`SqlMembershipProvider`</strong> <strong>e defina sua</strong> configuração de<strong>`connectionStringName`</strong> <strong>para apontar para o banco de</strong> dados<strong>`SecurityTutorials.mdf`</strong> <strong>.</strong> Essa abordagem é útil em cenários em que você deseja personalizar outras propriedades de configuração além da cadeia de conexão do banco de dados. Em meus próprios projetos, sempre uso essa abordagem devido à sua flexibilidade e legibilidade.

Antes que possamos adicionar um novo provedor registrado que faz referência ao banco de dados `SecurityTutorials.mdf`, primeiro precisamos adicionar um valor de cadeia de conexão apropriado na seção `<connectionStrings>` no `Web.config`. A marcação a seguir adiciona uma nova cadeia de conexão chamada `SecurityTutorialsConnectionString` que faz referência à SQL Server 2005 Express Edition `SecurityTutorials.mdf` banco de dados na pasta `App_Data`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample3.xml)]

> [!NOTE]
> Se você estiver usando um arquivo de banco de dados alternativo, atualize a cadeia de conexão conforme necessário. Para obter mais informações sobre como formar a cadeia de conexão correta, consulte [connectionStrings.com](http://www.connectionstrings.com/).

Em seguida, adicione a seguinte marcação de configuração de associação ao arquivo de `Web.config`. Essa marcação registra um novo provedor chamado `SecurityTutorialsSqlMembershipProvider`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample4.xml)]

Além de registrar o provedor de `SecurityTutorialsSqlMembershipProvider`, a marcação acima define o `SecurityTutorialsSqlMembershipProvider` como o provedor padrão (por meio do atributo `defaultProvider` no elemento `<membership>`). Lembre-se de que a estrutura de associação pode ter vários provedores registrados. Como `AspNetSqlMembershipProvider` é registrado como o primeiro provedor no `machine.config`, ele serve como o provedor padrão, a menos que indiquemos de outra forma.

Atualmente, nosso aplicativo tem dois provedores registrados: `AspNetSqlMembershipProvider` e `SecurityTutorialsSqlMembershipProvider`. No entanto, antes de registrar o provedor de `SecurityTutorialsSqlMembershipProvider`, poderíamos ter excluído todos os provedores registrados anteriormente adicionando um [elemento`<clear />`](https://msdn.microsoft.com/library/t062y6yc.aspx) imediatamente antes do nosso elemento `<add>`. Isso limparia a `AspNetSqlMembershipProvider` da lista de provedores registrados, o que significa que a `SecurityTutorialsSqlMembershipProvider` seria o único provedor de associação registrado. Se usarmos essa abordagem, não precisaremos marcar o `SecurityTutorialsSqlMembershipProvider` como o provedor padrão, pois ele seria o único provedor de associação registrado. Para obter mais informações sobre como usar `<clear />`, consulte [usando `<clear />` ao adicionar provedores](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx).

Observe que a configuração de `connectionStringName` do `SecurityTutorialsSqlMembershipProvider`faz referência ao nome da cadeia de conexão do `SecurityTutorialsConnectionString` recém-adicionado e que sua configuração de `applicationName` foi definida com um valor de SecurityTutorials. Além disso, a configuração `requiresUniqueEmail` foi definida como `true`. Todas as outras opções de configuração são idênticas aos valores em `AspNetSqlMembershipProvider`. Fique à vontade para fazer quaisquer modificações de configuração aqui, se desejar. Por exemplo, você pode apertar a intensidade da senha exigindo dois caracteres não alfanuméricos em vez de um, ou aumentando o comprimento da senha para oito caracteres, em vez de sete.

> [!NOTE]
> Lembre-se de que a estrutura de associação permite que um único repositório de usuário seja particionado em vários aplicativos. A configuração de `applicationName` do provedor de associação indica qual aplicativo o provedor usa ao trabalhar com o armazenamento do usuário. É importante definir explicitamente um valor para a configuração de `applicationName` porque, se o `applicationName` não estiver definido explicitamente, ele será atribuído ao caminho raiz virtual do aplicativo Web em tempo de execução. Isso funciona bem, desde que o caminho da raiz virtual do aplicativo não seja alterado, mas se você mover o aplicativo para um caminho diferente, a configuração de `applicationName` também será alterada. Quando isso acontece, o provedor de associação começará a trabalhar com uma partição de aplicativo diferente da usada anteriormente. As contas de usuário criadas antes da movimentação residirão em uma partição de aplicativo diferente e esses usuários não poderão mais fazer logon no site. Para obter uma discussão mais aprofundada sobre esse assunto, confira [sempre definir a propriedade `applicationName` ao configurar a associação do ASP.NET 2,0 e outros provedores](https://weblogs.asp.net/scottgu/443634).

## <a name="summary"></a>Resumo

Neste ponto, temos um banco de dados com os serviços de aplicativo configurados (`SecurityTutorials.mdf`) e configuramos nosso aplicativo Web para que a estrutura de associação use o provedor de `SecurityTutorialsSqlMembershipProvider` que acabamos de registrar. Este provedor registrado é do tipo `SqlMembershipProvider` e tem seu `connectionStringName` definido como a cadeia de conexão apropriada (`SecurityTutorialsConnectionString`) e seu valor de `applicationName` definido explicitamente.

Agora estamos prontos para usar a estrutura de associação de nosso aplicativo. No próximo tutorial, examinaremos como criar novas contas de usuário. A seguir, vamos explorar os usuários de autenticação, executar a autorização baseada no usuário e armazenar informações adicionais relacionadas ao usuário no banco de dados.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Sempre definir a propriedade `applicationName` ao configurar a associação do ASP.NET 2,0 e outros provedores](https://weblogs.asp.net/scottgu/443634)
- [Configurando o ASP.NET 2,0 Serviços de Aplicativos para usar SQL Server 2000 ou SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx)
- [Baixe o SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)
- [Examinando a associação, as funções e o perfil do ASP.NET 2,0 s](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [O elemento `<add>` para provedores para associação](https://msdn.microsoft.com/library/whae3t94.aspx)
- [O elemento `<membership>`](https://msdn.microsoft.com/library/1b9hw62f.aspx)
- [O elemento `<providers>` para associação](https://msdn.microsoft.com/library/6d4936ht.aspx)
- [Usando `<clear />` ao adicionar provedores](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx)
- [Trabalhando diretamente com o `SqlMembershipProvider`](http://aspnet.4guysfromrolla.com/articles/091207-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Treinamento em vídeo sobre tópicos contidos neste tutorial

- [Noções básicas sobre associações do ASP.NET](../../../videos/authentication/understanding-aspnet-memberships.md)
- [Configurando o SQL para trabalhar com esquemas de associação](../../../videos/authentication/configuring-sql-to-work-with-membership-schemas.md)
- [Alterar as configurações de associação do esquema de associação padrão](../../../videos/authentication/changing-membership-settings-in-the-default-membership-schema.md)

### <a name="about-the-author"></a>Sobre o autor

Scott Mitchell, autor de vários livros sobre ASP/ASP. NET e fundador da 4GuysFromRolla.com, tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a *[Sams ensina a ASP.NET 2,0 em 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott pode ser contatado em [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) ou por meio de seu blog em [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. O revisor de Lead para este tutorial foi Alicja Maziarz. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Próximo](creating-user-accounts-cs.md)
