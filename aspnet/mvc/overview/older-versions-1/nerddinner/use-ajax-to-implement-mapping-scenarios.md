---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
title: Use o AJAX para implementar cenários de mapeamento | Microsoft Docs
author: microsoft
description: A etapa 11 mostra como integrar o suporte ao mapeamento AJAX em nosso aplicativo NerdDinner, permitindo que os usuários que estão criando, editem ou exibam jantares para ver a l...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: f731990a-0a81-4d62-81df-87d676cdedd6
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
msc.type: authoredcontent
ms.openlocfilehash: 7fc90f978b9f9eca511feca70a3c0d02ec69b940
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78580111"
---
# <a name="use-ajax-to-implement-mapping-scenarios"></a>Usar o AJAX para implementar cenários de mapeamento

pela [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Esta é a etapa 11 de um [tutorial de aplicativo "NerdDinner"](introducing-the-nerddinner-tutorial.md) gratuito que percorre como criar um aplicativo Web pequeno, mas completo usando o ASP.NET MVC 1.
> 
> A etapa 11 mostra como integrar o suporte ao mapeamento AJAX em nosso aplicativo NerdDinner, permitindo que os usuários que estão criando, editem ou exibam jantares para ver o local do jantar graficamente.
> 
> Se você estiver usando o ASP.NET MVC 3, recomendamos seguir as [introdução com](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) os tutoriais da [loja de música](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) MVC 3 ou MVC.

## <a name="nerddinner-step-11-integrating-an-ajax-map"></a>Etapa 11 do NerdDinner: integrando um mapa AJAX

Agora vamos tornar nosso aplicativo um pouco mais interessante ao integrar o suporte de mapeamento AJAX. Isso permitirá que os usuários que estão criando, editem ou exibam jantares para ver o local do jantar graficamente.

### <a name="creating-a-map-partial-view"></a>Criando uma exibição parcial do mapa

Vamos usar a funcionalidade de mapeamento em vários locais dentro de nosso aplicativo. Para manter nosso código seco, encapsularemos a funcionalidade de mapa comum em um único modelo parcial que podemos usar novamente em várias ações de controlador e exibições. Vamos nomear essa exibição parcial "Map. ascx" e criá-la no diretório \Views\Dinners

Podemos criar o MAP. ascx parcial clicando com o botão direito do mouse no diretório \Views\Dinners e escolhendo o comando de menu do modo de exibição Add-&gt;. Vamos nomear o modo de exibição "Map. ascx", verificá-lo como uma exibição parcial e indicar que vamos passá-lo para uma classe de modelo "jantar" fortemente tipada:

![](use-ajax-to-implement-mapping-scenarios/_static/image1.png)

Quando clicamos no botão "Adicionar", nosso modelo parcial será criado. Em seguida, atualizaremos o arquivo Map. ascx para que ele tenha o seguinte conteúdo:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample1.aspx)]

O primeiro &lt;script&gt; pontos de referência para a biblioteca de mapeamento do Microsoft Virtual Earth 6,2. O segundo &lt;script&gt; pontos de referência para um arquivo Map. js que, em breve, criaremos, que encapsularemos nossa lógica de mapeamento comum do JavaScript. O elemento de&gt; &lt;div id = "theMap" é o contêiner HTML que o Virtual Earth usará para hospedar o mapa.

Em seguida, temos um bloco de&gt; de script inserido &lt;que contém duas funções JavaScript específicas a essa exibição. A primeira função usa o jQuery para conectar uma função que é executada quando a página está pronta para executar o script do lado do cliente. Ele chama uma função auxiliar LoadMap () que vamos definir em nosso arquivo de script Map. js para carregar o controle de mapa do Virtual Earth. A segunda função é um manipulador de eventos de retorno de chamada que adiciona um PIN ao mapa que identifica um local.

Observe como estamos usando um bloco de &lt;% =%&gt; do lado do servidor no bloco de script do lado do cliente para inserir a latitude e a longitude do jantar que desejamos mapear para o JavaScript. Essa é uma técnica útil para gerar valores dinâmicos que podem ser usados pelo script do lado do cliente (sem exigir uma chamada AJAX separada de volta ao servidor para recuperar os valores, o que o torna mais rápido). Os blocos de &lt;% =%&gt; serão executados quando a exibição for renderizada no servidor – e, portanto, a saída do HTML acabará com valores JavaScript inseridos (por exemplo: var latitude = 47,64312;).

