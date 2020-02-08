---
uid: single-page-application/overview/templates/hottowel-template
title: Modelo de toalha quente | Microsoft Docs
author: madskristensen
description: Modelo HotTowel
ms.author: riande
ms.date: 02/09/2013
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: eeab69e75546791978bb09d7823d95caf9dca1a0
ms.sourcegitcommit: e365196c75ce93cd8967412b1cfdc27121816110
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77075054"
---
# <a name="hot-towel-template"></a>Modelo Hot Towel

por [Mads Kristensen](https://github.com/madskristensen)

> O modelo do Hot toalha MVC é escrito por John Papa
> 
> Escolha qual versão baixar:
> 
> [Modelo do Hot toalha MVC para Visual Studio 2012](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [Modelo do Hot toalha MVC para Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> Toalha quente: porque você não deseja ir para o SPA sem um!

Deseja criar um SPA, mas não pode decidir onde começar? Use a toalha quente e, em segundos, você terá um SPA e todas as ferramentas de que precisa para se basear!

A toalha quente cria um ótimo ponto de partida para a criação de um aplicativo de página única (SPA) com ASP.NET. Você fornece uma estrutura modular para seu código, exibição de navegação, vinculação de dados, gerenciamento de dados avançado e estilo simples, mas elegante. A toalha quente fornece tudo o que você precisa para criar um SPA, para que você possa se concentrar em seu aplicativo, não no encanamento.

> Saiba mais sobre como criar um SPA nos [vídeos, tutoriais e cursos da Pluralsight do John Papa](http://johnpapa.net/spa?vsix).

## <a name="application-structure"></a>Estrutura de Aplicativos

O SPA quente fornece uma pasta de aplicativo que contém os arquivos JavaScript e HTML que definem seu aplicativo.

Dentro da pasta do aplicativo:

- Durandal
- services
- ViewModels
- Modos de exibição

A pasta do aplicativo contém uma coleção de módulos. Esses módulos encapsulam a funcionalidade e declaram dependências em outros módulos. A pasta views contém o HTML para seu aplicativo e a pasta ViewModels contém a lógica de apresentação para as exibições (um padrão MVVM comum). A pasta serviços é ideal para hospedar qualquer serviço comum que seu aplicativo possa precisar, como a recuperação de dados HTTP ou a interação de armazenamento local. É comum que vários ViewModels usem novamente o código dos módulos de serviço.

## <a name="aspnet-mvc-server-side-application-structure"></a>Estrutura de aplicativo do lado do servidor ASP.NET MVC

A toalha quente se baseia na estrutura MVC familiar e poderosa do ASP.NET.

- Início do\_do aplicativo
- Conteúdo
- Controladores
- Modelos
- Scripts
- Exibições

## <a name="featured-libraries"></a>Bibliotecas em destaque

- ASP.NET MVC
- ASP.NET Web API
- Otimização da Web do ASP.NET-agrupamento e minificação
- [Breeze. js](http://Breezejs.com) -gerenciamento de dados avançado
- [Durandal. js](http://Durandaljs.com) -composição de navegação e exibição
- [Knockout. js](http://Knockoutjs.com) -associações de dados
- [Requires. js](http://requirejs.org) -modularidade com AMD e otimização
- [Torradeira. js](http://jpapa.me/c7toastr) -mensagens pop-up
- [Bootstrap do Twitter](https://twitter.github.com/bootstrap/) – estilo de CSS robusto

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a>Instalando por meio do modelo de SPA do Visual Studio 2012 quente

A toalha quente pode ser instalada como um modelo do Visual Studio 2012. Basta clicar `File` | `New Project` e escolher `ASP.NET MVC 4 Web Application`. Em seguida, selecione o modelo "aplicativo de página única de toalha quente" e execute!

## <a name="installing-via-the-nuget-package"></a>Instalando por meio do pacote NuGet

A toalha quente também é um pacote NuGet que aumenta um projeto do MVC ASP.NET vazio existente. Basta instalar usando o NuGet e, em seguida, executar!

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a>Como faço para criar na toalha quente?

Basta começar a adicionar código!

1. Adicione seu próprio código do lado do servidor, preferencialmente Entity Framework e WebAPI (que realmente brilha com o Breeze. js)
2. Adicionar exibições à pasta `App/views`
3. Adicionar ViewModels à pasta `App/viewmodels`
4. Adicionar associações de dados HTML e Knockout às suas novas exibições
5. Atualizar as rotas de navegação no `shell.js`

## <a name="walkthrough-of-the-htmljavascript"></a>Walkthrough. HTML/JavaScript

### <a name="viewshottowelindexcshtml"></a>Views/HotTowel/index.cshtml

index. cshtml é a rota inicial e a exibição para o aplicativo MVC. Ele contém todas as marcas meta padrão, Links CSS e Referências JavaScript que você esperaria. O corpo contém um único `<div>` que é onde todo o conteúdo (suas exibições) será colocado quando forem solicitados. O `@Scripts.Render` usa o require. js para executar o ponto de entrada para o código do aplicativo, que está contido no arquivo de `main.js`. Uma tela inicial é fornecida para demonstrar como criar uma tela inicial enquanto seu aplicativo é carregado.

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a>App/Main. js

O arquivo de `main.js` contém o código que será executado assim que seu aplicativo for carregado. É aqui que você deseja definir suas rotas de navegação, definir suas exibições de inicialização e executar qualquer configuração/inicialização, como estender os dados do seu aplicativo.

O arquivo `main.js` define vários módulos do Durandal para ajudar a iniciar o início do aplicativo. A instrução define ajuda a resolver as dependências dos módulos para que fiquem disponíveis para a função. Primeiro, as mensagens de depuração são habilitadas, que enviam mensagens sobre quais eventos o aplicativo está executando na janela do console. O código app. Start diz ao Durandal Framework para iniciar o aplicativo. As convenções são definidas para que Durandal saiba que todos os modos de exibição e ViewModels estão contidos nas mesmas pastas nomeadas, respectivamente. Por fim, o `app.setRoot` aciona o `shell` usando uma animação de `entrance` predefinida.

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a>Exibições

As exibições são encontradas na pasta `App/views`.

### <a name="shellhtml"></a>shell.html

O `shell.html` contém o layout mestre para seu HTML. Todas as outras exibições serão compostas em algum lugar do seu `shell` exibição. A toalha quente fornece uma `shell` com três regiões: um cabeçalho, uma área de conteúdo e um rodapé. Cada uma dessas regiões é carregada com o conteúdo do formulário outras exibições quando solicitado.

As associações de `compose` para o cabeçalho e o rodapé são embutidas em código na toalha quente para apontar para as exibições de `nav` e `footer`, respectivamente. A associação de composição para a seção `#content` está associada ao item ativo do módulo `router`. Em outras palavras, quando você clica em um link de navegação, o modo de exibição correspondente é carregado nessa área.

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a>nav.html

O `nav.html` contém os links de navegação para o SPA. É aí que a estrutura de menu pode ser colocada, por exemplo. Geralmente, isso é associado a dados (usando o Knockout) ao módulo `router` para exibir a navegação que você definiu na `shell.js`. O Knockout procura os atributos de vinculação de dados e associa-os à `shell` ViewModel para exibir as rotas de navegação e mostrar um ProgressBar (usando a inicialização do Twitter) se o módulo `router` estiver no meio da navegação de uma exibição para outra (consulte `router.isNavigating`).

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a>Home. html e details. html

Essas exibições contêm HTML para exibições personalizadas. Quando o link de `home` no menu do `nav` exibição é clicado, o `home` exibição será colocado na área de conteúdo da exibição de `shell`. Essas exibições podem ser aumentadas ou substituídas por suas próprias exibições personalizadas.

### <a name="footerhtml"></a>footer.html

O `footer.html` contém HTML que aparece no rodapé, na parte inferior da exibição `shell`.

## <a name="viewmodels"></a>ViewModels

ViewModels são encontrados na pasta `App/viewmodels`.

### <a name="shelljs"></a>Shell. js

O `shell` ViewModel contém propriedades e funções que estão associadas à exibição `shell`. Geralmente, é aqui que as associações de navegação de menu são encontradas (consulte a lógica de `router.mapNav`).

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a>Home. js e details. js

Esses ViewModels contêm as propriedades e funções que estão associadas à exibição de `home`. Ele também contém a lógica de apresentação da exibição e é a cola entre os dados e a exibição.

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a>Serviços

Os serviços são encontrados na pasta app/Services. O ideal é que seus serviços futuros, como um módulo DataService, que sejam responsáveis por obter e postar dados remotos, possam ser colocados.

### <a name="loggerjs"></a>logger.js

A toalha quente fornece um módulo `logger` na pasta serviços. O módulo `logger` é ideal para registrar mensagens no console e para o usuário em exibir notificações.
