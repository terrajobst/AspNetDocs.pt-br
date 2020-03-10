---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: Classificando, paginando e Filtrando dados com associação de modelo e formulários da Web | Microsoft Docs
author: Rick-Anderson
description: Esta série de tutoriais demonstra os aspectos básicos do uso de associação de modelo com um projeto ASP.NET Web Forms. A associação de modelo torna a interação de dados mais direta-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: f8e64392af6110f36c6af98c4e4e9481c94a0d82
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78548058"
---
# <a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a>Classificando, paginando e Filtrando dados com associação de modelo e formulários da Web

por [Tom FitzMacken](https://github.com/tfitzmac)

> Esta série de tutoriais demonstra os aspectos básicos do uso de associação de modelo com um projeto ASP.NET Web Forms. A associação de modelo torna a interação de dados mais direta do que lidar com objetos de fonte de dados (como ObjectDataSource ou SqlDataSource). Esta série começa com material introdutório e passa para conceitos mais avançados em Tutoriais posteriores.
> 
> Este tutorial mostra como adicionar classificação, paginação e filtragem dos dados por meio de associação de modelo.
> 
> Este tutorial se baseia no projeto criado na primeira [parte](retrieving-data.md) da série.
> 
> Você pode [baixar](https://go.microsoft.com/fwlink/?LinkId=286116) o projeto completo no C# ou no VB. O código para download funciona com o Visual Studio 2012 ou Visual Studio 2013. Ele usa o modelo do Visual Studio 2012, que é um pouco diferente do modelo de Visual Studio 2013 mostrado neste tutorial.

## <a name="what-youll-build"></a>O que você criará

Neste tutorial, você aprenderá a:

1. Habilitar classificação e paginação dos dados
2. Habilitar a filtragem dos dados com base em uma seleção pelo usuário

## <a name="add-sorting"></a>Adicionar classificação

Habilitar a classificação no GridView é muito fácil. No arquivo Student. aspx, basta definir **AllowSorting** como **true** no GridView. Você não precisa definir um valor de **SortExpression** para cada coluna, pois DataField é usado automaticamente. O GridView modifica a consulta para incluir a ordenação dos dados pelo valor selecionado. O código realçado abaixo mostra a adição que você precisa fazer para habilitar a classificação.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

Execute o aplicativo Web e teste classificando os registros de aluno pelos valores em colunas diferentes.

![classificar alunos](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a>Adicionar paginação

Habilitar a paginação também é muito fácil. No GridView, defina a propriedade **AllowPaging** como **true** e defina a propriedade **PageSize** como o número de registros que você deseja exibir em cada página. Neste tutorial, você pode defini-lo como 4.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

Execute o aplicativo Web e observe que agora os registros são divididos em várias páginas com até 4 registros exibidos em uma única página.

![adicionar paginação](sorting-paging-and-filtering-data/_static/image4.png)

A execução da consulta adiada melhora a eficiência do aplicativo. Em vez de recuperar todo o conjunto de dados, o GridView modifica a consulta para recuperar somente os registros da página atual.

## <a name="filter-records-by-user-selection"></a>Filtrar registros por seleção de usuário

A associação de modelo adiciona vários atributos que permitem designar como definir o valor de um parâmetro em um método de associação de modelo. Esses atributos estão no namespace **System. Web. modelbinding** . Elas incluem:

- Control
- Cookie
- Formulário
- Perfil
- QueryString
- RouteData
- Session
- UserProfile
- ViewState

Neste tutorial, você usará o valor de um controle para filtrar quais registros são exibidos no GridView. Você adicionará o atributo de **controle** ao método de consulta criado anteriormente. Em um tutorial [posterior](using-query-string-values-to-retrieve-data.md) , você aplicará o atributo **QueryString** a um parâmetro para especificar que o valor do parâmetro é proveniente de um valor de cadeia de caracteres de consulta.

Primeiro, acima do ValidationSummary, adicione uma lista suspensa para filtrar quais alunos são mostrados.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

No arquivo code-behind, modifique o método Select para receber um valor do controle e defina o nome do parâmetro como o nome do controle que fornece o valor.

Você deve adicionar uma instrução **using** para o namespace **System. Web. modelbinding** para resolver o atributo de controle.

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

O código a seguir mostra o método Select trabalhado novamente para filtrar os dados retornados com base no valor da lista suspensa. A adição de um atributo de controle antes de um parâmetro especifica que o valor desse parâmetro é proveniente de um controle com o mesmo nome.

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

Execute o aplicativo Web e selecione valores diferentes na lista suspensa para filtrar a lista de alunos.

![filtrar alunos](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a>Conclusão

Neste tutorial, você habilitou a classificação e a paginação dos dados. Você também habilitou a filtragem dos dados pelo valor de um controle.

No próximo [tutorial](integrating-jquery-ui.md) , você aprimorará a interface do usuário integrando um widget da interface do usuário do jQuery ao modelo de dados dinâmicos.

> [!div class="step-by-step"]
> [Anterior](updating-deleting-and-creating-data.md)
> [Próximo](integrating-jquery-ui.md)
