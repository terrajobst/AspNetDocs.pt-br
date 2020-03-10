---
uid: mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
title: Proteger aplicativos usando autenticação e autorização | Microsoft Docs
author: microsoft
description: A etapa 9 mostra como adicionar autenticação e autorização para proteger nosso aplicativo NerdDinner, para que os usuários precisem se registrar e fazer logon no site para criar...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 9e4d5cac-b071-440c-b044-20b6d0c964fb
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
msc.type: authoredcontent
ms.openlocfilehash: 8d509c5f15bb4d5014e53b8dc2a736454238e72c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78600978"
---
# <a name="secure-applications-using-authentication-and-authorization"></a>Proteger aplicativos usando autenticação e autorização

pela [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Esta é a etapa 9 de um [tutorial de aplicativo "NerdDinner"](introducing-the-nerddinner-tutorial.md) gratuito que percorre como criar um aplicativo Web pequeno, mas completo usando o ASP.NET MVC 1.
> 
> A etapa 9 mostra como adicionar autenticação e autorização para proteger nosso aplicativo NerdDinner, para que os usuários precisem se registrar e fazer logon no site para criar novos jantares, e apenas o usuário que está hospedando um jantar pode editá-lo mais tarde.
> 
> Se você estiver usando o ASP.NET MVC 3, recomendamos seguir as [introdução com](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) os tutoriais da [loja de música](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) MVC 3 ou MVC.

## <a name="nerddinner-step-9-authentication-and-authorization"></a>Etapa 9 do NerdDinner: autenticação e autorização

Agora, nosso aplicativo NerdDinner concede a qualquer pessoa que visite o site a capacidade de criar e editar os detalhes de qualquer jantar. Vamos alterar isso para que os usuários precisem se registrar e fazer logon no site para criar novos jantares e adicionar uma restrição para que apenas o usuário que está hospedando um jantar possa editá-lo mais tarde.

Para habilitar isso, usaremos a autenticação e a autorização para proteger nosso aplicativo.

### <a name="understanding-authentication-and-authorization"></a>Noções básicas sobre autenticação e autorização

A *autenticação* é o processo de identificar e validar a identidade de um cliente que está acessando um aplicativo. Coloque mais simplesmente, trata-se de identificar "quem" é o usuário final quando visita um site. O ASP.NET dá suporte a várias maneiras de autenticar usuários do navegador. Para aplicativos Web da Internet, a abordagem de autenticação mais comum usada é chamada de "autenticação de formulários". A autenticação de formulários permite que um desenvolvedor crie um formulário de logon em HTML dentro de seu aplicativo e, em seguida, valide o nome de usuário/senha enviado por usuários finais em um banco de dados ou outro armazenamento de credenciais de senha. Se a combinação de nome de usuário/senha estiver correta, o desenvolvedor poderá pedir ao ASP.NET para emitir um cookie HTTP criptografado para identificar o usuário em solicitações futuras. Vamos usar a autenticação de formulários com nosso aplicativo NerdDinner.

A *autorização* é o processo de determinar se um usuário autenticado tem permissão para acessar uma URL/um recurso específico ou para executar alguma ação. Por exemplo, em nosso aplicativo NerdDinner, vamos autorizar que somente os usuários que fizeram logon possam acessar a URL do */Dinners/Create* e criar novos jantares. Também vamos querer adicionar a lógica de autorização para que apenas o usuário que está hospedando um jantar possa editá-la e negar acesso de edição a todos os outros usuários.

### <a name="forms-authentication-and-the-accountcontroller"></a>Autenticação de formulários e o AccountController

O modelo de projeto padrão do Visual Studio para o ASP.NET MVC habilita automaticamente a autenticação de formulários quando novos aplicativos MVC ASP.NET são criados. Ele também adiciona automaticamente uma implementação de página de logon de conta pré-criada ao projeto, o que torna muito fácil integrar a segurança em um site.

A página mestra padrão site. Master exibe um link "logon" na parte superior direita do site quando o usuário que o acessa não está autenticado:

![](secure-applications-using-authentication-and-authorization/_static/image1.png)

Clicar no link "logon" leva um usuário para a URL */Account/logon* :

![](secure-applications-using-authentication-and-authorization/_static/image2.png)

Os visitantes que não se registraram podem fazer isso clicando no link "registrar" – que os levará para a URL */Account/Register* e permitirão que eles insiram os detalhes da conta:

![](secure-applications-using-authentication-and-authorization/_static/image3.png)

Clicar no botão "registrar" criará um novo usuário no sistema de associação do ASP.NET e autenticará o usuário no site usando a autenticação de formulários.

Quando um usuário faz logon, o site. Master altera o canto superior direito da página para gerar uma saída "bem-vindo [username]!" mensagem e renderiza um link "fazer logoff" em vez de um "logon". Clicar no link "fazer logoff" faz logoff do usuário:

![](secure-applications-using-authentication-and-authorization/_static/image4.png)

A funcionalidade de logon, Logout e registro acima é implementada dentro da classe AccountController que foi adicionada ao nosso projeto pelo Visual Studio quando ele criou o projeto. A interface do usuário para o AccountController é implementada usando modelos de exibição dentro do diretório \Views\Account:

![](secure-applications-using-authentication-and-authorization/_static/image5.png)

A classe AccountController usa o sistema de autenticação de formulários do ASP.NET para emitir cookies de autenticação criptografados e a API de associação do ASP.NET para armazenar e validar nomes de acessações/senhas. A API de associação do ASP.NET é extensível e permite que qualquer repositório de credenciais de senha seja usado. O ASP.NET é fornecido com implementações de provedor de associação internas que armazenam nome de usuário/senhas em um banco de dados SQL ou dentro de Active Directory.

Podemos configurar qual provedor de associação nosso aplicativo NerdDinner deve usar abrindo o arquivo "Web. config" na raiz do projeto e procurando a &lt;Associação&gt; seção dentro dele. O Web. config padrão adicionado quando o projeto foi criado registra o provedor de associação do SQL e o configura para usar uma cadeia de conexão chamada "ApplicationServices" para especificar o local do banco de dados.

A cadeia de conexão padrão "ApplicationServices" (que é especificada na seção &lt;connectionStrings&gt; do arquivo Web. config) está configurada para usar o SQL Express. Ele aponta para um banco de dados do SQL Express chamado "ASPNETDB. MDF "no diretório" dados de\_de aplicativos "do aplicativo. Se esse banco de dados não existir na primeira vez em que a API de associação for usada dentro do aplicativo, o ASP.NET criará automaticamente o banco de dados e provisionrá o esquema de banco de dados de associação apropriado dentro dele:

![](secure-applications-using-authentication-and-authorization/_static/image6.png)

Se, em vez de usar o SQL Express, quiséssemos usar uma instância de SQL Server completa (ou conectar-se a um banco de dados remoto), tudo o que precisamos fazer é atualizar a cadeia de conexão "ApplicationServices" no arquivo Web. config e certificar-se de que o esquema de associação apropriado foi adicionado ao banco de dados no qual aponta. Você pode executar o utilitário "ASPNET\_RegSql. exe" dentro do diretório \Windows\Microsoft.NET\Framework\v2.0.50727\ para adicionar o esquema apropriado para associação e os outros serviços de aplicativo do ASP.NET a um banco de dados.

### <a name="authorizing-the-dinnerscreate-url-using-the-authorize-filter"></a>Autorizando a URL do/Dinners/Create usando o filtro [autorizar]

Não precisamos escrever nenhum código para habilitar uma implementação de autenticação segura e gerenciamento de conta para o aplicativo NerdDinner. Os usuários podem registrar novas contas com nosso aplicativo e fazer logon/logout do site.

Agora, podemos adicionar a lógica de autorização ao aplicativo e usar o status de autenticação e o nome de usuário dos visitantes para controlar o que eles podem ou não fazer no site. Vamos começar adicionando a lógica de autorização aos métodos de ação "Create" de nossa classe DinnersController. Especificamente, precisaremos que os usuários que acessam a URL */Dinners/Create* devem estar conectados. Se eles não estiverem registrados, redirecionaremos-os para a página de logon para que possam entrar.

A implementação dessa lógica é muito fácil. Tudo o que precisamos fazer é adicionar um atributo de filtro [Authorize] aos nossos métodos Create Action da seguinte forma:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample1.cs)]

