---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
title: Introdução aos fundamentos de programação de Páginas da Web do ASP.NET | Microsoft Docs
author: Rick-Anderson
description: 'Este tutorial fornece uma visão geral de como programar em Páginas da Web do ASP.NET com sintaxe Razor. O que você aprenderá: a sintaxe "Razor" básica que você usa para pr...'
ms.author: riande
ms.date: 06/17/2015
ms.assetid: 7526ed45-a97d-4e8a-8301-01324ef0eff9
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
msc.type: authoredcontent
ms.openlocfilehash: 474de7671ac2931e5ba9ff635d77385403644521
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628474"
---
# <a name="introducing-aspnet-web-pages---programming-basics"></a>Introdução aos fundamentos de programação de Páginas da Web do ASP.NET

por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial fornece uma visão geral de como programar em Páginas da Web do ASP.NET com sintaxe Razor.
> 
> O que você aprenderá:
> 
> - A sintaxe básica "Razor" que você usa para programar em Páginas da Web do ASP.NET.
> - Algum básico C#, que é a linguagem de programação que você usará.
> - Alguns conceitos fundamentais de programação para páginas da Web.
> - Como instalar pacotes (componentes que contêm código predefinido) para usar com seu site.
> - Como usar os *auxiliares* para executar tarefas comuns de programação.
>   
> 
> Recursos/tecnologias abordados:
> 
> - NuGet e o Gerenciador de pacotes.
> - O auxiliar de `Gravatar`.

Este tutorial é principalmente um exercício para apresentar a sintaxe de programação que você usará para Páginas da Web do ASP.NET. Você aprenderá sobre *sintaxe Razor* e código escrito na linguagem de C# programação. Você obteve uma visão dessa sintaxe no tutorial anterior; Neste tutorial, explicaremos a sintaxe mais.

Prometemos que este tutorial envolve a maior parte da programação que você verá em um único tutorial e que é o único tutorial *apenas* sobre programação. Nos tutoriais restantes deste conjunto, você realmente criará páginas que fazem coisas interessantes.

Você também aprenderá sobre os *auxiliares*. Um auxiliar é um componente — um trecho de código de pacote — que você pode adicionar a uma página. O auxiliar realiza o trabalho para você que, de outra forma, pode ser entediante ou complexo de ser feito manualmente.

## <a name="creating-a-page-to-play-with-razor"></a>Criando uma página para reprodução com o Razor

Nesta seção, você executará um pouco com o Razor para que possa ter uma noção da sintaxe básica.

Inicie o WebMatrix se ele ainda não estiver em execução. Você usará o site criado no tutorial anterior ([introdução com páginas da Web](https://go.microsoft.com/fwlink/?LinkId=251578)). Para reabri-lo, clique em **meus sites** e escolha **WebPageMovies**:

![Tela inicial do WebMatrix mostrando as opções de site aberto e meus sites realçadas](intro-to-web-pages-programming/_static/image1.png)

Selecione o espaço de trabalho **arquivos** .

Na faixa de faixas, clique em **novo** para criar uma página. Selecione **cshtml** e nomeie a nova página *TestRazor. cshtml*.

Clique em **OK**.

Copie o seguinte para o arquivo, substituindo completamente o que já existe.

> [!NOTE]
> Quando você copia o código ou a marcação dos exemplos em uma página, o recuo e o alinhamento podem não ser os mesmos do tutorial. O recuo e o alinhamento não afetam a forma como o código é executado.

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample1.cshtml)]

## <a name="examining-the-example-page"></a>Examinando a página de exemplo

A maior parte do que você vê é HTML comum. No entanto, na parte superior, há este bloco de código:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample2.cshtml)]

Observe as seguintes coisas sobre este bloco de código:

