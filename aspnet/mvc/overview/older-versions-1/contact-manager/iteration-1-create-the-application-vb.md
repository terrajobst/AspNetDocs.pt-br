---
uid: mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-vb
title: '#1 de iteração – criar o aplicativo (VB) | Microsoft Docs'
author: microsoft
description: 'Na primeira iteração, criamos o gerente de contatos da maneira mais simples possível. Adicionamos suporte para operações básicas de banco de dados: criar, ler, atualizar e D...'
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 5b033582-1646-42c2-b20d-7edc8814e970
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-vb
msc.type: authoredcontent
ms.openlocfilehash: c6bf4712fb734cf14420fd62c9eaf190a2c28168
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78602315"
---
# <a name="iteration-1--create-the-application-vb"></a>#1 de iteração – criar o aplicativo (VB)

pela [Microsoft](https://github.com/microsoft)

[Código de download](iteration-1-create-the-application-vb/_static/contactmanager_1_vb1.zip)

> Na primeira iteração, criamos o gerente de contatos da maneira mais simples possível. Adicionamos suporte para operações básicas de banco de dados: criar, ler, atualizar e excluir (CRUD).

## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Criando um aplicativo MVC de gerenciamento de contatos ASP.NET (VB)

Nesta série de tutoriais, criamos um aplicativo de gerenciamento de contatos inteiro do início ao fim. O aplicativo Contact Manager permite armazenar informações de contato-nomes, números de telefone e endereços de email – para obter uma lista de pessoas.

Criamos o aplicativo em várias iterações. Com cada iteração, aprimoramos gradualmente o aplicativo. O objetivo dessa abordagem de várias iterações é permitir que você entenda o motivo de cada alteração.

- #1 de iteração – crie o aplicativo. Na primeira iteração, criamos o gerente de contatos da maneira mais simples possível. Adicionamos suporte para operações básicas de banco de dados: criar, ler, atualizar e excluir (CRUD).

- #2 de iteração – faça com que o aplicativo fique bom. Nessa iteração, melhoramos a aparência do aplicativo modificando a página mestra de exibição do ASP.NET MVC padrão e a folha de estilos em cascata.

- #3 de iteração – adicionar validação de formulário. Na terceira iteração, adicionamos a validação básica de formulário. Impedimos que as pessoas enviem um formulário sem concluir os campos de formulário necessários. Também validamos endereços de email e números de telefone.

- #4 de iteração-torne o aplicativo levemente acoplado. Nesta quarta iteração, aproveitamos os vários padrões de design de software para facilitar a manutenção e a modificação do aplicativo Contact Manager. Por exemplo, podemos refatorar nosso aplicativo para usar o padrão de repositório e o padrão de injeção de dependência.

- #5 de iteração – criar testes de unidade. Na quinta iteração, tornamos o nosso aplicativo mais fácil de manter e modificar adicionando testes de unidade. Simulamos nossas classes de modelo de dados e criamos testes de unidade para nossos controladores e lógica de validação.

- #6 de iteração – use o desenvolvimento controlado por testes. Na sexta-iteração, adicionamos nova funcionalidade ao nosso aplicativo escrevendo testes de unidade primeiro e escrevendo código em relação aos testes de unidade. Nessa iteração, adicionamos grupos de contatos.

- #7 de iteração – adicione funcionalidade Ajax. Na sétima iteração, melhoramos a capacidade de resposta e o desempenho do nosso aplicativo adicionando suporte para AJAX.

## <a name="this-iteration"></a>Esta iteração

Nesta primeira iteração, criamos o aplicativo básico. O objetivo é criar o gerente de contatos da maneira mais rápida e simples possível. Em iterações posteriores, melhoramos o design do aplicativo.

O aplicativo Contact Manager é um aplicativo básico controlado por banco de dados. Você pode usar o aplicativo para criar novos contatos, editar contatos existentes e excluir contatos.

Nessa iteração, concluimos as seguintes etapas:

1. Aplicativo MVC ASP.NET
2. Criar um banco de dados para armazenar nossos contatos
3. Gerar uma classe de modelo para nosso banco de dados com o Microsoft Entity Framework
4. Criar uma ação de controlador e uma exibição que nos permite listar todos os contatos no banco de dados
5. Criar ações do controlador e uma exibição que nos permite criar um novo contato no banco de dados
6. Criar ações do controlador e uma exibição que nos permite editar um contato existente no banco de dados
7. Criar ações do controlador e uma exibição que nos permite excluir um contato existente no banco de dados

## <a name="software-prerequisites"></a>Pré-requisitos de software

Em aplicativos MVC ASP.NET, você deve ter o Visual Studio 2008 ou o Visual Web Developer 2008 instalado em seu computador (o Visual Web Developer é uma versão gratuita do Visual Studio que não inclui todos os recursos avançados do Visual Studio). Você pode baixar a versão de avaliação do Visual Studio 2008 ou Visual Web Developer do seguinte endereço:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

> [!NOTE] 
> 
> Para aplicativos MVC ASP.NET com o Visual Web Developer, você deve ter o Visual Web Developer Service Pack 1 instalado. Sem o Service Pack 1, você não pode criar projetos de aplicativos Web.

ASP.NET MVC Framework. Você pode baixar o ASP.NET MVC Framework a partir do seguinte endereço:

[https://www.asp.net/mvc](../../../index.md)

Neste tutorial, usamos o Entity Framework da Microsoft para acessar um banco de dados. O Entity Framework está incluído no .NET Framework 3,5 Service Pack 1. Você pode baixar esse service pack do seguinte local:

[https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;d isplaylang = en](https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang=en)

Como alternativa para executar cada um desses downloads um por um, você pode aproveitar o Web Platform Installer (Web PI). Você pode baixar o Web PI do seguinte endereço:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

## <a name="aspnet-mvc-project"></a>Projeto MVC do ASP.NET

Projeto de aplicativo Web do ASP.NET MVC. Inicie o Visual Studio e selecione o arquivo de opções de menu **, novo projeto**. A caixa de diálogo **novo projeto** é exibida (consulte a Figura 1). Selecione o tipo de projeto **Web** e o modelo de **aplicativo Web do ASP.NET MVC** . Nomeie o novo projeto *ContactManager* e clique no botão OK.

Verifique se você tem .NET Framework 3,5 selecionado na lista suspensa na parte superior direita da caixa de diálogo **novo projeto** . Caso contrário, o modelo de aplicativo Web ASP.NET MVC não aparecerá.

[![caixa de diálogo novo projeto](iteration-1-create-the-application-vb/_static/image1.jpg)](iteration-1-create-the-application-vb/_static/image1.png)

**Figura 01**: a caixa de diálogo novo projeto ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image2.png))

