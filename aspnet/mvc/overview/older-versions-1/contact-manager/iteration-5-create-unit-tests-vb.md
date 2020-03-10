---
uid: mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-vb
title: '#5 de iteração – criar testes de unidade (VB) | Microsoft Docs'
author: microsoft
description: Na quinta iteração, tornamos o nosso aplicativo mais fácil de manter e modificar adicionando testes de unidade. Simulamos nossas classes de modelo de dados e criamos testes de unidade para o...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: c6e5c036-2265-4fa7-a9eb-47f197bdc262
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-vb
msc.type: authoredcontent
ms.openlocfilehash: 4ce1c6224a7e9203ff62f136f4f3a43e4561a904
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78544292"
---
# <a name="iteration-5--create-unit-tests-vb"></a>#5 de iteração – criar testes de unidade (VB)

pela [Microsoft](https://github.com/microsoft)

[Código de download](iteration-5-create-unit-tests-vb/_static/contactmanager_5_vb1.zip)

> Na quinta iteração, tornamos o nosso aplicativo mais fácil de manter e modificar adicionando testes de unidade. Simulamos nossas classes de modelo de dados e criamos testes de unidade para nossos controladores e lógica de validação.

## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Criando um aplicativo MVC de gerenciamento de contatos ASP.NET (VB)

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

Na iteração anterior do aplicativo Contact Manager, podemos refatorar o aplicativo para ser mais rígido. Separamos o aplicativo em camadas distintas de controlador, serviço e repositório. Cada camada interage com a camada abaixo dela por meio de interfaces.

Refatorei o aplicativo para tornar o aplicativo mais fácil de manter e modificar. Por exemplo, se precisar usar uma nova tecnologia de acesso a dados, podemos simplesmente alterar a camada do repositório sem tocar no controlador ou na camada de serviço. Ao tornar o Gerenciador de contatos levemente acoplado, tornamos o aplicativo mais resiliente a ser alterado.

Mas o que acontece quando precisamos adicionar um novo recurso ao aplicativo Contact Manager? Ou, o que acontece quando corrigimos um bug? Uma verdade triste, mas bem comprovada, de escrever código é que sempre que você toca no código, você cria o risco de introduzir novos bugs.

Por exemplo, um dia bom, seu gerente pode pedir que você adicione um novo recurso ao Gerenciador de contatos. Ela deseja adicionar suporte para grupos de contatos. Ela deseja que você permita que os usuários organizem seus contatos em grupos como amigos, negócios e assim por diante.

Para implementar esse novo recurso, você precisará modificar todas as três camadas do aplicativo Contact Manager. Você precisará adicionar uma nova funcionalidade aos controladores, à camada de serviço e ao repositório. Assim que você começar a modificar o código, correrá o risco de perder a funcionalidade que funcionou antes.

Refatorar nosso aplicativo em camadas separadas, como fizemos na iteração anterior, era algo bom. Foi uma coisa boa porque permite fazer alterações em camadas inteiras sem tocar no restante do aplicativo. No entanto, se você quiser tornar o código dentro de uma camada mais fácil de manter e modificar, você precisará criar testes de unidade para o código.

Você usa um teste de unidade para testar uma unidade de código individual. Essas unidades de código são menores do que as camadas de aplicativo inteiras. Normalmente, você usa um teste de unidade para verificar se um método específico em seu código se comporta da maneira esperada. Por exemplo, você criaria um teste de unidade para o método CreateContact () exposto pela classe ContactManagerService.

Os testes de unidade de um aplicativo funcionam exatamente como uma rede de segurança. Sempre que você modifica o código em um aplicativo, pode executar um conjunto de testes de unidade para verificar se a modificação quebra a funcionalidade existente. Os testes de unidade tornam seu código seguro para modificar. Os testes de unidade tornam todo o código em seu aplicativo mais resiliente para ser alterado.

Nessa iteração, adicionamos testes de unidade ao nosso aplicativo Contact Manager. Dessa forma, na próxima iteração, podemos adicionar grupos de contatos ao nosso aplicativo sem se preocupar com a interrupção da funcionalidade existente.

> [!NOTE] 
> 
> Há uma variedade de estruturas de teste de unidade, incluindo NUnit, xUnit.net e MbUnit. Neste tutorial, usamos a estrutura de testes de unidade incluída no Visual Studio. No entanto, você poderia usar facilmente uma dessas estruturas alternativas.

## <a name="what-gets-tested"></a>O que é testado

No mundo perfeito, todo o seu código seria abordado por testes de unidade. No mundo perfeito, você teria a rede de segurança perfeita. Você poderá modificar qualquer linha de código em seu aplicativo e saber instantaneamente, executando os testes de unidade, independentemente de a alteração estar com a funcionalidade existente.

No entanto, nós não podemos viver em um mundo perfeito. Na prática, ao escrever testes de unidade, você se concentra em escrever testes para sua lógica de negócios (por exemplo, lógica de validação). Em particular, você *não* grava testes de unidade para sua lógica de acesso de dados ou para a lógica de exibição.

Para ser útil, os testes de unidade devem ser executados muito rapidamente. Você pode facilmente acumular centenas (ou até milhares) de testes de unidade para um aplicativo. Se os testes de unidade demorarem muito tempo para serem executados, você evitará executá-los. Em outras palavras, testes de unidade de longa execução são inúteis para fins de codificação dia a dia.

Por esse motivo, normalmente você não escreve testes de unidade para o código que interage com um banco de dados. A execução de centenas de testes de unidade em um banco de dados dinâmico seria muito lenta. Em vez disso, você simula seu banco de dados e escreve o código que interage com o banco de dados fictício (abordamos a simulação de um banco de dados abaixo).

Por um motivo semelhante, normalmente você não escreve testes de unidade para exibições. Para testar uma exibição, você deve criar um servidor Web. Como a rotação de um servidor Web é um processo relativamente lento, não é recomendável criar testes de unidade para suas exibições.

Se o modo de exibição contiver uma lógica complicada, você deverá considerar mover a lógica para métodos auxiliares. Você pode escrever testes de unidade para métodos auxiliares que executam sem girar um servidor Web.

> [!NOTE] 
> 
> Embora escrever testes para lógica de acesso a dados ou lógica de exibição não seja uma boa ideia ao escrever testes de unidade, esses testes podem ser muito valiosos ao criar testes funcionais ou de integração.

> [!NOTE] 
> 
> O ASP.NET MVC é o mecanismo de exibição Web Forms. Embora o mecanismo de exibição de Web Forms seja dependente de um servidor Web, outros mecanismos de exibição podem não ser.

## <a name="using-a-mock-object-framework"></a>Usando uma estrutura de objeto fictício

Ao criar testes de unidade, você quase sempre precisa tirar proveito de uma estrutura de objeto fictício. Uma estrutura de objeto fictício permite que você crie simulações e stubs para as classes em seu aplicativo.

Por exemplo, você pode usar uma estrutura de objeto fictício para gerar uma versão fictícia de sua classe de repositório. Dessa forma, você pode usar a classe de repositório fictício em vez da classe real de repositório em seus testes de unidade. O uso do repositório fictício permite evitar a execução do código do banco de dados durante a execução de um teste de unidade.

O Visual Studio não inclui uma estrutura de objeto fictício. No entanto, há várias estruturas de objeto de simulação de código-fonte aberto e comercial disponíveis para o .NET Framework:

1. MOQ-essa estrutura está disponível na licença BSD de software livre. Você pode baixar o MOQ de [https://code.google.com/p/moq/](https://code.google.com/p/moq/).
2. Rhino Mocks – essa estrutura está disponível na licença BSD de software livre. Você pode baixar as simulações Rhino de [http://ayende.com/projects/rhino-mocks.aspx](http://ayende.com/projects/rhino-mocks.aspx).
3. Typemock Isolator – esta é uma estrutura comercial. Você pode baixar uma versão de avaliação do [http://www.typemock.com/](http://www.typemock.com/).

Neste tutorial, decidi usar MOQ. No entanto, você poderia usar facilmente as simulações Rhino ou Typemock Isolator para criar os objetos fictícios para o aplicativo Contact Manager.

Para poder usar o MOQ, você precisa concluir as seguintes etapas:

1. .
2. Antes de descompactar o download, certifique-se de clicar com o botão direito do mouse no arquivo e clicar no botão **desbloquear** (consulte a Figura 1).
3. Descompacte o download.
4. Adicione uma referência ao assembly MOQ ao projeto de teste selecionando a opção de menu **projeto, Adicionar referência** para abrir a caixa de diálogo **Adicionar referência** . Na guia procurar, navegue até a pasta onde você descompactou o MOQ e selecione o assembly MOQ. dll. Clique no botão **OK** (consulte a Figura 2).

[![desbloquear MOQ](iteration-5-create-unit-tests-vb/_static/image1.jpg)](iteration-5-create-unit-tests-vb/_static/image1.png)

**Figura 01**: desbloqueio de MOQ ([clique para exibir a imagem em tamanho normal](iteration-5-create-unit-tests-vb/_static/image2.png))

[Referências de ![depois de adicionar MOQ](iteration-5-create-unit-tests-vb/_static/image2.jpg)](iteration-5-create-unit-tests-vb/_static/image3.png)

**Figura 02**: referências depois de adicionar MOQ ([clique para exibir a imagem em tamanho normal](iteration-5-create-unit-tests-vb/_static/image4.png))

## <a name="creating-unit-tests-for-the-service-layer"></a>Criando testes de unidade para a camada de serviço

Vamos começar criando um conjunto de testes de unidade para a camada de serviço s do aplicativo Contact Manager. Usaremos esses testes para verificar nossa lógica de validação.

Crie uma nova pasta chamada modelos no projeto ContactManager. Tests. Em seguida, clique com o botão direito do mouse na pasta modelos e selecione **Adicionar, novo teste**. A caixa de diálogo **Adicionar novo teste** mostrada na Figura 3 é exibida. Selecione o modelo de **teste de unidade** e nomeie o novo teste ContactManagerServiceTest. vb. Clique no botão **OK** para adicionar o novo teste ao projeto de teste.

> [!NOTE] 
> 
> Em geral, você deseja que a estrutura de pastas do seu projeto de teste corresponda à estrutura de pastas do seu projeto MVC ASP.NET. Por exemplo, você coloca os testes de controlador em uma pasta de controladores, os testes de modelo em uma pasta de modelos e assim por diante.

[![Models\ContactManagerServiceTest.cs](iteration-5-create-unit-tests-vb/_static/image3.jpg)](iteration-5-create-unit-tests-vb/_static/image5.png)

**Figura 03**: Models\ContactManagerServiceTest.cs ([clique para exibir a imagem em tamanho normal](iteration-5-create-unit-tests-vb/_static/image6.png))

Inicialmente, queremos testar o método CreateContact () exposto pela classe ContactManagerService. Vamos criar os cinco testes a seguir:

- CreateContact ()-testa se CreateContact () retorna o valor true quando um contato válido é passado para o método.
- CreateContactRequiredFirstName () – testa se uma mensagem de erro é adicionada ao estado do modelo quando um contato com um nome ausente é passado para o método CreateContact ().
- CreateContactRequiredLastName () – testa se uma mensagem de erro é adicionada ao estado do modelo quando um contato com um sobrenome ausente é passado para o método CreateContact ().
- CreateContactInvalidPhone () – testa se uma mensagem de erro é adicionada ao estado do modelo quando um contato com um número de telefone inválido é passado para o método CreateContact ().
- CreateContactInvalidEmail () – testa se uma mensagem de erro é adicionada ao estado do modelo quando um contato com um endereço de email inválido é passado para o método CreateContact ()..

O primeiro teste verifica se um contato válido não gera um erro de validação. Os testes restantes verificam cada uma das regras de validação.

O código para esses testes está contido na Listagem 1.

**Listagem 1-Models\ContactManagerServiceTest.vb**

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample1.vb)]

Como usamos a classe Contact na Listagem 1, precisamos adicionar uma referência ao Microsoft Entity Framework ao nosso projeto de teste. Adicione uma referência ao assembly System. Data. Entity.

A listagem 1 contém um método chamado Initialize () que é decorado com o atributo [TestInitialize]. Esse método é chamado automaticamente antes que cada um dos testes de unidade seja executado (ele é chamado 5 vezes logo antes de cada um dos testes de unidade). O método Initialize () cria um repositório fictício com a seguinte linha de código:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample2.vb)]

