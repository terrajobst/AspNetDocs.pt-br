---
uid: web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-vb
title: Configuração de autenticação de formulários e tópicos avançados (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, examinaremos as várias configurações de autenticação de formulários e veremos como modificá-las por meio do elemento Forms. Isso envolverá um detalhado...
ms.author: riande
ms.date: 01/14/2008
ms.assetid: 829d2f56-5c48-445b-b826-3418a450c788
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-vb
msc.type: authoredcontent
ms.openlocfilehash: 4d77816a489a4fa16cd70ec4214cd2f8ee563029
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74632163"
---
# <a name="forms-authentication-configuration-and-advanced-topics-vb"></a>Configuração de autenticação de formulários e tópicos avançados (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_03_VB.zip) ou [baixar PDF](https://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial03_AuthAdvanced_vb.pdf)

> Neste tutorial, examinaremos as várias configurações de autenticação de formulários e veremos como modificá-las por meio do elemento Forms. Isso envolverá uma visão detalhada da personalização do valor de tempo limite do tíquete de autenticação de formulários, usando uma página de logon com uma URL personalizada (como Signe. aspx em vez de login. aspx) e Tíquetes de autenticação de formulários sem cookie.

## <a name="introduction"></a>Introdução

No [tutorial anterior](an-overview-of-forms-authentication-vb.md) , examinamos as etapas necessárias para implementar a autenticação de formulários em um aplicativo ASP.net, de especificar definições de configuração em Web. config para criar uma página de logon para exibir conteúdo diferente para usuários autenticados e anônimos. Lembre-se de que configuramos o site para usar a autenticação de formulários definindo o atributo mode do elemento &lt;Authentication&gt; como Forms. O elemento de&gt; de autenticação &lt;pode, opcionalmente, incluir um formulário de &lt;&gt; elemento filho, por meio do qual uma variedade de configurações de autenticação de formulários pode ser especificada.

Neste tutorial, examinaremos as várias configurações de autenticação de formulários e veremos como modificá-las por meio do elemento &lt;Forms&gt;. Isso envolverá uma visão detalhada da personalização do valor de tempo limite do tíquete de autenticação de formulários, usando uma página de logon com uma URL personalizada (como Signe. aspx em vez de login. aspx) e Tíquetes de autenticação de formulários sem cookie. Também examinaremos a composição do tíquete de autenticação de formulários mais detalhadamente e veremos as precauções que o ASP.NET leva para garantir que os dados do tíquete estejam protegidos contra inspeção e adulteração. Por fim, veremos como armazenar dados de usuário adicionais no tíquete de autenticação de formulários e como modelar esses dados por meio de um objeto principal personalizado.

## <a name="step-1-examining-the-ltformsgt-configuration-settings"></a>Etapa 1: examinando as definições de configuração do &lt;Forms&gt;

O sistema de autenticação de formulários no ASP.NET oferece várias definições de configuração que podem ser personalizadas em uma base de aplicativo por aplicativo. Isso inclui configurações como: o tempo de vida do tíquete de autenticação de formulários; Qual tipo de proteção é aplicada ao tíquete; em quais condições tíquetes de autenticação sem cookie são usados; o caminho para a página de logon; e outras informações. Para modificar os valores padrão, adicione um [elemento&lt;forms&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx) como um filho do [elemento&gt; de autenticação&lt;](https://msdn.microsoft.com/library/532aee0e.aspx), especificando os valores de propriedade que você deseja personalizar como atributos XML, da seguinte forma:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample1.xml)]

A tabela 1 resume as propriedades que podem ser personalizadas por meio do elemento &lt;Forms&gt;. Como o Web. config é um arquivo XML, os nomes de atributo na coluna esquerda diferenciam maiúsculas de minúsculas.

| <strong>Attribute</strong> |                                                                                                                                                                                                                                     <strong>Descrição</strong>                                                                                                                                                                                                                                      |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         cookies         |                                                                                                                Esse atributo especifica em quais condições o tíquete de autenticação é armazenado em um cookie versus inserido na URL. Os valores permitidos são: UseCookies; UseUri; Detecção automática e UseDeviceProfile (o padrão). A etapa 2 examina essa configuração mais detalhadamente.                                                                                                                |
|         defaultUrl         |                                                                                                                                                         Indica a URL para a qual os usuários são redirecionados depois de entrar na página de logon, se não houver nenhum valor de RedirectUrl especificado na QueryString. O valor padrão é default. aspx.                                                                                                                                                         |
|           domain           | Ao usar tíquetes de autenticação com base em cookie, essa configuração especifica o valor de domínio do cookie s. O valor padrão é uma cadeia de caracteres vazia, o que faz com que o navegador use o domínio do qual foi emitido (como www.yourdomain.com). Nesse caso, o cookie <strong>não</strong> será enviado ao fazer solicitações para subdomínios, como admin.yourdomain.com. Se desejar que o cookie seja passado para todos os subdomínios, você precisará personalizar o atributo de domínio definindo-o como yourdomain.com. |
|  enableCrossAppRedirects   |                                                                                                                                                                   Um valor booliano que indica se os usuários autenticados são lembrados quando redirecionados para URLs em outros aplicativos Web no mesmo servidor. O padrão é falso.                                                                                                                                                                   |
|          loginUrl          |                                                                                                                                                                                                                      A URL da página de logon. O valor padrão é login.aspx.                                                                                                                                                                                                                      |
|            {1&gt;name&lt;1}            |                                                                                                                                                                                                   Ao usar tíquetes de autenticação baseada em cookie, o nome do cookie. O padrão é. ASPXAUTH.                                                                                                                                                                                                   |
|            path            |                                                                             Ao usar tíquetes de autenticação com base em cookie, essa configuração especifica o atributo de caminho do cookie. O atributo Path permite que um desenvolvedor limite o escopo de um cookie a uma hierarquia de diretório específica. O valor padrão é/, que informa ao navegador para enviar o cookie de tíquete de autenticação para qualquer solicitação feita ao domínio.                                                                              |
|         proteção         |                                                                                                                                            Indica quais técnicas são usadas para proteger o tíquete de autenticação de formulários. Os valores permitidos são: ALL (o padrão); Encripta None e validação. Essas configurações são discutidas detalhadamente na etapa 3.                                                                                                                                            |
|         requireSSL         |                                                                                                                                                                                Um valor booliano que indica se uma conexão SSL é necessária para transmitir o cookie de autenticação. O valor padrão é false.                                                                                                                                                                                |
|     slidingExpiration      |                                                                                                 Um valor booliano que indica se o tempo limite de s do cookie de autenticação é redefinido cada vez que o usuário visita o site durante uma única sessão. O valor padrão é verdadeiro. A política de tempo limite de tíquete de autenticação é discutida mais detalhadamente na seção especificando o valor de tempo limite do tíquete s.                                                                                                 |
|          timeout           |                                                                                                                               Especifica o tempo, em minutos, após o qual o cookie de tíquete de autenticação expira. O valor padrão é 30. A política de tempo limite de tíquete de autenticação é discutida mais detalhadamente na seção especificando o valor de tempo limite do tíquete s.                                                                                                                               |

**Tabela 1**: um resumo dos atributos do elemento do &lt;Forms&gt;

No ASP.NET 2,0 e posterior, os valores de autenticação de formulários padrão são embutidos em código na classe FormsAuthenticationConfiguration no .NET Framework. Todas as modificações devem ser aplicadas em uma base de aplicativo por aplicativo no arquivo Web. config. Isso difere do ASP.NET 1. x, em que os valores de autenticação de formulários padrão foram armazenados no arquivo Machine. config (e, portanto, poderiam ser modificados por meio da edição de Machine. config). No tópico de ASP.NET 1. x, vale a pena mencionar que um número de configurações do sistema de autenticação de formulários tem valores padrão diferentes no ASP.NET 2,0 e além de em ASP.NET 1. x. Se você estiver migrando seu aplicativo de um ambiente ASP.NET 1. x, é importante estar atento a essas diferenças. Consulte [a documentação técnica do elemento &lt;forms&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx) para obter uma lista das diferenças.

> [!NOTE]
> Várias configurações de autenticação de formulários, como o tempo limite, o domínio e o caminho, especificam detalhes para o cookie de tíquete de autenticação de formulários resultante. Para obter mais informações sobre cookies, como eles funcionam e suas várias propriedades, leia [este tutorial de cookies](http://www.quirksmode.org/js/cookies.html).

### <a name="specifying-the-tickets-timeout-value"></a>Especificando o valor de tempo limite do tíquete

O tíquete de autenticação de formulários é um token que representa uma identidade. Com tíquetes de autenticação baseada em cookie, esse token é mantido na forma de um cookie e enviado ao servidor Web em cada solicitação. A posse do token, em essência, declara, sou *username*, já fiz logon e é usada para que a identidade de um usuário possa ser lembrada em visitas à página.

O tíquete de autenticação de formulários não apenas inclui a identidade do usuário, mas também contém informações para ajudar a garantir a integridade e a segurança do token. Afinal, não queremos que um usuário perigoso seja capaz de criar um token falsificado ou de modificar um token legit de alguma forma.

Uma dessas informações incluídas no tíquete é uma *expiração*, que é a data e a hora em que o tíquete não é mais válido. Cada vez que o FormsAuthenticationModule inspeciona um tíquete de autenticação, ele garante que a expiração do tíquete ainda não tenha passado. Se tiver, ele desconsiderará o tíquete e identificará o usuário como sendo anônimo. Essa proteção ajuda a proteger contra ataques de repetição. Sem uma expiração, se um hacker fosse capaz de colocá-lo em um tíquete de autenticação válido de um usuário, talvez obtendo acesso físico ao seu computador e fazendo a raiz por meio de seus cookies, eles poderiam enviar uma solicitação ao servidor com esse tíquete de autenticação roubado e obter a entrada. Embora a expiração não impeça esse cenário, ela limita a janela durante a qual esse ataque pode ser executado com sucesso.

> [!NOTE]
> A etapa 3 detalha técnicas adicionais usadas pelo sistema de autenticação de formulários para proteger o tíquete de autenticação.

Ao criar o tíquete de autenticação, o sistema de autenticação de formulários determina sua expiração consultando a configuração de tempo limite. Conforme observado na tabela 1, a configuração de tempo limite usa como padrão 30 minutos, o que significa que quando o tíquete de autenticação de formulários é criado, sua expiração é definida como uma data e hora 30 minutos no futuro.

A expiração define um tempo absoluto no futuro quando o tíquete de autenticação de formulários expira. Mas geralmente os desenvolvedores desejam implementar uma expiração deslizante, uma que é redefinida toda vez que o usuário revisita o site. Esse comportamento é determinado pelas configurações de slidingExpiration. Se definido como true (o padrão), cada vez que o FormsAuthenticationModule autenticar um usuário, ele atualizará a expiração do tíquete. Se definido como false, a expiração não será atualizada em cada solicitação, fazendo com que o tíquete expire exatamente o número de minutos do tempo limite após quando o tíquete foi criado pela primeira vez.

> [!NOTE]
> A expiração armazenada no tíquete de autenticação é um valor de data e hora absoluto, como 2 de agosto de 2008 11:34. Além disso, a data e a hora são relativas à hora local do servidor Web. Essa decisão de design pode ter alguns efeitos colaterais interessantes sobre o horário de Verão (DST), que é quando os relógios na Estados Unidos são movidos uma hora para a frente (supondo que o servidor Web esteja hospedado em uma localidade onde o horário de verão é observado). Considere o que aconteceria para um site ASP.NET com uma expiração de 30 minutos perto do momento em que o DST começa (que está às 2:00 AM). Imagine um visitante que entra no site em 11 de março de 2008 às 1:55. Isso geraria um tíquete de autenticação de formulários que expira em 11 de março de 2008 às 2:25 (30 minutos no futuro). No entanto, uma vez que 2:00 está invertido, o relógio salta para 3:00, por causa do horário de verão. Quando o usuário carrega uma nova página seis minutos depois de entrar (às 3:01), o FormsAuthenticationModule observa que o tíquete expirou e redireciona o usuário para a página de logon. Para obter uma discussão mais detalhada sobre esse e outros singularidades de tempo limite de tíquete de autenticação, bem como soluções alternativas, pegue uma cópia da *segurança, associação e gerenciamento de função do 2,0 ASP.net do profissional* de Stefan Schackow (ISBN: 978-0-7645-9698-8).

A Figura 1 ilustra o fluxo de trabalho quando slidingExpiration é definido como false e timeout é definido como 30. Observe que o tíquete de autenticação gerado no logon contém a data de validade e esse valor não é atualizado nas solicitações subsequentes. Se o FormsAuthenticationModule descobrir que o tíquete expirou, ele o descartará e tratará a solicitação como anônima.

[![uma representação gráfica da expiração do tíquete de autenticação de formulários quando slidingExpiration é false](forms-authentication-configuration-and-advanced-topics-vb/_static/image2.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image1.png)

**Figura 01**: uma representação gráfica da expiração do tíquete de autenticação de formulários quando slidingExpiration é false ([clique para exibir a imagem em tamanho normal](forms-authentication-configuration-and-advanced-topics-vb/_static/image3.png))

A Figura 2 mostra o fluxo de trabalho quando slidingExpiration é definido como true e timeout é definido como 30. Quando uma solicitação autenticada é recebida (com um tíquete não expirado), sua expiração é atualizada para o tempo limite do número de minutos no futuro.

[![uma representação gráfica do tíquete de autenticação de formulários quando slidingExpiration é true](forms-authentication-configuration-and-advanced-topics-vb/_static/image5.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image4.png)

**Figura 02**: uma representação gráfica do tíquete de autenticação de formulários quando slidingExpiration é true ([clique para exibir a imagem em tamanho normal](forms-authentication-configuration-and-advanced-topics-vb/_static/image6.png))

Ao usar tíquetes de autenticação baseada em cookie (o padrão), essa discussão se torna um pouco mais confusa, pois os cookies também podem ter suas próprias expirações especificadas. A expiração de um cookie (ou a falta dele) instrui o navegador quando o cookie deve ser destruído. Se o cookie não tem uma expiração, ele é destruído quando o navegador é desligado. No entanto, se uma expiração estiver presente, o cookie permanecerá armazenado no computador do usuário até que a data e hora especificadas na expiração tenham passado. Quando um cookie é destruído pelo navegador, ele não é mais enviado ao servidor Web. Portanto, a destruição de um cookie é análoga ao usuário que está fazendo logoff do site.

> [!NOTE]
> É claro que um usuário pode remover proativamente todos os cookies armazenados em seu computador. No Internet Explorer 7, você deve ir para ferramentas, opções e clicar no botão excluir na seção Histórico de navegação. A partir daí, clique no botão Excluir cookies.

O sistema de autenticação de formulários cria cookies baseados em sessão ou expiração, dependendo do valor passado para o parâmetro *persistCookie* . Lembre-se de que os métodos GetAuthCookie, SetAuthCookie e RedirectFromLoginPage da classe FormsAuthentication usam dois parâmetros de entrada: *username* e *persistCookie*. A página de logon que criamos no tutorial anterior incluía uma caixa de seleção lembrar-me, que determinou se um cookie persistente foi criado. Os cookies persistentes são baseados em expiração; os cookies não persistentes são baseados em sessão.

Os conceitos de tempo limite e slidingExpiration já abordados aplicam-se os mesmos aos cookies baseados em sessão e expiração. Há apenas uma diferença mínima na execução: ao usar cookies baseados em expiração com slidingTimeout definido como true, a expiração do cookie é atualizada somente quando mais da metade do tempo especificado tiver decorrido.

Vamos atualizar as políticas de tempo limite de tíquete de autenticação de nosso site para que os tíquetes expirem após uma hora (60 minutos), usando uma expiração deslizante. Para ter efeito essa alteração, atualize o arquivo Web. config, adicionando um elemento &lt;Forms&gt; ao elemento&gt; de autenticação do &lt;com a seguinte marcação:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample2.xml)]

### <a name="using-an-login-page-url-other-than-loginaspx"></a>Usando uma URL de página de logon diferente de login. aspx

Como o FormsAuthenticationModule redireciona automaticamente os usuários não autorizados para a página de logon, ele precisa saber a URL da página de logon. Essa URL é especificada pelo atributo loginUrl no elemento &lt;Forms&gt; e usa como padrão login. aspx. Se você estiver portando por um site existente, talvez já tenha uma página de logon com uma URL diferente, uma que já tenha sido marcada e indexada pelos mecanismos de pesquisa. Em vez de renomear sua página de logon existente para login. aspx e interromper os indicadores e os marcadores dos usuários, você pode modificar o atributo loginUrl para apontar para sua página de logon.

Por exemplo, se sua página de logon foi denominada signin. aspx e estava localizada no diretório de usuários, você poderia apontar a configuração de loginUrl para ~/Users/SignIn.aspx desta forma:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample3.xml)]

Como nosso aplicativo atual já tem uma página de logon chamada Login. aspx, não há necessidade de especificar um valor personalizado no elemento &lt;Forms&gt;.

## <a name="step-2-using-cookieless-forms-authentication-tickets"></a>Etapa 2: usando tíquetes de autenticação de formulários sem cookie

Por padrão, o sistema de autenticação de formulários determina se os tíquetes de autenticação devem ser armazenados na coleção de cookies ou inseridos na URL com base no agente do usuário que está visitando o site. Todos os navegadores de área de trabalho principais, como o Internet Explorer, Firefox, Opera e Safari, dão suporte a cookies, mas nem todos os dispositivos móveis.

A política de cookie usada pelo sistema de autenticação de formulários depende da configuração sem cookie no elemento &lt;Forms&gt;, que pode ser atribuído a um dos quatro valores:

- UseCookies-especifica que tíquetes de autenticação baseada em cookie sempre serão usados.
- UseUri-indica que tíquetes de autenticação baseada em cookie nunca serão usados.
- Detecção automática – se o perfil do dispositivo não oferecer suporte a cookies, tíquetes de autenticação baseada em cookie não serão usados; Se o perfil do dispositivo oferecer suporte a cookies, um mecanismo de investigação será usado para determinar se os cookies estão habilitados.
- UseDeviceProfile-o padrão; usa tíquetes de autenticação baseada em cookie somente se o perfil do dispositivo der suporte a cookies. Nenhum mecanismo de investigação é usado.

As configurações de detecção automática e UseDeviceProfile dependem de um *perfil de dispositivo* em uma determinada se deseja usar tíquetes de autenticação sem cookie ou com base em cookie. O ASP.NET mantém um banco de dados de vários dispositivos e seus recursos, como se eles dão suporte a cookies, em qual versão do JavaScript eles dão suporte e assim por diante. Cada vez que um dispositivo solicita uma página da Web de um servidor Web, ele envia ao longo de um cabeçalho HTTP do *agente do usuário* que identifica o tipo de dispositivo. O ASP.NET corresponde automaticamente à cadeia de caracteres do agente do usuário fornecida com o perfil correspondente especificado em seu banco de dados.

> [!NOTE]
> Esse banco de dados de recursos do dispositivo é armazenado em vários arquivos XML que aderem ao [esquema do arquivo de definição do navegador](https://msdn.microsoft.com/library/ms228122.aspx). Os arquivos de perfil de dispositivo padrão estão localizados em%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG\Browsers. Você também pode adicionar arquivos personalizados à pasta de navegadores\_aplicativo do aplicativo. Para obter mais informações, consulte [como: detectar tipos de navegador no páginas da Web do ASP.net](https://msdn.microsoft.com/library/3yekbd5b.aspx).

Como a configuração padrão é UseDeviceProfile, os tíquetes de autenticação de formulários sem cookie serão usados quando o site for visitado por um dispositivo cujo perfil relata que ele não oferece suporte a cookies.

### <a name="encoding-the-authentication-ticket-in-the-url"></a>Codificando o tíquete de autenticação na URL

Os cookies são um meio natural para incluir informações do navegador em cada solicitação para um site específico, motivo pelo qual as configurações de autenticação de formulários padrão usam cookies se o dispositivo de visita dá suporte a eles. Se não houver suporte para os cookies, será necessário empregar um meio alternativo para passar o tíquete de autenticação do cliente para o servidor. Uma solução alternativa comum usada em ambientes sem cookies é codificar os dados do cookie na URL.

A melhor maneira de ver como essas informações podem ser inseridas na URL é forçar o site a usar tíquetes de autenticação sem cookie. Isso pode ser feito definindo a definição de configuração sem cookie como UseUri:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample4.xml)]

