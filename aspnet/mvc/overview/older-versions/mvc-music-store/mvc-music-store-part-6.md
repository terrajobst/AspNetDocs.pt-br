---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: 'Parte 6: usando as anotações de dados para a validação do modelo | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo de loja de música MVC do ASP.NET. A parte 6 aborda o uso de anotações de dados para o modelo V...
ms.author: riande
ms.date: 04/21/2011
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: bc031dd5be61cc6707c522f85f6af77a420c8b31
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539273"
---
# <a name="part-6-using-data-annotations-for-model-validation"></a>Parte 6: usando as anotações de dados para a validação do modelo

por [Jon Galloway](https://github.com/jongalloway)

> A loja de música MVC é um aplicativo de tutorial que apresenta e explica passo a passo como usar o ASP.NET MVC e o Visual Studio para desenvolvimento para a Web.  
>   
> A loja de música MVC é uma implementação de armazenamento de exemplo leve que vende os álbuns de música online e implementa a administração básica de site, a entrada do usuário e a funcionalidade do carrinho de compras.  
>   
> Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo de loja de música MVC do ASP.NET. A parte 6 aborda o uso de anotações de dados para a validação do modelo.

Temos um grande problema com nossos formulários de criação e edição: eles não estão fazendo nenhuma validação. Podemos fazer coisas como deixar os campos obrigatórios em branco ou digitar letras no campo preço, e o primeiro erro que veremos é do banco de dados.

Podemos adicionar facilmente a validação ao nosso aplicativo Adicionando anotações de dados às nossas classes de modelo. As anotações de dados nos permitem descrever as regras que queremos aplicar às propriedades do modelo, e o ASP.NET MVC cuidará da imposição delas e da exibição das mensagens apropriadas aos nossos usuários.

## <a name="adding-validation-to-our-album-forms"></a>Adicionando validação aos nossos formulários de álbum

Usaremos os seguintes atributos de anotação de dados:

- **Obrigatório** – indica que a propriedade é um campo obrigatório
- **DisplayName** – define o texto que queremos usar em campos de formulário e mensagens de validação
- **StringLength** – define um comprimento máximo para um campo de cadeia de caracteres
- **Range** – fornece um valor máximo e mínimo para um campo numérico
- **BIND** – lista os campos a serem excluídos ou incluídos ao associar valores de parâmetro ou formulário a propriedades de modelo
- **ScaffoldColumn** – permite ocultar campos de formulários do editor

*Observação: para obter mais informações sobre a validação de modelo usando atributos de anotação de dados, consulte a documentação do MSDN em* [`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)

Abra a classe album e adicione as instruções *using* a seguir à parte superior.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

Em seguida, atualize as propriedades para adicionar os atributos de exibição e validação, conforme mostrado abaixo.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

Enquanto estamos lá, também alteramos o gênero e o artista para propriedades virtuais. Isso permite que Entity Framework carregá-los, conforme necessário.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

Depois de ter adicionado esses atributos ao nosso modelo de álbum, nossa tela criar e editar imediatamente começa a validar campos e usando os nomes de exibição que escolhemos (por exemplo, a URL de arte do álbum em vez de AlbumArtUrl). Execute o aplicativo e navegue até/StoreManager/Create.

![](mvc-music-store-part-6/_static/image1.png)

Em seguida, vamos interromper algumas regras de validação. Insira um preço de 0 e deixe o título em branco. Quando clicamos no botão criar, veremos o formulário exibido com mensagens de erro de validação mostrando quais campos não atenderam às regras de validação que definimos.

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a>Testando a validação do lado do cliente

A validação no lado do servidor é muito importante da perspectiva do aplicativo, pois os usuários podem burlar a validação do lado do cliente. No entanto, os formulários da página da Web que implementam apenas a validação no lado do servidor exibem três problemas significativos.

1. O usuário deve aguardar até que o formulário seja Postado, validado no servidor e para que a resposta seja enviada ao navegador.
2. O usuário não obtém comentários imediatos ao corrigir um campo para que agora passe as regras de validação.
3. Estamos desperdiçando recursos de servidor para executar a lógica de validação em vez de aproveitar o navegador do usuário.

Felizmente, os modelos do ASP.NET MVC 3 Scaffold têm validação do lado do cliente interna, não exigindo nenhum trabalho adicional.

A digitação de uma única letra no campo título atende aos requisitos de validação, portanto, a mensagem de validação é removida imediatamente.

![](mvc-music-store-part-6/_static/image3.png)

> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-5.md)
> [Próximo](mvc-music-store-part-7.md)
