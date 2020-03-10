---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
title: Criando um farm de servidores com o Web farm Framework | Microsoft Docs
author: jrjlee
description: Este tópico descreve como usar o WFF (Web farm Framework) 2,0 para criar e configurar um farm de servidor Web de uma coleção de servidores.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 656dd06d-806c-467c-863d-9fc45e5ba3ab
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
msc.type: authoredcontent
ms.openlocfilehash: 204996514bed336e60ab77f184a923f04e7e2bba
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78637245"
---
# <a name="creating-a-server-farm-with-the-web-farm-framework"></a>Criação de um farm de servidores com o Web Farm Framework

por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como usar o WFF (Web farm Framework) 2,0 para criar e configurar um farm de servidor Web de uma coleção de servidores.

O WFF permite sincronizar produtos e componentes da plataforma Web, aplicativos Web, sites e definições de configuração entre vários servidores Web com balanceamento de carga. Em cenários em que você precisa de mais de um servidor Web, como ambientes de preparo e produção, isso pode simplificar muito o processo de implantação e configuração. Você pode implantar um aplicativo Web em um único servidor&#x2014;, o *servidor*&#x2014;primário e o WFF replicarão automaticamente esse aplicativo Web em todos os outros servidores Web no farm de servidores.

## <a name="understanding-the-web-farm-framework"></a>Noções básicas sobre a estrutura do Web farm

Você pode usar o WFF 2,0 para provisionar, gerenciar e implantar conteúdo em um grupo de servidores Web. Uma implantação do WFF consiste em três funções de servidor principais:

- O *servidor do controlador*. Use este servidor para criar e configurar farms de servidores WFF. O servidor do controlador gerencia a sincronização de componentes de plataforma da Web, definições de configuração e aplicativos entre os servidores Web em um farm de servidores. Você instala o WFF 2,0 no servidor do controlador, e o servidor do controlador, por sua vez, instalará o agente do WFF em cada um dos servidores em um farm de servidores. O servidor do controlador não pertence conceitualmente a nenhum farm de servidores WFF e um único servidor de controlador pode gerenciar vários farms de servidores. Nesse cenário, você usa um único servidor do controlador WFF para criar e gerenciar o farm de servidores de preparo e o farm de servidores de produção.
- O *servidor primário*. Cada farm de servidores do WFF inclui um único servidor primário. Quando você instala componentes da plataforma Web ou implanta aplicativos no servidor primário, o WFF sincroniza suas alterações para todos os outros servidores no farm de servidores.
- O *servidor secundário*. Cada farm de servidores do WFF inclui um ou mais servidores secundários. Todas as alterações feitas no servidor primário são replicadas para todos os servidores secundários no farm de servidores.

Isso mostra como essas funções de servidor se relacionam aos ambientes Fabrikam, Inc. de preparo e de produção:

![](creating-a-server-farm-with-the-web-farm-framework/_static/image1.png)

Nesse cenário, o ambiente de preparo e o ambiente de produção são configurados como farms de servidores WFF. Um único servidor do controlador WFF gerencia ambos os farms. Em cada farm de servidores, todas as alterações no servidor primário são replicadas para todos os servidores secundários.

Antes de começar a configurar seus ambientes de preparo e produção, recomendamos que você leia estes artigos para se familiarizar com os principais conceitos do WFF 2,0:

