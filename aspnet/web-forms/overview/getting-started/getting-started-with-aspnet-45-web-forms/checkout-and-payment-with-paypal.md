---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
title: Checkout e pagamento com o PayPal | Microsoft Docs
author: Erikre
description: Esta série de tutoriais ensinará as noções básicas da criação de um aplicativo ASP.NET Web Forms usando o ASP.NET 4,5 e o Microsoft Visual Studio Express 2013 para nós...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 664ec95e-b0c9-4f43-a39f-798d0f2a7e08
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
msc.type: authoredcontent
ms.openlocfilehash: 62d00a86c6c5845fb894896df65002c7086d039f
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74615142"
---
# <a name="checkout-and-payment-with-paypal"></a>Check-out e pagamento com o PayPal

por [Erik Reitan](https://github.com/Erikre)

[Baixar o projeto de exemplo WingtipC#Toys ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [baixar o livro eletrônico (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta série de tutoriais ensinará as noções básicas da criação de um aplicativo ASP.NET Web Forms usando o ASP.NET 4,5 e o Microsoft Visual Studio Express 2013 para a Web. Um [projeto Visual Studio 2013 com C# código-fonte](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) está disponível para acompanhar esta série de tutoriais.

Este tutorial descreve como modificar o aplicativo de exemplo Wingtip Toys para incluir autorização, registro e pagamento do usuário usando o PayPal. Somente os usuários que fizeram logon terão autorização para comprar produtos. A funcionalidade interna de registro de usuário do modelo de projeto do ASP.NET Web Forms 4,5 já inclui muito do que você precisa. Você adicionará a funcionalidade de check-out do PayPal Express. Neste tutorial, você está usando o ambiente de teste de desenvolvedor do PayPal, portanto, nenhum fundos reais será transferido. No final do tutorial, você testará o aplicativo selecionando produtos para adicionar ao carrinho de compras, clicando no botão Check-out e transferindo dados para o site de teste do PayPal. No site de teste do PayPal, você confirmará suas informações de envio e pagamento e, em seguida, retornará ao aplicativo de exemplo local Wingtip Toys para confirmar e concluir a compra.

Há vários processadores de pagamento de terceiros experientes que são especializados em compras online que abordam a escalabilidade e a segurança. Os desenvolvedores de ASP.NET devem considerar as vantagens de utilizar uma solução de pagamento de terceiros antes de implementar uma solução de compras e compras.

> [!NOTE] 
> 
> O aplicativo de exemplo Wingtip Toys foi projetado para mostrar os conceitos e os recursos específicos do ASP.NET disponíveis para os desenvolvedores da Web do ASP.NET. Este aplicativo de exemplo não foi otimizado para todas as circunstâncias possíveis em relação à escalabilidade e à segurança.

## <a name="what-youll-learn"></a>O que você aprenderá:

- Como restringir o acesso a páginas específicas em uma pasta.
- Como criar um carrinho de compras conhecido de um carrinho de compras anônimo.
- Como habilitar o SSL para o projeto.
- Como adicionar um provedor OAuth ao projeto.
- Como usar o PayPal para comprar produtos usando o ambiente de teste do PayPal.
- Como exibir detalhes do PayPal em um controle **DetailsView** .
- Como atualizar o banco de dados do aplicativo Wingtip Toys com detalhes obtidos no PayPal.

## <a name="adding-order-tracking"></a>Adicionando o acompanhamento de pedidos

Neste tutorial, você criará duas novas classes para acompanhar dados da ordem que um usuário criou. As classes acompanharão os dados referentes a informações de envio, total de compras e confirmação de pagamento.

### <a name="add-the-order-and-orderdetail-model-classes"></a>Adicionar as classes de modelo Order e OrderDetail

Anteriormente nesta série de tutoriais, você definiu o esquema para categorias, produtos e itens do carrinho de compras criando as classes `Category`, `Product`e `CartItem` na pasta *modelos* . Agora, você adicionará duas novas classes para definir o esquema para a ordem do produto e os detalhes do pedido.

1. Na pasta **modelos** , adicione uma nova classe chamada *Order.cs*.   
   O novo arquivo de classe é exibido no editor.
2. Substitua o código padrão pelo seguinte:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample1.cs)]
3. Adicione uma classe *OrderDetail.cs* à pasta *modelos* .
4. Substitua o código padrão pelo código a seguir:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample2.cs)]

As classes `Order` e `OrderDetail` contêm o esquema para definir as informações de pedidos usadas para compra e envio.

Além disso, você precisará atualizar a classe de contexto do banco de dados que gerencia as classes de entidade e que fornece acesso a um banco de dado. Para fazer isso, você adicionará o pedido recém-criado e as classes de modelo de `OrderDetail` para `ProductContext` classe.

1. Em **Gerenciador de soluções**, localize e abra o arquivo *ProductContext.cs* .
2. Adicione o código realçado ao arquivo *ProductContext.cs* , conforme mostrado abaixo:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample3.cs?highlight=14-15)]

Conforme mencionado anteriormente nesta série de tutoriais, o código no arquivo *ProductContext.cs* adiciona o namespace `System.Data.Entity` para que você tenha acesso a todas as funcionalidades principais do Entity Framework. Essa funcionalidade inclui a capacidade de consultar, inserir, atualizar e excluir dados trabalhando com objetos fortemente tipados. O código acima na classe `ProductContext` adiciona Entity Framework acesso às classes `Order` e `OrderDetail` adicionadas recentemente.

## <a name="adding-checkout-access"></a>Adicionando acesso de check-out

