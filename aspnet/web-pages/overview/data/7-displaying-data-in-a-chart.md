---
uid: web-pages/overview/data/7-displaying-data-in-a-chart
title: Exibindo dados em um gráfico com Páginas da Web do ASP.NET (Razor) | Microsoft Docs
author: microsoft
description: Este capítulo explica como exibir dados em um gráfico. Nos capítulos anteriores, você aprendeu a exibir dados manualmente e em uma grade. Este capítulo explica...
ms.author: riande
ms.date: 05/22/2012
ms.assetid: f889fd46-4dac-4ecb-83d8-60e64c22036e
msc.legacyurl: /web-pages/overview/data/7-displaying-data-in-a-chart
msc.type: authoredcontent
ms.openlocfilehash: 6dad67d4e3d38d57a761c567d937d714a3184ea9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78627487"
---
# <a name="displaying-data-in-a-chart-with-aspnet-web-pages-razor"></a>Exibindo dados em um gráfico com Páginas da Web do ASP.NET (Razor)

pela [Microsoft](https://github.com/microsoft)

> Este artigo explica como usar um gráfico para exibir dados em um site Páginas da Web do ASP.NET (Razor) usando o auxiliar de `Chart`.
> 
> **O que você aprenderá**:
> 
> - Como exibir dados em um gráfico.
> - Como estilizar gráficos usando temas internos.
> - Como salvar gráficos e como armazená-los em cache para melhorar o desempenho.
> 
> Estes são os recursos de programação do ASP.NET apresentados no artigo:
> 
> - O auxiliar de `Chart`.
> 
> > [!NOTE]
> > As informações neste artigo se aplicam a Páginas da Web do ASP.NET 1,0 e páginas da Web 2.

<a id="The_Chart_Helper"></a>
## <a name="the-chart-helper"></a>O auxiliar de gráfico

Quando desejar exibir seus dados em formato gráfico, você pode usar `Chart` auxiliar. O auxiliar de `Chart` pode renderizar uma imagem que exibe dados em uma variedade de tipos de gráfico. Ele dá suporte a muitas opções de formatação e rotulagem. O auxiliar de `Chart` pode renderizar mais de 30 tipos de gráficos, incluindo todos os tipos de gráficos com os quais você pode estar familiarizado no Microsoft Excel &#8212; ou em outros gráficos de área de ferramentas, gráficos de barras, gráficos de colunas, gráficos de linhas e gráficos de pizza, juntamente com gráficos mais especializados, como gráficos de ações.

| **Gráfico de área** ![Descrição: imagem do tipo de gráfico de área](7-displaying-data-in-a-chart/_static/image1.jpg) | **Gráfico de barras** ![Descrição: imagem do tipo de gráfico de barras](7-displaying-data-in-a-chart/_static/image2.jpg) |
| --- | --- |
| **Gráfico de colunas** ![Descrição: imagem do tipo de gráfico de colunas](7-displaying-data-in-a-chart/_static/image3.jpg) | **Gráfico de linhas** ![Descrição: imagem do tipo de gráfico de linha](7-displaying-data-in-a-chart/_static/image4.jpg) |
| **Gráfico de pizza** ![Descrição: imagem do tipo de gráfico de pizza](7-displaying-data-in-a-chart/_static/image5.jpg) | **Gráfico de ações** ![Descrição: imagem do tipo de gráfico de ações](7-displaying-data-in-a-chart/_static/image6.jpg) |

### <a name="chart-elements"></a>Elementos do gráfico

Os gráficos mostram dados e elementos adicionais, como legendas, eixos, séries e assim por diante. A figura a seguir mostra muitos dos elementos do gráfico que você pode personalizar ao usar o auxiliar de `Chart`. Este artigo mostra como definir alguns (não todos) desses elementos.

![Descrição: imagem mostrando os elementos do gráfico](7-displaying-data-in-a-chart/_static/image7.jpg)

<a id="Creating_a_Chart"></a>
## <a name="creating-a-chart-from-data"></a>Criando um gráfico com base em dados

Os dados que você exibe em um gráfico podem ser de uma matriz, dos resultados retornados de um banco de dados ou de data que estão em um arquivo XML.

### <a name="using-an-array"></a>Usando uma matriz

Conforme explicado na [introdução à programação de páginas da Web do ASP.NET usando a sintaxe do Razor](https://go.microsoft.com/fwlink/?LinkId=202890), uma matriz permite que você armazene uma coleção de itens semelhantes em uma única variável. Você pode usar matrizes para conter os dados que deseja incluir em seu gráfico.

Este procedimento mostra como você pode criar um gráfico a partir de dados em matrizes, usando o tipo de gráfico padrão. Ele também mostra como exibir o gráfico dentro da página.

1. Crie um novo arquivo chamado *ChartArrayBasic. cshtml*.
2. Substitua o conteúdo existente pelo seguinte: 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample1.cshtml)]

    O código cria primeiro um novo gráfico e define sua largura e altura. Você especifica o título do gráfico usando o método `AddTitle`. Para adicionar dados, use o método `AddSeries`. Neste exemplo, você usa os parâmetros `name`, `xValue`e `yValues` do método `AddSeries`. O parâmetro `name` é exibido na legenda do gráfico. O parâmetro `xValue` contém uma matriz de dados que é exibida ao longo do eixo horizontal do gráfico. O parâmetro `yValues` contém uma matriz de dados que é usada para plotar os pontos verticais do gráfico.

    O método `Write` realmente renderiza o gráfico. Nesse caso, como você não especificou um tipo de gráfico, o auxiliar `Chart` processa seu gráfico padrão, que é um gráfico de colunas.
