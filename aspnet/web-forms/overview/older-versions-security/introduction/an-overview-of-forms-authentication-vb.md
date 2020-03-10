---
uid: web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-vb
title: Uma visão geral da autenticação de formulários (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, vamos mudar de mera discussão para implementação; em particular, veremos como implementar a autenticação de formulários. O aplicativo Web w...
ms.author: riande
ms.date: 01/14/2008
ms.assetid: 83267f7d-64d9-41ee-82cf-da91b1bf534d
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: d8ceb6b5290300992e52199caa9314c573de1942
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78633899"
---
# <a name="an-overview-of-forms-authentication-vb"></a>Uma visão geral da autenticação de formulários (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_02_VB.zip) ou [baixar PDF](https://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial02_FormsAuth_vb.pdf)

> Neste tutorial, vamos mudar de mera discussão para implementação; em particular, veremos como implementar a autenticação de formulários. O aplicativo Web que começamos a construir neste tutorial continuará a ser criado nos tutoriais subsequentes, à medida que mudarmos da autenticação de formulários simples para associação e funções.
> 
> Consulte este vídeo para obter mais informações sobre este tópico: [usando a autenticação básica de formulários no ASP.net](../../../videos/authentication/using-basic-forms-authentication-in-aspnet.md).

## <a name="introduction"></a>Introdução

No [tutorial anterior](security-basics-and-asp-net-support-vb.md) , discutimos as várias opções de autenticação, autorização e conta de usuário fornecidas pelo ASP.net. Neste tutorial, vamos mudar de mera discussão para implementação; em particular, veremos como implementar a autenticação de formulários. O aplicativo Web que começamos a construir neste tutorial continuará a ser criado nos tutoriais subsequentes, à medida que mudarmos da autenticação de formulários simples para associação e funções.

Este tutorial começa com uma visão detalhada do fluxo de trabalho de autenticação de formulários, um tópico que abordamos no tutorial anterior. Depois disso, criaremos um site ASP.NET por meio do qual será possível demonstrar os conceitos da autenticação de formulários. Em seguida, configuraremos o site para usar a autenticação de formulários, criaremos uma página de logon simples e veremos como determinar, no código, se um usuário é autenticado e, nesse caso, o nome de usuário com o qual se conectou.

Entender o fluxo de trabalho de autenticação de formulários, habilitá-lo em um aplicativo Web e criar as páginas de logon e logoff são etapas vitais para criar um aplicativo ASP.NET que dá suporte a contas de usuário e autentica usuários por meio de uma página da Web. Por causa disso, e como esses tutoriais se baseiam em um do outro, eu o encorajava a trabalhar com este tutorial por completo antes de passar para o próximo, mesmo que você já tenha experiência com a configuração da autenticação de formulários nos projetos anteriores.

## <a name="understanding-the-forms-authentication-workflow"></a>Noções básicas sobre o fluxo de trabalho de autenticação de formulários

Quando o tempo de execução ASP.NET processa uma solicitação de um recurso ASP.NET, como uma página ASP.NET ou um serviço Web ASP.NET, a solicitação gera vários eventos durante seu ciclo de vida. Há eventos gerados no início e no final da solicitação, aqueles gerados quando a solicitação está sendo autenticada e autorizada, um evento gerado no caso de uma exceção sem tratamento e assim por diante. Para ver uma lista completa dos eventos, consulte os [eventos do objeto HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication_events.aspx).

*Módulos http* são classes gerenciadas cujo código é executado em resposta a um evento específico no ciclo de vida da solicitação. O ASP.NET é fornecido com vários módulos HTTP que executam tarefas essenciais em segundo plano. Dois módulos HTTP internos que são especialmente relevantes para nossa discussão são:

- **[FormsAuthenticationModule](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx)** -autentica o usuário inspecionando o tíquete de autenticação de formulários, que normalmente é incluído na coleção de cookies do usuário. Se nenhum tíquete de autenticação de formulários estiver presente, o usuário será anônimo.
- **[UrlAuthorizationModule](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)** -determina se o usuário atual está autorizado ou não a acessar a URL solicitada. Esse módulo determina a autoridade consultando as regras de autorização especificadas nos arquivos de configuração do aplicativo. O ASP.NET também inclui o [FileAuthorizationModule](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx) que determina a autoridade consultando as ACLs de arquivo (s) solicitadas.

O FormsAuthenticationModule tenta autenticar o usuário antes do UrlAuthorizationModule (e do FileAuthorizationModule) em execução. Se o usuário que está fazendo a solicitação não estiver autorizado a acessar o recurso solicitado, o módulo de autorização encerrará a solicitação e retornará um status de [HTTP 401 não autorizado](http://www.checkupdown.com/status/E401.html) . Nos cenários de autenticação do Windows, o status HTTP 401 é retornado para o navegador. Esse código de status faz com que o navegador solicite suas credenciais ao usuário por meio de uma caixa de diálogo modal. Com a autenticação de formulários, no entanto, o status HTTP 401 não autorizado nunca é enviado para o navegador porque o FormsAuthenticationModule detecta esse status e o modifica para redirecionar o usuário para a página de logon (por meio de um status de [redirecionamento HTTP 302](http://www.checkupdown.com/status/E302.html) ).

A responsabilidade da página de logon é determinar se as credenciais do usuário são válidas e, nesse caso, criar um tíquete de autenticação de formulários e redirecionar o usuário de volta para a página que ele estava tentando visitar. O tíquete de autenticação é incluído em solicitações subsequentes para as páginas no site, que o FormsAuthenticationModule usa para identificar o usuário.

[![o fluxo de trabalho de autenticação de formulários](an-overview-of-forms-authentication-vb/_static/image2.png)](an-overview-of-forms-authentication-vb/_static/image1.png)

**Figura 01**: o fluxo de trabalho de autenticação de formulários ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image3.png))

### <a name="remembering-the-authentication-ticket-across-page-visits"></a>Lembrando o tíquete de autenticação entre visitas à página

