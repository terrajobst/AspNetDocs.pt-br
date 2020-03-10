---
uid: mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-vb
title: Usando o ASP.NET MVC com versões diferentes do IIS (VB) | Microsoft Docs
author: microsoft
description: Neste tutorial, você aprenderá a usar o ASP.NET MVC e o roteamento de URL, com diferentes versões do Serviços de Informações da Internet. Você aprende estratégias diferentes...
ms.author: riande
ms.date: 08/19/2008
ms.assetid: 1c1283b2-6956-4937-b568-d30de432ce23
msc.legacyurl: /mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-vb
msc.type: authoredcontent
ms.openlocfilehash: b754175c853c20eec6be3521376b62d62f33106d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78581833"
---
# <a name="using-aspnet-mvc-with-different-versions-of-iis-vb"></a>Uso do ASP.NET MVC com diferentes versões de IIS (VB)

pela [Microsoft](https://github.com/microsoft)

> Neste tutorial, você aprenderá a usar o ASP.NET MVC e o roteamento de URL, com diferentes versões do Serviços de Informações da Internet. Você aprende estratégias diferentes para usar o ASP.NET MVC com o IIS 7,0 (modo clássico), o IIS 6,0 e versões anteriores do IIS.

A estrutura MVC do ASP.NET depende do roteamento do ASP.NET para rotear solicitações do navegador para ações do controlador. Para aproveitar o roteamento do ASP.NET, talvez seja necessário executar etapas de configuração adicionais no servidor Web. Tudo depende da versão do Serviços de Informações da Internet (IIS) e do modo de processamento de solicitação para seu aplicativo.

Aqui está um resumo das diferentes versões do IIS:

- IIS 7,0 (modo integrado) – nenhuma configuração especial necessária para usar o roteamento ASP.NET.
- IIS 7,0 (modo clássico)-você precisa executar uma configuração especial para usar o roteamento ASP.NET.
- IIS 6,0 ou inferior-você precisa executar uma configuração especial para usar o roteamento ASP.NET.

A versão mais recente do IIS é a versão 7,5 (no Win7). O IIS 7 do IIS está incluído no Windows Server 2008 e VISTA/SP1 e superior. Você também pode instalar o IIS 7,0 em qualquer versão do sistema operacional Vista, exceto a Home Basic (consulte [https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx](https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx)).

O IIS 7,0 dá suporte a dois modos de processamento de solicitações. Você pode usar o modo integrado ou clássico. Você não precisa executar nenhuma etapa especial de configuração ao usar o IIS 7,0 no modo integrado. No entanto, você precisa executar uma configuração adicional ao usar o IIS 7,0 no modo clássico.

O Microsoft Windows Server 2003 inclui o IIS 6,0. Não é possível atualizar o IIS 6,0 para o IIS 7,0 ao usar o sistema operacional Windows Server 2003. Você deve executar etapas de configuração adicionais ao usar o IIS 6,0.

O Microsoft Windows XP Professional inclui o IIS 5,1. Você deve executar etapas de configuração adicionais ao usar o IIS 5,1.

Por fim, o Microsoft Windows 2000 e o Microsoft Windows 2000 Professional incluem o IIS 5,0. Você deve executar etapas de configuração adicionais ao usar o IIS 5,0.

## <a name="integrated-versus-classic-mode"></a>Modo integrado versus clássico

O IIS 7,0 pode processar solicitações usando dois modos de processamento de solicitação diferentes: integrado e clássico. O modo integrado fornece melhor desempenho e mais recursos. O modo clássico está incluído para compatibilidade com versões anteriores do IIS.

O modo de processamento de solicitação é determinado pelo pool de aplicativos. Você pode determinar qual modo de processamento está sendo usado por um determinado aplicativo Web determinando o pool de aplicativos associado ao aplicativo. Siga estas etapas:

1. Iniciar o Gerenciador de Serviços de Informações da Internet
2. Na janela conexões, selecione um aplicativo
3. Na janela ações, clique no link **configurações básicas** para abrir a caixa de diálogo Editar aplicativo (veja a Figura 1)
4. Anote o pool de aplicativos selecionado.

Por padrão, o IIS é configurado para dar suporte a dois pools de aplicativos: **DefaultAppPool** e o **AppPool .net clássico**. Se DefaultAppPool estiver selecionado, o aplicativo estará sendo executado no modo de processamento de solicitação integrado. Se o AppPool clássico do .NET estiver selecionado, seu aplicativo estará sendo executado no modo de processamento de solicitação clássico.

[![caixa de diálogo novo projeto](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image1.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image1.png)

**Figura 1**: detectando o modo de processamento de solicitação ([clique para exibir a imagem em tamanho normal](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image2.png))

Observe que você pode modificar o modo de processamento de solicitação na caixa de diálogo Editar aplicativo. Clique no botão Selecionar e altere o pool de aplicativos associado ao aplicativo. Observe que há problemas de compatibilidade ao alterar um aplicativo ASP.NET do modo clássico para o integrado. Para obter mais informações, consulte os seguintes artigos:

- Atualizando o ASP.NET 1,1 para o IIS 7,0 no Windows Vista e no Windows Server 2008-- [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008)

- Integração do ASP.NET com o IIS 7,0- [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)

Se um aplicativo ASP.NET estiver usando DefaultAppPool, você não precisará executar outras etapas para obter o roteamento do ASP.NET (e, portanto, o ASP.NET MVC) para funcionar. No entanto, se o aplicativo ASP.NET estiver configurado para usar o AppPool clássico do .NET, continue lendo, você terá mais trabalho a fazer.

## <a name="using-aspnet-mvc-with-older-versions-of-iis"></a>Usando o ASP.NET MVC com versões mais antigas do IIS

Se você precisar usar o ASP.NET MVC com uma versão mais antiga do IIS do que o IIS 7,0 ou precisar usar o IIS 7,0 no modo clássico, terá duas opções. Primeiro, você pode modificar a tabela de rotas para usar extensões de arquivo. Por exemplo, em vez de solicitar uma URL como/Store/Details, você solicitaria uma URL como/Store.aspx/Details.

A segunda opção é criar algo chamado de *mapa de script curinga*. Um mapa de script curinga permite mapear cada solicitação para a estrutura ASP.NET.

Se você não tiver acesso ao seu servidor Web (por exemplo, seu aplicativo ASP.NET MVC está sendo hospedado por um provedor de serviços de Internet), você precisará usar a primeira opção. Se você não quiser modificar a aparência de suas URLs e tiver acesso ao seu servidor Web, poderá usar a segunda opção.

Exploraremos cada opção detalhadamente nas seções a seguir.

## <a name="adding-extensions-to-the-route-table"></a>Adicionando extensões à tabela de rotas

A maneira mais fácil de fazer o roteamento do ASP.NET funcionar com versões mais antigas do IIS é modificar a tabela de rotas no arquivo global. asax. O arquivo global. asax padrão e não modificado na Listagem 1 configura uma rota chamada de rota padrão.

**Listagem 1-global. asax (não modificado)**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample1.vb)]

