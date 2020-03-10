---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
title: Noções básicas sobre Serviços de Aplicativos de autenticação e perfil do ASP.NET AJAX | Microsoft Docs
author: scottcate
description: O serviço de autenticação permite que os usuários forneçam credenciais para receber um cookie de autenticação e é o serviço de gateway para permitir usuário personalizado...
ms.author: riande
ms.date: 03/14/2008
ms.assetid: 6ab4efb6-aab6-45ac-ad2c-bdec5848ef9e
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
msc.type: authoredcontent
ms.openlocfilehash: cab9acb1ffd75cca87f6c575a6abdd000235828e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78640528"
---
# <a name="understanding-aspnet-ajax-authentication-and-profile-application-services"></a>Noções básicas sobre Serviços de Aplicativos de perfil e autenticação do AJAX ASP.NET

por [Scott Cate](https://github.com/scottcate)

[Baixar PDF](https://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial03_MSAjax_ASP.NET_Services_cs.pdf)

> O serviço de autenticação permite que os usuários forneçam credenciais para receber um cookie de autenticação e é o serviço de gateway para permitir perfis de usuário personalizados fornecidos pelo ASP.NET. O uso do serviço de autenticação AJAX ASP.NET é compatível com a autenticação de formulários padrão do ASP.NET, para que os aplicativos que atualmente usam a autenticação de formulários (como com o controle de logon) não sejam desfeitos pela atualização para o serviço de autenticação AJAX.

## <a name="introduction"></a>Introdução

Como parte do .NET Framework 3,5, a Microsoft está oferecendo uma atualização de ambiente dimensionável; o não é apenas um novo ambiente de desenvolvimento disponível, mas os novos recursos de LINQ (consulta integrada à linguagem) e outros aprimoramentos de linguagem estão em breve. Além disso, alguns recursos familiares de outros conjuntos de ferramentas, especialmente as extensões AJAX do ASP.NET, estão sendo incluídos como membros de primeira classe da biblioteca de classes base .NET Framework. Essas extensões habilitam muitos novos recursos avançados de cliente, incluindo o processamento parcial de páginas sem a necessidade de uma atualização de página completa, a capacidade de acessar serviços Web por meio de script de cliente (incluindo a API de criação de perfil do ASP.NET) e uma API extensiva do lado do cliente projetado para espelhar muitos dos esquemas de controle vistos no conjunto de controle do lado do servidor ASP.NET.

Este White Paper analisa a implementação e o uso dos serviços de criação de perfil e autenticação de formulários do ASP.NET, pois eles são expostos pelo Microsoft ASP.NET AJAX ExtensionsThe Ajax Extensions tornam a autenticação de formulários incrivelmente fácil de dar suporte, como ele (bem como o serviço de criação de perfil) é exposto por meio de um script de proxy de serviço da Web. As extensões AJAX também oferecem suporte à autenticação personalizada por meio da classe AuthenticationServicemanager.

Este White Paper se baseia na versão beta 2 do Visual Studio 2008 e do .NET Framework 3,5. Este White Paper também pressupõe que você trabalhará com o Visual Studio 2008 Beta 2, não com o Visual Web Developer Express, e fornecerá orientações de orientação de acordo com a interface do usuário do Visual Studio. Alguns exemplos de código podem utilizar modelos de projeto indisponíveis no Visual Web Developer Express.

## <a name="profiles-and-authentication"></a>*Perfis e autenticação*

Os perfis de Microsoft ASP.NET e os serviços de autenticação são fornecidos pelo sistema de autenticação de formulários ASP.NET e são componentes padrão do ASP.NET. As extensões do AJAX ASP.NET fornecem acesso de script a esses serviços por meio de proxies de script, por meio de um modelo bastante direto no namespace sys. Services da biblioteca AJAX do cliente.

O serviço de autenticação permite que os usuários forneçam credenciais para receber um cookie de autenticação e é o serviço de gateway para permitir perfis de usuário personalizados fornecidos pelo ASP.NET. O uso do serviço de autenticação AJAX ASP.NET é compatível com a autenticação de formulários padrão do ASP.NET, para que os aplicativos que atualmente usam a autenticação de formulários (como com o controle de logon) não sejam desfeitos pela atualização para o serviço de autenticação AJAX.

O serviço de perfil permite a integração automática e o armazenamento de dados do usuário com base na associação, conforme fornecido pelo serviço de autenticação. Os dados armazenados são especificados pelo arquivo Web. config, e os vários provedores de serviço de criação de perfil manipulam o gerenciamento de dados. Assim como no serviço de autenticação, o serviço de perfil AJAX é compatível com o serviço de perfil ASP.NET padrão, de modo que as páginas que atualmente incorporam recursos do serviço de perfil ASP.NET não devem ser desfeitas, incluindo o suporte a AJAX.

A incorporação da autenticação ASP.NET e dos próprios serviços de criação de perfil em um aplicativo está fora do escopo deste White Paper. Para obter mais informações sobre o tópico, consulte o artigo de referência da biblioteca do MSDN Gerenciando usuários usando a associação em [https://msdn.microsoft.com/library/tw292whz.aspx](https://msdn.microsoft.com/library/tw292whz.aspx). O ASP.NET também inclui um utilitário para configurar automaticamente a associação com um SQL Server, que é o provedor de serviço de autenticação padrão para associação ASP.NET. Para obter mais informações, consulte o artigo ASP.NET SQL Server a ferramenta de registro (ASPNET\_RegSql. exe) em [https://msdn.microsoft.com/library/ms229862(vs.80).aspx](https://msdn.microsoft.com/library/ms229862(vs.80).aspx).

## <a name="using-the-aspnet-ajax-authentication-service"></a>*Usando o serviço de autenticação do ASP.NET AJAX*

O serviço de autenticação do ASP.NET AJAX deve ser habilitado no arquivo Web. config:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample1.xml)]

O serviço de autenticação requer que a autenticação de formulários do ASP.NET seja habilitada e requer que os cookies sejam habilitados no navegador do cliente (um script não pode habilitar uma sessão sem cookie, pois as sessões sem cookie exigem parâmetros de URL).

Depois que o serviço de autenticação AJAX estiver habilitado e configurado, o script de cliente poderá aproveitar imediatamente o objeto Sys. Services. AuthenticationService. Principalmente, o script de cliente desejará aproveitar o método de `login` e a propriedade `isLoggedIn`. Existem várias propriedades para fornecer padrões para o método login, que pode aceitar um grande número de parâmetros.

*Membros sys. Services. AuthenticationService*

*método de logon:*

O método login () inicia uma solicitação para autenticar as credenciais do usuário. Esse método é assíncrono e não bloqueia a execução.

*Parâmetros:*

| **Nome do parâmetro** | **Significado** |
| --- | --- |
| userName | Obrigatórios. O nome de usuário a ser autenticado. |
| password | Opcional (o padrão é NULL). A senha do usuário. |
| isPersistent | Opcional (o padrão é false). Se o cookie de autenticação do usuário deve persistir entre as sessões. Se for false, o usuário fará logoff quando o navegador for fechado ou a sessão expirar. |
| redirectUrl | Opcional (o padrão é NULL). A URL para redirecionar o navegador para após a autenticação bem-sucedida. Se esse parâmetro for nulo ou uma cadeia de caracteres vazia, nenhum redirecionamento ocorrerá. |
| customInfo | Opcional (o padrão é NULL). Este parâmetro não é usado no momento e está reservado para uso futuro. |
| loginCompletedCallback | Opcional (o padrão é NULL). A função a ser chamada quando o logon for concluído com êxito. Se especificado, esse parâmetro substitui a propriedade defaultLoginCompleted. |
| failedCallback | Opcional (o padrão é NULL). A função a ser chamada quando o logon tiver falhado. Se especificado, esse parâmetro substitui a propriedade defaultFailedCallback. |
| userContext | Opcional (o padrão é NULL). Dados de contexto de usuário personalizados que devem ser passados para as funções de retorno de chamada. |

*Valor de retorno:*

Essa função não inclui um valor de retorno. No entanto, vários comportamentos são incluídos na conclusão de uma chamada para esta função:

- A página atual será atualizada ou será alterada se o parâmetro `redirectUrl` não for nulo nem uma cadeia de caracteres vazia.
- No entanto, se o parâmetro era nulo ou uma cadeia de caracteres vazia, o parâmetro `loginCompletedCallback` ou a propriedade `defaultLoginCompletedCallback` é chamada.
- Se a chamada para o serviço Web falhar, o parâmetro `failedCallback` da propriedade `defaultFailedCallback` será chamado.

*método de logout:*

O método logout () remove o cookie de credenciais e faz logoff do usuário atual do aplicativo Web.

*Parâmetros:*

| **Nome do parâmetro** | **Significado** |
| --- | --- |
| redirectUrl | Opcional (o padrão é NULL). A URL para redirecionar o navegador para após a autenticação bem-sucedida. Se esse parâmetro for nulo ou uma cadeia de caracteres vazia, nenhum redirecionamento ocorrerá. |
| logoutCompletedCallback | Opcional (o padrão é NULL). A função a ser chamada quando o logout for concluído com êxito. Se especificado, esse parâmetro substitui a propriedade defaultLogoutCompleted. |
| failedCallback | Opcional (o padrão é NULL). A função a ser chamada quando o logon tiver falhado. Se especificado, esse parâmetro substitui a propriedade defaultFailedCallback. |
| userContext | Opcional (o padrão é NULL). Dados de contexto de usuário personalizados que devem ser passados para as funções de retorno de chamada. |

*Valor de retorno:*

Essa função não inclui um valor de retorno. No entanto, vários comportamentos são incluídos na conclusão de uma chamada para esta função:

- A página atual será atualizada ou será alterada se o parâmetro `redirectUrl` não for nulo nem uma cadeia de caracteres vazia.
- No entanto, se o parâmetro era nulo ou uma cadeia de caracteres vazia, o parâmetro `logoutCompletedCallback` ou a propriedade `defaultLogoutCompletedCallback` é chamada.
- Se a chamada para o serviço Web falhar, o parâmetro `failedCallback` da propriedade `defaultFailedCallback` será chamado.

*Propriedade defaultFailedCallback (Get, Set):*

Esta propriedade especifica uma função que deve ser chamada se ocorrer uma falha na comunicação com o serviço Web. Ele deve receber um delegado (ou referência de função).

A referência de função especificada por essa propriedade deve ter a seguinte assinatura:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample2.js)]

