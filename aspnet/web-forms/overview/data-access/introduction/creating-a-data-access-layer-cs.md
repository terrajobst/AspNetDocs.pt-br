---
uid: web-forms/overview/data-access/introduction/creating-a-data-access-layer-cs
title: Criando uma camada de acesso aC#dados () | Microsoft Docs
author: rick-anderson
description: Neste tutorial, vamos começar desde o início e criar a camada de acesso a dados (DAL), usando os datasets tipados para acessar as informações em um banco de dado.
ms.author: riande
ms.date: 04/05/2010
ms.assetid: cfe2a6a0-1e56-4dc8-9537-c8ec76ba96a4
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-data-access-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: 5aaf97dc8448dcb7b94ef2e4e23f34fd37ac4426
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78605038"
---
# <a name="creating-a-data-access-layer-c"></a>Criação de uma Camada de acesso a dados (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar PDF](creating-a-data-access-layer-cs/_static/datatutorial01cs1.pdf)

> Neste tutorial, vamos começar desde o início e criar a camada de acesso a dados (DAL), usando os datasets tipados para acessar as informações em um banco de dado.

## <a name="introduction"></a>Introdução

Como desenvolvedores da Web, nossas vidas giram em relação ao trabalho com dados. Nós criamos os bancos de dados para armazenar o dado, o código para recuperá-lo e modificá-lo e as páginas da Web para coletar e resumir. Este é o primeiro tutorial de uma série extensa que explorará técnicas para implementar esses padrões comuns no ASP.NET 2,0. Começaremos com a criação de uma [arquitetura de software](http://en.wikipedia.org/wiki/Software_architecture) composta de uma Dal (camada de acesso a dados) usando datasets tipados, uma camada de lógica de negócios (BLL) que impõe regras de negócios personalizadas e uma camada de apresentação composta de páginas ASP.NET que compartilham um layout de página comum. Depois que esse aterramento de back-end tiver sido disposto, passaremos para o relatório, mostrando como exibir, resumir, coletar e validar dados de um aplicativo Web. Esses tutoriais são voltados para serem concisos e fornecem instruções passo a passo com muitas capturas de tela para orientá-lo no processo visualmente. Cada tutorial está disponível nas C# versões e Visual Basic e inclui um download do código completo usado. (Esse primeiro tutorial é bastante longo, mas o restante é apresentado em partes muito mais fácils.)

Para esses tutoriais, usaremos uma versão de edição Microsoft SQL Server Express 2005 do banco de dados Northwind colocado no diretório do **aplicativo\_data** . Além do arquivo de banco de dados, a pasta **\_data do aplicativo** também contém os scripts SQL para criar o banco, caso você queira usar uma versão de banco de dado diferente. Esses scripts também podem ser [baixados diretamente da Microsoft](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en), se você preferir. Se você usar uma versão diferente SQL Server do banco de dados Northwind, será necessário atualizar a configuração **NORTHWNDConnectionString** no arquivo **Web. config** do aplicativo. O aplicativo Web foi criado usando o Visual Studio 2005 Professional Edition como um projeto de site baseado em sistema de arquivos. No entanto, todos os tutoriais funcionarão igualmente bem com a versão gratuita do Visual Studio 2005, o [Visual Web Developer](https://msdn.microsoft.com/vstudio/express/vwd/).  
  
Neste tutorial, vamos começar do começo e criar a camada de acesso a dados (DAL), seguida criando a camada de lógica de negócios (BLL) no segundo tutorial e trabalhando no layout da página e na navegação na terceira. Os tutoriais após o terceiro se basearão na base do disposto nos três primeiros. Temos muito a abordar neste primeiro tutorial, portanto, vamos disparar o Visual Studio e começar!

## <a name="step-1-creating-a-web-project-and-connecting-to-the-database"></a>Etapa 1: Criando um projeto Web e conectando-se ao banco de dados

Antes que possamos criar nossa DAL (camada de acesso a dados), primeiro precisamos criar um site e configurar nosso banco de dado. Comece criando um novo site do ASP.NET baseado em sistema de arquivos. Para fazer isso, vá para o menu arquivo e escolha novo site, exibindo a caixa de diálogo novo site da Web. Escolha o modelo de site da Web ASP.NET, defina a lista suspensa local para sistema de arquivos, escolha uma pasta para colocar o site e defina o idioma como C#.

[![criar um novo site baseado no sistema de arquivos](creating-a-data-access-layer-cs/_static/image2.png)](creating-a-data-access-layer-cs/_static/image1.png)

**Figura 1**: criar um novo site baseado no sistema de arquivos ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image3.png))

Isso criará um novo site com uma página **Default. aspx** ASP.net e uma pasta de **dados de\_de aplicativo** .

Com o site criado, a próxima etapa é adicionar uma referência ao banco de dados no Gerenciador de Servidores do Visual Studio. Ao adicionar um banco de dados ao Gerenciador de Servidores você pode adicionar tabelas, procedimentos armazenados, exibições e assim por diante no Visual Studio. Você também pode exibir dados da tabela ou criar suas próprias consultas manualmente ou graficamente por meio do Construtor de Consultas. Além disso, quando criamos os conjuntos de dados tipados para a DAL, precisaremos apontar para o Visual Studio para o Database a partir do qual os datasets tipados devem ser construídos. Embora possamos fornecer essas informações de conexão nesse momento determinado, o Visual Studio preenche automaticamente uma lista suspensa dos bancos de dados já registrados no Gerenciador de Servidores.

As etapas para adicionar o banco de dados Northwind ao Gerenciador de Servidores dependem de se você deseja usar o banco de dados SQL Server 2005 Express Edition na pasta **\_data do aplicativo** ou se tem uma instalação do servidor de banco de dado Microsoft SQL Server 2000 ou 2005 que você deseja usar em vez disso.

## <a name="using-a-database-in-the-app_data-folder"></a>Usando um banco de dados na pasta Data\_do aplicativo

Se você não tiver um servidor de banco de dados SQL Server 2000 ou 2005 para se conectar ou se simplesmente quiser evitar precisar adicionar o banco de dados a um servidor de banco de dados, poderá usar a versão SQL Server 2005 Express Edition do banco de dados Northwind que está localizado na pasta do **aplicativo\_data** do site baixado (**Northwnd. MDF**).

