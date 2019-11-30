---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
title: Noções básicas sobre atualizações de página parcial com o ASP.NET AJAX | Microsoft Docs
author: scottcate
description: Talvez o recurso mais visível das extensões do AJAX ASP.NET seja a capacidade de fazer uma atualização parcial ou incremental de página sem fazer um postback completo para t...
ms.author: riande
ms.date: 03/28/2008
ms.assetid: 54d9df99-1161-4899-b4e8-2679c85915e7
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
msc.type: authoredcontent
ms.openlocfilehash: 4b87cb8f58dbd7f27b16bcb0d488ff361770d4fe
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74622935"
---
# <a name="understanding-partial-page-updates-with-aspnet-ajax"></a>Noções básicas sobre atualizações de página parcial com o AJAX ASP.NET

por [Scott Cate](https://github.com/scottcate)

[Baixar PDF](https://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial01_Partial_Page_Updates_cs.pdf)

> Talvez o recurso mais visível das extensões AJAX do ASP.NET seja a capacidade de fazer uma atualização parcial ou incremental de páginas sem fazer um postback completo para o servidor, sem alterações de código e alterações mínimas de marcação. As vantagens são extensivas – o estado de sua multimídia (como Adobe Flash ou Windows Media) é inalterado, os custos de largura de banda são reduzidos e o cliente não experimenta a cintilação geralmente associada a um postback.

## <a name="introduction"></a>Introdução

A tecnologia ASP.NET da Microsoft traz um modelo de programação orientado a objeto e orientado a eventos e o une com os benefícios do código compilado. No entanto, seu modelo de processamento no servidor tem várias desvantagens inerentes à tecnologia:

- As atualizações de página exigem uma viagem de ida e volta para o servidor, que requer uma atualização de página.
- As viagens de ida e volta não mantêm nenhum efeito gerado pelo JavaScript ou por outra tecnologia do lado do cliente (como o Adobe Flash)
- Durante o postback, navegadores diferentes do Microsoft Internet Explorer não dão suporte à restauração automática da posição de rolagem. E mesmo no Internet Explorer, ainda há uma cintilação à medida que a página é atualizada.
- Os postbacks podem envolver uma grande quantidade de largura de banda, pois o \_\_campo de formulário VIEWSTATE pode crescer, especialmente ao lidar com controles como o controle GridView ou os repetidores.
- Não há um modelo unificado para acessar serviços Web por meio de JavaScript ou de outra tecnologia do lado do cliente.

Insira as extensões do ASP.NET AJAX da Microsoft. O AJAX, que representa **uma** avaScript síncrona de **J** , **um** nd **X** ml, é uma estrutura integrada para fornecer atualizações de página incrementais por meio de JavaScript de plataforma cruzada, composta por código do lado do servidor que compreende o Microsoft Ajax Framework e um componente de script chamado Microsoft Ajax script library. As extensões do AJAX ASP.NET também fornecem suporte de plataforma cruzada para acessar os serviços Web do ASP.NET via JavaScript.

Este White Paper examina a funcionalidade de atualizações de página parcial das extensões do AJAX ASP.NET, que inclui o componente do ScriptManager, o controle UpdatePanel e o controle UpdateProgress e considera os cenários nos quais eles devem ou não ser utilizados.

Este White Paper se baseia na versão beta 2 do Visual Studio 2008 e no .NET Framework 3,5, que integra as extensões do AJAX ASP.NET à biblioteca de classes base (em que era anteriormente um componente complementar disponível para ASP.NET 2,0). Este White Paper também pressupõe que você esteja usando o Visual Studio 2008 e não o Visual Web Developer Express Edition; alguns modelos de projeto que são referenciados podem não estar disponíveis para usuários do Visual Web Developer Express.

## <a name="partial-page-updates"></a>Atualizações de página parcial

Talvez o recurso mais visível das extensões AJAX do ASP.NET seja a capacidade de fazer uma atualização parcial ou incremental de páginas sem fazer um postback completo para o servidor, sem alterações de código e alterações mínimas de marcação. As vantagens são extensivas – o estado de sua multimídia (como Adobe Flash ou Windows Media) não é alterado, os custos de largura de banda são reduzidos e o cliente não experimenta a cintilação geralmente associada a um postback.

A capacidade de integrar a renderização de página parcial é integrada ao ASP.NET com alterações mínimas em seu projeto.

## <a name="walkthrough-integrating-partial-rendering-into-an-existing-project"></a>Walkthrough: integrando o processamento parcial em um projeto existente

1. No Microsoft Visual Studio 2008, crie um novo projeto de site do ASP.NET acessando <em>arquivo</em> <em>-&gt; novo</em>  <em>site do-&gt;</em> e selecionando ASP.net site da caixa de diálogo. Você pode nomeá-lo como desejar e pode instalá-lo no sistema de arquivos ou no Serviços de Informações da Internet (IIS).
2. Você verá a página padrão em branco com a marcação ASP.NET básica (um formulário do lado do servidor e uma diretiva `@Page`). Descarte um rótulo chamado `Label1` e um botão chamado `Button1` na página dentro do elemento form. Você pode definir suas propriedades de texto para qualquer coisa que desejar.
3. Em modo de exibição de Design, clique duas vezes em `Button1` para gerar um manipulador de eventos code-behind. Nesse manipulador de eventos, defina `Label1.Text` para você clicar no botão! .

**Listagem 1: marcação para default. aspx antes da renderização parcial ser habilitada**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample1.aspx)]

