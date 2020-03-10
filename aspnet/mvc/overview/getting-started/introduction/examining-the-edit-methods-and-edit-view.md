---
uid: mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
title: Examinando os métodos de edição e o modo de exibição de edição | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/06/2019
ms.assetid: 52a4d5fe-aa31-4471-b3cb-a064f82cb791
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: 6cef963910b957e8b4ad7c7909385f6dbdff95c1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78582435"
---
# <a name="examining-the-edit-methods-and-edit-view"></a>Examinar os métodos de edição e a exibição de edição

por [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [Tutorial Note](index.md)]

Nesta seção, você examinará os métodos de ação `Edit` gerados e as exibições do controlador de filme. Mas primeiro vamos pegar uma pequena versão para melhorar a aparência da data de lançamento. Abra o arquivo *Models\Movie.cs* e adicione as linhas realçadas mostradas abaixo:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cs?highlight=2,12-14)]

Você também pode tornar a cultura de data específica como esta:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs?highlight=3)]

Abordaremos [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) no próximo tutorial. O atributo [Display](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayattribute.aspx) especifica o que deve ser exibido no nome de um campo (neste caso, “Release Date” em vez de “ReleaseDate”). O atributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) especifica o tipo dos dados, neste caso, é uma data, portanto, as informações de hora armazenadas no campo não são exibidas. O atributo [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) é necessário para um bug no navegador Chrome que processa formatos de data incorretamente.

Execute o aplicativo e navegue até o controlador de `Movies`. Mantenha o ponteiro do mouse sobre um link de **edição** para ver a URL à qual ele se vincula.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

O link de **edição** foi gerado pelo método de `Html.ActionLink` na exibição *Views\Movies\Index.cshtml* :

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cshtml)]

![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

O objeto `Html` é um auxiliar que é exposto usando uma propriedade na classe base [System. Web. Mvc. WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx) . O método `ActionLink` do auxiliar facilita a geração dinâmica de hiperlinks HTML que se vinculam a métodos de ação em controladores. O primeiro argumento para o método `ActionLink` é o texto do link a ser renderizado (por exemplo, `<a>Edit Me</a>`). O segundo argumento é o nome do método de ação a ser invocado (nesse caso, a ação de `Edit`). O argumento final é um [objeto anônimo](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) que gera os dados da rota (nesse caso, a ID de 4).

O link gerado mostrado na imagem anterior é `http://localhost:1234/Movies/Edit/4`. A rota padrão (estabelecida no *aplicativo\_Start\RouteConfig.cs*) usa o padrão de URL `{controller}/{action}/{id}`. Portanto, o ASP.NET converte `http://localhost:1234/Movies/Edit/4` em uma solicitação para o método de ação `Edit` do controlador de `Movies` com o parâmetro `ID` igual a 4. Examine o seguinte código do *aplicativo\_arquivo Start\RouteConfig.cs* . O método [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) é usado para rotear solicitações HTTP para o controlador e o método de ação corretos e fornecer o parâmetro de ID opcional. O método [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) também é usado pelo [HtmlHelpers](https://msdn.microsoft.com/library/system.web.mvc.htmlhelper(v=vs.108).aspx) , como `ActionLink` para gerar URLs dadas o controlador, o método de ação e os dados de rota.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cs?highlight=7)]

Você também pode passar parâmetros de método de ação usando uma cadeia de caracteres de consulta. Por exemplo, a URL `http://localhost:1234/Movies/Edit?ID=3` também passa o parâmetro `ID` de 3 para o método de ação `Edit` do controlador de `Movies`.

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

Abra o controlador de `Movies`. Os dois métodos de ação de `Edit` são mostrados abaixo.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample5.cs?highlight=19-21)]

