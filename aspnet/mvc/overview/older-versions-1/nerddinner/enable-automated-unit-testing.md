---
uid: mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
title: Habilitar o teste automatizado de unidade | Microsoft Docs
author: microsoft
description: A etapa 12 mostra como desenvolver um pacote de testes de unidade automatizados que verificam nossa funcionalidade NerdDinner e que nos fornecerá a confiança de fazer alterações...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: a19ff2ce-3f7e-4358-9a51-a1403da9c63e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
msc.type: authoredcontent
ms.openlocfilehash: 09a7aa186605a6cce48ee94028425ded957c00d3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78541674"
---
# <a name="enable-automated-unit-testing"></a>Habilitar o teste de unidade automatizado

pela [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Esta é a etapa 12 de um [tutorial de aplicativo "NerdDinner"](introducing-the-nerddinner-tutorial.md) gratuito que percorre como criar um aplicativo Web pequeno, mas completo usando o ASP.NET MVC 1.
> 
> A etapa 12 mostra como desenvolver um pacote de testes de unidade automatizados que verificam nossa funcionalidade de NerdDinner e que nos fornecerá a confiança de fazer alterações e melhorias no aplicativo no futuro.
> 
> Se você estiver usando o ASP.NET MVC 3, recomendamos seguir as [introdução com](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) os tutoriais da [loja de música](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) MVC 3 ou MVC.

## <a name="nerddinner-step-12-unit-testing"></a>Etapa 12 do NerdDinner: teste de unidade

Vamos desenvolver um pacote de testes de unidade automatizados que verificam nossa funcionalidade de NerdDinner e que nos fornecerá a confiança de fazer alterações e melhorias no aplicativo no futuro.

### <a name="why-unit-test"></a>Por que testar unidade?

Na unidade no trabalho, uma manhã você tem um flash repentino de inspiração sobre um aplicativo no qual está trabalhando. Você percebe que há uma alteração que pode ser implementada, o que tornará o aplicativo muito melhor. Pode ser uma refatoração que limpa o código, adiciona um novo recurso ou corrige um bug.

A pergunta que o levará quando chegar ao seu computador é: "quão segura é para fazer essa melhoria?" E se fazer a alteração tiver efeitos colaterais ou quebrar algo? A alteração pode ser simples e levar apenas alguns minutos para ser implementada, mas e se levar horas para testar manualmente todos os cenários de aplicativos? E se você se esquecer de abordar um cenário e um aplicativo interrompido entrar em produção? Esse aperfeiçoamento vale muito o esforço?

Os testes de unidade automatizados podem fornecer uma rede de segurança que permite que você aprimore seus aplicativos continuamente e evite ter medo do código no qual está trabalhando. Ter testes automatizados que verificam a funcionalidade rapidamente permite codificar com confiança – e capacitar você a fazer melhorias que, de outra forma, você não sentiria confortável. Eles também ajudam a criar soluções que são mais fáceis de manter e têm um tempo de vida maior, o que leva a um retorno muito maior sobre o investimento.

O ASP.NET MVC Framework torna mais fácil e natural a funcionalidade do aplicativo de teste de unidade. Ele também habilita um fluxo de trabalho de TDD (desenvolvimento controlado por teste) que habilita o desenvolvimento baseado em teste primeiro.

### <a name="nerddinnertests-project"></a>Projeto NerdDinner. Tests

Quando criamos nosso aplicativo NerdDinner no início deste tutorial, recebemos uma caixa de diálogo perguntando se queríamos criar um projeto de teste de unidade para acompanhar o projeto de aplicativo:

![](enable-automated-unit-testing/_static/image1.png)

Mantivemos o botão de opção "Sim, criar um projeto de teste de unidade" selecionado – o que resultou em um projeto "NerdDinner. Tests" que está sendo adicionado à nossa solução:

![](enable-automated-unit-testing/_static/image2.png)

O projeto NerdDinner. Tests referencia o assembly do projeto de aplicativo NerdDinner e permite que possamos adicionar facilmente testes automatizados a ele que verificam a funcionalidade do aplicativo.

### <a name="creating-unit-tests-for-our-dinner-model-class"></a>Criando testes de unidade para nossa classe de modelo de jantar

Vamos adicionar alguns testes ao nosso projeto NerdDinner. Tests que verificamos a classe de jantar que criamos quando criamos nossa camada de modelo.

Vamos começar criando uma nova pasta dentro de nosso projeto de teste chamada "Models", onde colocaremos nossos testes relacionados ao modelo. Em seguida, clicaremos com o botão direito do mouse na pasta e escolheremos o comando **Add-&gt;New Test** menu. Isso abrirá a caixa de diálogo "Adicionar novo teste".

Vamos optar por criar um "teste de unidade" e nomeá-lo como "DinnerTest.cs":

![](enable-automated-unit-testing/_static/image3.png)

Quando clicamos no botão "OK", o Visual Studio adicionará (e abrirá) um arquivo DinnerTest.cs ao projeto:

![](enable-automated-unit-testing/_static/image4.png)

O modelo de teste de unidade padrão do Visual Studio tem um monte de códigos de chapas dentro dele que acho um pouco confuso. Vamos limpá-lo para conter apenas o código abaixo:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample1.cs)]

