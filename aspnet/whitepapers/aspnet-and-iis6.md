---
uid: whitepapers/aspnet-and-iis6
title: Executando o ASP.NET 1,1 com IIS 6,0 | Microsoft Docs
author: rick-anderson
description: Embora o Windows Server 2003 inclua o IIS 6,0 e o ASP.NET 1,1, esses componentes são desabilitados por padrão. Este white paper descreve como habilitar o IIS 6,0 um...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 2e7812f34481afe9a71927c0d9ba2a9abc9632e4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78523306"
---
# <a name="running-aspnet-11-with-iis-60"></a>Executar o ASP.NET 1.1 com o IIS 6.0

> Embora o Windows Server 2003 inclua o IIS 6,0 e o ASP.NET 1,1, esses componentes são desabilitados por padrão. Este white paper descreve como habilitar o IIS 6,0 e o ASP.NET 1,1 e recomenda várias definições de configuração para obter o desempenho ideal do IIS e do ASP.NET.
> 
> Aplica-se ao ASP.NET 1,1 e ao IIS 6,0.

O ASP.NET 1,1 é fornecido com o Windows Server 2003, que também inclui a versão mais recente do IIS (Internet Information Server) versão 6,0. O IIS 6,0 e o ASP.NET 1,1 foram projetados para integrar diretamente e ASP.NET agora usa como padrão o novo modelo de processo de trabalho do IIS 6,0.

## <a name="aspnet-11-is-not-installed-by-default"></a>O ASP.NET 1,1 não está instalado por padrão

Ao contrário das versões anteriores dos sistemas operacionais de servidor da Microsoft, o IIS (servidor de informações da Internet) não está habilitado por padrão; Nem é ASP.NET 1,1. Há duas opções para habilitar o IIS:

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a>Habilitando o IIS, opção #1 Assistente para configurar o servidor

O Windows Server 2003 envia um novo ' Assistente para configurar o servidor ' para ajudá-lo a configurar corretamente o servidor no modo desejado.

Para iniciar o assistente-observação, para executar o assistente, você deve estar conectado como administrador-ir para: iniciar | Programas | Ferramentas administrativas e selecione ' configurar seu servidor '.

Depois de selecionado, você deverá ver a tela de abertura ' Assistente para configurar o servidor ':

![](aspnet-and-iis6/_static/image1.jpg)

Clique em ' próximo &gt;':

![](aspnet-and-iis6/_static/image2.jpg)

Clique em ' Avançar &gt;'

![](aspnet-and-iis6/_static/image3.jpg)

Nessa tela, você precisará selecionar ' servidor de aplicativos (IIS, ASP.NET) como as opções a serem configuradas.

Clique em ' Avançar &gt;'.

![](aspnet-and-iis6/_static/image4.jpg)

Depois de selecionar para configurar o servidor como um servidor de aplicativos, essa tela será exibida solicitando quais recursos adicionais devem ser instalados. Nenhuma opção é selecionada por padrão. Para habilitar o ASP.NET automaticamente, você precisa selecionar ' habilitar ASP. NET '.

Clique em ' Avançar &gt;'.

![](aspnet-and-iis6/_static/image5.jpg)

Esta tela exibe as opções que devem ser instaladas.

Clique em ' Avançar &gt;'.

![](aspnet-and-iis6/_static/image6.jpg)

Você verá essa tela enquanto as opções selecionadas estiverem sendo instaladas. É normal ver outras caixas de diálogo exibidas à medida que os serviços estão sendo instalados. Além disso, você pode ser solicitado a fornecer o local do CD de instalação do Windows 2003 Server.

Clique em ' Avançar &gt;' ao concluir.

![](aspnet-and-iis6/_static/image7.jpg)

Clique em ' Concluir '-o Windows Server 2003 agora está configurado para dar suporte ao IIS 6,0 e ao ASP.NET 1,1.

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a>Habilitando o IIS, opção #2-configurar manualmente o IIS e o ASP.NET

Se você não quiser usar o ' Assistente para configurar o servidor ', poderá, opcionalmente, instalar o IIS 6,0 e o ASP.NET 1,1 usando "Adicionar ou remover programas" no painel de controle.

Primeiro, abra o painel de controle:

![](aspnet-and-iis6/_static/image8.jpg)

Em seguida, clique em "Adicionar/remover componentes do Windows", que abrirá o "Assistente de componentes do Windows":

![](aspnet-and-iis6/_static/image9.jpg)

Realce e marque ' servidor de aplicativos ' e clique em ' Detalhes? ' Button

![](aspnet-and-iis6/_static/image10.jpg)

Para instalar o ASP.NET, marque ' ASP. NET '.

