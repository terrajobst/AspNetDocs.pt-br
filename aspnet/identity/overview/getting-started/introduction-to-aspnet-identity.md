---
uid: identity/overview/getting-started/introduction-to-aspnet-identity
title: Introdução ao ASP.NET Identity-ASP.NET 4. x
author: jongalloway
description: O sistema de associação ASP.NET foi introduzido com o ASP.NET 2,0 de volta em 2005 e, desde então, houve muitas alterações na maneira como os aplicativos Web são típicos...
ms.author: riande
ms.date: 01/22/2019
ms.assetid: 38717fc1-5989-43cf-952d-4007cc1dd923
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/getting-started/introduction-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 0268dfc16cd2cfb1e79ee14997a4c5eb247af950
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583842"
---
# <a name="introduction-to-aspnet-identity"></a>Introdução à Identidade do ASP.NET

> O sistema de associação ASP.NET foi introduzido com o ASP.NET 2,0 de volta em 2005 e, desde então, houve muitas alterações nas maneiras como os aplicativos Web normalmente lidam com a autenticação e a autorização. ASP.NET Identity é uma visão atualizada do que o sistema de associação deve ser quando você está criando aplicativos modernos para a Web, telefone ou Tablet.

## <a name="background-membership-in-aspnet"></a>Plano de fundo: Associação em ASP.NET

### <a name="aspnet-membership"></a>Associação do ASP.NET

