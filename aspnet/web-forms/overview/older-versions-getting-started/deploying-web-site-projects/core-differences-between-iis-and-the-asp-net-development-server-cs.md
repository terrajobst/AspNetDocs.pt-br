---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs
title: Principais diferenças entre o IIS e o ASP.NET Development ServerC#() | Microsoft Docs
author: rick-anderson
description: Ao testar um aplicativo ASP.NET localmente, é provável que você esteja usando o servidor Web de desenvolvimento do ASP.NET. No entanto, o site de produção é provavelmente pow...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: 13a5a423-9235-4dde-b408-2fd10f791d63
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs
msc.type: authoredcontent
ms.openlocfilehash: 7fea3939bd910a052340215c207013e2269f32a2
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74617618"
---
# <a name="core-differences-between-iis-and-the-aspnet-development-server-c"></a>Principais diferenças entre o IIS e o ASP.NET Development Server (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_06_CS.zip) ou [baixar PDF](https://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial06_WebServerDiff_cs.pdf)

> Ao testar um aplicativo ASP.NET localmente, é provável que você esteja usando o servidor Web de desenvolvimento do ASP.NET. No entanto, o site de produção é provavelmente o IIS de energia. Há algumas diferenças entre como esses servidores Web lidam com as solicitações, e essas diferenças podem ter consequências importantes. Este tutorial explora algumas das diferenças mais alemãs.

## <a name="introduction"></a>Introdução

