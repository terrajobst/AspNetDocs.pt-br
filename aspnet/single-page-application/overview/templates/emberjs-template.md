---
uid: single-page-application/overview/templates/emberjs-template
title: Modelo de EmberJS | Microsoft Docs
author: xqiu
description: Modelo EmberJS
ms.author: riande
ms.date: 01/30/2013
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 1aefa46dd0841b1b06675409cc8a09f9a218d7ac
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578501"
---
# <a name="emberjs-template"></a>Modelo EmberJS

por [Xinyang Qiu](https://github.com/xqiu)

> O modelo MVC EmberJS é escrito por Nathan Totten, Thiago Santos e Xinyang Qiu.
> 
> [Baixar o modelo MVC do EmberJS](https://go.microsoft.com/fwlink/?LinkId=282647)

O modelo SPA EmberJS foi projetado para começar a criar rapidamente aplicativos Web do lado do cliente interativos usando o EmberJS.

O "aplicativo de página única" (SPA) é o termo geral para um aplicativo Web que carrega uma única página HTML e, em seguida, atualiza a página dinamicamente, em vez de carregar novas páginas. Após o carregamento inicial da página, o SPA se comunica com o servidor por meio de solicitações AJAX.

![](emberjs-template/_static/image1.png)

O AJAX não é nada novo, mas hoje há estruturas de JavaScript que facilitam a criação e a manutenção de um aplicativo SPA sofisticado grande. Além disso, o HTML 5 e o CSS3 estão facilitando a criação de UIs avançadas.

O modelo SPA EmberJS usa a biblioteca [Ember](http://emberjs.com/) JavaScript para lidar com atualizações de página de solicitações Ajax. O Ember. js usa a vinculação de dados para sincronizar a página com os dados mais recentes. Dessa forma, você não precisa escrever nenhum código que percorra os dados JSON e atualize o DOM. Em vez disso, você coloca atributos declarativos no HTML que dizem ao Ember. js como apresentar os dados.

No lado do servidor, o modelo EmberJS é quase idêntico ao [modelo Spa KnockoutJS](../introduction/knockoutjs-template.md). Ele usa o ASP.NET MVC para servir documentos HTML e ASP.NET Web API para lidar com solicitações AJAX do cliente. Para obter mais informações sobre esses aspectos do modelo, consulte a documentação do [modelo KnockoutJS](../introduction/knockoutjs-template.md) . Este tópico se concentra nas diferenças entre o modelo de Knockout e o modelo EmberJS.

## <a name="create-an-emberjs-spa-template-project"></a>Criar um projeto de modelo SPA EmberJS

Baixe e instale o modelo clicando no botão baixar acima. Talvez seja necessário reiniciar o Visual Studio.

No painel **modelos** , selecione **modelos instalados** e expanda o **nó C# Visual** . Em **Visual C#** , selecione **Web**. Na lista de modelos de projeto, selecione **aplicativo Web ASP.NET MVC 4**. Nomeie o projeto e clique em **OK**.

![](emberjs-template/_static/image2.png)

No assistente de **novo projeto** , selecione **projeto Spa Ember. js**.

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a>Visão geral do modelo SPA EmberJS

O modelo EmberJS usa uma combinação de jQuery, Ember. js, Handlebars. js para criar uma interface do usuário tranqüila e interativa.

Ember. js é uma biblioteca JavaScript que usa um padrão MVC do lado do cliente.

- Um *modelo*, escrito na linguagem de modelagem Handlebars, descreve a interface do usuário do aplicativo. No modo de liberação, o [compilador Handlebars](https://github.com/Myslik/csharp-ember-handlebars) é usado para agrupar e compilar o modelo Handlebars.
- Um *modelo* armazena os dados do aplicativo que ele obtém do servidor (listas de tarefas pendentes e itens de tarefas).
- Um *controlador* armazena o estado do aplicativo. Os controladores geralmente apresentam dados de modelo aos modelos correspondentes.
- Uma *exibição* traduz eventos primitivos do aplicativo e os passa para o controlador.
- Um *roteador* gerencia o estado do aplicativo, mantendo URLs e modelos em sincronia.

Além disso, a biblioteca de dados Ember pode ser usada para sincronizar objetos JSON (obtidos do servidor por meio de uma API RESTful) e os modelos de cliente.

O modelo SPA EmberJS organiza os scripts em oito camadas:

- webAPI\_Adapter. js, webAPI\_Serialization. js: estende a biblioteca de dados do Ember para trabalhar com ASP.NET Web API.
- Scripts/Helpers. js: define novos auxiliares do Ember Handlebars.
- Scripts/app. js: cria o aplicativo e configura o adaptador e o serializador.
- Scripts/app/Models/\*. js: define os modelos.
- Scripts/app/views/\*. js: define as exibições.
- Scripts/app/Controllers/\*. js: define os controladores.
- Scripts/aplicativos/rotas, scripts/app/router. js: define as rotas.
- Templates/\*. HBS: define os modelos de Handlebars.

Vamos examinar alguns desses scripts mais detalhadamente.

## <a name="models"></a>Modelos

Os modelos são definidos na pasta scripts/aplicativos/modelos. Há dois arquivos de modelo: todoItem. js e ToDoList. js.

**todo. Model. js** define os modelos do lado do cliente (navegador) para as listas de tarefas pendentes. Há duas classes de modelo: todoItem e ToDoList. No Ember, os modelos são subclasses de DS. Deprecia. Um modelo pode ter propriedades com atributos:

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

Os modelos podem definir relações com outros modelos:

[!code-css[Main](emberjs-template/samples/sample2.css)]

Os modelos podem ter propriedades computadas que se associam a outras propriedades:

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

Os modelos podem ter funções de observador, que são invocadas quando uma propriedade observada é alterada:

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a>Exibições

As exibições são definidas na pasta scripts/aplicativos/exibições. Uma exibição traduz eventos da interface do usuário do aplicativo. Um manipulador de eventos pode chamar de volta para funções de controlador ou simplesmente chamar o contexto de dados diretamente.

Por exemplo, o código a seguir é de views/TodoItemEditView. js. Ele define a manipulação de eventos para um campo de texto de entrada.

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a>Controller

Os controladores são definidos na pasta scripts/aplicativos/controladores. Para representar um único modelo, estenda `Ember.ObjectController`:

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

Um controlador também pode representar uma coleção de modelos estendendo `Ember.ArrayController`. Por exemplo, o TodoListController representa uma matriz de objetos `todoList`. O controlador classifica por ID de ToDoList, em ordem decrescente:

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

O controlador define uma função chamada `addTodoList`, que cria uma nova lista de tarefas e a adiciona à matriz. Para ver como essa função é chamada, abra o arquivo de modelo chamado todoListTemplate. html, na pasta modelos. O código de modelo a seguir associa um botão à função `addTodoList`:

[!code-html[Main](emberjs-template/samples/sample8.html)]

O controlador também contém uma propriedade `error`, que contém uma mensagem de erro. Aqui está o código de modelo para exibir a mensagem de erro (também em todoListTemplate. html):

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a>Rotas

O router. js define as rotas e o modelo padrão a ser exibido, configura o estado do aplicativo e corresponde às URLs às rotas:

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

O TodoListRoute. js carrega os dados para o TodoListRoute substituindo a função setupController:

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

O Ember usa convenções de nomenclatura para corresponder URLs, nomes de rotas, controladores e modelos. Para obter mais informações, consulte [http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) na documentação do EmberJS.

## <a name="templates"></a>Modelos

A pasta modelos contém quatro modelos:

- Application. HBS: o modelo padrão que é renderizado quando o aplicativo é iniciado.
- About. HBS: o modelo para a rota "/About".
- index. HBS: o modelo para a rota "/" raiz.
- ToDoList. HBS: o modelo para a rota "/todo".
- \_Navigation. HBS: o modelo define o menu de navegação.

O modelo de aplicativo funciona como uma página mestra. Ele contém um cabeçalho, um rodapé e um "{{tomada}}" para inserir outros modelos, dependendo da rota. Para obter mais informações sobre modelos de aplicativos no Ember, consulte [http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).

O modelo "/todoList" contém duas expressões de loop. O loop externo é `{{#each controller}}`e o loop interno é `{{#each todos}}`. O código a seguir mostra uma exibição `Ember.Checkbox` interna, uma `App.TodoItemEditView`personalizada e um link com uma ação `deleteTodo`.

[!code-html[Main](emberjs-template/samples/sample12.html)]

A classe `HtmlHelperExtensions`, definida em Controllers/HtmlHelperExtensions. cs, define uma função auxiliar para armazenar em cache e inserir arquivos de modelo quando **debug** é definido como **true** no arquivo Web. config. Essa função é chamada do arquivo de exibição ASP.NET MVC definido em views/home/app. cshtml:

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

Chamado sem argumentos, a função renderiza todos os arquivos de modelo na pasta modelos. Você também pode especificar uma subpasta ou um arquivo de modelo específico.

Quando **debug** for **false** em Web. config, o aplicativo incluirá o item "~/Bundles/templates" do pacote. Esse item de pacote é adicionado em BundleConfig.cs, usando a biblioteca do compilador Handlebars:

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
