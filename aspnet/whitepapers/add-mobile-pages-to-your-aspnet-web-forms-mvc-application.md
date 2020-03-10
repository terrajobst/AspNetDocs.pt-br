---
uid: whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
title: 'Como: adicionar páginas móveis ao seu aplicativo ASP.NET Web Forms/MVC | Microsoft Docs'
author: rick-anderson
description: Este "como" descreve várias maneiras de servir páginas otimizadas para dispositivos móveis do seu aplicativo ASP.NET Web Forms/MVC e sugere arquitetura e...
ms.author: riande
ms.date: 01/20/2011
ms.assetid: 3124f28e-cc32-418a-afe3-519fa56f4c36
msc.legacyurl: /whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
msc.type: content
ms.openlocfilehash: 63c555358d06a9506bb5c8c993800c3307108192
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78572733"
---
# <a name="how-to-add-mobile-pages-to-your-aspnet-web-forms--mvc-application"></a>Como: adicionar páginas móveis ao seu aplicativo ASP.NET Web Forms/MVC

> **Aplica-se a**
> 
> - ASP.NET Web Forms versão 4,0
> - ASP.NET MVC versão 3,0
> 
> **Resumo**
> 
> Esse manual descreve várias maneiras para servir páginas otimizadas para dispositivos móveis do seu Web Forms do ASP.NET / aplicativo MVC e sugere problemas de design e arquitetônicos a serem considerados ao direcionar uma ampla gama de dispositivos. Este documento também explica por que os controles móveis ASP.NET do ASP.NET 2,0 para 3,5 agora são obsoletos e discute algumas alternativas modernas.

## <a name="contents"></a>Conteúdo

- Visão geral
- Opções de arquitetura
- Detecção de navegador e dispositivo
- Como os aplicativos ASP.NET Web Forms podem apresentar páginas específicas para dispositivos móveis
- Como os aplicativos MVC ASP.NET podem apresentar páginas específicas para dispositivos móveis
- Recursos adicionais

