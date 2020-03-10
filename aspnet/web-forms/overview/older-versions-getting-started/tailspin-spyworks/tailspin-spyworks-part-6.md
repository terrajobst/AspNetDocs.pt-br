---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 'Parte 6: Associação de ASP.NET | Microsoft Docs'
author: JoeStagner
description: Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo Tailspin Spyworks. A parte 6 adiciona associação ASP.NET.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: b0caa89dc9ffb5bb7451fa2d9d346c7db2bf1466
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78564179"
---
# <a name="part-6-aspnet-membership"></a>Parte 6: Associação do ASP.NET

por [Joe Stagner](https://github.com/JoeStagner)

> A Tailspin Spyworks demonstra como é extremamente simples criar aplicativos poderosos e escalonáveis para a plataforma .NET. Ele mostra como usar os ótimos novos recursos do ASP.NET 4 para criar uma loja online, incluindo compras, check-out e administração.
> 
> Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo Tailspin Spyworks. A parte 6 adiciona associação ASP.NET.

## <a id="_Toc260221672"></a>Trabalhando com associação ASP.NET

![](tailspin-spyworks-part-6/_static/image1.png)

Clique em segurança

![](tailspin-spyworks-part-6/_static/image1.jpg)

Certifique-se de que estamos usando a autenticação de formulários.

![](tailspin-spyworks-part-6/_static/image2.jpg)

Use o link "criar usuário" para criar alguns usuários.

![](tailspin-spyworks-part-6/_static/image3.jpg)

Quando terminar, consulte a janela de Gerenciador de Soluções e atualize a exibição.

![](tailspin-spyworks-part-6/_static/image2.png)

Observe que o ASPNETDB. A multa do MDF foi criada. Esse arquivo contém as tabelas para dar suporte aos serviços principais do ASP.NET, como associação.

Agora, podemos começar a implementar o processo de check-out.

Comece criando uma página CheckOut. aspx.

A página CheckOut. aspx só deve estar disponível para os usuários que estão conectados, portanto, restringiremos o acesso a usuários conectados e redirecionarão os usuários que não estão conectados à página de logon.

Para fazer isso, adicionaremos o seguinte à seção de configuração do nosso arquivo Web. config.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

O modelo para ASP.NET Web Forms aplicativos adicionou automaticamente uma seção de autenticação ao nosso arquivo Web. config e estabeleceu a página de logon padrão.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

É necessário modificar o arquivo. aspx de login do. ini para migrar um carrinho de compras anônimo quando o usuário fizer logon. Altere a página\_evento de carregamento da seguinte maneira.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

Em seguida, adicione um manipulador de eventos "LoggedInTemplate" como este para definir o nome da sessão para o usuário conectado recentemente e altere a ID da sessão temporária no carrinho de compras para o do usuário chamando o método MigrateCart em nossa classe MyShoppingCart. (Implementado no arquivo. cs)

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

Implemente o método MigrateCart () como este.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

Em checkout. aspx, usaremos uma EntityDataSource e um GridView em nossa página de check-out como fizemos em nossa página de carrinho de compras.

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

Observe que nosso controle GridView especifica um manipulador de eventos "onligação" chamado myList\_de multiligação, portanto, vamos implementar esse manipulador de eventos como este.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

Esse método mantém um total acumulado do carrinho de compras, pois cada linha é associada e atualiza a linha inferior do GridView.

Neste estágio, implementamos uma apresentação de "revisão" do pedido a ser colocado.

Vamos manipular um cenário de carrinho vazio adicionando algumas linhas de código à nossa página\_evento de carregamento:

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

Quando o usuário clicar no botão "enviar", executaremos o seguinte código no manipulador de eventos de clique no botão enviar.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

A "carne" do processo de envio de pedidos deve ser implementada no método SubmitOrder () de nossa classe MyShoppingCart.

SubmitOrder irá:

- Pegue todos os itens de linha no carrinho de compras e use-os para criar um novo registro de pedido e os registros de OrderDetails associados.
- Calcular a data de envio.
- Desmarque o carrinho de compras.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

Para os fins deste aplicativo de exemplo, calcularemos uma data de remessa simplesmente adicionando dois dias à data atual.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

A execução do aplicativo agora nos permitirá testar o processo de compra do início ao fim.

> [!div class="step-by-step"]
> [Anterior](tailspin-spyworks-part-5.md)
> [Próximo](tailspin-spyworks-part-7.md)
