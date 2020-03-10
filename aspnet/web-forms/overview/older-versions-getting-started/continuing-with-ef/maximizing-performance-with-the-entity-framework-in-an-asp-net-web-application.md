---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
title: Maximizando o desempenho com o Entity Framework 4,0 em um aplicativo Web ASP.NET 4 | Microsoft Docs
author: tdykstra
description: Esta série de tutoriais se baseia no aplicativo Web da Contoso University criado pelo Introdução com a série de tutoriais do Entity Framework 4,0. I...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 4e43455e-dfa1-42db-83cb-c987703f04b5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: 5630200a1ad1d30f6d89b38e15179f15b699fa9f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78545958"
---
# <a name="maximizing-performance-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Maximizando o desempenho com o Entity Framework 4,0 em um aplicativo Web ASP.NET 4

por [Tom Dykstra](https://github.com/tdykstra)

> Esta série de tutoriais se baseia no aplicativo Web da Contoso University criado pelo [introdução com a série de tutoriais do Entity Framework 4,0](https://asp.net/entity-framework/tutorials#Getting%20Started) . Se você não concluiu os tutoriais anteriores, como ponto de partida para este tutorial, você pode [baixar o aplicativo](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que você criou. Você também pode [baixar o aplicativo](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) que é criado pela série de tutoriais completa. Se você tiver dúvidas sobre os tutoriais, poderá lançá-los no [Fórum de Entity Framework ASP.net](https://forums.asp.net/1227.aspx).

No tutorial anterior, você viu como lidar com conflitos de simultaneidade. Este tutorial mostra opções para melhorar o desempenho de um aplicativo Web ASP.NET que usa o Entity Framework. Você aprenderá vários métodos para maximizar o desempenho ou para diagnosticar problemas de desempenho.

As informações apresentadas nas seções a seguir provavelmente serão úteis em uma ampla variedade de cenários:

- Carregar dados relacionados com eficiência.
- Gerenciar estado de exibição.

As informações apresentadas nas seções a seguir podem ser úteis se você tiver consultas individuais que apresentam problemas de desempenho:

- Use a opção `NoTracking` mesclar.
- Pré-compile consultas LINQ.
- Examine os comandos de consulta enviados para o banco de dados.

As informações apresentadas na seção a seguir são potencialmente úteis para aplicativos que têm modelos de dados extremamente grandes:

- Pré-gerar exibições.

> [!NOTE]
> O desempenho do aplicativo Web é afetado por muitos fatores, incluindo coisas como o tamanho dos dados da solicitação e da resposta, a velocidade das consultas de banco de dado, quantas solicitações o servidor pode enfileirar e com que rapidez ele pode atender a elas e até mesmo a eficiência de qualquer bibliotecas de script de cliente que você pode estar usando. Se o desempenho for crítico em seu aplicativo ou se o teste ou a experiência mostrar que o desempenho do aplicativo não é satisfatório, você deverá seguir o protocolo normal para ajuste de desempenho. Medida para determinar onde os afunilamentos de desempenho estão ocorrendo e, em seguida, abordar as áreas que terão o maior impacto no desempenho geral do aplicativo.
> 
> Este tópico se concentra principalmente em maneiras pelas quais você pode melhorar o desempenho especificamente do Entity Framework no ASP.NET. As sugestões aqui serão úteis se você determinar que o acesso a dados é um dos gargalos de desempenho em seu aplicativo. Exceto como observado, os métodos explicados aqui não devem ser considerados &quot;práticas recomendadas&quot; em geral — muitas delas são apropriadas apenas em situações excepcionais ou para lidar com tipos muito específicos de afunilamentos de desempenho.

Para iniciar o tutorial, inicie o Visual Studio e abra o aplicativo Web da Contoso University com o qual você estava trabalhando no tutorial anterior.

## <a name="efficiently-loading-related-data"></a>Carregando dados relacionados com eficiência

Há várias maneiras pelas quais o Entity Framework pode carregar dados relacionados nas propriedades de navegação de uma entidade:

- *Carregamento lento*. Quando a entidade é lida pela primeira vez, os dados relacionados não são recuperados. No entanto, na primeira vez que você tenta acessar uma propriedade de navegação, os dados necessários para essa propriedade de navegação são recuperados automaticamente. Isso resulta em várias consultas enviadas ao banco de dados — uma para a própria entidade e uma a cada vez que os dados relacionados para a entidade devem ser recuperados. 

    [![Image05](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

*Carregamento adiantado*. Quando a entidade é lida, os dados relacionados são recuperados com ela. Normalmente, isso resulta em uma única consulta de junção que recupera todos os dados necessários. Você especifica o carregamento adiantado usando o método `Include`, como já viu nestes tutoriais.

[![Image07](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

- *Carregamento explícito*. Isso é semelhante ao carregamento lento, exceto que você recupera explicitamente os dados relacionados no código; Ele não acontece automaticamente quando você acessa uma propriedade de navegação. Você carrega dados relacionados manualmente usando o método `Load` da propriedade de navegação para coleções ou usa o método `Load` da propriedade de referência para propriedades que contêm um único objeto. (Por exemplo, você chama o método `PersonReference.Load` para carregar a propriedade de navegação `Person` de uma entidade `Department`.)

    [![Image06](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

Como eles não recuperam imediatamente os valores de propriedade, o carregamento lento e o carregamento explícito também são conhecidos como *carregamento adiado*.

O carregamento lento é o comportamento padrão para um contexto de objeto que foi gerado pelo designer. Se você abrir o arquivo *SchoolModel.designer.cs* que define a classe de contexto de objeto, encontrará três métodos de construtor, e cada um deles incluirá a seguinte instrução:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

Em geral, se você souber que precisa de dados relacionados para cada entidade recuperada, o carregamento adiantado oferece o melhor desempenho, porque uma única consulta enviada ao banco de dados é normalmente mais eficiente do que consultas separadas para cada entidade recuperada. Por outro lado, se você precisar acessar as propriedades de navegação de uma entidade com pouca frequência ou apenas para um pequeno conjunto de entidades, o carregamento lento ou o carregamento explícito poderá ser mais eficiente, pois o carregamento adiantado recuperaria mais dados do que o necessário.

Em um aplicativo Web, o carregamento lento pode ser de um valor relativamente pequeno, pois as ações do usuário que afetam a necessidade de dados relacionados ocorrem no navegador, que não tem conexão com o contexto do objeto que renderiza a página. Por outro lado, quando você vincule um controle, normalmente sabe quais dados são necessários e, portanto, é melhor escolher o carregamento adiantado ou o carregamento adiado com base no que é apropriado em cada cenário.

Além disso, um controle de ligação de ligações pode usar um objeto de entidade após o contexto do objeto ser Descartado. Nesse caso, uma tentativa de carga lenta de uma propriedade de navegação falharia. A mensagem de erro recebida é clara: &quot;`The ObjectContext instance has been disposed and can no longer be used for operations that require a connection.`&quot;

O controle de `EntityDataSource` desabilita o carregamento lento por padrão. Para o controle de `ObjectDataSource` que você está usando para o tutorial atual (ou se você acessa o contexto de objeto do código de página), há várias maneiras pelas quais você pode tornar o carregamento lento desabilitado por padrão. Você pode desabilitá-lo ao criar uma instância de um contexto de objeto. Por exemplo, você pode adicionar a seguinte linha ao método construtor da classe `SchoolRepository`:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Para o aplicativo da Contoso University, você fará com que o contexto do objeto desabilite o carregamento lento automaticamente para que essa propriedade não precise ser definida sempre que um contexto for instanciado.

Abra o modelo de dados *SchoolModel. edmx* , clique na superfície de design e, no painel Propriedades, defina a propriedade de **carregamento lento habilitado** como `False`. Salve e feche o modelo de dados.

[![Image04](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

## <a name="managing-view-state"></a>Gerenciando o estado de exibição

Para fornecer a funcionalidade de atualização, uma página da Web ASP.NET deve armazenar os valores de propriedade originais de uma entidade quando uma página é renderizada. Durante o processamento de postback, o controle pode recriar o estado original da entidade e chamar o método de `Attach` da entidade antes de aplicar as alterações e chamar o método `SaveChanges`. Por padrão, os controles de dados do ASP.NET Web Forms usam o estado de exibição para armazenar os valores originais. No entanto, o estado de exibição pode afetar o desempenho, pois ele é armazenado em campos ocultos que podem aumentar substancialmente o tamanho da página que é enviada para e do navegador.

As técnicas para gerenciar o estado de exibição, ou alternativas como o estado da sessão, não são exclusivas do Entity Framework, portanto, este tutorial não entra em detalhes neste tópico. Para obter mais informações, consulte os links no final do tutorial.

No entanto, a versão 4 do ASP.NET fornece uma nova maneira de trabalhar com o estado de exibição de que todo desenvolvedor ASP.NET de aplicativos Web Forms deve estar ciente: a propriedade `ViewStateMode`. Essa nova propriedade pode ser definida na página ou no nível de controle e permite desabilitar o estado de exibição por padrão para uma página e habilitá-la somente para controles que precisam dela.

Para aplicativos em que o desempenho é crítico, uma prática recomendada é sempre desabilitar o estado de exibição no nível da página e habilitá-lo somente para controles que o exigem. O tamanho do estado de exibição nas páginas da Universidade da Contoso não seria substancialmente diminuído por esse método, mas para ver como funciona, você fará isso para a página *instrutores. aspx* . Essa página contém muitos controles, incluindo um controle de `Label` que tem o estado de exibição desabilitado. Nenhum dos controles nesta página realmente precisa ter o estado de exibição habilitado. (A propriedade `DataKeyNames` do controle de `GridView` especifica o estado que deve ser mantido entre postbacks, mas esses valores são mantidos no estado de controle, que não é afetado pela propriedade `ViewStateMode`.)

A diretiva de `Page` e a marcação de `Label` controle são semelhantes ao exemplo a seguir:

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

Faça as seguintes alterações:

- Adicione `ViewStateMode="Disabled"` à diretiva `Page`.
- Remova `ViewStateMode="Disabled"` do controle de `Label`.

A marcação agora é semelhante ao exemplo a seguir:

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

O estado de exibição agora está desabilitado para todos os controles. Se posteriormente você adicionar um controle que precisa usar o estado de exibição, tudo o que você precisa fazer é incluir o atributo `ViewStateMode="Enabled"` para esse controle.

## <a name="using-the-notracking-merge-option"></a>Usando a opção de mesclagem NoTracking

Quando um contexto de objeto recupera linhas de banco de dados e cria objetos de entidade que os representam, por padrão ele também rastreia esses objetos de entidade usando seu Gerenciador de estado de objeto. Esses dados de rastreamento atuam como um cache e são usados quando você atualiza uma entidade. Como um aplicativo Web normalmente tem instâncias de contexto de objeto de curta duração, as consultas geralmente retornam dados que não precisam ser controlados, pois o contexto de objeto que lê-los será descartado antes que qualquer uma das entidades que ele lê seja usada novamente ou atualizada.

No Entity Framework, você pode especificar se o contexto de objeto rastreia objetos de entidade definindo uma *opção de mesclagem*. Você pode definir a opção de mesclagem para consultas individuais ou para conjuntos de entidades. Se você defini-lo para um conjunto de entidades, isso significa que você está definindo a opção de mesclagem padrão para todas as consultas que são criadas para esse conjunto de entidades.

Para o aplicativo da Contoso University, o controle não é necessário para qualquer um dos conjuntos de entidades que você acessa do repositório, para que você possa definir a opção de mesclagem para `NoTracking` para esses conjuntos de entidades ao instanciar o contexto do objeto na classe Repository. (Observe que, neste tutorial, definir a opção de mesclagem não terá um efeito perceptível no desempenho do aplicativo. A opção de `NoTracking` provavelmente fará uma melhoria de desempenho observável apenas em determinados cenários de alto volume de dados.)

Na pasta DAL, abra o arquivo *SchoolRepository.cs* e adicione um método de construtor que defina a opção de mesclagem para os conjuntos de entidades que o repositório acessa:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

## <a name="pre-compiling-linq-queries"></a>Pré-compilando consultas LINQ

Na primeira vez que o Entity Framework executa uma consulta Entity SQL dentro da vida de uma determinada instância de `ObjectContext`, leva algum tempo para compilar a consulta. O resultado da compilação é armazenado em cache, o que significa que as execuções subsequentes da consulta são muito mais rápidas. Consultas LINQ seguem um padrão semelhante, exceto que parte do trabalho necessário para compilar a consulta é feita toda vez que a consulta é executada. Em outras palavras, para consultas LINQ, por padrão, nem todos os resultados da compilação são armazenados em cache.

Se você tiver uma consulta LINQ que espera ser executada repetidamente na vida de um contexto de objeto, você pode escrever código que faz com que todos os resultados da compilação sejam armazenados em cache na primeira vez em que a consulta LINQ é executada.

Como ilustração, você fará isso para dois métodos `Get` na classe `SchoolRepository`, um dos quais não pega parâmetros (o método `GetInstructorNames`) e outro que exija um parâmetro (o método `GetDepartmentsByAdministrator`). Esses métodos agora não precisam ser compilados, pois não são consultas LINQ:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

No entanto, para que você possa experimentar consultas compiladas, você continuará como se elas tivessem sido escritas como as seguintes consultas LINQ:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

Você pode alterar o código nesses métodos para o que é mostrado acima e executar o aplicativo para verificar se ele funciona antes de continuar. Mas as instruções a seguir vão diretamente para a criação de versões previamente compiladas.

Crie um arquivo de classe na pasta *Dal* , nomeie-o *SchoolEntities.cs*e substitua o código existente pelo código a seguir:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

Esse código cria uma classe parcial que estende a classe de contexto de objeto gerada automaticamente. A classe Partial inclui duas consultas do LINQ compiladas usando o método `Compile` da classe `CompiledQuery`. Ele também cria métodos que você pode usar para chamar as consultas. Salve e feche este arquivo.

Em seguida, em *SchoolRepository.cs*, altere os métodos existentes de `GetInstructorNames` e `GetDepartmentsByAdministrator` na classe Repository para que eles chamem as consultas compiladas:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

Execute a página *departamentos. aspx* para verificar se ela funciona como antes. O método `GetInstructorNames` é chamado para preencher a lista suspensa administrador e o método `GetDepartmentsByAdministrator` é chamado quando você clica em **Atualizar** para verificar se nenhum instrutor é um administrador de mais de um departamento.

[![Image03](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

Você já compilou consultas no aplicativo da Contoso University apenas para ver como fazer isso, não porque ela melhoradoia a melhorar o desempenho. A pré-compilação de consultas LINQ adiciona um nível de complexidade ao seu código, portanto, certifique-se de fazer isso apenas para consultas que realmente representam gargalos de desempenho em seu aplicativo.

## <a name="examining-queries-sent-to-the-database"></a>Examinando consultas enviadas ao banco de dados

Quando você estiver investigando problemas de desempenho, às vezes, é útil saber os comandos SQL exatos que o Entity Framework está enviando para o banco de dados. Se você estiver trabalhando com um objeto `IQueryable`, uma maneira de fazer isso é usar o método `ToTraceString`.

No *SchoolRepository.cs*, altere o código no método `GetDepartmentsByName` para corresponder ao exemplo a seguir:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.cs)]

A variável `departments` deve ser convertida em um tipo de `ObjectQuery` somente porque o método `Where` no final da linha anterior cria um objeto `IQueryable`; sem o método `Where`, a conversão não seria necessária.

Defina um ponto de interrupção na linha de `return` e, em seguida, execute a página Departments *. aspx* no depurador. Quando você atingir o ponto de interrupção, examine a variável `commandText` na janela **locais** e use o Visualizador de texto (a lupa na coluna **valor** ) para exibir seu valor na janela **Visualizador de texto** . Você pode ver todo o comando SQL que resulta desse código:

[![Image08](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

Como alternativa, o recurso IntelliTrace no Visual Studio Ultimate fornece uma maneira de exibir comandos SQL gerados pelo Entity Framework que não exige que você altere seu código ou até mesmo defina um ponto de interrupção.

> [!NOTE]
> Você só poderá executar os procedimentos a seguir se tiver Visual Studio Ultimate.

Restaure o código original no método `GetDepartmentsByName` e, em seguida, execute a página Departments *. aspx* no depurador.

No Visual Studio, selecione o menu **depurar** , depois **IntelliTrace**e, em seguida, **eventos do IntelliTrace**.

[![Image11](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

Na janela do **IntelliTrace** , clique em **interromper tudo**.

[![Image12](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

A janela do **IntelliTrace** exibe uma lista de eventos recentes:

[![Image09](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Clique na linha **ADO.net** . Ele se expande para mostrar o texto do comando:

[![Image10](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

Você pode copiar a cadeia de texto do comando inteiro para a área de transferência na janela **locais** .

Suponha que você estivesse trabalhando com um banco de dados com mais tabelas, relações e colunas do que o simples banco de dados de `School`. Você pode achar que uma consulta que reúne todas as informações necessárias em uma única instrução de `Select` que contém várias cláusulas `Join` se torna muito complexa para funcionar com eficiência. Nesse caso, você pode alternar do carregamento adiantado para o carregamento explícito para simplificar a consulta.

Por exemplo, tente alterar o código no método `GetDepartmentsByName` em *SchoolRepository.cs*. Atualmente, nesse método, você tem uma consulta de objeto que tem métodos `Include` para as propriedades de navegação `Person` e `Courses`. Substitua a instrução `return` pelo código que executa o carregamento explícito, conforme mostrado no exemplo a seguir:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Execute a página Departments *. aspx* no depurador e verifique a janela do **IntelliTrace** novamente como fazia antes. Agora, onde havia uma única consulta antes, você verá uma longa sequência delas.

[![Image13](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

Clique na primeira linha **ADO.net** para ver o que aconteceu com a consulta complexa que você exibiu anteriormente.

[![Image14](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

A consulta de departamentos tornou-se uma simples consulta de `Select` sem cláusula `Join`, mas é seguida por consultas separadas que recuperam cursos relacionados e um administrador, usando um conjunto de duas consultas para cada departamento retornado pela consulta original.

> [!NOTE]
> Se você deixar o carregamento lento habilitado, o padrão exibido aqui, com a mesma consulta repetida muitas vezes, pode resultar do carregamento lento. Um padrão que você normalmente deseja evitar é o carregamento lento de dados relacionados para cada linha da tabela primária. A menos que você tenha verificado que uma única consulta de junção é muito complexa para ser eficiente, normalmente seria possível melhorar o desempenho nesses casos alterando a consulta principal para usar o carregamento adiantado.

## <a name="pre-generating-views"></a>Pré-gerando exibições

Quando um objeto `ObjectContext` é criado pela primeira vez em um novo domínio de aplicativo, o Entity Framework gera um conjunto de classes que ele usa para acessar o banco de dados. Essas classes são chamadas de *exibições*e, se você tiver um modelo de dados muito grande, gerar essas exibições poderá atrasar a resposta do site para a primeira solicitação de uma página depois que um novo domínio de aplicativo for inicializado. Você pode reduzir esse atraso de primeira solicitação criando as exibições em tempo de compilação em vez de em tempo de execução.

> [!NOTE]
> Se seu aplicativo não tiver um modelo de dados muito grande ou se ele tiver um modelo de dados grande, mas você não estiver preocupado com um problema de desempenho que afeta apenas a primeira solicitação de página depois que o IIS for reciclado, você poderá ignorar esta seção. A criação da exibição não ocorre sempre que você cria uma instância de um objeto `ObjectContext`, pois as exibições são armazenadas em cache no domínio do aplicativo. Portanto, a menos que você esteja reciclando com frequência seu aplicativo no IIS, poucas solicitações de página se beneficiarão de exibições geradas previamente.

Você pode gerar previamente modos de exibição usando a ferramenta de linha de comando *EdmGen. exe* ou usando um modelo de *ferramentas de transformação de modelo de texto* (T4). Neste tutorial, você usará um modelo T4.

Na pasta *Dal* , adicione um arquivo usando o modelo de **modelo de texto** (ele está sob o nó **geral** na lista de **modelos instalados** ) e nomeie-o *SchoolModel.views.tt*. Substitua o código existente no arquivo pelo código a seguir:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

Esse código gera exibições para um arquivo *. edmx* localizado na mesma pasta que o modelo e que tem o mesmo nome que o arquivo de modelo. Por exemplo, se o arquivo de modelo for nomeado *SchoolModel.views.tt*, ele procurará um arquivo de modelo de dados chamado *SchoolModel. edmx*.

Salve o arquivo, clique com o botão direito do mouse no arquivo em **Gerenciador de soluções** e selecione **executar ferramenta personalizada**.

[![Image02](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

O Visual Studio gera um arquivo de código que cria os modos de exibição, nomeados *SchoolModel.views.cs* com base no modelo. (Talvez você tenha notado que o arquivo de código é gerado mesmo antes de selecionar **executar ferramenta personalizada**, assim que você salvar o arquivo de modelo.)

[![Image01](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

Agora você pode executar o aplicativo e verificar se ele funciona como fazia antes.

Para obter mais informações sobre exibições geradas previamente, consulte os seguintes recursos:

- [Como: gerar previamente modos de exibição para melhorar o desempenho da consulta](https://msdn.microsoft.com/library/bb896240.aspx) no site do MSDN. Explica como usar a ferramenta de linha de comando `EdmGen.exe` para gerar exibições previamente.
- [Isolamento de desempenho com exibições pré-compiladas/recém-gerado no Entity Framework 4](https://blogs.msdn.com/b/appfabriccat/archive/2010/08/06/isolating-performance-with-precompiled-pre-generated-views-in-the-entity-framework-4.aspx) no blog da equipe de consultoria do cliente do Windows Server AppFabric.

Isso conclui a introdução para melhorar o desempenho em um aplicativo Web ASP.NET que usa o Entity Framework. Para saber mais, consulte os recursos a seguir:

- [Considerações sobre desempenho (Entity Framework)](https://msdn.microsoft.com/library/cc853327.aspx) no site do MSDN.
- [Postagens relacionadas ao desempenho no blog da equipe do Entity Framework](https://blogs.msdn.com/b/adonet/archive/tags/performance/).
- [Opções de mesclagem do EF e consultas compiladas](https://blogs.msdn.com/b/dsimmons/archive/2010/01/12/ef-merge-options-and-compiled-queries.aspx). Postagem no blog que explica comportamentos inesperados de consultas compiladas e opções de mesclagem, como `NoTracking`. Se você planeja usar consultas compiladas ou manipular configurações de opção de mesclagem em seu aplicativo, leia isso primeiro.
- [Postagens relacionadas ao Entity Framework no blog da equipe de consultoria de dados e modelagem do cliente](https://blogs.msdn.com/b/dmcat/archive/tags/entity+framework/). Inclui postagens em consultas compiladas e uso do criador de perfil do Visual Studio 2010 para descobrir problemas de desempenho.
- [Entity Framework thread de fórum com conselhos sobre como melhorar o desempenho de consultas altamente complexas](https://social.msdn.microsoft.com/Forums/adodotnetentityframework/thread/ffe8b2ab-c5b5-4331-8988-33a872d0b5f6).
- [Recomendações de gerenciamento de estado ASP.net](https://msdn.microsoft.com/library/z1hkazw7.aspx).
- [Usando o Entity Framework e a paginação do ObjectDataSource: Custom](http://geekswithblogs.net/Frez/articles/using-the-entity-framework-and-the-objectdatasource-custom-paging.aspx). Postagem de blog que se baseia no aplicativo ContosoUniversity criado nesses tutoriais para explicar como implementar a paginação na página Departments *. aspx* .

O próximo tutorial examina alguns dos aprimoramentos importantes do Entity Framework que são novos na versão 4.

> [!div class="step-by-step"]
> [Anterior](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
> [Próximo](what-s-new-in-the-entity-framework-4.md)
