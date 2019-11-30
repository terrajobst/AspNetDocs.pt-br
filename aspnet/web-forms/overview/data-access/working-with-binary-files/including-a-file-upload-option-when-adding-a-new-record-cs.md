---
uid: web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-cs
title: Incluindo uma opção de carregamento de arquivo ao adicionar um novoC#registro () | Microsoft Docs
author: rick-anderson
description: Este tutorial mostra como criar uma interface da Web que permite ao usuário inserir dados de texto e carregar arquivos binários. Para ilustrar as opções disponíveis t...
ms.author: riande
ms.date: 03/27/2007
ms.assetid: 362ade25-3965-4fb2-88d2-835c4786244f
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-cs
msc.type: authoredcontent
ms.openlocfilehash: f1287e180151b3034a7b90ef4b3f1fbe68354a09
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74576966"
---
# <a name="including-a-file-upload-option-when-adding-a-new-record-c"></a>Incluir uma opção de upload de arquivo ao adicionar um novo registro (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_56_CS.exe) ou [baixar PDF](including-a-file-upload-option-when-adding-a-new-record-cs/_static/datatutorial56cs1.pdf)

> Este tutorial mostra como criar uma interface da Web que permite ao usuário inserir dados de texto e carregar arquivos binários. Para ilustrar as opções disponíveis para armazenar dados binários, um arquivo será salvo no banco de dado enquanto o outro é armazenado no sistema de arquivos.

## <a name="introduction"></a>Introdução

Nos dois tutoriais anteriores, exploramos técnicas para armazenar dados binários associados ao modelo de dados do aplicativo, examinamos como usar o controle FileUpload para enviar arquivos do cliente para o servidor Web e vi como apresentar esses dados binários em uma data W controle EB. Ainda estamos falando sobre como associar dados carregados com o modelo de dados.

Neste tutorial, criaremos uma página da Web para adicionar uma nova categoria. Além das caixas de Textpara o nome e a descrição da categoria, essa página precisará incluir dois controles FileUpload um para a nova categoria s Picture e um para o folheto. A imagem carregada será armazenada diretamente na coluna novo registro s `Picture`, enquanto o folheto será salvo na pasta `~/Brochures` com o caminho para o arquivo salvo na coluna novo registro s `BrochurePath`.

Antes de criar essa nova página da Web, precisaremos atualizar a arquitetura. A consulta principal `CategoriesTableAdapter` s não recupera a coluna `Picture`. Consequentemente, o método de `Insert` gerado automaticamente só tem entradas para os campos `CategoryName`, `Description`e `BrochurePath`. Portanto, precisamos criar um método adicional no TableAdapter que solicita todos os quatro campos de `Categories`. A classe `CategoriesBLL` na camada de lógica de negócios também precisará ser atualizada.

## <a name="step-1-adding-aninsertwithpicturemethod-to-thecategoriestableadapter"></a>Etapa 1: adicionando um método de`InsertWithPicture`à`CategoriesTableAdapter`

Quando criamos o `CategoriesTableAdapter` de volta no tutorial [criando uma camada de acesso a dados](../introduction/creating-a-data-access-layer-cs.md) , configuramos-o para gerar automaticamente instruções `INSERT`, `UPDATE`e `DELETE` com base na consulta principal. Além disso, instruímos o TableAdapter a empregar a abordagem do DB Direct, que criou os métodos `Insert`, `Update`e `Delete`. Esses métodos executam as instruções `INSERT`, `UPDATE`e `DELETE` geradas automaticamente e, consequentemente, aceitam parâmetros de entrada com base nas colunas retornadas pela consulta principal. No tutorial [carregar arquivos](uploading-files-cs.md) , aumentamos a consulta principal `CategoriesTableAdapter` s para usar a coluna `BrochurePath`.

Como a consulta principal `CategoriesTableAdapter` s não faz referência à coluna `Picture`, não podemos adicionar um novo registro nem atualizar um registro existente com um valor para a coluna `Picture`. Para capturar essas informações, podemos criar um novo método no TableAdapter que é usado especificamente para inserir um registro com dados binários ou podemos personalizar a instrução de `INSERT` gerada automaticamente. O problema com a personalização da instrução de `INSERT` gerada automaticamente é que podemos arriscar a ter nossas personalizações substituídas pelo assistente. Por exemplo, imagine que personalizamos a instrução `INSERT` para incluir o uso da coluna `Picture`. Isso atualizaria o método TableAdapter s `Insert` para incluir um parâmetro de entrada adicional para os dados binários da categoria s Picture. Em seguida, poderíamos criar um método na camada de lógica de negócios para usar esse método DAL e invocar esse método BLL por meio da camada de apresentação, e tudo funcionará de forma maravilhosa. Ou seja, até a próxima vez que configurarmos o TableAdapter por meio do assistente de configuração do TableAdapter. Assim que o assistente for concluído, nossas personalizações para a instrução `INSERT` seriam substituídas, o método `Insert` reverteria para sua forma antiga e nosso código não será mais compilado!

