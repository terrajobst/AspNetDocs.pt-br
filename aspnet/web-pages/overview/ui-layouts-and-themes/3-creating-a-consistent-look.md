---
uid: web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
title: Criando um layout consistente em sites Páginas da Web do ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Para tornar mais eficiente a criação de páginas da Web para seu site, você pode criar blocos de conteúdo reutilizáveis (como cabeçalhos e rodapés) para seu site e c...
ms.author: riande
ms.date: 03/10/2014
ms.assetid: d7bd001b-6db2-4422-9b78-f3d08b743b00
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
msc.type: authoredcontent
ms.openlocfilehash: 3f63ce68ae4c13970ac0df196167ace0b22b592c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78627445"
---
# <a name="creating-a-consistent-layout-in-aspnet-web-pages-razor-sites"></a>Criando um layout consistente em sites Páginas da Web do ASP.NET (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo explica como você pode usar páginas de layout em um site Páginas da Web do ASP.NET (Razor) para criar blocos de conteúdo reutilizáveis (como cabeçalhos e rodapés) e para criar uma aparência consistente para todas as páginas no site.
> 
> **O que você aprenderá:** 
> 
> - Como criar blocos de conteúdo reutilizáveis como cabeçalhos e rodapés.
> - Como criar uma aparência consistente para todas as páginas em seu site usando um layout.
> - Como passar dados em tempo de execução para uma página de layout.
> 
> Estes são os recursos do ASP.NET apresentados no artigo:
> 
> - Blocos de conteúdo, que são arquivos que contêm conteúdo formatado em HTML para serem inseridos em várias páginas.
> - Páginas de layout, que são páginas que contêm conteúdo formatado em HTML que podem ser compartilhadas por páginas no site.
> - Os métodos `RenderPage`, `RenderBody`e `RenderSection`, que dizem ao ASP.NET onde inserir elementos de página.
> - O dicionário de `PageData` que permite que você compartilhe dados entre blocos de conteúdo e páginas de layout.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 3
>   
> 
> Este tutorial também funciona com o Páginas da Web do ASP.NET 2.

## <a name="about-layout-pages"></a>Sobre páginas de layout

Muitos sites têm conteúdo que é exibido em todas as páginas, como um cabeçalho e rodapé, ou uma caixa que informa aos usuários que eles estão conectados. O ASP.NET permite que você crie um arquivo separado com um bloco de conteúdo que pode conter texto, marcação e código, assim como uma página da Web normal. Em seguida, você pode inserir o bloco de conteúdo em outras páginas no site em que você deseja que as informações sejam exibidas. Dessa forma, você não precisa copiar e colar o mesmo conteúdo em cada página. Criar conteúdo comum como esse também torna mais fácil atualizar seu site. Se você precisar alterar o conteúdo, basta atualizar um único arquivo e as alterações serão refletidas em todos os lugares em que o conteúdo foi inserido.

O diagrama a seguir mostra como os blocos de conteúdo funcionam. Quando um navegador solicita uma página do servidor Web, o ASP.NET insere os blocos de conteúdo no ponto em que o método de `RenderPage` é chamado na página principal. A página concluída (mesclada) é enviada para o navegador.

![Diagrama conceitual que mostra como o método RenderPage insere uma página referenciada na página atual.](3-creating-a-consistent-look/_static/image1.jpg)

Neste procedimento, você criará uma página que faz referência a dois blocos de conteúdo (um cabeçalho e um rodapé) que estão localizados em arquivos separados. Você pode usar esses mesmos blocos de conteúdo em qualquer página do seu site. Quando terminar, você obterá uma página como esta:

![Captura de tela mostrando uma página no navegador que resulta da execução de uma página que inclui chamadas para o método RenderPage.](3-creating-a-consistent-look/_static/image2.png)

1. Na pasta raiz do seu site, crie um arquivo chamado *index. cshtml*.
2. Substitua a marcação existente pelo seguinte:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample1.html)]
3. Na pasta raiz, crie uma pasta chamada *Shared*.

    > [!NOTE]
    > É uma prática comum armazenar arquivos que são compartilhados entre páginas da Web em uma pasta chamada *Shared*.
4. Na pasta *compartilhada* , crie um arquivo chamado *\_header. cshtml*.
5. Substitua qualquer conteúdo existente pelo seguinte:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample2.html)]

    Observe que o nome do arquivo é *\_header. cshtml*, com um sublinhado (\_) como um prefixo. ASP.NET não enviará uma página para o navegador se seu nome começar com um sublinhado. Isso impede que as pessoas solicitem (inadvertidamente ou não) essas páginas diretamente. É uma boa ideia usar um sublinhado para nomear páginas que têm blocos de conteúdo neles, porque você não quer realmente que os usuários possam solicitar essas páginas &#8212; que existem estritamente para serem inseridas em outras páginas.
