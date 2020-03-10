---
uid: web-pages/overview/ui-layouts-and-themes/4-working-with-forms
title: Trabalhando com formulários HTML em sites Páginas da Web do ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Um formulário é uma seção de um documento HTML em que você coloca controles de entrada de usuário, como caixas de texto, caixas de seleção, botões de opção e listas de pull. Você usa formulários qu...
ms.author: riande
ms.date: 02/10/2014
ms.assetid: f3f4b8c8-e8f6-4474-ad94-69228a6c01ee
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/4-working-with-forms
msc.type: authoredcontent
ms.openlocfilehash: c7d4802063c8610a246afe67bd15eea429f7304a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78639702"
---
# <a name="working-with-html-forms-in-aspnet-web-pages-razor-sites"></a>Trabalhando com formulários HTML em sites Páginas da Web do ASP.NET (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo descreve como processar um formulário HTML (com caixas de texto e botões) quando você está trabalhando em um site Páginas da Web do ASP.NET (Razor).
> 
> **O que você aprenderá:** 
> 
> - Como criar um formulário HTML.
> - Como ler a entrada do usuário no formulário.
> - Como validar a entrada do usuário.
> - Como restaurar valores de formulário depois que a página é enviada.
> 
> Estes são os conceitos de programação do ASP.NET introduzidos no artigo:
> 
> - O objeto `Request`.
> - Validação de entrada.
> - Codificação HTML.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 3
>   
> 
> Este tutorial também funciona com o Páginas da Web do ASP.NET 2.

## <a name="creating-a-simple-html-form"></a>Criando um formulário HTML simples

1. Crie um novo site.
2. Na pasta raiz, crie uma página da Web chamada *Form. cshtml* e insira a seguinte marcação:

    [!code-html[Main](4-working-with-forms/samples/sample1.html)]
3. Inicie a página no seu navegador. (No WebMatrix, no espaço de trabalho **arquivos** , clique com o botão direito do mouse no arquivo e selecione **Iniciar no navegador**.) Um formulário simples com três campos de entrada e um botão **Enviar** é exibido.

    ![Captura de tela de um formulário com três caixas de texto.](4-working-with-forms/_static/image1.png)

    Neste ponto, se você clicar no botão **Enviar** , nada acontecerá. Para tornar o formulário útil, você precisa adicionar algum código que será executado no servidor.

## <a name="reading-user-input-from-the-form"></a>Lendo a entrada do usuário no formulário

Para processar o formulário, você adiciona um código que lê os valores de campo enviados e faz algo com eles. Este procedimento mostra como ler os campos e exibir a entrada do usuário na página. (Em um aplicativo de produção, geralmente você faz coisas mais interessantes com a entrada do usuário. Você fará isso no artigo sobre como trabalhar com bancos de dados.)

1. Na parte superior do arquivo *Form. cshtml* , insira o seguinte código:

    [!code-cshtml[Main](4-working-with-forms/samples/sample2.cshtml)]

    Quando o usuário solicita pela primeira vez a página, apenas o formulário vazio é exibido. O usuário (que será você) preenche o formulário e clica em **Enviar**. Isso envia (posta) a entrada do usuário para o servidor. Por padrão, a solicitação vai para a mesma página (ou seja, *Form. cshtml*).

    Quando você envia a página desta vez, os valores inseridos são exibidos logo acima do formulário:

    ![Captura de tela que mostra os valores que você inseriu exibidos na página.](4-working-with-forms/_static/image2.png)

    Examine o código da página. Primeiro, você usa o método `IsPost` para determinar se a página está sendo &#8212; postada, ou seja, se um usuário clicou no botão **Enviar** . Se esta for uma postagem, `IsPost` retornará true. Esse é o modo padrão no Páginas da Web do ASP.NET para determinar se você está trabalhando com uma solicitação inicial (uma solicitação GET) ou um postback (uma solicitação POST). (Para obter mais informações sobre GET e POST, consulte a barra lateral "HTTP GET e POST e a propriedade IsPost" na [introdução à programação de páginas da Web do ASP.NET usando a sintaxe do Razor](https://go.microsoft.com/fwlink/?LinkId=202890#SB_HttpGetPost).)

    Em seguida, você obtém os valores que o usuário preencheu do objeto `Request.Form` e os coloca em variáveis para mais tarde. O objeto `Request.Form` contém todos os valores que foram enviados com a página, cada um identificado por uma chave. A chave é equivalente ao atributo `name` do campo de formulário que você deseja ler. Por exemplo, para ler o campo `companyname` (caixa de texto), use `Request.Form["companyname"]`.

    Os valores de formulário são armazenados no objeto `Request.Form` como cadeias de caracteres. Portanto, quando você precisa trabalhar com um valor como um número ou uma data ou algum outro tipo, você precisa convertê-lo de uma cadeia de caracteres para esse tipo. No exemplo, o método `AsInt` da `Request.Form` é usado para converter o valor do campo Employees (que contém uma contagem Employee) em um inteiro.