Um banco de dados colocado na pasta **\_data do aplicativo** é adicionado automaticamente à Gerenciador de servidores. Supondo que você tenha SQL Server 2005 Express Edition instalado em seu computador, você deverá ver um nó chamado NORTHWND. MDF no Gerenciador de Servidores, que você pode expandir e explorar suas tabelas, exibições, procedimento armazenado e assim por diante (consulte a Figura 2).

A pasta de **dados de\_de aplicativos** também pode manter os arquivos **. mdb** do Microsoft Access, que, como suas contrapartes SQL Server, são adicionados automaticamente à Gerenciador de servidores. Se você não quiser usar qualquer uma das opções de SQL Server, sempre será possível [baixar uma versão do Microsoft Access do arquivo de banco de dados Northwind](https://www.microsoft.com/downloads/details.aspx?FamilyID=C6661372-8DBE-422B-8676-C632D66C529C&amp;displaylang=EN) e soltar no **aplicativo\_data** Directory. No entanto, tenha em mente que os bancos de dados do Access não são tão ricos em recursos quanto SQL Server e não são projetados para serem usados em cenários de site. Além disso, alguns dos mais de 35 tutoriais usarão determinados recursos de nível de banco de dados que não têm suporte do Access.

## <a name="connecting-to-the-database-in-a-microsoft-sql-server-2000-or-2005-database-server"></a>Conectando-se ao banco de dados em um servidor de banco de dados Microsoft SQL Server 2000 ou 2005

Como alternativa, você pode se conectar a um banco de dados Northwind instalado em um servidor de banco de dados. Se o servidor de banco de dados ainda não tiver o banco de dados Northwind instalado, primeiro você deverá adicioná-lo ao servidor de banco de dados executando o script de instalação incluído no download deste tutorial ou [baixando a versão SQL Server 2000 do script de instalação e do Northwind](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en) diretamente do site da Microsoft.

Depois de instalar o banco de dados, vá para o Gerenciador de Servidores no Visual Studio, clique com o botão direito do mouse no nó conexões de dados e escolha Adicionar conexão. Se você não vir o Gerenciador de Servidores vá para a exibição/Gerenciador de Servidores ou pressione CTRL + ALT + S. Isso abrirá a caixa de diálogo Adicionar conexão, onde você pode especificar o servidor ao qual se conectar, as informações de autenticação e o nome do banco de dados. Depois de ter configurado com êxito as informações de conexão de banco de dados e clicado no botão OK, o banco de dados será adicionado como um nó abaixo do nó Data Connections. Você pode expandir o nó do banco de dados para explorar suas tabelas, exibições, procedimentos armazenados e assim por diante.

![Adicionar uma conexão ao banco de dados Northwind do servidor de banco de dados](creating-a-data-access-layer-cs/_static/image4.png)

**Figura 2**: adicionar uma conexão ao banco de dados Northwind do servidor de banco de dados

## <a name="step-2-creating-the-data-access-layer"></a>Etapa 2: criando a camada de acesso a dados

Ao trabalhar com dados uma opção é inserir a lógica específica de dados diretamente na camada de apresentação (em um aplicativo Web, as páginas ASP.NET compõem a camada de apresentação). Isso pode assumir a forma de escrever o código ADO.NET na parte de código da página ASP.NET ou usando o controle SqlDataSource da parte de marcação. Em ambos os casos, essa abordagem acopla rigidamente a lógica de acesso a dados com a camada de apresentação. No entanto, a abordagem recomendada é separar a lógica de acesso de dados da camada de apresentação. Essa camada separada é chamada de camada de acesso a dados, DAL para curto e normalmente é implementada como um projeto de biblioteca de classes separado. Os benefícios dessa arquitetura em camadas são bem documentados (consulte a seção "leituras adicionais" no final deste tutorial para obter informações sobre essas vantagens) e é a abordagem que usaremos nesta série.

Todo o código que é específico da fonte de dados subjacente, como a criação de uma conexão com o Database, a emissão de comandos **Select**, **Insert**, **Update**e **delete** , e assim por diante, deve estar localizado na Dal. A camada de apresentação não deve conter nenhuma referência a esse código de acesso a dados, mas, em vez disso, deve fazer chamadas para a DAL para todas as solicitações de dados. Camadas de acesso a dados normalmente contêm métodos para acessar os dados subjacentes do banco de dados. O banco de dados Northwind, por exemplo, tem tabelas de **produtos** e **categorias** que registram os produtos para venda e as categorias às quais eles pertencem. Em nossa DAL, teremos métodos como:

- **GetCategories (),** que retornará informações sobre todas as categorias
- **GetProducts ()** , que irá retornar informações sobre todos os produtos
- **GetProductsByCategoryID (*CategoryID*)** , que retornará todos os produtos que pertencem a uma categoria especificada
- **GetProductByProductID (*ProductID*)** , que retornará informações sobre um produto específico

Esses métodos, quando invocados, se conectarão ao banco de dados, emitirão a consulta apropriada e retornarão os resultados. A forma como retornamos esses resultados é importante. Esses métodos poderiam simplesmente retornar um conjunto de dados ou DataReader preenchidos pela consulta de banco, mas idealmente esses resultados devem ser retornados usando *objetos fortemente tipados*. Um objeto fortemente tipado é aquele cujo esquema é definido de forma rígida no tempo de compilação, enquanto o oposto, um objeto com rigidez de tipos, é aquele cujo esquema não é conhecido até o tempo de execução.

Por exemplo, o DataReader e o conjunto de dados (por padrão) são objetos com tipo inflexível, pois seu esquema é definido pelas colunas retornadas pela consulta de banco de dados usada para preenchê-los. Para acessar uma coluna específica de uma DataTable com tipo inflexível, precisamos usar sintaxe como:  <strong><em>DataTable</em>. Rows [<em>index</em>] ["<em>ColumnName</em>"]</strong>. A tipagem flexível da DataTable neste exemplo é mostrada pelo fato de que precisamos acessar o nome da coluna usando uma cadeia de caracteres ou um índice ordinal. Uma DataTable fortemente tipada, por outro lado, terá cada uma de suas colunas implementadas como propriedades, resultando em código semelhante a:  <strong><em>DataTable</em>. Linhas [<em>índice</em>]. *ColumnName</strong>* .

Para retornar objetos fortemente tipados, os desenvolvedores podem criar seus próprios objetos comerciais personalizados ou usar datasets tipados. Um objeto comercial é implementado pelo desenvolvedor como uma classe cujas propriedades normalmente refletem as colunas da tabela de banco de dados subjacente que o objeto comercial representa. Um DataSet tipado é uma classe gerada para você pelo Visual Studio com base em um esquema de banco de dados e cujos membros são fortemente tipados de acordo com esse esquema. O próprio DataSet tipado consiste em classes que estendem as classes DataSet, DataTable e DataRow do ADO.NET. Além de DataTables fortemente tipadas, os conjuntos de dados tipados agora também incluem TableAdapters, que são classes com métodos para popular as DataTables do conjunto de dados e propagar modificações dentro das DataTables de volta para o Database.

> [!NOTE]
> Para obter mais informações sobre as vantagens e desvantagens de usar conjuntos de dados tipados em comparação com objetos comerciais personalizados, consulte [criando componentes da camada e passando dados por meio de camadas](https://msdn.microsoft.com/library/ms978496.aspx).

Usaremos conjuntos de linhas fortemente tipados para a arquitetura desses tutoriais. A Figura 3 ilustra o fluxo de trabalho entre as diferentes camadas de um aplicativo que usa datasets tipados.

[![todo o código de acesso a dados é relegado à DAL](creating-a-data-access-layer-cs/_static/image6.png)](creating-a-data-access-layer-cs/_static/image5.png)

**Figura 3**: todo o código de acesso a dados é relegado à Dal ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image7.png))

