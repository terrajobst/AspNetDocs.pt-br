---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: Habilitando a autenticação do Windows no Katana | Microsoft Docs
author: MikeWasson
description: 'Este artigo mostra como habilitar a autenticação do Windows no katana. Ele abrange dois cenários: usando o IIS para hospedar Katana e usar HttpListener para Kat de hospedagem interna...'
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: 3d81e7e1bf13ab63417378fba0c5ab80213f404b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78617176"
---
# <a name="enabling-windows-authentication-in-katana"></a>Habilitar a autenticação do Windows no Katana

por [Mike Wasson](https://github.com/MikeWasson)

> Este artigo mostra como habilitar a autenticação do Windows no katana. Ele abrange dois cenários: usando o IIS para hospedar Katana e usar HttpListener para Katana de hospedagem interna em um processo personalizado. Obrigado a Barry Dorrans, David Matson e Chris Ross por revisar este artigo.

Katana é a implementação de [OWIN](http://owin.org/)da Microsoft, a interface da Web aberta para .net. Você pode ler uma introdução a OWIN e Katana [aqui](an-overview-of-project-katana.md). A arquitetura OWIN tem várias camadas:

- Host: gerencia o processo no qual o pipeline OWIN é executado.
- Servidor: abre um soquete de rede e escuta as solicitações.
- Middleware: processa a solicitação e resposta HTTP.

Atualmente, o Katana fornece dois servidores, os quais oferecem suporte à autenticação integrada do Windows:

- **Microsoft. Owin. host. SystemWeb**. Usa o IIS com o pipeline ASP.NET.
- **Microsoft. Owin. host. HttpListener**. Usa [System .net. HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx). Atualmente, esse servidor é a opção padrão ao hospedar internamente katana.

> [!NOTE]
> No momento, o Katana não fornece o middleware OWIN para autenticação do Windows, pois essa funcionalidade já está disponível nos servidores.

## <a name="windows-authentication-in-iis"></a>Autenticação do Windows no IIS

Usando Microsoft. Owin. host. SystemWeb, você pode simplesmente habilitar a autenticação do Windows no IIS.

Vamos começar criando um novo aplicativo ASP.NET, usando o modelo de projeto "ASP.NET vazio aplicativo Web".

![](enabling-windows-authentication-in-katana/_static/image1.png)

Em seguida, adicione pacotes NuGet. No menu **ferramentas** , selecione **Gerenciador de pacotes NuGet**e, em seguida, selecione **console do Gerenciador de pacotes**. Na janela Console do Gerenciador de Pacotes, digite o seguinte comando:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

Agora, adicione uma classe chamada `Startup` com o seguinte código:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

Isso é tudo o que você precisa para criar um aplicativo "Olá, mundo" para OWIN, em execução no IIS. Pressione F5 para depurar o aplicativo. Você deve ver o "Hello World!" na janela do navegador.

![](enabling-windows-authentication-in-katana/_static/image2.png)

Em seguida, Habilitaremos a autenticação do Windows no IIS Express. No menu **Exibir** , selecione **Propriedades**. Clique no nome do projeto em Gerenciador de Soluções para exibir as propriedades do projeto.

Na janela **Propriedades** , defina **autenticação anônima** como **desabilitada** e defina **autenticação do Windows** como **habilitada**.

![](enabling-windows-authentication-in-katana/_static/image3.png)

Quando você executa o aplicativo do Visual Studio, IIS Express exigirá as credenciais do Windows do usuário. Você pode ver isso usando o [Fiddler](http://fiddler2.com/home) ou outra ferramenta de depuração de http. Aqui está um exemplo de resposta HTTP:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

Os cabeçalhos WWW-Authenticate nessa resposta indicam que o servidor dá suporte ao protocolo [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) , que usa o Kerberos ou NTLM.

Posteriormente, quando você implantar o aplicativo em um servidor, siga [estas etapas](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) para habilitar a autenticação do Windows no IIS nesse servidor.

## <a name="windows-authentication-in-httplistener"></a>Autenticação do Windows em HttpListener

Se estiver usando Microsoft. Owin. host. HttpListener para Katana de hospedagem interna, você poderá habilitar a autenticação do Windows diretamente na instância do **HttpListener** .

Primeiro, crie um novo aplicativo de console. Em seguida, adicione pacotes NuGet. No menu **ferramentas** , selecione **Gerenciador de pacotes NuGet**e, em seguida, selecione **console do Gerenciador de pacotes**. Na janela Console do Gerenciador de Pacotes, digite o seguinte comando:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

Agora, adicione uma classe chamada `Startup` com o seguinte código:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

Essa classe implementa o mesmo exemplo "Olá mundo" de antes, mas também define a autenticação do Windows como o esquema de autenticação.

Dentro da função `Main`, inicie o pipeline OWIN:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

Você pode enviar uma solicitação no Fiddler para confirmar que o aplicativo está usando a autenticação do Windows:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a>Tópicos Relacionados

[Uma visão geral do projeto Katana](an-overview-of-project-katana.md)

[System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx)

[Noções básicas sobre a autenticação de formulários OWIN no MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
