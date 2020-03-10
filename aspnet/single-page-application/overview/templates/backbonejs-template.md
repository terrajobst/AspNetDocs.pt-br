---
uid: single-page-application/overview/templates/backbonejs-template
title: Modelo de backbone | Microsoft Docs
author: madskristensen
description: Modelo SPA do backbone. js
ms.author: riande
ms.date: 04/04/2013
ms.assetid: 00aca413-f067-4108-9bd1-cf21e64a2646
msc.legacyurl: /single-page-application/overview/templates/backbonejs-template
msc.type: authoredcontent
ms.openlocfilehash: 7297db7d5b35a53b40f9d9162960e529a167bd12
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558222"
---
# <a name="backbone-template"></a>Modelo Backbone

por [Mads Kristensen](https://github.com/madskristensen)

> O modelo de SPA de backbone foi escrito por Kazi Manzur Rashid
> 
> [Baixar o modelo SPA do backbone. js](https://go.microsoft.com/fwlink/?LinkId=293631)

O modelo SPA do backbone. js foi projetado para começar a criar rapidamente aplicativos Web do lado do cliente interativos usando o [backbone. js.](http://backbonejs.org/)

O modelo fornece um esqueleto inicial para o desenvolvimento de um aplicativo backbone. js no ASP.NET MVC. Ele fornece a funcionalidade básica de logon de usuário, incluindo a inscrição, a entrada, a redefinição de senha e a confirmação do usuário com modelos de email básicos.

Requisitos:

- [Atualização do ASP.NET and Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650)

## <a name="create-a-backbone-template-project"></a>Criar um projeto de modelo de backbone

Baixe e instale o modelo clicando no botão baixar acima. O modelo é empacotado como um arquivo de VSIX (extensão do Visual Studio). Talvez seja necessário reiniciar o Visual Studio.

No painel **modelos** , selecione **modelos instalados** e expanda o **nó C# Visual** . Em **Visual C#** , selecione **Web**. Na lista de modelos de projeto, selecione **aplicativo Web ASP.NET MVC 4**. Nomeie o projeto e clique em **OK**.

![](backbonejs-template/_static/image1.png)

No assistente de **novo projeto** , selecione projeto Spa do backbone. js.

![](backbonejs-template/_static/image2.png)

Pressione CTRL-F5 para compilar e executar o aplicativo sem depuração ou pressione F5 para executar com a depuração.

![](backbonejs-template/_static/image3.png)

Clicar em "minha conta" abre a página de logon:

![](backbonejs-template/_static/image4.png)

## <a name="walkthrough-client-code"></a>Walkthrough: código do cliente

Vamos começar com o lado do cliente. Os scripts de aplicativo cliente estão localizados na pasta ~/scripts/Application. O aplicativo é gravado em [TypeScript](http://www.typescriptlang.org/) (arquivos. TS) que são compilados em JavaScript (arquivos. js).

**Aplicativo**

`Application` é definido em Application. TS. Esse objeto inicializa o aplicativo e atua como o namespace raiz. Ele mantém informações de configuração e estado que são compartilhadas pelo aplicativo, como se o usuário está conectado.

O método `application.start` cria as exibições modais e anexa manipuladores de eventos para eventos no nível do aplicativo, como entrada do usuário. Em seguida, ele cria o roteador padrão e verifica se qualquer URL do lado do cliente é especificada. Caso contrário, ele redireciona para a URL padrão (#!/).

**Eventos**

Os eventos são sempre importantes ao desenvolver componentes menos rígidos. Os aplicativos geralmente executam várias operações em resposta a uma ação do usuário. O backbone fornece eventos internos com componentes como modelo, coleção e exibição. Em vez de criar entre dependências entre esses componentes, o modelo usa um modelo "pub/sub": o objeto `events`, definido em Events. TS, atua como um hub de eventos para publicação e assinatura de eventos de aplicativo. O objeto `events` é um singleton. O código a seguir mostra como assinar um evento e, em seguida, disparar o evento:

[!code-csharp[Main](backbonejs-template/samples/sample1.cs)]

**Rota**

No backbone. js, um roteador fornece métodos para rotear páginas do lado do cliente e conectá-las a ações e eventos. O modelo define um único roteador, em roteador. TS. O roteador cria os modos de exibição activable e mantém o estado ao alternar entre exibições. (As exibições do Activable são descritas na próxima seção.) Inicialmente, o projeto tem duas exibições fictícias, página inicial e sobre. Ele também tem uma exibição não encontrada, que será exibida se a rota não for conhecida.

**Exibições**

Os modos de exibição são definidos em ~/scripts/Application/views. Há dois tipos de exibições, exibições activable e exibições de caixas restritas. As exibições do Activable são invocadas pelo roteador. Quando uma exibição activable é mostrada, todas as outras exibições de activable se tornam inativas. Para criar uma exibição activable, estenda a exibição com o objeto `Activable`:

[!code-javascript[Main](backbonejs-template/samples/sample2.js)]

A extensão com o `Activable` adiciona dois novos métodos à exibição, `activate` e `deactivate`. O roteador chama esses métodos para ativar e desativar o modo de exibição.

As exibições modais são implementadas como caixas de diálogo modais de [inicialização do Twitter](https://twitter.github.com/bootstrap/) . As exibições `Membership` e `Profile` são exibições modais. Os modos de exibição de modelo podem ser invocados por qualquer evento de aplicativo. Por exemplo, na exibição `Navigation`, clicar no link "minha conta" mostra a exibição `Membership` ou a `Profile` exibição, dependendo se o usuário está conectado. O `Navigation` anexa manipuladores de eventos de clique a qualquer elemento filho que tenha o atributo `data-command`. Aqui está a marcação HTML:

[!code-html[Main](backbonejs-template/samples/sample3.html)]

Aqui está o código em Navigation. TS para conectar os eventos:

[!code-csharp[Main](backbonejs-template/samples/sample4.cs)]

**Modelos**

Os modelos são definidos em ~/scripts/Application/Models. Todos os modelos têm três coisas básicas: atributos padrão, regras de validação e um ponto de extremidade do servidor. Veja um exemplo típico:

[!code-javascript[Main](backbonejs-template/samples/sample5.js)]

**Plug-ins**

A pasta ~/scripts/Application/lib contém alguns plug-ins do jQuery úteis. O arquivo Form. TS define um plug-in para trabalhar com dados de formulário. Geralmente, você precisa serializar ou desserializar dados de formulário e mostrar quaisquer erros de validação de modelo. O plug-in do formulário. TS tem métodos como `serializeFields`, `deserializeFields`e `showFieldErrors`. O exemplo a seguir mostra como serializar um formulário em um modelo.

[!code-javascript[Main](backbonejs-template/samples/sample6.js)]

O plug-in Flashbar. TS fornece vários tipos de mensagens de comentários para o usuário. Os métodos são `$.showSuccessbar`, `$.showErrorbar` e `$.showInfobar`. Nos bastidores, ele usa alertas de inicialização do Twitter para mostrar mensagens bem animadas.

O plug-in Confirm. TS substitui a caixa de diálogo confirmar do navegador, embora a API seja um pouco diferente:

[!code-javascript[Main](backbonejs-template/samples/sample7.js)]

## <a name="walkthrough-server-code"></a>Walkthrough: código do servidor

Agora, vamos examinar o lado do servidor.

**Controladores**

Em um aplicativo de página única, o servidor desempenha apenas uma pequena função na interface do usuário. Normalmente, o servidor renderiza a página inicial e, em seguida, envia e recebe dados JSON.

O modelo tem dois controladores MVC: `HomeController` renderiza a página inicial e `SupportsController` é usado para confirmar novas contas de usuário e redefinir senhas. Todos os outros controladores no modelo são ASP.NET Web API controladores, que enviam e recebem dados JSON. Por padrão, os controladores usam a nova classe `WebSecurity` para executar tarefas relacionadas ao usuário. No entanto, eles também têm construtores opcionais que permitem que você passe os delegados para essas tarefas. Isso facilita o teste e permite que você substitua `WebSecurity` com algo mais, usando um contêiner IoC. Veja um exemplo:

[!code-csharp[Main](backbonejs-template/samples/sample8.cs)]

## <a name="views"></a>Exibições

As exibições foram projetadas para serem modulares: cada seção de uma página tem sua própria exibição dedicada. Em um aplicativo de página única, é comum incluir exibições que não têm nenhum controlador correspondente. Você pode incluir uma exibição chamando `@Html.Partial('myView')`, mas isso é entediante. Para tornar isso mais fácil, o modelo define um método auxiliar, `IncludeClientViews`, que renderiza todas as exibições em uma pasta especificada:

[!code-cshtml[Main](backbonejs-template/samples/sample9.cshtml)]

Se o nome da pasta não for especificado, o nome da pasta padrão será "ClientViews". Se a exibição de cliente também usar exibições parciais, nomeie a exibição parcial com um caractere de sublinhado (por exemplo, `_SignUp`). O método `IncludeClientViews` exclui todas as exibições cujo nome começa com um sublinhado. Para incluir uma exibição parcial na exibição do cliente, chame `Html.ClientView('SignUp')` em vez de `Html.Partial('_SignUp')`.

**Enviando email**

Para enviar email, o modelo usa o [CEP](http://aboutcode.net/postal). No entanto, o CEP é abstrato do restante do código com a interface `IMailer`, para que você possa substituí-lo facilmente por outra implementação. Os modelos de email estão localizados na pasta views/emails. O endereço de email do remetente é especificado no arquivo Web. config, na chave `sender.email` da seção **appSettings** . Além disso, quando `debug="true"` no Web. config, o aplicativo não exige confirmação de email do usuário para acelerar o desenvolvimento.

## <a name="github"></a>GitHub

Você também pode encontrar o modelo de SPA do backbone. js no [GitHub](https://github.com/kazimanzurrashid/AspNetMvcBackboneJsSpa).