**Listagem 2: codebehind (cortado) em default.aspx.cs**

[!code-csharp[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample2.cs)]

1. Pressione F5 para iniciar o site. O Visual Studio solicitará que você adicione um arquivo Web. config para habilitar a depuração; Faça isso. Quando você clica no botão, observe que a página é atualizada para alterar o texto no rótulo e há uma pequena oscilação à medida que a página é redesenhada.
2. Depois de fechar a janela do navegador, retorne ao Visual Studio e à página de marcação. Role para baixo na caixa de ferramentas do Visual Studio e localize a guia rotulada extensões AJAX. (Se você não tiver essa guia porque está usando uma versão mais antiga do AJAX ou das extensões do Atlas, consulte o tutorial para registrar os itens da caixa de ferramentas de extensões AJAX posteriormente neste white paper ou instale a versão atual com o Windows Installer baixável no site do).

[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image2.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image1.png)

([Clique para exibir a imagem em tamanho normal](understanding-partial-page-updates-with-asp-net-ajax/_static/image3.png))

1. <em>Problema conhecido:</em> Se você instalar o Visual Studio 2008 em um computador que já tenha o Visual Studio 2005 instalado com as extensões do ASP.NET 2,0 AJAX, o Visual Studio 2008 importará os itens da caixa de ferramentas de extensões AJAX. Você pode determinar se esse é o caso examinando a dica de ferramenta dos componentes; Eles devem dizer a versão 3.5.0.0. Se eles tiverem a versão 2.0.0.0, você importou seus antigos itens da caixa de ferramentas e precisará importá-los manualmente usando a caixa de diálogo escolher itens da caixa de ferramentas no Visual Studio. Você não poderá adicionar controles da versão 2 por meio do designer.

2. Antes que a marca de `<asp:Label>` comece, crie uma linha de espaço em branco e clique duas vezes no controle UpdatePanel na caixa de ferramentas. Observe que uma nova diretiva `@Register` está incluída na parte superior da página, indicando que os controles dentro do namespace System. Web. UI devem ser importados usando o prefixo `asp:`.
3. Arraste a marca de `</asp:UpdatePanel>` de fechamento após o final do elemento Button, para que o elemento esteja bem formado com os controles Label e Button encapsulados.
4. Após a marcação de `<asp:UpdatePanel>` de abertura, comece a abrir uma nova marca. Observe que o IntelliSense solicita duas opções. Nesse caso, crie uma marca de `<ContentTemplate>`. Certifique-se de encapsular essa marca em seu rótulo e botão para que a marcação seja bem formada.

[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image5.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image4.png)

([Clique para exibir a imagem em tamanho normal](understanding-partial-page-updates-with-asp-net-ajax/_static/image6.png))

1. Em qualquer lugar dentro do elemento `<form>`, inclua um controle ScriptManager clicando duas vezes no item `ScriptManager` na caixa de ferramentas.
2. Edite a marca de `<asp:ScriptManager>` de forma que ela inclua o atributo `EnablePartialRendering= true`.

