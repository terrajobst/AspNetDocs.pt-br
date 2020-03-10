---
uid: mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-cs
title: '#6 de iteração – usar o desenvolvimento controladoC#por testes () | Microsoft Docs'
author: microsoft
description: Na sexta-iteração, adicionamos nova funcionalidade ao nosso aplicativo escrevendo testes de unidade primeiro e escrevendo código em relação aos testes de unidade. Nesta iteração,...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 013c3c26-7dc3-41d1-8064-f233c86008b5
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-cs
msc.type: authoredcontent
ms.openlocfilehash: aee0ff9d8d7f17e8a00dab12467bd3a3457fbe18
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601797"
---
# <a name="iteration-6--use-test-driven-development-c"></a>#6 de iteração – usar o desenvolvimento controladoC#por testes ()

pela [Microsoft](https://github.com/microsoft)

[Código de download](iteration-6-use-test-driven-development-cs/_static/contactmanager_6_cs1.zip)

> Na sexta-iteração, adicionamos nova funcionalidade ao nosso aplicativo escrevendo testes de unidade primeiro e escrevendo código em relação aos testes de unidade. Nessa iteração, adicionamos grupos de contatos.

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

Na iteração anterior do aplicativo Contact Manager, criamos testes de unidade para fornecer uma rede de segurança para nosso código. A motivação para criar os testes de unidade era tornar nosso código mais resiliente a ser alterado. Com testes de unidade em vigor, podemos fazer qualquer alteração em nosso código e saber imediatamente se desmembramos a funcionalidade existente.

Nessa iteração, usamos testes de unidade para uma finalidade totalmente diferente. Nessa iteração, usamos testes de unidade como parte de uma filosofia de design de aplicativo chamada *desenvolvimento orientado por testes*. Ao praticar o desenvolvimento controlado por testes, você escreve testes primeiro e, em seguida, escreve o código em relação aos testes.

Mais precisamente, ao praticar o desenvolvimento orientado por testes, há três etapas que você preenche ao criar código (vermelho/verde/Refactor):

1. Gravar um teste de unidade que falha (vermelho)
2. Escrever código que passa o teste de unidade (verde)
3. Refatorar seu código (Refactor)

Primeiro, você escreve o teste de unidade. O teste de unidade deve expressar sua intenção de como você espera que seu código se comporte. Quando você cria o teste de unidade pela primeira vez, o teste de unidade deve falhar. O teste deve falhar porque você ainda não escreveu nenhum código de aplicativo que satisfaça o teste.

Em seguida, você escreve apenas o código suficiente para que o teste de unidade passe. O objetivo é escrever o código da maneira mais lenta, sloppiest e mais rápida possível. Você não deve perder tempo pensando na arquitetura do seu aplicativo. Em vez disso, você deve se concentrar em escrever a quantidade mínima de código necessário para atender à intenção expressa pelo teste de unidade.

Por fim, depois de escrever código suficiente, você pode voltar e considerar a arquitetura geral do seu aplicativo. Nesta etapa, você reescreve (refatore) seu código aproveitando os padrões de design de software, como o padrão de repositório, para que seu código seja mais passível de manutenção. Você pode fearlessly reescrever seu código nesta etapa porque seu código é coberto por testes de unidade.

Há muitos benefícios resultantes da prática do desenvolvimento orientado a testes. Primeiro, o desenvolvimento orientado por testes força você a se concentrar no código que realmente precisa ser escrito. Como você está constantemente concentrado em apenas escrever código suficiente para passar em um determinado teste, você é impedido de se aprofundar nas buscas e escrever enormes quantidades de código que nunca será usado.

Em segundo lugar, uma metodologia de design "testar primeiro" força você a escrever código da perspectiva de como seu código será usado. Em outras palavras, ao praticar o desenvolvimento controlado por testes, você está constantemente escrevendo seus testes de uma perspectiva do usuário. Portanto, o desenvolvimento orientado por testes pode resultar em APIs mais claras e mais compreensíveis.

Por fim, o desenvolvimento orientado por testes força você a escrever testes de unidade como parte do processo normal de gravação de um aplicativo. Como um prazo de projeto se aproxima, o teste é normalmente a primeira coisa que sai da janela. Ao praticar o desenvolvimento controlado por testes, por outro lado, é mais provável que você seja virtuoso sobre escrever testes de unidade porque o desenvolvimento orientado por testes faz com que os testes de unidade sejam centralizados no processo de criação de um aplicativo.

> [!NOTE] 
> 
> Para saber mais sobre o desenvolvimento controlado por testes, recomendo que você leia o livro de Michael marinheiro **trabalhando com eficiência com código herdado**.

Nessa iteração, adicionamos um novo recurso ao nosso aplicativo Contact Manager. Adicionamos suporte para grupos de contatos. Você pode usar grupos de contatos para organizar seus contatos em categorias como grupos de negócios e amigos.

Vamos adicionar essa nova funcionalidade ao nosso aplicativo seguindo um processo de desenvolvimento controlado por testes. Vamos escrever primeiro os testes de unidade e vamos escrever todo o nosso código em relação a esses testes.

## <a name="what-gets-tested"></a>O que é testado

Como discutimos na iteração anterior, normalmente, você não escreve testes de unidade para lógica de acesso a dados ou lógica de exibição. Você não grava testes de unidade para lógica de acesso a dados porque o acesso a um banco de dado é uma operação relativamente lenta. Você não grava testes de unidade para a lógica de exibição porque o acesso a uma exibição requer a rotação de um servidor Web, que é uma operação relativamente lenta. Você não deve escrever um teste de unidade a menos que o teste possa ser executado várias vezes muito rapidamente

Como o desenvolvimento orientado por testes é orientado por testes de unidade, nos concentramos inicialmente no controlador de escrita e na lógica de negócios. Evitamos tocar no banco de dados ou em exibições. Não modificaremos o banco de dados nem criaremos nossas exibições até o final deste tutorial. Começamos com o que pode ser testado.

## <a name="creating-user-stories"></a>Criando histórias de usuários

Ao praticar o desenvolvimento controlado por testes, você sempre começa escrevendo um teste. Isso gera imediatamente a pergunta: como você decide qual teste deve ser escrito primeiro? Para responder a essa pergunta, você deve escrever um conjunto de [**histórias de usuários**](http://en.wikipedia.org/wiki/User_stories).

Uma história de usuário é uma descrição muito breve (geralmente uma frase) de um requisito de software. Deve ser uma descrição não técnica de um requisito escrito da perspectiva do usuário.

Aqui está o conjunto de histórias de usuários que descrevem os recursos exigidos pela nova funcionalidade de grupo de contatos:

1. O usuário pode exibir uma lista de grupos de contatos.
2. O usuário pode criar um novo grupo de contatos.
3. O usuário pode excluir um grupo de contatos existente.
4. O usuário pode selecionar um grupo de contatos ao criar um novo contato.
5. O usuário pode selecionar um grupo de contatos ao editar um contato existente.
6. Uma lista de grupos de contatos é exibida na exibição índice.
7. Quando um usuário clica em um grupo de contatos, uma lista de contatos correspondentes é exibida.

Observe que essa lista de histórias de usuários é totalmente compreensível por um cliente. Não há menção de detalhes de implementação técnica.

Durante o processo de criação do aplicativo, o conjunto de histórias de usuários pode se tornar mais refinado. Você pode dividir uma história de usuário em várias histórias (requisitos). Por exemplo, você pode decidir que a criação de um novo grupo de contatos deve envolver a validação. O envio de um grupo de contatos sem um nome deve retornar um erro de validação.

Depois de criar uma lista de histórias de usuário, você estará pronto para escrever seu primeiro teste de unidade. Vamos começar criando um teste de unidade para exibir a lista de grupos de contatos.

## <a name="listing-contact-groups"></a>Listando grupos de contatos

Nossa primeira história de usuário é que um usuário deve ser capaz de exibir uma lista de grupos de contatos. Precisamos expressar essa história com um teste.

Crie um novo teste de unidade clicando com o botão direito do mouse na pasta controladores no projeto ContactManager. Tests, selecionando **Adicionar, novo teste**e selecionando o modelo de **teste de unidade** (consulte a Figura 1). Nomeie o novo teste de unidade GroupControllerTest.cs e clique no botão **OK** .

[![adicionar o teste de unidade GroupControllerTest](iteration-6-use-test-driven-development-cs/_static/image1.jpg)](iteration-6-use-test-driven-development-cs/_static/image1.png)

**Figura 01**: adicionando o teste de unidade GroupControllerTest ([clique para exibir a imagem em tamanho normal](iteration-6-use-test-driven-development-cs/_static/image2.png))

Nosso primeiro teste de unidade está contido na Listagem 1. Esse teste verifica se o método index () do controlador de grupo retorna um conjunto de grupos. O teste verifica se uma coleção de grupos é retornada em exibir dados.

**Listagem 1-Controllers\GroupControllerTest.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample1.cs)]

