---
uid: mvc/overview/getting-started/introduction/adding-a-new-field
title: Adicionando um novo campo | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 4085de68-d243-4378-8a64-86236ea8d2da
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 5974e53e4610dccc7812df261dc97a9b0327de85
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78582904"
---
# <a name="adding-a-new-field"></a>Adicionando um Novo Campo

por [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [Tutorial Note](index.md)]

Nesta seção, você usará Migrações do Entity Framework Code First para migrar algumas alterações nas classes de modelo para que a alteração seja aplicada ao banco de dados.

Por padrão, quando você usa Entity Framework Code First para criar automaticamente um banco de dados, como fazia anteriormente neste tutorial, Code First adiciona uma tabela ao banco de dados para ajudar a controlar se o esquema do banco de dados está em sincronia com as classes de modelo das quais ele foi gerado. Se eles não estiverem em sincronia, o Entity Framework gerará um erro. Isso facilita o rastreamento de problemas em tempo de desenvolvimento que, de outra forma, você pode localizar (por erros obscuros) em tempo de execução.

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Configurando Migrações do Code First para alterações de modelo

Navegue até Gerenciador de Soluções. Clique com o botão direito do mouse no arquivo *Movies. MDF* e selecione **excluir** para remover o banco de dados de filmes. Se você não vir o arquivo *Movies. MDF* , clique no ícone **Mostrar todos os arquivos** mostrado abaixo na estrutura de tópicos vermelha.

![](adding-a-new-field/_static/image1.png)

Compile o aplicativo para verificar se não existem erros.

No menu **Ferramentas**, clique em **Gerenciador de Pacotes NuGet** e, em seguida, em **Console do Gerenciador de Pacotes**.

![Adicionar Man de pacote](adding-a-new-field/_static/image2.png)

Na janela do **console do Gerenciador de pacotes** , no prompt de `PM>`, insira

Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext

![](adding-a-new-field/_static/image3.png)

O comando **Enable-Migrations** (mostrado acima) cria um arquivo *Configuration.cs* em uma nova pasta *migrações* .

![](adding-a-new-field/_static/image4.png)

O Visual Studio abre o arquivo *Configuration.cs* . Substitua o método `Seed` no arquivo *Configuration.cs* pelo seguinte código:

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

Passe o mouse sobre a linha ondulada vermelha em `Movie` e clique em `Show Potential Fixes` e, em seguida, clique em **usando** **MvcMovie. Models;**

![](adding-a-new-field/_static/image5.png)

Isso adiciona a seguinte instrução Using:

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

> [!NOTE]
> 
> Migrações do Code First chama o método `Seed` após cada migração (ou seja, chamando **Update-Database** no console do Gerenciador de pacotes) e esse método atualiza as linhas que já foram inseridas ou as insere se elas ainda não existirem.
> 
> O método [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) no código a seguir executa uma operação "Upsert":
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample3.cs)]
> 
> Como o método de [semente](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) é executado com cada migração, você não pode apenas inserir dados, pois as linhas que você está tentando adicionar já estarão lá após a primeira migração que cria o banco de dados. A operação "[Upsert](http://en.wikipedia.org/wiki/Upsert)" impede erros que ocorrerão se você tentar inserir uma linha que já existe, mas ela substitui quaisquer alterações nos dados que você tenha feito durante o teste do aplicativo. Com os dados de teste em algumas tabelas, talvez você não queira que isso aconteça: em alguns casos, ao alterar os dados durante o teste, você deseja que as alterações permaneçam após as atualizações do banco. Nesse caso, você deseja fazer uma operação de inserção condicional: Insira uma linha somente se ela ainda não existir.   
> 
> O primeiro parâmetro passado para o método [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) especifica a propriedade a ser usada para verificar se já existe uma linha. Para os dados de filme de teste que você está fornecendo, a propriedade `Title` pode ser usada para essa finalidade, pois cada título na lista é exclusivo:
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample4.cs)]
> 
> Esse código pressupõe que os títulos são exclusivos. Se você adicionar manualmente um título duplicado, obterá a seguinte exceção na próxima vez que executar uma migração.   
> 
> *A sequência contém mais de um elemento*  
> 
> Para obter mais informações sobre o método [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) , consulte [tomar cuidado com o método AddOrUpdate do EF 4,3](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)..

**Pressione Ctrl-Shift-B para compilar o projeto.** (As etapas a seguir falharão se você não compilar neste ponto.)

