---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
title: Adicionando validação ao modelo | Microsoft Docs
author: Rick-Anderson
description: 'Observação: uma versão atualizada deste tutorial está disponível aqui que usa o ASP.NET MVC 5 e o Visual Studio 2013. É mais seguro, muito mais simples de seguir e demonstrar...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 5d9a2999-fcc4-4c45-a018-271fddf74a3b
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: c9f6699c5d3500d4c1fcade9252aeb9dd92983da
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78615027"
---
# <a name="adding-validation-to-the-model"></a>Adicionar validação ao modelo

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Uma versão atualizada deste tutorial está disponível [aqui](../../getting-started/introduction/getting-started.md) que usa o ASP.NET MVC 5 e o Visual Studio 2013. É mais seguro, muito mais simples de seguir e demonstra mais recursos.

Nesta seção, você adicionará a lógica de validação ao modelo de `Movie` e garantirá que as regras de validação sejam impostas sempre que um usuário tentar criar ou editar um filme usando o aplicativo.

## <a name="keeping-things-dry"></a>Mantendo as coisas SECAntes

Uma das principais filosofias de design do ASP.NET MVC é seca (&quot;Don't Repeat Yourself&quot;). O ASP.NET MVC incentiva você a especificar a funcionalidade ou o comportamento apenas uma vez e, em seguida, fazer com que ele seja refletido em qualquer lugar em um aplicativo. Isso reduz a quantidade de código que você precisa para escrever e torna o código que você escreve menos propenso a erros e é mais fácil de manter.

O suporte de validação fornecido pelo ASP.NET MVC e Entity Framework Code First é um ótimo exemplo do princípio seco em ação. Você pode especificar declarativamente regras de validação em um único local (na classe de modelo) e as regras são impostas em todos os lugares no aplicativo.

Vejamos como você pode aproveitar esse suporte de validação no aplicativo de filme.

## <a name="adding-validation-rules-to-the-movie-model"></a>Adicionando regras de validação ao modelo de filme

Você começará adicionando alguma lógica de validação à classe `Movie`.

Abra o arquivo *Movie.cs*. Adicione uma instrução `using` na parte superior do arquivo que faz referência ao namespace [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) :

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

Observe que o namespace não contém `System.Web`. Annotations fornece um conjunto interno de atributos de validação que você pode aplicar declarativamente a qualquer classe ou propriedade.

Agora, atualize a classe `Movie` para aproveitar os atributos internos de validação [`Required`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [`StringLength`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)e [`Range`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) . Use o código a seguir como um exemplo de onde aplicar os atributos.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs?highlight=4,10,13,17)]

Execute o aplicativo e você receberá novamente o seguinte erro de tempo de execução:

***O modelo que faz o backup do contexto ' MovieDBContext ' foi alterado desde que o banco de dados foi criado. Considere o uso de Migrações do Code First para atualizar o banco de dados ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).***

Usaremos as migrações para atualizar o esquema. Compile a solução e, em seguida, abra a janela do **console do Gerenciador de pacotes** e insira os seguintes comandos:

[!code-console[Main](adding-validation-to-the-model/samples/sample3.cmd)]

Quando esse comando for concluído, o Visual Studio abrirá o arquivo de classe que define o novo `DbMigration` classe derivada com o nome especificado (*AddDataAnnotationsMig*) e, no método `Up`, você poderá ver o código que atualiza as restrições de esquema. Os campos `Title` e `Genre` não são mais anuláveis (ou seja, você deve inserir um valor) e o campo `Rating` tem um comprimento máximo de 5.

Os atributos de validação especificam o comportamento que você deseja impor nas propriedades de modelo às quais eles são aplicados. O atributo `Required` indica que uma propriedade deve ter um valor; Neste exemplo, um filme deve ter valores para as propriedades `Title`, `ReleaseDate`, `Genre`e `Price` para ser válido. O atributo `Range` restringe um valor a um intervalo especificado. O atributo `StringLength` permite definir o tamanho máximo de uma propriedade de cadeia de caracteres e, opcionalmente, seu tamanho mínimo. Tipos intrínsecos (como `decimal, int, float, DateTime`) são necessários por padrão e não precisam do atributo `Required`.

Code First garante que as regras de validação especificadas em uma classe de modelo sejam impostas antes que o aplicativo salve as alterações no banco de dados. Por exemplo, o código a seguir gerará uma exceção quando o método de `SaveChanges` for chamado, porque vários valores de propriedade `Movie` necessários estão ausentes e o preço é zero (que está fora do intervalo válido).

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs?highlight=7-8)]

