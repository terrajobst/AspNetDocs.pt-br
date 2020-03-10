---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: 'Tutorial: implementar a funcionalidade CRUD com o Entity Framework no MVC ASP.NET | Microsoft Docs'
description: Examine e personalize o código CRUD (criar, ler, atualizar, excluir) que o MVC scaffolding cria automaticamente em controladores e exibições.
author: tdykstra
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: a2f70ba4-83d1-4002-9255-24732726c4f2
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9ed388543dd54d209ff2a0b92df4f7659962582c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583156"
---
# <a name="tutorial-implement-crud-functionality-with-the-entity-framework-in-aspnet-mvc"></a>Tutorial: implementar a funcionalidade CRUD com o Entity Framework no MVC ASP.NET

No [tutorial anterior](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md), você criou um aplicativo MVC que armazena e exibe dados usando o Entity Framework (EF) 6 e SQL Server LocalDB. Neste tutorial, você examina e personaliza o código CRUD (criar, ler, atualizar, excluir) que o MVC scaffolding cria automaticamente para você em controladores e exibições.

> [!NOTE]
> É uma prática comum implementar o padrão de repositório para criar uma camada de abstração entre o controlador e a camada de acesso a dados. Para manter esses tutoriais simples e focados em ensinar como usar o EF 6 em si, eles não usam repositórios. Para obter informações sobre como implementar repositórios, consulte o [mapa de conteúdo de acesso a dados do ASP.net](../../../../whitepapers/aspnet-data-access-content-map.md).

Aqui estão exemplos das páginas da Web que você cria:

![Uma captura de tela da página de detalhes do aluno.](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Uma captura de tela da página de criação de aluno.](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Uma captura de tela para a página de exclusão do aluno.](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

Neste tutorial, você:

> [!div class="checklist"]
> * Criar uma página de detalhes
> * Atualizar a página Criar
> * Atualizar o método de edição HttpPost
> * Atualizar a página Excluir
> * Fechará conexões de banco de dados
> * Lidar com transações

## <a name="prerequisites"></a>Prerequisites

* [Criar o modelo de dados Entity Framework](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)

## <a name="create-a-details-page"></a>Criar uma página de detalhes

O código com Scaffold para os alunos `Index` página saiu da propriedade `Enrollments`, pois essa propriedade contém uma coleção. Na página `Details`, você exibirá o conteúdo da coleção em uma tabela HTML.

No *Controllers\StudentController.cs*, o método de ação para a exibição `Details` usa o método [Find](https://msdn.microsoft.com/library/gg696418(v=VS.103).aspx) para recuperar uma única entidade `Student`.

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

O valor da chave é passado para o método como o parâmetro `id` e vem de *dados de rota* no hiperlink **detalhes** na página de índice.

### <a name="tip-route-data"></a>Dica: **rotear dados**

Os dados de rota são dados que o associador de modelo encontrou em um segmento de URL especificado na tabela de roteamento. Por exemplo, a rota padrão especifica `controller`, `action`e `id` segmentos:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cs?highlight=3)]

Na URL a seguir, a rota padrão mapeia `Instructor` como o `controller`, `Index` como `action` e 1 como `id`; Esses são valores de dados de rota.

`http://localhost:1230/Instructor/Index/1?courseID=2021`

`?courseID=2021` é um valor de cadeia de caracteres de consulta. O associador de modelo também funcionará se você passar o `id` como um valor de cadeia de caracteres de consulta:

`http://localhost:1230/Instructor/Index?id=1&CourseID=2021`

As URLs são criadas por `ActionLink` instruções na exibição do Razor. No código a seguir, o parâmetro `id` corresponde à rota padrão, portanto, `id` é adicionado aos dados da rota.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml)]

No código a seguir, `courseID` não corresponde a um parâmetro na rota padrão, portanto, ele é adicionado como uma cadeia de caracteres de consulta.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cshtml)]

### <a name="to-create-the-details-page"></a>Para criar a página de detalhes

1. Abra *Views\Student\Details.cshtml*.

   Cada campo é exibido usando um auxiliar `DisplayFor`, conforme mostrado no exemplo a seguir:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cshtml)]

