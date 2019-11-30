---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-vb
title: Configurando o aplicativo Web de produção para usar o banco de dados de produção (VB) | Microsoft Docs
author: rick-anderson
description: Conforme discutido nos tutoriais anteriores, não é incomum que as informações de configuração sejam diferentes entre os ambientes de desenvolvimento e de produção. Isso é es...
ms.author: riande
ms.date: 04/23/2009
ms.assetid: a64a7aa0-6608-449e-83bf-1ef8cceee504
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-vb
msc.type: authoredcontent
ms.openlocfilehash: 7fe4f545a76992ad687827af447d9a9e95bea73f
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74633630"
---
# <a name="configuring-the-production-web-application-to-use-the-production-database-vb"></a>Configuração do aplicativo Web de produção para usar o banco de dados de produção (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_08_VB.zip) ou [baixar PDF](https://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial08_DBConfig_vb.pdf)

> Conforme discutido nos tutoriais anteriores, não é incomum que as informações de configuração sejam diferentes entre os ambientes de desenvolvimento e de produção. Isso é especialmente verdadeiro para aplicativos Web controlados por dados, uma vez que as cadeias de conexão de Database diferem entre os ambientes de desenvolvimento e produção. Este tutorial explora maneiras de configurar o ambiente de produção para incluir a cadeia de conexão apropriada em mais detalhes.

## <a name="introduction"></a>Introdução

Os aplicativos Web controlados por dados geralmente usam um banco de dado diferente quando em desenvolvimento do que quando estiver em produção. Para aplicativos hospedados por um provedor de host da Web e desenvolvidos localmente, o banco de dados de desenvolvimento normalmente reside no computador do desenvolvedor, enquanto o banco de dados de produção é hospedado em um servidor de banco de dados na instalação da empresa de hospedagem na Web. A implantação de um aplicativo Web controlado por dados envolve copiar o banco de dado de desenvolvimento para o servidor de banco de dados de produção. No tutorial anterior, vimos maneiras de realizar essa etapa.

O aplicativo Web usa as informações em uma *cadeia de conexão* para estabelecer uma conexão com o banco de dados. A cadeia de conexão, que normalmente é armazenada em `Web.config`, especifica o nome do servidor de banco de dados, o nome do banco de dados, o contexto de segurança e outras informações. Como o banco de dados usado pelo aplicativo Web depende de se o aplicativo Web está sendo executado nos ambientes de desenvolvimento ou de produção, as cadeias de conexão devem ser diferentes entre os dois ambientes.

Não é incomum que as informações de configuração sejam diferentes entre os ambientes de desenvolvimento e de produção. As *diferenças de configuração comuns entre o tutorial de desenvolvimento e produção* discutiram as técnicas para manter informações de configuração separadas entre esses dois ambientes, bem como uma breve discussão sobre cadeias de conexão de banco de dados. Este tutorial explora maneiras de configurar o ambiente de produção para incluir a cadeia de conexão apropriada em mais detalhes.

## <a name="examining-the-connection-string-information"></a>Examinando as informações da cadeia de conexão

