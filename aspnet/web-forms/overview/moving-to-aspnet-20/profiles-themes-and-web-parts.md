---
uid: web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
title: Perfis, temas e Web Parts | Microsoft Docs
author: microsoft
description: Há grandes alterações na configuração e instrumentação no ASP.NET 2,0. A nova API de configuração do ASP.NET permite que as alterações de configuração sejam feitas PR...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 92df4051-77c6-492c-bd34-23d24189cea4
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
msc.type: authoredcontent
ms.openlocfilehash: cf5c45781be6d003d28c6aa27efa08032579a6dd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78587230"
---
# <a name="profiles-themes-and-web-parts"></a>Perfis, temas e Web Parts

pela [Microsoft](https://github.com/microsoft)

> Há grandes alterações na configuração e instrumentação no ASP.NET 2,0. A nova API de configuração do ASP.NET permite que as alterações de configuração sejam feitas programaticamente. Além disso, muitas novas definições de configuração existem permitem novas configurações e instrumentação.

O ASP.NET 2,0 representa uma melhoria substancial na área de sites personalizados. Além dos recursos de associação que já abordamos, os perfis ASP.NET, os temas e as Web Parts aprimoram significativamente a personalização em sites da Web.

## <a name="aspnet-profiles"></a>Perfis de ASP.NET

Os perfis de ASP.NET são semelhantes às sessões. A diferença é que um perfil é persistente, enquanto uma sessão é perdida quando o navegador é fechado. Outra grande diferença entre sessões e perfis é que os perfis são fortemente tipados e, portanto, fornecem a você o IntelliSense durante o processo de desenvolvimento.

Um perfil é definido no arquivo de configuração de computadores ou no arquivo Web. config do aplicativo. (Não é possível definir um perfil em um arquivo Web. config de subpastas.) O código a seguir define um perfil para armazenar o nome e o sobrenome dos visitantes do site.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample1.xml)]

O tipo de dados padrão para uma propriedade de perfil é System. String. No exemplo acima, nenhum tipo de dados foi especificado. Portanto, as propriedades FirstName e LastName são do tipo cadeia de caracteres. Conforme mencionado anteriormente, as propriedades de perfil são fortemente tipadas. O código a seguir adiciona uma nova propriedade para age que é do tipo Int32.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample2.xml)]

Os perfis são geralmente usados com a autenticação de formulários do ASP.NET. Quando usado em combinação com a autenticação de formulários, cada usuário tem um perfil separado associado à sua ID de usuário. No entanto, também é possível permitir o uso de perfis em um aplicativo anônimo usando o elemento &lt;anonymousIdentification&gt; no arquivo de configuração junto com o atributo **AllowAnonymous** , conforme mostrado abaixo:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample3.xml)]

Quando um usuário anônimo navega no site, o ASP.NET cria uma instância de **ProfileCommon** para o usuário. Esse perfil usa uma ID exclusiva armazenada em um cookie no navegador para identificar o usuário como um visitante exclusivo. Dessa forma, você pode armazenar informações de perfil para usuários que estão navegando anonimamente.

## <a name="profile-groups"></a>Grupos de perfis

É possível agrupar Propriedades de perfis. Ao agrupar Propriedades, é possível simular vários perfis para um aplicativo específico.

A configuração a seguir configura uma propriedade FirstName e LastName para dois grupos; Compradores e clientes potenciais.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample4.xml)]

Em seguida, é possível definir propriedades em um grupo específico da seguinte maneira:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample5.cs)]

## <a name="storing-complex-objects"></a>Armazenando objetos complexos

Até agora, os exemplos que abordamos têm tipos de dados simples armazenados em um perfil. Também é possível armazenar tipos de dados complexos em um perfil especificando o método de serialização usando o atributo **serializes** da seguinte maneira:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample6.xml)]

Nesse caso, o tipo é PurchaseInvoice. A classe PurchaseInvoice precisa ser marcada como serializável e pode conter qualquer número de propriedades. Por exemplo, se PurchaseInvoice tiver uma propriedade chamada **NumItemsPurchased**, você poderá consultar essa propriedade no código da seguinte maneira:

[!code-css[Main](profiles-themes-and-web-parts/samples/sample7.css)]

## <a name="profile-inheritance"></a>Herança de perfil

É possível criar um perfil para uso em vários aplicativos. Ao criar uma classe de perfil derivada de ProfileBase, você pode reutilizar um perfil em vários aplicativos usando o atributo **Inherits** , conforme mostrado abaixo:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample8.xml)]

Nesse caso, a classe **PurchasingProfile** ficaria assim:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample9.cs)]

## <a name="profile-providers"></a>Provedores de perfis

Os perfis ASP.NET usam o modelo de provedor. O provedor padrão armazena as informações em um banco de dados do SQL Server Express na pasta\_data do aplicativo da Web usando o provedor SqlProfileProvider. Se o banco de dados não existir, o ASP.NET o criará automaticamente quando o perfil tentar armazenar informações.

Em alguns casos, no entanto, talvez você queira desenvolver seu próprio provedor de perfil. O recurso de perfil ASP.NET permite que você use facilmente provedores diferentes.

Você cria um provedor de perfil personalizado quando:

- Você precisa armazenar informações de perfil em uma fonte de dados, como em um banco de dados FoxPro ou em um banco de dados Oracle, que não tem suporte dos provedores de perfil incluídos no .NET Framework.
- Você precisa gerenciar informações de perfil usando um esquema de banco de dados diferente do esquema de banco de dados usado pelos provedores incluídos com o .NET Framework. Um exemplo comum é que você deseja integrar as informações de perfil com dados de usuário em um banco de dado SQL Server existente.

### <a name="required-classes"></a>Classes necessárias

Para implementar um provedor de perfil, você cria uma classe que herda a classe abstrata System. Web. Profile. ProfileProvider. A classe abstrata **ProfileProvider** , por sua vez, herda a classe abstrata System. Configuration. SettingsProvider, que herda a classe abstrata System. Configuration. Provider. ProviderBase. Devido a essa cadeia de herança, além dos membros necessários da classe **ProfileProvider** , você deve implementar os membros necessários das classes **SettingsProvider** e **ProviderBase** .

As tabelas a seguir descrevem as propriedades e os métodos que você deve implementar das classes abstratas **ProviderBase**, **SettingsProvider**e **ProfileProvider** .

### <a name="providerbase-members"></a>Membros do ProviderBase

| **Membro** | **Descrição** |
| --- | --- |
| Método Initialize | Usa como entrada o nome da instância do provedor e uma NameValueCollection de definições de configuração. Usado para definir opções e valores de propriedade para a instância do provedor, incluindo valores específicos de implementação e opções especificadas na configuração do computador ou no arquivo Web. config. |

### <a name="settingsprovider-members"></a>Membros do SettingsProvider