A rota padrão configurada na Listagem 1 permite rotear URLs parecidas com esta:

/Home/Index

/Product/Details/3

/Product

Infelizmente, as versões anteriores do IIS não passarão essas solicitações para a estrutura ASP.NET. Portanto, essas solicitações não serão roteadas para um controlador. Por exemplo, se você fizer uma solicitação de navegador para a URL/Home/Index, obterá a página de erro na Figura 2.

[![caixa de diálogo novo projeto](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image2.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image3.png)

**Figura 2**: recebendo um erro 404 não encontrado ([clique para exibir a imagem em tamanho normal](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image4.png))

As versões mais antigas do IIS mapeiam apenas determinadas solicitações para a estrutura ASP.NET. A solicitação deve ser para uma URL com a extensão de arquivo correta. Por exemplo, uma solicitação de/SomePage.aspx é mapeada para a estrutura ASP.NET. No entanto, uma solicitação de/SomePage.htm não.

Portanto, para que o roteamento do ASP.NET funcione, devemos modificar a rota padrão para que ela inclua uma extensão de arquivo mapeada para a estrutura ASP.NET.

Isso é feito usando um script chamado `registermvc.wsf`. Ele foi incluído com a versão ASP.NET MVC 1 em `C:\Program Files\Microsoft ASP.NET\ASP.NET MVC\Scripts`, mas a partir de ASP.NET 2, esse script foi movido para o ASP.NET Futures, disponível em [http://aspnet.codeplex.com/releases/view/39978](http://aspnet.codeplex.com/releases/view/39978).

A execução desse script registra uma nova extensão. Mvc com o IIS. Depois de registrar a extensão. Mvc, você pode modificar suas rotas no arquivo global. asax para que as rotas usem a extensão. Mvc.

O arquivo global. asax modificado na Listagem 2 funciona com versões mais antigas do IIS.

**Listagem 2-global. asax (modificado com extensões)**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample2.vb)]