- O caractere @ informa ao ASP.NET que o que vem a seguir é o código do Razor, não HTML. ASP.NET tratará tudo depois do caractere @ como código até que ele seja executado em algum HTML novamente. (Nesse caso, essa é a &lt;! DOCTYPE&gt; elemento.
- As chaves ({e}) incluem um bloco de código do Razor se o código tiver mais de uma linha. As chaves dizem ASP.NET onde o código para esse bloco inicia e termina.
- Os caracteres//marcam um comentário — ou seja, uma parte do código que não será executada.
- Cada instrução deve terminar com um ponto e vírgula (;). (No entanto, não há comentários.)
- Você pode armazenar valores em *variáveis*, que você cria (*declare*) com a palavra-chave var. Ao criar uma variável, você atribui a ela um nome, que pode incluir letras, números e sublinhado (\_). Nomes de variáveis não podem começar com um número e não podem usar o nome de uma palavra-chave de programação (como var).
- Coloque as cadeias de caracteres (como "ASP.NET" e "páginas da Web") entre aspas. (Eles devem ser aspas duplas). Os números não estão entre aspas.
- Espaço em branco fora das aspas não importa. A maioria das quebras de linha não importa; a exceção é que você não pode dividir uma cadeia entre aspas nas linhas. Não importa o recuo e o alinhamento.

Algo que não é óbvio neste exemplo é que todo o código diferencia maiúsculas de minúsculas. Isso significa que a variável TheSum é uma variável diferente das variáveis que podem ser nomeadas theSum ou TheSum. Da mesma forma, var é uma palavra-chave, mas var não é.

### <a name="objects-and-properties-and-methods"></a>Objetos e propriedades e métodos

Então, há a expressão DateTime. Now. Em termos simples, DateTime é um *objeto*. Um objeto é algo com o qual você pode programar — uma página, uma caixa de texto, um arquivo, uma imagem, uma solicitação da Web, uma mensagem de email, um registro de cliente, etc. Os objetos têm uma ou mais *Propriedades* que descrevem suas características. Um objeto de caixa de texto tem uma propriedade de texto (entre outros), um objeto de solicitação tem uma propriedade de URL (e outros), uma mensagem de email tem uma propriedade from e uma propriedade to, e assim por diante. Os objetos também têm *métodos* que são os "verbos" que podem ser executados. Você trabalhará com objetos muito.

Como você pode ver no exemplo, DateTime é um objeto que permite que você programe datas e horas. Ele tem uma propriedade chamada agora que retorna a data e hora atuais.

### <a name="using-code-to-render-markup-in-the-page"></a>Usando o código para renderizar a marcação na página

No corpo da página, observe o seguinte:

[!code-html[Main](intro-to-web-pages-programming/samples/sample3.html)]

Novamente, o caractere @ informa ao ASP.NET que o que vem a seguir é o código, não HTML. Na marcação, você pode adicionar @ seguido por uma expressão de código e ASP.NET renderizará o valor dessa expressão diretamente nesse ponto. No exemplo, @a renderizará qualquer que seja o valor da variável chamada a, @product renderiza o que estiver na variável chamada Product e assim por diante.

No entanto, você não está limitado a variáveis. Em algumas instâncias aqui, o caractere @ precede uma expressão:

- @ (a\*b) renderiza o produto de qualquer coisa nas variáveis a e b. (O operador de \* significa multiplicação.)
- @ (Technology + "" + Product) renderiza os valores na tecnologia de variáveis e no produto depois de concatena-los e adicionar um espaço entre eles. O operador (+) para concatenar cadeias de caracteres é o mesmo que o operador para adicionar números. ASP.NET normalmente pode informar se você está trabalhando com números ou com cadeias de caracteres e faz a coisa certa com o operador +.
- @Request.Url renderiza a propriedade URL do objeto de solicitação. O objeto de solicitação contém informações sobre a solicitação atual do navegador e, claro, a propriedade URL contém a URL dessa solicitação atual.

O exemplo também foi projetado para mostrar que você pode trabalhar de maneiras diferentes. Você pode fazer cálculos no bloco de código na parte superior, colocar os resultados em uma variável e, em seguida, renderizar a variável na marcação. Ou você pode fazer cálculos em uma expressão à direita na marcação. A abordagem usada depende do que você está fazendo e, até certo ponto, em sua própria preferência.

### <a name="seeing-the-code-in-action"></a>Vendo o código em ação

Clique com o botão direito do mouse no nome do arquivo e escolha **Iniciar no navegador**. Você vê a página no navegador com todos os valores e expressões resolvidos na página.

![Página ' TestRazor ' em execução no navegador](intro-to-web-pages-programming/_static/image2.png)

Examine a origem no navegador.

![Fonte de página ' test Razor ' no navegador](intro-to-web-pages-programming/_static/image3.png)

Como você espera de sua experiência no tutorial anterior, nenhum do código do Razor está na página. Tudo o que você vê são os valores de exibição reais. Quando você executa uma página, na verdade está fazendo uma solicitação para o servidor Web interno do WebMatrix. Quando a solicitação é recebida, o ASP.NET resolve todos os valores e expressões e renderiza seus valores na página. Em seguida, ele envia a página para o navegador.

> [!TIP] 
> 
> **Razor eC#**
> 
> Até agora, dissemos que você está trabalhando com sintaxe Razor. Isso é verdade, mas não é a história completa. A linguagem de programação real que você está usando *C#* é chamada. C#foi criado pela Microsoft há mais de uma década e tornou-se uma das principais linguagens de programação para a criação de aplicativos do Windows. Todas as regras que você viu sobre como nomear uma variável e como criar instruções e assim por diante são, na verdade, todas C# as regras do idioma.
> 
> O Razor se refere mais especificamente ao pequeno conjunto de convenções de como você insere esse código em uma página. Por exemplo, a Convenção de usar @ para marcar o código na página e usar @ {} para inserir um bloco de código é o aspecto do Razor de uma página. Os auxiliares também são considerados como parte do Razor. Sintaxe Razor é usado em mais lugares do que apenas no Páginas da Web do ASP.NET. (Por exemplo, ele também é usado em exibições do MVC ASP.NET.)
> 
> Mencionamos isso porque, se você procurar informações sobre programação Páginas da Web do ASP.NET, encontrará muitas referências ao Razor. No entanto, muitas dessas referências não se aplicam ao que você está fazendo e, portanto, podem ser confusas. E, na verdade, muitas de suas perguntas de programação vão realmente estar trabalhando C# ou trabalhando com o ASP.net. Portanto, se você procurar informações especificamente sobre o Razor, talvez não encontre as respostas de que precisa.

## <a name="adding-some-conditional-logic"></a>Adicionando alguma lógica condicional

Um dos ótimos recursos sobre o uso de código em uma página é que você pode alterar o que acontece com base em várias condições. Nesta parte do tutorial, você vai brincar com algumas maneiras de alterar o que é exibido na página.

O exemplo será simples e, de certa forma, forçado para que possamos nos concentrar na lógica condicional. A página que você criará fará isso:

- Mostre um texto diferente na página, dependendo se é a primeira vez que a página é exibida ou se você clicou em um botão para enviar a página. Esse será o primeiro teste condicional.
- Exiba a mensagem somente se um determinado valor for passado na cadeia de caracteres de consulta da URL (http://...? show = true). Esse será o segundo teste condicional.

No WebMatrix, crie uma página e nomeie-a *TestRazorPart2. cshtml*. (Na faixa de faixas, clique em **novo**, escolha **cshtml**, nomeie o arquivo e clique em **OK**.)

Substitua o conteúdo dessa página pelo seguinte:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample4.cshtml)]