Para obter exemplos de código para download que demonstram as técnicas deste white paper para ASP.NET Web Forms e MVC, consulte [aplicativos móveis & sites com o ASP.net](https://docs.microsoft.com/aspnet/mobile/overview).

## <a name="overview"></a>Visão geral

Dispositivos móveis – smartphones, telefones de recursos e tablets – continuam crescendo em popularidade como um meio de acessar a Web. Para muitos desenvolvedores da Web e empresas orientadas na Web, isso significa que é cada vez mais importante fornecer uma ótima experiência de navegação para os visitantes que usam esses dispositivos.

### <a name="how-earlier-versions-of-aspnet-supported-mobile-browsers"></a>Como as versões anteriores do ASP.NET oferecem suporte a navegadores móveis

ASP.NET versões 2,0 a 3,5 *ASP.NET controles móveis*incluídos: um conjunto de controles de servidor para dispositivos móveis no assembly *System. Web. Mobile. dll* e no namespace *System. Web. UI. MobileControls* . O assembly ainda está incluído no ASP.NET 4, mas ele foi preterido. Os desenvolvedores são aconselhados a migrar para abordagens mais modernas, como as descritas neste documento.

O motivo pelo qual os controles móveis do ASP.NET foram marcados como obsoletos é que seu design é orientado em relação aos telefones celulares que eram comuns em relação ao 2005 e anterior. Os controles são projetados principalmente para processar a marcação WML ou cHTML (em vez de HTML normal) para os navegadores WAP dessa era. Mas WAP, WML e cHTML não são mais relevantes para a maioria dos projetos, porque o HTML agora se tornou a linguagem de marcação onipresente para navegadores móveis e de desktops.

### <a name="the-challenges-of-supporting-mobile-devices-today"></a>Os desafios de dar suporte a dispositivos móveis hoje

Embora os navegadores móveis agora ofereçam suporte a HTML quase universalmente, você ainda enfrentará muitos desafios ao visar criar excelentes experiências de navegação móvel:

- ***Tamanho da tela*** -os dispositivos móveis variam drasticamente no formulário e suas telas geralmente são muito menores do que os monitores da área de trabalho. Portanto, talvez seja necessário criar layouts de página completamente diferentes para eles.
- ***Métodos de entrada*** – alguns dispositivos têm teclados, alguns têm styluses, outros usam toque. Talvez seja necessário considerar vários mecanismos de navegação e métodos de entrada de dados.
- ***Conformidade*** com os padrões – muitos navegadores móveis não dão suporte aos padrões mais recentes de HTML, CSS ou JavaScript.
- ***Largura de banda*** – o desempenho da rede de dados de celular varia e alguns usuários finais estão em tarifas que cobram por megabyte.

Não há nenhuma solução de tamanho único; seu aplicativo precisará parecer e se comportar de forma diferente, de acordo com o dispositivo que o acessa. Dependendo do nível de suporte móvel que você deseja, isso pode ser um desafio maior para desenvolvedores da Web do que a área de trabalho "conflitos de navegadores" já foi.

Os desenvolvedores que estão se aproximando do suporte ao navegador móvel pela primeira vez costumam inicialmente considerar que é apenas importante dar suporte aos smartphones mais recentes e mais sofisticados (por exemplo, Windows Phone 7, iPhone ou Android), talvez porque os desenvolvedores geralmente pertencem a eles pseudodispositivos. No entanto, os telefones mais baratos ainda são extremamente populares e seus proprietários os usam para navegar na Web – especialmente em países em que os celulares são mais fáceis de obter do que uma conexão de banda larga. Sua empresa precisará decidir qual intervalo de dispositivos oferecer suporte Considerando seus clientes prováveis. Se você estiver criando um folheto online para um spa de saúde de luxo, poderá tomar uma decisão de negócios apenas para smartphones avançados, enquanto se estiver criando um sistema de reservas de ingressos para um cinema, provavelmente precisará considerar os visitantes com recursos menos poderosos telefones.

## <a name="architectural-options"></a>Opções de arquitetura

Antes de chegarmos aos detalhes técnicos específicos do ASP.NET Web Forms ou MVC, observe que os desenvolvedores da Web em geral têm três opções principais possíveis para dar suporte a navegadores móveis:

1. ***Não fazer nada –*** Você pode simplesmente criar um aplicativo Web padrão e orientado para desktop e contar com navegadores móveis para renderizá-los de forma aceitável. 

    - **Vantagem**: é a opção mais barata para implementar e manter – sem trabalho extra
    - **Desvantagem**: oferece a pior experiência do usuário final: 

        - Os smartphones mais recentes podem renderizar seu HTML tão bem quanto um navegador de desktop, mas os usuários ainda serão forçados a ampliar e rolar horizontalmente e verticalmente para consumir o conteúdo em uma tela pequena. Isso está longe de ser o ideal.
        - Dispositivos mais antigos e telefones de recursos podem falhar ao renderizar sua marcação de maneira satisfatória.
        - Mesmo nos dispositivos Tablet mais recentes (cujas telas podem ser tão grandes quanto as telas de laptop), diferentes regras de interação se aplicam. A entrada baseada em toque funciona melhor com botões e links maiores, e não há como focalizar um cursor do mouse sobre um menu suspenso.
2. ***Resolva o problema no cliente* –** com uso cuidadoso de CSS e [aprimoramento progressivo](http://en.wikipedia.org/wiki/Progressive_enhancement) , você pode criar marcação, estilos e scripts que se adaptem a qualquer navegador que esteja executando-os. Por exemplo, com [consultas de mídia do CSS 3](http://www.w3.org/TR/2010/CR-css3-mediaqueries-20100727/), você pode criar um layout de várias colunas que se transforma em um único layout de coluna em dispositivos cujas telas são mais estreitas do que um limite escolhido. 

    - **Vantagem**: otimiza a renderização para o dispositivo específico em uso, mesmo para dispositivos futuros desconhecidos de acordo com qualquer tela e características de entrada que eles têm
    - **Vantagem**: permite que você compartilhe a lógica do lado do servidor em todos os tipos de dispositivo – a duplicação mínima de código ou esforço
    - **Desvantagem**: os dispositivos móveis são tão diferentes dos dispositivos de área de trabalho que você pode querer que suas páginas móveis sejam completamente diferentes das páginas da área de trabalho, mostrando informações diferentes. Essas variações podem ser ineficientes ou impossíveis de serem obtidas de forma robusta por meio de CSS, especialmente considerando como dispositivos inconsistentes interpretam as regras de CSS. Isso é particularmente verdadeiro para atributos CSS 3.
    - **Desvantagem**: o não oferece suporte a diferentes lógicas e fluxos de trabalho do lado do servidor para diversos dispositivos. Você não pode, por exemplo, implementar um fluxo de trabalho de retirada do carrinho de compras simplificado para usuários móveis por meio do CSS sozinho.
    - **Desvantagem**: uso ineficiente da largura de banda. O servidor pode precisar transmitir a marcação e os estilos que se aplicam a todos os dispositivos possíveis, mesmo que o dispositivo de destino use apenas um subconjunto dessas informações.
3. ***Resolver o problema no servidor* –** Se o seu servidor sabe qual dispositivo está acessando-o – ou pelo menos as características desse dispositivo, como o tamanho da tela e o método de entrada, e se é um dispositivo móvel, ele pode executar uma lógica diferente e gerar uma marcação HTML diferente. 

    - **Vantagem:** Flexibilidade máxima. Não há limite para o quanto você pode variar a lógica do lado do servidor para dispositivos móveis ou otimizar sua marcação para o layout específico do dispositivo desejado.
    - **Vantagem:** Uso eficiente da largura de banda. Você só precisa transmitir as informações de marcação e estilo que o dispositivo de destino vai usar.
    - **Desvantagem:** Às vezes, força a repetição de esforço ou código (por exemplo, fazer com que você crie cópias semelhantes, mas ligeiramente diferentes, de suas páginas de Web Forms ou exibições do MVC). Sempre que possível, você criará uma lógica comum em uma camada ou serviço subjacente, mas ainda assim, algumas partes do código da interface do usuário ou da marcação talvez precisem ser duplicadas e mantidas em paralelo.
    - **Desvantagem:** A detecção de dispositivo não é trivial. Ele requer uma lista ou banco de dados de tipos de dispositivos conhecidos e suas características (que talvez nem sempre estejam perfeitamente atualizadas) e não é garantido que correspondam com precisão a cada solicitação de entrada. Este documento descreve algumas opções e suas armadilhas mais tarde.

Para obter os melhores resultados, a maioria dos desenvolvedores descobre que eles precisam combinar opções (2) e (3). Pequenas diferenças estilísticos são mais bem acomodadas no cliente usando CSS ou até mesmo JavaScript, enquanto as principais diferenças nos dados, no fluxo de trabalho ou na marcação são implementadas com mais eficiência no código do servidor.

### <a name="this-paper-focuses-on-server-side-techniques"></a>Este documento se concentra nas técnicas do lado do servidor

Como o ASP.NET Web Forms e o MVC são essencialmente tecnologias do lado do servidor, esse white paper se concentrará em técnicas do lado do servidor que permitem produzir diferentes marcações e lógica para navegadores móveis. É claro que você também pode combinar isso com qualquer técnica do lado do cliente (por exemplo, consultas de mídia do CSS 3, JavaScript de aprimoramento progressivo), mas essa é uma questão de design da Web do que o desenvolvimento de ASP.NET.

## <a name="browser-and-device-detection"></a>Detecção de navegador e dispositivo

O principal pré-requisito para todas as técnicas do lado do servidor para dar suporte a dispositivos móveis é saber qual dispositivo seu visitante está usando. Na verdade, ainda melhor do que saber o fabricante e o número do modelo desse dispositivo é conhecer as *características* do dispositivo. As características podem incluir:

- É um dispositivo móvel?
- Método de entrada (mouse/teclado, toque, teclado, joystick,...)
- Tamanho da tela (fisicamente e em pixels)
- Formatos de dados e mídia com suporte
- Etc.

É melhor tomar decisões com base em características do que o número do modelo, pois você estará mais bem equipado para lidar com futuros dispositivos.

### <a name="using-aspnets-built-in-browser-detection-support"></a>Usando ASP. Suporte de detecção de navegador interno da rede

Os desenvolvedores do ASP.NET Web Forms e MVC podem descobrir imediatamente características importantes de um navegador da visita inspecionando as propriedades do objeto *Request. browser* . Por exemplo, consulte

- Request.Browser.IsMobileDevice
- Request.Browser.MobileDeviceManufacturer, Request.Browser.MobileDeviceModel
- Request.Browser.ScreenPixelsWidth
- Request.Browser.SupportsXmlHttp
- ... e muitos outros

Nos bastidores, a plataforma ASP.NET corresponde ao cabeçalho HTTP de UA ( *agente de usuário* ) de entrada em relação a expressões regulares em um conjunto de arquivos XML de definição de navegador. Por padrão, a plataforma inclui definições para muitos dispositivos móveis comuns, e você pode adicionar arquivos de definição de navegador personalizados para outras pessoas que você deseja reconhecer. Para obter mais detalhes, consulte a página do MSDN [ASP.NET controles de servidor Web e recursos do navegador](https://msdn.microsoft.com/library/x3k2ssx2.aspx).

### <a name="using-the-wurfl-device-database-via-51degreesmobi-foundation"></a>Usando o banco de dados do dispositivo WURFL por meio do 51Degrees.mobi Foundation

Enquanto o ASP. O suporte interno à detecção de navegadores da rede será suficiente para muitos aplicativos, há dois casos principais em que ele pode não ser suficiente:

- ***Você deseja reconhecer os dispositivos mais recentes***(sem criar manualmente os arquivos de definição do navegador para eles). Observe que os arquivos de definição de navegador do .NET 4 não são recentes o suficiente para reconhecer Windows Phone 7, telefones Android, navegadores móveis operativos ou iPads da Apple.
- ***Você precisa de informações mais detalhadas sobre os recursos do dispositivo***. Talvez você precise saber sobre o método de entrada de um dispositivo (por exemplo, Touch vs teclado) ou quais formatos de áudio o navegador dá suporte. Essas informações não estão disponíveis nos arquivos de definição padrão do navegador.

O [projeto WURFL ( *Wireless Universal Resource File* )](http://wurfl.sourceforge.net/) mantém informações mais atualizadas e detalhadas sobre os dispositivos móveis em uso hoje.

A ótima notícia para os desenvolvedores de .NET é que o ASP. O recurso de detecção de navegador da rede é extensível, portanto, é possível aprimorá-lo para superar esses problemas. Por exemplo, você pode adicionar a biblioteca do Open Source [*51Degrees.mobi Foundation*](https://github.com/51Degrees/dotNET-Device-Detection) ao seu projeto. É um ASP.NET IHttpModule ou provedor de recursos de navegador (utilizável em aplicativos Web Forms e MVC), que lê diretamente os dados WURFL e os conecta ao ASP. Mecanismo de detecção de navegador interno da rede. Depois de instalar o módulo, a *solicitação. browser* terá, de repente, uma informação mais precisa e detalhada: Ele reconhecerá corretamente muitos dos dispositivos mencionados anteriormente e listaria seus recursos (incluindo recursos adicionais, como o método de entrada). Consulte a documentação do projeto para obter mais detalhes.

## <a name="how-web-forms-applications-can-present-mobile-specific-pages"></a>Como Web Forms aplicativos podem apresentar páginas específicas para dispositivos móveis

Por padrão, veja como uma nova Web Forms aplicativo é exibida em dispositivos móveis comuns:

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image1.png)

Claramente, nenhum layout parece muito móvel – esta página foi projetada para um monitor grande, orientado a paisagem, não para uma pequena tela orientada a retrato. Então, o que você pode fazer a respeito?

Como discutido anteriormente neste documento, há várias maneiras de personalizar suas páginas para dispositivos móveis. Algumas técnicas são baseadas em servidor, outras executadas no cliente.

### <a name="creating-a-mobile-specific-master-page"></a>Criando uma página mestra específica para dispositivos móveis

Dependendo dos seus requisitos, você poderá usar o mesmo Web Forms para todos os visitantes, mas ter duas páginas mestras separadas: uma para os visitantes da área de trabalho, outra para os visitantes móveis. Isso proporciona a flexibilidade de alterar a folha de estilos CSS ou sua marcação HTML de nível superior para atender a dispositivos móveis, sem forçá-lo a duplicar qualquer lógica de página.

Isso é fácil de fazer. Por exemplo, você pode adicionar um manipulador PreInit como o seguinte a um formulário da Web:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample1.cs)]

Agora, crie uma página mestra chamada Mobile. Master na pasta de nível superior do seu aplicativo e ela será usada quando um dispositivo móvel for detectado. Sua página mestra móvel pode fazer referência a uma folha de estilos CSS específica para dispositivos móveis, se necessário. Os visitantes da área de trabalho ainda verão sua página mestra padrão, não a móvel.

### <a name="creating-independent-mobile-specific-web-forms"></a>Criando Web Forms específicas para dispositivos móveis independentes

Para obter a máxima flexibilidade, você pode ir muito além do que apenas ter páginas mestras separadas para diferentes tipos de dispositivo. Você pode implementar dois *conjuntos totalmente separados de páginas Web Forms* – um conjunto para navegadores da área de trabalho, outro conjunto para dispositivos móveis. Isso funciona melhor se você quiser apresentar informações ou fluxos de trabalho muito diferentes para visitantes móveis. O restante desta seção descreve detalhadamente essa abordagem.

Supondo que você já tenha um aplicativo Web Forms projetado para navegadores de desktop, a maneira mais fácil de continuar é criar uma subpasta chamada "Mobile" em seu projeto e criar suas páginas móveis. Você pode construir um subsite inteiro, com suas próprias páginas mestras, folhas de estilo e páginas, usando todas as mesmas técnicas que usaria para qualquer outro aplicativo Web Forms. Você não precisa necessariamente produzir um equivalente móvel para *cada* página no seu site de área de trabalho; Você pode escolher qual subconjunto de funcionalidade faz sentido para os visitantes do celular.

Suas páginas móveis podem compartilhar recursos estáticos comuns (como imagens, JavaScript ou arquivos CSS) com suas páginas normais, se desejar. Como sua pasta "móvel" *não* será marcada como um aplicativo separado quando hospedada no IIS (é apenas uma subpasta simples de Web Forms páginas), ela também compartilhará a mesma configuração, os dados da sessão e outras infraestruturas como suas páginas da área de trabalho.

> [!NOTE]
> Como essa abordagem geralmente envolve uma duplicação de código (as páginas móveis provavelmente compartilharão algumas semelhanças com as páginas da área de trabalho), é importante considerar qualquer lógica comercial comum ou código de acesso a dados em uma camada subjacente compartilhada ou serviço. Caso contrário, você dobrará o esforço de criar e manter seu aplicativo.

#### <a name="redirecting-mobile-visitors-to-your-mobile-pages"></a>Redirecionando visitantes móveis para suas páginas móveis

Muitas vezes é conveniente redirecionar os visitantes móveis para as páginas móveis somente na *primeira* solicitação em sua sessão de navegação (e não em cada solicitação em sua sessão), porque:

- Você pode facilmente permitir que os visitantes móveis acessem suas páginas da área de trabalho se desejarem – basta colocar um link na página mestra que vai para "versão da área de trabalho". O visitante não será Redirecionado de volta para uma página móvel porque não é mais a primeira solicitação em sua sessão.
- Ele evita o risco de interferir com as solicitações de todos os recursos dinâmicos compartilhados entre a área de trabalho e as partes móveis do seu site (por exemplo, se você tiver um formulário da Web comum que as partes desktop e móvel do seu site sejam exibidas em um IFRAME ou em determinados manipuladores Ajax)

Para fazer isso, você pode posicionar a lógica de redirecionamento em uma **sessão\_método iniciar** . Por exemplo, adicione o seguinte método ao seu arquivo Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample2.cs)]

#### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Configurando a autenticação de formulários para respeitar suas páginas móveis

Observe que a autenticação de formulários faz determinadas suposições sobre onde ele pode redirecionar os visitantes durante e após o processo de autenticação:

- Quando um usuário precisar ser autenticado, a autenticação de formulários os redirecionará para a página de logon da área de trabalho, independentemente de ser um usuário de desktop ou móvel (porque ele tem apenas um conceito de *uma* URL de logon). Supondo que você queira estilizar sua página de logon móvel de forma diferente, você precisa aprimorar sua página de logon de área de trabalho para que ela redirecione usuários móveis para uma página de logon móvel separada. Por exemplo, adicione o seguinte código à sua página de logon de **área de trabalho** code-behind: 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample3.cs)]
- Depois que um usuário faz logon com êxito, a autenticação de formulários, por padrão, redirecionará-os para seu home page de área de trabalho (porque ele tem apenas um conceito de *uma* página padrão). Você precisa aprimorar sua página de logon móvel para que ela Redirecione para o home page móvel após um logon bem-sucedido. Por exemplo, adicione o seguinte código à sua página de logon **móvel** code-behind: 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample4.cs)]
  
  Esse código pressupõe que sua página tenha um controle de servidor de logon chamado LoginUser, como no modelo de projeto padrão.

