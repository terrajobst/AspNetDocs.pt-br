---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: Criar um banco de dados | Microsoft Docs
author: microsoft
description: A etapa 2 mostra as etapas para criar o banco de dados que contém todo o jantar e o RSVP para nosso aplicativo NerdDinner.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: b0aa7c8cdf741f44e09ed18e2b2f73fe6bf786ae
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78581000"
---
# <a name="create-a-database"></a>Criar um banco de dados

pela [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Esta é a etapa 2 de um [tutorial de aplicativo "NerdDinner"](introducing-the-nerddinner-tutorial.md) gratuito que percorre como criar um aplicativo Web pequeno, mas completo usando o ASP.NET MVC 1.
> 
> A etapa 2 mostra as etapas para criar o banco de dados que contém todo o jantar e o RSVP para nosso aplicativo NerdDinner.
> 
> Se você estiver usando o ASP.NET MVC 3, recomendamos seguir as [introdução com](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) os tutoriais da [loja de música](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) MVC 3 ou MVC.

## <a name="nerddinner-step-2-creating-the-database"></a>Etapa 2 do NerdDinner: criando o banco de dados

Vamos usar um banco de dados para armazenar todo o jantar e o RSVP para nosso aplicativo NerdDinner.

As etapas a seguir mostram como criar o banco de dados usando a edição gratuita do SQL Server Express (que você pode facilmente instalar usando a v2 do [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)). Todo o código que escreveremos funcionará com SQL Server Express e o SQL Server completo.

### <a name="creating-a-new-sql-server-express-database"></a>Criando um novo banco de dados SQL Server Express

Começaremos clicando com o botão direito do mouse em nosso projeto Web e selecionaremos o comando de menu **adicionar&gt;novo item** :

![](create-a-database/_static/image1.png)

Isso abrirá a caixa de diálogo "Adicionar novo item" do Visual Studio. Vamos filtrar pela categoria "data" e selecionar o modelo de item "SQL Server banco de dados":

![](create-a-database/_static/image2.png)

Vamos nomear o banco de dados SQL Server Express que queremos criar "NerdDinner. MDF" e clicar em OK. O Visual Studio nos perguntará se queremos adicionar esse arquivo ao nosso diretório de dados de\_de \App (que é um diretório já configurado com ACLs de segurança de leitura e gravação):

![](create-a-database/_static/image3.png)

Clicaremos em "Sim" e nosso novo banco de dados será criado e adicionado ao nosso Gerenciador de Soluções:

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a>Criando tabelas dentro de nosso banco de dados

Agora temos um novo banco de dados vazio. Vamos adicionar algumas tabelas a ele.

Para fazer isso, navegaremos até a janela de guia "Gerenciador de Servidores" no Visual Studio, que nos permite gerenciar bancos de dados e servidores. SQL Server Express bancos de dados armazenados na pasta \App\_data do nosso aplicativo aparecerão automaticamente dentro do Gerenciador de Servidores. Opcionalmente, podemos usar o ícone "conectar ao banco de dados" na parte superior da janela "Gerenciador de Servidores" para adicionar mais bancos de dados SQL Server (locais e remotos) à lista também:

![](create-a-database/_static/image5.png)

Adicionaremos duas tabelas ao nosso banco de dados NerdDinner – uma para armazenar nossos jantares e a outra para rastrear as aceitação de RSVP para eles. Podemos criar novas tabelas clicando com o botão direito do mouse na pasta "tabelas" em nosso banco de dados e escolhendo o comando de menu "Adicionar nova tabela":

![](create-a-database/_static/image6.png)

Isso abrirá um designer de tabela que nos permite configurar o esquema da nossa tabela. Para a nossa tabela de "JANTARS", adicionaremos 10 colunas de dados:

![](create-a-database/_static/image7.png)

Queremos que a coluna "Jantarid" seja uma chave primária exclusiva para a tabela. Podemos configurar isso clicando com o botão direito do mouse na coluna "Jantarid" e escolhendo o item de menu "definir chave primária":

![](create-a-database/_static/image8.png)

Além de tornar o Jantarid uma chave primária, também queremos configurá-lo como uma coluna de "identidade" cujo valor seja incrementado automaticamente à medida que novas linhas de dados são adicionadas à tabela (o que significa que a primeira linha de jantar inserida terá um Jantarid de 1, a segunda linha inserida terá um Jantarid de 2, etc.).

Podemos fazer isso selecionando a coluna "Jantarid" e, em seguida, usar o editor "Propriedades da coluna" para definir a propriedade "(is Identity)" na coluna como "Yes". Usaremos os padrões de identidade padrão (comece com 1 e incrementos 1 em cada nova linha de jantar):

![](create-a-database/_static/image9.png)

Em seguida, salvaremos nossa tabela digitando Ctrl-S ou usando o comando **File-&gt;Save** menu. Isso nos solicitará nomear a tabela. Vamos nomeá-lo como "jantares":

![](create-a-database/_static/image10.png)

Nossa nova tabela de jantares será exibida em nosso banco de dados no Gerenciador de servidores.

Em seguida, repetiremos as etapas acima e criaremos uma tabela "RSVP". Esta tabela com três colunas. Iremos configurar a coluna Rsvpid como a chave primária e também torná-la uma coluna de identidade:

![](create-a-database/_static/image11.png)

Vamos salvá-lo e dar a ele o nome "RSVP".

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a>Configurando uma relação de chave estrangeira entre tabelas

Agora temos duas tabelas em nosso banco de dados. Nossa última etapa de design de esquema será configurar uma relação de "um para muitos" entre essas duas tabelas – para que possamos associar cada linha de jantar com zero ou mais linhas RSVP que se aplicam a ela. Vamos fazer isso Configurando a coluna "Jantarid" da tabela RSVP para ter uma relação de chave estrangeira com a coluna "Jantarid" na tabela "jantares".

Para fazer isso, vamos abrir a tabela RSVP dentro do designer de tabela clicando duas vezes nela no Gerenciador de servidores. Em seguida, selecionaremos a coluna "Jantarid" dentro dela, clicamos com o botão direito do mouse e escolhemos as opções "relações..." comando de menu de contexto:

![](create-a-database/_static/image12.png)

Isso abrirá uma caixa de diálogo que podemos usar para configurar relações entre tabelas:

![](create-a-database/_static/image13.png)

Clicaremos no botão "Adicionar" para adicionar uma nova relação à caixa de diálogo. Depois que uma relação tiver sido adicionada, expandiremos o nó "especificação de tabelas e colunas" da exibição de árvore na grade de propriedades à direita da caixa de diálogo e, em seguida, clicaremos no botão "..." botão à direita dele:

![](create-a-database/_static/image14.png)

Clicando no botão "..." abrirá outra caixa de diálogo que nos permite especificar quais tabelas e colunas estão envolvidas na relação, bem como nos permitir nomear a relação.

Alteraremos a tabela de chaves primárias para "jantares" e selecionaremos a coluna "Jantarid" na tabela de jantares como a chave primária. Nossa tabela RSVP será a tabela de chave estrangeira e o RSVP. A coluna do jantarid será associada como chave estrangeira:

![](create-a-database/_static/image15.png)

Agora, cada linha na tabela RSVP será associada a uma linha na tabela de jantar. SQL Server manterá a integridade referencial para nós – e nos impedirá de adicionar uma nova linha RSVP se ela não apontar para uma linha de jantar válida. Ele também nos impedirá de excluir uma linha de jantar se ainda houver linhas RSVP fazendo referência a ela.

### <a name="adding-data-to-our-tables"></a>Adicionando dados a nossas tabelas

Vamos concluir adicionando alguns dados de exemplo à nossa tabela de jantares. Podemos adicionar dados a uma tabela clicando com o botão direito do mouse nele dentro do Gerenciador de Servidores e escolhendo o comando "mostrar dados da tabela":

![](create-a-database/_static/image16.png)

Adicionaremos algumas linhas de dados de jantar que podemos usar mais tarde, já que começamos a implementar o aplicativo:

![](create-a-database/_static/image17.png)

### <a name="next-step"></a>Próxima etapa

Terminamos de criar nosso banco de dados. Agora, vamos criar classes de modelo que podemos usar para consultá-la e atualizá-la.

> [!div class="step-by-step"]
> [Anterior](create-a-new-aspnet-mvc-project.md)
> [Próximo](build-a-model-with-business-rule-validations.md)
