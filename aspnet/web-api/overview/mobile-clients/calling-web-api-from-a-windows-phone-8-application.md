---
uid: web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
title: Chamando a API Web de um aplicativo Windows Phone 8C#()-ASP.NET 4. x
author: rmcmurray
description: 'Tutorial com código: Crie um aplicativo ASP.NET Web API no ASP.NET 4. x que fornece um catálogo de livros para um aplicativo Windows Phone 8.'
ms.author: riande
ms.date: 10/09/2013
ms.custom: seoapril2019
ms.assetid: b9775f41-352a-4f82-baa6-23e95b342e20
msc.legacyurl: /web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
msc.type: authoredcontent
ms.openlocfilehash: c5da14a6856f551343b6fb14f0aedc659e792f6b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614733"
---
# <a name="calling-web-api-from-a-windows-phone-8-application-c"></a>Chamar a API Web em um aplicativo do Windows Phone 8 (C#)

por [Robert McMurray](https://github.com/rmcmurray)

Neste tutorial, você aprenderá a criar um cenário completo de ponta a ponta que consiste em um aplicativo ASP.NET Web API que fornece um catálogo de livros para um aplicativo Windows Phone 8.

### <a name="overview"></a>Visão geral

Os serviços RESTful, como ASP.NET Web API simplificam a criação de aplicativos baseados em HTTP para desenvolvedores, abstraindo a arquitetura para aplicativos do lado do servidor e do cliente. Em vez de criar um protocolo baseado em soquete proprietário para comunicação, os desenvolvedores de API da Web simplesmente precisam publicar os métodos HTTP necessários para seu aplicativo, (por exemplo: GET, POST, PUT, DELETE) e os desenvolvedores de aplicativos cliente só precisam consumir os métodos HTTP que são necessários para seu aplicativo.

Neste tutorial de ponta a ponta, você aprenderá a usar a API Web para criar os seguintes projetos:

- Na [primeira parte deste tutorial](#STEP1), você criará um aplicativo ASP.NET Web API que dá suporte a todas as operações CRUD (criar, ler, atualizar e excluir) para gerenciar um catálogo de livros. Este aplicativo usará o [arquivo XML de exemplo (Books. xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) do MSDN.
- Na [segunda parte deste tutorial](#STEP2), você criará um aplicativo interativo Windows Phone 8 que recupera os dados do seu aplicativo de API Web.

#### <a name="prerequisites"></a>Prerequisites

- Visual Studio 2013 com o SDK do Windows Phone 8 instalado
- Windows 8 ou posterior em um sistema de 64 bits com o Hyper-V instalado
- Para obter uma lista de requisitos adicionais, consulte a seção *requisitos do sistema* na página de download do [Windows Phone SDK 8,0](https://www.microsoft.com/download/details.aspx?id=35471) .

> [!NOTE]
> Se você pretende testar a conectividade entre os projetos da API Web e do Windows Phone 8 no sistema local, será necessário seguir as instruções no artigo *[conectando o emulador Windows Phone 8 ao aplicativo da API Web em um computador local](https://go.microsoft.com/fwlink/?LinkId=324014)* para configurar seu ambiente de teste.

<a id="STEP1"></a>
### <a name="step-1-creating-the-web-api-bookstore-project"></a>Etapa 1: criando o projeto de livrar-API Web

A primeira etapa deste tutorial de ponta a ponta é criar um projeto de API Web que dê suporte a todas as operações CRUD; Observe que você adicionará o projeto de aplicativo Windows Phone a essa solução na [etapa 2](#STEP2) deste tutorial.

1. Abra **Visual Studio 2013**.
2. Clique em **arquivo**, em **novo**e em **projeto**.
3. Quando a caixa de diálogo **novo projeto** for exibida, expanda **instalado**, depois **modelos**, **Visual C#** e, em seguida, **Web**.

   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image2.png)](calling-web-api-from-a-windows-phone-8-application/_static/image1.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                Clique na imagem para expandir                                                                |

4. Realce **aplicativo Web ASP.net**, digite **bookstore** para o nome do projeto e clique em **OK**.
5. Quando a caixa de diálogo **novo projeto ASP.net** for exibida, selecione o modelo **API Web** e clique em **OK**.

   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image4.png)](calling-web-api-from-a-windows-phone-8-application/_static/image3.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                Clique na imagem para expandir                                                                |

6. Quando o projeto de API Web for aberto, remova o controlador de exemplo do projeto:

    1. Expanda a pasta **controladores** no Gerenciador de soluções.
    2. Clique com o botão direito do mouse no arquivo **ValuesController.cs** e clique em **excluir**.
    3. Clique em **OK** quando for solicitado a confirmar a exclusão.
7. Adicionar um arquivo de dados XML ao projeto da API Web; Esse arquivo contém o conteúdo do catálogo da livraria:

   1. Clique com o botão direito do mouse na pasta **\_dados do aplicativo** no Gerenciador de soluções, clique em **Adicionar**e em **novo item**.
   2. Quando a caixa de diálogo **Adicionar novo item** for exibida, realce o modelo de **arquivo XML** .
   3. Nomeie o arquivo **Books. xml**e clique em **Adicionar**.
   4. Quando o arquivo **Books. xml** for aberto, substitua o código no arquivo pelo XML do arquivo **Books. xml** de exemplo no MSDN: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample1.xml)]
   5. Salve e feche o arquivo XML.

8. Adicione o modelo de livrarte ao projeto de API Web; Esse modelo contém a lógica CRUD (criar, ler, atualizar e excluir) para o aplicativo de livraria:

   1. Clique com o botão direito do mouse na pasta **modelos** no Gerenciador de soluções, clique em **Adicionar**e em **classe**.
   2. Quando a caixa de diálogo **Adicionar novo item** for exibida, nomeie o arquivo de classe **BookDetails.cs**e clique em **Adicionar**.
   3. Quando o arquivo **BookDetails.cs** for aberto, substitua o código no arquivo pelo seguinte: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample2.cs)]
   4. Salve e feche o arquivo **BookDetails.cs** .