3. Execute a página no navegador. O navegador exibe o gráfico. 

    ![](7-displaying-data-in-a-chart/_static/image8.jpg)

### <a name="using-a-database-query-for-chart-data"></a>Usando uma consulta de banco de dados para gráficos

Se as informações que você deseja criar para o gráfico estiverem em um banco de dados, você poderá executar uma consulta de banco de dados e, em seguida, usar o dos resultados para criar o gráfico. Este procedimento mostra como ler e exibir os dados do banco de dados criado no artigo [introdução ao trabalho com um banco de dado em Sites páginas da Web do ASP.net](https://go.microsoft.com/fwlink/?LinkId=202893).

1. Adicione uma pasta de *dados de\_de aplicativo* à raiz do site se a pasta ainda não existir.
2. Na pasta *dados do\_de aplicativos* , adicione o arquivo de banco de dado chamado *SmallBakery. sdf* descrito em [introdução ao trabalho com um banco de dados no páginas da Web do ASP.net sites](https://go.microsoft.com/fwlink/?LinkId=202893).
3. Crie um novo arquivo chamado *ChartDataQuery. cshtml*.
4. Substitua o conteúdo existente pelo seguinte:   

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample2.cshtml)]

    O código abre primeiro o banco de dados SmallBakery e o atribui a uma variável chamada `db`. Essa variável representa um objeto `Database` que pode ser usado para ler e gravar no banco de dados. Em seguida, o código executa uma consulta SQL para obter o nome e o preço de cada produto. O código cria um novo gráfico e passa a consulta de banco de dados para ele chamando o método de `DataBindTable` do gráfico. Esse método usa dois parâmetros: o parâmetro `dataSource` é para os dados da consulta e o parâmetro `xField` permite que você defina qual coluna de dados é usada para o eixo x do gráfico.

    Como alternativa ao uso do método `DataBindTable`, você pode usar o método `AddSeries` do auxiliar de `Chart`. O método `AddSeries` permite que você defina os parâmetros de `xValue` e `yValues`. Por exemplo, em vez de usar o método `DataBindTable` como este:

    [!code-css[Main](7-displaying-data-in-a-chart/samples/sample3.css)]

    Você pode usar o método `AddSeries` como este:

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample4.html)]

    Ambos renderizam os mesmos resultados. O método `AddSeries` é mais flexível porque você pode especificar o tipo de gráfico e os dados mais explicitamente, mas o método `DataBindTable` é mais fácil de usar se você não precisar da flexibilidade extra.
5. Execute a página em um navegador. 

    ![](7-displaying-data-in-a-chart/_static/image9.jpg)

### <a name="using-xml-data"></a>Usando dados XML