- [Visão geral do Web farm Framework 2,0 para IIS 7](https://go.microsoft.com/?linkid=9805126)
- [Configurando um farm de servidores com o Web farm Framework 2,0 para IIS 7](https://go.microsoft.com/?linkid=9805127)
- [Requisitos de sistema e plataforma para o Web farm Framework 2,0 para IIS 7](https://go.microsoft.com/?linkid=9805128)

## <a name="task-overview"></a>Visão geral da tarefa

Para concluir as tarefas e os passo a passos neste tópico, você precisará de pelo menos&#x2014;três servidores um controlador WFF, um servidor Web primário para o farm de servidores e um ou mais servidores Web secundários para o farm de servidores. Você pode adicionar mais servidores secundários a um farm de servidores WFF a qualquer momento. Em um alto nível, para criar e configurar um farm de servidores WFF para seu ambiente de preparo ou de produção, você precisará:

- Crie um servidor do controlador instalando Serviços de Informações da Internet (IIS) 7,5 e WFF 2,0.
- Prepare servidores primários e secundários criando uma conta de administrador comum e configurando exceções de firewall.
- Configure o farm de servidores usando o Gerenciador do IIS no servidor do controlador.
- Configure o balanceamento de carga usando o IIS Application Request Routing (ARR) ou uma tecnologia de balanceamento de carga alternativa.

As tarefas e orientações neste tópico pressupõem que você está começando com as compilações de servidor limpas que executam o Windows Server 2008 R2. Antes de começar, para cada servidor, verifique se:

- O Windows Server 2008 R2 Service Pack 1 e todas as atualizações disponíveis estão instaladas.
- O servidor está ingressado no domínio.
- O servidor tem um endereço IP estático.

> [!NOTE]
> Para obter mais informações sobre como unir computadores a um domínio, consulte [unindo computadores ao domínio e fazendo logon](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Para obter mais informações sobre como configurar endereços IP estáticos, consulte [configurar um endereço IP estático](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).

## <a name="create-the-wff-controller-server"></a>Criar o servidor do controlador WFF

Para criar um servidor do controlador WFF, você precisará instalar o IIS 7 ou posterior e o WFF 2,0 ou posterior. Nos bastidores, o WFF usa a ferramenta de implantação da Web do IIS (Implantação da Web) 2. x para sincronizar os servidores em seu farm. Se você usar o Web Platform Installer para instalar o WFF, o instalador baixará e instalará automaticamente Implantação da Web para você.

**Para criar o servidor do controlador WFF**

1. Baixe e instale o [Web Platform Installer](https://go.microsoft.com/?linkid=9739157).
2. Na parte superior da janela **Web Platform Installer 3,0** , clique em **produtos**.
3. No lado esquerdo da janela, no painel de navegação, clique em **servidor**.
4. Na linha de **configuração recomendada do IIS 7** , clique em **Adicionar**.
5. No <strong>Web farm Framework 2.</strong> <em>x</em> , clique em <strong>Adicionar</strong>.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image2.png)
6. Clique em **Instalar**. Observe que a Web Platform Installer adicionou a ferramenta de implantação da Web, juntamente com várias outras dependências, à lista de instalação.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image3.png)
7. Examine os termos de licença e, se você concordar com os termos, clique em **aceito**.
8. Quando a instalação for concluída, clique em **concluir**e feche a janela **Web Platform Installer 3,0** .

## <a name="configure-the-primary-and-secondary-servers"></a>Configurar os servidores primário e secundário

Antes de criar um farm de servidores do WFF, você deve concluir algumas tarefas de preparação nos servidores Web que criarão o farm:

- Adicione exceções de firewall para permitir que os recursos de **rede principal**, **administração remota**e **compartilhamento de arquivos e impressoras** se comuniquem com o servidor do controlador WFF.
- Crie uma conta de domínio (por exemplo, **FABRIKAM\stagingfarm**) em Active Directory e adicione-a ao grupo local de administradores em cada servidor. Você usará essa conta como a conta de administrador do farm de servidores ao criar o farm de servidores.

Para obter mais informações sobre como configurar essas exceções de firewall no firewall do Windows, consulte [requisitos de sistema e plataforma para o Web farm Framework 2,0 para IIS 7](https://go.microsoft.com/?linkid=9805128). Para outros sistemas de firewall, consulte a documentação do produto.

Você pode usar o procedimento a seguir para adicionar uma conta de domínio ao grupo local de administradores no Windows Server 2008 R2. Você deve executar esse procedimento em cada servidor que deseja adicionar ao farm&#x2014;de servidores em outras palavras, adicionar a mesma conta de domínio ao grupo local de administradores no servidor primário e em cada servidor secundário.

**Para adicionar uma conta de domínio ao grupo local de administradores**

1. No menu **Iniciar** , aponte para **Ferramentas administrativas**e clique em **Gerenciador do servidor**.
2. Na janela **Gerenciador do servidor** , no painel exibição de árvore, expanda **configuração**, expanda **usuários e grupos locais**e clique em **grupos**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image4.png)
3. No painel **grupos** , clique duas vezes em **Administradores**.
4. Na caixa de diálogo **Propriedades de administradores** , clique em **Adicionar**.
5. Na caixa de diálogo **Selecionar usuários, computadores, contas de serviço ou grupos** , digite (ou procure) na sua conta de domínio (por exemplo, **FABRIKAM\stagingfarm**) e clique em **OK**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image5.png)
6. Na caixa de diálogo **Propriedades de administradores** , clique em **OK**.

Os servidores agora estão prontos para serem adicionados a um farm de servidores. No caso do servidor primário, você pode configurar o servidor para atender aos requisitos do aplicativo antes ou depois de criar o farm&#x2014;de servidores em ambos os casos, o WFF sincronizará os servidores implantando os mesmos produtos, componentes ou configurações em seus servidores secundários. Para simplificar, este tutorial pressupõe que você configurará o servidor primário quando terminar de criar o farm de servidores.

## <a name="create-the-wff-server-farm"></a>Criar o farm de servidores WFF

Neste ponto, todos os servidores estão prontos para serem adicionados a um farm de servidores do WFF:

- Você instalou o WFF no servidor do controlador.
- Você configurou exceções de firewall em seus servidores Web primários e secundários.
- Você adicionou uma conta de domínio ao grupo local de administradores em seus servidores Web primários e secundários.

A próxima etapa é criar o farm de servidores no WFF. Você pode fazer isso no Gerenciador do IIS no servidor do controlador WFF.

**Para criar um farm de servidores WFF**

1. No servidor do controlador WFF, no menu **Iniciar** , aponte para **Ferramentas administrativas**e clique em **Gerenciador do serviços de informações da Internet (IIS)** .
2. No painel **conexões** , expanda o nó servidor local, clique com o botão direito do mouse em **farms de servidores**e clique em **criar farm de servidores**.
3. Na caixa de diálogo **criar farm de servidores** , digite um nome significativo para o farm de servidores (por exemplo, **farm de preparo**) e, em seguida, selecione **provisionar farm de servidores**.
4. Digite o nome de usuário e a senha da conta de domínio que você adicionou ao grupo de administradores locais em cada servidor.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image6.png)
5. Clique em **Próximo**.
6. Na página **adicionar servidores** , digite o nome de domínio totalmente qualificado (FQDN) do servidor primário, selecione **servidor primário**e clique em **Adicionar**.
7. Neste ponto, o WFF tentará contatar o servidor primário usando as credenciais fornecidas. Se a conexão for realizada com sucesso, o servidor primário será adicionado à tabela na página **adicionar servidores** .

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image7.png)

    > [!NOTE]
    > Talvez você tenha notado que o **servidor está disponível para balanceamento de carga** está selecionado por padrão. O WFF usa o módulo ARR do IIS para implementar o balanceamento de carga e, assim, distribuir solicitações entre os servidores Web no farm de servidores. Na maioria dos cenários, você apenas limparia a opção o **servidor está disponível para balanceamento de carga** , se quisesse usar uma solução de balanceamento de carga de terceiros.
