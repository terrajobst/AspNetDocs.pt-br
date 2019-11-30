---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
title: Compreendendo os gatilhos do ASP.NET AJAX UpdatePanel | Microsoft Docs
author: scottcate
description: Ao trabalhar no editor de marcação no Visual Studio, você pode observar (do IntelliSense) que há dois elementos filho de um controle UpdatePanel. Um de qu...
ms.author: riande
ms.date: 03/12/2008
ms.assetid: faab8503-2984-48a9-8a40-7728461abc50
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
msc.type: authoredcontent
ms.openlocfilehash: b1cc869f373d4f8283b4d92af74707c3f11fef61
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74588794"
---
# <a name="understanding-aspnet-ajax-updatepanel-triggers"></a>Noções básicas sobre os gatilhos UpdatePanel do AJAX ASP.NET

por [Scott Cate](https://github.com/scottcate)

[Baixar PDF](https://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial02_Triggers_cs.pdf)

> Ao trabalhar no editor de marcação no Visual Studio, você pode observar (do IntelliSense) que há dois elementos filho de um controle UpdatePanel. Um deles é o elemento triggers, que especifica os controles na página (ou o controle de usuário, se você estiver usando um) que irá disparar um processamento parcial do controle UpdatePanel no qual o elemento reside.

## <a name="introduction"></a>Introdução

A tecnologia ASP.NET da Microsoft traz um modelo de programação orientado a objeto e orientado a eventos e o une com os benefícios do código compilado. No entanto, seu modelo de processamento do lado do servidor tem várias desvantagens inerentes à tecnologia, muitas das quais podem ser abordadas pelos novos recursos incluídos nas extensões AJAX do Microsoft ASP.NET 3,5. Essas extensões habilitam muitos novos recursos avançados de cliente, incluindo o processamento parcial de páginas sem a necessidade de uma atualização de página completa, a capacidade de acessar serviços Web por meio de script de cliente (incluindo a API de criação de perfil do ASP.NET) e uma API extensiva do lado do cliente projetado para espelhar muitos dos esquemas de controle vistos no conjunto de controle do lado do servidor ASP.NET.

Este White Paper examina a funcionalidade de gatilhos XML do componente `UpdatePanel` AJAX ASP.NET. Gatilhos XML fornecem controle granular sobre os componentes que podem causar processamento parcial para controles UpdatePanel específicos.

Este White Paper se baseia na versão beta 2 do .NET Framework 3,5 e do Visual Studio 2008. As extensões AJAX do ASP.NET, anteriormente um assembly complementar direcionado ao ASP.NET 2,0, agora estão integradas à biblioteca de classes base .NET Framework. Este White Paper também pressupõe que você estará trabalhando com o Visual Studio 2008, não com o Visual Web Developer Express e fornecerá orientações de acordo com a interface do usuário do Visual Studio (embora as listagens de código sejam totalmente compatíveis, independentemente de ambiente de desenvolvimento).

## <a name="triggers"></a>*Gatilhos*

Os gatilhos de um determinado UpdatePanel, por padrão, incluem automaticamente todos os controles filho que invocam um postback, incluindo (por exemplo) controles de caixa de texto que têm sua propriedade `AutoPostBack` definida como **true**. No entanto, os gatilhos também podem ser incluídos declarativamente usando marcação; Isso é feito dentro da seção `<triggers>` da declaração de controle UpdatePanel. Embora os gatilhos possam ser acessados por meio da propriedade de coleção `Triggers`, é recomendável registrar quaisquer gatilhos de renderização parciais em tempo de execução (por exemplo, se um controle não estiver disponível em tempo de Design) usando o método `RegisterAsyncPostBackControl(Control)` do objeto ScriptManager para sua página, dentro do evento `Page_Load`. Lembre-se de que as páginas são sem monitoração de estado e, portanto, você deve registrar novamente esses controles toda vez que eles forem criados.

A inclusão de gatilho de filho automático também pode ser desabilitada (de modo que os controles filho que criam postbacks não disparam renderizações parciais automaticamente), definindo a propriedade `ChildrenAsTriggers` como **false**. Isso permite a você a maior flexibilidade na atribuição de quais controles específicos podem invocar um processamento de página e é recomendado para que um desenvolvedor opte por responder a um evento, em vez de manipular quaisquer eventos que possam surgir.

Observe que quando os controles UpdatePanel são aninhados, quando o UpdateMode é definido como **Conditional**, se o UpdatePanel filho for disparado, mas o pai não for, somente o UpdatePanel filho será atualizado. No entanto, se o UpdatePanel pai for atualizado, o UpdatePanel filho também será atualizado.

## <a name="the-lttriggersgt-element"></a>*O elemento&gt; de gatilhos &lt;*

Ao trabalhar no editor de marcação no Visual Studio, você pode observar (do IntelliSense) que há dois elementos filho de um controle de `UpdatePanel`. O elemento visto com mais frequência é o elemento `<ContentTemplate>`, que basicamente encapsula o conteúdo que será mantido pelo painel de atualização (o conteúdo para o qual estamos habilitando o processamento parcial). O outro elemento é o `<Triggers>` elemento, que especifica os controles na página (ou o controle de usuário, se você estiver usando um) que irá disparar um processamento parcial do controle UpdatePanel no qual o &lt;dispara&gt; elemento.

O elemento `<Triggers>` pode conter qualquer número de dois nós filho: `<asp:AsyncPostBackTrigger>` e `<asp:PostBackTrigger>`. Ambos aceitam dois atributos, `ControlID` e `EventName`e podem especificar qualquer controle dentro da unidade atual de encapsulamento (por exemplo, se o seu controle UpdatePanel residir em um controle de usuário da Web, você não deve tentar fazer referência a um controle na página na qual o controle de usuário residirá).

O elemento `<asp:AsyncPostBackTrigger>` é particularmente útil em que ele pode direcionar qualquer evento de um controle que existe como um filho de *qualquer* controle UpdatePanel na unidade de encapsulamento, não apenas o UpdatePanel sob o qual esse gatilho é um filho. Assim, qualquer controle pode ser feito para disparar uma atualização de página parcial.

Da mesma forma, o elemento `<asp:PostBackTrigger>` pode ser usado para disparar um processamento de página parcial, mas um que requer uma viagem completa para o servidor. Esse elemento Trigger também pode ser usado para forçar um processamento de página inteira quando um controle, de outra forma, normalmente dispararia um processamento parcial de página (por exemplo, quando existe um controle de `Button` no elemento `<ContentTemplate>` de um controle UpdatePanel). Novamente, o elemento PostBackTrigger pode especificar qualquer controle que seja filho de qualquer controle UpdatePanel na unidade atual de encapsulamento.

## <a name="lttriggersgt-element-reference"></a>*&lt;gatilhos&gt; referência de elemento*

*Descendentes de marcação:*

| **Tags** | **Descrição** |
| --- | --- |
| &lt;ASP: AsyncPostBackTrigger&gt; | Especifica um controle e evento que causará uma atualização parcial de página para o UpdatePanel que contém essa referência de gatilho. |
| &lt;ASP: PostBackTrigger&gt; | Especifica um controle e evento que causará uma atualização de página completa (uma atualização de página completa). Essa marca pode ser usada para forçar uma atualização completa quando um controle, de outra forma, dispararia a renderização parcial. |

## <a name="walkthrough-cross-updatepanel-triggers"></a>*Walkthrough: gatilhos entre UpdatePanels*

1. Crie uma nova página ASP.NET com um objeto ScriptManager definido para habilitar a renderização parcial. Adicionar dois UpdatePanels a esta página – na primeira, inclua um controle rótulo (Label1) e dois controles Button (button1 e BUTTON2). O Button1 deve dizer que clique para atualizar e o Button2 deve dizer clique para atualizá-lo ou algo junto com essas linhas. No segundo UpdatePanel, inclua apenas um controle rótulo (Label2), mas defina sua propriedade ForeColor como algo diferente do padrão para diferenciá-lo.
2. Defina a propriedade UpdateMode de ambas as marcas UpdatePanel para **condicional**.

**Listagem 1: marcação para default. aspx:** 

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample1.aspx)]