6. Na pasta *compartilhada* , crie um arquivo chamado *\_footer. cshtml* e substitua o conteúdo pelo seguinte:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample3.html)]
7. Na página *index. cshtml* , adicione duas chamadas ao método `RenderPage`, conforme mostrado aqui:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample4.html)]

    Isso mostra como inserir um bloco de conteúdo em uma página da Web. Você chama o método `RenderPage` e passa o nome do arquivo cujo conteúdo você deseja inserir nesse ponto. Aqui, você está inserindo o conteúdo do *\_header. cshtml* e *\_arquivos footer. cshtml* no arquivo *index. cshtml* .
8. Execute a página *index. cshtml* em um navegador. (No WebMatrix, no espaço de trabalho **arquivos** , clique com o botão direito do mouse no arquivo e selecione **Iniciar no navegador**.)
9. No navegador, exiba a origem da página. (Por exemplo, no Internet Explorer, clique com o botão direito do mouse na página e clique em **Exibir origem**.)

    Isso permite que você veja a marcação de página da Web que é enviada para o navegador, que combina a marcação de página de índice com os blocos de conteúdo. O exemplo a seguir mostra a origem da página que é renderizada para *index. cshtml*. As chamadas para `RenderPage` que você inseriu em *index. cshtml* foram substituídas pelo conteúdo real dos arquivos de cabeçalho e rodapé.

    [!code-html[Main](3-creating-a-consistent-look/samples/sample5.html)]

## <a name="creating-a-consistent-look-using-layout-pages"></a>Criando uma aparência consistente usando páginas de layout

Até agora, você viu que é fácil incluir o mesmo conteúdo em várias páginas. Uma abordagem mais estruturada para criar uma aparência consistente para um site é usar páginas de layout. Uma página de layout define a estrutura de uma página da Web, mas não contém nenhum conteúdo real. Depois de criar uma página de layout, você pode criar páginas da Web que contêm o conteúdo e vinculá-las à página de layout. Quando essas páginas forem exibidas, elas serão formatadas de acordo com a página de layout. (Nesse sentido, uma página de layout atua como um tipo de modelo para o conteúdo que é definido em outras páginas.)

A página de layout é assim como qualquer página HTML, exceto que ela contém uma chamada para o método `RenderBody`. A posição do método `RenderBody` na página de layout determina onde as informações da página de conteúdo serão incluídas.

O diagrama a seguir mostra como as páginas de conteúdo e as páginas de layout são combinadas em tempo de execução para produzir a página da Web concluída. O navegador solicita uma página de conteúdo. A página conteúdo tem um código que especifica a página de layout a ser usada para a estrutura da página. Na página layout, o conteúdo é inserido no ponto em que o método `RenderBody` é chamado. Os blocos de conteúdo também podem ser inseridos na página de layout chamando o método `RenderPage`, como você fez na seção anterior. Quando a página da Web for concluída, ela será enviada para o navegador.

![Captura de tela mostrando uma página no navegador que resulta da execução de uma página que inclui chamadas para o método RenderBody.](3-creating-a-consistent-look/_static/image3.jpg)

O procedimento a seguir mostra como criar uma página de layout e vincular páginas de conteúdo a ela.

1. Na pasta *compartilhada* do seu site, crie um arquivo chamado *\_Layout1. cshtml*.
2. Substitua qualquer conteúdo existente pelo seguinte:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample6.html)]

    Use o método `RenderPage` em uma página de layout para inserir blocos de conteúdo. Uma página de layout só pode conter uma chamada para o método `RenderBody`.
3. Na pasta *compartilhada* , crie um arquivo chamado *\_Header2. cshtml* e substitua qualquer conteúdo existente pelo seguinte:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample7.html)]
4. Na pasta raiz, crie uma nova pasta e nomeie-a como *estilos*.
5. Na pasta *estilos* , crie um arquivo chamado *site. css* e adicione as seguintes definições de estilo:

    [!code-css[Main](3-creating-a-consistent-look/samples/sample8.css)]

    Essas definições de estilo estão aqui apenas para mostrar como as folhas de estilo podem ser usadas com páginas de layout. Se desejar, você pode definir seus próprios estilos para esses elementos.