Aplicativo MVC ASP.NET, a caixa de diálogo **criar projeto de teste de unidade** é exibida. Você pode usar essa caixa de diálogo para indicar que deseja criar e adicionar um projeto de teste de unidade à sua solução ao criar seu aplicativo MVC do ASP.NET. Embora não estejamos criando testes de unidade nessa iteração, você deve selecionar a opção **Sim, criar um projeto de teste de unidade** , pois planejamos adicionar testes de unidade em uma iteração posterior. Adicionar um projeto de teste quando você cria um novo projeto MVC do ASP.NET é muito mais fácil do que adicionar um projeto de teste depois que o projeto ASP.NET MVC tiver sido criado.

> [!NOTE] 
> 
> Como o Visual Web Developer não dá suporte a projetos de teste, você não obtém a caixa de diálogo Criar projeto de teste de unidade ao usar o Visual Web Developer.

[![caixa de diálogo novo projeto](iteration-1-create-the-application-vb/_static/image2.jpg)](iteration-1-create-the-application-vb/_static/image3.png)

**Figura 02**: a caixa de diálogo Criar projeto de teste de unidade ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image4.png))

O aplicativo ASP.NET MVC aparece na janela de Gerenciador de Soluções do Visual Studio (veja a Figura 3). Se você não vir a janela de Gerenciador de Soluções, poderá abrir essa janela selecionando a exibição de opção de menu **Gerenciador de soluções**. Observe que a solução contém dois projetos: o projeto MVC do ASP.NET e o projeto de teste. O projeto MVC do ASP.NET é chamado ContactManager e o projeto de teste é denominado ContactManager. Tests.

[![caixa de diálogo novo projeto](iteration-1-create-the-application-vb/_static/image3.jpg)](iteration-1-create-the-application-vb/_static/image5.png)

**Figura 03**: a janela de Gerenciador de soluções ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image6.png))

## <a name="deleting-the-project-sample-files"></a>Excluindo os arquivos de exemplo do projeto

