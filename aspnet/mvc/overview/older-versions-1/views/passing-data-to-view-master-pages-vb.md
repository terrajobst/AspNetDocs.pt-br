---
uid: mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-vb
title: Passando dados para exibir páginas mestras (VB) | Microsoft Docs
author: microsoft
description: O objetivo deste tutorial é explicar como você pode passar dados de um controlador para uma página mestra de exibição. Examinamos duas estratégias para passar dados para uma exibição m...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: 37a1ebae-8773-408f-8645-d21da7ff9ae1
msc.legacyurl: /mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 9f768f47557adedc43cebfa2c092014bba5842de
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78600103"
---
# <a name="passing-data-to-view-master-pages-vb"></a>Transmitir dados para Exibir páginas mestras (VB)

pela [Microsoft](https://github.com/microsoft)

[Baixar PDF](https://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_13_VB.pdf)

> O objetivo deste tutorial é explicar como você pode passar dados de um controlador para uma página mestra de exibição. Examinamos duas estratégias para passar dados para uma página mestra de exibição. Primeiro, discutimos uma solução fácil que resulta em um aplicativo que é difícil de manter. Em seguida, examinamos uma solução muito melhor que exige um pouco mais de trabalho inicial, mas resulta em um aplicativo muito mais passível de manutenção.

## <a name="passing-data-to-view-master-pages"></a>Passando dados para exibir páginas mestras

O objetivo deste tutorial é explicar como você pode passar dados de um controlador para uma página mestra de exibição. Examinamos duas estratégias para passar dados para uma página mestra de exibição. Primeiro, discutimos uma solução fácil que resulta em um aplicativo que é difícil de manter. Em seguida, examinamos uma solução muito melhor que exige um pouco mais de trabalho inicial, mas resulta em um aplicativo muito mais passível de manutenção.

### <a name="the-problem"></a>O problema

Imagine que você esteja criando um aplicativo de banco de dados de filmes e queira exibir a lista de categorias de filmes em cada página do seu aplicativo (consulte a Figura 1). Imagine, além disso, que a lista de categorias de filmes é armazenada em uma tabela de banco de dados. Nesse caso, faz sentido recuperar as categorias do banco de dados e renderizar a lista de categorias de filmes em uma página mestra de exibição.

[![exibir categorias de filme em uma página mestra de exibição](passing-data-to-view-master-pages-vb/_static/image2.png)](passing-data-to-view-master-pages-vb/_static/image1.png)

**Figura 01**: exibindo categorias de filme em uma página de exibição mestra ([clique para exibir a imagem em tamanho normal](passing-data-to-view-master-pages-vb/_static/image3.png))

Aqui está o problema. Como recuperar a lista de categorias de filmes na página mestra? É tentador chamar métodos de suas classes de modelo diretamente na página mestra. Em outras palavras, é tentador incluir o código para recuperar os dados do banco de dado diretamente na sua página mestra. No entanto, ignorar os controladores MVC para acessar o banco de dados violaria a separação clara de preocupações que é um dos principais benefícios da criação de um aplicativo MVC.

Em um aplicativo MVC, você deseja que todas as interações entre as exibições do MVC e o modelo MVC sejam manipuladas por seus controladores MVC. Essa separação de preocupações resulta em um aplicativo mais passível de manutenção, adaptável e de teste.

Em um aplicativo MVC, todos os dados passados para uma exibição, incluindo uma página mestra de exibição, devem ser passados para um modo de exibição por uma ação do controlador. Além disso, os dados devem ser passados tirando proveito dos dados de exibição. No restante deste tutorial, Eu examino dois métodos de passar dados de exibição para uma página mestra de exibição.

### <a name="the-simple-solution"></a>A solução simples

Vamos começar com a solução mais simples para passar dados de exibição de um controlador para uma página mestra de exibição. A solução mais simples é passar os dados de exibição para a página mestra em cada ação do controlador.

Considere o controlador na Listagem 1. Ele expõe duas ações chamadas `Index()` e `Details()`. O método de ação `Index()` retorna todos os filmes na tabela de banco de dados de filmes. O método de ação `Details()` retorna todos os filmes em uma determinada categoria de filme.

**Listagem 1 – `Controllers\HomeController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample1.vb)]

Observe que as ações `Index()` e `Details()` adicionam dois itens para exibir dados. A ação `Index()` adiciona duas chaves: categorias e filmes. A chave de categorias representa a lista de categorias de filmes exibidas pela página de exibição mestra. A chave de filmes representa a lista de filmes exibidos pela página de exibição de índice.

A ação `Details()` também adiciona duas chaves nomeadas categorias e filmes. A chave Categories, mais uma vez, representa a lista de categorias de filmes exibidas pela página mestra de exibição. A chave de filmes representa a lista de filmes em uma determinada categoria exibida pela página exibição de detalhes (consulte a Figura 2).

[![a exibição de detalhes](passing-data-to-view-master-pages-vb/_static/image5.png)](passing-data-to-view-master-pages-vb/_static/image4.png)

**Figura 02**: a exibição de detalhes ([clique para exibir a imagem em tamanho normal](passing-data-to-view-master-pages-vb/_static/image6.png))

A exibição de índice está contida na Listagem 2. Ele simplesmente itera na lista de filmes representados pelo item de filmes nos dados de exibição.

**Listagem 2 – `Views\Home\Index.aspx`**

[!code-aspx[Main](passing-data-to-view-master-pages-vb/samples/sample2.aspx)]

A página mestra de exibição está contida na Listagem 3. A página mestra de exibição itera e renderiza todas as categorias de filme representadas pelo item categorias de exibir dados.

**Listagem 3 – `Views\Shared\Site.master`**

[!code-aspx[Main](passing-data-to-view-master-pages-vb/samples/sample3.aspx)]

Todos os dados são passados para a exibição e a página de exibição mestra por meio de dados de exibição. Essa é a maneira correta de passar dados para a página mestra.

Então, o que há de errado com essa solução? O problema é que essa solução viole o princípio seco (Don't Repeat Yourself). Cada ação do controlador deve adicionar a mesma lista de categorias de filmes para exibir dados. Ter código duplicado em seu aplicativo torna seu aplicativo muito mais difícil de manter, adaptar e modificar.

### <a name="the-good-solution"></a>A boa solução

Nesta seção, examinaremos uma solução alternativa e melhor para passar dados de uma ação do controlador para uma página mestra de exibição. Em vez de adicionar as categorias de filme para a página mestra em cada ação do controlador, adicionamos as categorias de filme aos dados de exibição apenas uma vez. Todos os dados de exibição usados pela página mestra de exibição são adicionados a um controlador de aplicativo.

A classe ApplicationController está contida na Listagem 4.

A classe ApplicationController está contida na Listagem 4.

**Listagem 4 – `Controllers\ApplicationController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample4.vb)]

