---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: Validação de modelo no ASP.NET Web API-ASP.NET 4. x
author: MikeWasson
description: Visão geral da validação de modelo no ASP.NET Web API para ASP.NET 4. x.
ms.author: riande
ms.date: 07/20/2012
ms.custom: seoapril2019
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 531a66b7ab642bd012663517640f2766f1917f25
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557235"
---
# <a name="model-validation-in-aspnet-web-api"></a>Validação de modelo no ASP.NET Web API

por [Mike Wasson](https://github.com/MikeWasson)

Este artigo mostra como anotar seus modelos, usar as anotações para validação de dados e tratar erros de validação em sua API Web. Quando um cliente envia dados para sua API Web, geralmente você deseja validar os dados antes de fazer qualquer processamento. 

## <a name="data-annotations"></a>Anotações de dados

No ASP.NET Web API, você pode usar atributos do namespace [System. ComponentModel. Annotations](/dotnet/api/system.componentmodel.dataannotations) para definir regras de validação para propriedades em seu modelo. Considere o modelo a seguir:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

Se você usou a validação de modelo no ASP.NET MVC, isso deve parecer familiar. O atributo **Required** indica que a propriedade `Name` não deve ser nula. O atributo **Range** indica que `Weight` deve estar entre zero e 999.

Suponha que um cliente envie uma solicitação POST com a seguinte representação JSON:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

Você pode ver que o cliente não incluiu a propriedade `Name`, que está marcada como necessária. Quando a API da Web converte o JSON em uma `Product` instância, ela valida a `Product` em relação aos atributos de validação. Na ação do controlador, você pode verificar se o modelo é válido:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

A validação de modelo não garante que os dados do cliente sejam seguros. A validação adicional pode ser necessária em outras camadas do aplicativo. (Por exemplo, a camada de dados pode impor restrições Foreign Key). O tutorial [usando a API Web com o Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) explora alguns desses problemas.

**"Em lançamento"** : em lançamento acontece quando o cliente sai de algumas propriedades. Por exemplo, suponha que o cliente envie o seguinte:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

Aqui, o cliente não especificou valores para `Price` ou `Weight`. O formatador JSON atribui um valor padrão igual a zero às propriedades ausentes.

![](model-validation-in-aspnet-web-api/_static/image1.png)

O estado do modelo é válido, pois zero é um valor válido para essas propriedades. Se esse é um problema depende do seu cenário. Por exemplo, em uma operação de atualização, talvez você queira distinguir entre "zero" e "não definido". Para forçar os clientes a definir um valor, torne a propriedade anulável e defina o atributo **necessário** :

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

**"Excesso de postagem"** : um cliente também pode enviar *mais* dados do que o esperado. Por exemplo:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

Aqui, o JSON inclui uma propriedade ("Color") que não existe no modelo de `Product`. Nesse caso, o formatador JSON simplesmente ignora esse valor. (O formatador XML faz o mesmo.) O excesso de postagens causará problemas se seu modelo tiver propriedades que você pretende que sejam somente leitura. Por exemplo:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

Você não quer que os usuários atualizem a propriedade `IsAdmin` e se elevem a administradores! A estratégia mais segura é usar uma classe de modelo que corresponda exatamente ao que o cliente tem permissão para enviar:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> A postagem do blog de Brad Wilson "[validação de entrada versus validação de modelo no ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)" tem uma boa discussão sobre lançamento e excesso de postagem. Embora a postagem seja sobre o ASP.NET MVC 2, os problemas ainda são relevantes para a API da Web.

## <a name="handling-validation-errors"></a>Tratamento de erros de validação

A API da Web não retorna automaticamente um erro para o cliente quando a validação falha. Cabe à ação do controlador verificar o estado do modelo e responder adequadamente.

Você também pode criar um filtro de ação para verificar o estado do modelo antes que a ação do controlador seja invocada. O código a seguir mostra um exemplo:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

Se a validação do modelo falhar, esse filtro retornará uma resposta HTTP que contém os erros de validação. Nesse caso, a ação do controlador não é invocada.

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

Para aplicar esse filtro a todos os controladores de API da Web, adicione uma instância do filtro à coleção **HttpConfiguration. Filters** durante a configuração:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

Outra opção é definir o filtro como um atributo em controladores individuais ou ações do controlador:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
