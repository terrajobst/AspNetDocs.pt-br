---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
title: O que há de novo no Entity Framework 4,0 | Microsoft Docs
author: tdykstra
description: Esta série de tutoriais se baseia no aplicativo Web da Contoso University criado pelo Introdução com a série de tutoriais do Entity Framework 4,0. I...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 393df4a8-b1db-44c4-9db7-2b533ca887d0
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
msc.type: authoredcontent
ms.openlocfilehash: 29d2bd61e8361b130e7b9415dad4fe1d5b847945
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78642670"
---
# <a name="whats-new-in-the-entity-framework-40"></a>Novidades no Entity Framework 4.0

por [Tom Dykstra](https://github.com/tdykstra)

> Esta série de tutoriais se baseia no aplicativo Web da Contoso University criado pelo [introdução com a série de tutoriais Entity Framework](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) . Se você não concluiu os tutoriais anteriores, como ponto de partida para este tutorial, você pode [baixar o aplicativo](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que você criou. Você também pode [baixar o aplicativo](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) que é criado pela série de tutoriais completa. Se você tiver dúvidas sobre os tutoriais, poderá lançá-los no [Fórum de Entity Framework ASP.net](https://forums.asp.net/1227.aspx).

No tutorial anterior, você viu alguns métodos para maximizar o desempenho de um aplicativo Web que usa o Entity Framework. Este tutorial examina alguns dos novos recursos mais importantes na versão 4 do Entity Framework, além de links para recursos que fornecem uma introdução mais completa a todos os novos recursos. Os recursos realçados neste tutorial incluem o seguinte:

- Associações de chave estrangeira.
- Executando comandos SQL definidos pelo usuário.
- Modelo-primeiro desenvolvimento.
- Suporte a POCO.

Além disso, o tutorial apresentará brevemente o *desenvolvimento do primeiro código*, um recurso que será lançado na próxima versão do Entity Framework.

Para iniciar o tutorial, inicie o Visual Studio e abra o aplicativo Web da Contoso University com o qual você estava trabalhando no tutorial anterior.

## <a name="foreign-key-associations"></a>Associações de chave estrangeira

A versão 3,5 do Entity Framework Propriedades de navegação incluídas, mas não inclui propriedades de chave estrangeira no modelo de dados. Por exemplo, as colunas `CourseID` e `StudentID` da tabela `StudentGrade` seriam omitidas da entidade `StudentGrade`.

[![Image01](what-s-new-in-the-entity-framework-4/_static/image2.png)](what-s-new-in-the-entity-framework-4/_static/image1.png)

O motivo dessa abordagem foi que, estritamente falando, as chaves estrangeiras são um detalhe de implementação física e não pertencem a um modelo de dados conceitual. No entanto, como uma questão prática, geralmente é mais fácil trabalhar com entidades no código quando você tem acesso direto às chaves estrangeiras.

Para obter um exemplo de como as chaves estrangeiras no modelo de dados podem simplificar seu código, considere como você teria que codificar a página *DepartmentsAdd. aspx* sem elas. Na entidade `Department`, a propriedade `Administrator` é uma chave estrangeira que corresponde a `PersonID` na entidade `Person`. Para estabelecer a associação entre um novo departamento e seu administrador, tudo o que você precisava fazer foi definir o valor da propriedade `Administrator` no manipulador de eventos `ItemInserting` do controle de vinculação de valores:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample1.cs)]

Sem as chaves estrangeiras no modelo de dados, você trataria o evento de `Inserting` do controle da fonte de dados em vez do evento `ItemInserting` do controle de ligações, a fim de obter uma referência à entidade em si antes que a entidade seja adicionada ao conjunto de entidades. Quando você tiver essa referência, você estabelecerá a Associação usando um código como esse nos exemplos a seguir:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample2.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample3.cs)]

