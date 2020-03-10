---
uid: web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
title: O modelo de página do ASP.NET 2,0 | Microsoft Docs
author: microsoft
description: No ASP.NET 1. x, os desenvolvedores tinham uma opção entre um modelo de código embutido e um modelo de código code-behind. O code-behind pode ser implementado usando o atributo src...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: af4575a3-0ae3-4638-ba4d-218fad7a1642
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
msc.type: authoredcontent
ms.openlocfilehash: bcb71b2b5a484e8756406867e08e8aa699a9024d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78547645"
---
# <a name="the-aspnet-20-page-model"></a>O modelo de página do ASP.NET 2,0

pela [Microsoft](https://github.com/microsoft)

> No ASP.NET 1. x, os desenvolvedores tinham uma opção entre um modelo de código embutido e um modelo de código code-behind. O code-behind pode ser implementado usando o atributo src ou o atributo CodeBehind da diretiva @Page. No ASP.NET 2,0, os desenvolvedores ainda têm a opção de codificar código embutido e code-behind, mas houve aprimoramentos significativos no modelo code-behind.

No ASP.NET 1. x, os desenvolvedores tinham uma opção entre um modelo de código embutido e um modelo de código code-behind. O code-behind pode ser implementado usando o atributo src ou o atributo CodeBehind da diretiva @Page. No ASP.NET 2,0, os desenvolvedores ainda têm a opção de codificar código embutido e code-behind, mas houve aprimoramentos significativos no modelo code-behind.

## <a name="improvements-in-the-code-behind-model"></a>Aprimoramentos no modelo code-behind

Para entender totalmente as alterações no modelo code-behind no ASP.NET 2,0, seu melhor para examinar rapidamente o modelo como existia no ASP.NET 1. x.

## <a name="the-code-behind-model-in-aspnet-1x"></a>O modelo code-behind no ASP.NET 1. x

No ASP.NET 1. x, o modelo code-behind consistia em um arquivo ASPX (o webform) e um arquivo code-behind que contém o código de programação. Os dois arquivos foram conectados usando a diretiva @Page no arquivo ASPX. Cada controle na página ASPX tinha uma declaração correspondente no arquivo code-behind como uma variável de instância. O arquivo code-behind também continha código para associação de eventos e código gerado necessário para o designer do Visual Studio. Esse modelo funcionou muito bem, mas como cada elemento ASP.NET na página ASPX exigia o código correspondente no arquivo code-behind, não havia nenhuma separação real de código e conteúdo. Por exemplo, se um designer adicionar um novo controle de servidor a um arquivo ASPX fora do IDE do Visual Studio, o aplicativo será interrompido devido à ausência de uma declaração para esse controle no arquivo code-behind.

## <a name="the-code-behind-model-in-aspnet-20"></a>O modelo code-behind no ASP.NET 2,0

O ASP.NET 2,0 melhora muito esse modelo. No ASP.NET 2,0, o code-behind é implementado usando as novas *classes parciais* fornecidas no ASP.NET 2,0. A classe code-behind no ASP.NET 2,0 é definida como uma classe parcial, o que significa que ela contém apenas parte da definição de classe. A parte restante da definição de classe é gerada dinamicamente pelo ASP.NET 2,0 usando a página ASPX em tempo de execução ou quando o site é pré-compilado. O vínculo entre o arquivo code-behind e a página ASPX ainda é estabelecido usando a diretiva @ Page. No entanto, em vez de um atributo CodeBehind ou src, ASP.NET 2,0 agora usa o atributo CodeFile. O atributo Inherits também é usado para especificar o nome da classe para a página.

Uma diretiva @ Page típica pode ser parecida com esta:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample1.aspx)]

Uma definição de classe típica em um arquivo code-behind ASP.NET 2,0 pode ser assim:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample2.cs)]

> [!NOTE]
> C#e Visual Basic são as únicas linguagens gerenciadas que atualmente dão suporte a classes parciais. Portanto, os desenvolvedores que usam o J# não poderão usar o modelo code-behind no ASP.NET 2,0.

O novo modelo aprimora o modelo code-behind porque os desenvolvedores agora terão arquivos de código que contêm apenas o código que eles criaram. Ele também fornece uma separação real de código e conteúdo porque não há declarações de variável de instância no arquivo code-behind.

> [!NOTE]
> Como a classe Partial para a página ASPX é onde ocorre a vinculação de eventos, Visual Basic os desenvolvedores podem perceber um pequeno aumento de desempenho usando a palavra-chave Handles no code-behind para associar eventos. C#Não tem nenhuma palavra-chave equivalente.

## <a name="new--page-directive-attributes"></a>Novos atributos de diretiva @ Page

ASP.NET 2,0 adiciona muitos atributos novos à diretiva @ Page. Os seguintes atributos são novos no ASP.NET 2,0.

## <a name="async"></a>Assíncrono

O atributo Async permite que você configure a página a ser executada de forma assíncrona. Bem, aborde as páginas assíncronas posteriormente neste módulo.

## <a name="asynctimeout"></a>AsyncTimeout

Especificou o tempo limite para páginas assíncronas. O padrão é 45 segundos.

## <a name="codefile"></a>CodeFile