Importante: Lembre-se de criar seu aplicativo ASP.NET MVC novamente após alterar o arquivo global. asax.

Há duas alterações importantes no arquivo global. asax na Listagem 2. Agora há duas rotas definidas no global. asax. O padrão de URL para a rota padrão, a primeira rota, agora é semelhante a:

{controller}.mvc/{action}/{id}

A adição da extensão. Mvc altera o tipo de arquivos interceptados pelo módulo de roteamento ASP.NET. Com essa alteração, o aplicativo ASP.NET MVC agora roteia solicitações como as seguintes:

/Home.mvc/Index/

/Product.mvc/Details/3

/Product.mvc/

A segunda rota, a rota raiz, é nova. Esse padrão de URL para a rota raiz é uma cadeia de caracteres vazia. Essa rota é necessária para solicitações correspondentes feitas em relação à raiz do seu aplicativo. Por exemplo, a rota raiz corresponderá a uma solicitação parecida com esta:

[http://www.YourApplication.com/](http://www.YourApplication.com/)

Depois de fazer essas modificações em sua tabela de rotas, você precisará certificar-se de que todos os links em seu aplicativo sejam compatíveis com esses novos padrões de URL. Em outras palavras, certifique-se de que todos os links incluem a extensão. Mvc. Se você usar o método auxiliar HTML. ActionLink () para gerar seus links, não precisará fazer nenhuma alteração.

Em vez de usar o script registermvc. WCF, você pode adicionar uma nova extensão ao IIS que é mapeada para a estrutura ASP.NET manualmente. Ao adicionar uma nova extensão por conta própria, certifique-se de que a caixa de seleção **Verifique se o arquivo existe** não está marcada.

## <a name="hosted-server"></a>Servidor hospedado

Você nem sempre tem acesso ao seu servidor Web. Por exemplo, se você estiver hospedando seu aplicativo MVC ASP.NET usando um provedor de hospedagem na Internet, não terá necessariamente acesso ao IIS.

Nesse caso, você deve usar uma das extensões de arquivo existentes que são mapeadas para a estrutura ASP.NET. Exemplos de extensões de arquivo mapeadas para ASP.NET incluem as extensões. aspx,. axd e. ashx.

Por exemplo, o arquivo global. asax modificado na Listagem 3 usa a extensão. aspx em vez da extensão. Mvc.

**Listagem 3-global. asax (modificado com extensões. aspx)**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample3.vb)]

O arquivo global. asax na Listagem 3 é exatamente o mesmo que o arquivo global. asax anterior, exceto pelo fato de que ele usa a extensão. aspx em vez da extensão. Mvc. Você não precisa executar nenhuma configuração em seu servidor Web remoto para usar a extensão. aspx.

## <a name="creating-a-wildcard-script-map"></a>Criando um mapa de script curinga

Se você não quiser modificar as URLs para seu aplicativo ASP.NET MVC e tiver acesso ao seu servidor Web, terá uma opção adicional. Você pode criar um mapa de script curinga que mapeia todas as solicitações para o servidor Web para a estrutura ASP.NET. Dessa forma, você pode usar a tabela de rotas padrão do ASP.NET MVC com o IIS 7,0 (no modo clássico) ou IIS 6,0.