## <a name="creating-a-typed-dataset-and-table-adapter"></a>Criando um DataSet tipado e um adaptador de tabela

Para começar a criar nossa DAL, começamos adicionando um DataSet tipado ao nosso projeto. Para fazer isso, clique com o botão direito do mouse no nó do projeto na Gerenciador de Soluções e escolha Adicionar um novo item. Selecione a opção DataSet na lista de modelos e nomeie-o **Northwind. xsd**.

[![optar por adicionar um novo conjunto de uma ao seu projeto](creating-a-data-access-layer-cs/_static/image9.png)](creating-a-data-access-layer-cs/_static/image8.png)

**Figura 4**: escolha Adicionar um novo conjunto de novos conjuntos de um projeto ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image10.png))

Depois de clicar em Adicionar, quando for solicitado a adicionar o conjunto de aplicativos à pasta **\_de código do aplicativo** , escolha Sim. O designer para o DataSet tipado será exibido, e o assistente de configuração do TableAdapter será iniciado, permitindo que você adicione seu primeiro TableAdapter ao DataSet tipado.

Um DataSet tipado serve como uma coleção de dados fortemente tipada; Ele é composto de instâncias de DataTable fortemente tipadas, cada uma delas, por sua vez, composta de instâncias de DataRow fortemente tipadas. Criaremos uma DataTable fortemente tipada para cada uma das tabelas de banco de dados subjacentes com que precisamos trabalhar nesta série de tutoriais. Vamos começar com a criação de uma DataTable para a tabela **Products** .

Tenha em mente que DataTables fortemente tipadas não incluem informações sobre como acessar dados de sua tabela de banco de dados subjacente. Para recuperar os dados para preencher a DataTable, usamos uma classe TableAdapter, que funciona como nossa camada de acesso a dados. Para a DataTable **Products** , o TableAdapter conterá os métodos **GetProducts ()** , **GetProductByCategoryID (*CategoryID*)** e assim por diante, que invocaremos da camada de apresentação. A função da DataTable é servir como os objetos fortemente tipados usados para passar dados entre as camadas.

O assistente de configuração do TableAdapter começa solicitando que você selecione o banco de dados com o qual trabalhar. A lista suspensa mostra esses bancos de dados no Gerenciador de Servidores. Se você não adicionou o banco de dados Northwind ao Gerenciador de Servidores, você pode clicar no botão Nova conexão neste momento para fazer isso.

[![escolher o banco de dados Northwind na lista suspensa](creating-a-data-access-layer-cs/_static/image12.png)](creating-a-data-access-layer-cs/_static/image11.png)

**Figura 5**: escolha o banco de dados Northwind na lista suspensa ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image13.png))

Depois de selecionar o banco de dados e clicar em avançar, você será perguntado se deseja salvar a cadeia de conexão no arquivo **Web. config** . Ao salvar a cadeia de conexão, você evitará tê-las embutidas em código nas classes do TableAdapter, o que simplificará as coisas se as informações da cadeia de conexão forem alteradas no futuro. Se você optar por salvar a cadeia de conexão no arquivo de configuração, ela será colocada na seção **&lt;connectionstrings&gt;** , que pode ser [opcionalmente criptografada](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx) para segurança aprimorada ou modificada posteriormente por meio da nova página de propriedades ASP.NET 2,0 dentro da ferramenta de administração de GUI do IIS, que é mais ideal para os administradores.

[![salvar a cadeia de conexão em Web. config](creating-a-data-access-layer-cs/_static/image15.png)](creating-a-data-access-layer-cs/_static/image14.png)

**Figura 6**: salvar a cadeia de conexão em **Web. config** ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image16.png))

Em seguida, precisamos definir o esquema para a primeira DataTable fortemente tipada e fornecer o primeiro método para que nosso TableAdapter use ao preencher o conjunto de um DataSet fortemente tipado. Essas duas etapas são realizadas simultaneamente criando uma consulta que retorna as colunas da tabela que queremos que sejam refletidas em nossa DataTable. No final do assistente, daremos um nome de método a essa consulta. Depois que isso for feito, esse método poderá ser invocado de nossa camada de apresentação. O método executará a consulta definida e preencherá uma DataTable fortemente tipada.

Para começar a definir a consulta SQL, primeiro devemos indicar como queremos que o TableAdapter emita a consulta. Podemos usar uma instrução SQL ad hoc, criar um novo procedimento armazenado ou usar um procedimento armazenado existente. Para esses tutoriais, usaremos instruções SQL ad hoc. Consulte o artigo do [Brian Noyes](http://briannoyes.net/), [crie uma camada de acesso a dados com o Visual Studio 2005 DataSet Designer](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner) para obter um exemplo de como usar procedimentos armazenados.

[![consultar os dados usando uma instrução SQL ad hoc](creating-a-data-access-layer-cs/_static/image18.png)](creating-a-data-access-layer-cs/_static/image17.png)

**Figura 7**: consultar os dados usando uma instrução SQL ad hoc ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image19.png))

