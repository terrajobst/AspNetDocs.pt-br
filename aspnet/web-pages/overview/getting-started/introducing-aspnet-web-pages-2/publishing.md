---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
title: Introdução ao Páginas da Web do ASP.NET-publicação de um site usando o WebMatrix | Microsoft Docs
author: Rick-Anderson
description: Este tutorial é a edição final do conjunto de tutoriais que apresenta Páginas da Web do ASP.NET e o Microsoft WebMatrix. Ele aborda como publicar seu site t...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 7e85c70e-1a88-4408-8b3d-29611c7713ed
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
msc.type: authoredcontent
ms.openlocfilehash: 49a841dbda183bf1d59153b83f694c9f517e0b94
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78633612"
---
# <a name="introducing-aspnet-web-pages---publishing-a-site-by-using-webmatrix"></a>Introdução ao Páginas da Web do ASP.NET-publicação de um site usando o WebMatrix

por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial é a edição final do conjunto de tutoriais que apresenta Páginas da Web do ASP.NET e o Microsoft WebMatrix. Ele aborda como publicar seu site na Internet para que outras pessoas possam trabalhar com eles. Ele pressupõe que você concluiu a série por meio da [criação de uma aparência consistente para Sites páginas da Web do ASP.net](https://go.microsoft.com/fwlink/?LinkId=251585).
> 
> Você aprenderá a publicar seu site usando:
> 
> - Microsoft Azure
> - Empresa de hospedagem na Web

## <a name="about-publishing-your-site"></a>Sobre a publicação de seu site

Até agora, você fez todo o seu trabalho em um computador local, incluindo o teste de suas páginas. Para executar suas páginas<em>. cshtml</em> , você usou o servidor Web interno do WebMatrix, a saber IIS Express. Mas, claro, ninguém pode ver o site que você criou, exceto você. Para permitir que outras pessoas trabalhem com seu site, você precisa publicá-lo na Internet.

A menos que você tenha acesso a um servidor Web público já, a publicação significa que você precisa ter uma conta com uma *plataforma de nuvem* ou um *provedor de hospedagem*. Uma plataforma de nuvem, como o Microsoft Azure, fornece infraestrutura sob demanda para seus aplicativos. Um provedor de hospedagem é uma empresa que possui servidores Web acessíveis publicamente e que irá alugar espaço para seu site. Os planos de hospedagem são executados de alguns dólares por mês (ou até mesmo gratuitos) para pequenos sites a muitos centenas de dólares por mês para sites comerciais de alto volume.

> [!NOTE]
> Você pode ter acesso a um servidor Web público por meio do provedor de serviços de Internet (ISP) que você usa para obter o serviço de Internet em casa. No entanto, seu provedor de hospedagem deve dar suporte a Páginas da Web do ASP.NET. Muitos ISPs não, mas sempre vale a pena verificar.

Neste tutorial, daremos uma visão geral de como publicar. Não é prático fornecer detalhes exatos para tudo, pois o processo difere um pouco para cada provedor de hospedagem. Mas você terá uma boa ideia de como o processo funciona.

Este tutorial contém quatro seções:

1. [Configurando a página padrão](#defaultpage)
2. Publicação (escolha uma das opções a seguir)  
 a. [Publicando seu site no Microsoft Azure](#azure)  
 b. [Publicando seu site em uma empresa de hospedagem na Web](#host)
3. [Atualizando o site ativo: republicando](#update)

<a id="defaultpage"></a>
## <a name="setting-up-the-default-page"></a>Configurando a página padrão

Quando um usuário navega para o endereço base do seu site, a página padrão do seu site é exibida para o usuário. Por exemplo, quando *Default. htm* é definido como a página padrão do site em `www.contoso.com`, navegar para `www.contoso.com` é o mesmo que navegar até `www.contoso.com/Default.htm`.

Atualmente, seu site usa **Default. cshtml** como a página padrão. Esta página é adequada para sua página padrão, mas neste tutorial você não adicionou nenhum conteúdo a essa página para que ela exiba uma página em branco. Abra default. cshtml e substitua o conteúdo pelo código a seguir.

[!code-cshtml[Main](publishing/samples/sample1.cshtml)]

Agora seu site está pronto para publicação. Primeiro, você verá como implantar o site no Azure e como implantá-lo em uma empresa de hospedagem na Web. Qualquer opção funciona para seu site e você só precisa seguir uma das opções de implantação.

<a id="azure"></a>
## <a name="publishing-your-site-to-microsoft-azure"></a>Publicando seu site no Microsoft Azure

Este tutorial mostrará primeiro como implantar seu site no Microsoft Azure. Ao entrar com um conta Microsoft, você pode criar até 10 sites gratuitos no Azure. Esses sites gratuitos fornecem uma maneira conveniente de testar seus sites. Você sempre pode excluir este site de exemplo posteriormente para evitar o uso de todos os seus sites gratuitos. Você pode criar uma conta de avaliação gratuita em apenas alguns minutos. Para obter detalhes, consulte [Avaliação gratuita do Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

Na faixa de faixas do WebMatrix, clique no botão **publicar** .

![Botão ' publicar ' na faixa de faixas do WebMatrix](publishing/_static/image1.png)

A caixa de diálogo **publicar seu site** é exibida. Se você não tiver entrado no seu conta Microsoft, a caixa de diálogo conterá um link **introdução ao Azure** . Clique neste link.

![Publicar seu site](publishing/_static/image2.png)

Se você não tiver entrado em um conta Microsoft, você receberá novamente a oportunidade de entrar. Você deve entrar em um conta Microsoft para publicar seu site no Azure.

![Entrar](publishing/_static/image3.png)

Depois de entrar no seu conta Microsoft, a caixa de diálogo contém links para criar um novo site no Azure ou conectar-se a um de seus sites existentes no Azure.

![Criar novo site](publishing/_static/image4.png)

Selecione **criar um novo site**.

Se você tiver nomeado o projeto **WebPagesMovies**, o nome padrão do seu site será **webpagesmovies.azurewebsites.net**. Esse nome padrão provavelmente não está disponível, conforme indicado pelo ponto de exclamação vermelho.

![nome do site da Web padrão](publishing/_static/image5.png)

Altere o nome do site para algo que esteja disponível e selecione um local que esteja próximo do local.

![nome do site alterado](publishing/_static/image6.png)

Clique em **OK**.

O WebMatrix segue um teste para determinar se o servidor é compatível com seu site.

![teste de compatibilidade](publishing/_static/image7.png)

Selecione **Continuar**.

Os resultados do teste de compatibilidade são exibidos.

![resultado da compatibilidade](publishing/_static/image8.png)

Selecione **Continuar**.

O WebMatrix exibe os arquivos e os bancos de dados que serão publicados no site. Como esta é a primeira vez que você está publicando o site, todos os arquivos são listados. Você pode desmarcar um arquivo que não está pronto para ser publicado. Nas publicações subsequentes, somente os arquivos que foram alterados serão exibidos. Consulte [atualizando o site ativo: republicando](#update).

![Publicar Visualização](publishing/_static/image9.png)

Selecione **Continuar**.

Depois que o site tiver sido implantado no Azure, será exibida uma mensagem indicando que a implantação foi concluída.

![Publicação concluída](publishing/_static/image10.png)

Seu site e banco de dados foram publicados no Azure e agora estão disponíveis para o público. Clique no link na mensagem indicando que a publicação foi concluída e agora você verá seu site implantado. Você ou qualquer pessoa com acesso à Internet pode adicionar ou modificar registros no banco de dados.

![](publishing/_static/image11.png)

<a id="host"></a>
## <a name="publishing-your-site-to-a-web-hosting-company"></a>Publicando seu site em uma empresa de hospedagem na Web

Se você decidir não publicar no Azure, poderá publicar seu site em uma empresa de hospedagem na Web.

Clique no link **Localizar hospedagem na Web** .

![Botão ' Localizar hospedagem na Web ' na caixa de diálogo Configurações de publicação](publishing/_static/image12.png)

Você vai para uma página no site da Microsoft que lista os provedores de hospedagem que dão suporte a ASP.NET.

![Página no site da Microsoft que lista provedores de hospedagem](publishing/_static/image13.png)

Obviamente, pode ser difícil saber agora exatamente quais recursos de hospedagem você pode exigir a longo prazo. Aqui estão algumas coisas a serem consideradas:

- Para fins do site WebPagesMovies, você não precisa ter um complemento separado para SQL Server, o que geralmente custa extra. No seu site, você está usando SQL Server Compact Edition, que é independente. No entanto, talvez seja necessário SQL Server acesso para algum trabalho futuro do site. Se você acredita que pode, certifique-se de que você pode adicionar SQL Server recurso mais tarde.
- Verifique se o provedor de hospedagem dá suporte ao protocolo de publicação Implantação da Web. Você pode publicar usando o protocolo FTP, mas é mais conveniente usar Implantação da Web.

Alguns sites oferecem um período de avaliação gratuita. Uma avaliação gratuita é uma boa maneira de tentar publicar e hospedar enquanto você ainda está experimentando o WebMatrix e o Páginas da Web do ASP.NET.

Escolha um que você deseja. Para este tutorial, selecionamos DiscountASP.NET, porque durante a criação do tutorial, essa empresa tinha uma promoção que permite que as pessoas hospedem um site gratuitamente por alguns meses.

> [!NOTE]
> Nossa escolha de um provedor de hospedagem para este tutorial não deve ser interpretada como um endosso da empresa em qualquer outro. Mas tivemos que escolher uma para ilustração, e DiscountASP.NET é uma das muitas empresas que dá suporte a Páginas da Web do ASP.NET e ao protocolo de Implantação da Web para publicação.

Normalmente, depois de se inscrever no provedor de hospedagem, a empresa envia um email que contém um nome de usuário e senha, a URL do servidor Web e assim por diante. Se a empresa de hospedagem der suporte ao protocolo Implantação da Web, ele poderá enviar um arquivo que contém configurações de publicação ou permitir que você baixe um. Um arquivo de configurações de publicação simplifica o processo para você.

Quando você se inscreveu e está pronto para publicar, clique no botão **publicar** na faixa de faixas do WebMatrix. A caixa de diálogo **configurações de publicação** é exibida.

Se o provedor de hospedagem tiver enviado um arquivo de configurações de publicação, clique no link **Importar configurações de publicação** e importe o arquivo. Se você não tiver um arquivo de configurações de publicação, preencha os campos usando os valores que a empresa de hospedagem enviou por email. Esta é a aparência da caixa de diálogo **configurações de publicação** quando você terminar:

![Configurações de publicação preenchidas na caixa de diálogo ' configurações de publicação '](publishing/_static/image14.png)

Clique em **validar conexão**. Se tudo estiver ok, a caixa de diálogo relatará **com êxito**, o que significa que ele pode se comunicar com o servidor do provedor de hospedagem.

![Mensagem de êxito se as configurações de publicação estiverem corretas](publishing/_static/image15.png)

Se houver um problema, o WebMatrix faz o melhor para dizer qual é o problema:

![Mensagem de erro se houver um problema com as configurações de publicação](publishing/_static/image16.png)

Para salvar suas configurações, clique em **Salvar** . O WebMatrix oferece para executar um teste para garantir que ele possa se comunicar corretamente com o site de hospedagem:

![Oferta de mensagem para executar um teste do processo de publicação](publishing/_static/image17.png)

Clique em **Sim**. O WebMatrix carrega alguns arquivos de exemplo no provedor de hospedagem. Quando o teste de compatibilidade é concluído, o WebMatrix relata os resultados:

![Resultados do teste de publicação](publishing/_static/image18.png)

Se você estiver pronto para começar, vá em frente e clique em **continuar** para iniciar o processo de publicação para o real. O WebMatrix descobre quais arquivos estão em seu site e já estão no servidor host (no momento, nenhum) e oferece uma visualização do processo de publicação:

![Visualização do que os arquivos serão carregados pelo processo de publicação](publishing/_static/image19.png)

A lista de arquivos a serem publicados inclui as páginas da Web que você criou como *Movie. cshtml*. A lista também inclui arquivos para auxiliares que você instalou, os arquivos a serem executados SQL Server Compact edição para seu banco de dados e assim por diante. Como resultado, o processo de publicação inicial pode ser substancial.

Clique em **Continuar**. O WebMatrix copia os arquivos para o servidor do provedor de hospedagem. Quando terminar, os resultados serão relatados na barra de status:

![Mensagem da barra de status quando o processo de publicação foi concluído com êxito](publishing/_static/image20.png)

Para ver seu site ativo, clique no link na barra de status. Adicione *filmes* à URL e você verá o arquivo *Movies. cshtml* que você criou:

![O site ativo mostrando a página de filmes](publishing/_static/image21.png)

<a id="update"></a>
## <a name="updating-the-live-site-republishing"></a>Atualizando o site ativo: republicando

Depois de publicar seu site (para o Azure ou uma empresa de hospedagem na Web), há duas cópias dele &mdash; a versão no seu computador e a versão no provedor de serviços. Você provavelmente desejará continuar desenvolvendo o site (se nada mais, como parte do próximo conjunto de tutorial). Quando fizer isso, você precisará republicar seu site para copiar as alterações do seu computador para o provedor de serviços. O processo de publicação no WebMatrix pode determinar quais arquivos foram alterados no seu site e publicar apenas esses arquivos.

Para ver como a republicação funciona, abra o site *Movies. cshtml* , faça algumas pequenas alterações e salve o arquivo. Por exemplo, altere o título para `Movies - Updated`.

Clique no botão **publicar** na faixa de faixas. O WebMatrix determina o que mudou e mostra uma visualização dos arquivos que ele publicará.

![A caixa de diálogo ' publicar ' mostrando arquivos alterados prontos para republicação](publishing/_static/image22.png)

> [!IMPORTANT] 
> 
> Por padrão, o WebMatrix publica seu banco de dados (arquivo *. sdf* ) somente na primeira vez que você publicar o site. Depois que o site é publicado e as pessoas estão interagindo com o site, o banco de dados no site ativo normalmente tem o real dado do site. Você precisa ter muito cuidado para não substituir o banco de dados ao vivo pelo arquivo *. sdf* que está em seu computador, que geralmente contém apenas dado de teste. É por isso que você vê que a publicação de aviso **substituirá todos os bancos de dados remotos**e por que a caixa de seleção de *WebPagesMovies. sdf* é desmarcada por padrão.

Clique em **Continuar**. O WebMatrix publica os arquivos alterados e mostra uma mensagem de êxito, como na primeira vez que você publicou.

Vá para o site ativo (você pode clicar no link na mensagem de êxito se ele ainda estiver sendo exibido) e verificar se a alteração foi publicada.

> [!TIP] 
> 
> **Editando arquivos remotamente**
> 
> Como alternativa à alteração de seu site e, em seguida, à republicação, você pode editar arquivos remotos diretamente no WebMatrix. Nesse cenário, você abre um arquivo que está no provedor de serviços e o WebMatrix baixa uma cópia dele para editar. Toda vez que você salva o arquivo, o WebMatrix envia as alterações para o site.
> 
> A edição remota é uma maneira fácil de fazer alterações em seu site ativo. No entanto, as alterações feitas dessa maneira não são sincronizadas com os arquivos no site local. Para sincronizar os arquivos locais com o site remoto, você pode baixar os arquivos remotos. Esse processo funciona de forma muito semelhante à publicação, exceto no inverso.
> 
> Não descreveremos mais sobre os recursos de edição remota e download remoto do WebMatrix aqui. Eles serão bastante úteis se várias pessoas tiverem que trabalhar no mesmo site em computadores diferentes. Para obter mais informações, consulte [publicar e editar um site remoto com o WebMatrix 2 beta](https://go.microsoft.com/fwlink/?LinkId=251591).

## <a name="additional-resources"></a>Recursos adicionais

- [ASP.net WebMatrix páginas da Web do ASP.net forum](https://forums.asp.net/1224.aspx/1?WebMatrix+and+ASP+NET+Web+Pages), um ótimo lugar para postar perguntas e obter respostas.

> [!div class="step-by-step"]
> [Anterior](layouts.md)
