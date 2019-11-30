---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
title: Criar a camada de acesso a dados | Microsoft Docs
author: Erikre
description: Esta série de tutoriais ensinará as noções básicas da criação de um aplicativo ASP.NET Web Forms usando o ASP.NET 4,5 e o Microsoft Visual Studio Express 2013 para nós...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 0bbf7a6e-d7eb-4091-91e4-fff892777f32
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
msc.type: authoredcontent
ms.openlocfilehash: 0fcf050474a57be9ed53ec0783a6d6b7dde2bf4c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74575745"
---
# <a name="create-the-data-access-layer"></a>Criar a camada de acesso a dados

por [Erik Reitan](https://github.com/Erikre)

[Baixar o projeto de exemplo WingtipC#Toys ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [baixar o livro eletrônico (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta série de tutoriais ensinará as noções básicas da criação de um aplicativo ASP.NET Web Forms usando o ASP.NET 4,5 e o Microsoft Visual Studio Express 2013 para a Web. Um [projeto Visual Studio 2013 com C# código-fonte](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) está disponível para acompanhar esta série de tutoriais.

Este tutorial descreve como criar, acessar e examinar dados de um banco de dado usando o ASP.NET Web Forms e Entity Framework Code First. Este tutorial se baseia no tutorial anterior "criar o projeto" e faz parte da série de tutoriais da loja Wingtip Toy. Após concluir este tutorial, você terá criado um grupo de classes de acesso a dados que estão na pasta *modelos* do projeto.

## <a name="what-youll-learn"></a>O que você aprenderá:

- Como criar modelos de dados.
- Como inicializar e propagar o banco de dados.
- Como atualizar e configurar o aplicativo para dar suporte ao banco de dados.

### <a name="these-are-the-features-introduced-in-the-tutorial"></a>Estes são os recursos apresentados no tutorial:

- Entity Framework Code First
- LocalDB
- Anotações de dados

## <a name="creating-the-data-models"></a>Criando os modelos de dados

[Entity Framework](https://msdn.microsoft.com/data/aa937723) é uma estrutura de ORM (mapeamento relacional de objeto). Ele permite que você trabalhe com dados relacionais como objetos, eliminando a maior parte do código de acesso a dados que normalmente seria necessário escrever. Usando Entity Framework, você pode emitir consultas usando [LINQ](https://msdn.microsoft.com/library/bb397926.aspx)e, em seguida, recuperar e manipular dados como objetos fortemente tipados. O LINQ fornece padrões para consultar e atualizar dados. O uso de Entity Framework permite que você se concentre na criação do restante do seu aplicativo, em vez de concentrar-se nos conceitos básicos de acesso a dados. Posteriormente nesta série de tutoriais, mostraremos como usar os dados para popular a navegação e as consultas de produtos.

O Entity Framework dá suporte a um paradigma de desenvolvimento chamado *Code First*. Code First permite que você defina seus modelos de dados usando classes. Uma classe é uma construção que permite que você crie seus próprios tipos personalizados agrupando variáveis de outros tipos, métodos e eventos. Você pode mapear classes para um banco de dados existente ou usá-las para gerar um banco de dados. Neste tutorial, você criará os modelos de dados escrevendo classes de modelo de dados. Em seguida, você permitirá Entity Framework criar o banco de dados instantaneamente a partir dessas novas classes.

Você começará criando as classes de entidade que definem os modelos de dados para o aplicativo Web Forms. Em seguida, você criará uma classe de contexto que gerencia as classes de entidade e fornece acesso a dados ao banco de dado. Você também criará uma classe de inicializador que será usada para popular o banco de dados.

### <a name="entity-framework-and-references"></a>Entity Framework e referências

Por padrão, Entity Framework é incluído quando você cria um novo **aplicativo Web ASP.net** usando o modelo de **Web Forms** . Entity Framework pode ser instalado, desinstalado e atualizado como um pacote NuGet.

Este pacote NuGet inclui os seguintes assemblies de **tempo de execução** em seu projeto:

- EntityFramework. dll – todo o código de tempo de execução comum usado pelo Entity Framework
- EntityFramework. SqlServer. dll – o provedor de Microsoft SQL Server para Entity Framework

### <a name="entity-classes"></a>Classes de entidade

As classes que você cria para definir o esquema dos dados são chamadas de classes de entidade. Se você for novo no design de banco de dados, considere as classes de entidade como definições de tabela de um banco de dados. Cada propriedade na classe especifica uma coluna na tabela do banco de dados. Essas classes fornecem uma interface leve e relacional de objeto entre o código orientado a objeto e a estrutura de tabela relacional do banco de dados.

Neste tutorial, você começará adicionando classes de entidade simples representando os esquemas para produtos e categorias. A classe Products conterá definições para cada produto. O nome de cada um dos membros da classe Product será `ProductID`, `ProductName`, `Description`, `ImagePath`, `UnitPrice`, `CategoryID`e `Category`. A classe Category conterá definições para cada categoria à qual um produto pode pertencer, como carro, barco ou avião. O nome de cada um dos membros da classe Category será `CategoryID`, `CategoryName`, `Description`e `Products`. Cada produto pertencerá a uma das categorias. Essas classes de entidade serão adicionadas à pasta *modelos* existentes do projeto.

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse na pasta *modelos* e, em seguida, selecione **Adicionar** -&gt; **novo item**. 

    ![Criar a camada de acesso a dados – menu novo item](create_the_data_access_layer/_static/image1.png)

   A caixa de diálogo **Adicionar Novo Item** é exibida.
2. Em **Visual C#**  no painel **instalado** à esquerda, selecione **código**. 

    ![Criar a camada de acesso a dados – menu novo item](create_the_data_access_layer/_static/image2.png)
3. Selecione **classe** no painel central e nomeie essa nova classe *Product.cs*.
4. Clique em **Adicionar**.  
   O novo arquivo de classe é exibido no editor.
5. Substitua o código padrão pelo código a seguir:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample1.cs)]
6. Crie outra classe repetindo as etapas 1 a 4, no entanto, nomeie a nova classe *Category.cs* e substitua o código padrão pelo código a seguir:  

    [!code-csharp[Main](create_the_data_access_layer/samples/sample2.cs)]

Como mencionado anteriormente, a classe `Category` representa o tipo de produto que o aplicativo foi projetado para vender (como <a id="a"></a>&quot;carros&quot;, &quot;Boats&quot;, &quot;rockets&quot;e assim por diante) e a classe `Product` representa os produtos individuais (Toys) no banco de dados. Cada instância de um objeto de `Product` corresponderá a uma linha dentro de uma tabela de banco de dados relacional e cada propriedade da classe Product será mapeada para uma coluna na tabela de banco de dados relacional. Posteriormente neste tutorial, você examinará os dados do produto contidos no banco de dado.

### <a name="data-annotations"></a>Anotações de dados

Talvez você tenha notado que determinados membros das classes têm atributos especificando detalhes sobre o membro, como `[ScaffoldColumn(false)]`. Essas são *anotações de dados*. Os atributos de anotação de dados podem descrever como validar a entrada do usuário para esse membro, especificar a formatação para ele e especificar como ele é modelado quando o banco de dados é criado.

### <a name="context-class"></a>Classe Context

Para começar a usar as classes de acesso a dados, você deve definir uma classe de contexto. Conforme mencionado anteriormente, a classe de contexto gerencia as classes de entidade (como a classe `Product` e a classe `Category`) e fornece acesso a dados ao banco de dado.

Este procedimento adiciona uma nova C# classe de contexto à pasta *modelos* .

1. Clique com o botão direito do mouse na pasta *modelos* e selecione **Adicionar** -&gt; **novo item**.   
   A caixa de diálogo **Adicionar Novo Item** é exibida.
2. Selecione **classe** no painel central, nomeie-a *ProductContext.cs* e clique em **Adicionar**.
3. Substitua o código padrão contido na classe pelo código a seguir:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample3.cs)]

