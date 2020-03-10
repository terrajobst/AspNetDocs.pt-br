---
uid: web-pages/overview/getting-started/11-adding-email-to-your-web-site
title: Enviando email de um site Páginas da Web do ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Este capítulo explica como enviar uma mensagem de email automatizada de um site.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: fc49bcb9-f1a9-4048-8c3f-b60951853200
msc.legacyurl: /web-pages/overview/getting-started/11-adding-email-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 23e9717329525fb5a0ed505c9dc94505d4f9dbbe
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634711"
---
# <a name="sending-email-from-an-aspnet-web-pages-razor-site"></a>Enviando email de um site Páginas da Web do ASP.NET (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo explica como enviar uma mensagem de email de um site quando você usa Páginas da Web do ASP.NET (Razor).
> 
> O que você aprenderá:
> 
> - Como enviar uma mensagem de email do seu site.
> - Como anexar um arquivo a uma mensagem de email.
> 
> Esse é o recurso ASP.NET introduzido no artigo:
> 
> - O auxiliar de `WebMail`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 3
>   
> 
> Este tutorial também funciona com o Páginas da Web do ASP.NET 2.

<a id="Sending_Email_Messages"></a>
## <a name="sending-email-messages-from-your-website"></a>Enviando mensagens de email do seu site

Há todos os tipos de motivos pelos quais você pode precisar enviar emails do seu site. Você pode enviar mensagens de confirmação para os usuários ou você pode enviar notificações para si mesmo (por exemplo, que um novo usuário registrou). O auxiliar `WebMail` facilita o envio de emails.

Para usar o auxiliar de `WebMail`, você precisa ter acesso a um servidor SMTP. (SMTP significa *protocolo de transferência de email simples*.) Um servidor SMTP é um servidor de email que só encaminha mensagens para o servidor &#8212; do destinatário é o lado de saída do email. Se você usar um provedor de hospedagem para seu site, ele provavelmente o configurará com email e poderá dizer a você qual é o nome do servidor SMTP. Se você estiver trabalhando dentro de uma rede corporativa, um administrador ou seu departamento de ti normalmente poderá fornecer as informações sobre um servidor SMTP que você pode usar. Se você estiver trabalhando em casa, poderá até mesmo ser capaz de testar usando seu provedor de email comum, que pode informar o nome do servidor SMTP. Normalmente, você precisa de:

- O nome do servidor SMTP.
- O número da porta. É quase sempre 25. No entanto, seu ISP pode exigir que você use a porta 587. Se você estiver usando SSL (Secure Sockets Layer) para email, talvez precise de uma porta diferente. Verifique com seu provedor de email.
- Credenciais (nome de usuário, senha).

Neste procedimento, você cria duas páginas. A primeira página tem um formulário que permite aos usuários inserir uma descrição, como se estivessem preenchendo um formulário de suporte técnico. A primeira página envia suas informações para uma segunda página. Na segunda página, o código extrai as informações do usuário e envia uma mensagem de email. Ele também exibe uma mensagem confirmando que o relatório de problemas foi recebido.

![[imagem]](11-adding-email-to-your-web-site/_static/image1.jpg)

> [!NOTE]
> Para manter esse exemplo simples, o código inicializa o `WebMail` auxiliar na página em que você o usa. No entanto, para sites reais, é uma ideia melhor colocar o código de inicialização como este em um arquivo global, para que você inicialize o auxiliar de `WebMail` para todos os arquivos em seu site. Para obter mais informações, consulte [Personalizando o comportamento de todo o site para páginas da Web do ASP.net](https://go.microsoft.com/fwlink/?LinkId=202906#Setting_Values_For_Helpers).

1. Crie um novo site.
2. Adicione uma nova página chamada *EmailRequest. cshtml* e adicione a seguinte marcação: 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample1.html)]

    Observe que o atributo `action` do elemento form foi definido como *ProcessRequest. cshtml*. Isso significa que o formulário será enviado para essa página em vez de voltar para a página atual.
