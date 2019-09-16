---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: 'Tutorial: Saiba mais sobre cenários avançados do EF para um aplicativo Web MVC 5'
description: Este tutorial inclui um apresenta vários tópicos que são úteis para estar atento quando você vai além dos conceitos básicos do desenvolvimento de aplicativos Web ASP.NET que usam Entity Framework Code First.
author: tdykstra
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: f35a9b0c-49ef-4cde-b06d-19d1543feb0b
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: d7cc83a5b78a60f575f5c3065079679189296a0c
ms.sourcegitcommit: 4b324a11131e38f920126066b94ff478aa9927f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/16/2019
ms.locfileid: "58425269"
---
# <a name="tutorial-learn-about-advanced-ef-scenarios-for-an-mvc-5-web-app"></a>Tutorial: Saiba mais sobre cenários avançados do EF para um aplicativo Web MVC 5

No tutorial anterior, você implementou a herança de tabela por hierarquia. Este tutorial inclui um apresenta vários tópicos que são úteis para estar atento quando você vai além dos conceitos básicos do desenvolvimento de aplicativos Web ASP.NET que usam Entity Framework Code First. As primeiras seções têm instruções passo a passo que orientam você pelo código e usando o Visual Studio para concluir tarefas. as seções a seguir apresentam vários tópicos com Breves introduções seguidas por links para recursos para obter mais informações.

Para a maioria desses tópicos, você trabalhará com páginas que já criou. Para usar o SQL bruto para fazer atualizações em massa, você criará uma nova página que atualiza o número de créditos de todos os cursos no banco de dados:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

Neste tutorial, você:

> [!div class="checklist"]
> * Executar consultas SQL brutas
> * Executar consultas sem controle
> * Examinar consultas SQL enviadas ao banco de dados

Você também aprenderá sobre:

> [!div class="checklist"]
> * Criando uma camada de abstração
> * Classes de proxy
> * Detecção automática de alterações
> * Validação automática
> * Entity Framework Power Tools
> * Entity Framework código-fonte

## <a name="prerequisite"></a>Pré-requisito

* [Implementação de herança](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="perform-raw-sql-queries"></a>Executar consultas SQL brutas

A API de Code First de Entity Framework inclui métodos que permitem passar comandos SQL diretamente para o banco de dados. Você tem as seguintes opções:

- Use o método [DbSet. SQLQuery](https://msdn.microsoft.com/library/system.data.entity.dbset.sqlquery.aspx) para consultas que retornam tipos de entidade. Os objetos retornados devem ser do tipo esperado pelo `DbSet` objeto e são rastreados automaticamente pelo contexto do banco de dados, a menos que você desative o rastreamento. (Consulte a seção a seguir sobre o método [AsNoTracking](https://msdn.microsoft.com/library/system.data.entity.dbextensions.asnotracking.aspx) .)
- Use o método [Database. SQLQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery.aspx) para consultas que retornam tipos que não são entidades. Os dados retornados não são controlados pelo contexto de banco de dados, mesmo se esse método é usado para recuperar tipos de entidade.
- Use o [Database. ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456.aspx) para comandos que não são de consulta.

Uma das vantagens de usar o Entity Framework é que ele evita vincular o código de forma muito próxima a um método específico de armazenamento de dados. Ele faz isso pela geração de consultas SQL e comandos para você, que também libera você da necessidade de escrevê-los. Mas há cenários excepcionais quando você precisa executar consultas SQL específicas que você criou manualmente, e esses métodos possibilitam que você manipule essas exceções.

Como é sempre verdadeiro quando você executa comandos SQL em um aplicativo Web, é necessário tomar precauções para proteger o site contra ataques de injeção de SQL. Uma maneira de fazer isso é usar consultas parametrizadas para garantir que as cadeias de caracteres enviadas por uma página da Web não possam ser interpretadas como comandos SQL. Neste tutorial, você usará consultas parametrizadas ao integrar a entrada do usuário a uma consulta.

### <a name="calling-a-query-that-returns-entities"></a>Chamando uma consulta que retorna entidades

A [classe&lt;DbSet&gt; TEntity](https://msdn.microsoft.com/library/gg696460.aspx) fornece um método que você pode usar para executar uma consulta que retorna uma entidade do tipo `TEntity`. Para ver como isso funciona, você alterará o código no `Details` método `Department` do controlador.

No *DepartmentController.cs*, no `Details` método, substitua a chamada `db.Departments.FindAsync` de método por uma `db.Departments.SqlQuery` chamada de método, conforme mostrado no seguinte código realçado:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs?highlight=8-14)]

Para verificar se o novo código funciona corretamente, selecione a guia **Departamentos** e, em seguida, **Detalhes** de um dos departamentos. Verifique se todos os dados são exibidos conforme o esperado.

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>Chamando uma consulta que retorna outros tipos de objetos

Anteriormente, você criou uma grade de estatísticas de alunos para a página Sobre que mostrava o número de alunos para cada data de registro. O código que faz isso no *HomeController.cs* usa LINQ:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

Suponha que você queira escrever o código que recupera esses dados diretamente no SQL em vez de usar o LINQ. Para fazer isso, você precisa executar uma consulta que retorne algo diferente de objetos de entidade, o que significa que você precisa usar o método [Database. SQLQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery(v=VS.103).aspx) .

No *HomeController.cs*, substitua a instrução LINQ no `About` método por uma instrução SQL, conforme mostrado no seguinte código realçado:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs?highlight=3-18)]

