---
uid: identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
title: Migrando dados do provedor universal para associação e perfis de usuárioC#para ASP.net Identity ()-ASP.NET 4. x
author: rustd
description: Este tutorial descreve as etapas necessárias para migrar dados de usuário e função e dados de perfil de usuário criados usando Provedores Universais de um aplicativo existente...
ms.author: riande
ms.date: 12/13/2013
ms.custom: seoapril2019
ms.assetid: 2e260430-d13c-4658-bd05-e256fc0d63b8
msc.legacyurl: /identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 31f02a0cec3c531c45c37b7aad8456e01e80b5ea
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456103"
---
# <a name="migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity-c"></a>Migração de dados de Associação e Perfis de usuário do provedor universal para a Identidade do ASP.NET (C#)

por [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Robert McMurray](https://github.com/rmcmurray), [Suhas Joshi](https://github.com/suhasj)

> Este tutorial descreve as etapas necessárias para migrar dados de usuário e função e dados de perfil de usuário criados usando Provedores Universais de um aplicativo existente para o modelo de ASP.NET Identity. A abordagem mencionada aqui para migrar dados de perfil de usuário também pode ser usada em um aplicativo com associação SQL.

Com o lançamento do Visual Studio 2013, a equipe do ASP.NET introduziu um novo sistema de ASP.NET Identity e você pode ler mais sobre essa versão [aqui](../../index.md). Seguindo o artigo para migrar aplicativos Web da [associação do SQL para o novo sistema de identidade](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md), este artigo ilustra as etapas para migrar aplicativos existentes que seguem o modelo de provedores para gerenciamento de usuário e função para o novo modelo de identidade. O foco deste tutorial será, principalmente, a migração dos dados de perfil do usuário para conectá-los diretamente no novo sistema. A migração de informações de usuário e função é semelhante à associação do SQL. A abordagem seguida para migrar dados de perfil também pode ser usada em um aplicativo com associação SQL.

Por exemplo, vamos começar com um aplicativo Web criado usando o Visual Studio 2012, que usa o modelo de provedores. Em seguida, adicionaremos o código para o gerenciamento de perfil, registraremos um usuário, adicionaremos dados de perfil para os usuários, migraremos o esquema de banco e, em seguida, alteraremos o aplicativo para usar o sistema de identidade para o gerenciamento de usuários e funções. Como um teste de migração, os usuários criados usando Provedores Universais devem ser capazes de fazer logon e novos usuários devem ser capazes de se registrar.

> [!NOTE]
> Você pode encontrar o exemplo completo em [https://github.com/suhasj/UniversalProviders-Identity-Migrations](https://github.com/suhasj/UniversalProviders-Identity-Migrations).

## <a name="profile-data-migration-summary"></a>Resumo de migração de dados de perfil

Antes de começar com as migrações, vamos examinar a experiência de armazenamento de dados de perfil no modelo de provedores. Os dados de perfil para usuários de aplicativos podem ser armazenados de várias maneiras, o mais comum entre eles usando os provedores de perfil interno fornecidos junto com o Provedores Universais. As etapas incluem

1. Adicione uma classe que tenha propriedades usadas para armazenar dados de perfil.
2. Adicione uma classe que estenda ' ProfileBase ' e implemente métodos para obter os dados de perfil acima para o usuário.
3. Habilite o uso de provedores de perfil padrão no arquivo *Web. config* e defina a classe declarada na etapa #2 a ser usada no acesso às informações de perfil.

As informações de perfil são armazenadas como XML serializado e dados binários na tabela ' perfis ' no banco de dados.

Depois de migrar o aplicativo para usar o novo sistema de ASP.NET Identity, as informações de perfil são desserializadas e armazenadas como propriedades na classe User. Cada propriedade pode ser mapeada em colunas na tabela do usuário. A vantagem aqui é que as propriedades podem ser trabalhadas diretamente usando a classe User, além de não ter que serializar/desserializar informações de dados a cada vez ao acessá-la.

## <a name="getting-started"></a>Introdução

1. Crie um novo aplicativo ASP.NET 4,5 Web Forms no Visual Studio 2012. O exemplo atual usa o modelo de Web Forms, mas você também pode usar o aplicativo MVC.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.jpg)
2. Criar uma nova pasta "modelos" para armazenar informações de perfil  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.png)
3. Por exemplo, deixe-nos armazenar a data de nascimento, cidade, altura e peso do usuário no perfil. A altura e o peso são armazenados como uma classe personalizada chamada ' PersonalStats '. Para armazenar e recuperar o perfil, precisamos de uma classe que estenda ' ProfileBase '. Vamos criar uma nova classe ' AppProfile ' para obter e armazenar informações de perfil.

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample1.cs)]
4. Habilite o perfil no arquivo *Web. config* . Insira o nome da classe a ser usada para armazenar/recuperar as informações do usuário criadas na etapa #3.

    [!code-xml[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample2.xml)]
