---
uid: web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-cs
title: Validando credenciais de usuário no repositório de usuáriosC#da Associação () | Microsoft Docs
author: rick-anderson
description: Neste tutorial, examinaremos como validar as credenciais de um usuário em relação ao armazenamento de usuário da Associação usando tanto o meio de programação quanto o controle de logon....
ms.author: riande
ms.date: 01/18/2008
ms.assetid: 61aa4e08-aa81-4aeb-8ebe-19ba7a65e04c
msc.legacyurl: /web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-cs
msc.type: authoredcontent
ms.openlocfilehash: aaf6df6f52253ef0f7369a7e77211b6786b97db1
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74618309"
---
# <a name="validating-user-credentials-against-the-membership-user-store-c"></a>Validar credenciais de usuário no repositório de usuário associado (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_06_CS.zip) ou [baixar PDF](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial06_LoggingIn_cs.pdf)

> Neste tutorial, examinaremos como validar as credenciais de um usuário em relação ao armazenamento de usuário da Associação usando tanto o meio de programação quanto o controle de logon. Também veremos como personalizar a aparência e o comportamento do controle de logon.

## <a name="introduction"></a>Introdução

<a id="Tutorial05"> </a>No [tutorial anterior](creating-user-accounts-cs.md) , vimos como criar uma nova conta de usuário na estrutura de associação. Primeiro, examinamos programaticamente a criação de contas de usuário por meio do método `CreateUser` da classe `Membership` e, em seguida, examinamos usando o controle da Web CreateUserWizard. No entanto, a página de logon valida atualmente as credenciais fornecidas em uma lista embutida de pares de nome de usuário e senha. Precisamos atualizar a lógica da página de logon para que ela valide as credenciais no repositório de usuários da estrutura de associação.

Assim como na criação de contas de usuário, as credenciais podem ser validadas de forma programática ou declarativa. A API Membership inclui um método para validar programaticamente as credenciais de um usuário no armazenamento do usuário. E o ASP.NET é fornecido com o controle da Web de logon, que renderiza uma interface do usuário com caixas de para o nome de usuário e a senha e um botão para fazer logon.

Neste tutorial, examinaremos como validar as credenciais de um usuário em relação ao armazenamento de usuário da Associação usando tanto o meio de programação quanto o controle de logon. Também veremos como personalizar a aparência e o comportamento do controle de logon. Vamos começar!

## <a name="step-1-validating-credentials-against-the-membership-user-store"></a>Etapa 1: Validando credenciais no repositório de usuários da Associação

Para sites que usam a autenticação de formulários, um usuário faz logon no site visitando uma página de logon e inserindo suas credenciais. Essas credenciais são comparadas com o armazenamento do usuário. Se forem válidas, o usuário receberá um tíquete de autenticação de formulários, que é um token de segurança que indica a identidade e a autenticidade do visitante.

Para validar um usuário em relação à estrutura de associação, use o [método`ValidateUser`](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx)da classe `Membership`. O método `ValidateUser` usa dois parâmetros de entrada- *`username`* e *`password`* -e retorna um valor booliano que indica se as credenciais foram válidas. Assim como com o método `CreateUser` examinamos no tutorial anterior, o método `ValidateUser` delega a validação real para o provedor de associação configurado.

O `SqlMembershipProvider` valida as credenciais fornecidas obtendo a senha do usuário especificado por meio do procedimento armazenado `aspnet_Membership_GetPasswordWithFormat`. Lembre-se de que o `SqlMembershipProvider` armazena senhas de usuários usando um dos três formatos: Claro, criptografado ou com hash. O procedimento armazenado `aspnet_Membership_GetPasswordWithFormat` retorna a senha em seu formato bruto. Para senhas criptografadas ou com hash, o `SqlMembershipProvider` transforma o valor de *`password`* passado para o método `ValidateUser` em seu estado equivalente criptografado ou hash e, em seguida, compara-o com o que foi retornado do banco de dados. Se a senha armazenada no banco de dados corresponder à senha formatada inserida pelo usuário, as credenciais serão válidas.

Vamos atualizar nossa página de logon (~/`Login.aspx`) para que ela valide as credenciais fornecidas no repositório de usuários da estrutura de associação. Criamos essa página de logon de volta <a id="Tutorial02"> </a>na [*visão geral do tutorial de autenticação de formulários*](../introduction/an-overview-of-forms-authentication-cs.md) , criando uma interface com duas caixas de entrada para o nome de usuário e a senha, uma caixa de seleção lembrar-me e um botão de logon (veja a Figura 1). O código valida as credenciais inseridas em relação a uma lista embutida em código de pares de nome de usuário e senha (Scott/password, Jisun/senha e Sam/password). No tutorial [*configuração de autenticação de formulários e tópicos avançados*](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) , atualizamos o código da página de logon para armazenar informações adicionais na propriedade `UserData` do tíquete de autenticação de formulários. <a id="Tutorial03"> </a>

