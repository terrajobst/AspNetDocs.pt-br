---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: 'Parte 4: modelos e acesso a dados | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo de loja de música MVC do ASP.NET. A parte 4 abrange modelos e acesso a dados.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 402be340f1ea3344675e7b859cea8c5130cfc8ee
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559671"
---
# <a name="part-4-models-and-data-access"></a>Parte 4: modelos e acesso a dados

por [Jon Galloway](https://github.com/jongalloway)

> A loja de música MVC é um aplicativo de tutorial que apresenta e explica passo a passo como usar o ASP.NET MVC e o Visual Studio para desenvolvimento para a Web.  
>   
> A loja de música MVC é uma implementação de armazenamento de exemplo leve que vende os álbuns de música online e implementa a administração básica de site, a entrada do usuário e a funcionalidade do carrinho de compras.
> 
> Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo de loja de música MVC do ASP.NET. A parte 4 abrange modelos e acesso a dados.

Até agora, acabamos passando "dados fictícios" de nossos controladores para nossos modelos de exibição. Agora estamos prontos para conectar um banco de dados real. Neste tutorial, abordaremos como usar o SQL Server Compact Edition (geralmente chamado de SQL CE) como nosso mecanismo de banco de dados. O SQL CE é um banco de dados baseado em arquivo, inserido e gratuito que não requer instalação ou configuração, o que o torna muito conveniente para o desenvolvimento local.

## <a name="database-access-with-entity-framework-code-first"></a>Acesso ao banco de dados com Entity Framework Code-First

Usaremos o suporte do Entity Framework (EF) que está incluído nos projetos do ASP.NET MVC 3 para consultar e atualizar o banco de dados. O EF é uma API de dados de ORM (mapeamento relacional de objeto) flexível que permite aos desenvolvedores consultar e atualizar os dados armazenados em um banco de dado de forma orientada a objeto.

Entity Framework versão 4 dá suporte a um paradigma de desenvolvimento chamado Code-First. O Code-First permite que você crie um objeto de modelo escrevendo classes simples (também conhecidas como POCO de objetos CLR "Plain-Old") e pode até mesmo criar o banco de dados em tempo real a partir de suas classes.

### <a name="changes-to-our-model-classes"></a>Alterações em nossas classes de modelo

Usaremos o recurso de criação de banco de dados no Entity Framework neste tutorial. No entanto, antes de fazermos isso, vamos fazer algumas pequenas alterações em nossas classes de modelo para adicionar algumas coisas que usaremos posteriormente.

#### <a name="adding-the-artist-model-classes"></a>Adicionando as classes de modelo do artista

Nossos álbuns serão associados a artistas. portanto, vamos adicionar uma classe de modelo simples para descrever um artista. Adicione uma nova classe à pasta modelos chamada Artist.cs usando o código mostrado abaixo.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a>Atualizando nossas classes de modelo

Atualize a classe de álbum, conforme mostrado abaixo.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

Em seguida, faça as seguintes atualizações para a classe gênero.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-app_data-folder"></a>Adicionando a pasta de dados do\_de aplicativos

Adicionaremos um diretório de dados do\_de aplicativos ao nosso projeto para manter nossos arquivos de banco de SQL Server Express. Os dados de\_de aplicativo são um diretório especial no ASP.NET que já tem as permissões de acesso de segurança corretas para acesso ao banco de dado. No menu projeto, selecione Adicionar pasta ASP.NET e, em seguida,\_dados do aplicativo.

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a>Criando uma cadeia de conexão no arquivo Web. config

Adicionaremos algumas linhas ao arquivo de configuração do site para que Entity Framework saiba como se conectar ao nosso banco de dados. Clique duas vezes no arquivo Web. config localizado na raiz do projeto.

![](mvc-music-store-part-4/_static/image2.png)

Role até a parte inferior deste arquivo e adicione um &lt;connectionStrings&gt; seção diretamente acima da última linha, conforme mostrado abaixo.

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a>Adicionando uma classe de contexto

Clique com o botão direito do mouse na pasta modelos e adicione uma nova classe chamada MusicStoreEntities.cs.

![](mvc-music-store-part-4/_static/image3.png)

Essa classe representará o contexto de banco de dados Entity Framework e tratará nossas operações de criação, leitura, atualização e exclusão para nós. O código para essa classe é mostrado abaixo.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

Isso é isso – não há outra configuração, interfaces especiais, etc. Ao estender a classe base DbContext, nossa classe MusicStoreEntities é capaz de lidar com nossas operações de banco de dados para nós. Agora que temos sido conectados, vamos adicionar mais algumas propriedades às nossas classes de modelo para aproveitar algumas das informações adicionais em nosso banco de dados.

### <a name="adding-our-store-catalog-data"></a>Adicionando os dados do catálogo da loja

Aproveitaremos um recurso no Entity Framework que adiciona dados de "semente" a um banco de dado recém-criado. Isso preencherá previamente nosso catálogo de lojas com uma lista de gêneros, artistas e álbuns. O download de MvcMusicStore-Assets. zip-que incluía nossos arquivos de design de site usados anteriormente neste tutorial – tem um arquivo de classe com esses dados de semente, localizado em uma pasta chamada Code.

Na pasta Code/Models, localize o arquivo SampleData.cs e solte-o na pasta modelos em nosso projeto, conforme mostrado abaixo.

![](mvc-music-store-part-4/_static/image4.png)

Agora precisamos adicionar uma linha de código para informar Entity Framework sobre essa classe SampleData. Clique duas vezes no arquivo global. asax na raiz do projeto para abri-lo e adicione a seguinte linha à parte superior do método de início do aplicativo\_.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

Neste ponto, concluimos o trabalho necessário para configurar Entity Framework para o nosso projeto.

## <a name="querying-the-database"></a>Consultando o banco de dados

Agora, vamos atualizar nosso StoreController para que, em vez de usar "dados fictícios", em vez disso, ele chame nosso banco de dados para consultar todas as suas informações. Vamos começar declarando um campo no **StoreController** para manter uma instância da classe MusicStoreEntities, chamada storeDB:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a>Atualizando o índice de repositório para consultar o banco de dados

A classe MusicStoreEntities é mantida pelo Entity Framework e expõe uma propriedade de coleção para cada tabela em nosso banco de dados. Vamos atualizar nossa ação de índice do StoreController para recuperar todos os gêneros em nosso banco de dados. Anteriormente, fizemos isso por dados de cadeia de caracteres embutidos em código. Agora, podemos simplesmente usar a coleção de Generes de contexto de Entity Framework:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

Nenhuma alteração precisa acontecer ao nosso modelo de exibição, já que ainda estamos retornando o mesmo StoreIndexViewModel que retornamos antes-estamos apenas retornando dados dinâmicos do nosso banco de dado agora.

Quando executarmos o projeto novamente e visitar a URL "/Store", agora veremos uma lista de todos os gêneros em nosso banco de dados:

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a>Atualizando a procura e os detalhes do repositório para usar dados dinâmicos

Com o método de ação/Store/Browse? gênero = *[some-gênero]* , Estamos pesquisando um gênero por nome. Esperamos apenas um resultado, já que não devemos ter duas entradas para o mesmo nome de gênero e, portanto, podemos usar o. Extensão single () em LINQ para consultar o objeto de gênero apropriado como este (não digite isso ainda):

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

O método único usa uma expressão lambda como um parâmetro, que especifica que queremos um único objeto de gênero, de modo que seu nome corresponda ao valor que definimos. No caso acima, estamos carregando um único objeto de gênero com um valor de nome correspondente ao disco.

Aproveitaremos um recurso Entity Framework que nos permite indicar outras entidades relacionadas que desejamos carregar também quando o objeto gênero for recuperado. Esse recurso é chamado de modelagem de resultados de consulta e nos permite reduzir o número de vezes que precisamos acessar o banco de dados para recuperar todas as informações necessárias. Queremos buscar previamente os álbuns do gênero que recuperamos, portanto, atualizaremos nossa consulta para incluir de gêneros. include ("álbuns") para indicar que também queremos álbuns relacionados. Isso é mais eficiente, já que ele recuperará nossos dados de gênero e de álbum em uma única solicitação de banco de dado.

Com as explicações desatualizadas, veja como é a nossa ação atualizada do controlador de procura:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

Agora podemos atualizar a exibição de navegação da loja para exibir os álbuns que estão disponíveis em cada gênero. Abra o modelo de exibição (encontrado em/Views/Store/Browse.cshtml) e adicione uma lista com marcadores de álbuns, conforme mostrado abaixo.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

Executar nosso aplicativo e navegar até/Store/Browse? gênero = jazz mostra que nossos resultados agora estão sendo extraídos do banco de dados, exibindo todos os álbuns em nosso gênero selecionado.

![](mvc-music-store-part-4/_static/image2.jpg)

Faremos a mesma alteração em nossa URL de/Store/Details/[ID] e substituíremos nossos dados fictícios por uma consulta de banco que carrega um álbum cuja ID corresponde ao valor do parâmetro.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

Executar nosso aplicativo e navegar até/Store/Details/1 mostra que nossos resultados agora estão sendo extraídos do banco de dados.

![](mvc-music-store-part-4/_static/image5.png)

Agora que a página de detalhes da loja está configurada para exibir um álbum pela ID do álbum, vamos atualizar o modo de exibição de **navegação** para vincular ao modo de exibição de detalhes. Usaremos HTML. ActionLink, exatamente como fizemos para vincular do índice de repositório ao repositório de navegação no final da seção anterior. A origem completa do modo de exibição de navegação aparece abaixo.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

Agora podemos navegar de nossa página de armazenamento para uma página de gênero, que lista os álbuns disponíveis e, ao clicar em um álbum, podemos exibir detalhes para esse álbum.

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-3.md)
> [Próximo](mvc-music-store-part-5.md)
