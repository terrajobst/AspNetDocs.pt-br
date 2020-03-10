---
uid: single-page-application/overview/introduction/knockoutjs-template
title: 'Aplicativo de página única: modelo de KnockoutJS | Microsoft Docs'
author: MikeWasson
description: Modelo de Knockout
ms.author: riande
ms.date: 01/30/2013
ms.assetid: f9c07af0-4b20-4b08-af8f-47fc3df169a2
msc.legacyurl: /single-page-application/overview/introduction/knockoutjs-template
msc.type: authoredcontent
ms.openlocfilehash: 3a551db1caa9636eb7f2e04c287d3ef371263584
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578690"
---
# <a name="single-page-application-knockoutjs-template"></a>Aplicativo de página única: modelo de KnockoutJS

por [Mike Wasson](https://github.com/MikeWasson)

> O modelo MVC do Knockout faz parte do ASP.NET and Web Tools 2012,2
> 
> [Baixar o ASP.NET and Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650)

A atualização do ASP.NET and Web Tools 2012,2 inclui um modelo de aplicativo de página única (SPA) para o ASP.NET MVC 4. Este modelo foi projetado para ajudá-lo a começar a criar rapidamente aplicativos Web do lado do cliente interativos.

O "aplicativo de página única" (SPA) é o termo geral para um aplicativo Web que carrega uma única página HTML e, em seguida, atualiza a página dinamicamente, em vez de carregar novas páginas. Após o carregamento inicial da página, o SPA se comunica com o servidor por meio de solicitações AJAX.

![](knockoutjs-template/_static/image1.png)

O AJAX não é nada novo, mas hoje há estruturas de JavaScript que facilitam a criação e a manutenção de um aplicativo SPA sofisticado grande. Além disso, o HTML 5 e o CSS3 estão facilitando a criação de UIs avançadas.

Para começar, o modelo SPA cria um aplicativo de exemplo "lista de tarefas pendentes". Neste tutorial, vamos fazer um tour guiado do modelo. Primeiro, vamos examinar o aplicativo de lista de tarefas em si e, em seguida, examinar as partes de tecnologia que o fazem funcionar.

## <a name="create-a-new-spa-template-project"></a>Criar um novo projeto de modelo SPA

Requisitos:

- Visual Studio 2012 ou Visual Studio Express 2012 para Web
- Atualização do ASP.NET Web Tools 2012,2. Você pode instalar a atualização [aqui](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2).

Inicie o Visual Studio e selecione **novo projeto** na página inicial. Ou, no menu **arquivo** , selecione **novo** e **projeto**.

No painel **modelos** , selecione **modelos instalados** e expanda o **nó C# Visual** . Em **Visual C#** , selecione **Web**. Na lista de modelos de projeto, selecione **aplicativo Web ASP.NET MVC 4**. Nomeie o projeto e clique em **OK**.

![](knockoutjs-template/_static/image2.png)

No assistente de **novo projeto** , selecione **aplicativo de página única**.

![](knockoutjs-template/_static/image3.png)

Pressione F5 para compilar e executar o aplicativo. Quando o aplicativo é executado pela primeira vez, ele exibe uma tela de logon.

![](knockoutjs-template/_static/image4.png)

Clique no link &quot;inscrever-se&quot; e crie um novo usuário.

![](knockoutjs-template/_static/image5.png)

Depois de fazer logon, o aplicativo cria uma lista de tarefas padrão com dois itens. Você pode clicar em "Adicionar lista de tarefas" para adicionar uma nova lista.

![](knockoutjs-template/_static/image6.png)

Renomeie a lista, adicione itens à lista e verifique-os. Você também pode excluir itens ou excluir uma lista inteira. As alterações são persistidas automaticamente em um banco de dados no servidor (na verdade, LocalDB neste ponto, porque você está executando o aplicativo localmente).

![](knockoutjs-template/_static/image7.png)

## <a name="architecture-of-the-spa-template"></a>Arquitetura do modelo SPA

Este diagrama mostra os principais blocos de construção para o aplicativo.

![](knockoutjs-template/_static/image8.png)

No lado do servidor, o ASP.NET MVC serve o HTML e também manipula a autenticação baseada em formulários.

ASP.NET Web API lida com todas as solicitações relacionadas a ToDoLists e ToDoItems, incluindo obter, criar, atualizar e excluir. O cliente troca dados com a API Web no formato JSON.

Entity Framework (EF) é a camada O/RM. Ele é mediado entre o mundo orientado a objeto do ASP.NET e o banco de dados subjacente. O banco de dados usa o LocalDB, mas você pode alterá-lo no arquivo Web. config. Normalmente, você usaria o LocalDB para desenvolvimento local e, em seguida, implantou em um banco de dados SQL no servidor, usando a migração de código do EF.

No lado do cliente, a biblioteca knockout. js manipula atualizações de página de solicitações AJAX. O Knockout usa a vinculação de dados para sincronizar a página com os dados mais recentes. Dessa forma, você não precisa escrever nenhum código que percorra os dados JSON e atualize o DOM. Em vez disso, você coloca atributos declarativos no HTML que dizem ao Knockout como apresentar os dados.

Uma grande vantagem dessa arquitetura é que ela separa a camada de apresentação da lógica do aplicativo. Você pode criar a parte da API Web sem saber nada sobre como a sua página da Web será exibida. No lado do cliente, você cria um "modelo de exibição" para representar esses dados e o modelo de exibição usa o Knockout para associar ao HTML. Isso permite que você altere facilmente o HTML sem alterar o modelo de exibição. (Vamos examinar um pouco o Knockout mais tarde.)

## <a name="models"></a>Modelos

No projeto do Visual Studio, a pasta modelos contém os modelos que são usados no lado do servidor. (Também há modelos no lado do cliente; chegaremos a eles.)

![](knockoutjs-template/_static/image9.png)

**TodoItem, ToDoList**

Estes são os modelos de banco de dados para Entity Framework Code First. Observe que esses modelos têm propriedades que apontam entre si. `ToDoList` contém uma coleção de ToDoItems, e cada `ToDoItem` tem uma referência de volta para sua lista de tarefas pai. Essas propriedades são chamadas de propriedades de navegação e representam a relação um-para-muitos uma lista de tarefas pendentes e seus itens de tarefas pendentes.

A classe `ToDoItem` também usa o atributo **[ForeignKey]** para especificar que `ToDoListId` é uma chave estrangeira na tabela `ToDoList`. Isso informa ao EF para adicionar uma restrição FOREIGN-KEY ao banco de dados.

[!code-csharp[Main](knockoutjs-template/samples/sample1.cs)]

**TodoItemDto, TodoListDto**

Essas classes definem os dados que serão enviados ao cliente. "DTO" significa "objeto de transferência de dados". O DTO define como as entidades serão serializadas em JSON. Em geral, há várias razões para usar DTOs:

- Para controlar quais propriedades são serializadas. O DTO pode conter um subconjunto das propriedades do modelo de domínio. Você pode fazer isso por motivos de segurança (para ocultar dados confidenciais) ou simplesmente para reduzir a quantidade de dados enviados.
- Para alterar a forma dos dados – por exemplo, para mesclar uma estrutura de dados mais complexa.
- Para manter qualquer lógica de negócios fora do DTO (separação de preocupações).
- Se os modelos de domínio não puderem ser serializados por algum motivo. Por exemplo, referências circulares podem causar problemas quando você serializa um objeto há maneiras de lidar com esse problema na API Web (consulte [manipulação de referências a objetos circulares](../../../web-api/overview/formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)); Mas usar um DTO simplesmente evita o problema completamente.

No modelo SPA, o DTOs contém os mesmos dados que os modelos de domínio. No entanto, eles ainda são úteis porque evitam referências circulares das propriedades de navegação e demonstram o padrão de DTO geral.

**AccountModels.cs**

Este arquivo contém modelos para associação do site. A classe `UserProfile` define o esquema para perfis de usuário no BD de associação. (Nesse caso, a única informação é a ID de usuário e o nome de usuário.) As outras classes de modelo nesse arquivo são usadas para criar o registro de usuário e os formulários de logon.

## <a name="entity-framework"></a>Entity Framework

O modelo SPA usa o Code First EF. No desenvolvimento de Code First, você define os modelos primeiro no código e, em seguida, o EF usa o modelo para criar o banco de dados. Você também pode usar o EF com um banco de dados existente ([Database First](https://msdn.microsoft.com/data/jj206878.aspx)).

A classe `TodoItemContext` na pasta modelos deriva de **DbContext**. Essa classe fornece a "cola" entre os modelos e o EF. O `TodoItemContext` mantém uma coleção de `ToDoItem` e uma coleção de `TodoList`. Para consultar o banco de dados, basta escrever uma consulta LINQ nessas coleções. Por exemplo, aqui está como você pode selecionar todas as listas de tarefas pendentes para o usuário "Alice":

[!code-csharp[Main](knockoutjs-template/samples/sample2.cs)]

Você também pode adicionar novos itens à coleção, atualizar itens ou excluir itens da coleção e manter as alterações no banco de dados.

## <a name="aspnet-web-api-controllers"></a>Controladores de ASP.NET Web API

No ASP.NET Web API, os controladores são objetos que lidam com solicitações HTTP. Conforme mencionado, o modelo SPA usa a API da Web para habilitar operações CRUD em instâncias `ToDoList` e `ToDoItem`. Os controladores estão localizados na pasta controladores da solução.

![](knockoutjs-template/_static/image10.png)

- `TodoController`: manipula solicitações HTTP para itens de tarefas pendentes
- `TodoListController`: manipula solicitações HTTP para listas de tarefas pendentes.

Esses nomes são significativos, pois a API Web corresponde ao caminho do URI para o nome do controlador. (Para saber como a API Web roteia solicitações HTTP para controladores, consulte [Roteamento no ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).)

Vamos examinar a classe `ToDoListController`. Ele contém um único membro de dados:

[!code-csharp[Main](knockoutjs-template/samples/sample3.cs)]

O `TodoItemContext` é usado para se comunicar com o EF, conforme descrito anteriormente. Os métodos no controlador implementam as operações CRUD. A API Web mapeia solicitações HTTP do cliente para métodos de controlador, da seguinte maneira:

| Solicitação HTTP | Método de controlador | DESCRIÇÃO |
| --- | --- | --- |
| GET /api/todo | `GetTodoLists` | Obtém uma coleção de listas de tarefas pendentes. |
| OBTER*ID* do/API/todo/ | `GetTodoList` | Obtém uma lista de tarefas pendentes por ID |
| COLOCAR*ID* de/API/todo/ | `PutTodoList` | Atualiza uma lista de tarefas pendentes. |
| POST /api/todo | `PostTodoList` | Cria uma nova lista de tarefas pendentes. |
| EXCLUIR*ID* do/API/todo/ | `DeleteTodoList` | Exclui uma lista de tarefas pendentes. |

Observe que os URIs de algumas operações contêm espaços reservados para o valor de ID. Por exemplo, para excluir uma lista com uma ID de 42, o URI é `/api/todo/42`.

Para saber mais sobre como usar a API Web para operações CRUD, consulte [criando uma API Web que oferece suporte a operações CRUD](../../../web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations.md). O código para esse controlador é razoavelmente simples. Aqui estão alguns pontos interessantes:

- O método `GetTodoLists` usa uma consulta LINQ para filtrar os resultados pela ID do usuário conectado. Dessa forma, um usuário vê apenas os dados que pertencem a ele. Além disso, observe que uma instrução SELECT é usada para converter as instâncias de `ToDoList` em instâncias de `TodoListDto`.
- Os métodos PUT e POST verificam o estado do modelo antes de modificar o banco de dados. Se **ModelState. IsValid** for false, esses métodos retornarão http 400, solicitação inadequada. Leia mais sobre a validação de modelo na API Web na [validação do modelo](../../../web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api.md).
- A classe Controller também é decorada com o atributo **[Authorize]** . Esse atributo verifica se a solicitação HTTP é autenticada. Se a solicitação não for autenticada, o cliente receberá HTTP 401, não autorizado. Leia mais sobre autenticação na [autenticação e autorização no ASP.NET Web API](../../../web-api/overview/security/authentication-and-authorization-in-aspnet-web-api.md).

A classe `TodoController` é muito semelhante à `TodoListController`. A maior diferença é que ele não define nenhum método GET, pois o cliente obterá os itens de tarefas pendentes junto com cada lista de tarefas pendentes.

## <a name="mvc-controllers-and-views"></a>Controladores e exibições MVC

Os controladores MVC também estão localizados na pasta controladores da solução. `HomeController` renderiza o HTML principal para o aplicativo. A exibição do controlador inicial é definida em views/home/index. cshtml. A exibição página inicial renderiza conteúdo diferente dependendo se o usuário está conectado:

[!code-cshtml[Main](knockoutjs-template/samples/sample4.cshtml)]

Quando os usuários estão conectados, eles veem a interface do usuário principal. Caso contrário, eles verão o painel de logon. Observe que essa renderização condicional ocorre no lado do servidor. Nunca tente ocultar o conteúdo confidencial no lado&#8212;do cliente qualquer coisa que você enviar em uma resposta http é visível para alguém que está assistindo às mensagens http brutas.

## <a name="client-side-javascript-and-knockoutjs"></a>JavaScript do lado do cliente e knockout. js

Agora, vamos voltar do lado do servidor do aplicativo para o cliente. O modelo SPA usa uma combinação de jQuery e knockout. js para criar uma interface do usuário tranqüila e interativa. Knockout. js é uma biblioteca JavaScript que facilita a ligação de HTML aos dados. Knockout. js usa um padrão chamado "Model-View-ViewModel".

- O modelo são os dados de domínio (listas de tarefas pendentes e itens de tarefas).
- A exibição é o documento HTML.
- O View-Model é um objeto JavaScript que contém os dados do modelo. O View-Model é uma abstração de código da interface do usuário. Ele não tem conhecimento da representação HTML. Em vez disso, ele representa recursos abstratos da exibição, como "uma lista de itens de tarefas pendentes".

A exibição é associada a dados ao modelo de exibição. As atualizações do modelo de exibição são refletidas automaticamente na exibição. As associações funcionam também com a outra direção. Eventos no DOM (como cliques) são associados a dados para funções no modelo de exibição, que disparam chamadas AJAX.

O modelo SPA organiza o JavaScript do lado do cliente em três camadas:

- todo. DataContext. js: envia solicitações AJAX.
- todo. Model. js: define os modelos.
- todo. ViewModel. js: define o modelo de exibição.

![](knockoutjs-template/_static/image11.png)

Esses arquivos de script estão localizados na pasta scripts/aplicativo da solução.

![](knockoutjs-template/_static/image12.png)

**todo. DataContext** trata todas as chamadas AJAX para os controladores da API Web. (As chamadas AJAX para fazer logon estão definidas em outro lugar, em ajaxLogin. js.)

**todo. Model. js** define os modelos do lado do cliente (navegador) para as listas de tarefas pendentes. Há duas classes de modelo: todoItem e ToDoList.

Muitas das propriedades nas classes de modelo são do tipo "ko. observável". Observáveis são como o Knockout faz sua mágica. Na [documentação do Knockout](http://knockoutjs.com/documentation/introduction.html): um observável é um "objeto JavaScript que pode notificar os assinantes sobre alterações". Quando o valor de uma alteração observável, o Knockout atualiza todos os elementos HTML associados a esses observáveis. Por exemplo, todoItem tem observáveis para as propriedades Title e isdone:

[!code-javascript[Main](knockoutjs-template/samples/sample5.js)]

Você também pode assinar um observável no código. Por exemplo, a classe todoItem assina as alterações nas propriedades "isdone" e "title":

[!code-javascript[Main](knockoutjs-template/samples/sample6.js)]

**Exibir modelo**

O modelo de exibição é definido em todo. ViewModel. js. O modelo de exibição é o ponto central em que o aplicativo associa os elementos da página HTML aos dados do domínio. No modelo SPA, o modelo de exibição contém uma matriz observável de todoLists. O código a seguir no modelo de exibição informa ao Knockout para aplicar as associações:

[!code-javascript[Main](knockoutjs-template/samples/sample7.js)]

## <a name="html-and-data-binding"></a>HTML e Associação de dados

O HTML principal da página é definido em views/home/index. cshtml. Como estamos usando a vinculação de dados, o HTML é apenas um modelo para o que realmente é renderizado. O Knockout usa associações *declarativas* . Você associa elementos de página a dados adicionando um atributo de "vinculação de dados" ao elemento. Veja um exemplo muito simples, extraído da documentação do Knockout:

[!code-html[Main](knockoutjs-template/samples/sample8.html)]

Neste exemplo, o Knockout atualiza o conteúdo do elemento **&lt;span&gt;** com o valor de `myItems.count()`. Sempre que esse valor for alterado, o Knockout atualizará o documento.

O Knockout fornece vários tipos de ligação diferentes. Aqui estão algumas das associações usadas no modelo SPA:

- **foreach**: permite que você Itere em um loop e aplique a mesma marcação a cada item na lista. Isso é usado para renderizar as listas de tarefas e itens de tarefas pendentes. No **foreach**, as associações são aplicadas aos elementos da lista.
- **visível**: usado para alternar a visibilidade. Ocultar marcação quando uma coleção estiver vazia ou tornar a mensagem de erro visível.
- **valor**: usado para preencher valores de formulário.
- **clique em**: associa um evento de clique a uma função no modelo de exibição.

## <a name="anti-csrf-protection"></a>Proteção CSRF

A CSRF (solicitação intersite forjada) é um ataque em que um site mal-intencionado envia uma solicitação para um site vulnerável em que o usuário está conectado no momento. Para ajudar a evitar ataques de CSRF, o ASP.NET MVC usa *tokens antifalsificação*, também chamados de tokens de verificação de solicitação. A ideia é que o servidor coloca um token gerado aleatoriamente em uma página da Web. Quando o cliente envia dados para o servidor, ele deve incluir esse valor na mensagem de solicitação.

Os tokens antifalsificação funcionam porque a página mal-intencionada não pode ler os tokens do usuário, devido às políticas de mesma origem. (As políticas de mesma origem impedem que os documentos hospedados em dois sites diferentes acessem o conteúdo de cada um.)

O ASP.NET MVC fornece suporte interno para tokens antifalsificação, por meio da classe [antifalsificação](https://msdn.microsoft.com/library/system.web.helpers.antiforgery.aspx) e do atributo [[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute.aspx) . Atualmente, essa funcionalidade não é incorporada à API Web. No entanto, o modelo SPA inclui uma implementação personalizada para a API da Web. Esse código é definido na classe `ValidateHttpAntiForgeryTokenAttribute`, que está localizada na pasta filtros da solução. Para saber mais sobre o CSRF na API Web, consulte [prevenção de ataques de CSRF (solicitação intersite forjada)](../../../web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="conclusion"></a>Conclusão

O modelo SPA foi projetado para ajudá-lo a começar rapidamente a escrever aplicativos Web modernos e interativos. Ele usa a biblioteca knockout. js para separar a apresentação (marcação HTML) da lógica de dados e do aplicativo. Mas o Knockout não é a única biblioteca JavaScript que você pode usar para criar um SPA. Se você quiser explorar algumas outras opções, dê uma olhada nos [modelos Spa criados pela Comunidade](../templates/index.md).