### <a name="working-with-output-caching"></a>Trabalhando com cache de saída

Se você estiver usando o cache de saída, lembre-se de que, por padrão, é possível que um usuário de desktop visite uma determinada URL (fazendo com que sua saída seja armazenada em cache), seguida por um usuário móvel que recebe a saída de área de trabalho armazenada em cache. Esse aviso se aplica se você estiver apenas variando sua página mestra por tipo de dispositivo ou implementando Web Forms totalmente separados por tipo de dispositivo.

Para evitar o problema, você pode instruir o ASP.NET a variar a entrada de cache de acordo com o fato de o visitante estar usando um dispositivo móvel. Adicione um parâmetro VaryByCustom à declaração OutputCache da sua página da seguinte maneira:

[!code-aspx[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample5.aspx)]

Em seguida, defina *IsMobileDevice* como um parâmetro de cache personalizado adicionando a seguinte substituição de método ao seu arquivo global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample6.cs)]

Isso garantirá que os visitantes móveis da página não recebam a saída colocada anteriormente no cache por um visitante da área de trabalho.

### <a name="a-working-example"></a>Um exemplo de trabalho

Para ver essas técnicas em ação, baixe os [exemplos de código deste White Paper](https://docs.microsoft.com/aspnet/mobile/overview). O aplicativo de exemplo Web Forms redireciona automaticamente os usuários móveis para um conjunto de páginas específicas para dispositivos móveis em uma subpasta chamada móvel. A marcação e o estilo dessas páginas são mais otimizados para navegadores móveis, como você pode ver nas capturas de tela a seguir:

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image2.png)