> [!NOTE]
> Esse aborrecimento não é um problema ao usar procedimentos armazenados em vez de instruções SQL ad hoc. Um tutorial futuro irá explorar o uso de procedimentos armazenados no lugar de instruções SQL ad hoc na camada de acesso a dados.

Para evitar essa dor de cabeça potencial, em vez de personalizar as instruções SQL geradas automaticamente, deixe s criar um novo método para o TableAdapter. Esse método, chamado `InsertWithPicture`, aceitará valores para as colunas `CategoryName`, `Description`, `BrochurePath`e `Picture` e executará uma instrução `INSERT` que armazena todos os quatro valores em um novo registro.

Abra o DataSet tipado e, no designer, clique com o botão direito do mouse no cabeçalho `CategoriesTableAdapter` s e escolha Adicionar consulta no menu de contexto. Isso inicia o assistente de configuração de consulta do TableAdapter, que começa por nos perguntar como a consulta do TableAdapter deve acessar o banco de dados. Escolha usar instruções SQL e clique em Avançar. A próxima etapa solicita o tipo de consulta a ser gerada. Como recriamos uma consulta para adicionar um novo registro à tabela `Categories`, escolha Inserir e clique em Avançar.

[![selecionar a opção Inserir](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image1.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image1.png)

**Figura 1**: selecione a opção Inserir ([clique para exibir a imagem em tamanho normal](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image2.png))

Agora, precisamos especificar a instrução `INSERT` SQL. O assistente sugere automaticamente uma instrução `INSERT` correspondente à consulta principal do TableAdapter s. Nesse caso, é uma instrução `INSERT` que insere os valores `CategoryName`, `Description`e `BrochurePath`. Atualize a instrução para que a coluna `Picture` seja incluída junto com um parâmetro `@Picture`, desta forma:

[!code-sql[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample1.sql)]

A tela final do assistente nos pede para nomear o novo método TableAdapter. Insira `InsertWithPicture` e clique em concluir.

[![nomear o novo método TableAdapter InsertWithPicture](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image2.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image3.png)

**Figura 2**: nomeie o novo método TableAdapter `InsertWithPicture` ([clique para exibir a imagem em tamanho normal](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image4.png))

## <a name="step-2-updating-the-business-logic-layer"></a>Etapa 2: atualizando a camada de lógica de negócios

Como a camada de apresentação só deve interagir com a camada de lógica de negócios, em vez de ignorá-la para ir diretamente para a camada de acesso a dados, precisamos criar um método BLL que invoque o método DAL que acabamos de criar (`InsertWithPicture`). Para este tutorial, crie um método na classe `CategoriesBLL` chamada `InsertWithPicture` que aceita como entrada três `string` s e uma matriz de `byte`. Os parâmetros de entrada do `string` são para o nome, a descrição e o caminho do arquivo de folheto da categoria, enquanto a matriz `byte` é para o conteúdo binário da categoria s. Como mostra o código a seguir, esse método BLL invoca o método DAL correspondente:

[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample2.cs)]

> [!NOTE]
> Certifique-se de que você salvou o DataSet tipado antes de adicionar o método `InsertWithPicture` à BLL. Como o código de classe `CategoriesTableAdapter` é gerado automaticamente com base no conjunto de dado tipado, se você não salvar primeiro as alterações no conjunto de dado tipado, a propriedade `Adapter` não saberá sobre o método `InsertWithPicture`.

## <a name="step-3-listing-the-existing-categories-and-their-binary-data"></a>Etapa 3: listando as categorias existentes e seus dados binários

Neste tutorial, criaremos uma página que permite que um usuário final adicione uma nova categoria ao sistema, fornecendo uma imagem e um folheto para a nova categoria. No [tutorial anterior](displaying-binary-data-in-the-data-web-controls-cs.md) , usamos um GridView com um TemplateField e ImageField para exibir a cada categoria nome, descrição, imagem e um link para baixar seu folheto. Deixe que o s replique essa funcionalidade para este tutorial, criando uma página que lista todas as categorias existentes e permite que novas sejam criadas.

Comece abrindo a página de `DisplayOrDownload.aspx` da pasta `BinaryData`. Vá para a exibição de código-fonte e copie a sintaxe declarativa de GridView e ObjectDataSource s, colando-a dentro do elemento `<asp:Content>` no `UploadInDetailsView.aspx`. Além disso, Don t não se esqueça de copiar sobre o método `GenerateBrochureLink` da classe code-behind de `DisplayOrDownload.aspx` para `UploadInDetailsView.aspx`.

[![copiar e colar a sintaxe declarativa de DisplayOrDownload. aspx em UploadInDetailsView. aspx](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image3.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image5.png)

**Figura 3**: copiar e colar a sintaxe declarativa de `DisplayOrDownload.aspx` para `UploadInDetailsView.aspx` ([clique para exibir a imagem em tamanho normal](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image6.png))

Depois de copiar a sintaxe declarativa e `GenerateBrochureLink` método para a página `UploadInDetailsView.aspx`, exiba a página por meio de um navegador para garantir que tudo tenha sido copiado corretamente. Você deve ver um GridView listando as oito categorias que incluem um link para baixar o folheto, bem como a categoria s Picture.

[![agora você deve ver cada categoria junto com seus dados binários](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image4.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image7.png)

**Figura 4**: agora você deve ver cada categoria junto com seus dados binários ([clique para exibir a imagem em tamanho normal](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image8.png))

## <a name="step-4-configuring-thecategoriesdatasourceto-support-inserting"></a>Etapa 4: Configurando o`CategoriesDataSource`para dar suporte à inserção

O `CategoriesDataSource` ObjectDataSource usado pelo `Categories` GridView atualmente não fornece a capacidade de inserir dados. Para dar suporte à inserção por meio desse controle de fonte de dados, é necessário mapear seu método `Insert` para um método em seu objeto subjacente, `CategoriesBLL`. Em particular, queremos mapeá-lo para o método `CategoriesBLL` que adicionamos de volta na etapa 2, `InsertWithPicture`.

Comece clicando no link configurar fonte de dados da marca inteligente ObjectDataSource s. A primeira tela mostra o objeto com o qual a fonte de dados está configurada para trabalhar, `CategoriesBLL`. Deixe essa configuração como está e clique em avançar para ir para a tela definir métodos de dados. Mova para a guia Inserir e escolha o método `InsertWithPicture` na lista suspensa. Clique em Concluir para concluir o assistente.

[![configurar o ObjectDataSource para usar o método InsertWithPicture](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image5.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image9.png)

**Figura 5**: configurar o ObjectDataSource para usar o método `InsertWithPicture` ([clique para exibir a imagem em tamanho normal](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image10.png))

> [!NOTE]
> Ao concluir o assistente, o Visual Studio pode perguntar se você deseja atualizar campos e chaves, o que irá gerar os campos de controles da Web de dados. Escolha não, porque a escolha de Sim substituirá as personalizações de campo que você tenha feito.

Depois de concluir o assistente, o ObjectDataSource agora incluirá um valor para sua propriedade `InsertMethod`, bem como `InsertParameters` para as colunas de quatro categorias, como ilustra a seguinte marcação declarativa:

[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample3.aspx)]

## <a name="step-5-creating-the-inserting-interface"></a>Etapa 5: criando a interface de inserção

Como abordado pela primeira vez em [uma visão geral da inserção, atualização e exclusão de dados](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md), o controle DetailsView fornece uma interface de inserção interna que pode ser utilizada ao trabalhar com um controle da fonte de dados que dá suporte à inserção. Vamos adicionar um controle DetailsView a esta página acima do GridView que renderizará permanentemente sua interface de inserção, permitindo que um usuário adicione rapidamente uma nova categoria. Após a adição de uma nova categoria no DetailsView, o GridView abaixo será atualizado automaticamente e exibirá a nova categoria.

Comece arrastando um DetailsView da caixa de ferramentas para o designer acima de GridView, definindo sua propriedade `ID` como `NewCategory` e limpando os valores de propriedade `Height` e `Width`. Na marca inteligente DetailsView s, associe-o à `CategoriesDataSource` existente e marque a caixa de seleção Habilitar inserção.

[![associar o DetailsView ao CategoriesDataSource e habilitar a inserção](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image6.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image11.png)

**Figura 6**: associar o DetailsView ao `CategoriesDataSource` e habilitar a inserção ([clique para exibir a imagem em tamanho normal](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image12.png))

Para renderizar permanentemente o DetailsView em sua interface de inserção, defina sua propriedade `DefaultMode` como `Insert`.

Observe que o DetailsView tem cinco BoundFields `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`e `BrochurePath`, embora o `CategoryID` BoundField não seja renderizado na interface de inserção porque sua propriedade `InsertVisible` está definida como `false`. Esses BoundFields existem porque são as colunas retornadas pelo método `GetCategories()`, que é o que o ObjectDataSource invoca para recuperar seus dados. Para inserir, no entanto, não queremos deixar que o usuário especifique um valor para `NumberOfProducts`. Além disso, precisamos permitir que eles carreguem uma imagem para a nova categoria, bem como carreguem um PDF para o folheto.

Remova o `NumberOfProducts` BoundField do DetailsView completamente e, em seguida, atualize as propriedades `HeaderText` da `CategoryName` e `BrochurePath` BoundFields para categoria e folheto, respectivamente. Em seguida, converta o `BrochurePath` BoundField em um TemplateField e adicione um novo TemplateField para a imagem, dando a ele um valor de `HeaderText` de imagem. Mova o `Picture` TemplateField para que ele esteja entre o `BrochurePath` TemplateField e CommandField.

![Associar o DetailsView ao CategoriesDataSource e habilitar a inserção](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image7.gif)

**Figura 7**: associar o DetailsView ao `CategoriesDataSource` e habilitar a inserção

Se você converteu o `BrochurePath` BoundField em um TemplateField por meio da caixa de diálogo Editar campos, o TemplateField inclui um `ItemTemplate`, `EditItemTemplate`e `InsertItemTemplate`. No entanto, somente o `InsertItemTemplate` é necessário, portanto, fique à vontade para remover os outros dois modelos. Neste ponto, a sintaxe declarativa de DetailsView s deve ser semelhante ao seguinte:

[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample4.aspx)]

## <a name="adding-fileupload-controls-for-the-brochure-and-picture-fields"></a>Adicionando controles FileUpload para os campos de folheto e imagem

Atualmente, a `BrochurePath` TemplateField s `InsertItemTemplate` contém uma caixa de texto, enquanto a `Picture` TemplateField não contém modelos. Precisamos atualizar esses dois TemplateField s `InsertItemTemplate` s para usar os controles FileUpload.

Na marca inteligente DetailsView s, escolha a opção Editar modelos e, em seguida, selecione o `BrochurePath` `InsertItemTemplate` TemplateField na lista suspensa. Remova a caixa de texto e arraste um controle FileUpload da caixa de ferramentas para o modelo. Defina as `ID` s de controle FileUpload como `BrochureUpload`. Da mesma forma, adicione um controle FileUpload à `Picture` `InsertItemTemplate`TemplateField. Defina esta `ID` de controle do FileUpload como `PictureUpload`.

[![adicionar um controle FileUpload ao InsertItemTemplate](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image8.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image13.png)

**Figura 8**: adicionar um controle FileUpload ao `InsertItemTemplate` ([clique para exibir a imagem em tamanho normal](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image14.png))

Depois de fazer essas adições, a sintaxe declarativa de duas TemplateField s será:

[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample5.aspx)]

Quando um usuário adiciona uma nova categoria, queremos garantir que o folheto e a imagem sejam do tipo de arquivo correto. Para o folheto, o usuário deve fornecer um PDF. Para a imagem, precisamos que o usuário carregue um arquivo de imagem, mas permitimos *qualquer* arquivo de imagem ou apenas arquivos de imagem de um tipo específico, como GIFs ou JPGs? Para permitir diferentes tipos de arquivo, nós d precisamos estender o esquema de `Categories` para incluir uma coluna que captura o tipo de arquivo para que esse tipo possa ser enviado ao cliente por meio de `Response.ContentType` no `DisplayCategoryPicture.aspx`. Como não temos essa coluna, seria prudente restringir os usuários a fornecer apenas um tipo de arquivo de imagem específico. As imagens existentes da tabela de `Categories` são bitmaps, mas JPGs são um formato de arquivo mais apropriado para imagens servidas pela Web.

Se um usuário carregar um tipo de arquivo incorreto, será necessário cancelar a inserção e exibir uma mensagem indicando o problema. Adicione um controle de rótulo da Web sob o DetailsView. Defina sua propriedade `ID` como `UploadWarning`, desmarque sua propriedade `Text`, defina a propriedade `CssClass` como aviso e as propriedades `Visible` e `EnableViewState` como `false`. A classe CSS `Warning` é definida em `Styles.css` e renderiza o texto em uma fonte grande, vermelha, em itálico e em negrito.

> [!NOTE]
> O ideal é que o `CategoryName` e `Description` BoundFields sejam convertidos em TemplateFields e suas interfaces de inserção fossem personalizadas. O `Description` inserir a interface, por exemplo, provavelmente seria mais adequado por meio de uma caixa de texto de várias linhas. E como a coluna `CategoryName` não aceita valores de `NULL`, um RequiredFieldValidator deve ser adicionado para garantir que o usuário forneça um valor para o nome da nova categoria. Essas etapas são deixadas como um exercício para o leitor. Consulte de volta para [Personalizar a interface de modificação de dados](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) para uma análise detalhada do aumento das interfaces de modificação de dados.

## <a name="step-6-saving-the-uploaded-brochure-to-the-web-server-s-file-system"></a>Etapa 6: salvando o folheto carregado no sistema de arquivos do servidor Web

Quando o usuário insere os valores para uma nova categoria e clica no botão Inserir, ocorre um postback e o fluxo de trabalho de inserção é desdobrado. Primeiro, o evento de [`ItemInserting`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserting.aspx) de DetailsView s é acionado. Em seguida, o método ObjectDataSource s `Insert()` é invocado, o que resulta em um novo registro sendo adicionado à tabela `Categories`. Depois disso, o evento de [`ItemInserted`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserted.aspx) de DetailsView s é acionado.

Antes que o método ObjectDataSource s `Insert()` seja invocado, primeiro devemos garantir que os tipos de arquivo apropriados foram carregados pelo usuário e, em seguida, salvar o PDF do folheto no sistema de arquivos do servidor Web. Crie um manipulador de eventos para o evento de `ItemInserting` de DetailsView s e adicione o seguinte código:

[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample6.cs)]

O manipulador de eventos começa referenciando o controle FileUpload `BrochureUpload` dos modelos DetailsView s. Em seguida, se um folheto tiver sido carregado, a extensão de s do arquivo carregado será examinada. Se a extensão não for. PDF, um aviso é exibido, a inserção é cancelada e a execução do manipulador de eventos termina.

> [!NOTE]
> Depender da extensão de s do arquivo carregado não é uma técnica segura para garantir que o arquivo carregado seja um documento PDF. O usuário pode ter um documento PDF válido com a extensão `.Brochure`ou pode ter tomado um documento não PDF e ter dado a ele uma extensão `.pdf`. O conteúdo binário de arquivo s precisaria ser examinado programaticamente para verificar o tipo de arquivo de forma mais conclusiva. No entanto, essas abordagens completas costumam ser um exagero; verificar a extensão é suficiente para a maioria dos cenários.

Conforme discutido no tutorial [carregar arquivos](uploading-files-cs.md) , é necessário ter cuidado ao salvar arquivos no sistema de arquivos para que o upload de um usuário não substitua outro s. Para este tutorial, tentaremos usar o mesmo nome que o arquivo carregado. Se já existir um arquivo no diretório `~/Brochures` com esse mesmo nome de arquivo, no entanto, vamos acrescentar um número no final até que um nome exclusivo seja encontrado. Por exemplo, se o usuário carregar um arquivo de folheto chamado `Meats.pdf`, mas já houver um arquivo chamado `Meats.pdf` na pasta `~/Brochures`, alteraremos o nome do arquivo salvo para `Meats-1.pdf`. Se isso existir, tentaremos `Meats-2.pdf`e assim por diante, até que um nome de arquivo exclusivo seja encontrado.

O código a seguir usa o [método`File.Exists(path)`](https://msdn.microsoft.com/library/system.io.file.exists.aspx) para determinar se já existe um arquivo com o nome de arquivo especificado. Nesse caso, ele continua tentando novos nomes de arquivo para o folheto até que nenhum conflito seja encontrado.

[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample7.cs)]

Depois que um nome de arquivo válido for encontrado, o arquivo precisará ser salvo no sistema de arquivos e o valor de `brochurePath``InsertParameter` ObjectDataSource s precisa ser atualizado para que esse nome de arquivo seja gravado no banco de dados. Como vimos no tutorial de *upload de arquivos* , o arquivo pode ser salvo usando o método de `SaveAs(path)` do controle FileUpload. Para atualizar o parâmetro ObjectDataSource s `brochurePath`, use a coleção `e.Values`.

[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample8.cs)]

## <a name="step-7-saving-the-uploaded-picture-to-the-database"></a>Etapa 7: salvar a imagem carregada no banco de dados

Para armazenar a imagem carregada no novo registro de `Categories`, é necessário atribuir o conteúdo binário carregado ao parâmetro ObjectDataSource s `picture` no evento de `ItemInserting` DetailsView. Antes de fazermos essa atribuição, no entanto, precisamos primeiro verificar se a imagem carregada é um JPG e não outro tipo de imagem. Como na etapa 6, vamos usar a extensão de arquivo da imagem carregada para verificar seu tipo.

Enquanto a tabela `Categories` permite `NULL` valores para a coluna `Picture`, todas as categorias atualmente têm uma imagem. Vamos forçar o usuário a fornecer uma imagem ao adicionar uma nova categoria por meio desta página. O código a seguir verifica se uma imagem foi carregada e se tem uma extensão apropriada.

[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample9.cs)]