Após o logon, o tíquete de autenticação de formulários deve ser enviado de volta ao servidor Web em cada solicitação para que o usuário permaneça conectado enquanto navega no site. Normalmente, isso é feito colocando o tíquete de autenticação na coleção de cookies do usuário. [Cookies](http://en.wikipedia.org/wiki/HTTP_cookie) são pequenos arquivos de texto que residem no computador do usuário e são transmitidos nos cabeçalhos HTTP em cada solicitação para o site que criou o cookie. Portanto, depois que o tíquete de autenticação de formulários tiver sido criado e armazenado nos cookies do navegador, cada visita subsequente ao site enviará o tíquete de autenticação junto com a solicitação, identificando, assim, o usuário.

> [!NOTE]
> O aplicativo Web de demonstração usado em cada tutorial está disponível como um download. Este aplicativo baixável foi criado com o Visual Web Developer 2008 destinado para a versão de .NET Framework 3,5. Como o aplicativo é destinado ao .NET 3,5, seu arquivo Web. config inclui elementos de configuração adicionais de 3,5 específicos. Longa história, se você ainda tiver que instalar o .NET 3,5 em seu computador, o aplicativo Web para download não funcionará sem primeiro remover a marcação específica de 3,5 de Web. config.

Um aspecto dos cookies é a expiração, que é a data e a hora em que o navegador descarta o cookie. Quando o cookie de autenticação de formulários expira, o usuário não pode mais ser autenticado e, portanto, se torna anônimo. Quando um usuário está visitando de um terminal público, é provável que eles queiram que seu tíquete de autenticação expire quando eles fecham o navegador. No entanto, ao visitar de casa, esse mesmo usuário pode querer que o tíquete de autenticação seja lembrado entre reinicializações do navegador para que eles não precisem fazer logon novamente cada vez que visitarem o site. Essa decisão é geralmente feita pelo usuário na forma de uma caixa de seleção lembrar-me na página de logon. Na etapa 3, examinaremos como implementar uma caixa de seleção lembrar-me na página de logon. O tutorial a seguir aborda as configurações de tempo limite de tíquete de autenticação em detalhes.

> [!NOTE]
> É possível que o agente do usuário usado para fazer logon no site do não seja compatível com cookies. Nesse caso, o ASP.NET pode usar tíquetes de autenticação de formulários sem cookie. Nesse modo, o tíquete de autenticação é codificado na URL. Veremos quando tíquetes de autenticação sem cookie são usados e como eles são criados e gerenciados no próximo tutorial.

### <a name="the-scope-of-forms-authentication"></a>O escopo da autenticação de formulários

O FormsAuthenticationModule é um código gerenciado que faz parte do tempo de execução do ASP.NET. Antes da versão 7 do servidor Web [serviços de informações da Internet (IIS)](https://www.iis.net/) da Microsoft, havia uma barreira distinta entre o pipeline http do IIS e o pipeline do tempo de execução do ASP.net. Em suma, no IIS 6 e versões anteriores, o FormsAuthenticationModule só é executado quando uma solicitação é delegada do IIS para o tempo de execução do ASP.NET. Por padrão, o IIS processa o próprio conteúdo estático, como páginas HTML e arquivos CSS e de imagem, e apenas encaminha solicitações para o tempo de execução ASP.NET quando uma página com uma extensão de. aspx,. asmx ou. ashx é solicitada.

O IIS 7, no entanto, permite pipelines integrados do IIS e do ASP.NET. Com algumas definições de configuração, você pode configurar o IIS 7 para invocar o FormsAuthenticationModule para *todas as* solicitações. Além disso, com o IIS 7, você pode definir regras de autorização de URL para arquivos de qualquer tipo. Para obter mais informações, consulte [mudanças entre a segurança do IIS6 e do IIS7](https://www.iis.net/learn/get-started/whats-new-in-iis-7/changes-in-security-between-iis-60-and-iis-7-and-above), [a segurança da plataforma Web e a compreensão da](https://www.iis.net/learn/get-started/whats-new-in-iis-7/iis7-and-above-security-improvements)autorização de URL do [IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization).

Longa história, em versões anteriores ao IIS 7, você só pode usar a autenticação de formulários para proteger recursos manipulados pelo tempo de execução do ASP.NET. Da mesma forma, as regras de autorização de URL são aplicadas somente aos recursos manipulados pelo tempo de execução ASP.NET. Mas, com o IIS 7, é possível integrar o FormsAuthenticationModule e o UrlAuthorizationModule ao pipeline HTTP do IIS, estendendo assim essa funcionalidade para todas as solicitações.

## <a name="step-1-creating-an-aspnet-website-for-this-tutorial-series"></a>Etapa 1: Criando um site do ASP.NET para esta série de tutoriais

Para alcançar o público mais amplo possível, o site do ASP.NET que criaremos em toda esta série será criado com a versão gratuita da Microsoft do Visual Studio 2008, o [Visual Web developer 2008](https://www.microsoft.com/express/vwd/). Implementaremos o repositório de usuários do SqlMembershipProvider em um banco de dados [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx) . Se você estiver usando o Visual Studio 2005 ou uma edição diferente do Visual Studio 2008 ou SQL Server, não se preocupe – as etapas serão quase idênticas e qualquer diferença não trivial será indicada.

Antes que possamos configurar a autenticação de formulários, primeiro precisamos de um site ASP.NET. Comece criando um novo site do ASP.NET baseado em sistema de arquivos. Para fazer isso, inicie o Visual Web Developer e, em seguida, vá para o menu arquivo e escolha novo site, exibindo a caixa de diálogo novo site da Web. Escolha o modelo de site do ASP.NET, defina a lista suspensa local para sistema de arquivos, escolha uma pasta para colocar o site e defina o idioma como VB. Isso criará um novo site com uma página ASP.NET. aspx padrão, uma pasta de dados de\_de aplicativo e um arquivo Web. config.

> [!NOTE]
> O Visual Studio dá suporte a dois modos de gerenciamento de projeto: projetos de site e projetos de aplicativos Web. Projetos de site não têm um arquivo de projeto, enquanto projetos de aplicativos Web imitam a arquitetura do projeto no Visual Studio .NET 2002/2003-eles incluem um arquivo de projeto e compilam o código-fonte do projeto em um único assembly, que é colocado na pasta/bin Inicialmente, o Visual Studio 2005 tem suporte apenas para projetos de site, embora o modelo de projeto de aplicativo Web tenha sido reintroduzido com o Service Pack 1; O Visual Studio 2008 oferece ambos os modelos de projeto. No entanto, as edições Visual Web Developer 2005 e 2008 oferecem suporte apenas a projetos de site. Usarei o modelo de projeto de site. Se você estiver usando uma edição não Express e quiser usar o modelo de [projeto de aplicativo Web](https://msdn.microsoft.com/library/aa730880(vs.80).aspx) em vez disso, fique à vontade para fazer isso, mas lembre-se de que pode haver algumas discrepâncias entre o que você vê na tela e as etapas que devem ser executadas em comparação com as capturas de tela mostradas e as instruções fornecidas nesses tutoriais.

[![criar um novo site baseado no sistema de arquivos](an-overview-of-forms-authentication-vb/_static/image5.png)](an-overview-of-forms-authentication-vb/_static/image4.png)

**Figura 02**: criar um novo site baseado no sistema de arquivos ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image6.png))

### <a name="adding-a-master-page"></a>Adicionando uma página mestra

Em seguida, adicione uma nova página mestra ao site no diretório raiz chamado site. master. As [páginas mestras](https://msdn.microsoft.com/library/wtxbf3hh.aspx) permitem que um desenvolvedor de página defina um modelo de todo o site que pode ser aplicado a páginas ASP.net. O principal benefício das páginas mestras é que a aparência geral do site pode ser definida em um único local, facilitando a atualização ou o ajuste do layout do site.

[![adicionar uma página mestra denominada site. Master ao site](an-overview-of-forms-authentication-vb/_static/image8.png)](an-overview-of-forms-authentication-vb/_static/image7.png)

**Figura 03**: adicionar uma página mestra denominada site. Master ao site ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image9.png))

Defina o layout de página de todo o site aqui na página mestra. Você pode usar a modo de exibição de Design e adicionar quaisquer controles de layout ou Web necessários, ou pode adicionar manualmente a marcação à mão na exibição de origem. Defini o layout da minha página mestra para imitar o layout usado na série de tutoriais *[trabalhando com dados na ASP.NET 2,0](../../data-access/index.md)* (consulte a Figura 4). A página mestra usa [folhas de estilo em cascata](http://www.w3schools.com/css/default.asp) para posicionamento e estilos com as configurações de CSS definidas no arquivo Style. css (que está incluído no download associado deste tutorial). Embora não seja possível determinar a partir da marcação mostrada abaixo, as regras de CSS são definidas de modo que a navegação &lt;conteúdo da div&gt;seja absolutamente posicionada para que apareça à esquerda e tenha uma largura fixa de 200 pixels.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample1.aspx)]

Uma página mestra define o layout de página estática e as regiões que podem ser editadas pelas páginas ASP.NET que usam a página mestra. Essas regiões editáveis de conteúdo são indicadas pelo controle ContentPlaceHolder, que pode ser visto dentro do conteúdo &lt;div&gt;. Nossa página mestra tem um único ContentPlaceHolder (MainContent), mas a página mestra pode ter vários ContentPlaceHolders.

Com a marcação inserida acima, alternar para o modo de exibição de Design mostra o layout da página mestra. Todas as páginas ASP.NET que usam essa página mestra terão esse layout uniforme, com a capacidade de especificar a marcação para a região MainContent.

[![a página mestra, quando exibida por meio do modo de exibição de design](an-overview-of-forms-authentication-vb/_static/image11.png)](an-overview-of-forms-authentication-vb/_static/image10.png)

**Figura 04**: a página mestra, quando exibida por meio do modo de exibição de design ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image12.png))

### <a name="creating-content-pages"></a>Criando páginas de conteúdo

Neste ponto, temos uma página default. aspx em nosso site, mas ela não usa a página mestra que acabamos de criar. Embora seja possível manipular a marcação declarativa de uma página da Web para usar uma página mestra, se a página não contiver qualquer conteúdo, ainda é mais fácil apenas excluir a página e adicioná-la novamente ao projeto, especificando a página mestra a ser usada. Portanto, comece excluindo Default. aspx do projeto.

Em seguida, clique com o botão direito do mouse no nome do projeto na Gerenciador de Soluções e escolha Adicionar um novo formulário da Web chamado Default. aspx. Desta vez, marque a caixa de seleção selecionar página mestra e escolha a página mestra site. Master da lista.

[![adicionar uma nova página default. aspx escolhendo para selecionar uma página mestra](an-overview-of-forms-authentication-vb/_static/image14.png)](an-overview-of-forms-authentication-vb/_static/image13.png)

**Figura 05**: adicionar uma nova página default. aspx escolhendo para selecionar uma página mestra ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image15.png))

