---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: 'Parte 2: camada de acesso a dados | Microsoft Docs'
author: JoeStagner
description: Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo Tailspin Spyworks. A parte 2 aborda a adição da camada de acesso a dados.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: 342d2c54dfba5d052570e890f85dcf9739f9884f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78573244"
---
# <a name="part-2-data-access-layer"></a><span data-ttu-id="cc907-104">Parte 2: camada de acesso a dados</span><span class="sxs-lookup"><span data-stu-id="cc907-104">Part 2: Data Access Layer</span></span>

<span data-ttu-id="cc907-105">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="cc907-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="cc907-106">A Tailspin Spyworks demonstra como é extremamente simples criar aplicativos poderosos e escalonáveis para a plataforma .NET.</span><span class="sxs-lookup"><span data-stu-id="cc907-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="cc907-107">Ele mostra como usar os ótimos novos recursos do ASP.NET 4 para criar uma loja online, incluindo compras, check-out e administração.</span><span class="sxs-lookup"><span data-stu-id="cc907-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="cc907-108">Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="cc907-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="cc907-109">A parte 2 aborda a adição da camada de acesso a dados.</span><span class="sxs-lookup"><span data-stu-id="cc907-109">Part 2 covers adding the data access layer.</span></span>

## <a id="_Toc260221668"></a><span data-ttu-id="cc907-110">Adicionando a camada de acesso a dados</span><span class="sxs-lookup"><span data-stu-id="cc907-110">Adding the Data Access Layer</span></span>

<span data-ttu-id="cc907-111">Nosso aplicativo de comércio eletrônico dependerá de dois bancos de dados.</span><span class="sxs-lookup"><span data-stu-id="cc907-111">Our ecommerce application will depend on two databases.</span></span>

<span data-ttu-id="cc907-112">Para obter informações do cliente, usaremos o banco de dados de associação do ASP.NET padrão.</span><span class="sxs-lookup"><span data-stu-id="cc907-112">For customer information we'll use the standard ASP.NET Membership database.</span></span> <span data-ttu-id="cc907-113">Para nosso carrinho de compras e catálogo de produtos, implementaremos um banco de dados do SQL Express da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="cc907-113">For our shopping cart and product catalog we'll implement a SQL Express database as follows.</span></span>

![](tailspin-spyworks-part-2/_static/image1.jpg)

<span data-ttu-id="cc907-114">Depois de criar o banco de dados (Commerce. MDF) na pasta do aplicativo\_data, podemos continuar criando nossa camada de acesso a dados usando o Entity Framework .NET.</span><span class="sxs-lookup"><span data-stu-id="cc907-114">Having created the database (Commerce.mdf) in the application's App\_Data folder we can proceed to create our Data Access Layer using the .NET Entity Framework.</span></span>

<span data-ttu-id="cc907-115">Criaremos uma pasta denominada "data\_Access" e as clicaremos com o botão direito do mouse nessa pasta e selecionaremos "Adicionar novo item".</span><span class="sxs-lookup"><span data-stu-id="cc907-115">We'll create a folder named "Data\_Access" and them right click on that folder and select "Add New Item".</span></span>

<span data-ttu-id="cc907-116">No item "modelos instalados" e, em seguida, selecione "ADO.NET Modelo de Dados de Entidade" Insira EDM\_Commerce. edmx como o nome e clique no botão "Adicionar".</span><span class="sxs-lookup"><span data-stu-id="cc907-116">In the "Installed Templates" item and then select "ADO.NET Entity Data Model" enter EDM\_Commerce.edmx as the name and click the "Add" button.</span></span>

![](tailspin-spyworks-part-2/_static/image2.jpg)

<span data-ttu-id="cc907-117">Escolha "gerar do banco de dados".</span><span class="sxs-lookup"><span data-stu-id="cc907-117">Choose "Generate from Database".</span></span>

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

<span data-ttu-id="cc907-118">Salve e compile.</span><span class="sxs-lookup"><span data-stu-id="cc907-118">Save and build.</span></span>

<span data-ttu-id="cc907-119">Agora estamos prontos para adicionar nosso primeiro recurso – um menu de categorias de produtos.</span><span class="sxs-lookup"><span data-stu-id="cc907-119">Now we are ready to add our first feature – a product category menu.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="cc907-120">[Anterior](tailspin-spyworks-part-1.md)
> [Próximo](tailspin-spyworks-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="cc907-120">[Previous](tailspin-spyworks-part-1.md)
[Next](tailspin-spyworks-part-3.md)</span></span>