O atributo [TestClass] na classe DinnerTest acima o identifica como uma classe que conterá testes, bem como a inicialização de teste opcional e o código de subdivisão. Podemos definir testes dentro dele adicionando métodos públicos que têm um atributo [TestMethod] neles.

Abaixo estão os primeiros dois testes que adicionaremos a nossa aula de jantar. O primeiro teste verifica se o nosso jantar é inválido se um novo jantar é criado sem que todas as propriedades sejam definidas corretamente. O segundo teste verifica se o nosso jantar é válido quando um jantar tem todas as propriedades definidas com valores válidos:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample2.cs)]

Você notará acima que nossos nomes de teste são muito explícitos (e um pouco detalhados). Estamos fazendo isso porque podemos acabar criando centenas ou milhares de pequenos testes, e queremos facilitar a determinação rápida da intenção e do comportamento de cada um deles (especialmente quando estamos examinando uma lista de falhas em um executor de teste). Os nomes de teste devem ser nomeados após a funcionalidade que eles estão testando. Acima, estamos usando um padrão de nomenclatura "substantivo\_deve\_verbo".

Estamos estruturando os testes usando o padrão de teste "AAA", que significa "Arrange, Act, Assert":

- Organizar: configurar a unidade que está sendo testada
- Act: exercitar a unidade em teste e capturar os resultados
- Assert: verificar o comportamento

Quando escrevemos testes, queremos evitar que os testes individuais façam muito. Em vez disso, cada teste deve verificar apenas um único conceito (o que tornará muito mais fácil identificar a causa das falhas). Uma boa diretriz é tentar e ter apenas uma única instrução Assert para cada teste. Se você tiver mais de uma instrução Assert em um método de teste, certifique-se de que todas estejam sendo usadas para testar o mesmo conceito. Em caso de dúvida, faça outro teste.

### <a name="running-tests"></a>Executando testes

O Visual Studio 2008 Professional (e edições superiores) inclui um executor de teste interno que pode ser usado para executar projetos de teste de unidade do Visual Studio dentro do IDE. É possível selecionar o comando **Test-&gt;execute-&gt;todos os testes no menu da solução** (ou digite CTRL R, a) para executar todos os nossos testes de unidade. Ou, como alternativa, podemos posicionar nosso cursor dentro de uma classe de teste específica ou um método de teste e usar os **testes de execução de&gt;de teste&gt;no** comando de menu de contexto atual (ou digite CTRL R, t) para executar um subconjunto dos testes de unidade.

Vamos posicionar nosso cursor dentro da classe DinnerTest e digitar "CTRL R, T" para executar os dois testes que acabamos de definir. Quando fazemos isso, uma janela "Resultados de Teste" aparecerá no Visual Studio e veremos os resultados da nossa execução de teste listada nele:

![](enable-automated-unit-testing/_static/image5.png)

*Observação: a janela de resultados de teste do VS não mostra a coluna nome da classe por padrão. Você pode adicionar isso clicando com o botão direito do mouse na janela Resultados de Teste e usando o comando de menu Adicionar/remover colunas.*

Nossos dois testes levaram apenas uma fração de um segundo a ser executado – e como você pode ver que ambos passaram. Agora podemos passar e aumentá-los criando testes adicionais que verificam validações de regra específicas, bem como abrangem os dois métodos auxiliares – IsUserHost () e IsUserRegistered () – que adicionamos à classe jantar. Ter todos esses testes em vigor para a classe jantar tornará muito mais fácil e mais segura a adição de novas regras de negócios e validações a ele no futuro. Podemos adicionar nossa nova lógica de regra ao jantar e, em alguns segundos, verificar se ela não violou nenhuma de nossas funcionalidades lógicas anteriores.