2. Inicie a página no navegador, preencha os campos de formulário e clique em **Enviar**. A página exibe os valores inseridos.

> [!TIP] 
> 
> <a id="SB_HTMLEncoding"></a>
> ### <a name="html-encoding-for-appearance-and-security"></a>Codificação HTML para aparência e segurança
> 
> O HTML tem usos especiais para caracteres como `<`, `>`e `&`. Se esses caracteres especiais aparecerem onde não são esperados, eles poderão arruinar a aparência e a funcionalidade da sua página da Web. Por exemplo, o navegador interpreta o `<` caractere (a menos que seja seguido por um espaço) como o início de um elemento HTML, como `<b>` ou `<input ...>`. Se o navegador não reconhecer o elemento, ele simplesmente descartará a cadeia de caracteres que começa com `<` até alcançar algo que ele reconheça novamente. Obviamente, isso pode resultar em alguma renderização estranha na página.
> 
> A codificação HTML substitui esses caracteres reservados por um código que os navegadores interpretam como o símbolo correto. Por exemplo, o caractere `<` é substituído por `&lt;` e o caractere `>` é substituído por `&gt;`. O navegador renderiza essas cadeias de substituição como os caracteres que você deseja ver.
> 
> É uma boa ideia usar a codificação HTML sempre que você exibir cadeias de caracteres (entrada) obtidas de um usuário. Se você não fizer isso, um usuário poderá tentar fazer com que sua página da Web execute um script mal-intencionado ou faça outra coisa que comprometa a segurança do site ou que não seja o que você pretendia. (Isso é particularmente importante se você tomar a entrada do usuário, armazená-lo em algum lugar &#8212; e, em seguida, exibi-lo posteriormente, por exemplo, como um comentário do blog, uma revisão do usuário ou algo assim.)
> 
> Para ajudar a evitar esses problemas, Páginas da Web do ASP.NET código HTML automaticamente codifica qualquer conteúdo de texto que você produzir de seu código. Por exemplo, quando você exibe o conteúdo de uma variável ou uma expressão usando um código como `@MyVar`, Páginas da Web do ASP.NET codifica automaticamente a saída.

## <a name="validating-user-input"></a>Validando entradas de usuário

Os usuários cometem erros. Você pede que eles preencham um campo e eles se esquecem, ou você pede que eles insiram o número de funcionários e, em vez disso, digitem um nome. Para certificar-se de que um formulário foi preenchido corretamente antes de processá-lo, você valida a entrada do usuário.

Este procedimento mostra como validar todos os três campos de formulário para certificar-se de que o usuário não os deixou em branco. Você também verifica se o valor de contagem de funcionários é um número. Se houver erros, você exibirá uma mensagem de erro que informa ao usuário quais valores não passaram na validação.

1. No arquivo *Form. cshtml* , substitua o primeiro bloco de código pelo seguinte código: 

    [!code-cshtml[Main](4-working-with-forms/samples/sample3.cshtml)]

    Para validar a entrada do usuário, use o auxiliar `Validation`. Você registra os campos obrigatórios chamando `Validation.RequireField`. Você registra outros tipos de validação chamando `Validation.Add` e especificando o campo a ser validado e o tipo de validação a ser executado.

    Quando a página é executada, o ASP.NET faz toda a validação para você. Você pode verificar os resultados chamando `Validation.IsValid`, que retorna true se tudo passar e false se algum campo tiver falha na validação. Normalmente, você chama `Validation.IsValid` antes de executar qualquer processamento na entrada do usuário.
2. Atualize o elemento `<body>` adicionando três chamadas ao método `Html.ValidationMessage`, desta forma:

    [!code-cshtml[Main](4-working-with-forms/samples/sample4.cshtml?highlight=8,13,18)]

    Para exibir mensagens de erro de validação, você pode chamar HTML.`ValidationMessage` e passe o nome do campo para o qual você deseja a mensagem.
3. Execute a página. Deixe os campos em branco e clique em **Enviar**. Você verá mensagens de erro.

    ![Captura de tela que mostra as mensagens de erro exibidas se a entrada do usuário não passar na validação.](4-working-with-forms/_static/image3.jpg)
4. Adicione uma cadeia de caracteres (por exemplo, "ABC") ao campo **contagem de funcionários** e clique em **Enviar** novamente. Desta vez, você verá um erro que indica que a cadeia de caracteres não está no formato correto, ou seja, um inteiro.

    ![Captura de tela que mostra mensagens de erro exibidas se os usuários inserirem uma cadeia de caracteres para o campo Employees.](4-working-with-forms/_static/image4.jpg)

Páginas da Web do ASP.NET fornece mais opções para validar a entrada do usuário, incluindo a capacidade de executar automaticamente a validação usando o script do cliente, para que os usuários obtenham comentários imediatos no navegador. Consulte a seção [recursos adicionais](#Additional_Resources) mais tarde para obter mais informações.

## <a name="restoring-form-values-after-postbacks"></a>Restaurando valores de formulário após postbacks

Quando você testou a página na seção anterior, talvez tenha notado que, se tiver um erro de validação, tudo o que você inseriu (não apenas os dados inválidos) foi desfeito e você precisava inserir valores novamente para todos os campos. Isso ilustra um ponto importante: quando você envia uma página, processa-a e, em seguida, renderiza a página novamente, a página é recriada do zero. Como vimos, isso significa que todos os valores que estavam na página quando foram enviados são perdidos.

No entanto, você pode corrigir isso facilmente. Você tem acesso aos valores que foram enviados (no objeto `Request.Form`, de modo que você pode preencher esses valores de volta nos campos de formulário quando a página é renderizada.

1. No arquivo *Form. cshtml* , substitua os atributos de `value` dos elementos `<input>` usando o atributo `value`.: 

    [!code-cshtml[Main](4-working-with-forms/samples/sample5.cshtml?highlight=13,19,25)]

    O atributo `value` dos elementos `<input>` foi definido para ler dinamicamente o valor do campo para fora do objeto `Request.Form`. Na primeira vez que a página é solicitada, os valores no objeto `Request.Form` estão todos vazios. Isso é bom, porque dessa forma o formulário está em branco.
2. Inicie a página no navegador, preencha os campos de formulário ou deixe-os em branco e clique em **Enviar**. Uma página que mostra os valores enviados é exibida.

    ![formulários-5](4-working-with-forms/_static/image5.jpg)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais

- [1.001 maneiras de obter entrada de usuários da Web](https://msdn.microsoft.com/library/ms971057.aspx)
- [Usando formulários e processando entrada do usuário](https://msdn.microsoft.com/library/ms525182(VS.90).aspx)
- [Validação da entrada do usuário em sites de Páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=253002)
- [Usando o preenchimento automático em formulários HTML](https://msdn.microsoft.com/library/ms533032(VS.85).aspx)
