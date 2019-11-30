---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-vb
title: Determinando quais arquivos precisam ser implantados (VB) | Microsoft Docs
author: rick-anderson
description: Os arquivos que precisam ser implantados do ambiente de desenvolvimento para o ambiente de produção dependem, em parte, se o aplicativo ASP.NET foi criado para nós...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: ea918f62-c9d6-4a7f-9bc6-e054d3764b2c
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-vb
msc.type: authoredcontent
ms.openlocfilehash: a11dadfda8b6a189acedd7ac723d85f8b2084324
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74569963"
---
# <a name="determining-what-files-need-to-be-deployed-vb"></a>Determinar quais arquivos precisam ser implantados (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_02_VB.zip) ou [baixar PDF](https://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial02_FilesToDeploy_vb.pdf)

> Os arquivos que precisam ser implantados do ambiente de desenvolvimento para o ambiente de produção dependem, em parte, se o aplicativo ASP.NET foi criado usando o modelo de site ou o modelo de aplicativo Web. Saiba mais sobre esses dois modelos de projeto e como o modelo de projeto afeta a implantação.

## <a name="introduction"></a>Introdução

Implantar um aplicativo Web ASP.NET envolve copiar os arquivos relacionados ao ASP.NET do ambiente de desenvolvimento para o ambiente de produção. Os arquivos relacionados ao ASP.NET incluem marcação de página da Web ASP.NET e código e arquivos de suporte do cliente e do lado do servidor. Os arquivos de suporte do lado do cliente são os arquivos referenciados por suas páginas da Web e enviados diretamente para as imagens de navegador, arquivos CSS e arquivos JavaScript, por exemplo. Os arquivos de suporte do lado do servidor incluem aqueles usados para processar uma solicitação no lado do servidor. Isso inclui arquivos de configuração, serviços Web, arquivos de classe, datasets tipados e arquivos de LINQ to SQL, entre outros.

Em geral, todos os arquivos de suporte do lado do cliente devem ser copiados do ambiente de desenvolvimento para o ambiente de produção, mas os arquivos de suporte do lado do servidor são copiados dependendo se você está compilando explicitamente o código do lado do servidor em um assembly (um arquivo `.dll`) ou se você tem esses assemblies gerados automaticamente. Este tutorial realça quais arquivos precisam ser implantados ao compilar explicitamente o código em um assembly em comparação com a ocorrência dessa etapa de compilação automaticamente.

## <a name="explicit-compilation-versus-automatic-compilation"></a>Compilação explícita versus compilação automática

As páginas da Web ASP.NET são divididas em marcação declarativa e código-fonte. A parte de marcação declarativa inclui HTML, controles da Web e sintaxe de DataBinding; a parte de código contém manipuladores de eventos escritos em C# Visual Basic ou código. As partes de marcação e código normalmente são separadas em arquivos diferentes: `WebPage.aspx` contém a marcação declarativa enquanto `WebPage.aspx.vb` abriga o código.

Considere uma página ASP.NET chamada `Clock.aspx` que contenha um controle rótulo cuja propriedade Text seja definida como a data e a hora atuais quando a página for carregada. A parte de marcação declarativa (em `Clock.aspx`) conteria a marcação para um controle da Web de rótulo-`<asp:Label runat="server" id="TimeLabel" />`-enquanto a parte do código (em `Clock.aspx.vb`) teria um manipulador de eventos de `Page_Load` com o seguinte código:

[!code-vb[Main](determining-what-files-need-to-be-deployed-vb/samples/sample1.vb)]

Para o mecanismo de ASP.NET atender a uma solicitação para essa página, a parte de código da página (o arquivo de`.aspx.vb` de *`WebPage`* ) deve primeiro ser compilada. Essa compilação pode acontecer de forma explícita ou automática.