2. Após o campo de `EnrollmentDate` e imediatamente antes da marcação de `</dl>` de fechamento, adicione o código realçado para exibir uma lista de registros, conforme mostrado no exemplo a seguir:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml?highlight=8-29)]

    Se o recuo de código estiver errado depois que você colar o código, pressione **ctrl**+**K**, **Ctrl**+**D** para formatá-lo.

    Esse código percorre as entidades na propriedade de navegação `Enrollments`. Para cada entidade `Enrollment` na propriedade, ela exibe o título e a classificação do curso. O título do curso é recuperado da entidade `Course` que é armazenada na propriedade de navegação `Course` da entidade `Enrollments`. Todos esses dados são recuperados automaticamente do banco de dado quando necessário. Em outras palavras, você está usando o carregamento lento aqui. Você não especificou o *carregamento adiantado* para a propriedade de navegação `Courses`, de modo que os registros não foram recuperados na mesma consulta que os alunos. Em vez disso, na primeira vez que você tentar acessar a propriedade de navegação `Enrollments`, uma nova consulta será enviada para o banco de dados para recuperar os mesmos. Você pode ler mais sobre carregamento lento e carregamento adiantado no tutorial [lendo dados relacionados](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) mais adiante nesta série.

3. Abra a página de detalhes iniciando o programa (**Ctrl**+**F5**), selecionando a guia **alunos** e clicando no link **detalhes** para Alexander Carson. (Se você pressionar **Ctrl**+**F5** enquanto o arquivo *Details. cshtml* estiver aberto, você receberá um erro http 400. Isso ocorre porque o Visual Studio tenta executar a página de detalhes, mas não foi alcançado a partir de um link que especifica o aluno a ser exibido. Se isso acontecer, remova "aluno/detalhes" da URL e tente novamente, ou feche o navegador, clique com o botão direito do mouse no projeto e clique em **exibir** > **exibição no navegador**.)

    Você vê a lista de cursos e notas para o aluno selecionado.

4. Feche o navegador.

## <a name="update-the-create-page"></a>Atualizar a página Criar

