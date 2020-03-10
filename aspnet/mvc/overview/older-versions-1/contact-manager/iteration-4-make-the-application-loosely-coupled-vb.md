---
uid: mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-vb
title: '#4 de iteração – tornar o aplicativo levemente acoplado (VB) | Microsoft Docs'
author: microsoft
description: Nesta quarta iteração, aproveitamos os vários padrões de design de software para facilitar a manutenção e a modificação do aplicativo Contact Manager. Para...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 92c70297-4430-4e4e-919a-9c2333a8d09a
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-vb
msc.type: authoredcontent
ms.openlocfilehash: 422c75406d9c08279d0c2224ee4b6db3a71eb1b3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78582078"
---
# <a name="iteration-4--make-the-application-loosely-coupled-vb"></a>#4 de iteração – tornar o aplicativo levemente acoplado (VB)

pela [Microsoft](https://github.com/microsoft)

[Código de download](iteration-4-make-the-application-loosely-coupled-vb/_static/contactmanager_4_vb1.zip)

> Nesta quarta iteração, aproveitamos os vários padrões de design de software para facilitar a manutenção e a modificação do aplicativo Contact Manager. Por exemplo, podemos refatorar nosso aplicativo para usar o padrão de repositório e o padrão de injeção de dependência.

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

Nesta quarta iteração do aplicativo Contact Manager, podemos refatorar o aplicativo para tornar o aplicativo mais rígidomente acoplado. Quando um aplicativo está acoplado livremente, você pode modificar o código em uma parte do aplicativo sem a necessidade de modificar o código em outras partes do aplicativo. Aplicativos livremente acoplados são mais resilientes de serem alterados.

Atualmente, toda a lógica de acesso a dados e validação usada pelo aplicativo Contact Manager está contida nas classes do controlador. Essa é uma má ideia. Sempre que você precisar modificar uma parte do seu aplicativo, correrá o risco de introduzir bugs em outra parte do seu aplicativo. Por exemplo, se você modificar a lógica de validação, correrá o risco de introduzir novos bugs em sua lógica de acesso a dados ou de controlador.

> [!NOTE] 
> 
> (SRP), uma classe nunca deve ter mais de um motivo para ser alterada. A combinação de controlador, validação e lógica de banco de dados é uma grande violação do princípio de responsabilidade única.

Há várias razões pelas quais você pode precisar modificar seu aplicativo. Talvez seja necessário adicionar um novo recurso ao seu aplicativo, talvez seja necessário corrigir um bug em seu aplicativo, ou talvez seja necessário modificar como um recurso de seu aplicativo é implementado. Os aplicativos raramente são estáticos. Eles tendem a crescer e se mutar ao longo do tempo.

Imagine, por exemplo, que você decida alterar como implementar sua camada de acesso a dados. No momento, o aplicativo Contact Manager usa o Microsoft Entity Framework para acessar o banco de dados. No entanto, você pode optar por migrar para uma tecnologia de acesso a dados nova ou alternativa, como ADO.NET Data Services ou NHibernate. No entanto, como o código de acesso a dados não é isolado do código de validação e do controlador, não há como modificar o código de acesso a dados em seu aplicativo sem modificar outro código que não esteja diretamente relacionado ao acesso a dados.

Quando um aplicativo é livremente acoplado, por outro lado, você pode fazer alterações em uma parte de um aplicativo sem tocar em outras partes de um aplicativo. Por exemplo, você pode alternar as tecnologias de acesso a dados sem modificar a validação ou a lógica do controlador.

Nessa iteração, aproveitamos vários padrões de design de software que nos permitem refatorar nosso aplicativo Contact Manager em um aplicativo mais flexívelmente acoplado. Quando terminarmos, o gerente de contato ganhou t faz tudo o que não fazia antes. No entanto, poderemos alterar o aplicativo mais facilmente no futuro.

> [!NOTE] 
> 
> A refatoração é o processo de reescrever um aplicativo de forma que ele não perca nenhuma funcionalidade existente.

## <a name="using-the-repository-software-design-pattern"></a>Usando o padrão de design de software do repositório

Nossa primeira alteração é tirar proveito de um padrão de design de software chamado de padrão Repository. Usaremos o padrão Repository para isolar nosso código de acesso a dados do restante do nosso aplicativo.

A implementação do padrão Repository exige que possamos concluir as duas etapas a seguir:

1. Criar uma interface
2. Criar uma classe concreta que implementa a interface

Primeiro, precisamos criar uma interface que descreve todos os métodos de acesso a dados que precisamos executar. A interface IContactManagerRepository está contida na Listagem 1. Essa interface descreve cinco métodos: CreateContact (), DeleteContact (), EditContact (), GetContact e ListContacts ().

**Listagem 1-Models\IContactManagerRepository.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample1.vb)]

