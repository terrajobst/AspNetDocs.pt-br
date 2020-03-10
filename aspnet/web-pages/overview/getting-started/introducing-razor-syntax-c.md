---
uid: web-pages/overview/getting-started/introducing-razor-syntax-c
title: Introdução à programação da Web do ASP.NET usando a sintaxeC#do Razor () | Microsoft Docs
author: Rick-Anderson
description: Este capítulo fornece uma visão geral da programação com Páginas da Web do ASP.NET usando o sintaxe Razor. ASP.NET é a tecnologia da Microsoft para executar o PA da Web dinâmico...
ms.author: riande
ms.date: 02/07/2014
ms.assetid: aa67d304-583b-4bf8-a231-195656cfb587
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-c
msc.type: authoredcontent
ms.openlocfilehash: c2f420bb7c2f7d2e31654c20fb9ec7497a30a9f7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78641571"
---
# <a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-c"></a>Introdução à programação da Web do ASP.NET usando a sintaxeC#do Razor ()

por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo fornece uma visão geral da programação com Páginas da Web do ASP.NET usando o sintaxe Razor. ASP.NET é a tecnologia da Microsoft para executar páginas da Web dinâmicas em servidores Web. Este artigo se concentra no uso da C# linguagem de programação.
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

## <a name="the-top-8-programming-tips"></a>As 8 principais dicas de programação

Esta seção lista algumas dicas que você precisa saber ao começar a escrever o código do servidor ASP.NET usando o sintaxe Razor.

> [!NOTE]
> A sintaxe Razor é baseada na linguagem C# de programação, e essa é a linguagem usada com mais frequência com páginas da Web do ASP.net. No entanto, o sintaxe Razor também dá suporte à linguagem de Visual Basic e tudo o que você vê também pode fazer em Visual Basic. Para obter detalhes, consulte o apêndice [Visual Basic linguagem e sintaxe](https://go.microsoft.com/fwlink/?LinkId=202908).

Você pode encontrar mais detalhes sobre a maioria dessas técnicas de programação posteriormente neste artigo.

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1. você adiciona código a uma página usando o caractere @

O caractere `@` inicia expressões embutidas, blocos de instrução simples e blocos de várias instruções:

[!code-html[Main](introducing-razor-syntax-c/samples/sample1.html)]

É assim que essas instruções se parecem quando a página é executada em um navegador:

![Razor-Img1](introducing-razor-syntax-c/_static/image1.jpg)

> [!TIP] 
> 
> **Codificação HTML**
> 
> Quando você exibe o conteúdo em uma página usando o caractere `@`, como nos exemplos anteriores, o ASP.NET HTML codifica a saída. Isso substitui os caracteres HTML reservados (como `<` e `>` e `&`) por códigos que permitem que os caracteres sejam exibidos como caracteres em uma página da Web, em vez de serem interpretados como marcas HTML ou entidades. Sem codificação HTML, a saída do código do servidor pode não ser exibida corretamente e pode expor uma página a riscos de segurança.
> 
> Se sua meta é gerar uma marcação HTML que renderiza marcas como marcação (por exemplo `<p></p>` para um parágrafo ou `<em></em>` para enfatizar o texto), consulte a seção [combinando texto, marcação e código em blocos de código](#BM_CombiningTextMarkupAndCode) mais adiante neste artigo.
> 
> Você pode ler mais sobre codificação HTML em [trabalhando com formulários](https://go.microsoft.com/fwlink/?LinkId=202892).

### <a name="2-you-enclose-code-blocks-in-braces"></a>2. você coloca blocos de código entre chaves

Um *bloco de código* inclui uma ou mais instruções de código e é colocado entre chaves.

[!code-html[Main](introducing-razor-syntax-c/samples/sample2.html)]

O resultado exibido em um navegador:

![Razor-Img2](introducing-razor-syntax-c/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-semicolon"></a>3. dentro de um bloco, você encerra cada instrução de código com um ponto e vírgula

Dentro de um bloco de código, cada instrução de código completa deve terminar com um ponto e vírgula. Expressões embutidas não terminam com ponto e vírgula.

[!code-html[Main](introducing-razor-syntax-c/samples/sample3.html)]

### <a name="4-you-use-variables-to-store-values"></a>4. você usa variáveis para armazenar valores

Você pode armazenar valores em uma *variável*, incluindo cadeias de caracteres, números e datas, etc. Você cria uma nova variável usando a palavra-chave `var`. Você pode inserir valores de variáveis diretamente em uma página usando `@`.

[!code-html[Main](introducing-razor-syntax-c/samples/sample4.html)]

O resultado exibido em um navegador:

![Razor-Img3](introducing-razor-syntax-c/_static/image3.jpg)

<a id="ID_StringLiterals"></a>
### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5. você coloca valores de cadeia de caracteres literais entre aspas duplas

Uma *String* é uma sequência de caracteres que são tratados como texto. Para especificar uma cadeia de caracteres, coloque-a entre aspas duplas:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample5.cshtml)]

Se a cadeia de caracteres que você deseja exibir contiver um caractere de barra invertida (`\`) ou aspas duplas (`"`), use um *literal de cadeia de caracteres textual* prefixado com o operador de `@`. (No C#, o caractere \ tem um significado especial, a menos que você use um literal de cadeia de caracteres textual.)

[!code-html[Main](introducing-razor-syntax-c/samples/sample6.html)]

Para inserir aspas duplas, use um literal de cadeia de caracteres textual e repita as aspas:

[!code-html[Main](introducing-razor-syntax-c/samples/sample7.html)]

Este é o resultado do uso de ambos os exemplos em uma página:

![Razor-Img4](introducing-razor-syntax-c/_static/image4.jpg)

> [!NOTE]
> Observe que o caractere `@` é usado para marcar literais de cadeia de caracteres C# textuais em e para marcar o código em páginas ASP.net.

### <a name="6-code-is-case-sensitive"></a>6. o código diferencia maiúsculas de minúsculas

No C#, palavras-chave (como `var`, `true`e `if`) e nomes de variáveis diferenciam maiúsculas de minúsculas. As linhas de código a seguir criam duas variáveis diferentes, `lastName` e `LastName.`

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample8.cshtml)]

