---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: 'Parte 9: registro e check-out | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo de loja de música MVC do ASP.NET. A parte 9 aborda o registro e o check-out.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: 040bc0ccef889fb9a7c3d9b5ce88c75b7b754248
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559531"
---
# <a name="part-9-registration-and-checkout"></a>Parte 9: registro e check-out

por [Jon Galloway](https://github.com/jongalloway)

> A loja de música MVC é um aplicativo de tutorial que apresenta e explica passo a passo como usar o ASP.NET MVC e o Visual Studio para desenvolvimento para a Web.  
>   
> A loja de música MVC é uma implementação de armazenamento de exemplo leve que vende os álbuns de música online e implementa a administração básica de site, a entrada do usuário e a funcionalidade do carrinho de compras.  
>   
> Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo de loja de música MVC do ASP.NET. A parte 9 aborda o registro e o check-out.

Nesta seção, criaremos um CheckoutController que coletará as informações de endereço e pagamento do comprador. Exigiremos que os usuários se registrem em nosso site antes de fazer o check-out, portanto, esse controlador exigirá autorização.

Os usuários navegarão até o processo de check-out de seu carrinho de compras clicando no botão "check-out".

![](mvc-music-store-part-9/_static/image1.jpg)

Se o usuário não estiver conectado, ele será solicitado a fazê-lo.

![](mvc-music-store-part-9/_static/image1.png)

Após o logon bem-sucedido, o usuário mostra a exibição de endereço e de pagamento.

![](mvc-music-store-part-9/_static/image2.png)

Depois de preencher o formulário e enviar o pedido, eles serão mostrados na tela de confirmação do pedido.

![](mvc-music-store-part-9/_static/image3.png)

A tentativa de exibir uma ordem inexistente ou uma ordem que não pertence a você mostrará o modo de exibição de erro.

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a>Migrando o carrinho de compras

Embora o processo de compra seja anônimo, quando o usuário clica no botão de check-out, ele será solicitado a se registrar e fazer logon. Os usuários esperam que manteremos suas informações de carrinho de compras entre visitas, portanto, precisaremos associar as informações do carrinho de compras a um usuário quando ele concluir o registro ou o logon.

Na verdade, isso é muito simples, pois nossa classe ShoppingCart já tem um método que associará todos os itens do carrinho atual a um nome de usuário. Só precisaremos chamar esse método quando um usuário concluir o registro ou o logon.

Abra a classe **AccountController** que adicionamos ao configurar a associação e a autorização. Adicione uma instrução using referenciando MvcMusicStore. Models e, em seguida, adicione o seguinte método MigrateShoppingCart:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

Em seguida, modifique a ação de postagem de LogOn para chamar MigrateShoppingCart depois que o usuário tiver sido validado, conforme mostrado abaixo:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

Faça a mesma alteração na ação de postagem de registro imediatamente após a criação bem-sucedida da conta de usuário:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

É isso – agora um carrinho de compras anônimo será transferido automaticamente para uma conta de usuário após o registro ou logon bem-sucedido.

## <a name="creating-the-checkoutcontroller"></a>Criando o CheckoutController

Clique com o botão direito do mouse na pasta controladores e adicione um novo controlador ao projeto chamado CheckoutController usando o modelo de controlador vazio.

![](mvc-music-store-part-9/_static/image5.png)

Primeiro, adicione o atributo Authorize acima da declaração da classe Controller para exigir que os usuários se registrem antes do check-out:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

*Observação: isso é semelhante à alteração que fizemos anteriormente no StoreManagerController, mas, nesse caso, o atributo Authorize exigia que o usuário esteja em uma função de administrador. No controlador de check-out, estamos exigindo que o usuário esteja conectado, mas não exigindo que eles sejam administradores.*

Para simplificar, não lidaremos com as informações de pagamento neste tutorial. Em vez disso, estamos permitindo que os usuários confiram usando um código promocional. Armazenaremos esse código promocional usando uma constante chamada código promocional.

Como no StoreController, vamos declarar um campo para manter uma instância da classe MusicStoreEntities, chamada storeDB. Para fazer uso da classe MusicStoreEntities, precisaremos adicionar uma instrução using para o namespace MvcMusicStore. Models. A parte superior do nosso controlador de check-out aparece abaixo.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

O CheckoutController terá as seguintes ações do controlador:

**AddressAndPayment (Get Method)** exibirá um formulário para permitir que o usuário insira suas informações.

**AddressAndPayment (método post)** validará a entrada e processará a ordem.

**Concluir** será mostrado depois que um usuário tiver concluído com êxito o processo de check-out. Essa exibição incluirá o número do pedido do usuário, como confirmação.

Primeiro, vamos renomear a ação do controlador de índice (que foi gerado quando criamos o controlador) para AddressAndPayment. Essa ação do controlador apenas exibe o formulário de check-out, portanto, não requer nenhuma informação de modelo.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

Nosso método POST AddressAndPayment seguirá o mesmo padrão que usamos no StoreManagerController: ele tentará aceitar o envio do formulário e concluirá o pedido e exibirá novamente o formulário se ele falhar.

Depois de validar que a entrada do formulário atende aos nossos requisitos de validação para um pedido, verificaremos o valor do formulário código promocional diretamente. Supondo que tudo esteja correto, salvaremos as informações atualizadas com o pedido, informaremos ao objeto ShoppingCart para concluir o processo do pedido e redirecionarei para a ação completa.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

Após a conclusão bem-sucedida do processo de check-out, os usuários serão redirecionados para a ação completa do controlador. Essa ação executará uma verificação simples para validar que o pedido realmente pertence ao usuário conectado antes de mostrar o número do pedido como uma confirmação.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

*Observação: o modo de exibição de erro foi criado automaticamente para nós na pasta/Views/Shared quando começamos o projeto.*

O código completo do CheckoutController é o seguinte:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a>Adicionando a exibição AddressAndPayment

Agora, vamos criar a exibição AddressAndPayment. Clique com o botão direito do mouse em uma das ações do controlador AddressAndPayment e adicione uma exibição chamada AddressAndPayment, que é fortemente tipada como uma ordem, e usa o modelo de edição, conforme mostrado abaixo.

![](mvc-music-store-part-9/_static/image6.png)

Essa exibição fará uso de duas das técnicas que vimos ao criar a exibição StoreManagerEdit:

- Usaremos HTML. EditorForModel () para exibir campos de formulário para o modelo de pedido
- Aproveitaremos as regras de validação usando uma classe Order com atributos de validação

Começaremos atualizando o código do formulário para usar HTML. EditorForModel (), seguido por uma caixa de texto adicional para o código promocional. O código completo da exibição AddressAndPayment é mostrado abaixo.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a>Definindo regras de validação para o pedido

Agora que a nossa exibição está configurada, configuraremos as regras de validação para nosso modelo de pedido como fizemos anteriormente para o modelo de álbum. Clique com o botão direito do mouse na pasta modelos e adicione uma classe chamada Order. Além dos atributos de validação que usamos anteriormente para o álbum, também usaremos uma expressão regular para validar o endereço de email do usuário.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

A tentativa de enviar o formulário com informações ausentes ou inválidas agora mostrará a mensagem de erro usando a validação do lado do cliente.

![](mvc-music-store-part-9/_static/image7.png)

Ok, fizemos a maior parte do trabalho árduo para o processo de check-out; Temos apenas algumas chances e terminamos para terminar. Precisamos adicionar dois modos de exibição simples e precisamos cuidar da entrega das informações do carrinho durante o processo de logon.

## <a name="adding-the-checkout-complete-view"></a>Adicionando a exibição de check-out completo

A exibição de check-out completo é muito simples, pois ela apenas precisa exibir a ID do pedido. Clique com o botão direito do mouse na ação concluir controlador e adicione uma exibição chamada completa, que é fortemente tipada como um int.

![](mvc-music-store-part-9/_static/image8.png)

Agora, atualizaremos o código de exibição para exibir a ID do pedido, conforme mostrado abaixo.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a>Atualizando a exibição de erro

O modelo padrão inclui uma exibição de erro na pasta exibições compartilhadas para que possa ser usado novamente em outro lugar no site. Esta exibição de erro contém um erro muito simples e não usa nosso layout de site, portanto, nós o atualizaremos.

Como essa é uma página de erro genérica, o conteúdo é muito simples. Incluiremos uma mensagem e um link para navegar até a página anterior no histórico se o usuário quiser repetir a ação.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]

> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-8.md)
> [Próximo](mvc-music-store-part-10.md)
