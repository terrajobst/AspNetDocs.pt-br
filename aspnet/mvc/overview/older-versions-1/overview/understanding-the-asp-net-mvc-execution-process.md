---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: Noções básicas sobre o processo de execução do ASP.NET MVC | Microsoft Docs
author: microsoft
description: Saiba como o ASP.NET MVC Framework processa uma solicitação de navegador passo a passo.
ms.author: riande
ms.date: 01/27/2009
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: 28940947253e0af43886cf1231f8aaf4615526cc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78541513"
---
# <a name="understanding-the-aspnet-mvc-execution-process"></a>Noções básicas sobre o processo de execução do ASP.NET MVC

pela [Microsoft](https://github.com/microsoft)

> Saiba como o ASP.NET MVC Framework processa uma solicitação de navegador passo a passo.

As solicitações para um aplicativo Web baseado em ASP.NET MVC primeiro passam pelo objeto **UrlRoutingModule** , que é um módulo http. Este módulo analisa a solicitação e executa a seleção de rota. O objeto **UrlRoutingModule** seleciona o primeiro objeto de rota que corresponde à solicitação atual. (Um objeto de rota é uma classe que implementa **RouteBase**e normalmente é uma instância da classe **Route** .) Se nenhuma rota corresponder, o objeto **UrlRoutingModule** não fará nada e permitirá que a solicitação volte para o processamento de solicitações ASP.net ou IIS regular.

A partir do objeto de **rota** selecionado, o objeto **UrlRoutingModule** Obtém o objeto **IRouteHandler** associado ao objeto de **rota** . Normalmente, em um aplicativo MVC, essa será uma instância de **MvcRouteHandler**. A instância **IRouteHandler** cria um objeto **IHttpHandler** e o passa para o objeto **IHttpContext** . Por padrão, a instância **IHttpHandler** para MVC é o objeto **MvcHandler** . Em seguida, o objeto **MvcHandler** seleciona o controlador que, por fim, manipulará a solicitação.

> [!NOTE]
> Quando um aplicativo Web ASP.NET MVC é executado no IIS 7,0, nenhuma extensão de nome de arquivo é necessária para projetos do MVC. No entanto, no IIS 6,0, o manipulador requer que você mapeie a extensão de nome de arquivo. Mvc para a DLL ISAPI ASP.NET.

O módulo e o manipulador são os pontos de entrada para a estrutura MVC do ASP.NET. Eles executam as seguintes ações:

- Selecione o controlador apropriado em um aplicativo Web MVC.
- Obtenha uma instância de controlador específica.
- Chame o método **Execute** do controlador.

O seguinte lista os estágios de execução de um projeto Web MVC:

- Receber a primeira solicitação para o aplicativo 

    - No arquivo global. asax, os objetos de **rota** são adicionados ao objeto **RouteTable** .
- Executar roteamento 

    - O **módulo UrlRoutingModule** usa o primeiro objeto **de rota** correspondente na coleção **RouteTable** para criar o objeto **RouteData** , que ele usa para criar um objeto **RequestContext** (**IHttpContext**).
- Criar manipulador de solicitação MVC 

    - O objeto **MvcRouteHandler** cria uma instância da classe **MvcHandler** e a passa para a instância de **RequestContext** .
- Criar controlador 

    - O objeto **MvcHandler** usa a instância **RequestContext** para identificar o objeto **IControllerFactory** (normalmente uma instância da classe **DefaultControllerFactory** ) para criar a instância do controlador com o.
- Executar controlador-a instância **MvcHandler** chama o método **Execute** do controlador. |
- Ação de invocação 

    - A maioria dos controladores herdam da classe base do **controlador** . Para controladores que fazem isso, o objeto **ControllerActionInvoker** associado ao controlador determina qual método de ação da classe de controlador chamar e, em seguida, chama esse método.
- Executar resultado 

    - Um método de ação típico pode receber entrada do usuário, preparar os dados de resposta apropriados e, em seguida, executar o resultado retornando um tipo de resultado. Os tipos de resultados internos que podem ser executados incluem o seguinte: **ViewResult** (que renderiza uma exibição e é o tipo de resultado usado com mais frequência), **RedirectToRouteResult**, **RedirectResult**, **ContentResult**, **JsonResult**e **EmptyResult**.