O ASP.NET MVC dá suporte à capacidade de criar "filtros de ação" que podem ser usados para implementar a lógica reutilizável que pode ser aplicada declarativamente a métodos de ação. O filtro [autorizar] é um dos filtros de ação internos fornecidos pelo ASP.NET MVC e permite que um desenvolvedor aplique declarativamente regras de autorização a métodos de ação e classes de controlador.

Quando aplicado sem parâmetros (como acima), o filtro [autorizar] impõe que o usuário que está fazendo a solicitação do método de ação deve estar conectado – e ele redirecionará automaticamente o navegador para a URL de logon, se não for. Ao fazer esse redirecionamento, a URL solicitada originalmente é passada como um argumento QueryString (por exemplo:/Account/LogOn? ReturnUrl =% 2fDinners% 2fCreate). Em seguida, o AccountController redirecionará o usuário para a URL solicitada originalmente depois de fazer logon.

Opcionalmente, o filtro [autorizar] dá suporte à capacidade de especificar uma propriedade "Users" ou "Roles" que pode ser usada para exigir que o usuário esteja conectado e dentro de uma lista de usuários permitidos ou um membro de uma função de segurança permitida. Por exemplo, o código a seguir permite apenas dois usuários específicos, "scottgu" e "billg", para acessar a URL/Dinners/Create:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample2.cs)]

