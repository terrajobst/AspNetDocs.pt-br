---
uid: web-pages/overview/getting-started/introducing-razor-syntax-vb
title: Introdução à programação da Web do ASP.NET usando a sintaxe do Razor (Visual Basic) | Microsoft Docs
author: Rick-Anderson
description: Este apêndice fornece uma visão geral da programação com páginas da Web do ASP.NET em Visual Basic, usando o sintaxe Razor.
ms.author: riande
ms.date: 02/07/2014
ms.assetid: 5da59646-e973-41cd-88a9-c6b2c0594027
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-vb
msc.type: authoredcontent
ms.openlocfilehash: 2be57655b8c9b76b94e1d9a7ae5fbee27545a0a9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78526589"
---
# <a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-visual-basic"></a>Introdução à programação da Web do ASP.NET usando a sintaxe do Razor (Visual Basic)

por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo fornece uma visão geral da programação com Páginas da Web do ASP.NET usando o sintaxe Razor e Visual Basic. ASP.NET é a tecnologia da Microsoft para executar páginas da Web dinâmicas em servidores Web.
> 
> **O que você aprenderá**:
> 
> - As 8 principais dicas de programação para introdução ao Páginas da Web do ASP.NET de programação usando sintaxe Razor.
> - Conceitos básicos de programação que você precisará.
> - O que é o código do servidor ASP.NET e a sintaxe Razor.
>   
> 
> ## <a name="software-versions"></a>Versões de software
> 
> 
> - Páginas da Web do ASP.NET (Razor) 3
>   
> 
> Este tutorial também funciona com o Páginas da Web do ASP.NET 2.

A maioria dos exemplos de uso de Páginas da Web do ASP.NET C#com sintaxe Razor uso. Mas o sintaxe Razor também dá suporte a Visual Basic. Para programar uma página da Web do ASP.NET no Visual Basic, você cria uma página da Web com uma extensão de nome de arquivo *. vbhtml* e, em seguida, adiciona Visual Basic código. Este artigo fornece uma visão geral de como trabalhar com a linguagem de Visual Basic e a sintaxe para criar páginas da Web ASP.NET.

> [!NOTE]
> Os modelos de site padrão do Microsoft WebMatrix (**padaria**, **Galeria de fotos**e **site de início**, etc.) estão disponíveis no C# e Visual Basic versões. Você pode instalar os modelos de Visual Basic pelo como pacotes NuGet. Os modelos de site são instalados na pasta raiz do seu site em uma pasta chamada *Microsoft Templates*.

## <a name="the-top-8-programming-tips"></a>As 8 principais dicas de programação

Esta seção lista algumas dicas que você precisa saber ao começar a escrever o código do servidor ASP.NET usando o sintaxe Razor.

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1. você adiciona código a uma página usando o caractere @

O caractere `@` inicia expressões embutidas, blocos de instrução única e blocos de várias instruções:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample1.vbhtml)]

O resultado exibido em um navegador:

![Razor-Img1](introducing-razor-syntax-vb/_static/image1.jpg)

> [!TIP] 
> 
> **Codificação HTML**
> 
> Quando você exibe o conteúdo em uma página usando o caractere `@`, como nos exemplos anteriores, o ASP.NET HTML codifica a saída. Isso substitui os caracteres HTML reservados (como `<` e `>` e `&`) por códigos que permitem que os caracteres sejam exibidos como caracteres em uma página da Web, em vez de serem interpretados como marcas HTML ou entidades. Sem codificação HTML, a saída do código do servidor pode não ser exibida corretamente e pode expor uma página a riscos de segurança.
> 
> Se sua meta é gerar uma marcação HTML que renderiza marcas como marcação (por exemplo `<p></p>` para um parágrafo ou `<em></em>` para enfatizar o texto), consulte a seção [combinando texto, marcação e código em blocos de código](#BM_CombiningTextMarkupAndCode) mais adiante neste artigo.
> 
> Você pode ler mais sobre codificação HTML em [trabalhando com formulários HTML em páginas da Web do ASP.net sites](https://go.microsoft.com/fwlink/?LinkId=202892).

### <a name="2-you-enclose-code-blocks-with-codeend-code"></a>2. você coloca blocos de código com código... Código final

Um bloco de código inclui uma ou mais instruções de código e é incluído com as palavras-chave `Code` e `End Code`. Coloque a palavra-chave `Code` de abertura imediatamente após &#8212; o caractere de `@` não pode haver espaço em branco entre elas.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample2.vbhtml)]

O resultado exibido em um navegador:

![Razor-Img2](introducing-razor-syntax-vb/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-line-break"></a>3. dentro de um bloco, você encerra cada instrução de código com uma quebra de linha

Em um bloco de código Visual Basic, cada instrução termina com uma quebra de linha. (Posteriormente, no artigo, você verá uma maneira de encapsular uma instrução de código longo em várias linhas, se necessário.)

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample3.vbhtml)]

### <a name="4-you-use-variables-to-store-values"></a>4. você usa variáveis para armazenar valores

Você pode armazenar valores em uma *variável*, incluindo cadeias de caracteres, números e datas, etc. Você cria uma nova variável usando a palavra-chave `Dim`. Você pode inserir valores de variáveis diretamente em uma página usando `@`.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample4.vbhtml)]

