---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Exibir itens de dados e detalhes | Microsoft Docs
author: Erikre
description: Esta série de tutoriais mostrará as noções básicas da criação de um aplicativo ASP.NET Web Forms com ASP.NET 4,7 e Microsoft Visual Studio 2017
ms.author: riande
ms.date: 1/04/2019
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 130c9ffd29df612dac5bb954830a2eb9b738aaf0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78641109"
---
# <a name="display-data-items-and-details"></a>Exibir itens de dados e detalhes

por [Erik Reitan](https://github.com/Erikre)

> Esta série de tutoriais ensina as noções básicas da criação de um aplicativo ASP.NET Web Forms com ASP.NET 4,7 e Microsoft Visual Studio 2017.

Neste tutorial, você aprenderá a exibir itens de dados e detalhes de item de dados com ASP.NET Web Forms e Entity Framework Code First. Este tutorial se baseia no tutorial de "interface do usuário e navegação" anterior como parte da série de tutoriais da loja Wingtip Toy. Depois de concluir este tutorial, você verá produtos na página *ProductList. aspx* e nos detalhes do produto na página *ProductDetails. aspx* .

## <a name="youll-learn-how-to"></a>Você aprenderá a:

- Adicionar um controle de dados para exibir produtos do banco de dado
- Conectar um controle de dados aos dados selecionados
- Adicionar um controle de dados para exibir os detalhes do produto
- Recuperar um valor da cadeia de caracteres de consulta e usar esse valor para limitar os dados que são recuperados do Database

### <a name="features-introduced-in-this-tutorial"></a>Recursos apresentados neste tutorial:

- Model binding
- Provedores de valor

## <a name="add-a-data-control"></a>Adicionar um controle de dados

Você pode usar algumas opções diferentes para associar dados a um controle de servidor. As mais comuns incluem:

* Adicionando um controle da fonte de dados
* Adicionando código manualmente
* Usando a associação de modelo

### <a name="use-a-data-source-control-to-bind-data"></a>Usar um controle de fonte de dados para associar dados

Adicionar um controle de fonte de dados permite vincular o controle da fonte de dados ao controle que exibe os dados. Com essa abordagem, você pode declarativamente, em vez de programaticamente, conectar controles do lado do servidor a fontes de dados.

### <a name="code-by-hand-to-bind-data"></a>Código manualmente para associar dados

Codificar manualmente envolve:

1. Lendo um valor
2. Verificando se é nulo
3. Convertendo-o para um tipo apropriado
4. Verificando o êxito da conversão
5. Usando o valor na consulta 

Essa abordagem permite que você tenha controle total sobre a lógica de acesso a dados.

### <a name="use-model-binding-to-bind-data"></a>Usar Associação de modelo para associar dados

A associação de modelo permite que você associe os resultados com muito menos código e oferece a capacidade de reutilizar a funcionalidade em todo o aplicativo. Ele simplifica o trabalho com a lógica de acesso a dados focada em código enquanto ainda fornece uma estrutura de ligação de dados rica.

## <a name="display-products"></a>Exibir produtos

Neste tutorial, você usará a associação de modelo para associar dados. Para configurar um controle de dados para usar a associação de modelo para selecionar dados, defina a propriedade `SelectMethod` do controle como um nome de método no código da página. O controle de dados chama o método no momento apropriado no ciclo de vida da página e associa os dados retornados automaticamente. Não é necessário chamar explicitamente o método `DataBind`.

1. Em **Gerenciador de soluções**, abra *ProductList. aspx*.
2. Substituir a marcação existente por esta marcação:   

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample1.aspx)]

Esse código usa um controle **ListView** chamado `productList` para exibir produtos.

[!code-aspx-csharp[Main](display_data_items_and_details/samples/sample2.aspx)]

Com modelos e estilos, você define como o controle **ListView** exibe dados. Ele é útil para dados em qualquer estrutura repetitiva. Embora este exemplo de **ListView** simplesmente exiba dados de banco de dado, você também pode, sem código, permitir que os usuários editem, insiram e excluam dados, e classifiquem e coloquem dados na página.

Ao definir a propriedade `ItemType` no controle **ListView** , a expressão de vinculação de dados `Item` está disponível e o controle se torna fortemente tipado. Conforme mencionado no tutorial anterior, você pode selecionar detalhes de objeto de item com o IntelliSense, como especificar o `ProductName`:

![Exibir itens de dados e detalhes-IntelliSense](display_data_items_and_details/_static/image1.png)

Você também está usando a associação de modelo para especificar um valor de `SelectMethod`. Esse valor (`GetProducts`) corresponde ao método que você adicionará ao code-behind para exibir produtos na próxima etapa.

### <a name="add-code-to-display-products"></a>Adicionar código para exibir produtos

Nesta etapa, você adicionará o código para preencher o controle **ListView** com os dados do produto do banco de dado. O código dá suporte à exibição de todos os produtos e produtos de categoria individuais.

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse em *ProductList. aspx* e selecione **Exibir código**.
2. Substitua o código existente no arquivo *ProductList.aspx.cs* por:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

