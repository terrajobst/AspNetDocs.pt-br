---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
title: Introdução | Microsoft Docs
author: Rick-Anderson
description: O WebMatrix não é mais recomendado como um ambiente de desenvolvimento integrado para Páginas da Web do ASP.NET. Use o Visual Studio ou o Visual Studio Code. Este guia é um...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: a36d3bdf-ef1b-47a4-b932-3a0cf4cad716
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
msc.type: authoredcontent
ms.openlocfilehash: bb863f8605e6f8faca3b285607b63a3e88e83012
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78547099"
---
# <a name="getting-started"></a>Introdução

por [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE[](~/includes/rp.md)]

> > [!NOTE] 
> > 
> > O WebMatrix não é mais recomendado como um ambiente de desenvolvimento integrado para Páginas da Web do ASP.NET. Use o [Visual Studio](../program-asp-net-web-pages-in-visual-studio.md) ou o [Visual Studio Code](https://code.visualstudio.com/).
> 
> 
> Este guia e aplicativo fornece uma visão geral do Páginas da Web do ASP.NET (versão 2 ou posterior) e sintaxe Razor, uma estrutura leve para a criação de sites dinâmicos. Ele também apresenta o WebMatrix, uma ferramenta para a criação de páginas e sites.
> 
> **Nível**: novo no páginas da Web do ASP.net.  
> **Habilidades presumidas**: HTML, CSS (folhas de estilo em cascata) básicas.
> 
> O que você aprenderá no primeiro tutorial do conjunto:
> 
> - Qual é a tecnologia Páginas da Web do ASP.NET e para que ela se trata.
> - O que é o WebMatrix.
> - Como instalar tudo.
> - Como criar um site usando o WebMatrix.
>   
> 
> Recursos/tecnologias abordados:
> 
> - Microsoft Web Platform Installer.
> - WebMatrix.
> - páginas *. cshtml*
>   
> 
> Mike Papa escreveu originalmente este tutorial. Tom FitzMacken o atualizou para o Microsoft WebMatrix 3.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 2
> - WebMatrix 3

## <a name="what-should-you-know"></a>O que você deve saber?

Estamos supondo que você esteja familiarizado com:

- **HTML**. Não é necessário nenhum conhecimento aprofundado. Não explicaremos HTML, mas também não usamos nada de complexidade. Forneceremos links para tutoriais em HTML onde achamos que são úteis.
- **CSS (folhas de estilos em cascata)** . O mesmo que com HTML.
- **Ideias básicas do banco de dados**. Se você usou uma planilha para dados e classificou e filtrou os dados, esse é o nível de experiência que geralmente estamos supondo para este conjunto de tutorial.

Também estamos supondo que você esteja interessado em aprender sobre a programação básica. Páginas da Web do ASP.NET usar uma linguagem de programação C#chamada. Você não precisa ter nenhum plano de fundo em programação, apenas um interesse. Se você já escreveu algum JavaScript em uma página da Web antes, você tem muita experiência em segundo plano.

Observe que, se você estiver familiarizado com a programação, poderá descobrir que este conjunto de tutorial se move mais lentamente, enquanto colocamos novos programadores em velocidade. No entanto, à medida que passamos pelos primeiros tutoriais, haverá menos programação básica para explicar e as coisas serão movidas para um clipe mais rápido.

## <a name="what-do-you-need"></a>O que você precisa?

Você precisará de:

- Um computador que esteja executando o Windows 8, o Windows 7, o Windows Server 2008 ou o Windows Server 2012.
- Uma conexão com A Internet ativa.
- Privilégios de administrador (necessários para o processo de instalação).

## <a name="what-is-aspnet-web-pages"></a>O que é Páginas da Web do ASP.NET?

Páginas da Web do ASP.NET é uma estrutura que você pode usar para criar páginas da Web dinâmicas. Uma página da Web HTML simples é estática; seu conteúdo é determinado pela marcação HTML fixa que está na página. Páginas dinâmicas como as que você cria com Páginas da Web do ASP.NET permitem criar o conteúdo da página imediatamente, usando o código.

As páginas dinâmicas permitem que você faça todos os tipos de coisas. Você pode solicitar a entrada de um usuário usando um formulário e, em seguida, alterar o que a página exibe ou sua aparência. Você pode obter informações de um usuário, salvá-los em um banco de dados e, em seguida, listá-los mais tarde. Você pode enviar emails do seu site. Você pode interagir com outros serviços na Web (por exemplo, um serviço de mapeamento) e produzir páginas que integram informações dessas fontes.

## <a name="what-is-webmatrix"></a>O que é o WebMatrix?

O WebMatrix é uma ferramenta que integra um editor de página da Web, um utilitário de banco de dados, um servidor Web para testar páginas e recursos para publicar seu site na Internet. O WebMatrix é gratuito e é fácil de ser instalado e fácil de usar. (Ele também funciona apenas em páginas HTML simples, bem como para outras tecnologias como PHP.)

Na verdade, você não *precisa* usar o WebMatrix para trabalhar com páginas da Web do ASP.net. Você pode criar páginas usando um editor de texto, por exemplo, e páginas de teste usando um servidor Web ao qual você tem acesso. No entanto, o WebMatrix torna tudo muito fácil, portanto, esses tutoriais usarão o WebMatrix em todo o uso.

## <a name="about-these-tutorials"></a>Sobre estes tutoriais

Este conjunto de tutorial é uma introdução a como usar Páginas da Web do ASP.NET. Há 9 tutoriais no total deste conjunto de tutorial introdutório. Ele faz parte de uma série de conjuntos de tutoriais que o leva de Páginas da Web do ASP.NET iniciante a criar sites reais e de aparência profissional.

Este primeiro conjunto de tutoriais se concentra em mostrar as noções básicas de como trabalhar com Páginas da Web do ASP.NET. Quando terminar, você poderá trabalhar com conjuntos de tutorial adicionais que aparecem onde este termina e que exploram páginas da Web com mais profundidade.

Nós acabamos facilitando as explicações aprofundadas. E sempre que mostrarmos algo, para este conjunto de tutorial, sempre escolhemos a maneira que achamos mais fácil de entender. Os conjuntos de tutorial posteriores entram em mais detalhes e mostram abordagens mais eficientes ou mais flexíveis (também mais divertidas). Mas esses tutoriais exigem que você entenda os conceitos básicos primeiro.

O conjunto de tutorial que você acabou de começar abrange esses recursos do Páginas da Web do ASP.NET:

- Introdução e obtenção de tudo instalado. (Isso está no tutorial que você está lendo.)
- Noções básicas de programação de Páginas da Web do ASP.NET.
- Criando um banco de dados.
- Criando e processando um formulário de entrada do usuário.
- Adicionar, atualizar e excluir dados no banco de dado.

## <a name="what-will-you-create"></a>O que você criará?

Este conjunto de tutoriais e os próximos giram em um site onde você pode listar os filmes que desejar. Você poderá inserir filmes, editá-los e listá-los. Aqui estão algumas páginas que você criará neste tutorial set. A primeira mostra a página de listagem de filmes que você criará:

![Aplicativo de filme do terminou mostrando uma listagem de filmes](getting-started/_static/image1.png)

E aqui está a página que permite que você adicione novas informações de filme ao seu site:

![Aplicativo de filme concluído mostrando a página Adicionar filme](getting-started/_static/image2.png)

Os conjuntos de tutorial subsequentes se baseiam nesse conjunto e adicionam mais funcionalidade, como carregar imagens, permitir que as pessoas façam logon, enviem email e integrando com mídia social.

## <a name="see-this-app-running-on-azure"></a>Consulte este aplicativo em execução no Azure

Você gostaria de ver o site concluído em execução como um aplicativo Web ativo? Você pode implantar uma versão completa do aplicativo em sua conta do Azure simplesmente clicando no botão a seguir.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebPagesMovies)