O resultado exibido em um navegador:

![Razor-Img3](introducing-razor-syntax-vb/_static/image3.jpg)

### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5. você coloca valores de cadeia de caracteres literais entre aspas duplas

Uma *String* é uma sequência de caracteres que são tratados como texto. Para especificar uma cadeia de caracteres, coloque-a entre aspas duplas:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample5.vbhtml)]

Para inserir aspas duplas em um valor de cadeia de caracteres, insira dois caracteres de aspas duplas. Se você quiser que o caractere de aspas duplas apareça uma vez na saída da página, insira-o como `""` dentro da cadeia de caracteres entre aspas e, se desejar que ele apareça duas vezes, insira-o como `""""` dentro da cadeia de caracteres entre aspas.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample6.vbhtml)]

O resultado exibido em um navegador:

![Razor-Img4](introducing-razor-syntax-vb/_static/image4.jpg)

### <a name="6-visual-basic-code-is-not-case-sensitive"></a>6. o código de Visual Basic não diferencia maiúsculas de minúsculas

O idioma da Visual Basic não diferencia maiúsculas de minúsculas. As palavras-chave de programação (como `Dim`, `If`e `True`) e nomes de variáveis (como `myString`ou `subTotal`) podem ser escritas em qualquer caso.

As linhas de código a seguir atribuem um valor à variável `lastname` usando um nome em minúsculas e, em seguida, geram o valor da variável para a página usando um nome em maiúsculas.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample7.vbhtml)]

O resultado exibido em um navegador:

![vb-syntax-5](introducing-razor-syntax-vb/_static/image5.jpg)

### <a name="7-much-of-your-coding-involves-working-with-objects"></a>7. a maior parte da codificação envolve trabalhar com objetos

Um objeto representa algo que você pode programar com &#8212; uma página, uma caixa de texto, um arquivo, uma imagem, uma solicitação da Web, uma mensagem de email, um registro de cliente (linha de banco de dados), etc. Os objetos têm propriedades que descrevem suas &#8212; características um objeto de caixa de texto tem uma propriedade `Text`, um objeto de solicitação tem uma propriedade `Url`, uma mensagem de email tem uma propriedade `From` e um objeto Customer tem uma propriedade `FirstName`. Os objetos também têm métodos que são os verbos &quot;&quot; podem ser executados. Os exemplos incluem o método de `Save` de um objeto de arquivo, o método `Rotate` de um objeto de imagem e um método de `Send` do objeto de email.

Com freqüência, você trabalhará com o objeto `Request`, que fornece informações como os valores dos campos de formulário na página (caixas de texto, etc.), que tipo de navegador fez a solicitação, a URL da página, a identidade do usuário, etc. Este exemplo mostra como acessar as propriedades do objeto `Request` e como chamar o método `MapPath` do objeto `Request`, que fornece o caminho absoluto da página no servidor:

[!code-html[Main](introducing-razor-syntax-vb/samples/sample8.html)]

O resultado exibido em um navegador:

![Razor-Img5](introducing-razor-syntax-vb/_static/image6.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8. você pode escrever código que toma decisões

Um recurso importante das páginas da Web dinâmicas é que você pode determinar o que fazer com base nas condições. A maneira mais comum de fazer isso é com a instrução de `If` (e a instrução `Else` opcional).

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample9.vbhtml)]

A instrução `If IsPost` é uma maneira abreviada de escrever `If IsPost = True`. Juntamente com `If` instruções, há várias maneiras de testar condições, repetir blocos de código e assim por diante, que são descritos posteriormente neste artigo.

O resultado exibido em um navegador (depois de clicar em **Enviar**):

![Razor-Img6](introducing-razor-syntax-vb/_static/image7.jpg)

> [!TIP] 
> 
> **Métodos GET e POST de HTTP e a propriedade IsPost**
> 
> O protocolo usado para páginas da Web (HTTP) oferece suporte a um número muito limitado de métodos (&quot;verbos&quot;) que são usados para fazer solicitações ao servidor. Os dois mais comuns são GET, que é usado para ler uma página e POST, que é usado para enviar uma página. Em geral, na primeira vez que um usuário solicita uma página, a página é solicitada usando GET. Se o usuário preenche um formulário e clica em **Enviar**, o navegador faz uma solicitação post para o servidor.
> 
> Na programação da Web, geralmente é útil saber se uma página está sendo solicitada como um GET ou como uma POSTAgem para que você saiba como processar a página. No Páginas da Web do ASP.NET, você pode usar a propriedade `IsPost` para ver se uma solicitação é GET ou POST. Se a solicitação for uma POSTAgem, a propriedade `IsPost` retornará true e você poderá fazer coisas como ler os valores das caixas de texto em um formulário. Muitos exemplos que você verá mostram como processar a página de forma diferente, dependendo do valor de `IsPost`.

## <a name="a-simple-code-example"></a>Um exemplo de código simples

Este procedimento mostra como criar uma página que ilustra as técnicas básicas de programação. No exemplo, você cria uma página que permite aos usuários inserir dois números e, em seguida, adiciona-os e exibe o resultado.

