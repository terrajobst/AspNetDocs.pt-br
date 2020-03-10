---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: 'Parte 3: menu de layout e categoria | Microsoft Docs'
author: JoeStagner
description: Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo Tailspin Spyworks. A parte 3 aborda a adição de layout e um menu de categoria.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: a223b97fd362ecf73ecde431e141021c1dcc6a6d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78639114"
---
# <a name="part-3-layout-and-category-menu"></a><span data-ttu-id="9c19a-104">Parte 3: menu de layout e categoria</span><span class="sxs-lookup"><span data-stu-id="9c19a-104">Part 3: Layout and Category Menu</span></span>

<span data-ttu-id="9c19a-105">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="9c19a-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="9c19a-106">A Tailspin Spyworks demonstra como é extremamente simples criar aplicativos poderosos e escalonáveis para a plataforma .NET.</span><span class="sxs-lookup"><span data-stu-id="9c19a-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="9c19a-107">Ele mostra como usar os ótimos novos recursos do ASP.NET 4 para criar uma loja online, incluindo compras, check-out e administração.</span><span class="sxs-lookup"><span data-stu-id="9c19a-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="9c19a-108">Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="9c19a-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="9c19a-109">A parte 3 aborda a adição de layout e um menu de categoria.</span><span class="sxs-lookup"><span data-stu-id="9c19a-109">Part 3 covers adding layout and a category menu.</span></span>

## <a id="_Toc260221669"></a><span data-ttu-id="9c19a-110">Adicionando algum layout e um menu de categoria</span><span class="sxs-lookup"><span data-stu-id="9c19a-110">Adding Some Layout and a Category Menu</span></span>

<span data-ttu-id="9c19a-111">Em nossa página mestra do site, adicionaremos uma div à coluna do lado esquerdo que conterá nosso menu de categoria do produto.</span><span class="sxs-lookup"><span data-stu-id="9c19a-111">In our site master page we'll add a div for the left side column that will contain our product category menu.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

<span data-ttu-id="9c19a-112">Observe que o alinhamento desejado e outras formatações serão fornecidas pela classe CSS que adicionamos ao nosso arquivo Style. css.</span><span class="sxs-lookup"><span data-stu-id="9c19a-112">Note that the desired aligning and other formatting will be provided by the CSS class that we added to our Style.css file.</span></span>

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

<span data-ttu-id="9c19a-113">O menu categoria do produto será criado dinamicamente em tempo de execução consultando o banco de dados de comércio para obter categorias de produtos existentes e criando os itens de menu e links correspondentes.</span><span class="sxs-lookup"><span data-stu-id="9c19a-113">The product category menu will be dynamically created at runtime by querying the Commerce database for existing product categories and creating the menu items and corresponding links.</span></span>

<span data-ttu-id="9c19a-114">Para fazer isso, usaremos dois do ASP. Os poderosos controles de dados do NET.</span><span class="sxs-lookup"><span data-stu-id="9c19a-114">To accomplish this we will use two of ASP.NET's powerful data controls.</span></span> <span data-ttu-id="9c19a-115">O controle "fonte de dados de entidade" e o controle "ListView".</span><span class="sxs-lookup"><span data-stu-id="9c19a-115">The "Entity Data Source" control and the "ListView" control.</span></span>

![](tailspin-spyworks-part-3/_static/image1.jpg)

<span data-ttu-id="9c19a-116">Vamos mudar para "modo de exibição de design" e usar os auxiliares para configurar nossos controles.</span><span class="sxs-lookup"><span data-stu-id="9c19a-116">Let's switch to "Design View" and use the helpers to configure our controls.</span></span>

![](tailspin-spyworks-part-3/_static/image2.jpg)

<span data-ttu-id="9c19a-117">Vamos definir a propriedade de ID de EntityDataSource para o menu de\_de categoria da EDS\_e clicar em "Configurar fonte de dados".</span><span class="sxs-lookup"><span data-stu-id="9c19a-117">Let's set the EntityDataSource ID property to EDS\_Category\_Menu and click on "Configure Data Source".</span></span>

![](tailspin-spyworks-part-3/_static/image3.jpg)

