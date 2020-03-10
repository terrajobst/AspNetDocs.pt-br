---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: Adicionando um novo campo ao modelo de filme e à tabela | Microsoft Docs
author: Rick-Anderson
description: 'Observação: uma versão atualizada deste tutorial está disponível aqui que usa o ASP.NET MVC 5 e o Visual Studio 2013. É mais seguro, muito mais simples de seguir e demonstrar...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: d966b95163f64b20a17d2327a12c5d6c44a4a66b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78615083"
---
# <a name="adding-a-new-field-to-the-movie-model-and-table"></a>Adicionar um novo campo ao modelo de filme e à tabela

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Uma versão atualizada deste tutorial está disponível [aqui](../../getting-started/introduction/getting-started.md) que usa o ASP.NET MVC 5 e o Visual Studio 2013. É mais seguro, muito mais simples de seguir e demonstra mais recursos.

Nesta seção, você usará Migrações do Entity Framework Code First para migrar algumas alterações nas classes de modelo para que a alteração seja aplicada ao banco de dados.

Por padrão, quando você usa Entity Framework Code First para criar automaticamente um banco de dados, como fazia anteriormente neste tutorial, Code First adiciona uma tabela ao banco de dados para ajudar a controlar se o esquema do banco de dados está em sincronia com as classes de modelo das quais ele foi gerado. Se eles não estiverem em sincronia, o Entity Framework gerará um erro. Isso facilita o rastreamento de problemas em tempo de desenvolvimento que, de outra forma, você pode localizar (por erros obscuros) em tempo de execução.

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Configurando Migrações do Code First para alterações de modelo

Se você estiver usando o Visual Studio 2012, clique duas vezes no arquivo *Movies. MDF* de Gerenciador de soluções para abrir a ferramenta de banco de dados. Visual Studio Express para Web mostrará Gerenciador de Banco de Dados, o Visual Studio 2012 mostrará Gerenciador de Servidores. Se você estiver usando o Visual Studio 2010, use Pesquisador de Objetos do SQL Server.

Na ferramenta de banco de dados (Gerenciador de Banco de Dados, Gerenciador de Servidores ou Pesquisador de Objetos do SQL Server), clique com o botão direito do mouse em `MovieDBContext` e selecione **excluir** para remover o banco de dados de filmes.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

Navegue de volta para Gerenciador de Soluções. Clique com o botão direito do mouse no arquivo *Movies. MDF* e selecione **excluir** para remover o banco de dados de filmes.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

Compile o aplicativo para verificar se não existem erros.

No menu **Ferramentas**, clique em **Gerenciador de Pacotes NuGet** e, em seguida, em **Console do Gerenciador de Pacotes**.

