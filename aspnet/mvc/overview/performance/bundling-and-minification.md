---
uid: mvc/overview/performance/bundling-and-minification
title: Agrupamento e Minificação | Microsoft Docs
author: Rick-Anderson
description: O agrupamento e o minificação são duas técnicas que podem ser usadas no ASP.NET 4,5 para melhorar o tempo de carregamento da solicitação. O agrupamento e o minificação melhora o tempo de carregamento por reducin...
ms.author: riande
ms.date: 08/23/2012
ms.assetid: 5894dc13-5d45-4dad-8096-136499120f1d
msc.legacyurl: /mvc/overview/performance/bundling-and-minification
msc.type: authoredcontent
ms.openlocfilehash: 239980d747c6e0d6be1e9b4fe0371e276e37cf21
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519278"
---
# <a name="bundling-and-minification"></a>Empacotamento e minimização

por [Rick Anderson]((https://twitter.com/RickAndMSFT))

> O agrupamento e o minificação são duas técnicas que podem ser usadas no ASP.NET 4,5 para melhorar o tempo de carregamento da solicitação. O agrupamento e a minificação melhora o tempo de carregamento, reduzindo o número de solicitações para o servidor e reduzindo o tamanho dos ativos solicitados (como CSS e JavaScript).

A maioria dos principais navegadores atuais limita o número de [conexões simultâneas](http://www.browserscope.org/?category=network) por cada nome de host a seis. Isso significa que, enquanto seis solicitações estão sendo processadas, as solicitações adicionais de ativos em um host serão enfileiradas pelo navegador. Na imagem abaixo, as guias de rede de ferramentas de desenvolvedor F12 do IE mostram o tempo dos ativos exigidos pela exibição sobre de um aplicativo de exemplo.

![B/M](bundling-and-minification/_static/image1.png)

As barras cinzas mostram a hora em que a solicitação é enfileirada pelo navegador aguardando o limite de seis conexões. A barra amarela é o tempo de solicitação para o primeiro byte, ou seja, o tempo necessário para enviar a solicitação e receber a primeira resposta do servidor. As barras azuis mostram o tempo necessário para receber os dados de resposta do servidor. Você pode clicar duas vezes em um ativo para obter informações detalhadas de tempo. Por exemplo, a imagem a seguir mostra os detalhes de tempo para carregar o arquivo */scripts/MyScripts/JavaScript6.js* .

![](bundling-and-minification/_static/image2.png)

A imagem anterior mostra o evento **Iniciar** , que fornece a hora em que a solicitação foi enfileirada devido ao navegador limitar o número de conexões simultâneas. Nesse caso, a solicitação foi enfileirada por 46 milissegundos aguardando a conclusão de outra solicitação.

## <a name="bundling"></a>Reagrupamento

O agrupamento é um novo recurso do ASP.NET 4,5 que torna mais fácil combinar ou agrupar vários arquivos em um único arquivo. Você pode criar CSS, JavaScript e outros pacotes. Menos arquivos significa menos solicitações HTTP e que podem melhorar o desempenho de carregamento da primeira página.

A imagem a seguir mostra a mesma exibição de tempo da exibição sobre mostrada anteriormente, mas desta vez com o agrupamento e o minificação habilitados.

![](bundling-and-minification/_static/image3.png)

## <a name="minification"></a>Minificação

O minificação executa uma variedade de otimizações de código diferentes para scripts ou CSS, como remover espaços em branco e comentários desnecessários e encurtar nomes de variáveis para um caractere. Considere a seguinte função JavaScript.

[!code-javascript[Main](bundling-and-minification/samples/sample1.js)]

Após minificação, a função é reduzida para o seguinte:

[!code-javascript[Main](bundling-and-minification/samples/sample2.js)]

Além de remover os comentários e o espaço em branco desnecessário, os seguintes parâmetros e nomes de variáveis foram renomeados (reduzidos) da seguinte maneira:

| **Original** | **Renomeado** |
| --- | --- |
| imageTagAndImageID | {1&gt;n&lt;1} |
| imageContext | t |
| imageElement | {1&gt;i&lt;1} |

## <a name="impact-of-bundling-and-minification"></a>Impacto do agrupamento e Minificação

A tabela a seguir mostra várias diferenças importantes entre a listagem de todos os ativos individualmente e o uso de agrupamento e minificação (B/M) no programa de exemplo.

|  | **Usando B/M** | **Sem B/M** | **Alteração** |
| --- | --- | --- | --- |
| **Solicitações de arquivo** | 9 | 34 | 256% |
| **KB enviados** | 3.26 | 11.92 | 266% |
| **KB recebidos** | 388.51 | 530 | 36% |
| **Tempo de carregamento** | 510 MS | 780 MS | 53% |

Os bytes enviados tiveram uma redução significativa com o agrupamento, uma vez que os navegadores são bastante detalhados com os cabeçalhos HTTP que eles aplicam nas solicitações. A redução de bytes recebidos não é tão grande porque os maiores arquivos (*Scripts\\jQuery-UI-1.8.11. min. js* e *scripts\\jQuery-1.7.1. min. js*) já são reduzidos. Observação: os intervalos no programa de exemplo usaram a ferramenta [Fiddler](http://www.fiddler2.com/fiddler2/) para simular uma rede lenta. (No menu **regras** Fiddler, selecione **desempenho** e, em seguida, **simular velocidades de modem**.)

## <a name="debugging-bundled-and-minified-javascript"></a>Depurando o JavaScript agrupado e reduzidos

É fácil depurar o JavaScript em um ambiente de desenvolvimento (em que o [elemento Compilation](https://msdn.microsoft.com/library/s10awwz0.aspx) no arquivo *Web. config* é definido como `debug="true"`) porque os arquivos JavaScript não estão agrupados ou reduzidos. Você também pode depurar uma compilação de versão onde os arquivos JavaScript são agrupados e reduzidos. Usando as ferramentas de desenvolvedor F12 do IE, você depura uma função JavaScript incluída em um pacote reduzidos usando a seguinte abordagem:

1. Selecione a guia **script** e, em seguida, selecione o botão **Iniciar Depuração** .
2. Selecione o pacote que contém a função JavaScript que você deseja depurar usando o botão ativos.  
    ![](bundling-and-minification/_static/image4.png)
3. Formate o JavaScript reduzidos selecionando o **botão configuração** ![](bundling-and-minification/_static/image5.png)e, em seguida, selecionando **Formatar JavaScript**.
4. Na caixa de entrada do **script de pesquisa** , selecione o nome da função que você deseja depurar. Na imagem a seguir, **AddAltToImg** foi inserido na caixa de entrada do **script de pesquisa** .  
    ![](bundling-and-minification/_static/image6.png)

Para obter mais informações sobre a depuração com as ferramentas de desenvolvedor F12, consulte o artigo do MSDN [usando o ferramentas para desenvolvedores F12 para depurar erros de JavaScript](https://msdn.microsoft.com/library/ie/gg699336(v=vs.85).aspx).

## <a name="controlling-bundling-and-minification"></a>Controlando o agrupamento e o Minificação

O agrupamento e o minificação são habilitados ou desabilitados pela definição do valor do atributo debug no [elemento Compilation](https://msdn.microsoft.com/library/s10awwz0.aspx) no arquivo *Web. config* . No XML a seguir, `debug` é definido como true para que o agrupamento e o minificação sejam desabilitados.

[!code-xml[Main](bundling-and-minification/samples/sample3.xml?highlight=2)]

Para habilitar o agrupamento e o minificação, defina o valor de `debug` como "false". Você pode substituir a configuração *Web. config* pela propriedade `EnableOptimizations` na classe `BundleTable`. O código a seguir habilita o agrupamento e o minificação e substitui qualquer configuração no arquivo *Web. config* .

[!code-csharp[Main](bundling-and-minification/samples/sample4.cs?highlight=7)]

> [!NOTE]
> A menos que `EnableOptimizations` seja `true` ou o atributo debug no [elemento Compilation](https://msdn.microsoft.com/library/s10awwz0.aspx) no arquivo *Web. config* esteja definido como `false`, os arquivos não serão agrupados ou reduzidos. Além disso, a versão. min de arquivos não será usada, as versões de depuração completas serão selecionadas. `EnableOptimizations` substitui o atributo debug no [elemento Compilation](https://msdn.microsoft.com/library/s10awwz0.aspx) do arquivo *Web. config*

## <a name="using-bundling-and-minification-with-aspnet-web-forms-and-web-pages"></a>Usando agrupamento e Minificação com ASP.NET Web Forms e páginas da Web

- Para páginas da Web, consulte a entrada de blog [adicionando otimização da Web a um site de páginas da Web](https://docs.microsoft.com/archive/blogs/rickandy/adding-web-optimization-to-a-web-pages-site).
- Para Web Forms, consulte a entrada de blog [adicionando agrupamento e minificação a Web Forms](https://docs.microsoft.com/archive/blogs/rickandy/adding-bundling-and-minification-to-web-forms).

## <a name="using-bundling-and-minification-with-aspnet-mvc"></a>Usando o agrupamento e o Minificação com o ASP.NET MVC

Nesta seção, criaremos um projeto MVC do ASP.NET para examinar o agrupamento e o minificação. Primeiro, crie um novo projeto de Internet do ASP.NET MVC chamado **MvcBM** sem alterar nenhum dos padrões.

Abra o *aplicativo\\\_iniciar\\arquivo BundleConfig.cs* e examine o método `RegisterBundles` que é usado para criar, registrar e configurar pacotes. O código a seguir mostra uma parte do método `RegisterBundles`.

[!code-csharp[Main](bundling-and-minification/samples/sample5.cs)]

O código anterior cria um novo pacote JavaScript chamado *~/Bundles/jQuery* que inclui todos os apropriados (que é debug ou reduzidos, mas não. *vsdoc*) arquivos na pasta *scripts* que correspondem à cadeia de caracteres curinga "~/scripts/jQuery-{Version}.js". Para o ASP.NET MVC 4, isso significa que, com uma configuração de depuração, o arquivo *jQuery-1.7.1. js* será adicionado ao pacote. Em uma configuração de versão, *jQuery-1.7.1. min. js* será adicionado. A estrutura de agrupamento segue várias convenções comuns, como:

- Selecionando o arquivo ". min" para liberação quando *FileX. min. js* e *FileX. js* existirem.
- Selecionando a versão não ". min" para depuração.
- Ignorando arquivos "-vsdoc" (como *jQuery-1.7.1-vsdoc. js*), que são usados somente pelo IntelliSense.

A correspondência de curingas `{version}` mostrada acima é usada para criar automaticamente um pacote do jQuery com a versão apropriada do jQuery na sua pasta de *scripts* . Neste exemplo, o uso de um curinga oferece os seguintes benefícios:

- Permite que você use o NuGet para atualizar para uma versão mais recente do jQuery sem alterar o código de agrupamento ou as referências jQuery anteriores em suas páginas de exibição.
- Seleciona automaticamente a versão completa para as configurações de depuração e a versão ". min" para compilações de versão.

## <a name="using-a-cdn"></a>Usando uma CDN

 O código a seguir substitui o grupo local do jQuery por um pacote da CDN do jQuery.

[!code-csharp[Main](bundling-and-minification/samples/sample6.cs)]

No código acima, o jQuery será solicitado da CDN enquanto estiver no modo de liberação e a versão de depuração do jQuery será buscada localmente no modo de depuração. Ao usar uma CDN, você deve ter um mecanismo de fallback caso a solicitação da CDN falhe. O fragmento de marcação a seguir do final do arquivo de layout mostra o script adicionado para solicitar o jQuery caso a CDN falhe.

[!code-cshtml[Main](bundling-and-minification/samples/sample7.cshtml?highlight=5-13)]

## <a name="creating-a-bundle"></a>Criando um pacote

A classe [Bundle](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) `Include` método usa uma matriz de cadeias de caracteres, em que cada cadeia de caracteres é um caminho virtual para o recurso. O código a seguir do método `RegisterBundles` no *\\de aplicativo \_iniciar\\arquivo BundleConfig.cs* mostra como vários arquivos são adicionados a um pacote:

[!code-csharp[Main](bundling-and-minification/samples/sample8.cs)]

A classe [Bundle](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) `IncludeDirectory` método é fornecido para adicionar todos os arquivos em um diretório (e, opcionalmente, todos os subdiretórios) que correspondem a um padrão de pesquisa. A classe de [pacote](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) `IncludeDirectory` API é mostrada abaixo:

[!code-csharp[Main](bundling-and-minification/samples/sample9.cs)]

Os grupos são referenciados em exibições usando o método Render, (`Styles.Render` para CSS e `Scripts.Render` para JavaScript). A marcação a seguir do arquivo *views\\compartilhado\\\_layout. cshtml* mostra como os modos de exibição padrão do projeto de Internet ASP.net referenciam os pacotes CSS e JavaScript.

[!code-cshtml[Main](bundling-and-minification/samples/sample10.cshtml?highlight=5-6,11)]

Observe que os métodos render assumem uma matriz de cadeias de caracteres, para que você possa adicionar vários pacotes em uma linha de código. Geralmente, você desejará usar os métodos render que criam o HTML necessário para referenciar o ativo. Você pode usar o método `Url` para gerar a URL para o ativo sem a marcação necessária para fazer referência ao ativo. Suponha que você quisesse usar o novo atributo [Async](http://www.whatwg.org/specs/web-apps/current-work/#attr-script-async) do HTML5. O código a seguir mostra como referenciar o Modernizr usando o método `Url`.

[!code-cshtml[Main](bundling-and-minification/samples/sample11.cshtml?highlight=11)]

## <a name="using-the--wildcard-character-to-select-files"></a>Usando o caractere curinga "\*" para selecionar arquivos

O caminho virtual especificado no método `Include` e o padrão de pesquisa no método `IncludeDirectory` podem aceitar um caractere curinga "\*" como prefixo ou sufixo para no último segmento de caminho. A cadeia de caracteres de pesquisa não diferencia maiúsculas de minúsculas. O método `IncludeDirectory` tem a opção de Pesquisar subdiretórios.

Considere um projeto com os seguintes arquivos JavaScript:

- *Scripts\\Common\\AddAltToImg.js*
- *Scripts\\Common\\ToggleDiv. js*
- *Scripts\\Common\\ToggleImg.js*
- *Scripts\\Common\\Sub1\\ToggleLinks.js*

![Dir imagem](bundling-and-minification/_static/image7.png)

A tabela a seguir mostra os arquivos adicionados a um pacote usando o caractere curinga, conforme mostrado:

| **Call** | **Arquivos adicionados ou gerados pela exceção** |
| --- | --- |
| Include("~/Scripts/Common/\*.js") | *AddAltToImg.js*, *ToggleDiv.js*, *ToggleImg.js* |
| Include("~/Scripts/Common/T\*.js") | Exceção de padrão inválida. O caractere curinga só é permitido no prefixo ou sufixo. |
| Include("~/Scripts/Common/\*og.\*") | Exceção de padrão inválida. Somente um caractere curinga é permitido. |
| Include ("~/Scripts/Common/T\*") | *ToggleDiv. js*, *ToggleImg. js* |
| Include ("~/Scripts/Common/\*") | Exceção de padrão inválida. Um segmento curinga puro não é válido. |
| IncludeDirectory("~/Scripts/Common", "T\*") | *ToggleDiv. js*, *ToggleImg. js* |
| IncludeDirectory("~/Scripts/Common", "T\*", true) | *ToggleDiv.js*, *ToggleImg.js*, *ToggleLinks.js* |

Adicionar explicitamente cada arquivo a um pacote é geralmente o carregamento preferível de arquivos por curinga pelos seguintes motivos:

- Adicionar scripts por padrões de curinga para carregá-los em ordem alfabética, que normalmente não é o que você deseja. Os arquivos CSS e JavaScript frequentemente precisam ser adicionados em uma ordem específica (não alfabética). Você pode mitigar esse risco adicionando uma implementação de [IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) personalizada, mas adicionar explicitamente cada arquivo é menos propenso a erros. Por exemplo, você pode adicionar novos ativos a uma pasta no futuro, o que pode exigir que você modifique sua implementação de [IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) .
- Exibir arquivos específicos adicionados a um diretório usando o carregamento de curingas pode ser incluído em todas as exibições que fazem referência a esse pacote. Se o script específico de exibição for adicionado a um pacote, você poderá receber um erro de JavaScript em outras exibições que fazem referência ao pacote.
- Arquivos CSS que importam outros arquivos resultam em arquivos importados carregados duas vezes. Por exemplo, o código a seguir cria um pacote com a maioria dos arquivos CSS do tema da interface do usuário do jQuery carregados duas vezes. 

    [!code-csharp[Main](bundling-and-minification/samples/sample12.cs)]

  O seletor curinga "\*. css" traz cada arquivo CSS na pasta, incluindo o *conteúdo\\themes\\base\\jQuery. UI. All. css* File. O arquivo *jQuery. UI. All. css* importa outros arquivos CSS.

## <a name="bundle-caching"></a>Cache de pacote

Os grupos definem o cabeçalho HTTP expira um ano a partir do momento em que o pacote é criado. Se você navegar até uma página exibida anteriormente, o Fiddler mostrará que o IE não faz uma solicitação condicional para o grupo, ou seja, não há solicitações HTTP GET do IE para os pacotes e nenhuma resposta HTTP 304 do servidor. Você pode forçar o IE a fazer uma solicitação condicional para cada pacote com a tecla F5 (resultando em uma resposta HTTP 304 para cada pacote). Você pode forçar uma atualização completa usando ^ F5 (resultando em uma resposta HTTP 200 para cada pacote).

A imagem a seguir mostra a guia **cache** do painel resposta do Fiddler:

![imagem de cache Fiddler](bundling-and-minification/_static/image8.png)

A solicitação   
`http://localhost/MvcBM_time/bundles/AllMyScripts?v=r0sLDicvP58AIXN_mc3QdyVvVj5euZNzdsa2N1PKvb81`  
 é para o pacote **AllMyScripts** e contém um par de cadeia de caracteres de consulta **v = R0sLDicvP58AIXN\\\_mc3QdyVvVj5euZNzdsa2N1PKvb81**. A cadeia de caracteres de consulta **v** tem um token de valor que é um identificador exclusivo usado para Caching. Desde que o grupo não seja alterado, o aplicativo ASP.NET solicitará o pacote **AllMyScripts** usando esse token. Se qualquer arquivo no pacote for alterado, a estrutura de otimização do ASP.NET gerará um novo token, garantindo que as solicitações do navegador para o pacote obtenham o pacote mais recente.

Se você executar as ferramentas de desenvolvedor do IE9 F12 e navegar até uma página carregada anteriormente, o IE mostrará incorretamente as solicitações GET condicionais feitas para cada pacote e o servidor que retorna HTTP 304. Você pode ler por que IE9 tem problemas para determinar se uma solicitação condicional foi feita na entrada de blog [usando CDNs e expirar para melhorar o desempenho do site](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

## <a name="less-coffeescript-scss-sass-bundling"></a>MENOS, CoffeeScript, SCSS, com agrupamento de Sass.

A estrutura de agrupamento e minificação fornece um mecanismo para processar linguagens intermediárias, como [SCSS](http://sass-lang.com/), [Sass](http://sass-lang.com/), [less](http://www.dotlesscss.org/) ou [CoffeeScript](http://coffeescript.org/), e aplicar transformações como minificação ao pacote resultante. Por exemplo, para adicionar arquivos [. Less](http://www.dotlesscss.org/) a seu projeto MVC 4:

1. Crie uma pasta para o conteúdo menos. O exemplo a seguir usa o *conteúdo\\pasta myless* .
2. Adicione o [. menos](http://www.dotlesscss.org/) NuGet com baixo e sem **ping** ao seu projeto.  
    ![](bundling-and-minification/_static/image9.png) de instalação sem ponto de NuGet
3. Adicione uma classe que implemente a interface [IBundleTransform](https://msdn.microsoft.com/library/system.web.optimization.ibundletransform(VS.110).aspx) . Para a transformação. Less, adicione o código a seguir ao seu projeto.

    [!code-csharp[Main](bundling-and-minification/samples/sample13.cs)]
4. Crie um pacote de menos arquivos com o `LessTransform` e a transformação [CssMinify](https://msdn.microsoft.com/library/system.web.optimization.cssminify(VS.110).aspx) . Adicione o seguinte código ao método `RegisterBundles` no *aplicativo\\_Start\\arquivo BundleConfig.cs* .

    [!code-csharp[Main](bundling-and-minification/samples/sample14.cs)]
5. Adicione o código a seguir a qualquer exibição que faça referência ao pacote menos.

    [!code-cshtml[Main](bundling-and-minification/samples/sample15.cshtml)]

## <a name="bundle-considerations"></a>Considerações sobre o pacote

Uma boa Convenção a ser seguida ao criar pacotes é incluir "Bundle" como um prefixo no nome do pacote. Isso impedirá um possível [conflito de roteamento](https://forums.asp.net/post/5012037.aspx).

Depois de atualizar um arquivo em um pacote, um novo token é gerado para o parâmetro de cadeia de caracteres de consulta de pacote e o grupo completo deve ser baixado da próxima vez que um cliente solicitar uma página que contém o pacote. Na marcação tradicional em que cada ativo é listado individualmente, somente o arquivo alterado será baixado. Os ativos que mudam com frequência podem não ser bons candidatos para Agrupamento.

O agrupamento e o minificação melhoram principalmente o tempo de carregamento da solicitação da primeira página. Depois que uma página da Web for solicitada, o navegador armazenará em cache os ativos (JavaScript, CSS e imagens) para que o agrupamento e o minificação não forneçam nenhum aumento de desempenho ao solicitar a mesma página, ou páginas no mesmo site que solicitam os mesmos ativos. Se você não definir o cabeçalho Expires corretamente em seus ativos e não usar agrupamento e minificação, a heurística de atualização dos navegadores marcará os ativos como obsoletos após alguns dias e o navegador exigirá uma solicitação de validação para cada ativo. Nesse caso, o agrupamento e o minificação fornecem um aumento de desempenho após a primeira solicitação de página. Para obter detalhes, consulte o blog [usando CDNs e expira para melhorar o desempenho do site](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

A limitação de navegador de seis conexões simultâneas por cada nome de host pode ser atenuada com o uso de uma [CDN](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx). Como a CDN terá um nome de host diferente do seu site de hospedagem, as solicitações de ativos da CDN não serão contadas em relação ao limite de seis conexões simultâneas ao seu ambiente de hospedagem. Uma CDN também pode fornecer vantagens de cache de pacote e de cache de borda comuns.

Os grupos devem ser particionados por páginas que precisam deles. Por exemplo, o modelo MVC padrão do ASP.NET para um aplicativo de Internet cria um pacote de validação jQuery separado do jQuery. Como as exibições padrão criadas não têm nenhuma entrada e não postam valores, elas não incluem o pacote de validação.

O namespace `System.Web.Optimization` é implementado em *System. Web. Optimization. dll*. Ele aproveita a biblioteca webgraxa (*webgraxa. dll*) para recursos minificação, que por sua vez usa *Antlr3. Runtime. dll*.

*Eu uso o Twitter para fazer postagens rápidas e links de compartilhamento. Meu identificador do Twitter é*: [@RickAndMSFT](http://twitter.com/RickAndMSFT)

## <a name="additional-resources"></a>Recursos adicionais

- Vídeo:[agrupamento e otimização](https://channel9.msdn.com/Events/aspConf/aspConf/Bundling-and-Optimizing) de [Howard Dierking](https://twitter.com/#!/howard_dierking)
- [Adicionar otimização da Web a um site de páginas da Web](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx).
- [Adicionando agrupamento e minificação a Web Forms](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx).
- [Implicações de desempenho de agrupamento e minificação na navegação na Web](https://blogs.msdn.com/b/henrikn/archive/2012/06/17/performance-implications-of-bundling-and-minification-on-http.aspx) por [Henrik F Nielsen](http://en.wikipedia.org/wiki/Henrik_Frystyk_Nielsen) [@frystyk](https://twitter.com/frystyk)
- [Usando CDNs e expira para melhorar o desempenho do site](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx) de Rick Anderson [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)
- [Minimizar RTT (horas de ida e volta)](https://developers.google.com/speed/docs/best-practices/rtt)

## <a name="contributors"></a>Colaboradores

- Hao Kung
- [Howard Dierking](https://twitter.com/#!/howard_dierking)
- Diana LaRose
