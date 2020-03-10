---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: Usando valores de cadeia de caracteres de consulta para filtrar dados com associação de modelo e formulários da Web | Microsoft Docs
author: Rick-Anderson
description: Esta série de tutoriais demonstra os aspectos básicos do uso de associação de modelo com um projeto ASP.NET Web Forms. A associação de modelo torna a interação de dados mais direta-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: 143ddcb40b576a3129e659b90bfc8321c061a547
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78639093"
---
# <a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a>Usando valores de cadeia de caracteres de consulta para filtrar dados com associação de modelo e formulários da Web

por [Tom FitzMacken](https://github.com/tfitzmac)

> Esta série de tutoriais demonstra os aspectos básicos do uso de associação de modelo com um projeto ASP.NET Web Forms. A associação de modelo torna a interação de dados mais direta do que lidar com objetos de fonte de dados (como ObjectDataSource ou SqlDataSource). Esta série começa com material introdutório e passa para conceitos mais avançados em Tutoriais posteriores.
> 
> Este tutorial mostra como passar um valor na cadeia de caracteres de consulta e usar esse valor para recuperar dados por meio de associação de modelo.
> 
> Este tutorial se baseia no projeto criado nas partes [anteriores](retrieving-data.md) da série.
> 
> Você pode [baixar](https://go.microsoft.com/fwlink/?LinkId=286116) o projeto completo no C# ou no VB. O código para download funciona com o Visual Studio 2012 ou Visual Studio 2013. Ele usa o modelo do Visual Studio 2012, que é um pouco diferente do modelo de Visual Studio 2013 mostrado neste tutorial.

## <a name="what-youll-build"></a>O que você criará

Neste tutorial, você aprenderá a:

1. Adicionar uma nova página para mostrar os cursos registrados para um aluno
2. Recuperar os cursos registrados para o estudante selecionado com base em um valor na cadeia de caracteres de consulta
3. Adicionar um hiperlink com um valor de cadeia de caracteres de consulta do modo de exibição de grade para a nova página

As etapas neste tutorial são bastante semelhantes às que você fez no [tutorial](sorting-paging-and-filtering-data.md) anterior para filtrar os alunos exibidos com base na seleção de usuário em uma lista suspensa. Nesse tutorial, você usou o atributo **Control** no método Select para especificar que o valor do parâmetro vem de um controle. Neste tutorial, você usará o atributo **QueryString** no método Select para especificar que o valor do parâmetro é proveniente da cadeia de caracteres de consulta.

## <a name="add-new-page-for-displaying-a-students-courses"></a>Adicionar nova página para exibir os cursos de um aluno

Adicione um novo formulário da Web que use a página mestra site. Master e nomeie a página **cursos**.

No arquivo **courses. aspx** , adicione um modo de exibição de grade para exibir os cursos para o aluno selecionado.

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a>Definir o método Select

Em **courses.aspx.cs**, você adicionará o método Select com o nome especificado na propriedade **SelectMethod** da exibição de grade. Nesse método, você definirá a consulta para recuperar os cursos de um aluno e especificará que o parâmetro é proveniente de um valor de cadeia de caracteres de consulta com o mesmo nome que o parâmetro.

Primeiro, você deve adicionar as instruções **using** a seguir.

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

Em seguida, adicione o seguinte código a Courses.aspx.cs:

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

O atributo QueryString significa que um valor de cadeia de caracteres de consulta chamado StudentId é atribuído automaticamente ao parâmetro nesse método.

## <a name="add-hyperlink-with-query-string-value"></a>Adicionar hiperlink com o valor da cadeia de caracteres de consulta

No modo de exibição de grade em students. aspx, você adicionará um campo de hiperlink vinculado à sua nova página de cursos. O hiperlink incluirá um valor de cadeia de caracteres de consulta com a ID do aluno.

Em students. aspx, adicione o campo a seguir às colunas de exibição de grade logo abaixo do campo para créditos totais.

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

Execute o aplicativo e observe que o modo de exibição de grade agora inclui o link cursos.

![Adicionar hiperlink](using-query-string-values-to-retrieve-data/_static/image1.png)

Ao clicar em um dos links, você verá os cursos registrados do aluno.

![Mostrar cursos](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a>Conclusão

Neste tutorial, você adicionou um link com um valor de cadeia de caracteres de consulta. Você usou esse valor de cadeia de caracteres de consulta para o valor do parâmetro no método Select.

No próximo [tutorial](adding-business-logic-layer.md), você moverá o código dos arquivos code-behind para uma camada de lógica de negócios e uma camada de acesso a dados.

> [!div class="step-by-step"]
> [Anterior](integrating-jquery-ui.md)
> [Próximo](adding-business-logic-layer.md)
