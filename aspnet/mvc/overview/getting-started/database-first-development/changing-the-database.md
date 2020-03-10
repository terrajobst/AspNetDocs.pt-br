---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'Tutorial: alterar o banco de dados para o EF Database First com o aplicativo MVC ASP.NET'
description: Este tutorial se concentra em fazer uma atualização para a estrutura do banco de dados e propagar essa alteração em todo o aplicativo Web.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 52cad1120908cf0d4f85770f8e2690f9415c5f56
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616259"
---
# <a name="tutorial-change-the-database-for-ef-database-first-with-aspnet-mvc-app"></a>Tutorial: alterar o banco de dados para o EF Database First com o aplicativo MVC ASP.NET

Usando MVC, Entity Framework e ASP.NET scaffolding, você pode criar um aplicativo Web que fornece uma interface para um banco de dados existente. Esta série de tutoriais mostra como gerar automaticamente o código que permite que os usuários exibam, editem, criem e excluam dados que residem em uma tabela. O código gerado corresponde às colunas na tabela de banco de dados.

Este tutorial se concentra em fazer uma atualização para a estrutura do banco de dados e propagar essa alteração em todo o aplicativo Web.

Neste tutorial, você:

> [!div class="checklist"]
> * Adicionar uma coluna
> * Adicionar a propriedade às exibições

## <a name="prerequisites"></a>Prerequisites

* [Gerando exibições](generating-views.md)

## <a name="add-a-column"></a>Adicionar uma coluna

Se você atualizar a estrutura de uma tabela em seu banco de dados, precisará garantir que sua alteração seja propagada para o modelo de dados, exibições e controlador.

Para este tutorial, você adicionará uma nova coluna à tabela Student para registrar o nome do meio do aluno. Para adicionar essa coluna, abra o projeto de banco de dados e abra o arquivo Student. Sql. Por meio do designer ou do código T-SQL, adicione uma coluna chamada **MiddleName** que seja um nvarchar (50) e permita valores nulos.

Implante essa alteração em seu banco de dados local iniciando seu projeto de banco de dados (ou F5). O novo campo é adicionado à tabela. Se você não o vir no Pesquisador de Objetos do SQL Server, clique no botão Atualizar no painel.

![Mostrar nova coluna](changing-the-database/_static/image2.png)

A nova coluna existe na tabela de banco de dados, mas ela não existe atualmente na classe de modelo de dado. Você deve atualizar o modelo para incluir a nova coluna. Na pasta **modelos** , abra o arquivo **ContosoModel. edmx** para exibir o diagrama de modelo. Observe que o modelo de aluno não contém a propriedade MiddleName. Clique com o botão direito do mouse em qualquer lugar na superfície de design e selecione **atualizar modelo do banco de dados**.

No assistente de atualização, selecione a guia **Atualizar** e, em seguida, selecione **tabelas** > **dbo** > **aluno**. Clique em **Concluir**.

Depois que o processo de atualização for concluído, o diagrama de banco de dados incluirá a nova propriedade **MiddleName** . Salve o arquivo **ContosoModel. edmx** . Você deve salvar esse arquivo para que a nova propriedade seja propagada para a classe **Student.cs** . Agora você atualizou o banco de dados e o modelo.

Compile a solução.

## <a name="add-the-property-to-the-views"></a>Adicionar a propriedade às exibições

Infelizmente, as exibições ainda não contêm a nova propriedade. Para atualizar as exibições, você tem duas opções: você pode regenerar as exibições novamente adicionando scaffolding para a classe Student, ou você pode adicionar manualmente a nova propriedade a suas exibições existentes. Neste tutorial, você adicionará o scaffolding novamente, pois você não fez nenhuma alteração personalizada nas exibições geradas automaticamente. Você pode considerar adicionar a propriedade manualmente quando tiver feito alterações nas exibições e não quiser perder essas alterações.

Para garantir que as exibições sejam recriadas, exclua a pasta **estudantes** em **exibições**e exclua o **StudentsController**. Em seguida, clique com o botão direito do mouse na pasta **controladores** e adicione scaffolding para o modelo de **aluno** . Novamente, nomeie o controlador **StudentsController**. Selecione **Adicionar**.

Compile a solução novamente. As exibições agora contêm a propriedade MiddleName.

![Mostrar nome do meio](changing-the-database/_static/image5.png)

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você:

> [!div class="checklist"]
> * Adicionou uma coluna
> * Adicionada a propriedade às exibições

Avance para o próximo tutorial para aprender a personalizar o modo de exibição para mostrar detalhes sobre um registro de aluno.
> [!div class="nextstepaction"]
> [Personalizar uma exibição](customizing-a-view.md)