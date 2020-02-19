---
uid: identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
title: Implantando senhas e outros dados confidenciais em ASP.NET e Azure App Service-ASP.NET 4. x
author: Rick-Anderson
description: Este tutorial mostra como seu código pode armazenar e acessar com segurança informações seguras. O ponto mais importante é que você nunca deve armazenar senhas ou outra FRA...
ms.author: riande
ms.date: 05/21/2015
ms.assetid: 97902c66-cb61-4d11-be52-73f962f2db0a
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
msc.type: authoredcontent
ms.openlocfilehash: 8356a90611f791779cc4ff4730038d82cd76242f
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457044"
---
# <a name="best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure-app-service"></a>Melhores práticas para implantar senhas e outros dados confidenciais no ASP.NET e no Serviço de Aplicativo do Azure

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> Este tutorial mostra como seu código pode armazenar e acessar com segurança informações seguras. O ponto mais importante é que você nunca deve armazenar senhas ou outros dados confidenciais no código-fonte, e não deve usar os segredos de produção no modo de desenvolvimento e teste.
> 
> O código de exemplo é um aplicativo de console de trabalho Web simples e um aplicativo MVC ASP.NET que precisa acessar uma senha de cadeia de conexão de banco de dados, twilio, Google e SendGrid proteger chaves.
> 
> As configurações locais e o PHP também são mencionados.

- [Trabalhando com senhas no ambiente de desenvolvimento](#pwd)
- [Trabalhando com cadeias de conexão no ambiente de desenvolvimento](#con)
- [Aplicativos de console de trabalhos Web](#wj)
- [Implantando segredos no Azure](#da)
- [Observações para o local e PHP](#not)
- [Recursos adicionais](#addRes)

<a id="pwd"></a>
## <a name="working-with-passwords-in-the-development-environment"></a>Trabalhando com senhas no ambiente de desenvolvimento

Os tutoriais frequentemente mostram dados confidenciais no código-fonte, espero que você nunca armazene dados confidenciais no código-fonte. Por exemplo, meu [aplicativo ASP.NET MVC 5 com SMS e](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md) o tutorial de 2FA de email mostra o seguinte no arquivo *Web. config* :

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample1.xml)]

O arquivo *Web. config* é o código-fonte, portanto, esses segredos nunca devem ser armazenados nesse arquivo. Felizmente, o elemento `<appSettings>` tem um atributo `file` que permite especificar um arquivo externo que contém configurações de aplicativo confidenciais. Você pode mover todos os seus segredos para um arquivo externo, desde que o arquivo externo não seja verificado na árvore de origem. Por exemplo, na marcação a seguir, o arquivo *AppSettingsSecrets. config* contém todos os segredos do aplicativo:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample2.xml)]

A marcação no arquivo externo (*AppSettingsSecrets. config* neste exemplo) é a mesma marcação encontrada no arquivo *Web. config* :

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample3.xml)]

O tempo de execução do ASP.NET mescla o conteúdo do arquivo externo com a marcação no elemento &lt;appSettings&gt;. O runtime ignorará o atributo de arquivo se o arquivo especificado não puder ser encontrado.

> [!WARNING]
> Segurança-não adicione seu arquivo *segredos. config* ao seu projeto ou verifique-o no controle do código-fonte. Por padrão, o Visual Studio define o `Build Action` como `Content`, o que significa que o arquivo é implantado. Para obter mais informações, consulte [por que nem todos os arquivos na pasta do meu projeto são implantados?](https://msdn.microsoft.com/library/ee942158(v=vs.110).aspx#can_i_exclude_specific_files_or_folders_from_deployment) Embora você possa usar qualquer extensão para o arquivo *segredos. config* , é melhor mantê-lo *. config*, pois os arquivos de configuração não são atendidos pelo IIS. Observe também que o arquivo *AppSettingsSecrets. config* tem dois níveis de diretório do arquivo *Web. config* , portanto, ele está completamente fora do diretório da solução. Ao mover o arquivo para fora do diretório da solução, &quot;git Add \*&quot; não irá adicioná-lo ao seu repositório.

<a id="con"></a>
## <a name="working-with-connection-strings-in-the-development-environment"></a>Trabalhando com cadeias de conexão no ambiente de desenvolvimento

O Visual Studio cria novos projetos ASP.NET que usam o [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx). O LocalDB foi criado especificamente para o ambiente de desenvolvimento. Ele não requer uma senha, portanto, você não precisa fazer nada para impedir que os segredos sejam verificados em seu código-fonte. Algumas equipes de desenvolvimento usam as versões completas do SQL Server (ou outros DBMS) que exigem uma senha.

Você pode usar o atributo `configSource` para substituir toda a marcação de `<connectionStrings>`. Ao contrário do atributo `<appSettings>` `file` que mescla a marcação, o atributo `configSource` substitui a marcação. A marcação a seguir mostra o atributo `configSource` no arquivo *Web. config* :

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample4.xml?highlight=1)]

> [!NOTE]
> Se você usar o atributo `configSource` conforme mostrado acima para mover as cadeias de conexão para um arquivo externo e fazer com que o Visual Studio crie um novo site, ele não conseguirá detectar se você está usando um banco de dados e não terá a opção de configurar o banco de dados quando publicar no Azure por meio do Visual Studio. Se você estiver usando o atributo `configSource`, poderá usar o PowerShell para criar e implantar seu site e banco de dados, ou pode criar o site e o banco de dados no portal antes de publicar. O script [New-AzureWebsitewithDB. ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) criará um novo site e banco de dados.

> [!WARNING]
> Segurança-ao contrário do arquivo *AppSettingsSecrets. config* , o arquivo de cadeias de conexão externa deve estar no mesmo diretório que o arquivo *Web. config* raiz, portanto, você precisará tomar precauções para garantir que não o verifique em seu repositório de origem.

