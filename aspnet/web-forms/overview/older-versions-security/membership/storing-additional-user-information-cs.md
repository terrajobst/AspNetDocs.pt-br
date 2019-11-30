---
uid: web-forms/overview/older-versions-security/membership/storing-additional-user-information-cs
title: Armazenando informações adicionais doC#usuário () | Microsoft Docs
author: rick-anderson
description: Neste tutorial, responderemos a essa pergunta criando um aplicativo de livro de visitas muito rudimentar. Ao fazer isso, veremos opções diferentes para modeli...
ms.author: riande
ms.date: 01/18/2008
ms.assetid: 1642132a-1ca5-4872-983f-ab59fc8865d3
msc.legacyurl: /web-forms/overview/older-versions-security/membership/storing-additional-user-information-cs
msc.type: authoredcontent
ms.openlocfilehash: 24b96e86bc93e03d2639b73e35ed1fd1271bac5a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74641007"
---
# <a name="storing-additional-user-information-c"></a>Armazenar informações de usuário adicionais (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_08_CS.zip) ou [baixar PDF](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial08_ExtraUserInfo_cs.pdf)

> Neste tutorial, responderemos a essa pergunta criando um aplicativo de livro de visitas muito rudimentar. Ao fazer isso, veremos opções diferentes para modelar as informações do usuário em um banco de dados e, em seguida, veremos como associá-los às contas de usuário criadas pela estrutura de associação.

## <a name="introduction"></a>Introdução

ASP. A estrutura de associação do NET oferece uma interface flexível para gerenciar usuários. A API Membership inclui métodos para validar credenciais, recuperar informações sobre o usuário conectado no momento, criar uma nova conta de usuário e excluir uma conta de usuário, entre outras. Cada conta de usuário na estrutura de associação contém apenas as propriedades necessárias para validar as credenciais e executar tarefas essenciais relacionadas à conta de usuário. Isso é evidenciado pelos métodos e propriedades da [classe`MembershipUser`](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx), que modela uma conta de usuário na estrutura de associação. Essa classe tem propriedades como [`UserName`](https://msdn.microsoft.com/library/system.web.security.membershipuser.username.aspx), [`Email`](https://msdn.microsoft.com/library/system.web.security.membershipuser.email.aspx)e [`IsLockedOut`](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx), e métodos como [`GetPassword`](https://msdn.microsoft.com/library/system.web.security.membershipuser.getpassword.aspx) e [`UnlockUser`](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx).

Muitas vezes, os aplicativos precisam armazenar informações adicionais do usuário não incluídas na estrutura de associação. Por exemplo, um varejista online pode precisar permitir que cada usuário armazene seus endereços de remessa e cobrança, as informações de pagamento, as preferências de entrega e o número de telefone de contato. Além disso, cada pedido no sistema é associado a uma conta de usuário específica.

A classe `MembershipUser` não inclui propriedades como `PhoneNumber` ou `DeliveryPreferences` ou `PastOrders`. Então, como rastrear as informações do usuário necessárias para o aplicativo e fazer com que ele se integre à estrutura de associação? Neste tutorial, responderemos a essa pergunta criando um aplicativo de livro de visitas muito rudimentar. Ao fazer isso, veremos opções diferentes para modelar as informações do usuário em um banco de dados e, em seguida, veremos como associá-los às contas de usuário criadas pela estrutura de associação. Vamos começar!

## <a name="step-1-creating-the-guestbook-applications-data-model"></a>Etapa 1: criando o modelo de dados do aplicativo de livro de visitas

Há uma variedade de técnicas que podem ser empregadas para capturar informações do usuário em um banco de dados e associá-las às contas de usuário criadas pela estrutura de associação. Para ilustrar essas técnicas, precisaremos aumentar o tutorial do aplicativo Web para que ele Capture algum tipo de dados relacionados ao usuário. (Atualmente, o modelo de dados do aplicativo contém apenas as tabelas de serviços de aplicativo necessárias para o `SqlMembershipProvider`.)

Vamos criar um aplicativo de livro de visitas muito simples, em que um usuário autenticado pode deixar um comentário. Além de armazenar comentários do livro de visitas, vamos permitir que cada usuário armazene seu principal cidade, Home Page e assinatura. Se fornecido, a Home cidade, a Home Page e a assinatura do usuário aparecerão em cada mensagem que ele saiu no livro de visitas.

### <a name="adding-theguestbookcommentstable"></a>Adicionando a tabela de`GuestbookComments`

Para capturar os comentários do livro de visitas, precisamos criar uma tabela de banco de dados denominada `GuestbookComments` que tenha colunas como `CommentId`, `Subject`, `Body`e `CommentDate`. Também precisamos que cada registro na tabela de `GuestbookComments` faça referência ao usuário que saiu do comentário.

Para adicionar essa tabela ao nosso banco de dados, vá para o Gerenciador de Banco de Dados no Visual Studio e faça uma busca detalhada no banco de dados `SecurityTutorials`. Clique com o botão direito do mouse na pasta tabelas e escolha Adicionar nova tabela. Isso abre uma interface que nos permite definir as colunas para a nova tabela.

[![adicionar uma nova tabela ao banco de dados SecurityTutorials](storing-additional-user-information-cs/_static/image2.png)](storing-additional-user-information-cs/_static/image1.png)

**Figura 1**: adicionar uma nova tabela ao banco de dados de `SecurityTutorials` ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-cs/_static/image3.png))

Em seguida, defina as colunas do `GuestbookComments`. Comece adicionando uma coluna chamada `CommentId` do tipo `uniqueidentifier`. Essa coluna identificará exclusivamente cada comentário no livro de visitas, portanto, não permita `NULL` s e marque-o como a chave primária da tabela. Em vez de fornecer um valor para o campo de `CommentId` em cada `INSERT`, podemos indicar que um novo valor de `uniqueidentifier` deve ser gerado automaticamente para esse campo em `INSERT`, definindo o valor padrão da coluna como `NEWID()`. Depois de adicionar esse primeiro campo, marcá-lo como a chave primária e definir seu valor padrão, sua tela deverá ser semelhante à captura de tela mostrada na Figura 2.

[![adicionar uma coluna primária denominada commentId](storing-additional-user-information-cs/_static/image5.png)](storing-additional-user-information-cs/_static/image4.png)

**Figura 2**: adicionar uma coluna primária denominada `CommentId` ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-cs/_static/image6.png))

Em seguida, adicione uma coluna denominada `Subject` do tipo `nvarchar(50)` e uma coluna denominada `Body` do tipo `nvarchar(MAX)`, desautorizando `NULL` s em ambas as colunas. Depois disso, adicione uma coluna chamada `CommentDate` do tipo `datetime`. Não permita `NULL` s e defina o valor padrão da coluna de `CommentDate` como `getdate()`.

Tudo o que resta é adicionar uma coluna que associa uma conta de usuário a cada comentário do livro de visitas. Uma opção seria adicionar uma coluna chamada `UserName` do tipo `nvarchar(256)`. Essa é uma opção adequada ao usar um provedor de associação diferente do `SqlMembershipProvider`. Mas ao usar o `SqlMembershipProvider`, como estamos nesta série de tutoriais, a coluna `UserName` na tabela `aspnet_Users` não é garantida como exclusiva. A chave primária da tabela `aspnet_Users` é `UserId` e é do tipo `uniqueidentifier`. Portanto, a tabela `GuestbookComments` precisa de uma coluna chamada `UserId` do tipo `uniqueidentifier` (não permitir valores de `NULL`). Vá em frente e adicione esta coluna.

> [!NOTE]
> Como discutimos no tutorial [*criando o esquema de associação no SQL Server*](creating-the-membership-schema-in-sql-server-cs.md) , a estrutura de associação foi projetada para permitir que vários aplicativos Web com diferentes contas de usuário compartilhem o mesmo repositório de usuários. Ele faz isso Particionando as contas de usuário em diferentes aplicativos. E, embora cada nome de usuário seja garantido como exclusivo em um aplicativo, o mesmo nome de usuário pode ser usado em aplicativos diferentes usando o mesmo armazenamento de usuário. Há uma restrição de `UNIQUE` de composição na tabela `aspnet_Users` nos campos `UserName` e `ApplicationId`, mas não uma apenas no campo `UserName`. Consequentemente, é possível que a tabela de usuários\_do ASPNET tenha dois (ou mais) registros com o mesmo valor de `UserName`. Há, no entanto, uma restrição de `UNIQUE` no campo de `UserId` da tabela de `aspnet_Users` (já que é a chave primária). Uma restrição `UNIQUE` é importante porque, sem ela, não podemos estabelecer uma restrição FOREIGN KEY entre as tabelas `GuestbookComments` e `aspnet_Users`.