![Adicionar Man de pacote](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

Na janela do **console do Gerenciador de pacotes** , na `PM>` prompt, digite "Enable-Migrations-ContextTypeName MvcMovie. Models. MovieDBContext".

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

O comando **Enable-Migrations** (mostrado acima) cria um arquivo *Configuration.cs* em uma nova pasta *migrações* .

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

O Visual Studio abre o arquivo *Configuration.cs* . Substitua o método `Seed` no arquivo *Configuration.cs* pelo seguinte código:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

Clique com o botão direito do mouse na linha vermelha ondulada em `Movie` e selecione **resolver** e, em seguida, **usando** **MvcMovie. Models;**

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

Isso adiciona a seguinte instrução Using:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> Migrações do Code First chama o método `Seed` após cada migração (ou seja, chamando **Update-Database** no console do Gerenciador de pacotes) e esse método atualiza as linhas que já foram inseridas ou as insere se elas ainda não existirem.

**Pressione Ctrl-Shift-B para compilar o projeto.** (As etapas a seguir falharão se seu não for criado neste ponto.)

A próxima etapa é criar uma classe de `DbMigration` para a migração inicial. Essa migração para criar um novo banco de dados, é por isso que você excluiu o arquivo *Movie. MDF* em uma etapa anterior.

Na janela do **console do Gerenciador de pacotes** , insira o comando "Add-Migration Initial" para criar a migração inicial. O nome "Initial" é arbitrário e é usado para nomear o arquivo de migração criado.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

Migrações do Code First cria outro arquivo de classe na pasta *migrações* (com o nome *{DateStamp}\_Initial.cs* ) e essa classe contém um código que cria o esquema de banco de dados. O nome de arquivo de migração é previamente corrigido com um carimbo de data/hora para ajudar com a ordenação. Examine o arquivo *{dateStamp}\_Initial.cs* , que contém as instruções para criar a tabela de filmes para o BD de filme. Quando você atualizar o banco de dados nas instruções abaixo, este arquivo *{dateStamp}\_Initial.cs* será executado e criará o esquema do BD. Em seguida, o método **semente** será executado para popular o DB com dados de teste.

No **console do Gerenciador de pacotes**, digite o comando "Update-Database" para criar o banco de dados e executar o método **semente** .

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

Se você receber um erro que indica que uma tabela já existe e não pode ser criada, é provável que você tenha executado o aplicativo depois de ter excluído o banco de dados e antes de executar `update-database`. Nesse caso, exclua o arquivo *Movies. MDF* novamente e repita o comando `update-database`. Se você ainda receber um erro, exclua a pasta de migrações e o conteúdo, em seguida, inicie com as instruções na parte superior desta página (isto é, exclua o arquivo *Movies. MDF* e continue para Enable-Migrations).

Execute o aplicativo e navegue até a URL */Movies* . Os dados de semente são exibidos.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>Adicionando uma propriedade de classificação ao modelo de filme

Comece adicionando uma nova propriedade `Rating` à classe `Movie` existente. Abra o arquivo *Models\Movie.cs* e adicione a propriedade `Rating` como esta:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

A classe completa `Movie` agora é semelhante ao seguinte código:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

Crie o aplicativo usando o comando de menu **criar** &gt;**Compilar filme** ou pressionando Ctrl-Shift-B.

Agora que você atualizou a classe `Model`, também precisa atualizar os modelos de exibição *\Views\Movies\Index.cshtml* e *\Views\Movies\Create.cshtml* para exibir a nova propriedade `Rating` no modo de exibição de navegador.

Abra o arquivo<em>\Views\Movies\Index.cshtml</em> e adicione um título de coluna `<th>Rating</th>` logo após a coluna <strong>Price</strong> . Em seguida, adicione uma coluna `<td>` próximo ao final do modelo para renderizar o valor de `@item.Rating`. Abaixo está a aparência do modelo de exibição <em>index. cshtml</em> atualizado:

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

Em seguida, abra o arquivo *\Views\Movies\Create.cshtml* e adicione a marcação a seguir próximo ao final do formulário. Isso renderiza uma caixa de texto para que você possa especificar uma classificação quando um novo filme é criado.

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

Agora você atualizou o código do aplicativo para dar suporte à nova propriedade `Rating`.

Agora, execute o aplicativo e navegue até a URL */Movies* . No entanto, ao fazer isso, você verá um dos seguintes erros:

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

Você está vendo esse erro porque a classe do modelo de `Movie` atualizado no aplicativo agora é diferente do esquema da tabela `Movie` do banco de dados existente. (Não há nenhuma coluna `Rating` na tabela de banco de dados.)

Existem algumas abordagens para resolver o erro:

1. Faça com que o Entity Framework remova automaticamente e recrie o banco de dados com base no novo esquema de classe de modelo. Essa abordagem é muito conveniente ao fazer o desenvolvimento ativo em um banco de dados de teste; Ele permite que você evolua rapidamente o modelo e o esquema de banco de dados juntos. No entanto, a desvantagem é que você perde os dados existentes no banco de dados – portanto, *não* convém usar essa abordagem em um banco de dados de produção! O uso de um inicializador para propagar automaticamente um banco de dados com dado de teste é geralmente uma maneira produtiva de desenvolver um aplicativo. Para obter mais informações sobre inicializadores de banco de dados Entity Framework, consulte o [tutorial ASP.NET MVC/Entity Framework](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)de Tom Dykstra.
2. Modifique explicitamente o esquema do banco de dados existente para que ele corresponda às classes de modelo. A vantagem dessa abordagem é que você mantém os dados. Faça essa alteração manualmente ou criando um script de alteração de banco de dados.
3. Use as Migrações do Code First para atualizar o esquema de banco de dados.

Para este tutorial, usaremos as Migrações do Code First.

Atualize o método semente para que ele forneça um valor para a nova coluna. Abra o arquivo Migrations\Configuration.cs e adicione um campo de classificação a cada objeto de filme.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

Compile a solução e, em seguida, abra a janela do **console do Gerenciador de pacotes** e digite o seguinte comando:

`add-migration AddRatingMig`

O comando `add-migration` informa à estrutura de migração para examinar o modelo de filme atual com o esquema atual do BD do filme e criar o código necessário para migrar o BD para o novo modelo. O AddRatingMig é arbitrário e é usado para nomear o arquivo de migração. É útil usar um nome significativo para a etapa de migração.

Quando esse comando for concluído, o Visual Studio abrirá o arquivo de classe que define a nova classe derivada `DbMigration` e, no método `Up`, você poderá ver o código que cria a nova coluna.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

Compile a solução e, em seguida, insira o comando "Update-Database" na janela do **console do Gerenciador de pacotes** .

A imagem a seguir mostra a saída na janela do **console do Gerenciador de pacotes** (o carimbo de data AddRatingMig de pendência será diferente.)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

Execute novamente o aplicativo e navegue até a URL do/Movies. Você pode ver o novo campo de classificação.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

Clique no link **criar novo** para adicionar um novo filme. Observe que você pode adicionar uma classificação.

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

Clique em **Criar**. O novo filme, incluindo a classificação, agora aparece na listagem de filmes:

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

Você também deve adicionar o campo `Rating` aos modelos de exibição Editar, detalhes e SearchIndex.

Você pode inserir o comando "Update-Database" na janela do **console do Gerenciador de pacotes** novamente e nenhuma alteração será feita, pois o esquema corresponde ao modelo.

Nesta seção, você viu como é possível modificar objetos de modelo e manter o banco de dados em sincronia com as alterações. Você também aprendeu uma maneira de preencher um banco de dados recém-criado com exemplos de dado, de modo que você possa experimentar cenários. Em seguida, vamos dar uma olhada em como você pode adicionar uma lógica de validação mais rica às classes de modelo e permitir que algumas regras de negócio sejam impostas.

> [!div class="step-by-step"]
> [Anterior](examining-the-edit-methods-and-edit-view.md)
> [Próximo](adding-validation-to-the-model.md)
