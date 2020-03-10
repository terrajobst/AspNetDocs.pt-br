---
uid: web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
title: Definindo programaticamente os valores de parâmetro de ObjectDataSource (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, veremos como adicionar um método à nossa DAL e à BLL que aceita um único parâmetro de entrada e retorna dados. O exemplo definirá esse parâmetro...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 0ecb03b6-52a0-4731-8c7a-436391d36838
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
msc.type: authoredcontent
ms.openlocfilehash: f1dd50f46528e8dd51f85e503604d3f0dbc21ad2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78577052"
---
# <a name="programmatically-setting-the-objectdatasources-parameter-values-vb"></a>Configurar programaticamente os valores do parâmetro ObjectDataSource (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_6_VB.exe) ou [baixar PDF](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/datatutorial06vb1.pdf)

> Neste tutorial, veremos como adicionar um método à nossa DAL e à BLL que aceita um único parâmetro de entrada e retorna dados. O exemplo definirá esse parâmetro programaticamente.

## <a name="introduction"></a>Introdução

Como vimos no [tutorial anterior](declarative-parameters-vb.md), várias opções estão disponíveis para passar valores de parâmetro de forma declarativa para os métodos de ObjectDataSource. Se o valor do parâmetro for embutido em código, vier de um controle da Web na página ou em qualquer outra fonte que possa ser lida por uma fonte de dados `Parameter` objeto, por exemplo, esse valor poderá ser associado ao parâmetro de entrada sem escrever uma linha de código.

No entanto, pode haver ocasiões em que o valor do parâmetro venha de alguma fonte que ainda não tenha sido contada por um dos objetos da fonte de dados interna `Parameter`. Se o nosso site tiver suporte para contas de usuário, talvez queiramos definir o parâmetro com base na ID de usuário do visitante conectado no momento. Ou talvez seja necessário personalizar o valor do parâmetro antes de enviá-lo ao método do objeto subjacente do ObjectDataSource.

Sempre que o método `Select` do ObjectDataSource é invocado, o ObjectDataSource primeiro levanta seu [evento de seleção](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting%28VS.80%29.aspx). O método do objeto subjacente do ObjectDataSource é invocado. Depois de concluir o [evento selecionado](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selected%28VS.80%29.aspx) do ObjectDataSource, acionado (a Figura 1 ilustra essa sequência de eventos). Os valores de parâmetro passados para o método do objeto subjacente do ObjectDataSource podem ser definidos ou personalizados em um manipulador de eventos para o evento `Selecting`.

[![os eventos do ObjectDataSource selecionado e de seleção são acionados antes e depois que o método do objeto subjacente é invocado](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image2.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image1.png)

**Figura 1**: os eventos de `Selected` e `Selecting` do ObjectDataSource são acionados antes e depois que o método do objeto subjacente é invocado ([clique para exibir a imagem em tamanho normal](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image3.png))

Neste tutorial, veremos como adicionar um método à nossa DAL e à BLL que aceita um único parâmetro de entrada `Month`, do tipo `Integer` e retorna um objeto `EmployeesDataTable` populado com os funcionários que têm seu aniversário de contratação no `Month`especificado. Nosso exemplo definirá esse parâmetro programaticamente com base no mês atual, mostrando uma lista de "aniversários de funcionários deste mês".

Vamos começar!

## <a name="step-1-adding-a-method-toemployeestableadapter"></a>Etapa 1: adicionando um método a`EmployeesTableAdapter`

Para nosso primeiro exemplo, precisamos adicionar um meio para recuperar os funcionários cujos `HireDate` ocorreram em um mês especificado. Para fornecer essa funcionalidade de acordo com nossa arquitetura, precisamos primeiro criar um método em `EmployeesTableAdapter` que seja mapeado para a instrução SQL apropriada. Para fazer isso, comece abrindo o DataSet tipado Northwind. Clique com o botão direito do mouse no rótulo `EmployeesTableAdapter` e escolha Adicionar consulta.

[![adicionar uma nova consulta ao EmployeesTableAdapter](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image5.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image4.png)

**Figura 2**: adicionar uma nova consulta à `EmployeesTableAdapter` ([clique para exibir a imagem em tamanho normal](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image6.png))

Escolha Adicionar uma instrução SQL que retorna linhas. Quando você atingir a tela especificar uma `SELECT` instrução, a instrução de `SELECT` padrão para o `EmployeesTableAdapter` já será carregada. Basta adicionar na cláusula `WHERE`: `WHERE DATEPART(m, HireDate) = @Month`. [Datepart](https://msdn.microsoft.com/library/ms174420.aspx) é uma função T-SQL que retorna uma parte de data específica de um tipo de `datetime`; Nesse caso, estamos usando `DATEPART` para retornar o mês da coluna `HireDate`.

[![retornar apenas as linhas em que a coluna HireDate é menor ou igual ao parâmetro @HiredBeforeDate](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image8.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image7.png)

**Figura 3**: retornar apenas as linhas em que a coluna `HireDate` é menor ou igual ao parâmetro `@HiredBeforeDate` ([clique para exibir a imagem em tamanho normal](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image9.png))

Por fim, altere o `FillBy` e `GetDataBy` nomes de método para `FillByHiredDateMonth` e `GetEmployeesByHiredDateMonth`, respectivamente.

[![escolher os nomes de método mais apropriados do que FillBy e GetDataBy](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image11.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image10.png)

**Figura 4**: escolha mais nomes de métodos apropriados do que `FillBy` e `GetDataBy` ([clique para exibir a imagem em tamanho normal](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image12.png))

Clique em concluir para concluir o assistente e retornar à superfície de design do conjunto de um. O `EmployeesTableAdapter` agora deve incluir um novo conjunto de métodos para acessar os funcionários contratados em um mês especificado.

[![os novos métodos aparecem no Design Surface do conjunto de um](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image14.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image13.png)

**Figura 5**: os novos métodos aparecem na design Surface do conjunto de[datas (clique para exibir a imagem em tamanho normal](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image15.png))

## <a name="step-2-adding-thegetemployeesbyhireddatemonthmonthmethod-to-the-business-logic-layer"></a>Etapa 2: adicionando o método de`GetEmployeesByHiredDateMonth(month)`à camada de lógica de negócios

Como nossa arquitetura de aplicativo usa uma camada separada para a lógica de negócios e a lógica de acesso a dados, precisamos adicionar um método à nossa BLL que chama para a DAL para recuperar os funcionários contratados antes de uma data especificada. Abra o arquivo `EmployeesBLL.vb` e adicione o seguinte método:

[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample1.vb)]

Assim como acontece com nossos outros métodos nessa classe, `GetEmployeesByHiredDateMonth(month)` simplesmente chama a DAL e retorna os resultados.

## <a name="step-3-displaying-employees-whose-hiring-anniversary-is-this-month"></a>Etapa 3: exibindo funcionários cujo aniversário de contratação é este mês

Nossa etapa final para este exemplo é exibir os funcionários cujo aniversário de contratação é este mês. Comece adicionando um GridView à página `ProgrammaticParams.aspx` na pasta `BasicReporting` e adicione uma nova ObjectDataSource como sua fonte de dados. Configure o ObjectDataSource para usar a classe `EmployeesBLL` com a `SelectMethod` definida como `GetEmployeesByHiredDateMonth(month)`.

[![usar a classe EmployeesBLL](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image17.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image16.png)

**Figura 6**: usar a classe `EmployeesBLL` ([clique para exibir a imagem em tamanho normal](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image18.png))

[![selecionar do método GetEmployeesByHiredDateMonth (mês)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image20.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image19.png)

**Figura 7**: selecione no método de `GetEmployeesByHiredDateMonth(month)` ([clique para exibir a imagem em tamanho normal](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image21.png))

A tela final solicita que possamos fornecer a origem do valor do parâmetro de `month`. Como vamos definir esse valor programaticamente, deixe a origem do parâmetro definida como a opção None padrão e clique em concluir.

[![deixar a origem do parâmetro definida como None](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image23.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image22.png)

**Figura 8**: Deixe a origem do parâmetro definida como nenhum ([clique para exibir a imagem em tamanho normal](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image24.png))

Isso criará um objeto `Parameter` na coleção de `SelectParameters` do ObjectDataSource que não tem um valor especificado.

[!code-aspx[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample2.aspx)]

Para definir esse valor programaticamente, precisamos criar um manipulador de eventos para o evento de `Selecting` do ObjectDataSource. Para fazer isso, vá para a modo de exibição de Design e clique duas vezes em ObjectDataSource. Como alternativa, selecione o ObjectDataSource, vá para o janela Propriedades e clique no ícone de raio. Em seguida, clique duas vezes na caixa de texto ao lado do evento `Selecting` ou digite o nome do manipulador de eventos que você deseja usar. Como uma terceira opção, você pode criar o manipulador de eventos selecionando o evento ObjectDataSource e seu `Selecting` nas duas listas suspensas na parte superior da classe code-behind da página.

![Clique no ícone de raio na janela Propriedades para listar os eventos de um controle da Web](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image25.png)

**Figura 9**: clique no ícone de raio na janela Propriedades para listar os eventos de um controle da Web

Todas as três abordagens adicionam um novo manipulador de eventos para o evento de `Selecting` do ObjectDataSource à classe code-behind da página. Nesse manipulador de eventos, podemos ler e gravar nos valores de parâmetro usando `e.InputParameters(parameterName)`, em que *`parameterName`* é o valor do atributo `Name` na marca de `<asp:Parameter>` (a coleção de `InputParameters` também pode ser indexada no ordinal, como no `e.InputParameters(index)`). Para definir o parâmetro `month` para o mês atual, adicione o seguinte ao manipulador de eventos `Selecting`:

[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample3.vb)]

Ao visitar esta página por meio de um navegador, podemos ver que apenas um funcionário foi contratado neste mês (março) Laura Callahan, que foi a empresa desde 1994.

[![os funcionários cujos aniversários são mostrados neste mês](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image27.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image26.png)

**Figura 10**: os funcionários cujos aniversários deste mês são mostrados ([clique para exibir a imagem em tamanho normal](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image28.png))

## <a name="summary"></a>Resumo

Embora os valores dos parâmetros de ObjectDataSource possam ser normalmente definidos declarativamente, sem a necessidade de uma linha de código, é fácil definir os valores de parâmetro programaticamente. Tudo o que precisamos fazer é criar um manipulador de eventos para o evento `Selecting` do ObjectDataSource, que é acionado antes de o método do objeto subjacente ser invocado e definir manualmente os valores de um ou mais parâmetros por meio da coleção `InputParameters`.

Este tutorial conclui a seção de relatório básico. O [próximo tutorial](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) inicia a seção filtragem e cenários mestre-detalhes, na qual veremos as técnicas para permitir que o visitante filtre os dados e faça uma busca detalhada de um relatório mestre em um relatório de detalhes.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. O revisor de Lead para este tutorial foi Hilton Giesenow. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](declarative-parameters-vb.md)
