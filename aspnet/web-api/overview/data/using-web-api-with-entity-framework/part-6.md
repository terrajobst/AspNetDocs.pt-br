---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: Criar o cliente JavaScript | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: 74f2cc4e5e401d690042b05b028dfc0c46ae282a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622342"
---
# <a name="create-the-javascript-client"></a>Criar o cliente de JavaScript

por [Mike Wasson](https://github.com/MikeWasson)

[Baixar projeto concluído](https://github.com/MikeWasson/BookService)

Nesta seção, você criará o cliente para o aplicativo, usando HTML, JavaScript e a biblioteca [knockout. js](http://knockoutjs.com/) . Vamos criar o aplicativo cliente em estágios:

- Mostrando uma lista de livros.
- Mostrando um detalhe do livro.
- Adicionando um novo livro.

A biblioteca de Knockout usa o padrão Model-View-ViewModel (MVVM):

- O **modelo** é a representação do lado do servidor dos dados no domínio de negócios (no nosso caso, livros e autores).
- A **exibição** é a camada de apresentação (HTML).
- O **modelo de exibição** é um objeto JavaScript que contém os modelos. O modelo de exibição é uma abstração de código da interface do usuário. Ele não tem conhecimento da representação HTML. Em vez disso, ele representa recursos abstratos da exibição, como &quot;uma lista de livros&quot;.

A exibição é associada a dados para o modelo de exibição. As atualizações para o modelo de exibição são refletidas automaticamente na exibição. O modelo de exibição também obtém eventos da exibição, como cliques de botão.

![](part-6/_static/image1.png)

Essa abordagem facilita a alteração do layout e da interface do usuário do seu aplicativo, pois você pode alterar as associações sem reescrever nenhum código. Por exemplo, você pode mostrar uma lista de itens como um `<ul>`e, em seguida, alterá-lo mais tarde para uma tabela.

## <a name="add-the-knockout-library"></a>Adicionar a biblioteca de Knockouts

No Visual Studio, no menu **ferramentas** , selecione **Gerenciador de pacotes NuGet**. Em seguida, selecione o **Console do Gerenciador de Pacotes**. Na janela Console do Gerenciador de Pacotes, digite o seguinte comando:

[!code-console[Main](part-6/samples/sample1.cmd)]

Esse comando adiciona os arquivos de Knockout à pasta scripts.

## <a name="create-the-view-model"></a>Criar o modelo de exibição

Adicione um arquivo JavaScript chamado App. js à pasta scripts. (Em Gerenciador de Soluções, clique com o botão direito do mouse na pasta scripts, selecione **Adicionar**e, em seguida, selecione **arquivo JavaScript**.) Cole o código a seguir:

[!code-javascript[Main](part-6/samples/sample2.js)]

No knockout, a classe `observable` permite a vinculação de dados. Quando o conteúdo de uma alteração observável, o observável notifica todos os controles vinculados a dados para que eles possam se atualizar. (A classe `observableArray` é a versão de matriz de *Observ*.) Para começar, nosso modelo de exibição tem dois observáveis:

- `books` mantém a lista de livros.
- `error` contém uma mensagem de erro se uma chamada AJAX falhar.

O método `getAllBooks` faz uma chamada AJAX para obter a lista de livros. Em seguida, ele envia o resultado para a matriz de `books`.

O método `ko.applyBindings` faz parte da biblioteca de Knockouts. Ele usa o modelo de exibição como um parâmetro e configura a vinculação de dados.

## <a name="add-a-script-bundle"></a>Adicionar um pacote de script

O agrupamento é um recurso do ASP.NET 4,5 que torna mais fácil combinar ou agrupar vários arquivos em um único arquivo. O agrupamento reduz o número de solicitações para o servidor, o que pode melhorar o tempo de carregamento da página.

Abra o aplicativo de arquivo\_Start/BundleConfig. cs. Adicione o código a seguir ao método RegisterBundles.

[!code-csharp[Main](part-6/samples/sample3.cs)]

> [!div class="step-by-step"]
> [Anterior](part-5.md)
> [Próximo](part-7.md)