Para obter mais dicas sobre como otimizar sua marcação e CSS para navegadores móveis, consulte a seção "estilizando páginas móveis para navegadores móveis", mais adiante neste documento.

## <a name="how-aspnet-mvc-applications-can-present-mobile-specific-pages"></a>Como os aplicativos MVC ASP.NET podem apresentar páginas específicas para dispositivos móveis

Como o padrão Model-View-Controller separa a lógica do aplicativo (em controladores) da lógica de apresentação (em exibições), você pode escolher entre qualquer uma das seguintes abordagens para lidar com o suporte móvel no código do lado do servidor:

1. ***usar os mesmos controladores e exibições para navegadores de área de trabalho e móveis, mas renderizar as exibições com diferentes layouts do Razor dependendo do tipo de dispositivo *.** Essa opção funcionará melhor se você estiver exibindo dados idênticos em todos os dispositivos, mas simplesmente deseja fornecer folhas de estilo CSS diferentes ou alterar alguns elementos HTML de nível superior para dispositivos móveis.
2. ***Use os mesmos controladores para navegadores de área de trabalho e móveis, mas processe exibições diferentes dependendo do tipo de dispositivo***. Essa opção funcionará melhor se você estiver exibindo aproximadamente os mesmos dados e fornecendo os mesmos fluxos de trabalho para os usuários finais, mas deseja renderizar uma marcação HTML muito diferente para se adequar ao dispositivo que está sendo usado.
3. ***criar áreas separadas para desktops e navegadores móveis, implementando controladores independentes e exibições para cada *.** Essa opção funcionará melhor se você estiver exibindo telas muito diferentes, contendo informações diferentes e levando o usuário por meio de fluxos de trabalho diferentes, otimizados para o tipo de dispositivo. Isso pode significar uma repetição de código, mas você pode minimizar isso ao fatorar a lógica comum em uma camada ou serviço subjacente.