Esse código adiciona o namespace `System.Data.Entity` para que você tenha acesso a toda a funcionalidade básica do Entity Framework, que inclui a capacidade de consultar, inserir, atualizar e excluir dados trabalhando com objetos fortemente tipados.

A classe `ProductContext` representa Entity Framework contexto de banco de dados do produto, que lida com a busca, o armazenamento e a atualização de `Product` instâncias de classe no banco de dados. A classe `ProductContext` deriva da classe base `DbContext` fornecida pelo Entity Framework.

### <a name="initializer-class"></a>Classe do inicializador

Você precisará executar alguma lógica personalizada para inicializar o banco de dados na primeira vez em que o contexto for usado. Isso permitirá que os dados de propagação sejam adicionados ao banco de dado para que você possa exibir imediatamente os produtos e as categorias.

Este procedimento adiciona uma nova C# classe de inicializador à pasta *modelos* .

1. Crie outro `Class` na pasta *modelos* e nomeie-o *ProductDatabaseInitializer.cs*.
2. Substitua o código padrão contido na classe pelo código a seguir:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample4.cs)]

Como você pode ver no código acima, quando o banco de dados é criado e inicializado, a propriedade `Seed` é substituída e definida. Quando a propriedade `Seed` é definida, os valores das categorias e dos produtos são usados para popular o banco de dados. Se você tentar atualizar os dados de semente modificando o código acima depois que o banco de dado tiver sido criado, você não verá nenhuma atualização ao executar o aplicativo Web. O motivo é que o código acima usa uma implementação da classe `DropCreateDatabaseIfModelChanges` para reconhecer se o modelo (esquema) foi alterado antes de redefinir os dados semente. Se nenhuma alteração for feita nas classes de entidade `Category` e `Product`, o banco de dados não será reinicializado com os dados de semente.

> [!NOTE] 
> 
> Se você quisesse que o banco de dados fosse recriado toda vez que executou o aplicativo, você poderia usar a classe `DropCreateDatabaseAlways` em vez da classe `DropCreateDatabaseIfModelChanges`. No entanto, para esta série de tutoriais, use a classe `DropCreateDatabaseIfModelChanges`.