Ao digitar o código pela primeira vez na Listagem 1 no Visual Studio, você terá muitas linhas vermelhas onduladas. Não criamos as classes GroupController ou Group.

Neste ponto, não podemos nem criar nosso aplicativo para que possamos executar nosso primeiro teste de unidade. Isso é bom. Isso conta como um teste com falha. Portanto, agora temos permissão para começar a gravar o código do aplicativo. Precisamos escrever código suficiente para executar nosso teste.

A classe de controlador de grupo na Listagem 2 contém o mínimo de código necessário para passar o teste de unidade. A ação index () retorna uma lista de grupos codificada estaticamente (a classe Group é definida na Listagem 3).

**Listagem 2-Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample2.cs)]

**Listagem 3-Models\Group.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample3.cs)]

Depois de adicionarmos as classes GroupController e Group ao nosso projeto, nosso primeiro teste de unidade é concluído com êxito (consulte a Figura 2). Fizemos o mínimo de trabalho necessário para passar no teste. É hora de comemorar.

[![êxito!](iteration-6-use-test-driven-development-cs/_static/image2.jpg)](iteration-6-use-test-driven-development-cs/_static/image3.png)

**Figura 02**: sucesso! ([Clique para exibir a imagem em tamanho normal](iteration-6-use-test-driven-development-cs/_static/image4.png))

