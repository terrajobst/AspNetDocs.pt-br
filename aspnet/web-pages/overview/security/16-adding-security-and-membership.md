---
uid: web-pages/overview/security/16-adding-security-and-membership
title: Adicionando segurança e associação a um site Páginas da Web do ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Este capítulo mostra como proteger seu site para que algumas das páginas estejam disponíveis apenas para as pessoas que fazem logon. (Você também verá como criar páginas tha...
ms.author: riande
ms.date: 02/24/2014
ms.assetid: 7a77c2c0-deea-4290-a9c3-97958891758e
msc.legacyurl: /web-pages/overview/security/16-adding-security-and-membership
msc.type: authoredcontent
ms.openlocfilehash: 0be3e767a42939a3c343f6d4a730eb1d9a6b367c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628383"
---
# <a name="adding-security-and-membership-to-an-aspnet-web-pages-razor-site"></a>Adicionando segurança e associação a um site Páginas da Web do ASP.NET (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo explica como proteger um site Páginas da Web do ASP.NET (Razor) para que algumas das páginas estejam disponíveis apenas para as pessoas que fazem logon. (Você também verá como criar páginas que qualquer pessoa pode acessar.)
> 
> **O que você aprenderá:** 
> 
> - Como criar um site que tenha uma página de registro e uma página de logon para que, para algumas páginas, você possa limitar o acesso somente a membros.
> - Como criar páginas públicas e somente para membros.
> - Como definir funções, que são grupos que têm permissões de segurança diferentes em seu site e como atribuir usuários a uma função.
> - Como usar o CAPTCHA para impedir que os bots (programas automatizados) criem contas de membros.
>   
> 
> Estes são os recursos do ASP.NET apresentados no artigo:
> 
> - O modelo de **site inicial** do WebMatrix.
> - O auxiliar de `WebSecurity` e a classe `Roles`.
> - O auxiliar de `ReCaptcha`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 2
> - WebMatrix 3
> - Biblioteca de auxiliares Web do ASP.NET

Você pode configurar seu site para que os usuários possam fazer logon nele &#8212; , para que o site ofereça suporte à *Associação*. Isso pode ser útil por vários motivos. Por exemplo, seu site pode ter páginas que devem estar disponíveis somente para membros. Em alguns casos, você pode exigir que os usuários façam logon para enviar comentários ou deixar um comentário.

Mesmo que seu site dê suporte à associação, os usuários não precisam fazer logon antes de usarem algumas das páginas no site. Os usuários que não estão conectados são conhecidos como *usuários anônimos*.

Um usuário pode se registrar em seu site e, em seguida, pode fazer logon no site. O site requer um nome de usuário (um endereço de email) e uma senha para confirmar se os usuários são aqueles que afirmam ser. Esse processo de fazer logon e confirmar a identidade de um usuário é conhecido como *autenticação*.

Você pode configurar a segurança e a associação de maneiras diferentes:

- Se você estiver usando o WebMatrix, uma maneira fácil é criar como um novo site com base no modelo de **site inicial** . Este modelo já está configurado para segurança e associação e já tem uma página de registro, uma página de logon e assim por diante.

    O site criado pelo modelo também tem uma opção para permitir que os usuários façam logon usando um site externo, como Facebook, Google ou Twitter.
- Se você quiser adicionar segurança a um site existente ou se não quiser usar o modelo de **site inicial** , poderá criar sua própria página de registro, página de logon e assim por diante.

Este artigo se concentra na primeira opção &mdash; como adicionar segurança usando o modelo de **site inicial** . Ele também fornece algumas informações básicas sobre como implementar sua própria segurança e fornece links para mais informações sobre como fazer isso. Também há informações sobre como habilitar logons externos, que são descritos com mais detalhes em um artigo separado.

## <a name="creating-website-security-using-the-starter-site-template"></a>Criando a segurança do site usando o modelo de site inicial

No WebMatrix, você pode usar o modelo de **site inicial** para criar um site que contenha o seguinte:

- Um banco de dados que é usado para armazenar nomes de usuário e senhas para seus membros.
- Uma página de registro onde os usuários anônimos (novos) podem se registrar.
- Uma página de logon e logout.
- Uma página de recuperação e redefinição de senha.

O procedimento a seguir descreve como criar o site e configurá-lo.

1. Inicie o WebMatrix e, na página **início rápido** , selecione **site do modelo**.
2. Selecione o modelo de **site inicial** e clique em **OK**. O WebMatrix cria um novo site.
3. No painel esquerdo, clique no seletor de espaço de trabalho **arquivos** .
4. Na pasta raiz do seu site, abra o arquivo *\_AppStart. cshtml* , que é um arquivo especial que é usado para conter configurações globais. Ele contém algumas instruções que são comentadas usando os caracteres `//`:

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample1.cs)]

    Essas instruções configuram o auxiliar de `WebMail`, que pode ser usado para enviar email. O sistema de associação pode usar o email para enviar mensagens de confirmação quando os usuários se registram ou quando desejam alterar suas senhas. (Por exemplo, depois que os usuários se registram, eles recebem um email que inclui um link no qual eles podem clicar para concluir o processo de registro.)

    O envio de email requer acesso a um servidor SMTP, conforme descrito em [adicionando email a um Site páginas da Web do ASP.net](https://go.microsoft.com/fwlink/?LinkId=202899). Você armazenará as configurações de email nessa central *\_arquivo AppStart. cshtml* para que você não precise codifique-los repetidamente em cada página que possa enviar emails. (Você não precisa definir as configurações de SMTP para configurar um banco de dados de registro; você só precisará de configurações de SMTP se quiser validar os usuários de seu alias de email e permitir que os usuários redefinam uma senha esquecida.)
5. Remova os comentários das instruções removendo `//` de na frente de cada uma delas.

    Se você não quiser configurar a confirmação de email, poderá ignorar esta etapa e a próxima etapa. Se os valores SMTP não estiverem definidos, a nova conta estará imediatamente disponível sem um email de confirmação.
6. Modifique as seguintes configurações relacionadas a email no código:

   - Defina `WebMail.SmtpServer` como o nome do servidor SMTP ao qual você tem acesso.
   - Deixe `WebMail.EnableSsl` definido como `true`. Essa configuração protege as credenciais que são enviadas ao servidor SMTP criptografando-as.
   - Defina `WebMail.UserName` como o nome de usuário da sua conta do servidor SMTP.
   - Defina `WebMail.Password` para a senha da sua conta do servidor SMTP.
   - Defina `WebMail.From` para seu próprio endereço de email. Esse é o endereço de email do qual a mensagem é enviada.

     > [!NOTE] 
     > 
     > **Dica** Para obter informações adicionais sobre os valores para essas propriedades, consulte [definindo configurações de email](https://go.microsoft.com/fwlink/?LinkID=202906#configuring_email_settings) em [Personalizando o comportamento de todo o site para páginas da Web do ASP.net](https://go.microsoft.com/fwlink/?LinkID=202906).
7. Salve e feche *\_AppStart. cshtml*.
8. Execute a página *Default. cshtml* em um navegador.

    ![segurança-Associação-2](16-adding-security-and-membership/_static/image1.png)

   > [!NOTE]
   > Se você vir um erro que informa que uma propriedade deve ser uma instância de `ExtendedMembershipProvider`, o site pode não estar configurado para usar o sistema de associação de Páginas da Web do ASP.NET (SimpleMembership). Às vezes, isso pode ocorrer se o servidor de um provedor de hospedagem estiver configurado de forma diferente do servidor local. Para corrigir isso, adicione o seguinte elemento ao arquivo *Web. config* do site:
   > 
   > [!code-xml[Main](16-adding-security-and-membership/samples/sample2.xml)]
   > 
   > Adicione esse elemento como um filho do elemento `<configuration>` e como um par do elemento `<system.web>`.
9. No canto superior direito da página, clique no link **registrar** . A página *Register. cshtml* é exibida.
10. Insira um nome de usuário e uma senha e, em seguida, clique em **registrar**.

    ![segurança-Associação-3](16-adding-security-and-membership/_static/image2.png)

    Quando você criou o site do modelo de **site inicial** , um banco de dados chamado *StarterSite. sdf* foi criado na pasta de *dados do aplicativo\_* do site. Durante o registro, as informações do usuário são adicionadas ao banco de dados. Se você definir os valores SMTP, uma mensagem será enviada para o endereço de email usado para que você possa concluir o registro.

    ![segurança-Associação-4](16-adding-security-and-membership/_static/image3.png)
11. Acesse seu programa de email e localize a mensagem, que terá seu código de confirmação e um hiperlink para o site.
12. Clique no hiperlink para ativar sua conta. O hiperlink de confirmação abre uma página de confirmação de registro.

    ![segurança-Associação-5](16-adding-security-and-membership/_static/image4.png)
13. Clique no link **logon** e faça logon usando a conta que você registrou.

      Depois de fazer logon, os links de **logon** e de **registro** são substituídos por um link de **logoff** . Seu nome de logon é exibido como um link. (O link permite que você vá para uma página onde você pode alterar sua senha.)

      ![segurança-Associação-6](16-adding-security-and-membership/_static/image5.png)

      > [!NOTE]
      > Por padrão, as páginas da Web do ASP.NET enviam credenciais para o servidor em texto não criptografado (como texto legível). Um site de produção deve usar HTTP seguro (https://, também conhecido como *SSL)* para criptografar informações confidenciais que são trocadas com o servidor. Você pode exigir que as mensagens de email sejam enviadas usando SSL, definindo `WebMail.EnableSsl=true` como no exemplo anterior. Para obter mais informações sobre SSL, consulte [Securing Web Communications: Certificates, SSL e https://](https://go.microsoft.com/fwlink/?LinkId=208660).

## <a name="additional-membership-functionality-in-the-site"></a>Funcionalidade de associação adicional no site

Seu site contém outras funcionalidades que permitem que os usuários gerenciem suas contas. Os usuários podem fazer o seguinte:

- Altere suas senhas. Depois de fazer logon, eles podem clicar no nome de usuário (que é um link). Isso leva para uma página em que é possível criar uma nova senha (*Account/ChangePassword. cshtml*).
- Recuperar uma senha esquecida. Na página de logon, há um link (**você esqueceu sua senha?** ) que leva os usuários para uma página (*Account/ForgotPassword. cshtml*), em que eles podem inserir um endereço de email. O site envia a eles uma mensagem de email com um link no qual eles podem clicar para definir uma nova senha (*Account/PasswordReset. cshtml*).

Você também pode permitir que os usuários também possam fazer logon usando um site externo, conforme explicado posteriormente.

<a id="Creating_a_Members-Only_Page"></a>
## <a name="creating-a-members-only-page"></a>Criando uma página somente de membros

Por enquanto, qualquer pessoa pode navegar para qualquer página no seu site. Mas talvez você queira ter páginas que estão disponíveis apenas para pessoas que fizeram logon (ou seja, membros). O ASP.NET permite que você crie páginas que podem ser acessadas somente por membros conectados. Normalmente, se os usuários anônimos tentarem acessar uma página somente de membros, você os redirecionará para a página de logon.

Neste procedimento, você criará uma pasta que conterá páginas que estão disponíveis somente para usuários conectados.

1. Na raiz do site, crie uma nova pasta. (Na faixa de faixas, clique na seta abaixo de **novo** e escolha **nova pasta**.)
2. Nomeie os novos *Membros*da pasta.
3. Dentro da pasta *Membros* , crie uma nova página e a denominada *MembersInformation. cshtml*.
4. Substitua o conteúdo existente pelo seguinte código e marcação:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample3.cshtml)]

    Esse código testa a propriedade `IsAuthenticated` do objeto `WebSecurity`, que retorna `true` se o usuário fez logon. Se o usuário não estiver conectado, o código chamará `Response.Redirect` para enviar o usuário para a página *login. cshtml* na pasta *Account* .

    A URL do redirecionamento inclui um `returnUrl` valor de cadeia de caracteres de consulta que usa `Request.Url.LocalPath` para definir o caminho da página atual. Se você definir o valor de `returnUrl` na cadeia de caracteres de consulta como esta (e se a URL de retorno for um caminho local), a página de logon retornará os usuários para esta página depois que fizerem logon.

    O código também define *\_página SiteLayout. cshtml* como sua página de layout. (Para obter mais informações sobre páginas de layout, consulte [criando um layout consistente em Sites páginas da Web do ASP.net](https://go.microsoft.com/fwlink/?LinkId=202891).)
5. Execute o site. Se você ainda estiver conectado, clique no botão **logout** na parte superior da página.
6. No navegador, solicite a página */Members/MembersInformation*. Por exemplo, a URL pode ter a seguinte aparência:

    `http://localhost:38366/Members/MembersInformation`

    (O número da porta (38366) provavelmente será diferente em sua URL.)

    Você será redirecionado para a página *login. cshtml* , porque não está conectado.
7. Faça logon usando a conta que você criou anteriormente. Você será Redirecionado de volta para a página *MembersInformation* . Como você está conectado, desta vez você verá o conteúdo da página.

Para proteger o acesso a várias páginas, você pode fazer isso:

- Adicione a verificação de segurança a cada página.
- Crie uma página *\_PageStart. cshtml* na pasta onde você mantém as páginas protegidas e adicione a verificação de segurança lá. A página *\_PageStart. cshtml* atua como um tipo de página global para todas as páginas na pasta. Essa técnica é explicada em mais detalhes na [personalização do comportamento de todo o site para páginas da Web do ASP.net](https://go.microsoft.com/fwlink/?LinkId=202906#Using__PageStart.cshtml_to_Restrict_Folder_Access).

## <a name="creating-security-for-groups-of-users-roles"></a>Criando segurança para grupos de usuários (funções)

Se seu site tiver muitos membros, não será eficiente verificar a permissão para cada usuário individualmente antes de deixá-los ver uma página. O que você pode fazer em vez disso é criar grupos ou *funções*aos quais os membros individuais pertencem. Em seguida, você pode verificar as permissões com base na função. Nesta seção, você criará uma função de&quot; &quot;admin e, em seguida, criará uma página acessível aos usuários que estão no (que pertencem a) essa função.

O sistema de associação do ASP.NET é configurado para dar suporte a funções. No entanto, ao contrário do registro de associação e do logon, o modelo de **site inicial** não contém páginas que ajudam a gerenciar funções. (O gerenciamento de funções é uma tarefa administrativa em vez de uma tarefa de usuário.) No entanto, você pode adicionar grupos diretamente no banco de dados de associação no WebMatrix.

1. No WebMatrix, clique no seletor espaço de trabalho **bancos de dados** .
2. No painel esquerdo, abra o nó *StarterSite. sdf* , abra o nó **tabelas** e clique duas vezes na tabela de *funções\_de páginas da Web* .

    ![segurança-Associação-7](16-adding-security-and-membership/_static/image6.png)
3. Adicione uma função chamada&quot;do &quot;admin. O campo *RoleID* é preenchido automaticamente. (É a chave primária e foi definido para ser um campo de identificação, conforme explicado em [introdução ao trabalho com um banco de dados em Sites páginas da Web do ASP.net](https://go.microsoft.com/fwlink/?LinkId=202893).)
4. Anote o que é o valor para o campo *RoleID* . (Se esta for a primeira função que você está definindo, será 1.)

    ![segurança-Associação-8](16-adding-security-and-membership/_static/image7.png)
5. Feche a tabela de *funções de\_de páginas da Web* .
6. Abra a tabela *USERPROFILE* .
7. Anote o valor de *userid* de um ou mais dos usuários na tabela e, em seguida, feche a tabela.
8. Abra as *páginas da web\_tabela UserInRoles* e insira um *userid* e um valor *RoleID* na tabela. Por exemplo, para colocar o usuário 2 na função de&quot; &quot;admin, você deve inserir esses valores:

    ![segurança-Associação-9](16-adding-security-and-membership/_static/image8.png)
9. Feche as *páginas da web\_tabela UsersInRoles* .

    Agora que você tem funções definidas, é possível configurar uma página que seja acessível aos usuários que estão nessa função.
10. Na pasta raiz do site, crie uma nova página chamada *AdminError. cshtml* e substitua o conteúdo existente pelo código a seguir. Essa será a página para a qual os usuários serão redirecionados se não tiverem permissão de acesso a uma página.

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample4.cshtml)]
11. Na pasta raiz do site, crie uma nova página chamada *AdminOnly. cshtml* e substitua o código existente pelo código a seguir:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample5.cshtml)]

    O método `Roles.IsUserInRole` retorna `true` se o usuário atual for membro da função especificada (nesse caso, a função "admin").
12. Execute *Default. cshtml* em um navegador, mas não faça logon. (Se você já estiver conectado, faça logoff.)
13. Na barra de endereços do navegador, adicione *AdminOnly* na URL. (Em outras palavras, solicite o arquivo *AdminOnly. cshtml* .) Você será redirecionado para a página *AdminError. cshtml* , porque você não está conectado atualmente como um usuário na função de&quot; administrador do &quot;.
14. Retorne para *Default. cshtml* e faça logon como o usuário que você adicionou à função de&quot; admin do &quot;.
15. Navegue até a página *AdminOnly. cshtml* . Desta vez, você verá a página.

## <a name="preventing-automated-programs-from-joining-your-website"></a>Impedir que programas automatizados ingressem no seu site

A página de logon não irá parar os programas automatizados (às vezes chamados de *robôs da Web* ou *bots*) de se registrarem no site. Este procedimento descreve como habilitar um teste do ReCaptcha para a página de registro.

![/media/38777/ch16securitymembership-18.jpg](16-adding-security-and-membership/_static/image1.jpg)

1. Registre seu site em ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)). Quando você concluir o registro, obterá uma chave pública e uma chave privada.
2. Adicione a biblioteca de auxiliares Web do ASP.NET ao seu site, conforme descrito em [instalando auxiliares em um páginas da Web do ASP.net site](https://go.microsoft.com/fwlink/?LinkId=252372), se ainda não tiver feito isso.
3. Na pasta *conta* , abra o arquivo chamado *Register. cshtml*.
4. No código na parte superior da página, localize as linhas a seguir e remova os comentários removendo o `//` caracteres de comentário:

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample6.cs)]
5. Substitua `PRIVATE_KEY` pela sua própria chave privada ReCaptcha.
6. Na marcação da página, remova as `@*` e `*@` comentários de comentário das linhas a seguir na marcação de página:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample7.cshtml)]
7. Substitua `PUBLIC_KEY` pela sua chave.
8. Se você ainda não o tiver removido, remova o elemento `<div>` que contém o texto que começa com "para habilitar a verificação de CAPTCHA...". (Remova todo o elemento `<div>` e seu conteúdo.)

