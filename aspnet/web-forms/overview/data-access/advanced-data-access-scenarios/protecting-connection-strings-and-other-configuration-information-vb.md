---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-vb
title: Protegendo cadeias de conexão e outras informações de configuração (VB) | Microsoft Docs
author: rick-anderson
description: Um aplicativo ASP.NET normalmente armazena informações de configuração em um arquivo Web. config. Algumas dessas informações são confidenciais e garantem a proteção. Por def...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: cd17dbe1-c5e1-4be8-ad3d-57233d52cef1
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-vb
msc.type: authoredcontent
ms.openlocfilehash: 070e1dccb80ef9af21ea621357c5b23e2ada6f9f
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74607800"
---
# <a name="protecting-connection-strings-and-other-configuration-information-vb"></a>Proteger cadeias de conexão e outras informações de configuração (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_73_VB.zip) ou [baixar PDF](protecting-connection-strings-and-other-configuration-information-vb/_static/datatutorial73vb1.pdf)

> Um aplicativo ASP.NET normalmente armazena informações de configuração em um arquivo Web. config. Algumas dessas informações são confidenciais e garantem a proteção. Por padrão, esse arquivo não será servido a um visitante do site, mas um administrador ou um hacker poderá obter acesso ao sistema de arquivos do servidor Web e exibir o conteúdo do arquivo. Neste tutorial, aprendemos que o ASP.NET 2,0 nos permite proteger informações confidenciais criptografando seções do arquivo Web. config.

## <a name="introduction"></a>Introdução

As informações de configuração para aplicativos ASP.NET normalmente são armazenadas em um arquivo XML chamado `Web.config`. No decorrer desses tutoriais, atualizamos o `Web.config` alguns momentos. Ao criar o conjunto de dados `Northwind` tipado no [primeiro tutorial](../introduction/creating-a-data-access-layer-vb.md), por exemplo, as informações de cadeia de conexão foram adicionadas automaticamente a `Web.config` na seção `<connectionStrings>`. Posteriormente, nas [páginas mestras e no](../introduction/master-pages-and-site-navigation-vb.md) tutorial de navegação do site, nós atualizamos manualmente `Web.config`, adicionando um elemento `<pages>` indicando que todas as páginas ASP.net em nosso projeto devem usar o tema `DataWebControls`.

Como `Web.config` pode conter dados confidenciais, como cadeias de conexão, é importante que o conteúdo de `Web.config` seja mantido seguro e oculto de visualizadores não autorizados. Por padrão, qualquer solicitação HTTP para um arquivo com a extensão `.config` é manipulada pelo mecanismo ASP.NET, que retorna o *tipo de página que não é uma mensagem servida* mostrada na Figura 1. Isso significa que os visitantes não podem exibir o conteúdo do `Web.config` arquivo s simplesmente inserindo http://www.YourServer.com/Web.config na barra de endereços de seu navegador.