1. No manipulador de eventos de clique para Button1, defina Label1. Text e Label2. Text como algo dependente de tempo (como DateTime. Now. paraantigostring ()). Para o manipulador de eventos de clique para Button2, defina somente Label1. Text para o valor dependente de tempo.

**Listagem 2: codebehind (cortado) em default.aspx.cs:** 

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample2.cs)]

1. Pressione F5 para compilar e executar o projeto. Observe que, quando você clica em atualizar ambos os painéis, os dois rótulos alteram o texto; no entanto, quando você clica em atualizar este painel, somente as atualizações do Label1.

[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image2.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image1.png)

([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-updatepanel-triggers/_static/image3.png))

## <a name="under-the-hood"></a>*Nos bastidores*

Utilizando o exemplo que acabamos de construir, podemos dar uma olhada no que o AJAX ASP.NET está fazendo e como nossos gatilhos de painel cruzado do UpdatePanel funcionam. Para fazer isso, trabalharemos com o HTML de origem da página gerada, bem como a extensão do Mozilla Firefox chamada FireBug – com ele, podemos facilmente examinar os postbacks AJAX. Também usaremos a ferramenta de refletor .NET da Lutz Roeder. Ambas as ferramentas estão disponíveis gratuitamente online e podem ser encontradas com uma pesquisa na Internet.