**Listagem 3: marcação para default. aspx com processamento parcial habilitado**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample3.aspx)]

1. Abra o arquivo Web. config. Observe que o Visual Studio adicionou automaticamente uma referência de compilação a System. Web. Extensions. dll.

1. O que há de novo no Visual Studio 2008: o Web. config que vem com os modelos de projeto de site ASP.NET inclui automaticamente todas as referências necessárias para as extensões do ASP.NET AJAX e inclui seções comentadas de informações de configuração que podem ser Não comentado para habilitar a funcionalidade adicional. O Visual Studio 2005 tinha modelos semelhantes quando as extensões AJAX ASP.NET 2,0 foram instaladas. No entanto, no Visual Studio 2008, as extensões AJAX são recusadas por padrão (ou seja, são referenciadas por padrão, mas podem ser removidas como referências).

[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image8.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image7.png)

([Clique para exibir a imagem em tamanho normal](understanding-partial-page-updates-with-asp-net-ajax/_static/image9.png))

1. Pressione F5 para iniciar o site. Observe como nenhuma alteração no código-fonte era necessária para dar suporte à marcação parcial somente de renderização.

Ao iniciar seu site, você verá que o processamento parcial agora está habilitado, porque quando você clica no botão, não haverá cintilação, nem haverá nenhuma alteração na posição da rolagem da página (Este exemplo não demonstra isso). Se você examinar a origem renderizada da página depois de clicar no botão, ele confirmará que, na verdade, um postback não ocorreu-o texto do rótulo original ainda faz parte da marcação de origem e o rótulo foi alterado por meio de JavaScript.

O Visual Studio 2008 parece não vir com um modelo predefinido para um site da Web habilitado para AJAX ASP.NET. No entanto, esse modelo estava disponível no Visual Studio 2005 se as extensões do Visual Studio 2005 e do ASP.NET 2,0 AJAX foram instaladas. Consequentemente, configurar um site e começar com o modelo de site habilitado para AJAX provavelmente será ainda mais fácil, pois o modelo deve incluir um arquivo Web. config totalmente configurado (dando suporte a todas as extensões do AJAX ASP.NET, incluindo o acesso aos serviços da Web e a serialização JSON-JavaScript Object Notation) e inclui um UpdatePanel e ContentTemplate na página principal Web Forms por padrão. Habilitar o processamento parcial com essa página padrão é tão simples quanto revisitar a etapa 10 deste passo a passos e soltar controles na página.

## <a name="the-scriptmanager-control"></a>O controle do ScriptManager

## <a name="scriptmanager-control-reference"></a>Referência de controle do ScriptManager

Propriedades habilitadas para marcação:

| **Nome da propriedade** | **Tipo** | **Descrição** |
| --- | --- | --- |
| AllowCustomErrors-redirecionar | Bool | Especifica se a seção de erro personalizado do arquivo Web. config deve ser usada para tratar erros. |
| AsyncPostBackError-mensagem | Cadeia de Caracteres | Obtém ou define a mensagem de erro enviada ao cliente se um erro for gerado. |
| AsyncPostBack-tempo limite | Int32 | Obtém ou define o valor padrão de um tempo que um cliente deve aguardar até que a solicitação assíncrona seja concluída. |
| XsltSettings-globalização | Bool | Obtém ou define se a globalização de script está habilitada. |
| XsltSettings-localização | Bool | Obtém ou define se a localização de script está habilitada. |
| ScriptLoadTimeout | Int32 | Determina o número de segundos permitidos para carregar scripts no cliente |
| ScriptMode | Enum (automático, depurar, liberar, herdar) | Obtém ou define se as versões de lançamento de scripts devem ser renderizadas |
| ScriptPath | Cadeia de Caracteres | Obtém ou define o caminho raiz para o local dos arquivos de script a serem enviados ao cliente. |

Propriedades somente de código:

| **Nome da propriedade** | **Tipo** | **Descrição** |
| --- | --- | --- |
| AuthenticationService | Gerenciador de AuthenticationService | Obtém detalhes sobre o proxy do serviço de autenticação ASP.NET que será enviado ao cliente. |
| IsDebuggingEnabled | Bool | Obtém se a depuração de script e de código está habilitada. |
| IsInAsyncPostback | Bool | Obtém se a página está atualmente em uma solicitação de postback assíncrona. |
| ProfileService | Gerenciador de ProfileService | Obtém detalhes sobre o proxy de serviço de criação de perfil do ASP.NET que será enviado ao cliente. |
| Scripts | &gt; de referência de script de coleção&lt; | Obtém uma coleção de referências de script que serão enviadas ao cliente. |
| Serviços | Coleção&lt;&gt; de referência de serviço | Obtém uma coleção de referências de proxy de serviço Web que serão enviadas ao cliente. |
| SupportsPartialRendering | Bool | Obtém se o cliente atual dá suporte à renderização parcial. Se essa propriedade retornar **false**, todas as solicitações de página serão postagens padrão. |

Métodos de código público:

| **Nome do método** | **Tipo** | **Descrição** |
| --- | --- | --- |
| SetFocus (cadeia de caracteres) | Void | Define o foco do cliente para um controle específico quando a solicitação é concluída. |

Descendentes de marcação:

| **Tags** | **Descrição** |
| --- | --- |
| &gt; &lt;AuthenticationService | Fornece detalhes sobre o proxy para o serviço de autenticação ASP.NET. |
| &gt; de &lt;ProfileService | Fornece detalhes sobre o proxy para o serviço de criação de perfil do ASP.NET. |
| &lt;Scripts&gt; | Fornece referências de script adicionais. |
| &lt;ASP: ScriptReference&gt; | Denota uma referência de script específica. |
| &lt;Serviço&gt; | Fornece referências de serviço Web adicionais que terão classes de proxy geradas. |
| &lt;ASP:&gt; de imreferência | Denota uma referência de serviço Web específica. |

O controle ScriptManager é o núcleo essencial para as extensões do AJAX ASP.NET. Ele fornece acesso à biblioteca de scripts (incluindo o amplo sistema de tipos de script do lado do cliente), dá suporte à renderização parcial e fornece suporte extensivo para serviços ASP.NET adicionais (como autenticação e criação de perfil, mas também outros serviços Web). O controle ScriptManager também fornece suporte à globalização e à localização para os scripts do cliente.

## <a name="providing-alternative-and-supplemental-scripts"></a>Fornecendo scripts complementares e alternativos

Embora as extensões do Microsoft ASP.NET AJAX 2,0 incluam o código de script inteiro nas edições de depuração e de versão como recursos inseridos nos assemblies referenciados, os desenvolvedores estão livres para redirecionar o ScriptManager para arquivos de script personalizados, bem como para registrar scripts adicionais necessários.

Para substituir a associação padrão para os scripts normalmente incluídos (como aqueles que dão suporte ao namespace sys. WebForms e ao sistema de digitação personalizada), você pode se registrar para o evento `ResolveScriptReference` da classe ScriptManager. Quando esse método é chamado, o manipulador de eventos tem a oportunidade de alterar o caminho para o arquivo de script em questão; o Gerenciador de scripts enviará uma cópia diferente ou personalizada dos scripts para o cliente.

Além disso, as referências de script (representadas pela classe `ScriptReference`) podem ser incluídas programaticamente ou por meio de marcação. Para fazer isso, modifique programaticamente a coleção de `ScriptManager.Scripts` ou inclua `<asp:ScriptReference>` marcas na marca `<Scripts>`, que é um filho de primeiro nível do controle do ScriptManager.

## <a name="custom-error-handling-for-updatepanels"></a>Tratamento de erro personalizado para UpdatePanels

Embora as atualizações sejam manipuladas por gatilhos especificados por controles UpdatePanel, o suporte para tratamento de erros e mensagens de erro personalizadas é tratado pela instância de controle do ScriptManager de uma página. Isso é feito pela exposição de um evento, `AsyncPostBackError`, à página, que pode fornecer lógica de tratamento de exceção personalizada.

Ao consumir o evento AsyncPostBackError, você pode especificar a propriedade `AsyncPostBackErrorMessage`, que, em seguida, faz com que uma caixa de alerta seja gerada após a conclusão do retorno de chamada.