Depois de adicionar a coluna `UserId`, salve a tabela clicando no ícone salvar na barra de ferramentas. Nomeie a nova tabela `GuestbookComments`.

Temos uma última questão para participar com a tabela `GuestbookComments`: precisamos criar uma [restrição FOREIGN KEY](https://msdn.microsoft.com/library/ms175464.aspx) entre a coluna `GuestbookComments.UserId` e a coluna `aspnet_Users.UserId`. Para conseguir isso, clique no ícone de relação na barra de ferramentas para iniciar a caixa de diálogo relações de chave estrangeira. (Como alternativa, você pode iniciar essa caixa de diálogo acessando o menu Designer de Tabela e escolhendo relações.)

Clique no botão Adicionar no canto inferior esquerdo da caixa de diálogo relações de chave estrangeira. Isso adicionará uma nova restrição FOREIGN KEY, embora ainda precisemos definir as tabelas que participam da relação.

[![usar a caixa de diálogo relações de chave estrangeira para gerenciar as restrições de chave estrangeira de uma tabela](storing-additional-user-information-cs/_static/image8.png)](storing-additional-user-information-cs/_static/image7.png)

**Figura 3**: usar a caixa de diálogo relações de chave estrangeira para gerenciar as restrições de chave estrangeira de uma tabela ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-cs/_static/image9.png))

Em seguida, clique no ícone de reticências na linha "especificações de tabela e colunas" à direita. Isso iniciará a caixa de diálogo tabelas e colunas, na qual podemos especificar a tabela e a coluna de chave primária e a coluna de chave estrangeira da tabela `GuestbookComments`. Em particular, selecione `aspnet_Users` e `UserId` como a tabela de chave primária e a coluna e `UserId` da tabela `GuestbookComments` como a coluna de chave estrangeira (consulte a Figura 4). Depois de definir as tabelas e as colunas de chave estrangeira e primária, clique em OK para retornar à caixa de diálogo relações de chave estrangeira.

[![estabelecer uma restrição FOREIGN KEY entre as tabelas aspnet_Users e GuesbookComments](storing-additional-user-information-cs/_static/image11.png)](storing-additional-user-information-cs/_static/image10.png)

**Figura 4**: estabelecer uma restrição de chave estrangeira entre as tabelas `aspnet_Users` e `GuesbookComments` ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-cs/_static/image12.png))

Neste ponto, a restrição FOREIGN KEY foi estabelecida. A presença dessa restrição garante a [integridade relacional](http://en.wikipedia.org/wiki/Referential_integrity) entre as duas tabelas, garantindo que nunca haverá uma entrada de livro de visitas referente a uma conta de usuário inexistente. Por padrão, uma restrição FOREIGN KEY não permitirá que um registro pai seja excluído se houver registros filho correspondentes. Ou seja, se um usuário fizer um ou mais comentários do livro de visitas e tentar excluir essa conta de usuário, a exclusão falhará, a menos que seus comentários do livro de visitas sejam excluídos primeiro.

As restrições FOREIGN KEY podem ser configuradas para excluir automaticamente os registros filho associados quando um registro pai é excluído. Em outras palavras, podemos configurar essa restrição de chave estrangeira para que as entradas do livro de visitas de um usuário sejam excluídas automaticamente quando a conta de usuário for excluída. Para fazer isso, expanda a seção "especificação de inserção e atualização" e defina a propriedade "excluir regra" como em cascata.

[![configurar a restrição FOREIGN KEY para as exclusões em cascata](storing-additional-user-information-cs/_static/image14.png)](storing-additional-user-information-cs/_static/image13.png)

**Figura 5**: configurar a restrição de chave estrangeira para as exclusões em cascata ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-cs/_static/image15.png))

Para salvar a restrição FOREIGN KEY, clique no botão fechar para sair das relações de chave estrangeira. Em seguida, clique no ícone salvar na barra de ferramentas para salvar a tabela e essa relação.

### <a name="storing-the-users-home-town-homepage-and-signature"></a>Armazenando a Home Page, a Home Page e a assinatura do usuário

A tabela `GuestbookComments` ilustra como armazenar informações que compartilham uma relação um-para-muitos com contas de usuário. Como cada conta de usuário pode ter um número arbitrário de comentários associados, essa relação é modelada criando uma tabela para conter o conjunto de comentários que inclui uma coluna que vincula de volta cada comentário a um usuário específico. Ao usar o `SqlMembershipProvider`, esse link é melhor estabelecido criando uma coluna chamada `UserId` do tipo `uniqueidentifier` e uma restrição FOREIGN KEY entre essa coluna e `aspnet_Users.UserId`.

Agora, precisamos associar três colunas a cada conta de usuário para armazenar a cidade, a Home Page e a assinatura do usuário, que aparecerão em seus comentários do livro de visitas. Há várias maneiras diferentes de fazer isso:

- <strong>Adicione novas colunas às</strong> tabelas<strong>`aspnet_Users`</strong> <strong>ou</strong> <strong>`aspnet_Membership`</strong> <strong>.</strong> Eu não recomendaria essa abordagem porque ela modifica o esquema usado pelo `SqlMembershipProvider`. Essa decisão pode voltar a contra-lo. Por exemplo, e se uma versão futura do ASP.NET usar um esquema de `SqlMembershipProvider` diferente. A Microsoft pode incluir uma ferramenta para migrar os dados de `SqlMembershipProvider` do ASP.NET 2,0 para o novo esquema, mas se você tiver modificado o esquema de `SqlMembershipProvider` do ASP.NET 2,0, essa conversão poderá não ser possível.

- **Use ASP. Estrutura de perfil da rede, definindo uma propriedade de perfil para a cidade principal, Home Page e assinatura.** O ASP.NET inclui uma estrutura de perfil projetada para armazenar dados adicionais específicos do usuário. Como a estrutura de associação, a estrutura de perfil é criada sobre o modelo de provedor. O .NET Framework é fornecido com um `SqlProfileProvider` sthat armazena dados de perfil em um banco de SQL Server. Na verdade, nosso banco de dados já tem a tabela usada pelo `SqlProfileProvider` (`aspnet_Profile`), pois ele foi adicionado quando adicionamos os serviços de aplicativo novamente <a id="_msoanchor_2"> </a>no tutorial [*criando o esquema de associação no SQL Server*](creating-the-membership-schema-in-sql-server-cs.md) .   
  O principal benefício da estrutura de perfil é que ele permite que os desenvolvedores definam as propriedades de perfil no `Web.config` – nenhum código precisa ser escrito para serializar os dados de perfil de e para o armazenamento de dados subjacente. Em suma, é incrivelmente fácil definir um conjunto de propriedades de perfil e trabalhar com eles no código. No entanto, o sistema de perfis deixa muito a desejar quando se trata do controle de versão, portanto, se você tiver um aplicativo em que você espera que novas propriedades específicas do usuário sejam adicionadas posteriormente, ou que as existentes sejam removidas ou modificadas, a estrutura do perfil poderá não ser a  melhor opção. Além disso, a `SqlProfileProvider` armazena as propriedades de perfil de uma maneira altamente desnormalizada, tornando-a próxima de impossível executar consultas diretamente nos dados de perfil (como quantos usuários têm uma cidade de Nova York).   
  Para obter mais informações sobre a estrutura de perfil, consulte a seção "leituras adicionais" no final deste tutorial.

- <strong>Adicione essas três colunas a uma nova tabela no banco de dados e estabeleça uma relação um-para-um entre essa tabela e</strong> <strong>`aspnet_Users`</strong> <strong>.</strong> Essa abordagem envolve um pouco mais de trabalho do que com a estrutura de perfil, mas oferece flexibilidade máxima em como as propriedades de usuário adicionais são modeladas no banco de dados. Essa é a opção que usaremos neste tutorial.

Criaremos uma nova tabela chamada `UserProfiles` para salvar a cidade, a Home Page e a assinatura de cada usuário. Clique com o botão direito do mouse na pasta tabelas na janela Gerenciador de Banco de Dados e escolha criar uma nova tabela. Nomeie a primeira coluna `UserId` e defina seu tipo como `uniqueidentifier`. Não permita `NULL` valores e marque a coluna como uma chave primária. Em seguida, adicione colunas nomeadas: `HomeTown` do tipo `nvarchar(50)`; `HomepageUrl` do tipo `nvarchar(100)`; e a assinatura do tipo `nvarchar(500)`. Cada uma dessas três colunas pode aceitar um valor `NULL`.

[![criar a tabela userperfis](storing-additional-user-information-cs/_static/image17.png)](storing-additional-user-information-cs/_static/image16.png)