3. Adicione uma nova página chamada *ProcessRequest. cshtml* ao site e adicione o seguinte código e marcação:   

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample2.cshtml)]

    No código, você obtém os valores dos campos de formulário que foram enviados para a página. Em seguida, você chama o método `Send` do auxiliar de `WebMail` para criar e enviar a mensagem de email. Nesse caso, os valores a serem usados são compostos de texto que você concatena com os valores que foram enviados do formulário.

    O código desta página está dentro de um bloco de `try/catch`. Se por algum motivo a tentativa de enviar um email não funcionar (por exemplo, as configurações não estão corretas), o código no bloco de `catch` será executado e definirá a variável `errorMessage` como o erro que ocorreu. (Para obter mais informações sobre blocos de `try/catch` ou a marca de `<text>`, consulte [introdução à programação de páginas da Web do ASP.NET usando a sintaxe do Razor](https://go.microsoft.com/fwlink/?LinkID=251587#ID_HandlingErrors).)

    No corpo da página, se a variável `errorMessage` estiver vazia (o padrão), o usuário verá uma mensagem informando que a mensagem de email foi enviada. Se a variável de `errorMessage` for definida como true, o usuário verá uma mensagem informando que há um problema ao enviar a mensagem.

    Observe que, na parte da página que exibe uma mensagem de erro, há um teste adicional: `if(debuggingFlag)`. Essa é uma variável que você pode definir como true se você estiver tendo problemas para enviar email. Quando `debuggingFlag` for true e, se houver um problema ao enviar email, será exibida uma mensagem de erro adicional que mostra qualquer ASP.NET relatado quando ele tentou enviar a mensagem de email. Aviso justo, no entanto: as mensagens de erro que o ASP.NET relata quando não pode enviar uma mensagem de email podem ser genéricas. Por exemplo, se ASP.NET não puder entrar em contato com o servidor SMTP (por exemplo, porque você fez um erro no nome do servidor), o erro será `Failure sending mail`.

    > [!NOTE] 
    > 
    > **Importante** Quando você recebe uma mensagem de erro de um objeto de exceção (`ex` no código), *não* passe rotineiramente essa mensagem para os usuários. Objetos de exceção geralmente incluem informações que os usuários não devem ver e que podem até mesmo ser uma vulnerabilidade de segurança. É por isso que esse código inclui a variável `debuggingFlag` que é usada como uma opção para exibir a mensagem de erro e por que a variável, por padrão, é definida como false. Você deve definir essa variável como true (e, portanto, exibir a mensagem de erro) *somente* se você estiver tendo um problema com o envio de email e precisar Depurar. Depois de corrigir os problemas, defina `debuggingFlag` de volta para false.

    Modifique as seguintes configurações relacionadas a email no código:

   - Defina `your-SMTP-host` como o nome do servidor SMTP ao qual você tem acesso.
   - Defina `your-user-name-here` como o nome de usuário da sua conta do servidor SMTP.
   - Defina `your-account-password` para a senha da sua conta do servidor SMTP.
   - Defina `your-email-address-here` para seu próprio endereço de email. Esse é o endereço de email do qual a mensagem é enviada. (Alguns provedores de email não permitem que você especifique um endereço de `From` diferente e usará seu nome de usuário como o endereço de `From`.)

     > [!TIP] 
     > 
     > <a id="configuring_email_settings"></a>
     > ### <a name="configuring-email-settings"></a>Definindo configurações de email
     > 
     > Pode ser um desafio, às vezes, garantir que você tenha as configurações corretas para o servidor SMTP, o número da porta e assim por diante. Veja a seguir algumas dicas:
     > 
     > - O nome do servidor SMTP geralmente é algo como `smtp.provider.com` ou `smtp.provider.net`. No entanto, se você publicar seu site em um provedor de hospedagem, o nome do servidor SMTP nesse ponto poderá ser `localhost`. Isso ocorre porque depois de você ter publicado e seu site está em execução no servidor do provedor, o servidor de email pode ser local a partir da perspectiva do seu aplicativo. Essa alteração nos nomes de servidor pode significar que você precisa alterar o nome do servidor SMTP como parte do processo de publicação.
     > - O número da porta geralmente é 25. No entanto, alguns provedores exigem que você use a porta 587 ou alguma outra porta.
     > - Certifique-se de usar as credenciais corretas. Se você publicou seu site em um provedor de hospedagem, use as credenciais que o provedor indicou especificamente para email. Elas podem ser diferentes das credenciais que você usa para publicar.
     > - Às vezes, você não precisa de credenciais. Se você estiver enviando emails usando seu ISP pessoal, seu provedor de email talvez já conheça suas credenciais. Depois de publicar, talvez seja necessário usar credenciais diferentes do que quando você testar em seu computador local.
     > - Se seu provedor de email usar criptografia, você precisará definir `WebMail.EnableSsl` como `true`.
4. Execute a página *EmailRequest. cshtml* em um navegador. (Verifique se a página está selecionada no espaço de trabalho **arquivos** antes de executá-la.)
5. Insira seu nome e uma descrição do problema e, em seguida, clique no botão **Enviar** . Você é redirecionado para a página *ProcessRequest. cshtml* , que confirma sua mensagem e que envia uma mensagem de email. 

    ![[imagem]](11-adding-email-to-your-web-site/_static/image2.jpg)

<a id="Sending_a_File"></a>
## <a name="sending-a-file-using-email"></a>Enviando um arquivo usando email

Você também pode enviar arquivos anexados a mensagens de email. Neste procedimento, você cria um arquivo de texto e duas páginas HTML. Você usará o arquivo de texto como um anexo de email.

1. No site, adicione um novo arquivo de texto e nomeie-o como *MyFile. txt*.
2. Copie o texto a seguir e cole-o no arquivo: 

    `Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.`
3. Crie uma página chamada *SendFile. cshtml* e adicione a seguinte marcação: 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample3.html)]
4. Crie uma página chamada *ProcessFile. cshtml* e adicione a seguinte marcação: 

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample4.cshtml)]
5. Modifique as seguintes configurações relacionadas a email no código do exemplo:

    - Defina `your-SMTP-host` como o nome de um servidor SMTP ao qual você tem acesso.
    - Defina `your-user-name-here` como o nome de usuário da sua conta do servidor SMTP.
    - Defina `your-email-address-here` para seu próprio endereço de email. Esse é o endereço de email do qual a mensagem é enviada.
    - Defina `your-account-password` para a senha da sua conta do servidor SMTP.
    - Defina `target-email-address-here` para seu próprio endereço de email. (Como antes, você normalmente enviaria um email para outra pessoa, mas para teste, você pode enviá-lo para você mesmo.)
6. Execute a página *SendFile. cshtml* em um navegador.
7. Insira seu nome, uma linha de assunto e o nome do arquivo de texto a ser anexado (*MyFile. txt*).
8. Clique no botão `Submit`. Como antes, você é redirecionado para a página *ProcessFile. cshtml* , que confirma sua mensagem e que envia uma mensagem de email com o arquivo anexado.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais

- [Guia de solução de problemas de Páginas da Web do ASP.NET (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001)
- [Protocolo de transferência de email simples](https://msdn.microsoft.com/library/aa480435.aspx)
- [Personalizando o comportamento de todo o site para Páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202906)