A cadeia de conexão usada pelo aplicativo Web de revisões de livros é armazenada no arquivo de configuração do aplicativo, `Web.config`. `Web.config` inclui uma seção especial para armazenar cadeias de conexão, adequadamente denominada [&lt;connectionstrings&gt;](https://msdn.microsoft.com/library/bf7sd233.aspx). O arquivo de `Web.config` para o site de revisões de livros tem uma cadeia de conexão definida nesta seção chamada `ReviewsConnectionString`:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-vb/samples/sample1.xml)]

A cadeia de conexão-fonte de dados = .\SQLEXPRESS; AttachDbFilename = | DataDirectory | \Reviews.MDF; segurança integrada = true; User Instance = true – é composto por um número de opções e valores, com pares de opção/valor delimitados por um ponto e vírgula e cada opção e valor delimitado por um sinal de igual. As quatro opções usadas nesta cadeia de conexão são:

- `Data Source`-especifica o local do servidor de banco de dados e o nome da instância do servidor de banco de dados (se houver). O valor, `.\SQLEXPRESS`, é um exemplo em que há um servidor de banco de dados e um nome de instância. O período especifica que o servidor de banco de dados está no mesmo computador que o aplicativo; o nome da instância é `SQLEXPRESS`.
- `AttachDbFilename`-especifica o local do arquivo de banco de dados. O valor contém o espaço reservado `|DataDirectory|`, que é resolvido para o caminho completo da pasta Application s `App_Data` em tempo de execução.
- `Integrated Security`-um valor booliano que indica se deve ser usado um nome de usuário/senha especificado ao se conectar ao banco de dados (false) ou às credenciais da conta atual do Windows (true).
- `User Instance`-uma opção de configuração específica para as edições de SQL Server Express que indica se deve permitir que usuários não administrativos no computador local anexem e se conectem a um banco de dados do SQL Server Express Edition. Consulte [SQL Server Express instâncias de usuário](https://msdn.microsoft.com/library/ms254504.aspx) para obter mais informações sobre essa configuração.

As opções de cadeia de conexão permitidas dependem do banco de dados ao qual você está se conectando e do provedor de banco de dados [ADO.net](http://ADO.NET) que está sendo usado. Por exemplo, a cadeia de conexão para se conectar a um banco de dados Microsoft SQL Server difere da usada para se conectar a um banco de dados Oracle. Da mesma forma, conectar-se a um banco de dados Microsoft SQL Server usando o provedor SqlClient usa uma cadeia de conexão diferente de quando usar o provedor OLE-DB.

Você pode criar a cadeia de conexão do banco de dados manualmente usando um site como [connectionStrings.com](http://www.connectionstrings.com/) como um recurso para saber quais opções estão disponíveis. No entanto, uma abordagem mais fácil é adicionar o banco de dados ao Gerenciador de Servidores no Visual Studio e, em seguida, obter a cadeia de conexão do janela Propriedades. Vamos usar essa última técnica para construir a cadeia de conexão para o servidor de banco de dados de produção.

Abra o Visual Studio e, em seguida, navegue até a janela de Gerenciador de Servidores (no Visual Web Developer, essa janela é chamada de Gerenciador de Banco de Dados). Clique com o botão direito do mouse na opção conexões de dados e escolha a opção Adicionar conexão no menu de contexto. Isso abre o assistente mostrado na Figura 1. Escolha a fonte de dados apropriada e clique em continuar.

[![optar por adicionar um novo banco de dados ao Gerenciador de Servidores](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image2.jpg)](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image1.jpg) 

**Figura 1**: escolha Adicionar um novo banco de dados ao Gerenciador de servidores ([clique para exibir a imagem em tamanho normal](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image3.jpg))

Em seguida, especifique as várias informações de conexão de banco de dados (consulte a Figura 2). Quando você se inscreveu com sua empresa de hospedagem na Web, ela deve ter fornecido informações sobre como se conectar ao banco de dados-o nome do servidor de banco de dados, o nome do banco de dados, o nome de usuário e a senha a serem usados para se conectar ao banco de dados e assim por diante. Depois de inserir essas informações, clique em OK para concluir este assistente e adicionar o banco de dados ao Gerenciador de Servidores.

[![especificar as informações de conexão do banco de dados](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image5.jpg)](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image4.jpg) 

**Figura 2**: especificar as informações de conexão do banco de dados ([clique para exibir a imagem em tamanho normal](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image6.jpg))

O banco de dados do ambiente de produção agora deve estar listado na Gerenciador de Servidores. Selecione o banco de dados do Gerenciador de Servidores e vá para a janela Propriedades. Lá, você encontrará uma propriedade chamada cadeia de conexão com a cadeia de conexão do banco de dados. Supondo que você esteja usando um banco de dados Microsoft SQL Server em produção e o provedor SqlClient, sua cadeia de conexão deve ser semelhante ao seguinte:

<strong>Fonte de dados =<em>ServerName</em>; Catálogo inicial =<em>DatabaseName</em>; Informações de persistência de segurança = verdadeiro; ID de usuário =<em>username</em>; Senha =*senha</strong>*

Em que *ServerName*, *DatabaseName*, *username*e *password* estão com os valores para o nome do servidor de banco de dados, o nome do banco de dados e o nome de usuário e a senha fornecidos por sua empresa de host Web.

## <a name="deploying-the-book-reviews-web-application"></a>Implantando o aplicativo Web de análises de livros

O tutorial anterior apresentou a cópia do banco de dados de desenvolvimento para o ambiente de produção, mas não explorou a implantação do aplicativo controlado por dados. Neste ponto, o ambiente de produção contém o banco de dados, mas está usando a versão do aplicativo análises de livros com revisões estáticas. Precisamos implantar o novo aplicativo controlado por dados no servidor de produção, juntamente com as informações de configuração atualizadas.

Reserve um tempo para implantar o aplicativo controlado por dados do ambiente de desenvolvimento para produção. Esse processo foi abordado em detalhes nos tutoriais anteriores. Se você precisar de um atualizador, consulte o *implantando seu site usando um cliente FTP* ou *implantando seu site usando* os tutoriais do Visual Studio. Você precisará garantir que a cadeia de conexão do banco de dados de produção seja aquela usada no ambiente de produção, o que significa que um arquivo de `Web.config` alternativo deve ser implantado. Especificamente, isso modificou `Web.config` elemento `<connectionStrings>` do arquivo precisa conter a cadeia de conexão do banco de dados de produção e deve ser semelhante ao seguinte:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-vb/samples/sample2.xml)]

Observe que a cadeia de conexão no elemento `<connectionStrings>` é nomeada a mesma (`ReviewsConnectionString`), mas agora contém a cadeia de conexão do banco de dados de produção em vez da cadeia de conexão do banco de dados de desenvolvimento.

A menos que você tenha um fluxo de trabalho de implantação mais formalizado, modifique manualmente o arquivo de `Web.config` para usar a cadeia de conexão do banco de dados de produção antes de implantar (Lembre-se de revertê-lo para usar a cadeia de conexão do banco de dados de desenvolvimento posteriormente) ou manter um arquivo `Web.config` separado com as informações de configuração do ambiente de produção que são carregadas no ambiente

> [!NOTE]
> Se você implantar acidentalmente um arquivo de `Web.config` que contém a cadeia de conexão do banco de dados de desenvolvimento, haverá um erro quando o aplicativo na produção tentar se conectar ao banco de dados. Esse erro é manifestado como um `SqlException` com uma mensagem relatando que o servidor não foi encontrado ou não estava acessível.

Depois que o site tiver sido implantado para produção, visite o site de produção por meio do navegador. Você deve ver e aproveitar a mesma experiência do usuário que ao executar o aplicativo controlado por dados localmente. É claro que, quando você visita o site na produção, o site é alimentado pelo servidor de banco de dados de produção, enquanto que visitar o site no ambiente de desenvolvimento usa o banco de dados em desenvolvimento. A Figura 3 mostra a página de revisão *ensinar ASP.NET 3,5 em 24 horas* do site no ambiente de produção (Observe a URL na barra de endereços do navegador).

[![aplicativo controlado por dados agora está disponível em produção!](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image8.jpg)](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image7.jpg) 

**Figura 3**: o aplicativo controlado por dados agora está disponível em produção! ([Clique para exibir a imagem em tamanho normal](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image9.jpg))

### <a name="storing-connection-strings-in-a-separate-configuration-file"></a>Armazenando cadeias de conexão em um arquivo de configuração separado

Uma técnica comum para manter informações de configuração separadas sobre os ambientes de desenvolvimento e produção é ter duas versões do `Web.config`: uma para o ambiente de desenvolvimento e outra para produção. Em tempo de implantação, a versão apropriada do `Web.config` pode ser copiada para o ambiente de produção. O ideal é que esse processo seja automatizado como parte do fluxo de trabalho de implantação.

Em vez de manter dois arquivos de `Web.config` separados, você pode, opcionalmente, fornecer diferenças mais granulares. Os elementos que compõem o arquivo de `Web.config` podem ser definidos em arquivos de configuração externos que são referenciados no arquivo `Web.config`. Em resumo, você pode ter um arquivo de `Web.config` para ambos os ambientes que fazem referência a um arquivo databaseConnectionStrings. config, que conteria as cadeias de conexão usadas pelo aplicativo e seria exclusivo para cada ambiente. Acho que separar as informações de configuração diferentes em arquivos separados fornece um arquivo mais organizada e mais simples `Web.config` e descreve mais claramente as diferenças de configuração entre os ambientes de desenvolvimento e produção.

Para usar essa técnica, comece criando uma nova pasta no aplicativo Web chamada `ConfigSections`. Em seguida, adicione dois arquivos a essa nova pasta chamada databaseConnectionStrings. dev. config e databaseConnectionStrings. Production. config. Em seguida, copie o elemento `<connectionStrings>` de `Web.config` nos arquivos databaseConnectionStrings. dev. config e databaseConnectionStrings. Production. config e, em seguida, modifique a cadeia de conexão no arquivo databaseConnectionStrings. Production. config para que ele especifique a cadeia de conexão do banco de dados de produção. Por exemplo, o arquivo databaseConnectionStrings. dev. config deve conter apenas o elemento `<connectionStrings>` com uma cadeia de conexão que faz referência ao banco de dados de desenvolvimento:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-vb/samples/sample3.xml)]

