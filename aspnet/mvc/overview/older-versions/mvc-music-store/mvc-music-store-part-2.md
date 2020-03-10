---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: 'Parte 2: controladores | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo de loja de música MVC do ASP.NET. A parte 2 abrange controladores.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: 9dc2226f4951d4bed122df37d35bbb94730a00ad
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559874"
---
# <a name="part-2-controllers"></a>Parte 2: controladores

por [Jon Galloway](https://github.com/jongalloway)

> A loja de música MVC é um aplicativo de tutorial que apresenta e explica passo a passo como usar o ASP.NET MVC e o Visual Studio para desenvolvimento para a Web.  
>   
> A loja de música MVC é uma implementação de armazenamento de exemplo leve que vende os álbuns de música online e implementa a administração básica de site, a entrada do usuário e a funcionalidade do carrinho de compras.  
>   
> Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo de loja de música MVC do ASP.NET. A parte 2 abrange controladores.

Com estruturas da Web tradicionais, as URLs de entrada são normalmente mapeadas para arquivos em disco. Por exemplo: uma solicitação para uma URL como "/Products.aspx" ou "/Products.php" pode ser processada por um arquivo "Products. aspx" ou "Products. php".

As estruturas MVC baseadas na Web mapeiam URLs para código de servidor de forma ligeiramente diferente. Em vez de mapear URLs de entrada para arquivos, eles mapeiam URLs para métodos em classes. Essas classes são chamadas de "controladores" e são responsáveis pelo processamento de solicitações HTTP de entrada, tratamento de entrada do usuário, recuperação e salvamento de dados e determinação da resposta a ser enviada de volta ao cliente (exibir HTML, baixar um arquivo, redirecionar para outro URL, etc.).

## <a name="adding-a-homecontroller"></a>Adicionando um HomeController

Vamos começar nosso aplicativo MVC Music Store adicionando uma classe Controller que tratará URLs para a Home Page de nosso site. Seguiremos as convenções de nomenclatura padrão do ASP.NET MVC e as chamarei de HomeController.

Clique com o botão direito do mouse na pasta "controladores" dentro da Gerenciador de Soluções e selecione "Adicionar" e, em seguida, o "controlador..." linha

![](mvc-music-store-part-2/_static/image1.jpg)

Isso abrirá a caixa de diálogo "Adicionar controlador". Nomeie o controlador como "HomeController" e pressione o botão Adicionar.

![](mvc-music-store-part-2/_static/image1.png)

Isso criará um novo arquivo, HomeController.cs, com o seguinte código:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

Para iniciar o mais simples possível, vamos substituir o método de índice por um método simples que simplesmente retorna uma cadeia de caracteres. Faremos duas alterações:

- Alterar o método para retornar uma cadeia de caracteres em vez de um ActionResult
- Altere a instrução Return para retornar "Olá de casa"

O método agora deve ser assim:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a>Executando o aplicativo

Agora, vamos executar o site. Podemos iniciar nosso servidor Web e experimentar o site usando uma das seguintes opções:

- Escolha o item de menu Depurar ⇨ iniciar depuração
- Clique no botão de seta verde na barra de ferramentas ![](mvc-music-store-part-2/_static/image2.jpg)
- Use o atalho de teclado, F5.

O uso de qualquer uma das etapas acima compilará nosso projeto e, em seguida, fará com que o ASP.NET Development Server que é integrado ao Visual Web Developer seja iniciado. Uma notificação será exibida no canto inferior da tela para indicar que a ASP.NET Development Server foi iniciada e mostrará o número da porta em que ela está sendo executada.

![](mvc-music-store-part-2/_static/image2.png)

O Visual Web Developer abrirá automaticamente uma janela do navegador cuja URL aponta para nosso servidor Web. Isso nos permitirá testar rapidamente nosso aplicativo Web:

![](mvc-music-store-part-2/_static/image3.png)

Ok, isso foi bem rápido – criamos um novo site, adicionamos uma função de três linhas e temos texto em um navegador. Não tem ciência de Rocket, mas é um começo.

*Observação: o Visual Web Developer inclui o ASP.NET Development Server, que executará seu site em um número "porta" livre aleatório. Na captura de tela acima, o site está em execução em `http://localhost:26641/`, portanto, ele está usando a porta 26641. O número da porta será diferente. Quando falamos sobre a URL como/Store/Browse neste tutorial, isso vai depois do número da porta. Supondo que um número de porta 26641, navegar até/Store/Browse significará navegar até `http://localhost:26641/Store/Browse`.*

## <a name="adding-a-storecontroller"></a>Adicionando um StoreController

Adicionamos um HomeController simples que implementa a home page do nosso site. Agora, vamos adicionar outro controlador que usaremos para implementar a funcionalidade de navegação de nossa loja de música. Nosso controlador de loja dará suporte a três cenários:

- Uma página de listagem dos gêneros musicais em nossa loja de música
- Uma página de navegação que lista todos os álbuns de música em um gênero específico
- Uma página de detalhes que mostra informações sobre um álbum de música específico

Vamos começar adicionando uma nova classe StoreController. Se você ainda não fez isso, interrompa a execução do aplicativo fechando o navegador ou selecionando o item de menu Depurar ⇨ parar depuração.

Agora, adicione um novo StoreController. Assim como fizemos com HomeController, faremos isso clicando com o botão direito do mouse na pasta "controladores" dentro da Gerenciador de Soluções e escolhendo o item de menu do controlador Add-&gt;

![](mvc-music-store-part-2/_static/image4.png)

Nosso novo StoreController já tem um método "index". Usaremos esse método de "índice" para implementar nossa página de listagem que lista todos os gêneros em nossa loja de música. Também adicionaremos dois métodos adicionais para implementar os dois outros cenários que queremos que nosso StoreController manipule: Browse e details.

Esses métodos (índice, navegação e detalhes) em nosso controlador são chamados de "ações do controlador" e, como você já viu com o método de ação HomeController. Index (), seu trabalho é responder às solicitações de URL e (geralmente falando) determinar qual conteúdo deve ser enviado de volta para o navegador ou usuário que invocou a URL.

Vamos iniciar nossa implementação de StoreController alterando o método index () para retornar a cadeia de caracteres "Hello from Store. Index ()" e adicionaremos métodos semelhantes para Browse () e Details ():

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

Execute o projeto novamente e procure as seguintes URLs:

- /Store
- /Store/Browse
- /Store/Details

O acesso a essas URLs invocará os métodos de ação em nosso controlador e retornará respostas de cadeia de caracteres:

![](mvc-music-store-part-2/_static/image5.png)

Isso é ótimo, mas essas são apenas cadeias de caracteres constantes. Vamos torná-las dinâmicas, portanto, elas recebem informações da URL e as exibem na saída da página.

Primeiro, alteraremos o método de ação Browse para recuperar um valor de QueryString da URL. Podemos fazer isso adicionando um parâmetro "gênero" ao nosso método de ação. Quando fizermos isso, o ASP.NET MVC passará automaticamente qualquer parâmetro de postagem de formulário ou QueryString chamado "gênero" para nosso método de ação quando for invocado.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

*Observação: estamos usando o método de utilitário HttpUtility. HtmlEncode para limpar a entrada do usuário. Isso impede que os usuários insiram JavaScript em nossa exibição com um link como/Store/Browse? Gênero =&lt;script&gt;janela. Location = 'http://hackersite.com'&lt;/script&gt;.*

Agora, vamos navegar até/Store/Browse? Gênero = disco

![](mvc-music-store-part-2/_static/image6.png)

Em seguida, vamos alterar a ação Details para ler e exibir um parâmetro de entrada chamado ID. Ao contrário de nosso método anterior, não inseriremos o valor de ID como um parâmetro QueryString. Em vez disso, vamos inseri-lo diretamente na própria URL. Por exemplo:/Store/Details/5.

O ASP.NET MVC nos permite fazer isso facilmente sem precisar configurar nada. A Convenção de roteamento padrão do ASP.NET MVC é tratar o segmento de uma URL após o nome do método de ação como um parâmetro chamado "ID". Se o método de ação tiver um parâmetro chamado ID, o ASP.NET MVC passará automaticamente o segmento de URL para você como um parâmetro.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

Execute o aplicativo e navegue até/Store/Details/5:

![](mvc-music-store-part-2/_static/image7.png)

Vamos recapitular o que fizemos até agora:

- Criamos um novo projeto MVC do ASP.NET no Visual Web Developer
- Discutimos a estrutura básica de pastas de um aplicativo MVC ASP.NET
- Aprendemos como executar nosso site usando o ASP.NET Development Server
- Criamos duas classes de controlador: um HomeController e um StoreController
- Adicionamos métodos de ação a nossos controladores que respondem a solicitações de URL e retornam texto para o navegador

> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-1.md)
> [Próximo](mvc-music-store-part-3.md)