No entanto, a inserção de nomes de usuário específicos no código tende a ser bastante dessustentável. Uma abordagem melhor é definir as "funções" de nível superior que o código verifica e, em seguida, mapear usuários para a função usando um banco de dados ou um sistema do Active Directory (permitindo que a lista real de mapeamentos de usuários seja armazenada externamente a partir do código). O ASP.NET inclui uma API de gerenciamento de função interna, bem como um conjunto interno de provedores de função (incluindo aqueles para SQL e Active Directory) que pode ajudar a executar esse mapeamento de usuário/função. Em seguida, podemos atualizar o código para permitir que apenas os usuários em uma função específica de "administrador" acessem a URL do/Dinners/Create:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample3.cs)]

### <a name="using-the-useridentityname-property-when-creating-dinners"></a>Usando a propriedade User.Identity.Name ao criar jantares

Podemos recuperar o nome do usuário conectado no momento de uma solicitação usando a propriedade User.Identity.Name exposta na classe base do controlador.

Antes de implementarmos a versão HTTP-POST do nosso método de ação Create (), codificamos a propriedade "HostedBy" do jantar em uma cadeia de caracteres estática. Agora, podemos atualizar esse código para usar a propriedade User.Identity.Name, bem como adicionar automaticamente um RSVP para o host que cria o jantar:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample4.cs)]

Como adicionamos um atributo [Authorize] ao método Create (), o ASP.NET MVC garante que o método de ação seja executado somente se o usuário que visitar a URL/Dinners/Create estiver conectado no site. Como tal, o valor da propriedade User.Identity.Name sempre conterá um nome de usuário válido.

### <a name="using-the-useridentityname-property-when-editing-dinners"></a>Usando a propriedade User.Identity.Name ao editar jantares

Agora, vamos adicionar uma lógica de autorização que restringe os usuários para que eles possam editar apenas as propriedades dos JANTARS que eles mesmos estão hospedando.

Para ajudar com isso, primeiro adicionaremos um método auxiliar "IsHostedBy (username)" ao nosso objeto de jantar (dentro da classe parcial Dinner.cs que criamos anteriormente). Esse método auxiliar retorna true ou false dependendo se um nome de usuário fornecido corresponde à propriedade HostedBy do jantar e encapsula a lógica necessária para executar uma comparação de cadeias de caracteres que não diferencia maiúsculas de minúsculas:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample5.cs)]

Em seguida, adicionaremos um atributo [Authorize] aos métodos de ação Edit () em nossa classe DinnersController. Isso garantirá que os usuários devem estar conectados para solicitar uma URL de */Dinners/Edit/[ID]* .

Em seguida, podemos adicionar código aos nossos métodos de edição que usam o método auxiliar jantar. IsHostedBy (username) para verificar se o usuário conectado corresponde ao host do jantar. Se o usuário não for o host, vamos exibir uma exibição "InvalidOwner" e encerrar a solicitação. O código para fazer isso se parece com o seguinte:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample6.cs)]

Em seguida, podemos clicar com o botão direito do mouse no diretório \Views\Dinners e escolher o comando de menu Add-&gt;View para criar uma nova exibição "InvalidOwner". Vamos preenchê-lo com a mensagem de erro abaixo:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample7.aspx)]

E agora, quando um usuário tenta editar um jantar que não é de sua propriedade, ele receberá uma mensagem de erro:

![](secure-applications-using-authentication-and-authorization/_static/image7.png)

Podemos repetir as mesmas etapas para os métodos de ação DELETE () em nosso controlador para bloquear a permissão para excluir os jantares também e garantir que apenas o host de um jantar possa excluí-lo.

### <a name="showinghiding-edit-and-delete-links"></a>Mostrando/ocultando links de edição e exclusão

Estamos vinculando ao método de ação editar e excluir da nossa classe DinnersController de nossa URL de detalhes:

![](secure-applications-using-authentication-and-authorization/_static/image8.png)

No momento, estamos mostrando os links de ação editar e excluir, independentemente de o visitante da URL de detalhes ser o host do jantar. Vamos alterar isso para que os links sejam exibidos apenas se o usuário visitante for o proprietário do jantar.

O método de ação Details () em nosso DinnersController recupera um objeto de jantar e, em seguida, o passa como o objeto de modelo para nosso modelo de exibição:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample8.cs)]

Podemos atualizar nosso modelo de exibição para mostrar/ocultar condicionalmente os links editar e excluir usando o método auxiliar jantar. IsHostedBy (), como abaixo:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample9.aspx)]

#### <a name="next-steps"></a>Próximas etapas

Agora, vamos ver como podemos habilitar usuários autenticados para RSVP para jantares usando AJAX.

> [!div class="step-by-step"]
> [Anterior](implement-efficient-data-paging.md)
> [Próximo](use-ajax-to-deliver-dynamic-updates.md)
