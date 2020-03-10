---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
title: Criar o projeto | Microsoft Docs
author: Erikre
description: Esta série de tutoriais ensinará as noções básicas da criação de um aplicativo ASP.NET Web Forms usando o ASP.NET 4,5 e o Microsoft Visual Studio Express 2013 para nós...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 2ce36f78-8ecb-4ab1-b748-6d0ab633ea3f
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
msc.type: authoredcontent
ms.openlocfilehash: 62918b17f42e54dfe4e45a08927b1039dcbb7012
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78571984"
---
# <a name="create-the-project"></a>Criar o projeto

por [Erik Reitan](https://github.com/Erikre)

[Baixar o projeto de exemplo WingtipC#Toys ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [baixar o livro eletrônico (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta série de tutoriais ensinará as noções básicas da criação de um aplicativo ASP.NET Web Forms usando o ASP.NET 4,5 e o Microsoft Visual Studio Express 2013 para a Web. Um [projeto Visual Studio 2013 com C# código-fonte](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) está disponível para acompanhar esta série de tutoriais.

Neste tutorial, você criará, examinará e executará o projeto padrão no Visual Studio, o que permitirá que você se familiarize com os recursos do ASP.NET. Além disso, você examinará o ambiente do Visual Studio.

## <a name="what-youll-learn"></a>O que você aprenderá:

- Como criar um novo projeto de Web Forms.
- A estrutura de arquivos do projeto Web Forms.
- Como executar o projeto no Visual Studio.
- Os diferentes recursos do aplicativo Web Forms padrão.
- Algumas noções básicas sobre como usar o ambiente do Visual Studio.

## <a name="creating-the-project"></a>Criando o Projeto

1. Abra o Visual Studio.
2. Selecione **novo projeto** no menu **arquivo** no Visual Studio. 

    ![Criar o item de menu projeto-novo projeto](create-the-project/_static/image1.png)
3. Selecione os **modelos** -&gt; **Visual C#**  -&gt; grupo de modelos **da Web** à esquerda.
4. Selecione o modelo **Aplicativo Web ASP.NET** na coluna central.  
 Esta série de tutoriais está usando .NET Framework 4.5.2.
5. Nomeie o projeto *WingtipToys* e escolha o botão **OK** . 

    ![Criar o projeto – caixa de diálogo novo projeto](create-the-project/_static/image2.png)

    > [!NOTE]
    > O nome do projeto nesta série de tutoriais é **WingtipToys**. É recomendável que você use esse nome de projeto *exato* para que o código fornecido durante a série de tutoriais funcione conforme o esperado.

6. Clique no botão **Alterar Autenticação**. Selecione **contas de usuário individuais** e clique no botão **OK** .

7. Selecione o modelo de **Web Forms** e clique no botão **OK** .

    ![Criar o modelo projeto-novo projeto](create-the-project/_static/image3.png)

O projeto levará algum tempo para ser criado. Quando estiver pronto, abra a página **Default. aspx** .

![Criar o modelo projeto-novo projeto](create-the-project/_static/image4.png)

Você pode alternar entre a exibição de **design** e a exibição de **código-fonte** selecionando uma opção na parte inferior da janela central. O modo **design** exibe páginas da Web ASP.net, páginas mestras, páginas de conteúdo, páginas HTML e controles de usuário usando uma exibição quase WYSIWYG. Exibição de **origem** exibe a marcação HTML para sua página da Web, que você pode editar.

> [!TIP] 
> 
> **Compreendendo as estruturas ASP.NET**
> 
> O ASP.NET Web Forms lhe permite compilar sites dinâmicos usando um modelo familiar de arrastar e soltar, baseado em evento. Uma superfície de design e muitos controles e componentes lhe permitem compilar rapidamente sites sofisticados, poderosos baseados em UI com acesso de dados. A loja Wingtip Toy é baseada em ASP.NET Web Forms, mas muitos dos conceitos que você aprende nessa série de tutoriais são aplicáveis a todos os ASP.NET.
> 
> O ASP.NET oferece quatro estruturas de desenvolvimento principais:
> 
> - [Web Forms ASP.NET](../../../index.md)  
>  O Web Forms Framework visa os desenvolvedores que preferem a programação declarativa e baseada em controle, como o Microsoft Windows Forms (WinForms) e o WPF/XAML/Silverlight. Ele oferece um modelo de desenvolvimento controlado por designer WYSIWYG, portanto, é popular com os desenvolvedores que buscam um ambiente de desenvolvimento rápido de aplicativos (RAD) para o desenvolvimento para a Web. Se você for novo na programação da Web e estiver familiarizado com as ferramentas tradicionais de desenvolvimento de clientes do Microsoft RAD (por exemplo, C#para Visual Basic e Visual), poderá criar rapidamente um aplicativo Web sem ter experiência em HTML e JavaScript.
> - [ASP.NET MVC](../../../../mvc/index.md)  
>  O ASP.NET MVC visa os desenvolvedores que estão interessados em padrões e princípios, como desenvolvimento orientado a testes, separação de preocupações, inversão de controle (IoC) e injeção de dependência (DI). Essa estrutura incentiva a separação da camada de lógica de negócios de um aplicativo Web de sua camada de apresentação.
> - [Páginas da Web do ASP.NET](../../../../web-pages/index.md)  
>  Páginas da Web do ASP.NET visa os desenvolvedores que desejam uma história simples de desenvolvimento para a Web, juntamente com as linhas do PHP. No modelo de páginas da Web, você cria páginas HTML e, em seguida, adiciona o código baseado em servidor à página para controlar dinamicamente como essa marcação é renderizada. As páginas da Web são especificamente projetadas para ser uma estrutura leve, e é o ponto de entrada mais fácil em ASP.NET para pessoas que conhecem HTML, mas podem não ter ampla experiência de programação – por exemplo, estudantes ou entusiastas. Também é uma boa maneira para os desenvolvedores da Web que conhecem PHP ou estruturas semelhantes para começar a usar o ASP.NET.
> - [Aplicativo de página única do ASP.NET](../../../../single-page-application/index.md)  
>  O aplicativo de página única (SPA) do ASP.NET ajuda a criar aplicativos que incluem interações significativas do lado do cliente usando HTML 5, CSS 3 e JavaScript. A atualização do ASP.NET and Web Tools 2012,2 envia um novo modelo para a criação de aplicativos de página única usando knockout. js e ASP.NET Web API. Além do novo modelo SPA, novos modelos SPA criados pela Comunidade também estão disponíveis para download.
> 
> Além das quatro estruturas de desenvolvimento principais, o ASP.NET também oferece tecnologias adicionais que são importantes para conhecer e estar familiarizados, mas não são abordadas nesta série de tutoriais:
> 
> - [ASP.NET Web API](../../../../web-api/index.md) -uma estrutura para a criação de serviços http que atingem uma ampla variedade de clientes, incluindo navegadores e dispositivos móveis.
> - [ASP.net signalr](../../../../signalr/index.md) -uma biblioteca que facilita o desenvolvimento de funcionalidades da Web em tempo real.

### <a name="reviewing-the-project"></a>Examinando o projeto

No Visual Studio, a janela **Gerenciador de soluções** permite gerenciar arquivos para o projeto. Vamos dar uma olhada nas pastas que foram adicionadas ao seu aplicativo em **Gerenciador de soluções**. O modelo de aplicativo Web adiciona uma estrutura básica de pastas:

![Criar o projeto-Gerenciador de Soluções](create-the-project/_static/image5.png)

O Visual Studio cria algumas pastas e arquivos iniciais para seu projeto. Os primeiros arquivos com os quais você trabalhará mais adiante neste tutorial são os seguintes:

| **Arquivo** | **Finalidade** |
| --- | --- |
| *Default. aspx* | Normalmente, a primeira página exibida quando o aplicativo é executado em um navegador. |
| *Site. Master* | Uma página que permite que você crie um layout consistente e use o comportamento padrão para páginas em seu aplicativo. |
| *Global. asax* | Um arquivo opcional que contém o código para responder a eventos no nível de aplicativo e de sessão gerados pelo ASP.NET ou por módulos HTTP. |
| *Web.config* | Os dados de configuração de um aplicativo. |

### <a name="running-the-default-web-application"></a>Executando o aplicativo Web padrão

O aplicativo Web padrão fornece uma experiência rica com base na funcionalidade interna e no suporte. Sem alterações no projeto padrão do Web Forms, o aplicativo estará pronto para ser executado no navegador da Web local.

1. Pressione a tecla ***F5*** enquanto estiver no Visual Studio.   
 O aplicativo será criado e exibido em seu navegador da Web.  

    ![Criar o projeto-página padrão](create-the-project/_static/image6.png)
2. Depois de concluir a revisão do aplicativo em execução, feche a janela do navegador.

Há três páginas principais neste aplicativo Web padrão: *Default. aspx* (Home), *about. aspx*e *Contact. aspx*. Cada uma dessas páginas pode ser acessada na barra de navegação superior. Há também duas páginas adicionais contidas na pasta Account, a página Register. aspx e a página login. aspx. Essas duas páginas permitem que você use os recursos de associação do ASP.NET para criar, armazenar e validar as credenciais do usuário.

## <a name="aspnet-web-forms-background"></a>ASP.NET Web Forms plano de fundo

ASP.NET Web Forms são páginas baseadas na tecnologia Microsoft ASP.NET, em que o código executado no servidor gera dinamicamente a saída da página da Web para o dispositivo do navegador ou do cliente. Uma página de Web Forms ASP.NET renderiza automaticamente o HTML compatível com navegador correto para recursos como estilos, layout e assim por diante. Web Forms são compatíveis com qualquer linguagem compatível com o Common Language Runtime .NET, como Microsoft Visual Basic e Microsoft Visual C#. Além disso, os Web Forms são criados no [Microsoft .NET Framework](https://msdn.microsoft.com/vstudio/aa496123), que fornece benefícios como um ambiente gerenciado, segurança de tipo e herança.

Quando uma página Web Forms ASP.NET é executada, a página passa por um ciclo de vida no qual executa uma série de etapas de processamento. Essas etapas incluem inicialização, criação de instâncias de controles, restauração e manutenção de estado, execução do código do manipulador de eventos e renderização. Ao se familiarizar mais com o poder do ASP.NET Web Forms, é importante entender o ciclo de vida da [página ASP.net](https://msdn.microsoft.com/library/ms178472(v=vs.100).aspx) para que você possa escrever código no estágio apropriado do ciclo de vida para o efeito pretendido.

Quando um servidor Web recebe uma solicitação de uma página, ele localiza a página, processa-a, envia-a para o navegador e, em seguida, descarta todas as informações da página. Se o usuário solicitar a mesma página novamente, o servidor repetirá a sequência inteira, reprocessando a página do zero. Em outras palavras, um servidor não tem memória de páginas processadas-as páginas são sem monitoração de estado. A estrutura de página do ASP.NET manipula automaticamente a tarefa de manter o estado de sua página e seus controles e fornece maneiras explícitas de manter o estado das informações específicas do aplicativo.

> [!TIP] 
> 
> **Recursos do aplicativo Web no modelo de aplicativo Web Forms**
> 
> O modelo de aplicativo ASP.NET Web Forms fornece um rico conjunto de funcionalidades internas. Ele não apenas fornece uma página *Home. aspx* , uma página *about. aspx* , uma página *Contact. aspx* , mas também inclui a funcionalidade de associação que registra os usuários e salva suas credenciais para que eles possam fazer logon no seu site. Esta visão geral fornece mais informações sobre alguns dos recursos contidos no modelo de aplicativo ASP.NET Web Forms e como eles são usados no aplicativo Wingtip Toys.
> 
> **Associação**
> 
> [ASP.net](https://msdn.microsoft.com/library/yh26yfzy.aspx) A identidade armazena as credenciais de seus usuários em um banco de dados criado pelo aplicativo. Quando os usuários fizerem logon, o aplicativo validará suas credenciais lendo o banco de dados. A pasta *conta* do seu projeto contém os arquivos que implementam as várias partes de associação: registro, logon, alteração de senha e autorização do acesso. Além disso, o ASP.NET Web Forms dá suporte ao OAuth e ao OpenID. Esses aprimoramentos de autenticação permitem que os usuários façam logon em seu site usando credenciais existentes, a partir de contas como Facebook, Twitter, Windows Live e Google.
> 
> ![Criar o projeto-Gerenciador de Soluções (ASP.NET Identity)](create-the-project/_static/image7.png)
> 
> Por padrão, o modelo cria um banco de dados de associação usando um nome de banco de dados padrão em uma instância do SQL Server Express LocalDB, o servidor de banco de dados de desenvolvimento que vem com Visual Studio Express 2013 para Web.
> 
> **SQL Server Express LocalDB**
> 
> [SQL Server Express LocalDB](https://technet.microsoft.com/library/hh510202.aspx) é uma versão leve do SQL Server que tem muitos recursos de programação de um banco de dados SQL Server. SQL Server Express LocalDB é executado no modo de usuário e tem uma instalação rápida e sem configuração que tem uma pequena lista de pré-requisitos de instalação. No Microsoft SQL Server, qualquer código de banco de dados ou Transact-SQL pode ser movido de SQL Server Express LocalDB para SQL Server e SQL Azure sem nenhuma etapa de atualização. Portanto, SQL Server Express LocalDB pode ser usado como um ambiente de desenvolvedor para aplicativos direcionados a todas as edições do SQL Server. SQL Server Express o LocalDB habilita recursos como procedimentos armazenados, funções definidas pelo usuário e agregações, integração de .NET Framework, tipos espaciais e outros que não estão disponíveis no SQL Server Compact.
> 
> **Páginas mestras**
> 
> Uma [página mestra ASP.net](https://msdn.microsoft.com/library/wtxbf3hh.aspx) define uma aparência e um comportamento consistentes para todas as páginas em seu aplicativo. O layout da página mestra é mesclado com o conteúdo de uma página de conteúdo individual para produzir a página final que o usuário vê. No aplicativo Wingtip Toys, você modifica a página mestra *site. Master* para que todas as páginas do site da Wingtip Toys compartilhem o mesmo logotipo distintivo e a mesma barra de navegação.
> 
> **HTML5**
> 
> O modelo de aplicativo ASP.NET Web Forms dá suporte a [HTML5](http://www.w3schools.com/html/html5_intro.asp), que é a versão mais recente da linguagem de marcação HTML. O HTML5 dá suporte a novos elementos e funcionalidades que facilitam a criação de sites da Web.
> 
> **Modernizr**
> 
> Para navegadores que não dão suporte a HTML5, você pode usar o [Modernizr](http://www.modernizr.com/). O Modernizr é uma biblioteca JavaScript de software livre que pode detectar se um navegador oferece suporte a recursos do HTML5 e habilitá-los se não o tiver. No modelo de aplicativo ASP.NET Web Forms, o Modernizr é instalado como um pacote NuGet.
> 
> **Bootstrap**
> 
> Os modelos de projeto Visual Studio 2013 usam a [inicialização](http://getbootstrap.com/), um layout e estrutura de temas criados pelo Twitter. A inicialização usa o CSS3 para fornecer design responsivo, o que significa que os layouts podem se adaptar dinamicamente a diferentes tamanhos de janela do navegador. Você também pode usar o recurso de ti da Bootstrap para efetivar facilmente uma alteração na aparência do aplicativo. Por padrão, o modelo de aplicativo Web ASP.NET no Visual Studio 2013 inclui a inicialização como um pacote NuGet.
> 
> **Pacotes NuGet**
> 
> O modelo de aplicativo ASP.NET Web Forms inclui um conjunto de pacotes [NuGet](http://www.nuget.org/) . Esses pacotes fornecem funcionalidade de componentes na forma de bibliotecas e ferramentas de software livre. Há uma ampla variedade de pacotes para ajudá-lo a criar e testar seus aplicativos. O Visual Studio torna mais fácil adicionar, remover e atualizar pacotes NuGet. Os desenvolvedores também podem criar e adicionar pacotes ao NuGet.
> 
> ![Criar a caixa de diálogo projeto-NuGet](create-the-project/_static/image8.png)
> 
> Quando você instala um pacote, o NuGet copia arquivos para sua solução e faz automaticamente quaisquer alterações necessárias, como adicionar referências e alterar a configuração associada ao seu aplicativo Web. Se você decidir remover a biblioteca, o NuGet removerá os arquivos e inverterá quaisquer alterações feitas em seu projeto para que nenhuma desordem seja deixada. O NuGet está disponível no menu **ferramentas** no Visual Studio.
> 
> **jQuery**
> 
> o [jQuery](http://jquery.com/) é uma biblioteca JavaScript rápida e concisa que simplifica a travessia de documentos HTML, manipulação de eventos, animação e interações com AJAX para desenvolvimento rápido na Web. A biblioteca JavaScript jQuery está incluída no modelo de aplicativo ASP.NET Web Forms como um pacote NuGet.
> 
> **Validação não invasiva**
> 
> Controles de validação internos foram configurados para usar JavaScript discreto para lógica de validação no lado do cliente. Isso reduz significativamente a quantidade de JavaScript renderizada embutida na marcação de página e reduz o tamanho geral da página. A validação discreta é adicionada globalmente ao modelo de aplicativo ASP.NET Web Forms com base na configuração no elemento &lt;appSettings&gt; do arquivo *Web. config* na raiz do aplicativo.
> 
> **Entity Framework Code First**
> 
> Além dos recursos do modelo de aplicativo ASP.NET Web Forms, o aplicativo Wingtip Toys usa [Entity Framework Code First](https://weblogs.asp.net/scottgu/archive/2010/12/08/announcing-entity-framework-code-first-ctp5-release.aspx), que é uma biblioteca NuGet que permite o desenvolvimento centrado em código quando você trabalha com dados. Em suma, ele cria a parte do banco de dados do seu aplicativo com base no código que você escreve. Usando o Entity Framework, você recupera e manipula dados como objetos fortemente tipados. Isso permite que você se concentre na lógica de negócios em seu aplicativo, e não nos detalhes de como os dados são acessados.
> 
> Para obter informações adicionais sobre as bibliotecas e os pacotes instalados incluídos no modelo de Web Forms ASP.NET, consulte a lista de pacotes NuGet instalados. Para fazer isso, no Visual Studio, crie um novo projeto de Web Forms, selecione **ferramentas** > **Gerenciador de pacotes NuGet** > **gerenciar pacotes NuGet para solução**e selecione **pacotes instalados** na caixa de diálogo **gerenciar pacotes NuGet** .

### <a name="touring-visual-studio"></a>Tourando o Visual Studio

As janelas primárias no Visual Studio incluem **o Gerenciador de soluções**, a **Gerenciador de servidores** (**Gerenciador de banco de dados** no Express), a **janela Propriedades**, a **caixa**de ferramentas, a barra de **ferramentas**e a janela do **documento**.

![Criar a caixa de diálogo projeto-NuGet](create-the-project/_static/image9.png)

Para obter mais informações sobre o Visual Studio, consulte [Guia Visual para Visual Web Developer](https://msdn.microsoft.com/library/ee410104.aspx).

## <a name="summary"></a>Resumo

Neste tutorial, você criou, analisou e executou o aplicativo de Web Forms padrão. Você analisou os diferentes recursos do aplicativo Web Forms padrão e aprendeu algumas noções básicas sobre como usar o ambiente do Visual Studio. Nos tutoriais a seguir, você criará a camada de acesso a dados.

## <a name="additional-resources"></a>Recursos adicionais

[Escolhendo o modelo de programação certo](../../../videos/how-do-i/choosing-the-right-programming-model.md)   
[Projetos de aplicativos Web versus projetos de site](https://msdn.microsoft.com/library/dd547590.aspx)   
[Visão geral das páginas do ASP.NET Web Forms](https://msdn.microsoft.com/library/428509ah.aspx)

> [!div class="step-by-step"]
> [Anterior](introduction-and-overview.md)
> [Próximo](create_the_data_access_layer.md)