[![usar a página mestra site. Master](an-overview-of-forms-authentication-vb/_static/image17.png)](an-overview-of-forms-authentication-vb/_static/image16.png)

**Figura 06**: usar a página mestra site. Master ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image18.png))

> [!NOTE]
> Se você estiver usando o modelo de projeto de aplicativo Web, a caixa de diálogo Adicionar novo item não incluirá uma caixa de seleção selecionar página mestra. Em vez disso, você precisa adicionar um item do tipo formulário de conteúdo da Web. Depois de escolher a opção formulário de conteúdo da Web e clicar em Adicionar, o Visual Studio exibirá a mesma caixa de diálogo Selecionar um mestre mostrada na Figura 6.

A nova marcação declarativa da página default. aspx inclui apenas uma diretiva @Page especificando o caminho para o arquivo da página mestra e um controle de conteúdo para o MainContent ContentPlaceHolder da página mestra.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample2.aspx)]

Por enquanto, deixe default. aspx vazio. Voltaremos a ele mais tarde neste tutorial para adicionar conteúdo.

> [!NOTE]
> Nossa página mestra inclui uma seção para um menu ou alguma outra interface de navegação. Criaremos uma interface desse tipo em um tutorial futuro.

## <a name="step-2-enabling-forms-authentication"></a>Etapa 2: Habilitando a autenticação de formulários

Com o site do ASP.NET criado, nossa próxima tarefa é habilitar a autenticação de formulários. A configuração de autenticação do aplicativo é especificada por meio do [elemento de&gt; de autenticação&lt;](https://msdn.microsoft.com/library/532aee0e.aspx) em Web. config. O elemento de&gt; de autenticação &lt;contém um único atributo chamado modo que especifica o modelo de autenticação usado pelo aplicativo. Esse atributo pode ter um dos quatro valores a seguir:

- **Windows** – conforme discutido no tutorial anterior, quando um aplicativo usa a autenticação do Windows, é responsabilidade do servidor Web autenticar o visitante, e isso geralmente é feito por meio da autenticação básica, Digest ou integrada do Windows.
- **Formulários**-os usuários são autenticados por meio de um formulário em uma página da Web.
- **Passport**-os usuários são autenticados usando o Microsoft Passport Network.
- **Nenhum**-nenhum modelo de autenticação é usado; todos os visitantes são anônimos.

Por padrão, os aplicativos ASP.NET usam a autenticação do Windows. Para alterar o tipo de autenticação para autenticação de formulários, precisamos modificar o atributo de modo do elemento de&gt; de autenticação de &lt;para formulários.

Se o seu projeto ainda não contiver um arquivo Web. config, adicione um agora clicando com o botão direito do mouse no nome do projeto na Gerenciador de Soluções, escolhendo Adicionar novo item e, em seguida, adicionando um arquivo de configuração da Web.

[![se o projeto ainda não incluir Web. config, adicione-o agora](an-overview-of-forms-authentication-vb/_static/image20.png)](an-overview-of-forms-authentication-vb/_static/image19.png)

**Figura 07**: se seu projeto ainda não incluir Web. config, adicione-o agora ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image21.png))

Em seguida, localize o elemento de&gt; de autenticação &lt;e atualize-o para usar a autenticação de formulários. Após essa alteração, a marcação do seu arquivo Web. config deve ser semelhante ao seguinte:

[!code-xml[Main](an-overview-of-forms-authentication-vb/samples/sample3.xml)]

> [!NOTE]
> Como o Web. config é um arquivo XML, o uso de maiúsculas e minúsculas é importante. Certifique-se de definir o atributo Mode como Forms, com um F maiúsculo. Se você usar uma maiúsculas e minúsculas diferentes, como formulários, receberá um erro de configuração ao visitar o site por meio de um navegador.

