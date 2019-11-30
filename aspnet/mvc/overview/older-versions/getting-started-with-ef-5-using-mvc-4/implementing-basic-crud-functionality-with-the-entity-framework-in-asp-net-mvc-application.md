---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: Implementando a funcionalidade CRUD básica com o Entity Framework no aplicativo MVC ASP.NET (2 de 10) | Microsoft Docs
author: tdykstra
description: O aplicativo Web de exemplo da Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Entity Framework 5 Code First e o Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: f7bace3f-b85a-47ff-b5fe-49e81441cdf9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: eba146d2975e0dcf243facf7c205c4acfe40b6f6
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74595335"
---
# <a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application-2-of-10"></a>Implementando a funcionalidade CRUD básica com o Entity Framework no aplicativo MVC ASP.NET (2 de 10)

por [Tom Dykstra](https://github.com/tdykstra)

[Baixar projeto concluído](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> O aplicativo Web de exemplo da Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Entity Framework 5 Code First e o Visual Studio 2012. Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Você pode iniciar a série de tutoriais desde o início ou [baixar um projeto inicial para este capítulo](building-the-ef5-mvc4-chapter-downloads.md) e começar aqui.
> 
> > [!NOTE] 
> > 
> > Se você encontrar um problema que não possa resolver, [Baixe o capítulo concluído](building-the-ef5-mvc4-chapter-downloads.md) e tente reproduzir o problema. Em geral, você pode encontrar a solução para o problema comparando seu código com o código concluído. Para alguns erros comuns e como resolvê-los, consulte [erros e soluções alternativas.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)

No tutorial anterior, você criou um aplicativo MVC que armazena e exibe dados usando o Entity Framework e SQL Server LocalDB. Neste tutorial, você examinará e personalizará o código CRUD (criar, ler, atualizar, excluir) que o MVC scaffolding cria automaticamente para você em controladores e exibições.

> [!NOTE]
> É uma prática comum implementar o padrão de repositório para criar uma camada de abstração entre o controlador e a camada de acesso a dados. Para manter esses tutoriais simples, você não implementará um repositório até um tutorial posterior nesta série.

Neste tutorial, você criará as seguintes páginas da Web:

![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

![Student_delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

## <a name="creating-a-details-page"></a>Criando uma página de detalhes

O código com Scaffold para os alunos `Index` página saiu da propriedade `Enrollments`, pois essa propriedade contém uma coleção. Na página `Details`, você exibirá o conteúdo da coleção em uma tabela HTML.

 No *Controllers\StudentController.cs*, o método de ação para a exibição de `Details` usa o método `Find` para recuperar uma única entidade de `Student`. 

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

 O valor da chave é passado para o método como o parâmetro `id` e vem de dados de rota no hiperlink **detalhes** na página de índice. 

1. Abra *Views\Student\Details.cshtml*. Cada campo é exibido usando um auxiliar `DisplayFor`, conforme mostrado no exemplo a seguir: 

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cshtml)]
2. Após o campo de `EnrollmentDate` e imediatamente antes da marcação de `fieldset` de fechamento, adicione o código para exibir uma lista de registros, conforme mostrado no exemplo a seguir:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml?highlight=4-22)]

    Esse código percorre as entidades na propriedade de navegação `Enrollments`. Para cada entidade `Enrollment` na propriedade, ela exibe o título e a classificação do curso. O título do curso é recuperado da entidade `Course` que é armazenada na propriedade de navegação `Course` da entidade `Enrollments`. Todos esses dados são recuperados automaticamente do banco de dado quando necessário. (Em outras palavras, você está usando o carregamento lento aqui. Você não especificou o *carregamento adiantado* para a propriedade de navegação `Courses`, de modo que, na primeira vez que tentar acessar essa propriedade, uma consulta será enviada ao banco de dados para recuperar os mesmos. Você pode ler mais sobre o carregamento lento e o carregamento adiantado no tutorial [lendo dados relacionados](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) mais adiante nesta série.)
3. Execute a página selecionando a guia **estudantes** e clicando em um link **detalhes** para Alexander Carson. Você verá a lista de cursos e notas do aluno selecionado:

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)

## <a name="updating-the-create-page"></a>Atualizando a página criar

