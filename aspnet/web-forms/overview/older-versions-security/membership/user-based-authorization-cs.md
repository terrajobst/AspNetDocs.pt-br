---
uid: web-forms/overview/older-versions-security/membership/user-based-authorization-cs
title: Autorização baseada em usuário (C#) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, veremos como limitar o acesso a páginas e restringir a funcionalidade em nível de página por meio de uma variedade de técnicas.
ms.author: riande
ms.date: 01/18/2008
ms.assetid: 3c815a9e-2296-4b9b-b945-776d54989daa
msc.legacyurl: /web-forms/overview/older-versions-security/membership/user-based-authorization-cs
msc.type: authoredcontent
ms.openlocfilehash: 059dbf42956268884dcfdade696491ac39e32da9
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74614312"
---
# <a name="user-based-authorization-c"></a>Autorização baseada em usuário (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_07_CS.zip) ou [baixar PDF](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial07_UserAuth_cs.pdf)

> Neste tutorial, veremos como limitar o acesso a páginas e restringir a funcionalidade em nível de página por meio de uma variedade de técnicas.

## <a name="introduction"></a>Introdução

A maioria dos aplicativos Web que oferecem contas de usuário faz isso em parte para impedir que determinados visitantes acessem determinadas páginas no site. Na maioria dos sites messageboard online, por exemplo, todos os usuários – anônimos e autenticados-são capazes de exibir as postagens do messageboard, mas somente os usuários autenticados podem visitar a página da Web para criar uma nova postagem. E pode haver páginas administrativas que só podem ser acessadas por um usuário específico (ou um determinado conjunto de usuários). Além disso, a funcionalidade de nível de página pode ser diferente de uma base de usuário por usuário. Ao exibir uma lista de postagens, os usuários autenticados são mostrados como uma interface para classificar cada postagem, enquanto essa interface não está disponível para visitantes anônimos.

O ASP.NET facilita a definição de regras de autorização baseadas no usuário. Com apenas um pouco de marcação em `Web.config`, páginas da Web específicas ou diretórios inteiros podem ser bloqueados para que fiquem acessíveis apenas a um subconjunto especificado de usuários. A funcionalidade de nível de página pode ser ativada ou desativada com base no usuário conectado no momento por meio de programação e meios declarativos.

Neste tutorial, veremos como limitar o acesso a páginas e restringir a funcionalidade em nível de página por meio de uma variedade de técnicas. Vamos começar!

## <a name="a-look-at-the-url-authorization-workflow"></a>Uma visão do fluxo de trabalho de autorização de URL

Conforme discutido em [*uma visão geral do tutorial de autenticação de formulários*](../introduction/an-overview-of-forms-authentication-cs.md) , quando o tempo de execução do ASP.net processa uma solicitação de um recurso ASP.net, a solicitação gera um número de eventos durante seu ciclo de vida. *Módulos http* são classes gerenciadas cujo código é executado em resposta a um evento específico no ciclo de vida da solicitação. O ASP.NET é fornecido com vários módulos HTTP que executam tarefas essenciais em segundo plano.

Um desses módulos HTTP é [`FormsAuthenticationModule`](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx). Conforme discutido nos tutoriais anteriores, a função principal do `FormsAuthenticationModule` é determinar a identidade da solicitação atual. Isso é feito inspecionando o tíquete de autenticação de formulários, que está localizado em um cookie ou inserido dentro da URL. Essa identificação ocorre durante o [evento de`AuthenticateRequest`](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx).

Outro módulo HTTP importante é o [`UrlAuthorizationModule`](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx), que é gerado em resposta ao [evento`AuthorizeRequest`](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx) (que acontece após o evento `AuthenticateRequest`). O `UrlAuthorizationModule` examina a marcação de configuração no `Web.config` para determinar se a identidade atual tem autoridade para visitar a página especificada. Esse processo é conhecido como *autorização de URL*.

Examinaremos a sintaxe das regras de autorização de URL na etapa 1, mas primeiro vamos ver o que a `UrlAuthorizationModule` faz, dependendo se a solicitação está autorizada ou não. Se a `UrlAuthorizationModule` determinar que a solicitação é autorizada, ela não fará nada e a solicitação continuará pelo ciclo de vida. No entanto, se a solicitação *não* for autorizada, o `UrlAuthorizationModule` abortará o ciclo de vida e instruirá o objeto `Response` a retornar um status de [http 401 não autorizado](http://www.checkupdown.com/status/E401.html) . Ao usar a autenticação de formulários, esse status HTTP 401 nunca é retornado ao cliente porque, se o `FormsAuthenticationModule` detectar um status HTTP 401, ele será modificado para um [redirecionamento http 302](http://www.checkupdown.com/status/E302.html) para a página de logon.

A Figura 1 ilustra o fluxo de trabalho do pipeline ASP.NET, o `FormsAuthenticationModule`e o `UrlAuthorizationModule` quando uma solicitação não autorizada chega. Em particular, a Figura 1 mostra uma solicitação por um visitante anônimo para `ProtectedPage.aspx`, que é uma página que nega acesso a usuários anônimos. Como o visitante é anônimo, o `UrlAuthorizationModule` aborta a solicitação e retorna um status de HTTP 401 não autorizado. Em seguida, o `FormsAuthenticationModule` converte o status 401 em um redirecionamento 302 para a página de logon. Depois que o usuário é autenticado por meio da página de logon, ele é redirecionado para `ProtectedPage.aspx`. Desta vez, o `FormsAuthenticationModule` identifica o usuário com base em seu tíquete de autenticação. Agora que o visitante está autenticado, o `UrlAuthorizationModule` permite o acesso à página.

[![o fluxo de trabalho de autenticação de formulários e autorização de URL](user-based-authorization-cs/_static/image2.png)](user-based-authorization-cs/_static/image1.png)

**Figura 1**: o fluxo de trabalho de autenticação de formulários e autorização de URL ([clique para exibir a imagem em tamanho normal](user-based-authorization-cs/_static/image3.png))

A Figura 1 descreve a interação que ocorre quando um visitante anônimo tenta acessar um recurso que não está disponível para usuários anônimos. Nesse caso, o visitante anônimo é redirecionado para a página de logon com a página que tentou visitar especificada na QueryString. Depois que o usuário tiver feito logon com êxito, ele será redirecionado automaticamente para o recurso que ele estava tentando exibir inicialmente.

Quando a solicitação não autorizada é feita por um usuário anônimo, esse fluxo de trabalho é simples e é fácil para o visitante entender o que aconteceu e por quê. Mas tenha em mente que o `FormsAuthenticationModule` redirecionará *qualquer* usuário não autorizado para a página de logon, mesmo que a solicitação seja feita por um usuário autenticado. Isso pode resultar em uma experiência de usuário confusa se um usuário autenticado tentar visitar uma página para a qual ela não tem autoridade.

Imagine que nosso site tinha suas regras de autorização de URL configuradas de modo que a página ASP.NET `OnlyTito.aspx` fosse accessibly apenas para Tito. Agora, imagine que o Sam visite o site, faça logon e tente visitar `OnlyTito.aspx`. O `UrlAuthorizationModule` interromperá o ciclo de vida da solicitação e retornará um status de HTTP 401 não autorizado, que o `FormsAuthenticationModule` detectará e, em seguida, redirecionará o Sam para a página de logon. No entanto, como o Sam já fez logon, ela pode estar imaginando por que ela foi enviada de volta para a página de logon. Ela pode parecer que suas credenciais de logon foram perdidas de alguma forma ou que ela inseriu credenciais inválidas. Se Samuel reinserir suas credenciais da página de logon, ela será conectada (novamente) e redirecionada para `OnlyTito.aspx`. O `UrlAuthorizationModule` detectará que o Sam não pode visitar esta página e ela será retornada para a página de logon.

A Figura 2 descreve esse fluxo de trabalho confuso.

[![o fluxo de trabalho padrão pode levar a um ciclo confuso](user-based-authorization-cs/_static/image5.png)](user-based-authorization-cs/_static/image4.png)

**Figura 2**: o fluxo de trabalho padrão pode levar a um ciclo confuso ([clique para exibir a imagem em tamanho normal](user-based-authorization-cs/_static/image6.png))

O fluxo de trabalho ilustrado na Figura 2 pode rapidamente Befuddle até o visitante mais experiente do computador. Veremos maneiras de evitar esse ciclo confuso na etapa 2.

> [!NOTE]
> O ASP.NET usa dois mecanismos para determinar se o usuário atual pode acessar uma página da Web específica: autorização de URL e autorização de arquivo. A autorização de arquivo é implementada pelo [`FileAuthorizationModule`](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx), que determina a autoridade consultando as ACLs de arquivo (s) solicitadas. A autorização de arquivo é usada com mais frequência com a autenticação do Windows porque as ACLs são permissões que se aplicam a contas do Windows. Ao usar a autenticação de formulários, todas as solicitações em nível de sistema operacional e de sistema de arquivos são executadas pela mesma conta do Windows, independentemente do usuário que está visitando o site. Como esta série de tutoriais se concentra na autenticação de formulários, não discutiremos a autorização de arquivos.

### <a name="the-scope-of-url-authorization"></a>O escopo da autorização de URL

O `UrlAuthorizationModule` é um código gerenciado que faz parte do tempo de execução do ASP.NET. Antes da versão 7 do servidor Web [serviços de informações da Internet (IIS)](https://www.iis.net/) da Microsoft, havia uma barreira distinta entre o pipeline http do IIS e o pipeline do tempo de execução do ASP.net. Em suma, no IIS 6 e versões anteriores, o ASP. A `UrlAuthorizationModule` da rede só é executada quando uma solicitação é delegada do IIS para o tempo de execução do ASP.NET. Por padrão, o IIS processa o conteúdo estático em si, como páginas HTML e CSS, JavaScript e arquivos de imagem, e apenas encaminha solicitações para o tempo de execução ASP.NET quando uma página com uma extensão de `.aspx`, `.asmx`ou `.ashx` é solicitada.

O IIS 7, no entanto, permite pipelines integrados do IIS e do ASP.NET. Com algumas definições de configuração, você pode configurar o IIS 7 para invocar o `UrlAuthorizationModule` para *todas* as solicitações, o que significa que as regras de autorização de URL podem ser definidas para arquivos de qualquer tipo. Além disso, o IIS 7 inclui seu próprio mecanismo de autorização de URL. Para obter mais informações sobre a integração do ASP.NET e a funcionalidade de autorização de URL nativa do IIS 7, consulte [noções básicas sobre autorização de URL do IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization). Para obter uma visão mais detalhada da integração do ASP.NET e do IIS 7, pegue uma cópia do livro Shahram Khosravi, *Professional IIS 7 e ASP.net Integrated Programming* (ISBN: 978-0470152539).

Resumindo, em versões anteriores ao IIS 7, as regras de autorização de URL são aplicadas somente aos recursos manipulados pelo tempo de execução do ASP.NET. Mas com o IIS 7, é possível usar o recurso de autorização de URL nativa do IIS ou para integrar o ASP. A `UrlAuthorizationModule` da rede no pipeline HTTP do IIS, estendendo assim essa funcionalidade para todas as solicitações.

> [!NOTE]
> Há algumas diferenças sutis, mas importantes em como o ASP. O `UrlAuthorizationModule` da rede e o recurso de autorização de URL do IIS 7 processam as regras de autorização. Este tutorial não examina a funcionalidade de autorização de URL do IIS 7 ou as diferenças em como ele analisa as regras de autorização em comparação com o `UrlAuthorizationModule`. Para obter mais informações sobre esses tópicos, consulte a documentação do IIS 7 no MSDN ou em [www.IIS.net](https://www.iis.net/).

## <a name="step-1-defining-url-authorization-rules-inwebconfig"></a>Etapa 1: definindo regras de autorização de URL no`Web.config`

O `UrlAuthorizationModule` determina se deve-se conceder ou negar acesso a um recurso solicitado para uma identidade específica com base nas regras de autorização de URL definidas na configuração do aplicativo. As regras de autorização são escritas no [elemento`<authorization>`](https://msdn.microsoft.com/library/8d82143t.aspx) na forma de `<allow>` e `<deny>` elementos filho. Cada `<allow>` e `<deny>` elemento filho podem especificar:

- Um usuário específico
- Uma lista delimitada por vírgulas de usuários
- Todos os usuários anônimos, indicados por um ponto de interrogação (?)
- Todos os usuários, indicados por um asterisco (\*)

A marcação a seguir ilustra como usar as regras de autorização de URL para permitir que os usuários Tito e Scott e neguem todos os outros:

[!code-xml[Main](user-based-authorization-cs/samples/sample1.xml)]

O elemento `<allow>` define quais usuários são permitidos-Tito e Scott-enquanto o elemento `<deny>` instrui que *todos* os usuários são negados.

> [!NOTE]
> Os elementos `<allow>` e `<deny>` também podem especificar regras de autorização para funções. Examinaremos a autorização baseada em função em um tutorial futuro.

A configuração a seguir concede acesso a qualquer pessoa que não seja Sam (incluindo visitantes anônimos):

[!code-xml[Main](user-based-authorization-cs/samples/sample2.xml)]

Para permitir somente usuários autenticados, use a configuração a seguir, que nega acesso a todos os usuários anônimos:

[!code-xml[Main](user-based-authorization-cs/samples/sample3.xml)]

As regras de autorização são definidas dentro do elemento `<system.web>` no `Web.config` e se aplicam a todos os recursos do ASP.NET no aplicativo Web. Muitas vezes, um aplicativo tem regras de autorização diferentes para seções diferentes. Por exemplo, em um site de comércio eletrônico, todos os visitantes podem examinar os produtos, ver as revisões de produtos, pesquisar o catálogo e assim por diante. No entanto, somente usuários autenticados podem acessar o check-out ou as páginas para gerenciar o histórico de remessa de um. Além disso, pode haver partes do site que só podem ser acessadas por usuários selecionados, como administradores de site.

O ASP.NET facilita a definição de diferentes regras de autorização para arquivos e pastas diferentes no site. As regras de autorização especificadas no arquivo de `Web.config` da pasta raiz se aplicam a todos os recursos do ASP.NET no site. No entanto, essas configurações de autorização padrão podem ser substituídas para uma determinada pasta adicionando um `Web.config` com uma seção `<authorization>`.

Vamos atualizar nosso site para que somente usuários autenticados possam visitar as páginas ASP.NET na pasta `Membership`. Para fazer isso, precisamos adicionar um arquivo de `Web.config` à pasta `Membership` e definir suas configurações de autorização para negar usuários anônimos. Clique com o botão direito do mouse na pasta `Membership` na Gerenciador de Soluções, escolha o menu Adicionar novo item no menu de contexto e adicione um novo arquivo de configuração da Web chamado `Web.config`.

[![adicionar um arquivo Web. config à pasta de associação](user-based-authorization-cs/_static/image8.png)](user-based-authorization-cs/_static/image7.png)

**Figura 3**: adicionar um arquivo de `Web.config` à pasta `Membership` ([clique para exibir a imagem em tamanho normal](user-based-authorization-cs/_static/image9.png))

Neste ponto, seu projeto deve conter dois arquivos de `Web.config`: um no diretório raiz e outro na pasta `Membership`.

[![seu aplicativo agora deve conter dois arquivos Web. config](user-based-authorization-cs/_static/image11.png)](user-based-authorization-cs/_static/image10.png)

**Figura 4**: seu aplicativo agora deve conter dois arquivos de `Web.config` ([clique para exibir a imagem em tamanho normal](user-based-authorization-cs/_static/image12.png))

Atualize o arquivo de configuração na pasta `Membership` para que ele proíba o acesso a usuários anônimos.

[!code-xml[Main](user-based-authorization-cs/samples/sample4.xml)]

E isso é tudo!

Para testar essa alteração, visite a Home Page em um navegador e verifique se você está desconectado. Como o comportamento padrão de um aplicativo ASP.NET é permitir todos os visitantes, e como não fazemos nenhuma modificação de autorização no arquivo de `Web.config` do diretório raiz, podemos visitar os arquivos no diretório raiz como um visitante anônimo.

Clique no link criando contas de usuário localizado na coluna esquerda. Isso o levará para a `~/Membership/CreatingUserAccounts.aspx`. Como o arquivo de `Web.config` na pasta `Membership` define as regras de autorização para proibir o acesso anônimo, o `UrlAuthorizationModule` anula a solicitação e retorna um status de HTTP 401 não autorizado. O `FormsAuthenticationModule` modifica isso para um status de redirecionamento 302, enviando-nos para a página de logon. Observe que a página que tentamos acessar (`CreatingUserAccounts.aspx`) é passada para a página de logon por meio do parâmetro `ReturnUrl` QueryString.

[![uma vez que as regras de autorização de URL proíbem o acesso anônimo, somos redirecionados para a página de logon](user-based-authorization-cs/_static/image14.png)](user-based-authorization-cs/_static/image13.png)

**Figura 5**: como as regras de autorização de URL proíbem o acesso anônimo, somos redirecionados para a página de logon ([clique para exibir a imagem em tamanho normal](user-based-authorization-cs/_static/image15.png))

Após fazer logon com êxito, somos redirecionados para a página de `CreatingUserAccounts.aspx`. Desta vez, o `UrlAuthorizationModule` permite o acesso à página porque não somos mais anônimos.

### <a name="applying-url-authorization-rules-to-a-specific-location"></a>Aplicando regras de autorização de URL a um local específico

As configurações de autorização definidas na seção `<system.web>` de `Web.config` se aplicam a todos os recursos ASP.NET nesse diretório e a seus subdiretórios (até o contrário, substituído por outro arquivo `Web.config`). No entanto, em alguns casos, podemos querer que todos os recursos ASP.NET em um determinado diretório tenham uma configuração de autorização específica, exceto uma ou duas páginas específicas. Isso pode ser feito adicionando um elemento `<location>` no `Web.config`, apontando-o para o arquivo cujas regras de autorização diferem e definindo suas regras exclusivas de autorização aqui.

Para ilustrar o uso do elemento `<location>` para substituir as definições de configuração de um recurso específico, vamos personalizar as configurações de autorização para que apenas Tito possa visitar `CreatingUserAccounts.aspx`. Para fazer isso, adicione um elemento `<location>` ao arquivo de `Web.config` da pasta `Membership` e atualize sua marcação para que fique semelhante ao seguinte:

[!code-xml[Main](user-based-authorization-cs/samples/sample5.xml)]

O elemento `<authorization>` no `<system.web>` define as regras de autorização de URL padrão para recursos ASP.NET na pasta `Membership` e suas subpastas. O elemento `<location>` nos permite substituir essas regras para um recurso específico. Na marcação acima, o elemento `<location>` referencia a página `CreatingUserAccounts.aspx` e especifica suas regras de autorização, como para permitir Tito, mas negar todos os outros.

Para testar essa alteração de autorização, comece visitando o site como um usuário anônimo. Se você tentar visitar qualquer página na pasta `Membership`, como `UserBasedAuthorization.aspx`, a `UrlAuthorizationModule` negará a solicitação e você será redirecionado para a página de logon. Depois de fazer logon como, digamos, Scott, você pode visitar qualquer página na pasta `Membership` *, com exceção* de `CreatingUserAccounts.aspx`. Tentar visitar `CreatingUserAccounts.aspx` conectado como qualquer pessoa, mas Tito resultará em uma tentativa de acesso não autorizado, redirecionando você de volta para a página de logon.

> [!NOTE]
> O elemento `<location>` deve aparecer fora do elemento `<system.web>` da configuração. Você precisa usar um elemento `<location>` separado para cada recurso cujas configurações de autorização você deseja substituir.

### <a name="a-look-at-how-theurlauthorizationmoduleuses-the-authorization-rules-to-grant-or-deny-access"></a>Uma olhada em como o`UrlAuthorizationModule`usa as regras de autorização para conceder ou negar acesso

O `UrlAuthorizationModule` determina se deve autorizar uma identidade específica para uma URL específica analisando as regras de autorização de URL, uma de cada vez, começando da primeira e trabalhando de maneira inativa. Assim que uma correspondência for encontrada, o usuário terá o acesso concedido ou negado, dependendo de se a correspondência foi encontrada em um elemento `<allow>` ou `<deny>`. <strong>Se nenhuma correspondência for encontrada, o usuário receberá acesso.</strong> Consequentemente, se você quiser restringir o acesso, é imperativo usar um elemento `<deny>` como o último elemento na configuração de autorização de URL. <strong>Se você omitir um</strong> elemento<strong>`<deny>`</strong> <strong>, todos os usuários terão o acesso concedido.</strong>

Para entender melhor o processo usado pelo `UrlAuthorizationModule` para determinar a autoridade, considere o exemplo de regras de autorização de URL que vimos anteriormente nesta etapa. A primeira regra é um elemento `<allow>` que permite o acesso a Tito e Scott. A segunda regra é um elemento `<deny>` que nega acesso a todos. Se um usuário anônimo visitar, o `UrlAuthorizationModule` começará perguntando, é anônimo ou Scott ou Tito? A resposta, obviamente, é não, então ela prossegue para a segunda regra. É anônimo no conjunto de todos? Como a resposta aqui é sim, a regra de `<deny>` é colocada em vigor e o visitante é redirecionado para a página de logon. Da mesma forma, se Jisun estiver visitando, o `UrlAuthorizationModule` começará perguntando, será Jisun Scott ou Tito? Como ela não é, a `UrlAuthorizationModule` prossegue para a segunda pergunta, é Jisun no conjunto de todas as pessoas? Ela também é um acesso negado. Por fim, se Tito visitas, a primeira pergunta feita pela `UrlAuthorizationModule` é uma resposta afirmativo, portanto, Tito recebe acesso.

Como o `UrlAuthorizationModule` processa as regras de autorização de cima para baixo, parando a qualquer correspondência, é importante que as regras mais específicas venham antes das menos específicas. Ou seja, para definir regras de autorização que proíbam usuários Jisun e anônimos, mas permitir todos os outros usuários autenticados, você começaria com a regra mais específica-a que afetou o Jisun-e, em seguida, prossegue para as regras menos específicas, aquelas que permitem todas as outras usuários autenticados, mas negando todos os usuários anônimos. As regras de autorização de URL a seguir implementam essa política, primeiro negando Jisun e, em seguida, negando qualquer usuário anônimo. Qualquer usuário autenticado que não seja Jisun receberá acesso porque nenhuma dessas `<deny>` instruções serão correspondentes.

[!code-xml[Main](user-based-authorization-cs/samples/sample6.xml)]

## <a name="step-2-fixing-the-workflow-for-unauthorized-authenticated-users"></a>Etapa 2: corrigindo o fluxo de trabalho para usuários autenticados e não autorizados

Como discutimos anteriormente neste tutorial, em uma olhada na seção fluxo de trabalho de autorização de URL, a qualquer momento que uma solicitação não autorizada ocorre, o `UrlAuthorizationModule` aborta a solicitação e retorna um status de HTTP 401 não autorizado. Esse status 401 é modificado pelo `FormsAuthenticationModule` em um status de redirecionamento 302 que envia o usuário para a página de logon. Esse fluxo de trabalho ocorre em qualquer solicitação não autorizada, mesmo que o usuário seja autenticado.

É provável que o retorno de um usuário autenticado na página de logon seja confundido, pois eles já fizeram logon no sistema. Com um pouco de trabalho, podemos melhorar esse fluxo Redirecionando usuários autenticados que fazem solicitações não autorizadas a uma página que explica que eles tentaram acessar uma página restrita.

Comece criando uma nova página ASP.NET na pasta raiz do aplicativo Web chamada `UnauthorizedAccess.aspx`; Não se esqueça de associar esta página à página mestra de `Site.master`. Depois de criar essa página, remova o controle de conteúdo que faz referência ao `LoginContent` ContentPlaceHolder para que o conteúdo padrão da página mestra seja exibido. Em seguida, adicione uma mensagem que explica a situação, ou seja, que o usuário tentou acessar um recurso protegido. Depois de adicionar essa mensagem, a marcação declarativa da página `UnauthorizedAccess.aspx` deve ser semelhante ao seguinte:

[!code-aspx[Main](user-based-authorization-cs/samples/sample7.aspx)]

Agora, precisamos alterar o fluxo de trabalho para que, se uma solicitação não autorizada for executada por um usuário autenticado, eles sejam enviados para a página de `UnauthorizedAccess.aspx` em vez da página de logon. A lógica que redireciona as solicitações não autorizadas para a página de logon é incluída em um método particular da classe `FormsAuthenticationModule`, portanto, não podemos personalizar esse comportamento. No entanto, o que podemos fazer é adicionar nossa própria lógica à página de logon que redireciona o usuário para `UnauthorizedAccess.aspx`, se necessário.

Quando o `FormsAuthenticationModule` redireciona um visitante não autorizado para a página de logon, ele acrescenta a URL solicitada e não autorizada à QueryString com o nome `ReturnUrl`. Por exemplo, se um usuário não autorizado tentar visitar `OnlyTito.aspx`, o `FormsAuthenticationModule` o redirecionaria para `Login.aspx?ReturnUrl=OnlyTito.aspx`. Portanto, se a página de logon for alcançada por um usuário autenticado com uma QueryString que inclui o parâmetro `ReturnUrl`, sabemos que esse usuário não autenticado tentou visitar uma página que não está autorizada a exibir. Nesse caso, queremos redirecioná-la para `UnauthorizedAccess.aspx`.

Para fazer isso, adicione o seguinte código ao manipulador de eventos `Page_Load` da página de logon:

[!code-csharp[Main](user-based-authorization-cs/samples/sample8.cs)]

O código acima redireciona os usuários autenticados e não autorizados para a página de `UnauthorizedAccess.aspx`. Para ver essa lógica em ação, visite o site como um visitante anônimo e clique no link criando contas de usuário na coluna à esquerda. Isso levará você à página `~/Membership/CreatingUserAccounts.aspx`, que, na etapa 1, configuramos para permitir apenas o acesso ao Tito. Como os usuários anônimos são proibidos, o `FormsAuthenticationModule` redireciona-nos de volta para a página de logon.

Neste ponto, somos anônimos, portanto `Request.IsAuthenticated` retorna `false` e não é redirecionado para `UnauthorizedAccess.aspx`. Em vez disso, a página de logon é exibida. Faça logon como um usuário diferente de Tito, como Bruce. Depois de inserir as credenciais apropriadas, a página de logon redireciona-nos de volta para `~/Membership/CreatingUserAccounts.aspx`. No entanto, como essa página só está acessível a Tito, não temos autorização para exibi-la e é retornada imediatamente para a página de logon. Dessa vez, no entanto, `Request.IsAuthenticated` retorna `true` (e o parâmetro `ReturnUrl` QueryString existe), portanto, é redirecionado para a página `UnauthorizedAccess.aspx`.

[![usuários autenticados, não autorizados são redirecionados para UnauthorizedAccess. aspx](user-based-authorization-cs/_static/image17.png)](user-based-authorization-cs/_static/image16.png)

**Figura 6**: usuários autenticados e não autorizados são redirecionados para `UnauthorizedAccess.aspx` ([clique para exibir a imagem em tamanho normal](user-based-authorization-cs/_static/image18.png))

Esse fluxo de trabalho personalizado apresenta uma experiência de usuário mais sensata e direta por meio do circuito curto do ciclo descrito na Figura 2.

## <a name="step-3-limiting-functionality-based-on-the-currently-logged-in-user"></a>Etapa 3: limitando a funcionalidade com base no usuário conectado no momento

A autorização de URL facilita a especificação de regras de autorização grosseira. Como vimos na etapa 1, com a autorização de URL, podemos declarar sucintamente quais identidades são permitidas e quais delas são negadas de exibir uma página específica ou todas as páginas em uma pasta. Em alguns cenários, no entanto, talvez queiramos permitir que todos os usuários visitem uma página, mas limitamos a funcionalidade da página com base no usuário que o está visitando.

Considere o caso de um site de comércio eletrônico que permite que os visitantes autenticados revisem seus produtos. Quando um usuário anônimo visita a página de um produto, ele vê apenas as informações do produto e não teria a oportunidade de sair de uma revisão. No entanto, um usuário autenticado visitando a mesma página veria a interface de revisão. Se o usuário autenticado ainda não tiver revisado este produto, a interface permitirá que eles enviassem uma análise; caso contrário, ele mostraria sua revisão enviada anteriormente. Para levar esse cenário um pouco além, a página do produto pode mostrar informações adicionais e oferecer recursos estendidos para os usuários que trabalham para a empresa de comércio eletrônico. Por exemplo, a página do produto pode listar o estoque em estoque e incluir opções para editar o preço e a descrição do produto quando visitado por um funcionário.

Essas regras de autorização de refinamento podem ser implementadas de forma declarativa ou programática (ou por meio de uma combinação das duas). Na próxima seção, veremos como implementar a autorização refinada por meio do controle LoginView. Depois disso, exploraremos técnicas programáticas. Antes que possamos examinar a aplicação de regras de autorização refinadas, no entanto, primeiro precisamos criar uma página cuja funcionalidade depende do usuário que a visita.

Vamos criar uma página que lista os arquivos em um diretório específico em um GridView. Juntamente com a listagem de nome, tamanho e outras informações de cada arquivo, o GridView incluirá duas colunas de LinkButtons: uma exibição intitulada e outra intitulada excluir. Se o modo de exibição LinkButton for clicado, o conteúdo do arquivo selecionado será exibido; se a exclusão de LinkButton for clicada, o arquivo será excluído. Inicialmente, vamos criar essa página de modo que sua funcionalidade de exibição e exclusão esteja disponível para todos os usuários. Nas seções usando o controle LoginView e limitando programaticamente a funcionalidade, veremos como habilitar ou desabilitar esses recursos com base no usuário que está visitando a página.

> [!NOTE]
> A página ASP.NET que estamos prestes a Compilar usa um controle GridView para exibir uma lista de arquivos. Como essa série de tutoriais se concentra em autenticação de formulários, autorização, contas de usuário e funções, não quero gastar muito tempo discutindo o funcionamento interno do controle GridView. Embora este tutorial forneça instruções passo a passo específicas para configurar essa página, ele não se aprofunda nos detalhes de por que determinadas opções foram feitas ou quais propriedades específicas de efeito têm sobre a saída renderizada. Para um exame completo do controle GridView, consulte meu *[trabalho com dados na](../../data-access/index.md)* série de tutoriais do ASP.NET 2,0.

Comece abrindo o arquivo de `UserBasedAuthorization.aspx` na pasta `Membership` e adicionando um controle GridView à página denominada `FilesGrid`. Na marca inteligente do GridView, clique no link Editar colunas para iniciar a caixa de diálogo campos. A partir daqui, desmarque a caixa de seleção Gerar campos automaticamente no canto inferior esquerdo. Em seguida, adicione um botão de seleção, um botão de exclusão e dois BoundFields no canto superior esquerdo (os botões selecionar e excluir podem ser encontrados no tipo de comando). Defina a propriedade `SelectText` do botão Selecionar para exibir e as propriedades `HeaderText` e `DataField` da primeira BoundField como nome. Defina a propriedade de `HeaderText` da segunda BoundField como tamanho em bytes, sua propriedade `DataField` para comprimento, sua propriedade `DataFormatString` como {0:N0} e sua propriedade `HtmlEncode` como false.

Depois de configurar as colunas do GridView, clique em OK para fechar a caixa de diálogo campos. Na janela Propriedades, defina a propriedade `DataKeyNames` do GridView como `FullName`. Neste ponto, a marcação declarativa do GridView deve ser semelhante ao seguinte:

[!code-aspx[Main](user-based-authorization-cs/samples/sample9.aspx)]

Com a marcação do GridView criada, estamos prontos para escrever o código que irá recuperar os arquivos em um diretório específico e associá-los ao GridView. Adicione o seguinte código ao manipulador de eventos de `Page_Load` da página:

[!code-csharp[Main](user-based-authorization-cs/samples/sample10.cs)]

O código acima usa a [classe`DirectoryInfo`](https://msdn.microsoft.com/library/system.io.directoryinfo.aspx) para obter uma lista dos arquivos na pasta raiz do aplicativo. O [método`GetFiles()`](https://msdn.microsoft.com/library/system.io.directoryinfo.getfiles.aspx) retorna todos os arquivos no diretório como uma matriz de [objetos`FileInfo`](https://msdn.microsoft.com/library/system.io.fileinfo.aspx), que, em seguida, é associado ao GridView. O objeto `FileInfo` tem uma variedade de propriedades, como `Name`, `Length`e `IsReadOnly`, entre outras. Como você pode ver em sua marcação declarativa, o GridView exibe apenas as propriedades `Name` e `Length`.

> [!NOTE]
> As classes `DirectoryInfo` e `FileInfo` são encontradas no [namespace`System.IO`](https://msdn.microsoft.com/library/system.io.aspx). Portanto, você precisará preceder esses nomes de classe com seus nomes de namespace ou ter o namespace importado para o arquivo de classe (via `using System.IO`).

Reserve um tempo para visitar esta página por meio de um navegador. Ele exibirá a lista de arquivos que residem no diretório raiz do aplicativo. Clicar em qualquer uma das exibições ou excluir LinkButton causará um postback, mas nenhuma ação ocorrerá porque ainda criamos os manipuladores de eventos necessários.

[![o GridView lista os arquivos no diretório raiz do aplicativo Web](user-based-authorization-cs/_static/image20.png)](user-based-authorization-cs/_static/image19.png)

**Figura 7**: o GridView lista os arquivos no diretório raiz do aplicativo Web ([clique para exibir a imagem em tamanho normal](user-based-authorization-cs/_static/image21.png))

Precisamos de um meio para exibir o conteúdo do arquivo selecionado. Retorne ao Visual Studio e adicione uma caixa de texto chamada `FileContents` acima do GridView. Defina sua propriedade `TextMode` como `MultiLine` e suas propriedades `Columns` e `Rows` como 95% e 10, respectivamente.

[!code-aspx[Main](user-based-authorization-cs/samples/sample11.aspx)]

Em seguida, crie um manipulador de eventos para o [evento`SelectedIndexChanged`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindexchanged.aspx) do GridView e adicione o seguinte código:

[!code-csharp[Main](user-based-authorization-cs/samples/sample12.cs)]

Esse código usa a propriedade `SelectedValue` do GridView para determinar o nome de arquivo completo do arquivo selecionado. Internamente, a coleção de `DataKeys` é referenciada para obter o `SelectedValue`, portanto, é imperativo que você defina a propriedade de `DataKeyNames` do GridView como nome, conforme descrito anteriormente nesta etapa. A [classe`File`](https://msdn.microsoft.com/library/system.io.file.aspx) é usada para ler o conteúdo do arquivo selecionado em uma cadeia de caracteres, que é atribuída à propriedade `Text` da caixa de texto do `FileContents`, exibindo assim o conteúdo do arquivo selecionado na página.

[![o conteúdo do arquivo selecionado é exibido na caixa de texto](user-based-authorization-cs/_static/image23.png)](user-based-authorization-cs/_static/image22.png)

**Figura 8**: o conteúdo do arquivo selecionado é exibido na caixa de texto ([clique para exibir a imagem em tamanho normal](user-based-authorization-cs/_static/image24.png))

> [!NOTE]
> Se você exibir o conteúdo de um arquivo que contém marcação HTML e, em seguida, tentar exibir ou excluir um arquivo, receberá um erro de `HttpRequestValidationException`. Isso ocorre porque, no postback, o conteúdo da caixa de texto é enviado de volta para o servidor Web. Por padrão, ASP.NET gera um erro `HttpRequestValidationException` sempre que um conteúdo de postback potencialmente perigoso, como marcação HTML, é detectado. Para desabilitar a ocorrência desse erro, desative a validação de solicitação da página adicionando `ValidateRequest="false"` à diretiva `@Page`. Para obter mais informações sobre os benefícios da validação de solicitação, bem como as precauções que você deve tomar ao desabilitá-la, leia [a validação da solicitação-prevenção de ataques de script](https://asp.net/learn/whitepapers/request-validation/).

Por fim, adicione um manipulador de eventos com o seguinte código para o [evento de`RowDeleting`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx)do GridView:

[!code-csharp[Main](user-based-authorization-cs/samples/sample13.cs)]

O código simplesmente exibe o nome completo do arquivo a ser excluído na caixa de texto `FileContents` *sem* realmente excluir o arquivo.

[![clicar no botão excluir não excluirá realmente o arquivo](user-based-authorization-cs/_static/image26.png)](user-based-authorization-cs/_static/image25.png)

**Figura 9**: clicar no botão excluir não excluirá realmente o arquivo ([clique para exibir a imagem em tamanho normal](user-based-authorization-cs/_static/image27.png))

Na etapa 1, configuramos as regras de autorização de URL para proibir usuários anônimos de exibir as páginas na pasta `Membership`. Para exibir melhor a autenticação refinada, vamos permitir que usuários anônimos visitem a página `UserBasedAuthorization.aspx`, mas com funcionalidade limitada. Para abrir essa página para ser acessada por todos os usuários, adicione o seguinte elemento `<location>` ao arquivo `Web.config` na pasta `Membership`:

[!code-xml[Main](user-based-authorization-cs/samples/sample14.xml)]

Depois de adicionar esse `<location>` elemento, teste as novas regras de autorização de URL fazendo logoff do site. Como um usuário anônimo, você deve ter permissão para visitar a página `UserBasedAuthorization.aspx`.

Atualmente, qualquer usuário autenticado ou anônimo pode visitar a página `UserBasedAuthorization.aspx` e exibir ou excluir arquivos. Vamos fazê-lo para que somente os usuários autenticados possam exibir o conteúdo de um arquivo e somente Tito possa excluir um arquivo. Essas regras de autorização refinadas podem ser aplicadas declarativamente, programaticamente ou por meio de uma combinação de ambos os métodos. Vamos usar a abordagem declarativa para limitar quem pode exibir o conteúdo de um arquivo; Usaremos a abordagem programática para limitar quem pode excluir um arquivo.

### <a name="using-the-loginview-control"></a>Usando o controle LoginView

Como vimos nos tutoriais anteriores, o controle LoginView é útil para exibir interfaces diferentes para usuários autenticados e anônimos e oferece uma maneira fácil de ocultar a funcionalidade que não é acessível a usuários anônimos. Como os usuários anônimos não podem exibir ou excluir arquivos, só precisamos mostrar a caixa de texto `FileContents` quando a página é visitada por um usuário autenticado. Para conseguir isso, adicione um controle LoginView à página, nomeie-o `LoginViewForFileContentsTextBox`e mova a marcação declarativa de `FileContents` caixa de texto para a `LoggedInTemplate`do controle LoginView.

[!code-aspx[Main](user-based-authorization-cs/samples/sample15.aspx)]

Os controles da Web nos modelos do LoginView não são mais acessíveis diretamente da classe code-behind. Por exemplo, o `SelectedIndexChanged` do `FilesGrid` GridView e os manipuladores de eventos `RowDeleting` atualmente fazem referência ao controle TextBox do `FileContents` com um código como:

[!code-csharp[Main](user-based-authorization-cs/samples/sample16.cs)]

No entanto, esse código não é mais válido. Ao mover a caixa de texto `FileContents` para a `LoggedInTemplate` a caixa de texto não pode ser acessada diretamente. Em vez disso, devemos usar o método `FindControl("controlId")` para referenciar o controle programaticamente. Atualize os manipuladores de eventos `FilesGrid` para referenciar a caixa de texto da seguinte forma:

[!code-csharp[Main](user-based-authorization-cs/samples/sample17.cs)]

Depois de mover a caixa de texto para o `LoggedInTemplate` do LoginView e atualizar o código da página para fazer referência à caixa de texto usando o padrão de `FindControl("controlId")`, visite a página como um usuário anônimo. Como mostra a Figura 10, a caixa de texto `FileContents` não é exibida. No entanto, o modo de exibição LinkButton ainda é exibido.

[![o controle LoginView só renderiza a caixa de texto FileContent para usuários autenticados](user-based-authorization-cs/_static/image29.png)](user-based-authorization-cs/_static/image28.png)

**Figura 10**: o controle LoginView só renderiza a caixa de texto `FileContents` para usuários autenticados ([clique para exibir a imagem em tamanho normal](user-based-authorization-cs/_static/image30.png))

Uma maneira de ocultar o botão Exibir para usuários anônimos é converter o campo GridView em um TemplateField. Isso irá gerar um modelo que contém a marcação declarativa para o modo de exibição LinkButton. Em seguida, podemos adicionar um controle LoginView ao TemplateField e posicionar o LinkButton dentro do `LoggedInTemplate`do LoginView, ocultando, assim, o botão de exibição de visitantes anônimos. Para fazer isso, clique no link Editar colunas na marca inteligente do GridView para iniciar a caixa de diálogo campos. Em seguida, selecione o botão Selecionar na lista no canto inferior esquerdo e, em seguida, clique no link converter este campo em um modelo. Isso modificará a marcação declarativa do campo de:

[!code-aspx[Main](user-based-authorization-cs/samples/sample18.aspx)]

 Para: 

[!code-aspx[Main](user-based-authorization-cs/samples/sample19.aspx)]

Neste ponto, podemos adicionar um LoginView ao TemplateField. A marcação a seguir exibe o modo de exibição LinkButton somente para usuários autenticados.

[!code-aspx[Main](user-based-authorization-cs/samples/sample20.aspx)]

Como mostra a Figura 11, o resultado final não é tão bem quanto a coluna de exibição ainda é exibida mesmo que a exibição LinkButtons dentro da coluna esteja oculta. Veremos como ocultar toda a coluna GridView (e não apenas o LinkButton) na próxima seção.

[![o controle LoginView oculta a exibição LinkButtons para visitantes anônimos](user-based-authorization-cs/_static/image32.png)](user-based-authorization-cs/_static/image31.png)

**Figura 11**: o controle LoginView oculta a exibição LinkButtons para visitantes anônimos ([clique para exibir a imagem em tamanho normal](user-based-authorization-cs/_static/image33.png))

### <a name="programmatically-limiting-functionality"></a>Limitando programaticamente a funcionalidade

Em algumas circunstâncias, técnicas declarativas são insuficientes para limitar a funcionalidade a uma página. Por exemplo, a disponibilidade de determinada funcionalidade de página pode depender de critérios além de se o usuário que está visitando a página é anônimo ou autenticado. Nesses casos, os vários elementos da interface do usuário podem ser exibidos ou ocultados por meio de programação.

Para limitar a funcionalidade programaticamente, precisamos executar duas tarefas:

1. Determinar se o usuário que visita a página pode acessar a funcionalidade e
2. Modifique programaticamente a interface do usuário com base em se o usuário tem acesso à funcionalidade em questão.

Para demonstrar a aplicação dessas duas tarefas, vamos permitir apenas que o Tito exclua arquivos do GridView. Nossa primeira tarefa, então, é determinar se ela está Tito visitando a página. Depois de determinado, precisamos ocultar (ou mostrar) a coluna de exclusão do GridView. As colunas do GridView podem ser acessadas por meio de sua propriedade `Columns`; uma coluna só será renderizada se sua propriedade `Visible` estiver definida como `true` (o padrão).

Adicione o seguinte código ao manipulador de eventos `Page_Load` antes de vincular os dados ao GridView:

[!code-csharp[Main](user-based-authorization-cs/samples/sample21.cs)]

Como discutimos em [*uma visão geral do tutorial de autenticação de formulários*](../introduction/an-overview-of-forms-authentication-cs.md) , `User.Identity.Name` retorna o nome da identidade. Isso corresponde ao nome de usuário inserido no controle de logon. Se for Tito visitando a página, a propriedade de `Visible` da segunda coluna do GridView será definida como `true`; caso contrário, ele será definido como `false`. O resultado líquido é que, quando alguém que não seja o Tito visitar a página, outro usuário autenticado ou um usuário anônimo, a coluna excluir não será renderizada (veja a Figura 12); no entanto, quando o Tito visita a página, a coluna excluir está presente (consulte a Figura 13).

[![a coluna excluir não é renderizada quando visitada por alguém diferente de Tito (como Bruce)](user-based-authorization-cs/_static/image35.png)](user-based-authorization-cs/_static/image34.png)

**Figura 12**: a coluna excluir não é renderizada quando visitada por alguém diferente de Tito (como Bruce) ([clique para exibir a imagem em tamanho normal](user-based-authorization-cs/_static/image36.png))

[![a coluna Delete é renderizada para Tito](user-based-authorization-cs/_static/image38.png)](user-based-authorization-cs/_static/image37.png)

**Figura 13**: a coluna Delete é renderizada para Tito ([clique para exibir a imagem em tamanho normal](user-based-authorization-cs/_static/image39.png))

## <a name="step-4-applying-authorization-rules-to-classes-and-methods"></a>Etapa 4: aplicando regras de autorização a classes e métodos

Na etapa 3, não permitimos que usuários anônimos exibam o conteúdo de um arquivo e proibido todos os usuários, mas Tito a exclusão de arquivos. Isso foi feito ocultando os elementos de interface do usuário associados para visitantes não autorizados por meio de técnicas declarativas e programáticas. Para nosso exemplo simples, ocultar corretamente os elementos da interface do usuário era simples, mas e quanto aos sites mais complexos em que pode haver várias maneiras diferentes de executar a mesma funcionalidade? Ao limitar essa funcionalidade a usuários não autorizados, o que acontecerá se esquecermos de ocultar ou desabilitar todos os elementos da interface do usuário aplicáveis?

Uma maneira fácil de garantir que uma parte da funcionalidade específica não possa ser acessada por um usuário não autorizado é decorar essa classe ou método com o [atributo`PrincipalPermission`](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx). Quando o tempo de execução do .NET usa uma classe ou executa um de seus métodos, ele verifica se o contexto de segurança atual tem permissão para usar a classe ou executar o método. O atributo `PrincipalPermission` fornece um mecanismo pelo qual podemos definir essas regras.

Vamos demonstrar como usar o atributo `PrincipalPermission` no `SelectedIndexChanged` do GridView e `RowDeleting` manipuladores de eventos para proibir a execução por usuários anônimos e usuários diferentes de Tito, respectivamente. Tudo o que precisamos fazer é adicionar o atributo apropriado acima de cada definição de função:

[!code-csharp[Main](user-based-authorization-cs/samples/sample22.cs)]

O atributo para o manipulador de eventos `SelectedIndexChanged` determina que somente usuários autenticados podem executar o manipulador de eventos, onde como o atributo no manipulador de eventos `RowDeleting` limita a execução a Tito.

Se, de alguma forma, um usuário que não seja o Tito tentar executar o manipulador de eventos `RowDeleting` ou um usuário não autenticado tentar executar o manipulador de eventos `SelectedIndexChanged`, o tempo de execução do .NET gerará uma `SecurityException`.

[![se o contexto de segurança não estiver autorizado a executar o método, uma SecurityException será gerada](user-based-authorization-cs/_static/image41.png)](user-based-authorization-cs/_static/image40.png)

**Figura 14**: se o contexto de segurança não estiver autorizado a executar o método, um `SecurityException` será gerado ([clique para exibir a imagem em tamanho normal](user-based-authorization-cs/_static/image42.png))

> [!NOTE]
> Para permitir que vários contextos de segurança acessem uma classe ou um método, decorar a classe ou o método com um atributo `PrincipalPermission` para cada contexto de segurança. Ou seja, para permitir que Tito e Bruce executem o manipulador de eventos `RowDeleting`, adicione *dois* atributos `PrincipalPermission`:

[!code-csharp[Main](user-based-authorization-cs/samples/sample23.cs)]

Além das páginas ASP.NET, muitos aplicativos também têm uma arquitetura que inclui várias camadas, como lógica de negócios e camadas de acesso a dados. Essas camadas normalmente são implementadas como bibliotecas de classes e oferecem classes e métodos para executar a lógica comercial e a funcionalidade relacionada a dados. O atributo `PrincipalPermission` é útil para aplicar regras de autorização a essas camadas.

Para obter mais informações sobre como usar o atributo `PrincipalPermission` para definir regras de autorização em classes e métodos, consulte a entrada do blog de [Scott Guthrie](https://weblogs.asp.net/scottgu/) [adicionando regras de autorização a camadas de dados e de negócios usando `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>Resumo

Neste tutorial, vimos como aplicar regras de autorização baseadas no usuário. Começamos com uma olhada em ASP. Estrutura de autorização de URL do NET. Em cada solicitação, o `UrlAuthorizationModule` do mecanismo ASP.NET inspeciona as regras de autorização de URL definidas na configuração do aplicativo para determinar se a identidade está autorizada a acessar o recurso solicitado. Em suma, a autorização de URL facilita a especificação de regras de autorização para uma página específica ou para todas as páginas em um diretório específico.

A estrutura de autorização de URL aplica regras de autorização de acordo com a página. Com a autorização de URL, a identidade solicitante está autorizada a acessar um recurso específico ou não. No entanto, muitos cenários chamam regras de autorização mais refinadas. Em vez de definir quem tem permissão para acessar uma página, talvez seja necessário permitir que todos acessem uma página, mas mostrar dados diferentes ou oferecer funcionalidade diferente, dependendo do usuário que visitar a página. A autorização em nível de página geralmente envolve a ocultação de elementos específicos da interface do usuário para impedir que usuários não autorizados acessem a funcionalidade proibida. Além disso, é possível usar atributos para restringir o acesso a classes e execução de seus métodos para determinados usuários.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Adicionando regras de autorização a camadas de dados e de negócios usando `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [Autorização de ASP.NET](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [Alterações entre o IIS6 e o IIS7 Security](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [Configurando arquivos e subdiretórios específicos](https://msdn.microsoft.com/library/6hbkh9s7.aspx)
- [Limitando a funcionalidade de modificação de dados com base no usuário](../../data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-cs.md)
- [Guias de início rápido do controle LoginView](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/loginview.aspx)
- [Compreendendo a autorização da URL do IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization)
- [Documentação técnica do `UrlAuthorizationModule`](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)
- [Trabalhando com dados no ASP.NET 2,0](../../data-access/index.md)

### <a name="about-the-author"></a>Sobre o autor

Scott Mitchell, autor de vários livros sobre ASP/ASP. NET e fundador da 4GuysFromRolla.com, tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a *[Sams ensina a ASP.NET 2,0 em 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott pode ser contatado em [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) ou por meio de seu blog em [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](validating-user-credentials-against-the-membership-user-store-cs.md)
> [Próximo](storing-additional-user-information-cs.md)