O elemento de&gt; de autenticação &lt;pode, opcionalmente, incluir um formulário de &lt;&gt; elemento filho que contém configurações específicas de autenticação de formulários. Por enquanto, vamos usar apenas as configurações de autenticação de formulários padrão. Exploraremos os &lt;formulários&gt; elemento filho mais detalhadamente no próximo tutorial.

## <a name="step-3-building-the-login-page"></a>Etapa 3: criando a página de logon

Para oferecer suporte à autenticação de formulários, nosso site precisa de uma página de logon. Conforme discutido na seção noções básicas sobre o fluxo de trabalho de autenticação de formulários, o FormsAuthenticationModule redirecionará automaticamente o usuário para a página de logon se tentarem acessar uma página que não está autorizada a exibir. Também há ASP.NET controles da Web que exibirão um link para a página de logon para usuários anônimos. Isso levanta a pergunta, qual é a URL da página de logon?

Por padrão, o sistema de autenticação de formulários espera que a página de logon seja nomeada login. aspx e colocada no diretório raiz do aplicativo Web. Se você quiser usar uma URL de página de logon diferente, poderá fazê-lo especificando-a em Web. config. Veremos como fazer isso no tutorial subsequente.

A página de logon tem três responsabilidades:

1. Forneça uma interface que permita ao visitante inserir suas credenciais.
2. Determine se as credenciais enviadas são válidas.
3. Faça logon no usuário criando o tíquete de autenticação de formulários.

### <a name="creating-the-login-pages-user-interface"></a>Criando a interface do usuário da página de logon

Vamos começar a usar a primeira tarefa. Adicione uma nova página do ASP.NET ao diretório raiz do site chamado login. aspx e associe-a à página mestra do site. master.

[![adicionar uma nova página ASP.NET chamada Login. aspx](an-overview-of-forms-authentication-vb/_static/image23.png)](an-overview-of-forms-authentication-vb/_static/image22.png)

**Figura 08**: adicionar uma nova página ASP.net chamada Login. aspx ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image24.png))

A interface de página de logon típica consiste em duas caixas de Text: uma para o nome do usuário, uma para sua senha e um botão para enviar o formulário. Os sites muitas vezes incluem uma caixa de seleção lembrar-me que, se marcada, persiste o tíquete de autenticação resultante entre reinicializações do navegador.

Adicione dois TextBoxes a login. aspx e defina suas propriedades de ID como nome de usuário e senha, respectivamente. Defina também a propriedade TextMode da senha como password. Em seguida, adicione um controle CheckBox, definindo sua propriedade ID como RememberMe e sua propriedade Text para me lembrar. Depois disso, adicione um botão chamado LoginButton cuja propriedade Text esteja definida como login. E, finalmente, adicione um controle de rótulo da Web e defina sua propriedade ID como InvalidCredentialsMessage, sua propriedade Text para seu nome de usuário ou senha é inválida. Tente novamente., sua propriedade ForeColor como vermelha e sua propriedade Visible como false.

Neste ponto, sua tela deve ser semelhante à captura de tela na Figura 9, e a sintaxe declarativa da sua página deve ser semelhante ao seguinte:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample4.aspx)]

[![a página de logon contém duas caixas de entrada, uma caixa de seleção, um botão e um rótulo](an-overview-of-forms-authentication-vb/_static/image26.png)](an-overview-of-forms-authentication-vb/_static/image25.png)

**Figura 09**: a página de logon contém duas caixas de entrada, uma caixa de seleção, um botão e um rótulo ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image27.png))

Por fim, crie um manipulador de eventos para o evento de clique do LoginButton. No designer, basta clicar duas vezes no controle Button para criar esse manipulador de eventos.

### <a name="determining-if-the-supplied-credentials-are-valid"></a>Determinando se as credenciais fornecidas são válidas

Agora, precisamos implementar a tarefa 2 no manipulador de eventos Click do botão – determinando se as credenciais fornecidas são válidas. Para fazer isso, precisa haver um armazenamento de usuário que contenha todas as credenciais dos usuários para que possamos determinar se as credenciais fornecidas correspondem a todas as credenciais conhecidas.

Antes do ASP.NET 2,0, os desenvolvedores eram responsáveis por implementar suas próprias lojas de usuários e escrever o código para validar as credenciais fornecidas no repositório. A maioria dos desenvolvedores implementaria o armazenamento de usuários em um banco de dados, criando uma tabela chamada usuários com colunas como nome de usuário, senha, email, LastLoginDate e assim por diante. Essa tabela, em seguida, teria um registro por conta de usuário. Verificar as credenciais fornecidas de um usuário envolveria consultar o banco de dados para obter um nome de usuário correspondente e, em seguida, garantir que a senha no banco de dados correspondesse à senha fornecida.

