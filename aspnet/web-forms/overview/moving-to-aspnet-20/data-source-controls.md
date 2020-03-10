---
uid: web-forms/overview/moving-to-aspnet-20/data-source-controls
title: Controles de fonte de dados | Microsoft Docs
author: microsoft
description: O controle DataGrid no ASP.NET 1. x marcou uma ótima melhoria no acesso a dados em aplicativos Web. No entanto, não era tão amigável quanto o usuário...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 78fd0e92-f9c6-4e96-a5e9-0375b307a828
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-source-controls
msc.type: authoredcontent
ms.openlocfilehash: a2e2cfbec3e5aebf42a2de30bab7d45b4b610298
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78639408"
---
# <a name="data-source-controls"></a>Controles de fonte de dados

pela [Microsoft](https://github.com/microsoft)

> O controle DataGrid no ASP.NET 1. x marcou uma ótima melhoria no acesso a dados em aplicativos Web. No entanto, não era tão amigável quanto o usuário. Ele ainda exigia uma quantidade considerável de código para obter uma funcionalidade muito útil. Esse é o modelo em todos os esforços de acesso a dados em 1. x.

O controle DataGrid no ASP.NET 1. x marcou uma ótima melhoria no acesso a dados em aplicativos Web. No entanto, não era tão amigável quanto o usuário. Ele ainda exigia uma quantidade considerável de código para obter uma funcionalidade muito útil. Esse é o modelo em todos os esforços de acesso a dados em 1. x.

ASP.NET 2,0 aborda isso com o em parte com controles de fonte de dados. Os controles da fonte de dados no ASP.NET 2,0 fornecem aos desenvolvedores um modelo declarativo para recuperação de dados, exibição de dados e edição de dados. A finalidade dos controles da fonte de dados é fornecer uma representação consistente dos dados para controles vinculados a dados, independentemente da origem desses dados. No coração dos controles da fonte de dados no ASP.NET 2,0 está a classe do DataSourceControl abstract. A classe DataSourceControl fornece uma implementação base da interface IDataSource e da interface IListSource, a última que permite que você atribua o controle da fonte de dados como a DataSource de um controle associado a dados (por meio da nova propriedade DataSourceId discutida posteriormente) e expor os dados contidos como uma lista. Cada lista de dados de um controle da fonte de dados é exposta como um objeto DataSourceView. O acesso às instâncias DataSourceView é fornecido pela interface IDataSource. Por exemplo, o método GetViewNames retorna um ICollection que permite enumerar os DataSourceViews associados a um controle de fonte de dados específico, e o método GetView permite que você acesse uma instância DataSourceView específica por nome.

Os controles da fonte de dados não têm nenhuma interface do usuário. Eles são implementados como controles de servidor para que possam dar suporte à sintaxe declarativa e para que eles tenham acesso ao estado da página, se desejado. Os controles de fonte de dados não processam nenhuma marcação HTML no cliente.

> [!NOTE]
> Como você verá posteriormente, também há benefícios de cache obtidos com o uso de controles de fonte de dados.

## <a name="storing-connection-strings"></a>Armazenando cadeias de conexão

Antes de examinarmos como configurar os controles da fonte de dados, devemos abordar um novo recurso no ASP.NET 2,0 em relação a cadeias de conexão. O ASP.NET 2,0 apresenta uma nova seção no arquivo de configuração que permite que você armazene facilmente cadeias de conexão que podem ser lidas dinamicamente em tempo de execução. A seção &lt;connectionStrings&gt; facilita o armazenamento de cadeias de conexão.

O trecho a seguir adiciona uma nova cadeia de conexão.

[!code-xml[Main](data-source-controls/samples/sample1.xml)]

> [!NOTE]
> Assim como ocorre com a seção &lt;appSettings&gt;, a seção &lt;connectionStrings&gt; aparece fora da seção &lt;System. Web&gt; no arquivo de configuração.

Para usar essa cadeia de conexão, você pode usar a sintaxe a seguir ao definir o atributo ConnectionString de um controle de servidor.

[!code-aspx[Main](data-source-controls/samples/sample2.aspx)]

A seção &lt;connectionStrings&gt; também pode ser criptografada para que as informações confidenciais não sejam expostas. Essa capacidade será abordada em um módulo posterior.

## <a name="caching-data-sources"></a>Armazenando fontes de dados em cache

Cada DataSourceControl fornece quatro propriedades para configurar o cache; EnableCaching, CacheDuration, CacheExpirationPolicy e CacheKeyDependency.

## <a name="enablecaching"></a>EnableCaching

EnableCaching é uma propriedade booliana que determina se o Caching está habilitado ou não para o controle da fonte de dados.

## <a name="cacheduration-property"></a>Propriedade CacheDuration

A propriedade CacheDuration define o número de segundos que o cache permanece válido. Definir essa propriedade como **0** faz com que o cache permaneça válido até invalidar explicitamente.

## <a name="cacheexpirationpolicy-property"></a>Propriedade CacheExpirationPolicy

A propriedade CacheExpirationPolicy pode ser definida como **absoluta** ou **deslizante**. Defini-lo como Absolute significa que a quantidade máxima de tempo em que os dados serão armazenados em cache é o número de segundos especificado pela propriedade CacheDuration. Ao defini-lo como deslizante, o tempo de expiração é redefinido quando cada operação é executada.

## <a name="cachekeydependency-property"></a>Propriedade CacheKeyDependency

Se um valor de cadeia de caracteres for especificado para a propriedade CacheKeyDependency, ASP.NET irá configurar uma nova dependência de cache com base nessa cadeia de caracteres. Isso permite invalidar explicitamente o cache simplesmente alterando ou removendo o CacheKeyDependency.

**Importante**: se a representação estiver habilitada e o acesso à fonte de dados e/ou ao conteúdo dos dados for baseado na identidade do cliente, é recomendável que o cache seja desabilitado definindo EnableCaching como false. Se o Caching estiver habilitado nesse cenário e um usuário diferente do usuário que solicitou originalmente os dados emitir uma solicitação, a autorização para a fonte de dados não será imposta. Os dados serão simplesmente servidos do cache.

## <a name="the-sqldatasource-control"></a>O controle SqlDataSource

O controle SqlDataSource permite que um desenvolvedor acesse dados armazenados em qualquer banco de dado relacional que dá suporte a ADO.NET. Ele pode usar o provedor System. Data. SqlClient para acessar um banco de dados SQL Server, o provedor System. Data. OleDb, o provedor System. Data. ODBC ou o provedor System. Data. OracleClient para acessar o Oracle. Portanto, o SqlDataSource certamente não é usado apenas para acessar dados em um banco SQL Server.

Para usar o SqlDataSource, basta fornecer um valor para a propriedade ConnectionString e especificar um comando SQL ou um procedimento armazenado. O controle SqlDataSource cuida do trabalho com a arquitetura ADO.NET subjacente. Ele abre a conexão, consulta a fonte de dados ou executa o procedimento armazenado, retorna os dados e fecha a conexão para você.

> [!NOTE]
> Como a classe DataSourceControl fecha automaticamente a conexão para você, ela deve reduzir o número de chamadas de clientes geradas vazando conexões de banco de dados.

O trecho de código a seguir associa um controle DropDownList a um controle SqlDataSource usando a cadeia de conexão armazenada no arquivo de configuração, conforme mostrado acima.

[!code-aspx[Main](data-source-controls/samples/sample3.aspx)]

Conforme ilustrado acima, a propriedade DataSourceMode de SqlDataSource especifica o modo para a fonte de dados. No exemplo acima, o DataSourceMode é definido como DataReader. Nesse caso, o SqlDataSource retornará um objeto IDataReader usando um cursor somente de avanço e somente leitura. O tipo de objeto especificado que é retornado é controlado pelo provedor que é usado. Nesse caso, estou usando o provedor System. Data. SqlClient conforme especificado na seção &lt;connectionStrings&gt; do arquivo Web. config. Portanto, o objeto retornado será do tipo SqlDataReader. Ao especificar um valor de DataSourceMode de DataSet, os dados podem ser armazenados em um DataSet no servidor. Esse modo permite que você adicione recursos como classificação, paginação, etc. Se eu tivesse sido a vinculação de dados de SqlDataSource a um controle GridView, teria escolhido o modo DataSet. No entanto, no caso de uma DropDownList, o modo DataReader é a opção correta.

> [!NOTE]
> Ao armazenar em cache um SqlDataSource ou um AccessDataSource, a propriedade DataSourceMode deve ser definida como DataSet. Ocorrerá uma exceção se você habilitar o Caching com um DataSourceMode de DataReader.

## <a name="sqldatasource-properties"></a>Propriedades de SqlDataSource

A seguir estão algumas das propriedades do controle SqlDataSource.

### <a name="cancelselectonnullparameter"></a>CancelSelectOnNullParameter

Um valor booliano que especifica se um comando SELECT será cancelado se um dos parâmetros for nulo. True por padrão.

### <a name="conflictdetection"></a>ConflictDetection

Em uma situação em que vários usuários podem estar atualizando uma fonte de dados ao mesmo tempo, a propriedade ConflictDetection determina o comportamento do controle SqlDataSource. Essa propriedade é avaliada como um dos valores da Enumeração ConflictOptions. Esses valores são **CompareAllValues** e **OverwriteChanges**. Se definido como OverwriteChanges, a última pessoa a gravar dados na fonte de dados substituirá as alterações anteriores. No entanto, se a propriedade ConflictDetection for definida como CompareAllValues, os parâmetros criados para as colunas retornadas pelo SelectCommand e parâmetros também serão criados para conter os valores originais em cada uma dessas colunas, permitindo o SqlDataSource a Determine se os valores foram alterados desde que o SelectCommand foi executado.

### <a name="deletecommand"></a>DeleteCommand

Define ou obtém a cadeia de caracteres SQL usada ao excluir linhas do banco de dados. Pode ser uma consulta SQL ou um nome de procedimento armazenado.

### <a name="deletecommandtype"></a>DeleteCommandType

Define ou Obtém o tipo de comando delete, uma consulta SQL (texto) ou um procedimento armazenado (StoredProcedure).

### <a name="deleteparameters"></a>DeleteParameters

Retorna os parâmetros que são usados pelo DeleteCommand do objeto SqlDataSourceView associado ao controle SqlDataSource.

### <a name="oldvaluesparameterformatstring"></a>OldValuesParameterFormatString

Essa propriedade é usada para especificar o formato dos parâmetros de valor original nos casos em que a propriedade ConflictDetection está definida como CompareAllValues. O padrão é {0}, o que significa que os parâmetros de valor original terão o mesmo nome que o parâmetro original. Em outras palavras, se o nome do campo for EmployeeID, o parâmetro valor original será @EmployeeID.

### <a name="selectcommand"></a>SelectCommand

Define ou obtém a cadeia de caracteres SQL que é usada para recuperar dados do banco de dado. Pode ser uma consulta SQL ou um nome de procedimento armazenado.

### <a name="selectcommandtype"></a>SelectCommandType

Define ou Obtém o tipo de comando SELECT, uma consulta SQL (texto) ou um procedimento armazenado (StoredProcedure).

### <a name="selectparameters"></a>Select

Retorna os parâmetros que são usados pelo SelectCommand do objeto SqlDataSourceView associado ao controle SqlDataSource.

### <a name="sortparametername"></a>SortParameterName

Obtém ou define o nome de um parâmetro de procedimento armazenado que é usado ao classificar dados recuperados pelo controle da fonte de dados. Válido somente quando SelectCommandtype é definido como StoredProcedure.

### <a name="sqlcachedependency"></a>SqlCacheDependency

Uma cadeia de caracteres delimitada por ponto e vírgula especificando os bancos de dados e as tabelas usados em uma dependência de cache SQL Server. (As dependências do cache do SQL serão discutidas em um módulo posterior.)

### <a name="updatecommand"></a>UpdateCommand

Define ou obtém a cadeia de caracteres SQL que é usada ao atualizar dados no banco de dado. Pode ser uma consulta SQL ou um nome de procedimento armazenado.

### <a name="updatecommandtype"></a>UpdateCommandType

Define ou Obtém o tipo de comando Update, uma consulta SQL (texto) ou um procedimento armazenado (StoredProcedure).

### <a name="updateparameters"></a>UpdateParameters

Retorna os parâmetros que são usados pelo UpdateCommand do objeto SqlDataSourceView associado ao controle SqlDataSource.

## <a name="the-accessdatasource-control"></a>O controle AccessDataSource

O controle AccessDataSource deriva da classe SqlDataSource e é usado para vinculação de dados a um banco de dado do Microsoft Access. A propriedade ConnectionString do controle AccessDataSource é uma propriedade somente leitura. Em vez de usar a propriedade ConnectionString, a propriedade DataFile é usada para apontar para o banco de dados do Access, conforme mostrado abaixo.

[!code-aspx[Main](data-source-controls/samples/sample4.aspx)]

O AccessDataSource sempre definirá o ProviderName da base SqlDataSource para System. Data. OleDb e se conectará ao banco de dados usando o provedor de OLE DB Microsoft. Jet. OLEDB. 4.0. Você não pode usar o controle AccessDataSource para se conectar a um banco de dados de acesso protegido por senha. Se você precisar se conectar a um banco de dados protegido por senha, deverá usar o controle SqlDataSource.

> [!NOTE]
> Os bancos de dados do Access armazenados no site devem ser colocados no diretório Data\_do aplicativo. ASP.NET não permite que arquivos neste diretório sejam procurados. Você precisará conceder as permissões de leitura e gravação da conta de processo ao diretório de dados do\_de aplicativos ao usar bancos de dado do Access.

## <a name="the-xmldatasource-control"></a>O controle XmlDataSource

O XmlDataSource é usado para associar dados XML a controles vinculados a dados. Você pode associar a um arquivo XML usando a propriedade DataFile ou pode associar a uma cadeia de caracteres XML usando a propriedade Data. O XmlDataSource expõe atributos XML como campos vinculáveis. Nos casos em que você precisa associar a valores que não são representados como atributos, será necessário usar uma transformação XSL. Você também pode usar expressões XPath para filtrar dados XML.

Considere o seguinte arquivo XML:

[!code-xml[Main](data-source-controls/samples/sample5.xml)]

Observe que o XmlDataSource usa uma propriedade XPath de *pessoas/pessoa* para filtrar apenas o &lt;pessoa&gt; nós. O DropDownList então vincula dados ao atributo LastName usando a propriedade DataTextField.

Embora o controle XmlDataSource seja usado principalmente para associar dados a dados XML somente leitura, é possível editar o arquivo de dados XML. Observe que, nesses casos, a inserção automática, a atualização e a exclusão de informações no arquivo XML não acontecem automaticamente como faz com outros controles de fonte de dados. Em vez disso, você precisará escrever código para editar manualmente os dados usando os métodos a seguir do controle XmlDataSource.

### <a name="getxmldocument"></a>GetXmlDocument

Recupera um objeto XmlDocument que contém o código XML recuperado pelo XmlDataSource.

### <a name="save"></a>Salvar

Salva o XmlDocument na memória de volta para a fonte de dados.

É importante perceber que o método Save funcionará apenas quando as duas condições a seguir forem atendidas:

1. O XmlDataSource está usando a propriedade DataFile para associar a um arquivo XML em vez da propriedade data a ser associada a dados XML na memória.
2. Nenhuma transformação é especificada por meio da propriedade Transform ou transformafile.

Observe também que o método Save pode gerar resultados inesperados quando chamados por vários usuários simultaneamente.

## <a name="the-objectdatasource-control"></a>O controle ObjectDataSource

Os controles da fonte de dados que abordamos até esse ponto são excelentes opções para aplicativos de duas camadas em que o controle da fonte de dados se comunica diretamente com o armazenamento de dados. No entanto, muitos aplicativos do mundo real são aplicativos de várias camadas em que um controle da fonte de dados pode precisar se comunicar com um objeto comercial que, por sua vez, se comunica com a camada de dados. Nessas situações, o ObjectDataSource preenche a fatura. O ObjectDataSource funciona em conjunto com um objeto de origem. O controle ObjectDataSource criará uma instância do objeto de origem, chamará o método especificado e descartará a instância do objeto no escopo de uma única solicitação, se o objeto tiver métodos de instância em vez de métodos estáticos (compartilhados em Visual Basic). Portanto, o objeto deve ser sem estado. Ou seja, o objeto deve adquirir e liberar todos os recursos necessários dentro do período de uma única solicitação. Você pode controlar como o objeto de origem é criado manipulando o evento recriation do controle ObjectDataSource. Você pode criar uma instância do objeto de origem e, em seguida, definir a propriedade ObjectInstance da classe ObjectDataSourceEventArgs para essa instância. O controle ObjectDataSource usará a instância criada no evento objectcriar em vez de criar uma instância por conta própria.

Se o objeto de origem para um controle ObjectDataSource expõe métodos estáticos públicos (compartilhados em Visual Basic) que podem ser chamados para recuperar e modificar dados, um controle ObjectDataSource chamará esses métodos diretamente. Se um controle ObjectDataSource precisar criar uma instância do objeto de origem para fazer chamadas de método, o objeto deverá incluir um construtor público que não usa parâmetros. O controle ObjectDataSource chamará esse construtor quando ele criar uma nova instância do objeto de origem.

Se o objeto de origem não contiver um construtor público sem parâmetros, você poderá criar uma instância do objeto de origem que será usada pelo controle ObjectDataSource no evento objectcriation.

## <a name="specifying-object-methods"></a>Especificando métodos de objeto

O objeto de origem para um controle ObjectDataSource pode conter qualquer número de métodos que são usados para selecionar, inserir, atualizar ou excluir dados. Esses métodos são chamados pelo controle ObjectDataSource com base no nome do método, conforme identificado usando a propriedade SelectMethod, InsertMethod, UpdateMethod ou DeleteMethod do controle ObjectDataSource. O objeto de origem também pode incluir um método SelectCount opcional, que é identificado pelo controle ObjectDataSource usando a propriedade SelectCountMethod, que retorna a contagem do número total de objetos na fonte de dados. O controle ObjectDataSource chamará o método SelectCount depois que um método Select tiver sido chamado para recuperar o número total de registros na fonte de dados para uso na paginação.

## <a name="lab-using-data-source-controls"></a>Laboratório usando controles de fonte de dados

## <a name="exercise-1---displaying-data-with-the-sqldatasource-control"></a>Exercício 1-exibindo dados com o controle SqlDataSource

O exercício a seguir usa o controle SqlDataSource para se conectar ao banco de dados Northwind. Ele pressupõe que você tenha acesso ao banco de dados Northwind em uma instância SQL Server 2000.

1. Criar um site do ASP.NET.
2. Adicione um novo arquivo Web. config.

    1. Clique com o botão direito do mouse no projeto no Gerenciador de Soluções e clique em Adicionar novo item.
    2. Escolha Arquivo de configuração da Web na lista de modelos e clique em Adicionar.
3. Edite a seção &lt;connectionStrings&gt; da seguinte maneira: 

    [!code-aspx[Main](data-source-controls/samples/sample6.aspx)]
4. Alterne para a exibição de código e adicione um atributo ConnectionString e um atributo SelectCommand ao controle &lt;ASP: SqlDataSource&gt; da seguinte maneira: 

    [!code-aspx[Main](data-source-controls/samples/sample7.aspx)]
5. Em modo de exibição de Design, adicione um novo controle GridView.
6. Na lista suspensa escolher fonte de dados no menu tarefas GridView, escolha SqlDataSource1.
7. Clique com o botão direito do mouse em Default. aspx e escolha Exibir no navegador no menu. Clique em Sim quando for solicitado a salvar.
8. O GridView exibe os dados da tabela Products.

## <a name="exercise-2---editing-data-with-the-sqldatasource-control"></a>Exercício 2-editando dados com o controle SqlDataSource

O exercício a seguir demonstra como associar dados a um controle DropDownList usando a sintaxe declarativa e permite que você edite os dados apresentados no controle DropDownList.

1. Em modo de exibição de Design, exclua o controle GridView de default. aspx. 

    **Importante**: Deixe o controle SqlDataSource na página.
2. Adicione um controle DropDownList a default. aspx.
3. Alternar para o modo de exibição de código-fonte.
4. Adicione um atributo DataSourceId, DataTextField e DataValueField ao controle &lt;ASP: DropDownList&gt; da seguinte maneira: 

    [!code-aspx[Main](data-source-controls/samples/sample8.aspx)]
5. Salve default. aspx e exiba-o no navegador. Observe que a DropDownList contém todos os produtos do banco de dados Northwind.
6. Feche o navegador.
7. Na exibição de código-fonte de default. aspx, adicione um novo controle TextBox abaixo do controle DropDownList. Altere a propriedade ID da caixa de texto para txtProductName.
8. No controle TextBox, adicione um novo controle Button. Altere a propriedade ID do botão para btnUpdate e a propriedade Text para **atualizar o nome do produto**.
9. Na exibição de código-fonte de default. aspx, adicione uma propriedade UpdateCommand e dois novos UpdateParameters à marca SqlDataSource da seguinte maneira: 

    [!code-aspx[Main](data-source-controls/samples/sample9.aspx)]

    > [!NOTE]
    > Observe que há dois parâmetros de atualização (NomeDoProduto e ProductID) adicionados neste código. Esses parâmetros são mapeados para a propriedade Text da caixa de texto txtProductName e da propriedade SelectedValue de ddlProducts DropDownList.
10. Alterne para modo de exibição de Design e clique duas vezes no controle de botão para adicionar um manipulador de eventos.
11. Adicione o seguinte código ao btnUpdate\_clique em código: 

    [!code-csharp[Main](data-source-controls/samples/sample10.cs)]
12. Clique com o botão direito do mouse em Default. aspx e escolha para exibi-lo no navegador. Clique em Sim quando for solicitado para salvar todas as alterações.
13. As classes parciais ASP.NET 2,0 permitem a compilação no tempo de execução. Não é necessário criar um aplicativo para ver as alterações de código entrarem em vigor.
14. Selecione um produto da DropDownList.
15. Insira um novo nome para o produto selecionado na caixa de texto e, em seguida, clique no botão atualizar.
16. O nome do produto é atualizado no banco de dados.

## <a name="exercise-3-using-the-objectdatasource-control"></a>Exercício 3 usando o controle ObjectDataSource

Este exercício demonstrará como usar o controle ObjectDataSource e um objeto de origem para interagir com o banco de dados Northwind.

1. Clique com o botão direito do mouse no projeto no Gerenciador de Soluções e clique em Adicionar novo item.
2. Selecione formulário da Web na lista modelos. Altere o nome para Object. aspx e clique em Adicionar.
3. Clique com o botão direito do mouse no projeto no Gerenciador de Soluções e clique em Adicionar novo item.
4. Selecione classe na lista modelos. Altere o nome da classe para NorthwindData.cs e clique em Adicionar.
5. Clique em Sim quando solicitado a adicionar a classe à pasta de código de\_do aplicativo.
6. Adicione o seguinte código ao arquivo NorthwindData.cs: 

    [!code-csharp[Main](data-source-controls/samples/sample11.cs)]
7. Adicione o seguinte código à exibição de código-fonte de Object. aspx: 

    [!code-aspx[Main](data-source-controls/samples/sample12.aspx)]
8. Salve todos os arquivos e procure Object. aspx.
9. Interaja com a interface exibindo detalhes, editando funcionários, adicionando funcionários e excluindo funcionários.