9. Adicione o controlador da livraria ao projeto da API Web:

   1. Clique com o botão direito do mouse na pasta **controladores** no Gerenciador de soluções, clique em **Adicionar**e em **controlador**.
   2. Quando a caixa de diálogo **Adicionar Scaffold** for exibida, realce **controlador da API Web 2 – vazio**e clique em **Adicionar**.
   3. Quando a caixa de diálogo **Adicionar controlador** for exibida, nomeie o controlador **BooksController**e clique em **Adicionar**.
   4. Quando o arquivo **BooksController.cs** for aberto, substitua o código no arquivo pelo seguinte: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample3.cs)]
   5. Salve e feche o arquivo **BooksController.cs** .

10. Crie o aplicativo de API Web para verificar se há erros.

<a id="STEP2"></a>
### <a name="step-2-adding-the-windows-phone-8-bookstore-catalog-project"></a>Etapa 2: adicionando o projeto de catálogo da livraria do Windows Phone 8

A próxima etapa desse cenário de ponta a ponta é criar o aplicativo de catálogo para o Windows Phone 8. Este aplicativo usará o *Windows Phone modelo de aplicativo vinculado* para a interface do usuário padrão e usará o aplicativo de API Web que você criou na [etapa 1](#STEP1) deste tutorial como a fonte de dados.

1. Clique com o botão direito do mouse na solução de **livraria** no Gerenciador de soluções, clique em **Adicionar**e em **novo projeto**.
2. Quando a caixa de diálogo **novo projeto** for exibida, expanda **instalado**, **Visual C#** e, em seguida, **Windows Phone**.
3. Realce **Windows Phone aplicativo vinculado**, insira **BookCatalog** para o nome e clique em **OK**.
4. Adicione o pacote NuGet Json.NET ao projeto **BookCatalog** :

    1. Clique com o botão direito do mouse em **referências** para o projeto **BookCatalog** no Gerenciador de soluções e clique em **gerenciar pacotes NuGet**.
    2. Quando a caixa de diálogo **gerenciar pacotes NuGet** for exibida, expanda a seção **online** e realce **NuGet.org**.
    3. Insira **JSON.net** no campo de pesquisa e clique no ícone de pesquisa.
    4. Realce **JSON.net** nos resultados da pesquisa e clique em **instalar**.
    5. Quando a instalação for concluída, clique em **fechar**.
5. Adicione o modelo **BookDetails** ao projeto **BookCatalog** ; contém um modelo genérico da classe bookstore:

   1. Clique com o botão direito do mouse no projeto **BookCatalog** no Gerenciador de soluções, clique em **Adicionar**e clique em **nova pasta**.
   2. Nomeie os novos **modelos**de pasta.
   3. Clique com o botão direito do mouse na pasta **modelos** no Gerenciador de soluções, clique em **Adicionar**e em **classe**.
   4. Quando a caixa de diálogo **Adicionar novo item** for exibida, nomeie o arquivo de classe **BookDetails.cs**e clique em **Adicionar**.
   5. Quando o arquivo **BookDetails.cs** for aberto, substitua o código no arquivo pelo seguinte: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample4.cs)]
   6. Salve e feche o arquivo **BookDetails.cs** .