O aplicativo de exemplo Wingtip Toys permite que usuários anônimos examinem e adicionem produtos a um carrinho de compras. No entanto, quando os usuários anônimos optam por comprar os produtos que eles adicionaram ao carrinho de compras, eles devem fazer logon no site. Depois que tiverem feito logon, eles poderão acessar as páginas restritas do aplicativo Web que lidam com o processo de check-out e compra. Essas páginas restritas estão contidas na pasta de *check-out* do aplicativo.

### <a name="add-a-checkout-folder-and-pages"></a>Adicionar uma pasta e páginas de check-out

Agora, você criará a pasta de *check-out* e as páginas que o cliente verá durante o processo de check-out. Você atualizará essas páginas posteriormente neste tutorial.

1. Clique com o botão direito do mouse no nome do projeto (**Wingtip Toys**) em **Gerenciador de soluções** e selecione **Adicionar uma nova pasta**. 

    ![Checkout e pagamento com o PayPal – nova pasta](checkout-and-payment-with-paypal/_static/image1.png)
2. Nomeie a nova pasta *checkout*.
3. Clique com o botão direito do mouse na pasta de *check-out* e selecione **Adicionar**-&gt;**novo item**. 

    ![Checkout e pagamento com o PayPal – novo item](checkout-and-payment-with-paypal/_static/image2.png)
4. A caixa de diálogo **Adicionar Novo Item** é exibida.
5. Selecione o grupo modelos **da Web** do **Visual C#**  -&gt; à esquerda. Em seguida, no painel central, selecione **Web Form com página mestra**e nomeie-o *CheckoutStart. aspx*. 

    ![Check-out e pagamento com o PayPal – adicionar novo item caixa de diálogo](checkout-and-payment-with-paypal/_static/image3.png)
6. Como antes, selecione o arquivo *site. Master* como a página mestra.
7. Adicione as seguintes páginas adicionais à pasta de *check-out* usando as mesmas etapas acima:   

    - CheckoutReview. aspx
    - CheckoutComplete. aspx
    - CheckoutCancel. aspx
    - CheckoutError. aspx

### <a name="add-a-webconfig-file"></a>Adicionar um arquivo Web. config

Ao adicionar um novo arquivo *Web. config* à pasta de *check-out* , você poderá restringir o acesso a todas as páginas contidas na pasta.

1. Clique com o botão direito do mouse na pasta de *check-out* e selecione **Adicionar** -&gt; **novo item**.  
   A caixa de diálogo **Adicionar Novo Item** é exibida.
2. Selecione o grupo modelos **da Web** do **Visual C#**  -&gt; à esquerda. Em seguida, no painel central, selecione **arquivo de configuração da Web**, aceite o nome padrão de *Web. config*e, em seguida, selecione **Adicionar**.
3. Substitua o conteúdo XML existente no arquivo *Web. config* pelo seguinte:  

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample4.xml)]
4. Salve o arquivo *Web. config* .

O arquivo *Web. config* especifica que todos os usuários desconhecidos do aplicativo Web devem ter acesso negado às páginas contidas na pasta de *check-out* . No entanto, se o usuário tiver registrado uma conta e estiver conectado, ele será um usuário conhecido e terá acesso às páginas na pasta de *check-out* .

É importante observar que a configuração do ASP.NET segue uma hierarquia, em que cada arquivo *Web. config* aplica as definições de configuração à pasta na qual ela está e a todos os diretórios filho abaixo dela.

<a id="SSLWebForms"></a>
## <a name="enable-ssl-for-the-project"></a>Habilitar SSL para o projeto

 O protocolo SSL (SSL) é um protocolo definido para permitir que servidores Web e clientes Web se comuniquem com mais segurança por meio do uso de criptografia. Quando o SSL não é usado, os dados enviados entre o cliente e o servidor são abertos para a detecção de pacotes por qualquer pessoa com acesso físico à rede. Além disso, vários esquemas de autenticação comuns não são seguros em relação a HTTP simples. Em particular, autenticação básica e autenticação de formulários enviam credenciais não criptografadas. Para ser seguro, esses esquemas de autenticação devem usar SSL. 

1. Em **Gerenciador de soluções**, clique no projeto **WingtipToys** e pressione **F4** para exibir a janela **Propriedades** .
2. Altere o **SSL habilitado** para `true`.
3. Copie a **URL do SSL** para que você possa usá-la mais tarde.   
 A URL SSL será `https://localhost:44300/`, a menos que você tenha criado anteriormente sites SSL (como mostrado abaixo).   
    ![Propriedades de projeto](checkout-and-payment-with-paypal/_static/image4.png)
4. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto **WingtipToys** e clique em **Propriedades**.
5. Na guia à esquerda, clique em **Web**.
6. Altere a **URL do projeto** para usar a **URL SSL** que você salvou anteriormente.   
    ![](checkout-and-payment-with-paypal/_static/image5.png) de propriedades da Web do projeto
7. Salve a página pressionando **Ctrl + S**.
8. Pressione **CTRL+F5** para executar o aplicativo. O Visual Studio exibirá uma opção para permitir que você evite os avisos SSL.
9. Clique em **Sim** para confiar no certificado SSL de IIS Express e continuar.   
    detalhes do certificado SSL de ![IIS Express](checkout-and-payment-with-paypal/_static/image6.png)  
 Um aviso de segurança é exibido.
10. Clique em **Sim** para instalar o certificado em seu localhost.   
    caixa de diálogo ![aviso de segurança](checkout-and-payment-with-paypal/_static/image7.png)  
 A janela do navegador será exibida.

