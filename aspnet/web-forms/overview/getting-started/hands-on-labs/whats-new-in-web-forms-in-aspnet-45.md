---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
title: O que há de novo na Web Forms no ASP.NET 4,5 | Microsoft Docs
author: rick-anderson
description: A nova versão do ASP.NET Web Forms introduz uma série de aprimoramentos focados na melhoria da experiência do usuário ao trabalhar com dados. Nas versões anteriores do...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 0a1f88bd-97da-4ed1-86f1-605199dc75a4
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: 301af8ed877b58624e419c04f605c41f27dbdd0c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78525728"
---
# <a name="whats-new-in-web-forms-in-aspnet-45"></a>Novidades em Web Forms no ASP.NET 4.5

por [equipe de acampamentos da Web](https://twitter.com/webcamps)

> A nova versão do ASP.NET Web Forms introduz uma série de aprimoramentos focados na melhoria da experiência do usuário ao trabalhar com dados.
> 
> Nas versões anteriores do Web Forms, ao usar a vinculação de dados para emitir o valor de um membro de objeto, você usou as expressões de vinculação de dados BIND () ou eval (). Na nova versão do ASP.NET, você pode declarar a quais tipos de dados um controle será associado usando uma nova Propriedade ItemType... Definir essa propriedade permitirá que você use uma variável fortemente tipada para receber os benefícios completos da experiência de desenvolvimento do Visual Studio, como o IntelliSense, a navegação de membros e a verificação de tempo de compilação.
> 
> Com os controles associados a dados, agora você também pode especificar seus próprios métodos personalizados para selecionar, atualizar, excluir e inserir dados, simplificando a interação entre os controles da página e a lógica do aplicativo. Além disso, os recursos de associação de modelo foram adicionados ao ASP.NET, o que significa que você pode mapear dados da página diretamente para parâmetros de tipo de método.
> 
> A validação da entrada do usuário também deve ser mais fácil com a versão mais recente do Web Forms. Agora você pode anotar suas classes de modelo com atributos de validação do namespace **System. ComponentModel. Annotations** e solicitar que todos os controles do site validem a entrada do usuário usando essas informações. A validação no lado do cliente no Web Forms agora é integrada ao jQuery, fornecendo código de cliente mais limpo e recursos de JavaScript discretos.
> 
> Na área de validação de solicitação, foram feitos aprimoramentos para facilitar a desativação seletiva da validação de solicitação para partes específicas de seus aplicativos ou a leitura de dados de solicitação invalidados.
> 
> Foram feitas algumas melhorias nos controles de servidor Web Forms para aproveitar os novos recursos do HTML5:
> 
> - A propriedade TextMode do controle TextBox foi atualizada para dar suporte aos novos tipos de entrada do HTML5, como email, DateTime e assim por diante.
> - O controle FileUpload agora dá suporte a vários carregamentos de arquivo de navegadores que dão suporte a esse recurso HTML5.
> - Os controles de validação agora dão suporte à validação de elementos de entrada do HTML5.
> - Os novos elementos HTML5 que têm atributos que representam uma URL agora dão suporte a runat =&quot;Server&quot;. Como resultado, você pode usar as convenções ASP.NET em caminhos de URL, como o operador ~ para representar a raiz do aplicativo (por exemplo, &lt;vídeo runat =&quot;Server&quot; src =&quot;~/myVideo.wmv&quot;&gt;&lt;/Video&gt;).
> - O controle UpdatePanel foi corrigido para dar suporte ao lançamento de campos de entrada HTML5.
> 
> No portal oficial do ASP.NET, você pode encontrar mais exemplos dos novos recursos no ASP.NET WebForms 4,5: [novidades no ASP.NET 4,5 e no Visual Studio 2012](../../../../whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012.md#_Toc318097385)
> 
> Todos os códigos de exemplo e trechos de código estão incluídos no kit de treinamento do acampamentos da Web, disponível em [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

Neste laboratório prático, você aprenderá a:

- Usar expressões de vinculação de dados fortemente tipadas
- Usar novos recursos de associação de modelo no Web Forms
- Usar provedores de valor para mapear dados de página para métodos code-behind
- Usar anotações de dados para validação de entrada do usuário
- Aproveite a validação do lado do cliente não invasiva com o jQuery no Web Forms
- Implementar a validação de solicitação granular
- Implementar o processamento de página assíncrona no Web Forms

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisites

Você deve ter os seguintes itens para concluir este laboratório:

- [Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou superior (leia [o apêndice a](#AppendixA) para obter instruções sobre como instalá-lo).

<a id="Setup"></a>
### <a name="setup"></a>Instalação

**Instalando trechos de código**

Para sua conveniência, grande parte do código que você gerenciará ao longo deste laboratório está disponível como trechos de código do Visual Studio. Para instalar os trechos de código, execute o arquivo **.\Source\Setup\CodeSnippets.VSI** .

Se você não estiver familiarizado com os trechos de Visual Studio Code e quiser saber como usá-los, consulte o apêndice deste documento &quot;[Apêndice C: usando trechos de código](#AppendixC)&quot;.

<a id="Exercises"></a>
## <a name="exercises"></a>Exercícios

Este laboratório prático inclui os seguintes exercícios:

1. [Exercício 1: Associação de modelo no ASP.NET Web Forms](#Exercise1)
2. [Exercício 2: validação de dados](#Exercise2)
3. [Exercício 3: processamento de página assíncrona no ASP.NET Web Forms](#Exercise3)

> [!NOTE]
> Cada exercício é acompanhado por uma pasta **final** que contém a solução resultante que você deve obter depois de concluir os exercícios. Você pode usar essa solução como um guia se precisar de ajuda adicional para trabalhar com os exercícios.

Tempo estimado para concluir este laboratório: **60 minutos**.

<a id="Exercise1"></a>

<a id="Exercise_1_Model_Binding_in_ASPNET_Web_Forms"></a>
### <a name="exercise-1-model-binding-in-aspnet-web-forms"></a>Exercício 1: Associação de modelo no ASP.NET Web Forms

A nova versão do ASP.NET Web Forms apresenta vários aprimoramentos focados na melhoria da experiência ao trabalhar com dados. Ao longo deste exercício, você aprenderá sobre controles de dados e Associação de modelo com rigidez de tipos.

<a id="Task_1_-_Using_Strongly-Typed_Data-Bindings"></a>
#### <a name="task-1---using-strongly-typed-data-bindings"></a>Tarefa 1-usando associações de dados com rigidez de tipos

Nesta tarefa, você descobrirá as novas associações fortemente tipadas disponíveis no ASP.NET 4,5.

1. Abra a solução **inicial** localizada em **origem/EX1-modelbinding/início/** pasta.

   1. Será necessário baixar alguns pacotes NuGet ausentes antes de continuar. Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.
   2. Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, Compile a solução clicando em **build** | **Compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto. É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.
2. Abra a página **Customers. aspx** . Coloque uma lista não numerada no controle principal e inclua um controle Repeater dentro para listar cada cliente. Defina o nome do repetidor como **customersRepeater** , conforme mostrado no código a seguir.

    Nas versões anteriores do Web Forms, ao usar a vinculação de dados para emitir o valor de um membro em um objeto ao qual você está vinculando dados, você usaria uma expressão de vinculação de dados, juntamente com uma chamada para o método eval, passando o nome do membro como uma cadeia de caracteres.

    Em tempo de execução, essas chamadas para eval usarão a reflexão em relação ao objeto atualmente associado para ler o valor do membro com o nome fornecido e exibir o resultado no HTML. Essa abordagem torna muito fácil a vinculação de dados contra dados arbitrários e não formados.

    Infelizmente, você perde muitos dos ótimos recursos de experiência em tempo de desenvolvimento no Visual Studio, incluindo IntelliSense para nomes de membros, suporte para navegação (como ir para definição) e verificação de tempo de compilação.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample1.aspx)]
3. Abra o arquivo **Customers.aspx.cs** .
4. Adicione a seguinte instrução using.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample2.cs)]
5. Na **página\_método Load** , adicione o código para preencher o repetidor com a lista de clientes.

    (Trecho de código- *fonte de dados de Web Forms Lab-Ex01-BIND Customers*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample3.cs)]

    A solução usa o EntityFramework junto com o CodeFirst para criar e acessar o banco de dados. No código a seguir, o customersRepeater está associado a uma consulta materializada que retorna todos os clientes do banco de dados.
6. Pressione **F5** para executar a solução e vá para a página **clientes** para ver o repetidor em ação. Como a solução está usando o CodeFirst, o banco de dados será criado e preenchido na instância local do SQL Express ao executar o aplicativo.

    ![Listando os clientes com um repetidor](whats-new-in-web-forms-in-aspnet-45/_static/image1.png "Listando os clientes com um repetidor")

    *Listando os clientes com um repetidor*

    > [!NOTE]
    > No Visual Studio 2012, IIS Express é o servidor de desenvolvimento da Web padrão.
7. Feche o navegador e volte para o Visual Studio.
8. Agora, substitua a implementação para usar associações com rigidez de tipos. Abra a página **Customers. aspx** e use o novo atributo **ItemType** no Repeater para definir o tipo **Customer** como o tipo de associação.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample4.aspx)]

    A Propriedade ItemType permite que você declare a qual tipo de dados o controle será associado e permite que você use a associação de tipo forte dentro do controle associado a dados.
9. Substitua o conteúdo ItemTemplate pelo código a seguir.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample5.aspx)]

    Uma desvantagem das abordagens acima é que as chamadas para Eval () e BIND () são de associação tardia, o que significa que você passa cadeias de caracteres para representar os nomes das propriedades. Isso significa que você não obtém o IntelliSense para os nomes de membro, suporte para navegação de código (como ir para definição), nem suporte para verificação de tempo de compilação.

    Definir a Propriedade ItemType faz com que duas novas variáveis de tipo sejam geradas no escopo das expressões de vinculação de dados: **Item** e **BindItem**. Você pode usar essas variáveis com rigidez de tipos nas expressões de vinculação de dados e obter os benefícios completos da experiência de desenvolvimento do Visual Studio.

    O &quot; **:** &quot; usado na expressão irá codificar automaticamente a saída em HTML para evitar problemas de segurança (por exemplo, ataques de script entre sites). Essa notação estava disponível desde o .NET 4 para gravação de resposta, mas agora também está disponível em expressões de ligação de dados.

    > [!NOTE]
    > O membro do item funciona para uma associação unidirecional. Se você quiser executar a ligação bidirecional, use o membro **BindItem** .

    ![Suporte IntelliSense em associação fortemente tipada](whats-new-in-web-forms-in-aspnet-45/_static/image2.png "Suporte IntelliSense em associação fortemente tipada")

    *Suporte IntelliSense em associação fortemente tipada*
10. Pressione **F5** para executar a solução e vá para a página clientes para verificar se as alterações funcionam conforme o esperado.

    ![Listando detalhes do cliente](whats-new-in-web-forms-in-aspnet-45/_static/image3.png "Listando detalhes do cliente")

    *Listando detalhes do cliente*
11. Feche o navegador e volte para o Visual Studio.

<a id="Task_2_-_Introducing_Model_Binding_in_Web_Forms"></a>
#### <a name="task-2---introducing-model-binding-in-web-forms"></a>Tarefa 2-Introdução à associação de modelo no Web Forms

Nas versões anteriores do ASP.NET Web Forms, quando você quisesse executar a vinculação de dados bidirecional, tanto a recuperação quanto a atualização de dados, você precisava usar um objeto de fonte de dados. Pode ser uma fonte de dados de objeto, uma fonte de dados SQL, uma fonte de dados LINQ e assim por diante. No entanto, se o seu cenário exigir código personalizado para lidar com os dados, você precisava usar a fonte de dados do objeto e isso trouxe algumas desvantagens. Por exemplo, você precisava evitar tipos complexos e precisava tratar exceções ao executar a lógica de validação.

Na nova versão do ASP.NET Web Forms os controles associados a dados oferecem suporte à associação de modelo. Isso significa que você pode especificar os métodos Select, Update, INSERT e Delete diretamente no controle associado a dados para chamar a lógica do arquivo code-behind ou de outra classe.

Para saber mais sobre isso, você usará um GridView para listar as categorias de produto usando o novo atributo **SelectMethod** . Esse atributo permite que você especifique um método para recuperar os dados GridView.

1. Abra a página **Products. aspx** e inclua um **GridView**. Configure o GridView conforme mostrado abaixo para usar associações fortemente tipadas e habilitar a classificação e paginação.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample6.aspx)]
2. Use o novo atributo **SelectMethod** para configurar o GridView para chamar um método **GetCategories** para selecionar os dados.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample7.aspx)]
3. Abra o arquivo code-behind **Products.aspx.cs** e adicione as instruções using a seguir.

    (Trecho de código- *Web Forms Lab-Ex01-namespaces*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample8.cs)]
4. Adicione um membro privado na classe **Products** e atribua uma nova instância de **ProductsContext**. Essa propriedade irá armazenar o Entity Framework contexto de dados que permite que você se conecte ao banco de dado.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample9.cs)]
5. Crie um método **GetCategories** para recuperar a lista de categorias usando LINQ. A consulta incluirá a propriedade **Products** para que o GridView possa mostrar a quantidade de produtos para cada categoria. Observe que o método retorna um objeto IQueryable bruto que representa a consulta a ser executada posteriormente no ciclo de vida da página.

    (Trecho de código- *Web Forms Lab-Ex01-GetCategories*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample10.cs)]

    > [!NOTE]
    > Nas versões anteriores do ASP.NET Web Forms, habilitar a classificação e paginação usando sua própria lógica de repositório dentro de um contexto de fonte de dados de objeto, necessário para escrever seu próprio código personalizado e receber todos os parâmetros necessários. Agora, como os métodos de vinculação de dados podem retornar IQueryable e isso representa uma consulta que ainda deve ser executada, ASP.NET pode cuidar da modificação da consulta para adicionar os parâmetros de classificação e paginação adequados.
6. Pressione **F5** para iniciar a depuração do site e ir para a página produtos. Você verá que o GridView é populado com as categorias retornadas pelo método GetCategories.

    ![Populando um GridView usando a associação de modelo](whats-new-in-web-forms-in-aspnet-45/_static/image4.png "Populando um GridView usando a associação de modelo")

    *Populando um GridView usando a associação de modelo*
7. Pressione **SHIFT**+**F5** parar a depuração.

<a id="Task_3_-_Value_Providers_in_Model_Binding"></a>
#### <a name="task-3---value-providers-in-model-binding"></a>Provedores de tarefas de 3 valores na associação de modelo

A associação de modelo não apenas permite que você especifique métodos personalizados para trabalhar com seus dados diretamente no controle de vinculação de dados, mas também permite mapear dados da página para parâmetros desses métodos. No parâmetro do método, você pode usar atributos do provedor de valor para especificar a fonte de dados do valor. Por exemplo:

- Controles na página
- Valores de cadeia de caracteres de consulta
- Exibir dados
- Estado de sessão
- Cookies
- Dados de formulário postados
- Estado de exibição
- Também há suporte para provedores de valor personalizado

Se você usou o ASP.NET MVC 4, observará que o suporte à associação de modelo é semelhante. Na verdade, esses recursos foram tirados do ASP.NET MVC e movidos para o assembly **System. Web** para poder usá-los em Web Forms também.

Nesta tarefa, você atualizará o GridView para filtrar seus resultados pela quantidade de produtos para cada categoria, recebendo o parâmetro de filtro com associação de modelo.

1. Volte para a página **Products. aspx** .
2. Na parte superior do GridView, adicione um **rótulo** e uma **ComboBox** para selecionar o número de produtos para cada categoria, conforme mostrado abaixo.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample11.aspx)]
3. Adicione um **EmptyDataTemplate** ao GridView para mostrar uma mensagem quando não houver nenhuma categoria com o número selecionado de produtos.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample12.aspx)]
4. Abra o code-behind **Products.aspx.cs** e adicione a instrução using a seguir.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample13.cs)]
5. Modifique o método **GetCategories** para receber um argumento Integer **minProductsCount** e filtrar os resultados retornados. Para fazer isso, substitua o método pelo código a seguir.

    (Trecho de código- *Web Forms Lab-Ex01-GetCategories 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample14.cs)]

    O novo atributo **[Control]** no argumento **minProductsCount** permitirá que ASP.NET saiba que seu valor deve ser populado usando um controle na página. ASP.NET procurará qualquer controle correspondente ao nome do argumento (minProductsCount) e executará o mapeamento e a conversão necessários para preencher o parâmetro com o valor de controle.

    Como alternativa, o atributo fornece um construtor sobrecarregado que permite que você especifique o controle de onde obter o valor.

    > [!NOTE]
    > Uma meta dos recursos de vinculação de dados é reduzir a quantidade de código que precisa ser gravada para a interação da página. Além do provedor de valor [Control], você pode usar outros provedores de associação de modelo em seus parâmetros de método. Alguns deles estão listados na introdução à tarefa.
6. Pressione **F5** para iniciar a depuração do site e ir para a página produtos. Selecione um número de produtos na lista suspensa e observe como o GridView agora é atualizado.

    ![Filtrando o GridView com um valor de lista suspensa](whats-new-in-web-forms-in-aspnet-45/_static/image5.png "Filtrando o GridView com um valor de lista suspensa")

    *Filtrando o GridView com um valor de lista suspensa*
7. {2&gt;Pare a depuração.&lt;2}

<a id="Task_4_-_Using_Model_Binding_for_Filtering"></a>
#### <a name="task-4---using-model-binding-for-filtering"></a>Tarefa 4-usando a associação de modelo para filtragem

Nesta tarefa, você adicionará um segundo GridView filho para mostrar os produtos dentro da categoria selecionada.

1. Abra a página **Products. aspx** e atualize as categorias GridView para gerar automaticamente o botão Selecionar.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample15.aspx)]
2. Adicione um segundo **GridView** chamado **productsGrid** na parte inferior. Defina **ItemType** como **WebFormsLab. Model. Product**, **DataKeyNames** como **ProductID** e **SelectMethod** como **GetProducts**. Defina **AutoGenerateColumns** como **false** e adicione as colunas para ProductID, ProductName, Description e UnitPrice.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample16.aspx)]
3. Abra o arquivo code-behind **Products.aspx.cs** . Implemente o método **GetProducts** para receber a ID da categoria da categoria GridView e filtrar os produtos. A associação de modelo definirá o valor do parâmetro usando a linha selecionada no **categoriesGrid**. Como o nome do argumento e o nome do controle não correspondem, você deve especificar o nome do controle no provedor de valor de controle.

    (Trecho de código- *Web Forms Lab-Ex01-GetProducts*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample17.cs)]

    > [!NOTE]
    > Essa abordagem facilita o teste de unidade desses métodos. Em um contexto de teste de unidade, em que Web Forms não está em execução, o atributo [Control] não executará nenhuma ação específica.
4. Abra a página **Products. aspx** e localize os produtos GridView. Atualize o GridView de produtos para mostrar um link para editar o produto selecionado.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample18.aspx)]
5. Abra o code-behind da página **ProductDetails. aspx** e substitua o método **SelectProduct** pelo código a seguir.

    (Trecho de código – *laboratório de Web Forms – método Ex01-SelectProduct*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample19.cs)]

    > [!NOTE]
    > Observe que o atributo **[QueryString]** é usado para preencher o parâmetro do método a partir de um parâmetro ProductID na cadeia de caracteres de consulta.
6. Pressione **F5** para iniciar a depuração do site e ir para a página produtos. Selecione qualquer categoria no GridView das categorias e observe que o GridView dos produtos é atualizado.

    ![Mostrando produtos da categoria selecionada](whats-new-in-web-forms-in-aspnet-45/_static/image6.png "Mostrando produtos da categoria selecionada")

    *Mostrando produtos da categoria selecionada*
7. Clique no link **Exibir** em um produto para abrir a página ProductDetails. aspx.

    Observe que a página está recuperando o produto com SelectMethod usando o parâmetro productId da cadeia de caracteres de consulta.

    ![Exibindo os detalhes do produto](whats-new-in-web-forms-in-aspnet-45/_static/image7.png "Exibindo os detalhes do produto")

    *Exibindo os detalhes do produto*

    > [!NOTE]
    > A capacidade de digitar uma descrição HTML será implementada no próximo exercício.

<a id="Task_5_-_Using_Model_Binding_for_Update_Operations"></a>
#### <a name="task-5---using-model-binding-for-update-operations"></a>Tarefa 5-usando a associação de modelo para operações de atualização

Na tarefa anterior, você usou a associação de modelo principalmente para selecionar dados, nesta tarefa, você aprenderá a usar a associação de modelo em operações de atualização.

Você atualizará as categorias GridView para permitir que o usuário atualize as categorias.

1. Abra a página **Products. aspx** e atualize as categorias GridView para gerar automaticamente o botão Editar e use o novo atributo **UpdateMethod** para especificar um método **UpdateCategory** para atualizar o item selecionado.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample20.aspx)]

    O atributo DataKeyNames no GridView define quais são os membros que identificam exclusivamente o objeto associado a um modelo e, portanto, quais são os parâmetros que o método Update deve pelo menos receber.
2. Abra o arquivo code-behind **Products.aspx.cs** e implemente o método **UpdateCategory** . O método deve receber a ID da categoria para carregar a categoria atual, preencher os valores de GridView e, em seguida, atualizar a categoria.

    (Trecho de código- *Web Forms Lab-Ex01-UpdateCategory*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample21.cs)]

    O novo método **TryUpdateModel** na classe Page é responsável por preencher o objeto de modelo usando os valores dos controles na página. Nesse caso, ele substituirá os valores atualizados da linha GridView atual que está sendo editada no objeto **Category** .

    > [!NOTE]
    > O próximo exercício explicará o uso do ModelState. IsValid para validar os dados inseridos pelo usuário ao editar o objeto.
3. Execute o site e vá para a página produtos. Edite uma categoria. Digite um novo nome e clique em **Atualizar** para manter as alterações.

    ![Editando categorias](whats-new-in-web-forms-in-aspnet-45/_static/image8.png "Editando categorias")

    *Editando categorias*

<a id="Exercise2"></a>

<a id="Exercise_2_Data_Validation"></a>
### <a name="exercise-2-data-validation"></a>Exercício 2: validação de dados

Neste exercício, você aprenderá sobre os novos recursos de validação de dados no ASP.NET 4,5. Você configurará os novos recursos de validação não invasiva no Web Forms. Você usará as anotações de dados nas classes de modelo de aplicativo para a validação de entrada do usuário e, finalmente, aprenderá a ativar ou desativar a validação de solicitação para controles individuais em uma página.

<a id="Task_1_-_Unobtrusive_Validation"></a>
#### <a name="task-1---unobtrusive-validation"></a>Tarefa 1-validação não invasiva

Formulários com dados complexos, incluindo validadores, tendem a gerar muito código JavaScript na página, que pode representar cerca de 60% do código. Com a validação não invasiva habilitada, o código HTML parecerá mais limpo e mais organizada.

Nesta seção, você habilitará a validação discreta em ASP.NET para comparar o código HTML gerado por ambas as configurações.

1. Abra o **Visual Studio 2012** e abra a solução **inicial** localizada na pasta **Source\Ex2-Validation\Begin** deste laboratório. Como alternativa, você pode continuar trabalhando em sua solução existente do exercício anterior.

   1. Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar. Para fazer isso, na Gerenciador de Soluções, clique no projeto **WebFormsLab** **gerenciar pacotes NuGet**.
   2. Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, Compile a solução clicando em **build** | **Compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto. É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.
2. Pressione **F5** para iniciar o aplicativo Web. Vá para a página clientes e clique no link **Adicionar um novo cliente** .
3. Clique com o botão direito do mouse na página do navegador e selecione a opção **Exibir origem** para abrir o código HTML gerado pelo aplicativo.

    ![Mostrando o código HTML da página](whats-new-in-web-forms-in-aspnet-45/_static/image9.png "Mostrando o código HTML da página")

    *Mostrando o código HTML da página*
4. Percorra o código-fonte da página e observe que o ASP.NET inseriu o código JavaScript e os validadores de dados na página para executar as validações e mostrar a lista de erros.

    ![Código JavaScript de validação na página CustomerDetails](whats-new-in-web-forms-in-aspnet-45/_static/image10.png "Código JavaScript de validação na página CustomerDetails")

    *Código JavaScript de validação na página CustomerDetails*
5. Feche o navegador e volte para o Visual Studio.
6. Agora, você habilitará a validação não invasiva. Abra o **Web. config** e localize a chave **ValidationSettings: UnobtrusiveValidationMode** na seção **appSettings** **.** Defina o valor da chave como **WebForms**.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample22.xml)]

    > [!NOTE]
    > Você também pode definir essa propriedade na página &quot; **\_carregar**&quot; evento caso queira habilitar a validação discreta apenas para algumas páginas.
7. Abra **CustomerDetails. aspx** e pressione **F5** para iniciar o aplicativo Web.
8. Pressione a tecla F12 para abrir as ferramentas de desenvolvedor do IE. Depois que as ferramentas de desenvolvedor estiverem abertas, selecione a guia script. Selecione **CustomerDetails. aspx** no menu e observe que os scripts necessários para executar o jQuery na página foram carregados no navegador do site local.

    ![Carregando os arquivos do jQuery JavaScript diretamente do servidor IIS local](whats-new-in-web-forms-in-aspnet-45/_static/image11.png "Carregando os arquivos do jQuery JavaScript diretamente do servidor IIS local")

    *Carregando os arquivos do jQuery JavaScript diretamente do servidor IIS local*
9. Feche o navegador para retornar ao Visual Studio. Abra o arquivo **site. Master** novamente e localize o **ScriptManager**. Adicione a propriedade **EnableCdn** do atributo com o valor **true**. Isso forçará o jQuery a ser carregado da URL online, não da URL do site local.
10. Abra **CustomerDetails. aspx** no Visual Studio. Pressione a tecla F5 para executar o site. Quando o Internet Explorer for aberto, pressione a tecla F12 para abrir as ferramentas de desenvolvedor. Selecione a guia **script** e dê uma olhada na lista suspensa. Observe que os arquivos JavaScript jQuery não estão mais sendo carregados do site local, mas sim da CDN online do jQuery.

    ![Carregando os arquivos jQuery JavaScript da CDN](whats-new-in-web-forms-in-aspnet-45/_static/image12.png "Carregando os arquivos jQuery JavaScript da CDN")

    *Carregando os arquivos jQuery JavaScript da CDN*
11. Abra o código-fonte da página HTML novamente usando a opção Exibir fonte no navegador. Observe que, ao habilitar a validação discreta, o ASP.NET substituiu o código JavaScript injetado por atributos de \*de dados.

    ![Código de validação não invasivo](whats-new-in-web-forms-in-aspnet-45/_static/image13.png "Código de validação não invasivo")

    *Código de validação não invasivo*

    > [!NOTE]
    > Neste exemplo, você viu como um resumo de validação com anotações de dados foi simplificado para apenas algumas linhas HTML e JavaScript. Anteriormente, sem validação discreta, quanto mais controles de validação você adicionar, maior será o seu código de validação do JavaScript.

<a id="Task_2_-_Validating_the_Model_with_Data_Annotations"></a>
#### <a name="task-2---validating-the-model-with-data-annotations"></a>Tarefa 2-Validando o modelo com anotações de dados

O ASP.NET 4,5 apresenta a validação de anotações de dados para Web Forms. Em vez de ter um controle de validação em cada entrada, agora você pode definir restrições em suas classes de modelo e usá-las em todos os seus aplicativos Web. Nesta seção, você aprenderá a usar as anotações de dados para validar um formulário novo/editar cliente.

1. Abra a página **CustomerDetail. aspx** . Observe que o nome do cliente e o segundo nome nas seções **EditItemTemplate** e **InsertItemTemplate** são validados usando controles RequiredFieldValidator. Cada validador é associado a uma condição específica, portanto, você precisa incluir tantos validadores como condições para verificar.
2. Adicione anotações de dados para validar a classe de modelo do cliente. Abra a classe **Customer.cs** na pasta **modelo** e *decorar* cada propriedade usando atributos de anotação de dados.

    (Trecho de código- *Web Forms Lab-Ex02-anotações de dados*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample23.cs)]

    > [!NOTE]
    > .NET Framework 4,5 ampliou a coleção de anotação de dados existente. Estas são algumas das anotações de dados que você pode usar: [CreditCard], [telefone], [EmailAddress], [intervalo], [comparar], [url], [FileExtensions], [Required], [Chave], [RegularExpression].
    > 
    > Alguns exemplos de uso:
    > 
    > [Chave]: Specifies that an attribute is the unique identifier
    > 
    > [Range(0.4, 0.5, ErrorMessage=&quot;{Write an error message}&quot;]: Double range
    > 
    > [EmailAddress(ErrorMessage=&quot;Invalid Email&quot;), MaxLength(56)]: Two annotations in the same line.
    > 
    > Você também pode definir suas próprias mensagens de erro dentro de cada atributo.
3. Abra **CustomerDetails. aspx** e remova todos os requiredfieldvalidators dos campos First e Last Name nas seções em EditItemTemplate e InsertItemTemplate do controle FormView.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample24.aspx)]

    > [!NOTE]
    > Uma vantagem de usar as anotações de dados é que a lógica de validação não é duplicada nas páginas do aplicativo. Defina-a uma vez no modelo e use-a em todas as páginas de aplicativo que manipulam dados.
4. Abra o code-behind do **CustomerDetails. aspx** e localize o método saveCustomer. Esse método é chamado ao inserir um novo cliente e recebe o parâmetro Customer dos valores de controle FormView. Quando o mapeamento entre os controles de página e o objeto de parâmetro ocorre, o ASP.NET executará a validação do modelo em todos os atributos de anotação de dados e preencherá o dicionário ModelState com os erros encontrados, se houver.

    O ModelState. IsValid retornará true somente se todos os campos em seu modelo forem válidos após a execução da validação.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample25.cs)]
5. Adicione um controle **ValidationSummary** ao final da página CustomerDetails para mostrar a lista de erros de modelo.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample26.aspx)]

    O **ShowModelStateErrors** é uma nova propriedade no controle ValidationSummary que, quando definida como **true**, o controle mostrará os erros do dicionário ModelState. Esses erros são provenientes da validação de anotações de dados.
6. Pressione **F5** para executar o aplicativo Web. Preencha o formulário com alguns valores errados e clique em **salvar** para executar a validação. Observe o resumo de erros na parte inferior.

    ![Validação com anotações de dados](whats-new-in-web-forms-in-aspnet-45/_static/image14.png "Validação com anotações de dados")

    *Validação com anotações de dados*

<a id="Task_3_-_Handling_Custom_Database_Errors_with_ModelState"></a>
#### <a name="task-3---handling-custom-database-errors-with-modelstate"></a>Tarefa 3-lidando com erros de banco de dados personalizados com ModelState

Na versão anterior do Web Forms, manipular erros de banco de dados como uma cadeia de caracteres muito longa ou uma violação de chave exclusiva poderia envolver a geração de exceções no código do repositório e, em seguida, manipular as exceções em seu code-behind para exibir um erro. Uma grande quantidade de código é necessária para fazer algo relativamente simples.

No Web Forms 4,5, o objeto ModelState pode ser usado para exibir os erros na página, seja de seu modelo ou do banco de dados, de maneira consistente.

Nesta tarefa, você adicionará o código para lidar corretamente com exceções de banco de dados e mostrará a mensagem apropriada para o usuário usando o objeto ModelState.

1. Enquanto o aplicativo ainda estiver em execução, tente atualizar o nome de uma categoria usando um valor duplicado.

    ![Atualizando uma categoria com um nome duplicado](whats-new-in-web-forms-in-aspnet-45/_static/image15.png "Atualizando uma categoria com um nome duplicado")

    *Atualizando uma categoria com um nome duplicado*

    Observe que uma exceção é gerada devido à &quot;única restrição&quot; da coluna **CategoryName** .

    ![Exceção para nomes de categoria duplicados](whats-new-in-web-forms-in-aspnet-45/_static/image16.png "Exceção para nomes de categoria duplicados")

    *Exceção para nomes de categoria duplicados*
2. {2&gt;Pare a depuração.&lt;2} No arquivo code-behind **Products.aspx.cs** , atualize o método **UpdateCategory** para manipular as exceções lançadas pelo banco de forma. A chamada do método SaveChanges () e adiciona um erro ao objeto **ModelState** .

    O novo método **TryUpdateModel** atualiza o objeto Category recuperado a partir do banco de dados usando o formulário dado fornecido pelo usuário.

    (Trecho de código- *Web Forms Lab-Ex02-UpdateCategory manipulam erros*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample27.cs)]

    > [!NOTE]
    > O ideal é que você precise identificar a causa do DbUpdateException e verificar se a causa raiz é a violação de uma restrição de chave exclusiva.
3. Abra **Products. aspx** e adicione um controle **ValidationSummary** abaixo do GridView de categorias para mostrar a lista de erros de modelo.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample28.aspx)]
4. Execute o site e vá para a página produtos. Tente atualizar o nome de uma categoria usando um valor duplicado.

    Observe que a exceção foi tratada e a mensagem de erro aparece no controle **ValidationSummary** .

    ![Erro de categoria duplicado](whats-new-in-web-forms-in-aspnet-45/_static/image17.png "Erro de categoria duplicado")

    *Erro de categoria duplicado*

<a id="Task_4_-_Request_Validation_in_ASPNET_Web_Forms_45"></a>
#### <a name="task-4---request-validation-in-aspnet-web-forms-45"></a>Tarefa 4-validação de solicitação no ASP.NET Web Forms 4,5

O recurso de validação de solicitação no ASP.NET fornece um certo nível de proteção padrão contra ataques XSS (script entre sites). Nas versões anteriores do ASP.NET, a validação de solicitação foi habilitada por padrão e só poderia ser desabilitada para uma página inteira. Com a nova versão do ASP.NET Web Forms agora você pode desabilitar a validação de solicitação para um único controle, executar a validação de solicitação lenta ou acessar dados de solicitação não validados (tenha cuidado se você fizer isso!).

1. Pressione **Ctrl + F5** para iniciar o site sem depuração e vá para a página produtos. Selecione uma categoria e, em seguida, clique no link **Editar** em qualquer um dos produtos.
2. Digite uma descrição que contenha conteúdo potencialmente perigoso, por exemplo, incluindo marcas HTML. Tome nota da exceção gerada devido à validação da solicitação.

    ![Editando um produto com conteúdo potencialmente perigoso](whats-new-in-web-forms-in-aspnet-45/_static/image18.png "Editando um produto com conteúdo potencialmente perigoso")

    *Editando um produto com conteúdo potencialmente perigoso*

    ![Exceção gerada devido à validação da solicitação](whats-new-in-web-forms-in-aspnet-45/_static/image19.png "Exceção gerada devido à validação da solicitação")

    *Exceção gerada devido à validação da solicitação*
3. Feche a página e, no Visual Studio, pressione **Shift + F5** para parar a depuração.
4. Abra a página **ProductDetails. aspx** e localize a caixa de texto **Descrição** .
5. Adicione a nova propriedade **ValidateRequestMode** à caixa de texto e defina seu valor como **Disabled**.

    O novo atributo **ValidateRequestMode** permite que você desabilite a validação de solicitação de forma granular em cada controle. Isso é útil quando você deseja usar uma entrada que pode receber código HTML, mas deseja manter a validação funcionando para o restante da página.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample29.aspx)]
6. Pressione **F5** para executar o aplicativo Web. Abra a página Editar produto novamente e conclua uma descrição do produto, incluindo marcas HTML. Observe que agora você pode adicionar conteúdo HTML à descrição.

    ![Validação de solicitação desabilitada para a descrição do produto](whats-new-in-web-forms-in-aspnet-45/_static/image20.png "Validação de solicitação desabilitada para a descrição do produto")

    *Validação de solicitação desabilitada para a descrição do produto*

    > [!NOTE]
    > Em um aplicativo de produção, você deve limpar o código HTML inserido pelo usuário para garantir que apenas marcas HTML seguras sejam inseridas (por exemplo, não há &lt;script&gt; marcas). Para fazer isso, você pode usar a [biblioteca de proteção da Web da Microsoft](https://www.nuget.org/packages/AntiXSS).
7. Edite o produto novamente. Digite código HTML no campo nome e clique em **salvar**. Observe que a validação de solicitação só é desabilitada para o campo Descrição e o restante dos campos ainda é validado em relação ao conteúdo potencialmente perigoso.

    ![Validação de solicitação habilitada no restante dos campos](whats-new-in-web-forms-in-aspnet-45/_static/image21.png "Validação de solicitação habilitada no restante dos campos")

    *Validação de solicitação habilitada no restante dos campos*

    O ASP.NET Web Forms 4,5 inclui um novo modo de validação de solicitação para executar a validação de solicitação lentamente. Com o modo de validação de solicitação definido como **4,5**, se uma parte do código acessar *solicitação. Form [&quot;Key&quot;]* , a validação de solicitação do ASP.NET 4.5 só disparará a validação da solicitação para esse elemento específico na coleção de formulários.

    Além disso, o ASP.NET 4,5 agora inclui rotinas de codificação de núcleo da biblioteca Microsoft anti-XSS v 4.0. As rotinas de codificação anti-XSS são implementadas pelo novo tipo *AntiXssEncoder* encontrado no novo namespace **System. Web. Security. AntiXss** . Com o parâmetro **EncoderType** configurado para usar *AntiXssEncoder*, toda a codificação de saída dentro do ASP.NET usa automaticamente as novas rotinas de codificação.
8. A validação de solicitação do ASP.NET 4,5 também dá suporte ao acesso não validado para solicitar dados. ASP.NET 4,5 adiciona uma nova propriedade de coleção ao objeto **HttpRequest** chamado **Unvalidated**. Quando você navega para **HttpRequest. Unvalidated** , você tem acesso a todas as partes comuns de dados de solicitação, incluindo formulários, querystrings, cookies, URLs e assim por diante.

    ![Solicitação. objeto não validado](whats-new-in-web-forms-in-aspnet-45/_static/image22.png "Solicitação. objeto não validado")

    *Solicitação. objeto não validado*

    > [!NOTE]
    > **Use a Propriedade HttpRequest. Unvalidated com cuidado!** Certifique-se de executar cuidadosamente a validação personalizada nos dados de solicitação brutos para garantir que o texto perigoso não seja arredondado e processado de volta para clientes desconhecidos!

<a id="Exercise3"></a>

<a id="Exercise_3_Asynchronous_Page_Processing_in_ASPNET_Web_Forms"></a>
### <a name="exercise-3-asynchronous-page-processing-in-aspnet-web-forms"></a>Exercício 3: processamento de página assíncrona no ASP.NET Web Forms

Neste exercício, você será apresentado aos novos recursos de processamento de página assíncrona no ASP.NET Web Forms.

<a id="Task_1_-_Updating_the_Product_Details_Page_to_Upload_and_Show_Images"></a>
#### <a name="task-1---updating-the-product-details-page-to-upload-and-show-images"></a>Tarefa 1-Atualizando a página de detalhes do produto para carregar e mostrar imagens

Nesta tarefa, você atualizará a página de detalhes do produto para permitir que o usuário especifique uma URL de imagem para o produto e a exiba no modo de exibição somente leitura. Você criará uma cópia local da imagem especificada baixando-a de forma síncrona. Na próxima tarefa, você atualizará essa implementação para fazê-la funcionar de forma assíncrona.

1. Abra o **Visual Studio 2012** e carregue a solução **inicial** localizada em **Source\Ex3-Async\Begin** da pasta deste laboratório. Como alternativa, você pode continuar trabalhando em sua solução existente dos exercícios anteriores.

   1. Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar. Para fazer isso, na Gerenciador de Soluções, clique no projeto **WebFormsLab** e selecione **gerenciar pacotes NuGet**.
   2. Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, Compile a solução clicando em **build** | **Compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto. É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.
2. Abra a fonte da página **ProductDetails. aspx** e adicione um campo no ItemTemplate do FormView para mostrar a imagem do produto.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample30.aspx)]
3. Adicione um campo para especificar a URL da imagem no Edittemplate do FormView.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample31.aspx)]
4. Abra o arquivo code-behind **ProductDetails.aspx.cs** e adicione as seguintes diretivas de namespace.

    (Trecho de código- *Web Forms Lab-Ex03-namespaces*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample32.cs)]
5. Crie um método **UpdateProductImage** para armazenar imagens remotas na pasta **imagens** locais e atualize a entidade produto com o novo valor de local da imagem.

    (Trecho de código- *Web Forms Lab-Ex03-UpdateProductImage*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample33.cs)]
6. Atualize o método **UpdateProduct** para chamar o método **UpdateProductImage** .

    (Trecho de código- *Web Forms Lab – chamada de Ex03-UpdateProductImage*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample34.cs)]
7. Execute o aplicativo e tente carregar uma imagem para um produto. Por exemplo, você pode usar a seguinte URL de imagem do clip-arts do Office: [ [http://officeimg.vo.msecnd.net/images/MB900437099.jpg](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)

    ![Configurando uma imagem para um produto](whats-new-in-web-forms-in-aspnet-45/_static/image23.png "Configurando uma imagem para um produto")

    *Configurando uma imagem para um produto*

<a id="Task_2_-_Adding_Asynchronous_Processing_to_the_Product_Details_Page"></a>
#### <a name="task-2---adding-asynchronous-processing-to-the-product-details-page"></a>Tarefa 2-Adicionando processamento assíncrono à página de detalhes do produto

Nesta tarefa, você atualizará a página de detalhes do produto para fazê-lo funcionar de forma assíncrona. Você aprimorará uma tarefa de execução longa-o processo de download da imagem-usando o processamento de página assíncrona do ASP.NET 4,5.

Métodos assíncronos em aplicativos Web podem ser usados para otimizar a maneira como os pools de threads ASP.NET são usados. No ASP.NET, há um número limitado de threads no pool de threads para participar de solicitações, assim, quando todos os threads estão ocupados, o ASP.NET começa a rejeitar novas solicitações, envia mensagens de erro de aplicativo e torna seu site indisponível.

As operações demoradas em seu site são excelentes candidatos à programação assíncrona porque ocupam o thread atribuído por um longo tempo. Isso inclui solicitações de longa execução, páginas com muitos elementos e páginas diferentes que exigem operações offline, consultando um banco de dados ou acessando um servidor Web externo. A vantagem é que, se você usar métodos assíncronos para essas operações, enquanto a página estiver sendo processada, o thread será liberado e retornado ao pool de threads e poderá ser usado para participar de uma nova solicitação de página. Isso significa que a página iniciará o processamento em um thread a partir do pool de threads e poderá concluir o processamento em um diferente, depois que o processamento assíncrono for concluído.

1. Abra a página **ProductDetails. aspx** . Adicione o atributo **Async** no elemento **Page** e defina-o como **true**. Esse atributo informa ao ASP.NET para implementar a interface IHttpAsyncHandler.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample35.aspx)]
2. Adicione um rótulo na parte inferior da página para mostrar os detalhes dos threads que executam a página.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample36.aspx)]
3. Abra o **ProductDetails.aspx.cs** e adicione as seguintes diretivas de namespace.

    (Trecho de código- *Web Forms Lab-Ex03-namespaces 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample37.cs)]
4. Modifique o método **UpdateProductImage** para baixar a imagem com uma tarefa assíncrona. Você substituirá o método **DownloadFile** **WebClient** pelo método **DownloadFileTaskAsync** e incluirá a palavra-chave **Await** .

    (Trecho de código- *Web Forms Lab-Ex03-UpdateProductImage Async*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample38.cs)]

    O RegisterAsyncTask registra uma nova tarefa assíncrona de página a ser executada em um thread diferente. Ele recebe uma expressão lambda com a tarefa (t) a ser executada. A palavra-chave **Await** no método **DownloadFileTaskAsync** converte o restante do método em um retorno de chamada que é invocado de forma assíncrona após a conclusão do método **DownloadFileTaskAsync** . ASP.NET continuará a execução do método mantendo automaticamente todos os valores originais da solicitação HTTP. O novo modelo de programação assíncrona no .NET 4,5 permite que você escreva código assíncrono que parece muito com código síncrono e permite que o compilador Manipule as complicações das funções de retorno de chamada ou do código de continuação.
    > [!NOTE]
    > RegisterAsyncTask e PageAsyncTask já estavam disponíveis desde o .NET 2,0. A palavra-chave Await é nova do modelo de programação assíncrona do .NET 4,5 e pode ser usada junto com os novos métodos TaskAsync do objeto WebClient do .NET.
5. Adicione o código para exibir os threads nos quais o código iniciou e concluiu a execução. Para fazer isso, atualize o método **UpdateProductImage** com o código a seguir.

    (Trecho de código- *Web Forms Lab-Ex03-show threads*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample39.cs)]
6. Abra o arquivo **Web. config** do site da Web. Adicione a seguinte variável appSetting.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample40.xml)]
7. Pressione **F5** para executar o aplicativo e carregar uma imagem para o produto. Observe que a ID de threads em que o código foi iniciado e concluído pode ser diferente. Isso ocorre porque as tarefas assíncronas são executadas em um thread separado do pool de threads do ASP.NET. Quando a tarefa é concluída, o ASP.NET coloca a tarefa de volta na fila e atribui qualquer um dos threads disponíveis.

    ![Baixando uma imagem de forma assíncrona](whats-new-in-web-forms-in-aspnet-45/_static/image24.png "Baixando uma imagem de forma assíncrona")

    *Baixando uma imagem de forma assíncrona*

> [!NOTE]
> Além disso, você pode implantar este aplicativo no Azure após o [Apêndice B: publicando um aplicativo ASP.NET MVC 4 usando implantação da Web](#AppendixB).

---

<a id="Summary"></a>
## <a name="summary"></a>Resumo

Neste laboratório prático, os conceitos a seguir foram abordados e demonstrados:

- Usar expressões de vinculação de dados fortemente tipadas
- Usar novos recursos de associação de modelo no Web Forms
- Usar provedores de valor para mapear dados de página para métodos code-behind
- Usar anotações de dados para validação de entrada do usuário
- Aproveite a validação do lado do cliente não invasiva com o jQuery no Web Forms
- Implementar a validação de solicitação granular
- Implementar o processamento de página assíncrona no Web Forms

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apêndice A: Instalando o Visual Studio Express 2012 para Web

Você pode instalar o **Microsoft Visual Studio Express 2012 para Web** ou outra versão &quot;Express&quot; usando o **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** . As instruções a seguir orientam você pelas etapas necessárias para instalar o *Visual Studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.

1. Vá para [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Como alternativa, se você já tiver instalado o Web Platform Installer, poderá abri-lo e pesquisar o produto &quot;<em>Visual Studio Express 2012 para Web com o SDK do Azure</em>&quot;.
2. Clique em **instalar agora**. Se você não tiver **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.
3. Quando **Web Platform Installer** estiver aberto, clique em **instalar** para iniciar a instalação.

    ![Instalar Visual Studio Express](whats-new-in-web-forms-in-aspnet-45/_static/image25.png "Instalar Visual Studio Express")

    *Instalar Visual Studio Express*
4. Leia todos os termos e licenças de produtos e clique em **aceito** para continuar.

    ![Aceitando os termos de licença](whats-new-in-web-forms-in-aspnet-45/_static/image26.png)

    *Aceitando os termos de licença*
5. Aguarde até que o processo de download e instalação seja concluído.

    ![Progresso da instalação](whats-new-in-web-forms-in-aspnet-45/_static/image27.png)

    *Progresso da instalação*
6. Quando a instalação for concluída, clique em **concluir**.

    ![Instalação concluída](whats-new-in-web-forms-in-aspnet-45/_static/image28.png)

    *Instalação concluída*
7. Clique em **sair** para fechar Web Platform Installer.
8. Para abrir o Visual Studio Express para Web, vá para a tela **Iniciar** e comece a escrever &quot;**vs Express**&quot;e, em seguida, clique no bloco **vs Express para Web** .

    ![Bloco VS Express para Web](whats-new-in-web-forms-in-aspnet-45/_static/image29.png)

    *Bloco VS Express para Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Apêndice B: publicando um aplicativo ASP.NET MVC 4 usando Implantação da Web

Este apêndice mostrará como criar um novo site no portal do Azure e publicar o aplicativo obtido seguindo o laboratório, aproveitando o recurso de publicação de Implantação da Web fornecido pelo Azure.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>Tarefa 1-Criando um novo site no portal do Azure

1. Vá para o [portal de gerenciamento do Azure](https://manage.windowsazure.com/) e entre usando as credenciais da Microsoft associadas à sua assinatura.

    > [!NOTE]
    > Com o Azure, você pode hospedar 10 sites da Web de ASP.NET gratuitamente e, em seguida, dimensionar conforme o tráfego cresce. Você pode se inscrever [aqui](https://aka.ms/aspnet-hol-azure).

    ![Fazer logon no Windows portal do Azure](whats-new-in-web-forms-in-aspnet-45/_static/image30.png "Fazer logon no Windows portal do Azure")

    *Faça logon no Portal*
2. Clique em **novo** na barra de comandos.

    ![Criando um novo site](whats-new-in-web-forms-in-aspnet-45/_static/image31.png "Criando um novo site")

    *Criando um novo site*
3. Clique em **computação** | **site**. Em seguida, selecione a opção **criação rápida** . Forneça uma URL disponível para o novo site e clique em **criar site**.

    > [!NOTE]
    > O Azure é o host para um aplicativo Web em execução na nuvem que você pode controlar e gerenciar. A opção criação rápida permite que você implante um aplicativo Web completo no Azure de fora do Portal. Ele não inclui etapas para configurar um banco de dados.

    ![Criando um novo site usando a criação rápida](whats-new-in-web-forms-in-aspnet-45/_static/image32.png "Criando um novo site usando a criação rápida")

    *Criando um novo site usando a criação rápida*
4. Aguarde até que o novo **site** seja criado.
5. Depois que o site for criado, clique no link sob a coluna **URL** . Verifique se o novo site está funcionando.

    ![Navegando até o novo site](whats-new-in-web-forms-in-aspnet-45/_static/image33.png "Navegando até o novo site")

    *Navegando até o novo site*

    ![Site em execução](whats-new-in-web-forms-in-aspnet-45/_static/image34.png "Site em execução")

    *Site em execução*
6. Volte para o portal e clique no nome do site na coluna **nome** para exibir as páginas de gerenciamento.

    ![Abrindo as páginas de gerenciamento de site](whats-new-in-web-forms-in-aspnet-45/_static/image35.png "Abrindo as páginas de gerenciamento de site")

    *Abrindo as páginas de gerenciamento de site*
7. Na página **painel** , na seção **visão rápida** , clique no link **baixar perfil de publicação** .

    > [!NOTE]
    > O *perfil de publicação* contém todas as informações necessárias para publicar um aplicativo Web no Azure para cada método de publicação habilitado. O perfil de publicação contém as URLs, as credenciais de usuário e as cadeias de conexão de banco de dados necessárias para conectar-se e autenticar cada um dos pontos de extremidade para os quais um método de publicação é habilitado. **O Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** e **Microsoft Visual Studio 2012** dão suporte à leitura de perfis de publicação para automatizar a configuração desses programas para a publicação de aplicativos Web no Azure.

    ![Baixando o perfil de publicação do site](whats-new-in-web-forms-in-aspnet-45/_static/image36.png "Baixando o perfil de publicação do site")

    *Baixando o perfil de publicação do site*
8. Baixe o arquivo de perfil de publicação em um local conhecido. Neste exercício, você verá como usar esse arquivo para publicar um aplicativo Web no Azure por meio do Visual Studio.

    ![Salvando o arquivo de perfil de publicação](whats-new-in-web-forms-in-aspnet-45/_static/image37.png "Salvando o perfil de publicação")

    *Salvando o arquivo de perfil de publicação*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tarefa 2-Configurando o servidor de banco de dados

Se seu aplicativo utiliza bancos de dados SQL Server, você precisará criar um servidor de banco de dados SQL. Se você quiser implantar um aplicativo simples que não usa SQL Server você pode ignorar essa tarefa.

1. Você precisará de um servidor do banco de dados SQL para armazenar o banco de dados do aplicativo. Você pode exibir os servidores do banco de dados SQL de sua assinatura no portal de gerenciamento do Azure em bancos de dados **sql** | **servidores** | **painel do servidor**. Se você não tiver um servidor criado, poderá criar um usando o botão **Adicionar** na barra de comandos. Anote o nome do **servidor e a URL, o nome de logon e a senha do administrador**, pois você irá usá-los nas próximas tarefas. Não crie o banco de dados ainda, pois ele será criado em um estágio posterior.

    ![Painel do servidor do banco de dados SQL](whats-new-in-web-forms-in-aspnet-45/_static/image38.png "Painel do servidor do banco de dados SQL")

    *Painel do servidor do banco de dados SQL*
2. Na próxima tarefa, você testará a conexão de banco de dados do Visual Studio, por esse motivo, será necessário incluir o endereço IP local na lista de **endereços IP permitidos**do servidor. Para fazer isso, clique em **Configurar**, selecione o endereço IP do **endereço IP do cliente atual** e cole-o nas caixas de texto endereço IP **inicial** e **endereço IP final** e clique no botão ![adicionar-cliente-IP-endereço-OK-botão](whats-new-in-web-forms-in-aspnet-45/_static/image39.png).

    ![Adicionando endereço IP do cliente](whats-new-in-web-forms-in-aspnet-45/_static/image40.png)

    *Adicionando endereço IP do cliente*
3. Depois que o **endereço IP do cliente** for adicionado à lista endereços IP permitidos, clique em **salvar** para confirmar as alterações.

    ![Confirmar alterações](whats-new-in-web-forms-in-aspnet-45/_static/image41.png)

    *Confirmar alterações*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tarefa 3-publicando um aplicativo ASP.NET MVC 4 usando Implantação da Web

1. Volte para a solução ASP.NET MVC 4. Na **Gerenciador de soluções**, clique com o botão direito do mouse no projeto de site e selecione **publicar**.

    ![Publicando o aplicativo](whats-new-in-web-forms-in-aspnet-45/_static/image42.png "Publicando o aplicativo")

    *Publicando o site*
2. Importe o perfil de publicação salvo na primeira tarefa.

    ![Importando o perfil de publicação](whats-new-in-web-forms-in-aspnet-45/_static/image43.png "Importando o perfil de publicação")

    *Importando perfil de publicação*
3. Clique em **validar conexão**. Depois que a validação for concluída, clique em **Avançar**.

    > [!NOTE]
    > A validação será concluída quando você vir uma marca de seleção verde ao lado do botão Validar conexão.

    ![Validando conexão](whats-new-in-web-forms-in-aspnet-45/_static/image44.png "Validando conexão")

    *Validando conexão*
4. Na página **configurações** , na seção **bancos** de dados, clique no botão ao lado da caixa de texto da sua conexão de banco (ou seja, **DefaultConnection**).

    ![Configuração de implantação da Web](whats-new-in-web-forms-in-aspnet-45/_static/image45.png "Configuração de implantação da Web")

    *Configuração de implantação da Web*
5. Configure a conexão de banco de dados da seguinte maneira:

   - No **nome do servidor** , digite a URL do servidor do banco de dados SQL usando o prefixo *TCP:* .
   - Em **nome de usuário** , digite o nome de logon do administrador do servidor.
   - Em **senha** , digite a senha de logon do administrador do servidor.
   - Digite um novo nome de banco de dados.

     ![Configurando a cadeia de conexão de destino](whats-new-in-web-forms-in-aspnet-45/_static/image46.png "Configurando a cadeia de conexão de destino")

     *Configurando a cadeia de conexão de destino*
6. Em seguida, clique em **OK**. Quando for solicitado a criar o banco de dados, clique em **Sim**.

    ![Criando o banco de dados](whats-new-in-web-forms-in-aspnet-45/_static/image47.png "Criando a cadeia de caracteres do banco de dados")

    *Criando o banco de dados*
7. A cadeia de conexão que você usará para se conectar ao banco de dados SQL no Azure é mostrada na caixa de texto conexão padrão. Em seguida, clique em **Próximo**.

    ![Cadeia de conexão apontando para o banco de dados SQL](whats-new-in-web-forms-in-aspnet-45/_static/image48.png "Cadeia de conexão apontando para o banco de dados SQL")

    *Cadeia de conexão apontando para o banco de dados SQL*
8. Na página **Visualização** , clique em **publicar**.

    ![Publicando o aplicativo Web](whats-new-in-web-forms-in-aspnet-45/_static/image49.png "Publicando o aplicativo Web")

    *Publicando o aplicativo Web*
9. Quando o processo de publicação for concluído, o navegador padrão abrirá o site publicado.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Apêndice C: usando trechos de código

Com trechos de código, você tem todo o código de que precisa ao seu alcance. O documento de laboratório informará exatamente quando você pode usá-los, conforme mostrado na figura a seguir.

![Usando trechos de código do Visual Studio para inserir código em seu projeto](whats-new-in-web-forms-in-aspnet-45/_static/image50.png "Usando trechos de código do Visual Studio para inserir código em seu projeto")

*Usando trechos de código do Visual Studio para inserir código em seu projeto*

***Para adicionar um trecho de código usando o tecladoC# (somente)***

1. Coloque o cursor onde você deseja inserir o código.
2. Comece digitando o nome do trecho de código (sem espaços ou hifens).
3. Observe como o IntelliSense exibe nomes de trechos de código correspondentes.
4. Selecione o trecho correto (ou continue digitando até que o nome do trecho inteiro seja selecionado).
5. Pressione a tecla TAB duas vezes para inserir o trecho de código no local do cursor.

![Comece a digitar o nome do trecho](whats-new-in-web-forms-in-aspnet-45/_static/image51.png "Comece a digitar o nome do trecho")

*Comece a digitar o nome do trecho*

![Pressione Tab para selecionar o trecho realçado](whats-new-in-web-forms-in-aspnet-45/_static/image52.png "Pressione Tab para selecionar o trecho realçado")

*Pressione Tab para selecionar o trecho realçado*

![Pressione Tab novamente e o trecho será expandido](whats-new-in-web-forms-in-aspnet-45/_static/image53.png "Pressione Tab novamente e o trecho será expandido")

*Pressione Tab novamente e o trecho será expandido*

***Para adicionar um trecho de código usando o mouseC#(, Visual Basic e XML)*** uma. Clique com o botão direito do mouse no local em que você deseja inserir o trecho de código.

1. Selecione **Inserir trecho** seguido por **meus trechos de código**.
2. Selecione o trecho relevante na lista clicando nele.

![Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho](whats-new-in-web-forms-in-aspnet-45/_static/image54.png "Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho")

*Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho*

![Selecione o trecho relevante na lista clicando nele](whats-new-in-web-forms-in-aspnet-45/_static/image55.png "Selecione o trecho relevante na lista clicando nele")

*Selecione o trecho relevante na lista clicando nele*