1. No editor, crie um novo arquivo e nomeie-o como *Addnumerates. vbhtml*.
2. Copie o código e a marcação a seguir para a página, substituindo qualquer coisa que já esteja na página.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample10.vbhtml)]

    Aqui estão algumas coisas que você deve observar:

    - O caractere `@` inicia o primeiro bloco de código na página e precede a variável `totalMessage` inserida perto da parte inferior.
    - O bloco na parte superior da página está entre `Code...End Code`.
    - As variáveis `total`, `num1`, `num2`e `totalMessage` armazenam vários números e uma cadeia de caracteres.
    - O valor da cadeia de caracteres literal atribuído à variável `totalMessage` está entre aspas duplas.
    - Como o código de Visual Basic não diferencia maiúsculas de minúsculas, quando a variável de `totalMessage` é usada perto da parte inferior da página, seu nome precisa apenas corresponder à grafia da declaração de variável na parte superior da página. A capitalização não importa.
    - A expressão `num1.AsInt()` + `num2.AsInt()` mostra como trabalhar com objetos e métodos. O método `AsInt` em cada variável converte a cadeia de caracteres inserida por um usuário em um número inteiro (um inteiro) que pode ser adicionado.
    - A marca de `<form>` inclui um atributo de `method="post"`. Isso especifica que quando o usuário clicar em **Adicionar**, a página será enviada ao servidor usando o método http post. Quando a página é enviada, o código `If IsPost` é avaliado como true e o código condicional é executado, exibindo o resultado da adição dos números.
3. Salve a página e execute-a em um navegador. (Verifique se a página está selecionada no espaço de trabalho **arquivos** antes de executá-la.) Insira dois números inteiros e, em seguida, clique no botão **Adicionar** .

    ![Razor-Img7](introducing-razor-syntax-vb/_static/image8.jpg)

## <a name="visual-basic-language-and-syntax"></a>Linguagem e sintaxe de Visual Basic

Anteriormente, você viu um exemplo básico de como criar uma página da Web ASP.NET e como você pode adicionar código de servidor à marcação HTML. Aqui, você aprenderá as noções básicas do uso de Visual Basic para escrever código de servidor &#8212; ASP.NET usando o sintaxe Razor, ou seja, as regras de linguagem de programação.

Se você tiver experiência com programação (especialmente se tiver usado C, C++, C#, Visual Basic ou JavaScript), grande parte do que você lê aqui será familiar. Você provavelmente precisará se familiarizar apenas com o modo como o código do WebMatrix é adicionado à marcação em arquivos *. vbhtml* .

### <a id="BM_CombiningTextMarkupAndCode"></a>Combinando texto, marcação e código em blocos de código

Em blocos de código de servidor, muitas vezes você desejará produzir texto e marcação para a página. Se um bloco de código de servidor contiver texto que não seja código e que, em vez disso, deve ser renderizado como está, ASP.NET precisará ser capaz de distinguir esse texto do código. Há várias maneiras de fazer isso.

- Coloque o texto em um elemento de bloco HTML como `<p></p>` ou `<em></em>`:

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample11.vbhtml)]

    O elemento HTML pode incluir texto, elementos HTML adicionais e expressões de código de servidor. Quando ASP.NET vê a marca HTML de abertura (por exemplo, `<p>`), ela processa tudo o elemento e seu conteúdo como está no navegador (e resolve as expressões de código de servidor).

- Use o operador `@:` ou o elemento `<text>`. O `@:` gera uma única linha de conteúdo que contém texto sem formatação ou marcas HTML não correspondentes; o elemento `<text>` inclui várias linhas na saída. Essas opções são úteis quando você não deseja renderizar um elemento HTML como parte da saída.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample12.vbhtml)]

    O exemplo a seguir repete o exemplo anterior, mas usa um único par de marcas `<text>` para colocar o texto a ser renderizado.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample13.vbhtml)]

    No exemplo a seguir, as marcas `<text>` e `</text>` incluem três linhas, todas as quais têm um texto não contido e marcas HTML não correspondentes (`<br />`), juntamente com o código do servidor e as marcas HTML correspondentes. Novamente, você também pode preceder cada linha individualmente com o operador de `@:`; de qualquer forma funciona.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample14.vbhtml)]

    > [!NOTE]
    > Quando você faz a saída de texto conforme mostrado &#8212; nesta seção usando um elemento HTML, o operador `@:` ou o elemento &#8212; `<text>` ASP.net não codifica a saída em HTML. (Como observado anteriormente, o ASP.NET codifica a saída de expressões de código do servidor e os blocos de código do servidor que são precedidos por `@`, exceto nos casos especiais indicados nesta seção.)

### <a name="whitespace"></a>Espaço em branco

Espaços extras em uma instrução (e fora de um literal de cadeia de caracteres) não afetam a instrução:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample15.vbhtml)]

### <a name="breaking-long-statements-into-multiple-lines"></a>Dividindo instruções longas em várias linhas

Você pode dividir uma instrução de código longo em várias linhas usando o caractere de sublinhado `_` (que, em Visual Basic, é chamado de *caractere de continuação*) após cada linha de código. Para quebrar uma instrução na próxima linha, no final da linha, adicione um espaço e, em seguida, o caractere de continuação. Continue a instrução na próxima linha. Você pode encapsular instruções em quantas linhas forem necessárias para melhorar a legibilidade. As seguintes instruções são iguais:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample16.vbhtml)]

