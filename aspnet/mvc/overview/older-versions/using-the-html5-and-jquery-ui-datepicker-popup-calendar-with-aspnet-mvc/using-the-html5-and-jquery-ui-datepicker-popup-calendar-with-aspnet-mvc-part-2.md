---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
title: Usando o calendário de pop-up DatePicker do HTML5 e jQuery UI com o ASP.NET MVC-parte 2 | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensinará a você noções básicas de como trabalhar com modelos do editor, modelos de exibição e o calendário de pop-up do jQuery UI DatePicker em um ASP.NET MV...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 21a178de-4c5a-4211-8a9c-74ec576c0f30
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
msc.type: authoredcontent
ms.openlocfilehash: 325cc90eb6e717c47863eda6253e0d48d796386b
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77455887"
---
# <a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-2"></a>Usando o calendário de pop-up DatePicker do HTML5 e jQuery UI com o ASP.NET MVC-parte 2

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> Este tutorial ensinará a você noções básicas de como trabalhar com modelos do editor, modelos de exibição e o calendário de pop-up do jQuery UI DatePicker em um aplicativo Web ASP.NET MVC.

## <a name="adding-an-automatic-datetime-template"></a>Adicionando um modelo de DateTime automático

Na primeira parte deste tutorial, você viu como é possível adicionar atributos ao modelo para especificar explicitamente a formatação e como você pode especificar explicitamente o modelo que é usado para renderizar o modelo. Por exemplo, o atributo [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) no código a seguir especifica explicitamente a formatação para a propriedade `ReleaseDate`.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample1.cs)]

No exemplo a seguir, o atributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) , usando a enumeração `Date`, especifica que o modelo de data deve ser usado para renderizar o modelo. Se não houver nenhum modelo de data em seu projeto, o modelo de data interno será usado.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample2.cs)]

No entanto, ASP. O MVC pode executar a correspondência de tipos usando a Convenção de configuração, procurando um modelo que corresponda ao nome de um tipo. Isso permite que você crie um modelo que formata automaticamente os dados sem usar nenhum atributo ou código. Para esta parte do tutorial, você criará um modelo que é aplicado automaticamente às propriedades do modelo do tipo [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx). Você não precisará usar um atributo ou outra configuração para especificar que o modelo deve ser usado para renderizar todas as propriedades do modelo do tipo [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx).

Você também aprenderá uma maneira de personalizar a exibição de propriedades individuais ou até mesmo de campos individuais.

Para começar, vamos remover as informações de formatação existentes e exibir as datas completas no aplicativo.

Abra o arquivo *Movie.cs* e comente o atributo `DataType` na propriedade `ReleaseDate`:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample3.cs)]

Pressione CTRL+F5 para executar o aplicativo.

Observe que a propriedade `ReleaseDate` agora exibe a data e a hora, porque esse é o padrão quando nenhuma informação de formatação é fornecida.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image1.png)

### <a name="adding-css-styles-for-testing-new-templates"></a>Adicionando estilos de CSS para testar novos modelos

Antes de criar um modelo para formatar datas, você adicionará algumas regras de estilo CSS que podem ser aplicadas aos novos modelos. Isso o ajudará a verificar se a página renderizada está usando o novo modelo.

Abra o arquivo *Content\Site.cs*s e adicione as seguintes regras de CSS à parte inferior do arquivo:

[!code-css[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample4.css)]

### <a name="adding-datetime-display-templates"></a>Adicionando modelos de exibição de DateTime

Agora você pode criar o novo modelo. Na pasta *Views\Movies* , crie uma pasta *DisplayTemplates* .

Na pasta *Views\Shared* , crie uma pasta *DisplayTemplates* e uma pasta *EditorTemplates* .

Os modelos de exibição na pasta *Views\Shared\DisplayTemplates* serão usados por todos os controladores. Os modelos de exibição na pasta *Views\Movie\DisplayTemplates* serão usados somente pelo controlador de `Movie`. (Se um modelo com o mesmo nome aparecer em ambas as pastas, o modelo na pasta *Views\Movie\DisplayTemplates* — ou seja, o modelo mais específico — terá precedência para exibições retornadas pelo controlador de `Movie`.)

Em **Gerenciador de soluções**, expanda a pasta *exibições* , expanda a pasta *compartilhada* e clique com o botão direito do mouse na pasta *Views\Shared\DisplayTemplates* .

Clique em **Adicionar** e em **Exibir**. A caixa de diálogo **Adicionar exibição** é exibida.

Na caixa **nome da exibição** , digite `DateTime`. (Você deve usar esse nome para corresponder ao nome do tipo.)

Marque a caixa de seleção **criar como uma exibição parcial** . Verifique se as caixas de seleção **usar um layout ou uma página mestra** e **criar uma exibição fortemente tipada** não estão marcadas.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image2.png)

Clique em **Adicionar**. Um modelo *DateTime. cshtml* é criado no *Views\Shared\DisplayTemplates*.

A imagem a seguir mostra a pasta *views* no **Gerenciador de soluções** depois que os modelos de exibição `DateTime` e de editor são criados.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image3.png)

Abra o arquivo *Views\Shared\DisplayTemplates\DateTime.cshtml* e adicione a marcação a seguir, que usa o método [String. Format](https://msdn.microsoft.com/library/system.string.format.aspx) para formatar a propriedade como uma data sem a hora. (O formato de `{0:d}` especifica o formato de data abreviada.)

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample5.cs)]

Repita essa etapa para criar um modelo de `DateTime` na pasta *Views\Movie\DisplayTemplates* Use o código a seguir no arquivo *Views\Movie\DisplayTemplates\DateTime.cshtml* .

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample6.cs)]