6. Na pasta raiz, crie um arquivo chamado *Content1. cshtml* e substitua qualquer conteúdo existente pelo seguinte:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample9.cshtml)]

    Esta é uma página que usará uma página de layout. O bloco de código na parte superior da página indica qual página de layout usar para formatar esse conteúdo.
7. Execute *Content1. cshtml* em um navegador. A página renderizada usa o formato e a folha de estilo definidos em *\_Layout1. cshtml* e o texto (conteúdo) definido em *Content1. cshtml*.

    ![[imagem]](3-creating-a-consistent-look/_static/image4.png)

    Você pode repetir a etapa 6 para criar páginas de conteúdo adicionais que podem compartilhar a mesma página de layout.

    > [!NOTE]
    > Você pode configurar seu site para que você possa usar automaticamente a mesma página de layout para todas as páginas de conteúdo em uma pasta. Para obter detalhes, consulte [Personalizando o comportamento de todo o site para páginas da Web do ASP.net](https://go.microsoft.com/fwlink/?LinkId=202906).

## <a name="designing-layout-pages-that-have-multiple-content-sections"></a>Criando páginas de layout que têm várias seções de conteúdo

Uma página de conteúdo pode ter várias seções, o que é útil se você quiser usar layouts que tenham várias áreas com conteúdo substituível. Na página conteúdo, você dá a cada seção um nome exclusivo. (A seção padrão é Left sem nome.) Na página layout, você adiciona um método `RenderBody` para especificar onde a seção sem nome (padrão) deve aparecer. Em seguida, adicione métodos `RenderSection` separados para renderizar seções nomeadas individualmente.

O diagrama a seguir mostra como o ASP.NET lida com o conteúdo dividido em várias seções. Cada seção nomeada está contida em um bloco de seção na página conteúdo. (Eles são nomeados `Header` e `List` no exemplo.) O Framework insere a seção conteúdo na página de layout no ponto em que o método `RenderSection` é chamado. A seção sem nome (padrão) é inserida no ponto em que o método `RenderBody` é chamado, como vimos anteriormente.

![Diagrama conceitual que mostra como o método RenderSection insere seções de referências na página atual.](3-creating-a-consistent-look/_static/image5.jpg)

Este procedimento mostra como criar uma página de conteúdo que tem várias seções de conteúdo e como renderizá-la usando uma página de layout que dá suporte a várias seções de conteúdo.

1. Na pasta *compartilhada* , crie um arquivo chamado *\_Layout2. cshtml*.
2. Substitua qualquer conteúdo existente pelo seguinte:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample10.html)]

    Você usa o método `RenderSection` para renderizar as seções cabeçalho e lista.
3. Na pasta raiz, crie um arquivo chamado *Content2. cshtml* e substitua qualquer conteúdo existente pelo seguinte:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample11.cshtml)]

    Esta página de conteúdo contém um bloco de código na parte superior da página. Cada seção nomeada está contida em um bloco de seção. O restante da página contém a seção de conteúdo padrão (sem nome).
4. Execute *Content2. cshtml* em um navegador.

    ![Captura de tela mostrando uma página no navegador que resulta da execução de uma página que inclui chamadas para o método RenderSection.](3-creating-a-consistent-look/_static/image6.png)

## <a name="making-content-sections-optional"></a>Tornando as seções de conteúdo opcionais

Normalmente, as seções que você cria em uma página de conteúdo precisam corresponder às seções definidas na página layout. Você poderá obter erros se qualquer uma das seguintes ações ocorrer:

- A página conteúdo contém uma seção que não tem nenhuma seção correspondente na página de layout.
- A página de layout contém uma seção para a qual não há conteúdo.
- A página de layout inclui chamadas de método que tentam renderizar a mesma seção mais de uma vez.

No entanto, você pode substituir esse comportamento para uma seção nomeada declarando a seção como opcional na página de layout. Isso permite que você defina várias páginas de conteúdo que podem compartilhar uma página de layout, mas que podem ou não ter conteúdo para uma seção específica.

1. Abra *Content2. cshtml* e remova a seguinte seção:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample12.cshtml)]
2. Salve a página e execute-a em um navegador. Uma mensagem de erro é exibida, pois a página de conteúdo não fornece conteúdo para uma seção definida na página de layout, ou seja, a seção de cabeçalho.

    ![Captura de tela que mostra o erro que ocorre se você executar uma página que chama o método RenderSection, mas a seção correspondente não for fornecida.](3-creating-a-consistent-look/_static/image7.png)