Se você declarar uma variável como `var lastName = "Smith";` e tentar fazer referência a essa variável em sua página como `@LastName`, obteria o valor `"Jones"` em vez de `"Smith"`.

> [!NOTE]
> Em Visual Basic, as palavras-chave e as variáveis *não* diferenciam maiúsculas de minúsculas.

### <a name="7-much-of-your-coding-involves-objects"></a>7. a maior parte da codificação envolve objetos

Um *objeto* representa algo que você pode programar com &#8212; uma página, uma caixa de texto, um arquivo, uma imagem, uma solicitação da Web, uma mensagem de email, um registro de cliente (linha de banco de dados), etc. Os objetos têm propriedades que descrevem suas características e que você pode ler ou &#8212; alterar um objeto de caixa de texto tem uma propriedade de `Text` (entre outros), um objeto de solicitação tem uma propriedade `Url`, uma mensagem de email tem uma propriedade `From` e um objeto Customer tem uma propriedade `FirstName`. Os objetos também têm métodos que são os verbos &quot;&quot; podem ser executados. Os exemplos incluem o método de `Save` de um objeto de arquivo, o método `Rotate` de um objeto de imagem e um método de `Send` do objeto de email.

Com freqüência, você trabalhará com o objeto `Request`, que fornece informações como os valores das caixas de texto (campos de formulário) na página, que tipo de navegador fez a solicitação, a URL da página, a identidade do usuário, etc. O exemplo a seguir mostra como acessar as propriedades do objeto `Request` e como chamar o método `MapPath` do objeto `Request`, que fornece o caminho absoluto da página no servidor:

[!code-html[Main](introducing-razor-syntax-c/samples/sample9.html)]

O resultado exibido em um navegador:

![Razor-Img5](introducing-razor-syntax-c/_static/image5.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8. você pode escrever código que toma decisões

Um recurso importante das páginas da Web dinâmicas é que você pode determinar o que fazer com base nas condições. A maneira mais comum de fazer isso é com a instrução de `if` (e a instrução `else` opcional).

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample10.cshtml)]

A instrução `if(IsPost)` é uma maneira abreviada de escrever `if(IsPost == true)`. Juntamente com `if` instruções, há várias maneiras de testar condições, repetir blocos de código e assim por diante, que são descritos posteriormente neste artigo.

O resultado exibido em um navegador (depois de clicar em **Enviar**):

![Razor-Img6](introducing-razor-syntax-c/_static/image6.jpg)

> [!TIP] 
> 
> <a id="SB_HttpGetPost"></a>
> ### <a name="http-get-and-post-methods-and-the-ispost-property"></a>Métodos GET e POST de HTTP e a propriedade IsPost
> 
> O protocolo usado para páginas da Web (HTTP) dá suporte a um número muito limitado de métodos (verbos) que são usados para fazer solicitações ao servidor. Os dois mais comuns são GET, que é usado para ler uma página e POST, que é usado para enviar uma página. Em geral, na primeira vez que um usuário solicita uma página, a página é solicitada usando GET. Se o usuário preencher um formulário e clicar em um botão enviar, o navegador fará uma solicitação POST para o servidor.
> 
> Na programação da Web, geralmente é útil saber se uma página está sendo solicitada como um GET ou como uma POSTAgem para que você saiba como processar a página. No Páginas da Web do ASP.NET, você pode usar a propriedade `IsPost` para ver se uma solicitação é GET ou POST. Se a solicitação for uma POSTAgem, a propriedade `IsPost` retornará true e você poderá fazer coisas como ler os valores das caixas de texto em um formulário. Muitos exemplos que você verá mostram como processar a página de forma diferente, dependendo do valor de `IsPost`.

## <a name="a-simple-code-example"></a>Um exemplo de código simples

Este procedimento mostra como criar uma página que ilustra as técnicas básicas de programação. No exemplo, você cria uma página que permite aos usuários inserir dois números e, em seguida, adiciona-os e exibe o resultado.

1. No editor, crie um novo arquivo e nomeie-o como *AddNumbers. cshtml*.
2. Copie o código e a marcação a seguir para a página, substituindo qualquer coisa que já esteja na página.  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample11.cshtml)]

    Aqui estão algumas coisas que você deve observar:

    - O caractere `@` inicia o primeiro bloco de código na página e precede a variável `totalMessage` que é inserida perto da parte inferior da página.
    - O bloco na parte superior da página é colocado entre chaves.
    - No bloco na parte superior, todas as linhas terminam com um ponto e vírgula.
    - As variáveis `total`, `num1`, `num2`e `totalMessage` armazenam vários números e uma cadeia de caracteres.
    - O valor da cadeia de caracteres literal atribuído à variável `totalMessage` está entre aspas duplas.
    - Como o código diferencia maiúsculas de minúsculas, quando a variável de `totalMessage` é usada perto da parte inferior da página, seu nome deve corresponder à variável na parte superior exata.
    - A expressão `num1.AsInt() + num2.AsInt()` mostra como trabalhar com objetos e métodos. O método `AsInt` em cada variável converte a cadeia de caracteres inserida por um usuário em um número (um inteiro) para que você possa executar aritmética nela.
    - A marca de `<form>` inclui um atributo de `method="post"`. Isso especifica que quando o usuário clicar em **Adicionar**, a página será enviada ao servidor usando o método http post. Quando a página é enviada, o teste de `if(IsPost)` é avaliado como true e o código condicional é executado, exibindo o resultado da adição dos números.