Com o ASP.NET 2,0, os desenvolvedores devem usar um dos provedores de associação para gerenciar o armazenamento do usuário. Nesta série de tutoriais, usaremos o SqlMembershipProvider, que usa um banco de dados SQL Server para o armazenamento do usuário. Ao usar o SqlMembershipProvider, precisamos implementar um esquema de banco de dados específico que inclui as tabelas, exibições e procedimentos armazenados esperados pelo provedor. Vamos examinar como implementar esse esquema no tutorial *[criando o esquema de associação no SQL Server](../membership/creating-the-membership-schema-in-sql-server-vb.md)* . Com o provedor de associação em vigor, validar as credenciais do usuário é tão simples quanto chamar o [método ValidateUser (*username*, *password*)](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx)da [classe Membership](https://msdn.microsoft.com/library/system.web.security.membership.aspx), que retorna um valor booliano que indica se a validade da combinação de *nome de usuário* e *senha* . Vendo como ainda não implementamos o repositório de usuários do SqlMembershipProvider, não podemos usar o método ValidateUser da classe Membership neste momento.

Em vez de levar o tempo para criar nossa própria tabela de banco de dados personalizada de usuários (que estaria obsoleta depois de implementarmos o SqlMembershipProvider), vamos codificar as credenciais válidas na própria página de logon. No manipulador de eventos de clique do LoginButton, adicione o seguinte código:

[!code-vb[Main](an-overview-of-forms-authentication-vb/samples/sample5.vb)]

Como você pode ver, há três contas de usuário válidas: Scott, Jisun e Sam-e todas as três têm a mesma senha (senha). O código percorre os usuários e as matrizes de senhas que procuram uma correspondência válida de nome de usuário e senha. Se o nome de usuário e a senha forem válidos, precisaremos fazer logon do usuário e redirecioná-los para a página apropriada. Se as credenciais forem inválidas, exibiremos o rótulo InvalidCredentialsMessage.

Quando um usuário insere credenciais válidas, mencionei que elas são redirecionadas para a página apropriada. No entanto, qual é a página apropriada? Lembre-se de que quando um usuário visita uma página que não está autorizada a exibir, o FormsAuthenticationModule os redireciona automaticamente para a página de logon. Ao fazer isso, ele inclui a URL solicitada na QueryString por meio do parâmetro ReturnUrl. Ou seja, se um usuário tentou visitar ProtectedPage. aspx e não tiver autorização para fazer isso, o FormsAuthenticationModule o redirecionaria para:

Login.aspx?ReturnUrl=ProtectedPage.aspx

Após fazer logon com êxito, o usuário deve ser Redirecionado de volta para ProtectedPage. aspx. Como alternativa, os usuários podem visitar a página de logon em seu próprio Volition. Nesse caso, depois de fazer logon no usuário, eles devem ser enviados à página default. aspx da pasta raiz.

### <a name="logging-in-the-user"></a>Registro em log no usuário

Supondo que as credenciais fornecidas sejam válidas, precisamos criar um tíquete de autenticação de formulários, registrando assim o usuário no site. A [classe FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthentication.aspx) no [namespace System. Web. Security](https://msdn.microsoft.com/library/system.web.security.aspx) fornece métodos asclassificados para fazer logon e fazer logoff de usuários por meio do sistema de autenticação de formulários. Embora haja vários métodos na classe FormsAuthentication, os três que estamos interessados nesse momento são:

- [GetAuthCookie (*username*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.getauthcookie.aspx) – cria um tíquete de autenticação de formulários para o nome de *usuário*fornecido. Em seguida, esse método cria e retorna um objeto HttpCookie que mantém o conteúdo do tíquete de autenticação. Se *persistCookie* for true, um cookie persistente será criado.
- [SetAuthCookie (*username*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) – chama o método GetAuthCookie (*username*, *persistCookie*) para gerar o cookie de autenticação de formulários. Esse método adiciona o cookie retornado pelo GetAuthCookie à coleção de cookies (supondo que a autenticação de formulários baseada em cookies esteja sendo usada; caso contrário, esse método chama uma classe interna que manipula a lógica de tíquete sem cookie).
- [RedirectFromLoginPage (*username*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.redirectfromloginpage.aspx) -esse método chama SetAuthCookie (*username*, *persistCookie*) e redireciona o usuário para a página apropriada.

GetAuthCookie é útil quando você precisa modificar o tíquete de autenticação antes de gravar o cookie na coleção de cookies. SetAuthCookie será útil se você quiser criar o tíquete de autenticação de formulários e adicioná-lo à coleção de cookies, mas não quiser redirecionar o usuário para a página apropriada. Talvez você queira mantê-los na página de logon ou enviá-los para algumas páginas alternativas.

Como queremos fazer logon no usuário e redirecioná-los para a página apropriada, vamos usar RedirectFromLoginPage. Atualize o manipulador de eventos de clique do LoginButton, substituindo as duas linhas de tarefas comentadas pela seguinte linha de código:

FormsAuthentication.RedirectFromLoginPage(UserName.Text, RememberMe.Checked)

Ao criar o tíquete de autenticação de formulários, usamos a propriedade Text da caixa de texto nome de usuário para o parâmetro *nome de usuário* do tíquete de autenticação de formulários e o estado marcado da caixa de seleção rememberMe para o parâmetro *persistCookie* .

Para testar a página de logon, visite-a em um navegador. Comece inserindo credenciais inválidas, como um nome de usuário de verão e uma senha de errado. Ao clicar no botão login, um postback ocorrerá e o rótulo InvalidCredentialsMessage será exibido.

[![o rótulo InvalidCredentialsMessage é exibido ao inserir credenciais inválidas](an-overview-of-forms-authentication-vb/_static/image29.png)](an-overview-of-forms-authentication-vb/_static/image28.png)

**Figura 10**: o rótulo InvalidCredentialsMessage é exibido ao inserir credenciais inválidas ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image30.png))

Em seguida, insira credenciais válidas e clique no botão logon. Dessa vez, quando o postback ocorrer, um tíquete de autenticação de formulários será criado e você será redirecionado automaticamente de volta para default. aspx. Neste ponto, você fez logon no site, embora não haja nenhuma indicação visual para indicar que você está conectado no momento. Na etapa 4, veremos como determinar programaticamente se um usuário está conectado ou não, além de como identificar o usuário que está visitando a página.

A etapa 5 examina as técnicas para fazer o logoff de um usuário do site.

### <a name="securing-the-login-page"></a>Protegendo a página de logon

Quando o usuário insere suas credenciais e envia o formulário de página de logon, as credenciais-incluindo sua senha são transmitidas pela Internet para o servidor Web em *texto sem formatação*. Isso significa que qualquer hacker que fareja o tráfego de rede pode ver o nome de usuário e a senha. Para evitar isso, é essencial criptografar o tráfego de rede usando [camadas de soquete seguro (SSL)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer). Isso garantirá que as credenciais (bem como a marcação HTML da página inteira) sejam criptografadas desde o momento em que deixam o navegador até que sejam recebidas pelo servidor Web.

A menos que seu site contenha informações confidenciais, você só precisará usar SSL na página de logon e em outras páginas em que a senha do usuário, de outra forma, seria enviada pela rede em texto sem formatação. Você não precisa se preocupar em proteger o tíquete de autenticação de formulários, já que, por padrão, ele é criptografado e assinado digitalmente (para impedir a violação). Uma discussão mais completa sobre segurança de tíquete de autenticação de formulários é apresentada no tutorial a seguir.

> [!NOTE]
> Muitos sites financeiros e médicos são configurados para usar SSL em *todas as* páginas acessíveis a usuários autenticados. Se você estiver criando um site desse tipo, poderá configurar o sistema de autenticação de formulários para que o tíquete de autenticação de formulários seja transmitido apenas por uma conexão segura. Veremos as várias opções de configuração de autenticação de formulários no próximo tutorial, *[configuração de autenticação de formulários e tópicos avançados](../membership/creating-the-membership-schema-in-sql-server-vb.md)* .

## <a name="step-4-detecting-authenticated-visitors-and-determining-their-identity"></a>Etapa 4: detectando visitantes autenticados e determinando sua identidade

Neste ponto, habilitamos a autenticação de formulários e criamos uma página de logon rudimentar, mas ainda temos que examinar como podemos determinar se um usuário é autenticado ou anônimo. Em determinados cenários, talvez você queira exibir dados ou informações diferentes, dependendo se um usuário autenticado ou anônimo está visitando a página. Além disso, muitas vezes precisamos saber a identidade do usuário autenticado.

Vamos aumentar a página default. aspx existente para ilustrar essas técnicas. Em Default. aspx, adicione dois controles de painel, um chamado AuthenticatedMessagePanel e outro chamado AnonymousMessagePanel. Adicione um controle rótulo chamado WelcomeBackMessage no primeiro painel. No segundo painel, adicione um controle de hiperlink, defina sua propriedade Text para fazer logon e sua propriedade NavigateUrl como ~/login.aspx. Neste ponto, a marcação declarativa para default. aspx deve ser semelhante ao seguinte:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample6.aspx)]

Como você já deve ter adivinhado agora, a ideia aqui é exibir apenas o AuthenticatedMessagePanel para os visitantes autenticados e apenas o AnonymousMessagePanel para os visitantes anônimos. Para fazer isso, precisamos definir essas propriedades visíveis de painéis, dependendo se o usuário estiver conectado ou não.

A [Propriedade Request. IsAuthenticated](https://msdn.microsoft.com/library/system.web.httprequest.isauthenticated.aspx) retorna um valor booliano que indica se a solicitação foi autenticada. Insira o código a seguir na página\_carregar o código do manipulador de eventos:

[!code-vb[Main](an-overview-of-forms-authentication-vb/samples/sample7.vb)]

Com esse código em vigor, visite default. aspx por meio de um navegador. Supondo que você ainda tenha feito logon, você verá um link para a página de logon (consulte a Figura 11). Clique nesse link e faça logon no site. Como vimos na etapa 3, depois de inserir suas credenciais, você retornará a default. aspx, mas desta vez a página mostra o bem-vindo de volta! mensagem (consulte a Figura 12).

[![ao visitar anonimamente, um link de logon é exibido](an-overview-of-forms-authentication-vb/_static/image32.png)](an-overview-of-forms-authentication-vb/_static/image31.png)

**Figura 11**: ao visitar anonimamente, um link de logon é exibido ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image33.png))