Ter regras de validação automaticamente impostas pelo .NET Framework ajuda a tornar seu aplicativo mais robusto. Também garante que você não se esqueça de validar algo e inadvertidamente permita dados incorretos no banco de dados.

Aqui está uma listagem de código completa para o arquivo *Movie.cs* atualizado:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>IU de erro de validação no ASP.NET MVC

Execute novamente o aplicativo e navegue até a URL do */Movies* .

Clique no link **criar novo** para adicionar um novo filme. Preencha o formulário com alguns valores inválidos e, em seguida, clique no botão **criar** .

![8_validationErrors](adding-validation-to-the-model/_static/image1.png)

> [!NOTE]
> para dar suporte à validação do jQuery para localidades não inglesas que usam uma vírgula (&quot;,&quot;) para um ponto decimal, você deve incluir *globalizable. js* e seu arquivo *culturas/globalizate. culturas* específico (de [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) e JavaScript para usar `Globalize.parseFloat`. O código a seguir mostra as modificações no arquivo Views\Movies\Edit.cshtml para trabalhar com a cultura de&quot; &quot;fr-FR:

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

Observe como o formulário usou automaticamente uma cor de borda vermelha para realçar as caixas de texto que contêm dados inválidos e emitiu uma mensagem de erro de validação apropriada ao lado de cada uma delas. Os erros são impostos no lado do cliente (usando o JavaScript e o jQuery) e no lado do servidor (caso um usuário tenha o JavaScript desabilitado).

Um benefício real é que você não precisa alterar uma única linha de código na classe `MoviesController` ou na exibição *Create. cshtml* para habilitar essa interface do usuário de validação. O controlador e as exibições criados anteriormente neste tutorial selecionaram automaticamente as regras de validação especificadas com atributos de validação nas propriedades da classe de modelo `Movie`.

Talvez você tenha notado as propriedades `Title` e `Genre`, o atributo necessário não será imposto até que você envie o formulário (pressione o botão **criar** ) ou insira o texto no campo de entrada e o removeu. Para um campo que está inicialmente vazio (como os campos no modo de exibição de criação) e que tem apenas o atributo necessário e outros atributos de validação, você pode fazer o seguinte para disparar a validação:

1. Para o campo.
2. Insira algum texto.
3. Saída da guia.
4. Volte para o campo.
5. Remova o texto.
6. Saída da guia.

A sequência acima irá disparar a validação necessária sem pressionar o botão enviar. Simplesmente pressionar o botão enviar sem inserir nenhum dos campos irá disparar a validação do lado do cliente. Os dados de formulário não são enviados no servidor até que não haja erros de validação do lado do cliente. Você pode testar isso colocando um ponto de interrupção no método HTTP post ou usando a [ferramenta Fiddler](http://fiddler2.com/fiddler2/) ou as [ferramentas de desenvolvedor F12](https://msdn.microsoft.com/ie/aa740478)do IE 9.

![](adding-validation-to-the-model/_static/image2.png)

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Como a validação ocorre no método Create View e Create Action

Talvez você esteja se perguntando como a interface do usuário de validação foi gerada sem atualizações do código no controlador ou nas exibições. A próxima listagem mostra a aparência dos métodos de `Create` na classe `MovieController`. Eles são inalterados de como você os criou anteriormente neste tutorial.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs?highlight=12,15)]

O primeiro método de ação (HTTP GET) `Create` exibe o formulário Criar inicial. A segunda versão (`[HttpPost]`) manipula a postagem de formulário. O segundo método `Create` (a versão `HttpPost`) chama `ModelState.IsValid` para verificar se o filme tem erros de validação. A chamada a esse método avalia os atributos de validação que foram aplicados ao objeto. Se o objeto tiver erros de validação, o método `Create` exibirá o formulário novamente. Se não houver erros, o método salvará o novo filme no banco de dados. Em nosso exemplo de filme que estamos usando, **o formulário não é Postado no servidor quando há erros de validação detectados no lado do cliente; o segundo** **método `Create`nunca é chamado**. Se você desabilitar o JavaScript em seu navegador, a validação do cliente será desabilitada e o método HTTP POST `Create` chamará `ModelState.IsValid` para verificar se o filme tem erros de validação.

Defina um ponto de interrupção no método `HttpPost Create` e verifique se o método nunca é chamado; a validação do lado do cliente não enviará os dados de formulário quando forem detectados erros de validação. Se você desabilitar o JavaScript no navegador e, em seguida, enviar o formulário com erros, o ponto de interrupção será atingido. Você ainda pode obter uma validação completa sem o JavaScript. A imagem a seguir mostra como desabilitar o JavaScript no Internet Explorer.

