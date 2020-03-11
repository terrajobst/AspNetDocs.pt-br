---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
title: Criando um extensor de controle do kit de ferramentas de controle AJAX personalizado (VB) | Microsoft Docs
author: microsoft
description: Os extensores personalizados permitem que você personalize e estenda os recursos dos controles ASP.NET sem precisar criar novas classes.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 18b29834-c991-4e0c-b533-44d358fbfc9c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: 0849fa6c13679e0cd01bb20a4067a097acbce298
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578158"
---
# <a name="creating-a-custom-ajax-control-toolkit-control-extender-vb"></a>Criação de um extensor personalizado do AJAX Control Toolkit (VB)

pela [Microsoft](https://github.com/microsoft)

> Os extensores personalizados permitem que você personalize e estenda os recursos dos controles ASP.NET sem precisar criar novas classes.

Neste tutorial, você aprenderá a criar um extensor de controle personalizado do kit de ferramentas de controle AJAX. Criamos um extensor simples, mas útil, novo que altera o estado de um botão de desabilitado para habilitado quando você digita texto em uma caixa de texto. Depois de ler este tutorial, você poderá estender o kit de ferramentas do ASP.NET AJAX com seus próprios extensores de controle.

Você pode criar extensores de controle personalizados usando o Visual Studio ou o Visual Web Developer (verifique se você tem a versão mais recente do Visual Web Developer).

## <a name="overview-of-the-disabledbutton-extender"></a>Visão geral do extensor DisabledButton

Nosso novo extensor de controle é chamado de extensor DisabledButton. Esse extensor terá três propriedades:

- TargetControlID-a caixa de texto que o controle estende.
- TargetButtonIID-o botão que está desabilitado ou habilitado.
- DisabledText-o texto que é exibido inicialmente no botão. Quando você começa a digitar, o botão exibe o valor da propriedade texto do botão.

Você vincula o extensor DisabledButton a uma caixa de texto e controle de botão. Antes de digitar qualquer texto, o botão é desabilitado e a caixa de texto e o botão têm a seguinte aparência:

[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image1.png)

([Clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image3.png))

Depois de começar a digitar o texto, o botão é habilitado e a caixa de texto e o botão têm a seguinte aparência:

[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image4.png)

([Clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image6.png))

Para criar nosso extensor de controle, precisamos criar os três arquivos a seguir:

- DisabledButtonExtender. vb-esse arquivo é a classe de controle do lado do servidor que gerenciará a criação do seu extensor e permitirá que você defina as propriedades em tempo de design. Ele também define as propriedades que podem ser definidas em seu extensor. Essas propriedades são acessíveis por meio de código e em tempo de design e correspondem às propriedades definidas no arquivo DisableButtonBehavior. js.
- DisabledButtonBehavior. js--esse arquivo é onde você adicionará toda a lógica do script do cliente.
- DisabledButtonDesigner. vb – essa classe habilita a funcionalidade de tempo de design. Você precisará dessa classe se quiser que o extensor de controle funcione corretamente com o Visual Studio/Visual Web Developer designer.

Portanto, um extensor de controle consiste em um controle do lado do servidor, um comportamento do lado do cliente e uma classe de designer do lado do servidor. Você aprende a criar todos esses três arquivos nas seções a seguir.

## <a name="creating-the-custom-extender-website-and-project"></a>Criando o site e o projeto do extensor personalizado

A primeira etapa é criar um projeto e um site da biblioteca de classes no Visual Studio/Visual Web Developer. Criamos o extensor personalizado no projeto de biblioteca de classes e testamos o extensor personalizado no site.

Vamos começar com o site. Siga estas etapas para criar o site:

1. Selecione o arquivo de opção de menu **, novo site**.
2. Selecione o modelo de **site da Web ASP.net** .
3. Nomeie o novo site *Website1*.
4. Clique no botão **OK**.

Em seguida, precisamos criar o projeto de biblioteca de classes que conterá o código para o extensor de controle:

1. Selecione o arquivo de opção de menu **, adicionar, novo projeto**.
2. Selecione o modelo de **biblioteca de classes** .
3. Nomeie a nova biblioteca de classes com o nome **CustomExtenders**.
4. Clique no botão **OK**.

Depois de concluir essas etapas, sua janela de Gerenciador de Soluções deve ser parecida com a Figura 1.

[![solução com o site e o projeto de biblioteca de classes](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image7.png)

**Figura 01**: solução com o site e o projeto de biblioteca de classes ([clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image9.png))

Em seguida, você precisa adicionar todas as referências de assembly necessárias ao projeto de biblioteca de classes:

1. Clique com o botão direito do mouse no projeto CustomExtenders e selecione a opção de menu **Adicionar referência**.
2. Selecione a guia .NET.
3. Adicione referências aos assemblies a seguir:

    1. System.Web.dll
    2. System.Web.Extensions.dll
    3. System.Design.dll
    4. System.Web.Extensions.Design.dll
4. Selecione a guia procurar.
5. Adicione uma referência ao assembly AjaxControlToolkit. dll. Esse assembly está localizado na pasta em que você baixou o AJAX Control Toolkit.

Você pode verificar se adicionou todas as referências corretas clicando com o botão direito do mouse em seu projeto, selecionando Propriedades e clicando na guia Referências (consulte a Figura 2).

[![a pasta de referências com as referências necessárias](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image10.png)

**Figura 02**: pasta References com as referências necessárias ([clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image12.png))

## <a name="creating-the-custom-control-extender"></a>Criando o extensor de controle personalizado

Agora que temos nossa biblioteca de classes, podemos começar a criar nosso controle de extensor. Vamos começar com os Bones simples de uma classe de controle de extensor personalizada (consulte a listagem 1).

**Listagem 1-MyCustomExtender. vb**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample1.vb)]

Há várias coisas que você observa sobre a classe do extensor de controle na Listagem 1. Primeiro, observe que a classe herda da classe base ExtenderControlBase. Todos os controles do extensor do AJAX Control Toolkit derivam dessa classe base. Por exemplo, a classe base inclui a propriedade TargetId que é uma propriedade necessária de cada extensor de controle.

Em seguida, observe que a classe inclui os dois atributos a seguir relacionados ao script de cliente:

- WebResource-faz com que um arquivo seja incluído como um recurso incorporado em um assembly.
- ClientScriptResource-faz com que um recurso de script seja recuperado de um assembly.

O atributo WebResource é usado para inserir o arquivo JavaScript MyControlBehavior. js no assembly quando o extensor personalizado é compilado. O atributo ClientScriptResource é usado para recuperar o script MyControlBehavior. js do assembly quando o extensor personalizado é usado em uma página da Web.

Para que os atributos WebResource e ClientScriptResource funcionem, você deve compilar o arquivo JavaScript como um recurso incorporado. Selecione o arquivo na janela de Gerenciador de Soluções, abra a folha de propriedades e atribua o recurso de valor *inserido* à propriedade de **ação de compilação** .

Observe que o extensor de controle também inclui um atributo TargetControlType. Esse atributo é usado para especificar o tipo de controle que é estendido pelo extensor de controle. No caso da listagem 1, o extensor de controle é usado para estender uma caixa de texto.

Por fim, observe que o extensor personalizado inclui uma propriedade chamada MyProperty. A propriedade é marcada com o atributo ExtenderControlProperty. Os métodos GetPropertyValue () e SetPropertyValue () são usados para passar o valor da Propriedade do extensor de controle do lado do servidor para o comportamento do lado do cliente.

Vamos continuar e implementar o código para nosso extensor DisabledButton. O código para esse extensor pode ser encontrado na Listagem 2.

**Listagem 2-DisabledButtonExtender. vb**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample2.vb)]