Observe como usar um nome de teste descritivo torna mais fácil entender rapidamente o que cada teste está verificando. Recomendo usar o comando de menu **ferramentas –&gt;opções** , abrir a tela de configuração de execução de teste ferramentas de&gt;de teste e marcar a caixa de seleção "clicar duas vezes em um resultado de teste de unidade com falha ou inconclusivo exibe o ponto de falha no teste". Isso permitirá que você clique duas vezes em uma falha na janela de resultados de teste e vá imediatamente para a falha de Assert.

### <a name="creating-dinnerscontroller-unit-tests"></a>Criando testes de unidade DinnersController

Agora, vamos criar alguns testes de unidade que verificam nossa funcionalidade DinnersController. Começaremos clicando com o botão direito do mouse na pasta "controladores" em nosso projeto de teste e escolheremos o comando de menu **adicionar&gt;novo teste** . Vamos criar um "teste de unidade" e nomeá-lo como "DinnersControllerTest.cs".

Criaremos dois métodos de teste que verificam o método de ação Details () no DinnersController. O primeiro verificará se uma exibição é retornada quando um jantar existente é solicitado. O segundo verificará se uma exibição "não encontrada" é retornada quando um jantar não existente é solicitado:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample3.cs)]

O código acima compila a limpeza. No entanto, quando executamos os testes, eles falham:

![](enable-automated-unit-testing/_static/image6.png)

Se observarmos as mensagens de erro, veremos que o motivo da falha nos testes foi porque nossa classe DinnersRepository não pôde se conectar a um banco de dados. Nosso aplicativo NerdDinner está usando uma cadeia de conexão para um arquivo de SQL Server Express local que reside no diretório de dados do \App\_do projeto de aplicativo NerdDinner. Como nosso projeto NerdDinner. Tests compila e executa em um diretório diferente, o projeto de aplicativo, o local do caminho relativo da nossa cadeia de conexão está incorreto.

Poderíamos *corrigir isso* copiando o arquivo de banco de dados do SQL Express para nosso projeto de teste e, em seguida, adicionar uma cadeia de conexão de teste apropriada a ele no app. config do nosso projeto de teste. Isso obteria os testes acima desbloqueados e em execução.

No entanto, o código de teste de unidade usando um banco de dados real traz vários desafios. Especificamente:

- Ele reduz significativamente o tempo de execução dos testes de unidade. Quanto mais tempo é necessário para executar testes, menor é a probabilidade de executá-los com frequência. O ideal é que você queira que os testes de unidade possam ser executados em segundos – e fazer com que seja algo que você faça tão naturalmente quanto compilar o projeto.
- Isso complica a lógica de configuração e limpeza em testes. Você deseja que cada teste de unidade seja isolado e independente de outros (sem efeitos colaterais ou dependências). Ao trabalhar com um banco de dados real, você precisa estar atento ao estado e redefini-lo entre os testes.

Vamos examinar um padrão de design chamado "injeção de dependência" que pode nos ajudar a solucionar esses problemas e evitar a necessidade de usar um banco de dados real com nossos testes.

### <a name="dependency-injection"></a>Injeção de dependência

No momento, DinnersController está rigidamente "acoplado" à classe DinnerRepository. "Acoplamento" refere-se a uma situação em que uma classe depende explicitamente de outra classe para funcionar:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample4.cs)]

Como a classe DinnerRepository requer acesso a um banco de dados, a dependência rigidamente acoplada que a classe DinnersController tem no DinnerRepository acaba exigindo que tenhamos um banco de dados para que os métodos de ação DinnersController sejam testados.

Podemos contornar isso empregando um padrão de design chamado "injeção de dependência" – que é uma abordagem em que as dependências (como as classes de repositório que fornecem acesso a dados) não são mais implicitamente criadas em classes que as usam. Em vez disso, as dependências podem ser passadas explicitamente para a classe que as utiliza usando argumentos de construtor. Se as dependências forem definidas usando interfaces, então temos a flexibilidade para passar por implementações de dependências "falsas" para cenários de teste de unidade. Isso nos permite criar implementações de dependência específicas de teste que, na verdade, não exigem acesso a um banco de dados.

Para ver isso em ação, vamos implementar injeção de dependência com nosso DinnersController.

#### <a name="extracting-an-idinnerrepository-interface"></a>Extraindo uma interface IDinnerRepository