8. Na página **adicionar servidores** , digite o FQDN do seu primeiro servidor secundário e clique em **Adicionar**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image8.png)
9. Repita a etapa 7 para quaisquer servidores secundários adicionais no farm e clique em **concluir**.

Seu farm de servidores do WFF agora está em execução. Todos os produtos ou componentes da plataforma da Web instalados no servidor primário e quaisquer aplicativos Web ou conteúdo que você implantar no servidor primário serão provisionados automaticamente em todos os seus servidores secundários.

WFF é um tópico amplo e complexo, e você pode aprender mais sobre ele no site do [Microsoft Web farm Framework 2,0 para IIS 7](https://go.microsoft.com/?linkid=9805129) . No entanto, por enquanto, há duas áreas de recursos que você precisa conhecer:

- O *provisionamento de aplicativos* é o processo que Replica o conteúdo do servidor primário, como aplicativos Web e definições de configuração, em todos os servidores secundários no farm de servidores. Por exemplo, se você implantar a solução de exemplo Contact Manager em seu servidor de preparo primário, o processo de provisionamento de aplicativo WFF implantará essa solução em todos os seus servidores de preparo secundários. Por padrão, o processo de provisionamento de aplicativos é executado a cada 30 segundos.
- O *provisionamento de plataforma* é o processo que sincroniza os produtos e componentes da plataforma Web do servidor primário para todos os servidores secundários no farm de servidores. Por exemplo, se você instalar o ASP.NET MVC 3 em seu servidor de preparo primário, o processo de provisionamento da plataforma usará o Web Platform Installer para instalar o ASP.NET MVC 3 em todos os seus servidores de preparação secundários. Por padrão, o processo de provisionamento da plataforma é executado a cada cinco minutos.

Você pode gerenciar as configurações básicas de provisionamento de aplicativos e plataformas do Gerenciador do IIS no servidor do controlador WFF.

**Explorar as configurações de provisionamento de aplicativos e plataformas**

1. No Gerenciador do IIS, no painel **conexões** , selecione o farm de servidores.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image9.png)
2. No painel **farm de servidores** , clique duas vezes em **provisionamento de aplicativos**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image10.png)
3. Como você pode ver, o farm de servidores está configurado atualmente para sincronizar o conteúdo da Web e as definições de configuração entre o servidor primário e os servidores secundários a cada 30 segundos.
4. Clique em **voltar**e clique duas vezes em **provisionamento de plataforma**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image11.png)
5. Como você pode ver, o farm de servidores está configurado atualmente para sincronizar produtos e componentes da plataforma Web entre o servidor primário e os servidores secundários a cada cinco minutos.
6. Clique em **Voltar**.
7. Para forçar o farm de servidores a sincronizar os produtos da plataforma Web imediatamente, no painel **ações** , clique em **provisionar plataforma**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image12.png)

    > [!NOTE]
    > O provisionamento de plataforma pode levar algum tempo. O processo do instalador é executado em segundo plano nos servidores secundários no farm de servidores.