O extensor DisabledButton na Listagem 2 tem duas propriedades chamadas TargetButtonID e DisabledText. O IDReferenceProperty aplicado à propriedade TargetButtonID impede que você atribua algo diferente da ID de um controle de botão a essa propriedade.

Os atributos WebResource e ClientScriptResource associam um comportamento do lado do cliente localizado em um arquivo chamado DisabledButtonBehavior. js a esse extensor. Discutiremos esse arquivo JavaScript na próxima seção.

## <a name="creating-the-custom-extender-behavior"></a>Criando o comportamento do extensor personalizado

O componente do lado do cliente de um extensor de controle é chamado de comportamento. A lógica real para desabilitar e habilitar o botão está contida no comportamento de DisabledButton. O código JavaScript para o comportamento está incluído na Listagem 3.

**Listagem 3-DisabledButton. js**

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample3.js)]

O arquivo JavaScript na Listagem 3 contém uma classe do lado do cliente chamada DisabledButtonBehavior. Essa classe, como a de seu lado do servidor, inclui duas propriedades chamadas TargetButtonID e DisabledText, que você pode acessar usando Get\_TargetButtonID/Set\_TargetButtonID e Get\_DisabledText/Set\_DisabledText.

O método Initialize () associa um manipulador de eventos KeyUp ao elemento de destino para o comportamento. Cada vez que você digita uma letra na caixa de texto associada a esse comportamento, o manipulador KeyUp é executado. O manipulador KeyUp habilita ou desabilita o botão dependendo se a caixa de texto associada ao comportamento contém qualquer texto.

