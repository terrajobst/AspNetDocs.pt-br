---
uid: mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
title: Usar controladores e exibições para implementar uma interface de usuário de listagem/detalhes | Microsoft Docs
author: microsoft
description: A etapa 4 mostra como adicionar um controlador ao aplicativo que aproveita o nosso modelo para fornecer aos usuários uma experiência de navegação de listagem de dados/detalhes...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 64116e56-1c9a-4f07-8097-bb36cbb6e57f
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
msc.type: authoredcontent
ms.openlocfilehash: 74319fe5ea4c79b50140834349e2fdf86420cfbb
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78600740"
---
# <a name="use-controllers-and-views-to-implement-a-listingdetails-ui"></a>Usar controladores e exibições para implementar uma interface do usuário de listagem/detalhes

pela [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Esta é a etapa 4 de um [tutorial de aplicativo "NerdDinner"](introducing-the-nerddinner-tutorial.md) gratuito que percorre como criar um aplicativo Web pequeno, mas completo usando o ASP.NET MVC 1.
> 
> A etapa 4 mostra como adicionar um controlador ao aplicativo que aproveita o nosso modelo para fornecer aos usuários uma experiência de navegação de listagem de dados/detalhes para jantares em nosso site NerdDinner.
> 
> Se você estiver usando o ASP.NET MVC 3, recomendamos seguir as [introdução com](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) os tutoriais da [loja de música](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) MVC 3 ou MVC.

## <a name="nerddinner-step-4-controllers-and-views"></a>Etapa 4 do NerdDinner: controladores e exibições

Com estruturas da Web tradicionais (ASP clássico, PHP, ASP.NET Web Forms, etc.), as URLs de entrada são normalmente mapeadas para arquivos no disco. Por exemplo: uma solicitação para uma URL como "/Products.aspx" ou "/Products.php" pode ser processada por um arquivo "Products. aspx" ou "Products. php".

As estruturas MVC baseadas na Web mapeiam URLs para código de servidor de forma ligeiramente diferente. Em vez de mapear URLs de entrada para arquivos, eles mapeiam URLs para métodos em classes. Essas classes são chamadas de "controladores" e são responsáveis pelo processamento de solicitações HTTP de entrada, tratamento de entrada do usuário, recuperação e salvamento de dados e determinação da resposta a ser enviada de volta ao cliente (exibir HTML, baixar um arquivo, redirecionar para outro URL, etc.).

Agora que criamos um modelo básico para nosso aplicativo NerdDinner, nossa próxima etapa será adicionar um controlador ao aplicativo que tira proveito dele para fornecer aos usuários uma experiência de navegação de listagem de dados/detalhes para jantares em nosso site.

### <a name="adding-a-dinnerscontroller-controller"></a>Adicionando um controlador DinnersController

Vamos começar clicando com o botão direito do mouse na pasta "controladores" em nosso projeto Web e, em seguida, selecionando o comando de menu do **controlador Add-&gt;** (você também pode executar esse comando digitando Ctrl-M, CTRL-C):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image1.png)

Isso abrirá a caixa de diálogo "Adicionar controlador":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image2.png)

Vamos nomear o novo controlador "DinnersController" e clicar no botão "Add" (Adicionar). O Visual Studio adicionará um arquivo DinnersController.cs em nosso diretório \Controllers:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image3.png)

Ele também abrirá a nova classe DinnersController dentro do editor de código.

### <a name="adding-index-and-details-action-methods-to-the-dinnerscontroller-class"></a>Adicionando os métodos de ação index () e Details () à classe DinnersController

Queremos permitir que os visitantes usem nosso aplicativo para procurar uma lista de jantares futuros e permitir que eles cliquem em qualquer jantar na lista para ver detalhes específicos sobre ele. Faremos isso publicando as seguintes URLs de nosso aplicativo:

| **URL** | **Finalidade** |
| --- | --- |
| */Dinners/* | Exibir uma lista HTML de futuros jantares |
| */Dinners/Details/[ID]* | Exibir detalhes sobre um jantar específico indicado por um parâmetro "ID" inserido na URL, que corresponderá ao Jantarid do jantar no banco de dados. Por exemplo:/Dinners/Details/2 exibiria uma página HTML com detalhes sobre o jantar cujo valor de Jantarid é 2. |

Publicaremos as implementações iniciais dessas URLs adicionando dois "métodos de ação" públicos à nossa classe DinnersController, como a seguir:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample1.cs)]

