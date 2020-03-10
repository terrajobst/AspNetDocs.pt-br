---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'Tutorial: personalizar o modo de exibição para o EF Database First com o aplicativo MVC ASP.NET'
description: Este tutorial se concentra em alterar as exibições geradas automaticamente para aprimorar a apresentação.
author: Rick-Anderson
ms.author: riande
ms.date: 01/24/2019
ms.topic: tutorial
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: 89b8a0eb84b6e287c45bc141c68a2c76e63b0e41
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583590"
---
# <a name="tutorial-customize-view-for-ef-database-first-with-aspnet-mvc-app"></a>Tutorial: personalizar o modo de exibição para o EF Database First com o aplicativo MVC ASP.NET

Usando MVC, Entity Framework e ASP.NET scaffolding, você pode criar um aplicativo Web que fornece uma interface para um banco de dados existente. Esta série de tutoriais mostra como gerar automaticamente o código que permite que os usuários exibam, editem, criem e excluam dados que residem em uma tabela. O código gerado corresponde às colunas na tabela de banco de dados.

Este tutorial se concentra em alterar as exibições geradas automaticamente para aprimorar a apresentação.

Neste tutorial, você:

> [!div class="checklist"]
> * Adicionar cursos à página de detalhes do aluno
> * Confirmar que os cursos são adicionados à página

## <a name="prerequisites"></a>Prerequisites

* [Alterar o banco de dados](changing-the-database.md)

## <a name="add-courses-to-student-detail"></a>Adicionar cursos aos detalhes do aluno

O código gerado fornece um bom ponto de partida para seu aplicativo, mas ele não fornece necessariamente todas as funcionalidades de que você precisa em seu aplicativo. Você pode personalizar o código para atender aos requisitos específicos do seu aplicativo. Atualmente, seu aplicativo não exibe os cursos registrados para o aluno selecionado. Nesta seção, você adicionará os cursos registrados para cada aluno à exibição de **detalhes** do aluno.

Abra **exibições** > **alunos** > *Details. cshtml*. Abaixo da última &lt;/DL&gt; marca, mas antes de fechar &lt;marca de&gt; de/div, adicione o código a seguir.

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

Esse código cria uma tabela que exibe uma linha para cada registro na tabela de registro do aluno selecionado. O método de **exibição** renderiza HTML para o objeto (ModelItem) que representa a expressão. Você usa o método de exibição (em vez de simplesmente inserir o valor da propriedade no código) para garantir que o valor seja formatado corretamente com base em seu tipo e no modelo para esse tipo. Neste exemplo, cada expressão retorna uma única propriedade do registro atual no loop e os valores são tipos primitivos que são renderizados como texto.

## <a name="confirm-courses-are-added"></a>Confirmar que os cursos foram adicionados

Execute a solução. Clique em **lista de alunos** e selecione **detalhes** para um dos alunos. Você verá que os cursos registrados foram incluídos na exibição.

![aluno com registro](customizing-a-view/_static/image1.png)

## <a name="next-steps"></a>Próximas etapas
Neste tutorial, você:

> [!div class="checklist"]
> * Cursos adicionados à página de detalhes do aluno
> * Confirmado que os cursos são adicionados à página

Avance para o próximo tutorial para saber como adicionar anotações de dados para especificar requisitos de validação e formatação de exibição.
> [!div class="nextstepaction"]
> [Aprimorar a validação de dados](enhancing-data-validation.md)