Um exame do código-fonte da página mostra quase nada fora do comum; os controles UpdatePanel são renderizados como contêineres `<div>` e podemos ver que o recurso de script inclui fornecido pelo `<asp:ScriptManager>`. Há também algumas novas chamadas específicas do AJAX para o PageRequestManager que são internas à biblioteca de scripts do cliente AJAX. Por fim, vemos os dois contêineres UpdatePanel – um com os botões `<input>` processados com os dois controles `<asp:Label>` processados como contêineres `<span>`. (Se você inspecionar a árvore DOM no FireBug, observará que os rótulos estão esmaecidos para indicar que não estão produzindo conteúdo visível).

Clique no botão atualizar este painel e observe que o UpdatePanel superior será atualizado com a hora atual do servidor. No FireBug, escolha a guia Console para que você possa examinar a solicitação. Examine os parâmetros da solicitação POST primeiro:

[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image5.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image4.png)

([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-updatepanel-triggers/_static/image6.png))

Observe que o UpdatePanel indicou ao código AJAX do lado do servidor precisamente qual árvore de controle foi acionada por meio do parâmetro ScriptManager1: `Button1` do controle `UpdatePanel1`. Agora, clique no botão atualizar ambos os painéis. Em seguida, examinando a resposta, vemos uma série delimitada por pipe de variáveis definidas em uma cadeia de caracteres; especificamente, vemos o principal UpdatePanel, `UpdatePanel1`, tem todo o seu HTML enviado para o navegador. A biblioteca de scripts do cliente AJAX substitui o conteúdo HTML original do UpdatePanel pelo novo conteúdo por meio da propriedade `.innerHTML` e, portanto, o servidor envia o conteúdo alterado do servidor como HTML.

Agora, clique no botão atualizar ambos os painéis e examine os resultados do servidor. Os resultados são muito semelhantes-ambos os UpdatePanels recebem um novo HTML do servidor. Assim como no retorno de chamada anterior, o estado de página adicional é enviado.

Como podemos ver, como nenhum código especial é utilizado para executar um postback AJAX, a biblioteca de scripts de cliente AJAX é capaz de interceptar os postbacks de formulário sem nenhum código adicional. Os controles de servidor utilizam automaticamente o JavaScript para que eles não enviem automaticamente o formulário-ASP.NET injeta automaticamente o código para validação de formulário e estado já, obtido principalmente pela inclusão automática de recursos de script, a classe PostBackOptions e a classe ClientScriptmanager.

Por exemplo, considere um controle de caixa de seleção; Examine a desmontagem de classe no reflector do .NET. Para fazer isso, verifique se o seu assembly System. Web está aberto e navegue até a classe `System.Web.UI.WebControls.CheckBox`, abrindo o método `RenderInputTag`. Procure uma condicional que verifica a propriedade `AutoPostBack`:

[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image8.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image7.png)

([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-updatepanel-triggers/_static/image9.png))

Quando o postback automático está habilitado em um controle de `CheckBox` (por meio da propriedade AutoPostBack ser verdadeira), a marca de `<input>` resultante é, portanto, renderizada com um script de manipulação de eventos ASP.NET em seu atributo `onclick`. A interceptação do envio do formulário, então, permite que o ASP.NET AJAX seja injetado na página de forma não invasiva, ajudando a evitar possíveis alterações significativas que possam ocorrer utilizando uma substituição de cadeia de caracteres possivelmente imprecisa. Além disso, isso permite que *qualquer* controle ASP.net personalizado utilize o poder do ASP.NET AJAX sem qualquer código adicional para dar suporte ao seu uso em um contêiner UpdatePanel.

A funcionalidade `<triggers>` corresponde aos valores inicializados na chamada PageRequestManager para \_updateControls (Observe que a biblioteca de script do cliente do ASP.NET AJAX utiliza a Convenção de que os métodos, eventos e nomes de campo que começam com um sublinhado são marcados como internos e não se destinam ao uso fora da própria biblioteca). Com ele, podemos observar quais controles destinam-se a causar postbacks AJAX.