Agora você pode testar facilmente seu aplicativo Web localmente usando SSL.

<a id="OAuthWebForms"></a>
## <a name="add-an-oauth-20-provider"></a>Adicionar um provedor OAuth 2,0

ASP.NET Web Forms fornece opções aprimoradas para associação e autenticação. Esses aprimoramentos incluem o OAuth. O OAuth é um protocolo aberto que permite a autorização segura em um método simples e padrão de aplicativos Web, móveis e de área de trabalho. O modelo de Web Forms ASP.NET usa o OAuth para expor o Facebook, o Twitter, o Google e a Microsoft como provedores de autenticação. Embora este tutorial Use apenas o Google como o provedor de autenticação, você pode facilmente modificar o código para usar qualquer um dos provedores. As etapas para implementar outros provedores são muito semelhantes às etapas que você verá neste tutorial.

Além da autenticação, o tutorial também usará funções para implementar a autorização. Somente os usuários adicionados à função de `canEdit` poderão alterar os dados (criar, editar ou excluir contatos).

> [!NOTE] 
> 
> Os aplicativos do Windows Live aceitam apenas uma URL ativa para um site de trabalho, portanto, você não pode usar uma URL do site local para testar logons.

As etapas a seguir permitirão que você adicione um provedor de autenticação do Google.

1. Abra o *aplicativo\_arquivo Start\Startup.auth.cs* .
2. Remova os caracteres de comentário do método `app.UseGoogleAuthentication()` para que o método seja exibido da seguinte maneira: 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample5.cs)]
3. Navegue até o [console de desenvolvedores do Google](https://console.developers.google.com/). Você também precisará entrar com sua conta de email do Google Developer (gmail.com). Se você não tiver uma conta do Google, selecione o link **criar uma conta** .   
   Em seguida, você verá o **console de desenvolvedores do Google**.   
    ![console de desenvolvedores do Google](checkout-and-payment-with-paypal/_static/image8.png)
4. Clique no botão **criar projeto** e insira um nome e uma ID do projeto (você pode usar os valores padrão). Em seguida, clique na **caixa de seleção contrato** e no botão **criar** .  

    ![Google-novo projeto](checkout-and-payment-with-paypal/_static/image9.png)

   Em alguns segundos, o novo projeto será criado e o navegador exibirá a página novos projetos.
5. Na guia à esquerda, clique em **APIs &amp; autenticação**e, em seguida, clique em **credenciais**.
6. Clique em **criar nova ID de cliente** em **OAuth**.   
   A caixa de diálogo **criar ID do cliente** será exibida.   
    ![Google-criar ID do cliente](checkout-and-payment-with-paypal/_static/image10.png)
7. Na caixa de diálogo **criar ID do cliente** , mantenha o **aplicativo Web** padrão para o tipo de aplicativo.
8. Defina as **origens de JavaScript autorizadas** para a URL SSL usada anteriormente neste tutorial (`https://localhost:44300/`, a menos que você tenha criado outros projetos SSL).   
   Essa URL é a origem do seu aplicativo. Para este exemplo, você só inserirá a URL de teste do localhost. No entanto, você pode inserir várias URLs para considerar o localhost e a produção.
9. Defina o **URI de redirecionamento autorizado** para o seguinte: 

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample6.html)]

   Esse valor é o URI que ASP.NET os usuários do OAuth a se comunicarem com o servidor OAuth do Google. Lembre-se da URL SSL usada acima (`https://localhost:44300/`, a menos que você tenha criado outros projetos SSL).
10. Clique no botão **criar ID do cliente** .
11. No menu à esquerda do console de desenvolvedores do Google, clique no item de menu da **tela de consentimento** e defina seu endereço de email e o nome do produto. Quando você tiver concluído o formulário, clique em **salvar**.
12. Clique no item de menu **APIs** , role para baixo e clique no botão **desativar** ao lado de **API do Google +** .   
    Aceitar esta opção habilitará a API do Google +.
13. Você também deve atualizar o pacote NuGet **Microsoft. Owin** para a versão 3.0.0.   
    No menu **ferramentas** , selecione **Gerenciador de pacotes NuGet** e, em seguida, selecione **gerenciar pacotes NuGet para solução**.  
    Na janela **gerenciar pacotes NuGet** , localize e atualize o pacote **Microsoft. Owin** para a versão 3.0.0.
14. No Visual Studio, atualize o método `UseGoogleAuthentication` da página *Startup.auth.cs* copiando e colando a **ID do cliente** e o **segredo do cliente** no método. Os valores de **ID do cliente** e **segredo do cliente** mostrados abaixo são exemplos e não funcionarão. 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample7.cs?highlight=64-65)]
15. Pressione **Ctrl + F5** para compilar e executar o aplicativo. Clique no link **fazer logon** .
16. Em **usar outro serviço para fazer logon**, clique em **Google**.  
    ![fazer logon](checkout-and-payment-with-paypal/_static/image11.png)
17. Se precisar inserir suas credenciais, você será redirecionado para o site do Google, onde você inserirá suas credenciais.  
    ![Google-entrar](checkout-and-payment-with-paypal/_static/image12.png)
18. Depois de inserir suas credenciais, você será solicitado a conceder permissões para o aplicativo Web que acabou de criar.  
    ![Conta de serviço padrão do projeto](checkout-and-payment-with-paypal/_static/image13.png)
19. Clique em **aceitar**. Agora você será Redirecionado de volta para a página de **registro** do aplicativo **WingtipToys** , onde você pode registrar sua conta do Google.  
    ![registrar com sua conta do Google](checkout-and-payment-with-paypal/_static/image14.png)
