---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: Adicionando a camada de lógica de negócios a um projeto que usa associação de modelo e formulários da Web | Microsoft Docs
author: Rick-Anderson
description: Esta série de tutoriais demonstra os aspectos básicos do uso de associação de modelo com um projeto ASP.NET Web Forms. A associação de modelo torna a interação de dados mais direta-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: a824d06d3781e11706f2a48d44ea3ad89bdb7c8b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634830"
---
# <a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a>Adicionando a camada de lógica de negócios a um projeto que usa associação de modelo e formulários da Web

por [Tom FitzMacken](https://github.com/tfitzmac)

> Esta série de tutoriais demonstra os aspectos básicos do uso de associação de modelo com um projeto ASP.NET Web Forms. A associação de modelo torna a interação de dados mais direta do que lidar com objetos de fonte de dados (como ObjectDataSource ou SqlDataSource). Esta série começa com material introdutório e passa para conceitos mais avançados em Tutoriais posteriores.
> 
> Este tutorial mostra como usar a associação de modelo com uma camada de lógica de negócios. Você definirá o membro OnCallingDataMethods para especificar que um objeto diferente da página atual é usado para chamar os métodos de dados.
> 
> Este tutorial se baseia no projeto criado nas partes [anteriores](retrieving-data.md) da série.
> 
> Você pode [baixar](https://go.microsoft.com/fwlink/?LinkId=286116) o projeto completo no C# ou no VB. O código para download funciona com o Visual Studio 2012 ou Visual Studio 2013. Ele usa o modelo do Visual Studio 2012, que é um pouco diferente do modelo de Visual Studio 2013 mostrado neste tutorial.

## <a name="what-youll-build"></a>O que você criará

A associação de modelo permite colocar o código de interação de dados no arquivo code-behind para uma página da Web ou em uma classe lógica de negócios separada. Os tutoriais anteriores mostraram como usar os arquivos code-behind para o código de interação de dados. Essa abordagem funciona para sites pequenos, mas pode levar à repetição de código e maior dificuldade ao manter um site grande. Também pode ser muito difícil testar programaticamente o código que reside em arquivos code-behind porque não há nenhuma camada de abstração.

Para centralizar o código de interação de dados, você pode criar uma camada de lógica de negócios que contenha toda a lógica para interagir com os dados. Em seguida, você chama a camada de lógica de negócios em suas páginas da Web. Este tutorial mostra como mover todo o código que você escreveu nos tutoriais anteriores em uma camada de lógica de negócios e, em seguida, usar esse código nas páginas.

Neste tutorial, você aprenderá a:

1. Mover o código dos arquivos code-behind para uma camada de lógica de negócios
2. Alterar os controles associados a dados para chamar os métodos na camada de lógica de negócios

## <a name="create-business-logic-layer"></a>Criar camada de lógica de negócios

Agora, você criará a classe que é chamada a partir das páginas da Web. Os métodos nessa classe são semelhantes aos métodos usados nos tutoriais anteriores e incluem os atributos do provedor de valor.

Primeiro, adicione uma nova pasta chamada **BLL**.

![Adicionar pasta](adding-business-logic-layer/_static/image1.png)

Na pasta BLL, crie uma nova classe chamada **SchoolBL.cs**. Ele conterá todas as operações de dados que residiram originalmente nos arquivos code-behind. Os métodos são quase iguais aos métodos no arquivo code-behind, mas incluirão algumas alterações.

A alteração mais importante a ser observada é que você não está mais executando o código de dentro de uma instância da classe de **página** . A classe Page contém o método **TryUpdateModel** e a propriedade **ModelState** . Quando esse código é movido para uma camada de lógica de negócios, você não tem mais uma instância da classe de página para chamar esses membros. Para contornar esse problema, você deve adicionar um parâmetro **ModelMethodContext** a qualquer método que acesse TryUpdateModel ou ModelState. Use esse parâmetro ModelMethodContext para chamar TryUpdateModel ou recuperar ModelState. Você não precisa alterar nada na página da Web para considerar esse novo parâmetro.

Substitua o código em SchoolBL.cs pelo código a seguir.

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a>Revisar páginas existentes para recuperar dados da camada de lógica de negócios

Por fim, você converterá as páginas students. aspx, addstudent. aspx e courses. aspx de usar consultas no arquivo code-behind para usar a camada de lógica de negócios.

Nos arquivos code-behind para estudantes, mystudent e cursos, exclua ou comente os seguintes métodos de consulta:

- studentsGrid\_GetData
- studentsGrid\_UpdateItem
- studentsGrid\_DeleteItem
- addStudentForm\_InsertItem
- coursesGrid\_GetData

Agora você não deve ter nenhum código no arquivo code-behind que pertença a operações de dados.

O manipulador de eventos **OnCallingDataMethods** permite que você especifique um objeto a ser usado para os métodos de dados. Em students. aspx, adicione um valor para esse manipulador de eventos e altere os nomes dos métodos de dados para os nomes dos métodos na classe lógica de negócios.

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

No arquivo code-behind para students. aspx, defina o manipulador de eventos para o evento CallingDataMethods. Nesse manipulador de eventos, você especifica a classe de lógica de negócios para operações de dados.

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

Em addstudent. aspx, faça alterações semelhantes.

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

Em cursos. aspx, faça alterações semelhantes.

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

Execute o aplicativo e observe que todas as páginas funcionam como estavam anteriormente. A lógica de validação também funciona corretamente.

## <a name="conclusion"></a>Conclusão

Neste tutorial, você reestruturará seu aplicativo para usar uma camada de acesso a dados e a lógica de negócios. Você especificou que os controles de dados usam um objeto que não é a página atual para operações de dados.

> [!div class="step-by-step"]
> [Anterior](using-query-string-values-to-retrieve-data.md)
