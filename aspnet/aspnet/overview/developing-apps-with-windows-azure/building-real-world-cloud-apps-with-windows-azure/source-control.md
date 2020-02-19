---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
title: Controle do código-fonte (criando aplicativos de nuvem do mundo real com o Azure) | Microsoft Docs
author: MikeWasson
description: A criação de aplicativos de nuvem do mundo real com o livro eletrônico do Azure é baseada em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas que podem...
ms.author: riande
ms.date: 06/23/2015
ms.assetid: 2a0370d3-c2fb-4bf3-88b8-aad5a736c793
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
msc.type: authoredcontent
ms.openlocfilehash: 5a1e0d7cd3c396d4be79c8958422602055eb3db1
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457096"
---
# <a name="source-control-building-real-world-cloud-apps-with-azure"></a>Controle do código-fonte (criando aplicativos de nuvem do mundo real com o Azure)

por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Baixar o projeto de ti](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [baixar o livro eletrônico](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> A **criação de aplicativos de nuvem do mundo real com** o livro eletrônico do Azure é baseada em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas que podem ajudá-lo a desenvolver com sucesso aplicativos Web para a nuvem. Para obter informações sobre o livro eletrônico, consulte [o primeiro capítulo](introduction.md).

O controle do código-fonte é essencial para todos os projetos de desenvolvimento em nuvem, não apenas para ambientes de equipe. Você não imagina editar o código-fonte ou até mesmo um documento do Word sem uma função undo e backups automáticos, e o controle do código-fonte fornece essas funções em um nível de projeto, onde podem economizar ainda mais tempo quando algo dá errado. Com os serviços de controle do código-fonte de nuvem, você não precisa mais se preocupar com a configuração complicada e pode usar Azure Repos controle do código-fonte gratuitamente para até 5 usuários.

A primeira parte deste capítulo explica três principais práticas recomendadas para se ter em mente:

- [Trate scripts de automação como código-fonte](#scripts) e os versione em conjunto com o código do aplicativo.
- [Nunca faça check-in de segredos](#secrets) (dados confidenciais, como credenciais) em um repositório de código-fonte.
- [Configure branches de origem](#devops) para habilitar o fluxo de trabalho DevOps.

O restante do capítulo fornece algumas implementações de exemplo desses padrões no Visual Studio, no Azure e no Azure Repos:

- [Adicionar scripts ao controle do código-fonte no Visual Studio](#vsscripts)
- [Armazenar dados confidenciais no Azure](#appsettings)
- [Usar o Git no Visual Studio e Azure Repos](#gittfs)

<a id="scripts"></a>
## <a name="treat-automation-scripts-as-source-code"></a>Tratar scripts de automação como código-fonte

Quando você estiver trabalhando em um projeto de nuvem que está alterando as coisas com frequência e deseja ser capaz de reagir rapidamente aos problemas relatados por seus clientes. Responder rapidamente envolve o uso de scripts de automação, conforme explicado no capítulo [automatizar tudo](automate-everything.md) . Todos os scripts que você usa para criar seu ambiente, implantá-lo, dimensioná-lo, etc., precisam estar em sincronia com o código-fonte do aplicativo.

Para manter os scripts sincronizados com o código, armazene-os no sistema de controle do código-fonte. Em seguida, se você precisar reverter as alterações ou fazer uma correção rápida para o código de produção, que é diferente do código de desenvolvimento, você não precisa perder tempo tentando rastrear quais configurações foram alteradas ou quais membros da equipe têm cópias da versão de que você precisa. Você tem certeza de que os scripts de que precisa estão em sincronia com a base de código para a qual você precisa, e tem certeza de que todos os membros da equipe estão trabalhando com os mesmos scripts. Se você precisar automatizar o teste e a implantação de um Hot Fix para a produção ou para um novo desenvolvimento de recursos, terá o script correto para o código que precisa ser atualizado.

<a id="secrets"></a>
## <a name="dont-check-in-secrets"></a>Não fazer check-in de segredos

Um repositório de código-fonte normalmente é acessível a muitas pessoas para que seja um local apropriado para dados confidenciais, como senhas. Se os scripts dependerem de segredos como senhas, parametrizará essas configurações para que elas não sejam salvas no código-fonte e armazenem seus segredos em outro lugar.

Por exemplo, o Azure permite que você baixe arquivos que contêm configurações de publicação a fim de automatizar a criação de perfis de publicação. Esses arquivos incluem nomes de usuário e senhas que são autorizados a gerenciar seus serviços do Azure. Se você usar esse método para criar perfis de publicação e se fizer check-in desses arquivos no controle do código-fonte, qualquer pessoa com acesso ao seu repositório poderá ver esses nomes de usuário e senhas. Você pode armazenar a senha com segurança no próprio perfil de publicação porque ela está criptografada e está em um arquivo *. pubxml. User* que, por padrão, não está incluído no controle do código-fonte.

<a id="devops"></a>
## <a name="structure-source-branches-to-facilitate-devops-workflow"></a>Ramificações de origem da estrutura para facilitar o fluxo de trabalho do DevOps

A maneira como você implementa as ramificações em seu repositório afeta sua capacidade de desenvolver novos recursos e corrigir problemas na produção. Aqui está um padrão que muitas equipes de médio porte usam:

![Estrutura de Branch de origem](source-control/_static/image1.png)

A ramificação Mestre sempre corresponde ao código que está em produção. As ramificações abaixo do mestre correspondem a diferentes estágios no ciclo de vida de desenvolvimento. A ramificação de desenvolvimento é onde você implementa novos recursos. Para uma pequena equipe, você pode ter apenas o mestre e o desenvolvimento, mas geralmente recomendamos que as pessoas tenham uma ramificação de preparo entre o desenvolvimento e o mestre. Você pode usar o preparo para o teste de integração final antes que uma atualização seja movida para a produção.

Para equipes grandes, pode haver ramificações separadas para cada novo recurso; para uma equipe menor, você pode ter todos fazendo check-in na ramificação de desenvolvimento.

Se você tiver uma ramificação para cada recurso, quando o recurso A estiver pronto, você mesclará suas alterações de código-fonte na ramificação de desenvolvimento e em outras ramificações de recurso. Esse processo de mesclagem de código-fonte pode ser demorado e para evitar que o trabalho ainda mantenha os recursos separados, algumas equipes implementam uma alternativa chamada *[recurso alterna](http://en.wikipedia.org/wiki/Feature_toggle)* (também conhecido como *sinalizadores de recurso*). Isso significa que todo o código para todos os recursos está na mesma ramificação, mas você habilita ou desabilita cada recurso usando opções no código. Por exemplo, suponha que o recurso A é um novo campo para corrigir tarefas de aplicativo de ti e o recurso B adiciona a funcionalidade de cache. O código para ambos os recursos pode estar no Branch de desenvolvimento, mas o aplicativo só exibirá o novo campo quando uma variável for definida como true e usará apenas o cache quando uma variável diferente for definida como true. Se o recurso A não estiver pronto para ser promovido, mas o recurso B estiver pronto, você poderá promover todo o código para produção com o recurso A de um interruptor e o recurso B ligar. Em seguida, você pode concluir o recurso A e promovê-lo mais tarde, tudo isso sem nenhuma mesclagem de código-fonte.

Se você usa ou não ramificações ou alterna para recursos, uma estrutura de ramificação como essa permite que você flua seu código do desenvolvimento para produção de maneira ágil e reproduzível.

Essa estrutura também permite que você reaja rapidamente aos comentários dos clientes. Se você precisar fazer uma correção rápida para a produção, também poderá fazer isso com eficiência de maneira ágil. Você pode criar uma ramificação do mestre ou de preparo e, quando ela estiver pronta, mesclá-la no mestre e nos branches de desenvolvimento e recursos.

![Branch de hotfix](source-control/_static/image2.png)

Sem uma estrutura de ramificação como essa com sua separação de ramificações de produção e desenvolvimento, um problema de produção pode colocar você na posição de ter que promover um novo código de recurso junto com sua correção de produção. O novo código de recurso pode não estar totalmente testado e pronto para produção e talvez você precise fazer muito trabalho de backup de alterações que não estão prontas. Ou talvez seja necessário atrasar a correção para testar as alterações e colocá-las prontas para serem implantadas.

Em seguida, você verá exemplos de como implementar esses três padrões no Visual Studio, no Azure e no Azure Repos. Esses são exemplos, em vez de instruções detalhadas passo a passo sobre como fazer. para obter instruções detalhadas que fornecem todo o contexto necessário, consulte a seção [recursos](#resources) no final do capítulo.

<a id="vsscripts"></a>
## <a name="add-scripts-to-source-control-in-visual-studio"></a>Adicionar scripts ao controle do código-fonte no Visual Studio

Você pode adicionar scripts ao controle do código-fonte no Visual Studio incluindo-os em uma pasta de solução do Visual Studio (supondo que seu projeto esteja no controle do código-fonte). Aqui está uma maneira de fazer isso.

Crie uma pasta para os scripts na pasta da solução (a mesma pasta que tem o arquivo *. sln* ).

![Pasta de automação](source-control/_static/image3.png)

Copie os arquivos de script na pasta.

![Conteúdo da pasta de automação](source-control/_static/image4.png)

No Visual Studio, adicione uma pasta de solução ao projeto.

![Seleção do menu nova pasta de solução](source-control/_static/image5.png)

E adicione os arquivos de script à pasta da solução.

![Adicionar seleção de menu de item existente](source-control/_static/image6.png)

![Caixa de diálogo Adicionar Item Existente](source-control/_static/image7.png)

Os arquivos de script agora estão incluídos no seu projeto e o controle do código-fonte está acompanhando as alterações de versão junto com as alterações de código-fonte correspondentes.

<a id="appsettings"></a>
## <a name="store-sensitive-data-in-azure"></a>Armazenar dados confidenciais no Azure

Se você executar seu aplicativo em um site do Azure, uma maneira de evitar o armazenamento de credenciais no controle do código-fonte é armazená-los no Azure.

Por exemplo, o aplicativo Fix it armazena em seu arquivo Web. config duas cadeias de conexão que terão senhas em produção e uma chave que fornece acesso à sua conta de armazenamento do Azure.

[!code-xml[Main](source-control/samples/sample1.xml?highlight=2-3,11)]

Se você colocar os valores de produção reais para essas configurações em seu arquivo *Web. config* , ou se você colocá-las no arquivo *Web. Release. config* para configurar uma transformação Web. config para inseri-las durante a implantação, elas serão armazenadas no repositório de origem. Se você inserir as cadeias de conexão de banco de dados no perfil de publicação de produção, a senha estará no arquivo *. pubxml* . (Você pode excluir o arquivo *. pubxml* do controle do código-fonte, mas, em seguida, você perde o benefício de compartilhar todas as outras configurações de implantação.)

O Azure fornece uma alternativa para as seções **appSettings** e cadeias de conexão do arquivo *Web. config* . Aqui está a parte relevante da guia de **configuração** para um site no portal de gerenciamento do Azure:

![appSettings e connectionStrings no portal](source-control/_static/image8.png)

Quando você implanta um projeto nesse site e o aplicativo é executado, quaisquer valores armazenados no Azure substituem quaisquer valores no arquivo Web. config.

Você pode definir esses valores no Azure usando o portal de gerenciamento ou scripts. O script de automação de criação de ambiente que você viu no capítulo [automatizar tudo](automate-everything.md) cria um banco de dados SQL do Azure, obtém as cadeias de conexão de armazenamento e banco de dados SQL e armazena esses segredos nas configurações do seu site.

[!code-powershell[Main](source-control/samples/sample2.ps1)]

[!code-powershell[Main](source-control/samples/sample3.ps1)]

Observe que os scripts são parametrizados para que os valores reais não sejam persistidos no repositório de origem.

Quando você executa localmente em seu ambiente de desenvolvimento, o aplicativo lê o arquivo Web. config local e sua cadeia de conexão aponta para um banco de dados de SQL Server do LocalDB na pasta *\_data do aplicativo* de seu projeto Web. Quando você executa o aplicativo no Azure e o aplicativo tenta ler esses valores do arquivo Web. config, o que ele retorna e usa são os valores armazenados para o site, não o que realmente está no arquivo Web. config.

<a id="gittfs"></a>
## <a name="use-git-in-visual-studio-and-azure-devops"></a>Usar o Git no Visual Studio e no Azure DevOps

Você pode usar qualquer ambiente de controle do código-fonte para implementar a estrutura de ramificação DevOps apresentada anteriormente. Para equipes distribuídas, um [sistema de controle de versão distribuído](http://en.wikipedia.org/wiki/Distributed_revision_control) (DVCS) pode funcionar melhor; para outras equipes, um [sistema centralizado](http://en.wikipedia.org/wiki/Revision_control) pode funcionar melhor.

O [git](http://git-scm.com/) é um sistema de controle de versão distribuído popular. Quando você usa o Git para controle do código-fonte, você tem uma cópia completa do repositório com todo o seu histórico em seu computador local. Muitas pessoas preferem isso porque é mais fácil continuar trabalhando quando você não estiver conectado à rede--você pode continuar a fazer confirmações e reversões, criar e alternar ramificações e assim por diante. Mesmo quando você está conectado à rede, é mais fácil e rápido criar ramificações e alternar ramificações quando tudo é local. Você também pode fazer confirmações e reversões locais sem ter um impacto em outros desenvolvedores. E você pode executar confirmações em lote antes de enviá-las ao servidor.

O [Azure Repos](/azure/devops/repos/index?view=vsts) oferece o [Git](/azure/devops/repos/git/?view=vsts) e o [controle de versão do Team Foundation](/azure/devops/repos/tfvc/index?view=vsts) (TFVC; controle do código-fonte centralizado). Introdução ao DevOps do Azure [aqui](https://app.vsaex.visualstudio.com/signup).

O Visual Studio 2017 inclui suporte interno a [git](https://msdn.microsoft.com/library/hh850437.aspx)de primeira classe. Aqui está uma demonstração rápida de como isso funciona.

Com um projeto aberto no Visual Studio, clique com o botão direito do mouse na solução em **Gerenciador de soluções**e escolha **Adicionar solução ao controle do código-fonte**.

![Adicionar solução ao controle do código-fonte](source-control/_static/image9.png)

O Visual Studio pergunta se você deseja usar TFVC (controle de versão centralizado) ou git.

![Escolher controle do código-fonte](source-control/_static/image10.png)

Quando você seleciona git e clica em **OK**, o Visual Studio cria um novo repositório git local na pasta da solução. O novo repositório ainda não tem arquivos; Você precisa adicioná-los ao repositório fazendo uma confirmação do git. Clique com o botão direito do mouse na solução em **Gerenciador de soluções**e clique em **confirmar**.

![Commit](source-control/_static/image11.png)

O Visual Studio automaticamente prepara todos os arquivos de projeto para a confirmação e os lista em **Team Explorer** no painel **alterações incluídas** . (Se houver alguns que não desejamos incluir na confirmação, você poderá selecioná-los, clicar com o botão direito do mouse e clicar em **excluir**.)

![Team Explorer](source-control/_static/image12.png)

Insira um comentário de confirmação e clique em **confirmar**, e o Visual Studio executará a confirmação e exibirá a ID de confirmação.

![Team Explorer alterações](source-control/_static/image13.png)

Agora, se você alterar algum código para que ele seja diferente do que está no repositório, você poderá exibir facilmente as diferenças. Clique com o botão direito do mouse em um arquivo que você alterou, selecione **comparar com não modificado**e você obterá uma exibição de comparação que mostra a alteração não confirmada.

![Comparar com não modificado](source-control/_static/image14.png)

![Comparação mostrando alterações](source-control/_static/image15.png)

Você pode ver facilmente as alterações que está fazendo e verificá-las.

Suponha que você precise fazer uma ramificação – você pode fazer isso no Visual Studio também. Em **Team Explorer**, clique em **nova ramificação**.

![Team Explorer nova Branch](source-control/_static/image16.png)

Insira um nome de Branch, clique em **criar ramificação**e, se você selecionou **Branch de check-out**, o Visual Studio verificará automaticamente o novo Branch.

![Team Explorer nova Branch](source-control/_static/image17.png)

Agora você pode fazer alterações em arquivos e verificá-los nessa ramificação. E você pode alternar facilmente entre branches e o Visual Studio sincroniza automaticamente os arquivos para qualquer Branch que você fez check-out. Neste exemplo, o título da página da Web em *\_layout. cshtml* foi alterado para "Hot Fix 1" na ramificação HotFix1.

![Ramificação Hotfix1](source-control/_static/image18.png)

Se você voltar para a ramificação mestre, o conteúdo do arquivo *layout. cshtml de\_* será revertido automaticamente para o que eles estão no Branch mestre.

![Branch mestre](source-control/_static/image19.png)

Este é um exemplo simples de como você pode criar rapidamente uma ramificação e alternar entre branches. Esse recurso permite um fluxo de trabalho altamente ágil usando a estrutura de ramificação e os scripts de automação apresentados no capítulo [automatizar tudo](automate-everything.md) . Por exemplo, você pode estar trabalhando na ramificação de desenvolvimento, criar um Branch de correção automática do mestre, alternar para o novo Branch, fazer suas alterações e confirmá-las e, em seguida, voltar para a ramificação de desenvolvimento e continuar o que estava fazendo.

O que você viu aqui é como trabalhar com um repositório git local no Visual Studio. Em um ambiente de equipe, você normalmente também envia por push as alterações para um repositório comum. As ferramentas do Visual Studio também permitem que você aponte para um repositório git remoto. Você pode usar o GitHub.com para essa finalidade ou pode usar o [git e Azure Repos](/azure/devops/repos/git/overview?view=vsts) integrado com todos os outros recursos de DevOps do Azure, como item de trabalho e acompanhamento de bugs.

Isso não é a única maneira de implementar uma estratégia de ramificação ágil, é claro. Você pode habilitar o mesmo fluxo de trabalho ágil usando um repositório de controle do código-fonte centralizado.

## <a name="summary"></a>Resumo

Meça o sucesso do sistema de controle do código-fonte com base na rapidez com que você pode fazer uma alteração e colocá-la em uma maneira segura e previsível. Se você se deparar com a medo para fazer uma alteração porque precisa fazer um dia ou dois testes manuais nele, você pode se perguntar o que precisa fazer de processo ou de teste para que possa fazer essa alteração em minutos ou, no máximo, não mais do que uma hora. Uma estratégia para fazer isso é implementar a integração contínua e a entrega contínua, que abordaremos no [próximo capítulo](continuous-integration-and-continuous-delivery.md).

<a id="resources"></a>
## <a name="resources"></a>Recursos

Para obter mais informações sobre estratégias de ramificação, consulte os seguintes recursos:

- [Criando um pipeline de versão com o Team Foundation Server 2012](https://msdn.microsoft.com/library/dn449957.aspx). Documentação de práticas e padrões da Microsoft. Consulte o capítulo 6 para obter uma discussão sobre estratégias de ramificação. Defende as alternâncias de recursos em relação a ramificações de recursos e, se as ramificações para recursos forem usadas, defendem mantê-las de curta duração (horas ou dias no máximo).
- [Guia de controle de versão](https://aka.ms/vsarsolutions). Guia para as estratégias de ramificação dos ALM Rangers. Consulte a ramificação Strategies. pdf na guia downloads.
- [Desenvolvimento de software com alternâncias de recursos](https://msdn.microsoft.com/magazine/dn683796.aspx). Artigo da MSDN Magazine.
- [Alternância de recursos](http://martinfowler.com/bliki/FeatureToggle.html). Introdução aos sinalizadores de recurso/alternâncias de recursos no blog de Martin Fowler.
- [Alternância de recursos vs. branches de recursos](http://geekswithblogs.net/Optikal/archive/2013/02/10/152069.aspx). Outra postagem de blog sobre alternâncias de recursos, de Dylan Smith.

Para obter mais informações sobre como lidar com informações confidenciais que não devem ser mantidas em repositórios de controle do código-fonte, consulte os seguintes recursos:

- [Práticas recomendadas para implantar senhas e outros dados confidenciais no ASP.net e no serviço de Azure app](../../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
- [Sites do Azure: como as cadeias de caracteres do aplicativo e de conexão funcionam](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/). Explica o recurso do Azure que substitui `appSettings` e `connectionStrings` dados no arquivo *Web. config* .
- [Configuração personalizada e configurações de aplicativo nos sites do Azure-com Stefan Schackow](https://azure.microsoft.com/documentation/videos/configuration-and-app-settings-of-azure-web-sites/).

Para obter informações sobre outros métodos para manter informações confidenciais fora do controle do código-fonte, consulte [ASP.NET MVC: manter as configurações particulares do controle do código-fonte](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).

> [!div class="step-by-step"]
> [Anterior](automate-everything.md)
> [Próximo](continuous-integration-and-continuous-delivery.md)