Neste ponto, podemos digitar a consulta SQL manualmente. Ao criar o primeiro método no TableAdapter, normalmente você desejará que a consulta retorne as colunas que precisam ser expressas na DataTable correspondente. Podemos fazer isso criando uma consulta que retorna todas as colunas e todas as linhas da tabela **Products** :

[![inserir a consulta SQL na caixa de texto](creating-a-data-access-layer-cs/_static/image21.png)](creating-a-data-access-layer-cs/_static/image20.png)

**Figura 8**: Insira a consulta SQL na caixa de texto ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image22.png))

Como alternativa, use a Construtor de Consultas e construa graficamente a consulta, como mostra a Figura 9.

[![criar a consulta graficamente, por meio do editor de consultas](creating-a-data-access-layer-cs/_static/image24.png)](creating-a-data-access-layer-cs/_static/image23.png)

**Figura 9**: criar a consulta graficamente, por meio do editor de consultas ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image25.png))

Depois de criar a consulta, mas antes de passar para a próxima tela, clique no botão Opções avançadas. Em projetos de site, "gerar instruções INSERT, Update e Delete" é a única opção avançada selecionada por padrão; Se você executar esse assistente de uma biblioteca de classes ou um projeto do Windows, a opção "usar simultaneidade otimista" também será selecionada. Deixe a opção "usar simultaneidade otimista" desmarcada por enquanto. Examinaremos a simultaneidade otimista em Tutoriais futuros.

[![selecionar apenas a opção gerar instruções INSERT, Update e Delete](creating-a-data-access-layer-cs/_static/image27.png)](creating-a-data-access-layer-cs/_static/image26.png)

**Figura 10**: Selecione apenas a opção gerar instruções INSERT, Update e Delete ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image28.png))

Depois de verificar as opções avançadas, clique em avançar para prosseguir para a tela final. Aqui, é solicitado que você Selecione quais métodos adicionar ao TableAdapter. Há dois padrões para popular dados:

- **Preencher uma DataTable** com essa abordagem é criado um método que usa uma DataTable como um parâmetro e o popula com base nos resultados da consulta. A classe ADO.NET DataAdapter, por exemplo, implementa esse padrão com seu método **Fill ()** .
- **Retornar uma DataTable** com essa abordagem o método cria e preenche a DataTable para você e a retorna como o valor de retorno dos métodos.

Você pode fazer com que o TableAdapter implemente um ou ambos os padrões. Você também pode renomear os métodos fornecidos aqui. Vamos deixar ambas as caixas de seleção marcadas, mesmo que use apenas o último padrão em todos esses tutoriais. Além disso, vamos renomear o método **GetData** do contrário genérico para **GetProducts**.

Se marcada, a caixa de seleção final, "GenerateDBDirectMethods", cria os métodos **Insert ()** , **Update ()** e **Delete ()** para o TableAdapter. Se você deixar essa opção desmarcada, todas as atualizações precisarão ser feitas por meio do método único **Update ()** do TableAdapter, que usa o conjunto de registros, uma DataTable, uma única DataRow ou uma matriz de DataRows. (Se você tiver desmarcado a opção "gerar instruções INSERT, Update e Delete" nas propriedades avançadas da Figura 9, essa configuração da caixa de seleção não terá nenhum efeito.) Vamos deixar essa caixa de seleção selecionada.

[![alterar o nome do método de GetData para GetProducts](creating-a-data-access-layer-cs/_static/image30.png)](creating-a-data-access-layer-cs/_static/image29.png)

**Figura 11**: alterar o nome do método de **GetData** para **GetProducts** ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image31.png))

Conclua o assistente clicando em concluir. Depois que o assistente é fechado, retornamos para o designer de conjunto de DataSet que mostra a DataTable que acabamos de criar. Você pode ver a lista de colunas na DataTable **Products** (**ProductID**, **ProductName**e assim por diante), bem como os métodos de **ProductsTableAdapter** (**Fill ()** e **GetProducts ()** ).

[![os produtos DataTable e ProductsTableAdapter foram adicionados ao DataSet tipado](creating-a-data-access-layer-cs/_static/image33.png)](creating-a-data-access-layer-cs/_static/image32.png)

**Figura 12**: os **produtos** DataTable e **ProductsTableAdapter** foram adicionados ao dataset tipado ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image34.png))

Neste ponto, temos um DataSet tipado com uma única DataTable (**Northwind. Products**) e uma classe DataAdapter fortemente tipada (**NorthwindTableAdapters. ProductsTableAdapter**) com um método **GetProducts ()** . Esses objetos podem ser usados para acessar uma lista de todos os produtos de código como:

[!code-html[Main](creating-a-data-access-layer-cs/samples/sample1.html)]

Esse código não exigia a gravação de um pouco de código específico de acesso a dados. Não precisamos instanciar nenhuma classe ADO.NET, não precisamos fazer referência a cadeias de conexão, consultas SQL ou procedimentos armazenados. Em vez disso, o TableAdapter fornece o código de acesso a dados de baixo nível para nós.

Cada objeto usado neste exemplo também é fortemente tipado, permitindo que o Visual Studio forneça verificação de tipo IntelliSense e de tempo de compilação. E o melhor de todas as tabelas de dados retornadas pelo TableAdapter pode ser associado aos controles da Web ASP.NET Data, como GridView, DetailsView, DropDownList, CheckBoxList e vários outros. O exemplo a seguir ilustra a associação da DataTable retornada pelo método **GetProducts ()** a um GridView em apenas uma escassa três linhas de código dentro da **página\_** manipulador de eventos de carregamento.

AllProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample2.aspx)]

AllProducts.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample3.cs)]

[![a lista de produtos é exibida em um GridView](creating-a-data-access-layer-cs/_static/image36.png)](creating-a-data-access-layer-cs/_static/image35.png)

**Figura 13**: a lista de produtos é exibida em um GridView ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image37.png))

Embora esse exemplo tenha reescrito três linhas de código em nossa página de ASP.NET Page **\_** manipulador de eventos de carregamento, em Tutoriais futuros, examinaremos como usar o ObjectDataSource para recuperar de forma declarativa os dados da Dal. Com o ObjectDataSource, não precisaremos escrever nenhum código e também obterá o suporte de paginação e classificação!

## <a name="step-3-adding-parameterized-methods-to-the-data-access-layer"></a>Etapa 3: adicionando métodos com parâmetros à camada de acesso a dados