20. Você tem a opção de alterar o nome de registro de email local usado para sua conta do Gmail, mas geralmente deseja manter o alias de email padrão (ou seja, aquele que você usou para autenticação). Clique em **fazer logon** , conforme mostrado acima.

### <a name="modifying-login-functionality"></a>Modificando a funcionalidade de logon

Como mencionado anteriormente nesta série de tutoriais, grande parte da funcionalidade de registro de usuário foi incluída no modelo de Web Forms de ASP.NET por padrão. Agora, você modificará as páginas *login. aspx* e *Register. aspx* padrão para chamar o método `MigrateCart`. O método `MigrateCart` associa um usuário conectado recentemente a um carrinho de compras anônimo. Ao associar o usuário e o carrinho de compras, o aplicativo de exemplo Wingtip Toys poderá manter o carrinho de compras do usuário entre as visitas.

1. Em **Gerenciador de soluções**, localize e abra a pasta *conta* .
2. Modifique a página code-behind chamada *login.aspx.cs* para incluir o código realçado em amarelo, para que ele apareça da seguinte maneira:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample8.cs?highlight=41-43)]
3. Salve o arquivo *login.aspx.cs* .

Por enquanto, você pode ignorar o aviso de que não há nenhuma definição para o método `MigrateCart`. Você vai adicioná-lo um pouco mais adiante neste tutorial.

O arquivo code-behind *login.aspx.cs* dá suporte a um método de logon. Ao inspecionar a página login. aspx, você verá que essa página inclui um botão "fazer logon" que, quando clica, dispara o manipulador de `LogIn` no code-behind.

Quando o método de `Login` em *login.aspx.cs* é chamado, uma nova instância do carrinho de compras denominada `usersShoppingCart` é criada. A ID do carrinho de compras (um GUID) é recuperada e definida como a variável `cartId`. Em seguida, o método `MigrateCart` é chamado, passando o `cartId` e o nome do usuário conectado para esse método. Quando o carrinho de compras é migrado, o GUID usado para identificar o carrinho de compras anônimo é substituído pelo nome de usuário.

Além de modificar o arquivo code-behind *login.aspx.cs* para migrar o carrinho de compras quando o usuário fizer logon, você também deverá modificar o *arquivo code-behind Register.aspx.cs* para migrar o carrinho de compras quando o usuário criar uma nova conta e fizer logon.

1. Na pasta *conta* , abra o arquivo code-behind chamado *Register.aspx.cs*.
2. Modifique o arquivo code-behind incluindo o código em amarelo, para que ele apareça da seguinte maneira:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample9.cs?highlight=28-32)]
3. Salve o arquivo *Register.aspx.cs* . Mais uma vez, ignore o aviso sobre o método `MigrateCart`.

Observe que o código usado no manipulador de eventos `CreateUser_Click` é muito semelhante ao código usado no método `LogIn`. Quando o usuário registra ou faz logon no site, uma chamada para o método de `MigrateCart` será feita.

## <a name="migrating-the-shopping-cart"></a>Migrando o carrinho de compras

Agora que o processo de logon e de registro foi atualizado, você pode adicionar o código para migrar o carrinho de compras usando o método `MigrateCart`.

1. Em **Gerenciador de soluções**, localize a pasta *lógica* e abra o arquivo de classe *ShoppingCartActions.cs* .
2. Adicione o código realçado em amarelo ao código existente no arquivo *ShoppingCartActions.cs* , para que o código no arquivo *ShoppingCartActions.cs* seja exibido da seguinte maneira:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample10.cs?highlight=215-224)]

O método `MigrateCart` usa o cartid existente para localizar o carrinho de compras do usuário. Em seguida, o código percorre todos os itens do carrinho de compras e substitui a propriedade `CartId` (conforme especificado pelo esquema de `CartItem`) pelo nome de usuário conectado.

### <a name="updating-the-database-connection"></a>Atualizando a conexão de banco de dados

Se você estiver seguindo este tutorial usando o aplicativo de exemplo da Wingtip Toys **predefinido** , deverá recriar o banco de dados de associação padrão. Ao modificar a cadeia de conexão padrão, o banco de dados de associação será criado na próxima vez em que o aplicativo for executado.

1. Abra o arquivo *Web. config* na raiz do projeto.
2. Atualize a cadeia de conexão padrão para que ela apareça da seguinte maneira:   

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample11.xml)]

<a id="PayPalWebForms"></a>
## <a name="integrating-paypal"></a>Integrando o PayPal

O PayPal é uma plataforma de cobrança baseada na Web que aceita pagamentos por comerciantes online. Em seguida, este tutorial explica como integrar a funcionalidade de check-out expresso do PayPal ao seu aplicativo. O Express Checkout permite que seus clientes usem o PayPal para pagar pelos itens que eles adicionaram ao carrinho de compras.

### <a name="create-paypal-test-accounts"></a>Criar contas de teste do PayPal

Para usar o ambiente de teste do PayPal, você deve criar e verificar uma conta de teste do desenvolvedor. Você usará a conta de teste do desenvolvedor para criar uma conta de teste do comprador e uma conta de teste do vendedor. As credenciais da conta de teste do desenvolvedor também permitirão que o aplicativo de exemplo Wingtip Toys acesse o ambiente de teste do PayPal.