1. No *Controllers\StudentController.cs*, substitua o método de ação `HttpPost``Create` pelo código a seguir para adicionar um bloco de `try-catch` e o [atributo BIND](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) ao método com Scaffold: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cs?highlight=4,7-8,14-20)]

    Esse código adiciona a entidade `Student` criada pelo associador de modelo MVC do ASP.NET ao conjunto de entidades de `Students` e salva as alterações no banco de dados. (O*associador de modelo* refere-se à funcionalidade MVC ASP.NET que facilita o trabalho com os dados enviados por um formulário; um associador de modelo converte valores de formulário postados em tipos CLR e os passa para o método de ação em parâmetros. Nesse caso, o associador de modelo instancia uma entidade `Student` para você usando valores de propriedade da coleção `Form`.)

    O atributo `ValidateAntiForgeryToken` ajuda a impedir ataques [de solicitação entre sites forjada](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) .

<a id="overpost"></a>

    > [!WARNING]
    > Security - The `Bind` attribute is added to protect against *over-posting*. For example, suppose the `Student` entity includes a `Secret` property that you don't want this web page to update.
    > 
    > [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cs?highlight=7)]
    > 
    > Even if you don't have a `Secret` field on the web page, a hacker could use a tool such as [fiddler](http://fiddler2.com/home), or write some JavaScript, to post a `Secret` form value. Without the [Bind](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) attribute limiting the fields that the model binder uses when it creates a `Student` instance*,* the model binder would pick up that `Secret` form value and use it to update the `Student` entity instance. Then whatever value the hacker specified for the `Secret` form field would be updated in your database. The following image shows the fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.
    > 
    > ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image6.png)  
    > 
    > The value "OverPost" would then be successfully added to the `Secret` property of the inserted row, although you never intended that the web page be able to update that property.
    > 
    > It's a security best practice to use the `Include` parameter with the `Bind` attribute to *whitelist* fields. It's also possible to use the `Exclude` parameter to *blacklist* fields you want to exclude. The reason `Include` is more secure is that when you add a new property to the entity, the new field is not automatically protected by an `Exclude` list.
    > 
    > Another alternative approach, and one preferred by many, is to use only view models with model binding. The view model contains only the properties you want to bind. Once the MVC model binder has finished, you copy the view model properties to the entity instance.

    Other than the `Bind` attribute, the `try-catch` block is the only change you've made to the scaffolded code. If an exception that derives from [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) is caught while the changes are being saved, a generic error message is displayed. [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) exceptions are sometimes caused by something external to the application rather than a programming error, so the user is advised to try again. Although not implemented in this sample, a production quality application would log the exception (and non-null inner exceptions ) with a logging mechanism such as [ELMAH](https://code.google.com/p/elmah/).

    The code in *Views\Student\Create.cshtml* is similar to what you saw in *Details.cshtml*, except that `EditorFor` and `ValidationMessageFor` helpers are used for each field instead of `DisplayFor`. The following example shows the relevant code:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml)]

    *Create.cshtml* also includes `@Html.AntiForgeryToken()`, which works with the `ValidateAntiForgeryToken` attribute in the controller to help prevent [cross-site request forgery](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) attacks.

    No changes are required in *Create.cshtml*.
2. Execute a página selecionando a guia **alunos** e clicando em **criar nova**.

    ![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image7.png)

    Por padrão, algumas validações de dados funcionam. Insira nomes e uma data inválida e clique em **criar** para ver a mensagem de erro.

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

    O código realçado a seguir mostra a verificação de validação do modelo.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=5)]

    Altere a data para um valor válido como 9/1/2005 e clique em **criar** para ver o novo aluno aparecer na página de **índice** .

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

## <a name="updating-the-edit-post-page"></a>Atualizando a página Editar POSTAgem

No *Controllers\StudentController.cs*, o método `Edit` `HttpGet` (aquele sem o atributo `HttpPost`) usa o método `Find` para recuperar a entidade de `Student` selecionada, como vimos no método `Details`. Não é necessário alterar esse método.

No entanto, substitua o método de ação de `Edit` `HttpPost` pelo código a seguir para adicionar um bloco de `try-catch` e o [atributo de associação](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx):

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs)]

