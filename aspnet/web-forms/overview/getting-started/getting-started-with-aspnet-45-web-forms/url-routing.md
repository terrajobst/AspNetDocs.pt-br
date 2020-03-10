---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
title: Roteamento de URL | Microsoft Docs
author: Erikre
description: Esta série de tutoriais ensinará as noções básicas da criação de um aplicativo ASP.NET Web Forms usando o ASP.NET 4,5 e o Microsoft Visual Studio Express 2013 para nós...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 4f4bf092-c400-471f-a876-78fda0417890
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
msc.type: authoredcontent
ms.openlocfilehash: 66b727b69ca4f9a3d35b67f492f9a554146e09ef
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78587433"
---
# <a name="url-routing"></a>Roteamento de URL

por [Erik Reitan](https://github.com/Erikre)

[Baixar o projeto de exemplo WingtipC#Toys ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [baixar o livro eletrônico (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta série de tutoriais ensinará as noções básicas da criação de um aplicativo ASP.NET Web Forms usando o ASP.NET 4,5 e o Microsoft Visual Studio Express 2013 para a Web. Um [projeto Visual Studio 2013 com C# código-fonte](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) está disponível para acompanhar esta série de tutoriais.

Neste tutorial, você modificará o aplicativo de exemplo Wingtip Toys para dar suporte ao roteamento de URL. O roteamento permite que seu aplicativo Web Use URLs amigáveis, fáceis de lembrar e melhor com suporte dos mecanismos de pesquisa. Este tutorial se baseia no tutorial anterior "associação e administração" e faz parte da série de tutoriais da Wingtip Toys.

## <a name="what-youll-learn"></a>O que você aprenderá:

- Como registrar rotas para um aplicativo ASP.NET Web Forms.
- Como adicionar rotas a uma página da Web.
- Como selecionar dados de um banco de dado para dar suporte a rotas.

## <a name="aspnet-routing-overview"></a>Visão geral do roteamento de ASP.NET

O roteamento de URL permite que você configure um aplicativo para aceitar URLs de solicitação que não são mapeadas para arquivos físicos. Uma URL de solicitação é simplesmente a URL que um usuário insere em seu navegador para localizar uma página no seu site. Você usa o roteamento para definir as URLs que são semanticamente significativas para os usuários e que podem ajudar com a SEO (otimização do mecanismo de pesquisa).

Por padrão, o modelo de Web Forms inclui [URLs amigáveis ASP.net](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/). Grande parte do trabalho de roteamento básico será implementada usando *URLs amigáveis*. No entanto, neste tutorial, você adicionará recursos de roteamento personalizados.

Antes de personalizar o roteamento de URL, o aplicativo de exemplo Wingtip Toys pode vincular a um produto usando a seguinte URL:

`https://localhost:44300/ProductDetails.aspx?productID=2`

Ao personalizar o roteamento de URL, o aplicativo de exemplo Wingtip Toys será vinculado ao mesmo produto usando uma URL mais fácil de ler:

`https://localhost:44300/Product/Convertible%20Car`

### <a name="routes"></a>Rotas

Uma rota é um padrão de URL que é mapeado para um manipulador. O manipulador pode ser um arquivo físico, como um arquivo. aspx em um aplicativo Web Forms. Um manipulador também pode ser uma classe que processa a solicitação. Para definir uma rota, você cria uma instância da classe Route especificando o padrão de URL, o manipulador e, opcionalmente, um nome para a rota.

Você adiciona a rota ao aplicativo adicionando o objeto `Route` à propriedade estática `Routes` da classe `RouteTable`. A Propriedade Routes é um objeto `RouteCollection` que armazena todas as rotas para o aplicativo.

### <a name="url-patterns"></a>Padrões de URL

Um padrão de URL pode conter valores literais e espaços reservados de variáveis (chamados de parâmetros de URL). Os literais e espaços reservados estão localizados em segmentos da URL que são delimitadas pelo caractere de barra (`/`).

Quando uma solicitação para seu aplicativo Web é feita, a URL é analisada em segmentos e espaços reservados, e os valores de variáveis são fornecidos para o manipulador de solicitação. Esse processo é semelhante ao modo como os dados em uma cadeia de caracteres de consulta são analisados e passados para o manipulador de solicitação. Em ambos os casos, as informações de variáveis são incluídas na URL e passadas para o manipulador na forma de pares chave-valor. Para cadeias de caracteres de consulta, as chaves e os valores estão na URL. Para rotas, as chaves são os nomes de espaço reservado definidos no padrão de URL e somente os valores estão na URL.

Em um padrão de URL, você define espaços reservados ao colocá-los entre chaves (`{` e `}`). Você pode definir mais de um espaço reservado em um segmento, mas os espaços reservados devem ser separados por um valor literal. Por exemplo, `{language}-{country}/{action}` é um padrão de rota válido. No entanto, `{language}{country}/{action}` não é um padrão válido, porque não há nenhum valor literal ou delimitador entre os espaços reservados. Portanto, o roteamento não pode determinar onde separar o valor do espaço reservado do idioma do valor para o espaço reservado do país.

### <a name="mapping-and-registering-routes"></a>Mapeando e registrando rotas

Antes de incluir rotas para páginas do aplicativo de exemplo Wingtip Toys, você deve registrar as rotas quando o aplicativo for iniciado. Para registrar as rotas, você modificará o manipulador de eventos `Application_Start`.

1. Em **Gerenciador de soluções**do Visual Studio, localize e abra o arquivo *global.asax.cs* .
2. Adicione o código realçado em amarelo ao arquivo *global.asax.cs* da seguinte maneira:   

    [!code-csharp[Main](url-routing/samples/sample1.cs?highlight=30-31,34-46)]

Quando o aplicativo de exemplo Wingtip Toys é iniciado, ele chama o manipulador de eventos `Application_Start`. No final desse manipulador de eventos, o método `RegisterCustomRoutes` é chamado. O método `RegisterCustomRoutes` adiciona cada rota chamando o método `MapPageRoute` do objeto `RouteCollection`. As rotas são definidas usando um nome de rota, uma URL de rota e uma URL física.

O primeiro parâmetro ("`ProductsByCategoryRoute`") é o nome da rota. Ele é usado para chamar a rota quando necessário. O segundo parâmetro ("`Category/{categoryName}`") define a URL de substituição amigável que pode ser dinâmica com base no código. Você usa essa rota quando está preenchendo um controle de dados com links gerados com base nos dados. Uma rota é mostrada da seguinte maneira:

[!code-csharp[Main](url-routing/samples/sample2.cs)]

O segundo parâmetro da rota inclui um valor dinâmico especificado por chaves (`{ }`). Nesse caso, o `categoryName` é uma variável que será usada para determinar o caminho de roteamento adequado.

> [!NOTE] 
> 
> **Opcional**
> 
> Você pode achar mais fácil gerenciar seu código movendo o método `RegisterCustomRoutes` para uma classe separada. Na pasta *lógica* , crie uma classe de `RouteActions` separada. Mova o método `RegisterCustomRoutes` acima do arquivo *global.asax.cs* para a nova classe `RoutesActions`. Use a classe `RoleActions` e o método `createAdmin` como um exemplo de como chamar o método `RegisterCustomRoutes` do arquivo *global.asax.cs* .

Você também pode ter notado a chamada do método `RegisterRoutes` usando o objeto `RouteConfig` no início do manipulador de eventos `Application_Start`. Essa chamada é feita para implementar o roteamento padrão. Ele foi incluído como código padrão quando você criou o aplicativo usando o modelo de Web Forms do Visual Studio.

## <a name="retrieving-and-using-route-data"></a>Recuperando e usando dados de rota

Conforme mencionado acima, as rotas podem ser definidas. O código que você adicionou ao manipulador de eventos `Application_Start` no arquivo *global.asax.cs* carrega as rotas definíveis.

### <a name="setting-routes"></a>Definindo rotas

As rotas exigem que você adicione código adicional. Neste tutorial, você usará a associação de modelo para recuperar um objeto `RouteValueDictionary` que é usado ao gerar as rotas usando dados de um controle de dados. O objeto `RouteValueDictionary` conterá uma lista de nomes de produtos que pertencem a uma categoria específica de produtos. Um link é criado para cada produto com base nos dados e na rota.

#### <a name="enable-routes-for-categories-and-products"></a>Habilitar rotas para categorias e produtos

Em seguida, você atualizará o aplicativo para usar o `ProductsByCategoryRoute` para determinar a rota correta a ser incluída para cada link de categoria de produto. Você também atualizará a página *ProductList. aspx* para incluir um link roteado para cada produto. Os links serão exibidos como estavam antes da alteração, no entanto, os links agora usarão o roteamento de URL.

1. Em **Gerenciador de soluções**, abra a página *site. Master* se ela ainda não estiver aberta.
2. Atualize o controle **ListView** chamado "`categoryList`" com as alterações realçadas em amarelo, para que a marcação apareça da seguinte maneira:   

    [!code-aspx[Main](url-routing/samples/sample3.aspx?highlight=7-9)]
3. Em **Gerenciador de soluções**, abra a página *ProductList. aspx* .
4. Atualize o elemento `ItemTemplate` da página *ProductList. aspx* com as atualizações realçadas em amarelo, para que a marcação apareça da seguinte maneira:   

    [!code-aspx[Main](url-routing/samples/sample4.aspx?highlight=6-9,14-16)]
5. Abra o code-behind de *ProductList.aspx.cs* e adicione o seguinte namespace como realçado em amarelo:  

    [!code-csharp[Main](url-routing/samples/sample5.cs?highlight=9)]
6. Substitua o método `GetProducts` do code-behind (*ProductList.aspx.cs*) pelo código a seguir:   

    [!code-csharp[Main](url-routing/samples/sample6.cs)]

#### <a name="add-code-for-product-details"></a>Adicionar código para detalhes do produto

Agora, atualize o code-behind (*ProductDetails.aspx.cs*) para a página *ProductDetails. aspx* para usar os dados de rota. Observe que o novo método `GetProduct` também aceita um valor de cadeia de caracteres de consulta para o caso em que o usuário tem um link marcado que usa a URL não amigável e não roteada mais antiga.

1. Substitua o método `GetProduct` do code-behind (*ProductDetails.aspx.cs*) pelo código a seguir:   

    [!code-csharp[Main](url-routing/samples/sample7.cs)]

## <a name="running-the-application"></a>Executando o aplicativo

Você pode executar o aplicativo agora para ver as rotas atualizadas.

1. Pressione **F5** para executar o aplicativo de exemplo Wingtip Toys.  
 O navegador é aberto e mostra a página *Default. aspx* .
2. Clique no link **produtos** na parte superior da página.  
 Todos os produtos são exibidos na página *ProductList. aspx* . A URL a seguir (usando o número da porta) é exibida para o navegador:  
    `https://localhost:44300/ProductList`
3. Em seguida, clique no link de **carros** categoria próximo à parte superior da página.  
 Somente carros são exibidos na página *ProductList. aspx* . A URL a seguir (usando o número da porta) é exibida para o navegador:  
    `https://localhost:44300/Category/Cars`
4. Clique no link que contém o nome do primeiro carro listado na página ("**carro conversível**") para exibir os detalhes do produto.  
 A URL a seguir (usando o número da porta) é exibida para o navegador:  
    `https://localhost:44300/Product/Convertible%20Car`
5. Em seguida, insira a seguinte URL não roteada (usando o número da porta) no navegador:  
    `https://localhost:44300/ProductDetails.aspx?productID=2`  
 O código ainda reconhece uma URL que inclui uma cadeia de caracteres de consulta, para o caso em que um usuário tem um link marcado.

## <a name="summary"></a>Resumo

Neste tutorial, você adicionou rotas para categorias e produtos. Você aprendeu como as rotas podem ser integradas a controles de dados que usam a associação de modelo. No próximo tutorial, você implementará o tratamento de erros global.

## <a name="additional-resources"></a>Recursos adicionais

[URLs amigáveis do ASP.NET](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/)  
[Implantar um aplicativo ASP.NET Web Forms seguro com associação, OAuth e banco de dados SQL no serviço Azure App](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Avaliação gratuita Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [Anterior](membership-and-administration.md)
> [Próximo](aspnet-error-handling.md)
