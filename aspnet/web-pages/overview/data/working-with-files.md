---
uid: web-pages/overview/data/working-with-files
title: Trabalhando com arquivos em um site Páginas da Web do ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Este capítulo explica como ler, gravar, acrescentar, excluir e carregar arquivos.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: eee916e4-ba4c-439a-a24e-68df7d45a569
msc.legacyurl: /web-pages/overview/data/working-with-files
msc.type: authoredcontent
ms.openlocfilehash: 684c47a8a8480dc040e5144144577c94c35d39e5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78642362"
---
# <a name="working-with-files-in-an-aspnet-web-pages-razor-site"></a>Trabalhando com arquivos em um site Páginas da Web do ASP.NET (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo explica como ler, gravar, acrescentar, excluir e carregar arquivos em um site Páginas da Web do ASP.NET (Razor).
> 
> > [!NOTE]
> > Se você quiser carregar imagens e manipulá-las (por exemplo, inverter ou redimensioná-las), consulte [trabalhando com imagens em um Site páginas da Web do ASP.net](/aspnet/web-pages/overview/ui-layouts-and-themes/9-working-with-images).
> 
> 
> **O que você aprenderá:** 
> 
> - Como criar um arquivo de texto e gravar dados nele.
> - Como acrescentar dados a um arquivo existente.
> - Como ler um arquivo e exibi-lo.
> - Como excluir arquivos de um site.
> - Como permitir que os usuários carreguem um arquivo ou vários arquivos.
> 
> Estes são os recursos de programação do ASP.NET apresentados no artigo:
> 
> - O objeto `File`, que fornece uma maneira de gerenciar arquivos.
> - O auxiliar de `FileUpload`.
> - O objeto `Path`, que fornece métodos que permitem manipular nomes de arquivo e caminho.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 2
> - WebMatrix 2
>   
> 
> Este tutorial também funciona com o WebMatrix 3.

<a id="Creating_a_Text_File"></a>
## <a name="creating-a-text-file-and-writing-data-to-it"></a>Criando um arquivo de texto e gravando dados nele

Além de usar um banco de dados em seu site, você pode trabalhar com arquivos. Por exemplo, você pode usar arquivos de texto como uma maneira simples de armazenar dados para o site. (Um arquivo de texto que é usado para armazenar dados é, às vezes, chamado de *arquivo simples*.) Os arquivos de texto podem estar em formatos diferentes, como *. txt*, *. xml*ou *. csv* (valores delimitados por vírgula).

Se você quiser armazenar dados em um arquivo de texto, poderá usar o método `File.WriteAllText` para especificar o arquivo a ser criado e os dados a serem gravados nele. Neste procedimento, você criará uma página que contém um formulário simples com três elementos `input` (nome, sobrenome e endereço de email) e um botão **Enviar** . Quando o usuário enviar o formulário, você armazenará a entrada do usuário em um arquivo de texto.

