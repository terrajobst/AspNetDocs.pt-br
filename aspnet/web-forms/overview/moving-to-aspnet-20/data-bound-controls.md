---
uid: web-forms/overview/moving-to-aspnet-20/data-bound-controls
title: Controles associados a dados | Microsoft Docs
author: microsoft
description: A maioria dos aplicativos ASP.NET depende de algum grau de apresentação de dados de uma fonte de dados de back-end. Controles vinculados a dados têm sido uma parte dinâmica da interação com w...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 0e23ff32-646d-43f3-8bec-6b2313d3abd6
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-bound-controls
msc.type: authoredcontent
ms.openlocfilehash: 1154b38e0fa3d9d56cb407ae659c3b0d69871fda
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78603834"
---
# <a name="data-bound-controls"></a>Controles de dados associados

pela [Microsoft](https://github.com/microsoft)

> A maioria dos aplicativos ASP.NET depende de algum grau de apresentação de dados de uma fonte de dados de back-end. Os controles vinculados a dados têm sido uma parte dinâmica da interação com os dados em aplicativos Web dinâmicos. O ASP.NET 2,0 apresenta algumas melhorias substanciais em controles vinculados a dados, incluindo uma nova classe BaseDataBoundControl e uma sintaxe declarativa.

A maioria dos aplicativos ASP.NET depende de algum grau de apresentação de dados de uma fonte de dados de back-end. Os controles vinculados a dados têm sido uma parte dinâmica da interação com os dados em aplicativos Web dinâmicos. O ASP.NET 2,0 apresenta algumas melhorias substanciais em controles vinculados a dados, incluindo uma nova classe BaseDataBoundControl e uma sintaxe declarativa.

O BaseDataBoundControl atua como a classe base para a classe DataBoundControl e a classe HierarchicalDataBoundControl. Neste módulo, discutiremos as seguintes classes que derivam de DataBoundControl:

- AdRotator
- Controles de lista
- GridView
- FormView
- DetailsView

Também Discutiremos as seguintes classes que derivam da classe HierarchicalDataBoundControl:

- TreeView
- Menu
- SiteMapPath

## <a name="databoundcontrol-class"></a>Classe DataBoundControl

A classe DataBoundControl é uma classe abstrata (que é marcada como MustInherit no VB) usada para interagir com dados tabulares ou de estilo de lista. Os controles a seguir são alguns dos controles que derivam de DataBoundControl.

## <a name="adrotator"></a>AdRotator

O controle AdRotator permite que você exiba uma faixa gráfica em uma página da Web vinculada a uma URL específica. O gráfico que é exibido é girado usando as propriedades do controle. A frequência de um determinado anúncio exibido em uma página pode ser configurada usando a propriedade **impressões** e os anúncios podem ser filtrados usando a filtragem de palavra-chave.

Os controles de AdRotator usam um arquivo XML ou uma tabela em um banco de dados para o Data. Os atributos a seguir são usados em arquivos XML para configurar o controle AdRotator.

### <a name="imageurl"></a>ImageUrl
A URL de uma imagem a ser exibida para o anúncio.

### <a name="navigateurl"></a>NavigateUrl
A URL para a qual o usuário deve ser levado quando o anúncio é clicado. Deve ser codificado por URL.

### <a name="alternatetext"></a>AlternateText
O texto alternativo que é exibido em uma dica de ferramenta e lido por leitores de tela. Também exibe quando a imagem especificada por ImageUrl não está disponível.

### <a name="keyword"></a>Palavra-chave
Define uma palavra-chave que pode ser usada ao usar a filtragem de palavra-chave. Se especificado, somente os anúncios com uma palavra-chave correspondente ao filtro de palavra-chave serão exibidos.

### <a name="impressions"></a>Impressões
Um número ponderado que determina com que frequência um determinado anúncio provavelmente aparecerá. Ele é relativo à impressão de outros anúncios no mesmo arquivo. O valor máximo das impressões coletiva para todos os anúncios em um arquivo XML é 2.048.000.000 1.

### <a name="height"></a>Altura
A altura do anúncio em pixels.

### <a name="width"></a>Largura
A largura do anúncio em pixels.

> [!NOTE]
> Os atributos Height e Width substituem a altura e a largura do próprio controle AdRotator.

Um arquivo XML típico pode ser semelhante ao seguinte:

[!code-xml[Main](data-bound-controls/samples/sample1.xml)]

No exemplo acima, o anúncio da Contoso é duas vezes mais provável de aparecer como o anúncio do site ASP.NET devido ao valor do atributo de impressões.

Para exibir anúncios do arquivo XML acima, adicione um controle AdRotator a uma página e defina a propriedade **AdvertisementFile** para apontar para o arquivo XML, conforme mostrado abaixo:

[!code-aspx[Main](data-bound-controls/samples/sample2.aspx)]

Se você optar por usar uma tabela de banco de dados como a origem do seu controle AdRotator, primeiro precisará configurar um banco de dado usando o esquema a seguir:

| **Nome da coluna** | **Data type** | **Descrição** |
| --- | --- | --- |
| ID | INT | Chave primária. Esta coluna pode ter qualquer nome. |
| ImageUrl | nvarchar (*comprimento*) | A URL relativa ou absoluta da imagem a ser exibida para o anúncio. |
| NavigateUrl | nvarchar (*comprimento*) | A URL de destino do anúncio. Se você não fornecer um valor, o anúncio não será um hiperlink. |
| AlternateText | nvarchar (*comprimento*) | O texto exibido se a imagem não puder ser encontrada. Em alguns navegadores, o texto é exibido como uma dica de ferramenta. O texto alternativo também é usado para acessibilidade para que os usuários que não podem ver o gráfico possam ouvir sua descrição leitura alta. |
| Palavra-chave | nvarchar (*comprimento*) | Uma categoria para o anúncio no qual a página pode filtrar. |
| Impressões | int(4) | Um número que indica a probabilidade de a frequência com que o anúncio é exibido. Quanto maior o número, mais frequentemente o anúncio será exibido. O total de todos os valores de impressões no arquivo XML não pode exceder 2.048.000.000-1. |
| Largura | int(4) | A largura da imagem em pixels. |
| Altura | int(4) | A altura da imagem em pixels. |

Nos casos em que você já tem um banco de dados com um esquema diferente, você pode usar as propriedades **AlternateTextField**, **ImageUrlField**e **NavigateUrlField** para mapear os atributos de AdRotator para o banco de dados existente. Para exibir os dados a partir do banco de dados no controle AdRotator, adicione um controle da fonte de dado à página, configure a cadeia de conexão para o controle da fonte de dados para apontar para seu banco e defina a propriedade **DataSourceID** do controle AdRotator como a ID do controle da fonte de dados. Nos casos em que você tem a necessidade de configurar o AdRotator ADS programaticamente, use o evento AdCreated. O evento AdCreated usa dois parâmetros; um objeto e a outra instância de AdCreatedEventArgs. O AdCreatedEventArgs é uma referência ao AD que está sendo criado.

O trecho de código a seguir define o ImageUrl, NavigateUrl e AlternateText para um AD programaticamente:

[!code-csharp[Main](data-bound-controls/samples/sample3.cs)]

## <a name="list-controls"></a>Controles de lista

Os controles de lista incluem ListBox, DropDownList, CheckBoxList, RadioButtonList e BulletedList. Cada um desses controles pode ser vinculado a dados a uma fonte de dados. Eles usam um campo na fonte de dados como texto de exibição e, opcionalmente, podem usar um segundo campo como o valor de um item. Os itens também podem ser adicionados estaticamente em tempo de design, e você pode misturar itens estáticos e itens dinâmicos adicionados de uma fonte de dados.

Para associar dados a um controle de lista, adicione um controle de fonte de dados à página. Especifique um comando SELECT para o controle da fonte de dados e, em seguida, defina a propriedade DataSourceID do controle de lista como a ID do controle da fonte de dados. Use as propriedades **DataTextField** e **DataValueField** para definir o texto de exibição e o valor do controle. Além disso, você pode usar a propriedade **DataTextFormatString** para controlar a aparência do texto de exibição da seguinte maneira:

| **Expression** | **Descrição** |
| --- | --- |
| Preço: {0:C} | Para dados numéricos/decimais. Exibe o literal "Price:" seguido de números no formato de moeda. O formato de moeda depende da configuração de cultura especificada no atributo Culture na diretiva **Page** ou no arquivo Web. config. |
| {0:D4} | Para dados inteiros. Não pode ser usado com números decimais. Os inteiros são exibidos em um campo de valor zero com quatro caracteres de largura. |
| {0:N2}% | Para dados numéricos. Exibe o número com a precisão de casa de 2 casas decimais seguida pelo literal "%". |
| {0:000.0} | Para dados numéricos/decimais. Os números são arredondados para uma casa decimal. Os números com menos de três dígitos zero são preenchidos com zeros. |
| {0:D} | Para dados de data/hora. Exibe o formato de data por extenso ("quinta-feira, 06 de agosto de 1996"). O formato de data depende da configuração da cultura da página ou do arquivo Web.config. |
| {0:d} | Para dados de data/hora. Exibe o formato de data abreviada ("12/31/99"). |
| {0:yy-MM-dd} | Para dados de data/hora. Exibe a data no formato de ano numérico-mês-dia (96-08-06) |

## <a name="gridview"></a>GridView

O controle GridView permite a exibição e a edição de dados tabulares usando uma abordagem declarativa e é o sucessor do controle DataGrid. Os recursos a seguir estão disponíveis no controle GridView.

- Associação a controles de fonte de dados, como SqlDataSource.
- Recursos internos de classificação.
- Recursos internos de atualização e exclusão.
- Recursos de paginação internos.
- Recursos de seleção de linha internos.
- Acesso programático ao modelo de objeto GridView para definir dinamicamente Propriedades, manipular eventos e assim por diante.
- Vários campos de chave.
- Vários campos de dados para as colunas de hiperlink.
- Aparência personalizável por meio de temas e estilos.

**Campos de coluna**

Cada coluna no controle GridView é representada por um objeto DataControlField. Por padrão, a propriedade AutoGenerateColumns é definida como **true**, que cria um objeto AutoGeneratedField para cada campo na fonte de dados. Cada campo é renderizado como uma coluna no controle GridView na ordem em que cada campo aparece na fonte de dados. Você também pode controlar manualmente quais campos de coluna aparecem no controle **GridView** definindo a propriedade **AutoGenerateColumns** como **false** e, em seguida, definindo sua própria coleção de campos de coluna. Tipos de campo de coluna diferentes determinam o comportamento das colunas no controle.

A tabela a seguir lista os diferentes tipos de campo de coluna que podem ser usados.

| **Tipo de campo de coluna** | **Descrição** |
| --- | --- |
| BoundField | Exibe o valor de um campo em uma fonte de dados. Esse é o tipo de coluna padrão do controle GridView. |
| ButtonField | Exibe um botão de comando para cada item no controle GridView. Isso permite que você crie uma coluna de controles de botão personalizados, como o botão Adicionar ou remover. |
| CheckBoxField | Exibe uma caixa de seleção para cada item no controle GridView. Esse tipo de campo de coluna é comumente usado para exibir campos com um valor booliano. |
| CommandField | Exibe botões de comando predefinidos para executar operações de seleção, edição ou exclusão. |
| HyperLinkField | Exibe o valor de um campo em uma fonte de dados como um hiperlink. Esse tipo de campo de coluna permite associar um segundo campo à URL do hiperlink. |
| ImageField | Exibe uma imagem para cada item no controle GridView. |
| Modelofield | Exibe o conteúdo definido pelo usuário para cada item no controle GridView de acordo com um modelo especificado. Esse tipo de campo de coluna permite que você crie um campo de coluna personalizado. |

Para definir uma coleção de campos de coluna declarativamente, primeiro adicione abertura e fechamento de **colunas de&lt;&gt;** marcações entre as marcas de abertura e fechamento do controle GridView. Em seguida, liste os campos de coluna que você deseja incluir entre as **colunas de&lt;** de abertura e fechamento&gt;marcações. As colunas especificadas são adicionadas à coleção de colunas na ordem listada. A coleção **Columns** armazena todos os campos de coluna no controle e permite que você gerencie programaticamente os campos de coluna no controle GridView.

Campos de coluna declarados explicitamente podem ser exibidos em combinação com campos de coluna gerados automaticamente. Quando ambos são usados, os campos de coluna declarados explicitamente são processados primeiro, seguidos pelos campos de coluna gerados automaticamente.

## <a name="binding-to-data"></a>Associação a dados

O controle GridView pode ser associado a um controle da fonte de dados (como **SqlDataSource**, **ObjectDataSource**e assim por diante), bem como qualquer fonte de dados que implemente a interface System. Collections. IEnumerable (como System. Data. DataView, System. Collections. ArrayList ou System. Collections. Hashtable). Use um dos seguintes métodos para associar o controle GridView ao tipo de fonte de dados apropriado:

- Para associar a um controle da fonte de dados, defina a propriedade DataSourceID do controle GridView como o valor de ID do controle da fonte de dados. O controle GridView é automaticamente associado ao controle da fonte de dados especificado e pode tirar proveito dos recursos do controle da fonte de dados para executar a funcionalidade de classificação, atualização, exclusão e paginação. Esse é o método preferencial para associar dados.
- Para associar a uma fonte de dados que implementa a interface System. Collections. IEnumerable, defina programaticamente a propriedade DataSource do controle GridView para a fonte de dados e, em seguida, chame o método DataBind. Ao usar esse método, o controle GridView não fornece funcionalidade interna de classificação, atualização, exclusão e paginação. Você mesmo precisa fornecer essa funcionalidade.

## <a name="data-operations"></a>Operações de dados

O controle GridView fornece muitos recursos internos que permitem ao usuário classificar, atualizar, excluir, selecionar e paginar itens no controle. Quando o controle GridView está associado a um controle da fonte de dados, o controle GridView pode tirar proveito dos recursos do controle da fonte de dados e fornecer a funcionalidade automática de classificação, atualização e exclusão.

> [!NOTE]
> O controle GridView pode fornecer suporte para classificação, atualização e exclusão com outros tipos de fontes de dados; no entanto, será necessário fornecer um manipulador de eventos apropriado com a implementação para essas operações.

A classificação permite que o usuário classifique os itens no controle GridView em relação a uma coluna específica clicando no cabeçalho da coluna. Para habilitar a classificação, defina a propriedade AllowSorting como **true**.

As funcionalidades de atualização automática, exclusão e seleção são habilitadas quando um botão em um campo de coluna **ButtonField** ou **TemplateField** , com um nome de comando "Edit", "Delete" e "Select", respectivamente, é clicado. O controle GridView pode adicionar automaticamente um campo de coluna **CommandField** com um botão Editar, excluir ou selecionar se a propriedade AutoGenerateEditButton, AutoGenerateDeleteButton ou AutoGenerateSelectButton for definida como **true**, respectivamente.

> [!NOTE]
> Não há suporte direto para a inserção de registros na fonte de dados pelo controle GridView. No entanto, é possível inserir registros usando o controle GridView em conjunto com o controle DetailsView ou FormView.

Em vez de exibir todos os registros na fonte de dados ao mesmo tempo, o controle GridView pode quebrar automaticamente os registros em páginas. Para habilitar a paginação, defina a propriedade AllowPaging como **true**.

## <a name="customizing-the-user-interface"></a>Personalizando a interface do usuário

Você pode personalizar a aparência do controle GridView definindo as propriedades de estilo para as diferentes partes do controle. A tabela a seguir lista as propriedades de estilo diferentes.

| **Propriedade de estilo** | **Descrição** |
| --- | --- |
| AlternatingRowStyle | As configurações de estilo para as linhas de dados alternadas no controle GridView. Quando essa propriedade é definida, as linhas de dados são exibidas alternando entre as configurações de RowStyle e as configurações de **AlternatingRowStyle** . |
| EditRowStyle | As configurações de estilo para a linha que está sendo editada no controle GridView. |
| EmptyDataRowStyle | As configurações de estilo para a linha de dados vazia exibida no controle GridView quando a fonte de dados não contém nenhum registro. |
| Rodapé | As configurações de estilo da linha de rodapé do controle GridView. |
| HeaderStyle | As configurações de estilo da linha de cabeçalho do controle GridView. |
| Pager | As configurações de estilo da linha do pager do controle GridView. |
| RowStyle | As configurações de estilo para as linhas de dados no controle GridView. Quando a propriedade **AlternatingRowStyle** também é definida, as linhas de dados são exibidas alternando entre as configurações de **RowStyle** e as configurações de **AlternatingRowStyle** . |
| SelectedRowStyle | As configurações de estilo da linha selecionada no controle GridView. |

Você também pode mostrar ou ocultar partes diferentes do controle. A tabela a seguir lista as propriedades que controlam quais partes são mostradas ou ocultas.

| **Propriedade** | **Descrição** |
| --- | --- |
| Conpé | Mostra ou oculta a seção de rodapé do controle GridView. |
| ShowHeader | Mostra ou oculta a seção de cabeçalho do controle GridView. |

### <a name="events"></a>Eventos

O controle GridView fornece vários eventos com os quais você pode programar. Isso permite que você execute uma rotina personalizada sempre que um evento ocorrer. A tabela a seguir lista os eventos com suporte no controle GridView.

| **Evento** | **Descrição** |
| --- | --- |
| PageIndexChanged | Ocorre quando um dos botões de paginação é clicado, mas depois que o controle GridView manipula a operação de paginação. Esse evento é normalmente usado quando você precisa executar uma tarefa depois que o usuário navega para uma página diferente no controle. |
| PageIndexChanging | Ocorre quando um dos botões de pager é clicado, mas antes que o controle GridView manipule a operação de paginação. Esse evento é geralmente usado para cancelar a operação de paginação. |
| RowCancelingEdit | Ocorre quando um botão de cancelamento de uma linha é clicado, mas antes de o controle GridView sair do modo de edição. Esse evento é geralmente usado para interromper a operação de cancelamento. |
| RowCommand | Ocorre quando um botão é clicado no controle GridView. Esse evento é geralmente usado para executar uma tarefa quando um botão é clicado no controle. |
| RowCreated | Ocorre quando uma nova linha é criada no controle GridView. Esse evento é geralmente usado para modificar o conteúdo de uma linha quando a linha é criada. |
| RowDataBound | Ocorre quando uma linha de dados é associada a dados no controle GridView. Esse evento é frequentemente usado para modificar o conteúdo de uma linha quando a linha está associada a dados. |
| RowDeleted | Ocorre quando um botão de exclusão de uma linha é clicado, mas depois que o controle GridView exclui o registro da fonte de dados. Esse evento é geralmente usado para verificar os resultados da operação de exclusão. |
| RowDeleting | Ocorre quando um botão de exclusão de uma linha é clicado, mas antes de o controle GridView excluir o registro da fonte de dados. Esse evento é geralmente usado para cancelar a operação de exclusão. |
| RowEditing | Ocorre quando um botão de edição de uma linha é clicado, mas antes de o controle GridView entrar no modo de edição. Esse evento é geralmente usado para cancelar a operação de edição. |
| Atualizado | Ocorre quando um botão de atualização de uma linha é clicado, mas depois que o controle GridView atualiza a linha. Esse evento é geralmente usado para verificar os resultados da operação de atualização. |
| RowUpdating | Ocorre quando um botão de atualização de uma linha é clicado, mas antes de o controle GridView atualizar a linha. Esse evento é geralmente usado para cancelar a operação de atualização. |
| SelectedIndexChanged | Ocorre quando um botão de seleção de uma linha é clicado, mas depois que o controle GridView manipula a operação de seleção. Esse evento é geralmente usado para executar uma tarefa depois que uma linha é selecionada no controle. |
| SelectedIndexChanging | Ocorre quando um botão de seleção de uma linha é clicado, mas antes de o controle GridView manipular a operação SELECT. Esse evento é geralmente usado para cancelar a operação de seleção. |
| Classificado | Ocorre quando o hiperlink para classificar uma coluna é clicado, mas depois que o controle GridView manipula a operação de classificação. Esse evento é normalmente usado para executar uma tarefa depois que o usuário clica em um hiperlink para classificar uma coluna. |
| Classificação | Ocorre quando o hiperlink para classificar uma coluna é clicado, mas antes que o controle GridView manipule a operação de classificação. Esse evento é geralmente usado para cancelar a operação de classificação ou para executar uma rotina de classificação personalizada. |

## <a name="formview"></a>FormView

O controle FormView é usado para exibir um único registro de uma fonte de dados. Ele é semelhante ao controle DetailsView, exceto pelo fato de que ele exibe modelos definidos pelo usuário em vez de campos de linha. Criar seus próprios modelos proporciona maior flexibilidade no controle de como os dados são exibidos. O controle FormView dá suporte aos seguintes recursos:

- Associação a controles de fonte de dados, como SqlDataSource e ObjectDataSource.
- Recursos internos de inserção.
- Recursos internos de atualização e exclusão.
- Recursos de paginação internos.
- Acesso programático ao modelo de objeto FormView para definir propriedades dinamicamente, manipular eventos e assim por diante.
- Aparência personalizável por meio de modelos, temas e estilos definidos pelo usuário.

## <a name="templates"></a>Modelos

Para que o controle FormView exiba conteúdo, você precisa criar modelos para as diferentes partes do controle. A maioria dos modelos são opcionais; no entanto, você deve criar um modelo para o modo no qual o controle está configurado. Por exemplo, um controle FormView que dá suporte à inserção de registros deve ter um modelo de inserção de item definido. A tabela a seguir lista os diferentes modelos que você pode criar.

| **Tipo de modelo** | **Descrição** |
| --- | --- |
| EditItemTemplate | Define o conteúdo para a linha de dados quando o controle FormView está no modo de edição. Esse modelo geralmente contém controles de entrada e botões de comando com os quais o usuário pode editar um registro existente. |
| EmptyDataTemplate | Define o conteúdo para a linha de dados vazia exibida quando o controle FormView está associado a uma fonte de dados que não contém nenhum registro. Esse modelo geralmente contém conteúdo para alertar o usuário que a fonte de dados não contém nenhum registro. |
| FooterTemplate | Define o conteúdo da linha de rodapé. Esse modelo geralmente contém qualquer conteúdo adicional que você deseja exibir na linha de rodapé. Como alternativa, você pode simplesmente especificar o texto a ser exibido na linha de rodapé, definindo a propriedade FooterText. |
| Cabeçalhotemplate | Define o conteúdo da linha de cabeçalho. Esse modelo geralmente contém qualquer conteúdo adicional que você deseja exibir na linha de cabeçalho. Como alternativa, você pode simplesmente especificar o texto a ser exibido na linha de cabeçalho definindo a propriedade HeaderText. |
| ItemTemplate | Define o conteúdo para a linha de dados quando o controle FormView está no modo somente leitura. Esse modelo geralmente contém conteúdo para exibir os valores de um registro existente. |
| InsertItemTemplate | Define o conteúdo para a linha de dados quando o controle FormView está no modo de inserção. Esse modelo geralmente contém controles de entrada e botões de comando com os quais o usuário pode adicionar um novo registro. |
| PagerTemplate | Define o conteúdo para a linha do pager exibida quando o recurso de paginação é habilitado (quando a propriedade AllowPaging é definida como **true**). Esse modelo geralmente contém controles com os quais o usuário pode navegar para outro registro. |

Os controles de entrada no modelo de edição de item e no modelo de inserção de item podem ser associados aos campos de uma fonte de dados usando uma expressão de associação bidirecional. Isso permite que o controle FormView Extraia automaticamente os valores do controle de entrada para uma operação de atualização ou inserção. As expressões de ligação bidirecionais também permitem que os controles de entrada em um modelo de edição de item exibam automaticamente os valores de campo originais.

### <a name="binding-to-data"></a>Associação a dados

O controle FormView pode ser associado a um controle da fonte de dados (como **SqlDataSource**, AccessDataSource, **ObjectDataSource** e assim por diante) ou a qualquer fonte de dados que implemente a interface System. Collections. IEnumerable (como System. Data. DataView, System. Collections. ArrayList e System. Collections. Hashtable). Use um dos seguintes métodos para associar o controle FormView ao tipo de fonte de dados apropriado:

- Para associar a um controle da fonte de dados, defina a propriedade DataSourceID do controle FormView como o valor de ID do controle da fonte de dados. O controle FormView é automaticamente associado ao controle da fonte de dados especificado e pode tirar proveito dos recursos do controle da fonte de dados para executar a inserção, atualização, exclusão e a funcionalidade de paginação. Esse é o método preferencial para associar dados.
- Para associar a uma fonte de dados que implementa a interface **System. Collections. IEnumerable** , defina programaticamente a propriedade DataSource do controle FormView como a fonte de dados e, em seguida, chame o método DataBind. Ao usar esse método, o controle FormView não fornece funcionalidade interna de inserção, atualização, exclusão e paginação. Você precisa fornecer essa funcionalidade usando o evento apropriado.

## <a name="data-operations"></a>Operações de dados

O controle FormView fornece muitos recursos internos que permitem ao usuário atualizar, excluir, inserir e paginar itens no controle. Quando o controle FormView está associado a um controle da fonte de dados, o controle FormView pode aproveitar os recursos do controle da fonte de dados e fornecer a funcionalidade de atualização automática, exclusão, inserção e paginação. O controle FormView pode fornecer suporte para operações de atualização, exclusão, inserção e paginação com outros tipos de fontes de dados; no entanto, você deve fornecer um manipulador de eventos apropriado com a implementação para essas operações.

Como o controle FormView usa modelos, ele não fornece uma maneira de gerar automaticamente botões de comando para executar as operações de atualização, exclusão ou inserção. Você deve incluir manualmente esses botões de comando no modelo apropriado. O controle FormView reconhece determinados botões que têm suas propriedades **CommandName** definidas como valores específicos. A tabela a seguir lista os botões de comando que o controle FormView reconhece.

| **Button** | **Valor de CommandName** | **Descrição** |
| --- | --- | --- |
| Cancelar | Cancela | Usado na atualização ou inserção de operações para cancelar a operação e para descartar os valores inseridos pelo usuário. O controle FormView retorna para o modo especificado pela propriedade DefaultMode. |
| Excluir | Apagar | Usado na exclusão de operações para excluir o registro exibido da fonte de dados. Gera os eventos de exclusão e excluído. |
| Editar | Editar | Usado na atualização de operações para colocar o controle FormView no modo de edição. O conteúdo especificado na propriedade **EditItemTemplate** é exibido para a linha de dados. |
| Insert | Inserido | Usado em operações de inserção para tentar inserir um novo registro na fonte de dados usando os valores fornecidos pelo usuário. Gera os eventos ItemInserting e ItemInserted. |
| Novo | Outra | Usado em operações de inserção para colocar o controle FormView no modo de inserção. O conteúdo especificado na propriedade **InsertItemTemplate** é exibido para a linha de dados. |
| Página | Web | Usado em operações de paginação para representar um botão na linha do pager que executa a paginação. Para especificar a operação de paginação, defina a propriedade **CommandArgument** do botão como "Avançar", "anterior", "primeiro", "último" ou o índice da página na qual navegar. Gera os eventos PageIndexChanging e PageIndexChanged. |
| Atualizar | Cumulativo | Usado na atualização de operações para tentar atualizar o registro exibido na fonte de dados com os valores fornecidos pelo usuário. Gera os eventos de atualização e de atualização. |

Ao contrário do botão de exclusão (que exclui o registro exibido imediatamente), quando o botão Editar ou novo é clicado, o controle FormView entra no modo de edição ou inserção, respectivamente. No modo de edição, o conteúdo contido na propriedade **EditItemTemplate** é exibido para o item de dados atual. Normalmente, o modelo editar item é definido de modo que o botão Editar seja substituído por uma atualização e um botão Cancelar. Os controles de entrada apropriados para o tipo de dados do campo (como uma caixa de texto ou um controle de caixa de seleção) geralmente são exibidos com o valor de um campo para que o usuário modifique. Clicar no botão atualizar atualiza o registro na fonte de dados, enquanto clicar no botão Cancelar abandona as alterações.

Da mesma forma, o conteúdo contido na propriedade **InsertItemTemplate** é exibido para o item de dados quando o controle está no modo de inserção. O modelo de inserção de item geralmente é definido de modo que o novo botão seja substituído por um botão de inserção e cancelamento, e controles de entrada vazios são exibidos para que o usuário insira os valores para o novo registro. Clicar no botão Inserir insere o registro na fonte de dados, enquanto clicar no botão Cancelar abandona as alterações.

O controle FormView fornece um recurso de paginação, que permite ao usuário navegar para outros registros na fonte de dados. Quando habilitada, uma linha de pager é exibida no controle FormView que contém os controles de navegação de página. Para habilitar a paginação, defina a propriedade **AllowPaging** como **true**. Você pode personalizar a linha do pager definindo as propriedades dos objetos contidos no pager e na propriedade PagerSettings. Em vez de usar a interface do usuário de linha de pager interna, você pode criar sua própria interface do usuário usando a propriedade **PagerTemplate** .

## <a name="customizing-the-user-interface"></a>Personalizando a interface do usuário

Você pode personalizar a aparência do controle FormView definindo as propriedades de estilo para as diferentes partes do controle. A tabela a seguir lista as propriedades de estilo diferentes.

| **Propriedade de estilo** | **Descrição** |
| --- | --- |
| EditRowStyle | As configurações de estilo para a linha de dados quando o controle FormView está no modo de edição. |
| EmptyDataRowStyle | As configurações de estilo para a linha de dados vazia exibida no controle FormView quando a fonte de dados não contém nenhum registro. |
| Rodapé | As configurações de estilo da linha de rodapé do controle FormView. |
| HeaderStyle | As configurações de estilo da linha de cabeçalho do controle FormView. |
| InsertRowStyle | As configurações de estilo para a linha de dados quando o controle FormView está no modo de inserção. |
| Pager | As configurações de estilo para a linha do pager exibida no controle FormView quando o recurso de paginação está habilitado. |
| RowStyle | As configurações de estilo para a linha de dados quando o controle FormView está no modo somente leitura. |

## <a name="events"></a>Eventos

O controle FormView fornece vários eventos com os quais você pode programar. Isso permite que você execute uma rotina personalizada sempre que um evento ocorrer. A tabela a seguir lista os eventos com suporte no controle FormView.

| **Evento** | **Descrição** |
| --- | --- |
| ItemCommand | Ocorre quando um botão dentro de um controle FormView é clicado. Esse evento é geralmente usado para executar uma tarefa quando um botão é clicado no controle. |
| ItemCreated | Ocorre depois que todos os objetos FormViewRow são criados no controle FormView. Esse evento é geralmente usado para modificar os valores de um registro antes que ele seja exibido. |
| ItemDeleted | Ocorre quando um botão de exclusão (um botão com sua propriedade **CommandName** definido como "Delete") é clicado, mas depois que o controle FormView exclui o registro da fonte de dados. Esse evento é geralmente usado para verificar os resultados da operação de exclusão. |
| ItemDeleting | Ocorre quando um botão de exclusão é clicado, mas antes de o controle FormView excluir o registro da fonte de dados. Esse evento é geralmente usado para cancelar a operação de exclusão. |
| ItemInserted | Ocorre quando um botão de inserção (um botão com sua propriedade **CommandName** definido como "Insert") é clicado, mas depois que o controle FormView insere o registro. Esse evento é geralmente usado para verificar os resultados da operação de inserção. |
| ItemInserting | Ocorre quando um botão de inserção é clicado, mas antes de o controle FormView inserir o registro. Esse evento é geralmente usado para cancelar a operação de inserção. |
| Atualizado | Ocorre quando um botão de atualização (um botão com sua propriedade **CommandName** definido como "Update") é clicado, mas depois que o controle FormView atualiza a linha. Esse evento é geralmente usado para verificar os resultados da operação de atualização. |
| ItemUpdating | Ocorre quando um botão de atualização é clicado, mas antes de o controle FormView atualizar o registro. Esse evento é geralmente usado para cancelar a operação de atualização. |
| ModeChanged | Ocorre depois que o controle FormView muda de modos (para edição, inserção ou modo somente leitura). Esse evento é geralmente usado para executar uma tarefa quando o controle FormView muda de modos. |
| ModeChanging | Ocorre antes de o controle FormView alterar os modos (para edição, inserção ou modo somente leitura). Esse evento é geralmente usado para cancelar uma alteração no modo. |
| PageIndexChanged | Ocorre quando um dos botões de paginação é clicado, mas depois que o controle FormView manipula a operação de paginação. Esse evento é normalmente usado quando você precisa executar uma tarefa depois que o usuário navega para um registro diferente no controle. |
| PageIndexChanging | Ocorre quando um dos botões de pager é clicado, mas antes que o controle FormView manipule a operação de paginação. Esse evento é geralmente usado para cancelar a operação de paginação. |

## <a name="detailsview"></a>DetailsView

O controle DetailsView é usado para exibir um único registro de uma fonte de dados em uma tabela, em que cada campo do registro é exibido em uma linha da tabela. Ele pode ser usado em combinação com um controle GridView para cenários de detalhes mestre. O controle DetailsView oferece suporte aos seguintes recursos:

- Associação a controles de fonte de dados, como SqlDataSource.
- Recursos internos de inserção.
- Recursos internos de atualização e exclusão.
- Recursos de paginação internos.
- Acesso programático ao modelo de objeto DetailsView para definir dinamicamente Propriedades, manipular eventos e assim por diante.
- Aparência personalizável por meio de temas e estilos.

## <a name="row-fields"></a>Campos de linha

Cada linha de dados no controle DetailsView é criada pela declaração de um controle de campo. Tipos de campo de linha diferentes determinam o comportamento das linhas no controle. Os controles de campo derivam de DataControlField. A tabela a seguir lista os diferentes tipos de campo de linha que podem ser usados.

| **Tipo de campo de coluna** | **Descrição** |
| --- | --- |
| BoundField | Exibe o valor de um campo em uma fonte de dados como texto. |
| ButtonField | Exibe um botão de comando no controle DetailsView. Isso permite que você exiba uma linha com um controle de botão personalizado, como um botão Adicionar ou remover. |
| CheckBoxField | Exibe uma caixa de seleção no controle DetailsView. Esse tipo de campo de linha é comumente usado para exibir campos com um valor booliano. |
| CommandField | Exibe botões de comando internos para executar operações de edição, inserção ou exclusão no controle DetailsView. |
| HyperLinkField | Exibe o valor de um campo em uma fonte de dados como um hiperlink. Esse tipo de campo de linha permite que você vincule um segundo campo à URL do hiperlink. |
| ImageField | Exibe uma imagem no controle DetailsView. |
| Modelofield | Exibe o conteúdo definido pelo usuário para uma linha no controle DetailsView de acordo com um modelo especificado. Esse tipo de campo de linha permite que você crie um campo de linha personalizado. |

Por padrão, a propriedade AutoGenerateRows é definida como **true**, o que gera automaticamente um objeto de campo de linha associado para cada campo de um tipo vinculável na fonte de dados. Os tipos vinculáveis válidos são cadeia de caracteres, DateTime, Decimal, GUID e conjunto de tipos primitivos. Cada campo é exibido em uma linha como texto, na ordem em que cada campo é exibido na fonte de dados.

A geração automática de linhas fornece uma maneira rápida e fácil de exibir cada campo no registro. No entanto, para fazer uso dos recursos avançados do controle DetailsView, você deve declarar explicitamente os campos de linha a serem incluídos no controle DetailsView. Para declarar os campos de linha, primeiro defina a propriedade **AutoGenerateRows** como **false**. Em seguida, adicione os **campos de&lt;** de abertura e fechamento&gt;marcas entre as marcas de abertura e fechamento do controle DetailsView. Por fim, liste os campos de linha que você deseja incluir entre os **campos de&lt;** de abertura e fechamento&gt;marcas. Os campos de linha especificados são adicionados à coleção Fields na ordem listada. A coleção **Fields** permite que você gerencie programaticamente os campos de linha no controle DetailsView.

> [!NOTE]
> Campos de linha gerados automaticamente não são adicionados à coleção de campos.

## <a name="binding-to-data"></a>Associação a dados

O controle DetailsView pode ser associado a um controle da fonte de dados, como **SqlDataSource** ou AccessDataSource, ou a qualquer fonte de dados que implemente a interface System. Collections. IEnumerable, como System. Data. DataView, System. Collections. ArrayList e System. Collections. Hashtable.

Use um dos métodos a seguir para associar o controle DetailsView ao tipo de fonte de dados apropriado:

- Para associar a um controle da fonte de dados, defina a propriedade DataSourceID do controle DetailsView como o valor de ID do controle da fonte de dados. O controle DetailsView é associado automaticamente ao controle da fonte de dados especificado. Esse é o método preferencial para associar dados.
- Para associar a uma fonte de dados que implementa a interface **System. Collections. IEnumerable** , defina programaticamente a propriedade DataSource do controle DetailsView com a fonte de dados e, em seguida, chame o método DataBind.

## <a name="security"></a>Segurança

Esse controle pode ser usado para exibir a entrada do usuário, que pode incluir script de cliente mal-intencionado. Verifique todas as informações enviadas de um cliente para script executável, instruções SQL ou outro código antes de exibi-lo em seu aplicativo. O ASP.NET fornece um recurso de validação de solicitação de entrada para bloquear o script e o HTML na entrada do usuário.

## <a name="data-operations"></a>Operações de dados

O controle DetailsView fornece recursos internos que permitem ao usuário atualizar, excluir, inserir e paginar itens no controle. Quando o controle DetailsView está associado a um controle da fonte de dados, o controle DetailsView pode aproveitar os recursos do controle da fonte de dados e fornecer a funcionalidade de atualização automática, exclusão, inserção e paginação.

O controle DetailsView pode fornecer suporte para operações de atualização, exclusão, inserção e paginação com outros tipos de fontes de dados; no entanto, você deve fornecer a implementação para essas operações em um manipulador de eventos apropriado.

O controle DetailsView pode adicionar automaticamente um campo de linha **CommandField** com um botão Editar, excluir ou novo definindo as propriedades AutoGenerateEditButton, AutoGenerateDeleteButton ou AutoGenerateInsertButton como **true**, respectivamente. Ao contrário do botão de exclusão (que exclui o registro selecionado imediatamente), quando o botão Editar ou novo é clicado, o controle DetailsView entra no modo de edição ou inserção, respectivamente. No modo de edição, o botão Editar é substituído por uma atualização e um botão Cancelar. Os controles de entrada apropriados para o tipo de dados do campo (como uma caixa de texto ou um controle de caixa de seleção) são exibidos com o valor de um campo para que o usuário modifique. Clicar no botão atualizar atualiza o registro na fonte de dados, enquanto clicar no botão Cancelar abandona as alterações. Da mesma forma, no modo de inserção, o novo botão é substituído por um botão de inserção e cancelamento, e controles de entrada vazios são exibidos para que o usuário insira os valores para o novo registro.

O controle DetailsView fornece um recurso de paginação, que permite ao usuário navegar para outros registros na fonte de dados. Quando habilitado, os controles de navegação de página são exibidos em uma linha de pager. Para habilitar a paginação, defina a propriedade AllowPaging como **true**. A linha do pager pode ser personalizada usando as propriedades pager e PagerSettings.

## <a name="customizing-the-user-interface"></a>Personalizando a interface do usuário

Você pode personalizar a aparência do controle DetailsView definindo as propriedades de estilo para as diferentes partes do controle. A tabela a seguir lista as propriedades de estilo diferentes.

| **Propriedade de estilo** | **Descrição** |
| --- | --- |
| AlternatingRowStyle | As configurações de estilo para as linhas de dados alternadas no controle DetailsView. Quando essa propriedade é definida, as linhas de dados são exibidas alternando entre as configurações de RowStyle e as configurações de **AlternatingRowStyle** . |
| CommandRowStyle | As configurações de estilo para a linha que contém os botões de comando internos no controle DetailsView. |
| EditRowStyle | As configurações de estilo para as linhas de dados quando o controle DetailsView está no modo de edição. |
| EmptyDataRowStyle | As configurações de estilo para a linha de dados vazia exibida no controle DetailsView quando a fonte de dados não contém nenhum registro. |
| Rodapé | As configurações de estilo da linha de rodapé do controle DetailsView. |
| HeaderStyle | As configurações de estilo da linha de cabeçalho do controle DetailsView. |
| InsertRowStyle | As configurações de estilo para as linhas de dados quando o controle DetailsView está no modo de inserção. |
| Pager | As configurações de estilo da linha do pager do controle DetailsView. |
| RowStyle | As configurações de estilo para as linhas de dados no controle DetailsView. Quando a propriedade **AlternatingRowStyle** também é definida, as linhas de dados são exibidas alternando entre as configurações de **RowStyle** e as configurações de **AlternatingRowStyle** . |
| FieldHeaderStyle | As configurações de estilo da coluna de cabeçalho do controle DetailsView. |

## <a name="events"></a>Eventos

O controle DetailsView fornece vários eventos com os quais você pode programar. Isso permite que você execute uma rotina personalizada sempre que um evento ocorrer. A tabela a seguir lista os eventos com suporte no controle DetailsView. O controle DetailsView também herda esses eventos de suas classes base: DataBinding, vinculação de dados, descarte, init, Load, PreRender e render.

| **Evento** | **Descrição** |
| --- | --- |
| ItemCommand | Ocorre quando um botão é clicado no controle DetailsView. |
| ItemCreated | Ocorre depois que todos os objetos DetailsViewRow são criados no controle DetailsView. Esse evento é geralmente usado para modificar os valores de um registro antes que ele seja exibido. |
| ItemDeleted | Ocorre quando um botão de exclusão é clicado, mas depois que o controle DetailsView exclui o registro da fonte de dados. Esse evento é geralmente usado para verificar os resultados da operação de exclusão. |
| ItemDeleting | Ocorre quando um botão de exclusão é clicado, mas antes de o controle DetailsView excluir o registro da fonte de dados. Esse evento é geralmente usado para cancelar a operação de exclusão. |
| ItemInserted | Ocorre quando um botão de inserção é clicado, mas depois que o controle DetailsView insere o registro. Esse evento é geralmente usado para verificar os resultados da operação de inserção. |
| ItemInserting | Ocorre quando um botão de inserção é clicado, mas antes de o controle DetailsView inserir o registro. Esse evento é geralmente usado para cancelar a operação de inserção. |
| Atualizado | Ocorre quando um botão de atualização é clicado, mas depois que o controle DetailsView atualiza a linha. Esse evento é geralmente usado para verificar os resultados da operação de atualização. |
| ItemUpdating | Ocorre quando um botão de atualização é clicado, mas antes de o controle DetailsView atualizar o registro. Esse evento é geralmente usado para cancelar a operação de atualização. |
| ModeChanged | Ocorre depois que o controle DetailsView muda de modos (modo Editar, inserir ou somente leitura). Esse evento é geralmente usado para executar uma tarefa quando o controle DetailsView altera os modos. |
| ModeChanging | Ocorre antes do controle DetailsView alterar os modos (modo de edição, inserção ou somente leitura). Esse evento é geralmente usado para cancelar uma alteração no modo. |
| PageIndexChanged | Ocorre quando um dos botões de paginação é clicado, mas depois que o controle DetailsView manipula a operação de paginação. Esse evento é normalmente usado quando você precisa executar uma tarefa depois que o usuário navega para um registro diferente no controle. |
| PageIndexChanging | Ocorre quando um dos botões de pager é clicado, mas antes que o controle DetailsView manipule a operação de paginação. Esse evento é geralmente usado para cancelar a operação de paginação. |

## <a name="the-menu-control"></a>O controle menu

O controle de menu no ASP.NET 2,0 foi projetado para ser um sistema de navegação completo. Ele pode ser vinculado facilmente a fontes de dados hierárquicas como, por exemplo, SiteMapDataSource.

Uma estrutura de controles de menu pode ser definida de forma declarativa ou dinâmica e consiste em um único nó raiz e qualquer número de subnós. O código a seguir declarativamente define um menu para o controle de menu.

[!code-aspx[Main](data-bound-controls/samples/sample4.aspx)]

No exemplo acima, o nó Home. aspx é o nó raiz. Todos os outros nós são aninhados dentro do nó raiz em vários níveis.

Há dois tipos de menus que o controle de menu pode processar; Menus estáticos e menus dinâmicos. Os menus estáticos consistem em itens de menu que estão sempre visíveis. Os menus dinâmicos consistem em itens de menu que só são visíveis quando o usuário passa o mouse sobre eles. Os clientes geralmente podem confundir menus estáticos com menus definidos declarativamente e dinâmicos com menus que são vinculados em tempo de execução. Na verdade, os menus dinâmicos e estáticos não estão relacionados ao método de população. Os termos *estático* e *dinâmico* referem-se apenas ao fato de o menu ser exibido estaticamente ou não por padrão ou somente exibido quando o usuário executar alguma ação.

A propriedade **StaticDisplayLevels** é usada para configurar quantos níveis do menu são estáticos e, portanto, exibidos por padrão. No exemplo acima, definir a propriedade **StaticDisplayLevels** com um valor de 2 faria com que o menu exibisse estaticamente o nó de início, o nó música e o nó de filmes. Todos os outros nós seriam exibidos dinamicamente quando o usuário passa o mouse sobre o nó pai.

A propriedade **MaximumDynamicDisplayLevels** configura o número máximo de níveis dinâmicos que o menu é capaz de exibir. Todos os menus dinâmicos em um nível acima do valor especificado pela propriedade **MaximumDynamicDisplayLevels** são descartados.

> [!NOTE]
> É quase certo que você pode encontrar situações em que os menus não parecem renderizar devido à propriedade MaximumDynamicDisplayLevels. Nesses casos, verifique se a propriedade está definida suficientemente para permitir a exibição dos menus Customers.

## <a name="data-binding-the-menu-control"></a>Vinculação de dados ao controle menu

O controle menu pode ser associado a qualquer fonte de dados hierárquica, como SiteMapDataSource ou XMLDataSource. O SiteMapDataSource é o método mais comumente usado para vinculação de dados a um controle de menu porque ele alimenta o arquivo Web. sitemap e seu esquema fornece uma API conhecida para o controle de menu. A listagem a seguir mostra um arquivo Web. sitemap simples.

[!code-xml[Main](data-bound-controls/samples/sample5.xml)]

Observe que há apenas um elemento raiz siteMapNode, nesse caso, o elemento Home. Vários atributos podem ser configurados para cada siteMapNode. Os atributos usados com mais frequência são:

- **URL** do Especifica a URL a ser exibida quando um usuário clica no item de menu. Se esse atributo não estiver presente, o nó simplesmente será lançado de volta quando clicado.
- **título** do Especifica o texto que é exibido no item de menu.
- **Descrição** do Usado como documentação para o nó. Também é exibido como uma dica de ferramenta quando o mouse é focalizado sobre o nó.
- **siteMapFile** Permite Sitemaps aninhados. Esse atributo deve apontar para um arquivo de sitemap ASP.NET bem formado.
- **funções** do Permite a aparência de um nó a ser controlado pela remoção de segurança do ASP.NET.

Observe que, embora esses atributos sejam todos opcionais, o comportamento do menu pode não ser o esperado se não forem especificados. Por exemplo, se o atributo de *URL* for especificado, mas o atributo de *Descrição* não for, o nó não ficará visível e não haverá como navegar para a URL especificada.

## <a name="controlling-a-menus-operation"></a>Controlando uma operação de menus

Há várias propriedades que afetam a operação de um controle de menu ASP.NET; a propriedade **Orientation** , a propriedade **DisappearAfter** , a propriedade **StaticItemFormatString** e a propriedade **StaticPopoutImageUrl** são apenas algumas delas.

- A **orientação** pode ser definida como *horizontal* ou *vertical* e controla se os itens de menu estáticos são dispostos horizontalmente em uma linha ou verticalmente e empilhados um no outro. Essa propriedade não afeta os menus dinâmicos.
- A propriedade **DisappearAfter** define por quanto tempo um menu dinâmico deve permanecer visível depois que o mouse é movido para fora dele. O valor é especificado em milissegundos e o padrão é 500. Definir essa propriedade com um valor de-1 fará com que o menu nunca desapareça automaticamente. Nesse caso, o menu desaparecerá apenas quando o usuário clicar fora do menu.
- A propriedade **StaticItemFormatString** facilita a manutenção de argumentação consistentes em seu sistema de menus. Ao especificar essa propriedade, *{0}* deve ser inserido no lugar da descrição que aparece na fonte de dados. Por exemplo, para que o item de menu do exercício 1 diga visitar nossa página de produtos, etc., você especificaria visitar nossa página de {0} para o StaticItemFormatString. Em tempo de execução, o ASP.NET substituirá qualquer ocorrência de {0} pela descrição correta do item de menu.
- A propriedade **StaticPopoutImageUrl** especifica a imagem que é usada para indicar que um nó de menu específico tem nós filho que podem ser acessados passando o mouse sobre ele. Os menus dinâmicos continuarão a usar a imagem padrão.

## <a name="templated-menu-controls"></a>Controles de menu modelo

O controle menu é um controle modelo e permite dois modelos diferentes; o StaticItemTemplate e o DynamicItemTemplate. Usando esses modelos, você pode adicionar facilmente controles de servidor ou controles de usuário aos seus menus.

Para editar os modelos no Visual Studio .NET, clique no botão de marca inteligente no menu e escolha Editar modelos. Em seguida, você pode escolher entre editar o StaticItemTemplate ou o DynamicItemTemplate.

Todos os controles adicionados ao StaticItemTemplate aparecerão no menu estático quando a página for carregada. Todos os controles adicionados ao DynamicItemTemplate serão exibidos em todos os menus pop-up.

## <a name="menu-events"></a>Eventos de menu

O controle de menu tem dois eventos que são exclusivos para ele; o **MenuItemClicked** e o evento **MenuItemDatabound** .

O evento MenuItemClicked é gerado quando um item de menu é clicado. O evento MenuItemDatabound é gerado quando um item de menu é vinculado. O **MenuEventArgs** que é passado para o manipulador de eventos fornece acesso ao item de menu por meio da propriedade item.

## <a name="controlling-a-menus-appearance"></a>Controlando a aparência de menus

Você também pode afetar a aparência de um controle de menu usando um ou mais dos muitos estilos disponíveis para formatar menus. Entre elas estão **StaticMenuStyle**, **DynamicMenuStyle**, **DynamicMenuItemStyle**, **DynamicSelectedStyle**e **DynamicHoverStyle**. Essas propriedades são configuradas usando uma cadeia de estilo HTML padrão. Por exemplo, o seguinte afetará o estilo de menus dinâmicos.

[!code-aspx[Main](data-bound-controls/samples/sample6.aspx)]

> [!NOTE]
> Se você estiver usando qualquer um dos estilos de foco, será necessário adicionar um elemento de&gt; de cabeçalho de &lt;no à página com o elemento *runat* definido como *Server*.

Os controles de menu também dão suporte ao uso de temas do ASP.NET 2,0.

## <a name="the-treeview-control"></a>O controle TreeView

O controle TreeView exibe dados em uma estrutura do tipo árvore. Assim como no controle de menu, ele pode ser facilmente vinculado a qualquer fonte de dados hierárquica, como o SiteMapDataSource.

A primeira pergunta que os clientes provavelmente perguntarão sobre o controle TreeView no ASP.NET 2,0 é se ela está ou não relacionada ao modo de exibição de árvore do modo de exibição do IE que estava disponível para o ASP.NET 1. x. Não é. O controle TreeView ASP.NET 2,0 foi escrito desde o início e oferece uma melhoria significativa sobre o WebControl TreeView do IE que estava disponível anteriormente.

Não entrarei em detalhes sobre como associar um controle TreeView a um mapa do site, pois ele é executado exatamente da mesma forma que o controle de menu. No entanto, o controle TreeView tem algumas diferenças distintas no modo como ele opera.

Por padrão, um controle TreeView aparece totalmente expandido. Para alterar o nível de expansão na carga inicial, modifique a propriedade **ExpandDepth** do controle. Isso é particularmente importante nos casos em que o TreeView é vinculado ao expandir nós específicos.

## <a name="databinding-the-treeview-control"></a>Vinculando o controle TreeView

Ao contrário do controle menu, o TreeView se presta bem para lidar com grandes quantidades de dados. Portanto, além de associação de dados a um SiteMapDataSource ou XMLDataSource, a TreeView costuma ser vinculada a um conjunto ou a outros dados relacionais. Nos casos em que o controle TreeView está associado a grandes quantidades de dados, é melhor associar somente aos dados que estão realmente visíveis no controle. Em seguida, você pode associar dados a dados adicionais, já que nós TreeView são expandidos.

Nesses casos, a propriedade **PopulateOnDemand** de TreeView deve ser definida como *true*. Em seguida, será necessário fornecer uma implementação para o método **TreeNodePopulate** .

## <a name="data-binding-without-postback"></a>Associação de dados sem PostBack

Observe que quando você expande um nó no exemplo anterior pela primeira vez, a página é postada novamente e atualizada. Isso não é um problema neste exemplo, mas você pode imaginar que ele pode estar em um ambiente de produção com uma grande quantidade de dados. Um cenário melhor seria aquele em que o TreeView ainda preenche dinamicamente seus nós, mas sem um postback para o servidor.

Ao definir as propriedades **PopulateNodesFromClient** e **PopulateOnDemand** como true, o controle TreeView ASP.net irá preencher os nós dinamicamente sem um postback. Quando o nó pai é expandido, uma solicitação XMLHttp é feita do cliente e o evento OnTreeNodePopulate é acionado. O servidor responde com uma ilha de dados XML que é usada para associar os dados aos nós filho.

O ASP.NET cria dinamicamente o código do cliente que implementa essa funcionalidade. O script de &lt;&gt; marcas que contêm o script são geradas apontando para um arquivo AXD. Por exemplo, a listagem a seguir mostra os links de script para o código de script que gera a solicitação XMLHttp.

[!code-html[Main](data-bound-controls/samples/sample7.html)]

Se você navegar no arquivo AXD acima no navegador e abri-lo, verá o código que implementa a solicitação XMLHttp. Esse método impede que os clientes modifiquem o arquivo de script.

## <a name="controlling-the-operation-of-the-treeview-control"></a>Controlando a operação do controle TreeView

O controle TreeView tem várias propriedades que afetam a operação do controle. As propriedades mais óbvias são as **caixas de seleção**, **ShowExpandCollapse**e **linhas**.

A propriedade **Excheckboxs** afeta se os nós exibem ou não uma caixa de seleção quando renderizado. Os valores válidos para essa propriedade são **nenhum**, **raiz**, **pai**, **folha**e **todos**. Eles afetam o controle TreeView da seguinte maneira:

| **Valor da propriedade** | **Efeito** |
| --- | --- |
| Nenhum | Caixas de seleção não são exibidas em nenhum nó. Essa é a configuração padrão. |
| Root | Uma caixa de seleção só é exibida no nó raiz. |
| Pai | Uma caixa de seleção só é exibida nesses nós que têm nós filhos. Esses nós filho podem ser nós pai ou nós folha. |
| Folha | Uma caixa de seleção é exibida somente nos nós que não têm nenhum nó filho. |
| Todos | Uma caixa de seleção é exibida em todos os nós. |

Quando caixas de seleção estiverem sendo usadas, a propriedade **CheckedNodes** retornará uma coleção de nós TreeView que são verificados após o postback.

A propriedade **ShowExpandCollapse** controla a aparência da imagem de expansão/recolhimento ao lado dos nós raiz e pai. Se essa propriedade for definida como **false**, os nós de TreeView serão renderizados como hiperlinks e serão expandidos/recolhidos clicando no link.

A propriedade **Conlines** controla se as linhas são exibidas ou não conectando nós pai a nós filho. Quando **false** (o padrão), nenhuma linha é exibida. Quando **true**, o controle TreeView usará as imagens de linhas na pasta especificada pela propriedade **LineImagesFolder** .

Para personalizar a aparência de linhas TreeView, o Visual Studio .NET 2005 inclui uma ferramenta de designer de linha. Você pode acessar essa ferramenta usando o botão de marca inteligente no controle TreeView, como mostrado abaixo.

![](data-bound-controls/_static/image1.jpg)

**Figura 1**

Ao selecionar a opção de menu **Personalizar imagens de linha** , a ferramenta de designer de linha será iniciada permitindo que você configure a aparência de linhas TreeView.

## <a name="treeview-events"></a>Eventos de TreeView

O controle TreeView tem os seguintes eventos exclusivos:

- SelectedNodeChanged ocorre quando um nó é selecionado com base na propriedade **SelectAction** .
- TreeNodeCheckChanged ocorre quando o estado de caixas de seleção de nós é alterado.
- TreeNodeExpanded ocorre quando um nó é expandido com base na propriedade **SelectAction** .
- TreeNodeCollapsed ocorre quando um nó é recolhido.
- TreeNodeDataBound ocorre quando um nó está associado a dados.
- TreeNodePopulate ocorre quando um nó é populado.

A propriedade **SelectAction** permite que você configure qual evento é acionado quando um nó é selecionado. A propriedade SelectAction fornece as seguintes ações:

- TreeNodeSelectAction. Expand gera TreeNodeExpanded quando o nó é selecionado.
- TreeNodeSelectAction. None não lança nenhum evento quando o nó é selecionado.
- TreeNodeSelectAction. Select gera o evento SelectedNodeChanged quando o nó é selecionado.
- TreeNodeSelectAction. SelectExpand gera o evento SelectedNodeChanged e o evento TreeNodeExpanded quando o nó é selecionado.

## <a name="controlling-appearance-with-styles"></a>Controlando a aparência com estilos

O controle TreeView fornece muitas propriedades para controlar a aparência do controle com estilos. As propriedades a seguir estão disponíveis.

| **Nome da Propriedade** | **Controles** |
| --- | --- |
| HoverNodeStyle | Controla o estilo de nós quando o mouse passa sobre eles. |
| LeafNodeStyle | Controla o estilo de nós folha. |
| NodeStyle | Controla o estilo de todos os nós. Os estilos de nó específicos (como LeafNodeStyle) substituem esse estilo. |
| ParentNodeStyle | Controla o estilo de todos os nós pai. |
| RootNodeStyle | Controla o estilo do nó raiz. |
| SelectedNodeStyle | Controla o estilo do nó selecionado. |

Cada uma dessas propriedades é somente leitura. No entanto, cada um retorna um objeto **TreeNodeStyle** e as propriedades desse objeto podem ser modificadas usando o formato *Property-subpropriedade* . Por exemplo, para definir a propriedade **ForeColor** de **SelectedNodeStyle**, você usaria a seguinte sintaxe:

[!code-aspx[Main](data-bound-controls/samples/sample8.aspx)]

Observe que a marca acima não está fechada. Isso ocorre porque, ao usar a sintaxe declarativa mostrada aqui, você também inclui os nós TreeViews no código HTML.

As propriedades de estilo também podem ser especificadas no código usando o formato *Property. subpropriedade* . Por exemplo, para definir a propriedade **ForeColor** do **RootNodeStyle** no código, você usaria a seguinte sintaxe:

[!code-csharp[Main](data-bound-controls/samples/sample9.cs)]

> [!NOTE]
> Para obter uma lista abrangente das propriedades de estilo diferentes, consulte a documentação do MSDN no objeto TreeNodestyle.

## <a name="the-sitemappath-control"></a>O controle SiteMapPath

O controle SiteMapPath fornece um controle de navegação de trilha estrutural para desenvolvedores de ASP.NET. Como os outros controles de navegação, ele pode ser facilmente vinculado a fontes de dados hierárquicas, como SiteMapDataSource ou XmlDataSource.

Um controle SiteMapPath é composto de objetos SiteMapNodeItem. Há três tipos de nós; o nó raiz, os nós pai e o nó atual. O nó raiz é o nó na parte superior da estrutura hierárquica. O nó atual representa a página atual. Todos os outros nós são nós pai.

## <a name="controlling-the-operation-of-the-sitemappath-control"></a>Controlando a operação do controle SiteMapPath

As propriedades que controlam a operação do controle SiteMapPath são as seguintes:

| **Propriedade** | **Descrição da propriedade** |
| --- | --- |
| ParentLevelsDisplayed | Controla quantos nós pai são exibidos. O padrão é-1 que impõe nenhuma restrição sobre o número de nós pai exibidos. |
| PathDirection | Controla a direção do SiteMapPath. Os valores válidos são RootToCurrent (padrão) e CurrentToRoot. |
| PathSeparator | Uma cadeia de caracteres que controla o caractere que separa nós em um controle SiteMapPath. O valor padrão é:. |
| RenderCurrentNodeAsLink | Um valor booliano que controla se o nó atual é renderizado ou não como um link. O padrão é False. |
| SkipLinkText | Ajuda na acessibilidade quando a página é exibida por leitores de tela. Essa propriedade permite que os leitores de tela ignorem o controle SiteMapPath. Para desabilitar esse recurso, defina a propriedade como String. Empty. |

## <a name="templated-sitemappath-controls"></a>Controles SiteMapPath de modelo

O SiteMapControl é um controle de modelo e, como tal, você pode definir modelos diferentes para uso na exibição do controle. Para editar modelos em um controle SiteMapPath, clique no botão de marca inteligente no controle e escolha Editar modelos no menu. Isso exibe o menu SiteMapTasks, conforme mostrado abaixo, onde você pode escolher entre os diferentes modelos disponíveis.

![](data-bound-controls/_static/image2.jpg)

**Figura 2**

O modelo **NodeTemplate** se refere a qualquer nó no SiteMapPath. Se o nó for um nó raiz ou o nó atual e um **RootNodeTemplate** ou **CurrentNodeTemplate** estiverem configurados, o NodeTemplate será substituído.

## <a name="sitemappath-events"></a>Eventos SiteMapPath

O controle SiteMapPath tem dois eventos que não são derivados da classe Control; o evento **criado** e o evento de **Hiperligação** de eventos. O evento criado é gerado quando um item SiteMapPath é criado. Os vínculos de dados são gerados quando o método DataBind é chamado durante a vinculação de dado de um nó SiteMapPath. Um objeto **SiteMapNodeItemEventArgs** fornece acesso ao SiteMapNodeItem específico por meio da propriedade item.

## <a name="controlling-appearance-with-styles"></a>Controlando a aparência com estilos

Os estilos a seguir estão disponíveis para formatação de um controle SiteMapPath.

| **Nome da Propriedade** | **Controles** |
| --- | --- |
| CurrentNodeStyle | Controla o estilo do texto do nó atual. |
| RootNodeStyle | Controla o estilo do texto do nó raiz. |
| NodeStyle | Controla o estilo do texto para todos os nós supondo que um CurrentNodeStyle ou RootNodeStyle não se aplica. |

A propriedade NodeStyle é substituída pelo CurrentNodeStyle ou pelo RootNodeStyle. Cada uma dessas propriedades é somente leitura e retorna um objeto **Style** . Para afetar a aparência de um nó usando uma dessas propriedades, será necessário definir as propriedades do objeto Style retornado. Por exemplo, o código a seguir altera a propriedade ForeColor do nó atual.

[!code-aspx[Main](data-bound-controls/samples/sample10.aspx)]

A propriedade também pode ser aplicada programaticamente da seguinte maneira:

[!code-csharp[Main](data-bound-controls/samples/sample11.cs)]

> [!NOTE]
> Se um modelo for aplicado, o estilo não será aplicado.

## <a name="lab-1-configuring-an-aspnet-menu-control"></a>Laboratório 1: Configurando um controle de menu ASP.NET

1. Crie um novo site da Web.
2. Adicione um arquivo de mapa do site selecionando arquivo, novo, arquivo e escolhendo mapa do site na lista de modelos de arquivo.
3. Abra o mapa do site (Web. sitemap por padrão) e modifique-o para que fique semelhante à listagem abaixo. As páginas às quais você está vinculando no arquivo do mapa do site não existem de fato, mas isso não será um problema para este exercício.

    [!code-xml[Main](data-bound-controls/samples/sample12.xml)]
4. Abra o formulário da Web padrão em modo de exibição de Design.
5. Na seção de navegação da caixa de ferramentas, adicione um novo controle de menu à página.
6. Na seção dados da caixa de ferramentas, adicione um novo SiteMapDataSource. O SiteMapDataSource usará automaticamente o arquivo Web. sitemap em seu site. (O arquivo Web. sitemap *deve* estar na pasta raiz do site.)
7. Clique no controle de menu e, em seguida, clique no botão de marca inteligente para exibir a caixa de diálogo tarefas de menu.
8. Na lista suspensa escolher fonte de dados, selecione SiteMapDataSource1.
9. Clique no link AutoFormat e escolha um formato para o menu.
10. No painel Propriedades, defina a propriedade **StaticDisplayLevels** como 2. O controle menu agora deve exibir o nó Home, Products e Services no designer.
11. Procure a página no navegador para usar o menu. (Como as páginas que você adicionou ao mapa do site não existem de fato, você verá um erro quando tentar e navegar até elas.)

Experimente alterar as propriedades StaticDisplayLevels e MaximumDynamicDisplayLevels e veja como elas afetam a forma como o menu é renderizado.

## <a name="lab-2-dynamically-binding-a-treeview-control"></a>Laboratório 2: ligando dinamicamente um controle TreeView

Este exercício pressupõe que você tenha SQL Server em execução localmente e que o banco de dados Northwind esteja presente na instância do SQL Server. Se essas condições não forem atendidas, altere a cadeia de conexão no exemplo. Observe que você também pode precisar especificar SQL Server autenticação em vez de uma conexão confiável.

1. Crie um novo site da Web.
2. Alterne para a exibição de código para default. aspx e substitua todo o código pelo código listado abaixo. 

    [!code-aspx[Main](data-bound-controls/samples/sample13.aspx)]
3. Salve a página como TreeView. aspx.
4. Procure a página.
5. Quando a página for exibida pela primeira vez, exiba a origem da página no navegador. Observe que apenas os nós visíveis foram enviados ao cliente.
6. Clique no sinal de adição ao lado de qualquer nó.
7. Exiba a fonte novamente na página. Observe que os nós recentemente exibidos agora estão presentes.

## <a name="lab-3-details-view-and-editing-data-using-a-gridview-and-detailsview"></a>Laboratório 3: Exibir detalhes e editar dados usando um GridView e DetailsView

1. Crie um novo site da Web.
2. Adicione um novo Web. config ao site.
3. Adicione uma cadeia de conexão ao arquivo Web. config, conforme mostrado abaixo: 

    [!code-xml[Main](data-bound-controls/samples/sample14.xml)]

    > [!NOTE]
    > Talvez seja necessário alterar a cadeia de conexão com base no seu ambiente.
4. Salve e feche o arquivo Web. config.
5. Abra default. aspx e adicione um novo controle SqlDataSource.
6. Altere a ID do controle SqlDataSource para **Products**.
7. No menu **tarefas SqlDataSource** , clique em **Configurar fonte de dados**.
8. Selecione **Northwind** na lista suspensa conexão e clique em Avançar.
9. Selecione **produtos** na lista suspensa **nome** e marque as caixas de seleção **ProductID**, **NomeDoProduto**, **PreçoUnitário**e **UnidadesEmEstoque** , conforme mostrado abaixo. 

![](data-bound-controls/_static/image3.jpg)

    **Figure 3**
10. Clique em **Próximo**.
11. Clique em **Concluir**.
12. Alterne para o modo de exibição de origem e examine o código que foi gerado. Observe o **SelectCommand**, **DeleteCommand**, **InsertCommand**e **UpdateCommand** que foram adicionados ao controle SqlDataSource. Observe também os parâmetros que foram adicionados.
13. Alterne para modo de exibição de Design e adicione um novo controle GridView à página.
14. Selecione **produtos** na lista suspensa **escolher fonte de dados** .
15. Marque **habilitar paginação** e **habilitar a seleção** , conforme mostrado abaixo. 

![](data-bound-controls/_static/image4.jpg)

    **Figure 4**
16. Clique no link **Editar colunas** e verifique se **a opção Gerar campos automaticamente** está marcada.
17. Clique em **OK**.
18. Com o controle GridView selecionado, clique no botão ao lado da propriedade **DataKeyNames** no painel Propriedades.
19. Selecione **ProductID** na lista **campos de dados disponíveis** e clique no botão **&gt;** para adicioná-lo.
20. Clique em OK.
21. Adicione um novo controle SqlDataSource à página.
22. Altere a ID do controle SqlDataSource para **detalhes**.
23. No menu tarefas SqlDataSource, escolha **Configurar fonte de dados**.
24. Escolha **Northwind** na lista suspensa e clique em **Avançar**.
25. Selecione <strong>produtos</strong> na lista suspensa <strong>nome</strong> e marque a caixa de seleção <strong>\</Strong > * na ListBox <strong>colunas</strong> .
26. Clique no botão **onde** .
27. Selecione **ProductID** na lista suspensa **coluna** .
28. Selecione **=** na lista suspensa operador.
29. Selecione **controle** na lista suspensa **origem** .
30. Selecione **GridView1** na lista suspensa **ID de controle** .
31. Clique no botão **Adicionar** para adicionar a cláusula WHERE.
32. Clique em **OK**.
33. Clique no botão **avançado** e marque a caixa de seleção **gerar instruções INSERT, Update e Delete** .
34. Clique em **OK**.
35. Clique em **Avançar** e em **concluir**.
36. Adicione um controle DetailsView à página.
37. Na lista suspensa **escolher fonte de dados** , escolha **detalhes**.
38. Marque a caixa de seleção **Habilitar edição** , conforme mostrado abaixo. 

![](data-bound-controls/_static/image1.gif)

    **Figure 5**
39. Salve a página e procure default. aspx.
40. Clique no link **selecionar** ao lado de registros diferentes para ver a atualização de DetailsView automaticamente.
41. Clique no link **Editar** no controle DetailsView.
42. Faça uma alteração no registro e clique em **Atualizar**.