**Figura 6**: criar a tabela de `UserProfiles` ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-cs/_static/image18.png))

Salve a tabela e nomeie-a `UserProfiles`. Por fim, estabeleça uma restrição de chave estrangeira entre o campo `UserId` da tabela de `UserProfiles` e o campo `aspnet_Users.UserId`. Como fizemos com a restrição FOREIGN KEY entre as tabelas `GuestbookComments` e `aspnet_Users`, essa restrição exclui a cascata. Como o campo `UserId` em `UserProfiles` é a chave primária, isso garante que não haverá mais de um registro na tabela `UserProfiles` para cada conta de usuário. Esse tipo de relação é conhecido como um-para-um.

Agora que temos o modelo de dados criado, estamos prontos para usá-lo. Nas etapas 2 e 3, veremos como o usuário conectado no momento pode exibir e editar suas informações da Home Page, da Home Page e da assinatura. Na etapa 4, criaremos a interface para usuários autenticados para enviar novos comentários para o livro de visitas e exibir os existentes.

## <a name="step-2-displaying-the-users-home-town-homepage-and-signature"></a>Etapa 2: exibindo a cidade, a Home Page e a assinatura do usuário

Há várias maneiras de permitir que o usuário conectado no momento exiba e edite seu principal cidade, Home Page e informações de assinatura. Poderíamos criar manualmente a interface do usuário com controles TextBox e Label, ou podemos usar um dos controles da Web de dados, como o controle DetailsView. Para executar as instruções `SELECT` do banco de dados e `UPDATE` poderíamos escrever código ADO.NET em nossa classe code-behind da página ou, como alternativa, empregar uma abordagem declarativa com o SqlDataSource. O ideal é que nosso aplicativo contenha uma arquitetura em camadas, que poderíamos invocar programaticamente da classe code-behind da página ou declarativamente por meio do controle ObjectDataSource.

Como esta série de tutoriais se concentra em autenticação de formulários, autorização, contas de usuário e funções, não haverá uma discussão completa dessas diferentes opções de acesso a dados ou por que uma arquitetura em camadas é preferida em relação à execução de instruções SQL diretamente na página ASP.NET. Vou percorrer usando um DetailsView e SqlDataSource – a opção mais rápida e fácil – mas os conceitos discutidos podem certamente ser aplicados a controles da Web alternativos e à lógica de acesso a dados. Para obter mais informações sobre como trabalhar com dados no ASP.NET, consulte meus *[dados de trabalho em série de tutoriais do ASP.NET 2,0](../../data-access/index.md)* .

Abra a página `AdditionalUserInfo.aspx` na pasta `Membership` e adicione um controle DetailsView à página, definindo sua propriedade `ID` como `UserProfile` e desmarcando suas propriedades `Width` e `Height`. Expanda a marca inteligente de DetailsView e escolha associá-la a um novo controle da fonte de dados. Isso iniciará o assistente de configuração de fonte de fontes (veja a Figura 7). A primeira etapa solicita que você especifique o tipo de fonte de dados. Como vamos nos conectar diretamente ao banco de dados de `SecurityTutorials`, escolha o ícone de banco de dados, especificando o `ID` como `UserProfileDataSource`.

[![adicionar um novo controle SqlDataSource chamado UserProfileDataSource](storing-additional-user-information-cs/_static/image20.png)](storing-additional-user-information-cs/_static/image19.png)

**Figura 7**: adicionar um novo controle SqlDataSource chamado `UserProfileDataSource` ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-cs/_static/image21.png))

A próxima tela solicita o banco de dados a ser usado. Já definimos uma cadeia de conexão em `Web.config` para o banco de dados `SecurityTutorials`. Esse nome de cadeia de conexão – `SecurityTutorialsConnectionString` – deve estar na lista suspensa. Selecione essa opção e clique em Avançar.

[![escolher SecurityTutorialsConnectionString na lista suspensa](storing-additional-user-information-cs/_static/image23.png)](storing-additional-user-information-cs/_static/image22.png)

**Figura 8**: escolha `SecurityTutorialsConnectionString` na lista suspensa ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-cs/_static/image24.png))

A tela subsequente nos pede para especificar a tabela e as colunas a serem consultadas. Escolha a tabela `UserProfiles` na lista suspensa e verifique todas as colunas.

[![retornar todas as colunas da tabela userperfis](storing-additional-user-information-cs/_static/image26.png)](storing-additional-user-information-cs/_static/image25.png)

**Figura 9**: retornar todas as colunas da tabela de `UserProfiles` ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-cs/_static/image27.png))

A consulta atual na Figura 9 retorna *todos* os registros em `UserProfiles`, mas estamos interessados apenas no registro do usuário conectado no momento. Para adicionar uma cláusula `WHERE`, clique no botão `WHERE` para abrir a caixa de diálogo Adicionar cláusula `WHERE` (consulte a Figura 10). Aqui você pode selecionar a coluna para filtrar, o operador e a origem do parâmetro de filtro. Selecione `UserId` como a coluna e "=" como o operador.

Infelizmente, não há nenhuma origem de parâmetro interna para retornar o valor de `UserId` do usuário conectado no momento. Precisaremos obter esse valor programaticamente. Portanto, defina a lista suspensa origem como "nenhum", clique no botão Adicionar para adicionar o parâmetro e clique em OK.

[![adicionar um parâmetro de filtro na coluna UserId](storing-additional-user-information-cs/_static/image29.png)](storing-additional-user-information-cs/_static/image28.png)

**Figura 10**: adicionar um parâmetro de filtro na coluna `UserId` ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-cs/_static/image30.png))

Depois de clicar em OK, você será retornado para a tela mostrada na Figura 9. Desta vez, no entanto, a consulta SQL na parte inferior da tela deve incluir uma cláusula `WHERE`. Clique em avançar para passar para a tela "consulta de teste". Aqui você pode executar a consulta e ver os resultados. Clique em Concluir para concluir o assistente.

Após a conclusão do assistente de configuração de fonte de fontes, o Visual Studio cria o controle SqlDataSource com base nas configurações especificadas no assistente. Além disso, ele adiciona manualmente os BoundFields ao DetailsView para cada coluna retornada pelo `SelectCommand`do SqlDataSource. Não é necessário mostrar o campo `UserId` no DetailsView, já que o usuário não precisa saber esse valor. Você pode remover esse campo diretamente da marcação declarativa do controle DetailsView ou clicando no link "editar campos" em sua marca inteligente.

Neste ponto, a marcação declarativa de sua página deve ser semelhante ao seguinte:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample1.aspx)]

Precisamos definir programaticamente o parâmetro `UserId` do controle SqlDataSource para o `UserId` do usuário conectado no momento antes de os dados serem selecionados. Isso pode ser feito criando um manipulador de eventos para o evento de `Selecting` do SqlDataSource e adicionando o código a seguir:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample2.cs)]

O código acima começa obtendo uma referência ao usuário conectado no momento chamando o método `GetUser` da classe `Membership`. Isso retorna um objeto `MembershipUser`, cuja propriedade `ProviderUserKey` contém a `UserId`. O valor de `UserId` é então atribuído ao parâmetro de `@UserId` do SqlDataSource.

> [!NOTE]
> O método `Membership.GetUser()` retorna informações sobre o usuário conectado no momento. Se um usuário anônimo estiver visitando a página, ele retornará um valor de `null`. Nesse caso, isso resultará em um `NullReferenceException` na linha de código a seguir ao tentar ler a propriedade `ProviderUserKey`. É claro que não precisamos nos preocupar `Membership.GetUser()` retornar um valor de `null` na página `AdditionalUserInfo.aspx` porque configuramos a autorização de URL em um tutorial anterior para que somente usuários autenticados pudessem acessar os recursos de ASP.NET nessa pasta. Se você precisar acessar informações sobre o usuário conectado no momento em uma página em que o acesso anônimo é permitido, certifique-se de verificar se um objeto não`null MembershipUser` é retornado do método `GetUser()` antes de referenciar suas propriedades.

Se você visitar a página `AdditionalUserInfo.aspx` por meio de um navegador, verá uma página em branco, pois ainda adicionamos linhas à tabela `UserProfiles`. Na etapa 6, veremos como personalizar o controle CreateUserWizard para adicionar automaticamente uma nova linha à tabela `UserProfiles` quando uma nova conta de usuário é criada. Por enquanto, no entanto, precisaremos criar manualmente um registro na tabela.

Navegue até a Gerenciador de Banco de Dados no Visual Studio e expanda a pasta tabelas. Clique com o botão direito do mouse na tabela `aspnet_Users` e escolha "mostrar dados da tabela" para ver os registros na tabela; Faça a mesma coisa para a tabela de `UserProfiles`. A Figura 11 mostra esses resultados quando eles são posicionados à vertical. No meu banco de dados, atualmente há `aspnet_Users` registros para Bruce, Fred e Tito, mas sem registros na tabela `UserProfiles`.