Neste ponto deste tutorial, você terá uma pasta *modelos* com quatro novas classes e uma classe padrão:

![Criar a pasta camada de acesso a dados-modelos](create_the_data_access_layer/_static/image3.png)

### <a name="configuring-the-application-to-use-the-data-model"></a>Configurando o aplicativo para usar o modelo de dados

Agora que você criou as classes que representam os dados, deve configurar o aplicativo para usar as classes. No arquivo *global. asax* , você adiciona um código que inicializa o modelo. No arquivo *Web. config* , você adiciona informações que dizem ao aplicativo qual banco de dados você usará para armazenar o dado que é representado pelas novas classes de dados. O arquivo *global. asax* pode ser usado para manipular eventos ou métodos de aplicativo. O arquivo *Web. config* permite que você controle a configuração do seu aplicativo Web ASP.net.

#### <a name="updating-the-globalasax-file"></a>Atualizando o arquivo global. asax

Para inicializar os modelos de dados quando o aplicativo for iniciado, você atualizará o manipulador de `Application_Start` no arquivo *global.asax.cs* .

> [!NOTE] 
> 
> No Gerenciador de Soluções, você pode selecionar o arquivo *global. asax* ou o arquivo *global.asax.cs* para editar o arquivo *global.asax.cs* .

1. Adicione o código a seguir realçado em amarelo para o método `Application_Start` no arquivo *global.asax.cs* .   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample5.cs?highlight=9-10,22-23)]

> [!NOTE] 
> 
> Seu navegador deve dar suporte a HTML5 para exibir o código realçado em amarelo ao exibir esta série de tutoriais em um navegador.

Conforme mostrado no código acima, quando o aplicativo é iniciado, o aplicativo especifica o inicializador que será executado durante a primeira vez que os dados são acessados. Os dois namespaces adicionais são necessários para acessar o objeto `Database` e o objeto `ProductDatabaseInitializer`.

 Modificando o arquivo Web. config 

Embora Entity Framework Code First gere um banco de dados para você em um local padrão quando o banco de dado é populado com data de propagação, adicionar suas próprias informações de conexão ao seu aplicativo lhe dá controle do local do banco de dados. Você especifica essa conexão de banco de dados usando uma cadeia de conexão no arquivo *Web. config* do aplicativo na raiz do projeto. Ao adicionar uma nova cadeia de conexão, você pode direcionar o local do banco de dados (*wingtiptoys. MDF*) para que ele seja compilado no diretório data do aplicativo (*dados de\_do aplicativo*), em vez de seu local padrão. Fazer essa alteração permitirá que você encontre e inspecione o arquivo de banco de dados mais adiante neste tutorial.

1. Em **Gerenciador de soluções**, localize e abra o arquivo *Web. config* .
2. Adicione a cadeia de conexão realçada em amarelo à seção `<connectionStrings>` do arquivo *Web. config* da seguinte maneira:  

    [!code-xml[Main](create_the_data_access_layer/samples/sample6.xml?highlight=4-7)]

Quando o aplicativo for executado pela primeira vez, ele criará o banco de dados no local especificado pela cadeia de conexão. Mas antes de executar o aplicativo, vamos compilá-lo primeiro.

## <a name="building-the-application"></a>Compilando o aplicativo

Para garantir que todas as classes e alterações em seu aplicativo Web funcionem, você deve compilar o aplicativo.

1. No menu **depurar** , selecione **criar WingtipToys**.  
 A janela **saída** é exibida e, se tudo der certo, você verá uma mensagem bem- *sucedida* .  

    ![Criar a camada de acesso a dados-janelas de saída](create_the_data_access_layer/_static/image4.png)

Se você encontrar um erro, verifique novamente as etapas acima. As informações na janela de **saída** indicarão qual arquivo tem um problema e em que local o arquivo uma alteração é necessária. Essas informações permitirão que você determine qual parte das etapas acima precisa ser revisada e corrigida em seu projeto.

## <a name="summary"></a>Resumo

Neste tutorial da série, você criou o modelo de dados, bem como, adicionou o código que será usado para inicializar e propagar o banco de dado. Você também configurou o aplicativo para usar os modelos de dados quando o aplicativo é executado.

No próximo tutorial, você atualizará a interface do usuário, adicionará a navegação e recuperará dados do banco. Isso fará com que o banco de dados seja criado automaticamente com base nas classes de entidade que você criou neste tutorial.

## <a name="additional-resources"></a>Recursos adicionais

[Visão geral de Entity Framework](https://msdn.microsoft.com/library/bb399567.aspx)   
[Guia para iniciantes do ADO.NET Entity Framework](https://msdn.microsoft.com/data/ee712907)   
[Desenvolvimento de Code First com Entity Framework](http://www.msteched.com/2010/Europe/DEV212) (vídeo)   
[API fluente de relações de Code First](https://msdn.microsoft.com/data/hh134698)   
[Code First anotações de dados](https://msdn.microsoft.com/data/gg193958)  
[Melhorias de produtividade para o Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0)

> [!div class="step-by-step"]
> [Anterior](create-the-project.md)
> [Próximo](ui_and_navigation.md)