O modelo de projeto ASP.NET MVC inclui arquivos de exemplo para controladores e exibições. Antes de criar um novo aplicativo MVC do ASP.NET, exclua esses arquivos. Você pode excluir arquivos e pastas na janela de Gerenciador de Soluções clicando com o botão direito do mouse em um arquivo ou pasta e selecionando a opção de menu **excluir**.

Você precisa excluir os seguintes arquivos do projeto MVC do ASP.NET:

- \Controllers\HomeController.vb

- \Views\Home\About.aspx

- \Views\Home\Index.aspx

E, você precisa excluir o seguinte arquivo do projeto de teste:

\Controllers\HomeControllerTest.vb

## <a name="creating-the-database"></a>Criando o banco de dados

O aplicativo Gerenciador de contatos é um aplicativo Web controlado por banco de dados. Usamos um banco de dados para armazenar as informações de contato.

O ASP.NET MVC Framework com qualquer banco de dados moderno, incluindo Microsoft SQL Server, Oracle, MySQL e IBM DB2. Neste tutorial, usamos um banco de dados Microsoft SQL Server. Ao instalar o Visual Studio, você receberá a opção de instalar Microsoft SQL Server Express que é uma versão gratuita do banco de dados Microsoft SQL Server.

Crie um novo banco de dados clicando com o botão direito do mouse na pasta\_data do aplicativo na janela Gerenciador de Soluções e selecionando a opção de menu **Adicionar, novo item**. Na caixa de diálogo **Adicionar novo item** , selecione a categoria de dados e o modelo de banco de **dado** **SQL Server** (consulte a Figura 4). Nomeie o novo banco de dados ContactManagerDB. MDF e clique no botão OK.

[![caixa de diálogo novo projeto](iteration-1-create-the-application-vb/_static/image4.jpg)](iteration-1-create-the-application-vb/_static/image7.png)

**Figura 04**: Criando um novo banco de dados Microsoft SQL Server Express ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image8.png))

Após a criação do novo banco de dados, o banco de dados aparecerá na pasta Data\_de aplicativos na janela Gerenciador de Soluções. Clique duas vezes no arquivo ContactManager. MDF para abrir a janela Gerenciador de Servidores e conectar-se ao banco de dados.

> [!NOTE] 
> 
> A janela de Gerenciador de Servidores é chamada de janela de Gerenciador de Banco de Dados no caso do Microsoft Visual Web Developer.

Você pode usar a janela Gerenciador de Servidores para criar novos objetos de banco de dados, como tabelas de banco de dados, exibições, gatilhos e procedimentos armazenados. Clique com o botão direito do mouse na pasta tabelas e selecione a opção de menu **Adicionar nova tabela**. O banco de dados Designer de Tabela aparece (consulte a Figura 5).

[![caixa de diálogo novo projeto](iteration-1-create-the-application-vb/_static/image5.jpg)](iteration-1-create-the-application-vb/_static/image9.png)

**Figura 05**: a designer de tabela do banco de dados ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image10.png))

Precisamos criar uma tabela que contenha as seguintes colunas:

<a id="0.2_table01"></a>

| **Nome da Coluna** | **Tipo de Dados** | **Permitir Nulos** |
| --- | --- | --- |
| ID | INT | false |
| Nome | nvarchar (50) | false |
| LastName | nvarchar (50) | false |
| Telefone | nvarchar (50) | false |
| Email | nvarchar (255) | false |

A primeira coluna, a coluna ID, é especial. Você precisa marcar a coluna ID como uma coluna de identidade e uma coluna de chave primária. Você indica que uma coluna é uma coluna de identidade expandindo as propriedades da coluna (Observe a parte inferior da Figura 6) e rolando para baixo até a propriedade especificação de identidade. Defina a propriedade **(is Identity)** com o valor **Yes**.

Você marca uma coluna como uma coluna de chave primária selecionando a coluna e clicando no botão com o ícone de uma chave. Depois que uma coluna é marcada como uma coluna de chave primária, um ícone de uma chave é exibido ao lado da coluna (consulte a Figura 6).

Depois de concluir a criação da tabela, clique no botão salvar (o botão com um ícone de um disquete) para salvar a nova tabela. Dê à sua nova tabela o nome *Contacts*.

