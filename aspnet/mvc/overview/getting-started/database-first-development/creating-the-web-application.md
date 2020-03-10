---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'Tutorial: criar o aplicativo Web e modelos de dados para o EF Database First com o ASP.NET MVC'
description: Este tutorial se concentra na criação do aplicativo Web e na geração de modelos de dados com base em suas tabelas de banco.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: 30fd42be5677df6fa6ee0630914098c30d21385b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616266"
---
# <a name="tutorial-create-the-web-application-and-data-models-for-ef-database-first-with-aspnet-mvc"></a>Tutorial: criar o aplicativo Web e modelos de dados para o EF Database First com o ASP.NET MVC

 Usando MVC, Entity Framework e ASP.NET scaffolding, você pode criar um aplicativo Web que fornece uma interface para um banco de dados existente. Esta série de tutoriais mostra como gerar automaticamente o código que permite que os usuários exibam, editem, criem e excluam dados que residem em uma tabela. O código gerado corresponde às colunas na tabela de banco de dados.

Este tutorial se concentra na criação do aplicativo Web e na geração de modelos de dados com base em suas tabelas de banco.

Neste tutorial, você:

> [!div class="checklist"]
> * Criar um aplicativo Web ASP .NET
> * Gerar os modelos

## <a name="prerequisites"></a>Prerequisites

* [Introdução ao Entity Framework 6 Database First usando o MVC 5](setting-up-database.md)

## <a name="create-an-aspnet-web-app"></a>Criar um aplicativo Web ASP .NET

Em uma nova solução ou na mesma solução que o projeto de banco de dados, crie um novo projeto no Visual Studio e selecione o modelo de **aplicativo Web ASP.net** . Nomeie o projeto **ContosoSite**.

![criar projeto](creating-the-web-application/_static/image1.png)

Clique em **OK**.

Na janela novo projeto ASP.NET, selecione o modelo **MVC** . Você pode limpar o **host na** opção de nuvem por enquanto, pois você implantará o aplicativo na nuvem mais tarde. Clique em **OK** para criar o aplicativo.

O projeto é criado com os arquivos e pastas padrão.

Neste tutorial, você usará o Entity Framework 6. Você pode verificar novamente a versão do Entity Framework em seu projeto por meio da janela gerenciar pacotes NuGet. Se necessário, atualize sua versão do Entity Framework.

![Mostrar versão](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a>Gerar os modelos

Agora, você criará Entity Framework modelos a partir das tabelas de banco de dados. Esses modelos são classes que você usará para trabalhar com os dados. Cada modelo espelha uma tabela no banco de dados e contém propriedades que correspondem às colunas na tabela.

Clique com o botão direito do mouse na pasta **modelos** e selecione **Adicionar** e **novo item**.

Na janela Adicionar novo item, selecione **dados** no painel esquerdo e **ADO.NET modelo de dados de entidade** das opções no painel central. Nomeie o novo arquivo de modelo **ContosoModel**.

Clique em **Adicionar**.

No assistente de Modelo de Dados de Entidade, selecione **EF designer do banco de dados**.

Clique em **Próximo**.

Se você tiver conexões de banco de dados definidas em seu ambiente de desenvolvimento, poderá ver uma dessas conexões previamente selecionada. No entanto, você deseja criar uma nova conexão com o banco de dados criado na primeira parte deste tutorial. Clique no botão **nova conexão** .

No janela Propriedades de conexão, forneça o nome do servidor local onde o banco de dados foi criado (neste caso, **\ProjectsV13**). Depois de fornecer o nome do servidor, selecione o ContosoUniversityData dos bancos de dados disponíveis.

![definir propriedades de conexão](creating-the-web-application/_static/image8.png)

Clique em **OK**.

As propriedades de conexão corretas agora são exibidas. Você pode usar o nome padrão para conexão no arquivo Web. config.

Clique em **Próximo**.

Selecione a versão mais recente do Entity Framework.

Clique em **Próximo**.

Selecione **tabelas** para gerar modelos para todas as três tabelas.

Clique em **Concluir**.

Se você receber um aviso de segurança, selecione **OK** para continuar a execução do modelo.

Os modelos são gerados a partir das tabelas de banco de dados e é exibido um diagrama que mostra as propriedades e relações entre as tabelas.

![diagrama de modelo](creating-the-web-application/_static/image11.png)

A pasta modelos agora inclui muitos arquivos novos relacionados aos modelos que foram gerados a partir do banco de dados.

O arquivo **ContosoModel.Context.cs** contém uma classe que deriva da classe **DbContext** e fornece uma propriedade para cada classe de modelo que corresponde a uma tabela de banco de dados. Os arquivos **Course.cs**, **Enrollment.cs**e **Student.cs** contêm as classes de modelo que representam as tabelas de bancos de dados. Você usará a classe de contexto e as classes de modelo ao trabalhar com scaffolding.

Antes de prosseguir com este tutorial, compile o projeto. Na próxima seção, você gerará código com base nos modelos de dados, mas essa seção não funcionará se o projeto não tiver sido criado.

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você:

> [!div class="checklist"]
> * Criou um aplicativo Web ASP.NET
> * Os modelos foram gerados

Avance para o próximo tutorial para aprender a criar o código de geração com base nos modelos de dados.
> [!div class="nextstepaction"]
> [Gerando exibições](generating-views.md)
