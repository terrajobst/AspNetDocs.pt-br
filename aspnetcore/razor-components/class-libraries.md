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
# <a name="razor-components-class-libraries"></a>Bibliotecas de classes de componentes do Razor

Por [Simon Timms](https://github.com/stimms)

> [!NOTE]
> SDK do .NET Core 3.0 versão prévia 2 não inclui um modelo de projeto para bibliotecas de classes de componente do Razor, mas podemos esperar adicionar um modelo em uma visualização futura. Em enquanto isso, você pode usar o modelo de biblioteca de classes de componente Blazor explicado neste tópico.

Componentes podem ser compartilhados em bibliotecas de componentes entre projetos. Componentes podem ser incluídas nela:

* Outro projeto na solução.
* Um pacote do NuGet.
* Uma biblioteca referenciada do .NET.

Assim como os componentes são tipos regulares do .NET, bibliotecas de componentes são assemblies do .NET normais.

Para criar uma nova biblioteca de componente, use o `blazorlib` modelo com o [dotnet novo](/dotnet/core/tools/dotnet-new) comando. O modelo faz parte dos modelos instalados quando [Configurando os componentes do Razor](xref:razor-components/get-started).

```console
dotnet new blazorlib -o MyComponentLib1
```

Para adicionar a biblioteca a um projeto existente, use o [dotnet sln](/dotnet/core/tools/dotnet-sln) comando:

```console
dotnet sln add .\MyComponentLib1
```

Bibliotecas de componentes podem conter arquivos estáticos, como imagens, JavaScript e folhas de estilo. No momento da compilação, arquivos estáticos são inseridos no arquivo de assembly compilado (*. dll*), que permite o consumo dos componentes sem precisar se preocupar sobre como incluir seus recursos. Todos os arquivos incluídos no `content` directory são marcados como um recurso inserido. 

## <a name="consume-a-library-component"></a>Consumir um componente de biblioteca

Para consumir componentes definidos em uma biblioteca em outro projeto, o [ @addTagHelper ](/aspnet/core/mvc/views/tag-helpers/intro#add-helper-label) diretiva deve ser usada. Componentes individuais podem ser adicionados por nome. Por exemplo, adiciona a seguinte diretiva `Component1` de `MyComponentLib1`:

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1
```

O formato geral da diretiva é:

```cshtml
@addTagHelper <namespaced component name>, <assembly name>
```

No entanto, é comum incluir todos os componentes de um assembly usando um caractere curinga:

```cshtml
@addTagHelper *, MyComponentLib1
```

O `@addTagHelper` diretiva pode ser incluída no *_ViewImport.cshtml* para tornar os componentes disponíveis para um projeto inteiro ou aplicadas a uma única página ou um conjunto de páginas dentro de uma pasta. Com o `@addTagHelper` diretiva em vigor, os componentes da biblioteca de componentes podem ser consumidos como se estivessem no mesmo assembly que o aplicativo. 

## <a name="build-pack-and-ship-to-nuget"></a>Compilação, pacote e entregar para o NuGet

Como as bibliotecas de componentes são bibliotecas .NET padrão, empacotamento e enviá-los para o NuGet não é diferente de empacotamento e envio de qualquer biblioteca no NuGet. Empacotamento é realizado usando o [dotnet pack](/dotnet/core/tools/dotnet-pack) comando:

```console
dotnet pack
```

Carregue o pacote NuGet usando o [dotnet nuget publicar](/dotnet/core/tools/dotnet-nuget-push) comando:

```console
dotnet nuget publish
```

Todos os recursos estáticos incluídos são incluídos no pacote do NuGet. Os clientes de biblioteca recebem automaticamente scripts e folhas de estilo, para que os consumidores não é necessários instalar manualmente os recursos.
