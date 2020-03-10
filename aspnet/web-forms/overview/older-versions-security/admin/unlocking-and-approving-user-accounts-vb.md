---
uid: web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-vb
title: Desbloqueando e aprovando contas de usuário (VB) | Microsoft Docs
author: rick-anderson
description: Este tutorial mostra como criar uma página da Web para os administradores gerenciarem os status bloqueados e aprovados dos usuários. Também veremos como aprovar novos usuários o...
ms.author: riande
ms.date: 04/01/2008
ms.assetid: 041854a5-ea8c-4de0-82f1-121ba6cb2893
msc.legacyurl: /web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-vb
msc.type: authoredcontent
ms.openlocfilehash: 4a7474676b8f502c583e226678de2b275e0ea3c7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78637518"
---
# <a name="unlocking-and-approving-user-accounts-vb"></a>Desbloqueio e aprovação de contas de usuário (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/VB.14.zip) ou [baixar PDF](https://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial14_UnlockAndApprove_vb.pdf)

> Este tutorial mostra como criar uma página da Web para os administradores gerenciarem os status bloqueados e aprovados dos usuários. Também veremos como aprovar novos usuários somente depois de verificarem seu endereço de email.

## <a name="introduction"></a>Introdução

Junto com um nome de usuário, senha e email, cada conta de usuário tem dois campos de status que determinam se o usuário pode fazer logon no site: bloqueado e aprovado. Um usuário será bloqueado automaticamente se fornecer credenciais inválidas um número especificado de vezes dentro de um determinado número de minutos (as configurações padrão bloqueiam o usuário após 5 tentativas de logon inválidas em 10 minutos). O status aprovado é útil em cenários em que alguma ação deve ser transacionada antes que um novo usuário possa fazer logon no site. Por exemplo, um usuário pode precisar primeiro verificar seu endereço de email ou ser aprovado por um administrador antes de poder fazer logon.

Como um usuário bloqueado ou não aprovado não pode fazer logon, é natural imaginar como esses status podem ser redefinidos. O ASP.NET não inclui nenhuma funcionalidade interna ou controles da Web para gerenciar status bloqueados e aprovados pelos usuários, em parte porque essas decisões precisam ser tratadas em uma base site a site. Alguns sites podem aprovar automaticamente todas as novas contas de usuário (o comportamento padrão). Outras pessoas têm um administrador aprovam novas contas ou não aprovam os usuários até visitarem um link enviado para o endereço de email fornecido quando eles se inscreveram. Da mesma forma, alguns sites podem bloquear usuários até que um administrador redefina seu status, enquanto outros sites enviam um email para o usuário bloqueado com uma URL que eles podem visitar para desbloquear sua conta.

Este tutorial mostra como criar uma página da Web para os administradores gerenciarem os status bloqueados e aprovados dos usuários. Também veremos como aprovar novos usuários somente depois de verificarem seu endereço de email.

## <a name="step-1-managing-users-locked-out-and-approved-statuses"></a>Etapa 1: Gerenciando status bloqueados e aprovados dos usuários

<a id="Tutorial12"> </a>Na página [*criando uma interface para selecionar uma conta de usuário a partir de muitos*](building-an-interface-to-select-one-user-account-from-many-vb.md) , criamos uma páginas que listou cada conta de usuário em um GridView filtrado, paginado. A grade lista o nome e o email de cada usuário, seus status aprovados e bloqueados, se eles estão online no momento e comentários sobre o usuário. Para gerenciar os status aprovados e bloqueados dos usuários, poderíamos tornar essa grade editável. Para alterar o status aprovado de um usuário, o administrador primeiro localiza a conta de usuário e, em seguida, editou a linha GridView correspondente, marcando ou desmarcando a caixa de seleção aprovada. Como alternativa, poderíamos gerenciar os status aprovado e bloqueado por meio de uma página ASP.NET separada.

Para este tutorial, vamos usar duas páginas ASP.NET: `ManageUsers.aspx` e `UserInformation.aspx`. A ideia aqui é que `ManageUsers.aspx` lista as contas de usuário no sistema, enquanto `UserInformation.aspx` permite que o administrador gerencie os status aprovados e bloqueados para um usuário específico. Nossa primeira ordem de negócios é aumentar o GridView no `ManageUsers.aspx` para incluir um HyperLinkField, que é renderizado como uma coluna de links. Queremos que cada link aponte para `UserInformation.aspx?user=UserName`, em que *username* é o nome do usuário a ser editado.