[![interface da página de logon inclui duas caixas de entrada, um CheckBoxList e um botão](validating-user-credentials-against-the-membership-user-store-cs/_static/image2.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image1.png)

**Figura 1**: a interface da página de logon inclui duas caixas de entrada, um CheckBoxList e um botão ([clique para exibir a imagem em tamanho normal](validating-user-credentials-against-the-membership-user-store-cs/_static/image3.png))

A interface do usuário da página de logon pode permanecer inalterada, mas precisamos substituir o manipulador de eventos `Click` do botão de logon pelo código que valida o usuário em relação ao repositório de usuários da estrutura de associação. Atualize o manipulador de eventos para que seu código seja exibido da seguinte maneira:

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample1.cs)]

Esse código é notavelmente simples. Começamos chamando o método `Membership.ValidateUser`, passando o nome de usuário e a senha fornecidos. Se esse método retornar true, o usuário será conectado ao site por meio do método RedirectFromLoginPage da classe `FormsAuthentication`. (Como discutimos em <a id="Tutorial2"> </a> [*uma visão geral do tutorial de autenticação de formulários*](../introduction/an-overview-of-forms-authentication-cs.md) , o `FormsAuthentication.RedirectFromLoginPage` cria o tíquete de autenticação de formulários e, em seguida, redireciona o usuário para a página apropriada.) Se as credenciais forem inválidas, no entanto, o rótulo de `InvalidCredentialsMessage` será exibido, informando ao usuário que seu nome de usuário ou senha estava incorreto.

E isso é tudo!

Para testar se a página de logon funciona conforme o esperado, tente fazer logon com uma das contas de usuário criadas no tutorial anterior. Ou, se você ainda não tiver criado uma conta, vá em frente e crie uma na página `~/Membership/CreatingUserAccounts.aspx`.

> [!NOTE]
> Quando o usuário insere suas credenciais e envia o formulário da página de logon, as credenciais, incluindo sua senha, são transmitidas pela Internet para o servidor Web em *texto sem formatação*. Isso significa que qualquer hacker que fareja o tráfego de rede pode ver o nome de usuário e a senha. Para evitar isso, é essencial criptografar o tráfego de rede usando [camadas de soquete seguro (SSL)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer). Isso garantirá que as credenciais (bem como a marcação HTML da página inteira) sejam criptografadas desde o momento em que deixam o navegador até que sejam recebidas pelo servidor Web.

### <a name="how-the-membership-framework-handles-invalid-login-attempts"></a>Como a estrutura de associação lida com tentativas de logon inválidas

Quando um visitante atinge a página de logon e envia suas credenciais, o navegador faz uma solicitação HTTP para a página de logon. Se as credenciais forem válidas, a resposta HTTP incluirá o tíquete de autenticação em um cookie. Portanto, um hacker tentando invadir seu site pode criar um programa que envia exaustivamente solicitações HTTP para a página de logon com um nome de usuário válido e uma estimativa da senha. Se a estimativa de senha estiver correta, a página de logon retornará o cookie de tíquete de autenticação, ponto em que o programa sabe que ele se deparou com um par válido de nome de usuário/senha. Por meio da força bruta, um programa como esse pode ser capaz de se deparar com a senha de um usuário, especialmente se a senha for fraca.

Para evitar esses ataques de força bruta, a estrutura de associação bloqueia um usuário se houver um determinado número de tentativas de logon malsucedidas em um determinado período de tempo. Os parâmetros exatos são configuráveis por meio das duas definições de configuração do provedor de associação a seguir:

- `maxInvalidPasswordAttempts`-especifica quantas tentativas de senha inválidas são permitidas para o usuário dentro do período de tempo antes que a conta seja bloqueada. O valor padrão é 5.
- `passwordAttemptWindow`-indica o período de tempo em minutos durante o qual o número especificado de tentativas de logon inválidas fará com que a conta seja bloqueada. O valor padrão é 10.

Se um usuário tiver sido bloqueado, ele não poderá fazer logon até que um administrador desbloqueie sua conta. Quando um usuário é bloqueado, o método `ValidateUser` *sempre* retornará `false`, mesmo que credenciais válidas sejam fornecidas. Embora esse comportamento reduza a probabilidade de que um hacker se divida em seu site por meio de métodos de força bruta, ele pode acabar bloqueando um usuário válido que simplesmente esqueceu sua senha ou acidentalmente tem o bloqueio de Caps ativado ou está tendo um dia de digitação inadequado.

