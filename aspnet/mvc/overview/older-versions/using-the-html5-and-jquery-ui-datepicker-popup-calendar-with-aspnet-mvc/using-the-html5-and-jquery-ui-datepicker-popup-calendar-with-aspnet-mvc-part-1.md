---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
title: Usando o calendário de pop-up DatePicker do HTML5 e jQuery UI com o ASP.NET MVC-parte 1 | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensinará a você noções básicas de como trabalhar com modelos do editor, modelos de exibição e o calendário de pop-up do jQuery UI DatePicker em um ASP.NET MV...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: c23d27f7-b0cf-44f2-8445-fb69e045c674
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
msc.type: authoredcontent
ms.openlocfilehash: c1c2380f24c72f6aabaaacaf975e95288a384ff1
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457616"
---
# <a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-1"></a>Usando o calendário de pop-up DatePicker do HTML5 e jQuery UI com o ASP.NET MVC-parte 1

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> Este tutorial ensinará a você noções básicas de como trabalhar com modelos do editor, modelos de exibição e o calendário de pop-up do jQuery UI DatePicker em um aplicativo Web ASP.NET MVC.

Este tutorial ensinará a você noções básicas de como trabalhar com modelos do editor, modelos de exibição e o [calendário de pop-up do jQuery UI DatePicker](http://plugins.jquery.com/project/datepicker) em um aplicativo Web ASP.NET MVC. Para este tutorial, você pode usar o Microsoft Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer&quot;), que é uma versão gratuita do Microsoft Visual Studio, ou você pode usar o Visual Studio 2010 SP1 se você já tiver isso.

Antes de começar, verifique se você instalou os pré-requisitos listados abaixo. Você pode instalar todos eles clicando no seguinte link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, você pode instalar o software necessário individualmente usando os seguintes links:

- [Pré-requisitos do Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [Atualização de ferramentas do ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(suporte + ferramentas de tempo de execução)

Se você estiver usando o Visual Studio 2010 em vez do Visual Web Developer, instale os pré-requisitos clicando no seguinte link: [pré-requisitos do Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).

Este tutorial pressupõe que você concluiu o tutorial [introdução com MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou que está familiarizado com o desenvolvimento MVC ASP.net. Este tutorial começa com o projeto concluído no tutorial [introdução com MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) .

Este tutorial mostra o código C#em. No entanto, o [projeto inicial](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800) e o projeto concluído também estão disponíveis no Visual Basic.

Um projeto do Visual Studio C# com o e Visual Basic código-fonte está disponível para acompanhar este tópico: [Download](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800).

### <a name="what-youll-build"></a>O que você vai construir

Você adicionará modelos (especificamente, editando e exibindo modelos) ao aplicativo de listagem de filmes simples que foi criado no tutorial [introdução com MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) . Você também adicionará um calendário de pop-up [do jQuery UI DatePicker](http://jqueryui.com/demos/datepicker/) para simplificar o processo de inserção de datas. A captura de tela a seguir mostra o aplicativo modificado com o calendário de pop-ups do jQuery UI DatePicker exibido.

![jQuery concluído](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image1.png)

### <a name="skills-youll-learn"></a>Qualificações que você aprenderá

Eis o que você vai aprender:

- Como usar atributos do namespace [Annotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) para controlar o formato dos dados quando ele é exibido e quando ele está no modo de edição.
- Como criar modelos (editar e exibir modelos) para controlar a formatação dos dados.
- Como adicionar a [interface do usuário do jQuery DatePicker](http://jqueryui.com/demos/datepicker/) como uma maneira de inserir campos de data.

### <a name="getting-started"></a>Introdução

Se você ainda não tiver o aplicativo de listagem de filmes do projeto inicial, baixe-o: 

* [Baixar](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).
* No Windows Explorer, clique com o botão direito do mouse no arquivo *MvcMovie. zip* e selecione **Propriedades**. 
* Na caixa de diálogo **Propriedades de MvcMovie. zip** , selecione **desbloquear**. (Desbloquear impede um aviso de segurança que ocorre quando você tenta usar um arquivo *.zip* que você baixou da Web).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image2.png)

Clique com o botão direito do mouse no arquivo *MvcMovie. zip* e selecione **extrair tudo** para descompactar o arquivo. No Visual Web Developer ou no Visual Studio 2010, abra o arquivo *MvcMovieCS\_tu. sln* .

Em **Gerenciador de soluções**, clique duas vezes em *Views\Shared\\_Layout. cshtml* para abri-lo. Altere o cabeçalho de `H1` do **aplicativo de filme do MVC** para o **Movie jQuery**. Pressione CTRL + F5 para executar o aplicativo e clique na guia **início** , que leva você para o método `Index` do controlador de filme. Para experimentar o aplicativo, selecione o link **Editar** e o link **detalhes** para um dos filmes. Observe que nos modos de exibição de índice, editar e detalhes, a data de lançamento e o preço são formatados adequadamente:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image3.png)

A formatação da data e do preço é o resultado do uso do atributo [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) nas propriedades da classe `Movie`.

Abra o arquivo *Movie.cs* e comente o atributo `DisplayFormat` nas propriedades `ReleaseDate` e `Price`. A classe `Movie` resultante é parecida com esta:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample1.cs)]

Pressione CTRL + F5 novamente para executar o aplicativo e selecione a guia **início** para exibir a lista de filmes. Desta vez, a data de lançamento mostra a data e hora e o campo de preço não mostra mais o símbolo de moeda. Sua alteração na classe de `Movie` desfeitou a boa formatação que você viu anteriormente, mas você corrigirá isso daqui a pouco.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image4.png)

### <a name="using-the-dataannotations-datatype-attribute-to-specify-the-data-type"></a>Usando o atributo DataType Annotations para especificar o tipo de dados

Substitua o atributo `DisplayFormat` comentado para a propriedade `ReleaseDate` com o atributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) , usando a enumeração `Date`. Substitua o atributo `DisplayFormat` da propriedade `Price` com o atributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) novamente, desta vez usando a enumeração `Currency`. É assim que o código completo é semelhante a:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample2.cs)]

Execute o aplicativo. Agora, a data de lançamento e as propriedades de preço são formatadas corretamente (ou seja, usando formatos de data e moeda apropriados). O atributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) fornece metadados de tipo para os modelos internos do ASP.NET MVC para que os campos sejam renderizados no formato correto. Usar o atributo `DataType` é preferível ao uso do atributo `DisplayFormat` que estava originalmente no código, porque o atributo `DataType` torna o modelo mais limpo e mais flexível para fins como a internacionalização.

Na próxima seção, você verá como criar modelos personalizados para exibir campos de data.

> [!div class="step-by-step"]
> [Próximo](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