> [!NOTE]
> Se você baixou o código para <a id="Tutorial13"> </a>o tutorial de [*recuperação e alteração de senhas*](recovering-and-changing-passwords-vb.md) , talvez tenha notado que a página de `ManageUsers.aspx` já contém um conjunto de links de "gerenciar" e a página de `UserInformation.aspx` fornece uma interface para alterar a senha do usuário selecionado. Decidi não replicar essa funcionalidade no código associado a este tutorial porque ela funcionou ao burlar a API Membership e operar diretamente com o banco de dados SQL Server para alterar a senha de um usuário. Este tutorial começa do zero com a página `UserInformation.aspx`.

### <a name="adding-manage-links-to-theuseraccountsgridview"></a>Adicionando links "gerenciar" para o`UserAccounts`GridView

Abra a página `ManageUsers.aspx` e adicione um HyperLinkField ao `UserAccounts` GridView. Defina a propriedade `Text` do HyperLinkField como "gerenciar" e suas propriedades `DataNavigateUrlFields` e `DataNavigateUrlFormatString` como `UserName` e "UserInformation. aspx? User ={0}", respectivamente. Essas configurações configuram o HyperLinkField de modo que todos os hiperlinks exibam o texto "gerenciar", mas cada link passa o valor de *nome de usuário* apropriado para a QueryString.

Depois de adicionar o HyperLinkField ao GridView, Reserve um momento para exibir a página de `ManageUsers.aspx` por meio de um navegador. Como mostra a Figura 1, cada linha GridView agora inclui um link "Manage". O link "gerenciar" de Bruce aponta para `UserInformation.aspx?user=Bruce`, enquanto o link "gerenciar" para Dave aponta para `UserInformation.aspx?user=Dave`.

[![o HyperLinkField adiciona um](unlocking-and-approving-user-accounts-vb/_static/image2.png)](unlocking-and-approving-user-accounts-vb/_static/image1.png)

**Figura 1**: o HyperLinkField adiciona um link "gerenciar" para cada conta de usuário ([clique para exibir a imagem em tamanho normal](unlocking-and-approving-user-accounts-vb/_static/image3.png))

