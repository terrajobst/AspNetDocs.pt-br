---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
title: Configurando um servidor de banco de dados para publicação de Implantação da Web | Microsoft Docs
author: jrjlee
description: Este tópico descreve como configurar um servidor de banco de dados SQL Server 2008 R2 para dar suporte à implantação e à publicação na Web. As tarefas descritas neste tópico são co...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: e7c447f9-eddf-4bbe-9f18-3326d965d093
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
msc.type: authoredcontent
ms.openlocfilehash: ade3c1ba1c470092f512436f39b8831458408c2c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634613"
---
# <a name="configuring-a-database-server-for-web-deploy-publishing"></a>Configuração de um servidor de banco de dados para publicação de Implantação da Web

por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como configurar um servidor de banco de dados SQL Server 2008 R2 para dar suporte à implantação e à publicação na Web.
> 
> As tarefas descritas neste tópico são comuns a cada cenário&#x2014;de implantação, não importa se os servidores Web estão configurados para usar o serviço de agente remoto da ferramenta de implantação da Web do IIS (implantação da Web), o manipulador de implantação da Web ou a implantação offline ou se o aplicativo está sendo executado em um único servidor Web ou em um farm de servidores. A maneira como você implanta o banco de dados pode mudar de acordo com os requisitos de segurança e outras considerações. Por exemplo, você pode implantar o banco de dados com ou sem um exemplo de dado, e você pode implantar mapeamentos de função de usuário ou configurá-los manualmente após a implantação. No entanto, a maneira como você configura o servidor de banco de dados permanece a mesma.

Você não precisa instalar nenhum produto ou ferramenta adicional para configurar um servidor de banco de dados para dar suporte à implantação da Web. Supondo que o servidor de banco de dados e o servidor Web sejam executados em computadores diferentes, você simplesmente precisa:

- Permitir que SQL Server se comunique usando TCP/IP.
- Permitir tráfego de SQL Server por meio de firewalls.
- Forneça à conta da máquina do servidor Web um logon SQL Server.
- Mapeie o logon da conta do computador para qualquer função de banco de dados necessária.
- Forneça à conta que executará a implantação uma SQL Server permissões de logon e criador de banco de dados.
- Para dar suporte a implantações repetidas, mapeie o logon da conta de implantação para o **bd\_** função de banco de dados proprietário.

Este tópico mostrará como executar cada um desses procedimentos. As tarefas e orientações neste tópico pressupõem que você está começando com uma instância padrão do SQL Server 2008 R2 em execução no Windows Server 2008 R2. Antes de continuar, verifique se:

- O Windows Server 2008 R2 Service Pack 1 e todas as atualizações disponíveis estão instaladas.
- O servidor está ingressado no domínio.
- O servidor tem um endereço IP estático.
- SQL Server 2008 R2 Service Pack 1 e todas as atualizações disponíveis estão instaladas.

A instância de SQL Server só precisa incluir a função **serviços de mecanismo de banco de dados** , que é incluída automaticamente em qualquer instalação do SQL Server. No entanto, para facilitar a configuração e a manutenção, recomendamos que você inclua as **ferramentas de gerenciamento – básicas** e **ferramentas de gerenciamento –** funções de servidor completas.

> [!NOTE]
> Para obter mais informações sobre como unir computadores a um domínio, consulte [unindo computadores ao domínio e fazendo logon](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Para obter mais informações sobre como configurar endereços IP estáticos, consulte [configurar um endereço IP estático](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx). Para obter mais informações sobre como instalar o SQL Server, consulte [instalando o SQL Server 2008 R2](https://technet.microsoft.com/library/bb500395.aspx).

## <a name="enable-remote-access-to-sql-server"></a>Habilitar o acesso remoto para SQL Server

SQL Server usa TCP/IP para se comunicar com computadores remotos. Se o servidor de banco de dados e o servidor Web estiverem em computadores diferentes, você precisará:

- Defina SQL Server configurações de rede para permitir a comunicação por TCP/IP.
- Configure qualquer firewall de hardware ou software para permitir o tráfego TCP (e, em alguns casos, o tráfego UDP) nas portas que a instância de SQL Server usa.

Para permitir que SQL Server se comuniquem por TCP/IP, use SQL Server Configuration Manager para alterar a configuração de rede para sua instância de SQL Server.

**Para permitir que SQL Server se comunique usando TCP/IP**

1. No menu **Iniciar** , aponte para **todos os programas**, clique **em Microsoft SQL Server 2008 R2**, clique em **ferramentas de configuração**e, em seguida, clique em **SQL Server Configuration Manager**.
2. No painel exibição de árvore, expanda **SQL Server configuração de rede**e clique em **protocolos para MSSQLSERVER**.

   > [!NOTE]
   > Se você tiver instalado várias instâncias do SQL Server, verá um <strong>protocolo para</strong>o item<em>[nome da instância]</em> para cada instância. Você precisa definir as configurações de rede em uma base de instância por instância.
3. No painel de detalhes, clique com o botão direito do mouse na linha **TCP/IP** e clique em **habilitar**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image1.png)
4. Na caixa de diálogo de **aviso** , clique em **OK**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image2.png)
5. Você precisa reiniciar o serviço MSSQLSERVER antes de sua nova configuração de rede entrar em vigor. Você pode fazer isso em um prompt de comando, no console de serviços ou em SQL Server Management Studio. Neste procedimento, você usará SQL Server Management Studio.
6. Feche o SQL Server Configuration Manager.
7. No menu **Iniciar** , aponte para **todos os programas**, clique em **Microsoft SQL Server 2008 R2**e, em seguida, clique em **SQL Server Management Studio**.
8. Na caixa de diálogo **conectar ao servidor** , na caixa **nome do servidor** , digite o nome do servidor de banco de dados e clique em **conectar**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image3.png)
9. No painel Pesquisador de **objetos** , clique com o botão direito do mouse no nó do servidor pai (por exemplo, **TESTDB1**) e clique em **reiniciar**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image4.png)
10. Na caixa de diálogo **Microsoft SQL Server Management Studio** , clique em **Sim**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image5.png)
11. Quando o serviço for reiniciado, feche SQL Server Management Studio.

