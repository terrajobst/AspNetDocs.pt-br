---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
title: Validando com uma camada deC#serviço () | Microsoft Docs
author: StephenWalther
description: Saiba como mover a lógica de validação para fora das ações do controlador e para uma camada de serviço separada. Neste tutorial, Stephen Walther explica como você...
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 4eabc535-b8a1-43f5-bb99-cfeb86db0fca
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: da1a1c9cc79a452eb0d7597810e86f7bcf6cd179
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78542857"
---
# <a name="validating-with-a-service-layer-c"></a>Validação com uma camada de serviço (C#)

por [Stephen Walther](https://github.com/StephenWalther)

> Saiba como mover a lógica de validação para fora das ações do controlador e para uma camada de serviço separada. Neste tutorial, Stephen Walther explica como você pode manter uma separação nítida de preocupações isolando sua camada de serviço da camada do controlador.

O objetivo deste tutorial é descrever um método de execução de validação em um aplicativo MVC ASP.NET. Neste tutorial, você aprende a mover a lógica de validação para fora de seus controladores e para uma camada de serviço separada.

## <a name="separating-concerns"></a>Separação de preocupações

Ao criar um aplicativo MVC do ASP.NET, você não deve posicionar a lógica do banco de dados dentro das ações do controlador. Misturar o banco de dados e a lógica do controlador torna seu aplicativo mais difícil de manter ao longo do tempo. A recomendação é que você coloque toda a lógica do banco de dados em uma camada de repositório separada.

Por exemplo, a listagem 1 contém um repositório simples chamado ProductRepository. O repositório do produto contém todo o código de acesso a dados para o aplicativo. A listagem também inclui a interface IProductRepository que o repositório do produto implementa.

**Listagem 1--Models\ProductRepository.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample1.cs)]

O controlador na Listagem 2 usa a camada de repositório em ambas as ações de índice () e Create (). Observe que esse controlador não contém nenhuma lógica de banco de dados. A criação de uma camada de repositório permite que você mantenha uma separação clara de preocupações. Os controladores são responsáveis pela lógica de controle de fluxo de aplicativo e o repositório é responsável pela lógica de acesso a dados.

**Listagem 2-Controllers\ProductController.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample2.cs)]

## <a name="creating-a-service-layer"></a>Criando uma camada de serviço

Portanto, a lógica de controle de fluxo de aplicativo pertence a um controlador e a lógica de acesso a dados pertence a um repositório. Nesse caso, onde você coloca sua lógica de validação? Uma opção é posicionar a lógica de validação em uma *camada de serviço*.

Uma camada de serviço é uma camada adicional em um aplicativo MVC ASP.NET que Media a comunicação entre um controlador e uma camada de repositório. A camada de serviço contém a lógica de negócios. Em particular, ele contém a lógica de validação.

Por exemplo, a camada de serviço do produto na Listagem 3 tem um método createproduct (). O método createproduct () chama o método ValidateProduct () para validar um novo produto antes de passar o produto para o repositório do produto.

**Listagem 3-Models\ProductService.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample3.cs)]

O controlador do produto foi atualizado na Listagem 4 para usar a camada de serviço em vez da camada de repositório. A camada do controlador se comunica com a camada de serviço. A camada de serviço se comunica com a camada de repositório. Cada camada tem uma responsabilidade separada.

**Listagem 4-Controllers\ProductController.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample4.cs)]

Observe que o serviço do produto é criado no construtor do controlador do produto. Quando o serviço do produto é criado, o dicionário de estado do modelo é passado para o serviço. O serviço do produto usa o estado do modelo para passar mensagens de erro de validação de volta para o controlador.

## <a name="decoupling-the-service-layer"></a>Desacoplando a camada de serviço

Falha ao isolar as camadas do controlador e do serviço em um aspecto. As camadas do controlador e do serviço se comunicam por meio do estado do modelo. Em outras palavras, a camada de serviço tem uma dependência em um recurso específico da estrutura MVC do ASP.NET.

Queremos isolar a camada de serviço da nossa camada de controlador tanto quanto possível. Teoricamente, devemos ser capazes de usar a camada de serviço com qualquer tipo de aplicativo e não apenas com um aplicativo ASP.NET MVC. Por exemplo, no futuro, talvez queiramos criar um front-end do WPF para nosso aplicativo. Devemos encontrar uma maneira de remover a dependência no estado do modelo MVC ASP.NET de nossa camada de serviço.

Na listagem 5, a camada de serviço foi atualizada para que ela não use mais o estado do modelo. Em vez disso, ele usa qualquer classe que implemente a interface IValidationDictionary.

**Listagem 5-Models\ProductService.cs (dissociado)**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample5.cs)]

A interface IValidationDictionary é definida na Listagem 6. Essa interface simples tem um único método e uma única propriedade.

**Listagem 6-Models\IValidationDictionary.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample6.cs)]

A classe na Listagem 7, chamada de classe ModelStateWrapper, implementa a interface IValidationDictionary. Você pode instanciar a classe ModelStateWrapper passando um dicionário de estado do modelo para o construtor.

**Listagem 7-Models\ModelStateWrapper.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample7.cs)]

Por fim, o controlador atualizado na Listagem 8 usa o ModelStateWrapper ao criar a camada de serviço em seu construtor.

**Listagem 8-Controllers\ProductController.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample8.cs)]

Usar a interface IValidationDictionary e a classe ModelStateWrapper nos permite isolar completamente nossa camada de serviço de nossa camada de controlador. A camada de serviço não depende mais do estado do modelo. Você pode passar qualquer classe que implemente a interface IValidationDictionary para a camada de serviço. Por exemplo, um aplicativo WPF pode implementar a interface IValidationDictionary com uma classe de coleção simples.

## <a name="summary"></a>Resumo

O objetivo deste tutorial foi discutir uma abordagem para executar a validação em um aplicativo MVC ASP.NET. Neste tutorial, você aprendeu a mover toda a lógica de validação para fora de seus controladores e para uma camada de serviço separada. Você também aprendeu como isolar sua camada de serviço da camada do controlador criando uma classe ModelStateWrapper.

> [!div class="step-by-step"]
> [Anterior](validating-with-the-idataerrorinfo-interface-cs.md)
> [Próximo](validation-with-the-data-annotation-validators-cs.md)