No entanto, não é possível encapsular uma linha no meio de um literal de cadeia de caracteres. O exemplo a seguir não funciona:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample17.vbhtml)]

Para combinar uma cadeia de caracteres longa que envolve várias linhas como o código acima, você precisaria usar o *operador de concatenação* (`&`), que você verá mais adiante neste artigo.

### <a name="code-comments"></a>Comentários de código

Os comentários permitem que você deixe anotações para si mesmo ou para outras pessoas. Sintaxe Razor comentários são prefixados com `@*` e terminam com `*@`.

[!code-cshtml[Main](introducing-razor-syntax-vb/samples/sample18.cshtml)]

Nos blocos de código, você pode usar os comentários de sintaxe Razor ou pode usar o caractere de comentário de Visual Basic comum, que é uma aspa simples (`'`) prefixada para cada linha.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample19.vbhtml)]

## <a name="variables"></a>variáveis

Uma variável é um objeto nomeado que você usa para armazenar dados. Você pode nomear variáveis como qualquer coisa, mas o nome deve começar com um caractere alfabético e não pode conter espaços em branco ou caracteres reservados. Em Visual Basic, como vimos anteriormente, o caso das letras em um nome de variável não importa.

### <a name="variables-and-data-types"></a>Variáveis e tipos de dados

Uma variável pode ter um tipo de dados específico, que indica que tipo de dados é armazenado na variável. Você pode ter variáveis de cadeia de caracteres que armazenam valores de cadeia de caracteres (como &quot;Olá, mundo&quot;), variáveis de inteiro que armazenam valores de número inteiro (como 3 ou 79) e variáveis de data que armazenam valores de data em uma variedade de formatos (como 4/12/2012 ou 2009 de março). E há muitos outros tipos de dados que você pode usar.

No entanto, você não precisa especificar um tipo para uma variável. Na maioria dos casos, ASP.NET pode descobrir o tipo com base em como os dados na variável estão sendo usados. (Ocasionalmente, você deve especificar um tipo; você verá exemplos em que isso é verdadeiro.)

Para declarar uma variável sem especificar um tipo, use `Dim` mais o nome da variável (por exemplo, `Dim myVar`). Para declarar uma variável com um tipo, use `Dim` mais o nome da variável, seguido por `As` e, em seguida, o nome do tipo (por exemplo, `Dim myVar As String`).

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample20.vbhtml)]

O exemplo a seguir mostra algumas expressões embutidas que usam as variáveis em uma página da Web.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample21.vbhtml)]

O resultado exibido em um navegador:

![Razor-Img9](introducing-razor-syntax-vb/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>Convertendo e testando tipos de dados

Embora ASP.NET normalmente possa determinar um tipo de dados automaticamente, às vezes ele não pode. Portanto, talvez seja necessário ajudar a ASP.NET com a execução de uma conversão explícita. Mesmo que você não precise converter tipos, às vezes é útil testar para ver de que tipo de dados você pode estar trabalhando.

O caso mais comum é que você precisa converter uma cadeia de caracteres em outro tipo, como em um inteiro ou data. O exemplo a seguir mostra um caso típico em que você deve converter uma cadeia de caracteres em um número.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample22.vbhtml)]

Como regra, a entrada do usuário chega a você como cadeias de caracteres. Mesmo que você tenha solicitado ao usuário que insira um número e, mesmo que tenha inserido um dígito, quando a entrada do usuário é enviada e você a lê no código, os dados estão no formato de cadeia de caracteres. Portanto, você deve converter a cadeia de caracteres em um número. No exemplo, se você tentar executar aritmética nos valores sem convertê-los, os seguintes resultados de erro, porque ASP.NET não pode adicionar duas cadeias de caracteres:

`Cannot implicitly convert type 'string' to 'int'.`

Para converter os valores em inteiros, você chama o método `AsInt`. Se a conversão for bem-sucedida, você poderá adicionar os números.

A tabela a seguir lista alguns métodos comuns de conversão e teste para variáveis.