[![usuários autenticados são mostrados de volta à tela de boas-vindas! Mensagem](an-overview-of-forms-authentication-vb/_static/image35.png)](an-overview-of-forms-authentication-vb/_static/image34.png)

**Figura 12**: os usuários autenticados são mostrados de volta à vontade! Mensagem ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image36.png))

Podemos determinar a identidade do usuário conectado no momento por meio da [propriedade de usuário](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx)do [objeto HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx). O objeto HttpContext representa informações sobre a solicitação atual e é a página inicial para esses objetos ASP.NET comuns como resposta, solicitação e sessão, entre outros. A propriedade User representa o contexto de segurança da solicitação HTTP atual e implementa a [interface IPrincipal](https://msdn.microsoft.com/library/system.security.principal.iprincipal.aspx).

A propriedade User é definida pelo FormsAuthenticationModule. Especificamente, quando o FormsAuthenticationModule encontra um tíquete de autenticação de formulários na solicitação de entrada, ele cria um novo objeto GenericPrincipal e o atribui à propriedade User.

Os objetos principais (como GenericPrincipal) fornecem informações sobre a identidade do usuário e as funções às quais eles pertencem. A interface IPrincipal define dois membros:

- [IsInRole (*roleName*)](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole.aspx) – um método que retorna um valor booliano que indica se a entidade pertence à função especificada.
- [Identity](https://msdn.microsoft.com/library/system.security.principal.iprincipal.identity.aspx) – uma propriedade que retorna um objeto que implementa a [interface IIdentity](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx). A interface IIdentity define três propriedades: [AuthenticationType](https://msdn.microsoft.com/library/system.security.principal.iidentity.authenticationtype.aspx), [IsAuthenticated](https://msdn.microsoft.com/library/system.security.principal.iidentity.isauthenticated.aspx)e [Name](https://msdn.microsoft.com/library/system.security.principal.iidentity.name.aspx).

Podemos determinar o nome do visitante atual usando o seguinte código:

Dim currentUsersName as String = User.Identity.Name

Ao usar a autenticação de formulários, um [objeto FormsIdentity](https://msdn.microsoft.com/library/system.web.security.formsidentity.aspx) é criado para a propriedade Identity de GenericPrincipal. A classe FormsIdentity sempre retorna os formulários de cadeia de caracteres para sua propriedade AuthenticationType e true para sua propriedade IsAuthenticated. A propriedade Name retorna o nome de usuário especificado ao criar o tíquete de autenticação de formulários. Além dessas três propriedades, FormsIdentity inclui acesso ao tíquete de autenticação subjacente por meio de sua [Propriedade ticket](https://msdn.microsoft.com/library/system.web.security.formsidentity.ticket.aspx). A propriedade ticket retorna um objeto do tipo [FormsAuthenticationTicket](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx), que tem propriedades como expiração, IsPersistent, emitido, nome e assim por diante.

O ponto importante a ser resumido aqui é que o parâmetro *username* especificado nos métodos FormsAuthentication. GetAuthCookie (*username*, *PersistCookie*), FormsAuthentication. SetAuthCookie (*username*, *persistCookie*) e FormsAuthentication. RedirectFromLoginPage (*username*, *persistCookie*) é o mesmo valor retornado por User.Identity.Name. Além disso, o tíquete de autenticação criado por esses métodos está disponível com a conversão de User. Identity em um objeto FormsIdentity e, em seguida, o acesso à propriedade ticket:

Dim ident as FormsIdentity = CType (User. Identity, FormsIdentity)

Dim authTicket as FormsAuthenticationTicket = IDENT. Concessão

Vamos fornecer uma mensagem mais personalizada em Default. aspx. Atualize a página\_carregar manipulador de eventos para que a propriedade Text do rótulo WelcomeBackMessage receba a cadeia de caracteres de boas-vindas de volta, *username*!

WelcomeBackMessage. Text = "Welcome Back", &amp; User.Identity.Name &amp; "!"

A Figura 13 mostra o efeito dessa modificação (ao fazer logon como o usuário Scott).

[![a mensagem de boas-vindas inclui o nome do usuário conectado no momento](an-overview-of-forms-authentication-vb/_static/image38.png)](an-overview-of-forms-authentication-vb/_static/image37.png)

**Figura 13**: a mensagem de boas-vindas inclui o nome do usuário conectado no momento ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image39.png))

### <a name="using-the-loginview-and-loginname-controls"></a>Usando os controles LoginView e LoginName

Exibir conteúdo diferente para usuários autenticados e anônimos é um requisito comum; Portanto, está exibindo o nome do usuário conectado no momento. Por esse motivo, o ASP.NET inclui dois controles da Web que fornecem a mesma funcionalidade mostrada na Figura 13, mas sem a necessidade de escrever uma única linha de código.

O [controle LoginView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) é um controle da Web baseado em modelo que torna mais fácil exibir dados diferentes para usuários autenticados e anônimos. O LoginView inclui dois modelos predefinidos:

- AnonymousTemplate-qualquer marcação adicionada a esse modelo é exibida somente para visitantes anônimos.
- LoggedInTemplate-a marcação deste modelo é mostrada somente para usuários autenticados.

Vamos adicionar o controle LoginView à página mestra do nosso site, site. master. Em vez de adicionar apenas o controle LoginView, no entanto, vamos adicionar um novo controle ContentPlaceHolder e, em seguida, colocar o controle LoginView dentro desse novo ContentPlaceHolder. A lógica para essa decisão se tornará aparente em breve.

> [!NOTE]
> Além de AnonymousTemplate e LoggedInTemplate, o controle LoginView pode incluir modelos específicos de função. Modelos específicos de função mostram a marcação somente para os usuários que pertencem a uma função especificada. Examinaremos os recursos baseados em função do controle LoginView em um tutorial futuro.

Comece adicionando um ContentPlaceHolder chamado LoginContent à página mestra dentro do elemento de navegação &lt;div&gt;. Você pode simplesmente arrastar um controle ContentPlaceHolder da caixa de ferramentas para a exibição de código-fonte, colocando o texto de marcação resultante logo acima do TODO: menu aparecerá aqui.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample8.aspx)]