Depois de concluir a criação da tabela de banco de dados de contatos, você deve adicionar alguns registros à tabela. Clique com o botão direito do mouse na tabela contatos na janela Gerenciador de Servidores e selecione a opção de menu **Mostrar dados da tabela**. Insira um ou mais contatos na grade que aparece.

## <a name="creating-the-data-model"></a>Criando o modelo de dados

O aplicativo MVC ASP.NET consiste em modelos, exibições e controladores. Começamos criando uma classe de modelo que representa a tabela de contatos que criamos na seção anterior.

Neste tutorial, usamos o Entity Framework da Microsoft para gerar uma classe de modelo a partir do banco de dados automaticamente.

> [!NOTE] 
> 
> O ASP.NET MVC Framework não está vinculado ao Microsoft Entity Framework de forma alguma. Você pode usar o ASP.NET MVC com tecnologias de acesso de banco de dados alternativas, incluindo NHibernate, LINQ to SQL ou ADO.NET.

Siga estas etapas para criar as classes de modelo de dados:

1. Clique com o botão direito do mouse na pasta modelos na janela Gerenciador de Soluções e selecione **Adicionar, novo item**. A caixa de diálogo **Adicionar novo item** é exibida (consulte a Figura 6).
2. Selecione a categoria de **dados** e o modelo de **modelo de dados de entidade ADO.net** . Nomeie o modelo de dados *ContactManagerModel. edmx* e clique no botão **Adicionar** . O assistente de Modelo de Dados de Entidade é exibido (veja a Figura 7).
3. Na etapa **escolher conteúdo do modelo** , selecione **gerar do banco de dados** (veja a Figura 7).
4. Na etapa **escolher sua conexão de dados** , selecione o banco de ContactManagerDB. MDF e insira o nome *ContactManagerDBEntities* para as configurações de conexão de entidade (consulte a Figura 8).
5. Na etapa **escolher seus objetos de banco de dados** , marque a caixa de seleção tabelas rotuladas (consulte a Figura 9). O modelo de dados incluirá todas as tabelas contidas no banco de dado (há apenas uma, a tabela contatos). Insira os *modelos*de namespace. Clique no botão Concluir para concluir o assistente.

[![caixa de diálogo novo projeto](iteration-1-create-the-application-vb/_static/image6.jpg)](iteration-1-create-the-application-vb/_static/image11.png)

**Figura 06**: a caixa de diálogo Adicionar novo item ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image12.png))

[![caixa de diálogo novo projeto](iteration-1-create-the-application-vb/_static/image7.jpg)](iteration-1-create-the-application-vb/_static/image13.png)

**Figura 07**: escolher conteúdo do modelo ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image14.png))

[![caixa de diálogo novo projeto](iteration-1-create-the-application-vb/_static/image8.jpg)](iteration-1-create-the-application-vb/_static/image15.png)

**Figura 08**: escolha sua conexão de dados ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image16.png))

[![caixa de diálogo novo projeto](iteration-1-create-the-application-vb/_static/image9.jpg)](iteration-1-create-the-application-vb/_static/image17.png)

**Figura 09**: escolher seus objetos de banco de dados ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image18.png))

Depois de concluir o assistente de Modelo de Dados de Entidade, o Modelo de Dados de Entidade designer é exibido. O designer exibe uma classe que corresponde a cada tabela que está sendo modelada. Você deve ver uma classe denominada contatos.

O assistente de Modelo de Dados de Entidade gera nomes de classe baseados em nomes de tabela de banco de dados. Você quase sempre precisa alterar o nome da classe gerada pelo assistente. Clique com o botão direito do mouse na classe contatos no designer e selecione a opção de menu **renomear**. Altere o nome da classe de Contacts (plural) para contact (singular). Depois de alterar o nome da classe, a classe deve aparecer como a Figura 10.

[![caixa de diálogo novo projeto](iteration-1-create-the-application-vb/_static/image10.jpg)](iteration-1-create-the-application-vb/_static/image19.png)

**Figura 10**: a classe Contact ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image20.png))

Neste ponto, criamos nosso modelo de banco de dados. Podemos usar a classe Contact para representar um registro de contato específico em nosso banco de dados.

## <a name="creating-the-home-controller"></a>Criando o controlador doméstico

A próxima etapa é criar nosso controlador doméstico. O controlador inicial é o controlador padrão invocado em um aplicativo MVC ASP.NET.