O atributo CodeFile é a substituição para o atributo CodeBehind no Visual Studio 2002/2003.

### <a name="codefilebaseclass"></a>CodeFileBaseClass

O atributo CodeFileBaseClass é usado nos casos em que você deseja que várias páginas derivem de uma classe base única. Devido à implementação de classes parciais em ASP.NET, sem esse atributo, uma classe base que usa campos comuns compartilhados para referenciar controles declarados em uma página ASPX não funcionaria corretamente porque o mecanismo de compilação ASP. Networks criará automaticamente novos membros com base em controles na página. Portanto, se você quiser uma classe base comum para duas ou mais páginas em ASP.NET, será necessário definir especificar sua classe base no atributo CodeFileBaseClass e, em seguida, derivar cada classe de páginas dessa classe base. O atributo CodeFile também é necessário quando esse atributo é usado.

## <a name="compilationmode"></a>CompilationMode

Esse atributo permite que você defina a propriedade CompilationMode da página ASPX. A propriedade compilationMode é uma enumeração que contém os valores **Always**, **auto**e **Never**. O padrão é **sempre**. A configuração **automática** impedirá ASP.net de compilar a página dinamicamente, se possível. A exclusão de páginas da compilação dinâmica aumenta o desempenho. No entanto, se uma página excluída contiver esse código que deve ser compilado, um erro será gerado quando a página for procurada.

## <a name="enableeventvalidation"></a>EnableEventValidation

Esse atributo especifica se os eventos de postback e de retorno de chamada são validados ou não. Quando habilitado, os argumentos para eventos de postback ou de retorno de chamada são verificados para garantir que eles se originam do controle de servidor que os processamentou originalmente.

## <a name="enabletheming"></a>EnableTheming

Esse atributo especifica se os temas ASP.NET são usados ou não em uma página. O padrão é **false**. Os temas do ASP.NET são abordados no [módulo 10](profiles-themes-and-web-parts.md).

## <a name="linepragmas"></a>LinePragmas

Esse atributo especifica se os pragmas de linha devem ser adicionados durante a compilação. Os pragmas de linha são opções usadas por depuradores para marcar seções específicas de código.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostback

Esse atributo especifica se o JavaScript é injetado na página para manter a posição de rolagem entre postbacks. Esse atributo é **false** por padrão.

Quando esse atributo for **true**, ASP.net adicionará um script &lt;bloco de&gt; em um postback semelhante a este:

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample3.html)]

Observe que a src para esse bloco de script é WebResource. axd. Este recurso não é um caminho físico. Quando esse script é solicitado, o ASP.NET compila dinamicamente o script.

### <a name="masterpagefile"></a>MasterPageFile

Esse atributo especifica o arquivo de página mestra da página atual. O caminho pode ser relativo ou absoluto. As páginas mestras são abordadas no [módulo 4](master-pages.md).

## <a name="stylesheettheme"></a>StyleSheetTheme

Esse atributo permite que você substitua as propriedades de aparência da interface do usuário definidas por um tema do ASP.NET 2,0. Os temas são abordados no [módulo 10](profiles-themes-and-web-parts.md).

## <a name="theme"></a>Tema

Especifica o tema para a página. Se um valor não for especificado para o atributo StyleSheetTheme, o atributo Theme substituirá todos os estilos aplicados aos controles na página.

## <a name="title"></a>Title

Define o título da página. O valor especificado aqui será exibido no elemento &lt;title&gt; da página renderizada.

### <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

Define o valor para a enumeração ViewStateEncryptionMode. Os valores disponíveis são **Always**, **auto**e **neve**. O valor padrão é **auto**. Quando esse atributo é definido como um valor de **auto**, ViewState é criptografado é um controle que o solicita chamando o método **RegisterRequiresViewStateEncryption** .

## <a name="setting-public-property-values-via-the--page-directive"></a>Definindo valores de propriedade pública por meio da diretiva @ Page

Outro novo recurso da diretiva @ Page no ASP.NET 2,0 é a capacidade de definir o valor inicial das propriedades públicas de uma classe base. Suponha, por exemplo, que você tenha uma propriedade pública chamada **SomeText** em sua classe base e d como ela será inicializada para **Olá** quando uma página for carregada. Você pode fazer isso simplesmente definindo o valor na diretiva @ Page da seguinte forma:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample4.aspx)]

O atributo **SomeText** da diretiva @ Page define o valor inicial da propriedade SomeText na classe base como *Hello!* . O vídeo abaixo é uma explicação da definição do valor inicial de uma propriedade pública em uma classe base usando a diretiva @ Page.

![](the-asp-net-2-0-page-model/_static/image1.png)

[Abrir vídeo de tela inteira](the-asp-net-2-0-page-model/_static/setprop1.wmv)

## <a name="new-public-properties-of-the-page-class"></a>Novas propriedades públicas da classe Page

As propriedades públicas a seguir são novas no ASP.NET 2,0.

## <a name="apprelativetemplatesourcedirectory"></a>AppRelativeTemplateSourceDirectory

Retorna o caminho relativo do aplicativo para a página ou controle. Por exemplo, para uma página localizada em http://app/folder/page.aspx, a propriedade retorna ~/Folder/.

## <a name="apprelativevirtualpath"></a>AppRelativeVirtualPath

