---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: Atualizando, excluindo e criando dados com associação de modelo e formulários da Web | Microsoft Docs
author: Rick-Anderson
description: Esta série de tutoriais demonstra os aspectos básicos do uso de associação de modelo com um projeto ASP.NET Web Forms. A associação de modelo torna a interação de dados mais direta-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: 11dc52ec79ca91119b37ea60e08164eb1b1d0e2b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78586642"
---
# <a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a>Atualizando, excluindo e criando dados com associação de modelo e formulários da Web

por [Tom FitzMacken](https://github.com/tfitzmac)

> Esta série de tutoriais demonstra os aspectos básicos do uso de associação de modelo com um projeto ASP.NET Web Forms. A associação de modelo torna a interação de dados mais direta do que lidar com objetos de fonte de dados (como ObjectDataSource ou SqlDataSource). Esta série começa com material introdutório e passa para conceitos mais avançados em Tutoriais posteriores.
> 
> Este tutorial mostra como criar, atualizar e excluir dados com associação de modelo. Você definirá as seguintes propriedades:
> 
> - DeleteMethod
> - InsertMethod
> - UpdateMethod
> 
> Essas propriedades recebem o nome do método que manipula a operação correspondente. Dentro desse método, você fornece a lógica para interagir com os dados.
> 
> Este tutorial se baseia no projeto criado na primeira [parte](retrieving-data.md) da série.
> 
> Você pode [baixar](https://go.microsoft.com/fwlink/?LinkId=286116) o projeto completo no C# ou no VB. O código para download funciona com o Visual Studio 2012 ou Visual Studio 2013. Ele usa o modelo do Visual Studio 2012, que é um pouco diferente do modelo de Visual Studio 2013 mostrado neste tutorial.

## <a name="what-youll-build"></a>O que você criará

Neste tutorial, você aprenderá a:

1. Adicionar modelos de dados dinâmicos
2. Habilitar atualização e exclusão de dados por meio de métodos de associação de modelo
3. Aplicar regras de validação de dados – habilitar a criação de um novo registro no banco

## <a name="add-dynamic-data-templates"></a>Adicionar modelos de dados dinâmicos

Para fornecer a melhor experiência de usuário e minimizar a repetição de código, você usará modelos de dados dinâmicos. Você pode integrar facilmente modelos de dados dinâmicos pré-criados em seu site existente instalando um pacote NuGet.

Em **gerenciar pacotes NuGet**, instale o **DynamicDataTemplatesCS**.

![modelos de dados dinâmicos](updating-deleting-and-creating-data/_static/image1.png)

Observe que o projeto agora inclui uma pasta chamada **DynamicData**. Nessa pasta, você encontrará os modelos que são aplicados automaticamente aos controles dinâmicos em seus Web Forms.

![pasta de dados dinâmicos](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a>Habilitar atualização e exclusão

Permitir que os usuários atualizem e excluam registros no banco de dados é muito semelhante ao processo de recuperação de dado. Nas propriedades **UpdateMethod** e **DeleteMethod** , você especifica os nomes dos métodos que executam essas operações. Com um controle GridView, você também pode especificar a geração automática de botões editar e excluir. O código realçado a seguir mostra as adições ao código GridView.

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

No arquivo code-behind, adicione uma instrução using para **System. Data. Entity. Infrastructure**.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

Em seguida, adicione os seguintes métodos Update e Delete.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

O método **TryUpdateModel** aplica os valores de associação de dados correspondentes do formulário da Web ao item de dados. O item de dados é recuperado com base no valor do parâmetro ID.

## <a name="enforce-validation-requirements"></a>Impor requisitos de validação

Os atributos de validação aplicados às propriedades FirstName, LastName e year na classe Student são aplicados automaticamente ao atualizar os dados. Os controles DynamicField adicionam validadores de cliente e de servidor com base nos atributos de validação. As propriedades FirstName e LastName são obrigatórias. FirstName não pode exceder 20 caracteres de comprimento e LastName não pode exceder 40 caracteres. Year deve ser um valor válido para a enumeração AcademicYear.

Se o usuário violar um dos requisitos de validação, a atualização não continuará. Para ver a mensagem de erro, adicione um controle ValidationSummary acima do GridView. Para exibir os erros de validação da Associação de modelo, defina a propriedade **ShowModelStateErrors** definida como **true**. 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

Execute o aplicativo Web e atualize e exclua qualquer um dos registros.

![atualizar dados](updating-deleting-and-creating-data/_static/image3.png)

Observe que, quando estiver no modo de edição, o valor da propriedade year será automaticamente renderizado como uma lista suspensa. A propriedade year é um valor de enumeração e o modelo de dados dinâmicos para um valor de enumeração Especifica uma lista suspensa para edição. Você pode encontrar esse modelo abrindo a **enumeração\_arquivo Edit. ascx** na pasta **DynamicData**/**FieldTemplates** .

Se você fornecer valores válidos, a atualização será concluída com êxito. Se você violar um dos requisitos de validação, a atualização não continuará e uma mensagem de erro será exibida acima da grade.

![mensagem de erro](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a>Adicionar novos registros

O controle GridView não inclui a propriedade **InsertMethod** e, portanto, não pode ser usado para adicionar um novo registro com associação de modelo. Você pode encontrar a propriedade InsertMethod nos controles **FormView**, **DetailsView**ou **ListView** . Neste tutorial, você usará um controle FormView para adicionar um novo registro.

Primeiro, adicione um link para a nova página que será criada para adicionar um novo registro. Acima do ValidationSummary, adicione:

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

O novo link aparecerá na parte superior do conteúdo para a página dos alunos.

![novo link](updating-deleting-and-creating-data/_static/image5.png)

Em seguida, adicione um novo formulário da Web usando uma página mestra e nomeie-o como **mystudent**. Selecione site. master como a página mestra.

Você renderizará os campos para adicionar um novo aluno usando um controle **DynamicEntity** . O controle DynamicEntity renderiza as propriedades editáveis na classe especificada na Propriedade ItemType. A propriedade StudentId foi marcada com o atributo **[ScaffoldColumn (false)]** para que não seja renderizada. No espaço reservado MainContent da página addaluno, adicione o código a seguir.

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

No arquivo code-behind (AddStudent.aspx.cs), adicione uma instrução **using** para o namespace **ContosoUniversityModelBinding. Models** .

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

Em seguida, adicione os métodos a seguir para especificar como inserir um novo registro e um manipulador de eventos para o botão Cancelar.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

Salve todas as alterações.

Execute o aplicativo Web e crie um novo aluno.

![Adicionar novo aluno](updating-deleting-and-creating-data/_static/image6.png)

Clique em **Inserir** e observe que o novo aluno foi criado.

![exibir novo aluno](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a>Conclusão

Neste tutorial, você habilitou a atualização, a exclusão e a criação de dados. Você assegurau que as regras de validação são aplicadas ao interagir com os dados.

No próximo [tutorial](sorting-paging-and-filtering-data.md) desta série, você permitirá classificar, paginar e filtrar dados.

> [!div class="step-by-step"]
> [Anterior](retrieving-data.md)
> [Próximo](sorting-paging-and-filtering-data.md)