## <a name="creating-contact-groups"></a>Criando grupos de contatos

Agora, podemos passar para a segunda história de usuário. Precisamos ser capazes de criar novos grupos de contatos. Precisamos expressar essa intenção com um teste.

O teste na Listagem 4 verifica se a chamada do método Create () com um novo grupo adiciona o grupo à lista de grupos retornada pelo método index (). Em outras palavras, se eu criar um novo grupo, eu deveria poder obter o novo grupo de volta na lista de grupos retornada pelo método index ().

**Listagem 4-Controllers\GroupControllerTest.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample4.cs)]

O teste na Listagem 4 chama o método Create () do controlador de grupo com um novo grupo de contatos. Em seguida, o teste verifica se a chamada do método de índice do controlador de grupo () retorna o novo grupo em dados de exibição.

O controlador de grupo modificado na listagem 5 contém o mínimo de alterações necessárias para passar o novo teste.

**Listagem 5-Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample5.cs)]

## <a name="the-group-controller-in-listing-5-has-a-new-create-action-this-action-adds-a-group-to-a-collection-of-groups-notice-that-the-index-action-has-been-modified-to-return-the-contents-of-the-collection-of-groups"></a>O controlador de grupo na listagem 5 tem uma nova ação criar (). Essa ação adiciona um grupo a uma coleção de grupos. Observe que a ação index () foi modificada para retornar o conteúdo da coleção de grupos.

Mais uma vez, realizamos a quantidade mínima de trabalho necessária para passar o teste de unidade. Depois de fazer essas alterações no controlador de grupo, todos os testes de unidade são aprovados.

## <a name="adding-validation"></a>Adicionando uma Validação

Esse requisito não foi declarado explicitamente na história do usuário. No entanto, é razoável exigir que um grupo tenha um nome. Caso contrário, organizar os contatos em grupos não seria muito útil.

A listagem 6 contém um novo teste que expressa essa intenção. Esse teste verifica se a tentativa de criar um grupo sem fornecer um nome resulta em uma mensagem de erro de validação no estado do modelo.

**Listagem 6-Controllers\GroupControllerTest.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample6.cs)]