A terceira opção para o gráfico é usar um arquivo XML como os dados do gráfico. Isso requer que o arquivo XML também tenha um arquivo de esquema (arquivo *. xsd* ) que descreva a estrutura XML. Este procedimento mostra como ler dados de um arquivo XML.

1. Na pasta *dados do\_de aplicativos* , crie um novo arquivo XML chamado *Data. xml*.
2. Substitua o XML existente pelo seguinte, que são alguns dados XML sobre os funcionários de uma empresa fictícia. 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample5.xml)]
3. Na pasta *dados do\_de aplicativos* , crie um novo arquivo XML chamado *Data. xsd*. (Observe que a extensão desta vez é *. xsd*.)
4. Substitua o XML existente pelo seguinte: 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample6.xml)]
5. Na raiz do site, crie um novo arquivo chamado *ChartDataXML. cshtml*.
6. Substitua o conteúdo existente pelo seguinte: 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample7.cshtml)]

    O código cria primeiro um objeto `DataSet`. Esse objeto é usado para gerenciar os dados lidos a partir do arquivo XML e organizá-los de acordo com as informações no arquivo de esquema. (Observe que a parte superior do código inclui a instrução `using SystemData`. Isso é necessário para poder trabalhar com o objeto `DataSet`. Para obter mais informações, consulte [&quot;usando instruções&quot; e nomes totalmente qualificados](#SB_UsingStatements) posteriormente neste artigo.)

    Em seguida, o código cria um objeto `DataView` com base no conjunto de código. A exibição de dados fornece um objeto que o gráfico pode associar &#8212; a isto é, ler e plotar. O gráfico é associado aos dados usando o método `AddSeries`, como vimos anteriormente ao definir o gráfico dos dados da matriz, exceto que, dessa vez, os parâmetros `xValue` e `yValues` são definidos como o objeto `DataView`.

    Este exemplo também mostra como especificar um determinado tipo de gráfico. Quando os dados são adicionados ao método `AddSeries`, o parâmetro `chartType` também é definido para exibir um gráfico de pizza.
7. Execute a página em um navegador. 

    ![](7-displaying-data-in-a-chart/_static/image10.jpg)

> [!TIP]
> 
> <a id="SB_UsingStatements"></a>
> ### <a name="using-statements-and-fully-qualified-names"></a>Instruções "using" e nomes totalmente qualificados
> 
> Os .NET Framework que Páginas da Web do ASP.NET com sintaxe Razor baseiam-se em muitos milhares de componentes (classes). Para torná-lo gerenciável para funcionar com todas essas classes, elas são organizadas em *namespaces*, que são, de certa forma, bibliotecas. Por exemplo, o namespace `System.Web` contém classes que dão suporte à comunicação de navegador/servidor, o namespace `System.Xml` contém classes que são usadas para criar e ler arquivos XML, e o namespace `System.Data` contém classes que permitem trabalhar com dados.
> 
> Para acessar qualquer classe específica na .NET Framework, o código precisa saber não apenas o nome da classe, mas também o namespace em que a classe está. Por exemplo, para usar o auxiliar de `Chart`, o código precisa localizar a classe `System.Web.Helpers.Chart`, que combina o namespace (`System.Web.Helpers`) com o nome da classe (`Chart`). Isso é conhecido como nome &#8212; *totalmente qualificado* da classe seu local completo e não ambíguo na grande parte do .NET Framework. No código, isso seria semelhante ao seguinte:
> 
> `var myChart = new System.Web.Helpers.Chart(width: 600, height: 400) // etc.`
> 
> No entanto, é trabalhoso (e propenso a erros) ter que usar esses nomes longos e totalmente qualificados toda vez que você quiser se referir a uma classe ou um auxiliar. Portanto, para facilitar o uso de nomes de classe, você pode *importar* os namespaces dos quais está interessado, o que geralmente é apenas um pouco entre os vários namespaces no .NET Framework. Se você tiver importado um namespace, poderá usar apenas um nome de classe (`Chart`) em vez do nome totalmente qualificado (`System.Web.Helpers.Chart`). Quando seu código é executado e encontra um nome de classe, ele pode examinar apenas os namespaces que você importou para encontrar essa classe.
> 
> Quando você usa Páginas da Web do ASP.NET com sintaxe Razor para criar páginas da Web, normalmente usa o mesmo conjunto de classes a cada vez, incluindo a classe `WebPage`, os vários auxiliares e assim por diante. Para poupar o trabalho de importar os namespaces relevantes sempre que você criar um site, o ASP.NET será configurado para que ele importe automaticamente um conjunto de namespaces de núcleo para cada site. É por isso que você não teve de lidar com namespaces nem importar até agora; todas as classes com as quais você trabalhou estão em namespaces que já foram importados para você.
> 
> No entanto, às vezes você precisa trabalhar com uma classe que não está em um namespace que é importado automaticamente para você. Nesse caso, você pode usar o nome totalmente qualificado dessa classe ou pode importar manualmente o namespace que contém a classe. Para importar um namespace, use a instrução `using` (`import` em Visual Basic), como vimos em um exemplo anterior ao artigo.
> 
> Por exemplo, a classe `DataSet` está no namespace `System.Data`. O namespace `System.Data` não está disponível automaticamente para as páginas Razor do ASP.NET. Portanto, para trabalhar com a classe `DataSet` usando seu nome totalmente qualificado, você pode usar um código como este:
> 
> `var dataSet = new System.Data.DataSet();`
> 
> Se você precisar usar a classe `DataSet` repetidamente, poderá importar um namespace como este e, em seguida, usar apenas o nome da classe no código:
> 
> [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample8.cshtml)]
> 
> Você pode adicionar `using` instruções para quaisquer outros namespaces de .NET Framework que você deseja referenciar. No entanto, como observado, você não precisará fazer isso com frequência, pois a maioria das classes com as quais você trabalhará está em namespaces que são importados automaticamente pelo ASP.NET para uso nas páginas *. cshtml* e *. vbhtml* .

<a id="Displaying_Charts"></a>
## <a name="displaying-charts-inside-a-web-page"></a>Exibindo gráficos dentro de uma página da Web

Nos exemplos que você viu até agora, você cria um gráfico e, em seguida, o gráfico é renderizado diretamente para o navegador como um gráfico. Em muitos casos, no entanto, você deseja exibir um gráfico como parte de uma página, não apenas por si só no navegador. Fazer isso requer um processo de duas etapas. A primeira etapa é criar uma página que gere o gráfico, como você já viu.

A segunda etapa é exibir a imagem resultante em outra página. Para exibir a imagem, você usa um elemento HTML `<img>`, da mesma maneira que faria para exibir qualquer imagem. No entanto, em vez de fazer referência a um arquivo *. jpg* ou *. png* , o elemento `<img>` referencia o arquivo *. cshtml* que contém o auxiliar de `Chart` que cria o gráfico. Quando a página de exibição é executada, o elemento `<img>` Obtém a saída do auxiliar de `Chart` e renderiza o gráfico.

![](7-displaying-data-in-a-chart/_static/image11.jpg)

1. Crie um arquivo chamado de *defaultchart. cshtml*.
2. Substitua o conteúdo existente pelo seguinte: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample9.html)]

    O código usa o elemento `<img>` para exibir o gráfico que você criou anteriormente no arquivo *ChartArrayBasic. cshtml* .