A [associação do ASP.net](https://msdn.microsoft.com/library/yh26yfzy(v=VS.100).aspx) foi projetada para resolver os requisitos de associação do site que eram comuns em 2005, que envolvia a autenticação de formulários e um banco de dados SQL Server para nomes de usuário, senhas, bem como para a entrada de perfil. Hoje, há uma matriz muito mais ampla de opções de armazenamento de dados para aplicativos Web, e a maioria dos desenvolvedores deseja permitir que seus sites usem provedores de identidade social para a funcionalidade de autenticação e autorização. As limitações do design da associação do ASP.NET tornam essa transição difícil:

- O esquema de banco de dados foi projetado para SQL Server e você não pode alterá-lo. Você pode adicionar informações de perfil, mas os dados adicionais são incluídos em uma tabela diferente, o que dificulta o acesso por qualquer meio, exceto por meio da API do provedor de perfil.
- O sistema do provedor permite que você altere o armazenamento de dados de backup, mas o sistema foi projetado em relação às suposições apropriadas para um banco de dado relacional. Você pode escrever um provedor para armazenar informações de associação em um mecanismo de armazenamento não relacional, como tabelas de armazenamento do Azure, mas, em seguida, você precisa contornar o design relacional escrevendo muito código e muitas `System.NotImplementedException` exceções para métodos que não se aplicam a bancos de dados NoSQL.
- Como a funcionalidade de logon/logout é baseada na autenticação de formulários, o sistema de associação não pode usar [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md). O OWIN inclui componentes de middleware para autenticação, incluindo suporte para logons usando provedores de identidade externos (como contas da Microsoft, Facebook, Google, Twitter) e logons usando contas organizacionais do Active Directory local ou Azure Active Directory. O OWIN também inclui suporte para OAuth 2,0, JWT e CORS.

### <a name="aspnet-simple-membership"></a>Associação simples do ASP.NET

A [Associação simples do ASP.net](../../../web-pages/overview/security/16-adding-security-and-membership.md) foi desenvolvida como um sistema de associação para páginas da Web do ASP.net. Ele foi lançado com o WebMatrix e o Visual Studio 2010 SP1. O objetivo da Associação simples era facilitar a adição da funcionalidade de associação a um aplicativo de páginas da Web.

A associação simples facilitou a personalização das informações de perfil do usuário, mas ela ainda compartilha os outros problemas com a associação do ASP.NET e tem algumas limitações:

- Era difícil manter os dados do sistema de associação em um não relational store.
- Você não pode usá-lo com OWIN.
- Ele não funciona bem com os provedores de associação do ASP.NET existentes e não é extensível.

### <a name="aspnet-universal-providers"></a>Provedores Universais ASP.NET

[Provedores universais ASP.net](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) foram desenvolvidas para possibilitar a persistência de informações de associação em banco de dados SQL do Microsoft Azure, e elas também funcionam com SQL Server Compact. Os Provedores Universais foram criados em Entity Framework Code First, o que significa que o Provedores Universais pode ser usado para manter os dados em qualquer repositório com suporte do EF. Com o Provedores Universais, o esquema de banco de dados também foi limpo muito bem.

Os Provedores Universais são criados na infraestrutura de associação do ASP.NET, portanto, eles ainda têm as mesmas limitações que o provedor de usermember. Ou seja, foram projetados para bancos de dados relacionais e é difícil personalizar o perfil e as informações do usuário. Esses provedores ainda usam a autenticação de formulários para a funcionalidade de entrada e saída.

## <a name="aspnet-identity"></a>ASP.NET Identity

Como a história da associação em ASP.NET evoluiu ao longo dos anos, a equipe de ASP.NET aprendeu muito com os comentários dos clientes.

A suposição de que os usuários entrarão inserindo um nome de usuário e uma senha que eles registraram em seu próprio aplicativo não são mais válidas. A Web se tornou mais social. Os usuários estão interagindo entre si em tempo real por meio de canais sociais, como Facebook, Twitter e outros sites sociais. Os desenvolvedores querem que os usuários possam entrar com suas identidades sociais para que possam ter uma experiência rica em seus sites. Um sistema de associação moderno deve habilitar os logons baseados em redirecionamento para provedores de autenticação, como Facebook, Twitter e outros.

À medida que o desenvolvimento da Web evoluiu, os padrões do desenvolvimento para a Web. O teste de unidade do código do aplicativo tornou-se uma preocupação principal para os desenvolvedores de aplicativos. No 2008 ASP.NET adicionou uma nova estrutura com base no padrão MVC (Model-View-Controller), em parte, para ajudar os desenvolvedores a criar aplicativos ASP.NET de teste de unidade. Os desenvolvedores que desejavam testar a unidade lógica do aplicativo também queriam poder fazer isso com o sistema de associação.

Considerando essas alterações no desenvolvimento de aplicativos Web, ASP.NET Identity foi desenvolvida com os seguintes objetivos:

- **Um sistema ASP.NET Identity**

    - ASP.NET Identity pode ser usado com todas as estruturas ASP.NET, como ASP.NET MVC, Web Forms, páginas da Web, API da Web e Signalr.
    - ASP.NET Identity pode ser usado quando você estiver criando aplicativos Web, de telefone, de repositório ou híbridos.
- **Facilidade de conexão de dados de perfil sobre o usuário**

    - Você tem controle sobre o esquema de informações de perfil e de usuário. Por exemplo, você pode facilmente permitir que o sistema armazene datas de nascimento inseridas pelos usuários ao registrar uma conta em seu aplicativo.

- **Controle de persistência**

    - Por padrão, o sistema ASP.NET Identity armazena todas as informações do usuário em um banco de dados. ASP.NET Identity usa Entity Framework Code First para implementar todo o seu mecanismo de persistência.
    - Como você controla o esquema de banco de dados, tarefas comuns, como alterar nomes de tabela ou alterar o tipo de dados de chaves primárias, são simples de fazer.
    - É fácil conectar diferentes mecanismos de armazenamento, como o SharePoint, o serviço tabela de armazenamento do Azure, bancos de dados NoSQL, etc., sem a necessidade de gerar `System.NotImplementedExceptions` exceções.
- **Capacidade de teste de unidade**

    - ASP.NET Identity torna o aplicativo Web mais um teste de unidade. Você pode escrever testes de unidade para as partes do seu aplicativo que usam ASP.NET Identity.
- **Provedor de função**

    - Há um provedor de função que permite restringir o acesso a partes do seu aplicativo por funções. Você pode criar facilmente funções como "administrador" e adicionar usuários a funções.
- **Baseado em declarações**

    - O ASP.NET Identity dá suporte à autenticação baseada em declarações, em que a identidade do usuário é representada como um conjunto de declarações. As declarações permitem que os desenvolvedores sejam muito mais expressivos na descrição da identidade de um usuário do que as funções permitem. Enquanto a associação de função é apenas um booliano (membro ou não membro), uma declaração pode incluir informações avançadas sobre a identidade e a associação do usuário.
- **Provedores de logon social**

    - Você pode facilmente adicionar logs sociais, como conta da Microsoft, Facebook, Twitter, Google e outros, ao seu aplicativo e armazenar os dados específicos do usuário em seu aplicativo.

- **Integração do OWIN**

    - A autenticação ASP.NET agora é baseada no middleware OWIN que pode ser usado em qualquer host baseado em OWIN. ASP.NET Identity não tem nenhuma dependência no System. Web. É uma estrutura OWIN totalmente compatível e pode ser usada em qualquer aplicativo hospedado por OWIN.
    - ASP.NET Identity usa a autenticação OWIN para fazer logon/logoff de usuários no site. Isso significa que, em vez de usar o FormsAuthentication para gerar o cookie, o aplicativo usa OWIN CookieAuthentication para fazer isso.
- **Pacote NuGet**

    - ASP.NET Identity é redistribuído como um pacote NuGet que é instalado no ASP.NET MVC, Web Forms e modelos de API Web fornecidos com o Visual Studio 2017. Você pode baixar este pacote NuGet na galeria do NuGet.
    - Liberar ASP.NET Identity como um pacote NuGet torna mais fácil para a equipe de ASP.NET iterar os novos recursos e correções de bugs e fornecê-los para os desenvolvedores de maneira ágil.

## <a name="get-started-with-aspnet-identity"></a>Introdução ao ASP.NET Identity

ASP.NET Identity é usado nos modelos de projeto do Visual Studio 2017 para ASP.NET MVC, Web Forms, API Web e SPA. Neste tutorial, Ilustraremos como os modelos de projeto usam ASP.NET Identity para adicionar funcionalidade para registrar, entrar e sair de um usuário.

ASP.NET Identity é implementado usando o procedimento a seguir. A finalidade deste artigo é fornecer uma visão geral de alto nível do ASP.NET Identity; Você pode seguir o passo a passo ou apenas ler os detalhes. Para obter instruções mais detalhadas sobre como criar aplicativos usando ASP.NET Identity, incluindo o uso da nova API para adicionar usuários, funções e informações de perfil, consulte a seção próximas etapas no final deste artigo.

1. Crie um aplicativo MVC ASP.NET com contas individuais. Você pode usar ASP.NET Identity no ASP.NET MVC, Web Forms, API da Web, Signalr etc. Neste artigo, vamos começar com um aplicativo MVC ASP.NET.  
  
    ![](introduction-to-aspnet-identity/_static/image1.png)
2. O projeto criado contém os três pacotes a seguir para ASP.NET Identity.

    - [`Microsoft.AspNet.Identity.EntityFramework`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/)  
   Este pacote tem a implementação Entity Framework do ASP.NET Identity que persistirá os dados e o esquema do ASP.NET Identity para SQL Server.
    - [`Microsoft.AspNet.Identity.Core`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Core/)  
   Este pacote tem as interfaces principais para ASP.NET Identity. Esse pacote pode ser usado para gravar uma implementação para ASP.NET Identity que tem como alvo diferentes repositórios de persistência, como o armazenamento de tabelas do Azure, bancos de dados NoSQL, etc.
    - [`Microsoft.AspNet.Identity.OWIN`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Owin/)  
   Este pacote contém a funcionalidade que é usada para conectar a autenticação OWIN com ASP.NET Identity em aplicativos ASP.NET. Isso é usado quando você adiciona a funcionalidade de entrada ao seu aplicativo e chama o middleware de autenticação de cookie OWIN para gerar um cookie.
3. Criando um usuário.  
   Inicie o aplicativo e, em seguida, clique no link **registrar** para criar um usuário. A imagem a seguir mostra a página de registro que coleta o nome de usuário e a senha.  
  
    ![](introduction-to-aspnet-identity/_static/image2.png)  
  
   Quando o usuário seleciona o botão **registrar** , a ação `Register` do controlador da conta cria o usuário chamando a API ASP.net Identity, conforme destacado abaixo:

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample1.cs?highlight=8-9)]
4. Entrar.  
   Se o usuário foi criado com êxito, ele está conectado pelo método `SignInAsync`.  

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample6.cs?highlight=12)]

   O método `SignInManager.SignInAsync` gera um [ClaimsIdentity](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx). Como a autenticação de cookie ASP.NET Identity e OWIN é um sistema baseado em declarações, a estrutura requer que o aplicativo gere um ClaimsIdentity para o usuário. ClaimsIdentity tem informações sobre todas as declarações para o usuário, como as funções às quais o usuário pertence.   
 