Em seguida, adicione um controle LoginView dentro do LoginContent ContentPlaceHolder. O conteúdo colocado nos controles ContentPlaceHolder da página mestra é considerado *conteúdo padrão* para o ContentPlaceHolder. Ou seja, as páginas do ASP.NET que usam essa página mestra podem especificar seu próprio conteúdo para cada ContentPlaceHolder ou usar o conteúdo padrão da página mestra.

O LoginView e outros controles relacionados ao logon estão localizados na guia login da caixa de ferramentas.

[![o controle LoginView na caixa de ferramentas](an-overview-of-forms-authentication-vb/_static/image41.png)](an-overview-of-forms-authentication-vb/_static/image40.png)

**Figura 14**: o controle LoginView na caixa de ferramentas ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image42.png))

Em seguida, adicione dois &lt;br/&gt; elementos imediatamente após o controle LoginView, mas ainda dentro do ContentPlaceHolder. Neste ponto, a marcação do elemento de&gt; de navegação &lt;div deve ser semelhante ao seguinte:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample9.aspx)]

Os modelos de LoginView podem ser definidos no designer ou na marcação declarativa. No designer do Visual Studio, expanda a marca inteligente do LoginView, que lista os modelos configurados em uma lista suspensa. Digite o texto Hello, estranho no AnonymousTemplate; em seguida, adicione um controle HyperLink e defina suas propriedades Text e NavigateUrl para fazer logon e ~/login.aspx, respectivamente.

Depois de configurar o AnonymousTemplate, alterne para o LoggedInTemplate e insira o texto "bem-vindo de volta". Em seguida, arraste um controle LoginName da caixa de ferramentas para o LoggedInTemplate, colocando-o imediatamente após o texto "bem-vindo de volta". O [controle LoginName](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginname.aspx), como o nome indica, exibe o nome do usuário conectado no momento. Internamente, o controle LoginName simplesmente gera a propriedade User.Identity.Name

Depois de fazer essas adições aos modelos do LoginView, a marcação deve ser semelhante ao seguinte:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample10.aspx)]

Com essa adição à página mestra site. Master, cada página em nosso site exibirá uma mensagem diferente dependendo se o usuário for autenticado. A Figura 15 mostra a página default. aspx quando visitada por meio de um navegador pelo usuário Jisun. A mensagem de boas-vindas, Jisun é repetida duas vezes: uma vez na seção de navegação da página mestra à esquerda (por meio do controle LoginView que acabamos de adicionar) e uma vez na área de conteúdo do default. aspx (por meio de controles Panel e lógica programática).

[![o controle LoginView exibe bem-vindo de volta, Jisun.](an-overview-of-forms-authentication-vb/_static/image44.png)](an-overview-of-forms-authentication-vb/_static/image43.png)

**Figura 15**: o controle LoginView exibe bem-vindo de volta, Jisun. ([Clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image45.png))

Como adicionamos o LoginView à página mestra, ele pode aparecer em cada página em nosso site. No entanto, pode haver páginas da Web em que não queremos mostrar essa mensagem. Uma dessas páginas é a página de logon, já que um link para a página de logon parece estar fora desse local. Como colocamos o controle LoginView em um ContentPlaceHolder na página mestra, podemos substituir essa marcação padrão em nossa página de conteúdo. Abra login. aspx e vá para o designer. Como não definimos explicitamente um controle de conteúdo em Login. aspx para o LoginContent ContentPlaceHolder na página mestra, a página de logon mostrará a marcação padrão da página mestra para este ContentPlaceHolder. Você pode ver isso por meio do designer – o LoginContent ContentPlaceHolder mostra a marcação padrão (o controle LoginView).

[![a página de logon mostra o conteúdo padrão para o LoginContent ContentPlaceHolder da página mestra](an-overview-of-forms-authentication-vb/_static/image47.png)](an-overview-of-forms-authentication-vb/_static/image46.png)

**Figura 16**: a página de logon mostra o conteúdo padrão para o LoginContent ContentPlaceHolder da página mestra ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image48.png))

Para substituir a marcação padrão para o LoginContent ContentPlaceHolder, basta clicar com o botão direito do mouse na região no designer e escolher a opção criar conteúdo personalizado no menu de contexto. (Ao usar o Visual Studio 2008, o ContentPlaceHolder inclui uma marca inteligente que, quando selecionada, oferece a mesma opção.) Isso adiciona um novo controle de conteúdo à marcação da página e, portanto, nos permite definir conteúdo personalizado para esta página. Você pode adicionar uma mensagem personalizada aqui, como faça logon, mas vamos deixar isso em branco.

> [!NOTE]
> No Visual Studio 2005, a criação de conteúdo personalizado cria um controle de conteúdo vazio na página ASP.NET. No Visual Studio 2008, no entanto, a criação de conteúdo personalizado copia o conteúdo padrão da página mestra no controle de conteúdo recém-criado. Se você estiver usando o Visual Studio 2008, depois de criar o novo controle de conteúdo, certifique-se de limpar o conteúdo copiado da página mestra.

A figura 17 mostra a página login. aspx quando visitada em um navegador depois de fazer essa alteração. Observe que não há nenhum Hello, estranho ou bem-vindo, mensagem de *nome de usuário* na navegação à esquerda &lt;div&gt; como há ao visitar default. aspx.

[![a página de logon oculta a marcação padrão do LoginContent ContentPlaceHolder](an-overview-of-forms-authentication-vb/_static/image50.png)](an-overview-of-forms-authentication-vb/_static/image49.png)

**Figura 17**: a página de logon oculta a marcação padrão do LoginContent ContentPlaceHolder ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image51.png))

## <a name="step-5-logging-out"></a>Etapa 5: fazendo logoff

Na etapa 3, examinamos a criação de uma página de logon para fazer logon de um usuário no site, mas ainda assim veremos como fazer logoff de um usuário. Além dos métodos para registrar um usuário no, a classe FormsAuthentication também fornece um [método de saída](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx). O método de saída simplesmente destrói o tíquete de autenticação de formulários, registrando assim o usuário fora do site.

A oferta de um link de logoff é um recurso comum, que ASP.NET inclui um controle especificamente projetado para fazer logoff de um usuário. O [controle LoginStatus](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginstatus.aspx) exibe um LinkButton de logon ou um logoff LinkButton, dependendo do status de autenticação do usuário. Um LinkButton de logon é renderizado para usuários anônimos, enquanto um LinkButton de logoff é exibido para usuários autenticados. O texto para os LinkButtons login e logout pode ser configurado por meio das propriedades LoginText e LogoutText de LoginStatus.

