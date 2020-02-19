---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
title: Adicionando um modelo (VB) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensinará as noções básicas da criação de um aplicativo Web ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: b3aa7720-5c78-4ca2-baef-9a52234fb7ce
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: e69d59aed4d74f08f1c653c4965b128c4dbe20ff
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457239"
---
# <a name="adding-a-model-vb"></a>Adicionar um modelo (VB)

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> Este tutorial ensinará as noções básicas da criação de um aplicativo Web ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é uma versão gratuita do Microsoft Visual Studio. Antes de começar, verifique se você instalou os pré-requisitos listados abaixo. Você pode instalar todos eles clicando no seguinte link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, você pode instalar os pré-requisitos individualmente usando os seguintes links:
> 
> - [Pré-requisitos do Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Atualização de ferramentas do ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(suporte + ferramentas de tempo de execução)
> 
> Se você estiver usando o Visual Studio 2010 em vez do Visual Web Developer 2010, instale os pré-requisitos clicando no seguinte link: [pré-requisitos do Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Um projeto do Visual Web Developer com código-fonte VB.NET está disponível para acompanhar este tópico. [Baixe a versão do VB.net](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se preferir C#, alterne para a [ C# versão](../cs/adding-a-model.md) deste tutorial.

## <a name="adding-a-model"></a>Adicionar um modelo

Nesta seção, você adicionará algumas classes para gerenciar filmes em um banco de dados. Essas classes serão a parte "modelo" do aplicativo MVC ASP.NET.

Você usará uma .NET Framework tecnologia de acesso a dados conhecida como Entity Framework para definir e trabalhar com essas classes de modelo. A Entity Framework (geralmente conhecida como EF) dá suporte a um paradigma de desenvolvimento chamado *Code First*. Code First permite que você crie objetos de modelo escrevendo classes simples. (Elas também são conhecidas como classes POCO, de "objetos CLR antigos".) Em seguida, você pode fazer com que o banco de dados seja criado dinamicamente de suas classes, o que permite um fluxo de trabalho de desenvolvimento muito limpo e rápido.

## <a name="adding-model-classes"></a>Adicionando classes de modelo

Em **Gerenciador de soluções**, clique com o botão direito do mouse na pasta *modelos* , selecione **Adicionar**e, em seguida, selecione **classe**.

![](adding-a-model/_static/image1.png)

Nomeie a classe "filme".

Adicione as cinco propriedades a seguir à classe `Movie`:

[!code-vb[Main](adding-a-model/samples/sample1.vb)]

Usaremos a classe `Movie` para representar filmes em um banco de dados. Cada instância de um objeto `Movie` corresponderá a uma linha em uma tabela de banco de dados, e cada propriedade da classe `Movie` será mapeada para uma coluna na tabela.

No mesmo arquivo, adicione a seguinte classe de `MovieDBContext`:

[!code-vb[Main](adding-a-model/samples/sample2.vb)]

A classe `MovieDBContext` representa o contexto de banco de dados Entity Framework filme, que lida com a busca, o armazenamento e a atualização de `Movie` instâncias de classe em um banco de dados. O `MovieDBContext` deriva da classe base `DbContext` fornecida pelo Entity Framework. Para obter mais informações sobre `DbContext` e `DbSet`, consulte [melhorias de produtividade para o Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).

Para poder referenciar `DbContext` e `DbSet`, você precisa adicionar a seguinte instrução `imports` na parte superior do arquivo:

[!code-vb[Main](adding-a-model/samples/sample3.vb)]

O arquivo *Movie. vb* completo é mostrado abaixo.

[!code-vb[Main](adding-a-model/samples/sample4.vb)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a>Criando uma cadeia de conexão e trabalhando com SQL Server Compact

A classe `MovieDBContext` que você criou manipula a tarefa de conexão com o banco de dados e o mapeamento de objetos `Movie` para registros de banco de dados. No entanto, uma pergunta que você pode fazer é como especificar a qual banco de dados ele será conectado. Você fará isso adicionando informações de conexão no arquivo *Web. config* do aplicativo.

Abra o arquivo *Web. config da* raiz do aplicativo. (Não o arquivo *Web. config* na pasta *views* .) A imagem abaixo mostra os arquivos *Web. config* ; Abra o arquivo *Web. config* circulado em vermelho.

![](adding-a-model/_static/image2.png)

Adicione a seguinte cadeia de conexão ao elemento `<connectionStrings>` no arquivo *Web. config* .

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

O exemplo a seguir mostra uma parte do arquivo *Web. config* com a nova cadeia de conexão adicionada:

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

Essa pequena quantidade de código e XML é tudo o que você precisa para escrever a fim de representar e armazenar os dados do filme em um banco de dado.

Em seguida, você criará uma nova classe `MoviesController` que pode ser usada para exibir os dados do filme e permitir que os usuários criem novas listagens de filmes.

> [!div class="step-by-step"]
> [Anterior](adding-a-view.md)
> [Próximo](accessing-your-models-data-from-a-controller.md)