Retorna o caminho relativo do diretório virtual para a página ou o controle. Por exemplo, para uma página localizada em http://app/folder/page.aspx, a propriedade retorna ~/Folder/Page.aspx.

## <a name="asynctimeout"></a>AsyncTimeout

Obtém ou define o tempo limite usado para manipulação de página assíncrona. (As páginas assíncronas serão abordadas posteriormente neste módulo.)

## <a name="clientquerystring"></a>ClientQueryString

Uma propriedade somente leitura que retorna a parte da cadeia de caracteres de consulta da URL solicitada. Esse valor é codificado por URL. Você pode usar o método UrlDecode da classe HttpServerUtility para decodificá-lo.

## <a name="clientscript"></a>Property

Essa propriedade retorna um objeto ClientScriptmanager que pode ser usado para gerenciar a emissão de páginas ASP. sub-redes do script do lado do cliente. (A classe ClientScriptmanager é abordada posteriormente neste módulo.)

## <a name="enableeventvalidation"></a>EnableEventValidation

Essa propriedade controla se a validação de evento está habilitada ou não para eventos de postback e de retorno de chamada. Quando habilitado, os argumentos para eventos de postback ou de retorno de chamada são verificados para garantir que eles tenham origem no controle de servidor que os gerou originalmente.

## <a name="enabletheming"></a>EnableTheming

Essa propriedade Obtém ou define um booliano que especifica se um tema ASP.NET 2,0 se aplica à página.

## <a name="form"></a>Formulário

Essa propriedade retorna o formulário HTML na página ASPX como um objeto HtmlForm.

## <a name="header"></a>Cabeçalho

Essa propriedade retorna uma referência a um objeto HtmlHead que contém o cabeçalho da página. Você pode usar o objeto HtmlHead retornado para obter/definir folhas de estilo, marcas meta, etc.

## <a name="idseparator"></a>IdSeparator

Essa propriedade somente leitura Obtém o caractere que é usado para separar identificadores de controle quando ASP.NET está criando uma ID exclusiva para controles em uma página. Não é destinado a ser utilizado diretamente do seu código.

## <a name="isasync"></a>IsAsync

Essa propriedade permite páginas assíncronas. As páginas assíncronas são discutidas posteriormente neste módulo.

## <a name="iscallback"></a>IsCallback

Essa propriedade somente leitura retornará **true** se a página for o resultado de um retorno de chamada. Os backups de chamada são discutidos posteriormente neste módulo.

## <a name="iscrosspagepostback"></a>IsCrossPagePostBack

Essa propriedade somente leitura retornará **true** se a página fizer parte de um postback de página cruzada. Os postbacks entre páginas são abordados posteriormente neste módulo.

## <a name="items"></a>Itens

Retorna uma referência a uma instância de IDictionary que contém todos os objetos armazenados no contexto de páginas. Você pode adicionar itens a esse objeto IDictionary e eles estarão disponíveis durante todo o tempo de vida do contexto.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostBack

Essa propriedade controla se o ASP.NET emite ou não o JavaScript que mantém a posição de rolagem de páginas no navegador após a ocorrência de um postback. (Os detalhes dessa propriedade foram discutidos anteriormente neste módulo.)

## <a name="master"></a>Mestre

Essa propriedade somente leitura retorna uma referência à instância de MasterPage para uma página à qual uma página mestra foi aplicada.

## <a name="masterpagefile"></a>MasterPageFile

Obtém ou define o nome de arquivo da página mestra da página. Essa propriedade só pode ser definida no método PreInit.

## <a name="maxpagestatefieldlength"></a>MaxPageStateFieldLength

Esta propriedade Obtém ou define o comprimento máximo para o estado de páginas em bytes. Se a propriedade for definida como um número positivo, o estado de exibição de páginas será dividido em vários campos ocultos para que ele não exceda o número de bytes especificados. Se a propriedade for um número negativo, o estado de exibição não será dividido em partes.

## <a name="pageadapter"></a>PageAdapter

Retorna uma referência ao objeto PageAdapter que modifica a página do navegador solicitante.

## <a name="previouspage"></a>PreviousPage

Retorna uma referência à página anterior em casos de um Server. Transfer ou um postback de página cruzada.

## <a name="skinid"></a>SkinID

Especifica a capa ASP.NET 2,0 a ser aplicada à página.

## <a name="stylesheettheme"></a>StyleSheetTheme

Esta propriedade Obtém ou define a folha de estilos que é aplicada a uma página.

## <a name="templatecontrol"></a>TemplateControl

Retorna uma referência ao controle que o contém para a página.

## <a name="theme"></a>Tema

Obtém ou define o nome do tema do ASP.NET 2,0 aplicado à página. Esse valor deve ser definido antes do método PreInit.

## <a name="title"></a>Title

Essa propriedade Obtém ou define o título da página, conforme obtido no cabeçalho de páginas.

## <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

Obtém ou define o ViewStateEncryptionMode da página. Consulte uma discussão detalhada sobre essa propriedade anteriormente neste módulo.

## <a name="new-protected-properties-of-the-page-class"></a>Novas propriedades protegidas da classe de página

A seguir estão as novas propriedades protegidas da classe Page no ASP.NET 2,0.