Em seguida, executaremos o aplicativo NerdDinner e usaremos nosso navegador para chamá-los. Digitar a URL *"/Dinners/"* fará com que o método *Index ()* seja executado e ele retornará a seguinte resposta:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image4.png)

Digitar a URL *"/Dinners/Details/2"* fará com que o método *Details ()* seja executado e envie de volta a seguinte resposta:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image5.png)

Você deve estar se perguntando – como ASP.NET MVC sabia criar nossa classe DinnersController e invocar esses métodos? Para entender isso, vamos dar uma olhada rápida em como funciona o roteamento.

### <a name="understanding-aspnet-mvc-routing"></a>Entendendo o roteamento do ASP.NET MVC

O ASP.NET MVC inclui um poderoso mecanismo de roteamento de URL que fornece muita flexibilidade no controle de como as URLs são mapeadas para as classes de controlador. Ele nos permite personalizar completamente como o ASP.NET MVC escolhe qual classe de controlador deve ser criada, qual método invocar, bem como configurar diferentes maneiras pelas quais as variáveis podem ser analisadas automaticamente a partir da URL/QueryString e passadas para o método como argumentos de parâmetro. Ele fornece a flexibilidade para otimizar totalmente um site para SEO (otimização do mecanismo de pesquisa), bem como publicar qualquer estrutura de URL que desejamos de um aplicativo.

Por padrão, novos projetos do ASP.NET MVC vêm com um conjunto pré-configurado de regras de roteamento de URL já registradas. Isso nos permite começar a usar um aplicativo com facilidade sem precisar configurar explicitamente nada. Os registros de regra de roteamento padrão podem ser encontrados na classe "aplicativo" de nossos projetos, que podemos abrir clicando duas vezes no arquivo "global. asax" na raiz do nosso projeto:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image6.png)

As regras de roteamento padrão do ASP.NET MVC são registradas no método "RegisterRoutes" dessa classe:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample2.cs)]

As "rotas. A chamada de método MapRoute () "acima registra uma regra de roteamento padrão que mapeia URLs de entrada para classes de controlador usando o formato de URL:"/{Controller}/{Action}/{ID} "– onde" Controller "é o nome da classe do controlador a ser instanciada," Action "é o nome de um método público a ser invocado, e" ID "é um parâmetro opcional inserido dentro da URL que pode ser passado como um argumento para o método. O terceiro parâmetro passado para a chamada do método "MapRoute ()" é um conjunto de valores padrão a ser usado para os valores de Controller/Action/ID no caso de eles não estarem presentes na URL (Controller = "Home", Action = "index", ID = "").

Abaixo está uma tabela que demonstra como uma variedade de URLs é mapeada usando a regra de rota "<em>/{Controllers}/{Action}/{ID}"</em>padrão:

| **URL** | **Classe de controlador** | **Método de ação** | **Parâmetros passados** |
| --- | --- | --- | --- |
| */Dinners/Details/2* | DinnersController | Detalhes (ID) | id=2 |
| */Dinners/Edit/5* | DinnersController | Editar (ID) | id=5 |
| */Dinners/Create* | DinnersController | Criar() | N/D |
| */Dinners* | DinnersController | Índice () | N/D |
| */Home* | HomeController | Índice () | N/D |
| */* | HomeController | Índice () | N/D |

As três últimas linhas mostram os valores padrão (Controller = Home, Action = index, ID = "") que estão sendo usados. Como o método "index" é registrado como o nome da ação padrão, se um não for especificado, as URLs "/Dinners" e "/Home" farão com que o método de ação index () seja invocado em suas classes de controlador. Como o controlador "Home" é registrado como o controlador padrão, se um não for especificado, a URL "/" fará com que o HomeController seja criado e o método de ação index () nele seja invocado.

Se você não gostar dessas regras de roteamento de URL padrão, a boa notícia é que elas são fáceis de alterar. basta editá-las no método RegisterRoutes acima. No entanto, para nosso aplicativo NerdDinner, não vamos alterar nenhuma das regras de roteamento de URL padrão. em vez disso, vamos usá-las como estão.

### <a name="using-the-dinnerrepository-from-our-dinnerscontroller"></a>Usando o DinnerRepository da nossa DinnersController

Agora, vamos substituir nossa implementação atual dos métodos de ação index () e Details () do DinnersController por implementações que usam nosso modelo.

Vamos usar a classe DinnerRepository criada anteriormente para implementar o comportamento. Vamos começar adicionando uma instrução "using" que faz referência ao namespace "NerdDinner. Models" e, em seguida, declare uma instância de nosso DinnerRepository como um campo em nossa classe DinnerController.

Posteriormente neste capítulo, apresentaremos o conceito de "injeção de dependência" e mostraremos outra maneira para nossos controladores obterem uma referência a um DinnerRepository que permite um melhor teste de unidade – mas, para agora, vamos criar uma instância de nosso DinnerRepository embutido como abaixo.

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample3.cs)]

Agora estamos prontos para gerar uma resposta HTML de volta usando nossos objetos de modelo de dados recuperados.

### <a name="using-views-with-our-controller"></a>Usando exibições com nosso controlador

Embora seja possível escrever código dentro de nossos métodos de ação para montar HTML e, em seguida, usar o método auxiliar *Response. Write ()* para enviá-lo de volta ao cliente, essa abordagem se torna bastante complicada rapidamente. Uma abordagem muito melhor é para que possamos executar apenas aplicativos e lógica de dados dentro de nossos métodos de ação DinnersController e, em seguida, passar os dados necessários para processar uma resposta em HTML para um modelo de "exibição" separado que seja responsável pela saída da representação HTML de ti. Como veremos daqui a pouco, um modelo de "exibição" é um arquivo de texto que normalmente contém uma combinação de marcação HTML e código de renderização incorporado.

Separar nossa lógica do controlador de nossa exibição de visualização traz vários grandes benefícios. Em particular, ele ajuda a impor uma "separação de preocupações" clara entre o código do aplicativo e o código de formatação/renderização da interface do usuário. Isso torna muito mais fácil a lógica do aplicativo de teste de unidade isoladamente da lógica de renderização da interface do usuário. Ele facilita a modificação posterior dos modelos de renderização da interface do usuário sem a necessidade de fazer alterações no código do aplicativo. E isso pode facilitar para os desenvolvedores e designers colaborarem juntos em projetos.

Podemos atualizar nossa classe DinnersController para indicar que queremos usar um modelo de exibição para enviar de volta uma resposta de interface do usuário HTML alterando as assinaturas de método dos nossos dois métodos de ação de ter um tipo de retorno de "Void" para, em vez disso, ter um tipo de retorno de "ActionResult". Em seguida, podemos chamar o método auxiliar *View ()* na classe base Controller para retornar um objeto "ViewResult" como abaixo:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample4.cs)]

A assinatura do método auxiliar *View ()* que estamos usando é semelhante ao seguinte:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image7.png)

O primeiro parâmetro para o método auxiliar *View ()* é o nome do arquivo de modelo de exibição que desejamos usar para renderizar a resposta HTML. O segundo parâmetro é um objeto de modelo que contém os dados de que o modelo de exibição precisa para processar a resposta HTML.

Dentro do nosso método de ação index (), estamos chamando o método auxiliar *View ()* e indicando que desejamos renderizar uma listagem HTML de jantares usando um modelo de exibição "index". Estamos passando o modelo de exibição uma sequência de objetos de jantar para gerar a lista:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample5.cs)]

Em nosso método de ação de detalhes (), tentamos recuperar um objeto de jantar usando a ID fornecida na URL. Se for encontrado um jantar válido, chamamos o método auxiliar *View ()* , indicando que queremos usar um modelo de exibição de "detalhes" para renderizar o objeto de jantar recuperado. Se um jantar inválido for solicitado, renderizaremos uma mensagem de erro útil que indica que o jantar não existe usando um modelo de exibição "não encontrado" (e uma versão sobrecarregada do método auxiliar *View ()* que simplesmente usa o nome do modelo):

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample6.cs)]

Agora, vamos implementar os modelos de exibição "não encontrado", "detalhes" e "índice".

### <a name="implementing-the-notfound-view-template"></a>Implementando o modelo de exibição "não encontrado"

Começaremos implementando o modelo de exibição "não encontrado" – que exibe uma mensagem de erro amigável indicando que o jantar solicitado não foi encontrado.

Vamos criar um novo modelo de exibição posicionando nosso cursor de texto dentro de um método de ação do controlador e, em seguida, clicar com o botão direito do mouse e escolher o comando de menu "Add View" (também é possível executar esse comando digitando Ctrl-M, Ctrl-V):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image8.png)

Isso abrirá uma caixa de diálogo "Adicionar exibição", como abaixo. Por padrão, a caixa de diálogo preencherá previamente o nome da exibição a ser criada para corresponder ao nome do método de ação no qual o cursor se encontrava quando a caixa de diálogo foi iniciada (neste caso, "detalhes"). Como desejamos implementar primeiro o modelo "não encontrado", vamos substituir esse nome de exibição e defini-lo como "não encontrado":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image9.png)

Quando clicamos no botão "Add" (Adicionar), o Visual Studio criará um novo modelo de exibição "não encontrado. aspx" para nós no diretório "\Views\Dinners" (que também será criado se o diretório ainda não existir):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image10.png)

Ele também abrirá nosso novo modelo de exibição "não encontrado. aspx" no editor de código:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image11.png)

Por padrão, os modelos de exibição têm duas "regiões de conteúdo", nas quais podemos adicionar conteúdo e código. O primeiro nos permite personalizar o "título" da página HTML enviada de volta. O segundo nos permite personalizar o "conteúdo principal" da página HTML enviada de volta.

Para implementar nosso modelo de exibição "não encontrado", adicionaremos um conteúdo básico:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample7.aspx)]

Em seguida, podemos experimentá-lo no navegador. Para fazer isso, vamos solicitar a URL *"/Dinners/Details/9999"* . Isso fará referência a um jantar que atualmente não exista no banco de dados e fará com que nosso método de ação DinnersController. Details () processe nosso modelo de exibição "não encontrado":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image12.png)

Uma coisa que você observará na captura de tela acima é que nosso modelo de exibição básico herdou um monte de HTML que circunda o conteúdo principal na tela. Isso ocorre porque nosso View-template está usando um modelo de "página mestra" que nos permite aplicar um layout consistente em todas as exibições no site. Vamos discutir como as páginas mestras funcionam mais em uma parte posterior deste tutorial.

### <a name="implementing-the-details-view-template"></a>Implementando o modelo de exibição "detalhes"

Agora, vamos implementar o modelo de exibição "detalhes", que irá gerar HTML para um único modelo de jantar.

Vamos fazer isso posicionando nosso cursor de texto dentro do método de ação de detalhes e, em seguida, clicar com o botão direito do mouse e escolher o comando de menu "Add View" (ou pressione CTRL-M, Ctrl-V):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image13.png)

Isso abrirá a caixa de diálogo "Adicionar exibição". Vamos manter o nome da exibição padrão ("Details"). Também selecionaremos a caixa de seleção "criar uma exibição fortemente tipada" na caixa de diálogo e selecionar (usando o menu suspenso ComboBox) o nome do tipo de modelo que estamos passando do controlador para a exibição. Para este modo de exibição, estamos passando um objeto de jantar (o nome totalmente qualificado para esse tipo é: "NerdDinner. Models. jantar"):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image14.png)

Ao contrário do modelo anterior, no qual optamos por criar uma "exibição vazia", desta vez, optaremos por "Scaffold" automaticamente o modo de exibição usando um modelo de "detalhes". Podemos indicar isso alterando a lista suspensa "exibir conteúdo" na caixa de diálogo acima.

"Scaffolding" irá gerar uma implementação inicial de nosso modelo de exibição de detalhes com base no objeto de jantar que estamos passando para ele. Isso fornece uma maneira fácil para que possamos rapidamente começar a nossa implementação de modelo de exibição.

Quando clicamos no botão "Add" (Adicionar), o Visual Studio criará um novo arquivo de modelo de exibição "details. aspx" para nós em nosso diretório "\Views\Dinners":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image15.png)

Ele também abrirá nosso novo modelo de exibição "details. aspx" no editor de código. Ele conterá uma implementação inicial de Scaffold de uma exibição de detalhes com base em um modelo de jantar. O mecanismo scaffolding usa a reflexão do .NET para examinar as propriedades públicas expostas na classe transmitida e adicionará o conteúdo apropriado com base em cada tipo que encontrar:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample8.aspx)]

Podemos solicitar a URL *"/Dinners/Details/1"* para ver o que é a implementação de Scaffold "detalhes" no navegador. Usar essa URL exibirá um dos jantares que adicionamos manualmente ao nosso banco de dados quando o criamos pela primeira vez:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image16.png)

Isso nos coloca em funcionamento rapidamente e nos fornece uma implementação inicial de nossa exibição details. aspx. Em seguida, podemos prosseguir e ajustá-lo para personalizar a interface do usuário para nossa satisfação.

Quando examinamos mais detalhadamente o modelo details. aspx, descobrimos que ele contém HTML estático, bem como código de renderização inserido. &lt;%%&gt; código Nuggets executar código quando o modelo de exibição for renderizado e &lt;% =%&gt; código Nuggets executar o código contido neles e, em seguida, renderizar o resultado para o fluxo de saída do modelo.

Podemos escrever código dentro de nossa exibição que acessa o objeto de modelo "jantar" que foi passado do nosso controlador usando uma propriedade de "modelo" fortemente tipada. O Visual Studio nos fornece o Code-IntelliSense completo ao acessar essa propriedade de "modelo" dentro do editor:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image17.png)

Vamos fazer alguns ajustes para que a origem de nosso modelo de exibição de detalhes final seja parecida com a seguinte:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample9.aspx)]

Quando Acessamos a URL *"/Dinners/Details/1"* novamente, agora ele será renderizado da seguinte maneira:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image18.png)

### <a name="implementing-the-index-view-template"></a>Implementando o modelo de exibição "index"

Agora, vamos implementar o modelo de exibição "index", que irá gerar uma lista de jantares futuros. Para fazer isso, vamos posicionar nosso cursor de texto dentro do método de ação de índice e clicar com o botão direito do mouse e escolher o comando de menu "Add View" (ou pressione CTRL-M, Ctrl-V).

Na caixa de diálogo "Adicionar exibição", manteremos o modelo de exibição chamado "index" e selecionaremos a caixa de seleção "criar uma exibição fortemente tipada". Desta vez, optaremos por gerar automaticamente um modelo de exibição de "lista" e selecionar "NerdDinner. Models. jantar" como o tipo de modelo passado para a exibição (que porque indicamos que estamos criando uma "List" Scaffold fará com que a caixa de diálogo Adicionar exibição assuma que estamos passando uma sequência de objetos de jantar de nosso controlador para a exibição):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image19.png)

Quando clicamos no botão "Add" (Adicionar), o Visual Studio criará um novo arquivo de modelo de exibição "index. aspx" para nós em nosso diretório "\Views\Dinners". Ele "Scaffold" uma implementação inicial dentro dela, que fornece uma listagem de tabela HTML dos jantares que passamos para a exibição.

Quando executamos o aplicativo e acessamos a URL *"/Dinners/"* , ele renderiza a nossa lista de jantares da seguinte forma:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image20.png)

A solução de tabela acima nos fornece um layout de grade de nossos dados de jantar, que não é exatamente o que queremos para nossa listagem de jantar voltada para o consumidor. Podemos atualizar o modelo de exibição index. aspx e modificá-lo para listar menos colunas de dados e usar um elemento &lt;UL&gt; para renderizá-los em vez de uma tabela usando o código abaixo:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample10.aspx)]

Estamos usando a palavra-chave "var" dentro da instrução foreach acima enquanto executamos um loop em cada jantar em nosso modelo. Aqueles que não conhecem C# o 3,0 podem imaginar que o uso de "var" significa que o objeto do jantar é vinculado à tarde. Em vez disso, isso significa que o compilador está usando a inferência de tipos em relação à propriedade "modelo" fortemente tipada (que é do tipo "IEnumerable&lt;jantar&gt;") e compilando a variável "jantar" local como um tipo de jantar, o que significa que obtemos o IntelliSense completo e a verificação de tempo de compilação para ele nos blocos de código:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image21.png)

Quando pressionamos a atualização na URL do */Dinners* em nosso navegador, nosso modo de exibição atualizado agora se parece com o seguinte:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image22.png)