Da mesma forma, o arquivo databaseConnectionStrings. Production. config deve conter apenas um elemento `<connectionStrings>`, mas um que tenha a cadeia de conexão do banco de dados de produção.

Faça uma cópia do arquivo databaseConnectionStrings. dev. config e nomeie-o databaseConnectionStrings. config.

> [!NOTE]
> Você pode nomear o arquivo de configuração algo diferente de databaseConnectionStrings. config, se você d como `connectionStrings.config` ou `dbInfo.config`. No entanto, não se esqueça de nomear o arquivo com uma extensão de `.config`, pois os arquivos de `.config` são, por padrão, não atendidos pelo mecanismo ASP.NET. Se você nomear o arquivo outra coisa, como `connectionStrings.txt`, um usuário poderá apontar seu navegador para [www.yoursite.com/ConfigSettings/connectionStrings.txt](http://www.yoursite.com/ConfigSettings/connectionStrings.txt) e exibir o conteúdo do arquivo!

Neste ponto, a pasta `ConfigSections` deve conter três arquivos (consulte a Figura 4). Os arquivos databaseConnectionStrings. dev. config e databaseConnectionStrings. Production. config contêm as cadeias de conexão para os ambientes de desenvolvimento e produção, respectivamente. O arquivo databaseConnectionStrings. config contém as informações da cadeia de conexão que serão usadas pelo aplicativo Web em tempo de execução. Consequentemente, o arquivo databaseConnectionStrings. config deve ser idêntico ao arquivo databaseConnectionStrings. dev. config no ambiente de desenvolvimento, enquanto em produção o arquivo databaseConnectionStrings. config deve ser idêntico a databaseConnectionStrings. Production. config.

[![ConfigSections](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image11.jpg)](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image10.jpg) 

**Figura 4**: configSections ([clique para exibir a imagem em tamanho normal](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image12.jpg))

Agora, precisamos instruir `Web.config` a usar o arquivo databaseConnectionStrings. config para seu armazenamento de cadeia de conexão. Abra `Web.config` e substitua o elemento `<connectionStrings>` existente pelo seguinte:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-vb/samples/sample4.xml)]

