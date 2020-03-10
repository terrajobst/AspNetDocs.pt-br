---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: 'Parte 7: Associação e autorização | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo de loja de música MVC do ASP.NET. A parte 7 aborda associação e autorização.
ms.author: riande
ms.date: 10/13/2010
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: a6a1a936e0ea29ea36721ba78f35845401f74c01
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539196"
---
# <a name="part-7-membership-and-authorization"></a>Parte 7: Associação e autorização

por [Jon Galloway](https://github.com/jongalloway)

> A loja de música MVC é um aplicativo de tutorial que apresenta e explica passo a passo como usar o ASP.NET MVC e o Visual Studio para desenvolvimento para a Web.  
>   
> A loja de música MVC é uma implementação de armazenamento de exemplo leve que vende os álbuns de música online e implementa a administração básica de site, a entrada do usuário e a funcionalidade do carrinho de compras.  
>   
> Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo de loja de música MVC do ASP.NET. A parte 7 aborda associação e autorização.

Nosso controlador Store Manager está acessível no momento para qualquer pessoa que visitar nosso site. Vamos alterar isso para restringir a permissão aos administradores do site.

## <a name="adding-the-accountcontroller-and-views"></a>Adicionando AccountController e exibições

Uma diferença entre o modelo completo de aplicativo Web do ASP.NET MVC 3 e o modelo de aplicativo Web do ASP.NET MVC 3 vazio é que o modelo vazio não inclui um controlador de conta. Adicionaremos um controlador de conta copiando alguns arquivos de um novo aplicativo MVC ASP.NET criado a partir do modelo de aplicativo Web completo do ASP.NET MVC 3.

Crie um novo aplicativo MVC do ASP.NET usando o modelo completo de aplicativo Web do ASP.NET MVC 3 e copie os seguintes arquivos para os mesmos diretórios em nosso projeto:

1. Copiar AccountController.cs no diretório de controladores
2. Copiar AccountModels no diretório de modelos
3. Crie um diretório de conta dentro do diretório views e copie todas as quatro exibições em

Altere o namespace para as classes de controlador e modelo para que comecem com MvcMusicStore. A classe AccountController deve usar o namespace MvcMusicStore. Controllers e a classe AccountModels deve usar o namespace MvcMusicStore. Models.

*Observação: esses arquivos também estão disponíveis no download MvcMusicStore-Assets. zip do qual copiamos nossos arquivos de design de site no início do tutorial. Os arquivos de associação estão localizados no diretório de código.*

A solução atualizada deve ser parecida com a seguinte:

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a>Adicionar um usuário administrativo com o site de configuração do ASP.NET

Antes de exigirmos autorização em nosso site, precisaremos criar um usuário com acesso. A maneira mais fácil de criar um usuário é usar o site interno de configuração do ASP.NET.

Inicie o site de configuração do ASP.NET clicando em seguir o ícone na Gerenciador de Soluções.

![](mvc-music-store-part-7/_static/image2.png)

Isso inicia um site de configuração. Clique na guia Segurança na tela inicial e, em seguida, clique no link "Habilitar funções" no centro da tela.

![](mvc-music-store-part-7/_static/image3.png)

Clique no link "criar ou gerenciar funções".

![](mvc-music-store-part-7/_static/image4.png)

Insira "administrador" como o nome da função e pressione o botão Adicionar função.

![](mvc-music-store-part-7/_static/image5.png)

Clique no botão voltar e, em seguida, clique no link criar usuário no lado esquerdo.

![](mvc-music-store-part-7/_static/image6.png)

Preencha os campos de informações do usuário à esquerda usando as seguintes informações:

| **Campo** | **Valor** |
| --- | --- |
| **Nome de usuário** | Administrador |
| **Senha** | password123! |
| **Confirmar Senha** | password123! |
| **Email** | (qualquer endereço de email funcionará) |
| **Pergunta de Segurança** | (o que você desejar) |
| **Resposta de Segurança** | (o que você desejar) |

*Observação: é claro que você pode usar qualquer senha que desejar. As configurações de segurança de senha padrão exigem uma senha com até 7 caracteres e contém um caractere não alfanumérico.*

Selecione a função Administrador para esse usuário e clique no botão criar usuário.

![](mvc-music-store-part-7/_static/image7.png)

Neste ponto, você deverá ver uma mensagem indicando que o usuário foi criado com êxito.

![](mvc-music-store-part-7/_static/image8.png)

Agora você pode fechar a janela do navegador.

## <a name="role-based-authorization"></a>Autorização baseada em função

Agora, podemos restringir o acesso ao StoreManagerController usando o atributo [Authorize], especificando que o usuário deve estar na função Administrador para acessar qualquer ação do controlador na classe.

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

*Observação: o atributo [autorizar] pode ser colocado em métodos de ação específicos, bem como no nível de classe do controlador.*

Agora, navegar até/StoreManager abre uma caixa de diálogo de logon:

![](mvc-music-store-part-7/_static/image9.png)

Depois de fazer logon com nossa nova conta de administrador, podemos acessar a tela de edição do álbum como antes.

> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-6.md)
> [Próximo](mvc-music-store-part-8.md)
