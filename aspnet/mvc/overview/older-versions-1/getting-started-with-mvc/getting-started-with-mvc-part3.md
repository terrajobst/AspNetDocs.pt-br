---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
title: Adicionando uma exibição | Microsoft Docs
author: shanselman
description: Este é um tutorial principiante que apresenta os fundamentos do ASP.NET MVC. Crie um aplicativo Web simples que lê e grava de um banco de dados.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: e8f1515c-c277-47ff-a23e-224118f13f02
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
msc.type: authoredcontent
ms.openlocfilehash: 462b1210c45da67058899193afcea973f3daf122
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78581553"
---
# <a name="adding-a-view"></a>Adicionar uma exibição

por [Scott Hanselman](https://github.com/shanselman)

> Este é um tutorial principiante que apresenta os fundamentos do ASP.NET MVC. Você criará um aplicativo Web simples que lê e grava de um banco de dados. Visite o [ASP.NET MVC Learning Center](../../../index.md) para encontrar outros tutoriais e exemplos do ASP.NET MVC.

Nesta seção, vamos examinar como podemos fazer com que nossa classe HelloWorldController use um arquivo de modelo de exibição para encapsular corretamente a geração de respostas HTML de volta a um cliente.

Vamos começar usando um modelo de exibição com nosso método de índice. Nosso método é chamado de index e está no HelloWorldController. Atualmente, nosso método index () retorna uma cadeia de caracteres com uma mensagem que é codificada dentro da classe Controller.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample1.cs)]

Agora, vamos alterar o método index para que fique assim:

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample2.cs)]

Agora, vamos adicionar um modelo de exibição ao nosso projeto que podemos usar para o método index (). Para fazer isso, clique com o botão direito do mouse em algum lugar no meio do método index e clique em Adicionar modo de exibição...

![image](getting-started-with-mvc-part3/_static/image1.png)

Isso abrirá a caixa de diálogo "Adicionar exibição" que nos fornece algumas opções sobre como desejamos criar um modelo de exibição que possa ser usado pelo nosso método de índice. Por enquanto, não altere nada e apenas clique no botão Adicionar.

[Caixa de diálogo ![adicionar exibição](getting-started-with-mvc-part3/_static/image3.png)](getting-started-with-mvc-part3/_static/image2.png)

Depois de clicar em Adicionar, uma nova pasta e um novo arquivo aparecerão na pasta da solução, como visto aqui. Agora tenho uma pasta HelloWorld em views e um arquivo index. aspx dentro dessa pasta.

[![solutionexplorerwithindex](getting-started-with-mvc-part3/_static/image5.png)](getting-started-with-mvc-part3/_static/image4.png)

O novo arquivo de índice também já está aberto e pronto para edição. Adicione algum texto na primeira &lt;H2&gt;índice&lt;/H2&gt; como "Olá, Mundo".

[!code-html[Main](getting-started-with-mvc-part3/samples/sample3.html)]

Execute o aplicativo e visite [`http://localhost:xx/HelloWorld`](http://localhostxx) novamente no navegador. O método index em nosso controlador, neste exemplo, não funcionou nenhum trabalho, mas chamamos de "Return View ()", que indica que queríamos usar um arquivo de modelo de exibição para renderizar uma resposta para o cliente. Como não especificamos explicitamente o nome do arquivo de modelo de exibição a ser usado, ASP.NET MVC assumei o uso do arquivo de exibição index. aspx dentro da pasta \Views\HelloWorld Agora vemos a cadeia de caracteres embutida em código em nossa exibição.

[Índice de ![-Windows Internet Explorer](getting-started-with-mvc-part3/_static/image7.png)](getting-started-with-mvc-part3/_static/image6.png)

Parece muito bom. No entanto, observe que o título do navegador diz "index" e o título grande na página diz "meu aplicativo MVC". Vamos alterá-las.

### <a name="changing-views-and-master-pages"></a>Alterando exibições e páginas mestras

Primeiro, vamos alterar o texto "meu aplicativo MVC". Esse texto é compartilhado e aparece em cada página. Na verdade, ele aparece em apenas um lugar em nosso código, mesmo que ele esteja em cada página em nosso aplicativo. Vá para a pasta/Views/Shared no Gerenciador de Soluções e abra o arquivo site. master. Esse arquivo é chamado de página mestra e é o "Shell" compartilhado que todas as outras páginas usam.

Observe um texto que diz ContentPlaceholder "MainContent" neste arquivo.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample4.aspx)]

Esse espaço reservado é onde todas as páginas que você criar serão mostradas, "encapsuladas" na página mestra. Tente alterar o título e, em seguida, execute seu aplicativo e visite várias páginas. Você observará que a alteração aparece em várias páginas.

[!code-html[Main](getting-started-with-mvc-part3/samples/sample5.html)]