> [!NOTE]
> **Aviso de segurança no arquivo de segredos:** Uma prática recomendada é não usar segredos de produção em teste e desenvolvimento. O uso de senhas de produção em teste ou desenvolvimento vaza esses segredos.

<a id="wj"></a>
## <a name="webjobs-console-apps"></a>Aplicativos de console de trabalhos Web

O arquivo *app. config* usado por um aplicativo de console não dá suporte a caminhos relativos, mas oferece suporte a caminhos absolutos. Você pode usar um caminho absoluto para mover seus segredos para fora do diretório do projeto. A marcação a seguir mostra os segredos no arquivo *C:\secrets\AppSettingsSecrets.config* e dados não confidenciais no arquivo *app. config* .

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample5.xml?highlight=2)]

<a id="da"></a>
## <a name="deploying-secrets-to-azure"></a>Implantando segredos no Azure

Quando você implanta seu aplicativo Web no Azure, o arquivo *AppSettingsSecrets. config* não será implantado (isso é o que você deseja). Você pode ir para o [portal de gerenciamento do Azure](https://azure.microsoft.com/services/management-portal/) e defini-los manualmente, para fazer isso:

1. Vá para [https://portal.azure.com](https://portal.azure.com)e entre com suas credenciais do Azure.
2. Clique em **procurar &gt; aplicativos Web**e, em seguida, clique no nome do seu aplicativo Web.
3. Clique em **todas as configurações &gt; configurações do aplicativo**.

As **configurações do aplicativo** e os valores da **cadeia de conexão** substituem as mesmas configurações no arquivo *Web. config* . Em nosso exemplo, não implantamos essas configurações no Azure, mas se essas chaves estivessem no arquivo *Web. config* , as configurações mostradas no portal teriam precedência.

Uma prática recomendada é seguir um [fluxo de trabalho DevOps](../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything.md) e usar [Azure PowerShell](https://azure.microsoft.com/documentation/articles/install-configure-powershell/) (ou outra estrutura como [chefe](http://www.opscode.com/chef/) ou [Puppet](http://puppetlabs.com/puppet/what-is-puppet)) para automatizar a configuração desses valores no Azure. O script do PowerShell a seguir usa [Export-CliXml](http://www.powershellcookbook.com/recipe/PukO/securely-store-credentials-on-disk) para exportar os segredos criptografados para o disco:

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample6.ps1)]

No script acima, ' name ' é o nome da chave secreta, como '&quot;FB\_AppSecret&quot; ou "TwitterSecret". Você pode exibir o arquivo ". Credential" criado pelo script em seu navegador. O trecho de código abaixo testa cada um dos arquivos de credencial e define os segredos para o aplicativo Web nomeado:

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample7.ps1)]

> [!WARNING]
> Segurança-não incluir senhas ou outros segredos no script do PowerShell, isso anulará a finalidade de usar um script do PowerShell para implantar dados confidenciais. O cmdlet [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) fornece um mecanismo seguro para obter uma senha. Usar um prompt de interface do usuário pode impedir o vazamento de uma senha.

### <a name="deploying-db-connection-strings"></a>Implantando cadeias de conexão do BD

As cadeias de conexão do BD são tratadas de forma semelhante às configurações do aplicativo. Se você implantar seu aplicativo Web do Visual Studio, a cadeia de conexão será configurada para você. Você pode verificar isso no Portal. A maneira recomendada para definir a cadeia de conexão é com o PowerShell. Para obter um exemplo de um script do PowerShell, o cria um site e um banco de dados e define a cadeia de conexão no site, baixe [New-AzureWebsitewithDB. ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) da [biblioteca de scripts do Azure](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure).

<a id="not"></a>
## <a name="notes-for-php"></a>Observações para PHP

Como os pares chave-valor para **as configurações do aplicativo e as** **cadeias de conexão** são armazenados em variáveis de ambiente no serviço Azure app, os desenvolvedores que usam qualquer estrutura de aplicativo Web (como o php) podem facilmente recuperar esses valores. Consulte sites do Windows Azure de Stefan Schackow [: como as cadeias de caracteres do aplicativo e cadeias de conexão funcionam na](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/) postagem do blog que mostra um trecho de código php para ler configurações do aplicativo e cadeias de conexão.

## <a name="notes-for-on-premises-servers"></a>Observações para servidores locais

Se você estiver implantando em servidores Web locais, poderá ajudar [a proteger os segredos criptografando as seções de configuração dos arquivos de configuração](https://msdn.microsoft.com/library/ff647398.aspx). Como alternativa, você pode usar a mesma abordagem recomendada para sites do Azure: manter as configurações de desenvolvimento em arquivos de configuração e usar valores de variáveis de ambiente para configurações de produção. Nesse caso, no entanto, você precisa escrever o código do aplicativo para a funcionalidade que é automática nos sites do Azure: recupere as configurações de variáveis de ambiente e use esses valores no lugar das configurações do arquivo de configuração ou use as configurações do arquivo de configuração quando variáveis de ambiente não encontradas.

<a id="addRes"></a>
## <a name="additional-resources"></a>Recursos adicionais

Para obter um exemplo de um script do PowerShell que cria um aplicativo Web + banco de dados, define a cadeia de conexão + configurações do aplicativo, baixar [New-AzureWebsitewithDB. ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) da [biblioteca de scripts do Azure](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure). 

Consulte os sites do [Windows Azure da Schackow de Stefan: como funcionam as cadeias de caracteres de aplicativo e as cadeias de conexão](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)

Agradecimentos especiais a Barry Dorrans ( [@blowdart](https://twitter.com/blowdart) ) e Carlos farre para revisão.
