---
uid: web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
title: Guia de solução de problemas do Páginas da Web do ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Este artigo descreve os problemas que você pode ter ao trabalhar com Páginas da Web do ASP.NET (Razor) e algumas soluções sugeridas. Versões de software ASP.NET Web pag...
ms.author: riande
ms.date: 02/10/2014
ms.assetid: 2a2c1833-0bfe-4e2e-9cc0-341b52c7b121
msc.legacyurl: /web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
msc.type: authoredcontent
ms.openlocfilehash: fc03767c16f46c1e282d24ee3a7df2409a7c38bb
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78585760"
---
# <a name="aspnet-web-pages-razor-troubleshooting-guide"></a>Guia de solução de problemas de Páginas da Web do ASP.NET (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo descreve os problemas que você pode ter ao trabalhar com Páginas da Web do ASP.NET (Razor) e algumas soluções sugeridas.
> 
> ## <a name="software-versions"></a>Versões de software
> 
> 
> - Páginas da Web do ASP.NET (Razor) 3
>   
> 
> Este tutorial também funciona com Páginas da Web do ASP.NET 2 e Páginas da Web do ASP.NET 1,0.

Este tópico contém as seguintes seções:

- [Problemas com páginas em execução](#Issues_Running_.cshtml_Pages)
- [Problemas com o código do Razor](#IssuesWithRazorCode)
- [Problemas de segurança e Associação](#membership)
- [Problemas com o envio de email](#email)
- [Recursos adicionais](#AdditionalResources)

Para perguntas gerais, consulte [FAQ do páginas da Web do ASP.net (Razor)](https://go.microsoft.com/fwlink/?LinkId=253000).

<a id="Issues_Running_.cshtml_Pages"></a>
## <a name="issues-with-running-pages"></a>Problemas com páginas em execução

Uma variedade de problemas pode impedir que as páginas *. cshtml* e *. vbhtml* sejam executadas corretamente. Esta seção lista mensagens de erro comuns e causas prováveis.

### <a name="http-error-403---forbidden-access-is-denied"></a>Erro HTTP 403-Proibido: acesso negado

*Você não tem permissão para exibir este diretório ou página usando as credenciais fornecidas.*

Esse erro pode ocorrer se o servidor não estiver executando a versão correta do .NET Framework. Certifique-se de que o computador que está executando o servidor (local ou remotamente) tenha pelo menos o .NET Framework 4 instalado. Verifique também se o próprio aplicativo está configurado para executar a versão correta.

Se você vir esse problema localmente enquanto trabalha no WebMatrix, clique no espaço de trabalho **site** e, em seguida, em TreeView, clique em **configurações**. Na lista **selecionar versão .NET Framework** , selecione **.NET 4 (integrado)** . Se essa versão já estiver definida, tente executar o WebMatrix como administrador.

Certifique-se de que a raiz do seu site tenha pelo menos um arquivo *. cshtml* nele.

Se você vir esse erro quando o servidor Web estiver em um servidor remoto, contate o administrador do servidor. Verifique se o servidor tem o .NET Framework 4 ou posterior instalado. Verifique também se o aplicativo está em execução em um pool de aplicativos que está configurado para usar essa versão do the.NET Framework.

Se você tiver controle sobre o servidor, verifique se ele está executando a versão correta do .NET Framework. Você também pode tentar reparar a instalação executando o comando `aspnet_regiis -iru`. (Por exemplo, se você instalar o IIS depois de instalar o .NET Framework, o IIS não será configurado corretamente para executar páginas do ASP.NET.) Para obter mais informações, consulte [ferramenta de registro do IIS do ASP.net (Aspnet\_regiis. exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx).

### <a name="http-error-40314---forbidden"></a>Erro HTTP 403,14-proibido

*O servidor Web está configurado para não listar o conteúdo deste diretório.*

Esse erro pode ocorrer se você solicitar um recurso protegido (como o arquivo *Web. config* ) ou se ele estiver em uma pasta protegida (como *aplicativo\_dados* ou *código de\_de aplicativo*).

### <a name="http-error-40417---not-found"></a>Erro HTTP 404,17-não encontrado

*O conteúdo solicitado parece ser um script e não será servido pelo manipulador de arquivo estático.*

Esse erro pode ocorrer se o servidor não estiver configurado corretamente para usar o .NET Framework 4 ou posterior e, portanto, não reconhecer o código em blocos de `@{ }`. Consulte a descrição anterior para o *erro HTTP 403-Proibido: acesso negado*.

### <a name="http-error-4047---not-found"></a>Erro HTTP 404,7-não encontrado

*O módulo de filtragem de solicitação está configurado para negar a extensão de arquivo*

Esse erro pode ocorrer se as extensões *. cshtml* ou *. vbhtml* tiverem sido explicitamente bloqueadas no servidor. Um sintoma desse problema é que as URLs funcionam quando não incluem a extensão, mas as URLs que incluem *. cshtml* ou *. vbhtml* não funcionam. Uma solução possível é reabilitar as extensões no arquivo *Web. config* do site. O exemplo a seguir mostra como habilitar a extensão *. cshtml* .

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample1.xml?highlight=5-6)]

### <a name="http-error-4048---not-found"></a>Erro HTTP 404,8-não encontrado

*O módulo de filtragem de solicitação está configurado para negar um caminho na URL que contém uma seção hiddenSegment.*

Esse erro pode ocorrer se você solicitar um recurso protegido (como o arquivo *Web. config* ) ou se ele estiver em uma pasta protegida (como *aplicativo\_dados* ou *código de\_de aplicativo*).

### <a name="this-type-of-page-is-not-served-server-error-in--application"></a>Este tipo de página não é atendido (erro de servidor no aplicativo '/')

Consulte a descrição anterior para o erro HTTP 404,17.

<a id="IssuesWithRazorCode"></a>
## <a name="issues-with-razor-code"></a>Problemas com o código do Razor

### <a name="the-name-class-does-not-exist-in-the-current-context"></a>O nome '*Class*' não existe no contexto atual

Geralmente, um motivo para você ver esse erro é que `class` referencia um auxiliar, mas o auxiliar não está instalado. Por exemplo, se você tentar usar um auxiliar, mas se não tiver instalado o pacote do NuGet, você verá esse erro. Use a galeria do WebMatrix para localizar e instalar o auxiliar.

Se o auxiliar estiver instalado, mas a página ainda não o reconhecer, tente adicionar uma instrução de `using` ao código. Na instrução `using`, referencie o namespace que inclui o auxiliar. Por exemplo, os auxiliares básicos que estão no pacote auxiliares da Web do ASP.NET estão no namespace `System.Web.Helpers`. Na parte superior da página em que você deseja usar o auxiliar, adicione esta linha:

`@using Microsoft.Web.Helpers;`

<a id="membership"></a>
## <a name="issues-with-security-and-membership"></a>Problemas de segurança e Associação

Se você estiver usando o sistema interno de segurança (Associação) no Páginas da Web do ASP.NET (Razor), poderá encontrar os seguintes problemas.

### <a name="to-call-this-method-the-membershipprovider-property-must-be-an-instance-of-extendedmembershipprovider"></a>Para chamar esse método, a propriedade "Membership. Provider" deve ser uma instância de "ExtendedMembershipProvider"

Esse erro pode indicar que nenhuma classe de `AspNetSqlMembershipProvider` está configurada. (Um sintoma é que o site funciona bem localmente, mas gera esse erro quando você o publica no servidor de um provedor de hospedagem.) Uma correção para esse problema é habilitar explicitamente a associação simples adicionando o seguinte ao arquivo *Web. config* do site:

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample2.xml?highlight=6)]

<a id="email"></a>
## <a name="issues-with-sending-email"></a>Problemas com o envio de email

Problemas com o envio de emails podem ser desafiadores de depurar. Um problema inicial pode ser que você não pode se conectar ao servidor SMTP. Se a conexão for bem-sucedida, o ASP.NET entrega a mensagem para o servidor SMTP. No entanto, pode haver problemas com a própria mensagem que impede o envio do servidor SMTP.

Se seu aplicativo não enviar email com êxito, tente o seguinte:

- O nome do servidor SMTP geralmente é algo como `smtp.provider.com` ou `smtp.provider.net`. No entanto, se você publicar seu site em um provedor de hospedagem, o nome do servidor SMTP nesse ponto poderá ser `localhost`. Essa situação ocorre porque, depois de você ter publicado e seu site está em execução no servidor do provedor, o servidor SMTP pode ser local a partir da perspectiva do seu aplicativo. Essa alteração nos nomes de servidor pode significar que você precisa alterar o nome do servidor SMTP como parte do processo de publicação.
- O número da porta geralmente é 25. No entanto, alguns provedores exigem que você use a porta 587 ou alguma outra porta. Verifique com o proprietário do servidor SMTP qual número de porta eles esperam que você use.
- Certifique-se de usar as credenciais corretas. Se você publicou seu site em um provedor de hospedagem, use as credenciais que o provedor indicou especificamente para email. Essas credenciais podem ser diferentes das credenciais que você usa para publicar.
- Às vezes, você não precisa de credenciais. Se você estiver enviando emails usando seu provedor de Internet pessoal, seu provedor de email talvez já conheça suas credenciais. Depois de publicar, talvez seja necessário usar credenciais diferentes do que quando você testar em seu computador local.
- Se seu provedor de email usar criptografia, defina `WebMail.EnableSsl` como `true`.

Se houver um erro ao enviar email, você poderá ver uma mensagem de erro padrão do ASP.NET, que tem esta aparência:

![Mensagem de erro do ASP.NET quando há um problema com o email](aspnet-web-pages-razor-troubleshooting-guide/_static/image1.png)

Você também pode depurar problemas com o envio de email usando um bloco de `try-catch`, como no exemplo a seguir. Quando você usa um bloco de `try-catch`, o ASP.NET não exibe suas mensagens de erro padrão. Em vez disso, você pode capturar o erro na parte `catch` do bloco.

[!code-cshtml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample3.cshtml)]

Substitua os valores apropriados para `your-SMTP-server-name`e assim por diante. Algumas das mensagens de erro que você pode ver dessa maneira incluem o seguinte:

- *Falha ao enviar email.*

    -ou-

    *Uma tentativa de conexão falhou porque a parte conectada não respondeu corretamente após um período de tempo ou a conexão estabelecida falhou porque o host conectado falhou ao responder*

    Esse erro normalmente significa que o aplicativo não pôde se conectar ao servidor SMTP. Verifique o nome do servidor e o número da porta.
- *Caixa de correio não disponível. A resposta do servidor foi: 5.1.0 &lt;someuser@invaliddomain&gt; remetente rejeitado: domínio do remetente inválido*

    Esta mensagem pode indicar que o endereço de `From` não está correto ou ausente.
- *A cadeia de caracteres especificada não está no formato necessário para um endereço de email.*

    Esse erro pode indicar que o valor das propriedades `To` ou `From` não são reconhecidos como endereços de email. (ASP.NET não pode verificar se o endereço de email é válido, apenas se ele está no formato correto, como *name@domain.com* .)

> [!NOTE]
> Remova a marcação que exibe o erro (`@errorMessage`) antes de publicar a página em um site ativo. Não é uma boa ideia permitir que os usuários vejam mensagens de erro obtidas de um servidor.

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Recursos adicionais

[Perguntas frequentes sobre Páginas da Web do ASP.NET (Razor)](https://go.microsoft.com/fwlink/?LinkId=253000)

Fórum do [WebMatrix e do páginas da Web do ASP.net](https://forums.asp.net/1224.aspx/1?WebMatrix) no site do ASP.net
