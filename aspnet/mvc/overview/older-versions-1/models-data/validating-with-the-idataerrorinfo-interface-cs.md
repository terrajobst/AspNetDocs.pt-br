---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
title: Validando com a interface IDataErrorInfoC#() | Microsoft Docs
author: StephenWalther
description: Stephen Walther mostra como exibir mensagens de erro de validação personalizadas implementando a interface IDataErrorInfo em uma classe de modelo.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 4733b9f1-9999-48fb-8b73-6038fbcc5ecb
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: 938b180da02b1963acffd021d18621d75d1d0447
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78542556"
---
# <a name="validating-with-the-idataerrorinfo-interface-c"></a>Validação com a interface IDataErrorInfo (C#)

por [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther mostra como exibir mensagens de erro de validação personalizadas implementando a interface IDataErrorInfo em uma classe de modelo.

O objetivo deste tutorial é explicar uma abordagem para executar a validação em um aplicativo MVC ASP.NET. Você aprende a impedir que alguém envie um formulário HTML sem fornecer valores para os campos de formulário necessários. Neste tutorial, você aprenderá a executar a validação usando a interface IErrorDataInfo.

## <a name="assumptions"></a>Suposições

Neste tutorial, usarei o banco de dados MoviesDB e a tabela de banco de dados de filmes. Essa tabela tem as seguintes colunas:

<a id="0.5_table01"></a>

| **Nome da Coluna** | **Tipo de Dados** | **Permitir Nulos** |
| --- | --- | --- |
| ID | Int | Falso |
| Title | Nvarchar(100) | Falso |
| Diretor | Nvarchar(100) | Falso |
| DateReleased | Datetime | Falso |

Neste tutorial, uso o Microsoft Entity Framework para gerar minhas classes de modelo de banco de dados. A classe de filme gerada pelo Entity Framework é exibida na Figura 1.

[![a entidade de filme](validating-with-the-idataerrorinfo-interface-cs/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image1.png)

**Figura 01**: a entidade Movie ([clique para exibir a imagem em tamanho normal](validating-with-the-idataerrorinfo-interface-cs/_static/image2.png))

> [!NOTE] 
> 
> Para saber mais sobre como usar o Entity Framework para gerar suas classes de modelo de banco de dados, consulte meu tutorial intitulado criando classes de modelo com o Entity Framework.

## <a name="the-controller-class"></a>A classe do controlador

Usamos o controlador Home para listar filmes e criar novos filmes. O código para essa classe está contido na Listagem 1.

**Listagem 1-Controllers\HomeController.cs**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample1.cs)]

A classe de controlador Home na Listagem 1 contém duas ações Create (). A primeira ação exibe o formulário HTML para criar um novo filme. A segunda ação Create () executa a inserção real do novo filme no banco de dados. A segunda ação criar () é invocada quando o formulário exibido pela primeira ação criar () é enviado ao servidor.

Observe que a segunda ação Create () contém as seguintes linhas de código:

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample2.cs)]

A Propriedade IsValid retorna false quando há um erro de validação. Nesse caso, a exibição criar que contém o formulário HTML para criar um filme é reexibida.

## <a name="creating-a-partial-class"></a>Criando uma classe parcial

A classe de filme é gerada pelo Entity Framework. Você poderá ver o código para a classe Movie se expandir o arquivo MoviesDBModel. edmx na janela Gerenciador de Soluções e abrir o arquivo MoviesDBModel.Designer.cs no editor de código (consulte a Figura 2).

[![o código para a entidade de filme](validating-with-the-idataerrorinfo-interface-cs/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image3.png)

**Figura 02**: o código para a entidade de filme ([clique para exibir a imagem em tamanho normal](validating-with-the-idataerrorinfo-interface-cs/_static/image4.png))

A classe Movie é uma classe parcial. Isso significa que podemos adicionar outra classe parcial com o mesmo nome para estender a funcionalidade da classe Movie. Vamos adicionar nossa lógica de validação à nova classe parcial.

Adicione a classe na Listagem 2 à pasta modelos.

**Listagem 2-Models\Movie.cs**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample3.cs)]

Observe que a classe na Listagem 2 inclui o modificador *parcial* . Quaisquer métodos ou propriedades que você adicionar a essa classe se tornarão parte da classe de filme gerada pelo Entity Framework.

## <a name="adding-onchanging-and-onchanged-partial-methods"></a>Adicionando métodos parciais OnChange e onaltered

Quando o Entity Framework gera uma classe de entidade, o Entity Framework adiciona métodos parciais à classe automaticamente. O Entity Framework gera métodos parciais onalterion e OnChanged que correspondem a cada propriedade da classe.

No caso da classe Movie, o Entity Framework cria os seguintes métodos:

- OnIdChanging
- OnIdChanged
- OnTitleChanging
- Ontitlechanged
- OnDirectorChanging
- Ondirectorchanged
- OnDateReleasedChanging
- OnDateReleasedChanged

