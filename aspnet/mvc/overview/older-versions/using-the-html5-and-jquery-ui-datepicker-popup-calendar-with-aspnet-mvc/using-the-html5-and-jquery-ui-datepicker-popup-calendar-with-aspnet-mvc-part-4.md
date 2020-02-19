---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
title: Usando o calendário de pop-up DatePicker do HTML5 e jQuery UI com o ASP.NET MVC-parte 4 | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensinará a você noções básicas de como trabalhar com modelos do editor, modelos de exibição e o calendário de pop-up do jQuery UI DatePicker em um ASP.NET MV...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 57666c69-2b0f-423a-a61d-be49547fa585
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
msc.type: authoredcontent
ms.openlocfilehash: 583e782641efea9a9517edb31f7718b28203d756
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457486"
---
# <a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-4"></a>Usando o calendário de pop-up DatePicker do HTML5 e jQuery UI com o ASP.NET MVC-parte 4

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> Este tutorial ensinará a você noções básicas de como trabalhar com modelos do editor, modelos de exibição e o calendário de pop-up do jQuery UI DatePicker em um aplicativo Web ASP.NET MVC.

### <a name="adding-a-template-for-editing-dates"></a>Adicionando um modelo para Editar datas

Nesta seção, você criará um modelo para Editar datas que serão aplicadas quando o ASP.NET MVC exibir a interface do usuário para editar propriedades de modelo que são marcadas com a enumeração de **Data** do atributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) . O modelo só renderizará a data; a hora não será exibida. No modelo, você usará o calendário de pop-up [do jQuery UI DatePicker](http://jqueryui.com/demos/datepicker/) para fornecer uma maneira de Editar datas.

Para começar, abra o arquivo *Movie.cs* e adicione o atributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) com a enumeração **Date** à propriedade `ReleaseDate`, conforme mostrado no código a seguir:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample1.cs)]

Esse código faz com que o campo de `ReleaseDate` seja exibido sem o tempo nos modelos de exibição e nos modelos de edição. Se seu aplicativo contiver um modelo *Date. cshtml* na pasta *Views\Shared\EditorTemplates* ou na pasta *Views\Movies\EditorTemplates* , esse modelo será usado para renderizar qualquer propriedade `DateTime` durante a edição. Caso contrário, o sistema de modelagem de ASP.NET interno exibirá a propriedade como uma data.

Pressione CTRL+F5 para executar o aplicativo. Selecione um link de edição para verificar se o campo de entrada para a data de lançamento está mostrando apenas a data.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image1.png)

Em **Gerenciador de soluções**, expanda a pasta *exibições* , expanda a pasta *compartilhada* e clique com o botão direito do mouse na pasta *Views\Shared\EditorTemplates* .

Clique em **Adicionar**e em **Exibir**. A caixa de diálogo **Adicionar exibição** é exibida.

Na caixa **nome da exibição** , digite &quot;data&quot;.

Marque a caixa de seleção **criar como uma exibição parcial** . Verifique se as caixas de seleção **usar um layout ou uma página mestra** e **criar uma exibição fortemente tipada** não estão marcadas.

Clique em **Adicionar**. O modelo *Views\Shared\EditorTemplates\Date.cshtml* é criado.

Adicione o código a seguir ao modelo *Views\Shared\EditorTemplates\Date.cshtml* .

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample2.cshtml)]

