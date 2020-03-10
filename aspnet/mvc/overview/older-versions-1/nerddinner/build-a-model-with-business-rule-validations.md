---
uid: mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
title: Criar um modelo com validações de regra de negócio | Microsoft Docs
author: microsoft
description: A etapa 3 mostra como criar um modelo que podemos usar para consultar e atualizar o banco de dados para nosso aplicativo NerdDinner.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 0bc191b2-4311-479a-a83a-7f1b1c32e6fe
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
msc.type: authoredcontent
ms.openlocfilehash: 6ebf1b71c089229ba9139ff7dc788b8978724046
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78541660"
---
# <a name="build-a-model-with-business-rule-validations"></a>Compilar um modelo com validações de regra de negócios

pela [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Esta é a etapa 3 de um [tutorial de aplicativo "NerdDinner"](introducing-the-nerddinner-tutorial.md) gratuito que percorre como criar um aplicativo Web pequeno, mas completo usando o ASP.NET MVC 1.
> 
> A etapa 3 mostra como criar um modelo que podemos usar para consultar e atualizar o banco de dados para nosso aplicativo NerdDinner.
> 
> Se você estiver usando o ASP.NET MVC 3, recomendamos seguir as [introdução com](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) os tutoriais da [loja de música](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) MVC 3 ou MVC.

## <a name="nerddinner-step-3-building-the-model"></a>Etapa 3 do NerdDinner: criando o modelo

Em uma estrutura Model-View-Controller, o termo "Model" refere-se aos objetos que representam os dados do aplicativo, bem como a lógica de domínio correspondente que integra as regras de validação e de negócio com ele. O modelo é de muitas maneiras o "coração" de um aplicativo baseado em MVC, e como veremos mais adiante o comportamento de ti.

O ASP.NET MVC Framework dá suporte ao uso de qualquer tecnologia de acesso a dados, e os desenvolvedores podem escolher entre uma variedade de opções de dados .NET ricas para implementar seus modelos, incluindo: LINQ to Entities, LINQ to SQL, NHibernate, LLBLGen Pro, Subsonic, WilsonORM ou apenas ADO bruto. DataReaders ou DataSets NET.

Para nosso aplicativo NerdDinner, vamos usar LINQ to SQL para criar um modelo simples que corresponde bastante ao nosso design de banco de dados e adiciona algumas regras de validação e lógica de negócios personalizadas. Em seguida, implementaremos uma classe Repository que ajuda a abstrair a implementação da persistência de dados do restante do aplicativo e permite que possamos testá-la facilmente.

### <a name="linq-to-sql"></a>LINQ to SQL

LINQ to SQL é um ORM (mapeador relacional de objeto) que é fornecido como parte do .NET 3,5.

LINQ to SQL fornece uma maneira fácil de mapear tabelas de banco de dados para classes .NET com as quais podemos codificar. Para nosso aplicativo NerdDinner, vamos usá-lo para mapear os jantares e as tabelas RSVP dentro de nosso banco de dados para as aulas de jantar e de RSVP. As colunas das tabelas de jantares e de RSVP corresponderão às propriedades nas classes de jantar e de RSVP. Cada objeto de jantar e RSVP representará uma linha separada dentro dos jantares ou das tabelas RSVP no banco de dados.

LINQ to SQL nos permite evitar a criação manual de instruções SQL para recuperar e atualizar objetos de jantar e RSVP com dados de banco de dado. Em vez disso, definiremos as classes de jantar e RSVP, como elas são mapeadas de/para o banco de dados e as relações entre elas. LINQ to SQL, em seguida, cuidará da geração da lógica de execução SQL apropriada a ser usada no tempo de execução quando interagimos e as usamos.

Podemos usar o suporte à linguagem LINQ no VB e C# escrever consultas expressivas que recuperam objetos de jantar e RSVP do banco de dados. Isso minimiza a quantidade de código de dados que precisamos escrever e nos permite criar aplicativos realmente limpos.

### <a name="adding-linq-to-sql-classes-to-our-project"></a>Adicionando classes de LINQ to SQL ao nosso projeto

Começaremos clicando com o botão direito do mouse na pasta "modelos" em nosso projeto e selecionaremos o comando de menu **adicionar&gt;novo item** :

![](build-a-model-with-business-rule-validations/_static/image1.png)

Isso abrirá a caixa de diálogo "Adicionar novo item". Vamos filtrar pela categoria "data" e selecionar o modelo "LINQ to SQL classes" dentro dela:

![](build-a-model-with-business-rule-validations/_static/image2.png)

Vamos nomear o item de "NerdDinner" e clicar no botão "Add" (Adicionar). O Visual Studio adicionará um arquivo NerdDinner. dbml em nosso diretório \Models e, em seguida, abrirá o objeto LINQ to SQL Designer Relacional:

![](build-a-model-with-business-rule-validations/_static/image3.png)

### <a name="creating-data-model-classes-with-linq-to-sql"></a>Criando classes de modelo de dados com LINQ to SQL

LINQ to SQL nos permite criar rapidamente classes de modelo de dados a partir do esquema de banco de dado existente. Para fazer isso, vamos abrir o banco de dados NerdDinner no Gerenciador de Servidores e selecionar as tabelas que desejamos modelar:

![](build-a-model-with-business-rule-validations/_static/image4.png)

Em seguida, podemos arrastar as tabelas para a superfície do designer de LINQ to SQL. Quando fazemos isso LINQ to SQL criará automaticamente as classes de jantar e RSVP usando o esquema das tabelas (com propriedades de classe que são mapeadas para as colunas da tabela de banco de dados):

![](build-a-model-with-business-rule-validations/_static/image5.png)

Por padrão, o LINQ to SQL Designer "pluraliza" automaticamente os nomes de tabela e coluna ao criar classes com base em um esquema de banco de dados. Por exemplo: a tabela "jantares" em nosso exemplo acima resultou em uma classe "jantar". Essa nomenclatura de classe ajuda a tornar nossos modelos consistentes com as convenções de nomenclatura do .NET, e eu geralmente acho que o designer corrigir isso de forma conveniente (especialmente ao adicionar muitas tabelas). No entanto, se você não gostar do nome de uma classe ou propriedade que o designer gera, você sempre poderá substituí-lo e alterá-lo para qualquer nome desejado. Você pode fazer isso editando o nome de entidade/Propriedade embutido no designer ou modificando-o por meio da grade de propriedades.

Por padrão, o designer do LINQ to SQL também inspeciona as relações de chave primária/chave estrangeira das tabelas e, com base nelas, cria automaticamente "associações de relação" padrão entre as diferentes classes de modelo que ele cria. Por exemplo, quando arrastamos as tabelas de jantares e de RSVP para o designer de LINQ to SQL, uma associação de relação um-para-muitos entre os dois foi inferida com base no fato de que a tabela RSVP tinha uma chave estrangeira para a tabela de jantares (isso é indicado pela seta no Designer):

![](build-a-model-with-business-rule-validations/_static/image6.png)

A associação acima fará com que LINQ to SQL adicione uma propriedade de "jantar" fortemente tipada à classe RSVP que os desenvolvedores podem usar para acessar o jantar associado a um determinado RSVP. Também fará com que a classe jantar tenha uma propriedade de coleção "RSVPs" que permite aos desenvolvedores recuperar e atualizar objetos RSVP associados a um jantar específico.

Abaixo, você pode ver um exemplo do IntelliSense no Visual Studio quando criamos um novo objeto RSVP e o adicionamos à coleção de RSVPs de um jantar. Observe como LINQ to SQL adicionou automaticamente uma coleção de "RSVPs" no objeto de jantar:

![](build-a-model-with-business-rule-validations/_static/image7.png)

Ao adicionar o objeto RSVP à coleção de RSVPs do jantar, estamos dizendo LINQ to SQL para associar uma relação de chave estrangeira entre o jantar e a linha RSVP em nosso banco de dados:

![](build-a-model-with-business-rule-validations/_static/image8.png)

Se você não gostar de como o designer foi modelado ou nomeado como uma associação de tabela, poderá substituí-lo. Basta clicar na seta de associação dentro do designer e acessar suas propriedades por meio da grade de propriedades para renomeá-la, excluí-la ou modificá-la. No entanto, para nosso aplicativo NerdDinner, as regras de associação padrão funcionam bem para as classes de modelo de dados que estamos criando e podemos simplesmente usar o comportamento padrão.

### <a name="nerddinnerdatacontext-class"></a>Classe NerdDinnerDataContext

O Visual Studio criará automaticamente classes .NET que representam os modelos e relações de banco de dados definidos usando o designer de LINQ to SQL. Uma classe DataContext LINQ to SQL também é gerada para cada arquivo do designer de LINQ to SQL adicionado à solução. Como nomeamos nosso LINQ to SQL item de classe "NerdDinner", a classe DataContext criada será chamada de "NerdDinnerDataContext". Essa classe NerdDinnerDataContext é a principal maneira de interagir com o banco de dados.

Nossa classe NerdDinnerDataContext expõe duas propriedades-"jantares" e "RSVPs" – que representam as duas tabelas modeladas no banco de dados. Podemos usar C# para gravar consultas LINQ em relação a essas propriedades para consultar e recuperar objetos de jantar e RSVP do banco de dados.

O código a seguir demonstra como criar uma instância de um objeto NerdDinnerDataContext e executar uma consulta LINQ em relação a ele para obter uma sequência de jantares que ocorrem no futuro. O Visual Studio fornece o IntelliSense completo ao escrever a consulta LINQ, e os objetos retornados são fortemente tipados e também oferecem suporte ao IntelliSense:

![](build-a-model-with-business-rule-validations/_static/image9.png)

Além de permitir que possamos consultar objetos de jantar e de RSVP, um NerdDinnerDataContext também rastreia automaticamente todas as alterações que fizermos posteriormente nos objetos de jantar e RSVP que recuperamos por meio dele. Podemos usar essa funcionalidade para salvar facilmente as alterações no banco de dados, sem precisar escrever nenhum código explícito de atualização do SQL.

Por exemplo, o código a seguir demonstra como usar uma consulta LINQ para recuperar um único objeto de jantar do banco de dados, atualizar duas das propriedades do jantar e, em seguida, salvar as alterações de volta no banco de dados:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample1.cs)]