Esse código deve ser colocado *antes* do código da etapa 6 para que, se houver um problema com o carregamento da imagem, o manipulador de eventos seja encerrado antes que o arquivo de folheto seja salvo no sistema de arquivos.

Supondo que um arquivo apropriado tenha sido carregado, atribua o conteúdo binário carregado ao valor do parâmetro de figura s com a seguinte linha de código:

[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample10.cs)]

## <a name="the-completeiteminsertingevent-handler"></a>O manipulador de eventos de`ItemInserting`completo

Para fins de integridade, aqui está o manipulador de eventos `ItemInserting` em sua totalidade:

[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample11.cs)]

## <a name="step-8-fixing-thedisplaycategorypictureaspxpage"></a>Etapa 8: corrigindo a página de`DisplayCategoryPicture.aspx`

Vamos reservar um momento para testar a interface de inserção e `ItemInserting` manipulador de eventos que foi criado nas últimas etapas. Visite a página `UploadInDetailsView.aspx` por meio de um navegador e tente adicionar uma categoria, mas omita a imagem ou especifique uma imagem não JPG ou um folheto não-PDF. Em qualquer um desses casos, uma mensagem de erro será exibida e o fluxo de trabalho de inserção foi cancelado.

[![uma mensagem de aviso será exibida se um tipo de arquivo inválido for carregado](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image9.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image15.png)

**Figura 9**: uma mensagem de aviso será exibida se um tipo de arquivo inválido for carregado ([clique para exibir a imagem em tamanho normal](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image16.png))

Depois de verificar se a página requer que uma imagem seja carregada e não aceite arquivos não PDF ou não JPG, adicione uma nova categoria com uma imagem JPG válida, deixando o campo de folheto vazio. Depois de clicar no botão Inserir, a página será renovada e um novo registro será adicionado à tabela de `Categories` com o conteúdo binário da imagem carregada armazenada diretamente no banco de dados. O GridView é atualizado e mostra uma linha para a categoria recém-adicionada, mas, como mostra a Figura 10, a nova figura da categoria s não é renderizada corretamente.

[![a nova imagem da categoria s não é exibida](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image10.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image17.png)

**Figura 10**: a nova imagem da categoria s não é exibida ([clique para exibir a imagem em tamanho normal](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image18.png))

O motivo pelo qual a nova imagem não é exibida é porque a página de `DisplayCategoryPicture.aspx` que retorna uma categoria de figura especificada é configurada para processar bitmaps que têm um cabeçalho OLE. Esse cabeçalho de 78 bytes é removido do conteúdo binário da coluna de `Picture` antes de serem enviados de volta ao cliente. Mas o arquivo JPG que acabamos de carregar para a nova categoria não tem esse cabeçalho OLE; Portanto, os bytes necessários válidos estão sendo removidos dos dados binários da imagem.

Como agora há os dois bitmaps com cabeçalhos OLE e JPGs na tabela `Categories`, precisamos atualizar `DisplayCategoryPicture.aspx` para que ele faça o cabeçalho OLE para as oito categorias originais e ignore essa remoção para os registros de categoria mais recentes. Em nosso próximo tutorial, examinaremos como atualizar uma imagem de s de registro existente e atualizaremos todas as fotos de categoria antigas para que elas sejam JPGs. Por enquanto, no entanto, use o código a seguir em `DisplayCategoryPicture.aspx` para retirar os cabeçalhos OLE apenas das oito categorias originais:

[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample12.cs)]

Com essa alteração, a imagem JPG agora é renderizada corretamente no GridView.

[![as imagens do JPG para novas categorias são processadas corretamente](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image11.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image19.png)

**Figura 11**: as imagens do jpg para novas categorias são processadas corretamente ([clique para exibir a imagem em tamanho normal](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image20.png))

## <a name="step-9-deleting-the-brochure-in-the-face-of-an-exception"></a>Etapa 9: excluindo o folheto na face de uma exceção

Um dos desafios de armazenar dados binários no sistema de arquivos do servidor Web é que ele introduz uma desconexão entre o modelo de dados e seus dados binários. Portanto, sempre que um registro é excluído, os dados binários correspondentes no sistema de arquivos também devem ser removidos. Isso também pode ser tocado ao inserir. Considere o seguinte cenário: um usuário adiciona uma nova categoria, especificando uma imagem e um folheto válidos. Ao clicar no botão Inserir, um postback ocorre e o evento de `ItemInserting` de DetailsView s é acionado, salvando o folheto no sistema de arquivos do servidor Web. Em seguida, o método ObjectDataSource s `Insert()` é invocado, que chama o método `CategoriesBLL` Class s `InsertWithPicture`, que chama o método `CategoriesTableAdapter` s `InsertWithPicture`.

Agora, o que acontece se o banco de dados estiver offline ou se houver um erro na instrução `INSERT` SQL? Claramente, a inserção falhará, portanto, nenhuma nova linha de categoria será adicionada ao banco de dados. Mas ainda temos o arquivo de folheto carregado localizado no sistema de arquivos do servidor Web! Esse arquivo precisa ser excluído diante de uma exceção durante o fluxo de trabalho de inserção.

Conforme discutido anteriormente na [manipulação de exceções de nível de BLL e Dal em um tutorial de página do ASP.net](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) , quando uma exceção é lançada de dentro das profundidades da arquitetura, ela é propagada pelas várias camadas. Na camada de apresentação, podemos determinar se uma exceção ocorreu no evento de `ItemInserted` de DetailsView s. Esse manipulador de eventos também fornece os valores da `InsertParameters`ObjectDataSource s. Portanto, podemos criar um manipulador de eventos para o evento `ItemInserted` que verifica se houve uma exceção e, em caso afirmativo, exclui o arquivo especificado pelo parâmetro ObjectDataSource s `brochurePath`:

[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample13.cs)]

## <a name="summary"></a>Resumo

Há uma série de etapas que devem ser executadas para fornecer uma interface baseada na Web para a adição de registros que incluem dados binários. Se os dados binários estiverem sendo armazenados diretamente no banco de dado, é provável que você precise atualizar a arquitetura, adicionando métodos específicos para lidar com o caso em que os dados binários estão sendo inseridos. Depois que a arquitetura tiver sido atualizada, a próxima etapa será criar a interface de inserção, que pode ser realizada usando um DetailsView que foi personalizado para incluir um controle FileUpload para cada campo de dados binários. Os dados carregados podem então ser salvos no sistema de arquivos do servidor Web ou atribuídos a um parâmetro de fonte de dados no manipulador de eventos de `ItemInserting` de DetailsView.

Salvar dados binários no sistema de arquivos requer mais planejamento do que salvar dados diretamente no banco de dado. Um esquema de nomenclatura deve ser escolhido para evitar que um usuário faça o upload substituindo outros s. Além disso, etapas adicionais devem ser executadas para excluir o arquivo carregado se a inserção do banco de dados falhar.

Agora, temos a capacidade de adicionar novas categorias ao sistema com um folheto e uma imagem, mas ainda veremos como atualizar uma categoria existente de dados binários existentes ou como remover corretamente os dados binários de uma categoria excluída. Exploraremos esses dois tópicos no próximo tutorial.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores potenciais para este tutorial foram Dave Gardner, Teresa Murphy e Bernadette Leigh. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](displaying-binary-data-in-the-data-web-controls-cs.md)
> [Próximo](updating-and-deleting-existing-binary-data-cs.md)