Como você pode ver na postagem do blog da equipe de Entity Framework [em associações de chave estrangeira](https://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx), há outros casos em que a diferença na complexidade do código é muito maior. Para atender às necessidades dos que preferem viver com os detalhes de implementação no modelo de dados conceitual para o código mais simples, o Entity Framework agora oferece a opção de incluir chaves estrangeiras no modelo de dados.

Na terminologia Entity Framework, se você incluir chaves estrangeiras no modelo de dados que está usando *associações de chave estrangeira*e se excluir chaves estrangeiras, você usará *associações independentes*.

## <a name="executing-user-defined-sql-commands"></a>Executando comandos SQL definidos pelo usuário

Em versões anteriores do Entity Framework, não havia uma maneira fácil de criar seus próprios comandos SQL imediatamente e executá-los. O Entity Framework dinamicamente gera comandos SQL para você, ou você tinha que criar um procedimento armazenado e importá-lo como uma função. A versão 4 adiciona `ExecuteStoreQuery` e `ExecuteStoreCommand` métodos à classe `ObjectContext` que facilitam a passagem de qualquer consulta diretamente para o banco de dados.

Suponha que os administradores da Contoso University desejam ser capazes de executar alterações em massa no banco de dados sem precisar passar pelo processo de criação de um procedimento armazenado e importá-lo para o modelo. Sua primeira solicitação é para uma página que permite alterar o número de créditos de todos os cursos no banco de dados. Na página da Web, eles desejam ser capazes de inserir um número a ser usado para multiplicar o valor de cada `Course` linha `Credits` coluna.

Crie uma nova página que usa a página mestra *site. Master* e nomeie-a *UpdateCredits. aspx*. Em seguida, adicione a seguinte marcação ao controle de `Content` chamado `Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample4.aspx)]

Essa marcação cria um controle de `TextBox` no qual o usuário pode inserir o valor de multiplicador, um controle de `Button` para clicar para executar o comando e um controle de `Label` para indicar o número de linhas afetadas.

Abra *UpdateCredits.aspx.cs*e adicione a seguinte instrução de `using` e um manipulador para o evento de `Click` do botão:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample5.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample6.cs)]

Esse código executa o comando SQL `Update` usando o valor na caixa de texto e usa o rótulo para exibir o número de linhas afetadas. Antes de executar a página, execute a página *cursos. aspx* para obter uma imagem "antes" de alguns dados.

[![Image02](what-s-new-in-the-entity-framework-4/_static/image4.png)](what-s-new-in-the-entity-framework-4/_static/image3.png)

Execute *UpdateCredits. aspx*, digite "10" como multiplicador e clique em **executar**.

[![Image03](what-s-new-in-the-entity-framework-4/_static/image6.png)](what-s-new-in-the-entity-framework-4/_static/image5.png)

Execute a página *cursos. aspx* novamente para ver os dados alterados.

[![Image04](what-s-new-in-the-entity-framework-4/_static/image8.png)](what-s-new-in-the-entity-framework-4/_static/image7.png)

(Se você quiser definir o número de créditos de volta para seus valores originais, em *UpdateCredits.aspx.cs* , altere `Credits * {0}` para `Credits / {0}` e execute novamente a página, inserindo 10 como o divisor.)

Para obter mais informações sobre a execução de consultas que você define no código, consulte [como: executar comandos diretamente na fonte de dados](https://msdn.microsoft.com/library/ee358769.aspx).

## <a name="model-first-development"></a>Modelo-primeiro desenvolvimento

Nestas orientações, você criou o banco de dados primeiro e, em seguida, gerou o modelo data com base na estrutura do banco de dado. No Entity Framework 4, você pode começar com o modelo de dados em vez disso e gerar o banco de dados com base na estrutura do modelo. Se você estiver criando um aplicativo para o qual o banco de dados ainda não existe, a abordagem modelo-primeiro permite criar entidades e relações que fazem sentido conceitualmente para o aplicativo, sem se preocupar com detalhes de implementação física . (No entanto, isso permanece sendo verdadeiro apenas nos estágios iniciais do desenvolvimento. Por fim, o banco de dados será criado e terá dado de produção nele, e a recriação do modelo não será mais prática; Nesse ponto, você voltará à abordagem do banco de dados primeiro.)

Nesta seção do tutorial, você criará um modelo de dados simples e irá gerá-lo a partir dele.

Em **Gerenciador de soluções**, clique com o botão direito do mouse na pasta *Dal* e selecione **Adicionar novo item**. Na caixa de diálogo **Adicionar novo item** , em **modelos instalados** , selecione **dados** e, em seguida, selecione o modelo de **modelo de dados de entidade ADO.net** . Nomeie o novo arquivo *AlumniAssociationModel. edmx* e clique em **Adicionar**.

[![Image06](what-s-new-in-the-entity-framework-4/_static/image10.png)](what-s-new-in-the-entity-framework-4/_static/image9.png)

Isso inicia o assistente de Modelo de Dados de Entidade. Na etapa **escolher conteúdo do modelo** , selecione **modelo vazio** e clique em **concluir**.

[![Image07](what-s-new-in-the-entity-framework-4/_static/image12.png)](what-s-new-in-the-entity-framework-4/_static/image11.png)

O **Designer de modelo de dados de entidade** é aberto com uma superfície de design em branco. Arraste um item de **entidade** da **caixa de ferramentas** para a superfície de design.

[![Image08](what-s-new-in-the-entity-framework-4/_static/image14.png)](what-s-new-in-the-entity-framework-4/_static/image13.png)

Altere o nome da entidade de `Entity1` para `Alumnus`, altere o nome da propriedade `Id` para `AlumnusId`e adicione uma nova propriedade escalar denominada `Name`. Para adicionar novas propriedades, você pode pressionar ENTER depois de alterar o nome da coluna `Id` ou clicar com o botão direito do mouse na entidade e selecionar **Adicionar Propriedade escalar**. O tipo padrão para novas propriedades é `String`, o que é bom para esta demonstração simples, mas é claro que você pode alterar coisas como tipo de dados na janela **Propriedades** .

Crie outra entidade da mesma maneira e nomeie-a `Donation`. Altere a propriedade `Id` para `DonationId` e adicione uma propriedade escalar denominada `DateAndAmount`.

[![Image09](what-s-new-in-the-entity-framework-4/_static/image16.png)](what-s-new-in-the-entity-framework-4/_static/image15.png)

Para adicionar uma associação entre essas duas entidades, clique com o botão direito do mouse na entidade `Alumnus`, selecione **Adicionar**e, em seguida, selecione **Associação**.

[![Image10](what-s-new-in-the-entity-framework-4/_static/image18.png)](what-s-new-in-the-entity-framework-4/_static/image17.png)

Os valores padrão na caixa de diálogo **Adicionar Associação** são o que você deseja (um-para-muitos, incluir propriedades de navegação, incluir chaves estrangeiras), portanto, basta clicar em **OK**.

[![Image11](what-s-new-in-the-entity-framework-4/_static/image20.png)](what-s-new-in-the-entity-framework-4/_static/image19.png)

O designer adiciona uma linha de associação e uma propriedade de chave estrangeira.

[![Image12](what-s-new-in-the-entity-framework-4/_static/image22.png)](what-s-new-in-the-entity-framework-4/_static/image21.png)

Agora você está pronto para criar o banco de dados. Clique com o botão direito do mouse na superfície de design e selecione **gerar banco de dados do modelo**.

[![Image13](what-s-new-in-the-entity-framework-4/_static/image24.png)](what-s-new-in-the-entity-framework-4/_static/image23.png)

Isso inicia o assistente para gerar banco de dados. (Se você vir avisos que indiquem que as entidades não estão mapeadas, você poderá ignorá-las por enquanto.)

Na etapa **escolher sua conexão de dados** , clique em **nova conexão**.

[![Image14](what-s-new-in-the-entity-framework-4/_static/image26.png)](what-s-new-in-the-entity-framework-4/_static/image25.png)

Na caixa de diálogo **Propriedades da conexão** , selecione a instância de SQL Server Express local e nomeie o banco de dados `AlumniAssociation`.

[![Image15](what-s-new-in-the-entity-framework-4/_static/image28.png)](what-s-new-in-the-entity-framework-4/_static/image27.png)

Clique em **Sim** quando for perguntado se deseja criar o banco de dados. Quando a etapa **escolher sua conexão de dados** for exibida novamente, clique em **Avançar**.

Na etapa **Resumo e configurações** , clique em **concluir**.

[![Image18](what-s-new-in-the-entity-framework-4/_static/image30.png)](what-s-new-in-the-entity-framework-4/_static/image29.png)

Um arquivo *. SQL* com comandos DDL (linguagem de definição de dados) é criado, mas os comandos ainda não foram executados.

[![Image20](what-s-new-in-the-entity-framework-4/_static/image32.png)](what-s-new-in-the-entity-framework-4/_static/image31.png)

Use uma ferramenta como **SQL Server Management Studio** para executar o script e criar as tabelas, como você pode ter feito quando criou o banco de dados `School` para [o primeiro tutorial na série de tutoriais introdução](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). (A menos que você tenha baixado o banco de dados.)

Agora você pode usar o modelo de dados `AlumniAssociation` em suas páginas da Web da mesma maneira que estava usando o modelo de `School`. Para experimentar, adicione alguns dados às tabelas e crie uma página da Web que exiba os dados.

Usando **Gerenciador de servidores**, adicione as linhas a seguir às tabelas `Alumnus` e `Donation`.

[![Image21](what-s-new-in-the-entity-framework-4/_static/image34.png)](what-s-new-in-the-entity-framework-4/_static/image33.png)

Crie uma nova página da Web chamada *Alumni. aspx* que usa a página mestra *site. Master* . Adicione a seguinte marcação ao controle de `Content` chamado `Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample7.aspx)]