:::row:::
    :::column:::
        <strong>Método</strong>
    :::column-end:::
    :::column:::
        <strong>Descrição</strong>
    :::column-end:::
    :::column:::
        <strong>Exemplo</strong>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsInt(), IsInt()`
    :::column-end:::
    :::column:::
        Converte uma cadeia de caracteres que representa um número inteiro (como &quot;593&quot;) em um número inteiro.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample23.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsBool(), IsBool()`
    :::column-end:::
    :::column:::
        Converte uma cadeia de caracteres como &quot;true&quot; ou &quot;false&quot; em um tipo Boolean.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample24.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsFloat(), IsFloat()`
    :::column-end:::
    :::column:::
        Converte uma cadeia de caracteres que tem um valor decimal como &quot;1,3&quot; ou &quot;7,439&quot; para um número de ponto flutuante.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample25.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsDecimal(), IsDecimal()`
    :::column-end:::
    :::column:::
        Converte uma cadeia de caracteres que tem um valor decimal como &quot;1,3&quot; ou &quot;7,439&quot; em um número decimal. (Em ASP.NET, um número decimal é mais preciso do que um número de ponto flutuante.)
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample26.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsDateTime(), IsDateTime()`
    :::column-end:::
    :::column:::
        Converte uma cadeia de caracteres que representa um valor de data e hora para o tipo de `DateTime` ASP.NET.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample27.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `ToString()`
    :::column-end:::
    :::column:::
        Converte qualquer outro tipo de dados em uma cadeia de caracteres.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)]
    :::column-end:::
:::row-end:::

## <a name="operators"></a>Operadores

Um operador é uma palavra-chave ou um caractere que informa ASP.NET que tipo de comando executar em uma expressão. O Visual Basic dá suporte a muitos operadores, mas você só precisa reconhecer alguns para começar a desenvolver páginas da Web do ASP.NET. A tabela a seguir resume os operadores mais comuns.

:::row:::
    :::column:::
        <strong>Operador</strong>
    :::column-end:::
    :::column:::
        <strong>Descrição</strong>
    :::column-end:::
    :::column:::
        <strong>Exemplos</strong>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+ - * /`
    :::column-end:::
    :::column:::
        Operadores matemáticos usados em expressões numéricas.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample29.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `=`
    :::column-end:::
    :::column:::
        Atribuição e igualdade. Dependendo do contexto, o atribui o valor no lado direito de uma instrução ao objeto no lado esquerdo ou verifica os valores de igualdade.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample30.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `<>`
    :::column-end:::
    :::column:::
        Desigualdade. Retorna `True` se os valores não forem iguais.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample31.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `< > <= >=`
    :::column-end:::
    :::column:::
        Menor que, maior que, menor ou igual a ou maior que ou igual a.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample32.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `&`
    :::column-end:::
    :::column:::
        Concatenação, que é usada para unir cadeias de caracteres.
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample33.vbhtml)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+= -=`
    :::column-end:::
    :::column:::
        Os operadores de incremento e decremento, que adicionam e subtraim 1 (respectivamente) de uma variável.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample34.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `.`
    :::column-end:::
    :::column:::
        Final. Usado para distinguir objetos e suas propriedades e métodos.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample35.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `()`
    :::column-end:::
    :::column:::
        Parênteses. Usado para agrupar expressões, para passar parâmetros para métodos e para acessar membros de matrizes e coleções.
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample36.vbhtml)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `Not`
    :::column-end:::
    :::column:::
        Válido. Reverte um valor verdadeiro para false e vice-versa. Normalmente usado como uma maneira abreviada de testar `False` (ou seja, para não `True`).
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample37.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AndAlso OrElse`
    :::column-end:::
    :::column:::
        AND lógico e OR, que são usados para vincular condições juntas.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)]
    :::column-end:::
:::row-end:::

## <a name="working-with-file-and-folder-paths-in-code"></a>Trabalhando com caminhos de arquivos e pastas no código

Com freqüência, você trabalhará com caminhos de arquivos e pastas em seu código. Aqui está um exemplo de estrutura de pasta física para um site como pode aparecer em seu computador de desenvolvimento:

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

Aqui estão alguns detalhes essenciais sobre URLs e caminhos:

- Uma URL começa com um nome de domínio (`http://www.example.com`) ou um nome de servidor (`http://localhost`, `http://mycomputer`).
- Uma URL corresponde a um caminho físico em um computador host. Por exemplo, `http://myserver` pode corresponder à pasta *C:\websites\mywebsite* no servidor.
- Um caminho virtual é abreviado para representar caminhos no código sem precisar especificar o caminho completo. Ele inclui a parte de uma URL que segue o nome do domínio ou do servidor. Ao usar caminhos virtuais, você pode mover seu código para um domínio ou servidor diferente sem precisar atualizar os caminhos.

Aqui está um exemplo para ajudá-lo a entender as diferenças:

| URL completa | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| Nome do servidor | *mycompanyserver* |
| Caminho virtual | */humanresources/CompanyPolicy.htm* |
| Caminho físico | *C:\mywebsites\humanresources\CompanyPolicy.htm* |

A raiz virtual é/, assim como a raiz da sua unidade C: é \. (Os caminhos de pastas virtuais sempre usam barras "/".) O caminho virtual de uma pasta não precisa ter o mesmo nome que a pasta física; pode ser um alias. (Em servidores de produção, o caminho virtual raramente corresponde a um caminho físico exato.)

Quando você trabalha com arquivos e pastas no código, às vezes você precisa referenciar o caminho físico e, às vezes, um caminho virtual, dependendo de quais objetos você está trabalhando. O ASP.NET fornece essas ferramentas para trabalhar com caminhos de arquivos e pastas no código: o método `Server.MapPath` e o operador `~` e o método `Href`.

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a>Convertendo caminhos virtuais em físicos: o método Server. MapPath

O método `Server.MapPath` converte um caminho virtual (como */Default.cshtml*) em um caminho físico absoluto (como *C:\WebSites\MyWebSiteFolder\default.cshtml*). Você usa esse método sempre que precisar de um caminho físico completo. Um exemplo típico é quando você está lendo ou gravando um arquivo de texto ou de imagem no servidor Web.