Infelizmente, não há nenhuma ferramenta interna para desbloquear uma conta de usuário. Para desbloquear uma conta, você pode modificar o banco de dados diretamente-altere o campo `IsLockedOut` na tabela `aspnet_Membership` para a conta de usuário apropriada-ou crie uma interface baseada na Web que liste contas bloqueadas com opções para desbloqueá-las. Examinaremos a criação de interfaces administrativas para realizar tarefas comuns relacionadas a contas de usuário e funções em um tutorial futuro.

> [!NOTE]
> Uma desvantagem do método `ValidateUser` é que, quando as credenciais fornecidas são inválidas, ele não fornece nenhuma explicação sobre o porquê. As credenciais podem ser inválidas porque não há nenhum par de nome de usuário/senha correspondente no repositório de usuários ou porque o usuário ainda não foi aprovado ou porque o usuário foi bloqueado. Na etapa 4, veremos como mostrar uma mensagem mais detalhada ao usuário quando a tentativa de logon falhar.

## <a name="step-2-collecting-credentials-through-the-login-web-control"></a>Etapa 2: coletando credenciais por meio do controle de logon na Web

O [controle da Web de logon](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx) renderiza uma interface do usuário padrão muito semelhante à que criamos de volta <a id="SKM5"> </a>no tutorial [*visão geral da autenticação de formulários*](../introduction/an-overview-of-forms-authentication-cs.md) . O uso do controle login nos poupa o trabalho de ter que criar a interface para coletar as credenciais do visitante. Além disso, o controle de logon entra automaticamente no usuário (supondo que as credenciais enviadas sejam válidas), poupando-nos de ter que escrever qualquer código.

Vamos atualizar `Login.aspx`, substituindo a interface criada manualmente e o código por um controle de logon. Comece removendo a marcação e o código existentes em `Login.aspx`. Você pode excluí-lo imediatamente ou simplesmente comentar. Para comentar a marcação declarativa, coloque-a com os delimitadores `<%--` e `--%>`. Você pode inserir esses delimitadores manualmente ou, como mostra a Figura 2, você pode selecionar o texto para comentar e, em seguida, clicar no ícone comentar as linhas selecionadas na barra de ferramentas. Da mesma forma, você pode usar o ícone comentar as linhas selecionadas para comentar o código selecionado na classe code-behind.