1. Em um navegador, navegue até o site de teste do desenvolvedor do PayPal:   
    [https://developer.paypal.com](https://developer.paypal.com/)
2. Se você não tiver uma conta de desenvolvedor do PayPal, crie uma nova conta clicando em **inscrever-se**e seguindo as etapas de inscrição. Se você tiver uma conta de desenvolvedor do PayPal existente, entre clicando em **fazer logon**. Você precisará da sua conta de desenvolvedor do PayPal para testar o aplicativo de exemplo Wingtip Toys posteriormente neste tutorial.
3. Se você acabou de se inscrever para sua conta de desenvolvedor do PayPal, talvez seja necessário verificar sua conta de desenvolvedor do PayPal com o PayPal. Você pode verificar sua conta seguindo as etapas que o PayPal enviou para sua conta de email. Depois de verificar sua conta de desenvolvedor do PayPal, faça logon novamente no site de teste do desenvolvedor do PayPal.
4. Depois de fazer logon no site do desenvolvedor do PayPal com sua conta de desenvolvedor do PayPal, você precisará criar uma conta de teste do comprador do PayPal se ainda não tiver uma. Para criar uma conta de teste do comprador, no site do PayPal, clique na guia **aplicativos** e clique em **contas da área restrita**.   
 A página **contas de teste da área restrita** é mostrada.   

    > [!NOTE] 
    > 
    > O site do desenvolvedor do PayPal já fornece uma conta de teste de comerciante.

    ![Check-out e pagamento com contas de teste do PayPal-sandbox](checkout-and-payment-with-paypal/_static/image15.png)
5. Na página contas de teste da área restrita, clique em **criar conta**.
6. Na página **criar conta de teste** , escolha um email de conta de teste do comprador e a senha de sua escolha.   

    > [!NOTE] 
    > 
    > Você precisará dos endereços de email do comprador e da senha para testar o aplicativo de exemplo Wingtip Toys no final deste tutorial.

    ![Check-out e pagamento com contas de teste do PayPal-sandbox](checkout-and-payment-with-paypal/_static/image16.png)
7. Crie a conta de teste do comprador clicando no botão **criar conta** .  
 A página **contas de teste de área restrita** é exibida. 

    ![Checkout e pagamento com contas do PayPal-PayPal](checkout-and-payment-with-paypal/_static/image17.png)
8. Na página **contas de teste da área restrita** , clique na conta de email do **facilitador** .  
    As opções de **perfil** e de **notificação** são exibidas.
9. Selecione a opção **perfil** e clique em **credenciais da API** para exibir suas credenciais de API para a conta de teste de comerciante.
10. Copie as credenciais da API de teste para o bloco de notas.

Você precisará de suas credenciais de API de teste clássico exibidas (nome de usuário, senha e assinatura) para fazer chamadas à API do aplicativo de exemplo Wingtip Toys para o ambiente de teste do PayPal. Você adicionará as credenciais na próxima etapa.

### <a name="add-paypal-class-and-api-credentials"></a>Adicionar credenciais de classe e API do PayPal

Você irá posicionar a maioria do código PayPal em uma única classe. Essa classe contém os métodos usados para se comunicar com o PayPal. Além disso, você adicionará suas credenciais do PayPal a essa classe.

1. No aplicativo de exemplo Wingtip Toys no Visual Studio, clique com o botão direito do mouse na pasta **lógica** e selecione **Adicionar** -&gt; **novo item**.   
   A caixa de diálogo **Adicionar Novo Item** é exibida.
2. Em **Visual C#**  no painel **instalado** à esquerda, selecione **código**.
3. No painel central, selecione **classe**. Nomeie essa nova classe **PayPalFunctions.cs**.
4. Clique em **Adicionar**.  
   O novo arquivo de classe é exibido no editor.
5. Substitua o código padrão pelo código a seguir:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample12.cs)]
6. Adicione as credenciais da API de comerciante (nome de usuário, senha e assinatura) que você exibiu anteriormente neste tutorial para que você possa fazer chamadas de função para o ambiente de teste do PayPal.  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample13.cs)]

> [!NOTE] 
> 
> Neste aplicativo de exemplo, você está simplesmente adicionando credenciais a C# um arquivo (. cs). No entanto, em uma solução implementada, você deve considerar a criptografia de suas credenciais em um arquivo de configuração.

A classe NVPAPICaller contém a maior parte da funcionalidade do PayPal. O código na classe fornece os métodos necessários para fazer uma compra de teste do ambiente de teste do PayPal. As três funções do PayPal a seguir são usadas para fazer compras:

- função `SetExpressCheckout`
- função `GetExpressCheckoutDetails`
- função `DoExpressCheckoutPayment`

O método `ShortcutExpressCheckout` coleta as informações de compra de teste e detalhes do produto do carrinho de compras e chama a função `SetExpressCheckout` do PayPal. O método `GetCheckoutDetails` confirma os detalhes da compra e chama a função de `GetExpressCheckoutDetails` do PayPal antes de fazer a compra de teste. O método `DoCheckoutPayment` conclui a compra de teste do ambiente de teste chamando a função `DoExpressCheckoutPayment` PayPal. O código restante dá suporte aos métodos e ao processo do PayPal, como cadeias de caracteres de codificação, decodificação de cadeias de caracteres, processamento de matrizes e determinação de credenciais.

