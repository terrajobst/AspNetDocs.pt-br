---
uid: single-page-application/overview/templates/breezeknockout-template
title: Modelo de Breeze/Knockout | Microsoft Docs
author: madskristensen
description: Modelo de aplicativo de página única da Breeze/Knockout
ms.author: riande
ms.date: 01/30/2013
ms.assetid: 3bd94827-3c59-448f-abc3-36e6df4858db
msc.legacyurl: /single-page-application/overview/templates/breezeknockout-template
msc.type: authoredcontent
ms.openlocfilehash: 5bb9ee8f758a25afa6baf3ccbaf7d5864754c7df
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558187"
---
# <a name="breezeknockout-template"></a>Modelo Breeze/Knockout

por [Mads Kristensen](https://github.com/madskristensen)

> O modelo da Breeze/Knockout MVC foi escrito pela Bell em diante
> 
> [Baixar o modelo da Breeze/Knockout MVC](https://go.microsoft.com/fwlink/?LinkId=282649)

Você já ouviu falar de "aplicativo de página única" (SPA) e se perguntou o que ele é. Embora você possa ler sobre isso, você gostaria de experimentar por conta própria. Mas quem tem tempo para baixar um exemplo? Se você tiver o Visual Studio, terá um exemplo de SPA em execução em menos de 60 segundos com o modelo ASP.NET MVC 4 "Breeze/Knockout single page Application".

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

## <a name="what-is-the-breezeknockout-spa-template"></a>O que é o modelo de SPA da Breeze/Knockout?

A maioria dos modelos de projeto gera um esqueleto de aplicativo. Você coloca os Bones adicionando seu código e, eventualmente, entrega um aplicativo de trabalho. O modelo de SPA da Breeze/Knockout é diferente. Ele gera um aplicativo de exemplo para você estudar. Ele demonstra um design de aplicativo SPA e muitas das técnicas para a criação de um SPA.

O modelo Breeze/Knockout é uma variação no [modelo Spa KnockoutJS](../introduction/knockoutjs-template.md) incluído na atualização do ASP.net and Web Tools 2012,2. O modelo de SPA da Breeze gera um aplicativo com a mesma experiência do usuário, mas tem uma implementação diferente, usando a Breeze para o gerenciamento de dados.

O modelo SPA do KnockoutJS faz solicitações de serviço com o AJAX jQuery bruto, que é adequado para um aplicativo simples. Mas aplicativos mais sofisticados têm requisitos de gerenciamento de dados mais exigentes. Por exemplo, a maioria dos aplicativos:

- Consultar e consultar novamente o servidor durante uma sessão de usuário estendida.
- Adicionar filtros de consulta, classificação e paginação.
- Compartilhe os mesmos dados em várias telas.
- Acumule alterações em vários objetos e salve-os como uma única transação.
- Valide as alterações no cliente, para que o usuário possa corrigir os erros antes de confirmar as alterações no banco de dados.

A biblioteca BreezeJS lida com essas tarefas para você, liberando-o para desenvolver a lógica do aplicativo e a experiência do usuário mais importantes.

A [**Breeze**](http://www.breezejs.com/?utm_source=ms-spa) é uma biblioteca de software livre para a criação de aplicativos de dados avançados em JavaScript e HTML, os tipos de aplicativos que historicamente foram fornecidos como aplicativos de área de trabalho autônomos.

O modelo Breeze/Knockout ajuda você a realizar essa primeira etapa crucial em direção a uma infra-estrutura de gerenciamento de dados mais robusta. Ele produz um aplicativo de exemplo todo que é muito idêntico ao modelo SPA KnockoutJS. No interior, ele substitui a camada de dados AJAX pela Breeze, para que você possa comparar as duas abordagens lado a lado. É claro que ele não atinge o potencial de um aplicativo da Breeze. Mas você verá como a Breeze funciona e quanto pouco é necessário para fazer essa transição.

Vamos começar.

## <a name="create-a-breezeknockout-template-project"></a>Criar um projeto de modelo de Breeze/Knockout

Baixe e instale o modelo clicando no botão baixar acima. O modelo é empacotado como um arquivo de VSIX (extensão do Visual Studio). Talvez seja necessário reiniciar o Visual Studio.

No painel **modelos** , selecione **modelos instalados** e expanda o **nó C# Visual** . Em **Visual C#** , selecione **Web**. Na lista de modelos de projeto, selecione **aplicativo Web ASP.NET MVC 4**. Nomeie o projeto e clique em **OK**.

No assistente de **novo projeto** , selecione o **Spa de Knockout da Breeze**.

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeKOSpaTemplate.png)

Pressione CTRL-F5 para compilar e executar o aplicativo sem depuração ou pressione F5 para executar com a depuração.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

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

A lógica de validação é executada no lado do cliente pela Breeze. Os atributos de validação nas classes de modelo de servidor são propagados para o cliente e executados automaticamente antes que o cliente entre em contato com o servidor.

Examine o tráfego de rede. Observe que não havia chamadas para o servidor quando a Breeze detectou um erro. Cada alteração válida resultou em uma solicitação POST para "/api/Todo/SaveChanges". A Breeze agrupa as alterações e as envia como uma única solicitação ao método de `SaveChanges` do controlador da API Web. Isso é diferente do modelo SPA KnockoutJS, que faz solicitações PUT, POST e DELETE para cada item individualmente.

## <a name="peek-inside"></a>Inspecionar dentro

Esse aplicativo tem um lado do cliente e um servidor. A pilha do lado do cliente consiste em um pequeno HTML e uma combinação de módulos JavaScript do aplicativo (na pasta "aplicativo"), além de bibliotecas JavaScript de terceiros (na pasta "scripts").

![](http://www.breezejs.com/sites/all/images/spa-template/ClientArchitecture.png)

Se você investigou o modelo SPA KnockoutJS, isso deve parecer muito familiar. Concentre-se nas caixas azuis. A arquitetura da interface do usuário é Model-View-ViewModel (MVVM), na qual os widgets HTML da exibição são completamente separados do código de apresentação de suporte no modelo de exibição. Um sistema de associação de dados (knockout, neste caso) coordena a exibição e o modelo de exibição para que cada um possa fazer seu trabalho sem conhecimento profundo do outro.

O modelo encapsula os dados de tarefas pendentes. As entidades no modelo são construídas pela Breeze com as propriedades observáveis do knockout, para que possam ser vinculadas diretamente aos widgets na exibição. O View-Model solicita que o contexto de dados adquira e salve as entidades do modelo. O contexto de dados delega a maior parte do trabalho para a Breeze.

A pilha do lado do servidor consiste em um código de desenvolvedor e três bibliotecas .NET de princípio: API Web, Entity Framework e Breeze.NET:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

A arquitetura básica é a mesma do modelo SPA KnockoutJS. No entanto, a implementação é muito mais simples: os DTOs foram excluídos e a maioria dos detalhes de Entity Framework foi delegada para Breeze.NET.

## <a name="next-steps"></a>Próximas etapas

Sugerimos que você explore o código, guiado pela [ampla discussão](http://www.breezejs.com/spa-template?utm_source=ms-spa) do cliente e das pilhas de servidor no site da Breeze.

Você pode tentar reproduzir com a consulta do lado do cliente da Breeze; Adicione alguns filtros e classificações. Você pode adicionar mais propriedades de modelo e mais entidades para obter uma ideia melhor para o desenvolvimento de SPA de ponta a ponta. Quando você estiver confiante do design, poderá desmontar os recursos de tarefas e substituí-los pelos seus próprios.

Em breve, você estará pronto para a próxima etapa grande: Adicionar telas do lado do cliente e navegar entre elas. Você deixará esse modelo SPA para trás e passará para uma pilha SPA mais completa, como a [toalha quente de John Papa](https://github.com/johnpapa/HotTowel#readme "Toalha quente"), que adiciona Durandal à Breeze e à mistura de Knockout.
