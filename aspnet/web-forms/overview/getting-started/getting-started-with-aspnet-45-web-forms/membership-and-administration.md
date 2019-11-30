---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
title: Associação e administração | Microsoft Docs
author: Erikre
description: Esta série de tutoriais ensinará as noções básicas da criação de um aplicativo ASP.NET Web Forms usando o ASP.NET 4,5 e o Microsoft Visual Studio Express 2013 para nós...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 732a2316-e49f-4f72-becd-0cd72f14457e
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
msc.type: authoredcontent
ms.openlocfilehash: ab00bc90bfc767d06e747be6dfb973245b5aae88
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74615466"
---
# <a name="membership-and-administration"></a>Associação e administração

por [Erik Reitan](https://github.com/Erikre)

[Baixar o projeto de exemplo WingtipC#Toys ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [baixar o livro eletrônico (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta série de tutoriais ensinará as noções básicas da criação de um aplicativo ASP.NET Web Forms usando o ASP.NET 4,5 e o Microsoft Visual Studio Express 2013 para a Web. Um [projeto Visual Studio 2013 com C# código-fonte](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) está disponível para acompanhar esta série de tutoriais.

Este tutorial mostra como atualizar o aplicativo de exemplo Wingtip Toys para adicionar uma função personalizada e usar ASP.NET Identity. Ele também mostra como implementar uma página de administração da qual o usuário com uma função personalizada pode adicionar e remover produtos do site.

[ASP.net Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md) é o sistema de associação usado para criar o aplicativo Web ASP.net e está disponível no ASP.NET 4,5. ASP.NET Identity é usado no modelo de projeto Visual Studio 2013 Web Forms, bem como os modelos para [ASP.NET MVC](../../../../mvc/index.md), [ASP.NET Web API](../../../../web-api/index.md)e [ASP.NET aplicativo de página única](../../../../single-page-application/index.md). Você também pode instalar especificamente o sistema de ASP.NET Identity usando o NuGet ao iniciar com um aplicativo Web vazio. No entanto, nesta série de tutoriais, você usa o **Web Forms**ProjectTemplate, que inclui o sistema ASP.net Identity. ASP.NET Identity facilita a integração de dados de perfil específicos do usuário com os dados do aplicativo. Além disso, ASP.NET Identity permite que você escolha o modelo de persistência para perfis de usuário em seu aplicativo. Você pode armazenar os dados em um banco de dados de SQL Server ou em outro repositório, incluindo armazenamentos de dados *NoSQL* , como tabelas de armazenamento do Windows Azure.

Este tutorial se baseia no tutorial anterior intitulado "check-out e pagamento com o PayPal" na série de tutoriais da Wingtip Toys.

## <a name="what-youll-learn"></a>O que você aprenderá:

- Como usar o código para adicionar uma função personalizada e um usuário ao aplicativo.
- Como restringir o acesso à pasta e à página de administração.
- Como fornecer navegação para o usuário que pertence à função personalizada.
- Como usar a associação de modelo para popular um controle [DropDownList](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist(v=vs.110).aspx) com categorias de produtos.
- Como carregar um arquivo para o aplicativo Web usando o controle [FileUpload](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload(v=vs.110).aspx) .
- Como usar controles de validação para implementar a validação de entrada.
- Como adicionar e remover produtos do aplicativo.

## <a name="these-features-are-included-in-the-tutorial"></a>Esses recursos estão incluídos no tutorial:

- ASP.NET Identity
- Configuração e autorização
- Model binding
- Validação não invasiva

ASP.NET Web Forms fornece recursos de associação. Usando o modelo padrão, você tem funcionalidade de associação interna que pode ser usada imediatamente quando o aplicativo é executado. Este tutorial mostra como usar ASP.NET Identity para adicionar uma função personalizada e atribuir um usuário a essa função. Você aprenderá a restringir o acesso à pasta de administração. Você adicionará uma página à pasta administração que permite que um usuário com uma função personalizada adicione e remova produtos e visualize um produto depois que ele tiver sido adicionado.

## <a name="adding-a-custom-role"></a>Adicionando uma função personalizada

Usando ASP.NET Identity, você pode adicionar uma função personalizada e atribuir um usuário a essa função usando código.

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse na pasta *lógica* e crie uma nova classe.
2. Nomeie a nova classe *RoleActions.cs*.
3. Modifique o código para que ele apareça da seguinte maneira:  

    [!code-csharp[Main](membership-and-administration/samples/sample1.cs?highlight=8)]
4. No **Gerenciador de soluções**, abra o arquivo *global.asax.cs* .
5. Modifique o arquivo *global.asax.cs* adicionando o código realçado em amarelo para que ele apareça da seguinte maneira:  

    [!code-csharp[Main](membership-and-administration/samples/sample2.cs?highlight=11,26-28)]
6. Observe que `AddUserAndRole` é sublinhada em vermelho. Clique duas vezes no código AddUserAndRole.  
   A letra "A" no início do método realçado será sublinhada.
7. Passe o mouse sobre a letra "A" e clique na interface do usuário que permite gerar um stub de método para o método `AddUserAndRole`. 

    ![Associação e administração – stub do método de geração](membership-and-administration/_static/image1.png)
8. Clique na opção intitulada:  
    `Generate method stub for "AddUserAndRole" in "WingtipToys.Logic.RoleActions"`
9. Abra o arquivo *RoleActions.cs* da pasta *lógica* .  
   O método `AddUserAndRole` foi adicionado ao arquivo de classe.
10. Modifique o arquivo *RoleActions.cs* removendo o `NotImplementedException` e adicionando o código realçado em amarelo, para que ele apareça da seguinte maneira:  

    [!code-csharp[Main](membership-and-administration/samples/sample3.cs?highlight=5-7,15-51)]

O código acima estabelece primeiro um contexto de banco de dados para o banco de dados de associação. O banco de dados Membership também é armazenado como um arquivo *. MDF* na pasta *Data\_app* . Você poderá exibir esse banco de dados quando o primeiro usuário tiver entrado neste aplicativo Web. 

> [!NOTE] 
> 
> Se você deseja armazenar os dados de associação junto com os dados do produto, você pode considerar o uso do mesmo **DbContext** usado para armazenar os dados do produto no código acima.

 A palavra-chave *Internal* é um modificador de acesso para tipos (como classes) e membros de tipo (como métodos ou Propriedades). Tipos internos ou membros são acessíveis somente dentro dos arquivos contidos no mesmo assembly *(arquivo. dll* ). Quando você cria seu aplicativo, é criado um arquivo de assembly *(. dll*) que contém o código que é executado quando você executa o aplicativo. 

Um objeto `RoleStore`, que fornece gerenciamento de função, é criado com base no contexto do banco de dados.

> [!NOTE] 
> 
> Observe que, quando o objeto de `RoleStore` é criado, ele usa um tipo de `IdentityRole` genérico. Isso significa que a `RoleStore` só pode conter objetos `IdentityRole`. Além disso, com o uso de genéricos, os recursos na memória são melhor tratados.

Em seguida, o objeto `RoleManager`, é criado com base no objeto `RoleStore` que você acabou de criar. o objeto `RoleManager` expõe a API relacionada à função que pode ser usada para salvar automaticamente as alterações no `RoleStore`. O `RoleManager` só pode conter objetos `IdentityRole` porque o código usa o tipo genérico `<IdentityRole>`.

Você chama o método `RoleExists` para determinar se a função "CanEdit" está presente no banco de dados de associação. Se não for, você criará a função.

A criação do objeto de `UserManager` parece ser mais complicada do que o controle de `RoleManager`, no entanto, é praticamente o mesmo. Ele é codificado apenas em uma linha em vez de vários. Aqui, o parâmetro que você está passando é instanciar como um novo objeto contido no parêntese.

Em seguida, você cria o usuário "canEditUser" criando um novo objeto `ApplicationUser`. Em seguida, se você criar o usuário com êxito, adicione o usuário à nova função.

> [!NOTE] 
> 
> O tratamento de erros será atualizado durante o tutorial "tratamento de erros ASP.NET" posteriormente nesta série de tutoriais.

Na próxima vez que o aplicativo for iniciado, o usuário chamado "canEditUser" será adicionado como a função denominada "CanEdit" do aplicativo. Posteriormente neste tutorial, você fará logon como o usuário "canEditUser" para exibir recursos adicionais que serão adicionados durante este tutorial. Para obter detalhes sobre a API sobre ASP.NET Identity, consulte o [namespace Microsoft. AspNet. Identity](https://msdn.microsoft.com/library/microsoft.aspnet.identity(v=vs.111).aspx). Para obter detalhes adicionais sobre como inicializar o sistema de ASP.NET Identity, consulte o [AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample/blob/master/AspnetIdentitySample/App_Start/IdentityConfig.cs).

### <a name="restricting-access-to-the-administration-page"></a>Restringindo o acesso à página de administração

O aplicativo de exemplo Wingtip Toys permite que usuários anônimos e usuários conectados exibam e comprem produtos. No entanto, o usuário conectado que tem a função personalizada "CanEdit" pode acessar uma página restrita para adicionar e remover produtos.

#### <a name="add-an-administration-folder-and-page"></a>Adicionar uma pasta e uma página de administração

Em seguida, você criará uma pasta chamada *admin* para o usuário "canEditUser" pertencente à função personalizada do aplicativo de exemplo Wingtip Toys.

1. Clique com o botão direito do mouse no nome do projeto (**Wingtip Toys**) em **Gerenciador de soluções** e selecione **Adicionar** -&gt; **nova pasta**.
2. Nomeie o novo *administrador*da pasta.
3. Clique com o botão direito do mouse na pasta *admin* e selecione **Adicionar** -&gt; **novo item**.   
   A caixa de diálogo **Adicionar Novo Item** é exibida.
4. Selecione o grupo modelos <strong>da Web</strong> do <strong>Visual C#</strong> -&gt; à esquerda. Na lista intermediária, selecione <strong>Web Form com página mestra</strong>, nomeie-o <em>AdminPage. aspx</em>e<strong>,</strong> em seguida, selecione <strong>Adicionar</strong>.
5. Selecione o arquivo *site. Master* como a página mestra e escolha **OK**.

#### <a name="add-a-webconfig-file"></a>Adicionar um arquivo Web. config

Ao adicionar um arquivo *Web. config* à pasta *admin* , você pode restringir o acesso à página contida na pasta.

1. Clique com o botão direito do mouse na pasta *admin* e selecione **Adicionar** -&gt; **novo item**.  
   A caixa de diálogo **Adicionar Novo Item** é exibida.
2. Na lista de modelos do C# Visual Web, selecione <strong>arquivo de configuração da Web</strong>na lista intermediária, aceite o nome padrão de <em>Web. config</em><strong>e, em seguida, selecione</strong> <strong>Adicionar</strong>.
3. Substitua o conteúdo XML existente no arquivo *Web. config* pelo seguinte:  

    [!code-xml[Main](membership-and-administration/samples/sample4.xml)]

Salve o arquivo *Web. config* . O arquivo *Web. config* especifica que apenas o usuário pertencente à função "CanEdit" do aplicativo pode acessar a página contida na pasta *admin* .

### <a name="including-custom-role-navigation"></a>Incluindo navegação de função personalizada

Para habilitar o usuário da função "CanEdit" personalizada para navegar até a seção Administração do aplicativo, você deve adicionar um link à página *site. Master* . Somente os usuários que pertencem à função "CanEdit" poderão ver o link de **administrador** e acessar a seção de administração.

1. Em Gerenciador de Soluções, localize e abra a página *site. Master* .
2. Para criar um link para o usuário da função "CanEdit", adicione a marcação realçada em amarelo ao elemento de `<ul>` de lista não ordenado a seguir para que a lista seja exibida da seguinte maneira:  

    [!code-html[Main](membership-and-administration/samples/sample5.html?highlight=2-3)]
3. Abra o arquivo *site.master.cs* . Torne o link do **administrador** visível somente para o usuário "canEditUser" adicionando o código realçado em amarelo ao manipulador de `Page_Load`. O manipulador de `Page_Load` será exibido da seguinte maneira:   

    [!code-csharp[Main](membership-and-administration/samples/sample6.cs?highlight=3-6)]

Quando a página é carregada, o código verifica se o usuário conectado tem a função "CanEdit". Se o usuário pertencer à função "CanEdit", o elemento span que contém o link para a página *AdminPage. aspx* (e, consequentemente, o link dentro do SPAN) se tornará visível.

### <a name="enabling-product-administration"></a>Habilitando a administração do produto

Até agora, você criou a função "CanEdit" e adicionou um usuário "canEditUser", uma pasta de administração e uma página de administração. Você definiu direitos de acesso para a pasta e a página de administração e adicionou um link de navegação para o usuário da função "CanEdit" para o aplicativo. Em seguida, você adicionará marcação à página *AdminPage. aspx* e o código ao arquivo code-behind *AdminPage.aspx.cs* , que permitirá que o usuário com a função "CanEdit" Adicione e remova produtos.

1. No **Gerenciador de soluções**, abra o arquivo *AdminPage. aspx* da pasta *admin* .
2. Substitua a marcação existente pelo seguinte:  

    [!code-aspx[Main](membership-and-administration/samples/sample7.aspx)]
3. Em seguida, abra o arquivo code-behind *AdminPage.aspx.cs* clicando com o botão direito do mouse em *AdminPage. aspx* e clicando em **Exibir código**.
4. Substitua o código existente no arquivo code-behind *AdminPage.aspx.cs* pelo código a seguir:  

    [!code-csharp[Main](membership-and-administration/samples/sample8.cs)]

No código que você inseriu para o arquivo code-behind *AdminPage.aspx.cs* , uma classe chamada `AddProducts` faz o trabalho real de adicionar produtos ao banco de dados. Essa classe ainda não existe, portanto, você a criará agora.

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse na pasta *lógica* e, em seguida, selecione **Adicionar** -&gt; **novo item**.   
   A caixa de diálogo **Adicionar Novo Item** é exibida.
2. Selecione o grupo de modelos de **código** do **Visual C#**  -&gt; à esquerda. Em seguida, selecione **classe**na lista intermediária e nomeie-a *AddProducts.cs*.   
   O novo arquivo de classe é exibido.
3. Substitua o código existente pelo seguinte:  

    [!code-csharp[Main](membership-and-administration/samples/sample9.cs)]

A página *AdminPage. aspx* permite que o usuário que pertence à função "CanEdit" Adicione e remova produtos. Quando um novo produto é adicionado, os detalhes sobre o produto são validados e, em seguida, inseridos no banco de dados. O novo produto está imediatamente disponível para todos os usuários do aplicativo Web.

#### <a name="unobtrusive-validation"></a>Validação não invasiva

Os detalhes do produto que o usuário fornece na página *AdminPage. aspx* são validados usando controles de validação (`RequiredFieldValidator` e `RegularExpressionValidator`). Esses controles usam automaticamente a validação discreta. A validação não invasiva permite que os controles de validação usem JavaScript para lógica de validação do lado do cliente, o que significa que a página não requer uma viagem para que o servidor seja validado. Por padrão, a validação discreta é incluída no arquivo *Web. config* com base na seguinte configuração:

[!code-xml[Main](membership-and-administration/samples/sample10.xml)]

#### <a name="regular-expressions"></a>Expressões Regulares

O preço do produto na página *AdminPage. aspx* é validado usando um controle **RegularExpressionValidator** . Esse controle valida se o valor do controle de entrada associado (a caixa de texto "AddProductPrice") corresponde ao padrão especificado pela expressão regular. Uma expressão regular é uma notação de correspondência de padrões que permite localizar e corresponder rapidamente padrões de caracteres específicos. O controle **RegularExpressionValidator** inclui uma propriedade chamada `ValidationExpression` que contém a expressão regular usada para validar a entrada de preço, conforme mostrado abaixo:

[!code-aspx[Main](membership-and-administration/samples/sample11.aspx)]

#### <a name="fileupload-control"></a>Controle FileUpload

Além dos controles de entrada e validação, você adicionou o controle **FileUpload** à página *AdminPage. aspx* . Esse controle fornece a capacidade de carregar arquivos. Nesse caso, você só permite que arquivos de imagem sejam carregados. No arquivo code-behind (*AdminPage.aspx.cs*), quando o `AddProductButton` é clicado, o código verifica a propriedade `HasFile` do controle **FileUpload** . Se o controle tiver um arquivo e se o tipo de arquivo (baseado na extensão de arquivo) for permitido, a imagem será salva na pasta *imagens* e na pasta *imagens/polegares* do aplicativo.

#### <a name="model-binding"></a>Model binding

Anteriormente nesta série de tutoriais, você usou a associação de modelo para popular um controle **ListView** , um controle **FormsView** , um controle **GridView** e um controle **detailview** . Neste tutorial, você usa a associação de modelo para preencher um controle **DropDownList** com uma lista de categorias de produtos.

A marcação que você adicionou ao arquivo *AdminPage. aspx* contém um controle **DropDownList** chamado `DropDownAddCategory`:

[!code-aspx[Main](membership-and-administration/samples/sample12.aspx)]

Você usa a associação de modelo para popular essa **DropDownList** definindo o atributo `ItemType` e o atributo `SelectMethod`. O atributo `ItemType` especifica que você usa o tipo de `WingtipToys.Models.Category` ao preencher o controle. Você definiu esse tipo no início desta série de tutoriais criando a classe `Category` (mostrada abaixo). A classe `Category` está na pasta *modelos* dentro do arquivo *Category.cs* .

[!code-csharp[Main](membership-and-administration/samples/sample13.cs)]

O atributo `SelectMethod` do controle **DropDownList** especifica que você usa o método `GetCategories` (mostrado abaixo) que está incluído no arquivo code-behind (*AdminPage.aspx.cs*).

[!code-csharp[Main](membership-and-administration/samples/sample14.cs)]

Esse método especifica que uma interface `IQueryable` é usada para avaliar uma consulta em relação a um tipo de `Category`. O valor retornado é usado para popular a **DropDownList** na marcação da página (*AdminPage. aspx*).

O texto exibido para cada item na lista é especificado definindo o atributo `DataTextField`. O atributo `DataTextField` usa a `CategoryName` da classe `Category` (mostrada acima) para exibir cada categoria no controle **DropDownList** . O valor real que é passado quando um item é selecionado no controle **DropDownList** é baseado no atributo `DataValueField`. O atributo `DataValueField` é definido como o `CategoryID` como definido na classe `Category` (mostrada acima).

### <a name="how-the-application-will-work"></a>Como o aplicativo funcionará

Quando o usuário pertencente à função "CanEdit" navega até a página pela primeira vez, o controle `DropDownAddCategory`**DropDownList** é populado conforme descrito acima. O controle `DropDownRemoveProduct`**DropDownList** também é preenchido com produtos que usam a mesma abordagem. O usuário pertencente à função "CanEdit" seleciona o tipo de categoria e adiciona detalhes do produto (**nome**, **Descrição**, **preço**e **arquivo de imagem**). Quando o usuário pertencente à função "CanEdit" clica no botão **Adicionar produto** , o manipulador de eventos `AddProductButton_Click` é disparado. O manipulador de eventos `AddProductButton_Click` localizado no arquivo code-behind (*AdminPage.aspx.cs*) verifica o arquivo de imagem para certificar-se de que ele corresponde aos tipos de arquivo permitidos *(. gif*, *. png*, *. jpeg*ou *. jpg*). Em seguida, o arquivo de imagem é salvo em uma pasta do aplicativo de exemplo Wingtip Toys. Em seguida, o novo produto é adicionado ao banco de dados. Para realizar a adição de um novo produto, uma nova instância da classe `AddProducts` é criada e denominada produtos. A classe `AddProducts` tem um método chamado `AddProduct`e o objeto Products chama esse método para adicionar produtos ao banco de dados.

[!code-csharp[Main](membership-and-administration/samples/sample15.cs)]

Se o código Adicionar o novo produto ao banco de dados com êxito, a página será recarregada com o valor da cadeia de caracteres de consulta `ProductAction=add`.

[!code-csharp[Main](membership-and-administration/samples/sample16.cs)]

Quando a página é recarregada, a cadeia de caracteres de consulta é incluída na URL. Ao recarregar a página, o usuário pertencente à função "CanEdit" pode ver imediatamente as atualizações nos controles **DropDownList** na página *AdminPage. aspx* . Além disso, ao incluir a cadeia de caracteres de consulta com a URL, a página pode exibir uma mensagem de êxito para o usuário que pertence à função "CanEdit".

Quando a página *AdminPage. aspx* é recarregada, o evento `Page_Load` é chamado.

[!code-csharp[Main](membership-and-administration/samples/sample17.cs)]

O manipulador de eventos `Page_Load` verifica o valor da cadeia de caracteres de consulta e determina se uma mensagem de êxito deve ser exibida.

## <a name="running-the-application"></a>Executando o aplicativo

Você pode executar o aplicativo agora para ver como você pode adicionar, excluir e atualizar itens no carrinho de compras. O total do carrinho de compras refletirá o custo total de todos os itens no carrinho de compras.

1. Em Gerenciador de Soluções, pressione **F5** para executar o aplicativo de exemplo Wingtip Toys.  
   O navegador é aberto e mostra a página *Default. aspx* .
2. Clique no link **fazer logon** na parte superior da página. 

    ![Associação e administração-link de logon](membership-and-administration/_static/image2.png)

   A página *login. aspx* é exibida.
3. Use o seguinte nome de usuário e senha:  
   Nome de usuário: canEditUser@wingtiptoys.com  
   Senha: PA $ $word 1 

    ![Associação e administração – página de logon](membership-and-administration/_static/image3.png)
4. Clique no botão **fazer logon** próximo à parte inferior da página.
5. Na parte superior da página seguinte, selecione o link de **administrador** para navegar até a página *AdminPage. aspx* . 

    ![Associação e administração-link de administrador](membership-and-administration/_static/image4.png)
6. Para testar a validação de entrada, clique no botão **Adicionar produto** sem adicionar detalhes do produto. 

    ![Associação e administração – página de administração](membership-and-administration/_static/image5.png)

   Observe que as mensagens de campo necessárias são exibidas.
7. Adicione os detalhes de um novo produto e, em seguida, clique no botão **Adicionar produto** . 

    ![Associação e administração – adicionar produto](membership-and-administration/_static/image6.png)
8. Selecione **produtos** no menu de navegação superior para exibir o novo produto adicionado. 

    ![Associação e administração – mostrar novo produto](membership-and-administration/_static/image7.png)
9. Clique no link **administrador** para retornar à página Administração.
10. Na seção **remover produto** da página, selecione o novo produto que você adicionou no **DropDownListBox**.
11. Clique no botão **remover produto** para remover o novo produto do aplicativo. 

    ![Associação e administração – remover produto](membership-and-administration/_static/image8.png)
12. Selecione **produtos** no menu de navegação superior para confirmar que o produto foi removido.
13. Clique em **fazer logoff** para existir no modo de administração.   
    Observe que o painel de navegação superior não mostra mais o item de menu **admin** .

## <a name="summary"></a>Resumo

Neste tutorial, você adicionou uma função personalizada e um usuário que pertence à função personalizada, acesso restrito à pasta e à página de administração e forneceu navegação para o usuário pertencente à função personalizada. Você usou a associação de modelo para popular um controle **DropDownList** com dados. Você implementou o controle **FileUpload** e os controles de validação. Além disso, você aprendeu a adicionar e remover produtos de um banco de dados. No próximo tutorial, você aprenderá a implementar o roteamento do ASP.NET.

## <a name="additional-resources"></a>Recursos adicionais

[Elemento Web. config-Authorization](https://msdn.microsoft.com/library/8d82143t(v=vs.100).aspx)  
[ASP.NET Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md)  
[Implantar um aplicativo ASP.NET Web Forms seguro com associação, OAuth e banco de dados SQL em um site do Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Avaliação gratuita Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [Anterior](checkout-and-payment-with-paypal.md)
> [Próximo](url-routing.md)