> [!NOTE] 
> 
> O PayPal permite que você inclua detalhes de compra opcionais com base na [especificação de API do PayPal](https://cms.paypal.com/us/cgi-bin/?cmd=_render-content&amp;content_ID=developer/e_howto_api_nvp_r_SetExpressCheckout). Ao estender o código no aplicativo de exemplo Wingtip Toys, você pode incluir detalhes de localização, descrições de produtos, imposto, um número de atendimento ao cliente, bem como muitos outros campos opcionais.

Observe que as URLs de retorno e cancelamento especificadas no método **ShortcutExpressCheckout** usam um número de porta.

[!code-html[Main](checkout-and-payment-with-paypal/samples/sample14.html)]

Quando o Visual Web Developer executa um projeto Web usando SSL, normalmente a porta 44300 é usada para o servidor Web. Como mostrado acima, o número da porta é 44300. Ao executar o aplicativo, você poderá ver um número de porta diferente. O número da porta deve ser definido corretamente no código para que você possa executar com êxito o aplicativo de exemplo Wingtip Toys no final deste tutorial. A próxima seção deste tutorial explica como recuperar o número da porta do host local e atualizar a classe PayPal.

### <a name="update-the-localhost-port-number-in-the-paypal-class"></a>Atualizar o número da porta LocalHost na classe PayPal

O aplicativo de exemplo Wingtip Toys compra produtos navegando até o site de teste do PayPal e retornando à sua instância local do aplicativo de exemplo Wingtip Toys. Para que o PayPal retorne à URL correta, você precisa especificar o número da porta do aplicativo de exemplo em execução localmente no código do PayPal mencionado acima.

1. Clique com o botão direito do mouse no nome do projeto (**WingtipToys**) em **Gerenciador de soluções** e selecione **Propriedades**.
2. Na coluna à esquerda, selecione a guia **Web** .
3. Recupere o número da porta na caixa **URL do projeto** .
4. Se necessário, atualize o `returnURL` e `cancelURL` na classe PayPal (`NVPAPICaller`) no arquivo *PayPalFunctions.cs* para usar o número da porta do seu aplicativo Web:   

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample15.html?highlight=1-2)]

Agora, o código que você adicionou corresponderá à porta esperada para seu aplicativo Web local. O PayPal poderá retornar à URL correta no computador local.

### <a name="add-the-paypal-checkout-button"></a>Adicionar o botão de check-out do PayPal

Agora que as funções principais do PayPal foram adicionadas ao aplicativo de exemplo, você pode começar a adicionar a marcação e o código necessários para chamar essas funções. Primeiro, você deve adicionar o botão de check-out que o usuário verá na página do carrinho de compras.

1. Abra o arquivo *ShoppingCart. aspx* .
2. Role até a parte inferior do arquivo e localize o comentário `<!--Checkout Placeholder -->`.
3. Substitua o comentário por um controle de `ImageButton` para que a marcação seja substituída da seguinte maneira:  

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample16.aspx)]
4. No arquivo *ShoppingCart.aspx.cs* , após o manipulador de eventos `UpdateBtn_Click` próximo ao final do arquivo, adicione o manipulador de eventos `CheckOutBtn_Click`:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample17.cs)]
5. Também no arquivo *ShoppingCart.aspx.cs* , adicione uma referência à `CheckoutBtn`, para que o botão nova imagem seja referenciado da seguinte maneira:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample18.cs?highlight=18)]
6. Salve as alterações no arquivo *ShoppingCart. aspx* e no arquivo *ShoppingCart.aspx.cs* .
7. No menu, selecione **depurar**-&gt;**criar WingtipToys**.  
   O projeto será recriado com o controle **ImageButton** recém-adicionado.

### <a name="send-purchase-details-to-paypal"></a>Enviar detalhes de compra para o PayPal

Quando o usuário clica no botão **check-out** na página do carrinho de compras (*ShoppingCart. aspx*), ele iniciará o processo de compra. O código a seguir chama a primeira função PayPal necessária para comprar produtos.

1. Na pasta de *check-out* , abra o arquivo code-behind chamado *CheckoutStart.aspx.cs*.   
   Certifique-se de abrir o arquivo code-behind.
2. Substitua o código existente pelo seguinte:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample19.cs)]

Quando o usuário do aplicativo clicar no botão **checkout** na página carrinho de compras, o navegador navegará até a página *CheckoutStart. aspx* . Quando a página *CheckoutStart. aspx* é carregada, o método `ShortcutExpressCheckout` é chamado. Neste ponto, o usuário é transferido para o site de teste do PayPal. No site do PayPal, o usuário insere suas credenciais do PayPal, revisa os detalhes da compra, aceita o contrato do PayPal e retorna ao aplicativo de exemplo Wingtip Toys em que o método `ShortcutExpressCheckout` é concluído. Quando o método de `ShortcutExpressCheckout` estiver concluído, ele redirecionará o usuário para a página *CheckoutReview. aspx* especificada no método `ShortcutExpressCheckout`. Isso permite que o usuário examine os detalhes do pedido de dentro do aplicativo de exemplo Wingtip Toys.

### <a name="review-order-details"></a>Examinar detalhes do pedido

Depois de retornar do PayPal, a página *CheckoutReview. aspx* do aplicativo de exemplo Wingtip Toys exibe os detalhes do pedido. Esta página permite que o usuário examine os detalhes do pedido antes de comprar os produtos. A página *CheckoutReview. aspx* deve ser criada da seguinte maneira:

1. Na pasta de *check-out* , abra a página chamada *CheckoutReview. aspx*.
2. Substitua a marcação existente pelo seguinte:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample20.aspx)]
3. Abra a página code-behind chamada *CheckoutReview.aspx.cs* e substitua o código existente pelo seguinte:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample21.cs)]

O controle **DetailsView** é usado para exibir os detalhes do pedido que foram retornados do PayPal. Além disso, o código acima salva os detalhes do pedido no banco de dados Wingtip Toys como um objeto `OrderDetail`. Quando o usuário clica no botão **concluir ordem** , ele é redirecionado para a página *CheckoutComplete. aspx* .

