---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: Injeção de dependência do MVC 4 do ASP.NET | Microsoft Docs
author: rick-anderson
description: 'Observação: este laboratório prático pressupõe que você tenha conhecimento básico dos filtros ASP.NET MVC e ASP.NET MVC 4. Se você não usou os filtros do ASP.NET MVC 4 antes, nós...'
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 15c9d4dcb9e2c6b9f6adf54d65d15737b32cca3b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78560609"
---
# <a name="aspnet-mvc-4-dependency-injection"></a>Injeção de dependência do ASP.NET MVC 4

por [equipe de acampamentos da Web](https://twitter.com/webcamps)

[Baixe o kit de treinamento do Web acampamentos](https://aka.ms/webcamps-training-kit)

Este laboratório prático pressupõe que você tenha conhecimento básico dos filtros **ASP.NET MVC** e **ASP.NET MVC 4**. Se você não usou os **filtros do ASP.NET MVC 4** antes, recomendamos que você vá para o laboratório prático de **filtros de ação personalizada do ASP.NET MVC** .

> [!NOTE]
> Todos os códigos de exemplo e trechos de código estão incluídos no Web acampamentos Training Kit, disponível em [Microsoft-Web/WebCampTrainingKit releases](https://aka.ms/webcamps-training-kit). O projeto específico deste laboratório está disponível na [injeção de dependência ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).

No paradigma de **programação orientada a objeto** , os objetos trabalham juntos em um modelo de colaboração onde há colaboradores e consumidores. Naturalmente, esse modelo de comunicação gera dependências entre objetos e componentes, tornando-se difícil de gerenciar quando a complexidade aumenta.

![Dependências de classe e complexidade do modelo](aspnet-mvc-4-dependency-injection/_static/image1.png "Dependências de classe e complexidade do modelo")

*Dependências de classe e complexidade do modelo*

Você provavelmente já ouviu falar do **padrão de fábrica** e da separação entre a interface e a implementação usando serviços, em que os objetos de cliente são frequentemente responsáveis pelo local do serviço.

O padrão de injeção de dependência é uma implementação específica de inversão de controle. A **inversão de controle (IOC)** significa que os objetos não criam outros objetos nos quais eles dependem para realizar seu trabalho. Em vez disso, eles obtêm os objetos de que precisam de uma fonte externa (por exemplo, um arquivo de configuração XML).

**Injeção de dependência (di)** significa que isso é feito sem a intervenção do objeto, geralmente por um componente de estrutura que passa parâmetros de construtor e define propriedades.

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a>O padrão de design de injeção de dependência (DI)

Em um alto nível, o objetivo da injeção de dependência é que uma classe de cliente (por exemplo, *o jogador*) precisa de algo que satisfaça uma interface (por exemplo, *IClub*). Não importa qual é o tipo concreto (por exemplo *, WoodClub, IronClub, WedgeClub* ou *PutterClub*), quer que outra pessoa manipule isso (por exemplo, um bom *Caddy*). O resolvedor de dependência no MVC ASP.NET pode permitir que você registre sua lógica de dependência em outro lugar (por exemplo, um contêiner ou um recipiente *de paus*).

![Diagrama de injeção de dependência](aspnet-mvc-4-dependency-injection/_static/image2.png "Ilustração de injeção de dependência")

*Injeção de dependência-analogia de golfe*

As vantagens de usar o padrão de injeção de dependência e a inversão de controle são as seguintes:

- Reduz o acoplamento de classe
- Aumenta a reutilização de código
- Melhora a manutenção do código
- Melhora o teste de aplicativos

> [!NOTE]
> A injeção de dependência às vezes é comparada com o padrão de design de fábrica abstrato, mas há uma pequena diferença entre ambas as abordagens. DI tem uma estrutura que está trabalhando para resolver dependências chamando as fábricas e os serviços registrados.

Agora que você entende o padrão de injeção de dependência, aprenderá em todo este laboratório como aplicá-lo no ASP.NET MVC 4. Você começará a usar a injeção de dependência nos **controladores** para incluir um serviço de acesso ao banco de dados. Em seguida, você aplicará injeção de dependência às **exibições** para consumir um serviço e mostrar informações. Por fim, você estenderá os filtros DI para ASP.NET MVC 4, injetando um filtro de ação personalizado na solução.

Neste laboratório prático, você aprenderá a:

- Integrar o ASP.NET MVC 4 ao Unity para injeção de dependência usando pacotes NuGet
- Usar injeção de dependência dentro de um controlador MVC ASP.NET
- Usar injeção de dependência dentro de uma exibição do ASP.NET MVC
- Usar injeção de dependência dentro de um filtro de ação do ASP.NET MVC

> [!NOTE]
> Este laboratório está usando o pacote NuGet do Unity. Mvc3 para resolução de dependência, mas é possível adaptar qualquer estrutura de injeção de dependência para trabalhar com o ASP.NET MVC 4.

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisites

Você deve ter os seguintes itens para concluir este laboratório:

- [Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou superior (leia [o apêndice a](#AppendixA) para obter instruções sobre como instalá-lo).

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Instalação

**Instalando trechos de código**

Para sua conveniência, grande parte do código que você gerenciará ao longo deste laboratório está disponível como trechos de código do Visual Studio. Para instalar os trechos de código, execute o arquivo **.\Source\Setup\CodeSnippets.VSI** .

Se você não estiver familiarizado com os trechos de Visual Studio Code e quiser saber como usá-los, consulte o apêndice deste documento &quot;[Apêndice B: usando trechos de código](#AppendixB)&quot;.

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Exercícios

Este laboratório prático é composto pelos seguintes exercícios:

1. [Exercício 1: injetando um controlador](#Exercise1)
2. [Exercício 2: injetando uma exibição](#Exercise2)
3. [Exercício 3: injetando filtros](#Exercise3)

> [!NOTE]
> Cada exercício é acompanhado por uma pasta **final** que contém a solução resultante que você deve obter depois de concluir os exercícios. Você pode usar essa solução como um guia se precisar de ajuda adicional para trabalhar com os exercícios.

Tempo estimado para concluir este laboratório: **30 minutos**.

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a>Exercício 1: injetando um controlador

Neste exercício, você aprenderá a usar a injeção de dependência em controladores MVC do ASP.NET integrando o Unity usando um pacote NuGet. Por esse motivo, você incluirá serviços em seus controladores MvcMusicStore para separar a lógica do acesso a dados. Os serviços criarão uma nova dependência no construtor do controlador, que será resolvida usando injeção de dependência com a ajuda do **Unity**.

Essa abordagem lhe mostrará como gerar aplicativos menos acoplados, que são mais flexíveis e fáceis de manter e testar. Você também aprenderá a integrar o ASP.NET MVC ao Unity.

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a>Sobre o serviço Storemanager

A loja de música MVC fornecida na solução inicial agora inclui um serviço que gerencia os dados do controlador de armazenamento denominado **StoreService**. Abaixo, você encontrará a implementação do serviço de armazenamento. Observe que todos os métodos retornam entidades de modelo.

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

O **StoreController** da solução inicial agora consome **StoreService**. Todas as referências de dados foram removidas do **StoreController**e agora é possível modificar o provedor de acesso a dados atual sem alterar nenhum método que consuma **StoreService**.

Você verá abaixo que a implementação de **StoreController** tem uma dependência com **StoreService** dentro do construtor de classe.

> [!NOTE]
> A dependência introduzida neste exercício está relacionada à **inversão de controle** (IOC).
> 
> O construtor da classe **StoreController** recebe um parâmetro de tipo **IStoreService** , que é essencial para executar chamadas de serviço de dentro da classe. No entanto, o **StoreController** não implementa o construtor padrão (sem parâmetros) que qualquer controlador deve ter para trabalhar com o MVC do ASP.net.
> 
> Para resolver a dependência, o controlador deve ser criado por uma fábrica abstrata (uma classe que retorna qualquer objeto do tipo especificado).

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> Você receberá um erro quando a classe tentar criar o StoreController sem enviar o objeto de serviço, já que não há nenhum construtor com parâmetros declarado.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a>Tarefa 1-executando o aplicativo

Nesta tarefa, você executará o aplicativo Begin, que inclui o serviço no controlador da loja que separa o acesso a dados da lógica do aplicativo.

Ao executar o aplicativo, você receberá uma exceção, pois o serviço do controlador não será passado como um parâmetro por padrão:

1. Abra a solução **inicial** localizada em **Source\Ex01-Injecting Controller\Begin**.

   1. Será necessário baixar alguns pacotes NuGet ausentes antes de continuar. Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.
   2. Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, Compile a solução clicando em **build** | **Compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto. É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.
2. Pressione **Ctrl + F5** para executar o aplicativo sem depuração. Você receberá a mensagem de erro &quot;**nenhum construtor sem parâmetros definido para este objeto**&quot;:

    ![Erro ao executar o aplicativo de início do ASP.NET MVC](aspnet-mvc-4-dependency-injection/_static/image3.png "Erro ao executar o aplicativo de início do ASP.NET MVC")

    *Erro ao executar o aplicativo de início do ASP.NET MVC*
3. Feche o navegador.

Nas etapas a seguir, você trabalhará na solução de loja de música para injetar a dependência que esse controlador precisa.

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a>Tarefa 2-incluindo o Unity na solução MvcMusicStore

Nesta tarefa, você incluirá o pacote NuGet do **Unity. Mvc3** para a solução.

> [!NOTE]
> O pacote Unity. Mvc3 foi projetado para o ASP.NET MVC 3, mas é totalmente compatível com o ASP.NET MVC 4.
> 
> O Unity é um contêiner de injeção de dependência simples e extensível com suporte opcional para a interceptação de instância e tipo. É um contêiner de finalidade geral para uso em qualquer tipo de aplicativo .NET. Ele fornece todos os recursos comuns encontrados em mecanismos de injeção de dependência, incluindo: criação de objeto, abstração de requisitos, especificando dependências em tempo de execução e flexibilidade, adiando a configuração do componente para o contêiner.

1. Instale o pacote NuGet do **Unity. Mvc3** no projeto **MvcMusicStore** . Para fazer isso, abra o **console do Gerenciador de pacotes** do **modo de exibição** | **outras janelas**.
2. Execute o comando a seguir.

    PMC

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    ![Instalando o pacote NuGet do Unity. Mvc3](aspnet-mvc-4-dependency-injection/_static/image4.png "Instalando o pacote NuGet do Unity. Mvc3")

    *Instalando o pacote NuGet do Unity. Mvc3*
3. Depois que o pacote **Unity. Mvc3** for instalado, explore os arquivos e as pastas que ele adiciona automaticamente para simplificar a configuração do Unity.

    ![Pacote do Unity. Mvc3 instalado](aspnet-mvc-4-dependency-injection/_static/image5.png "Pacote do Unity. Mvc3 instalado")

    *Pacote do Unity. Mvc3 instalado*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-application_start"></a>Tarefa 3-registrando o Unity no aplicativo Global.asax.cs\_iniciar

Nesta tarefa, você atualizará o método **Start do aplicativo\_** localizado em **global.asax.cs** para chamar o inicializador do bootstrapper do Unity e, em seguida, atualizará o arquivo bootstrapper registrando o serviço e o controlador que será usado para injeção de dependência.

1. Agora, você conectará o bootstrapper, que é o arquivo que inicializa o contêiner do Unity e o resolvedor de dependência. Para fazer isso, abra **global.asax.cs** e adicione o seguinte código realçado dentro do método de **início do aplicativo\_** .

    (Trecho de código – *laboratório de injeção de dependência ASP.net-Ex01-Initialize Unity*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. Abra o arquivo **bootstrapper.cs** .
3. Inclua os seguintes namespaces: **MvcMusicStore. Services** e **MusicStore. Controllers**.

    (Trecho de código- *laboratório de injeção de dependência ASP.net-Ex01-bootstrapper Adicionando namespaces*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. Substitua o conteúdo do método **BuildUnityContainer** pelo código a seguir que registra o controlador de armazenamento e o serviço de repositório.

    (Trecho de código – *laboratório de injeção de dependência ASP.net-Ex01-registrar controlador de armazenamento e serviço*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Tarefa 4-executando o aplicativo

Nesta tarefa, você executará o aplicativo para verificar se ele agora pode ser carregado depois de incluir o Unity.

1. Pressione **F5** para executar o aplicativo. agora, o aplicativo deve ser carregado sem mostrar nenhuma mensagem de erro.

    ![Executando o aplicativo com injeção de dependência](aspnet-mvc-4-dependency-injection/_static/image6.png "Executando o aplicativo com injeção de dependência")

    *Executando o aplicativo com injeção de dependência*
2. Navegue até **/Store**. Isso invocará **StoreController**, que agora é criado usando o **Unity**.

    ![Loja de música MVC](aspnet-mvc-4-dependency-injection/_static/image7.png "Loja de música MVC")

    *Loja de música MVC*
3. Feche o navegador.

Nos exercícios a seguir, você aprenderá a estender o escopo de injeção de dependência para usá-lo dentro de exibições e filtros de ação do ASP.NET MVC.

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a>Exercício 2: injetando uma exibição

Neste exercício, você aprenderá a usar a injeção de dependência em uma exibição com os novos recursos do ASP.NET MVC 4 para integração com o Unity. Para fazer isso, você chamará um serviço personalizado dentro da exibição de navegação da loja, que mostrará uma mensagem e uma imagem abaixo.

Em seguida, você integrará o projeto ao Unity e criará um resolvedor de dependência personalizado para injetar as dependências.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a>Tarefa 1-criando uma exibição que consome um serviço

Nesta tarefa, você criará uma exibição que executa uma chamada de serviço para gerar uma nova dependência. O serviço consiste em um serviço de mensagens simples incluído nesta solução.

1. Abra a solução **inicial** localizada na pasta **Source\Ex02-Injecting View\Begin** Caso contrário, você pode continuar usando a solução **final** obtida concluindo o exercício anterior.

   1. Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar. Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.
   2. Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, Compile a solução clicando em **build** | **Compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto. É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.
      > 
      > Para obter mais informações, consulte este artigo: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Inclua as classes **MessageService.cs** e **IMessageService.cs** localizadas na pasta **\Assets de origem** em **/Services**. Para fazer isso, clique com o botão direito do mouse em **Serviços** pasta e selecione **Adicionar item existente**. Navegue até o local dos arquivos e inclua-os.

    ![Adicionando serviço de mensagens e interface de serviço](aspnet-mvc-4-dependency-injection/_static/image8.png "Adicionando serviço de mensagens e interface de serviço")

    *Adicionando serviço de mensagens e interface de serviço*

    > [!NOTE]
    > A interface **IMessageService** define duas propriedades implementadas pela classe **MessageService** . Essas propriedades-**Message** e **ImageUrl**-armazenam a mensagem e a URL da imagem a ser exibida.
3. Crie a pasta **/pages** na pasta raiz do projeto e, em seguida, adicione a classe existente **MyBasePage.cs** de **Source\Assets**. A página de base da qual você herdará tem a seguinte estrutura.

    ![Pasta de páginas](aspnet-mvc-4-dependency-injection/_static/image9.png "Pasta Páginas")

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. Abra o modo de exibição **Browse. cshtml** na pasta **/views/Store** e torne-o herdado de **MyBasePage.cs**.

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. No modo de exibição de **navegação** , adicione uma chamada para **MessageService** para exibir uma imagem e uma mensagem recuperada pelo serviço.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a>Tarefa 2-incluindo um resolvedor de dependência personalizado e um ativador de página de exibição personalizada

Na tarefa anterior, você inseriu uma nova dependência dentro de uma exibição para executar uma chamada de serviço dentro dela. Agora, você resolverá essa dependência implementando as interfaces de injeção de dependência ASP.NET MVC **IViewPageActivator** e **IDependencyResolver**. Você incluirá na solução uma implementação de **IDependencyResolver** que tratará da recuperação do serviço usando o Unity. Em seguida, você incluirá outra implementação personalizada da interface **IViewPageActivator** , que resolverá a criação das exibições.

> [!NOTE]
> Desde o ASP.NET MVC 3, a implementação para injeção de dependência simplificou as interfaces para registrar serviços. **IDependencyResolver** e **IViewPageActivator** fazem parte dos recursos do ASP.NET MVC 3 para injeção de dependência.
> 
> **-** A interface IDependencyResolver substitui a IMvcServiceLocator anterior. Os implementadores de IDependencyResolver devem retornar uma instância do serviço ou de uma coleção de serviços.
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> **-A interface IViewPageActivator** fornece um controle mais refinado sobre como as páginas de exibição são instanciadas por meio da injeção de dependência. As classes que implementam a interface **IViewPageActivator** podem criar instâncias de exibição usando informações de contexto.
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]

1. Crie a pasta/**factories** na pasta raiz do projeto.
2. Inclua **CustomViewPageActivator.cs** para sua solução da pasta **/sources/assets/** para **fábricas** . Para fazer isso, clique com o botão direito do mouse na pasta **/factories** , selecione **Add | Item existente** e, em seguida, selecione **CustomViewPageActivator.cs**. Essa classe implementa a interface **IViewPageActivator** para manter o contêiner do Unity.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > **CustomViewPageActivator** é responsável por gerenciar a criação de uma exibição usando um contêiner do Unity.
3. Inclua o arquivo **UnityDependencyResolver.cs** de **/sources/assets** para a pasta **/factories** . Para fazer isso, clique com o botão direito do mouse na pasta **/factories** , selecione **Add | Item existente** e, em seguida, selecione arquivo **UnityDependencyResolver.cs** .

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > A classe **UnityDependencyResolver** é uma DependencyResolver personalizada para o Unity. Quando um serviço não pode ser encontrado dentro do contêiner do Unity, o resolvedor de base é invocated.

Na tarefa a seguir, ambas as implementações serão registradas para permitir que o modelo saiba o local dos serviços e as exibições.

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a>Tarefa 3-registrando para injeção de dependência no contêiner do Unity

Nesta tarefa, você colocará todas as coisas anteriores em conjunto para tornar o trabalho de injeção de dependência.

Até agora, sua solução tem os seguintes elementos:

- Um modo de exibição de **navegação** que herda de **MyBaseClass** e consome **MessageService**.
- Uma classe intermediária-**MyBaseClass**-que tem injeção de dependência declarada para a interface de serviço.
- Um Service- **MessageService** -e sua interface **IMessageService**.
- Um resolvedor de dependência personalizado para o Unity- **UnityDependencyResolver** -que lida com a recuperação de serviço.
- Uma exibição Page Activator- **CustomViewPageActivator** -que cria a página.

Para injetar o modo de exibição de **procura** , agora você registrará o resolvedor de dependência personalizado no contêiner do Unity.

1. Abra o arquivo **bootstrapper.cs** .
2. Registre uma instância do **MessageService** no contêiner do Unity para inicializar o serviço:

    (Trecho de código – *laboratório de injeção de dependência ASP.net-Ex02-serviço de mensagens de registro*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. Adicione uma referência ao namespace **MvcMusicStore. fábricas** .

    (Trecho de código – *laboratório de injeção de dependência ASP.net-Ex02-fábricas namespace*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. Registre o **CustomViewPageActivator** como um ativador de página de exibição no contêiner do Unity:

    (Trecho de código – *laboratório de injeção de dependência ASP.net-Ex02-Register CustomViewPageActivator*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. Substitua o resolvedor de dependência padrão ASP.NET MVC 4 por uma instância de **UnityDependencyResolver**. Para fazer isso, substitua o conteúdo do método **Initialize** pelo código a seguir:

    (Trecho de código – *laboratório de injeção de dependência ASP.net-Ex02 – resolvedor de dependência de atualização*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > O ASP.NET MVC fornece uma classe de resolvedor de dependências padrão. Para trabalhar com resolvedores de dependência personalizados como aquele que criamos para o Unity, esse resolvedor precisa ser substituído.

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Tarefa 4-executando o aplicativo

Nesta tarefa, você executará o aplicativo para verificar se o navegador da loja consome o serviço e mostra a imagem e a mensagem recuperada:

1. Pressione **F5** para executar o aplicativo.
2. Clique em **rock** dentro do menu gêneros e veja como o **MessageService** foi injetado na exibição e carregou a mensagem de boas-vindas e a imagem. Neste exemplo, estamos entrando para &quot;&quot;de **rock** :

    ![Loja de música MVC-exibir injeção](aspnet-mvc-4-dependency-injection/_static/image10.png "Loja de música MVC-exibir injeção")

    *Loja de música MVC-exibir injeção*
3. Feche o navegador.

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a>Exercício 3: injetando filtros de ação

Nos **filtros de ação personalizada** do laboratório prático anterior, você trabalhou com a personalização e a injeção de filtros. Neste exercício, você aprenderá a injetar filtros com injeção de dependência usando o contêiner do Unity. Para fazer isso, você adicionará à solução de loja de música um filtro de ação personalizado que rastreará a atividade do site.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a>Tarefa 1-incluindo o filtro de controle na solução

Nesta tarefa, você incluirá na loja de músicas um filtro de ação personalizada para rastrear eventos. Como os conceitos de filtro de ação personalizada já são tratados no laboratório anterior &quot;filtros de ação personalizados&quot;, você incluirá apenas a classe de filtro da pasta ativos deste laboratório e, em seguida, criará um provedor de filtro para o Unity:

1. Abra a solução **inicial** localizada na pasta **Filter\Begin da ação de injeção de Source\Ex03** . Caso contrário, você pode continuar usando a solução **final** obtida concluindo o exercício anterior.

   1. Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar. Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.
   2. Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, Compile a solução clicando em **build** | **Compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto. É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.
      > 
      > Para obter mais informações, consulte este artigo: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Inclua o arquivo **TraceActionFilter.cs** de **/sources/assets** para a pasta **/Filters** .

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > Esse filtro de ação personalizada executa o rastreamento ASP.NET. Você pode verificar &quot;filtros de ação locais e dinâmicos do ASP.NET MVC 4&quot; laboratório para obter mais referências.
3. Adicione a classe vazia **FilterProvider.cs** ao projeto na pasta **/Filters.**
4. Adicione os namespaces **System. Web. Mvc** e **Microsoft. Practices. Unity** no **FilterProvider.cs**.

    (Trecho de código – *laboratório de injeção de dependência ASP.net-Ex03-provedor de filtros Adicionando namespaces*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. Faça a classe herdar da interface **IFilterProvider** .

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. Adicione uma propriedade **IUnityContainer** na classe **FilterProvider** e, em seguida, crie um construtor de classe para atribuir o contêiner.

    (Trecho de código- *ASP.net de injeção de dependência do Ex03-Construtor de provedor de filtro*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > O construtor da classe do provedor de filtro não está criando um **novo** objeto dentro de. O contêiner é passado como um parâmetro e a dependência é resolvida pelo Unity.
7. Na classe **FilterProvider** , implemente o método **getfiltras** da interface **IFilterProvider** .

    (Trecho de código – *laboratório de injeção de dependência ASP.net-Ex03-filtro Getfiltros*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a>Tarefa 2-registrando e habilitando o filtro

Nesta tarefa, você habilitará o rastreamento de site. Para fazer isso, você registrará o filtro no método **bootstrapper.cs BuildUnityContainer** para iniciar o rastreamento:

1. Abra o **Web. config** localizado na raiz do projeto e habilite o rastreamento de rastreamento no grupo System. Web.

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. Abra **bootstrapper.cs** na raiz do projeto.
3. Adicione uma referência ao namespace **MvcMusicStore. Filters** .

    (Trecho de código- *laboratório de injeção de dependência ASP.net-Ex03-bootstrapper Adicionando namespaces*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. Selecione o método **BuildUnityContainer** e registre o filtro no contêiner do Unity. Você precisará registrar o provedor de filtro, bem como o filtro de ação.

    (Trecho de código – *laboratório de injeção de dependência ASP.net-Ex03-registrar FilterProvider e ActionFilter*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tarefa 3-executando o aplicativo

Nesta tarefa, você executará o aplicativo e testará se o filtro de ação personalizada está rastreando a atividade:

1. Pressione **F5** para executar o aplicativo.
2. Clique em **rock** dentro do menu gêneros. Você pode navegar para mais gêneros se desejar.

    ![Loja de Música](aspnet-mvc-4-dependency-injection/_static/image11.png "Loja de Música")

    *Loja de Música*
3. Navegue até **/trace.axd** para ver a página de rastreamento do aplicativo e clique em **Exibir detalhes**.

    ![Log de rastreamento do aplicativo](aspnet-mvc-4-dependency-injection/_static/image12.png "Log de rastreamento do aplicativo")

    *Log de rastreamento do aplicativo*

    ![Rastreamento de aplicativo-detalhes da solicitação](aspnet-mvc-4-dependency-injection/_static/image13.png "Rastreamento de aplicativo-detalhes da solicitação")

    *Rastreamento de aplicativo-detalhes da solicitação*
4. Feche o navegador.

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Resumo

Ao concluir este laboratório prático, você aprendeu a usar a injeção de dependência no ASP.NET MVC 4 integrando o Unity usando um pacote NuGet. Para conseguir isso, você usou a injeção de dependência dentro de controladores, exibições e filtros de ação.

Os conceitos a seguir foram abordados:

- Recursos de injeção de dependência do MVC 4 do ASP.NET
- Integração do Unity usando o pacote NuGet do Unity. Mvc3
- Injeção de dependência em controladores
- Injeção de dependência em exibições
- Injeção de dependência de filtros de ação

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apêndice A: Instalando o Visual Studio Express 2012 para Web

Você pode instalar o **Microsoft Visual Studio Express 2012 para Web** ou outra versão &quot;Express&quot; usando o **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** . As instruções a seguir orientam você pelas etapas necessárias para instalar o *Visual Studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.

1. Ir para [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169). Como alternativa, se você já tiver instalado o Web Platform Installer, poderá abri-lo e pesquisar o produto &quot;<em>Visual Studio Express 2012 para Web com o SDK do Windows Azure</em>&quot;.
2. Clique em **instalar agora**. Se você não tiver **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.
3. Quando **Web Platform Installer** estiver aberto, clique em **instalar** para iniciar a instalação.

    ![Instalar Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Instalar Visual Studio Express")

    *Instalar Visual Studio Express*
4. Leia todos os termos e licenças de produtos e clique em **aceito** para continuar.

    ![Aceitando os termos de licença](aspnet-mvc-4-dependency-injection/_static/image15.png)

    *Aceitando os termos de licença*
5. Aguarde até que o processo de download e instalação seja concluído.

    ![Progresso da instalação](aspnet-mvc-4-dependency-injection/_static/image16.png)

    *Progresso da instalação*
6. Quando a instalação for concluída, clique em **concluir**.

    ![Instalação concluída](aspnet-mvc-4-dependency-injection/_static/image17.png)

    *Instalação concluída*
7. Clique em **sair** para fechar Web Platform Installer.
8. Para abrir o Visual Studio Express para Web, vá para a tela **Iniciar** e comece a escrever &quot;**vs Express**&quot;e, em seguida, clique no bloco **vs Express para Web** .

    ![Bloco VS Express para Web](aspnet-mvc-4-dependency-injection/_static/image18.png)

    *Bloco VS Express para Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Apêndice B: usando trechos de código

Com trechos de código, você tem todo o código de que precisa ao seu alcance. O documento de laboratório informará exatamente quando você pode usá-los, conforme mostrado na figura a seguir.

![Usando trechos de código do Visual Studio para inserir código em seu projeto](aspnet-mvc-4-dependency-injection/_static/image19.png "Usando trechos de código do Visual Studio para inserir código em seu projeto")

*Usando trechos de código do Visual Studio para inserir código em seu projeto*

***Para adicionar um trecho de código usando o tecladoC# (somente)***

1. Coloque o cursor onde você deseja inserir o código.
2. Comece digitando o nome do trecho de código (sem espaços ou hifens).
3. Observe como o IntelliSense exibe nomes de trechos de código correspondentes.
4. Selecione o trecho correto (ou continue digitando até que o nome do trecho inteiro seja selecionado).
5. Pressione a tecla TAB duas vezes para inserir o trecho de código no local do cursor.

![Comece a digitar o nome do trecho](aspnet-mvc-4-dependency-injection/_static/image20.png "Comece a digitar o nome do trecho")

*Comece a digitar o nome do trecho*

![Pressione Tab para selecionar o trecho realçado](aspnet-mvc-4-dependency-injection/_static/image21.png "Pressione Tab para selecionar o trecho realçado")

*Pressione Tab para selecionar o trecho realçado*

![Pressione Tab novamente e o trecho será expandido](aspnet-mvc-4-dependency-injection/_static/image22.png "Pressione Tab novamente e o trecho será expandido")

*Pressione Tab novamente e o trecho será expandido*

***Para adicionar um trecho de código usando o mouseC#(, Visual Basic e XML)*** uma. Clique com o botão direito do mouse no local em que você deseja inserir o trecho de código.

1. Selecione **Inserir trecho** seguido por **meus trechos de código**.
2. Selecione o trecho relevante na lista clicando nele.

![Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho](aspnet-mvc-4-dependency-injection/_static/image23.png "Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho")

*Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho*

![Selecione o trecho relevante na lista clicando nele](aspnet-mvc-4-dependency-injection/_static/image24.png "Selecione o trecho relevante na lista clicando nele")

*Selecione o trecho relevante na lista clicando nele*
