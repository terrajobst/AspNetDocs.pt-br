---
uid: mvc/overview/older-versions-1/overview/asp-net-mvc-overview
title: Visão geral do ASP.NET MVC | Microsoft Docs
author: microsoft
description: Saiba mais sobre as diferenças entre o aplicativo ASP.NET MVC e os aplicativos de ASP.NET Web Forms. Saiba como decidir quando criar um aplicativo MVC ASP.NET.
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 2dcb44a4-5cbf-4d62-b363-718104082d86
msc.legacyurl: /mvc/overview/older-versions-1/overview/asp-net-mvc-overview
msc.type: authoredcontent
ms.openlocfilehash: 73965c71f37de13e3813df089a253fde528ea7ee
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78541555"
---
# <a name="aspnet-mvc-overview"></a>Visão geral do ASP.NET MVC

pela [Microsoft](https://github.com/microsoft)

> Saiba mais sobre as diferenças entre o aplicativo ASP.NET MVC e os aplicativos de ASP.NET Web Forms. Saiba como decidir quando criar um aplicativo MVC ASP.NET.

O padrão arquitetônico do MVC (Model-View-Controller) separa um aplicativo em três componentes principais: o modelo, a exibição e o controlador. O ASP.NET MVC Framework fornece uma alternativa ao padrão de Web Forms ASP.NET para a criação de aplicativos Web baseados em MVC. A estrutura ASP.NET MVC é uma estrutura de apresentação leve e altamente testável que (à semelhança dos aplicativos baseados em Web Forms) é integrada aos recursos ASP.NET existentes, como páginas mestras e autenticação baseada em associação. A MVC Framework é definida no namespace **System. Web. Mvc** e é uma parte fundamental e com suporte do namespace **System. Web** .   
  
O MVC é um padrão de design padrão que muitos desenvolvedores conhecem. Alguns tipos de aplicativos Web irão se beneficiar da estrutura MVC. Outros vão continuar usando o padrão de aplicativo ASP.NET tradicional que é baseado em Web Forms e postbacks. Outros tipos de aplicativos Web irão combinar as duas abordagens; uma abordagem não exclui a outra.   
  
A estrutura MVC inclui os seguintes componentes:

[![invocando uma ação do controlador que espera um valor de parâmetro](asp-net-mvc-overview/_static/image1.jpg)](asp-net-mvc-overview/_static/image1.png)

**Figura 01**: invocando uma ação do controlador que espera um valor de parâmetro ([clique para exibir a imagem em tamanho normal](asp-net-mvc-overview/_static/image2.png))

- **Modelos**. Objetos de modelo são as partes do aplicativo que implementam a lógica para o domínio de dados do aplicativo. Muitas vezes, os objetos de modelo recuperam e armazenam o estado do modelo em um banco de dados. Por exemplo, um objeto de produto pode recuperar informações de um banco de dados, operar nele e, em seguida, gravar informações atualizadas de volta em uma tabela de produtos no SQL Server.

Em aplicativos pequenos, o modelo, muitas vezes, é uma separação conceitual em vez de física. Por exemplo, se o aplicativo lê apenas um conjunto de dados e o envia para o modo de exibição, o aplicativo não tem uma camada de modelo físico e classes associadas. Nesse caso, o conjunto de dados assume a função de um objeto de modelo.

- **Exibições**. As exibições são os componentes que exibem a interface do usuário do aplicativo. Normalmente, esta IU é criada a partir dos dados do modelo. Um exemplo seria um modo de exibição de edição de uma tabela Products que exibe caixas de texto, listas suspensas e caixas de seleção com base no estado atual de um objeto Products.

- **Controladores**. Os controladores são os componentes que lidam com a interação do usuário, trabalham com o modelo e, finalmente, selecionam uma exibição de renderização que mostra essa IU. Em um aplicativo MVC, a exibição só mostra informações; o controlador manipula e responde à entrada e à interação do usuário. Por exemplo, o controlador manipula valores de cadeia de caracteres de consulta e passa esses valores para o modelo, que, por sua vez, consulta o banco de dados usando os valores.

O padrão MVC ajuda a criar aplicativos que separam os diferentes aspectos do aplicativo (lógica de entrada, lógica de negócio e lógica da IU), enquanto fornece um acoplamento flexível entre esses elementos. O padrão especifica onde cada tipo de lógica deve ficar localizado no aplicativo. A lógica da IU fica na exibição. A lógica de entrada fica no controlador. A lógica de negócios fica no modelo. Essa separação ajuda a administrar a complexidade quando você cria um aplicativo, porque ela permite que você se concentre em um aspecto da implementação por vez. Por exemplo, você pode se concentrar na exibição sem depender da lógica de negócios.   
  
Além de administrar a complexidade, o padrão MVC torna o teste de aplicativos mais fácil do que o teste de um aplicativo Web ASP.NET baseado em Web Forms. Por exemplo, num aplicativo Web ASP.NET baseado em Web Forms, uma única classe é usada para exibir a saída e para responder à entrada do usuário. Escrever testes automatizados para aplicativos ASP.NET baseados em Web Forms pode ser complexo, pois para testar uma página individual, você deve criar uma instância da classe da página, de todos os seus controles filhos e das classes dependentes adicionais no aplicativo. Uma vez que são criadas instâncias de tantas classes para executar a página, pode ser complicado escrever testes que se foquem exclusivamente em partes individuais do aplicativo. Os testes para aplicativos ASP.NET baseados em Web Forms podem, por isto, ser mais difíceis de implementar do que testes em um aplicativo MVC. Além disso, os testes em aplicativos ASP.NET baseados em Web Forms exigem um servidor Web. A estrutura MVC separa os componentes e usa interfaces intensamente, o que torna possível testar componentes individuais isoladamente do resto da estrutura.   
  
O acoplamento flexível entre os três componentes principais de um aplicativo MVC também promove o desenvolvimento paralelo. Por exemplo, um desenvolvedor pode trabalhar na exibição, um segundo desenvolvedor pode trabalhar na lógica do controlador e um terceiro desenvolvedor pode se concentrar na lógica de negócios no modelo.

## <a name="deciding-when-to-create-an-mvc-application"></a>Decidindo quando criar um aplicativo MVC

Você deve considerar cuidadosamente se deve implementar um aplicativo Web usando a estrutura ASP.NET MVC ou o modelo de Web Forms do ASP.NET. A estrutura MVC não substitui o modelo de Web Forms; você pode usar ambas as estruturas para aplicativos Web. (Se você tem aplicativos existentes baseados em Web Forms, eles continuarão a funcionar exatamente como funcionavam antes).   
  
Antes de você decidir usar a estrutura MVC ou o modelo de Web Forms para um determinado site, pondere as vantagens de cada abordagem.

### <a name="advantages-of-an-mvc-based-web-application"></a>Vantagens de um aplicativo Web baseado em MVC

A estrutura ASP.NET MVC oferece as seguintes vantagens:

- Ela torna mais fácil gerenciar a complexidade ao dividir o aplicativo em modelo, exibição e controlador.
- Ela não utiliza o estado de exibição nem formulários baseados no servidor. Isto torna a estrutura MVC ideal para desenvolvedores que desejam controle completo sobre o comportamento do aplicativo.
- Ela usa um padrão Front Controller que processa as solicitações do aplicativo Web através de um único controlador. Isto permite desenvolver um aplicativo que suporta uma poderosa infraestrutura de roteamento. Para obter mais informações, consulte [front Controller](https://go.microsoft.com/fwlink/?LinkId=106357 "Controlador frontal") no site do MSDN.
- Ela fornece um melhor suporte para desenvolvimento controlado por testes (TDD – test-driven development).
- Ele funciona bem para aplicativos Web que têm suporte em grandes equipes de desenvolvedores e Web designers que precisam de um alto grau de controle sobre o comportamento do aplicativo.

### <a name="advantages-of-a-web-forms-based-web-application"></a>Vantagens de um aplicativo Web baseado em Web Forms

A estrutura baseada em Web Forms oferece as seguintes vantagens:

- Ela suporta um modelo de evento que preserva o estado sobre HTTP, o que beneficia o desenvolvimento de aplicativos Web de linha de negócios. O aplicativo baseado em Web Forms fornece dezenas de eventos que são suportados em centenas de controles de servidor.
- Ela usa um padrão Page Controller que adiciona funcionalidade em páginas individuais. Para obter mais informações, consulte [controlador de página](https://go.microsoft.com/fwlink/?LinkId=106359 "Controlador de página") no site do MSDN.
- Ele usa o estado de exibição ou formulários baseados em servidor, o que pode facilitar o gerenciamento de informações de estado.
- Ela funciona bem com pequenas equipes de desenvolvedores e Web designers que desejem tirar proveito do grande número de componentes disponíveis para um rápido desenvolvimento do aplicativo.
- Em geral, é menos complexo para o desenvolvimento de aplicativos, pois os componentes (a classe de **página** , os controles e assim por diante) são totalmente integrados e geralmente exigem menos código do que o modelo MVC.

## <a name="features-of-the-aspnet-mvc-framework"></a>Recursos da estrutura ASP.NET MVC

A estrutura ASP.NET MVC fornece os seguintes recursos:

- Separação de tarefas de aplicativo (lógica de entrada, lógica de negócios e lógica de interface do usuário), capacidade de teste e desenvolvimento orientado a testes (TDD) por padrão. Todos os contratos núcleo na estrutura MVC são baseados na interface e podem ser testados usando objetos fictícios, que são objetos simulados que imitam o comportamento dos objetos reais no aplicativo. Você pode testar a unidade do aplicativo sem ter que executar os controladores em um processo ASP.NET, o que acelera e flexibiliza o teste da unidade. Você pode usar qualquer estrutura de teste da unidade que seja compatível com o .NET Framework.
- Uma estrutura extensível e conectável. Os componentes da estrutura ASP.NET MVC são desenvolvidos para serem facilmente substituídos ou personalizados. É possível conectar o seu próprio mecanismo de exibição, política de roteamento de URL, serialização de parâmetro ação-método e outros componentes. A estrutura ASP.NET MVC também suporta o uso dos modelos de contêiner de DI (Dependency Injection – Injeção de Dependência) e IOC (Inversion of Control – Inversão de Controle). DI permite injetar objetos em uma classe, em vez de depender da classe para criar o objeto em si. A IOC especifica que se um objeto requer outro objeto, os primeiros objetos devem obter o segundo objeto de uma fonte exterior como um arquivo de configuração. Isto facilita os testes.
- Um poderoso componente de mapeamento de URL que permite criar aplicativos com abrangente e URLs pesquisáveis. As URLs não precisam incluir extensões de nome de arquivo e são desenvolvidas para suportar padrões de denominação de URLs que funcionam bem para SEO (Search Engine Optimization – Otimização do Mecanismo de Pesquisa) e endereçamento REST (Representational State Transfer – Transferência de Estado Representacional).
- Suporte ao uso de marcação em arquivos de marcação de páginas ASP.NET (arquivos .aspx), de controle de usuário (arquivos .ascx) e de página mestra (arquivos .master) existentes como modelos de exibição. Você pode usar os recursos existentes do ASP.NET com a estrutura MVC do ASP.NET, como páginas mestras aninhadas, expressões embutidas (&lt;% =%&gt;), controles de servidor declarativos, modelos, associação de dados, localização e assim por diante.
- Suporte para recursos ASP.NET existentes. A estrutura ASP.NET MVC permite a utilização de recursos como autenticação de formulários e autenticação do Windows, autorização de URLs, associação e funções, caching de saída e dados, gerenciamento de estado de sessão e perfil, monitoramento de integridade, sistema de configuração e arquitetura de provedor.
