---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
title: Carrinho de compras | Microsoft Docs
author: Erikre
description: Esta série de tutoriais ensinará as noções básicas da criação de um aplicativo ASP.NET Web Forms usando o ASP.NET 4,5 e o Microsoft Visual Studio Express 2013 para nós...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 6898c601-6c31-432f-8388-e6843f8a17cb
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
msc.type: authoredcontent
ms.openlocfilehash: d3b619ebd9448d30857ffbaf17fd245b1d54a662
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519291"
---
# <a name="shopping-cart"></a>Carrinho de compras

por [Erik Reitan](https://github.com/Erikre)

[Baixar o projeto de exemplo WingtipC#Toys ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [baixar o livro eletrônico (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta série de tutoriais ensinará as noções básicas da criação de um aplicativo ASP.NET Web Forms usando o ASP.NET 4,5 e o Microsoft Visual Studio Express 2013 para a Web. Um [projeto Visual Studio 2013 com C# código-fonte](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) está disponível para acompanhar esta série de tutoriais.

Este tutorial descreve a lógica de negócios necessária para adicionar um carrinho de compras ao aplicativo ASP.NET Web Forms de exemplo Wingtip Toys. Este tutorial se baseia no tutorial anterior "Exibir itens e detalhes de dados" e faz parte da série de tutoriais da loja Wingtip Toy. Quando você concluir este tutorial, os usuários do aplicativo de exemplo poderão adicionar, remover e modificar os produtos em seu carrinho de compras.

## <a name="what-youll-learn"></a>O que você aprenderá:

1. Como criar um carrinho de compras para o aplicativo Web.
2. Como permitir que os usuários adicionem itens ao carrinho de compras.
3. Como adicionar um controle [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview(v=vs.110).aspx#introduction) para exibir detalhes do carrinho de compras.
4. Como calcular e exibir o total do pedido.
5. Como remover e atualizar itens no carrinho de compras.
6. Como incluir um contador de carrinhos de compras.

## <a name="code-features-in-this-tutorial"></a>Recursos de código neste tutorial:

1. Entity Framework Code First
2. Anotações de dados
3. Controles de dados com rigidez de tipos
4. Model binding

## <a name="creating-a-shopping-cart"></a>Criando um carrinho de compras

Anteriormente nesta série de tutoriais, você adicionou páginas e código para exibir dados de produto de um banco de dado. Neste tutorial, você criará um carrinho de compras para gerenciar os produtos que os usuários estão interessados em comprar. Os usuários poderão procurar e adicionar itens ao carrinho de compras, mesmo que não estejam registrados ou conectados. Para gerenciar o acesso ao carrinho de compras, você atribuirá aos usuários um `ID` exclusivo usando um GUID (identificador global exclusivo) quando o usuário acessar o carrinho de compras pela primeira vez. Você armazenará esse `ID` usando o estado de sessão ASP.NET.

> [!NOTE] 
> 
> O estado de sessão do ASP.NET é um local conveniente para armazenar informações específicas do usuário que expirarão depois que o usuário sair do site. Embora o uso indevido do estado da sessão possa causar implicações de desempenho em sites maiores, o uso leve do estado de sessão funciona bem para fins de demonstração. O projeto de exemplo Wingtip Toys mostra como usar o estado de sessão sem um provedor externo, em que o estado de sessão é armazenado em processo no servidor Web que hospeda o site. Para sites maiores que fornecem várias instâncias de um aplicativo ou para sites que executam várias instâncias de um aplicativo em servidores diferentes, considere o uso **do serviço de cache do Windows Azure**. Esse serviço de cache fornece um serviço de cache distribuído que é externo ao site e resolve o problema de usar o estado de sessão em processo. Para obter mais informações, consulte [como usar o estado de sessão ASP.NET com os sites do Windows Azure](https://docs.microsoft.com/azure/redis-cache/cache-aspnet-session-state-provider).

### <a name="add-cartitem-as-a-model-class"></a>Adicionar CartItem como uma classe de modelo

Anteriormente nesta série de tutoriais, você definiu o esquema para os dados de categoria e produto criando as classes `Category` e `Product` na pasta *modelos* . Agora, adicione uma nova classe para definir o esquema para o carrinho de compras. Posteriormente neste tutorial, você adicionará uma classe para manipular o acesso a dados à tabela `CartItem`. Essa classe fornecerá a lógica de negócios para adicionar, remover e atualizar itens no carrinho de compras.

1. Clique com o botão direito do mouse na pasta *modelos* e selecione **Adicionar** -&gt; **novo item**. 

    ![Carrinho de compras-novo item](shopping-cart/_static/image1.png)
2. A caixa de diálogo **Adicionar Novo Item** é exibida. Selecione **código**e, em seguida, selecione **classe**. 

    ![Carrinho de compras – caixa de diálogo Adicionar novo item](shopping-cart/_static/image2.png)
3. Nomeie essa nova classe *CartItem.cs*.
4. Clique em **Adicionar**.  
   O novo arquivo de classe é exibido no editor.
5. Substitua o código padrão pelo código a seguir:   

    [!code-csharp[Main](shopping-cart/samples/sample1.cs)]

A classe `CartItem` contém o esquema que definirá cada produto que um usuário adiciona ao carrinho de compras. Essa classe é semelhante às outras classes de esquema que você criou anteriormente nesta série de tutoriais. Por convenção, Entity Framework Code First espera que a chave primária para a tabela `CartItem` seja `CartItemId` ou `ID`. No entanto, o código substitui o comportamento padrão usando o atributo Data Annotation `[Key]`. O atributo `Key` da propriedade ItemId especifica que a propriedade `ItemID` é a chave primária.

A propriedade `CartId` especifica o `ID` do usuário que está associado ao item a ser comprado. Você adicionará código para criar esse usuário `ID` quando o usuário acessar o carrinho de compras. Essa `ID` também será armazenada como uma variável de sessão ASP.NET.

### <a name="update-the-product-context"></a>Atualizar o contexto do produto

Além de adicionar a classe de `CartItem`, você precisará atualizar a classe de contexto do banco de dados que gerencia as classes de entidade e que fornece acesso a um banco de dado. Para fazer isso, você adicionará a classe de modelo `CartItem` recém-criada à classe `ProductContext`.

1. Em **Gerenciador de soluções**, localize e abra o arquivo *ProductContext.cs* na pasta *modelos* .
2. Adicione o código realçado ao arquivo *ProductContext.cs* da seguinte maneira:  

    [!code-csharp[Main](shopping-cart/samples/sample2.cs?highlight=14)]

Conforme mencionado anteriormente nesta série de tutoriais, o código no arquivo *ProductContext.cs* adiciona o namespace `System.Data.Entity` para que você tenha acesso a todas as funcionalidades principais do Entity Framework. Essa funcionalidade inclui a capacidade de consultar, inserir, atualizar e excluir dados trabalhando com objetos fortemente tipados. A classe `ProductContext` Adiciona acesso à classe de modelo de `CartItem` recém adicionada.

### <a name="managing-the-shopping-cart-business-logic"></a>Gerenciando a lógica de negócios do carrinho de compras

Em seguida, você criará a classe `ShoppingCart` em uma nova pasta *lógica* . A classe `ShoppingCart` lida com o acesso a dados para a tabela `CartItem`. A classe também incluirá a lógica de negócios para adicionar, remover e atualizar itens no carrinho de compras.

A lógica do carrinho de compras que você adicionará conterá a funcionalidade para gerenciar as seguintes ações:

1. Adicionando itens ao carrinho de compras
2. Removendo itens do carrinho de compras
3. Obtendo a ID do carrinho de compras
4. Recuperando itens do carrinho de compras
5. Totalizando a quantidade de todos os itens do carrinho de compras
6. Atualizando os dados do carrinho de compras

Uma página de carrinho de compras (*ShoppingCart. aspx*) e a classe de carrinho de compras serão usadas em conjunto para acessar dados do carrinho de compras. A página carrinho de compras exibirá todos os itens que o usuário adiciona ao carrinho de compras. Além da página e da classe do carrinho de compras, você criará uma página (*addToCart. aspx*) para adicionar produtos ao carrinho de compras. Você também adicionará código à página *ProductList. aspx* e à página *ProductDetails. aspx* que fornecerá um link para a página *addToCart. aspx* , para que o usuário possa adicionar produtos ao carrinho de compras.

O diagrama a seguir mostra o processo básico que ocorre quando o usuário adiciona um produto ao carrinho de compras.

![Carrinho de compras-adicionando ao carrinho de compras](shopping-cart/_static/image3.png)

Quando o usuário clica no link **Adicionar ao carrinho** na página *ProductList. aspx* ou na página *ProductDetails. aspx* , o aplicativo navegará para a página *addToCart. aspx* e, em seguida, automaticamente para a página *ShoppingCart. aspx* . A página *addToCart. aspx* adicionará o produto Select ao carrinho de compras chamando um método na classe ShoppingCart. A página *ShoppingCart. aspx* exibirá os produtos que foram adicionados ao carrinho de compras.

#### <a name="creating-the-shopping-cart-class"></a>Criando a classe de carrinho de compras

A classe `ShoppingCart` será adicionada a uma pasta separada no aplicativo, de forma que haverá uma clara distinção entre o modelo (pasta modelos), as páginas (pasta raiz) e a lógica (pasta lógica).

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto **WingtipToys**e selecione **Adicionar**-&gt;**nova pasta**. Nomeie a nova *lógica*de pasta.
2. Clique com o botão direito do mouse na pasta *lógica* e selecione **Adicionar** -&gt; **novo item**.
3. Adicione um novo arquivo de classe chamado *ShoppingCartActions.cs*.
4. Substitua o código padrão pelo código a seguir:   

    [!code-csharp[Main](shopping-cart/samples/sample3.cs)]

O método `AddToCart` permite que produtos individuais sejam incluídos no carrinho de compras com base no `ID`do produto. O produto é adicionado ao carrinho ou, se o carrinho já contiver um item para esse produto, a quantidade será incrementada.

O método `GetCartId` retorna o `ID` do carrinho para o usuário. O `ID` do carrinho é usado para acompanhar os itens que um usuário tem em seu carrinho de compras. Se o usuário não tiver um `ID`de carrinho existente, um novo carrinho `ID` será criado para eles. Se o usuário estiver conectado como um usuário registrado, o `ID` do carrinho será definido como seu nome de usuário. No entanto, se o usuário não estiver conectado, o `ID` do carrinho será definido como um valor exclusivo (um GUID). Um GUID garante que apenas um carrinho seja criado para cada usuário, com base na sessão.

O método `GetCartItems` retorna uma lista de itens do carrinho de compras para o usuário. Posteriormente neste tutorial, você verá que a associação de modelo é usada para exibir os itens do carrinho no carrinho de compras usando o método `GetCartItems`.

### <a name="creating-the-add-to-cart-functionality"></a>Criando a funcionalidade de adição a carrinho

Como mencionado anteriormente, você criará uma página de processamento chamada *addToCart. aspx* que será usada para adicionar novos produtos ao carrinho de compras do usuário. Esta página chamará o método `AddToCart` na classe `ShoppingCart` que você acabou de criar. A página *addToCart. aspx* esperará que um produto `ID` seja passado para ele. Este `ID` de produto será usado ao chamar o método `AddToCart` na classe `ShoppingCart`.

> [!NOTE] 
> 
> Você modificará o code-behind (*addToCart.aspx.cs*) desta página, não a interface do usuário da página (*addToCart. aspx*).

#### <a name="to-create-the-add-to-cart-functionality"></a>Para criar a funcionalidade de adição a carrinho:

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto **WingtipToys**, clique em **Adicionar** -&gt; **novo item**.  
   A caixa de diálogo **Adicionar Novo Item** é exibida.
2. Adicione uma nova página padrão (Web Form) ao aplicativo chamado *addToCart. aspx*. 

    ![Carrinho de compras – Adicionar formulário da Web](shopping-cart/_static/image4.png)
3. No **Gerenciador de soluções**, clique com o botão direito do mouse na página *addToCart. aspx* e clique em **Exibir código**. O arquivo code-behind *addToCart.aspx.cs* é aberto no editor.
4. Substitua o código existente no código do *addToCart.aspx.cs* por trás do seguinte:   

    [!code-csharp[Main](shopping-cart/samples/sample4.cs)]

Quando a página *addToCart. aspx* é carregada, o produto `ID` é recuperado da cadeia de caracteres de consulta. Em seguida, uma instância da classe de carrinho de compras é criada e usada para chamar o método de `AddToCart` que você adicionou anteriormente neste tutorial. O método `AddToCart`, contido no arquivo *ShoppingCartActions.cs* , inclui a lógica para adicionar o produto selecionado ao carrinho de compras ou incrementar a quantidade de produtos do produto selecionado. Se o produto não tiver sido adicionado ao carrinho de compras, o produto será adicionado à tabela de `CartItem` do banco de dados. Se o produto já tiver sido adicionado ao carrinho de compras e o usuário adicionar um item adicional do mesmo produto, a quantidade de produtos será incrementada na tabela de `CartItem`. Por fim, a página redireciona de volta para a página *ShoppingCart. aspx* que você adicionará na próxima etapa, em que o usuário vê uma lista atualizada de itens no carrinho.

Como mencionado anteriormente, um `ID` de usuário é usado para identificar os produtos que estão associados a um usuário específico. Essa `ID` é adicionada a uma linha na tabela `CartItem` cada vez que o usuário adiciona um produto ao carrinho de compras.

### <a name="creating-the-shopping-cart-ui"></a>Criando a interface do usuário do carrinho de compras

A página *ShoppingCart. aspx* exibirá os produtos que o usuário adicionou ao carrinho de compras. Ele também fornecerá a capacidade de adicionar, remover e atualizar itens no carrinho de compras.

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse em **WingtipToys**, clique em **Adicionar** -&gt; **novo item**.  
   A caixa de diálogo **Adicionar Novo Item** é exibida.
2. Adicione uma nova página (formulário da Web) que inclui uma página mestra selecionando **formulário da Web usando a página mestra**. Nomeie a nova página *ShoppingCart. aspx*.
3. Selecione **site. Master** para anexar a página mestra à página *. aspx* recém-criada.
4. Na página *ShoppingCart. aspx* , substitua a marcação existente pela marcação a seguir:   

    [!code-aspx[Main](shopping-cart/samples/sample5.aspx)]

A página *ShoppingCart. aspx* inclui um controle **GridView** chamado `CartList`. Esse controle usa a associação de modelo para associar os dados do carrinho de compras do banco de dado ao controle **GridView** . Quando você define a propriedade `ItemType` do controle **GridView** , a expressão de vinculação de dados `Item` está disponível na marcação do controle e o controle se torna fortemente tipado. Conforme mencionado anteriormente nesta série de tutoriais, você pode selecionar detalhes do objeto de `Item` usando o IntelliSense. Para configurar um controle de dados para usar a associação de modelo para selecionar dados, defina a propriedade `SelectMethod` do controle. Na marcação acima, você define o `SelectMethod` para usar o método GetShoppingCartItems, que retorna uma lista de objetos `CartItem`. O controle de dados **GridView** chama o método no momento apropriado no ciclo de vida da página e associa os dados retornados automaticamente. O método `GetShoppingCartItems` ainda deve ser adicionado.

#### <a name="retrieving-the-shopping-cart-items"></a>Recuperando os itens do carrinho de compras

Em seguida, você adiciona código ao código-behind *ShoppingCart.aspx.cs* para recuperar e popular a interface do usuário do carrinho de compras.

1. No **Gerenciador de soluções**, clique com o botão direito do mouse na página *ShoppingCart. aspx* e clique em **Exibir código**. O arquivo code-behind *ShoppingCart.aspx.cs* é aberto no editor.
2. Substitua o código existente pelo seguinte:  

    [!code-csharp[Main](shopping-cart/samples/sample6.cs)]

Como mencionado acima, o controle de dados `GridView` chama o método `GetShoppingCartItems` no momento apropriado do ciclo de vida da página e associa os dados retornados automaticamente. O método `GetShoppingCartItems` cria uma instância do objeto `ShoppingCartActions`. Em seguida, o código usa essa instância para retornar os itens no carrinho chamando o método `GetCartItems`.

### <a name="adding-products-to-the-shopping-cart"></a>Adicionando produtos ao carrinho de compras

Quando a página *ProductList. aspx* ou *ProductDetails. aspx* for exibida, o usuário poderá adicionar o produto ao carrinho de compras usando um link. Quando eles clicam no link, o aplicativo navega para a página de processamento chamada *addToCart. aspx*. A página *addToCart. aspx* chamará o método `AddToCart` na classe `ShoppingCart` que você adicionou anteriormente neste tutorial.

Agora, você adicionará um link **Adicionar ao carrinho** à página *ProductList. aspx* e à página *ProductDetails. aspx* . Esse link incluirá o `ID` do produto que é recuperado do banco de dados.

1. Em **Gerenciador de soluções**, localize e abra a página chamada *ProductList. aspx*.
2. Adicione a marcação realçada em amarelo à página *ProductList. aspx* para que a página inteira apareça da seguinte maneira:  

    [!code-aspx[Main](shopping-cart/samples/sample7.aspx?highlight=50-54)]

### <a name="testing-the-shopping-cart"></a>Testando o carrinho de compras

Execute o aplicativo para ver como você adiciona produtos ao carrinho de compras.

1. Pressione **F5** para executar o aplicativo.  
 Depois que o projeto recriar o banco de dados, o navegador será aberto e mostrará a página *Default. aspx* .
2. Selecione **carros** no menu de navegação categoria.  
 A página *ProductList. aspx* é exibida mostrando apenas os produtos incluídos na categoria "carros". 

    ![Carrinho de compras – carros](shopping-cart/_static/image5.png)
3. Clique no link **Adicionar ao carrinho** próximo ao primeiro produto listado (o carro conversível).   
 A página *ShoppingCart. aspx* é exibida, mostrando a seleção em seu carrinho de compras. 

    ![Carrinho de compras-carrinho](shopping-cart/_static/image6.png)
4. Exiba produtos adicionais selecionando **planos** no menu de navegação categoria.
5. Clique no link **Adicionar ao carrinho** próximo ao primeiro produto listado.  
 A página *ShoppingCart. aspx* é exibida com o item adicional.
6. Feche o navegador.

### <a name="calculating-and-displaying-the-order-total"></a>Calculando e exibindo o total do pedido

Além de adicionar produtos ao carrinho de compras, você adicionará um método `GetTotal` à classe `ShoppingCart` e exibirá o valor total da ordem na página carrinho de compras.

1. Em **Gerenciador de soluções**, abra o arquivo *ShoppingCartActions.cs* na pasta *lógica* .
2. Adicione o seguinte método `GetTotal` realçado em amarelo à classe `ShoppingCart`, para que a classe seja exibida da seguinte maneira:   

    [!code-csharp[Main](shopping-cart/samples/sample8.cs?highlight=85-97)]

Primeiro, o método `GetTotal` Obtém a ID do carrinho de compras para o usuário. Em seguida, o método obtém o total do carrinho multiplicando o preço do produto pela quantidade do produto para cada produto listado no carrinho.

> [!NOTE] 
> 
> O código acima usa o tipo anulável "`int?`". Tipos anuláveis podem representar todos os valores de um tipo subjacente e também como um valor nulo. Para obter mais informações, consulte [usando tipos anuláveis](https://msdn.microsoft.com/library/2cf62fcy(v=vs.110).aspx).

### <a name="modify-the-shopping-cart-display"></a>Modificar a exibição do carrinho de compras

Em seguida, você modificará o código da página *ShoppingCart. aspx* para chamar o método `GetTotal` e exibirá esse total na página *ShoppingCart. aspx* quando a página for carregada.

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse na página *ShoppingCart. aspx* e selecione **Exibir código**.
2. No arquivo *ShoppingCart.aspx.cs* , atualize o manipulador de `Page_Load` adicionando o seguinte código realçado em amarelo:   

    [!code-csharp[Main](shopping-cart/samples/sample9.cs?highlight=16-31)]

Quando a página *ShoppingCart. aspx* é carregada, ela carrega o objeto de carrinho de compras e, em seguida, recupera o total do carrinho de compras chamando o método `GetTotal` da classe `ShoppingCart`. Se o carrinho de compras estiver vazio, será exibida uma mensagem para esse efeito.

### <a name="testing-the-shopping-cart-total"></a>Testando o total do carrinho de compras

Execute o aplicativo agora para ver como você não pode adicionar um produto ao carrinho de compras, mas você pode ver o total do carrinho de compras.

1. Pressione **F5** para executar o aplicativo.  
 O navegador será aberto e mostrará a página *Default.aspx* .
2. Selecione **carros** no menu de navegação categoria.
3. Clique no link **Adicionar ao carrinho** ao lado do primeiro produto.   
 A página *ShoppingCart. aspx* é exibida com o total do pedido. 

    ![Carrinho de compras-total do carrinho](shopping-cart/_static/image7.png)
4. Adicione alguns outros produtos (por exemplo, um plano) ao carrinho.
5. A página *ShoppingCart. aspx* é exibida com um total atualizado para todos os produtos que você adicionou. 

    ![Carrinho de compras – vários produtos](shopping-cart/_static/image8.png)
6. Pare o aplicativo em execução fechando a janela do navegador.

### <a name="adding-update-and-checkout-buttons-to-the-shopping-cart"></a>Adicionando botões de atualização e de check-out ao carrinho de compras

Para permitir que os usuários modifiquem o carrinho de compras, você adicionará um botão de **atualização** e um botão de **check-out** à página do carrinho de compras. O botão de **check-out** não será usado até mais tarde nesta série de tutoriais.

1. No **Gerenciador de soluções**, abra a página *ShoppingCart. aspx* na raiz do projeto de aplicativo Web.
2. Para adicionar o botão de **atualização** e o botão de **check-out** à página *ShoppingCart. aspx* , adicione a marcação realçada em amarelo à marcação existente, conforme mostrado no código a seguir:   

    [!code-aspx[Main](shopping-cart/samples/sample10.aspx?highlight=36-45)]

Quando o usuário clica no botão **Atualizar** , o manipulador de eventos `UpdateBtn_Click` será chamado. Esse manipulador de eventos chamará o código que você adicionará na próxima etapa.

Em seguida, você pode atualizar o código contido no arquivo *ShoppingCart.aspx.cs* para executar um loop pelos itens do carrinho e chamar os métodos `RemoveItem` e `UpdateItem`.

1. No **Gerenciador de soluções**, abra o arquivo *ShoppingCart.aspx.cs* na raiz do projeto de aplicativo Web.
2. Adicione as seguintes seções de código realçadas em amarelo ao arquivo *ShoppingCart.aspx.cs* :   

    [!code-csharp[Main](shopping-cart/samples/sample11.cs?highlight=9-11,33,44-89)]

Quando o usuário clica no botão **Atualizar** na página *ShoppingCart. aspx* , o método UpdateCartItems é chamado. O método UpdateCartItems Obtém os valores atualizados para cada item no carrinho de compras. Em seguida, o método UpdateCartItems chama o método `UpdateShoppingCartDatabase` (adicionado e explicado na próxima etapa) para adicionar ou remover itens do carrinho de compras. Depois que o banco de dados tiver sido atualizado para refletir as atualizações para o carrinho de compras, o controle **GridView** será atualizado na página do carrinho de compras chamando o método `DataBind` para **GridView**. Além disso, o valor total do pedido na página do carrinho de compras é atualizado para refletir a lista atualizada de itens.

### <a name="updating-and-removing-shopping-cart-items"></a>Atualizando e removendo itens do carrinho de compras

Na página *ShoppingCart. aspx* , você pode ver que os controles foram adicionados para atualizar a quantidade de um item e remover um item. Agora, adicione o código que fará com que esses controles funcionem.

1. Em **Gerenciador de soluções**, abra o arquivo *ShoppingCartActions.cs* na pasta *lógica* .
2. Adicione o seguinte código realçado em amarelo ao arquivo de classe *ShoppingCartActions.cs* :   

    [!code-csharp[Main](shopping-cart/samples/sample12.cs?highlight=99-213)]

O método `UpdateShoppingCartDatabase`, chamado a partir do método `UpdateCartItems` na página *ShoppingCart.aspx.cs* , contém a lógica para atualizar ou remover itens do carrinho de compras. O método `UpdateShoppingCartDatabase` itera em todas as linhas dentro da lista de carrinhos de compras. Se um item de carrinho de compras tiver sido marcado para ser removido ou a quantidade for menor que um, o método `RemoveItem` será chamado. Caso contrário, o item do carrinho de compras será verificado quanto a atualizações quando o método `UpdateItem` for chamado. Depois que o item do carrinho de compras tiver sido removido ou atualizado, as alterações no banco de dados serão salvas.

A estrutura de `ShoppingCartUpdates` é usada para manter todos os itens do carrinho de compras. O método `UpdateShoppingCartDatabase` usa a estrutura `ShoppingCartUpdates` para determinar se algum dos itens precisa ser atualizado ou removido.

No próximo tutorial, você usará o método `EmptyCart` para limpar o carrinho de compras depois de comprar produtos. Mas, por enquanto, você usará o método `GetCount` que acabou de adicionar ao arquivo *ShoppingCartActions.cs* para determinar quantos itens estão no carrinho de compras.

### <a name="adding-a-shopping-cart-counter"></a>Adicionando um contador de carrinhos de compras

Para permitir que o usuário exiba o número total de itens no carrinho de compras, você adicionará um contador à página *site. Master* . Esse contador também funcionará como um link para o carrinho de compras.

1. Em **Gerenciador de soluções**, abra a página *site. Master* .
2. Modifique a marcação adicionando o link do contador de carrinho de compras, conforme mostrado em amarelo, para a seção de navegação, para que ele apareça da seguinte maneira:  

    [!code-html[Main](shopping-cart/samples/sample13.html?highlight=6)]
3. Em seguida, atualize o código para trás do arquivo *site.master.cs* adicionando o código realçado em amarelo da seguinte maneira:  

    [!code-csharp[Main](shopping-cart/samples/sample14.cs?highlight=11,77-84)]

Antes que a página seja processada como HTML, o evento `Page_PreRender` é gerado. No manipulador de `Page_PreRender`, a contagem total do carrinho de compras é determinada chamando o método `GetCount`. O valor retornado é adicionado ao `cartCount` span incluído na marcação da página *site. Master* . As marcas de `<span>` permitem que os elementos internos sejam processados corretamente. Quando qualquer página do site for exibida, o total do carrinho de compras será exibido. O usuário também pode clicar no total do carrinho de compras para exibir o carrinho de compras.

## <a name="testing-the-completed-shopping-cart"></a>Testando o carrinho de compras concluído

Você pode executar o aplicativo agora para ver como você pode adicionar, excluir e atualizar itens no carrinho de compras. O total do carrinho de compras refletirá o custo total de todos os itens no carrinho de compras.

1. Pressione **F5** para executar o aplicativo.  
 O navegador é aberto e mostra a página *Default. aspx* .
2. Selecione **carros** no menu de navegação categoria.
3. Clique no link **Adicionar ao carrinho** ao lado do primeiro produto.   
 A página *ShoppingCart. aspx* é exibida com o total do pedido.
4. Selecione **planos** no menu de navegação categoria.
5. Clique no link **Adicionar ao carrinho** ao lado do primeiro produto.
6. Defina a quantidade do primeiro item no carrinho de compras como 3 e marque a caixa de seleção **Remover item** do segundo item.<a id="a"></a>
7. Clique no botão **Atualizar** para atualizar a página do carrinho de compras e exibir o novo total do pedido. 

    ![Carrinho de compras-atualização do carrinho](shopping-cart/_static/image9.png)

## <a name="summary"></a>Resumo

Neste tutorial, você criou um carrinho de compras para o aplicativo de exemplo Web Forms Wingtip Toys. Durante este tutorial, você usou Entity Framework Code First, anotações de dados, controles de dados com rigidez de tipos e Associação de modelo.

O carrinho de compras dá suporte à adição, exclusão e atualização de itens que o usuário selecionou para compra. Além de implementar a funcionalidade de carrinho de compras, você aprendeu como exibir itens de carrinho de compras em um controle **GridView** e calcular o total do pedido.

Para entender como a funcionalidade descrita funciona em um aplicativo de negócios real, você pode exibir o exemplo de carrinho de compras de software livre baseado em [Nopcommerce](https://github.com/nopSolutions/nopCommerce) -ASP.net. Originalmente, ele foi criado em Web Forms e ao longo dos anos movidos para o MVC e agora para ASP.NET Core.

## <a name="addition-information"></a>Informações de adição

[Visão geral do estado de sessão do ASP.NET](https://msdn.microsoft.com/library/ms178581.aspx)

> [!div class="step-by-step"]
> [Anterior](display_data_items_and_details.md)
> [Próximo](checkout-and-payment-with-paypal.md)
