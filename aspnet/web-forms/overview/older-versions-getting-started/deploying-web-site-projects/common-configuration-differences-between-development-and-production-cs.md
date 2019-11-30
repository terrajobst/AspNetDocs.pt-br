---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs
title: Diferenças de configuração comuns entre desenvolvimento e produçãoC#() | Microsoft Docs
author: rick-anderson
description: Nos tutoriais anteriores, implantamos nosso site copiando todos os arquivos pertinentes do ambiente de desenvolvimento para o ambiente de produção. No entanto, i...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: 721a5c37-7e21-48e0-832e-535c6351dcae
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs
msc.type: authoredcontent
ms.openlocfilehash: 60379c87a8cf58b89066a6070ac659e65930b4fa
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74620426"
---
# <a name="common-configuration-differences-between-development-and-production-c"></a>Diferenças de configuração comuns entre desenvolvimento e produção (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar PDF](https://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial05_ConfigDifferences_cs.pdf)

> Nos tutoriais anteriores, implantamos nosso site copiando todos os arquivos pertinentes do ambiente de desenvolvimento para o ambiente de produção. No entanto, não é incomum que haja diferenças de configuração entre ambientes, o que exige que cada ambiente tenha um arquivo Web. config exclusivo. Este tutorial examina as diferenças de configuração típicas e examina as estratégias para manter informações de configuração separadas.

## <a name="introduction"></a>Introdução

Os dois últimos tutoriais percorrem a implantação de um aplicativo Web simples. O tutorial [*implantando seu site usando um cliente FTP*](deploying-your-site-using-an-ftp-client-cs.md) mostrou como usar um cliente FTP autônomo para copiar os arquivos necessários do ambiente de desenvolvimento para produção. O tutorial anterior, [*implantando seu site usando o Visual Studio*](deploying-your-site-using-visual-studio-cs.md), examinou na implantação usando a ferramenta Copiar site da Web e a opção Publicar do Visual Studio. Em ambos os tutoriais, cada arquivo no ambiente de produção era uma cópia de um arquivo no ambiente de desenvolvimento. No entanto, não é incomum que os arquivos de configuração no ambiente de produção sejam diferentes daqueles no ambiente de desenvolvimento. A configuração de um aplicativo Web é armazenada no arquivo de `Web.config` e normalmente inclui informações sobre recursos externos, como servidores de banco de dados, Web e de email. Ele também verifica o comportamento do aplicativo em determinadas situações, como o curso de ação a ser tomada quando ocorre uma exceção sem tratamento.

Ao implantar um aplicativo Web, é importante que as informações de configuração corretas terminem no ambiente de produção. Na maioria dos casos, o arquivo de `Web.config` no ambiente de desenvolvimento não pode ser copiado para o ambiente de produção como está. Em vez disso, uma versão personalizada do `Web.config` precisa ser carregada para produção. Este tutorial revisa brevemente algumas das diferenças de configuração mais comuns; Ele também resume algumas técnicas para manter diferentes informações de configuração entre os ambientes.

## <a name="typical-configuration-differences-between-the-development-and-production-environments"></a>Diferenças de configuração típicas entre os ambientes de desenvolvimento e de produção

O arquivo de `Web.config` inclui uma variedade de informações de configuração para um aplicativo ASP.NET. Algumas dessas informações de configuração são as mesmas, independentemente do ambiente. Por exemplo, as configurações de autenticação e as regras de autorização de URL escritas nos elementos `<authentication>` e `<authorization>` do arquivo de `Web.config` são geralmente as mesmas, independentemente do ambiente. Mas outras informações de configuração, como informações sobre recursos externos, normalmente diferem dependendo do ambiente.