Para atender a esse teste, precisamos adicionar uma propriedade Name à nossa classe Group (consulte a listagem 7). Além disso, precisamos adicionar um pouco de lógica de validação à nossa ação Create () do controlador de grupo (consulte a listagem 8).

**Listagem 7-Models\Group.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample7.cs)]

**Listagem 8-Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample8.cs)]

Observe que a ação criar () do controlador de grupo agora contém a validação e a lógica do banco de dados. Atualmente, o banco de dados usado pelo controlador de grupo consiste em nada mais do que uma coleção na memória.

## <a name="time-to-refactor"></a>Tempo para refatorar

A terceira etapa em vermelho/verde/Refactor é a parte de refatoração. Neste ponto, precisamos voltar do nosso código e considerar como podemos refatorar nosso aplicativo para melhorar seu design. O estágio de refatoração é o estágio no qual achamos muito sobre a melhor maneira de implementar os princípios e padrões de design de software.

Somos livres para modificar nosso código de qualquer forma que optamos por melhorar o design do código. Temos uma rede de segurança de testes de unidade que nos impedem de interromper a funcionalidade existente.

No momento, nosso controlador de grupo é uma bagunça da perspectiva do bom design de software. O controlador de grupo contém uma bagunça complicada do código de validação e de acesso a dados. Para evitar a violação do princípio de responsabilidade única, precisamos separar essas preocupações em diferentes classes.

Nossa classe de controlador de grupo refatorado está contida na listagem 9. O controlador foi modificado para usar a camada de serviço ContactManager. Essa é a mesma camada de serviço que usamos com o controlador de contato.

A listagem 10 contém os novos métodos adicionados à camada de serviço ContactManager para dar suporte à validação, listagem e criação de grupos. A interface IContactManagerService foi atualizada para incluir os novos métodos.

A listagem 11 contém uma nova classe FakeContactManagerRepository que implementa a interface IContactManagerRepository. Ao contrário da classe EntityContactManagerRepository que também implementa a interface IContactManagerRepository, nossa nova classe FakeContactManagerRepository não se comunica com o banco de dados. A classe FakeContactManagerRepository usa uma coleção na memória como um proxy para o banco de dados. Usaremos essa classe em nossos testes de unidade como uma camada de repositório falsa.

**Listagem 9-Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample9.cs)]

**Listagem 10-Controllers\ContactManagerService.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample10.cs)]

**Listagem 11-Controllers\FakeContactManagerRepository.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample11.cs)]

Modificar a interface IContactManagerRepository requer o uso para implementar os métodos CreateMethod () e ListGroups () na classe EntityContactManagerRepository. A maneira mais lenta e mais rápida de fazer isso é adicionar métodos de stub que se pareçam com o seguinte:   

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample12.cs)]

Por fim, essas alterações no design de nosso aplicativo exigem que possamos fazer algumas modificações nos testes de unidade. Agora, precisamos usar o FakeContactManagerRepository ao executar os testes de unidade. A classe GroupControllerTest atualizada está contida na listagem 12.

**Listagem 12-Controllers\GroupControllerTest.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample13.cs)]

Depois de fazermos todas essas alterações, mais uma vez, todos os testes de unidade são aprovados. Concluimos o ciclo inteiro de vermelho/verde/Refactor. Implementamos as duas primeiras histórias de usuários. Agora temos suporte a testes de unidade para os requisitos expressos nas histórias do usuário. A implementação do restante das histórias do usuário envolve a repetição do mesmo ciclo de vermelho/verde/Refactor.

## <a name="modifying-our-database"></a>Modificando nosso banco de dados

Infelizmente, embora tenhamos satisfeito todos os requisitos expressos por nossos testes de unidade, nosso trabalho não é feito. Ainda precisamos modificar nosso banco de dados.

Precisamos criar uma nova tabela de banco de dados de grupo. Siga estas etapas:

1. Na janela Gerenciador de Servidores, clique com o botão direito do mouse na pasta tabelas e selecione a opção de menu **Adicionar nova tabela**.
2. Insira as duas colunas descritas abaixo na Designer de Tabela.
3. Marque a coluna ID como uma chave primária e uma coluna de identidade.
4. Salve a nova tabela com os grupos de nomes clicando no ícone do disquete.