[![o conteúdo das tabelas de aspnet_Users e userperfis é exibido](storing-additional-user-information-cs/_static/image32.png)](storing-additional-user-information-cs/_static/image31.png)

**Figura 11**: o conteúdo das tabelas `aspnet_Users` e `UserProfiles` é exibido ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-cs/_static/image33.png))

Adicione um novo registro à tabela `UserProfiles` digitando manualmente os valores para os campos `HomeTown`, `HomepageUrl`e `Signature`. A maneira mais fácil de obter um valor de `UserId` válido no novo registro de `UserProfiles` é selecionar o campo `UserId` de uma conta de usuário específica na tabela `aspnet_Users` e copiá-lo e colá-lo no campo `UserId` no `UserProfiles`. A Figura 12 mostra a tabela `UserProfiles` após a adição de um novo registro para Bruce.

[![um registro foi adicionado ao userperfiles para Bruce](storing-additional-user-information-cs/_static/image35.png)](storing-additional-user-information-cs/_static/image34.png)

**Figura 12**: um registro foi adicionado ao `UserProfiles` para Bruce ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-cs/_static/image36.png))

Volte para a página `AdditionalUserInfo.aspx`, conectado como Bruce. Como mostra a Figura 13, as configurações de Bruce são exibidas.

[![o usuário que está visitando atualmente é mostrado suas configurações](storing-additional-user-information-cs/_static/image38.png)](storing-additional-user-information-cs/_static/image37.png)

**Figura 13**: o usuário que está visitando atualmente mostra suas configurações ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-cs/_static/image39.png))

> [!NOTE]
> Vá em frente e adicione manualmente os registros na tabela `UserProfiles` para cada usuário da associação. Na etapa 6, veremos como personalizar o controle CreateUserWizard para adicionar automaticamente uma nova linha à tabela `UserProfiles` quando uma nova conta de usuário é criada.

## <a name="step-3-allowing-the-user-to-edit-his-home-town-homepage-and-signature"></a>Etapa 3: permitir que o usuário edite seu principal cidade, Home Page e assinatura

Neste ponto, o usuário conectado no momento pode exibir sua página inicial, Home Page e configuração de assinatura, mas eles ainda não podem modificá-los. Vamos atualizar o controle DetailsView para que os dados possam ser editados.

A primeira coisa que precisamos fazer é adicionar um `UpdateCommand` para o SqlDataSource, especificando a instrução `UPDATE` a ser executada e seus parâmetros correspondentes. Selecione o SqlDataSource e, no janela Propriedades, clique nas reticências ao lado da propriedade UpdateQuery para abrir a caixa de diálogo Editor de parâmetros e comandos. Insira a instrução `UPDATE` a seguir na caixa de texto:

[!code-sql[Main](storing-additional-user-information-cs/samples/sample3.sql)]

Em seguida, clique no botão "atualizar parâmetros", que criará um parâmetro na coleção de `UpdateParameters` do controle SqlDataSource para cada um dos parâmetros na instrução `UPDATE`. Deixe a origem de todos os parâmetros definida como nenhum e clique no botão OK para concluir a caixa de diálogo.

[![especificar o UpdateCommand e UpdateParameters do SqlDataSource](storing-additional-user-information-cs/_static/image41.png)](storing-additional-user-information-cs/_static/image40.png)

**Figura 14**: especificar o `UpdateCommand` e o `UpdateParameters` do SqlDataSource ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-cs/_static/image42.png))

Devido às adições que fizemos no controle SqlDataSource, o controle DetailsView agora pode dar suporte à edição. Na marca inteligente de DetailsView, marque a caixa de seleção "Habilitar edição". Isso adiciona um CommandField à coleção de `Fields` do controle com sua propriedade `ShowEditButton` definida como true. Isso renderiza um botão Editar quando o DetailsView é exibido no modo somente leitura e os botões atualizar e cancelar quando são exibidos no modo de edição. Em vez de exigir que o usuário clique em Editar, no entanto, podemos ter a renderização DetailsView em um estado "sempre editável" definindo a [propriedade`DefaultMode`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode.aspx) do controle DetailsView como `Edit`.

Com essas alterações, a marcação declarativa do controle DetailsView deve ser semelhante ao seguinte:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample4.aspx)]

Observe a adição do comando e a propriedade `DefaultMode`.

Vá em frente e teste esta página por meio de um navegador. Ao visitar com um usuário que tem um registro correspondente no `UserProfiles`, as configurações do usuário são exibidas em uma interface editável.

[![o DetailsView renderiza uma interface editável](storing-additional-user-information-cs/_static/image44.png)](storing-additional-user-information-cs/_static/image43.png)

**Figura 15**: o DetailsView renderiza uma interface editável ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-cs/_static/image45.png))

Tente alterar os valores e clicar no botão atualizar. Parece que nada acontece. Há um postback e os valores são salvos no banco de dados, mas não há nenhum comentário visual que o salvamento tenha ocorrido.

Para corrigir isso, retorne ao Visual Studio e adicione um controle rótulo acima de DetailsView. Defina seu `ID` como `SettingsUpdatedMessage`, sua propriedade `Text` como "suas configurações foram atualizadas" e suas propriedades `Visible` e `EnableViewState` para `false`.

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample5.aspx)]

Precisamos exibir o rótulo `SettingsUpdatedMessage` sempre que o DetailsView for atualizado. Para fazer isso, crie um manipulador de eventos para o evento de `ItemUpdated` do DetailsView e adicione o seguinte código:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample6.cs)]

Retorne à página de `AdditionalUserInfo.aspx` por meio de um navegador e atualize os dados. Desta vez, uma mensagem de status útil é exibida.

[![uma mensagem curta é exibida quando as configurações são atualizadas](storing-additional-user-information-cs/_static/image47.png)](storing-additional-user-information-cs/_static/image46.png)

**Figura 16**: uma mensagem curta é exibida quando as configurações são atualizadas ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-cs/_static/image48.png))

> [!NOTE]
> A interface de edição do controle DetailsView deixa muito a desejar. Ele usa caixas de texto de tamanho padrão, mas o campo de assinatura deve ser, provavelmente, uma TextBox de várias linhas. Um RegularExpressionValidator deve ser usado para garantir que a URL da Home Page, se inserida, comece com "http://" ou "https://". Além disso, como o controle DetailsView tem sua propriedade `DefaultMode` definida como `Edit`, o botão Cancelar não faz nada. Ele deve ser removido ou, quando clicado, redirecionar o usuário para alguma outra página (como `~/Default.aspx`). Eu deixe esses aprimoramentos como um exercício para o leitor.

### <a name="adding-a-link-to-theadditionaluserinfoaspxpage-in-the-master-page"></a>Adicionando um link à página de`AdditionalUserInfo.aspx`na página mestra

Atualmente, o site não fornece links para a página de `AdditionalUserInfo.aspx`. A única maneira de contatá-lo é inserir a URL da página diretamente na barra de endereços do navegador. Vamos adicionar um link para essa página na página mestra de `Site.master`.

Lembre-se de que a página mestra contém um controle da Web LoginView em seu `LoginContent` ContentPlaceHolder que exibe marcação diferente para visitantes autenticados e anônimos. Atualize o `LoggedInTemplate` do controle LoginView para incluir um link para a página `AdditionalUserInfo.aspx`. Depois de fazer essas alterações, a marcação declarativa do controle LoginView deve ser semelhante ao seguinte:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample7.aspx)]

Observe a adição do `lnkUpdateSettings` controle de hiperlink à `LoggedInTemplate`. Com esse link implementado, os usuários autenticados podem ir rapidamente para a página para exibir e modificar suas configurações de cidade, Home Page e Home.

## <a name="step-4-adding-new-guestbook-comments"></a>Etapa 4: adicionando novos comentários do livro de visitas

A página `Guestbook.aspx` é onde os usuários autenticados podem exibir o livro de visitas e deixar um comentário. Vamos começar criando a interface para adicionar novos comentários do livro de visitas.

Abra a página `Guestbook.aspx` no Visual Studio e construa uma interface do usuário que consiste em dois controles TextBox, um para o assunto do novo comentário e outro para seu corpo. Defina a propriedade `ID` do primeiro controle TextBox como `Subject` e sua propriedade `Columns` como 40; Defina o `ID` de segundo como `Body`, seu `TextMode` para `MultiLine`e suas propriedades `Width` e `Rows` como "95%" e 8, respectivamente. Para concluir a interface do usuário, adicione um controle Web de botão chamado `PostCommentButton` e defina sua propriedade `Text` como "postar seu comentário".