O objeto NerdDinnerDataContext no código acima rastreou automaticamente as alterações de propriedade feitas no objeto de jantar que recuperamos dela. Quando chamamos o método "SubmitChanges ()", ele executará uma instrução SQL "UPDATE" apropriada para o banco de dados para manter os valores atualizados de volta.

### <a name="creating-a-dinnerrepository-class"></a>Criando uma classe DinnerRepository

Para aplicativos pequenos, às vezes é bom ter controladores trabalhando diretamente em uma classe DataContext LINQ to SQL e inserir consultas LINQ dentro dos controladores. No entanto, como os aplicativos ficam maiores, essa abordagem se torna complicada para manter e testar. Ele também pode levar a duplicar as mesmas consultas LINQ em vários lugares.

Uma abordagem que pode tornar os aplicativos mais fáceis de manter e testar é usar um padrão de "repositório". Uma classe de repositório ajuda a encapsular a lógica de consulta e a persistência de dados e abstrai os detalhes de implementação da persistência de dados do aplicativo. Além de tornar o limpador de código do aplicativo, usar um padrão de repositório pode facilitar a alteração das implementações de armazenamento de dados no futuro, e pode ajudar a facilitar o teste de unidade de um aplicativo sem exigir um banco de dados real.

Para nosso aplicativo NerdDinner, definiremos uma classe DinnerRepository com a assinatura abaixo:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample2.cs)]

