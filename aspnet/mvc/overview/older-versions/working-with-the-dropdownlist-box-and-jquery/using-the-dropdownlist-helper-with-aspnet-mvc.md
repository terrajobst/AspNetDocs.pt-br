---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
title: Usando o auxiliar DropDownList com o ASP.NET MVC | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/12/2012
ms.assetid: 53767e05-c8ab-42e1-a94b-22d906195200
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 6375bb2be158cea18309ffa71c71ac3e67bc91ed
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457863"
---
# <a name="using-the-dropdownlist-helper-with-aspnet-mvc"></a>Usar o auxiliar do DropDownList com ASP.NET MVC

por [Rick Anderson](https://twitter.com/RickAndMSFT)

Este tutorial ensinará as noções básicas de trabalhar com o auxiliar [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) e o auxiliar [ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) em um aplicativo Web ASP.NET MVC. Você pode usar o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é uma versão gratuita do Microsoft Visual Studio para seguir o tutorial. Antes de começar, verifique se você instalou os pré-requisitos listados abaixo. Você pode instalar todos eles clicando no seguinte link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, você pode instalar os pré-requisitos individualmente usando os seguintes links:

- [Pré-requisitos do Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)<a id="post"></a>
- [Atualização de ferramentas do ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(suporte + ferramentas de tempo de execução)

Se você estiver usando o Visual Studio 2010 em vez do Visual Web Developer 2010, instale os pré-requisitos clicando no seguinte link: [pré-requisitos do Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack). Este tutorial pressupõe que você concluiu o tutorial de [introdução ao ASP.NET MVC](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou a[loja de músicas do ASP.NET MVC](../mvc-music-store/mvc-music-store-part-1.md) ou está familiarizado com o desenvolvimento do ASP.NET MVC. Este tutorial começa com um projeto modificado do tutorial da [loja de músicas do ASP.NET MVC](../mvc-music-store/mvc-music-store-part-1.md) . Você pode baixar o projeto inicial com o link a seguir para [baixar C# a versão](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829).

Um projeto do Visual Web Developer com o C# código-fonte completo do tutorial está disponível para acompanhar este tópico. [Baixar](https://code.msdn.microsoft.com/Using-the-DropDownList-67f9367d).

### <a name="what-youll-build"></a>O que você vai construir

Você criará métodos de ação e exibições que usam o auxiliar [DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) para selecionar uma categoria. Você também usará o **jQuery** para adicionar uma caixa de diálogo Inserir categoria que pode ser usada quando uma nova categoria (como gênero ou artista) for necessária. Veja abaixo uma captura de tela da exibição criar, mostrando links para adicionar um novo gênero e adicionar um novo artista.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image1.png)

### <a name="skills-youll-learn"></a>Qualificações que você aprenderá

Eis o que você vai aprender:

- Como usar o auxiliar [DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) para selecionar dados de categoria.
- Como adicionar uma caixa de diálogo do **jQuery** para adicionar novas categorias.

### <a name="getting-started"></a>Introdução

Comece baixando o projeto inicial com o link a seguir, [baixar](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829). No Windows Explorer, clique com o botão direito do mouse no arquivo *DDL\_Starter. zip* e selecione Propriedades. Na caixa de diálogo **Propriedades do DDL\_Starter. zip** , selecione desbloquear.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image2.png)

Clique com o botão direito do mouse no arquivo DDL\_Starter. zip e selecione **extrair tudo** para descompactar o arquivo. Abra o arquivo *StartMusicStore. sln* com o Visual Web developer 2010 Express ("Visual Web Developer" ou "VWD" para curto) ou Visual Studio 2010.

Pressione CTRL + F5 para executar o aplicativo e clique no link **testar** .

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image3.png)

Selecione o link **selecionar categoria de filme (simples)** . Um tipo de filme selecionar lista é exibido, com comédia o valor selecionado.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image4.png)

Clique com o botão direito do mouse no navegador e selecione Exibir origem. O HTML da página é exibido. O código a seguir mostra o HTML para o elemento select.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample1.html)]

Você pode ver que cada item na lista de seleção tem um valor (0 para ação, 1 para inmonitoração, 2 para comédia e 3 para ficção científica) e um nome de exibição (ação, inmonitoração, comédia e ficção científica). O código acima é HTML padrão para uma lista de seleção.

Altere a lista de seleção para inativo e pressione o botão **Enviar** . A URL no navegador é `http://localhost:2468/Home/CategoryChosen?MovieType=1` e a página é exibida **: 1**.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image5.png)

