---
uid: web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
title: Introdução ao ASP.NET Web API 2 (C#)-ASP.NET 4. x
author: MikeWasson
description: Tutorial com código. Use ASP.NET Web API para criar uma API Web que retorna uma lista de produtos.
ms.author: riande
ms.date: 11/28/2017
ms.custom: seoapril2019
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
msc.type: authoredcontent
ms.openlocfilehash: 2717d93f47be9d4a6548731d8deeca312b25f39f
ms.sourcegitcommit: 9e3ca74997a67c18589729d4b7303799905473eb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79084046"
---
# <a name="get-started-with-aspnet-web-api-2-c"></a>Introdução ao ASP.NET Web API 2 (C#)

por [Mike Wasson](https://github.com/MikeWasson)

[Baixar projeto concluído](https://code.msdn.microsoft.com/Sample-code-of-Getting-c56ccb28)

Neste tutorial, você usará a API Web ASP.NET para criar uma API Web que retorna uma lista de produtos.

HTTP não serve apenas para servir páginas da Web. O HTTP também é uma plataforma poderosa para a criação de APIs que expõem serviços e dados. O HTTP é simples, flexível e onipresente. Quase todas as plataformas que você pode considerar têm uma biblioteca HTTP, portanto, os serviços HTTP podem alcançar uma ampla variedade de clientes, incluindo navegadores, dispositivos móveis e aplicativos de desktops tradicionais.

A API Web ASP.NET é uma estrutura para a criação de APIs Web sobre o .NET Framework. 

## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial

- [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
- API Web 2

Consulte [criar uma API Web com o ASP.NET Core e o Visual Studio para Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) para obter uma versão mais recente deste tutorial.

## <a name="create-a-web-api-project"></a>Criar um projeto de API Web

Neste tutorial, você usará a API Web ASP.NET para criar uma API Web que retorna uma lista de produtos. A página da Web front-end usa jQuery para exibir os resultados.

![](tutorial-your-first-web-api/_static/image1.png)

Inicie o Visual Studio e selecione **novo projeto** na página **inicial** . Ou, no menu **arquivo** , selecione **novo** e **projeto**.

No painel **modelos** , selecione **modelos instalados** e expanda o **nó C# Visual** . Em **Visual C#** , selecione **Web**. Na lista de modelos de projeto, selecione **aplicativo Web ASP.net**. Nomeie o projeto "ProductsApp" e clique em **OK**.

![](tutorial-your-first-web-api/_static/image2.png)

Na caixa de diálogo **novo projeto ASP.net** , selecione o modelo **vazio** . Em &quot;adicionar pastas e referências principais para&quot;, verifique a **API Web**. Clique em **OK**.

![](tutorial-your-first-web-api/_static/image3.png)

> [!NOTE]
> Você também pode criar um projeto de API Web usando o modelo &quot;API Web&quot;. O modelo de Web API usa o ASP.NET MVC para fornecer páginas de Ajuda da API. Estou usando o modelo vazio para este tutorial porque Mostrar Web API sem MVC. Em geral, você não precisa saber o ASP.NET MVC para usar a Web API.

## <a name="adding-a-model"></a>Adicionar um modelo

Um *modelo* é um objeto que representa os dados no seu aplicativo. ASP.NET Web API pode serializar automaticamente seu modelo para outro formato, XML ou JSON, e, em seguida, gravar os dados serializados no corpo da mensagem de resposta HTTP. Como um cliente pode ler o formato de serialização, ele pode desserializar o objeto. A maioria dos clientes pode analisar XML ou JSON. Além disso, o cliente pode indicar qual formato ele deseja definindo o cabeçalho Accept na mensagem de solicitação HTTP.

Vamos começar criando um modelo simples que representa um produto.

Se o Gerenciador de Soluções não estiver visível, clique no menu **Exibir** e selecione **Gerenciador de Soluções**. No Gerenciador de Soluções, clique com o botão direito do mouse na pasta Modelos. No menu de contexto, selecione **Adicionar** e, em seguida, selecione **Classe**.

![](tutorial-your-first-web-api/_static/image4.png)

Nomeie a classe &quot;produto&quot;. Adicione as propriedades a seguir à classe `Product`.

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample1.cs)]

## <a name="adding-a-controller"></a>Adicionando um controlador

Na API da Web, um *controlador* é um objeto que manipula as solicitações HTTP. Vamos adicionar um controlador que pode retornar uma lista de produtos ou um único produto especificado por ID.

> [!NOTE]
> Se você tiver usado o ASP.NET MVC, você já está familiarizados com os controladores. Os controladores de API da Web são semelhantes aos controladores MVC, mas herdam a classe **ApiController** em vez da classe **Controller** .

No **Gerenciador de Soluções**, clique com o botão direito do mouse na pasta Controladores. Selecione **Adicionar** e, em seguida, selecione **Controlador**.

![](tutorial-your-first-web-api/_static/image5.png)

Na caixa de diálogo **Adicionar Scaffold**, selecione **Controlador da API Web – Vazio**. Clique em **Adicionar**.

![](tutorial-your-first-web-api/_static/image6.png)

Na caixa de diálogo **Adicionar controlador** , nomeie o controlador &quot;ProductsController&quot;. Clique em **Adicionar**.

![](tutorial-your-first-web-api/_static/image7.png)

O scaffolding cria um arquivo chamado ProductsController.cs na pasta controladores.

![](tutorial-your-first-web-api/_static/image8.png)

> [!NOTE]
> Você não precisa colocar seus controladores em uma pasta chamada Controllers. O nome da pasta é apenas uma maneira conveniente de organizar os arquivos de origem.

Se esse arquivo não estiver aberto, clique duas vezes no arquivo para abri-lo. Substitua o código deste arquivo pelo seguinte:

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample2.cs)]

