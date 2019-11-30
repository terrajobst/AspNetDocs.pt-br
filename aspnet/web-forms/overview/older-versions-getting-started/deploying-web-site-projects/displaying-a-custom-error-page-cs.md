---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-cs
title: Exibindo uma página de erroC#personalizada () | Microsoft Docs
author: rick-anderson
description: O que o usuário vê quando ocorre um erro em tempo de execução em um aplicativo Web ASP.NET? A resposta depende de como o &lt;customErrors do site&gt; configuração....
ms.author: riande
ms.date: 06/09/2009
ms.assetid: cb061642-faf3-41b2-9372-69e13444d458
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-cs
msc.type: authoredcontent
ms.openlocfilehash: c1ff4c112b9a489b8fb9ef3443663cd71eda7965
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74625498"
---
# <a name="displaying-a-custom-error-page-c"></a>Exibição de uma página de erro personalizada (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_11_CS.zip) ou [baixar PDF](https://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial11_CustomErrors_cs.pdf)

> O que o usuário vê quando ocorre um erro em tempo de execução em um aplicativo Web ASP.NET? A resposta depende de como o &lt;customErrors do site&gt; configuração. Por padrão, os usuários são mostrados em uma tela amarela insightável que ocorreu um erro de tempo de execução. Este tutorial mostra como personalizar essas configurações para exibir uma página de erro personalizada estéticamente agradável que corresponda à aparência do seu site.

## <a name="introduction"></a>Introdução

Em um mundo perfeito, não haveria nenhum erro em tempo de execução. Os programadores escreveram código com nary um bug e com uma validação de entrada de usuário robusta, e recursos externos como servidores de banco de dados e servidores de email nunca ficarão offline. É claro que, na realidade, os erros são inevitáveis. As classes no .NET Framework sinalizam um erro lançando uma exceção. Por exemplo, chamar o método Open de um objeto SqlConnection estabelece uma conexão com o banco de dados especificado por uma cadeia de conexão. No entanto, se o banco de dados estiver inoperante ou se as credenciais na cadeia de conexão forem inválidas, o método Open lançará um `SqlException`. As exceções podem ser tratadas pelo uso de blocos de `try/catch/finally`. Se o código dentro de um bloco de `try` gerar uma exceção, o controle será transferido para o bloco catch apropriado, no qual o desenvolvedor pode tentar se recuperar do erro. Se não houver nenhum bloco catch correspondente ou se o código que gerou a exceção não estiver em um bloco try, a exceção percolates a pilha de chamadas na pesquisa de blocos de `try/catch/finally`.