O atributo `configSource` especifica um caminho físico relativo ao arquivo de `Web.config`. Se o arquivo de `.config` externo estiver no mesmo diretório que `Web.config` em seguida, defina esse atributo como o nome de arquivo do arquivo de `.config`. Se ele estiver em um subdiretório, como é o caso com databaseConnectionStrings. config, especifique a subpasta usando uma barra invertida para delimitar os nomes de pasta e arquivo, como ConfigSections\databaseConnectionStrings.config.

Com essa modificação, os ambientes de desenvolvimento e produção contêm o mesmo arquivo de `Web.config`. Agora, a única diferença é o arquivo databaseConnectionStrings. config. Copie o arquivo databaseConnectionStrings. Production. config para produção e renomeie-o como databaseConnectionStrings. config. Se, no futuro, houver alterações na cadeia de conexão do banco de dados de produção, você precisará torná-las no arquivo databaseConnectionStrings. Production. config e, em seguida, carregar esse arquivo para produção, renomeando-o databaseConnectionStrings. config.

> [!NOTE]
> Você pode especificar as informações para qualquer elemento `Web.config` em um arquivo separado e usar o atributo `configSource` para fazer referência a esse arquivo de dentro de `Web.config`.

## <a name="summary"></a>Resumo

Normalmente, os aplicativos controlados por dados usam bancos de dado diferentes nos ambientes de desenvolvimento e produção. Consequentemente, as cadeias de conexão de banco de dados armazenadas na configuração do aplicativo Web s devem ser exclusivas por ambiente. Neste tutorial, vimos como determinar a cadeia de conexão do banco de dados de produção e maneiras de manter informações de cadeias de conexão exclusivas nos dois ambientes.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Cadeias de conexão e arquivos de configuração](https://msdn.microsoft.com/library/ms254494.aspx)
- [Informações de cadeias de configuração de banco de dados @ ConnectionStrings.com](http://www.connectionstrings.com/)
- [Mover configurações para fora do arquivo Web. config](http://www.asp101.com/tips/index.asp?id=154)
- [Documentação técnica para o elemento &lt;connectionStrings&gt;](https://msdn.microsoft.com/library/bf7sd233.aspx)

> [!div class="step-by-step"]
> [Anterior](deploying-a-database-vb.md)
> [Próximo](configuring-a-website-that-uses-application-services-vb.md)
