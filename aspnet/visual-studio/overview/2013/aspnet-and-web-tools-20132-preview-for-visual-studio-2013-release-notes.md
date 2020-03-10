---
uid: visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
title: ASP.NET and Web Tools 2013,2 para notas de versão do Visual Studio 2013 | Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 03/06/2014
ms.assetid: 7ef5f73c-ca60-43c1-bdb2-702800347e7e
msc.legacyurl: /visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
msc.type: authoredcontent
ms.openlocfilehash: 22d4d4afd6963f23d6cfef1745a859c20b69d599
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78623189"
---
# <a name="aspnet-and-web-tools-20132--for-visual-studio-2013-release-notes"></a>Notas de versão do ASP.NET and Web Tools 2013.2 para Visual Studio 2013

pela [Microsoft](https://github.com/microsoft)

## <a name="installation-notes"></a>Notas de instalação

ASP.NET and Web Tools para Visual Studio 2013,2 são agrupadas no instalador principal e podem ser baixadas como parte do [Visual Studio 2013 atualização 2](https://go.microsoft.com/fwlink/?LinkId=390521).

## <a name="documentation"></a>Documentação

Os tutoriais e outras informações sobre ASP.NET and Web Tools para o Visual Studio 2013,2 estão disponíveis no [site do ASP.net](https://www.asp.net/).

## <a name="software-requirements"></a>Requisitos de software

O ASP.NET and Web Tools para Visual Studio 2013,2 requer Visual Studio 2013.

## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-20132"></a>Novos recursos no ASP.NET and Web Tools para Visual Studio 2013,2

As seções a seguir descrevem os recursos que foram introduzidos na versão.

- [Um modelo de projeto do ASP.NET](#oneaspnet)
- [Suporte a SSL ao iniciar aplicativos Web no IIS Express](#ssl)
- [Aprimoramentos do editor da Web do Visual Studio](#vswebeditor)
- [Link do navegador](#browserlink)
- [Suporte para aplicativos Web do serviço Azure App no Visual Studio](#waws)
- [Criar recursos remotos do Azure ao criar um novo projeto Web](#AzureResources)
- [Aprimoramentos de publicação na Web](#webpublish)
- [ASP.NET scaffolding](#scaffolding)
- [NuGet 2.8.1](#nuget)
- [Web Forms ASP.NET](#webforms)
- [ASP.NET MVC 5.1.2](#mvc)
- [ASP.NET Web API 2.1.2](#webapi)
- [Páginas da Web do ASP.NET 3.1.2](#webpages)
- [Entity Framework 6,1](#ef)
- [ASP.NET Identity 2.0.0](#identity)
- [Componentes do Microsoft OWIN](#owin)
- [ASP.NET Signalr 2.0.2](#signalr)

<a id="oneaspnet"></a>
### <a name="one-aspnet-project-templates"></a>Um modelo de projeto do ASP.NET

- Atualizações para modelos de projeto ASP.NET para dar suporte à confirmação da conta e à redefinição de senha.
- Atualize ASP.NET Web API modelo para dar suporte à autenticação usando contas organizacionais locais.
- O modelo SPA ASP.NET agora contém autenticação baseada em exibições do MVC e do lado do servidor. O modelo tem um controlador WebAPI que só pode ser acessado por usuários autenticados.

<a id="ssl"></a>
### <a name="support-ssl-when-launching-web-applications-on-iis-express"></a>Suporte a SSL ao iniciar aplicativos Web no IIS Express

Para eliminar o aviso de segurança durante a navegação e a depuração de HTTPS no localhost, adicionamos uma caixa de diálogo para permitir que o Internet Explorer e o Chrome confiem no certificado SSL expresso do IIS Express autoassinado.

Por exemplo, uma propriedade de projeto Web pode ser definida para usar SSL. Clique em F4 para exibir a caixa de diálogo Propriedades. Altere **SSL habilitado** para verdadeiro. Copie a URL do SSL.

![Propriedade SSL habilitada](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image1.png)

Defina a guia Web da página de propriedades do projeto Web para usar a URL baseada em HTTPS (a URL SSL será `https://localhost:44300/`, a menos que você tenha criado sites SSL anteriormente).

![Definir URL do projeto (HTTPS)](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image2.png)

Pressione CTRL+F5 para executar o aplicativo. Siga as instruções para confiar no certificado autoassinado que IIS Express gerou.

![Aviso SSL](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image3.png)

Leia a caixa de diálogo **aviso de segurança** e clique em **Sim** se desejar instalar o certificado que representa o localhost.

![Aviso de segurança](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image4.png)

O site será mostrado no IE ou no Chrome sem o aviso do certificado no navegador.

![Página HTTPS sem avisos](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image5.png)

O Firefox usa seu próprio repositório de certificados, portanto, ele exibirá um aviso.

<a id="vswebeditor"></a>
### <a name="visual-studio-web-editor-enhancements"></a>Aprimoramentos do editor da Web do Visual Studio

- **Novo editor e item de projeto JSON**: adicionamos um editor e item de projeto JSON ao Visual Studio. Os recursos atuais do editor JSON incluem colorização, validação de sintaxe, preenchimento de chave, estrutura de opções, configuração de opção de ferramentas e muito mais.

    ![JSON Editor](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image6.png)

    O IntelliSense agora dá suporte ao [esquema JSON](http://json-schema.org/) v3 e v4. Há uma caixa de combinação de esquema para escolher esquemas existentes, editar o caminho do esquema local ou simplesmente arrastar e soltar um arquivo JSON do projeto para ele para obter o caminho relativo.

    ![JSON IntelliSense](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image7.png)    ![Editor de esquema JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image8.png)
- **Novo editor de Sass (SCSS)** : adicionamos menos no VS2013 RTM, e agora temos um editor e item de projeto do Sass. Os recursos do editor do Sass são comparáveis ao editor LESS e incluem colorização, variável e mesclas IntelliSense, comentário/Remover comentário, informações rápidas, formatação, validação de sintaxe, estrutura de tópicos, definição de Goto, seletor de cores, configuração de opção de ferramentas, etc.

    ![Adicionar novo item: folha de estilos SCSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image9.png)    ![Editor de folhas de estilo](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image10.png)
- **Novo seletor de URL em documentos HTML, Razor, CSS, menos e Sass:** O VS 2013 foi fornecido sem nenhum seletor de URL fora das páginas Web Forms. O novo seletor de URL para editores HTML, Razor, CSS, LESS e Sass é um seletor de digitação Fluent sem caixa de diálogo que compreende '.. ' e filtra as listas de arquivos adequadamente para marcas e links img.

    ![Seletor de URL para marca de imagem](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image11.png)    ![Seletor de URL para exibições](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image12.png)    ![Seletor de URL para CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image13.png)
- **Atualizações para menos editor adicionando mais recursos**
- **Atualização do IntelliSense do Knockout**: adicionamos uma sintaxe de vazado não padrão para o vs IntelliSense, sintaxe "ko-vs-editor viewModel:". Ele pode ser usado para associar vários modelos de exibição em uma página usando comentários no formulário:

    ![IntelliSense do Knockout](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image14.png)

    Também adicionamos suporte para IntelliSense ViewModel aninhado, para que você possa analisar objetos profundamente aninhados no ViewModel.

    `<div data-bind="text: foo.bar.baz.etc" />`

    O IntelliSense exibido é o IntelliSense completo do objeto JavaScript.

    ![IntelliSense mostrando objeto JavaScript completo](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image15.png)
- **Novo seletor de URL em documentos HTML, Razor, CSS, menos e Sass**: vs 2013 fornecido sem nenhum seletor de URL fora das páginas Web Forms. O novo seletor de URL para editores HTML, Razor, CSS, LESS e Sass é um seletor de digitação Fluent sem caixa de diálogo que compreende '.. ' e filtra as listas de arquivos adequadamente para marcas e links img.

    ![Seletor de URL para marca de imagem](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image16.png)    ![Seletor de URL para exibições](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image17.png)    ![Seletor de URL para CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image18.png)

<a id="browserlink"></a>
### <a name="browser-link"></a>Link do navegador

- O link do navegador agora dá suporte a conexões HTTPS e listará isso no painel com outras conexões, desde que o certificado seja confiável pelo navegador.
- Mapeamento de origem HTML estático
- Suporte a SPA para mapeamento de dados
- Atualizar dados de mapeamento automaticamente

<a id="waws"></a>
### <a name="support-for-azure-app-service-web-apps-in-visual-studio"></a>Suporte para aplicativos Web do serviço Azure App no Visual Studio

- **Suporte à entrada do Azure.**
- **Depuração remota e exibição remota para aplicativos Web**: Agora damos suporte à [depuração remota para aplicativos web no serviço Azure app](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) e exibição remota de arquivos de conteúdo do aplicativo Web no Gerenciador de servidores.

<a id="AzureResources"></a>
### <a name="create-remote-azure-resources-when-creating-a-new-web-project"></a>Criar recursos remotos do Azure ao criar um novo projeto Web

Adicionamos uma caixa de seleção ["criar recursos remotos"](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) do Azure na caixa de diálogo novo aplicativo Web. Ao escolher, você poderá integrar a experiência de criação de um novo aplicativo Web, configurar o site de publicação do Azure para teste e criar o perfil de publicação em algumas etapas simples.

![Novo projeto com recursos do Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image19.png)![Publicando no Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image20.png)

<a id="webpublish"></a>
### <a name="web-publish-enhancements"></a>Aprimoramentos de publicação na Web

- Melhore a experiência do usuário para publicação.

<a id="scaffolding"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET scaffolding

- **Suporte a enum:** Se seu modelo estiver usando enums, o MVC Scaffolder gerará DropDown para enum. Isso usa os auxiliares de enumeração no MVC.
- **Suporte à inicialização**: os modelos EditorFor atualizados no MVC scaffolding para que usem as classes bootstrap.
- **Suporte a pacotes**: o MVC e a API Web Scaffolders adicionarão pacotes de 5,1 para MVC e API Web

As capturas de tela a seguir demonstram modelos scaffolding.

- Código do modelo:

     ![Código do modelo](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image21.png)
- Compile o código do modelo, clique com o botão direito do mouse e selecione **Adicionar**, **novo item com Scaffold**.

     ![Adicionar novo item com Scaffold](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image22.png)
- Escolha **controlador MVC5 com exibições, usando Entity Framework**:

     ![Adicionar novo controlador MVC5 com exibições](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image23.png)
- **Adicionar controlador** usando o modelo:

    ![](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image24.png)
- Verifique o código gerado; por exemplo, views/WeekdayModels/Edit. cshtml contém `@Html.EnumDropDownListFor`![exibição que contém EnumDropDownListFor](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image25.png)
- Execute a página para ver a ComboBox enum gerada, observe que, se um valor puder ser nulo, uma cadeia de caracteres vazia poderá ser escolhida para a caixa de combinação. Por exemplo, a página **criar** mostra o seguinte:

    ![Caixa de combinação permitindo cadeia de caracteres vazia](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image26.png)

<a id="nuget"></a>
### <a name="nuget-281"></a>NuGet 2.8.1

O NuGet 2.8.1 RTM será lançado em abril de 2014. Aqui estão os pontos evidentes das notas de versão, mas consulte as [notas de versão completas](http://docs.nuget.org/docs/release-notes/nuget-2.8) para obter mais informações sobre essas alterações.

- **Windows Phone aplicativos de destino 8,1**: o NuGet 2.8.1 agora dá suporte ao direcionamento de Windows Phone aplicativos 8,1 usando os monikers da estrutura de destino ' WindowsPhoneApp ', ' wpa ', ' WindowsPhoneApp81 ' e ' WPA81 '.

- **Resolução de patch para dependências**: ao resolver dependências de pacote, o NuGet implementou historicamente uma estratégia de seleção da versão mais baixa e secundária do pacote que satisfaz as dependências no pacote. No entanto, ao contrário da versão principal e secundária, a versão do patch foi sempre resolvida para a versão mais recente. Embora o comportamento tenha sido bem intencional, ele criou uma falta de determinantes para a instalação de pacotes com dependências.
- **Opção DependencyVersion**: embora o NuGet 2,8 altere o comportamento *padrão* para resolver dependências, ele também adiciona controle mais preciso sobre o processo de resolução de dependência por meio da opção-DependencyVersion no console do Gerenciador de pacotes. A opção habilita a resolução de dependências para a versão mais baixa possível (comportamento padrão), a versão mais alta possível ou a versão secundária ou de patch mais alta. Essa opção funciona apenas para Install-Package no comando do PowerShell.
- **Atributo DependencyVersion**: além da opção-DependencyVersion detalhadamente acima, o NuGet também permitiu a capacidade de definir um novo atributo no arquivo NuGet. config que define o valor padrão é, se a opção-DependencyVersion não for especificada em uma invocação do pacote de instalação. Esse valor também será respeitado pela caixa de diálogo Gerenciador de pacotes NuGet para qualquer operação de pacote de instalação. Para definir esse valor, adicione o atributo abaixo ao seu arquivo NuGet. config:

    `<config> <add key="dependencyversion" value="Highest" /> </config>`
- **Visualizar operações do NuGet com-WhatIf**: alguns pacotes NuGet podem ter grafos de dependência profunda e, dessa forma, podem ser úteis durante uma operação de instalação, desinstalação ou atualização para ver primeiro o que acontecerá. O NuGet 2,8 adiciona o padrão PowerShell – e se mudar para os comandos install-Package, Uninstall-Package e Update-Package para habilitar a visualização de todo o fechamento de pacotes ao qual o comando será aplicado.
- **Pacote de downgrade**: não é incomum instalar uma versão de pré-lançamento de um pacote para investigar novos recursos e, em seguida, decidir reverter para a última versão estável. Antes do NuGet 2,8, esse era um processo de várias etapas para desinstalar o pacote de pré-lançamento e suas dependências e, em seguida, instalar a versão anterior. Com o NuGet 2,8, no entanto, o Update-Package agora reverterá todo o fechamento do pacote (por exemplo, a árvore de dependência do pacote) para a versão anterior.
- **Dependências de desenvolvimento**: muitos tipos diferentes de recursos podem ser fornecidos como pacotes NuGet, incluindo ferramentas que são usadas para otimizar o processo de desenvolvimento. Esses componentes, embora possam ser fundamentais no desenvolvimento de um novo pacote, não devem ser considerados uma dependência do novo pacote quando ele for publicado posteriormente. O NuGet 2,8 permite que um pacote se identifique no arquivo. nuspec como um developmentDependency. Quando instalado, esses metadados também serão adicionados ao arquivo Packages. config do projeto no qual o pacote foi instalado. Quando esse arquivo Packages. config for posteriormente analisado para as dependências do NuGet durante o pacote NuGet. exe, ele excluirá as dependências marcadas como dependências de desenvolvimento.
- **Arquivos. config de pacotes individuais para diferentes plataformas**: ao desenvolver aplicativos para várias plataformas de destino, é comum ter arquivos de projeto diferentes para cada um dos respectivos ambientes de compilação. Também é comum consumir diferentes pacotes NuGet em arquivos de projeto diferentes, pois os pacotes têm níveis variados de suporte para diferentes plataformas. O NuGet 2,8 fornece suporte aprimorado para esse cenário criando arquivos Packages. config diferentes para diferentes arquivos de projeto específicos da plataforma.
- **Fallback para o cache local**: embora os pacotes NuGet normalmente são consumidos de uma galeria remota, como a [Galeria do NuGet](http://www.nuget.org) usando uma conexão de rede, há muitos cenários em que o cliente não está conectado. Sem uma conexão de rede, o cliente NuGet não conseguiu instalar pacotes com êxito, mesmo quando esses pacotes já estavam no computador do cliente no cache do NuGet local. O NuGet 2,8 adiciona o fallback de cache automático para o console do Gerenciador de pacotes.

    O recurso fallback de cache não requer nenhum argumento de comando específico. Além disso, o fallback de cache atualmente funciona apenas no console do Gerenciador de pacotes-o comportamento não funciona atualmente na caixa de diálogo Gerenciador de pacotes.
- **Correções de bugs**: uma das principais correções de bugs feitas foi a melhoria no desempenho do comando Update-Package-REINSTALL.

    Além desses recursos e da correção de desempenho mencionada anteriormente, esta versão do NuGet também inclui muitas outras correções de bugs. Havia 181 problemas totais abordados na versão. Para obter uma lista completa dos itens de trabalho corrigidos no NuGet 2,8, consulte o [rastreador de problemas do NuGet](https://nuget.codeplex.com/workitem/list/advanced?release=NuGet%202.8&status=all) para esta versão.

<a id="webforms"></a>
### <a name="aspnet-web-forms"></a>Web Forms do ASP.NET

- Os modelos de Web Forms agora mostram como fazer a confirmação da conta e a redefinição de senha para ASP.NET Identity.
- O controle da fonte de dados de entidade e o provedor de Dados Dinâmicos para Entity Framework 6. Para obter mais detalhes, consulte o seguinte blog do MSDN: [provedor de dados dinâmicos e controle de EntityDataSource para Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/01/30/announcing-preview-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx).

<a id="mvc"></a>
### <a name="aspnet-mvc-512"></a>ASP.NET MVC 5.1.2

- [Aprimoramentos de roteamento de atributos](../../../mvc/overview/releases/mvc51-release-notes.md#AttributeRouting)
- [Suporte de inicialização para modelos do editor](../../../mvc/overview/releases/mvc51-release-notes.md#Bootstrap)
- [Suporte de enumeração em exibições](../../../mvc/overview/releases/mvc51-release-notes.md#Enum)
- [Suporte não invasivo para atributos MinLength/MaxLength](../../../mvc/overview/releases/mvc51-release-notes.md#Unobtrusive)
- [Dando suporte ao contexto ' this ' em AJAX discreto](../../../mvc/overview/releases/mvc51-release-notes.md#thisContext)
- Várias [correções de bugs](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=MVC&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webapi"></a>
### <a name="aspnet-web-api-212"></a>ASP.NET Web API 2.1.2

- [Tratamento de erro global](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#global-error)
- [Aprimoramentos de roteamento de atributos](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#attribute-routing)
- [Aprimoramentos na página de ajuda](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#help-page)
- [Suporte do IgnoreRoute](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#ignoreroute)
- [Formatador de tipo de mídia BSON](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#bson)
- [Melhor suporte para filtros assíncronos](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#async-filters)
- [Análise de consulta para a biblioteca de formatação de cliente](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#query-parsing)
- Várias [correções de bugs](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20API%7cWeb%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webpages"></a>
### <a name="aspnet-web-pages-312"></a>Páginas da Web do ASP.NET 3.1.2

- Várias [correções de bugs](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20Pages/Razor&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="ef"></a>
### <a name="entity-framework-61"></a>Entity Framework 6,1

O Entity Framework foi atualizado para a versão 6,1 para tempo de execução e ferramentas. O Entity Framework (EF) 6,1 é uma pequena atualização para Entity Framework 6 e inclui uma série de correções de bugs e novos recursos. Para obter informações detalhadas sobre o EF 6.1, incluindo links para a documentação dos novos recursos, consulte [Entity Framework histórico de versão](https://msdn.microsoft.com/data/jj574253). Os novos recursos desta versão incluem:

- A **consolidação de ferramentas** fornece uma maneira consistente de criar um novo modelo do EF. Esse recurso estende o assistente de Modelo de Dados de Entidade de ADO.NET para dar suporte à criação de modelos de Code First, incluindo a engenharia reversa de um banco de dados existente. Esses recursos estavam disponíveis anteriormente em qualidade beta no EF Power Tools.
- A **manipulação de falhas de confirmação de transação** fornece o novo [System. Data. Entity. Infrastructure. CommitFailureHandler](https://msdn.microsoft.com/library/system.data.entity.infrastructure.commitfailurehandler(v=vs.113).aspx) que usa a capacidade introduzida recentemente para interceptar operações de transação. O **CommitFailureHandler** permite a recuperação automática de falhas de conexão durante a confirmação de uma transação.
- **Indexattribute** permite que os índices sejam especificados colocando um atributo em uma propriedade (ou Propriedades) em seu modelo de Code First. Code First criará um índice correspondente no banco de dados.
- **A API de mapeamento público** fornece acesso às informações que o EF tem sobre como as propriedades e os tipos são mapeados para colunas e tabelas no banco de dados. Em versões anteriores, essa API era interna.
- **Capacidade de configurar interceptores por meio do arquivo app/Web. config**(permitindo que os interceptores sejam adicionados sem recompilar o aplicativo).
- **DatabaseLogger** é um novo interceptador que torna mais fácil registrar em log todas as operações de banco de dados em um arquivo. Em combinação com o recurso anterior, isso permite que você alterne facilmente o log de operações de banco de dados para um aplicativo implantado, sem a necessidade de recompilar.
- A **detecção de alteração do modelo de migrações** foi aprimorada para que as migrações de com Scaffold sejam mais precisas; o desempenho do processo de detecção de alteração também foi muito aprimorado.
- **Melhorias de desempenho** , incluindo operações de banco de dados reduzidas durante a inicialização, otimizações para comparação de igualdade nula em consultas LINQ, geração de exibição mais rápida (criação de modelo) em mais cenários e materialização mais eficiente de entidades rastreadas com várias associações.

<a id="identity"></a>
### <a name="aspnet-identity-200"></a>ASP.NET Identity 2.0.0

- **Autenticação de dois fatores**: o ASP.net Identity agora dá suporte à autenticação de dois fatores. A autenticação de dois fatores fornece uma camada extra de segurança para suas contas de usuário no caso em que a senha é comprometida. Também há proteção para ataques de força bruta contra os códigos de dois fatores.
- **Bloqueio de conta:** Fornece uma maneira de bloquear o usuário se o usuário inserir sua senha ou códigos de dois fatores incorretamente. O número de tentativas inválidas e o período de tempo para os usuários estão bloqueados podem ser configurados. Um desenvolvedor pode opcionalmente desativar o bloqueio de conta para determinadas contas de usuário, caso seja necessário.
- **Confirmação da conta:** O sistema de ASP.NET Identity agora dá suporte à confirmação da conta. Esse é um cenário bastante comum na maioria dos sites hoje em que, quando você se registra para uma nova conta no site, é necessário confirmar seu email para poder fazer qualquer coisa no site. A confirmação por email é útil porque impede que contas falsas sejam criadas. Isso é extremamente útil se você estiver usando email como um método de comunicação com os usuários do seu site, como sites de fórum, serviços bancários, comércio eletrônico ou sites sociais.
- **Redefinição de senha:** A redefinição de senha é um recurso em que o usuário pode redefinir suas senhas se tiver esquecido sua senha.
- **Carimbo de segurança (sair em qualquer lugar):** Dá suporte a uma maneira de regenerar o token de segurança para o usuário em casos em que o usuário altera sua senha ou qualquer outra informação relacionada à segurança, como remover um logon associado (como Facebook, Google, conta da Microsoft e assim por diante). Isso é necessário para garantir que todos os tokens gerados com a senha antiga sejam invalidados. No projeto de exemplo, se você alterar a senha do usuário, um novo token será gerado para o usuário e quaisquer tokens anteriores serão invalidados. Esse recurso fornece uma camada extra de segurança para seu aplicativo desde que, quando você alterar sua senha, você será desconectado de todos os lugares (todos os outros navegadores) em que você fez logon neste aplicativo.
- **Tornar o tipo de chave primária extensível para usuários e funções**: no ASP.net Identity 1,0, o tipo de chave primária para usuários e funções de tabela era cadeia de caracteres. Isso significa que quando o sistema de ASP.NET Identity persistiu em SQL Server usando Entity Framework, estávamos usando nvarchar. Havia muitas discussões sobre essa implementação padrão no Stack Overflow e com base nos comentários recebidos. Fornecemos um gancho de extensibilidade onde você pode especificar o que deve ser a chave primária da tabela Users e Roles. Esse gancho de extensibilidade é particularmente útil se você estiver migrando seu aplicativo e o aplicativo estava armazenando UserIds são GUIDs ou ints.
- **Suporte a IQueryable em usuários e funções**: suporte adicionado para IQueryable em UsersStore e RolesStore, você pode obter facilmente a lista de usuários e funções.
- **Suporte à operação de exclusão por meio do usermanager**
- **Indexação em nome de usuário**: na implementação ASP.net Identity Entity Framework, adicionamos um índice exclusivo no nome de usuário usando o novo indexattribute no EF 6.1.0. Isso garante que os nomes de usersejam sempre exclusivos e não haja nenhuma condição de corrida na qual você possa acabar com nomes de userduplicados.
- **Validador de senha aprimorado:** O validador de senha enviado no ASP.NET Identity 1,0 foi um validador de senha bastante básico que estava Validando apenas o comprimento mínimo. Há um novo validador de senha que oferece mais controle sobre a complexidade da senha. Observe que, mesmo que você ative todas as configurações nesta senha, incentivamos você a habilitar a autenticação de dois fatores para as contas de usuário.
- **Middleware IdentityFactory/CreatePerOwinContext**:

    - **Gerenciador de usuários**: você pode usar a implementação de fábrica para obter uma instância de usermanager do contexto OWIN. Esse padrão é semelhante ao que usamos para obter CustomTargetNameDictionary do contexto OWIN para Signe e SignOut. Essa é uma maneira recomendada de obter uma instância de usermanager por solicitação para o aplicativo.
    - **DbContextFactory**: o ASP.NET Identity usa Entity Framework para persistir o sistema de identidade no SQL Server. Para fazer isso, o sistema de identidade tem uma referência ao ApplicationDbContext. O middleware DbContextFactory retorna uma instância do ApplicationDbContext por solicitação que você pode usar em seu aplicativo.
- **Pacote NuGet de exemplos de ASP.net Identity**: o pacote NuGet de exemplos pode facilitar a instalação e a execução de exemplos para ASP.net Identity e seguir as práticas recomendadas. Este é um aplicativo ASP.NET MVC de exemplo. Modifique o código para se adequar ao seu aplicativo antes de implantá-lo em produção. O exemplo deve ser instalado em um aplicativo ASP.NET vazio. Para obter mais informações sobre o pacote, acesse a seguinte postagem no blog: [anunciando a versão RTM do ASP.net Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx)

<a id="owin"></a>
### <a name="microsoft-owin-components"></a>Componentes Microsoft OWIN

Houve muitos bugs que foram corrigidos nesta versão. Consulte as [notas de versão da versão 2.1.0](https://katanaproject.codeplex.com/releases/view/113281) para obter informações mais detalhadas.

<a id="signalr"></a>
### <a name="aspnet-signalr-202"></a>ASP.NET Signalr 2.0.2

Houve muitos bugs que foram corrigidos nesta versão. Consulte as [notas de versão da versão 2.0.2](https://github.com/SignalR/SignalR/releases/tag/2.0.2) para obter informações mais detalhadas.