Crie a classe do controlador Home clicando com o botão direito do mouse na pasta controladores na janela Gerenciador de Soluções e selecionando a opção de menu **Adicionar, controlador** (consulte a Figura 11). Observe a caixa de seleção **Adicionar métodos de ação para cenários de criação, atualização e detalhes**. Certifique-se de que essa caixa de seleção esteja marcada antes de clicar no botão **Adicionar** .

[![caixa de diálogo novo projeto](iteration-1-create-the-application-vb/_static/image11.jpg)](iteration-1-create-the-application-vb/_static/image21.png)

**Figura 11**: adicionando o controlador inicial ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image22.png))

Ao criar o controlador doméstico, você obtém a classe na Listagem 1.

**Listagem 1-Controllers\HomeController.vb**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample1.vb)]

## <a name="listing-the-contacts"></a>Listando os contatos

Para exibir os registros na tabela de banco de dados de contatos, precisamos criar uma ação de índice () e uma exibição de índice.

O controlador inicial já contém uma ação de índice (). Precisamos modificar esse método para que ele seja semelhante à listagem 2.

**Listagem 2-Controllers\HomeController.vb**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample2.vb)]

Observe que a classe do controlador Home na Listagem 2 contém um campo privado denominado \_Entities. O campo \_entidades representa as entidades do modelo de dados. Usamos o campo \_entidades para se comunicar com o banco de dados.

O método index () retorna uma exibição que representa todos os contatos da tabela de banco de dados Contacts. A expressão \_entidades. Contactset. ToList () retorna a lista de contatos como uma lista genérica.

Agora que criamos o controlador de índice, precisamos criar a exibição de índice. Antes de criar o modo de exibição de índice, compile o aplicativo selecionando a opção de menu **Compilar, Compilar solução**. Você sempre deve compilar seu projeto antes de adicionar um modo de exibição para que a lista de classes de modelo seja exibida na caixa de diálogo **Adicionar exibição** .

Você cria a exibição de índice clicando com o botão direito do mouse no método index () e selecionando a opção de menu **Add View** (consulte a Figura 12). A seleção dessa opção de menu abre a caixa de diálogo **Adicionar exibição** (veja a Figura 13).

[![caixa de diálogo novo projeto](iteration-1-create-the-application-vb/_static/image12.jpg)](iteration-1-create-the-application-vb/_static/image23.png)

**Figura 12**: adicionando a exibição de índice ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image24.png))

Na caixa de diálogo **Adicionar exibição** , marque a caixa de seleção **criar uma exibição fortemente tipada**. Selecione a classe exibir dados ContactManager. Contact e a lista exibir conteúdo. A seleção dessas opções gera uma exibição que exibe uma lista de registros de contato.

[![caixa de diálogo novo projeto](iteration-1-create-the-application-vb/_static/image13.jpg)](iteration-1-create-the-application-vb/_static/image25.png)

**Figura 13**: a caixa de diálogo Adicionar exibição ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image26.png))

Quando você clica no botão **Adicionar** , a exibição de índice na Listagem 3 é gerada. Observe a diretiva &lt;% @ Page%&gt; que aparece na parte superior do arquivo. A exibição de índice herda do ViewPage&lt;IEnumerable&lt;ContactManager. Models. Contact&gt;classe &gt;. Em outras palavras, a classe de modelo na exibição representa uma lista de entidades de contato.

O corpo da exibição de índice contém um loop foreach que itera em cada um dos contatos representados pela classe Model. O valor de cada propriedade da classe Contact é exibido em uma tabela HTML.

**Listagem 3-Views\Home\Index.aspx (não modificado)**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample3.aspx)]

Precisamos fazer uma modificação na exibição de índice. Como não estamos criando uma exibição de detalhes, podemos remover o link detalhes. Localize e remova o seguinte código da exibição de índice:

{. ID = item. ID})%&gt;

Depois de modificar a exibição de índice, você pode executar o aplicativo Contact Manager. Selecione a opção de menu Depurar, iniciar depuração ou simplesmente pressione F5. Na primeira vez que você executar o aplicativo, você receberá a caixa de diálogo da Figura 14. Selecione a opção **Modificar o arquivo Web. config para habilitar a depuração** e clique no botão OK.

[![caixa de diálogo novo projeto](iteration-1-create-the-application-vb/_static/image14.jpg)](iteration-1-create-the-application-vb/_static/image27.png)