Essa linha de código usa a estrutura MOQ para gerar um repositório fictício da interface IContactManagerRepository. O repositório de simulações é usado em vez do EntityContactManagerRepository real para evitar o acesso ao banco de dados quando cada teste de unidade é executado. O repositório de simulações implementa os métodos da interface IContactManagerRepository, mas os métodos Don t realmente fazem nada.

> [!NOTE] 
> 
> Ao usar a estrutura MOQ, há uma distinção entre \_mockRepository e \_mockRepository. Object. O primeiro se refere à classe fictício (Of IContactManagerRepository) que contém métodos para especificar como o repositório de imitação se comportará. O último se refere ao repositório de simulação real que implementa a interface IContactManagerRepository.

O repositório de simulações é usado no método Initialize () ao criar uma instância da classe ContactManagerService. Todos os testes de unidade individuais usam essa instância da classe ContactManagerService.

A listagem 1 contém cinco métodos que correspondem a cada um dos testes de unidade. Cada um desses métodos é decorado com o atributo [TestMethod]. Quando você executa os testes de unidade, qualquer método que tenha esse atributo é chamado. Em outras palavras, qualquer método decorado com o atributo [TestMethod] é um teste de unidade.

O primeiro teste de unidade, chamado CreateContact (), verifica se a chamada de CreateContact () retorna o valor true quando uma instância válida da classe Contact é passada para o método. O teste cria uma instância da classe Contact, chama o método CreateContact () e verifica se CreateContact () retorna o valor true.