Como cada comentário do livro de visitas requer um assunto e um corpo, adicione um RequiredFieldValidator para cada uma das caixas de texto. Defina a propriedade `ValidationGroup` desses controles como "EnterComment"; da mesma forma, defina a propriedade `ValidationGroup` do controle de `PostCommentButton` como "EnterComment". Para obter mais informações sobre o ASP. Controles de validação da rede, confira [validação de formulário em ASP.net](http://www.4guysfromrolla.com/webtech/090200-1.shtml), descartando [os controles de validação no ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)e o [tutorial de controles do servidor de validação](http://www.w3schools.com/aspnet/aspnet_refvalidationcontrols.asp) no [W3Schools](http://www.w3schools.com/).

Depois de criar a interface do usuário, a marcação declarativa de sua página deve ser semelhante ao seguinte:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample8.aspx)]

Com a interface do usuário concluída, nossa próxima tarefa é inserir um novo registro na tabela `GuestbookComments` quando o `PostCommentButton` é clicado. Isso pode ser feito de várias maneiras: podemos escrever o código ADO.NET no manipulador de eventos de `Click` do botão; Podemos adicionar um controle SqlDataSource à página, configurar seu `InsertCommand`e, em seguida, chamar seu método `Insert` do manipulador de eventos `Click`; ou poderíamos criar uma camada intermediária responsável por inserir novos comentários do livro de visitas e invocar essa funcionalidade a partir do manipulador de eventos `Click`. Como vimos o uso de um SqlDataSource na etapa 3, vamos usar o código ADO.NET aqui.

> [!NOTE]
> As classes ADO.NET usadas para acessar dados por meio de programação de um banco de dado Microsoft SQL Server estão localizadas no namespace `System.Data.SqlClient`. Talvez seja necessário importar esse namespace para a classe code-behind da página (ou seja, `using System.Data.SqlClient;`).

Crie um manipulador de eventos para o evento de `Click` do `PostCommentButton`e adicione o seguinte código:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample9.cs)]

O manipulador de eventos `Click` começa verificando se os dados fornecidos pelo usuário são válidos. Se não for, o manipulador de eventos será encerrado antes de inserir um registro. Supondo que os dados fornecidos sejam válidos, o valor `UserId` do usuário conectado no momento é recuperado e armazenado na variável local `currentUserId`. Esse valor é necessário porque devemos fornecer um valor de `UserId` ao inserir um registro em `GuestbookComments`.

Depois disso, a cadeia de conexão para o banco de dados de `SecurityTutorials` é recuperada de `Web.config` e a instrução SQL `INSERT` é especificada. Um objeto `SqlConnection` é então criado e aberto. Em seguida, um objeto `SqlCommand` é construído e os valores para os parâmetros usados na consulta `INSERT` são atribuídos. Em seguida, a instrução `INSERT` é executada e a conexão é fechada. No final do manipulador de eventos, as propriedades de `Text` de caixas de Text`Subject` e `Body` são limpas para que os valores do usuário não sejam persistidos no postback.

Vá em frente e teste esta página em um navegador. Como essa página está na pasta `Membership`, ela não pode ser acessada por visitantes anônimos. Portanto, você precisará primeiro fazer logon (se ainda não tiver feito isso). Insira um valor nas caixas de Text`Subject` e `Body` e clique no botão `PostCommentButton`. Isso fará com que um novo registro seja adicionado ao `GuestbookComments`. No postback, o assunto e o corpo que você forneceu são apagados das caixas de texto.

Depois de clicar no botão `PostCommentButton`, não há nenhum comentário visual de que o comentário foi adicionado ao livro de visitas. Ainda precisamos atualizar esta página para exibir os comentários existentes do livro de visitas, que faremos na etapa 5. Depois de fazer isso, o comentário recém-adicionado aparecerá na lista de comentários, fornecendo comentários visuais adequados. Por enquanto, confirme se o comentário do livro de visitas foi salvo examinando o conteúdo da tabela `GuestbookComments`.

A figura 17 mostra o conteúdo da tabela `GuestbookComments` depois que dois comentários foram deixados.

[![você pode ver os comentários do livro de visitas na tabela GuestbookComments](storing-additional-user-information-cs/_static/image50.png)](storing-additional-user-information-cs/_static/image49.png)

**Figura 17**: você pode ver os comentários do livro de visitas na tabela `GuestbookComments` ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-cs/_static/image51.png))

> [!NOTE]
> Se um usuário tentar inserir um comentário de livro de visitas que contenha marcação potencialmente perigosa, como HTML, ASP.NET gerará um `HttpRequestValidationException`. Para saber mais sobre essa exceção, por que ela é gerada e como permitir que os usuários enviem valores potencialmente perigosos, consulte o [whitepaper de validação de solicitação](../../../../whitepapers/request-validation.md).

## <a name="step-5-listing-the-existing-guestbook-comments"></a>Etapa 5: listando os comentários existentes do livro de visitas

Além de deixar comentários, um usuário visitando a página `Guestbook.aspx` também deve ser capaz de exibir os comentários existentes do livro de visitas. Para fazer isso, adicione um controle ListView chamado `CommentList` à parte inferior da página.

> [!NOTE]
> O controle ListView é novo no ASP.NET versão 3,5. Ele foi projetado para exibir uma lista de itens em um layout muito personalizável e flexível, ainda assim oferecer funcionalidade interna de edição, inserção, exclusão, paginação e classificação, como o GridView. Se você estiver usando o ASP.NET 2,0, será necessário usar o controle DataList ou Repeater. Para obter mais informações sobre como usar ListView, consulte a entrada de blog de [Scott Guthrie](https://weblogs.asp.net/scottgu/), [o controle ASP: ListView](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)e meu artigo, [exibindo dados com o controle ListView](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx).

Abra a marca inteligente do ListView e, na lista suspensa escolher fonte de dados, associe o controle a uma nova fonte de dados. Como vimos na etapa 2, isso iniciará o assistente de configuração de fonte de dados. Selecione o ícone de banco de dados, nomeie o `CommentsDataSource`SqlDataSource resultante e clique em OK. Em seguida, selecione a cadeia de conexão `SecurityTutorialsConnectionString` na lista suspensa e clique em Avançar.

Neste ponto da etapa 2, especificamos os dados a serem consultados escolhendo a tabela `UserProfiles` na lista suspensa e selecionando as colunas a serem retornadas (consulte novamente a Figura 9). Desta vez, no entanto, queremos criar uma instrução SQL que retorna não apenas os registros de `GuestbookComments`, mas também o principal município, Home Page, assinatura e nome de usuário do comentário. Portanto, selecione o botão de opção "especificar uma instrução SQL personalizada ou um procedimento armazenado" e clique em Avançar.

Isso abrirá a tela "definir instruções personalizadas ou procedimentos armazenados". Clique no botão Construtor de Consultas para criar a consulta graficamente. O Construtor de Consultas começa solicitando que especifiquemos as tabelas das quais desejamos consultar. Selecione as tabelas `GuestbookComments`, `UserProfiles`e `aspnet_Users` e clique em OK. Isso adicionará todas as três tabelas à superfície de design. Como há restrições de chave estrangeira entre as tabelas `GuestbookComments`, `UserProfiles`e `aspnet_Users`, a Construtor de Consultas automaticamente `JOIN` s essas tabelas.

Tudo o que resta é especificar as colunas a serem retornadas. Na tabela `GuestbookComments`, selecione as colunas `Subject`, `Body`e `CommentDate`; retornar as colunas `HomeTown`, `HomepageUrl`e `Signature` da tabela `UserProfiles`; e retorne `UserName` de `aspnet_Users`. Além disso, adicione "`ORDER BY CommentDate DESC`" ao final da consulta `SELECT` para que as postagens mais recentes sejam retornadas primeiro. Depois de fazer essas seleções, sua interface de Construtor de Consultas deve ser semelhante à captura de tela na Figura 18.

[![a consulta construída une as tabelas GuestbookComments, userperfils e aspnet_Users](storing-additional-user-information-cs/_static/image53.png)](storing-additional-user-information-cs/_static/image52.png)

**Figura 18**: a consulta construída `JOIN` s `GuestbookComments`, `UserProfiles`e tabelas de `aspnet_Users` ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-cs/_static/image54.png))

Clique em OK para fechar a janela de Construtor de Consultas e retornar para a tela "definir instruções personalizadas ou procedimentos armazenados". Clique em avançar para ir para a tela "consulta de teste", em que você pode exibir os resultados da consulta clicando no botão Testar consulta. Quando estiver pronto, clique em concluir para concluir o assistente para configurar fonte de dados.