Em seguida, precisamos criar uma classe concreta que implemente a interface IContactManagerRepository. Como estamos usando o Entity Framework da Microsoft para acessar o banco de dados, criaremos uma nova classe chamada EntityContactManagerRepository. Essa classe está contida na Listagem 2.

**Listagem 2-Models\EntityContactManagerRepository.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample2.vb)]

Observe que a classe EntityContactManagerRepository implementa a interface IContactManagerRepository. A classe implementa todos os cinco métodos descritos por essa interface.

Você deve estar imaginando por que precisamos nos preocupar com uma interface. Por que precisamos criar uma interface e uma classe que a implemente?

Com uma exceção, o restante do nosso aplicativo irá interagir com a interface e não com a classe concreta. Em vez de chamar os métodos expostos pela classe EntityContactManagerRepository, vamos chamar os métodos expostos pela interface IContactManagerRepository.

Dessa forma, podemos implementar a interface com uma nova classe sem a necessidade de modificar o restante do nosso aplicativo. Por exemplo, em alguma data futura, talvez queiramos implementar uma classe DataServicesContactManagerRepository que implemente a interface IContactManagerRepository. A classe DataServicesContactManagerRepository pode usar o ADO.NET Data Services para acessar um banco de dados em vez do Microsoft Entity Framework.

Se o código do aplicativo for programado na interface IContactManagerRepository em vez da classe concreta EntityContactManagerRepository, poderemos mudar as classes concretas sem modificar qualquer um dos demais códigos. Por exemplo, podemos mudar da classe EntityContactManagerRepository para a classe DataServicesContactManagerRepository sem modificar nossa lógica de validação ou acesso a dados.

A programação em interfaces (abstrações) em vez de classes concretas torna nosso aplicativo mais resiliente a ser alterado.

> [!NOTE] 
> 
> Você pode criar rapidamente uma interface de uma classe concreta no Visual Studio selecionando a opção de menu Refactor, extrair interface. Por exemplo, você pode criar a classe EntityContactManagerRepository primeiro e, em seguida, usar a interface de extração para gerar a interface IContactManagerRepository automaticamente.

## <a name="using-the-dependency-injection-software-design-pattern"></a>Usando o padrão de design de software de injeção de dependência

Agora que migramos nosso código de acesso a dados para uma classe de repositório separada, precisamos modificar nosso controlador de contato para usar essa classe. Aproveitaremos um padrão de design de software chamado injeção de dependência para usar a classe Repository em nosso controlador.

O controlador de contato modificado está contido na Listagem 3.

**Listagem 3-Controllers\ContactController.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample3.vb)]

Observe que o controlador de contato na Listagem 3 tem dois construtores. O primeiro construtor passa uma instância concreta da interface IContactManagerRepository para o segundo construtor. A classe de controlador de contato usa *injeção de dependência de Construtor*.

O único lugar em que a classe EntityContactManagerRepository é usada está no primeiro construtor. O restante da classe usa a interface IContactManagerRepository em vez da classe concreta EntityContactManagerRepository.

Isso facilita alternar as implementações da classe IContactManagerRepository no futuro. Se você quiser usar a classe DataServicesContactRepository em vez da classe EntityContactManagerRepository, basta modificar o primeiro construtor.

A injeção de dependência de Construtor também torna a classe de controlador de contato muito testada. Em seus testes de unidade, você pode instanciar o controlador de contato passando uma implementação fictícia da classe IContactManagerRepository. Esse recurso de injeção de dependência será muito importante para nós na próxima iteração quando criarmos testes de unidade para o aplicativo Contact Manager.

> [!NOTE] 
> 
> Se você quiser desacoplar completamente a classe do controlador de contato de uma implementação específica da interface IContactManagerRepository, poderá aproveitar uma estrutura que dá suporte à injeção de dependência, como StructureMap ou a Microsoft Entity Framework (MEF). Aproveitando uma estrutura de injeção de dependência, você nunca precisa se referir a uma classe concreta em seu código.

## <a name="creating-a-service-layer"></a>Criando uma camada de serviço

Talvez você tenha notado que nossa lógica de validação ainda está misturada com nossa lógica do controlador na classe do controlador modificado na Listagem 3. Pelo mesmo motivo que é uma boa ideia isolar nossa lógica de acesso a dados, é uma boa ideia isolar nossa lógica de validação.

Para corrigir esse problema, podemos criar uma camada de [serviço](http://martinfowler.com/eaaCatalog/serviceLayer.html)separada. A camada de serviço é uma camada separada que podemos inserir entre nossas classes de controlador e de repositório. A camada de serviço contém nossa lógica de negócios, incluindo toda a nossa lógica de validação.

O ContactManagerService está contido na Listagem 4. Ele contém a lógica de validação da classe de controlador de contato.

**Listagem 4-Models\ContactManagerService.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample4.vb)]