### <a name="creating-a-mapjs-utility-library"></a>Criando uma biblioteca de utilitários do MAP. js

Agora, vamos criar o arquivo Map. js que podemos usar para encapsular a funcionalidade JavaScript para nosso mapa (e implementar os métodos LoadMap e LoadPin acima). Podemos fazer isso clicando com o botão direito do mouse no diretório \Scripts dentro de nosso projeto e, em seguida, escolhemos o comando de menu "Add-&gt;New Item", seleciono o item JScript e o nome dele como "Map. js".

Abaixo está o código JavaScript que adicionaremos ao arquivo Map. js que irá interagir com o Virtual Earth para exibir nosso mapa e adicionar Pins de locais a ele para nossos jantares:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample2.js)]

### <a name="integrating-the-map-with-create-and-edit-forms"></a>Integrando o mapa com criar e editar formulários

Agora, integraremos o suporte de mapa com nossos cenários de criação e edição existentes. A boa notícia é que isso é bem fácil e não exige a alteração de qualquer código de controlador. Como nossas exibições criar e editar compartilham uma exibição parcial "DinnerForm" comum para implementar a interface do usuário do formulário, podemos adicionar o mapa em um único local e fazer com que ambos os cenários de criação e edição o usem.

Tudo o que precisamos fazer é abrir a exibição parcial \Views\Dinners\DinnerForm.ascx e atualizá-la para incluir o novo mapa parcial. Abaixo está a aparência do DinnerForm atualizado assim que o mapa é adicionado (Observação: os elementos de formulário HTML são omitidos do trecho de código abaixo para fins de brevidade):

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample3.aspx)]

O DinnerForm partial acima usa um objeto do tipo "DinnerFormViewModel" como seu tipo de modelo (porque ele precisa de um objeto de jantar, bem como umalist de seleção para popular a DropDownList dos países). Nosso mapa parcial precisa apenas de um objeto do tipo "jantar" como seu tipo de modelo e, assim, quando renderizamos o mapa parcial, estamos passando apenas a subpropriedade do jantar de DinnerFormViewModel para ele:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample4.aspx)]

A função JavaScript que adicionamos ao partial usa o jQuery para anexar um evento "Blur" à caixa de texto HTML "address". Você provavelmente já ouviu falar de eventos "Focus" que são acionados quando um usuário clica ou Tabula em uma caixa de texto. O oposto é um evento de "desfoque" que é acionado quando um usuário sai de uma caixa de texto. O manipulador de eventos acima limpa os valores de caixa de texto latitude e longitude quando isso acontece e, em seguida, plota o novo local de endereço em nosso mapa. Um manipulador de eventos de retorno de chamada que definimos no arquivo Map. js atualizará as caixas de textlongitude e latitude em nosso formulário usando valores retornados pelo Virtual Earth com base no endereço que fornecemos.

E agora, quando executamos nosso aplicativo novamente e clicamos na guia "hospedar jantar", veremos um mapa padrão exibido junto com nossos elementos de formulário de jantar padrão:

![](use-ajax-to-implement-mapping-scenarios/_static/image2.png)

Quando digitamos um endereço e, em seguida, Tab para fora, o mapa será atualizado dinamicamente para exibir o local e nosso manipulador de eventos preencherá as caixas de texto de latitude/longitude com os valores de local:

![](use-ajax-to-implement-mapping-scenarios/_static/image3.png)

Se salvarmos o novo jantar e abri-lo novamente para edição, descobriremos que o local do mapa é exibido quando a página é carregada:

![](use-ajax-to-implement-mapping-scenarios/_static/image4.png)

Toda vez que o campo de endereço é alterado, as coordenadas de mapa e latitude/longitude serão atualizadas.

Agora que o mapa exibe o local do jantar, também podemos alterar os campos de formulário de latitude e longitude para que fiquem visíveis para os elementos ocultos (já que o mapa é atualizado automaticamente cada vez que um endereço é inserido). Para-fazer isso, vamos mudar de usar o auxiliar HTML. TextBox () para usar o método auxiliar HTML. Hidden ():

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample5.aspx)]

