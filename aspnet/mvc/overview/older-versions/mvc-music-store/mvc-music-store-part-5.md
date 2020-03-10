---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
title: 'Parte 5: editar formulários e Templating | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo de loja de música MVC do ASP.NET. A parte 5 aborda os formulários de edição e modelagem.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 6b09413a-6d6a-425a-87c9-629f91b91b28
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
msc.type: authoredcontent
ms.openlocfilehash: 20b99cbe57b5dfa623205838a5929733a6c2d70d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559545"
---
# <a name="part-5-edit-forms-and-templating"></a>Parte 5: editar formulários e modelos

por [Jon Galloway](https://github.com/jongalloway)

> A loja de música MVC é um aplicativo de tutorial que apresenta e explica passo a passo como usar o ASP.NET MVC e o Visual Studio para desenvolvimento para a Web.  
>   
> A loja de música MVC é uma implementação de armazenamento de exemplo leve que vende os álbuns de música online e implementa a administração básica de site, a entrada do usuário e a funcionalidade do carrinho de compras.
> 
> Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo de loja de música MVC do ASP.NET. A parte 5 aborda os formulários de edição e modelagem.

No capítulo anterior, estávamos carregando dados de nosso banco de dado e exibindo-os. Neste capítulo, também Habilitaremos a edição dos dados.

## <a name="creating-the-storemanagercontroller"></a>Criando o StoreManagerController

Vamos começar criando um novo controlador chamado **StoreManagerController**. Para esse controlador, aproveitaremos os recursos do scaffolding disponíveis na atualização das ferramentas do ASP.NET MVC 3. Defina as opções para a caixa de diálogo Adicionar controlador, conforme mostrado abaixo.

![](mvc-music-store-part-5/_static/image1.png)

Ao clicar no botão Adicionar, você verá que o mecanismo ASP.NET MVC 3 scaffolding faz uma boa quantidade de trabalho para você:

- Ele cria o novo StoreManagerController com uma variável de Entity Framework local
- Ele adiciona uma pasta Storemanager à pasta views do projeto
- Ele adiciona Create. cshtml, Delete. cshtml, details. cshtml, Edit. cshtml e index. cshtml View, fortemente tipado à classe Album

![](mvc-music-store-part-5/_static/image2.png)

A nova classe do controlador Storemanager inclui ações de controlador CRUD (criar, ler, atualizar, excluir) que sabem como trabalhar com a classe de modelo de álbum e usar nosso contexto de Entity Framework para acesso ao banco de dados.

## <a name="modifying-a-scaffolded-view"></a>Modificando uma exibição do com Scaffold

É importante lembrar que, embora esse código tenha sido gerado para nós, ele é o código MVC padrão do ASP.NET, assim como escrevemos durante todo este tutorial. Ele se destina a poupar o tempo que você gastaria escrevendo código de controlador clichê e criando exibições com rigidez de tipos manualmente, mas esse não é o tipo de código gerado que você pode ter visto precedido com avisos de dire em comentários sobre como você não deve alterar o auto-completar. Esse é o seu código e você deve alterá-lo.

Portanto, vamos começar com uma edição rápida para a exibição do índice do Storemanager (/Views/StoreManager/Index.cshtml). Essa exibição exibirá uma tabela que lista os álbuns em nossa loja com editar/detalhes/excluir links e inclui as propriedades públicas do álbum. Removeremos o campo AlbumArtUrl, pois ele não é muito útil nessa exibição. Na tabela &lt;&gt; seção do código de exibição, remova os elementos &lt;th&gt; e &lt;TD&gt; ao redor das referências AlbumArtUrl, conforme indicado pelas linhas realçadas abaixo:

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample1.cshtml)]

O código de exibição modificado será exibido da seguinte maneira:

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample2.cshtml)]

## <a name="a-first-look-at-the-store-manager"></a>Uma primeira olhada no Store Manager

Agora execute o aplicativo e navegue até/StoreManager/. Isso exibe o índice do Store Manager que acabamos de modificar, mostrando uma lista dos álbuns no repositório com links para editar, detalhar e excluir.