| **Membro** | **Descrição** |
| --- | --- |
| Propriedade ApplicationName | O nome do aplicativo que é armazenado com cada perfil. O provedor de perfil usa o nome do aplicativo para armazenar informações de perfil separadamente para cada aplicativo. Isso permite que vários aplicativos ASP.NET usem a mesma fonte de dados sem um conflito se o mesmo nome de usuário for criado em aplicativos diferentes. Como alternativa, vários aplicativos ASP.NET podem compartilhar uma fonte de dados de perfil especificando o mesmo nome de aplicativo. |
| Método getvalordapropriedades | Usa como entrada um SettingsContext e um objeto SettingsPropertyCollection. O **SettingsContext** fornece informações sobre o usuário. Você pode usar as informações como uma chave primária para recuperar informações de propriedade de perfil para o usuário. Use o objeto **SettingsContext** para obter o nome de usuário e se o usuário é autenticado ou anônimo. O **SettingsPropertyCollection** contém uma coleção de objetos SettingsProperty. Cada objeto **SettingsProperty** fornece o nome e o tipo da propriedade, bem como informações adicionais, como o valor padrão para a propriedade e se a propriedade é somente leitura. O método **Getvalordapropriedades** popula um SettingsPropertyValueCollection com objetos SettingsPropertyValue com base nos objetos **SettingsProperty** fornecidos como entrada. Os valores da fonte de dados para o usuário especificado são atribuídos às propriedades PropertyValue para cada objeto **SettingsPropertyValue** e toda a coleção é retornada. Chamar o método também atualiza o valor de LastActivityDate para o perfil de usuário especificado para a data e hora atuais. |
| Método setvalordapropriedades | Usa como entrada um **SettingsContext** e um objeto **SettingsPropertyValueCollection** . O **SettingsContext** fornece informações sobre o usuário. Você pode usar as informações como uma chave primária para recuperar informações de propriedade de perfil para o usuário. Use o objeto **SettingsContext** para obter o nome de usuário e se o usuário é autenticado ou anônimo. O **SettingsPropertyValueCollection** contém uma coleção de objetos **SettingsPropertyValue** . Cada objeto **SettingsPropertyValue** fornece o nome, o tipo e o valor da propriedade, bem como informações adicionais, como o valor padrão para a propriedade e se a propriedade é somente leitura. O método **Setvalordapropriedades** atualiza os valores de propriedade de perfil na fonte de dados para o usuário especificado. Chamar o método também atualiza os valores de **LastActivityDate** e LastUpdatedDate para o perfil de usuário especificado para a data e hora atuais. |

### <a name="profileprovider-members"></a>Membros de ProfileProvider

| **Membro** | **Descrição** |
| --- | --- |
| Método DeleteProfiles | Usa como entrada uma matriz de cadeia de caracteres de nomes de usuário e exclui da fonte de dados todas as informações de perfil e valores de propriedade para os nomes especificados em que o nome do aplicativo corresponde ao valor da propriedade **ApplicationName** . Se a fonte de dados der suporte a transações, é recomendável incluir todas as operações de exclusão em uma transação e reverter a transação e lançar uma exceção se qualquer operação de exclusão falhar. |
| Método DeleteProfiles | Usa como entrada uma coleção de objetos ProfileInfo e exclui da fonte de dados todas as informações de perfil e valores de propriedade para cada perfil em que o nome do aplicativo corresponde ao valor da propriedade **ApplicationName** . Se a fonte de dados der suporte a transações, é recomendável incluir todas as operações de exclusão em uma transação e reverter a transação e lançar uma exceção se qualquer operação de exclusão falhar. |
| Método DeleteInactiveProfiles | Usa como entrada um valor ProfileAuthenticationOption e um objeto DateTime e exclui da fonte de dados todas as informações de perfil e valores de propriedade em que a data da última atividade é menor ou igual à data e hora especificadas e onde o nome do aplicativo corresponde ao valor da propriedade **ApplicationName** . O parâmetro **ProfileAuthenticationOption** especifica se somente perfis anônimos, somente perfis autenticados ou todos os perfis devem ser excluídos. Se a fonte de dados der suporte a transações, é recomendável incluir todas as operações de exclusão em uma transação e reverter a transação e lançar uma exceção se qualquer operação de exclusão falhar. |
| Método GetAllProfiles | Usa como entrada um valor de **ProfileAuthenticationOption** , um inteiro que especifica o índice de página, um inteiro que especifica o tamanho da página e uma referência a um inteiro que será definido como a contagem total de perfis. Retorna um ProfileInfoCollection que contém objetos **ProfileInfo** para todos os perfis na fonte de dados em que o nome do aplicativo corresponde ao valor da propriedade **ApplicationName** . O parâmetro **ProfileAuthenticationOption** especifica se somente perfis anônimos, somente perfis autenticados ou todos os perfis devem ser retornados. Os resultados retornados pelo método **GetAllProfiles** são restritos pelos valores de índice de página e tamanho de página. O valor de tamanho da página especifica o número máximo de objetos **ProfileInfo** a serem retornados no **ProfileInfoCollection**. O valor de índice da página especifica qual página de resultados retornar, em que 1 identifica a primeira página. O parâmetro para o total de registros é um parâmetro out (você pode usar **ByRef** no Visual Basic) que é definido como o número total de perfis. Por exemplo, se o armazenamento de dados contiver 13 perfis para o aplicativo e o valor de índice da página for 2 com um tamanho de página de 5, o **ProfileInfoCollection** retornado conterá o sexto até o décimo dos perfis. O valor total de registros é definido como 13 quando o método retorna. |
| Método GetAllInactiveProfiles | Usa como entrada um valor de **ProfileAuthenticationOption** , um objeto **DateTime** , um inteiro que especifica o índice de página, um inteiro que especifica o tamanho da página e uma referência a um inteiro que será definido como a contagem total de perfis. Retorna um **ProfileInfoCollection** que contém objetos **ProfileInfo** para todos os perfis na fonte de dados em que a data da última atividade é menor ou igual ao **DateTime** especificado e onde o nome do aplicativo corresponde ao valor da propriedade **ApplicationName** . O parâmetro **ProfileAuthenticationOption** especifica se somente perfis anônimos, somente perfis autenticados ou todos os perfis devem ser retornados. Os resultados retornados pelo método **GetAllInactiveProfiles** são restritos pelos valores de índice de página e tamanho de página. O valor de tamanho da página especifica o número máximo de objetos **ProfileInfo** a serem retornados no **ProfileInfoCollection**. O valor de índice da página especifica qual página de resultados retornar, em que 1 identifica a primeira página. O parâmetro para o total de registros é um parâmetro out (você pode usar **ByRef** no Visual Basic) que é definido como o número total de perfis. Por exemplo, se o armazenamento de dados contiver 13 perfis para o aplicativo e o valor de índice da página for 2 com um tamanho de página de 5, o **ProfileInfoCollection** retornado conterá o sexto até o décimo dos perfis. O valor total de registros é definido como 13 quando o método retorna. |
| Método FindProfilesByUserName | Usa como entrada um valor de **ProfileAuthenticationOption** , uma cadeia de caracteres que contém um nome de usuário, um inteiro que especifica o índice de página, um inteiro que especifica o tamanho da página e uma referência a um inteiro que será definido como a contagem total de perfis. Retorna um **ProfileInfoCollection** que contém objetos **ProfileInfo** para todos os perfis na fonte de dados em que o nome de usuário corresponde ao nome de usuário especificado e onde o nome do aplicativo corresponde ao valor da propriedade **ApplicationName** . O parâmetro **ProfileAuthenticationOption** especifica se somente perfis anônimos, somente perfis autenticados ou todos os perfis devem ser retornados. Se sua fonte de dados oferecer suporte a recursos de pesquisa adicionais, como caracteres curinga, você poderá fornecer recursos de pesquisa mais extensivos para nomes de usuário. Os resultados retornados pelo método **FindProfilesByUserName** são restritos pelos valores de índice de página e tamanho de página. O valor de tamanho da página especifica o número máximo de objetos **ProfileInfo** a serem retornados no **ProfileInfoCollection**. O valor de índice da página especifica qual página de resultados retornar, em que 1 identifica a primeira página. O parâmetro para o total de registros é um parâmetro out (você pode usar **ByRef** no Visual Basic) que é definido como o número total de perfis. Por exemplo, se o armazenamento de dados contiver 13 perfis para o aplicativo e o valor de índice da página for 2 com um tamanho de página de 5, o **ProfileInfoCollection** retornado conterá o sexto até o décimo dos perfis. O valor total de registros é definido como 13 quando o método retorna. |
| Método FindInactiveProfilesByUserName | Usa como entrada um valor de **ProfileAuthenticationOption** , uma cadeia de caracteres que contém um nome de usuário, um objeto **DateTime** , um inteiro que especifica o índice de página, um inteiro que especifica o tamanho da página e uma referência a um inteiro que será definido como a contagem total de perfis. Retorna um **ProfileInfoCollection** que contém objetos **ProfileInfo** para todos os perfis na fonte de dados em que o nome de usuário corresponde ao nome de usuário especificado, em que a data da última atividade é menor ou igual ao **DateTime**especificado e onde o nome do aplicativo corresponde ao valor da propriedade **ApplicationName** . O parâmetro **ProfileAuthenticationOption** especifica se somente perfis anônimos, somente perfis autenticados ou todos os perfis devem ser retornados. Se sua fonte de dados oferecer suporte a recursos de pesquisa adicionais, como caracteres curinga, você poderá fornecer recursos de pesquisa mais extensivos para nomes de usuário. Os resultados retornados pelo método **FindInactiveProfilesByUserName** são restritos pelos valores de índice de página e tamanho de página. O valor de tamanho da página especifica o número máximo de objetos **ProfileInfo** a serem retornados no **ProfileInfoCollection**. O valor de índice da página especifica qual página de resultados retornar, em que 1 identifica a primeira página. O parâmetro para o total de registros é um parâmetro out (você pode usar **ByRef** no Visual Basic) que é definido como o número total de perfis. Por exemplo, se o armazenamento de dados contiver 13 perfis para o aplicativo e o valor de índice da página for 2 com um tamanho de página de 5, o **ProfileInfoCollection** retornado conterá o sexto até o décimo dos perfis. O valor total de registros é definido como 13 quando o método retorna. |
| Método GetNumberOfInActiveProfiles | Usa como entrada um valor **ProfileAuthenticationOption** e um objeto **DateTime** e retorna uma contagem de todos os perfis na fonte de dados em que a data da última atividade é menor ou igual ao **DateTime** especificado e onde o nome do aplicativo corresponde ao valor da propriedade **ApplicationName** . O parâmetro **ProfileAuthenticationOption** especifica se somente perfis anônimos, somente perfis autenticados ou todos os perfis devem ser contados. |

