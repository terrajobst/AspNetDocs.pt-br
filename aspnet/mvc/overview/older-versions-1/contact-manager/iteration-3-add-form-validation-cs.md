---
uid: mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-cs
title: '#3 de iteração – adicionar validaçãoC#de formulário () | Microsoft Docs'
author: microsoft
description: Na terceira iteração, adicionamos a validação básica de formulário. Impedimos que as pessoas enviem um formulário sem concluir os campos de formulário necessários. Também validamos o emai...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 51a0d175-913b-43d8-95e3-840fb96ad1a9
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-cs
msc.type: authoredcontent
ms.openlocfilehash: af2e86e820f60f0a3d8e3db8f78eba67ef63579a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78544467"
---
# <a name="iteration-3--add-form-validation-c"></a>#3 de iteração – adicionar validaçãoC#de formulário ()

pela [Microsoft](https://github.com/microsoft)

[Código de download](iteration-3-add-form-validation-cs/_static/contactmanager_3_cs1.zip)

> Na terceira iteração, adicionamos a validação básica de formulário. Impedimos que as pessoas enviem um formulário sem concluir os campos de formulário necessários. Também validamos endereços de email e números de telefone.

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

Nesta segunda iteração do aplicativo Contact Manager, adicionamos a validação básica de formulário. Impedimos que as pessoas enviem um contato sem inserir valores para os campos de formulário necessários. Também validamos Números de telefone e endereços de email (veja a Figura 1).

[![caixa de diálogo novo projeto](iteration-3-add-form-validation-cs/_static/image1.jpg)](iteration-3-add-form-validation-cs/_static/image1.png)

**Figura 01**: um formulário com validação ([clique para exibir a imagem em tamanho normal](iteration-3-add-form-validation-cs/_static/image2.png))

Nessa iteração, adicionamos a lógica de validação diretamente às ações do controlador. Em geral, essa não é a maneira recomendada para adicionar validação a um aplicativo MVC ASP.NET. Uma abordagem melhor é inserir uma lógica de validação de aplicativo em uma [camada de serviço](http://martinfowler.com/eaaCatalog/serviceLayer.html)separada. Na próxima iteração, refactomos o aplicativo Contact Manager para tornar o aplicativo mais passível de manutenção.

Nessa iteração, para manter as coisas simples, escrevemos todo o código de validação manualmente. Em vez de escrever o código de validação por si só, poderíamos aproveitar uma estrutura de validação. Por exemplo, você pode usar o Microsoft Enterprise Library Validation Application Block (VAB) para implementar a lógica de validação para seu aplicativo ASP.NET MVC. Para saber mais sobre o bloco de aplicativo de validação, consulte:

[*http://msdn.microsoft.com/library/dd203099.aspx*](https://msdn.microsoft.com/library/dd203099.aspx)

## <a name="adding-validation-to-the-create-view"></a>Adicionando validação ao modo de exibição de criação

Vamos começar adicionando a lógica de validação ao modo de exibição de criação. Felizmente, como geramos a exibição Create com o Visual Studio, a exibição Create já contém toda a lógica da interface do usuário necessária para exibir mensagens de validação. A exibição criar está contida na Listagem 1.

**Listagem 1-\Views\Contact\Create.aspx**

[!code-aspx[Main](iteration-3-add-form-validation-cs/samples/sample1.aspx)]

Observe a chamada para o método auxiliar HTML. ValidationSummary () que aparece imediatamente acima do formulário HTML. Se houver mensagens de erro de validação, esse método exibirá mensagens de validação em uma lista com marcadores.

Observe, além disso, as chamadas para HTML. ValidationMessage () que aparecem ao lado de cada campo de formulário. O auxiliar ValidationMessage () exibe uma mensagem de erro de validação individual. No caso da listagem 1, um asterisco é exibido quando há um erro de validação.

Por fim, o auxiliar HTML. TextBox () renderiza automaticamente uma classe de folha de estilos em cascata quando há um erro de validação associado à propriedade exibida pelo auxiliar. O auxiliar HTML. TextBox () renderiza uma classe chamada **Input-Validation-Error**.

Quando você cria um novo aplicativo MVC ASP.NET, uma folha de estilos denominada site. CSS é criada na pasta de conteúdo automaticamente. Esta folha de estilo contém as seguintes definições para classes CSS relacionadas à aparência de mensagens de erro de validação:

[!code-css[Main](iteration-3-add-form-validation-cs/samples/sample2.css)]

A classe campo-validação-erro é usada para estilizar a saída renderizada pelo auxiliar HTML. ValidationMessage (). A classe de erro de validação de entrada é usada para estilizar a caixa de texto (entrada) renderizada pelo auxiliar HTML. TextBox (). A classe Validation-Summary-Errors é usada para estilizar a lista não ordenada renderizada pelo auxiliar HTML. ValidationSummary ().

> [!NOTE] 
> 
> Você pode modificar as classes de folha de estilo descritas nesta seção para personalizar a aparência de mensagens de erro de validação.

## <a name="adding-validation-logic-to-the-create-action"></a>Adicionando lógica de validação à ação de criação

No momento, o Create View nunca exibe mensagens de erro de validação porque não escrevemos a lógica para gerar nenhuma mensagem. Para exibir mensagens de erro de validação, você precisa adicionar as mensagens de erro a ModelState.

> [!NOTE] 
> 
> O método UpdateModel () adiciona mensagens de erro a ModelState automaticamente quando há um erro ao atribuir o valor de um campo de formulário a uma propriedade. Por exemplo, se você tentar atribuir a cadeia de caracteres "Apple" a uma propriedade BirthDate que aceita valores DateTime, o método UpdateModel () adicionará um erro a ModelState.

O método Create () modificado na Listagem 2 contém uma nova seção que valida as propriedades da classe Contact antes que o novo contato seja inserido no banco de dados.

**Listagem 2-Controllers\ContactController.cs (criar com validação)**

[!code-csharp[Main](iteration-3-add-form-validation-cs/samples/sample3.cs)]

A seção validar impõe quatro regras de validação distintas:

- A propriedade FirstName deve ter um comprimento maior que zero (e não pode consistir apenas em espaços)
- A propriedade LastName deve ter um comprimento maior que zero (e não pode consistir apenas em espaços)
- Se a propriedade Phone tiver um valor (tem um comprimento maior que 0), a propriedade Phone deverá corresponder a uma expressão regular.
- Se a propriedade de email tiver um valor (tem um comprimento maior que 0), a propriedade de email deverá corresponder a uma expressão regular.

Quando há uma violação de regra de validação, uma mensagem de erro é adicionada a ModelState com a ajuda do método AddModelError (). Ao adicionar uma mensagem a ModelState, você fornece o nome de uma propriedade e o texto de uma mensagem de erro de validação. Essa mensagem de erro é exibida na exibição pelos métodos auxiliares HTML. ValidationSummary () e HTML. ValidationMessage ().

Depois que as regras de validação são executadas, a Propriedade IsValid de ModelState é verificada. A Propriedade IsValid retorna false quando quaisquer mensagens de erro de validação foram adicionadas a ModelState. Se a validação falhar, o formulário criar será exibido novamente com as mensagens de erro.

> [!NOTE] 
> 
> Obtive as expressões regulares para validar o número de telefone e o endereço de email do repositório de expressões regulares em [ *http://regexlib.com* ](http://regexlib.com)

## <a name="adding-validation-logic-to-the-edit-action"></a>Adicionando lógica de validação à ação de edição

A ação editar () atualiza um contato. A ação editar () precisa executar exatamente a mesma validação que a ação criar (). Em vez de duplicar o mesmo código de validação, devemos refatorar o controlador de contato para que as ações de Create () e Edit () chamem o mesmo método de validação.

A classe do controlador de contato modificado está contida na Listagem 3. Essa classe tem um novo método ValidateContact () que é chamado nas ações Create () e Edit ().

**Listagem 3-Controllers\ContactController.cs**

[!code-csharp[Main](iteration-3-add-form-validation-cs/samples/sample4.cs)]

## <a name="summary"></a>Resumo

Nessa iteração, adicionamos a validação de formulário básica ao nosso aplicativo Contact Manager. Nossa lógica de validação impede que os usuários enviem um novo contato ou editem um contato existente sem fornecer valores para as propriedades FirstName e LastName. Além disso, os usuários devem fornecer números de telefone e endereços de email válidos.

Nessa iteração, adicionamos a lógica de validação ao nosso aplicativo Contact Manager da maneira mais fácil possível. No entanto, misturar nossa lógica de validação em nossa lógica do controlador criará problemas para nós a longo prazo. Nosso aplicativo será mais difícil de manter e modificar com o passar do tempo.

Na próxima iteração, iremos refatorar nossa lógica de validação e lógica de acesso do banco de dados de nossos controladores. Aproveitaremos vários princípios de design de software para nos permitir a criação de um aplicativo mais flexível e mais passível de manutenção.

> [!div class="step-by-step"]
> [Anterior](iteration-2-make-the-application-look-nice-cs.md)
> [Próximo](iteration-4-make-the-application-loosely-coupled-cs.md)