Você precisa de uma conta do Azure para implantar essa solução no Azure. Se você ainda não tiver uma conta, terá as seguintes opções:

- [Abra uma conta do Azure gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -você obtém créditos que podem ser usados para experimentar serviços pagos do Azure e, mesmo depois que eles são usados, você pode manter a conta e usar os serviços gratuitos do Azure.
- [Ativar os benefícios do assinante do MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -sua assinatura do MSDN fornece créditos todos os meses que você pode usar para serviços pagos do Azure.

## <a name="installing-everything"></a>Instalando tudo

Você pode instalar tudo usando o Web Platform Installer da Microsoft. Na verdade, você instala o instalador e o usa para instalar todo o resto.

Para usar páginas da Web, você precisa ter pelo menos o Windows XP com SP3 instalado ou o Windows Server 2008 ou posterior.

Na [página páginas da Web](../../../index.md) do site do ASP.net, clique em **instalar**.

![ASP.NET Web site mostrando &quot;botão instalar&quot; do WebMatrix](getting-started/_static/image3.png)

Você será solicitado a aceitar os termos de licença e a política de privacidade antes de instalar o WebMatrix.

![aceitar o termo para iniciar a instalação](getting-started/_static/image4.png)

Clique em **executar** para iniciar a instalação. (Se você quiser salvar o instalador, clique em **salvar** e, em seguida, execute o instalador da pasta onde você o salvou.)

![](getting-started/_static/image5.png)

O Web Platform Installer é exibido, pronto para instalar o WebMatrix. Clique em **Instalar**.

![](getting-started/_static/image6.png)

O processo de instalação descobre o que deve ser instalado no computador e inicia o processo. Dependendo do que exatamente precisa ser instalado, o processo pode levar de alguns minutos a vários segundos. Selecione aceito para aceitar os **termos de licença** .

## <a name="hello-webmatrix"></a>Hello, WebMatrix

Quando terminar, o processo de instalação poderá iniciar o WebMatrix automaticamente. Se não estiver, no Windows, no menu **Iniciar** , inicie o **Microsoft WebMatrix**.

Ao iniciar o WebMatrix pela primeira vez, você terá a oportunidade de entrar no Microsoft Azure com seu conta Microsoft. Ao entrar, você receberá 10 aplicativos Web gratuitos por meio do Azure. Esses aplicativos Web gratuitos fornecem uma maneira conveniente de testar seus aplicativos. Se você ainda não tiver uma conta do Azure, mas tiver uma assinatura do MSDN, poderá [ativar os benefícios da assinatura do MSDN](https://www.windowsazure.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604). Caso contrário, você pode criar uma conta de avaliação gratuita em apenas alguns minutos. Para obter detalhes, consulte [Avaliação gratuita do Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

Você não precisa entrar no momento para continuar com este tutorial. Se você não entrar agora, ainda terá a opção de entrar mais tarde. O último [tópico](publishing.md) desta série de tutoriais aborda como implantar seu site no Azure; Portanto, você precisará entrar para concluir esse tópico.

Neste ponto, entre com seu conta Microsoft ou selecione **não agora** no canto inferior direito.

![Entrar](getting-started/_static/image7.png)

Para começar, você criará um site em branco e adicionará uma página. Em um tutorial posterior neste conjunto, você vai brincar com um dos modelos de site internos.

Na janela iniciar, clique em **novo**.

![Tela de inicialização do WebMatrix](getting-started/_static/image8.png)

Os modelos são arquivos e páginas pré-criados para diferentes tipos de sites. Para ver todos os modelos que estão disponíveis por padrão, selecione a opção Galeria de modelos.

![Selecionar Galeria de modelos](getting-started/_static/image9.png)

Na janela **início rápido** , selecione **site vazio** no grupo **ASP.net** e nomeie o novo site "WebPagesMovies".

![Janela do WebMatrix Início Rápido com o modelo de site vazio selecionado](getting-started/_static/image10.png)

Clique em **Próximo**.

Se você tiver entrado no seu conta Microsoft, terá a oportunidade de criar o site no Azure. Com base no nome do seu site, o nome padrão de **WebPagesMovies.azurewebsites.net** é sugerido; no entanto, o ponto de exclamação indica que esse nome não está disponível no Windows Azure. Para simplificar, selecione **ignorar** para ignorar a criação do site no Azure no momento. Posteriormente nesta série, publicaremos o site no Azure.

![criar site do Azure](getting-started/_static/image11.png)

O WebMatrix cria e abre o site:

![Novo site do WebPagesMovies aberto no WebMatrix](getting-started/_static/image12.png)

Na parte superior, há uma barra de ferramentas de acesso rápido e uma faixa de faixas. Na parte inferior esquerda, você vê o seletor de espaço de trabalho onde alterna entre tarefas (**site**, **arquivos**, **bancos de dados**, **relatórios**). À direita está o painel de conteúdo para o editor e para relatórios. E, na parte inferior, ocasionalmente você verá uma barra de notificação para mensagens.

Você aprenderá mais sobre o WebMatrix e seus recursos ao percorrer esses tutoriais.

## <a name="creating-a-web-page"></a>Criando uma página da Web

Para se familiarizar com o WebMatrix e o Páginas da Web do ASP.NET, você criará uma página simples.

No seletor de espaço de trabalho, selecione o espaço de trabalho **arquivos** . Esse espaço de trabalho permite que você trabalhe com arquivos e pastas. O painel esquerdo mostra a estrutura de arquivos do seu site. A faixa de faixas muda para mostrar tarefas relacionadas a arquivos.

![Espaço de trabalho de arquivo no WebMatrix](getting-started/_static/image13.png)

Na faixa de faixas, clique na seta em **novo** e, em seguida, clique em **novo arquivo**.

![Usando o &quot;novo comando&quot; na faixa de faixas para criar um novo arquivo](getting-started/_static/image14.png)

O WebMatrix exibe uma lista de tipos de arquivo. Selecione **cshtml**e, na caixa **nome** , digite "HelloWorld". Uma página CSHTML é uma página Páginas da Web do ASP.NET.

![Criando uma nova página CSHTML chamada HelloWorld. cshtml](getting-started/_static/image15.png)

Clique em **OK**.

O WebMatrix cria a página e a abre no editor.

![A nova página HelloWorld no editor do WebMatrix](getting-started/_static/image16.png)

Como você pode ver, a página contém a marcação HTML comum, com exceção de um bloco na parte superior que tem esta aparência:

[!code-cshtml[Main](getting-started/samples/sample1.cshtml)]

Isso é para adicionar código, como você verá em breve.

Observe que as diferentes partes da página &mdash; os nomes de elemento, os atributos e o texto, além do bloco na parte superior — estão todas em cores diferentes. Isso é chamado de *realce de sintaxe*e torna mais fácil manter tudo claro. É um dos recursos que torna mais fácil trabalhar com páginas da Web no WebMatrix.

Adicione conteúdo para os elementos `<head>` e `<body>` como no exemplo a seguir. (Se desejar, basta copiar o bloco a seguir e substituir toda a página existente por esse código.)

[!code-cshtml[Main](getting-started/samples/sample2.cshtml)]

Na barra de ferramentas acesso rápido ou no menu **arquivo** , clique em **salvar**.

![Botão salvar na barra de ferramentas de acesso rápido do WebMatrix](getting-started/_static/image17.png)

## <a name="testing-the-page"></a>Testando a página

No espaço de trabalho **arquivos** , clique com o botão direito do mouse na página *HelloWorld. cshtml* e clique em **Iniciar no navegador**.

![Executando uma página usando o botão Executar na faixa de faixas do WebMatrix](getting-started/_static/image18.png)

O WebMatrix inicia um servidor Web interno (IIS Express) que você pode usar para testar páginas em seu computador. (Sem IIS Express no WebMatrix, você precisaria publicar sua página em um servidor Web em algum lugar antes de poder testá-la.) A página é exibida no navegador padrão.

![&quot;Olá, Mundo página&quot; em execução no navegador](getting-started/_static/image19.png)

Observe que, quando você testa uma página no WebMatrix, a URL no navegador é algo como `http://localhost:33651/HelloWorld.cshtml.` o nome *localhost* refere-se a um servidor local, o que significa que a página é servida por um servidor Web que está no seu próprio computador. Como observado, o WebMatrix inclui um programa de servidor Web chamado IIS Express que é executado quando você inicia uma página.

O número após *localhost* (por exemplo, *localhost: 33651*) refere-se a um *número de porta* em seu computador. Este é o número do "canal" que o IIS Express usa para esse site específico. O número da porta é selecionado aleatoriamente do intervalo de 1024 a 65536 quando você cria seu site e é diferente para cada site que você criar. (Quando você testa seu próprio site, o número da porta certamente será um número diferente de 33561.) Ao usar uma porta diferente para cada site, IIS Express pode manter diretamente em qual dos seus sites ele está conversando.

Posteriormente, quando você publicar seu site em um servidor Web público, não verá mais *localhost* na URL. Nesse ponto, você verá uma URL mais típica, como `http://myhostingsite/mywebsite/HelloWorld.cshtml` ou qualquer que seja a página. Você aprenderá mais sobre a publicação de um site mais adiante nesta série de tutoriais.

## <a name="adding-some-server-side-code"></a>Adicionando um código do servidor

Feche o navegador e volte para a página no WebMatrix.

Adicione uma linha ao bloco de código para que seja semelhante ao seguinte código:

[!code-cshtml[Main](getting-started/samples/sample3.cshtml)]

Esse é um pouco de código do Razor. Provavelmente, é claro que ele obtém a data e a hora atuais e coloca esse valor em uma *variável* chamada `currentDateTime`. Você vai ler mais sobre sintaxe Razor no próximo tutorial.

No corpo da página, depois do elemento `<p>Hello World!</p>`, adicione o seguinte:

[!code-html[Main](getting-started/samples/sample4.html)]

Esse código obtém o valor que você coloca na variável `currentDateTime` na parte superior e a insere na marcação da página. O caractere `@` marca o código de Páginas da Web do ASP.NET na página.

Execute a página novamente (o WebMatrix salva as alterações para você antes de executar a página). Desta vez, você verá a data e a hora na página.

![&quot;Olá, Mundo página&quot; em execução no navegador com uma exibição de hora gerada dinamicamente](getting-started/_static/image20.png)

Aguarde alguns instantes e, em seguida, atualize a página no navegador. A exibição de data e hora é atualizada.

No navegador, examine a origem da página. Ele é semelhante à seguinte marcação:

[!code-html[Main](getting-started/samples/sample5.html)]

Observe que o bloco de `@{ }` na parte superior não está lá. Observe também que a exibição de data e hora mostra uma cadeia de caracteres real (`1/18/2012 2:49:50 PM` ou qualquer que), não `@currentDateTime` como você tinha na página *. cshtml* . O que aconteceu aqui é que, quando você executou a página, ASP.NET processou todo o código (muito pouco nesse caso) que foi marcado com `@`. O código produz a saída e essa saída foi inserida na página.

## <a name="this-is-what-aspnet-web-pages-are-about"></a>É isso que Páginas da Web do ASP.NET

Quando você lê que Páginas da Web do ASP.NET produz conteúdo da Web dinâmico, o que você viu aqui é a ideia. A página que você acabou de criar contém a mesma marcação HTML que você viu anteriormente. Ele também pode conter código que pode executar todos os tipos de tarefas. Neste exemplo, ele fez a tarefa trivial de obter a data e a hora atuais. Como vimos, você pode intercalar o código com o HTML para produzir a saída na página. Quando alguém solicita uma página *. cshtml* no navegador, o ASP.net processa a página enquanto ela ainda está nas mãos do servidor Web. ASP.NET insere a saída do código (se houver) na página como HTML. Quando o processamento do código é feito, o ASP.NET envia a página resultante para o navegador. Todo o navegador já recebe o HTML. Aqui está um diagrama:

![Fluxo conceitual de como o ASP.NET gera HTML dinamicamente](getting-started/_static/image21.png)

A ideia é simples, mas há muitas tarefas interessantes que o código pode executar, e há muitas maneiras interessantes pelas quais você pode adicionar dinamicamente conteúdo HTML à página. As páginas ASP.NET *. cshtml* , como qualquer página HTML, também podem incluir o código que é executado no próprio navegador (código JavaScript e jQuery). Você explorará todas essas coisas neste conjunto de tutoriais e nas subsequentes.

## <a name="coming-up-next"></a>Chegando em seguida

No próximo tutorial desta série, você explora Páginas da Web do ASP.NET programação um pouco mais.

## <a name="additional-resources"></a>Recursos adicionais

[Crie um site do ASP.net do zero](https://www.microsoft.com/web/post/create-an-aspnet-website-from-scratch). Este é um tutorial que é especificamente sobre o uso do WebMatrix (não Páginas da Web do ASP.NET). Ele entra em um pouco mais de detalhes sobre alguns dos recursos adicionais do WebMatrix que não abordaremos neste conjunto de tutoriais.

> [!div class="step-by-step"]
> [Próximo](intro-to-web-pages-programming.md)