Esse código mostra o método `GetProducts` que a propriedade `ItemType` do controle **ListView** faz referência na página *ProductList. aspx* . Para limitar os resultados a uma categoria de banco de dados específica, o código define o valor `categoryId` do valor da cadeia de caracteres de consulta passado para a página *ProductList. aspx* quando a página *ProductList. aspx* é navegada. A classe `QueryStringAttribute` no namespace `System.Web.ModelBinding` é usada para recuperar o valor da variável de cadeia de caracteres de consulta `id`. Isso instrui a associação de modelo para tentar associar um valor da cadeia de caracteres de consulta ao parâmetro `categoryId` em tempo de execução.

Quando uma categoria válida é passada como uma cadeia de caracteres de consulta para a página, os resultados da consulta são limitados a esses produtos no banco de dados que correspondem ao valor de `categoryId`. Por exemplo, se a URL da página *ProductList. aspx* for esta:

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

A página exibe somente os produtos em que o `categoryId` é igual a `1`.

Todos os produtos serão exibidos se nenhuma cadeia de caracteres de consulta for incluída quando a página *ProductList. aspx* for chamada.

As fontes de valores para esses métodos são chamadas de *provedores de valor* (como *QueryString*) e os atributos de parâmetro que indicam qual provedor de valor usar são chamados de atributos de *provedor de valor* (como `id`). O ASP.NET inclui provedores de valor e atributos correspondentes para todas as fontes típicas de entrada do usuário em um aplicativo Web Forms como a cadeia de caracteres de consulta, cookies, valores de formulário, controles, estado de exibição, estado de sessão e propriedades de perfil. Você também pode escrever provedores de valor personalizado.

### <a name="run-the-application"></a>Executar o aplicativo

Execute o aplicativo agora para exibir todos os produtos ou os produtos de uma categoria.

1. Pressione **F5** enquanto estiver no Visual Studio para executar o aplicativo.  
   O navegador é aberto e mostra a página *Default. aspx* .

2. Selecione **carros** no menu de navegação categoria do produto.  
   A página *ProductList. aspx* exibe mostrando apenas os produtos da categoria **carros** . Posteriormente neste tutorial, você exibirá os detalhes do produto.  

    ![Exibir itens de dados e detalhes-carros](display_data_items_and_details/_static/image2.png)

3. Selecione **produtos** no menu de navegação na parte superior.  
   Novamente, a página *ProductList. aspx* é exibida, no entanto, desta vez ele mostra a lista completa de produtos.   

    ![Exibir itens de dados e detalhes-produtos](display_data_items_and_details/_static/image3.png)

4. Feche o navegador e retorne ao Visual Studio.

### <a name="add-a-data-control-to-display-product-details"></a>Adicionar um controle de dados para exibir detalhes do produto

Em seguida, você modificará a marcação na página *ProductDetails. aspx* que você adicionou no tutorial anterior para exibir informações específicas do produto.

1. No **Gerenciador de soluções**, abra *ProductDetails. aspx*.

2. Substituir a marcação existente por esta marcação:

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample5.aspx)] 

    Esse código usa um controle **FormView** para exibir detalhes específicos do produto. Essa marcação usa métodos como os métodos usados para exibir dados na página *ProductList. aspx* . O controle **FormView** é usado para exibir um único registro por vez de uma fonte de dados. Quando você usa o controle **FormView** , cria modelos para exibir e editar valores associados a dados. Esses modelos contêm controles, expressões de associação e formatação que definem a aparência e a funcionalidade do formulário.

Conectar a marcação anterior ao banco de dados requer código adicional.

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse em *ProductDetails. aspx* e clique em **Exibir código**.  
   O arquivo *ProductDetails.aspx.cs* é exibido.

2. Substitua o código existente por este código:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

Esse código verifica um valor de cadeia de caracteres de consulta "`productID`". Se um valor de cadeia de caracteres de consulta válido for encontrado, o produto correspondente será exibido. Se a cadeia de caracteres de consulta não for encontrada ou seu valor não for válido, nenhum produto será exibido.

### <a name="run-the-application"></a>Executar o aplicativo

Agora você pode executar o aplicativo para ver um produto individual exibido com base na ID do produto.

1. Pressione **F5** enquanto estiver no Visual Studio para executar o aplicativo.  
   O navegador é aberto e mostra a página *Default. aspx* .

2. Selecione **Boats** no menu de navegação categoria.  
   A página *ProductList. aspx* é exibida.

3. Selecione o **barco de papel** na lista de produtos.
   A página *ProductDetails. aspx* é exibida.

    ![Exibir itens de dados e detalhes-produtos](display_data_items_and_details/_static/image4.png)
    
4. Feche o navegador.

## <a name="additional-resources"></a>Recursos adicionais

[Recuperando e exibindo dados com associação de modelo e formulários da Web](../../presenting-and-managing-data/model-binding/retrieving-data.md)

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você adicionou marcação e código para exibir produtos e detalhes do produto. Você aprendeu sobre controles de dados fortemente tipados, associação de modelo e provedores de valor. No próximo tutorial, você adicionará um carrinho de compras ao aplicativo de exemplo Wingtip Toys. 

> [!div class="step-by-step"]
> [Anterior](ui_and_navigation.md)
> [Próximo](shopping-cart.md)