5. Faça logoff.  
   Selecione o link **fazer** logoff para chamar a ação de logoff no controlador da conta. 

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample5.cs?highlight=6)]

   O código realçado acima mostra o método de `AuthenticationManager.SignOut` OWIN. Isso é análogo ao método [FormsAuthentication. SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx) usado pelo módulo [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) no Web Forms.

## <a name="components-of-aspnet-identity"></a>Componentes do ASP.NET Identity

O diagrama a seguir mostra os componentes do sistema de ASP.NET Identity (selecione [neste ou no](introduction-to-aspnet-identity/_static/image3.png) diagrama para ampliá-lo). Os pacotes em verde compõem o sistema de ASP.NET Identity. Todos os outros pacotes são dependências que são necessárias para usar o sistema de ASP.NET Identity em aplicativos ASP.NET.

[![](introduction-to-aspnet-identity/_static/image5.png)](introduction-to-aspnet-identity/_static/image4.png)

Veja a seguir uma breve descrição dos pacotes NuGet não mencionados anteriormente:

- [Microsoft.Owin.Security.Cookies](http://www.nuget.org/packages/Microsoft.Owin.Security.Cookies/)  
 Middleware que permite que um aplicativo use autenticação baseada em cookie, semelhante ao ASP. Autenticação de formulários do NET.
- [EntityFramework](http://www.nuget.org/packages/EntityFramework/)  
 Entity Framework é a tecnologia de acesso a dados recomendada pela Microsoft para bancos de dados relacionais.

## <a name="migrating-from-membership-to-aspnet-identity"></a>Migrando da Associação para ASP.NET Identity

Esperamos fornecer orientações em breve sobre como migrar seus aplicativos existentes que usam associação ASP.NET ou associação simples ao novo sistema de ASP.NET Identity.

## <a name="next-steps"></a>Próximas etapas

- [Criar um aplicativo ASP.NET MVC 5 com o Facebook e o Google OAuth2 e o logon OpenID](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)  
 O tutorial usa a API ASP.NET Identity para adicionar informações de perfil ao banco de dados do usuário e como autenticar com o Google e o Facebook.
- [Criar um aplicativo ASP.NET MVC com autenticação e o banco de dados SQL e implantar no Serviço de Aplicativo do Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)  
 Este tutorial mostra como usar a API de identidade para adicionar usuários e funções.
- [https://github.com/rustd/AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample)  
 Aplicativo de exemplo que mostra como adicionar funções básicas e suporte ao usuário e como fazer funções e gerenciamento de usuários.