Clique em ' OK ' para retornar ao assistente de componente do Windows. Clique em ' Avançar &gt;' no assistente de componentes do Windows para começar a instalar o:

![](aspnet-and-iis6/_static/image11.jpg)

É normal ver outras caixas de diálogo exibidas à medida que os serviços estão sendo instalados. Além disso, você pode ser solicitado a fornecer o local do CD de instalação do Windows 2003 Server.

Quando a instalação for concluída, você verá a última tela do assistente de componentes do Windows:

![](aspnet-and-iis6/_static/image12.jpg)

O IIS 6,0 e o ASP.NET 1,1 agora estão configurados e disponíveis.

## <a name="recommended-settings"></a>Configurações recomendadas

Ao executar o ASP.NET 1,1 com IIS 6,0, há várias definições de configuração que são recomendadas para obter o desempenho ideal do ASP.NET:

- Configurando limites de memória de processo de trabalho
- Configurando a reciclagem de processo de trabalho

### <a name="configuring-worker-process-memory-limits"></a>Configurando limites de memória de processo de trabalho

Por padrão, o IIS 6,0 não define um limite na quantidade de memória que o IIS tem permissão para usar. ASP. O recurso de cache do NET depende de uma limitação de memória para que o cache possa remover proativamente itens não utilizados da memória.

É recomendável que você configure o recurso de reciclagem de memória do IIS 6,0. Para configurar este gerenciador de Serviços de Informações da Internet aberto (iniciar | Programas | Ferramentas administrativas | Serviços de Informações da Internet). Depois de abrir, expanda a pasta ' pools de aplicativos ':

Para cada pool de aplicativos:

![](aspnet-and-iis6/_static/image13.jpg)

1. Clique com o botão direito do mouse no pool de aplicativos, por exemplo, ' DefaultAppPool ' e selecione ' Propriedades ':

![](aspnet-and-iis6/_static/image14.jpg)

2. Em seguida, habilite a reciclagem de memória clicando em ' máximo de memória usada (em megabytes): '. O valor não deve ser maior que a quantidade de memória física (não virtual) no servidor, uma boa aproximação é de 60% da memória física, ou seja, para um servidor com 512 MB de memória física, selecione 310. Também é recomendável que o máximo não exceda 800 MB ao usar um espaço de endereço de 2GB. Se o espaço de endereço de memória do servidor for 3GB, o limite máximo de memória para o processo de trabalho pode ser tão alto quanto 1, 800 MB:

![](aspnet-and-iis6/_static/image15.jpg)

Clique em ' Aplicar ' e em ' OK ' para sair da caixa de diálogo de propriedades. Repita isso para todos os pools de aplicativos disponíveis.

### <a name="configuring-worker-recycling"></a>Configurando a reciclagem de trabalho

Por padrão, o IIS 6,0 é configurado para reciclar seu processo de trabalho a cada 29 horas. Isso é um pouco agressivo para um aplicativo que executa o ASP.NET e é recomendável que a reciclagem automática do processo de trabalho esteja desabilitada.

Para desabilitar a reciclagem automática do processo de trabalho, primeiro abra o Gerenciador de Serviços de Informações da Internet (iniciar | Programas | Ferramentas administrativas | Serviços de Informações da Internet). Depois de abrir, expanda a pasta ' pools de aplicativos ':

![](aspnet-and-iis6/_static/image16.jpg)

Para cada pool de aplicativos:

1. Clique com o botão direito do mouse no pool de aplicativos, por exemplo, ' DefaultAppPool ' e selecione ' Propriedades ':

![](aspnet-and-iis6/_static/image17.jpg)

2. Desmarque ' reciclar processo de trabalho (em minutos): ':

![](aspnet-and-iis6/_static/image18.jpg)

Clique em ' Aplicar ' e em ' OK ' para sair da caixa de diálogo de propriedades. Repita isso para todos os pools de aplicativos disponíveis.

## <a name="granting-write-access-to-the-file-system"></a>Concedendo acesso de gravação ao sistema de arquivos

Se seu aplicativo exigir acesso de gravação ao sistema de arquivos e você estiver usando NTFS, precisará modificar uma ACL (lista de controle de acesso) na pasta ou no arquivo para conceder acesso ASP.NET.

Por exemplo, para conceder acesso de gravação ASP.NET ao c:\inetpub\wwwroot First Open Explorer e navegue até o diretório:

![](aspnet-and-iis6/_static/image19.jpg)

Em seguida, clique com o botão direito do mouse no diretório, por exemplo, ' wwwroot ' e selecione Propriedades. Depois que a caixa de diálogo Propriedades for aberta, selecione a guia "segurança":