1. No *Controllers\StudentController.cs*, substitua o método de ação `Create` <xref:System.Web.Mvc.HttpPostAttribute> pelo código a seguir. Esse código adiciona um bloco de `try-catch` e remove `ID` do atributo <xref:System.Web.Mvc.BindAttribute> para o método com Scaffold:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=3,5-6,13-18)]

    Esse código adiciona a entidade `Student` criada pelo associador de modelo MVC do ASP.NET ao conjunto de entidades de `Students` e salva as alterações no banco de dados. O *associador de modelo* refere-se à funcionalidade MVC ASP.NET que facilita o trabalho com os dados enviados por um formulário; um associador de modelo converte valores de formulário postados em tipos CLR e os passa para o método de ação em parâmetros. Nesse caso, o associador de modelo instancia uma entidade `Student` para você usando valores de propriedade da coleção `Form`.

    Você removeu `ID` do atributo BIND porque `ID` é o valor de chave primária que SQL Server será definido automaticamente quando a linha for inserida. A entrada do usuário não define o valor `ID`.

    <a id="overpost"></a>

    ### <a name="security-warning---the-validateantiforgerytoken-attribute-helps-prevent-cross-site-request-forgery-attacks-it-requires-a-corresponding-htmlantiforgerytoken-statement-in-the-view-which-youll-see-later"></a>Aviso de segurança-o atributo `ValidateAntiForgeryToken` ajuda a impedir ataques [de solicitação entre sites forjado](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) . Ele requer uma instrução `Html.AntiForgeryToken()` correspondente na exibição, que você verá posteriormente.

    O atributo `Bind` é uma maneira de se proteger contra o *excesso de lançamentos* em cenários de criação. Por exemplo, suponha que a entidade `Student` inclui uma propriedade `Secret` que você não deseja que essa página da Web defina.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs?highlight=7)]

    Mesmo que você não tenha um campo de `Secret` na página da Web, um hacker poderia usar uma ferramenta como o [Fiddler](http://fiddler2.com/home)ou escrever algum JavaScript para postar um valor de formulário de `Secret`. Sem o atributo <xref:System.Web.Mvc.BindAttribute> limitando os campos que o associador de modelo usa quando cria uma instância de `Student`<em>,</em> o associador de modelo pega que `Secret` valor de formulário e o usamos para criar a instância de entidade `Student`. Em seguida, seja qual for o valor que o invasor especificou para o campo de formulário `Secret`, ele é atualizado no banco de dados. A imagem a seguir mostra a ferramenta Fiddler adicionando o campo de `Secret` (com o valor "overpost") aos valores de formulário postados.

    ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)

    Em seguida, o valor "OverPost" é adicionado com êxito à propriedade `Secret` da linha inserida, embora você nunca desejou que a página da Web pudesse definir essa propriedade.

    É melhor usar o parâmetro `Include` com o atributo `Bind` para campos de *lista* de permissões. Também é possível usar o parâmetro `Exclude` para os campos de lista *negra* que você deseja excluir. O motivo `Include` é mais seguro é que quando você adiciona uma nova propriedade à entidade, o novo campo não é automaticamente protegido por uma lista de `Exclude`.

    Você pode evitar a superpostagem em cenários de edição, lendo primeiro a entidade do banco de dados e, em seguida, chamando `TryUpdateModel`, passando uma lista de propriedades permitidas explícitas. Esse é o método usado nesses tutoriais.

    Uma maneira alternativa de evitar a superpostagem preferida por muitos desenvolvedores é usar modelos de exibição em vez de classes de entidade com associação de modelo. Inclua apenas as propriedades que você deseja atualizar no modelo de exibição. Depois que o associador de modelo do MVC for concluído, copie as propriedades do modelo de exibição para a instância da entidade, opcionalmente usando uma ferramenta como o [AutoMapper](http://automapper.org/). Use o banco de BD. Na instância da entidade para definir seu estado como inalterado e, em seguida, defina Property ("PropertyName"). IsModified para true em cada propriedade de entidade que está incluída no modelo de exibição. Esse método funciona nos cenários de edição e criação.

    Além do atributo `Bind`, o bloco de `try-catch` é a única alteração feita no código com Scaffold. Se uma exceção que é derivada de <xref:System.Data.DataException> é capturada enquanto as alterações estão sendo salvas, uma mensagem de erro genérica é exibida. Às vezes, as exceções <xref:System.Data.DataException> são causadas por algo externo ao aplicativo, em vez de por um erro de programação e, portanto, o usuário é aconselhado a tentar novamente. Embora não implementado nesta amostra, um aplicativo de qualidade de produção registrará a exceção em log. Para obter mais informações, consulte a seção **Log para informações** em [Monitoramento e telemetria (criando aplicativos de nuvem do mundo real com o Azure)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log).

    O código em *Views\Student\Create.cshtml* é semelhante ao que você viu em *Details. cshtml*, exceto pelo fato de que `EditorFor` e `ValidationMessageFor` auxiliares são usados para cada campo em vez de `DisplayFor`. Este é o código relevante:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cshtml)]

    *Create. cshtml* também inclui `@Html.AntiForgeryToken()`, que funciona com o atributo `ValidateAntiForgeryToken` no controlador para ajudar a evitar ataques de [solicitação entre sites forjada](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) .

    Nenhuma alteração é necessária em *Create. cshtml*.

2. Execute a página iniciando o programa, selecionando a guia **alunos** e, em seguida, clicando em **criar novo**.

3. Insira nomes e uma data inválida e clique em **criar** para ver a mensagem de erro.

    Essa é a validação do lado do servidor que você obtém por padrão. Em um tutorial posterior, você verá como adicionar atributos que geram código para validação no lado do cliente. O código realçado a seguir mostra a verificação de validação do modelo no método **Create** .

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs?highlight=1)]

4. Altere a data para um valor válido e clique em **Criar** para ver o novo aluno ser exibido na página **Índice**.

5. Feche o navegador.

## <a name="update-httppost-edit-method"></a>Atualizar método de edição HttpPost

