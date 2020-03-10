---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: Criar um novo projeto MVC do ASP.NET | Microsoft Docs
author: microsoft
description: A etapa 1 mostra como colocar a estrutura do aplicativo NerdDinner básica em vigor.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: 189ddc187fc83db14106b2da199ba12a70a32b45
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78580930"
---
# <a name="create-a-new-aspnet-mvc-project"></a>Criar um novo projeto do ASP.NET MVC

pela [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Esta é a etapa 1 de um [tutorial de aplicativo "NerdDinner"](introducing-the-nerddinner-tutorial.md) gratuito que percorre como criar um aplicativo Web pequeno, mas completo usando o ASP.NET MVC 1.
> 
> A etapa 1 mostra como colocar a estrutura do aplicativo NerdDinner básica em vigor.
> 
> Se você estiver usando o ASP.NET MVC 3, recomendamos seguir as [introdução com](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) os tutoriais da [loja de música](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) MVC 3 ou MVC.

## <a name="nerddinner-step-1-file-gtnew-project"></a>NerdDinner etapa 1: arquivo-&gt;novo projeto

Vamos começar nosso aplicativo NerdDinner selecionando o item de menu **arquivo-&gt;novo projeto** no visual Studio 2008 ou o Visual Web developer 2008 Express gratuito.

Isso abrirá a caixa de diálogo "novo projeto". Para criar um novo aplicativo MVC do ASP.NET, selecionaremos o nó "Web" no lado esquerdo da caixa de diálogo e, em seguida, escolheremos o modelo de projeto "aplicativo Web do ASP.NET MVC" à direita:

![](create-a-new-aspnet-mvc-project/_static/image1.png)

*Importante: Verifique se você baixou e instalou o ASP.NET MVC-caso contrário, ele não aparecerá na caixa de diálogo novo projeto. Você pode usar a v2 do [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) se ainda não o tiver instalado (o ASP.NET MVC está disponível na seção "plataforma Web-&gt;estruturas e tempos de execução").*

Vamos nomear o novo projeto de que vamos criar "NerdDinner" e clicar no botão "OK" para criá-lo.

Quando clicamos em "OK", o Visual Studio abrirá uma caixa de diálogo adicional que nos solicitará também criar um projeto de teste de unidade para o novo aplicativo. Este projeto de teste de unidade nos permite criar testes automatizados que verificam a funcionalidade e o comportamento do nosso aplicativo (algo que abordaremos como fazer mais tarde neste tutorial).

![](create-a-new-aspnet-mvc-project/_static/image2.png)

O menu suspenso "estrutura de teste" na caixa de diálogo acima é populado com todos os modelos de projeto de teste de unidade MVC ASP.NET disponíveis instalados no computador. As versões podem ser baixadas para NUnit, MBUnit e XUnit. Também há suporte para a estrutura de teste de unidade do Visual Studio interna.

*Observação: a estrutura de teste de unidade do Visual Studio só está disponível com o Visual Studio 2008 Professional e versões posteriores. Se estiver usando o VS 2008 Standard Edition ou o Visual Web Developer 2008 Express, você precisará baixar e instalar as extensões NUnit, MBUnit ou XUnit para o ASP.NET MVC para que essa caixa de diálogo seja mostrada. A caixa de diálogo não será exibida se não houver nenhuma estrutura de teste instalada.*

Usaremos o nome padrão "NerdDinner. Tests" para o projeto de teste que criamos e usamos a opção de estrutura "teste de unidade do Visual Studio". Quando clicamos no botão "OK", o Visual Studio criará uma solução para nós com dois projetos em ti, um para nosso aplicativo Web e outro para nossos testes de unidade:

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a>Examinando a estrutura do diretório NerdDinner

Quando você cria um novo aplicativo MVC ASP.NET com o Visual Studio, ele adiciona automaticamente vários arquivos e diretórios ao projeto:

![](create-a-new-aspnet-mvc-project/_static/image4.png)

Por padrão, os projetos do ASP.NET MVC têm seis diretórios de nível superior:

| **Diretório** | **Finalidade** |
| --- | --- |
| **/Controllers** | Onde você coloca classes de controlador que manipulam solicitações de URL |
| **/Models** | Onde você coloca classes que representam e manipulam dados |
| **/Views** | Onde você coloca os arquivos de modelo da interface do usuário que são responsáveis por processar a saída |
| **/Scripts** | Onde você coloca os arquivos e scripts da biblioteca JavaScript (. js) |
| **/Content** | Onde você coloca arquivos de CSS e de imagem e outros conteúdos não dinâmicos/não-JavaScript |
| **/App dados de\_** | Onde você armazena os arquivos de dados que deseja ler/gravar. |

O ASP.NET MVC não requer essa estrutura. Na verdade, os desenvolvedores que trabalham em aplicativos grandes normalmente particionam o aplicativo em vários projetos para torná-lo mais gerenciável (por exemplo: classes de modelo de dados geralmente vão em um projeto de biblioteca de classes separado do aplicativo Web). A estrutura de projeto padrão, no entanto, fornece uma boa Convenção de diretório padrão que podemos usar para manter as preocupações de nosso aplicativo limpas.

Quando expandirmos o diretório/Controllers, descobriremos que o Visual Studio adicionou duas classes Controller – HomeController e AccountController – por padrão ao projeto:

![](create-a-new-aspnet-mvc-project/_static/image5.png)

Quando expandirmos o diretório/views, localizaremos três subdiretórios –/Home,/Account e/Shared – assim como vários arquivos de modelo dentro deles também foram adicionados ao projeto por padrão:

![](create-a-new-aspnet-mvc-project/_static/image6.png)

Quando expandirmos os diretórios/Content e/scripts, encontrarei um arquivo site. CSS que é usado para estilizar todo o HTML no site, bem como bibliotecas JavaScript que podem habilitar o suporte a AJAX e jQuery no aplicativo:

![](create-a-new-aspnet-mvc-project/_static/image7.png)

Quando expandirmos o projeto NerdDinner. Tests, localizaremos duas classes que contêm testes de unidade para nossas classes de controlador:

![](create-a-new-aspnet-mvc-project/_static/image8.png)

Esses arquivos padrão adicionados pelo Visual Studio nos fornecem uma estrutura básica para um aplicativo funcional-completo com home page, páginas sobre página, logon de conta/logout/registro e uma página de erro não tratada (tudo conectado e pronto para uso).

### <a name="running-the-nerddinner-application"></a>Executando o aplicativo NerdDinner

Podemos executar o projeto escolhendo **debug-&gt;iniciar depuração** ou **debug-&gt;iniciar sem Depurar itens de** menu:

![](create-a-new-aspnet-mvc-project/_static/image9.png)

Isso iniciará o servidor Web ASP.NET interno que vem com o Visual Studio e executará nosso aplicativo:

![](create-a-new-aspnet-mvc-project/_static/image10.png)

Abaixo está o home page para nosso novo projeto (URL: "/") quando ele é executado:

![](create-a-new-aspnet-mvc-project/_static/image11.png)

Clicar na guia "sobre" exibe uma página About (URL: "/Home/About"):

![](create-a-new-aspnet-mvc-project/_static/image12.png)

Clicar no link "logon" no canto superior direito nos leva a uma página de logon (URL: "/Account/LogOn")

![](create-a-new-aspnet-mvc-project/_static/image13.png)

Se não tivermos uma conta de logon, poderemos clicar no link de registro (URL: "/Account/Register") para criar uma:

![](create-a-new-aspnet-mvc-project/_static/image14.png)

O código para implementar a funcionalidade de início, sobre e logout/registro acima foi adicionado por padrão quando criamos nosso novo projeto. Vamos usá-lo como o ponto de partida de nosso aplicativo.

### <a name="testing-the-nerddinner-application"></a>Testando o aplicativo NerdDinner

Se estivermos usando a Professional Edition ou uma versão superior do Visual Studio 2008, podemos usar o suporte de IDE de teste de unidade interno no Visual Studio para testar o projeto:

![](create-a-new-aspnet-mvc-project/_static/image15.png)

A escolha de uma das opções acima abrirá o painel "Resultados de Teste" no IDE e nos fornecerá o status de aprovação/reprovação nos testes de unidade 27 incluídos em nosso novo projeto que abrange a funcionalidade interna:

![](create-a-new-aspnet-mvc-project/_static/image16.png)

Mais adiante neste tutorial, falaremos mais sobre os testes automatizados e adicionaremos outros testes de unidade que abrangem a funcionalidade do aplicativo que implementamos.

### <a name="next-step"></a>Próxima etapa

Agora temos uma estrutura de aplicativo básica em vigor. Agora, vamos [criar um banco de dados para armazenar os aplicativos de aplicativo](create-a-database.md).

> [!div class="step-by-step"]
> [Anterior](introducing-the-nerddinner-tutorial.md)
> [Próximo](create-a-database.md)
