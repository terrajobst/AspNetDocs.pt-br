---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: 'Tutorial: aprimorar a validação de dados para o EF Database First com o aplicativo MVC ASP.NET'
description: Este tutorial se concentra na adição de anotações de dados ao modelo de dados para especificar requisitos de validação e formatação de exibição.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: 897cd7c6a40445e2a4abede50d81e101372d3233
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616273"
---
# <a name="tutorial-enhance-data-validation-for-ef-database-first-with-aspnet-mvc-app"></a>Tutorial: aprimorar a validação de dados para o EF Database First com o aplicativo MVC ASP.NET

Usando MVC, Entity Framework e ASP.NET scaffolding, você pode criar um aplicativo Web que fornece uma interface para um banco de dados existente. Esta série de tutoriais mostra como gerar automaticamente o código que permite que os usuários exibam, editem, criem e excluam dados que residem em uma tabela. O código gerado corresponde às colunas na tabela de banco de dados.

Este tutorial se concentra na adição de anotações de dados ao modelo de dados para especificar requisitos de validação e formatação de exibição. Ele foi aprimorado com base nos comentários dos usuários na seção de comentários.

Neste tutorial, você:

> [!div class="checklist"]
> * Adicionar anotações de dados
> * Adicionar classes de metadados

## <a name="prerequisites"></a>Prerequisites

* [Personalizar uma exibição](customizing-a-view.md)

## <a name="add-data-annotations"></a>Adicionar anotações de dados

Como vimos em um tópico anterior, algumas regras de validação de dados são aplicadas automaticamente à entrada do usuário. Por exemplo, você só pode fornecer um número para a propriedade grau. Para especificar mais regras de validação de dados, você pode adicionar anotações de dados à sua classe de modelo. Essas anotações são aplicadas em todo o aplicativo Web para a propriedade especificada. Você também pode aplicar atributos de formatação que alteram como as propriedades são exibidas; como, alterar o valor usado para rótulos de texto.

Neste tutorial, você adicionará anotações de dados para restringir o comprimento dos valores fornecidos para as propriedades FirstName, LastName e MiddleName. No banco de dados, esses valores são limitados a 50 caracteres; no entanto, no aplicativo Web, esse limite de caracteres não é imposto no momento. Se um usuário fornecer mais de 50 caracteres para um desses valores, a página falhará ao tentar salvar o valor no banco de dados. Você também restringirá a taxa de valores entre 0 e 4.

Selecione **modelos** > **ContosoModel. edmx** > **ContosoModel.tt** e abra o arquivo *Student.cs* . Adicione o seguinte código realçado à classe.

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

Abra *Enrollment.cs* e adicione o seguinte código realçado.

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

Compile a solução.

Clique em **lista de alunos** e selecione **Editar**. Se você tentar inserir mais de 50 caracteres, uma mensagem de erro será exibida.

![Mostrar mensagem de erro](enhancing-data-validation/_static/image1.png)

Volte para a home page. Clique em **lista de registros** e selecione **Editar**. Tente fornecer uma classificação acima de 4. Você receberá esse erro: *a classificação do campo deve estar entre 0 e 4.*

## <a name="add-metadata-classes"></a>Adicionar classes de metadados

Adicionar os atributos de validação diretamente à classe de modelo funciona quando você não espera que o banco de dados seja alterado; no entanto, se o banco de dados for alterado e você precisar regenerar a classe de modelo, perderá todos os atributos que você aplicou à classe de modelo. Essa abordagem pode ser muito ineficiente e propenso a perder regras de validação importantes.

Para evitar esse problema, você pode adicionar uma classe de metadados que contém os atributos. Quando você associa a classe de modelo à classe de metadados, esses atributos são aplicados ao modelo. Nessa abordagem, a classe de modelo pode ser regenerada sem perder todos os atributos que foram aplicados à classe de metadados.

Na pasta **modelos** , adicione uma classe chamada *Metadata.cs*.

Substitua o código em *Metadata.cs* pelo código a seguir.

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

Essas classes de metadados contêm todos os atributos de validação que você aplicou anteriormente às classes de modelo. O atributo de **exibição** é usado para alterar o valor usado para rótulos de texto.

Agora, você deve associar as classes de modelo às classes de metadados.

Na pasta **modelos** , adicione uma classe chamada *PartialClasses.cs*.

Substitua o conteúdo do arquivo pelo código a seguir.

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

Observe que cada classe é marcada como uma classe de `partial`, e cada uma corresponde ao nome e ao namespace como a classe gerada automaticamente. Aplicando o atributo de metadados à classe parcial, você garante que os atributos de validação de dados serão aplicados à classe gerada automaticamente. Esses atributos não serão perdidos quando você regenerar as classes de modelo porque o atributo de metadados é aplicado em classes parciais que não são geradas novamente.

Para regenerar as classes geradas automaticamente, abra o arquivo *ContosoModel. edmx* . Mais uma vez, clique com o botão direito do mouse na superfície de design e selecione **atualizar modelo do banco de dados**. Mesmo que você não tenha alterado o banco de dados, esse processo irá regenerar as classes. Na guia **Atualizar** , selecione **tabelas** e **concluir**.

Salve o arquivo *ContosoModel. edmx* para aplicar as alterações.

Abra o arquivo *Student.cs* ou o arquivo *Enrollment.cs* e observe que os atributos de validação de dados aplicados anteriormente não estão mais no arquivo. No entanto, execute o aplicativo e observe que as regras de validação ainda são aplicadas quando você insere dados.

## <a name="conclusion"></a>Conclusão

Esta série forneceu um exemplo simples de como gerar código a partir de um banco de dados existente que permite aos usuários editar, atualizar, criar e excluí-los. Ele usou o ASP.NET MVC 5, o Entity Framework e o ASP.NET scaffolding para criar o projeto. 

Para obter um exemplo introdutório de desenvolvimento de Code First, consulte [introdução com o ASP.NET MVC 5](../introduction/getting-started.md). 

Para obter um exemplo mais avançado, consulte [criando um modelo de dados de Entity Framework para um aplicativo ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Observe que a API DbContext usada para trabalhar com dados no Database First é igual à API usada para trabalhar com dados no Code First. Mesmo que você pretenda usar Database First, você pode aprender a lidar com cenários mais complexos, como leitura e atualização de dados relacionados, tratamento de conflitos de simultaneidade e assim por diante de um tutorial de Code First. A única diferença está em como o banco de dados, a classe de contexto e as classes de entidade são criadas.

## <a name="additional-resources"></a>Recursos adicionais

Para obter uma lista completa das anotações de validação de dados que você pode aplicar a propriedades e classes, consulte [System. ComponentModel. Annotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você:

> [!div class="checklist"]
> * Anotações de dados adicionadas
> * Classes de metadados adicionadas

Para saber como implantar um aplicativo Web e um banco de dados SQL no serviço Azure App, consulte este tutorial:
> [!div class="nextstepaction"]
> [Implantar um aplicativo .NET no serviço Azure App](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase/)
