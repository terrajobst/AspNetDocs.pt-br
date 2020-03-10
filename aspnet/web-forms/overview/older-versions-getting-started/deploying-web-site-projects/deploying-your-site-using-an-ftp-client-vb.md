---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-vb
title: Implantando seu site usando um cliente FTP (VB) | Microsoft Docs
author: rick-anderson
description: A maneira mais simples de implantar um aplicativo ASP.NET é copiar manualmente os arquivos necessários do ambiente de desenvolvimento para o ambiente de produção. Thi...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: 09279194-bcf9-4b59-a09d-c68e5926a758
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-vb
msc.type: authoredcontent
ms.openlocfilehash: 7875304c672625d8c0eaaf0fea8ef509bb801a3a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78545503"
---
# <a name="deploying-your-site-using-an-ftp-client-vb"></a>Implantação do site com o uso de um cliente de FTP (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_03_VB.zip) ou [baixar PDF](https://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial03_DeployingViaFTP_vb.pdf)

> A maneira mais simples de implantar um aplicativo ASP.NET é copiar manualmente os arquivos necessários do ambiente de desenvolvimento para o ambiente de produção. Este tutorial mostra como usar um cliente FTP para obter os arquivos de sua área de trabalho para o provedor de host da Web.

## <a name="introduction"></a>Introdução

O tutorial anterior introduziu um livro simples revisar ASP.NET aplicativo Web, que é composto de algumas páginas ASP.NET, uma página mestra, uma classe de `Page` de base personalizada, várias imagens e três folhas de estilo CSS. Agora estamos prontos para implantar esse aplicativo em um provedor de host da Web, ponto em que o aplicativo estará acessível a qualquer pessoa com uma conexão com a Internet!