Neste ponto, nossa classe **ProductsTableAdapter** tem, mas um método, **GetProducts ()** , que retorna todos os produtos no banco de dados. Embora a capacidade de trabalhar com todos os produtos seja definitivamente útil, há ocasiões em que desejaremos recuperar informações sobre um produto específico ou todos os produtos que pertencem a uma determinada categoria. Para adicionar essa funcionalidade à nossa camada de acesso a dados, podemos adicionar métodos parametrizados ao TableAdapter.

Vamos adicionar o método **GetProductsByCategoryID (*CategoryID*)** . Para adicionar um novo método à DAL, retorne ao designer de conjunto de um, clique com o botão direito do mouse na seção **ProductsTableAdapter** e escolha Adicionar consulta.

![Clique com o botão direito do mouse no TableAdapter e escolha Adicionar consulta](creating-a-data-access-layer-cs/_static/image38.png)

**Figura 14**: clique com o botão direito do mouse no TableAdapter e escolha Adicionar consulta

Primeiro, é perguntado se desejamos acessar o banco de dados usando uma instrução SQL ad hoc ou um procedimento armazenado novo ou existente. Vamos optar por usar uma instrução SQL ad hoc novamente. Em seguida, pedimos a você o tipo de consulta SQL que gostaríamos de usar. Como desejamos retornar todos os produtos que pertencem a uma categoria especificada, queremos escrever uma instrução **Select** que retorne linhas.

[![optar por criar uma instrução SELECT que retorna linhas](creating-a-data-access-layer-cs/_static/image40.png)](creating-a-data-access-layer-cs/_static/image39.png)

**Figura 15**: escolha criar uma instrução **Select** que retorna linhas ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image41.png))

A próxima etapa é definir a consulta SQL usada para acessar os dados. Como queremos retornar apenas os produtos que pertencem a uma determinada categoria, uso a mesma instrução <strong>Select</strong> de <strong>GetProducts ()</strong>, mas adicione a seguinte cláusula <strong>Where</strong> : <strong>em que CategoryID = @CategoryID</strong>. O parâmetro <strong>@CategoryID</strong> indica ao assistente do TableAdapter que o método que estamos criando exigirá um parâmetro de entrada do tipo correspondente (ou seja, um inteiro anulável).

[![inserir uma consulta para retornar apenas produtos em uma categoria especificada](creating-a-data-access-layer-cs/_static/image43.png)](creating-a-data-access-layer-cs/_static/image42.png)

**Figura 16**: inserir uma consulta para retornar apenas os produtos em uma categoria especificada ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image44.png))

Na etapa final, podemos escolher quais padrões de acesso de dados usar, bem como personalizar os nomes dos métodos gerados. Para o padrão de preenchimento, vamos alterar o nome para <strong>FillByCategoryID</strong> e para retornar um padrão de retorno DataTable (os métodos <strong>Get*X</strong>*  ), vamos usar <strong>GetProductsByCategoryID</strong>.

[![escolher os nomes para os métodos TableAdapter](creating-a-data-access-layer-cs/_static/image46.png)](creating-a-data-access-layer-cs/_static/image45.png)

**Figura 17**: escolha os nomes para os métodos do TableAdapter ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image47.png))

Depois de concluir o assistente, o DataSet Designer inclui os novos métodos TableAdapter.

![Os produtos agora podem ser consultados por categoria](creating-a-data-access-layer-cs/_static/image48.png)

**Figura 18**: os produtos agora podem ser consultados por categoria

Reserve um tempo para adicionar um método **GetProductByProductID (*ProductID*)** usando a mesma técnica.

Essas consultas parametrizadas podem ser testadas diretamente do DataSet Designer. Clique com o botão direito do mouse no método no TableAdapter e escolha Visualizar dados. Em seguida, insira os valores a serem usados para os parâmetros e clique em Visualizar.

[![os produtos que pertencem à categoria bebidas são mostrados](creating-a-data-access-layer-cs/_static/image50.png)](creating-a-data-access-layer-cs/_static/image49.png)

**Figura 19**: os produtos que pertencem à categoria bebidas são mostrados ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image51.png))

Com o método **GetProductsByCategoryID (*CategoryID*)** em nossa Dal, agora podemos criar uma página ASP.NET que exibe apenas os produtos em uma categoria especificada. O exemplo a seguir mostra todos os produtos que estão na categoria bebidas, que têm um **CategoryID** de 1.

Bebidas. asp

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample4.aspx)]

Beverages.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample5.cs)]

[![esses produtos na categoria bebidas são exibidos](creating-a-data-access-layer-cs/_static/image53.png)](creating-a-data-access-layer-cs/_static/image52.png)

**Figura 20**: os produtos na categoria bebidas são exibidos ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image54.png))

## <a name="step-4-inserting-updating-and-deleting-data"></a>Etapa 4: inserindo, atualizando e excluindo dados

Há dois padrões comumente usados para inserir, atualizar e excluir dados. O primeiro padrão, que chamarei de padrão direto de banco de dados, envolve a criação de métodos que, quando invocados, emitam um comando **Insert**, **Update**ou **delete** para o banco de dados que opera em um único registro de banco de dados. Esses métodos normalmente são passados em uma série de valores escalares (inteiros, cadeias de caracteres, Boolianos, DateTimes e assim por diante) que correspondem aos valores a serem inseridos, atualizados ou excluídos. Por exemplo, com esse padrão para a tabela **Products** , o método Delete usaria um parâmetro Integer, indicando o **ProductID** do registro a ser excluído, enquanto o método Insert usaria uma cadeia de caracteres para o **NomeDoProduto**, um decimal para o **PreçoUnitário**, um inteiro para o **UnitsOnStock**e assim por diante.

[![cada solicitação de inserção, atualização e exclusão é enviada para o banco de dados imediatamente](creating-a-data-access-layer-cs/_static/image56.png)](creating-a-data-access-layer-cs/_static/image55.png)

**Figura 21**: cada solicitação de inserção, atualização e exclusão é enviada para o banco de dados imediatamente ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image57.png))

O outro padrão, ao qual vou me referir como o padrão de atualização do lote, é atualizar um conjunto de registros, uma DataTable ou uma coleção de DataRows inteiras em uma chamada de método. Com esse padrão, um desenvolvedor exclui, insere e modifica as linhas de tabela em uma DataTable e, em seguida, passa esses DataRows ou DataTable para um método Update. Esse método enumera as DataRows transmitidas, determina se elas foram modificadas, adicionadas ou excluídas (por meio do valor da [propriedade RowState](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx) da DataRow) e emite a solicitação de banco de dados apropriada para cada registro.

