---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-vb
title: Configurando um site que usa Serviços de Aplicativos (VB) | Microsoft Docs
author: rick-anderson
description: A versão 2,0 do ASP.NET introduziu uma série de serviços de aplicativos, que fazem parte do .NET Framework e servem como um conjunto de serviços de blocos de construção que yo...
ms.author: riande
ms.date: 04/23/2009
ms.assetid: 9c31a42f-d8bb-4c0f-9ccc-597d4f70ac42
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-vb
msc.type: authoredcontent
ms.openlocfilehash: 19e7258b558372259c7554a36c6ad73ce572dfa8
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74588739"
---
# <a name="configuring-a-website-that-uses-application-services-vb"></a>Configuração de um site que usa Serviços de Aplicativos (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_09_VB.zip) ou [baixar PDF](https://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial09_AppServicesConfig_vb.pdf)

> A versão 2,0 do ASP.NET introduziu uma série de serviços de aplicativos, que fazem parte do .NET Framework e servem como um conjunto de serviços de blocos de construção que você pode usar para adicionar funcionalidades avançadas ao seu aplicativo Web. Este tutorial explora como configurar um site no ambiente de produção para usar os serviços de aplicativos e solucionar problemas comuns com o gerenciamento de contas de usuário e funções no ambiente de produção.

## <a name="introduction"></a>Introdução

A versão 2,0 do ASP.NET introduziu uma série de *serviços de aplicativos*, que fazem parte do .NET Framework e servem como um conjunto de serviços de blocos de construção que você pode usar para adicionar funcionalidades avançadas ao seu aplicativo Web. Os serviços de aplicativo incluem:

