---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb
title: Processando exceções não tratadas (VB) | Microsoft Docs
author: rick-anderson
description: Quando ocorre um erro de tempo de execução em um aplicativo Web em produção, é importante notificar um desenvolvedor e registrar o erro para que ele possa ser diagnosticado em uma la...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: 051296f0-9519-4e78-835c-d868da13b0a0
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb
msc.type: authoredcontent
ms.openlocfilehash: 8d2e12af29bd95ce9898fa6a5e81be8b7a761f76
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78586061"
---
# <a name="processing-unhandled-exceptions-vb"></a>Processamento de exceções sem tratamento (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Exibir ou baixar código de exemplo](https://github.com/dotnet/AspNetDocs/tree/master/aspnet/web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb/samples) ([como baixar](/aspnet/core/tutorials/index#how-to-download-a-sample))

> Quando ocorre um erro de tempo de execução em um aplicativo Web em produção, é importante notificar um desenvolvedor e registrar o erro para que ele possa ser diagnosticado em um momento posterior. Este tutorial fornece uma visão geral de como o ASP.NET processa erros de tempo de execução e examina uma maneira de executar código personalizado sempre que uma exceção sem tratamento se assemelha ao tempo de execução do ASP.NET.

## <a name="introduction"></a>Introdução

Quando ocorre uma exceção sem tratamento em um aplicativo ASP.NET, ele se parece com o tempo de execução ASP.NET, que gera o evento `Error` e exibe a página de erro apropriada. Há três tipos diferentes de páginas de erro: a tela amarela de erro de tempo de execução de morte (YSOD); os detalhes da exceção YSOD; e páginas de erro personalizadas. No [tutorial anterior](displaying-a-custom-error-page-vb.md) , configuramos o aplicativo para usar uma página de erro personalizada para usuários remotos e os detalhes da exceção YSOD para os usuários que visitam localmente.

Usar uma página de erro personalizada amigável para o homem que corresponda à aparência do site é preferencial para o erro de tempo de execução padrão YSOD, mas a exibição de uma página de erro personalizada é apenas uma parte de uma solução de tratamento de erros abrangente. Quando ocorre um erro em um aplicativo em produção, é importante que os desenvolvedores sejam notificados sobre o erro para que eles possam revelaçãor a causa da exceção e solucioná-lo. Além disso, os detalhes do erro devem ser registrados para que o erro possa ser examinado e diagnosticado em um momento posterior.

Este tutorial mostra como acessar os detalhes de uma exceção sem tratamento para que elas possam ser registradas em log e um desenvolvedor seja notificado. Os dois tutoriais após esse um exploram bibliotecas de log de erros que, após um pouco de configuração, notificarão automaticamente os desenvolvedores de erros de tempo de execução e registrarão seus detalhes.

> [!NOTE]
> As informações examinadas neste tutorial serão mais úteis se você precisar processar exceções sem tratamento de alguma maneira exclusiva ou personalizada. Nos casos em que você só precisa registrar a exceção e notificar um desenvolvedor, usar uma biblioteca de log de erros é a melhor opção. Os próximos dois tutoriais fornecem uma visão geral de duas bibliotecas desse tipo.

## <a name="executing-code-when-theerrorevent-is-raised"></a>Executando o código quando o evento de`Error`é gerado

Os eventos fornecem um objeto um mecanismo para sinalizar que algo interessante ocorreu e para outro objeto executar código em resposta. Como desenvolvedor ASP.NET, você está acostumado a pensar em termos de eventos. Se você quiser executar algum código quando o visitante clicar em um botão específico, você criará um manipulador de eventos para o evento `Click` desse botão e colocará seu código lá. Considerando que o tempo de execução do ASP.NET gera seu [evento de`Error`](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx) sempre que ocorre uma exceção sem tratamento, ele segue que o código para registrar os detalhes do erro seria em um manipulador de eventos. Mas como você cria um manipulador de eventos para o evento `Error`?

