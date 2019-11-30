---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-cs
title: Configurando a conexão da camada de acesso a dados e as configuraçõesC#de nível de comando () | Microsoft Docs
author: rick-anderson
description: Os TableAdapters em um conjunto de dados tipado automaticamente cuidam da conexão com o banco de dados, emitindo comandos e preenchendo uma DataTable com os resultados...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: cd330dd9-6254-4305-9351-dd727384c83b
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-cs
msc.type: authoredcontent
ms.openlocfilehash: 8fe7a5a2e410b47c8c2be62851f2b7b775d60209
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74604883"
---
# <a name="configuring-the-data-access-layers-connection--and-command-level-settings-c"></a>Definir as configurações de nível de conexão e comando da Camada de Acesso a Dados (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_72_CS.zip) ou [baixar PDF](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/datatutorial72cs1.pdf)

> Os TableAdapters em um conjunto de dados tipado automaticamente cuidam da conexão com o banco de dados, emitindo comandos e preenchendo uma DataTable com os resultados. No entanto, há ocasiões em que desejamos cuidar desses detalhes e, neste tutorial, aprendemos como acessar as configurações de nível de comando e de conexão do banco de dados no TableAdapter.

## <a name="introduction"></a>Introdução

Em toda a série de tutoriais, usamos conjuntos de dados tipados para implementar a camada de acesso de dado e os objetos comerciais de nossa arquitetura em camadas. Conforme discutido no [primeiro tutorial](../introduction/creating-a-data-access-layer-cs.md), o conjunto de dados s de dataset tipado serve como repositórios de data, enquanto os TableAdapters atuam como wrappers para se comunicarem com o banco de dados para recuperar e modificar o dado subjacente. Os TableAdapters encapsulam a complexidade envolvida no trabalho com o banco de dados e nos poupa de ter que escrever código para se conectar ao banco de dados, emitir um comando ou preencher os resultados em uma DataTable.

No entanto, há ocasiões em que precisamos burrowr as profundidades do TableAdapter e escrever o código que funciona diretamente com os objetos ADO.NET. Nas [modificações de banco de dados de disposição em um](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md) tutorial de transação, por exemplo, adicionamos métodos ao TableAdapter para iniciar, confirmar e reverter transações de ADO.net. Esses métodos usaram um objeto `SqlTransaction` interno criado manualmente que foi atribuído aos objetos de `SqlCommand` do TableAdapter.

Neste tutorial, examinaremos como acessar as configurações de nível de comando e de conexão de banco de dados no TableAdapter. Em particular, adicionaremos funcionalidade à `ProductsTableAdapter` que permite o acesso à cadeia de conexão subjacente e às configurações de tempo limite do comando.

## <a name="working-with-data-using-adonet"></a>Trabalhando com dados usando ADO.NET