3. Execute a página da Web em um navegador. O arquivo *. cshtml do gráfico* exibe a imagem do gráfico com base no código contido no arquivo *ChartArrayBasic. cshtml* .

<a id="Styling_a_Chart"></a>
## <a name="styling-a-chart"></a>Estilizando um gráfico

O auxiliar de `Chart` dá suporte a um grande número de opções que permitem personalizar a aparência do gráfico. Você pode definir cores, fontes, bordas e assim por diante. Uma maneira fácil de personalizar a aparência de um gráfico é usar um *tema*. Os temas são conjuntos de informações que especificam como renderizar um gráfico usando fontes, cores, rótulos, paletas, bordas e efeitos. (Observe que o estilo de um gráfico não indica o tipo de gráfico.)

A tabela a seguir lista os temas internos.

| Tema | DESCRIÇÃO |
| --- | --- |
| `Vanilla` | Exibe colunas vermelhas em um plano de fundo branco. |
| `Blue` | Exibe colunas azuis em um plano de fundo de gradiente azul. |
| `Green` | Exibe colunas azuis em um plano de fundo de gradiente verde. |
| `Yellow` | Exibe colunas em laranja em um plano de fundo amarelo de gradiente. |
| `Vanilla3D` | Exibe colunas vermelhas 3D em um plano de fundo branco. |

