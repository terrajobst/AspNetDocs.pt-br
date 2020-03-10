---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-vb
title: Pré-compilando seu site (VB) | Microsoft Docs
author: rick-anderson
description: 'O Visual Studio oferece aos desenvolvedores de ASP.NET dois tipos de projetos: WAPs (projetos de aplicativos Web) e WSPs (projetos de site). Uma das principais diferenças betwe...'
ms.author: riande
ms.date: 06/09/2009
ms.assetid: c285dc6f-a1c6-46e6-ac03-3830947f57e3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-vb
msc.type: authoredcontent
ms.openlocfilehash: 1a46870b73f95300ef5bc1f72dda74d7d62ab11f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78627844"
---
# <a name="precompiling-your-website-vb"></a>Pré-compilação do seu site (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_15_VB.zip) ou [baixar PDF](https://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial15_Precompiling_vb.pdf)

> O Visual Studio oferece aos desenvolvedores de ASP.NET dois tipos de projetos: WAPs (projetos de aplicativos Web) e WSPs (projetos de site). Uma das principais diferenças entre os dois tipos de projeto é que WAPs deve ter o código compilado explicitamente antes da implantação, enquanto o código em um WSP pode ser compilado automaticamente no servidor Web. No entanto, é possível pré-compilar um WSP antes da implantação. Este tutorial explora os benefícios de pré-compilação e mostra como pré-compilar um site no Visual Studio e na linha de comando.

## <a name="introduction"></a>Introdução

O Visual Studio oferece aos desenvolvedores de ASP.NET dois tipos diferentes de projetos: WAP (projetos de aplicativos Web) e WSP (projetos de site). Uma das principais diferenças entre esses tipos de projeto é que WAPs exigem *compilação explícita* , enquanto WSPs usam a *compilação automática*, por padrão. Com WAPs, você compila o código do aplicativo Web em um único assembly, que é criado na pasta `Bin` do site. A implantação envolve copiar o conteúdo de marcação (os arquivos `.aspx.ascx`e `.master`) no projeto, juntamente com o assembly na pasta `Bin`; os próprios arquivos de classe code-behind não precisam ser implantados. Por outro lado, você implanta o WSPs copiando as páginas de marcação e suas classes code-behind correspondentes para o ambiente de produção. As classes code-behind são compiladas sob demanda no servidor Web.

> [!NOTE]
> Consulte a seção "compilação explícita versus compilação automática" no [tutorial *determinando quais arquivos precisam ser implantados* ](determining-what-files-need-to-be-deployed-vb.md) para obter mais informações sobre as diferenças entre os modelos de projeto, a compilação explícita e automática e como o modelo de compilação afeta a implantação.

A opção de compilação automática é simples de usar. Não há nenhuma etapa de compilação explícita e somente os arquivos que foram modificados precisam ser implantados, enquanto que a compilação explícita precisa implantar as páginas de marcação alteradas e o assembly recém compilado. No entanto, a implantação automática tem duas desvantagens potenciais:

- Como as páginas devem ser compiladas automaticamente quando são visitadas pela primeira vez, pode haver um atraso curto, mas perceptível, quando uma página ASP.NET é solicitada pelo primeiro depois de ser implantada.
- A compilação automática requer que tanto a marcação declarativa quanto o código-fonte estejam presentes no servidor Web. Isso pode ser uma opção atraente se você planeja vender o aplicativo Web para clientes que irão instalá-lo em seus servidores Web.

Se qualquer uma das duas deficiências acima forem os disjuntores de negociação, você poderá alternar para o modelo WAP ou *pré-compilar* o WSP antes da implantação. Este tutorial examina as opções de pré-compilação mais adequadas para um site hospedado e percorre o processo de pré-compilação e a implantação de um site pré-compilado.

## <a name="an-overview-of-aspnet-code-generation-and-compilation"></a>Uma visão geral da geração e compilação de código ASP.NET

Antes de examinarmos as opções de pré-compilação disponíveis, vamos falar primeiro sobre a geração de código e a compilação que ocorre quando uma página ASP.NET é solicitada pela primeira vez desde sua criação ou última atualização. Como você sabe, as páginas ASP.NET são compostas de duas partes: marcação declarativa no arquivo de `.aspx`; e uma parte do código-fonte, normalmente em um arquivo de classe code-behind separado (`.aspx.vb`). As etapas executadas pelo tempo de execução quando uma página ASP.NET é solicitada dependem do modelo de compilação do aplicativo.