Observe se o segundo método de ação `Edit` é precedido pelo atributo `HttpPost`. Esse atributo especifica que a sobrecarga do método `Edit` pode ser invocada somente para solicitações POST. Você pode aplicar o atributo `HttpGet` ao primeiro método Edit, mas isso não é necessário porque é o padrão. (Vamos nos referir aos métodos de ação que são atribuídos implicitamente ao atributo `HttpGet` como `HttpGet` métodos). O atributo [BIND](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) é outro mecanismo de segurança importante que impede que os hackers sobrepostem dados ao seu modelo. Você só deve incluir propriedades no atributo de associação que deseja alterar. Você pode ler sobre sobrepostos e o atributo BIND na minha [Observação de segurança de sobreposição](https://go.microsoft.com/fwlink/?LinkId=317598). No modelo simples usado neste tutorial, iremos ligar todos os dados no modelo. O atributo [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) é usado para impedir falsificação de uma solicitação e é emparelhado com `@Html.AntiForgeryToken()` no*Views\Movies\Edit.cshtml*(arquivo de exibição de edição), uma parte é mostrada abaixo:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cshtml?highlight=9)]

`@Html.AntiForgeryToken()` gera um token antifalsificação de formulário oculto que deve corresponder ao método `Edit` do controlador de `Movies`. Você pode ler mais sobre falsificação de solicitação entre sites (também conhecido como XSRF ou CSRF) em minha [prevenção de XSRF/CSRF do tutorial no MVC](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md).

O método `HttpGet` `Edit` usa o parâmetro ID do filme, pesquisa o filme usando o Método Entity Framework `Find` e retorna o filme selecionado para o modo de exibição Editar. Se não for possível encontrar um filme, [HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx) será retornado. Quando o sistema de scaffolding criou a exibição de Edição, ele examinou a classe `Movie` e o código criado para renderizar os elementos `<label>` e `<input>` de cada propriedade da classe. O exemplo a seguir mostra o modo de exibição de edição que foi gerado pelo sistema scaffolding do Visual Studio:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

Observe como o modelo de exibição tem uma instrução `@model MvcMovie.Models.Movie` na parte superior do arquivo — isso especifica que a exibição espera que o modelo para o modelo de exibição seja do tipo `Movie`.

O código com Scaffold usa vários *métodos auxiliares* para simplificar a marcação HTML. O auxiliar de [`Html.LabelFor`](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) exibe o nome do campo (&quot;título&quot;, &quot;liberou&quot;, &quot;gênero&quot;ou &quot;&quot;de preço). O auxiliar de [`Html.EditorFor`](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) renderiza um elemento HTML `<input>`. O auxiliar de [`Html.ValidationMessageFor`](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) exibe todas as mensagens de validação associadas a essa propriedade.

Execute o aplicativo e navegue até a URL */Movies* . Clique em um link **Editar**. No navegador, exiba a origem da página. O HTML do elemento form é mostrado abaixo.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.cshtml?highlight=1-2)]

Os elementos de `<input>` estão em um elemento HTML `<form>` cujo atributo `action` é definido como post para a URL */Movies/Edit* . Os dados do formulário serão postados no servidor quando o botão **salvar** for clicado. A segunda linha mostra o token [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) oculto gerado pela chamada `@Html.AntiForgeryToken()`.

## <a name="processing-the-post-request"></a>Processando a solicitação POST

A lista a seguir mostra a versão `HttpPost` do método de ação `Edit`.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

O atributo [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) valida o token [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) gerado pela chamada `@Html.AntiForgeryToken()` na exibição.