![](mvc-music-store-part-5/_static/image3.png)

Clicar no link Editar exibe um formulário de edição com campos para o álbum, incluindo os menus suspensos para gênero e artista.

![](mvc-music-store-part-5/_static/image4.png)

Clique no link "voltar à lista" na parte inferior e, em seguida, clique no link detalhes de um álbum. Isso exibe as informações detalhadas de um álbum individual.

![](mvc-music-store-part-5/_static/image5.png)

Novamente, clique no link voltar à lista e, em seguida, clique em um link de exclusão. Isso exibe uma caixa de diálogo de confirmação, mostrando os detalhes do álbum e perguntando se temos certeza de que desejamos excluí-lo.

![](mvc-music-store-part-5/_static/image6.png)

Clicar no botão excluir na parte inferior excluirá o álbum e o retornará para a página de índice, que mostra o álbum excluído.

Não fazemos isso com o Store Manager, mas temos o controlador de trabalho e o código de exibição para as operações CRUD começarem.

## <a name="looking-at-the-store-manager-controller-code"></a>Examinando o código do controlador do Store Manager

O controlador do Store Manager contém uma boa quantidade de código. Vamos percorrer isso de cima para baixo. O controlador inclui alguns namespaces padrão para um controlador MVC, bem como uma referência ao nosso namespace de modelos. O controlador tem uma instância privada de MusicStoreEntities, usada por cada uma das ações do controlador para acesso a dados.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample3.cs)]

### <a name="store-manager-index-and-details-actions"></a>Ações de índice e detalhes do Store Manager

A exibição de índice recupera uma lista de álbuns, incluindo as informações de gênero e artista referenciadas de cada álbum, como vimos anteriormente ao trabalhar no método de procura da loja. A exibição de índice está seguindo as referências aos objetos vinculados para que ele possa exibir o nome do gênero e o nome do artista de cada álbum, para que o controlador esteja sendo eficiente e consultando essas informações na solicitação original.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample4.cs)]

A ação do controlador de detalhes do controlador Storemanager funciona exatamente da mesma forma que a ação de detalhes do controlador da loja que escrevemos anteriormente. ele consulta o álbum por ID usando o método Find () e, em seguida, retorna-o para a exibição.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample5.cs)]

### <a name="the-create-action-methods"></a>Os métodos de ação Create

Os métodos de ação Create são um pouco diferentes daqueles que vimos até agora, pois manipulam a entrada de formulário. Quando um usuário visita pela primeira vez/StoreManager/Create/, ele mostrará um formulário vazio. Essa página HTML conterá um &lt;formulário&gt; elemento que contém os elementos de entrada DropDown e TextBox, em que eles podem inserir os detalhes do álbum.

Depois que o usuário preenche os valores de formulário do álbum, eles podem pressionar o botão "salvar" para enviar essas alterações de volta ao nosso aplicativo para salvar no banco de dados. Quando o usuário pressionar o botão "salvar", o formulário de &lt;&gt; executará um HTTP-POST de volta para a URL/StoreManager/Create/e enviará os valores&gt; formulário de &lt;como parte do HTTP-POST.

O ASP.NET MVC nos permite dividir facilmente a lógica desses dois cenários de invocação de URL, permitindo que implementem dois métodos de ação "Create" separados em nossa classe StoreManagerController – um para tratar o HTTP inicial GET para a URL/StoreManager/Create/e o outro para manipular o HTTP-POST das alterações enviadas.

### <a name="passing-information-to-a-view-using-viewbag"></a>Passando informações para uma exibição usando ViewBag

Usamos a ViewBag anteriormente neste tutorial, mas não falamos muito sobre isso. ViewBag nos permite passar informações para a exibição sem usar um objeto de modelo fortemente tipado. Nesse caso, nossa ação editar controlador HTTP-GET precisa passar uma lista de gêneros e artistas para o formulário para preencher os menus suspensos e a maneira mais simples de fazer isso é retorná-los como itens ViewBag.