Nossa primeira etapa será criar uma nova interface IDinnerRepository que encapsula o contrato de repositório que nossos controladores precisam para recuperar e atualizar os jantares.

Podemos definir esse contrato de interface manualmente clicando com o botão direito do mouse na pasta \Models e escolhendo o comando de menu **adicionar&gt;novo item** e criando uma nova interface chamada IDinnerRepository.cs.

Como alternativa, podemos usar as ferramentas de refatoração internas Visual Studio Professional (e edições superiores) para extrair e criar automaticamente uma interface para nós da nossa classe DinnerRepository existente. Para extrair essa interface usando VS, basta posicionar o cursor no editor de texto na classe DinnerRepository e clicar com o botão direito do mouse e escolher o comando de menu **Refactor-&gt;extrair interface** :

![](enable-automated-unit-testing/_static/image7.png)

Isso abrirá a caixa de diálogo "Extrair interface" e solicitará o nome da interface a ser criada. Ele usará como padrão IDinnerRepository e selecionará automaticamente todos os métodos públicos na classe DinnerRepository existente para adicionar à interface:

![](enable-automated-unit-testing/_static/image8.png)

Quando clicamos no botão "OK", o Visual Studio adicionará uma nova interface IDinnerRepository ao nosso aplicativo:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample5.cs)]

E nossa classe DinnerRepository existente será atualizada para que ela implemente a interface:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample6.cs)]

#### <a name="updating-dinnerscontroller-to-support-constructor-injection"></a>Atualizando DinnersController para dar suporte à injeção de Construtor

Agora, atualizaremos a classe DinnersController para usar a nova interface.

Atualmente, o DinnersController é embutido em código, de modo que seu campo "dinnerRepository" é sempre uma classe DinnerRepository:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample7.cs)]

Vamos alterá-lo para que o campo "dinnerRepository" seja do tipo IDinnerRepository em vez de DinnerRepository. Em seguida, adicionaremos dois construtores DinnersController públicos. Um dos construtores permite que um IDinnerRepository seja passado como um argumento. O outro é um construtor padrão que usa nossa implementação de DinnerRepository existente:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample8.cs)]

Como o ASP.NET MVC, por padrão, cria classes de controlador usando construtores padrão, nosso DinnersController em tempo de execução continuará a usar a classe DinnerRepository para executar o acesso a dados.

Agora podemos atualizar nossos testes de unidade, no entanto, para passar uma implementação de repositório de jantar "falsa" usando o construtor de parâmetro. Esse repositório de jantar "falsificado" não exigirá acesso a um banco de dados real e, em vez disso, usará o uso da memória.

#### <a name="creating-the-fakedinnerrepository-class"></a>Criando a classe FakeDinnerRepository

Vamos criar uma classe FakeDinnerRepository.

Vamos começar criando um diretório "falsificações" em nosso projeto NerdDinner. Tests e, em seguida, adicionar uma nova classe FakeDinnerRepository a ela (clique com o botão direito do mouse na pasta e escolha **Add-&gt;nova classe**):

![](enable-automated-unit-testing/_static/image9.png)

Atualizaremos o código para que a classe FakeDinnerRepository implemente a interface IDinnerRepository. Em seguida, podemos clicar com o botão direito do mouse nele e escolher o comando de menu de contexto "Implement interface IDinnerRepository":

![](enable-automated-unit-testing/_static/image10.png)

Isso fará com que o Visual Studio adicione automaticamente todos os membros da interface IDinnerRepository à nossa classe FakeDinnerRepository com implementações padrão de "stub":

[!code-csharp[Main](enable-automated-unit-testing/samples/sample9.cs)]

Em seguida, podemos atualizar a implementação de FakeDinnerRepository para trabalhar fora de uma lista na memória&lt;&gt; a coleção de jantar passada como um argumento de construtor:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample10.cs)]

Agora temos uma implementação de IDinnerRepository falsa que não requer um banco de dados e, em vez disso, pode trabalhar com uma lista na memória de objetos de jantar.

#### <a name="using-the-fakedinnerrepository-with-unit-tests"></a>Usando o FakeDinnerRepository com testes de unidade

Vamos voltar para os testes de unidade DinnersController que falharam anteriormente porque o banco de dados não estava disponível. Podemos atualizar os métodos de teste para usar um FakeDinnerRepository populado com dados de jantar na memória de exemplo para o DinnersController usando o código abaixo:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample11.cs)]

