---
uid: mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
title: Validação com os validadores de anotação de dados (VB) | Microsoft Docs
author: microsoft
description: Aproveite o associador de modelo de anotação de dados para executar a validação em um aplicativo MVC ASP.NET. Saiba como usar os diferentes tipos de validador...
ms.author: riande
ms.date: 05/29/2009
ms.assetid: 0d23ff2b-f2ec-434a-be3b-1180beeccba3
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
msc.type: authoredcontent
ms.openlocfilehash: 00150575baabc659f7dd0c07349cde52105f6c8b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78542080"
---
# <a name="validation-with-the-data-annotation-validators-vb"></a>Validação com os validadores de anotação de dados (VB)

pela [Microsoft](https://github.com/microsoft)

> Aproveite o associador de modelo de anotação de dados para executar a validação em um aplicativo MVC ASP.NET. Saiba como usar os diferentes tipos de atributos de validador e trabalhar com eles no Microsoft Entity Framework.

Neste tutorial, você aprenderá a usar os validadores de anotação de dados para executar a validação em um aplicativo MVC ASP.NET. A vantagem de usar os validadores de anotação de dados é que eles permitem que você execute a validação simplesmente adicionando um ou mais atributos – como o atributo Required ou StringLength – a uma propriedade de classe.

Antes de poder usar os validadores de anotação de dados, você deve baixar o associador de modelo de anotações de dados. Você pode baixar o exemplo de associador de modelo de anotações de dados no site do CodePlex clicando [aqui](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).

É importante entender que o associador de modelo de anotações de dados não é uma parte oficial do Microsoft ASP.NET MVC Framework. Embora o associador de modelo de anotações de dados tenha sido criado pela equipe do Microsoft ASP.NET MVC, a Microsoft não oferece suporte a produtos oficiais para o associador de modelo de anotações de dados descrito e usada neste tutorial.

## <a name="using-the-data-annotation-model-binder"></a>Usando o associador de modelo de anotação de dados

Para usar o associador de modelo de anotações de dados em um aplicativo MVC ASP.net, primeiro você precisa adicionar uma referência ao assembly Microsoft. Web. Mvc. Annotations. dll e ao assembly System. ComponentModel. Annotations. dll. Selecione o projeto de opção de menu **, Adicionar referência**. Em seguida, clique na guia **procurar** e navegue até o local onde você baixou (e descompactou) o exemplo de fichário de modelo de anotações de dados (consulte a **Figura 1**).

[![](validation-with-the-data-annotation-validators-vb/_static/image2.png)](validation-with-the-data-annotation-validators-vb/_static/image1.png)

**Figura 1**: adicionando uma referência ao associador de modelo de anotações de dados ([clique para exibir a imagem em tamanho normal](validation-with-the-data-annotation-validators-vb/_static/image3.png))

Selecione o assembly Microsoft. Web. Mvc. Annotations. dll e o assembly System. ComponentModel. Annotations. dll e clique no botão **OK** .

Você não pode usar o assembly System. ComponentModel. Annotations. dll incluído com o .NET Framework Service Pack 1 com o associador de modelo de data Annotations. Você deve usar a versão do assembly System. ComponentModel. Annotations. dll incluída com o download de exemplo de fichário de modelo de anotações de dados.

Por fim, você precisa registrar o associador de modelo Annotations no arquivo global. asax. Adicione a seguinte linha de código ao aplicativo\_o manipulador de eventos Start () para que o aplicativo\_método Start () tenha esta aparência:

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample1.vb)]

Essa linha de código registra o DataAnnotationsModelBinder como o associador de modelo padrão para todo o aplicativo MVC ASP.NET.

## <a name="using-the-data-annotation-validator-attributes"></a>Usando os atributos do validador de anotação de dados

Ao usar o associador de modelo de anotações de dados, você usa atributos de validador para executar a validação. O namespace System. ComponentModel. Annotations inclui os seguintes atributos de validador:

- Intervalo – permite que você valide se o valor de uma propriedade cai entre um intervalo de valores especificado.
- RegularExpression – permite que você valide se o valor de uma propriedade corresponde a um padrão de expressão regular especificado.
- Obrigatório – permite marcar uma propriedade conforme necessário.
- StringLength – permite que você especifique um comprimento máximo para uma propriedade de cadeia de caracteres.
- Validação – a classe base para todos os atributos do validador.

> [!NOTE] 
> 
> Se suas necessidades de validação não forem satisfeitas por nenhum dos validadores padrão, você sempre terá a opção de criar um atributo de validador personalizado herdando um novo atributo de validador do atributo de validação base.

A classe Product na **Listagem 1** ilustra como usar esses atributos de validador. As propriedades Name, Description e PreçoUnitário são marcadas como obrigatórias. A propriedade Name deve ter um comprimento de cadeia de caracteres com menos de 10 caracteres. Por fim, a propriedade PreçoUnitário deve corresponder a um padrão de expressão regular que representa um valor de moeda.

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample2.vb)]

**Listagem 1**: Models\Product.vb

A classe Product ilustra como usar um atributo adicional: o atributo DisplayName. O atributo DisplayName permite que você modifique o nome da propriedade quando a propriedade é exibida em uma mensagem de erro. Em vez de exibir a mensagem de erro "o campo PreçoUnitário é necessário", você pode exibir a mensagem de erro "o campo de preço é obrigatório".

> [!NOTE] 
> 
> Se desejar personalizar completamente a mensagem de erro exibida por um validador, você poderá atribuir uma mensagem de erro personalizada à propriedade ErrorMessage do validador como esta: `<Required(ErrorMessage:="This field needs a value!")>`

