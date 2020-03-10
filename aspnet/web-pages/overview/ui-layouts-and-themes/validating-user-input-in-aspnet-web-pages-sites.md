---
uid: web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
title: Validando a entrada do usuário em sites Páginas da Web do ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Este artigo discute como validar as informações que você obtém dos usuários &mdash; ou seja, para garantir que os usuários insiram informações válidas em formulários HTML em um como...
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 4eb060cc-cf14-41ae-bab1-14a2c15332d0
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: e6f8e1051d09d11f1756bfada44a73ba7c2a1db2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78563500"
---
# <a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a>Validando a entrada do usuário em sites Páginas da Web do ASP.NET (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo discute como validar as informações que você obtém dos usuários &mdash; ou seja, para garantir que os usuários insiram informações válidas em formulários HTML em um site do Páginas da Web do ASP.NET (Razor).
> 
> O que você aprenderá:
> 
> - Como verificar se a entrada de um usuário corresponde aos critérios de validação definidos por você.
> - Como determinar se todos os testes de validação passaram.
> - Como exibir erros de validação (e como formatá-los).
> - Como validar dados que não são fornecidos diretamente dos usuários.
> 
> Estes são os conceitos de programação do ASP.NET introduzidos no artigo:
> 
> - O auxiliar de `Validation`.
> - Os métodos `Html.ValidationSummary` e `Html.ValidationMessage`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 3
>   
> 
> Este tutorial também funciona com o Páginas da Web do ASP.NET 2.

Este artigo inclui as seções a seguir:

- [Visão geral da validação de entrada do usuário](#Overview_of_User_Input_Validation)
- [Validando entradas de usuário](#Validating_User_Input)
- [Adicionando validação do lado do cliente](#Adding_Client-Side_Validation)
- [Erros de validação de formatação](#Formatting_Validation_Errors)
- [Validando dados que não são fornecidos diretamente dos usuários](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a>Visão geral da validação de entrada do usuário

Se você solicitar que os usuários insiram informações em uma página — por exemplo, em um formulário — é importante certificar-se de que os valores que eles inserem são válidos. Por exemplo, você não deseja processar um formulário que não tenha informações críticas.

Quando os usuários inserem valores em um formulário HTML, os valores que eles inserem são cadeias de caracteres. Em muitos casos, os valores necessários são alguns outros tipos de dados, como números inteiros ou datas. Portanto, você também precisa certificar-se de que os valores inseridos pelos usuários possam ser convertidos corretamente nos tipos de dados apropriados.

Você também pode ter certas restrições sobre os valores. Mesmo que os usuários insiram corretamente um inteiro, por exemplo, talvez seja necessário certificar-se de que o valor esteja dentro de um determinado intervalo.

![Erros de validação que usam classes de estilo CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> **Importante** A validação da entrada do usuário também é importante para segurança. Ao restringir os valores que os usuários podem inserir em formulários, você reduz a chance de que alguém possa inserir um valor que possa comprometer a segurança do seu site.

<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a>Validando entradas de usuário

No Páginas da Web do ASP.NET 2, você pode usar o auxiliar de `Validator` para testar a entrada do usuário. A abordagem básica é fazer o seguinte:

1. Determine quais elementos de entrada (campos) você deseja validar.

    Normalmente, você valida valores em elementos `<input>` em um formulário. No entanto, é uma boa prática validar todas as entradas, até mesmo a entrada que vem de um elemento restrito, como uma lista de `<select>`. Isso ajuda a garantir que os usuários não ignorem os controles em uma página e enviem um formulário.
2. No código de página, adicione verificações de validação individuais para cada elemento de entrada usando métodos do auxiliar de `Validation`.

    Para verificar os campos obrigatórios, use `Validation.RequireField(field, [error message])` (para um campo individual) ou `Validation.RequireFields(field1, field2, ...))` (para obter uma lista de campos). Para outros tipos de validação, use `Validation.Add(field, ValidationType)`. Por `ValidationType`, você pode usar estas opções:

    `Validator.DateTime ([error message])`  
   `Validator.Decimal([error message])`  
   `Validator.EqualsTo(otherField [, error message])`  
   `Validator.Float([error message])`  
   `Validator.Integer([error message])`  
   `Validator.Range(min, max [, error message])`  
   `Validator.RegEx(pattern [, error message])`  
   `Validator.Required([error message])`  
   `Validator.StringLength(length)`  
   `Validator.Url([error message])`
3. Quando a página for enviada, verifique se a validação foi aprovada verificando `Validation.IsValid`:

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    Se houver erros de validação, você ignorará o processamento de página normal. Por exemplo, se a finalidade da página for atualizar um banco de dados, você não fará isso até que todos os erros de validação tenham sido corrigidos.
4. Se houver erros de validação, exiba as mensagens de erro na marcação da página usando `Html.ValidationSummary` ou `Html.ValidationMessage`ou ambos.

O exemplo a seguir mostra uma página que ilustra essas etapas.

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

Para ver como a validação funciona, execute esta página e faça os erros deliberadamente. Por exemplo, aqui está a aparência da página se você esquecer de inserir um nome de curso, se inserir um, e se você inserir uma data inválida:

![Erros de validação na página renderizada](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a>Adicionando validação do lado do cliente

Por padrão, a entrada do usuário é validada depois que os usuários enviam a página, ou seja, a validação é executada no código do servidor. Uma desvantagem dessa abordagem é que os usuários não sabem que fizeram um erro até que eles enviem a página. Se um formulário for longo ou complexo, os erros de relatório somente depois que a página for enviada poderão ser inconvenientes para o usuário.

Você pode adicionar suporte para executar a validação no script do cliente. Nesse caso, a validação é executada à medida que os usuários trabalham no navegador. Por exemplo, suponha que você especifique que um valor deve ser um inteiro. Se um usuário inserir um valor não inteiro, o erro será relatado assim que o usuário sair do campo de entrada. Os usuários obtêm comentários imediatos, o que é conveniente para eles. A validação baseada no cliente também pode reduzir o número de vezes que o usuário precisa enviar o formulário para corrigir vários erros.

> [!NOTE]
> Mesmo se você usar a validação do lado do cliente, a validação sempre será executada no código do servidor. Executar a validação no código do servidor é uma medida de segurança, caso os usuários ignorem a validação baseada no cliente.

1. Registre as seguintes bibliotecas JavaScript na página:  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

   Duas das bibliotecas são carregáveis de uma CDN (rede de distribuição de conteúdo), de modo que você não precisa necessariamente tê-las em seu computador ou servidor. No entanto, você deve ter uma cópia local de *jQuery. Validate. uninvasive. js*. Se você ainda não estiver trabalhando com um modelo do WebMatrix (como o **site inicial** ) que inclui a biblioteca, crie um site de páginas da Web baseado no **site inicial**. Em seguida, copie o arquivo *. js* para seu site atual.
2. Em marcação, para cada elemento que você está validando, adicione uma chamada para `Validation.For(field)`. Esse método emite atributos que são usados pela validação do lado do cliente. (Em vez de emitir o código JavaScript real, o método emite atributos como `data-val-...`. Esses atributos dão suporte à validação de cliente discreta que usa jQuery para fazer o trabalho.)

A página a seguir mostra como adicionar recursos de validação do cliente ao exemplo mostrado anteriormente.

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

Nem todas as verificações de validação são executadas no cliente. Em particular, a validação do tipo de dados (inteiro, data e assim por diante) não é executada no cliente. As verificações a seguir funcionam no cliente e no servidor:

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

Neste exemplo, o teste para uma data válida não funcionará no código do cliente. No entanto, o teste será executado no código do servidor.

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a>Erros de validação de formatação

Você pode controlar como os erros de validação são exibidos definindo as classes CSS que têm os seguintes nomes reservados:

- `field-validation-error`. Define a saída do método `Html.ValidationMessage` quando ele está exibindo um erro.
- `field-validation-valid`. Define a saída do método `Html.ValidationMessage` quando não há nenhum erro.
- `input-validation-error`. Define como os elementos de `<input>` são renderizados quando há um erro. (Por exemplo, você pode usar essa classe para definir a cor do plano de fundo de um &lt;entrada&gt; elemento para uma cor diferente se seu valor for inválido.) Essa classe CSS é usada somente durante a validação do cliente (no Páginas da Web do ASP.NET 2).
- `input-validation-valid`. Define a aparência dos elementos de `<input>` quando não há nenhum erro.
- `validation-summary-errors`. Define a saída do método `Html.ValidationSummary` que está exibindo uma lista de erros.
- `validation-summary-valid`. Define a saída do método `Html.ValidationSummary` quando não há nenhum erro.

O bloco de `<style>` a seguir mostra regras para condições de erro.

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

Se você incluir esse bloco de estilo nas páginas de exemplo anteriores no artigo, a exibição do erro será parecida com a ilustração a seguir:

![Erros de validação que usam classes de estilo CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> Se você não estiver usando a validação de cliente no Páginas da Web do ASP.NET 2, as classes CSS para os elementos `<input>` (`input-validation-error` e `input-validation-valid` não terão nenhum efeito.

### <a name="static-and-dynamic-error-display"></a>Exibição de erro estático e dinâmico

As regras de CSS são fornecidas em pares, como `validation-summary-errors` e `validation-summary-valid`. Esses pares permitem definir regras para as duas condições: uma condição de erro e uma condição "normal" (não erro). É importante entender que a marcação da exibição de erro sempre é renderizada, mesmo se não houver erros. Por exemplo, se uma página tiver um método `Html.ValidationSummary` na marcação, a origem da página conterá a marcação a seguir mesmo quando a página for solicitada pela primeira vez:

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

Em outras palavras, o método `Html.ValidationSummary` sempre renderiza um elemento `<div>` e uma lista, mesmo que a lista de erros esteja vazia. Da mesma forma, o método `Html.ValidationMessage` sempre renderiza um elemento `<span>` como um espaço reservado para um erro de campo individual, mesmo que não haja nenhum erro.

Em algumas situações, a exibição de uma mensagem de erro pode fazer com que a página flua novamente e pode fazer com que elementos na página se movimentem. As regras de CSS que terminam em `-valid` permitem que você defina um layout que pode ajudar a evitar esse problema. Por exemplo, você pode definir `field-validation-error` e `field-validation-valid` ter o mesmo tamanho fixo. Dessa forma, a área de exibição do campo é estática e não alterará o fluxo de página se uma mensagem de erro for exibida.

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a>Validando dados que não são fornecidos diretamente dos usuários

Às vezes, você precisa validar as informações que não são fornecidas diretamente de um formulário HTML. Um exemplo típico é uma página em que um valor é passado em uma cadeia de caracteres de consulta, como no exemplo a seguir:

`http://server/myapp/EditClassInformation?classid=1022`

Nesse caso, você deseja certificar-se de que o valor passado para a página (aqui, 1022 para o valor de `classid`) seja válido. Você não pode usar diretamente o auxiliar de `Validation` para executar essa validação. No entanto, você pode usar outros recursos do sistema de validação, como a capacidade de exibir mensagens de erro de validação.

> [!NOTE] 
> 
> **Importante** Sempre valide os valores obtidos de *qualquer* fonte, incluindo valores de campo de formulário, valores de cadeia de caracteres de consulta e valores de cookie. É fácil para as pessoas alterar esses valores (talvez para fins mal-intencionados). Portanto, você deve verificar esses valores para proteger seu aplicativo.

O exemplo a seguir mostra como você pode validar um valor que é passado em uma cadeia de caracteres de consulta. O código testa se o valor não está vazio e se é um inteiro.

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

Observe que o teste é executado quando a solicitação não é um envio de formulário (`if(!IsPost)`). Esse teste será aprovado na primeira vez que a página for solicitada, mas não quando a solicitação for um envio de formulário.

Para exibir esse erro, você pode adicionar o erro à lista de erros de validação chamando `Validation.AddFormError("message")`. Se a página contiver uma chamada para o método `Html.ValidationSummary`, o erro será exibido ali, assim como um erro de validação de entrada de usuário.

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Recursos adicionais

[Trabalhando com formulários HTML em sites Páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkID=202892)