O método onmutate é chamado logo antes que a propriedade correspondente seja alterada. O método OnChanged é chamado logo depois que a propriedade é alterada.

Você pode aproveitar esses métodos parciais para adicionar lógica de validação à classe Movie. A classe de filme de atualização na Listagem 3 verifica se as propriedades Title e director recebem valores não vazios.

> [!NOTE] 
> 
> Um método parcial é um método definido em uma classe que você não precisa implementar. Se você não implementar um método parcial, o compilador removerá a assinatura do método e todas as chamadas para o método, de modo que não haja custos de tempo de execução associados ao método parcial. No editor de Visual Studio Code, você pode adicionar um método parcial digitando a palavra-chave *parcial* seguida por um espaço para exibir uma lista de parciais a serem implementados.

**Listagem 3-Models\Movie.cs**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample4.cs)]

Por exemplo, se você tentar atribuir uma cadeia de caracteres vazia à propriedade Title, uma mensagem de erro será atribuída a um dicionário chamado \_erros.

Neste ponto, nada realmente acontece quando você atribui uma cadeia de caracteres vazia à propriedade Title e um erro é adicionado ao campo Private \_Errors. Precisamos implementar a interface IDataErrorInfo para expor esses erros de validação para a estrutura MVC do ASP.NET.

## <a name="implementing-the-idataerrorinfo-interface"></a>Implementando a interface IDataErrorInfo

A interface IDataErrorInfo faz parte do .NET Framework desde a primeira versão. Essa interface é uma interface muito simples:

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample5.cs)]

Se uma classe implementar a interface IDataErrorInfo, a estrutura MVC do ASP.NET usará essa interface ao criar uma instância da classe. Por exemplo, a ação Create () do controlador Home aceita uma instância da classe Movie:

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample6.cs)]

A estrutura MVC do ASP.NET cria a instância do filme passada para a ação criar () usando um associador de modelo (o DefaultModelBinder). O associador de modelo é responsável por criar uma instância do objeto de filme ligando os campos de formulário HTML a uma instância do objeto de filme.

O DefaultModelBinder detecta se uma classe implementa ou não a interface IDataErrorInfo. Se uma classe implementar essa interface, o associador de modelo invocará o IDataErrorInfo. esse indexador para cada propriedade da classe. Se o indexador retornar uma mensagem de erro, o associador de modelo adicionará essa mensagem de erro ao estado do modelo automaticamente.

O DefaultModelBinder também verifica a propriedade IDataErrorInfo. Error. Esta propriedade destina-se a representar erros de validação específicos de não Propriedade associados à classe. Por exemplo, talvez você queira impor uma regra de validação que dependa dos valores de várias propriedades da classe Movie. Nesse caso, você retornaria um erro de validação da Propriedade Error.

A classe de filme atualizada na Listagem 4 implementa a interface IDataErrorInfo.

**Listagem 4-Models\Movie.cs (implementa IDataErrorInfo)**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample7.cs)]

Na Listagem 4, a propriedade indexador verifica a coleção de erros \_para ver se ela contém uma chave que corresponde ao nome da propriedade passado para o indexador. Se não houver nenhum erro de validação associado à propriedade, uma cadeia de caracteres vazia será retornada.

Você não precisa modificar o controlador inicial de qualquer forma para usar a classe de filme modificada. A página exibida na Figura 3 ilustra o que acontece quando nenhum valor é inserido para os campos de formulário título ou diretor.

[![criar métodos de ação automaticamente](validating-with-the-idataerrorinfo-interface-cs/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image5.png)

**Figura 03**: um formulário com valores ausentes ([clique para exibir a imagem em tamanho normal](validating-with-the-idataerrorinfo-interface-cs/_static/image6.png))

Observe que o valor de DateReleased é validado automaticamente. Como a propriedade DateReleased não aceita valores nulos, o DefaultModelBinder gera um erro de validação para essa propriedade automaticamente quando não tem um valor. Se você quiser modificar a mensagem de erro para a propriedade DateReleased, precisará criar um associador de modelo personalizado.

## <a name="summary"></a>Resumo

Neste tutorial, você aprendeu a usar a interface IDataErrorInfo para gerar mensagens de erro de validação. Primeiro, criamos uma classe de filme parcial que estende a funcionalidade da classe de filme parcial gerada pelo Entity Framework. Em seguida, adicionamos lógica de validação aos métodos parciais OnTitleChanging () e OnDirectorChanging () da classe Movie. Por fim, implementamos a interface IDataErrorInfo para expor essas mensagens de validação para a estrutura MVC do ASP.NET.

> [!div class="step-by-step"]
> [Anterior](performing-simple-validation-cs.md)
> [Próximo](validating-with-a-service-layer-cs.md)