3. Na pasta *compartilhada* , abra a página *\_Layout2. cshtml* e substitua esta linha:

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample13.js)]

    pelo código a seguir:

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample14.js)]

    Como alternativa, você pode substituir a linha de código anterior pelo seguinte bloco de código, que produz os mesmos resultados:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample15.cshtml)]
4. Execute a página *Content2. cshtml* em um navegador novamente. (Se você ainda tiver essa página aberta no navegador, poderá apenas atualizá-la.) Desta vez, a página é exibida sem erro, mesmo que não tenha nenhum cabeçalho.

## <a name="passing-data-to-layout-pages"></a>Passando dados para páginas de layout

Você pode ter dados definidos na página de conteúdo que você precisa fazer referência em uma página de layout. Nesse caso, você precisa passar os dados da página de conteúdo para a página de layout. Por exemplo, talvez você queira exibir o status de logon de um usuário ou pode mostrar ou ocultar áreas de conteúdo com base na entrada do usuário.

Para passar dados de uma página de conteúdo para uma página de layout, você pode colocar valores na propriedade `PageData` da página de conteúdo. A propriedade `PageData` é uma coleção de pares de nome/valor que armazenam os dados que você deseja passar entre as páginas. Na página layout, você pode ler os valores da propriedade `PageData`.

Aqui está outro diagrama. Este mostra como o ASP.NET pode usar a propriedade `PageData` para passar valores de uma página de conteúdo para a página de layout. Quando o ASP.NET começa a criar a página da Web, ele cria a coleção de `PageData`. Na página conteúdo, você escreve o código para colocar os dados na coleção de `PageData`. Os valores na coleção de `PageData` também podem ser acessados por outras seções na página de conteúdo ou por blocos de conteúdo adicionais.

![Diagrama conceitual que mostra como uma página de conteúdo pode popular um dicionário PageData e passar essas informações para a página de layout.](3-creating-a-consistent-look/_static/image8.jpg)

O procedimento a seguir mostra como passar dados de uma página de conteúdo para uma página de layout. Quando a página é executada, ela exibe um botão que permite ao usuário ocultar ou mostrar uma lista definida na página de layout. Quando os usuários clicam no botão, ele define um valor true/false (booliano) na propriedade `PageData`. A página layout lê esse valor e, se for false, oculta a lista. O valor também é usado na página conteúdo para determinar se o botão **ocultar lista** ou o botão **Mostrar lista** deve ser exibido.

![[imagem]](3-creating-a-consistent-look/_static/image9.jpg)

1. Na pasta raiz, crie um arquivo chamado *Content3. cshtml* e substitua qualquer conteúdo existente pelo seguinte:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample16.cshtml)]

    O código armazena duas partes de dados na propriedade &#8212; `PageData` o título da página da Web e true ou false para especificar se uma lista deve ser exibida.

    Observe que o ASP.NET permite colocar a marcação HTML na página condicionalmente usando um bloco de código. Por exemplo, o bloco de `if/else` no corpo da página determina qual formulário Exibir, dependendo se `PageData["ShowList"]` está definido como true.
2. Na pasta *compartilhada* , crie um arquivo chamado *\_Layout3. cshtml* e substitua qualquer conteúdo existente pelo seguinte:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample17.cshtml)]

    A página layout inclui uma expressão no elemento `<title>` que obtém o valor title da propriedade `PageData`. Ele também usa o valor `ShowList` da propriedade `PageData` para determinar se o bloco de conteúdo da lista deve ser exibido.
3. Na pasta *compartilhada* , crie um arquivo chamado *\_List. cshtml* e substitua qualquer conteúdo existente pelo seguinte:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample18.html)]
4. Execute a página *Content3. cshtml* em um navegador. A página é exibida com a lista visível no lado esquerdo da página e um botão **ocultar lista** na parte inferior.

    ![Captura de tela mostrando a página que inclui a lista e um botão que diz ' Ocultar lista '.](3-creating-a-consistent-look/_static/image10.png)
5. Clique em **ocultar lista**. A lista desaparece e o botão é alterado para **Mostrar lista**.

    ![Captura de tela mostrando a página que não inclui a lista e um botão que diz ' Mostrar lista '.](3-creating-a-consistent-look/_static/image11.png)
6. Clique no botão **Mostrar lista** e a lista será exibida novamente.

## <a name="additional-resources"></a>Recursos adicionais

[Personalizando o comportamento de todo o site para Páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202906)