A personalização do lado do cliente também é possível em vez de usar a caixa de alerta padrão; por exemplo, talvez você queira exibir um elemento `<div>` personalizado em vez da caixa de diálogo modal do navegador padrão. Nesse caso, você pode manipular o erro no script de cliente:

**Listagem 5: script do lado do cliente para exibir erros personalizados**

[!code-html[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample4.html)]

Muito simplesmente, o script acima registra um retorno de chamada com o tempo de execução AJAX do lado do cliente para quando a solicitação assíncrona for concluída. Em seguida, ele verifica se um erro foi relatado e, nesse caso, processa os detalhes dele, por fim, indicando ao tempo de execução que o erro foi tratado no script personalizado.

## <a name="globalization-and-localization-support"></a>Suporte à globalização e à localização

O controle ScriptManager fornece amplo suporte para a localização de cadeias de caracteres de script e componentes de interface do usuário; no entanto, esse tópico está fora do escopo deste White Paper. Para obter mais informações, consulte o White Paper, suporte à globalização no ASP.NET AJAX Extensions.

## <a name="the-updatepanel-control"></a>O controle UpdatePanel

## <a name="updatepanel-control-reference"></a>Referência de controle UpdatePanel

Propriedades habilitadas para marcação:

| **Nome da propriedade** | **Tipo** | **Descrição** |
| --- | --- | --- |
| ChildrenAsTriggers | {1&gt;bool&lt;1} | Especifica se os controles filho invocam a atualização automaticamente no postback. |
| RenderMode | enum (bloco, embutido) | Especifica a maneira como o conteúdo será visualmente apresentado. |
| UpdateMode | enum (sempre, condicional) | Especifica se o UpdatePanel é sempre atualizado durante um processamento parcial ou se é atualizado apenas quando um gatilho é atingido. |

Propriedades somente de código:

| **Nome da propriedade** | **Tipo** | **Descrição** |
| --- | --- | --- |
| IsInPartialRendering | {1&gt;bool&lt;1} | Obtém se o UpdatePanel está dando suporte à renderização parcial para a solicitação atual. |
| ContentTemplate | ITemplate | Obtém o modelo de marcação para a solicitação de atualização. |
| ContentTemplateContainer | Controle | Obtém o modelo programático para a solicitação de atualização. |
| Gatilhos | UpdatePanel-TriggerCollection | Obtém a lista de gatilhos associados ao UpdatePanel atual. |

Métodos de código público:

| **Nome do método** | **Tipo** | **Descrição** |
| --- | --- | --- |
| Atualizar () | Void | Atualiza o UpdatePanel especificado programaticamente. Permite que uma solicitação do servidor dispare um processamento parcial de um UpdatePanel não disparado de outra forma. |

Descendentes de marcação:

| **Tags** | **Descrição** |
| --- | --- |
| &lt;ContentTemplate&gt; | Especifica a marcação a ser usada para renderizar o resultado da renderização parcial. Filho do &lt;ASP: UpdatePanel&gt;. |
| &lt;Gatilhos&gt; | Especifica uma coleção de *n* controles associados à atualização deste UpdatePanel. Filho do &lt;ASP: UpdatePanel&gt;. |
| &lt;ASP: AsyncPostBackTrigger&gt; | Especifica um gatilho que invoca um processamento de página parcial para o UpdatePanel especificado. Isso pode ou não ser um controle como um descendente do UpdatePanel em questão. Granular para o nome do evento. Filho de &lt;gatilhos&gt;. |
| &lt;ASP: PostBackTrigger&gt; | Especifica um controle que faz com que a página inteira seja atualizada. Isso pode ou não ser um controle como um descendente do UpdatePanel em questão. Granular para o objeto. Filho de &lt;gatilhos&gt;. |

O controle de `UpdatePanel` é o controle que delimita o conteúdo do lado do servidor que fará parte da funcionalidade de processamento parcial das extensões AJAX. Não há nenhum limite para o número de controles UpdatePanel que podem estar em uma página, e eles podem ser aninhados. Cada UpdatePanel é isolado, para que cada um possa trabalhar de forma independente (você pode ter dois UpdatePanels em execução ao mesmo tempo, Renderizando partes diferentes da página, independentemente do postback da página).