Observe que o construtor para o ContactManagerService requer um ValidationDictionary. A camada de serviço se comunica com a camada do controlador por meio desse ValidationDictionary. Discutiremos o ValidationDictionary em detalhes na seção a seguir ao discutirmos o padrão de decorador.

Observe, além disso, que o ContactManagerService implementa a interface IContactManagerService. Você sempre deve se esforçar para programar em relação a interfaces em vez de classes concretas. Outras classes no aplicativo Contact Manager não interagem diretamente com a classe ContactManagerService. Em vez disso, com uma exceção, o restante do aplicativo Contact Manager é programado na interface IContactManagerService.

A interface IContactManagerService está contida na listagem 5.

**Listagem 5-Models\IContactManagerService.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample5.vb)]

A classe do controlador de contato modificado está contida na Listagem 6. Observe que o controlador de contato não interage mais com o repositório ContactManager. Em vez disso, o controlador de contato interage com o serviço ContactManager. Cada camada é isolada tanto quanto possível de outras camadas.

**Listagem 6-Controllers\ContactController.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample6.vb)]

Nosso aplicativo não executa mais afoul do único princípio de responsabilidade (SRP). O controlador de contato na Listagem 6 foi eliminado de toda responsabilidade além de controlar o fluxo de execução do aplicativo. Toda a lógica de validação foi removida do controlador de contato e enviada por push para a camada de serviço. Toda a lógica do banco de dados foi enviada por push para a camada de repositório.

## <a name="using-the-decorator-pattern"></a>Usando o padrão de decorador

Queremos ser capazes de desassociar completamente nossa camada de serviço da nossa camada do controlador. Em princípio, devemos ser capazes de Compilar nossa camada de serviço em um assembly separado de nossa camada de controlador sem a necessidade de adicionar uma referência ao nosso aplicativo MVC.

No entanto, nossa camada de serviço precisa ser capaz de passar mensagens de erro de validação de volta para a camada do controlador. Como podemos habilitar a camada de serviço para comunicar mensagens de erro de validação sem unir o controlador e a camada de serviço? Podemos aproveitar um padrão de design de software chamado de [padrão decorador](http://en.wikipedia.org/wiki/Decorator_pattern).

Um controlador usa um ModelStateDictionary chamado ModelState para representar erros de validação. Portanto, você pode estar tentado a passar ModelState da camada do controlador para a camada de serviço. No entanto, usar ModelState na camada de serviço tornaria sua camada de serviço dependente de um recurso da estrutura MVC do ASP.NET. Isso seria insatisfatório porque, em algum momento, talvez você queira usar a camada de serviço com um aplicativo WPF em vez de um aplicativo MVC ASP.NET. Nesse caso, você não desejaria fazer referência à estrutura MVC do ASP.NET para usar a classe ModelStateDictionary.

O padrão decorador permite encapsular uma classe existente em uma nova classe a fim de implementar uma interface. Nosso projeto do Gerenciador de contatos inclui a classe ModelStateWrapper contida na Listagem 7. A classe ModelStateWrapper implementa a interface na Listagem 8.

**Listagem 7-Models\Validation\ModelStateWrapper.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample7.vb)]

**Listagem 8-Models\Validation\IValidationDictionary.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample8.vb)]

Se olhar com a listagem 5, você verá que a camada de serviço ContactManager usa exclusivamente a interface IValidationDictionary. O serviço ContactManager não depende da classe ModelStateDictionary. Quando o controlador de contato cria o serviço ContactManager, o controlador encapsula seu ModelState da seguinte maneira:

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample9.vb)]

## <a name="summary"></a>Resumo

Nessa iteração, não adicionamos nenhuma nova funcionalidade ao aplicativo Contact Manager. O objetivo dessa iteração era refatorar o aplicativo Contact Manager para que seja mais fácil de manter e modificar.

Primeiro, implementamos o padrão de design de software do repositório. Migramos todo o código de acesso a dados para uma classe de repositório ContactManager separada.

Também isolamos nossa lógica de validação da lógica do controlador. Criamos uma camada de serviço separada que contém todo o nosso código de validação. A camada do controlador interage com a camada de serviço e a camada de serviço interage com a camada de repositório.

Quando criamos a camada de serviço, tiramos proveito do padrão decorador para isolar ModelState de nossa camada de serviço. Em nossa camada de serviço, programamos na interface IValidationDictionary em vez de ModelState.

Finalmente, tiramos proveito de um padrão de design de software chamado padrão de injeção de dependência. Esse padrão nos permite programar em interfaces (abstrações) em vez de classes concretas. Implementar o padrão de design de injeção de dependência também torna nosso código mais testado. Na próxima iteração, adicionamos testes de unidade ao nosso projeto.

> [!div class="step-by-step"]
> [Anterior](iteration-3-add-form-validation-vb.md)
> [Próximo](iteration-5-create-unit-tests-vb.md)
