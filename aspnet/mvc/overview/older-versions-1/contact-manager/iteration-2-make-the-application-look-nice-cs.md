---
uid: mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
title: '#2 de iteração – faça com que oC#aplicativo pareça interessante () | Microsoft Docs'
author: microsoft
description: Nessa iteração, melhoramos a aparência do aplicativo modificando a página mestra de exibição do ASP.NET MVC padrão e a folha de estilos em cascata.
ms.author: riande
ms.date: 02/20/2009
ms.assetid: f1173feb-11ee-4017-8f3f-86599ea6ae13
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
msc.type: authoredcontent
ms.openlocfilehash: 246cb4b4668339cc4b7e4e03ea005102c6a2a5c3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78602014"
---
# <a name="iteration-2--make-the-application-look-nice-c"></a>#2 de iteração – faça com que oC#aplicativo fique bom ()

pela [Microsoft](https://github.com/microsoft)

[Código de download](iteration-2-make-the-application-look-nice-cs/_static/contactmanager_2_cs1.zip)

> Nessa iteração, melhoramos a aparência do aplicativo modificando a página mestra de exibição do ASP.NET MVC padrão e a folha de estilos em cascata.

## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Criando um aplicativo MVC de gerenciamento de contatosC#ASP.net ()

Nesta série de tutoriais, criamos um aplicativo de gerenciamento de contatos inteiro do início ao fim. O aplicativo Contact Manager permite armazenar informações de contato-nomes, números de telefone e endereços de email – para obter uma lista de pessoas.

Criamos o aplicativo em várias iterações. Com cada iteração, aprimoramos gradualmente o aplicativo. O objetivo dessa abordagem de várias iterações é permitir que você entenda o motivo de cada alteração.

- #1 de iteração – crie o aplicativo. Na primeira iteração, criamos o gerente de contatos da maneira mais simples possível. Adicionamos suporte para operações básicas de banco de dados: criar, ler, atualizar e excluir (CRUD).

- #2 de iteração – faça com que o aplicativo fique bom. Nessa iteração, melhoramos a aparência do aplicativo modificando a página mestra de exibição do ASP.NET MVC padrão e a folha de estilos em cascata.

- #3 de iteração – adicionar validação de formulário. Na terceira iteração, adicionamos a validação básica de formulário. Impedimos que as pessoas enviem um formulário sem concluir os campos de formulário necessários. Também validamos endereços de email e números de telefone.

- #4 de iteração-torne o aplicativo levemente acoplado. Nesta quarta iteração, aproveitamos os vários padrões de design de software para facilitar a manutenção e a modificação do aplicativo Contact Manager. Por exemplo, podemos refatorar nosso aplicativo para usar o padrão de repositório e o padrão de injeção de dependência.

- #5 de iteração – criar testes de unidade. Na quinta iteração, tornamos o nosso aplicativo mais fácil de manter e modificar adicionando testes de unidade. Simulamos nossas classes de modelo de dados e criamos testes de unidade para nossos controladores e lógica de validação.

- #6 de iteração – use o desenvolvimento controlado por testes. Na sexta-iteração, adicionamos nova funcionalidade ao nosso aplicativo escrevendo testes de unidade primeiro e escrevendo código em relação aos testes de unidade. Nessa iteração, adicionamos grupos de contatos.

- #7 de iteração – adicione funcionalidade Ajax. Na sétima iteração, melhoramos a capacidade de resposta e o desempenho do nosso aplicativo adicionando suporte para AJAX.

## <a name="this-iteration"></a>Esta iteração

O objetivo dessa iteração é melhorar a aparência do aplicativo Contact Manager. Atualmente, o gerente de contato usa a página mestra de exibição do ASP.NET MVC padrão e a folha de estilos em cascata (consulte a Figura 1). Eles não parecem ruins, mas eu não quero que o gerente de contatos seja semelhante a todos os outros sites do ASP.NET MVC. Quero substituir esses arquivos por arquivos personalizados.

[![caixa de diálogo novo projeto](iteration-2-make-the-application-look-nice-cs/_static/image1.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image1.png)

**Figura 01**: a aparência padrão de um aplicativo MVC ASP.net ([clique para exibir a imagem em tamanho normal](iteration-2-make-the-application-look-nice-cs/_static/image2.png))

Nessa iteração, abordo duas abordagens para melhorar o Design Visual de nosso aplicativo. Primeiro, mostrarei como tirar proveito da Galeria de design do ASP.NET MVC para baixar um modelo de design do ASP.NET MVC gratuito. A Galeria de design do ASP.NET MVC permite que você crie um aplicativo Web profissional sem fazer nenhum trabalho.

Decidi não usar um modelo da Galeria de design do ASP.NET MVC para o aplicativo Contact Manager. Em vez disso, eu tinha um design personalizado criado por uma empresa de design profissional. Na segunda parte deste tutorial, explicarei como eu trabalhei com uma empresa de design profissional para criar o design final ASP.NET MVC.

## <a name="the-aspnet-mvc-design-gallery"></a>A Galeria de design do ASP.NET MVC

A Galeria de design do ASP.NET MVC é um recurso gratuito fornecido pela Microsoft. A Galeria MVC do ASP.NET está localizada no seguinte endereço:

[https://www.asp.net/mvc/gallery](https://www.asp.net/mvc/gallery)

A Galeria de design MVC do ASP.NET hospeda uma coleção de designs de site gratuitos que foram criados especificamente para uso em um projeto MVC ASP.NET. Os designs são carregados por membros da Comunidade. Os visitantes da Galeria podem votar em seus designs favoritos (veja a Figura 2).

[![caixa de diálogo novo projeto](iteration-2-make-the-application-look-nice-cs/_static/image2.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image3.png)

**Figura 02**: a Galeria de Design do ASP.NET MVC ([clique para exibir a imagem em tamanho normal](iteration-2-make-the-application-look-nice-cs/_static/image4.png))

À medida que escrevo este tutorial, o design mais popular da galeria é um design chamado de outubro por David Hauser. Você pode usar esse design para um projeto MVC do ASP.NET concluindo as seguintes etapas:

1. Clique no botão **baixar** para baixar o arquivo. zip de outubro para o seu computador.
2. Clique com o botão direito do mouse no arquivo de outubro. zip baixado e clique no botão **desbloquear** (veja a Figura 3).
3. Descompacte o arquivo em uma pasta chamada outubro.
4. Selecione todos os arquivos da pasta Designtemplate contida na pasta outubro, clique com o botão direito do mouse nos arquivos e selecione a opção de menu **copiar**.
5. Clique com o botão direito do mouse no nó do projeto ContactManager na janela Gerenciador de Soluções do Visual Studio e selecione a opção de menu **colar** (veja a Figura 4).
6. Selecione a opção de menu do Visual Studio **Editar, localizar e substituir, substituição rápida** e substituir *[MyProjectName]* por *ContactManager* (consulte a Figura 5).

[![caixa de diálogo novo projeto](iteration-2-make-the-application-look-nice-cs/_static/image3.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image5.png)

**Figura 03**: desbloqueio de um arquivo baixado da Web ([clique para exibir a imagem em tamanho normal](iteration-2-make-the-application-look-nice-cs/_static/image6.png))

[![caixa de diálogo novo projeto](iteration-2-make-the-application-look-nice-cs/_static/image4.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image7.png)

**Figura 04**: substituindo arquivos no Gerenciador de soluções ([clique para exibir a imagem em tamanho normal](iteration-2-make-the-application-look-nice-cs/_static/image8.png))

[![caixa de diálogo novo projeto](iteration-2-make-the-application-look-nice-cs/_static/image5.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image9.png)

**Figura 05**: substituindo [ProjectName] por ContactManager ([clique para exibir a imagem em tamanho normal](iteration-2-make-the-application-look-nice-cs/_static/image10.png))

Depois de concluir essas etapas, seu aplicativo Web usará o novo design. A página da Figura 6 ilustra a aparência do aplicativo Contact Manager com o design de outubro.

[![caixa de diálogo novo projeto](iteration-2-make-the-application-look-nice-cs/_static/image6.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image11.png)

**Figura 06**: ContactManager com o modelo de outubro ([clique para exibir a imagem em tamanho normal](iteration-2-make-the-application-look-nice-cs/_static/image12.png))

## <a name="creating-a-custom-aspnet-mvc-design"></a>Criando um design MVC personalizado do ASP.NET

A Galeria de design MVC do ASP.NET tem uma boa seleção de diferentes estilos de design. A Galeria fornece uma maneira simples de personalizar a aparência dos seus aplicativos MVC ASP.NET. E, é claro, a galeria tem a grande vantagem de ser totalmente livre.

No entanto, talvez seja necessário criar um design completamente exclusivo para seu site. Nesse caso, faz sentido trabalhar com uma empresa de design de site. Decidi usar essa abordagem para o design do aplicativo Contact Manager.

Eu compactei o Gerenciador de contatos de iteração #1 e enviei o projeto para a empresa de design. Eles não possuíam o Visual Studio (pena neles!), mas isso não apresentava um problema. Eles conseguiram baixar o Microsoft Visual Web Developer gratuitamente do site [https://www.asp.net](https://www.asp.net) e abrir o aplicativo Contact Manager no Visual Web Developer. Em alguns dias, eles produziram o design na Figura 7.

[![caixa de diálogo novo projeto](iteration-2-make-the-application-look-nice-cs/_static/image7.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image13.png)

**Figura 07**: design do Gerenciador de contatos MVC do ASP.net ([clique para exibir a imagem em tamanho normal](iteration-2-make-the-application-look-nice-cs/_static/image14.png))

O novo design consistiu em dois arquivos principais: um novo arquivo de folha de estilos em cascata e um novo arquivo de página mestra de exibição. Uma página mestra de exibição contém o layout e o conteúdo compartilhado para exibições em um aplicativo MVC ASP.NET. Por exemplo, a página de exibição mestra inclui o cabeçalho, as guias de navegação e o rodapé que aparecem na Figura 7. Escrevi a página mestra do site. Master View existente na pasta Views\Shared com o novo arquivo site. Master da empresa de design,

A empresa de design também criou uma nova folha de estilos em cascata e um conjunto de imagens. Coloquei esses novos arquivos na pasta Content e escrevi o arquivo site. css existente. Você deve posicionar todo o conteúdo estático na pasta de conteúdo.

Observe que o novo design do Gerenciador de contatos inclui imagens para edição e exclusão de contatos. Uma imagem de editar e excluir aparece ao lado de cada contato na tabela HTML de contatos.

Originalmente, esses links eram renderizados com o HTML. O ActionLink () auxiliar como este:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample1.aspx)]

O método html. ActionLink () não oferece suporte a imagens (o método HTML codifica o texto do link por motivos de segurança). Portanto, substituí as chamadas em HTML. ActionLink () por chamadas para URL. Action () como esta:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample2.aspx)]

O método html. ActionLink () renderiza um hiperlink HTML inteiro. O método URL. Action (), por outro lado, renderiza apenas a URL sem a &lt;uma marca&gt;.

Observe, além disso, que o novo design inclui guias selecionadas e não selecionadas. Por exemplo, na Figura 8, a guia **criar novo contato** é selecionada e a guia **meus contatos** não está selecionada.

[![caixa de diálogo novo projeto](iteration-2-make-the-application-look-nice-cs/_static/image8.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image15.png)

**Figura 08**: guias selecionadas e não selecionadas ([clique para exibir a imagem em tamanho normal](iteration-2-make-the-application-look-nice-cs/_static/image16.png))

Para dar suporte à renderização de guias selecionadas e não selecionadas, criei um auxiliar HTML personalizado chamado MenuItemHelper. Esse método auxiliar renderiza uma marca &lt;li&gt; ou uma marca &lt;li Class = "Selected"&gt; dependendo se o controlador atual e a ação correspondem ao controlador e ao nome da ação passados para o auxiliar. O código para o MenuItemHelper está contido na Listagem 1.

**Listagem 1-Helpers\MenuItemHelper.cs**

[!code-csharp[Main](iteration-2-make-the-application-look-nice-cs/samples/sample3.cs)]

O MenuItemHelper usa a classe TagBuilder internamente para criar a marca &lt;li&gt; HTML. A classe TagBuilder é uma classe de utilitário muito útil que você pode usar sempre que precisar criar uma nova marca HTML. Ele inclui métodos para adicionar atributos, adicionar classes CSS, gerar IDs e modificar o HTML interno da marca s.

## <a name="summary"></a>Resumo

Nessa iteração, melhoramos o design visual do nosso aplicativo MVC ASP.NET. Primeiro, você foi apresentado à galeria de design do ASP.NET MVC. Você aprendeu a baixar modelos de design gratuitos da Galeria de design do ASP.NET MVC que pode usar em seus aplicativos MVC do ASP.NET.

Em seguida, discutimos como você pode criar um design personalizado modificando o arquivo de folha de estilos em cascata padrão e o arquivo de página de exibição mestre. Para dar suporte ao novo design, tivemos que fazer algumas alterações secundárias em nosso aplicativo Contact Manager. Por exemplo, adicionamos um novo auxiliar HTML chamado MenuItemHelper que exibe guias selecionadas e não selecionadas.

Na próxima iteração, resolvemos o assunto muito importante da validação. Adicionamos o código de validação ao nosso aplicativo para que um usuário não possa criar um novo contato sem fornecer os valores necessários, como o nome e sobrenome de uma pessoa.

> [!div class="step-by-step"]
> [Anterior](iteration-1-create-the-application-cs.md)
> [Próximo](iteration-3-add-form-validation-cs.md)