*Parâmetros:*

| **Nome do parâmetro** | **Significado** |
| --- | --- |
| erro | Especifica as informações de erro. |
| userContext | Especifica as informações de contexto de usuário fornecidas quando a função de logon ou logout foi chamada. |
| methodName | O nome do método de chamada. |

*Propriedade defaultLoginCompletedCallback (Get, Set):*

Esta propriedade especifica uma função que deve ser chamada quando a chamada de serviço Web de logon for concluída. Ele deve receber um delegado (ou referência de função).

A referência de função especificada por essa propriedade deve ter a seguinte assinatura:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample3.js)]

*Parâmetros:*

| **Nome do parâmetro** | **Significado** |
| --- | --- |
| validCredentials | Especifica se o usuário forneceu credenciais válidas. `true` se o usuário fez logon com êxito; caso contrário `false`. |
| userContext | Especifica as informações de contexto de usuário fornecidas quando a função de logon foi chamada. |
| methodName | O nome do método de chamada. |

*Propriedade defaultLogoutCompletedCallback (Get, Set):*

Esta propriedade especifica uma função que deve ser chamada quando a chamada de serviço Web de logout for concluída. Ele deve receber um delegado (ou referência de função).

A referência de função especificada por essa propriedade deve ter a seguinte assinatura:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample4.js)]