*Observação: mais adiante neste capítulo, vamos extrair uma interface IDinnerRepository dessa classe e habilitar a injeção de dependência com ela em nossos controladores. Para começar, no entanto, vamos começar simples e simplesmente trabalhar diretamente com a classe DinnerRepository.*

Para implementar essa classe, clicaremos com o botão direito do mouse em nossa pasta "modelos" e escolheremos o comando de menu **adicionar&gt;novo item** . Na caixa de diálogo "Adicionar novo item", selecionaremos o modelo "Class" e nome o arquivo "DinnerRepository.cs":

![](build-a-model-with-business-rule-validations/_static/image10.png)

Em seguida, podemos implementar nossa classe DinnerRepository usando o código abaixo:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample3.cs)]

### <a name="retrieving-updating-inserting-and-deleting-using-the-dinnerrepository-class"></a>Recuperando, atualizando, inserindo e excluindo usando a classe DinnerRepository

Agora que criamos nossa classe DinnerRepository, vamos dar uma olhada em alguns exemplos de código que demonstram tarefas comuns que podemos fazer com ele:

#### <a name="querying-examples"></a>Exemplos de consulta

O código a seguir recupera um único jantar usando o valor de Jantarid:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample4.cs)]

O código a seguir recupera todos os próximos jantares e loops sobre eles:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample5.cs)]

#### <a name="insert-and-update-examples"></a>Exemplos de INSERT e Update