E agora nossos formulários são um pouco mais amigáveis e evitam a exibição da latitude/longitude bruta (enquanto ainda os armazena em cada jantar no banco de dados):

![](use-ajax-to-implement-mapping-scenarios/_static/image5.png)

### <a name="integrating-the-map-with-the-details-view"></a>Integrando o mapa com a exibição de detalhes

Agora que temos o mapa integrado aos nossos cenários de criação e edição, vamos também integrá-lo com nosso cenário de detalhes. Tudo o que precisamos fazer é chamar &lt;% HTML. RenderPartial ("Map"); %&gt; na exibição de detalhes.

Abaixo está a aparência do código-fonte para a exibição de detalhes completa (com integração de mapa):

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample6.aspx)]

E agora, quando um usuário navega para uma URL do/Dinners/Details/[ID], ele verá detalhes sobre o jantar, o local do jantar no mapa (completo com um pino que, quando focalizado, exibe o título do jantar e o endereço dele) e tem um link AJAX para o RSVP para ele:

![](use-ajax-to-implement-mapping-scenarios/_static/image6.png)

### <a name="implementing-location-search-in-our-database-and-repository"></a>Implementando a pesquisa de local em nosso banco de dados e repositório

Para concluir nossa implementação do AJAX, vamos adicionar um mapa à home page do aplicativo que permite aos usuários pesquisar graficamente os jantares próximos a eles.

![](use-ajax-to-implement-mapping-scenarios/_static/image7.png)

Começaremos implementando o suporte dentro de nossa camada de banco de dados e de repositório de data para executar com eficiência uma pesquisa RADIUS baseada em local para jantares. Poderíamos usar os novos [recursos geoespaciais do SQL 2008](https://www.microsoft.com/sqlserver/2008/en/us/spatial-data.aspx) para implementar isso ou, como alternativa, podemos usar uma abordagem de função SQL que Carlos Dryden discutida no artigo aqui: [http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx](http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx) e roubar publicado cônicas sobre como usar com LINQ to SQL aqui: [http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/](http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/)

Para implementar essa técnica, vamos abrir o "Gerenciador de Servidores" no Visual Studio, selecionar o banco de dados NerdDinner e clicar com o botão direito do mouse no subnó "functions" sob ele e optar por criar uma nova "função de valor escalar":

![](use-ajax-to-implement-mapping-scenarios/_static/image8.png)

Em seguida, colaremos a seguinte função DistanceBetween:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample7.sql)]

Em seguida, criaremos uma nova função com valor de tabela em SQL Server que chamaremos de "NearestDinners":

![](use-ajax-to-implement-mapping-scenarios/_static/image9.png)

Essa função de tabela "NearestDinners" usa a função auxiliar DistanceBetween para retornar todos os jantares dentro de 100 milhas da latitude e longitude que fornecemos:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample8.sql)]

Para chamar essa função, primeiro abriremos o LINQ to SQL Designer clicando duas vezes no arquivo NerdDinner. dbml em nosso diretório \Models:

![](use-ajax-to-implement-mapping-scenarios/_static/image10.png)

Em seguida, arrastaremos as funções NearestDinners e DistanceBetween para o designer de LINQ to SQL, que fará com que elas sejam adicionadas como métodos em nossa classe NerdDinnerDataContext de LINQ to SQL:

![](use-ajax-to-implement-mapping-scenarios/_static/image11.png)

Em seguida, podemos expor um método de consulta "FindByLocation" em nossa classe DinnerRepository que usa a função NearestDinner para retornar JANTARS próximos que estão dentro de 100 milhas do local especificado:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample9.cs)]

### <a name="implementing-a-json-based-ajax-search-action-method"></a>Implementando um método de ação de pesquisa AJAX baseado em JSON

Agora, implementaremos um método de ação do controlador que aproveita o novo método de repositório FindByLocation () para retornar uma lista de dados de jantar que podem ser usados para popular um mapa. Teremos esse método de ação para retornar os dados do jantar em um formato JSON (JavaScript Object Notation) para que ele possa ser facilmente manipulado usando JavaScript no cliente.

Para implementar isso, criaremos uma nova classe "SearchController" clicando com o botão direito do mouse no diretório \Controllers e escolhendo o comando de menu do controlador Add-&gt;. Em seguida, implementaremos um método de ação "SearchByLocation" na nova classe SearchController, como abaixo:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample10.cs)]