3. Salve a página e execute-a em um navegador. (Verifique se a página está selecionada no espaço de trabalho **arquivos** antes de executá-la.) Insira dois números inteiros e, em seguida, clique no botão **Adicionar** . 

    ![Razor-Img7](introducing-razor-syntax-c/_static/image7.jpg)

## <a name="basic-programming-concepts"></a>Conceitos básicos de programação

Este artigo fornece uma visão geral da programação da Web do ASP.NET. Não é uma análise exaustiva, apenas um tour rápido pelos conceitos de programação que você usará com mais frequência. Mesmo assim, ele abrange quase tudo o que você precisará para começar a usar Páginas da Web do ASP.NET.

Mas primeiro, uma pequena experiência técnica.

### <a name="the-razor-syntax-server-code-and-aspnet"></a>A sintaxe do Razor, o código do servidor e o ASP.NET

Sintaxe Razor é uma sintaxe de programação simples para inserir código baseado em servidor em uma página da Web. Em uma página da Web que usa o sintaxe Razor, há dois tipos de conteúdo: conteúdo do cliente e código do servidor. O conteúdo do cliente é o que você está acostumado a usar em páginas da Web: marcação HTML (elementos), informações de estilo como CSS, talvez algum script de cliente como JavaScript e texto sem formatação.

Sintaxe Razor permite adicionar código de servidor a esse conteúdo de cliente. Se houver código de servidor na página, o servidor executará esse código primeiro, antes de enviar a página para o navegador. Ao executar no servidor, o código pode executar tarefas que podem ser muito mais complexas de usar o conteúdo do cliente, como acessar bancos de dados baseados em servidor. O mais importante é que o código do servidor pode &#8212; criar dinamicamente o conteúdo do cliente, ele pode gerar marcação HTML ou outro conteúdo imediatamente e, em seguida, enviá-lo ao navegador junto com qualquer HTML estático que a página possa conter. Da perspectiva do navegador, o conteúdo do cliente gerado pelo código do servidor não é diferente de qualquer outro conteúdo de cliente. Como você já viu, o código do servidor necessário é bem simples.

ASP.NET páginas da Web que incluem o sintaxe Razor ter uma extensão de arquivo especial ( *. cshtml* ou *. vbhtml*). O servidor reconhece essas extensões, executa o código marcado com sintaxe Razor e, em seguida, envia a página para o navegador.

### <a name="where-does-aspnet-fit-in"></a>Onde o ASP.NET se encaixa?

O sintaxe Razor se baseia em uma tecnologia da Microsoft chamada ASP.NET, que por sua vez é baseado na estrutura de Microsoft .NET. A estrutura The.NET é uma estrutura de programação grande e abrangente da Microsoft para o desenvolvimento de praticamente qualquer tipo de aplicativo de computador. ASP.NET é a parte da .NET Framework projetada especificamente para criar aplicativos Web. Os desenvolvedores usaram o ASP.NET para criar muitos dos maiores e mais grandes sites de tráfego do mundo. (Sempre que você vir a extensão de nome de arquivo *. aspx* como parte da URL em um site, saberá que o site foi escrito usando ASP.net.)

O sintaxe Razor dá a você toda a potência do ASP.NET, mas usando uma sintaxe simplificada que é mais fácil de aprender se você for um principiante e isso o tornará mais produtivo se você for um especialista. Embora essa sintaxe seja simples de usar, sua relação de família com ASP.NET e a .NET Framework significa que, à medida que seus sites se tornam mais sofisticados, você tem o poder das estruturas maiores disponíveis para você.

![Razor-Img8](introducing-razor-syntax-c/_static/image8.jpg)

> [!TIP] 
> 
> **Classes e instâncias**
> 
> O código do servidor ASP.NET usa objetos que, por sua vez, se baseiam na ideia de classes. A classe é a definição ou o modelo de um objeto. Por exemplo, um aplicativo pode conter uma classe `Customer` que define as propriedades e os métodos de que qualquer objeto de cliente precisa.
> 
> Quando o aplicativo precisa trabalhar com informações reais do cliente, ele cria uma instância de (ou *instancia*) um objeto de cliente. Cada cliente individual é uma instância separada da classe `Customer`. Cada instância dá suporte às mesmas propriedades e métodos, mas os valores de propriedade para cada instância são normalmente diferentes, pois cada objeto de cliente é exclusivo. Em um objeto Customer, a propriedade `LastName` pode ser "Smith"; em outro objeto de cliente, a propriedade `LastName` pode ser "Jones".
> 
> Da mesma forma, qualquer página da Web individual em seu site é um objeto `Page` que é uma instância da classe `Page`. Um botão na página é um objeto `Button` que é uma instância da classe `Button` e assim por diante. Cada instância tem suas próprias características, mas todas elas se baseiam no que é especificado na definição de classe do objeto.

## <a name="basic-syntax"></a>Sintaxe básica