Para permitir o tráfego de SQL Server por meio de um firewall, primeiro você precisa saber quais portas sua instância de SQL Server está usando. Isso dependerá de como a instância de SQL Server foi criada e configurada:

- Uma *instância padrão* do SQL Server ouve (e responde a) solicitações na porta TCP 1433.
- Uma *instância nomeada* do SQL Server ouve (e responde a) solicitações em uma porta TCP atribuída dinamicamente.
- Se o serviço de SQL Server Browser estiver habilitado, os clientes poderão consultar o serviço na porta UDP 1434 para descobrir qual porta TCP usar para uma instância de SQL Server específica. No entanto, esse serviço geralmente é desabilitado por motivos de segurança.

Supondo que você esteja usando uma instância padrão do SQL Server, você precisa configurar o firewall para permitir o tráfego.

| Direção | Da porta | Para porta | Tipo de Porta |
| --- | --- | --- | --- |
| Entrada | Qualquer | 1433 | TCP |
| Saída | 1433 | Qualquer | TCP |

> [!NOTE]
> Tecnicamente, um computador cliente usará uma porta TCP atribuída aleatoriamente entre 1024 e 5000 para se comunicar com SQL Server, e você poderá restringir suas regras de firewall de acordo. Para obter mais informações sobre SQL Server portas e firewalls, consulte [números de porta TCP/IP necessários para se comunicar com o SQL por meio de um firewall](https://go.microsoft.com/?linkid=9805125) e [como configurar um servidor para escutar em uma porta TCP específica (SQL Server Configuration Manager)](https://msdn.microsoft.com/library/ms177440.aspx).

Na maioria dos ambientes do Windows Server, você provavelmente precisará configurar o Firewall do Windows no servidor de banco de dados. Por padrão, o Firewall do Windows permite todo o tráfego de saída, a menos que uma regra o proíba especificamente. Para permitir que o servidor Web alcance seu banco de dados, você precisa configurar uma regra de entrada que permita o tráfego TCP no número da porta que a instância de SQL Server usa. Se você estiver usando uma instância padrão do SQL Server, poderá usar o procedimento a seguir para configurar essa regra.

**Para configurar o Firewall do Windows para permitir a comunicação com uma instância de SQL Server padrão**

1. No servidor de banco de dados, no menu **Iniciar** , aponte para **Ferramentas administrativas**e clique em **Firewall do Windows com segurança avançada**.
2. No painel exibição de árvore, clique em **regras de entrada**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image6.png)
3. No painel **ações** , em **regras de entrada**, clique em **nova regra**.
4. No Assistente para nova regra de entrada, na página **tipo de regra** , selecione **porta**e clique em **Avançar**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image7.png)
5. Na página **protocolo e portas** , verifique se **TCP** está selecionado e, na caixa **portas locais específicas** , digite **1433**e clique em **Avançar**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image8.png)
6. Na página **ação** , deixe **permitir a conexão** selecionada e clique em **Avançar**.
7. Na página **perfil** , deixe **domínio** selecionado, desmarque as caixas de seleção **privado** e **público** e clique em **Avançar**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image9.png)
8. Na página **nome** , dê ao regra um nome descritivo adequado (por exemplo, **SQL Server instância padrão – acesso à rede**) e clique em **concluir**.

