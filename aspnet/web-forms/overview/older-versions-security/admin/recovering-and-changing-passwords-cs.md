---
uid: web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-cs
title: Recuperando e alterando senhas (C#) | Microsoft Docs
author: rick-anderson
description: O ASP.NET inclui dois controles da Web para auxiliar na recuperação e na alteração de senhas. O controle PasswordRecovery permite que um visitante recupere seu PA perdido...
ms.author: riande
ms.date: 04/01/2008
ms.assetid: 19c4d042-4e34-4b44-9f1d-6bf2253ba366
msc.legacyurl: /web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-cs
msc.type: authoredcontent
ms.openlocfilehash: 8c07b8a3c36e4863c6d2d356b8483544ac4cafeb
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74576553"
---
# <a name="recovering-and-changing-passwords-c"></a>Recuperação e alteração de senhas (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.13.zip) ou [baixar PDF](https://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial13_ChangingPasswords_cs.pdf)

> O ASP.NET inclui dois controles da Web para auxiliar na recuperação e na alteração de senhas. O controle PasswordRecovery permite que um visitante recupere sua senha perdida. O controle ChangePassword permite que o usuário Atualize sua senha. Assim como os outros controles da Web relacionados ao logon que vimos em toda esta série de tutoriais, os controles PasswordRecovery e ChangePassword funcionam com a estrutura de associação nos bastidores para redefinir ou modificar as senhas dos usuários.

## <a name="introduction"></a>Introdução

Entre os sites do meu banco, a empresa do utilitário, a empresa de telefones, as contas de email e os portais da Web personalizados, eu, como a maioria das pessoas, têm dezenas de senhas diferentes a serem lembradas. Com tantas credenciais para memorizar esses dias, não é incomum que as pessoas esquecem sua senha. Para considerar isso, os sites que oferecem contas de usuário precisam incluir uma maneira para um usuário recuperar sua senha. Esse processo geralmente envolve a geração de uma senha nova e aleatória e o envio por email para o endereço de email do usuário no arquivo. Depois de receber sua nova senha, a maioria dos usuários retorna para o site e altera a senha do gerada aleatoriamente para um mais fácil de memorizar.

O ASP.NET inclui dois controles da Web para auxiliar na recuperação e na alteração de senhas. O controle PasswordRecovery permite que um visitante recupere sua senha perdida. O controle ChangePassword permite que o usuário Atualize sua senha. Assim como os outros controles da Web relacionados ao logon que vimos em toda esta série de tutoriais, os controles PasswordRecovery e ChangePassword funcionam com a estrutura de associação nos bastidores para redefinir ou modificar as senhas dos usuários.

Neste tutorial, examinaremos o uso desses dois controles. Também veremos como alterar e redefinir de forma programática a senha de um usuário por meio dos métodos `ChangePassword` e `ResetPassword` da classe `MembershipUser`.

## <a name="step-1-helping-users-recover-lost-passwords"></a>Etapa 1: ajudando os usuários a recuperar senhas perdidas

Todos os sites que dão suporte às contas de usuário precisam fornecer aos usuários algum mecanismo para recuperar suas senhas esquecidas. A boa notícia é que a implementação dessa funcionalidade em ASP.NET é uma Breeze graças ao controle da Web PasswordRecovery. O controle PasswordRecovery renderiza uma interface que solicita ao usuário o nome de seus nomes e, se necessário, a resposta para sua pergunta de segurança. Em seguida, ele envia por email a senha do usuário.

> [!NOTE]
> Como as mensagens de email são transmitidas pela transmissão em texto sem formatação, há riscos de segurança envolvidos no envio da senha de um usuário por email.

O controle PasswordRecovery consiste em três modos de exibição:

- **Nome de usuário** -solicita ao visitante seu nome de usuário. Esta é a exibição inicial.
- **Pergunta**– exibe o nome de usuário e a pergunta de segurança como texto, juntamente com uma caixa de texto para que o usuário insira a resposta para sua pergunta de segurança.
- **Êxito**– exibe uma mensagem informando ao usuário que sua senha foi enviada por email.

As exibições exibidas e ações executadas pelo controle PasswordRecovery dependem das seguintes definições de configuração de associação:

- `RequiresQuestionAndAnswer`
- `EnablePasswordRetrieval`
- `EnablePasswordReset`

A configuração de `RequiresQuestionAndAnswer` da estrutura de associação indica se os usuários devem especificar uma pergunta e resposta de segurança ao se registrarem para uma conta. Como discutimos no <a id="_msoanchor_1"> </a>tutorial [*criando contas de usuário*](../membership/creating-user-accounts-cs.md) , se `RequiresQuestionAndAnswer` for true (o padrão), a interface do CreateUserWizard incluirá controles TextBox para a pergunta e a resposta de segurança do novo usuário; se `RequiresQuestionAndAnswer` for false, nenhuma informação será coletada. Da mesma forma, se `RequiresQuestionAndAnswer` for true, o controle PasswordRecovery exibirá a exibição de pergunta depois que o usuário inserir seu nome de usuário; a senha será recuperada somente se o usuário inserir a resposta de segurança correta. Se `RequiresQuestionAndAnswer` for false, no entanto, o controle PasswordRecovery se moverá diretamente da exibição de nome de usuário para a exibição êxito.

Depois que o usuário tiver fornecido seu nome de usuário, ou seu nome de usuário e resposta de segurança, se `RequiresQuestionAndAnswer` for true, o PasswordRecovery enviará uma senha para o usuário. Se a opção `EnablePasswordRetrieval` for definida como true, o usuário receberá uma mensagem de senha atual. Se ele for definido como false e `EnablePasswordReset` for definido como true, o controle PasswordRecovery gerará uma nova senha aleatória para o usuário e enviará por email essa nova senha para eles. Se `EnablePasswordRetrieval` e `EnablePasswordReset` forem false, o controle PasswordRecovery lançará uma exceção.

> [!NOTE]
> Lembre-se de que o `SqlMembershipProvider` armazena as senhas dos usuários em um dos três formatos: Clear, com hash (o padrão) ou Encrypted. O mecanismo de armazenamento usado depende das definições de configuração de associação; o aplicativo de demonstração usa o formato de senha com hash. Ao usar o formato de senha com hash, a opção `EnablePasswordRetrieval` deve ser definida como false porque o sistema não pode determinar a senha real do usuário a partir da versão com hash armazenada no banco de dados.

A Figura 1 ilustra como a interface e o comportamento do PasswordRecovery são influenciados pela configuração da associação.

[![RequiresQuestionAndAnswer, EnablePasswordRetrieval e EnablePasswordReset influenciam a aparência e o comportamento do controle PasswordRecovery](recovering-and-changing-passwords-cs/_static/image2.png)](recovering-and-changing-passwords-cs/_static/image1.png)

**Figura 1**: o `RequiresQuestionAndAnswer`, `EnablePasswordRetrieval`e `EnablePasswordReset` influenciam a aparência e o comportamento do controle PasswordRecovery ([clique para exibir a imagem em tamanho normal](recovering-and-changing-passwords-cs/_static/image3.png))

> [!NOTE]
> <a id="_msoanchor_2"> </a>No tutorial [*criando o esquema de associação no SQL Server*](../membership/creating-the-membership-schema-in-sql-server-cs.md) , configuramos o provedor de associação definindo `RequiresQuestionAndAnswer` como true, `EnablePasswordRetrieval` como false e `EnablePasswordReset` como true.

### <a name="using-the-passwordrecovery-control"></a>Usando o controle PasswordRecovery

Vejamos o uso do controle PasswordRecovery em uma página ASP.NET. Abra `RecoverPassword.aspx` e arraste e solte um controle PasswordRecovery da caixa de ferramentas para o designer; Defina seu `ID` como `RecoverPwd`. Como os controles de logon e CreateUserWizard da Web, as exibições do controle PasswordRecovery renderizam uma interface composta rica que inclui rótulos, caixas de Text, botões e controles de validação. Você pode personalizar a aparência das exibições por meio das propriedades de estilo do controle ou convertendo as exibições em modelos. Deixe isso como um exercício para o leitor interessado.

Quando um usuário visitar esta página, ela inserirá seu nome de usuário e clicará no botão enviar. Como definimos a propriedade `RequiresQuestionAndAnswer` como true em nossas definições de configuração de associação, o controle PasswordRecovery exibirá a exibição de pergunta. Depois que o usuário inserir sua resposta de segurança correta e clicar em enviar, o controle PasswordRecovery atualizará a senha do usuário para uma gerada aleatoriamente e enviará por email essa senha para o endereço de email no arquivo. Tudo isso era possível sem a necessidade de escrever uma única linha de código!

Antes de testar esta página, há uma parte final da configuração para a qual tendem a: precisamos especificar as configurações de entrega de email em `Web.config`. O controle PasswordRecovery depende dessas configurações para enviar o email.

A configuração de entrega de email é especificada por meio do [elemento`<mailSettings>`](https://msdn.microsoft.com/library/w355a94k.aspx)do [elemento de`<system.net>`](https://msdn.microsoft.com/library/6484zdc1.aspx). Use o [elemento`<smtp>`](https://msdn.microsoft.com/library/ms164240.aspx) para indicar o método de entrega e o endereço padrão de. A marcação a seguir define as configurações de email para usar um servidor SMTP de rede chamado `smtp.example.com` na porta 25 e com credenciais de nome de usuário/senha de nome de usuário e senha.

> [!NOTE]
> `<system.net>` é um elemento filho do elemento de `<configuration>` raiz e um irmão de `<system.web>`. Portanto, não coloque o elemento `<system.net>` dentro do elemento `<system.web>`; em vez disso, coloque-o no mesmo nível.

[!code-xml[Main](recovering-and-changing-passwords-cs/samples/sample1.xml)]

Além de usar um servidor SMTP na rede, você pode especificar como alternativa um diretório de retirada onde as mensagens de email a serem enviadas devem ser depositadas.

Depois de definir as configurações de SMTP, visite a página `RecoverPassword.aspx` por meio de um navegador. Primeiro, tente inserir um nome de usuário que não exista no repositório de usuários. Como mostra a Figura 2, o controle PasswordRecovery exibe uma mensagem indicando que as informações do usuário não puderam ser acessadas. O texto da mensagem pode ser personalizado por meio da [propriedade`UserNameFailureText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.usernamefailuretext.aspx)do controle.

[![uma mensagem de erro será exibida se um nome de usuário inválido for inserido](recovering-and-changing-passwords-cs/_static/image5.png)](recovering-and-changing-passwords-cs/_static/image4.png)

**Figura 2**: uma mensagem de erro será exibida se um nome de usuário inválido for inserido ([clique para exibir a imagem em tamanho normal](recovering-and-changing-passwords-cs/_static/image6.png))

Agora, insira um nome de usuário. Use o nome de usuário de uma conta no sistema com um endereço de email que você pode acessar e cuja resposta de segurança você sabe. Depois de inserir o nome de usuário e clicar em enviar, o controle PasswordRecovery exibe sua exibição de pergunta. Assim como acontece com a exibição de nome de usuário, se você inserir uma resposta incorreta, o controle PasswordRecovery exibirá uma mensagem de erro (consulte a Figura 3). Use a [propriedade`QuestionFailureText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.questionfailuretext.aspx) para personalizar essa mensagem de erro.

[![uma mensagem de erro será exibida se o usuário inserir uma resposta de segurança inválida](recovering-and-changing-passwords-cs/_static/image8.png)](recovering-and-changing-passwords-cs/_static/image7.png)

**Figura 3**: uma mensagem de erro será exibida se o usuário inserir uma resposta de segurança inválida ([clique para exibir a imagem em tamanho normal](recovering-and-changing-passwords-cs/_static/image9.png))

Por fim, insira a resposta de segurança correta e clique em enviar. Nos bastidores, o controle PasswordRecovery gera uma senha aleatória, a atribui à conta de usuário, envia um email informando ao usuário de sua nova senha (consulte a Figura 4) e, em seguida, exibe a exibição êxito.

[![o usuário recebe um email com sua nova senha](recovering-and-changing-passwords-cs/_static/image11.png)](recovering-and-changing-passwords-cs/_static/image10.png)

**Figura 4**: o usuário recebe um email com sua nova senha ([clique para exibir a imagem em tamanho normal](recovering-and-changing-passwords-cs/_static/image12.png))

### <a name="customizing-the-email"></a>Personalizando o email

O email padrão enviado pelo controle PasswordRecovery é bem opaco (consulte a Figura 4). A mensagem é enviada da conta especificada no atributo `from` do elemento `<smtp>` com a senha da entidade e o corpo de texto sem formatação:

Retorne ao site e faça logon usando as informações a seguir.

Nome de usuário: *username*

Senha: *senha*

Essa mensagem pode ser personalizada programaticamente por meio de um manipulador de eventos para o [evento de`SendingMail`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.sendingmail.aspx)do controle PasswordRecovery ou declarativamente por meio da [Propriedade`MailDefinition`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.maildefinition.aspx). Vamos explorar essas duas opções.

O evento de `SendingMail` é disparado imediatamente antes de a mensagem de email ser enviada e é nossa última chance de ajustar programaticamente a mensagem de email. Quando esse evento é gerado, o manipulador de eventos recebe um objeto do tipo [`MailMessageEventArgs`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.mailmessageeventargs.aspx), cuja propriedade `Message` contém uma referência ao email prestes a ser enviado.

Crie um manipulador de eventos para o evento `SendingMail` e adicione o código a seguir, que adiciona programaticamente `webmaster@example.com` à lista CC.

[!code-csharp[Main](recovering-and-changing-passwords-cs/samples/sample2.cs)]

A mensagem de email também pode ser configurada por meio de meios declarativos. A propriedade `MailDefinition` do PasswordRecovery é um objeto do tipo [`MailDefinition`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.aspx). A classe `MailDefinition` oferece um host de propriedades relacionadas a email, incluindo `From`, `CC`, `Priority`, `Subject`, `IsBodyHtml`, `BodyFileName`e outras. Para os iniciantes, defina a [propriedade`Subject`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.subject.aspx) como algo mais descritivo do que aquele usado por padrão (senha), como sua senha foi redefinida...

Para personalizar o corpo da mensagem de email, precisamos criar um arquivo de modelo de email separado que contenha o conteúdo do corpo. Comece criando uma nova pasta no site denominada `EmailTemplates`. Em seguida, adicione um novo arquivo de texto a essa pasta chamado `PasswordRecovery.txt` e adicione o seguinte conteúdo:

[!code-aspx[Main](recovering-and-changing-passwords-cs/samples/sample3.aspx)]

Observe o uso dos espaços reservados `<%UserName%>` e `<%Password%>`. O controle PasswordRecovery substitui automaticamente esses dois espaços reservados pelo nome do usuário e a senha recuperada antes de enviar o email.

Por fim, aponte a [propriedade`BodyFileName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx) do `MailDefinition`para o modelo de email que acabamos de criar (`~/EmailTemplates/PasswordRecovery.txt`).

Depois de fazer essas alterações, revisite a página `RecoverPassword.aspx` e insira seu nome de usuário e resposta de segurança. Você receberá um email parecido com o da Figura 5. Observe que `webmaster@example.com` foi CC e que o assunto e o corpo foram atualizados.

[![o assunto, o corpo e a lista de CC foram atualizados](recovering-and-changing-passwords-cs/_static/image14.png)](recovering-and-changing-passwords-cs/_static/image13.png)

**Figura 5**: o assunto, o corpo e a lista de CC foram atualizados ([clique para exibir a imagem em tamanho normal](recovering-and-changing-passwords-cs/_static/image15.png))

Para enviar um conjunto de emails formatado em HTML [`IsBodyHtml`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.isbodyhtml.aspx) como true (o padrão é false) e atualizar o modelo de email para incluir HTML.

A propriedade `MailDefinition` não é exclusiva para a classe PasswordRecovery. Como veremos na etapa 2, o controle ChangePassword também oferece uma propriedade `MailDefinition`. Além disso, o controle CreateUserWizard inclui tal propriedade que você pode configurar para enviar automaticamente uma mensagem de email de boas-vindas para novos usuários.

> [!NOTE]
> No momento, não há links no painel de navegação à esquerda para acessar a página `RecoverPassword.aspx`. Um usuário só estaria interessado em visitar esta página se não conseguir fazer logon com êxito no site. Portanto, atualize a página de `Login.aspx` para incluir um link para a página de `RecoverPassword.aspx`.

### <a name="programmatically-resetting-a-users-password"></a>Redefinindo de forma programática a senha de um usuário

Ao redefinir a senha de um usuário, o controle PasswordRecovery chama o [método`ResetPassword`](https://msdn.microsoft.com/library/system.web.security.membershipuser.resetpassword.aspx)do objeto `MembershipUser`. Esse método tem duas sobrecargas:

- **[`ResetPassword`](https://msdn.microsoft.com/library/d94bdzz2.aspx)** -redefine a senha de um usuário. Use essa sobrecarga se `RequiresQuestionAndAnswer` for false.
- **[`ResetPassword(securityAnswer)`](https://msdn.microsoft.com/library/d90zte4w.aspx)** -redefine a senha de um usuário somente se o *securityAnswer* fornecido estiver correto. Use essa sobrecarga se `RequiresQuestionAndAnswer` for true.

Ambas as sobrecargas retornam a nova senha gerada aleatoriamente.

Assim como com os outros métodos na estrutura de associação, o método `ResetPassword` delega para o provedor configurado. O `SqlMembershipProvider` invoca o procedimento armazenado `aspnet_Membership_ResetPassword`, passando o nome de usuário, a nova senha e a resposta de senha fornecida, entre outros campos. O procedimento armazenado garante que a resposta de senha corresponda e, em seguida, atualiza a senha do usuário.

Algumas notas de implementação de nível baixo:

- Um usuário bloqueado não pode redefinir sua senha. No entanto, um usuário não aprovado pode. Discutiremos os Estados bloqueados e aprovados mais detalhadamente no tutorial <a id="_msoanchor_3"> </a> [*desbloqueando e aprovando*](unlocking-and-approving-user-accounts-cs.md) contas de usuário.
- Se a resposta de senha estiver incorreta, a contagem de tentativas de resposta de senha com falha do usuário será incrementada. Se um número especificado de tentativas de resposta de segurança inválidas ocorrer em uma janela de tempo especificada, o usuário será bloqueado.

### <a name="a-word-on-how-the-random-passwords-are-generated"></a>Uma palavra sobre como as senhas aleatórias são geradas

As senhas geradas aleatoriamente mostradas nas mensagens de email nas figuras 4 e 5 são criadas pelo [método`GeneratePassword`](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx)da classe Membership. Esse método aceita duas *entradas de número* inteiro parâmetros e *numberOfNonAlphanumericCharacters* -e retorna uma cadeia de caracteres com pelo menos um *comprimento* de caractere com pelo menos *numberOfNonAlphanumericCharacters* número de caracteres não alfanuméricos. Quando esse método é chamado de dentro das classes Membership ou controles da Web relacionados ao logon, os valores desses dois parâmetros são determinados pelas propriedades `MinRequiredPasswordLength` e `MinRequiredNonalphanumericCharacters` da configuração de associação, que definimos como 7 e 1, respectivamente.

O método `GeneratePassword` usa um gerador de números aleatórios criptograficamente forte para garantir que não haja nenhuma tendência em quais caracteres aleatórios são selecionados. Além disso, `GeneratePassword` é `public`, o que significa que você pode usá-lo diretamente do seu aplicativo ASP.NET se precisar gerar cadeias de caracteres ou senhas aleatórias.

> [!NOTE]
> A classe `SqlMembershipProvider` sempre gera uma senha aleatória com pelo menos 14 caracteres de comprimento, portanto, se `MinRequiredPasswordLength` for menor que 14, seu valor será ignorado.

## <a name="step-2-changing-passwords"></a>Etapa 2: alterando senhas

As senhas geradas aleatoriamente são difíceis de se lembrar. Considere a senha mostrada na Figura 4: `WWGUZv(f2yM:Bd`. Tente confirmar isso para a memória! Não é preciso dizer que, depois que um usuário recebe uma senha gerada aleatoriamente desse tipo, ela desejará alterar a senha para algo mais fácil de memorizar.

Use o controle ChangePassword para criar uma interface para um usuário alterar sua senha. Assim como o controle PasswordRecovery, o controle ChangePassword consiste em dois modos de exibição: alterar senha e êxito. A exibição Alterar senha solicita ao usuário suas senhas novas e antigas. Ao fornecer a senha antiga correta e uma nova senha que atenda aos requisitos mínimos de caracteres não alfanuméricos, o controle ChangePassword atualiza a senha do usuário e exibe a exibição êxito.

> [!NOTE]
> O controle ChangePassword modifica a senha do usuário invocando o [método`ChangePassword`](https://msdn.microsoft.com/library/system.web.security.membershipuser.changepassword.aspx)do objeto de `MembershipUser`. O método ChangePassword aceita dois parâmetros de entrada `string`- *oldPassword* e *newPassword*-e atualiza a conta do usuário com *newPassword*, supondo que o *oldPassword* fornecido esteja correto.

Abra a página `ChangePassword.aspx` e adicione um controle ChangePassword à página, nomeando-a `ChangePwd`. Neste ponto, o modo de exibição de Design deve mostrar a exibição Alterar senha (consulte a Figura 6). Assim como com o controle PasswordRecovery, você pode alternar entre as exibições por meio da marca inteligente do controle. Além disso, as aparências dessas exibições são personalizáveis por meio das propriedades de estilo asclassificadas ou convertendo-as em um modelo.

[![adicionar um controle ChangePassword à página](recovering-and-changing-passwords-cs/_static/image17.png)](recovering-and-changing-passwords-cs/_static/image16.png)

**Figura 6**: adicionar um controle ChangePassword à página ([clique para exibir a imagem em tamanho normal](recovering-and-changing-passwords-cs/_static/image18.png))

O controle ChangePassword pode atualizar a senha do usuário conectado no momento *ou* a senha de outro usuário especificado. Como mostra a Figura 6, a exibição padrão alterar senha processa apenas três controles TextBox: um para a senha antiga e dois para a nova senha. Essa interface padrão é usada para atualizar a senha do usuário conectado no momento.

Para usar o controle ChangePassword para atualizar a senha de outro usuário, defina a [propriedade`DisplayUserName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.changepassword.displayusername.aspx) do controle como true. Isso adiciona uma quarta caixa de texto à página, solicitando o nome do usuário cuja senha será alterada.

Definir `DisplayUserName` como true será útil se você quiser permitir que um usuário desconectado altere sua senha sem precisar fazer logon. Pessoalmente, acho que não há nada de errado ao exigir que um usuário faça logon antes de permitir que ele altere sua senha. Portanto, deixe `DisplayUserName` definido como false (seu padrão). No entanto, ao tomar essa decisão, estamos essencialmente impedindo que usuários anônimos cheguem a essa página. Atualize as regras de autorização de URL do site para negar que usuários anônimos visitem `ChangePassword.aspx`. Se você precisar atualizar sua memória na sintaxe da regra de autorização de URL, consulte o tutorial <a id="_msoanchor_4"> </a>de [*autorização baseada no usuário*](../membership/user-based-authorization-cs.md) .

> [!NOTE]
> Pode parecer que a propriedade `DisplayUserName` é útil para permitir que os administradores alterem as senhas de outros usuários. No entanto, mesmo quando `DisplayUserName` é definido como true, a senha antiga correta deve ser conhecida e inserida. Falaremos sobre técnicas para permitir que os administradores alterem as senhas dos usuários na etapa 3.

Visite a página `ChangePassword.aspx` por meio de um navegador e altere sua senha. Observe que uma mensagem de erro será exibida se você inserir uma nova senha que não atenda ao comprimento da senha e aos requisitos de caracteres não alfanuméricos especificados na configuração da Associação (consulte a Figura 7).

[![adicionar um controle ChangePassword à página](recovering-and-changing-passwords-cs/_static/image20.png)](recovering-and-changing-passwords-cs/_static/image19.png)

**Figura 7**: adicionar um controle ChangePassword à página ([clique para exibir a imagem em tamanho normal](recovering-and-changing-passwords-cs/_static/image21.png))

Ao inserir a senha antiga correta e uma nova senha válida, a senha do usuário conectado é alterada e a exibição de êxito exibida.

### <a name="sending-a-confirmation-email"></a>Enviando um email de confirmação

Por padrão, o controle ChangePassword não envia uma mensagem de email para o usuário cuja senha acabou de ser atualizada. Se você quiser enviar um email, basta configurar a propriedade de `MailDefinition` do controle. Vamos configurar o controle ChangePassword para que o usuário tenha enviado um email formatado em HTML que contenha sua nova senha.

Comece criando um novo arquivo na pasta `EmailTemplates` chamada `ChangePassword.htm`. Adicione a seguinte marcação:

[!code-html[Main](recovering-and-changing-passwords-cs/samples/sample4.html)]

Em seguida, defina as propriedades `BodyFileName`, `IsBodyHtml`e `Subject` da propriedade de `MailDefinition` do controle ChangePassword como ~/EmailTemplates/ChangePassword.htm, true, e sua senha foi alterada!, respectivamente.

Depois de fazer essas alterações, reveja a página e altere sua senha novamente. Desta vez, o controle ChangePassword envia um email personalizado e formatado em HTML para o endereço de email do usuário no arquivo (veja a Figura 8).

[![uma mensagem de email informa ao usuário que sua senha foi alterada](recovering-and-changing-passwords-cs/_static/image23.png)](recovering-and-changing-passwords-cs/_static/image22.png)

**Figura 8**: uma mensagem de email informa ao usuário que sua senha foi alterada ([clique para exibir a imagem em tamanho normal](recovering-and-changing-passwords-cs/_static/image24.png))

## <a name="step-3-allowing-administrators-to-change-users-passwords"></a>Etapa 3: permitir que os administradores alterem as senhas dos usuários

Um recurso comum em aplicativos que dão suporte a contas de usuário é a capacidade de um usuário administrativo alterar as senhas de outros usuários. Às vezes, essa funcionalidade é necessária porque o sistema não tem a capacidade de os usuários alterarem suas próprias senhas. Nesse caso, a única maneira de um usuário recuperar sua senha esquecida seria para o administrador atribuir uma nova senha. No entanto, com os controles PasswordRecovery e ChangePassword, os usuários administrativos não precisam ficar ocupados com a alteração das senhas dos usuários, pois os usuários são capazes de fazer isso por conta própria.

Mas e se o cliente insistir que os usuários administrativos devem ser capazes de alterar as senhas de outros usuários? Infelizmente, a adição dessa funcionalidade pode ser um pouco de trabalho. Para alterar a senha de um usuário, a senha antiga e a nova devem ser fornecidas para o método `ChangePassword` do objeto de `MembershipUser`, mas um administrador não precisa saber a senha de um usuário para modificá-la.

Uma solução alternativa é primeiro redefinir a senha do usuário e, em seguida, alterá-la para a nova senha usando um código semelhante ao seguinte:

[!code-aspx[Main](recovering-and-changing-passwords-cs/samples/sample5.aspx)]

Esse código começa recuperando informações sobre *username*, que é o usuário cuja senha o administrador deseja alterar. Em seguida, o método `ResetPassword` é invocado, que atribui e ao usuário uma senha nova e aleatória. Essa senha gerada aleatoriamente é retornada pelo método e armazenada na variável `resetPwd`. Agora que sabemos a senha do usuário, podemos alterá-la por meio de uma chamada para `ChangePassword`.

O problema é que esse código só funcionará se a configuração do sistema de associação for definida de modo que `RequiresQuestionAndAnswer` seja false. Se `RequiresQuestionAndAnswer` for true, como em nosso aplicativo, o método `ResetPassword` precisará passar a resposta de segurança, caso contrário, ele gerará uma exceção.

Se a estrutura de associação estiver configurada para exigir uma pergunta e resposta de segurança e, ainda assim, o cliente insistir que os administradores possam alterar as senhas dos usuários, você terá três opções:

- Jogue suas mãos no ar e diga ao seu cliente que isso é apenas uma coisa que não pode ser feita.
- Defina `RequiresQuestionAndAnswer` como false. Isso resulta em um aplicativo menos seguro. Imagine que um usuário perigoso tenha obtido acesso à caixa de entrada de email de outro usuário. Talvez o usuário comprometido tenha deixado sua mesa para ir para almoçar e não bloquear sua estação de trabalho, ou talvez tenha acessado seu email de um terminal público e não tenha se desconectado. Em ambos os casos, o usuário perigoso pode visitar a página `RecoverPassword.aspx` e inserir o nome do usuário. O sistema enviará por email a senha recuperada sem solicitar a resposta de segurança.
- Ignore a camada de abstração criada pela estrutura de associação e trabalhe diretamente com o banco de dados SQL Server. O esquema de associação inclui um procedimento armazenado chamado `aspnet_Membership_SetPassword` que define a senha de um usuário e não requer a resposta de segurança ou a senha antiga para realizar sua tarefa.

Nenhuma dessas opções é especialmente atraente, mas é assim que a vida de um desenvolvedor vai às vezes.

Eu fiz e implementei a terceira abordagem, escrevendo código que ignora as classes `Membership` e `MembershipUser` e opera diretamente com o banco de dados `SecurityTutorials`.

> [!NOTE]
> Trabalhando diretamente com o banco de dados, o encapsulamento fornecido pela estrutura de associação é eliminado. Essa decisão nos amarra à `SqlMembershipProvider`, tornando nosso código menos portátil. Além disso, esse código pode não funcionar como esperado em versões futuras do ASP.NET se o esquema de associação for alterado. Essa abordagem é uma solução alternativa e, como a maioria das soluções alternativas, não é um exemplo de práticas recomendadas.

O código tem alguns bits inatraentes e é bastante longo. Portanto, não quero obstruir este tutorial com um exame detalhado dele. Se você estiver interessado em saber mais, baixe o código deste tutorial e visite a página `~/Administration/ManageUsers.aspx`. Esta página, que criamos no <a id="_msoanchor_5"> </a> [tutorial anterior](building-an-interface-to-select-one-user-account-from-many-cs.md), lista cada usuário. Atualizei o GridView para incluir um link para a página `UserInformation.aspx`, passando o nome de usuário selecionado por meio de QueryString. A página `UserInformation.aspx` exibe informações sobre o usuário selecionado e caixas de Textpara alterar sua senha (consulte a Figura 9).

Depois de inserir a nova senha, confirmando-a na segunda caixa de texto e clicando no botão atualizar usuário, um postback massacre e o procedimento armazenado `aspnet_Membership_SetPassword` é invocado, atualizando a senha do usuário. Recomendo que os leitores interessados nessa funcionalidade se tornem mais familiarizados com o código e tentem estender a funcionalidade para incluir o envio de um email ao usuário cuja senha tenha sido alterada.

[![um administrador pode alterar a senha de um usuário](recovering-and-changing-passwords-cs/_static/image26.png)](recovering-and-changing-passwords-cs/_static/image25.png)

**Figura 9**: um administrador pode alterar a senha de um usuário ([clique para exibir a imagem em tamanho normal](recovering-and-changing-passwords-cs/_static/image27.png))

> [!NOTE]
> No momento, a página `UserInformation.aspx` só funciona se a estrutura de associação estiver configurada para armazenar senhas em formato limpo ou com hash. Ele não tem o código para criptografar a nova senha, embora você esteja convidado a adicionar essa funcionalidade. A maneira como recomendo adicionar o código necessário é usar um descompilador como o [reflector](http://www.aisto.com/roeder/dotnet/) para examinar o código-fonte em busca de métodos na .NET Framework; Comece examinando o método `ChangePassword` da classe `SqlMembershipProvider`. Essa é a técnica que usei para escrever o código para criar um hash da senha.

## <a name="summary"></a>Resumo

O ASP.NET oferece dois controles para ajudar os usuários a gerenciar sua senha. O controle PasswordRecovery é útil para aqueles que esqueceram suas senhas. Dependendo da configuração da estrutura de associação, o usuário receberá por email sua senha existente ou uma nova senha gerada aleatoriamente. O controle ChangePassword permite que um usuário Atualize sua senha.

Assim como os controles de logon e CreateUserWizard, os controles PasswordRecovery e ChangePassword renderizam uma rica interface do usuário sem precisar escrever uma marcação declarativa ou linha de código. Se a interface do usuário padrão não atender às suas necessidades, você poderá personalizá-la por meio de uma variedade de propriedades de estilo. Como alternativa, as interfaces dos controles podem ser convertidas em modelos, para um grau de controle ainda mais definado. Nos bastidores, esses controles usam a API Membership, invocando os métodos `ResetPassword` e `ChangePassword` do objeto de `MembershipUser`.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Guias de início rápido do controle ChangePassword](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/changepassword.aspx)
- [Guias de início rápido do controle PasswordRecovery](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/passwordrecovery.aspx)
- [Enviando email no ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [Perguntas frequentes `System.Net.Mail`](http://www.systemnetmail.com/)

### <a name="about-the-author"></a>Sobre o autor

Scott Mitchell, autor de vários livros sobre ASP/ASP. NET e fundador da 4GuysFromRolla.com, tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a *[Sams ensina a ASP.NET 2,0 em 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott pode ser contatado em [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) ou por meio de seu blog em [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores potenciais para este tutorial incluem Michael Emmings e Banerjee. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](building-an-interface-to-select-one-user-account-from-many-cs.md)
> [Próximo](unlocking-and-approving-user-accounts-cs.md)
