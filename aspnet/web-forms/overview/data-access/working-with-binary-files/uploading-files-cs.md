---
uid: web-forms/overview/data-access/working-with-binary-files/uploading-files-cs
title: Carregando arquivos (C#) | Microsoft Docs
author: rick-anderson
description: Saiba como permitir que os usuários carreguem arquivos binários (como documentos do Word ou PDF) em seu site onde eles podem ser armazenados no sistema de arquivos do servidor...
ms.author: riande
ms.date: 03/27/2007
ms.assetid: b381b1da-feb3-4776-bc1b-75db53eb90ab
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/uploading-files-cs
msc.type: authoredcontent
ms.openlocfilehash: 4e3e32a829de386a681504c8d5d61dd258b8b2e6
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74581674"
---
# <a name="uploading-files-c"></a>Carregar arquivos (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_54_CS.exe) ou [baixar PDF](uploading-files-cs/_static/datatutorial54cs1.pdf)

> Saiba como permitir que os usuários carreguem arquivos binários (como documentos do Word ou PDF) em seu site onde eles podem ser armazenados no sistema de arquivos ou no banco de dados do servidor.

## <a name="introduction"></a>Introdução

Todos os tutoriais que nós examinamos até agora trabalharam exclusivamente com dados de texto. No entanto, muitos aplicativos têm modelos de dados que capturam texto e dados binários. Um site de encontros online pode permitir que os usuários carreguem uma imagem para associar ao seu perfil. Um site de recrutamento pode permitir que os usuários carreguem seu currículo como um documento do Microsoft Word ou PDF.

Trabalhar com dados binários adiciona um novo conjunto de desafios. Devemos decidir como os dados binários são armazenados no aplicativo. A interface usada para inserir novos registros deve ser atualizada para permitir que o usuário carregue um arquivo de seu computador e etapas adicionais devem ser executadas para exibir ou fornecer um meio de baixar dados binários associados a um registro. Neste tutorial e nos próximos três, exploraremos como enfrentar esses desafios. No final desses tutoriais, criaremos um aplicativo totalmente funcional que associa um folheto de imagem e PDF a cada categoria. Neste tutorial específico, veremos técnicas diferentes para armazenar dados binários e exploraremos como permitir que os usuários carreguem um arquivo de seu computador e o Salvem no sistema de arquivos do servidor Web.

> [!NOTE]
> Os dados binários que fazem parte de um modelo de dados do aplicativo são, às vezes, chamados de [blob](http://en.wikipedia.org/wiki/Binary_large_object), um acrônimo para objeto binário grande. Nesses tutoriais, escolhi usar os dados binários de terminologia, embora o termo BLOB seja sinônimo.

## <a name="step-1-creating-the-working-with-binary-data-web-pages"></a>Etapa 1: criando o trabalho com páginas da Web de dados binários

Antes de começarmos a explorar os desafios associados à adição de suporte para dados binários, primeiro vamos reservar um momento para criar as páginas ASP.NET em nosso projeto de site, que precisaremos para este tutorial e os três seguintes. Comece adicionando uma nova pasta chamada `BinaryData`. Em seguida, adicione as seguintes páginas ASP.NET a essa pasta, assegurando a associação de cada página com a página mestra `Site.master`:

- `Default.aspx`
- `FileUpload.aspx`
- `DisplayOrDownloadData.aspx`
- `UploadInDetailsView.aspx`
- `UpdatingAndDeleting.aspx`

![Adicionar as páginas ASP.NET para os tutoriais relacionados a dados binários](uploading-files-cs/_static/image1.gif)

**Figura 1**: adicionar as páginas ASP.net para os tutoriais relacionados a dados binários

Assim como nas outras pastas, `Default.aspx` na pasta `BinaryData` listará os tutoriais em sua seção. Lembre-se de que o controle de usuário `SectionLevelTutorialListing.ascx` fornece essa funcionalidade. Portanto, adicione esse controle de usuário a `Default.aspx` arrastando-o da Gerenciador de Soluções para a página s modo de exibição de Design.

[![adicionar o controle de usuário SectionLevelTutorialListing. ascx a default. aspx](uploading-files-cs/_static/image2.gif)](uploading-files-cs/_static/image1.png)

**Figura 2**: Adicionar o controle de usuário `SectionLevelTutorialListing.ascx` ao `Default.aspx` ([clique para exibir a imagem em tamanho normal](uploading-files-cs/_static/image2.png))

Por fim, adicione essas páginas como entradas ao arquivo de `Web.sitemap`. Especificamente, adicione a seguinte marcação depois de aprimorar o `<siteMapNode>`GridView:

[!code-xml[Main](uploading-files-cs/samples/sample1.xml)]

Depois de atualizar `Web.sitemap`, Reserve um momento para exibir o site de tutoriais por meio de um navegador. O menu à esquerda agora inclui itens para os tutoriais de trabalho com dados binários.

![O mapa do site agora inclui entradas para os tutoriais de trabalho com dados binários](uploading-files-cs/_static/image3.gif)

**Figura 3**: o mapa do site agora inclui entradas para os tutoriais de trabalho com dados binários

## <a name="step-2-deciding-where-to-store-the-binary-data"></a>Etapa 2: decidindo onde armazenar os dados binários

Os dados binários associados ao modelo de dados do aplicativo podem ser armazenados em um dos dois locais: no sistema de arquivos do servidor Web, com uma referência ao arquivo armazenado no banco de dados; ou diretamente no próprio banco de dados (consulte a Figura 4). Cada abordagem tem seu próprio conjunto de prós e contras e merece uma discussão mais detalhada.

[![dados binários podem ser armazenados no sistema de arquivos ou diretamente no banco](uploading-files-cs/_static/image4.gif)](uploading-files-cs/_static/image3.png)

**Figura 4**: dados binários podem ser armazenados no sistema de arquivos ou diretamente no banco de dado ([clique para exibir a imagem em tamanho normal](uploading-files-cs/_static/image4.png))

Imagine que queríamos estender o banco de dados Northwind para associar uma imagem a cada produto. Uma opção seria armazenar esses arquivos de imagem no sistema de arquivos do servidor Web e registrar o caminho na tabela `Products`. Com essa abordagem, nós d adicionamos uma coluna `ImagePath` à tabela `Products` do tipo `varchar(200)`, talvez. Quando um usuário carregou uma imagem para Chai, essa imagem pode ser armazenada no sistema de arquivos do servidor Web em `~/Images/Tea.jpg`, em que `~` representa o caminho físico do aplicativo. Ou seja, se o site tiver raiz no caminho físico `C:\Websites\Northwind\`, `~/Images/Tea.jpg` será equivalente a `C:\Websites\Northwind\Images\Tea.jpg`. Depois de carregar o arquivo de imagem, nós d atualizamos o registro Chai na tabela `Products` de forma que sua coluna de `ImagePath` referenciava o caminho da nova imagem. Poderíamos usar `~/Images/Tea.jpg` ou apenas `Tea.jpg` se decidirmos que todas as imagens do produto seriam colocadas na pasta de `Images` do aplicativo.

As principais vantagens de armazenar os dados binários no sistema de arquivos são:

- A **facilidade de implementação** como veremos em breve, o armazenamento e a recuperação de dados binários armazenados diretamente dentro dele envolve um pouco mais de código do que ao trabalhar com dados por meio do sistema de arquivos. Além disso, para que um usuário exiba ou baixe dados binários, eles devem receber uma URL para esses dados. Se os dados residirem no sistema de arquivos do servidor Web, a URL será simples. No entanto, se os dados forem armazenados no banco de dado, será necessário criar uma página da Web que recuperará e retornará os dados do banco de dado.
- **Acesso mais amplo aos dados binários** talvez os dados binários precisem ser acessíveis a outros serviços ou aplicativos, aqueles que não podem efetuar pull dos dados do Database. Por exemplo, as imagens associadas a cada produto também podem precisar estar disponíveis para os usuários por [FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol); nesse caso, a que d queremos armazenar os dados binários no sistema de arquivos.
- **Desempenho** se os dados binários forem armazenados no sistema de arquivos, a demanda e o congestionamento de rede entre o servidor de banco de dados e o servidor Web serão menores do que se os dados binários forem armazenados diretamente no banco de dado.

A principal desvantagem de armazenar dados binários no sistema de arquivos é que ele separa os dados do banco de dado. Se um registro for excluído da tabela de `Products`, o arquivo associado no sistema de arquivos do servidor Web não será excluído automaticamente. Devemos escrever um código extra para excluir o arquivo ou o sistema de arquivos ficará desorganizado com arquivos órfãos e não utilizados. Além disso, ao fazer backup do banco de dados, devemos nos certificar também de fazer backups do arquivo binário associado no sistema de arquivos. Mover o banco de dados para outro site ou servidor apresenta desafios semelhantes.

Como alternativa, os dados binários podem ser armazenados diretamente em um banco de dado Microsoft SQL Server 2005 criando uma coluna do tipo `varbinary`. Assim como com outros tipos de dados de comprimento variável, você pode especificar um comprimento máximo dos dados binários que podem ser mantidos nesta coluna. Por exemplo, para reservar no máximo 5.000 bytes de uso `varbinary(5000)`; `varbinary(MAX)` permite o tamanho máximo de armazenamento, cerca de 2 GB.

A principal vantagem de armazenar os dados binários diretamente no banco de dados é o acoplamento rígido entre a data e o registro do banco de dados binários. Isso simplifica muito as tarefas de administração do banco de dados, como backups ou movimentação do banco de dados para um site ou servidor diferente. Além disso, a exclusão de um registro exclui automaticamente os dados binários correspondentes. Também há benefícios mais sutis de armazenar os dados binários no banco de dado. Consulte [armazenar arquivos binários diretamente no banco de dados usando o ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/120606-1.aspx) para obter uma discussão mais detalhada.

> [!NOTE]
> No Microsoft SQL Server 2000 e versões anteriores, o tipo de dados `varbinary` tinha um limite máximo de 8.000 bytes. Para armazenar até 2 GB de dados binários, é necessário usar o [tipo de dados`image`](https://msdn.microsoft.com/library/ms187993.aspx) . No entanto, com a adição de `MAX` no SQL Server 2005, o tipo de dados `image` foi preterido. Ainda há suporte para a compatibilidade com versões anteriores, mas a Microsoft anunciou que o tipo de dados `image` será removido em uma versão futura do SQL Server.

Se você estiver trabalhando com um modelo de dados mais antigo, poderá ver o tipo de dados `image`. A tabela de `Categories` do banco de dados Northwind tem uma coluna `Picture` que pode ser usada para armazenar os dados binários de um arquivo de imagem para a categoria. Como o banco de dados Northwind tem suas raízes no Microsoft Access e em versões anteriores do SQL Server, essa coluna é do tipo `image`.

Para este tutorial e os próximos três, usaremos as duas abordagens. A tabela `Categories` já tem uma coluna `Picture` para armazenar o conteúdo binário de uma imagem para a categoria. Adicionaremos uma coluna adicional, `BrochurePath`, para armazenar um caminho para um PDF no sistema de arquivos do servidor Web que pode ser usado para fornecer uma visão geral elegante da qualidade de impressão da categoria.

## <a name="step-3-adding-thebrochurepathcolumn-to-thecategoriestable"></a>Etapa 3: adicionar a coluna de`BrochurePath`à tabela`Categories`

Atualmente, a tabela Categories tem apenas quatro colunas: `CategoryID`, `CategoryName`, `Description`e `Picture`. Além desses campos, precisamos adicionar um novo que apontará para o folheto da categoria s (se houver). Para adicionar essa coluna, vá para a Gerenciador de Servidores, faça uma busca detalhada nas tabelas, clique com o botão direito do mouse na tabela `Categories` e escolha Abrir definição de tabela (veja a Figura 5). Se você não vir o Gerenciador de Servidores, exiba-o selecionando a opção Gerenciador de Servidores no menu exibir ou pressione CTRL + ALT + S.

Adicione uma nova coluna `varchar(200)` à tabela `Categories` chamada `BrochurePath` e permita `NULL` s e clique no ícone salvar (ou pressione Ctrl + S).

[![adicionar uma coluna BrochurePath à tabela Categories](uploading-files-cs/_static/image5.gif)](uploading-files-cs/_static/image5.png)

**Figura 5**: adicionar uma coluna `BrochurePath` à tabela `Categories` ([clique para exibir a imagem em tamanho normal](uploading-files-cs/_static/image6.png))

## <a name="step-4-updating-the-architecture-to-use-thepictureandbrochurepathcolumns"></a>Etapa 4: atualizando a arquitetura para usar as colunas`Picture`e`BrochurePath`

O `CategoriesDataTable` na DAL (camada de acesso a dados) atualmente tem quatro `DataColumn` s definidos: `CategoryID`, `CategoryName`, `Description`e `NumberOfProducts`. Quando originalmente criamos essa DataTable no tutorial [criando uma camada de acesso a dados](../introduction/creating-a-data-access-layer-cs.md) , a `CategoriesDataTable` tinha apenas as três primeiras colunas; a coluna `NumberOfProducts` foi adicionada ao [mestre/detalhes usando uma lista com marcadores de registros mestres com um tutorial de DataList detalhes](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) .

Conforme discutido na *criação de uma camada de acesso a dados*, as DataTables no dataset tipado compõem os objetos comerciais. Os TableAdapters são responsáveis por se comunicar com o banco de dados e preencher os objetos comerciais com os resultados da consulta. O `CategoriesDataTable` é preenchido pelo `CategoriesTableAdapter`, que tem três métodos de recuperação de dados:

- `GetCategories()` executa a consulta principal do TableAdapter s e retorna os campos `CategoryID`, `CategoryName`e `Description` de todos os registros na tabela `Categories`. A consulta principal é o que é usado pelos métodos `Insert` e `Update` gerados automaticamente.
- `GetCategoryByCategoryID(categoryID)` retorna os campos `CategoryID`, `CategoryName`e `Description` da categoria cujo `CategoryID` é igual a *CategoryID*.
- `GetCategoriesAndNumberOfProducts()`-retorna os campos `CategoryID`, `CategoryName`e `Description` para todos os registros na tabela `Categories`. Também usa uma subconsulta para retornar o número de produtos associados a cada categoria.

Observe que nenhuma dessas consultas retorna a tabela `Categories` s `Picture` ou `BrochurePath` colunas; Nem o `CategoriesDataTable` fornecer `DataColumn` s para esses campos. Para trabalhar com as propriedades Picture e `BrochurePath`, precisamos primeiro adicioná-las ao `CategoriesDataTable` e, em seguida, atualizar a classe `CategoriesTableAdapter` para retornar essas colunas.

## <a name="adding-thepictureandbrochurepathdatacolumn-s"></a>Adicionando o`Picture`e`BrochurePath``DataColumn` s

Comece adicionando essas duas colunas à `CategoriesDataTable`. Clique com o botão direito do mouse no cabeçalho `CategoriesDataTable` s, selecione Adicionar no menu de contexto e escolha a opção coluna. Isso criará um novo `DataColumn` na DataTable chamada `Column1`. Renomeie esta coluna como `Picture`. Na janela Propriedades, defina a propriedade `DataColumn` s `DataType` como `System.Byte[]` (isso não é uma opção na lista suspensa; você precisa digitá-la).

[![criar uma DataColumn denominada Picture cujo tipo de dados é System. Byte []](uploading-files-cs/_static/image6.gif)](uploading-files-cs/_static/image7.png)

**Figura 6**: criar um `DataColumn` nomeado `Picture` cujo `DataType` é `System.Byte[]` ([clique para exibir a imagem em tamanho normal](uploading-files-cs/_static/image8.png))

Adicione outro `DataColumn` à DataTable, nomeando-o `BrochurePath` usando o valor de `DataType` padrão (`System.String`).

## <a name="returning-thepictureandbrochurepathvalues-from-the-tableadapter"></a>Retornando os valores de`Picture`e`BrochurePath`do TableAdapter

Com esses dois `DataColumn` s adicionados ao `CategoriesDataTable`, estamos prontos para atualizar o `CategoriesTableAdapter`. Poderíamos ter esses dois valores de coluna retornados na consulta principal do TableAdapter, mas isso colocaria os dados binários sempre que o método `GetCategories()` fosse invocado. Em vez disso, vamos atualizar a consulta principal do TableAdapter para trazer de volta `BrochurePath` e criar um método de recuperação de dados adicional que retorna uma coluna de `Picture` de categoria específica.

Para atualizar a consulta principal do TableAdapter, clique com o botão direito do mouse no cabeçalho `CategoriesTableAdapter` s e escolha a opção configurar no menu de contexto. Isso abre o assistente de configuração do adaptador de tabela, que vimos em vários tutoriais anteriores. Atualize a consulta para retornar o `BrochurePath` e clique em concluir.

[![atualizar a lista de colunas na instrução SELECT para retornar também BrochurePath](uploading-files-cs/_static/image7.gif)](uploading-files-cs/_static/image9.png)

**Figura 7**: atualizar a lista de colunas na instrução `SELECT` para também retornar `BrochurePath` ([clique para exibir a imagem em tamanho normal](uploading-files-cs/_static/image10.png))

Ao usar instruções SQL ad hoc para o TableAdapter, atualizar a lista de colunas na consulta principal atualiza a lista de colunas para todos os métodos de consulta de `SELECT` no TableAdapter. Isso significa que o método `GetCategoryByCategoryID(categoryID)` foi atualizado para retornar a coluna `BrochurePath`, que pode ser o que pretendemos. No entanto, ela também atualizou a lista de colunas no método `GetCategoriesAndNumberOfProducts()`, removendo a subconsulta que retorna o número de produtos para cada categoria! Portanto, precisamos atualizar esse método s `SELECT` consulta. Clique com o botão direito do mouse no método `GetCategoriesAndNumberOfProducts()`, escolha configurar e reverta a consulta `SELECT` de volta para seu valor original:

[!code-sql[Main](uploading-files-cs/samples/sample2.sql)]

Em seguida, crie um novo método TableAdapter que retorna uma categoria específica `Picture` valor da coluna. Clique com o botão direito do mouse no cabeçalho `CategoriesTableAdapter` s e escolha a opção Adicionar consulta para iniciar o assistente de configuração de consulta do TableAdapter. A primeira etapa desse assistente nos pergunta se queremos consultar dados usando uma instrução SQL ad hoc, um novo procedimento armazenado ou um existente. Selecione usar instruções SQL e clique em Avançar. Como retornaremos uma linha, escolha a opção Selecionar que retorna linhas da segunda etapa.

[![selecionar a opção usar instruções SQL](uploading-files-cs/_static/image8.gif)](uploading-files-cs/_static/image11.png)

**Figura 8**: selecione a opção usar instruções SQL ([clique para exibir a imagem em tamanho normal](uploading-files-cs/_static/image12.png))

[![uma vez que a consulta retornará um registro da tabela categorias, escolha Selecionar que retorna linhas](uploading-files-cs/_static/image9.gif)](uploading-files-cs/_static/image13.png)

**Figura 9**: como a consulta retornará um registro da tabela categorias, escolha Selecionar que retorna linhas ([clique para exibir a imagem em tamanho normal](uploading-files-cs/_static/image14.png))

Na terceira etapa, insira a seguinte consulta SQL e clique em Avançar:

[!code-sql[Main](uploading-files-cs/samples/sample3.sql)]

A última etapa é escolher o nome para o novo método. Use `FillCategoryWithBinaryDataByCategoryID` e `GetCategoryWithBinaryDataByCategoryID` para preencher uma DataTable e retornar os padrões de DataTable, respectivamente. Clique em Concluir para concluir o assistente.

[![escolher os nomes para os métodos TableAdapter s](uploading-files-cs/_static/image10.gif)](uploading-files-cs/_static/image15.png)

**Figura 10**: escolha os nomes para os métodos TableAdapter s ([clique para exibir a imagem em tamanho normal](uploading-files-cs/_static/image16.png))

> [!NOTE]
> Depois de concluir o assistente de configuração de consulta do adaptador de tabela, você poderá ver uma caixa de diálogo informando que o novo texto de comando retorna dados com esquema diferente do esquema da consulta principal. Em suma, o assistente está observando que a consulta principal do TableAdapter s `GetCategories()` retorna um esquema diferente daquele que acabamos de criar. Mas isso é o que queremos, para que você possa desconsiderar essa mensagem.

Além disso, tenha em mente que, se você estiver usando instruções SQL ad hoc e usar o assistente para alterar a consulta principal do TableAdapter s em algum momento posterior, ele modificará a lista de colunas do `GetCategoryWithBinaryDataByCategoryID` método s `SELECT` instrução s para incluir apenas as colunas da consulta principal (ou seja, removerá a coluna `Picture` da consulta). Você precisará atualizar manualmente a lista de colunas para retornar a coluna `Picture`, semelhante ao que fizemos com o método `GetCategoriesAndNumberOfProducts()` anteriormente nesta etapa.

Depois de adicionar os dois `DataColumn` s ao `CategoriesDataTable` e ao método `GetCategoryWithBinaryDataByCategoryID` ao `CategoriesTableAdapter`, essas classes no designer de conjunto de um DataSet devem ser semelhantes à captura de tela da Figura 11.

![O designer de conjunto de um inclui as novas colunas e o método](uploading-files-cs/_static/image11.gif)

**Figura 11**: o DataSet Designer inclui as novas colunas e o método

## <a name="updating-the-business-logic-layer-bll"></a>Atualizando a camada de lógica de negócios (BLL)

Com a DAL atualizada, tudo o que resta é aumentar a camada de lógica de negócios (BLL) para incluir um método para o novo método de `CategoriesTableAdapter`. Adicione o seguinte método à classe `CategoriesBLL`:

[!code-csharp[Main](uploading-files-cs/samples/sample4.cs)]

## <a name="step-5-uploading-a-file-from-the-client-to-the-web-server"></a>Etapa 5: carregando um arquivo do cliente para o servidor Web

Ao coletar dados binários, muitas vezes esses dados são fornecidos por um usuário final. Para capturar essas informações, o usuário precisa ser capaz de carregar um arquivo de seu computador para o servidor Web. Os dados carregados, em seguida, precisam ser integrados ao modelo de dados, o que pode significar salvar o arquivo no sistema de arquivos do servidor Web e adicionar um caminho ao arquivo no banco de dado ou gravar o conteúdo binário diretamente no banco de dados. Nesta etapa, veremos como permitir que um usuário carregue arquivos de seu computador para o servidor. No próximo tutorial, vamos transformar nossa atenção para integrar o arquivo carregado com o modelo de dados.

O novo [controle da Web de FileUpload](https://msdn.microsoft.com/library/ms227677(VS.80).aspx) do ASP.NET 2,0 fornece um mecanismo para que os usuários enviem um arquivo do computador para o servidor Web. O controle FileUpload é renderizado como um elemento `<input>` cujo atributo `type` é definido como File, que os navegadores exibem como uma caixa de texto com um botão procurar. Clicar no botão procurar abre uma caixa de diálogo da qual o usuário pode selecionar um arquivo. Quando o formulário é Postado de volta, o conteúdo do arquivo s selecionado é enviado junto com o postback. No lado do servidor, as informações sobre o arquivo carregado podem ser acessadas por meio das propriedades do controle FileUpload s.

Para demonstrar o carregamento de arquivos, abra a página `FileUpload.aspx` na pasta `BinaryData`, arraste um controle FileUpload da caixa de ferramentas para o designer e defina a propriedade do controle s `ID` como `UploadTest`. Em seguida, adicione um botão controle da Web definindo suas propriedades `ID` e `Text` para `UploadButton` e carregar o arquivo selecionado, respectivamente. Por fim, coloque um controle da Web de rótulo abaixo do botão, desmarque sua propriedade `Text` e defina sua propriedade `ID` como `UploadDetails`.

[![adicionar um controle FileUpload à página ASP.NET](uploading-files-cs/_static/image12.gif)](uploading-files-cs/_static/image17.png)

**Figura 12**: adicionar um controle FileUpload à página ASP.net ([clique para exibir a imagem em tamanho normal](uploading-files-cs/_static/image18.png))

A Figura 13 mostra essa página quando exibida por meio de um navegador. Observe que clicar no botão procurar abre uma caixa de diálogo de seleção de arquivo, permitindo que o usuário escolha um arquivo de seu computador. Depois que um arquivo tiver sido selecionado, clicar no botão carregar arquivo selecionado causará um postback que envia o conteúdo binário do arquivo selecionado para o servidor Web.

[![o usuário pode selecionar um arquivo para carregar de seu computador para o servidor](uploading-files-cs/_static/image13.gif)](uploading-files-cs/_static/image19.png)

**Figura 13**: o usuário pode selecionar um arquivo para carregar de seu computador para o servidor ([clique para exibir a imagem em tamanho normal](uploading-files-cs/_static/image20.png))

No postback, o arquivo carregado pode ser salvo no sistema de arquivos ou seus dados binários podem ser trabalhados diretamente por meio de um fluxo. Para este exemplo, vamos criar uma pasta `~/Brochures` e salvar o arquivo carregado. Comece adicionando a pasta `Brochures` ao site como uma subpasta do diretório raiz. Em seguida, crie um manipulador de eventos para o evento `UploadButton` s `Click` e adicione o seguinte código:

[!code-csharp[Main](uploading-files-cs/samples/sample5.cs)]

O controle FileUpload fornece uma variedade de propriedades para trabalhar com os dados carregados. Por exemplo, a [propriedade`HasFile`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.hasfile.aspx) indica se um arquivo foi carregado pelo usuário, enquanto a [Propriedade`FileBytes`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.filebytes.aspx) fornece acesso aos dados binários carregados como uma matriz de bytes. O manipulador de eventos `Click` começa garantindo que um arquivo foi carregado. Se um arquivo tiver sido carregado, o rótulo mostrará o nome do arquivo carregado, seu tamanho em bytes e seu tipo de conteúdo.

> [!NOTE]
> Para garantir que o usuário carregue um arquivo, você pode verificar a propriedade `HasFile` e exibir um aviso se ele `false`, ou você pode usar o [controle RequiredFieldValidator](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/validation/default.aspx) em vez disso.

A `SaveAs(filePath)` FileUpload s salva o arquivo carregado no *FilePath*especificado. *FilePath* deve ser um *caminho físico* (`C:\Websites\Brochures\SomeFile.pdf`) em vez de um *caminho* *virtual* (`/Brochures/SomeFile.pdf`). O [método`Server.MapPath(virtPath)`](https://msdn.microsoft.com/library/system.web.httpserverutility.mappath.aspx) usa um caminho virtual e retorna seu caminho físico correspondente. Aqui, o caminho virtual é `~/Brochures/fileName`, em que *filename* é o nome do arquivo carregado. Consulte [usando o Server. MapPath](http://www.4guysfromrolla.com/webtech/121799-1.shtml) para obter mais informações sobre caminhos físicos e virtuais e usando `Server.MapPath`.

Depois de concluir o manipulador de eventos `Click`, Reserve um tempo para testar a página em um navegador. Clique no botão procurar e selecione um arquivo do disco rígido e, em seguida, clique no botão carregar arquivo selecionado. O postback enviará o conteúdo do arquivo selecionado para o servidor Web, que exibirá informações sobre o arquivo antes de salvá-lo na pasta `~/Brochures`. Depois de carregar o arquivo, retorne ao Visual Studio e clique no botão atualizar na Gerenciador de Soluções. Você deve ver o arquivo que acabou de carregar na pasta ~/brochures!

[![o arquivo EvolutionValley. jpg foi carregado no servidor Web](uploading-files-cs/_static/image14.gif)](uploading-files-cs/_static/image21.png)

**Figura 14**: o arquivo `EvolutionValley.jpg` foi carregado no servidor Web ([clique para exibir a imagem em tamanho normal](uploading-files-cs/_static/image22.png))

![EvolutionValley. jpg foi salvo na pasta ~/brochures](uploading-files-cs/_static/image15.gif)

**Figura 15**: `EvolutionValley.jpg` foi salva na pasta `~/Brochures`

## <a name="subtleties-with-saving-uploaded-files-to-the-file-system"></a>Sutilezas com o salvamento de arquivos carregados no sistema de arquivos

Há várias sutilezas que devem ser abordadas ao salvar os arquivos de carregamento no sistema de arquivos do servidor Web. Primeiro, há o problema de segurança. Para salvar um arquivo no sistema de arquivos, o contexto de segurança no qual a página ASP.NET está sendo executada deve ter permissões de gravação. O servidor Web de desenvolvimento ASP.NET é executado sob o contexto da sua conta de usuário atual. Se você estiver usando o Microsoft s Serviços de Informações da Internet (IIS) como o servidor Web, o contexto de segurança dependerá da versão do IIS e de sua configuração.

Outro desafio de salvar arquivos no sistema de arquivos gira em contranomear os arquivos. Atualmente, nossa página salva todos os arquivos carregados no diretório `~/Brochures` usando o mesmo nome do arquivo no computador do cliente. Se o usuário A carregar um folheto com o nome `Brochure.pdf`, o arquivo será salvo como `~/Brochure/Brochure.pdf`. Mas e se algum tempo depois o usuário B carregar um arquivo de folheto diferente que tem o mesmo nome de arquivo (`Brochure.pdf`)? Com o código que temos agora, o arquivo A s do usuário será substituído pelo carregamento do usuário B s.

Há várias técnicas para resolver conflitos de nome de arquivo. Uma opção é proibir o carregamento de um arquivo, caso já exista um com o mesmo nome. Com essa abordagem, quando o usuário B tenta carregar um arquivo chamado `Brochure.pdf`, o sistema não salvaria seu arquivo e exibiria uma mensagem informando ao usuário B para renomear o arquivo e tentar novamente. Outra abordagem é salvar o arquivo usando um nome de arquivo exclusivo, que poderia ser um [GUID (identificador global exclusivo)](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) ou o valor das colunas de chave primária (s) do registro de banco de dados correspondente (supondo que o carregamento esteja associado a uma linha específica no modelo de dados). No próximo tutorial, exploraremos essas opções mais detalhadamente.

## <a name="challenges-involved-with-very-large-amounts-of-binary-data"></a>Desafios envolvidos com grandes quantidades de dados binários

Esses tutoriais pressupõem que os dados binários capturados tenham um tamanho modesto. Trabalhar com grandes quantidades de arquivos de dados binários que são vários megabytes ou maiores apresenta novos desafios que estão além do escopo desses tutoriais. Por exemplo, por padrão, o ASP.NET rejeitará os carregamentos de mais de 4 MB, embora isso possa ser configurado por meio do [elemento`<httpRuntime>`](https://msdn.microsoft.com/library/e1f13641.aspx) no `Web.config`. O IIS impõe também suas próprias limitações de tamanho de upload de arquivo. Consulte [tamanho do arquivo de carregamento do IIS](http://vandamme.typepad.com/development/2005/09/iis_upload_file.html) para obter mais informações. Além disso, o tempo necessário para carregar arquivos grandes pode exceder o padrão de 110 segundos ASP.NET aguardará uma solicitação. Também há problemas de memória e desempenho que surgem ao trabalhar com arquivos grandes.

O controle FileUpload é impraticável para carregamentos de arquivos grandes. À medida que o conteúdo do arquivo está sendo Postado no servidor, o usuário final deve aguardar o paciente sem qualquer confirmação de que o upload está progredindo. Isso não é muito um problema ao lidar com arquivos menores que podem ser carregados em alguns segundos, mas pode ser um problema ao lidar com arquivos maiores que podem levar minutos para serem carregados. Há uma variedade de controles de upload de arquivo de terceiros que são mais adequados para lidar com grandes carregamentos e muitos desses fornecedores fornecem indicadores de progresso e gerenciadores de carregamento do ActiveX que apresentam uma experiência de usuário muito mais elegante.

Se seu aplicativo precisar lidar com arquivos grandes, você precisará investigar cuidadosamente os desafios e encontrar soluções adequadas para suas necessidades específicas.

## <a name="summary"></a>Resumo

A criação de um aplicativo que precisa capturar dados binários apresenta vários desafios. Neste tutorial, exploramos os dois primeiros: decidindo onde armazenar os dados binários e permitindo que um usuário carregue conteúdo binário por meio de uma página da Web. Nos próximos três tutoriais, veremos como associar os dados carregados a um registro no banco de dados, além de como exibir os dados binários junto com seus campos de texto.

Boa programação!

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Usando tipos de dados de valor grande](https://msdn.microsoft.com/library/ms178158.aspx)
- [Início rápido do controle FileUpload](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/fileupload.aspx)
- [O controle de servidor FileUpload do ASP.NET 2,0](http://www.wrox.com/WileyCDA/Section/id-292158.html)
- [O lado escuro dos carregamentos de arquivos](http://www.aspnetresources.com/articles/dark_side_of_file_uploads.aspx)

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores potenciais para este tutorial foram Teresa Murphy e Bernadette Leigh. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Avançar](displaying-binary-data-in-the-data-web-controls-cs.md)
