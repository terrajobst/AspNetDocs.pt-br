---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-cs
title: Implantando seu site usando o VisualC#Studio () | Microsoft Docs
author: rick-anderson
description: O Visual Studio inclui ferramentas para implantar um site. Saiba mais sobre essas ferramentas neste tutorial.
ms.author: riande
ms.date: 04/01/2009
ms.assetid: cde4ee53-a5d0-4937-a54b-67877e8266c3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-cs
msc.type: authoredcontent
ms.openlocfilehash: 4259e51f5a3e6a97bae2aa27b76cbd56ca3449d6
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74636306"
---
# <a name="deploying-your-site-using-visual-studio-c"></a>Implantação do site com o uso do Visual Studio (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_04_CS.zip) ou [baixar PDF](https://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial04_DeployingViaVS_cs.pdf)

> O Visual Studio inclui ferramentas para implantar um site. Saiba mais sobre essas ferramentas neste tutorial.

## <a name="introduction"></a>Introdução

O tutorial anterior examinou como implantar um aplicativo Web ASP.NET simples em um provedor de host da Web. Especificamente, o tutorial mostrou como usar um cliente FTP como o FileZilla para transferir os arquivos necessários do ambiente de desenvolvimento para o ambiente de produção. O Visual Studio também oferece ferramentas internas para facilitar a implantação em um provedor de host da Web. Este tutorial examina duas dessas ferramentas: a ferramenta Copy Web site, na qual você pode mover arquivos de e para um servidor Web remoto usando as extensões de servidor FTP ou FrontPage; e a ferramenta de publicação, que copia todo o site para um local especificado.