**Figura 14**: Habilitando a depuração ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image28.png))

A exibição de índice é retornada por padrão. Essa exibição lista todos os dados da tabela de banco de dados de contatos (consulte a Figura 15).

[![caixa de diálogo novo projeto](iteration-1-create-the-application-vb/_static/image15.jpg)](iteration-1-create-the-application-vb/_static/image29.png)

**Figura 15**: a exibição do índice ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image30.png))

Observe que a exibição de índice inclui um link rotulado criar novo na parte inferior da exibição. Na próxima seção, você aprenderá a criar novos contatos.

## <a name="creating-new-contacts"></a>Criando novos contatos

Para permitir que os usuários criem novos contatos, precisamos adicionar duas ações Create () ao controlador Home. Precisamos criar uma ação Create () que retorna um formulário HTML para a criação de um novo contato. Precisamos criar uma segunda ação Create () que executa a inserção real do banco de dados do novo contato.

Os novos métodos Create () que precisamos adicionar ao controlador Home estão contidos na Listagem 4.

**Listagem 4-Controllers\HomeController.vb (com métodos Create)**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample4.vb)]

O primeiro método Create () pode ser invocado com um HTTP GET, enquanto o segundo método Create () pode ser invocado apenas por um HTTP POST. Em outras palavras, o segundo método Create () pode ser invocado somente durante a postagem de um formulário HTML. O primeiro método Create () simplesmente retorna uma exibição que contém o formulário HTML para a criação de um novo contato. O segundo método Create () é muito mais interessante: ele adiciona o novo contato ao banco de dados.

Observe que o segundo método Create () foi modificado para aceitar uma instância da classe Contact. Os valores de formulário postados do formulário HTML são associados a essa classe de contato pela estrutura MVC do ASP.NET automaticamente. Cada campo de formulário do formulário de criação de HTML é atribuído a uma propriedade do parâmetro de contato.

Observe que o parâmetro de contato é decorado com um atributo [BIND]. O atributo [BIND] é usado para excluir a propriedade de ID de contato da associação. Como a propriedade ID representa uma propriedade Identity, não queremos definir a propriedade ID.

No corpo do método Create (), o Entity Framework é usado para inserir o novo contato no banco de dados. O novo contato é adicionado ao conjunto existente de contatos e o método SaveChanges () é chamado para enviar essas alterações de volta ao banco de dados subjacente.

Você pode gerar um formulário HTML para criar novos contatos clicando com o botão direito do mouse em um dos dois métodos Create () e selecionando a opção de menu **Add View** (consulte a Figura 16).

[![caixa de diálogo novo projeto](iteration-1-create-the-application-vb/_static/image16.jpg)](iteration-1-create-the-application-vb/_static/image31.png)

**Figura 16**: adicionando o modo de exibição de criação ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image32.png))

Na caixa de diálogo **Adicionar exibição** , selecione a classe **ContactManager. Contact** e a opção **criar** para exibir conteúdo (veja a figura 17). Quando você clica no botão **Adicionar** , um modo de exibição criar é gerado automaticamente.

[![caixa de diálogo novo projeto](iteration-1-create-the-application-vb/_static/image17.jpg)](iteration-1-create-the-application-vb/_static/image33.png)

**Figura 17**: ver um detalhamento de página ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image34.png))

A exibição criar contém campos de formulário para cada uma das propriedades da classe Contact. O código para o modo de exibição de criação é incluído na listagem 5.

**Listagem 5-Views\Home\Create.aspx**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample5.aspx)]

Depois de modificar os métodos Create () e adicionar o modo de exibição criar, você pode executar o aplicativo Contact Manager e criar novos contatos. Clique no link **criar novo** que aparece na exibição índice para navegar até a exibição criar. Você deve ver a exibição na Figura 18.

[![caixa de diálogo novo projeto](iteration-1-create-the-application-vb/_static/image18.jpg)](iteration-1-create-the-application-vb/_static/image35.png)

**Figura 18**: o modo de exibição de criação ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image36.png))

## <a name="editing-contacts"></a>Editando contatos

Adicionar a funcionalidade para editar um registro de contato é muito semelhante a adicionar a funcionalidade para criar novos registros de contato. Primeiro, precisamos adicionar dois novos métodos Edit à classe Home Controller. Esses novos métodos Edit () estão contidos na Listagem 6.