Vamos criar a interface do usuário e o código para a página de `UserInformation.aspx` daqui a pouco, mas primeiro vamos falar sobre como alterar programaticamente os status bloqueados e aprovados de um usuário. A [classe`MembershipUser`](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx) tem [`IsLockedOut`](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx) e [`IsApproved` Propriedades](https://msdn.microsoft.com/library/system.web.security.membershipuser.isapproved.aspx). A propriedade `IsLockedOut` é somente leitura. Não há nenhum mecanismo para bloquear programaticamente um usuário; para desbloquear um usuário, use o [método`UnlockUser`](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx)da classe `MembershipUser`. A propriedade `IsApproved` é legível e gravável. Para salvar as alterações feitas nessa propriedade, precisamos chamar o [método`UpdateUser`](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx)da classe `Membership`, passando o objeto `MembershipUser` modificado.

Como a propriedade `IsApproved` é legível e gravável, um controle de caixa de seleção é provavelmente o melhor elemento de interface de usuário para configurar essa propriedade. No entanto, uma caixa de seleção não funcionará para a propriedade `IsLockedOut` porque um administrador não pode bloquear um usuário, ela pode apenas desbloquear um usuário. Uma interface de usuário adequada para a propriedade `IsLockedOut` é um botão que, quando clicado, desbloqueia a conta de usuário. Esse botão só deve ser habilitado se o usuário estiver bloqueado.

### <a name="creating-theuserinformationaspxpage"></a>Criando a página de`UserInformation.aspx`

Agora estamos prontos para implementar a interface do usuário no `UserInformation.aspx`. Abra esta página e adicione os seguintes controles da Web:

- Um controle de hiperlink que, quando clicado, retorna o administrador para a página `ManageUsers.aspx`.
- Um controle de rótulo da Web para exibir o nome do usuário selecionado. Defina o `ID` deste rótulo como `UserNameLabel` e desmarque sua propriedade de texto.
- Um controle de caixa de seleção chamado `IsApproved`. Defina sua propriedade `AutoPostBack` como `True`.
- Um controle rótulo para exibir a data do último bloqueio do usuário. Nomeie esse rótulo `LastLockedOutDateLabel` e desmarque sua propriedade `Text`.
- Um botão para desbloquear o usuário. Nomeie esse botão `UnlockUserButton` e defina sua propriedade `Text` como "desbloquear usuário".
- Um controle rótulo para exibir mensagens de status, como "o status aprovado do usuário foi atualizado". Nomeie esse controle `StatusMessage`, desmarque sua propriedade `Text` e defina sua propriedade `CssClass` como `Important`. (A classe CSS `Important` é definida no arquivo de folha de estilo `Styles.css`; ela exibe o texto correspondente em uma fonte grande e vermelha.)

Depois de adicionar esses controles, o modo de exibição de Design no Visual Studio deve ser semelhante à captura de tela na Figura 2.

[![criar a interface do usuário para UserInformation. aspx](unlocking-and-approving-user-accounts-vb/_static/image5.png)](unlocking-and-approving-user-accounts-vb/_static/image4.png)

**Figura 2**: criar a interface do usuário para `UserInformation.aspx` ([clique para exibir a imagem em tamanho normal](unlocking-and-approving-user-accounts-vb/_static/image6.png))

Com a interface do usuário concluída, nossa próxima tarefa é definir a caixa de seleção `IsApproved` e outros controles com base nas informações do usuário selecionado. Crie um manipulador de eventos para o evento de `Load` da página e adicione o seguinte código:

[!code-vb[Main](unlocking-and-approving-user-accounts-vb/samples/sample1.vb)]

O código acima começa garantindo que esta é a primeira visita à página e não um postback subsequente. Em seguida, ele lê o nome de usuário passado pelo campo `user` QueryString e recupera informações sobre essa conta de usuário por meio do método `Membership.GetUser(username)`. Se nenhum nome de usuário foi fornecido por meio da QueryString ou se o usuário especificado não foi encontrado, o administrador é enviado de volta para a página de `ManageUsers.aspx`.

O valor de `UserName` do objeto de `MembershipUser` é exibido no `UserNameLabel` e a caixa de seleção `IsApproved` é marcada com base no valor da propriedade `IsApproved`.

A [propriedade`LastLockoutDate`](https://msdn.microsoft.com/library/system.web.security.membershipuser.lastlockoutdate.aspx) do objeto de `MembershipUser` retorna um valor `DateTime` indicando quando o usuário foi bloqueado pela última vez. Se o usuário nunca tiver sido bloqueado, o valor retornado dependerá do provedor de associação. Quando uma nova conta é criada, a `SqlMembershipProvider` define o campo de `LastLockoutDate` da tabela `aspnet_Membership` como `1754-01-01 12:00:00 AM`. O código acima exibe uma cadeia de caracteres vazia no `LastLockoutDateLabel` se a propriedade `LastLockoutDate` ocorrer antes do ano 2000; caso contrário, a parte de data da propriedade `LastLockoutDate` será exibida no rótulo. A propriedade `Enabled` do `UnlockUserButton`é definida como o status bloqueado do usuário, o que significa que esse botão só será habilitado se o usuário estiver bloqueado.

Reserve um tempo para testar a página de `UserInformation.aspx` por meio de um navegador. Você certamente precisará iniciar em `ManageUsers.aspx` e selecionar uma conta de usuário para gerenciar. Ao chegar em `UserInformation.aspx`, observe que a caixa de seleção `IsApproved` só será marcada se o usuário for aprovado. Se o usuário já tiver sido bloqueado, sua última data de bloqueio será exibida. O botão desbloquear usuário será habilitado somente se o usuário estiver bloqueado no momento. Marcar ou desmarcar a caixa de seleção `IsApproved` ou clicar no botão desbloquear usuário causará um postback, mas nenhuma modificação será feita na conta de usuário, pois ainda criamos manipuladores de eventos para esses eventos.

Retorne ao Visual Studio e crie manipuladores de eventos para o evento de `CheckedChanged` da caixa de seleção de `IsApproved` e o evento `Click` do botão de `UnlockUser`. No manipulador de eventos `CheckedChanged`, defina a propriedade `IsApproved` do usuário para a propriedade `Checked` da caixa de seleção e salve as alterações por meio de uma chamada para `Membership.UpdateUser`. No manipulador de eventos `Click`, basta chamar o método de `UnlockUser` do objeto `MembershipUser`. Em ambos os manipuladores de eventos, exiba uma mensagem adequada no rótulo `StatusMessage`.

[!code-vb[Main](unlocking-and-approving-user-accounts-vb/samples/sample2.vb)]

### <a name="testing-theuserinformationaspxpage"></a>Testando a página de`UserInformation.aspx`

Com esses manipuladores de eventos em vigor, revisite a página e não aprovou um usuário. Como mostra a Figura 3, você verá uma breve mensagem na página indicando que a propriedade de `IsApproved` do usuário foi modificada com êxito.

[![Chris não foi aprovada](unlocking-and-approving-user-accounts-vb/_static/image8.png)](unlocking-and-approving-user-accounts-vb/_static/image7.png)

**Figura 3**: Chris não foi aprovada ([clique para exibir a imagem em tamanho normal](unlocking-and-approving-user-accounts-vb/_static/image9.png))

Em seguida, faça logoff e tente fazer logon como o usuário cuja conta acabou de ser aprovada. Como o usuário não está aprovado, ele não pode fazer logon. Por padrão, o controle de logon exibe a mesma mensagem se o usuário não puder fazer logon, independentemente do motivo. Mas no <a id="Tutorial6"> </a>tutorial [*Validando credenciais de usuário no repositório de usuários da Associação*](../membership/validating-user-credentials-against-the-membership-user-store-vb.md) , examinamos aprimorar o controle de logon para exibir uma mensagem mais apropriada. Como mostra a Figura 4, Chris é mostrada uma mensagem explicando que ele não pode fazer logon porque sua conta ainda não foi aprovada.

[![Chris não pode fazer logon porque sua conta não está aprovada](unlocking-and-approving-user-accounts-vb/_static/image11.png)](unlocking-and-approving-user-accounts-vb/_static/image10.png)

**Figura 4**: Chris não pode fazer logon porque sua conta não está aprovada ([clique para exibir a imagem em tamanho normal](unlocking-and-approving-user-accounts-vb/_static/image12.png))

Para testar a funcionalidade bloqueada, tente fazer logon como um usuário aprovado, mas use uma senha incorreta. Repita esse processo o número necessário de vezes até que a conta do usuário tenha sido bloqueada. O controle de logon também foi atualizado para mostrar uma mensagem personalizada se estiver tentando fazer logon de uma conta bloqueada. Você sabe que uma conta foi bloqueada depois de começar a ver a seguinte mensagem na página de logon: "sua conta foi bloqueada devido a muitas tentativas de logon inválidas. Entre em contato com o administrador para que sua conta seja desbloqueada. "

Volte para a página `ManageUsers.aspx` e clique no link Gerenciar para o usuário bloqueado. Como mostra a Figura 5, você deve ver um valor no `LastLockedOutDateLabel` o botão desbloquear usuário deve ser habilitado. Clique no botão desbloquear usuário para desbloquear a conta de usuário. Depois de desbloquear o usuário, ele poderá fazer logon novamente.

[![Dave foi bloqueado do sistema](unlocking-and-approving-user-accounts-vb/_static/image14.png)](unlocking-and-approving-user-accounts-vb/_static/image13.png)

**Figura 5**: Dave foi bloqueado do sistema ([clique para exibir a imagem em tamanho normal](unlocking-and-approving-user-accounts-vb/_static/image15.png))

## <a name="step-2-specifying-new-users-approved-status"></a>Etapa 2: especificando o status aprovado de novos usuários

O status aprovado é útil em cenários em que você deseja que alguma ação seja executada antes que um novo usuário possa fazer logon e acessar os recursos específicos do usuário do site. Por exemplo, você pode estar executando um site privado em que todas as páginas, exceto as páginas de logon e de inscrição, são acessíveis somente para usuários autenticados. Mas o que acontece se um estranho atingir seu site, encontrar a página de inscrição e criar uma conta? Para evitar que isso aconteça, você pode mover a página de inscrição para uma `Administration` pasta e exigir que um administrador crie manualmente cada conta. Como alternativa, você pode permitir que qualquer pessoa se inscriçõesse, mas proíba o acesso ao site até que um administrador aprove a conta de usuário.

Por padrão, o controle CreateUserWizard aprova novas contas. Você pode configurar esse comportamento usando a propriedade de [`DisableCreatedUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx)do controle. Defina essa propriedade como `True` para não aprovar novas contas de usuário.

> [!NOTE]
> Por padrão, o controle CreateUserWizard faz logon automaticamente na nova conta de usuário. Esse comportamento é determinado pela [propriedade de`LoginCreatedUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx)do controle. Como os usuários não aprovados não podem fazer logon no site, quando `DisableCreatedUser` está `True` a nova conta de usuário não está conectada ao site, independentemente do valor da propriedade `LoginCreatedUser`.

Se você estiver criando programaticamente novas contas de usuário por meio do método `Membership.CreateUser`, para criar uma conta de usuário não aprovada, use uma das sobrecargas que aceitam o valor da propriedade `IsApproved` do novo usuário como um parâmetro de entrada.

## <a name="step-3-approving-users-by-verifying-their-email-address"></a>Etapa 3: aprovar os usuários verificando seu endereço de email

Muitos sites que dão suporte a contas de usuário não aprovam novos usuários até que eles verifiquem o endereço de email fornecido durante o registro. Esse processo de verificação é comumente usado para frustrar bots, spammers e outros ne'er-do-caixas, pois requer um endereço de email verificado exclusivo e adiciona uma etapa extra no processo de inscrição. Com esse modelo, quando um novo usuário se inscreve, ele recebe uma mensagem de email que inclui um link para uma página de verificação. Visitando o link que o usuário comprovou que recebeu o email e, portanto, que o endereço de email fornecido é válido. A página de verificação é responsável por aprovar o usuário. Isso pode acontecer automaticamente, aprovando, assim, qualquer usuário que chegue a essa página, ou somente depois que o usuário fornecer algumas informações adicionais, como um [captcha](http://en.wikipedia.org/wiki/Captcha).

Para acomodar esse fluxo de trabalho, precisamos primeiro atualizar a página de criação da conta para que novos usuários não sejam aprovados. Abra a página `EnhancedCreateUserWizard.aspx` na pasta `Membership` e defina a propriedade `DisableCreatedUser` do controle CreateUserWizard como `True`.

Em seguida, precisamos configurar o controle CreateUserWizard para enviar um email para o novo usuário com instruções sobre como verificar sua conta. Em particular, incluiremos um link no email para a página de `Verification.aspx` (que ainda criamos), passando o `UserId` do novo usuário por meio da QueryString. A página `Verification.aspx` pesquisará o usuário especificado e os marcará como aprovado.

### <a name="sending-a-verification-email-to-new-users"></a>Enviando um email de verificação para novos usuários

Para enviar um email do controle CreateUserWizard, configure sua propriedade `MailDefinition` adequadamente. Conforme discutido no <a id="Tutorial13"> </a> [tutorial anterior](recovering-and-changing-passwords-vb.md), os controles ChangePassword e PasswordRecovery incluem uma [Propriedade`MailDefinition`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx) que funciona da mesma maneira que o controle CreateUserWizard.

> [!NOTE]
> Para usar a propriedade `MailDefinition`, você precisa especificar opções de entrega de email no `Web.config`. Para obter mais informações, consulte [enviando email em ASP.net](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx).

Comece criando um novo modelo de email chamado `CreateUserWizard.txt` na pasta `EmailTemplates`. Use o seguinte texto para o modelo:

[!code-aspx[Main](unlocking-and-approving-user-accounts-vb/samples/sample3.aspx)]

Defina a propriedade `BodyFileName` do `MailDefinition`como "~/EmailTemplates/CreateUserWizard.txt" e sua propriedade `Subject` como "bem-vindo ao meu site! Ative sua conta. "

Observe que o modelo de email `CreateUserWizard.txt` inclui um espaço reservado `<%VerificationUrl%>`. É aqui que a URL para a página de `Verification.aspx` será colocada. O CreateUserWizard substitui automaticamente os espaços reservados `<%UserName%>` e `<%Password%>` com o nome de usuário e a senha da nova conta, mas não há espaço reservado `<%VerificationUrl%>` interno. Precisamos substituí-lo manualmente pela URL de verificação apropriada.

Para fazer isso, crie um manipulador de eventos para o [evento de`SendingMail`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.sendingmail.aspx) do CreateUserWizard e adicione o seguinte código:

[!code-vb[Main](unlocking-and-approving-user-accounts-vb/samples/sample4.vb)]

O evento `SendingMail` é acionado após o evento `CreatedUser`, o que significa que, na hora em que o manipulador de eventos acima é executado, a nova conta de usuário já foi criada. Podemos acessar o novo valor de `UserId` do usuário chamando o método `Membership.GetUser`, passando o `UserName` inserido no controle CreateUserWizard. Em seguida, a URL de verificação é formada. A instrução `Request.Url.GetLeftPart(UriPartial.Authority)` retorna a parte `http://yourserver.com` da URL; `Request.ApplicationPath` retorna o caminho em que o aplicativo tem raiz. A URL de verificação é definida como `Verification.aspx?ID=userId`. Essas duas cadeias de caracteres são concatenadas para formar a URL completa. Por fim, o corpo da mensagem de email (`e.Message.Body`) tem todas as ocorrências de `<%VerificationUrl%>` substituído pela URL completa.

O efeito líquido é que novos usuários não são aprovados, o que significa que eles não podem fazer logon no site. Além disso, eles receberão automaticamente um email com um link para a URL de verificação (veja a Figura 6).

[![o novo usuário recebe um email com um link para a URL de verificação](unlocking-and-approving-user-accounts-vb/_static/image17.png)](unlocking-and-approving-user-accounts-vb/_static/image16.png)

**Figura 6**: o novo usuário recebe um email com um link para a URL de verificação ([clique para exibir a imagem em tamanho normal](unlocking-and-approving-user-accounts-vb/_static/image18.png))

> [!NOTE]
> A etapa CreateUserWizard padrão do controle CreateUserWizard exibe uma mensagem informando ao usuário que sua conta foi criada e exibe um botão continuar. Clicar em isso leva o usuário para a URL especificada pela propriedade de `ContinueDestinationPageUrl` do controle. O CreateUserWizard no `EnhancedCreateUserWizard.aspx` está configurado para enviar novos usuários para o `~/Membership/AdditionalUserInfo.aspx`, que solicita ao usuário sua cidade natal, URL da Home Page e assinatura. Como essas informações só podem ser adicionadas por usuários conectados, faz sentido atualizar essa propriedade para enviar usuários de volta para a home page do site (`~/Default.aspx`). Além disso, a página `EnhancedCreateUserWizard.aspx` ou a etapa CreateUserWizard deve ser aumentada para informar ao usuário que um email de verificação foi enviado e sua conta não será ativada até que siga as instruções neste email. Eu deixe essas modificações como um exercício para o leitor.

### <a name="creating-the-verification-page"></a>Criando a página de verificação

Nossa tarefa final é criar a página de `Verification.aspx`. Adicione essa página à pasta raiz, associando-a à página mestra de `Site.master`. Como fizemos com a maioria das páginas de conteúdo anteriores adicionadas ao site, remova o controle de conteúdo que faz referência ao `LoginContent` ContentPlaceHolder para que a página de conteúdo use o conteúdo padrão da página mestra.

Adicione um controle rótulo da Web à página `Verification.aspx`, defina seu `ID` como `StatusMessage` e desmarque sua propriedade Text. Em seguida, crie o manipulador de eventos `Page_Load` e adicione o seguinte código:

[!code-vb[Main](unlocking-and-approving-user-accounts-vb/samples/sample5.vb)]

A massa do código acima verifica se a UserId fornecida por meio da QueryString existe, se é um valor de `Guid` válido e se faz referência a uma conta de usuário existente. Se todas essas verificações passarem, a conta de usuário será aprovada; caso contrário, uma mensagem de status adequada será exibida.

A Figura 7 mostra a página `Verification.aspx` quando visitada por meio de um navegador.

[![a nova conta do usuário agora está aprovada](unlocking-and-approving-user-accounts-vb/_static/image20.png)](unlocking-and-approving-user-accounts-vb/_static/image19.png)

**Figura 7**: a nova conta do usuário agora é aprovada ([clique para exibir a imagem em tamanho normal](unlocking-and-approving-user-accounts-vb/_static/image21.png))

## <a name="summary"></a>Resumo

Todas as contas de usuário de associação têm dois status que determinam se o usuário pode fazer logon no site: `IsLockedOut` e `IsApproved`. Ambas as propriedades devem ser `True` para que o usuário faça logon.

O status bloqueado do usuário é usado como medida de segurança para reduzir a probabilidade de um hacker invadir um site por meio de métodos de força bruta. Especificamente, um usuário será bloqueado se houver um determinado número de tentativas de logon inválidas dentro de uma determinada janela de tempo. Esses limites são configuráveis por meio das configurações do provedor de associação no `Web.config`.

O status aprovado é comumente usado como um meio de proibir novos usuários de fazer logon até que alguma ação tenha sido transacionada. Talvez o site exija que novas contas sejam aprovadas pela primeira vez pelo administrador ou, como vimos na etapa 3, verificando seu endereço de email.

Boa programação!

### <a name="about-the-author"></a>Sobre o autor

Scott Mitchell, autor de vários livros sobre ASP/ASP. NET e fundador da 4GuysFromRolla.com, tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a *[Sams ensina a ASP.NET 2,0 em 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott pode ser contatado em [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) ou por meio de seu blog em [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a...

Esta série de tutoriais foi revisada por muitos revisores úteis. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](recovering-and-changing-passwords-vb.md)