Execute a página sobre. Verifique se ele exibe os mesmos dados que antes.

### <a name="calling-an-update-query"></a>Chamando uma consulta Update

Suponha que os administradores da Contoso University desejam poder executar alterações em massa no banco de dados, como alterar o número de créditos de cada curso. Se a universidade tiver uma grande quantidade de cursos, poderá ser ineficiente recuperá-los como entidades e alterá-los individualmente. Nesta seção, você implementará uma página da Web que permite ao usuário especificar um fator pelo qual alterar o número de créditos de todos os cursos e fará a alteração executando uma instrução SQL `UPDATE` . 

No *CourseController.cs*, adicione `UpdateCourseCredits` métodos para `HttpGet` e `HttpPost`:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

Quando o controlador processa uma `HttpGet` solicitação, nada é retornado `ViewBag.RowsAffected` na variável e a exibição exibe uma caixa de texto vazia e um botão enviar.

Quando o botão de **atualização** é clicado `HttpPost` , o método é chamado `multiplier` e tem o valor inserido na caixa de texto. Em seguida, o código executa o SQL que atualiza os cursos e retorna o número de linhas afetadas para a `ViewBag.RowsAffected` exibição na variável. Quando a exibição Obtém um valor nessa variável, ela exibe o número de linhas atualizadas em vez da caixa de texto e do botão enviar.

No *CourseController.cs*, clique com o `UpdateCourseCredits` botão direito do mouse em um dos métodos e clique em **Adicionar exibição**. A caixa de diálogo **Adicionar exibição** é exibida. Deixe os padrões e selecione **Adicionar**.

No *Views\Course\UpdateCourseCredits.cshtml*, substitua o código do modelo pelo código a seguir:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cshtml)]

Execute o método `UpdateCourseCredits` selecionando a guia **Cursos**, adicionando, em seguida, "/UpdateCourseCredits" ao final da URL na barra de endereços do navegador (por exemplo: `http://localhost:50205/Course/UpdateCourseCredits`). Insira um número na caixa de texto:

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

Clique em **Atualizar**. Você verá o número de linhas afetadas.

Clique em **Voltar para a Lista** para ver a lista de cursos com o número revisado de créditos.