Esse código é semelhante ao que você viu no método `HttpPost` `Create`. No entanto, em vez de adicionar a entidade criada pelo associador de modelo ao conjunto de entidades, esse código define um sinalizador na entidade indicando que ela foi alterada. Quando o método [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) é chamado, o sinalizador [Modified](https://msdn.microsoft.com/library/system.data.entitystate.aspx) faz com que a Entity Framework crie instruções SQL para atualizar a linha do banco de dados. Todas as colunas da linha de banco de dados serão atualizadas, incluindo aquelas que o usuário não alterou e conflitos de simultaneidade serão ignorados. (Você aprenderá como lidar com a simultaneidade em um tutorial posterior nesta série.)

### <a name="entity-states-and-the-attach-and-savechanges-methods"></a>Estados de entidade e os métodos Attach e SaveChanges

O contexto de banco de dados controla se as entidades em memória estão em sincronia com suas linhas correspondentes no banco de dados, e essas informações determinam o que acontece quando você chama o método `SaveChanges`. Por exemplo, quando você passa uma nova entidade para o método [Add](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx) , o estado dessa entidade é definido como `Added`. Em seguida, quando você chama o método [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) , o contexto do banco de dados emite um comando SQL `INSERT`.

Uma entidade pode estar em um dos[seguintes Estados](https://msdn.microsoft.com/library/system.data.entitystate.aspx):

- `Added`. A entidade ainda não existe no banco de dados. O método `SaveChanges` deve emitir uma instrução `INSERT`.
- `Unchanged`. Nada precisa ser feito com essa entidade pelo método `SaveChanges`. Ao ler uma entidade do banco de dados, a entidade começa com esse status.
- `Modified`. Alguns ou todos os valores de propriedade da entidade foram modificados. O método `SaveChanges` deve emitir uma instrução `UPDATE`.
- `Deleted`. A entidade foi marcada para exclusão. O método `SaveChanges` deve emitir uma instrução `DELETE`.
- `Detached`. A entidade não está sendo controlada pelo contexto de banco de dados.

Em um aplicativo da área de trabalho, em geral, as alterações de estado são definidas automaticamente. Em um tipo de área de trabalho de aplicativo, você lê uma entidade e faz alterações em alguns de seus valores de propriedade. Isso faz com que seu estado da entidade seja alterado automaticamente para `Modified`. Em seguida, quando você chama `SaveChanges`, o Entity Framework gera uma instrução SQL `UPDATE` que atualiza apenas as propriedades reais que você alterou.

A natureza desconectada dos aplicativos Web não permite essa sequência contínua. O [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) que lê uma entidade é descartado depois que uma página é renderizada. Quando o `HttpPost` método de ação `Edit` é chamado, uma nova solicitação é feita e você tem uma nova instância do [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx), portanto, você precisa definir manualmente o estado da entidade como `Modified.`, quando chama `SaveChanges`, o Entity Framework atualiza todas as colunas da linha do banco de dados, porque o contexto não tem como saber quais propriedades você alterou.

Se você quiser que a instrução SQL `Update` atualize somente os campos que o usuário realmente alterou, poderá salvar os valores originais de alguma forma (como campos ocultos) para que fiquem disponíveis quando o método `HttpPost` `Edit` for chamado. Em seguida, você pode criar uma entidade `Student` usando os valores originais, chamar o método `Attach` com essa versão original da entidade, atualizar os valores da entidade para os novos valores e, em seguida, chamar `SaveChanges.` para obter mais informações, consulte [Estados de entidade e](https://msdn.microsoft.com/data/jj592676) [dados locais](https://msdn.microsoft.com/data/jj592872) no MSDN Data Developer Center.

O código em *Views\Student\Edit.cshtml* é semelhante ao que você viu em *Create. cshtml*, e nenhuma alteração é necessária.

Execute a página selecionando a guia **estudantes** e clicando em um hiperlink **Editar** .

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)

Altere alguns dos dados e clique em **Salvar**. Você vê os dados alterados na página de índice.

![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Atualizando a página de exclusão

No *Controllers\StudentController.cs*, o código do modelo para o método `HttpGet` `Delete` usa o método `Find` para recuperar a entidade `Student` selecionada, como vimos nos métodos `Details` e `Edit`. No entanto, para implementar uma mensagem de erro personalizada quando a chamada a `SaveChanges` falhar, você adicionará uma funcionalidade a esse método e à sua exibição correspondente.

Como você viu para operações de atualização e criação, as operações de exclusão exigem dois métodos de ação. O método que é chamado em resposta a uma solicitação GET exibe uma exibição que dá ao usuário a oportunidade de aprovar ou cancelar a operação de exclusão. Se o usuário aprová-la, uma solicitação POST será criada. Quando isso acontece, o método de `Delete` de `HttpPost` é chamado e, em seguida, esse método executa a operação de exclusão.

Você adicionará um bloco de `try-catch` ao método `HttpPost` `Delete` para lidar com quaisquer erros que possam ocorrer quando o banco de dados for atualizado. Se ocorrer um erro, o método `HttpPost` `Delete` chamará o método `HttpGet` `Delete`, passando um parâmetro que indica que ocorreu um erro. Em seguida, o método `HttpGet Delete` reexibe a página de confirmação junto com a mensagem de erro, dando ao usuário uma oportunidade de cancelar ou tentar novamente.

1. Substitua o método de ação de `Delete` `HttpGet` pelo código a seguir, que gerencia o relatório de erros: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cs)]

    Esse código aceita um parâmetro booliano [opcional](https://msdn.microsoft.com/library/dd264739.aspx) que indica se ele foi chamado após uma falha ao salvar as alterações. Esse parâmetro é `false` quando o método de `Delete` de `HttpGet` é chamado sem uma falha anterior. Quando é chamado pelo método `HttpPost` `Delete` em resposta a um erro de atualização do banco de dados, o parâmetro é `true` e uma mensagem de erro é passada para a exibição.
2. Substitua o método de ação de `Delete` `HttpPost` (chamado `DeleteConfirmed`) pelo código a seguir, que executa a operação de exclusão real e captura quaisquer erros de atualização de banco de dados.

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs)]

     Esse código recupera a entidade selecionada e, em seguida, chama o método [Remove](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx) para definir o status da entidade como `Deleted`. Quando `SaveChanges` é chamado, um comando SQL `DELETE` é gerado. Você também alterou o nome do método de ação de `DeleteConfirmed` para `Delete`. O código com Scaffold chamado `HttpPost` método `Delete` `DeleteConfirmed` para dar ao método `HttpPost` uma assinatura exclusiva. (O CLR requer métodos sobrecarregados para ter parâmetros de método diferentes.) Agora que as assinaturas são exclusivas, você pode se manter com a Convenção MVC e usar o mesmo nome para os métodos `HttpPost` e `HttpGet` Delete.

     Se melhorar o desempenho em um aplicativo de alto volume for uma prioridade, você poderá evitar uma consulta SQL desnecessária para recuperar a linha, substituindo as linhas de código que chamam os métodos `Find` e `Remove` com o seguinte código, conforme mostrado no realce amarelo:

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

     Esse código instancia uma entidade `Student` usando apenas o valor da chave primária e, em seguida, define o estado da entidade como `Deleted`. Isso é tudo o que o Entity Framework precisa para excluir a entidade.

     Como observado, o método `HttpGet` `Delete` não exclui os dados. Executar uma operação de exclusão em resposta a uma solicitação GET (ou, para esse caso, executar qualquer operação de edição, operação de criação ou qualquer outra operação que altere dados) cria um risco de segurança. Para obter mais informações, consulte [#46 de dicas MVC do ASP.net – não use os links de exclusão porque eles criam brechas de segurança](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) no blog de Stephen Walther.
3. No *Views\Student\Delete.cshtml*, adicione uma mensagem de erro entre o cabeçalho `h2` e o cabeçalho `h3`, conforme mostrado no exemplo a seguir:

     [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cshtml?highlight=2)]

     Execute a página selecionando a guia **estudantes** e clicando em um hiperlink **excluir** :

     ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image12.png)
