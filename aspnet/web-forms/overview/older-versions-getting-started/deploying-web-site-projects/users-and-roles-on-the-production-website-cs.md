---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs
title: Usuários e funções no site de produção (C#) | Microsoft Docs
author: rick-anderson
description: A ferramenta de administração de sites do ASP.NET (WSAT) fornece uma interface do usuário baseada na Web para definir configurações de associação e funções e para criar, editar, um...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: dbc54313-5d05-4285-98b3-726edea6d0c9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs
msc.type: authoredcontent
ms.openlocfilehash: c47bd2c1661f129dd8856916de04b8ba459fbfec
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74616524"
---
# <a name="users-and-roles-on-the-production-website-c"></a>Usuários e funções no site de produção (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar PDF](https://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial16_CustomAWAT_cs.pdf)

> A ferramenta de administração de sites do ASP.NET (WSAT) fornece uma interface do usuário baseada na Web para definir configurações de associação e funções e para criar, editar e excluir usuários e funções. Infelizmente, o WSAT só funciona quando visitado do localhost, o que significa que você não pode acessar a ferramenta de administração do site de produção por meio do seu navegador. A boa notícia é que há soluções alternativas que possibilitam o gerenciamento de usuários e funções na produção. Este tutorial analisa essas soluções alternativas e outras.

## <a name="introduction"></a>Introdução

O ASP.NET 2,0 introduziu vários *serviços de aplicativos*, que são um pacote de serviços de blocos de construção que você pode adicionar ao seu aplicativo Web. Adicionamos os serviços de associação e funções ao site de revisões de livros de volta no [tutorial *Configurando um site que usa serviços de aplicativos* ](configuring-a-website-that-uses-application-services-cs.md). O serviço de associação facilita a criação e o gerenciamento de contas de usuário; o serviço de funções oferece uma API para categorizar usuários em grupos. O site de análises de livros tem três contas de usuário – Scott, Jisun e Alice – e uma única função, administrador, com Scott e Jisun na função de administrador.

ASP. Os serviços de aplicativos da rede não estão vinculados a uma implementação específica. Em vez disso, você instrui os serviços de aplicativo a usar um *provedor*específico e esse provedor implementa o serviço usando uma tecnologia específica. Configuramos o aplicativo Web de análises de livros para usar os provedores de `SqlMembershipProvider` e `SqlRoleProvider` para os serviços de associação e funções. Esses dois provedores armazenam informações de conta e função de usuário em um banco de dados SQL Server e são os provedores usados com mais frequência para aplicativos Web baseados na Internet hospedados em uma empresa de hospedagem na Web.

Um desafio comum para os desenvolvedores que usam os serviços de associação e funções é gerenciar os usuários e funções no ambiente de produção. Como excluir uma conta de usuário do site de produção, adicionar uma nova função ou adicionar um usuário existente a uma função existente? Este tutorial explora diferentes técnicas para gerenciar usuários e funções no site de produção.

## <a name="using-the-aspnet-web-site-administration-tool"></a>Usando a ferramenta de administração de site do ASP.NET

O ASP.NET inclui uma [ferramenta de administração de site](https://msdn.microsoft.com/library/yy40ytx0.aspx) (WSAT) que facilita a criação e o gerenciamento de contas e funções de usuário e a especificação de regras de autorização baseadas em usuário e função. Para usar o WSAT, clique no ícone de configuração do ASP.NET na Gerenciador de Soluções ou vá até o site ou menu do projeto e escolha a opção de configuração ASP.NET. Qualquer uma das abordagens inicia um navegador da Web e a aponta para o WSAT em um endereço como: `http://localhost:portNumber/asp.netwebadminfiles/default.aspx?applicationPhysicalPath=pathToApplication`

O WSAT é dividido em três seções:

- **Segurança** -gerencie usuários, funções e regras de autorização.
- **ApplicationConfiguration** -gerencia o &lt;appsettings&gt; e as configurações de SMTP aqui. Você também pode colocar o aplicativo offline e gerenciar configurações de depuração e rastreamento aqui, bem como especificar a página de erro personalizada padrão.
- **ProviderConfiguration** -configure os provedores usados pelos serviços de aplicativo.

A seção segurança (mostrada na **Figura 1**) inclui links para a criação de novos usuários, o gerenciamento de usuários, a criação e o gerenciamento de funções e a criação e gerenciamento de regras de acesso. A partir daqui, você pode adicionar uma nova função ao sistema, excluir um usuário existente ou adicionar ou remover funções de uma conta de usuário específica.

[![](users-and-roles-on-the-production-website-cs/_static/image2.png)](users-and-roles-on-the-production-website-cs/_static/image1.png)

**Figura 1**: a seção de segurança do WSAT inclui opções para gerenciar usuários e funções  
([Clique para exibir a imagem em tamanho normal](users-and-roles-on-the-production-website-cs/_static/image3.png))

Infelizmente, o WSAT só é acessível localmente. Você não pode visitar o WSAT em seu site de produção remoto; Se você visitar `www.yoursite.com/asp.netwebadminfiles/default.aspx` obter uma resposta 404 não encontrada. O código que alimenta o WSAT usa as classes `Membership` e `Roles` no .NET Framework para criar, editar e excluir usuários e funções. Essas classes consultam as informações de configuração do aplicativo Web para determinar qual provedor usar; de volta ao [tutorial *configurar um site que usa serviços de aplicativos, configuramos* ](configuring-a-website-that-uses-application-services-cs.md) o site de revisões de livros para usar os provedores de `SqlMembershipProvider` e `SqlRoleProvider`. Isso envolveu a adição de seções `<membership>` e `<roleManager>` a `Web.config`.

[!code-xml[Main](users-and-roles-on-the-production-website-cs/samples/sample1.xml)]

Observe que as seções `<membership>` e `<roleManager>` fazem referência aos provedores `SqlMembershipProvider` e `SqlRoleProvider` no atributo `type`, respectivamente. Esses provedores armazenam as informações de usuário e função em um banco de dados SQL Server especificado. O banco de dados usado por esses provedores é especificado pelo atributo `connectionStringName`, `ReviewsConnectionString`, que é definido no arquivo `~/ConfigSections/databaseConnectionStrings.config`. Lembre-se de que o arquivo de `databaseConnectionStrings.config` no ambiente de desenvolvimento contém a cadeia de conexão para o banco de dados de desenvolvimento, enquanto o arquivo de `databaseConnectionStrings.config` na produção contém a cadeia de conexão para o banco de dados de produção.

Resumindo, o WSAT deve ser acessado localmente por meio do ambiente de desenvolvimento e funciona com as informações de usuário e função no banco de dados especificado no arquivo de `databaseConnectionStrings.config`. Consequentemente, se alterarmos as informações da cadeia de conexão no arquivo de `databaseConnectionStrings.config` no ambiente de desenvolvimento, poderemos usar o WSAT localmente para gerenciar usuários e funções no ambiente de produção.

Para ilustrar essa funcionalidade, abra o arquivo `databaseConnectionStrings.config` no Visual Studio no ambiente de desenvolvimento e substitua a cadeia de conexão do banco de dados de desenvolvimento pela cadeia de conexão do banco de dados de produção. Em seguida, inicie o WSAT, vá para a guia Segurança e adicione um novo usuário chamado Sam com a senha "senha!" (menos as aspas). A **Figura 2** mostra a tela WSAT ao criar essa conta.

[![](users-and-roles-on-the-production-website-cs/_static/image5.png)](users-and-roles-on-the-production-website-cs/_static/image4.png)

**Figura 2**: criar um novo usuário chamado Sam no ambiente de produção  
([Clique para exibir a imagem em tamanho normal](users-and-roles-on-the-production-website-cs/_static/image6.png))

Como alteramos a cadeia de conexão em `databaseConnectionStrings.config` para apontar para o servidor de banco de dados de produção, o Sam foi adicionado como um usuário no ambiente de produção. Para verificar isso, altere a cadeia de conexão no arquivo de `databaseConnectionStrings.config` de volta para o banco de dados de desenvolvimento e visite a página `Login.aspx` no ambiente de desenvolvimento. Tente entrar como Sam (consulte a **Figura 3**).

[![](users-and-roles-on-the-production-website-cs/_static/image8.png)](users-and-roles-on-the-production-website-cs/_static/image7.png)

**Figura 3**: não é possível entrar como Sam no ambiente de desenvolvimento  
([Clique para exibir a imagem em tamanho normal](users-and-roles-on-the-production-website-cs/_static/image9.png))

Você não pode entrar como Sam no ambiente de desenvolvimento porque as informações de conta de usuário não existem no banco de dados local. Em vez disso, o foi adicionado ao banco de dados de produção. Para verificar isso, exiba o conteúdo da tabela `aspnet_Users` nos bancos de dados de desenvolvimento e de produção. No ambiente de desenvolvimento, deve haver apenas três registros para os usuários Scott, Jisun e Alice. No entanto, a tabela `aspnet_Users` no banco de dados de produção tem quatro registros: Scott, Jisun, Alice e Sam. Consequentemente, o Sam pode entrar por meio do site em produção, mas não por meio do ambiente de desenvolvimento.

[![](users-and-roles-on-the-production-website-cs/_static/image11.png)](users-and-roles-on-the-production-website-cs/_static/image10.png)

**Figura 4**: Sam pode entrar no site de produção  
([Clique para exibir a imagem em tamanho normal](users-and-roles-on-the-production-website-cs/_static/image12.png))

> [!NOTE]
> Não se esqueça de alterar a cadeia de conexão no arquivo de `databaseConnectionStrings.config` de volta para a cadeia de conexão do banco de dados de desenvolvimento quando terminar de trabalhar com o WSAT, caso contrário, você trabalhará com o trabalho de produção ao testar o site por meio do ambiente de desenvolvimento. Também tenha em mente que, embora a técnica que acabamos de discutir nos permita usar o WSAT para gerenciar usuários e funções remotamente, as alterações em qualquer uma das outras opções de configuração do WSAT (regras de acesso, configurações de SMTP, configurações de depuração e rastreamento e assim por diante) modificam o arquivo de `Web.config`. Consequentemente, todas as alterações feitas nas configurações se aplicam ao ambiente de desenvolvimento e não ao ambiente de produção.

## <a name="creating-custom-user-and-role-management-web-pages"></a>Criando páginas da Web de gerenciamento de usuário e função personalizadas

O WSAT fornece um sistema pronto para gerenciar usuários e funções, mas só pode ser iniciado localmente e requer alterações nas informações da cadeia de conexão para gerenciar os usuários e as funções na produção. A maioria dos sites que dão suporte a contas de usuário também incluem várias páginas da Web de administração de usuários e funções que permitem que os administradores gerenciem usuários e funções de páginas no site. Essas páginas de administração baseadas na Web facilitam muito o gerenciamento de usuários e funções e são essenciais para sites em que pode haver muitos administradores ou administradores que não têm acesso ao ou a experiência técnica para usar o Visual Studio para iniciar o WSAT.

O ASP.NET inclui vários controles da Web relacionados ao logon internos que tornam a implementação de muitas dessas páginas da Web administrativas tão fácil quanto arrastar e soltar. Por exemplo, você pode criar uma página para que os administradores criem uma nova conta de usuário arrastando o controle CreateUserWizard para a página e definindo algumas propriedades. Na verdade, a página para criar usuários na WSAT mostrada na **Figura 2** usa o mesmo controle CreateUserWizard que você pode adicionar às suas páginas. Além disso, as funcionalidades dos serviços de associação e funções estão disponíveis programaticamente por meio das classes `Membership` e `Roles` no .NET Framework. Com essas classes, você pode escrever código para criar, editar e excluir usuários e funções, bem como para adicionar ou remover usuários de funções, para determinar quais usuários estão em quais funções e executar outras tarefas relacionadas a usuários e funções.

No tutorial [ *Configurando um site que usa serviços de aplicativos* ](configuring-a-website-that-uses-application-services-cs.md) , adicionei uma página à pasta `Admin` chamada `CreateAccount.aspx`. Esta página permite que um administrador adicione uma nova conta de usuário ao site e especifique se o usuário recém-criado está ou não na função de administrador (consulte a **Figura 5**).

[![](users-and-roles-on-the-production-website-cs/_static/image14.png)](users-and-roles-on-the-production-website-cs/_static/image13.png)

**Figura 5**: os administradores podem criar novas contas de usuário  
([Clique para exibir a imagem em tamanho normal](users-and-roles-on-the-production-website-cs/_static/image15.png))

Para obter uma visão mais detalhada da criação de páginas de administração de usuário e função, juntamente com as instruções passo a passos sobre como usar as classes `Membership` e `Roles` e os controles da Web ASP.NET relacionados ao logon, leia meus [tutoriais de segurança do site](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md). Lá, você encontrará orientações sobre como criar páginas da Web para criar novas contas, criar e gerenciar funções, atribuir usuários a funções e outras tarefas administrativas comuns.

Para implementar a funcionalidade do tipo WSAT no site de produção, você sempre pode criar sua própria série de páginas da Web que implementam os recursos do WSAT. Para ajudar a começar, confira o código-fonte WSAT, localizado na pasta `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\ASP.NETWebAdminFiles`. Outra opção é usar a alternativa de WSAT de Dan Clem, que ele compartilha em seu artigo, [distribuindo sua própria ferramenta de administração de site](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx). Dan percorre os leitores pelo processo de criação de uma ferramenta semelhante a WSAT personalizada, inclui o código-fonte do aplicativo para download C#(em) e fornece instruções passo a passo para adicionar seu WSAT personalizado a um site hospedado.

## <a name="summary"></a>Resumo

A ferramenta de administração de site ASP.NET (WSAT) pode ser usada em conjunto com os serviços de aplicativo de associação e funções para gerenciar informações de usuário e função para seu site. Infelizmente, o WSAT só é acessível localmente e não pode ser acessado em seu site de produção. No entanto, alterando a cadeia de conexão no ambiente de desenvolvimento para apontar para o banco de dados de produção, você pode usar o WSAT para gerenciar os usuários e as funções no site de produção.

Embora a abordagem WSAT proporciona uma maneira rápida e fácil de gerenciar usuários e funções, ela precisa iniciar o WSAT do Visual Studio, bem como as alterações temporárias nas informações da cadeia de conexão. O WSAT oferece uma maneira rápida de gerenciar usuários e funções na produção, mas é trabalhoso e não funciona bem para sites com vários administradores ou com administradores que não têm o ou não estão familiarizados com o Visual Studio e o WSAT. Por esses motivos, a maioria dos sites que dão suporte a contas de usuário incluem um conjunto de páginas da Web administrativas. Esse conjunto de páginas da Web elimina a necessidade de WSAT e é usado por vários usuários administrativos de qualquer computador.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Examinando ASP. Associação, funções e perfil da rede](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Distribuindo sua própria ferramenta de administração de site](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [Visão geral da ferramenta de administração de site](https://msdn.microsoft.com/library/yy40ytx0.aspx)
- [Tutoriais de segurança do site](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> [Anterior](precompiling-your-website-cs.md)
> [Próximo](asp-net-hosting-options-vb.md)