Sempre que um usuário visita um aplicativo ASP.NET, seu navegador envia uma solicitação ao site. Essa solicitação é coletada pelo software do servidor Web, que coordena com o tempo de execução ASP.NET para gerar e retornar o conteúdo para o recurso solicitado. O[**i** nto **i** nformation **S** ervices (IIS)](http://en.wikipedia.org/wiki/Internet_Information_Services) é um pacote de serviços que fornece funcionalidade comum baseada na Internet para servidores Windows. O IIS é o servidor Web mais comumente usado para aplicativos ASP.NET em ambientes de produção; é mais provável que o software do servidor Web esteja sendo usado pelo seu provedor de host da Web para atender seu aplicativo ASP.NET. O IIS também pode ser usado como o software do servidor Web no ambiente de desenvolvimento, embora isso envolve a instalação do IIS e a configuração correta.

O ASP.NET Development Server é uma opção de servidor Web alternativa para o ambiente de desenvolvimento; Ele é fornecido com o e está integrado ao Visual Studio. A menos que o aplicativo Web tenha sido configurado para usar o IIS, o ASP.NET Development Server será iniciado automaticamente e usado como o servidor Web na primeira vez que você visitar uma página da Web de dentro do Visual Studio. Os aplicativos Web de demonstração que criamos de volta no tutorial [*determinando quais arquivos precisam ser implantados*](determining-what-files-need-to-be-deployed-cs.md) eram aplicativos Web baseados em sistema de arquivos que não foram configurados para usar o IIS. Portanto, ao visitar qualquer um desses sites de dentro do Visual Studio, o ASP.NET Development Server é usado.

Em um mundo perfeito, os ambientes de desenvolvimento e produção seriam idênticos. No entanto, como discutimos no tutorial anterior, não é incomum que os ambientes tenham definições de configuração diferentes. O uso de um software de servidor Web diferente nos ambientes adiciona outra variável que deve ser levada em consideração ao implantar um aplicativo. Este tutorial aborda as principais diferenças entre o IIS e o ASP.NET Development Server. Devido a essas diferenças, há cenários em que o código executado sem falhas no ambiente de desenvolvimento gera uma exceção ou se comporta de forma diferente quando executado na produção.

## <a name="security-context-differences"></a>Diferenças de contexto de segurança

Sempre que o software do servidor Web manipula uma solicitação de entrada, ele associa essa solicitação a um contexto de segurança específico. Essas informações de contexto de segurança são usadas pelo sistema operacional para determinar quais ações são permitidas pela solicitação. Por exemplo, uma página ASP.NET pode incluir um código que registra alguma mensagem em um arquivo no disco. Para que essa página ASP.NET seja executada sem erros, o contexto de segurança deve ter as permissões de nível de sistema de arquivo apropriadas, ou seja, permissões de gravação nesse arquivo.

O ASP.NET Development Server associa as solicitações de entrada ao contexto de segurança do usuário conectado no momento. Se você estiver conectado à sua área de trabalho como administrador, as páginas ASP.NET servidas pelo ASP.NET Development Server terão os mesmos direitos de acesso que um administrador. No entanto, as solicitações ASP.NET tratadas pelo IIS são associadas a uma conta de computador específica. Por padrão, a conta da máquina de serviço de rede é usada pelas versões 6 e 7 do IIS, embora o provedor de host da Web possa ter configurado uma conta exclusiva para cada cliente. E mais, o provedor de host da Web provavelmente concedeu permissões limitadas para essa conta de computador. O resultado líquido é que você pode ter páginas da Web que são executadas sem erros no ambiente de desenvolvimento, mas geram exceções relacionadas à autorização quando hospedadas no ambiente de produção.

Para mostrar esse tipo de erro em ação, criei uma página no site de revisões de livros que cria um arquivo em disco que armazena a data e a hora mais recentes que alguém exibiu a revisão *ensinar ASP.NET 3,5 em 24 horas* . Para acompanhar, abra a página `~/Tech/TYASP35.aspx` e adicione o seguinte código ao manipulador de eventos `Page_Load`:

[!code-csharp[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample1.cs)]

> [!NOTE]
> O [método`File.WriteAllText`](https://msdn.microsoft.com/library/system.io.file.writealltext.aspx) criará um novo arquivo se ele não existir e, em seguida, gravará o conteúdo especificado nele. Se o arquivo já existir, o conteúdo existente será substituído.

Em seguida, visite a página de revisão do livro ensinar-se *ASP.NET 3,5 em 24 horas* no ambiente de desenvolvimento usando o ASP.NET Development Server. Supondo que você esteja conectado ao computador com uma conta que tenha permissões adequadas para criar e modificar um arquivo de texto no diretório raiz do aplicativo Web, a análise do livro será exibida da mesma forma que antes, mas sempre que a página for visitada, a data e a hora e o endereço IP do usuário serão armazenados no arquivo de `LastTYASP35Access.txt`. Aponte seu navegador para este arquivo; Você deverá ver uma mensagem semelhante à mostrada na Figura 1.

[![o arquivo de texto contém a última data e a hora em que a revisão do livro foi visitada](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image2.png)](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image1.png)

**Figura 1**: o arquivo de texto contém a última data e a hora em que a revisão do livro foi visitada ([clique para exibir a imagem em tamanho normal](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image3.png))

Implante o aplicativo Web para produção e visite a página de revisão do livro *ensinar você mesmo do ASP.NET 3,5 em 24 horas* . Neste ponto, você deve ver a página de revisão do livro como normal ou a mensagem de erro mostrada na Figura 2. Alguns provedores de host da Web concedem permissões de gravação para a conta de computador ASP.NET anônima; nesse caso, a página funcionará sem erros. No entanto, se o provedor de host da Web proibir o acesso de gravação para a conta anônima, uma [exceção`UnauthorizedAccessException`](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) será gerada quando a página de `TYASP35.aspx` tentar gravar a data e a hora atuais no arquivo de `LastTYASP35Access.txt`.

[![a conta de computador padrão usada pelo IIS não tem permissões para gravar no sistema de arquivos](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image5.png)](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image4.png)

**Figura 2**: a conta de computador padrão usada pelo IIS não tem permissões para gravar no sistema de arquivos ([clique para exibir a imagem em tamanho normal](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image6.png))

A boa notícia é que a maioria dos provedores de host da Web tem alguma ferramenta de permissões que permite especificar permissões do sistema de arquivos em seu site. Conceda à conta ASP.NET anônima acesso de gravação ao diretório raiz e, em seguida, revisite a página de revisão do livro. (Se necessário, contate o provedor de host Web para obter assistência sobre como conceder permissões de gravação para a conta padrão do ASP.NET.) Desta vez, a página deve ser carregada sem erros e o arquivo de `LastTYASP35Access.txt` deve ser criado com êxito.

A retomada aqui é que, como o ASP.NET Development Server opera em um contexto de segurança diferente do IIS, é possível que as páginas do ASP.NET que lêem ou gravem no sistema de arquivos, leiam ou gravem no log de eventos do Windows ou leiam ou gravem nas janelas  o registro funcionará conforme o esperado no desenvolvimento, mas gerará exceções quando estiver na produção. Ao criar um aplicativo Web que será implantado em um ambiente de hospedagem Web compartilhado, não leia ou grave no log de eventos ou no registro do Windows. Observe também as páginas ASP.NET que lêem ou gravam no sistema de arquivos, pois talvez seja necessário conceder privilégios de leitura e gravação nas pastas apropriadas no ambiente de produção.

## <a name="differences-on-serving-static-content"></a>Diferenças no fornecimento de conteúdo estático

Outra diferença principal entre o IIS e o ASP.NET Development Server é como eles lidam com solicitações de conteúdo estático. Cada solicitação que chega ao ASP.NET Development Server, seja para uma página ASP.NET, uma imagem ou um arquivo JavaScript, é processada pelo tempo de execução do ASP.NET. Por padrão, o IIS invoca apenas o tempo de execução ASP.NET quando uma solicitação chega para um recurso ASP.NET, como uma página da Web ASP.NET, um serviço Web e assim por diante. As solicitações de imagens de conteúdo estático, arquivos CSS, arquivos JavaScript, arquivos PDF, arquivos ZIP e similares são recuperadas pelo IIS sem o envolvimento do tempo de execução ASP.NET. (É possível instruir o IIS a trabalhar com o tempo de execução ASP.NET ao servir conteúdo estático; consulte a seção "executando autenticação baseada em formulários e autenticação de URL em arquivos estáticos com o IIS 7" neste tutorial para obter mais informações.)

O tempo de execução do ASP.NET executa várias etapas para gerar o conteúdo solicitado, incluindo a autenticação (identificando o solicitante) e a autorização (determinando se o solicitante tem permissão para exibir o conteúdo solicitado). Uma forma popular de autenticação é a *autenticação baseada em formulários*, na qual um usuário é identificado digitando suas credenciais, geralmente um nome de usuário e senha-em caixas de entrada em uma página da Web. Ao validar suas credenciais, o site armazena um cookie de *tíquete de autenticação* no navegador do usuário, que é enviado com cada solicitação subsequente ao site e é usado para autenticar o usuário. Além disso, é possível especificar regras de *autorização de URL* que ditam o que os usuários podem ou não acessar uma determinada pasta. Muitos sites ASP.NET usam autenticação baseada em formulários e autorização de URL para dar suporte a contas de usuário e para definir partes do site que só podem ser acessadas por usuários autenticados ou usuários que pertencem a uma determinada função.

> [!NOTE]
> Para um exame completo do ASP. Autenticação baseada em formulários do NET, autorização de URL e outros recursos relacionados à conta de usuário, certifique-se de conferir meus [tutoriais de segurança do site](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

Considere um site que dê suporte a contas de usuário usando a autorização baseada em formulários e que tenha uma pasta que, usando a autorização de URL, esteja configurada para permitir somente usuários autenticados. Imagine que essa pasta contenha páginas ASP.NET e arquivos PDF e que a intenção seja que somente os usuários autenticados possam exibir esses arquivos PDF.

O que acontece se um visitante tentar exibir um desses arquivos PDF inserindo a URL diretamente na barra de endereços do navegador? Para descobrir, vamos criar uma nova pasta no site de análises de livros, adicionar alguns arquivos PDF e configurar o site para usar a autorização de URL para proibir que usuários anônimos visitem essa pasta. Se você baixar o aplicativo de demonstração, verá que eu criei uma pasta chamada `PrivateDocs` e adicionei um PDF dos [tutoriais de segurança](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) do meu site (como ajustar!). A pasta `PrivateDocs` também contém um arquivo `Web.config` que especifica as regras de autorização de URL para negar usuários anônimos:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample2.xml)]

Por fim, configurei o aplicativo Web para usar a autenticação baseada em formulários atualizando o arquivo `Web.config` no diretório raiz, substituindo:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample3.xml)]