O método de ação SearchByLocation da SearchController chama internamente o método FindByLocation no DinnerRepository para obter uma lista de jantares próximos. Em vez de retornar os objetos do jantar diretamente para o cliente, no entanto, ele retorna os objetos JsonDinner. A classe JsonDinner expõe um subconjunto de propriedades de jantar (por exemplo: por motivos de segurança, ela não divulga os nomes das pessoas que têm o RSVP para um jantar). Ele também inclui uma propriedade RSVPCount que não existe no jantar – e que é calculada dinamicamente contando o número de objetos RSVP associados a um jantar específico.

Em seguida, estamos usando o método auxiliar JSON () na classe base do controlador para retornar a sequência de jantares usando um formato de conexão baseado em JSON. JSON é um formato de texto padrão para representar estruturas de dados simples. Abaixo está um exemplo de como uma lista formatada em JSON de dois objetos JsonDinner é parecida quando retornada pelo nosso método de ação:

[!code-json[Main](use-ajax-to-implement-mapping-scenarios/samples/sample11.json)]

### <a name="calling-the-json-based-ajax-method-using-jquery"></a>Chamando o método AJAX baseado em JSON usando o jQuery

Agora estamos prontos para atualizar a home page do aplicativo NerdDinner para usar o método de ação SearchByLocation do SearchController. Para fazer isso, vamos abrir o modelo de exibição/Views/Home/Index.aspx e atualizá-lo para ter uma caixa de texto, um botão de pesquisa, nosso mapa e um elemento &lt;div&gt; chamado jantarlist:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample12.aspx)]

Em seguida, podemos adicionar duas funções JavaScript à página:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample13.html)]

A primeira função JavaScript carrega o mapa quando a página é carregada pela primeira vez. A segunda função JavaScript conecta um manipulador de eventos de clique em JavaScript no botão de pesquisa. Quando o botão é pressionado, ele chama a função JavaScript FindDinnersGivenLocation () que adicionaremos ao nosso arquivo Map. js:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample14.js)]

Essa função FindDinnersGivenLocation () chama Map. Localize () no controle do Virtual Earth para centralizá-lo no local inserido. Quando o serviço de mapa do Virtual Earth retorna, o mapa. O método Find () invoca o método de retorno de chamada callbackUpdateMapDinners que passamos como o argumento final.

O método callbackUpdateMapDinners () é onde o trabalho real é feito. Ele usa o método auxiliar $. post () do jQuery para executar uma chamada AJAX para o método de ação SearchByLocation () de nosso SearchController, passando-o para a latitude e a longitude do mapa centralizado recentemente. Ele define uma função embutida que será chamada quando o método auxiliar $. post () for concluído, e os resultados de jantar formatados por JSON retornados do método de ação SearchByLocation () passarão por ele usando uma variável chamada "JANTARS". Em seguida, ele faz um foreach em cada jantar devolvido e usa a latitude e a longitude do jantar e outras propriedades para adicionar um novo PIN no mapa. Ele também adiciona uma entrada de jantar à lista HTML de jantares à direita do mapa. Em seguida, ele conecta um evento de foco para as anotações e a lista de HTML para que os detalhes sobre o jantar sejam exibidos quando um usuário passa sobre eles:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample15.html)]

E agora, quando executarmos o aplicativo e visitarmos a página inicial, será apresentado um mapa. Quando inserirmos o nome de uma cidade, o mapa exibirá os próximos jantares próximos a ele:

![](use-ajax-to-implement-mapping-scenarios/_static/image12.png)

Passar o mouse sobre um jantar mostrará detalhes sobre ele.

Clicar no título do jantar na bolha ou no lado direito na lista de HTML navegará até o jantar, que pode, opcionalmente, ser RSVP para:

![](use-ajax-to-implement-mapping-scenarios/_static/image13.png)

### <a name="next-step"></a>Próxima etapa

Agora implementamos toda a funcionalidade do aplicativo de nosso aplicativo NerdDinner. Agora, vamos ver como podemos habilitar o teste de unidade automatizado.

> [!div class="step-by-step"]
> [Anterior](use-ajax-to-deliver-dynamic-updates.md)
> [Próximo](enable-automated-unit-testing.md)