Anteriormente, você viu um exemplo básico de como criar uma página de Páginas da Web do ASP.NET e como você pode adicionar código de servidor à marcação HTML. Aqui, você aprenderá as noções básicas de como escrever código de servidor &#8212; ASP.NET usando o sintaxe Razor ou seja, as regras de linguagem de programação.

Se você tiver experiência com programação (especialmente se tiver usado C, C++, C#, Visual Basic ou JavaScript), grande parte do que você lê aqui será familiar. Você provavelmente precisará se familiarizar apenas com o modo como o código do servidor é adicionado à marcação em arquivos *. cshtml* .

<a id="BM_CombiningTextMarkupAndCode"></a>
### <a name="combining-text-markup-and-code-in-code-blocks"></a>Combinando texto, marcação e código em blocos de código

Em blocos de código de servidor, muitas vezes você deseja produzir texto ou marcação (ou ambos) para a página. Se um bloco de código de servidor contiver texto que não seja código e que, em vez disso, deve ser renderizado como está, ASP.NET precisará ser capaz de distinguir esse texto do código. Há várias maneiras de fazer isso.

- Coloque o texto em um elemento HTML como `<p></p>` ou `<em></em>`:   

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample12.cshtml)]

    O elemento HTML pode incluir texto, elementos HTML adicionais e expressões de código de servidor. Quando ASP.NET vê a marca HTML de abertura (por exemplo, `<p>`), ela processa tudo, incluindo o elemento e seu conteúdo como se encontra no navegador, resolvendo as expressões de código do servidor como ele vai.
- Use o operador `@:` ou o elemento `<text>`. O `@:` gera uma única linha de conteúdo que contém texto sem formatação ou marcas HTML não correspondentes; o elemento `<text>` inclui várias linhas na saída. Essas opções são úteis quando você não deseja renderizar um elemento HTML como parte da saída.  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample13.cshtml)]

    Se você quiser gerar várias linhas de texto ou marcas HTML sem correspondência, você pode preceder cada linha com `@:`, ou pode colocar a linha em um elemento `<text>`. Assim como o operador de `@:`, as marcas de`<text>` são usadas pelo ASP.NET para identificar o conteúdo de texto e nunca são renderizadas na saída da página.

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample14.cshtml)]

    O primeiro exemplo repete o exemplo anterior, mas usa um único par de marcas `<text>` para colocar o texto a ser renderizado. No segundo exemplo, as marcas `<text>` e `</text>` incluem três linhas, todas com texto não contido e marcas HTML não correspondentes (`<br />`), juntamente com o código do servidor e as marcas HTML correspondentes. Novamente, você também pode preceder cada linha individualmente com o operador de `@:`; de qualquer forma funciona.

    > [!NOTE]
    > Quando você faz a saída de texto conforme mostrado &#8212; nesta seção usando um elemento HTML, o operador `@:` ou o elemento &#8212; `<text>` ASP.net não codifica a saída em HTML. (Como observado anteriormente, o ASP.NET codifica a saída de expressões de código do servidor e os blocos de código do servidor que são precedidos por `@`, exceto nos casos especiais indicados nesta seção.)

### <a name="whitespace"></a>Espaço em branco

Espaços extras em uma instrução (e fora de um literal de cadeia de caracteres) não afetam a instrução:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample15.cshtml)]

Uma quebra de linha em uma instrução não tem efeito sobre a instrução e você pode encapsular instruções para facilitar a leitura. As seguintes instruções são iguais:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample16.cshtml)]

No entanto, não é possível encapsular uma linha no meio de um literal de cadeia de caracteres. O exemplo a seguir não funciona:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample17.cshtml)]

Para combinar uma cadeia de caracteres longa que envolve várias linhas como o código acima, há duas opções. Você pode usar o operador de concatenação (`+`), que será exibido posteriormente neste artigo. Você também pode usar o caractere `@` para criar um literal de cadeia de caracteres textual, como visto anteriormente neste artigo. Você pode quebrar literais de cadeia de caracteres idênticas entre as linhas:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample18.cshtml)]

### <a name="code-and-markup-comments"></a>Comentários de código (e marcação)

Os comentários permitem que você deixe anotações para si mesmo ou para outras pessoas. Eles também permitem que você desabilite (*comente*) uma seção de código ou marcação que você não deseja executar, mas deseja manter em sua página por enquanto.

Há uma sintaxe de comentário diferente para o código do Razor e para marcação HTML. Assim como acontece com todo o código do Razor, os comentários do Razor são processados (e, em seguida, removidos) no servidor antes que a página seja enviada para o navegador. Portanto, a sintaxe de comentários do Razor permite que você coloque comentários no código (ou até mesmo na marcação) que você pode ver ao editar o arquivo, mas que os usuários não veem, mesmo na origem da página.

Para comentários do ASP.NET Razor, você inicia o comentário com `@*` e o termina com `*@`. O comentário pode estar em uma linha ou em várias linhas:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample19.cshtml)]

Este é um comentário dentro de um bloco de código:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample20.cshtml)]

Aqui está o mesmo bloco de código, com a linha de código comentada para que ele não seja executado:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample21.cshtml)]

Dentro de um bloco de código, como uma alternativa ao uso da sintaxe de comentário do Razor, você pode usar a sintaxe de comentários da linguagem de programação que C#você está usando, como:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample22.cshtml)]

No C#, os comentários de linha única são precedidos pelo `//` caracteres e os comentários de várias linhas começam com `/*` e terminam com `*/`. (Assim como os comentários do C# Razor, os comentários não são renderizados para o navegador.)

Para marcação, como você provavelmente sabe, você pode criar um comentário HTML:

[!code-xml[Main](introducing-razor-syntax-c/samples/sample23.xml)]

Os comentários HTML começam com `<!--` caracteres e terminam com `-->`. Você pode usar comentários HTML para colocar não apenas texto, mas também qualquer marcação HTML que talvez você queira manter na página, mas não quer renderizar. Este comentário HTML ocultará todo o conteúdo das marcas e o texto que eles contêm:

[!code-html[Main](introducing-razor-syntax-c/samples/sample24.html)]

Ao contrário dos comentários do Razor, os comentários HTML *são* renderizados para a página e o usuário pode vê-los exibindo a fonte da página.

O Razor tem limitações em blocos aninhados do C#. Para obter mais informações [, C# consulte variáveis nomeadas e blocos aninhados geram código quebrado](http://aspnetwebstack.codeplex.com/workitem/1914)

## <a name="variables"></a>variáveis

Uma variável é um objeto nomeado que você usa para armazenar dados. Você pode nomear variáveis como qualquer coisa, mas o nome deve começar com um caractere alfabético e não pode conter espaços em branco ou caracteres reservados.

### <a name="variables-and-data-types"></a>Variáveis e tipos de dados

Uma variável pode ter um tipo de dados específico, que indica que tipo de dados é armazenado na variável. Você pode ter variáveis de cadeia de caracteres que armazenam valores de cadeia de caracteres (como &quot;Olá, mundo&quot;), variáveis de inteiro que armazenam valores de número inteiro (como 3 ou 79) e variáveis de data que armazenam valores de data em uma variedade de formatos (como 4/12/2012 ou 2009 de março). E há muitos outros tipos de dados que você pode usar.

No entanto, geralmente não é necessário especificar um tipo para uma variável. Na maioria das vezes, o ASP.NET pode descobrir o tipo com base em como os dados na variável estão sendo usados. (Ocasionalmente, você deve especificar um tipo; você verá exemplos em que isso é verdadeiro.)

Você declara uma variável usando a palavra-chave `var` (se não quiser especificar um tipo) ou usando o nome do tipo:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample25.cshtml)]

O exemplo a seguir mostra alguns usos típicos de variáveis em uma página da Web:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample26.cshtml)]

Se você combinar os exemplos anteriores em uma página, verá isso exibido em um navegador:

![Razor-Img9](introducing-razor-syntax-c/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>Convertendo e testando tipos de dados

Embora ASP.NET normalmente possa determinar um tipo de dados automaticamente, às vezes ele não pode. Portanto, talvez seja necessário ajudar a ASP.NET com a execução de uma conversão explícita. Mesmo que você não precise converter tipos, às vezes é útil testar para ver de que tipo de dados você pode estar trabalhando.

O caso mais comum é que você precisa converter uma cadeia de caracteres em outro tipo, como em um inteiro ou data. O exemplo a seguir mostra um caso típico em que você deve converter uma cadeia de caracteres em um número.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample27.cshtml)]

Como regra, a entrada do usuário chega a você como cadeias de caracteres. Mesmo que você tenha solicitado aos usuários que insiram um número e, mesmo que eles tenham inserido um dígito, quando a entrada do usuário for enviada e você lê-lo no código, os dados estão no formato de cadeia de caracteres. Portanto, você deve converter a cadeia de caracteres em um número. No exemplo, se você tentar executar aritmética nos valores sem convertê-los, os seguintes resultados de erro, porque ASP.NET não pode adicionar duas cadeias de caracteres:

*Não é possível converter implicitamente o tipo ' String ' em ' int '.*

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
    Converte uma cadeia de caracteres que representa um número inteiro (como "593") em um número inteiro.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample28.cs)]
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
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample29.cs)]
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
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample30.cs)]
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
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample31.cs)]
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
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample32.cs)]
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
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample33.js)]
    :::column-end:::
:::row-end:::

## <a name="operators"></a>Operadores

Um operador é uma palavra-chave ou um caractere que informa ASP.NET que tipo de comando executar em uma expressão. O C# idioma (e o sintaxe Razor que se baseia nele) dá suporte a muitos operadores, mas você só precisa reconhecer alguns para começar. A tabela a seguir resume os operadores mais comuns.

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
        `+` `-` `*` `/`
    :::column-end:::
    :::column:::
    Operadores matemáticos usados em expressões numéricas.
    :::column-end:::
    :::column:::
        [!code-css[Main](introducing-razor-syntax-c/samples/sample34.css)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `=`
    :::column-end:::
    :::column:::
    Atribuição. Atribui o valor no lado direito de uma instrução ao objeto no lado esquerdo.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample35.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `==`
    :::column-end:::
    :::column:::
    Igualdade. Retorna `true` se os valores forem iguais. (Observe a distinção entre o operador de `=` e o operador de `==`.)
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample36.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `!=`
    :::column-end:::
    :::column:::
    Desigualdade. Retorna `true` se os valores não forem iguais.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample37.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `< > <= >=`
    :::column-end:::
    :::column:::
    Menor que, maior que, menor que ou igual a, e maior que ou igual a.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample38.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+`
    :::column-end:::
    :::column:::
    Concatenação, que é usada para unir cadeias de caracteres. ASP.NET sabe a diferença entre esse operador e o operador de adição com base no tipo de dados da expressão.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample39.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+=` `-=`
    :::column-end:::
    :::column:::
    Os operadores de incremento e decremento, que adicionam e subtraim 1 (respectivamente) de uma variável.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample40.cs)]
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
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample41.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `()`
    :::column-end:::
    :::column:::
    Parênteses. Usado para agrupar expressões e para passar parâmetros para métodos.
    :::column-end:::
    :::column:::
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample42.js)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `[]`
    :::column-end:::
    :::column:::
    Colchetes. Usado para acessar valores em matrizes ou coleções.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample43.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `!`
    :::column-end:::
    :::column:::
    Válido. Reverte um valor de `true` para `false` e vice-versa. Normalmente usado como uma maneira abreviada de testar `false` (ou seja, para não `true`).
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample44.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `&&` `||`
    :::column-end:::
    :::column:::
    AND lógico e OR, que são usados para vincular condições juntas.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample45.cs)]
    :::column-end:::