### <a name="applicationname"></a>ApplicationName

Como os provedores de perfil armazenam informações de perfil separadamente para cada aplicativo, você deve garantir que o esquema de dados inclua o nome do aplicativo e que as consultas e atualizações também incluam o nome do aplicativo. Por exemplo, o comando a seguir é usado para recuperar um valor de propriedade de um banco de dados com base no nome de usuário e se o perfil é anônimo e garante que o valor de **ApplicationName** seja incluído na consulta.

[!code-sql[Main](profiles-themes-and-web-parts/samples/sample10.sql)]

## <a name="aspnet-themes"></a>Temas do ASP.NET

## <a name="what-are-aspnet-20-themes"></a>O que são temas do ASP.NET 2,0?

Um dos aspectos mais importantes de um aplicativo Web é uma aparência consistente em todo o site. Os desenvolvedores de ASP.NET 1. x geralmente usam folhas de estilos em cascata (CSS) para implementar uma aparência consistente. Os temas do ASP.NET 2,0 melhoram significativamente na CSS porque oferecem ao desenvolvedor ASP.NET a capacidade de definir a aparência de controles de servidor ASP.NET, bem como elementos HTML. Os temas do ASP.NET podem ser aplicados a controles individuais, a uma página da Web específica ou a um aplicativo Web inteiro. Os temas usam uma combinação de arquivos CSS, um arquivo de capa opcional e um diretório de imagens opcional se forem necessárias imagens. O arquivo de capa controla a aparência visual dos controles de servidor ASP.NET.

## <a name="where-are-themes-stored"></a>Onde os temas são armazenados?

O local onde os temas são armazenados difere com base no seu escopo. Os temas que podem ser aplicados a qualquer aplicativo são armazenados na seguinte pasta:

`C:\WINDOWS\Microsoft.NET\Framework\v2.x.xxxxx\ASP.NETClientFiles\Themes\<Theme_Name>`

Um tema que é específico para um aplicativo específico é armazenado em um diretório `App\_Themes\<Theme\_Name>` na raiz do site da Web.

> [!NOTE]
> Um arquivo de capa deve modificar somente as propriedades de controle de servidor que afetam a aparência.

Um tema global é um tema que pode ser aplicado a qualquer aplicativo ou site em execução no servidor Web. Esses temas são armazenados por padrão no diretório ASP. NETClientfiles\Themes que está dentro do diretório v2. x. xxxxx. Como alternativa, você pode mover os arquivos de tema para a pasta\_/cliente/sistema\_Web/[versão]/Themes/[Theme\_Name] na raiz do seu site da Web.

Temas específicos do aplicativo só podem ser aplicados ao aplicativo no qual os arquivos residem. Esses arquivos são armazenados no diretório `App\_Themes/<theme\_name>` na raiz do site da Web.

## <a name="the-components-of-a-theme"></a>Os componentes de um tema

Um tema é composto de um ou mais arquivos CSS, um arquivo de capa opcional e uma pasta de imagens opcionais. Os arquivos CSS podem ser qualquer nome que você desejar (ou seja, Default. css ou Theme. CSS, etc.) e devem estar na raiz da pasta Themes. Os arquivos CSS são usados para definir classes CSS comuns e atributos para seletores específicos. Para aplicar uma das classes CSS a um elemento Page, a propriedade **CSSClass** é usada.

O arquivo de capa é um arquivo XML que contém definições de propriedade para controles de servidor ASP.NET. O código listado abaixo é um exemplo de arquivo de aparência.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample11.aspx)]

A **Figura 1** abaixo mostra uma pequena página de ASP.net navegada sem um tema aplicado. A **Figura 2** mostra o mesmo arquivo com um tema aplicado. A cor de fundo e a cor do texto são configuradas por meio de um arquivo CSS. A aparência do botão e da caixa de texto é configurada usando o arquivo de capa listado acima.