Isso está com uma aparência melhor, mas ainda não está totalmente lá. Nossa última etapa é permitir que os usuários finais cliquem em JANTARS individuais na lista e vejam detalhes sobre eles. Implementaremos isso renderizando elementos de hiperlink HTML que são vinculados ao método de ação de detalhes em nosso DinnersController.

Podemos gerar esses hiperlinks em nossa exibição de índice de uma das duas maneiras. A primeira é criar o HTML manualmente &lt;um&gt; elementos como abaixo, onde inserimos &lt;%%&gt; blocos dentro do &lt;um elemento HTML&gt;:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image23.png)

Uma abordagem alternativa que podemos usar é aproveitar o método auxiliar interno "HTML. ActionLink ()" no ASP.NET MVC que dá suporte à criação programática de um HTML &lt;um elemento&gt; que se vincula a outro método de ação em um controlador:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample11.aspx)]

O primeiro parâmetro para o HTML. o método auxiliar ActionLink () é o texto de link a ser exibido (nesse caso, o título do jantar), o segundo parâmetro é o nome da ação do controlador para o qual desejamos gerar o link (neste caso, o método Details) e o terceiro parâmetro é um conjunto de parâmetros a serem enviados à ação (implementado como um tipo anônimo com nome/valores de propriedade). Nesse caso, estamos especificando o parâmetro "ID" do jantar para o qual desejamos vincular e, como a regra de roteamento de URL padrão no MVC ASP.NET é "{Controller}/{Action}/{id}", o método auxiliar HTML. ActionLink () gerará a seguinte saída:

[!code-html[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample12.html)]

Para nossa exibição index. aspx, usaremos a abordagem do método auxiliar HTML. ActionLink () e temos cada jantar na lista vinculada à URL de detalhes apropriada:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample13.aspx)]

E agora, quando atingirmos a URL do */Dinners* , a nossa lista de JANTARS é semelhante à seguinte:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image24.png)

Quando clicamos em qualquer um dos jantares na lista, navegamos para ver os detalhes sobre ele:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image25.png)

### <a name="convention-based-naming-and-the-views-directory-structure"></a>Nomenclatura baseada em convenção e a estrutura de diretório \Views

Por padrão, os aplicativos MVC do ASP.NET usam uma estrutura de nomenclatura de diretório baseada em Convenção ao resolver modelos de exibição. Isso permite que os desenvolvedores evitem ter que qualificar totalmente um caminho de local ao referenciar exibições de dentro de uma classe de controlador. Por padrão, o ASP.NET MVC procurará o arquivo de modelo de exibição dentro do diretório * \Views\[ControllerName]\* sob o aplicativo.

Por exemplo, trabalhamos na classe DinnersController – que referencia explicitamente três modelos de exibição: "index", "Details" e "Found". O ASP.NET MVC, por padrão, procurará essas exibições no diretório *\Views\Dinners* abaixo do diretório raiz do aplicativo:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image26.png)