[![todas as alterações são sincronizadas com o banco de dados quando o método Update é invocado](creating-a-data-access-layer-cs/_static/image59.png)](creating-a-data-access-layer-cs/_static/image58.png)

**Figura 22**: todas as alterações são sincronizadas com o banco de dados quando o método de atualização é invocado ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image60.png))

O TableAdapter usa o padrão de atualização de lote por padrão, mas também dá suporte ao padrão de banco de BD direto. Como selecionamos a opção "gerar instruções INSERT, Update e Delete" nas propriedades avançadas ao criar nosso TableAdapter, o **ProductsTableAdapter** contém um método **Update ()** , que implementa o padrão de atualização em lotes. Especificamente, o TableAdapter contém um método **Update ()** que pode ser passado pelo dataset tipado, uma DataTable fortemente tipada ou uma ou mais DataRows. Se você saiu da caixa de seleção "GenerateDBDirectMethods" marcada ao criar o TableAdapter, o padrão direto do banco de forma também será implementado por meio dos métodos **Insert ()** , **Update ()** e **Delete ()** .

Os dois padrões de modificação de dados usam as propriedades **InsertCommand**, **UpdateCommand**e **DeleteCommand** do TableAdapter para emitir seus comandos **Insert**, **Update**e **delete** para o banco de dados. Você pode inspecionar e modificar as propriedades **InsertCommand**, **UpdateCommand**e **DeleteCommand** clicando no TableAdapter no DataSet Designer e, em seguida, acessando o janela Propriedades. (Verifique se você selecionou o TableAdapter e se o objeto **ProductsTableAdapter** é aquele selecionado na lista suspensa na janela Propriedades.)

[![o TableAdapter tem as propriedades InsertCommand, UpdateCommand e DeleteCommand](creating-a-data-access-layer-cs/_static/image62.png)](creating-a-data-access-layer-cs/_static/image61.png)

**Figura 23**: o TableAdapter tem as propriedades **InsertCommand**, **UpdateCommand**e **DeleteCommand** ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image63.png))

Para examinar ou modificar qualquer uma dessas propriedades de comando de banco de dados, clique em **CommandText** subpropriedade, que abrirá o construtor de consultas.

[![configurar as instruções INSERT, UPDATE e DELETE no Construtor de Consultas](creating-a-data-access-layer-cs/_static/image65.png)](creating-a-data-access-layer-cs/_static/image64.png)

**Figura 24**: configurar as instruções **Insert**, **Update**e **delete** no construtor de consultas ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image66.png))

O exemplo de código a seguir mostra como usar o padrão de atualização do lote para dobrar o preço de todos os produtos que não são descontinuados e que têm 25 unidades em estoque ou menos:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample6.cs)]

O código a seguir ilustra como usar o padrão direto do banco de dado para excluir programaticamente um produto específico, depois atualizar um e, em seguida, adicionar um novo:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample7.cs)]

## <a name="creating-custom-insert-update-and-delete-methods"></a>Criando métodos personalizados de inserção, atualização e exclusão

Os métodos **Insert ()** , **Update ()** e **Delete ()** criados pelo método de banco de forma direto podem ser um pouco complicados, especialmente para tabelas com muitas colunas. Observando o exemplo de código anterior, sem a ajuda do IntelliSense, não é particularmente claro qual coluna de tabela de **produtos** é mapeada para cada parâmetro de entrada para os métodos **Update ()** e **Insert ()** . Pode haver ocasiões em que desejamos apenas atualizar uma única coluna ou duas, ou desejar um método personalizado **Insert ()** que, talvez, retorne o valor do campo de **identidade** do registro recentemente inserido (incremento automático).

Para criar esse método personalizado, retorne ao DataSet Designer. Clique com o botão direito do mouse no TableAdapter e escolha Adicionar consulta, retornando ao assistente do TableAdapter. Na segunda tela, podemos indicar o tipo de consulta a ser criado. Vamos criar um método que adiciona um novo produto e, em seguida, retorna o valor do **ProductID**do registro recém-adicionado. Portanto, opte por criar uma consulta de **inserção** .

[![criar um método para adicionar uma nova linha à tabela Products](creating-a-data-access-layer-cs/_static/image68.png)](creating-a-data-access-layer-cs/_static/image67.png)

**Figura 25**: criar um método para adicionar uma nova linha à tabela **produtos** ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image69.png))

Na próxima tela, o **CommandText** de **InsertCommand**é exibido. Aumente essa consulta adicionando o **escopo de seleção\_identidade ()** no final da consulta, que retornará o último valor de identidade inserido em uma coluna de **identidade** no mesmo escopo. (Consulte a [documentação técnica](https://msdn.microsoft.com/library/ms190315.aspx) para obter mais informações sobre o **escopo\_identidade ()** e por que você provavelmente desejará [usar o escopo\_identidade () em vez de @@IDENTITY](http://weblogs.sqlteam.com/travisl/archive/2003/10/29/405.aspx).) Verifique se você finalizou a instrução **Insert** com um ponto-e-vírgula antes de adicionar a instrução **Select** .

[![aumentar a consulta para retornar o valor de SCOPE_IDENTITY ()](creating-a-data-access-layer-cs/_static/image71.png)](creating-a-data-access-layer-cs/_static/image70.png)

**Figura 26**: aumentar a consulta para retornar o **escopo\_valor Identity ()** ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image72.png))

Por fim, nomeie o novo método **InsertProduct**.

[![definir o novo nome do método como InsertProduct](creating-a-data-access-layer-cs/_static/image74.png)](creating-a-data-access-layer-cs/_static/image73.png)

**Figura 27**: definir o novo nome do método como **InsertProduct** ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image75.png))

Ao retornar ao designer de conjunto de um, você verá que o **ProductsTableAdapter** contém um novo método, **InsertProduct**. Se esse novo método não tiver um parâmetro para cada coluna na tabela **Products** , é provável que você tenha esquecido de encerrar a instrução **Insert** com um ponto-e-vírgula. Configure o método **InsertProduct** e verifique se você tem um ponto-e-vírgula delimitando as instruções **Insert** e **Select** .

