---
uid: web-forms/overview/older-versions-security/roles/role-based-authorization-vb
title: Autorização baseada em função (VB) | Microsoft Docs
author: rick-anderson
description: Este tutorial começa com uma visão de como a estrutura de funções associa as funções de um usuário com seu contexto de segurança. Em seguida, ele examina como aplicar a URL baseada em função...
ms.author: riande
ms.date: 03/24/2008
ms.assetid: 83b4f5a4-4f5a-4380-ba33-f0b5c5ac6a75
msc.legacyurl: /web-forms/overview/older-versions-security/roles/role-based-authorization-vb
msc.type: authoredcontent
ms.openlocfilehash: feb3e5eb992284033853e67bfab3872243cefe39
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78573153"
---
# <a name="role-based-authorization-vb"></a>Autorização baseada em função (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/VB.11.zip) ou [baixar PDF](https://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial11_RoleAuth_vb.pdf)

> Este tutorial começa com uma visão de como a estrutura de funções associa as funções de um usuário com seu contexto de segurança. Em seguida, ele examina como aplicar regras de autorização de URL baseadas em função. Depois disso, veremos o uso de meios declarativos e programáticos para alterar os dados exibidos e a funcionalidade oferecida por uma página do ASP.NET.

## <a name="introduction"></a>Introdução

No tutorial <a id="_msoanchor_1"> </a>de [*autorização baseada no usuário*](../membership/user-based-authorization-vb.md) , vimos como usar a autorização de URL para especificar o que os usuários podiam visitar em um determinado conjunto de páginas. Com apenas um pouco de marcação na `Web.config`, poderíamos instruir o ASP.NET a permitir que apenas usuários autenticados visitem uma página. Ou poderíamos ditar que somente os usuários Tito e Bob eram permitidos, ou indicam que todos os usuários autenticados, exceto o Sam, eram permitidos.

Além da autorização de URL, também examinamos técnicas declarativas e Programáticas para controlar os dados exibidos e a funcionalidade oferecida por uma página com base no usuário que está visitando. Em particular, criamos uma página que listou o conteúdo do diretório atual. Qualquer pessoa pode visitar esta página, mas somente os usuários autenticados poderão exibir o conteúdo dos arquivos e somente Tito poderá excluir os arquivos.

A aplicação de regras de autorização a uma base de usuário por usuário pode crescer em um pesadelo de escrituração. Uma abordagem mais passível de manutenção é usar a autorização baseada em função. A boa notícia é que as ferramentas de nossa disposição para aplicar as regras de autorização funcionam igualmente bem com as funções que são para contas de usuário. As regras de autorização de URL podem especificar funções em vez de usuários. O controle LoginView, que renderiza uma saída diferente para usuários autenticados e anônimos, pode ser configurado para exibir conteúdo diferente com base nas funções do usuário conectado. E a API de funções inclui métodos para determinar as funções do usuário conectado.

Este tutorial começa com uma visão de como a estrutura de funções associa as funções de um usuário com seu contexto de segurança. Em seguida, ele examina como aplicar regras de autorização de URL baseadas em função. Depois disso, veremos o uso de meios declarativos e programáticos para alterar os dados exibidos e a funcionalidade oferecida por uma página do ASP.NET. Vamos começar!

## <a name="understanding-how-roles-are-associated-with-a-users-security-context"></a>Compreendendo como as funções são associadas ao contexto de segurança de um usuário

Sempre que uma solicitação entra no pipeline ASP.NET, ela é associada a um contexto de segurança, que inclui informações que identificam o solicitante. Ao usar a autenticação de formulários, um tíquete de autenticação é usado como um token de identidade. Como discutimos em <a id="_msoanchor_2"> </a> [*uma visão geral dos tutoriais sobre autenticação de formulários*](../introduction/an-overview-of-forms-authentication-vb.md) e <a id="_msoanchor_3"> </a> [*configuração de autenticação de formulários e tópicos avançados*](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md) , a `FormsAuthenticationModule` é responsável por determinar a identidade do solicitante, que é durante o [evento`AuthenticateRequest`](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx).

Se um tíquete de autenticação válido e não expirado for encontrado, o `FormsAuthenticationModule` o decodificará para verificar a identidade do solicitante. Ele cria um novo objeto `GenericPrincipal` e o atribui ao objeto `HttpContext.User`. A finalidade de uma entidade de segurança, como `GenericPrincipal`, é identificar o nome do usuário autenticado e a quais funções ela pertence. Essa finalidade é evidente pelo fato de que todos os objetos principais têm uma propriedade `Identity` e um método `IsInRole(roleName)`. No entanto, o `FormsAuthenticationModule`não está interessado em registrar informações de função e o `GenericPrincipal` objeto que ele cria não especifica nenhuma função.

