---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'Tutorial: gerar exibições para o EF Database First com o aplicativo MVC ASP.NET'
description: Este tutorial se concentra no uso do ASP.NET scaffolding para gerar os controladores e as exibições.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: e71e13e22d8a72e1699cfc70d4d93af603edba5b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616203"
---
# <a name="tutorial-generate-views-for-ef-database-first-with-aspnet-mvc-app"></a>Tutorial: gerar exibições para o EF Database First com o aplicativo MVC ASP.NET

Usando MVC, Entity Framework e ASP.NET scaffolding, você pode criar um aplicativo Web que fornece uma interface para um banco de dados existente. Esta série de tutoriais mostra como gerar automaticamente o código que permite que os usuários exibam, editem, criem e excluam dados que residem em uma tabela. O código gerado corresponde às colunas na tabela de banco de dados.

Este tutorial se concentra no uso do ASP.NET scaffolding para gerar os controladores e as exibições.

Neste tutorial, você:

> [!div class="checklist"]
> * Adicionar Scaffold
> * Adicionar links a novas exibições
> * Exibir exibições de aluno
> * Exibir exibições de registro

## <a name="prerequisite"></a>Pré-requisito

* [Criar o aplicativo Web e modelos de dados](creating-the-web-application.md)

## <a name="add-scaffold"></a>Adicionar Scaffold

Você está pronto para gerar código que fornecerá operações de dados padrão para as classes de modelo. Você adiciona o código adicionando um item Scaffold. Há muitas opções para o tipo de scaffolding que você pode adicionar; Neste tutorial, o scaffold incluirá um controlador e exibições que correspondem aos modelos de estudante e de registro que você criou na seção anterior.

Para manter a consistência em seu projeto, você adicionará o novo controlador à pasta **controladores** existentes. Clique com o botão direito do mouse na pasta **controladores** e selecione **Adicionar** > **novo item com Scaffold**.

Selecione o **controlador MVC 5 com exibições, usando Entity Framework** opção. Esta opção gerará o controlador e as exibições para atualizar, excluir, criar e exibir os dados em seu modelo.

![Adicionar controlador MVC](generating-views/_static/image2.png)

Selecione **aluno (ContosoSite. Models)** para a classe de modelo e selecione o **ContosoUniversityDataEntities (ContosoSite. Models)** para a classe de contexto. Mantenha o nome do controlador como **StudentsController**.

Clique em **Adicionar**.

Se você receber um erro, talvez seja porque você não criou o projeto na seção anterior. Nesse caso, tente Compilar o projeto e, em seguida, adicione o item com Scaffold novamente.

Depois que o processo de geração de código for concluído, você verá um novo controlador e exibições nos **controladores** e **exibições** do seu projeto > pastas dos **alunos** .

Execute as mesmas etapas novamente, mas adicione um Scaffold para a classe de **registro** . Quando terminar, você terá um arquivo **EnrollmentsController.cs** e uma pasta em **exibições** nomeadas como **registros** com as exibições criar, excluir, detalhes, editar e indexar.

## <a name="add-links-to-new-views"></a>Adicionar links a novas exibições

Para facilitar a navegação para suas novas exibições, você pode adicionar alguns hiperlinks às exibições de índice para estudantes e registros. Abra o arquivo em **exibições** > **página inicial** > *index. cshtml*, que é o home page do seu site. Adicione o seguinte código abaixo do Jumbotron.

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

Para o método ActionLink, o primeiro parâmetro é o texto a ser exibido no link. O segundo parâmetro é a ação e o terceiro parâmetro é o nome do controlador. Por exemplo, o primeiro link aponta para a ação de índice em StudentsController. O hiperlink real é construído com base nesses valores. O primeiro link leva os usuários para o arquivo **index. cshtml** dentro da pasta **views/Students** .

## <a name="display-student-views"></a>Exibir exibições de aluno

Você verificará se o código adicionado ao seu projeto exibe corretamente uma lista de alunos e permite que os usuários editem, criem ou excluam os registros de aluno no banco de dados.

Clique com o botão direito do mouse no arquivo **views** > **Home** > *index. cshtml* e selecione **Exibir no navegador**. No home page do aplicativo, selecione **lista de alunos**.

![](generating-views/_static/image6.png)

Na página **índice** , observe a lista de alunos e links para modificar esses dados. Selecione o link **criar novo** e forneça alguns valores para um novo aluno. Clique em **criar**e observe que o novo aluno é adicionado à sua lista.

De volta à página **índice** , selecione o link **Editar** e altere alguns dos valores de um aluno. Clique em **salvar**e observe que o registro do aluno foi alterado.

Por fim, selecione o link **excluir** e confirme que você deseja excluir o registro clicando no botão **excluir** .

Sem escrever nenhum código, você adicionou exibições que executam operações comuns nos dados na tabela Student.

Talvez você tenha notado que o rótulo de texto de um campo é baseado na Propriedade do banco de dados (como **LastName**), que não é necessariamente o que você deseja exibir na página da Web. Por exemplo, você pode preferir que o rótulo seja **sobrenome**. Você corrigirá esse problema de exibição posteriormente no tutorial.

## <a name="display-enrollment-views"></a>Exibir exibições de registro

Seu banco de dados inclui uma relação um-para-muitos entre as tabelas Student e de registro e uma relação um-para-muitos entre as tabelas curso e registro. As exibições do registro manipulam corretamente essas relações. Navegue até o home page do seu site e selecione o link **lista de registros** e, em seguida, o link **criar novo** .

A exibição exibe um formulário para criar um novo registro de registro. Em particular, observe que o formulário contém uma lista suspensa **cursoid** e uma lista suspensa **StudentId** . Ambos são preenchidos com valores das tabelas relacionadas.

Além disso, a validação dos valores fornecidos é aplicada automaticamente com base no tipo de dados do campo. A **classificação** requer um número, portanto, uma mensagem de erro será exibida se você tentar fornecer um valor incompatível: *a classificação do campo deve ser um número.*

Você verificou que as exibições geradas automaticamente permitem que os usuários trabalhem com os dados no banco de dado. No próximo tutorial desta série, você atualizará o banco de dados e fará as alterações correspondentes no aplicativo Web.

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você:

> [!div class="checklist"]
> * Scaffold adicionados
> * Links adicionados a novas exibições
> * Exibições de aluno exibidas
> * Exibições de registro exibidas

Avance para o próximo tutorial para saber como alterar o banco de dados.
> [!div class="nextstepaction"]
> [Alterar o banco de dados](changing-the-database.md)