> [!NOTE] 
> 
> **Dicas**
> 
> Na marcação da página *CheckoutReview. aspx* , observe que a marca `<ItemStyle>` é usada para alterar o estilo dos itens dentro do controle **DetailsView** próximo à parte inferior da página. Exibindo a página no **modo de exibição de design** (selecionando **design** no canto inferior esquerdo do Visual Studio), selecionando o controle **DetailsView** e selecionando a **marca inteligente** (o ícone de seta no canto superior direito do controle), você poderá ver as **tarefas de DetailsView**.
> 
> ![Checkout e pagamento com os campos PayPal-Edit](checkout-and-payment-with-paypal/_static/image18.png)
> 
> Ao selecionar **editar campos**, a caixa de diálogo **campos** será exibida. Nessa caixa de diálogo, você pode controlar facilmente as propriedades visuais, como **ItemStyle**, do controle **DetailsView** .
> 
> ![Caixa de diálogo check-out e pagamento com PayPal-Fields](checkout-and-payment-with-paypal/_static/image19.png)

### <a name="complete-purchase"></a>Concluir compra

A página *CheckoutComplete. aspx* faz a compra do PayPal. Conforme mencionado acima, o usuário deve clicar no botão **concluir ordem** antes que o aplicativo navegue para a página *CheckoutComplete. aspx* .

1. Na pasta de *check-out* , abra a página chamada *CheckoutComplete. aspx*.
2. Substitua a marcação existente pelo seguinte:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample22.aspx)]
3. Abra a página code-behind chamada *CheckoutComplete.aspx.cs* e substitua o código existente pelo seguinte:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample23.cs)]

Quando a página *CheckoutComplete. aspx* é carregada, o método `DoCheckoutPayment` é chamado. Como mencionado anteriormente, o método `DoCheckoutPayment` conclui a compra do ambiente de teste do PayPal. Depois que o PayPal concluir a compra do pedido, a página *CheckoutComplete. aspx* exibirá uma transação de pagamento `ID` para o comprador.

### <a name="handle-cancel-purchase"></a>Manipular cancelar compra

Se o usuário decidir cancelar a compra, ele será direcionado para a página *CheckoutCancel. aspx* , onde verá que o pedido foi cancelado.

1. Abra a página chamada *CheckoutCancel. aspx* na pasta de *check-out* .
2. Substitua a marcação existente pelo seguinte:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample24.aspx)]

### <a name="handle-purchase-errors"></a>Tratar erros de compra

Os erros durante o processo de compra serão tratados pela página *CheckoutError. aspx* . O code-behind da página *CheckoutStart. aspx* , a página *CheckoutReview. aspx* e a página *CheckoutComplete. aspx* serão redirecionados para a página *CheckoutError. aspx* se ocorrer um erro.

1. Abra a página chamada *CheckoutError. aspx* na pasta de *check-out* .
2. Substitua a marcação existente pelo seguinte:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample25.aspx)]

A página *CheckoutError. aspx* é exibida com os detalhes do erro quando ocorre um erro durante o processo de check-out.

## <a name="running-the-application"></a>Executando o aplicativo

Execute o aplicativo para ver como comprar produtos. Observe que você estará executando no ambiente de teste do PayPal. Nenhum dinheiro real está sendo trocado.