Para manter o exemplo simples, os produtos são armazenados em uma matriz fixa dentro da classe do controlador. É claro que, em um aplicativo real, você consulta um banco de dados ou usar outra fonte de dados externa.

O controlador define dois métodos que retornam produtos:

- O método `GetAllProducts` retorna a lista completa de produtos como um tipo de **&gt;de produto IEnumerable&lt;** .
- O método `GetProduct` pesquisa um único produto por sua ID.

É isso! Você está trabalhando em uma API Web. Cada método do controlador corresponde a um ou mais URIs:

| Método de controlador | URI |
| --- | --- |
| GetAllProducts | /api/products |
| Getproduct | *ID* do/API/Products/ |

Para o método `GetProduct`, a *ID* no URI é um espaço reservado. Por exemplo, para obter o produto com a ID 5, o URI é `api/products/5`.

Para obter mais informações sobre como a API Web roteia solicitações HTTP para métodos do controlador, consulte [Roteamento em ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).

## <a name="calling-the-web-api-with-javascript-and-jquery"></a>Chamar a API Web com Javascript e jQuery

Nesta seção, vamos adicionar uma página HTML que usa AJAX para chamar a API Web. Vamos usar jQuery para fazer chamadas AJAX e também para atualizar a página com os resultados.

Em Gerenciador de Soluções, clique com o botão direito do mouse no projeto e selecione **Adicionar**e, em seguida, selecione **novo item**.

![](tutorial-your-first-web-api/_static/image9.png)

Na caixa de diálogo **Adicionar novo item** , selecione o nó **Web** em **C#Visual**e, em seguida, selecione o item **página HTML** . Nomeie a página &quot;index. html&quot;.

![](tutorial-your-first-web-api/_static/image10.png)

Substitua tudo neste arquivo pelo seguinte:

[!code-html[Main](tutorial-your-first-web-api/samples/sample3.html)]