Os testes restantes verificam se, quando o método CreateContact () é chamado com um contato inválido, o método retorna false e a mensagem de erro de validação esperada é adicionada ao estado do modelo. Por exemplo, o teste CreateContactRequiredFirstName () cria uma instância da classe Contact com uma cadeia de caracteres vazia para sua propriedade FirstName. Em seguida, o método CreateContact () é chamado com o contato inválido. Por fim, o teste verifica se CreateContact () retorna false e se o estado do modelo contém a mensagem de erro de validação esperada "o primeiro nome é obrigatório".

Você pode executar os testes de unidade na Listagem 1 selecionando a opção de menu **teste, executar, todos os testes na solução (Ctrl + R, A)** . Os resultados dos testes são exibidos na janela de Resultados de Teste (consulte a Figura 4).

[![Resultados de Teste](iteration-5-create-unit-tests-vb/_static/image4.jpg)](iteration-5-create-unit-tests-vb/_static/image7.png)

**Figura 04**: resultados de teste ([clique para exibir a imagem em tamanho normal](iteration-5-create-unit-tests-vb/_static/image8.png))

## <a name="creating-unit-tests-for-controllers"></a>Criando testes de unidade para controladores

O aplicativo ASP.NET MVC controla o fluxo de interação do usuário. Ao testar um controlador, você deseja testar se o controlador retorna o resultado da ação correta e exibir os dados. Você também pode querer testar se um controlador interage com classes de modelo da maneira esperada.