Normalmente, você não sabe o caminho físico absoluto do seu site no servidor de um site de hospedagem, portanto, esse método pode converter o caminho que você conhece — o caminho virtual — para o caminho correspondente no servidor para você. Você passa o caminho virtual para um arquivo ou pasta para o método e retorna o caminho físico:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample39.vbhtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>Referenciando a raiz virtual: o operador ~ e o método href

Em um arquivo *. cshtml* ou *. vbhtml* , você pode fazer referência ao caminho raiz virtual usando o operador de `~`. Isso é muito útil porque você pode mover páginas em um site e quaisquer links que eles contêm para outras páginas não serão quebrados. Também é útil caso você sempre mova seu site para um local diferente. Estes são alguns exemplos:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample40.vbhtml)]

Se o site for `http://myserver/myapp`, aqui está como o ASP.NET tratará esses caminhos quando a página for executada:

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

(Você realmente não verá esses caminhos como os valores da variável, mas ASP.NET tratará os caminhos como se fosse isso.)

Você pode usar o operador de `~` no código do servidor (como acima) e na marcação, desta forma:

[!code-html[Main](introducing-razor-syntax-vb/samples/sample41.html)]

Na marcação, você usa o operador `~` para criar caminhos para recursos como arquivos de imagem, outras páginas da Web e arquivos CSS. Quando a página é executada, o ASP.NET examina a página (código e marcação) e resolve todas as referências de `~` para o caminho apropriado.

## <a name="conditional-logic-and-loops"></a>Lógica condicional e loops

O código do servidor ASP.NET permite executar tarefas com base em condições e escrever código que repete instruções em um número específico de vezes, o código que executa um loop).

### <a name="testing-conditions"></a>Condições de teste

Para testar uma condição simples, use a instrução `If...Then`, que retorna `True` ou `False` com base em um teste que você especificar:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample42.vbhtml)]

A palavra-chave `If` inicia um bloco. O teste real (condição) segue a palavra-chave `If` e retorna true ou false. A instrução `If` termina com `Then`. As instruções que serão executadas se o teste for true serão delimitadas por `If` e `End If`. Uma instrução `If` pode incluir um bloco `Else` que especifica as instruções a serem executadas se a condição for falsa:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample43.vbhtml)]

Se uma instrução `If` iniciar um bloco de código, você não precisará usar as instruções `Code...End Code` normais para incluir os blocos. Você pode apenas adicionar `@` ao bloco e ele funcionará. Essa abordagem funciona com `If`, bem como outras palavras-chave de programação de Visual Basic que são seguidas por blocos de código, incluindo `For`, `For Each`, `Do While`, etc.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample44.vbhtml)]

Você pode adicionar várias condições usando um ou mais blocos de `ElseIf`:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample45.vbhtml)]

Neste exemplo, se a primeira condição no bloco de `If` não for verdadeira, a condição de `ElseIf` será verificada. Se essa condição for atendida, as instruções no bloco de `ElseIf` serão executadas. Se nenhuma das condições for atendida, as instruções no bloco de `Else` serão executadas. Você pode adicionar qualquer número de blocos de `ElseIf` e, em seguida, fechar com um bloco de `Else` como a condição de &quot;todo o resto&quot;.

Para testar um grande número de condições, use um bloco de `Select Case`:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample46.vbhtml)]

O valor a ser testado está entre parênteses (no exemplo, a variável de dia da semana). Cada teste individual usa uma instrução `Case` que lista um valor. Se o valor de uma instrução `Case` corresponder ao valor de teste, o código nesse bloco de `Case` será executado.

O resultado dos dois últimos blocos condicionais exibidos em um navegador:

![Razor-Img10](introducing-razor-syntax-vb/_static/image10.jpg)

### <a name="looping-code"></a>Código de loop

Muitas vezes, você precisa executar as mesmas instruções repetidamente. Faça isso fazendo um loop. Por exemplo, você geralmente executa as mesmas instruções para cada item em uma coleção de dados. Se você souber exatamente quantas vezes deseja executar um loop, você pode usar um loop `For`. Esse tipo de loop é especialmente útil para contar ou contar para baixo:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample47.vbhtml)]

O loop começa com a palavra-chave `For`, seguida por três elementos:

- Imediatamente após a instrução `For`, você declara uma variável de contador (você não precisa usar `Dim`) e, em seguida, indica o intervalo, como em `i = 10 to 20`. Isso significa que a variável `i` começará a contar em 10 e continuará até atingir 20 (inclusivo).
- Entre as instruções `For` e `Next` é o conteúdo do bloco. Isso pode conter uma ou mais instruções de código que são executadas com cada loop.
- A instrução `Next i` encerra o loop. Ele incrementa o contador e inicia a próxima iteração do loop.

A linha de código entre as linhas `For` e `Next` contém o código que é executado para cada iteração do loop. A marcação cria um novo parágrafo (`<p>` elemento) a cada vez e adiciona uma linha à saída, exibindo o valor de i (o contador). Quando você executa essa página, o exemplo cria 11 linhas exibindo a saída, com o texto em cada linha indicando o número do item.

![Razor-Img11](introducing-razor-syntax-vb/_static/image11.jpg)