5. Adicione uma página de Web Forms na pasta ' conta ' para obter os dados de perfil do usuário e armazená-los. Clique com o botão direito do mouse em projeto e selecione ' Adicionar novo item '. Adicione uma nova página WebForms à página mestra ' AddProfileData. aspx '. Copie o seguinte na seção ' MainContent ':

    [!code-html[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample3.html)]

   Adicione o seguinte código ao code-behind:

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample4.cs)]

   Adicione o namespace sob o qual a classe AppProfile é definida para remover os erros de compilação.
6. Execute o aplicativo e crie um novo usuário com o nome de usuário '**olduser '.** Navegue até a página ' AddProfileData ' e adicione informações de perfil para o usuário.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.png)

Você pode verificar se os dados são armazenados como XML serializado na tabela ' perfis ' usando a janela de Gerenciador de Servidores. No Visual Studio, no menu ' Exibir ', escolha ' Gerenciador de Servidores '. Deve haver uma conexão de dados para o banco de dado definido no arquivo *Web. config* . Clicar na conexão de dados mostra subcategorias diferentes. Expanda ' tabelas ' para mostrar as diferentes tabelas no banco de dados, clique com o botão direito do mouse em ' perfis ' e escolha ' Mostrar dados da tabela ' para exibir os dados de perfil armazenados na tabela de perfis.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.png)

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image4.png)

## <a name="migrating-database-schema"></a>Migrando esquema de banco de dados

Para fazer com que o banco de dados existente funcione com o sistema de identidade, precisamos atualizar o esquema no banco de dados de identidade para dar suporte aos campos que adicionamos ao banco de dados original. Isso pode ser feito usando scripts SQL para criar novas tabelas e copiar as informações existentes. Na janela ' Gerenciador de Servidores ', expanda ' DefaultConnection ' para exibir as tabelas. Clique com o botão direito do mouse em tabelas e selecione ' nova consulta '

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image5.png)

Cole o script SQL de [https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt](https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt) e execute-o. Se a ' DefaultConnection ' for atualizada, podemos ver que as novas tabelas são adicionadas. Você pode verificar os dados dentro das tabelas para ver que as informações foram migradas.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image6.png)

## <a name="migrating-the-application-to-use-aspnet-identity"></a>Migrando o aplicativo para usar o ASP.NET Identity

1. Instale os pacotes NuGet necessários para ASP.NET Identity:

    - Microsoft.AspNet.Identity.EntityFramework
    - Microsoft.AspNet.Identity.Owin
    - Microsoft.Owin.Host.SystemWeb
    - Microsoft.Owin.Security.Facebook
    - Microsoft.Owin.Security.Google
    - Microsoft.Owin.Security.MicrosoftAccount
    - Microsoft.Owin.Security.Twitter

   Mais informações sobre como gerenciar pacotes NuGet podem ser encontradas [aqui](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog)