Você pode especificar o tema a ser usado ao criar um novo gráfico.

1. Crie um novo arquivo chamado *ChartStyleGreen. cshtml*.
2. Substitua o conteúdo existente na página pelo seguinte:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample10.cshtml)]

    Esse código é o mesmo que o exemplo anterior que usa o banco de dados para data, mas adiciona o parâmetro `theme` ao criar o objeto `Chart`. A seguir, é mostrado o código alterado:

    [!code-csharp[Main](7-displaying-data-in-a-chart/samples/sample11.cs)]
3. Execute a página em um navegador. Você vê os mesmos dados que antes, mas o gráfico parece mais elegante: 

    ![](7-displaying-data-in-a-chart/_static/image12.jpg)

<a id="Saving_a_Chart"></a>
## <a name="saving-a-chart"></a>Salvando um gráfico

Ao usar o auxiliar de `Chart` como você viu até agora neste artigo, o auxiliar recria o gráfico do zero sempre que ele é invocado. Se necessário, o código para o gráfico também consultará novamente o banco de dados ou lerá novamente o arquivo XML para obter o dado. Em alguns casos, fazer isso pode ser uma operação complexa, como se o banco de dados que você está consultando for grande, ou se o arquivo XML contiver muita Data. Mesmo que o gráfico não envolva muitos dados, o processo de criação dinâmica de uma imagem ocupará os recursos do servidor e, se muitas pessoas solicitarem a página ou as páginas que exibem o gráfico, poderá haver um impacto no desempenho do seu site.

Para ajudá-lo a reduzir o impacto potencial no desempenho da criação de um gráfico, você pode criar um gráfico na primeira vez que precisar dele e, em seguida, salvá-lo. Quando o gráfico for necessário novamente, em vez de gerá-lo novamente, você poderá apenas buscar a versão salva e renderizá-la.

Você pode salvar um gráfico das seguintes maneiras:

- Armazene o gráfico na memória do computador (no servidor).
- Salve o gráfico como um arquivo de imagem.
- Salve o gráfico como um arquivo XML. Essa opção permite que você modifique o gráfico antes de salvá-lo.

### <a name="caching-a-chart"></a>Caching de um gráfico

Depois de criar um gráfico, você pode armazená-lo em cache. Armazenar um gráfico em cache significa que ele não precisa ser recriado se precisar ser exibido novamente. Ao salvar um gráfico no cache, você lhe dá uma chave que deve ser exclusiva desse gráfico.

Os gráficos salvos no cache poderão ser removidos se o servidor estiver com pouca memória. Além disso, o cache será limpo se o aplicativo for reiniciado por qualquer motivo. Portanto, a maneira padrão de trabalhar com um gráfico armazenado em cache é sempre verificar primeiro se ele está disponível no cache e, se não, criar ou recriá-lo.

1. Na raiz do seu site, crie um arquivo chamado *ShowCachedChart. cshtml*.
2. Substitua o conteúdo existente pelo seguinte: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample12.html)]

    A marca `<img>` inclui um atributo `src` que aponta para o arquivo *ChartSaveToCache. cshtml* e passa uma chave para a página como uma cadeia de caracteres de consulta. A chave contém o valor &quot;myChartKey&quot;. O arquivo *ChartSaveToCache. cshtml* contém o auxiliar de `Chart` que cria o gráfico. Você criará essa página daqui a pouco.

    No final da página, há um link para uma página chamada *ClearCache. cshtml*. Essa é uma página que você também criará em breve. Você precisa do *ClearCache. cshtml* somente para o cache de teste para este exemplo — não é um link ou uma página que você normalmente incluiria ao trabalhar com gráficos em cache.