Por padrão, os métodos Insert emitem métodos que não são de consulta, o que significa que eles retornam o número de linhas afetadas. No entanto, queremos que o método **InsertProduct** retorne o valor retornado pela consulta, e não o número de linhas afetadas. Para fazer isso, ajuste a propriedade **executemode** do método **InsertProduct** para **escalar**.

[![alterar a propriedade Executemode para escalar](creating-a-data-access-layer-cs/_static/image77.png)](creating-a-data-access-layer-cs/_static/image76.png)

**Figura 28**: alterar a propriedade **executemode** para **escalar** ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image78.png))

O código a seguir mostra esse novo método **InsertProduct** em ação:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample8.cs)]

## <a name="step-5-completing-the-data-access-layer"></a>Etapa 5: concluindo a camada de acesso a dados

Observe que a classe **ProductsTableAdapters** retorna os **valores CategoryID** e **CódigoDoFornecedor** da tabela **Products** , mas não inclui a coluna **CategoryName** da tabela **Categories** ou a coluna **CompanyName** da tabela **suppliers** , embora essas sejam provavelmente as colunas que desejamos exibir ao mostrar as informações do produto. Podemos aumentar o método inicial do TableAdapter, **GetProducts ()** , para incluir os valores de coluna **CategoryName** e **CompanyName** , que atualizarão a DataTable fortemente tipada para incluir essas novas colunas também.

No entanto, isso pode apresentar um problema, pois os métodos do TableAdapter para inserção, atualização e exclusão de dados são baseados nesse método inicial. Felizmente, os métodos gerados automaticamente para inserção, atualização e exclusão não são afetados por subconsultas na cláusula **Select** . Ao se preocupar em Adicionar nossas consultas a **categorias** e **fornecedores** como subconsultas, em vez de **ingressar** em s, vamos evitar ter que retrabalhar com esses métodos para modificar os dados. Clique com o botão direito do mouse no método **GetProducts ()** no **ProductsTableAdapter** e escolha Configurar. Em seguida, ajuste a cláusula **Select** para que ela tenha a seguinte aparência:

[!code-sql[Main](creating-a-data-access-layer-cs/samples/sample9.sql)]

[![atualizar a instrução SELECT para o método GetProducts ()](creating-a-data-access-layer-cs/_static/image80.png)](creating-a-data-access-layer-cs/_static/image79.png)

**Figura 29**: atualizar a instrução **Select** para o método **GetProducts ()** ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image81.png))

Depois de atualizar o método **GetProducts ()** para usar essa nova consulta, a DataTable incluirá duas novas colunas: **CategoryName** e **suppliename**.

![A DataTable Products tem duas novas colunas](creating-a-data-access-layer-cs/_static/image82.png)

**Figura 30**: a DataTable **Products** tem duas novas colunas

Reserve um tempo para atualizar a cláusula **Select** no método **GetProductsByCategoryID (*CategoryID*)** também.

Se você atualizar o **GetProducts ()** **Select** usando a sintaxe de **junção** , o DataSet Designer não poderá gerar automaticamente os métodos para inserir, atualizar e excluir dados de banco usando o padrão de DB Direct. Em vez disso, você precisará criá-las manualmente da mesma forma que fizemos com o método **InsertProduct** anteriormente neste tutorial. Além disso, você terá de fornecer manualmente os valores de propriedade **InsertCommand**, **UpdateCommand**e **DeleteCommand** se quiser usar o padrão de atualização do lote.

## <a name="adding-the-remaining-tableadapters"></a>Adicionando os TableAdapters restantes

Até agora, vimos apenas trabalhar com um único TableAdapter para uma única tabela de banco de dados. No entanto, o banco de dados Northwind contém várias tabelas relacionadas com as quais precisaremos trabalhar em nosso aplicativo Web. Um DataSet tipado pode conter várias tabelas de tabela relacionadas. Portanto, para concluir nossa DAL, precisamos adicionar DataTables para as outras tabelas que usaremos nesses tutoriais. Para adicionar um novo TableAdapter a um DataSet tipado, abra o DataSet Designer, clique com o botão direito do mouse no designer e escolha Add/TableAdapter. Isso criará um novo DataTable e TableAdapter e o guiará pelo assistente que examinamos anteriormente neste tutorial.

Reserve alguns minutos para criar os seguintes TableAdapters e métodos usando as consultas a seguir. Observe que as consultas no **ProductsTableAdapter** incluem as subconsultas para obter a categoria de cada produto e os nomes de fornecedores. Além disso, se você esteve acompanhando, já adicionou os métodos **GetProducts ()** e **GetProductsByCategoryID (*CategoryID*)** da classe **ProductsTableAdapter** .

- **ProductsTableAdapter**

  - **GetProducts**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample10.sql)]
  - **GetProductsByCategoryID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample11.sql)]
  - **GetProductsBySupplierID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample12.sql)]
  - **GetProductByProductID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample13.sql)]
- **CategoriesTableAdapter**

  - **GetCategories**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample14.sql)]
  - **GetCategoryByCategoryID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample15.sql)]
- **SuppliersTableAdapter**

  - **Getsuppliers**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample16.sql)]
  - **GetSuppliersByCountry**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample17.sql)]
  - **GetSupplierBySupplierID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample18.sql)]
- **EmployeesTableAdapter**

  - **GetEmployees**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample19.sql)]
  - **GetEmployeesByManager**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample20.sql)]
  - **GetEmployeeByEmployeeID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample21.sql)]

[![o designer de conjunto de DataSet após os quatro TableAdapters terem sido adicionados](creating-a-data-access-layer-cs/_static/image84.png)](creating-a-data-access-layer-cs/_static/image83.png)

**Figura 31**: o designer de conjunto de DataSet após os quatro TableAdapters terem sido adicionados ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image85.png))

## <a name="adding-custom-code-to-the-dal"></a>Adicionando código personalizado à DAL

Os TableAdapters e DataTables adicionados ao DataSet tipado são expressos como um arquivo de definição de esquema XML (**Northwind. xsd**). Você pode exibir essas informações de esquema clicando com o botão direito do mouse no arquivo **Northwind. xsd** na Gerenciador de soluções e escolhendo exibir código.

[![o arquivo de definição de esquema XML (XSD) para o conjunto de tipos Northwind](creating-a-data-access-layer-cs/_static/image87.png)](creating-a-data-access-layer-cs/_static/image86.png)

**Figura 32**: o arquivo de definição de esquema XML (XSD) para o dataset tipado Northwind ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image88.png))