O evento `Error` é um dos muitos eventos na [classe`HttpApplication`](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) que são gerados em determinados estágios no pipeline http durante o tempo de vida de uma solicitação. Por exemplo, o [evento de`BeginRequest`](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx) da classe `HttpApplication` é gerado no início de cada solicitação; seu [evento de`AuthenticateRequest`](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) é gerado quando um módulo de segurança identificou o solicitante. Esses `HttpApplication` eventos dão ao desenvolvedor da página um meio de executar lógica personalizada em vários pontos no tempo de vida de uma solicitação.

Os manipuladores de eventos para os eventos de `HttpApplication` podem ser colocados em um arquivo especial chamado `Global.asax`. Para criar esse arquivo em seu site, adicione um novo item à raiz do seu site usando o modelo de classe de aplicativo global com o nome `Global.asax`.

[![](processing-unhandled-exceptions-vb/_static/image2.png)](processing-unhandled-exceptions-vb/_static/image1.png)

**Figura 1**: Adicionar `Global.asax` ao seu aplicativo Web  
 ([Clique para exibir a imagem em tamanho normal](processing-unhandled-exceptions-vb/_static/image3.png))

O conteúdo e a estrutura do arquivo de `Global.asax` criado pelo Visual Studio diferem ligeiramente dependendo se você estiver usando um WAP (projeto de aplicativo Web) ou um WSP (projeto de site da Web). Com um WAP, a `Global.asax` é implementada como dois arquivos separados – `Global.asax` e `Global.asax.vb`. O arquivo de `Global.asax` não contém nada, exceto uma diretiva de `@Application` que faz referência ao arquivo de `.vb`; os manipuladores de eventos de interesse são definidos no arquivo `Global.asax.vb`. Para WSPs, apenas um único arquivo é criado, `Global.asax`e os manipuladores de eventos são definidos em um bloco de `<script runat="server">`.

O arquivo de `Global.asax` criado em um WAP pelo modelo de classe de aplicativo global do Visual Studio inclui manipuladores de eventos chamados `Application_BeginRequest`, `Application_AuthenticateRequest`e `Application_Error`, que são manipuladores de eventos para `HttpApplication` eventos `BeginRequest`, `AuthenticateRequest`e `Error`, respectivamente. Também há manipuladores de eventos chamados `Application_Start`, `Session_Start`, `Application_End`e `Session_End`, que são manipuladores de eventos que são acionados quando o aplicativo Web é iniciado, quando uma nova sessão é iniciada, quando o aplicativo termina e quando uma sessão termina, respectivamente. O arquivo de `Global.asax` criado em um WSP pelo Visual Studio contém apenas os manipuladores de eventos `Application_Error`, `Application_Start`, `Session_Start`, `Application_End`e `Session_End`.

> [!NOTE]
> Ao implantar o aplicativo ASP.NET, você precisa copiar o arquivo `Global.asax` para o ambiente de produção. O arquivo de `Global.asax.vb`, que é criado no WAP, não precisa ser copiado para produção porque esse código é compilado no assembly do projeto.

Os manipuladores de eventos criados pelo modelo de classe de aplicativo global do Visual Studio não são exaustivos. Você pode adicionar um manipulador de eventos para qualquer evento de `HttpApplication` ao nomear o manipulador de eventos `Application_EventName`. Por exemplo, você pode adicionar o seguinte código ao arquivo de `Global.asax` para criar um manipulador de eventos para o [evento`AuthorizeRequest`](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx):

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample1.vb)]

Da mesma forma, você pode remover todos os manipuladores de eventos criados pelo modelo de classe de aplicativo global que não são necessários. Para este tutorial, exigimos apenas um manipulador de eventos para o evento `Error`; Sinta-se à vontade para remover os outros manipuladores de eventos do arquivo `Global.asax`.