Por exemplo, a listagem 2 contém dois testes de unidade para o método Create () do controlador de contato. O primeiro teste de unidade verifica se um contato válido é passado para o método Create () e, em seguida, o método Create () redireciona para a ação de índice. Em outras palavras, quando passado um contato válido, o método Create () deve retornar um RedirectToRouteResult que representa a ação de índice.

Não queremos testar a camada de serviço ContactManager quando estamos testando a camada do controlador. Portanto, simulamos a camada de serviço com o seguinte código no método Initialize:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample3.vb)]

No teste de unidade CreateValidContact (), simulamos o comportamento de chamar o método CreateContact () da camada de serviço com a seguinte linha de código:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample4.vb)]

Essa linha de código faz com que o serviço de contato do simulador retorne o valor true quando seu método CreateContact () é chamado. Ao simular a camada de serviço, podemos testar o comportamento de nosso controlador sem a necessidade de executar qualquer código na camada de serviço.

O segundo teste de unidade verifica se a ação criar () retorna a exibição criar quando um contato inválido é passado para o método. Fazemos com que o método CreateContact () da camada de serviço retorne o valor false com a seguinte linha de código:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample5.vb)]

Se o método Create () se comparecer como esperado, ele deve retornar a exibição Create quando a camada de serviço retornar o valor false. Dessa forma, o controlador pode exibir as mensagens de erro de validação no modo de exibição criar e o usuário tem a oportunidade de corrigir essas propriedades de contato inválidas.

Se você planeja criar testes de unidade para seus controladores, precisará retornar nomes de exibição explícitos das ações do controlador. Por exemplo, não retorne uma exibição como esta:

Retornar exibição ()

Em vez disso, retorne a exibição da seguinte maneira:

Retornar exibição ("criar")

Se você não for explícito ao retornar uma exibição, a propriedade ViewResult. ViewName retornará uma cadeia de caracteres vazia.

**Listagem 2-Controllers\ContactControllerTest.vb**

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample6.vb)]

## <a name="summary"></a>Resumo

Nessa iteração, criamos testes de unidade para nosso aplicativo Contact Manager. Podemos executar esses testes de unidade a qualquer momento para verificar se o nosso aplicativo ainda se comporta da maneira esperada. Os testes de unidade atuam como uma rede de segurança para nosso aplicativo, permitindo que possamos modificar com segurança nosso aplicativo no futuro.

Criamos dois conjuntos de testes de unidade. Primeiro, testamos nossa lógica de validação criando testes de unidade para nossa camada de serviço. Em seguida, testamos nossa lógica de controle de fluxo criando testes de unidade para nossa camada de controlador. Ao testar nossa camada de serviço, isolamos nossos testes de nossa camada de serviço de nossa camada de repositório simulando nossa camada de repositório. Ao testar a camada do controlador, isolamos nossos testes de nossa camada de controlador simulando a camada de serviço.

Na próxima iteração, modificamos o aplicativo Contact Manager para que ele dê suporte a grupos de contatos. Adicionaremos essa nova funcionalidade ao nosso aplicativo usando um processo de design de software chamado desenvolvimento orientado por testes.

> [!div class="step-by-step"]
> [Anterior](iteration-4-make-the-application-loosely-coupled-vb.md)
> [Próximo](iteration-6-use-test-driven-development-vb.md)