:::row-end:::

<a id="ID_WorkingWithFileAndFolderPaths"></a>
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

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample46.cshtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>Referenciando a raiz virtual: o operador ~ e o método href

Em um arquivo *. cshtml* ou *. vbhtml* , você pode fazer referência ao caminho raiz virtual usando o operador de `~`. Isso é muito útil porque você pode mover páginas em um site e quaisquer links que eles contêm para outras páginas não serão quebrados. Também é útil caso você sempre mova seu site para um local diferente. Estes são alguns exemplos:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample47.cshtml)]

Se o site for `http://myserver/myapp`, aqui está como o ASP.NET tratará esses caminhos quando a página for executada:

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

(Você realmente não verá esses caminhos como os valores da variável, mas ASP.NET tratará os caminhos como se fosse isso.)

Você pode usar o operador de `~` no código do servidor (como acima) e na marcação, desta forma:

[!code-html[Main](introducing-razor-syntax-c/samples/sample48.html)]

Na marcação, você usa o operador `~` para criar caminhos para recursos como arquivos de imagem, outras páginas da Web e arquivos CSS. Quando a página é executada, o ASP.NET examina a página (código e marcação) e resolve todas as referências de `~` para o caminho apropriado.

## <a name="conditional-logic-and-loops"></a>Lógica condicional e loops

O código do servidor ASP.NET permite executar tarefas com base em condições e escrever código que repete instruções um número específico de vezes (ou seja, o código que executa um loop).

### <a name="testing-conditions"></a>Condições de teste

Para testar uma condição simples, use a instrução `if`, que retorna true ou false com base em um teste que você especificar:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample49.cshtml)]

A palavra-chave `if` inicia um bloco. O teste real (condição) está entre parênteses e retorna true ou false. As instruções que são executadas se o teste é true são colocadas entre chaves. Uma instrução `if` pode incluir um bloco `else` que especifica as instruções a serem executadas se a condição for falsa:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample50.cshtml)]

Você pode adicionar várias condições usando um bloco de `else if`:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample51.cshtml)]

Neste exemplo, se a primeira condição no bloco If não for verdadeira, a condição de `else if` será verificada. Se essa condição for atendida, as instruções no bloco de `else if` serão executadas. Se nenhuma das condições for atendida, as instruções no bloco de `else` serão executadas. Você pode adicionar qualquer número de outros blocos If e, em seguida, fechar com um bloco de `else` como o &quot;o restante&quot; condição.

Para testar um grande número de condições, use um bloco de `switch`:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample52.cshtml)]

O valor a ser testado está entre parênteses (no exemplo, a variável `weekday`). Cada teste individual usa uma instrução `case` que termina com dois-pontos (:). Se o valor de uma instrução `case` corresponder ao valor de teste, o código nesse bloco de casos será executado. Você fecha cada instrução case com uma instrução `break`. (Se você se esquecer de incluir a quebra em cada bloco de `case`, o código da próxima instrução `case` será executado também.) Um bloco de `switch` geralmente tem uma instrução `default` como o último caso de uma opção de &quot;tudo mais&quot; que é executada se nenhum dos outros casos for verdadeiro.

O resultado dos dois últimos blocos condicionais exibidos em um navegador:

![Razor-Img10](introducing-razor-syntax-c/_static/image10.jpg)

### <a name="looping-code"></a>Código de loop

Muitas vezes, você precisa executar as mesmas instruções repetidamente. Faça isso fazendo um loop. Por exemplo, você geralmente executa as mesmas instruções para cada item em uma coleção de dados. Se você souber exatamente quantas vezes deseja executar um loop, você pode usar um loop `for`. Esse tipo de loop é especialmente útil para contar ou contar para baixo:

[!code-html[Main](introducing-razor-syntax-c/samples/sample53.html)]

O loop começa com a palavra-chave `for`, seguida por três instruções entre parênteses, cada uma termina com um ponto e vírgula.

- Dentro dos parênteses, a primeira instrução (`var i=10;`) cria um contador e inicializa-o como 10. Você não precisa nomear o contador `i` &#8212; você pode usar qualquer variável. Quando o loop de `for` é executado, o contador é incrementado automaticamente.
- A segunda instrução (`i < 21;`) define a condição para o quanto você deseja contar. Nesse caso, você deseja que ele vá para um máximo de 20 (ou seja, continue enquanto o contador for menor que 21).
- A terceira instrução (`i++`) usa um operador de incremento, que simplesmente especifica que o contador deve ter 1 adicionado a cada vez que o loop é executado.

Dentro das chaves está o código que será executado para cada iteração do loop. A marcação cria um novo parágrafo (`<p>` elemento) a cada vez e adiciona uma linha à saída, exibindo o valor de `i` (o contador). Quando você executa essa página, o exemplo cria 11 linhas exibindo a saída, com o texto em cada linha indicando o número do item.

![Razor-Img11](introducing-razor-syntax-c/_static/image11.jpg)