Com WAPs, o código-fonte das páginas deve ser compilado explicitamente em um único assembly antes de ser implantado. Durante a implantação, esse assembly e as várias páginas de marcação são copiados para o ambiente de produção. Quando uma solicitação chega ao servidor Web para uma página ASP.NET, o tempo de execução cria uma instância da classe code-behind da página e invoca seu método `ProcessRequest`, que inicia o ciclo de vida da página e, por fim, gera o conteúdo da página, que é retornado ao solicitante. O tempo de execução pode funcionar com a classe code-behind da página ASP.NET porque a classe code-behind já foi compilada em um assembly antes da implantação.

Com o WSPs e a compilação automática, não há nenhuma etapa de compilação explícita antes da implantação. Em vez disso, a implantação envolve copiar o conteúdo de código-fonte e declarativo para o ambiente de produção. Quando uma solicitação chega ao servidor Web para uma página ASP.NET pela primeira vez, desde que a página tenha sido criada ou atualizada por último, o tempo de execução deve primeiro compilar a classe code-behind em um assembly. Esse assembly compilado é salvo na pasta `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files`, embora o local dessa pasta possa ser personalizado por meio do elemento `<pages>` no `Web.config`. Como o assembly é salvo em disco, ele não precisa ser recompilado em solicitações subsequentes para a mesma página.

> [!NOTE]
> Como você esperaria, há um pequeno atraso ao solicitar uma página pela primeira vez (ou pela primeira vez desde que ela foi alterada) em um site que usa a compilação automática, pois leva um tempo para que o servidor compile o código da página e salve o assembly resultante em disco.

Em suma, com a compilação explícita, é necessário compilar o código-fonte do site antes da implantação, salvando o tempo de execução de ter que executar essa etapa. Com a compilação automática, o tempo de execução manipula a compilação do código-fonte das páginas, mas com um pequeno custo de inicialização para a primeira visita à página desde que ela foi criada ou atualizada pela última vez.

Mas e quanto à parte declarativa das páginas ASP.NET (o arquivo `.aspx`)? É óbvio que há uma relação entre os arquivos de `.aspx` e o código em suas classes code-behind, pois os controles da Web definidos na marcação declarativa podem ser acessados no código. Também é óbvio que o conteúdo na `.aspx` arquivos influencia muito a marcação renderizada gerada pela página. Então, como funciona o tempo de execução com a sintaxe de texto, HTML e controle da Web definida no arquivo de `.aspx` para gerar o conteúdo renderizado da página solicitada?