<a id="0.11_table01"></a>

| **Nome da Coluna** | **Tipo de Dados** | **Permitir Nulos** |
| --- | --- | --- |
| ID | INT | Falso |
| Nome | nvarchar (50) | Falso |

Em seguida, precisamos excluir todos os dados da tabela Contacts (caso contrário, não será possível criar uma relação entre as tabelas Contacts e groups). Siga estas etapas:

1. Clique com o botão direito do mouse na tabela contatos e selecione a opção de menu **Mostrar dados da tabela**.
2. Exclua todas as linhas.

Em seguida, precisamos definir uma relação entre a tabela de banco de dados Groups e a tabela de banco de dados Contacts existente. Siga estas etapas:

1. Clique duas vezes na tabela contatos na janela Gerenciador de Servidores para abrir o Designer de Tabela.
2. Adicione uma nova coluna de inteiro à tabela Contacts denominada GroupId.
3. Clique no botão relação para abrir a caixa de diálogo relações de chave estrangeira (consulte a Figura 3).
4. Clique no botão Adicionar.
5. Clique no botão de reticências que aparece ao lado do botão especificação de tabela e colunas.
6. Na caixa de diálogo tabelas e colunas, selecione grupos como a tabela de chave primária e a ID como a coluna de chave primária. Selecione contatos como a tabela de chave estrangeira e GroupId como a coluna de chave estrangeira (consulte a Figura 4). Clique no botão OK.
7. Em **especificação de inserção e atualização**, selecione o valor **cascata** para **excluir regra**.
8. Clique no botão fechar para fechar a caixa de diálogo relações de chave estrangeira.
9. Clique no botão Salvar para salvar as alterações na tabela contatos.

[![criar uma relação de tabela de banco de dados](iteration-6-use-test-driven-development-cs/_static/image3.jpg)](iteration-6-use-test-driven-development-cs/_static/image5.png)

**Figura 03**: criando uma relação de tabela de banco de dados ([clique para exibir a imagem em tamanho normal](iteration-6-use-test-driven-development-cs/_static/image6.png))

[![especificar relações de tabela](iteration-6-use-test-driven-development-cs/_static/image4.jpg)](iteration-6-use-test-driven-development-cs/_static/image7.png)

**Figura 04**: especificando relações de tabela ([clique para exibir a imagem em tamanho normal](iteration-6-use-test-driven-development-cs/_static/image8.png))

### <a name="updating-our-data-model"></a>Atualizando nosso modelo de dados

Em seguida, precisamos atualizar nosso modelo de dados para representar a nova tabela de banco de dado. Siga estas etapas:

1. Clique duas vezes no arquivo ContactManagerModel. edmx na pasta modelos para abrir o Entity Designer.
2. Clique com o botão direito do mouse na superfície do designer e selecione a opção de menu **atualizar modelo do banco de dados**.
3. No assistente de atualização, selecione a tabela grupos e clique no botão Concluir (veja a Figura 5).
4. Clique com o botão direito do mouse na entidade grupos e selecione a opção de menu **renomear**. Altere o nome da entidade *grupos* para *grupo* (singular).
5. Clique com o botão direito do mouse na propriedade de navegação grupos que aparece na parte inferior da entidade contato. Altere o nome da propriedade de navegação *groups* para *Group* (singular).

[![atualizar um modelo de Entity Framework do banco de dados](iteration-6-use-test-driven-development-cs/_static/image5.jpg)](iteration-6-use-test-driven-development-cs/_static/image9.png)

**Figura 05**: atualizando um modelo de Entity Framework do banco de dados ([clique para exibir a imagem em tamanho normal](iteration-6-use-test-driven-development-cs/_static/image10.png))

Depois de concluir essas etapas, seu modelo de dados representará as tabelas Contacts e groups. O Entity Designer deve mostrar ambas as entidades (consulte a Figura 6).

[![Entity Designer exibição de grupo e contato](iteration-6-use-test-driven-development-cs/_static/image6.jpg)](iteration-6-use-test-driven-development-cs/_static/image11.png)