Se a estrutura de funções estiver habilitada, o [`RoleManagerModule`](https://msdn.microsoft.com/library/system.web.security.rolemanagermodule.aspx) módulo HTTP será etapas em após o `FormsAuthenticationModule` e identificará as funções do usuário autenticado durante o [evento`PostAuthenticateRequest`](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx), que é acionado após o evento `AuthenticateRequest`. Se a solicitação for de um usuário autenticado, a `RoleManagerModule` substituirá o objeto `GenericPrincipal` criado pelo `FormsAuthenticationModule` e o substituirá por um [objeto`RolePrincipal`](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx). A classe `RolePrincipal` usa a API de funções para determinar a quais funções o usuário pertence.

A Figura 1 descreve o fluxo de trabalho do pipeline do ASP.NET ao usar a autenticação de formulários e a estrutura de funções. O `FormsAuthenticationModule` é executado primeiro, identifica o usuário por meio de seu tíquete de autenticação e cria um novo objeto `GenericPrincipal`. Em seguida, o `RoleManagerModule` etapas em e substitui o objeto `GenericPrincipal` por um objeto `RolePrincipal`.

Se um usuário anônimo visitar o site, nem o `FormsAuthenticationModule` nem o `RoleManagerModule` criará um objeto principal.

[![os eventos de pipeline ASP.NET para um usuário autenticado ao usar a autenticação de formulários e a estrutura de funções](role-based-authorization-vb/_static/image2.png)](role-based-authorization-vb/_static/image1.png)

**Figura 1**: os eventos de pipeline ASP.net para um usuário autenticado ao usar a autenticação de formulários e a estrutura de funções ([clique para exibir a imagem em tamanho normal](role-based-authorization-vb/_static/image3.png))

### <a name="caching-role-information-in-a-cookie"></a>Armazenando em cache as informações de função em um cookie

O método `IsInRole(roleName)` do objeto de `RolePrincipal` chama `Roles`.`GetRolesForUser` para obter as funções para o usuário a fim de determinar se o usuário é membro de *roleName*. Ao usar o `SqlRoleProvider`, isso resulta em uma consulta para o banco de dados de repositório de funções. Ao usar as regras de autorização de URL baseada em função, o método `IsInRole` do `RolePrincipal`será chamado em cada solicitação para uma página protegida pelas regras de autorização de URL baseadas em função. Em vez de ter que pesquisar as informações de função no banco de dados em cada solicitação, o `Roles` Framework inclui uma opção para armazenar em cache as funções do usuário em um cookie.

Se a estrutura de funções estiver configurada para armazenar em cache as funções do usuário em um cookie, o `RoleManagerModule` criará o cookie durante o [evento`EndRequest`](https://msdn.microsoft.com/library/system.web.httpapplication.endrequest.aspx)do pipeline de ASP.net. Esse cookie é usado em solicitações subsequentes no `PostAuthenticateRequest`, que é quando o objeto `RolePrincipal` é criado. Se o cookie for válido e não tiver expirado, os dados no cookie serão analisados e usados para popular as funções do usuário, salvando assim a `RolePrincipal` de ter que fazer uma chamada para a classe `Roles` para determinar as funções do usuário. A Figura 2 descreve esse fluxo de trabalho.

[![as informações de função do usuário podem ser armazenadas em um cookie para melhorar o desempenho](role-based-authorization-vb/_static/image5.png)](role-based-authorization-vb/_static/image4.png)

**Figura 2**: as informações de função do usuário podem ser armazenadas em um cookie para melhorar o desempenho ([clique para exibir a imagem em tamanho normal](role-based-authorization-vb/_static/image6.png))

Por padrão, o mecanismo de cookie de cache de função está desabilitado. Ele pode ser habilitado por meio do `<roleManager>`; marcação de configuração no `Web.config`. Discutimos o uso do [elemento`<roleManager>`](https://msdn.microsoft.com/library/ms164660.aspx) para especificar provedores de função <a id="_msoanchor_4"> </a>no tutorial [*criando e gerenciando funções*](creating-and-managing-roles-vb.md) , de modo que você já deve ter esse elemento no arquivo de `Web.config` do seu aplicativo. As configurações de cookie de cache de função são especificadas como atributos do `<roleManager>`; e são resumidos na tabela 1.

> [!NOTE]
> As definições de configuração listadas na tabela 1 especificam as propriedades do cookie de cache de função resultante. Para obter mais informações sobre cookies, como eles funcionam e suas várias propriedades, leia [este tutorial de cookies](http://www.quirksmode.org/js/cookies.html).

| <strong>Propriedade</strong> |                                                                                                                                                                                                                                                                                                                                                         <strong>Descrição</strong>                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   `cacheRolesInCookie`    |                                                                                                                                                                                                                                                                                                                              Um valor booliano que indica se o cache de cookie é usado. Usa `false` como padrão.                                                                                                                                                                                                                                                                                                                              |
|       `cookieName`        |                                                                                                                                                                                                                                                                                                                                     O nome do cookie de cache de função. O valor padrão é ". ASPXROLES".                                                                                                                                                                                                                                                                                                                                     |
|       `cookiePath`        |                                                                                                                                                                                                                                O caminho para o cookie de nome de funções. O atributo Path permite que um desenvolvedor limite o escopo de um cookie a uma hierarquia de diretório específica. O valor padrão é "/", que informa ao navegador para enviar o cookie de tíquete de autenticação para qualquer solicitação feita ao domínio.                                                                                                                                                                                                                                 |
|    `cookieProtection`     |                                                                                                                                                               Indica quais técnicas são usadas para proteger o cookie de cache de função. Os valores permitidos são: `All` (o padrão); `Encryption`; `None`; e `Validation`. Consulte novamente a etapa 3 do <a id="_anchor_5"> </a>tutorial configuração de autenticação de [*formulários e tópicos avançados*](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md) para obter mais informações sobre esses níveis de proteção.                                                                                                                                                                |
|    `cookieRequireSSL`     |                                                                                                                                                                                                                                                                                                   Um valor booliano que indica se uma conexão SSL é necessária para transmitir o cookie de autenticação. O valor padrão é `false`.                                                                                                                                                                                                                                                                                                   |
| `cookieSlidingExpiration` |                                                                                                                                                                                                                                                  Um valor booliano que indica se o tempo limite do cookie é redefinido cada vez que o usuário visita o site durante uma única sessão. O valor padrão é `false`. Esse valor só é pertinente quando `createPersistentCookie` é definido como `true`.                                                                                                                                                                                                                                                  |
|      `cookieTimeout`      |                                                                                                                                                                                                                                                                         Especifica o tempo, em minutos, após o qual o cookie de tíquete de autenticação expira. O valor padrão é `30`. Esse valor só é pertinente quando `createPersistentCookie` é definido como `true`.                                                                                                                                                                                                                                                                         |
| `createPersistentCookie`  |                                                                                                                                                                   Um valor booliano que especifica se o cookie de cache de função é um cookie de sessão ou persistente. Se `false` (o padrão), será usado um cookie de sessão, que será excluído quando o navegador for fechado. Se `true`, um cookie persistente será usado; Ele expira `cookieTimeout` número de minutos depois que ele foi criado ou após a visita anterior, dependendo do valor de `cookieSlidingExpiration`.                                                                                                                                                                    |
|         `domain`          |                                                                                                                                                 Especifica o valor de domínio do cookie. O valor padrão é uma cadeia de caracteres vazia, o que faz com que o navegador use o domínio do qual foi emitido (como www.yourdomain.com). Nesse caso, o cookie <strong>não</strong> será enviado ao fazer solicitações para subdomínios, como admin.yourdomain.com. Se você quiser que o cookie seja passado para todos os subdomínios, será necessário personalizar o atributo `domain`, definindo-o como "yourdomain.com".                                                                                                                                                 |
|    `maxCachedResults`     | Especifica o número máximo de nomes de função que são armazenados em cache no cookie. O padrão é 25. O `RoleManagerModule` não cria um cookie para usuários que pertencem a mais de `maxCachedResults` funções. Consequentemente, o método `IsInRole` do objeto de `RolePrincipal` usará a classe `Roles` para determinar as funções do usuário. O motivo `maxCachedResults` existe é porque muitos agentes de usuário não permitem cookies maiores que 4.096 bytes. Portanto, esse limite destina-se a reduzir a probabilidade de exceder essa limitação de tamanho. Se você tiver nomes de função extremamente longos, talvez queira considerar a especificação de um valor de `maxCachedResults` menor; contrariwise, se você tiver nomes de função extremamente curtos, provavelmente poderá aumentar esse valor. |

**Tabela 1**: as opções de configuração de cookie de cache de função

Vamos configurar nosso aplicativo para usar cookies de cache de função não persistente. Para fazer isso, atualize o elemento `<roleManager>` no `Web.config` para incluir os seguintes atributos relacionados ao cookie:

[!code-xml[Main](role-based-authorization-vb/samples/sample1.xml)]

Atualizei o `<roleManager>`; Adicionando três atributos: `cacheRolesInCookie`, `createPersistentCookie`e `cookieProtection`. Ao definir `cacheRolesInCookie` como `true`, o `RoleManagerModule` agora armazenará automaticamente as funções do usuário em um cookie em vez de ter que pesquisar as informações de função do usuário em cada solicitação. Defini explicitamente os atributos `createPersistentCookie` e `cookieProtection` como `false` e `All`, respectivamente. Tecnicamente, não precisei especificar valores para esses atributos, pois acabei de atribuí-los a seus valores padrão, mas os coloco para torná-lo explicitamente claro que não estou usando cookies persistentes e que o cookie é criptografado e validado.

Isso é tudo! Daqui em diante, a estrutura de funções armazenará em cache as funções dos usuários em cookies. Se o navegador do usuário não oferecer suporte a cookies, ou se seus cookies forem excluídos ou perdidos, de alguma forma, o objeto de `RolePrincipal` simplesmente usará a classe `Roles` no caso de nenhum cookie (ou de um inválido ou expirado) estar disponível.

> [!NOTE]
> O grupo de práticas &amp; de padrões da Microsoft desencoraja o uso de cookies de cache de função persistente. Como a posse do cookie de cache de função é suficiente para provar a associação de função, se um hacker puder de alguma forma obter acesso a um cookie de usuário válido, ele poderá representar esse usuário. A probabilidade de isso acontecer aumentará se o cookie persistir no navegador do usuário. Para obter mais informações sobre essa recomendação de segurança, bem como outras questões de segurança, consulte a [lista de perguntas de segurança para ASP.NET 2,0](https://msdn.microsoft.com/library/ms998375.aspx).

## <a name="step-1-defining-role-based-url-authorization-rules"></a>Etapa 1: definindo regras de autorização de URL baseadas em função

Conforme discutido no tutorial <a id="_msoanchor_6"> </a>de [*autorização baseada no usuário*](../membership/user-based-authorization-vb.md) , a autorização de URL oferece um meio de restringir o acesso a um conjunto de páginas por usuário ou função por função. As regras de autorização de URL são escritas em `Web.config` usando o [elemento`<authorization>`](https://msdn.microsoft.com/library/8d82143t.aspx) com os elementos filho `<allow>` e `<deny>`. Além das regras de autorização relacionadas ao usuário discutidas nos tutoriais anteriores, cada `<allow>` e `<deny>` elemento filho também podem incluir:

- Uma função específica
- Uma lista delimitada por vírgulas de funções

Por exemplo, as regras de autorização de URL concedem acesso a esses usuários nas funções de administradores e supervisores, mas negam acesso a todos os outros:

[!code-xml[Main](role-based-authorization-vb/samples/sample2.xml)]

O elemento `<allow>` na marcação acima declara que as funções Administrators e supervisores são permitidas; o `<deny>`; o elemento instrui que *todos* os usuários são negados.

Vamos configurar nosso aplicativo para que as páginas `ManageRoles.aspx`, `UsersAndRoles.aspx`e `CreateUserWizardWithRoles.aspx` sejam acessíveis somente a esses usuários na função Administradores, enquanto a página `RoleBasedAuthorization.aspx` permanece acessível a todos os visitantes.

Para fazer isso, comece adicionando um arquivo de `Web.config` à pasta `Roles`.

[![adicionar um arquivo Web. config ao diretório de funções](role-based-authorization-vb/_static/image8.png)](role-based-authorization-vb/_static/image7.png)

**Figura 3**: adicionar um arquivo de `Web.config` ao diretório `Roles` ([clique para exibir a imagem em tamanho normal](role-based-authorization-vb/_static/image9.png))

Em seguida, adicione a seguinte marcação de configuração a `Web.config`:

[!code-xml[Main](role-based-authorization-vb/samples/sample3.xml)]

O elemento `<authorization>` na seção `<system.web>` indica que somente os usuários na função administradores podem acessar os recursos do ASP.NET no diretório `Roles`. O elemento `<location>` define um conjunto alternativo de regras de autorização de URL para a página `RoleBasedAuthorization.aspx`, permitindo que todos os usuários visitem a página.

Depois de salvar as alterações no `Web.config`, faça logon como um usuário que não esteja na função Administradores e tente visitar uma das páginas protegidas. O `UrlAuthorizationModule` detectará que você não tem permissão para visitar o recurso solicitado; Consequentemente, o `FormsAuthenticationModule` irá redirecioná-lo para a página de logon. Em seguida, a página de logon será redirecionada para a página `UnauthorizedAccess.aspx` (consulte a Figura 4). Esse redirecionamento final da página de logon para `UnauthorizedAccess.aspx` ocorre devido ao código que adicionamos à página de logon na etapa <a id="_msoanchor_7"> </a>2 do tutorial de [*autorização baseada no usuário*](../membership/user-based-authorization-vb.md) . Em particular, a página de logon redireciona automaticamente qualquer usuário autenticado para `UnauthorizedAccess.aspx` se a QueryString contiver um parâmetro `ReturnUrl`, pois esse parâmetro indica que o usuário chegou na página de logon depois de tentar exibir uma página que não estava autorizada a exibir.

[![somente os usuários na função administradores podem exibir as páginas protegidas](role-based-authorization-vb/_static/image11.png)](role-based-authorization-vb/_static/image10.png)

**Figura 4**: somente os usuários na função administradores podem exibir as páginas protegidas ([clique para exibir a imagem em tamanho normal](role-based-authorization-vb/_static/image12.png))

Faça logoff e, em seguida, faça logon como um usuário que está na função Administradores. Agora você deve ser capaz de exibir as três páginas protegidas.

[![Tito pode visitar a página UsersAndRoles. aspx porque ela está na função Administradores](role-based-authorization-vb/_static/image14.png)](role-based-authorization-vb/_static/image13.png)

**Figura 5**: o Tito pode visitar a página `UsersAndRoles.aspx` porque ela está na função Administradores ([clique para exibir a imagem em tamanho normal](role-based-authorization-vb/_static/image15.png))

> [!NOTE]
> Ao especificar regras de autorização de URL – para funções ou usuários – é importante ter em mente que as regras são analisadas uma de cada vez, de cima para baixo. Assim que uma correspondência for encontrada, o usuário terá o acesso concedido ou negado, dependendo de se a correspondência foi encontrada em um elemento `<allow>` ou `<deny>`. **Se nenhuma correspondência for encontrada, o usuário receberá acesso.** Consequentemente, se você quiser restringir o acesso a uma ou mais contas de usuário, é imperativo usar um elemento `<deny>` como o último elemento na configuração de autorização de URL. **Se suas regras de autorização de URL não incluírem um** **elemento`<deny>`, todos os usuários terão acesso.** Para obter uma discussão mais detalhada sobre como as regras de autorização de URL são analisadas, consulte a seção "Veja como a `UrlAuthorizationModule` usa as regras de autorização para conceder ou negar acesso" do <a id="_msoanchor_8"> </a>tutorial de [*autorização baseada no usuário*](../membership/user-based-authorization-vb.md) .

## <a name="step-2-limiting-functionality-based-on-the-currently-logged-in-users-roles"></a>Etapa 2: limitando a funcionalidade com base nas funções do usuário conectado no momento

A autorização de URL facilita a especificação de regras de autorização grosseira que determinam quais identidades são permitidas e quais delas são negadas de exibir uma determinada página (ou todas as páginas de uma pasta e suas subpastas). No entanto, em alguns casos, podemos permitir que todos os usuários visitem uma página, mas limitamos a funcionalidade da página com base nas funções do usuário de visita. Isso pode envolver a exibição ou ocultação de dados com base na função do usuário ou a oferta de funcionalidade adicional aos usuários que pertencem a uma função específica.

Essas regras refinadas de autorização baseada em função podem ser implementadas de forma declarativa ou programática (ou por meio de uma combinação das duas). Na próxima seção, veremos como implementar a autorização refinada declarativa por meio do controle LoginView. Depois disso, exploraremos técnicas programáticas. Antes de examinarmos a aplicação de regras de autorização refinadas, no entanto, primeiro precisamos criar uma página cuja funcionalidade depende da função do usuário que a está visitando.

Vamos criar uma página que lista todas as contas de usuário no sistema em um GridView. O GridView incluirá o nome de usuário, o endereço de email, a data do último logon e os comentários do usuário. Além de exibir as informações de cada usuário, o GridView incluirá os recursos de edição e exclusão. Inicialmente, criaremos essa página com a funcionalidade Editar e excluir disponível para todos os usuários. Nas seções "usando o controle LoginView" e "limitando programaticamente a funcionalidade", veremos como habilitar ou desabilitar esses recursos com base na função do usuário de visita.

> [!NOTE]
> A página ASP.NET que estamos prestes a Compilar usa um controle GridView para exibir as contas de usuário. Como essa série de tutoriais se concentra em autenticação de formulários, autorização, contas de usuário e funções, não quero gastar muito tempo discutindo o funcionamento interno do controle GridView. Embora este tutorial forneça instruções passo a passo específicas para configurar essa página, ele não se aprofunda nos detalhes de por que determinadas opções foram feitas ou quais propriedades específicas de efeito têm sobre a saída renderizada. Para um exame completo do controle GridView, Confira meu *[trabalho com dados na série de tutoriais do ASP.NET 2,0](../../data-access/index.md)* .

Comece abrindo a página de `RoleBasedAuthorization.aspx` na pasta `Roles`. Arraste um GridView da página para o designer e defina seu `ID` como `UserGrid`. Em alguns instantes, escreveremos o código que chama o `Membership`.`GetAllUsers` e associa o objeto `MembershipUserCollection` resultante ao GridView. O `MembershipUserCollection` contém um objeto `MembershipUser` para cada conta de usuário no sistema; `MembershipUser` objetos têm propriedades como `UserName`,`Email`,`LastLoginDate` e assim por diante.

Antes de escrevermos o código que associa as contas de usuário à grade, vamos definir primeiro os campos do GridView. Na marca inteligente do GridView, clique no link "Editar colunas" para iniciar a caixa de diálogo campos (consulte a Figura 6). A partir daqui, desmarque a caixa de seleção "gerar campos automaticamente" no canto inferior esquerdo. Como queremos que esse GridView inclua recursos de edição e exclusão, adicione um CommandField e defina suas propriedades `ShowEditButton` e `ShowDeleteButton` como true. Em seguida, adicione quatro campos para exibir as propriedades `UserName`, `Email`, `LastLoginDate`e `Comment`. Use um BoundField para as duas propriedades somente leitura (`UserName` e `LastLoginDate`) e TemplateFields para os dois campos editáveis (`Email` e `Comment`).

Faça com que a primeira BoundField exiba a propriedade `UserName`; Defina suas propriedades `HeaderText` e `DataField` como "UserName". Esse campo não será editável, portanto, defina sua propriedade `ReadOnly` como true. Configure o `LastLoginDate` BoundField definindo seu `HeaderText` como "último logon" e seu `DataField` como "LastLoginDate". Vamos Formatar a saída desse BoundField para que apenas a data seja exibida (em vez da data e hora). Para fazer isso, defina a propriedade `HtmlEncode` de BoundField como false e sua propriedade `DataFormatString` como "{0:d}". Defina também a propriedade `ReadOnly` como true.

Defina as propriedades de `HeaderText` dos dois TemplateFields como "email" e "comentário".

[![os campos de GridView podem ser configurados por meio da caixa de diálogo campos](role-based-authorization-vb/_static/image17.png)](role-based-authorization-vb/_static/image16.png)

**Figura 6**: os campos do GridView podem ser configurados por meio da caixa de diálogo campos ([clique para exibir a imagem em tamanho normal](role-based-authorization-vb/_static/image18.png))

Agora, precisamos definir o `ItemTemplate` e `EditItemTemplate` para os TemplateFields "email" e "comentário". Adicione um controle rótulo da Web a cada um dos `ItemTemplates` e vincule suas propriedades `Text` às propriedades `Email` e `Comment`, respectivamente.

Para o modelo de "email", adicione uma caixa de texto chamada `Email` ao seu `EditItemTemplate` e associe sua propriedade `Text` à propriedade `Email` usando a associação de dados bidirecional. Adicione um RequiredFieldValidator e um RegularExpressionValidator à `EditItemTemplate` para garantir que um visitante editando a propriedade email tenha inserido um endereço de email válido. Para o modelo do "comentário", adicione uma caixa de texto de várias linhas chamada `Comment` ao seu `EditItemTemplate`. Defina as propriedades `Columns` e `Rows` da caixa de texto como 40 e 4, respectivamente, e, em seguida, associe sua propriedade `Text` à propriedade `Comment` usando a associação de dados bidirecional.

Depois de configurar essas TemplateFields, sua marcação declarativa deve ser semelhante ao seguinte:

[!code-aspx[Main](role-based-authorization-vb/samples/sample4.aspx)]

Ao editar ou excluir uma conta de usuário, precisaremos saber o valor da propriedade `UserName` do usuário. Defina a propriedade `DataKeyNames` do GridView como "UserName" para que essas informações estejam disponíveis por meio da coleção de `DataKeys` do GridView.

Por fim, adicione um controle ValidationSummary à página e defina sua propriedade `ShowMessageBox` como true e sua propriedade `ShowSummary` como false. Com essas configurações, o ValidationSummary exibirá um alerta do lado do cliente se o usuário tentar editar uma conta de usuário com um endereço de email ausente ou inválido.

[!code-aspx[Main](role-based-authorization-vb/samples/sample5.aspx)]

Agora, concluímos a marcação declarativa desta página. Nossa próxima tarefa é associar o conjunto de contas de usuário ao GridView. Adicione um método chamado `BindUserGrid` à classe code-behind da página de `RoleBasedAuthorization.aspx` que associa o `MembershipUserCollection` retornado pelo `Membership.GetAllUsers` ao `UserGrid` GridView. Chame esse método a partir do manipulador de eventos `Page_Load` na primeira página de visita.

[!code-vb[Main](role-based-authorization-vb/samples/sample6.vb)]

Com esse código em vigor, visite a página por meio de um navegador. Como mostra a Figura 7, você verá um GridView listando informações sobre cada conta de usuário no sistema.

[![o GridView do usergrid lista informações sobre cada usuário no sistema](role-based-authorization-vb/_static/image20.png)](role-based-authorization-vb/_static/image19.png)

**Figura 7**: o `UserGrid` GridView lista informações sobre cada usuário no sistema ([clique para exibir a imagem em tamanho normal](role-based-authorization-vb/_static/image21.png))

> [!NOTE]
> O `UserGrid` GridView lista todos os usuários em uma interface não paginável. Essa interface de grade simples não é adequada para cenários em que há várias dúzias ou mais de usuários. Uma opção é configurar o GridView para habilitar a paginação. O método `Membership.GetAllUsers` tem duas sobrecargas: uma que não aceita parâmetros de entrada e retorna todos os usuários e um que usa valores inteiros para o índice de página e o tamanho da página e retorna apenas o subconjunto especificado dos usuários. A segunda sobrecarga pode ser usada para percorrer com mais eficiência os usuários, uma vez que ele retorna apenas o subconjunto preciso de contas de usuário em vez de *todos* eles. Se você tiver milhares de contas de usuário, talvez queira considerar uma interface baseada em filtro, uma que mostre apenas os usuários cujo nome de usuário começa com um caractere selecionado, por exemplo. O [método`Membership.FindUsersByName`](https://msdn.microsoft.com/library/system.web.security.membership.findusersbyname.aspx) é ideal para criar uma interface do usuário baseada em filtro. Vamos examinar a criação de uma interface desse tipo em um tutorial futuro.

O controle GridView oferece suporte interno para edição e exclusão quando o controle está associado a um controle de fonte de dados configurado corretamente, como SqlDataSource ou ObjectDataSource. O `UserGrid` GridView, no entanto, tem seus dados vinculados programaticamente; Portanto, devemos escrever código para executar essas duas tarefas. Em particular, precisaremos criar manipuladores de eventos para os eventos `RowEditing`do GridView, `RowCancelingEdit`, `RowUpdating`e `RowDeleting`, que são acionados quando um visitante clica nos botões editar, cancelar, atualizar ou excluir do GridView.

Comece criando os manipuladores de eventos para os eventos `RowEditing`, `RowCancelingEdit`e `RowUpdating` do GridView e, em seguida, adicione o seguinte código:

[!code-vb[Main](role-based-authorization-vb/samples/sample7.vb)]

Os manipuladores de eventos `RowEditing` e `RowCancelingEdit` simplesmente definem a propriedade de `EditIndex` do GridView e, em seguida, reassociam a lista de contas de usuário à grade. A coisa interessante acontece no manipulador de eventos `RowUpdating`. Esse manipulador de eventos começa garantindo que os dados são válidos e, em seguida, captura o valor `UserName` da conta de usuário editada da coleção `DataKeys`. As caixas de Text`Email` e `Comment` nos dois TemplateFields ' `EditItemTemplate` s são então referenciadas por programação. Suas propriedades `Text` contêm o endereço de email editado e o comentário.

Para atualizar uma conta de usuário por meio da API Membership, precisamos primeiro obter as informações do usuário, que fazemos por meio de uma chamada para `Membership.GetUser(userName)`. As propriedades `Email` e `Comment` do objeto retornado `MembershipUser` são então atualizadas com os valores inseridos nas duas caixas de caixa de entrada da interface de edição. Por fim, essas modificações são salvas com uma chamada para [`Membership.UpdateUser`](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx). O manipulador de eventos `RowUpdating` é concluído revertendo o GridView para sua interface de pré-edição.

Em seguida, crie o manipulador de eventos `RowDeleting` de exclusão e adicione o seguinte código:

[!code-vb[Main](role-based-authorization-vb/samples/sample8.vb)]

O manipulador de eventos acima começa com a captura do valor de `UserName` da coleção de `DataKeys` do GridView; Esse valor de `UserName` é passado para o [método`DeleteUser`](https://msdn.microsoft.com/library/system.web.security.membership.deleteuser.aspx)da classe de associação. O método `DeleteUser` exclui a conta de usuário do sistema, incluindo os dados de associação relacionados (como as funções às quais este usuário pertence). Depois de excluir o usuário, a `EditIndex` da grade é definida como-1 (caso o usuário tenha clicado em excluir enquanto outra linha estava no modo de edição) e o método `BindUserGrid` é chamado.

> [!NOTE]
> O botão excluir não requer nenhum tipo de confirmação do usuário antes de excluir a conta de usuário. Recomendo que você adicione alguma forma de confirmação do usuário para diminuir a chance de uma conta ser excluída acidentalmente. Uma das maneiras mais fáceis de confirmar uma ação é por meio de uma caixa de diálogo de confirmação do lado do cliente. Para obter mais informações sobre essa técnica, consulte [adicionando confirmação do lado do cliente ao excluir](https://asp.net/learn/data-access/tutorial-42-vb.aspx).

Verifique se essa página funciona conforme o esperado. Você deve ser capaz de editar o endereço de email e o comentário de qualquer usuário, bem como excluir qualquer conta de usuário. Como a página `RoleBasedAuthorization.aspx` é acessível a todos os usuários, qualquer usuário – mesmo visitantes anônimos – pode visitar esta página e editar e excluir contas de usuário! Vamos atualizar essa página para que somente os usuários nas funções de supervisores e administradores possam editar o endereço de email e o comentário de um usuário e apenas os administradores possam excluir uma conta de usuário.

A seção "usando o controle LoginView" examina o uso do controle LoginView para mostrar instruções específicas à função do usuário. Se uma pessoa na função Administradores visitar esta página, mostraremos instruções sobre como editar e excluir usuários. Se um usuário na função de supervisores chegar a essa página, mostraremos instruções sobre como editar usuários. E se o visitante for anônimo ou não estiver na função de supervisores ou administradores, exibiremos uma mensagem explicando que eles não podem editar ou excluir informações de conta de usuário. Na seção "limitando a funcionalidade de forma programática", escreveremos o código que mostra ou oculta os botões editar e excluir de forma programática com base na função do usuário.

### <a name="using-the-loginview-control"></a>Usando o controle LoginView

Como vimos nos tutoriais anteriores, o controle LoginView é útil para exibir interfaces diferentes para usuários autenticados e anônimos, mas o controle LoginView também pode ser usado para exibir diferentes marcações com base nas funções do usuário. Vamos usar um controle LoginView para exibir instruções diferentes com base na função do usuário de visita.

Comece adicionando um LoginView acima do `UserGrid` GridView. Como discutimos anteriormente, o controle LoginView tem dois modelos internos: `AnonymousTemplate` e `LoggedInTemplate`. Insira uma breve mensagem em ambos os modelos que informa ao usuário que eles não podem editar nem excluir informações do usuário.

[!code-aspx[Main](role-based-authorization-vb/samples/sample9.aspx)]

Além do `AnonymousTemplate` e `LoggedInTemplate`, o controle LoginView pode incluir *RoleGroups*, que são modelos específicos de função. Cada RoleGroup contém uma única propriedade, `Roles`, que especifica a quais funções o RoleGroup se aplica. A propriedade `Roles` pode ser definida como uma única função (como "administradores") ou em uma lista delimitada por vírgulas de funções (como "administradores, supervisores").

Para gerenciar o RoleGroups, clique no link "Editar RoleGroups" na marca inteligente do controle para abrir o editor de coleção RoleGroup. Adicione dois novos RoleGroups. Defina a propriedade `Roles` do primeiro RoleGroup como "administradores" e a segunda como "supervisores".

[![gerenciar os modelos específicos de função do LoginView por meio do editor de coleção RoleGroup](role-based-authorization-vb/_static/image23.png)](role-based-authorization-vb/_static/image22.png)

**Figura 8**: gerenciar os modelos específicos de função do LoginView por meio do editor de coleção RoleGroup ([clique para exibir a imagem em tamanho normal](role-based-authorization-vb/_static/image24.png))

Clique em OK para fechar o editor de coleção RoleGroup; Isso atualiza a marcação declarativa do LoginView para incluir uma seção `<RoleGroups>` com um elemento filho `<asp:RoleGroup>` para cada RoleGroup definido no editor de coleção RoleGroup. Além disso, a lista suspensa "views" na marca inteligente do LoginView, que inicialmente é listada apenas o `AnonymousTemplate` e o `LoggedInTemplate` – agora inclui também o RoleGroups adicionado.

Edite o RoleGroups para que os usuários na função de supervisores sejam exibidos instruções sobre como editar contas de usuário, enquanto os usuários na função Administradores são mostrados instruções para edição e exclusão. Depois de fazer essas alterações, a marcação declarativa de LoginView deve ser semelhante ao seguinte.

[!code-aspx[Main](role-based-authorization-vb/samples/sample10.aspx)]

Depois de fazer essas alterações, salve a página e, em seguida, visite-a por meio de um navegador. Primeiro, visite a página como um usuário anônimo. Você deve ser mostrado na mensagem "você não está conectado ao sistema. Portanto, você não pode editar ou excluir nenhuma informação do usuário. " Em seguida, faça logon como um usuário autenticado, mas um que não esteja na função supervisoras nem administradores. Desta vez, você verá a mensagem "você não é membro das funções supervisores ou administradores. Portanto, você não pode editar ou excluir nenhuma informação do usuário. "

Em seguida, faça logon como um usuário que seja membro da função de supervisores. Desta vez, você deverá ver a mensagem específica de função de supervisores (consulte a Figura 9). E se você fizer logon como um usuário na função Administradores, deverá ver a mensagem específica de função de administradores (consulte a Figura 10).

[![Bruce é mostrado na mensagem específica de função de supervisores](role-based-authorization-vb/_static/image26.png)](role-based-authorization-vb/_static/image25.png)

**Figura 9**: Bruce é mostrado na mensagem específica de função de supervisores ([clique para exibir a imagem em tamanho normal](role-based-authorization-vb/_static/image27.png))

[![Tito é mostrada a mensagem de função específica de administradores](role-based-authorization-vb/_static/image29.png)](role-based-authorization-vb/_static/image28.png)

**Figura 10**: o Tito é mostrado na mensagem específica de função de administradores ([clique para exibir a imagem em tamanho normal](role-based-authorization-vb/_static/image30.png))

Como as capturas de tela nas figuras 9 e 10 mostram, o LoginView só renderiza um modelo, mesmo que vários modelos se apliquem. Bruce e Tito são usuários conectados, mas o LoginView renderiza apenas o RoleGroup correspondente e não o `LoggedInTemplate`. Além disso, o Tito pertence às funções de administradores e supervisores, mas o controle LoginView renderiza o modelo específico de função de administradores em vez dos supervisores.

A Figura 11 ilustra o fluxo de trabalho usado pelo controle LoginView para determinar qual modelo renderizar. Observe que, se houver mais de um RoleGroup especificado, o modelo LoginView renderizará o *primeiro* RoleGroup que corresponde a. Em outras palavras, se tivéssemos colocado os supervisores RoleGroup como o primeiro RoleGroup e os administradores como o segundo, quando Tito visitou essa página, ele veria a mensagem de supervisores.

[![o fluxo de trabalho do controle LoginView para determinar qual modelo processar](role-based-authorization-vb/_static/image32.png)](role-based-authorization-vb/_static/image31.png)

**Figura 11**: o fluxo de trabalho do controle LoginView para determinar qual modelo processar ([clique para exibir a imagem em tamanho normal](role-based-authorization-vb/_static/image33.png))

### <a name="programmatically-limiting-functionality"></a>Limitando programaticamente a funcionalidade

Enquanto o controle LoginView exibe instruções diferentes com base na função do usuário que está visitando a página, os botões editar e cancelar permanecem visíveis para todos. Precisamos ocultar programaticamente os botões editar e excluir de visitantes anônimos e usuários que não estão na função supervisores nem administradores. Precisamos ocultar o botão excluir para todos que não sejam um administrador. Para fazer isso, escreveremos um pouco de código que referencia programaticamente os LinkButtons Edit e Delete de CommandField e define suas propriedades `Visible` como `False`, se necessário.

A maneira mais fácil de fazer referência programaticamente a controles em um CommandField é primeiro convertê-lo em um modelo. Para fazer isso, clique no link "Editar colunas" na marca inteligente do GridView, selecione o CommandField na lista de campos atuais e clique no link "converter este campo em um TemplateField". Isso transforma o CommandField em um TemplateField com um `ItemTemplate` e `EditItemTemplate`. O `ItemTemplate` contém os LinkButtons Edit e Delete, enquanto o `EditItemTemplate` abriga a atualização e cancela LinkButtons.

[![converter o CommandField em um TemplateField](role-based-authorization-vb/_static/image35.png)](role-based-authorization-vb/_static/image34.png)

**Figura 12**: converter o CommandField em um TemplateField ([clique para exibir a imagem em tamanho normal](role-based-authorization-vb/_static/image36.png))

Atualize os LinkButtons editar e excluir no `ItemTemplate`, definindo suas propriedades `ID` como valores de `EditButton` e `DeleteButton`, respectivamente.

[!code-aspx[Main](role-based-authorization-vb/samples/sample11.aspx)]

Sempre que os dados são associados ao GridView, o GridView enumera os registros em sua propriedade `DataSource` e gera um objeto `GridViewRow` correspondente. À medida que cada objeto de `GridViewRow` é criado, o evento de `RowCreated` é disparado. Para ocultar os botões editar e excluir para usuários não autorizados, precisamos criar um manipulador de eventos para esse evento e fazer referência programaticamente a editar e excluir LinkButton, definindo suas propriedades `Visible` de forma adequada.

Crie um manipulador de eventos no evento `RowCreated` e, em seguida, adicione o seguinte código:

[!code-vb[Main](role-based-authorization-vb/samples/sample12.vb)]

Tenha em mente que o evento `RowCreated` é acionado para *todas* as linhas GridView, incluindo o cabeçalho, o rodapé, a interface do pager e assim por diante. Só queremos fazer referência de forma programática para editar e excluir LinkButtons se estivermos lidando com uma linha de dados que não está no modo de edição (já que a linha no modo de edição tem os botões atualizar e cancelar em vez de editar e excluir). Essa verificação é tratada pela instrução `If`.

Se estivermos lidando com uma linha de dados que não está no modo de edição, os LinkButtons editar e excluir serão referenciados e suas propriedades `Visible` serão definidas com base nos valores Boolianos retornados pelo método `IsInRole(roleName)` do objeto `User`. O objeto `User` faz referência à entidade de segurança criada pelo `RoleManagerModule`; Consequentemente, o método `IsInRole(roleName)` usa a API de funções para determinar se o visitante atual pertence a *roleName*.

> [!NOTE]
> Poderíamos ter usado a classe Roles diretamente, substituindo a chamada para `User.IsInRole(roleName)` por uma chamada para o [método`Roles.IsUserInRole(roleName)`](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx). Decidi usar o método de `IsInRole(roleName)` do objeto principal neste exemplo porque ele é mais eficiente do que usar a API de funções diretamente. Anteriormente neste tutorial, configuramos o Gerenciador de funções para armazenar em cache as funções do usuário em um cookie. Esses dados de cookie armazenados em cache só são utilizados quando o método de `IsInRole(roleName)` da entidade de segurança é chamado; chamadas diretas para a API de funções sempre envolvem uma viagem para o armazenamento de função. Mesmo que as funções não sejam armazenadas em cache em um cookie, chamar o método de `IsInRole(roleName)` do objeto principal é geralmente mais eficiente porque, quando ele é chamado pela primeira vez durante uma solicitação, ele armazena os resultados em cache. A API de funções, por outro lado, não executa nenhum cache. Como o evento de `RowCreated` é disparado uma vez para cada linha no GridView, usar `User.IsInRole(roleName)` envolve apenas uma viagem ao repositório de funções, enquanto `Roles.IsUserInRole(roleName)` requer *n* viagens, em que *n* é o número de contas de usuário exibidas na grade.

A propriedade `Visible` do botão Editar é definida como `True` se o usuário que está visitando esta página estiver na função Administradores ou supervisores; caso contrário, ele será definido como `False`. A propriedade `Visible` do botão de exclusão é definida como `True` somente se o usuário estiver na função Administradores.

Testar esta página por meio de um navegador. Se você visitar a página como um visitante anônimo ou como um usuário que não seja um supervisor nem um administrador, o Comandofield estará vazio; Ele ainda existe, mas como um prata fino sem os botões editar ou excluir.

> [!NOTE]
> É possível ocultar o CommandField completamente quando um não supervisor e não administrador estiver visitando a página. Eu deixe isso como um exercício para o leitor.

[![os botões editar e excluir estão ocultos para não-supervisores e não-administradores](role-based-authorization-vb/_static/image38.png)](role-based-authorization-vb/_static/image37.png)

**Figura 13**: os botões editar e excluir estão ocultos para não-supervisores e não-administradores ([clique para exibir a imagem em tamanho normal](role-based-authorization-vb/_static/image39.png))

Se um usuário que pertence à função de supervisores (mas não à função de administradores) visitar, ele verá apenas o botão Editar.

[![enquanto o botão Editar está disponível para supervisores, o botão excluir está oculto](role-based-authorization-vb/_static/image41.png)](role-based-authorization-vb/_static/image40.png)

**Figura 14**: enquanto o botão Editar está disponível para supervisores, o botão excluir está oculto ([clique para exibir a imagem em tamanho normal](role-based-authorization-vb/_static/image42.png))

E se um administrador visitar, ela terá acesso aos botões editar e excluir.

[![os botões editar e excluir estão disponíveis somente para administradores](role-based-authorization-vb/_static/image44.png)](role-based-authorization-vb/_static/image43.png)

**Figura 15**: os botões editar e excluir estão disponíveis somente para administradores ([clique para exibir a imagem em tamanho normal](role-based-authorization-vb/_static/image45.png))

## <a name="step-3-applying-role-based-authorization-rules-to-classes-and-methods"></a>Etapa 3: aplicando regras de autorização baseadas em função a classes e métodos

Na etapa 2, limitamos os recursos de edição aos usuários nas funções de supervisores e administradores e os recursos de exclusão apenas aos administradores. Isso foi feito ocultando os elementos de interface do usuário associados para usuários não autorizados por meio de técnicas programáticas. Essas medidas não garantem que um usuário não autorizado não consiga executar uma ação privilegiada. Pode haver elementos de interface do usuário que são adicionados posteriormente ou que esquecemos de ocultar para usuários não autorizados. Ou um hacker pode descobrir alguma outra maneira de obter a página ASP.NET para executar o método desejado.

Uma maneira fácil de garantir que uma parte da funcionalidade específica não possa ser acessada por um usuário não autorizado é decorar essa classe ou método com o [atributo`PrincipalPermission`](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx). Quando o tempo de execução do .NET usa uma classe ou executa um de seus métodos, ele verifica se o contexto de segurança atual tem permissão. O atributo `PrincipalPermission` fornece um mecanismo pelo qual podemos definir essas regras.

Analisamos o uso do atributo `PrincipalPermission` de volta no <a id="_msoanchor_9"> </a>tutorial de [*autorização baseada no usuário*](../membership/user-based-authorization-vb.md) . Especificamente, vimos como decorar o `SelectedIndexChanged` do GridView e o manipulador de eventos de `RowDeleting` para que eles pudessem ser executados apenas por usuários autenticados e Tito, respectivamente. O atributo `PrincipalPermission` funciona muito bem com funções.

Vamos demonstrar como usar o atributo `PrincipalPermission` no `RowUpdating` do GridView e `RowDeleting` manipuladores de eventos para proibir a execução de usuários não autorizados. Tudo o que precisamos fazer é adicionar o atributo apropriado acima de cada definição de função:

[!code-vb[Main](role-based-authorization-vb/samples/sample13.vb)]

O atributo para o manipulador de eventos `RowUpdating` determina que somente os usuários nas funções Administradores ou supervisores podem executar o manipulador de eventos, em que como o atributo no manipulador de eventos `RowDeleting` limita a execução aos usuários na função Administradores.

> [!NOTE]
> O atributo `PrincipalPermission` é representado como uma classe no namespace `System.Security.Permissions`. Certifique-se de adicionar uma instrução `Imports System.Security.Permissions` na parte superior do arquivo de classe code-behind para importar esse namespace.

Se, de alguma forma, um não administrador tentar executar o manipulador de eventos `RowDeleting` ou se um não supervisor ou não administrador tentar executar o manipulador de eventos `RowUpdating`, o tempo de execução do .NET gerará uma `SecurityException`.

[![se o contexto de segurança não estiver autorizado a executar o método, uma SecurityException será gerada](role-based-authorization-vb/_static/image47.png)](role-based-authorization-vb/_static/image46.png)

**Figura 16**: se o contexto de segurança não estiver autorizado a executar o método, um `SecurityException` será gerado ([clique para exibir a imagem em tamanho normal](role-based-authorization-vb/_static/image48.png))

Além das páginas ASP.NET, muitos aplicativos também têm uma arquitetura que inclui várias camadas, como lógica de negócios e camadas de acesso a dados. Essas camadas normalmente são implementadas como bibliotecas de classes e oferecem classes e métodos para executar a lógica comercial e a funcionalidade relacionada a dados. O atributo `PrincipalPermission` também é útil para aplicar regras de autorização a essas camadas.

Para obter mais informações sobre como usar o atributo `PrincipalPermission` para definir regras de autorização em classes e métodos, consulte a entrada do blog de [Scott Guthrie](https://weblogs.asp.net/scottgu/) [adicionando regras de autorização a camadas de dados e de negócios usando `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>Resumo

Neste tutorial, examinamos como especificar regras de autorização de grande e refinamento com base nas funções do usuário. ASP. O recurso de autorização de URL do NET permite que um desenvolvedor de página Especifique quais identidades têm permissão ou acesso negado a quais páginas. Como vimos de volta no tutorial <a id="_msoanchor_10"> </a>de [*autorização baseada no usuário*](../membership/user-based-authorization-vb.md) , as regras de autorização de URL podem ser aplicadas de cada usuário. Eles também podem ser aplicados em uma base função por função, como vimos na etapa 1 deste tutorial.

Regras de autorização refinadas podem ser aplicadas declarativamente ou de forma programática. Na etapa 2, examinamos o uso do recurso RoleGroups do controle LoginView para processar uma saída diferente com base nas funções do usuário visitantes. Também vimos maneiras de determinar programaticamente se um usuário pertence a uma função específica e como ajustar a funcionalidade da página de forma adequada.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Adicionando regras de autorização a camadas de dados e de negócios usando `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [Examinando a associação, as funções e o perfil do ASP.NET 2.0: trabalhando com funções](http://aspnet.4guysfromrolla.com/articles/121405-1.aspx)
- [Lista de perguntas de segurança para ASP.NET 2,0](https://msdn.microsoft.com/library/ms998375.aspx)
- [Documentação técnica para o elemento `<roleManager>`](https://msdn.microsoft.com/library/ms164660.aspx)

### <a name="about-the-author"></a>Sobre o autor

Scott Mitchell, autor de vários livros sobre ASP/ASP. NET e fundador da 4GuysFromRolla.com, tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a *[Sams ensina a ASP.NET 2,0 em 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott pode ser contatado em [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) ou por meio de seu blog em [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a...

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores potenciais para este tutorial incluem Banerjee e Teresa Murphy. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](assigning-roles-to-users-vb.md)