Se você estiver trabalhando com uma coleção ou matriz, geralmente usará um loop `foreach`. Uma coleção é um grupo de objetos semelhantes, e o loop de `foreach` permite que você execute uma tarefa em cada item da coleção. Esse tipo de loop é conveniente para coleções, porque, ao contrário de um loop de `for`, você não precisa incrementar o contador ou definir um limite. Em vez disso, o código de loop de `foreach` simplesmente prossegue pela coleção até que seja concluído.

Por exemplo, o código a seguir retorna os itens na coleção de `Request.ServerVariables`, que é um objeto que contém informações sobre o servidor Web. Ele usa um loop `foreac` h para exibir o nome de cada item criando um novo elemento `<li>` em uma lista com marcadores HTML.

[!code-html[Main](introducing-razor-syntax-c/samples/sample54.html)]

A palavra-chave `foreach` é seguida por parênteses em que você declara uma variável que representa um único item na coleção (no exemplo, `var item`), seguido pela palavra-chave `in`, seguida pela coleção na qual você deseja executar o loop. No corpo do loop de `foreach`, você pode acessar o item atual usando a variável que você declarou anteriormente.

![Razor-Img12](introducing-razor-syntax-c/_static/image12.jpg)

Para criar um loop de finalidade mais geral, use a instrução `while`:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample55.cshtml)]

Um loop de `while` começa com a palavra-chave `while`, seguido por parênteses em que você especifica por quanto tempo o loop continua (aqui, desde que `countNum` seja menor que 50) e, em seguida, o bloco seja repetido. Os loops normalmente incrementam (adicionam) ou decrementam (subtraim de) uma variável ou objeto usado para contagem. No exemplo, o operador de `+=` adiciona 1 a `countNum` cada vez que o loop é executado. (Para diminuir uma variável em um loop que conta inativo, você usaria o operador decremento `-=`).

## <a name="objects-and-collections"></a>Objetos e coleções

Quase tudo em um site ASP.NET é um objeto, incluindo a própria página da Web. Esta seção aborda alguns objetos importantes com os quais você trabalhará com frequência em seu código.

### <a name="page-objects"></a>Objetos de página

O objeto mais básico em ASP.NET é a página. Você pode acessar as propriedades do objeto Page diretamente sem nenhum objeto qualificado. O código a seguir obtém o caminho do arquivo da página, usando o objeto `Request` da página:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample56.cshtml)]

Para tornar claro que você está fazendo referência a propriedades e métodos no objeto Page atual, você pode, opcionalmente, usar a palavra-chave `this` para representar o objeto Page em seu código. Aqui está o exemplo de código anterior, com `this` adicionado para representar a página:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample57.cshtml)]

Você pode usar as propriedades do objeto `Page` para obter muitas informações, como:

- `Request`. Como você já viu, trata-se de uma coleção de informações sobre a solicitação atual, incluindo que tipo de navegador fez a solicitação, a URL da página, a identidade do usuário, etc.
- `Response`. Esta é uma coleção de informações sobre a resposta (página) que será enviada ao navegador quando o código do servidor terminar a execução. Por exemplo, você pode usar essa propriedade para gravar informações na resposta. 

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample58.cshtml)]

<a id="ID_CollectionsAndObjects"></a>
### <a name="collection-objects-arrays-and-dictionaries"></a>Objetos de coleção (matrizes e dicionários)

Uma *coleção* é um grupo de objetos do mesmo tipo, como uma coleção de objetos `Customer` de um banco de dados. ASP.NET contém muitas coleções internas, como a coleção de `Request.Files`.

Você geralmente trabalhará com dados em coleções. Dois tipos de coleção comuns são a *matriz* e o *dicionário*. Uma matriz é útil quando você deseja armazenar uma coleção de itens semelhantes, mas não quer criar uma variável separada para conter cada item:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample59.cshtml)]

Com matrizes, você declara um tipo de dados específico, como `string`, `int`ou `DateTime`. Para indicar que a variável pode conter uma matriz, você adiciona colchetes à declaração (como `string[]` ou `int[]`). Você pode acessar itens em uma matriz usando sua posição (índice) ou usando a instrução `foreach`. Os índices de matriz são baseados &#8212; em zero ou seja, o primeiro item está na posição 0, o segundo item está na posição 1 e assim por diante.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample60.cshtml)]

Você pode determinar o número de itens em uma matriz obtendo sua propriedade `Length`. Para obter a posição de um item específico na matriz (para pesquisar a matriz), use o método `Array.IndexOf`. Você também pode fazer coisas como reverter o conteúdo de uma matriz (o método `Array.Reverse`) ou classificar o conteúdo (o método `Array.Sort`).

A saída do código da matriz de cadeia de caracteres exibido em um navegador:

![Razor-Img13](introducing-razor-syntax-c/_static/image13.jpg)

Um dicionário é uma coleção de pares de chave/valor, em que você fornece a chave (ou nome) para definir ou recuperar o valor correspondente:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample61.cshtml)]

Para criar um dicionário, use a palavra-chave `new` para indicar que você está criando um novo objeto Dictionary. Você pode atribuir um dicionário a uma variável usando a palavra-chave `var`. Você indica os tipos de dados dos itens no dicionário usando colchetes angulares (`< >`). No final da declaração, você deve adicionar um par de parênteses, porque esse é, na verdade, um método que cria um novo dicionário.

Para adicionar itens ao dicionário, você pode chamar o método `Add` da variável Dictionary (`myScores` nesse caso) e, em seguida, especificar uma chave e um valor. Como alternativa, você pode usar colchetes para indicar a chave e fazer uma atribuição simples, como no exemplo a seguir:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample62.cs)]