Se você quiser usar a **primeira** opção e variar apenas o layout do Razor por tipo de dispositivo, será muito fácil. Basta modificar o arquivo \_ViewStart. cshtml da seguinte maneira:

[!code-cshtml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample7.cshtml)]

Agora você pode criar um layout específico ao celular chamado \_LayoutMobile. cshtml com uma estrutura de página e as regras de CSS otimizadas para dispositivos móveis.

Se você quiser usar a **segunda** opção para renderizar exibições totalmente diferentes de acordo com o tipo de dispositivo do visitante, consulte a [postagem do blog de Scott Hanselman](http://www.hanselman.com/blog/ABetterASPNETMVCMobileDeviceCapabilitiesViewEngine.aspx).

O restante deste documento se concentra na **terceira** opção – criando controladores *e* exibições separados para dispositivos móveis, para que você possa controlar exatamente qual subconjunto de funcionalidade é oferecido para os visitantes móveis.

### <a name="setting-up-a-mobile-area-within-your-aspnet-mvc-application"></a>Configurando uma área móvel em seu aplicativo MVC ASP.NET

Você pode adicionar uma área chamada "móvel" a um aplicativo MVC ASP.NET existente da maneira normal: clique com o botão direito do mouse no nome do projeto em Gerenciador de Soluções e escolha Adicionar à área. Em seguida, você pode adicionar controladores e exibições como faria em qualquer outra área dentro de um aplicativo MVC ASP.NET. Por exemplo, adicione à sua área móvel um novo controlador chamado HomeController para atuar como uma home page para visitantes móveis.

### <a name="ensuring-the-url-mobile-reaches-the-mobile-homepage"></a>Garantindo que a URL/chegue à Home page móvel

Se você quiser que a URL/alcance a ação de índice no HomeController dentro de sua área móvel, será necessário fazer duas pequenas alterações na sua configuração de roteamento. Primeiro, atualize sua classe MobileAreaRegistration para que HomeController seja o controlador padrão em sua área móvel, conforme mostrado no código a seguir:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample8.cs)]

Isso significa que a home page móvel agora estará localizada em/, em vez de/Mobile/Home, porque "Home" agora é o nome do controlador implicitamente padrão para páginas móveis.

Em seguida, observe que, ao adicionar um segundo HomeController ao seu aplicativo (ou seja, o dispositivo móvel, além da área de trabalho existente), você terá quebrado a Home Page da área de trabalho normal. Ele falhará com o erro "*foram encontrados vários tipos que correspondem ao controlador chamado ' Home '* ". Para resolver isso, atualize sua configuração de roteamento de nível superior (em Global.asax.cs) para especificar que a sua área de trabalho HomeController deve ter prioridade quando houver ambiguidade:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample9.cs)]

Agora, o erro será desaparecedo e a URL http:\/\/*seusite*/chegará à Home Page da área de trabalho e http:\/\/*seusite*/Mobile/chegará à Home page móvel.

### <a name="redirecting-mobile-visitors-to-your-mobile-area"></a>Redirecionando visitantes móveis para sua área móvel

Há muitos pontos de extensibilidade diferentes no ASP.NET MVC, portanto, há muitas maneiras possíveis de injetar a lógica de redirecionamento. Uma opção mais clara é criar um atributo de filtro, [RedirectMobileDevicesToMobileArea], que executa um redirecionamento se as seguintes condições forem atendidas:

1. É a primeira solicitação na sessão do usuário (ou seja, Session. IsNewSession é igual a true)
2. A solicitação vem de um navegador móvel (ou seja, Request. browser. IsMobileDevice é igual a true)
3. O usuário ainda não está solicitando um recurso na área móvel (ou seja, a parte do *caminho* da URL não começa com/)

O exemplo baixável incluído com este white paper inclui uma implementação dessa lógica. Ele é implementado como um filtro de autorização, derivado de Authorattribute, o que significa que ele pode funcionar corretamente mesmo se você estiver usando o cache de saída (caso contrário, se um visitante do desktop primeiro acessar uma determinada URL, a saída da área de trabalho poderá ser armazenada em cache e, em seguida, servida para visitantes móveis subsequentes).

Como é um filtro, você pode optar por aplicá-lo a controladores e ações específicos, por exemplo,

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample10.cs)]

… ou você pode aplicá-lo a todos os controladores e ações como um *filtro global* MVC 3 em seu arquivo global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample11.cs)]