> [!NOTE]
> Outras ferramentas relacionadas à implantação oferecidas pelo Visual Studio incluem [projetos de configuração da Web](https://msdn.microsoft.com/library/wx3b589t.aspx) e suplemento de [projetos de implantação na Web](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) . Os projetos de instalação da Web empacotam o conteúdo e as informações de configuração de um site em um único arquivo MSI. Essa opção é mais útil para sites que são implantados em uma intranet ou para empresas que vendem um aplicativo Web predefinido que os clientes instalam em seus próprios servidores Web. O suplemento de projetos de implantação da Web é um suplemento do Visual Studio que facilita a especificação de diferenças de configuração entre compilações para ambientes de desenvolvimento e ambientes de produção. Os projetos de configuração da Web não são discutidos nesta série de tutoriais; Os projetos de implantação na Web são resumidos nas [*diferenças de configuração comuns entre o tutorial de desenvolvimento e produção*](common-configuration-differences-between-development-and-production-cs.md) .

## <a name="deploying-your-site-using-the-copy-web-site-tool"></a>Implantando seu site usando a ferramenta Copy Web site

A ferramenta Copy Web site do Visual Studio é semelhante em funcionalidade a um cliente FTP autônomo. Resumindo, a ferramenta Copy Web site permite que você se conecte a um site remoto por meio de extensões de servidor FTP ou FrontPage. Semelhante à interface do usuário do FileZilla, a interface do usuário de copiar site consiste em dois painéis: o painel esquerdo lista os arquivos locais enquanto o painel direito lista esses arquivos no servidor de destino.

> [!NOTE]
> A ferramenta Copy Web site só está disponível para projetos de site. O Visual Studio oferece essa ferramenta quando você está trabalhando com um projeto de aplicativo Web.

Vamos dar uma olhada no uso da ferramenta Copy Web site para publicar o aplicativo de análise de livros em produção. Como a ferramenta Copy Web site funciona apenas com projetos que usam o modelo de projeto de site, só podemos examinar o uso dessa ferramenta com o projeto BookReviewsWSP. Abra esse projeto.

Inicie o projeto de ferramenta Copiar site da Web clicando no ícone Copiar site na Gerenciador de Soluções (este ícone está circulado na Figura 1); Como alternativa, você pode selecionar a opção Copy Web site no menu do site. Qualquer uma das abordagens inicia a interface do usuário de copiar site da Web mostrada na Figura 1; somente o painel esquerdo da Figura 1 é populado porque ainda temos que se conectar a um servidor remoto.

[![a interface do usuário da ferramenta Copiar site da Web é dividida em dois painéis](deploying-your-site-using-visual-studio-cs/_static/image2.png)](deploying-your-site-using-visual-studio-cs/_static/image1.png)

**Figura 1**: a interface do usuário da ferramenta Copiar site da Web é dividida em dois painéis ([clique para exibir a imagem em tamanho normal](deploying-your-site-using-visual-studio-cs/_static/image3.png))

Para implantar nosso site, precisamos primeiro se conectar ao provedor de host da Web. Clique no botão conectar na parte superior da interface do usuário copiar site. Isso exibe a caixa de diálogo abrir site da Web mostrada na Figura 2.

Você pode se conectar ao site de destino selecionando uma das quatro opções à esquerda:

- **Sistema de arquivos** -Selecione esta ação para implantar seu site em uma pasta ou compartilhamento de rede acessível do seu computador.
- **IIS local** -Use essa opção para implantar o site no servidor Web do IIS instalado no computador.
- **Site FTP** -Conecte-se a um site remoto usando FTP.
- **Site remoto** -Conecte-se a um site remoto usando as extensões de servidor do FrontPage.

A maioria dos provedores de hosts Web dá suporte a FTP, mas menos oferece suporte à extensão de servidor do FrontPage. Por esse motivo, selecionei a opção site FTP e, em seguida, inseri as informações de conexão, conforme mostrado na Figura 2.

[![especificar o site de destino](deploying-your-site-using-visual-studio-cs/_static/image5.png)](deploying-your-site-using-visual-studio-cs/_static/image4.png)

**Figura 2**: especificar o site de destino ([clique para exibir a imagem em tamanho normal](deploying-your-site-using-visual-studio-cs/_static/image6.png))

Depois de se conectar, a ferramenta Copy Web site carrega os arquivos no site remoto no painel direito e indica o status de cada arquivo: novo, excluído, alterado ou inalterado. Você pode copiar um arquivo do site local para o site remoto, ou vice-versa.

Vamos adicionar uma nova página ao projeto BookReviewsWSP e implantá-la para que possamos ver a ferramenta Copy Web site em ação. Crie uma nova página do ASP.NET no Visual Studio no diretório raiz chamado `Privacy.aspx`. Faça com que a página use a página mestra `Site.master` e adicione a política de privacidade do site a esta página. A Figura 3 mostra o Visual Studio após a criação desta página.

[![adicionar uma nova página chamada &lt;Code&gt;privacy. aspx&lt;/Code&gt; à pasta raiz do site](deploying-your-site-using-visual-studio-cs/_static/image8.png)](deploying-your-site-using-visual-studio-cs/_static/image7.png)

**Figura 3**: adicionar uma nova página chamada `Privacy.aspx` à pasta raiz do site ([clique para exibir a imagem em tamanho normal](deploying-your-site-using-visual-studio-cs/_static/image9.png))

Em seguida, retorne à interface do usuário do site de cópia. Como mostra a Figura 4, o painel esquerdo agora inclui os novos arquivos – `Policy.aspx` e `Policy.aspx.cs`. Além disso, esses arquivos são marcados com um ícone de seta e um status novo, indicando que eles existem no site local, mas não no site remoto.

[![a ferramenta Copy Web site inclui o novo código de &lt;&gt;privacy. aspx&lt;página de&gt; de/code em seu painel esquerdo](deploying-your-site-using-visual-studio-cs/_static/image11.png)](deploying-your-site-using-visual-studio-cs/_static/image10.png)

**Figura 4**: a ferramenta Copy Web site inclui a nova página `Privacy.aspx` no painel esquerdo ([clique para exibir a imagem em tamanho normal](deploying-your-site-using-visual-studio-cs/_static/image12.png))

Para implantar os novos arquivos, selecione-os e clique no ícone de seta para transferi-los para o site remoto. Depois que a transferência concluir a `Policy.aspx` e `Policy.aspx.cs` existirem arquivos em sites locais e remotos com o status inalterado.

Juntamente com a listagem de novos arquivos, a ferramenta Copy Web site realça todos os arquivos que diferem entre os sites locais e remotos. Para ver isso em ação, retorne à página `Privacy.aspx` e adicione mais algumas palavras à política de privacidade. Salve a página e, em seguida, retorne à ferramenta Copy Web site. Como mostra a Figura 5, a página `Privacy.aspx` no painel esquerdo tem um status de alterado indicando que ele está fora de sincronia com o site remoto.

[![a ferramenta Copy Web site indica que o código de &lt;&gt;privacy. aspx&lt;página de&gt; de/Code foi alterada](deploying-your-site-using-visual-studio-cs/_static/image14.png)](deploying-your-site-using-visual-studio-cs/_static/image13.png)

**Figura 5**: a ferramenta Copy Web site indica que a página de `Privacy.aspx` foi alterada ([clique para exibir a imagem em tamanho normal](deploying-your-site-using-visual-studio-cs/_static/image15.png))

A ferramenta Copiar site da Web também indica se um arquivo foi excluído desde a última operação de cópia. Exclua o `Privacy.aspx` do projeto local e atualize a ferramenta Copy Web site. Os arquivos de `Privacy.aspx` e `Privacy.aspx.cs` permanecem listados no painel esquerdo, mas têm um status excluído indicando que foram removidos desde a última operação de cópia.

## <a name="publishing-a-web-application"></a>Publicando um aplicativo Web

Outra maneira de implantar seu aplicativo Web no Visual Studio é usar a opção publish, que pode ser acessada por meio do menu Build. A opção de publicação compila explicitamente o aplicativo e, em seguida, copia todos os arquivos necessários até o site remoto especificado. Como veremos em breve, a opção publish é mais moderada do que a ferramenta Copy Web site. Enquanto a ferramenta Copy Web site permite examinar os arquivos nos sites locais e remotos e permite que você carregue ou baixe arquivos individuais conforme necessário, a opção de publicação implanta todo o aplicativo Web.

Além de copiar todos os arquivos necessários para o site remoto especificado, a opção de publicação também compila explicitamente o aplicativo. Considerando que os projetos de aplicativos Web precisam ser compilados explicitamente, deve ser tão surpresa que a opção de publicação esteja disponível para projetos de aplicativos Web. O que pode ser um pouco surpreendente é que a opção de publicação também está disponível para projetos de site. Conforme observado no tutorial [*determinando quais arquivos precisam ser implantados*](determining-what-files-need-to-be-deployed-cs.md) , os projetos de site podem ser compilados explicitamente por meio de um processo referido como *pré-compilação*. Este tutorial se concentra no uso da opção publish com projetos de aplicativos Web; um tutorial futuro explorará a pré-compilação, ponto em que vamos voltar a examinar usando a opção de publicação com projetos de site.

> [!NOTE]
> Embora a opção de publicação esteja disponível no Visual Studio para projetos de sites e projetos de aplicativos Web, o Visual Web Developer oferece apenas a opção de publicação para projetos de aplicativos Web.

Vamos examinar a implantação do aplicativo Reviews de livros usando a opção publicar. Comece abrindo BookReviewsWAP (o projeto de aplicativo Web) no Visual Studio. No menu publicar, escolha o projeto Build BookReviewsWAP. Isso abre uma caixa de diálogo que solicita o local de destino, entre outras opções de configuração (veja a Figura 6). Assim como com a ferramenta Copy Web site, você pode inserir um local que aponta para uma pasta local, um site local no IIS, um site remoto que dá suporte a extensões de servidor do FrontPage ou um endereço de servidor FTP. Você pode escolher se deseja substituir os arquivos no servidor Web remoto pelos arquivos implantados ou excluir todo o conteúdo no site remoto antes da publicação. Você também pode especificar se deseja copiar:

- Somente os arquivos no projeto precisavam executar o aplicativo, o que omite o código-fonte desnecessário e os arquivos relacionados ao projeto.
- Todos os arquivos de projeto, que incluem os arquivos de código-fonte e os arquivos de projeto do Visual Studio, como o arquivo de solução.
- Todos os arquivos na pasta do projeto de origem, que copia todos os arquivos na pasta do projeto de origem, independentemente de estarem incluídos no projeto.

Há também uma opção para carregar o conteúdo da pasta `App_Data`.

[![especificar o site de destino](deploying-your-site-using-visual-studio-cs/_static/image17.png)](deploying-your-site-using-visual-studio-cs/_static/image16.png)

**Figura 6**: especificar o site de destino ([clique para exibir a imagem em tamanho normal](deploying-your-site-using-visual-studio-cs/_static/image18.png))

Para o aplicativo de análise de livro, o site remoto contém os arquivos implantados ao copiar o projeto BookReviewsWSP por meio da ferramenta Copy Web site. Portanto, vamos fazer com que a opção publish seja iniciada excluindo todo o conteúdo existente. Além disso, vamos copiar os arquivos necessários, em vez de obstruir o ambiente de produção com código-fonte desnecessário e arquivos de projeto. Depois de especificar essas opções, clique no botão publicar. Nos próximos vários segundos, o Visual Studio implantará os arquivos necessários no site de destino, exibindo seu progresso na janela de saída.

A Figura 7 mostra os arquivos no site FTP após a conclusão da operação de publicação. Observe que somente as páginas de marcação e os arquivos de suporte necessários do servidor e do lado do cliente foram carregados.

[![apenas os arquivos necessários foram publicados no ambiente de produção](deploying-your-site-using-visual-studio-cs/_static/image20.png)](deploying-your-site-using-visual-studio-cs/_static/image19.png)

**Figura 7**: somente os arquivos necessários foram publicados no ambiente de produção ([clique para exibir a imagem em tamanho normal](deploying-your-site-using-visual-studio-cs/_static/image21.png))

A opção publish é uma ferramenta menos nuance do que a ferramenta Copy Web site. Enquanto a ferramenta Copy Web site permite inspecionar os arquivos nos sites locais e remotos e ver como eles diferem, a opção publicar não fornece essa interface. Além disso, a ferramenta Copy Web site permite que você faça alterações isoladas, carregando ou excluindo arquivos individuais. A opção de publicação não permite esse controle refinado; em vez disso, ele publica *todo* o aplicativo. Esse comportamento tem seus prós e contras. No lado de mais, você sabe ao usar a opção de publicação, você não esquecerá de carregar um arquivo importante. Mas considere o que acontece se você tiver feito uma pequena alteração em um site muito grande – com a opção de publicação, você não poderá atualizar essa página ou duas que foram modificadas, mas, em vez disso, você deve aguardar enquanto o Visual Studio implanta todo o site.

Não é incomum que haja alguns arquivos cujo conteúdo seja diferente entre os ambientes de produção e desenvolvimento. Um exemplo de chave é o arquivo de configuração do aplicativo, `Web.config`. Como a opção de publicação copia indistintamente os arquivos do aplicativo Web, ele substitui os arquivos de configuração personalizados do ambiente de produção pela versão no ambiente de desenvolvimento. O tutorial subsequente explora ainda mais este tópico e oferece dicas para implantar um aplicativo Web quando existem essas diferenças.

## <a name="summary"></a>Resumo

A implantação de um site envolve a cópia dos arquivos necessários do ambiente de desenvolvimento para o ambiente de produção. O tutorial anterior mostrou como transferir arquivos usando um cliente FTP, como o FileZilla. Este tutorial examinou duas ferramentas de implantação no Visual Studio: a ferramenta Copy Web site e a opção publish. A ferramenta Copy Web site é semelhante a um cliente FTP, pois tem uma interface de dois painéis listando os arquivos no computador local e um computador remoto especificado que facilita o carregamento ou o download de arquivos entre os dois computadores. A opção de publicação é uma ferramenta mais moderada que compila explicitamente o projeto e, em seguida, implanta todo o aplicativo no destino especificado.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Copiando o site com a ferramenta Copy Web site](https://msdn.microsoft.com/library/1cc82atw.aspx)
- [Como faço para: implantar um site usando a ferramenta Copy Web site](../../../videos/how-do-i/how-do-i-deploy-a-web-site-using-the-copy-web-site-tool.md) (vídeo)
- [Como publicar projetos de aplicativos Web](https://msdn.microsoft.com/library/aa983453.aspx)
- [Como publicar sites da Web](https://msdn.microsoft.com/library/20yh9f1b.aspx)
- [Projetos de instalação e implantação no Visual Studio](https://msdn.microsoft.com/library/wx3b589t.aspx)

> [!div class="step-by-step"]
> [Anterior](deploying-your-site-using-an-ftp-client-cs.md)
> [Próximo](common-configuration-differences-between-development-and-production-cs.md)