*Parâmetros:*

| **Nome do parâmetro** | **Significado** |
| --- | --- |
| result | Esse parâmetro sempre será `null`; Ele é reservado para uso futuro. |
| userContext | Especifica as informações de contexto de usuário fornecidas quando a função de logon foi chamada. |
| methodName | O nome do método de chamada. |

*Propriedade islogind (Get):*

Esta propriedade Obtém o estado de autenticação atual do usuário; Ele é definido pelo objeto ScriptManager durante uma solicitação de página.

Essa propriedade retornará `true` se o usuário estiver conectado no momento; caso contrário, ele retornará `false`.

*Propriedade Path (Get, Set):*

Essa propriedade determina programaticamente o local do serviço Web de autenticação. Ele pode ser usado para substituir o provedor de autenticação padrão, bem como um conjunto declarativamente na propriedade Path do nó filho de AuthenticationService do controle do ScriptManager (para obter mais informações, consulte usando um provedor de serviço de autenticação personalizado tópico abaixo).

Observe que o local do serviço de autenticação padrão não é alterado. No entanto, o ASP.NET AJAX permite que você especifique o local de um serviço Web que fornece a mesma interface de classe do proxy do serviço de autenticação do ASP.NET AJAX.

Observe também que essa propriedade não deve ser definida como um valor que direcione a solicitação de script para fora do site atual. Como o aplicativo atual não receberá as credenciais de autenticação, ele seria inútil; Além disso, o AJAX subjacente à tecnologia não deve lançar solicitações entre sites e pode gerar uma exceção de segurança no navegador do cliente.