E agora, quando executamos esses testes, ambos eles são aprovados:

![](enable-automated-unit-testing/_static/image11.png)

O melhor de tudo é que eles levam apenas uma fração de um segundo para execução e não exigem nenhuma lógica complicada de instalação/limpeza. Agora podemos testar a unidade todo o nosso código de método de ação DinnersController (incluindo listagem, paginação, detalhes, criar, atualizar e excluir) sem precisar se conectar a um banco de dados real.

| **Tópico lateral: estruturas de injeção de dependência** |
| --- |
| A execução da injeção de dependência manual (como a que estamos acima) funciona bem, mas torna-se mais difícil de manter à medida que o número de dependências e componentes em um aplicativo aumenta. Existem várias estruturas de injeção de dependência para o .NET que podem ajudar a fornecer ainda mais flexibilidade de gerenciamento de dependência. Essas estruturas, às vezes chamadas de contêineres IoC (inversão de controle), fornecem mecanismos que permitem um nível adicional de suporte à configuração para especificar e passar dependências para objetos em tempo de execução (com mais frequência usando injeção de Construtor ). Algumas das estruturas de injeção de dependência OSS/IOC mais populares no .NET incluem: AutoFac, Ninject, Spring.NET, StructureMap e Windsor. O ASP.NET MVC expõe APIs de extensibilidade que permitem que os desenvolvedores participem da resolução e da instanciação de controladores, e que permite que as estruturas de injeção de dependência/IoC sejam integradas com clareza nesse processo. Usar uma estrutura DI/IOC também nos permitiria remover o construtor padrão de nosso DinnersController – que removeria completamente o acoplamento entre ele e o DinnerRepository. Não vamos usar uma estrutura de injeção de dependência/IOC com nosso aplicativo NerdDinner. Mas é algo que poderíamos considerar para o futuro se a base de código e os recursos NerdDinner crescerem. |

### <a name="creating-edit-action-unit-tests"></a>Criando testes de unidade de ação de edição

Agora, vamos criar alguns testes de unidade que verificam a funcionalidade de edição do DinnersController. Começaremos testando a versão HTTP-GET da nossa ação de edição:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample12.cs)]

Vamos criar um teste que verifica se um modo de exibição apoiado por um objeto DinnerFormViewModel é renderizado de volta quando um jantar válido é solicitado:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample13.cs)]

No entanto, quando executamos o teste, descobrimos que ele falha porque uma exceção de referência nula é gerada quando o método Edit acessa a propriedade User.Identity.Name para executar a verificação de jantar. IsHostedBy ().

O objeto user na classe base do controlador encapsula detalhes sobre o usuário conectado e é populado pelo ASP.NET MVC quando ele cria o controlador em tempo de execução. Como estamos testando o DinnersController fora de um ambiente de servidor Web, o objeto de usuário não é definido (portanto, a exceção de referência nula).

### <a name="mocking-the-useridentityname-property"></a>Simulação da propriedade User.Identity.Name

As estruturas fictícias facilitam o teste ao permitir que possamos criar dinamicamente versões falsas de objetos dependentes que dão suporte a nossos testes. Por exemplo, podemos usar uma estrutura fictícia em nosso teste de ação de edição para criar dinamicamente um objeto de usuário que o nosso DinnersController pode usar para pesquisar um nome simulado. Isso evitará que uma referência nula seja gerada quando executarmos nosso teste.

Há muitas estruturas de simulação do .NET que podem ser usadas com o ASP.NET MVC (você pode ver uma lista delas aqui: [http://www.mockframeworks.com/](http://www.mockframeworks.com/)). Para testar nosso aplicativo NerdDinner, usaremos uma estrutura fictícia de código-fonte aberto chamada "MOQ", que pode ser baixada gratuitamente do [http://www.mockframeworks.com/moq](http://www.mockframeworks.com/moq).

Depois de baixado, adicionaremos uma referência em nosso projeto NerdDinner. Tests ao assembly MOQ. dll:

![](enable-automated-unit-testing/_static/image12.png)

Em seguida, adicionaremos um método auxiliar "CreateDinnersControllerAs (username)" à nossa classe de teste que usa um nome de usuário como parâmetro e que, em seguida, "imita" a propriedade User.Identity.Name na instância DinnersController:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample14.cs)]