Lembre-se de que essa opção faz com que o IIS Intercepte cada solicitação feita no servidor Web. Isso inclui solicitações de imagens, páginas ASP clássicas e páginas HTML. Portanto, a habilitação de um mapa de script curinga para ASP.NET tem implicações de desempenho.

Veja como habilitar um mapa de script curinga para o IIS 7,0:

1. Selecione seu aplicativo na janela conexões
2. Verifique se a exibição **recursos** está selecionada
3. Clique duas vezes no botão **mapeamentos de manipulador**
4. Clique no link **adicionar mapa de script curinga** (veja a Figura 3)
5. Insira o caminho para o arquivo Aspnet\_ISAPI. dll (você pode copiar esse caminho do mapa de script do PageHandlerFactory)
6. Insira o nome MVC
7. Clique no botão **OK**

[![caixa de diálogo novo projeto](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image3.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image5.png)

**Figura 3**: Criando um mapa de script curinga com o IIS 7,0 ([clique para exibir a imagem em tamanho normal](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image6.png))

Siga estas etapas para criar um mapa de script curinga com o IIS 6,0:

1. Clique com o botão direito do mouse em um site e selecione Propriedades
2. Selecione a guia **diretório base**
3. Clique no botão **configuração**
4. Selecione a guia **mapeamentos**
5. Clique no botão **Inserir** (veja a Figura 4)
6. Cole o caminho para o ASPNET\_ISAPI. dll no campo executável (você pode copiar esse caminho do mapa de script para arquivos. aspx)
7. Desmarque a caixa de seleção rotulada verificar se o **arquivo existe**
8. Clique no botão **OK**

[![caixa de diálogo novo projeto](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image4.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image7.png)

**Figura 4**: Criando um mapa de script curinga com o IIS 6,0 ([clique para exibir a imagem em tamanho normal](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image8.png))

Depois de habilitar mapas de script curinga, você precisa modificar a tabela de rotas no arquivo global. asax para que ele inclua uma rota raiz. Caso contrário, você receberá a página de erro na Figura 5 quando fizer uma solicitação para a página raiz do seu aplicativo. Você pode usar o arquivo global. asax modificado na Listagem 4.

[![caixa de diálogo novo projeto](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image5.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image9.png)

**Figura 5**: erro de rota de raiz ausente ([clique para exibir a imagem em tamanho normal](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image10.png))

**Listagem 4-global. asax (modificado com rota de raiz)**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample4.vb)]

Depois de habilitar um mapa de script curinga para o IIS 7,0 ou o IIS 6,0, você pode fazer solicitações que funcionam com a tabela de rotas padrão parecida com esta:

/

/Home/Index

/Product/Details/3

/Product

## <a name="summary"></a>Resumo

O objetivo deste tutorial foi explicar como você pode usar o ASP.NET MVC ao usar uma versão mais antiga do IIS (ou IIS 7,0 no modo clássico). Discutimos dois métodos de obter o roteamento ASP.NET para trabalhar com versões anteriores do IIS: modificar a tabela de rotas padrão ou criar um mapa de script curinga.

A primeira opção exige que você modifique as URLs usadas em seu aplicativo ASP.NET MVC. Uma vantagem muito significativa dessa primeira opção é que você não precisa ter acesso a um servidor Web para modificar a tabela de rotas. Isso significa que você pode usar essa primeira opção mesmo ao hospedar seu aplicativo MVC ASP.NET com uma empresa de hospedagem na Internet.

A segunda opção é criar um mapa de script curinga. A vantagem dessa segunda opção é que você não precisa modificar suas URLs. A desvantagem dessa segunda opção é que ela pode afetar o desempenho do seu aplicativo ASP.NET MVC.

> [!div class="step-by-step"]
> [Anterior](using-asp-net-mvc-with-different-versions-of-iis-cs.md)