Essa propriedade é um objeto `String` que representa o caminho para o serviço Web de autenticação.

*Propriedade Timeout (Get, Set):*

Esta propriedade determina o período de tempo de espera para o serviço de autenticação antes de assumir que a solicitação de logon falhou. Se o tempo limite expirar enquanto aguarda a conclusão de uma chamada, o retorno de chamada da solicitação-falha será chamado e a chamada não será concluída.

Essa propriedade é um objeto `Number` que representa o número de milissegundos para aguardar os resultados do serviço de autenticação.

*Exemplo de código: fazendo logon no serviço de autenticação*

A marcação a seguir é uma página ASP.NET de exemplo com uma chamada de script simples para os métodos login e logout da classe AuthenticationService.

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample5.aspx)]

## <a name="accessing-aspnet-profiling-data-via-ajax"></a>Acessando dados de criação de perfil do ASP.NET via AJAX

O serviço de criação de perfil do ASP.NET também é exposto por meio das extensões AJAX do ASP.NET. Como o serviço de criação de perfil do ASP.NET fornece uma API sofisticada e granular pela qual armazenar e recuperar dados do usuário, essa pode ser uma excelente ferramenta de produtividade.

O serviço de perfil deve ser habilitado em Web. config; Ele não é por padrão. Para fazer isso, verifique se o elemento filho `profileService` foi habilitado = true especificado em Web. config e se você especificou quais propriedades podem ser lidas ou gravadas da seguinte maneira:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample6.xml)]

O serviço de perfil também deve ser configurado. Embora a configuração do serviço de criação de perfil esteja fora do escopo deste White Paper, vale a pena observar que os grupos conforme definidos nas definições de configuração do perfil estarão acessíveis como subpropriedades do nome do grupo. Por exemplo, com a seguinte seção de perfil especificada:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample7.xml)]

O script de cliente seria capaz de acessar Name, address. linha1, address. linhafinal, address. City, address. State, address. zip e BackgroundColor como propriedades do campo Properties da classe ProfileService.

Depois que o serviço de criação de perfil do AJAX estiver configurado, ele estará imediatamente disponível em páginas; no entanto, ele precisará ser carregado uma vez antes do uso.

*Membros sys. Services. ProfileService*

*campo de propriedades:*

O campo Propriedades expõe todos os dados de perfil configurados como propriedades filho que podem ser referenciadas pela Convenção ponto-operador-nome. Propriedades que são filhos de grupos de propriedades são referenciadas como GroupName. PropertyName. Na configuração de perfil de exemplo apresentada acima, para obter o estado do usuário, você pode usar o seguinte identificador:

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample8.cs)]

*método de carregamento:*

Carrega uma lista selecionada ou todas as propriedades do servidor.

*Parâmetros:*