Por exemplo, vamos adicionar dois controles adicionais à página, deixando um controle fora dos UpdatePanels inteiramente e deixando um dentro de um UpdatePanel. Adicionaremos um controle de caixa de seleção no UpdatePanel superior e soltaremos uma DropDownList com um número de cores definido na lista. Aqui está a nova marcação:

**Listagem 3: nova marcação**

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample3.aspx)]

E aqui está o novo code-behind:

**Listagem 4: codebehind**

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample4.cs)]

A ideia por trás dessa página é que a lista suspensa seleciona uma das três cores para mostrar o segundo rótulo, que a caixa de seleção determina se ela está em negrito e se os rótulos exibem a data e a hora. A caixa de seleção não deve causar uma atualização do AJAX, mas a lista suspensa deve, embora não seja armazenada em um UpdatePanel.

[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image11.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image10.png)

([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-updatepanel-triggers/_static/image12.png))

Como é aparente na captura de tela acima, o botão mais recente a ser clicado foi o botão direito atualizar este painel, que atualizou o tempo superior, independentemente do tempo de inbase. A data também foi desligada entre cliques, pois a data é visível no rótulo inferior. Finalmente, o interesse é a cor do rótulo inferior: ele foi atualizado mais recentemente do que o texto do rótulo, o que demonstra que o estado do controle é importante e os usuários esperam que ele seja preservado por meio de postbacks AJAX. *No entanto*, a hora não foi atualizada. O tempo foi automaticamente repopulado por meio da persistência do \_\_campo VIEWSTATE da página que está sendo interpretada pelo tempo de execução ASP.NET quando o controle estava sendo rerenderizado no servidor. O código do servidor AJAX ASP.NET não reconhece em quais métodos os controles estão alterando o estado; Ele simplesmente preenche novamente a partir do estado de exibição e, em seguida, executa os eventos que são apropriados.

No entanto, deve-se indicar que eu Inicializa o tempo dentro da página\_evento de carregamento, o tempo seria incrementado corretamente. Consequentemente, os desenvolvedores devem ter cuidado para que o código apropriado esteja sendo executado durante os manipuladores de eventos apropriados e evitar o uso da carga de\_de página quando um manipulador de eventos de controle for apropriado.

## <a name="summary"></a>Resumo

O controle UpdatePanel das extensões AJAX do ASP.NET é versátil e pode utilizar vários métodos para identificar eventos de controle que devem fazer com que ele seja atualizado. Ele dá suporte à atualização automática por seus controles filho, mas também pode responder a eventos de controle em outro lugar na página.

Para reduzir o potencial de carga de processamento do servidor, é recomendável que a propriedade `ChildrenAsTriggers` de um UpdatePanel seja definida como `false`e que os eventos sejam aceitos em vez de incluídos por padrão. Isso também impede que os eventos desnecessários causem efeitos potencialmente indesejados, incluindo validação e alterações em campos de entrada. Esses tipos de bugs podem ser difíceis de isolar, pois a página é atualizada de forma transparente para o usuário e a causa, portanto, pode não ser imediatamente óbvia.

Ao examinar o funcionamento interno do modelo de interceptação de postagem de formulário ASP.NET AJAX, pudemos determinar que ele utiliza a estrutura já fornecida pelo ASP.NET. Ao fazer isso, preserva a compatibilidade máxima com controles criados usando a mesma estrutura e invasos minimamente em qualquer JavaScript adicional escrito para a página.

## <a name="bio"></a>Biografia

Rob Paveza é desenvolvedor de aplicativos .NET sênior na Terralever ([www.Terralever.com](http://www.terralever.com)), uma empresa de marketing interativa líder em Tempe, AZ. Ele pode ser acessado em [robpaveza@gmail.com](mailto:robpaveza@gmail.com)e seu blog está localizado em [http://geekswithblogs.net/robp/](http://geekswithblogs.net/robp/).

Scott Cate tem trabalhado com tecnologias Web da Microsoft desde 1997 e é presidente da myKB.com ([www.myKB.com](http://www.myKB.com)), onde é especialista em escrever aplicativos baseados em ASP.net voltados para as soluções de software da base de dados de conhecimento. Scott pode ser contatado por email em [scott.cate@myKB.com](mailto:scott.cate@myKB.com) ou em seu blog em [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Anterior](understanding-partial-page-updates-with-asp-net-ajax.md)
> [Próximo](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
