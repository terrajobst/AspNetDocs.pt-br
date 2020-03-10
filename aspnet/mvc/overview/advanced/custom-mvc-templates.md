---
uid: mvc/overview/advanced/custom-mvc-templates
title: Modelo MVC personalizado | Microsoft Docs
author: joeloff
description: Crie um modelo como uma extensão VSIX.
ms.author: riande
ms.date: 12/10/2012
ms.assetid: b0a214c7-2f38-4dbc-b47f-bd7bd9df97bd
msc.legacyurl: /mvc/overview/advanced/custom-mvc-templates
msc.type: authoredcontent
ms.openlocfilehash: 0603bc24e070e223551813f66a75889a2e46fd35
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616350"
---
# <a name="custom-mvc-template"></a>Modelo MVC personalizado

por [Jacques Eloff](https://github.com/joeloff)

O lançamento da atualização das ferramentas MVC 3 para Visual Studio 2010 introduziu um assistente de projeto separado para projetos MVC. A alteração foi orientada por dois fatores. Primeiro, a introdução dos novos modelos no MVC 3 e o suporte para mecanismos de exibição adicionais, como o Razor, levam à sobredimensionamento da caixa de diálogo novo projeto no Visual Studio. Em segundo lugar, os clientes tinham pedido de pontos de extensibilidade e o novo assistente de projeto MVC nos permitiria responder a essas solicitações.

A adição de modelos personalizados era um processo árduo que dependia do uso do registro para tornar novos modelos visíveis para o assistente de projeto do MVC. O autor de um novo modelo tinha que encapsule-lo dentro de um MSI para garantir que as entradas de Registro necessárias sejam criadas no momento da instalação. A alternativa era tornar um arquivo ZIP contendo o modelo disponível e fazer com que o usuário final crie as entradas de Registro necessárias manualmente.

Nenhuma das abordagens mencionadas acima é ideal, portanto, decidimos aproveitar algumas das infraestruturas existentes fornecidas pelas extensões [VSIX](https://msdn.microsoft.com/library/ff363239.aspx) para facilitar o autor, a distribuição e a instalação de modelos MVC personalizados a partir do MVC 4 para o Visual Studio 2012. Alguns dos benefícios fornecidos por essa abordagem são:

- Uma extensão VSIX pode conter vários modelos que dão suporte a diferentesC# idiomas (e Visual Basic) e vários mecanismos de exibição (aspx e Razor).
- Uma extensão do VSIX pode ter como alvo várias SKUs do Visual Studio, incluindo SKUs expressos.
- A [Galeria do Visual Studio](https://visualstudiogallery.msdn.microsoft.com/) facilita a distribuição da extensão para um público amplo.
- As extensões do VSIX podem ser atualizadas, facilitando a criação de correções e atualizações para seus modelos personalizados.

## <a name="prerequisites"></a>Prerequisites

- Os usuários precisam estar familiarizados com a criação de modelos de projeto, incluindo a marcação necessária para arquivos vstemplate, etc.
- Os usuários precisarão ter Visual Studio Professional e superior instalados. Os SKUs expressos não dão suporte à criação de projetos VSIX.
- [SDK do Visual Studio 2012](https://www.microsoft.com/download/details.aspx?id=30668) instalado.

## <a name="example"></a>Exemplo

A primeira etapa é criar um novo projeto VSIX usando o ou C# o Visual Basic. Selecione **arquivo > novo projeto**, clique em **extensibilidade** no painel esquerdo e selecione o **projeto VSIX**.

![Novo Projeto](custom-mvc-templates/_static/image1.jpg)

Depois que o projeto for criado, o designer do VSIX será aberto.

![Metadados do designer de projeto](custom-mvc-templates/_static/image2.jpg)

O designer pode ser usado para editar algumas das propriedades gerais da extensão que serão mostradas aos usuários quando eles instalarem a extensão ou procurarem as extensões instaladas no Visual Studio (**ferramentas > extensões e atualizações**). Depois de concluir as informações gerais, clique na **guia instalar destinos**.

![Destinos de instalação do designer de projeto](custom-mvc-templates/_static/image3.jpg)

Essa guia é usada para especificar os SKUs e as versões do Visual Studio com suporte na sua extensão. Marque a caixa de seleção para **esse VSIX é instalado para que todos os usuários** habilitem instalações por computador do VSIX. Clique no botão **novo** à direita para adicionar SKUs adicionais, como o Web Developer Express (VWD).

![Adicionar novo destino de instalação](custom-mvc-templates/_static/image4.jpg)

Se você pretende dar suporte a todos os SKUs Professional e superiores (Professional, Premium e Ultimate), precisa apenas selecionar o SKU mínimo na família, **Microsoft.VisualStudio.pro**. Lembre-se de salvar todas as alterações depois de concluir os destinos de instalação.

![Destinos de instalação do designer de projeto](custom-mvc-templates/_static/image5.jpg)

A guia **ativos** é usada para adicionar todos os arquivos de conteúdo ao VSIX. Como o MVC requer metadados personalizados, você editará o XML bruto do arquivo de manifesto do VSIX em vez de usar a guia **ativos** para adicionar conteúdo. Comece adicionando o conteúdo do modelo ao projeto VSIX. É importante que a estrutura da pasta e o conteúdo espelhem o layout do projeto. O exemplo a seguir contém quatro modelos de projeto que foram derivados do modelo de projeto do MVC básico. Certifique-se de que todos os arquivos que compõem o modelo de projeto (tudo abaixo da pasta ProjectTemplates) sejam adicionados ao grupo de itens de **conteúdo** no arquivo de projeto do VSIX e que cada item contenha os metadados **CopyToOutputDirectory** e **IncludeInVsix** definidos, conforme mostrado no exemplo a seguir.

&lt;conteúdo include =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicWeb.config&quot;&gt;

&lt;CopyToOutputDirectory&gt;sempre&lt;/CopyToOutputDirectory&gt;

&lt;IncludeInVSIX&gt;true&lt;/IncludeInVSIX&gt;

&lt;/Content&gt;

Caso contrário, o IDE tentará compilar o conteúdo do modelo quando você criar o VSIX e provavelmente verá um erro. Os arquivos de código em modelos geralmente contêm parâmetros especiais de [modelo](https://msdn.microsoft.com/library/eehb4faa(v=vs.110).aspx) usados pelo Visual Studio quando o modelo de projeto é instanciado e, portanto, não pode ser compilado no IDE.

![Gerenciador de Soluções](custom-mvc-templates/_static/image6.jpg)

Feche o designer do VSIX, clique com o botão direito do mouse no arquivo **Source. ramal. manifest** em **Gerenciador de soluções** e selecione **abrir com** e escolha a opção **Editor de XML (texto)** .

![Caixa de diálogo abrir com](custom-mvc-templates/_static/image7.jpg)

Crie um elemento de **&gt;de ativos de&lt;** e adicione um elemento de&gt;de ativo de **&lt;** para cada arquivo que deve ser incluído no VSIX. O atributo **Type** de cada elemento **&lt;Asset&gt;** deve ser definido como **Microsoft. VisualStudio. Mvc. Template**. Esse é um namespace personalizado que apenas o assistente de projeto do MVC compreende. Consulte a documentação do esquema do VSIX 2,0 para obter informações adicionais sobre a estrutura e o layout do arquivo de manifesto.

Apenas adicionar os arquivos ao VSIX não é suficiente para registrar os modelos com o assistente do MVC. Você precisa fornecer informações como o nome do modelo, descrição, mecanismos de exibição com suporte e linguagem de programação para o assistente do MVC. Essas informações são transportadas em atributos personalizados associados ao elemento **&lt;Asset&gt;** para cada arquivo **vstemplate** .

&lt;Asset d:VsixSubPath =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx&quot;

Tipo =&quot;Microsoft. VisualStudio. Mvc. Template&quot;

d:Source = arquivo de&quot;&quot;

Caminho =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicMvcWebApplicationProjectTemplate.11.csaspx.vstemplate&quot;

ProjectType =&quot;MVC&quot;

Language =&quot;C#&quot;

ViewEngine =&quot;&quot; aspx

TemplateID =&quot;MyMvcApplication&quot;

Título =&quot;aplicativo Web básico personalizado&quot;

Descrição =&quot;um modelo personalizado derivado de um aplicativo Web MVC (Razor) básico&quot;

Versão =&quot;4,0&quot;/&gt;

Veja abaixo uma explicação dos atributos personalizados que devem estar presentes:

- **ProjectType** deve ser definido como MVC.
- **Idioma** designa a linguagem de desenvolvimento com suporte do modelo. Os valores válidos são C# ou VB.
- **ViewEngine** designa o mecanismo de exibição suportado pelo modelo, como aspx ou Razor. Você pode especificar um valor personalizado para esse campo.
- **TemplateID** é usado para agrupar os modelos. Se o valor corresponder a uma ID de modelo existente, os modelos registrados anteriormente com o assistente do MVC serão substituídos.
- **Título** designa a breve descrição exibida no assistente MVC abaixo de cada modelo de projeto.
- **Descrição** designa uma descrição mais detalhada do modelo.

Depois de adicionar todos os arquivos ao manifesto e salvá-lo, você observará que a guia **ativos** no Designer exibirá todos os arquivos, mas não os atributos personalizados que você adicionou ao ativo de **&lt;&gt;** elementos para os arquivos **vstemplate** .

![Ativos do designer de projeto](custom-mvc-templates/_static/image8.jpg)

Tudo o que resta agora é compilar o projeto VSIX e instalá-lo.

Verifique se todas as instâncias do Visual Studio estão fechadas no computador em que você pretende testar a extensão do VSIX. O Visual Studio verifica se há novas extensões durante a inicialização, portanto, se o IDE estiver aberto durante a instalação de um VSIX, será necessário reiniciar o Visual Studio. No Explorer, clique duas vezes no arquivo VSIX para iniciar o **instalador do VSIX**, clique em **instalar** e inicie o Visual Studio.

![Instalador do VSIX](custom-mvc-templates/_static/image9.jpg)

No menu, selecione **ferramentas > extensões e atualizações** para confirmar que sua extensão foi instalada. Se o instalador do VSIX relatou erros durante a instalação da extensão, você pode exibir o log do instalador do VSIX para obter mais informações. Normalmente, o log é criado na pasta **% Temp%** do usuário que instalou a extensão, por exemplo, **C:\Users\Bob\AppData\Local\Temp**.

![Extensões e atualizações](custom-mvc-templates/_static/image10.jpg)

Depois de fechar a janela, você pode criar um projeto MVC 4 para ver se os novos modelos são mostrados no assistente do MVC.

![Novo projeto ASP.NET MVC 4](custom-mvc-templates/_static/image11.jpg)

## <a name="limitations"></a>Limitações

1. O assistente do MVC não oferece suporte a modelos personalizados localizados.
2. O assistente não relatará nenhum erro se falhar ao localizar modelos personalizados. Se qualquer um dos atributos personalizados necessários estiver ausente, o modelo simplesmente seria excluído do assistente.