![](adding-validation-to-the-model/_static/image3.png)

![](adding-validation-to-the-model/_static/image4.png)

A imagem a seguir mostra como desabilitar o JavaScript no navegador FireFox.

![](adding-validation-to-the-model/_static/image5.png)

A imagem a seguir mostra como desabilitar o JavaScript com o navegador Chrome.

![](adding-validation-to-the-model/_static/image6.png)

Veja abaixo o modelo de exibição *Create. cshtml* que você com Scaffold anteriormente no tutorial. Ela é usada pelos métodos de ação mostrados acima para exibir o formulário inicial e exibi-lo novamente em caso de erro.

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample8.cshtml?highlight=22-23,30-31,38-39,46-47)]

Observe como o código usa um `Html.EditorFor` auxiliar para gerar o elemento `<input>` para cada propriedade `Movie`. Ao lado desse auxiliar, há uma chamada para o método auxiliar `Html.ValidationMessageFor`. Esses dois métodos auxiliares funcionam com o objeto de modelo passado pelo controlador para a exibição (nesse caso, um objeto `Movie`). Eles procuram automaticamente os atributos de validação especificados no modelo e exibem mensagens de erro conforme apropriado.

O que é realmente bom para essa abordagem é que nem o controlador nem o modelo de exibição de criação sabem nada sobre as regras de validação reais sendo impostas ou sobre as mensagens de erro específicas exibidas. As regras de validação e as cadeias de caracteres de erro são especificadas somente na classe `Movie`. Essas mesmas regras de validação são aplicadas automaticamente ao modo de exibição de edição e a qualquer outro modelo de exibições que você possa criar para editar seu modelo.

Se você quiser alterar a lógica de validação mais tarde, poderá fazer isso em exatamente um lugar adicionando atributos de validação ao modelo (neste exemplo, a classe `movie`). Você não precisa se preocupar se diferentes partes do aplicativo estão inconsistentes com a forma como as regras são impostas – toda a lógica de validação será definida em um lugar e usada em todos os lugares. Isso mantém o código muito limpo e torna-o mais fácil de manter e desenvolver. Além disso, isso significa que você respeitará totalmente o princípio DRY.

## <a name="adding-formatting-to-the-movie-model"></a>Adicionando formatação ao modelo de filme

Abra o arquivo *Movie.cs* e examine a classe `Movie`. O namespace [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) fornece atributos de formatação além do conjunto interno de atributos de validação. Já aplicamos um [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) valor de enumeração à data de lançamento e aos campos de preço. O código a seguir mostra as propriedades `ReleaseDate` e `Price` com o atributo [`DisplayFormat`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) apropriado.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

Os atributos de [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) não são atributos de validação, eles são usados para informar ao mecanismo de exibição como renderizar o HTML. No exemplo acima, o atributo `DataType.Date` exibe as datas do filme somente como datas, sem tempo. Por exemplo, os seguintes atributos de [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) não validam o formato dos dados:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

Os atributos listados acima fornecem dicas para que o mecanismo de exibição formate os dados (e forneça atributos como &lt;um&gt; para URL e &lt;a href =&quot;mailto:EmailAddress. com&quot;&gt; para email. Você pode usar o atributo [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) para validar o formato dos dados.

Uma abordagem alternativa para usar os atributos de `DataType`, você poderia definir explicitamente um valor de [`DataFormatString`](https://msdn.microsoft.com/library/system.string.format.aspx) . O código a seguir mostra a propriedade data de lançamento com uma cadeia de caracteres de formato de data (ou seja, &quot;d&quot;). Você usará isso para especificar que não quer que o horário seja como parte da data de lançamento.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample11.cs)]

A classe de `Movie` completa é mostrada abaixo.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample12.cs)]

Execute o aplicativo e navegue até o controlador de `Movies`. A data de lançamento e o preço são formatados adequadamente. A imagem abaixo mostra a data de lançamento e o preço usando &quot;fr-FR&quot; como a cultura.

![8_format_SM](adding-validation-to-the-model/_static/image7.png)

A imagem abaixo mostra os mesmos dados exibidos com a cultura padrão (inglês dos EUA).

![](adding-validation-to-the-model/_static/image8.png)

Na próxima parte da série, examinaremos o aplicativo e faremos algumas melhorias nos métodos `Details` e `Delete` gerados automaticamente.

> [!div class="step-by-step"]
> [Anterior](adding-a-new-field-to-the-movie-model-and-table.md)
> [Próximo](examining-the-details-and-delete-methods.md)