Há várias maneiras de obter o jQuery. Neste exemplo, usei a CDN do [Microsoft Ajax](../../../ajax/cdn/overview.md). Você também pode baixá-lo de [http://jquery.com/](http://jquery.com/)e o modelo de projeto ASP.net "API Web" também inclui o jQuery.

### <a name="getting-a-list-of-products"></a>Obtendo uma lista de produtos

Para obter uma lista de produtos, envie uma solicitação HTTP GET para &quot;/API/Products&quot;.

A função [getJson](http://api.jquery.com/jQuery.getJSON/) do jQuery envia uma solicitação Ajax. A resposta contém a matriz de objetos JSON. A função `done` especifica um retorno de chamada que é chamado se a solicitação for realizada com sucesso. No retorno de chamada, atualizamos o DOM com as informações de produto.

[!code-html[Main](tutorial-your-first-web-api/samples/sample4.html)]

### <a name="getting-a-product-by-id"></a>Obtendo um produto por ID

Para obter um produto por ID, envie uma solicitação HTTP GET para &quot;*ID* /API/Products/&quot;, em que *ID* é a ID do produto.

[!code-javascript[Main](tutorial-your-first-web-api/samples/sample5.js)]

Ainda chamamos `getJSON` para enviar a solicitação AJAX, mas desta vez colocamos a ID no URI de solicitação. A resposta dessa solicitação é uma representação JSON de um único produto.

## <a name="running-the-application"></a>Executando o aplicativo

Pressione F5 para iniciar a depuração do aplicativo. A página da Web deve ser parecida com a seguinte:

![](tutorial-your-first-web-api/_static/image11.png)

Para obter um produto por ID, insira a ID e clique em Pesquisar:

![](tutorial-your-first-web-api/_static/image12.png)

Se você inserir uma ID inválida, o servidor retornará um erro HTTP:

![](tutorial-your-first-web-api/_static/image13.png)

## <a name="using-f12-to-view-the-http-request-and-response"></a>Usando F12 para exibir a solicitação e a resposta HTTP

Quando você está trabalhando com um serviço HTTP, pode ser muito útil ver as mensagens de solicitação e resposta HTTP. Você pode fazer isso usando as ferramentas de desenvolvedor F12 no Internet Explorer 9. No Internet Explorer 9, pressione **F12** para abrir as ferramentas. Clique na guia **rede** e pressione **Iniciar captura**. Agora, volte para a página da Web e pressione **F5** para recarregar a página da Web. O Internet Explorer capturará o tráfego HTTP entre o navegador e o servidor Web. A exibição de resumo mostra todo o tráfego de rede de uma página:

![](tutorial-your-first-web-api/_static/image14.png)

Localize a entrada para o URI relativo "api/produtos /". Selecione essa entrada e clique em **ir para exibição detalhada**. Na exibição detalhada, há guias para exibir os corpos e cabeçalhos de solicitação e resposta. Por exemplo, se você clicar na guia **cabeçalhos de solicitação** , poderá ver que o cliente solicitou &quot;aplicativo/JSON&quot; no cabeçalho Accept.

![](tutorial-your-first-web-api/_static/image15.png)

Se você clicar na guia corpo da resposta, poderá ver como a lista de produtos foi serializada para JSON. Outros navegadores têm funcionalidade semelhante. Outra ferramenta útil é o [Fiddler](http://www.fiddler2.com/fiddler2/), um proxy de depuração da Web. Você pode usar o Fiddler para exibir o tráfego HTTP e também para compor solicitações HTTP, o que lhe dá controle total sobre os cabeçalhos HTTP na solicitação.

## <a name="see-this-app-running-on-azure"></a>Consulte este aplicativo em execução no Azure

Você gostaria de ver o site concluído em execução como um aplicativo Web ativo? Você pode implantar uma versão completa do aplicativo em sua conta do Azure simplesmente clicando no botão a seguir.

[![](https://azuredeploy.net/deploybutton.png)](https://deploy.azure.com/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebAPI-ProductsApp#/form/setup)

Você precisa de uma conta do Azure para implantar essa solução no Azure. Se você ainda não tiver uma conta, terá as seguintes opções:

- [Abra uma conta do Azure gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -você obtém créditos que podem ser usados para experimentar serviços pagos do Azure e, mesmo depois que eles são usados, você pode manter a conta e usar os serviços gratuitos do Azure.
- [Ativar os benefícios do assinante do MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -sua assinatura do MSDN fornece créditos todos os meses que você pode usar para serviços pagos do Azure.

## <a name="next-steps"></a>Próximas etapas

- Para obter um exemplo mais completo de um serviço HTTP que dá suporte a ações POST, PUT e DELETE e gravações em um banco de dados, consulte [usando a API da Web 2 com Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).
- Para obter mais informações sobre a criação de aplicativos Web fluidos e responsivos sobre um serviço HTTP, consulte [aplicativo de página única do ASP.net](../../../single-page-application/index.md).
- Para obter informações sobre como implantar um projeto Web do Visual Studio para Azure App serviço, consulte [criar um aplicativo web ASP.net no serviço Azure app](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).