ViewBag é um objeto dinâmico, o que significa que você pode digitar ViewBag. foo ou ViewBag. YourNameHere sem escrever código para definir essas propriedades. Nesse caso, o código do controlador usa ViewBag. Gêneroid e ViewBag. Artistaid para que os valores suspensos enviados com o formulário sejam Gêneroid e Artistaid, que são as propriedades do álbum que serão configuradas.

Esses valores suspensos são retornados para o formulário usando o objeto SelectList, que é criado apenas para essa finalidade. Isso é feito usando um código como este:

[!code-csharp[Main](mvc-music-store-part-5/samples/sample6.cs)]

Como você pode ver no código do método de ação, três parâmetros estão sendo usados para criar esse objeto:

- A lista de itens que o menu suspenso exibirá. Observe que isso não é apenas uma cadeia de caracteres – estamos passando uma lista de gêneros.
- O próximo parâmetro que está sendo passado para a SelectList é o valor selecionado. Esta é a maneira como a lista de seleção sabe como selecionar previamente um item na lista. Isso será mais fácil de entender quando observarmos o formulário de edição, que é bastante semelhante.
- O parâmetro final é a propriedade a ser exibida. Nesse caso, isso indica que a propriedade Genre.Name é o que será exibido para o usuário.

Com isso em mente, a ação de criação de HTTP-GET é bem simples-duas SelectLists são adicionadas a ViewBag e nenhum objeto de modelo é passado para o formulário (já que ainda não foi criado).

[!code-csharp[Main](mvc-music-store-part-5/samples/sample7.cs)]

### <a name="html-helpers-to-display-the-drop-downs-in-the-create-view"></a>Auxiliares HTML para exibir os menus suspensos na exibição criar

Como falamos sobre como os valores de lista suspensa são passados para a exibição, vamos dar uma olhada rápida na exibição para ver como esses valores são exibidos. No código de exibição (/Views/StoreManager/Create.cshtml), você verá que a seguinte chamada é feita para exibir a lista suspensa gênero.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample8.cshtml)]

Isso é conhecido como um auxiliar HTML – um método utilitário que executa uma tarefa de exibição comum. Os auxiliares HTML são muito úteis para manter nosso código de exibição conciso e legível. O auxiliar HTML. DropDownList é fornecido pelo ASP.NET MVC, mas como veremos posteriormente, é possível criar nossos próprios auxiliares para o código de exibição que vamos reutilizar em nosso aplicativo.

A chamada de HTML. DropDownList só precisa ser formada por duas coisas – onde obter a lista para exibição e qual valor (se houver) deve ser pré-selecionado. O primeiro parâmetro, Gêneroid, informa ao DropDownList para procurar um valor chamado Gêneroid no modelo ou ViewBag. O segundo parâmetro é usado para indicar o valor a ser mostrado como selecionado inicialmente na lista suspensa. Como esse formulário é um formulário de criação, não há nenhum valor a ser selecionado e String. Empty é passado.

### <a name="handling-the-posted-form-values"></a>Manipulando os valores de formulário postados

Como discutimos anteriormente, há dois métodos de ação associados a cada formulário. O primeiro manipula a solicitação HTTP-GET e exibe o formulário. O segundo manipula a solicitação HTTP-POST, que contém os valores de formulário enviados. Observe que a ação do controlador tem um atributo [HttpPost], que informa ao ASP.NET MVC que ele só deve responder às solicitações HTTP-POST.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample9.cs)]

Esta ação tem quatro responsabilidades:

- 1. Ler os valores de formulário
- 2. Verificar se os valores de formulário passam por qualquer regra de validação
- 3. Se o envio do formulário for válido, salve os dados e exiba a lista atualizada
- 4. Se o envio do formulário não for válido, exiba novamente o formulário com erros de validação

#### <a name="reading-form-values-with-model-binding"></a>Lendo valores de formulário com associação de modelo

