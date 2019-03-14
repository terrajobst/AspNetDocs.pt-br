---
title: Bibliotecas de classes de componentes do Razor
author: guardrex
description: Descubra como os componentes podem ser incluídos em Razor componentes aplicativos de uma biblioteca de componentes externos.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/09/2019
uid: razor-components/class-libraries
ms.openlocfilehash: 0e644627178bae2b8880760335860b3e0ebef156
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57037403"
---
# <a name="razor-components-class-libraries"></a><span data-ttu-id="a3362-103">Bibliotecas de classes de componentes do Razor</span><span class="sxs-lookup"><span data-stu-id="a3362-103">Razor Components Class Libraries</span></span>

<span data-ttu-id="a3362-104">Por [Simon Timms](https://github.com/stimms)</span><span class="sxs-lookup"><span data-stu-id="a3362-104">By [Simon Timms](https://github.com/stimms)</span></span>

> [!NOTE]
> <span data-ttu-id="a3362-105">SDK do .NET Core 3.0 versão prévia 2 não inclui um modelo de projeto para bibliotecas de classes de componente do Razor, mas podemos esperar adicionar um modelo em uma visualização futura.</span><span class="sxs-lookup"><span data-stu-id="a3362-105">The .NET Core 3.0 Preview 2 SDK doesn't include a project template for Razor Component Class Libraries, but we expect to add a template in a future preview.</span></span> <span data-ttu-id="a3362-106">Em enquanto isso, você pode usar o modelo de biblioteca de classes de componente Blazor explicado neste tópico.</span><span class="sxs-lookup"><span data-stu-id="a3362-106">In meantime, you can use the Blazor Component Class Library template explained in this topic.</span></span>

<span data-ttu-id="a3362-107">Componentes podem ser compartilhados em bibliotecas de componentes entre projetos.</span><span class="sxs-lookup"><span data-stu-id="a3362-107">Components can be shared in component libraries across projects.</span></span> <span data-ttu-id="a3362-108">Componentes podem ser incluídas nela:</span><span class="sxs-lookup"><span data-stu-id="a3362-108">Components can be included from:</span></span>

* <span data-ttu-id="a3362-109">Outro projeto na solução.</span><span class="sxs-lookup"><span data-stu-id="a3362-109">Another project in the solution.</span></span>
* <span data-ttu-id="a3362-110">Um pacote do NuGet.</span><span class="sxs-lookup"><span data-stu-id="a3362-110">A NuGet package.</span></span>
* <span data-ttu-id="a3362-111">Uma biblioteca referenciada do .NET.</span><span class="sxs-lookup"><span data-stu-id="a3362-111">A referenced .NET library.</span></span>

<span data-ttu-id="a3362-112">Assim como os componentes são tipos regulares do .NET, bibliotecas de componentes são assemblies do .NET normais.</span><span class="sxs-lookup"><span data-stu-id="a3362-112">Just as components are regular .NET types, component libraries are normal .NET assemblies.</span></span>

<span data-ttu-id="a3362-113">Para criar uma nova biblioteca de componente, use o `blazorlib` modelo com o [dotnet novo](/dotnet/core/tools/dotnet-new) comando.</span><span class="sxs-lookup"><span data-stu-id="a3362-113">To create a new component library, use the `blazorlib` template with the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="a3362-114">O modelo faz parte dos modelos instalados quando [Configurando os componentes do Razor](xref:razor-components/get-started).</span><span class="sxs-lookup"><span data-stu-id="a3362-114">The template is part of the templates installed when [setting up Razor Components](xref:razor-components/get-started).</span></span>

```console
dotnet new blazorlib -o MyComponentLib1
```

<span data-ttu-id="a3362-115">Para adicionar a biblioteca a um projeto existente, use o [dotnet sln](/dotnet/core/tools/dotnet-sln) comando:</span><span class="sxs-lookup"><span data-stu-id="a3362-115">To add the library to an existing project, use the [dotnet sln](/dotnet/core/tools/dotnet-sln) command:</span></span>

```console
dotnet sln add .\MyComponentLib1
```

<span data-ttu-id="a3362-116">Bibliotecas de componentes podem conter arquivos estáticos, como imagens, JavaScript e folhas de estilo.</span><span class="sxs-lookup"><span data-stu-id="a3362-116">Component libraries may contain static files, such as images, JavaScript, and stylesheets.</span></span> <span data-ttu-id="a3362-117">No momento da compilação, arquivos estáticos são inseridos no arquivo de assembly compilado (*. dll*), que permite o consumo dos componentes sem precisar se preocupar sobre como incluir seus recursos.</span><span class="sxs-lookup"><span data-stu-id="a3362-117">At build time, static files are embedded into the built assembly file (*.dll*), which allows consumption of the components without having to worry about how to include their resources.</span></span> <span data-ttu-id="a3362-118">Todos os arquivos incluídos no `content` directory são marcados como um recurso inserido.</span><span class="sxs-lookup"><span data-stu-id="a3362-118">Any files included in the `content` directory are marked as an embedded resource.</span></span> 

## <a name="consume-a-library-component"></a><span data-ttu-id="a3362-119">Consumir um componente de biblioteca</span><span class="sxs-lookup"><span data-stu-id="a3362-119">Consume a library component</span></span>

<span data-ttu-id="a3362-120">Para consumir componentes definidos em uma biblioteca em outro projeto, o [ @addTagHelper ](/aspnet/core/mvc/views/tag-helpers/intro#add-helper-label) diretiva deve ser usada.</span><span class="sxs-lookup"><span data-stu-id="a3362-120">In order to consume components defined in a library in another project, the [@addTagHelper](/aspnet/core/mvc/views/tag-helpers/intro#add-helper-label) directive must be used.</span></span> <span data-ttu-id="a3362-121">Componentes individuais podem ser adicionados por nome.</span><span class="sxs-lookup"><span data-stu-id="a3362-121">Individual components may be added by name.</span></span> <span data-ttu-id="a3362-122">Por exemplo, adiciona a seguinte diretiva `Component1` de `MyComponentLib1`:</span><span class="sxs-lookup"><span data-stu-id="a3362-122">For example, the following directive adds `Component1` of `MyComponentLib1`:</span></span>

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1
```

<span data-ttu-id="a3362-123">O formato geral da diretiva é:</span><span class="sxs-lookup"><span data-stu-id="a3362-123">The general format of the directive is:</span></span>

```cshtml
@addTagHelper <namespaced component name>, <assembly name>
```

<span data-ttu-id="a3362-124">No entanto, é comum incluir todos os componentes de um assembly usando um caractere curinga:</span><span class="sxs-lookup"><span data-stu-id="a3362-124">However, it's common to include all of the components from an assembly using a wildcard:</span></span>

```cshtml
@addTagHelper *, MyComponentLib1
```

<span data-ttu-id="a3362-125">O `@addTagHelper` diretiva pode ser incluída no *_ViewImport.cshtml* para tornar os componentes disponíveis para um projeto inteiro ou aplicadas a uma única página ou um conjunto de páginas dentro de uma pasta.</span><span class="sxs-lookup"><span data-stu-id="a3362-125">The `@addTagHelper` directive can be included in *_ViewImport.cshtml* to make the components available for an entire project or applied to a single page or set of pages within a folder.</span></span> <span data-ttu-id="a3362-126">Com o `@addTagHelper` diretiva em vigor, os componentes da biblioteca de componentes podem ser consumidos como se estivessem no mesmo assembly que o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="a3362-126">With the `@addTagHelper` directive in place, the components of the component library can be consumed as if they were in the same assembly as the app.</span></span> 

## <a name="build-pack-and-ship-to-nuget"></a><span data-ttu-id="a3362-127">Compilação, pacote e entregar para o NuGet</span><span class="sxs-lookup"><span data-stu-id="a3362-127">Build, pack, and ship to NuGet</span></span>

<span data-ttu-id="a3362-128">Como as bibliotecas de componentes são bibliotecas .NET padrão, empacotamento e enviá-los para o NuGet não é diferente de empacotamento e envio de qualquer biblioteca no NuGet.</span><span class="sxs-lookup"><span data-stu-id="a3362-128">Because component libraries are standard .NET libraries, packaging and shipping them to NuGet is no different from packaging and shipping any library to NuGet.</span></span> <span data-ttu-id="a3362-129">Empacotamento é realizado usando o [dotnet pack](/dotnet/core/tools/dotnet-pack) comando:</span><span class="sxs-lookup"><span data-stu-id="a3362-129">Packaging is performed using the [dotnet pack](/dotnet/core/tools/dotnet-pack) command:</span></span>

```console
dotnet pack
```

<span data-ttu-id="a3362-130">Carregue o pacote NuGet usando o [dotnet nuget publicar](/dotnet/core/tools/dotnet-nuget-push) comando:</span><span class="sxs-lookup"><span data-stu-id="a3362-130">Upload the package to NuGet using the [dotnet nuget publish](/dotnet/core/tools/dotnet-nuget-push) command:</span></span>

```console
dotnet nuget publish
```

<span data-ttu-id="a3362-131">Todos os recursos estáticos incluídos são incluídos no pacote do NuGet.</span><span class="sxs-lookup"><span data-stu-id="a3362-131">Any included static resources are included in the NuGet package.</span></span> <span data-ttu-id="a3362-132">Os clientes de biblioteca recebem automaticamente scripts e folhas de estilo, para que os consumidores não é necessários instalar manualmente os recursos.</span><span class="sxs-lookup"><span data-stu-id="a3362-132">Library consumers automatically receive scripts and stylesheets, so consumers aren't required to manually install the resources.</span></span>
