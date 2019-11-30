---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
title: Interface do usuário e navegação | Microsoft Docs
author: Erikre
description: Esta série de tutoriais ensinará as noções básicas da criação de um aplicativo ASP.NET Web Forms usando o ASP.NET 4,5 e o Microsoft Visual Studio Express 2013 para nós...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 5c76891d-e515-4885-b576-76bd2c494efe
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
msc.type: authoredcontent
ms.openlocfilehash: ac1dcaf1ba911fdcaeb3845c6836ec771733d93e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74636811"
---
# <a name="ui-and-navigation"></a>Interface do usuário e navegação

por [Erik Reitan](https://github.com/Erikre)

[Baixar o projeto de exemplo WingtipC#Toys ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [baixar o livro eletrônico (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta série de tutoriais ensinará as noções básicas da criação de um aplicativo ASP.NET Web Forms usando o ASP.NET 4,5 e o Microsoft Visual Studio Express 2013 para a Web. Um [projeto Visual Studio 2013 com C# código-fonte](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) está disponível para acompanhar esta série de tutoriais.

Neste tutorial, você modificará a interface do usuário do aplicativo Web padrão para dar suporte aos recursos do aplicativo front de armazenamento Wingtip Toys. Além disso, você adicionará navegação simples e vinculada a dados. Este tutorial se baseia no tutorial anterior "criar a camada de acesso a dados" e faz parte da série de tutoriais da Wingtip Toys.

## <a name="what-youll-learn"></a>O que você aprenderá:

- Como alterar a interface do usuário para oferecer suporte a recursos do aplicativo front de armazenamento Wingtip Toys.
- Como configurar um elemento HTML5 para incluir a navegação de página.
- Como criar um controle controlado por dados para navegar para dados específicos do produto.
- Como exibir dados de um banco de dado criado usando Entity Framework Code First.

ASP.NET Web Forms permite que você crie conteúdo dinâmico para seu aplicativo Web. Cada página da Web ASP.NET é criada de maneira semelhante a uma página da Web HTML estática (uma página que não inclui processamento baseado em servidor), mas a página da Web ASP.NET inclui elementos adicionais que o ASP.NET reconhece e processa para gerar HTML quando a página é executada.

Com uma página HTML estática (arquivo *. htm* ou *. html* ), o servidor atende a uma solicitação de `Web` lendo o arquivo e enviando-o como está para o navegador. Por outro lado, quando alguém solicita uma página da Web ASP.NET (arquivo *. aspx* ), a página é executada como um programa no servidor Web. Enquanto a página estiver em execução, ela poderá executar qualquer tarefa que o seu site exigir, incluindo o cálculo de valores, a leitura ou a gravação de informações do banco de dados ou a chamada de outros programas. Como sua saída, a página produz dinamicamente a marcação (como elementos em HTML) e envia essa saída dinâmica para o navegador.

## <a name="modifying-the-ui"></a>Modificando a interface do usuário

Você continuará esta série de tutoriais modificando a página *Default. aspx* . Você modificará a interface do usuário que já está estabelecida pelo modelo padrão usado para criar o aplicativo. O tipo de modificações que você fará é típico ao criar qualquer aplicativo Web Forms. Você fará isso alterando o título, substituindo algum conteúdo e removendo o conteúdo padrão desnecessário.

1. Abra ou alterne para a página *Default. aspx* .
2. Se a página aparecer no modo **design** , alterne para o modo de exibição de **código-fonte** .
3. Na parte superior da página na diretiva `@Page`, altere o atributo `Title` para "bem-vindo", conforme mostrado em amarelo abaixo. 

    [!code-aspx[Main](ui_and_navigation/samples/sample1.aspx?highlight=1)]
4. Também na página *Default. aspx* , substitua todo o conteúdo padrão contido na marca `<asp:Content>` para que a marcação apareça como abaixo. 

    [!code-aspx[Main](ui_and_navigation/samples/sample2.aspx)]
5. Salve a página *Default. aspx* selecionando **salvar default. aspx** no menu **arquivo** .

   A página *Default. aspx* resultante será exibida da seguinte maneira: 

[!code-aspx[Main](ui_and_navigation/samples/sample3.aspx)]

No exemplo, você definiu o atributo `Title` da diretiva `@Page`. Quando o HTML é exibido em um navegador, o código do servidor `<%: Page.Title %>` é resolvido para o conteúdo contido no atributo `Title`.

A página de exemplo inclui os elementos básicos que constituem uma página da Web ASP.NET. A página contém texto estático como você pode ter em uma página HTML, juntamente com elementos que são específicos para ASP.NET. O conteúdo contido na página *Default. aspx* será integrado ao conteúdo da página mestra, que será explicado posteriormente neste tutorial.

### <a name="page-directive"></a>@Page diretiva

O ASP.NET Web Forms geralmente contém diretivas que permitem especificar propriedades de página e informações de configuração para a página. As diretivas são usadas pelo ASP.NET como instruções sobre como processar a página, mas elas não são renderizadas como parte da marcação que é enviada para o navegador.

A diretiva mais comumente usada é a diretiva `@Page`, que permite que você especifique muitas opções de configuração para a página, incluindo o seguinte:

1. A linguagem de programação de servidor para o código na página, C#como.
2. Se a página é uma página com código de servidor diretamente na página, que é chamada de página de arquivo único ou se é uma página com código em um arquivo de classe separado, que é chamado de página code-behind.
3. Se a página tem uma página mestra associada e, portanto, deve ser tratada como uma página de conteúdo.
4. Opções de depuração e rastreamento.

Se você não incluir uma diretiva `@Page` na página, ou se a diretiva não incluir uma configuração específica, uma configuração será herdada do arquivo de configuração *Web. config* ou do arquivo de configuração *Machine. config* . O arquivo *Machine. config* fornece definições de configuração adicionais para todos os aplicativos em execução em um computador.

> [!NOTE] 
> 
> O *Machine. config* também fornece detalhes sobre todas as definições de configuração possíveis.

### <a name="web-server-controls"></a>Controles de servidor Web

Na maioria dos ASP.NET Web Forms aplicativos, você adicionará controles que permitem que o usuário interaja com a página, como botões, caixas de texto, listas e assim por diante. Esses controles de servidor Web são semelhantes aos botões HTML e elementos de entrada. No entanto, eles são processados no servidor, permitindo que você use o código do servidor para definir suas propriedades. Esses controles também geram eventos que você pode manipular no código do servidor.

Os controles de servidor usam uma sintaxe especial que o ASP.NET reconhece quando a página é executada. O nome da marca para controles de servidor ASP.NET começa com um prefixo `asp:`. Isso permite que o ASP.NET reconheça e processe esses controles de servidor. O prefixo poderá ser diferente se o controle não fizer parte do .NET Framework. Além do prefixo de `asp:`, os controles de servidor ASP.NET também incluem o atributo `runat="server"` e uma `ID` que você pode usar para fazer referência ao controle no código do servidor.

Quando a página é executada, ASP.NET identifica os controles de servidor e executa o código associado a esses controles. Muitos controles renderizam algum HTML ou outra marcação na página quando ele é exibido em um navegador.

### <a name="server-code"></a>Código do servidor

A maioria das ASP.NET Web Forms aplicativos incluem o código que é executado no servidor quando a página é processada. Como mencionado acima, o código do servidor pode ser usado para fazer uma variedade de coisas, como adicionar dados a um controle ListView. O ASP.NET dá suporte a várias linguagens para execução no C#servidor, incluindo, Visual Basic, J# e outros.

O ASP.NET dá suporte a dois modelos de gravação de código de servidor para uma página da Web. No modelo de arquivo único, o código para a página está em um elemento de script em que a marca de abertura inclui o atributo `runat="server"`. Como alternativa, você pode criar o código para a página em um arquivo de classe separado, que é conhecido como modelo code-behind. Nesse caso, a página de Web Forms ASP.NET geralmente não contém nenhum código de servidor. Em vez disso, a diretiva `@Page` inclui informações que vinculam a página *. aspx* com seu arquivo code-behind associado.

O atributo `CodeBehind` contido na diretiva `@Page` especifica o nome do arquivo de classe separado e o atributo `Inherits` especifica o nome da classe dentro do arquivo code-behind que corresponde à página.

### <a name="updating-the-master-page"></a>Atualizando a página mestra

No ASP.NET Web Forms, as páginas mestras permitem que você crie um layout consistente para as páginas em seu aplicativo. Uma única página mestra define a aparência e o comportamento padrão que você deseja para todas as páginas (ou um grupo de páginas) em seu aplicativo. Em seguida, você pode criar páginas de conteúdo individuais que contêm o conteúdo que você deseja exibir, conforme explicado acima. Quando os usuários solicitam as páginas de conteúdo, o ASP.NET as mescla com a página mestra para produzir uma saída que combina o layout da página mestra com o conteúdo da página de conteúdo.

O novo site precisa de um único logotipo para ser exibido em cada página. Para adicionar esse logotipo, você pode modificar o HTML na página mestra.

1. Em **Gerenciador de soluções**, localize e abra a página **site. Master** .
2. Se a página estiver no modo **design** , alterne para o modo de exibição de **código-fonte** .
3. Atualize a página mestra **modificando ou adicionando** a marcação realçada em amarelo: 

    [!code-aspx[Main](ui_and_navigation/samples/sample4.aspx?highlight=9,49,76-81,87)]

Esse HTML exibirá a imagem chamada *logo. jpg* da pasta *imagens* do aplicativo Web, que você adicionará mais tarde. Quando uma página que usa a página mestra é exibida em um navegador, o logotipo será exibido. Se um usuário clicar no logotipo, o usuário navegará de volta para a página *Default. aspx* . A marca de âncora HTML `<a>` encapsula o controle de servidor de imagem e permite que a imagem seja incluída como parte do link. O atributo `href` para a marca Anchor especifica a raiz "`~/`" do site da Web como o local do link. Por padrão, a página *Default. aspx* é exibida quando o usuário navega para a raiz do site da Web. O controle de servidor de `<asp:Image>` de **imagem** inclui propriedades de adição, como `BorderStyle`, que são renderizadas como HTML quando exibidas em um navegador.

### <a name="master-pages"></a>Páginas mestras

Uma página mestra é um arquivo ASP.NET com a extensão. Master (por exemplo, *site. Master*) com um layout predefinido que pode incluir texto estático, elementos HTML e controles de servidor. A página mestra é identificada por uma diretiva de `@Master` especial que substitui a diretiva `@Page` usada para páginas *. aspx* comuns.

Além da diretiva `@Master`, a página mestra também contém todos os elementos HTML de nível superior de uma página, como `html`, `head`e `form`. Por exemplo, na página mestra que você adicionou acima, use um `table` HTML para o layout, um elemento `img` para o logotipo da empresa, texto estático e controles de servidor para lidar com a associação comum para seu site. Você pode usar qualquer HTML e qualquer elemento ASP.NET como parte de sua página mestra.

Além do texto estático e dos controles que serão exibidos em todas as páginas, a página mestra também inclui um ou mais controles **ContentPlaceHolder** . Esses controles de espaço reservado definem regiões em que o conteúdo substituível será exibido. Por sua vez, o conteúdo substituível é definido em páginas de conteúdo, como *Default. aspx*, usando o controle **Content** Server.

#### <a name="adding-image-files"></a>Adicionando arquivos de imagem

A imagem de logotipo que é referenciada acima, juntamente com todas as imagens de produto, deve ser adicionada ao aplicativo Web para que possa ser visto quando o projeto é exibido em um navegador.

#### <a name="download-from-msdn-samples-site"></a>Baixe no site de exemplos do MSDN:

[Introdução com ASP.NET 4,5 Web Forms e Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)

O download inclui recursos na pasta *WingtipToys-assets* que são usados para criar o aplicativo de exemplo.

1. Se você ainda não fez isso, baixe os arquivos de exemplo compactados usando o link acima do site de exemplos do MSDN.
2. Depois de baixado, abra o arquivo. zip e copie o conteúdo para uma pasta local em seu computador.
3. Localize e abra a pasta *WingtipToys-assets* .
4. Arrastando e soltando, copie a pasta de *Catálogo* da pasta local para a raiz do projeto de aplicativo Web no **Gerenciador de soluções** do Visual Studio. 

    ![Interface do usuário e navegação-arquivos de cópia](ui_and_navigation/_static/image1.png)
5. Em seguida, crie uma nova pasta chamada *imagens* clicando com o botão direito do mouse no projeto **WingtipToys** em **Gerenciador de Soluções** e selecionando **Adicionar** -&gt; **nova pasta**.
6. Copie o arquivo *logo. jpg* da pasta *WingtipToys-assets* no **Explorador de arquivos** para a pasta *imagens* do projeto de aplicativo Web em **Gerenciador de soluções** do Visual Studio.
7. Clique na opção **Mostrar todos os arquivos** na parte superior de **Gerenciador de soluções** para atualizar a lista de arquivos se você não vir os novos arquivos.  
  
    **Gerenciador de soluções** agora mostra os arquivos de projeto atualizados. 

    ![Interface do usuário e navegação-Gerenciador de Soluções](ui_and_navigation/_static/image2.png)

### <a name="adding-pages"></a>Adicionando páginas

Antes de adicionar navegação ao aplicativo Web, você primeiro adicionará duas novas páginas para as quais você navegará. Posteriormente nesta série de tutoriais, você exibirá produtos e detalhes do produto nessas novas páginas.

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse em **WingtipToys**, clique em **Adicionar**e em **novo item**.   
 A caixa de diálogo **Adicionar Novo Item** é exibida.
2. Selecione o grupo modelos **da Web** do **Visual C#**  -&gt; à esquerda. Em seguida, selecione **Web Form com página mestra** na lista intermediária e nomeie-o como *ProductList. aspx*. 

    ![IU e navegação – caixa de diálogo Adicionar novo item](ui_and_navigation/_static/image3.png)
3. Selecione **site. Master** para anexar a página mestra à página *. aspx* recém-criada. 

    ![Interface do usuário e navegação – selecionar página mestra](ui_and_navigation/_static/image4.png)
4. Adicione uma página adicional chamada *ProductDetails. aspx* seguindo estas mesmas etapas.

### <a name="updating-bootstrap"></a>Atualizando inicialização

Os modelos de projeto Visual Studio 2013 usam a [inicialização](http://getbootstrap.com/), um layout e estrutura de temas criados pelo Twitter. A inicialização usa o CSS3 para fornecer design responsivo, o que significa que os layouts podem se adaptar dinamicamente a diferentes tamanhos de janela do navegador. Você também pode usar o recurso de ti da Bootstrap para efetivar facilmente uma alteração na aparência do aplicativo. Por padrão, o modelo de aplicativo Web ASP.NET no Visual Studio 2013 inclui a inicialização como um pacote NuGet.

Neste tutorial, você irá alterar a aparência do aplicativo Wingtip Toys, substituindo os arquivos de inicialização do CSS.

1. Em **Gerenciador de soluções**, abra a pasta de *conteúdo* .
2. Clique com o botão direito do mouse no arquivo *bootstrap. css* e renomeie-o como *Bootstrap-original. css*.
3. Renomeie o *bootstrap. min. css* para *Bootstrap-original. min. css*.
4. Em **Gerenciador de soluções**, clique com o botão direito do mouse na pasta *conteúdo* e selecione **abrir pasta no explorador de arquivos**.  
   O explorador de arquivos será exibido. Você salvará os arquivos CSS de Bootstrap baixados nesse local.
5. No navegador, vá para [https://bootswatch.com/3/](https://bootswatch.com/3/).
6. Role a janela do navegador até ver o tema Cerulean. 

    ![Interface do usuário e navegação – tema Cerulean](ui_and_navigation/_static/image5.png)
7. Baixe o arquivo *bootstrap. css* e o arquivo *bootstrap. min. css* na pasta de *conteúdo* . Use o caminho para a pasta de conteúdo que é exibida na janela **Explorador de arquivos** que você abriu anteriormente.
8. No **Visual Studio** na parte superior de **Gerenciador de soluções**, selecione a opção **Mostrar todos os arquivos** para exibir os novos arquivos na pasta de conteúdo. 

    ![Interface do usuário e navegação-Gerenciador de Soluções](ui_and_navigation/_static/image6.png)

   Você verá os dois novos arquivos CSS na pasta **conteúdo** , mas observe que o ícone ao lado de cada nome de arquivo fica esmaecido. Isso significa que o arquivo ainda não foi adicionado ao projeto.
9. Clique com o botão direito do mouse nos arquivos *bootstrap. css* e *bootstrap. min. css* e selecione **incluir no projeto**.   
   Quando você executar o aplicativo Wingtip Toys posteriormente neste tutorial, a nova interface do usuário será exibida.

> [!NOTE] 
> 
> O modelo de aplicativo Web ASP.NET usa o arquivo *Bundle. config* na raiz do projeto para armazenar o caminho dos arquivos CSS de bootstrap.

### <a name="modifying-the-default-navigation"></a>Modificando a navegação padrão

A navegação padrão para cada página no aplicativo pode ser modificada alterando o elemento de lista de navegação não ordenado que está na página *site. Master* .

1. Em **Gerenciador de soluções**, localize e abra a página *site. Master* .
2. Adicione o link de navegação adicional realçado em amarelo à lista não ordenada mostrada abaixo:   

    [!code-html[Main](ui_and_navigation/samples/sample5.html?highlight=5)]

Como você pode ver no HTML acima, você modificou cada item de linha `<li>` contendo uma marca de ancoragem `<a>` com um link `href` atributo. Cada `href` aponta para uma página no aplicativo Web. No navegador, quando um usuário clica em um desses links (como **produtos**), ele navegará até a página contida na `href` (como **ProductList. aspx**). Você executará o aplicativo no final deste tutorial.

> [!NOTE] 
> 
> O caractere de til (`~`) é usado para especificar que o caminho de `href` começa na raiz do projeto.

### <a name="adding-a-data-control-to-display-navigation-data"></a>Adicionando um controle de dados para exibir dados de navegação

Em seguida, você adicionará um controle para exibir todas as categorias do banco de dados. Cada categoria atuará como um link para a página *ProductList. aspx* . Quando um usuário clica em um link de categoria no navegador, ele navega até a página produtos e vê apenas os produtos associados à categoria selecionada.

Você usará um controle **ListView** para exibir todas as categorias contidas no banco de dados. Para adicionar um controle **ListView** à página mestra:

1. Na página *site. Master* , adicione o seguinte elemento `<div>` realçado **após** o elemento `<div>` que contém o `id="TitleContent"` que você adicionou anteriormente:  

    [!code-aspx[Main](ui_and_navigation/samples/sample6.aspx?highlight=7-21)]

Esse código exibirá todas as categorias do banco de dados. O controle **ListView** exibe cada nome de categoria como texto de link e inclui um link para a página *ProductList. aspx* com um valor de cadeia de caracteres de consulta contendo o `ID` da categoria. Ao definir a propriedade `ItemType` no controle **ListView** , a expressão de vinculação de dados `Item` está disponível dentro do nó `ItemTemplate` e o controle se torna fortemente tipado. Você pode selecionar detalhes do objeto `Item` usando o IntelliSense, como especificar o `CategoryName`. Esse código está contido dentro do contêiner `<%#: %>` que marca uma expressão de vinculação de dados. Adicionando o (:) para o final do prefixo de `<%#`, o resultado da expressão de associação de dados é codificado em HTML. Quando o resultado é codificado em HTML, seu aplicativo é melhor protegido contra ataques de injeção de script entre sites (XSS) e injeção de HTML.

> [!NOTE] 
> 
> **Dicas**
> 
> Quando você adiciona código digitando durante o desenvolvimento, você pode ter certeza de que um membro válido de um objeto é encontrado porque os controles de dados com rigidez de tipos mostram os membros disponíveis com base no IntelliSense. O IntelliSense oferece opções de código apropriadas ao contexto à medida que você digita o código, como propriedades, métodos e objetos.

Na próxima etapa, você implementará o método `GetCategories` para recuperar dados.

### <a name="linking-the-data-control-to-the-database"></a>Vinculando o controle de dados ao banco de dado

Antes de poder exibir dados no controle de dados, você precisa vincular o controle de dados ao banco de dado. Para fazer o link, você pode modificar o código por trás do arquivo *site.master.cs* .

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse na página *site. Master* e clique em **Exibir código**. O arquivo *site.master.cs* é aberto no editor.
2. Próximo ao início do arquivo *site.master.cs* , adicione dois namespaces adicionais para que todos os namespaces incluídos sejam exibidos da seguinte maneira:  

    [!code-csharp[Main](ui_and_navigation/samples/sample7.cs?highlight=8-9)]
3. Adicione o método realçado `GetCategories` após o manipulador de eventos `Page_Load` da seguinte maneira:  

    [!code-csharp[Main](ui_and_navigation/samples/sample8.cs?highlight=6-11)]

O código acima é executado quando qualquer página que usa a página mestra é carregada no navegador. O controle de `ListView` (chamado de "CategoryList") que você adicionou anteriormente neste tutorial usa a associação de modelo para selecionar dados. Na marcação do controle de `ListView`, você define a propriedade `SelectMethod` do controle como o método `GetCategories`, mostrado acima. O controle de `ListView` chama o método `GetCategories` no momento apropriado no ciclo de vida da página e associa os dados retornados automaticamente. Você aprenderá mais sobre a vinculação de dados no próximo tutorial.

### <a name="running-the-application-and-creating-the-database"></a>Executando o aplicativo e criando o banco de dados

Anteriormente nesta série de tutoriais, você criou uma classe de inicializador (chamada "ProductDatabaseInitializer") e especificou essa classe no arquivo *global.asax.cs* . O Entity Framework gerará o banco de dados quando o aplicativo for executado pela primeira vez, pois o método `Application_Start` contido no arquivo *global.asax.cs* chamará a classe do inicializador. A classe do inicializador usará as classes de modelo (`Category` e `Product`) que você adicionou anteriormente nesta série de tutoriais para criar o banco de dados.

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse na página *Default. aspx* e selecione **definir como página inicial**.
2. No Visual Studio, pressione **F5**.   
 Levará algum tempo para configurar tudo durante a primeira execução.   
    interface do usuário ![e navegação-Windows](ui_and_navigation/_static/image7.png)  
 Quando você executar o aplicativo, o aplicativo será compilado e o banco de dados chamado *wingtiptoys. MDF* será criado na pasta *\_data do aplicativo* . No navegador, você verá um menu de navegação de categoria. Esse menu foi gerado recuperando-se as categorias do banco de dados. No próximo tutorial, você implementará a navegação.
3. Feche o navegador para interromper o aplicativo em execução.

### <a name="reviewing-the-database"></a>Revisando o banco de dados

Abra o arquivo *Web. config* e examine a seção cadeia de conexão. Você pode ver que o valor `AttachDbFilename` na cadeia de conexão aponta para o `DataDirectory` do projeto de aplicativo Web. O valor `|DataDirectory|` é um valor reservado que representa a pasta de *dados do aplicativo\_* no projeto. Essa pasta é onde o banco de dados criado a partir de suas classes de entidade está localizado.

[!code-xml[Main](ui_and_navigation/samples/sample9.xml)]

> [!NOTE] 
> 
> Se a pasta *dados do\_de aplicativos* não estiver visível ou se a pasta estiver vazia, selecione o ícone **Atualizar** e, em seguida, o ícone **Mostrar todos os arquivos** na parte superior da janela **Gerenciador de soluções** . Expandir a largura do **Gerenciador de soluções** Windows pode ser necessário para mostrar todos os ícones disponíveis.

Agora você pode inspecionar os dados contidos no arquivo de banco de *wingtiptoys. MDF* usando a janela **Gerenciador de servidores** .

1. Expanda a pasta de *dados do\_de aplicativos* . Se a pasta de *dados do\_de aplicativos* não estiver visível, consulte a observação acima.
2. Se o arquivo de banco de dados *wingtiptoys. MDF* não estiver visível, selecione o ícone de **atualização** e, em seguida, o ícone **Mostrar todos os arquivos** na parte superior da janela de **Gerenciador de soluções** .
3. Clique com o botão direito do mouse no arquivo de banco de dados *wingtiptoys. MDF* e selecione **abrir**.  
    **Gerenciador de servidores** é exibido. 

    ![Interface do usuário e navegação-Gerenciador de Servidores](ui_and_navigation/_static/image8.png)
4. Expanda a pasta *tabelas* .
5. Clique com o botão direito do mouse na tabela **produtos**e selecione **Mostrar dados da tabela**.  
 A tabela **Products** é exibida. 

    ![Interface de usuário e a tabela produtos de navegação](ui_and_navigation/_static/image9.png)
6. Essa exibição permite que você veja e modifique os dados na tabela **produtos** manualmente.
7. Feche a janela **produtos** tabela.
8. Na **Gerenciador de servidores**, clique com o botão direito do mouse na tabela **produtos** novamente e selecione **Abrir definição de tabela**.  
 O design de dados para a tabela **produtos** é exibido. 

    ![Interface do usuário e navegação – design de produtos](ui_and_navigation/_static/image10.png)
9. Na guia **T-SQL** , você verá a instrução SQL DDL que foi usada para criar a tabela. Você também pode usar a interface do usuário na guia **design** para modificar o esquema.
10. Na **Gerenciador de servidores**, clique com o botão direito do mouse em banco de dados **WingtipToys** e selecione **fechar conexão**.   
 Desanexando o banco de dados do Visual Studio, o esquema de banco de dados poderá ser modificado posteriormente nesta série de tutoriais.
11. Volte para **Gerenciador de soluções**selecionando a guia **Gerenciador de soluções** na parte inferior da janela **Gerenciador de servidores** .

## <a name="summary"></a>Resumo

Neste tutorial da série, você adicionou algumas interfaces do usuário, elementos gráficos, páginas e navegação básicos. Além disso, você executou o aplicativo Web, que criou o banco de dados a partir das classes que você adicionou no tutorial anterior. Você também exibiu o conteúdo da tabela *produtos* do banco de dados exibindo o banco de dados diretamente. No próximo tutorial, você exibirá itens e detalhes do banco de dados.

## <a name="additional-resources"></a>Recursos adicionais

[Introdução à programação Páginas da Web do ASP.NET](https://msdn.microsoft.com/library/ms178125.aspx)   
[Visão geral dos controles de servidor Web ASP.NET](https://msdn.microsoft.com/library/zsyt68f1.aspx)   
[Tutorial do CSS](http://www.w3schools.com/css/default.asp)

> [!div class="step-by-step"]
> [Anterior](create_the_data_access_layer.md)
> [Próximo](display_data_items_and_details.md)