| **Nome do parâmetro** | **Significado** |
| --- | --- |
| propertyNames | Opcional (o padrão é NULL). As propriedades a serem carregadas do servidor. |
| loadCompletedCallback | Opcional (o padrão é NULL). A função a ser chamada quando o carregamento for concluído. |
| failedCallback | Opcional (o padrão é NULL). A função a ser chamada se ocorrer um erro. |
| userContext | Opcional (o padrão é NULL). Informações de contexto a serem passadas para a função de retorno de chamada. |

A função load não tem um valor de retorno. Se a chamada for concluída com êxito, ela chamará o parâmetro `loadCompletedCallback` ou a propriedade `defaultLoadCompletedCallback`. Se a chamada tiver falhado ou o tempo limite expirar, o parâmetro `failedCallback` ou a propriedade `defaultFailedCallback` será chamada.

Se o parâmetro `propertyNames` não for fornecido, todas as propriedades de leitura configuradas serão recuperadas do servidor.

*salvar método:*

O método Save () salva a lista de propriedades especificada (ou todas as propriedades) no perfil ASP.NET do usuário.

*Parâmetros:*

| **Nome do parâmetro** | **Significado** |
| --- | --- |
| propertyNames | Opcional (o padrão é NULL). As propriedades a serem salvas no servidor. |
| saveCompletedCallback | Opcional (o padrão é NULL). A função a ser chamada quando o salvamento for concluído. |
| failedCallback | Opcional (o padrão é NULL). A função a ser chamada se ocorrer um erro. |
| userContext | Opcional (o padrão é NULL). Informações de contexto a serem passadas para a função de retorno de chamada. |

A função Save não tem um valor de retorno. Se a chamada for concluída com êxito, ela chamará o parâmetro `saveCompletedCallback` ou a propriedade `defaultSaveCompletedCallback`. Se a chamada tiver falhado ou o tempo limite expirar, a propriedade `failedCallback` ou `defaultFailedCallback` será chamada.

Se o parâmetro `propertyNames` for nulo, todas as propriedades de perfil serão enviadas ao servidor e o servidor decidirá quais propriedades podem ser salvas e quais não podem.

*Propriedade defaultFailedCallback (Get, Set):*

Esta propriedade especifica uma função que deve ser chamada se ocorrer uma falha na comunicação com o serviço Web. Ele deve receber um delegado (ou referência de função).

A referência de função especificada por essa propriedade deve ter a seguinte assinatura:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample9.js)]

*Parâmetros:*

| **Nome do parâmetro** | **Significado** |
| --- | --- |
| Erro | Especifica as informações de erro. |
| userContext | Especifica as informações de contexto de usuário fornecidas quando a função carregar ou salvar foi chamada. |
| methodName | O nome do método de chamada. |

*Propriedade defaultSaveCompleted (Get, Set):*

Esta propriedade especifica uma função que deve ser chamada após a conclusão do salvamento dos dados de perfil do usuário. Ele deve receber um delegado (ou referência de função).

A referência de função especificada por essa propriedade deve ter a seguinte assinatura:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample10.js)]

*Parâmetros:*

| **Nome do parâmetro** | **Significado** |
| --- | --- |
| numPropsSaved | Especifica o número de propriedades que foram salvas. |
| userContext | Especifica as informações de contexto de usuário fornecidas quando a função carregar ou salvar foi chamada. |
| methodName | O nome do método de chamada. |

*Propriedade defaultLoadCompleted (Get, Set):*

Esta propriedade especifica uma função que deve ser chamada após a conclusão do carregamento dos dados de perfil do usuário. Ele deve receber um delegado (ou referência de função).

A referência de função especificada por essa propriedade deve ter a seguinte assinatura:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample11.js)]

*Parâmetros:*

| **Nome do parâmetro** | **Significado** |
| --- | --- |
| numPropsLoaded | Especifica o número de propriedades carregadas. |
| userContext | Especifica as informações de contexto de usuário fornecidas quando a função carregar ou salvar foi chamada. |
| methodName | O nome do método de chamada. |

*Propriedade Path (Get, Set):*

Essa propriedade determina programaticamente o local do serviço Web de perfil. Ele pode ser usado para substituir o provedor de serviços de perfil padrão, bem como um conjunto declarativamente na propriedade Path do nó filho ProfileService do controle ScriptManager.

