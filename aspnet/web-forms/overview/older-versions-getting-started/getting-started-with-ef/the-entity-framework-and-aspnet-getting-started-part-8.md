---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
title: Introdução com Entity Framework 4,0 Database First e ASP.NET 4 Web Forms-parte 8 | Microsoft Docs
author: tdykstra
description: O aplicativo Web de exemplo Contoso University demonstra como criar ASP.NET Web Forms aplicativos usando o Entity Framework. O aplicativo de exemplo é...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: aaadd9bb-5508-448c-ad57-5497dff90e13
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
msc.type: authoredcontent
ms.openlocfilehash: ff6a808dd501df8056c0fb0784a8e42e094d5db7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78585907"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-8"></a>Introdução com Entity Framework 4,0 Database First e ASP.NET 4 Web Forms-parte 8

por [Tom Dykstra](https://github.com/tdykstra)

> O aplicativo Web de exemplo Contoso University demonstra como criar ASP.NET Web Forms aplicativos usando o Entity Framework 4,0 e o Visual Studio 2010. Para obter informações sobre a série de tutoriais, consulte [o primeiro tutorial da série](the-entity-framework-and-aspnet-getting-started-part-1.md)

## <a name="using-dynamic-data-functionality-to-format-and-validate-data"></a>Usando Dados Dinâmicos funcionalidade para formatar e validar dados

No tutorial anterior, você implementou procedimentos armazenados. Este tutorial mostrará como Dados Dinâmicos funcionalidade pode fornecer os seguintes benefícios:

- Os campos são formatados automaticamente para exibição com base em seu tipo de dados.
- Os campos são validados automaticamente com base no seu tipo de dados.
- Você pode adicionar metadados ao modelo de dados para personalizar a formatação e o comportamento de validação. Ao fazer isso, você pode adicionar as regras de validação e formatação em apenas um local e elas são aplicadas automaticamente em todos os lugares em que você acessar os campos usando controles Dados Dinâmicos.

Para ver como isso funciona, você alterará os controles usados para exibir e editar campos na página *students. aspx* existente e adicionará metadados de formatação e validação aos campos nome e data do tipo de entidade `Student`.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)

## <a name="using-dynamicfield-and-dynamiccontrol-controls"></a>Usando controles DynamicField e DynamicControl

Abra a página *students. aspx* e, no controle de `StudentsGridView`, substitua o **nome** e a **data de registro** `TemplateField` elementos com a seguinte marcação:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample1.aspx)]

Essa marcação usa controles de `DynamicControl` no lugar de controles `TextBox` e `Label` no campo modelo de nome de aluno e usa um controle de `DynamicField` para a data de registro. Nenhuma cadeia de caracteres de formato especificada.

Adicione um controle de `ValidationSummary` após o controle de `StudentsGridView`.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample2.aspx)]

No controle de `SearchGridView`, substitua a marcação pelas colunas de **Data** de **nome** e de registro como você fez no controle de `StudentsGridView`, exceto omitir o elemento `EditItemTemplate`. O elemento `Columns` do controle de `SearchGridView` agora contém a seguinte marcação:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample3.aspx)]

Abra *students.aspx.cs* e adicione a seguinte instrução de `using`:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample4.cs)]

Adicione um manipulador para o evento de `Init` da página:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample5.cs)]

Esse código especifica que Dados Dinâmicos fornecerá formatação e validação nesses controles associados a dados para campos da entidade `Student`. Se você receber uma mensagem de erro como o exemplo a seguir ao executar a página, isso normalmente significa que você esqueceu de chamar o método `EnableDynamicData` no `Page_Init`:

`Could not determine a MetaTable. A MetaTable could not be determined for the data source 'StudentsEntityDataSource' and one could not be inferred from the request URL.`

Execute a página.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)

Na coluna **data de registro** , a hora é exibida junto com a data, pois o tipo de propriedade é `DateTime`. Você corrigirá isso mais tarde.

Por enquanto, observe que Dados Dinâmicos fornece automaticamente a validação de dados básica. Por exemplo, clique em **Editar**, desmarque o campo de data, clique em **Atualizar**, e você verá que dados dinâmicos automaticamente torna este campo obrigatório, pois o valor não permite valor nulo no modelo de dados. A página exibe um asterisco após o campo e uma mensagem de erro no controle de `ValidationSummary`:

[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)

Você pode omitir o controle `ValidationSummary`, porque você também pode manter o ponteiro do mouse sobre o asterisco para ver a mensagem de erro:

[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)

Dados Dinâmicos também validará que os dados inseridos no campo de **data de registro** são uma data válida:

[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)

Como você pode ver, essa é uma mensagem de erro genérica. Na próxima seção, você verá como personalizar mensagens, bem como regras de validação e formatação.

## <a name="adding-metadata-to-the-data-model"></a>Adicionando metadados ao modelo de dados

Normalmente, você deseja personalizar a funcionalidade fornecida pelo Dados Dinâmicos. Por exemplo, você pode alterar a forma como os dados são exibidos e o conteúdo das mensagens de erro. Normalmente, você também personaliza regras de validação de dados para fornecer mais funcionalidade do que o que Dados Dinâmicos fornece automaticamente com base em tipos de dados. Para fazer isso, você cria classes parciais que correspondem aos tipos de entidade.

Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto **ContosoUniversity** , selecione **Adicionar referência**e adicione uma referência a `System.ComponentModel.DataAnnotations`.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)

Na pasta *Dal* , crie um novo arquivo de classe, nomeie-o *Student.cs*e substitua o código de modelo nele pelo código a seguir.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample6.cs)]

Esse código cria uma classe parcial para a entidade `Student`. O atributo `MetadataType` aplicado a essa classe parcial identifica a classe que você está usando para especificar metadados. A classe Metadata pode ter qualquer nome, mas usar o nome da entidade mais "Metadata" é uma prática comum.

Os atributos aplicados às propriedades na classe de metadados especificam formatação, validação, regras e mensagens de erro. Os atributos mostrados aqui terão os seguintes resultados:

- `EnrollmentDate` será exibido como uma data (sem hora).
- Os dois campos de nome devem ter 25 caracteres ou menos de comprimento e uma mensagem de erro personalizada é fornecida.
- Ambos os campos de nome são necessários e uma mensagem de erro personalizada é fornecida.

Execute a página *students. aspx* novamente e você verá que as datas agora são exibidas sem horas:

[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)

Edite uma linha e tente limpar os valores nos campos de nome. Os asteriscos que indicam erros de campo aparecem assim que você deixa um campo, antes de clicar em **Atualizar**. Quando você clica em **Atualizar**, a página exibe o texto da mensagem de erro que você especificou.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)

Tente inserir nomes com mais de 25 caracteres, clique em **Atualizar**e a página exibirá o texto da mensagem de erro especificado.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)

Agora que você configurou essas regras de formatação e validação nos metadados do modelo de dados, as regras serão automaticamente aplicadas em cada página que exibe ou permite alterações nesses campos, desde que você use controles `DynamicControl` ou `DynamicField`. Isso reduz a quantidade de código redundante que você precisa escrever, o que facilita a programação e o teste, além de garantir que a formatação e a validação dos dados sejam consistentes em todo o aplicativo.

## <a name="more-information"></a>Mais informações

Isso conclui esta série de tutoriais sobre Introdução com o Entity Framework. Para obter mais recursos para ajudá-lo a aprender a usar o Entity Framework, continue com [o primeiro tutorial na próxima série de tutoriais Entity Framework](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md) ou visite os seguintes sites:

- [Perguntas frequentes Entity Framework](http://www.ef-faq.org/introduction.html)
- [Blog da equipe do Entity Framework](https://blogs.msdn.com/b/adonet/)
- [Entity Framework na biblioteca MSDN](https://msdn.microsoft.com/library/bb399572.aspx)
- [Entity Framework no MSDN Data Developer Center](https://msdn.microsoft.com/data/ef.aspx)
- [Visão geral do controle de servidor Web EntityDataSource na biblioteca MSDN](https://msdn.microsoft.com/library/cc488502.aspx)
- [Referência de API de controle de EntityDataSource na biblioteca MSDN](https://msdn.microsoft.com/library/system.web.ui.webcontrols.entitydatasource.aspx)
- [Fóruns de Entity Framework no MSDN](https://social.msdn.microsoft.com/forums/adodotnetentityframework/)
- [Blog de Julie Lerman](http://thedatafarm.com/blog/)

> [!div class="step-by-step"]
> [Anterior](the-entity-framework-and-aspnet-getting-started-part-7.md)