[![visitando Web. config por meio de um navegador retorna um tipo de página que não é uma mensagem servida](protecting-connection-strings-and-other-configuration-information-vb/_static/image2.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image1.png)

**Figura 1**: visitar `Web.config` por meio de um navegador retorna um tipo de página que não é uma mensagem servida ([clique para exibir a imagem em tamanho normal](protecting-connection-strings-and-other-configuration-information-vb/_static/image3.png))

Mas e se um invasor for capaz de encontrar alguma outra exploração que permita exibir o conteúdo do `Web.config` arquivo s? O que um invasor poderia fazer com essas informações e quais etapas podem ser executadas para proteger ainda mais as informações confidenciais dentro do `Web.config`? Felizmente, a maioria das seções no `Web.config` não contém informações confidenciais. Que danos um invasor pode perpetrar se souber o nome do tema padrão usado por suas páginas do ASP.NET?

Algumas seções de `Web.config`, no entanto, contêm informações confidenciais que podem incluir cadeias de conexão, nomes de usuário, senhas, nomes de servidor, chaves de criptografia e assim por diante. Essas informações normalmente são encontradas nas seguintes `Web.config` seções:

- `<appSettings>`
- `<connectionStrings>`
- `<identity>`
- `<sessionState>`

Neste tutorial, veremos as técnicas para proteger essas informações de configuração confidenciais. Como veremos, a versão 2,0 do .NET Framework inclui um sistema de configurações protegida que faz com que a criptografia e descriptografia programaticamente das seções de configuração selecionadas sejam uma Breeze.

> [!NOTE]
> Este tutorial termina com uma visão das recomendações da Microsoft para se conectar a um banco de dados de um aplicativo ASP.NET. Além de criptografar suas cadeias de conexão, você pode ajudar a proteger seu sistema, garantindo que você esteja se conectando ao banco de dados de maneira segura.

## <a name="step-1-exploring-aspnet-20-s-protected-configuration-options"></a>Etapa 1: explorando as opções de configuração protegidas do ASP.NET 2,0

O ASP.NET 2,0 inclui um sistema de configuração protegido para criptografar e descriptografar informações de configuração. Isso inclui métodos no .NET Framework que podem ser usados para criptografar ou descriptografar programaticamente informações de configuração. O sistema de configuração protegida usa o [modelo de provedor](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), que permite aos desenvolvedores escolher qual implementação de criptografia é usada.

O .NET Framework é fornecido com dois provedores de configuração protegidos:

- [`RSAProtectedConfigurationProvider`](https://msdn.microsoft.com/library/system.configuration.rsaprotectedconfigurationprovider.aspx) -usa o [algoritmo do RSA](http://en.wikipedia.org/wiki/Rsa) assimétrico para criptografia e descriptografia.
- [`DPAPIProtectedConfigurationProvider`](https://msdn.microsoft.com/system.configuration.dpapiprotectedconfigurationprovider.aspx) -usa a API de proteção de dados do Windows [(DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx) para criptografia e descriptografia.

Como o sistema de configuração protegida implementa o padrão de design do provedor, é possível criar seu próprio provedor de configuração protegida e conectá-lo ao seu aplicativo. Consulte [implementando um provedor de configuração protegida](https://msdn.microsoft.com/library/wfc2t3az(VS.80).aspx) para obter mais informações sobre esse processo.

Os provedores RSA e DPAPI usam chaves para suas rotinas de criptografia e descriptografia, e essas chaves podem ser armazenadas no nível do computador ou do usuário. As chaves de nível de máquina são ideais para cenários em que o aplicativo Web é executado em seu próprio servidor dedicado ou se há vários aplicativos em um servidor que precisam compartilhar informações criptografadas. As chaves de nível de usuário são uma opção mais segura em ambientes de hospedagem compartilhada em que outros aplicativos no mesmo servidor não devem ser capazes de descriptografar as seções de configuração protegidas do aplicativo.

Neste tutorial, nossos exemplos usarão o provedor DPAPI e as chaves de nível de máquina. Especificamente, examinaremos a criptografia da seção `<connectionStrings>` em `Web.config`, embora o sistema de configuração protegida possa ser usado para criptografar a maioria das `Web.config` seção. Para obter informações sobre como usar chaves no nível do usuário ou usar o provedor RSA, consulte os recursos na seção leituras adicionais no final deste tutorial.

> [!NOTE]
> Os provedores de `RSAProtectedConfigurationProvider` e `DPAPIProtectedConfigurationProvider` são registrados no arquivo `machine.config` com os nomes de provedor `RsaProtectedConfigurationProvider` e `DataProtectionConfigurationProvider`, respectivamente. Ao criptografar ou descriptografar informações de configuração, precisaremos fornecer o nome do provedor apropriado (`RsaProtectedConfigurationProvider` ou `DataProtectionConfigurationProvider`) em vez do nome do tipo real (`RSAProtectedConfigurationProvider` e `DPAPIProtectedConfigurationProvider`). Você pode encontrar o arquivo de `machine.config` na pasta `$WINDOWS$\Microsoft.NET\Framework\version\CONFIG`.

## <a name="step-2-programmatically-encrypting-and-decrypting-configuration-sections"></a>Etapa 2: criptografar e descriptografar programaticamente seções de configuração

Com algumas linhas de código, podemos criptografar ou descriptografar uma seção de configuração específica usando um provedor especificado. O código, como veremos em breve, simplesmente precisa fazer referência programaticamente à seção de configuração apropriada, chamar seu `ProtectSection` ou `UnprotectSection` método e, em seguida, chamar o método `Save` para manter as alterações. Além disso, a .NET Framework inclui um utilitário de linha de comando útil que pode criptografar e descriptografar informações de configuração. Exploraremos esse utilitário de linha de comando na etapa 3.

Para ilustrar programaticamente a proteção das informações de configuração, vamos criar uma página ASP.NET que inclui botões para criptografar e descriptografar a seção `<connectionStrings>` em `Web.config`.

Comece abrindo a página de `EncryptingConfigSections.aspx` na pasta `AdvancedDAL`. Arraste um controle TextBox da caixa de ferramentas para o designer, definindo sua propriedade `ID` como `WebConfigContents`, sua propriedade `TextMode` como `MultiLine`e suas propriedades `Width` e `Rows` como 95% e 15, respectivamente. Esse controle TextBox exibirá o conteúdo de `Web.config` nos permitindo ver rapidamente se o conteúdo está criptografado ou não. É claro que, em um aplicativo real, você nunca iria querer exibir o conteúdo de `Web.config`.

Abaixo da caixa de texto, adicione dois controles Button chamados `EncryptConnStrings` e `DecryptConnStrings`. Defina suas propriedades de texto para criptografar cadeias de conexão e descriptografar cadeias de conexão.

Neste ponto, sua tela deve ser semelhante à figura 2.

[![adicionar uma caixa de texto e dois controles de botão da Web à página](protecting-connection-strings-and-other-configuration-information-vb/_static/image5.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image4.png)

**Figura 2**: adicionar uma caixa de texto e dois controles de botão da Web à página ([clique para exibir a imagem em tamanho normal](protecting-connection-strings-and-other-configuration-information-vb/_static/image6.png))

Em seguida, precisamos escrever um código que carregue e exiba o conteúdo de `Web.config` na caixa de texto `WebConfigContents` quando a página for carregada pela primeira vez. Adicione o seguinte código à classe s code-behind da página. Esse código adiciona um método chamado `DisplayWebConfig` e o chama do manipulador de eventos `Page_Load` quando `Page.IsPostBack` é `False`:

[!code-vb[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample1.vb)]

O método `DisplayWebConfig` usa a [classe`File`](https://msdn.microsoft.com/library/system.io.file.aspx) para abrir o arquivo de `Web.config` do aplicativo, a [classe`StreamReader`](https://msdn.microsoft.com/library/system.io.streamreader.aspx) para ler seu conteúdo em uma cadeia de caracteres e a [classe`Path`](https://msdn.microsoft.com/library/system.io.path.aspx) para gerar o caminho físico para o arquivo `Web.config`. Todas essas três classes são encontradas no [namespace`System.IO`](https://msdn.microsoft.com/library/system.io.aspx). Consequentemente, você precisará adicionar uma instrução `Imports``System.IO` à parte superior da classe code-behind ou, como alternativa, prefixar esses nomes de classe com `System.IO.`

Em seguida, precisamos adicionar manipuladores de eventos para os dois controles Button `Click` eventos e adicionar o código necessário para criptografar e descriptografar a seção `<connectionStrings>` usando uma chave de nível de máquina com o provedor DPAPI. No designer, clique duas vezes em cada um dos botões para adicionar um manipulador de eventos `Click` na classe code-behind e, em seguida, adicione o seguinte código:

[!code-vb[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample2.vb)]

O código usado nos dois manipuladores de eventos é quase idêntico. Ambos começam obtendo informações sobre o arquivo s `Web.config` do aplicativo atual por meio do método [`WebConfigurationManager` Class](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.aspx) s [`OpenWebConfiguration`](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.openwebconfiguration.aspx). Esse método retorna o arquivo de configuração da Web para o caminho virtual especificado. Em seguida, a seção `Web.config` `<connectionStrings>` do arquivo é acessada por meio do [método`GetSection(sectionName)`](https://msdn.microsoft.com/library/system.configuration.configuration.getsection.aspx)da [classe`Configuration`](https://msdn.microsoft.com/library/system.configuration.configuration.aspx) , que retorna um objeto [`ConfigurationSection`](https://msdn.microsoft.com/library/system.configuration.configurationsection.aspx) .

O objeto `ConfigurationSection` inclui uma [propriedade`SectionInformation`](https://msdn.microsoft.com/library/system.configuration.configurationsection.sectioninformation.aspx) que fornece informações adicionais e funcionalidade sobre a seção de configuração. Como mostra o código acima, podemos determinar se a seção de configuração é criptografada verificando a propriedade `SectionInformation` s `IsProtected`. Além disso, a seção pode ser criptografada ou descriptografada por meio da propriedade `SectionInformation` s `ProtectSection(provider)` e `UnprotectSection` métodos.

O método `ProtectSection(provider)` aceita como entrada uma cadeia de caracteres especificando o nome do provedor de configuração protegida a ser usado ao criptografar. No botão `EncryptConnString` s manipulador de eventos, passamos DataProtectionConfigurationProvider para o método `ProtectSection(provider)` para que o provedor DPAPI seja usado. O método `UnprotectSection` pode determinar o provedor que foi usado para criptografar a seção de configuração e, portanto, não requer nenhum parâmetro de entrada.

Depois de chamar o método `ProtectSection(provider)` ou `UnprotectSection`, você deve chamar o [método`Save`](https://msdn.microsoft.com/library/system.configuration.configuration.save.aspx) do objeto `Configuration` para persistir as alterações. Depois que as informações de configuração forem criptografadas ou descriptografadas e as alterações forem salvas, chamamos `DisplayWebConfig` para carregar o conteúdo do `Web.config` atualizado no controle TextBox.

Depois de inserir o código acima, teste-o visitando a página `EncryptingConfigSections.aspx` por meio de um navegador. Inicialmente, você deve ver uma página que lista o conteúdo de `Web.config` com a seção `<connectionStrings>` exibida em texto sem formatação (consulte a Figura 3).

[![adicionar uma caixa de texto e dois controles de botão da Web à página](protecting-connection-strings-and-other-configuration-information-vb/_static/image8.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image7.png)

**Figura 3**: adicionar uma caixa de texto e dois controles de botão da Web à página ([clique para exibir a imagem em tamanho normal](protecting-connection-strings-and-other-configuration-information-vb/_static/image9.png))

Agora, clique no botão criptografar cadeias de conexão. Se a validação de solicitação estiver habilitada, a marcação enviada de volta da caixa de texto `WebConfigContents` produzirá um `HttpRequestValidationException`, que exibe a mensagem, um valor de `Request.Form` potencialmente perigoso foi detectado no cliente. A validação de solicitação, que é habilitada por padrão no ASP.NET 2,0, proíbe postbacks que incluem HTML não codificado e é projetada para ajudar a evitar ataques de injeção de script. Essa verificação pode ser desabilitada na página ou no nível do aplicativo. Para desativá-lo para essa página, defina a configuração `ValidateRequest` como `False` na diretiva `@Page`. A diretiva `@Page` é encontrada na parte superior da marcação declarativa de s da página.

[!code-aspx[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample3.aspx)]

Para obter mais informações sobre a validação de solicitação, sua finalidade, como desabilitá-la na página e no nível de aplicativo, bem como como codificar marcação HTML, consulte [solicitar validação-impedindo ataques de script](../../../../whitepapers/request-validation.md).

Depois de desabilitar a validação de solicitação para a página, tente clicar no botão criptografar cadeias de conexão novamente. No postback, o arquivo de configuração será acessado e sua seção `<connectionStrings>` criptografada usando o provedor DPAPI. Em seguida, a caixa de texto é atualizada para exibir o novo conteúdo de `Web.config`. Como mostra a Figura 4, as informações de `<connectionStrings>` agora estão criptografadas.

[![clicar no botão criptografar cadeias de conexão criptografa a seção &lt;connectionString&gt;](protecting-connection-strings-and-other-configuration-information-vb/_static/image11.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image10.png)

**Figura 4**: clicar no botão criptografar cadeias de conexão criptografa a seção `<connectionString>` ([clique para exibir a imagem em tamanho normal](protecting-connection-strings-and-other-configuration-information-vb/_static/image12.png))

A seção `<connectionStrings>` criptografada gerada no meu computador segue, embora parte do conteúdo do elemento `<CipherData>` tenha sido removida para fins de brevidade:

[!code-xml[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample4.xml)]

> [!NOTE]
> O elemento `<connectionStrings>` especifica o provedor usado para executar a criptografia (`DataProtectionConfigurationProvider`). Essas informações são usadas pelo método `UnprotectSection` quando o botão descriptografar cadeias de conexão é clicado.

Quando as informações da cadeia de conexão são acessadas de `Web.config` por código que escrevemos, de um controle SqlDataSource ou do código gerado automaticamente dos TableAdapters em nossos conjuntos de dados tipados, ele é automaticamente descriptografado. Em suma, não precisamos adicionar nenhum código ou lógica extra para descriptografar a seção `<connectionString>` criptografada. Para demonstrar isso, visite um dos tutoriais anteriores no momento, como o tutorial de exibição simples da seção de relatórios básicos (`~/BasicReporting/SimpleDisplay.aspx`). Como mostra a Figura 5, o tutorial funciona exatamente como esperamos, indicando que as informações de cadeia de conexão criptografadas estão sendo descriptografadas automaticamente pela página ASP.NET.

[![a camada de acesso a dados descriptografa automaticamente as informações da cadeia de conexão](protecting-connection-strings-and-other-configuration-information-vb/_static/image14.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image13.png)

**Figura 5**: a camada de acesso a dados descriptografa automaticamente as informações da cadeia de conexão ([clique para exibir a imagem em tamanho normal](protecting-connection-strings-and-other-configuration-information-vb/_static/image15.png))

Para reverter a seção `<connectionStrings>` de volta para sua representação de texto sem formatação, clique no botão descriptografar cadeias de conexão. No postback, você deve ver as cadeias de conexão em `Web.config` em texto sem formatação. Neste ponto, sua tela deve ter a aparência que fazia ao visitar essa página (veja na Figura 3).

## <a name="step-3-encrypting-configuration-sections-usingaspnet_regiisexe"></a>Etapa 3: Criptografando seções de configuração usando`aspnet_regiis.exe`

O .NET Framework inclui uma variedade de ferramentas de linha de comando na pasta `$WINDOWS$\Microsoft.NET\Framework\version\`. No tutorial [usando dependências de cache SQL](../caching-data/using-sql-cache-dependencies-vb.md) , por exemplo, examinamos o uso da ferramenta de linha de comando `aspnet_regsql.exe` para adicionar a infraestrutura necessária para dependências de cache do SQL. Outra ferramenta de linha de comando útil nessa pasta é a [ASP.NET`aspnet_regiis.exe`(ferramenta de registro do IIS)](https://msdn.microsoft.com/library/k6h9cz8h(VS.80).aspx). Como o nome indica, a ferramenta de registro do IIS ASP.NET é usada principalmente para registrar um aplicativo ASP.NET 2,0 com o servidor Web Microsoft s de nível profissional, o IIS. Além de seus recursos relacionados ao IIS, a ferramenta de registro do IIS ASP.NET também pode ser usada para criptografar ou descriptografar seções de configuração especificadas no `Web.config`.

A instrução a seguir mostra a sintaxe geral usada para criptografar uma seção de configuração com a ferramenta de linha de comando `aspnet_regiis.exe`:

[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample5.cmd)]

*seção* é a seção de configuração para criptografar (como connectionStrings), o *diretório de\_físico* é o caminho físico completo para o diretório raiz do aplicativo Web e o *provedor* é o nome do provedor de configuração protegida a ser usado (como DataProtectionConfigurationProvider). Como alternativa, se o aplicativo Web estiver registrado no IIS, você poderá inserir o caminho virtual em vez do caminho físico usando a seguinte sintaxe:

[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample6.cmd)]

O exemplo a seguir `aspnet_regiis.exe` criptografa a seção `<connectionStrings>` usando o provedor DPAPI com uma chave de nível de máquina:

[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample7.cmd)]

Da mesma forma, a ferramenta de linha de comando `aspnet_regiis.exe` pode ser usada para descriptografar seções de configuração. Em vez de usar a opção `-pef`, use `-pdf` (ou em vez de `-pe`, use `-pd`). Além disso, observe que o nome do provedor não é necessário ao descriptografar.

[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample8.cmd)]

> [!NOTE]
> Como estamos usando o provedor DPAPI, que usa chaves específicas para o computador, você deve executar `aspnet_regiis.exe` no mesmo computador do qual as páginas da Web estão sendo servidas. Por exemplo, se você executar esse programa de linha de comando do computador de desenvolvimento local e, em seguida, carregar o arquivo Web. config criptografado no servidor de produção, o servidor de produção não poderá descriptografar as informações da cadeia de conexão desde que ela foi criptografada usando chaves específicas para seu computador de desenvolvimento. O provedor RSA não tem essa limitação, pois é possível exportar as chaves RSA para outro computador.

## <a name="understanding-database-authentication-options"></a>Entendendo opções de autenticação de banco de dados

Antes que qualquer aplicativo possa emitir `SELECT`, `INSERT`, `UPDATE`ou `DELETE` consultas a um banco de dados Microsoft SQL Server, primeiro o banco de dados deve identificar o solicitante. Esse processo é conhecido como *autenticação* e o SQL Server fornece dois métodos de autenticação:

- **Autenticação do Windows** -o processo no qual o aplicativo está sendo executado é usado para se comunicar com o banco de dados. Ao executar um aplicativo ASP.NET por meio do Visual Studio 2005 s ASP.NET Development Server, o aplicativo ASP.NET assume a identidade do usuário conectado no momento. Para aplicativos ASP.NET no IIS (Microsoft Internet Information Server), os aplicativos ASP.NET geralmente assumem a identidade de `domainName``\MachineName` ou `domainName``\NETWORK SERVICE`, embora isso possa ser personalizado.
- **Autenticação do SQL** : os valores de ID de usuário e senha são fornecidos como credenciais para autenticação. Com a autenticação do SQL, a ID de usuário e a senha são fornecidas na cadeia de conexão.

A autenticação do Windows é preferida em relação à autenticação do SQL porque é mais segura. Com a autenticação do Windows, a cadeia de conexão é gratuita de um nome de usuário e senha e, se o servidor Web e o servidor de banco de dados residirem em dois computadores diferentes, as credenciais não serão enviadas pela rede em texto sem formatação. Com a autenticação do SQL, no entanto, as credenciais de autenticação são embutidas em código na cadeia de conexão e são transmitidas do servidor Web para o servidor de banco de dados em texto sem formatação.

Esses tutoriais usaram a autenticação do Windows. Você pode informar qual modo de autenticação está sendo usado inspecionando a cadeia de conexão. A cadeia de conexão no `Web.config` para nossos tutoriais foi:

`Data Source=.\SQLEXPRESS; AttachDbFilename=|DataDirectory|\NORTHWND.MDF; Integrated Security=True; User Instance=True`

A segurança integrada = true e a falta de um nome de usuário e senha indicam que a autenticação do Windows está sendo usada. Em algumas cadeias de conexão, o termo conexão confiável = Sim ou segurança integrada = SSPI é usado em vez de segurança integrada = true, mas todos os três indicam o uso da autenticação do Windows.

O exemplo a seguir mostra uma cadeia de conexão que usa a autenticação do SQL. Observe as credenciais inseridas na cadeia de conexão:

`Server=serverName; Database=Northwind; uid=userID; pwd=password`

Imagine que um invasor seja capaz de exibir o arquivo s `Web.config` do seu aplicativo. Se você usar a autenticação do SQL para se conectar a um banco de dados acessível pela Internet, o invasor poderá usar essa cadeia de conexão para se conectar ao banco de dados por meio do SQL Management Studio ou de páginas do ASP.NET em seu próprio site. Para ajudar a mitigar essa ameaça, criptografe as informações da cadeia de conexão no `Web.config` usando o sistema de configuração protegida.

> [!NOTE]
> Para obter mais informações sobre os diferentes tipos de autenticação disponíveis no SQL Server, consulte [criando aplicativos seguros do ASP.net: autenticação, autorização e comunicação segura](https://msdn.microsoft.com/library/aa302392.aspx). Para obter mais exemplos de cadeias de conexão que ilustram as diferenças entre a sintaxe de autenticação do Windows e do SQL, consulte [connectionStrings.com](http://www.connectionstrings.com/).

## <a name="summary"></a>Resumo

Por padrão, os arquivos com uma extensão `.config` em um aplicativo ASP.NET não podem ser acessados por meio de um navegador. Esses tipos de arquivos não são retornados porque podem conter informações confidenciais, como cadeias de conexão de banco de dados, nomes de usuários e senhas e assim por diante. O sistema de configuração protegida no .NET 2,0 ajuda a proteger ainda mais as informações confidenciais, permitindo que as seções de configuração especificadas sejam criptografadas. Há dois provedores de configuração protegidos internos: um que usa o algoritmo RSA e outro que usa a API de proteção de dados do Windows (DPAPI).

Neste tutorial, examinamos como criptografar e descriptografar definições de configuração usando o provedor DPAPI. Isso pode ser feito de forma programática, como vimos na etapa 2, bem como por meio da ferramenta de linha de comando `aspnet_regiis.exe`, que foi abordada na etapa 3. Para obter mais informações sobre como usar chaves no nível do usuário ou usar o provedor RSA, consulte os recursos na seção leitura adicional.

Boa programação!

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Criando um aplicativo ASP.NET seguro: autenticação, autorização e comunicação segura](https://msdn.microsoft.com/library/aa302392.aspx)
- [Criptografando informações de configuração em aplicativos ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [Criptografando `Web.config` valores no ASP.NET 2,0](https://weblogs.asp.net/scottgu/archive/2006/01/09/434893.aspx)
- [Como: criptografar seções de configuração no ASP.NET 2,0 usando DPAPI](https://msdn.microsoft.com/library/ms998280.aspx)
- [Como: criptografar seções de configuração no ASP.NET 2,0 usando RSA](https://msdn.microsoft.com/library/ms998283.aspx)
- [A API de configuração no .NET 2,0](http://www.odetocode.com/Articles/418.aspx)
- [Proteção de dados do Windows](https://msdn.microsoft.com/library/ms995355.aspx)

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores potenciais para este tutorial foram Teresa Murphy e Randy Schmidt. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb.md)
> [Próximo](debugging-stored-procedures-vb.md)