Agora, toda página terá o título principal – que é H1-de "meu aplicativo de filme do MVC". Que manipula o texto branco na parte superior, que é compartilhado entre todas as páginas.

Aqui está o site. Master em sua totalidade com nosso título alterado:

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample6.aspx)]

Agora, vamos alterar o título da página de índice.

Abrir/HelloWorld/Index.aspx. Há dois locais a serem alterados. Primeiro, o título que aparece no título do navegador, em seguida, o cabeçalho secundário-também S2. Vou torná-los um pouco diferentes, de modo que você possa ver qual parte do código muda de um aplicativo.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample7.aspx)]

Execute seu aplicativo e visite/Movies. Observe que o título do navegador, o título principal e os títulos secundários foram alterados. É fácil fazer grandes alterações em seu aplicativo com pequenas alterações na exibição.

[Lista de filmes ![-Windows Internet Explorer](getting-started-with-mvc-part3/_static/image9.png)](getting-started-with-mvc-part3/_static/image8.png)

Nosso pequeno "dados" (neste caso, o "Olá, Mundo!" mensagem) estava embutido em código. Temos V (exibições) e temos C (controladores), mas não há M (modelo) ainda. Em breve, veremos como criar um banco de dados e recuperar de forma a sua base.

## <a name="passing-a-viewmodel"></a>Passando um ViewModel

Antes de passarmos para um banco de dados e falarmos sobre modelos, no entanto, vamos falar primeiro sobre "ViewModels". Esses são objetos que representam o que um modelo de exibição exige para renderizar uma resposta HTML de volta para um cliente. Normalmente, eles são criados e passados por uma classe de controlador para um modelo de exibição e devem conter apenas os dados que o modelo de exibição requer, e não mais.

Anteriormente, com nosso exemplo HelloWorld, nosso método de ação Welcome () pegava um nome e um parâmetro numTimes e o reproduziria para o navegador. Em vez de o controlador continuar a renderizar essa resposta diretamente, vamos criar uma pequena classe para manter esses dados e passá-los para um modelo de exibição para renderizar a resposta HTML usando-o. Dessa forma, o controlador se preocupa com uma coisa e com o modelo de exibição outro – permitindo manter a "separação de preocupações" limpa em nosso aplicativo.

Retorne ao arquivo HelloWorldController.cs e adicione uma nova classe "WelcomeViewModel" e altere o método Welcome dentro de seu controlador. Aqui está a HelloWorldController.cs completa com a nova classe no mesmo arquivo.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample8.cs)]

Embora ele esteja em várias linhas, nosso método de boas-vindas é, na verdade, apenas duas instruções de código. A primeira instrução empacota nossos dois parâmetros em um objeto ViewModel e o segundo passa o objeto resultante para a exibição.

Agora precisamos de um modelo de exibição de boas-vindas! Clique com o botão direito do mouse no método Welcome e selecione Adicionar exibição. Desta vez, vamos verificar "criar uma exibição fortemente tipada" e selecionar nossa classe WelcomeViewModel na lista suspensa. Essa nova exibição só saberá sobre WelcomeViewModels e nenhum outro tipo de objeto.

> *Observação: você precisará ter compilado uma vez depois de adicionar o WelcomeViewModel para para aparecer na lista suspensa.*

Veja como deve ser a aparência da caixa de diálogo Adicionar exibição. Clique no botão Adicionar. ![Adicionar modo de exibição circulado](getting-started-with-mvc-part3/_static/image10.png)

Adicione este código sob o &lt;H2&gt; em seu novo Welcome. aspx. Vamos fazer um loop e dizer Olá quantas vezes o usuário disser que deveria!

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample9.aspx)]

Além disso, observe enquanto você está digitando isso porque dissemos a essa exibição sobre o WelcomeViewModel (eles estão casado, lembramos?) de que obtemos o IntelliSense sempre que referenciamos nosso objeto de modelo, como visto na captura de tela abaixo:

[![código-fonte NumTime](getting-started-with-mvc-part3/_static/image12.png)](getting-started-with-mvc-part3/_static/image11.png)

Execute o aplicativo e visite `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` novamente. Agora estamos obtendo dados da URL, eles são passados em nosso controlador automaticamente, nosso controlador empacota os dados em um ViewModel e passa esse objeto para nossa exibição. A exibição do que exibe os dados como HTML para o usuário.

[![bem-vindo-Windows Internet Explorer](getting-started-with-mvc-part3/_static/image14.png)](getting-started-with-mvc-part3/_static/image13.png)

Bem, esse era um tipo de "M" para o modelo, mas não o tipo de banco de dados. Vamos pegar o que aprendemos e criamos um banco de dados de filmes.

> [!div class="step-by-step"]
> [Anterior](getting-started-with-mvc-part2.md)
> [Próximo](getting-started-with-mvc-part4.md)