O código a seguir demonstra como adicionar dois novos jantares. As inclusões/modificações no repositório não são confirmadas no banco de dados até que o método "Save ()" seja chamado nele. LINQ to SQL encapsula automaticamente todas as alterações em uma transação de banco de dados – de modo que todas as alterações acontecem ou nenhuma delas faz quando o repositório é salvo:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample6.cs)]

O código a seguir recupera um objeto de jantar existente e modifica duas propriedades nele. As alterações são confirmadas no banco de dados quando o método "Save ()" é chamado em nosso repositório:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample7.cs)]

O código a seguir recupera um jantar e, em seguida, adiciona um RSVP a ele. Ele faz isso usando a coleção de RSVPs no objeto de jantar que LINQ to SQL criado para nós (porque há uma relação de chave primária/chave estrangeira entre os dois no banco de dados). Essa alteração é persistida de volta para o banco de dados como uma nova linha de tabela RSVP quando o método "Save ()" é chamado no repositório:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample8.cs)]

#### <a name="delete-example"></a>Excluir exemplo

O código a seguir recupera um objeto de jantar existente e o marca para ser excluído. Quando o método "Save ()" é chamado no repositório, ele confirma a exclusão de volta para o banco de dados:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample9.cs)]

### <a name="integrating-validation-and-business-rule-logic-with-model-classes"></a>Integrando a validação e a lógica de regra de negócio com classes de modelo

A integração da validação e da lógica de regra de negócio é uma parte fundamental de qualquer aplicativo que funcione com dados.

#### <a name="schema-validation"></a>Validação de esquema

Quando as classes de modelo são definidas usando o designer de LINQ to SQL, os dados das propriedades nas classes de modelo de dados correspondem aos dados de tipo da tabela de banco de dado. Por exemplo: se a coluna "EventDate" na tabela de jantares for um "DateTime", a classe de modelo de dados criada por LINQ to SQL será do tipo "DateTime" (que é um tipo de dados .NET interno). Isso significa que você receberá erros de compilação se tentar atribuir um inteiro ou booliano a ele do código, e ele gerará um erro automaticamente se você tentar converter implicitamente um tipo de cadeia de caracteres não válido em tempo de execução.

LINQ to SQL também manipulará automaticamente valores de saída de SQL para você ao usar cadeias de caracteres, o que ajuda a protegê-lo contra ataques de injeção de SQL ao usá-lo.

#### <a name="validation-and-business-rule-logic"></a>Validação e lógica de regra de negócio

A validação de esquema é útil como uma primeira etapa, mas raramente é suficiente. A maioria dos cenários do mundo real exige a capacidade de especificar uma lógica de validação mais rica que pode abranger várias propriedades, executar código e, muitas vezes, ter consciência do estado de um modelo (por exemplo: ele está sendo criado/updated/Deleted ou dentro de um estado específico de domínio, como "arquivado"). Há uma variedade de diferentes padrões e estruturas que podem ser usadas para definir e aplicar regras de validação a classes de modelo, e há várias estruturas baseadas em .NET que podem ser usadas para ajudar com isso. Você pode usar praticamente qualquer um deles nos aplicativos MVC ASP.NET.

Para os fins do nosso aplicativo NerdDinner, usaremos um padrão relativamente simples e direto em que expõemos uma Propriedade IsValid e um método GetRuleViolations () em nosso objeto de modelo de jantar. A Propriedade IsValid retornará true ou false, dependendo se a validação e as regras de negócio são válidas. O método GetRuleViolations () retornará uma lista de erros de regra.

Implementaremos IsValid e GetRuleViolations () para nosso modelo de jantar adicionando uma "classe parcial" ao nosso projeto. As classes parciais podem ser usadas para adicionar métodos/Propriedades/eventos a classes mantidas por um designer do VS (como a classe de jantar gerada pelo designer de LINQ to SQL) e ajudar a evitar que a ferramenta seja bagunçada em nosso código. Podemos adicionar uma nova classe parcial ao nosso projeto clicando com o botão direito do mouse na pasta \Models e selecionando o comando de menu "Adicionar novo item". Em seguida, podemos escolher o modelo "classe" na caixa de diálogo "Adicionar novo item" e nomeá-lo Dinner.cs.

![](build-a-model-with-business-rule-validations/_static/image11.png)

