---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: 'Parte 4: adicionando uma exibição de administrador | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: 664aeb33031e933322886a6d6bdd989277e9fda2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556010"
---
# <a name="part-4-adding-an-admin-view"></a>Parte 4: adicionando uma exibição de administrador

por [Mike Wasson](https://github.com/MikeWasson)

[Baixar projeto concluído](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a>Adicionar uma exibição de administrador

Agora, vamos voltar ao lado do cliente e adicionar uma página que pode consumir dados do controlador de administração. A página permitirá que os usuários criem, editem ou excluam produtos, enviando solicitações AJAX para o controlador.

Em Gerenciador de Soluções, expanda a pasta controladores e abra o arquivo chamado HomeController.cs. Esse arquivo contém um controlador MVC. Adicione um método chamado `Admin`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

O método **HttpRouteUrl** cria o URI para a API da Web e armazenamos isso no recipiente de exibição para mais tarde.

Em seguida, posicione o cursor de texto dentro do método de ação `Admin`, clique com o botão direito do mouse e selecione **Adicionar exibição**. Isso abrirá a caixa de diálogo **Adicionar exibição** .

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

Na caixa de diálogo **Adicionar exibição** , nomeie o modo de exibição "admin". Marque a caixa de seleção rotulada **criar uma exibição fortemente tipada**. Em **classe de modelo**, selecione "produto (ProductStore. Models)". Deixe todas as outras opções como seus valores padrão.

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

Clicar em **Adicionar** adiciona um arquivo chamado admin. cshtml em exibições/início. Abra este arquivo e adicione o seguinte HTML. Esse HTML define a estrutura da página, mas nenhuma funcionalidade está conectada ainda.

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a>Criar um link para a página de administração

Em Gerenciador de Soluções, expanda a pasta exibições e expanda a pasta compartilhada. Abra o arquivo chamado \_layout. cshtml. Localize o elemento **UL** com ID = "menu" e um link de ação para a exibição de administrador:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> No projeto de exemplo, fiz algumas outras alterações superficiais, como substituir a cadeia de caracteres "seu logotipo aqui". Eles não afetam a funcionalidade do aplicativo. Você pode baixar o projeto e comparar os arquivos.

Execute o aplicativo e clique no link "admin" que aparece na parte superior do home page. A página de administração deve ser parecida com a seguinte:

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

No momento, a página não faz nada. Na próxima seção, usaremos knockout. js para criar uma interface do usuário dinâmica.

## <a name="add-authorization"></a>Adicionar autorização

A página de administração está acessível no momento para qualquer pessoa que visitar o site. Vamos alterar isso para restringir a permissão aos administradores.

Comece adicionando uma função de "administrador" e um usuário administrador. Em Gerenciador de Soluções, expanda a pasta filtros e abra o arquivo chamado InitializeSimpleMembershipAttribute.cs. Localize o construtor de `SimpleMembershipInitializer`. Após a chamada para **websecurity. InitializeDatabaseConnection**, adicione o seguinte código:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

Essa é uma maneira rápida e suja de adicionar a função de "administrador" e criar um usuário para a função.

Em Gerenciador de Soluções, expanda a pasta controladores e abra o arquivo HomeController.cs. Adicione o atributo **Authorize** ao método `Admin`.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

Abra o arquivo AdminController.cs e adicione o atributo **autorizar** a toda a classe de `AdminController`.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> O MVC e a API da Web definem **autorizar** atributos, em namespaces diferentes. O MVC usa **System. Web. Mvc. authorattribute**, enquanto a API Web usa **System. Web. http. authorattribute**.

Agora, somente os administradores podem exibir a página de administração. Além disso, se você enviar uma solicitação HTTP para o controlador de administração, a solicitação deverá conter um cookie de autenticação. Caso contrário, o servidor enviará uma resposta HTTP 401 (não autorizado). Você pode ver isso no Fiddler enviando uma solicitação GET para `http://localhost:*port*/api/admin`.

> [!div class="step-by-step"]
> [Anterior](using-web-api-with-entity-framework-part-3.md)
> [Próximo](using-web-api-with-entity-framework-part-5.md)
