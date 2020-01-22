---
uid: mvc/overview/getting-started/introduction/adding-validation
title: Adicionando validação | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/06/2019
ms.assetid: 9f35ca15-e216-4db6-9ebf-24380b0f31b4
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-validation
msc.type: authoredcontent
ms.openlocfilehash: 67df1a473cd13a651c1276054b93f34323479082
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519018"
---
# <a name="adding-validation"></a>Adicionando uma Validação

por [Rick Anderson]((https://twitter.com/RickAndMSFT))

[!INCLUDE [Tutorial Note](index.md)]

Nesta seção, você adicionará a lógica de validação ao modelo de `Movie` e garantirá que as regras de validação sejam impostas sempre que um usuário tentar criar ou editar um filme usando o aplicativo.

## <a name="keeping-things-dry"></a>Mantendo as coisas SECAntes

Uma das principais filosofias de design do ASP.NET MVC é [seca](http://en.wikipedia.org/wiki/Don't_repeat_yourself) (&quot;Don't REPEAT yourself&quot;). O ASP.NET MVC incentiva você a especificar a funcionalidade ou o comportamento apenas uma vez e, em seguida, fazer com que ele seja refletido em qualquer lugar em um aplicativo. Isso reduz a quantidade de código que você precisa para escrever e torna o código que você escreve menos propenso a erros e é mais fácil de manter.

O suporte de validação fornecido pelo ASP.NET MVC e Entity Framework Code First é um ótimo exemplo do princípio seco em ação. Você pode especificar declarativamente regras de validação em um único local (na classe de modelo) e as regras são impostas em todos os lugares no aplicativo.

Vejamos como você pode aproveitar esse suporte de validação no aplicativo de filme.

## <a name="adding-validation-rules-to-the-movie-model"></a>Adicionando regras de validação ao modelo de filme

Você começará adicionando alguma lógica de validação à classe `Movie`.

Abra o arquivo *Movie.cs*. Observe que o namespace [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) não contém `System.Web`. Annotations fornece um conjunto interno de atributos de validação que você pode aplicar declarativamente a qualquer classe ou propriedade. (Ele também contém atributos de formatação como [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) que ajudam com a formatação e não fornecem nenhuma validação.)

Agora, atualize a classe `Movie` para aproveitar os atributos internos de validação [`Required`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [`StringLength`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx)e [`Range`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) . Substitua a classe `Movie` pelo seguinte:

[!code-csharp[Main](adding-validation/samples/sample1.cs?highlight=5,13-15,18-19,22-23)]

O atributo [`StringLength`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) define o comprimento máximo da cadeia de caracteres e define essa limitação no banco de dados, portanto, o esquema do banco de dados será alterado. Clique com o botão direito do mouse na tabela **filmes** no **Gerenciador de servidores** e clique em **Abrir definição de tabela**:

![](adding-validation/_static/image1.png)

Na imagem acima, você pode ver que todos os campos de cadeia de caracteres estão definidos como [nvarchar (max)](https://technet.microsoft.com/library/ms186939.aspx). Usaremos as migrações para atualizar o esquema. Compile a solução e, em seguida, abra a janela do **console do Gerenciador de pacotes** e insira os seguintes comandos:

[!code-console[Main](adding-validation/samples/sample2.cmd)]

Quando esse comando for concluído, o Visual Studio abrirá o arquivo de classe que define o novo `DbMigration` classe derivada com o nome especificado (`DataAnnotations`) e, no método `Up`, você poderá ver o código que atualiza as restrições de esquema:

[!code-csharp[Main](adding-validation/samples/sample3.cs)]

O campo `Genre` não é mais anulável (ou seja, você deve inserir um valor). O campo de `Rating` tem um comprimento máximo de 5 e `Title` tem um comprimento máximo de 60. O comprimento mínimo de 3 em `Title` e o intervalo em `Price` não criaram alterações de esquema.

Examine o esquema do filme:

![](adding-validation/_static/image2.png)

Os campos de cadeia de caracteres mostram os novos limites de comprimento e `Genre` não são mais verificados como anuláveis.

Os atributos de validação especificam o comportamento que você deseja impor nas propriedades de modelo às quais eles são aplicados. Os atributos `Required` e `MinimumLength` indicam que uma propriedade deve ter um valor; porém, nada impede que um usuário insira um espaço em branco para atender a essa validação. O atributo [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) é usado para limitar quais caracteres podem ser inseridos. No código acima, `Genre` e `Rating` devem usar apenas letras (espaço em branco, números e caracteres especiais não são permitidos). O atributo [`Range`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) restringe um valor para dentro de um intervalo especificado. O atributo `StringLength` permite definir o tamanho máximo de uma propriedade de cadeia de caracteres e, opcionalmente, seu tamanho mínimo. Tipos de valor (como `decimal, int, float, DateTime`) são inerentemente necessários e não precisam do atributo `Required`.

Code First garante que as regras de validação especificadas em uma classe de modelo sejam impostas antes que o aplicativo salve as alterações no banco de dados. Por exemplo, o código a seguir lançará uma exceção [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception(v=vs.103).aspx) quando o método `SaveChanges` for chamado, porque vários valores de propriedade de `Movie` necessários estão ausentes:

[!code-csharp[Main](adding-validation/samples/sample4.cs)]

O código acima gera a seguinte exceção:

*Falha na validação de uma ou mais entidades. Consulte a propriedade ' EntityValidationErrors ' para obter mais detalhes.*

Ter regras de validação automaticamente impostas pelo .NET Framework ajuda a tornar seu aplicativo mais robusto. Também garante que você não se esqueça de validar algo e inadvertidamente permita dados incorretos no banco de dados.

## <a name="validation-error-ui-in-aspnet-mvc"></a>IU de erro de validação no ASP.NET MVC

Execute o aplicativo e navegue até a URL */Movies* .

Clique no link **criar novo** para adicionar um novo filme. Preencha o formulário com alguns valores inválidos. Assim que a validação do lado do cliente do jQuery detecta o erro, ela exibe uma mensagem de erro.

![8_validationErrors](adding-validation/_static/image3.png)

> [!NOTE]
> para dar suporte à validação do jQuery para localidades não inglesas que usam uma vírgula (",") para um ponto decimal, você deve incluir o NuGet globalizate conforme descrito anteriormente neste tutorial.

Observe como o formulário usou automaticamente uma cor de borda vermelha para realçar as caixas de texto que contêm dados inválidos e emitiu uma mensagem de erro de validação apropriada ao lado de cada uma delas. Os erros são impostos no lado do cliente (usando o JavaScript e o jQuery) e no lado do servidor (caso um usuário tenha o JavaScript desabilitado).

Um benefício real é que você não precisa alterar uma única linha de código na classe `MoviesController` ou na exibição *Create. cshtml* para habilitar essa interface do usuário de validação. O controlador e as exibições criados anteriormente neste tutorial selecionaram automaticamente as regras de validação especificadas com atributos de validação nas propriedades da classe de modelo `Movie`. Teste a validação usando o método de ação `Edit` e a mesma validação é aplicada.

Os dados de formulário não são enviados no servidor até que não haja erros de validação do lado do cliente. Você pode verificar isso colocando um ponto de interrupção no método HTTP post, usando a [ferramenta Fiddler](http://fiddler2.com/fiddler2/)ou as [ferramentas de desenvolvedor F12](https://msdn.microsoft.com/ie/aa740478)do IE.

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Como a validação ocorre no método Create View e Create Action

Talvez você esteja se perguntando como a interface do usuário de validação foi gerada sem atualizações do código no controlador ou nas exibições. A próxima listagem mostra a aparência dos métodos de `Create` na classe `MovieController`. Eles são inalterados de como você os criou anteriormente neste tutorial.

[!code-csharp[Main](adding-validation/samples/sample5.cs)]

O primeiro método de ação (HTTP GET) `Create` exibe o formulário Criar inicial. A segunda versão (`[HttpPost]`) manipula a postagem de formulário. O segundo método de `Create` (a versão de `HttpPost`) verifica `ModelState.IsValid` para ver se o filme tem erros de validação. Obter essa propriedade avalia todos os atributos de validação que foram aplicados ao objeto. Se o objeto tiver erros de validação, o método `Create` reexibirá o formulário. Se não houver erros, o método salvará o novo filme no banco de dados. Em nosso exemplo de filme, **o formulário não é Postado no servidor quando há erros de validação detectados no lado do cliente; o segundo** **método `Create` nunca é chamado**. Se você desabilitar o JavaScript em seu navegador, a validação do cliente será desabilitada e o método HTTP POST `Create` obterá `ModelState.IsValid` para verificar se o filme tem erros de validação.

Defina um ponto de interrupção no método `HttpPost Create` e verifique se o método nunca é chamado; a validação do lado do cliente não enviará os dados de formulário quando forem detectados erros de validação. Se você desabilitar o JavaScript no navegador e, em seguida, enviar o formulário com erros, o ponto de interrupção será atingido. Você ainda pode obter uma validação completa sem o JavaScript. A imagem a seguir mostra como desabilitar o JavaScript no Internet Explorer.

![](adding-validation/_static/image5.png)

![](adding-validation/_static/image6.png)

A imagem a seguir mostra como desabilitar o JavaScript no navegador FireFox.

![](adding-validation/_static/image7.png)

A imagem a seguir mostra como desabilitar o JavaScript no navegador Chrome.

![](adding-validation/_static/image8.png)

Veja abaixo o modelo de exibição *Create. cshtml* que você com Scaffold anteriormente no tutorial. Ela é usada pelos métodos de ação mostrados acima para exibir o formulário inicial e exibi-lo novamente em caso de erro.

[!code-cshtml[Main](adding-validation/samples/sample6.cshtml?highlight=16-17)]

Observe como o código usa um `Html.EditorFor` auxiliar para gerar o elemento `<input>` para cada propriedade `Movie`. Ao lado desse auxiliar, há uma chamada para o método auxiliar `Html.ValidationMessageFor`. Esses dois métodos auxiliares funcionam com o objeto de modelo passado pelo controlador para a exibição (nesse caso, um objeto `Movie`). Eles procuram automaticamente os atributos de validação especificados no modelo e exibem mensagens de erro conforme apropriado.

O que é realmente interessante nessa abordagem é que o controlador nem o modelo de exibição `Create` sabem nada sobre as regras de validação reais que estão sendo impostas ou as mensagens de erro específicas exibidas. As regras de validação e as cadeias de caracteres de erro são especificadas somente na classe `Movie`. Essas mesmas regras de validação são aplicadas automaticamente à exibição `Edit` e a outros modelos de exibição que podem ser criados e que editam o modelo.

Se você quiser alterar a lógica de validação mais tarde, poderá fazer isso em exatamente um lugar adicionando atributos de validação ao modelo (neste exemplo, a classe `movie`). Você não precisa se preocupar se diferentes partes do aplicativo estão inconsistentes com a forma como as regras são impostas – toda a lógica de validação será definida em um lugar e usada em todos os lugares. Isso mantém o código muito limpo e torna-o mais fácil de manter e desenvolver. E isso significa que você estará totalmente respeitando o princípio *seco* .

## <a name="using-datatype-attributes"></a>Usando atributos DataType

Abra o arquivo *Movie.cs* e examine a classe `Movie`. O namespace [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) fornece atributos de formatação além do conjunto interno de atributos de validação. Já aplicamos um [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) valor de enumeração à data de lançamento e aos campos de preço. O código a seguir mostra as propriedades `ReleaseDate` e `Price` com o atributo [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) apropriado.

[!code-csharp[Main](adding-validation/samples/sample7.cs)]

Os atributos [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) só fornecem dicas para que o mecanismo de exibição formate os dados (e forneça atributos como `<a>` para URL e `<a href="mailto:EmailAddress.com">` para email. Você pode usar o atributo [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) para validar o formato dos dados. O atributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) é usado para especificar um tipo de dados que seja mais específico do que o tipo intrínseco de Database; eles ***não*** são atributos de validação. Nesse caso, apenas desejamos acompanhar a data, não a data e a hora. A [Enumeração DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) fornece vários tipos de dados, como *data, hora, PhoneNumber, moeda, EmailAddress* e muito mais. O atributo `DataType` também pode permitir que o aplicativo forneça automaticamente recursos específicos a um tipo. Por exemplo, um link de `mailto:` pode ser criado para [DataType. EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)e um seletor de data pode ser fornecido para [DataType. Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) em navegadores que dão suporte a [HTML5](http://html5.org/). Os atributos [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) emite os atributos [de dados](http://ejohn.org/blog/html-5-data-attributes/) HTML 5-(pronuncia-se *os dados Dash*) que os navegadores HTML 5 podem entender. Os atributos de [tipo de dados](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) não fornecem nenhuma validação.

`DataType.Date` não especifica o formato da data exibida. Por padrão, o campo de dados é exibido de acordo com os formatos padrão com base no [CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx)do servidor.

O atributo `DisplayFormat` é usado para especificar explicitamente o formato de data:

[!code-csharp[Main](adding-validation/samples/sample8.cs)]

A configuração `ApplyFormatInEditMode` especifica que a formatação especificada também deve ser aplicada quando o valor é exibido em uma caixa de texto para edição. (Talvez você não queira que, para alguns campos — por exemplo, para valores de moeda, talvez não queira o símbolo de moeda na caixa de texto para edição.)

Você pode usar o atributo [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) por si só, mas, em geral, é uma boa ideia usar o atributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) também. O atributo `DataType` transmite a *semântica* dos dados em vez de como renderizá-los em uma tela e fornece os seguintes benefícios que você não obtém com `DisplayFormat`:

- O navegador pode habilitar recursos do HTML5 (por exemplo, para mostrar um controle de calendário, o símbolo de moeda apropriado da localidade, links de email, etc.).
- Por padrão, o navegador renderizará dados usando o formato correto com base em sua [localidade](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx).
- O atributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) pode habilitar o MVC a escolher o modelo de campo à direita para renderizar os dados (o [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) se usado por si só usa o modelo de cadeia de caracteres). Para obter mais informações, consulte Brad Wilson ' s [ASP.NET MVC 2 templates](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). (Embora seja escrito para MVC 2, este artigo ainda se aplica à versão atual do ASP.NET MVC.)

Se você usar o atributo `DataType` com um campo de data, precisará especificar o atributo `DisplayFormat` também para garantir que o campo seja renderizado corretamente em navegadores Chrome. Para obter mais informações, consulte [este thread StackOverflow](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

> [!NOTE]
> a validação do jQuery não funciona com o atributo de [intervalo](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) e [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx). Por exemplo, o seguinte código sempre exibirá um erro de validação do lado do cliente, mesmo quando a data estiver no intervalo especificado:
> 
> [!code-csharp[Main](adding-validation/samples/sample9.cs)]
> 
> Será necessário desabilitar a validação de data do jQuery para usar o atributo [Range](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) com [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx). Geralmente, não é uma boa prática compilar datas rígidas em seus modelos, portanto, usar o atributo [Range](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) e [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx) não é recomendado.

O seguinte código mostra como combinar atributos em uma linha:

[!code-csharp[Main](adding-validation/samples/sample10.cs?highlight=4,6,10,12)]

Na próxima parte da série, examinaremos o aplicativo e faremos algumas melhorias nos métodos `Details` e `Delete` gerados automaticamente.

> [!div class="step-by-step"]
> [Anterior](adding-a-new-field.md)
> [Próximo](examining-the-details-and-delete-methods.md)