Para obter um valor do dicionário, especifique a chave entre colchetes:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample63.cs)]

## <a name="calling-methods-with-parameters"></a>Chamando métodos com parâmetros

Conforme você lê anteriormente neste artigo, os objetos com os quais você programa pode ter métodos. Por exemplo, um objeto `Database` pode ter um método `Database.Connect`. Muitos métodos também têm um ou mais parâmetros. Um *parâmetro* é um valor que você passa para um método para permitir que o método conclua sua tarefa. Por exemplo, examine uma declaração para o método `Request.MapPath`, que usa três parâmetros:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample64.cs)]

(A linha foi encapsulada para torná-la mais legível. Lembre-se de que você pode colocar quebras de linha quase qualquer lugar, exceto as cadeias de caracteres entre aspas.)

Esse método retorna o caminho físico no servidor que corresponde a um caminho virtual especificado. Os três parâmetros para o método são `virtualPath`, `baseVirtualDir`e `allowCrossAppMapping`. (Observe que na declaração, os parâmetros são listados com os tipos de dados dos dados que eles aceitarão.) Ao chamar esse método, você deve fornecer valores para todos os três parâmetros.

O sintaxe Razor fornece duas opções para passar parâmetros para um método: *parâmetros posicionais* e *parâmetros nomeados*. Para chamar um método usando parâmetros posicionais, você passa os parâmetros em uma ordem estrita que é especificada na declaração do método. (Normalmente, você sabe esse pedido lendo a documentação do método.) Você deve seguir a ordem e não pode ignorar nenhum dos parâmetros &#8212; , se necessário, você passa uma cadeia de caracteres vazia (`""`) ou `null` para um parâmetro posicional para o qual você não tenha um valor.

O exemplo a seguir pressupõe que você tenha uma pasta chamada *scripts* em seu site. O código chama o método `Request.MapPath` e passa valores para os três parâmetros na ordem correta. Em seguida, ele exibe o caminho mapeado resultante.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample65.cshtml)]

Quando um método tem muitos parâmetros, você pode manter seu código mais legível usando parâmetros nomeados. Para chamar um método usando parâmetros nomeados, especifique o nome do parâmetro seguido por dois-pontos (:) e, em seguida, o valor. A vantagem dos parâmetros nomeados é que você pode passá-los em qualquer ordem desejada. (Uma desvantagem é que a chamada de método não é compactada.)

O exemplo a seguir chama o mesmo método mostrado acima, mas usa parâmetros nomeados para fornecer os valores:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample66.cshtml)]

Como você pode ver, os parâmetros são passados em uma ordem diferente. No entanto, se você executar o exemplo anterior e este exemplo, eles retornarão o mesmo valor.

<a id="ID_HandlingErrors"></a>
## <a name="handling-errors"></a>Tratando erros

### <a name="try-catch-statements"></a>Instruções Try-Catch

Muitas vezes, você terá instruções em seu código que podem falhar por motivos fora do seu controle. Por exemplo:

- Se o seu código tentar criar ou acessar um arquivo, todos os tipos de erros poderão ocorrer. O arquivo desejado talvez não exista, ele pode estar bloqueado, o código pode não ter permissões e assim por diante.
- Da mesma forma, se o seu código tentar atualizar os registros em um banco de dados, poderá haver problemas de permissões, a conexão com o banco de dados pode ser descartada, o que pode ser salvo, e assim por diante.

Em termos de programação, essas situações são chamadas de *exceções*. Se o código encontrar uma exceção, ele gerará (gera) uma mensagem de erro que é, no melhor, irritante para os usuários:

![Razor-Img14](introducing-razor-syntax-c/_static/image14.jpg)

Em situações em que o código pode encontrar exceções e, para evitar mensagens de erro desse tipo, você pode usar instruções de `try/catch`. Na instrução `try`, você executa o código que está verificando. Em uma ou mais instruções `catch`, você pode procurar erros específicos (tipos específicos de exceções) que podem ter ocorrido. Você pode incluir tantas `catch` instruções quantas forem necessárias para procurar erros que você está prevendo.

> [!NOTE]
> Recomendamos que você evite usar o método `Response.Redirect` em instruções `try/catch`, pois isso pode causar uma exceção em sua página.

O exemplo a seguir mostra uma página que cria um arquivo de texto na primeira solicitação e, em seguida, exibe um botão que permite ao usuário abrir o arquivo. O exemplo deliberadamente usa um nome de arquivo inadequado para que cause uma exceção. O código inclui `catch` instruções para duas possíveis exceções: `FileNotFoundException`, que ocorrerá se o nome do arquivo for insatisfatório, e `DirectoryNotFoundException`, que ocorrerá se ASP.NET não conseguir localizar a pasta. (Você pode remover o comentário de uma instrução no exemplo para ver como ela é executada quando tudo funciona corretamente.)

Se o código não tratar a exceção, você verá uma página de erro como a captura de tela anterior. No entanto, a seção `try/catch` ajuda a impedir que o usuário veja esses tipos de erros.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample67.cshtml)]

## <a name="additional-resources"></a>Recursos adicionais

**Programando com Visual Basic**

[Apêndice: linguagem e sintaxe de Visual Basic](https://go.microsoft.com/fwlink/?LinkId=202908)

**Documentação de Referência**

[ASP.NET](https://msdn.microsoft.com/library/ee532866.aspx)

[C#Idioma](https://msdn.microsoft.com/library/kx37x362.aspx)