Para obter mais informações sobre como configurar o Firewall do Windows para SQL Server, especialmente se você precisar se comunicar com SQL Server por meio de portas não padrão ou dinâmicas, consulte [como configurar um firewall do Windows para acesso ao mecanismo de banco de dados](https://technet.microsoft.com/library/ms175043.aspx).

## <a name="configure-logins-and-database-permissions"></a>Configurar logons e permissões de banco de dados

Quando você implanta um aplicativo Web no Serviços de Informações da Internet (IIS), o aplicativo é executado usando a identidade do pool de aplicativos. Em um ambiente de domínio, as identidades do pool de aplicativos usam a conta da máquina do servidor no qual elas são executadas para acessar os recursos da rede. As contas de computador usam o formato <em>[nome de domínio]</em><strong>\</strong ><em>[nome da máquina]</em> <strong>$</strong> &#x2014;por exemplo, <strong>FABRIKAM\TESTWEB1 $</strong>. Para permitir que seu aplicativo Web acesse um banco de dados pela rede, você precisa:

- Adicione um logon para a conta do computador do servidor Web à instância de SQL Server.
- Mapeie o logon da conta da máquina para qualquer função de banco de dados necessária (normalmente, o **bd\_DataReader** e o **DB\_DataWriter**).

Se seu aplicativo Web estiver em execução em um farm de servidores, em vez de em um único servidor, você precisará repetir esses procedimentos para cada servidor Web no farm de servidores.

> [!NOTE]
> Para obter mais informações sobre identidades de pool de aplicativos e acesso a recursos de rede, consulte [identidades do pool de aplicativos](https://go.microsoft.com/?linkid=9805123).

Você pode abordar essas tarefas de várias maneiras. Para criar o logon, você pode:

- Crie o logon manualmente no servidor de banco de dados, usando Transact-SQL ou SQL Server Management Studio.
- Use um projeto de servidor SQL Server 2008 no Visual Studio para criar e implantar o logon.

Um logon SQL Server é um objeto de nível de servidor, em vez de um objeto de nível de banco de dados, portanto, não depende do banco de dados que você deseja implantar. Assim, você pode criar o logon a qualquer momento, e a abordagem mais fácil costuma criar o logon manualmente no servidor de banco de dados antes de iniciar a implantação de bancos de dados. Você pode usar o procedimento a seguir para criar um logon no SQL Server Management Studio.

**Para criar um logon SQL Server para a conta do computador do servidor Web**

1. No servidor de banco de dados, no menu **Iniciar** , aponte para **todos os programas**, clique em **Microsoft SQL Server 2008 R2**e, em seguida, clique em **SQL Server Management Studio**.
2. Na caixa de diálogo **conectar ao servidor** , na caixa **nome do servidor** , digite o nome do servidor de banco de dados e clique em **conectar**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image10.png)
3. No painel Pesquisador de **objetos** , clique com o botão direito do mouse em **segurança**, aponte para **novo**e clique em **logon**.
4. Na caixa de diálogo **logon – novo** , na caixa **nome de logon** , digite o nome da conta do computador do servidor Web (por exemplo, **FABRIKAM\TESTWEB1 $** ).

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image11.png)
5. Clique em **OK**.

Neste ponto, o servidor de banco de dados está pronto para a publicação Implantação da Web. No entanto, as soluções que você implantar não funcionarão até que você mapeie o logon da conta da máquina para as funções de banco de dados necessárias. O mapeamento do logon para funções de banco de dados requer muito mais pensamento, pois você não pode mapear funções até depois de ter implantado o banco de dados. Para mapear o logon da conta de computador para as funções de banco de dados necessárias, você pode:

- Atribua as funções de banco de dados ao logon manualmente, depois de ter implantado o banco de dados pela primeira vez.
- Use um script pós-implantação para atribuir as funções de banco de dados ao logon.

Para obter mais informações sobre como automatizar a criação de logons e mapeamentos de função de banco de dados, consulte [implantando associações de função de banco de dados para ambientes de teste](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Como alternativa, você pode usar o procedimento a seguir para mapear o logon da conta do computador para as funções de banco de dados necessárias manualmente. Lembre-se de que você não pode executar esse procedimento até *depois* de ter implantado o banco de dados.

**Para mapear funções de banco de dados para o logon de conta do computador do servidor Web**

1. Abra SQL Server Management Studio como antes.
2. No painel Pesquisador de **objetos** , expanda o nó **segurança** , expanda o nó **logons** e clique duas vezes no logon da conta do computador (por exemplo, **FABRIKAM\TESTWEB1 $** ).

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image12.png)
3. Na caixa de diálogo **Propriedades de logon** , clique em **mapeamento de usuário**.
4. Na tabela **Usuários mapeados para este logon** , selecione o nome do seu banco de dados (por exemplo, **ContactManager**).
5. Na lista **Associação de função de banco de dados para:** *[nome do banco de dados]* , selecione as permissões necessárias. No caso da solução de exemplo do Gerenciador de contatos, você deve selecionar as funções de **banco de\_DataReader** e **DB\_DataWriter** .

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image13.png)
6. Clique em **OK**.