![Nenhum tema](profiles-themes-and-web-parts/_static/image1.gif)

**Figura 1**: nenhum tema

![Tema aplicado](profiles-themes-and-web-parts/_static/image2.gif)

**Figura 2**: tema aplicado

O arquivo de capa listado acima define uma aparência padrão para todos os controles de caixa de texto e controles de botão. Isso significa que todos os controles TextBox e Button inseridos em uma página terão essa aparência. Você também pode definir uma capa que pode ser aplicada a instâncias específicas desses controles usando a propriedade **SkinID** do controle.

O código a seguir define uma capa para um controle de botão. Somente os controles Button com uma propriedade **SkinID** de **goButton** assumirão a aparência da capa.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample12.aspx)]

Você só pode ter uma aparência padrão por tipo de controle de servidor. Se você precisar de capas adicionais, deverá usar a propriedade SkinID.

## <a name="applying-themes-to-pages"></a>Aplicando temas a páginas

Um tema pode ser aplicado usando qualquer um dos seguintes métodos:

- No elemento &lt;Pages&gt; do arquivo Web. config
- Na diretiva @Page de uma página
- Programaticamente

## <a name="applying-a-theme-in-the-configuration-file"></a>Aplicando um tema no arquivo de configuração

Para aplicar um tema no arquivo de configuração de aplicativos, use a seguinte sintaxe:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample13.xml)]

O nome do tema especificado aqui deve corresponder ao nome da pasta Themes. Essa pasta pode existir em qualquer um dos locais mencionados anteriormente neste curso. Se você tentar aplicar um tema que não existe, ocorrerá um erro de configuração.

## <a name="applying-a-theme-in-the-page-directive"></a>Aplicando um tema na diretiva Page

Você também pode aplicar um tema na diretiva @ Page. Esse método permite que você use um tema para uma página específica.

Para aplicar um tema na diretiva @Page, use a seguinte sintaxe:

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample14.aspx)]

Mais uma vez, o tema especificado aqui deve corresponder à pasta de tema mencionada anteriormente. Se você tentar aplicar um tema que não existe, ocorrerá uma falha de compilação. O Visual Studio também realçará o atributo e o notificará de que não existe nenhum tema.

## <a name="applying-a-theme-programmatically"></a>Aplicando um tema programaticamente

Para aplicar um tema programaticamente, você deve especificar a propriedade **Theme** para a página na **página\_método PreInit** .

Para aplicar um tema programaticamente, use a seguinte sintaxe:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample15.cs)]

É necessário aplicar o tema no método PreInit devido ao ciclo de vida da página. Se você aplicá-la após esse ponto, o tema páginas já terá sido aplicado pelo tempo de execução e uma alteração nesse ponto será muito tarde no ciclo de vida. Se você aplicar um tema que não existe, ocorrerá uma **HttpException** . Quando um tema é aplicado programaticamente, ocorrerá um aviso de compilação se algum controle de servidor tiver uma propriedade SkinID especificada. Esse aviso destina-se a informar que nenhum tema é aplicado declarativamente e pode ser ignorado.

## <a name="exercise-1--applying-a-theme"></a>Exercício 1: aplicando um tema

Neste exercício, você aplicará um tema ASP.NET a um site da Web.

> [!IMPORTANT]
> Se você estiver usando o Microsoft Word para inserir informações em um arquivo de capa, certifique-se de que você não está substituindo aspas normais por aspas inglesas. As aspas inteligentes causarão problemas com arquivos de capa.

1. Criar um site do ASP.NET.
2. Clique com o botão direito do mouse no projeto no Gerenciador de Soluções e escolha Adicionar novo item.
3. Escolha Arquivo de configuração da Web na lista de arquivos e clique em Adicionar.
4. Clique com o botão direito do mouse no projeto no Gerenciador de Soluções e escolha Adicionar novo item.
5. Escolha Arquivo de capa e clique em Adicionar.
6. Clique em Sim quando for perguntado se você deseja colocar o arquivo dentro da pasta de temas do aplicativo\_.
7. Clique com o botão direito do mouse na pasta Skinfile dentro da pasta de temas do aplicativo\_em Gerenciador de Soluções e escolha Adicionar novo item.
8. Escolha folha de estilos na lista de arquivos e clique em Adicionar. Agora você tem todos os arquivos necessários para implementar seu novo tema. No entanto, o Visual Studio nomeou sua pasta de temas de Capafile. Clique com o botão direito do mouse nessa pasta e altere o nome para CoolTheme.
9. Abra o arquivo Skinfile. Skin e adicione o seguinte código ao final do arquivo: 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample16.aspx)]
10. Salve o arquivo Skinfile. Skin.
11. Abra o StyleSheet. css.
12. Substitua todo o texto nele pelo seguinte: 

    [!code-css[Main](profiles-themes-and-web-parts/samples/sample17.css)]
13. Salve o arquivo StyleSheet. css.
14. Abra a página default. aspx.
15. Adicione um controle TextBox e um controle Button.
16. Salve a página. Agora, procure a página default. aspx. Ele deve ser exibido como um formulário da Web normal.
17. Abra o arquivo Web. config.
18. Adicione o seguinte diretamente abaixo da marca de `<system.web>` de abertura: 

    [!code-xml[Main](profiles-themes-and-web-parts/samples/sample18.xml)]
19. Salve o arquivo Web. config. Agora, procure a página default. aspx. Ele deve ser exibido com o tema aplicado.
20. Se ainda não estiver aberto, abra a página default. aspx no Visual Studio.
21. Selecione o botão.
22. Altere a propriedade **SkinID** para goButton. Observe que o Visual Studio fornece uma lista suspensa com valores SkinID válidos para um controle de botão.
23. Salve a página. Agora, visualize a página no navegador novamente. O botão agora deve dizer "Go" e deve ser mais largo na aparência.

Usando a propriedade **SkinID** , você pode facilmente configurar diferentes capas para diferentes instâncias de um determinado tipo de controle de servidor.

## <a name="the-stylesheettheme-property"></a>A propriedade StyleSheetTheme

Até agora, falamos apenas sobre a aplicação de temas usando a propriedade Theme. Ao usar a propriedade Theme, o arquivo de capa substituirá as configurações declarativas para controles de servidor. Por exemplo, no exercício 1, você especificou uma SkinID de "goButton" para o controle de botão e alterou o texto do botão para "Go". Você deve ter notado que a propriedade Text do botão no designer foi definida como "Button", mas o tema substituiu isso. O tema sempre substituirá as configurações de propriedade no designer.

Se você quiser ser capaz de substituir as propriedades definidas no arquivo de aparência do tema pelas propriedades especificadas no designer, poderá usar a propriedade **StyleSheetTheme** em vez da propriedade Theme. A propriedade StyleSheetTheme é igual à propriedade Theme, exceto pelo fato de que ela não substitui todas as configurações de propriedade explícitas, como a propriedade Theme.

Para ver isso em ação, abra o arquivo Web. config do projeto no exercício 1 e altere o elemento `<pages>` para o seguinte:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample19.xml)]

Agora, procure a página default. aspx e você verá que o controle de botão tem uma propriedade Text de "Button" novamente. Isso ocorre porque a configuração de propriedade explícita no designer está substituindo a propriedade Text definida pelo goButton SkinID.