## <a name="adapter"></a>Personalizado

Retorna uma referência ao ControlAdapter que renderiza a página no dispositivo que o solicitou.

## <a name="asyncmode"></a>AsyncMode

Essa propriedade indica se a página é processada ou não de forma assíncrona. Ele é destinado ao uso pelo tempo de execução e não diretamente no código.

## <a name="clientidseparator"></a>ClientIDSeparator

Essa propriedade retorna o caractere usado como um separador ao criar IDs de cliente exclusivas para controles. Ele é destinado ao uso pelo tempo de execução e não diretamente no código.

## <a name="pagestatepersister"></a>PageStatePersister

Essa propriedade retorna o objeto PageStatePersister para a página. Essa propriedade é usada principalmente por desenvolvedores de controle ASP.NET.

## <a name="uniquefilepathsuffix"></a>UniqueFilePathSuffix

Essa propriedade retorna um sufixo exclusivo que é anexado ao caminho do arquivo para os navegadores de cache. O valor padrão é \_\_ufps = e um número de seis dígitos.

## <a name="new-public-methods-for-the-page-class"></a>Novos métodos públicos para a classe Page

Os métodos públicos a seguir são novos na classe de página no ASP.NET 2,0.

## <a name="addonprerendercompleteasync"></a>AddOnPreRenderCompleteAsync

Esse método registra delegados de manipulador de eventos para a execução de página assíncrona. As páginas assíncronas são discutidas posteriormente neste módulo.

## <a name="applystylesheetskin"></a>ApplyStyleSheetSkin

Aplica as propriedades em uma folha de estilos de páginas à página.

## <a name="executeregisteredasynctasks"></a>ExecuteRegisteredAsyncTasks

Esse método se forma com uma tarefa assíncrona.

### <a name="getvalidators"></a>Getvalidadores

Retorna uma coleção de validadores para o grupo de validação especificado ou o grupo de validação padrão se nenhum for especificado.

## <a name="registerasynctask"></a>RegisterAsyncTask

Esse método registra uma nova tarefa assíncrona. As páginas assíncronas são abordadas posteriormente neste módulo.

## <a name="registerrequirescontrolstate"></a>RegisterRequiresControlState

Esse método informa ao ASP.NET que o estado de controle das páginas deve ser persistente.

## <a name="registerrequiresviewstateencryption"></a>RegisterRequiresViewStateEncryption

Esse método informa ao ASP.NET que o ViewState Pages requer criptografia.

## <a name="resolveclienturl"></a>ResolveClientUrl

Retorna uma URL relativa que pode ser usada para solicitações de cliente para imagens, etc.

## <a name="setfocus"></a>SetFocus

Esse método definirá o foco para o controle que é especificado quando a página é carregada inicialmente.

## <a name="unregisterrequirescontrolstate"></a>UnregisterRequiresControlState

Esse método cancelará o registro do controle que é passado para ele, pois não precisa mais de persistência de estado de controle.

## <a name="changes-to-the-page-lifecycle"></a>Alterações no ciclo de vida da página

O ciclo de vida da página no ASP.NET 2,0 não mudou drasticamente, mas há alguns novos métodos dos quais você deve estar atento. O ciclo de vida da página ASP.NET 2,0 é descrito abaixo.

## <a name="preinit-new-in-aspnet-20"></a>PreInit (novo no ASP.NET 2,0)

O evento PreInit é o primeiro estágio do ciclo de vida que um desenvolvedor pode acessar. A adição desse evento possibilita alterar programaticamente os temas do ASP.NET 2,0, as páginas mestras, as propriedades de acesso de um perfil ASP.NET 2,0, etc. Se você estiver em um estado de postback, é importante perceber que ViewState ainda não foi aplicado aos controles neste ponto do ciclo de vida. Portanto, se um desenvolvedor alterar uma propriedade de um controle nesse estágio, ele provavelmente será substituído posteriormente no ciclo de vida das páginas.

## <a name="init"></a>Init

O evento init não foi alterado de ASP.NET 1. x. É aí que você desejaria ler ou inicializar propriedades de controles em sua página. Neste estágio, as páginas mestras, os temas, etc. já estão aplicados à página.

## <a name="initcomplete-new-in-20"></a>InitComplete (novo em 2,0)

O evento InitComplete é chamado no final do estágio de inicialização de páginas. Neste ponto do ciclo de vida, você pode acessar controles na página, mas seu estado ainda não foi preenchido.

## <a name="preload-new-in-20"></a>Pré-carregar (novo no 2,0)

Esse evento é chamado depois que todos os dados de postback são aplicados e logo antes da carga de\_da página.

## <a name="load"></a>Carregar

O evento de carregamento não foi alterado de ASP.NET 1. x.

## <a name="loadcomplete-new-in-20"></a>LoadComplete (novo em 2,0)

O evento LoadComplete é o último evento no estágio de carregamento de páginas. Neste estágio, todos os dados de postback e ViewState foram aplicados à página.

## <a name="prerender"></a>PreRender

Se você quiser que o ViewState seja mantido adequadamente para controles adicionados à página dinamicamente, o evento PreRender será a última oportunidade para adicioná-los.

## <a name="prerendercomplete-new-in-20"></a>PreRenderComplete (novo no 2,0)