O bloco de código na parte superior Inicializa uma mensagem chamada de variável com algum texto. No corpo da página, o conteúdo da variável de mensagem é exibido dentro de um elemento &lt;p&gt;. A marcação também contém um elemento de&gt; de entrada de &lt;para criar um botão de **envio** .

Execute a página para ver como ela funciona agora. Por enquanto, ela é basicamente uma página estática, mesmo que você clique no botão **Enviar** .

Volte para o WebMatrix. Dentro do bloco de código, adicione o seguinte código realçado *após* a linha que inicializa a mensagem:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample5.cshtml?highlight=4-6)]

### <a name="the-if---block"></a>O bloco If {}

O que você acabou de adicionar era uma condição if. No código, a condição if tem uma estrutura como esta:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample6.cs)]

A condição a ser testada está entre parênteses. Deve ser um valor ou uma expressão que retorna true ou false. Se a condição for verdadeira, ASP.NET executará a instrução ou as instruções que estão dentro das chaves. (Esses são os *que fazem parte da lógica* *if-then* .) Se a condição for false, o bloco de código será ignorado.

Aqui estão alguns exemplos de condições que você pode testar em uma instrução If:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample7.cs)]

Você pode testar variáveis em relação a valores ou a expressões usando um operador *lógico* ou *operador de comparação*: igual a (= =), maior que (&gt;), menor que (&lt;), maior que ou igual a (&gt;=) e menor ou igual a (&lt;=). O operador! = significa que não é igual a — por exemplo, if (a! = 0) significa *se um não é igual a 0*.

