---
uid: web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-cs
title: Noções básicas de segurança e suporteC#ASP.net () | Microsoft Docs
author: rick-anderson
description: Este é o primeiro tutorial de uma série de tutoriais que explorarão técnicas para autenticar visitantes por meio de um formulário da Web, autorizando o acesso a parte...
ms.author: riande
ms.date: 01/13/2008
ms.assetid: 07e15538-2f29-40c6-b2e7-e6115075ac83
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-cs
msc.type: authoredcontent
ms.openlocfilehash: 1ccaac101a83d0e28b07b220b8b7b61a9039227e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78640248"
---
# <a name="security-basics-and-aspnet-support-c"></a>Noções básicas sobre segurança e suporte do ASP.NET (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar PDF](https://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial01_Basics_cs.pdf)

> Este é o primeiro tutorial de uma série de tutoriais que explorarão técnicas para autenticar visitantes por meio de um formulário da Web, autorizando o acesso a páginas e funcionalidades específicas e gerenciando contas de usuário em um aplicativo ASP.NET.

## <a name="introduction"></a>Introdução

Quais são os fóruns de uma coisa, sites de eCommerce, sites de email online, sites de portal e sites de redes sociais que todos têm em comum? Todos eles oferecem *contas de usuário*. Os sites que oferecem contas de usuário devem fornecer vários serviços. No mínimo, novos visitantes precisam ser capazes de criar uma conta e o retorno de visitantes deve ser capaz de fazer logon. Esses aplicativos Web podem tomar decisões com base no usuário conectado: algumas páginas ou ações podem ser restritas somente a usuários conectados ou a um determinado subconjunto de usuários; outras páginas podem mostrar informações específicas para o usuário conectado ou podem mostrar mais ou menos informações, dependendo de qual usuário está exibindo a página.

Este é o primeiro tutorial de uma série de tutoriais que explorarão técnicas para autenticar visitantes por meio de um formulário da Web, autorizando o acesso a páginas e funcionalidades específicas e gerenciando contas de usuário em um aplicativo ASP.NET. Ao longo desses tutoriais, examinaremos como:

- Identificar e registrar usuários em um site
- Use ASP. Estrutura de associação do NET para gerenciar contas de usuário
- Criar, atualizar e excluir contas de usuário
- Limitar o acesso a uma página da Web, diretório ou funcionalidade específica com base no usuário conectado
- Use ASP. Estrutura de funções da rede para associar contas de usuário a funções
- Gerenciar funções de usuário
- Limitar o acesso a uma página da Web, diretório ou funcionalidade específica com base na função do usuário conectado
- Personalize e estenda o ASP. Controles da Web de segurança da rede

Esses tutoriais são voltados para serem concisos e fornecem instruções passo a passo com muitas capturas de tela para orientá-lo no processo visualmente. Cada tutorial está disponível nas C# versões e Visual Basic e inclui um download do código completo usado. (Este primeiro tutorial enfoca os conceitos de segurança de um ponto de vista de alto nível e, portanto, não contém nenhum código associado.)

Neste tutorial, discutiremos conceitos de segurança importantes e quais instalações estão disponíveis no ASP.NET para auxiliar na implementação de autenticação de formulários, autorização, contas de usuário e funções. Vamos começar!

> [!NOTE]
> A segurança é um aspecto importante de qualquer aplicativo que abranja decisões físicas, tecnológicas e de política e requer um alto grau de conhecimento de planejamento e de domínio. Esta série de tutoriais não se destina a um guia para o desenvolvimento de aplicativos Web seguros. Em vez disso, ele se concentra especificamente em autenticação de formulários, autorização, contas de usuário e funções. Embora alguns conceitos de segurança que estejam relacionados a esses problemas sejam discutidos nesta série, outros são deixados sem exploração.

## <a name="authentication-authorization-user-accounts-and-roles"></a>Autenticação, autorização, contas de usuário e funções

A autenticação, a autorização, as contas de usuário e as funções são quatro termos que serão usados com muita frequência em toda esta série de tutoriais, portanto, gostaria de fazer um rápido momento para definir esses termos no contexto da segurança da Web. Em um modelo de cliente-servidor, como a Internet, há muitos cenários em que o servidor precisa identificar o cliente que faz a solicitação. A *autenticação* é o processo de garantir a identidade do cliente. Um cliente que foi identificado com êxito é considerado *autenticado*. Um cliente não identificado é dito como não *autenticado* ou *anônimo*.

