---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: Modelos e acesso a dados do ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: 'Observação: este laboratório prático pressupõe que você tenha conhecimento básico do ASP.NET MVC. Se você não usou o ASP.NET MVC antes, recomendamos que você percorra o ASP.NET MVC 4...'
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: 90635b617930d0a9c126795f4c8790d542e33dc9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78560196"
---
# <a name="aspnet-mvc-4-models-and-data-access"></a>Acesso a dados e modelos do ASP.NET MVC 4

por [equipe de acampamentos da Web](https://twitter.com/webcamps)

[Baixe o kit de treinamento do Web acampamentos](https://aka.ms/webcamps-training-kit)

Este laboratório prático pressupõe que você tenha conhecimento básico do **ASP.NET MVC**. Se você não usou o **ASP.NET MVC** antes, recomendamos que você vá para o laboratório prático do **ASP.NET MVC 4 Fundamentals** .

Este laboratório orienta você pelos aprimoramentos e novos recursos descritos anteriormente, aplicando pequenas alterações a um aplicativo Web de exemplo fornecido na pasta de origem.

> [!NOTE]
> Todos os códigos de exemplo e trechos de código estão incluídos no Web acampamentos Training Kit, disponível nas [versões Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). O projeto específico deste laboratório está disponível em [modelos do ASP.NET MVC 4 e acesso a dados](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).

No laboratório prático dos **conceitos básicos do ASP.NET MVC** , você passou a passar dados embutidos em código dos controladores para os modelos de exibição. Mas, para criar um aplicativo Web real, talvez você queira usar um banco de dados real.

Este laboratório prático mostrará como usar um mecanismo de banco de dados para armazenar e recuperar os dados necessários para o aplicativo de loja de música. Para fazer isso, você começará com um banco de dados existente e criará o Modelo de Dados de Entidade a partir dele. Ao longo deste laboratório, você encontrará a abordagem de **Database First** , bem como a abordagem de **Code First** .

No entanto, você também pode usar a abordagem **Model First** , criar o mesmo modelo usando as ferramentas e, em seguida, gerar o banco de dados a partir dele.

![Database First versus Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First versus Model First")

*Database First versus Model First*

Depois de gerar o modelo, você fará os ajustes adequados no StoreController para fornecer as exibições de armazenamento com os dados extraídos do banco de dados, em vez de usar os dado embutido em código. Você não precisará fazer nenhuma alteração nos modelos de exibição porque o StoreController retornará os mesmos ViewModels para os modelos de exibição, embora desta vez os dados sejam provenientes do banco de dado.

**A abordagem de Code First**

A abordagem Code First nos permite definir o modelo a partir do código sem gerar classes que geralmente são acopladas à estrutura.

No Code First, os objetos de modelo são definidos com POCOs, &quot;objetos CLR antigos simples&quot;. POCOs são classes simples, sem herança, e não implementam interfaces. Podemos gerar automaticamente o banco de dados a partir deles ou podemos usar um banco de dados existente e gerar o mapeamento de classe do código.

Os benefícios de usar essa abordagem é que o modelo permanece independente da estrutura de persistência (nesse caso, Entity Framework), já que as classes POCOs não são acopladas à estrutura de mapeamento.

> [!NOTE]
> Este laboratório se baseia no ASP.NET MVC 4 e em uma versão do aplicativo de exemplo da loja de música personalizada e minimizada para se ajustar apenas aos recursos mostrados neste laboratório prático.
> 
> Se você quiser explorar o aplicativo de tutorial da **loja de música** inteiro, poderá encontrá-lo no [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisites

Você deve ter os seguintes itens para concluir este laboratório:

- [Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou superior (leia [o apêndice a](#AppendixA) para obter instruções sobre como instalá-lo).

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Instalação

**Instalando trechos de código**

Para sua conveniência, grande parte do código que você gerenciará ao longo deste laboratório está disponível como trechos de código do Visual Studio. Para instalar os trechos de código, execute o arquivo **.\Source\Setup\CodeSnippets.VSI** .

Se você não estiver familiarizado com os trechos de Visual Studio Code e quiser saber como usá-los, consulte o apêndice deste documento &quot;[Apêndice C: usando trechos de código](#AppendixC)&quot;.

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Exercícios

Este laboratório prático é composto pelos seguintes exercícios:

1. [Exercício 1: adicionando um banco de dados](#Exercise1)
2. [Exercício 2: Criando um banco de dados usando Code First](#Exercise2)
3. [Exercício 3: consultando o banco de dados com parâmetros](#Exercise3)

> [!NOTE]
> Cada exercício é acompanhado por uma pasta **final** que contém a solução resultante que você deve obter depois de concluir os exercícios. Você pode usar essa solução como um guia se precisar de ajuda adicional para trabalhar com os exercícios.

Tempo estimado para concluir este laboratório: **35 minutos**.

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a>Exercício 1: adicionando um banco de dados

Neste exercício, você aprenderá a adicionar um banco de dados com as tabelas do aplicativo MusicStore à solução a fim de consumir os mesmos. Depois que o banco de dados for gerado com o modelo e adicionado à solução, você modificará a classe StoreController para fornecer o modelo de exibição com os dados extraídos do banco de dado, em vez de usar valores embutidos em código.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a>Tarefa 1-adicionando um banco de dados

Nesta tarefa, você adicionará um banco de dados já criado com as tabelas principais do aplicativo MusicStore à solução.

1. Abra a solução **inicial** localizada na **origem/EX1-AddingADatabaseDBFirst/início/** pasta.

   1. Será necessário baixar alguns pacotes NuGet ausentes antes de continuar. Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.
   2. Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, Compile a solução clicando em **build** | **Compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto. É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.
2. Adicionar arquivo de banco de dados **MvcMusicStore** . Neste laboratório prático, você usará um banco de dados já criado chamado **MvcMusicStore. MDF**. Para fazer isso, clique com o botão direito do mouse em **App\_data** Folder, aponte para **Adicionar** e clique em **Item existente**. Navegue até **\Source\Assets** e selecione o arquivo **MvcMusicStore. MDF** .

    ![Adicionando um item existente](aspnet-mvc-4-models-and-data-access/_static/image2.png "Adicionando um item existente")

    *Adicionando um item existente*

    ![Arquivo de banco de dados MvcMusicStore. MDF](aspnet-mvc-4-models-and-data-access/_static/image3.png "Arquivo de banco de dados MvcMusicStore. MDF")

    *Arquivo de banco de dados MvcMusicStore. MDF*

    O banco de dados foi adicionado ao projeto. Mesmo quando o banco de dados está localizado dentro da solução, você pode consultá-lo e atualizá-lo como hospedado em um servidor de banco de dados diferente.

    ![Banco de dados MvcMusicStore no Gerenciador de Soluções](aspnet-mvc-4-models-and-data-access/_static/image4.png "Banco de dados MvcMusicStore no Gerenciador de Soluções")

    *Banco de dados MvcMusicStore no Gerenciador de Soluções*
3. Verifique a conexão com o banco de dados. Para fazer isso, clique duas vezes em **MvcMusicStore. MDF** para estabelecer uma conexão.

    ![Conectando-se ao MvcMusicStore. MDF](aspnet-mvc-4-models-and-data-access/_static/image5.png "Conectando-se ao MvcMusicStore. MDF")

    *Conectando-se ao MvcMusicStore. MDF*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a>Tarefa 2-Criando um modelo de dados

Nesta tarefa, você criará um modelo de dados para interagir com o banco de dados adicionado na tarefa anterior.

1. Crie um modelo de dados que representará o banco de dado. Para fazer isso, em Gerenciador de Soluções clique com o botão direito do mouse na pasta **modelos** , aponte para **Adicionar** e clique em **novo item**. Na caixa de diálogo **Adicionar novo item** , selecione o modelo de **dados** e, em seguida, **ADO.NET modelo de dados de entidade** item. Altere o nome do modelo de dados para **StoreDB. edmx** e clique em **Adicionar**.

    ![Adicionando StoreDB ADO.NET Modelo de Dados de Entidade](aspnet-mvc-4-models-and-data-access/_static/image6.png "Adicionando StoreDB ADO.NET Modelo de Dados de Entidade")

    *Adicionando StoreDB ADO.NET Modelo de Dados de Entidade*
2. O **Assistente de modelo de dados de entidade** será exibido. Este assistente o guiará pela criação da camada do modelo. Como o modelo deve ser criado com base no banco de dados existente adicionado recentemente, selecione **gerar do banco de dados** e clique em **Avançar**.

    ![Escolhendo o conteúdo do modelo](aspnet-mvc-4-models-and-data-access/_static/image7.png "Escolhendo o conteúdo do modelo")

    *Escolhendo o conteúdo do modelo*
3. Como você está gerando um modelo de um banco de dados, será necessário especificar a conexão a ser usada. Clique em **nova conexão**.
4. Selecione **Microsoft SQL Server arquivo de banco de dados** e clique em **continuar**.

    ![Escolher fonte de dados](aspnet-mvc-4-models-and-data-access/_static/image8.png "Escolher fonte de dados")

    *Caixa de diálogo escolher fonte de dados*
5. Clique em **procurar** e selecione o banco de dados **MvcMusicStore. MDF** que você localizou na pasta **Data\_de aplicativos** e clique em **OK**.

    ![Propriedades da conexão](aspnet-mvc-4-models-and-data-access/_static/image9.png "Propriedades da conexão")

    *Propriedades da conexão*
6. A classe gerada deve ter o mesmo nome que a cadeia de conexão de entidade, portanto, altere seu nome para **MusicStoreEntities** e clique em **Avançar**.

    ![Escolhendo a conexão de dados](aspnet-mvc-4-models-and-data-access/_static/image10.png "Escolhendo a conexão de dados")

    *Escolhendo a conexão de dados*
7. Escolha os objetos de banco de dados a serem usados. Como o modelo de entidade usará apenas as tabelas do banco de dados, selecione a opção **tabelas** e verifique se as opções **incluir colunas de chave estrangeira no modelo** e **Pluralize ou permitir nomes de objeto gerados** também estão selecionadas. Altere o namespace do modelo para **MvcMusicStore. Model** e clique em **concluir**.

    ![Escolhendo os objetos de banco de dados](aspnet-mvc-4-models-and-data-access/_static/image11.png "Escolhendo os objetos de banco de dados")

    *Escolhendo os objetos de banco de dados*

    > [!NOTE]
    > Se uma caixa de diálogo de aviso de segurança for exibida, clique em **OK** para executar o modelo e gerar as classes para as entidades de modelo.
8. Um diagrama de entidade para o banco de dados será exibido, enquanto uma classe separada que mapeia cada tabela para o banco de dados será criada. Por exemplo, a tabela **álbuns** será representada por uma classe de **álbum** , em que cada coluna na tabela será mapeada para uma propriedade de classe. Isso permitirá que você consulte e trabalhe com objetos que representam linhas no banco de dados.

    ![Diagrama de entidade](aspnet-mvc-4-models-and-data-access/_static/image12.png "Diagrama de entidade")

    *Diagrama de entidade*

    > [!NOTE]
    > Os modelos T4 (. TT) executam o código para gerar as classes de entidades e substituirão as classes existentes com o mesmo nome. Neste exemplo, as classes &quot;álbum&quot;, &quot;gênero&quot; e &quot;artista&quot; foram substituídas pelo código gerado.

<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a>Tarefa 3-criando o aplicativo

Nesta tarefa, você verificará que, embora a geração de modelo tenha removido as classes de modelo de **álbum**, **gênero** e **artista** , o projeto é compilado com êxito usando as novas classes de modelo de dados.

1. Crie o projeto selecionando o item de menu **Build** e, em seguida, **Build MvcMusicStore**.

    ![Compilando o projeto](aspnet-mvc-4-models-and-data-access/_static/image13.png "Compilando o projeto")

    *Compilando o projeto*
2. O projeto é compilado com êxito. Por que ele ainda funciona? Ele funciona porque as tabelas de banco de dados têm campos que incluem as propriedades que você estava usando no **álbum** e no **gênero**das classes removidas.

    ![Compilações com êxito](aspnet-mvc-4-models-and-data-access/_static/image14.png "Compilações com êxito")

    *Compilações com êxito*
3. Embora o designer exiba as entidades em um formato de diagrama, elas são C# realmente classes. Expanda o nó **StoreDB. edmx** no Gerenciador de soluções e, em seguida, **StoreDB.tt**, você verá as novas entidades geradas.

    ![Arquivos gerados](aspnet-mvc-4-models-and-data-access/_static/image15.png "Arquivos gerados")

    *Arquivos gerados*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>Tarefa 4-consultando o banco de dados

Nesta tarefa, você atualizará a classe StoreController para que, em vez de usar dados codificados, ele consultará o banco de dado para recuperar as informações.

1. Abra **Controllers\StoreController.cs** e adicione o seguinte campo à classe para manter uma instância da classe **MusicStoreEntities** , chamada **storeDB**:

    (Trecho de código- *modelos e acesso a dados-EX1 storeDB*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
2. A classe **MusicStoreEntities** expõe uma propriedade de coleção para cada tabela no banco de dados. Atualize o método de ação de **procura** para recuperar um gênero com todos os **álbuns**.

    (Trecho de código- *modelos e acesso a dados-navegação do EX1 Store*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

    > [!NOTE]
    > Você está usando uma funcionalidade do .NET chamada **LINQ** (consulta integrada à linguagem) para gravar expressões de consulta fortemente tipadas em relação a essas coleções, o que executará o código no banco de dados e retornará objetos com os quais você pode programar.
    > 
    > Para obter mais informações sobre o LINQ, visite o [site do MSDN](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).
3. Atualizar o método de ação de **índice** para recuperar todos os gêneros.

    (Trecho de código- *modelos e acesso a dados-índice de armazenamento EX1*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
4. Atualizar o método de ação de **índice** para recuperar todos os gêneros e transformar a coleção em uma lista.

    (Trecho de código- *modelos e acesso a dados-EX1 Store GenreMenu*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Tarefa 5-executando o aplicativo

Nesta tarefa, você verificará se a página de índice da loja agora exibirá os gêneros armazenados no banco de dados em vez dos codificados. Não é necessário alterar o modelo de exibição porque o **StoreController** está retornando as mesmas entidades que antes, embora desta vez os dados serão provenientes do banco de dado.

1. Recompile a solução e pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home Page. Verifique se o menu de **gêneros** não é mais uma lista embutida em código e se os dados são recuperados diretamente do banco de dado.

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    *Pesquisando gêneros do banco de dados*
3. Agora, navegue até qualquer gênero e verifique se os álbuns estão populados do banco de dados.

    ![Pesquisando álbuns do banco de dados](aspnet-mvc-4-models-and-data-access/_static/image17.png "Pesquisando álbuns do banco de dados")

    *Pesquisando álbuns do banco de dados*

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a>Exercício 2: Criando um banco de dados usando Code First

Neste exercício, você aprenderá a usar a abordagem de Code First para criar um banco de dados com as tabelas do aplicativo MusicStore e como acessar seus dados.

Depois que o modelo for gerado, você modificará o StoreController para fornecer o modelo de exibição com os dados extraídos do banco de dado, em vez de usar valores codificados.

> [!NOTE]
> Se você concluiu o exercício 1 e já trabalhou com a abordagem de Database First, agora você aprenderá a obter os mesmos resultados com um processo diferente. As tarefas que estão em comum com o exercício 1 foram marcadas para facilitar sua leitura. Se você não concluiu o exercício 1, mas gostaria de aprender a abordagem Code First, poderá iniciar neste exercício e obter uma cobertura completa do tópico.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a>Tarefa 1-preenchendo dados de exemplo

Nesta tarefa, você preencherá o banco de dados com exemplos de dado quando ele for inicialmente criado usando Code-First.

1. Abra a solução **inicial** localizada na **origem/EX2-CreatingADatabaseCodeFirst/início/** pasta. Caso contrário, você pode continuar usando a solução **final** obtida concluindo o exercício anterior.

   1. Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar. Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.
   2. Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, Compile a solução clicando em **build** | **Compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto. É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.
2. Adicione o arquivo **SampleData.cs** à pasta **modelos** . Para fazer isso, clique com o botão direito do mouse na pasta **modelos** , aponte para **Adicionar** e clique em **Item existente**. Navegue até **\Source\Assets** e selecione o arquivo **SampleData.cs** .

    ![Código de preenchimento de dados de exemplo](aspnet-mvc-4-models-and-data-access/_static/image18.png "Código de preenchimento de dados de exemplo")

    *Código de preenchimento de dados de exemplo*
3. Abra o arquivo **global.asax.cs** e adicione as instruções *using* a seguir.

    (Trecho de código- *modelos e acesso a dados-EX2 global asax usa*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
4. No **aplicativo\_método Start ()** , adicione a linha a seguir para definir o inicializador de banco de dados.

    (Trecho de código- *modelos e acesso a dados – EX2 global asax setinitialize*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a>Tarefa 2-Configurando a conexão com o banco de dados

Agora que você já adicionou um banco de dados ao nosso projeto, escreverá no arquivo **Web. config** a cadeia de conexão.

1. Adicione uma cadeia de conexão em **Web. config**. Para fazer isso, abra o **Web. config** na raiz do projeto e substitua a cadeia de conexão chamada DefaultConnection por essa linha na seção **&lt;connectionStrings&gt;** :

    ![Local do arquivo Web. config](aspnet-mvc-4-models-and-data-access/_static/image19.png "Local do arquivo Web. config")

    *Local do arquivo Web. config*

    [!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a>Tarefa 3-trabalhando com o modelo

Agora que você já configurou a conexão com o banco de dados, você vinculará o modelo com as tabelas de banco de dados. Nesta tarefa, você criará uma classe que será vinculada ao banco de dados com Code First. Lembre-se de que há uma classe de modelo POCO existente que deve ser modificada.

> [!NOTE]
> Se você concluiu o exercício 1, observará que essa etapa foi executada por um assistente. Ao fazer Code First, você criará manualmente as classes que serão vinculadas a entidades de dados.

1. Abra o **gênero** classe de modelo poco da pasta **modelos** projeto e inclua uma ID. Use uma propriedade int com o nome **gêneroid**.

    (Trecho de código- *modelos e acesso a dados-Ex2 Code First gênero*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

    > [!NOTE]
    > Para trabalhar com as convenções de Code First, o gênero de classe deve ter uma propriedade de chave primária que será detectada automaticamente.
    > 
    > Você pode ler mais sobre as convenções de Code First neste [artigo do MSDN](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).
2. Agora, abra o **álbum** de classes do modelo poco na pasta **modelos** do projeto e inclua as chaves estrangeiras, crie propriedades com os nomes **gêneroid** e **artistaid**. Esta classe já tem o **gêneroid** para a chave primária.

    (Trecho de código- *modelos e acesso a dados-Ex2 Code First álbum*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
3. Abra o **artista** de classe do modelo poco e inclua a propriedade **artistaid** .

    (Trecho de código- *modelos e acesso a dados-Ex2 Code First artista*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
4. Clique com o botão direito do mouse na pasta **modelos** e selecione **Adicionar | Classe**. Nomeie o arquivo **MusicStoreEntities.cs**. Em seguida, clique em **Adicionar.**

    ![Adicionando uma classe](aspnet-mvc-4-models-and-data-access/_static/image20.png "Adicionando uma classe")

    *Adicionando um novo item*

    ![Adicionando um class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Adicionando um class2")

    *Adicionando uma classe*
5. Abra a classe que você acabou de criar, **MusicStoreEntities.cs**e inclua os namespaces **System. Data. Entity** e **System. Data. Entity. Infrastructure**.

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
6. Substitua a declaração de classe para estender a classe **DbContext** : declare um **DBSet** público e substitua o método **OnModelCreating** . Após essa etapa, você obterá uma classe de domínio que vinculará seu modelo ao Entity Framework. Para fazer isso, substitua o código de classe pelo seguinte:

    (Trecho de código- *modelos e acesso a dados-Ex2 Code First MusicStoreEntities*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> Com Entity Framework **DbContext** e **DBSet** , você poderá consultar o gênero da classe poco. Ao estender o método **OnModelCreating** , você está especificando no **código** como o gênero será mapeado para uma tabela de banco de dados. Você pode encontrar mais informações sobre DBContext e DBSet neste artigo do MSDN: [link](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>Tarefa 4-consultando o banco de dados

Nesta tarefa, você atualizará a classe StoreController para que, em vez de usar os dados codificados, ele a recuperará do banco de dado.

> [!NOTE]
> Essa tarefa é comum com o exercício 1.
> 
> Se você concluiu o exercício 1, observará que essas etapas são as mesmas em ambas as abordagens (primeiro banco de dados ou código primeiro). Eles são diferentes na forma como os dados são vinculados ao modelo, mas o acesso às entidades de dados ainda é transparente do controlador.

1. Abra **Controllers\StoreController.cs** e adicione o seguinte campo à classe para manter uma instância da classe **MusicStoreEntities** , chamada **storeDB**:

    (Trecho de código- *modelos e acesso a dados-EX1 storeDB*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
2. A classe **MusicStoreEntities** expõe uma propriedade de coleção para cada tabela no banco de dados. Atualize o método de ação de **procura** para recuperar um gênero com todos os **álbuns**.

    (Trecho de código- *modelos e acesso a dados-navegação do EX2 Store*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

    > [!NOTE]
    > Você está usando uma funcionalidade do .NET chamada **LINQ** (consulta integrada à linguagem) para gravar expressões de consulta fortemente tipadas em relação a essas coleções, o que executará o código no banco de dados e retornará objetos com os quais você pode programar.
    > 
    > Para obter mais informações sobre o LINQ, visite o [site do MSDN](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).
3. Atualizar o método de ação de **índice** para recuperar todos os gêneros.

    (Trecho de código- *modelos e acesso a dados-índice de armazenamento EX2*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
4. Atualizar o método de ação de **índice** para recuperar todos os gêneros e transformar a coleção em uma lista.

    (Trecho de código- *modelos e acesso a dados-EX2 Store GenreMenu*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Tarefa 5-executando o aplicativo

Nesta tarefa, você verificará se a página de índice da loja agora exibirá os gêneros armazenados no banco de dados em vez dos codificados. Não é necessário alterar o modelo de exibição porque o **StoreController** está retornando o mesmo **StoreIndexViewModel** que antes, mas desta vez os dados serão provenientes do banco de dado.

1. Recompile a solução e pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home Page. Verifique se o menu de **gêneros** não é mais uma lista embutida em código e se os dados são recuperados diretamente do banco de dado.

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    *Pesquisando gêneros do banco de dados*
3. Agora, navegue até qualquer gênero e verifique se os álbuns estão populados do banco de dados.

    ![Pesquisando álbuns do banco de dados](aspnet-mvc-4-models-and-data-access/_static/image23.png "Pesquisando álbuns do banco de dados")

    *Pesquisando álbuns do banco de dados*

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a>Exercício 3: consultando o banco de dados com parâmetros

Neste exercício, você aprenderá a consultar o banco de dados usando parâmetros e a usar a formatação de resultados da consulta, um recurso que reduz o número de acessos de banco de dados que recuperam os mesmos de maneira mais eficiente.

> [!NOTE]
> Para obter mais informações sobre o formato de resultado da consulta, visite o seguinte [artigo do MSDN](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a>Tarefa 1-modificando StoreController para recuperar álbuns do banco de dados

Nesta tarefa, você vai alterar a classe **StoreController** para acessar o banco de dados para recuperar álbuns de um gênero específico.

1. Abra a solução **inicial** localizada na pasta **Source\Ex3-QueryingTheDatabaseWithParametersCodeFirst\Begin** se você quiser usar a abordagem de primeiro código ou a pasta **Source\Ex3-QueryingTheDatabaseWithParametersDBFirst\Begin** se quiser usar a abordagem do banco de dados primeiro. Caso contrário, você pode continuar usando a solução **final** obtida concluindo o exercício anterior.

   1. Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar. Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.
   2. Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, Compile a solução clicando em **build** | **Compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto. É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.
2. Abra a classe **StoreController** para alterar o método de ação de **procura** . Para fazer isso, na **Gerenciador de soluções**, expanda a pasta **controladores** e clique duas vezes em **StoreController.cs**.
3. Altere o método de ação **procurar** para recuperar álbuns de um gênero específico. Para fazer isso, substitua o seguinte código:

    (Trecho de código- *modelos e acesso a dados-EX3 StoreController BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> Para preencher uma coleção da entidade, você precisa usar o método **include** para especificar que deseja recuperar os álbuns também. Você pode usar o. Extensão **Single ()** no LINQ porque, nesse caso, apenas um gênero é esperado para um álbum. O método **Single ()** usa uma expressão lambda como um parâmetro, que nesse caso especifica um único objeto de gênero, de modo que seu nome corresponde ao valor definido.
> 
> Você aproveitará um recurso que permite que você indique outras entidades relacionadas que deseja carregar também quando o objeto gênero for recuperado. Esse recurso é chamado de **formato de resultado de consulta**e permite reduzir o número de vezes necessário para acessar o banco de dados para recuperar informações. Nesse cenário, você desejará buscar previamente os álbuns para o gênero recuperado.
> 
> A consulta inclui **gêneros. include (&quot;álbuns&quot;)** para indicar que você também deseja álbuns relacionados. Isso resultará em um aplicativo mais eficiente, pois ele recuperará os dados de gênero e de álbum em uma única solicitação de banco de dado.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Tarefa 2-executando o aplicativo

Nesta tarefa, você executará o aplicativo e recuperará os álbuns de um gênero específico do banco de dados.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home Page. Altere a URL para **/Store/Browse? gênero = pop** para verificar se os resultados estão sendo recuperados do banco de dados.

    ![Pesquisando por gênero](aspnet-mvc-4-models-and-data-access/_static/image24.png "Pesquisando por gênero")

    *Navegando em/Store/Browse? gênero = pop*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a>Tarefa 3-acessando álbuns por ID

Nesta tarefa, você repetirá o procedimento anterior para obter álbuns por sua ID.

1. Feche o navegador, se necessário, para retornar ao Visual Studio. Abra a classe **StoreController** para alterar o método de ação de **detalhes** . Para fazer isso, na **Gerenciador de soluções**, expanda a pasta **controladores** e clique duas vezes em **StoreController.cs**.
2. Altere o método de ação de **detalhes** para recuperar os detalhes dos álbuns com base em sua **ID**. Para fazer isso, substitua o seguinte código:

    (Trecho de código- *modelos e acesso a dados-EX3 StoreController DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Tarefa 4-executando o aplicativo

Nesta tarefa, você executará o aplicativo em um navegador da Web e obterá detalhes do álbum por sua ID.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home Page. Altere a URL para **/Store/Details/51** ou navegue pelos gêneros e selecione um álbum para verificar se os resultados estão sendo recuperados do banco de dados.

    ![Detalhes da navegação](aspnet-mvc-4-models-and-data-access/_static/image25.png "Detalhes da navegação")

    *Navegando em/Store/Details/51*

> [!NOTE]
> Além disso, você pode implantar esse aplicativo em sites do Windows Azure seguindo [o apêndice B: publicando um aplicativo ASP.NET MVC 4 usando implantação da Web](#AppendixB).

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Resumo

Ao concluir este laboratório prático, você aprendeu os conceitos básicos dos modelos e do acesso a dados do ASP.NET MVC, usando a abordagem de **Database First** , bem como a abordagem de **Code First** :

- Como adicionar um banco de dados à solução a fim de consumir seu dado
- Como atualizar controladores para fornecer modelos de exibição com os dados extraídos do banco de dado em vez de embutidos em código um
- Como consultar o banco de dados usando parâmetros
- Como usar o formato de resultado da consulta, um recurso que reduz o número de acessos ao banco de dados, recuperando os dados de maneira mais eficiente
- Como usar as abordagens Database First e Code First no Microsoft Entity Framework para vincular o banco de dados ao modelo

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apêndice A: Instalando o Visual Studio Express 2012 para Web

Você pode instalar o **Microsoft Visual Studio Express 2012 para Web** ou outra versão &quot;Express&quot; usando o **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** . As instruções a seguir orientam você pelas etapas necessárias para instalar o *Visual Studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.

1. Vá para [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Como alternativa, se você já tiver instalado o Web Platform Installer, poderá abri-lo e pesquisar o produto &quot;<em>Visual Studio Express 2012 para Web com o SDK do Windows Azure</em>&quot;.
2. Clique em **instalar agora**. Se você não tiver **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.
3. Quando **Web Platform Installer** estiver aberto, clique em **instalar** para iniciar a instalação.

    ![Instalar Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "Instalar Visual Studio Express")

    *Instalar Visual Studio Express*
4. Leia todos os termos e licenças de produtos e clique em **aceito** para continuar.

    ![Aceitando os termos de licença](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    *Aceitando os termos de licença*
5. Aguarde até que o processo de download e instalação seja concluído.

    ![Progresso da instalação](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    *Progresso da instalação*
6. Quando a instalação for concluída, clique em **concluir**.

    ![Instalação concluída](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    *Instalação concluída*
7. Clique em **sair** para fechar Web Platform Installer.
8. Para abrir o Visual Studio Express para Web, vá para a tela **Iniciar** e comece a escrever &quot;**vs Express**&quot;e, em seguida, clique no bloco **vs Express para Web** .

    ![Bloco VS Express para Web](aspnet-mvc-4-models-and-data-access/_static/image30.png)

    *Bloco VS Express para Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Apêndice B: publicando um aplicativo ASP.NET MVC 4 usando Implantação da Web

Este apêndice mostrará como criar um novo site da Web do Windows Azure Portal de Gerenciamento e publicar o aplicativo obtido seguindo o laboratório, aproveitando o recurso de publicação de Implantação da Web fornecido pelo Windows Azure.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Tarefa 1-Criando um novo site no portal do Windows Azure

1. Acesse o [portal de gerenciamento do Windows Azure](https://manage.windowsazure.com/) e entre usando as credenciais da Microsoft associadas à sua assinatura.

    > [!NOTE]
    > Com o Windows Azure, você pode hospedar 10 sites da Web de ASP.NET gratuitamente e, em seguida, dimensionar conforme o tráfego cresce. Você pode se inscrever [aqui](https://aka.ms/aspnet-hol-azure).

    ![Fazer logon no Windows portal do Azure](aspnet-mvc-4-models-and-data-access/_static/image31.png "Fazer logon no Windows portal do Azure")

    *Fazer logon no Windows Azure Portal de Gerenciamento*
2. Clique em **novo** na barra de comandos.

    ![Criando um novo site](aspnet-mvc-4-models-and-data-access/_static/image32.png "Criando um novo site")

    *Criando um novo site*
3. Clique em **computação** | **site**. Em seguida, selecione a opção **criação rápida** . Forneça uma URL disponível para o novo site e clique em **criar site**.

    > [!NOTE]
    > Um site do Windows Azure é o host de um aplicativo Web em execução na nuvem que você pode controlar e gerenciar. A opção criação rápida permite que você implante um aplicativo Web concluído no site do Windows Azure de fora do Portal. Ele não inclui etapas para configurar um banco de dados.

    ![Criando um novo site usando a criação rápida](aspnet-mvc-4-models-and-data-access/_static/image33.png "Criando um novo site usando a criação rápida")

    *Criando um novo site usando a criação rápida*
4. Aguarde até que o novo **site** seja criado.
5. Depois que o site for criado, clique no link sob a coluna **URL** . Verifique se o novo site está funcionando.

    ![Navegando até o novo site](aspnet-mvc-4-models-and-data-access/_static/image34.png "Navegando até o novo site")

    *Navegando até o novo site*

    ![Site em execução](aspnet-mvc-4-models-and-data-access/_static/image35.png "Site em execução")

    *Site em execução*
6. Volte para o portal e clique no nome do site na coluna **nome** para exibir as páginas de gerenciamento.

    ![Abrindo as páginas de gerenciamento de site](aspnet-mvc-4-models-and-data-access/_static/image36.png "Abrindo as páginas de gerenciamento de site")

    *Abrindo as páginas de gerenciamento de site*
7. Na página **painel** , na seção **visão rápida** , clique no link **baixar perfil de publicação** .

    > [!NOTE]
    > O *perfil de publicação* contém todas as informações necessárias para publicar um aplicativo Web em um site do Windows Azure para cada método de publicação habilitado. O perfil de publicação contém as URLs, as credenciais de usuário e as cadeias de conexão de banco de dados necessárias para conectar-se e autenticar cada um dos pontos de extremidade para os quais um método de publicação é habilitado. **O Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** e **Microsoft Visual Studio 2012** dão suporte à leitura de perfis de publicação para automatizar a configuração desses programas para a publicação de aplicativos Web em sites do Windows Azure.

    ![Baixando o perfil de publicação do site](aspnet-mvc-4-models-and-data-access/_static/image37.png "Baixando o perfil de publicação do site")

    *Baixando o perfil de publicação do site*
8. Baixe o arquivo de perfil de publicação em um local conhecido. Neste exercício, você verá como usar esse arquivo para publicar um aplicativo Web em um site do Windows Azure a partir do Visual Studio.

    ![Salvando o arquivo de perfil de publicação](aspnet-mvc-4-models-and-data-access/_static/image38.png "Salvando o perfil de publicação")

    *Salvando o arquivo de perfil de publicação*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tarefa 2-Configurando o servidor de banco de dados

Se seu aplicativo utiliza bancos de dados SQL Server, você precisará criar um servidor de banco de dados SQL. Se você quiser implantar um aplicativo simples que não usa SQL Server você pode ignorar essa tarefa.

1. Você precisará de um servidor do banco de dados SQL para armazenar o banco de dados do aplicativo. Você pode exibir os servidores do banco de dados SQL de sua assinatura no portal de gerenciamento do Windows Azure em bancos de dados **sql** | **servidores** | **painel do servidor**. Se você não tiver um servidor criado, poderá criar um usando o botão **Adicionar** na barra de comandos. Anote o nome do **servidor e a URL, o nome de logon e a senha do administrador**, pois você irá usá-los nas próximas tarefas. Não crie o banco de dados ainda, pois ele será criado em um estágio posterior.

    ![Painel do servidor do banco de dados SQL](aspnet-mvc-4-models-and-data-access/_static/image39.png "Painel do servidor do banco de dados SQL")

    *Painel do servidor do banco de dados SQL*
2. Na próxima tarefa, você testará a conexão de banco de dados do Visual Studio, por esse motivo, será necessário incluir o endereço IP local na lista de **endereços IP permitidos**do servidor. Para fazer isso, clique em **Configurar**, selecione o endereço IP do **endereço IP do cliente atual** e cole-o nas caixas de texto endereço IP **inicial** e **endereço IP final** e clique no botão ![adicionar-cliente-IP-endereço-OK-botão](aspnet-mvc-4-models-and-data-access/_static/image40.png).

    ![Adicionando endereço IP do cliente](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    *Adicionando endereço IP do cliente*
3. Depois que o **endereço IP do cliente** for adicionado à lista endereços IP permitidos, clique em **salvar** para confirmar as alterações.

    ![Confirmar alterações](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    *Confirmar alterações*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tarefa 3-publicando um aplicativo ASP.NET MVC 4 usando Implantação da Web

1. Volte para a solução ASP.NET MVC 4. Na **Gerenciador de soluções**, clique com o botão direito do mouse no projeto de site e selecione **publicar**.

    ![Publicando o aplicativo](aspnet-mvc-4-models-and-data-access/_static/image43.png "Publicando o aplicativo")

    *Publicando o site*
2. Importe o perfil de publicação salvo na primeira tarefa.

    ![Importando o perfil de publicação](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importando o perfil de publicação")

    *Importando perfil de publicação*
3. Clique em **validar conexão**. Depois que a validação for concluída, clique em **Avançar**.

    > [!NOTE]
    > A validação será concluída quando você vir uma marca de seleção verde ao lado do botão Validar conexão.

    ![Validando conexão](aspnet-mvc-4-models-and-data-access/_static/image45.png "Validando conexão")

    *Validando conexão*
4. Na página **configurações** , na seção **bancos** de dados, clique no botão ao lado da caixa de texto da sua conexão de banco (ou seja, **DefaultConnection**).

    ![Configuração de implantação da Web](aspnet-mvc-4-models-and-data-access/_static/image46.png "Configuração de implantação da Web")

    *Configuração de implantação da Web*
5. Configure a conexão de banco de dados da seguinte maneira:

   - No **nome do servidor** , digite a URL do servidor do banco de dados SQL usando o prefixo *TCP:* .
   - Em **nome de usuário** , digite o nome de logon do administrador do servidor.
   - Em **senha** , digite a senha de logon do administrador do servidor.
   - Digite um novo nome de banco de dados.

     ![Configurando a cadeia de conexão de destino](aspnet-mvc-4-models-and-data-access/_static/image47.png "Configurando a cadeia de conexão de destino")

     *Configurando a cadeia de conexão de destino*
6. Em seguida, clique em **OK**. Quando for solicitado a criar o banco de dados, clique em **Sim**.

    ![Criando o banco de dados](aspnet-mvc-4-models-and-data-access/_static/image48.png "Criando a cadeia de caracteres do banco de dados")

    *Criando o banco de dados*
7. A cadeia de conexão que você usará para se conectar ao banco de dados SQL no Windows Azure é mostrada na caixa de texto conexão padrão. Em seguida, clique em **Próximo**.

    ![Cadeia de conexão apontando para o banco de dados SQL](aspnet-mvc-4-models-and-data-access/_static/image49.png "Cadeia de conexão apontando para o banco de dados SQL")

    *Cadeia de conexão apontando para o banco de dados SQL*
8. Na página **Visualização** , clique em **publicar**.

    ![Publicando o aplicativo Web](aspnet-mvc-4-models-and-data-access/_static/image50.png "Publicando o aplicativo Web")

    *Publicando o aplicativo Web*
9. Quando o processo de publicação for concluído, o navegador padrão abrirá o site publicado.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Apêndice C: usando trechos de código

Com trechos de código, você tem todo o código de que precisa ao seu alcance. O documento de laboratório informará exatamente quando você pode usá-los, conforme mostrado na figura a seguir.

![Usando trechos de código do Visual Studio para inserir código em seu projeto](aspnet-mvc-4-models-and-data-access/_static/image51.png "Usando trechos de código do Visual Studio para inserir código em seu projeto")

*Usando trechos de código do Visual Studio para inserir código em seu projeto*

***Para adicionar um trecho de código usando o tecladoC# (somente)***

1. Coloque o cursor onde você deseja inserir o código.
2. Comece digitando o nome do trecho de código (sem espaços ou hifens).
3. Observe como o IntelliSense exibe nomes de trechos de código correspondentes.
4. Selecione o trecho correto (ou continue digitando até que o nome do trecho inteiro seja selecionado).
5. Pressione a tecla TAB duas vezes para inserir o trecho de código no local do cursor.

![Comece a digitar o nome do trecho](aspnet-mvc-4-models-and-data-access/_static/image52.png "Comece a digitar o nome do trecho")

*Comece a digitar o nome do trecho*

![Pressione Tab para selecionar o trecho realçado](aspnet-mvc-4-models-and-data-access/_static/image53.png "Pressione Tab para selecionar o trecho realçado")

*Pressione Tab para selecionar o trecho realçado*

![Pressione Tab novamente e o trecho será expandido](aspnet-mvc-4-models-and-data-access/_static/image54.png "Pressione Tab novamente e o trecho será expandido")

*Pressione Tab novamente e o trecho será expandido*

***Para adicionar um trecho de código usando o mouseC#(, Visual Basic e XML)*** uma. Clique com o botão direito do mouse no local em que você deseja inserir o trecho de código.

1. Selecione **Inserir trecho** seguido por **meus trechos de código**.
2. Selecione o trecho relevante na lista clicando nele.

![Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho](aspnet-mvc-4-models-and-data-access/_static/image55.png "Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho")

*Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho*

![Selecione o trecho relevante na lista clicando nele](aspnet-mvc-4-models-and-data-access/_static/image56.png "Selecione o trecho relevante na lista clicando nele")

*Selecione o trecho relevante na lista clicando nele*