Clicar no logon LinkButton causa um postback, do qual um redirecionamento é emitido para a página de logon. Clicar no logout LinkButton faz com que o controle LoginStatus invoque o método FormsAuthentication. posign e redireciona o usuário para uma página. A página para a qual o usuário desconectado é redirecionado depende da propriedade LogoutAction, que pode ser atribuída a um dos três valores a seguir:

- Atualizar-o padrão; redireciona o usuário para a página que eles estavam visitando. Se a página que ele estava visitando não permitir usuários anônimos, o FormsAuthenticationModule redirecionará automaticamente o usuário para a página de logon.

Você pode estar curioso para saber por que um redirecionamento é executado aqui. Se o usuário quiser permanecer na mesma página, por que a necessidade do redirecionamento explícito? O motivo é que, quando o logoff de logoff é clicado, o usuário ainda tem o tíquete de autenticação de formulários em sua coleção de cookies. Consequentemente, a solicitação de postback é uma solicitação autenticada. O controle LoginStatus chama o método SignOut, mas isso acontece depois que o FormsAuthenticationModule autenticou o usuário. Portanto, um redirecionamento explícito faz com que o navegador solicite novamente a página. No momento em que o navegador solicitar novamente a página, o tíquete de autenticação de formulários foi removido e, portanto, a solicitação de entrada é anônima.

- Redirect – o usuário é redirecionado para a URL especificada pela propriedade LogoutPageUrl de LoginStatus.
- RedirectToLoginPage-o usuário é redirecionado para a página de logon.

Vamos adicionar um controle LoginStatus à página mestra e configurá-lo para usar a opção de redirecionamento para enviar o usuário a uma página que exibe uma mensagem confirmando que eles foram desconectados. Comece criando uma página no diretório raiz chamado logout. aspx. Não se esqueça de associar esta página à página mestra do site. master. Em seguida, insira uma mensagem na marcação da página, explicando o usuário que foi desconectado.

Em seguida, retorne à página mestra site. Master e adicione um controle LoginStatus abaixo do LoginView no LoginContent ContentPlaceHolder. Defina a propriedade LogoutAction do controle LoginStatus para redirecionar e sua propriedade LogoutPageUrl para ~/logout.aspx.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample11.aspx)]

Como o LoginStatus está fora do controle LoginView, ele aparecerá para os usuários anônimos e autenticados, mas isso não ocorrerá porque o LoginStatus exibirá corretamente um logon ou logout LinkButton. Com a adição do controle LoginStatus, o hiperlink logon no AnonymousTemplate é supérfluo, portanto, remova-o.

A Figura 18 mostra default. aspx quando o Jisun visita. Observe que a coluna à esquerda exibe a mensagem, bem-vindo de volta, Jisun junto com um link para fazer logoff. Clicar no LinkButton logoff causa um postback, assina Jisun fora do sistema e, em seguida, redireciona-o para logout. aspx. Como a Figura 19 mostra, no momento em que Jisun acessa logout. aspx, ela já foi desconectada e, portanto, é anônima. Consequentemente, a coluna à esquerda mostra o texto Welcome, estranho e um link para a página de logon.

[![default. aspx mostra Welcome de volta, Jisun juntamente com um LinkButton logoff](an-overview-of-forms-authentication-vb/_static/image53.png)](an-overview-of-forms-authentication-vb/_static/image52.png)

**Figura 18**: default. aspx mostra bem-vindo de volta, Jisun juntamente com um logoff LinkButton ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image54.png))

[![logout. aspx mostra bem-vindo, estranho junto com um logon LinkButton](an-overview-of-forms-authentication-vb/_static/image56.png)](an-overview-of-forms-authentication-vb/_static/image55.png)

**Figura 19**: logout. aspx mostra bem-vindo, estranho junto com um logon LinkButton ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image57.png))

> [!NOTE]
> Recomendo que você personalize a página logout. aspx para ocultar o LoginContent ContentPlaceHolder da página mestra (como fizemos para o login. aspx na etapa 4). O motivo é que o LinkButton de logon processado pelo controle LoginStatus (aquele abaixo de Hello, estranho) envia o usuário para a página de logon passando a URL atual no parâmetro ReturnUrl QueryString. Em suma, se um usuário que fez logoff clicar nesse botão de logon do LoginStatus e fizer logon, ele será Redirecionado de volta para logout. aspx, o que poderia confundir facilmente o usuário.

## <a name="summary"></a>Resumo

Neste tutorial, começamos com um exame do fluxo de trabalho de autenticação de formulários e, em seguida, desativamos a implementação de autenticação de formulários em um aplicativo ASP.NET. A autenticação de formulários é alimentada pelo FormsAuthenticationModule, que tem duas responsabilidades: identificar usuários com base em seu tíquete de autenticação de formulários e redirecionar usuários não autorizados para a página de logon.

A classe FormsAuthentication do .NET Framework inclui métodos para criar, inspecionar e remover tíquetes de autenticação de formulários. A propriedade Request. IsAuthenticated e o objeto user fornecem suporte de programação adicional para determinar se uma solicitação é autenticada e informações sobre a identidade do usuário. Também há os controles da Web LoginView, LoginStatus e LoginName, que oferecem aos desenvolvedores uma maneira rápida e sem código para executar muitas tarefas comuns relacionadas ao logon. Examinaremos esses e outros controles da Web relacionados ao logon com mais detalhes em Tutoriais futuros.

Este tutorial forneceu uma visão geral do curso de autenticação de formulários. Não examinamos as opções de configuração assorted, veja como os tíquetes de autenticação de formulários sem cookie funcionam ou explore como o ASP.NET protege o conteúdo do tíquete de autenticação de formulários. Discutiremos esses tópicos e muito mais no [próximo tutorial](forms-authentication-configuration-and-advanced-topics-vb.md).

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Alterações entre o IIS6 e o IIS7 Security](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [Controles ASP.NET de logon](https://msdn.microsoft.com/library/d51ttbhx.aspx)
- [Gerenciamento de função, associação e segurança do Professional ASP.NET 2,0](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [O elemento de&gt; de autenticação &lt;](https://msdn.microsoft.com/library/532aee0e.aspx)
- [O elemento &lt;Forms&gt; para &lt;autenticação&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Treinamento em vídeo sobre tópicos contidos neste tutorial

- [Uso da autenticação de formulários básica no ASP.NET](../../../videos/authentication/using-basic-forms-authentication-in-aspnet.md)

### <a name="about-the-author"></a>Sobre o autor

Scott Mitchell, autor de vários livros sobre ASP/ASP. NET e fundador da 4GuysFromRolla.com, tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a *[Sams ensina a ASP.NET 2,0 em 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott pode ser contatado em [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) ou por meio de seu blog em [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores potenciais para este tutorial incluem Alicja Maziarz, John Suru e Teresa Murphy. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Anterior](security-basics-and-asp-net-support-vb.md)
> [Próximo](forms-authentication-configuration-and-advanced-topics-vb.md)