## <a name="overriding-themes"></a>Substituindo temas

Um tema global pode ser substituído aplicando-se um tema com o mesmo nome na pasta de temas de\_de aplicativo do aplicativo. No entanto, o tema não é aplicado em um cenário de substituição verdadeiro. Se o tempo de execução encontrar arquivos de tema na pasta de temas do aplicativo\_, ele aplicará o tema usando esses arquivos e ignorará o tema global.

A propriedade StyleSheetTheme é substituível e pode ser substituída no código da seguinte maneira:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample20.cs)]

## <a name="web-parts"></a>Partes de Web

O ASP.NET Web Parts é um conjunto integrado de controles para a criação de sites que permitem aos usuários finais modificar o conteúdo, a aparência e o comportamento de páginas da Web diretamente de um navegador. As modificações podem ser aplicadas a todos os usuários no site ou a usuários individuais. Quando os usuários modificam páginas e controles, as configurações podem ser salvas para manter as preferências pessoais de um usuário em sessões futuras do navegador, um recurso chamado personalização. Esses Web Parts recursos significam que os desenvolvedores podem capacitar os usuários finais a personalizar um aplicativo Web dinamicamente, sem a intervenção do desenvolvedor ou do administrador.

Usando o conjunto de controle de Web Parts, você, como desenvolvedor, pode permitir que os usuários finais:

- Personalizar o conteúdo da página. Os usuários podem adicionar novos controles de Web Parts a uma página, removê-los, ocultá-los ou minimizá-los como janelas comuns.
- Personalize o layout da página. Os usuários podem arrastar um controle de Web Parts para uma zona diferente em uma página ou alterar sua aparência, propriedades e comportamento.
- Exportar e importar controles. Os usuários podem importar ou exportar Web Parts configurações de controle para uso em outras páginas ou sites, retendo as propriedades, a aparência e até mesmo os dados nos controles. Isso reduz a entrada de dados e as demandas de configuração dos usuários finais.
- Criar conexões. Os usuários podem estabelecer conexões entre controles para que, por exemplo, um controle de gráfico possa exibir um grafo para os dados em um controle de cotação de estoque. Os usuários podiam Personalizar não apenas a conexão em si, mas a aparência e os detalhes de como o controle de gráfico exibe os dados.
- Gerencie e personalize configurações de nível de site. Os usuários autorizados podem definir configurações de nível de site, determinar quem pode acessar um site ou página, definir o acesso baseado em função a controles e assim por diante. Por exemplo, um usuário em uma função administrativa pode definir um controle de Web Parts a ser compartilhado por todos os usuários e impedir que os usuários que não são administradores de personalizar o controle compartilhado.

Normalmente, você trabalhará com Web Parts de três maneiras: Criando páginas que usam controles de Web Parts, criando controles de Web Parts individuais ou criando aplicativos Web completos e personalizáveis, como um portal.

## <a name="page-development"></a>Desenvolvimento de página

Os desenvolvedores de páginas podem usar as ferramentas de Design Visual, como Microsoft Visual Studio 2005, para criar páginas que usam Web Parts. Uma vantagem de usar uma ferramenta como o Visual Studio é que o conjunto de controle de Web Parts fornece recursos para criação e configuração de arrastar e soltar de controles de Web Parts em um designer visual. Por exemplo, você pode usar o designer para arrastar uma zona Web Parts, ou um controle editor de Web Parts, até a superfície de design e, em seguida, configurar o controle diretamente no designer usando a interface do usuário fornecida pelo conjunto de controle de Web Parts. Isso pode agilizar o desenvolvimento de Web Parts aplicativos e reduzir a quantidade de código que você precisa escrever.

## <a name="control-development"></a>Desenvolvimento de controle

Você pode usar qualquer controle ASP.NET existente como um controle Web Parts, incluindo controles de servidor Web padrão, controles de servidor personalizados e controles de usuário. Para obter o máximo controle programático do seu ambiente, você também pode criar controles de Web Parts personalizados que derivam da classe WebPart. Para o desenvolvimento de controle de Web Parts individual, você normalmente criará um controle de usuário e o usará como um controle de Web Parts ou desenvolverá um controle de Web Parts personalizado.

Como exemplo de desenvolvimento de um controle de Web Parts personalizado, você pode criar um controle para fornecer qualquer um dos recursos fornecidos por outros controles de servidor ASP.NET que podem ser úteis para empacotar como um controle de Web Parts personalizável: calendários, listas, informações financeiras, Notícias, calculadoras, controles de Rich Text para atualizar o conteúdo, grades editáveis que se conectam a bancos de dados, gráficos que atualizam dinamicamente suas exibições ou informações de clima e viagem. Se você fornecer um designer visual com seu controle, qualquer desenvolvedor de página usando o Visual Studio poderá simplesmente arrastar o controle para uma Web Parts zona e configurá-lo em tempo de design sem precisar escrever código adicional.

A personalização é a base do recurso Web Parts. Ele permite que os usuários modifiquem, ou personalizem, o layout, a aparência e o comportamento dos controles de Web Parts em uma página. As configurações personalizadas são de longa duração: elas são mantidas não apenas durante a sessão atual do navegador (como com o estado de exibição), mas também no armazenamento de longo prazo, para que as configurações de um usuário sejam salvas para futuras sessões do navegador também. A personalização é habilitada por padrão para páginas Web Parts.

Os componentes estruturais da interface do usuário dependem da personalização e fornecem a estrutura principal e os serviços necessários para todos os controles de Web Parts. Um componente estrutural de interface do usuário necessário em cada página de Web Parts é o controle WebPartManager. Embora nunca esteja visível, esse controle tem a tarefa crítica de coordenar todos os controles de Web Parts em uma página. Por exemplo, ele controla todos os controles de Web Parts individuais. Ele gerencia zonas de Web Parts (regiões que contêm Web Parts controles em uma página) e quais controles estão em quais zonas. Ele também controla e controla os diferentes modos de exibição em que uma página pode estar, como procurar, conectar, editar ou o modo de catálogo, e se as alterações de personalização se aplicam a todos os usuários ou a usuários individuais. Por fim, ele inicia e rastreia conexões e comunicação entre controles de Web Parts.

O segundo tipo de componente estrutural da interface do usuário é a zona. As zonas atuam como gerenciadores de layout em uma página Web Parts. Eles contêm e organizam controles que derivam da classe Part (controles de parte) e fornecem a capacidade de fazer o layout de página modular na orientação horizontal ou vertical. As zonas também oferecem elementos de interface do usuário comuns e consistentes (como estilo de cabeçalho e rodapé, título, estilo de borda, botões de ação e assim por diante) para cada controle que eles contêm; esses elementos comuns são conhecidos como o Chrome de um controle. Vários tipos especializados de zonas são usados nos modos de exibição diferentes e com vários controles. Os diferentes tipos de zonas são descritos na seção Web Parts controles essenciais abaixo.