> [!NOTE]
> Os *módulos http* oferecem outra maneira de definir manipuladores de eventos para eventos de `HttpApplication`. Os módulos HTTP são criados como um arquivo de classe que pode ser colocado diretamente no projeto de aplicativo Web ou separado em uma biblioteca de classes separada. Como eles podem ser separados em uma biblioteca de classes, os módulos HTTP oferecem um modelo mais flexível e reutilizável para criar `HttpApplication` manipuladores de eventos. Enquanto o arquivo de `Global.asax` é específico para o aplicativo Web onde ele reside, os módulos HTTP podem ser compilados em assemblies. nesse ponto, adicionar o módulo HTTP a um site é tão simples quanto remover o assembly na pasta `Bin` e registrar o módulo no `Web.config`. Este tutorial não examina a criação e o uso de módulos HTTP, mas as duas bibliotecas de log de erros usadas nos dois tutoriais a seguir são implementadas como módulos HTTP. Para obter mais informações sobre os benefícios dos módulos HTTP [, consulte usando módulos e manipuladores HTTP para criar componentes ASP.net conectáveis](https://msdn.microsoft.com/library/aa479332.aspx).

## <a name="retrieving-information-about-the-unhandled-exception"></a>Recuperando informações sobre a exceção sem tratamento

Neste ponto, temos um arquivo global. asax com um manipulador de eventos `Application_Error`. Quando esse manipulador de eventos é executado, precisamos notificar um desenvolvedor sobre o erro e registrar seus detalhes. Para realizar essas tarefas, primeiro precisamos determinar os detalhes da exceção que foi gerada. Use o [método de`GetLastError`](https://msdn.microsoft.com/library/system.web.httpserverutility.getlasterror.aspx) do objeto de servidor para recuperar detalhes da exceção sem tratamento que fez com que o evento de `Error` fosse disparado.

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample2.vb)]

O método `GetLastError` retorna um objeto do tipo `Exception`, que é o tipo base para todas as exceções no .NET Framework. No entanto, no código acima, Estou convertendo o objeto de exceção retornado por `GetLastError` em um objeto `HttpException`. Se o evento `Error` estiver sendo disparado porque uma exceção foi lançada durante o processamento de um recurso ASP.NET, a exceção gerada será encapsulada em um `HttpException`. Para obter a exceção real que precipitou o evento de erro, use a propriedade `InnerException`. Se o evento `Error` foi gerado devido a uma exceção baseada em HTTP, como uma solicitação para uma página não existente, um `HttpException` é gerado, mas não tem uma exceção interna.

O código a seguir usa o `GetLastErrormessage` para recuperar informações sobre a exceção que disparou o evento `Error`, armazenando o `HttpException` em uma variável chamada `lastErrorWrapper`. Em seguida, ele armazena o tipo, a mensagem e o rastreamento de pilha da exceção de origem em três variáveis de cadeia de caracteres, verificando se o `lastErrorWrapper` é a exceção real que disparou o evento `Error` (no caso de exceções baseadas em HTTP) ou se é meramente um wrapper para uma exceção que foi lançada durante o processamento da solicitação.

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample3.vb)]

Neste ponto, você tem todas as informações necessárias para escrever um código que registrará os detalhes da exceção em uma tabela de banco de dados. Você pode criar uma tabela de banco de dados com colunas para cada um dos detalhes de erro de interesse – o tipo, a mensagem, o rastreamento de pilha e assim por diante, juntamente com outras informações úteis, como a URL da página solicitada e o nome do usuário conectado no momento. No manipulador de eventos `Application_Error`, você se conectaria ao banco de dados e inseriria um registro na tabela. Da mesma forma, você pode adicionar código para alertar um desenvolvedor do erro por email.

As bibliotecas de erros de log examinadas nos próximos dois tutoriais fornecem essa funcionalidade pronta para uso, portanto, não há necessidade de criar esse log de erros e notificação por conta própria. No entanto, para ilustrar que o evento de `Error` está sendo gerado e que o manipulador de eventos `Application_Error` pode ser usado para registrar detalhes de erros e notificar um desenvolvedor, vamos adicionar um código que notifique um desenvolvedor quando ocorrer um erro.

## <a name="notifying-a-developer-when-an-unhandled-exception-occurs"></a>Notificando um desenvolvedor quando ocorre uma exceção sem tratamento