No estágio PreRenderComplete, todos os controles foram adicionados à página e a página está pronta para ser renderizada. O evento PreRenderComplete é o último evento gerado antes que o ViewState das páginas seja salvo.

## <a name="savestatecomplete-new-in-20"></a>SaveStateComplete (novo em 2,0)

O evento SaveStateComplete é chamado imediatamente após a gravação de todos os ViewState da página e do estado de controle. Esse é o último evento antes que a página seja realmente renderizada para o navegador.

## <a name="render"></a>Renderizar

O método Render não foi alterado desde ASP.NET 1. x. É aí que o HtmlTextWriter é inicializado e a página é renderizada para o navegador.

## <a name="cross-page-postback-in-aspnet-20"></a>Postback de página cruzada no ASP.NET 2,0

No ASP.NET 1. x, os postbacks eram necessários para postar na mesma página. Não foram permitidos postbacks entre páginas. O ASP.NET 2,0 adiciona a capacidade de postar novamente em uma página diferente por meio da interface IButtonControl. Qualquer controle que implemente a nova interface IButtonControl (Button, LinkButton e ImageButton, além de controles personalizados de terceiros) pode aproveitar essa nova funcionalidade por meio do uso do atributo PostBackUrl. O código a seguir mostra um controle de botão que é Postado de volta para uma segunda página.

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample5.aspx)]

Quando a página é postada de volta, a página que inicia o postback é acessível por meio da propriedade PreviousPageType na segunda página. Essa funcionalidade é implementada por meio do novo webform\_função do lado do cliente DoPostBackWithOptions que o ASP.NET 2,0 renderiza para a página quando um controle é Postado de volta para uma página diferente. Essa função JavaScript é fornecida pelo novo manipulador WebResource. axd que emite script para o cliente.

O vídeo abaixo é uma explicação detalhada de um postback de página cruzada.

![](the-asp-net-2-0-page-model/_static/image2.png)

[Abrir vídeo de tela inteira](the-asp-net-2-0-page-model/_static/xpage1.wmv)

## <a name="more-details-on-cross-page-postbacks"></a>Mais detalhes sobre postbacks entre páginas

### <a name="viewstate"></a>ViewState

Você já deve ter se perguntado sobre o que acontece com o ViewState da primeira página em um cenário de postback entre páginas. Afinal, qualquer controle que não implementa IPostBackDataHandler manterá seu estado por meio de ViewState, portanto, para ter acesso às propriedades desse controle na segunda página de um postback de página cruzada, você deve ter acesso ao ViewState da página. O ASP.NET 2,0 cuida desse cenário usando um novo campo oculto na segunda página chamada \_\_de PREVIOUSPAGETYPE. O campo de formulário \_\_PREVIOUSPAGETYPE contém o ViewState da primeira página para que você possa ter acesso às propriedades de todos os controles na segunda página.

### <a name="circumventing-findcontrol"></a>Contornar o FindControl

No tutorial em vídeo de um postback de página cruzada, usei o método FindControl para obter uma referência ao controle TextBox na primeira página. Esse método funciona bem para essa finalidade, mas FindControl é caro e requer a escrita de código adicional. Felizmente, o ASP.NET 2,0 fornece uma alternativa ao FindControl para essa finalidade que funcionará em muitos cenários. A diretiva PreviousPageType permite que você tenha uma referência fortemente tipada à página anterior usando o TypeName ou o atributo VirtualPath. O atributo TypeName permite que você especifique o tipo da página anterior, enquanto o atributo VirtualPath permite que você faça referência à página anterior usando um caminho virtual. Depois de definir a diretiva PreviousPageType, você deve expor os controles, etc. para os quais você deseja permitir o acesso usando propriedades públicas.

## <a name="lab-1-cross-page-postback"></a>Postagem entre páginas do laboratório 1

Neste laboratório, você criará um aplicativo que usa a nova funcionalidade de postback de página cruzada do ASP.NET 2,0.

1. Abra o Visual Studio 2005 e crie um novo site da ASP.NET.
2. Adicione um novo WebForms chamado página2. aspx.
3. Abra o default. aspx em modo de exibição de Design e adicione um controle de botão e um controle TextBox. 

    1. Dê ao botão controle uma ID de **SubmitButton** e o controle TEXTBOX uma ID de **username**.
    2. Defina a propriedade PostBackUrl do botão como página2. aspx.
4. Abra o página2. aspx na exibição de origem.
5. Adicione uma diretiva @ PreviousPageType, conforme mostrado abaixo:
6. Adicione o seguinte código à página\_carga do code-behind da página2. aspx: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample6.cs)]
7. Crie o projeto clicando em Compilar no menu Compilar.
8. Adicione o código a seguir ao code-behind para default. aspx: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample7.cs)]
9. Altere a página\_carregar em página2. aspx para o seguinte: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample8.cs)]
10. Compile o projeto.
11. Execute o projeto.
12. Insira seu nome na caixa de texto e clique no botão.
13. Qual é o resultado?

## <a name="asynchronous-pages-in-aspnet-20"></a>Páginas assíncronas no ASP.NET 2,0