Os controles de interface do usuário Web Parts, todos derivados da classe **Part** , compõem a interface do usuário principal em uma página Web Parts. O conjunto de controle de Web Parts é flexível e inclusivo nas opções que ele fornece para a criação de controles de parte. Além de criar seus próprios controles de Web Parts personalizados, você também pode usar controles de servidor ASP.NET existentes, controles de usuário ou controles de servidor personalizados como Web Parts controles. Os controles essenciais que são mais comumente usados para criar Web Parts páginas são descritos na próxima seção.

## <a name="web-parts-essential-controls"></a>Web Parts controles essenciais

O conjunto de controle de Web Parts é extensivo, mas alguns controles são essenciais porque são necessários para que Web Parts funcionem ou porque são os controles usados com mais frequência em Web Parts páginas. Ao começar a usar Web Parts e criar páginas de Web Parts básicas, é útil estar familiarizado com os controles de Web Parts essenciais descritos na tabela a seguir.

| **Controle de Web Parts** | **Descrição** |
| --- | --- |
| WebPartManager | Gerencia todos os controles de Web Parts em uma página. Um (e apenas um) controle **WebPartManager** é necessário para cada página de Web Parts. |
| CatalogZone | Contém controles de CatalogPart. Use essa zona para criar um catálogo de Web Parts controles dos quais os usuários podem selecionar controles para adicionar a uma página. |
| EditorZone | Contém controles EditorPart. Use essa zona para permitir que os usuários editem e personalizem controles de Web Parts em uma página. |
| WebPartZone | Contém e fornece layout geral para os controles de WebPart que compõem a interface do usuário principal de uma página. Use essa zona sempre que criar páginas com controles de Web Parts. As páginas podem conter uma ou mais zonas. |
| ConnectionsZone | Contém controles WebPartConnection e fornece uma interface do usuário para o gerenciamento de conexões. |
| WebPart (GenericWebPart) | Renderiza a interface do usuário principal; a maioria Web Parts controles da interface do usuário se enquadram nessa categoria. Para o controle programático máximo, você pode criar controles de Web Parts personalizados que derivam do controle **WebPart** de base. Você também pode usar controles de servidor existentes, controles de usuário ou controles personalizados como Web Parts controles. Sempre que qualquer um desses controles é colocado em uma zona, o controle **WebPartManager** os encapsula automaticamente com controles **GenericWebPart** em tempo de execução para que você possa usá-los com Web Parts funcionalidade. |
| CatalogPart | Contém uma lista de controles de Web Parts disponíveis que os usuários podem adicionar à página. |
| WebPartConnection | Cria uma conexão entre dois controles de Web Parts em uma página. A conexão define um dos controles de Web Parts como um provedor (de dados) e o outro como um consumidor. |
| EditorPart | Serve como a classe base para os controles de editor especializados. |
| Controles EditorPart (AppearanceEditorPart, LayoutEditorPart, BehaviorEditorPart e PropertyGridEditorPart) | Permitir que os usuários personalizem vários aspectos de Web Parts controles de interface do usuário em uma página |

## <a name="lab-create-a-web-part-page"></a>Laboratório: criar uma página de Web Part

Neste laboratório, você criará uma página de Web Part que manterá informações por meio de perfis de ASP.NET.

### <a name="creating-a-simple-page-with-web-parts"></a>Criando uma página simples com Web Parts

Nesta parte, você cria uma página que usa Web Parts controles para mostrar conteúdo estático. A primeira etapa ao trabalhar com Web Parts é criar uma página com dois elementos estruturais necessários. Primeiro, uma página de Web Parts precisa de um controle WebPartManager para rastrear e coordenar todos os controles de Web Parts. Em segundo lugar, uma página de Web Parts precisa de uma ou mais zonas, que são controles compostos que contêm controles de WebPart ou de outros servidores e ocupam uma região especificada de uma página.

> [!NOTE]
> Você não precisa fazer nada para habilitar a personalização de Web Parts; Ele é habilitado por padrão para o conjunto de controle de Web Parts. Quando você executa pela primeira vez uma página Web Parts em um site, o ASP.NET configura um provedor de personalização padrão para armazenar as configurações de personalização do usuário. Para obter mais informações sobre personalização, consulte Visão geral de personalização de Web Parts.

### <a name="to-create-a-page-for-containing-web-parts-controls"></a>Para criar uma página para conter Web Parts controles

1. Feche a página padrão e adicione uma nova página ao site chamada WebPartsDemo. aspx.
2. Alterne para o modo de exibição de **design** .
3. No menu **Exibir** , verifique se os controles e as opções de **detalhes** **não visuais** estão selecionados para que você possa ver as marcas de layout e os controles que não têm uma interface do usuário.
4. Coloque o ponto de inserção antes das marcas de `<div>` na superfície de design e pressione ENTER para adicionar uma nova linha. Posicione o ponto de inserção antes do caractere de nova linha, clique no controle de lista suspensa **formato de bloco** no menu e selecione a opção **título 1** . No título, adicione o texto **Web Parts página de demonstração**.
5. Na guia **Web Parts** da caixa de ferramentas, arraste um controle **WebPartManager** para a página, posicionando-o logo após o caractere de nova linha e antes das marcas de `<div>`.   
  
   O controle **WebPartManager** não renderiza nenhuma saída, portanto aparece como uma caixa cinza na superfície do designer.
6. Posicione o ponto de inserção dentro das marcas de `<div>`.
7. No menu **layout** , clique em **Inserir Tabela**e crie uma nova tabela que tenha uma linha e três colunas. Clique no botão **Propriedades da célula** , selecione **superior** na lista suspensa **alinhamento vertical** , clique em **OK**e clique em **OK** novamente para criar a tabela.
8. Arraste um controle WebPartZone para a coluna da tabela esquerda. Clique com o botão direito do mouse no controle **WebPartZone** , escolha **Propriedades**e defina as seguintes propriedades:   
  
   ID: SidebarZone   
  
   HeaderText: barra lateral
9. Arraste um segundo controle **WebPartZone** para a coluna da tabela intermediária e defina as seguintes propriedades:   
  
   ID: MainZone   
  
   HeaderText: principal
10. Salve o arquivo.

Sua página agora tem duas zonas distintas que você pode controlar separadamente. No entanto, nenhuma das zonas tem qualquer conteúdo; portanto, a criação de conteúdo é a próxima etapa. Para este passo a passos, você trabalha com Web Parts controles que exibem apenas conteúdo estático.

O layout de uma zona de Web Parts é especificado por um elemento de&gt; &lt;ZoneTemplate. Dentro do modelo de zona, você pode adicionar qualquer controle ASP.NET, seja um controle de Web Parts personalizado, um controle de usuário ou um controle de servidor existente. Observe que você está usando o controle rótulo e, para que você esteja simplesmente adicionando texto estático. Quando você coloca um controle de servidor normal em uma zona **WebPartZone** , o ASP.NET trata o controle como um controle de Web Parts em tempo de execução, que habilita Web Parts recursos do controle.

**Para criar conteúdo para a zona principal**