1. Verifique se todos os arquivos foram salvos no Visual Studio.
2. Abra um navegador da Web e navegue até [https://developer.paypal.com](https://developer.paypal.com/).
3. Faça logon com sua conta de desenvolvedor do PayPal que você criou anteriormente neste tutorial.  
   Para a área restrita do desenvolvedor do PayPal, você precisa estar conectado em [https://developer.paypal.com](https://developer.paypal.com/) para testar o check-out expresso. Isso se aplica somente ao teste da área restrita do PayPal, não ao ambiente em tempo real do PayPal.
4. No Visual Studio, pressione **F5** para executar o aplicativo de exemplo Wingtip Toys.  
   Após a recompilação do banco de dados, o navegador será aberto e mostrará a página *Default. aspx* .
5. Adicione três produtos diferentes ao carrinho de compras selecionando a categoria de produto, como "carros" e, em seguida, clicando em **Adicionar ao carrinho** ao lado de cada produto.  
   O carrinho de compras exibirá o produto que você selecionou.
6. Clique no botão **paypal** para fazer checkout. 

    ![Checkout e pagamento com o PayPal-cart](checkout-and-payment-with-paypal/_static/image20.png)

   Fazer check-out exigirá que você tenha uma conta de usuário para o aplicativo de exemplo Wingtip Toys.
7. Clique no link do **Google** à direita da página para fazer logon com uma conta de email do gmail.com existente.  
   Se você não tiver uma conta do gmail.com, poderá criar uma para fins de teste em [www.gmail.com](https://www.gmail.com/). Você também pode usar uma conta local padrão clicando em "registrar". 

    ![Check-out e pagamento com o PayPal – fazer logon](checkout-and-payment-with-paypal/_static/image21.png)
8. Entre com sua conta e senha do gmail. 

    ![Check-out e pagamento com a entrada do PayPal-gmail](checkout-and-payment-with-paypal/_static/image22.png)
9. Clique no botão **fazer logon** para registrar sua conta do Gmail com o nome de usuário do aplicativo de exemplo Wingtip Toys. 

    ![Check-out e pagamento com o PayPal – registrar conta](checkout-and-payment-with-paypal/_static/image23.png)
10. No site de teste do PayPal, adicione seu endereço de email do **comprador** e a senha que você criou anteriormente neste tutorial e, em seguida, clique no botão **fazer logon** . 

    ![Check-out e pagamento com a entrada do PayPal-PayPal](checkout-and-payment-with-paypal/_static/image24.png)
11. Concorde com a política do PayPal e clique no botão **concordo e continuar** .  
    Observe que essa página só é exibida na primeira vez que você usar essa conta do PayPal. Novamente, observe que essa é uma conta de teste, nenhum dinheiro real é trocado. 

    ![Check-out e pagamento com a política PayPal-PayPal](checkout-and-payment-with-paypal/_static/image25.png)
12. Examine as informações do pedido na página revisão do ambiente de teste do PayPal e clique em **continuar**. 

    ![Check-out e pagamento com o PayPal-informações de revisão](checkout-and-payment-with-paypal/_static/image26.png)
13. Na página *CheckoutReview. aspx* , verifique o valor da ordem e exiba o endereço de envio gerado. Em seguida, clique no botão **concluir ordem** . 

    ![Check-out e pagamento com o PayPal – revisão do pedido](checkout-and-payment-with-paypal/_static/image27.png)
14. A página **CheckoutComplete. aspx** é exibida com uma ID de transação de pagamento. 

    ![Check-out e pagamento com PayPal-check-out concluído](checkout-and-payment-with-paypal/_static/image28.png)

<a id="ReviewDBWebForms"></a>
## <a name="reviewing-the-database"></a>Revisando o banco de dados

Examinando os dados atualizados no banco de dado de aplicativo de exemplo Wingtip Toys depois de executar o aplicativo, você pode ver que o aplicativo registrou com êxito a compra dos produtos.

Você pode inspecionar os dados contidos no arquivo *wingtiptoys. MDF* usando a janela **Gerenciador de banco de dados** (**Gerenciador de servidores** janela no Visual Studio) como fazia anteriormente nesta série de tutoriais.

1. Feche a janela do navegador se ela ainda estiver aberta.
2. No Visual Studio, selecione o ícone **Mostrar todos os arquivos** na parte superior da **Gerenciador de soluções** para permitir que você expanda a pasta de **dados do aplicativo\_** .
3. Expanda a pasta de **dados do\_de aplicativos** .  
 Talvez seja necessário selecionar o ícone **Mostrar todos os arquivos** da pasta.
4. Clique com o botão direito do mouse no arquivo de banco de dados *wingtiptoys. MDF* e selecione **abrir**.  
    **Gerenciador de servidores** é exibido.
5. Expanda a pasta **tabelas** .
6. Clique com o botão direito do mouse na tabela **pedidos**e selecione **Mostrar dados da tabela**.  
 A tabela **Orders** é exibida.
7. Examine a coluna **PaymentTransactionID** para confirmar as transações bem-sucedidas. 

    ![Check-out e pagamento com o PayPal – examinar banco de dados](checkout-and-payment-with-paypal/_static/image29.png)
8. Feche a janela **Orders** Table.
9. Na Gerenciador de Servidores, clique com o botão direito do mouse na tabela **OrderDetails** e selecione **Mostrar dados da tabela**.
10. Examine os valores de `OrderId` e `Username` na tabela **OrderDetails** . Observe que esses valores correspondem aos valores `OrderId` e `Username` incluídos na tabela **Orders** .
11. Feche a janela de tabela **OrderDetails** .
12. Clique com o botão direito do mouse no arquivo de banco de dados Wingtip Toys (*wingtiptoys. MDF*) e selecione **fechar conexão**.
13. Se você não vir a janela **Gerenciador de soluções** , clique em **Gerenciador de soluções** na parte inferior da janela **Gerenciador de servidores** para mostrar a **Gerenciador de soluções** novamente.

## <a name="summary"></a>Resumo

Neste tutorial, você adicionou esquemas de detalhes de pedidos e pedidos para acompanhar a compra de produtos. Você também integrou a funcionalidade do PayPal ao aplicativo de exemplo Wingtip Toys.

## <a name="additional-resources"></a>Recursos adicionais

[Visão geral da configuração do ASP.NET](https://msdn.microsoft.com/library/ms178683(v=vs.100).aspx)  
[Implantar um aplicativo ASP.NET Web Forms seguro com associação, OAuth e banco de dados SQL no serviço Azure App](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Avaliação gratuita Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/)

## <a name="disclaimer"></a>Aviso de isenção de responsabilidade

Este tutorial contém código de exemplo. Esse código de exemplo é fornecido "no estado em que se encontra" sem nenhuma garantia de qualquer tipo. Da mesma forma, a Microsoft não garante a precisão, a integridade ou a qualidade do código de exemplo. Você concorda em usar o código de exemplo por sua conta e risco. Sob nenhuma circunstância, a Microsoft será responsável por qualquer tipo de código de exemplo, conteúdo, incluindo, mas não se limitando a, quaisquer erros ou omissões em qualquer código de exemplo, conteúdo ou qualquer perda ou dano de qualquer espécie incorrido como resultado do uso de qualquer código de exemplo. Você é notificado por aqui e, por aqui, concorda em indenizar, economizar e manter a Microsoft inofensiva de e contra qualquer e qualquer perda, reivindicações de perda, ferimentos ou danos de qualquer espécie, incluindo, sem limitação, aquelas que se deparam ou decorrentes do material que você postar, Transmita, use ou dependa de incluir, mas não se limitar a, as exibições expressas aqui.

> [!div class="step-by-step"]
> [Anterior](shopping-cart.md)
> [Próximo](membership-and-administration.md)