> [!NOTE]
> Lembre-se de observar que o operador de comparação para igual a (= =) não é o mesmo que =. O operador = é usado apenas para atribuir valores (var a = 2). Se você misturar esses operadores, receberá um erro ou obterá resultados estranhos.

Para testar se algo é verdadeiro, a sintaxe completa é if (isdone = = true). Mas você também pode usar o atalho se (isdone). Se não houver nenhum operador de comparação, ASP.NET assumirá que você está testando para verdadeiro.

O operador ! o operador por si só significa um não lógico. Por exemplo, a condição se (! IsPost) significa *se IsPost não for verdadeiro*.

Você pode combinar condições usando um operador lógico AND (&amp;&amp;) ou lógico OR (| | Operator). Por exemplo, a última das condições If nos exemplos anteriores significa que *se FileProcessingIsDone não estiver definido como true e displayMessage estiver definido como false*.

### <a name="the-else-block"></a>O bloco else

Uma última coisa sobre blocos If: um bloco If pode ser seguido por um bloco else. Um bloco else é útil é que você precisa executar código diferente quando a condição for falsa. Eis um exemplo simples:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample8.cs)]

Você verá alguns exemplos em Tutoriais posteriores nesta série em que o uso de um bloco else é útil.

### <a name="testing-whether-the-request-is-a-submit-post"></a>Testando se a solicitação é um envio (post)

Há mais, mas vamos voltar ao exemplo, que tem a condição if (IsPost) {...}. IsPost é, na verdade, uma propriedade da página atual. Na primeira vez que a página for solicitada, IsPost retornará false. No entanto, se você clicar em um botão ou enviar a página, ou seja, você o publicará — IsPost retornará true. Então, IsPost permite que você determine se está lidando com um envio de formulário. (Em termos de verbos HTTP, se a solicitação for uma operação GET, IsPost retornará false. Se a solicitação for uma operação POST, IsPost retornará true.) Em um tutorial posterior, você trabalhará com formulários de entrada, em que esse teste se torna particularmente útil.

Execute a página. Como esta é a primeira vez que a página é solicitada, você verá "esta é a primeira vez que você solicitou a página". Essa cadeia de caracteres é o valor para o qual você inicializou a variável de mensagem. Há um teste IF (IsPost), mas que retorna false no momento, portanto, o código dentro do bloco If não é executado.

Clique no botão **Enviar** . A página é solicitada novamente. Como antes, a variável de mensagem é definida como "esta é a primeira vez...". Mas, desta vez, o teste IF (IsPost) retorna true, portanto, o código dentro do bloco If é executado. O código altera o valor da variável de mensagem para um valor diferente, que é o que é processado na marcação.

Agora, adicione uma condição if na marcação. Abaixo do &lt;p&gt; elemento que contém o botão **Enviar** , adicione a seguinte marcação:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample9.cshtml)]

Você está adicionando código dentro da marcação, portanto, você precisa começar com @. Em seguida, há um teste IF semelhante àquele que você adicionou anteriormente no bloco de código. Dentro das chaves, no entanto, você está adicionando HTML comum — pelo menos, é comum até chegar a @DateTime.Now. Esse é outro pouco de código do Razor, então, novamente, você precisa adicionar @ na frente dele.

O ponto aqui é que você pode adicionar condições If no bloco de código na parte superior e na marcação. Se você usar uma condição if no corpo da página, as linhas dentro do bloco poderão ser marcação ou código. Nesse caso, e como é verdadeiro sempre que você misturar a marcação e o código, precisará usar @ para deixá-lo claro para ASP.NET onde está o código.

Execute a página e clique em **Enviar**. Desta vez, você não só verá uma mensagem diferente ao enviar ("agora você já enviou..."), mas verá uma nova mensagem que lista a data e a hora.

![Página ' test Razor 2 ' em execução no navegador com carimbo de data/hora mostrando após enviar](intro-to-web-pages-programming/_static/image4.png)

### <a name="testing-the-value-of-a-query-string"></a>Testando o valor de uma cadeia de caracteres de consulta

Mais um teste. Desta vez, você adicionará um bloco If que testa um valor chamado show que pode ser passado na cadeia de caracteres de consulta. (Assim: `http://localhost:43097/TestRazorPart2.cshtml?show=true`) Você alterará a página para que a mensagem que você está exibindo ("esta é a primeira vez..." etc.) só será exibida se o valor de show for true.