O Microsoft .NET Framework contém uma infinidade de classes projetadas especificamente para trabalhar com dados. Essas classes, encontradas no [namespace`System.Data`](https://msdn.microsoft.com/library/system.data.aspx), são chamadas de classes *ADO.net* . Algumas das classes sob a proteção ADO.NET estão vinculadas a um *provedor de dados*específico. Você pode considerar um provedor de dados como um canal de comunicação que permite que as informações fluam entre as classes ADO.NET e o armazenamento de dados subjacente. Há provedores generalizados, como OleDb e ODBC, bem como provedores que são especialmente projetados para um sistema de banco de dados específico. Por exemplo, embora seja possível se conectar a um banco de dados de Microsoft SQL Server usando o provedor OleDb, o provedor SqlClient é muito mais eficiente, pois foi projetado e otimizado especificamente para SQL Server.

Ao acessar dados programaticamente, o padrão a seguir é normalmente usado:

- Estabeleça uma conexão com o banco de dados.
- Emita um comando.
- Para `SELECT` consultas, trabalhe com os registros resultantes.

Há classes ADO.NET separadas para executar cada uma dessas etapas. Para se conectar a um banco de dados usando o provedor SqlClient, por exemplo, use a [classe`SqlConnection`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(VS.80).aspx). Para emitir um comando `INSERT`, `UPDATE`, `DELETE`ou `SELECT` para o banco de dados, use a [classe`SqlCommand`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.aspx).

Exceto para as [modificações de banco de dados em um](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md) tutorial de transação, não tivemos que escrever nenhum código ADO.net de baixo nível, por si só, porque o código gerado automaticamente pelo TableAdapters inclui a funcionalidade necessária para se conectar ao banco de dados, emitir comandos, recuperar dados e preencher esses dados em tabelas. No entanto, pode haver ocasiões em que precisamos personalizar essas configurações de nível baixo. Nas próximas etapas, examinaremos como explorar os objetos ADO.NET usados internamente pelos TableAdapters.

## <a name="step-1-examining-with-the-connection-property"></a>Etapa 1: examinando com a propriedade Connection

Cada classe TableAdapter tem uma propriedade `Connection` que especifica informações de conexão de banco de dados. Esse tipo de dados s de propriedade e `ConnectionString` valor são determinados pelas seleções feitas no assistente de configuração do TableAdapter. Lembre-se de que quando adicionamos primeiro um TableAdapter a um DataSet tipado, esse assistente nos solicita a fonte do banco de dados (veja a Figura 1). A lista suspensa nessa primeira etapa inclui os bancos de dados especificados no arquivo de configuração, bem como quaisquer outros bancos de dado nas conexões de Gerenciador de Servidores s. Se o banco de dados que queremos usar não existir na lista suspensa, uma nova conexão de banco de dados poderá ser especificada clicando no botão Nova conexão e fornecendo as informações de conexão necessárias.

[![a primeira etapa do assistente de configuração do TableAdapter](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image2.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image1.png)

**Figura 1**: a primeira etapa do assistente de configuração do TableAdapter ([clique para exibir a imagem em tamanho normal](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image3.png))

Vamos reservar um momento para inspecionar o código para a propriedade TableAdapter s `Connection`. Conforme observado no tutorial [criando uma camada de acesso a dados](../introduction/creating-a-data-access-layer-cs.md) , podemos exibir o código TableAdapter gerado automaticamente acessando a janela modo de exibição de classe, fazendo uma busca detalhada na classe apropriada e clicando duas vezes no nome do membro.

Navegue até a janela de Modo de Exibição de Classe acessando o menu exibir e escolhendo Modo de Exibição de Classe (ou digitando Ctrl + Shift + C). Na parte superior da janela de Modo de Exibição de Classe, faça uma busca detalhada no namespace `NorthwindTableAdapters` e selecione a classe `ProductsTableAdapter`. Isso exibirá os membros do `ProductsTableAdapter` na metade inferior da Modo de Exibição de Classe, como mostra a Figura 2. Clique duas vezes na propriedade `Connection` para ver seu código.

![Clique duas vezes na Propriedade Connection no Modo de Exibição de Classe para exibir seu código gerado automaticamente](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image4.png)

**Figura 2**: clique duas vezes na Propriedade Connection no modo de exibição de classe para exibir seu código gerado automaticamente

A propriedade TableAdapter s `Connection` e outros códigos relacionados à conexão a seguir:

[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample1.cs)]

Quando a classe do TableAdapter é instanciada, a variável de membro `_connection` é igual a `null`. Quando a propriedade `Connection` é acessada, ela verifica primeiro se a variável de membro `_connection` foi instanciada. Se não tiver, o método `InitConnection` será invocado, que instanciará `_connection` e definirá sua propriedade `ConnectionString` como o valor da cadeia de conexão especificado na primeira etapa do assistente de configuração do TableAdapter.

A propriedade `Connection` também pode ser atribuída a um objeto `SqlConnection`. Isso associa o novo objeto `SqlConnection` a cada um dos objetos `SqlCommand` do TableAdapter.

## <a name="step-2-exposing-connection-level-settings"></a>Etapa 2: expondo as configurações de nível de conexão

As informações de conexão devem permanecer encapsuladas no TableAdapter e não podem ser acessadas por outras camadas na arquitetura do aplicativo. No entanto, pode haver cenários em que as informações de nível de conexão do TableAdapter s precisam ser acessíveis ou personalizáveis para uma consulta, usuário ou página ASP.NET.

Deixe que os s estendam o `ProductsTableAdapter` no conjunto de `Northwind` para incluir uma propriedade `ConnectionString` que possa ser usada pela camada de lógica de negócios para ler ou alterar a cadeia de conexão usada pelo TableAdapter.

> [!NOTE]
> Uma *cadeia de conexão* é uma cadeia de caracteres que especifica informações de conexão de banco de dados, como o provedor a ser usado, o local do banco de dados, as credenciais de autenticação e outras configurações relacionadas ao banco de dados. Para obter uma lista dos padrões de cadeia de conexão usados por uma variedade de repositórios de dados e provedores, consulte [connectionStrings.com](http://www.connectionstrings.com/).

Conforme discutido no tutorial [criando uma camada de acesso a dados](../introduction/creating-a-data-access-layer-cs.md) , as classes geradas automaticamente do DataSet s tipadas podem ser estendidas por meio do uso de classes parciais. Primeiro, crie uma nova subpasta no projeto denominada `ConnectionAndCommandSettings` abaixo da pasta `~/App_Code/DAL`.

![Adicione uma subpasta chamada ConnectionAndCommandSettings](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image5.png)

**Figura 3**: adicionar uma subpasta chamada `ConnectionAndCommandSettings`

Adicione um novo arquivo de classe chamado `ProductsTableAdapter.ConnectionAndCommandSettings.cs` e insira o código a seguir:

[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample2.cs)]

Essa classe parcial adiciona uma propriedade `public` chamada `ConnectionString` à classe `ProductsTableAdapter` que permite que qualquer camada leia ou atualize a cadeia de conexão para a conexão subjacente do TableAdapter s.

Com essa classe parcial criada (e salva), abra a classe `ProductsBLL`. Vá para um dos métodos existentes e digite `Adapter` e, em seguida, pressione a tecla period para abrir o IntelliSense. Você deve ver a nova propriedade `ConnectionString` disponível no IntelliSense, o que significa que você pode ler ou ajustar programaticamente esse valor da BLL.

## <a name="exposing-the-entire-connection-object"></a>Expondo todo o objeto de conexão

Essa classe parcial expõe apenas uma propriedade do objeto de conexão subjacente: `ConnectionString`. Se desejar disponibilizar todo o objeto de conexão além dos limites do TableAdapter, você poderá alterar o nível de proteção da propriedade de `Connection` s. O código gerado automaticamente que examinamos na etapa 1 mostrou que a propriedade TableAdapter s `Connection` está marcada como `internal`, o que significa que ele só pode ser acessado por classes no mesmo assembly. No entanto, isso pode ser alterado por meio da propriedade TableAdapter s `ConnectionModifier`.

Abra o conjunto de `Northwind` DataSet, clique na `ProductsTableAdapter` no designer e navegue até a janela Propriedades. Lá, você verá o `ConnectionModifier` definido com o valor padrão `Assembly`. Para tornar a propriedade `Connection` disponível fora do assembly do DataSet tipado, altere a propriedade `ConnectionModifier` para `Public`.

[![o nível de acessibilidade de s da propriedade de conexão pode ser configurado por meio da propriedade ConnectionModifier](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image7.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image6.png)

**Figura 4**: o nível de acessibilidade da propriedade `Connection` s pode ser configurado por meio da propriedade `ConnectionModifier` ([clique para exibir a imagem em tamanho normal](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image8.png))

Salve o conjunto de e retorne à classe `ProductsBLL`. Como antes, vá para um dos métodos existentes e digite `Adapter` e, em seguida, pressione a tecla period para abrir o IntelliSense. A lista deve incluir uma propriedade `Connection`, o que significa que agora você pode ler ou atribuir programaticamente qualquer configuração de nível de conexão da BLL.

## <a name="step-3-examining-the-command-related-properties"></a>Etapa 3: examinando as propriedades relacionadas a comandos

Um TableAdapter consiste em uma consulta principal que, por padrão, tem instruções geradas automaticamente `INSERT`, `UPDATE`e `DELETE`. As instruções s `INSERT`, `UPDATE`e `DELETE` da consulta principal são implementadas no código do TableAdapter s como um objeto de adaptador de dados ADO.NET por meio da propriedade `Adapter`. Assim como com sua propriedade `Connection`, o tipo de dados da propriedade `Adapter` s é determinado pelo provedor de dados usado. Como esses tutoriais usam o provedor SqlClient, a propriedade `Adapter` é do tipo [`SqlDataAdapter`](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter(VS.80).aspx).

A propriedade TableAdapter s `Adapter` tem três propriedades do tipo `SqlCommand` que ele usa para emitir as instruções `INSERT`, `UPDATE`e `DELETE`:

- `InsertCommand`
- `UpdateCommand`
- `DeleteCommand`

Um objeto `SqlCommand` é responsável por enviar uma consulta específica para o banco de dados e tem propriedades como: [`CommandText`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtext.aspx), que contém a instrução SQL ad hoc ou o procedimento armazenado a ser executado; e [`Parameters`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.parameters.aspx), que é uma coleção de objetos `SqlParameter`. Como vimos de volta no tutorial [criando uma camada de acesso a dados](../introduction/creating-a-data-access-layer-cs.md) , esses objetos de comando podem ser personalizados por meio da janela Propriedades.

Além de sua consulta principal, o TableAdapter pode incluir um número variável de métodos que, quando invocados, expedem um comando especificado para o banco de dados. O objeto de comando de consulta principal e os objetos de comando para todos os métodos adicionais são armazenados na propriedade TableAdapter s `CommandCollection`.

Vamos reservar um momento para examinar o código gerado pelo `ProductsTableAdapter` no conjunto de `Northwind` para essas duas propriedades e suas variáveis de membro e métodos auxiliares de suporte:

[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample3.cs)]

O código para as propriedades `Adapter` e `CommandCollection` imita de forma muito semelhante a da propriedade `Connection`. Há variáveis de membro que contêm os objetos usados pelas propriedades. As propriedades `get` acessadores começam verificando se a variável de membro correspondente está `null`. Nesse caso, um método de inicialização é chamado, que cria uma instância da variável de membro e atribui as propriedades principais relacionadas ao comando.

## <a name="step-4-exposing-command-level-settings"></a>Etapa 4: expondo as configurações de nível de comando

O ideal é que as informações de nível de comando permaneçam encapsuladas na camada de acesso a dados. Caso essas informações sejam necessárias em outras camadas da arquitetura, no entanto, elas podem ser expostas por meio de uma classe parcial, assim como as configurações de nível de conexão.

Como o TableAdapter tem apenas uma única propriedade `Connection`, o código para expor as configurações de nível de conexão é razoavelmente simples. As coisas são um pouco mais complicadas ao modificar as configurações de nível de comando porque o TableAdapter pode ter vários objetos de comando, um `InsertCommand`, `UpdateCommand`e `DeleteCommand`, juntamente com um número variável de objetos Command na propriedade `CommandCollection`. Ao atualizar as configurações de nível de comando, essas configurações deverão ser propagadas para todos os objetos de comando.

Por exemplo, imagine que havia determinadas consultas no TableAdapter que levaram muito tempo para serem executadas. Ao usar o TableAdapter para executar uma dessas consultas, talvez queiramos aumentar o objeto de comando s [`CommandTimeout` Propriedade](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtimeout.aspx). Essa propriedade especifica o número de segundos a aguardar até que o comando seja executado e o padrão é 30.

Para permitir que a propriedade `CommandTimeout` seja ajustada pela BLL, adicione o seguinte método `public` ao `ProductsDataTable` usando o arquivo de classe parcial criado na etapa 2 (`ProductsTableAdapter.ConnectionAndCommandSettings.cs`):

[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample4.cs)]