A primeira linha declara que o modelo é um tipo de `DateTime`. Embora você não precise declarar o tipo de modelo em modelos de edição e exibição, é uma prática recomendada para que você obtenha a verificação de tempo de compilação do modelo que está sendo passado para a exibição. (Outro benefício é que, em seguida, você obtém o IntelliSense para o modelo na exibição no Visual Studio.) Se o tipo de modelo não for declarado, o ASP.NET MVC considerará um tipo [dinâmico](https://msdn.microsoft.com/library/dd264741.aspx) e não haverá nenhuma verificação de tipo em tempo de compilação. Se você declarar o modelo para ser um tipo de `DateTime`, ele se tornará fortemente tipado.

A segunda linha é apenas uma marcação HTML literal que exibe &quot;usando o modelo de data&quot; antes de um campo de data. Você usará essa linha temporariamente para verificar se esse modelo de data está sendo usado.

A próxima linha é um auxiliar [HTML. TextBox](https://msdn.microsoft.com/library/system.web.mvc.html.inputextensions.textbox.aspx) que renderiza um campo `input` que é uma caixa de texto. O terceiro parâmetro para o auxiliar usa um tipo anônimo para definir a classe da caixa de texto como `datefield` e o tipo como `date`. (Como `class` é um reservado no C#, você precisa usar o caractere de `@` para escapar o atributo `class` no C# analisador.)

O tipo de `date` é um tipo de entrada HTML5 que permite que os navegadores com reconhecimento de HTML5 processem um controle de calendário HTML5. Posteriormente, você adicionará um JavaScript para conectar o jQuery DatePicker ao elemento `Html.TextBox` usando a classe `datefield`.

Pressione CTRL+F5 para executar o aplicativo. Você pode verificar se a propriedade `ReleaseDate` no modo de exibição de edição está usando o modelo de edição porque o modelo exibe &quot;usando o modelo de data&quot; logo antes da caixa de entrada de texto `ReleaseDate`, conforme mostrado nesta imagem:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image2.png)

No navegador, exiba a origem da página. (Por exemplo, clique com o botão direito do mouse na página e selecione **Exibir origem**.) O exemplo a seguir mostra algumas das marcações da página, ilustrando os atributos `class` e `type` no HTML renderizado.

[!code-html[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample3.html)]

Retorne ao modelo *Views\Shared\EditorTemplates\Date.cshtml* e remova o &quot;usando o modelo de data&quot; marcação. Agora, o modelo concluído tem esta aparência:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample4.cshtml)]

### <a name="adding-a-jquery-ui-datepicker-popup-calendar-using-nuget"></a>Adicionando um calendário de pop-up do jQuery UI DatePicker usando o NuGet

Nesta seção, você adicionará o calendário de pop-up [do jQuery UI DatePicker](http://jqueryui.com/demos/datepicker/) ao modelo de edição de data. A biblioteca [da interface do usuário do jQuery](http://jqueryui.com/) fornece suporte para animação, efeitos avançados e widgets personalizáveis. Ele é criado sobre a biblioteca JavaScript do jQuery. O calendário pop-up DatePicker torna mais fácil e natural inserir datas usando um calendário em vez de inserir uma cadeia de caracteres. O calendário pop-up também limita os usuários a datas legais – a entrada de texto comum para uma data permite que você insira algo como `2/33/1999` (fevereiro de 33rd, 1999), mas o calendário de pop-up [da interface do usuário do jQuery DatePicker](http://jqueryui.com/demos/datepicker/) não permitirá isso.

Primeiro, você precisa instalar as bibliotecas da interface do usuário do jQuery. Para fazer isso, você usará o NuGet, que é um Gerenciador de pacotes incluído nas versões do SP1 do Visual Studio 2010 e do Visual Web Developer.

No Visual Web Developer, no menu **ferramentas** , selecione **Gerenciador de pacotes NuGet** e, em seguida, selecione **gerenciar pacotes NuGet**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image3.png)

Observação: se o menu **ferramentas** não exibir o comando **Gerenciador de pacotes NuGet** , você precisará instalar o NuGet seguindo as instruções na página [instalando o NuGet](http://docs.nuget.org/docs/start-here/installing-nuget) do site do NuGet.   
  
Se você estiver usando o Visual Studio em vez do Visual Web Developer, no menu **ferramentas** , selecione **Gerenciador de pacotes NuGet** e, em seguida, selecione **Adicionar referência de pacote de biblioteca**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image4.png)

