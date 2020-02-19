---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: Usando o calendário de pop-up DatePicker do HTML5 e jQuery UI com o ASP.NET MVC-parte 3 | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensinará a você noções básicas de como trabalhar com modelos do editor, modelos de exibição e o calendário de pop-up do jQuery UI DatePicker em um ASP.NET MV...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: b3249397e54e64538c4dc78e5fe8b94656e8962b
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457889"
---
# <a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a>Usando o calendário de pop-up DatePicker do HTML5 e jQuery UI com o ASP.NET MVC-parte 3

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> Este tutorial ensinará a você noções básicas de como trabalhar com modelos do editor, modelos de exibição e o calendário de pop-up do jQuery UI DatePicker em um aplicativo Web ASP.NET MVC.

## <a name="working-with-complex-types"></a>Trabalhando com tipos complexos

Nesta seção, você criará uma classe de endereço e aprenderá a criar um modelo para exibi-lo.

Na pasta *modelos* , crie um novo arquivo de classe chamado *Person.cs* , onde você colocará dois tipos: uma classe `Person` e uma classe `Address`. A classe `Person` conterá uma propriedade que é digitada como `Address`. O tipo de `Address` é um tipo complexo, o que significa que não é um dos tipos internos, como `int`, `string`ou `double`. Em vez disso, ele tem várias propriedades. O código para as novas classes tem a seguinte aparência:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

No controlador de `Movie`, adicione a seguinte ação de `PersonDetail` para exibir uma instância Person:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

Em seguida, adicione o seguinte código ao controlador de `Movie` para preencher o modelo de `Person` com alguns dados de exemplo:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

Abra o arquivo *Views\Movies\PersonDetail.cshtml* e adicione a marcação a seguir para a exibição `PersonDetail`.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

Pressione CTRL + F5 para executar o aplicativo e navegue até *filmes/PersonDetail*.

A exibição `PersonDetail` não contém o `Address` tipo complexo, como você pode ver nesta captura de tela. (Nenhum endereço é mostrado.)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

Os dados do modelo de `Address` não são exibidos porque ele é um tipo complexo. Para exibir as informações de endereço, abra o arquivo *Views\Movies\PersonDetail.cshtml* novamente e adicione a marcação a seguir.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

A marcação completa para o modo de exibição de `PersonDetail` agora tem a seguinte aparência:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

Execute o aplicativo novamente e exiba a exibição `PersonDetail`. As informações de endereço agora são exibidas:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a>Criando um modelo para um tipo complexo

Nesta seção, você criará um modelo que será usado para renderizar o `Address` tipo complexo. Quando você cria um modelo para o tipo de `Address`, o ASP.NET MVC pode usá-lo automaticamente para formatar um modelo de endereço em qualquer lugar no aplicativo. Isso lhe dá uma maneira de controlar a renderização do tipo de `Address` de apenas um lugar no aplicativo.

Na pasta *Views\Shared\DisplayTemplates* , crie um modo de exibição parcial com rigidez de tipos chamado **endereço**:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

Clique em **Adicionar**e, em seguida, abra o novo arquivo *Views\Shared\DisplayTemplates\Address.cshtml* . A nova exibição contém a seguinte marcação gerada:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

Execute o aplicativo e exiba a exibição `PersonDetail`. Desta vez, o modelo de `Address` que você acabou de criar é usado para exibir o `Address` tipo complexo, portanto, a exibição é semelhante ao seguinte:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a>Resumo: maneiras de especificar o modelo e o formato de exibição do modelo

Você viu que pode especificar o formato ou o modelo para uma propriedade de modelo usando as seguintes abordagens:

- Aplicando o atributo `DisplayFormat` a uma propriedade no modelo. Por exemplo, o código a seguir faz com que a data seja exibida sem a hora:

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- Aplicar um atributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) a uma propriedade no modelo e especificar o tipo de dados. Por exemplo, o código a seguir faz com que a data seja exibida sem a hora.

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    Se o aplicativo contiver um modelo *Date. cshtml* na pasta *Views\Shared\DisplayTemplates* ou na pasta *Views\Movies\DisplayTemplates* , esse modelo será usado para renderizar a propriedade `DateTime`. Caso contrário, o sistema interno de modelagem de ASP.NET exibe a propriedade como uma data.
- Criar um modelo de exibição na pasta *Views\Shared\DisplayTemplates* ou na pasta *Views\Movies\DisplayTemplates* cujo nome corresponde ao tipo de dados que você deseja formatar. Por exemplo, você viu que o *Views\Shared\DisplayTemplates\DateTime.cshtml* foi usado para processar propriedades de `DateTime` em um modelo, sem adicionar um atributo ao modelo e sem adicionar nenhuma marcação a exibições.
- Usando o atributo [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) no modelo para especificar o modelo para exibir a propriedade do modelo.
- Adicionar explicitamente o nome do modelo de exibição à chamada [HTML. DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) em uma exibição.

A abordagem usada depende do que você precisa fazer em seu aplicativo. Não é incomum misturar essas abordagens para obter exatamente o tipo de formatação de que você precisa.

Na próxima seção, você alternará um pouco e passará da personalização de como os dados são exibidos para personalizar como eles são inseridos. Você conectará o jQuery DatePicker às exibições de edição no aplicativo para fornecer uma maneira ágil de especificar datas.

> [!div class="step-by-step"]
> [Anterior](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
> [Próximo](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)