A próxima etapa é criar uma classe de `DbMigration` para a migração inicial. Essa migração cria um novo banco de dados, é por isso que você excluiu o arquivo *Movie. MDF* em uma etapa anterior.

Na janela do **console do Gerenciador de pacotes** , insira o comando `add-migration Initial` para criar a migração inicial. O nome "Initial" é arbitrário e é usado para nomear o arquivo de migração criado.

![](adding-a-new-field/_static/image6.png)

Migrações do Code First cria outro arquivo de classe na pasta *migrações* (com o nome *{DateStamp}\_Initial.cs* ) e essa classe contém um código que cria o esquema de banco de dados. O nome de arquivo de migração é previamente corrigido com um carimbo de data/hora para ajudar com a ordenação. Examine o arquivo *{dateStamp}\_Initial.cs* , que contém as instruções para criar a tabela `Movies` para o BD de filme. Quando você atualizar o banco de dados nas instruções abaixo, este arquivo *{dateStamp}\_Initial.cs* será executado e criará o esquema do BD. Em seguida, o método **semente** será executado para popular o DB com dados de teste.

No **console do Gerenciador de pacotes**, insira o comando `update-database` para criar o banco de dados e executar o método `Seed`.

![](adding-a-new-field/_static/image7.png)

Se você receber um erro que indica que uma tabela já existe e não pode ser criada, é provável que você tenha executado o aplicativo depois de ter excluído o banco de dados e antes de executar `update-database`. Nesse caso, exclua o arquivo *Movies. MDF* novamente e repita o comando `update-database`. Se você ainda receber um erro, exclua a pasta de migrações e o conteúdo, em seguida, inicie com as instruções na parte superior desta página (isto é, exclua o arquivo *Movies. MDF* e continue para Enable-Migrations). Se você ainda receber um erro, abra Pesquisador de Objetos do SQL Server e remova o banco de dados da lista.

Execute o aplicativo e navegue até a URL */Movies* . Os dados de semente são exibidos.

![](adding-a-new-field/_static/image8.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>Adicionando uma propriedade de classificação ao modelo de filme

Comece adicionando uma nova propriedade `Rating` à classe `Movie` existente. Abra o arquivo *Models\Movie.cs* e adicione a propriedade `Rating` como esta:

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

A classe completa `Movie` agora é semelhante ao seguinte código:

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs?highlight=12)]

Compile o aplicativo (Ctrl + Shift + B).

Como você adicionou um novo campo à classe `Movie`, também precisa atualizar a *lista branca* de associação para que essa nova propriedade seja incluída. Atualize o atributo `bind` para `Create` e `Edit` métodos de ação para incluir a propriedade `Rating`:

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs?highlight=1)]

Você também precisa atualizar os modelos de exibição para exibir, criar e editar a nova propriedade `Rating` na exibição do navegador.

Abra o arquivo *\Views\Movies\Index.cshtml* e adicione um título de coluna `<th>Rating</th>` logo após a coluna **Price** . Em seguida, adicione uma coluna `<td>` próximo ao final do modelo para renderizar o valor de `@item.Rating`. Abaixo está a aparência do modelo de exibição *index. cshtml* atualizado:

[!code-cshtml[Main](adding-a-new-field/samples/sample8.cshtml?highlight=31-33,52-54)]

Em seguida, abra o arquivo *\Views\Movies\Create.cshtml* e adicione o campo `Rating` com a marcação realçada a seguir. Isso renderiza uma caixa de texto para que você possa especificar uma classificação quando um novo filme é criado.

[!code-cshtml[Main](adding-a-new-field/samples/sample9.cshtml?highlight=9-15)]

Agora você atualizou o código do aplicativo para dar suporte à nova propriedade `Rating`.

Execute o aplicativo e navegue até a URL */Movies* . No entanto, ao fazer isso, você verá um dos seguintes erros:

![](adding-a-new-field/_static/image9.png)  
  