**Listagem 6-Controllers\HomeController.vb (com métodos de edição)**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample6.vb)]

O primeiro método Edit () é invocado por uma operação HTTP GET. Um parâmetro de ID é passado para esse método que representa a ID do registro de contato que está sendo editado. O Entity Framework é usado para recuperar um contato que corresponda à ID. Uma exibição que contém um formulário HTML para editar um registro é retornada.

O segundo método Edit () executa a atualização real para o banco de dados. Esse método aceita uma instância da classe contact como um parâmetro. A estrutura MVC do ASP.NET associa os campos de formulário do formulário de edição a essa classe automaticamente. Observe que você não inclui o atributo [BIND] ao editar um contato (precisamos do valor da propriedade ID).

O Entity Framework é usado para salvar o contato modificado no banco de dados. Primeiro, o contato original deve ser recuperado do banco de dados. Em seguida, o Método Entity Framework ApplyPropertyChanges () é chamado para registrar as alterações no contato. Por fim, o Método Entity Framework SaveChanges () é chamado para persistir as alterações no banco de dados subjacente.

Você pode gerar a exibição que contém o formulário de edição clicando com o botão direito do mouse no método Edit () e selecionando a opção de menu Add View. Na caixa de diálogo Adicionar exibição, selecione a classe **ContactManager. Models. Contact** e o conteúdo de exibição de **edição** (veja a Figura 19).

[![caixa de diálogo novo projeto](iteration-1-create-the-application-vb/_static/image19.jpg)](iteration-1-create-the-application-vb/_static/image37.png)

**Figura 19**: adicionando um modo de exibição de edição ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image38.png))

Quando você clica no botão Adicionar, uma nova exibição de edição é gerada automaticamente. O formulário HTML gerado contém campos que correspondem a cada uma das propriedades da classe Contact (consulte a listagem 7).

**Listagem 7-Views\Home\Edit.aspx**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample7.aspx)]

## <a name="deleting-contacts"></a>Excluindo contatos

Se você quiser excluir contatos, precisará adicionar duas ações Delete () à classe Home Controller. A primeira ação DELETE () exibe um formulário de confirmação de exclusão. A segunda ação DELETE () executa a exclusão real.

> [!NOTE] 
> 
> Posteriormente, na iteração #7, modificamos o Gerenciador de contatos para que ele dê suporte a uma única etapa de exclusão do Ajax.

Os dois novos métodos Delete () estão contidos na Listagem 8.

**Listagem 8-Controllers\HomeController.vb (métodos Delete)**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample8.vb)]

O primeiro método Delete () retorna um formulário de confirmação para excluir um registro de contato do banco de dados (consulte Figure20). O segundo método Delete () executa a operação de exclusão real no banco de dados. Depois que o contato original for recuperado do banco de dados, os métodos Entity Framework ExcluirObjeto () e SaveChanges () serão chamados para executar a exclusão do banco de dados.

[![caixa de diálogo novo projeto](iteration-1-create-the-application-vb/_static/image20.jpg)](iteration-1-create-the-application-vb/_static/image39.png)

**Figura 20**: a exibição de confirmação de exclusão ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image40.png))

Precisamos modificar a exibição do índice para que ela contenha um link para excluir registros de contato (veja a figura 21). Você precisa adicionar o seguinte código à mesma célula da tabela que contém o link de edição:

{. ID = item. ID})%&gt;

[![caixa de diálogo novo projeto](iteration-1-create-the-application-vb/_static/image21.jpg)](iteration-1-create-the-application-vb/_static/image41.png)

**Figura 21**: exibição de índice com o link de edição ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image42.png))

Em seguida, precisamos criar a exibição de confirmação de exclusão. Clique com o botão direito do mouse no método Delete () na classe Home Controller e selecione a opção de menu Add View. A caixa de diálogo Adicionar exibição é exibida (consulte a Figura 22).

Ao contrário do caso da lista, criação e edição de exibições, a caixa de diálogo Adicionar exibição não contém uma opção para criar uma exibição de exclusão. Em vez disso, selecione a classe de dados **ContactManager. Models. Contact** e o conteúdo de exibição **vazio** . A seleção da opção de exibir conteúdo em branco exigirá que possamos criar a exibição de nós.

[![caixa de diálogo novo projeto](iteration-1-create-the-application-vb/_static/image22.jpg)](iteration-1-create-the-application-vb/_static/image43.png)

