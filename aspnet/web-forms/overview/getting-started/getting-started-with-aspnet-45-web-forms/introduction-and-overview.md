---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Introdução com o ASP.NET 4,7 Web Forms e o Visual Studio 2017 | Microsoft Docs
author: Erikre
description: Esta série de tutoriais passo a passo ensinará as noções básicas da criação de um aplicativo ASP.NET Web Forms usando o ASP.NET 4,7 e o Microsoft Visual Studio
ms.author: riande
ms.date: 01/09/2019
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: 52d5eb7abe4520ebdf6667d299d055fc7619a635
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78568218"
---
# <a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2017"></a>Introdução com o ASP.NET 4,5 Web Forms e o Visual Studio 2017

[Baixar o projeto de exemplo WingtipC#Toys ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [baixar o livro eletrônico (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

Esta série de tutoriais mostra como criar um aplicativo ASP.NET Web Forms com o ASP.NET 4,5 e o Microsoft Visual Studio 2017. 

## <a name="introduction"></a>Introdução

Esta série de tutoriais orienta você pela criação de um aplicativo ASP.NET Web Forms usando o Visual Studio 2017 e o ASP.NET 4,5. Você criará um aplicativo chamado **Wingtip Toys** – um site de vitrine simplificado vendendo itens online. Durante a série, novos recursos do ASP.NET 4,5 são realçados.

### <a name="target-audience"></a>Público-alvo

Os desenvolvedores novos no ASP.NET Web Forms são o público-alvo desta série de tutoriais.

Você deve ter algum conhecimento nas seguintes áreas:

- Programação orientada a objeto (OOP) e linguagens
- Desenvolvimento para a Web (HTML, CSS, JavaScript)
- Bancos de dados relacionais
- Arquitetura de N camadas

Para examinar essas áreas, considere estudar o seguinte conteúdo:

- [Introdução com VisualC#](https://msdn.microsoft.com/library/a72418yk.aspx)
- [Desenvolvimento](https://msdn.microsoft.com/beginner/bb308760.aspx)para a Web, [HTML, CSS, JavaScript, SQL, PHP, jQuery](http://w3schools.com/)
- [Banco de dados relacional](http://en.wikipedia.org/wiki/Relational_database)
- [Arquitetura de várias camadas](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a>Recursos do aplicativo

Os recursos de formulário da Web do ASP.NET apresentados nesta série incluem:

- O projeto de aplicativo Web (não o projeto de site da Web)
- Formulários da Web
- Páginas mestras, configuração
- Inicialização
- Entity Framework Code First, LocalDB
- Validação de solicitação
- Controles de dados fortemente tipados
- Model binding
- Anotações de dados
- Provedores de valor
- SSL e OAuth
- ASP.NET Identity, configuração e autorização
- Validação não invasiva
- Roteamento
- Tratamento de erro do ASP.NET

### <a name="application-scenarios-and-tasks"></a>Cenários e tarefas de aplicativos

As tarefas da série de tutoriais incluem:

- Criando, revisando e executando um novo projeto
- Criando uma estrutura de banco de dados
- Inicializando e propagando um banco de dados
- Personalizando a interface do usuário com estilos, elementos gráficos e uma página mestra
- Adicionando páginas e navegação
- Exibindo detalhes do menu e dados do produto
- Criando um carrinho de compras
- Adicionando suporte a SSL e OAuth
- Adicionando um método de pagamento
- Incluindo uma função de administrador e um usuário para o aplicativo
- Restringindo o acesso a páginas e pastas específicas
- Carregando um arquivo para o aplicativo Web
- Implementando validação de entrada
- Registrando rotas para o aplicativo Web
- Implementando o tratamento de erros e o log de erros

## <a name="overview"></a>Visão geral

Esta série de tutoriais destina-se a alguém familiarizado com conceitos de programação, mas novidade no ASP.NET Web Forms. Se você já estiver familiarizado com o ASP.NET Web Forms, esta série ainda poderá ajudá-lo a aprender sobre os novos recursos do ASP.NET 4,5. Para leitores que não conhecem os conceitos de programação e Web Forms ASP.NET, consulte os tutoriais de Web Forms adicionais fornecidos na seção [introdução](../../../index.md) no site do ASP.net.

O ASP.NET 4,5 fornecido nesta série de tutoriais inclui os seguintes recursos:

- Uma interface do usuário simples para a criação de projetos que oferece [suporte para muitas estruturas ASP.net](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC e API da Web).
- [Inicialização](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), layout, temas e estrutura de design responsivo.
- [ASP.net Identity](../../../../identity/index.md), um novo sistema de associação ASP.NET que funciona da mesma em todas as estruturas do ASP.net e funciona com software de hospedagem na Web diferente do IIS.
- [Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx)

  Uma atualização do Entity Framework permitindo que você:
  - Recuperar e manipular dados como objetos fortemente tipados
  - Acessar dados de forma assíncrona
  - Tratar falhas de conexão transitórias
  - Instruções SQL de log

Para obter uma lista completa de recursos do ASP.NET 4,5, consulte [ASP.net and Web Tools para ver Visual Studio 2013 notas de versão](../../../../visual-studio/overview/2013/release-notes.md).

### <a name="the-wingtip-toys-sample-application"></a>O aplicativo de exemplo Wingtip Toys

As capturas de tela a seguir são do aplicativo ASP.NET Web Forms que você cria nesta série de tutoriais. Quando você executa o aplicativo no Visual Studio, a seguinte página inicial da Web é exibida.

![Wingtip Toys – página padrão](introduction-and-overview/_static/image1.png)

Você pode registrar-se como um novo usuário ou entrar como um usuário existente. A navegação superior tem links para categorias de produtos e seus produtos do banco de dados.

Se você selecionar **produtos**, todos os produtos disponíveis serão exibidos. 

![Wingtip Toys-produtos](introduction-and-overview/_static/image2.png)

Se você selecionar um produto específico, os detalhes do produto serão exibidos.

![Wingtip Toys-detalhes do produto](introduction-and-overview/_static/image3.png)

Como usuário, você pode registrar e entrar com Web Forms funcionalidade padrão do modelo. Este tutorial também explica como entrar usando uma conta do Gmail existente. Além disso, você pode entrar como administrador para adicionar e remover produtos do banco de dados.

![Wingtip Toys-entrar](introduction-and-overview/_static/image4.png)

Depois de entrar como um usuário, você pode adicionar produtos ao carrinho de compras e fazer checkout com o PayPal. O aplicativo de exemplo foi projetado para funcionar na área restrita do desenvolvedor do PayPal. Não ocorre nenhuma transação real do Money.

![Wingtip Toys – carrinho de compras](introduction-and-overview/_static/image5.png)

O PayPal confirma suas informações de conta, de pedido e de pagamento.

![Wingtip Toys-PayPal](introduction-and-overview/_static/image6.png)

Depois de retornar do PayPal, você pode revisar e concluir seu pedido.

![Wingtip Toys – revisão do pedido](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a>Prerequisites

Antes de começar, verifique se o seguinte software está instalado no computador:

- [Microsoft Visual Studio 2017 ou Microsoft Visual Studio Community 2017](https://visualstudio.microsoft.com/downloads/).

O .NET Framework é instalado automaticamente.

Esta série de tutoriais usa o Microsoft Visual Studio Community 2017. Você pode usar ou Microsoft Visual Studio 2017 para concluir esta série de tutoriais.

Observe o seguinte sobre o Visual Studio:

* Microsoft Visual Studio 2017 e Microsoft Visual Studio Community 2017 são chamados de *Visual Studio* durante esta série de tutoriais.

* O Visual Studio 2017 é instalado ao lado de todas as versões mais antigas já instaladas. Os sites criados em versões anteriores podem ser abertos no Visual Studio 2017 e continuam a ser abertos em versões anteriores.

* Na primeira vez que você iniciou o Visual Studio, supõe-se que você selecionou as configurações de *desenvolvimento da Web* . Para obter mais informações, consulte [como: selecionar configurações de ambiente de desenvolvimento da Web](https://msdn.microsoft.com/library/ff521558.aspx).

Depois de instalar os pré-requisitos, você estará pronto para começar a criar o projeto Web apresentado nesta série de tutoriais.

## <a name="download-the-sample-application"></a>Baixar o aplicativo de exemplo

 Você pode baixar o aplicativo de exemplo completo a qualquer momento no site de exemplos do MSDN:

[Introdução com ASP.NET 4,5 Web Forms e Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) 

 Este download tem os seguintes itens:

- O aplicativo de exemplo na pasta *WingtipToys*
- Os recursos usados para criar o aplicativo de exemplo na pasta *WingtipToys-assets* na pasta *WingtipToys*

O download é um arquivo *. zip* . Para ver o projeto concluído que essa série de tutoriais cria, localize e selecione *C#* a pasta no arquivo. zip. Salve a C# pasta na pasta que você usa para trabalhar com projetos do Visual Studio. Por padrão, a pasta Projects do Visual Studio 2017 é:

<strong>C:\Users&#92;</strong>  <strong><em>&lt;nome de usuário&gt;</em></strong> <strong>\source\repos</strong>

Renomeie a ***C#*** pasta para ***WingtipToys***.

> [!NOTE]
> Se você já tiver uma pasta chamada *WingtipToys* na pasta Projects, renomeie temporariamente essa pasta existente antes de *C#* renomear a pasta como *WingtipToys*.

Para executar o projeto concluído, abra a pasta *WingtipToys* e clique duas vezes no arquivo *WingtipToys. sln* . O Visual Studio 2017 abre o projeto. Em seguida, clique com o botão direito do mouse no arquivo *Default. aspx* em **Gerenciador de soluções** e selecione **Exibir no navegador**.

## <a name="take-a-aspnet-web-forms-quiz-to-review-content"></a>Faça um teste de Web Forms de ASP.NET para examinar o conteúdo

Depois de concluir a série de tutoriais, faça um teste para testar seu conhecimento e reforçar os principais conceitos. Cada pergunta fornece uma explicação e links para diretrizes adicionais.

* [Teste de Web Forms de ASP.NET](https://blogs.msdn.microsoft.com/erikreitan/2016/01/08/asp-net-web-forms-quiz/) 

## <a name="tutorial-support-and-comments"></a>Suporte e comentários do tutorial

Para dúvidas e comentários, use a seção Q e a incluída no [introdução com o ASP.NET 4,5 Web Forms e](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) a página de exemplo Visual Studio 2013C#da Wingtip Toys ().

Comentários sobre esta série de tutoriais são bem-vindos. Quando essa série de tutoriais é atualizada, cada esforço é feito para considerar correções ou sugestões de melhorias.

Se ocorrer um erro, as mensagens de erro correspondentes poderão ser confusas, sem uma boa explicação sobre como corrigi-lo. Para obter ajuda, você pode verificar os [fóruns do ASP.net](https://forums.asp.net/). Outra boa fonte é a Q e uma seção na [introdução com ASP.NET 4,5 Web Forms e](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) a página de exemplo Visual Studio 2013 daC#Wingtip Toys (). 

> [!div class="step-by-step"]
> [Próximo](create-the-project.md)