Observe que o local do serviço de perfil padrão não é alterado. No entanto, o ASP.NET AJAX permite que você especifique o local de um serviço Web que fornece a mesma interface de classe do proxy do serviço de autenticação do ASP.NET AJAX.

Observe também que essa propriedade não deve ser definida como um valor que direcione a solicitação de script para fora do site atual. O AJAX subjacente à tecnologia não deve lançar solicitações entre sites e pode gerar uma exceção de segurança no navegador do cliente.

Essa propriedade é um objeto `String` que representa o caminho para o serviço Web de perfil.

*Propriedade Timeout (Get, Set):*

Esta propriedade determina o período de tempo de espera para o serviço de perfil antes de assumir que a solicitação de carregamento ou gravação falhou. Se o tempo limite expirar enquanto aguarda a conclusão de uma chamada, o retorno de chamada da solicitação-falha será chamado e a chamada não será concluída.

Essa propriedade é um objeto `Number` que representa o número de milissegundos para aguardar os resultados do serviço de perfil.

*Exemplo de código: carregando dados de perfil no carregamento de página*

O código a seguir verificará se um usuário está autenticado e, nesse caso, carregará a cor de plano de fundo preferencial do usuário como a página.

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample12.js)]

## <a name="using-a-custom-authentication-service-provider"></a>*Usando um provedor de serviços de autenticação personalizado*

As extensões AJAX do ASP.NET permitem que você crie um provedor de serviço de autenticação de script personalizado expondo sua funcionalidade por meio de um serviço Web personalizado. Para ser usado, seu serviço Web deve expor dois métodos, `Login` e `Logout`; e esses métodos devem ser especificados com as mesmas assinaturas de método que o serviço Web de autenticação do ASP.NET AJAX padrão.

Depois de criar o serviço Web personalizado, você precisará especificar o caminho para ele, seja declarativamente em sua página, programaticamente no código ou por meio do script do cliente.

*Para definir o caminho declarativamente:*

Para definir o caminho de forma declarativa, inclua o filho de AuthenticationService do objeto ScriptManager em sua página do ASP.NET:

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample13.aspx)]

*Para definir o caminho no código:*

Para definir o caminho programaticamente, especifique o caminho por meio da instância do seu Gerenciador de scripts:

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample14.cs)]

*Para definir o caminho no script:*

Para definir o caminho programaticamente no script, utilize a propriedade `path` da classe AuthenticationService:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample15.js)]

*Serviço Web de exemplo para autenticação personalizada*

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample16.aspx)]

## <a name="summary"></a>Resumo

Serviços ASP.NETs – especificamente os serviços de criação de perfil, associação e autenticação – são facilmente expostos ao JavaScript no navegador do cliente. Isso permite que os desenvolvedores integrem o código do lado do cliente com o mecanismo de autenticação de forma direta, sem depender de controles como UpdatePanels para fazer o trabalho pesado. Os dados de perfil também podem ser protegidos do cliente, utilizando as definições de configuração da Web; nenhum dado está disponível por padrão e os desenvolvedores devem optar por propriedades de perfil.

Além disso, ao criar implementações simplificadas do serviço Web com assinaturas de método equivalentes, os desenvolvedores podem criar provedores de script personalizados para esses serviços ASP.NET intrínsecos. O suporte para essas técnicas simplifica o desenvolvimento de aplicativos cliente avançados, fornecendo aos desenvolvedores uma ampla variedade de flexibilidade para atender a necessidades específicas.

## <a name="bio"></a>*Biografia*

Scott Cate tem trabalhado com tecnologias Web da Microsoft desde 1997 e é presidente da myKB.com ([www.myKB.com](http://www.myKB.com)), onde é especialista em escrever aplicativos baseados em ASP.net voltados para as soluções de software da base de dados de conhecimento. Scott pode ser contatado por email em [scott.cate@myKB.com](mailto:scott.cate@myKB.com) ou em seu blog em [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Anterior](understanding-asp-net-ajax-updatepanel-triggers.md)
> [Próximo](understanding-asp-net-ajax-localization.md)