O exemplo para download também demonstra como você pode criar subclasses desse atributo que redirecionem para locais específicos dentro de sua área móvel. Isso significa, por exemplo, que você pode:

- Registre um filtro global, conforme mostrado acima, que envia os visitantes móveis para a home page móvel por padrão.
- Também aplique um filtro especial [RedirectMobileDevicesToMobileProductPage] a uma ação de "Exibir produto" que leva os visitantes móveis à versão móvel de qualquer página de produto solicitada.
- Também aplique outras subclasses especiais do filtro a ações específicas, redirecionando os visitantes móveis para a página móvel equivalente

### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Configurando a autenticação de formulários para respeitar suas páginas móveis

Se você estiver usando a autenticação de formulários, deverá observar que, quando um usuário precisar fazer logon, ele redireciona automaticamente o usuário para uma única URL específica de "logon", que por padrão é **/Account/logon**. Isso significa que os usuários móveis podem ser redirecionados para a ação de "logon" da área de trabalho.

Para evitar esse problema, adicione lógica à ação de "logon" da área de trabalho para que ela Redirecione os usuários móveis novamente para uma ação de "logon" móvel. Se você estiver usando o modelo de aplicativo padrão do ASP.NET MVC, atualize a ação de LogOn do AccountController da seguinte maneira:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample12.cs)]

… e, em seguida, implementar uma ação adequada de "logon" para dispositivos móveis em um controlador chamado AccountController em sua área móvel.

### <a name="working-with-output-caching"></a>Trabalhando com cache de saída

Se você estiver usando o filtro [OutputCache], deverá forçar a entrada de cache a variar por tipo de dispositivo. Por exemplo, escreva:

[!code-javascript[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample13.js)]

Em seguida, adicione o seguinte método ao seu arquivo Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample14.cs)]

Isso garantirá que os visitantes móveis da página não recebam a saída colocada anteriormente no cache por um visitante da área de trabalho.

### <a name="a-working-example"></a>Um exemplo de trabalho

Para ver essas técnicas em ação, baixe os [exemplos associados ao código do White Paper](https://docs.microsoft.com/aspnet/mobile/overview). O exemplo inclui um aplicativo ASP.NET MVC 3 (versão Release Candidate) aprimorado para dar suporte a dispositivos móveis usando os métodos descritos acima.

## <a name="further-guidance-and-suggestions"></a>Mais orientações e sugestões

A discussão a seguir se aplica aos desenvolvedores Web Forms e MVC que estão usando as técnicas abordadas neste documento.

### <a name="enhancing-your-redirection-logic-using-51degreesmobi-foundation"></a>Aprimorando a lógica de redirecionamento usando o 51Degrees.mobi Foundation

A lógica de redirecionamento mostrada neste documento pode ser perfeitamente suficiente para seu aplicativo, mas não funcionará se você precisar desabilitar sessões ou com navegadores móveis que rejeitem cookies (não podem ter sessões), porque não saberá se uma determinada solicitação é o primeiro daquele visitante.

Você já aprendeu como o Open Source 51Degrees.mobi Foundation pode melhorar a precisão do ASP. Detecção de navegador da rede. Ele também tem uma capacidade interna de redirecionar visitantes móveis para locais específicos configurados em Web. config. Ele é capaz de trabalhar sem depender de sessões de ASP.NET (e, portanto, cookies) armazenando um log temporário de hashes de cabeçalhos HTTP dos visitantes e endereços IP, para que ele saiba se cada solicitação é ou não a primeira de um determinado solicitante.

O elemento a seguir adicionado à seção fiftyOne do arquivo Web. config redirecionará a primeira solicitação de um dispositivo móvel detectado para a página ~/Mobile/Default.aspx. Qualquer solicitação para páginas sob a pasta móvel *não* será redirecionada, independentemente do tipo de dispositivo. Se o dispositivo móvel ficar inativo por um período de 20 minutos ou se o dispositivo for esquecido e as solicitações subsequentes serão tratadas como novas para fins de redirecionamento.

[!code-xml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample15.xml)]

Para obter mais detalhes, consulte a [documentação do 51Degrees.mobi Foundation](https://github.com/51Degrees/dotNET-Device-Detection).

> [!NOTE]
> Você *pode* usar o recurso de redirecionamento do 51Degrees.mobi Foundation em aplicativos ASP.NET MVC, mas será necessário definir sua configuração de redirecionamento em termos de URLs simples, não em termos de parâmetros de roteamento ou colocando filtros MVC em ações. Isso ocorre porque (no momento da escrita) o 51Degrees.mobi Foundation não reconhece filtros ou roteamento.

### <a name="disabling-transcoders-and-proxy-servers"></a>Desabilitando transcodificadores e servidores proxy

Os operadores de rede móvel têm dois objetivos amplos em sua abordagem para a Internet móvel:

1. Fornecer o máximo possível de conteúdo relevante
2. Maximize o número de clientes que podem compartilhar a largura de banda limitada da rede de rádio

Como a maioria das páginas da Web foi projetada para grandes telas de tamanho de área de trabalho e conexões rápidas de banda larga fixa, muitos operadores usam *transcodificadores* ou *servidores proxy* que alteram dinamicamente o conteúdo da Web. Eles podem modificar sua marcação HTML ou CSS para atender a telas menores (especialmente para "telefones de recursos" que não têm a capacidade de processamento para lidar com layouts complexos) e podem recompactar suas imagens (reduzindo significativamente sua qualidade) para melhorar as velocidades de entrega de página.

Mas, se você tomou o esforço de produzir uma versão otimizada para dispositivos móveis do seu site, provavelmente não quer que o operador de rede interfira mais. Você pode adicionar a seguinte linha à página\_evento de carregamento em qualquer formulário da Web do ASP.NET:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample16.cs)]