**Figura 22**: adicionando a exibição de confirmação de exclusão ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image44.png))

O conteúdo da exibição de exclusão está contido na listagem 9. Essa exibição contém um formulário que confirma se um contato específico deve ou não ser excluído (veja a figura 21).

**Listagem 9-Views\Home\Delete.aspx**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample9.aspx)]

## <a name="changing-the-name-of-the-default-controller"></a>Alterando o nome do controlador padrão

Pode se preocupar com que o nome da nossa classe Controller para trabalhar com contatos é chamado de classe HomeController. O controlador não deve ser nomeado ContactController?

Esse problema é bastante fácil de corrigir. Primeiro, precisamos refatorar o nome do controlador doméstico. Abra a classe HomeController no editor de Visual Studio Code, clique com o botão direito do mouse no nome da classe e selecione a opção de menu **renomear**. A seleção dessa opção de menu abre a caixa de diálogo Renomear.

[![caixa de diálogo novo projeto](iteration-1-create-the-application-vb/_static/image23.jpg)](iteration-1-create-the-application-vb/_static/image45.png)

**Figura 23**: refatorar um nome de controlador ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image46.png))

[![caixa de diálogo novo projeto](iteration-1-create-the-application-vb/_static/image24.jpg)](iteration-1-create-the-application-vb/_static/image47.png)

**Figura 24**: usando a caixa de diálogo Renomear ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image48.png))

Se você renomear sua classe de controlador, o Visual Studio atualizará o nome da pasta na pasta views também. O Visual Studio renomeará a pasta \Views\Home para a pasta \Views\Contact.

Depois de fazer essa alteração, seu aplicativo não terá mais um controlador doméstico. Ao executar o aplicativo, você receberá a página de erro na figura 25.

[![caixa de diálogo novo projeto](iteration-1-create-the-application-vb/_static/image25.jpg)](iteration-1-create-the-application-vb/_static/image49.png)

**Figura 25**: nenhum controlador padrão ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image50.png))

Precisamos atualizar a rota padrão no arquivo global. asax para usar o controlador de contato em vez do controlador doméstico. Abra o arquivo global. asax e modifique o controlador padrão usado pela rota padrão (consulte a listagem 10).

**Listagem 10-global. asax. vb**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample10.vb)]

Depois de fazer essas alterações, o gerente de contato será executado corretamente. Agora, ele usará a classe de controlador de contato como o controlador padrão.

## <a name="summary"></a>Resumo

Nesta primeira iteração, criamos um aplicativo de Gerenciador de contatos básico da maneira mais rápida possível. Tiramos proveito do Visual Studio para gerar o código inicial para nossos controladores e exibições automaticamente. Também tiramos proveito do Entity Framework para gerar automaticamente nossas classes de modelo de banco de dados.

Atualmente, podemos listar, criar, editar e excluir registros de contato com o aplicativo Contact Manager. Em outras palavras, podemos executar todas as operações básicas de banco de dados exigidas por um aplicativo Web controlado por banco de dados.

Infelizmente, nosso aplicativo apresenta alguns problemas. Primeiro, eu hesita em admitir isso, o aplicativo Contact Manager não é o aplicativo mais atrativo. Ele precisa de algum trabalho de design. Na próxima iteração, veremos como é possível alterar a página mestra de exibição padrão e a folha de estilos em cascata para melhorar a aparência do aplicativo.

Em segundo lugar, não implementamos nenhuma validação de formulário. Por exemplo, não há nada para impedir que você envie o formulário criar contato sem inserir valores para qualquer um dos campos de formulário. Além disso, você pode inserir números de telefone e endereços de email inválidos. Começamos a lidar com o problema de validação de formulário na iteração #3.

Finalmente, e o mais importante é que a iteração atual do aplicativo Contact Manager não pode ser facilmente modificada ou mantida. Por exemplo, a lógica de acesso ao banco de dados é inclusas diretamente nas ações do controlador. Isso significa que não podemos modificar nosso código de acesso a dados sem modificar nossos controladores. Em iterações posteriores, exploraremos os padrões de design de software que podemos implementar para tornar o Gerenciador de contatos mais resiliente a mudar.

> [!div class="step-by-step"]
> [Anterior](iteration-7-add-ajax-functionality-cs.md)
> [Próximo](iteration-2-make-the-application-look-nice-vb.md)