Quando concluimos o assistente para configurar fonte de dados na etapa 2, a coleção de `Fields` do controle DetailsView associado foi atualizada para incluir um BoundField para cada coluna retornada pelo `SelectCommand`. O ListView, no entanto, permanece inalterado; Ainda precisamos definir seu layout. O layout do ListView pode ser construído manualmente por meio de sua marcação declarativa ou da opção "Configurar ListView" em sua marca inteligente. Eu geralmente prefiro definir a marcação manualmente, mas usar qualquer método que seja mais natural para você.

Terminei usando os seguintes `LayoutTemplate`, `ItemTemplate`e `ItemSeparatorTemplate` para o meu controle ListView:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample10.aspx)]

O `LayoutTemplate` define a marcação emitida pelo controle, enquanto o `ItemTemplate` renderiza cada item retornado pelo SqlDataSource. A marcação resultante do `ItemTemplate`é colocada no controle de `itemPlaceholder` do `LayoutTemplate`. Além do `itemPlaceholder`, o `LayoutTemplate` inclui um controle DataPager, que limita a ListView para mostrar apenas 10 comentários de visitantes por página (o padrão) e renderiza uma interface de paginação.

Minha `ItemTemplate` exibe o assunto de cada comentário do livro de visitas em um elemento `<h4>` com o corpo situado abaixo do assunto. Observe que a sintaxe usada para exibir o corpo usa os dados retornados pela instrução `Eval("Body")` DataBinding, converte-os em uma cadeia de caracteres e substitui as quebras de linha pelo elemento `<br />`. Essa conversão é necessária para mostrar as quebras de linha inseridas ao enviar o comentário, pois o espaço em branco é ignorado por HTML. A assinatura do usuário é exibida abaixo do corpo em itálico, seguida pela cidade inicial do usuário, um link para sua página inicial, a data e a hora em que o comentário foi feito e o nome de usuário da pessoa que saiu do comentário.

Reserve um tempo para exibir a página por meio de um navegador. Você deve ver os comentários que você adicionou ao livro de visitas na etapa 5 exibido aqui.

[![livro de visitas. aspx agora exibe os comentários do livro de visitas](storing-additional-user-information-cs/_static/image56.png)](storing-additional-user-information-cs/_static/image55.png)

**Figura 19**: agora `Guestbook.aspx` exibe os comentários do livro de visitas ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-cs/_static/image57.png))

Tente adicionar um novo comentário ao livro de visitas. Ao clicar no botão `PostCommentButton`, a página é postada novamente e o comentário é adicionado ao banco de dados, mas o controle ListView não é atualizado para mostrar o novo comentário. Isso pode ser corrigido por um dos dois:

- Atualizar o manipulador de eventos `Click` do botão de `PostCommentButton` para que ele invoque o método de `DataBind()` do controle ListView depois de inserir o novo comentário no banco de dados ou
- Definindo a propriedade `EnableViewState` do controle ListView como `false`. Essa abordagem funciona porque ao desabilitar o estado de exibição do controle, ele deve se reassociar aos dados subjacentes em cada postback.

O site do tutorial baixável neste tutorial ilustra as duas técnicas. A propriedade `EnableViewState` do controle ListView para `false` e o código necessário para reassociar os dados de forma programática ao ListView está presente no manipulador de eventos `Click`, mas é comentado.

> [!NOTE]
> Atualmente, a página `AdditionalUserInfo.aspx` permite que o usuário exiba e edite sua home page, Home Page e configurações de assinatura. Pode ser interessante atualizar `AdditionalUserInfo.aspx` para exibir os comentários do livro de visitas do usuário conectado. Ou seja, além de examinar e modificar suas informações, um usuário pode visitar a página `AdditionalUserInfo.aspx` para ver quais comentários do livro de visitas ela fez no passado. Deixe isso como um exercício para o leitor interessado.

## <a name="step-6-customizing-the-createuserwizard-control-to-include-an-interface-for-the-home-town-homepage-and-signature"></a>Etapa 6: Personalizando o controle CreateUserWizard para incluir uma interface para a cidade principal, Home Page e assinatura

A consulta `SELECT` usada pela página `Guestbook.aspx` usa uma `INNER JOIN` para combinar os registros relacionados entre as tabelas `GuestbookComments`, `UserProfiles`e `aspnet_Users`. Se um usuário que não tem registro no `UserProfiles` fizer um comentário do livro de visitas, o comentário não será exibido na ListView porque a `INNER JOIN` retorna apenas `GuestbookComments` registros quando há registros correspondentes em `UserProfiles` e `aspnet_Users`. E como vimos na etapa 3, se um usuário não tiver um registro em `UserProfiles` ela não poderá exibir ou editar suas configurações na página `AdditionalUserInfo.aspx`.

Não é preciso dizer, devido às nossas decisões de design, é importante que cada conta de usuário no sistema de associação tenha um registro correspondente na tabela de `UserProfiles`. O que queremos é que um registro correspondente seja adicionado ao `UserProfiles` sempre que uma nova conta de usuário de associação for criada por meio do CreateUserWizard.

Conforme discutido no tutorial [*criando contas de usuário*](creating-user-accounts-cs.md) , depois que a nova conta de usuário de associação é criada, o controle CreateUserWizard gera seu [evento de`CreatedUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx). Podemos criar um manipulador de eventos para esse evento, obter o UserId para o usuário recém-criado e, em seguida, inserir um registro na tabela `UserProfiles` com valores padrão para as colunas `HomeTown`, `HomepageUrl`e `Signature`. O que é mais, é possível solicitar ao usuário esses valores Personalizando a interface do controle CreateUserWizard para incluir caixas de textadicionais.

Primeiro, vamos examinar como adicionar uma nova linha à tabela `UserProfiles` no manipulador de eventos `CreatedUser` com valores padrão. Depois disso, veremos como personalizar a interface do usuário do controle CreateUserWizard para incluir campos de formulário adicionais para coletar a cidade, a Home Page e a assinatura do novo usuário.

### <a name="adding-a-default-row-touserprofiles"></a>Adicionando uma linha padrão a`UserProfiles`

No tutorial [*criando contas de usuário*](creating-user-accounts-cs.md) , adicionamos um controle CreateUserWizard à página de `CreatingUserAccounts.aspx` na pasta `Membership`. Para que o controle CreateUserWizard adicione um registro a `UserProfiles` tabela na criação da conta de usuário, precisamos atualizar a funcionalidade do controle CreateUserWizard. Em vez de fazer essas alterações na página de `CreatingUserAccounts.aspx`, vamos adicionar um novo controle CreateUserWizard à página `EnhancedCreateUserWizard.aspx` e fazer as modificações para este tutorial.

Abra a página `EnhancedCreateUserWizard.aspx` no Visual Studio e arraste um controle CreateUserWizard da caixa de ferramentas para a página. Defina a propriedade `ID` do controle CreateUserWizard como `NewUserWizard`. Como discutimos no <a id="_msoanchor_5"> </a>tutorial [*criando contas de usuário*](creating-user-accounts-cs.md) , a interface de usuário padrão do CreateUserWizard solicita ao visitante as informações necessárias. Depois que essas informações forem fornecidas, o controle criará internamente uma nova conta de usuário na estrutura de associação, tudo sem ter que escrever uma única linha de código.

O controle CreateUserWizard gera vários eventos durante seu fluxo de trabalho. Depois que um visitante fornecer as informações de solicitação e enviar o formulário, o controle CreateUserWizard inicialmente acionará seu [evento de`CreatingUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx). Se houver um problema durante o processo de criação, o [evento de`CreateUserError`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx) será acionado; no entanto, se o usuário for criado com êxito, o [evento`CreatedUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx) será gerado. <a id="_msoanchor_6"> </a>No tutorial [*criando contas de usuário*](creating-user-accounts-cs.md) , criamos um manipulador de eventos para o evento `CreatingUser` para garantir que o nome de usuário fornecido não contenha espaços à esquerda ou à direita, e que o nome de usuário não apareceu em nenhum lugar da senha.

Para adicionar uma linha na tabela de `UserProfiles` para o usuário recém-criado, precisamos criar um manipulador de eventos para o evento `CreatedUser`. No momento em que o evento de `CreatedUser` foi disparado, a conta de usuário já foi criada na estrutura de associação, permitindo que recuperemos o valor de UserId da conta.

Crie um manipulador de eventos para o evento de `CreatedUser` do `NewUserWizard`e adicione o seguinte código:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample11.cs)]

O código acima se aplica ao recuperar o UserId da conta de usuário recém-adicionada. Isso é feito usando o método `Membership.GetUser(username)` para retornar informações sobre um usuário específico e, em seguida, usar a propriedade `ProviderUserKey` para recuperar seu UserId. O nome de usuário inserido pelo usuário no controle CreateUserWizard está disponível por meio de sua [propriedade`UserName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx).