Para obter mais informações sobre consultas SQL brutas, consulte [consultas SQL brutas](https://msdn.microsoft.com/data/jj592907) no msdn.

## <a name="no-tracking-queries"></a>Consultas sem controle

Quando um contexto de banco de dados recupera linhas de tabela e cria objetos de entidade que as representam, por padrão, ele controla se as entidades em memória estão em sincronia com o que está no banco de dados. Os dados em memória atuam como um cache e são usados quando uma entidade é atualizada. Esse cache costuma ser desnecessário em um aplicativo Web porque as instâncias de contexto são normalmente de curta duração (uma nova é criada e descartada para cada solicitação) e o contexto que lê uma entidade normalmente é descartado antes que essa entidade seja usada novamente.

Você pode desabilitar o rastreamento de objetos de entidade na memória usando o método [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) . Os cenários típicos em que talvez você deseje fazer isso incluem os seguintes:

- Uma consulta recupera um grande volume de dados que desativar o controle pode melhorar o desempenho de forma perceptível.
- Você deseja anexar uma entidade para atualizá-la, mas anteriormente você recuperou a mesma entidade para uma finalidade diferente. Como a entidade já está sendo controlada pelo contexto de banco de dados, não é possível anexar a entidade que você deseja alterar. Uma maneira de lidar com essa situação é usar a `AsNoTracking` opção com a consulta anterior.

Para obter um exemplo que demonstra como usar o método [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) , consulte [a versão anterior deste tutorial](../../older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Esta versão do tutorial não define o sinalizador modificado em uma entidade de modelo criado pelo fichário no método editar, de modo que ele não precisa `AsNoTracking`.

## <a name="examine-sql-sent-to-database"></a>Examinar o SQL enviado ao banco de dados

Às vezes, é útil poder ver as consultas SQL reais que são enviadas ao banco de dados. Em um tutorial anterior, você viu como fazer isso no código do Interceptor; Agora você verá algumas maneiras de fazer isso sem escrever o código do Interceptor. Para testar isso, você examinará uma consulta simples e examinará o que acontece com ela à medida que adiciona opções, como carregamento, filtragem e classificação adiantados.

Em *controladores/CourseController*, substitua o `Index` método pelo código a seguir para interromper temporariamente o carregamento adiantado:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

Agora, defina um ponto de `return` interrupção na instrução (F9 com o cursor nessa linha). Pressione **F5** para executar o projeto no modo de depuração e selecione a página de índice do curso. Quando o código atingir o ponto de interrupção, `sql` examine a variável. Você vê a consulta que é enviada para SQL Server. É uma instrução simples `Select` .

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.json)]

Clique na lupa para ver a consulta no **Visualizador de texto**.

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

Agora você adicionará uma lista suspensa à página de índice de cursos para que os usuários possam filtrar um departamento específico. Você classificará os cursos por título e especificará o carregamento adiantado para a `Department` propriedade de navegação.

No *CourseController.cs*, substitua o `Index` método pelo código a seguir:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

Restaure o ponto de interrupção `return` na instrução.

O método recebe o valor selecionado da lista suspensa no `SelectedDepartment` parâmetro. Se nada estiver selecionado, esse parâmetro será NULL.

Uma `SelectList` coleção que contém todos os departamentos é passada para a exibição da lista suspensa. Os parâmetros passados para o `SelectList` Construtor especificam o nome do campo de valor, o nome do campo de texto e o item selecionado.

Para o `Get` método `Course` do repositório, o código especifica uma expressão de filtro, uma ordem de classificação e carregamento adiantado para `Department` a propriedade de navegação. A expressão de filtro sempre `true` retorna se nada estiver selecionado na lista suspensa (ou seja, `SelectedDepartment` for nulo).

No *Views\Course\Index.cshtml*, imediatamente antes da marca `table` de abertura, adicione o seguinte código para criar a lista suspensa e um botão enviar:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

Com o ponto de interrupção ainda definido, execute a página de índice do curso. Continue na primeira vez que o código atingir um ponto de interrupção, para que a página seja exibida no navegador. Selecione um departamento na lista suspensa e clique em **Filtrar**.

Desta vez, o primeiro ponto de interrupção será para a consulta de departamentos para a lista suspensa. Pule e exiba a `query` variável na próxima vez em que o código atingir o ponto de interrupção para ver a aparência da `Course` consulta agora. Você verá algo semelhante ao seguinte:

[!code-sql[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.sql)]

Você pode ver que a consulta agora é uma `JOIN` consulta que carrega `Department` dados junto com os `Course` dados e que ele inclui uma `WHERE` cláusula.