2. Para trabalhar com os dados existentes na tabela, precisamos criar classes de modelo que mapeiem de volta para as tabelas e conectá-las no sistema de identidade. Como parte do contrato de identidade, as classes de modelo devem implementar as interfaces definidas na DLL Identity. Core ou podem estender a implementação existente dessas interfaces disponíveis em Microsoft. AspNet. Identity. EntityFramework. Usaremos as classes existentes para função, logons de usuário e declarações de usuário. Precisamos usar um usuário personalizado para nosso exemplo. Clique com o botão direito do mouse no projeto e crie a nova pasta ' IdentityModels '. Adicione uma nova classe ' user ', conforme mostrado abaixo:

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample5.cs)]

   Observe que ' ProfileInfo ' agora é uma propriedade na classe User. Portanto, podemos usar a classe User para trabalhar diretamente com dados de perfil.

Copie os arquivos nas pastas **identitymodels** e **IdentityAccount** da origem de download ( [https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations](https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations) ). Elas têm as classes de modelo restantes e as novas páginas necessárias para o gerenciamento de usuários e funções usando as APIs de ASP.NET Identity. A abordagem usada é semelhante à associação do SQL e a explicação detalhada pode ser encontrada [aqui](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md).

[!INCLUDE[](../../../includes/identity/alter-command-exception.md)]

## <a name="copying-profile-data-to-the-new-tables"></a>Copiando dados de perfil para as novas tabelas

Como mencionado anteriormente, precisamos desserializar os dados XML nas tabelas de perfis e armazená-los nas colunas da tabela AspNetUsers. As novas colunas foram criadas na tabela usuários na etapa anterior, portanto, tudo o que resta é preencher essas colunas com os dados necessários. Para isso, usaremos um aplicativo de console que é executado uma vez para popular as colunas recém-criadas na tabela users.

1. Crie um novo aplicativo de console na solução de saída.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.jpg)
2. Instale a versão mais recente do pacote de Entity Framework.
3. Adicione o aplicativo Web criado acima como uma referência ao aplicativo de console. Para fazer isso, clique com o botão direito do mouse em projeto, em seguida, em ' Adicionar referências ' e em solução, em seguida, clique no projeto e clique em OK.
4. Copie o código abaixo na classe Program.cs. Essa lógica lê os dados de perfil para cada usuário, serializa-os como um objeto ' ProfileInfo ' e o armazena de volta ao banco de dados.

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample6.cs)]

   Alguns dos modelos usados são definidos na pasta ' IdentityModels ' do projeto de aplicativo Web, portanto, você deve incluir os namespaces correspondentes.
5. O código acima funciona no arquivo de banco de dados na pasta Data\_app do projeto de aplicativo Web criado nas etapas anteriores. Para fazer referência a isso, atualize a cadeia de conexão no arquivo app. config do aplicativo de console com a cadeia de conexão no Web. config do aplicativo Web. Forneça também o caminho físico completo na propriedade ' AttachDbFilename '.
6. Abra um prompt de comando e navegue até a pasta bin do aplicativo de console acima. Execute o executável e examine a saída do log, conforme mostrado na imagem a seguir.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.jpg)
7. Abra a tabela ' AspNetUsers ' na Gerenciador de Servidores e verifique os dados nas novas colunas que contêm as propriedades. Eles devem ser atualizados com os valores de propriedade correspondentes.

## <a name="verify-functionality"></a>Verificar funcionalidade

Use as páginas de associação adicionadas recentemente que são implementadas usando ASP.NET Identity para fazer logon de um usuário do banco de dados antigo. O usuário deve ser capaz de fazer logon usando as mesmas credenciais. Experimente as outras funcionalidades, como adicionar o OAuth, criar um novo usuário, alterar uma senha, adicionar funções, adicionar usuários a funções, etc.

Os dados de perfil do usuário antigo e os novos usuários devem ser recuperados e armazenados na tabela usuários. A tabela antiga não deve mais ser referenciada.

## <a name="conclusion"></a>Conclusão

O artigo descreveu o processo de migração de aplicativos Web que usaram o modelo de provedor para associação a ASP.NET Identity. O artigo também descreveu a migração de dados de perfil para que os usuários sejam conectados ao sistema de identidade. Deixe comentários abaixo para perguntas e problemas encontrados ao migrar seu aplicativo.

*Agradecemos a Rick Anderson e Robert McMurray pela revisão do artigo.*