Quando ocorre uma exceção sem tratamento no ambiente de produção, é importante alertar a equipe de desenvolvimento para que ela possa avaliar o erro e determinar quais ações precisam ser tomadas. Por exemplo, se houver um erro na conexão com o banco de dados, você precisará verificar a cadeia de conexão e, talvez, abrir um tíquete de suporte com sua empresa de hospedagem na Web. Se a exceção ocorreu devido a um erro de programação, talvez seja necessário adicionar código adicional ou lógica de validação para evitar esses erros no futuro.

As classes de .NET Framework no [namespace`System.Net.Mail`](https://msdn.microsoft.com/library/system.net.mail.aspx) facilitam o envio de um email. A [classe`MailMessage`](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx) representa uma mensagem de email e tem propriedades como `To`, `From`, `Subject`, `Body`e `Attachments`. O `SmtpClass` é usado para enviar um objeto de `MailMessage` usando um servidor SMTP especificado; as configurações do servidor SMTP podem ser especificadas programaticamente ou declarativamente no [elemento`<system.net>`](https://msdn.microsoft.com/library/6484zdc1.aspx) no `Web.config file`. Para obter mais informações sobre como enviar mensagens de email em um aplicativo ASP.NET, Confira meu artigo, [enviando email em ASP.net](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)e as [perguntas frequentes sobre o System .net. mail](http://systemnetmail.com/).

> [!NOTE]
> O elemento `<system.net>` contém as configurações do servidor SMTP usadas pela classe `SmtpClient` ao enviar um email. Provavelmente, sua empresa de hospedagem na Web tem um servidor SMTP que você pode usar para enviar emails do seu aplicativo. Consulte a seção de suporte do seu host Web para obter informações sobre as configurações do servidor SMTP que você deve usar em seu aplicativo Web.

Adicione o seguinte código ao manipulador de eventos `Application_Error` para enviar um email ao desenvolvedor quando ocorrer um erro:

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample4.vb)]

Embora o código acima seja bastante longo, a maior parte dele cria o HTML que aparece no email enviado ao desenvolvedor. O código começa referenciando o `HttpException` retornado pelo método `GetLastError` (`lastErrorWrapper`). A exceção real que foi gerada pela solicitação é recuperada por meio de `lastErrorWrapper.InnerException` e é atribuída à variável `lastError`. As informações de rastreamento de tipo, mensagem e pilha são recuperadas de `lastError` e armazenadas em três variáveis de cadeia de caracteres.