As cadeias de conexão de banco de dados são um exemplo primo de informações de configuração que diferem com base no ambiente. Quando um aplicativo Web se comunica com um servidor de banco de dados, ele deve primeiro estabelecer uma conexão, e isso é obtido por meio de uma [cadeia de conexão](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string). Embora seja possível embutir a cadeia de conexão do banco de dados diretamente nas páginas da Web ou no código que se conecta ao banco de dados, é melhor colocá-lo `Web.config`[elemento`<connectionStrings>`](https://msdn.microsoft.com/library/bf7sd233.aspx) para que as informações da cadeia de conexão estejam em um único local centralizado. Muitas vezes, um banco de dados diferente é usado durante o desenvolvimento do que é usado na produção; Consequentemente, as informações da cadeia de conexão devem ser exclusivas para cada ambiente.

> [!NOTE]
> Os tutoriais futuros exploram a implantação de aplicativos controlados por dados. nesse ponto, vamos nos aprofundar nas especificações de como as cadeias de conexão de banco de dado são armazenadas no arquivo de configuração.

O comportamento pretendido dos ambientes de desenvolvimento e de produção difere substancialmente. Um aplicativo Web no ambiente de desenvolvimento está sendo criado, testado e depurado por um pequeno grupo de desenvolvedores. No ambiente de produção, o mesmo aplicativo está sendo visitado por vários usuários simultâneos diferentes. O ASP.NET inclui uma série de recursos que ajudam os desenvolvedores a testar e depurar um aplicativo, mas esses recursos devem ser desabilitados por motivos de desempenho e segurança no ambiente de produção. Vamos examinar alguns desses parâmetros de configuração.

### <a name="configuration-settings-that-impact-performance"></a>Definições de configuração que afetam o desempenho

Quando uma página ASP.NET é visitada pela primeira vez (ou pela primeira vez depois que ela é alterada), sua marcação declarativa deve ser convertida em uma classe e essa classe deve ser compilada. Se o aplicativo Web usar a compilação automática, a classe code-behind da página também precisará ser compilada. Você pode configurar uma variedade de opções de compilação por meio do [elemento`<compilation>`](https://msdn.microsoft.com/library/s10awwz0.aspx)do arquivo de `Web.config`.

O atributo debug é um dos atributos mais importantes no elemento `<compilation>`. Se o atributo `debug` for definido como "true", os assemblies compilados incluem símbolos de depuração, que são necessários ao depurar um aplicativo no Visual Studio. Mas os símbolos de depuração aumentam o tamanho do assembly e impõem requisitos de memória adicionais ao executar o código. Além disso, quando o atributo `debug` é definido como "true", qualquer conteúdo retornado pelo `WebResource.axd` não é armazenado em cache, o que significa que cada vez que um usuário visita uma página, ele precisará baixar novamente o conteúdo estático retornado pelo `WebResource.axd`.

> [!NOTE]
> `WebResource.axd` é um manipulador HTTP interno introduzido no ASP.NET 2,0 que os controles de servidor usam para recuperar recursos incorporados, como arquivos de script, imagens, arquivos CSS e outros conteúdos. Para obter mais informações sobre como `WebResource.axd` funciona e como você pode usá-lo para acessar recursos incorporados de seus controles de servidor personalizados, consulte [acessando recursos inseridos por meio de uma URL usando `WebResource.axd`](http://aspnet.4guysfromrolla.com/articles/080906-1.aspx).

O atributo `debug` do elemento de `<compilation>` geralmente é definido como "true" no ambiente de desenvolvimento. Na verdade, esse atributo deve ser definido como "true" para depurar um aplicativo Web; Se você tentar depurar um aplicativo ASP.NET do Visual Studio e o atributo `debug` estiver definido como "false", o Visual Studio exibirá uma mensagem explicando que o aplicativo não pode ser depurado até que o atributo `debug` seja definido como "true" e oferecerá essa alteração para você.

Você **nunca** deve ter o atributo `debug` definido como "true" em um ambiente de produção devido a seu impacto no desempenho. Para obter uma discussão mais completa sobre este tópico, consulte a postagem do blog de [Scott Guthrie](https://weblogs.asp.net/scottgu/), [não execute aplicativos ASP.net de produção com `debug="true"` habilitado](https://weblogs.asp.net/scottgu/442448).

### <a name="custom-errors-and-tracing"></a>Rastreamento e erros personalizados

Quando ocorre uma exceção sem tratamento em um aplicativo ASP.NET, ele se parece com o tempo de execução em que um de três coisas acontece:

- Uma mensagem de erro de tempo de execução genérico é exibida. Esta página informa ao usuário que houve um erro de tempo de execução, mas não fornece nenhum detalhe sobre o erro.
- Uma mensagem de detalhes de exceção é exibida, que inclui informações sobre a exceção que acabou de ser lançada.
- Uma página de erro personalizada é exibida, que é uma página ASP.NET que você cria, que exibe qualquer mensagem que desejar.

O que acontece na face de uma exceção sem tratamento depende da [seção`<customErrors>`](https://msdn.microsoft.com/library/h0hfz6fc.aspx)do arquivo de `Web.config`.

Ao desenvolver e testar um aplicativo, ele ajuda a ver detalhes de qualquer exceção no navegador. No entanto, a exibição de detalhes de exceção em um aplicativo na produção é um risco de segurança potencial. Além disso, é unflattering e faz com que seu site pareça não profissional. O ideal é que, no caso de uma exceção sem tratamento, um aplicativo Web no ambiente de desenvolvimento mostrará os detalhes da exceção, enquanto o mesmo aplicativo em produção mostrará uma página de erro personalizada.

> [!NOTE]
> A configuração da seção `<customErrors>` padrão mostra a mensagem de detalhes da exceção somente quando a página está sendo visitada por meio de localhost e mostra a página de erro de tempo de execução genérica de outra forma. Isso não é o ideal, mas é uma garantia de saber que o comportamento padrão não revela detalhes de exceção para visitantes não locais. Um tutorial futuro examina a seção `<customErrors>` em mais detalhes e mostra como ter uma página de erro personalizada mostrada quando ocorre um erro na produção.

Outro recurso do ASP.NET que é útil durante o desenvolvimento é o rastreamento. O rastreamento, se habilitado, registra as informações sobre cada solicitação de entrada e fornece uma página da Web especial, `Trace.axd`, para exibir os detalhes da solicitação recente. Você pode ativar e configurar o rastreamento por meio do [elemento`<trace>`](https://msdn.microsoft.com/library/6915t83k.aspx) no `Web.config`.

Se você habilitar o rastreamento, verifique se ele está desabilitado na produção. Como as informações de rastreamento incluem cookies, dados de sessão e outras informações potencialmente confidenciais, é importante desabilitar o rastreamento em produção. A boa notícia é que, por padrão, o rastreamento está desabilitado e o arquivo de `Trace.axd` só é acessível por meio do localhost. Se você alterar essas configurações padrão em desenvolvimento, certifique-se de que elas sejam reativadas na produção.

## <a name="techniques-for-maintaining-separate-configuration-information"></a>Técnicas para manter informações de configuração separadas

Ter definições de configuração diferentes nos ambientes de desenvolvimento e produção complica o processo de implantação. Nos dois tutoriais anteriores, o processo de implantação envolvia a cópia de todos os arquivos necessários do desenvolvimento para a produção, mas essa abordagem só funciona se as informações de configuração forem as mesmas em ambos os ambientes. Há uma variedade de técnicas para implantar um aplicativo com informações de configuração variadas. Vamos catalogar algumas dessas opções para aplicativos Web hospedados.

### <a name="manually-deploying-the-production-environment-configuration-file"></a>Implantando manualmente o arquivo de configuração do ambiente de produção

A abordagem mais simples é manter duas versões do arquivo de `Web.config`: uma para o ambiente de desenvolvimento e outra para o ambiente de produção. Implantar um site para produção envolve copiar todos os arquivos para o servidor de produção no ambiente de desenvolvimento *, exceto* para o arquivo de `Web.config`. Em vez disso, o arquivo de `Web.config` específico do ambiente de produção seria copiado para produção.

Essa abordagem não é muito sofisticada, mas é fácil de implementar porque as informações de configuração raramente são alteradas. Ele funciona melhor para aplicativos com uma pequena equipe de desenvolvimento hospedada em um único servidor Web e cujas informações de configuração não são alteradas com freqüência. É mais tenable ao implantar manualmente os arquivos de aplicativo usando um cliente FTP autônomo. Ao usar a ferramenta Copiar site da Web ou a opção Publicar do Visual Studio, primeiro você precisará alternar o arquivo de `Web.config` específico da implantação com o específico da produção antes da implantação e depois trocá-los novamente após a conclusão da implantação.

### <a name="change-the-configuration-during-the-build-or-deployment-process"></a>Alterar a configuração durante o processo de compilação ou implantação

As discussões, até agora, assumiram um processo de compilação e implantação ad hoc. Muitos projetos de software maiores têm processos mais formalizados que utilizam ferramentas de terceiros, de código aberto ou de terceiros. Para esses projetos, você provavelmente pode personalizar o processo de compilação ou implantação para modificar adequadamente as informações de configuração antes que elas sejam enviadas para a produção. Se você criar seu aplicativo Web usando o [MSBuild](http://en.wikipedia.org/wiki/MSBuild), o [Nant](http://nant.sourceforge.net/)ou alguma outra ferramenta de compilação, provavelmente poderá adicionar uma etapa de compilação para modificar o arquivo de `Web.config` para incluir as configurações específicas de produção. Ou seu fluxo de trabalho de implantação pode se conectar programaticamente ao servidor de controle do código-fonte e recuperar o arquivo de `Web.config` apropriado.

A abordagem real para obter as informações de configuração apropriadas para a produção varia muito com base em suas ferramentas e fluxo de trabalho. Como tal, não nos aprofundaremos mais neste tópico. Se você estiver usando uma ferramenta de compilação popular como MSBuild ou NAnt, poderá encontrar artigos de implantação e tutoriais específicos para essas ferramentas por meio de uma pesquisa na Web.

### <a name="managing-configuration-differences-via-the-web-deployment-project-add-in"></a>Gerenciando diferenças de configuração por meio do suplemento do projeto de implantação da Web

Na 2006, a Microsoft lançou o suplemento de projeto de desenvolvimento da Web para o Visual Studio 2005. Um suplemento para o Visual Studio 2008 foi lançado em 2008. Esse suplemento permite que os desenvolvedores de ASP.NET criem um projeto de implantação da Web separado junto com o projeto de aplicativo Web que, quando compilado, compila explicitamente o aplicativo Web e copia os arquivos a serem implantados em um diretório de saída local. O projeto de aplicativo Web usa o MSBuild em segundo plano.

Por padrão, o arquivo de `Web.config` do ambiente de desenvolvimento é copiado para o diretório de saída, mas você pode configurar o projeto de implantação da Web para personalizar o

informações de configuração que são copiadas para esse diretório das seguintes maneiras:

- Por meio da substituição da seção arquivo `Web.config`, na qual você especifica a seção a ser substituída e um arquivo XML que contém o texto de substituição.
- Fornecendo um caminho para um arquivo de origem de configuração externa. Com essa opção selecionada, o projeto de implantação da Web copia um arquivo de `Web.config` específico para o diretório de saída (em vez do arquivo de `Web.config` usado no ambiente de desenvolvimento).
- Adicionando regras personalizadas ao arquivo MSBuild usado pelo projeto de implantação da Web.

Para implantar o aplicativo Web, crie o projeto de implantação da Web e, em seguida, copie os arquivos da pasta de saída do projeto para o ambiente de produção.

Para saber mais sobre como usar o projeto de implantação da Web, confira [Este artigo](https://msdn.microsoft.com/magazine/cc163448.aspx) sobre os projetos de implantação da Web da edição de abril de 2007 da [msdn Magazine](https://msdn.microsoft.com/magazine/default.aspx)ou consulte os links na seção leitura adicional no final deste tutorial.

> [!NOTE]
> Você não pode usar o projeto de implantação da Web com o Visual Web Developer porque o projeto de implantação da Web é implementado como um suplemento do Visual Studio e as edições de Visual Studio Express (incluindo o Visual Web Developer) não dão suporte a suplementos.

## <a name="summary"></a>Resumo

Os recursos externos e o comportamento de um aplicativo Web em desenvolvimento normalmente são diferentes de quando o mesmo aplicativo está em produção. Por exemplo, cadeias de conexão de banco de dados, opções de compilação e o comportamento quando uma exceção não tratada ocorre normalmente difere entre ambientes. O processo de implantação deve acomodar essas diferenças. Como discutimos neste tutorial, a abordagem mais simples é copiar manualmente um arquivo de configuração alternativo para o ambiente de produção. Soluções mais elegantes são possíveis ao usar o suplemento do projeto de implantação da Web ou com um processo de compilação ou implantação mais formalizado que pode acomodar essas personalizações.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Cadeias de conexão explicadas](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string)
- [Cadeias de conexão do banco de dados @ ConnectionStrings.com](http://www.connectionstrings.com/)
- [Não executar aplicativos ASP.NET de produção com `debug="true"` habilitado](https://weblogs.asp.net/scottgu/Don_1920_t-run-production-ASP.NET-Applications-with-debug_3D001D20_true_1D20_-enabled)
- [Respondendo normalmente a exceções sem tratamento – exibindo páginas de erro amigáveis](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [Como faço para: usar um projeto de implantação da Web do Visual Studio 2008?](../../../videos/how-do-i/how-do-i-use-a-visual-studio-2008-web-deployment-project.md)
- [Definições de configuração de chave ao implantar um banco de dados](http://aspnet.4guysfromrolla.com/articles/121008-1.aspx)
- Download de [projetos de implantação da Web do visual studio 2008](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) | download de [projetos de implantação da Web do Visual Studio 2005](https://download.microsoft.com/download/9/4/9/9496adc4-574e-4043-bb70-bc841e27f13c/WebDeploymentSetup.msi)
- [Projetos de implantação na Web do vs 2008](https://weblogs.asp.net/scottgu/archive/2005/11/06/429723.aspx) | [suporte ao projeto de implantação da web do vs 2008 lançado](https://weblogs.asp.net/scottgu/archive/2008/01/28/vs-2008-web-deployment-project-support-released.aspx)
- [Projetos de Implantação da Web](https://msdn.microsoft.com/magazine/cc163448.aspx)

> [!div class="step-by-step"]
> [Anterior](deploying-your-site-using-visual-studio-cs.md)
> [Próximo](core-differences-between-iis-and-the-asp-net-development-server-cs.md)