O modelo que faz o backup do contexto ' MovieDBContext ' foi alterado desde que o banco de dados foi criado. Considere o uso de Migrações do Code First para atualizar o banco de dados (https://go.microsoft.com/fwlink/?LinkId=238269).

![](adding-a-new-field/_static/image10.png)

Você está vendo esse erro porque a classe do modelo de `Movie` atualizado no aplicativo agora é diferente do esquema da tabela `Movie` do banco de dados existente. (Não há nenhuma coluna `Rating` na tabela de banco de dados.)

Existem algumas abordagens para resolver o erro:

1. Faça com que o Entity Framework remova automaticamente e recrie o banco de dados com base no novo esquema de classe de modelo. Essa abordagem é muito conveniente no início do ciclo de desenvolvimento, quando você está fazendo o desenvolvimento ativo em um banco de dados de teste; ela permite que você desenvolva rapidamente o modelo e o esquema de banco de dados juntos. No entanto, a desvantagem é que você perde os dados existentes no banco de dados – portanto, *não* convém usar essa abordagem em um banco de dados de produção! Muitas vezes, o uso de um inicializador para propagar um banco de dados com os dados de teste automaticamente é uma maneira produtiva de desenvolver um aplicativo. Para obter mais informações sobre inicializadores de banco de dados Entity Framework, consulte o [tutorial ASP.NET MVC/Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
2. Modifique explicitamente o esquema do banco de dados existente para que ele corresponda às classes de modelo. A vantagem dessa abordagem é que você mantém os dados. Faça essa alteração manualmente ou criando um script de alteração de banco de dados.
3. Use as Migrações do Code First para atualizar o esquema de banco de dados.

Para este tutorial, usaremos as Migrações do Code First.

Atualize o método semente para que ele forneça um valor para a nova coluna. Abra o arquivo Migrations\Configuration.cs e adicione um campo de classificação a cada objeto de filme.

[!code-csharp[Main](adding-a-new-field/samples/sample10.cs?highlight=6)]

Compile a solução e, em seguida, abra a janela do **console do Gerenciador de pacotes** e digite o seguinte comando:

`add-migration Rating`

O comando `add-migration` informa à estrutura de migração para examinar o modelo de filme atual com o esquema atual do BD do filme e criar o código necessário para migrar o BD para o novo modelo. A *classificação* de nome é arbitrária e é usada para nomear o arquivo de migração. É útil usar um nome significativo para a etapa de migração.

Quando esse comando for concluído, o Visual Studio abrirá o arquivo de classe que define a nova classe derivada `DbMigration` e, no método `Up`, você poderá ver o código que cria a nova coluna.

[!code-csharp[Main](adding-a-new-field/samples/sample11.cs)]

Compile a solução e, em seguida, insira o comando `update-database` na janela do **console do Gerenciador de pacotes** .

A imagem a seguir mostra a saída na janela do **console do Gerenciador de pacotes** (a *classificação* de prependências de carimbo de data/hora será diferente).

![](adding-a-new-field/_static/image11.png)

Execute novamente o aplicativo e navegue até a URL do/Movies. Você pode ver o novo campo de classificação.

![](adding-a-new-field/_static/image12.png)

Clique no link **criar novo** para adicionar um novo filme. Observe que você pode adicionar uma classificação.

![7_CreateRioII](adding-a-new-field/_static/image13.png)

Clique em **Criar**. O novo filme, incluindo a classificação, agora aparece na listagem de filmes:

![7_ourNewMovie_SM](adding-a-new-field/_static/image14.png)

Agora que o projeto está usando migrações, você não precisará remover o banco de dados quando adicionar um novo campo ou atualizar o esquema. Na próxima seção, vamos fazer mais alterações de esquema e usar migrações para atualizar o banco de dados.

Você também deve adicionar o campo `Rating` aos modelos de exibição Editar, detalhes e excluir.

Você pode inserir o comando "Update-Database" na janela do **console do Gerenciador de pacotes** novamente e nenhum código de migração será executado, pois o esquema corresponde ao modelo. No entanto, a execução de "Update-Database" executará o método `Seed` novamente e, se você tiver alterado qualquer um dos dados semente, as alterações serão perdidas porque o método `Seed` upserts os dados. Você pode ler mais sobre o método `Seed` no [tutorial popular ASP.NET MVC/Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)do Tom Dykstra.

Nesta seção, você viu como é possível modificar objetos de modelo e manter o banco de dados em sincronia com as alterações. Você também aprendeu uma maneira de preencher um banco de dados recém-criado com exemplos de dado, de modo que você possa experimentar cenários. Essa foi apenas uma breve introdução à Code First, consulte [criando um modelo de dados de Entity Framework para um aplicativo MVC ASP.net](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) para obter um tutorial mais completo sobre o assunto. Em seguida, vamos dar uma olhada em como você pode adicionar uma lógica de validação mais rica às classes de modelo e permitir que algumas regras de negócio sejam impostas.

> [!div class="step-by-step"]
> [Anterior](adding-search.md)
> [Próximo](adding-validation.md)