Ou, para um controlador MVC ASP.NET, você pode adicionar a substituição do método a seguir para que ele se aplique a todas as ações nesse controlador:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample17.cs)]

A mensagem HTTP resultante informa transcodificadores e proxies compatíveis com o W3C para não alterar o conteúdo. É claro que não há nenhuma garantia de que os operadores de rede móvel respeitarão essa mensagem.

### <a name="styling-mobile-pages-for-mobile-browsers"></a>Estilizando páginas móveis para navegadores móveis

Está além do escopo deste documento descrever detalhadamente quais tipos de marcação HTML funcionam corretamente ou quais técnicas de design da Web maximizam a usabilidade em determinados dispositivos. Cabe a você encontrar um layout suficientemente simples, otimizado para uma tela de tamanho móvel, sem usar truques de posicionamento HTML ou CSS não confiáveis. No entanto, uma técnica importante que vale a pena mencionar é a *marca meta viewport*.

Alguns navegadores móveis modernos, em um esforço de exibição de páginas da Web, destinados a monitores de área de trabalho, renderizam a página em uma tela virtual, também chamada de "visor" (por exemplo, o visor virtual tem 980 pixels de largura no iPhone e 850 pixels de largura no Opera Mobile por padrão) e, em seguida, dimensione o resultado para cima para caber nos pixels físicos da tela. O usuário pode ampliar e deslocar esse visor. Isso tem a vantagem de permitir que o navegador exiba a página em seu layout pretendido, mas também tem a desvantagem de que ele força o zoom e o movimento panorâmico, o que é inconveniente para o usuário. Se você estiver projetando para dispositivos móveis, é melhor criar uma tela estreita para que não seja necessária nenhuma rolagem horizontal ou de zoom.

Uma maneira de dizer ao navegador móvel a largura do visor deve ser por meio da marca meta *viewport* não padrão. Por exemplo, se você adicionar o seguinte à seção de cabeçalho da página,

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample18.html)]

… em seguida, o suporte a navegadores do smartphone fará o layout da página em uma tela virtual de 480 pixels de largura. Isso significa que, se os elementos HTML definirem suas larguras em termos percentuais, os percentuais serão interpretados em relação a essa largura de 480 pixels, não à largura padrão do visor. Como resultado, é menos provável que o usuário tenha que aplicar zoom e panorâmica horizontalmente, aprimorando consideravelmente a experiência de navegação móvel.

Se desejar que a largura do visor corresponda aos pixels físicos do dispositivo, você poderá especificar o seguinte:

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample19.html)]

Para que isso funcione corretamente, você não deve forçar explicitamente que os elementos excedam essa largura (por exemplo, usando um atributo de *largura* ou propriedade CSS), caso contrário, o navegador será forçado a usar um visor maior, independentemente. Consulte também: [mais detalhes sobre a marca viewport não padrão](https://developer.apple.com/library/safari/#documentation/AppleApplications/Reference/SafariHTMLRef/Articles/MetaTags.html).

A maioria dos smartphones modernos dá suporte à *orientação dupla*: eles podem ser mantidos no modo retrato ou paisagem. Portanto, é importante não fazer suposições sobre a largura da tela em pixels. Nem mesmo suponha que a largura da tela seja corrigida, pois o usuário pode reorientar seu dispositivo enquanto eles estão em sua página.

Dispositivos Windows Mobile e Blackberry mais antigos também podem aceitar as seguintes marcas meta no cabeçalho da página para informar que o conteúdo foi otimizado para dispositivos móveis e, portanto, não deve ser transformado.

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample20.html)]

## <a name="additional-resources"></a>Recursos adicionais

Para obter uma lista de emuladores de dispositivo móvel e simuladores que você pode usar para testar seu aplicativo Web ASP.NET móvel, consulte a página [simular dispositivos móveis populares para teste](../mobile/device-simulators.md).

## <a name="credits"></a>Credits

- Autor principal: Steven Sanderson
- Revisores/gravadores de conteúdo adicionais: James Rosewell, Mikael Söderström, Scott Hanselman, Scott Hunter
