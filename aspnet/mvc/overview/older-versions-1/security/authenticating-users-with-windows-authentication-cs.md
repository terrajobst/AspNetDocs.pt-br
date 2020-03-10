---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-cs
title: Autenticando usuários com a autenticaçãoC#do Windows () | Microsoft Docs
author: microsoft
description: Saiba como usar a autenticação do Windows no contexto de um aplicativo MVC. Você aprende a habilitar a autenticação do Windows dentro da Web do seu aplicativo...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 418bb07e-f369-4119-b4b0-08f890f7abb2
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-cs
msc.type: authoredcontent
ms.openlocfilehash: bb3909bff2791c15a8737fc12cac69f79b55733f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78624120"
---
# <a name="authenticating-users-with-windows-authentication-c"></a>Autenticar usuários com a autenticação do Windows (C#)

pela [Microsoft](https://github.com/microsoft)

> Saiba como usar a autenticação do Windows no contexto de um aplicativo MVC. Você aprende a habilitar a autenticação do Windows no arquivo de configuração da Web do aplicativo e a configurar a autenticação com o IIS. Por fim, você aprende a usar o atributo [autorizar] para restringir o acesso a ações do controlador a usuários ou grupos específicos do Windows.

O objetivo deste tutorial é explicar como você pode aproveitar os recursos de segurança internos do Serviços de Informações da Internet para proteger com senha as exibições em seus aplicativos MVC. Você aprende como permitir que as ações do controlador sejam invocadas somente por usuários específicos do Windows ou por usuários que são membros de grupos específicos do Windows.

Usar a autenticação do Windows faz sentido quando você está criando um site interno da empresa (um site de intranet) e deseja que os usuários possam usar seus nomes de usuário e senhas padrão do Windows ao acessar o site. Se você estiver criando um site voltado para a frente (um site da Internet), considere usar a autenticação de formulários.

#### <a name="enabling-windows-authentication"></a>Habilitando a autenticação do Windows

Quando você cria um novo aplicativo MVC do ASP.NET, a autenticação do Windows não é habilitada por padrão. A autenticação de formulários é o tipo de autenticação padrão habilitado para aplicativos MVC. Você deve habilitar a autenticação do Windows modificando o arquivo de configuração da Web do aplicativo MVC (Web. config). Localize a seção &lt;&gt; de autenticação e modifique-a para usar o Windows em vez da autenticação de formulários como esta:

[!code-xml[Main](authenticating-users-with-windows-authentication-cs/samples/sample1.xml)]

Quando você habilita a autenticação do Windows, seu servidor Web torna-se responsável por autenticar usuários. Normalmente, há dois tipos diferentes de servidores Web que você usa ao criar e implantar um aplicativo MVC ASP.NET.

Primeiro, ao desenvolver um aplicativo MVC, você usa o servidor Web de desenvolvimento ASP.NET incluído com o Visual Studio. Por padrão, o servidor Web de desenvolvimento ASP.NET executa todas as páginas no contexto da conta atual do Windows (qualquer que seja a conta usada para fazer logon no Windows).

O servidor Web de desenvolvimento ASP.NET também dá suporte à autenticação NTLM. Você pode habilitar a autenticação NTLM clicando com o botão direito do mouse no nome do seu projeto na janela Gerenciador de Soluções e selecionando Propriedades. Em seguida, selecione a guia Web e marque a caixa de seleção NTLM (veja a Figura 1).

**Figura 1 – habilitando a autenticação NTLM para o servidor Web de desenvolvimento ASP.NET**

![clip_image002](authenticating-users-with-windows-authentication-cs/_static/image1.jpg)

Para um aplicativo Web de produção, ao lado, você usa o IIS como seu servidor Web. O IIS dá suporte a vários tipos de autenticação, incluindo:

- Autenticação básica – definida como parte do protocolo HTTP 1,0. Envia nomes de usuário e senhas em texto não criptografado (codificado em Base64) pela Internet. -Autenticação Digest – envia um hash de uma senha, em vez da própria senha, pela Internet. -Autenticação do Windows (NTLM) integrada – o melhor tipo de autenticação a ser usado em ambientes de intranet usando o Windows. -Autenticação de certificado – habilita a autenticação usando um certificado do lado do cliente. O certificado é mapeado para uma conta de usuário do Windows.

> [!NOTE] 
> 
> Para obter uma visão geral mais detalhada desses diferentes tipos de autenticação, consulte [https://msdn.microsoft.com/library/aa292114(VS.71).aspx](https://msdn.microsoft.com/library/aa292114(VS.71).aspx).

Você pode usar o Gerenciador de Serviços de Informações da Internet para habilitar um determinado tipo de autenticação. Lembre-se de que todos os tipos de autenticação não estão disponíveis no caso de todos os sistemas operacionais. Além disso, se você estiver usando o IIS 7,0 com o Windows Vista, será necessário habilitar os diferentes tipos de autenticação do Windows antes que eles apareçam no Gerenciador de Serviços de Informações da Internet. Abra o **painel de controle, programas, programas e recursos, ativar ou desativar recursos do Windows**e expanda o nó serviços de informações da Internet (consulte a Figura 2).

**Figura 2 – habilitando os recursos do Windows IIS**

![clip_image004](authenticating-users-with-windows-authentication-cs/_static/image2.jpg)

Usando Serviços de Informações da Internet, você pode habilitar ou desabilitar diferentes tipos de autenticação. Por exemplo, a Figura 3 ilustra a desabilitação da autenticação anônima e a habilitação da autenticação integrada do Windows (NTLM) ao usar o IIS 7,0.

**Figura 3 – habilitando a autenticação integrada do Windows**

![clip_image006](authenticating-users-with-windows-authentication-cs/_static/image3.jpg)

#### <a name="authorizing-windows-users-and-groups"></a>Autorizando usuários e grupos do Windows

Depois de habilitar a autenticação do Windows, você pode usar o atributo [autorizar] para controlar o acesso a controladores ou ações do controlador. Esse atributo pode ser aplicado a um controlador MVC inteiro ou a uma ação de controlador específica.

Por exemplo, o controlador Home na Listagem 1 expõe três ações nomeadas index (), CompanySecrets () e StephenSecrets (). Qualquer pessoa pode invocar a ação de índice (). No entanto, somente os membros do grupo gerenciadores locais do Windows podem invocar a ação CompanySecrets (). Por fim, somente o usuário de domínio do Windows chamado Stephen (no domínio Redmond) pode invocar a ação StephenSecrets ().

**Listagem 1 – Controllers\HomeController.cs**

[!code-csharp[Main](authenticating-users-with-windows-authentication-cs/samples/sample2.cs)]

> [!NOTE] 
> 
> Devido ao UAC (controle de conta de usuário) do Windows, ao trabalhar com o Windows Vista ou o Windows Server 2008, o grupo local de administradores se comportará de forma diferente de outros grupos. O atributo [autorizar] não reconhecerá corretamente um membro do grupo local de administradores, a menos que você modifique as configurações de UAC do computador.

Exatamente o que acontece quando você tenta invocar uma ação do controlador sem ser as permissões corretas depende do tipo de autenticação habilitado. Por padrão, ao usar o ASP.NET Development Server, basta obter uma página em branco. A página é servida com um status de resposta http **não autorizado de 401** .

Se, por outro lado, você estiver usando o IIS com a autenticação anônima desabilitada e a autenticação básica habilitada, você continuará recebendo uma solicitação de diálogo de logon sempre que solicitar a página protegida (consulte a Figura 4).

**Figura 4 – caixa de diálogo logon de autenticação básica**

![clip_image008](authenticating-users-with-windows-authentication-cs/_static/image4.jpg)

#### <a name="summary"></a>Resumo

Este tutorial explicou como você pode usar a autenticação do Windows no contexto de um aplicativo MVC ASP.NET. Você aprendeu como habilitar a autenticação do Windows no arquivo de configuração da Web do aplicativo e como configurar a autenticação com o IIS. Por fim, você aprendeu a usar o atributo [autorizar] para restringir o acesso a ações do controlador a usuários ou grupos específicos do Windows.

> [!div class="step-by-step"]
> [Anterior](authenticating-users-with-forms-authentication-cs.md)
> [Próximo](preventing-javascript-injection-attacks-cs.md)