Em seguida, um objeto `MailMessage` chamado `mm` é criado. O corpo do email é formatado em HTML e exibe a URL da página solicitada, o nome do usuário conectado no momento e as informações sobre a exceção (o tipo, a mensagem e o rastreamento de pilha). Uma das coisas legais sobre a classe de `HttpException` é que você pode gerar o HTML usado para criar a exceção detalhes na tela amarela de morte (YSOD) chamando o [método GetHtmlErrorMessage](https://msdn.microsoft.com/library/system.web.httpexception.gethtmlerrormessage.aspx). Esse método é usado aqui para recuperar a marcação YSOD de detalhes da exceção e adicioná-la ao email como um anexo. Uma palavra de cuidado: se a exceção que disparou o evento de `Error` fosse uma exceção baseada em HTTP (como uma solicitação de uma página não existente), o método de `GetHtmlErrorMessage` retornará `null`.

A etapa final é enviar o `MailMessage`. Isso é feito criando um novo método `SmtpClient` e chamando seu método `Send`.

> [!NOTE]
> Antes de usar esse código em seu aplicativo Web, você deverá alterar os valores nas constantes `ToAddress` e `FromAddress` de support@example.com para qualquer endereço de email para o qual o email de notificação de erro deve ser enviado e originado. Você também precisará especificar as configurações do servidor SMTP na seção `<system.net>` em `Web.config`. Consulte seu provedor de host da Web para determinar as configurações do servidor SMTP a serem usadas.

Com esse código em vigor sempre que houver um erro, o desenvolvedor receberá uma mensagem de email que resume o erro e inclui o YSOD. No tutorial anterior, demonstramos um erro de tempo de execução visitando o gênero. aspx e passando um valor de `ID` inválido por meio de QueryString, como `Genre.aspx?ID=foo`. Visitar a página com o arquivo de `Global.asax` no local produz a mesma experiência de usuário do tutorial anterior – no ambiente de desenvolvimento, você continuará vendo a exceção detalhes da tela amarela de morte, enquanto no ambiente de produção você verá a página de erro personalizada. Além desse comportamento existente, o desenvolvedor recebe um email.

A **Figura 2** mostra o email recebido ao visitar `Genre.aspx?ID=foo`. O corpo do email resume as informações de exceção, enquanto o anexo de `YSOD.htm` exibe o conteúdo mostrado nos detalhes da exceção YSOD (consulte a **Figura 3**).

[![](processing-unhandled-exceptions-vb/_static/image5.png)](processing-unhandled-exceptions-vb/_static/image4.png)

**Figura 2**: o desenvolvedor recebe uma notificação por email sempre que há uma exceção sem tratamento  
 ([Clique para exibir a imagem em tamanho normal](processing-unhandled-exceptions-vb/_static/image6.png))

[![](processing-unhandled-exceptions-vb/_static/image8.png)](processing-unhandled-exceptions-vb/_static/image7.png)

**Figura 3**: a notificação por email inclui os detalhes da exceção YSOD como um anexo  
 ([Clique para exibir a imagem em tamanho normal](processing-unhandled-exceptions-vb/_static/image9.png))

## <a name="what-about-using-the-custom-error-page"></a>E quanto ao uso da página de erro personalizada?

Este tutorial mostrou como usar `Global.asax` e o manipulador de eventos `Application_Error` para executar código quando ocorre uma exceção sem tratamento. Especificamente, usamos esse manipulador de eventos para notificar um desenvolvedor sobre um erro; Poderíamos estendê-lo para registrar também os detalhes do erro em um banco de dados. A presença do manipulador de eventos `Application_Error` não afeta a experiência do usuário final. Eles ainda veem a página de erro configurada, são os detalhes do erro YSOD, o erro de tempo de execução YSOD ou a página de erro personalizada.

É natural imaginar se o arquivo de `Global.asax` e `Application_Error` evento são necessários ao usar uma página de erro personalizada. Quando ocorre um erro, o usuário é mostrado na página de erro personalizada, por que não podemos colocar o código para notificar o desenvolvedor e registrar os detalhes do erro na classe code-behind da página de erro personalizada? Embora você certamente possa adicionar código à classe code-behind da página de erro personalizada, você não tem acesso aos detalhes da exceção que disparou o evento de `Error` ao usar a técnica que exploramos no tutorial anterior. Chamar o método `GetLastError` da página de erro personalizada retorna `Nothing`.

O motivo para esse comportamento é porque a página de erro personalizada é atingida por meio de um redirecionamento. Quando uma exceção sem tratamento alcança o tempo de execução ASP.NET, o mecanismo de ASP.NET gera seu evento de `Error` (que executa o manipulador de eventos `Application_Error`) e *redireciona* o usuário para a página de erro personalizada emitindo um `Response.Redirect(customErrorPageUrl)`. O método `Response.Redirect` envia uma resposta ao cliente com um código de status HTTP 302, instruindo o navegador a solicitar uma nova URL, ou seja, a página de erro personalizada. Em seguida, o navegador solicita automaticamente essa nova página. Você pode dizer que a página de erro personalizada foi solicitada separadamente da página em que o erro foi originado porque a barra de endereços do navegador muda para a URL da página de erro personalizada (consulte a **Figura 4**).

[![](processing-unhandled-exceptions-vb/_static/image11.png)](processing-unhandled-exceptions-vb/_static/image10.png)

**Figura 4**: quando ocorre um erro, o navegador é redirecionado para a URL da página de erro personalizada  
 ([Clique para exibir a imagem em tamanho normal](processing-unhandled-exceptions-vb/_static/image12.png))

O efeito líquido é que a solicitação em que a exceção não tratada ocorreu termina quando o servidor responde com o redirecionamento HTTP 302. A solicitação subsequente para a página de erro personalizada é uma solicitação totalmente nova; Nesse ponto, o mecanismo de ASP.NET descartou as informações de erro e, além disso, não tem como associar a exceção sem tratamento na solicitação anterior com a nova solicitação para a página de erro personalizada. É por isso que `GetLastError` retorna `null` quando chamado na página de erro personalizada.

No entanto, é possível que a página de erro personalizada seja executada durante a mesma solicitação que causou o erro. O método [`Server.Transfer(url)`](https://msdn.microsoft.com/library/system.web.httpserverutility.transfer.aspx) transfere a execução para a URL especificada e a processa na mesma solicitação. Você pode mover o código no manipulador de eventos `Application_Error` para a classe code-behind da página de erro personalizada, substituindo-o em `Global.asax` pelo seguinte código:

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample5.vb)]