Observe que, no momento, há três classes de controlador no projeto (DinnersController, HomeController e AccountController – as últimas duas foram adicionadas por padrão quando criamos o projeto), e há três subdiretórios (um para cada de domínio) no diretório \Views

As exibições referenciadas por meio de controladores domésticos e de contas resolverão automaticamente seus modelos de exibição dos respectivos diretórios *\Views\Home* e *\Views\Account* . O subdiretório *pasta \views\shared* fornece uma maneira de armazenar modelos de exibição que são reutilizados em vários controladores dentro do aplicativo. Quando o ASP.NET MVC tenta resolver um modelo de exibição, ele primeiro faz o check-in do diretório específico do *\Views\[Controller]* e, se ele não conseguir localizar o modelo de exibição, ele procurará no diretório *pasta \views\shared* .

Quando se trata de nomear modelos de exibição individuais, as diretrizes recomendadas para fazer com que o modelo de exibição Compartilhe o mesmo nome que o método de ação que fez com que ele fosse renderizado. Por exemplo, acima de nosso método de ação "index" está usando a exibição "index" para renderizar o resultado da exibição e o método de ação "Details" está usando a exibição "detalhes" para renderizar seus resultados. Isso facilita a visualização rápida do modelo associado a cada ação.

Os desenvolvedores não precisam especificar explicitamente o nome do modelo de exibição quando o modelo de exibição tem o mesmo nome que o método de ação que está sendo invocado no controlador. Em vez disso, podemos apenas passar o objeto de modelo para o método auxiliar "View ()" (sem especificar o nome da exibição) e o ASP.NET MVC inferirá automaticamente que desejamos usar o modelo de exibição *\Views\[ControllerName]\[açãoname]* no disco para renderizá-lo.

Isso nos permite limpar o código do controlador um pouco e evitar duplicar o nome duas vezes em nosso código:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample14.cs)]

O código acima é tudo o que é necessário para implementar uma experiência de listagem/detalhes de jantar interessante para o site.

#### <a name="next-step"></a>Próxima etapa

Agora temos uma boa experiência de navegação de jantar criada.

Agora, vamos habilitar o suporte à edição do formulário de dados CRUD (criar, ler, atualizar, excluir).

> [!div class="step-by-step"]
> [Anterior](build-a-model-with-business-rule-validations.md)
> [Próximo](provide-crud-create-read-update-delete-data-form-entry-support.md)