- **Associação** -uma API para criar e gerenciar contas de usuário.
- **Funções** – uma API para categorizar usuários em grupos.
- **Perfil** -uma API para armazenar conteúdo personalizado específico do usuário.
- **Mapa do site** -uma API para definir uma estrutura lógica de um site na forma de uma hierarquia, que pode ser exibida por meio de controles de navegação, como menus e trilhas.
- **Personalização** -uma API para manter as preferências de personalização, usadas com mais frequência com [*Web Parts*](https://msdn.microsoft.com/library/e0s9t4ck.aspx).
- **Monitoramento de integridade** -uma API para monitorar o desempenho, a segurança, os erros e outras métricas de integridade do sistema para um aplicativo Web em execução.

As APIs dos serviços de aplicativos não estão ligadas a uma implementação específica. Em vez disso, você instrui os serviços de aplicativo a usar um *provedor*específico e esse provedor implementa o serviço usando uma tecnologia específica. Os provedores usados com mais frequência para aplicativos Web baseados na Internet hospedados em uma empresa de hospedagem na Web são aqueles que usam uma implementação de banco de dados SQL Server. Por exemplo, o `SqlMembershipProvider` é um provedor para a API de associação que armazena informações de conta de usuário em um banco de dados Microsoft SQL Server.

Usar os serviços de aplicativos e os provedores de SQL Server adiciona alguns desafios ao implantar o aplicativo. Para os iniciantes, os objetos de banco de dados dos serviços de aplicativos devem ser criados corretamente nos bancos de dados de desenvolvimento e de produção e inicializados adequadamente. Também há parâmetros de configuração importantes que precisam ser feitos.

> [!NOTE]
> As APIs de serviços de aplicativos foram projetadas usando o [*modelo de provedor*](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), um padrão de design que permite que os detalhes de implementação de uma API sejam fornecidos em tempo de execução. O .NET Framework é fornecido com vários provedores de serviço de aplicativo que podem ser usados, como o `SqlMembershipProvider` e `SqlRoleProvider`, que são provedores para as APIs de associação e funções que usam uma implementação de banco de dados SQL Server. Você também pode criar e conectar um provedor personalizado. Na verdade, o livro Reviews Web já contém um provedor personalizado para a API do mapa do site (`ReviewSiteMapProvider`), que constrói o mapa do site a partir dos dados no `Genres` e `Books` tabelas no Database.

Este tutorial começa com uma análise de como estender o aplicativo Web de análises de livros para usar as APIs de associação e funções. Em seguida, ele percorre a implantação de um aplicativo Web que usa os serviços de aplicativos com uma implementação de banco de dados SQL Server e conclui resolvendo problemas comuns com o gerenciamento de contas de usuário e funções no ambiente de produção.

## <a name="updates-to-the-book-reviews-application"></a>Atualizações para o aplicativo de revisões de livros

Nos últimos tutoriais, o livro revisa o aplicativo Web foi atualizado de um site estático para um aplicativo Web dinâmico, controlado por dados, completo com um conjunto de páginas de administração para gerenciar gêneros e revisões. No entanto, essa seção de administração não está protegida no momento – qualquer usuário que conheça (ou adivinhe) a URL da página de administração pode Waltz e criar, editar ou excluir revisões em nosso site. Uma maneira comum de proteger determinadas partes de um site é implementar contas de usuário e, em seguida, usar regras de autorização de URL para restringir o acesso a determinados usuários ou funções. O livro Reviews Web Application disponível para download com este tutorial dá suporte a contas de usuário e funções. Ele tem uma única função definida com o nome admin e somente os usuários nessa função podem acessar as páginas de administração.

> [!NOTE]
> Eu criei três contas de usuário no livro Reviews Web Application: Scott, Jisun e Alice. Todos os três usuários têm a mesma senha: **senha!** Scott e Jisun estão na função de administrador, Alice não é. As páginas de não administração do site ainda são acessíveis a usuários anônimos. Ou seja, você não precisa entrar para visitar o site, a menos que queira administrá-lo, caso em que você deve entrar como um usuário na função de administrador.

A página mestra de aplicativos de revisões de registros foi atualizada para incluir uma interface de usuário diferente para usuários autenticados e anônimos. Se um usuário anônimo visitar o site, ele verá um link de logon no canto superior direito. Um usuário autenticado vê a mensagem "bem-vindo de volta, *nome de usuário*!" e um link para fazer logoff. Há também uma página de logon (`~/Login.aspx`), que contém um controle de logon na Web que fornece a interface do usuário e a lógica para autenticar um visitante. Somente os administradores podem criar novas contas. (Há páginas para criar e gerenciar contas de usuário na pasta `~/Admin`.)

### <a name="configuring-the-membership-and-roles-apis"></a>Configurando as APIs de associação e funções

O livro Reviews Web Application usa as APIs Membership e roles para dar suporte a contas de usuário e agrupar esses usuários em funções (ou seja, a função de administrador). As classes de provedor `SqlMembershipProvider` e `SqlRoleProvider` são usadas porque queremos armazenar informações de conta e função em um banco de dados SQL Server.

> [!NOTE]
> Este tutorial não pretende ser um exame detalhado na configuração de um aplicativo Web para dar suporte às APIs de associação e funções. Para obter uma visão completa dessas APIs e as etapas que você precisa seguir para configurar um site para usá-las, leia meus [*tutoriais de segurança do site*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

Para usar os serviços de aplicativos com um banco de dados SQL Server, você deve primeiro adicionar os objetos de banco de dados usados por esses provedores ao banco de dados onde você deseja que as informações de conta e função de usuário sejam armazenadas. Esses objetos de banco de dados de requisito incluem uma variedade de tabelas, exibições e procedimentos armazenados. A menos que especificado de outra forma, as classes de provedor `SqlMembershipProvider` e `SqlRoleProvider` usam um banco de dados SQL Server Express Edition chamado `ASPNETDB` localizado na pasta Application s `App_Data`; Se esse banco de dados não existir, ele será criado automaticamente com os objetos de banco de dados necessários por esses provedores em tempo de execução.

É possível, e geralmente ideal, criar os objetos de banco de dados dos serviços de aplicativo no mesmo banco de dados em que os aplicativos específicos do aplicativo do site são armazenados. O .NET Framework é fornecido com uma ferramenta chamada `aspnet_regsql.exe` que instala os objetos de banco de dados em um banco de dados especificado. Em frente, usei essa ferramenta para adicionar esses objetos ao banco de dados `Reviews.mdf` na pasta `App_Data` (o banco de dados de desenvolvimento). Veremos como usar essa ferramenta posteriormente neste tutorial quando adicionamos esses objetos ao banco de dados de produção.

Se você adicionar objetos de banco de dados de serviços de aplicativos a um banco de dados que não seja `ASPNETDB`, precisará personalizar as configurações de classes de provedor de `SqlMembershipProvider` e `SqlRoleProvider` para que elas usem o banco de dados apropriado. Para personalizar o provedor de associação, adicione um [*elemento de&gt; de associação de&lt;* ](https://msdn.microsoft.com/library/1b9hw62f.aspx) na seção `<system.web>` no `Web.config`; Use o [*elemento&lt;rolemanager&gt;* ](https://msdn.microsoft.com/library/ms164660.aspx) para configurar o provedor de funções. O trecho a seguir é obtido do livro Reviews Application s `Web.config` e mostra as configurações de configuração para as APIs Membership e Roles. Observe que ambos registram um novo provedor-`ReviewMembership` e `ReviewRole`-que usam os provedores `SqlMembershipProvider` e `SqlRoleProvider`, respectivamente.

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample1.xml)]

O elemento `Web.config` `<authentication>` do arquivo também foi configurado para oferecer suporte à autenticação baseada em formulários.

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample2.xml)]