4. Clique em **Excluir**. A página Índice será exibida sem o aluno excluído. (Você verá um exemplo do código de tratamento de erros em ação no tutorial de [manipulação de simultaneidade](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) , mais adiante nesta série.)

## <a name="ensuring-that-database-connections-are-not-left-open"></a>Garantindo que as conexões de banco de dados não sejam deixadas abertas

Para verificar se as conexões de banco de dados estão corretamente fechadas e os recursos que eles contêm, você deve ver que a instância de contexto foi descartada. É por isso que o código com Scaffold fornece um método [Dispose](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx) no final da classe `StudentController` em *StudentController.cs*, conforme mostrado no exemplo a seguir:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

A classe de `Controller` base já implementa a interface `IDisposable`, portanto, esse código simplesmente adiciona uma substituição ao método `Dispose(bool)` para descartar explicitamente a instância de contexto.

## <a name="summary"></a>Resumo

Agora você tem um conjunto completo de páginas que executam operações CRUD simples para `Student` entidades. Você usou auxiliares MVC para gerar elementos de interface do usuário para campos de dados. Para obter mais informações sobre auxiliares MVC, consulte [renderizando um formulário usando auxiliares HTML](https://msdn.microsoft.com/library/dd410596(v=VS.98).aspx) (a página é para o MVC 3, mas ainda é relevante para o MVC 4).

No próximo tutorial, você expandirá a funcionalidade da página de índice adicionando classificação e paginação.

Links para outros recursos de Entity Framework podem ser encontrados no [mapa de conteúdo de acesso a dados do ASP.net](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Anterior](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
> [Próximo](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