Se você estiver trabalhando com uma coleção ou matriz, geralmente usará um loop `For Each`. Uma coleção é um grupo de objetos semelhantes, e o loop de `For Each` permite que você execute uma tarefa em cada item da coleção. Esse tipo de loop é conveniente para coleções, porque, ao contrário de um loop de `For`, você não precisa incrementar o contador ou definir um limite. Em vez disso, o código de loop de `For Each` simplesmente prossegue pela coleção até que seja concluído.

Este exemplo retorna os itens na coleção de `Request.ServerVariables` (que contém informações sobre seu servidor Web). Ele usa um loop `For Each` para exibir o nome de cada item criando um novo elemento `<li>` em uma lista com marcadores HTML.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample48.vbhtml)]

A palavra-chave `For Each` é seguida por uma variável que representa um único item na coleção (no exemplo, `myItem`), seguido pela palavra-chave `In`, seguida pela coleção na qual você deseja executar o loop. No corpo do loop de `For Each`, você pode acessar o item atual usando a variável que você declarou anteriormente.

![Razor-Img12](introducing-razor-syntax-vb/_static/image12.jpg)

Para criar um loop de finalidade mais geral, use a instrução `Do While`:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample49.vbhtml)]

Esse loop começa com a palavra-chave `Do While`, seguida por uma condição, seguida pelo bloco a ser repetido. Os loops normalmente incrementam (adicionam) ou decrementam (subtraim de) uma variável ou objeto usado para contagem. No exemplo, o operador de `+=` adiciona 1 ao valor de uma variável toda vez que o loop é executado. (Para diminuir uma variável em um loop que conta inativo, você usaria o operador decremento `-=`.)

## <a name="objects-and-collections"></a>Objetos e coleções

Quase tudo em um site ASP.NET é um objeto, incluindo a própria página da Web. Esta seção aborda alguns objetos importantes com os quais você trabalhará com frequência em seu código.

### <a name="page-objects"></a>Objetos de página

O objeto mais básico em ASP.NET é a página. Você pode acessar as propriedades do objeto Page diretamente sem nenhum objeto qualificado. O código a seguir obtém o caminho do arquivo da página, usando o objeto `Request` da página:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample50.vbhtml)]

Você pode usar as propriedades do objeto `Page` para obter muitas informações, como:

- `Request`. Como você já viu, trata-se de uma coleção de informações sobre a solicitação atual, incluindo que tipo de navegador fez a solicitação, a URL da página, a identidade do usuário, etc.
- `Response`. Esta é uma coleção de informações sobre a resposta (página) que será enviada ao navegador quando o código do servidor terminar a execução. Por exemplo, você pode usar essa propriedade para gravar informações na resposta.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample51.vbhtml)]

### <a name="collection-objects-arrays-and-dictionaries"></a>Objetos de coleção (matrizes e dicionários)

Uma coleção é um grupo de objetos do mesmo tipo, como uma coleção de objetos `Customer` de um banco de dados. ASP.NET contém muitas coleções internas, como a coleção de `Request.Files`.

Você geralmente trabalhará com dados em coleções. Dois tipos de coleção comuns são a *matriz* e o *dicionário*. Uma matriz é útil quando você deseja armazenar uma coleção de itens semelhantes, mas não quer criar uma variável separada para conter cada item:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample52.vbhtml)]

Com matrizes, você declara um tipo de dados específico, como `String`, `Integer`ou `DateTime`. Para indicar que a variável pode conter uma matriz, você adiciona parênteses ao nome da variável na declaração (como `Dim myVar() As String`). Você pode acessar itens em uma matriz usando sua posição (índice) ou usando a instrução `For Each`. Os índices de matriz são baseados &#8212; em zero ou seja, o primeiro item está na posição 0, o segundo item está na posição 1 e assim por diante.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample53.vbhtml)]

Você pode determinar o número de itens em uma matriz obtendo sua propriedade `Length`. Para obter a posição de um item específico na matriz (ou seja, para pesquisar a matriz), use o método `Array.IndexOf`. Você também pode fazer coisas como reverter o conteúdo de uma matriz (o método `Array.Reverse`) ou classificar o conteúdo (o método `Array.Sort`).

A saída do código da matriz de cadeia de caracteres exibido em um navegador:

![Razor-Img13](introducing-razor-syntax-vb/_static/image13.jpg)

Um dicionário é uma coleção de pares de chave/valor, em que você fornece a chave (ou nome) para definir ou recuperar o valor correspondente:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample54.vbhtml)]

Para criar um dicionário, use a palavra-chave `New` para indicar que você está criando um novo objeto de `Dictionary`. Você pode atribuir um dicionário a uma variável usando a palavra-chave `Dim`. Você indica os tipos de dados dos itens no dicionário usando parênteses (`( )`). No final da declaração, você deve adicionar outro par de parênteses, pois esse é, na verdade, um método que cria um novo dicionário.

Para adicionar itens ao dicionário, você pode chamar o método `Add` da variável Dictionary (`myScores` nesse caso) e, em seguida, especificar uma chave e um valor. Como alternativa, você pode usar parênteses para indicar a chave e fazer uma atribuição simples, como no exemplo a seguir:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample55.vbhtml)]

Para obter um valor do dicionário, especifique a chave entre parênteses:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample56.vbhtml)]