### <a name="limiting-access-to-the-administration-pages"></a>Limitando o acesso às páginas de administração

O ASP.NET facilita a concessão ou a negação de acesso a um determinado arquivo ou pasta pelo usuário ou por função por meio de seu recurso de *autorização de URL* . (Discutimos brevemente a autorização de URL nas *principais diferenças entre o IIS e o tutorial de ASP.NET Development Server* e mostramos como o IIS e o ASP.NET Development Server aplicam regras de autorização de URL de forma diferente para conteúdo estático versus dinâmico.) Como queremos proibir o acesso à pasta `~/Admin`, exceto para os usuários na função admin, precisamos adicionar regras de autorização de URL a essa pasta. Especificamente, as regras de autorização de URL precisam permitir usuários na função de administrador e negar todos os outros usuários. Isso é feito adicionando um arquivo de `Web.config` à pasta `~/Admin` com o seguinte conteúdo:

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample3.xml)]

Para obter mais informações sobre o recurso de autorização de URL do ASP.NET e como usá-lo para soletrar as regras de autorização para usuários e funções, leia os tutoriais de autorização baseada em [*função*](../../older-versions-security/roles/role-based-authorization-vb.md) e [*autorização baseada em usuário*](../../older-versions-security/membership/user-based-authorization-vb.md) nos [*tutoriais de segurança*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)do meu site.

## <a name="deploying-a-web-application-that-uses-application-services"></a>Implantando um aplicativo Web que usa Serviços de Aplicativos

Ao implantar um site que usa serviços de aplicativos e um provedor que armazena as informações de serviços de aplicativos em um banco de dados, é imperativo que os objetos de banco de dados necessários para os serviços de aplicativos sejam criados no banco de dados de produção. Inicialmente, o banco de dados de produção não contém esses objetos, portanto, quando o aplicativo é implantado pela primeira vez (ou quando é implantado pela primeira vez depois que os serviços de aplicativo foram adicionados), você deve executar etapas adicionais para obter esses objetos de banco de dados necessários no banco de dados de produção.