Acima, estamos usando MOQ para criar um objeto fictício que falsifica um objeto ControllerContext (que é o que o ASP.NET MVC passa para classes de controlador para expor objetos de tempo de execução como usuário, solicitação, resposta e sessão). Estamos chamando o método "SetupGet" na simulação para indicar que a propriedade HttpContext.User.Identity.Name em ControllerContext deve retornar a cadeia de caracteres de nome de usuário que passamos para o método auxiliar.

Podemos simular qualquer número de propriedades e métodos de ControllerContext. Para ilustrar isso, também adicionei uma chamada SetupGet () para a propriedade Request. IsAuthenticated (que não é realmente necessária para os testes abaixo, mas que ajuda a ilustrar como você pode simular Propriedades de solicitação). Quando concluímos, atribuímos uma instância da simulação ControllerContext ao DinnersController que nosso método auxiliar retorna.

Agora podemos escrever testes de unidade que usam esse método auxiliar para testar cenários de edição que envolvem usuários diferentes:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample15.cs)]

E agora, quando executamos os testes que eles passam:

![](enable-automated-unit-testing/_static/image13.png)

### <a name="testing-updatemodel-scenarios"></a>Testando cenários UpdateModel ()

Criamos testes que abrangem a versão HTTP-GET da ação editar. Agora, vamos criar alguns testes que verificam a versão HTTP-POST da ação editar:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample16.cs)]

O novo cenário de teste interessante para que possamos dar suporte com esse método de ação é seu uso do método auxiliar UpdateModel () na classe base do controlador. Estamos usando esse método auxiliar para associar valores de formulário-postagem à nossa instância de objeto do jantar.

Abaixo estão dois testes que demonstram como podemos fornecer valores postados de formulário para o método auxiliar UpdateModel () a ser usado. Faremos isso criando e Populando um objeto FormsCollection e, em seguida, atribuí-lo à propriedade "ValueProvider" no controlador.

O primeiro teste verifica se, em um salvamento bem-sucedido, o navegador é redirecionado para a ação de detalhes. O segundo teste verifica se, quando a entrada inválida é postada, a ação reexibe a exibição editar novamente com uma mensagem de erro.

[!code-csharp[Main](enable-automated-unit-testing/samples/sample17.cs)]

### <a name="testing-wrap-up"></a>Testando a conclusão

Nós abordamos os conceitos básicos envolvidos nas classes do controlador de teste de unidade. Podemos usar essas técnicas para criar facilmente centenas de testes simples que verificam o comportamento de nosso aplicativo.

Como nossos testes de controlador e modelo não exigem um banco de dados real, eles são extremamente rápidos e fáceis de executar. Poderemos executar centenas de testes automatizados em segundos e, imediatamente, obter comentários sobre se uma alteração que fizemos ou não quebrou algo. Isso ajudará a nos fornecer a confiança para aprimorar, Refatorar e refinar continuamente nosso aplicativo.

Abordamos o teste como o último tópico deste capítulo, mas não porque o teste é algo que você deve fazer no final de um processo de desenvolvimento! Por contrário, você deve escrever testes automatizados o mais cedo possível em seu processo de desenvolvimento. Isso permite que você obtenha comentários imediatos à medida que desenvolve, ajuda você a pensar em consideração sobre os cenários de caso de uso do seu aplicativo e o orienta a projetar seu aplicativo com a disposição em camadas e acoplamento em mente.

Um capítulo posterior no livro discutirá o TDD (desenvolvimento controlado por teste) e como usá-lo com o ASP.NET MVC. O TDD é uma prática de codificação iterativa na qual você primeiro escreve os testes que seu código resultante atenderá. Com o TDD, você começa cada recurso criando um teste que verifica a funcionalidade que você está prestes a implementar. Escrever o teste de unidade primeiro ajuda a garantir que você entenda claramente o recurso e como ele deve funcionar. Somente depois que o teste é gravado (e você verificou que ele falha), você implementa a funcionalidade real que o teste verifica. Como você já passou o tempo pensando no caso de uso de como o recurso deve funcionar, você terá uma melhor compreensão dos requisitos e da melhor maneira de implementá-los. Quando terminar a implementação, você poderá executar novamente o teste e obter comentários imediatos sobre se o recurso funciona corretamente. Abordaremos mais o TDD no capítulo 10.

### <a name="next-step"></a>Próxima etapa

Alguns comentários finais de retorno.

> [!div class="step-by-step"]
> [Anterior](use-ajax-to-implement-mapping-scenarios.md)
> [Próximo](nerddinner-wrap-up.md)