Abra o arquivo *Controllers\HomeController.cs* e examine o método `SelectCategory`.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample2.cs)]

O auxiliar [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) usado para criar uma lista de seleção HTML requer um **IEnumerable&lt;SelectListItem &gt;** , explícita ou implicitamente. Ou seja, você pode passar o **ienumerable&lt;SelectListItem &gt;** explicitamente para o auxiliar [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) ou pode adicionar o **IEnumerable&lt;SelectListItem &gt;** ao [ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) usando o mesmo nome para **SelectListItem** como a propriedade Model. Passar o **SelectListItem** implicitamente e explicitamente é abordado na próxima parte do tutorial. O código acima mostra a maneira mais simples possível de criar um **IEnumerable&lt;SelectListItem &gt;** e preenchê-lo com texto e valores. Observe que o `Comedy`[SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) tem a propriedade [Selected](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.selected.aspx) definida como **true;** isso fará com que a lista de seleção renderizada mostre **comédia** como o item selecionado na lista.

A **IEnumerable&lt;SelectListItem &gt;** criada acima é adicionada à [ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) com o nome movietype. É assim que passamos o **IEnumerable&lt;SelectListItem &gt;** implicitamente para o auxiliar [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) mostrado abaixo.

Abra o arquivo *Views\Home\SelectCategory.cshtml* e examine a marcação.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample3.cshtml)]

Na terceira linha, definimos o layout como views/Shared/\_Simple\_layout. cshtml, que é uma versão simplificada do arquivo de layout padrão. Fazemos isso para manter a exibição e o HTML renderizados simples.

Neste exemplo, não estamos alterando o estado do aplicativo, portanto, enviaremos os dados usando um **http Get**, não **http post**. Consulte a lista de [verificação rápida da seção W3C para escolher http Get ou post](http://www.w3.org/2001/tag/doc/whenToUseGet.html#checklist). Como não estamos alterando o aplicativo e postando o formulário, usamos a sobrecarga [HTML. BeginForm](https://msdn.microsoft.com/library/dd460344.aspx) que nos permite especificar o método de ação, o controlador e o método de formulário (**http post** ou **http Get**). Normalmente, as exibições contêm a sobrecarga [HTML. BeginForm](https://msdn.microsoft.com/library/dd505244.aspx) que não usa parâmetros. A versão sem parâmetros usa como padrão a postagem dos dados do formulário na versão POST do mesmo método e controlador de ação.

A linha a seguir

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample4.cshtml)]

passa um argumento de cadeia de caracteres para o auxiliar **DropDownList** . Essa cadeia de caracteres, "Movietype", em nosso exemplo, faz duas coisas:

- Ele fornece a chave para o auxiliar **DropDownList** localizar um **IEnumerable&lt;SelectListItem &gt;** no **ViewBag**.
- Ele é vinculado a dados para o elemento de formulário Movietype. Se o método Submit for **http Get**, `MovieType` será uma cadeia de caracteres de consulta. Se o método de envio for **http post**, `MovieType` será adicionado no corpo da mensagem. A imagem a seguir mostra a cadeia de caracteres de consulta com o valor de 1.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image6.png)

O código a seguir mostra o método de `CategoryChosen` para o qual o formulário foi enviado.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample5.cs)]

Navegue de volta para a página de teste e selecione o link de **seleção de HTML** . A página HTML renderiza um elemento select semelhante à página de teste simples do ASP.NET MVC. Clique com o botão direito do mouse na janela do navegador e selecione **Exibir origem**. A marcação HTML para a lista de seleção é essencialmente idêntica. Teste a página HTML, ela funciona como o método de ação ASP.NET MVC e a exibição que testamos anteriormente.

### <a name="improving-the-movie-select-list-with-enums"></a>Aprimorando a lista de seleção de filmes com enums

Se as categorias em seu aplicativo forem corrigidas e não forem alteradas, você poderá aproveitar os enums para tornar seu código mais robusto e mais simples de estender. Quando você adiciona uma nova categoria, o valor correto da categoria é gerado. O evita erros de copiar e colar quando você adiciona uma nova categoria, mas se esquece de atualizar o valor da categoria.

Abra o arquivo *Controllers\HomeController.cs* e examine o código a seguir:

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample6.cs)]

