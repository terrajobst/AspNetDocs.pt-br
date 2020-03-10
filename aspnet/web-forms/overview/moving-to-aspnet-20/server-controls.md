---
uid: web-forms/overview/moving-to-aspnet-20/server-controls
title: Controles de servidor | Microsoft Docs
author: microsoft
description: O ASP.NET 2,0 aprimora os controles de servidor de várias maneiras. Neste módulo, abordaremos algumas das mudanças de arquitetura para a maneira como o ASP.NET 2,0 e o Visual Studio 200...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 43f6ac47-76fc-4cf7-8e9f-c18ce673dfd8
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/server-controls
msc.type: authoredcontent
ms.openlocfilehash: c02a633013f061c09141d4f98871848c011a799e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78641438"
---
# <a name="server-controls"></a>Controles de servidor

pela [Microsoft](https://github.com/microsoft)

> O ASP.NET 2,0 aprimora os controles de servidor de várias maneiras. Neste módulo, abordaremos algumas das mudanças de arquitetura para o modo como o ASP.NET 2,0 e o Visual Studio 2005 lidam com controles de servidor.

O ASP.NET 2,0 aprimora os controles de servidor de várias maneiras. Neste módulo, abordaremos algumas das mudanças de arquitetura para o modo como o ASP.NET 2,0 e o Visual Studio 2005 lidam com controles de servidor.

## <a name="view-state"></a>Estado de exibição

A principal alteração no estado de exibição no ASP.NET 2,0 é uma redução drástica em tamanho. Considere uma página com apenas um controle Calendar. Aqui está o estado de exibição no ASP.NET 1,1.

[!code-css[Main](server-controls/samples/sample1.css)]

Agora, aqui está o estado de exibição em uma página idêntica no ASP.NET 2,0.

[!code-css[Main](server-controls/samples/sample2.css)]

Essa é uma alteração bastante significativa e considerar que o estado de exibição é transportado para frente e para trás pela conexão, essa alteração pode dar aos desenvolvedores um aumento significativo no desempenho. A redução no tamanho do estado de exibição é basicamente devido à maneira como lidamos internamente. Lembre-se que o estado de exibição é uma cadeia de caracteres codificada em base64. Para entender melhor a alteração no estado de exibição no ASP.NET 2,0, vamos dar uma olhada nos valores decodificados dos exemplos acima.

Aqui está o estado de exibição 1,1 decodificado:

[!code-css[Main](server-controls/samples/sample3.css)]

Isso pode parecer um pouco como ininteligível, mas há um padrão aqui. No ASP.NET 1. x, usamos caracteres únicos para identificar tipos de dados e valores delimitados usando o &lt;&gt; caracteres. O "t" no exemplo de estado de exibição acima representa um terceto. O terceto contém um par de ArrayLists (o "l" representa um ArrayList). Um desses ArrayLists contém um Int32 ("i") com um valor de 1 e o outro contém outro terceto. O terceto contém um par de ArrayLists, etc. O importante a ser lembrado é que usamos tercetos que contêm pares, identificamos os tipos de dados por meio de uma letra e usamos o &lt; e &gt; caracteres como delimitadores.

No ASP.NET 2,0, o estado de exibição decodificada parece um pouco diferente.

[!code-powershell[Main](server-controls/samples/sample4.ps1)]

Você deve observar uma enorme alteração na aparência do estado de exibição decodificado. Essa alteração tem várias bases de arquitetura. O estado de exibição no ASP.NET 1. x usou o LosFormatter para serializar dados. Em 2,0, usamos a nova classe ObjectStateFormatter. Essa classe foi projetada especificamente para auxiliar na serialização e desserialização do estado de exibição e do estado de controle. (O estado do controle será abordado na próxima seção.) Há muitos benefícios obtidos pela alteração do método pelo qual ocorre a serialização e a desserialização. Um dos mais significativos é o fato de que, ao contrário do LosFormatter que usa um TextWriter, o ObjectStateFormatter usa um BinaryWriter. Isso permite que o ASP.NET 2,0 armazene o estado de exibição de uma série de bytes em vez de cadeias de caracteres. Veja, por exemplo, um inteiro. No ASP.NET 1,1, um inteiro exigia 4 bytes de estado de exibição. No ASP.NET 2,0, esse mesmo inteiro requer apenas 1 byte. Foram feitas outras melhorias para diminuir a quantidade de estado de exibição que é armazenada. Os valores DateTime, por exemplo, agora são armazenados usando um or em vez de uma cadeia de caracteres.

Como se tudo isso não fosse suficiente, a atenção especial foi paga ao fato de que um dos maiores consumidores do estado de exibição em 1. x era o DataGrid e controles semelhantes. Uma grande desvantagem dos controles como, por exemplo, o DataGrid em que diz respeito ao estado de exibição é que ele geralmente contém grandes quantidades de informações repetidas. No ASP.NET 1. x, essas informações repetidas eram simplesmente armazenadas repetidamente, resultando em um estado de exibição inflado. No ASP.NET 2,0, usamos a nova classe IndexedString para armazenar esses dados. Se uma cadeia de caracteres for repetida, basta armazenar o token para Indexstring e o índice em uma tabela em execução de objetos IndexedString.

## <a name="control-state"></a>Estado de controle

Uma das principais alças que os desenvolvedores tinham com o estado de exibição era o tamanho que ele adicionou à carga de HTTP. Como mencionado anteriormente, um dos maiores consumidores do estado de exibição é o controle DataGrid. Para evitar grandes quantidades de estado de exibição gerados por um DataGrid, muitos desenvolvedores simplesmente desabilitaram o estado de exibição para esse controle. Infelizmente, essa solução nem sempre era uma boa. O estado de exibição no ASP.NET 1. x contém não apenas os dados necessários para a funcionalidade correta do controle. Ele também contém informações relacionadas ao estado da interface do usuário do controle. Isso significa que se você quiser permitir paginação em um DataGrid, deverá habilitar o estado de exibição mesmo que não precise de todas as informações de interface do usuário que o estado de exibição contenha. É um cenário tudo ou nada.

No ASP.NET 2,0, o estado de controle resolve esse problema bem por meio da introdução do estado de controle. O estado de controle contém os dados que são absolutamente necessários para a funcionalidade adequada de um controle. Diferentemente do estado de exibição, o estado de controle não pode ser desabilitado. Portanto, é importante que os dados armazenados no estado de controle sejam cuidadosamente controlados.

> [!NOTE]
> O estado de controle é persistido junto com o estado de exibição no campo de formulário \_\_VIEWSTATE Hidden.

Este vídeo é uma explicação do estado de exibição e do estado de controle.

![](server-controls/_static/image1.png)

[Abrir vídeo de tela inteira](server-controls/_static/state1.wmv)

Para que um controle de servidor Leia e grave no estado de controle, você deve executar três etapas.

## <a name="step-1-call-the-registerrequirescontrolstate-method"></a>Etapa 1: chamar o método RegisterRequiresControlState

O método RegisterRequiresControlState informa ASP.NET que um controle precisa para manter o estado do controle. Ele usa um argumento do tipo Control, que é o controle que está sendo registrado.

É importante observar que o registro não persiste da solicitação para a solicitação. Portanto, esse método deve ser chamado em cada solicitação se um controle for manter o estado do controle. É recomendável que o método seja chamado em OnInit.

[!code-csharp[Main](server-controls/samples/sample5.cs)]

## <a name="step-2-override-savecontrolstate"></a>Etapa 2: substituir SaveControlState

O método SaveControlState salva as alterações de estado de controle de um controle desde o último postback. Ele retorna um objeto que representa o estado do controle.

## <a name="step-3-override-loadcontrolstate"></a>Etapa 3: substituir LoadControlState

O método LoadControlState carrega o estado salvo em um controle. O método usa um argumento do tipo Object que mantém o estado salvo do controle.

## <a name="full-xhtml-compliance"></a>Conformidade total de XHTML

Qualquer desenvolvedor da Web sabe a importância dos padrões em aplicativos Web. Para manter um ambiente de desenvolvimento baseado em padrões, o ASP.NET 2,0 é totalmente compatível com XHTML. Portanto, todas as marcas são renderizadas de acordo com os padrões XHTML em navegadores que dão suporte a HTML 4,0 ou superior.

A definição de DOCTYPE no ASP.NET 1,1 foi a seguinte:

[!code-html[Main](server-controls/samples/sample6.html)]

No ASP.NET 2,0, a definição DOCTYPE padrão é a seguinte:

[!code-html[Main](server-controls/samples/sample7.html)]

Se você escolher, poderá alterar a conformidade padrão do XHTML por meio do nó xhtmlConformance no arquivo de configuração. Por exemplo, o seguinte nó no arquivo Web. config alterará a conformidade XHTML para XHTML 1,0 estrito:

[!code-xml[Main](server-controls/samples/sample8.xml)]

Se você escolher, também poderá configurar o ASP.NET para usar a configuração herdada usada no ASP.NET 1. x da seguinte maneira:

[!code-xml[Main](server-controls/samples/sample9.xml)]

## <a name="adaptive-rendering-using-adapters"></a>Renderização adaptável usando adaptadores

No ASP.NET 1. x, o arquivo de configuração continha um &lt;browserCaps&gt; seção que populava um objeto HttpBrowserCapabilities. Esse objeto permitia que um desenvolvedor determinasse qual dispositivo está fazendo uma solicitação específica e processasse o código adequadamente. No ASP.NET 2,0, o modelo melhorou e agora usa a nova classe ControlAdapter. A classe ControlAdapter substitui eventos no ciclo de vida do controle e controla a renderização de controles com base nos recursos do agente do usuário. Os recursos de um agente de usuário específico são definidos por um arquivo de definição de navegador (um arquivo com uma extensão de arquivo. browser) armazenado no c:\Windows\Microsoft.NET\Framework\v2.0.\*\*\*\*pasta \CONFIG\Browsers

> [!NOTE]
> A classe ControlAdapter é uma classe abstrata.

Assim como a seção &lt;browserCaps&gt; no 1. x, o arquivo de definição de navegador usa uma expressão regular para analisar a cadeia de caracteres do agente do usuário a fim de identificar o navegador solicitante. Ele define recursos específicos para esse agente do usuário. O ControlAdapter processa o controle por meio do método Render. Portanto, se você substituir o método Render, não deverá chamar render na classe base. Isso pode fazer com que a renderização ocorra duas vezes, uma vez para o adaptador e uma vez para o próprio controle.

## <a name="developing-a-custom-adapter"></a>Desenvolvendo um adaptador personalizado

Você pode desenvolver seu próprio adaptador personalizado herdando de ControlAdapter. Além disso, você pode herdar da classe abstrata PageAdapter em casos em que um adaptador é necessário para uma página. O mapeamento de controles para o adaptador personalizado é realizado por meio do elemento &lt;controlAdapters&gt; no arquivo de definição do navegador. Por exemplo, o XML a seguir de um arquivo de definição de navegador mapeia o controle de menu para a classe MenuAdapter:

[!code-html[Main](server-controls/samples/sample10.html)]

Usando esse modelo, torna-se muito fácil para um desenvolvedor de controle direcionar a um determinado dispositivo ou navegador. Também é muito simples para um desenvolvedor ter controle total sobre como as páginas são renderizadas em cada dispositivo.

## <a name="per-device-rendering"></a>Renderização por dispositivo

As propriedades de controle de servidor no ASP.NET 2,0 podem ser especificadas por dispositivo usando um prefixo específico do navegador. Por exemplo, o código a seguir alterará o texto de um rótulo, dependendo de qual dispositivo está sendo usado para navegar na página.

[!code-aspx[Main](server-controls/samples/sample11.aspx)]

Quando a página que contém esse rótulo for navegada pelo Internet Explorer, o rótulo exibirá o texto dizendo "você está navegando no Internet Explorer". Quando a página for navegada no Firefox, o rótulo exibirá o texto "você está navegando no Firefox". Quando a página for navegada de qualquer outro dispositivo, ela exibirá "você está navegando em um dispositivo desconhecido". Qualquer propriedade pode ser especificada usando essa sintaxe especial.

## <a name="setting-focus"></a>Definindo o foco

Os desenvolvedores de ASP.NET 1. x frequentemente perguntaram como definir o foco inicial em um controle específico. Por exemplo, em uma página de logon, é útil fazer com que a caixa de texto ID de usuário obtenha o foco quando a página for carregada pela primeira vez. No ASP.NET 1. x, fazer isso exigiu a gravação de algum script do lado do cliente. Mesmo que tal script seja uma tarefa trivial, ele não é mais necessário no ASP.NET 2,0 graças ao método SetFocus. O método SetFocus usa um argumento que indica o controle que deve receber o foco. Esse argumento pode ser a ID do cliente do controle como uma cadeia de caracteres ou o nome do controle de servidor como um objeto de controle. Por exemplo, para definir o foco inicial para um controle TextBox chamado txtUserID quando a página é carregada pela primeira vez, adicione o seguinte código à página\_carregar:

[!code-csharp[Main](server-controls/samples/sample12.cs)]

--ou

[!code-csharp[Main](server-controls/samples/sample13.cs)]

O ASP.NET 2,0 usa o manipulador WebResource. axd (discutido anteriormente) para renderizar uma função do lado do cliente que define o foco. O nome da função do lado do cliente é o WebForms\_autofocus, como mostrado aqui:

[!code-html[Main](server-controls/samples/sample14.html)]

Como alternativa, você pode usar o método Focus para um controle para definir o foco inicial para esse controle. O método Focus deriva da classe Control e está disponível para todos os controles ASP.NET 2,0. Também é possível definir o foco para um controle específico quando ocorre um erro de validação. Isso será abordado em um módulo posterior.

## <a name="new-server-controls-in-aspnet-20"></a>Novos controles de servidor no ASP.NET 2,0

Veja a seguir novos controles de servidor no ASP.NET 2,0. Entraremos em mais detalhes em alguns deles em módulos posteriores.

## <a name="imagemap-control"></a>Controle ImageMap

O controle ImageMap permite que você adicione hotspots a uma imagem que pode iniciar um postback ou navegar para uma URL. Há três tipos de hotspots disponíveis; CircleHotSpot, RectangleHotSpot e PolygonHotSpot. Os hotspots são adicionados por meio de um editor de coleção no Visual Studio ou programaticamente no código. Não há nenhuma interface do usuário disponível para o desenho de hotspots em uma imagem. As coordenadas e o tamanho ou o raio do ponto de interativo deve ser especificado declarativamente. Também não há nenhuma representação visual de um ponto de interativação no designer. Se um ponto de HotSpot estiver configurado para navegar para uma URL, a URL será especificada por meio da propriedade NavigateUrl do hotspot. No caso de um ponto de acesso de postback, a propriedade PostBackValue permite que você passe uma cadeia de caracteres no postback que pode ser recuperado no código do servidor.

![Editor de coleção HotSpot no Visual Studio](server-controls/_static/image1.jpg)

**Figura 1**: editor de coleção do hotspot no Visual Studio

## <a name="bulletedlist-control"></a>Controle BulletedList

O controle BulletedList é uma lista com marcadores que pode ser facilmente associada a dados. A lista pode ser ordenada (numerada) ou desordenada por meio da propriedade BulletStyle. Cada item na lista é representado por um objeto ListItem.

![Controle BulletedList no Visual Studio](server-controls/_static/image1.gif)

**Figura 2**: controle BulletedList no Visual Studio

## <a name="hiddenfield-control"></a>Controle HiddenField

O controle HiddenField adiciona um campo de formulário oculto à sua página, o valor que está disponível no código do servidor. Geralmente, o valor de um campo de formulário oculto permanece inalterado entre os postbacks. No entanto, é possível que um usuário mal-intencionado altere o valor antes de lançá-lo novamente. Se isso acontecer, o controle HiddenField irá gerar o evento ValueChanged. Se você tiver informações confidenciais no controle HiddenField e quiser garantir que elas permaneçam inalteradas, você deve manipular o evento ValueChanged em seu código.

## <a name="fileupload-control"></a>Controle FileUpload

O controle FileUpload no ASP.NET 2,0 torna possível carregar arquivos em um servidor Web por meio de uma página ASP.NET. Esse controle é bastante semelhante à classe ASP.NET 1. x HtmlInputFile com algumas exceções. No ASP.NET 1. x, era recomendável que a propriedade Postfile seja verificada como nula para determinar se você tinha um bom arquivo. O controle FileUpload no ASP.NET 2,0 adiciona uma nova propriedade HasFile que você pode usar para a mesma finalidade e é um pouco mais eficiente.

A propriedade Postfile ainda está disponível para acesso a um objeto HttpPostedFile, mas algumas das funcionalidades do HttpPostedFile agora estão disponíveis intrinsecamente com o controle FileUpload. Por exemplo, para salvar um arquivo carregado no ASP.NET 1. x, chame o método SaveAs no objeto HttpPostedFile. Usando o controle FileUpload no ASP.NET 2,0, você chamaria o método SaveAs no próprio controle FileUpload.

Outra alteração significativa no comportamento de 2,0 (e provavelmente a alteração mais significativa) é que não é mais necessário carregar um arquivo carregado inteiro na memória antes de salvá-lo. Em 1. x, qualquer arquivo carregado é salvo inteiramente na memória antes de ser gravado no disco. Essa arquitetura impede o upload de arquivos grandes.

No ASP.NET 2,0, o atributo requestLengthDiskThreshold do elemento httpRuntime permite que você configure quantos kilobytes são mantidos em um buffer na memória antes de serem gravados no disco.

**Importante**: a documentação do MSDN (e a documentação em outro lugar) especifica que esse valor está em bytes (não quilobytes) e que o padrão é 256. O valor é realmente especificado em kilobytes e o valor padrão é 80. Com um valor padrão de 80K, garantimos que o buffer não termine na heap de objeto grande.

## <a name="wizard-control"></a>Controle do assistente

É muito comum encontrar os desenvolvedores de ASP.NET com a tentativa de reunir informações em uma série de "páginas" usando painéis ou transferindo de página para página. Com mais frequência do que não, o esforço é frustrante e consome tempo. O novo controle Wizard resolve os problemas, permitindo etapas lineares e não lineares em uma interface de assistente com a qual os usuários estão familiarizados. O controle Wizard apresenta formulários de entrada em uma série de etapas. Cada etapa é de um tipo específico especificado pela propriedade StepType do controle. Os tipos de etapa disponíveis são os seguintes:

| **Tipo de etapa** | **Explicação** |
| --- | --- |
| Auto | O assistente determina automaticamente o tipo de etapa com base na sua posição dentro da hierarquia de etapas. |
| Iniciar | A primeira etapa, geralmente usada para apresentar uma instrução introdutória. |
| Etapa | Uma etapa normal. |
| Concluir | A etapa final, geralmente usada para apresentar um botão para concluir o assistente. |
| Concluído | Apresenta uma mensagem que se comunica com êxito ou falha. |

> [!NOTE]
> O controle Wizard mantém o registro de seu estado usando o estado de controle ASP.NET. Portanto, a propriedade EnableViewState pode ser definida como false sem nenhum prejudicial.

Este vídeo é uma explicação do controle do assistente.

![](server-controls/_static/image2.png)

[Abrir vídeo de tela inteira](server-controls/_static/wizard1.wmv)

## <a name="localize-control"></a>Controle de localização

O controle Localize é semelhante a um controle literal. No entanto, o controle Localize tem uma propriedade **Mode** que controla como a marcação que é adicionada a ela é renderizada. A propriedade Mode oferece suporte aos seguintes valores:

| **Modo** | **Explicação** |
| --- | --- |
| Transformar | A marcação é transformada de acordo com o protocolo do navegador que faz a solicitação. |
| Passagem | A marcação é renderizada como está. |
| Codificar | A marcação que é adicionada ao controle é codificada usando HtmlEncode. |

## <a name="multiview-and-view-controls"></a>Controles MultiView e View

O controle MultiView atua como um contêiner para controles View, e o controle View atua como um contêiner (muito parecido com um controle de painel) para outros controles. Cada exibição em um controle MultiView é representada por um único controle de exibição. O primeiro controle de exibição no MultiView é o modo de exibição 0, o segundo é a exibição 1, etc. Você pode alternar os modos de exibição especificando o ActiveViewIndex do controle MultiView.

## <a name="substitution-control"></a>Controle de substituição

O controle de substituição é usado em conjunto com o cache ASP.NET. Nos casos em que você deseja aproveitar o cache, mas tem partes de uma página que devem ser atualizadas em cada solicitação (em outras palavras, partes de uma página que são isentas do cache), o componente de substituição fornece uma ótima solução. Na verdade, o controle não renderiza nenhuma saída por conta própria. Em vez disso, ele está associado a um método no código do servidor. Quando a página é solicitada, o método é chamado e a marcação retornada é renderizada no lugar do controle de substituição.

O método ao qual o controle de substituição está associado é especificado por meio da propriedade **MethodName** . Esse método deve atender aos seguintes critérios:

- Ele deve ser um método estático (compartilhado no VB).
- Ele aceita um parâmetro do tipo HttpContext.
- Ele retorna uma cadeia de caracteres que representa a marcação que deve substituir o controle na página.

O controle de substituição não tem a capacidade de modificar nenhum outro controle na página, mas tem acesso ao HttpContext atual por meio de seu parâmetro.

## <a name="gridview-control"></a>Controle GridView

O controle GridView é a substituição do controle DataGrid. Esse controle será abordado em mais detalhes em um módulo posterior.

## <a name="detailsview-control"></a>Controle DetailsView

O controle DetailsView permite que você exiba um único registro de uma fonte de dados e edite ou exclua-o. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="formview-control"></a>Controle FormView

O controle FormView é usado para exibir um único registro de uma DataSource em uma interface configurável. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="accessdatasource-control"></a>Controle AccessDataSource

O controle AccessDataSource é usado para associar dados a um banco de dado do Access. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="objectdatasource-control"></a>Controle ObjectDataSource

O controle ObjectDataSource é usado para dar suporte a uma arquitetura de três camadas para que os controles possam ser vinculados a dados a um objeto comercial de camada intermediária, em oposição a um modelo de duas camadas, onde os controles são associados diretamente à fonte de dados. Ele será discutido mais detalhadamente em um módulo posterior.

## <a name="xmldatasource-control"></a>Controle XmlDataSource

O controle XmlDataSource é usado para associar dados a uma fonte de dados XML. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="sitemapdatasource-control"></a>Controle SiteMapDataSource

O controle SiteMapDataSource fornece vinculação de dados para controles de navegação do site com base em um mapa do site. Ele será discutido mais detalhadamente em um módulo posterior.

## <a name="sitemappath-control"></a>Controle SiteMapPath

O controle SiteMapPath exibe uma série de links de navegação comumente chamados de trilhas. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="menu-control"></a>Controle de Menu

O controle menu exibe menus dinâmicos usando DHTML. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="treeview-control"></a>Controle TreeView

O controle TreeView é usado para exibir uma exibição de árvore hierárquica de dados. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="login-control"></a>Controle de logon

O controle de logon fornece um mecanismo para fazer logon em um site da Web. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="loginview-control"></a>Controle LoginView

O controle LoginView permite a exibição de modelos diferentes com base no status de logon de um usuário. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="passwordrecovery-control"></a>Controle PasswordRecovery

O controle PasswordRecovery é usado para recuperar senhas esquecidas por usuários de um aplicativo ASP.NET. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="loginstatus"></a>LoginStatus

O controle LoginStatus exibe o status de logon de um usuário. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="loginname"></a>LoginName

O controle LoginName exibe o nome de usuário de um User depois de ser conectado a um aplicativo ASP.NET. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="createuserwizard"></a>CreateUserWizard

O CreateUserWizard é um assistente configurável que fornece aos usuários a capacidade de criar uma conta de associação do ASP.NET para uso em um aplicativo ASP.NET. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="changepassword"></a>ChangePassword

O controle ChangePassword permite que os usuários alterem sua senha para um aplicativo ASP.NET. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="various-webparts"></a>Várias Web Parts

O ASP.NET 2,0 é fornecido com vários Web Parts. Eles serão abordados em detalhes em um módulo posterior.