Você pode usar a classe Product na **Listagem 1** com a ação do controlador Create () na **Listagem 2**. Essa ação do controlador exibe novamente a exibição criar quando o estado do modelo contém erros.

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample3.vb)]

**Listagem 2**: Controllers\ProductController.vb

Por fim, você pode criar o modo de exibição na **Listagem 3** clicando com o botão direito do mouse na ação criar () e selecionando a opção de menu **Adicionar modo de exibição**. Crie uma exibição fortemente tipada com a classe Product como a classe Model. Selecione **criar** na lista suspensa Exibir conteúdo (veja a **Figura 2**).

[![](validation-with-the-data-annotation-validators-vb/_static/image5.png)](validation-with-the-data-annotation-validators-vb/_static/image4.png)

**Figura 2**: adicionando o modo de exibição de criação

[!code-aspx[Main](validation-with-the-data-annotation-validators-vb/samples/sample4.aspx)]

**Listagem 3**: Views\Product\Create.aspx

> [!NOTE] 
> 
> Remova o campo ID do formulário criar gerado pela opção de menu **Adicionar exibição** . Como o campo de ID corresponde a uma coluna de identidade, você não deseja permitir que os usuários insiram um valor para esse campo.

Se você enviar o formulário para criar um produto e não inserir valores para os campos obrigatórios, as mensagens de erro de validação na **Figura 3** serão exibidas.

[![](validation-with-the-data-annotation-validators-vb/_static/image7.png)](validation-with-the-data-annotation-validators-vb/_static/image6.png)

**Figura 3**: campos obrigatórios ausentes

Se você inserir um valor de moeda inválido, a mensagem de erro na **Figura 4** será exibida.

[![](validation-with-the-data-annotation-validators-vb/_static/image9.png)](validation-with-the-data-annotation-validators-vb/_static/image8.png)

**Figura 4**: valor de moeda inválido

## <a name="using-data-annotation-validators-with-the-entity-framework"></a>Usando validadores de anotação de dados com o Entity Framework

Se você estiver usando o Entity Framework da Microsoft para gerar suas classes de modelo de dados, não poderá aplicar os atributos do validador diretamente às suas classes. Como o Entity Framework Designer gera as classes de modelo, todas as alterações feitas nas classes de modelo serão substituídas na próxima vez que você fizer alterações no designer.

Se você quiser usar os validadores com as classes geradas pelo Entity Framework, precisará criar classes de metadados. Aplique os validadores à classe de metadados em vez de aplicar os validadores à classe real.

Por exemplo, imagine que você criou uma classe de filme usando o Entity Framework (consulte a **Figura 5**). Imagine, além disso, que você deseja fazer as propriedades título do filme e propriedades do diretor necessárias. Nesse caso, você pode criar a classe parcial e metadados de meta de dados na **listagem 4**.

[![](validation-with-the-data-annotation-validators-vb/_static/image11.png)](validation-with-the-data-annotation-validators-vb/_static/image10.png)

**Figura 5**: classe de filme gerada por Entity Framework

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample5.vb)]

**Listagem 4**: Models\Movie.vb

O arquivo na **listagem 4** contém duas classes chamadas Movie e MovieMetaData. A classe Movie é uma classe parcial. Ele corresponde à classe parcial gerada pelo Entity Framework que está contido no arquivo DataModel. designer. vb.

Atualmente, o .NET Framework não oferece suporte a Propriedades parciais. Portanto, não há como aplicar os atributos do validador às propriedades da classe Movie definida no arquivo DataModel. designer. vb aplicando os atributos do validador às propriedades da classe Movie definida no arquivo na **listagem 4**.

Observe que a classe Partial do filme é decorada com um atributo MetadataType que aponta para a classe MovieMetaData. A classe MovieMetaData contém propriedades de proxy para as propriedades da classe Movie.

Os atributos do validador são aplicados às propriedades da classe MovieMetaData. As propriedades Title, director e DateReleased são marcadas como propriedades obrigatórias. A propriedade director deve ser atribuída a uma cadeia de caracteres que contenha menos de 5 caracteres. Por fim, o atributo DisplayName é aplicado à propriedade DateReleased para exibir uma mensagem de erro como "o campo data de lançamento é necessário." em vez do erro "o campo DateReleased é obrigatório".

> [!NOTE] 
> 
> Observe que as propriedades de proxy na classe MovieMetaData não precisam representar os mesmos tipos das propriedades correspondentes na classe Movie. Por exemplo, a propriedade director é uma propriedade de cadeia de caracteres na classe Movie e uma propriedade Object na classe MovieMetaData.

A página na **Figura 6** ilustra as mensagens de erro retornadas quando você insere valores inválidos para as propriedades do filme.

[![](validation-with-the-data-annotation-validators-vb/_static/image13.png)](validation-with-the-data-annotation-validators-vb/_static/image12.png)

**Figura 6**: usando validadores com a Entity Framework ([clique para exibir a imagem em tamanho normal](validation-with-the-data-annotation-validators-vb/_static/image14.png))

## <a name="summary"></a>Resumo

Neste tutorial, você aprendeu a aproveitar o associador de modelo de anotação de dados para executar a validação em um aplicativo MVC ASP.NET. Você aprendeu a usar os diferentes tipos de atributos de validador, como os atributos Required e StringLength. Você também aprendeu a usar esses atributos ao trabalhar com o Microsoft Entity Framework.

> [!div class="step-by-step"]
> [Anterior](validating-with-a-service-layer-vb.md)