9. Execute *Default. cshtml* em um navegador. Se você estiver conectado ao site, clique no link **logout** .
10. Clique no link **registrar** e teste o registro usando o teste captcha.

     ![segurança-Associação-10](16-adding-security-and-membership/_static/image9.png)

Para obter mais informações sobre o auxiliar de `ReCaptcha`, consulte [usando um CATPCHA para impedir que os bots (programas automatizados) usem seu site do ASP.net](https://go.microsoft.com/fwlink/?LinkId=251967).

<a id="Additional_Resources"></a>
## <a name="letting-users-log-in-using-an-external-site"></a>Permitindo que os usuários façam logon usando um site externo

O modelo de **site inicial** inclui código e marcação que permite que os usuários façam logon usando o Facebook, o Windows Live, o Twitter, o Google ou o Yahoo. Por padrão, essa funcionalidade não está habilitada. O procedimento geral para usar permitir que os usuários façam logon usando esses provedores externos é o seguinte:

- Decida quais sites externos você deseja dar suporte.
- Se necessário, vá para esse site e configure um aplicativo de logon. (Por exemplo, você precisa fazer isso para permitir logons do Facebook.)
- Em seu site, configure o provedor. Na maioria dos casos, basta remover o comentário de um código no arquivo *\_AppStart. cshtml* .
- Adicione marcação à página de registro que permite que as pessoas vinculem o site externo para fazer logon. Normalmente, você pode copiar a marcação de que precisa e alterar o texto um pouco.

Você pode encontrar instruções passo a passo no tópico [habilitando o logon de sites externos em um páginas da Web do ASP.net site](https://go.microsoft.com/fwlink/?LinkId=251969).

Depois que um usuário faz logon de outro site, o usuário retorna ao seu site e *associa* esse logon ao seu site. Em vigor, isso cria uma entrada de associação em seu site para o logon externo do usuário. Isso permite que você use os recursos normais de associação (como funções) com o logon externo.

## <a name="adding-security-to-an-existing-website"></a>Adicionando segurança a um site da Web existente

O procedimento anterior neste artigo depende do uso do modelo de **site inicial** como base para a segurança do site. Se não for prático iniciar a partir do modelo de **site inicial** ou copiar as páginas relevantes de um site com base nesse modelo, você poderá implementar o mesmo tipo de segurança em seu próprio site codificando-o por conta própria. Você cria os mesmos tipos de páginas – registro, logon e assim por diante — e, em seguida, usa auxiliares e classes para configurar a associação.

O processo básico é descrito na postagem [do blog a maneira mais básica de implementar a segurança do ASP.net Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240). A maior parte do trabalho é feita usando os seguintes métodos e propriedades do auxiliar de `WebSecurity`:

- [WebSecurty. Userexistent](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.userexists(v=vs.99).aspx), [websecurity. CreateUserAndAccount](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.createuserandaccount(v=vs.99).aspx). Esses métodos permitem determinar se alguém já está registrado e registrá-los.
- [WebSecurty. IsAuthenticated](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.isauthenticated(v=vs.99).aspx). Essa propriedade permite determinar se o usuário atual está conectado. Isso será útil para redirecionar os usuários para uma página de logon se eles ainda não tiverem feito logon.
- [Websecurity. logon](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.login(v=vs.99).aspx), [websecurity. logout](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.logout(v=vs.99).aspx). Esses métodos fazem logon ou saída de um usuário.
- [Websecurity. currentusername](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.currentusername(v=vs.99).aspx). Essa propriedade é útil para exibir o nome de logon do usuário atual (se o usuário estiver conectado).
- [Websecurity. ConfirmAccount](https://msdn.microsoft.com/library/gg569286(v=vs.99).aspx). Esse método será útil se você configurar a confirmação de email para o registro. (Os detalhes são descritos na postagem do blog [usando o recurso de confirmação para páginas da Web do ASP.net segurança](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267).)

Para gerenciar funções, você pode usar as [funções](https://msdn.microsoft.com/library/gg538398(v=vs.99).aspx) e as classes de [Associação](https://msdn.microsoft.com/library/gg569035(v=vs.99).aspx) , conforme descrito na entrada do blog.

## <a name="additional-resources"></a>Recursos adicionais

- [Personalizar o comportamento de todo o site](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Protegendo as comunicações da Web: certificados, SSL e https://](https://go.microsoft.com/fwlink/?LinkId=208660)
- [A maneira mais básica de implementar a segurança do ASP.net Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240) e [usar o recurso de confirmação para páginas da Web do ASP.net segurança](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267). Essas Postagens de blog descrevem como implementar recursos de associação do ASP.NET sem usar o modelo de **site inicial** .
- [Habilitar logon de sites externos em um site de Páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251969)
- [Referência de API de classe websecurity](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity(v=vs.99)) (MSDN)
- [Referência de API de classe SimpleRoleProvider](https://msdn.microsoft.com/library/webmatrix.webdata.simpleroleprovider(v=vs.99)) (MSDN)
- [Referência de API de classe SimpleMembershipProvider](https://msdn.microsoft.com/library/webmatrix.webdata.simplemembershipprovider(v=vs.99)) (MSDN)