Na parte inferior (mas dentro) do bloco de código na parte superior da página, adicione o seguinte:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample10.cs)]

O bloco de código completo agora se parece com o exemplo a seguir. (Lembre-se de que quando você copiar o código em sua página, o recuo poderá parecer diferente. Mas isso não afeta a forma como o código é executado.)

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample11.cshtml)]

O novo código no bloco Inicializa uma variável chamada exmessage como false. Em seguida, ele faz um teste IF para procurar um valor na cadeia de caracteres de consulta. Quando você solicita a página pela primeira vez, ela tem uma URL como esta:

`http://localhost:43097/TestRazorPart2.cshtml`

O código determina se a URL contém uma variável chamada show na cadeia de caracteres de consulta, como esta versão da URL:

`http://localhost:43097/TestRazorPart2.cshtml`? show = true

O próprio teste examina a propriedade QueryString do objeto de solicitação. Se a cadeia de caracteres de consulta contiver um item chamado show e se esse item for definido como true, o bloco If será executado e definirá a variável exmessage como true.

Há um truque aqui, como você pode ver. Como o nome diz, a cadeia de caracteres de consulta é uma cadeia de caracteres. No entanto, você só pode testar true e false se o valor que você está testando for um valor booliano (true/false). Antes de poder testar o valor da variável show na cadeia de caracteres de consulta, você precisa convertê-lo em um valor booliano. É isso que o método asbool faz — usa uma cadeia de caracteres como entrada e converte-a em um valor booliano. Claramente, se a cadeia de caracteres for "true", o método asbool converterá esse valor em true. Se o valor da cadeia de caracteres for qualquer outra coisa, asbool retornará false.

> [!TIP] 
> 
> **Tipos de dados e métodos as ()**
> 
> Já dissemos até agora que, ao criar uma variável, você use a palavra-chave var. No entanto, essa não é a história inteira. Para manipular valores — para adicionar números ou concatenar cadeias de caracteres ou comparar datas ou testar se for true/false — C# deve trabalhar com uma representação interna apropriada do valor. C#*normalmente* , pode descobrir o que a representação deve ser (ou seja, que *tipo* os dados são) com base no que você está fazendo com os valores. Agora, e então, não é possível fazer isso. Caso contrário, você precisa ajudar indicando explicitamente como C# o deve representar os dados. O método asbool faz isso — diz C# que um valor de cadeia de caracteres "true" ou "false" deve ser tratado como um valor booliano. Existem métodos semelhantes para representar cadeias de caracteres como outros tipos, como o Asen (tratar como um inteiro), asdatetime (tratar como uma data/hora), asfloat (tratar como um número de ponto flutuante) e assim por diante. Ao usar esses métodos as (), se não C# for possível representar o valor da cadeia de caracteres conforme solicitado, você verá um erro.

Na marcação da página, remova ou Comente este elemento (aqui, ele é mostrado comentado):

[!code-html[Main](intro-to-web-pages-programming/samples/sample12.html)]

Logo em que você removeu ou comentou esse texto, adicione o seguinte:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample13.cshtml)]

O teste IF diz que se a variável exmessage for true, processe um elemento &lt;p&gt; com o valor da variável Message.

### <a name="summary-of-your-conditional-logic"></a>Resumo de sua lógica condicional

Caso você não esteja totalmente certo do que acabou de fazer, aqui está um resumo.

- A variável de mensagem é inicializada para uma cadeia de caracteres padrão ("esta é a primeira vez...").
- Se a solicitação de página for o resultado de um envio (post), o valor da mensagem será alterado para "agora que você enviou..."
- A variável exmessage é inicializada como false.
- Se a cadeia de caracteres de consulta contiver? show = true, a variável exmessage será definida como true.
- Na marcação, se a @ Message for true, um elemento &lt;p&gt; será renderizado, mostrando o valor da mensagem. (Se o exmessage for false, nada será renderizado nesse ponto na marcação.)
- Na marcação, se a solicitação for uma postagem, um elemento &lt;p&gt; será renderizado que exibirá a data e a hora.

Execute a página. Não há nenhuma mensagem, porque a @ Message é false, portanto, na marcação, o teste IF (exmessage) retorna false.

Clique em **Enviar**. Você vê a data e a hora, mas ainda não há mensagem.

No navegador, vá para a caixa URL e adicione o seguinte ao final da URL:? show = true e pressione Enter.