1. Substitua o método de ação de `Edit` <xref:System.Web.Mvc.HttpPostAttribute> pelo seguinte código:

   [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

   > [!NOTE]
   > No *Controllers\StudentController.cs*, o método de `HttpGet Edit` (aquele sem o atributo `HttpPost`) usa o método `Find` para recuperar a entidade `Student` selecionada, como vimos no método `Details`. Não é necessário alterar esse método.

   Essas alterações implementam uma prática recomendada de segurança para evitar a [sobreposta](#overpost), o scaffolder gerou um atributo `Bind` e adicionou a entidade criada pelo associador de modelo ao conjunto de entidades com um sinalizador modificado. Esse código não é mais recomendado porque o atributo `Bind` limpa todos os dados pré-existentes em campos não listados no parâmetro `Include`. No futuro, o controlador MVC scaffolder será atualizado para que ele não gere atributos de `Bind` para métodos de edição.

   O novo código lê a entidade existente e chama <xref:System.Web.Mvc.Controller.TryUpdateModel%2A> para atualizar campos da entrada do usuário nos dados de formulário postados. O controle de alterações automático do Entity Framework define o sinalizador [EntityState. Modified](<xref:System.Data.EntityState.Modified>) na entidade. Quando o método [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) é chamado, o sinalizador <xref:System.Data.EntityState.Modified> faz com que a Entity Framework crie instruções SQL para atualizar a linha do banco de dados. Os [conflitos de simultaneidade](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) são ignorados e todas as colunas da linha de banco de dados são atualizadas, incluindo aquelas que o usuário não alterou. (Um tutorial posterior mostra como lidar com conflitos de simultaneidade e, se você quiser que apenas campos individuais sejam atualizados no banco de dados, poderá definir a entidade como [EntityState. inalterado](<xref:System.Data.EntityState.Unchanged>) e definir campos individuais como [EntityState. Modified](<xref:System.Data.EntityState.Modified>).)

   Para evitar sobrepostos, os campos que você deseja atualizá-los pela página de edição são colocados na lista de permissões no `TryUpdateModel` parâmetros. Atualmente, não há nenhum campo extra que está sendo protegido, mas listar os campos que você deseja que o associador de modelos associe garante que, se você adicionar campos ao modelo de dados no futuro, eles serão automaticamente protegidos até que você adicione-os aqui de forma explícita.

   Como resultado dessas alterações, a assinatura do método do método de edição HttpPost é igual ao método de edição HttpGet; Portanto, você renomeou o método EditPost.

   > [!TIP]
   >
   > **Estados de entidade e os métodos Attach e SaveChanges**
   >
   > O contexto de banco de dados controla se as entidades em memória estão em sincronia com suas linhas correspondentes no banco de dados, e essas informações determinam o que acontece quando você chama o método `SaveChanges`. Por exemplo, quando você passa uma nova entidade para o método [Add](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx) , o estado dessa entidade é definido como `Added`. Em seguida, quando você chama o método [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) , o contexto do banco de dados emite um comando SQL `INSERT`.
   >
   > Uma entidade pode estar em um dos seguintes [Estados](xref:System.Data.EntityState):
   >
   > - `Added`. A entidade ainda não existe no banco de dados. O método `SaveChanges` deve emitir uma instrução `INSERT`.
   > - `Unchanged`. Nada precisa ser feito com essa entidade pelo método `SaveChanges`. Ao ler uma entidade do banco de dados, a entidade começa com esse status.
   > - `Modified`. Alguns ou todos os valores de propriedade da entidade foram modificados. O método `SaveChanges` deve emitir uma instrução `UPDATE`.
   > - `Deleted`. A entidade foi marcada para exclusão. O método `SaveChanges` deve emitir uma instrução `DELETE`.
   > - `Detached`. A entidade não está sendo controlada pelo contexto de banco de dados.
   >
   > Em um aplicativo da área de trabalho, em geral, as alterações de estado são definidas automaticamente. Em um tipo de área de trabalho de aplicativo, você lê uma entidade e faz alterações em alguns de seus valores de propriedade. Isso faz com que seu estado da entidade seja alterado automaticamente para `Modified`. Em seguida, quando você chama `SaveChanges`, o Entity Framework gera uma instrução SQL `UPDATE` que atualiza apenas as propriedades reais que você alterou.
   >
   > A natureza desconectada dos aplicativos Web não permite essa sequência contínua. O [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) que lê uma entidade é descartado depois que uma página é renderizada. Quando o `HttpPost` método de ação `Edit` é chamado, uma nova solicitação é feita e você tem uma nova instância do [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx), portanto, você precisa definir manualmente o estado da entidade como `Modified.`, quando chama `SaveChanges`, o Entity Framework atualiza todas as colunas da linha do banco de dados, porque o contexto não tem como saber quais propriedades você alterou.
   >
   > Se você quiser que a instrução SQL `Update` atualize somente os campos que o usuário realmente alterou, poderá salvar os valores originais de alguma forma (como campos ocultos) para que fiquem disponíveis quando o método `HttpPost` `Edit` for chamado. Em seguida, você pode criar uma entidade `Student` usando os valores originais, chamar o método `Attach` com essa versão original da entidade, atualizar os valores da entidade para os novos valores e, em seguida, chamar `SaveChanges.` para obter mais informações, consulte [Estados de entidades e](/ef/ef6/saving/change-tracking/entity-state) [dados locais](/ef/ef6/querying/local-data)e SaveChanges.

   O código HTML e Razor em *Views\Student\Edit.cshtml* é semelhante ao que você viu em *Create. cshtml*, e nenhuma alteração é necessária.

2. Execute a página iniciando o programa, selecionando a guia **alunos** e clicando em um hiperlink **Editar** .

3. Altere alguns dos dados e clique em **Salvar**. Você vê os dados alterados na página de índice.

4. Feche o navegador.

## <a name="update-the-delete-page"></a>Atualizar a página Excluir

No *Controllers\StudentController.cs*, o código do modelo para o método <xref:System.Web.Mvc.HttpGetAttribute> `Delete` usa o método `Find` para recuperar a entidade `Student` selecionada, como vimos nos métodos `Details` e `Edit`. No entanto, para implementar uma mensagem de erro personalizada quando a chamada a `SaveChanges` falhar, você adicionará uma funcionalidade a esse método e à sua exibição correspondente.

Como você viu para operações de atualização e criação, as operações de exclusão exigem dois métodos de ação. O método que é chamado em resposta a uma solicitação GET exibe uma exibição que dá ao usuário a oportunidade de aprovar ou cancelar a operação de exclusão. Se o usuário aprová-la, uma solicitação POST será criada. Quando isso acontece, o método de `Delete` de `HttpPost` é chamado e, em seguida, esse método executa a operação de exclusão.

Você adicionará um bloco de `try-catch` ao método <xref:System.Web.Mvc.HttpPostAttribute> `Delete` para lidar com quaisquer erros que possam ocorrer quando o banco de dados for atualizado. Se ocorrer um erro, o método <xref:System.Web.Mvc.HttpPostAttribute> `Delete` chamará o método <xref:System.Web.Mvc.HttpGetAttribute> `Delete`, passando um parâmetro que indica que ocorreu um erro. O método <xref:System.Web.Mvc.HttpGetAttribute> `Delete` reexibe a página de confirmação junto com a mensagem de erro, dando ao usuário uma oportunidade de cancelar ou tentar novamente.

1. Substitua o método de ação de `Delete` <xref:System.Web.Mvc.HttpGetAttribute> pelo código a seguir, que gerencia o relatório de erros:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cs?highlight=1,7-10)]

    Esse código aceita um [parâmetro opcional](https://msdn.microsoft.com/library/dd264739.aspx) que indica se o método foi chamado após uma falha ao salvar as alterações. Esse parâmetro é `false` quando o método de `Delete` de `HttpGet` é chamado sem uma falha anterior. Quando é chamado pelo método `HttpPost` `Delete` em resposta a um erro de atualização do banco de dados, o parâmetro é `true` e uma mensagem de erro é passada para a exibição.

2. Substitua o método de ação de `Delete` <xref:System.Web.Mvc.HttpPostAttribute> (chamado `DeleteConfirmed`) pelo código a seguir, que executa a operação de exclusão real e captura quaisquer erros de atualização de banco de dados.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

    Esse código recupera a entidade selecionada e, em seguida, chama o método [Remove](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx) para definir o status da entidade como `Deleted`. Quando `SaveChanges` é chamado, um comando SQL `DELETE` é gerado. Você também alterou o nome do método de ação de `DeleteConfirmed` para `Delete`. O código com Scaffold chamado `HttpPost` método `Delete` `DeleteConfirmed` para dar ao método `HttpPost` uma assinatura exclusiva. (O CLR requer métodos sobrecarregados para ter parâmetros de método diferentes.) Agora que as assinaturas são exclusivas, você pode se manter com a Convenção MVC e usar o mesmo nome para os métodos `HttpPost` e `HttpGet` Delete.

    Se melhorar o desempenho em um aplicativo de alto volume for uma prioridade, você poderá evitar uma consulta SQL desnecessária para recuperar a linha, substituindo as linhas de código que chamam os métodos `Find` e `Remove` com o seguinte código:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample14.cs)]

    Esse código instancia uma entidade `Student` usando apenas o valor da chave primária e, em seguida, define o estado da entidade como `Deleted`. Isso é tudo o que o Entity Framework precisa para excluir a entidade.

    Como observado, o método `HttpGet` `Delete` não exclui os dados. Executar uma operação de exclusão em resposta a uma solicitação GET (ou, para esse caso, executar qualquer operação de edição, operação de criação ou qualquer outra operação que altere dados) cria um risco de segurança. Para obter mais informações, consulte [#46 de dicas MVC do ASP.net – não use os links de exclusão porque eles criam brechas de segurança](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) no blog de Stephen Walther.

3. No *Views\Student\Delete.cshtml*, adicione uma mensagem de erro entre o cabeçalho `h2` e o cabeçalho `h3`, conforme mostrado no exemplo a seguir:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample15.cshtml?highlight=2)]

