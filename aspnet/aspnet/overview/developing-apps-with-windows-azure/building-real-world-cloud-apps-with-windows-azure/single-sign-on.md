---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
title: Logon único (criando aplicativos de nuvem do mundo real com o Azure) | Microsoft Docs
author: MikeWasson
description: A criação de aplicativos de nuvem do mundo real com o livro eletrônico do Azure é baseada em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas que podem...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 7d82d5e9-0619-4f22-9e03-32a6d52940a5
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 1ca93cce22487295a24aae95437b3e69dfc5b504
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78617442"
---
# <a name="single-sign-on-building-real-world-cloud-apps-with-azure"></a>Logon único (compilando aplicativos de nuvem do mundo real com o Azure)

por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Baixar o projeto de ti](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [baixar o livro eletrônico](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> A **criação de aplicativos de nuvem do mundo real com** o livro eletrônico do Azure é baseada em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas que podem ajudá-lo a desenvolver com sucesso aplicativos Web para a nuvem. Para obter informações sobre o livro eletrônico, consulte [o primeiro capítulo](introduction.md).

Há muitos problemas de segurança a serem considerados quando você está desenvolvendo um aplicativo de nuvem, mas para esta série, nos concentraremos em apenas um: logon único. Uma pergunta que as pessoas costumam fazer é: "Estou criando principalmente aplicativos para os funcionários da minha empresa; como hospedar esses aplicativos na nuvem e ainda permitir que eles usem o mesmo modelo de segurança que meus funcionários conhecem e usam no ambiente local quando estão executando aplicativos que estão hospedados no firewall? " Uma das maneiras de habilitar esse cenário é chamada de Azure Active Directory (Azure AD). O Azure AD permite que você crie aplicativos LOB (linha de negócios) da empresa disponíveis pela Internet e também permite tornar esses aplicativos disponíveis para parceiros de negócios.

## <a name="introduction-to-azure-ad"></a>Introdução ao Azure AD

O [Azure ad](https://docs.microsoft.com/azure/active-directory/) fornece [Active Directory](https://msdn.microsoft.com/library/windows/desktop/aa746492.aspx) na nuvem. Os principais recursos incluem o seguinte:

- Ele se integra com Active Directory locais.
- Ele habilita o logon único com seus aplicativos.
- Ele dá suporte a padrões abertos, como [SAML](http://en.wikipedia.org/wiki/SAML_2.0), [WS-enalimentado](http://en.wikipedia.org/wiki/WS-Federation)e [OAuth 2,0](http://oauth.net/2/).
- Ele dá suporte à [API REST do grafo](https://msdn.microsoft.com/library/hh974476.aspx)empresarial.

Suponha que você tenha um ambiente local Active Directory do Windows Server que você usa para permitir que os funcionários se conectem a aplicativos da intranet:

![](single-sign-on/_static/image1.png)

O que o Azure AD permite fazer é criar um diretório na nuvem. É um recurso gratuito e fácil de configurar.

Ele pode ser totalmente independente do seu Active Directory local; Você pode colocar qualquer um que desejar nele e autenticá-los em aplicativos de Internet.

![Windows Azure Active Directory](single-sign-on/_static/image2.png)

Ou você pode integrá-lo ao seu AD local.

![Integração do AD e do WAAD](single-sign-on/_static/image3.png)

Agora, todos os funcionários que podem ser autenticados localmente também podem se autenticar pela Internet – sem precisar abrir um firewall ou implantar novos servidores em seu data center. Você pode continuar a aproveitar todo o ambiente de Active Directory existente que você conhece e usar atualmente para fornecer o recurso de logon único de aplicativos internos.

Depois de fazer essa conexão entre o AD e o Azure AD, você também pode habilitar seus aplicativos Web e seus dispositivos móveis para autenticar seus funcionários na nuvem e pode habilitar aplicativos de terceiros, como Office 365, SalesForce.com ou Google Apps, para aceitar seu credenciais dos funcionários. Se você estiver usando o Office 365, já estará configurado com o Azure AD porque o Office 365 usa o Azure AD para autenticação e autorização.

![aplicativos de terceiros](single-sign-on/_static/image4.png)

A beleza dessa abordagem é que, sempre que sua organização adicionar ou excluir um usuário, ou um usuário alterar uma senha, você usará o mesmo processo que usará hoje em seu ambiente local. Todas as alterações do AD local são propagadas automaticamente para o ambiente de nuvem.

Se sua empresa estiver usando ou mudando para o Office 365, a boa notícia é que você terá o Azure AD configurado automaticamente porque o Office 365 usa o Azure AD para autenticação. Portanto, você pode usar facilmente em seus próprios aplicativos a mesma autenticação que o Office 365 usa.

## <a name="set-up-an-azure-ad-tenant"></a>Configurar um locatário do Azure AD

um diretório do AD do Azure é chamado de [locatário](https://technet.microsoft.com/library/jj573650.aspx)do Azure AD e a configuração de um locatário é muito fácil. Mostraremos como isso é feito na Portal de Gerenciamento do Azure para ilustrar os conceitos, mas é claro que as outras funções do portal também podem ser feitas usando um script ou uma API de gerenciamento.

No portal de gerenciamento, clique na guia Active Directory.

![WAAD no portal](single-sign-on/_static/image5.png)

Você tem automaticamente um locatário do Azure AD para sua conta do Azure e pode clicar no botão **Adicionar** na parte inferior da página para criar diretórios adicionais. Você pode desejar um para um ambiente de teste e outro para produção, por exemplo. Pense atentamente sobre o nome de um novo diretório. Se você usar seu nome para o diretório e, em seguida, usar seu nome novamente para um dos usuários, isso pode ser confuso.

![Adicionar diretório](single-sign-on/_static/image6.png)

O portal tem suporte completo para criar, excluir e gerenciar usuários nesse ambiente. Por exemplo, para adicionar um usuário, acesse a guia **usuários** e clique no botão **Adicionar usuário** .

![Botão Adicionar Usuário](single-sign-on/_static/image7.png)

![Caixa de diálogo Adicionar usuário](single-sign-on/_static/image8.png)

Você pode criar um novo usuário que existe somente nesse diretório ou pode registrar uma conta da Microsoft como um usuário nesse diretório ou registrar ou um usuário de outro diretório do Azure AD como um usuário nesse diretório. (Em um diretório real, o domínio padrão seria ContosoTest.onmicrosoft.com. Você também pode usar um domínio de sua escolha, como contoso.com.)

![Tipos de usuário](single-sign-on/_static/image9.png)

![Caixa de diálogo Adicionar usuário](single-sign-on/_static/image10.png)

Você pode atribuir o usuário a uma função.

![Perfil de Usuário](single-sign-on/_static/image11.png)

E a conta é criada com uma senha temporária.

![Senha temporária](single-sign-on/_static/image12.png)

Os usuários criados dessa maneira podem fazer logon imediatamente em seus aplicativos Web usando esse diretório de nuvem.

No entanto, o que é ótimo para o logon único corporativo é a guia **integração de diretórios** :

![Guia integração de diretórios](single-sign-on/_static/image13.png)

Se você habilitar a integração de diretórios e [baixar uma ferramenta](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx), poderá sincronizar esse diretório de nuvem com o Active Directory local existente que você já está usando dentro de sua organização. Em seguida, todos os usuários armazenados em seu diretório aparecerão nesse diretório na nuvem. Seus aplicativos de nuvem agora podem autenticar todos os seus funcionários usando suas credenciais de Active Directory existentes. E tudo isso é gratuito – a ferramenta de sincronização e o próprio Azure AD.

A ferramenta é um assistente que é fácil de usar, como você pode ver nessas capturas de tela. Essas instruções não são completas, apenas um exemplo que mostra o processo básico. Para obter informações mais detalhadas sobre como fazer, consulte os links na seção [recursos](#resources) no final do capítulo.

![Assistente de configuração da ferramenta de sincronização WAAD](single-sign-on/_static/image14.png)

Clique em **Avançar**e insira suas credenciais de Azure Active Directory.

![Assistente de configuração da ferramenta de sincronização WAAD](single-sign-on/_static/image15.png)

Clique em **Avançar**e insira suas credenciais do AD local.

![Assistente de configuração da ferramenta de sincronização WAAD](single-sign-on/_static/image16.png)

Clique em **Avançar**e, em seguida, indique se deseja armazenar um hash de suas senhas do AD na nuvem.

![Assistente de configuração da ferramenta de sincronização WAAD](single-sign-on/_static/image17.png)

O hash de senha que você pode armazenar na nuvem é um hash unidirecional; as senhas reais nunca são armazenadas no Azure AD. Se você decidir armazenar hashes na nuvem, precisará usar [serviços de Federação do Active Directory (AD FS)](https://technet.microsoft.com/library/hh831502.aspx) (ADFS). Também há [outros fatores a serem considerados ao escolher se deseja ou não usar o ADFS](https://technet.microsoft.com/library/jj573653.aspx). A opção ADFS requer algumas etapas de configuração adicionais.

Se você optar por armazenar hashes na nuvem, terminará e a ferramenta iniciará a sincronização de diretórios quando você clicar em **Avançar**.

![Assistente de configuração da ferramenta de sincronização WAAD](single-sign-on/_static/image18.png)

E, em alguns minutos, você está pronto.

![Assistente de configuração da ferramenta de sincronização WAAD](single-sign-on/_static/image19.png)

Você só precisa executar isso em um controlador de domínio na organização, no Windows 2003 ou superior. E não é necessário reinicializar. Quando terminar, todos os usuários estarão na nuvem e você poderá fazer logon único em qualquer aplicativo Web ou móvel, usando SAML, OAuth ou WS-enalimentado.

Às vezes, recebemos uma pergunta sobre quão seguro isso é: a Microsoft a utiliza para seus próprios dados comerciais confidenciais? E a resposta é sim. Por exemplo, se você acessar o site interno do Microsoft SharePoint em [https://microsoft.sharepoint.com/](https://microsoft.sharepoint.com/), você será solicitado a fazer logon.

![Entrar no Office 365](single-sign-on/_static/image20.png)

A Microsoft habilitou o ADFS, portanto, ao inserir uma ID da Microsoft, você será redirecionado para uma página de logon do ADFS.

![Entrada no ADFS](single-sign-on/_static/image21.png)

E, depois de inserir as credenciais armazenadas em uma conta interna do Microsoft AD, você terá acesso a esse aplicativo interno.

![Site do MS SharePoint](single-sign-on/_static/image22.png)

Estamos usando um servidor de entrada do AD principalmente porque já tínhamos o ADFS configurado antes que o Azure AD ficasse disponível, mas o processo de logon está passando por um diretório do Azure AD na nuvem. Colocamos nossos documentos importantes, controle do código-fonte, arquivos de gerenciamento de desempenho, relatórios de vendas e muito mais, na nuvem e estamos usando exatamente a mesma solução para protegê-los.

## <a name="create-an-aspnet-app-that-uses-azure-ad-for-single-sign-on"></a>Criar um aplicativo ASP.NET que usa o Azure AD para logon único

O Visual Studio torna muito fácil criar um aplicativo que usa o Azure AD para logon único, como você pode ver em algumas capturas de tela.

Quando você cria um novo aplicativo ASP.NET, o MVC ou o Web Forms, o método de autenticação padrão é ASP.NET Identity. Para alterar isso para o Azure AD, clique em um botão **alterar autenticação** .

![Alterar Autenticação](single-sign-on/_static/image23.png)

Selecione contas organizacionais, insira seu nome de domínio e, em seguida, selecione logon único.

![Caixa de diálogo configurar autenticação](single-sign-on/_static/image24.png)

Você também pode fornecer permissão de leitura/gravação do aplicativo para dados do diretório. Se você fizer isso, poderá usar a [API REST do Graph do Azure](https://msdn.microsoft.com/library/windowsazure/hh974476.aspx) para procurar o número de telefone dos usuários, descobrir se eles estão no escritório, quando eles fizeram logon pela última vez, etc.

Isso é tudo o que você precisa fazer – o Visual Studio solicita as credenciais de um administrador para seu locatário do Azure AD e, em seguida, configura seu projeto e seu locatário do Azure AD para o novo aplicativo.

Ao executar o projeto, você verá uma página de entrada e poderá entrar com as credenciais de um usuário em seu diretório do AD do Azure.

![Entrada da conta da organização](single-sign-on/_static/image25.png)

![Conectado](single-sign-on/_static/image26.png)

Quando você implanta o aplicativo no Azure, tudo o que precisa fazer é marcar uma caixa de seleção **habilitar autenticação organizacional** e, mais uma vez, o Visual Studio cuida de toda a configuração para você.

![Publicar na Web](single-sign-on/_static/image27.png)

Essas capturas de tela vêm de um tutorial passo a passo completo que mostra como criar um aplicativo que usa a autenticação do Azure AD: [desenvolvendo aplicativos ASP.NET com Azure Active Directory](../../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md).

## <a name="summary"></a>Resumo

Neste capítulo, você viu que Azure Active Directory, Visual Studio e ASP.NET, facilitam a configuração de logon único em aplicativos de Internet para os usuários da sua organização. Os usuários podem fazer logon em aplicativos de Internet usando as mesmas credenciais que usam para fazer logon usando Active Directory em sua rede interna.

O [próximo capítulo](data-storage-options.md) analisa as opções de armazenamento de dados disponíveis para um aplicativo de nuvem.

<a id="resources"></a>
## <a name="resources"></a>Recursos

Para saber mais, consulte os recursos a seguir:

- [Documentação do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/). Página do portal para a documentação do Azure AD no site do windowsazure.com. Para obter os tutoriais passo a passo, consulte a seção **desenvolver** .
- [Autenticação multifator do Azure](https://docs.microsoft.com/azure/multi-factor-authentication/). Página do portal para obter a documentação sobre a autenticação multifator no Azure.
- [Opções de autenticação da conta organizacional](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauthoptions). Explicação das opções de autenticação do Azure AD na caixa de diálogo Visual Studio 2013 New-Project.
- [Padrões e práticas da Microsoft – padrão de identidade federada](https://msdn.microsoft.com/library/dn589790.aspx).
- [HOWTO: instalar a ferramenta de sincronização de Azure Active Directory](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx).
- [Serviços de Federação do Active Directory (AD FS) o mapa de conteúdo do 2,0](https://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx). Links para documentação sobre o ADFS 2,0.
- [Autorização baseada em função e ACL em um aplicativo do Windows Azure ad](https://code.msdn.microsoft.com/Role-Based-and-ACL-Based-86ad71a1). Aplicativo de exemplo.
- [Azure Active Directory API do Graph blog](https://blogs.msdn.com/b/aadgraphteam/).
- [Controle de acesso no BYOD e integração de diretórios em uma infraestrutura de identidade híbrida](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/PCIT-B213#fbid=). Vídeo de sessão do Tech Ed 2014 por Gayana Bagdasaryan.

> [!div class="step-by-step"]
> [Anterior](web-development-best-practices.md)
> [Próximo](data-storage-options.md)