## <a name="calling-methods-with-parameters"></a>Chamando métodos com parâmetros

Como vimos anteriormente neste artigo, os objetos com os quais você programau têm métodos. Por exemplo, um objeto `Database` pode ter um método `Database.Connect`. Muitos métodos também têm um ou mais parâmetros. Um *parâmetro* é um valor que você passa para um método para permitir que o método conclua sua tarefa. Por exemplo, examine uma declaração para o método `Request.MapPath`, que usa três parâmetros:

[!code-vb[Main](introducing-razor-syntax-vb/samples/sample57.vb)]

Esse método retorna o caminho físico no servidor que corresponde a um caminho virtual especificado. Os três parâmetros para o método são `virtualPath`, `baseVirtualDir`e `allowCrossAppMapping`. (Observe que na declaração, os parâmetros são listados com os tipos de dados dos dados que eles aceitarão.) Ao chamar esse método, você deve fornecer valores para todos os três parâmetros.

Quando você estiver usando Visual Basic com a sintaxe Razor, terá duas opções para passar parâmetros para um método: *parâmetros posicionais* ou *parâmetros nomeados*. Para chamar um método usando parâmetros posicionais, você passa os parâmetros em uma ordem estrita que é especificada na declaração do método. (Normalmente, você sabe esse pedido lendo a documentação do método.) Você deve seguir a ordem e não pode ignorar nenhum dos parâmetros &#8212; , se necessário, você passa uma cadeia de caracteres vazia (`""`) ou NULL para um parâmetro posicional para o qual você não tem um valor.

O exemplo a seguir pressupõe que você tenha uma pasta chamada *scripts* em seu site. O código chama o método `Request.MapPath` e passa valores para os três parâmetros na ordem correta. Em seguida, ele exibe o caminho mapeado resultante.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample58.vbhtml)]

Quando há muitos parâmetros para um método, você pode manter o seu código mais limpo e mais legível usando parâmetros nomeados. Para chamar um método usando parâmetros nomeados, especifique o nome do parâmetro seguido por `:=` e, em seguida, forneça o valor. Uma vantagem dos parâmetros nomeados é que você pode adicioná-los em qualquer ordem desejada. (Uma desvantagem é que a chamada de método não é compactada.)

O exemplo a seguir chama o mesmo método mostrado acima, mas usa parâmetros nomeados para fornecer os valores:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample59.vbhtml)]

Como você pode ver, os parâmetros são passados em uma ordem diferente. No entanto, se você executar o exemplo anterior e este exemplo, eles retornarão o mesmo valor.

## <a name="handling-errors"></a>Tratando erros

### <a name="try-catch-statements"></a>Instruções Try-Catch

Muitas vezes, você terá instruções em seu código que podem falhar por motivos fora do seu controle. Por exemplo:

- Se o seu código tentar abrir, criar, ler ou gravar um arquivo, todos os tipos de erros poderão ocorrer. O arquivo desejado talvez não exista, ele pode estar bloqueado, o código pode não ter permissões e assim por diante.
- Da mesma forma, se o seu código tentar atualizar os registros em um banco de dados, poderá haver problemas de permissões, a conexão com o banco de dados pode ser descartada, o que pode ser salvo, e assim por diante.

Em termos de programação, essas situações são chamadas de *exceções*. Se o código encontrar uma exceção, ele gerará (gera) uma mensagem de erro que é, no melhor, irritante para os usuários.

![Razor-Img14](introducing-razor-syntax-vb/_static/image14.jpg)

Em situações em que o código pode encontrar exceções e, para evitar mensagens de erro desse tipo, você pode usar instruções de `Try/Catch`. Na instrução `Try`, você executa o código que está verificando. Em uma ou mais instruções `Catch`, você pode procurar erros específicos (tipos específicos de exceções) que podem ter ocorrido. Você pode incluir tantas `Catch` instruções quantas forem necessárias para procurar erros que você está prevendo.

> [!NOTE]
> Recomendamos que você evite usar o método `Response.Redirect` em instruções `Try/Catch`, pois isso pode causar uma exceção em sua página.

O exemplo a seguir mostra uma página que cria um arquivo de texto na primeira solicitação e, em seguida, exibe um botão que permite ao usuário abrir o arquivo. O exemplo deliberadamente usa um nome de arquivo inadequado para que cause uma exceção. O código inclui `Catch` instruções para duas possíveis exceções: `FileNotFoundException`, que ocorrerá se o nome do arquivo for insatisfatório, e `DirectoryNotFoundException`, que ocorrerá se ASP.NET não conseguir localizar a pasta. (Você pode remover o comentário de uma instrução no exemplo para ver como ela é executada quando tudo funciona corretamente.)

Se o código não tratar a exceção, você verá uma página de erro como a captura de tela anterior. No entanto, a seção `Try/Catch` ajuda a impedir que o usuário veja esses tipos de erros.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample60.vbhtml)]

## <a name="additional-resources"></a>Recursos adicionais

### <a name="reference-documentation"></a>Documentação de referência

- [ASP.NET](https://msdn.microsoft.com/library/ee532866.aspx)
- [Linguagem Visual Basic](https://msdn.microsoft.com/library/2x7h1hfk.aspx)