Muitos problemas de contenção em ASP.NET são causados pela latência de chamadas externas (como chamadas de banco de dados ou serviço Web), latência de e/s de arquivo, etc. Quando uma solicitação é feita em um aplicativo ASP.NET, o ASP.NET usa um de seus threads de trabalho para atender a essa solicitação. Essa solicitação possui esse thread até que a solicitação seja concluída e a resposta seja enviada. O ASP.NET 2,0 busca resolver problemas de latência com esses tipos de problemas adicionando a capacidade de executar páginas de forma assíncrona. Isso significa que um thread de trabalho pode iniciar a solicitação e, em seguida, entregar a execução adicional para outro thread, retornando, assim, o pool de threads disponível rapidamente. Quando a e/s de arquivo, chamada de banco de dados, etc. tiver sido concluída, um novo thread será obtido do pool de threads para concluir a solicitação.

A primeira etapa para fazer uma página ser executada de forma assíncrona é definir o atributo **Async** da diretiva Page da seguinte forma:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample9.aspx)]

Esse atributo informa ao ASP.NET para implementar o IHttpAsyncHandler para a página.

A próxima etapa é chamar o método AddOnPreRenderCompleteAsync em um ponto no ciclo de vida da página antes de PreRender. (Esse método é normalmente chamado na página de\_carga.) O método AddOnPreRenderCompleteAsync usa dois parâmetros; um BeginEventHandler e um EndEventHandler. O BeginEventHandler retorna um IAsyncResult que é passado como um parâmetro para o EndEventHandler.

O vídeo abaixo é uma explicação de uma solicitação de página assíncrona.

![](the-asp-net-2-0-page-model/_static/image3.png)

[Abrir vídeo de tela inteira](the-asp-net-2-0-page-model/_static/async1.wmv)

> [!NOTE]
> Uma página assíncrona não é processada para o navegador até que o EndEventHandler seja concluído. Sem dúvida, mas que alguns desenvolvedores considerarão as solicitações assíncronas como sendo semelhantes aos retornos de chamada assíncronos. É importante perceber que eles não são. O benefício para solicitações assíncronas é que o primeiro thread de trabalho pode ser retornado para o pool de threads a fim de atender a novas solicitações, reduzindo assim a contenção devido a uma ligação de e/s, etc.

## <a name="script-callbacks-in-aspnet-20"></a>Retornos de chamada de script no ASP.NET 2,0

Os desenvolvedores da Web sempre procuraram maneiras de evitar a cintilação associada a um retorno de chamada. No ASP.NET 1. x, o SmartNavigation foi o método mais comum para evitar oscilação, mas o SmartNavigation causou problemas para alguns desenvolvedores devido à complexidade de sua implementação no cliente. ASP.NET 2,0 resolve esse problema com retornos de chamada de script. Os retornos de chamada de script utilizam XMLHttp para fazer solicitações no servidor Web via JavaScript. A solicitação XMLHttp retorna dados XML que podem então ser manipulados por meio do DOM do navegador. O código XMLHttp é ocultado do usuário pelo novo manipulador WebResource. axd.

Há várias etapas necessárias para configurar um retorno de chamada de script no ASP.NET 2,0.

## <a name="step-1--implement-the-icallbackeventhandler-interface"></a>Etapa 1: implementar a interface ICallbackEventHandler

Para que o ASP.NET reconheça sua página como participante de um retorno de chamada de script, você deve implementar a interface ICallbackEventHandler. Você pode fazer isso no arquivo code-behind da seguinte forma:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample10.cs)]

Você também pode fazer isso usando a diretiva @ Implements da seguinte forma:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample11.aspx)]

Normalmente, você usaria a diretiva @ Implements ao usar código ASP.NET embutido.

## <a name="step-2--call-getcallbackeventreference"></a>Etapa 2: chamar GetCallbackEventReference

Conforme mencionado anteriormente, a chamada XMLHttp é encapsulada no manipulador WebResource. axd. Quando sua página for renderizada, o ASP.NET adicionará uma chamada ao WebForms\_DoCallBack, um script de cliente que é fornecido pelo WebResource. axd. O webform\_função DoCallBack substitui a função \_\_doPostBack para um retorno de chamada. Lembre-se de que \_\_doPostBack envia programaticamente o formulário na página. Em um cenário de retorno de chamada, você deseja evitar um postback, portanto \_\_doPostBack não será suficiente.

> [!NOTE]
> \_\_doPostBack ainda é renderizado para a página em um cenário de retorno de chamada de script de cliente. No entanto, ele não é usado para o retorno de chamada.

Os argumentos para o webform\_função DoCallBack do lado do cliente são fornecidos por meio da função GetCallbackEventReference do lado do servidor, que normalmente seria chamada na carga de\_de página. Uma chamada típica para GetCallbackEventReference poderia ser assim:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample12.cs)]

> [!NOTE]
> Nesse caso, cm é uma instância de ClientScriptmanager. A classe ClientScriptmanager será abordada posteriormente neste módulo.

Há várias versões sobrecarregadas do GetCallbackEventReference. Nesse caso, os argumentos são os seguintes:

`this`

Uma referência ao controle em que GetCallbackEventReference está sendo chamado. Nesse caso, é a própria página.

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample13.js)]