Os sistemas de autenticação segura envolvem pelo menos uma das três facetas a seguir: [algo que você sabe, algo que você tem ou algo que você está](http://www.cs.cornell.edu/Courses/cs513/2005fa/NNLauthPeople.html). A maioria dos aplicativos Web depende de algo que o cliente sabe, como uma senha ou um PIN. As informações usadas para identificar o nome de usuário e a senha deles, por exemplo, são chamadas de *credenciais*. Esta série de tutoriais se concentra na *autenticação de formulários*, que é um modelo de autenticação no qual os usuários fazem logon no site, fornecendo suas credenciais em um formulário de página da Web. Todos nós já experimentamos esse tipo de autenticação antes. Vá para qualquer site de comércio eletrônico. Quando você estiver pronto para fazer check-out, será solicitado que você faça logon inserindo seu nome de usuário e senha em caixas de diálogo em uma página da Web.

Além de identificar clientes, um servidor pode precisar limitar quais recursos ou funcionalidades podem ser acessadas dependendo do cliente que faz a solicitação. A *autorização* é o processo de determinar se um usuário específico tem autoridade para acessar um recurso ou uma funcionalidade específica.

Uma *conta de usuário* é um repositório para informações persistentes sobre um usuário específico. As contas de usuário devem incluir minimamente informações que identificam exclusivamente o usuário, como o nome de logon e a senha do usuário. Juntamente com essas informações essenciais, as contas de usuário podem incluir itens como: o endereço de email do usuário; a data e a hora em que a conta foi criada; a data e a hora em que o logon foi feito pela última vez; nome e sobrenome; número de telefone; e endereço para correspondência. Ao usar a autenticação de formulários, as informações de conta de usuário normalmente são armazenadas em um banco de dados relacional como Microsoft SQL Server.

Os aplicativos Web que dão suporte a contas de usuário podem, opcionalmente, agrupar usuários em *funções*. Uma função é simplesmente um rótulo que é aplicado a um usuário e fornece uma abstração para definir regras de autorização e funcionalidade de nível de página. Por exemplo, um site pode incluir uma função de administrador com regras de autorização que proíbem que qualquer pessoa, mas um administrador, acesse um determinado conjunto de páginas da Web. Além disso, uma variedade de páginas que são acessíveis a todos os usuários (incluindo não administradores) podem exibir dados adicionais ou oferecer funcionalidade extra quando visitados por usuários na função Administradores. Usando funções, podemos definir essas regras de autorização em uma base função por função, em vez de usuário a usuário.

## <a name="authenticating-users-in-an-aspnet-application"></a>Autenticando usuários em um aplicativo ASP.NET

Quando um usuário insere uma URL na janela de endereço do navegador ou clica em um link, o navegador faz uma solicitação [http (Hypertext Transfer Protocol)](http://en.wikipedia.org/wiki/HTTP) para o servidor Web para o conteúdo especificado, seja uma página ASP.net, uma imagem, um arquivo JavaScript ou qualquer outro tipo de conteúdo. O servidor Web é tarefa com o retorno do conteúdo solicitado. Ao fazer isso, ele deve determinar várias coisas sobre a solicitação, incluindo quem fez a solicitação e se a identidade está autorizada a recuperar o conteúdo solicitado.

Por padrão, os navegadores enviam solicitações HTTP que não têm qualquer tipo de informação de identificação. Mas se o navegador incluir informações de autenticação, o servidor Web iniciará o fluxo de trabalho de autenticação, que tentará identificar o cliente que faz a solicitação. As etapas do fluxo de trabalho de autenticação dependem do tipo de autenticação que está sendo usado pelo aplicativo Web. O ASP.NET dá suporte a três tipos de autenticação: Windows, Passport e Forms. Esta série de tutoriais se concentra na autenticação de formulários, mas vamos levar um minuto para comparar e contrastar as lojas de usuários e o fluxo de trabalho da autenticação do Windows.

### <a name="authentication-via-windows-authentication"></a>Autenticação por meio da autenticação do Windows

O fluxo de trabalho de autenticação do Windows usa uma das seguintes técnicas de autenticação:

- Autenticação Básica
- Autenticação resumida
- Autenticação Integrada do Windows

Todas as três técnicas funcionam praticamente da mesma forma: quando uma solicitação anônima não autorizada chega, o servidor Web envia de volta uma resposta HTTP que indica que a autorização é necessária para continuar. O navegador, em seguida, exibe uma caixa de diálogo modal que solicita ao usuário seu nome e senha (veja a Figura 1). Essas informações são enviadas de volta ao servidor Web por meio de um cabeçalho HTTP.

![Uma caixa de diálogo modal solicita ao usuário suas credenciais](security-basics-and-asp-net-support-cs/_static/image1.png)

**Figura 1**: uma caixa de diálogo modal solicita ao usuário suas credenciais

As credenciais fornecidas são validadas em relação ao armazenamento de usuário do Windows do servidor Web. Isso significa que cada usuário autenticado em seu aplicativo Web deve ter uma conta do Windows em sua organização. Isso é comum em cenários de intranet. Na verdade, ao usar a autenticação integrada do Windows em uma configuração de intranet, o navegador fornece automaticamente ao servidor Web as credenciais usadas para fazer logon na rede, suprimindo, assim, a caixa de diálogo mostrada na Figura 1. Embora a autenticação do Windows seja ótima para aplicativos de intranet, ela geralmente é impossível para aplicativos de Internet, pois você não deseja criar contas do Windows para cada usuário que se inscreve em seu site.

### <a name="authentication-via-forms-authentication"></a>Autenticação por meio da autenticação de formulários

A autenticação de formulários, por outro lado, é ideal para aplicativos Web da Internet. Lembre-se de que a autenticação de formulários identifica o usuário solicitando que insiram suas credenciais por meio de um formulário da Web. Consequentemente, quando um usuário tenta acessar um recurso não autorizado, ele é automaticamente redirecionado para a página de logon na qual eles podem inserir suas credenciais. As credenciais enviadas são então validadas em um repositório de usuários personalizado – geralmente um banco de dados.

Depois de verificar as credenciais enviadas, um *tíquete de autenticação de formulários* é criado para o usuário. Esse tíquete indica que o usuário foi autenticado e inclui informações de identificação, como o nome de usuário. O tíquete de autenticação de formulários é (normalmente) armazenado como um cookie no computador cliente. Portanto, as visitas subsequentes ao site incluem o tíquete de autenticação de formulários na solicitação HTTP, permitindo assim que o aplicativo Web identifique o usuário após o logon.

A Figura 2 ilustra o fluxo de trabalho de autenticação de formulários de um ponto de privilegiando de alto nível. Observe como as partes de autenticação e autorização no ASP.NET atuam como duas entidades separadas. O sistema de autenticação de formulários identifica o usuário (ou relata que eles são anônimos). O sistema de autorização é o que determina se o usuário tem acesso ao recurso solicitado. Se o usuário não estiver autorizado (como está na Figura 2 ao tentar visitar anonimamente o ProtectedPage. aspx), o sistema de autorização relatará que o usuário foi negado, fazendo com que o sistema de autenticação de formulários redirecione automaticamente o usuário para a página de logon.

Depois que o usuário tiver feito logon com êxito, as solicitações HTTP subsequentes incluirão o tíquete de autenticação de formulários. O sistema de autenticação de formulários simplesmente identifica o usuário – é o sistema de autorização que determina se o usuário pode acessar o recurso solicitado.

![O fluxo de trabalho de autenticação de formulários](security-basics-and-asp-net-support-cs/_static/image2.png)

**Figura 2**: fluxo de trabalho de autenticação de formulários

Vamos nos aprofundar na autenticação de formulários com muito mais detalhes nos próximos dois tutoriais,[uma visão geral da configuração de autenticação de formulários e de](an-overview-of-forms-authentication-cs.md) [autenticação de formulários e tópicos avançados](forms-authentication-configuration-and-advanced-topics-cs.md). Para saber mais sobre o ASP. Opções de autenticação da rede, consulte [autenticação ASP.net](https://msdn.microsoft.com/library/eeyk640h.aspx).

## <a name="limiting-access-to-web-pages-directories-and-page-functionality"></a>Limitando o acesso a páginas da Web, diretórios e funcionalidade de página

O ASP.NET inclui duas maneiras de determinar se um usuário específico tem autoridade para acessar um arquivo ou diretório específico:

- **Autorização de arquivo** -como as páginas ASP.net e os serviços Web são implementados como arquivos que residem no sistema de arquivos do servidor Web, o acesso a esses arquivos pode ser especificado por meio de ACLs (listas de controle de acesso). A autorização de arquivo é usada com mais frequência com a autenticação do Windows porque as ACLs são permissões que se aplicam a contas do Windows. Ao usar a autenticação de formulários, todas as solicitações em nível de sistema operacional e de sistema de arquivos são executadas pela mesma conta do Windows, independentemente do usuário que está visitando o site.
- **Autorização de URL**-com autorização de URL, o desenvolvedor da página especifica regras de autorização em Web. config. Essas regras de autorização especificam quais usuários ou funções têm permissão de acesso ou são negadas de acessar determinadas páginas ou diretórios no aplicativo.

Autorização de arquivo e autorização de URL definem regras de autorização para acessar uma página ASP.NET específica ou para todas as páginas do ASP.NET em um diretório específico. Usando essas técnicas, podemos instruir o ASP.NET a negar solicitações a uma determinada página para um determinado usuário ou permitir o acesso a um conjunto de usuários e negar acesso a todos os outros. E quanto aos cenários em que todos os usuários podem acessar a página, mas a funcionalidade da página depende do usuário? Por exemplo, muitos sites que dão suporte a contas de usuário têm páginas que exibem diferentes conteúdo ou dados para usuários autenticados versus usuários anônimos. Um usuário anônimo pode ver um link para fazer logon no site, enquanto que um usuário autenticado veria uma mensagem como, bem-vindo de volta, *nome* de usuário junto com um link para fazer logoff. Outro exemplo: ao exibir um item em um site de leilão, você verá informações diferentes, dependendo se você for um oferta ou o que está informando o item.

Esses ajustes no nível de página podem ser realizados declarativamente ou programaticamente. Para mostrar conteúdo diferente para anônimos do que usuários autenticados, basta arrastar um [controle LoginView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) para sua página e inserir o conteúdo apropriado em seus modelos AnonymousTemplate e LoggedInTemplate. Como alternativa, você pode determinar de forma programática se a solicitação atual é autenticada, quem é o usuário e a quais funções elas pertencem (se houver). Você pode usar essas informações para mostrar ou ocultar colunas em uma grade ou painéis na página.

Esta série inclui três tutoriais que se concentram na autorização. A ***autorização baseada no usuário***examina como limitar o acesso a uma página ou páginas em um diretório para contas de usuário específicas; A ***autorização baseada em função*** examina o fornecimento de regras de autorização no nível de função; Por fim, a ***exibição do conteúdo com base no tutorial do usuário conectado no momento*** explora a modificação do conteúdo e da funcionalidade de uma página específica com base no usuário que está visitando a página. Para saber mais sobre o ASP. Opções de autorização do NET, consulte [autorização do ASP.net](https://msdn.microsoft.com/library/wce3kxhd.aspx).

## <a name="user-accounts-and-roles"></a>Contas de usuário e funções

ASP. A autenticação de formulários do NET fornece uma infraestrutura para que os usuários façam logon em um site e tenham seu estado autenticado lembrado em visitas à página. E a autorização de URL oferece uma estrutura para limitar o acesso a arquivos ou pastas específicas em um aplicativo ASP.NET. No entanto, nenhum recurso fornece um meio para armazenar informações de conta de usuário ou gerenciar funções.

Antes do ASP.NET 2,0, os desenvolvedores eram responsáveis por criar seus próprios armazenamentos de usuários e de funções. Eles também estavam no gancho para criar as interfaces do usuário e escrever o código para páginas essenciais relacionadas à conta de usuário, como a página de logon e a página para criar uma nova conta, entre outras. Sem qualquer estrutura de conta de usuário interna no ASP.NET, cada desenvolvedor que está implementando contas de usuário tinha que chegar a suas próprias decisões de design sobre perguntas como, Como fazer armazenar senhas ou outras informações confidenciais? e quais diretrizes devo impor com relação ao comprimento e à força da senha?

Hoje, a implementação de contas de usuário em um aplicativo ASP.NET é muito mais simples graças à *estrutura de associação* e aos controles da Web de logon internos. A estrutura de associação é uma grande quantidade de classes no [namespace System. Web. Security](https://msdn.microsoft.com/library/system.web.security.aspx) que fornece funcionalidade para executar tarefas essenciais relacionadas à conta de usuário. A classe de chave na estrutura de associação é a [classe Membership](https://msdn.microsoft.com/library/system.web.security.membership.aspx), que tem métodos como:

- CreateUser
- DeleteUser
- GetAllUsers
- GetUser
- UpdateUser
- ValidateUser

A estrutura de associação usa o [modelo de provedor](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), que separa claramente a API da estrutura de associação de sua implementação. Isso permite que os desenvolvedores usem uma API comum, mas permitem que eles usem uma implementação que atenda às necessidades personalizadas do aplicativo. Em suma, a classe Membership define a funcionalidade essencial da estrutura (os métodos, as propriedades e os eventos), mas não fornece, na verdade, nenhum detalhe de implementação. Em vez disso, os métodos da classe Membership invocam o provedor configurado, que é o que executa o trabalho real. Por exemplo, quando o método CreateUser da classe Membership é invocado, a classe Membership não conhece os detalhes do armazenamento do usuário. Não sabe se os usuários estão sendo mantidos em um banco de dados ou em um arquivo XML ou em alguma outra loja. A classe Membership examina a configuração do aplicativo Web para determinar para qual provedor delegar a chamada, e essa classe de provedor é responsável por realmente criar a nova conta de usuário no repositório de usuários apropriado. Essa interação é ilustrada na Figura 3.

A Microsoft envia duas classes de provedor de associação no .NET Framework:

- [ActiveDirectoryMembershipProvider](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) -implementa a API de associação no Active Directory e servidores de modo de aplicativo Active Directory (Adam).
- [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) -implementa a API de associação em um banco de dados SQL Server.

Esta série de tutoriais se concentra exclusivamente no SqlMembershipProvider.

[![o modelo de provedor permite que diferentes implementações sejam conectadas diretamente à estrutura&lt;/Strong&gt;](security-basics-and-asp-net-support-cs/_static/image4.png)](security-basics-and-asp-net-support-cs/_static/image3.png)

**Figura 03**: o modelo de provedor permite que diferentes implementações sejam conectadas diretamente à estrutura ([clique para exibir a imagem em tamanho normal](security-basics-and-asp-net-support-cs/_static/image5.png))

O benefício do modelo de provedor é que as implementações alternativas podem ser desenvolvidas pela Microsoft, por fornecedores de terceiros ou por desenvolvedores individuais e conectados diretamente à estrutura de associação. Por exemplo, a Microsoft lançou [um provedor de associação para bancos de dados do Microsoft Access](https://download.microsoft.com/download/5/5/b/55bc291f-4316-4fd7-9269-dbf9edbaada8/sampleaccessproviders.vsi). Para obter mais informações sobre os provedores de associação, consulte o [provedor Toolkit](https://msdn.microsoft.com/asp.net/aa336558.aspx), que inclui uma explicação dos provedores de associação, exemplos de provedores personalizados, mais de 100 páginas de documentação sobre o modelo de provedor e o código-fonte completo para os provedores de associação internos (ou seja, ActiveDirectoryMembershipProvider e SqlMembershipProvider).

O ASP.NET 2,0 também introduziu a estrutura de funções. Como a estrutura de associação, a estrutura de funções é criada acima do modelo de provedor. Sua API é exposta por meio da [classe Roles](https://msdn.microsoft.com/library/system.web.security.roles.aspx) e o .NET Framework é fornecido com três classes de provedor:

- [AuthorizationStoreRoleProvider](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx) -gerencia informações de função em um repositório de política do Gerenciador de autorização, como Active Directory ou Adam.
- [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) – implementa funções em um banco de dados SQL Server.
- [WindowsTokenRoleProvider](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx) -associa informações de função com base no grupo do Windows do visitante. Esse método é normalmente usado com a autenticação do Windows.

Esta série de tutoriais se concentra exclusivamente no SqlRoleProvider.

Como o modelo de provedor inclui uma única API direcionada para frente (as classes Membership e Roles), é possível criar funcionalidades em torno dessa API sem precisar se preocupar com os detalhes da implementação. elas são tratadas pelos provedores selecionados pela página desenvolvedor. Essa API unificada permite que os fornecedores da Microsoft e de terceiros criem controles da Web que fazem interface com as estruturas de associação e funções. O ASP.NET é fornecido com vários [controles da Web de logon](https://msdn.microsoft.com/library/ms178329.aspx) para implementar interfaces de usuário de conta de usuário comuns. Por exemplo, o [controle de logon](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx) solicita a um usuário suas credenciais, valida-as e, em seguida, as registra no por meio da autenticação de formulários. O [controle LoginView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) oferece modelos para exibir diferentes marcações para usuários anônimos versus usuários autenticados ou diferentes marcações com base na função do usuário. E o [controle CreateUserWizard](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.aspx) fornece uma interface de usuário passo a passo para criar uma nova conta de usuário.

Embaixo dos bastidores, os vários controles de logon interagem com as estruturas de associação e funções. A maioria dos controles de logon pode ser implementada sem a necessidade de escrever uma única linha de código. Examinaremos esses controles com mais detalhes em Tutoriais futuros, incluindo técnicas para estender e personalizar sua funcionalidade.

## <a name="summary"></a>Resumo

Todos os aplicativos Web que dão suporte a contas de usuário exigem recursos semelhantes, como: a capacidade dos usuários de fazer logon e ter seu status de log lembrado em visitas à página; uma página da Web para novos visitantes criarem uma conta; e a capacidade para o desenvolvedor de página especificar quais recursos, dados e funcionalidade estão disponíveis para os usuários ou funções. As tarefas de autenticação e autorização de usuários e de gerenciamento de contas de usuário e funções são notavelmente fáceis de realizar em aplicativos ASP.NET graças à autenticação de formulários, à autorização de URL e às estruturas de associação e funções.

Ao longo dos próximos tutoriais, examinaremos esses aspectos criando um aplicativo Web de trabalho do zero em um modo passo a passo. Nos próximos dois tutoriais, exploraremos a autenticação de formulários em detalhes. Veremos o fluxo de trabalho de autenticação de formulários em ação, dissecar o tíquete de autenticação de formulários, discutiremos questões de segurança e veremos como configurar o sistema de autenticação de formulários-tudo ao criar um aplicativo Web que permite que os visitantes façam logon e logoff.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Associação, funções, autenticação de formulários e recursos de segurança do ASP.NET 2,0](https://weblogs.asp.net/scottgu/ASP.NET-2.0-Membership_2C00_-Roles_2C00_-Forms-Authentication_2C00_-and-Security-Resources-)
- [Diretrizes de segurança do ASP.NET 2,0](https://msdn.microsoft.com/library/ms998258.aspx)
- [Autenticação do ASP.NET](https://msdn.microsoft.com/library/eeyk640h.aspx)
- [Autorização de ASP.NET](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [Visão geral dos controles de logon do ASP.NET](https://msdn.microsoft.com/library/ms178329.aspx)
- [Examinando a associação, as funções e o perfil do ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Como faço para: proteger meu site usando associação e funções?](https://asp.net/learn/videos/video-45.aspx) Monitor
- [Introdução à associação](https://msdn.microsoft.com/library/yh26yfzy.aspx)
- [Central de desenvolvedores de segurança do MSDN](https://msdn.microsoft.com/security/default.aspx)
- [Gerenciamento de função, associação e segurança do Professional ASP.NET 2,0](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Kit de ferramentas do provedor](https://msdn.microsoft.com/asp.net/aa336558.aspx)

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. O revisor de cliente potencial deste tutorial foi que esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores potenciais para este tutorial incluem Alicja Maziarz, John Suru e Teresa Murphy. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Próximo](an-overview-of-forms-authentication-cs.md)