O `eMovieCategories` de [Enumeração](https://msdn.microsoft.com/library/sbbt4032(VS.80).aspx) captura os quatro tipos de filme. O método `SetViewBagMovieType` cria os **&gt;IEnumerable&lt;SelectListItem** do `eMovieCategories`**enum**e define a propriedade `Selected` do parâmetro `selectedMovie`. O método de ação `SelectCategoryEnum` usa a mesma exibição que o método de ação `SelectCategory`.

Navegue até a página de teste e clique no link `Select Movie Category (Enum)`. Desta vez, em vez de um valor (número) sendo exibido, uma cadeia de caracteres representando a enumeração é exibida.

### <a name="posting-enum-values"></a>Postando valores de enumeração

Normalmente, os formulários HTML são usados para postar dados no servidor. O código a seguir mostra as versões `HTTP GET` e `HTTP POST` do método `SelectCategoryEnumPost`.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample7.cs)]

Ao passar um `eMovieCategories` enum para o método `POST`, podemos extrair o valor de enumeração e a cadeia de caracteres de enumeração. Execute o exemplo e navegue até a página de teste. Clique no link `Select Movie Category(Enum Post)`. Selecione um tipo de filme e, em seguida, clique no botão enviar. A exibição mostra o valor e o nome do tipo de filme.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image7.png)

### <a name="creating-a-multiple-section-select-element"></a>Criando um elemento select de várias seções

O [ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) HTML Helper renderiza o elemento HTML `<select>` com o atributo `multiple`, que permite que os usuários façam várias seleções. Navegue até o link de teste e selecione o link **país de seleção múltipla** . A interface do usuário renderizada permite que você selecione vários países. Na imagem abaixo, Canadá e China estão selecionados.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image8.png)

### <a name="examining-the-multiselectcountry-code"></a>Examinando o código MultiSelectCountry

Examine o código a seguir do arquivo *Controllers\HomeController.cs* .

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample8.cs)]

O método `GetCountries` cria uma lista de países e, em seguida, passa-o para o Construtor `MultiSelectList`. A sobrecarga de construtor de `MultiSelectList` usada no método `GetCountries` acima usa quatro parâmetros:

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample9.cs)]

1. *itens*: um [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) contendo os itens na lista. No exemplo acima, a lista de países.
2. *DataValueField*: o nome da propriedade na lista **IEnumerable** que contém o valor. No exemplo acima, a propriedade `ID`.
3. *DataTextField*: o nome da propriedade na lista **IEnumerable** que contém as informações a serem exibidas. No exemplo acima, a propriedade `name`.
4. *SelectedValues*: a lista de valores selecionados.

No exemplo acima, o método `MultiSelectCountry` passa um valor de `null` para os países selecionados e, portanto, nenhum país é selecionado quando a interface do usuário é exibida. O código a seguir mostra a marcação Razor usada para renderizar a exibição `MultiSelectCountry`.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample10.cshtml)]

O método [ListBox](https://msdn.microsoft.com/library/dd470200.aspx) de HTML Helper usado acima pega dois parâmetros, o nome da propriedade a ser vinculada ao modelo e alist de [seleção](https://msdn.microsoft.com/library/system.web.mvc.multiselectlist.aspx) seleção que contém as opções e os valores selecionados. O código de `ViewBag.YouSelected` acima é usado para exibir os valores dos países que você selecionou ao enviar o formulário. Examine a sobrecarga HTTP POST do método `MultiSelectCountry`.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample11.cs)]

O `ViewBag.YouSelected` propriedade dinâmica contém os países selecionados, obtidos para a entrada de `Countries` na coleção de formulários. Nesta versão, o método getpaíses é passado para uma lista dos países selecionados, portanto, quando a exibição de `MultiSelectCountry` é exibida, os países selecionados são selecionados na interface do usuário.

### <a name="making-a-select-element-friendly-with-the-harvest-chosen-jquery-plugin"></a>Tornando um elemento select amigável com o plugin do jQuery escolhido pela colheita

A coleta do plug-in jQuery [escolhido](https://harvesthq.github.com/chosen/) pode ser adicionada a um HTML &lt;selecione&gt; elemento para criar uma interface do usuário amigável. As imagens a seguir demonstram a coleta do plug-in jQuery [escolhido](https://harvesthq.github.com/chosen/) com `MultiSelectCountry` exibição.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image9.png)

Nas duas imagens abaixo, o **Canadá** está selecionado.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image10.png)

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image11.png)

Na imagem acima, o Canadá está selecionado e contém um **x** no qual você pode clicar para remover a seleção. A imagem abaixo mostra o Canadá, a China e o Japão selecionados.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image12.png)