![](aspnet-and-iis6/_static/image20.jpg)

O diretório c:\inetpub\wwwroot\ é um diretório especial no qual o grupo especial do IIS 6,0 ' IIS\_WPG ' já foi concedido a leitura &amp; execução, listar conteúdo da pasta e permissões de leitura. No entanto, para conceder permissão de gravação, você precisa clicar na caixa de seleção Permitir para gravação:

![](aspnet-and-iis6/_static/image21.jpg)

O IIS 6,0 agora tem permissão de gravação nessa pasta. Para conceder permissões de gravação em outras pastas, siga estas etapas-Observe que talvez seja necessário adicionar o grupo do IIS\_WPG se ele ainda não existir.

> [!CAUTION]
> Conceder permissão de gravação ao IIS\_WPG permitirá que qualquer aplicativo ASP.NET grave nesse diretório.

## <a name="supporting-integrated-authentication-with-sql-server"></a>Suporte à autenticação integrada com o SQL Server

A autenticação integrada permite que o SQL Server Aproveite a autenticação do Windows NT para validar SQL Server contas de logon. Isso permite que o usuário ignore o processo de logon SQL Server padrão. Com essa abordagem, um usuário de rede pode acessar um banco de dados SQL Server sem fornecer uma identificação de logon ou senha separada, pois SQL Server obtém as informações de usuário e senha do processo de segurança de rede do Windows NT.

Escolher a autenticação integrada para aplicativos ASP.NET é uma boa opção porque nenhuma credencial nunca é armazenada em sua cadeia de conexão para seu aplicativo. Em vez disso, a cadeia de conexão usada para se conectar ao SQL terá a seguinte aparência:

`"server=localhost; database=Northwind;Trusted_Connection=true"`

Essa cadeia de conexão informa SQL Server usar as credenciais do Windows do aplicativo tentando acessar o SQL Server. No caso do ASP.NET/IIS 6, isso seria uma conta no grupo do IIS\_WPG.

Para habilitar a autenticação integrada entre o SQL Server e o ASP.NET, você precisará primeiro garantir que SQL Server esteja configurado para autenticação integrada ou autenticação de modo misto-Verifique com seu DBA para determinar isso. Se SQL Server estiver em um desses dois modos, você poderá usar a autenticação integrada.

Abrir o Gerenciador de SQL Server Enterprise (iniciar | Programas | Microsoft SQL Server | Enterprise Manager), selecione o servidor apropriado e expanda a pasta segurança:

![](aspnet-and-iis6/_static/image22.jpg)

Se o grupo ' BUILTINT\IIS\_WPG ' não estiver listado, clique com o botão direito do mouse em logons e selecione ' New login:

![](aspnet-and-iis6/_static/image23.jpg)

Na caixa de texto ' nome: ', insira ' [nome do servidor/domínio] \IIS\_WPG ' ou clique no botão de reticências para abrir o seletor de usuário/grupo do Windows NT:

![](aspnet-and-iis6/_static/image24.jpg)

Selecione o grupo IIS\_WPG do computador atual e clique em ' Adicionar ' e em OK para fechar o seletor.

Você também precisa definir o banco de dados padrão e as permissões para acessar o banco de dados. Para definir o banco de dados padrão, escolha na lista suspensa, por exemplo, abaixo de Northwind está selecionado:

![](aspnet-and-iis6/_static/image25.jpg)

Em seguida, clique na guia acesso ao banco de dados:

![](aspnet-and-iis6/_static/image26.jpg)

Clique na caixa de seleção Permitir para cada banco de dados ao qual você deseja permitir acesso. Você também precisará selecionar funções de banco de dados, verificar o proprietário do\_do BD garantirá que o logon tenha todas as permissões necessárias para gerenciar e usar o banco de dados selecionado.

Clique em OK para sair da caixa de diálogo de propriedade. Seu aplicativo ASP.NET agora está configurado para dar suporte à autenticação de SQL Server integrada.

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a>Não execute o ASP.NET 1,0 no modo nativo do IIS 6,0

O ASP.NET 1,0 no IIS 6,0 só tem suporte no modo de compatibilidade do IIS 5.

Para configurar o ASP.NET 1,0 para ser executado no modo de compatibilidade do IIS 5,0, abra Gerenciador de Serviços de Internet e clique com o botão direito do mouse em sites e selecione Propriedades:

![](aspnet-and-iis6/_static/image27.jpg)

Alternar para a guia de serviço e verificar? Executar o serviço WWW no modo de isolamento do IIS 5,0?:

![](aspnet-and-iis6/_static/image28.jpg)
