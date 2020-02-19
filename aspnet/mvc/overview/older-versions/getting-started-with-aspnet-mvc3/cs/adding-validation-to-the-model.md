---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-validation-to-the-model
title: Adicionando validação ao modelo (C#) | Microsoft Docs
author: Rick-Anderson
description: Criando um controlador
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 9af927e2-1c3b-43d9-917d-1df75be3c482
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: 19d86dc0df931a9d135e46209559892b77626cf6
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457708"
---
# <a name="adding-validation-to-the-model-c"></a>Adicionar validação ao modelo (C#)

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Uma versão atualizada deste tutorial está disponível [aqui](../../../getting-started/introduction/getting-started.md) que usa o ASP.NET MVC 5 e o Visual Studio 2013. É mais seguro, muito mais simples de seguir e demonstra mais recursos.
> 
> 
> Este tutorial ensinará as noções básicas da criação de um aplicativo Web ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é uma versão gratuita do Microsoft Visual Studio. Antes de começar, verifique se você instalou os pré-requisitos listados abaixo. Você pode instalar todos eles clicando no seguinte link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, você pode instalar os pré-requisitos individualmente usando os seguintes links:
> 
> - [Pré-requisitos do Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Atualização de ferramentas do ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(suporte + ferramentas de tempo de execução)
> 
> Se você estiver usando o Visual Studio 2010 em vez do Visual Web Developer 2010, instale os pré-requisitos clicando no seguinte link: [pré-requisitos do Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Um projeto do Visual Web Developer com C# código-fonte está disponível para acompanhar este tópico. [Baixe a C# versão](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se preferir Visual Basic, alterne para a [versão Visual Basic](../vb/intro-to-aspnet-mvc-3.md) deste tutorial.

Nesta seção, você adicionará a lógica de validação ao modelo de `Movie` e garantirá que as regras de validação sejam impostas sempre que um usuário tentar criar ou editar um filme usando o aplicativo.

## <a name="keeping-things-dry"></a>Mantendo as coisas SECAntes

Uma das principais filosofias de design do ASP.NET MVC é seca ("Don't Repeat Yourself"). O ASP.NET MVC incentiva você a especificar a funcionalidade ou o comportamento apenas uma vez e, em seguida, fazer com que ele seja refletido em qualquer lugar em um aplicativo. Isso reduz a quantidade de código que você precisa escrever e torna o código que você escreve de forma muito mais fácil de manter.

O suporte de validação fornecido pelo ASP.NET MVC e Entity Framework Code First é um ótimo exemplo do princípio seco em ação. Você pode especificar declarativamente as regras de validação em um único local (na classe de modelo) e, em seguida, essas regras são impostas em todos os lugares do aplicativo.

Vejamos como você pode aproveitar esse suporte de validação no aplicativo de filme.

## <a name="adding-validation-rules-to-the-movie-model"></a>Adicionando regras de validação ao modelo de filme

Você começará adicionando alguma lógica de validação à classe `Movie`.

Abra o arquivo *Movie.cs*. Adicione uma instrução `using` na parte superior do arquivo que faz referência ao namespace [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) :

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

O namespace faz parte do .NET Framework. Ele fornece um conjunto interno de atributos de validação que você pode aplicar declarativamente a qualquer classe ou propriedade.

Agora, atualize a classe `Movie` para aproveitar os atributos internos de validação [`Required`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [`StringLength`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)e [`Range`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) . Use o código a seguir como um exemplo de onde aplicar os atributos.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs)]

Os atributos de validação especificam o comportamento que você deseja impor nas propriedades de modelo às quais eles são aplicados. O atributo `Required` indica que uma propriedade deve ter um valor; Neste exemplo, um filme deve ter valores para as propriedades `Title`, `ReleaseDate`, `Genre`e `Price` para ser válido. O atributo `Range` restringe um valor a um intervalo especificado. O atributo `StringLength` permite definir o tamanho máximo de uma propriedade de cadeia de caracteres e, opcionalmente, seu tamanho mínimo.

Code First garante que as regras de validação especificadas em uma classe de modelo sejam impostas antes que o aplicativo salve as alterações no banco de dados. Por exemplo, o código a seguir gerará uma exceção quando o método de `SaveChanges` for chamado, porque vários valores de propriedade `Movie` necessários estão ausentes e o preço é zero (que está fora do intervalo válido).

[!code-csharp[Main](adding-validation-to-the-model/samples/sample3.cs)]

Ter regras de validação automaticamente impostas pelo .NET Framework ajuda a tornar seu aplicativo mais robusto. Também garante que você não se esqueça de validar algo e inadvertidamente permita dados incorretos no banco de dados.

Aqui está uma listagem de código completa para o arquivo *Movie.cs* atualizado:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>IU de erro de validação no ASP.NET MVC

Execute novamente o aplicativo e navegue até a URL do */Movies* .

Clique no link **criar filme** para adicionar um novo filme. Preencha o formulário com alguns valores inválidos e, em seguida, clique no botão **criar** .

[![8_validationErrors](adding-validation-to-the-model/_static/image2.png)](adding-validation-to-the-model/_static/image1.png)

Observe como o formulário usou automaticamente uma cor de plano de fundo para realçar as caixas de texto que contêm dados inválidos e emitiu uma mensagem de erro de validação apropriada ao lado de cada uma delas. As mensagens de erro correspondem às cadeias de caracteres de erro que você especificou ao anotar a classe `Movie`. Os erros são impostos no lado do cliente (usando JavaScript) e no lado do servidor (caso um usuário tenha o JavaScript desabilitado).

Um benefício real é que você não precisa alterar uma única linha de código na classe `MoviesController` ou na exibição *Create. cshtml* para habilitar essa interface do usuário de validação. O controlador e as exibições que você criou anteriormente neste tutorial captam automaticamente as regras de validação que você especificou usando atributos na classe de modelo de `Movie`.

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Como a validação ocorre no método Create View e Create Action

Talvez você esteja se perguntando como a interface do usuário de validação foi gerada sem atualizações do código no controlador ou nas exibições. A próxima listagem mostra a aparência dos métodos de `Create` na classe `MovieController`. Eles são inalterados de como você os criou anteriormente neste tutorial.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

O primeiro método de ação exibe o formulário de criação inicial. O segundo manipula a postagem do formulário. O segundo método `Create` chama `ModelState.IsValid` para verificar se o filme tem erros de validação. A chamada a esse método avalia os atributos de validação que foram aplicados ao objeto. Se o objeto tiver erros de validação, o método `Create` reexibirá o formulário. Se não houver erros, o método salvará o novo filme no banco de dados.

Veja abaixo o modelo de exibição *Create. cshtml* que você com Scaffold anteriormente no tutorial. Ela é usada pelos métodos de ação mostrados acima para exibir o formulário inicial e exibi-lo novamente em caso de erro.

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

Observe como o código usa um `Html.EditorFor` auxiliar para gerar o elemento `<input>` para cada propriedade `Movie`. Ao lado desse auxiliar, há uma chamada para o método auxiliar `Html.ValidationMessageFor`. Esses dois métodos auxiliares funcionam com o objeto de modelo passado pelo controlador para a exibição (nesse caso, um objeto `Movie`). Eles procuram automaticamente os atributos de validação especificados no modelo e exibem mensagens de erro conforme apropriado.

O que é realmente bom para essa abordagem é que nem o controlador nem o modelo de exibição de criação sabem nada sobre as regras de validação reais sendo impostas ou sobre as mensagens de erro específicas exibidas. As regras de validação e as cadeias de caracteres de erro são especificadas somente na classe `Movie`.

Se você quiser alterar a lógica de validação mais tarde, poderá fazer isso em exatamente um local. Você não precisa se preocupar se diferentes partes do aplicativo estão inconsistentes com a forma como as regras são impostas – toda a lógica de validação será definida em um lugar e usada em todos os lugares. Isso mantém o código muito limpo e torna-o mais fácil de manter e desenvolver. Além disso, isso significa que você respeitará totalmente o princípio DRY.

## <a name="adding-formatting-to-the-movie-model"></a>Adicionando formatação ao modelo de filme

Abra o arquivo *Movie.cs*. O namespace [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) fornece atributos de formatação além do conjunto interno de atributos de validação. Você aplicará o atributo [`DisplayFormat`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) e um valor de enumeração [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) à data de lançamento e aos campos de preço. O código a seguir mostra as propriedades `ReleaseDate` e `Price` com o atributo [`DisplayFormat`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) apropriado.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs)]

Como alternativa, você poderia definir explicitamente um valor de [`DataFormatString`](https://msdn.microsoft.com/library/system.string.format.aspx) . O código a seguir mostra a propriedade data de lançamento com uma cadeia de caracteres de formato de data (isto é, "d"). Você usará isso para especificar que não quer que o horário seja como parte da data de lançamento.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample8.cs)]

O código a seguir formata a propriedade `Price` como moeda.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

A classe de `Movie` completa é mostrada abaixo.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

Execute o aplicativo e navegue até o controlador de `Movies`.

![8_format_SM](adding-validation-to-the-model/_static/image3.png)

Na próxima parte da série, examinaremos o aplicativo e faremos algumas melhorias nos métodos `Details` e `Delete` gerados automaticamente.

> [!div class="step-by-step"]
> [Anterior](adding-a-new-field.md)
> [Próximo](improving-the-details-and-delete-methods.md)
