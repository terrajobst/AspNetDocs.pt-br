---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: Adicionando validação ao modelo | Microsoft Docs
author: shanselman
description: Este é um tutorial principiante que apresenta os fundamentos do ASP.NET MVC. Crie um aplicativo Web simples que lê e grava de um banco de dados.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 9403be574324c34edf93bef1e0e4fd7ba68a3a9d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78543641"
---
# <a name="adding-validation-to-the-model"></a>Adicionar validação ao modelo

por [Scott Hanselman](https://github.com/shanselman)

> Este é um tutorial principiante que apresenta os fundamentos do ASP.NET MVC. Você criará um aplicativo Web simples que lê e grava de um banco de dados. Visite o [ASP.NET MVC Learning Center](../../../index.md) para encontrar outros tutoriais e exemplos do ASP.NET MVC.

Nesta seção, vamos implementar o suporte necessário para habilitar a validação de entrada em nosso aplicativo. Vamos garantir que nosso conteúdo de banco de dados esteja sempre correto e forneça mensagens de erro úteis aos usuários finais ao tentarem e insiram dados de filmes que não são válidos. Vamos começar adicionando uma pequena lógica de validação à classe Movie.

Clique com o botão direito do mouse na pasta modelo e selecione Adicionar classe. Nomeie seu filme de classe.

Quando criamos o modelo de entidade de filme anteriormente, o IDE criou uma classe de filme. Na verdade, parte da classe Movie pode estar em um arquivo e parte em outro. Isso é chamado de classe parcial. Vamos estender a classe Movie de outro arquivo.

Criaremos uma classe de filme parcial que aponta para uma "classe Buddy" com alguns atributos que fornecerão dicas de validação para o sistema. Vamos marcar o título e o preço conforme necessário e também insistir que o preço esteja dentro de um determinado intervalo. Clique com o botão direito do mouse na pasta modelos e selecione Adicionar classe. Nomeie seu filme de classe e clique no botão OK. Esta é a aparência de nossa classe de filme parcial.

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

Execute novamente o aplicativo e tente inserir um filme com um preço acima de 100. Você receberá um erro depois de enviar o formulário. O erro é capturado no lado do servidor e ocorre depois que o formulário é Postado. Observe como os auxiliares HTML internos do ASP.NET MVC eram inteligentes o bastante para exibir a mensagem de erro e manter os valores para nós nos elementos TextBox:

[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)

Isso funciona muito bem, mas seria bom se pudéssemos dizer ao usuário no lado do cliente, imediatamente, antes de o servidor ficar envolvido.

Vamos habilitar uma validação do lado do cliente com o JavaScript.

## <a name="adding-client-side-validation"></a>Adicionando validação do lado do cliente

Como nossa classe de filmes já tem alguns atributos de validação, precisamos apenas adicionar alguns arquivos JavaScript ao nosso modelo de exibição Create. aspx e adicionar uma linha de código para permitir que a validação do lado do cliente ocorra.

No VWD, vá para nossa pasta views/Movie e abra Create. aspx.

Abra a pasta scripts na Gerenciador de Soluções e arraste os três scripts a seguir para dentro da marca&gt; &lt;cabeçalho.

- MicrosoftAjax.js
- MicrosoftMvcValidation.js

Você deseja que esses arquivos de script apareçam nesta ordem.

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

Além disso, adicione essa única linha acima do HTML. BeginForm:

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

Aqui está o código mostrado no IDE.

[Filmes de ![-Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)

Execute o aplicativo e visite/Movies/Create novamente e clique em criar sem inserir nenhum dado. As mensagens de erro são exibidas imediatamente sem o flash de página que associamos ao envio de dados até o servidor. Isso ocorre porque o ASP.NET MVC agora está validando a entrada no cliente (usando JavaScript) e no servidor.

[![criar-Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)

Parece bom! Agora, vamos adicionar uma coluna adicional ao banco de dados.

> [!div class="step-by-step"]
> [Anterior](getting-started-with-mvc-part6.md)
> [Próximo](getting-started-with-mvc-part8.md)