Um argumento de cadeia de caracteres que será passado do código do lado do cliente para o evento do lado do servidor. Nesse caso, o im passa o valor de uma lista suspensa chamada ddlCompany.

`ShowCompanyName`

O nome da função do lado do cliente que aceitará o valor de retorno (como cadeia de caracteres) do evento de retorno de chamada do lado do servidor. Essa função só será chamada quando o retorno de chamada do lado do servidor for bem-sucedido. Portanto, para fins de robustez, geralmente é recomendável usar a versão sobrecarregada do GetCallbackEventReference que usa um argumento de cadeia de caracteres adicional especificando o nome de uma função do lado do cliente a ser executada no caso de um erro.

`null`

Uma cadeia de caracteres que representa uma função do lado do cliente que foi iniciada antes do retorno de chamada para o servidor. Nesse caso, esse script não existe, portanto, o argumento é nulo.

`true`

Um booliano que especifica se deve ou não conduzir o retorno de chamada de forma assíncrona.

A chamada para WebForms\_DoCallBack no cliente passará esses argumentos. Portanto, quando essa página for renderizada no cliente, esse código terá a seguinte aparência:

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample14.js)]

Observe que a assinatura da função no cliente é um pouco diferente. A função do lado do cliente passa 5 cadeias de caracteres e um booliano. A cadeia de caracteres adicional (que é nula no exemplo acima) contém a função do lado do cliente que tratará quaisquer erros do retorno de chamada do lado do servidor.

## <a name="step-3--hook-the-client-side-control-event"></a>Etapa 3: vincular o evento de controle do lado do cliente

Observe que o valor de retorno de GetCallbackEventReference acima foi atribuído a uma variável de cadeia de caracteres. Essa cadeia de caracteres é usada para vincular um evento do lado do cliente para o controle que inicia o retorno de chamada. Neste exemplo, o retorno de chamada é iniciado por uma lista suspensa na página, portanto, quero vincular o evento *onChange* .

Para conectar o evento do lado do cliente, basta adicionar um manipulador à marcação do lado do cliente da seguinte maneira:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample15.cs)]

Lembre-se de que *cbRef* é o valor de retorno da chamada para GetCallbackEventReference. Ele contém a chamada para WebForms\_DoCallBack que foi mostrado acima.

## <a name="step-4--register-the-client-side-script"></a>Etapa 4: registrar o script do lado do cliente

Lembre-se de que a chamada para GetCallbackEventReference especificou que um script do lado do cliente chamado **Mycompanyname** seria executado quando o retorno de chamada do lado do servidor obtiver sucesso. Esse script precisa ser adicionado à página usando uma instância de ClientScriptmanager. (A classe ClientScriptmanager será discutida posteriormente neste módulo.) Você faz isso da seguinte forma:

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample16.js)]

## <a name="step-5--call-the-methods-of-the-icallbackeventhandler-interface"></a>Etapa 5: chamar os métodos da Interface ICallbackEventHandler

O ICallbackEventHandler contém dois métodos que você precisa implementar em seu código. Eles são **RaiseCallbackEvent** e **GetCallbackEvent**.

**RaiseCallbackEvent** usa uma cadeia de caracteres como um argumento e não retorna nada. O argumento de cadeia de caracteres é passado da chamada do lado do cliente para webform\_DoCallBack. Nesse caso, esse valor é o atributo *Value* da lista suspensa chamada ddlCompany. O código do servidor deve ser colocado no método RaiseCallbackEvent. Por exemplo, se o retorno de chamada estiver fazendo uma WebRequest em relação a um recurso externo, esse código deverá ser colocado em RaiseCallbackEvent.

**GetCallbackEvent** é responsável por processar o retorno do retorno de chamada para o cliente. Ele não usa nenhum argumento e retorna uma cadeia de caracteres. A cadeia de caracteres que ela retorna será passada como um argumento para a função do lado do cliente, neste caso, o complemento *da empresa*.

Depois de concluir as etapas acima, você estará pronto para executar um retorno de chamada de script no ASP.NET 2,0.

![](the-asp-net-2-0-page-model/_static/image4.png)

[Abrir vídeo de tela inteira](the-asp-net-2-0-page-model/_static/callback1.wmv)

Os retornos de chamada de script em ASP.NET têm suporte em qualquer navegador que ofereça suporte a chamadas XMLHttp. Isso inclui todos os navegadores modernos em uso hoje. O Internet Explorer usa o objeto ActiveX XMLHttp enquanto outros navegadores modernos (incluindo o próximo IE 7) usam um objeto XMLHttp intrínseco. Para determinar programaticamente se um navegador dá suporte a retornos de chamada, você pode usar a propriedade **Request. browser. SupportCallback** . Essa propriedade retornará **true** se o cliente solicitante oferecer suporte a retornos de chamada de script.

## <a name="working-with-client-script-in-aspnet-20"></a>Trabalhando com script de cliente no ASP.NET 2,0

Os scripts de cliente no ASP.NET 2,0 são gerenciados por meio do uso da classe ClientScriptmanager. A classe ClientScriptmanager mantém o controle de scripts de cliente usando um tipo e um nome. Isso impede que o mesmo script seja inserido de forma programática em uma página mais de uma vez.