<span data-ttu-id="9c19a-118">Selecione a conexão CommerceEntities que foi criada para nós quando criamos o modelo de fonte de dados de entidade para nosso banco de dado de comércio e clique em "Avançar".</span><span class="sxs-lookup"><span data-stu-id="9c19a-118">Select the CommerceEntities Connection that was created for us when we created the Entity Data Source Model for our Commerce Database and click "Next".</span></span>

![](tailspin-spyworks-part-3/_static/image4.jpg)

<span data-ttu-id="9c19a-119">Selecione o nome do conjunto de entidades "categorias" e deixe o restante das opções como padrão.</span><span class="sxs-lookup"><span data-stu-id="9c19a-119">Select the "Categories" Entity set name and leave the rest of the options as default.</span></span> <span data-ttu-id="9c19a-120">Clique em "Concluir".</span><span class="sxs-lookup"><span data-stu-id="9c19a-120">Click "Finish".</span></span>

<span data-ttu-id="9c19a-121">Agora, vamos definir a propriedade ID da instância de controle ListView que colocamos em nossa página para ListView\_ProductsMenu e ativar seu auxiliar.</span><span class="sxs-lookup"><span data-stu-id="9c19a-121">Now let's set the ID property of the ListView control instance that we placed on our page to ListView\_ProductsMenu and activate its helper.</span></span>

![](tailspin-spyworks-part-3/_static/image5.jpg)

<span data-ttu-id="9c19a-122">Embora possamos usar as opções de controle para formatar a exibição e a formatação do item de dados, nossa criação de menu exigirá apenas marcação simples, portanto, entraremos no código na exibição da fonte.</span><span class="sxs-lookup"><span data-stu-id="9c19a-122">Though we could use control options to format the data item display and formatting, our menu creation will only require simple markup so we will enter the code in the source view.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

<span data-ttu-id="9c19a-123">Observe a instrução "eval": &lt;% # Eval ("CategoryName")%&gt;</span><span class="sxs-lookup"><span data-stu-id="9c19a-123">Please note the "Eval" statement : &lt;%# Eval("CategoryName") %&gt;</span></span>

<span data-ttu-id="9c19a-124">A sintaxe ASP.NET &lt;% #%&gt; é uma convenção abreviada que instrui o tempo de execução para executar o que estiver contido em e gerar os resultados "in line".</span><span class="sxs-lookup"><span data-stu-id="9c19a-124">The ASP.NET syntax &lt;%# %&gt; is a shorthand convention that instructs the runtime to execute whatever is contained within and output the results "in Line".</span></span>

<span data-ttu-id="9c19a-125">A instrução Eval ("CategoryName") instrui isso, para a entrada atual na coleção associada de itens de dados, busque o valor dos nomes de item de modelo de entidade "CategoryName".</span><span class="sxs-lookup"><span data-stu-id="9c19a-125">The statement Eval("CategoryName") instructs that, for the current entry in the bound collection of data items, fetch the value of the Entity Model item names "CategoryName".</span></span> <span data-ttu-id="9c19a-126">Essa é uma sintaxe concisa para um recurso muito potente.</span><span class="sxs-lookup"><span data-stu-id="9c19a-126">This is concise syntax for a very powerful feature.</span></span>

<span data-ttu-id="9c19a-127">Vamos executar o aplicativo agora.</span><span class="sxs-lookup"><span data-stu-id="9c19a-127">Lets run the application now.</span></span>

![](tailspin-spyworks-part-3/_static/image6.jpg)

<span data-ttu-id="9c19a-128">Observe que nosso menu de categoria de produto agora é exibido e, quando passamos o mouse sobre um dos itens de menu de categoria, podemos ver os pontos de link do item de menu para uma página que ainda implementamos ProductList. aspx e criamos um argumento de cadeia de caracteres de consulta dinâmica que contém o  ID da categoria.</span><span class="sxs-lookup"><span data-stu-id="9c19a-128">Note that our product category menu is now displayed and when we hover over one of the category menu items we can see the menu item link points to a page we have yet to implement named ProductsList.aspx and that we have built a dynamic query string argument that contains the category id.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9c19a-129">[Anterior](tailspin-spyworks-part-2.md)
> [Próximo](tailspin-spyworks-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="9c19a-129">[Previous](tailspin-spyworks-part-2.md)
[Next](tailspin-spyworks-part-4.md)</span></span>