Há três coisas que você deve observar sobre o controlador de aplicativo na Listagem 4. Primeiro, observe que a classe herda da classe base System. Web. Mvc. Controller. O controlador de aplicativo é uma classe de controlador.

Em segundo lugar, observe que a classe do controlador de aplicativo é uma classe MustInherit. Uma classe MustInherit é uma classe que deve ser implementada por uma classe concreta. Como o controlador de aplicativo é uma classe MustInherit, você não pode invocar nenhum método definido na classe diretamente. Se você tentar invocar a classe de aplicativo diretamente, receberá uma mensagem de erro não é possível encontrar um recurso.

Em terceiro lugar, observe que o controlador de aplicativo contém um construtor que adiciona a lista de categorias de filmes para exibir dados. Cada classe de controlador herdada do controlador de aplicativo chama automaticamente o construtor do controlador de aplicativo. Sempre que você chamar qualquer ação em qualquer controlador herdado do controlador de aplicativo, as categorias de filme serão incluídas na exibição de dados automaticamente.

O controlador de filmes na listagem 5 é herdado do controlador de aplicativos.

**Listagem 5 – `Controllers\MoviesController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample5.vb)]

O controlador de filmes, assim como o controlador doméstico abordado na seção anterior, expõe dois métodos de ação chamados `Index()` e `Details()`. Observe que a lista de categorias de filmes exibidas pela página mestra de exibição não é adicionada para exibir dados no método `Index()` ou `Details()`. Como o controlador de filmes é herdado do controlador de aplicativos, a lista de categorias de filmes é adicionada para exibir dados automaticamente.

Observe que essa solução para adicionar dados de exibição para uma página mestra de exibição não viola o princípio de SECAgem (Don't Repeat Yourself). O código para adicionar a lista de categorias de filmes para exibir dados está contido em apenas um local: o construtor para o controlador de aplicativo.

### <a name="summary"></a>Resumo

Neste tutorial, discutimos duas abordagens para passar dados de exibição de um controlador para uma página mestra de exibição. Primeiro, examinamos uma abordagem simples, mas difícil de manter. Na primeira seção, discutimos como você pode adicionar dados de exibição para uma página mestra de exibição em cada ação de controlador em seu aplicativo. Concluimos que essa era uma abordagem inadequada porque viola o princípio seco (Don't Repeat Yourself).

Em seguida, examinamos uma estratégia muito melhor para adicionar dados exigidos por uma página mestra de exibição para exibir dados. Em vez de adicionar os dados de exibição em cada ação do controlador, adicionamos os dados de exibição apenas uma vez em um controlador de aplicativo. Dessa forma, você pode evitar o código duplicado ao passar dados para uma página mestra de exibição em um aplicativo MVC ASP.NET.

> [!div class="step-by-step"]
> [Anterior](creating-page-layouts-with-view-master-pages-vb.md)
