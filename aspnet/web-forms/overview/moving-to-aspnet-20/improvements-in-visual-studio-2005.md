---
uid: web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
title: Aprimoramentos no Visual Studio 2005 | Microsoft Docs
author: microsoft
description: O Visual Studio 2005 fornece aos desenvolvedores de aplicativos Web uma longa lista de aprimoramentos e aprimoramentos em projetos da Web.
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 72d90cd0-b3d9-454c-b2eb-ed0d9812f32c
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
msc.type: authoredcontent
ms.openlocfilehash: 64215d556ded0850537a13856fe69b094116ebca
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78575575"
---
# <a name="improvements-in-visual-studio-2005"></a>Aprimoramentos no Visual Studio 2005

pela [Microsoft](https://github.com/microsoft)

> O Visual Studio 2005 fornece aos desenvolvedores de aplicativos Web uma longa lista de aprimoramentos e aprimoramentos em projetos da Web.

O Visual Studio 2005 fornece aos desenvolvedores de aplicativos Web uma longa lista de aprimoramentos e aprimoramentos em projetos da Web. Tão potente quanto o Visual Studio .NET 2002 e 2003 são, houve muitas reclamações na forma como os projetos da Web eram tratados. O Visual Studio 2005 adiciona um número significativo de novos recursos para atender a essas reclamações. Para aqueles que preferem a maneira como o Visual Studio .NET 2003 tratou de compilação de aplicativos Web, consulte [projetos de aplicativos Web](https://go.microsoft.com/fwlink/?LinkId=57870).

Neste módulo, aborde melhorias na criação, no gerenciamento e no desenvolvimento de projetos da Web. Em um módulo posterior, aborde bem as melhorias na criação de projetos da Web e na implantação deles.

## <a name="frontpage-server-extensions"></a>Extensões de servidor do FrontPage

O Visual Studio .NET 2002 e 2003 exigiam extensões de servidor do FrontPage na caixa para criar ou criar projetos Web. Os desenvolvedores têm uma opção entre dois modos de acesso diferentes (extensões de servidor do FrontPage ou modo de acesso a arquivo), ambos usavam extensões de servidor do FrontPage para executar tarefas como definir a raiz do aplicativo no IIS, etc.

O Visual Studio 2005 remove a dependência nas extensões de servidor do FrontPage para projetos locais. O Visual Studio 2005 agora acessa a metabase do IIS diretamente em vez de usar as extensões de servidor do FrontPage. O Visual Studio 2005 também adiciona suporte para FTP que permite o acesso ao projeto remoto sem a necessidade de extensões de servidor do FrontPage.

Para os desenvolvedores que desejam usar as extensões de servidor do FrontPage em seus projetos, a opção ainda estará disponível. No entanto, com base nos comentários fortes da comunidade de desenvolvedores do ASP.NET, não é um requisito.

> [!NOTE]
> As extensões de servidor do FrontPage ainda são necessárias para a criação, abertura, etc. do projeto remoto

## <a name="aspnet-development-server"></a>ASP.NET Development Server

O Visual Studio 2005 é fornecido com um novo servidor Web chamado ASP.NET Development Server. (Esse servidor Web era conhecido anteriormente como Cassini.)

Há vários benefícios do ASP.NET Development Server.

- Agora é possível que não administradores desenvolvam e depurem em um servidor Web.
- O ASP.NET Development Server mapeia dinamicamente os diretórios virtuais para qualquer local no sistema de arquivos, permitindo locais de projeto flexíveis.
- Os usuários do Windows XP Professional que já estiverem usando o IIS agora poderão criar novos aplicativos Web que não afetarão a estrutura de arquivos ou pastas do site padrão no IIS.

Nenhuma configuração especial é necessária para aproveitar o ASP.NET Development Server. Quando um projeto web hospedado no sistema de arquivos é depurado ou navegado, o Visual Studio 2005 iniciará automaticamente uma instância do ASP.NET Development Server em uma porta aleatória para atender à solicitação.

Mais informações serão abordadas na ASP.NET Development Server mais adiante neste módulo.

## <a name="improved-file-management"></a>Gerenciamento de arquivos aprimorado

No Visual Studio 2002 e 2003, um arquivo de projeto (. vbproj para VB.NET e. csproj C#para) armazenou informações em todos os arquivos no aplicativo Web. A exibição de Gerenciador de Soluções é baseada nas informações de arquivo no arquivo de projeto. Por isso, a Gerenciador de Soluções geralmente exibiria informações imprecisas em casos em que editores externos eram usados. O Visual Studio 2002 e 2003 muitas vezes substituem as alterações de arquivo ou não exibimos a versão mais recente dos arquivos.

O Visual Studio 2005 sai do arquivo de projeto. Em vez disso, ele lê as informações de arquivo e pasta diretamente do disco, resultando em uma exibição precisa dos arquivos em seu projeto. Como a pasta References no Visual Studio 2002 e 2003 não representa uma pasta real em seu aplicativo Web, o Visual Studio 2005 também remove a pasta References de Gerenciador de Soluções. Para acessar as referências para seu projeto no Visual Studio 2005, você deve usar as páginas de propriedades para o projeto.

## <a name="creating-web-projects"></a>Criando projetos Web

Os desenvolvedores da Web têm muitas novas opções disponíveis para a criação de projetos no Visual Studio 2005. Os sites agora podem ser criados em qualquer lugar no sistema de arquivos e, em seguida, podem ser depurados ou navegados usando o novo ASP.NET Development Server. Os desenvolvedores também podem criar novos sites usando FTP.

Clique aqui para exibir uma explicação em vídeo sobre como criar projetos Web no Visual Studio 2005.

![](improvements-in-visual-studio-2005/_static/image1.png)

[Abrir vídeo de tela inteira](improvements-in-visual-studio-2005/_static/creating_projects1.wmv)

### <a name="file-system-projects"></a>Projetos do sistema de arquivos

Como vimos no tutorial em vídeo, você pode optar por criar sites no sistema de arquivos no computador local ou em um local remoto por meio de um compartilhamento de arquivos. Os sites criados no sistema de arquivos são procurados e depurados usando o ASP.NET Development Server.

> [!NOTE]
> A ASP.NET Development Server pode causar alguma confusão aos clientes. Se um projeto Web for criado no sistema de arquivos na estrutura de diretório do IISs (ou seja, c:/inetpub/wwwroot), o site ainda será navegado por meio do ASP.NET Development Server quando iniciado no Visual Studio 2005. Portanto, qualquer configuração do IIS (ou seja, métodos de autenticação) não é aplicável.

O projeto Web padrão também remove uma grande parte da sobrecarga apenas inclui uma página default. aspx, um arquivo default.cs e uma pasta app/_Data. As pastas Web. config e especial (ou seja, app/_code) são adicionadas, pois são necessárias. Seu projeto Web inclui apenas os arquivos e pastas de que você precisa.

### <a name="http-projects"></a>Projetos HTTP

Os projetos HTTP podem ser projetos criados em um site local do IIS ou em um site remoto. O local do projeto padrão é `http://localhost`. Se você clicar no botão procurar, haverá duas opções de HTTP: IIS local e site remoto. A principal diferença nessas duas opções é o método no qual as informações do site são exibidas na caixa de diálogo escolher local e no modo como os arquivos são copiados para o servidor Web.

A opção IIS local lê as informações do site da metabase no computador local e os arquivos são copiados usando o sistema de arquivos. A opção site remoto usa as extensões de servidor do FrontPage e as informações e os arquivos do site são copiados usando chamadas RPC de extensões de servidor HTTP e FrontPage.

> [!NOTE]
> O arquivo vs # # #/_tmp. htm e Get/_aspx/_ver. aspx não são mais usados para determinar informações de versão.

A opção HTTP padrão é IIS local. Essa opção lê a metabase do IIS para determinar quais sites estão disponíveis e o local no qual criar conteúdo. Você pode selecionar uma pasta ou um diretório virtual diferente selecionando-o no modo de exibição de árvore. Você também pode criar um novo diretório virtual, marcar pastas como aplicativos, bem como excluir diretórios virtuais existentes dessa caixa de diálogo.

![A caixa de diálogo escolher local](improvements-in-visual-studio-2005/_static/image1.gif)

**Figura 1**: a caixa de diálogo escolher local

Ao contrário das versões anteriores do Visual Studio, se você marcar a caixa de seleção **usar protocolo SSL** e o certificado SSL não corresponder à URL que está navegando, você receberá uma caixa de diálogo alerta de segurança perguntando se deseja continuar. Usando o Visual Studio .NET 2003, se o certificado não for um correspondente, a criação do projeto falhará.

![Alerta de segurança sobre o certificado SSL](improvements-in-visual-studio-2005/_static/image2.gif)

**Figura 2**: alerta de segurança sobre o certificado SSL

### <a name="note-on-host-headers"></a>Observação nos cabeçalhos de host

Se você estiver criando um aplicativo Web em um site associado a um IP específico, será necessário garantir que um cabeçalho de host esteja configurado. Caso contrário, o Visual Studio criará o site em `http://localhost`, mas o endereço IP não será resolvido corretamente quando o site for navegado ou depurado de dentro do IDE.

Se você selecionar a opção site remoto, a caixa de diálogo será alterada para permitir que você insira a URL de destino para o novo site. Essa URL deve estar em um servidor que tenha as extensões de servidor do FrontPage habilitadas. Se você quiser trabalhar com o servidor Web local usando as extensões de servidor do FrontPage, poderá usar a opção site remoto e especificar uma URL local.

![Criando um site em um servidor remoto](improvements-in-visual-studio-2005/_static/image1.jpg)

**Figura 3**: Criando um site em um servidor remoto

Ao criar um aplicativo em um site remoto via SSL, se o certificado SSL não corresponder, a caixa de diálogo de confirmação será ligeiramente diferente da caixa de diálogo exibida ao usar a opção IIS local.

![O alerta de segurança de site remoto](improvements-in-visual-studio-2005/_static/image3.gif)

**Figura 4**: o alerta de segurança de site remoto

<a id="_Toc116100243"></a>

#### <a name="ftp"></a>FTP

O Visual Studio 2005 apresenta a opção de criar sites via FTP. Quando você usa essa opção, o IDE cria os arquivos localmente na pasta Temp dos usuários e, em seguida, usa o FTP para mover os arquivos para o local do FTP.

> [!NOTE]
> O local da pasta Temp é c:/Documents and Settings/&lt;User&gt;/Local Settings/Temp/VWDWebCache/&lt;Server&gt;/_&lt;nome do aplicativo&gt;

Ao usar a opção FTP, será exibida uma caixa de diálogo escolher local. Você insere as informações de conexão de FTP necessárias nessa caixa de diálogo, conforme mostrado abaixo.

![A caixa de diálogo escolher local para FTP](improvements-in-visual-studio-2005/_static/image2.jpg)

**Figura 5**: a caixa de diálogo escolher local para FTP

## <a name="lab-setup-ftp-site-and-create-a-project"></a>Laboratório: configurar o site FTP e criar um projeto

As etapas a seguir configuram o site FTP para que um usuário tenha um local que pode ser carregado por meio de FTP.

### <a name="install-the-ftp-service"></a>Instalar o serviço FTP

1. Abra adicionar/remover programas, selecione Adicionar/remover componentes do Windows
2. Selecione Serviços de Informações da Internet (servidor de aplicativos no Windows 2003) e clique em **detalhes**.
3. Verifique o **serviço protocolo FTP (FTP)** e clique em **OK**.
4. Clique em **Avançar** para instalar o serviço FTP.

### <a name="create-a-new-folder-for-content"></a>Criar uma nova pasta para o conteúdo

1. No Windows Explorer, crie uma nova pasta chamada **Usuário1** dentro de c:/inetpub/wwwroot.

#### <a name="configure-folders-and-permissions-on-folders"></a>Configure pastas e permissões em pastas.

1. Abra o snap-in Serviços de Informações da Internet em ferramentas administrativas. Agora você terá uma pasta sites FTP sob o nó nome do computador.
2. Expanda **sites FTP**.
3. Clique com o botão direito do mouse no **site FTP padrão**, selecione **novo**, **diretório virtual**e clique em **Avançar**.
4. Digite **user1** para o nome do diretório virtual e clique em **Avançar**.
5. Digite **c:/inetpub/wwwroot/Usuário1** para o caminho e clique em **Avançar**.
6. Clique em **Avançar** e em **concluir** para concluir o assistente.
7. Clique com o botão direito do mouse no diretório virtual **Usuário1** em site FTP padrão e selecione **Propriedades**.
8. Marque a caixa de seleção **gravar** e clique em **OK** para fechar a caixa de diálogo.
9. Clique com o botão direito do mouse em **site FTP padrão** e selecione **Propriedades**.
10. Na guia **contas de segurança** , desmarque **permitir conexões anônimas**.
11. Clique em **Sim** na caixa de diálogo perguntando se deseja continuar.
12. Clique em **OK** para fechar o diálogo.
13. Expanda o **site da Web padrão** no nó **sites** .
14. Clique com o botão direito do mouse no diretório **Usuário1** e selecione **Propriedades**
15. Na seção **configurações do aplicativo** , clique em **criar** para marcar a pasta como um aplicativo.
16. Clique em **OK** para fechar o diálogo.
17. Feche o snap-in Serviços de Informações da Internet.

### <a name="create-web-project"></a>Criar projeto Web

1. Abra o Visual Studio 2005.
2. No menu **arquivo** , selecione **novo site**.
3. Na lista suspensa **local** , selecione **FTP**.
4. Clique em **Procurar**.
5. Digite **localhost** na caixa de texto **servidor** .
6. Insira **user1** na caixa de texto diretório.
7. Clique em **Abrir**. O local do FTP será inserido na caixa de diálogo novo site.
8. Clique em **OK**.
9. Desmarque **logon anônimo** na caixa de diálogo logon de FTP, insira suas credenciais e clique em **OK**.
10. Qual é a URL para o projeto? (A URL do projeto será exibida no Gerenciador de Soluções.)
11. No menu **Compilar** , selecione **criar site da Web** ou **Compilar solução**.
12. Clique com o botão direito do mouse em Default. aspx em Gerenciador de Soluções e selecione **Exibir no navegador**.
13. Na caixa de diálogo URL do site necessária, insira `http://localhost/user1` para a URL e clique em **OK**.

> [!NOTE]
> Se você receber um erro indicando uma incapacidade de carregar o tipo/_Default, certifique-se de que você está executando o ASP.NET 2,0 no seu site e não uma versão anterior. Você pode fazer isso na guia ASP.NET em Serviços de Informações da Internet.

## <a name="opening-web-projects"></a>Abrindo projetos Web

A abertura de projetos da Web é semelhante à criação de projetos. As seções a seguir chamam áreas para ficar atento ao trabalho no IDE. Ele também aborda o trabalho com projetos Web usando HTTP e FTP.

Para abrir um projeto Web, selecione abrir site no menu arquivo. Você será solicitado com a mesma caixa de diálogo escolher local coberta anteriormente e terá as mesmas quatro opções disponíveis para você: sistema de arquivos, IIS local, FTP e site remoto.

<a id="_Toc116100245"></a>

## <a name="file-system"></a>Sistema de Arquivos

Conforme indicado anteriormente neste módulo, o Visual Studio não usa mais um arquivo de projeto. Portanto, se você optar por abrir um site do sistema de arquivos, terá a opção de escolher qualquer pasta que desejar, mesmo que a pasta escolhida não tenha sido criada inicialmente como um projeto da Web no Visual Studio. Por exemplo, você pode optar por abrir a pasta meus documentos como um site e o Visual Studio irá abri-lo e exibir os arquivos conforme mostrado abaixo.

![Meus documentos abertos como um site da Web](improvements-in-visual-studio-2005/_static/image3.jpg)

**Figura 6**: *meus documentos* abertos como um site da Web

Como o Visual Studio cria apenas arquivos e pastas adicionais quando necessário, nenhum arquivo ou pasta adicional é adicionado ao local que você abrir. Um efeito colateral dessa arquitetura é que ela impede que você aninhe sites no sistema de arquivos. Por exemplo, considere a seguinte estrutura de diretório.

Projeto Web em C:/mysite

Outro projeto Web em C:/mysite/Nested

Quando você abre o site em c:/mysite, a pasta aninhada aparecerá como uma subpasta desse aplicativo.

<a id="_Toc116100246"></a>

## <a name="http"></a>HTTP

Ao abrir sites via HTTP, as configurações são lidas na metabase do IIS (IIS local) ou usando extensões de servidor do FrontPage (site remoto). Se houver aplicativos Web aninhados, eles serão exibidos também com um ícone que os identifica como um aplicativo. Se você estiver familiarizado com o trabalho com aplicativos Web no FrontPage, o comportamento no Visual Studio 2005 é semelhante.

Embora o Visual Studio exiba um ícone para aplicativos aninhados abaixo do aplicativo que está aberto no momento no IDE, ele não permitirá que você os expanda para ver seu conteúdo. No entanto, você pode clicar duas vezes neles para abri-los. Ao fazer isso, você verá uma caixa de diálogo solicitando que você abra o aplicativo Web (e substitua a solução aberta no momento) ou adicione o aplicativo Web à sua solução atual.

![Clicar duas vezes em um ícone de aplicativo aninhado apresenta essa caixa de diálogo](improvements-in-visual-studio-2005/_static/image4.jpg)

**Figura 7**: clicar duas vezes em um ícone de aplicativo aninhado apresenta essa caixa de diálogo

<a id="_Toc116100247"></a>

## <a name="ftp-site"></a>Site FTP

Quando você abre um site via FTP, os arquivos são copiados localmente para a pasta Temp. O caminho completo para o local de armazenamento local é exibido no painel Propriedades do projeto e é criado usando o formato a seguir.

C:/Documents and Settings/&lt;User&gt;/Local Settings/Temp/VWDWebCache/&lt;Server&gt;/_&lt;nome do aplicativo&gt;

Ao usar o FTP, o Visual Studio precisará especificar a URL base para o seu projeto para que você possa procurá-lo, conforme mostrado abaixo. Se você não especificar uma URL base, o Visual Studio lhe pedirá na primeira vez que você tentar navegar em uma página no site.

![Especificando uma URL base para sites FTP](improvements-in-visual-studio-2005/_static/image5.jpg)

**Figura 8**: ESPECIFICANDO uma URL base para sites FTP

## <a name="improvements-in-compilation"></a>Aprimoramentos na compilação

Trabalhar com aplicativos Web no Visual Studio 2005 é notavelmente mais rápido do que as versões anteriores. Isso ocorre devido a uma pequena parte das alterações na arquitetura de compilação.

No Visual Studio 2002 e 2003, os aplicativos Web foram compilados em um assembly primário que reside na pasta/bin No Visual Studio 2005, uma pasta app/_Code foi adicionada. As classes e outros códigos que não são de interface do usuário são adicionados à pasta app/_Code. Quando o Visual Studio cria o projeto, todos os arquivos na pasta app/_Code são compilados em um único arquivo app/_Code. dll. O resultado dessa alteração é que as compilações subsequentes são muito mais rápidas do que nas versões anteriores.

> [!NOTE]
> O utilitário de linha de comando do MSBuild também pode ser usado para criar aplicativos Web ASP.NET. Essa ferramenta será abordada no módulo 9.

Outro aprimoramento da compilação é a nova opção de página de compilação no menu Compilar. Esse recurso permite que um desenvolvedor reconstrua apenas a página atual (juntamente com, é claro, e dependências) para que as alterações possam ser compiladas mais rapidamente. Como C# o não oferece a compilação em segundo plano para fins de atualização do IntelliSense, etc., eles se beneficiarão imensamente desse recurso, pois permitirá que o IntelliSense seja atualizado rapidamente simplesmente recriando uma única página.

As propriedades de compilação para um projeto permitem que você configure o tipo de compilação que ocorre antes da página de inicialização ser executada. Os desenvolvedores podem optar por Compilar apenas a página atual para que o Visual Studio possa iniciar a depuração de aplicativos mais rapidamente após a alteração do código.

![A ação de início da página de compilação](improvements-in-visual-studio-2005/_static/image6.jpg)

**Figura 9**: a ação de início da página de compilação

Outro grande aprimoramento do Visual Studio e da arquitetura ASP.NET está na área de editar e continuar. No Visual Studio 2005, os desenvolvedores podem iniciar a depuração de um projeto e fazer alterações de código no projeto sem desanexar o depurador. Na verdade, você pode, literalmente, iniciar a depuração de um projeto, adicionar uma nova classe, adicionar código a essa classe, adicionar código à sua página que cria uma nova instância dessa classe e executar um método da classe, tudo isso sem desanexar o depurador. A execução do novo código é literalmente tão fácil quanto atualizar o navegador!

Clique aqui para ver uma explicação em vídeo sobre o recurso Editar e continuar no Visual Studio 2005.

![](improvements-in-visual-studio-2005/_static/image2.png)

[Abrir vídeo de tela inteira](improvements-in-visual-studio-2005/_static/editcontinue1.wmv)

A funcionalidade robusta de editar e continuar no ASP.NET 2,0 e no Visual Studio 2005 é devido a uma alteração de arquitetura para aplicativos ASP.NET. No ASP.NET 1. x, os aplicativos criados no Visual Studio 2002/2003 foram compilados em um assembly primário que foi armazenado na pasta/bin. Todas as classes, páginas, etc. para o aplicativo foram compiladas nessa DLL. Em seguida, em tempo de execução, ASP.NET compilaria todos os controles, a marcação e o código ASP.NET em páginas e copiaria essas DLLs para a pasta temporária ASP.NET.

No Visual Studio 2005 usando ASP.NET 2,0, os dois modelos de compilação estruturados acima (um para o Visual Studio e outro para ASP.NET em tempo de execução) foram mesclados em um modelo de compilação comum. Isso significa que todos os problemas de compilação agora são capturados durante o estágio de desenvolvimento em vez de em tempo de execução. Ele também permite o suporte do designer e do IntelliSense para recursos como controles de usuário e páginas mestras.

Clique aqui para ver uma explicação em vídeo sobre o suporte do designer para controles de usuário.

![](improvements-in-visual-studio-2005/_static/image3.png)

[Abrir vídeo de tela inteira](improvements-in-visual-studio-2005/_static/usercontrols1.wmv)

> [!NOTE]
> Quando um controle de usuário é removido de uma página, a diretiva de @Register permanece na marcação e deve ser removida manualmente para evitar erros de analisador se o controle de usuário é excluído do site da Web.

Outra melhoria no modelo de compilação do Visual Studio é o recurso Publish Web site. Como o recurso de publicação pré-compila o site da Web, os desenvolvedores podem desfrutar do desempenho adicional de não ter que compilar nada sob demanda. Ele também pré-compila todo o código-fonte na pasta app/_Code em uma DLL para que nenhum código-fonte tenha de ser implantado.

![A caixa de diálogo Publicar site](improvements-in-visual-studio-2005/_static/image7.jpg)

**Figura 10**: a caixa de diálogo Publicar site

> [!NOTE]
> O utilitário ASPNET/_compile. exe também pode ser usado para pré-compilar previamente um aplicativo Web ASP.NET. Essa ferramenta será abordada no módulo 9.

Quando você publica um site, os arquivos pré-compilados são armazenados na pasta Temporary ASP.NET Files, conforme mostrado abaixo. Arquivos com uma extensão de arquivo *. Compiled* são arquivos XML que definem dependências para DLLs específicas. Qualquer Web Form ou controles de usuário são compilados em DLLs aleatórias que começam com *app/_Web/_* .

Se você deixar a caixa de seleção *permitir que este site pré-compilado seja atualizado* marcada, a marcação dentro de seus WebForms e controles de usuário não será pré-compilado em uma dll, permitindo que você faça alterações após a implantação. Se você preferir bloquear a marcação para que as alterações no conteúdo implantado não sejam permitidas, desmarque essa caixa.

A caixa de seleção *usar assemblies de nomenclatura fixa e de página única* permite que você desabilite a compilação em lotes para que cada página seja compilada em um assembly de nome fixo. Deixar essa caixa desmarcada permite que você aproveite a compilação em lote.

A caixa de seleção *habilitar nome forte em assemblies pré-compilados* permite que você nomeie seus assemblies pré-compilados com um nome forte.

> [!NOTE]
> No ASP.NET 1. x, os assemblies de nome forte tinham que ser instalados no GAC (cache de assembly global). No ASP.NET 2,0, não é necessário instalar assemblies de nome forte no GAC.

![Um arquivo ASP.NET de aplicativos compilados previamente](improvements-in-visual-studio-2005/_static/image8.jpg)

**Figura 11**: um arquivo ASP.net de aplicativos compilados previamente

> [!NOTE]
> No aplicativo acima, não havia nenhum arquivo Web. config. Se houvesse, ele teria sido chamado de *PrecompiledApp. config* após o processo de publicação do site.

## <a name="improvements-in-deployment"></a>Melhorias na implantação

Assim como no Visual Studio 2002 e 2003, o Visual Studio 2005 oferece um recurso de copiar projeto. No entanto, o recurso foi aprimorado no Visual Studio 2005 e agora é chamado de Copy Web site.

A caixa de diálogo copiar site é dividida em um quadro à esquerda e em um quadro à direita. O quadro à esquerda é chamado de site de origem e o quadro à direita é chamado de site remoto. Uma coisa que pode confundir alguns desenvolvedores é que o site exibido no quadro certo não é necessariamente um site remoto. Pode ser um site no sistema de arquivos local ou na instância local do IIS. Além disso, o site exibido no quadro esquerdo não é necessariamente o site de origem, pois a caixa de diálogo permite que você publique do site remoto *para* o site de origem.

Se você estiver copiando um projeto para um site remoto, esse site deverá ter as extensões de servidor do FrontPage instaladas nele. Se não tiver, você precisará se conectar usando FTP. Por outro lado, se você estiver copiando um projeto para a instância local do IIS, as extensões de servidor do FrontPage não serão necessárias.

> [!NOTE]
> Se você tentar criar um novo site na instância local do IIS e as extensões de servidor do FrontPage 2002 estiverem instaladas, você receberá uma mensagem de erro informando que não há suporte para a criação de sites em um servidor do SharePoint. Nesse caso, você tem a opção de instalar as extensões de servidor do FrontPage 2000 ou remover as extensões de servidor do FrontPage.

Clique aqui para obter uma explicação em vídeo sobre o recurso Copy Web site.

![](improvements-in-visual-studio-2005/_static/image4.png)

[Abrir vídeo de tela inteira](improvements-in-visual-studio-2005/_static/copysite1.wmv)

## <a name="improvements-in-debugging"></a>Melhorias na depuração

Há quatro aprimoramentos importantes na depuração do Visual Studio 2005.

- A depuração local como um não administrador é possível pronta para uso.
- O atributo debug para o elemento Compilation agora é false por padrão.
- A instalação e configuração de depuração remota é mais fácil do que antes.
- Agora você pode depurar um site da Web aberto por meio de um local FTP.

## <a name="debugging-as-a-non-administrator"></a>Depuração como um não administrador

A adição do ASP.NET Development Server permite que não administradores depurem facilmente aplicativos ASP.NET prontos para uso. Quando um aplicativo ASP.NET em execução no sistema de arquivos local é depurado, o Visual Studio inicia o ASP.NET Development Server sob o contexto do usuário conectado. Esse usuário pode depurar esse aplicativo sem nenhuma configuração adicional.

## <a name="debug-is-false-by-default"></a>Debug é false por padrão

No ASP.NET 1. x, o atributo *debug* no elemento *Compilation* do arquivo Web. config foi definido como *true* por padrão. Sempre foi recomendável que os desenvolvedores definam esse atributo como *false* antes de implantar um aplicativo para produção, mas como a maioria dos desenvolvedores não entende totalmente as consequências de deixar o atributo de depuração definido como true, eles simplesmente o deixavam como estão.

O problema mais grave com o atributo de depuração definido como true é que ele desabilita o modelo de compilação em lote ASP. sub-redes. Portanto, cada página é compilada em uma DLL separada. Se um aplicativo Web consistir em milhares de páginas (não desconhecidos por qualquer meio), isso significará que vários milhares de DLLs pequenas serão criadas por esse aplicativo. Embora essas DLLs tenham tamanho pequeno, elas não são carregadas em nenhum local específico na memória. Portanto, eles causam a fragmentação na memória do sistema e podem contribuir com as ocorrências de OutOfMemoryexception.

No ASP.NET 2,0, o atributo debug é definido como false por padrão. Como você já viu, quando um desenvolvedor detecte um aplicativo ASP.NET no Visual Studio 2005, ele é solicitado a adicionar um arquivo Web. config com a depuração habilitada. Fazer isso incorre nas mesmas desvantagens que estavam presentes no ASP.NET 1. x, mas agora o desenvolvedor é claramente avisado de que o atributo deve ser redefinido para false antes de mover o aplicativo para produção.

## <a name="remote-debugging-setup-and-configuration"></a>Configuração e instalação de depuração remota

No Visual Studio 2002/2003, a depuração remota dependia do Machine Debug Manager (MDM. exe) e do processo VS7Jit. exe. Por isso, a solução de problemas de depuração remota geralmente era uma caixa preta para os clientes e, em geral, não era muito melhor para o PSS.

O Visual Studio 2005 remove a dependência nos processos MDM. exe e VS7Jit. exe. Em vez disso, ele agora usa o serviço Remote Debug monitor (msvsmon. exe.)

O requisito de depuração no Visual Studio 2005 remotamente é bastante simples. Você precisa executar o msvsmon. exe no servidor remoto antes da depuração. Você pode instalar o monitor de depuração remota do CD do Visual Studio ou simplesmente executar msvsmon. exe de um compartilhamento sem instalar nada no servidor Web.

Quando você executa o msvsmon. exe, é provável que ele seja reclamado sobre as portas que estão sendo bloqueadas para depuração remota. Felizmente, você pode facilmente desbloquear as portas diretamente na caixa de diálogo de aviso, conforme mostrado abaixo.

![Notificação de que o Firewall do Windows está bloqueando a depuração remota](improvements-in-visual-studio-2005/_static/image9.jpg)

**Figura 12**: notificação de que o Firewall do Windows está bloqueando a depuração remota

Depois de desbloquear as portas necessárias para depuração, você verá o Monitor de Depuração Remota como mostrado abaixo. A partir dessa interface, você pode monitorar conexões e alterar as permissões de depuração facilmente.

![O Monitor de Depuração Remota](improvements-in-visual-studio-2005/_static/image10.jpg)

**Figura 13**: o monitor de depuração remota

Também é possível depurar remotamente um aplicativo Web aberto via FTP. As etapas são as mesmas abordadas anteriormente. No entanto, você precisará especificar uma URL base para navegar pelo projeto FTP, conforme descrito anteriormente neste módulo.

## <a name="lab-2"></a>Laboratório 2

## <a name="remote-debugging-with-visual-studio-2005"></a>Depuração remota com o Visual Studio 2005

Este laboratório explicará a depuração remota com o Visual Studio 2005.

Clique aqui para obter uma explicação em vídeo deste laboratório.

![](improvements-in-visual-studio-2005/_static/image5.png)

[Abrir vídeo de tela inteira](improvements-in-visual-studio-2005/_static/remdebug1.wmv)

Este laboratório exige que você tenha dois computadores, um executando o Visual Studio 2005 e o outro executando o IIS 5 ou superior.

1. Abra o Visual Studio 2005 e crie um novo site no servidor remoto.

> [!NOTE]
> Você pode criar o site em uma instância remota do IIS ou via FTP.

1. No servidor Web remoto, localize msvsmon. exe no computador de desenvolvimento usando um caminho UNC e execute-o.  
 O local padrão para msvsmon. exe é//Server/c $/Program Files/Microsoft Visual Studio 8/Common7/IDE/Remote Debugger/x86.
2. Se for solicitado a desbloquear portas para depuração remota, faça isso.
3. No computador de desenvolvimento, abra o code-behind para default. aspx e defina um ponto de interrupção no método Page/_Load.
4. Inicie a depuração do computador de desenvolvimento.

Você deve atingir o ponto de interrupção conforme o esperado.

## <a name="aspnet-development-server"></a>ASP.NET Development Server

Como já discutimos, o Visual Studio 2005 é fornecido com um servidor Web chamado de ASP.NET Development Server. (Às vezes, o ASP.NET Development Server é chamado de Cassini.) Esse servidor Web é um meio conveniente para procurar e depurar aplicativos Web em execução no sistema de arquivos.

O ASP.NET Development Server é um servidor Web restrito. Ele não permite conexões remotas, ele não permite nenhuma solicitação de nenhum usuário que não seja o usuário que iniciou o servidor Web. Ele também não tem a capacidade de servir páginas ASP. Somente recursos ASP.NET e recursos HTML (incluindo imagens, arquivos CSS, etc.) são atendidos.

O ASP.NET Development Server pode ser iniciado por meio da linha de comando executando o arquivo WebDev. WebServer. exe localizado em c:/Windows/Microsoft. NET/Framework/v 2.0./ */* / */* /*. A caixa de diálogo a seguir exibe os parâmetros que estão disponíveis.

![](improvements-in-visual-studio-2005/_static/image11.jpg)

**Figura 14**

> [!NOTE]
> Não há suporte para o ASP.NET Development Server quando iniciado explicitamente por meio da linha de comando.