Remova a `var sql = courses.ToString()` linha.

## <a name="create-an-abstraction-layer"></a>Criar uma camada de abstração

Muitos desenvolvedores escrevem um código para implementar padrões de repositório e unidade de trabalho como um wrapper em torno do código que funciona com o Entity Framework. Esses padrões destinam-se a criar uma camada de abstração entre a camada de acesso a dados e a camada da lógica de negócios de um aplicativo. A implementação desses padrões pode ajudar a isolar o aplicativo de alterações no armazenamento de dados e pode facilitar o teste de unidade automatizado ou TDD (desenvolvimento orientado por testes). No entanto, escrever código adicional para implementar esses padrões nem sempre é a melhor opção para aplicativos que usam o EF, por vários motivos:

- A própria classe de contexto do EF isola o código de código específico a um armazenamento de dados.
- A classe de contexto do EF pode atuar como uma classe de unidade de trabalho para as atualizações de banco de dados feitas com o EF.
- Os recursos introduzidos no Entity Framework 6 facilitam a implementação do TDD sem escrever o código do repositório.

Para obter mais informações sobre como implementar o repositório e os padrões de unidade de trabalho, consulte [a versão Entity Framework 5 desta série de tutoriais](../../older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md). Para obter informações sobre maneiras de implementar o TDD no Entity Framework 6, consulte os seguintes recursos:

- [Como o EF6 permite a simulação de DbSets mais facilmente](http://thedatafarm.com/data-access/how-ef6-enables-mocking-dbsets-more-easily/)
- [Testando com uma estrutura fictícia](https://msdn.microsoft.com/data/dn314429)
- [Testando com suas próprias duplicatas de teste](https://msdn.microsoft.com/data/dn314431)

<a id="proxies"></a>

## <a name="proxy-classes"></a>Classes de proxy

Quando o Entity Framework cria instâncias de entidade (por exemplo, quando você executa uma consulta), ele geralmente as cria como instâncias de um tipo derivado gerado dinamicamente que atua como um proxy para a entidade. Por exemplo, consulte as duas imagens de depurador a seguir. Na primeira imagem, você verá que a `student` variável é o tipo esperado `Student` imediatamente depois de instanciar a entidade. Na segunda imagem, depois que o EF tiver sido usado para ler uma entidade de aluno do banco de dados, você verá a classe proxy.

![Antes da classe de proxy](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

![Depois da classe proxy](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

Essa classe de proxy substitui algumas propriedades virtuais da entidade para inserir ganchos para executar ações automaticamente quando a propriedade é acessada. Uma função para a qual esse mecanismo é usado é o carregamento lento.

Na maioria das vezes, você não precisa estar ciente desse uso de proxies, mas há exceções:

- Em alguns cenários, talvez você queira impedir que o Entity Framework Crie instâncias de proxy. Por exemplo, quando você estiver serializando entidades, geralmente você desejará as classes POCO, não as classes de proxy. Uma maneira de evitar problemas de serialização é serializar os DTOs (objetos de transferência de dados) em vez de objetos de entidade, conforme mostrado no tutorial [usando a API Web com Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-1.md) . Outra maneira é [desabilitar a criação de proxy](https://msdn.microsoft.com/data/jj592886.aspx).
- Ao instanciar uma classe de entidade usando `new` o operador, você não obtém uma instância de proxy. Isso significa que você não obtém funcionalidade como carregamento lento e controle de alterações automático. Normalmente, isso é ok; Geralmente, você não precisa de carregamento lento, pois você está criando uma nova entidade que não está no banco de dados e, em geral, não precisa de controle de alterações se `Added`estiver marcando explicitamente a entidade como. No entanto, se você precisar de carregamento lento e precisar de controle de alterações, poderá criar novas instâncias de entidade com proxies usando o método `DbSet` [Create](https://msdn.microsoft.com/library/gg679504.aspx) da classe.
- Talvez você queira obter um tipo de entidade real de um tipo de proxy. Você pode usar o método [GetObjectType](https://msdn.microsoft.com/library/system.data.objects.objectcontext.getobjecttype.aspx) da `ObjectContext` classe para obter o tipo de entidade real de uma instância de tipo de proxy.

Para obter mais informações, consulte [trabalhando com proxies](https://msdn.microsoft.com/data/JJ592886.aspx) no msdn.

## <a name="automatic-change-detection"></a>Detecção automática de alterações

O Entity Framework determina como uma entidade foi alterada (e, portanto, quais atualizações precisam ser enviadas ao banco de dados), comparando os valores atuais de uma entidade com os valores originais. Os valores originais são armazenados quando a entidade é consultada ou anexada. Alguns dos métodos que causam a detecção automática de alterações são os seguintes:

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

Se você estiver acompanhando um grande número de entidades e chamar um desses métodos muitas vezes em um loop, poderá obter melhorias de desempenho significativas temporariamente desativando a detecção automática de alterações usando a propriedade [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled.aspx) . Para obter mais informações, consulte [detectando alterações automaticamente](https://msdn.microsoft.com/data/jj556205) no msdn.

## <a name="automatic-validation"></a>Validação automática

Quando você chamar o `SaveChanges` método, por padrão, o Entity Framework validará os dados em todas as propriedades de todas as entidades alteradas antes de atualizar o banco de dados. Se você atualizou um grande número de entidades e já validou os dados, esse trabalho é desnecessário e você pode fazer com que o processo de salvar as alterações demore menos tempo desligando temporariamente a validação. Você pode fazer isso usando a propriedade [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled.aspx) . Para obter mais informações, consulte [validação](https://msdn.microsoft.com/data/gg193959) no msdn.

## <a name="entity-framework-power-tools"></a>Entity Framework Power Tools

O [Entity Framework Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) é um suplemento do Visual Studio que foi usado para criar os diagramas de modelo de dados mostrados nesses tutoriais. As ferramentas também podem fazer outras funções, como gerar classes de entidade com base nas tabelas de um banco de dados existente para que você possa usar o banco de dados com Code First. Depois de instalar as ferramentas, algumas opções adicionais aparecem nos menus de contexto. Por exemplo, quando você clica com o botão direito do mouse em sua classe de contexto no **Gerenciador de soluções**, você vê e **Entity Framework** opção. Isso lhe dá a capacidade de gerar um diagrama. Quando você estiver usando Code First não é possível alterar o modelo de dados no diagrama, mas pode mover as coisas para facilitar a compreensão.

![Diagrama do EF](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image15.png)

## <a name="entity-framework-source-code"></a>Entity Framework código-fonte

O código-fonte para o Entity Framework 6 está disponível no [GitHub](https://github.com/aspnet/EntityFramework6). Você pode arquivar bugs e pode contribuir com seus próprios aprimoramentos no código-fonte do EF.

Embora o código-fonte esteja aberto, Entity Framework tem suporte completo como um produto da Microsoft. A equipe do Microsoft Entity Framework mantém controle sobre quais contribuições são aceitas e testa todas as alterações de código para garantir a qualidade de cada versão.

## <a name="acknowledgments"></a>Agradecimentos

- Tom Dykstra escreveu a versão original deste tutorial, coautoria a atualização do EF 5 e escreveu a atualização do EF 6. José é um escritor de programação sênior da equipe de conteúdo da Microsoft Web Platform e Tools.
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (Twitter [@RickAndMSFT](http://twitter.com/RickAndMSFT)) fez a maior parte do trabalho atualizando o tutorial para o EF 5 e o MVC 4 e coautoria a atualização do EF 6. Rick é um escritor de programação sênior para a Microsoft focado no Azure e no MVC.
- A [Rowan Miller](http://www.romiller.com) e outros membros da equipe de Entity Framework assistida pelas revisões de código e ajudaram a depurar muitos problemas com as migrações que surgiram enquanto atualizamos o tutorial para o EF 5 e o EF 6.

## <a name="troubleshoot-common-errors"></a>Solucionar erros comuns

### <a name="cannot-createshadow-copy"></a>Não é possível criar/copiar sombra

Mensagem de erro:

> Não é possível criar uma cópia&lt;de&gt;sombra ' FileName ' quando esse arquivo já existe.

Solução

Aguarde alguns segundos e atualize a página.

### <a name="update-database-not-recognized"></a>Atualização-banco de dados não reconhecido

Mensagem de erro (do `Update-Database` comando no PMC):

> O termo ' Update-Database ' não é reconhecido como o nome de um cmdlet, função, arquivo de script ou programa operável. Verifique a ortografia do nome ou, se um caminho foi incluído, verifique se o caminho está correto e tente novamente.

Solução

Saia do Visual Studio. Reabra o projeto e tente novamente.

### <a name="validation-failed"></a>Falha na validação

Mensagem de erro (do `Update-Database` comando no PMC):

> Falha na validação de uma ou mais entidades. Consulte a propriedade ' EntityValidationErrors ' para obter mais detalhes.

Solução

Uma causa desse problema é erros de validação quando o `Seed` método é executado. Consulte [bancos de Entity Framework de propagação e depuração (EF)](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) para obter dicas sobre `Seed` como depurar o método.

### <a name="http-50019-error"></a>Erro HTTP 500,19

Mensagem de erro:

> Erro HTTP 500,19-erro interno do servidor a página solicitada não pode ser acessada porque os dados de configuração relacionados para a página são inválidos.

Solução

Uma maneira de obter esse erro é ter várias cópias da solução, cada uma delas usando o mesmo número de porta. Normalmente, você pode resolver esse problema saindo de todas as instâncias do Visual Studio e, em seguida, reiniciando o projeto no qual você está trabalhando. Se isso não funcionar, tente alterar o número da porta. Clique com o botão direito do mouse no arquivo de projeto e clique em Propriedades. Selecione a guia **Web** e, em seguida, altere o número da porta na caixa de texto **URL do projeto** .

### <a name="error-locating-sql-server-instance"></a>Erro ao localizar a instância do SQL Server

Mensagem de erro:

> Ocorreu um erro relacionado à rede ou específico a uma instância ao estabelecer uma conexão com o SQL Server. O servidor não foi encontrado ou não estava acessível. Verifique se o nome da instância está correto e se o SQL Server está configurado para permitir conexões remotas. (provedor: Adaptadores de Rede do SQL, erro: 26 – Erro ao localizar a instância/o servidor especificado)

Solução

Verifique a cadeia de conexão. Se você tiver excluído o banco de dados manualmente, altere o nome do banco de dados na cadeia de caracteres de construção.

## <a name="get-the-code"></a>Obter o código

[Baixar projeto concluído](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Recursos adicionais

 Para obter mais informações sobre como trabalhar com dados usando o Entity Framework, consulte a [página de documentação do EF no MSDN](https://msdn.microsoft.com/data/ee712907) e [ASP.NET Data Access – recursos recomendados](../../../../whitepapers/aspnet-data-access-content-map.md).

Para obter mais informações sobre como implantar seu aplicativo Web depois de tê-lo criado, consulte [ASP.NET Web Deployment-recursos recomendados](../../../../whitepapers/aspnet-web-deployment-content-map.md) na biblioteca do MSDN.

Para obter informações sobre outros tópicos relacionados ao MVC, como autenticação e autorização, consulte os [recursos recomendados do ASP.NET MVC](../recommended-resources-for-mvc.md).

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você:

> [!div class="checklist"]
> * Executou consultas SQL brutas
> * Consultas sem rastreamento executadas
> * Consultas SQL examinadas enviadas ao banco de dados

Você também aprendeu sobre:

> [!div class="checklist"]
> * Criando uma camada de abstração
> * Classes de proxy
> * Detecção automática de alterações
> * Validação automática
> * Entity Framework Power Tools
> * Entity Framework código-fonte

Isso conclui esta série de tutoriais sobre como usar o Entity Framework em um aplicativo MVC ASP.NET. Se você quiser saber mais sobre o EF Database First, consulte a série de tutoriais do BD First.
> [!div class="nextstepaction"]
> [Entity Framework Database First](../database-first-development/setting-up-database.md)