O controle UpdatePanel lida principalmente com gatilhos de controle – por padrão, qualquer controle contido no `ContentTemplate` de um UpdatePanel que cria um postback é registrado como um gatilho para o UpdatePanel. Isso significa que o UpdatePanel pode funcionar com os controles de associação de dados padrão (como o GridView), com controles de usuário, e eles podem ser programados em script.

Por padrão, quando um processamento de página parcial é disparado, todos os controles UpdatePanel na página serão atualizados, quer os controles UpdatePanel definidos ou não para tal ação. Por exemplo, se um UpdatePanel definir um controle de botão e esse controle de botão for clicado, todos os controles UpdatePanel nessa página serão atualizados por padrão. Isso ocorre porque, por padrão, a propriedade `UpdateMode` do UpdatePanel é definida como `Always`. Como alternativa, você pode definir a propriedade UpdateMode como `Conditional`, o que significa que o UpdatePanel será atualizado somente se um gatilho específico for atingido.

## <a name="custom-control-notes"></a>Notas de controle personalizado

Um UpdatePanel pode ser adicionado a qualquer controle de usuário ou controle personalizado; no entanto, a página na qual esses controles são incluídos também deve incluir um controle ScriptManager com a propriedade EnablePartialRendering definida como **true**.

Uma maneira na qual você pode considerar isso ao usar controles personalizados da Web é substituir o método de `CreateChildControls()` protegido da classe `CompositeControl`. Ao fazer isso, você pode injetar um UpdatePanel entre os filhos do controle e o mundo exterior se determinar que a página dá suporte à renderização parcial; caso contrário, você pode simplesmente colocar em camadas os controles filho em um contêiner `Control` instância.

## <a name="updatepanel-considerations"></a>Considerações sobre UpdatePanel

O UpdatePanel Opera como algo de uma caixa preta, encapsulando postbacks ASP.NET dentro do contexto de um XMLHttpRequest do JavaScript. No entanto, há considerações de desempenho significativas para se lembrar, tanto em termos de comportamento quanto em velocidade. Para entender como o UpdatePanel funciona, para que você possa decidir melhor quando seu uso é apropriado, você deve examinar a troca AJAX. O exemplo a seguir usa um site existente e o Mozilla Firefox com a extensão do Firebug (o Firebug captura dados XMLHttpRequest).

Considere um formulário que, entre outras coisas, tenha uma caixa de texto CEP que deve popular um campo de cidade e estado em um formulário ou controle. Esse formulário, por fim, coleta informações de associação, incluindo o nome, o endereço e as informações de contato do usuário. Há muitas considerações de design a serem levadas em conta, com base nos requisitos de um projeto específico.

[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image11.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image10.png)

([Clique para exibir a imagem em tamanho normal](understanding-partial-page-updates-with-asp-net-ajax/_static/image12.png))

[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image14.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image13.png)

([Clique para exibir a imagem em tamanho normal](understanding-partial-page-updates-with-asp-net-ajax/_static/image15.png))

Na iteração original deste aplicativo, foi criado um controle que incorporou o inteiro dos dados de registro do usuário, incluindo o código postal, a cidade e o estado. O controle inteiro foi encapsulado em um UpdatePanel e descartado em um formulário da Web. Quando o CEP é inserido pelo usuário, o UpdatePanel detecta o evento (o evento TextChanged correspondente no back-end, seja especificando gatilhos ou usando a propriedade ChildrenAsTriggers definida como true). O AJAX posta todos os campos dentro do UpdatePanel, conforme capturados pelo FireBug (consulte o diagrama à direita).

À medida que a captura de tela indica, os valores de cada controle dentro do UpdatePanel são entregues (nesse caso, estão todos vazios), bem como o campo ViewState. Todas as afirmadas, sobre 9KB de dados são enviadas, quando, na verdade, apenas cinco bytes de dados eram necessários para fazer essa solicitação específica. A resposta é ainda mais inflada: no total, o 57kb é enviado para o cliente, simplesmente para atualizar um campo de texto e um campo suspenso.