Lembre-se de que você deve compilar o arquivo JavaScript na Listagem 3 como um recurso incorporado. Selecione o arquivo na janela de Gerenciador de Soluções, abra a folha de propriedades e atribua o recurso de valor *inserido* à propriedade de **ação de compilação** (consulte a Figura 3). Essa opção está disponível no Visual Studio e no Visual Web Developer.

[![adicionar um arquivo JavaScript como um recurso incorporado](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image13.png)

**Figura 03**: adicionando um arquivo JavaScript como um recurso inserido ([clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image15.png))

## <a name="creating-the-custom-extender-designer"></a>Criando o designer de extensor personalizado

Há uma última classe que precisamos criar para concluir nosso extensor. Precisamos criar a classe de designer na Listagem 4. Essa classe é necessária para fazer com que o extensor se comporte corretamente com o Visual Studio/Visual Web Developer designer.

**Listagem 4-DisabledButtonDesigner. vb**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample4.vb)]

Você associa o designer na Listagem 4 com o extensor DisabledButton com o atributo designer. Você precisa aplicar o atributo designer à classe DisabledButtonExtender como esta:

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample5.vb)]

## <a name="using-the-custom-extender"></a>Usando o extensor personalizado

Agora que acabamos de criar o extensor de controle DisabledButton, é hora de usá-lo em nosso site do ASP.NET. Primeiro, precisamos adicionar o extensor personalizado à caixa de ferramentas. Siga estas etapas:

1. Abra uma página do ASP.NET clicando duas vezes na página na janela Gerenciador de Soluções.
2. Clique com o botão direito do mouse na caixa de ferramentas e selecione a opção de menu **escolher itens**.
3. Na caixa de diálogo escolher itens da caixa de ferramentas, navegue até o assembly CustomExtenders. dll.
4. Clique no botão **OK** para fechar a caixa de diálogo.

Depois de concluir essas etapas, o extensor de controle DisabledButton deve aparecer na caixa de ferramentas (consulte a Figura 4).

[![DisabledButton na caixa de ferramentas](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image16.png)

**Figura 04**: DisabledButton na caixa de ferramentas ([clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image18.png))

Em seguida, precisamos criar uma nova página ASP.NET. Siga estas etapas:

1. Crie uma nova página do ASP.NET chamada ShowDisabledButton. aspx.
2. Arraste um ScriptManager para a página.
3. Arraste um controle TextBox para a página.
4. Arraste um controle de botão para a página.
5. No janela Propriedades, altere a propriedade ID do botão para o valor <em>btnSave</em> e a propriedade Text para o valor *Save\** .

Criamos uma página com uma caixa de texto e um controle de botão padrão ASP.NET.

Em seguida, precisamos estender o controle TextBox com o extensor DisabledButton:

1. Selecione a opção Adicionar tarefa de **extensor** para abrir a caixa de diálogo Assistente de extensor (consulte a Figura 5). Observe que a caixa de diálogo inclui nosso extensor DisabledButton personalizado.
2. Selecione o extensor DisabledButton e clique no botão **OK** .

[![a caixa de diálogo Assistente de extensor](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image19.png)

**Figura 05**: a caixa de diálogo do assistente do extensor ([clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image21.png))

Por fim, podemos definir as propriedades do extensor DisabledButton. Você pode modificar as propriedades do extensor DisabledButton modificando as propriedades do controle TextBox:

1. Selecione a caixa de texto no designer.
2. Na janela Propriedades, expanda o nó extensores (veja a Figura 6).
3. Atribua o valor *Save* à propriedade DisabledText e o valor *BtnSave* à propriedade TargetButtonID.

[Propriedades do extensor de configuração de ![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image22.png)

**Figura 06**: definindo propriedades do extensor ([clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image24.png))

Quando você executa a página (pressionando F5), o controle de botão é inicialmente desabilitado. Assim que você começar a inserir texto na TextBox, o controle Button será habilitado (consulte a Figura 7).

[![o extensor DisabledButton em ação](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image25.png)

**Figura 07**: o extensor DisabledButton em ação ([clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image27.png))

## <a name="summary"></a>Resumo

O objetivo deste tutorial foi explicar como você pode estender o AJAX Control Toolkit com controles de extensor personalizados. Neste tutorial, criamos um extensor de controle DisabledButton simples. Implementamos esse extensor criando uma classe DisabledButtonExtender, um comportamento de JavaScript DisabledButtonBehavior e uma classe DisabledButtonDesigner. Você segue um conjunto de etapas semelhantes sempre que criar um extensor de controle personalizado.

> [!div class="step-by-step"]
> [Anterior](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)
