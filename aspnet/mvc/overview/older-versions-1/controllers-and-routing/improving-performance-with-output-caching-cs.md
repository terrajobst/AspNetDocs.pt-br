---
uid: mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-cs
title: Melhorando o desempenho com oC#cache de saída () | Microsoft Docs
author: microsoft
description: Neste tutorial, você aprenderá a melhorar drasticamente o desempenho de seus aplicativos Web ASP.NET MVC aproveitando o cache de saída. Você...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 521c9117-81cd-4d8d-9d96-0256dc7bf50f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-cs
msc.type: authoredcontent
ms.openlocfilehash: 548c5bea2e9cf26e0574e72d2c0ea204dbd90f9c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601265"
---
# <a name="improving-performance-with-output-caching-c"></a>Melhorar o desempenho com o cache de saída (C#)

pela [Microsoft](https://github.com/microsoft)

> Neste tutorial, você aprenderá a melhorar drasticamente o desempenho de seus aplicativos Web ASP.NET MVC aproveitando o cache de saída. Você aprende a armazenar em cache o resultado retornado de uma ação do controlador para que o mesmo conteúdo não precise ser criado cada um e sempre que um novo usuário chamar a ação.

O objetivo deste tutorial é explicar como é possível melhorar drasticamente o desempenho de um aplicativo MVC ASP.NET aproveitando o cache de saída. O cache de saída permite que você armazene em cache o conteúdo retornado por uma ação do controlador. Dessa forma, o mesmo conteúdo não precisa ser gerado cada e sempre que a mesma ação do controlador for invocada.

Imagine, por exemplo, que seu aplicativo ASP.NET MVC exiba uma lista de registros de banco de dados em uma exibição denominada index. Normalmente, cada e sempre que um usuário invoca a ação do controlador que retorna a exibição do índice, o conjunto de registros do banco de dados deve ser recuperado do banco de dados executando uma consulta de banco de dados.

Se, por outro lado, você tirar proveito do cache de saída, poderá evitar a execução de uma consulta de banco de dados sempre que qualquer usuário chamar a mesma ação do controlador. O modo de exibição pode ser recuperado do cache em vez de ser regenerado a partir da ação do controlador. O Caching permite evitar a execução de trabalho redundante no servidor.

## <a name="enabling-output-caching"></a>Habilitando o cache de saída

Você pode habilitar o cache de saída adicionando um atributo [OutputCache] a uma ação de controlador individual ou a uma classe de controlador inteira. Por exemplo, o controlador na Listagem 1 expõe uma ação chamada index (). A saída da ação index () é armazenada em cache por 10 segundos.

**Listagem 1 – Controllers\HomeController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample1.cs)]