**Figura 06**: Entity designer exibição de grupo e contato ([clique para exibir a imagem em tamanho normal](iteration-6-use-test-driven-development-cs/_static/image12.png))

### <a name="creating-our-repository-classes"></a>Criando nossas classes de repositório

Em seguida, precisamos implementar nossa classe Repository. Ao longo desta iteração, adicionamos vários métodos novos à interface IContactManagerRepository ao escrever código para atender aos nossos testes de unidade. A versão final da interface IContactManagerRepository está contida na listagem 14.

**Listagem 14-Models\IContactManagerRepository.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample14.cs)]

Na verdade, não implementamos nenhum dos métodos relacionados ao trabalho com grupos de contatos. Atualmente, a classe EntityContactManagerRepository tem métodos stub para cada um dos métodos do grupo de contatos listados na interface IContactManagerRepository. Por exemplo, o método ListGroups () tem a seguinte aparência:

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample15.cs)]

Os métodos de stub nos permitiram compilar nosso aplicativo e passar os testes de unidade. No entanto, agora é hora de realmente implementar esses métodos. A versão final da classe EntityContactManagerRepository está contida na listagem 13.

**Listagem 13-Models\EntityContactManagerRepository.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample16.cs)]

### <a name="creating-the-views"></a>Criando as exibições

Aplicativo ASP.NET MVC quando você usa o mecanismo de exibição padrão do ASP.NET. Portanto, você não cria exibições em resposta a um teste de unidade em particular. No entanto, como um aplicativo seria inútil sem exibições, não podemos concluir essa iteração sem criar e modificar as exibições contidas no aplicativo Contact Manager.

Precisamos criar as novas exibições a seguir para gerenciar grupos de contatos (veja a Figura 7):

- Views\Group\Index.aspx-exibe a lista de grupos de contatos
- Views\Group\Delete.aspx-exibe o formulário de confirmação para excluir um grupo de contatos

[![a exibição do índice do grupo](iteration-6-use-test-driven-development-cs/_static/image7.jpg)](iteration-6-use-test-driven-development-cs/_static/image13.png)

**Figura 07**: a exibição do índice do grupo ([clique para exibir a imagem em tamanho normal](iteration-6-use-test-driven-development-cs/_static/image14.png))

Precisamos modificar os seguintes modos de exibição existentes para que eles incluam grupos de contatos:

- Views\Home\Create.aspx
- Views\Home\Edit.aspx
- Views\Home\Index.aspx

Você pode ver os modos de exibição modificados examinando o aplicativo do Visual Studio que acompanha este tutorial. Por exemplo, a Figura 8 ilustra a exibição do índice de contato.

[![a exibição do índice de contato](iteration-6-use-test-driven-development-cs/_static/image8.jpg)](iteration-6-use-test-driven-development-cs/_static/image15.png)

**Figura 08**: a exibição do índice de contato ([clique para exibir a imagem em tamanho normal](iteration-6-use-test-driven-development-cs/_static/image16.png))

## <a name="summary"></a>Resumo

Nessa iteração, adicionamos nova funcionalidade ao nosso aplicativo Contact Manager seguindo uma metodologia de design de aplicativo de desenvolvimento controlado por testes. Começamos criando um conjunto de histórias de usuários. Criamos um conjunto de testes de unidade que corresponde aos requisitos expressos pelas histórias do usuário. Por fim, escrevemos apenas código suficiente para atender aos requisitos expressos pelos testes de unidade.

Depois de terminarmos de escrever código suficiente para atender aos requisitos expressos pelos testes de unidade, atualizamos nosso banco de dados e exibições. Adicionamos uma nova tabela de grupos ao nosso banco de dados e atualizamos nosso modelo Entity Framework Data. Também criamos e modificamos um conjunto de exibições.

Na próxima iteração – a iteração final – reescrevemos nosso aplicativo para aproveitar o Ajax. Aproveitando o Ajax, melhoraremos a capacidade de resposta e o desempenho do aplicativo Contact Manager.

> [!div class="step-by-step"]
> [Anterior](iteration-5-create-unit-tests-cs.md)
> [Próximo](iteration-7-add-ajax-functionality-cs.md)