8. Depois de ter permitido tempo suficiente para que o processo de provisionamento seja concluído, você pode verificar se os produtos e componentes adicionados ao servidor primário foram replicados nos servidores secundários. Por exemplo, você pode fazer logon em um servidor secundário e usar a janela **Gerenciador do servidor** para verificar se a função de servidor Web foi instalada.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image13.png)
9. Você também pode verificar a lista de programas instalados para verificar se vários componentes da plataforma da Web foram adicionados.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image14.png)

## <a name="configure-load-balancing"></a>Configurar balanceamento de carga

Ao criar um web farm, você precisa configurar alguma forma de balanceamento de carga para distribuir solicitações HTTP entre os servidores Web. Isso pode ser o balanceamento de carga de rede do Windows Server 2008, o IIS ARR ou uma solução de balanceamento de carga baseada em hardware ou em software de terceiros.

O WFF foi projetado para se integrar com o ARR do IIS. Para aproveitar essa integração, você precisa instalar o módulo ARR no servidor do controlador WFF. Em seguida, você direciona todo o tráfego da Web para o servidor do controlador, normalmente Configurando registros DNS (sistema de nomes de domínio). O servidor do controlador distribuirá as solicitações de entrada entre os servidores no farm, com base na disponibilidade do servidor e em vários outros critérios.

> [!NOTE]
> Você não precisa usar o ARR com WFF; Você pode configurar o WFF para trabalhar com soluções de balanceamento de carga de terceiros. Para obter mais informações, consulte [visão geral do Web farm Framework 2,0 para IIS 7](https://go.microsoft.com/?linkid=9805126).

O balanceamento de carga usando ARR é um tópico complexo, mas a maior parte está além do escopo deste tutorial. No entanto, você pode usar o próximo procedimento para instalar o módulo ARR e começar com o balanceamento de carga.

**Para configurar o balanceamento de carga no servidor do controlador WFF**

1. No servidor do controlador WFF, inicie o Web Platform Installer.
2. Na parte superior da janela **Web Platform Installer 3,0** , clique em **produtos**.
3. No lado esquerdo da janela, no painel de navegação, clique em **servidor**.
4. Na linha **Application Request Routing 2,5** , clique em **Adicionar**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image15.png)
5. Clique em **instalar**e siga as instruções na janela de **instalação da plataforma Web** .
6. Quando a instalação for concluída, inicie o Gerenciador do IIS e, no painel **conexões** , clique no nó do farm de servidores. Observe que vários ícones novos foram adicionados ao painel **farm de servidores** .

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image16.png)
7. No painel **farm de servidores** , clique duas vezes em **balanceamento de carga**.
8. No painel **balanceamento de carga** , selecione um algoritmo de balanceamento de carga (por exemplo, **menos a solicitação atual**).

    > [!NOTE]
    > Para obter mais informações sobre algoritmos de balanceamento de carga e outras definições de configuração, consulte [Application Request Routing Module](https://go.microsoft.com/?linkid=9805130).

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image17.png)
9. No painel **ações** , clique em **aplicar**.

Agora você configurou o balanceamento de carga básico para os servidores no farm de servidores. Se você direcionar todo o seu tráfego de web farm para o servidor do controlador, as solicitações serão distribuídas entre os servidores no seu farm de acordo com a disponibilidade e o algoritmo de balanceamento de carga selecionado.

Para obter mais informações sobre como configurar o balanceamento de carga com o ARR, consulte [Application Request Routing Module](https://go.microsoft.com/?linkid=9805130).

## <a name="monitor-the-server-farm"></a>Monitorar o farm de servidores

Você pode monitorar a integridade do farm de servidores a qualquer momento por meio do Gerenciador do IIS no servidor do controlador. No painel **conexões** , expanda o farm de servidores e clique em **servidores**. O painel central mostrará um resumo de cada servidor no farm, junto com um log de rastreamento da atividade recente.

![](creating-a-server-farm-with-the-web-farm-framework/_static/image18.png)

## <a name="conclusion"></a>Conclusão

O farm de servidores do WFF agora deve estar em execução. Você pode configurar o servidor primário para dar suporte a qualquer abordagem de&#x2014;implantação que preferir ver a seção de&#x2014;leitura adicional para obter detalhes e sua configuração será replicada em cada servidor secundário no farm de servidores.

## <a name="further-reading"></a>Leitura adicional

Para obter mais diretrizes sobre todos os aspectos de configuração e uso do WFF, consulte o site do [Microsoft Web farm Framework 2,0 para IIS 7](https://go.microsoft.com/?linkid=9805129) .

> [!div class="step-by-step"]
> [Anterior](configuring-a-database-server-for-web-deploy-publishing.md)
> [Próximo](configuring-deployment-properties-for-a-target-environment.md)