Na caixa de diálogo **MVCMovie-gerenciar pacotes NuGet** , clique na guia **online** à esquerda e, em seguida, insira &quot;jQuery. UI&quot; na caixa de pesquisa. Selecione j **consultar widgets da interface do usuário: DatePicker**e, em seguida, selecione o botão **instalar** .

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image5.png)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image6.png)

O NuGet adiciona essas versões de depuração e versões reduzidos do jQuery UI Core e o seletor de data da interface do usuário do jQuery ao seu projeto:

- *jQuery. UI. Core. js*
- *jQuery. UI. Core. min. js*
- *jQuery. UI. DatePicker. js*
- *jQuery. UI. DatePicker. min. js*

Observação: as versões de depuração (os arquivos sem a extensão *. min. js* ) são úteis para depuração, mas em um site de produção, você incluiria apenas as versões reduzidos.

Para realmente usar o seletor de data do jQuery, você precisa criar um script jQuery que conectará o widget calendário ao modelo de edição. Em **Gerenciador de soluções**, clique com o botão direito do mouse na pasta *scripts* e selecione **Adicionar**, **novo item**e, em seguida, **arquivo JScript**. Nomeie o arquivo *DatePickerReady. js*.

Adicione o seguinte código ao arquivo *DatePickerReady. js* :

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample5.js)]

Se você não estiver familiarizado com o jQuery, aqui está uma breve explicação do que isso faz: a primeira linha é a função &quot;jQuery Ready&quot;, que é chamada quando todos os elementos DOM em uma página são carregados. A segunda linha seleciona todos os elementos DOM que têm o nome de classe `datefield`e, em seguida, invoca a função `datepicker` para cada um deles. (Lembre-se de que você adicionou a classe `datefield` ao modelo *Views\Shared\EditorTemplates\Date.cshtml* anteriormente no tutorial.)

Em seguida, abra o arquivo *Views\Shared\\_Layout. cshtml* . Você precisa adicionar referências aos seguintes arquivos, que são necessários para que você possa usar o seletor de data:

- *Content/themes/base/jQuery. UI. Core. css*
- *Content/themes/base/jQuery. UI. DatePicker. css*
- *Content/themes/base/jQuery. UI. Theme. css*
- *jQuery. UI. Core. min. js*
- *jQuery. UI. DatePicker. min. js*
- *DatePickerReady. js*

O exemplo a seguir mostra o código real que você deve adicionar na parte inferior do elemento `head` no arquivo *Views\Shared\\_Layout. cshtml* .

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample6.cshtml)]

A seção `head` completa é mostrada aqui:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample7.cshtml)]

O método [auxiliar de conteúdo de URL](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.content.aspx) converte o caminho de recurso em um caminho absoluto. Você deve usar `@URL.Content` para referenciar corretamente esses recursos quando o aplicativo estiver em execução no IIS.

Pressione CTRL+F5 para executar o aplicativo. Selecione um link de edição e, em seguida, coloque o ponto de inserção no campo **liberado** . O calendário de pop-up da interface do usuário do jQuery é exibido.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image7.png)

Como a maioria dos controles jQuery, o DatePicker permite personalizá-lo extensivamente. Para obter informações, consulte [personalização visual: Criando um tema da interface do usuário do jQuery](http://learn.jquery.com/jquery-ui/getting-started/#visual-customization-designing-a-jquery-ui-theme) no site [da interface do usuário do jQuery](http://learn.jquery.com/jquery-ui/getting-started/) .

### <a name="supporting-the-html5-date-input-control"></a>Suporte ao controle de entrada de data do HTML5

À medida que mais navegadores dão suporte a HTML5, você desejará usar a entrada do HTML5 nativo, como o `date` elemento de entrada e não usar o calendário da interface do usuário do jQuery. Você pode adicionar lógica ao seu aplicativo para usar automaticamente os controles HTML5 se o navegador oferecer suporte a eles. Para fazer isso, substitua o conteúdo do arquivo *DatePickerReady. js* pelo seguinte:

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample8.js)]