Clicar no botão "Adicionar" adicionará um arquivo Dinner.cs ao nosso projeto e o abrirá no IDE. Em seguida, podemos implementar uma estrutura básica de imposição de regra/validação usando o código abaixo:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample10.cs)]

Algumas observações sobre o código acima:

- A classe jantar é precedida por uma palavra-chave "partial", o que significa que o código contido nela será combinado com a classe gerada/mantida pelo designer de LINQ to SQL e compilada em uma única classe.
- A classe RuleViolation é uma classe auxiliar que adicionaremos ao projeto que nos permite fornecer mais detalhes sobre uma violação de regra.
- O método jantar. GetRuleViolations () faz com que nossa validação e regras de negócio sejam avaliadas (Vamos implementá-las em breve). Em seguida, ele retorna uma sequência de objetos RuleViolation que fornecem mais detalhes sobre quaisquer erros de regra.
- A propriedade jantar. IsValid fornece uma propriedade auxiliar conveniente que indica se o objeto de jantar tem qualquer RuleViolations ativa. Ele pode ser verificado proativamente por um desenvolvedor usando o objeto de jantar a qualquer momento (e não gera uma exceção).
- O método parcial jantar. OnValidate () é um gancho que LINQ to SQL fornece que nos permite ser notificado sempre que o objeto do jantar está prestes a ser persistido no banco de dados. Nossa implementação OnValidate () acima garante que o jantar não tenha nenhum RuleViolations antes de ser salvo. Se estiver em um estado inválido, ele gerará uma exceção, o que fará LINQ to SQL anular a transação.

Essa abordagem fornece uma estrutura simples para a qual podemos integrar regras de validação e de negócios. Por enquanto, vamos adicionar as regras abaixo ao nosso método GetRuleViolations ():

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample11.cs)]

Estamos usando o recurso "yield return" do C# para retornar uma sequência de qualquer RuleViolations. As primeiras seis verificações de regra acima simplesmente impõem que as propriedades de cadeia de caracteres em nosso jantar não podem ser nulas ou vazias. A última regra é um pouco mais interessante e chama um método auxiliar PhoneValidator. IsValidNumber () que podemos adicionar ao nosso projeto para verificar se o formato de número ContactPhone corresponde ao país do jantar.

Podemos usar. O suporte à expressão regular da rede para implementar esse suporte à validação de telefone. Veja abaixo uma implementação simples de PhoneValidator que podemos adicionar ao nosso projeto que nos permite adicionar verificações de padrão Regex específicas de país:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample12.cs)]

#### <a name="handling-validation-and-business-logic-violations"></a>Tratamento de violações de validação e lógica de negócios

Agora que adicionamos a validação acima e o código de regra de negócio, sempre que tentarmos criar ou atualizar um jantar, nossas regras de lógica de validação serão avaliadas e impostas.

Os desenvolvedores podem escrever um código como abaixo para determinar proativamente se um objeto de jantar é válido e recuperar uma lista de todas as violações sem gerar nenhuma exceção:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample13.cs)]

Se tentarmos salvar um jantar em um estado inválido, uma exceção será gerada quando chamarmos o método Save () no DinnerRepository. Isso ocorre porque LINQ to SQL chama automaticamente nosso método parcial de jantar. OnValidate () antes de salvar as alterações do jantar e adicionamos o código a jantar. OnValidate () para gerar uma exceção se existirem violações de regra no jantar. Podemos capturar essa exceção e recuperar de forma reativa uma lista das violações a serem corrigidas:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample14.cs)]

Como nossas regras de validação e de negócios são implementadas em nossa camada de modelo, e não na camada de interface do usuário, elas serão aplicadas e usadas em todos os cenários em nosso aplicativo. Posteriormente, podemos alterar ou adicionar regras de negócio e ter todo o código que funciona com nossos objetos de jantar honra-las.

Ter a flexibilidade de alterar as regras de negócio em um só lugar, sem ter essas alterações propagando em toda a lógica do aplicativo e da interface do usuário, é um sinal de um aplicativo bem escrito e um benefício que uma estrutura MVC ajuda a incentivar.

### <a name="next-step"></a>Próxima etapa

Agora temos um modelo que podemos usar para consultar e atualizar nosso banco de dados.

Agora, vamos adicionar alguns controladores e exibições ao projeto que podemos usar para criar uma experiência de interface do usuário em HTML.

> [!div class="step-by-step"]
> [Anterior](create-a-database.md)
> [Próximo](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