Nas versões beta do ASP.NET MVC, o cache de saída não funciona para uma URL como [http://www.MySite.com/](http://www.mysite.com/). Em vez disso, você deve inserir uma URL como [http://www.MySite.com/Home/Index](http://www.mysite.com/Home/Index). 

Na Listagem 1, a saída da ação index () é armazenada em cache por 10 segundos. Se preferir, você pode especificar uma duração de cache muito maior. Por exemplo, se você quiser armazenar em cache a saída de uma ação do controlador para um dia, poderá especificar uma duração de cache de 86400 segundos (60 segundos \* 60 minutos \* 24 horas).

Não há nenhuma garantia de que o conteúdo será armazenado em cache pelo período de tempo especificado. Quando os recursos de memória ficarem baixos, o cache começará a remover o conteúdo automaticamente.

O controlador Home na Listagem 1 retorna a exibição de índice na Listagem 2. Não há nada de especial sobre essa exibição. A exibição índice simplesmente exibe a hora atual (consulte a Figura 1).

**Listagem 2 – Views\Home\Index.aspx**

[!code-aspx[Main](improving-performance-with-output-caching-cs/samples/sample2.aspx)]

**Figura 1 – exibição de índice em cache**

![clip_image002](improving-performance-with-output-caching-cs/_static/image1.jpg)

Se você invocar a ação index () várias vezes inserindo a URL/Home/Index na barra de endereços do seu navegador e pressionando o botão atualizar/recarregar no navegador repetidamente, o horário exibido pela exibição do índice não será alterado por 10 segundos. O mesmo horário é exibido porque a exibição é armazenada em cache.

É importante entender que a mesma exibição é armazenada em cache para todos que visitam seu aplicativo. Qualquer pessoa que invoque a ação index () obterá a mesma versão armazenada em cache da exibição de índice. Isso significa que a quantidade de trabalho que o servidor Web deve executar para atender à exibição do índice é drasticamente reduzida.

A exibição na Listagem 2 acontece para fazer algo realmente simples. A exibição apenas exibe a hora atual. No entanto, você poderia facilmente armazenar em cache uma exibição que exibe um conjunto de registros de banco de dados. Nesse caso, o conjunto de registros de banco de dados não precisa ser recuperado do banco de dados cada e sempre que a ação do controlador que retorna a exibição é invocada. O Caching pode reduzir a quantidade de trabalho que o servidor Web e o servidor de banco de dados devem executar.

Não use a &lt;da página% @ OutputCache%&gt; diretiva em uma exibição do MVC. Essa diretiva está se comportando do mundo Web Forms e não deve ser usada em um aplicativo MVC ASP.NET.

## <a name="where-content-is-cached"></a>Onde o conteúdo é armazenado em cache

Por padrão, quando você usa o atributo [OutputCache], o conteúdo é armazenado em cache em três locais: o servidor Web, todos os servidores proxy e o navegador da Web. Você pode controlar exatamente onde o conteúdo é armazenado em cache modificando a propriedade Location do atributo [OutputCache].

Você pode definir a propriedade Location para qualquer um dos seguintes valores:

> · Outro
> 
> · Cliente
> 
> · Inferior
> 
> · Servidor
> 
> · None
> 
> · ServerAndClient

Por padrão, a propriedade Location tem o valor any. No entanto, há situações em que você talvez queira armazenar em cache somente no navegador ou somente no servidor. Por exemplo, se você estiver armazenando em cache informações personalizadas para cada usuário, não deverá armazenar em cache as informações no servidor. Se você estiver exibindo informações diferentes para usuários diferentes, deverá armazenar em cache as informações somente no cliente.

Por exemplo, o controlador na Listagem 3 expõe uma ação chamada GetName () que retorna o nome de usuário atual. Se o Jack fizer logon no site e invocar a ação GetName (), a ação retornará a cadeia de caracteres "ficha Hi". Se, subsequentemente, Jill fizer logon no site e invocar a ação GetName (), ela também receberá a cadeia de caracteres "Hi Jack". A cadeia de caracteres é armazenada em cache no servidor Web para todos os usuários após a tomada inicialmente invocar a ação do controlador.

**Listagem 3 – Controllers\BadUserController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample3.cs)]

Provavelmente, o controlador na Listagem 3 não funciona da maneira desejada. Você não quer exibir a mensagem "Hi Jack" para Jill.

Você nunca deve armazenar em cache o conteúdo personalizado no cache do servidor. No entanto, talvez você queira armazenar em cache o conteúdo personalizado no cache do navegador para melhorar o desempenho. Se você armazenar em cache o conteúdo no navegador e um usuário invocar a mesma ação do controlador várias vezes, o conteúdo poderá ser recuperado do cache do navegador em vez do servidor.

O controlador modificado na Listagem 4 armazena em cache a saída da ação GetName (). No entanto, o conteúdo é armazenado em cache somente no navegador e não no servidor. Dessa forma, quando vários usuários chamam o método GetName (), cada pessoa obtém seu próprio nome de usuário e não o nome de usuário de outra pessoa.

**Listagem 4 – Controllers\UserController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample4.cs)]

Observe que o atributo [OutputCache] na Listagem 4 inclui uma propriedade Location definida como o valor OutputCacheLocation. Client. O atributo [OutputCache] também inclui uma propriedade NoStore. A propriedade NoStore é usada para informar aos servidores proxy e ao navegador que eles não devem armazenar uma cópia permanente do conteúdo armazenado em cache.

## <a name="varying-the-output-cache"></a>Variando o cache de saída

Em algumas situações, talvez você queira diferentes versões em cache do mesmo conteúdo. Imagine, por exemplo, que você está criando uma página mestra/de detalhes. A página mestra exibe uma lista de títulos de filmes. Ao clicar em um título, você obtém detalhes do filme selecionado.

Se você armazenar em cache a página de detalhes, os detalhes do mesmo filme serão exibidos, independentemente do filme em que você clicar. O primeiro filme selecionado pelo primeiro usuário será exibido para todos os usuários futuros.