Em seguida, a cadeia de conexão é recuperada de `Web.config` e a instrução `INSERT` é especificada. Os objetos ADO.NET necessários são instanciados e o comando é executado. O código atribui uma instância de [`DBNull`](https://msdn.microsoft.com/library/system.dbnull.aspx) para os parâmetros `@HomeTown`, `@HomepageUrl`e `@Signature`, que tem o efeito de inserir valores de `NULL` de banco de dados para os campos `HomeTown`, `HomepageUrl`e `Signature`.

Visite a página `EnhancedCreateUserWizard.aspx` por meio de um navegador e crie uma nova conta de usuário. Depois de fazer isso, retorne ao Visual Studio e examine o conteúdo das tabelas `aspnet_Users` e `UserProfiles` (como fizemos de volta na Figura 12). Você deve ver a nova conta de usuário em `aspnet_Users` e uma linha de `UserProfiles` correspondente (com valores de `NULL` para `HomeTown`, `HomepageUrl`e `Signature`).

[![um novo registro de conta de usuário e userperfis foi adicionado](storing-additional-user-information-cs/_static/image59.png)](storing-additional-user-information-cs/_static/image58.png)

**Figura 20**: uma nova conta de usuário e um registro de `UserProfiles` foram adicionados ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-cs/_static/image60.png))

Depois que o visitante tiver fornecido suas novas informações de conta e clicado no botão "criar usuário", a conta de usuário será criada e uma linha adicionada à tabela de `UserProfiles`. O CreateUserWizard, em seguida, exibe sua `CompleteWizardStep`, que exibe uma mensagem de êxito e um botão continuar. Clicar no botão continuar causa um postback, mas nenhuma ação é tomada, deixando o usuário preso na página `EnhancedCreateUserWizard.aspx`.

Podemos especificar uma URL para a qual o usuário será enviado quando o botão continuar for clicado por meio da [propriedade`ContinueDestinationPageUrl`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx)do controle CreateUserWizard. Defina a propriedade `ContinueDestinationPageUrl` como "~/Membership/AdditionalUserInfo.aspx". Isso leva o novo usuário para `AdditionalUserInfo.aspx`, no qual eles podem exibir e atualizar suas configurações.

### <a name="customizing-the-createuserwizards-interface-to-prompt-for-the-new-users-home-town-homepage-and-signature"></a>Personalizando a interface do CreateUserWizard para solicitar a cidade, a Home Page e a assinatura do novo usuário

A interface padrão do controle CreateUserWizard é suficiente para cenários de criação de conta simples, em que somente informações de conta de usuário de núcleo, como username, password e email precisam ser coletadas. Mas e se quiséssemos solicitar ao visitante que insira seu principal cidade, Home Page e assinatura ao criar sua conta? É possível personalizar a interface do controle CreateUserWizard para coletar informações adicionais na inscrição, e essas informações podem ser usadas no manipulador de eventos `CreatedUser` para inserir registros adicionais no banco de dados subjacente.

O controle CreateUserWizard estende o controle do assistente de ASP.NET, que é um controle que permite que um desenvolvedor de página defina uma série de `WizardSteps`ordenados. O controle Wizard renderiza a etapa ativa e fornece uma interface de navegação que permite ao visitante percorrer essas etapas. O controle Wizard é ideal para dividir uma tarefa longa em várias etapas curtas. Para obter mais informações sobre o controle do assistente, consulte [criando uma interface do usuário passo a passo com o controle do assistente ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx).

A marcação padrão do controle CreateUserWizard define dois `WizardSteps`: `CreateUserWizardStep` e `CompleteWizardStep`.

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample12.aspx)]

A primeira `WizardStep`, `CreateUserWizardStep`, renderiza a interface que solicita o nome de usuário, a senha, o email e assim por diante. Depois que o visitante fornecer essas informações e clicar em "criar usuário", ela será mostrada na `CompleteWizardStep`, que mostra a mensagem de êxito e um botão continuar.

Para personalizar a interface do controle CreateUserWizard para incluir campos de formulário adicionais, podemos:

- <strong>Crie um ou mais novos</strong> <strong>`WizardStep`</strong> <strong>s para conter os elementos adicionais da interface do usuário</strong>. Para adicionar um novo `WizardStep` ao CreateUserWizard, clique no link "Adicionar/remover `WizardSteps`" de sua marca inteligente para iniciar o editor de coleção de `WizardStep`. A partir daí, você pode adicionar, remover ou reordenar as etapas no assistente. Esta é a abordagem que usaremos para este tutorial.

- <strong>Converta o</strong> <strong>`CreateUserWizardStep`</strong> <strong>em uma</strong> <strong>`WizardStep`</strong>editável <strong>.</strong> Isso substitui a `CreateUserWizardStep` com um `WizardStep` equivalente cuja marcação define uma interface do usuário que corresponde à do `CreateUserWizardStep`. Ao converter o `CreateUserWizardStep` em uma `WizardStep` podemos reposicionar os controles ou adicionar outros elementos da interface do usuário a essa etapa. Para converter o `CreateUserWizardStep` ou `CompleteWizardStep` em uma `WizardStep`editável, clique no link "Personalizar etapa de criação de usuário" ou "Personalizar etapa completa" na marca inteligente do controle.

- **Use uma combinação das duas opções acima.**

Uma coisa importante a ser lembrada é que o controle CreateUserWizard executa seu processo de criação de conta de usuário quando o botão "criar usuário" é clicado em seu `CreateUserWizardStep`. Não importa se há mais `WizardStep` s após o `CreateUserWizardStep` ou não.

Ao adicionar um `WizardStep` personalizado ao controle CreateUserWizard para coletar entrada de usuário adicional, o `WizardStep` personalizado pode ser colocado antes ou depois da `CreateUserWizardStep`. Se vier antes da `CreateUserWizardStep`, a entrada de usuário adicional coletada da `WizardStep` personalizada estará disponível para o manipulador de eventos `CreatedUser`. No entanto, se o `WizardStep` personalizado vier após `CreateUserWizardStep` a hora em que o `WizardStep` personalizado for exibido, a nova conta de usuário já foi criada e o evento de `CreatedUser` já foi disparado.

A figura 21 mostra o fluxo de trabalho quando o `WizardStep` adicionado precede o `CreateUserWizardStep`. Como as informações adicionais do usuário foram coletadas no momento em que o evento de `CreatedUser` é acionado, tudo o que precisamos fazer é atualizar o manipulador de eventos `CreatedUser` para recuperar essas entradas e usá-las para os valores de parâmetro da instrução `INSERT` (em vez de `DBNull.Value`).

[![o fluxo de trabalho CreateUserWizard quando um WizardStep adicional precede o CreateUserWizardStep](storing-additional-user-information-cs/_static/image62.png)](storing-additional-user-information-cs/_static/image61.png)

**Figura 21**: o fluxo de trabalho CreateUserWizard quando um `WizardStep` adicional precede a `CreateUserWizardStep` ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-cs/_static/image63.png))

Se o `WizardStep` personalizado for colocado *após* a `CreateUserWizardStep`, no entanto, o processo de criação de conta de usuário ocorrerá antes que o usuário tenha a oportunidade de inserir seu cidade principal, Home Page ou assinatura. Nesse caso, essas informações adicionais precisam ser inseridas no banco de dados depois que a conta de usuário tiver sido criada, como mostra a Figura 22.

[![o fluxo de trabalho CreateUserWizard quando um WizardStep adicional vier após o CreateUserWizardStep](storing-additional-user-information-cs/_static/image65.png)](storing-additional-user-information-cs/_static/image64.png)

**Figura 22**: o fluxo de trabalho CreateUserWizard quando uma `WizardStep` adicional é exibida após a `CreateUserWizardStep` ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-cs/_static/image66.png))

O fluxo de trabalho mostrado na Figura 22 aguarda a inserção de um registro na tabela `UserProfiles` até que a etapa 2 seja concluída. No entanto, se o visitante fechar seu navegador após a etapa 1, você terá atingido um estado em que uma conta de usuário foi criada, mas nenhum registro foi adicionado a `UserProfiles`. Uma solução alternativa é ter um registro com `NULL` ou valores padrão inseridos em `UserProfiles` no manipulador de eventos `CreatedUser` (que é acionado após a etapa 1) e, em seguida, atualizar esse registro após a conclusão da etapa 2. Isso garante que um registro de `UserProfiles` será adicionado para a conta de usuário, mesmo que o usuário saia do processo de registro no centro.