Também pode ser interessante ver como o ASP.NET AJAX atualiza a apresentação. A parte de resposta da solicitação de atualização do UpdatePanel é mostrada na exibição do console do Firebug à esquerda; é uma cadeia de caracteres especialmente delimitada por pipe, que é dividida pelo script do cliente e, em seguida, remontado na página. Especificamente, o ASP.NET AJAX define a propriedade *InnerHtml* do elemento HTML no cliente que representa o UpdatePanel. À medida que o navegador gera novamente o DOM, há um pequeno atraso, dependendo da quantidade de informações que precisam ser processadas.

A regeneração do DOM dispara vários problemas adicionais:

[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image17.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image16.png)

([Clique para exibir a imagem em tamanho normal](understanding-partial-page-updates-with-asp-net-ajax/_static/image18.png))

- Se o elemento HTML focalizado estiver dentro do UpdatePanel, ele perderá o foco. Portanto, para os usuários que pressionaram a tecla Tab para sair da caixa de texto CEP, o próximo destino teria sido a caixa de texto cidade. No entanto, uma vez que o UpdatePanel atualizasse a exibição, o formulário não teria mais foco e pressionar Tab teria começado a destacar os elementos de foco (como links).
- Se qualquer tipo de script personalizado do lado do cliente estiver em uso que acessa elementos DOM, as referências persistentes por funções poderão se tornar expiradas após um postback parcial.

Os UpdatePanels não se destinam a ser soluções de detecção. Em vez disso, eles fornecem uma solução rápida para determinadas situações, incluindo protótipos, pequenas atualizações de controle e fornecem uma interface familiar para desenvolvedores ASP.NET que podem estar familiarizados com o modelo de objeto .NET, mas com o DOM. Há várias alternativas que podem resultar em melhor desempenho, dependendo do cenário do aplicativo:

- Considere o uso de PageMethods e JSON (JavaScript Object Notation) permite que o desenvolvedor invoque métodos estáticos em uma página como se uma chamada de serviço Web estivesse sendo invocada. Como os métodos são estáticos, nenhum estado é necessário; o chamador de script fornece os parâmetros e o resultado é retornado de forma assíncrona.
- Considere usar um serviço Web e JSON se um único controle precisar ser usado em vários lugares em um aplicativo. Isso novamente requer muito pouco trabalho especial e funciona de forma assíncrona.

A incorporação da funcionalidade por meio de Web Services ou métodos de página também tem desvantagens. Em primeiro lugar, os desenvolvedores de ASP.NET geralmente tendem a criar componentes pequenos de funcionalidade em controles de usuário (arquivos. ascx). Os métodos de página não podem ser hospedados nesses arquivos; Eles devem ser hospedados na classe de página. aspx real. Os serviços Web, da mesma forma, devem ser hospedados na classe. asmx. Dependendo do aplicativo, essa arquitetura pode violar o princípio de responsabilidade única, pois a funcionalidade de um único componente agora está espalhada por dois ou mais componentes físicos que podem ter pouco ou nenhum vínculos coesos.

Por fim, se um aplicativo exigir que os UpdatePanels sejam usados, as diretrizes a seguir devem ajudar na solução de problemas e na manutenção.

- **Aninhe os UpdatePanels o mínimo possível, não apenas dentro das unidades, mas também nas unidades de código.** Por exemplo, ter um UpdatePanel em uma página que encapsula um controle, enquanto esse controle também contém um UpdatePanel, que contém outro controle que contém um UpdatePanel, é aninhamento entre unidades. Isso ajuda a manter claro quais elementos devem ser atualizados e impede atualizações inesperadas para UpdatePanels filho.
- **Mantenha a propriedade *ChildrenAsTriggers* definida como false e defina explicitamente os eventos de disparo.** Utilizar a coleção de `<Triggers>` é uma maneira muito mais clara de lidar com eventos e pode evitar um comportamento inesperado, ajudando com tarefas de manutenção e forçando um desenvolvedor a aceitar um evento.
- **Use a menor unidade possível para obter a funcionalidade.** Conforme observado na discussão do serviço de código postal, envolver apenas o tempo mínimo de redução para o servidor, o processamento total e a superfície da troca cliente-servidor, aprimorando o desempenho.