Não quero ficar muito postos nos detalhes da implementação de baixo nível, que variam entre WAPs e WSPs, mas em resumo o tempo de execução gera automaticamente um arquivo de classe que contém os vários controles da Web como membros e métodos protegidos. Esse arquivo gerado é implementado como uma *classe parcial* para a classe code-behind correspondente. (As[classes parciais](http://www.dotnet-guide.com/partialclasses.html) permitem que o conteúdo de uma única classe seja distribuído entre vários arquivos.) Portanto, a classe code-behind é definida em dois locais: no arquivo `.aspx.vb` que você cria e nessa classe gerada automaticamente, criada pelo tempo de execução. Essa classe gerada automaticamente é armazenada na pasta `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files`.

O importante é que, para que uma página ASP.NET seja renderizada pelo tempo de execução, suas partes declarativas e de código-fonte devem ser compiladas em um assembly. Com WAPs, o código-fonte é compilado explicitamente em um assembly antes da implantação, mas a marcação declarativa ainda deve ser convertida em código e compilada pelo tempo de execução no servidor Web. Com o WSPs usando a compilação automática, o código-fonte e a marcação declarativa precisam ser compilados pelo servidor Web.

É possível usar a compilação explícita com o modelo WSP. Você pode compilar explicitamente a parte do código-fonte, como com o modelo WAP. Além disso, você também pode compilar a marcação declarativa.

## <a name="precompilation-options"></a>Opções de pré-compilação

O .NET Framework é fornecido com uma [ferramenta de compilação ASP.net (`aspnet_compiler.exe`)](https://msdn.microsoft.com/library/ms229863.aspx) que permite que você compile o código-fonte (e até mesmo o conteúdo) de um aplicativo ASP.NET criado usando o modelo WSP. Essa ferramenta foi lançada com o .NET Framework versão 2,0 e está localizada na pasta `%WINDIR%\Microsoft.NET\Framework\v2.0.50727`; Ela pode ser usada na linha de comando ou iniciada no Visual Studio por meio da opção publicar site do menu Compilar.

A ferramenta de compilação fornece duas formas gerais de compilação: pré-compilação in-loco e pré-compilação para implantação. Com a pré-compilação in-loco, você executa a ferramenta de `aspnet_compiler.exe` da linha de comando e especifica o caminho para o diretório virtual ou o caminho físico de um site que reside em seu computador. Em seguida, a ferramenta de compilação compila cada página do ASP.NET no projeto, armazenando a versão compilada na pasta `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files`, assim como se as páginas tivessem sido visitadas pela primeira vez por meio de um navegador. A pré-compilação in-loco pode acelerar a primeira solicitação feita para páginas ASP.NET implantadas recentemente no seu site, pois minimiza o tempo de execução da necessidade de executar essa etapa. No entanto, a pré-compilação in-loco não é útil para a maioria dos sites hospedados porque requer que você possa executar programas da linha de comando do servidor Web. Em ambientes de hospedagem compartilhada, esse nível de acesso não é permitido.

> [!NOTE]
> Para obter mais informações sobre pré-compilação in-loco, confira [como: Precompile ASP.NET Web sites](https://msdn.microsoft.com/library/ms227972.aspx) e Precompile [no ASP.NET 2,0](http://www.odetocode.com/Articles/417.aspx).

Em vez de compilar as páginas no site para a pasta `Temporary ASP.NET Files`, a pré-compilação para implantação compila as páginas em um diretório de sua escolha e em um formato que pode ser implantado no ambiente de produção.

Há dois tipos de pré-compilação para implantação que exploramos neste tutorial: pré-compilação com uma interface do usuário atualizável e pré-compilação com uma interface do usuário não atualizável. A pré-compilação com uma interface do usuário atualizável deixa a marcação declarativa nos arquivos `.aspx`, `.ascx`e `.master`, permitindo, assim, que um desenvolvedor exiba e, se desejado, modifique a marcação declarativa no servidor de produção. A pré-compilação com uma interface do usuário não atualizável gera `.aspx` páginas que são anuladas de qualquer conteúdo e remove `.ascx` e `.master` arquivos, ocultando, assim, a marcação declarativa e proibindo um desenvolvedor de alterá-lo do ambiente de produção.

### <a name="precompiling-for-deployment-with-an-updatable-user-interface"></a>Pré-compilação para implantação com uma interface de usuário atualizável

A melhor maneira de entender a pré-compilação para a implantação é ver um exemplo em ação. Vamos pré-compilar o Book Reviews WSP for Deployment usando uma interface de usuário atualizável. A ferramenta de compilação ASP.NET pode ser chamada no menu de compilação do Visual Studio ou na linha de comando. Esta seção examina o uso da ferramenta no Visual Studio; a seção "pré-compilando da linha de comando" examina a execução da ferramenta do compilador na linha de comando.

Abra o catálogo revisar WSP no Visual Studio, vá para o menu Compilar e selecione a opção de menu publicar site. Isso inicia a caixa de diálogo Publicar site (consulte a **Figura 1**), em que é possível especificar o local de destino, se a interface do usuário do site pré-compilado é atualizável e outras opções da ferramenta do compilador. O local de destino pode ser um servidor Web remoto ou servidor de FTP, mas, por enquanto, escolha uma pasta no disco rígido do computador. Como queremos pré-compilar o site com uma interface de usuário atualizável, deixe a caixa de seleção "permitir que este site pré-compilado seja atualizável" marcada e clique em OK.

[![](precompiling-your-website-vb/_static/image2.png)](precompiling-your-website-vb/_static/image1.png)

**Figura 1**: a ferramenta de compilação ASP.NET pré-compilará seu site para o local de destino especificado  
 ([Clique para exibir a imagem em tamanho normal](precompiling-your-website-vb/_static/image3.png))

> [!NOTE]
> A opção publicar site no menu Compilar não está disponível no Visual Web Developer. Se você estiver usando o Visual Web Developer, será necessário usar a versão de linha de comando da ferramenta de compilação ASP.NET, que é abordada na seção "pré-compilando da linha de comando".

Depois de pré-compilar o site, navegue até o local de destino que você inseriu na caixa de diálogo Publicar site. Reserve um tempo para comparar o conteúdo dessa pasta com o conteúdo do seu site. A **Figura 2** mostra a pasta do site de revisões de livros. Observe que ele contém os arquivos `.aspx` e `.aspx.cs`. Além disso, observe que o diretório `Bin` inclui apenas um arquivo, `Elmah.dll`, que adicionamos no [tutorial anterior](logging-error-details-with-elmah-vb.md)

[![](precompiling-your-website-vb/_static/image5.png)](precompiling-your-website-vb/_static/image4.png)

**Figura 2**: o diretório do projeto contém arquivos de `.aspx` e `.aspx.cs`; a pasta `Bin` inclui apenas `Elmah.dll`  
 ([Clique para exibir a imagem em tamanho normal](precompiling-your-website-vb/_static/image6.png))

A **Figura 3** mostra a pasta de local de destino cujo conteúdo foi criado pela ferramenta de compilação ASP.net. Essa pasta não contém nenhum arquivo code-behind. Além disso, o diretório `Bin` desta pasta inclui vários assemblies e dois arquivos de `.compiled` além do assembly `Elmah.dll`.

[![](precompiling-your-website-vb/_static/image8.png)](precompiling-your-website-vb/_static/image7.png)

**Figura 3**: a pasta de local de destino inclui os arquivos para implantação  
 ([Clique para exibir a imagem em tamanho normal](precompiling-your-website-vb/_static/image9.png))

Diferentemente da compilação explícita em WAPs, o processo de pré-compilação para implantação não cria um assembly para todo o site. Em vez disso, ele agrupa várias páginas em cada assembly. Ele também compila o arquivo de `Global.asax` (se presente) em seu próprio assembly, bem como qualquer classe na pasta `App_Code`. Os arquivos que contêm a marcação declarativa para páginas da Web ASP.NET, controles de usuário e páginas mestras (`.aspx`, `.ascx`e `.master` arquivos, respectivamente) são copiados como estão para o diretório de local de destino. Da mesma forma, o arquivo de `Web.config` é copiado diretamente, juntamente com quaisquer arquivos estáticos, como imagens, classes CSS e arquivos PDF. Para obter uma descrição mais formal de como a ferramenta de compilação lida com vários tipos de arquivo, consulte [manipulação de arquivos durante a pré-compilação de ASP.net](https://msdn.microsoft.com/library/e22s60h9.aspx).

> [!NOTE]
> Você pode instruir a ferramenta de compilação para criar um assembly por página ASP.NET, controle de usuário ou página mestra marcando a caixa de seleção "nomes fixos usados e assemblies de página única" na caixa de diálogo Publicar site. Ter cada página ASP.NET compilada em seu próprio assembly permite um controle mais refinado sobre a implantação. Por exemplo, se você atualizou uma única página da Web ASP.NET e precisava implantar essa alteração, só precisará implantar o arquivo de `.aspx` da página e o assembly associado ao ambiente de produção. Consulte [como: gerar nomes fixos com a ferramenta de compilação ASP.net](https://msdn.microsoft.com/library/ms228040.aspx) para obter mais informações.

O diretório de local de destino também contém um arquivo que não era parte do projeto Web pré-compilado, ou seja, `PrecompiledApp.config`. Esse arquivo informa o tempo de execução ASP.NET de que o aplicativo foi pré-compilado e se foi pré-compilado com uma interface do usuário atualizável ou com bom desempenho.

Por fim, Reserve um momento para abrir um dos arquivos de `.aspx` no local de destino usando o Visual Studio ou seu editor de texto de sua escolha. Ao pré-compilar para implantação com uma interface de usuário atualizável, as páginas ASP.NET no diretório de local de destino contêm exatamente a mesma marcação que os arquivos correspondentes no site.

### <a name="precompiling-for-deployment-with-a-non-updatable-user-interface"></a>Pré-compilação para implantação com uma interface de usuário não atualizável

A ferramenta de compilador ASP.NET também pode ser usada para pré-compilar um site para implantação com uma interface do usuário não atualizável. A pré-compilação do site com uma interface do usuário não atualizável funciona de forma muito parecida com uma interface do usuário atualizável, a principal diferença é que as páginas ASP.NET, os controles de usuário e as páginas mestras no diretório de destino são eliminados de sua marcação. Para pré-compilar um site para implantação com uma interface do usuário não atualizável, escolha a opção publicar site no menu Compilar, mas desmarque a opção "permitir que este site pré-compilado seja atualizável" (consulte a **Figura 4**).

[![](precompiling-your-website-vb/_static/image11.png)](precompiling-your-website-vb/_static/image10.png)

**Figura 4**: desmarque a opção "permitir que este site pré-compilado seja atualizável" para pré-compilar com uma interface do usuário não atualizável  
 ([Clique para exibir a imagem em tamanho normal](precompiling-your-website-vb/_static/image12.png))

A **Figura 5** mostra a pasta de local de destino após a pré-compilação com uma interface de usuário não atualizável.

[![](precompiling-your-website-vb/_static/image14.png)](precompiling-your-website-vb/_static/image13.png)

**Figura 5**: a pasta do local de destino para implantação com uma interface do usuário não atualizável  
 ([Clique para exibir a imagem em tamanho normal](precompiling-your-website-vb/_static/image15.png))

Compare a **Figura 3** à **Figura 5**. Embora as duas pastas possam parecer idênticas, observe que a pasta da interface do usuário não atualizável não tem a página mestra, `Site.master`. E, embora a **Figura 5** inclua as várias páginas do ASP.net, se você exibir o conteúdo desses arquivos, verá que eles foram retirados de sua marcação declarativa e substituídos pelo texto do espaço reservado: "Este é um arquivo de marcador gerado pela ferramenta de pré-compilação e não deve ser excluído!"

[![](precompiling-your-website-vb/_static/image17.png)](precompiling-your-website-vb/_static/image16.png)

**Figura 5**: a marcação declarativa foi removida das páginas ASP.net

As `Bin` pastas nas **figuras 3** e **5** diferem mais substancialmente. Além dos assemblies, a pasta `Bin` na **Figura 5** inclui um arquivo `.compiled` para cada página ASP.net, controle de usuário e página mestra.

Pré-compilar um site com uma interface do usuário não atualizável é útil em situações em que você não deseja que o conteúdo das páginas do ASP.NET seja modificado pela pessoa ou empresa que instala ou gerencia o site no ambiente de produção. Se você criar um aplicativo Web ASP.NET que você vende aos clientes para instalar em seus próprios servidores Web, convém garantir que eles não modifiquem a aparência do seu site editando diretamente as páginas de `.aspx` que você os envia. Ao pré-compilar seu site com uma interface do usuário não atualizável, você envia o espaço reservado `.aspx` páginas como parte da instalação, impedindo que seus clientes examinem ou modifiquem seu conteúdo.

### <a name="precompiling-from-the-command-line"></a>Pré-compilando a partir da linha de comando

Nos bastidores, a caixa de diálogo Publicar site do Visual Studio invoca a ferramenta de compilação ASP.NET (`aspnet_compiler.exe`) para pré-compilar o site. Como alternativa, você pode invocar essa ferramenta na linha de comando. Na verdade, se você usar o Visual Web Developer, precisará executar a ferramenta do compilador na linha de comando, pois o menu de compilação do Visual Web Developer não inclui a opção Publish Web site.

Para usar a ferramenta do compilador na linha de comando, comece descartando até a linha de comando e navegando até o diretório do Framework, `%WINDIR%\Microsoft.NET\Framework\v2.0.50727`. Em seguida, insira a seguinte instrução na linha de comando:

`aspnet_compiler -p "physical_path_to_app" -v / -f -u "target_location_folder"`

O comando acima inicia a ferramenta de compilador ASP.NET (`aspnet_compiler.exe`) e, por meio da opção `-p`, instrui-o a pré-compilar o site com raiz no *caminho de\_físico\_para o aplicativo\_* ; Esse valor será algo como `C:\MySites\BookReviews`e deve ser delimitado por aspas.

A opção `-v` especifica o diretório virtual do site. Se seu site estiver registrado como o site padrão na metabase do IIS, você poderá omitir a opção `-p` e especificar apenas o diretório virtual do aplicativo. Se você usar a opção `-p`, o valor prosseguirá com a opção `-v` indicará a raiz do site e será usado para resolver referências de raiz de aplicativo. Por exemplo, se você especificar um valor de `-v /MySite`, as referências no aplicativo para `~/path/file` serão resolvidas como `~/MySite/path/file`. Como o site de análises de livros está localizado no diretório raiz na minha empresa de hospedagem Web, usei a opção `-v /`.

A opção `-f`, se presente, instrui a ferramenta de compilação a substituir o diretório de *pasta\_de destino\_local* , caso ele já exista. Se você omitir a opção `-f` e a pasta de local de destino já existir, a ferramenta de compilação será encerrada com o erro: "erro asremovetime: o diretório de destino não está vazio. Exclua-o manualmente ou escolha um destino diferente. "

A opção `-u`, se presente, informa a ferramenta para criar uma interface do usuário atualizável. Omita essa opção para pré-compilar o site com uma interface do usuário não atualizável.

Por fim, o *local do\_de destino\_pasta* é o caminho físico para o diretório de local de destino; Esse valor será algo como `C:\MySites\Output\BookReviews`e deve ser delimitado por aspas.

## <a name="deploying-the-precompiled-website"></a>Implantando o site pré-compilado

Neste ponto, vimos como usar a ferramenta de compilação ASP.NET para pré-compilar um site usando as opções de interface do usuário atualizáveis e não atualizáveis. No entanto, nossos exemplos até aqui têm pré-compilado o site para uma pasta local e não para o ambiente de produção. A boa notícia é que a implantação do site pré-compilado é uma Breeze e pode ser feita por meio do Visual Studio ou de algum outro mecanismo de cópia de arquivo, como de um cliente FTP autônomo.

A caixa de diálogo Publicar site da Web (mostrada pela primeira vez na **Figura 1**) tem uma opção local de destino, que indica onde os arquivos de site pré-compilados são copiados. Esse local pode ser um servidor Web remoto ou servidor de FTP. A inserção de um servidor remoto nessa caixa de texto pré-compila e implanta o site no servidor especificado em uma única etapa. Como alternativa, você pode pré-compilar o site para uma pasta local e, em seguida, copiar manualmente o conteúdo dessa pasta para o ambiente de produção via FTP ou alguma outra abordagem.

Ter o site pré-compilado automaticamente implantado por meio da caixa de diálogo Publicar site do Visual Studio é útil para sites simples em que não há nenhuma diferença de configuração entre os ambientes de desenvolvimento e produção. No entanto, conforme observado nas [ *diferenças de configuração comuns entre o tutorial de desenvolvimento e produção* ](common-configuration-differences-between-development-and-production-vb.md) , não é incomum que essas diferenças existam. Por exemplo, o livro Reviews Web Application usa um banco de dados diferente no ambiente de produção do que no ambiente de desenvolvimento. Quando o Visual Studio publica o site em um servidor remoto, ele copia de forma oculta as informações do arquivo de configuração no ambiente de desenvolvimento.

Para sites com diferenças de configuração entre os ambientes de desenvolvimento e produção, pode ser melhor pré-compilar o site para um diretório local, copiar os arquivos de configuração específicos da produção e, em seguida, copiar o conteúdo da saída pré-compilada para produção.

Para obter um atualizador sobre a cópia de arquivos do ambiente de desenvolvimento para o ambiente de produção, consulte a [*implantação de seu site usando um cliente FTP*](deploying-your-site-using-an-ftp-client-vb.md) e [*implantando seu site usando os tutoriais do Visual Studio*](determining-what-files-need-to-be-deployed-vb.md) .

## <a name="summary"></a>Resumo

O ASP.NET dá suporte a dois modos de compilação: automática e explícita. Como discutido nos tutoriais anteriores, o WAPs (projetos de aplicativos Web) usa a compilação explícita, enquanto os projetos de site (WSPs) usam a compilação automática, por padrão. No entanto, é possível compilar explicitamente um WSP antes da implantação usando a ferramenta de compilação ASP.NET.

Este tutorial se concentrou na pré-compilação da ferramenta de compilação para o suporte à implantação. Ao pré-compilar para implantação, a ferramenta de compilação cria uma pasta de local de destino, compila o código-fonte do aplicativo Web especificado e copia esses assemblies compilados e os arquivos de conteúdo para a pasta de local de destino. A ferramenta de compilação pode ser configurada para criar uma interface do usuário atualizável ou não atualizável. Ao pré-compilar com uma opção de interface do usuário não atualizável, a marcação declarativa nos arquivos de conteúdo é removida. Resumindo, a pré-compilação permite que você implante seu aplicativo baseado no projeto de site sem incluir nenhum arquivo de código-fonte e com a marcação declarativa removida, se desejado.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Pré-compilação de site da ASP.NET](https://msdn.microsoft.com/library/ms228015.aspx)
- [Codebehind e compilação no ASP.NET 2,0](https://msdn.microsoft.com/magazine/cc163675.aspx)
- [Pré-compilação em ASP.NET](http://www.odetocode.com/Articles/417.aspx)
- [Opções de site pré-compilado no ASP.NET](http://www.dotnetperls.com/precompiled)

> [!div class="step-by-step"]
> [Anterior](logging-error-details-with-elmah-vb.md)
> [Próximo](users-and-roles-on-the-production-website-vb.md)