Embora o mapeamento manual das funções de banco de dados seja geralmente mais do que adequado para ambientes de teste, ele é menos desejável para implantações automatizadas ou de um clique em ambientes de preparo ou produção. Você pode encontrar mais informações sobre como automatizar esse tipo de tarefa usando scripts de pós-implantação em [implantando associações de função de banco de dados para ambientes de teste](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md).

> [!NOTE]
> Para obter mais informações sobre projetos de servidor e projetos de banco de dados, consulte [projetos de banco de dados do Visual Studio 2010 SQL Server](https://msdn.microsoft.com/library/ff678491.aspx).

## <a name="configure-permissions-for-the-deployment-account"></a>Configurar permissões para a conta de implantação

Se a conta que você usar para executar a implantação não for um administrador SQL Server, você também precisará criar um logon para essa conta. Para criar o banco de dados, a conta deve ser um membro da função de servidor **dbcreator** ou ter permissões equivalentes.

> [!NOTE]
> Ao usar Implantação da Web ou VSDBCMD para implantar um banco de dados, você pode usar credenciais do Windows ou credenciais de SQL Server (se sua instância de SQL Server estiver configurada para oferecer suporte à autenticação de modo misto). O próximo procedimento pressupõe que você deseja usar as credenciais do Windows, mas não há nada que impeça você de especificar um SQL Server nome de usuário e senha na cadeia de conexão ao configurar a implantação.

**Para configurar permissões para a conta de implantação**

1. Abra SQL Server Management Studio como antes.
2. No painel Pesquisador de **objetos** , clique com o botão direito do mouse em **segurança**, aponte para **novo**e clique em **logon**.
3. Na caixa de diálogo **logon – novo** , na caixa **nome de logon** , digite o nome da sua conta de implantação (por exemplo, **FABRIKAM\matt**).
4. No painel **selecionar uma página** , clique em **funções de servidor**.
5. Selecione **dbcreator**e clique em **OK**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image14.png)

Para dar suporte a implantações subsequentes, você também precisará adicionar a conta de implantação ao **bd\_** função de proprietário no banco de dados após a primeira implantação. Isso ocorre porque, em implantações subsequentes, você está modificando o esquema de um banco de dados existente, em vez de criar um novo banco de dados. Conforme descrito na seção anterior, você não pode adicionar um usuário a uma função de banco de dados até que você tenha criado o banco de dados, por motivos óbvios.

**Para mapear o logon da conta de implantação para o BD\_função de banco de dados proprietário**

1. Abra SQL Server Management Studio como antes.
2. Na janela pesquisador de **objetos** , expanda o nó **segurança** , expanda o nó **logons** e clique duas vezes no logon da conta do computador (por exemplo, **FABRIKAM\matt**).
3. Na caixa de diálogo **Propriedades de logon** , clique em **mapeamento de usuário**.
4. Na tabela **Usuários mapeados para este logon** , selecione o nome do seu banco de dados (por exemplo, **ContactManager**).
5. Na lista **Associação de função de banco de dados para:** *[nome do banco de dados]* , selecione a função de **proprietário do\_do BD** .

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image15.png)
6. Clique em **OK**.

## <a name="conclusion"></a>Conclusão

Agora, o servidor de banco de dados deve estar pronto para aceitar implantações de banco de dados remoto e permitir que servidores Web IIS remotos acessem seus bancos. Antes de tentar implantar e usar bancos de dados, talvez você queira verificar estes pontos-chave:

- Você configurou SQL Server para aceitar conexões TCP/IP remotas?
- Você configurou os firewalls para permitir o tráfego de SQL Server?
- Você criou um logon de conta de computador para cada servidor Web que acessará SQL Server?
- A implantação do banco de dados inclui um script para criar mapeamentos de função de usuário ou você precisa criá-los manualmente depois de implantar o banco de dados pela primeira vez?
- Você criou um logon para a conta de implantação e a adicionou à função de servidor **dbcreator** ?

## <a name="further-reading"></a>Leitura adicional

Para obter orientação sobre a implantação de projetos de banco de dados, consulte [Implantando projetos de banco de dados](../web-deployment-in-the-enterprise/deploying-database-projects.md). Para obter orientação sobre como criar associações de função de banco de dados executando um script pós-implantação, consulte [implantando associações de função de banco de dados para ambientes de teste](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Para obter orientação sobre como atender aos desafios de implantação exclusivos que os bancos de dados de associação representam, consulte [implantando bancos de dados de associação em ambientes corporativos](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

> [!div class="step-by-step"]
> [Anterior](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
> [Próximo](creating-a-server-farm-with-the-web-farm-framework.md)
