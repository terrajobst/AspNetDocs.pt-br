---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: Habilitando solicitações entre origens no ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: Mostra como oferecer suporte ao compartilhamento de recursos entre origens (CORS) no ASP.NET Web API.
ms.author: riande
ms.date: 01/29/2019
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 9d3016d98fa6c3a55359c6dab0737407b29925f1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555702"
---
# <a name="enable-cross-origin-requests-in-aspnet-web-api-2"></a>Habilitar solicitações entre origens no ASP.NET Web API 2

por [Mike Wasson](https://github.com/MikeWasson)

> A segurança do navegador impede que uma página da Web envie solicitações do AJAX para outro domínio. Essa restrição é chamada de *política de mesma origem*e impede que um site mal-intencionado leia dados confidenciais de outro site. No entanto, às vezes, talvez você queira permitir que outros sites Chamem sua API da Web.
>
> O CORS ( [compartilhamento de recursos entre origens](http://www.w3.org/TR/cors/) ) é um padrão W3C que permite que um servidor Relaxe a política de mesma origem. Usando o CORS, um servidor pode explicitamente permitir algumas solicitações entre origens e rejeitar outras. O CORS é mais seguro e mais flexível do que as técnicas anteriores, como [JSONP](http://en.wikipedia.org/wiki/JSONP). Este tutorial mostra como habilitar o CORS em seu aplicativo de API Web.
>
> ## <a name="software-used-in-the-tutorial"></a>Software usado no tutorial
>
> - [Visual Studio](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - API Web 2,2

## <a name="introduction"></a>Introdução

Este tutorial demonstra o suporte a CORS no ASP.NET Web API. Vamos começar criando dois projetos ASP.NET – um chamado "WebService", que hospeda um controlador de API da Web e o outro chamado "WebClient", que chama WebService. Como os dois aplicativos são hospedados em domínios diferentes, uma solicitação AJAX de WebClient para WebService é uma solicitação entre origens.

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a>O que é a "mesma origem"?

Duas URLs têm a mesma origem se tiverem esquemas, hosts e portas idênticos. ([RFC 6454](http://tools.ietf.org/html/rfc6454))

Essas duas URLs têm a mesma origem:

- `http://example.com/foo.html`
- `http://example.com/bar.html`

Essas URLs têm origens diferentes das duas anteriores:

- `http://example.net`-domínio diferente
- `http://example.com:9000/foo.html`-porta diferente
- `https://example.com/foo.html`-esquema diferente
- `http://www.example.com/foo.html`-subdomínio diferente

> [!NOTE]
> O Internet Explorer não considera a porta ao comparar origens.

## <a name="create-the-webservice-project"></a>Criar o projeto WebService

> [!NOTE]
> Esta seção pressupõe que você já sabe como criar projetos de API Web. Caso contrário, consulte [introdução com ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).

1. Inicie o Visual Studio e crie um novo projeto de **aplicativo Web ASP.net (.NET Framework)** .
2. Na caixa de diálogo **novo aplicativo Web ASP.net** , selecione o modelo de projeto **vazio** . Em **Adicionar pastas e referências principais para**, marque a caixa de seleção **API da Web** .

   ![Caixa de diálogo novo projeto ASP.NET no Visual Studio](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog.png)

3. Adicione um controlador de API da Web chamado `TestController` com o seguinte código:

   [!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

4. Você pode executar o aplicativo localmente ou implantá-lo no Azure. (Para as capturas de tela neste tutorial, o aplicativo é implantado em Azure App aplicativos Web do serviço.) Para verificar se a API da Web está funcionando, navegue até `http://hostname/api/test/`, em que *hostname* é o domínio no qual você implantou o aplicativo. Você deve ver o texto da resposta &quot;GET: Test Message&quot;.

   ![Navegador da Web mostrando mensagem de teste](enabling-cross-origin-requests-in-web-api/_static/image4.png)

## <a name="create-the-webclient-project"></a>Criar o projeto WebClient

1. Crie outro projeto de **aplicativo Web do ASP.net (.NET Framework)** e selecione o modelo de projeto do **MVC** . Opcionalmente, selecione **alterar autenticação** > **sem autenticação**. Você não precisa de autenticação para este tutorial.

   ![Modelo MVC na caixa de diálogo novo projeto ASP.NET no Visual Studio](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog-mvc.png)

2. Em **Gerenciador de soluções**, abra o arquivo *views/home/index. cshtml*. Substitua o código deste arquivo pelo seguinte:

   [!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

   Para a variável *ServiceUrl* , use o URI do aplicativo WebService.

3. Execute o aplicativo WebClient localmente ou publique-o em outro site.

Quando você clica no botão "experimentar", uma solicitação AJAX é enviada para o aplicativo WebService usando o método HTTP listado na caixa suspensa (GET, POST ou PUT). Isso permite que você examine diferentes solicitações entre origens. Atualmente, o aplicativo WebService não oferece suporte a CORS, portanto, se você clicar no botão, receberá um erro.

![Erro ' Try ' no navegador](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> Se você observar o tráfego HTTP em uma ferramenta como o [Fiddler](https://www.telerik.com/fiddler), verá que o navegador envia a solicitação get e a solicitação é realizada com sucesso, mas a chamada AJAX retorna um erro. É importante entender que a política de mesma origem não impede que o navegador *envie* a solicitação. Em vez disso, ele impede que o aplicativo Veja a *resposta*.

![Depurador da Web do Fiddler mostrando solicitações da Web](enabling-cross-origin-requests-in-web-api/_static/image8.png)

## <a name="enable-cors"></a>Habilitar CORS

Agora, vamos habilitar o CORS no aplicativo WebService. Primeiro, adicione o pacote NuGet do CORS. No Visual Studio, no menu **ferramentas** , selecione **Gerenciador de pacotes NuGet**e, em seguida, selecione **console do Gerenciador de pacotes**. Na janela do console do Gerenciador de pacotes, digite o seguinte comando:

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

Esse comando instala o pacote mais recente e atualiza todas as dependências, incluindo as principais bibliotecas de API Web. Use o sinalizador `-Version` para direcionar uma versão específica. O pacote CORS requer a API Web 2,0 ou posterior.

Abra o aplicativo de arquivo *\_Start/WebApiConfig. cs*. Adicione o seguinte código ao método **WebApiConfig. Register** :

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

Em seguida, adicione o atributo **[EnableCors]** à classe `TestController`:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

Para o parâmetro *Origins* , use o URI no qual você implantou o aplicativo WebClient. Isso permite solicitações entre origens do WebClient, ao mesmo tempo que ainda não permite todas as outras solicitações entre domínios. Posteriormente, descreverei os parâmetros para **[EnableCors]** mais detalhadamente.

Não inclua uma barra invertida no final da URL de *origens* .

Reimplante o aplicativo WebService atualizado. Você não precisa atualizar o WebClient. Agora a solicitação AJAX do WebClient deve ser realizada com sucesso. Todos os métodos GET, PUT e POST são permitidos.

![Navegador da Web mostrando mensagem de teste bem-sucedida](enabling-cross-origin-requests-in-web-api/_static/image9.png)

## <a name="how-cors-works"></a>Como o CORS funciona

Esta seção descreve o que acontece em uma solicitação CORS, no nível das mensagens HTTP. É importante entender como o CORS funciona, para que você possa configurar o atributo **[EnableCors]** corretamente e solucionar problemas se as coisas não funcionarem conforme o esperado.

A especificação CORS apresenta vários novos cabeçalhos HTTP que habilitam solicitações entre origens. Se um navegador oferecer suporte a CORS, ele definirá esses cabeçalhos automaticamente para solicitações entre origens; Você não precisa fazer nada especial no seu código JavaScript.

Aqui está um exemplo de uma solicitação entre origens. O cabeçalho "origem" fornece o domínio do site que está fazendo a solicitação.

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

Se o servidor permitir a solicitação, ele definirá o cabeçalho Access-Control-Allow-Origin. O valor desse cabeçalho corresponde ao cabeçalho de origem ou é o valor curinga "\*", o que significa que qualquer origem é permitida.

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

Se a resposta não incluir o cabeçalho Access-Control-Allow-Origin, a solicitação AJAX falhará. Especificamente, o navegador não permite a solicitação. Mesmo que o servidor retorne uma resposta bem-sucedida, o navegador não disponibilizará a resposta para o aplicativo cliente.

**Solicitações de simulação**

Para algumas solicitações de CORS, o navegador envia uma solicitação adicional, chamada de "solicitação de simulação", antes de enviar a solicitação real para o recurso.

O navegador poderá ignorar a solicitação de simulação se as seguintes condições forem verdadeiras:

- O método de solicitação é GET, HEAD ou POST *e*
- O aplicativo não define nenhum cabeçalho de solicitação que não seja aceito, Accept-Language, Content-Language, Content-Type ou Last-Event-ID *e*
- O cabeçalho Content-Type (se definido) é um dos seguintes:

    - Application/x-www-form-urlencoded
    - multipart/form-data
    - texto/sem formatação

A regra sobre cabeçalhos de solicitação aplica-se aos cabeçalhos que o aplicativo define chamando **setRequestHeader** no objeto **XMLHttpRequest** . (A especificação CORS chama esses "cabeçalhos de solicitação de autor".) A regra não se aplica aos cabeçalhos que o *navegador* pode definir, como agente de usuário, host ou comprimento de conteúdo.

Aqui está um exemplo de uma solicitação de simulação:

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

A solicitação de simulação usa o método de opções HTTP. Ele inclui dois cabeçalhos especiais:

- Access-Control-Request-Method: o método HTTP que será usado para a solicitação real.
- Access-Control-request-headers: uma lista de cabeçalhos de solicitação que o *aplicativo* definiu na solicitação real. (Novamente, isso não inclui os cabeçalhos que o navegador define.)

Aqui está um exemplo de resposta, supondo que o servidor permita a solicitação:

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

A resposta inclui um cabeçalho Access-Control-Allow-Methods que lista os métodos permitidos e, opcionalmente, um cabeçalho Access-Control-Allow-headers, que lista os cabeçalhos permitidos. Se a solicitação de simulação for realizada com sucesso, o navegador enviará a solicitação real, conforme descrito anteriormente.

As ferramentas normalmente usadas para testar pontos de extremidade com as solicitações de simulação (por exemplo, [Fiddler](https://www.telerik.com/fiddler) e [postmaster](https://www.getpostman.com/)) não enviam os cabeçalhos de opções necessárias por padrão. Confirme se os cabeçalhos `Access-Control-Request-Method` e `Access-Control-Request-Headers` são enviados com a solicitação e se os cabeçalhos de opções chegam ao aplicativo por meio do IIS.

Para configurar o IIS para permitir que um aplicativo ASP.NET receba e manipule solicitações de opção, adicione a seguinte configuração ao arquivo *Web. config* do aplicativo na seção `<system.webServer><handlers>`:

```xml
<system.webServer>
  <handlers>
    <remove name="ExtensionlessUrlHandler-Integrated-4.0" />
    <remove name="OPTIONSVerbHandler" />
    <add name="ExtensionlessUrlHandler-Integrated-4.0" path="*." verb="*" type="System.Web.Handlers.TransferRequestHandler" preCondition="integratedMode,runtimeVersionv4.0" />
  </handlers>
</system.webServer>
```

A remoção de `OPTIONSVerbHandler` impede que o IIS trate as solicitações de opções. A substituição de `ExtensionlessUrlHandler-Integrated-4.0` permite que as solicitações de opções alcancem o aplicativo, pois o registro de módulo padrão só permite solicitações GET, HEAD, POST e DEBUG com URLs sem extensão.

## <a name="scope-rules-for-enablecors"></a>Regras de escopo para [EnableCors]

Você pode habilitar o CORS por ação, por controlador ou globalmente para todos os controladores de API da Web em seu aplicativo.

**Por ação**

Para habilitar o CORS para uma única ação, defina o atributo **[EnableCors]** no método de ação. O exemplo a seguir habilita o CORS somente para o método `GetItem`.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

**Por controlador**

Se você definir **[EnableCors]** na classe do controlador, ele se aplicará a todas as ações no controlador. Para desabilitar o CORS para uma ação, adicione o atributo **[DisableCors]** à ação. O exemplo a seguir habilita o CORS para cada método, exceto `PutItem`.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

**Globalmente**

Para habilitar o CORS para todos os controladores de API da Web em seu aplicativo, passe uma instância de **EnableCorsAttribute** para o método **EnableCors** :

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

Se você definir o atributo em mais de um escopo, a ordem de precedência será:

1. Ação
2. Controller
3. Global

## <a name="set-the-allowed-origins"></a>Definir as origens permitidas

O parâmetro *Origins* do atributo **[EnableCors]** especifica quais origens têm permissão para acessar o recurso. O valor é uma lista separada por vírgulas das origens permitidas.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

Você também pode usar o valor curinga "\*" para permitir solicitações de quaisquer origens.

Considere atentamente antes de permitir solicitações de qualquer origem. Isso significa que literalmente qualquer site pode fazer chamadas AJAX para sua API da Web.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

## <a name="set-the-allowed-http-methods"></a>Definir os métodos HTTP permitidos

O parâmetro *Methods* do atributo **[EnableCors]** especifica quais métodos http têm permissão para acessar o recurso. Para permitir todos os métodos, use o valor curinga "\*". O exemplo a seguir permite apenas solicitações GET e POST.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

## <a name="set-the-allowed-request-headers"></a>Definir os cabeçalhos de solicitação permitidos

Este artigo descreveu anteriormente como uma solicitação de simulação pode incluir um cabeçalho Access-Control-request-headers, listando os cabeçalhos HTTP definidos pelo aplicativo (o que chamamos de "cabeçalhos de solicitação de autor"). O parâmetro *Headers* do atributo **[EnableCors]** especifica quais cabeçalhos de solicitação de autor são permitidos. Para permitir todos os cabeçalhos, defina os *cabeçalhos* como "\*". Para cabeçalhos específicos da lista de permissões, defina *cabeçalhos* para uma lista separada por vírgulas dos cabeçalhos permitidos:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

No entanto, os navegadores não são totalmente consistentes em como eles definem o Access-Control-request-headers. Por exemplo, o Chrome atualmente inclui "Origin". O FireFox não inclui cabeçalhos padrão, como "Accept", mesmo quando o aplicativo os define em script.

Se você definir *cabeçalhos* para algo diferente de "\*", deverá incluir pelo menos "Accept", "Content-Type" e "Origin", além de todos os cabeçalhos personalizados aos quais você deseja dar suporte.

## <a name="set-the-allowed-response-headers"></a>Definir os cabeçalhos de resposta permitidos

Por padrão, o navegador não expõe todos os cabeçalhos de resposta ao aplicativo. Os cabeçalhos de resposta que estão disponíveis por padrão são:

- Cache-Control
- Content-Language
- Tipo de conteúdo
- Expires
- Última modificação
- Pragma

A especificação CORS chama esses [cabeçalhos de resposta simples](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header). Para disponibilizar outros cabeçalhos para o aplicativo, defina o parâmetro *exposedHeaders* de **[EnableCors]** .

No exemplo a seguir, o método `Get` do controlador define um cabeçalho personalizado chamado ' X-Custom-Header '. Por padrão, o navegador não vai expor esse cabeçalho em uma solicitação entre origens. Para disponibilizar o cabeçalho, inclua ' X-Custom-Header ' em *exposedHeaders*.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

## <a name="pass-credentials-in-cross-origin-requests"></a>Passar credenciais em solicitações entre origens

As credenciais exigem tratamento especial em uma solicitação CORS. Por padrão, o navegador não envia credenciais com uma solicitação entre origens. As credenciais incluem cookies, bem como esquemas de autenticação HTTP. Para enviar credenciais com uma solicitação entre origens, o cliente deve definir **XMLHttpRequest. withCredentials** como true.

Usando **XMLHttpRequest** diretamente:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

No jQuery:

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

Além disso, o servidor deve permitir as credenciais. Para permitir credenciais entre origens na API Web, defina a propriedade **SupportsCredentials** como true no atributo **[EnableCors]** :

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

Se essa propriedade for true, a resposta HTTP incluirá um cabeçalho Access-Control-Allow-Credentials. Esse cabeçalho informa ao navegador que o servidor permite credenciais para uma solicitação entre origens.

Se o navegador enviar credenciais, mas a resposta não incluir um cabeçalho Access-Control-Allow-Credentials válido, o navegador não vai expor a resposta ao aplicativo e a solicitação AJAX falhará.

Tenha cuidado ao definir **SupportsCredentials** como true, porque isso significa que um site em outro domínio pode enviar credenciais de um usuário conectado para sua API Web em nome do usuário, sem que o usuário esteja ciente. A especificação CORS também indica que a definição de *origens* para &quot;\*&quot; será inválida se **SupportsCredentials** for true.

## <a name="custom-cors-policy-providers"></a>Provedores de política CORS personalizados

O atributo **[EnableCors]** implementa a interface **ICorsPolicyProvider** . Você pode fornecer sua própria implementação criando uma classe que deriva de **atributo** e implementa **ICorsPolicyProvider**.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

Agora você pode aplicar o atributo em qualquer lugar que você colocaria **[EnableCors]** .

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

Por exemplo, um provedor de política CORS personalizado pode ler as configurações de um arquivo de configuração.

Como alternativa ao uso de atributos, você pode registrar um objeto **ICorsPolicyProviderFactory** que cria objetos **ICorsPolicyProvider** .

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

Para definir o **ICorsPolicyProviderFactory**, chame o método de extensão **SetCorsPolicyProviderFactory** na inicialização, da seguinte maneira:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

## <a name="browser-support"></a>Suporte ao navegador

O pacote CORS da API Web é uma tecnologia do lado do servidor. O navegador do usuário também precisa dar suporte a CORS. Felizmente, as versões atuais de todos os principais navegadores incluem [suporte para CORS](http://caniuse.com/cors).