6. Atualize a classe **MainViewModel.cs** para incluir a funcionalidade para se comunicar com o aplicativo de API Web da livraria:

   1. Expanda a pasta **ViewModels** no Gerenciador de soluções e clique duas vezes no arquivo **MainViewModel.cs** .
   2. Quando o arquivo **MainViewModel.cs** for aberto, substitua o código no arquivo pelo seguinte; Observe que você precisará atualizar o valor da constante `apiUrl` com a URL real de sua API Web: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample5.cs)]
   3. Salve e feche o arquivo **MainViewModel.cs** .

7. Atualize o arquivo **MainPage. XAML** para personalizar o nome do aplicativo:

   1. Clique duas vezes no arquivo **MainPage. XAML** no Gerenciador de soluções.
   2. Quando o arquivo **MainPage. XAML** for aberto, localize as seguintes linhas de código: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample6.xml)]
   3. Substitua essas linhas pelo seguinte: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample7.xml)]
   4. Salve e feche o arquivo **MainPage. XAML** .

8. Atualize o arquivo **DetailsPage. XAML** para personalizar os itens exibidos:

   1. Clique duas vezes no arquivo **DetailsPage. XAML** no Gerenciador de soluções.
   2. Quando o arquivo **DetailsPage. XAML** for aberto, localize as seguintes linhas de código: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample8.xml)]
   3. Substitua essas linhas pelo seguinte: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample9.xml)]
   4. Salve e feche o arquivo **DetailsPage. XAML** .

9. Compile o aplicativo Windows Phone para verificar se há erros.

### <a name="step-3-testing-the-end-to-end-solution"></a>Etapa 3: testando a solução de ponta a ponta

Conforme mencionado na seção *pré-requisitos* deste tutorial, quando você estiver testando a conectividade entre os projetos da API Web e do Windows Phone 8 no sistema local, será necessário seguir as instruções no artigo *[conectando o emulador do Windows Phone 8 ao aplicativo da API Web em um computador local](https://go.microsoft.com/fwlink/?LinkId=324014)* para configurar o ambiente de teste.

Depois de configurar o ambiente de teste, você precisará definir o aplicativo Windows Phone como o projeto de inicialização. Para fazer isso, realce o aplicativo **BookCatalog** no Gerenciador de soluções e, em seguida, clique em **definir como projeto de inicialização**:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image6.png)](calling-web-api-from-a-windows-phone-8-application/_static/image5.png) |
| --- |
| Clique na imagem para expandir |

Quando você pressionar F5, o Visual Studio iniciará o emulador Windows Phone, que exibirá um &quot;aguardará&quot; mensagem enquanto os dados do aplicativo são recuperados da API Web:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image8.png)](calling-web-api-from-a-windows-phone-8-application/_static/image7.png) |
| --- |
| Clique na imagem para expandir |

Se tudo for bem-sucedido, você verá o catálogo exibido:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image10.png)](calling-web-api-from-a-windows-phone-8-application/_static/image9.png) |
| --- |
| Clique na imagem para expandir |

Se você tocar em qualquer título de livro, o aplicativo exibirá a descrição do livro:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image12.png)](calling-web-api-from-a-windows-phone-8-application/_static/image11.png) |
| --- |
| Clique na imagem para expandir |

Se o aplicativo não puder se comunicar com sua API Web, uma mensagem de erro será exibida:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image14.png)](calling-web-api-from-a-windows-phone-8-application/_static/image13.png) |
| --- |
| Clique na imagem para expandir |

Se você tocar na mensagem de erro, todos os detalhes adicionais sobre o erro serão exibidos:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image16.png)](calling-web-api-from-a-windows-phone-8-application/_static/image15.png) |
|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                                 Clique na imagem para expandir                                                                 |
