---
uid: mvc/overview/getting-started/database-first-development/setting-up-database
title: 'Tutorial: introdução ao EF Database First usando o MVC 5'
description: Este tutorial mostra como começar com um banco de dados existente e criar rapidamente um aplicativo Web que permite aos usuários interagir com os dados.
author: Rick-Anderson
ms.author: riande
ms.date: 01/15/2019
ms.topic: tutorial
ms.assetid: 095abad4-3bfe-4f06-b092-ae6a735b7e49
msc.legacyurl: /mvc/overview/getting-started/database-first-development/setting-up-database
msc.type: authoredcontent
ms.openlocfilehash: a760767839a834a9c7e9fe358a3fd806a833261f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583520"
---
# <a name="tutorial-get-started-with-ef-database-first-using-mvc-5"></a>Tutorial: introdução ao EF Database First usando o MVC 5

Usando MVC, Entity Framework e ASP.NET scaffolding, você pode criar um aplicativo Web que fornece uma interface para um banco de dados existente. Esta série de tutoriais mostra como gerar automaticamente o código que permite que os usuários exibam, editem, criem e excluam dados que residem em uma tabela. O código gerado corresponde às colunas na tabela de banco de dados. Na última parte da série, você aprende como adicionar anotações de dados ao modelo de dados para especificar requisitos de validação e formatação de exibição. Quando terminar, você poderá avançar para um artigo do Azure para saber como implantar um aplicativo .NET e um banco de dados SQL no serviço Azure App.

Este tutorial mostra como começar com um banco de dados existente e criar rapidamente um aplicativo Web que permite aos usuários interagir com os dados. Ele usa o Entity Framework 6 e MVC 5 para criar o aplicativo Web. O recurso ASP.NET scaffolding permite que você gere automaticamente o código para exibir, atualizar, criar e excluir dados. Usando as ferramentas de publicação no Visual Studio, você pode implantar facilmente o site e o banco de dados no Azure.

Esta parte da série concentra-se na criação de um banco de dados e no preenchimento deles com eles.

Esta série foi escrita com contribuições de Tom Dykstra e Rick Anderson. Ele foi aprimorado com base nos comentários dos usuários na seção de comentários.

Neste tutorial, você:

> [!div class="checklist"]
> * Configurar o banco de dados

## <a name="prerequisites"></a>Prerequisites

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)

## <a name="set-up-the-database"></a>Configurar o banco de dados

Para imitar o ambiente de ter um banco de dados existente, primeiro você criará um banco de dado com alguns de pré-teste e, em seguida, criará seu aplicativo Web que se conecta ao banco de dados.

Este tutorial foi desenvolvido usando o LocalDB com o Visual Studio 2017. Você pode usar um servidor de banco de dados existente em vez do LocalDB, mas dependendo da sua versão do Visual Studio e do seu tipo de banco de dados, talvez não haja suporte para todas as ferramentas de data no Visual Studio. Se as ferramentas não estiverem disponíveis para seu banco de dados, talvez seja necessário executar algumas das etapas específicas do banco de dados no Management Suite para seu banco de dados.

Se você tiver um problema com as ferramentas de banco de dados em sua versão do Visual Studio, certifique-se de ter instalado a versão mais recente das ferramentas de banco de dados. Para obter informações sobre como atualizar ou instalar as ferramentas de banco de dados, consulte [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027).

Inicie o Visual Studio e crie um **projeto de banco de dados SQL Server**. Nomeie o projeto **ContosoUniversityData**.

![Criar projeto de banco de dados](setting-up-database/_static/image1.png)

Agora você tem um projeto de banco de dados vazio. Para garantir que você possa implantar esse banco de dados no Azure, você definirá o banco de dados SQL do Azure como a plataforma de destino para o projeto. Definir a plataforma de destino não implanta realmente o banco de dados; Isso significa apenas que o projeto de banco de dados verificará se o design do banco de dados é compatível com a plataforma de destino. Para definir a plataforma de destino, abra as **Propriedades** do projeto e selecione **banco de dados SQL do Microsoft Azure** para a plataforma de destino.