Agora, quando uma exceção sem tratamento ocorre, o manipulador de eventos `Application_Error` transfere o controle para a página de erro personalizada apropriada com base no código de status HTTP. Como o controle foi transferido, a página de erro personalizada tem acesso às informações de exceção sem tratamento por meio de `Server.GetLastError` e pode notificar um desenvolvedor do erro e registrar seus detalhes. A chamada de `Server.Transfer` interrompe o mecanismo ASP.NET de redirecionar o usuário para a página de erro personalizada. Em vez disso, o conteúdo da página de erro personalizada é retornado como a resposta à página que gerou o erro.

## <a name="summary"></a>Resumo

Quando ocorre uma exceção sem tratamento em um aplicativo Web ASP.NET, o tempo de execução do ASP.NET gera o evento `Error` e exibe a página de erro configurada. Podemos notificar o desenvolvedor do erro, registrar seus detalhes ou processá-lo de alguma outra maneira, criando um manipulador de eventos para o evento de erro. Há duas maneiras de criar um manipulador de eventos para `HttpApplication` eventos como `Error`: no arquivo `Global.asax` ou em um módulo HTTP. Este tutorial mostrou como criar um manipulador de eventos de `Error` no arquivo de `Global.asax` que notifica os desenvolvedores de um erro por meio de uma mensagem de email.

Criar um manipulador de eventos de `Error` será útil se você precisar processar exceções sem tratamento de alguma maneira exclusiva ou personalizada. No entanto, criar seu próprio manipulador de eventos `Error` para registrar a exceção ou notificar um desenvolvedor não é o uso mais eficiente do seu tempo, pois já existe gratuitamente e fácil usar bibliotecas de log de erros que podem ser configuradas em questão de minutos. Os dois próximos tutoriais examinam duas bibliotecas desse tipo.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Visão geral de módulos HTTP ASP.NET e manipuladores HTTP](https://support.microsoft.com/kb/307985)
- [Respondendo normalmente a exceções não tratadas – processando exceções sem tratamento](http://aspnet.4guysfromrolla.com/articles/091306-1.aspx)
- [Classe `HttpApplication` e o objeto de aplicativo ASP.NET](http://www.eggheadcafe.com/articles/20030211.asp)
- [Manipuladores HTTP e módulos HTTP no ASP.NET](http://www.15seconds.com/Issue/020417.htm)
- [Enviando email no ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [Noções básicas sobre o arquivo de `Global.asax`](http://aspalliance.com/1114_Understanding_the_Globalasax_file.all)
- [Usando módulos e manipuladores HTTP para criar componentes ASP.NET conectáveis](https://msdn.microsoft.com/library/aa479332.aspx)
- [Trabalhando com o arquivo de `Global.asax` ASP.NET](http://articles.techrepublic.com.com/5100-10878_11-5771721.html)
- [Trabalhando com instâncias de `HttpApplication`](https://msdn.microsoft.com/library/a0xez8f2.aspx)

> [!div class="step-by-step"]
> [Anterior](displaying-a-custom-error-page-vb.md)
> [Próximo](logging-error-details-with-asp-net-health-monitoring-vb.md)