O [associador de modelo MVC do ASP.net](https://msdn.microsoft.com/library/dd410405.aspx) pega os valores de formulário postados e cria um objeto `Movie` que é passado como o parâmetro `movie`. O `ModelState.IsValid` verifica se os dados enviados no formulário podem ser usados para modificar (editar ou atualizar) um objeto `Movie`. Se os dados forem válidos, os dados do filme serão salvos na coleção de `Movies` do `db`(`MovieDBContext` instância). Os novos dados de filme são salvos no banco de dado chamando o método de `SaveChanges` de `MovieDBContext`. Depois de salvar os dados, o código redireciona o usuário para o método de ação `Index` da classe `MoviesController`, que exibe a coleção de filmes, incluindo as alterações feitas recentemente.

Assim que a validação do lado do cliente determina que o valor de um campo não é válido, uma mensagem de erro é exibida. Se o JavaScript estiver desabilitado, a validação do lado do cliente será desabilitada. No entanto, o servidor detecta que os valores postados não são válidos e os valores de formulário são reexibidos com mensagens de erro.

A validação é examinada em mais detalhes posteriormente no tutorial.

Os auxiliares `Html.ValidationMessageFor` no modelo de exibição *Edit. cshtml* cuidam da exibição das mensagens de erro apropriadas.

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

Todos os métodos de `HttpGet` seguem um padrão semelhante. Eles obtêm um objeto de filme (ou lista de objetos, no caso de `Index`) e passam o modelo para a exibição. O método `Create` passa um objeto de filme vazio para o modo de exibição Create. Todos os métodos que criam, editam, excluem ou, de outro modo, modificam dados fazem isso na sobrecarga `HttpPost` do método. A modificação de dados em um método HTTP GET é um risco de segurança, conforme descrito na entrada de postagem de blog [ASP.net a Tip do MVC #46 – não use os links de exclusão porque eles criam brechas de segurança](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Modificar dados em um método GET também viola as práticas recomendadas de HTTP e o padrão de [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer) arquitetônico, que especifica que as solicitações GET não devem alterar o estado do seu aplicativo. Em outras palavras, a execução de uma operação GET deve ser uma operação segura que não tem efeitos colaterais e não modifica os dados persistentes.

## <a name="jquery-validation-for-non-english-locales"></a>validação do jQuery para localidades não inglesas

Se você estiver usando um computador em inglês dos EUA, poderá ignorar esta seção e ir para o próximo tutorial. Você pode baixar a versão Globalizate deste tutorial [aqui](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=16475). Para obter um tutorial de duas partes excelente sobre internacionalização, consulte a [internacionalização do ASP.NET MVC 5 da Nadeem](http://afana.me/post/aspnet-mvc-internationalization.aspx).

> [!NOTE]
> Para dar suporte à validação do jQuery para localidades não inglesas que usam uma vírgula (&quot;,&quot;) para um ponto decimal, e os formatos de data em inglês dos EUA, você deve incluir *globalizable. js* e seu arquivo *culturas/globalizate. culturas* específico (de [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) e JavaScript para usar `Globalize.parseFloat`. Você pode obter a validação do jQuery que não está em inglês do NuGet. (Não instale Globalizate se você estiver usando uma localidade em inglês.)

1. No menu **ferramentas** , clique em **Gerenciador de pacotes NuGet**e, em seguida, clique em **gerenciar pacotes NuGet para solução**.

    ![](examining-the-edit-methods-and-edit-view/_static/image5.png)
2. No painel esquerdo, selecione <strong>procurar *.</strong>*  (Veja a imagem abaixo.)
3. Na caixa de entrada, digite * Globalizate * *.

    ![](examining-the-edit-methods-and-edit-view/_static/image6.png) escolha `jQuery.Validation.Globalize`, escolha `MvcMovie` e clique em **instalar**. O arquivo *Scripts\jquery.globalize\globalize.js* será adicionado ao seu projeto. A pasta * Scripts\jquery.globalize\cultures\* conterá muitos arquivos JavaScript de cultura. Observe que pode levar cinco minutos para instalar este pacote.

   O código a seguir mostra as modificações no arquivo Views\Movies\Edit.cshtml:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cshtml)]

Para evitar repetir esse código em cada modo de exibição de edição, você pode movê-lo para o arquivo de layout. Para otimizar o download do script, consulte meu tutorial [reunindo e minificação](../../performance/bundling-and-minification.md).

Para obter mais informações, consulte [ASP.NET MVC 3 internacionalização](http://afana.me/post/aspnet-mvc-internationalization.aspx) e [ASP.NET MVC 3 internacionalização – parte 2 (NerdDinner)](http://afana.me/post/aspnet-mvc-internationalization-part-2.aspx).

Como uma correção temporária, se você não conseguir fazer a validação funcionar na sua localidade, poderá forçar seu computador a usar o inglês dos EUA ou pode desabilitar o JavaScript em seu navegador. Para forçar seu computador a usar o inglês americano, você pode adicionar o elemento Globalization ao arquivo *Web. config da* raiz de projetos. O código a seguir mostra o elemento Globalization com a cultura definida como Estados Unidos Inglês.

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample11.xml)]

<a id="gettingstarted"></a><a id="jQueryAjaxJSON"></a>No próximo tutorial, implementaremos a funcionalidade de pesquisa.

> [!div class="step-by-step"]
> [Anterior](accessing-your-models-data-from-a-controller.md)
> [Próximo](adding-search.md)