De nossas discussões no tutorial [*determinando quais arquivos precisam ser implantados*](determining-what-files-need-to-be-deployed-vb.md) , sabemos quais arquivos precisam ser copiados para o provedor de host da Web. (Lembre-se de que os arquivos que são copiados dependem de seu aplicativo ser compilado de forma explícita ou automática.) Mas como obtemos os arquivos do ambiente de desenvolvimento (nossa área de trabalho) até o ambiente de produção (o servidor Web gerenciado pelo provedor de host da Web)? O [ **F** Ile **T** ransfer **P** rotocol (FTP)](http://en.wikipedia.org/wiki/File_Transfer_Protocol) é um protocolo usado comumente para copiar arquivos de um computador para outro em uma rede. Outra opção é o FPSE (extensões de servidor do FrontPage). Este tutorial se concentra em usar o software cliente de FTP autônomo para implantar os arquivos necessários do ambiente de desenvolvimento para o ambiente de produção.

> [!NOTE]
> O Visual Studio inclui ferramentas para publicar sites via FTP; essas ferramentas, bem como uma visão das ferramentas que usam o FPSE, são abordadas no próximo tutorial.

Para copiar os arquivos usando FTP, precisamos de um *cliente FTP* no ambiente de desenvolvimento. Um cliente FTP é um aplicativo projetado para copiar arquivos do computador instalado em um computador que esteja executando um *servidor FTP*. (Se o seu provedor de host da Web oferecer suporte a transferências de arquivos via FTP, como a maioria, há um servidor FTP em execução em seus servidores Web.) Há vários aplicativos cliente FTP disponíveis. Seu navegador da Web pode até mesmo dobrar como um cliente FTP. Meu cliente FTP favorito e o que usarei para este tutorial é o [FileZilla](http://filezilla-project.org/), um cliente de FTP gratuito e de software livre que está disponível para Windows, Linux e Macs. No entanto, qualquer cliente FTP funcionará, portanto, fique à vontade para usar qualquer cliente com o qual você esteja mais familiarizado.

Se você estiver acompanhando, precisará criar uma conta com um provedor de host da Web para poder concluir este tutorial ou os subsequentes. Conforme observado no tutorial anterior, há uma gaggle de empresas do provedor de host Web com um amplo espectro de preços, recursos e qualidade de serviço. Para esta série de tutoriais, usarei o [desconto ASP.net](http://discountasp.net) como meu provedor de host da Web, mas você pode acompanhar com qualquer provedor de host da Web, desde que ele ofereça suporte à versão ASP.net em que seu site é desenvolvido. (Esses tutoriais foram criados usando o ASP.NET 3,5.) Além disso, como iremos copiar os arquivos para o provedor de host da Web usando FTP neste tutorial e, no futuro, é imperativo que o provedor de host da Web dê suporte ao acesso de FTP para seus servidores Web. Praticamente todos os provedores de host da Web oferecem esse recurso, mas você deve fazer uma verificação dupla antes de se inscrever.

## <a name="deploying-the-book-review-web-application-project"></a>Implantando o livro analisar o projeto de aplicativo Web

Lembre-se de que há duas versões do livro examinar aplicativo Web: uma implementada usando o BookReviewsWAP (modelo de projeto de aplicativo Web) e a outra usando o BookReviewsWSP (modelo de projeto de site da Web). O tipo de projeto influencia se o site é compilado automaticamente ou explicitamente e se o modelo de compilação determina quais arquivos precisam ser implantados. Consequentemente, examinaremos a implantação dos projetos BookReviewsWAP e BookReviewsWSP separadamente, começando com o BookReviewsWAP. Reserve um tempo para baixar esses dois aplicativos ASP.NET, se você ainda não tiver feito isso.

Inicie o projeto BookReviewsWAP navegando até a pasta `BookReviewsWAP` e clicando duas vezes no arquivo `BookReviewsWAP.sln`. Antes de implantar o projeto, é importante compilá-lo para garantir que todas as alterações no código-fonte sejam incluídas no assembly compilado. Para criar o projeto, vá para o menu Compilar e escolha a opção de menu criar BookReviewsWAP. Isso compila o código-fonte no projeto em um único assembly, `BookReviewsWAP.dll`, que é colocado na pasta `Bin`.

Agora estamos prontos para implantar os arquivos necessários! Inicie o cliente FTP e conecte-se ao servidor Web no provedor de host da Web. (Quando você se inscrever com uma empresa de hospedagem na Web, ele enviará informações por email sobre como se conectar ao servidor FTP; isso inclui o endereço para o servidor FTP, bem como um nome de usuário e senha.)

Copie os seguintes arquivos de sua área de trabalho para a pasta raiz do site em seu provedor de host da Web. Quando você é o FTP no servidor Web no provedor de host da Web, provavelmente você está no diretório raiz do site. No entanto, alguns provedores de host da Web têm uma subpasta chamada `www` ou `wwwroot` que serve como a pasta raiz para seus arquivos de site. Por fim, quando FTPing os arquivos, talvez seja necessário criar a estrutura de pastas correspondente no ambiente de produção – a pasta `Bin`, a pasta `Fiction`, a pasta `Images` e assim por diante.

- `~/Default.aspx`
- `~/About.aspx`
- `~/Site.master`
- `~/Web.config`
- `~/Web.sitemap`
- O conteúdo completo da pasta `Styles`
- O conteúdo completo da pasta `Images` (e sua subpasta, `BookCovers`)
- `~/Fiction/Default.aspx`
- `~/Fiction/Blaze.aspx`
- `~/Tech/Default.aspx`
- `~/Tech/CYOW.aspx`
- `~/Tech/TYASP35.aspx`
- `~/Bin/BookReviewsWAP.dll`

A Figura 1 mostra o FileZilla depois que os arquivos necessários foram copiados. O FileZilla exibe os arquivos no computador local à esquerda e os arquivos no computador remoto à direita. Como mostra a Figura 1, os arquivos de código-fonte ASP.NET, como `About.aspx.vb`, estão no computador local (ambiente de desenvolvimento), mas não foram copiados para o provedor de host da Web (o ambiente de produção), pois os arquivos de código não precisam ser implantados ao usar a compilação explícita.

> [!NOTE]
> Não há nenhum dano em ter os arquivos de código-fonte no servidor de produção, pois eles são ignorados. O ASP.NET proíbe solicitações HTTP para arquivos de código-fonte por padrão, de modo que mesmo que os arquivos de código-fonte estejam presentes no servidor de produção, eles ficam inacessíveis para os visitantes do seu site. (Ou seja, se um usuário tentar visitar `http://www.yoursite.com/Default.aspx.vb` ele receberá uma página de erro que explica que esses tipos de arquivos-`.vb`-são proibidos.)

[![usar um cliente FTP para copiar os arquivos necessários da área de trabalho para o servidor Web no provedor de host da Web.](deploying-your-site-using-an-ftp-client-vb/_static/image2.png)](deploying-your-site-using-an-ftp-client-vb/_static/image1.png)

**Figura 1**: usar um cliente FTP para copiar os arquivos necessários da área de trabalho para o servidor Web no provedor de host da Web ([clique para exibir a imagem em tamanho normal](deploying-your-site-using-an-ftp-client-vb/_static/image3.png))

Depois de implantar seu site, Reserve um momento para testar o site. Se você comprou um nome de domínio e definiu as configurações de DNS corretamente, você pode visitar seu site digitando seu nome de domínio. Como alternativa, seu provedor de host da Web deve ter fornecido uma URL para seu site, que será semelhante a *AccountName*. *webhostprovider*. com ou *webhostprovider*. com/*AccountName*. Por exemplo, a URL para minha conta no desconto ASP.NET é: `http://httpruntime.web703.discountasp.net`.

A Figura 2 mostra o site de revisões do livro implantado. Observe que estou exibindo no desconto ASP. Os servidores da rede, em `http://httpruntime.web703.discountasp.net`. Neste momento, qualquer pessoa com uma conexão com a Internet poderia exibir meu site! Como esperamos, o site tem a aparência e se comporta da mesma forma que fazia ao testá-lo no ambiente de desenvolvimento.

> [!NOTE]
> Se você receber um erro ao exibir seu aplicativo, Reserve um momento para garantir que você implantou o conjunto correto de arquivos. Em seguida, verifique a mensagem de erro para ver se ele revela quaisquer pistas sobre o problema. Depois disso, você pode mudar para o helpdesk da sua empresa host da Web ou postar sua pergunta no fórum apropriado nos [fóruns do ASP.net](https://forums.asp.net/).

[![o site de análises de livros agora está acessível a qualquer pessoa com uma conexão com a Internet.](deploying-your-site-using-an-ftp-client-vb/_static/image5.png)](deploying-your-site-using-an-ftp-client-vb/_static/image4.png)

**Figura 2**: o site de análises de livros agora está acessível a qualquer pessoa com uma conexão com a Internet ([clique para exibir a imagem em tamanho normal](deploying-your-site-using-an-ftp-client-vb/_static/image6.png))

## <a name="deploying-the-book-review-web-site-project"></a>Implantando o projeto de site da Web de análise de livros

Ao implantar um aplicativo ASP.NET que usa a compilação automática, como o projeto de site do BookReviewsWSP, não há nenhum assembly compilado na pasta `Bin`. Como resultado, os arquivos de código-fonte do aplicativo Web devem ser implantados no ambiente de produção. Vamos examinar esse processo.

Assim como no projeto de aplicativo Web, é prudente primeiro compilar o aplicativo antes de implantá-lo. Embora a criação de um projeto de site não crie um assembly, ele verifica se há erros de tempo de compilação na página. Melhor encontrar esses erros agora, em vez de ter um visitante para seu site, descubra-os para você!

Depois de criar o projeto com êxito, use o cliente FTP para copiar os seguintes arquivos para a pasta raiz do site em seu provedor de host da Web. Talvez seja necessário criar a estrutura de pastas correspondente no ambiente de produção.

> [!NOTE]
> Se você já implantou o projeto BookReviewsWAP, mas ainda deseja tentar implantar o projeto BookReviewsWSP, primeiro exclua todos os arquivos no servidor Web que foram carregados ao implantar BookReviewsWAP e, em seguida, implante os arquivos para BookReviewsWSP.

- `~/Default.aspx`
- `~/Default.aspx.vb`
- `~/About.aspx`
- `~/About.aspx.vb`
- `~/Site.master`
- `~/Site.master.vb`
- `~/Web.config`
- `~/Web.sitemap`
- O conteúdo completo da pasta `Styles`
- O conteúdo completo da pasta `Images` (e sua subpasta, `BookCovers`)
- `~/App_Code/BasePage.vb`
- `~/Fiction/Default.aspx`
- `~/Fiction/Default.aspx.vb`
- `~/Fiction/Blaze.aspx`
- `~/Fiction/Blaze.aspx.vb`
- `~/Tech/Default.aspx`
- `~/Tech/Default.aspx.vb`
- `~/Tech/CYOW.aspx`
- `~/Tech/CYOW.aspx.vb`
- `~/Tech/TYASP35.aspx`
- `~/Tech/TYASP35.aspx.vb`

A Figura 3 mostra o FileZilla depois de copiar os arquivos necessários. Como você pode ver, os arquivos de código-fonte do ASP.NET, como `About.aspx.vb`, estão presentes no computador local (o ambiente de desenvolvimento) e no provedor de host da Web (o ambiente de produção), pois os arquivos de código precisam ser implantados ao usar a compilação automática.

[![usar um cliente FTP para copiar os arquivos necessários da área de trabalho para o servidor Web no provedor de host da Web](deploying-your-site-using-an-ftp-client-vb/_static/image8.png)](deploying-your-site-using-an-ftp-client-vb/_static/image7.png)

**Figura 3**: usar um cliente FTP para copiar os arquivos necessários da área de trabalho para o servidor Web no provedor de host da Web ([clique para exibir a imagem em tamanho normal](deploying-your-site-using-an-ftp-client-vb/_static/image9.png))

A experiência do usuário não é afetada pelo modelo de compilação do aplicativo. As mesmas páginas ASP.NET podem ser acessadas e se comportam da mesma forma que o site foi criado usando o modelo de projeto de aplicativo Web ou o modelo de projeto de site da Web.

## <a name="updating-a-web-application-on-production"></a>Atualizando um aplicativo Web na produção

O desenvolvimento e a implantação de aplicativos Web não são um processo de uso único. Por exemplo, ao criar o site de revisão do livro, criei as várias páginas e escrevi o código que o acompanha em meu computador pessoal (o ambiente de desenvolvimento). Depois de atingir um certo estado estável, implantei meu aplicativo para que outras pessoas pudessem visitar o site e ler minhas revisões. Mas a implantação não marca o final do meu desenvolvimento neste site. Posso adicionar mais análises de livros ou implementar novos recursos, como permitir que meus visitantes avaliem livros ou deixem seus próprios comentários. Esses aprimoramentos seriam desenvolvidos no ambiente de desenvolvimento e, quando concluído, precisariam ser implantados. O desenvolvimento e a implantação, portanto, são cíclicos. Você desenvolve um aplicativo e o implanta. Embora o site esteja ativo e em produção, novos recursos são adicionados e os bugs são corrigidos ao longo do tempo, o que exige a reimplantação do aplicativo. E assim por diante.

Como você deve esperar, ao reimplantar um aplicativo Web, você só precisa copiar arquivos novos e alterados. Não é necessário reimplantar páginas inalteradas ou arquivos de suporte do servidor ou do lado do cliente (embora não haja nenhum dano ao fazer isso).

> [!NOTE]
> Uma coisa a ter em mente ao usar a compilação explícita é que sempre que você adiciona uma nova página ASP.NET ao projeto ou faz qualquer alteração relacionada ao código, você precisa recompilar seu projeto, o que atualiza o assembly na pasta `Bin`. Consequentemente, você precisará copiar esse assembly atualizado para produção ao atualizar um aplicativo Web em produção (juntamente com o outro conteúdo novo e atualizado).

Além disso, entenda que qualquer alteração no `Web.config` ou nos arquivos no diretório `Bin` para e reinicia o pool de aplicativos do site. Se o estado da sessão for armazenado usando o modo de `InProc` (o padrão), os visitantes do site perderão seu estado de sessão sempre que esses arquivos de chave forem modificados. Para evitar essa armadilha, considere armazenar a sessão usando os modos `StateServer` ou `SQLServer`. Para obter mais informações sobre este tópico [, leia modos de estado de sessão](https://msdn.microsoft.com/library/ms178586.aspx).

Por fim, tenha em mente que a reimplantação de um aplicativo pode levar de alguns segundos a vários minutos, dependendo do número e do tamanho dos arquivos que precisam ser copiados para o ambiente de produção. Durante esse tempo, os usuários que visitam seu site podem enfrentar erros ou comportamento estranho. Você pode "desligar" o aplicativo inteiro adicionando uma página chamada `App_Offline.htm` ao diretório raiz do seu aplicativo que explique aos usuários que o site está inativo para manutenção (ou qualquer outro) e será feito um backup em breve. Quando o arquivo de `App_Offline.htm` está presente, o tempo de execução do ASP.NET redireciona todas as solicitações de entrada para essa página.

## <a name="summary"></a>Resumo

Implantar um aplicativo Web envolve copiar os arquivos necessários do ambiente de desenvolvimento para o ambiente de produção. O meio mais comum pelo qual os arquivos são transferidos por uma rede é o protocolo FTP (FTP), e a maioria dos provedores de host da Web dá suporte ao acesso FTP para seus servidores Web. Neste tutorial, vimos como usar um cliente FTP para implantar os arquivos necessários no servidor Web. Depois de implantado, o site pode ser visitado por qualquer pessoa com uma conexão com a Internet!

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [O aplicativo\_offline. htm e está contornando o recurso "erros amigáveis do IE"](https://weblogs.asp.net/scottgu/App_5F00_Offline.htm-and-working-around-the-_2200_IE-Friendly-Errors_2200_-feature)
- [Modos de estado da sessão](https://msdn.microsoft.com/library/ms178586.aspx)

> [!div class="step-by-step"]
> [Anterior](determining-what-files-need-to-be-deployed-vb.md)
> [Próximo](deploying-your-site-using-visual-studio-vb.md)