Por:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample4.xml)]

Usando o ASP.NET Development Server, visite o site e insira a URL direta para um dos arquivos PDF na barra de endereços do navegador. Se você baixou o site associado a este tutorial, a URL deve ser semelhante a: `http://localhost:portNumber/PrivateDocs/aspnet_tutorial01_Basics_vb.pdf`

A inserção dessa URL na barra de endereços faz com que o navegador envie uma solicitação para o ASP.NET Development Server para o arquivo. O ASP.NET Development Server transmite a solicitação para o tempo de execução do ASP.NET para processamento. Como ainda não fizemos logon e como a `Web.config` na pasta `PrivateDocs` está configurada para negar acesso anônimo, o tempo de execução do ASP.NET automaticamente redireciona para a página de logon `Login.aspx` (consulte a Figura 3). Ao redirecionar o usuário para a página de logon, ASP.NET inclui um parâmetro `ReturnUrl` QueryString que indica a página que o usuário estava tentando exibir. Depois de fazer logon com êxito, o usuário pode retornar a esta página.

[![usuários não autorizados são automaticamente redirecionados para a página de logon](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image8.png)](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image7.png)

**Figura 3**: usuários não autorizados são redirecionados automaticamente para a página de logon ([clique para exibir a imagem em tamanho normal](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image9.png))