Se a exceção se somar até o tempo de execução ASP.NET sem ser manipulada, o [evento`Error`](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx) da [classe`HttpApplication`](https://msdn.microsoft.com/library/system.web.httpapplication.aspx)será gerado e a *página de erro* configurada será exibida. Por padrão, ASP.NET exibe uma página de erro que é carinhosamente referenciada como a [tela amarela de morte](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow) (YSOD). Há duas versões do YSOD: uma mostra os detalhes da exceção, um rastreamento de pilha e outras informações úteis para os desenvolvedores que depuram o aplicativo (veja a **Figura 1**); a outra simplesmente afirma que houve um erro em tempo de execução (consulte a **Figura 2**).

Os detalhes da exceção YSOD são muito úteis para os desenvolvedores Depurando o aplicativo, mas mostrar um YSOD aos usuários finais é um pouco e não profissional. Em vez disso, os usuários finais devem ser levados para uma página de erro que mantém a aparência do site com um suplemento mais amigável que descreve a situação. A boa notícia é que a criação dessa página de erro personalizada é muito fácil. Este tutorial começa com uma visão do ASP. Páginas de erro diferentes da rede. Em seguida, ele mostra como configurar o aplicativo Web para mostrar aos usuários uma página de erro personalizada diante de um erro.

### <a name="examining-the-three-types-of-error-pages"></a>Examinando os três tipos de páginas de erro

Quando surge uma exceção sem tratamento em um aplicativo ASP.NET, um dos três tipos de páginas de erro é exibido:

- O detalhes da exceção tela amarela da página de erro de morte,
- O erro de tempo de execução tela amarela da página de erro de morte ou
- Uma página de erro personalizada

A página de erro que os desenvolvedores estão mais familiarizados são os detalhes da exceção YSOD. Por padrão, essa página é exibida para os usuários que estão visitando localmente e, portanto, é a página que você vê quando ocorre um erro ao testar o site no ambiente de desenvolvimento. Como o nome indica, os detalhes da exceção YSOD fornecem detalhes sobre a exceção-o tipo, a mensagem e o rastreamento de pilha. Além disso, se a exceção tiver sido gerada pelo código na classe code-behind da página ASP.NET e se o aplicativo estiver configurado para depuração, os detalhes da exceção YSOD também mostrarão essa linha de código (e algumas linhas de código acima e abaixo dele).

A **Figura 1** mostra a página detalhes da exceção YSOD. Observe a URL na janela de endereço do navegador: `http://localhost:62275/Genre.aspx?ID=foo`. Lembre-se de que a página `Genre.aspx` lista as revisões de livros em um gênero específico. Ele requer que `GenreId` valor (um `uniqueidentifier`) seja passado por meio de QueryString; por exemplo, a URL apropriada para exibir as revisões de ficção é `Genre.aspx?ID=7683ab5d-4589-4f03-a139-1c26044d0146`. Se um valor não`uniqueidentifier` for passado por meio de QueryString (como "foo"), uma exceção será lançada.

> [!NOTE]
> Para reproduzir esse erro no aplicativo Web de demonstração disponível para download, você pode visitar `Genre.aspx?ID=foo` diretamente ou clicar no link "gerar um erro de tempo de execução" em `Default.aspx`.

Observe as informações de exceção apresentadas na **Figura 1**. A mensagem de exceção "falha na conversão ao converter de uma cadeia de caracteres em uniqueidentifier" está presente na parte superior da página. O tipo de exceção, `System.Data.SqlClient.SqlException`, é listado também. Também há o rastreamento de pilha.

[![](displaying-a-custom-error-page-cs/_static/image2.png)](displaying-a-custom-error-page-cs/_static/image1.png)

**Figura 1**: os detalhes da exceção YSOD inclui informações sobre a exceção  
 ([Clique para exibir a imagem em tamanho normal](displaying-a-custom-error-page-cs/_static/image3.png))

O outro tipo de YSOD é o erro de tempo de execução YSOD e é mostrado na **Figura 2**. O erro de tempo de execução YSOD informa ao visitante que ocorreu um erro em tempo de execução, mas não inclui nenhuma informação sobre a exceção que foi lançada. (No entanto, ele fornece instruções sobre como tornar os detalhes do erro visíveis modificando o arquivo de `Web.config`, que faz parte do que torna essa aparência YSOD.)

Por padrão, o erro de tempo de execução YSOD é mostrado aos usuários que visitam remotamente (por meio de http://www.yoursite.com), conforme evidenciado pela URL na barra de endereços do navegador na **Figura 2**: `http://httpruntime.web703.discountasp.net/Genre.aspx?ID=foo`. As duas diferentes telas de YSOD existem porque os desenvolvedores estão interessados em saber os detalhes do erro, mas essas informações não devem ser mostradas em um site ativo, pois podem revelar possíveis vulnerabilidades de segurança ou outras informações confidenciais a qualquer pessoa que visite seu locais.

> [!NOTE]
> Se estiver acompanhando e usando DiscountASP.NET como seu host da Web, você pode observar que o erro de tempo de execução YSOD não é exibido ao visitar o site ativo. Isso ocorre porque o DiscountASP.NET tem seus servidores configurados para mostrar os detalhes da exceção YSOD por padrão. A boa notícia é que você pode substituir esse comportamento padrão adicionando uma seção `<customErrors>` ao arquivo `Web.config`. A seção "Configurando qual página de erro é exibida" examina a seção `<customErrors>` detalhadamente.

[![](displaying-a-custom-error-page-cs/_static/image5.png)](displaying-a-custom-error-page-cs/_static/image4.png)

**Figura 2**: o erro de tempo de execução YSOD não inclui nenhum detalhe de erro  
 ([Clique para exibir a imagem em tamanho normal](displaying-a-custom-error-page-cs/_static/image6.png))

O terceiro tipo de página de erro é a página de erro personalizada, que é uma página da Web que você cria. O benefício de uma página de erro personalizada é que você tem controle total sobre as informações que são exibidas para o usuário junto com a aparência da página; a página de erro personalizada pode usar a mesma página mestra e estilos que suas outras páginas. A seção "usando uma página de erro personalizada" orienta a criação de uma página de erro personalizada e sua configuração para exibição no caso de uma exceção sem tratamento. A **Figura 3** oferece uma repiada dessa página de erro personalizada. Como você pode ver, a aparência da página de erro é muito mais profissional-a aparência de uma das telas amarelas de morte mostradas nas figuras 1 e 2.

[![](displaying-a-custom-error-page-cs/_static/image8.png)](displaying-a-custom-error-page-cs/_static/image7.png)

**Figura 3**: uma página de erro personalizada oferece uma aparência mais personalizada  
 ([Clique para exibir a imagem em tamanho normal](displaying-a-custom-error-page-cs/_static/image9.png))

Reserve um momento para inspecionar a barra de endereços do navegador na **Figura 3**. Observe que a barra de endereços mostra a URL da página de erro personalizada (`/ErrorPages/Oops.aspx`). Nas figuras 1 e 2, as telas amarelas de morte são mostradas na mesma página da qual o erro foi originado (`Genre.aspx`). A página de erro personalizada recebe a URL da página em que o erro ocorreu por meio do parâmetro `aspxerrorpath` QueryString.

## <a name="configuring-which-error-page-is-displayed"></a>Configurando qual página de erro é exibida

Qual das três páginas de erro possíveis é exibida com base em duas variáveis:

- As informações de configuração na seção `<customErrors>` e
- Se o usuário está visitando o site localmente ou remotamente.

A [seção`<customErrors>`](https://msdn.microsoft.com/library/h0hfz6fc.aspx) em `Web.config` tem dois atributos que afetam qual página de erro é mostrada: `defaultRedirect` e `mode`. O atributo `defaultRedirect` é opcional. Se fornecido, ele especifica a URL da página de erro personalizada e indica que a página de erro personalizada deve ser mostrada em vez do erro de tempo de execução YSOD. O atributo `mode` é necessário e aceita um dos três valores: `On`, `Off`ou `RemoteOnly`. Esses valores têm o seguinte comportamento:

- `On`-indica que a página de erro personalizada ou o erro de tempo de execução YSOD é mostrado para todos os visitantes, independentemente de serem locais ou remotos.
- `Off`-especifica que os detalhes da exceção YSOD são exibidos para todos os visitantes, independentemente de serem locais ou remotos.
- `RemoteOnly`-indica que a página de erro personalizada ou o erro de tempo de execução YSOD é mostrado aos visitantes remotos, enquanto os detalhes da exceção YSOD são mostrados aos visitantes locais.

A menos que você especifique o contrário, ASP.NET age como se você tivesse definido o atributo Mode como `RemoteOnly` e não tinha especificado um valor de `defaultRedirect`. Em outras palavras, o comportamento padrão é que os detalhes da exceção YSOD são exibidos para visitantes locais enquanto o erro de tempo de execução YSOD é mostrado aos visitantes remotos. Você pode substituir esse comportamento padrão adicionando uma seção `<customErrors>` à `Web.config file.` do seu aplicativo Web

## <a name="using-a-custom-error-page"></a>Usando uma página de erro personalizada

Todo aplicativo Web deve ter uma página de erro personalizada. Ele fornece uma alternativa de aparência profissional para o erro de tempo de execução YSOD, é fácil de criar e a configuração do aplicativo para usar a página de erro personalizada leva apenas alguns minutos. A primeira etapa é criar a página de erro personalizada. Adicionei uma nova pasta ao aplicativo Reviews do livro chamado `ErrorPages` e adicionei a essa nova página ASP.NET chamada `Oops.aspx`. Faça com que a página use a mesma página mestra do restante das páginas no seu site para que ela herde automaticamente a mesma aparência.

[![](displaying-a-custom-error-page-cs/_static/image11.png)](displaying-a-custom-error-page-cs/_static/image10.png)

**Figura 4**: criar uma página de erro personalizada

Em seguida, dedique alguns minutos criando o conteúdo da página de erro. Criei uma página de erro personalizada bastante simples com uma mensagem indicando que houve um erro inesperado e um link de volta para a home page do site.

[![](displaying-a-custom-error-page-cs/_static/image13.png)](displaying-a-custom-error-page-cs/_static/image12.png)

**Figura 5**: criar sua página de erro personalizada  
 ([Clique para exibir a imagem em tamanho normal](displaying-a-custom-error-page-cs/_static/image14.png))

Com a página de erro concluída, configure o aplicativo Web para usar a página de erro personalizada no lugar do erro de tempo de execução YSOD. Isso é feito especificando a URL da página de erro no atributo `defaultRedirect` da seção `<customErrors>`. Adicione a seguinte marcação ao arquivo de `Web.config` do seu aplicativo:

[!code-xml[Main](displaying-a-custom-error-page-cs/samples/sample1.xml)]

A marcação acima configura o aplicativo para mostrar os detalhes da exceção YSOD aos usuários que visitam localmente, ao usar a página de erro personalizada Ops. aspx para os usuários que visitam remotamente. Para ver isso em ação, implante seu site no ambiente de produção e visite a página gênero. aspx no site ativo com um valor de QueryString inválido. Você deverá ver a página de erro personalizada (consulte novamente a **Figura 3**).

Para verificar se a página de erro personalizada é mostrada apenas para usuários remotos, visite a página `Genre.aspx` com uma QueryString inválida do ambiente de desenvolvimento. Você ainda deve ver os detalhes da exceção YSOD (consulte novamente a **Figura 1**). A configuração de `RemoteOnly` garante que os usuários visitando o site no ambiente de produção consulte a página de erro personalizada enquanto os desenvolvedores trabalhando localmente continuam a ver os detalhes da exceção.

## <a name="notifying-developers-and-logging-error-details"></a>Notificando os desenvolvedores e registrando detalhes do erro

Os erros que ocorrem no ambiente de desenvolvimento foram causados pelo desenvolvedor que reside em seu computador. Ela mostra as informações da exceção nos detalhes da exceção YSOD e sabe quais etapas ela estava executando quando o erro ocorreu. Mas quando ocorre um erro na produção, o desenvolvedor não tem conhecimento de que ocorreu um erro, a menos que o usuário final que está visitando o site demore o tempo para relatar o erro. E, mesmo que o usuário saia de seu caminho para alertar a equipe de desenvolvimento de que ocorreu um erro, sem conhecer o tipo de exceção, a mensagem e o rastreamento de pilha, pode ser difícil diagnosticar a causa do erro e, assim, corrigi-lo.

Por esses motivos, é fundamental que qualquer erro no ambiente de produção seja registrado em um repositório persistente (como um banco de dados) e que os desenvolvedores sejam alertados sobre esse erro. A página de erro personalizada pode parecer um bom local para fazer esse registro em log e notificação. Infelizmente, a página de erro personalizada não tem acesso aos detalhes do erro e, portanto, não pode ser usada para registrar essas informações. A boa notícia é que há várias maneiras de interceptar os detalhes do erro e de fazê-los, e os próximos três tutoriais exploram esse tópico em mais detalhes.

## <a name="using-different-custom-error-pages-for-different-http-error-statuses"></a>Usando páginas de erro personalizadas diferentes para diferentes status de erro HTTP

Quando uma exceção é gerada por uma página do ASP.NET e não é manipulada, a exceção percolates até o tempo de execução do ASP.NET, que exibe a página de erro configurada. Se uma solicitação entrar no mecanismo de ASP.NET, mas não puder ser processada por algum motivo, talvez o arquivo solicitado não seja encontrado ou as permissões de leitura tenham sido desabilitadas para o arquivo-Então, o mecanismo de ASP.NET gerará um `HttpException`. Essa exceção, como exceções geradas de páginas ASP.NET, se destaca no tempo de execução, fazendo com que a página de erro apropriada seja exibida.

O que isso significa para o aplicativo Web em produção é que, se um usuário solicitar uma página que não foi encontrada, ele verá a página de erro personalizada. A **Figura 6** mostra um exemplo desse tipo. Como a solicitação é para uma página não existente (`NoSuchPage.aspx`), um `HttpException` é gerado e a página de erro personalizada é exibida (Observe a referência a `NoSuchPage.aspx` no parâmetro QueryString `aspxerrorpath`).

[![](displaying-a-custom-error-page-cs/_static/image16.png)](displaying-a-custom-error-page-cs/_static/image15.png)

**Figura 6**: o tempo de execução do ASP.net exibe a página de erro configurada em resposta a uma solicitação inválida ([clique para exibir a imagem em tamanho normal](displaying-a-custom-error-page-cs/_static/image17.png))

Por padrão, todos os tipos de erros fazem com que a mesma página de erro personalizada seja exibida. No entanto, você pode especificar uma página de erro personalizada diferente para um código de status HTTP específico usando `<error>` elementos filhos dentro da seção `<customErrors>`. Por exemplo, para ter uma página de erro diferente exibida no caso de um erro de página não encontrada, que tem um código de status HTTP 404, atualize a seção `<customErrors>` para incluir a seguinte marcação:

[!code-xml[Main](displaying-a-custom-error-page-cs/samples/sample2.xml)]

Com essa alteração no local, sempre que um usuário visitar remotamente um recurso ASP.NET que não existe, ele será redirecionado para a página de erro `404.aspx` personalizada em vez de `Oops.aspx`. Como a **Figura 7** ilustra, a página `404.aspx` pode incluir uma mensagem mais específica do que a página de erro geral personalizada.

> [!NOTE]
> Confira [404 páginas de erro, mais uma vez](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/) para obter orientação sobre como criar páginas de erro 404 efetivas.

[![](displaying-a-custom-error-page-cs/_static/image19.png)](displaying-a-custom-error-page-cs/_static/image18.png)

**Figura 7**: a página de erro 404 Personalizada exibe uma mensagem mais direcionada do que `Oops.aspx`  
([Clique para exibir a imagem em tamanho normal](displaying-a-custom-error-page-cs/_static/image20.png)) 

Como você sabe que a página de `404.aspx` só é atingida quando o usuário faz uma solicitação para uma página que não foi encontrada, você pode aprimorar essa página de erro personalizada para incluir funcionalidade para ajudar o usuário a resolver esse tipo específico de erro. Por exemplo, você pode criar uma tabela de banco de dados que mapeia URLs ruins conhecidas para boas URLs e, em seguida, fazer com que a página de erro `404.aspx` personalizada execute uma consulta nessa tabela e sugira páginas que o usuário pode estar tentando acessar.

> [!NOTE]
> A página de erro personalizada só é exibida quando uma solicitação é feita a um recurso manipulado pelo mecanismo do ASP.NET. Como discutimos nas [principais diferenças entre o IIS e o tutorial ASP.NET Development Server](core-differences-between-iis-and-the-asp-net-development-server-cs.md) , o servidor Web pode manipular determinadas solicitações por conta própria. Por padrão, o servidor Web do IIS processa solicitações de conteúdo estático, como imagens e arquivos HTML, sem invocar o mecanismo ASP.NET. Consequentemente, se o usuário solicitar um arquivo de imagem não existente, ele receberá a mensagem de erro padrão do IIS 404 em vez de ASP. Página de erro configurada da rede.

## <a name="summary"></a>Resumo

Quando ocorre uma exceção sem tratamento em um aplicativo ASP.NET, o usuário é mostrado em uma das três páginas de erro: a exceção indica a tela amarela de morte; o erro de tempo de execução tela amarela de morte; ou uma página de erro personalizada. A página de erro exibida depende da configuração de `<customErrors>` do aplicativo e se o usuário está visitando local ou remotamente. O comportamento padrão é mostrar os detalhes da exceção YSOD aos visitantes locais e o erro de tempo de execução YSOD para os visitantes remotos.

Embora o erro de tempo de execução YSOD oculte as informações de erro potencialmente confidenciais do usuário que está visitando o site, ele quebra a aparência do seu site e faz com que seu aplicativo pareça contratado. Uma abordagem melhor é usar uma página de erro personalizada, que envolve criar e criar a página de erro personalizada e especificar sua URL no atributo `defaultRedirect` da seção de `<customErrors>`. Você pode até mesmo ter várias páginas de erro personalizadas para diferentes status de erro HTTP.

A página de erro personalizada é a primeira etapa de uma estratégia de tratamento de erros abrangente para um site em produção. Alertar o desenvolvedor do erro e registrar seus detalhes também é uma etapa importante. Os próximos três tutoriais exploram técnicas para notificação de erros e registro em log.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Páginas de erro, mais uma vez](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/)
- [Diretrizes de design para exceções](https://msdn.microsoft.com/library/ms229014.aspx)
- [Páginas de erro amigáveis](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [Manipulando e gerando exceções](https://msdn.microsoft.com/library/5b2yeyab.aspx)
- [Usando páginas de erro personalizadas corretamente no ASP.NET](http://professionalaspnet.com/archive/2007/09/30/Properly-Using-Custom-Error-Pages-in-ASP.NET.aspx)

> [!div class="step-by-step"]
> [Anterior](strategies-for-database-development-and-deployment-cs.md)
> [Próximo](processing-unhandled-exceptions-cs.md)