A ação do controlador está processando um envio de formulário que inclui valores para Gêneroid e Artistaid (da lista suspensa) e valores de caixa de texto para título, preço e AlbumArtUrl. Embora seja possível acessar diretamente os valores de formulário, uma abordagem melhor é usar os recursos de associação de modelo criados no ASP.NET MVC. Quando uma ação do controlador usa um tipo de modelo como parâmetro, o ASP.NET MVC tentará preencher um objeto desse tipo usando entradas de formulário (bem como valores de Route e QueryString). Ele faz isso procurando valores cujos nomes correspondem às propriedades do objeto de modelo, por exemplo, ao definir o valor de Gêneroid do novo objeto de álbum, ele procura uma entrada com o nome Gêneroid. Quando você cria modos de exibição usando os métodos padrão no ASP.NET MVC, os formulários sempre serão renderizados usando nomes de propriedade como nomes de campo de entrada, de modo que os nomes dos campos só corresponderão.

#### <a name="validating-the-model"></a>Validando o modelo

O modelo é validado com uma chamada simples para ModelState. IsValid. Ainda não adicionamos nenhuma regra de validação à nossa classe de álbum – faremos isso em breve. agora, essa verificação não tem muito a ver. O que é importante é que essa verificação de ModelStat. IsValid se adaptará às regras de validação que colocamos em nosso modelo, assim as alterações futuras nas regras de validação não exigirão nenhuma atualização para o código de ação do controlador.

#### <a name="saving-the-submitted-values"></a>Salvando os valores enviados

Se o envio do formulário passar na validação, será hora de salvar os valores no banco de dados. Com Entity Framework, isso apenas requer a adição do modelo à coleção de álbuns e a chamada a SaveChanges.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample10.cs)]

Entity Framework gera os comandos SQL apropriados para manter o valor. Depois de salvar os dados, redirecionamos de volta para a lista de álbuns para que possamos ver nossa atualização. Isso é feito retornando RedirectToAction com o nome da ação do controlador que desejamos exibir. Nesse caso, esse é o método de índice.

#### <a name="displaying-invalid-form-submissions-with-validation-errors"></a>Exibindo envios de formulário inválidos com erros de validação

No caso de entrada de formulário inválida, os valores suspensos são adicionados ao ViewBag (como no caso de HTTP-GET) e os valores do modelo associado são passados de volta para a exibição para exibição. Os erros de validação são exibidos automaticamente usando o @Html.ValidationMessageFor HTML Helper.

#### <a name="testing-the-create-form"></a>Testando o formulário de criação

Para testar isso, execute o aplicativo e navegue até/StoreManager/Create/-isso lhe mostrará o formulário em branco que foi retornado pelo método Create HTTP-GET de StoreController.

Preencha alguns valores e clique no botão criar para enviar o formulário.

![](mvc-music-store-part-5/_static/image7.png)

![](mvc-music-store-part-5/_static/image8.png)

### <a name="handling-edits"></a>Manipulando edições

O par editar ação (HTTP-GET e HTTP-POST) são muito semelhantes aos métodos de ação de criação que acabamos de examinar. Como o cenário de edição envolve trabalhar com um álbum existente, o método Edit HTTP-GET carrega o álbum com base no parâmetro "ID", passado por meio da rota. Esse código para recuperar um álbum por albumid é o mesmo que vimos anteriormente na ação do controlador de detalhes. Assim como acontece com o método Create/HTTP-GET, os valores suspensos são retornados por meio de ViewBag. Isso nos permite retornar um álbum como nosso objeto de modelo para a exibição (que é fortemente tipada para a classe de álbum) ao passar dados adicionais (por exemplo, uma lista de gêneros) por meio de ViewBag.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample11.cs)]

A ação editar HTTP-POST é muito semelhante à ação criar HTTP-POST. A única diferença é que, em vez de adicionar um novo álbum ao BD. Coleção de álbuns, estamos encontrando a instância atual do álbum usando o BD. Entrada (álbum) e definir seu estado como modificado. Isso informa Entity Framework que estamos modificando um álbum existente em vez de criar um novo.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample12.cs)]