![Página ' testar Razor 2 ' no navegador mostrando a cadeia de caracteres de consulta](intro-to-web-pages-programming/_static/image5.png)

A página é exibida novamente. (Como você alterou a URL, essa é uma nova solicitação, não um envio.) Clique em **Enviar** novamente. A mensagem é exibida novamente, como é a data e a hora.

![Página ' test Razor 2 ' após o envio quando há uma cadeia de caracteres de consulta](intro-to-web-pages-programming/_static/image6.png)

Na URL, altere? show = true para? show = false e pressione Enter. Envie a página novamente. A página volta a como você começou, sem mensagem.

Conforme observado anteriormente, a lógica deste exemplo é um pouco forçado. No entanto, se o for surgir em muitas de suas páginas, ele usará um ou mais dos formulários que você viu aqui.

## <a name="installing-a-helper-displaying-a-gravatar-image"></a>Instalando um auxiliar (exibindo uma imagem Gravatar)

Algumas tarefas que as pessoas geralmente desejam fazer em páginas da Web exigem muito código ou exigem conhecimento extra. Exemplos: exibindo um gráfico para dados; colocando um botão "Like" do Facebook em uma página; enviando emails do seu site; corte ou redimensionamento de imagens; usando o PayPal para seu site. Para facilitar esse tipo de coisas, Páginas da Web do ASP.NET permite que você use *auxiliares*. Os auxiliares são componentes que você instala para um site e que permitem executar tarefas típicas usando apenas algumas linhas de código do Razor.

Páginas da Web do ASP.NET tem alguns auxiliares internos. No entanto, muitos auxiliares estão disponíveis em pacotes (suplementos) que são fornecidos usando o Gerenciador de pacotes NuGet. O NuGet permite selecionar um pacote a ser instalado e cuida de todos os detalhes da instalação.

Nesta parte do tutorial, você instalará um auxiliar que permite exibir uma imagem Gravatar ("Avatar globalmente reconhecido"). Você aprenderá duas coisas. Um é como localizar e instalar um auxiliar. Você também aprenderá como um auxiliar facilita a realização de algo que, de outra forma, você precisaria fazer usando uma grande quantidade de código que teria de escrever.