Outro desafio pode surgir ao implantar um site que usa serviços de aplicativos se você pretende replicar as contas de usuário criadas no ambiente de desenvolvimento para o ambiente de produção. Dependendo da configuração da associação e das funções, é possível que, mesmo que você copie com êxito as contas de usuário criadas no ambiente de desenvolvimento para o banco de dados de produção, esses usuários não poderão entrar no aplicativo Web em produção. Vamos examinar a causa desse problema e discutir como evitar que ele aconteça.

O ASP.NET é fornecido com uma ótima [*ferramenta de administração de site (WSAT)* ](https://msdn.microsoft.com/library/yy40ytx0.aspx) que pode ser iniciada no Visual Studio e permite que a conta de usuário, as funções e as regras de autorização sejam gerenciadas por meio de uma interface baseada na Web. Infelizmente, o WSAT só funciona para sites locais, o que significa que ele não pode ser usado para gerenciar remotamente contas de usuário, funções e regras de autorização para o aplicativo Web no ambiente de produção. Veremos diferentes maneiras de implementar o comportamento do tipo WSAT do seu site de produção.

### <a name="adding-the-database-objects-using-aspnet_regsqlexe"></a>Adicionando os objetos de banco de dados usando ASPNET\_RegSql. exe

O tutorial *implantando um banco* de dados mostrou como copiar as tabelas e os dados do banco de dado de desenvolvimento para o banco de dados de produção, e essas técnicas certamente podem ser usadas para copiar os objetos de banco de dados de serviços de aplicativo para o banco de dados de produção. Outra opção é a ferramenta `aspnet_regsql.exe`, que adiciona ou remove os objetos de banco de dados dos serviços de aplicativo de um banco de dados.

> [!NOTE]
> A ferramenta de `aspnet_regsql.exe` cria os objetos de banco de dados em um banco de dados especificado. Ele não migra dados nesses objetos de banco de dado do banco de dados de desenvolvimento para o banco de dados de produção. Se você quer copiar a conta de usuário e as informações de função no banco de dados de desenvolvimento para o banco de dados de produção, use as técnicas abordadas no tutorial *implantando um banco de dados* .

Vamos ver como adicionar os objetos de banco de dados ao banco de dados de produção usando a ferramenta `aspnet_regsql.exe`. Comece abrindo o Windows Explorer e navegando até o diretório .NET Framework versão 2,0 no seu computador,% WINDIR% \ Microsoft. NET\Framework\v2.0.50727. Lá, você deve encontrar a ferramenta `aspnet_regsql.exe`. Essa ferramenta pode ser usada na linha de comando, mas também inclui uma interface gráfica do usuário; Clique duas vezes no arquivo `aspnet_regsql.exe` para iniciar seu componente gráfico.

A ferramenta é iniciada exibindo uma tela inicial explicando sua finalidade. Clique em avançar para ir para a tela "selecionar uma opção de instalação", que é mostrada na Figura 1. A partir daqui, você pode optar por adicionar os objetos de banco de dados dos serviços de aplicativo ou removê-los de um banco de dados. Como queremos adicionar esses objetos ao banco de dados de produção, selecione a opção "configurar SQL Server para serviços de aplicativos" e clique em Avançar.

[![optar por configurar SQL Server para Serviços de Aplicativos](configuring-a-website-that-uses-application-services-vb/_static/image2.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image1.jpg)

**Figura 1**: optar por configurar SQL Server para serviços de aplicativos ([clique para exibir a imagem em tamanho normal](configuring-a-website-that-uses-application-services-vb/_static/image3.jpg))

Na tela "selecionar o servidor e o banco de dados" solicita informações para se conectar ao banco de dados. Insira o servidor de banco de dados, as credenciais de segurança e o nome do banco de dados fornecidos a você pela empresa de hospedagem Web e clique em Avançar.

> [!NOTE]
> Depois de inserir o servidor de banco de dados e as credenciais, você pode receber um erro ao expandir a lista suspensa banco de dados. A ferramenta de `aspnet_regsql.exe` consulta a tabela do sistema `sysdatabases` para recuperar uma lista de bancos de dados no servidor, mas algumas empresas de hospedagem da Web bloqueiam seus servidores de banco de dados para que essas informações não estejam disponíveis publicamente. Se você receber esse erro, poderá digitar o nome do banco de dados diretamente na lista suspensa.

[![fornecer a ferramenta com as informações de conexão de seu banco de dados](configuring-a-website-that-uses-application-services-vb/_static/image5.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image4.jpg)

**Figura 2**: fornecer a ferramenta com as informações de conexão do banco de dados ([clique para exibir a imagem em tamanho normal](configuring-a-website-that-uses-application-services-vb/_static/image6.jpg))

A tela subsequente resume as ações que estão prestes a ocorrer, ou seja, os objetos de banco de dados dos serviços de aplicativo serão adicionados ao banco de dados especificado. Clique em avançar para concluir esta ação. Após alguns instantes, a tela final será exibida, observando que os objetos de banco de dados foram adicionados (veja a Figura 3).

[![êxito! Os objetos de banco de dados Serviços de Aplicativos foram adicionados ao banco de dados de produção](configuring-a-website-that-uses-application-services-vb/_static/image8.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image7.jpg)

**Figura 3**: sucesso! Os objetos de banco de dados Serviços de Aplicativos foram adicionados ao banco de dados de produção ([clique para exibir a imagem em tamanho normal](configuring-a-website-that-uses-application-services-vb/_static/image9.jpg))

Para verificar se os objetos de banco de dados dos serviços de aplicativo foram adicionados com êxito ao banco de dados de produção, abra SQL Server Management Studio e conecte-se ao banco de dados de produção. Como mostra a Figura 4, agora você deve ver as tabelas de banco de dados de serviços de aplicativo em seu banco de dados, `aspnet_Applications`, `aspnet_Membership`, `aspnet_Users`e assim por diante.

[![confirmar que os objetos de banco de dados foram adicionados ao banco de dados de produção](configuring-a-website-that-uses-application-services-vb/_static/image11.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image10.jpg)

**Figura 4**: confirmar que os objetos de banco de dados foram adicionados ao banco de dados de produção ([clique para exibir a imagem em tamanho normal](configuring-a-website-that-uses-application-services-vb/_static/image12.jpg))

Você só precisará usar a ferramenta `aspnet_regsql.exe` ao implantar seu aplicativo Web pela primeira vez ou pela primeira vez depois de começar a usar os serviços de aplicativo. Depois que esses objetos de banco de dados estiverem no banco de dados de produção, eles não precisarão ser adicionados novamente ou modificados.

### <a name="copying-user-accounts-from-development-to-production"></a>Copiando contas de usuário de desenvolvimento para produção

Ao usar as classes de provedor `SqlMembershipProvider` e `SqlRoleProvider` para armazenar as informações de serviços de aplicativos em um banco de dados SQL Server, a conta de usuário e as informações de função são armazenadas em uma variedade de tabelas de banco de dados, incluindo `aspnet_Users`, `aspnet_Membership`, `aspnet_Roles`e `aspnet_UsersInRoles`, entre outras. Se durante o desenvolvimento você criar contas de usuário no ambiente de desenvolvimento, poderá replicar essas contas de usuário em produção copiando os registros correspondentes das tabelas de banco de dados aplicáveis. Se você usou o assistente de publicação de banco de dados para implantar os objetos de banco de dados dos serviços de aplicativos, você também pode ter optado por copiar os registros, o que resultaria em que as contas de usuário criadas no desenvolvimento também estejam em produção. Mas, dependendo de suas definições de configuração, você pode achar que os usuários cujas contas foram criadas no desenvolvimento e copiados para produção não podem fazer logon no site de produção. O que dá?

As classes de provedor `SqlMembershipProvider` e `SqlRoleProvider` foram projetadas de modo que um único banco de dados possa servir como um armazenamento de usuários para vários aplicativos, em que cada aplicativo poderia, teoricamente, ter usuários com sobreposição de nomes de usuário e funções com o mesmo nome. Para permitir essa flexibilidade, o banco de dados mantém uma lista de aplicativos na tabela `aspnet_Applications` e cada usuário é associado a um desses aplicativos. Especificamente, a tabela `aspnet_Users` tem uma coluna `ApplicationId` que vincula cada usuário a um registro na tabela `aspnet_Applications`.

Além da coluna `ApplicationId`, a tabela `aspnet_Applications` também inclui uma coluna `ApplicationName`, que fornece um nome mais amigável para o aplicativo. Quando um site tenta trabalhar com uma conta de usuário, como validar credenciais de um usuário na página de logon, ele deve informar à classe de `SqlMembershipProvider` com qual aplicativo trabalhar. Ele geralmente faz isso fornecendo o nome do aplicativo, e esse valor é proveniente da configuração do provedor em `Web.config`, especificamente por meio do atributo `applicationName`.

Mas o que acontece se o atributo `applicationName` não for especificado em `Web.config`? Nesse caso, o sistema de associação usa o caminho raiz do aplicativo como o valor `applicationName`. Se o atributo `applicationName` não for definido explicitamente em `Web.config`, então haverá a possibilidade de que o ambiente de desenvolvimento e o ambiente de produção usem uma raiz de aplicativo diferente e, portanto, serão associados a nomes de aplicativos diferentes nos serviços de aplicativos. Se tal incompatibilidade ocorrer, esses usuários criados no ambiente de desenvolvimento terão um valor `ApplicationId` que não corresponde ao valor de `ApplicationId` para o ambiente de produção. O resultado líquido é que esses usuários não poderão fazer logon.

> [!NOTE]
> Se você se encontrar nessa situação – com as contas de usuário copiadas para produção com um valor de `ApplicationId` incompatível, você poderia escrever uma consulta para atualizar esses valores de `ApplicationId` incorretos para o `ApplicationId` usado na produção. Após a atualização, os usuários cujas contas foram criadas no ambiente de desenvolvimento agora poderão entrar no aplicativo Web em produção.

A boa notícia é que há uma etapa simples que você pode tomar para garantir que os dois ambientes usem o mesmo `ApplicationId`-defina explicitamente o atributo `applicationName` no `Web.config` para todos os seus provedores de serviços de aplicativos. Defini explicitamente o atributo `applicationName` como "BookReviews" nos elementos `<membership>` e `<roleManager>`, pois esse trecho de `Web.config` mostra.

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample4.xml)]

Para obter mais discussões sobre como definir o atributo `applicationName` e a lógica por trás dele, consulte a postagem de blog de [*Scott Guthrie*](https://weblogs.asp.net/scottgu/) s, [*sempre defina a propriedade ApplicationName ao configurar a associação de ASP.net e outros provedores*](https://weblogs.asp.net/scottgu/443634).

### <a name="managing-user-accounts-in-the-production-environment"></a>Gerenciando contas de usuário no ambiente de produção

A ferramenta de administração de site da ASP.NET (WSAT) facilita a criação e o gerenciamento de contas de usuário, a definição e a aplicação de funções e a ortografia de regras de autorização baseadas em usuário e função. Você pode iniciar o WSAT do Visual Studio acessando o Gerenciador de Soluções e clicando no ícone de configuração do ASP.NET ou acessando o site ou os menus do projeto e selecionando o item de menu de configuração do ASP.NET. Infelizmente, o WSAT só pode funcionar com sites locais. Portanto, você não pode usar o WSAT de sua estação de trabalho para gerenciar o site no ambiente de produção.

A boa notícia é que toda a funcionalidade exposta fornecida pelo WSAT está disponível programaticamente por meio das APIs de associação e de funções; Além disso, muitas das telas WSAT usam os controles padrão relacionados ao logon do ASP.NET. Em suma, você pode adicionar páginas do ASP.NET ao seu site que oferecem os recursos de gerenciamento necessários.

Lembre-se de que um tutorial anterior atualizou o aplicativo Web de revisões de livros para incluir uma pasta `~/Admin`, e essa pasta foi configurada para permitir somente usuários na função Administrador. Adicionei uma página à pasta chamada `CreateAccount.aspx` da qual um administrador pode criar uma nova conta de usuário. Esta página usa o controle CreateUserWizard para exibir a interface do usuário e a lógica de back-end para criar uma nova conta de usuário. O que mais, eu personalizei o controle para incluir uma caixa de seleção que solicita se o novo usuário também deve ser adicionado à função de administrador (consulte a Figura 5). Com um pouco de trabalho, você pode criar um conjunto personalizado de páginas que implementa as tarefas relacionadas ao gerenciamento de usuários e funções que, de outra forma, seriam fornecidas pelo WSAT.

> [!NOTE]
> Para obter mais informações sobre como usar as APIs de associação e de funções juntamente com os controles da Web ASP.NET relacionados ao logon, leia meus [*tutoriais de segurança do site*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md). Para obter mais informações sobre como personalizar o controle CreateUserWizard, consulte o artigo [*criando contas de usuário*](../../older-versions-security/membership/creating-user-accounts-vb.md) e [*armazenando tutoriais adicionais de informações de usuário*](../../older-versions-security/membership/storing-additional-user-information-vb.md) , ou confira [*Erich Peterson*](http://www.erichpeterson.com/) s, [*Personalizando o controle CreateUserWizard*](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx).

[![administradores podem criar novas contas de usuário](configuring-a-website-that-uses-application-services-vb/_static/image14.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image13.jpg)

**Figura 5**: os administradores podem criar novas contas de usuário ([clique para exibir a imagem em tamanho normal](configuring-a-website-that-uses-application-services-vb/_static/image15.jpg))

Se você precisar da funcionalidade completa do WSAT, confira a [*distribuição da sua própria ferramenta de administração de site*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx), na qual o autor Dan Clem percorre o processo de criação de uma ferramenta personalizada de WSAT. Dan compartilha seu código-fonte do aplicativo ( C#em) e fornece instruções passo a passo para adicioná-lo ao seu site hospedado.

## <a name="summary"></a>Resumo

Ao implantar um aplicativo Web que usa a implementação de banco de dados dos serviços de aplicativo, você deve primeiro garantir que o banco de dados de produção tenha os objetos de banco de dados necessários. Esses objetos podem ser adicionados usando as técnicas discutidas no tutorial *implantando um banco de dados* ; Como alternativa, você pode usar a ferramenta `aspnet_regsql.exe`, como vimos neste tutorial. Outros desafios que abordamos no centro em relação à sincronização do nome do aplicativo usado nos ambientes de desenvolvimento e produção (que é importante se você quiser que os usuários e funções criados no ambiente de desenvolvimento sejam válidos na produção) e técnicas para gerenciamento de usuários e funções no ambiente de produção.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [*Ferramenta de registro de SQL Server ASP.NET (aspnet_regsql. exe)* ](https://msdn.microsoft.com/library/ms229862.aspx)
- [*Criando o banco de dados Serviços de Aplicativos para SQL Server*](https://msdn.microsoft.com/library/x28wfk74.aspx)
- [*Criando o esquema de associação no SQL Server*](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb.md)
- [*Examinando associação, funções e perfil do ASP.NET s*](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [*Distribuindo sua própria ferramenta de administração de site*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [*Tutoriais de segurança do site*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)
- [*Visão geral da ferramenta de administração de site*](https://msdn.microsoft.com/library/yy40ytx0.aspx)

> [!div class="step-by-step"]
> [Anterior](configuring-the-production-web-application-to-use-the-production-database-vb.md)
> [Próximo](strategies-for-database-development-and-deployment-vb.md)