Você pode corrigir esse problema tirando proveito da propriedade VaryByParam do atributo [OutputCache]. Essa propriedade permite que você crie diferentes versões em cache do mesmo conteúdo quando um parâmetro de formulário ou parâmetro de cadeia de caracteres de consulta varia.

Por exemplo, o controlador na listagem 5 expõe duas ações chamadas Master () e Details (). A ação mestre () retorna uma lista de títulos de filmes e a ação detalhes () retorna os detalhes do filme selecionado.

**Listagem 5 – Controllers\MoviesController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample5.cs)]

A ação mestre () inclui uma propriedade VaryByParam com o valor "None". Quando a ação mestre () é invocada, a mesma versão em cache do modo de exibição mestre é retornada. Quaisquer parâmetros de formulário ou parâmetros de cadeia de caracteres de consulta são ignorados (consulte a Figura 2).

**Figura 2 – a exibição/Movies/Master**

![clip_image004](improving-performance-with-output-caching-cs/_static/image2.jpg)

**Figura 3 – a exibição/Movies/Details**

![clip_image006](improving-performance-with-output-caching-cs/_static/image3.jpg)

A ação Details () inclui uma propriedade VaryByParam com o valor "ID". Quando valores diferentes do parâmetro ID são passados para a ação do controlador, diferentes versões armazenadas em cache do modo de exibição de detalhes são geradas.

É importante entender que o uso da propriedade VaryByParam resulta em mais cache e não menos. Uma versão armazenada em cache diferente da exibição de detalhes é criada para cada versão diferente do parâmetro ID.

Você pode definir a propriedade VaryByParam com os seguintes valores:

> \* = crie uma versão armazenada em cache diferente sempre que um parâmetro de cadeia de caracteres de formulário ou de consulta varia.
> 
> nenhum = nunca criar diferentes versões em cache
> 
> Lista de pontos-e-vírgulas de parâmetros = criar diferentes versões armazenadas em cache sempre que qualquer um dos parâmetros de cadeia de caracteres de consulta ou formulário na lista varia

## <a name="creating-a-cache-profile"></a>Criando um perfil de cache

Como alternativa para configurar as propriedades do cache de saída modificando as propriedades do atributo [OutputCache], você pode criar um perfil de cache no arquivo de configuração da Web (Web. config). A criação de um perfil de cache no arquivo de configuração da Web oferece algumas vantagens importantes.

Primeiro, ao configurar o cache de saída no arquivo de configuração da Web, você pode controlar como as ações do controlador armazenam o conteúdo em cache em um local central. Você pode criar um perfil de cache e aplicar o perfil a vários controladores ou ações de controlador.

Em segundo lugar, você pode modificar o arquivo de configuração da Web sem recompilar seu aplicativo. Se você precisar desabilitar o cache para um aplicativo que já foi implantado na produção, poderá simplesmente modificar os perfis de cache definidos no arquivo de configuração da Web. Todas as alterações no arquivo de configuração da Web serão detectadas automaticamente e aplicadas.

Por exemplo, a seção de configuração da Web do &lt;Caching&gt; na Listagem 6 define um perfil de cache chamado Cache1Hour. A seção&gt; do &lt;Caching deve aparecer na seção do &lt;System. Web&gt; de um arquivo de configuração da Web.

**Listagem 6 – seção de cache para Web. config**

[!code-xml[Main](improving-performance-with-output-caching-cs/samples/sample6.xml)]

O controlador na Listagem 7 ilustra como você pode aplicar o perfil Cache1Hour a uma ação do controlador com o atributo [OutputCache].

**Listagem 7 – Controllers\ProfileController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample7.cs)]

Se você invocar a ação index () exposta pelo controlador na Listagem 7, a mesma hora será retornada por 1 hora.

## <a name="summary"></a>Resumo

O cache de saída fornece um método muito fácil de melhorar drasticamente o desempenho de seus aplicativos MVC ASP.NET. Neste tutorial, você aprendeu a usar o atributo [OutputCache] para armazenar em cache a saída de ações do controlador. Você também aprendeu como modificar as propriedades do atributo [OutputCache], como as propriedades Duration e VaryByParam, para modificar como o conteúdo é armazenado em cache. Por fim, você aprendeu como definir perfis de cache no arquivo de configuração da Web.

> [!div class="step-by-step"]
> [Anterior](understanding-action-filters-cs.md)
> [Próximo](adding-dynamic-content-to-a-cached-page-cs.md)