Você pode registrar seu próprio Gravatar no site do gravatar em [http://www.gravatar.com/](http://www.gravatar.com/), mas não é essencial criar uma conta do Gravatar para executar essa parte do tutorial.

No WebMatrix, clique no botão **NuGet** .

![Caixa de diálogo Galeria do NuGet no WebMatrix](intro-to-web-pages-programming/_static/image7.png)

Isso inicia o Gerenciador de pacotes NuGet e exibe os pacotes disponíveis. (Nem todos os pacotes são auxiliares; alguns deles adicionam funcionalidade ao próprio WebMatrix, alguns são modelos adicionais e assim por diante.) Você pode receber uma mensagem de erro sobre a incompatibilidade de versão. Você pode ignorar essa mensagem de erro clicando em **OK** e prosseguindo com este tutorial.

![Caixa de diálogo Galeria do NuGet no WebMatrix](intro-to-web-pages-programming/_static/image8.png)

Na caixa de pesquisa, digite "asp.net Helpers". O NuGet mostra os pacotes que correspondem aos termos de pesquisa.

![Galeria do NuGet no WebMatrix mostrando pacotes](intro-to-web-pages-programming/_static/image9.png)

A biblioteca de auxiliares da Web do ASP.NET contém código para simplificar muitas tarefas comuns, incluindo o uso de imagens Gravatar. Selecione o pacote de **biblioteca de auxiliares Web do ASP.net** e clique em **instalar** para iniciar o instalador. Selecione **Sim** quando for perguntado se você deseja instalar o pacote e aceite os termos para concluir a instalação.

É isso. O NuGet baixa e instala tudo, incluindo quaisquer componentes adicionais que possam ser necessários (*dependências*).

Se, por alguma razão, você precisar desinstalar um auxiliar, o processo será muito semelhante. Clique no botão **NuGet** , clique na guia **instalado** e selecione o pacote que deseja desinstalar.

## <a name="using-a-helper-in-a-page"></a>Usando um auxiliar em uma página

Agora você usará o auxiliar que acabou de instalar. O processo para adicionar um auxiliar a uma página é semelhante para a maioria dos auxiliares.

No WebMatrix, crie uma página e nomeie-a *GravatarTest. cshml*. (Você está criando uma página especial para testar o auxiliar, mas pode usar auxiliares em qualquer página no seu site.)

Dentro do elemento&gt; do corpo do &lt;, adicione um elemento &lt;div&gt;. Dentro do elemento &lt;div&gt;, digite:

@Gravatar.

O caractere @ é o mesmo caractere que você estava usando para marcar o código do Razor. **Gravatar** é o objeto auxiliar com o qual você está trabalhando.

Assim que você digitar o período (.), o WebMatrix exibirá uma lista de *métodos* (funções) que o auxiliar do Gravatar disponibiliza:

![Lista suspensa do IntelliSense do Gravatar Helper](intro-to-web-pages-programming/_static/image10.png)

Esse recurso é conhecido como *IntelliSense*. Ele ajuda você a codificar fornecendo opções apropriadas de contexto. O IntelliSense funciona com HTML, CSS, código ASP.NET, JavaScript e outras linguagens com suporte no WebMatrix. É outro recurso que torna mais fácil desenvolver páginas da Web no WebMatrix.

Pressione G no teclado e você verá que o IntelliSense encontra o método GetHtml. Pressione Tab. o IntelliSense insere o método selecionado (GetHtml) para você. Digite um parênteses de abertura e observe que o parêntese de fechamento é adicionado automaticamente. Digite seu endereço de email entre aspas entre os dois parênteses. Se você tiver uma conta do Gravatar, a imagem do seu perfil será retornada. Se você não tiver uma conta do Gravatar, uma imagem padrão será retornada. Quando terminar, a linha terá esta aparência:

[!code-css[Main](intro-to-web-pages-programming/samples/sample14.css)]

Agora, exiba a página em um navegador. A imagem ou a imagem padrão é exibida, dependendo se você tem uma conta do Gravatar.

![Gravatar](intro-to-web-pages-programming/_static/image11.png) ![imagem padrão](intro-to-web-pages-programming/_static/image12.png)

Para ter uma ideia do que o auxiliar está fazendo para você, exiba a origem da página no navegador. Junto com o HTML que você tinha em sua página, você vê um elemento Image que inclui um identificador. Esse é o código que o auxiliar renderizau na página no local onde você tinha @Gravatar.GetHtml. O auxiliar assumiu as informações fornecidas e gerou o código que se comunica diretamente com o Gravatar para obter a imagem correta para a conta fornecida.

O método GetHtml também permite que você personalize a imagem fornecendo outros parâmetros. O código a seguir mostra como solicitar que uma imagem tenha uma largura e altura de 40 pixels e use uma imagem padrão especificada chamada **wavatar** se a conta especificada não existir.

[!code-javascript[Main](intro-to-web-pages-programming/samples/sample15.js)]

Esse código produz algo parecido com o seguinte resultado (a imagem padrão varia aleatoriamente).

![](intro-to-web-pages-programming/_static/image13.png)

## <a name="coming-up-next"></a>Chegando em seguida

Para manter este tutorial curto, precisamos nos concentrar em apenas alguns conceitos básicos. Naturalmente, há *muito* mais no Razor e C#no. Você aprenderá mais ao percorrer esses tutoriais. Se você estiver interessado em aprender mais sobre os aspectos de programação do Razor C# e agora, você pode ler uma introdução mais completa aqui: [introdução à programação da Web do ASP.NET usando a sintaxe do Razor](https://go.microsoft.com/fwlink/?LinkID=202890).

O próximo tutorial apresenta o trabalho com um banco de dados. Nesse tutorial, você começará a criar o aplicativo de exemplo que permite listar seus filmes favoritos.

## <a name="complete-listing-for-testrazor-page"></a>Listagem completa da página TestRazor

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample16.cshtml)]

## <a name="complete-listing-for-testrazorpart2-page"></a>Listagem completa da página TestRazorPart2

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample17.cshtml)]

## <a name="complete-listing-for-gravatartest-page"></a>Listagem completa da página GravatarTest

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample18.cshtml)]

## <a name="additional-resources"></a>Recursos adicionais

- [Introdução à programação da Web do ASP.NET usando a sintaxe do Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Auxiliar do Twitter](../../ui-layouts-and-themes/twitter-helper.md)

> [!div class="step-by-step"]
> [Anterior](getting-started.md)
> [Próximo](displaying-data.md)