1. Crie uma nova pasta chamada *App\_data*, caso ela ainda não exista.
2. Na raiz do seu site, crie um novo arquivo chamado *UserData. cshtml*.
3. Substitua o conteúdo existente pelo seguinte: 

    [!code-cshtml[Main](working-with-files/samples/sample1.cshtml)]

    A marcação HTML cria o formulário com as três caixas de texto. No código, você usa a propriedade `IsPost` para determinar se a página foi enviada antes de iniciar o processamento.

    A primeira tarefa é obter a entrada do usuário e atribuí-la a variáveis. Em seguida, o código concatena os valores das variáveis separadas em uma cadeia de caracteres delimitada por vírgula, que é armazenada em uma variável diferente. Observe que o separador de vírgula é uma cadeia de caracteres contida em aspas (","), porque você está literalmente inserindo uma vírgula na cadeia de caracteres grande que você está criando. No final dos dados que você concatena em conjunto, você adiciona `Environment.NewLine`. Isso adiciona uma quebra de linha (um caractere de linha nova). O que você está criando com toda essa concatenação é uma cadeia de caracteres parecida com esta:

    [!code-css[Main](working-with-files/samples/sample2.css)]

    (Com uma quebra de linha invisível no final.)

    Em seguida, você cria uma variável (`dataFile`) que contém o local e o nome do arquivo no qual armazenar os dados. A configuração do local requer um tratamento especial. Nos sites, é uma prática inadequada fazer referência ao código para caminhos absolutos, como *C:\Folder\File.txt* para arquivos no servidor Web. Se um site for movido, um caminho absoluto estará incorreto. Além disso, para um site hospedado (em vez de em seu próprio computador), normalmente, você não sabe qual é o caminho correto quando está escrevendo o código.

    Mas, às vezes, (como agora, para gravar um arquivo), você precisa de um caminho completo. A solução é usar o método `MapPath` do objeto `Server`. Isso retorna o caminho completo para seu site. Para obter o caminho para a raiz do site, você faz com que o operador de `~` (represe a raiz virtual do site) para `MapPath`. (Você também pode passar um nome de subpasta para ele, como *~/App\_data/* , para obter o caminho para essa subpasta.) Em seguida, você pode concatenar informações adicionais em qualquer coisa que o método retornar para criar um caminho completo. Neste exemplo, você adiciona um nome de arquivo. (Você pode ler mais sobre como trabalhar com caminhos de arquivos e pastas na [introdução à programação de páginas da Web do ASP.NET usando a sintaxe do Razor](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths).)

    O arquivo é salvo na pasta de *dados do\_de aplicativos* . Essa pasta é uma pasta especial no ASP.NET que é usada para armazenar arquivos de dados, conforme descrito em [introdução ao trabalho com um banco de dado em Sites páginas da Web do ASP.net](https://go.microsoft.com/fwlink/?LinkId=195209).

    O método `WriteAllText` do objeto `File` grava os dados no arquivo. Esse método usa dois parâmetros: o nome (com caminho) do arquivo a ser gravado e os dados reais a serem gravados. Observe que o nome do primeiro parâmetro tem um caractere de `@` como um prefixo. Isso informa ao ASP.NET que você está fornecendo um literal de cadeia de caracteres textual e que os caracteres como "/" não devem ser interpretados de maneiras especiais. (Para obter mais informações, consulte [introdução à programação da Web do ASP.NET usando a sintaxe do Razor](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths).)

    > [!NOTE]
    > Para que seu código salve arquivos na pasta de *dados do\_de aplicativos* , o aplicativo precisa de permissões de leitura/gravação para essa pasta. Em seu computador de desenvolvimento, isso normalmente não é um problema. No entanto, quando você publica seu site no servidor Web de um provedor de hospedagem, talvez seja necessário definir explicitamente essas permissões. Se você executar esse código em um servidor do provedor de hospedagem e receber erros, verifique com o provedor de hospedagem para descobrir como definir essas permissões.

- Execute a página em um navegador. 

    ![](working-with-files/_static/image1.jpg)
- Insira valores nos campos e clique em **Enviar**.
- Feche o navegador.
- Retorne ao projeto e atualize a exibição.
- Abra o arquivo *Data. txt* . Os dados que você enviou no formulário estão no arquivo. 

    ![[imagem]](working-with-files/_static/image2.jpg)
- Feche o arquivo *Data. txt* .

<a id="Appending_Data"></a>
## <a name="appending-data-to-an-existing-file"></a>Anexando dados a um arquivo existente

No exemplo anterior, você usou `WriteAllText` para criar um arquivo de texto que tem apenas uma parte dos dados nele. Se você chamar o método novamente e passá-lo para o mesmo nome de arquivo, o arquivo existente será completamente substituído. No entanto, depois de criar um arquivo, você geralmente deseja adicionar novos dados ao final do arquivo. Você pode fazer isso usando o método `AppendAllText` do objeto `File`.

1. No site, faça uma cópia do arquivo *UserData. cshtml* e nomeie a cópia *UserDataMultiple. cshtml*.
2. Substitua o bloco de código antes de abrir a marca de `<!DOCTYPE html>` com o seguinte bloco de código: 

    [!code-cshtml[Main](working-with-files/samples/sample3.cshtml)]

    Esse código tem uma alteração no exemplo anterior. Em vez de usar `WriteAllText`, ele usa `the AppendAllText` método. Os métodos são semelhantes, exceto que `AppendAllText` adiciona os dados ao final do arquivo. Assim como ocorre com `WriteAllText`, `AppendAllText` criará o arquivo se ele ainda não existir.
3. Execute a página em um navegador.
4. Insira valores para os campos e clique em **Enviar**.
5. Adicione mais dados e envie o formulário novamente.
6. Retorne ao seu projeto, clique com o botão direito do mouse na pasta do projeto e clique em **Atualizar**.
7. Abra o arquivo *Data. txt* . Agora ele contém os novos dados que você acabou de inserir. 

    ![[imagem]](working-with-files/_static/image3.jpg)

<a id="Reading_and_Displaying_Data"></a>
## <a name="reading-and-displaying-data-from-a-file"></a>Lendo e exibindo dados de um arquivo

Mesmo que você não precise gravar dados em um arquivo de texto, às vezes, você provavelmente precisará ler dados de um. Para fazer isso, você pode usar novamente o objeto `File`. Você pode usar o objeto `File` para ler cada linha individualmente (separada por quebras de linha) ou para ler o item individual, independentemente de como eles são separados.

Este procedimento mostra como ler e exibir os dados que você criou no exemplo anterior.

1. Na raiz do seu site, crie um novo arquivo chamado *DisplayData. cshtml*.
2. Substitua o conteúdo existente pelo seguinte: 

    [!code-cshtml[Main](working-with-files/samples/sample4.cshtml)]

    O código começa lendo o arquivo que você criou no exemplo anterior em uma variável chamada `userData`, usando essa chamada de método:

    [!code-css[Main](working-with-files/samples/sample5.css)]

    O código para fazer isso está dentro de uma instrução de `if`. Quando você deseja ler um arquivo, é uma boa ideia usar o método `File.Exists` para determinar primeiro se o arquivo está disponível. O código também verifica se o arquivo está vazio.

    O corpo da página contém dois loops de `foreach`, um aninhado dentro do outro. O loop de `foreach` externo Obtém uma linha por vez do arquivo de dados. Nesse caso, as linhas são definidas por quebras de linha no arquivo &#8212; , ou seja, cada item de dados está em sua própria linha. O loop externo cria um novo item (`<li>` elemento) dentro de uma lista ordenada (elemento`<ol>`).

    O loop interno divide cada linha de dados em itens (campos) usando uma vírgula como um delimitador. (Com base no exemplo anterior, isso significa que cada linha contém três campos &#8212; , o nome, o sobrenome e o endereço de email, cada um separado por uma vírgula.) O loop interno também cria uma lista de `<ul>` e exibe um item de lista para cada campo na linha de dados.

    O código ilustra como usar dois tipos de dados, uma matriz e o tipo de dados `char`. A matriz é necessária porque o método `File.ReadAllLines` retorna dados como uma matriz. O tipo de dados `char` é necessário porque o método `Split` retorna uma `array` na qual cada elemento é do tipo `char`. (Para obter informações sobre matrizes, consulte [introdução à programação da Web do ASP.NET usando a sintaxe do Razor](https://go.microsoft.com/fwlink/?LinkId=202890#ID_CollectionsAndObjects).)
3. Execute a página em um navegador. Os dados que você inseriu para os exemplos anteriores são exibidos. 

    ![[imagem]](working-with-files/_static/image4.jpg)

> [!TIP] 
> 
> **Exibindo dados de um arquivo delimitado por vírgulas do Microsoft Excel**
> 
> Você pode usar o Microsoft Excel para salvar os dados contidos em uma planilha como um arquivo delimitado por vírgula (arquivo *. csv* ). Quando você faz isso, o arquivo é salvo em texto sem formatação, não no formato do Excel. Cada linha na planilha é separada por uma quebra de linha no arquivo de texto, e cada item de dados é separado por uma vírgula. Você pode usar o código mostrado no exemplo anterior para ler um arquivo delimitado por vírgulas do Excel apenas alterando o nome do arquivo de dados em seu código.

<a id="Deleting_Files"></a>
## <a name="deleting-files"></a>Excluindo arquivos

Para excluir arquivos do seu site, você pode usar o método `File.Delete`. Este procedimento mostra como permitir que os usuários excluam uma imagem (arquivo *. jpg* ) de uma pasta de *imagens* se saberem o nome do arquivo.

> [!NOTE] 
> 
> **Importante** Em um site de produção, você normalmente restringe quem tem permissão para fazer alterações nos dados. Para obter informações sobre como configurar a associação e sobre maneiras de autorizar os usuários a executar tarefas no site, consulte [adicionando segurança e associação a um site páginas da Web do ASP.net](https://go.microsoft.com/fwlink/?LinkId=202904).

1. No site, crie uma subpasta denominada *imagens*.
2. Copie um ou mais arquivos *. jpg* na pasta *imagens* .
3. Na raiz do site, crie um novo arquivo chamado *filedelete. cshtml*.
4. Substitua o conteúdo existente pelo seguinte: 

    [!code-cshtml[Main](working-with-files/samples/sample6.cshtml)]

    Esta página contém um formulário onde os usuários podem inserir o nome de um arquivo de imagem. Eles não inserem a extensão de nome de arquivo *. jpg* ; ao restringir o nome de arquivo como este, você ajuda a impedir que os usuários excluam arquivos arbitrários no seu site.

    O código lê o nome do arquivo que o usuário inseriu e, em seguida, constrói um caminho completo. Para criar o caminho, o código usa o caminho do site atual (como retornado pelo método `Server.MapPath`), o nome da pasta *imagens* , o nome que o usuário forneceu e ". jpg" como uma cadeia de caracteres literal.

    Para excluir o arquivo, o código chama o método `File.Delete`, passando-o para o caminho completo que você acabou de construir. No final da marcação, o código exibe uma mensagem de confirmação de que o arquivo foi excluído.
5. Execute a página em um navegador. 

    ![[imagem]](working-with-files/_static/image5.jpg)
6. Insira o nome do arquivo a ser excluído e clique em **Enviar**. Se o arquivo tiver sido excluído, o nome do arquivo será exibido na parte inferior da página.

<a id="Letting_Users_Upload_a_File"></a>
## <a name="letting-users-upload-a-file"></a>Permitindo que os usuários carreguem um arquivo

O auxiliar de `FileUpload` permite que os usuários carreguem arquivos em seu site. O procedimento a seguir mostra como permitir que os usuários carreguem um único arquivo.

1. Adicione a biblioteca de auxiliares Web do ASP.NET ao seu site, conforme descrito em [instalando auxiliares em um páginas da Web do ASP.net site](https://go.microsoft.com/fwlink/?LinkId=252372), se você não o adicionou anteriormente.
2. Na pasta *dados do\_de aplicativos* , crie uma nova pasta e nomeie-a *uploadedFiles*.
3. Na raiz, crie um novo arquivo chamado *FileUpload. cshtml*.
4. Substitua o conteúdo existente na página pelo seguinte: 

    [!code-cshtml[Main](working-with-files/samples/sample7.cshtml)]

    A parte do corpo da página usa o `FileUpload` auxiliar para criar a caixa de carregamento e os botões com os quais você provavelmente está familiarizado:

    ![[imagem]](working-with-files/_static/image6.jpg)

    As propriedades definidas para o auxiliar de `FileUpload` especificam que você deseja uma única caixa para o arquivo carregar e que deseja que o botão enviar para ler o **carregamento**. (Você adicionará mais caixas posteriormente neste artigo.)

    Quando o usuário clica em **carregar**, o código na parte superior da página Obtém o arquivo e o salva. O objeto `Request` que você normalmente usa para obter valores de campos de formulário também tem uma matriz `Files` que contém o arquivo (ou arquivos) que foram carregados. Você pode obter arquivos individuais fora de posições específicas na matriz &#8212; , por exemplo, para obter o primeiro arquivo carregado, você obtém `Request.Files[0]`, para obter o segundo arquivo, você obtém `Request.Files[1]`e assim por diante. (Lembre-se de que, em programação, a contagem geralmente começa em zero.)

    Ao buscar um arquivo carregado, você o coloca em uma variável (aqui, `uploadedFile`) para que possa manipulá-lo. Para determinar o nome do arquivo carregado, basta obter sua propriedade `FileName`. No entanto, quando o usuário carrega um arquivo, `FileName` contém o nome original do usuário, que inclui o caminho inteiro. Pode ser assim:

    *C:\Users\Public\Sample.txt*

    No entanto, você não quer todas essas informações de caminho porque esse é o caminho no computador do usuário, não para o servidor. Você só quer o nome de arquivo real (*Sample. txt*). Você pode remover apenas o arquivo de um caminho usando o método `Path.GetFileName`, da seguinte maneira:

    [!code-csharp[Main](working-with-files/samples/sample8.cs)]

    O objeto `Path` é um utilitário que tem vários métodos como esse, que você pode usar para remover caminhos, combinar caminhos e assim por diante.

    Depois de obter o nome do arquivo carregado, você pode criar um novo caminho para o local em que deseja armazenar o arquivo carregado no site. Nesse caso, você combina `Server.MapPath`, os nomes de pasta (*aplicativo\_data/uploadedFiles*) e o nome de arquivo recentemente removido para criar um novo caminho. Em seguida, você pode chamar o método `SaveAs` do arquivo carregado para realmente salvar o arquivo.
5. Execute a página em um navegador. 

    ![[imagem]](working-with-files/_static/image7.jpg)
6. Clique em **procurar** e selecione um arquivo para carregar. 

    ![[imagem]](working-with-files/_static/image8.jpg)

    A caixa de texto ao lado do botão **procurar** conterá o caminho e o local do arquivo.

    ![[imagem]](working-with-files/_static/image9.jpg)
7. Clique em **Carregar**.
8. No site, clique com o botão direito do mouse na pasta do projeto e clique em **Atualizar**.
9. Abra a pasta *uploadedFiles* . O arquivo que você carregou está na pasta. 

    ![[imagem]](working-with-files/_static/image10.jpg)

<a id="Letting_Users_Upload_Multiple_Files"></a>
## <a name="letting-users-upload-multiple-files"></a>Permitindo que os usuários carreguem vários arquivos

No exemplo anterior, você permite que os usuários carreguem um arquivo. Mas você pode usar o auxiliar de `FileUpload` para carregar mais de um arquivo por vez. Isso é útil para cenários como carregar fotos, onde o carregamento de um arquivo por vez é entediante. (Você pode ler sobre como carregar fotos em [trabalhando com imagens em um Site páginas da Web do ASP.net](https://go.microsoft.com/fwlink/?LinkId=202897).) Este exemplo mostra como permitir que os usuários carreguem dois de cada vez, embora você possa usar a mesma técnica para carregar mais do que isso.

1. Adicione a biblioteca de auxiliares Web do ASP.NET ao seu site, conforme descrito em [instalando auxiliares em um páginas da Web do ASP.net site](https://go.microsoft.com/fwlink/?LinkId=252372), se ainda não tiver feito isso.
2. Crie uma nova página chamada *FileUploadMultiple. cshtml*.
3. Substitua o conteúdo existente na página pelo seguinte:  

    [!code-cshtml[Main](working-with-files/samples/sample9.cshtml)]

    Neste exemplo, o auxiliar `FileUpload` no corpo da página está configurado para permitir que os usuários carreguem dois arquivos por padrão. Como `allowMoreFilesToBeAdded` é definido como `true`, o auxiliar renderiza um link que permite ao usuário adicionar mais caixas de carregamento:

    ![[imagem]](working-with-files/_static/image11.jpg)

    Para processar os arquivos que o usuário carrega, o código usa a mesma técnica básica que você usou no exemplo &#8212; anterior obter um arquivo de `Request.Files` e, em seguida, salvá-lo. (Incluindo as várias coisas que você precisa fazer para obter o nome e o caminho corretos do arquivo.) A inovação desta vez é que o usuário pode estar carregando vários arquivos e você não conhece muitos. Para descobrir, você pode obter `Request.Files.Count`.

    Com esse número em mãos, você pode executar um loop por meio de `Request.Files`, buscar cada arquivo por vez e salvá-lo. Quando desejar executar um loop em um número conhecido de vezes por meio de uma coleção, você poderá usar um loop de `for`, desta forma:

    [!code-csharp[Main](working-with-files/samples/sample10.cs)]

    A variável `i` é apenas um contador temporário que vai de zero a qualquer limite superior definido. Nesse caso, o limite superior é o número de arquivos. Mas como o contador começa em zero, como é típico para cenários de contagem em ASP.NET, o limite superior é, na verdade, um valor menor que a contagem de arquivos. (Se três arquivos forem carregados, a contagem será de zero a 2.)

    A variável `uploadedCount` totaliza todos os arquivos que foram carregados e salvos com êxito. Esse código conta com a possibilidade de que um arquivo esperado não possa ser carregado.
4. Execute a página em um navegador. O navegador exibe a página e suas duas caixas de carregamento.
5. Selecione dois arquivos para carregar.
6. Clique em **Adicionar outro arquivo**. A página exibe uma nova caixa de carregamento. 

    ![[imagem]](working-with-files/_static/image12.jpg)
7. Clique em **Carregar**.
8. No site, clique com o botão direito do mouse na pasta do projeto e clique em **Atualizar**.
9. Abra a pasta *uploadedFiles* para ver os arquivos carregados com êxito.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais

[Trabalhando com imagens em um site Páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202897)

[Exportar para um arquivo CSV](https://msdn.microsoft.com/library/ms155919.aspx)