Depois de fazer essa alteração, visite o site por meio de um navegador. Ao visitar como um usuário anônimo, as URLs terão a aparência exatamente como faziam antes. Por exemplo, ao visitar a página default. aspx, minha barra de endereços do navegador mostra a seguinte URL:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/default.aspx`

No entanto, após o logon, o tíquete de autenticação de formulários é inserido na URL. Por exemplo, depois de visitar a página de logon e fazer logon como Sam, sou retornado para a página default. aspx, mas a URL desta vez é:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/default.aspx`

O tíquete de autenticação de formulários foi inserido na URL. A cadeia de caracteres (F (jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2) representa as informações de tíquete de autenticação codificada em Hex e são os mesmos dados que geralmente são armazenados em um cookie.

Para que os tíquetes de autenticação sem cookie funcionem, o sistema deve codificar todas as URLs na página para incluir os dados do tíquete de autenticação; caso contrário, o tíquete de autenticação será perdido quando o usuário clicar em um link. Felizmente, essa lógica de incorporação é executada automaticamente. Para demonstrar essa funcionalidade, abra a página default. aspx e adicione um controle de hiperlink, definindo suas propriedades Text e NavigateUrl como Test link e SomePage. aspx, respectivamente. Não importa que não haja realmente uma página em nosso projeto chamada SomePage. aspx.

Salve as alterações em Default. aspx e, em seguida, visite-as por meio de um navegador. Faça logon no site para que o tíquete de autenticação de formulários seja inserido na URL. Em seguida, em Default. aspx, clique no link testar link. O que aconteceu? Se não houver uma página chamada SomePage. aspx, ocorreu um erro 404, mas isso não é o que é importante aqui. Em vez disso, concentre-se na barra de endereços em seu navegador. Observe que ele inclui o tíquete de autenticação de formulários na URL!

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/SomePage.aspx`

A URL SomePage. aspx no link foi automaticamente convertida em uma URL que incluiu o tíquete de autenticação-não foi preciso escrever uma "lique" de código! O tíquete de autenticação de formulário será inserido automaticamente na URL para todos os hiperlinks que não iniciam com http://ou/. Não importa se o hiperlink aparece em uma chamada para Response. redirecionar, em um controle de hiperlink ou em um elemento HTML de âncora (ou seja, &lt;um href = "..."&gt;...&lt;/a&gt;). Desde que a URL não seja algo como http://www.someserver.com/SomePage.aspx ou/SomePage.aspx, o tíquete de autenticação de formulários será inserido para nós.

> [!NOTE]
> Os tíquetes de autenticação de formulários sem cookie aderem às mesmas políticas de tempo limite que os tíquetes de autenticação baseados em cookie. No entanto, tíquetes de autenticação sem cookie são mais propensos a ataques de repetição, pois o tíquete de autenticação é inserido diretamente na URL. Imagine um usuário que visita um site, faz logon e, em seguida, cola a URL em um email para um colega. Se o colega clicar nesse link antes que a expiração seja atingida, ele será conectado como o usuário que enviou o email!

## <a name="step-3-securing-the-authentication-ticket"></a>Etapa 3: proteger o tíquete de autenticação

O tíquete de autenticação de formulários é transmitido pela conexão em um cookie ou inserido diretamente na URL. Além das informações de identidade, o tíquete de autenticação também pode incluir dados do usuário (como veremos na etapa 4). Consequentemente, é importante que os dados do tíquete sejam criptografados de olhos curiosos e (ainda mais importante) que o sistema de autenticação de formulários possa garantir que o tíquete não foi violado.

Para garantir a privacidade dos dados do tíquete, o sistema de autenticação de formulários pode criptografar os dados do tíquete. A falha ao criptografar os dados de tíquete envia informações potencialmente confidenciais pela rede em texto sem formatação.

Para garantir a autenticidade de um tíquete, o sistema de autenticação de formulários deve *validar* o tíquete. A validação é o ato de garantir que uma parte específica dos dados não tenha sido modificada e seja realizada por meio de um *[Mac (código de autenticação de mensagens)](http://en.wikipedia.org/wiki/Message_authentication_code)* . Resumindo, o MAC é uma pequena informação que identifica os dados que precisam ser validados (nesse caso, o tíquete). Se os dados representados pelo MAC forem modificados, o MAC e os dados não serão correspondidos. Além disso, é computacionalmente difícil para um hacker modificar os dados e gerar seu próprio MAC para corresponder aos dados modificados.

Ao criar (ou modificar) um tíquete, o sistema de autenticação de formulários cria um MAC e o anexa aos dados do tíquete. Quando chega uma solicitação subsequente, o sistema de autenticação de formulários compara os dados do MAC e do tíquete para validar a autenticidade dos dados do tíquete. A Figura 3 ilustra esse fluxo de trabalho graficamente.

[![a autenticidade do tíquete é garantida por meio de um MAC](forms-authentication-configuration-and-advanced-topics-vb/_static/image8.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image7.png)

**Figura 03**: a autenticidade do tíquete é garantida por meio de um Mac ([clique para exibir a imagem em tamanho normal](forms-authentication-configuration-and-advanced-topics-vb/_static/image9.png))

As medidas de segurança aplicadas ao tíquete de autenticação dependem da configuração de proteção no elemento &lt;Forms&gt;. A configuração de proteção pode ser atribuída a um dos três valores a seguir:

- All-o tíquete é criptografado e assinado digitalmente (o padrão).
- Criptografia somente criptografia é aplicada-nenhum MAC é gerado.
- Nenhum-o tíquete não é criptografado nem assinado digitalmente.
- Validação-um MAC é gerado, mas os dados do tíquete são enviados pela transmissão em texto sem formatação.

A Microsoft recomenda enfaticamente o uso da configuração ALL.

### <a name="setting-the-validation-and-decryption-keys"></a>Definindo as chaves de validação e de descriptografia

Os algoritmos de criptografia e de hash usados pelo sistema de autenticação de formulários para criptografar e validar o tíquete de autenticação são personalizáveis por meio do [elemento&lt;machineKey&gt;](https://msdn.microsoft.com/library/w8h3skw9.aspx) no Web. config. A tabela 2 descreve os atributos do elemento &lt;machineKey&gt; e seus valores possíveis.

| **Attribute** | **Descrição** |
| --- | --- |
| descriptografia | Indica o algoritmo usado para criptografia. Esse atributo pode ter um dos quatro valores a seguir:-auto-o padrão; determina o algoritmo com base no comprimento do atributo decryptionKey. -AES – usa o algoritmo de [criptografia AES (AES)](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) . -DES-usa o [padrão de criptografia de dados (des)](http://en.wikipedia.org/wiki/Data_Encryption_Standard) esse algoritmo é considerado computacionalmente fraco e não deve ser usado. -3DES-usa o algoritmo [des triplo](http://en.wikipedia.org/wiki/Triple_DES) , que funciona aplicando o algoritmo des três vezes. |
| decryptionKey | A chave secreta usada pelo algoritmo de criptografia. Esse valor deve ser uma cadeia de caracteres hexadecimal do comprimento apropriado (com base no valor em descriptografia), AutoGenerate ou um valor acrescentado com, IsolateApps. Adicionar IsolateApps instrui o ASP.NET a usar um valor exclusivo para cada aplicativo. O padrão é AutoGenerate, IsolateApps. |
| validation | Indica o algoritmo usado para validação. Esse atributo pode ter um dos quatro valores a seguir:-AES-usa o algoritmo criptografia AES (AES). -MD5-usa o algoritmo [MD5 (Message-Digest 5)](http://en.wikipedia.org/wiki/MD5) . -SHA1-usa o algoritmo [SHA1](http://en.wikipedia.org/wiki/Sha1) (o padrão). -3DES-usa o algoritmo DES triplo. |
| validationKey | A chave secreta usada pelo algoritmo de validação. Esse valor deve ser uma cadeia de caracteres hexadecimal do comprimento apropriado (com base no valor na validação), gerar automaticamente ou um valor acrescentado com, IsolateApps. Adicionar IsolateApps instrui o ASP.NET a usar um valor exclusivo para cada aplicativo. O padrão é AutoGenerate, IsolateApps. |

**Tabela 2**: os atributos do elemento &lt;machineKey&gt;

Uma discussão completa sobre essas opções de criptografia e validação, bem como os prós e contras dos vários algoritmos, está além do escopo deste tutorial. Para obter uma visão detalhada desses problemas, incluindo diretrizes sobre quais algoritmos de criptografia e validação usar, quais comprimentos de chave usar e a melhor forma de gerar essas chaves, consulte segurança do *Professional ASP.NET 2,0, associação e gerenciamento de função*.

Por padrão, as chaves usadas para criptografia e validação são geradas automaticamente para cada aplicativo, e essas chaves são armazenadas na autoridade de segurança local (LSA). Em suma, as configurações padrão garantem chaves exclusivas em um servidor Web-por-Web e aplicativo por aplicativo. Consequentemente, esse comportamento padrão não funcionará para os dois cenários a seguir:

- **Web farms** – em um cenário de [Web farm](http://en.wikipedia.org/wiki/Web_farm) , um único aplicativo Web é hospedado em vários servidores Web para fins de escalabilidade e redundância. Cada solicitação de entrada é expedida para um servidor no farm, o que significa que durante o tempo de vida da sessão de um usuário, servidores diferentes podem ser usados para lidar com suas várias solicitações. Consequentemente, cada servidor deve usar as mesmas chaves de criptografia e validação para que o tíquete de autenticação de formulários criado, criptografado e validado em um servidor possa ser descriptografado e validado em um servidor diferente no farm.
- **Compartilhamento de tíquetes entre aplicativos** -um único servidor Web pode hospedar vários aplicativos ASP.net. Se você precisar que esses aplicativos diferentes compartilhem um único tíquete de autenticação de formulários, é imperativo que suas chaves de criptografia e validação correspondam.

Ao trabalhar em uma configuração de web farm ou compartilhar tíquetes de autenticação entre aplicativos no mesmo servidor, você precisará configurar o elemento de&gt; &lt;machineKey nos aplicativos afetados para que seus valores decryptionKey e validationKey correspondam.

Embora nenhum dos cenários acima se aplique ao nosso aplicativo de exemplo, ainda podemos especificar valores explícitos de decryptionKey e validationKey e definir os algoritmos a serem usados. Adicione uma configuração de&gt; machineKey &lt;ao arquivo Web. config:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample5.xml)]

Para obter mais informações, confira [como: configurar o machineKey no ASP.NET 2,0](https://msdn.microsoft.com/library/ms998288.aspx).

> [!NOTE]
> Os valores decryptionKey e validationKey foram extraídos da [página da Web](https://www.grc.com/passwords.htm)de [Steve Gibson](http://www.grc.com/stevegibson.htm), que gera 64 caracteres hexadecimais aleatórios em cada página visitada. Para diminuir a probabilidade dessas chaves chegarem aos seus aplicativos de produção, você é incentivado a substituir as chaves acima por aquelas geradas aleatoriamente da página de senhas perfeita.

## <a name="step-4-storing-additional-user-data-in-the-ticket"></a>Etapa 4: armazenando dados de usuário adicionais no tíquete

Muitos aplicativos Web exibem informações sobre ou baseiam a exibição da página no usuário conectado no momento. Por exemplo, uma página da Web pode mostrar o nome do usuário e a data da última vez que fez logon no canto superior de cada página. O tíquete de autenticação de formulários armazena o nome de usuário conectado no momento, mas quando qualquer outra informação é necessária, a página deve ir para o repositório do usuário – normalmente um banco de dados – para pesquisar as informações não armazenadas no tíquete de autenticação.

Com um pouco de código, podemos armazenar informações adicionais do usuário no tíquete de autenticação de formulários. Esses dados podem ser expressos por meio da [Propriedade UserData](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.userdata.aspx)da [classe FormsAuthenticationTicket](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx). Esse é um local útil para colocar pequenas quantidades de informações sobre o usuário que normalmente é necessário. O valor especificado na Propriedade UserData é incluído como parte do cookie de tíquete de autenticação e, assim como os outros campos de tíquete, é criptografado e validado com base na configuração do sistema de autenticação de formulários. Por padrão, UserData é uma cadeia de caracteres vazia.

Para armazenar dados do usuário no tíquete de autenticação, precisamos escrever um pouco de código na página de logon que captura as informações específicas do usuário e as armazena no tíquete. Como o UserData é uma propriedade do tipo cadeia de caracteres, os dados armazenados nele devem ser serializados corretamente como uma cadeia de caracteres. Por exemplo, imagine que nosso repositório de usuários incluía a data de nascimento de cada usuário e o nome de seu empregador, e queríamos armazenar esses dois valores de propriedade no tíquete de autenticação. Poderíamos serializar esses valores em uma cadeia de caracteres concatenando a data da cadeia de caracteres de nascimento do usuário com um pipe (|), seguido pelo nome do empregador. Para um usuário nasceu em 15 de agosto de 1974 que funciona para a Northwind Traders, atribuímos a Propriedade UserData String: 1974-08-15 | Northwind Traders.

Sempre que precisamos acessar os dados armazenados no tíquete, podemos fazer isso pegando a solicitação atual FormsAuthenticationTicket e desserializando a Propriedade UserData. No caso do exemplo da data de nascimento e do nome do empregador, dividiremos a cadeia de dados de UserData em duas subcadeias de caracteres com base no delimitador (|).

[![informações adicionais do usuário podem ser armazenadas no tíquete de autenticação](forms-authentication-configuration-and-advanced-topics-vb/_static/image11.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image10.png)

**Figura 04**: informações adicionais do usuário podem ser armazenadas no tíquete de autenticação ([clique para exibir a imagem em tamanho normal](forms-authentication-configuration-and-advanced-topics-vb/_static/image12.png))

### <a name="writing-information-to-userdata"></a>Gravando informações no UserData

Infelizmente, a adição de informações específicas do usuário a um tíquete de autenticação de formulários não é tão simples quanto pode ser esperada. A Propriedade UserData da classe FormsAuthenticationTicket é somente leitura e só pode ser especificada por meio do construtor da classe FormsAuthenticationTicket. Ao especificar a Propriedade UserData no construtor, também precisamos fornecer os outros valores do tíquete: o nome de usuário, a data de emissão, a expiração e assim por diante. Quando criamos a página de logon no tutorial anterior, tudo foi tratado para nós pela classe FormsAuthentication. Ao adicionar UserData ao FormsAuthenticationTicket, será necessário escrever um código para replicar grande parte da funcionalidade já fornecida pela classe FormsAuthentication.

Vamos explorar o código necessário para trabalhar com UserData atualizando a página login. aspx para registrar informações adicionais sobre o usuário no tíquete de autenticação. Suponhamos que nossa loja de usuários contenha informações sobre a empresa para as quais o usuário trabalha e seu título, e que desejamos capturar essas informações no tíquete de autenticação. Atualize o manipulador de eventos de clique LoginButton da página login. aspx para que o código fique semelhante ao seguinte:

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample6.vb)]

Vamos percorrer esse código uma linha por vez. O método começa definindo quatro matrizes de cadeia de caracteres: Users, passwords, companyName e titleAtCompany. Essas matrizes contêm os nomes de usuário, senhas, nomes de empresa e títulos para as contas de usuários no sistema, das quais há três: Scott, Jisun e Sam. Em um aplicativo real, esses valores seriam consultados do repositório de usuários, não embutidos no código-fonte da página.

No tutorial anterior, se as credenciais fornecidas fossem válidas, chamamos apenas de FormsAuthentication. RedirectFromLoginPage (UserName. Text, RememberMe. Checked), que realizou as seguintes etapas:

1. Criou o tíquete de autenticação de formulários
2. Gravou o tíquete no repositório apropriado. Para tíquetes de autenticação com base em cookies, a coleção de cookies do navegador é usada; para tíquetes de autenticação sem cookie, os dados do tíquete são serializados na URL
3. Redirecionar o usuário para a página apropriada

Essas etapas são replicadas no código acima. Primeiro, a cadeia de caracteres que eventualmente armazenaremos na Propriedade UserData é formada pela combinação do nome da empresa e do título, delimitando os dois valores com um caractere de barra vertical (|).

Dim userdatastring as String = String. Concat (companyName (i), "|", titleAtCompany (i))

Em seguida, o método FormsAuthentication. GetAuthCookie é invocado, que cria o tíquete de autenticação, criptografa e valida-o de acordo com as definições de configuração e o coloca em um objeto HttpCookie.

Dim authCookie as HttpCookie = FormsAuthentication. GetAuthCookie (UserName. Text, RememberMe. Checked)

Para trabalhar com o FormAuthenticationTicket incorporado no cookie, precisamos chamar o [método Decrypt](https://msdn.microsoft.com/library/system.web.security.formsauthentication.decrypt.aspx)da classe FormAuthentication, passando o valor do cookie.

Dim ticket as FormsAuthenticationTicket = FormsAuthentication. dedecrypte (authCookie. Value)

Em seguida, criamos uma *nova* instância de FormsAuthenticationTicket com base nos valores de FormsAuthenticationTicket existentes. No entanto, esse novo tíquete inclui as informações específicas do usuário (userdatastring).

Dim newTicket as FormsAuthenticationTicket = New FormsAuthenticationTicket (tíquete. Versão, tíquete. Nome, tíquete. Foi emitido, ticket. Expiração, tíquete. Ispersistente, userdatastring)

Em seguida, criptografamos (e validamos) a nova instância FormsAuthenticationTicket chamando o [método Encrypt](https://msdn.microsoft.com/library/system.web.security.formsauthentication.encrypt.aspx)e colocamos esses dados criptografados (e validados) novamente no authCookie.

authCookie. Value = FormsAuthentication. Encrypt (newTicket)

Por fim, authCookie é adicionado à coleção Response. cookies e o método GetRedirectUrl é chamado para determinar a página apropriada para enviar o usuário.

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample7.vb)]

Todo esse código é necessário porque a Propriedade UserData é somente leitura e a classe FormsAuthentication não fornece nenhum método para especificar informações de UserData em seus métodos GetAuthCookie, SetAuthCookie ou RedirectFromLoginPage.

> [!NOTE]
> O código que acabamos de examinar armazena informações específicas do usuário em um tíquete de autenticação baseado em cookies. As classes responsáveis pela serialização do tíquete de autenticação de formulários para a URL são internas ao .NET Framework. Longa história, você não pode armazenar dados de usuário em um tíquete de autenticação de formulários sem cookie.

### <a name="accessing-the-userdata-information"></a>Acessando as informações do UserData

Neste ponto, o nome e o título da empresa de cada usuário são armazenados na Propriedade UserData do tíquete de autenticação de formulários quando eles fazem logon. Essas informações podem ser acessadas do tíquete de autenticação em qualquer página sem a necessidade de uma viagem para o armazenamento do usuário. Para ilustrar como essas informações podem ser recuperadas da Propriedade UserData, vamos atualizar default. aspx para que sua mensagem de boas-vindas inclua não apenas o nome do usuário, mas também a empresa na qual eles trabalham e seu título.

Atualmente, Default. aspx contém um painel AuthenticatedMessagePanel com um controle rótulo chamado WelcomeBackMessage. Esse painel só é exibido para usuários autenticados. Atualize o código na página do default. aspx\_manipulador de eventos Load para que ele se pareça com o seguinte:

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample8.vb)]

Se a solicitação. IsAuthenticated for true, a propriedade Text do WelcomeBackMessage será definida primeiro como Welcome, *username*. Em seguida, a propriedade User. Identity é convertida em um objeto FormsIdentity para que possamos acessar o FormsAuthenticationTicket subjacente. Assim que tivermos o FormsAuthenticationTicket, desserializaremos a Propriedade UserData para o nome e o título da empresa. Isso é feito dividindo a cadeia de caracteres no caractere de pipe. O nome e o título da empresa são exibidos no rótulo WelcomeBackMessage.

A Figura 5 mostra uma captura de tela dessa exibição em ação. Fazer logon como Scott exibe uma mensagem de boas-vindas que inclui a empresa e o título de Scott.

[![a empresa e o título do usuário conectado no momento são exibidos](forms-authentication-configuration-and-advanced-topics-vb/_static/image14.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image13.png)

**Figura 05**: a empresa e o título do usuário conectado no momento são exibidos ([clique para exibir a imagem em tamanho normal](forms-authentication-configuration-and-advanced-topics-vb/_static/image15.png))

> [!NOTE]
> A Propriedade UserData do tíquete de autenticação serve como um cache para o armazenamento do usuário. Como qualquer cache, ele precisa ser atualizado quando os dados subjacentes são modificados. Por exemplo, se houver uma página da Web da qual os usuários podem atualizar seu perfil, os campos armazenados em cache na Propriedade UserData devem ser atualizados para refletir as alterações feitas pelo usuário.

## <a name="step-5-using-a-custom-principal"></a>Etapa 5: usando uma entidade personalizada

Em cada solicitação de entrada, o FormsAuthenticationModule tenta autenticar o usuário. Se um tíquete de autenticação não expirado estiver presente, o FormsAuthenticationModule atribuirá a propriedade HttpContext. User a um novo objeto GenericPrincipal. Este objeto GenericPrincipal tem uma identidade do tipo FormsIdentity, que inclui uma referência ao tíquete de autenticação de formulários. A classe GenericPrincipal contém a funcionalidade mínima necessária para uma classe que implementa IPrincipal; ela tem apenas uma propriedade Identity e um método IsInRole.

O objeto principal tem duas responsabilidades: para indicar a quais funções o usuário pertence e fornecer informações de identidade. Isso é feito por meio do método IsInRole da interface IPrincipal (*roleName*) e da propriedade Identity, respectivamente. A classe GenericPrincipal permite que uma matriz de cadeia de caracteres de nomes de função seja especificada por meio de seu construtor; seu método IsInRole (*roleName*) simplesmente verifica se o passado em *roleName* existe na matriz de cadeia de caracteres. Quando o FormsAuthenticationModule cria o GenericPrincipal, ele passa uma matriz de cadeia de caracteres vazia para o construtor de GenericPrincipal. Consequentemente, qualquer chamada para IsInRole sempre retornará false.

A classe GenericPrincipal atende às necessidades da maioria dos cenários de autenticação baseados em formulários em que as funções não são usadas. Para as situações em que a manipulação de função padrão é insuficiente ou quando você precisa associar um objeto IIdentity personalizado ao usuário, você pode criar um objeto IPrincipal personalizado durante o fluxo de trabalho de autenticação e atribuí-lo à propriedade HttpContext. User.

> [!NOTE]
> Como veremos nos tutoriais futuros, quando o ASP. A estrutura de funções da rede está habilitada; ela cria um objeto principal personalizado do tipo [RolePrincipal](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx) e substitui o objeto GenericPrincipal criado pela autenticação de formulários. Ele faz isso para personalizar o método IsInRole da entidade de segurança para interface com a API da estrutura de funções.

Como ainda não nos preocupamos com as funções, o único motivo para criar uma entidade personalizada nesse momento seria associar um objeto IIdentity personalizado à entidade de segurança. Na etapa 4, examinamos o armazenamento de informações adicionais do usuário na Propriedade UserData do tíquete de autenticação, em particular o nome da empresa do usuário e seu título. No entanto, as informações do UserData só podem ser acessadas por meio do tíquete de autenticação e, em seguida, apenas como uma cadeia de caracteres serializada, o que significa que, sempre que quisermos exibir as informações do usuário armazenadas no tíquete, precisamos analisar a Propriedade UserData.

Podemos melhorar a experiência do desenvolvedor criando uma classe que implementa o IIdentity e inclui propriedades CompanyName e title. Dessa forma, um desenvolvedor pode acessar o nome da empresa do usuário conectado no momento e o título diretamente por meio das propriedades CompanyName e title sem precisar saber como analisar a Propriedade UserData.

### <a name="creating-the-custom-identity-and-principal-classes"></a>Criando a identidade personalizada e classes de entidade de segurança

Para este tutorial, vamos criar os objetos personalizados de entidade e de identidade na pasta de código do\_de aplicativos. Comece adicionando uma pasta de código de\_de aplicativo ao seu projeto-clique com o botão direito do mouse no nome do projeto em Gerenciador de Soluções, selecione a opção Adicionar pasta ASP.NET e escolha o código do aplicativo\_. A pasta de código do\_de aplicativo é uma pasta ASP.NET especial que mantém os arquivos de classe específicos do site.

> [!NOTE]
> A pasta de código de\_de aplicativo só deve ser usada ao gerenciar seu projeto por meio do modelo de projeto de site. Se você estiver usando o [modelo de projeto de aplicativo Web](https://msdn.microsoft.com/asp.net/Aa336618.aspx), crie uma pasta padrão e adicione as classes a ela. Por exemplo, você pode adicionar uma nova pasta chamada classes e posicionar o código nela.

Em seguida, adicione dois novos arquivos de classe à pasta de código do\_de aplicativo, um chamado CustomIdentity. vb e outro chamado CustomPrincipal. vb.

[![adicionar as classes CustomIdentity e CustomPrincipal ao seu projeto](forms-authentication-configuration-and-advanced-topics-vb/_static/image17.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image16.png)

**Figura 06**: adicionar as classes CustomIdentity e CustomPrincipal ao seu projeto ([clique para exibir a imagem em tamanho normal](forms-authentication-configuration-and-advanced-topics-vb/_static/image18.png))

A classe CustomIdentity é responsável pela implementação da interface IIdentity, que define as propriedades AuthenticationType, IsAuthenticated e Name. Além dessas propriedades obrigatórias, estamos interessados em expor o tíquete de autenticação de formulários subjacente, bem como propriedades para o nome e o título da empresa do usuário. Insira o código a seguir na classe CustomIdentity.

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample9.vb)]

Observe que a classe inclui uma variável de membro FormsAuthenticationTicket (\_tíquete) e que essas informações de tíquete devem ser fornecidas por meio do construtor. Esses dados de tíquete são usados no retorno do nome da identidade; sua Propriedade UserData é analisada para retornar os valores das propriedades CompanyName e title.

Em seguida, crie a classe CustomPrincipal. Como não estamos preocupados com as funções neste momento, o construtor da classe CustomPrincipal aceita apenas um objeto CustomIdentity; seu método IsInRole sempre retorna false.

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample10.vb)]

### <a name="assigning-a-customprincipal-object-to-the-incoming-requests-security-context"></a>Atribuindo um objeto CustomPrincipal ao contexto de segurança da solicitação de entrada

Agora temos uma classe que estende a especificação de IIdentity padrão para incluir propriedades CompanyName e title, bem como uma classe de entidade personalizada que usa a identidade personalizada. Estamos prontos para entrar no pipeline ASP.NET e atribuir nosso objeto principal personalizado ao contexto de segurança da solicitação de entrada.

O pipeline ASP.NET usa uma solicitação de entrada e a processa por meio de várias etapas. Em cada etapa, um evento específico é gerado, possibilitando que os desenvolvedores toquem no pipeline ASP.NET e modifiquem a solicitação em determinados pontos em seu ciclo de vida. O FormsAuthenticationModule, por exemplo, aguarda que o ASP.NET gere o [evento AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx), no ponto em que ele inspeciona a solicitação de entrada para um tíquete de autenticação. Se um tíquete de autenticação for encontrado, um objeto GenericPrincipal será criado e atribuído à propriedade HttpContext. User.

Após o evento AuthenticateRequest, o pipeline ASP.NET gera o [evento PostAuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx), que é onde podemos substituir o objeto GenericPrincipal criado pelo FormsAuthenticationModule por uma instância do nosso objeto CustomPrincipal. A Figura 7 descreve esse fluxo de trabalho.

[![o GenericPrincipal é substituído por um CustomPrincipal no evento PostAuthenticationRequest](forms-authentication-configuration-and-advanced-topics-vb/_static/image20.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image19.png)

**Figura 07**: o GenericPrincipal é substituído por um CustomPrincipal no evento PostAuthenticationRequest ([clique para exibir a imagem em tamanho normal](forms-authentication-configuration-and-advanced-topics-vb/_static/image21.png))

Para executar o código em resposta a um evento de pipeline ASP.NET, podemos criar o manipulador de eventos apropriado em global. asax ou criar nosso próprio módulo HTTP. Para este tutorial, vamos criar o manipulador de eventos em global. asax. Comece adicionando global. asax ao seu site. Clique com o botão direito do mouse no nome do projeto em Gerenciador de Soluções e adicione um item do tipo classe de aplicativo global denominada global. asax.

[![adicionar um arquivo global. asax ao seu site](forms-authentication-configuration-and-advanced-topics-vb/_static/image23.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image22.png)

**Figura 08**: adicionar um arquivo global. asax ao seu site ([clique para exibir a imagem em tamanho normal](forms-authentication-configuration-and-advanced-topics-vb/_static/image24.png))

O modelo global. asax padrão inclui manipuladores de eventos para vários eventos de pipeline ASP.NET, incluindo o evento de início, término e [erro](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx), entre outros. Sinta-se à vontade para remover esses manipuladores de eventos, pois eles não são necessários para esse aplicativo. O evento no qual estamos interessados é PostAuthenticateRequest. Atualize seu arquivo global. asax para que sua marcação tenha uma aparência semelhante à seguinte:

[!code-aspx[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample11.aspx)]

O método OnPostAuthenticateRequest do aplicativo\_é executado sempre que o tempo de execução do ASP.NET gera o evento PostAuthenticateRequest, que ocorre uma vez em cada solicitação de página de entrada. O manipulador de eventos começa verificando se o usuário está autenticado e se foi autenticado por meio da autenticação de formulários. Nesse caso, um novo objeto CustomIdentity é criado e passado o tíquete de autenticação da solicitação atual em seu construtor. Depois disso, um objeto CustomPrincipal é criado e passado o objeto CustomIdentity recém-criado em seu construtor. Por fim, o contexto de segurança da solicitação atual é atribuído ao objeto CustomPrincipal recém-criado.

Observe que a última etapa – associando o objeto CustomPrincipal ao contexto de segurança da solicitação – atribui a entidade a duas propriedades: HttpContext. User e thread. CurrentPrincipal. Essas duas atribuições são necessárias devido à maneira como os contextos de segurança são manipulados em ASP.NET. O .NET Framework associa um contexto de segurança a cada thread em execução; essas informações estão disponíveis como um objeto IPrincipal por meio da [propriedade CurrentPrincipal](https://msdn.microsoft.com/library/system.threading.thread.currentcontext.aspx)do [objeto thread](https://msdn.microsoft.com/library/system.threading.thread.aspx). O que é confuso é que o ASP.NET tem suas próprias informações de contexto de segurança (HttpContext. User).

Em determinados cenários, a propriedade Thread. CurrentPrincipal é examinada ao determinar o contexto de segurança; em outros cenários, HttpContext. User é usado. Por exemplo, há recursos de segurança no .NET que permitem aos desenvolvedores declarar declarativamente quais usuários ou funções podem instanciar uma classe ou invocar métodos específicos (consulte [adicionando regras de autorização a camadas de dados e de negócios usando principalpermissionattributes](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)). Sob os bastidores, essas técnicas declarativas determinam o contexto de segurança por meio da propriedade Thread. CurrentPrincipal.

Em outros cenários, a propriedade HttpContext. User é usada. Por exemplo, no tutorial anterior, usamos essa propriedade para exibir o nome de usuário conectado no momento. Obviamente, é imperativo que as informações de contexto de segurança nas propriedades thread. CurrentPrincipal e HttpContext. User correspondam.

O tempo de execução ASP.NET sincroniza automaticamente esses valores de propriedade para nós. No entanto, essa sincronização ocorre após o evento AuthenticateRequest, mas *antes* do evento PostAuthenticateRequest. Consequentemente, ao adicionar uma entidade de segurança personalizada no evento PostAuthenticateRequest, precisamos ter certeza de que é necessário atribuir manualmente o thread. CurrentPrincipal ou o thread. CurrentPrincipal e HttpContext. User estará fora de sincronia. Consulte [Context. User vs. thread. CurrentPrincipal](http://leastprivilege.com/2005/11/23/context-user-vs-thread-currentprincipal/) para obter uma discussão mais detalhada sobre esse problema.

### <a name="accessing-the-companyname-and-title-properties"></a>Acessando as propriedades CompanyName e title

Sempre que uma solicitação chega e é expedida para o mecanismo ASP.NET, o aplicativo\_manipulador de eventos OnPostAuthenticateRequest no global. asax será acionado. Se a solicitação tiver sido autenticada com êxito pelo FormsAuthenticationModule, o manipulador de eventos criará um novo objeto CustomPrincipal com um objeto CustomIdentity com base no tíquete de autenticação de formulários. Com essa lógica em vigor, o acesso a informações sobre o nome e o título da empresa do usuário conectado no momento é incrivelmente simples.

Volte para a página\_carregar o manipulador de eventos em Default. aspx, em que, na etapa 4, escrevemos o código para recuperar o tíquete de autenticação de formulário e analisar a Propriedade UserData para exibir o nome e o título da empresa do usuário. Com os objetos CustomPrincipal e CustomIdentity em uso agora, não há necessidade de analisar os valores da Propriedade UserData do tíquete. Em vez disso, basta obter uma referência ao objeto CustomIdentity e usar suas propriedades CompanyName e title:

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample12.vb)]

## <a name="summary"></a>Resumo

Neste tutorial, examinamos como personalizar as configurações do sistema de autenticação de formulários por meio de Web. config. Analisamos como a expiração do tíquete de autenticação é tratada e como as proteções de criptografia e validação são usadas para proteger o tíquete contra inspeção e modificação. Por fim, discutimos usando a Propriedade UserData do tíquete de autenticação para armazenar informações adicionais do usuário no próprio tíquete e como usar objetos de identidade e entidade personalizados para expor essas informações de maneira mais amigável para o desenvolvedor.

Este tutorial conclui nosso exame de autenticação de formulários no ASP.NET. O próximo tutorial inicia nossa jornada na estrutura de associação.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Disparando a autenticação de formulários](http://aspnet.4guysfromrolla.com/articles/072005-1.aspx)
- [Explicado: autenticação de formulários no ASP.NET 2,0](https://msdn.microsoft.com/library/aa480476.aspx)
- [Como: proteger a autenticação de formulários no ASP.NET 2,0](https://msdn.microsoft.com/library/ms998310.aspx)
- [Gerenciamento de função, associação e segurança do Professional ASP.NET 2,0](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Protegendo controles de logon](https://msdn.microsoft.com/library/ms178346.aspx)
- [O elemento de&gt; de autenticação &lt;](https://msdn.microsoft.com/library/532aee0e.aspx)
- [O elemento &lt;Forms&gt; para &lt;autenticação&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx)
- [O elemento&gt; machineKey &lt;](https://msdn.microsoft.com/library/w8h3skw9.aspx)
- [Noções básicas sobre o cookie e o tíquete de autenticação de formulários](https://support.microsoft.com/kb/910443)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Treinamento em vídeo sobre tópicos contidos neste tutorial

- [Como alterar as propriedades de autenticação de formulários](../../../videos/authentication/how-to-change-the-forms-authentication-properties.md)
- [Como configurar e usar a autenticação sem cookie em um aplicativo ASP.NET](../../../videos/authentication/how-to-setup-and-use-cookie-less-authentication-in-an-aspnet-application.md)
- [Realocação de logon de formulários do ASP](../../../videos/authentication/asp-forms-login-relocation.md)
- [Configuração de chave personalizada de logon de formulários](../../../videos/authentication/forms-login-custom-key-configuration.md)
- [Adicionar dados personalizados ao método de autenticação](../../../videos/authentication/add-custom-data-to-the-authentication-method.md)
- [Usar objetos de entidade de segurança personalizada](../../../videos/authentication/use-custom-principal-objects.md)

### <a name="about-the-author"></a>Sobre o autor

Scott Mitchell, autor de vários livros sobre ASP/ASP. NET e fundador da 4GuysFromRolla.com, tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a *[Sams ensina a ASP.NET 2,0 em 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott pode ser contatado em [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) ou por meio de seu blog em [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. O revisor de Lead para este tutorial foi Alicja Maziarz. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Anterior](an-overview-of-forms-authentication-vb.md)