Essas informações de esquema são convertidas no C# código ou Visual Basic em tempo de design quando compiladas ou no tempo de execução (se necessário), em que ponto você pode percorrer isso com o depurador. Para exibir esse código gerado automaticamente, vá para a Modo de Exibição de Classe e faça drill down até as classes TableAdapter ou DataSet tipado. Se você não vir o Modo de Exibição de Classe na tela, vá para o menu exibir e selecione-o a partir daí, ou pressione Ctrl + Shift + C. No Modo de Exibição de Classe você pode ver as propriedades, os métodos e os eventos do DataSet tipado e classes do TableAdapter. Para exibir o código de um método específico, clique duas vezes no nome do método na Modo de Exibição de Classe ou clique com o botão direito do mouse nele e escolha ir para definição.

![Inspecione o código gerado automaticamente selecionando ir para definição na Modo de Exibição de Classe](creating-a-data-access-layer-cs/_static/image89.png)

**Figura 33**: Inspecione o código gerado automaticamente selecionando ir para definição na modo de exibição de classe

Embora o código gerado automaticamente possa ser uma excelente economia de tempo, o código geralmente é muito genérico e precisa ser personalizado para atender às necessidades exclusivas de um aplicativo. No entanto, o risco de estender o código gerado automaticamente é que a ferramenta que gerou o código pode decidir que é a hora de "regenerar" e substituir suas personalizações. Com o novo conceito de classe parcial do .NET 2.0, é fácil dividir uma classe em vários arquivos. Isso nos permite adicionar nossos próprios métodos, propriedades e eventos às classes geradas automaticamente sem precisar se preocupar com o fato de o Visual Studio substituir nossas personalizações.

Para demonstrar como personalizar a DAL, vamos adicionar um método **GetProducts ()** à classe **SuppliersRow** . A classe **SuppliersRow** representa um único registro na tabela **suppliers** ; cada fornecedor pode usar zero para muitos produtos, de modo que **GetProducts ()** retornará os produtos do fornecedor especificado. Para fazer isso, crie um novo arquivo de classe na pasta de **código do\_de aplicativos** chamada **SuppliersRow.cs** e adicione o seguinte código:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample22.cs)]

Essa classe parcial instrui o compilador que ao criar a classe **Northwind. SuppliersRow** para incluir o método **GetProducts ()** que acabamos de definir. Se você compilar seu projeto e retornar ao Modo de Exibição de Classe você verá **GetProducts ()** agora listado como um método de **Northwind. SuppliersRow**.

![O método GetProducts () agora faz parte da classe Northwind. SuppliersRow](creating-a-data-access-layer-cs/_static/image90.png)

**Figura 34**: o método **GetProducts ()** agora faz parte da classe **Northwind. SuppliersRow**

O método **GetProducts ()** agora pode ser usado para enumerar o conjunto de produtos para um fornecedor específico, como mostra o código a seguir:

[!code-html[Main](creating-a-data-access-layer-cs/samples/sample23.html)]

Esses dados também podem ser exibidos em qualquer ASP. Controles da Web de dados da rede. A página a seguir usa um controle GridView com dois campos:

- Um BoundField que exibe o nome de cada fornecedor e
- Um TemplateField que contém um controle BulletedList que está associado aos resultados retornados pelo método **GetProducts ()** para cada fornecedor.

Vamos examinar como exibir esses relatórios de detalhes mestres em Tutoriais futuros. Por enquanto, este exemplo foi projetado para ilustrar o uso do método personalizado adicionado à classe **Northwind. SuppliersRow** .

SuppliersAndProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample24.aspx)]

SuppliersAndProducts.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample25.cs)]

[![o nome da empresa do fornecedor estiver listado na coluna à esquerda, seus produtos à direita](creating-a-data-access-layer-cs/_static/image92.png)](creating-a-data-access-layer-cs/_static/image91.png)

**Figura 35**: o nome da empresa do fornecedor está listado na coluna esquerda, seus produtos à direita ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image93.png))

## <a name="summary"></a>Resumo

Ao criar um aplicativo Web que cria a DAL deve ser uma de suas primeiras etapas, ocorrendo antes de você começar a criar sua camada de apresentação. Com o Visual Studio, a criação de uma DAL baseada em datasets tipados é uma tarefa que pode ser realizada em 10-15 minutos sem escrever uma linha de código. Os tutoriais que avançam serão criados com base nessa DAL. No [próximo tutorial](creating-a-business-logic-layer-cs.md) , definiremos um número de regras de negócio e veremos como implementá-las em uma camada de lógica de negócios separada.

Boa programação!

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Criando uma DAL usando TableAdapters e DataTables com rigidez de tipos no VS 2005 e ASP.NET 2,0](https://weblogs.asp.net/scottgu/435498)
- [Criando componentes da camada de dados e passando dados por meio de camadas](https://msdn.microsoft.com/library/ms978496.aspx)
- [Criar uma camada de acesso a dados com o Visual Studio 2005 DataSet Designer](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner)
- [Criptografando informações de configuração em aplicativos ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [Visão geral de TableAdapter](https://msdn.microsoft.com/library/bz9tthwx.aspx)
- [Trabalhando com um DataSet tipado](https://msdn.microsoft.com/library/esbykkzb.aspx)
- [Usando o acesso a dados fortemente tipados no Visual Studio 2005 e no ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/020806-1.aspx)
- [Como estender métodos do TableAdapter](https://blogs.msdn.com/vbteam/archive/2005/05/04/ExtendingTableAdapters.aspx)
- [Recuperando dados escalares de um procedimento armazenado](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Treinamento em vídeo sobre tópicos contidos neste tutorial

- [Camadas de Acesso a Dados em aplicativos do ASP.NET](../../../videos/data-access/adonet-data-services/data-access-layers-in-aspnet-applications.md)
- [Como associar manualmente um conjunto de um DataSet a um DataGrid](../../../videos/data-access/adonet-data-services/how-to-manually-bind-a-dataset-to-a-datagrid.md)
- [Como trabalhar com conjuntos de e filtros de um aplicativo ASP](../../../videos/data-access/adonet-data-services/how-to-work-with-datasets-and-filters-from-an-asp-application.md)

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores potenciais para este tutorial foram Ron Green, Hilton Giesenow, Dennis Patterson, Liz Shulok, Abel Gomez e Carlos Santos. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Próximo](creating-a-business-logic-layer-cs.md)