Agora, vamos ver como isso se comporta na produção. Implante seu aplicativo e insira a URL direta para um dos PDFs na pasta `PrivateDocs` em produção. Isso solicita que seu navegador envie um IIS de solicitação para o arquivo. Como um arquivo estático é solicitado, o IIS recupera e retorna o arquivo sem invocar o tempo de execução ASP.NET. Como resultado, nenhuma verificação de autorização de URL foi executada; o conteúdo do PDF privado do supostamente pode ser acessado por qualquer pessoa que conheça a URL direta para o arquivo.

[![usuários anônimos podem baixar os arquivos PDF privados inserindo a URL direta para o arquivo](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image11.png)](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image10.png)

**Figura 4**: os usuários anônimos podem baixar os arquivos PDF privados inserindo a URL direta para o arquivo ([clique para exibir a imagem em tamanho normal](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image12.png))

### <a name="performing-forms-based-authentication-and-url-authentication-on-static-files-with-iis-7"></a>Executando autenticação baseada em formulários e autenticação de URL em arquivos estáticos com o IIS 7

Há algumas técnicas que você pode usar para proteger o conteúdo estático de usuários não autorizados. O IIS 7 introduziu o *pipeline integrado*, que casasse o fluxo de trabalho do IIS com o fluxo de trabalho do ASP.net Runtime. Resumindo, você pode instruir o IIS a invocar os módulos de autenticação e autorização do tempo de execução do ASP.NET todas as solicitações de entrada (incluindo conteúdo estático, como arquivos PDF). Entre em contato com seu provedor de host Web para saber como configurar seu site para usar o pipeline integrado.

Depois que o IIS tiver sido configurado para usar o pipeline integrado, adicione a seguinte marcação ao arquivo de `Web.config` no diretório raiz:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample5.xml)]

Essa marcação instrui o IIS 7 a usar os módulos de autenticação e autorização baseados em ASP.NET. Implante novamente seu aplicativo e visite novamente o arquivo PDF. Desta vez, quando o IIS lida com a solicitação, ele dá à lógica de autorização e autenticação do tempo de execução do ASP.NET uma oportunidade de inspecionar a solicitação. Como somente usuários autenticados estão autorizados a exibir o conteúdo na pasta `PrivateDocs`, o visitante anônimo é automaticamente redirecionado para a página de logon (consulte novamente a Figura 3).

> [!NOTE]
> Se o provedor de host da Web ainda estiver usando o IIS 6, você não poderá usar o recurso de pipeline integrado. Uma solução alternativa é colocar seus documentos particulares em uma pasta que proíba o acesso HTTP (como `App_Data`) e, em seguida, criar uma página para atender a esses documentos. Essa página pode ser chamada `GetPDF.aspx`e recebe o nome do PDF por meio de um parâmetro QueryString. A página `GetPDF.aspx` primeiro verificaria se o usuário tem permissão para exibir o arquivo e, em caso afirmativo, usaria o método [`Response.WriteFile(filePath)`](https://msdn.microsoft.com/library/system.web.httpresponse.writefile.aspx) para enviar o conteúdo do arquivo PDF solicitado de volta para o cliente solicitante. Essa técnica também funcionaria para o IIS 7 se você não quiser habilitar o pipeline integrado.

## <a name="summary"></a>Resumo

Os aplicativos Web em um ambiente de produção são hospedados usando o software do servidor Web do IIS da Microsoft. No ambiente de desenvolvimento, no entanto, o aplicativo pode ser hospedado usando o IIS ou o ASP.NET Development Server. O ideal é que o mesmo software de servidor Web seja usado em ambos os ambientes porque o uso de software diferente adiciona outra variável na combinação. No entanto, a facilidade de uso do ASP.NET Development Server o torna uma opção atraente no ambiente de desenvolvimento. A boa notícia é que há apenas algumas diferenças fundamentais entre o IIS e o ASP.NET Development Server e, se você estiver ciente dessas diferenças, poderá tomar medidas para ajudar a garantir que o aplicativo funcione e funcione da mesma forma, independentemente do ambiente.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Integração do ASP.NET com o IIS 7,0](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)
- [Usando a autenticação de fóruns do ASP.NET com todos os tipos de conteúdo no IIS 7](https://blogs.iis.net/bills/archive/2007/05/19/using-asp-net-forms-authentication-with-all-types-of-content-with-iis7-video.aspx) (vídeo)
- [Servidores Web no Visual Web Developer](https://msdn.microsoft.com/library/58wxa9w5.aspx)

> [!div class="step-by-step"]
> [Anterior](common-configuration-differences-between-development-and-production-cs.md)
> [Próximo](deploying-a-database-cs.md)