A classe CSS `loud-1` faz com que a data seja exibida em texto vermelho negrito. Você adicionou a classe `loud-1` CSS da mesma forma que uma medida temporária para poder ver facilmente quando esse modelo específico está sendo usado.

O que você fez é criar e personalizar modelos que ASP.NET usarão para exibir datas. O modelo mais geral (na pasta *Views\Shared\DisplayTemplates* ) exibe uma data curta simples. O modelo que é especificamente para o controlador de `Movie` (na pasta *Views\Movies\DisplayTemplates* ) exibe uma data abreviada que também é formatada como texto vermelho em negrito.

Pressione CTRL+F5 para executar o aplicativo. O navegador renderiza a exibição de índice para o aplicativo.

A propriedade `ReleaseDate` agora exibe a data em uma fonte vermelha em negrito sem a hora. Isso ajuda a confirmar se a `DateTime` auxiliar modelado na pasta *Views\Movies\DisplayTemplates* está selecionada sobre o `DateTime` modelo do auxiliar na pasta compartilhada (*Views\Shared\DisplayTemplates*).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image4.png)

Agora renomeie o arquivo *Views\Movies\DisplayTemplates\DateTime.cshtml* como *Views\Movies\DisplayTemplates\LoudDateTime.cshtml*.

Pressione CTRL+F5 para executar o aplicativo.

Desta vez, a propriedade `ReleaseDate` exibe uma data sem a hora e sem a fonte vermelha em negrito. Isso ilustra que um modelo que tem o nome do tipo de dados (nesse caso `DateTime`) é usado automaticamente para exibir todas as propriedades do modelo desse tipo. Depois de renomear o arquivo *DateTime. cshtml* como *LoudDateTime. cshtml*, ASP.net não encontrará mais um modelo na pasta *Views\Movies\DisplayTemplates* , portanto, ele usou o modelo *DateTime. cshtml* da pasta * Views\Movies\Shared\*.

(A correspondência de modelo não diferencia maiúsculas de minúsculas, portanto, você poderia ter criado o nome do arquivo de modelo com qualquer maiúscula. Por exemplo, *DateTime. cshtml, DateTime. cshtml*e *DateTime. cshtml* corresponderia ao tipo de `DateTime`.)

Para revisar: neste ponto, o campo `ReleaseDate` está sendo exibido usando o modelo *Views\Movies\DisplayTemplates\DateTime.cshtml* , que exibe os dados usando um formato de data abreviada, mas, caso contrário, não adiciona nenhum formato especial.

### <a name="using-uihint-to-specify-a-display-template"></a>Usando UIHint para especificar um modelo de exibição

Se seu aplicativo Web tem muitos campos de `DateTime` e, por padrão, você deseja exibir todos ou a maioria deles no formato somente de data, o modelo *DateTime. cshtml* é uma boa abordagem. Mas e se você tiver algumas datas em que deseja exibir a data e a hora completas? Sem problema. Você pode criar um modelo adicional e usar o atributo [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) para especificar a formatação para a data e hora completas. Em seguida, você pode aplicar esse modelo seletivamente. Você pode usar o atributo [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) no nível do modelo ou pode especificar o modelo dentro de uma exibição. Nesta seção, você verá como usar o atributo `UIHint` para alterar seletivamente a formatação para algumas instâncias de campos de data e hora.

Abra o arquivo *Views\Movies\DisplayTemplates\LoudDateTime.cshtml* e substitua o código existente pelo seguinte:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample7.cshtml)]

Isso faz com que a data e a hora completas sejam exibidas e adiciona a classe CSS que torna o texto verde e grande.

Abra o arquivo *Movie.cs* e adicione o atributo [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) à propriedade `ReleaseDate`, conforme mostrado no exemplo a seguir:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample8.cs)]

Isso informa ao ASP.NET MVC que quando ele exibe a propriedade `ReleaseDate` (especificamente, e não apenas qualquer objeto `DateTime`), ele deve usar o modelo *LoudDateTime. cshtml* .

Pressione CTRL+F5 para executar o aplicativo.

Observe que a propriedade `ReleaseDate` agora exibe a data e a hora em uma fonte verde grande.

Retorne ao atributo `UIHint` no arquivo *Movie.cs* e comente para que o modelo *LoudDateTime. cshtml* não seja usado. Execute o aplicativo novamente. A data de lançamento não é exibida em grande e verde. Isso verifica se o modelo *Views\Shared\DisplayTemplates\DateTime.cshtml* é usado nos modos de exibição de índice e detalhes.

Como mencionado anteriormente, você também pode aplicar um modelo em uma exibição, que permite aplicar o modelo a uma instância individual de alguns dados. Abra a exibição *Views\Movies\Details.cshtml* . Adicione `"LoudDateTime"` como o segundo parâmetro da chamada [HTML. DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) para o campo `ReleaseDate`. O código completo tem esta aparência:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample9.cshtml)]

Isso especifica que o modelo de `LoudDateTime` deve ser usado para exibir a propriedade do modelo, independentemente de quais atributos são aplicados ao modelo.

Pressione CTRL+F5 para executar o aplicativo.

Verifique se a página de índice do filme está usando o modelo *Views\Shared\DisplayTemplates\DateTime.cshtml* (negrito vermelho) e se a página *Movie\Details* está usando o modelo *Views\Movies\DisplayTemplates\LoudDateTime.cshtml* (grande e verde).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image5.png)

Na próxima seção, você criará um modelo para um tipo complexo.

> [!div class="step-by-step"]
> [Anterior](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1.md)
> [Próximo](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