[![comentar a marcação declarativa e o código-fonte existentes em Login. aspx](validating-user-credentials-against-the-membership-user-store-cs/_static/image5.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image4.png)

**Figura 2**: comentar a marcação declarativa e o código-fonte existentes no `Login.aspx` ([clique para exibir a imagem em tamanho normal](validating-user-credentials-against-the-membership-user-store-cs/_static/image6.png))

> [!NOTE]
> O comentário sobre o ícone linhas selecionadas não está disponível ao exibir a marcação declarativa no Visual Studio 2005. Se você não estiver usando o Visual Studio 2008, precisará adicionar manualmente os delimitadores `<%--` e `--%>`.

Em seguida, arraste um controle de logon da caixa de ferramentas para a página e defina sua propriedade `ID` como `myLogin`. Neste ponto, sua tela deve ser semelhante à figura 3. Observe que a interface padrão do controle de logon inclui controles de caixa de texto para o nome de usuário e a senha, uma caixa de seleção lembrar-me da próxima vez e um botão fazer logon. Também há `RequiredFieldValidator` controles para as duas caixas de Text.

[![adicionar um controle de logon à página](validating-user-credentials-against-the-membership-user-store-cs/_static/image8.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image7.png)

**Figura 3**: adicionar um controle de logon à página ([clique para exibir a imagem em tamanho normal](validating-user-credentials-against-the-membership-user-store-cs/_static/image9.png))

E pronto! Quando o botão logon do controle de logon é clicado, ocorre um postback e o controle de logon chamará o método `Membership.ValidateUser`, passando o nome de usuário e a senha inseridos. Se as credenciais forem inválidas, o controle de logon exibirá uma mensagem informando tal. No entanto, se as credenciais forem válidas, o controle de logon criará o tíquete de autenticação de formulários e redirecionará o usuário para a página apropriada.

O controle de logon usa quatro fatores para determinar a página apropriada para redirecionar o usuário ao após um logon bem-sucedido:

- Se o controle de logon está na página de logon, conforme definido pela configuração de `loginUrl` na configuração de autenticação de formulários; o valor padrão desta configuração é `Login.aspx`
- A presença de um parâmetro `ReturnUrl` QueryString
- O valor da [propriedade`DestinationUrl`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.destinationpageurl.aspx) do controle de logon
- O valor de `defaultUrl` especificado nos parâmetros de configuração de autenticação de formulários; o valor padrão desta configuração é `Default.aspx`

A Figura 4 descreve como o controle de logon usa esses quatro parâmetros para chegar à sua decisão de página apropriada.

[![adicionar um controle de logon à página](validating-user-credentials-against-the-membership-user-store-cs/_static/image11.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image10.png)

**Figura 4**: adicionar um controle de logon à página ([clique para exibir a imagem em tamanho normal](validating-user-credentials-against-the-membership-user-store-cs/_static/image12.png))

Reserve um tempo para testar o controle de logon visitando o site por meio de um navegador e fazendo logon como um usuário existente na estrutura de associação.

A interface renderizada do controle de logon é altamente configurável. Há várias propriedades que influenciam sua aparência; Além do mais, o controle login pode ser convertido em um modelo para ter um controle preciso sobre o layout dos elementos da interface do usuário. O restante desta etapa examina como personalizar a aparência e o layout.

### <a name="customizing-the-login-controls-appearance"></a>Personalizando a aparência do controle de logon

As configurações de propriedade padrão do controle de logon processam uma interface do usuário com um título (logon), caixa de texto e controles de rótulo para as entradas de nome de usuário e senha, uma caixa de seleção lembrar-me da próxima vez e um botão fazer logon. As aparências desses elementos são todas configuráveis por meio das várias propriedades do controle de logon. Além disso, elementos adicionais da interface do usuário, como um link para uma página para criar uma nova conta de usuário, podem ser adicionados definindo uma propriedade ou dois.

Vamos gastar alguns minutos para enfeitarr a aparência do nosso controle de logon. Como a página de `Login.aspx` já tem texto na parte superior da página que diz logon, o título do controle de logon é supérfluo. Portanto, desmarque o valor da [propriedade`TitleText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.titletext.aspx) para remover o título do controle de logon.

O nome de usuário: e a senha: rótulos à esquerda dos dois controles TextBox podem ser personalizados por meio das propriedades [`UserNameLabelText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.usernamelabeltext.aspx) e [`PasswordLabelText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordlabeltext.aspx), respectivamente. Vamos alterar o nome de usuário: Label para ler username:. Os estilos rótulo e caixa de texto são configuráveis por meio das propriedades [`LabelStyle`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.labelstyle.aspx) e [`TextBoxStyle`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textboxstyle.aspx), respectivamente.

A propriedade Text da caixa de seleção lembrar da próxima vez pode ser definida por meio do [`RememberMeText property`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermetext.aspx)do controle de logon, e seu estado de verificação padrão é configurável por meio da [`RememberMeSet property`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermeset.aspx) (que usa como padrão false). Vá em frente e defina a propriedade `RememberMeSet` como true para que a caixa de seleção lembrar-me da próxima vez seja marcada por padrão.

O controle de logon oferece duas propriedades para ajustar o layout de seus controles de interface do usuário. O [`TextLayout property`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textlayout.aspx) indica se os rótulos username: e Password: são exibidos à esquerda de suas caixas de textcorrespondentes (o padrão) ou acima deles. O [`Orientation property`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.orientation.aspx) indica se as entradas de nome de usuário e senha estão situadas verticalmente (uma acima da outra) ou horizontalmente. Vou deixar essas duas propriedades definidas com seus padrões, mas recomendo que você tente definir essas duas propriedades com seus valores não padrão para ver o efeito resultante.

> [!NOTE]
> Na próxima seção, configurando o layout do controle de logon, veremos como usar modelos para definir o layout preciso dos elementos da interface do usuário do controle de layout.

Empacote as configurações de Propriedade do controle de logon definindo as propriedades [`CreateUserText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createusertext.aspx) e [`CreateUserUrl`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createuserurl.aspx) como ainda não registradas? Crie uma conta! e `~/Membership/CreatingUserAccounts.aspx`, respectivamente. Isso adiciona um hiperlink à interface do controle de logon apontando para a página que criamos no <a id="SKM6"> </a> [tutorial anterior](creating-user-accounts-cs.md). As [propriedades](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppageurl.aspx) [`HelpPageText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppagetext.aspx) e`HelpPageUrl` do controle de logon e as [Propriedades](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoveryurl.aspx) [`PasswordRecoveryText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoverytext.aspx) e`PasswordRecoveryUrl` funcionam da mesma maneira, processando links para uma página de ajuda e uma página de recuperação de senha.

Depois de fazer essas alterações de propriedade, a marcação declarativa e a aparência do controle de logon devem ser semelhantes àquelas mostradas na Figura 5.

[![os valores das propriedades do controle de logon determinam sua aparência](validating-user-credentials-against-the-membership-user-store-cs/_static/image14.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image13.png)

**Figura 5**: os valores das propriedades do controle de logon ditam sua aparência ([clique para exibir a imagem em tamanho normal](validating-user-credentials-against-the-membership-user-store-cs/_static/image15.png))

### <a name="configuring-the-login-controls-layout"></a>Configurando o layout do controle de logon

A interface do usuário padrão do controle da Web de logon apresenta a interface em um `<table>`HTML. Mas e se precisar de um controle mais preciso sobre a saída renderizada? Talvez queiramos substituir o `<table>` por uma série de marcas de `<div>`. Ou e se nosso aplicativo exigir credenciais adicionais para autenticação? Muitos sites financeiros, por exemplo, exigem que os usuários forneçam não apenas um nome de usuário e uma senha, mas também um PIN (número de identificação pessoal) ou outras informações de identificação. Seja qual for o motivo, é possível converter o controle de logon em um modelo, a partir do qual podemos definir explicitamente a marcação declarativa da interface.

Precisamos fazer duas coisas para atualizar o controle de logon para coletar credenciais adicionais:

1. Atualize a interface do controle de logon para incluir controles da Web para coletar as credenciais adicionais.
2. Substitua a lógica de autenticação interna do controle de logon para que um usuário só seja autenticado se seu nome de usuário e senha forem válidos e suas credenciais adicionais também forem válidas.

Para realizar a primeira tarefa, precisamos converter o controle de logon em um modelo e adicionar os controles da Web necessários. Assim como na segunda tarefa, a lógica de autenticação do controle de logon pode ser substituída pela criação de um manipulador de eventos para o [evento de`Authenticate`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx)do controle.

Vamos atualizar o controle de logon para que ele solicite aos usuários o nome de usuário, a senha e o endereço de email e só autentica o usuário se o endereço de email fornecido corresponder ao seu endereço de email no arquivo. Primeiro, precisamos converter a interface do controle de logon em um modelo. Na marca inteligente do controle de logon, escolha a opção converter em modelo.

[![converter o controle de logon em um modelo](validating-user-credentials-against-the-membership-user-store-cs/_static/image17.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image16.png)

**Figura 6**: converter o controle de logon em um modelo ([clique para exibir a imagem em tamanho normal](validating-user-credentials-against-the-membership-user-store-cs/_static/image18.png))

> [!NOTE]
> Para reverter o controle de logon para sua versão anterior ao modelo, clique no link redefinir da marca inteligente do controle.

Converter o controle de logon em um modelo adiciona uma `LayoutTemplate` à marcação declarativa do controle com elementos HTML e controles da Web definindo a interface do usuário. Como mostra a Figura 7, a conversão do controle em um modelo remove várias propriedades da janela Propriedades, como `TitleText`, `CreateUserUrl`e assim por diante, já que esses valores de propriedade são ignorados ao usar um modelo.

[![menos propriedades estão disponíveis quando o controle de logon é convertido em um modelo](validating-user-credentials-against-the-membership-user-store-cs/_static/image20.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image19.png)

**Figura 7**: menos propriedades estão disponíveis quando o controle de logon é convertido em um modelo ([clique para exibir a imagem em tamanho normal](validating-user-credentials-against-the-membership-user-store-cs/_static/image21.png))

A marcação HTML no `LayoutTemplate` pode ser modificada conforme necessário. Da mesma forma, sinta-se à vontade para adicionar novos controles da Web ao modelo. No entanto, é importante que os principais controles da Web do controle de logon permaneçam no modelo e mantenha seus valores de `ID` atribuídos. Em particular, não remova nem renomeie as caixas de `Password` `UserName` ou, a caixa de seleção `RememberMe`, o botão `LoginButton`, o rótulo `FailureText` ou os controles `RequiredFieldValidator`.

Para coletar o endereço de email do visitante, precisamos adicionar uma caixa de texto ao modelo. Adicione a seguinte marcação declarativa entre a linha da tabela (`<tr>`) que contém a caixa de texto `Password` e a linha da tabela que contém a caixa de seleção lembrar-me da próxima vez:

[!code-aspx[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample2.aspx)]

Depois de adicionar a caixa de texto `Email`, visite a página por meio de um navegador. Como mostra a Figura 8, a interface do usuário do controle de logon agora inclui uma terceira caixa de texto.

[![o controle de logon agora inclui uma caixa de texto para o endereço de email do usuário](validating-user-credentials-against-the-membership-user-store-cs/_static/image23.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image22.png)

**Figura 8**: o controle de logon agora inclui uma caixa de texto para o endereço de email do usuário ([clique para exibir a imagem em tamanho normal](validating-user-credentials-against-the-membership-user-store-cs/_static/image24.png))

Neste ponto, o controle de logon ainda está usando o método `Membership.ValidateUser` para validar as credenciais fornecidas. De forma correspondente, o valor inserido na caixa de texto `Email` não tem nenhuma influência sobre se o usuário pode fazer logon. Na etapa 3, veremos como substituir a lógica de autenticação do controle de logon para que as credenciais sejam consideradas válidas apenas se o nome de usuário e a senha forem válidos e o endereço de email fornecido corresponder ao endereço de email no arquivo.

## <a name="step-3-modifying-the-login-controls-authentication-logic"></a>Etapa 3: modificando a lógica de autenticação do controle de logon

Quando um visitante fornece suas credenciais e clica no botão fazer logon, um postback massacre e o controle de logon progride por meio de seu fluxo de trabalho de autenticação. O fluxo de trabalho começa com a geração do [evento`LoggingIn`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggingin.aspx). Todos os manipuladores de eventos associados a esse evento podem cancelar a operação de logon definindo a propriedade `e.Cancel` como `true`.

Se a operação de logon não for cancelada, o fluxo de trabalho progride gerando o [evento`Authenticate`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx). Se houver um manipulador de eventos para o evento `Authenticate`, ele será responsável por determinar se as credenciais fornecidas são válidas ou não. Se nenhum manipulador de eventos for especificado, o controle de logon usará o método `Membership.ValidateUser` para determinar a validade das credenciais.

Se as credenciais fornecidas forem válidas, o tíquete de autenticação de formulários será criado, o [`LoggedIn` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggedin.aspx) será gerado e o usuário será redirecionado para a página apropriada. No entanto, se as credenciais forem consideradas inválidas, o [evento`LoginError`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loginerror.aspx) será gerado e uma mensagem será exibida informando ao usuário que suas credenciais eram inválidas. Por padrão, em caso de falha, o controle de logon simplesmente define sua propriedade Text do controle rótulo de `FailureText` como uma mensagem de falha (a tentativa de logon não foi bem-sucedida. Tente novamente). No entanto, se a [propriedade de`FailureAction`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failureaction.aspx) do controle de logon for definida como `RedirectToLoginPage`, o controle de logon emitirá um `Response.Redirect` à página de logon acrescentando o parâmetro QueryString `loginfailure=1` (que faz com que o controle de logon exiba a mensagem de falha).

A Figura 9 oferece um gráfico de fluxo do fluxo de trabalho de autenticação.

[![o fluxo de trabalho de autenticação do controle de logon](validating-user-credentials-against-the-membership-user-store-cs/_static/image26.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image25.png)

**Figura 9**: fluxo de trabalho de autenticação do controle de logon ([clique para exibir a imagem em tamanho normal](validating-user-credentials-against-the-membership-user-store-cs/_static/image27.png))

> [!NOTE]
> Se você estiver imaginando quando usar a opção de página de `RedirectToLogin` do `FailureAction`, considere o cenário a seguir. Agora, nossa página mestra de `Site.master` atualmente tem o texto, Olá, estranho exibido na coluna à esquerda quando visitado por um usuário anônimo, mas imagine que queríamos substituir esse texto por um controle de logon. Isso permitiria que um usuário anônimo efetuasse logon de qualquer página no site, em vez de exigir que eles visitassem a página de logon diretamente. No entanto, se um usuário não conseguiu fazer logon por meio do controle de logon processado pela página mestra, pode fazer sentido redirecioná-los para a página de logon (`Login.aspx`) porque essa página provavelmente inclui instruções adicionais, links e outras ajuda-como links para criar uma nova conta ou recuperar uma senha perdida-que não foram adicionadas à página mestra.

### <a name="creating-theauthenticateevent-handler"></a>Criando o manipulador de eventos`Authenticate`

Para conectar nossa lógica de autenticação personalizada, precisamos criar um manipulador de eventos para o evento de `Authenticate` do controle de logon. Criar um manipulador de eventos para o evento de `Authenticate` gerará a seguinte definição de manipulador de eventos:

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample3.cs)]

Como você pode ver, o manipulador de eventos `Authenticate` é passado a um objeto do tipo [`AuthenticateEventArgs`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.authenticateeventargs.aspx) como seu segundo parâmetro de entrada. A classe `AuthenticateEventArgs` contém uma propriedade booliana chamada `Authenticated` que é usada para especificar se as credenciais fornecidas são válidas. Nossa tarefa, então, é escrever código aqui que determina se as credenciais fornecidas são válidas ou não, e para definir a propriedade `e.Authenticate` de acordo.

### <a name="determining-and-validating-the-supplied-credentials"></a>Determinando e validando as credenciais fornecidas

Use as propriedades [`UserName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.username.aspx) e [`Password`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.password.aspx) do controle de logon para determinar as credenciais de nome de usuário e senha inseridas pelo usuário. Para determinar os valores inseridos em qualquer outro controle da Web (como a caixa de texto `Email` que adicionamos na etapa anterior), use *`LoginControlID`* `.FindControl`(" *`controlID`* ") para obter uma referência programática para o controle da Web no modelo cuja propriedade `ID` é igual a *`controlID`* . Por exemplo, para obter uma referência à caixa de texto `Email`, use o seguinte código:

`TextBox EmailTextBox = myLogin.FindControl("Email") as TextBox;`

Para validar as credenciais do usuário, precisamos fazer duas coisas:

1. Verifique se o nome de usuário e a senha fornecidos são válidos
2. Verifique se o endereço de email inserido corresponde ao endereço de email no arquivo para o usuário tentar fazer logon

Para realizar a primeira verificação, podemos simplesmente usar o método `Membership.ValidateUser` como vimos na etapa 1. Para a segunda verificação, precisamos determinar o endereço de email do usuário para que possamos compará-lo com o endereço de email inserido no controle TextBox. Para obter informações sobre um usuário específico, use o [método`GetUser`](https://msdn.microsoft.com/library/system.web.security.membership.getuser.aspx)da classe `Membership`.

O método `GetUser` tem várias sobrecargas. Se usado sem passar nenhum parâmetro, ele retorna informações sobre o usuário conectado no momento. Para obter informações sobre um usuário específico, chame `GetUser` passando o nome de seu usuário. De qualquer forma, `GetUser` retorna um [objeto`MembershipUser`](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx), que tem propriedades como `UserName`, `Email`, `IsApproved`, `IsOnline`e assim por diante.

O código a seguir implementa essas duas verificações. Se ambos forem aprovados, `e.Authenticate` será definido como `true`, caso contrário, ele será atribuído `false`.

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample4.cs)]

Com esse código em vigor, tente fazer logon como um usuário válido, inserindo o nome de usuário, a senha e o endereço de email corretos. Tente novamente, mas esse tempo usa propositadamente um endereço de email incorreto (veja a Figura 10). Por fim, experimente uma terceira vez usando um nome de usuário não existente. No primeiro caso, você deve ter feito logon com êxito no site, mas nos últimos dois casos, você deve ver a mensagem de credenciais inválidas do controle de logon.

[![Tito não pode fazer logon ao fornecer um endereço de email incorreto](validating-user-credentials-against-the-membership-user-store-cs/_static/image29.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image28.png)

**Figura 10**: o Tito não pode fazer logon ao fornecer um endereço de email incorreto ([clique para exibir a imagem em tamanho normal](validating-user-credentials-against-the-membership-user-store-cs/_static/image30.png))

> [!NOTE]
> Conforme discutido na seção como a estrutura de associação manipula tentativas de logon inválidas na etapa 1, quando o método de `Membership.ValidateUser` é chamado e recebe credenciais inválidas, ele controla a tentativa de logon inválida e bloqueia o usuário se eles excederem um determinado limite de tentativas inválidas em uma janela de tempo especificada. Como nossa lógica de autenticação personalizada chama o método `ValidateUser`, uma senha incorreta para um nome de usuário válido incrementará o contador de tentativas de logon inválido, mas esse contador não será incrementado no caso em que o nome de usuário e a senha forem válidos, mas o endereço de email estiver incorreto. É provável que esse comportamento seja adequado, pois é improvável que um hacker saiba o nome de usuário e a senha, mas precise usar técnicas de força bruta para determinar o endereço de email do usuário.

## <a name="step-4-improving-the-login-controls-invalid-credentials-message"></a>Etapa 4: aprimorando a mensagem de credenciais inválidas do controle de logon

Quando um usuário tenta fazer logon com credenciais inválidas, o controle de logon exibe uma mensagem explicando que a tentativa de logon não foi bem-sucedida. Em particular, o controle exibe a mensagem especificada por sua [propriedade`FailureText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failuretext.aspx), que tem um valor padrão de sua tentativa de logon não foi bem-sucedida. Tente novamente.

Lembre-se de que há muitas razões pelas quais as credenciais de um usuário podem ser inválidas:

- O nome de usuário pode não existir
- O nome de usuário existe, mas a senha é inválida
- O nome de usuário e a senha são válidos, mas o usuário ainda não foi aprovado
- O nome de usuário e a senha são válidos, mas o usuário está bloqueado (provavelmente porque excedeu o número de tentativas de logon inválidas dentro do período de tempo especificado)

E pode haver outros motivos ao usar a lógica de autenticação personalizada. Por exemplo, com o código que escrevemos na etapa 3, o nome de usuário e a senha podem ser válidos, mas o endereço de email pode estar incorreto.

Independentemente de por que as credenciais são inválidas, o controle de logon exibe a mesma mensagem de erro. Essa falta de comentários pode ser confusa para um usuário cuja conta ainda não tenha sido aprovada ou que tenha sido bloqueada. Com um pouco de trabalho, no entanto, podemos fazer com que o controle de logon exiba uma mensagem mais apropriada.

Sempre que um usuário tenta fazer logon com credenciais inválidas, o controle de logon gera seu evento de `LoginError`. Vá em frente e crie um manipulador de eventos para esse evento e adicione o seguinte código:

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample5.cs)]

O código acima começa definindo a propriedade de `FailureText` do controle de logon como o valor padrão (sua tentativa de logon não foi bem-sucedida. Tente novamente). Em seguida, ele verifica se o nome de usuário fornecido é mapeado para uma conta de usuário existente. Nesse caso, ele consulta as propriedades `IsLockedOut` e `IsApproved` do objeto resultante `MembershipUser` para determinar se a conta foi bloqueada ou ainda não foi aprovada. Em ambos os casos, a propriedade `FailureText` é atualizada para um valor correspondente.

Para testar esse código, tente fazer logon propositadamente como um usuário existente, mas use uma senha incorreta. Faça isso cinco vezes em uma linha em um período de 10 minutos e a conta será bloqueada. Como mostra a Figura 11, as tentativas de logon subsequentes sempre falharão (mesmo com a senha correta), mas agora exibirão mais descritiva que sua conta foi bloqueada devido a muitas tentativas de logon inválidas. Entre em contato com o administrador para que a mensagem da sua conta seja desbloqueada.

[![Tito realizou muitas tentativas de logon inválidas e foi bloqueada](validating-user-credentials-against-the-membership-user-store-cs/_static/image32.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image31.png)

**Figura 11**: Tito realizou muitas tentativas de logon inválidas e foi bloqueada ([clique para exibir a imagem em tamanho normal](validating-user-credentials-against-the-membership-user-store-cs/_static/image33.png))

## <a name="summary"></a>Resumo

Antes deste tutorial, nossa página de logon validou as credenciais fornecidas em relação a uma lista embutida em código de pares de nome de usuário/senha. Neste tutorial, atualizamos a página para validar credenciais em relação à estrutura de associação. Na etapa 1, examinamos o uso do método `Membership.ValidateUser` programaticamente. Na etapa 2, substituímos nossa interface do usuário criada manualmente e o código pelo controle de logon.

O controle de logon renderiza uma interface do usuário de logon padrão e valida automaticamente as credenciais do usuário em relação à estrutura de associação. Além disso, no caso de credenciais válidas, o controle de logon conecta o usuário por meio da autenticação de formulários. Em suma, uma experiência de usuário de logon totalmente funcional está disponível simplesmente arrastando o controle login para uma página, sem marcação declarativa ou código adicional necessário. Além do mais, o controle de logon é altamente personalizável, permitindo um grau de controle adequado sobre a interface do usuário renderizada e a lógica de autenticação.

Neste ponto, os visitantes do nosso site podem criar uma nova conta de usuário e fazer logon no site, mas ainda precisamos examinar a restrição do acesso às páginas com base no usuário autenticado. Atualmente, qualquer usuário, autenticado ou anônimo, pode exibir qualquer página em nosso site. Além de controlar o acesso às páginas de nosso site por usuário, poderemos ter determinadas páginas cuja funcionalidade depende do usuário. O próximo tutorial analisa como limitar o acesso e a funcionalidade na página com base no usuário conectado.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Exibindo mensagens personalizadas para usuários bloqueados e não aprovados](http://aspnet.4guysfromrolla.com/articles/050306-1.aspx)
- [Examinando a associação, as funções e o perfil do ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Como: criar uma página de logon do ASP.NET](https://msdn.microsoft.com/library/ms178331.aspx)
- [Documentação técnica do controle de logon](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx)
- [Usando os controles de logon](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/login.aspx)

### <a name="about-the-author"></a>Sobre o autor

Scott Mitchell, autor de vários livros sobre ASP/ASP. NET e fundador da 4GuysFromRolla.com, tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a *[Sams ensina a ASP.NET 2,0 em 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott pode ser contatado em [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) ou por meio de seu blog em [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores potenciais para este tutorial foram Teresa Murphy e Michael Olivero. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Anterior](creating-user-accounts-cs.md)
> [Próximo](user-based-authorization-cs.md)