Podemos testar isso executando o aplicativo e navegando até/StoreManger/e clicando no link editar para um álbum.

![](mvc-music-store-part-5/_static/image9.png)

Isso exibe o formulário de edição mostrado pelo método editar HTTP-GET. Preencha alguns valores e clique no botão salvar.

![](mvc-music-store-part-5/_static/image10.png)

Isso posta o formulário, salva os valores e nos retorna à lista de álbuns, mostrando que os valores foram atualizados.

![](mvc-music-store-part-5/_static/image11.png)

### <a name="handling-deletion"></a>Tratamento da exclusão

A exclusão segue o mesmo padrão que editar e criar, usando uma ação do controlador para exibir o formulário de confirmação e outra ação do controlador para lidar com o envio do formulário.

A ação HTTP-obter controlador de exclusão é exatamente a mesma que nossa ação anterior do controlador de detalhes do Store Manager.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample13.cs)]

Exibimos um formulário fortemente tipado para um tipo de álbum, usando o modelo excluir conteúdo de exibição.

![](mvc-music-store-part-5/_static/image12.png)

O modelo de exclusão mostra todos os campos para o modelo, mas podemos simplificar isso um pouco. Altere o código de exibição em/Views/StoreManager/Delete.cshtml para o seguinte.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample14.cshtml)]

Isso exibe uma confirmação de exclusão simplificada.

![](mvc-music-store-part-5/_static/image13.png)

Clicar no botão excluir faz com que o formulário seja Postado de volta para o servidor, que executa a ação DeleteConfirmed.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample15.cs)]

Nossa ação do controlador HTTP-POST Delete executa as seguintes ações:

- 1. Carrega o álbum por ID
- 2. Exclui o álbum e salva as alterações
- 3. Redireciona para o índice, mostrando que o álbum foi removido da lista

Para testar isso, execute o aplicativo e navegue até/StoreManager. Selecione um álbum na lista e clique no link excluir.

![](mvc-music-store-part-5/_static/image14.png)

Isso exibe nossa tela de confirmação de exclusão.

![](mvc-music-store-part-5/_static/image15.png)

Clicar no botão excluir remove o álbum e nos retorna à página de índice do Store Manager, que mostra que o álbum foi excluído.

![](mvc-music-store-part-5/_static/image16.png)

### <a name="using-a-custom-html-helper-to-truncate-text"></a>Usando um auxiliar HTML personalizado para truncar o texto

Temos um possível problema com nossa página de índice do Store Manager. As propriedades título do nosso álbum e nome do artista podem ser longas o suficiente para que possam lançar nossa formatação de tabela. Vamos criar um auxiliar HTML personalizado para nos permitir truncar facilmente essas e outras propriedades em nossas exibições.

![](mvc-music-store-part-5/_static/image17.png)

A sintaxe @helper do Razor tornou muito fácil criar suas próprias funções auxiliares para uso em suas exibições. Abra a exibição/Views/StoreManager/Index.cshtml e adicione o código a seguir diretamente após a linha de @model.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample16.cshtml)]

Esse método auxiliar usa uma cadeia de caracteres e um comprimento máximo para permitir. Se o texto fornecido for menor que o comprimento especificado, o auxiliar o produzirá como está. Se for maior, ele truncará o texto e renderizará "..." para o restante.

Agora, podemos usar nosso auxiliar TRUNCATE para garantir que as propriedades título do álbum e nome do artista tenham menos de 25 caracteres. O código de exibição completo usando nosso novo auxiliar TRUNCATE aparece abaixo.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample17.cshtml)]

Agora, quando navegamos pela URL do/StoreManager/, os álbuns e os títulos são mantidos abaixo de nossos comprimentos máximos.

![](mvc-music-store-part-5/_static/image18.png)

Observação: isso mostra o caso simples de criar e usar um auxiliar em uma exibição. Para saber mais sobre como criar auxiliares que podem ser usados em todo o site, consulte minha postagem no blog: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)

> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-4.md)
> [Próximo](mvc-music-store-part-6.md)