Essa marcação cria controles de `GridView` aninhados, o externo para exibir nomes de Alumni e o interno para exibir datas e valores de doação.

Abra *Alumni.aspx.cs*. Adicione uma instrução `using` para a camada de acesso a dados e um manipulador para o evento `RowDataBound` do controle de `GridView` externo:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample8.cs)]

Esse código vincula o controle de `GridView` interno usando a propriedade de navegação `Donations` da entidade de `Alumnus` da linha atual.

Execute a página.

[![Image22](what-s-new-in-the-entity-framework-4/_static/image36.png)](what-s-new-in-the-entity-framework-4/_static/image35.png)

(Observação: Esta página está incluída no projeto que pode ser baixado, mas para fazê-lo funcionar, você deve criar o banco de dados em sua instância de SQL Server Express local; o banco de dados não está incluído como um arquivo *. MDF* na pasta *\_data do aplicativo* .)

Para obter mais informações sobre como usar o recurso de modelo-primeiro do Entity Framework, consulte [Model-First no Entity Framework 4](https://msdn.microsoft.com/data/ff830362.aspx).

## <a name="poco-support"></a>Suporte para POCO

Ao usar a metodologia de design controlada por domínio, você cria classes de dados que representam dados e comportamento relevantes para o domínio de negócios. Essas classes devem ser independentes de qualquer tecnologia específica usada para armazenar (manter) os dados; em outras palavras, eles devem ser *Persistence que desconhecem*. A ignorância de persistência também pode tornar uma classe mais fácil de testar unidade porque o projeto de teste de unidade pode usar qualquer tecnologia de persistência que seja mais conveniente para os testes. As versões anteriores do Entity Framework ofereciam suporte limitado para ignorância de persistência porque as classes de entidade tinham de herdar da classe `EntityObject` e, portanto, incluíam uma grande quantidade de funcionalidades específicas de Entity Framework.

O Entity Framework 4 introduz a capacidade de usar classes de entidade que não herdam da classe `EntityObject` e, portanto, são que desconhecem de persistência. No contexto do Entity Framework, classes como essa geralmente são chamadas de *objetos CLR antigos* (poco ou pocos). Você pode gravar as classes POCO manualmente ou pode gerá-las automaticamente com base em um modelo de dados existente usando modelos T4 (text template Transformation Toolkit) fornecidos pelo Entity Framework.

Para obter mais informações sobre como usar o POCOs no Entity Framework, consulte os seguintes recursos:

- [Trabalhando com entidades POCO](https://msdn.microsoft.com/library/dd456853.aspx). Este é um documento do MSDN que é uma visão geral do POCOs, com links para outros documentos que têm informações mais detalhadas.
- [Walkthrough: modelo poco para o Entity Framework](https://blogs.msdn.com/b/adonet/archive/2010/01/25/walkthrough-poco-template-for-the-entity-framework.aspx) Esta é uma postagem de blog da equipe de desenvolvimento Entity Framework, com links para outras postagens de blog sobre POCOs.

## <a name="code-first-development"></a>Desenvolvimento de código primeiro

O suporte a POCO no Entity Framework 4 ainda requer que você crie um modelo de dados e vincule suas classes de entidade ao modelo de dados. A próxima versão do Entity Framework incluirá um recurso chamado *desenvolvimento de primeiro código*. Esse recurso permite que você use o Entity Framework com suas próprias classes POCO sem a necessidade de usar o designer de modelo de dados ou um arquivo XML de modelo de dados. (Portanto, essa opção também foi chamada *somente de código*; o *código* e o código somente referem *-* se ao mesmo recurso Entity Framework.)

Para obter mais informações sobre como usar a abordagem Code-First para desenvolvimento, consulte os seguintes recursos:

- [Desenvolvimento de código primeiro com Entity Framework 4](https://weblogs.asp.net/scottgu/archive/2010/07/16/code-first-development-with-entity-framework-4.aspx). Esta é uma postagem de blog de Scott Guthrie introduzindo o desenvolvimento de código primeiro.
- [Blog da equipe de desenvolvimento do Entity Framework-postagens marcadas CodeOnly](https://blogs.msdn.com/b/efdesign/archive/tags/codeonly/)
- [Blog da equipe de desenvolvimento do Entity Framework-postagens marcadas Code First](https://blogs.msdn.com/b/efdesign/archive/tags/code+first/)
- [Tutorial da loja de música MVC-parte 4: modelos e acesso a dados](../../../../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4.md)
- [Introdução com MVC 3-parte 4: desenvolvimento de código-primeiro Entity Framework](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model.md)

Além disso, um novo tutorial do código MVC primeiro que cria um aplicativo semelhante ao aplicativo da Contoso University é projetado para ser publicado na Primavera de 2011 às [https://asp.net/entity-framework/tutorials](../../../../entity-framework.md)

## <a name="more-information"></a>Mais informações

Isso conclui a visão geral do que há de novo na Entity Framework e isso continua com a série de tutoriais de Entity Framework. Para obter mais informações sobre os novos recursos do Entity Framework 4 que não são abordados aqui, consulte os seguintes recursos:

- [O que há de novo no ADO.net](https://msdn.microsoft.com/library/ex6y04yf.aspx) Tópico do MSDN sobre novos recursos na versão 4 do Entity Framework.
- [Anunciando o lançamento do Entity Framework 4](https://blogs.msdn.com/b/efdesign/archive/2010/04/12/announcing-the-release-of-entity-framework-4.aspx) A postagem no blog da equipe de desenvolvimento do Entity Framework sobre os novos recursos da versão 4.

> [!div class="step-by-step"]
> [Anterior](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