Para este tutorial, vamos criar um novo `WizardStep` que ocorre após o `CreateUserWizardStep`, mas antes da `CompleteWizardStep`. Primeiro, vamos obter o WizardStep em vigor e configurado e, em seguida, veremos o código.

Na marca inteligente do controle CreateUserWizard, selecione "Adicionar/remover `WizardStep` s", que abre a caixa de diálogo `WizardStep` editor de coleção. Adicione uma nova `WizardStep`, definindo sua `ID` como `UserSettings`, sua `Title` para "suas configurações" e sua `StepType` para `Step`. Em seguida, posicione-o para que ele venha após a `CreateUserWizardStep` ("inscrever-se para sua nova conta") e antes da `CompleteWizardStep` ("Concluir"), como mostra a Figura 23.

[![adicionar um novo WizardStep ao controle CreateUserWizard](storing-additional-user-information-cs/_static/image68.png)](storing-additional-user-information-cs/_static/image67.png)

**Figura 23**: adicionar um novo `WizardStep` ao controle CreateUserWizard ([clique para exibir a imagem em tamanho normal](storing-additional-user-information-cs/_static/image69.png))

Clique em OK para fechar a caixa de diálogo `WizardStep` editor de coleção. A nova `WizardStep` é evidenciada pela marcação declarativa atualizada do controle CreateUserWizard:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample13.aspx)]

Observe o novo elemento `<asp:WizardStep>`. Precisamos adicionar a interface do usuário para coletar a cidade, a Home Page e a assinatura do novo usuário aqui. Você pode inserir esse conteúdo na sintaxe declarativa ou por meio do designer. Para usar o designer, selecione a etapa "suas configurações" na lista suspensa na marca inteligente para ver a etapa no designer.

> [!NOTE]
> A seleção de uma etapa na lista suspensa da marca inteligente atualiza a [propriedade`ActiveStepIndex`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.activestepindex.aspx)do controle CreateUserWizard, que especifica o índice da etapa inicial. Portanto, se você usar essa lista suspensa para editar a etapa "suas configurações" no designer, certifique-se de defini-la de volta como "inscrever-se para sua nova conta" para que essa etapa seja mostrada quando os usuários visitarem a página `EnhancedCreateUserWizard.aspx` pela primeira vez.

Crie uma interface do usuário na etapa "suas configurações" que contém três controles de caixa de texto chamados `HomeTown`, `HomepageUrl`e `Signature`. Depois de construir essa interface, a marcação declarativa do CreateUserWizard deve ser semelhante ao seguinte:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample14.aspx)]

Vá em frente e visite esta página por meio de um navegador e crie uma nova conta de usuário, especificando valores para a cidade principal, Home Page e assinatura. Depois de concluir a `CreateUserWizardStep` a conta de usuário é criada na estrutura de associação e o manipulador de eventos de `CreatedUser` é executado, o que adiciona uma nova linha a `UserProfiles`, mas com um banco de dados `NULL` valor para `HomeTown`, `HomepageUrl`e `Signature`. Os valores inseridos para a cidade principal, Home Page e assinatura nunca são usados. O resultado líquido é uma nova conta de usuário com um registro de `UserProfiles` cujos campos `HomeTown`, `HomepageUrl`e `Signature` ainda devem ser especificados.

Precisamos executar o código após a etapa "Your Settings" (suas configurações) que usa a cidade inicial, honepage e valores de assinatura inseridos pelo usuário e atualiza o registro de `UserProfiles` apropriado. Cada vez que o usuário se move entre as etapas em um controle de assistente, o [evento de`ActiveStepChanged`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.activestepchanged.aspx) do assistente é acionado. Podemos criar um manipulador de eventos para esse evento e atualizar a tabela `UserProfiles` quando a etapa "suas configurações" for concluída.

Adicione um manipulador de eventos para o evento de `ActiveStepChanged` do CreateUserWizard e adicione o seguinte código:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample15.cs)]

O código acima começa determinando se nós acabamos de chegar à etapa "concluído". Como a etapa "concluída" ocorre imediatamente após a etapa "suas configurações", quando o visitante atinge a etapa "concluído", isso significa que ela acabou de concluir a etapa "suas configurações".

Nesse caso, precisamos fazer referência programaticamente aos controles TextBox dentro do `UserSettings WizardStep`. Isso é feito primeiro usando o método `FindControl` para referenciar programaticamente o `UserSettings WizardStep`e, em seguida, novamente para fazer referência às caixas de mensagem de dentro do `WizardStep`. Depois que as caixas de entrada tiverem sido referenciadas, estamos prontos para executar a instrução `UPDATE`. A instrução `UPDATE` tem o mesmo número de parâmetros que a instrução `INSERT` no manipulador de eventos `CreatedUser`, mas aqui usamos a cidade inicial, a Home Page e os valores de assinatura fornecidos pelo usuário.

Com esse manipulador de eventos em vigor, visite a página `EnhancedCreateUserWizard.aspx` por meio de um navegador e crie uma nova conta de usuário especificando valores para a cidade principal, Home Page e assinatura. Depois de criar a nova conta, você deve ser redirecionado para a página `AdditionalUserInfo.aspx`, em que as informações da Home, da página inicial, da Home Page e da assinatura são exibidas.

> [!NOTE]
> No momento, nosso site tem duas páginas das quais um visitante pode criar uma nova conta: `CreatingUserAccounts.aspx` e `EnhancedCreateUserWizard.aspx`. O mapa do site e a página de logon apontam para a página de `CreatingUserAccounts.aspx`, mas a página de `CreatingUserAccounts.aspx` não solicita ao usuário o seu local principal, a Home Page e as informações de assinatura e não adiciona uma linha correspondente à `UserProfiles`. Portanto, atualize a página `CreatingUserAccounts.aspx` para que ela ofereça essa funcionalidade ou atualize a página sitemap e login para fazer referência a `EnhancedCreateUserWizard.aspx` em vez de `CreatingUserAccounts.aspx`. Se você escolher a última opção, certifique-se de atualizar o arquivo de `Web.config` da pasta `Membership` para permitir que usuários anônimos acessem a página `EnhancedCreateUserWizard.aspx`.

## <a name="summary"></a>Resumo

Neste tutorial, vimos técnicas para modelar dados relacionados a contas de usuário dentro da estrutura de associação. Em particular, examinamos as entidades de modelagem que compartilham uma relação um-para-muitos com contas de usuário, bem como dados que compartilham uma relação um-para-um. Além disso, vimos como essas informações relacionadas poderiam ser exibidas, inseridas e atualizadas, com alguns exemplos usando o controle SqlDataSource e outras usando o código ADO.NET.

Este tutorial conclui nossa visão das contas de usuário. A partir do próximo tutorial, vamos mudar nossa atenção para as funções. Nos próximos tutoriais, veremos a estrutura de funções, Confira como criar novas funções, como atribuir funções a usuários, como determinar a quais funções um usuário pertence e como aplicar a autorização baseada em funções.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Acessando e atualizando dados no ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Controle do assistente do ASP.NET 2,0](https://weblogs.asp.net/scottgu/archive/2006/02/21/438732.aspx)
- [Criando uma interface do usuário passo a passo com o controle do assistente ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [Criando parâmetros de controle DataSource personalizados](http://aspnet.4guysfromrolla.com/articles/110106-1.aspx)
- [Personalizando o controle CreateUserWizard](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx)
- [Guias de início rápido do controle DetailsView](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/detailsview.aspx)
- [Exibindo dados com o controle ListView](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [Disparando os controles de validação no ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)
- [Editando inserção e excluindo dados](../../data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)
- [Validação de formulário em ASP.NET](http://www.4guysfromrolla.com/webtech/090200-1.shtml)
- [Coletando informações de registro de usuário personalizado](https://weblogs.asp.net/scottgu/archive/2006/07/05/Tip_2F00_Trick_3A00_-Gathering-Custom-User-Registration-Information.aspx)
- [Perfis no ASP.NET 2,0](http://www.odetocode.com/Articles/440.aspx)
- [O controle ASP: ListView](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)
- [Início rápido de perfis de usuário](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/profile/default.aspx)

### <a name="about-the-author"></a>Sobre o autor

Scott Mitchell, autor de vários livros sobre ASP/ASP. NET e fundador da 4GuysFromRolla.com, tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a *[Sams ensina a ASP.NET 2,0 em 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott pode ser contatado em [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) ou por meio de seu blog em [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a...

Esta série de tutoriais foi revisada por muitos revisores úteis. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](user-based-authorization-cs.md)
> [Próximo](creating-the-membership-schema-in-sql-server-vb.md)
