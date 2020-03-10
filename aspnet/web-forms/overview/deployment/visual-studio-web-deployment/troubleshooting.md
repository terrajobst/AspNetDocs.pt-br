---
uid: web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting
title: 'Implantação da Web do ASP.NET usando o Visual Studio: solução de problemas | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um aplicativo Web ASP.NET em aplicativos Web do serviço Azure App ou em um provedor de Hospedagem de terceiros, por usin...
ms.author: riande
ms.date: 06/01/2015
ms.assetid: c0090595-ab3b-4b9b-9e16-7a1891e8cb2f
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: b42476fca18b04f4557a216ee205cfd9220023e8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78576093"
---
# <a name="aspnet-web-deployment-using-visual-studio-troubleshooting"></a>Implantação da Web do ASP.NET usando o Visual Studio: solução de problemas

por [Tom Dykstra](https://github.com/tdykstra)

[Baixar o projeto inicial](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta série de tutoriais mostra como implantar (publicar) um aplicativo Web ASP.NET em aplicativos Web do serviço Azure App ou em um provedor de Hospedagem de terceiros usando o Visual Studio 2012 ou o Visual Studio 2010. Para obter informações sobre a série, consulte [o primeiro tutorial da série](introduction.md).

Esta página descreve alguns problemas comuns que podem surgir quando você implanta um aplicativo Web ASP.NET usando o Visual Studio. Para cada uma, uma ou mais causas possíveis e as soluções correspondentes são fornecidas.

Os cenários mostrados se aplicam ao Azure e a provedores de Hospedagem de terceiros. Para saber mais sobre como solucionar problemas de aplicativos Web no Serviço de Aplicativo do Azure, confira os seguintes recursos:

- [Solucionar problemas de um aplicativo Web no Serviço de Aplicativo do Azure usando o Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)
- [Monitorar aplicativos Web no Serviço de Aplicativo do Azure](https://azure.microsoft.com/documentation/articles/web-sites-monitor//)
- [Anunciando o lançamento do SDK do Windows Azure 2,0 para .net](http://https://weblogs.asp.net/scottgu/announcing-the-release-of-windows-azure-sdk-2-0-for-net) (blog do ScottGu, mostra como obter logs de diagnóstico no Visual Studio)

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a>Erro de servidor no aplicativo '/'-as configurações de erro personalizadas atuais impedem que os detalhes do erro sejam exibidos remotamente

### <a name="scenario"></a>Cenário

Depois de implantar um site em um host remoto, você recebe uma mensagem de erro que menciona a configuração customErrors no arquivo Web. config, mas não indica qual é a causa real do erro:

[!code-xml[Main](troubleshooting/samples/sample1.xml)]

### <a name="possible-cause-and-solution"></a>Possível causa e solução

Por padrão, ASP.NET mostra informações de erro detalhadas somente quando o aplicativo Web está em execução no computador local. Em geral, você não deseja exibir informações detalhadas de erro quando seu aplicativo Web está publicamente disponível pela Internet, pois os hackers podem usar essas informações para encontrar vulnerabilidades no aplicativo. No entanto, quando você estiver implantando um site do ou atualizações em um site, algumas vezes algo dará errado e você precisará obter a mensagem de erro real.

Para permitir que o aplicativo exiba mensagens de erro detalhadas quando ele for executado no host remoto, edite o arquivo Web. config para definir o modo customErrors como desativado, reimplante o aplicativo e execute o aplicativo novamente:

1. Se o arquivo Web. config do aplicativo tiver um elemento customErrors no elemento System. Web, altere o atributo Mode para "off". Caso contrário, adicione um elemento customErrors no elemento System. Web com o atributo mode definido como "off", conforme mostrado no exemplo a seguir: 

    [!code-xml[Main](troubleshooting/samples/sample2.xml)]
2. Implante o aplicativo.
3. Execute o aplicativo e repita o que fez anteriormente, o que causou a ocorrência do erro. Agora você pode ver qual é a mensagem de erro real.
4. Quando você tiver resolvido o erro, restaure a configuração original customErrors e reimplante o aplicativo.

## <a name="cannot-createshadow-copy-contosouniversity-when-that-file-already-exists"></a>Não é possível criar/copiar a cópia ' ContosoUniversity ' quando esse arquivo já existe.

### <a name="scenario"></a>Cenário

Ao tentar executar um projeto no Visual Studio, você obtém uma página de erro com uma mensagem semelhante ao exemplo a seguir:

Erro de servidor no aplicativo '/'. Não é possível criar/copiar a cópia ' ContosoUniversity ' quando esse arquivo já existe.

### <a name="possible-cause-and-solution"></a>Possível causa e solução

Aguarde um minuto e atualize o navegador ou recompile o site e tente executá-lo novamente.

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a>Acesso negado em uma página da Web que usa SQL Server Compact

### <a name="scenario"></a>Cenário

Quando você implanta um site que usa SQL Server Compact e executa uma página no site implantado que acessa o banco de dados do, você vê a seguinte mensagem de erro:

Acesso negado. (Exceção de HRESULT: 0x80070005 (E\_ACCESSDENIED))

### <a name="possible-cause-and-solution"></a>Possível causa e solução

A conta de serviço de rede no servidor precisa ser capaz de ler os binários do SQL Service Compact nativo que estão na pasta *bin\amd64* ou *bin\x86* , mas não tem permissões de leitura para essas pastas. Defina permissão de leitura para serviço de rede na pasta *bin* , tornando-se estender as permissões para subpastas.

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a>Não é possível ler o arquivo de configuração devido a permissões insuficientes

### <a name="scenario"></a>Cenário

Quando você clica no botão Publicar do Visual Studio para implantar um aplicativo no IIS em seu computador local, a publicação falha e a janela **saída** mostra uma mensagem de erro semelhante a esta:

Ocorreu um erro ao ler o arquivo de configuração do IIS ' máquina/redirecionamento '. A identidade que está executando esta operação foi... Erro: não é possível ler o arquivo de configuração devido a permissões insuficientes.

### <a name="possible-cause-and-solution"></a>Possível causa e solução

Para usar a publicação de um clique no IIS em seu computador local, você deve estar executando o Visual Studio com permissões de administrador. Feche o Visual Studio e reinicie-o com permissões de administrador.

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a>Não foi possível conectar ao computador de destino... Usando o processo especificado

### <a name="scenario"></a>Cenário

Quando você clica no botão Publicar do Visual Studio para implantar um aplicativo, a publicação falha e a janela **saída** mostra uma mensagem de erro semelhante a esta:

[!code-console[Main](troubleshooting/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a>Possível causa e solução

Um servidor proxy está interrompendo a comunicação com o servidor de destino. No painel de controle do Windows ou no Internet Explorer, selecione **Opções da Internet** e selecione a guia **conexões** . Na caixa de diálogo **Propriedades da Internet** , clique em **configurações de LAN**. Na caixa de diálogo **configurações da LAN (rede local)** , desmarque a caixa de seleção **detectar configurações automaticamente** . Em seguida, clique no botão Publicar novamente.

Se o problema persistir, contate o administrador do sistema para determinar o que pode ser feito com as configurações de proxy ou firewall. O problema ocorre porque Implantação da Web usa uma porta não padrão para a implantação do serviço de gerenciamento da Web (8172); para outras conexões, Implantação da Web usa a porta 80. Quando você está implantando em um provedor de Hospedagem de terceiros, normalmente está usando o serviço de gerenciamento da Web.

## <a name="default-net-40-application-pool-does-not-exist"></a>O pool de aplicativos padrão do .NET 4,0 não existe

### <a name="scenario"></a>Cenário

Ao implantar um aplicativo que requer o .NET Framework 4, você verá a seguinte mensagem de erro:

O pool de aplicativos padrão do .NET 4,0 não existe ou não foi possível adicionar o aplicativo. Verifique se o ASP.NET 4,0 está instalado neste computador.

### <a name="possible-cause-and-solution"></a>Possível causa e solução

O ASP.NET 4 não está instalado no IIS. Se o servidor no qual você está implantando for o seu computador de desenvolvimento e tiver o Visual Studio 2010 instalado, o ASP.NET 4 será instalado no computador, mas poderá não estar instalado no IIS. No servidor em que você está implantando, abra um prompt de comando com privilégios elevados e instale o ASP.NET 4 no IIS executando os seguintes comandos:

[!code-console[Main](troubleshooting/samples/sample4.cmd)]

Talvez você também precise definir manualmente a versão .NET Framework do pool de aplicativos padrão. Para obter mais informações, consulte o tutorial implantando no IIS como um ambiente de teste nesta série.

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a>O formato da cadeia de inicialização não está de acordo com a especificação que começa no índice 0.

### <a name="scenario"></a>Cenário

Depois de implantar um aplicativo usando a publicação com um clique, quando você executar uma página que acessa o banco de dados, receberá a seguinte mensagem de erro:

O formato da cadeia de inicialização não está de acordo com a especificação que começa no índice 0.

### <a name="possible-cause-and-solution"></a>Possível causa e solução

Abra o arquivo *Web. config* no site implantado e verifique se os valores da cadeia de conexão começam com `$(ReplaceableToken_`, como no exemplo a seguir:

[!code-xml[Main](troubleshooting/samples/sample5.xml)]

Se as cadeias de conexão se parecerem com este exemplo, edite o arquivo de projeto e adicione a seguinte propriedade ao elemento PropertyGroup que é para todas as configurações de compilação:

[!code-xml[Main](troubleshooting/samples/sample6.xml)]

Em seguida, reimplante o aplicativo.

## <a name="http-500-internal-server-error"></a>Erro interno do servidor HTTP 500

### <a name="scenario"></a>Cenário

Ao executar o site implantado, você verá a seguinte mensagem de erro sem informações específicas indicando a causa do erro:

Erro HTTP 500-erro interno do servidor.

### <a name="possible-cause-and-solution"></a>Possível causa e solução

Há muitas causas de erros 500, mas uma causa possível se você estiver seguindo esses tutoriais é colocar um elemento XML no local errado em um dos arquivos de transformação Web. config. Por exemplo, você receberá esse erro se colocar a transformação que insere um elemento &lt;local&gt; em &lt;System. Web&gt; em vez de diretamente em &lt;Configuration&gt;. Você pode usar o recurso de visualização de transformação Web. config para verificar se as transformações estão funcionando como pretendida. A solução se você encontrar uma transformação codificada incorretamente é corrigir o arquivo de transformação e reimplantar. Se um erro não for óbvio, tente comentar as transformações e reimplantar para ver qual delas está causando o erro 500.

## <a name="http-50021-internal-server-error"></a>Erro interno do servidor HTTP 500,21

### <a name="scenario"></a>Cenário

Ao executar o site implantado, você verá a seguinte mensagem de erro:

Erro HTTP 500,21-erro interno do servidor. O manipulador "PageHandlerFactory-Integrated" tem um módulo inadequado "ManagedPipelineHandler" em sua lista de módulos.

### <a name="possible-cause-and-solution"></a>Possível causa e solução

O site que você implantou tem como destino o ASP.NET 4, mas o ASP.NET 4 não está registrado no IIS no servidor. No servidor, abra um prompt de comandos com privilégios elevados e registre o ASP.NET 4 executando os seguintes comandos:

[!code-console[Main](troubleshooting/samples/sample7.cmd)]

Talvez você também precise definir manualmente a versão .NET Framework do pool de aplicativos padrão. Para obter mais informações, consulte o tutorial implantando no IIS como um ambiente de teste nesta série.

## <a name="login-failed-opening-sql-server-express-database-in-app_data"></a>Falha de logon ao abrir o banco de dados SQL Server Express no aplicativo\_data

### <a name="scenario"></a>Cenário

Você atualizou a cadeia de conexão do arquivo *Web. config* para apontar para um banco de dados SQL Server Express como um arquivo *. MDF* em sua pasta de *\_de dados do aplicativo* e, na primeira vez que executar o aplicativo, você verá a seguinte mensagem de erro:

System. Data. SqlClient. SqlException: não é possível abrir o banco de dados "DatabaseName" solicitado pelo logon. Falha no logon.

### <a name="possible-cause-and-solution"></a>Possível causa e solução

O nome do arquivo *. MDF* não pode corresponder ao nome de qualquer SQL Server Express banco de dados que já existia no seu computador, mesmo que você tenha excluído o arquivo *. MDF* do banco de dados existente anteriormente. Altere o nome do arquivo *. MDF* para um nome que nunca tenha sido usado como um nome de banco de dados e altere o arquivo *Web. config* para usar o novo nome. Como alternativa, você pode usar [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) para excluir bancos de dados SQL Server Express existentes anteriormente.

## <a name="model-compatibility-cannot-be-checked"></a>Não é possível verificar a compatibilidade do modelo

### <a name="scenario"></a>Cenário

Você atualizou a cadeia de conexão do arquivo *Web. config* para apontar para um novo banco de dados SQL Server Express e, na primeira vez em que executar o aplicativo, verá a seguinte mensagem de erro:

Não é possível verificar a compatibilidade do modelo porque o banco de dados não contém metadados do modelo. Verifique se IncludeMetadataConvention foi adicionado às convenções DbModelBuilder.

### <a name="possible-cause-and-solution"></a>Possível causa e solução

Se o nome do banco de dados que você colocou no arquivo Web. config já foi usado antes em seu computador, um banco de dados poderá existir com algumas tabelas. Selecione um novo nome que não tenha sido usado no computador antes e altere o arquivo *Web. config* para apontar para usar esse novo nome de banco de dados. Como alternativa, você pode usar [SQL Server Express Utility](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990) ou [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) para excluir o banco de dados existente.

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a>Erro de SQL quando um script tenta criar usuários ou funções

### <a name="scenario"></a>Cenário

Você está usando a implantação de banco de dados configurada na guia **pacote/publicar SQL** , os scripts SQL executados durante a implantação incluem comandos Create User ou CREATE ROLE, e a execução do script falha quando esses comandos são executados. Você pode ver mensagens mais detalhadas, como as seguintes:

[!code-console[Main](troubleshooting/samples/sample8.cmd)]

Se esse erro ocorrer quando você tiver configurado a implantação de banco de dados no assistente **publicar Web** em vez da guia **pacote/publicar SQL** , crie um thread no fórum de [configuração e implantação](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) e a solução será adicionada a essa página de solução de problemas.

### <a name="possible-cause-and-solution"></a>Possível causa e solução

A conta de usuário que você está usando para executar a implantação não tem permissão para criar usuários ou funções. Por exemplo, a empresa de hospedagem pode atribuir o banco de\_DataReader, o DB\_DataWriter e o DB\_funções de ddladmin à conta de usuário que ele configura para você. Elas são suficientes para criar a maioria dos objetos de banco de dados, mas não para a criação de usuários ou funções. Uma maneira de evitar o erro é excluindo usuários e funções da implantação de banco de dados. Você pode fazer isso editando o elemento de origem para o script gerado automaticamente do banco de dados para que ele inclua os seguintes atributos:

[!code-console[Main](troubleshooting/samples/sample9.cmd)]

Para obter informações sobre como editar o elemento de origem no arquivo de projeto, consulte [como: Editar configurações de implantação no arquivo de projeto](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx). Se os usuários ou funções no banco de dados de desenvolvimento precisarem estar no banco de dados de destino, entre em contato com seu provedor de hospedagem para obter assistência.

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a>Erro de SQL Server tempo limite ao executar scripts personalizados durante a implantação

### <a name="scenario"></a>Cenário

Você especificou scripts SQL personalizados para execução durante a implantação e, quando Implantação da Web executá-los, eles atingirão o tempo limite.

### <a name="possible-cause-and-solution"></a>Possível causa e solução

A execução de vários scripts que têm modos de transação diferentes pode causar erros de tempo limite. Por padrão, os scripts gerados automaticamente são executados em uma transação, mas os scripts personalizados não. Se você selecionar a opção efetuar **pull de dados e/ou esquema de uma existente** na guia **pacote/publicar SQL** e, se adicionar um script SQL personalizado, deverá alterar as configurações de transação em alguns scripts para que todos os scripts usem as mesmas configurações de transação. Para obter mais informações, consulte [como: implantar um banco de dados com um projeto de aplicativo Web](https://msdn.microsoft.com/library/dd465343.aspx).

Se você tiver configurado as configurações de transação para que todas sejam as mesmas, mas ainda recebam esse erro, uma possível solução alternativa é executar os scripts separadamente. Na grade **scripts de banco de dados** na guia **empacotar/publicar** SQL, desmarque a caixa de seleção **incluir** para o script que causa o erro de tempo limite e, em seguida, publique o projeto. Em seguida, volte para a grade **scripts de banco de dados** , marque a caixa de seleção **inclusão** do script e desmarque as caixas de seleção **incluir** para os outros scripts. Em seguida, publique o projeto novamente. Desta vez, quando você publicar, somente o script personalizado selecionado será executado.

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a>O fluxo de dados do manifesto do site ainda não está disponível

### <a name="scenario"></a>Cenário

Ao instalar um pacote usando o arquivo *Deploy. cmd* com a opção t (Test), você verá a seguinte mensagem de erro:

Erro: os dados de fluxo de ' sitemanifest/dbFullSql [@path= ' C:\TEMP\AdventureWorksGrant.sql ']/sqlScript ' ainda não estão disponíveis.

### <a name="possible-cause-and-solution"></a>Possível causa e solução

A mensagem de erro significa que o comando não pode produzir um relatório de teste. No entanto, o comando poderá ser executado se você usar a opção y (instalação real). A mensagem indica apenas que há um problema com a execução do comando no modo de teste.

## <a name="this-application-requires-managedruntimeversion-v40"></a>Este aplicativo requer o ManagedRuntimeVersion v 4.0

### <a name="scenario"></a>Cenário

Ao tentar implantar o, você verá a seguinte mensagem de erro:

O pool de aplicativos que você está tentando usar tem a propriedade ' managedRuntimeVersion ' definida como ' v 2.0 '. Este aplicativo requer ' v 4.0 '.

### <a name="possible-cause-and-solution"></a>Possível causa e solução

O ASP.NET 4 não está instalado no IIS. Se o servidor no qual você está implantando for o seu computador de desenvolvimento e tiver o Visual Studio 2010 instalado, o ASP.NET 4 será instalado no computador, mas poderá não estar instalado no IIS. No servidor em que você está implantando, abra um prompt de comando com privilégios elevados e instale o ASP.NET 4 no IIS executando os seguintes comandos:

[!code-console[Main](troubleshooting/samples/sample10.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a>Não é possível converter Microsoft. Web. Deployment. DeploymentProviderOptions

### <a name="scenario"></a>Cenário

Ao implantar um pacote, você verá a seguinte mensagem de erro:

Não é possível converter o objeto do tipo ' Microsoft. Web. Deployment. DeploymentProviderOptions ' em ' Microsoft. Web. Deployment. DeploymentProviderOptions '.

### <a name="possible-cause-and-solution"></a>Possível causa e solução

Você está tentando implantar do Gerenciador do IIS usando a interface do usuário do Implantação da Web 1,1 em um servidor que tenha o Implantação da Web 2,0 instalado. Se você estiver usando a ferramenta de administração remota do IIS para implantar importando um pacote, marque a caixa de diálogo **novos recursos disponíveis** ao estabelecer a conexão. (Essa caixa de diálogo pode ser mostrada apenas uma vez quando a conexão é estabelecida. Para limpar a conexão e recomeçar, feche o Gerenciador do IIS e inicie-o novamente digitando inetmgr/Reset reiniciar no prompt de comando.) Se um dos recursos listados for **implantação da Web interface do usuário**e tiver um número de versão inferior a 8, o servidor que você está implantando poderá ter as versões 1,1 e 2,0 do implantação da Web instalado. Para implantar de um cliente que tenha o 2,0 instalado, o servidor deve ter apenas Implantação da Web 2,0 instalado. Você precisará entrar em contato com seu provedor de hospedagem para resolver esse problema.

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a>Não é possível carregar os componentes nativos do SQL Server Compact

### <a name="scenario"></a>Cenário

Ao executar o site implantado, você verá a seguinte mensagem de erro:

Não é possível carregar os componentes nativos do SQL Server Compact correspondente ao provedor ADO.NET da versão 8482. Instale a versão correta do SQL Server Compact. Consulte o artigo 974247 da base de conhecimento para obter mais detalhes.

### <a name="possible-cause-and-solution"></a>Possível causa e solução

O site implantado não tem as subpastas *AMD64* e *x86* com os assemblies nativos nelas na pasta *bin* do aplicativo. Em um computador com SQL Server Compact instalado, os assemblies nativos estão localizados em *c:\Arquivos de programas\microsoft SQL Server Compact Edition\v4.0\Private*. A melhor maneira de obter os arquivos corretos nas pastas corretas em um projeto do Visual Studio é instalar o pacote NuGet SqlServerCompact. A instalação do pacote adiciona um script de pós-Build para copiar os assemblies nativos em *AMD64* e *x86*. No entanto, para que eles sejam implantados, você precisa incluí-los manualmente no projeto. Para obter mais informações, consulte o tutorial [Implantando SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) .

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a>Erro "o caminho não é válido" após implantar um aplicativo de Code First de Entity Framework

### <a name="scenario"></a>Cenário

Você implanta um aplicativo que usa Migrações do Entity Framework Code First e um DBMS, como SQL Server Compact que armazena seu banco de dados em um arquivo na pasta do aplicativo\_Data. Você tem Migrações do Code First configurado para criar o banco de dados após sua primeira implantação. Ao executar o aplicativo, você receberá uma mensagem de erro semelhante ao exemplo a seguir:

O caminho não é válido. Verifique o diretório do banco de dados. [Caminho = c:\inetpub\wwwroot\App\_Data\DatabaseName.sdf]

### <a name="possible-cause-and-solution"></a>Possível causa e solução

Code First está tentando criar o banco de dados, mas a pasta do aplicativo\_data não existe. Você não tinha nenhum arquivo na pasta de *dados do\_de aplicativos* quando implantou ou selecionou **excluir aplicativo\_dados** na guia **pacote/publicar Web** do projeto janela Propriedades. O processo de implantação não criará uma pasta no servidor se não houver nenhum arquivo na pasta a ser copiada para o servidor. Se você já tiver o banco de dados configurado no site, o processo de implantação excluirá os arquivos e o *aplicativo\_* pasta de dados se você tiver selecionado **remover arquivos adicionais no destino** no perfil de publicação. Para resolver o problema, coloque um arquivo de espaço reservado como um arquivo. txt na pasta de *dados do\_de aplicativos* , verifique se você não tem excluir o **aplicativo\_dados** selecionados e reimplante-os.

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a>"Objeto COM que foi separado de seu RCW subjacente não pode ser usado."

### <a name="scenario"></a>Cenário

Você foi usado com êxito a publicação com um clique para implantar seu aplicativo e, em seguida, você começa a obter esse erro:

Falha na tarefa de implantação da Web. (Não foi possível concluir a solicitação para a URL remota do agente '<https://serverurl.com/msdeploy.axd?site=sitename>'.)  
 Não foi possível concluir a solicitação para a URL remota do agente '<https://url/msdeploy.axd?site=sitename>'.  
A solicitação foi anulada: a solicitação foi cancelada.  
O objeto COM que foi separado de seu RCW subjacente não pode ser usado.

### <a name="possible-cause-and-solution"></a>Possível causa e solução

O fechamento e a reinicialização do Visual Studio normalmente são tudo o que é necessário para resolver esse erro.

## <a name="deployment-fails-because-user-credentials-used-for-publishing-dont-have-setacl-authority"></a>A implantação falha porque as credenciais do usuário usadas para publicação não têm autoridade setACL

### <a name="scenario"></a>Cenário

A publicação falha com um erro que indica que você não tem autoridade para definir permissões de pasta (a conta de usuário que você está usando não tem autoridade setACL).

### <a name="possible-cause-and-solution"></a>Possível causa e solução

Por padrão, o Visual Studio define permissões de leitura na pasta raiz do site e permissões de gravação na pasta de dados do\_de aplicativos. Se você souber que as permissões padrão nas pastas do site estão corretas e não precisam ser definidas, você desabilitará esse comportamento adicionando **&lt;destino IncludeSetACLProviderOn&gt;False&lt;o/IncludeSetACLProviderOnDestination&gt;** ao arquivo de perfil de publicação (para afetar um único perfil) ou ao arquivo WPP. targets (para afetar todos os perfis). Para obter informações sobre como editar esses arquivos, consulte [como: Editar configurações de implantação em arquivos de perfil (. pubxml)](https://msdn.microsoft.com/library/ff398069.aspx).

## <a name="access-denied-errors-when-the-application-tries-to-write-to-an-application-folder"></a>Erros de acesso negado quando o aplicativo tenta gravar em uma pasta de aplicativo

### <a name="scenario"></a>Cenário

Os erros do aplicativo ao tentar criar ou editar um arquivo em uma das pastas do aplicativo, pois ele não tem autoridade de gravação para essa pasta.

### <a name="possible-cause-and-solution"></a>Possível causa e solução

Por padrão, o Visual Studio define permissões de leitura na pasta raiz do site e permissões de gravação na pasta de dados do\_de aplicativos. Se o seu aplicativo precisar de acesso de gravação a uma subpasta, você poderá definir permissões para essa pasta, conforme mostrado nos tutoriais de configuração permissões de pasta e implantação para o ambiente de produção, nesta série. Se seu aplicativo precisar de acesso de gravação à pasta raiz do site, você precisará impedi-lo de configurar o acesso somente leitura na pasta raiz adicionando **&lt;destino IncludeSetACLProviderOn&gt;False&lt;o&gt;** para o arquivo de perfil de publicação (para afetar um único perfil) ou para o arquivo WPP. targets (para afetar todos os perfis). Para obter informações sobre como editar esses arquivos, consulte [como: Editar configurações de implantação em arquivos de perfil (. pubxml)](https://msdn.microsoft.com/library/ff398069.aspx).

<a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a>Erro de configuração-o atributo targetFramework faz referência a uma versão posterior à versão instalada do .NET Framework

### <a name="scenario"></a>Cenário

Você publicou com êxito um projeto Web que tem como alvo o ASP.NET 4,5, mas quando você executa o aplicativo (com o modo customErrors definido como "off" no arquivo Web. config), obtém o seguinte erro:

O atributo ' targetFramework ' no elemento de&gt; de compilação &lt;do arquivo Web. config é usado somente para a versão de destino 4,0 e posteriores do .NET Framework (por exemplo, '&lt;Compilation targetFramework = "4.0"&gt;'). O atributo ' targetFramework ' atualmente faz referência a uma versão posterior à versão instalada do .NET Framework. Especifique uma versão de destino válida do .NET Framework ou instale a versão necessária do .NET Framework.

A caixa erro de origem da página de erro realça a seguinte linha de Web. config como a causa do erro:

&lt;compilação targetFramework = "4.5"/&gt;

### <a name="possible-cause-and-solution"></a>Possível causa e solução

O servidor não dá suporte a ASP.NET 4,5. Entre em contato com o provedor de hospedagem para determinar quando e se o suporte para ASP.NET 4,5 pode ser adicionado. Se a atualização do servidor não for uma opção, você precisará implantar um projeto Web que tenha como destino o ASP.NET 4 ou anterior.

Se você implantar um projeto Web ASP.NET 4 ou anterior no mesmo destino, marque a caixa de seleção **remover arquivos adicionais no destino** na guia **configurações** do assistente **publicar Web** . Se você não selecionar **remover arquivos adicionais no destino**, você continuará a obter a página de erro de configuração.

As janelas **Propriedades** do projeto incluem uma lista suspensa estrutura de destino, mas você não pode resolver esse problema apenas alterando isso de **.NET Framework 4,5** para **.NET Framework 4**. Se você alterar a estrutura de destino para uma versão anterior do Framework, o projeto ainda terá referências aos assemblies da versão do Framework posterior e não será executado. Você precisa alterar manualmente essas referências ou criar um novo projeto que tenha como destino .NET Framework 4 ou anterior. Para obter mais informações, consulte [.NET Framework direcionamento para sites](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx).

## <a name="medium-trust-errors"></a>Erros de confiança média

### <a name="scenario"></a>Cenário

Quando você executa o aplicativo em produção, ele obtém um erro relacionado à confiança média.

### <a name="possible-cause-and-solution"></a>Possível causa e solução

Muitos provedores de Hospedagem de terceiros executam seu site em confiança média, o que significa que há algumas coisas que não tem permissão para fazer. Por exemplo, o código do aplicativo não pode acessar o registro do Windows e não pode ler ou gravar arquivos que estão fora da hierarquia de pastas do seu aplicativo. Por padrão, o aplicativo é executado com *confiança total* no computador local, o que significa que o aplicativo pode ser capaz de fazer coisas que falhariam ao implantá-lo na produção.

Você pode configurar o aplicativo para ser executado em confiança média no ambiente local do IIS para solucionar problemas. Para fazer isso, abra o arquivo *Web. config* do aplicativo e adicione um elemento **Trust** no elemento **System. Web** , conforme mostrado neste exemplo.

[!code-xml[Main](troubleshooting/samples/sample11.xml)]

O aplicativo agora será executado em confiança média no IIS, mesmo no computador local.

Não faça isso se estiver implantando em Azure App serviço, porque o Azure não requer confiança média. No momento em que este tutorial está sendo escrito em fevereiro de 2012, usar esse método para fazer com que seu aplicativo seja executado em confiança média causará um erro no Azure.

Se você estiver usando Migrações do Entity Framework Code First e estiver implantando em um provedor de hospedagem que executa seu aplicativo em confiança média, verifique se você tem a versão 5,0 ou posterior instalada. No Entity Framework versão 4,3, as migrações exigem confiança total para atualizar o esquema de banco de dados.

## <a name="http-40417-not-found-error"></a>Erro HTTP 404,17 não encontrado

### <a name="scenario"></a>Cenário

Ao executar o site implantado em seu computador de desenvolvimento no IIS, você verá a seguinte mensagem de erro relatando que o servidor não pode processar default. aspx:

Erro HTTP 404,17-não encontrado

O conteúdo solicitado parece ser um script e não será servido pelo manipulador de arquivo estático.

### <a name="possible-cause-and-solution"></a>Possível causa e solução

O ASP.NET 4,5 pode não estar instalado no seu computador. Consulte as etapas no tutorial implantando no IIS como um ambiente de teste nesta série que explicam como instalar o ASP.NET 4,5.

> [!div class="step-by-step"]
> [Anterior](deploying-extra-files.md)
