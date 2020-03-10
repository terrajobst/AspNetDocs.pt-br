---
uid: web-pages/overview/ui-layouts-and-themes/9-working-with-images
title: Trabalhando com imagens em um site Páginas da Web do ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Este capítulo mostra como adicionar, exibir e manipular imagens (redimensionar, inverter e adicionar marcas-d ' água) em seu site.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 778c4e58-4372-4d25-bab9-aec4a8d8e38d
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/9-working-with-images
msc.type: authoredcontent
ms.openlocfilehash: 53514b3c314fc182a43c82974ffcfa8158a636a1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78631855"
---
# <a name="working-with-images-in-an-aspnet-web-pages-razor-site"></a>Trabalhando com imagens em um site Páginas da Web do ASP.NET (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo mostra como adicionar, exibir e manipular imagens (redimensionar, virar e adicionar marcas-d ' água) em um site Páginas da Web do ASP.NET (Razor).
> 
> O que você aprenderá:
> 
> - Como adicionar uma imagem a uma página dinamicamente.
> - Como permitir que os usuários carreguem uma imagem.
> - Como redimensionar uma imagem.
> - Como inverter ou girar uma imagem.
> - Como adicionar uma marca d' água a uma imagem.
> - Como usar uma imagem como uma marca d' água.
> 
> Estes são os recursos de programação do ASP.NET apresentados no artigo:
> 
> - O auxiliar de `WebImage`.
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

<a id="Adding_an_Image"></a>
## <a name="adding-an-image-to-a-web-page-dynamically"></a>Adicionando uma imagem a uma página da Web dinamicamente

Você pode adicionar imagens ao seu site e a páginas individuais enquanto estiver desenvolvendo o site. Você também pode permitir que os usuários carreguem imagens, o que pode ser útil para tarefas como permitir que adicionem uma foto de perfil.

Se uma imagem já estiver disponível no seu site e você apenas quiser exibi-la em uma página, use um elemento HTML `<img>` como este:

[!code-html[Main](9-working-with-images/samples/sample1.html)]

No entanto, às vezes, você precisa ser capaz de exibir &#8212; imagens dinamicamente, ou seja, você não sabe qual imagem deve ser exibida até que a página esteja em execução.

O procedimento nesta seção mostra como exibir uma imagem imediatamente onde os usuários especificam o nome do arquivo de imagem de uma lista de nomes de imagem. Eles selecionam o nome da imagem em uma lista suspensa e, quando enviam a página, a imagem selecionada é exibida.

![imagem](9-working-with-images/_static/image1.jpg "ch9images-1. jpg")

1. No WebMatrix, crie um novo site.
2. Adicione uma nova página chamada *DynamicImage. cshtml*.
3. Na pasta raiz do site, adicione uma nova pasta e nomeie-a como *imagens*.
4. Adicione quatro imagens à pasta *imagens* que você acabou de criar. (Qualquer imagem que você tenha útil será, mas elas devem se ajustar a uma página.) Renomeie as imagens *Photo1. jpg*, *Photo2. jpg*, *Photo3. jpg*e *Photo4. jpg*. (Você não usará o *Photo4. jpg* neste procedimento, mas você o usará posteriormente neste artigo.)
5. Verifique se as quatro imagens não estão marcadas como somente leitura.
6. Substitua o conteúdo existente na página pelo seguinte:

    [!code-cshtml[Main](9-working-with-images/samples/sample2.cshtml)]

    O corpo da página tem uma lista suspensa (um elemento `<select>`) denominada `photoChoice`. A lista tem três opções, e o atributo `value` de cada opção de lista tem o nome de uma das imagens que você colocou na pasta *imagens* . Essencialmente, a lista permite que o usuário selecione um nome amigável como &quot;foto 1&quot;e, em seguida, passe o nome do arquivo *. jpg* quando a página é enviada.

    No código, você pode obter a seleção do usuário (em outras palavras, o nome do arquivo de imagem) da lista lendo `Request["photoChoice"]`. Primeiro, você verá se há uma seleção. Se houver, você construirá um caminho para a imagem que consiste no nome da pasta para as imagens e o nome do arquivo de imagem do usuário. (Se você tentou construir um caminho, mas não havia nada em `Request["photoChoice"]`, obteria um erro.) Isso resulta em um caminho relativo como este:

    *imagens/Photo1. jpg*

    O caminho é armazenado em uma variável chamada `imagePath` que você precisará mais tarde na página.

    No corpo, há também um elemento `<img>` que é usado para exibir a imagem que o usuário escolheu. O atributo `src` não está definido como um nome de arquivo ou URL, como você faria para exibir um elemento estático. Em vez disso, ele é definido como `@imagePath`, o que significa que ele obtém seu valor do caminho que você definiu no código.

    No entanto, na primeira vez que a página é executada, não há nenhuma imagem a ser exibida, pois o usuário não selecionou nada. Isso normalmente significa que o atributo `src` estaria vazio e a imagem apareceria como um&quot; vermelho &quot;x (ou qualquer que seja o navegador renderizado quando não conseguir encontrar uma imagem). Para evitar isso, coloque o elemento `<img>` em um bloco de `if` que teste para ver se a variável `imagePath` tem alguma coisa. Se o usuário tiver feito uma seleção, `imagePath` conterá o caminho. Se o usuário não tiver escolhido uma imagem ou se esta for a primeira vez que a página for exibida, o elemento `<img>` não será até mesmo renderizado.
7. Salve o arquivo e execute a página em um navegador. (Verifique se a página está selecionada no espaço de trabalho **arquivos** antes de executá-la.)
8. Selecione uma imagem na lista suspensa e clique em **imagem de exemplo**. Verifique se você vê imagens diferentes para diferentes opções.

<a id="Uploading_an_Image"></a>
## <a name="uploading-an-image"></a>Carregando uma imagem

O exemplo anterior mostrou como exibir uma imagem dinamicamente, mas funcionou apenas com imagens que já estavam em seu site. Este procedimento mostra como permitir que os usuários carreguem uma imagem, que é exibida na página. No ASP.NET, você pode manipular imagens rapidamente usando o auxiliar `WebImage`, que tem métodos que permitem criar, manipular e salvar imagens. O auxiliar de `WebImage` dá suporte a todos os tipos de arquivo de imagem da Web comuns, incluindo *. jpg*, *. png*e *. bmp*. Ao longo deste artigo, você usará imagens *. jpg* , mas poderá usar qualquer um dos tipos de imagem.

![imagem](9-working-with-images/_static/image2.jpg "ch9images-2. jpg")

1. Adicione uma nova página e nomeie-a *UploadImage. cshtml*.
2. Substitua o conteúdo existente na página pelo seguinte: 

    [!code-cshtml[Main](9-working-with-images/samples/sample3.cshtml)]

    O corpo do texto tem um elemento `<input type="file">`, que permite aos usuários selecionar um arquivo a ser carregado. Quando eles clicam em **Enviar**, o arquivo escolhido é enviado junto com o formulário.

    Para obter a imagem carregada, use o auxiliar de `WebImage`, que tem todos os tipos de métodos úteis para trabalhar com imagens. Especificamente, você usa `WebImage.GetImageFromRequest` para obter a imagem carregada (se houver) e armazená-la em uma variável chamada `photo`.

    Muitos dos trabalhos neste exemplo envolvem obter e definir nomes de arquivo e caminho. O problema é que você deseja obter o nome (e apenas o nome) da imagem que o usuário carregou e, em seguida, criar um novo caminho para onde você vai armazenar a imagem. Como os usuários podem carregar várias imagens que têm o mesmo nome, você usa um pouco de código extra para criar nomes exclusivos e garantir que os usuários não substituam as imagens existentes.

    Se uma imagem for realmente carregada (o `if (photo != null)`de teste), você obterá o nome da imagem da propriedade `FileName` da imagem. Quando o usuário carrega a imagem, `FileName` contém o nome original do usuário, que inclui o caminho do computador do usuário. Pode ser assim:

    *C:\Users\Joe\Pictures\SamplePhoto1.jpg*

    Você não quer todas essas informações de caminho, &#8212; embora queira apenas o nome real do arquivo (*SamplePhoto1. jpg*). Você pode remover apenas o arquivo de um caminho usando o método `Path.GetFileName`, da seguinte maneira:

    [!code-csharp[Main](9-working-with-images/samples/sample4.cs)]

    Em seguida, você cria um novo nome de arquivo exclusivo adicionando um GUID ao nome original. (Para obter mais informações sobre GUIDs, consulte [sobre GUIDs](#SB_AboutGUIDs) mais adiante neste artigo.) Em seguida, você cria um caminho completo que pode ser usado para salvar a imagem. O caminho de salvamento é composto pelo nome do novo arquivo, a pasta (imagens) e o local atual do site.

    > [!NOTE]
    > Para que seu código salve arquivos na pasta *imagens* , o aplicativo precisa de permissões de leitura/gravação para essa pasta. Em seu computador de desenvolvimento, isso normalmente não é um problema. No entanto, quando você publica seu site no servidor Web de um provedor de hospedagem, talvez seja necessário definir explicitamente essas permissões. Se você executar esse código em um servidor do provedor de hospedagem e receber erros, verifique com o provedor de hospedagem para descobrir como definir essas permissões.

    Por fim, você passa o caminho de salvamento para o método `Save` do auxiliar de `WebImage`. Isso armazena a imagem carregada com seu novo nome. O método Save é semelhante a este: `photo.Save(@"~\" + imagePath)`. O caminho completo é acrescentado a `@"~\"`, que é o local atual do site. (Para obter informações sobre o operador de `~`, consulte [introdução à programação da Web do ASP.NET usando a sintaxe do Razor](https://go.microsoft.com/fwlink/?LinkId=202890#ID_WorkingWithFileAndFolderPaths).)

    Como no exemplo anterior, o corpo da página contém um elemento `<img>` para exibir a imagem. Se `imagePath` tiver sido definido, o elemento `<img>` será renderizado e seu atributo `src` será definido como o valor `imagePath`.
3. Execute a página em um navegador.
4. Carregue uma imagem e verifique se ela é exibida na página.
5. No seu site, abra a pasta *imagens* . Você verá que um novo arquivo foi adicionado cujo nome de arquivo é semelhante a:: 

    *45ea4527-7ddd-4965-b9ca-c6444982b342\_myphoto. png*

    Essa é a imagem que você carregou com um GUID prefixado para o nome. (Seu próprio arquivo terá um GUID diferente e provavelmente será nomeado algo diferente de *myphoto. png*.)

> [!TIP] 
> 
> <a id="SB_AboutGUIDs"></a>
> ### <a name="about-guids"></a>Sobre GUIDs
> 
> Um GUID (globalmente-ID exclusivo) é um identificador que geralmente é processado em um formato como este: `936DA01F-9ABD-4d9d-80C7-02AF85C822A8`. Os números e as letras (de A-F) diferem para cada GUID, mas todos seguem o padrão de usar grupos de 8-4-4-4-12 caracteres. (Tecnicamente, um GUID é um número de 16 bytes/128 bits.) Quando você precisar de um GUID, poderá chamar um código especializado que gera um GUID para você. A ideia por trás dos GUIDs é que entre o enorme tamanho do número (3,4 x 10<sup>38</sup>) e o algoritmo para gerá-lo, o número resultante é praticamente garantido como sendo um de um tipo. Os GUIDs, portanto, são uma boa maneira de gerar nomes para coisas quando você deve garantir que não use o mesmo nome duas vezes. A desvantagem, é claro, é que os GUIDs não são particularmente amigáveis para o usuário, portanto, eles tendem a ser usados quando o nome é usado apenas no código.

<a id="Resizing_an_Image"></a>
## <a name="resizing-an-image"></a>Redimensionando uma imagem

Se o seu site aceitar imagens de um usuário, talvez você queira redimensionar as imagens antes de exibi-las ou salvá-las. Você pode usar novamente o auxiliar de `WebImage` para isso.

Este procedimento mostra como redimensionar uma imagem carregada para criar uma miniatura e, em seguida, salvar a miniatura e a imagem original no site. Você exibe a miniatura na página e usa um hiperlink para redirecionar os usuários para a imagem em tamanho normal.

![imagem](9-working-with-images/_static/image3.jpg "ch9images-3. jpg")

1. Adicione uma nova página chamada *thumbnail. cshtml*.
2. Na pasta *imagens* , crie uma subpasta chamada *thumbs*.
3. Substitua o conteúdo existente na página pelo seguinte: 

    [!code-cshtml[Main](9-working-with-images/samples/sample5.cshtml)]

    Esse código é semelhante ao código do exemplo anterior. A diferença é que esse código salva a imagem duas vezes, uma vez normalmente e uma vez após a criação de uma cópia em miniatura da imagem. Primeiro, você obtém a imagem carregada e a salva na pasta *imagens* . Em seguida, você cria um novo caminho para a imagem em miniatura. Para criar realmente a miniatura, você chama o método `Resize` do auxiliar de `WebImage` para criar uma imagem de 60 pixels por 60 pixels. O exemplo mostra como você preserva a taxa de proporção e como você pode impedir que a imagem seja ampliada (caso o novo tamanho realmente torne a imagem maior). A imagem redimensionada é salva na subpasta *thumbs* .

    No final da marcação, você usa o mesmo elemento `<img>` com o atributo Dynamic `src` que você viu nos exemplos anteriores para mostrar condicionalmente a imagem. Nesse caso, você exibe a miniatura. Você também usa um elemento `<a>` para criar um hiperlink para a versão grande da imagem. Assim como com o atributo `src` do elemento `<img>`, você define o atributo `href` do elemento `<a>` dinamicamente para o que estiver em `imagePath`. Para certificar-se de que o caminho possa funcionar como uma URL, você passa `imagePath` para o método `Html.AttributeEncode`, que converte os caracteres reservados no caminho para caracteres que estão ok em uma URL.
4. Execute a página em um navegador.
5. Carregue uma foto e verifique se a miniatura é mostrada.
6. Clique na miniatura para ver a imagem em tamanho normal.
7. Nas *imagens* e *imagens/polegares*, observe que novos arquivos foram adicionados.

<a id="Rotating_and_Flipping"></a>
## <a name="rotating-and-flipping-an-image"></a>Girando e invertendo uma imagem

O auxiliar de `WebImage` também permite que você inverta e gire imagens. Este procedimento mostra como obter uma imagem do servidor, virar a imagem de cabeça para baixo (verticalmente), salvá-la e exibir a imagem invertida na página. Neste exemplo, você está usando apenas um arquivo que você já tem no servidor (*Photo2. jpg*). Em um aplicativo real, você provavelmente inverteria uma imagem cujo nome você obteve dinamicamente, como fez nos exemplos anteriores.

![imagem](9-working-with-images/_static/image4.jpg "ch9images-4. jpg")

1. Adicione uma nova página chamada *FlipImage. cshtml*.
2. Substitua o conteúdo existente na página pelo seguinte: 

    [!code-cshtml[Main](9-working-with-images/samples/sample6.cshtml)]

    O código usa o auxiliar `WebImage` para obter uma imagem do servidor. Você cria o caminho para a imagem usando a mesma técnica usada em exemplos anteriores para salvar imagens e passa esse caminho ao criar uma imagem usando `WebImage`:

    [!code-javascript[Main](9-working-with-images/samples/sample7.js)]

    Se uma imagem for encontrada, você construirá um novo caminho e nome de arquivo, como você fez nos exemplos anteriores. Para inverter a imagem, você chama o método `FlipVertical` e, em seguida, salva a imagem novamente.

    A imagem é exibida novamente na página usando o elemento `<img>` com o atributo `src` definido como `imagePath`.
3. Execute a página em um navegador. A imagem de *Photo2. jpg* é mostrada de cabeça para baixo.
4. Atualize a página ou solicite a página novamente para ver se a imagem foi invertida novamente no lado direito.

Para girar uma imagem, use o mesmo código, exceto que, em vez de chamar o `FlipVertical` ou `FlipHorizontal`, você chama `RotateLeft` ou `RotateRight`.

<a id="Adding_a_Watermark"></a>
## <a name="adding-a-watermark-to-an-image"></a>Adicionando uma marca d' água a uma imagem

Ao adicionar imagens ao seu site, talvez você queira adicionar uma marca d' água à imagem antes de salvá-la ou exibi-la em uma página. As pessoas geralmente usam marcas d' água para adicionar informações de direitos autorais a uma imagem ou para anunciar seu nome comercial.

![imagem](9-working-with-images/_static/image5.jpg "ch9images-5. jpg")

1. Adicione uma nova página chamada *watermark. cshtml*.
2. Substitua o conteúdo existente na página pelo seguinte: 

    [!code-cshtml[Main](9-working-with-images/samples/sample8.cshtml)]

    Esse código é como o código na página *FlipImage. cshtml* de antes (embora desta vez use o arquivo *Photo3. jpg* ). Para adicionar a marca d' água, chame o método `AddTextWatermark` do auxiliar de `WebImage` antes de salvar a imagem. Na chamada para `AddTextWatermark`, você passa o texto &quot;minha marca d' água&quot;, define a cor da fonte como amarelo e define a família de fontes como Arial. (Embora não seja mostrado aqui, o auxiliar de `WebImage` também permite que você especifique a opacidade, a família de fontes e o tamanho da fonte e a posição do texto da marca-d ' água.) Quando você salva a imagem, ela não deve ser somente leitura.

    Como você viu antes, a imagem é exibida na página usando o elemento `<img>` com o atributo src definido como `@imagePath`.
3. Execute a página em um navegador. Observe o texto "minha marca d' água" no canto inferior direito da imagem.

<a id="Using_an_Image_as_a_Watermark"></a>
## <a name="using-an-image-as-a-watermark"></a>Usando uma imagem como uma marca d' água

Em vez de usar texto para uma marca d' água, você pode usar outra imagem. Às vezes, as pessoas usam imagens como um logotipo da empresa como marca-d ' água ou usam uma imagem de marca d' água em vez de texto para informações de direitos autorais.

![imagem](9-working-with-images/_static/image6.jpg "ch9images-6. jpg")

1. Adicione uma nova página chamada *ImageWatermark. cshtml*.
2. Adicione uma imagem à pasta *imagens* que você pode usar como um logotipo e renomeie a imagem *MyCompanyLogo. jpg*. Essa imagem deve ser uma imagem que pode ser exibida claramente quando definida como 80 pixels de largura e 20 pixels de altura.
3. Substitua o conteúdo existente na página pelo seguinte: 

    [!code-cshtml[Main](9-working-with-images/samples/sample9.cshtml)]

    Essa é outra variação no código de exemplos anteriores. Nesse caso, você chama `AddImageWatermark` para adicionar a imagem de marca d' água à imagem de destino (*Photo3. jpg*) antes de salvar a imagem. Ao chamar `AddImageWatermark`, você define sua largura como 80 pixels e a altura como 20 pixels. A imagem *MyCompanyLogo. jpg* é alinhada horizontalmente no centro e alinhado verticalmente na parte inferior da imagem de destino. A opacidade é definida como 100% e o preenchimento é definido como 10 pixels. Se a imagem de marca d' água for maior que a imagem de destino, nada acontecerá. Se a imagem de marca d' água for maior que a imagem de destino e você definir o preenchimento da marca-d ' água da imagem como zero, a marca d' água será ignorada.

    Como antes, você exibe a imagem usando o elemento `<img>` e um atributo de `src` dinâmico.
4. Execute a página em um navegador. Observe que a imagem de marca d' água aparece na parte inferior da imagem principal.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais

[Trabalhando com arquivos em um site Páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202896)

[Introdução à programação de Páginas da Web do ASP.NET usando a sintaxe do Razor](https://go.microsoft.com/fwlink/?LinkID=251587)