4. Execute a página iniciando o programa, selecionando a guia **alunos** e clicando em um hiperlink **excluir** .

5. Escolha **excluir** na página que diz **tem certeza de que deseja excluir isso?** .

    A página de índice é exibida sem o aluno excluído. (Você verá um exemplo do código de tratamento de erros em ação no [tutorial de simultaneidade](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md).)

## <a name="close-database-connections"></a>Fechará conexões de banco de dados

Para fechar as conexões de banco de dados e liberar os recursos que eles contêm assim que possível, descarte a instância de contexto quando terminar. É por isso que o código com Scaffold fornece um método [Dispose](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx) no final da classe `StudentController` em *StudentController.cs*, conforme mostrado no exemplo a seguir:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample16.cs)]

A classe de `Controller` base já implementa a interface `IDisposable`, portanto, esse código simplesmente adiciona uma substituição ao método `Dispose(bool)` para descartar explicitamente a instância de contexto.

## <a name="handle-transactions"></a>Lidar com transações

Por padrão, o Entity Framework implementa transações de forma implícita. Em cenários em que você faz alterações em várias linhas ou tabelas e, em seguida, chama `SaveChanges`, a Entity Framework garante automaticamente que todas as suas alterações tenham êxito ou todas falham. Se algumas alterações forem feitas pela primeira vez e, em seguida, ocorrer um erro, essas alterações serão revertidas automaticamente. Para cenários em que você precisa de mais controle&mdash;por exemplo, se desejar incluir operações feitas fora do Entity Framework em uma transação&mdash;consulte [trabalhando com transações](/ef/ef6/saving/transactions).

## <a name="get-the-code"></a>Obter o código

[Baixar projeto concluído](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Recursos adicionais

Agora você tem um conjunto completo de páginas que executam operações CRUD simples para `Student` entidades. Você usou auxiliares MVC para gerar elementos de interface do usuário para campos de dados. Para obter mais informações sobre auxiliares MVC, consulte [renderizando um formulário usando auxiliares HTML](/previous-versions/aspnet/dd410596(v=vs.98)) (o artigo é para o MVC 3, mas ainda é relevante para MVC 5).

Links para outros recursos do EF 6 podem ser encontrados em [ASP.NET Data Access – recursos recomendados](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você:

> [!div class="checklist"]
> * Criou uma página de detalhes
> * Atualizou a página Criar
> * Atualizado o método de edição HttpPost
> * Atualizou a página Excluir
> * Fechou conexões de banco de dados
> * Transações manipuladas

Avance para o próximo artigo para saber como adicionar classificação, filtragem e paginação ao projeto.
> [!div class="nextstepaction"]
> [Classificação, filtragem e paginação](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