3. Na raiz do seu site, crie um novo arquivo chamado *ChartSaveToCache. cshtml*.
4. Substitua o conteúdo existente pelo seguinte:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample13.cshtml)]

    O código verifica primeiro se algo foi passado como o valor da chave na cadeia de caracteres de consulta. Nesse caso, o código tenta ler um gráfico fora do cache chamando o método `GetFromCache` e passando-o para a chave. Se acontece que não há nada no cache sob essa chave (o que aconteceria na primeira vez que o gráfico é solicitado), o código cria o gráfico como de costume. Quando o gráfico é concluído, o código o salva no cache chamando `SaveToCache`. Esse método requer uma chave (para que o gráfico possa ser solicitado posteriormente) e a quantidade de tempo que o gráfico deve ser salvo no cache. (O tempo exato que você armazenaria em cache de um gráfico dependerá da frequência com que você pensou que os dados representados podem mudar.) O método `SaveToCache` também requer um parâmetro &#8212; `slidingExpiration` se estiver definido como true, o contador de tempo limite será redefinido cada vez que o gráfico for acessado. Nesse caso, isso em vigor significa que a entrada de cache do gráfico expira 2 minutos após a última vez que alguém acessou o gráfico. (A alternativa à expiração deslizante é a expiração absoluta, o que significa que a entrada de cache expirará exatamente em 2 minutos depois de ser colocada no cache, independentemente da frequência com que ele foi acessado.)

    Por fim, o código usa o método `WriteFromCache` para buscar e renderizar o gráfico do cache. Observe que esse método está fora do bloco de `if` que verifica o cache, pois ele obterá o gráfico do cache, independentemente de o gráfico estar lá para começar ou ter que ser gerado e salvo no cache.

    Observe que, no exemplo, o método `AddTitle` inclui um carimbo de data/hora. (Adiciona a data e hora &#8212; atuais `DateTime.Now` &#8212; ao título.)
5. Crie uma nova página chamada *ClearCache. cshtml* e substitua seu conteúdo pelo seguinte:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample14.cshtml)]

    Esta página usa o auxiliar de `WebCache` para remover o gráfico armazenado em cache em *ChartSaveToCache. cshtml*. Conforme observado anteriormente, normalmente você não precisa ter uma página como esta. Você está criando aqui apenas para facilitar o teste de cache.
6. Execute a página da Web *ShowCachedChart. cshtml* em um navegador. A página exibe a imagem do gráfico com base no código contido no arquivo *ChartSaveToCache. cshtml* . Anote o que o carimbo de data/hora diz no título do gráfico. 

    ![Descrição: imagem do gráfico básico com carimbo de data/hora no título do gráfico](7-displaying-data-in-a-chart/_static/image13.jpg)
7. Feche o navegador.
8. Execute o *ShowCachedChart. cshtml* novamente. Observe que o carimbo de data/hora é o mesmo que antes, o que indica que o gráfico não foi regenerado, mas em vez disso, foi lido do cache.
9. Em *ShowCachedChart. cshtml*, clique no link **Limpar cache** . Isso leva você para *ClearCache. cshtml*, que relata que o cache foi limpo.
10. Clique no link **retornar ao ShowCachedChart. cshtml** ou execute novamente *ShowCachedChart. cshtml* no WebMatrix. Observe que, desta vez, o carimbo de data/hora foi alterado, pois o cache foi limpo. Portanto, o código tinha de gerar o gráfico novamente e colocá-lo de volta no cache.

### <a name="saving-a-chart-as-an-image-file"></a>Salvando um gráfico como um arquivo de imagem

Você também pode salvar um gráfico como um arquivo de imagem (por exemplo, como um arquivo *. jpg* ) no servidor. Você pode usar o arquivo de imagem da maneira como faria com qualquer imagem. A vantagem é que o arquivo é armazenado em vez de ser salvo em um cache temporário. Você pode salvar uma nova imagem de gráfico em momentos diferentes (por exemplo, a cada hora) e manter um registro permanente das alterações que ocorrem ao longo do tempo. Observe que você deve certificar-se de que seu aplicativo Web tenha permissão para salvar um arquivo na pasta no servidor em que você deseja colocar o arquivo de imagem.