Se a compilação ocorrer explicitamente, o código-fonte do aplicativo inteiro será compilado em um ou mais assemblies (`.dll` arquivos) localizados no diretório `Bin` do aplicativo. Se a compilação ocorrer automaticamente, o assembly gerado automaticamente resultante será, por padrão, colocado na pasta `Temporary ASP.NET Files`, que pode ser encontrada em `%WINDOWS%\Microsoft.NET\Framework\<version>`, embora esse local seja configurável por meio do [elemento&gt; de compilação&lt;](https://msdn.microsoft.com/library/s10awwz0.aspx) no `Web.config`. Com a compilação explícita, você deve tomar alguma atitude para compilar o código do aplicativo ASP.NET em um assembly, e essa etapa ocorre antes da implantação. Com a compilação automática, o processo de compilação ocorre no servidor Web quando o recurso é acessado pela primeira vez.

Independentemente de qual modelo de compilação você usa, a parte de marcação de todas as páginas ASP.NET (os arquivos `WebPage.aspx`) precisa ser copiada para o ambiente de produção. Com a compilação explícita, você precisa copiar os assemblies na pasta `Bin`, mas não é necessário copiar as partes do código de páginas ASP.NET (os arquivos `WebPage.aspx.vb`). Com a compilação automática, você precisa copiar os arquivos da parte do código para que o código esteja presente e possa ser compilado automaticamente quando a página for visitada. A parte de marcação de cada página da Web do ASP.NET inclui uma diretiva `@Page` com atributos que indicam se o código associado da página já foi compilado explicitamente ou se precisa ser compilado automaticamente. Como resultado, o ambiente de produção pode trabalhar com um modelo de compilação sem problemas e você não precisa aplicar nenhuma definição especial de configuração para indicar que a compilação explícita ou automática é usada.

A tabela 1 resume os diferentes arquivos a serem implantados ao usar compilação explícita versus compilação automática. Observe que, independentemente do modelo de compilação usado, você sempre deve implantar os assemblies na pasta `Bin`, se essa pasta existir. A pasta `Bin` contém os assemblies específicos para o aplicativo Web, que incluem o código-fonte compilado ao usar o modelo de compilação explícita. O diretório `Bin` também contém assemblies de outros projetos e qualquer assembly de software livre ou de terceiros que você possa estar usando, e eles precisam estar no servidor de produção. Portanto, como regra geral, copie a pasta `Bin` para a produção durante a implantação. (Se você estiver usando o modelo de compilação automática e não estiver usando assemblies externos, não terá um `Bin` Directory – OK!)

| **Modelo de compilação** | **Implantar arquivo de parte de marcação?** | **Implantar o arquivo de código-fonte?** | **Implantar assemblies no diretório `Bin`?** |
| --- | --- | --- | --- |
| Compilação explícita | Sim | Não | Sim |
| Compilação automática | Sim | Sim | Sim (se existir) |

**Tabela 1: quais arquivos são implantados depende do modelo de compilação usado.**

## <a name="taking-a-trip-down-memory-lane"></a>Fazendo uma pista de memória inativa

A abordagem de compilação que é usada depende, em parte, de como o aplicativo ASP.NET é gerenciado no Visual Studio. Depois. O início da rede no ano 2000 tem quatro versões diferentes do Visual Studio – Visual Studio .NET 2002, Visual Studio .NET 2003, Visual Studio 2005 e Visual Studio 2008. Aplicativos gerenciados ASP.NET do Visual Studio .NET 2002 e 2003 usando o modelo de *projeto de aplicativo Web* . Os principais recursos do modelo de projeto de aplicativo Web são:

- Os arquivos que fazem a composição do projeto são definidos em um único arquivo de projeto. Todos os arquivos não definidos no arquivo de projeto não são considerados parte do aplicativo Web pelo Visual Studio.
- Usa compilação explícita. Compilar o projeto compila os arquivos de código dentro do projeto em um único assembly que é colocado na pasta `Bin`.

Quando a Microsoft lançou o Visual Studio 2005, ele removeu o suporte para o modelo de projeto de aplicativo Web e o substituiu pelo modelo de projeto de site. O modelo de projeto de site é diferenciado do modelo de *projeto de aplicativo Web* das seguintes maneiras:

- Em vez de ter um único arquivo de projeto que soletra os arquivos do projeto, o sistema de arquivos é usado em vez disso. Em suma, todos os arquivos dentro da pasta (ou subpastas) do aplicativo Web são considerados parte do projeto.
- A criação de um projeto no Visual Studio não cria um assembly no diretório `Bin`. Em vez disso, criar um projeto de site relata quaisquer erros de tempo de compilação.
- Suporte para compilação automática. Os projetos de site normalmente são implantados copiando a marcação e o código-fonte para o ambiente de produção, embora o código possa ser pré-compilado (compilação explícita).

A Microsoft reativava o modelo de projeto de aplicativo Web quando lançou o Visual Studio 2005 Service Pack 1. No entanto, o Visual Web Developer continuou a dar suporte apenas ao modelo de projeto de site. A boa notícia é que essa limitação foi descartada com o Visual Web Developer 2008 Service Pack 1. Hoje, você pode criar aplicativos ASP.NET no Visual Studio (e Visual Web Developer) usando o modelo de projeto de aplicativo Web ou o modelo de projeto de site. Ambos os modelos têm seus prós e contras. Consulte [introdução aos projetos de aplicativos Web: comparando projetos de sites e projetos de aplicativos Web](https://msdn.microsoft.com/library/aa730880.aspx#wapp_topic5) para uma comparação dos dois modelos e para ajudar a decidir qual modelo de projeto funciona melhor para sua situação.

## <a name="exploring-the-sample-web-application"></a>Explorando o aplicativo Web de exemplo

O download para este tutorial inclui um aplicativo ASP.NET chamado Reviews de livros. O site imita um site de hobby que alguém pode criar para compartilhar suas revisões de livros com a comunidade online. Esse aplicativo Web ASP.NET é muito simples e consiste nos seguintes recursos:

- `Web.config`, o arquivo de configuração do aplicativo.
- Uma página mestra (`Site.master`).
- Sete páginas ASP.NET diferentes:

    - ~/`Default.aspx`-a home page do site.
    - ~/`About.aspx`-uma página "sobre o site".
    - ~/`Fiction/Default.aspx`-uma página que lista os livros de ficção que foram revisados.

        - ~/`Fiction/Blaze.aspx`-uma revisão da *Blaze*Richard Bachman romance.
    - ~/`Tech/Default.aspx`-uma página que lista os livros de tecnologia que foram revisados.

        - ~/`Tech/CYOW.aspx`-uma revisão de *criar seu próprio site*.
        - ~/`Tech/TYASP35.aspx`-uma revisão de *ensinar a ASP.NET 3,5 em 24 horas*.
- Três arquivos CSS diferentes na pasta `Styles`.
- Quatro arquivos de imagem – um logotipo da plataforma ASP.NET e imagens das capas dos três livros revisados – todos localizados na pasta `Images`.
- Um arquivo de `Web.sitemap`, que define o mapa do site e é usado para exibir menus no `Default.aspx` páginas no diretório raiz e as pastas `Fiction` e `Tech`.
- Um arquivo de classe chamado `BasePage.vb` que define uma classe base `Page`. Essa classe estende a funcionalidade da classe `Page` definindo automaticamente a propriedade `Title` com base na posição da página no mapa do site. Resumindo, qualquer classe code-behind ASP.NET que estenda `BasePage` (em vez de `System.Web.UI.Page`) terá seu título definido como um valor, dependendo de sua posição no mapa do site. Por exemplo, ao exibir a página ~/`Tech/CYOW.aspx`, o título é definido como "Home: Technology: criar seu próprio site".

A Figura 1 mostra uma captura de tela do site de revisões de livros quando visualizada por meio de um navegador. Aqui você vê a página ~/Tech/TYASP35.aspx, que revisa o livro *ensina você mesmo ASP.NET 3,5 em 24 horas*. O breadcrumb que abrange a parte superior da página e o menu na coluna à esquerda baseia-se na estrutura do mapa do site definida em `Web.sitemap`. A imagem no canto superior direito é uma das imagens de capa do livro localizadas na pasta `Images`. A aparência do site é definida por meio de regras de folha de estilos em cascata escritas por arquivos CSS na pasta `Styles`, enquanto o layout de página abrangente é definido na página mestra, `Site.master`.

[![o site de análises de livros oferece revisões sobre uma variedade de títulos](determining-what-files-need-to-be-deployed-vb/_static/image2.png)](determining-what-files-need-to-be-deployed-vb/_static/image1.png)

**Figura 1**: o site de revisões do livro oferece revisões sobre uma variedade de títulos ([clique para exibir a imagem em tamanho normal](determining-what-files-need-to-be-deployed-vb/_static/image3.png))

Este aplicativo não usa um banco de dados; cada revisão é implementada como uma página da Web separada no aplicativo. Este tutorial (e os vários tutoriais) examina a implantação de um aplicativo Web que não tem um banco de dados. No entanto, em um tutorial futuro, aprimoraremos esse aplicativo para armazenar revisões, comentários de leitor e outras informações em um banco de dados, e exploraremos quais etapas precisam ser executadas para implantar corretamente um aplicativo Web controlado por data.

> [!NOTE]
> Esses tutoriais se concentram na Hospedagem de aplicativos ASP.NET com um provedor de host da Web e não exploram tópicos auxiliares como o ASP. Sistema de mapa do site da rede ou usando uma classe de página de base. Para saber mais sobre essas tecnologias e obter mais informações sobre outros tópicos abordados em todo o tutorial, consulte a seção leitura adicional no final de cada tutorial.

O download deste tutorial tem duas cópias do aplicativo Web, cada uma implementada como um tipo de projeto do Visual Studio diferente: BookReviewsWAP, um projeto de aplicativo Web e BookReviewsWSP, um projeto de site da Web. Ambos os projetos foram criados com o Visual Web Developer 2008 SP1 e usam o ASP.NET 3,5 SP1. Para trabalhar com esses projetos, Comece descompactando o conteúdo em sua área de trabalho. Para abrir o projeto de aplicativo Web (BookReviewsWAP), navegue até a pasta `BookReviewsWAP` e clique duas vezes no arquivo da solução `BookReviewsWAP.sln`. Para abrir o projeto de site (BookReviewsWSP), inicie o Visual Studio e, no menu arquivo, escolha a opção abrir site da Web, navegue até a pasta `BookReviewsWSP` na área de trabalho e clique em OK.

As duas seções restantes neste tutorial examinam quais arquivos você precisará copiar para o ambiente de produção ao implantar o aplicativo. Os próximos dois tutoriais – [*implantando seu site usando o FTP*](deploying-your-site-using-an-ftp-client-vb.md) e [*implantando seu site usando o Visual Studio*](deploying-your-site-using-visual-studio-vb.md) -mostre diferentes maneiras de copiar esses arquivos para um provedor de host da Web.

## <a name="determining-the-files-to-deploy-for-the-web-application-project"></a>Determinando os arquivos a serem implantados para o projeto de aplicativo Web

O modelo de projeto de aplicativo Web usa compilação explícita – o código-fonte do projeto é compilado em um único assembly cada vez que você cria o aplicativo. Essa compilação inclui os arquivos code-behind das páginas ASP.NET (~/`Default.aspx.vb`, ~/`About.aspx.vb`e assim por diante), bem como a classe `BasePage.vb`. O assembly resultante é nomeado `BookReviewsWAP.dll` e está localizado no diretório `Bin` do aplicativo.

A Figura 2 mostra os arquivos que compõem o projeto de aplicativo Web de análises de livros.

[![a Gerenciador de Soluções lista os arquivos que compõem o projeto de aplicativo Web.](determining-what-files-need-to-be-deployed-vb/_static/image5.png)](determining-what-files-need-to-be-deployed-vb/_static/image4.png)

**Figura 2**: a Gerenciador de soluções lista os arquivos que compõem o projeto de aplicativo Web

> [!NOTE]
> Como mostra a Figura 2, os arquivos de código por trás das páginas ASP.NET não são exibidos no Gerenciador de Soluções para um projeto de aplicativo Web Visual Basic. Para exibir a classe code-behind de uma página, clique com o botão direito do mouse na página em Gerenciador de Soluções e escolha Exibir código.

Para implantar um aplicativo ASP.NET desenvolvido usando o modelo de projeto de aplicativo Web, comece criando o aplicativo para compilar explicitamente o código-fonte mais recente em um assembly. Em seguida, copie os seguintes arquivos para o ambiente de produção:

- Os arquivos que contêm a marcação declarativa para cada página do ASP.NET, como ~/`Default.aspx`, ~/`About.aspx`e assim por diante. Além disso, copie a marcação declarativa para quaisquer páginas mestras e controles de usuário.
- Os assemblies (`.dll` arquivos) na pasta `Bin`. Você não precisa copiar os arquivos de banco de dados do programa (`.pdb`) ou quaisquer arquivos XML que você possa encontrar no diretório `Bin`.

Você não precisa copiar os arquivos de código-fonte de páginas ASP.NET para o ambiente de produção, nem precisa copiar o arquivo de classe `BasePage.vb`.

> [!NOTE]
> Como mostra a Figura 2, a classe `BasePage` é implementada como um arquivo de classe no projeto, colocado na pasta chamada `HelperClasses`. Quando o projeto é compilado, o código no arquivo de `BasePage.vb` é compilado junto com as classes de código-behind das páginas ASP.NET no único assembly, `BookReviewsWAP.dll`. ASP.NET tem uma pasta especial chamada `App_Code` que foi projetada para manter arquivos de classe para projetos de site. O código na pasta `App_Code` é compilado automaticamente e, portanto, não deve ser usado com projetos de aplicativos Web. Em vez disso, você deve posicionar os arquivos de classe do aplicativo em uma pasta normal chamada `HelperClasses`ou `Classes`ou algo semelhante. Como alternativa, você pode posicionar arquivos de classe em um projeto de biblioteca de classes separado.

Além de copiar os arquivos de marcação relacionados ao ASP.NET e o assembly na pasta `Bin`, você também precisa copiar os arquivos de suporte do lado do cliente – as imagens e os arquivos CSS, bem como os outros arquivos de suporte do lado do servidor, `Web.config` e `Web.sitemap`. Esses arquivos de suporte do lado do cliente e do servidor precisam ser copiados para o ambiente de produção independentemente de você usar a compilação explícita ou automática.

## <a name="determining-the-files-to-deploy-for-the-web-site-project-files"></a>Determinando os arquivos a serem implantados para os arquivos de projeto do site

O modelo de projeto de site dá suporte à compilação automática, um recurso não disponível ao usar o modelo de projeto de aplicativo Web. Com a compilação explícita, você deve compilar o código-fonte do projeto em um assembly e copiar esse assembly para o ambiente de produção. Por outro lado, com a compilação automática, basta copiar o código-fonte para o ambiente de produção e ele será compilado pelo tempo de execução sob demanda, conforme necessário.

A opção de menu Build no Visual Studio está presente em projetos de aplicativos Web e em projetos de site. A criação de projetos de aplicativos Web compila o código-fonte do projeto em um único assembly localizado no diretório `Bin`; a criação de um projeto de site verifica se há erros de tempo de compilação, mas não cria nenhum assembly. Para implantar um aplicativo ASP.NET desenvolvido usando o modelo de projeto de site, tudo o que você precisa fazer é copiar os arquivos apropriados para o ambiente de produção, mas eu recomendo que você primeiro crie o projeto para garantir que não haja erros de tempo de compilação.

A Figura 3 mostra os arquivos que compõem o projeto de site da Web de análises de livros.

[![a Gerenciador de Soluções lista os arquivos que compõem o projeto de site.](determining-what-files-need-to-be-deployed-vb/_static/image7.png)](determining-what-files-need-to-be-deployed-vb/_static/image6.png)

**Figura 3**: a Gerenciador de soluções lista os arquivos que compõem o projeto de site

A implantação de um projeto de site envolve a cópia de todos os arquivos relacionados ao ASP.NET para o ambiente de produção – que inclui as páginas de marcação para páginas ASP.NET, páginas mestras e controles de usuário, juntamente com seus arquivos de código. Você também precisa copiar todos os arquivos de classe, como `BasePage.vb`. Observe que o arquivo de `BasePage.vb` está localizado na pasta `App_Code`, que é uma pasta ASP.NET especial usada em projetos de site para arquivos de classe. A pasta especial também precisa ser criada na produção, já que os arquivos de classe na pasta `App_Code` no ambiente de desenvolvimento devem ser copiados para a pasta `App_Code` na produção.

Além de copiar os arquivos de código-fonte e a marcação ASP.NET, você também precisa copiar os arquivos de suporte do lado do cliente – as imagens e os arquivos CSS, bem como os outros arquivos de suporte do lado do servidor, `Web.config` e `Web.sitemap`.

> [!NOTE]
> Os projetos de site também podem usar a compilação explícita. Um tutorial futuro examinará como compilar explicitamente um projeto de site.

## <a name="summary"></a>Resumo

Implantar um aplicativo ASP.NET envolve copiar os arquivos necessários do ambiente de desenvolvimento para o ambiente de produção. O conjunto preciso de arquivos que precisam ser sincronizados depende se o código do aplicativo ASP.NET é compilado explicitamente ou automaticamente. A estratégia de compilação empregada é influenciada pelo fato de o Visual Studio estar configurado para gerenciar o aplicativo ASP.NET usando o modelo de projeto de aplicativo Web ou o modelo de projeto de site da Web.

O modelo de projeto de aplicativo Web usa compilação explícita e compila o código do projeto em um único assembly na pasta `Bin`. Ao implantar o aplicativo, a parte de marcação das páginas ASP.NET e o conteúdo da pasta `Bin` devem ser enviados para o ambiente de produção; o código-fonte no aplicativo – os arquivos de código e as classes code-behind, por exemplo, não precisam ser copiados para o ambiente de produção.

O modelo de projeto de site usa a compilação automática por padrão, embora seja possível compilar explicitamente um projeto de site da Web, como veremos em Tutoriais futuros. Implantar um aplicativo ASP.NET que usa a compilação automática requer que a parte de marcação *e* o código-fonte sejam copiados para o ambiente de produção. O código é compilado automaticamente no ambiente de produção quando solicitado pela primeira vez.

Agora que examinamos quais arquivos precisam ser sincronizados entre os ambientes de desenvolvimento e produção, estamos prontos para implantar o aplicativo Reviews de livros em um provedor de host da Web.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Visão geral da compilação do ASP.NET](https://msdn.microsoft.com/library/ms178466.aspx)
- [Controles de usuário ASP.NET](https://msdn.microsoft.com/library/y6wb1a0e.aspx)
- [Examinando ASP. Navegação do site da rede](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Introdução aos projetos de aplicativos Web](https://msdn.microsoft.com/library/aa730880.aspx)
- [TUTORIAIS da página mestra](../master-pages/creating-a-site-wide-layout-using-master-pages-cs.md)
- [Compartilhando código entre páginas](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/pages/code.aspx)
- [Usando uma classe base personalizada para as classes code-behind de suas páginas ASP.NET](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)
- [Sistema de projeto de site do Visual Studio 2005: o que é isso e por que fazemos isso?](https://weblogs.asp.net/scottgu/archive/2005/08/21/423201.aspx)
- [Walkthrough: convertendo um projeto de site em um projeto de aplicativo Web no Visual Studio](https://msdn.microsoft.com/library/aa983476.aspx)

> [!div class="step-by-step"]
> [Anterior](asp-net-hosting-options-vb.md)
> [Próximo](deploying-your-site-using-an-ftp-client-vb.md)
