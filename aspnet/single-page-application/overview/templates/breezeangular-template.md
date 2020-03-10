---
uid: single-page-application/overview/templates/breezeangular-template
title: Modelo de Breeze/angular | Microsoft Docs
author: madskristensen
description: Modelo de aplicativo de página única da Breeze/angular
ms.author: riande
ms.date: 03/08/2013
ms.assetid: db31e909-563a-4516-aadd-62aa210ac7e4
msc.legacyurl: /single-page-application/overview/templates/breezeangular-template
msc.type: authoredcontent
ms.openlocfilehash: 3e4e63d385a56d51d3d08696782b43d6228f6201
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578536"
---
# <a name="breezeangular-template"></a>Modelo Breeze/Angular

por [Mads Kristensen](https://github.com/madskristensen)

> O modelo da Breeze/angular MVC foi escrito pela Bell em diante
> 
> [Baixar o modelo Breeze/angular MVC](https://go.microsoft.com/fwlink/?LinkId=286437)

O [AngularJS](http://angularjs.org) é uma biblioteca de software livre do Google para criar aplicativos de página única (spas). Ele oferece Associação de dados, injeção de dependência e gerenciamento de tela. Combine-o com o [BreezeJS](http://www.breezejs.com/?utm_source=ms-spa), outra biblioteca de software livre para modelagem de dados e gerenciamento de dados, e você tem os ingredientes essenciais para um excelente aplicativo cliente HTML/JavaScript.

O modelo de SPA da Breeze/angular é uma variação no [modelo Spa KnockoutJS](../introduction/knockoutjs-template.md) incluído na atualização do ASP.net and Web Tools 2012,2. Se você tem o Visual Studio, terá um exemplo de SPA em execução em menos de 60 segundos.

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningTodoPage.png)

Por fim, o aplicativo parece muito semelhante ao modelo SPA KnockoutJS. Mas é bem diferente nos bastidores. O modelo KnockoutJS usa o Knockout para vinculação de dados e AJAX bruto para acesso a dados. O modelo Breeze/angular usa angular para vinculação de dados e Breeze para acesso a dados. Essas bibliotecas permitem recursos adicionais, incluindo a navegação por página e o histórico.

Aqui está a página sobre do aplicativo:

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningAboutPage.png)

Esta página exibe um log de eventos em execução durante a sessão de usuário atual, incluindo:

- Houver. Observe a criação do controlador todo em #2 e #7.
- Consultas remotas (#3) e consultas de cache local (#7).
- Salvando novas entidades (#5, #6) e modificadas (#4).
- As alterações são validadas no cliente (#9), para que o usuário possa corrigir os erros antes de confirmar as alterações no banco de dados.

Há mais a explorar neste modelo, incluindo:

- Carregamento dinâmico de modelos de exibição de HTML.
- Vinculação de dados personalizada por meio de "diretivas" angulares.
- Modularidade e injeção de dependência.
- Filtros de consulta, classificações, paginação, projeções e inclusão de entidades relacionadas.
- Compartilhando dados em várias telas.
- Salvar várias alterações como uma única transação.
- Regras de validação propagadas automaticamente do servidor para o cliente JavaScript.

Vamos começar.

## <a name="create-a-breezeangular-template-project"></a>Criar um projeto de modelo de Breeze/angular

Baixe e instale o modelo clicando no botão baixar acima. O modelo é empacotado como um arquivo de VSIX (extensão do Visual Studio). Talvez seja necessário reiniciar o Visual Studio.

No painel **modelos** , selecione **modelos instalados** e expanda o **nó C# Visual** . Em **Visual C#** , selecione **Web**. Na lista de modelos de projeto, selecione **aplicativo Web ASP.NET MVC 4**. Nomeie o projeto e clique em **OK**.

No assistente de **novo projeto** , selecione **Spa angular da Breeze**.

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeNgSpaTemplate.png)

Pressione CTRL-F5 para compilar e executar o aplicativo sem depuração ou pressione F5 para executar com a depuração.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrLogin.png)

Quando o aplicativo é executado pela primeira vez, ele exibe uma tela de logon. Clique no link "inscrever-se" e uma nova página deslizará para a exibição, onde você pode inserir um nome de usuário e uma senha. (As páginas de logon e de registro são criadas usando o ASP.NET MVC.) Quando você envia o formulário de registro, o servidor gera uma lista de tarefas pendentes com dois itens para sua conta. Em seguida, ele os apresenta em uma nota amarela.

![](http://www.breezejs.com/sites/all/images/spa-template/TodoList.png)

Agora você está no solo do SPA. Tudo o que você vê e experimenta durante a manipulação de todo é renderizado e gerenciado no cliente com a ajuda do Knockout e da Breeze. Explorar o aplicativo como um usuário... Mas com os olhos de um desenvolvedor. Use as ferramentas de desenvolvedor em seu navegador para capturar o tráfego de rede. (No Internet Explorer: pressione F12, selecione a guia **rede** e clique em **Iniciar captura**.) Agora, tente o seguinte:

- Adicione um novo item de tarefas pendentes.
- Clique no rótulo e edite o título do item todo
- Marque uma caixa de seleção para marcar o item concluído. Observe que a caixa de texto está desabilitada, portanto o título não é mais editável.
- Clique no ' x ' à direita do rótulo. O item desaparece e é excluído do banco de dados.
- Escolha outro item e limpe seu título. Você receberá um erro de validação de que o título é necessário. Após uma breve pausa, o título anterior é restaurado.
- Digite um título longo de absurdamente. Você obterá um erro de validação diferente de que o título é muito longo.
- Clique no botão "Adicionar lista de tarefas pendentes". Uma nova lista é exibida à esquerda da lista anterior.
- Brincar com o título de ToDoList, disparando suas validações necessárias e de comprimento.
- Clique na caixa de texto título para limpar a mensagem de erro.
- Clique no "x" no círculo no canto superior direito para excluir o ToDoList e seus respectivos.
- Clique no link "sobre" no canto superior direito para ver um log dessas atividades.

A lógica de validação é executada no lado do cliente pela Breeze. Os atributos de validação nas classes de modelo de servidor são propagados para o cliente e executados automaticamente antes que o cliente entre em contato com o servidor.

Examine o tráfego de rede. Observe que não havia chamadas para o servidor quando a Breeze detectou um erro. Cada alteração válida resultou em uma solicitação POST para "/api/Todo/SaveChanges". A Breeze agrupa as alterações e as envia como uma única solicitação ao método de `SaveChanges` do controlador da API Web. Isso é diferente do modelo SPA KnockoutJS, que faz solicitações PUT, POST e DELETE para cada item individualmente.

Além disso, observe que não há tráfego de rede quando você alterna entre as páginas ToDoList e sobre. Isso ocorre porque a consulta foi restrita ao cache da Breeze local.

## <a name="peek-inside"></a>Inspecionar dentro

Esse aplicativo tem um lado do cliente e um servidor. A pilha do lado do cliente consiste em um pequeno HTML e uma combinação de módulos JavaScript do aplicativo (na pasta "aplicativo"), além de bibliotecas JavaScript de terceiros (na pasta "scripts").

![](http://www.breezejs.com/sites/all/images/spa-template/NgClientArchitecture2.png)

A arquitetura da interface do usuário separa os widgets HTML dos modos de exibição do código de apresentação de suporte nos controladores. O sistema de vinculação de dados angular coordena exibições e controladores para que cada um possa fazer seu trabalho sem conhecimento profundo do outro.

O controlador solicita que o contexto de dados adquira e salve as entidades do modelo. O contexto de dados delega a maior parte do trabalho à Breeze, que constrói objetos de modelo de autocontrole dos resultados da consulta JSON.

A pilha do lado do servidor consiste em um código de desenvolvedor e três bibliotecas .NET de princípio: API Web, Entity Framework e Breeze.NET:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

A arquitetura básica é a mesma do modelo SPA KnockoutJS. No entanto, a implementação é muito mais simples: os DTOs foram excluídos e a maioria dos detalhes de Entity Framework foi delegada para Breeze.NET.

## <a name="next-steps"></a>Próximas etapas

Sugerimos que você explore o código, guiado pela [ampla discussão](http://www.breezejs.com/ng-spa-template?utm_source=ms-spa) do cliente e das pilhas de servidor no site da Breeze.

Você pode tentar reproduzir com a consulta do lado do cliente da Breeze; Adicione alguns filtros e classificações. Você pode adicionar mais propriedades de modelo e mais entidades para obter uma ideia melhor para o desenvolvimento de SPA de ponta a ponta. Quando você estiver confiante do design, poderá desmontar os recursos de tarefas e substituí-los pelos seus próprios.

Boa codificação!