A primeira linha desse script usa o Modernizr para verificar se há suporte para a entrada de data do HTML5. Se não houver suporte, o seletor de data da interface do usuário do jQuery será conectado em vez disso. (O[Modernizr](http://www.modernizr.com/docs/) é uma biblioteca JavaScript de código aberto que detecta a disponibilidade de implementações nativas do HTML5 e do CSS3. O Modernizr está incluído em quaisquer novos projetos do ASP.NET MVC que você criar.)

Depois de fazer essa alteração, você pode testá-la usando um navegador que dá suporte a HTML5, como o Opera 11. Execute o aplicativo usando um navegador compatível com o HTML5 e edite uma entrada de filme. O controle de data do HTML5 é usado em vez do calendário de pop-up da interface do usuário do jQuery:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image8.png)

Como as novas versões dos navegadores estão implementando o HTML5 de forma incremental, uma boa abordagem para agora é adicionar código ao seu site que acomoda uma ampla variedade de suporte a HTML5. Por exemplo, um script *DatePickerReady. js* mais robusto é mostrado abaixo, permitindo que o site ofereça suporte a navegadores que só dão suporte parcial ao controle de data do HTML5.

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample9.js)]

Esse script seleciona elementos HTML5 `input` do tipo `date` que não dão suporte completo ao controle de data do HTML5. Para esses elementos, ele conecta o calendário de Popup da interface do usuário do jQuery e, em seguida, altera o atributo `type` de `date` para `text`. Ao alterar o atributo `type` de `date` para `text`, o suporte parcial a datas do HTML5 é eliminado. Um script *DatePickerReady. js* ainda mais robusto pode ser encontrado em [JSFIDDLE](http://jsfiddle.net/XSTK8/15/).

### <a name="adding-nullable-dates-to-the-templates"></a>Adicionando datas anuláveis aos modelos

Se você usar um dos modelos de data existentes e passar uma data nula, obterá um erro em tempo de execução. Para tornar os modelos de data mais robustos, você os alterará para manipular valores nulos. Para dar suporte a datas anuláveis, altere o código no *Views\Shared\DisplayTemplates\DateTime.cshtml* para o seguinte:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample10.cshtml)]

O código retorna uma cadeia de caracteres vazia quando o modelo é **nulo**.

Altere o código no arquivo *Views\Shared\EditorTemplates\Date.cshtml* para o seguinte:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample11.cshtml)]

Quando esse código é executado, se o modelo não for nulo, o valor de `DateTime` do modelo será usado. Se o modelo for nulo, a data atual será usada em seu lugar.

### <a name="wrapup"></a>Wrapup

Este tutorial abordou os conceitos básicos dos auxiliares do ASP.NET templateed e mostra como usar o calendário de pop-up do jQuery UI DatePicker em um aplicativo MVC do ASP.NET. Para obter mais informações, Experimente estes recursos:

- Para obter informações sobre localização, consulte blog do Rajeesh [JQueryUI DatePicker no ASP.NET MVC](http://www.rajeeshcv.com/2010/02/jqueryui-datepicker-in-asp-net-mvc/).
- Para obter informações sobre a interface do usuário do jQuery, consulte [jQuery UI](http://docs.jquery.com/UI).
- Para obter informações sobre como localizar o controle DatePicker, consulte [UI/DatePicker/Localization](http://docs.jquery.com/UI/Datepicker/Localization).
- Para obter mais informações sobre os modelos ASP.NET MVC, consulte a série de Blogs de Brad Wilson em [modelos do ASP.NET MVC 2](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). Embora a série tenha sido escrita para o ASP.NET MVC 2, o material ainda se aplica à versão atual do ASP.NET MVC.

> [!div class="step-by-step"]
> [Anterior](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