### <a name="hooking-up-the-harvest-chosen-jquery-plugin"></a>Conectando a coleta do plug-in jQuery escolhido

A seção a seguir será mais fácil de seguir se você tiver alguma experiência com o jQuery. Se você nunca usou o jQuery antes, talvez queira experimentar um dos tutoriais do jQuery a seguir.

- [Como o jQuery funciona](http://docs.jquery.com/Tutorials:How_jQuery_Works) por [John Resig](http://ejohn.org/)
- [Introdução com jQuery](http://docs.jquery.com/Tutorials:Getting_Started_with_jQuery) da [Jörn Zaefferer](http://bassistance.de/)
- [Exemplos ao vivo do jQuery](http://codylindley.com/blogstuff/js/jquery/#) por [Cody Lindley](http://codylindley.com/)

O plug-in escolhido está incluído nos projetos de exemplo iniciador e concluídos que acompanham este tutorial. Para este tutorial, você só precisará usar o jQuery para conectá-lo à interface do usuário. Para usar o plug-in do jQuery escolhido para a coleta em um projeto MVC do ASP.NET, você deve:

1. Baixe o plug-in escolhido do [GitHub](https://github.com/harvesthq/chosen/). Esta etapa foi feita para você.
2. Adicione a pasta escolhida ao seu projeto MVC do ASP.NET. Adicione os ativos do plug-in escolhido que você baixou na etapa anterior para a pasta escolhida. Esta etapa foi feita para você.
3. Conecte o plug-in escolhido ao auxiliar de HTML **DropDownList** ou **ListBox** .

### <a name="hooking-up-the-chosen-plugin-to-the-multiselectcountry-view"></a>Conectar o plug-in escolhido à exibição MultiSelectCountry.

Abra o arquivo *Views\Home\MultiSelectCountry.cshtml* e adicione um parâmetro `htmlAttributes` à `Html.ListBox`. O parâmetro que você adicionará contém um nome de classe para a lista de seleção (`@class = "chzn-select"`). O código completo é mostrado abaixo:

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample12.cshtml)]

No código acima, estamos adicionando o atributo HTML e o valor do atributo `class = "chzn-select"`. O caractere \@ classe anterior não tem nada a ver com o mecanismo de exibição do Razor. `class` é uma [ C# palavra-chave](https://msdn.microsoft.com/library/x53a06bb.aspx). C#as palavras-chave não podem ser usadas como identificadores, a menos que incluam \@ como um prefixo. No exemplo acima, `@class` é um identificador válido, mas a **classe** não é porque a **classe** é uma palavra-chave.

Adicione referências para os arquivos. *jQuery. js selecionados/escolhidos* e *escolhido/escolhido. css* . O *. jQuery. js escolhido/escolhido* e implementa a funcionalidade do plug-in escolhido. O arquivo *. css escolhido/escolhido* fornece o estilo. Adicione essas referências à parte inferior do arquivo *Views\Home\MultiSelectCountry.cshtml* . O código a seguir mostra como fazer referência ao plug-in escolhido.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample13.cshtml)]

Ative o plug-in escolhido usando o nome de classe usado no código **HTML. ListBox** . No exemplo acima, o nome da classe é `chzn-select`. Adicione a seguinte linha à parte inferior do arquivo de exibição *Views\Home\MultiSelectCountry.cshtml* . Essa linha ativa o plug-in escolhido.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample14.html)]

A linha a seguir é a sintaxe para chamar a função preparada do jQuery, que seleciona o elemento DOM com o nome de classe `chzn-select`.

[!code-powershell[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample15.ps1)]

O conjunto encapsulado retornado da chamada acima, em seguida, aplica o método escolhido (`.chosen();`), que conecta o plug-in escolhido.

O código a seguir mostra o arquivo de exibição *Views\Home\MultiSelectCountry.cshtml* concluído.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample16.cshtml)]

Execute o aplicativo e navegue até a exibição `MultiSelectCountry`. Tente adicionar e excluir países. O download de exemplo fornecido também contém um método `MultiCountryVM` e uma exibição que implementa a funcionalidade MultiSelectCountry usando um modelo de exibição em vez de um **ViewBag**.

Na próxima seção, você verá como o mecanismo ASP.NET MVC scaffolding funciona com o auxiliar **DropDownList** .

> [!div class="step-by-step"]
> [Próximo](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