1. No modo **design** , arraste um controle **rótulo** da guia **padrão** da caixa de ferramentas para a área de conteúdo da zona cuja propriedade **ID** está definida como MainZone.
2. Alternar para o modo de exibição de **código-fonte** . Observe que um elemento de&gt; de zona de &lt;foi adicionado para encapsular o controle de **rótulo** no MainZone.
3. Adicione um atributo chamado **title** ao elemento &lt;ASP: Label&gt; e defina seu valor como Content. Remova o atributo Text = "Label" do elemento &lt;ASP: Label&gt;. Entre as marcas de abertura e de fechamento do elemento &lt;ASP: Label&gt;, adicione algum texto, como **Bem-vindo à minha página inicial** dentro de um par de marcas de elemento &lt;H2&gt;. Seu código deve ter a seguinte aparência. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample21.aspx)]
4. Salve o arquivo.

Em seguida, crie um controle de usuário que também pode ser adicionado à página como um controle de Web Parts.

### <a name="to-create-a-user-control"></a>Para criar um controle de usuário

1. Adicione um novo controle de usuário da Web ao seu site para servir como um controle de pesquisa. Desmarque a opção para **posicionar o código-fonte em um arquivo separado**. Adicione-o no mesmo diretório que a página WebPartsDemo. aspx e nomeie-o SearchUserControl. ascx.   
  
    > [!NOTE]
    > O controle de usuário para este passo a passos não implementa a funcionalidade de pesquisa real; Ele é usado apenas para demonstrar Web Parts recursos.
2. Alterne para o modo de exibição de **design** . Na guia **padrão** da caixa de ferramentas, arraste um controle TextBox para a página.
3. Coloque o ponto de inserção após a caixa de texto que você acabou de adicionar e pressione ENTER para adicionar uma nova linha.
4. Arraste um controle de botão para a página na nova linha abaixo da caixa de texto que você acabou de adicionar.
5. Alternar para o modo de exibição de **código-fonte** . Verifique se o código-fonte do controle de usuário é semelhante ao exemplo a seguir. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample22.aspx)]
6. Salve e feche o arquivo.

Agora você pode adicionar Web Parts controles à zona da barra lateral. Você está adicionando dois controles à zona da barra lateral, um contendo uma lista de links e outro que é o controle de usuário que você criou no procedimento anterior. Os links são adicionados como um controle de servidor de **rótulo** padrão, semelhante ao modo como você criou o texto estático para a zona principal. No entanto, embora os controles de servidor individuais contidos no controle de usuário possam estar contidos diretamente na zona (como o controle rótulo), nesse caso, eles não são. Em vez disso, eles fazem parte do controle de usuário que você criou no procedimento anterior. Isso demonstra uma maneira comum de empacotar quaisquer controles e funcionalidades adicionais que você desejar em um controle de usuário e, em seguida, fazer referência a esse controle em uma zona como um controle de Web Parts.

Em tempo de execução, o conjunto de controle de Web Parts encapsula ambos os controles com controles GenericWebPart. Quando um controle **GenericWebPart** encapsula um controle de servidor Web, o controle de parte genérico é o controle pai e você pode acessar o controle de servidor por meio da propriedade ChildControl do controle pai. Esse uso de controles de parte genéricos permite que controles de servidor Web padrão tenham o mesmo comportamento e atributos básicos que Web Parts controles que derivam da classe **WebPart** .

### <a name="to-add-web-parts-controls-to-the-sidebar-zone"></a>Para adicionar controles de Web Parts à zona da barra lateral

1. Abra a página WebPartsDemo. aspx.
2. Alterne para o modo de exibição de **design** .
3. Arraste a página de controle de usuário que você criou, SearchUserControl. ascx, de **Gerenciador de soluções** para a zona cuja propriedade **ID** está definida como SidebarZone e solte-a lá.
4. Salve a página WebPartsDemo. aspx.
5. Alternar para o modo de exibição de **código-fonte** .
6. Dentro do elemento &lt;ASP: WebPartZone&gt; para o SidebarZone, logo acima da referência ao seu controle de usuário, adicione um elemento &lt;ASP: Label&gt; com links contidos, conforme mostrado no exemplo a seguir. Além disso, adicione um atributo **title** à marca de controle de usuário, com um valor de **pesquisa**, conforme mostrado. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample23.aspx)]
7. Salve e feche o arquivo.

Agora você pode testar sua página navegando até ela no navegador. A página exibe as duas zonas. A captura de tela a seguir mostra a página.

**Web Parts página de demonstração com duas zonas**

![Captura de tela do Web Parts VS passo 1](profiles-themes-and-web-parts/_static/image3.gif)

**Figura 3**: captura de tela do Web Parts do vs passo 1

Na barra de título de cada controle há uma seta para baixo que fornece acesso a um menu de verbos das ações disponíveis que você pode executar em um controle. Clique no menu de verbos de um dos controles e, em seguida, clique no verbo **minimizar** e observe que o controle é minimizado. No menu de verbos, clique em **restaurar**e o controle retorna ao seu tamanho normal.

### <a name="enabling-users-to-edit-pages-and-change-layout"></a>Habilitando os usuários a editar páginas e alterar o layout

Web Parts fornece a capacidade para os usuários alterarem o layout dos controles de Web Parts arrastando-os de uma zona para outra. Além de permitir que os usuários movam controles de **WebPart** de uma zona para outra, você pode permitir que os usuários editem várias características dos controles, incluindo a aparência, o layout e o comportamento. O conjunto de controle de Web Parts fornece funcionalidade de edição básica para controles de **WebPart** . Embora você não faça isso neste passo a passos, você também pode criar controles de editor personalizados que permitem aos usuários editar os recursos dos controles de **WebPart** . Assim como alterar o local de um controle **WebPart** , editar as propriedades de um controle depende da personalização de ASP.net para salvar as alterações feitas pelos usuários.

Nesta parte do tutorial, você adiciona a capacidade para os usuários editarem as características básicas de qualquer controle **WebPart** na página. Para habilitar esses recursos, você adiciona outro controle de usuário personalizado à página, juntamente com um elemento &lt;ASP: EditorZone&gt; e dois controles de edição.

### <a name="to-create-a-user-control-that-enables-changing-page-layout"></a>Para criar um controle de usuário que permite alterar o layout da página

1. No Visual Studio, no menu **arquivo** , selecione o **novo** submenu e clique na opção **arquivo** .
2. Na caixa de diálogo **Adicionar novo item** , selecione **controle de usuário da Web**. Nomeie o novo arquivo DisplayModeMenu. ascx. Desmarque a opção para **posicionar o código-fonte em um arquivo separado**.
3. Clique em Adicionar para criar o novo controle.
4. Alternar para o modo de exibição de **código-fonte** .
5. Remova todo o código existente no novo arquivo e cole o código a seguir. Esse código de controle de usuário usa recursos do conjunto de controle de Web Parts que permitem que uma página altere sua exibição ou modo de exibição, além de permitir que você altere a aparência física e o layout da página enquanto estiver em determinados modos de exibição. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample24.aspx)]
6. Salve o arquivo clicando no ícone salvar na barra de ferramentas ou selecionando **salvar** no menu **arquivo** .

### <a name="to-enable-users-to-change-the-layout"></a>Para permitir que os usuários alterem o layout