> [!NOTE]
> Depois que um script tiver sido registrado com êxito em uma página, qualquer tentativa subsequente de registrar o mesmo script simplesmente resultará na não registro do script pela segunda vez. Nenhum script duplicado é adicionado e nenhuma exceção ocorre. Para evitar a computação desnecessária, há métodos que você pode usar para determinar se um script já está registrado para que você não tente registrá-lo mais de uma vez.

Os métodos do ClientScriptmanager devem estar familiarizados com todos os desenvolvedores de ASP.NET atuais:

## <a name="registerclientscriptblock"></a>RegisterClientScriptBlock

Esse método adiciona um script à parte superior da página renderizada. Isso é útil para adicionar funções que serão explicitamente chamadas no cliente.

Há duas versões sobrecarregadas desse método. Três de quatro argumentos são comuns entre eles. Eles são:

`type (string)`

O argumento de ***tipo*** identifica um tipo para o script. Geralmente, é uma boa ideia usar o tipo da página (isso. GetType ()) para o tipo.

`key (string)`

O argumento de ***chave*** é uma chave definida pelo usuário para o script. Isso deve ser exclusivo para cada script. Se você tentar adicionar um script com a mesma chave e tipo de um script já adicionado, ele não será adicionado.

`script (string)`

O argumento de ***script*** é uma cadeia de caracteres que contém o script real a ser adicionado. É recomendável que você use um StringBuilder para criar o script e, em seguida, use o método ToString () no StringBuilder para atribuir o argumento ***script*** .

Se você usar o RegisterClientScriptBlock sobrecarregado que usa apenas três argumentos, deverá incluir elementos de script (&lt;script&gt; e &lt;/script&gt;) em seu script.

Você pode optar por usar a sobrecarga de RegisterClientScriptBlock que usa um quarto argumento. O quarto argumento é um booliano que especifica se ASP.NET deve ou não adicionar elementos de script para você. Se esse argumento for **true**, o script não deverá incluir explicitamente os elementos de script.

Use o método IsClientScriptBlockRegistered para determinar se um script já foi registrado. Isso permite que você evite uma tentativa de registrar novamente um script que já foi registrado.

### <a name="registerclientscriptinclude-new-in-20"></a>RegisterClientScriptInclude (novo em 2,0)

A marca RegisterClientScriptInclude cria um bloco de script que vincula a um arquivo de script externo. Ele tem duas sobrecargas. Uma delas usa uma chave e uma URL. O segundo adiciona um terceiro argumento especificando o tipo.

Por exemplo, o código a seguir gera um bloco de script que vincula ao jsfunctions. js na raiz da pasta scripts do aplicativo:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample17.cs)]

Esse código produz o seguinte código na página renderizada:

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample18.html)]

> [!NOTE]
> O bloco de script é renderizado na parte inferior da página.

Use o método IsClientScriptIncludeRegistered para determinar se um script já foi registrado. Isso permite que você evite uma tentativa de registrar novamente um script.

## <a name="registerstartupscript"></a>RegisterStartupScript

O método RegisterStartupScript usa os mesmos argumentos que o método RegisterClientScriptBlock. Um script registrado com o RegisterStartupScript é executado depois que a página é carregada, mas antes do evento OnLoad do lado do cliente. Em 1. X, os scripts registrados com RegisterStartupScript foram colocados antes do fechamento &lt;a marca de&gt; do formulário enquanto os scripts registrados com RegisterClientScriptBlock eram colocados imediatamente após a marca de &lt;&gt; de abertura. No ASP.NET 2,0, ambos são colocados imediatamente antes da marca de&gt; de fechamento &lt;do formulário.

> [!NOTE]
> Se você registrar uma função com RegisterStartupScript, essa função não será executada até que você a chame explicitamente no código do lado do cliente.

Use o método IsStartupScriptRegistered para determinar se um script já foi registrado e evitar uma tentativa de registrar novamente um script.

## <a name="other-clientscriptmanager-methods"></a>Outros métodos ClientScriptmanager

Aqui estão alguns dos outros métodos úteis da classe ClientScriptmanager.

|  <strong>GetCallbackEventReference</strong>   |                                                 Consulte retornos de chamada de script anteriormente neste módulo.                                                 |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|  <strong>GetPostBackClientHyperlink</strong>  |                Obtém uma referência JavaScript (JavaScript:&lt;chamada&gt;) que pode ser usada para postar de volta de um evento do lado do cliente.                 |
|  <strong>GetPostBackEventReference</strong>   |                                   Obtém uma cadeia de caracteres que pode ser usada para iniciar um postback do cliente.                                    |
|      <strong>GetWebResourceUrl</strong>       | Retorna uma URL para um recurso que é inserido em um assembly. Deve ser usado em conjunto com <strong>RegisterClientScriptResource</strong>. |
| <strong>RegisterClientScriptResource</strong> |     Registra um recurso da Web com a página. Esses são recursos inseridos em um assembly e manipulados pelo novo manipulador WebResource. axd.      |
|     <strong>RegisterHiddenField</strong>      |                                                 Registra um campo de formulário oculto com a página.                                                 |
|  <strong>RegisterOnSubmitStatement</strong>   |                                  Registra o código do lado do cliente que é executado quando o formulário HTML é enviado.                                   |