## <a name="the-updateprogress-control"></a>O controle UpdateProgress

## <a name="updateprogress-control-reference"></a>Referência de controle UpdateProgress

Propriedades habilitadas para marcação:

| **Nome da propriedade** | **Tipo** | **Descrição** |
| --- | --- | --- |
| AssociatedUpdate-Panelid | Cadeia de Caracteres | Especifica a ID do UpdatePanel no qual este UpdateProgress deve se reportar. |
| DisplayAfter | Int | Especifica o tempo limite em milissegundos antes que esse controle seja exibido após o início da solicitação assíncrona. |
| DynamicLayout | {1&gt;bool&lt;1} | Especifica se o progresso é renderizado dinamicamente. |

Descendentes de marcação:

| **Tags** | **Descrição** |
| --- | --- |
| &lt;ProgressTemplate&gt; | Contém o conjunto de modelos de controle para o conteúdo que será exibido com este controle. |

O controle UpdateProgress fornece uma medida de comentários para manter o interesse dos seus usuários ao fazer o trabalho necessário para transporte para o servidor. Isso pode ajudar os usuários a saber que você está fazendo algo, mesmo que ele não seja aparente, especialmente porque a maioria dos usuários são usados para a atualização de página e a exibição da barra de status é realçada.

Como observação, os controles UpdateProgress podem aparecer em qualquer lugar em uma hierarquia de página. No entanto, nos casos em que um postback parcial é iniciado a partir de um UpdatePanel filho (em que um UpdatePanel é aninhado dentro de outro UpdatePanel), os postbacks que disparam o UpdatePanel filho farão com que os modelos de UpdateProgress sejam exibidos para o filho UpdatePanel, bem como o UpdatePanel pai. Mas, se o gatilho for um filho direto do UpdatePanel pai, somente os modelos UpdateProgress associados ao pai serão exibidos.

## <a name="summary"></a>Resumo

O Microsoft ASP.NET extensões AJAX são produtos sofisticados projetados para ajudar a tornar o conteúdo da Web mais acessível e fornecer uma experiência de usuário mais rica aos seus aplicativos Web. Como parte das extensões do AJAX ASP.NET, os controles de renderização de página parcial, incluindo os controles ScriptManager, UpdatePanel e UpdateProgress, são alguns dos componentes mais visíveis do kit de ferramentas.

O componente do ScriptManager integra o provisionamento do JavaScript do cliente para as extensões, bem como permite que os vários componentes do lado do servidor e do cliente funcionem junto com o investimento mínimo em desenvolvimento.

O controle UpdatePanel é a caixa mágica aparente – marcação dentro do UpdatePanel pode ter code-behind do lado do servidor e não disparar uma atualização de página. Os controles UpdatePanel podem ser aninhados e podem ser dependentes de controles em outros UpdatePanels. Por padrão, os UpdatePanels lidam com quaisquer postbacks invocados por seus controles descendentes, embora essa funcionalidade possa ser ajustada de forma declarativa ou programaticamente.

Ao usar o controle UpdatePanel, os desenvolvedores devem estar cientes do impacto no desempenho que pode surgir potencialmente. As alternativas potenciais incluem serviços Web e métodos de página, embora o design do aplicativo deva ser considerado.

O controle UpdateProgress permite que o usuário saiba que ele não está sendo ignorado e que a solicitação em segundo plano está acontecendo enquanto a página não está fazendo nada para responder à entrada do usuário. Ele também inclui a capacidade de anular resultados de renderização parcial.

Juntas, essas ferramentas ajudam a criar uma experiência de usuário rica e direta, tornando o servidor mais aparente para o usuário e interrompendo o fluxo de trabalho menos.

## <a name="bio"></a>Biografia

Scott Cate tem trabalhado com tecnologias Web da Microsoft desde 1997 e é presidente da myKB.com ([www.myKB.com](http://www.myKB.com)), onde é especialista em escrever aplicativos baseados em ASP.net voltados para as soluções de software da base de dados de conhecimento. Scott pode ser contatado por email em [scott.cate@myKB.com](mailto:scott.cate@myKB.com) ou em seu blog em [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Avançar](understanding-asp-net-ajax-updatepanel-triggers.md)