Você pode criar as tabelas necessárias para este tutorial adicionando scripts SQL que definem as tabelas. Clique com o botão direito do mouse em seu projeto e adicione um novo item. Selecione **tabelas e exibições** > **tabela** e nomeie-o como *aluno*.

No arquivo de tabela, substitua o comando T-SQL pelo código a seguir para criar a tabela.

[!code-sql[Main](setting-up-database/samples/sample1.sql)]

Observe que a janela de design sincroniza automaticamente com o código. Você pode trabalhar com o código ou o designer.

![Mostrar código e design](setting-up-database/_static/image5.png)

Adicione outra tabela. Neste momento, nomeie-o como curso e use o comando T-SQL a seguir.

[!code-sql[Main](setting-up-database/samples/sample2.sql)]

E repita mais uma vez para criar uma tabela chamada registro.

[!code-sql[Main](setting-up-database/samples/sample3.sql)]

Você pode preencher o banco de dados com os data por meio de um script que é executado após a implantação. Adicione um script pós-implantação ao projeto. Clique com o botão direito do mouse em seu projeto e adicione um novo item. Selecione **scripts de usuário** > **script pós-implantação**. Você pode usar o nome padrão.

Adicione o código T-SQL a seguir ao script pós-implantação. Esse script simplesmente adiciona dados ao banco de dado quando nenhum registro correspondente é encontrado. Ele não substitui nem exclui nenhum dado que você tenha inserido no banco de dados.

[!code-sql[Main](setting-up-database/samples/sample4.sql)]

É importante observar que o script pós-implantação é executado toda vez que você implanta seu projeto de banco de dados. Portanto, você precisa considerar cuidadosamente seus requisitos ao escrever esse script. Em alguns casos, talvez você queira recomeçar a partir de um conjunto conhecido de dados sempre que o projeto for implantado. Em outros casos, talvez você não queira alterar os dados existentes de nenhuma forma. Com base em seus requisitos, você pode decidir se precisa de um script pós-implantação ou o que precisa incluir no script. Para obter mais informações sobre como preencher seu banco de dados com um script pós-implantação, consulte [incluindo dados em um projeto de banco de SQL Server](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).

Agora você tem quatro arquivos de script SQL, mas nenhuma tabela real. Você está pronto para implantar seu projeto de banco de dados no LocalDB. No Visual Studio, clique no botão Iniciar (ou F5) para compilar e implantar seu projeto de banco de dados. Verifique a guia **saída** para verificar se a compilação e a implantação foram bem-sucedidas.

Para ver que o novo banco de dados foi criado, abra **pesquisador de objetos do SQL Server** e procure o nome do projeto no servidor de banco de dados local correto (neste caso, o **LocalDB) \ProjectsV13**).

Para ver que as tabelas são preenchidas com dados, clique com o botão direito do mouse em uma tabela e selecione **exibir dados**.

![Mostrar dados da tabela](setting-up-database/_static/image9.png)

Uma exibição editável dos dados da tabela é exibida. Por exemplo, se você selecionar **tabelas** > **dbo. Course** > **exibir dados**, verá uma tabela com três colunas (**curso**, **título**e **créditos**) e quatro linhas.

## <a name="additional-resources"></a>Recursos adicionais

Para obter um exemplo introdutório de desenvolvimento de Code First, consulte [introdução com o ASP.NET MVC 5](../introduction/getting-started.md). Para obter um exemplo mais avançado, consulte [criando um modelo de dados de Entity Framework para um aplicativo ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

Para obter orientação sobre como selecionar qual Entity Framework abordagem usar, consulte [Entity Framework abordagens de desenvolvimento](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você:

> [!div class="checklist"]
> * Configurar o banco de dados

Avance para o próximo tutorial para aprender a criar os modelos de dados e aplicativos Web.
> [!div class="nextstepaction"]
> [Criar o aplicativo Web e modelos de dados](creating-the-web-application.md)