Esse método pode ser invocado a partir da camada de apresentação ou BLL para definir o tempo limite do comando para todos os problemas de comandos por essa instância do TableAdapter.

> [!NOTE]
> As propriedades `Adapter` e `CommandCollection` são marcadas como `private`, o que significa que elas só podem ser acessadas a partir do código no TableAdapter. Ao contrário da propriedade `Connection`, esses modificadores de acesso não são configuráveis. Portanto, se você precisar expor propriedades de nível de comando para outras camadas na arquitetura, deverá usar a abordagem de classe parcial discutida acima para fornecer um método ou propriedade de `public` que leia ou grave para os objetos de comando `private`.

## <a name="summary"></a>Resumo

Os TableAdapters em um DataSet tipado servem para encapsular detalhes de acesso a dados e a complexidade. Usando os TableAdapters, não precisamos nos preocupar em escrever o código ADO.NET para se conectar ao banco de dados, emitir um comando ou preencher os resultados em uma DataTable. Tudo é manipulado automaticamente para nós.

No entanto, pode haver ocasiões em que precisamos personalizar as especificidades ADO.NET de baixo nível, como alterar a cadeia de conexão ou os valores padrão de conexão ou tempo limite do comando. O TableAdapter tem propriedades `Connection`, `Adapter`e `CommandCollection` geradas automaticamente, mas elas são `internal` ou `private`, por padrão. Essas informações internas podem ser expostas estendendo o TableAdapter usando classes parciais para incluir `public` métodos ou propriedades. Como alternativa, o modificador de acesso de Propriedade do TableAdapter s `Connection` pode ser configurado por meio da propriedade TableAdapter s `ConnectionModifier`.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores potenciais para este tutorial foram Burnadette Leigh, S Ren Jacob Lauritsen, Teresa Murphy e Hilton Geisenow. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](working-with-computed-columns-cs.md)
> [Próximo](protecting-connection-strings-and-other-configuration-information-cs.md)