1. Abra a página WebPartsDemo. aspx e alterne para o modo de exibição de **design** .
2. Posicione o ponto de inserção no modo de exibição de **design** logo após o controle **WebPartManager** que você adicionou anteriormente. Adicione um retorno forçado após o texto para que haja uma linha em branco após o controle **WebPartManager** . Coloque o ponto de inserção na linha em branco.
3. Arraste o controle de usuário que você acabou de criar (o arquivo é denominado DisplayModeMenu. ascx) na página WebPartsDemo. aspx e solte-o na linha em branco.
4. Arraste um controle EditorZone da seção Web **Parts** da caixa de ferramentas para a célula aberta da tabela restante na página WebPartsDemo. aspx.
5. Na seção **Web Parts** da caixa de ferramentas, arraste um controle AppearanceEditorPart e um controle LayoutEditorPart para o controle **EditorZone** .
6. Alternar para o modo de exibição de **código-fonte** . O código resultante na célula da tabela deve ser semelhante ao código a seguir. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample25.aspx)]
7. Salve o arquivo WebPartsDemo. aspx. Você criou um controle de usuário que permite alterar modos de exibição e alterar o layout da página, e você referenciou o controle na página da Web principal.

Agora você pode testar a capacidade de editar páginas e alterar o layout.

### <a name="to-test-layout-changes"></a>Para testar alterações de layout

1. Carregue a página em um navegador.
2. Clique no menu suspenso **modo de exibição** e selecione **Editar**. Os títulos de zona são exibidos.
3. Arraste o controle **meus links** por sua barra de título da zona da barra lateral até a parte inferior da zona principal. Sua página deve ser semelhante à captura de tela a seguir.

### <a name="web-parts-demo-page-with-my-links-control-moved"></a>Web Parts página de demonstração com meu controle de links movido

![Captura de tela do Web Parts VS Walkthrough 2](profiles-themes-and-web-parts/_static/image4.gif)

**Figura 4**: captura de tela do Web Parts vs Walkthrough 2

1. Clique no menu suspenso **modo de exibição** e selecione **procurar**. A página é atualizada, os nomes de zona desaparecem e o controle **meus links** permanece onde você o posicionou.
2. Para demonstrar que a personalização está funcionando, feche o navegador e, em seguida, carregue a página novamente. As alterações feitas são salvas para futuras sessões do navegador.
3. No menu **modo de exibição** , selecione **Editar**.   
  
   Agora, cada controle na página é exibido com uma seta para baixo na barra de título, que contém o menu suspenso verbos.
4. Clique na seta para exibir o menu de verbos no controle **meus links** . Clique no verbo **Editar** .   
  
   O controle **EditorZone** é exibido, exibindo os controles EditorPart que você adicionou.
5. Na seção **aparência** do controle de edição, altere o **título** para meus favoritos, use a lista suspensa **tipo de Chrome** para selecionar **título apenas**e clique em **aplicar**. A captura de tela a seguir mostra a página no modo de edição.

### <a name="web-parts-demo-page-in-edit-mode"></a>Web Parts página de demonstração no modo de edição

![Captura de tela do Web Parts VS Walkthrough 3](profiles-themes-and-web-parts/_static/image5.gif)

**Figura 5**: captura de tela do Web Parts com a orientação 3

1. Clique no menu **modo de exibição** e selecione **procurar** para retornar ao modo de procura.
2. O controle agora tem um título atualizado e nenhuma borda, conforme mostrado na captura de tela a seguir.

### <a name="edited-web-parts-demo-page"></a>Página de demonstração de Web Parts editada

![Captura de tela do Web Parts VS passo 4](profiles-themes-and-web-parts/_static/image6.gif)

**Figura 4**: captura de tela do Web Parts com a orientação 4

### <a name="adding-web-parts-at-run-time"></a>Adicionando Web Parts em tempo de execução

Você também pode permitir que os usuários adicionem Web Parts controles à sua página em tempo de execução. Para fazer isso, configure a página com um catálogo Web Parts, que contém uma lista de controles de Web Parts que você deseja disponibilizar para os usuários.

**Para permitir que os usuários adicionem Web Parts em tempo de execução**

1. Abra a página WebPartsDemo. aspx e alterne para o modo de exibição de **design** .
2. Na guia **Web Parts** da caixa de ferramentas, arraste um controle CatalogZone para a coluna direita da tabela, abaixo do controle **EditorZone** .   
  
   Ambos os controles podem estar na mesma célula de tabela porque eles não serão exibidos ao mesmo tempo.
3. No painel Propriedades, atribua a cadeia de caracteres **adicionar Web Parts** à propriedade HeaderText do controle **CatalogZone** .
4. Na seção **Web Parts** da caixa de ferramentas, arraste um controle DeclarativeCatalogPart para a área de conteúdo do controle **CatalogZone** .
5. Clique na seta no canto superior direito do controle **DeclarativeCatalogPart** para expor seu menu tarefas e selecione **editar modelos**.
6. Na seção **padrão** da caixa de ferramentas, arraste um controle **FileUpload** e um controle **Calendar** para a seção **WebParts** do controle **DeclarativeCatalogPart** .
7. Alternar para o modo de exibição de **código-fonte** . Inspecione o código-fonte do elemento &lt;ASP: CatalogZone&gt;. Observe que o controle **DeclarativeCatalogPart** contém um elemento &lt;webparttemplate&gt; com os dois controles de servidor incluídos que você poderá adicionar à sua página do catálogo.
8. Adicione uma propriedade **title** a cada um dos controles que você adicionou ao catálogo, usando o valor de cadeia de caracteres mostrado para cada título no exemplo de código abaixo. Embora o título não seja uma propriedade que você normalmente pode definir nesses dois controles de servidor em tempo de design, quando um usuário adiciona esses controles a uma zona **WebPartZone** a partir do catálogo em tempo de execução, eles são cada um encapsulado com um controle **GenericWebPart** . Isso permite que eles atuem como Web Parts controles, portanto, poderão exibir títulos.   
  
   O código para os dois controles contidos no controle **DeclarativeCatalogPart** deve ser semelhante ao seguinte. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample26.aspx)]
9. Salve a página.

Agora você pode testar o catálogo.

### <a name="to-test-the-web-parts-catalog"></a>Para testar o catálogo de Web Parts

1. Carregue a página em um navegador.
2. Clique no menu suspenso **modo de exibição** e selecione **Catálogo**.   
  
   O catálogo intitulado **Add Web Parts** é exibido.
3. Arraste o controle **meus favoritos** da zona principal de volta para a parte superior da zona da barra lateral e solte-o lá.
4. No catálogo **adicionar Web Parts** , marque ambas as caixas de seleção e, em seguida, selecione **principal** na lista suspensa que contém as zonas disponíveis.
5. Clique em **Adicionar** no catálogo. Os controles são adicionados à zona principal. Se desejar, você pode adicionar várias instâncias de controles do catálogo à sua página.   
  
   A captura de tela a seguir mostra a página com o controle de carregamento de arquivo e o calendário na zona principal. 

![Controles adicionados à zona principal do catálogo](profiles-themes-and-web-parts/_static/image7.gif)

    **Figure 5**: Controls added to Main zone from the catalog
6. Clique no menu suspenso **modo de exibição** e selecione **procurar**. O catálogo desaparece e a página é atualizada.
7. Feche o navegador. Carregue a página novamente. As alterações que você fez são mantidas.
