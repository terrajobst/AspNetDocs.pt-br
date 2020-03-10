---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: Criando um banco de dados | Microsoft Docs
author: shanselman
description: Este é um tutorial principiante que apresenta os fundamentos do ASP.NET MVC. Crie um aplicativo Web simples que lê e grava de um banco de dados.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: 995db714ce6365415d06dc458aee84a31c7c8fd6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78581434"
---
# <a name="creating-a-database"></a>Criar um banco de dados

por [Scott Hanselman](https://github.com/shanselman)

> Este é um tutorial principiante que apresenta os fundamentos do ASP.NET MVC. Você criará um aplicativo Web simples que lê e grava de um banco de dados. Visite o [ASP.NET MVC Learning Center](../../../index.md) para encontrar outros tutoriais e exemplos do ASP.NET MVC.

Nesta seção, vamos criar um novo banco de dados do SQL Express que usaremos para armazenar e recuperar os nossos dados de filme. No IDE do Visual Web Developer, selecione Exibir | Gerenciador de Servidores. Clique com o botão direito em conexões de dados e clique em Adicionar conexão...

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

Na caixa de diálogo escolher fonte de dados, selecione Microsoft SQL Server e selecione continuar.

![](getting-started-with-mvc-part4/_static/image2.png)

Na caixa de diálogo Adicionar conexão, digite ".\SQLEXPRESS" para o nome do servidor e insira "filmes" como o nome do novo banco de dados.

[caixa de diálogo ![Adicionar conexão](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)

Clique em OK e você será perguntado se deseja criar esse banco de dados. Selecione Sim.

[![criar filmes?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)

Agora você tem um banco de dados vazio no Gerenciador de Servidores.

![Adicionar nova tabela](getting-started-with-mvc-part4/_static/image7.png)

Clique com o botão direito do mouse em tabelas e clique em Adicionar tabela. O Designer de Tabela será exibido. Adicione colunas para ID, título, liberado, gênero e preço. Clique com o botão direito do mouse na coluna ID e clique em definir chave primária. Veja a aparência de minhas áreas de design.

[Editor de tabela de banco de dados ![](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)

Além disso, selecione a coluna ID e, em Propriedades da coluna abaixo, altere "especificação de identidade" para "Sim".

[![ndo IsIdentity-Propriedades da coluna](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)

Quando tiver feito isso, clique no ícone salvar na barra de ferramentas ou selecione Arquivo | Salve no menu e nomeie a tabela "**filme**" (singular). Temos um banco de dados e uma tabela!

[![escolher nome](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)

Volte para Gerenciador de Servidores e clique com o botão direito do mouse na tabela filme e selecione "mostrar dados da tabela". Insira alguns filmes para que o nosso banco de dados tenha algum dado.

[Edição de tabela de banco de dados ![](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)

## <a name="creating-a-model"></a>Criar um Modelo

Agora, volte para a Gerenciador de Soluções no lado direito do IDE e clique com o botão direito do mouse na pasta modelos e selecione Adicionar | Novo item.

[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)

Vamos criar um modelo de entidade do nosso novo banco de dados. Isso adicionará um conjunto de classes ao nosso projeto que facilitará a consulta e a manipulação dos dados em nosso banco de dado. Selecione o nó de dados no lado esquerdo da caixa de diálogo e, em seguida, selecione o modelo de item de Modelo de Dados de Entidade ADO.NET. Nomeie-o como Movies. edmx.

[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)

Clique no botão "Adicionar". Isso iniciará o "Assistente de Modelo de Dados de Entidade".

Na caixa de diálogo Nova que aparece, selecione gerar do banco de dados. Como acabamos de criar um banco de dados, só precisaremos dizer ao Entity Framework sobre nosso novo banco de dados e sua tabela. Clique em avançar para salvar nossa conexão de banco de dados na configuração de nosso aplicativo Web. Agora, marque a caixa de seleção tabelas e filmes e clique em concluir.

[Assistente de Modelo de Dados de Entidade de ![](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)

Agora, podemos ver nossa nova tabela de filmes na Entity Framework Designer e acessá-la a partir do código.

[Filmes de ![-Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)

Na superfície de design, você pode ver uma classe de "filme". Essa classe é mapeada para a tabela "filme" em nosso banco de dados, e cada propriedade dentro dela é mapeada para uma coluna com a tabela. Cada instância de uma classe "filme" corresponderá a uma linha na tabela "filme".

Se você não gostar das convenções de nomenclatura e mapeamento padrão usadas pelo Entity Framework, poderá usar o designer de Entity Framework para alterá-las ou personalizá-las. Para este aplicativo, usaremos os padrões e apenas salvaremos o arquivo como está.

Agora, vamos trabalhar com alguns dados reais!

> [!div class="step-by-step"]
> [Anterior](getting-started-with-mvc-part3.md)
> [Próximo](getting-started-with-mvc-part5.md)
