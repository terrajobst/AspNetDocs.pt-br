---
uid: web-forms/overview/deployment/visual-studio-web-deployment/introduction
title: 'Implantação da Web do ASP.NET usando o Visual Studio: introdução | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um aplicativo Web ASP.NET em aplicativos Web do serviço Azure App ou em um provedor de Hospedagem de terceiros, usando o V...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 24ad086d-865e-433c-9ac9-05f1a553da16
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/introduction
msc.type: authoredcontent
ms.openlocfilehash: 96dd31d949633e001fc595621bedbf74e98000fc
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74640230"
---
# <a name="aspnet-web-deployment-using-visual-studio-introduction"></a>Implantação da Web do ASP.NET usando o Visual Studio: introdução

por [Tom Dykstra](https://github.com/tdykstra)

[Baixar o projeto inicial](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta série de tutoriais mostra como implantar (publicar) um aplicativo Web ASP.NET em aplicativos Web do serviço Azure App ou em um provedor de Hospedagem de terceiros, usando o Visual Studio 2012 com o SDK do Azure para .NET. A maioria dos procedimentos é semelhante para Visual Studio 2013.
> 
> Você desenvolve um aplicativo Web para torná-lo disponível para pessoas pela Internet. Mas os tutoriais de programação da Web normalmente são interrompidos logo após terem mostrado como fazer algo funcionando no seu computador de desenvolvimento. Esta série de tutoriais começa onde os outros deixam de sair: você criou um aplicativo Web, testou-o e está pronto para começar. O que vem a seguir? Esses tutoriais mostram como implantar primeiro no IIS em seu computador de desenvolvimento local para teste e, em seguida, para o Azure ou um provedor de Hospedagem de terceiros para preparo e produção. O aplicativo de exemplo que você implantará é um projeto de aplicativo Web que usa o Entity Framework, SQL Server e o sistema de associação ASP.NET. O aplicativo de exemplo usa o ASP.NET Web Forms, mas os procedimentos mostrados se aplicam também ao MVC e à API da Web do ASP.NET.
> 
> Esses tutoriais pressupõem que você saiba como trabalhar com o ASP.NET no Visual Studio. Se você não fizer isso, um bom lugar para começar é um [tutorial básico de Web Forms ASP.net](../../older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1.md) ou um [tutorial ASP.NET MVC básico](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).
> 
> Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, você poderá lançá-las no [Fórum de implantação do ASP.net](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) ou [StackOverflow](http://stackoverflow.com).
> 
> Esse conteúdo também está disponível como um livro eletrônico gratuito na [Galeria de livros eletrônicos do TechNet](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#ASPNETWebDeploymentusingVisualStudio).

## <a name="overview"></a>{1&gt;Visão Geral&lt;1}

Esses tutoriais orientam você pela implantação de um aplicativo Web ASP.NET que inclui bancos de dados SQL Server. Você implantará primeiro no IIS em seu computador de desenvolvimento local para teste e, em seguida, em aplicativos Web no serviço de Azure App e no banco de dados SQL do Azure para preparo e produção. Você verá como implantar usando a publicação de um clique do Visual Studio e verá como implantar usando a linha de comando.

O número de tutoriais pode fazer com que o processo de implantação pareça assustador. Na verdade, os procedimentos básicos são simples. No entanto, em situações do mundo real, muitas vezes você precisa fazer tarefas de implantação adicionais, por exemplo, definir permissões de pasta no servidor de destino. Ilustramos algumas dessas tarefas adicionais, na esperança de que os tutoriais não saiam de informações que possam impedi-lo de implantar com êxito um aplicativo real.

Os tutoriais são projetados para serem executados em sequência e cada parte se baseia na parte anterior. Você pode ignorar partes que não são relevantes para sua situação, mas talvez precise ajustar os procedimentos em Tutoriais posteriores.

## <a name="intended-audience"></a>Público-alvo

Os tutoriais se destinam a desenvolvedores de ASP.NET que trabalham em ambientes em que:

- O ambiente de produção é Azure App aplicativos Web do serviço ou um provedor de Hospedagem de terceiros.
- A implantação não está limitada a um processo de integração contínua, mas pode ser feita diretamente do Visual Studio.

A implantação do [controle do código-fonte](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md) usando um processo de [entrega contínua](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) não é abordada nesses tutoriais, exceto por um tutorial que mostra como implantar a partir da linha de comando. Para obter informações sobre a entrega contínua, consulte os seguintes recursos:

- [Integração contínua e entrega contínua (criando aplicativos de nuvem do mundo real com o Windows Azure)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md)
- [Implantar um aplicativo Web no serviço Azure App](https://azure.microsoft.com/documentation/articles/web-sites-deploy/)
- [Implantação de aplicativos Web em cenários empresariais](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md) (um conjunto mais antigo de tutoriais escritos para o Visual Studio 2010, que ainda tem informações úteis para ambientes empresariais.)

## <a name="using-a-third-party-hosting-provider"></a>Usando um provedor de Hospedagem de terceiros

Os tutoriais guiam você pelo processo de configuração de uma conta do Azure e da implantação do aplicativo em aplicativos Web no serviço Azure App para preparo e produção. No entanto, você pode usar os mesmos procedimentos básicos para implantar o para um provedor de Hospedagem de terceiros de sua escolha. Em que os tutoriais passam por processos exclusivos do Azure, eles explicam e aconselham as diferenças que você pode esperar em um provedor de Hospedagem de terceiros.

## <a name="deploying-web-app-projects"></a>Implantando projetos de aplicativo Web

O aplicativo de exemplo que você baixa e implanta para esses tutoriais é um projeto de aplicativo Web do Visual Studio. No entanto, se você instalar a atualização de publicação da Web mais recente para o Visual Studio, poderá usar os mesmos métodos e ferramentas de implantação para projetos de aplicativo Web.

## <a name="deploying-aspnet-mvc-projects"></a>Implantando projetos do ASP.NET MVC

O aplicativo de exemplo é um projeto ASP.NET Web Forms, mas tudo o que você aprende como fazer também se aplica ao MVC do ASP.NET. Um projeto do MVC do Visual Studio é apenas outra forma de projeto de aplicativo Web. A única diferença é que, se você estiver implantando em um provedor de hospedagem que não dá suporte ao ASP.NET MVC ou à sua versão de destino, certifique-se de ter instalado o pacote NuGet apropriado ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0), [MVC 4](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/4.0.30506.0)ou [MVC 5](http://www.nuget.org/packages/Microsoft.AspNet.Mvc)) em seu projeto.

## <a name="programming-language"></a>Linguagem de programação

O aplicativo de exemplo C# usa, mas os tutoriais não exigem conhecimento C#do, e as técnicas de implantação mostradas pelos tutoriais não são específicas do idioma.

## <a name="database-deployment-methods"></a>Métodos de implantação de banco de dados

Há três maneiras de implantar um banco de dados SQL Server juntamente com a implantação da Web no Visual Studio:

- Migrações do Entity Framework Code First
- O provedor de Implantação da Web dbDacFx
- O provedor de Implantação da Web dbFullSql

Neste tutorial, você usará os dois primeiros desses métodos. O provedor de Implantação da Web dbFullSql é um método herdado que não é mais recomendado, exceto para alguns cenários específicos, como migrar de SQL Server Compact para SQL Server.

Os métodos mostrados neste tutorial são para bancos de dados SQL Server, não SQL Server Compact. Para obter informações sobre como implantar um banco de dados SQL Server Compact, consulte [implantação da Web do Visual Studio com SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md).

Os métodos mostrados neste tutorial exigem que você use o método de publicação Implantação da Web. Se você preferir um método de publicação diferente, como FTP, sistema de arquivos ou FPSE, consulte [implantando um banco de dados separadamente da implantação de aplicativo Web](https://go.microsoft.com/fwlink/p/?LinkId=282413#databaseseparate) no mapa de conteúdo de implantação da Web para Visual Studio e ASP.net.

### <a name="entity-framework-code-first-migrations"></a>Migrações do Entity Framework Code First

Na versão Entity Framework 4,3, a Microsoft introduziu o Migrações do Code First. Migrações do Code First automatiza o processo de fazer alterações incrementais em um modelo de dados e propagar essas alterações para o banco de dado. Em versões anteriores do Code First, você geralmente permite que o Entity Framework descarte e recrie o banco de dados sempre que alterar o modelo. Isso não é um problema no desenvolvimento porque os dados de teste são facilmente recriados, mas em produção geralmente você deseja atualizar o esquema de banco sem remover o banco de dados. O recurso de migrações permite que Code First atualize o banco de dados sem descartar e recriá-lo. Você pode permitir que Code First decida automaticamente como fazer as alterações de esquema necessárias ou pode escrever um código que personalize as alterações. Para obter uma introdução ao Migrações do Code First, consulte [migrações do Code First](https://msdn.microsoft.com/library/hh770484.aspx).

Quando você está implantando um projeto Web, o Visual Studio pode automatizar o processo de implantação de um banco de dados gerenciado pelo Migrações do Code First. Ao criar o perfil de publicação, você marca uma caixa de seleção com o rótulo executar Migrações do Code First (é executado no início do aplicativo). Essa configuração faz com que o processo de implantação configure automaticamente o arquivo Web. config do aplicativo no servidor de destino para que Code First use a classe de inicializador `MigrateDatabaseToLatestVersion`.

O Visual Studio não faz nada com o banco de dados durante o processo de implantação. Quando o aplicativo implantado acessa o banco de dados pela primeira vez após a implantação, Code First cria automaticamente o banco de dados ou atualiza o esquema de banco de dados para a versão mais recente. Se o aplicativo implementar um método de semente de migrações, o método será executado depois que o banco de dados for criado ou o esquema for atualizado.

Neste tutorial, você usará Migrações do Code First para implantar o banco de dados do aplicativo.

### <a name="the-dbdacfx-web-deploy-provider"></a>O provedor de Implantação da Web dbDacFx

Para um banco de dados SQL Server que não é gerenciado pelo Entity Framework Code First, você pode marcar uma caixa de seleção que é rotulada como atualizar banco de dados ao configurar o perfil de publicação. Durante a implantação inicial, o provedor dbDacFx cria tabelas e outros objetos de banco de dados no banco de dados de destino para corresponder ao banco de dados de origem. Em implantações subsequentes, o provedor determina o que é diferente entre os bancos de dados de origem e de destino e atualiza o esquema do banco de dados de destino para corresponder ao banco de dados de origem. Por padrão, o provedor não fará nenhuma alteração que cause perda de dados, como quando uma tabela ou coluna for descartada.

Esse método não automatiza a implantação de dados em tabelas de banco de dado, mas você pode criar scripts para fazer isso e configurar o Visual Studio para executá-los durante a implantação. Outro motivo para executar scripts durante a implantação é fazer alterações de esquema que não podem ser feitas automaticamente, pois isso causaria perda de dados.

Neste tutorial, você usará o provedor dbDacFx para implantar o banco de dados de associação do ASP.NET.

## <a name="troubleshooting-during-this-tutorial"></a>Solução de problemas durante este tutorial

Quando ocorrer um erro durante a implantação ou se o site implantado não for executado corretamente, as mensagens de erro nem sempre fornecerão uma solução óbvia. Para ajudá-lo com alguns cenários de problema comuns, uma [página de referência de solução de problemas](troubleshooting.md) está disponível. Se você receber uma mensagem de erro ou algo não funcionar enquanto percorre os tutoriais, certifique-se de verificar a página de solução de problemas.

## <a name="comments-welcome"></a>Comentários-boas-vindas

Os comentários sobre os tutoriais são bem-vindos e, quando o tutorial for atualizado, todos os esforços serão feitos para levar em conta as correções ou sugestões para melhorias fornecidas nos comentários do tutorial.

<a id="prerequisites"></a>

## <a name="prerequisites"></a>{1&gt;{2&gt;Pré-requisitos&lt;2}&lt;1}

Este tutorial foi escrito para os seguintes produtos:

- Windows 8 ou Windows 7.
- Visual Studio 2012 ou Visual Studio 2012 Express para Web com [a atualização mais recente](https://go.microsoft.com/fwlink/?LinkId=272486).
- [SDK do Azure para Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=254364)

Você pode seguir o tutorial usando o Visual Studio 2010 SP1 ou Visual Studio 2013, mas algumas capturas de tela serão diferentes e alguns recursos serão diferentes.

Se você estiver usando Visual Studio 2013, instale o [SDK do Azure para Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkID=324322).

Se você estiver usando o Visual Studio 2010 SP1, instale o seguinte software:

- [SDK do Azure para Visual Studio 2010](https://go.microsoft.com/fwlink/?LinkID=254269)
- [SQL Server Express LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=SQLLocalDBOnly_11_0)
- [SQL Server Data Tools](https://msdn.microsoft.com/library/hh500335.aspx).

Dependendo de quantas dependências do SDK você já tem em seu computador, a instalação do SDK do Azure pode levar muito tempo, de vários minutos a meia hora ou mais. Você precisará do SDK do Azure mesmo se planeja publicar em um provedor de Hospedagem de terceiros, em vez de no Azure, porque o SDK inclui as atualizações mais recentes dos recursos de publicação na Web do Visual Studio.

> [!NOTE]
> Este tutorial foi escrito com a versão 1.8.1 do SDK do Azure. Desde então, as versões mais recentes com recursos adicionais foram lançadas. Os tutoriais foram atualizados para mencionar esses recursos e vincular a recursos que têm mais informações sobre eles.

As instruções e capturas de tela baseiam-se no Windows 8, mas os tutoriais explicam as diferenças do Windows 7.

Algum outro software é necessário para concluir o tutorial, mas você ainda não precisa ter isso instalado. O tutorial irá orientá-lo pelas etapas para instalá-lo quando necessário.

## <a name="download-the-sample-application"></a>Baixar o aplicativo de exemplo

O aplicativo que você implantará é chamado de Contoso University e já foi criado para você. É uma versão simplificada de um site da Universidade, com base no aplicativo da Contoso University descrito nos tutoriais de [Entity Framework no site do ASP.net](https://asp.net/entity-framework/tutorials).

Quando tiver os pré-requisitos instalados, baixe o [aplicativo Web da Contoso University](https://go.microsoft.com/fwlink/p/?LinkId=282627). O arquivo *. zip* contém várias versões do projeto. Para trabalhar nas etapas do tutorial, comece com o projeto localizado na C# pasta. Para ver a aparência do projeto no final dos tutoriais, abra o projeto na pasta ContosoUniversity-end.

Para preparar o projeto para trabalhar nas etapas do tutorial, execute as seguintes etapas:

1. Salve os arquivos da solução ContosoUniversity da C# pasta em uma pasta chamada ContosoUniversity em qualquer pasta usada para trabalhar com projetos do Visual Studio.

    Por padrão, esta é a seguinte pasta para o Visual Studio 2012:

    `C:\Users\<username>\Documents\Visual Studio 2012\Projects`

    (Para as capturas de tela deste tutorial, a pasta do projeto está localizada no diretório raiz na unidade `C`:.)
2. Inicie o Visual Studio e abra o projeto.
3. Em **Gerenciador de soluções**, clique com o botão direito do mouse na solução e clique em **EnableNuGet pacote de restauração**.
4. {1&gt;Compile a solução.&lt;1}
5. Se você receber erros de compilação, restaure manualmente os pacotes NuGet:

    1. No **Gerenciador de soluções**, clique com o botão direito do mouse na solução e clique em **gerenciar pacotes NuGet para solução**.
    2. Na parte superior da caixa de diálogo **gerenciar pacotes NuGet** , você verá que **alguns pacotes NuGet estão ausentes dessa solução. Clique para restaurar.** Clique no botão **restaurar** .
    3. Recompile a solução.
6. Pressione CTRL-F5 para executar o aplicativo.

    O aplicativo é aberto para a Contoso University home page.

    ![Desenvolvimento de Home Page](introduction/_static/image1.png)

    (Pode haver um tempo de espera enquanto o Visual Studio inicia a instância de LocalDB SQL Server Express, e você poderá obter um erro de tempo limite se o processo demorar muito. Nesse caso, basta iniciar o projeto.)

As páginas do site podem ser acessadas na barra de menus e permitem que você execute as seguintes funções:

- Exibir estatísticas de alunos (a página sobre).
- Exibir, editar, excluir e adicionar alunos.
- Exibir e editar cursos.
- Exibir e editar instrutores.
- Exibir e editar departamentos.

Veja a seguir as capturas de tela de algumas páginas representativas.

![Desenvolvimento de página de alunos](introduction/_static/image2.png)

![Adicionar desenvolvedor de páginas de alunos](introduction/_static/image3.png)

## <a name="review-application-features-that-affect-deployment"></a>Examinar os recursos do aplicativo que afetam a implantação

Os seguintes recursos do aplicativo afetam a maneira como você o implanta ou o que você precisa fazer para implantá-lo. Cada um deles é explicado com mais detalhes nos seguintes tutoriais da série.

- A Contoso University usa um banco de dados de SQL Server para armazenar aplicativos de aplicativo, como nomes de alunos e instrutores. O banco de dados contém uma combinação de dado de teste e dados de produção e, ao implantar na produção, você precisa excluir os dados de teste.
- O aplicativo usa o sistema de associação ASP.NET, que armazena informações de conta de usuário em um banco de dados SQL Server. O aplicativo define um usuário administrador que tem acesso a algumas informações restritas. Você precisa implantar o banco de dados de associação sem contas de teste, mas com uma conta de administrador.
- O aplicativo usa um utilitário de relatório e registro em log de erros de terceiros. Esse utilitário é fornecido em um assembly que deve ser implantado com o aplicativo.
- O utilitário de log de erros grava informações de erro em arquivos XML em uma pasta de arquivos. Você precisa certificar-se de que a conta que ASP.NET é executada no site implantado tem permissão de gravação para essa pasta, e você precisa excluir essa pasta da implantação. (Caso contrário, os dados de log de erros do ambiente de teste podem ser implantados em arquivos de log de erros de produção e/ou de produção podem ser excluídos.)
- O aplicativo inclui algumas configurações que devem ser alteradas no arquivo *Web. config* implantado dependendo do ambiente de destino (teste, preparo ou produção) e outras configurações que devem ser alteradas dependendo da configuração da compilação (depuração ou versão).
- A solução do Visual Studio inclui um projeto de biblioteca de classes. Somente o assembly que este projeto gera deve ser implantado, não o projeto em si.

## <a name="summary"></a>Resumo

Neste primeiro tutorial da série, você baixou o projeto de exemplo do Visual Studio e revisou os recursos do site que afetam a forma como você implanta o aplicativo. Nos tutoriais a seguir, você se prepara para a implantação Configurando algumas dessas coisas para serem tratadas automaticamente. Outros que você cuida manualmente.

> [!div class="step-by-step"]
> [Avançar](preparing-databases.md)