1. Na raiz do seu site, crie uma pasta chamada *\_ChartFiles* , caso ela ainda não exista.
2. Na raiz do seu site, crie um novo arquivo chamado *ChartSave. cshtml*.
3. Substitua o conteúdo existente pelo seguinte:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample15.cshtml)]

    O código verifica primeiro se o arquivo *. jpg* existe chamando o método `File.Exists`. Se o arquivo não existir, o código criará um novo `Chart` de uma matriz. Desta vez, o código chama o método `Save` e passa o parâmetro `path` para especificar o caminho do arquivo e o nome do arquivo onde salvar o gráfico. No corpo da página, um elemento `<img>` usa o caminho para apontar para o arquivo *. jpg* a ser exibido.
4. Execute o arquivo *ChartSave. cshtml* .
5. Retorne ao WebMatrix. Observe que um arquivo de imagem chamado *chart01. jpg* foi salvo na pasta *\_ChartFiles* .

### <a name="saving-a-chart-as-an-xml-file"></a>Salvando um gráfico como um arquivo XML

Por fim, você pode salvar um gráfico como um arquivo XML no servidor. Uma vantagem de usar esse método sobre o armazenamento em cache do gráfico ou salvar o gráfico em um arquivo é que você pode modificar o XML antes de exibir o gráfico, se desejar. Seu aplicativo precisa ter permissões de leitura/gravação para a pasta no servidor em que você deseja colocar o arquivo de imagem.

1. Na raiz do seu site, crie um novo arquivo chamado *ChartSaveXml. cshtml*.
2. Substitua o conteúdo existente pelo seguinte:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample16.cshtml)]

    Esse código é semelhante ao código que você viu anteriormente para armazenar um gráfico no cache, exceto que ele usa um arquivo XML. O código verifica primeiro se o arquivo XML existe chamando o método `File.Exists`. Se o arquivo existir, o código criará um novo objeto `Chart` e passará o nome do arquivo como o parâmetro `themePath`. Isso cria o gráfico com base em tudo o que está no arquivo XML. Se o arquivo XML ainda não existir, o código criará um gráfico como normal e, em seguida, chamará `SaveXml` para salvá-lo. O gráfico é renderizado usando o método `Write`, como visto anteriormente.

    Assim como acontece com a página que mostrou o Caching, esse código inclui um carimbo de data/hora no título do gráfico.
3. Crie uma nova página chamada *ChartDisplayXMLChart. cshtml* e adicione a seguinte marcação a ela: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample17.html)]
4. Execute a página *ChartDisplayXMLChart. cshtml* . O gráfico é exibido. Anote o carimbo de data/hora no título do gráfico.
5. Feche o navegador.
6. No WebMatrix, clique com o botão direito do mouse na pasta *\_ChartFiles* , clique em **Atualizar**e abra a pasta. O arquivo *Xmlchart. xml* nessa pasta foi criado pelo auxiliar `Chart`. 

    ![Descrição: a pasta _ChartFiles mostrando o arquivo xmlchart. XML criado pelo auxiliar de gráfico.](7-displaying-data-in-a-chart/_static/image14.jpg)
7. Execute a página *ChartDisplayXMLChart. cshtml* novamente. O gráfico mostra o mesmo carimbo de data/hora como a primeira vez que você executou a página. Isso ocorre porque o gráfico está sendo gerado a partir do XML que você salvou anteriormente.
8. No WebMatrix, abra a pasta *\_ChartFiles* e exclua o arquivo *xmlchart. xml* .
9. Execute a página *ChartDisplayXMLChart. cshtml* mais uma vez. Desta vez, o carimbo de data/hora é atualizado, pois o auxiliar de `Chart` tinha que recriar o arquivo XML. Se desejar, verifique a pasta *\_ChartFiles* e observe que o arquivo XML está de volta.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais

- [Introdução ao trabalho com um banco de dados em sites Páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202893)
- [Usando o Caching em sites Páginas da Web do ASP.NET para melhorar o desempenho](https://go.microsoft.com/fwlink/?LinkId=202903)
- [Classe de gráfico](https://msdn.microsoft.com/library/system.web.helpers.chart(v=vs.99)) (referência de API de páginas da Web do ASP.net no MSDN)
